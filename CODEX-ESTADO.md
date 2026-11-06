# CODEX-ESTADO

> Ultima resposta processada: **06/11/2026 05:22** · publicado em **06/11/2026 05:26** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 06/11 05:22 · sessao c611628a56e6 · OTIMIZA-3 · censo · origem=Code

**BLOQUEIO 1: classe (e) — NÃO PERGUNTADO.** Nenhuma das três perguntas trouxe loader/import dinâmico ou leitura de símbolo por atributo de módulo carregado. Referência: anexos q1–q3 fornecidos; arquivo:linha na cópia [nao verificado].

| achado | classe | trecho pergunta | trecho resposta | trecho triagem |
|---|---|---|---|---|
| Loader de string deixa a faceta símbolo-de-topo imprimir zero falso. O caso por caminho existe na cópia: carregamento e chamada em `bin/gerar_estado.py:142` e `bin/gerar_estado.py:145`; resolução do atributo depende de `apelidos` em `bin/quem_chama.py:473`. | **(e) NÃO PERGUNTADO** — pelas perguntas fornecidas [nao verificado]. | q1/P1: “Existe forma de Python embutido em .sh, config ou string de .py que IMPORTA um modulo-alvo e que o novo `_rx_embutido` nao casa”. q1/P7: “Qualquer outro defeito que voce veja no diff”. q2/P6 e q3/P5: “Qualquer outro defeito no delta”. Nenhum desses trechos explicita o mecanismo de loader. [nao verificado] | r1/6: “há escapes reais da regex, mas não demonstrei uma faceta com zero falso causada por eles”. r2/6: “nenhum dos antigos achados 5, 6, 8 e 9 ganhou falso zero demonstrado com o delta”. Tratam dos escapes anteriores; nenhuma das três respostas relata o bloqueio de loader. [nao verificado] | v5/6: “regex embutida nao resolve `from .ci import *` dentro de heredoc nem `from pacote.sub import` contra o __init__ do pacote”; motivo escrito: “falso 0 real nao demonstrado”. **Esse encaminhamento para FILA trata de outro mecanismo**, não de um loader identificado pelo Codex e descartado. [nao verificado] |

**O pedido genérico de procurar zero falso não conta, sozinho, como tema PRESENTE nesta classificação.** Caso contrário, “qualquer outro defeito” tornaria impossível a classe (e). Aqui, P1 delimita Python embutido e `_rx_embutido`; P2 pergunta por “faceta de um modulo rebaixado”, pressupondo rebaixamento. Nenhum deles pergunta pelo vínculo criado por loader que não foi reconhecido. Referência: q1/P1–P2/P7 fornecidos [nao verificado].

A distinção tem fundamento no código: os vínculos de `apelidos` vêm de `ast.Import`/`ast.ImportFrom`; o carregamento real usa `spec_from_file_location` seguido de `mod.placar()`. (`bin/quem_chama.py:448`, `bin/quem_chama.py:454`, `bin/gerar_estado.py:142`, `bin/gerar_estado.py:145`)

## 06/11 05:22 · sessao 78075a70af27 · LASTRO-PARTE-2 · verificador · origem=Code

**D1. O `continue` não provoca rollback, mas pode confirmar efeitos do gate.** Quando `reconciliar()` retorna `False`, a nota e o incremento de `n` são pulados; a saída normal do `with` preserva eventuais escritas anteriores, inclusive a trilha `fechamento_recusado`. Portanto, “sem efeito colateral” seria incorreto. (`app/ponto/services/lastro.py:399`; `app/chamados/models.py:617`; `app/chamados/models.py:407`; `app/chamados/models.py:391`)

O `atomic(savepoint=False)` continua envolvendo a decisão e a gravação; agora participa da transação externa até a nota. O `select_for_update()` já era usado porque a leitura ocorria dentro daquele atomic; a mudança prolonga a retenção da trava até terminar o bloco novo. (`app/chamados/models.py:616`; `app/chamados/models.py:673`; `app/ponto/services/lastro.py:402`)

A premissa de que os signals **passaram agora** a rodar em transação está errada: `_gravar_estado()` já chamava `save()` dentro de outro atomic, ainda sob o atomic de `reconciliar()`. O delta amplia a unidade transacional para incluir a nota. (`app/chamados/models.py:274`; `app/chamados/models.py:279`; `app/chamados/models.py:616`; `app/ponto/services/lastro.py:399`)

Há efeitos relevantes nessa ampliação:

| Efeito | Consequência do bloco novo |
|---|---|
| `on_commit` do filho que acorda o pai; fechamento de becos por pergunta; retomada de regularizações por disputa | Quando acionados pela cascata, aguardam também a nota; rollback anterior ao commit descarta os callbacks. (`app/chamados/signals.py:314`; `app/chamados/signals.py:202`; `app/chamados/signals.py:121`; `app/ponto/services/lastro.py:402`) |
| Lavra da célula e `EventoCelula` | São escritas síncronas pelo ORM, na transação compartilhada; uma falha posterior da nota também pode desfazê-las. (`app/ponto/services/cartorio.py:1802`; `app/ponto/services/cartorio.py:1822`; `app/ponto/portas/celula.py:681`; `app/ponto/portas/celula.py:673`) |
| Push da reemissão | **Escapa ao rollback:** o cartório pode chamar o emissor, que envia push síncrono à supervisão antes da nota. Se ela falhar, as escritas podem voltar depois de o aviso já ter saído; o retry pode reenviá-lo. (`app/ponto/services/cartorio.py:1778`; `app/ponto/management/commands/detectar_ausencias.py:778`; `app/chamados/push_utils.py:40`; `app/ponto/services/lastro.py:402`) |

No caminho de persistência rastreado, as operações usam o ORM sem conexão de banco separada declarada; o envio de push é uma chamada externa imediata. Configuração e entrega efetiva em produção: **[nao verificado]**. (`app/ponto/portas/celula.py:673`; `app/chamados/services/nota.py:103`; `app/chamados/push_utils.py:40`)

**D2. Exceção do cartório não passou genericamente a desfazer o fecho.** O receiver captura `Exception` por `engolir()`, que registra e não relança. Uma exceção Python capturada ali não chega automaticamente à lista `falhas` do lastro. (`app/chamados/signals.py:172`; `app/chamados/signals.py:180`; `app/core/observ.py:85`)

`engolir()` sozinho não cria savepoint; entretanto, o cartório já executava dentro dos atomics antigos. Logo, eventual transação comprometida por erro SQL engolido não deve ser atribuída como novidade deste delta. A gravação célula/evento tem seu próprio savepoint. Comportamento efetivamente observado desse erro de banco: **[nao verificado]**. (`app/chamados/signals.py:172`; `app/chamados/models.py:274`; `app/chamados/models.py:616`; `app/ponto/portas/celula.py:681`)

A mudança intencional é a **falha da nota posterior ao gate**: agora ela desfaz fecho e escritas transacionais da cascata; o handler registra a falha, e o consumidor incrementa as tentativas dos eventos daquele lote do colaborador. O teste A6 descreve uma falha e posterior recuperação, mas sua execução nesta verificação é **[nao verificado]**. (`app/ponto/services/lastro.py:399`; `app/ponto/services/lastro.py:404`; `app/chamados/reconciliador.py:1377`; `app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:163`)

Quem escreve e quem lê:

| Objeto | Escritor e leitor |
|---|---|
| Estado do chamado | `reconciliar()` → `_gravar_estado()`; o lastro seleciona os chamados pela fila de cobrança. (`app/chamados/models.py:620`; `app/ponto/services/lastro.py:387`; `app/chamados/services/painel.py:187`) |
| Nota | `anexar_ocorrencia()` cria `FollowupChamado`, agora antes do commit do bloco externo. (`app/chamados/services/nota.py:103`; `app/ponto/services/lastro.py:402`) |
| Tentativas/erro/processamento | O consumidor atualiza o lote após o trabalho; `eventos_pendentes()` exclui os esgotados e `eventos_mortos()` os conta. (`app/chamados/reconciliador.py:1377`; `app/chamados/reconciliador.py:1238`; `app/chamados/reconciliador.py:1244`) |

**Pode ficar parado indefinidamente, sob condições.** Depois de cinco falhas, aquele evento sai do consumo automático. Outro evento do mesmo colaborador permite nova passagem pelo lastro, mas, sem ele, não existe retry daquele evento morto nesse laço; o comando antigo agora apenas alarma. Isso não prova um chamado preso em produção. (`app/chamados/reconciliador.py:1198`; `app/chamados/reconciliador.py:1238`; `app/chamados/reconciliador.py:1371`; `app/ponto/management/commands/fechar_cobranca_com_lastro.py:77`)

Quem vê o morto é o comando consumidor, que imprime a contagem, e `reconciliar_chamados --alarme`, que soma mortos ao alarme e sai com código 2. O registro declara esses comandos a cada minuto e a cada quinze minutos, respectivamente; instalação real, destinatário humano e recebimento do alarme: **[nao verificado]**. (`app/chamados/management/commands/consumir_eventos_celula.py:37`; `app/chamados/management/commands/reconciliar_chamados.py:118`; `app/config/crons.py:394`)

Também não contaria com `--semear` como recuperação geral de `batida_ausente`: seu primeiro universo contém somente os módulos de turno enumerados; o outro ramo trata perguntas órfãs. (`app/chamados/reconciliador.py:1278`; `app/chamados/reconciliador.py:1193`; `app/chamados/reconciliador.py:336`; `app/chamados/reconciliador.py:1291`)

**D3. Os descartes têm sustentação funcional, com uma correção na justificativa de cobertura de V10.**

| Item | Conferência |
|---|---|
| **V4** | Sustentado para a cascata descrita: o save aciona o cartório, que pode reemitir pelos marcos apagados; estes impedem novo fechamento pelo lastro. A equivalência histórica com o comando antigo: **[nao verificado]**. (`app/chamados/signals.py:180`; `app/ponto/services/cartorio.py:1768`; `app/ponto/services/cartorio.py:1778`; `app/ponto/services/lastro.py:179`) |
| **V5** | Coerente como descarte por escopo, não como ausência de lacuna: a regeneração muda DNA sem substituir a ata e `recalculo=False` pula o julgamento. Só ata/veredito acionam a outbox nessa porta. O signal de isenção agenda silêncio dos chamados. (`app/ponto/portas/celula.py:270`; `app/ponto/portas/celula.py:390`; `app/ponto/portas/celula.py:663`; `app/chamados/signals.py:224`) |
| **V6** | Sustentado: retidos saem em `(a2)` e entram no TOTAL do placar. O aval está documentado na cópia. (`app/ponto/management/commands/fechar_cobranca_com_lastro.py:61`; `bin/placar_code.sh:121`; `bin/placar_code.sh:549`; `app/docs/PROMPTS.md:1859`) |
| **V7** | O defeito do produtor existe: `_reais` admite células sintéticas de isenção; o ramo `j is None` acende por proximidade sem copiar o marcador. Sem marcador, a nota pode dizer `EXISTE`. Portanto, descarte por origem/escopo não elimina o defeito. Medição de sombra e número publicado: **[nao verificado]**. (`app/escala/utils.py:858`; `app/ponto/services/cartorio.py:593`; `app/ponto/services/cartorio.py:638`; `app/ponto/services/cartorio.py:645`; `app/ponto/services/lastro.py:340`; `app/ponto/services/lastro.py:356`) |
| **V10** | Correto que o comando virou vigia e o escritor exige lâmpada acesa. **Exagerada a alegação de cobertura:** A5 mocka `reconciliar=False`, sem testar o gate real; C5 testa o vigia, sem executar o consumidor. A3/A4 exercem apagado/metade. “M1–M5 morderam”: **[nao verificado]**. (`app/ponto/management/commands/fechar_cobranca_com_lastro.py:53`; `app/ponto/services/lastro.py:394`; `app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:144`; `app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:258`; `app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:122`; `app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:130`) |

**D4. Sim: o `.mmd` inteiro conferiu com a recomposição em memória do gerador.** O gerador compõe os blocos e deriva as contagens do registro: **40 vigias, 12 prazos, 1 juiz**, `disparar_perguntas_competencia`. `fechar_cobranca_com_lastro` está declarado como vigia e aparece no bloco correspondente. (`app/core/management/commands/gerar_diagrama.py:912`; `app/core/management/commands/gerar_diagrama.py:242`; `app/docs/ARQUITETURA.mmd:207`; `app/docs/ARQUITETURA.mmd:217`; `app/config/crons.py:2155`; `app/docs/ARQUITETURA.mmd:391`)

Execução da suíte e resultados observados em produção: **[nao verificado]**. [nao verificado]

## 06/11 05:21 · sessao 6e30e659ccea · - · - · origem=Ronald (tmux)

D1/D2 — evidências consolidadas:

- `continue` sai normalmente do `with`; não pede rollback, não anexa nota e não incrementa `n`. Porém **não significa ausência de efeitos**: `reconciliar(registrar=True)` pode gravar `fechamento_recusado` antes de retornar `False`; essa auditoria permanece na saída normal. (`app/ponto/services/lastro.py:399-403`; `app/chamados/models.py:617-619`, `:730-734`, `:380-391`, `:407-414`.)
- O `atomic(savepoint=False)` e o `select_for_update` já existiam e continuam operando: `reconciliar` abre atomic e `_recarregar_estado_se_mudou` trava quando está em atomic. O novo envoltório estende a transação/trava até a nota. (`app/chamados/models.py:616-620`, `:638`, `:671-675`; `app/ponto/services/lastro.py:399-402`.)
- **Os signals já rodavam dentro de transação antes do delta**: `_gravar_estado` tem seu próprio `atomic` e executa `save` dentro dele, chamado pelo atomic de `reconciliar`. Portanto não procede a premissa de que uma exceção do cartório passou agora, genericamente, a poder desfazer o fecho. (`app/chamados/models.py:274-279`, `:616-620`.)
- O receiver do cartório é síncrono e captura `Exception` via `engolir`; esse contexto somente registra e suprime, não cria savepoint. Erro Python ali não vira automaticamente falha do consumidor. SQL que deixe a transação abortada fora de savepoints é risco preexistente; manifestação específica de `needs_rollback`, sem execução/driver aberto: [nao verificado]. (`app/chamados/signals.py:171-180`; `app/core/observ.py:85-91`.)
- Há isolamento próprio em efeitos que prometem best-effort: resolução de perguntas, auditoria e escrita de célula/evento usam atomics internos. (`app/chamados/signals.py:88-94`; `app/core/observ.py:133-138`; `app/ponto/portas/celula.py:681-684`.)

Callbacks que passam a aguardar também a nota e são descartados se ela provocar rollback:

- Chamado filho morto → reconciliar pai: `on_commit`, condicionado à saída de VIVOS e existência de pai vivo. (`app/chamados/signals.py:295-314`.)
- Pergunta salva → fechar beco: `on_commit`. A resolução síncrona pode carimbar perguntas. (`app/chamados/signals.py:192-202`; `app/chamados/services/disputa_emissao.py:1603-1604`.)
- Disputa fechada → retomar regularizações: `on_commit`; a resolução síncrona pode fechar disputa. (`app/chamados/signals.py:98-121`; `app/chamados/services/disputa_emissao.py:1695-1696`.)
- Isentos: o receiver direto retorna para chamado encerrado; pode agendar na reabertura em cascata. (`app/chamados/signals.py:228-251`.)

Escrita de evento na cascata:

- Cartório lavra e carimba → `carimbar` → `salvar_e_emitir` → `EventoCelula.objects.create`; a emissão é síncrona, no mesmo banco/transação, não `on_commit`. Pode ser desfeita pela falha posterior da nota. Não encontrei conexão própria nos arquivos examinados; o caminho demonstrado usa ORM comum. (`app/ponto/services/cartorio.py:1800-1823`; `app/ponto/portas/celula.py:829-849`, `:676-684`, `:666-673`.)

**Risco novo relevante: push externo antes do commit.** [nao verificado]

- Cartório pode emitir/reabrir cobrança pelos marcos restantes e chamar `_abrir_chamado`. Esse emissor chama imediatamente `push_supervisao` para perfil não flexível. O utilitário efetivamente chama `webpush` durante a execução. (`app/ponto/services/cartorio.py:1740-1742`, `:1777-1780`; `app/ponto/management/commands/detectar_ausencias.py:774-783`; `app/chamados/push_utils.py:89-111`, `:38-48`.)
- O emissor também pode criar o fio de disputa; a porta pode enviar WebPush e FCM imediatamente, com `messaging.send(msg)` síncrono. A supressão de push usada pela fábrica da célula não envolve genericamente esse emissor. (`app/ponto/management/commands/detectar_ausencias.py:790-801`; `app/chamados/services/disputa_emissao.py:1123-1124`, `:1535-1543`; `app/chamados/fcm_utils.py:85`, `:101-106`; `app/chamados/services/fabrica_da_celula.py:175`.)
- Inferência: se a nota falhar **depois** desses envios, o novo atomic desfaz fecho/reabertura/lavra/eventos, mas não recolhe notificações já enviadas; outra tentativa pode repetir os avisos. Antes, a nota falhava depois do commit do gate. Ocorrência em produção e destinatários configurados: [nao verificado]. (`app/ponto/services/lastro.py:399-407`; caminhos de envio acima.)

Fila e morte:

- Falha propagada da nota é capturada pelo fechador, que acrescenta `falhas`; consumidor incrementa tentativas/erro dos eventos originalmente carregados e conserva `processado_em=NULL`. (`app/ponto/services/lastro.py:404-407`; `app/chamados/reconciliador.py:1376-1385`.)
- Após cinco falhas, o evento sai da seleção viva; pode ficar chamado aberto sem retry automático desse evento. Evento novo do mesmo colaborador pode provocar nova passagem, pois o fechador percorre a fila inteira do colaborador. (`app/chamados/reconciliador.py:1198`, `:1235-1244`, `:1336-1371`; `app/ponto/services/lastro.py:387-388`.)
- `--semear` não é recuperação genérica de `batida_ausente`: semeia chamados dos módulos de turno e perguntas órfãs, não toda cobrança viva. (`app/chamados/reconciliador.py:1190-1193`, `:1278-1299`; conforme definição de `MODULOS_DE_TURNO` conferida pelo agente principal.)
- Quem vê mortos: consumidor imprime a contagem; `reconciliar_chamados --alarme` inclui mortos na soma e sai 2. Config declara consumidor a cada minuto e vigia a cada quinze minutos. Destinatário humano efetivo: [nao verificado]. (`app/chamados/management/commands/consumir_eventos_celula.py:36-37`; `app/chamados/management/commands/reconciliar_chamados.py:100-124`; `app/config/crons.py:394-395`.)
- Vigia de lastro também considera morto como ausência de evento vivo e pode listar o chamado elegível. (`app/ponto/services/lastro.py:420-423`; `app/ponto/management/commands/fechar_cobranca_com_lastro.py:77-83`.)

## 06/11 05:19 · sessao ba4b19cc5a2f · - · - · origem=Ronald (tmux)

D3, leitura estática:

- **V4: motivo sustentado.** O save do estado chama o cartório (`app/chamados/models.py:279`, `app/chamados/signals.py:180`). O cartório emite cobrança parcial pelos marcos apagados (`app/ponto/services/cartorio.py:1768`, `app/ponto/services/cartorio.py:1778`); estes vêm de `marcos_apagados` (`app/ponto/services/cartorio.py:1878`) e substituem o contexto da cobrança (`app/ponto/management/commands/detectar_ausencias.py:558`). Um marco apagado impede novo fechamento (`app/ponto/services/lastro.py:179`). O comentário também documenta reabertura síncrona pelo receiver (`app/chamados/models.py:269`). A equivalência histórica com o comando antigo, sem abrir sua versão anterior, fica **[nao verificado]**.

- **V5: descarte coerente como limite de escopo; a lacuna existe.** O regenerador muda DNA e preserva `ata` no `update_fields` (`app/ponto/portas/celula.py:265`, `app/ponto/portas/celula.py:270`); `recalculo=False` pula o julgamento (`app/ponto/portas/celula.py:390`). Ainda agenda rejunção e reavaliação de feriado (`app/ponto/portas/celula.py:461`, `app/ponto/portas/celula.py:473`). Portanto é insumo novo sem relavra, e não ata nova sem evento: somente ata/veredito acordam a outbox nessa porta (`app/ponto/portas/celula.py:663`, `app/ponto/portas/celula.py:680`). Cadastro de isenção altera o insumo (`app/colaboradores/services/colaborador.py:34`, `app/colaboradores/services/colaborador.py:61`); o signal aberto agenda silêncio dos chamados, sem relavrar a célula (`app/chamados/signals.py:213`, `app/chamados/signals.py:224`).

- **V6: motivo sustentado.** O gate separa `com_lastro` e retidos (`app/ponto/services/lastro.py:318`); o comando imprime os retidos em `(a2)` (`app/ponto/management/commands/fechar_cobranca_com_lastro.py:61`); o placar raspa esse número e soma ao TOTAL (`bin/placar_code.sh:121`, `bin/placar_code.sh:549`). O aval consta da cópia (`app/docs/PROMPTS.md:1859`).

- **V7: defeito do produtor confirmado; descarte não significa inexistência do problema.** A isenção fabrica a hora planejada com `missing=False` (`app/escala/utils.py:858`). `_reais` aceita essas células sem excluir isenção (`app/ponto/services/cartorio.py:593`); o ramo `j is None` acende por proximidade e sai sem carimbar (`app/ponto/services/cartorio.py:632`, `app/ponto/services/cartorio.py:638`, `app/ponto/services/cartorio.py:645`), enquanto os demais ramos carimbam (`app/ponto/services/cartorio.py:680`, `app/ponto/services/cartorio.py:691`). Sem marcador, `como_acendeu` devolve batida e a nota pode dizer EXISTE (`app/ponto/services/lastro.py:338`, `app/ponto/services/lastro.py:340`, `app/ponto/services/lastro.py:356`). Os outros leitores também dependem do marcador (`app/escala/services/leitor_celula.py:139`, `app/ponto/supra_juiz.py:90`, `app/ponto/status_dia.py:186`). A ambiguidade das atas antigas é declarada (`app/ponto/services/cartorio.py:547`). Medição da sombra e número publicado na fila: **[nao verificado]**.

- **V10: conclusão funcional coerente, justificativa de cobertura exagerada.** O comando atual somente julga e alarma (`app/ponto/management/commands/fechar_cobranca_com_lastro.py:53`, `app/ponto/management/commands/fechar_cobranca_com_lastro.py:77`). O escritor exige lâmpada acesa (`app/ponto/services/lastro.py:394`). A3/A4 exercem apagado e metade resolvida (`app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:122`, `app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:130`); **A5 mocka** `reconciliar=False`, não testa o gate real (`app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:144`); **C5 testa o vigia**, não executa o consumidor (`app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:255`, `app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:258`). “M1–M5 morderam”: **[nao verificado]**.

## 06/11 05:19 · sessao 2414262bf15d · - · - · origem=Ronald (tmux)

D4: **sim, bate integralmente**. Comparei em memória o `.mmd` commitado com as oito funções e o cabeçalho que `gerar()` usa: igualdade byte a byte, 427 linhas. A composição está em `app/core/management/commands/gerar_diagrama.py:911-919`; o arquivo termina em `app/docs/ARQUITETURA.mmd:427`.

- **40 vigias, 12 prazos e 1 juiz**: o tabuleiro deriva os conjuntos por papel sobre `todas_as_entradas()` (`app/core/management/commands/gerar_diagrama.py:233-256`), que reúne `CRONS` e `ENCADEADOS` (`app/config/crons.py:1752-1763`). Os números estão corretos em `app/docs/ARQUITETURA.mmd:207-209`.
- O único juiz derivado é `disparar_perguntas_competencia`: registro em `app/config/crons.py:2009-2013`, derivação em `app/config/crons.py:2313-2322`, desenho em `app/docs/ARQUITETURA.mmd:217`.
- `fechar_cobranca_com_lastro` está declarado como **vigia** (`app/config/crons.py:2155-2157`), com agenda `40 * * * *`, argumento `--alarme`, log `lastro.log`, estágio `continuo` e dependências vazias (`app/config/crons.py:731-732`). O novo nó aparece em `app/docs/ARQUITETURA.mmd:391`.
- O bloco `(h)` continua derivando somente de `CRONS`, enquanto o contador do tabuleiro inclui também os encadeados; isso é comportamento do gerador, não divergência introduzida pelo delta (`app/core/management/commands/gerar_diagrama.py:396-401`; `app/core/management/commands/gerar_diagrama.py:242-243`).

Não executei a suíte: ela inclui teste que escreve no `MAPA.md`, incompatível com a restrição de leitura (`app/core/tests/test_selo_diagrama_do_codigo.py:49-60`). A comparação em memória executou as funções originais do gerador, removendo apenas o import de Django e a classe de comando do módulo carregado; execução da suíte e ambiente Django: [nao verificado].

## 06/11 04:45 · sessao d58c4d563f58 · OTIMIZA-3 · verificador · origem=Code

**Parecer: bloqueia pelo achado 1. Os bloqueios anteriores estão corrigidos, mas permanece outro falso zero com registros empilhados** (`bin/quem_chama.py:256`, `bin/quem_chama.py:284`, `bin/quem_chama.py:353`, `bin/quem_chama.py:714`).

1. **BLOQUEIA — P1/P5: remove todos os decoradores de registro, mas guarda somente o primeiro.**

   `_registro()` retorna no primeiro decorador reconhecido; a limpeza nova retira todos os registros reconhecidos de `decor` (`bin/quem_chama.py:242`, `bin/quem_chama.py:256`, `bin/quem_chama.py:282`).

   Contraexemplo, alterando somente o corpo de `f` em uma biblioteca `m_tags.py`:

   ```python
   register = template.Library()

   @register.filter(name="a")
   @register.filter(name="b")
   def f(x):
       return x
   ```

   Com `{% load m_tags %}` e `{{ x|b }}`, o fluxo conserva `decor=[]` e `registros=[('filtro', 'a')]`. A busca pelo nome `b` desaparece, e a faceta imprime **`simbolo f [filtro/tag]: 0 chamador(es)`**; os decoradores também não produzem leitura própria de `f` (`bin/quem_chama.py:284`, `bin/quem_chama.py:357`, `bin/quem_chama.py:521`, `bin/quem_chama.py:611`, `bin/quem_chama.py:771`).

   Esse falso zero já era possível pela precedência antiga mostrada no anexo; não é regressão exclusiva desta volta. Contudo, a limpeza nova impede que o segundo registro funcione como motivo opaco depois da inversão. O contrato anunciado inclui filtros registrados com nomes constantes (`bin/quem_chama.py:61`, `bin/quem_chama.py:256`, `bin/quem_chama.py:284`). Ocorrência desse empilhamento na produção: **[nao verificado]**.

2. **FILA — P1: `@stringfilter` rebaixa um filtro conhecido conservadoramente.**

   Contraexemplo: `@register.filter` empilhado com `@stringfilter`. O registro sai de `decor`, mas `stringfilter` permanece e não integra `DECORADORES_TRANSPARENTES`; o resultado passa a ser `decorado @stringfilter: o decorador pode registrar` (`bin/quem_chama.py:65`, `bin/quem_chama.py:69`, `bin/quem_chama.py:284`, `bin/quem_chama.py:353`).

   É perda de precisão, sem falso zero: o tipo sai da lista de contagem e os usos encontrados pelos registros continuam sendo emitidos como avulsos (`bin/quem_chama.py:611`, `bin/quem_chama.py:776`). Ocorrência real de `@stringfilter` nesta cópia: **[nao verificado]**.

Nos demais casos de **P1**, a proteção permanece:

| Caso concreto | Resultado |
|---|---|
| `@outra.filter`, com `outra` ausente de `_libs_de_template` | Não reconhece registro, conserva o decorador e classifica como opaco (`bin/quem_chama.py:245`, `bin/quem_chama.py:282`, `bin/quem_chama.py:353`). |
| `simple_tag(takes_context=True)` | Conserva o nome da função e classifica como `filtro/tag`; exemplo existente: `next_para` (`app/core/templatetags/nav_extras.py:49`, `bin/quem_chama.py:247`, `bin/quem_chama.py:253`). |
| `inclusion_tag(..., takes_context=True)` | Mesmo resultado; exemplo existente: `voltar` (`app/core/templatetags/nav_extras.py:36`, `bin/quem_chama.py:247`). |
| `register.filter('x', f)` fora de decorador | `f` fica `simbolo-de-topo`, mas a chamada conta como leitura própria de `f`, evitando zero nesse exemplo (`bin/quem_chama.py:360`, `bin/quem_chama.py:521`, `bin/quem_chama.py:529`). |
| `C.f` decorado dentro de classe | Não recebe registro; prevalece atributo de classe, não varrido (`bin/quem_chama.py:280`, `bin/quem_chama.py:345`). |

**P2:** não encontrei leitor operacional dependente da semântica antiga de `decor`; classificação e varredura recebem os campos depois da soma (`bin/quem_chama.py:712`, `bin/quem_chama.py:716`, `bin/quem_chama.py:737`).

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `decor` | Coleta, limpeza e gravação: `bin/quem_chama.py:281`, `bin/quem_chama.py:283`, `bin/quem_chama.py:285`; inicialização/união: `bin/quem_chama.py:710`, `bin/quem_chama.py:712` | União/deduplicação: `bin/quem_chama.py:712`; classificação: `bin/quem_chama.py:353` |
| `registro` singular | Extração: `bin/quem_chama.py:247`, `bin/quem_chama.py:251`, `bin/quem_chama.py:255`; gravação: `bin/quem_chama.py:285` | Condição da limpeza: `bin/quem_chama.py:282`; exclusão da cópia e transferência: `bin/quem_chama.py:709`, `bin/quem_chama.py:713`, `bin/quem_chama.py:714` |
| `registros` plural | Inicialização/acréscimo: `bin/quem_chama.py:710`, `bin/quem_chama.py:714` | Deduplicação: `bin/quem_chama.py:713`; classificação: `bin/quem_chama.py:356`, `bin/quem_chama.py:357`; templates: `bin/quem_chama.py:611`, `bin/quem_chama.py:612` |

**P3:** as asserções novas distinguem os mutantes apontados anteriormente; não identifiquei verde acidental por texto de outra faceta nos casos abaixo (`bin/tests/test_quem_chama.sh:197`, `bin/tests/test_quem_chama.sh:202`).

| Asserção nova | Por que distingue o defeito |
|---|---|
| Lista nominal sob arquivo | Exige texto específico do cabeçalho EMBUTIDO dentro da faceta arquivo (`bin/tests/test_quem_chama.sh:329`, `bin/quem_chama.py:766`). |
| Autoimport sob arquivo | Exige cabeçalho rebaixado **e** sítio `selfmod.py:5` nessa faceta (`bin/tests/test_quem_chama.sh:344`, `bin/tests/test_quem_chama.sh:345`). |
| Registro versus opaco | Exige `decorado @receiver`; `filtro/tag` não satisfaz o positivo (`bin/tests/test_quem_chama.sh:347`, `bin/tests/test_quem_chama.sh:348`). |
| Opaco removido | O `receiver` existe somente na base; conservar só a ponta perde o positivo (`bin/tests/test_quem_chama.sh:150`, `bin/tests/test_quem_chama.sh:183`, `bin/tests/test_quem_chama.sh:350`). |
| Nome antigo do filtro | O template específico usa `velho_f`; conservar somente o novo perde o achado (`bin/tests/test_quem_chama.sh:153`, `bin/tests/test_quem_chama.sh:352`). |
| Include entre comentários | A remoção gulosa apaga o include exigido na linha 2 (`bin/tests/test_quem_chama.sh:151`, `bin/tests/test_quem_chama.sh:354`). |
| Argumento de outra tag | Generalizar `filter` para qualquer tag produz exatamente o falso uso proibido (`bin/tests/test_quem_chama.sh:152`, `bin/tests/test_quem_chama.sh:356`). |

`_so_na_fac` agora detecta duplicação de linhas. Os fixtures atuais escolhem achados exclusivos; a troca de nomes entre dois filtros ainda poderia gerar duplicação legítima entre facetas, mas esse cenário não está nos fixtures atuais (`bin/tests/test_quem_chama.sh:205`, `bin/tests/test_quem_chama.sh:143`, `bin/tests/test_quem_chama.sh:180`, `bin/quem_chama.py:714`, `bin/quem_chama.py:620`).

**P4:** os bloqueios anteriores estão fechados: decorador acrescentado ou removido permanece na união; nomes velho e novo são buscados; autoimport rebaixa arquivo e símbolos; opaco prevalece sobre registro (`bin/quem_chama.py:712`, `bin/quem_chama.py:714`, `bin/quem_chama.py:611`, `bin/quem_chama.py:743`, `bin/quem_chama.py:757`, `bin/quem_chama.py:353`).

Execução integral do selo, mutantes integrais e ranges históricos: **[nao verificado]**.

## 06/11 04:44 · sessao 5d2f2b28a4fe · - · - · origem=Ronald (tmux)

P1/P5, conferidos por inspeção e execução em memória:

1. **BLOQUEIA — múltiplos registros são apagados de `decor`, mas somente um entra em `registros`.** `_registro` retorna no primeiro decorador reconhecido (`bin/quem_chama.py:256`), enquanto o delta remove todos os decoradores de registro (`bin/quem_chama.py:282`, `bin/quem_chama.py:284`).

   Contraexemplo:

   ```python
   register = template.Library()

   @register.filter(name="a")
   @register.filter(name="b")
   def f(x):
       return x
   ```

   Um template `{% load m_tags %}\n{{ x|b }}` não é associado ao símbolo: o metadado contém apenas `('filtro', 'a')`, e o varredor busca exclusivamente `info['registros']` (`bin/quem_chama.py:611`, `bin/quem_chama.py:614`). A reprodução em memória emitiu `simbolo f [filtro/tag]: 0 chamador(es)`; a leitura própria também ficou vazia (`bin/quem_chama.py:492`, `bin/quem_chama.py:529`, `bin/quem_chama.py:770`).

   O falso zero já era possível na v6 anexada, pela precedência antiga; a remoção indiscriminada nova conserva esse defeito mesmo depois da inversão de precedência (`bin/quem_chama.py:353`, `bin/quem_chama.py:356`). Ocorrência concreta desse empilhamento na cópia: **[nao verificado]**.

2. **FILA — `@stringfilter` passa a rebaixar filtro conhecido.** Com `@register.filter` empilhado com `@stringfilter`, o primeiro sai de `decor`, mas `stringfilter` permanece e não integra `DECORADORES_TRANSPARENTES` (`bin/quem_chama.py:65`, `bin/quem_chama.py:69`, `bin/quem_chama.py:284`). A reprodução devolveu `decorado @stringfilter: o decorador pode registrar`, antes de considerar o registro conhecido (`bin/quem_chama.py:353`, `bin/quem_chama.py:356`). É perda conservadora de precisão; não produz zero contado (`bin/quem_chama.py:774`). Ocorrência real desse decorador na cópia: **[nao verificado]**.

3. **FILA — os demais casos pedidos conservam a proteção.**

   - Biblioteca desconhecida de `_libs_de_template`: `_registro` não reconhece, o decorador permanece e vira motivo opaco (`bin/quem_chama.py:245`, `bin/quem_chama.py:282`, `bin/quem_chama.py:353`).
   - `simple_tag(takes_context=True)`: conserva o nome da função; a lógica especial de argumentos posicionais só vale para `filter` e `tag` (`bin/quem_chama.py:247`, `bin/quem_chama.py:253`). A reprodução sobre `nav_extras.next_para` devolveu `filtro/tag`; o caso existe em `app/core/templatetags/nav_extras.py:49`.
   - `inclusion_tag(..., takes_context=True)`: igualmente preservado; caso real `voltar` em `app/core/templatetags/nav_extras.py:36`.
   - `register.filter("x", f)` fora de decorador: `f` fica `simbolo-de-topo`, mas a chamada gera leitura própria de `f`, evitando zero no exemplo (`bin/quem_chama.py:360`, `bin/quem_chama.py:521`, `bin/quem_chama.py:529`).
   - Membro de classe: não recebe registro e prevalece o tipo não varrido de atributo de classe (`bin/quem_chama.py:280`, `bin/quem_chama.py:345`).

## 06/11 04:43 · sessao 16c879ef65ef · - · - · origem=Ronald (tmux)

P2: fluxo consistente; nenhum leitor encontrado exige o decorador de registro dentro de `decor` (`bin/quem_chama.py:280`, `bin/quem_chama.py:353`, `bin/quem_chama.py:712`).

| Campo | Escritores | Leitores |
|---|---|---|
| `decor` cru | Coleta e filtragem: `bin/quem_chama.py:281`, `bin/quem_chama.py:283`; gravação: `bin/quem_chama.py:285` | União: `bin/quem_chama.py:712` |
| `decor` somado | Inicialização: `bin/quem_chama.py:710`; união: `bin/quem_chama.py:712` | Deduplicação: `bin/quem_chama.py:712`; classificação: `bin/quem_chama.py:353` |
| `registro` singular | `_registro`: `bin/quem_chama.py:247`, `bin/quem_chama.py:251`, `bin/quem_chama.py:255`; dict cru: `bin/quem_chama.py:285` | Condição da filtragem: `bin/quem_chama.py:282`; exclusão da cópia: `bin/quem_chama.py:709`; união: `bin/quem_chama.py:713`, `bin/quem_chama.py:714` |
| `registros` plural | Inicialização: `bin/quem_chama.py:710`; acréscimo: `bin/quem_chama.py:714` | Deduplicação: `bin/quem_chama.py:713`; classificação: `bin/quem_chama.py:356`, `bin/quem_chama.py:357`; usos em template: `bin/quem_chama.py:611`, `bin/quem_chama.py:612` |

A classificação ocorre depois da soma e a varredura de templates ocorre depois da construção dos alvos (`bin/quem_chama.py:715`, `bin/quem_chama.py:716`, `bin/quem_chama.py:721`, `bin/quem_chama.py:737`). A busca textual em código de `app/` e `bin/` não encontrou consumidor adicional dessas funções; consumidor fora desse universo: **[nao verificado]**.

P4: os bloqueios identificados nas voltas anteriores estão fechados pelo fluxo atual:

- Decorador acrescentado na ponta: a união conserva `receiver`; o tipo fica “decorado”, sem contagem (`bin/quem_chama.py:712`, `bin/quem_chama.py:353`, `bin/quem_chama.py:355`).
- Registro renomeado: os nomes velho e novo ficam em `registros` e ambos geram usos em template (`bin/quem_chama.py:713`, `bin/quem_chama.py:714`, `bin/quem_chama.py:611`, `bin/quem_chama.py:620`).
- Autoimport em string executada: é processado antes da guarda do próprio arquivo; arquivo e símbolo recebem o sítio e são rebaixados (`bin/quem_chama.py:743`, `bin/quem_chama.py:746`, `bin/quem_chama.py:757`, `bin/quem_chama.py:760`).
- Registro na base e opaco na ponta: o registro reconhecido sai de `decor`, mas `receiver` permanece; opaco vence o registro (`bin/quem_chama.py:280`, `bin/quem_chama.py:284`, `bin/quem_chama.py:353`, `bin/quem_chama.py:356`).

Esses três primeiros fluxos também foram exercitados com `quem_chama()` e revisões substituídas por dicionários em memória; os resultados corresponderam aos ramos citados (`bin/quem_chama.py:705`, `bin/quem_chama.py:716`, `bin/quem_chama.py:743`, `bin/quem_chama.py:760`). Execução de ranges históricos e do selo integral: **[nao verificado]**.

Não identifiquei defeito adicional no delta relativo aos campos ou ao fechamento dos bloqueios; contraexemplo adicional: **[nao verificado]**.

## 06/11 04:43 · sessao 9b3c092840e4 · - · - · origem=Ronald (tmux)

P3: **nenhum defeito demonstrado nas asserções novas ou falha espúria nos fixtures atuais** (`bin/tests/test_quem_chama.sh:149`, `bin/tests/test_quem_chama.sh:205`, `bin/tests/test_quem_chama.sh:329`, `bin/tests/test_quem_chama.sh:344`).

| Asserção nova | Conferência |
|---|---|
| Lista nominal sob arquivo, linha 329 | Exige texto do cabeçalho EMBUTIDO dentro da faceta arquivo; o achado comum não contém esse texto (`bin/quem_chama.py:766`, `bin/quem_chama.py:778`). |
| Autoimport sob arquivo, linhas 344–345 | Exige tanto cabeçalho rebaixado quanto sítio específico `selfmod.py:5`; omitir o arquivo reprova ambas (`bin/tests/test_quem_chama.sh:181`, `bin/quem_chama.py:743`, `bin/quem_chama.py:757`). |
| Registro versus opaco, linhas 347–348 | Fixture conserva registro da base e acrescenta receiver na ponta; tipo `filtro/tag` não satisfaz o positivo (`bin/tests/test_quem_chama.sh:149`, `bin/tests/test_quem_chama.sh:182`, `bin/quem_chama.py:351`). |
| Opaco removido, linha 350 | O receiver existe somente na base; conservar apenas decoradores da ponta perde o texto exigido (`bin/tests/test_quem_chama.sh:150`, `bin/tests/test_quem_chama.sh:183`). |
| Nome antigo do filtro, linha 352 | O template específico usa somente `velho_f`; manter somente registro novo perde o achado exigido (`bin/tests/test_quem_chama.sh:153`, `bin/quem_chama.py:611`). |
| Include entre comentários, linha 354 | Exercício em memória: atual emite include na linha 2; mutante guloso `.*` não emite (`bin/tests/test_quem_chama.sh:151`, `bin/quem_chama.py:577`). |
| Argumento de outra tag, linha 356 | Exercício em memória: atual emite load e tag `tg`; mutante que generaliza `filter` para qualquer tag acrescenta exatamente o uso falso proibido (`bin/tests/test_quem_chama.sh:152`, `bin/quem_chama.py:614`). |

A exigência `tot=1, sob=1` detecta duplicação de linhas; os usos atuais do helper escolhem achados únicos nos respectivos fixtures (`bin/tests/test_quem_chama.sh:205`, `bin/tests/test_quem_chama.sh:216`, `bin/tests/test_quem_chama.sh:223`, `bin/tests/test_quem_chama.sh:228`, `bin/tests/test_quem_chama.sh:229`, `bin/tests/test_quem_chama.sh:243`, `bin/tests/test_quem_chama.sh:284`, `bin/tests/test_quem_chama.sh:340`, `bin/tests/test_quem_chama.sh:352`, `bin/tests/test_quem_chama.sh:354`).

Execução integral do selo e mutantes integrais: **[nao verificado]**.

## 06/11 04:34 · sessao b4f914dd4fbb · LASTRO-PARTE-2 · verificador · origem=Code

**Volta 1: há pendências verificáveis.** O diagrama diverge do registro dos crons, e a nova nota ainda pode afirmar “a batida EXISTE” quando a lâmpada perdeu o marcador de isenção. (`app/docs/ARQUITETURA.mmd:218`; `app/config/crons.py:2157`; `app/ponto/services/cartorio.py:593`; `app/ponto/services/cartorio.py:638`; `app/ponto/services/lastro.py:340`.) Execução da suíte, RED, sombra em produção e instalação do cron: **[nao verificado]**.

**V1. Chamadores que quebram**

Não encontrei, na busca textual e por AST da cópia, chamador de produção das funções apagadas nem execução residual de `fechar_cobranca_com_lastro --apply`. Os consumidores encontrados usam a interface nova: consumidor → `fechar_pelo_marco_colab`; comando → `julgar`/`com_lastro_sem_evento`; cron → `--alarme`; placar → comando sem `--apply`. (`app/chamados/reconciliador.py:1370`; `app/ponto/management/commands/fechar_cobranca_com_lastro.py:52`; `app/config/crons.py:731`; `bin/placar_code.sh:117`.)

Crontab efetivamente instalado, scripts externos à cópia e processos carregando código antigo: **[nao verificado]**.

**V2. Escritores e leitores**

| Campo/interface | Quem escreve | Quem lê |
|---|---|---|
| `status_local`, neste caminho | `reconciliar()` decide pelo gate e chama `_gravar_estado`, que atribui e salva. (`app/chamados/models.py:616`; `app/chamados/models.py:222`; `app/chamados/models.py:279`.) | O próprio gate recarrega o estado; fila, API e sinal do cartório também o leem. (`app/chamados/models.py:638`; `app/chamados/services/painel.py:187`; `app/api/views.py:1145`; `app/chamados/signals.py:169`.) |
| Nota `lastro_encontrado` | Lastro chama `anexar_ocorrencia` somente após retorno verdadeiro do gate; a porta cria `FollowupChamado`. (`app/ponto/services/lastro.py:395`; `app/chamados/services/nota.py:103`.) | Histórico, templates e API leem o conteúdo; a API filtra notas de sistema pela palavra `justificativa`. (`app/chamados/services/historico.py:84`; `app/templates/chamados/detalhe_local.html:135`; `app/api/views.py:1129`; `app/api/views.py:62`.) |
| `EventoCelula.processado_em` / `tentativas` | Nascimento usa `NULL`/zero; consumidor incrementa tentativa ou marca processado. (`app/escala/models.py:1859`; `app/chamados/reconciliador.py:1379`; `app/chamados/reconciliador.py:1385`.) | Seleção dos pendentes, contador de mortos, semeadura, vigias e retenção. (`app/chamados/reconciliador.py:1238`; `app/chamados/reconciliador.py:1244`; `app/chamados/reconciliador.py:1284`; `app/ponto/services/lastro.py:415`; `app/chamados/reconciliador.py:1392`.) |
| Chave `lastro` do resultado | Inicializada e acumulada pelo consumidor. (`app/chamados/reconciliador.py:1327`; `app/chamados/reconciliador.py:1371`.) | Testes a verificam; o comando de produção formata outras chaves e **não imprime `lastro`**. (`app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:103`; `app/chamados/management/commands/consumir_eventos_celula.py:32`.) |

`lastro_encontrado` **não é coluna `tipo` do Followup**: o modelo não possui esse campo. O argumento participa da identidade quando existe `objeto_ref`; esta chamada não fornece referência, portanto cria nota sem chave de deduplicação. (`app/chamados/models.py:1598`; `app/chamados/models.py:1618`; `app/chamados/services/nota.py:88`; `app/chamados/services/nota.py:95`; `app/ponto/services/lastro.py:396`.)

Não encontrei leitor de produção comparando o resultado inteiro, somando suas chaves ou exigindo o texto antigo. O leitor de produção encontrado usa formatação por nomes; os leitores de notas examinados trabalham com apresentação, autoria ou outros padrões, como `SLA` e `[COBRANCA]`. (`app/chamados/management/commands/consumir_eventos_celula.py:32`; `app/chamados/services/sla.py:45`; `app/chamados/services/painel.py:353`.) Consumidores externos: **[nao verificado]**.

**V3. Transação, exceção e `adiar`**

Não há transação do consumidor envolvendo o colaborador inteiro. Cada `reconciliar()` abre seu próprio `atomic(savepoint=False)`; a nota vem depois. A atualização dos eventos ocorre em outro `atomic`, posterior e curto. (`app/chamados/reconciliador.py:1343`; `app/chamados/models.py:616`; `app/ponto/services/lastro.py:395`; `app/chamados/reconciliador.py:1376`.)

- Exceção no lastro entra em `falhas`: mantém os eventos pendentes e incrementa uma tentativa. IDs já lidos não voltam na mesma execução. (`app/ponto/services/lastro.py:398`; `app/chamados/reconciliador.py:1334`; `app/chamados/reconciliador.py:1379`.)
- Recusa normal do gate não é falha e permite marcar o evento processado. (`app/ponto/services/lastro.py:395`; `app/chamados/reconciliador.py:1385`.)
- `adiar=True` **não pula o lastro**: a chamada acontece depois de definir a flag; posteriormente o evento permanece pendente sem gastar tentativa. (`app/chamados/reconciliador.py:1363`; `app/chamados/reconciliador.py:1371`; `app/chamados/reconciliador.py:1380`.)
- Exceção anterior ao lastro pula sua execução, mas preserva o evento com tentativa incrementada. Colaborador inexistente pula o trabalho e permite processamento do evento. (`app/chamados/reconciliador.py:1344`; `app/chamados/reconciliador.py:1372`; `app/chamados/reconciliador.py:1379`.)

**Risco de nota perdida:** o fechamento pode confirmar e a inserção da nota falhar depois. No retry, o chamado terminal sai da fila e a nota não é recuperada. Possibilidade pelo código; ocorrência em produção **[nao verificado]**. (`app/chamados/models.py:616`; `app/ponto/services/lastro.py:385`; `app/ponto/services/lastro.py:395`; `app/chamados/services/nota.py:103`.)

**V4. Ordem e fecha/reabre**

A ordem explícita é: **rejunção → perguntas absolvidas → chamados absolvidos → premissa morta → órfãs → lastro**. Nenhum desses passos é chamado explicitamente depois do lastro. (`app/chamados/reconciliador.py:1351`; `app/chamados/reconciliador.py:1371`.)

Entretanto, salvar o fechamento dispara o cartório sincronamente. O cartório pode reemitir por `FURO_PARCIAL` ou `FURO_COM_COBRANCA_MORTA`, encontrando e reabrindo o mesmo chamado do dia. (`app/chamados/models.py:279`; `app/chamados/signals.py:164`; `app/chamados/signals.py:180`; `app/ponto/services/cartorio.py:1689`; `app/ponto/services/cartorio.py:1777`; `app/ponto/management/commands/detectar_ausencias.py:693`.)

**Existe fecha/reabre no mesmo evento:** entrada acesa encerra a cobrança antiga; saída apagada permite renascimento pelos marcos novos, posteriormente espelhados no chamado. A guarda considera os marcos **do pedido**, portanto não barra essa troca. O próprio fixture migrado acrescentou saída para evitar essa reabertura. (`app/chamados/models.py:1223`; `app/chamados/models.py:1248`; `app/chamados/models.py:1310`; `app/ponto/management/commands/detectar_ausencias.py:743`; `app/ponto/tests/test_bug114_fechador_de_dois_dias.py:223`; `app/ponto/tests/test_bug114_fechador_de_dois_dias.py:259`.)

Consequentemente, `lastro=1` e nota “Encerrado” não garantem estado final terminal. Isso não demonstra loop infinito pelo mesmo marco: após espelhamento, o marco apagado não passa pelo lastro; perguntas do marco aceso também são excluídas da ressuscitação. (`app/ponto/services/lastro.py:392`; `app/ponto/services/lastro.py:395`; `app/chamados/models.py:1452`.)

**V5. Produtores de evento e escapes**

As escritas persistentes de ata encontradas passam por `salvar_e_emitir`, que salva e emite no mesmo `atomic` quando `ata`/`veredito` foi lavrado. Não encontrei alteração persistente de ata sem emissão nas vias rastreadas. (`app/ponto/portas/celula.py:662`; `app/ponto/portas/celula.py:676`; `app/ponto/portas/celula.py:785`; `app/ponto/portas/celula.py:817`; `app/ponto/portas/celula.py:864`.)

- **Cartório:** tanto o ramo normal quanto `nunca_bateu` passam os campos lavrados a `carimbar`, que salva e emite. (`app/ponto/services/cartorio.py:1242`; `app/ponto/services/cartorio.py:1264`; `app/ponto/services/cartorio.py:1802`; `app/ponto/services/cartorio.py:1822`; `app/ponto/portas/celula.py:849`.)
- **Ausência/atestado e justificativa:** sinais chamam o cartório; a dispensa integra a ata posteriormente lavrada. (`app/ponto/signals.py:143`; `app/ponto/signals.py:150`; `app/ponto/signals.py:161`; `app/ponto/services/cartorio.py:766`; `app/ponto/services/cartorio.py:1800`.)
- **Validação de pergunta:** materialização de batida/ausência aciona seus sinais. O carimbo de validação sozinho não altera ata nem emite diretamente esse evento. (`app/chamados/services/validacao.py:395`; `app/chamados/services/validacao.py:430`; `app/chamados/services/materializacao.py:484`; `app/chamados/services/materializacao.py:897`; `app/ponto/signals.py:131`.)
- **Regeneração:** com recálculo, rejulga dias tocados, passados e não humanos. Frota usa `recalculo=False`; reversão grava DNA sem rejulgar naquele ato. São mudanças de **insumo sem relavra imediata**, não atas modificadas sem evento. (`app/ponto/portas/celula.py:390`; `app/escala/services/cadastro_tipo.py:348`; `app/ponto/portas/celula.py:569`.)
- **Isenção:** ligar a flag pelo cadastro aciona silenciamento, sem relavra imediata da ata nesse caminho. Também é lacuna de atualização do insumo, anterior à existência de uma lâmpada nova gravada. (`app/colaboradores/views.py:173`; `app/colaboradores/services/colaborador.py:55`; `app/chamados/signals.py:213`.)

Essas lacunas de insumo **não são necessariamente detectadas pelo vigia**, pois ele consulta a ata gravada; se ela ainda não acendeu, falta a condição examinada. (`app/ponto/services/lastro.py:289`; `app/ponto/services/lastro.py:417`.)

**V6. Zero do vigia e permanência**

`sem_evento=0` não significa ausência de chamado vivo com marco aceso: retidos ficam fora de `com_lastro`; registrados e silenciados ficam fora da fila. O retido continua no TOTAL do placar pela linha `(a2)`. (`app/ponto/services/lastro.py:318`; `app/ponto/services/lastro.py:416`; `app/chamados/services/painel.py:187`; `bin/placar_code.sh:121`; `bin/placar_code.sh:549`.)

Os três exemplos não demonstram, isoladamente, invisibilidade permanente:

- Retido liberado sem evento passa a `com_lastro` na próxima medição e será acusado se não houver pendente vivo. (`app/ponto/services/lastro.py:318`; `app/ponto/services/lastro.py:415`.)
- Eventos com `tentativas>=5` não entram em `eventos_pendentes`, portanto não mascaram esse vigia; há contador separado de mortos. (`app/chamados/reconciliador.py:1198`; `app/chamados/reconciliador.py:1238`; `app/chamados/management/commands/consumir_eventos_celula.py:37`.)
- Evento de outro dia serve para o colaborador inteiro: o lastro percorre todos os seus chamados elegíveis. (`app/chamados/reconciliador.py:1337`; `app/ponto/services/lastro.py:385`.)

Falhas antes do lastro podem manter temporariamente o vigia em zero enquanto existir evento vivo. Permanência por fluxo contínuo de falhas/eventos: **[nao verificado]**. (`app/chamados/reconciliador.py:1351`; `app/chamados/reconciliador.py:1372`; `app/chamados/reconciliador.py:1379`; `app/ponto/services/lastro.py:415`.)

**V7. Forma das lâmpadas e verdade da nota**

Nos caminhos normais, as chaves conferem: isenção usa hora prevista com `isencao=True`; dispensa inteira/parcial usa `dispensada`; o cartório propaga ambas. Não encontrei terceira chave equivalente nesses produtores. (`app/escala/utils.py:858`; `app/escala/utils.py:1267`; `app/ponto/services/cartorio.py:556`; `app/ponto/services/cartorio.py:680`; `app/ponto/services/cartorio.py:691`; `app/ponto/services/cartorio.py:774`.)

**A garantia universal da nota falha:**

1. Atas antigas sem marcador são explicitamente documentadas como ambíguas; `como_acendeu` interpreta ausência dos marcadores como batida. Presença atual desses registros em produção: **[nao verificado]**. (`app/ponto/services/cartorio.py:547`; `app/ponto/services/lastro.py:336`; `app/ponto/services/lastro.py:340`.)
2. Há caminho atual que perde o marcador: `_reais` inclui horas de isenção; o ramo de marco fora da régua acende por proximidade sem copiar `isencao`. Na reprodução em memória da função real, uma célula isenta às `07:00`, com DNA `hi=07:20, hii=07:00`, gerou E07:20 acesa sem marcador. A nova nota a classifica como batida. Atingibilidade desse cadastro em produção: **[nao verificado]**. (`app/ponto/services/cartorio.py:593`; `app/ponto/services/cartorio.py:624`; `app/ponto/services/cartorio.py:638`; `app/ponto/services/lastro.py:340`; `app/ponto/services/lastro.py:355`.)

**V8. Crons, contadores e selos**

No diff fornecido, as mudanças de `crons.py` correspondem à entrada com `--alarme`, saída dos juízes e inclusão como vigia. Esses destinos conferem na cópia; comparação independente com a base `a37c673d`: **[nao verificado]**. (`app/config/crons.py:731`; `app/config/crons.py:2018`; `app/config/crons.py:2157`.)

Placar, contrato estrutural e duração não exigem manter `--apply` ou o papel juiz: o placar preserva `(a)/(a2)`, o contador deriva o papel declarado e o JSON guarda somente duração. O selo de host exige justamente o comando sem `--apply`. (`bin/placar_code.sh:114`; `bin/placar_code.sh:549`; `app/core/contratos_estruturais.py:263`; `app/config/crons.py:2322`; `app/config/crons_duracao.json:32`; `bin/tests/test_placar_conta_o_retido_do_lastro.sh:594`.)

**Há divergência certa no diagrama:** ainda mostra 39 vigias, dois juízes e este comando ligado a juiz. O gerador deriva essas informações do registro atual, e o selo exige igualdade. A extração em memória gerou 40 vigias e um juiz; execução da suíte completa: **[nao verificado]**. (`app/docs/ARQUITETURA.mmd:207`; `app/docs/ARQUITETURA.mmd:209`; `app/docs/ARQUITETURA.mmd:218`; `app/core/management/commands/gerar_diagrama.py:242`; `app/core/management/commands/gerar_diagrama.py:264`; `app/core/tests/test_selo_diagrama_do_codigo.py:44`.)

**V9. Classes de reprovação**

Não identifiquei novo juiz de fechamento, escritor manual de status ou leitura explícita nova de relógio no corpo acrescentado: ele reutiliza `marcos_acesos`, `reconciliar` e a porta de nota. A trava do chamado continua dentro do gate; não foi acrescentada transação envolvendo o colaborador inteiro. (`app/ponto/services/lastro.py:392`; `app/ponto/services/lastro.py:395`; `app/ponto/services/lastro.py:396`; `app/chamados/models.py:616`; `app/chamados/models.py:673`; `app/chamados/reconciliador.py:1376`.)

As ressalvas concretas são a testemunha sem prova suficiente de batida, a nota fora da transação do fechamento, o fecha/reabre síncrono e o artefato gerado divergente. (`app/ponto/services/lastro.py:340`; `app/ponto/services/lastro.py:395`; `app/chamados/signals.py:180`; `app/docs/ARQUITETURA.mmd:218`.) RED executado, sombra N=0, pré-requisitos de pouso e instalação: **[nao verificado]**.

**V10. Mordida dos testes migrados** [nao verificado]

- **BUG114:** o teste de 30 dias sem lâmpada mudou deliberadamente de “fecha” para “permanece vivo e alarma”; não preserva literalmente a expectativa anterior de fecho do degrau 2. Os negativos sem batida/longe/sem marco/agora só classificam com `julgar`, sem testar o novo escritor, que percorre a fila diretamente. Esses negativos isolados passam mesmo com defeito no novo fechamento. (`app/ponto/tests/test_bug114_fechador_de_dois_dias.py:144`; `app/ponto/tests/test_bug114_fechador_de_dois_dias.py:162`; `app/ponto/tests/test_bug114_fechador_de_dois_dias.py:171`; `app/ponto/tests/test_bug114_fechador_de_dois_dias.py:182`; `app/ponto/tests/test_bug114_fechador_de_dois_dias.py:193`; `app/ponto/services/lastro.py:385`.)
- **BUG115:** mantém o positivo “marco aceso com furo vizinho → `lastro=1` e terminal” e o negativo “marco apagado → vivo”. O teste de recuo por outro tipo passou a cobrir somente classificação. (`app/ponto/tests/test_bug115_lampada_do_marco.py:185`; `app/ponto/tests/test_bug115_lampada_do_marco.py:207`; `app/ponto/tests/test_bug115_lampada_do_marco.py:167`.)
- **O231:** mantém medições discordantes, exige uma única chamada a `julgar` e relaciona quadro impresso ao número do vigia. (`app/ponto/tests/test_o231_lastro_mede_uma_vez.py:54`; `app/ponto/tests/test_o231_lastro_mede_uma_vez.py:64`.)
- **lastro_rele:** preserva o par objeto obsoleto → zero/sem nota e chamado vivo → um/com nota. O fixture efetivamente planta o marco cobrado aceso. (`app/ponto/tests/test_lastro_rele_o_chamado.py:46`; `app/chamados/tests/_dia_regularizado.py:8`; `app/ponto/tests/test_lastro_rele_o_chamado.py:59`; `app/ponto/tests/test_lastro_rele_o_chamado.py:69`.)

Remover a chamada nova do consumidor é distinguido pelos positivos que exigem `lastro=1`; não são selos inteiramente vazios. Execução com cura revertida e resultados dinâmicos: **[nao verificado]**. (`app/ponto/tests/test_bug115_lampada_do_marco.py:204`; `app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:103`.)

## 06/11 04:33 · sessao 9be1368b2885 · OTIMIZA-3 · verificador · origem=Code

**Parecer: bloqueia pelo achado 1.** É uma falha que permanece na classificação; não a atribuo como regressão introduzida nesta volta (`bin/quem_chama.py:348`, `bin/quem_chama.py:352`). Ranges históricos e execução integral do selo: **[nao verificado]**.

1. **BLOQUEIA — P1/P6: registro de um lado ainda mascara decorador opaco do outro.**

   A soma conserva os dois metadados, mas `_tipo_simbolo()` retorna `filtro/tag` antes de examinar os decoradores opacos (`bin/quem_chama.py:348`, `bin/quem_chama.py:352`, `bin/quem_chama.py:707`, `bin/quem_chama.py:709`).

   Contraexemplo exercitado em memória:

   ```python
   # base
   @register.filter(name="velho")
   def f():
       return 1

   # ponta
   @receiver
   def f():
       return 2
   ```

   Com um `receiver` que guarda a função numa lista de callbacks, o despachante executa `f`, mas o instrumento conserva `registros=[('filtro','velho')]`, classifica como `filtro/tag` e imprime **`simbolo f [filtro/tag]: 0 chamador(es)`**. O decorador `receiver`, isoladamente, exigiria “nao varrido” (`bin/quem_chama.py:354`, `bin/quem_chama.py:765`). Ocorrência dessa transição na produção: **[nao verificado]**.

   Portanto, unir campos não basta para tornar a classificação a mais restritiva: ela precisa considerar também o motivo desconhecido representado pelo decorador opaco (`bin/quem_chama.py:348`, `bin/quem_chama.py:352`).

2. **FILA — P1/P2: os demais casos pedidos são conservadores; migração singular/plural está consistente.**

   - `membro=True` em qualquer lado prevalece e impede contagem. No produtor atual, mover `f` para uma classe normalmente muda também a chave para `C.f`; não constitui conflito do mesmo `q` (`bin/quem_chama.py:290`, `bin/quem_chama.py:292`, `bin/quem_chama.py:342`, `bin/quem_chama.py:706`).
   - Registro com nome `None` junto de registro constante continua “nome nao resolvido”, pois `all(...)` falha (`bin/quem_chama.py:349`).
   - Decorador removido da ponta permanece na união; símbolo existente somente num lado também recebe a estrutura plural (`bin/quem_chama.py:705`, `bin/quem_chama.py:707`, `bin/quem_chama.py:709`).
   - Outros campos conservam o primeiro valor, normalmente da base. Contudo, o produtor deriva `nome` do mesmo identificador usado em `q`; divergência de `nome` para uma mesma chave não é produzida pelos caminhos atuais (`bin/quem_chama.py:292`, `bin/quem_chama.py:300`, `bin/quem_chama.py:700`, `bin/quem_chama.py:704`).

   | Campo | Quem escreve | Quem lê |
   |---|---|---|
   | `registro` | `_add`: `bin/quem_chama.py:282` | Soma: `bin/quem_chama.py:708`, `bin/quem_chama.py:709`; exclusão da cópia: `bin/quem_chama.py:704` |
   | `registros` | Inicialização: `bin/quem_chama.py:705`; acréscimo: `bin/quem_chama.py:709` | Deduplicação: `bin/quem_chama.py:708`; classificação: `bin/quem_chama.py:348`, `bin/quem_chama.py:349`; template: `bin/quem_chama.py:606` |

   Não sobrou leitor singular sobre o dict somado, nem leitor plural sobre dict cru no fluxo operacional: classificação e construção dos alvos acontecem depois da soma (`bin/quem_chama.py:711`, `bin/quem_chama.py:716`). Contraexemplo adicional de fluxo incorreto: **[nao verificado]**.

3. **FILA — P3: há falso positivo real de EMBUTIDO próprio, mas ele não esconde chamadores.**

   O valor textual iniciado em `app/core/placar_estrutural.py:443` contém `from core import contratos_estruturais` e, adiante no mesmo literal concatenado, `logs/placar_estrutural/...` (`app/core/placar_estrutural.py:461`, `app/core/placar_estrutural.py:485`).

   A alternativa permissiva da regex atravessa esse texto e reconhece falsamente uma importação de **`core.placar_estrutural`** (`bin/quem_chama.py:93`). A varredura em memória do arquivo real emitiu esse autoimport na linha 443; o delta agora o aceita antes da guarda do próprio arquivo (`bin/quem_chama.py:472`, `bin/quem_chama.py:738`, `bin/quem_chama.py:741`). Nesse exemplo, o texto não contém um autoimport literal correto: é também um falso casamento da regex (`app/core/placar_estrutural.py:461`, `app/core/placar_estrutural.py:485`).

   Isso pode substituir um zero verdadeiro por “nao varrido”, conservadoramente. Não elimina chamador encontrado: o código acrescenta sítios aos conjuntos existentes e imprime os achados também nas facetas rebaixadas (`bin/quem_chama.py:753`, `bin/quem_chama.py:755`, `bin/quem_chama.py:773`).

4. **FILA — P4: cada grupo de asserções novas ainda admite mutante plausível.**

   A tabela cobre as condições acrescentadas ou fortalecidas. A preservação dessas condições foi conferida por inspeção; sobrevivência ao **selo integral** com cada mutante: **[nao verificado]**.

   | Asserções do delta | Mutante que as deixa satisfeitas e contraexemplo não coberto |
   |---|---|
   | `r5-init-reexporta`, `r5-import-local` — `bin/tests/test_quem_chama.sh:219`, `bin/tests/test_quem_chama.sh:220` | Duplicar linhas durante a renderização dentro da própria faceta. `tot == sob` continua verdadeiro; o helper não exige ocorrência única (`bin/quem_chama.py:773`, `bin/tests/test_quem_chama.sh:197`). |
   | `r5-comentario-multilinha`, `r5-comment-bloco-multilinha` — `bin/tests/test_quem_chama.sh:234`, `bin/tests/test_quem_chama.sh:235` | Trocar `.*?` por `.*` na remoção de `{% comment %}`. O único bloco do fixture continua removido; dois blocos passam a apagar um include executável entre eles (`bin/quem_chama.py:572`, `bin/tests/test_quem_chama.sh:146`). |
   | `r5-filter-tag` — `bin/tests/test_quem_chama.sh:275` | Ampliar `filter` para qualquer nome de tag na regex. O positivo permanece verde, mas argumentos de outras tags podem virar usos falsos de filtro (`bin/quem_chama.py:609`, `bin/tests/test_quem_chama.sh:137`). |
   | `r5-embutido-arquivo-sitio-*` — `bin/tests/test_quem_chama.sh:317` | Omitir sítios e rebaixamento da faceta **arquivo somente nos autoimports**. O loop cobre importadores externos; o caso próprio exige apenas símbolo (`bin/tests/test_quem_chama.sh:309`, `bin/tests/test_quem_chama.sh:332`, `bin/tests/test_quem_chama.sh:333`). |
   | `r5-embutido-lista-*` — `bin/tests/test_quem_chama.sh:319` | Usar cabeçalho genérico somente para `rotulo == 'arquivo'`. Motivo e sítio continuam presentes; a lista nominal é exigida apenas sob símbolo (`bin/quem_chama.py:760`, `bin/tests/test_quem_chama.sh:316`, `bin/tests/test_quem_chama.sh:319`). |
   | `r5-decorador-so-na-ponta`, `-zero`, `r5-decorador-somado` — `bin/tests/test_quem_chama.sh:325`, `bin/tests/test_quem_chama.sh:326`, `bin/tests/test_quem_chama.sh:328` | Substituir a união por `cur['decor'] = info['decor']`. Os dois fixtures têm opaco na ponta; falta a transição inversa, com opaco removido (`bin/quem_chama.py:707`, `bin/tests/test_quem_chama.sh:171`, `bin/tests/test_quem_chama.sh:172`). |
   | `r5-filtro-renomeado` — `bin/tests/test_quem_chama.sh:330` | Substituir o acréscimo por `cur['registros'][:] = [info['registro']]`. Fica somente o registro novo, exatamente o usado pelo fixture; um template usando `velho_f` perderia o chamador (`bin/quem_chama.py:709`, `bin/tests/test_quem_chama.sh:143`, `bin/tests/test_quem_chama.sh:144`). |
   | `r5-embutido-proprio`, `-sitio` — `bin/tests/test_quem_chama.sh:332`, `bin/tests/test_quem_chama.sh:333` | Processar os símbolos do autoimport, omitindo somente a faceta arquivo. Ambas continuam satisfeitas (`bin/quem_chama.py:752`, `bin/quem_chama.py:755`). |

5. **FILA — P4: `_so_na_fac` pode reprovar duplicação legítima entre facetas.**

   Contraexemplo exercitado em memória: base registra `f` como `a` e `g` como `b`; ponta troca os nomes. Pela união correta, ambos passam a possuir o registro `b`; o template `{{ x|b }}` produz o mesmo texto de achado sob **f e g** (`bin/quem_chama.py:709`, `bin/quem_chama.py:606`, `bin/quem_chama.py:615`).

   Nesse caso, `_so_na_fac` encontra `tot=2`, `sob=1` e reprova, embora a duplicação represente a política de buscar base e ponta (`bin/tests/test_quem_chama.sh:197`, `bin/tests/test_quem_chama.sh:198`). Falha espúria nos fixtures atuais: **[nao verificado]**; o fixture de renomeação tem somente um símbolo registrado (`bin/tests/test_quem_chama.sh:143`, `bin/tests/test_quem_chama.sh:173`).

   Nos imports atuais, arquivo e símbolo usam textos diferentes; contabilizar ambos não viola essas exclusividades (`bin/quem_chama.py:450`, `bin/quem_chama.py:455`).

6. **FILA — P5: nenhum dos antigos achados 5, 6, 8 e 9 ganhou falso zero demonstrado com o delta.**

   - **Antigo 5:** a linha continua calculada pelo valor decodificado; o exemplo físico permanece nas linhas 250–252. Isso desloca o sítio, sem impedir sua emissão (`bin/quem_chama.py:472`, `app/core/tests/test_selo_diagrama_do_codigo.py:250`, `app/core/tests/test_selo_diagrama_do_codigo.py:251`).
   - **Antigo 6:** o relativo do heredoc continua escapando, mas há import absoluto que rebaixa o mesmo alvo (`bin/regua_calendario.sh:30`, `bin/regua_relogio.sh:35`). Os pacotes dos exemplos também têm imports diretos reconhecíveis (`app/colaboradores/tests/test_s120.py:65`, `app/chamados/apps.py:10`, `bin/quem_chama.py:449`).
   - **Antigo 8:** as alterações fortalecem o selo; os limites restantes estão nos achados 4–5, não constituem por si mudança da contagem (`bin/tests/test_quem_chama.sh:219`, `bin/tests/test_quem_chama.sh:275`).
   - **Antigo 9:** a divergência continua conservadora no exemplo apresentado: o instrumento conta a string após `#`, enquanto o censo a corta (`app/core/management/commands/gerar_diagrama.py:873`, `bin/quem_chama.py:474`, `app/core/censo_codigo.py:60`).

   Outro defeito demonstrado no delta, além dos descritos acima: **[nao verificado]**.

## 06/11 04:32 · sessao 896a2e2fbe7f · - · - · origem=Ronald (tmux)

V5 — As vias persistentes de ata examinadas passam pela porta que salva e emite `EventoCelula` no mesmo `atomic`; a emissão depende de `ata`/`veredito` entre os campos lavrados. Não encontrei escritor persistente de ata que contorne isso nas vias rastreadas. (`app/ponto/portas/celula.py:662`, `app/ponto/portas/celula.py:676`, `app/ponto/portas/celula.py:783`, `app/ponto/portas/celula.py:810`, `app/ponto/portas/celula.py:862`.)

- Cartório normal: monta ata, chama `lavrar_veredito(save=False)` e depois `carimbar(lavrados=_cv)`; o ramo `nunca_bateu` faz o mesmo. (`app/ponto/services/cartorio.py:1800`, `app/ponto/services/cartorio.py:1822`, `app/ponto/services/cartorio.py:1242`, `app/ponto/services/cartorio.py:1264`.)
- Ausência/atestado: `post_save`/`post_delete` julgam os dias; ausência aprovada inteira ou dia justificado dispensa as lâmpadas. Parcial dispensa somente os marcos no período declarado, com `dispensada`. (`app/ponto/signals.py:143`, `app/ponto/signals.py:150`, `app/ponto/services/cartorio.py:766`, `app/escala/utils.py:1263`.)
- Justificativa: `post_save` chama o cartório; a grade marca `justificado`, lido pela dispensa. (`app/ponto/signals.py:161`, `app/escala/utils.py:1275`, `app/ponto/services/cartorio.py:768`.)
- Decisão administrativa justificada: cria ausência aprovada e lavra o veredito administrativo; a lavra emite pela mesma porta. (`app/ponto/services/veredito_celula.py:310`, `app/ponto/services/veredito_celula.py:330`, `app/ponto/portas/celula.py:815`.)
- Validação de pergunta: chama materialização; plantio de batida ou criação de ausência aciona os respectivos sinais e daí a lavra. O carimbo de validação sozinho não altera ata nem produz diretamente `EventoCelula`; o receptor da pergunta examinado trata o beco da disputa. (`app/chamados/services/validacao.py:395`, `app/chamados/services/validacao.py:430`, `app/chamados/services/materializacao.py:897`, `app/chamados/services/materializacao.py:977`, `app/chamados/services/materializacao.py:484`, `app/ponto/signals.py:131`, `app/chamados/signals.py:191`.)
- Regeneração: escreve DNA e, com `recalculo=True`, rejulga dias tocados, passados e não humanos; a nova ata então emite evento. Frota passa `recalculo=False`; reversão grava DNA e retorna sem rejulgamento. São alterações de **insumos sem relavra imediata**, não atas alteradas sem emissão. (`app/ponto/portas/celula.py:266`, `app/ponto/portas/celula.py:390`, `app/escala/services/cadastro_tipo.py:348`, `app/ponto/portas/celula.py:569`.)
- Isenção: `passe_livre` grava a flag pelo serviço do colaborador; o receptor examinado silencia chamados, sem relavrar ata ou emitir `EventoCelula`. Também é mudança de insumo sem relavra imediata. (`app/colaboradores/views.py:173`, `app/colaboradores/services/colaborador.py:55`, `app/chamados/signals.py:213`.)

V7 — Nos ramos normais, as chaves conferem: `isencao=True` credita hora prevista; `dispensada` representa dispensa, inclusive parcial; ambos são propagados às lâmpadas. Não encontrei terceira chave equivalente nas funções examinadas. (`app/escala/utils.py:858`, `app/escala/utils.py:1267`, `app/ponto/services/cartorio.py:556`, `app/ponto/services/cartorio.py:680`, `app/ponto/services/cartorio.py:691`.)

Há duas limitações da garantia “EXISTE só quando foi batida”:

1. `como_acendeu` considera batida qualquer lâmpada sem esses dois marcadores, até recorrendo à hora prevista; o próprio cartório documenta atas antigas de isenção sem marcador, mantidas ambíguas. Existência desses casos no acervo atual: **[nao verificado]**. (`app/ponto/services/lastro.py:336`, `app/ponto/services/lastro.py:340`, `app/ponto/services/cartorio.py:547`.)
2. O ramo de marco fora da régua coleta `_reais` sem excluir isenção e acende por proximidade sem propagar seus marcadores. Reproduzi a função real extraída por AST, em memória: `cels=[{'hora':'07:00','missing':False,'isencao':True}]`, `marcos=[{'tipo':'E','hora':'07:00'}]`, `dna.marcos={'hi':'07:20','hii':'07:00'}` produz `{'tipo':'E','hora':'07:20','acesa':True,'luz':'07:00','tipo_real':None}`, sem `isencao`. Essa lâmpada é classificada como batida pela nova nota. Atingibilidade desse cadastro em produção: **[nao verificado]**. (`app/ponto/services/cartorio.py:593`, `app/ponto/services/cartorio.py:624`, `app/ponto/services/cartorio.py:638`, `app/ponto/services/lastro.py:340`, `app/ponto/services/lastro.py:355`.)

A referência da nova docstring ao predicado do cartório não é exata: `status_dia` exclui `isencao` e `dispensada`, enquanto o contador mostrado do cartório exclui somente `dispensada`. (`app/ponto/services/lastro.py:332`, `app/ponto/status_dia.py:185`, `app/ponto/services/cartorio.py:1373`.)

V9 — O novo fecho reutiliza `marcos_acesos` e `reconciliar`, e a nota usa a porta de ocorrência; não introduz escrita manual de status nem leitura explícita de relógio nesse corpo. O risco identificado nesta exploração é a testemunha afirmar batida a partir de lâmpada que perdeu/omitiu o marcador, conforme V7. (`app/ponto/services/lastro.py:388`, `app/ponto/services/lastro.py:392`, `app/ponto/services/lastro.py:395`, `app/ponto/services/lastro.py:396`.)

A reprodução foi executada com `python3 -B`, extração de AST e objetos em memória, sem comando de escrita ou criação de arquivo; ausência absoluta de efeitos externos ao processo: **[nao verificado]**.

## 06/11 04:31 · sessao 8e267515a30b · - · - · origem=Ronald (tmux)

V3. `fechar_pelo_marco_colab` roda sem transação externa do consumidor; cada `reconciliar()` abre seu `atomic(savepoint=False)`, e a nota é gravada depois que esse gate retorna. A marcação dos eventos tem outro `atomic`, curto e posterior (`app/chamados/reconciliador.py:1343`, `app/chamados/reconciliador.py:1376`; `app/chamados/models.py:616`; `app/ponto/services/lastro.py:395`).

- Exceção por chamado entra em `falhas`; o evento continua pendente, recebe `tentativas+1` e não volta na mesma execução porque seu ID já entrou em `lidos` (`app/ponto/services/lastro.py:398`; `app/chamados/reconciliador.py:1334`, `app/chamados/reconciliador.py:1378`).
- Recusa normal de `reconciliar()` não é falha: devolve zero fechamentos e permite marcar o evento processado (`app/ponto/services/lastro.py:395`; `app/chamados/reconciliador.py:1385`).
- `adiar` não pula o lastro: órfãs definem a flag, lastro roda em seguida e só depois a flag preserva o evento pendente sem gastar tentativa (`app/chamados/reconciliador.py:1363`, `app/chamados/reconciliador.py:1371`, `app/chamados/reconciliador.py:1380`).
- Há buraco de atomicidade: falha ao anexar a nota pode ocorrer depois do fechamento já commitado; o retry exclui o chamado fechado e não recupera essa nota. É possibilidade pelo código, ocorrência em produção [nao verificado] (`app/chamados/models.py:616`; `app/ponto/services/lastro.py:385`, `app/ponto/services/lastro.py:395`; `app/chamados/services/nota.py:103`).
- Exceção anterior à chamada de lastro pula sua execução, mas deixa evento pendente com tentativa gasta; colaborador inexistente pula todo o trabalho e permite marcar processado (`app/chamados/reconciliador.py:1344`, `app/chamados/reconciliador.py:1372`, `app/chamados/reconciliador.py:1378`).

V4. A ordem explícita é rejunção → perguntas absolvidas → chamados absolvidos → premissa morta → órfãs → lastro (`app/chamados/reconciliador.py:1351`, `app/chamados/reconciliador.py:1371`).

Entretanto, o fechamento dispara `post_save` síncrono que chama o cartório; esse cartório pode reabrir o mesmo chamado dentro do próprio `save` por `FURO_PARCIAL` ou `FURO_COM_COBRANCA_MORTA` (`app/chamados/models.py:268`; `app/chamados/signals.py:164`, `app/chamados/signals.py:180`; `app/ponto/services/cartorio.py:1689`, `app/ponto/services/cartorio.py:1777`).

Caso concreto permitido pelo código: E acesa fecha a cobrança antiga; S apagada faz o emissor pedir renascimento pelo outro marco. A guarda pergunta os marcos **do pedido**, e o emissor depois substitui os marcos cobrados pelos apagados atuais (`app/chamados/models.py:1223`, `app/chamados/models.py:1248`, `app/chamados/models.py:1310`; `app/ponto/management/commands/detectar_ausencias.py:693`, `app/ponto/management/commands/detectar_ausencias.py:743`).

Assim, `out['lastro']=1` e nota “Encerrado” não provam estado final encerrado: o gate pode retornar `True` após reabertura síncrona por outra instância, sem releitura final no lastro (`app/chamados/models.py:620`; `app/ponto/services/lastro.py:395`). O próprio teste migrado passou a adicionar saída e documenta que sem ela o emissor reabre (`app/ponto/tests/test_bug114_fechador_de_dois_dias.py:223`, `app/ponto/tests/test_bug114_fechador_de_dois_dias.py:259`).

Isso demonstra fecha/reabre no mesmo evento, mas não demonstra loop infinito pelo mesmo marco: após espelhamento, a cobrança passa a nomear o marco apagado, que o lastro não fecha; perguntas do marco aceso também são excluídas da ressuscitação (`app/ponto/management/commands/detectar_ausencias.py:743`; `app/ponto/services/lastro.py:392`; `app/chamados/models.py:1450`).

V6. Não demonstrei falso negativo **permanente**, no universo `com_lastro`, causado isoladamente pelos três exemplos apresentados (`app/ponto/services/lastro.py:416`):

- Retido liberado sem evento: nova medição passa a colocá-lo em `com_lastro`, e sem pendente vivo o vigia acusa (`app/ponto/services/lastro.py:318`, `app/ponto/services/lastro.py:415`).
- Tentativas ≥5: esses eventos não entram em `eventos_pendentes`; portanto não mascaram o vigia. Mortos têm contador separado (`app/chamados/reconciliador.py:1198`, `app/chamados/reconciliador.py:1238`; `app/chamados/management/commands/consumir_eventos_celula.py:37`).
- Evento de outro dia: não mascara por si só, porque o consumidor agrupa por colaborador e o lastro percorre todos os chamados elegíveis dele, independentemente do dia do evento (`app/chamados/reconciliador.py:1337`; `app/ponto/services/lastro.py:385`).

Há `sem_evento=0` com vivo e marco aceso por definição: retido fica fora de `com_lastro`; registrado e silenciado ficam fora da própria fila. Isso limita o alcance do número, sem demonstrar regressão da migração (`app/ponto/services/lastro.py:318`; `app/chamados/services/painel.py:186`; `app/chamados/catalogo/motor.py:24`).

Zero também não certifica execução futura: falha anterior ao lastro preserva pendentes que o mascaram temporariamente; fluxo contínuo de novos eventos com falhas poderia prolongar a máscara, mas ocorrência ou permanência em produção [nao verificado] (`app/chamados/reconciliador.py:1351`, `app/chamados/reconciliador.py:1372`, `app/chamados/reconciliador.py:1379`; `app/ponto/services/lastro.py:415`).

## 06/11 04:31 · sessao 135dfe29a7ab · - · - · origem=Ronald (tmux)

P4: as mudanças matam os mutantes anteriormente apontados, mas não provam toda a união; há sobreviventes plausíveis por inspeção. Execução do selo completo: **[nao verificado]** (`bin/tests/test_quem_chama.sh:27`, `bin/tests/test_quem_chama.sh:183`).

| Asserções novas | Mutante que matam | Mutante plausível que continua verde no conjunto novo | [nao verificado]
|---|---|---|
| `r5-init-reexporta`, `r5-import-local` | Duplicar `importa mudada` sob arquivo, mantendo sob símbolo (`bin/tests/test_quem_chama.sh:219`, `bin/tests/test_quem_chama.sh:220`). | Duplicar cada linha durante renderização **dentro da própria faceta**: `tot == sob` continua verdadeiro; o helper não exige uma ocorrência (`bin/tests/test_quem_chama.sh:197`, `bin/tests/test_quem_chama.sh:198`, `bin/quem_chama.py:773`). |
| `r5-comentario-multilinha`, `r5-comment-bloco-multilinha` | Duplicar achado sob outra faceta; tirar `re.S` do comentário de bloco (`bin/tests/test_quem_chama.sh:234`, `bin/tests/test_quem_chama.sh:235`, `bin/quem_chama.py:573`). | Tornar o comentário de bloco guloso, substituindo `.*?` por `.*`: fixture com um bloco continua idêntico; com dois blocos, apaga um include executável colocado entre eles (`bin/quem_chama.py:572`, `bin/tests/test_quem_chama.sh:146`). **Exercitado em memória.** |
| `r5-filter-tag` | Duplicar o mesmo texto de filtro em outra faceta (`bin/tests/test_quem_chama.sh:275`). | Ampliar a alternativa `{% filter ... %}` da regex para `{% QUALQUER_TAG ... %}`; o positivo continua encontrado e faltam negativos para argumentos de outras tags (`bin/quem_chama.py:609`, `bin/tests/test_quem_chama.sh:137`, `bin/tests/test_quem_chama.sh:275`). |
| `r5-embutido-arquivo-sitio-*` | Guardar sítios somente sob símbolos (`bin/tests/test_quem_chama.sh:317`, `bin/quem_chama.py:755`). | Suprimir sítio/rebaixamento **somente na faceta arquivo dos autoimports**: o loop cobre importadores externos; `selfmod` só exige símbolo (`bin/tests/test_quem_chama.sh:309`, `bin/tests/test_quem_chama.sh:332`, `bin/tests/test_quem_chama.sh:333`). |
| `r5-embutido-lista-*` | Remover o ramo especializado de cabeçalho EMBUTIDO, deixando apenas motivo genérico (`bin/tests/test_quem_chama.sh:319`, `bin/quem_chama.py:760`). | Usar cabeçalho genérico somente para `rotulo == 'arquivo'`: sítio e motivo permanecem sob arquivo, lista é exigida somente sob símbolo (`bin/tests/test_quem_chama.sh:316`, `bin/tests/test_quem_chama.sh:317`, `bin/tests/test_quem_chama.sh:319`). |
| `r5-decorador-so-na-ponta`, negativo `-zero`, `r5-decorador-somado` | Conservar decoradores da base; atualizar apenas se lista antiga vazia (`bin/tests/test_quem_chama.sh:325`, `bin/tests/test_quem_chama.sh:326`, `bin/tests/test_quem_chama.sh:328`). | Trocar união por `cur['decor'] = info['decor']`: ambos fixtures têm decorador opaco na ponta, portanto passam; retirada do opaco na ponta deixa de conservar o motivo da base (`bin/quem_chama.py:700`, `bin/quem_chama.py:707`, `bin/tests/test_quem_chama.sh:171`, `bin/tests/test_quem_chama.sh:172`). |
| `r5-filtro-renomeado` | Conservar somente nome antigo (`bin/tests/test_quem_chama.sh:330`). | Trocar append por `cur['registros'][:] = [info['registro']]`: fica somente nome da ponta, precisamente o único usado pelo fixture; template usando `velho_f` perderia chamador (`bin/quem_chama.py:709`, `bin/tests/test_quem_chama.sh:143`, `bin/tests/test_quem_chama.sh:144`, `bin/tests/test_quem_chama.sh:173`). |
| `r5-embutido-proprio`, `-sitio` | Descartar autoimport antes do tratamento EMBUTIDO; rebaixar sem listar sítio sob símbolo (`bin/tests/test_quem_chama.sh:332`, `bin/tests/test_quem_chama.sh:333`, `bin/quem_chama.py:738`, `bin/quem_chama.py:741`). | Mesmo sobrevivente da faceta arquivo descrito acima: processar símbolos de autoimport, omitindo somente arquivo (`bin/quem_chama.py:752`, `bin/quem_chama.py:755`, `bin/tests/test_quem_chama.sh:332`, `bin/tests/test_quem_chama.sh:333`). |

Os sobreviventes são **lacunas do selo, gravidade fila**, e não defeitos presentes automaticamente no código: a implementação atual conserva listas e decoradores por união (`bin/quem_chama.py:707`, `bin/quem_chama.py:709`).

A exclusividade global pode reprovar uma duplicação legítima **em outro fixture**. Contraexemplo exercitado em memória: base registra `f` como `a` e `g` como `b`; ponta troca os nomes. Pela união, ambos possuem registro `b`; `{{ x|b }}` produz o mesmo texto sob `f` e `g`, coerentemente com a busca dos dois lados. `_so_na_fac` reprovaria porque `tot=2`, `sob=1` (`bin/quem_chama.py:708`, `bin/quem_chama.py:606`, `bin/quem_chama.py:615`, `bin/tests/test_quem_chama.sh:197`, `bin/tests/test_quem_chama.sh:198`). **Falha espúria nos fixtures atuais: [nao verificado]**; `ren_tags` contém apenas um símbolo registrado (`bin/tests/test_quem_chama.sh:143`, `bin/tests/test_quem_chama.sh:173`).

Nos imports atuais, carregar arquivo e símbolo simultaneamente não viola a exclusividade: os textos são respectivamente `importa de m.lib` e `importa mudada (mudado no diff)` (`bin/quem_chama.py:450`, `bin/quem_chama.py:455`, `bin/tests/test_quem_chama.sh:219`, `bin/tests/test_quem_chama.sh:220`).

## 06/11 04:31 · sessao b86b91d1e1ba · - · - · origem=Ronald (tmux)

V8: há uma divergência concreta no artefato gerado. O cron está como vigia e com `--alarme` em `app/config/crons.py:731` e `app/config/crons.py:2157`, mas o diagrama ainda publica 39 vigias, dois juízes e a aresta de `fechar_cobranca_com_lastro` para juiz em `app/docs/ARQUITETURA.mmd:207`, `app/docs/ARQUITETURA.mmd:209` e `app/docs/ARQUITETURA.mmd:218`. O gerador deriva esses dados de `config.crons` em `app/core/management/commands/gerar_diagrama.py:242`, `app/core/management/commands/gerar_diagrama.py:251` e `app/core/management/commands/gerar_diagrama.py:264`. Executei somente essa função, extraída por AST, com Python `-B`: ela gera 40 vigias e um juiz; o bloco não coincide com o arquivo. O selo exige igualdade em `app/core/tests/test_selo_diagrama_do_codigo.py:44`; o comparador está em `app/core/management/commands/gerar_diagrama.py:921`.

As demais dependências abertas não exigem manter o comando como juiz ou manter `--apply`:

- O contrato estrutural referencia a lista inteira, sem nomear esse comando: `app/core/contratos_estruturais.py:263`. O contador consulta o papel declarado: `app/config/crons.py:2322`. O contrato cobra consistência da lista e do papel: `app/core/tests/test_contract_tabuleiro.py:25`.
- O placar chama o comando sem `--apply`, raspa as linhas preservadas e mantém o retido: `bin/placar_code.sh:114`, `bin/placar_code.sh:117`, `bin/placar_code.sh:121`, `bin/placar_code.sh:549`. As strings continuam em `app/ponto/management/commands/fechar_cobranca_com_lastro.py:55` e `app/ponto/management/commands/fechar_cobranca_com_lastro.py:61`.
- O selo de host retira os formatos do próprio comando por AST e exige argv sem `--apply`: `bin/tests/test_placar_conta_o_retido_do_lastro.sh:75`, `bin/tests/test_placar_conta_o_retido_do_lastro.sh:594`, `bin/tests/test_placar_conta_o_retido_do_lastro.sh:618`. Os comentários “dry-run/fechar(True)” ficaram historicamente desatualizados em `bin/tests/test_placar_conta_o_retido_do_lastro.sh:591`.
- `crons_duracao.json` guarda somente a duração desse nome, 30 segundos: `app/config/crons_duracao.json:32`; a leitura utiliza o mapa `dur_s`, sem papel ou argumentos: `app/config/crons.py:154`.
- `core/placar_estrutural.py` referencia a lista como fonte, mas conserva prosa histórica com números anteriores: `app/core/placar_estrutural.py:535`, `app/core/placar_estrutural.py:629`.
- Comparação independente da árvore com a base a37c673d: [nao verificado]. A cópia não possui repositório Git disponível. No diff fornecido, os três blocos de `crons.py` correspondem à entrada, saída da lista e inclusão como vigia; seus destinos atuais são `app/config/crons.py:731`, `app/config/crons.py:2018` e `app/config/crons.py:2157`.

V10: a migração mantém mordida relevante, mas não preserva literalmente toda cobertura anterior:

- **BUG114, idade:** o caso de 30 dias sem lâmpada agora exige permanecer vivo e alarmar; abandonou expressamente a expectativa antiga de fecho nesse degrau. Isso corresponde ao escopo informado para esta obra, mas é mudança de contrato do teste: `app/ponto/tests/test_bug114_fechador_de_dois_dias.py:144`.
- **BUG114, negativos:** sem batida, marca longe, sem marco e apenas `horario_previsto` passaram a verificar somente `julgar`; não executam o consumidor nem verificam o estado após sua passagem: `app/ponto/tests/test_bug114_fechador_de_dois_dias.py:162`, `app/ponto/tests/test_bug114_fechador_de_dois_dias.py:171`, `app/ponto/tests/test_bug114_fechador_de_dois_dias.py:182`, `app/ponto/tests/test_bug114_fechador_de_dois_dias.py:193`. Assim, isoladamente, passam mesmo com um defeito no novo escritor — ele percorre a fila diretamente, sem utilizar `julgar`: `app/ponto/services/lastro.py:385`. Não demonstra defeito atual; demonstra perda de cobertura do caminho que escreve.
- **BUG114, retido/nota:** preserva pergunta viva real, classificação do retido, trilha de recusa e idempotência: `app/ponto/tests/test_bug114_fechador_de_dois_dias.py:242`, `app/ponto/tests/test_bug114_fechador_de_dois_dias.py:268`. A nota continua coberta pelo par “gate recusa → zero notas / fecha duas vezes → uma nota”: `app/ponto/tests/test_bug114_fechador_de_dois_dias.py:355`, `app/ponto/tests/test_bug114_fechador_de_dois_dias.py:365`.
- **BUG115:** conserva o caso marco satisfeito com furo vizinho, exigindo `out['lastro']==1` e chamado terminal; o marco apagado exige zero e chamado vivo: `app/ponto/tests/test_bug115_lampada_do_marco.py:185`, `app/ponto/tests/test_bug115_lampada_do_marco.py:207`. O recuo por batida de outro tipo agora cobre só classificação: `app/ponto/tests/test_bug115_lampada_do_marco.py:167`.
- **O231:** mantém duas medições deliberadamente discordantes e exige uma chamada a `julgar`; também compara `(a)=3` com `sem_evento=3`/rc2: `app/ponto/tests/test_o231_lastro_mede_uma_vez.py:54`, `app/ponto/tests/test_o231_lastro_mede_uma_vez.py:64`.
- **lastro_rele:** não virou teste vazio. O chamado cobra E07 e o fixture planta E07 acesa: `app/ponto/tests/test_lastro_rele_o_chamado.py:46`, `app/chamados/tests/_dia_regularizado.py:8`. O objeto obsoleto deve produzir zero/nenhum FU e o controle vivo deve produzir um/nota: `app/ponto/tests/test_lastro_rele_o_chamado.py:59`, `app/ponto/tests/test_lastro_rele_o_chamado.py:69`.
- **Cura nova revertida:** os positivos que exigem especificamente `lastro=1` continuam distinguindo a presença da chamada no consumidor, inclusive o teste novo A1: `app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:103` e `app/ponto/tests/test_bug115_lampada_do_marco.py:204`. Os negativos isolados naturalmente podem continuar passando sem o fecho; a suíte possui esses controles positivos nos mesmos caminhos.

Não executei suíte Django, selos de host nem mutações: resultados dinâmicos deles [nao verificado]. [nao verificado]

## 06/11 04:30 · sessao 6dbfe6056c73 · - · - · origem=Ronald (tmux)

1. **FILA — P3: falso positivo real de import embutido próprio.** Em `app/core/placar_estrutural.py:443`, o valor textual de `numero` contém a menção `from core import contratos_estruturais` (`app/core/placar_estrutural.py:461`) e, adiante no mesmo literal concatenado, `logs/placar_estrutural/...` (`app/core/placar_estrutural.py:485`). A alternativa permissiva de `bin/quem_chama.py:93` casa esse intervalo como importação de **`core.placar_estrutural`**, embora seja apenas texto no dicionário.

   Exercício em memória com o arquivo real produziu:
   ```
   (app/core/placar_estrutural.py, EMBUTIDO, 443,
    importa core.placar_estrutural em python embutido (string de codigo))
   ```
   O delta passa a aceitar esse achado próprio (`bin/quem_chama.py:738`), rebaixando as facetas originalmente varridas (`bin/quem_chama.py:753`). Isso pode esconder um zero verdadeiro, conservadoramente; **não elimina chamadores**: conserva os conjuntos existentes e acrescenta sítios (`bin/quem_chama.py:755`), imprimindo todos (`bin/quem_chama.py:773`).

2. **FILA — P5, antigo 5: posição incorreta permanece; não surgiu caminho novo para zero.** O cálculo usa quebras do valor decodificado (`bin/quem_chama.py:472`), enquanto o import real ocupa `app/core/tests/test_selo_diagrama_do_codigo.py:251`, dentro de literal iniciado em `app/core/tests/test_selo_diagrama_do_codigo.py:250`. Esse cálculo altera a linha do achado, mantendo sua emissão (`bin/quem_chama.py:472`).

3. **FILA — P5, antigo 6: escapes seguem existentes, mas os exemplos reais não demonstram zero.** O relativo do heredoc (`bin/regua_calendario.sh:30`) continua fora da regex (`bin/quem_chama.py:89`), porém o absoluto em `bin/regua_relogio.sh:35` permite rebaixar `config.settings.ci`. Os pacotes citados em `bin/alarme_sessao_ociosa.py:144` e `bin/alarme_sessao_ociosa.py:145` também possuem imports diretos reconhecíveis: `app/colaboradores/tests/test_s120.py:65` e `app/chamados/apps.py:10`, reconhecidos pelo ramo `bin/quem_chama.py:449`. Falso zero novo desses exemplos: **[nao verificado]**.

4. **FILA — P5, antigos 8 e 9: sem falso zero novo demonstrado.** As mudanças de exclusividade pertencem ao selo (`bin/tests/test_quem_chama.sh:219`, `bin/tests/test_quem_chama.sh:220`, `bin/tests/test_quem_chama.sh:234`, `bin/tests/test_quem_chama.sh:275`). A divergência de prosa continua conservadora no exemplo real: a string contendo `bin/gerar_diagrama.py` (`app/core/management/commands/gerar_diagrama.py:873`) é visitada por `bin/quem_chama.py:474`, enquanto o censo corta no primeiro `#` (`app/core/censo_codigo.py:60`).

Ranges históricos, execução integral do selo e confirmação do SHA via Git: **[nao verificado]**; esta cópia não contém repositório Git acessível **[nao verificado]**.

## 06/11 04:29 · sessao c166bb812a26 · - · - · origem=Ronald (tmux)

1. **BLOQUEIA — classificação ainda pode mascarar decorador opaco com registro do outro lado; defeito preexistente, não regressão exclusiva do delta.** A soma conserva `decor=['register.filter', 'receiver']` e `registros=[('filtro','old')]`, mas `_tipo_simbolo` retorna `filtro/tag` antes de examinar `receiver` (`bin/quem_chama.py:704`, `bin/quem_chama.py:707`, `bin/quem_chama.py:709`, `bin/quem_chama.py:348`, `bin/quem_chama.py:352`).

   Contraexemplo exercitado em memória: base `@register.filter(name='old') def f(x): return x`; ponta `@receiver('signal') def f(x): return x+1`. Resultado: `simbolo f [filtro/tag]: 0 chamador(es)`, embora a própria classificação trate `receiver` sem registro como motivo para não varrer (`bin/quem_chama.py:354`, `bin/quem_chama.py:765`). O chamador indireto é o despachante do registrador; ocorrência dessa transição no repo real: **[nao verificado]**. O mesmo caminho já existe no algoritmo anterior anexado, pois conservava o registro da base: **[nao verificado por execução da revisão antiga]**.

2. **P1 — casos pedidos são conservadores, exceto a interação acima.** `membro=True` em qualquer lado vence; registro `(papel,None)` junto de registro constante continua “nome não resolvido”; decorador removido permanece na união; um lado sozinho recebe `registros` normalmente (`bin/quem_chama.py:706`, `bin/quem_chama.py:349`, `bin/quem_chama.py:707`, `bin/quem_chama.py:705`, `bin/quem_chama.py:708`). Esses casos foram exercitados em memória.

   Campos restantes usam primeiro valor, geralmente da base: a cópia está no `setdefault` e os únicos campos posteriormente combinados são `membro`, `decor` e `registros` (`bin/quem_chama.py:700`, `bin/quem_chama.py:704`, `bin/quem_chama.py:706`). Contudo, o produtor atual sempre deriva `nome` do sufixo de `q`; mudar de topo `f` para membro `C.f` também muda a chave, impedindo esse conflito natural entre lados (`bin/quem_chama.py:290`, `bin/quem_chama.py:292`, `bin/quem_chama.py:300`). Divergência de `nome` só foi demonstrada com infos artificiais injetados na soma, não produzidos pelo parser (`bin/quem_chama.py:279`).

3. **P2 — migração singular/plural consistente no caminho operacional.**

   | Campo | Escritores | Leitores |
   |---|---|---|
   | `registro` | `_add`, `bin/quem_chama.py:282` | Somente soma, `bin/quem_chama.py:708`, `bin/quem_chama.py:709`; excluído da cópia em `bin/quem_chama.py:704`. |
   | `registros` | Inicialização em `bin/quem_chama.py:705`; acréscimo em `bin/quem_chama.py:709`. | Classificação em `bin/quem_chama.py:348`, `bin/quem_chama.py:349`; varredura de template em `bin/quem_chama.py:606`; deduplicação em `bin/quem_chama.py:708`. |

   O fluxo passa pela soma antes da classificação e da construção de `Alvos`, usados pelas varreduras; não encontrei leitor singular sobre dict somado nem leitor plural sobre dict cru nesse fluxo (`bin/quem_chama.py:701`, `bin/quem_chama.py:711`, `bin/quem_chama.py:716`, `bin/quem_chama.py:728`, `bin/quem_chama.py:732`).

## 06/11 04:19 · sessao 985df92221f6 · OTIMIZA-3 · verificador · origem=Code

**Parecer: bloqueia.** Há caminhos reproduzíveis para zero falso e uma lacuna no selo de facetas (`bin/quem_chama.py:701`, `bin/quem_chama.py:732`, `bin/tests/test_quem_chama.sh:303`). Execução dos ranges históricos e do selo completo: **[nao verificado]**. Os contraexemplos sintéticos abaixo foram exercitados em memória; ocorrência deles em produção: **[nao verificado]**.

1. **BLOQUEIA — P2/P7: metadado velho pode esconder decorador novo.** A coleta percorre base antes da ponta, conserva o primeiro `info` por `setdefault` e atualiza apenas `registro` (`bin/quem_chama.py:699`, `bin/quem_chama.py:701`, `bin/quem_chama.py:703`).

   Contraexemplo: base com `def f(): ...`; ponta acrescenta `@receiver(sinal)` ao mesmo `f`. Permanece `decor=[]`, e a classificação resulta em `simbolo-de-topo`, permitindo zero, em vez do motivo “decorado” (`bin/quem_chama.py:350`, `bin/quem_chama.py:705`, `bin/quem_chama.py:759`). O fixture mantém `@receiver` nos dois lados, portanto não cobre essa transição (`bin/tests/test_quem_chama.sh:117`, `bin/tests/test_quem_chama.sh:158`).

2. **BLOQUEIA — P7: renomear o registro de filtro conserva somente o nome velho.** Se ambos os lados têm `registro`, a união mantém o antigo (`bin/quem_chama.py:702`).

   Contraexemplo: `@register.filter(name="old")` passa para `name="new"`; template carrega a biblioteca e usa `{{ v|new }}`. O símbolo continua registrado internamente como `old`; a busca usa esse nome e retorna zero para o filtro realmente utilizado (`bin/quem_chama.py:605`, `bin/quem_chama.py:608`, `bin/quem_chama.py:612`). É o mesmo problema de precedência do lado velho, agora produzindo falso zero em `filtro/tag` (`bin/quem_chama.py:699`, `bin/quem_chama.py:759`).

3. **BLOQUEIA — P2/P7: Python embutido no próprio arquivo é descartado antes do rebaixamento.** O guarda `alvo_rel == rel` vem antes do tratamento de `EMBUTIDO` (`bin/quem_chama.py:732`, `bin/quem_chama.py:734`).

   Contraexemplo sintético em `app/m/selfmod.py`:

   ```python
   def f(): return 1
   exec("from m.selfmod import f; f()")
   ```

   `varrer_py` reconhece o import embutido; o guarda o elimina, e `varrer_proprio` não enxerga nomes dentro da string. A faceta `simbolo f [simbolo-de-topo]` termina com zero (`bin/quem_chama.py:469`, `bin/quem_chama.py:506`, `bin/quem_chama.py:732`). Ocorrência real dessa forma executável no próprio módulo: **[nao verificado]**.

4. **BLOQUEIA — P5: sítios podem desaparecer da faceta arquivo sem morder as asserções novas.** Mutante plausível em `bin/quem_chama.py:749`:

   ```python
   if q is not None:
       por[(a['rel'], q)] |= sitios
   ```

   No heredoc do fixture, ele mantém o símbolo rebaixado com `bin/heredoc.sh:3`, mas deixa a faceta arquivo sem esse sítio (`bin/tests/test_quem_chama.sh:127`). As quatro condições de r5 continuam satisfeitas: cabeçalho, sítio e ausência de zero são exigidos no símbolo; no arquivo exige-se somente o cabeçalho (`bin/tests/test_quem_chama.sh:300`, `bin/tests/test_quem_chama.sh:301`, `bin/tests/test_quem_chama.sh:302`, `bin/tests/test_quem_chama.sh:303`). Sobrevivência ao selo completo: **[nao verificado]**.

5. **FILA — P3: linha errada em string de `.py` com escapes/concatenação.** O cálculo usa quebras do **valor decodificado**, somadas à linha inicial do nó AST (`bin/quem_chama.py:470`).

   Contraexemplo real: o literal começa em `app/core/tests/test_selo_diagrama_do_codigo.py:250`, contendo dois `\n`; o import está fisicamente na linha **251**, mas é relatado como **252**, onde começa outro literal (`app/core/tests/test_selo_diagrama_do_codigo.py:251`, `app/core/tests/test_selo_diagrama_do_codigo.py:252`).

   Para heredoc e comentários intermediários de `.sh`, o cálculo preserva as posições: `_codigo_sh` conserva os números originais, e `varrer_linhas` repõe lacunas como linhas vazias (`bin/quem_chama.py:524`, `bin/quem_chama.py:528`, `bin/quem_chama.py:557`). O import real em `bin/esteira_status.sh:55` permanece na linha 55 por esse caminho (`bin/quem_chama.py:558`).

6. **FILA — P1: há escapes reais da regex, mas não demonstrei uma faceta com zero falso causada por eles.**

   - O heredoc gera um módulo com `from .ci import *`; a regex de `config.settings.ci` não resolve esse relativo (`bin/regua_calendario.sh:28`, `bin/regua_calendario.sh:30`, `bin/quem_chama.py:89`). Entretanto, há import absoluto do mesmo alvo em outro script, suficiente para provocar rebaixamento global (`bin/regua_relogio.sh:35`, `bin/quem_chama.py:741`). **Faceta que ainda sairia zero por esse escape: [nao verificado].**
   - A string executada importa `colaboradores.models` e `chamados.fcm_utils`; as regex dos módulos-pacote `colaboradores` e `chamados` não reconhecem esses `from pacote.submod import ...` (`bin/alarme_sessao_ociosa.py:142`, `bin/alarme_sessao_ociosa.py:144`, `bin/alarme_sessao_ociosa.py:145`, `bin/quem_chama.py:89`). **Zero das facetas arquivo dos respectivos `__init__.py` na árvore completa: [nao verificado].**

   Esses sítios demonstram perda de cobertura; não sustentam afirmar uma repetição real da reprovação de `datas_cravadas_prod` **[nao verificado]**.

7. **FILA — P4/P5: comportamento atual de comentário está coerente; falta mordida para `{% comment %}` multilinha.** A remoção de DOTALL de `{# #}` concorda com o contrato já existente no repo (`bin/quem_chama.py:569`, `app/holerite/tests/test_contract_lapide_nao_vaza.py:3`). `{% comment %}` multilinha continua removido e conserva as quebras de linha; há exemplo real delimitado em `app/templates/base.html:139` e `app/templates/base.html:152` (`bin/quem_chama.py:570`).

   Mutante: remover `flags=re.S` da segunda substituição. O novo r5 de comentário não o mata, pois exercita `{# #}`; o fixture de `{% comment %}` está inteiro numa linha (`bin/quem_chama.py:571`, `bin/tests/test_quem_chama.sh:67`, `bin/tests/test_quem_chama.sh:138`, `bin/tests/test_quem_chama.sh:222`). Sobrevivência ao selo completo: **[nao verificado]**.

   Falso positivo grave ou falso zero causado pelo novo regex de `{% filter %}` em template real: **[nao verificado]**; o caso acrescentado ao selo é positivo e sintético (`bin/quem_chama.py:608`, `bin/tests/test_quem_chama.sh:137`, `bin/tests/test_quem_chama.sh:262`).

8. **FILA — P5: alcance das demais asserções novas.**

   | Asserção/helper | Verificação e limite |
   |---|---|
   | `_fac` | Exige espaço ou `:` após o rótulo. `simbolo f_emb` não casa `simbolo f_emb_here`; o alvo também exige caminho seguido de ` (` (`bin/tests/test_quem_chama.sh:176`, `bin/tests/test_quem_chama.sh:179`). |
   | `_so_na_fac` | **Total zero não passa**: exige `tot >= 1`. Mover o achado de símbolo-parado/relativo-parado para outra faceta viola a igualdade entre total e ocorrências sob arquivo (`bin/tests/test_quem_chama.sh:186`, `bin/tests/test_quem_chama.sh:195`, `bin/tests/test_quem_chama.sh:202`). |
   | `r5-init-reexporta` e `r5-import-local` | Exigem presença correta, mas aceitam duplicação adicional sob arquivo. Mutante: emitir também com `q=None` o achado de importação do símbolo (`bin/quem_chama.py:453`, `bin/tests/test_quem_chama.sh:207`, `bin/tests/test_quem_chama.sh:208`). |
   | `r5-comentario-multilinha` e `r5-filter-tag` | Também são positivos sem exclusividade; duplicar o achado em outra faceta não contradiz essas asserções (`bin/tests/test_quem_chama.sh:222`, `bin/tests/test_quem_chama.sh:262`). |
   | `r5-embutido-*` e reais | Ausência total é morta pelos positivos vizinhos; o negativo sozinho seria vacuoso. A perda exclusiva do sítio sob arquivo sobrevive às condições novas, como no achado 4 (`bin/tests/test_quem_chama.sh:182`, `bin/tests/test_quem_chama.sh:300`, `bin/tests/test_quem_chama.sh:353`). |
   | `r5-solo-*` | Prendem o zero e o tipo nos dois cabeçalhos, mas não cobrem comentário com import absoluto direto: fixture usa `from m import solo` (`bin/tests/test_quem_chama.sh:133`, `bin/tests/test_quem_chama.sh:134`, `bin/tests/test_quem_chama.sh:306`). |

9. **FILA — P7: segunda implementação da regra código/prosa, com divergência concreta.** O instrumento declara seguir a regra da casa, mas implementa `_prosa` local; a autoridade existente declara centralização e já possui wrapper importador (`bin/quem_chama.py:21`, `bin/quem_chama.py:356`, `app/core/censo_codigo.py:15`, `bin/censo_codigo_py.py:34`).

   Contraexemplo real: a string de código em `app/core/management/commands/gerar_diagrama.py:873` cita `bin/gerar_diagrama.py` depois de `#`. O instrumento conta a citação, enquanto o censo a corta no primeiro `#`, inclusive dentro da string (`bin/quem_chama.py:472`, `app/core/censo_codigo.py:60`). Isso prova divergência entre derivadores; nesse caso, o instrumento é mais conservador e não produz falso zero (`bin/quem_chama.py:473`).

10. **P2/P6 — fluxo dos campos: sem falha adicional de ordem no rebaixamento.** Uma vez coletado o sítio, todas as facetas são percorridas; apenas tipos em `VARRIDOS` mudam, preservando os motivos de teste, setting e decorado (`bin/quem_chama.py:745`, `bin/quem_chama.py:746`, `bin/quem_chama.py:747`). Símbolos são classificados antes disso; alvo apagado conserva módulo e símbolos da base; `__init__.py` de pacote conserva módulo, enquanto `app/__init__.py` sem módulo cai fora da lista positiva (`bin/quem_chama.py:151`, `bin/quem_chama.py:317`, `bin/quem_chama.py:687`, `bin/quem_chama.py:699`, `bin/quem_chama.py:705`).

   | Campo | Escritores | Leitores |
   |---|---|---|
   | `alvo['tipo']` | Inicialização: `bin/quem_chama.py:707`; rebaixamento por alias `info`: `bin/quem_chama.py:748`. | Guarda por alias: `bin/quem_chama.py:747`; render: `bin/quem_chama.py:782`. |
   | `info['tipo']` | Classificação: `bin/quem_chama.py:705`; rebaixamento: `bin/quem_chama.py:748`. | Guarda: `bin/quem_chama.py:747`; render dos símbolos: `bin/quem_chama.py:784`. |
   | `alvo['embutido']` | `bin/quem_chama.py:745`. | `bin/quem_chama.py:781`; conteúdo formatado em `bin/quem_chama.py:756`. |
   | `por[(rel,q)]` | Inicialização: `bin/quem_chama.py:711`, `bin/quem_chama.py:714`; acréscimos: `bin/quem_chama.py:737`, `bin/quem_chama.py:740`, `bin/quem_chama.py:749`. | Acumulação nos mesmos sítios; ordenação: `bin/quem_chama.py:750`; render: `bin/quem_chama.py:782`, `bin/quem_chama.py:784`; total: `bin/quem_chama.py:817`. |
   | `EMBUTIDO` | Constante: `bin/quem_chama.py:81`. | Emissão do marcador: `bin/quem_chama.py:470`, `bin/quem_chama.py:558`; coleta: `bin/quem_chama.py:734`; tipo final: `bin/quem_chama.py:748`; apresentação: `bin/quem_chama.py:754`. |
   | `Alvos.emb` | Construção: `bin/quem_chama.py:400`. | Busca: `bin/quem_chama.py:410`, chamada por `bin/quem_chama.py:469` e `bin/quem_chama.py:559`. |

   A guarda de tipo e a acumulação de `por` leem valores antes da última atualização por necessidade do próprio algoritmo; a renderização lê depois do laço final. O defeito de precedência identificado está nos metadados antigos preservados pelos `setdefault`, conforme achados 1–2 (`bin/quem_chama.py:701`, `bin/quem_chama.py:747`, `bin/quem_chama.py:750`, `bin/quem_chama.py:782`).

## 06/11 04:16 · sessao 04a0a7bc1c6c · - · - · origem=Ronald (tmux)

1. **P3 — fila: número de linha errado em string concatenada de `.py`.** Em `app/core/tests/test_selo_diagrama_do_codigo.py:250`, o primeiro literal contém dois `\n` decodificados; o import de `ponto.janelas` está fisicamente na linha **251**. `varrer_py` soma esses dois caracteres a `no.lineno=250` e relata **252**, onde está `def g()` (`bin/quem_chama.py:470`; `app/core/tests/test_selo_diagrama_do_codigo.py:252`). Reproduzido em memória, sem arquivos auxiliares.

2. **P3 — cálculo preserva comentários intermediários em `.sh`.** `_codigo_sh` conserva as chaves com os números originais (`bin/quem_chama.py:524-529`); `varrer_linhas` repõe as linhas ausentes como vazias antes de contar `\n` (`bin/quem_chama.py:557-558`). O heredoc real de `bin/esteira_status.sh:55` foi relatado como linha **55**, apesar dos comentários anteriores (`bin/esteira_status.sh:45-51`).

3. **P1 — escape real, mas sem falso zero demonstrado.** `bin/regua_calendario.sh:28-35` gera `config/settings/ci_calendario.py` com `from .ci import *` na linha 30. `_rx_embutido('config.settings.ci', True)` não casa o relativo nem `config.settings.ci_calendario` da linha 45, pois `_` impede a fronteira final (`bin/quem_chama.py:89-95`). Contudo, `bin/regua_relogio.sh:35` importa `config.settings.ci` absolutamente e `bin/regua.sh:169` cita o módulo completo; esses outros sítios já provocam o rebaixamento global (`bin/quem_chama.py:723-733`). Faceta com falso zero por este escape: **[nao verificado]**.

4. **P1 — pacote pai também escapa; falso zero não demonstrado.** O subprocess de `bin/alarme_sessao_ociosa.py:142-149` executa `from colaboradores.models import Colaborador` e `from chamados.fcm_utils import push_fcm_colaborador`. As regex dos módulos-pacote `colaboradores` e `chamados` não casam essas strings, pois exigem `from pacote import` ou `import pacote` (`bin/quem_chama.py:89-95`). Entretanto, não foi demonstrada saída zero da faceta arquivo desses `__init__.py` na árvore completa: **[nao verificado]**.

## 06/11 04:15 · sessao d50b6974a0a4 · - · - · origem=Ronald (tmux)

1. **P5 — bloqueia: falta mordida para sítios sob a faceta arquivo.** O mutante `if q is not None: por[(a['rel'], q)] |= sitios` em `bin/quem_chama.py:749` remove os achados embutidos da faceta arquivo. Para o heredoc definido em `bin/tests/test_quem_chama.sh:127`, confirmei em memória: arquivo fica com `0 achado(s) avulso(s)` e perde `bin/heredoc.sh:3:`, enquanto todas as quatro condições de `bin/tests/test_quem_chama.sh:300`–`303` permanecem verdadeiras. O sítio só é exigido sob símbolo, em `bin/tests/test_quem_chama.sh:301`; arquivo exige apenas o cabeçalho em `bin/tests/test_quem_chama.sh:303`. Execução do selo completo: [nao verificado].

2. **P5 — sem defeito no prefixo citado.** `_fac` exige espaço ou `:` depois do rótulo, portanto `simbolo f_emb` não casa `simbolo f_emb_here`; `arquivo` não casa `arquivo_extra`. A seção exige o caminho seguido de ` (`. Confirmei os casos executando o awk em memória — `bin/tests/test_quem_chama.sh:176`–`180`. `_so_na_fac` também **não passa com total=0**: exige explicitamente `tot >= 1` — `bin/tests/test_quem_chama.sh:186`.

3. **P5 — fila: faceta correta não implica exclusividade em todos os r5.** `r5-init-reexporta`, `r5-import-local`, `r5-comentario-multilinha` e `r5-filter-tag` exigem presença sob a faceta correta, mas aceitam duplicação sob facetas erradas — `bin/tests/test_quem_chama.sh:207`, `208`, `222`, `262`. As duas reprovações específicas “símbolo parado” e “relativo parado” têm exclusividade efetiva por `_so_na_fac` — `bin/tests/test_quem_chama.sh:195`, `202`.

4. **P5 — fila: negativo sozinho é vacuoso, conjunto tem mordida.** `_naotem_fac` aceita faceta ausente — `bin/tests/test_quem_chama.sh:182`. Contudo, `r5-embutido-zero` vem acompanhado de cabeçalho e sítio positivos em `bin/tests/test_quem_chama.sh:300`–`302`; o real também em `bin/tests/test_quem_chama.sh:353`–`355`. Não apontar ausência total como mutante sobrevivente desses conjuntos.

5. **P5 — fila: teste novo de comentário não protege `{% comment %}` multilinha.** Remover `flags=re.S` de `bin/quem_chama.py:571` preserva o r5 de `{# #}` multilinha, que é outro mecanismo — `bin/tests/test_quem_chama.sh:138`, `222`. O fixture negativo de `{% comment %}` fica inteiro na mesma linha — `bin/tests/test_quem_chama.sh:67`. Sobrevivência ao selo histórico inteiro: [nao verificado].

6. **P7 — bloqueia: metadados antigos prevalecem e podem produzir zero falso.** A coleta percorre antiga antes de nova e usa `setdefault`, atualizando apenas `registro` — `bin/quem_chama.py:699`–`703`. Contraexemplo: base contém `def f(): ...`; ponta acrescenta `@receiver("s")` ao mesmo `f`. O `decor=[]` antigo permanece, e `_tipo_simbolo` classifica como `simbolo-de-topo`, permitindo `0 chamador(es)` embora o decorador registre a função — `bin/quem_chama.py:705`, `350`–`353`, `759`–`763`. O fixture atual mantém `@receiver` nos dois lados e não cobre a transição — `bin/tests/test_quem_chama.sh:117`, `158`.

_2113 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
