# CODEX-ESTADO

> Ultima resposta processada: **06/11/2026 04:44** · publicado em **06/11/2026 04:43** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

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

## 06/11 04:15 · sessao b6933d148bf2 · - · - · origem=Ronald (tmux)

1. **P4 — sem defeito real identificado [nao verificado quanto à execução Django].** Na cópia, não encontrei `{% filter %}` nem pares `{#...#}` multilinha nos 267 templates HTML examinados [nao verificado por referência de linha, resultado de busca]. A remoção de DOTALL concorda com o contrato local do lexer em `app/holerite/tests/test_contract_lapide_nao_vaza.py:3` e com seu caso de mordida em `app/holerite/tests/test_contract_lapide_nao_vaza.py:83`. O novo regex de filter está em `bin/quem_chama.py:608`; sem sítio real dessa sintaxe, falso positivo/falso zero provocado por ele no repo permanece [nao verificado].

2. **P4 — `{% comment %}` multilinha continua removido, preservando linhas.** O segundo `re.sub` mantém `flags=re.S` e devolve o mesmo número de quebras em `bin/quem_chama.py:570`. Exemplo real delimitado em `app/templates/base.html:139` e `app/templates/base.html:152`. Não confundir isso com `{#...#}` multilinha, que o contrato existente considera texto executável/renderizado (`app/holerite/tests/test_contract_lapide_nao_vaza.py:3`).

3. **P7 — FILA: derivação paralela da regra de código/prosa, com divergência concreta.** O instrumento declara seguir a regra da casa em `bin/quem_chama.py:21`, mas implementa `_prosa` local em `bin/quem_chama.py:356`; a regra existente declara centralização em `app/core/censo_codigo.py:15`, e o wrapper existente a importa em `bin/censo_codigo_py.py:34`. Contraexemplo real: a string de código em `app/core/management/commands/gerar_diagrama.py:873` contém `# MAPA ... bin/gerar_diagrama.py`. Em reprodução somente em memória, `varrer_py` contou a citação a `bin/gerar_diagrama.py` nessa linha (`bin/quem_chama.py:465`), enquanto `cita_no_codigo` não a contou: o censo corta no primeiro `#`, inclusive dentro da string (`app/core/censo_codigo.py:60`). Isso prova divergência; neste caso o instrumento novo é mais conservador, portanto não é prova de falso zero nele (`bin/quem_chama.py:465`; `app/core/censo_codigo.py:60`).

## 06/11 04:13 · sessao 67e1fb9bcf59 · LASTRO-PARTE-2 · censo · origem=Code

Execução efetiva desses caminhos, estado dos chamados, eventos pendentes e crontab instalado em produção: **[nao verificado]**. As referências abaixo são da cópia fornecida.

**1. Quem chama o comando e as funções de `lastro.py`**

| Alvo | Chamadores fora de `*/tests/*` |
|---|---|
| Command `fechar_cobranca_com_lastro` | Cron horário, minuto 40, com `--apply`: `app/config/crons.py:729`. Script do placar, sem `--apply`: `bin/placar_code.sh:116`, `bin/placar_code.sh:117`. |
| `fechar` | `Command.handle`: `app/ponto/management/commands/fechar_cobranca_com_lastro.py:53`. |
| `julgar` | Dentro de `fechar`: `app/ponto/services/lastro.py:327`; dentro de `contador`: `app/ponto/services/lastro.py:377`. |
| `contador` | Definição em `app/ponto/services/lastro.py:363`; nenhum chamador de produção localizado **[nao verificado quanto a chamadas externas/dinâmicas]**. O placar chama o command e raspa sua saída: `bin/placar_code.sh:116`, `bin/placar_code.sh:118`. |
| `marcos_acesos` | `marcos_acesos_do_dia`: `app/ponto/services/lastro.py:194`; coletor da premissa: `app/chamados/models.py:1249`; cartório, pelo alias `_acesos`: `app/ponto/services/cartorio.py:1408`. |
| `marcos_acesos_do_dia` | Detector, pelo alias `_acesos_do_dia`: `app/ponto/management/commands/detectar_ausencias.py:405`; entrada sincronizada: `app/ponto/services/entrada_sincronizada.py:29`. |
| `marcos_inexistentes` | Coletor da premissa: `app/chamados/models.py:1258`; cartório, pelo alias `_inexist`: `app/ponto/services/cartorio.py:1500`. |
| `marcos_apagados` | Worklist: `app/ponto/worklist.py:24`; contexto da reemissão: `app/chamados/reconciliador.py:695`; nomeador: `app/chamados/management/commands/nomear_marco_por_lampada.py:61`; command de resíduo: `app/ponto/management/commands/curar_residuo_bug115.py:81`; cartório: `app/ponto/services/cartorio.py:1878`. |
| `marcos_pendentes` | Juiz da pergunta viva: `app/chamados/juizes.py:1195`; predicado de absolvição pela célula: `app/chamados/reconciliador.py:1018`. |
| `_lampada` | Dentro de `marcos_inexistentes`: `app/ponto/services/lastro.py:115`; `marcos_acesos`: `app/ponto/services/lastro.py:175`; `julgar`: `app/ponto/services/lastro.py:289`. |

Outro command chamando nominalmente `fechar_cobranca_com_lastro` não foi localizado **[nao verificado quanto a chamadas externas/dinâmicas]**. A chamada do cron é construída genericamente por `linhas()`, usando `MANAGE`, comando, schema e argumentos: `app/config/crons.py:21`, `app/config/crons.py:1838`.

**2. Quem pode encerrar `ChamadoColaborador` de módulo `batida_ausente`**

A escrita persistida central é `_gravar_estado`: atribui `status_local` e salva. `reconciliar`, `transicionar`, `encerrar`, `encerrar_por_competencia_trancada` e `cancelar` chegam a essa escrita por suas portas. (`app/chamados/models.py:222`, `app/chamados/models.py:279`, `app/chamados/models.py:433`, `app/chamados/models.py:620`, `app/chamados/models.py:751`, `app/chamados/models.py:1549`, `app/chamados/models.py:1569`.)

| Sítio de fechamento | Gatilho e condição |
|---|---|
| `app/ponto/services/lastro.py:345` | Command de lastro; cron com `--apply`: `app/config/crons.py:729`. |
| `app/ponto/management/commands/detectar_ausencias.py:821` | `_fechar_por_afastamento`, acionado pela cobertura detectada em `app/ponto/management/commands/detectar_ausencias.py:351`; cron `*/5`: `app/config/crons.py:364`. |
| `app/ponto/management/commands/detectar_ausencias.py:831` | `_fechar_por_cobertura`, chamado em `app/ponto/management/commands/detectar_ausencias.py:381`; mesmo cron: `app/config/crons.py:364`. |
| `app/ponto/management/commands/detectar_ausencias.py:842` | Entrada encontrada e marcos cobrados acesos: `app/ponto/management/commands/detectar_ausencias.py:395`, `app/ponto/management/commands/detectar_ausencias.py:405`; mesmo cron: `app/config/crons.py:364`. |
| `app/ponto/services/cartorio.py:963` | Reconciliação dos adiados depois da lavra e do tratamento das perguntas: `app/ponto/services/cartorio.py:958`. |
| `app/ponto/services/cartorio.py:1488` | Chamados que não protestam e não foram adiados: `app/ponto/services/cartorio.py:1482`, `app/ponto/services/cartorio.py:1483`. |
| `app/ponto/services/cartorio.py:1501` | Marcos cobrados inexistentes na ata atual: `app/ponto/services/cartorio.py:1500`. |
| `app/ponto/services/cartorio.py:1522` | Códigos `COBRANCA_EM_DIA_SEM_TRABALHO` ou `COBRANCA_SEM_FURO`: `app/ponto/services/cartorio.py:1514`. |
| `app/ponto/services/cartorio.py:1600` | Fio sem pergunta viva, sem resposta textual e com todas as vias de duplicata: `app/ponto/services/cartorio.py:1594`, `app/ponto/services/cartorio.py:1599`. |
| `app/ponto/services/cartorio.py:1615` | Deduplicação dos demais chamados do dia pelo sobrevivente C8: `app/ponto/services/cartorio.py:1610`. |
| `app/ponto/services/entrada_sincronizada.py:31` | Entrada sincronizada pela API: `app/api/views.py:805`, `app/api/views_core.py:805`; exige `marcos_acesos_do_dia(...) is True`: `app/ponto/services/entrada_sincronizada.py:29`. |
| `app/ponto/services/triagem_batida.py:597` | Registro web chama `_autoresolve_f2_f4`: `app/ponto/services/triagem_batida.py:741`; entrada E seleciona `batida_ausente`: `app/ponto/services/triagem_batida.py:535`. |
| `app/ponto/services/cobertura.py:122` | Designação de cobertura pela view: `app/ponto/views.py:3048`; chamado de origem obtido por FK: `app/ponto/services/cobertura.py:95`. |
| `app/ponto/services/cobertura.py:317` | Declaração de cobertura passada; percorre chamados vivos do dia: `app/ponto/services/cobertura.py:246`, `app/ponto/services/cobertura.py:309`. |
| `app/chamados/services/materializacao.py:564` | Ausência já decidida e pergunta com destino: `app/chamados/services/materializacao.py:563`. |
| `app/chamados/services/materializacao.py:741` | Reconcilia destino e clones depois da materialização, com guarda de irmãs respondidas sem consumo: `app/chamados/services/materializacao.py:732`, `app/chamados/services/materializacao.py:744`, `app/chamados/services/materializacao.py:751`; chamadas do agrupador: `app/chamados/services/materializacao.py:944`, `app/chamados/services/materializacao.py:1029`, `app/chamados/services/materializacao.py:1049`. |
| `app/chamados/services/silencio_ausencia.py:101` | Ausência aprovada e premissa morta: `app/chamados/services/silencio_ausencia.py:93`; signals de save/delete agendam o consumidor: `app/ponto/signals.py:269`, `app/ponto/signals.py:275`, `app/ponto/signals.py:282`. |
| `app/chamados/services/feriado_reavaliar.py:47` | Universo explicitamente `batida_ausente`: `app/chamados/services/feriado_reavaliar.py:17`; evento de Feriado: `app/chamados/signals.py:269`, `app/chamados/signals.py:279`; regeneração de célula: `app/ponto/portas/celula.py:472`. |
| `app/chamados/services/adesao.py:115` | Agrupamento dos furos sob adesão: `app/chamados/services/adesao.py:102`; apuração diária chama a sincronização: `app/escala/management/commands/apurar_furos_diarios.py:17`. |
| `app/chamados/services/limbo_folgas.py:277` e `app/chamados/services/limbo_folgas.py:280` | Agrupamento sob LIMBO e eventual segunda tentativa após desfazer reata; chamado pelo detector e pela apuração: `app/ponto/management/commands/detectar_ausencias.py:761`, `app/chamados/services/limbo_folgas.py:325`, `app/escala/management/commands/apurar_furos_diarios.py:19`. |
| `app/ponto/services/veredito_celula.py:183` | `fechar_o_fio_do_dia`, chamado pelo veredito, declaração e validação: `app/ponto/services/veredito_celula.py:325`, `app/ponto/services/declarar_dia.py:204`, `app/chamados/services/validacao.py:271`. |
| `app/chamados/reconciliador.py:523` | Unidade `_retratar`; consumidor de eventos chama retratação por célula e por premissa: `app/chamados/reconciliador.py:1356`, `app/chamados/reconciliador.py:1360`; batida e morte de filho também acionam o reconciliador: `app/ponto/signals.py:215`, `app/chamados/signals.py:313`. |
| `app/ponto/management/commands/reconciliar_grade.py:309` → `_retratar` | Caminho com aplicação: `app/ponto/management/commands/reconciliar_grade.py:305`; agenda atual sem `--apply`: `app/config/crons.py:462`. |
| `app/chamados/management/commands/retratar_chamados_lei_cala.py:59` → `_retratar` | Command recebe módulo e exige `--apply`: `app/chamados/management/commands/retratar_chamados_lei_cala.py:34`, `app/chamados/management/commands/retratar_chamados_lei_cala.py:54`; declarado sob ordem: `app/config/crons.py:1371`. |
| `app/ponto/management/commands/curar_residuo_bug115.py:151` | Command de acervo, ramo “ata sem furo”, com `--apply`: `app/ponto/management/commands/curar_residuo_bug115.py:64`, `app/ponto/management/commands/curar_residuo_bug115.py:150`; fora da rotina: `app/config/crons.py:1429`. |
| `app/colaboradores/management/commands/marcar_feriado_do_posto.py:140` | Command com `--apply`; altera o posto e depois tenta reconciliar os chamados selecionados: `app/colaboradores/management/commands/marcar_feriado_do_posto.py:123`, `app/colaboradores/management/commands/marcar_feriado_do_posto.py:130`, `app/colaboradores/management/commands/marcar_feriado_do_posto.py:138`. |
| `app/ponto/services/justificativa.py:42` → `_retratar` | Aprovação da justificativa e materialização retroativa: `app/ponto/services/justificativa.py:202`, `app/ponto/services/justificativa.py:393`; lote da view: `app/ponto/views.py:1837`. |
| `app/chamados/reconciliador.py:496` → `encerrar()` | Cascata do ajuste retroativo: `app/ponto/services/justificativa.py:385`; queryset inclui `batida_ausente`: `app/ponto/services/justificativa.py:51`. |
| `app/colaboradores/services/desligamento.py:155` → `encerrar()` | Desligamento percorre todos os `NAO_FECHADOS` do colaborador: `app/colaboradores/services/desligamento.py:150`. |
| `app/chamados/services/competencia_trancada.py:184` | Tranca do período dispara a operação: `app/ponto/services/fechamento.py:1412`; também existe command: `app/chamados/management/commands/encerrar_por_competencia_trancada.py:20`. |

Os seis sítios do cartório são alcançáveis pelos signals de Batida, Ausência, Justificativa e criação/mudança de status de chamado, além de `processar_cartorio --apply` e `vigia_de_hora`. (`app/ponto/signals.py:131`, `app/ponto/signals.py:138`, `app/ponto/signals.py:143`, `app/ponto/signals.py:150`, `app/ponto/signals.py:161`, `app/chamados/signals.py:164`, `app/chamados/signals.py:180`, `app/config/crons.py:616`, `app/config/crons.py:367`, `app/ponto/services/vigia_de_hora.py:131`.)

Há também portas genéricas cujo alcance concreto depende dos dados:

| Sítio | Alcance condicionado |
|---|---|
| `app/chamados/signals.py:42` | Fechamento da disputa reconcilia somente sua FK `chamado`, sem filtro de módulo: `app/chamados/signals.py:28`, `app/chamados/signals.py:32`. FK concreta para `batida_ausente`: **[nao verificado]**. |
| `app/chamados/services/regularizacao_ext.py:205` | Encerra `reg.chamado` depois da materialização confirmada: `app/chamados/services/regularizacao_ext.py:51`, `app/chamados/services/regularizacao_ext.py:201`. FK concreta: **[nao verificado]**. |
| `app/chamados/management/commands/auditar_chamados_orfaos.py:67` | `--fechar`, categoria `disputa_supervisao`, sem filtro de módulo: `app/chamados/management/commands/auditar_chamados_orfaos.py:35`, `app/chamados/management/commands/auditar_chamados_orfaos.py:58`. Combinação com `batida_ausente`: **[nao verificado]**. |
| `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:57` | Cancela com `--apply`, filtrando subcategoria `vinculo_divergente`: `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:18`, `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:42`, `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:52`. Combinação com esse módulo: **[nao verificado]**. |
| `app/chamados/services/acoes_chamado.py:315` | Gestor envia `novo_status` pelo POST; dispatcher: `app/chamados/views.py:547`. |
| `app/chamados/services/fio.py:717` | Resposta rápida do DP, chamado por PK/colaborador: `app/chamados/services/fio.py:707`. |
| `app/chamados/services/acoes_chamado.py:70` | Aprovar exceção; dispatcher: `app/chamados/views.py:427`. |
| `app/chamados/services/acoes_chamado.py:222` e `app/chamados/services/acoes_chamado.py:277` | Autorizar/recusar geofence; dispatchers: `app/chamados/views.py:541`, `app/chamados/views.py:544`. Alcance concreto a `batida_ausente`: **[nao verificado]**. |
| `app/chamados/services/acoes_chamado.py:108` e `app/chamados/services/acoes_chamado.py:149` | Exigem categorias `agenda_aceite`/`escala_aceite`: `app/chamados/services/acoes_chamado.py:88`, `app/chamados/services/acoes_chamado.py:130`. Combinação com esse módulo: **[nao verificado]**. |
| `app/colaboradores/views.py:2068` | Aprovação/rejeição de benefício usa `sol.chamado_origem`: `app/colaboradores/views.py:2066`. FK concreta: **[nao verificado]**. |

A chamada real de `ChamadoColaborador.fechar()` é `fechar_chamados_meio_periodo`, cujo universo declarado contém apenas `intervalo_pendente` e `batida_fora_escala_leve`. (`app/ponto/management/commands/fechar_chamados_meio_periodo.py:109`, `app/ponto/management/commands/fechar_chamados_meio_periodo.py:30`.)

As outras chamadas a `reconciliar()` selecionam módulos/pais distintos: LIMBO/adesão, `ausencia_colab`, intervalo e cluster. (`app/ponto/management/commands/detectar_ausencias.py:143`, `app/ponto/management/commands/detectar_ausencias.py:158`, `app/chamados/services/adesao.py:142`, `app/chamados/services/adesao.py:150`, `app/colaboradores/services/pos_vinculo.py:42`, `app/chamados/services/limbo_folgas.py:98`, `app/chamados/services/limbo_folgas.py:101`, `app/ponto/services/ausencia.py:678`, `app/ponto/services/ausencia.py:693`, `app/ponto/management/commands/detectar_intervalo_ausente.py:124`, `app/ponto/management/commands/detectar_intervalo_ausente.py:138`, `app/ponto/services/cluster_espurio.py:124`.)

Não foi localizado `.update(status_local=...)` encerrando chamados fora da escrita central **[nao verificado quanto a sítios dinâmicos adicionais]**. As atribuições de `status_local` no julgamento do pós-estado são temporárias em memória; o `.update()` de rebaixamento altera urgência. (`app/chamados/models.py:339`, `app/chamados/models.py:354`, `app/chamados/models.py:602`.)

**3. Estados lidos pelos gates, escritores e sequência posterior**

`_decisao_do_fechamento` relê o estado persistido, consulta o motor, passa pela premissa e depois pela pergunta viva. `reconciliaria()` usa a mesma decisão sem gravar o fechamento. (`app/chamados/models.py:627`, `app/chamados/models.py:638`, `app/chamados/models.py:639`, `app/chamados/models.py:648`, `app/chamados/models.py:651`.)

| Estado/campo lido | Escritor e o que roda depois |
|---|---|
| `status_local` do chamado, do pai agrupador e dos possíveis sobreviventes | `_gravar_estado` atribui e salva: `app/chamados/models.py:222`, `app/chamados/models.py:279`. Criação ou mudança de status dispara julgamento da célula: `app/chamados/signals.py:165`, `app/chamados/signals.py:169`, `app/chamados/signals.py:180`. |
| `modulo_origem`, `categoria`, `subcategoria` | Identificam chamado de furo: `app/chamados/models.py:1205`. Nascem pelas portas `abrir`/`criar`: `app/chamados/models.py:932`, `app/chamados/models.py:1056`. Nascimento de módulo de turno com célula emite evento: `app/chamados/models.py:939`, `app/chamados/models.py:941`, `app/chamados/models.py:1058`, `app/chamados/models.py:1060`. |
| Dia forte: `celula.data`, datas do catálogo/contexto | Leitura: `app/chamados/juizes.py:91`, `app/chamados/juizes.py:107`, `app/chamados/juizes.py:111`, `app/chamados/juizes.py:120`. FK escrita por `apontar_celula`: `app/chamados/services/vinculos.py:92`, `app/chamados/services/vinculos.py:98`. Contexto salvo pelas portas `anotar`, `renovar`, `retirar`, `fundir`: `app/chamados/services/contexto.py:76`, `app/chamados/services/contexto.py:101`, `app/chamados/services/contexto.py:123`, `app/chamados/services/contexto.py:158`. Save sem mudança de status é ignorado pelo receiver do cartório: `app/chamados/signals.py:169`. |
| `justificativa_relacionada_id`, `Justificativa.status` | Leitura da aprovação: `app/chamados/models.py:1227`; FK escrita em `app/chamados/services/vinculos.py:81`, `app/chamados/services/vinculos.py:87`. Aprovação individual, retroativa e em lote: `app/ponto/services/justificativa.py:190`, `app/ponto/services/justificativa.py:368`, `app/ponto/views.py:1796`. Depois, os três chamam encerramento da origem: `app/ponto/services/justificativa.py:202`, `app/ponto/services/justificativa.py:393`, `app/ponto/views.py:1837`; essa porta chega a `_retratar`: `app/ponto/services/justificativa.py:38`, `app/ponto/services/justificativa.py:42`. O save também julga o dia pelo signal: `app/ponto/signals.py:162`, `app/ponto/signals.py:165`. |
| `CelulaDia.veredito`, `veredito_via`, `ata` | Premissa aceita decisão administrativa e consulta os marcos: `app/chamados/models.py:1243`, `app/chamados/models.py:1249`, `app/chamados/models.py:1258`. A porta grava esses campos e chama `salvar_e_emitir`: `app/ponto/portas/celula.py:810`, `app/ponto/portas/celula.py:812`, `app/ponto/portas/celula.py:813`, `app/ponto/portas/celula.py:817`. Alteração de `veredito` ou `ata` emite `EventoCelula`: `app/ponto/portas/celula.py:663`, `app/ponto/portas/celula.py:680`, `app/ponto/portas/celula.py:684`. |
| `ata.tipo_dia`, `ata.lampadas.{hora,tipo,acesa}`, `contexto_json.marcos_faltantes` | Leitura por dia sem trabalho/marcos: `app/chamados/juizes.py:413`, `app/ponto/services/lastro.py:75`, `app/ponto/services/lastro.py:89`, `app/ponto/services/lastro.py:179`. Ata persistida passa pela lavra acima; contexto passa pelas portas de contexto citadas. Outra escrita de ata, precedência, também emite: `app/ponto/portas/celula.py:862`, `app/ponto/portas/celula.py:864`. |
| `CelulaDia.trabalha`, `dna` | Escritas da regeneração: `app/ponto/portas/celula.py:237`, `app/ponto/portas/celula.py:238`, `app/ponto/portas/celula.py:266`, `app/ponto/portas/celula.py:270`. Depois há julgamento dos dias, rejunção e reavaliação de cobranças espúrias: `app/ponto/portas/celula.py:393`, `app/ponto/portas/celula.py:402`, `app/ponto/portas/celula.py:461`, `app/ponto/portas/celula.py:469`, `app/ponto/portas/celula.py:472`, `app/ponto/portas/celula.py:473`. |
| Reversão/geração de `trabalha` e `dna` | A reversão salva e retorna sem os ganchos anteriores no corpo: `app/ponto/portas/celula.py:570`, `app/ponto/portas/celula.py:578`, `app/ponto/portas/celula.py:592`. A geração de período também salva e retorna: `app/ponto/portas/celula.py:644`, `app/ponto/portas/celula.py:645`, `app/ponto/portas/celula.py:652`, `app/ponto/portas/celula.py:657`. Reconciliação posterior por um chamador externo dessas operações: **[nao verificado]**. |
| Ausência que cobre o dia inteiro, situação/período da ausência | Leitura: `app/chamados/juizes.py:448`, `app/chamados/juizes.py:449`. Criação, edição e decisão: `app/ponto/services/ausencia.py:418`, `app/ponto/services/ausencia.py:508`, `app/ponto/services/ausencia.py:808`, `app/ponto/services/ausencia.py:819`, `app/ponto/services/ausencia.py:911`. Save/delete julga o dia e agenda consumidor de ausência: `app/ponto/signals.py:143`, `app/ponto/signals.py:150`, `app/ponto/signals.py:269`, `app/ponto/signals.py:275`, `app/ponto/signals.py:282`; esse consumidor pode reconciliar o `batida_ausente`: `app/chamados/services/silencio_ausencia.py:101`. |
| `ata.orfas`/`n_orfas`, confiança da escala | Leitura: `app/chamados/juizes.py:482`, `app/chamados/juizes.py:488`. Ata segue a lavra/evento acima. Confiança é persistida via `gravar_regua`, chamada pela avaliação/corroboracão: `app/escala/regua_defesa.py:662`, `app/escala/regua_defesa.py:719`, `app/escala/regua_defesa.py:765`. Desfazer corroboração pode rejulgar furos: `app/escala/regua_defesa.py:725`, `app/escala/regua_defesa.py:692`; avaliação simples termina retornando o resultado: `app/escala/regua_defesa.py:770`. |
| Feriado/folga no

[truncado em 20000 de 44275 caracteres]

## 06/11 04:08 · sessao 49b429ce1ed7 · - · - · origem=Ronald (tmux)

Itens 2 e 4 — censo estático. Execução efetiva em produção, cadastros/FKs concretos e frequência observada: [não verificado].

**2. Portas e sítios de encerramento**

A escrita persistida do estado passa por `_gravar_estado`: atribuição em `app/chamados/models.py:222`, gravação em `app/chamados/models.py:279`. `reconciliar()` chama a decisão e grava em `app/chamados/models.py:617` e `app/chamados/models.py:620`; `fechar()` delega a `transicionar()` em `app/chamados/models.py:437`; `encerrar()` grava fechado em `app/chamados/models.py:1549`; a variante por competência grava em `app/chamados/models.py:1569`; `cancelar()` grava o terminal escolhido pelo motor em `app/chamados/models.py:751`.

| Sítio que pode atingir `batida_ausente` | Gatilho/caminho demonstrado |
|---|---|
| `app/ponto/services/lastro.py:345` | Management command/cron; agenda `--apply` em `app/config/crons.py:729`. |
| `app/ponto/management/commands/detectar_ausencias.py:821` | `_fechar_por_afastamento`, chamado após detectar cobertura em `app/ponto/management/commands/detectar_ausencias.py:351`; cron `*/5` em `app/config/crons.py:364`. |
| `app/ponto/management/commands/detectar_ausencias.py:831` | `_fechar_por_cobertura`, chamado em `app/ponto/management/commands/detectar_ausencias.py:381`; mesmo cron `app/config/crons.py:364`. |
| `app/ponto/management/commands/detectar_ausencias.py:842` | `_fechar_chamado` por entrada encontrada; definição em `app/ponto/management/commands/detectar_ausencias.py:839`; mesmo cron `app/config/crons.py:364`. |
| `app/ponto/services/cartorio.py:963` | Após lavra/E2 dos adiados; ordem explicitada e executada em `app/ponto/services/cartorio.py:958`. |
| `app/ponto/services/cartorio.py:1488` | Chamados que não protestam, sem adiamento; seleção em `app/ponto/services/cartorio.py:1482`. |
| `app/ponto/services/cartorio.py:1501` | Marcos cobrados inexistentes na ata; filtro em `app/ponto/services/cartorio.py:1500`. |
| `app/ponto/services/cartorio.py:1522` | Códigos `COBRANCA_EM_DIA_SEM_TRABALHO` ou `COBRANCA_SEM_FURO`; condição em `app/ponto/services/cartorio.py:1514`. |
| `app/ponto/services/cartorio.py:1600` | Fio sem pergunta viva, sem resposta textual, todas as vias duplicata; condições em `app/ponto/services/cartorio.py:1594` e `app/ponto/services/cartorio.py:1599`. |
| `app/ponto/services/cartorio.py:1615` | Outros chamados do dia, deduplicados pelo sobrevivente C8; seleção em `app/ponto/services/cartorio.py:1610`. |
| `app/ponto/services/entrada_sincronizada.py:31` | Entrada sincronizada da API: `app/api/views.py:805` e `app/api/views_core.py:805`; exige `marcos_acesos_do_dia is True` em `app/ponto/services/entrada_sincronizada.py:29`. |
| `app/ponto/services/triagem_batida.py:597` | Registro web de batida chama `_autoresolve_f2_f4` em `app/ponto/services/triagem_batida.py:741`; tipo E seleciona `batida_ausente` em `app/ponto/services/triagem_batida.py:535`. |
| `app/ponto/services/cobertura.py:122` | Designação de cobertura via view `app/ponto/views.py:3048`; FK de origem buscada sem filtro de módulo em `app/ponto/services/cobertura.py:95`. |
| `app/ponto/services/cobertura.py:317` | Declaração de cobertura passada, função em `app/ponto/services/cobertura.py:246`; percorre vivos do dia em `app/ponto/services/cobertura.py:309`. |
| `app/chamados/services/materializacao.py:564` | Materialização de ausência decidida; exige destino e ausência fora de `ABERTAS` em `app/chamados/services/materializacao.py:563`. |
| `app/chamados/services/materializacao.py:741` | `_fecha` usado para destino em `app/chamados/services/materializacao.py:744` e clones em `app/chamados/services/materializacao.py:751`; materialização de respostas chama o agrupador em `app/chamados/services/materializacao.py:944`, `app/chamados/services/materializacao.py:1029` e `app/chamados/services/materializacao.py:1049`. |
| `app/chamados/services/silencio_ausencia.py:101` | Ausência aprovada e premissa morta; condição em `app/chamados/services/silencio_ausencia.py:93`; signal save/delete de ausência em `app/ponto/signals.py:275` e `app/ponto/signals.py:282`, callback chama reconciliador em `app/ponto/signals.py:269`. O cron agendado é `--alarme`: `app/config/crons.py:396`. |
| `app/chamados/services/feriado_reavaliar.py:47` | Universo explicitamente `batida_ausente` em `app/chamados/services/feriado_reavaliar.py:17`; signal de Feriado em `app/chamados/signals.py:269`, callback em `app/chamados/signals.py:279`; regeneração chama `reavaliar_dias` em `app/ponto/portas/celula.py:472`. Cron é `--alarme` em `app/config/crons.py:608`. |
| `app/chamados/services/adesao.py:115` | Agrupamento sob adesão; percorre módulos de furo em `app/chamados/services/adesao.py:102`; apuração diária chama sincronização em `app/escala/management/commands/apurar_furos_diarios.py:17`. |
| `app/chamados/services/limbo_folgas.py:277` e `app/chamados/services/limbo_folgas.py:280` | Agrupamento sob LIMBO, com nova tentativa após rollback da reata; chamado pelo detectar em `app/ponto/management/commands/detectar_ausencias.py:761` e pelo lote em `app/chamados/services/limbo_folgas.py:325`; apuração diária chama esse lote em `app/escala/management/commands/apurar_furos_diarios.py:19`. |
| `app/ponto/services/veredito_celula.py:183` | `fechar_o_fio_do_dia`; chamado por veredito em `app/ponto/services/veredito_celula.py:325`, declaração em `app/ponto/services/declarar_dia.py:204` e validação em `app/chamados/services/validacao.py:271`. |
| `app/chamados/reconciliador.py:523` | Unidade `_retratar`, acessível por wrapper em `app/chamados/reconciliador.py:546`; consumidor chama retratação por célula em `app/chamados/reconciliador.py:1356` e por premissa em `app/chamados/reconciliador.py:1360`; signal da batida chama o reconciliador em `app/ponto/signals.py:215`; morte de filho também em `app/chamados/signals.py:313`. |
| `app/ponto/management/commands/reconciliar_grade.py:309` → `_retratar` | Caminho manual `apply` em `app/ponto/management/commands/reconciliar_grade.py:305`; agenda atual sem `--apply` em `app/config/crons.py:462`. |
| `app/chamados/management/commands/retratar_chamados_lei_cala.py:59` → `_retratar` | Command com módulo informado em `app/chamados/management/commands/retratar_chamados_lei_cala.py:34`, exige `--apply` em `app/chamados/management/commands/retratar_chamados_lei_cala.py:54`; classificado sob ordem em `app/config/crons.py:1371`. |
| `app/ponto/management/commands/curar_residuo_bug115.py:151` | Command de acervo, classe ata sem furo, método em `app/ponto/management/commands/curar_residuo_bug115.py:150`; `--apply` declarado em `app/ponto/management/commands/curar_residuo_bug115.py:64`; fora de rotina em `app/config/crons.py:1429`. |
| `app/colaboradores/management/commands/marcar_feriado_do_posto.py:140` | Command com `--apply`, retorno dry em `app/colaboradores/management/commands/marcar_feriado_do_posto.py:123`; altera posto e depois tenta os chamados em `app/colaboradores/management/commands/marcar_feriado_do_posto.py:130` e `app/colaboradores/management/commands/marcar_feriado_do_posto.py:138`. |
| `app/ponto/services/justificativa.py:42` → `_retratar` | Aprovação da justificativa chama em `app/ponto/services/justificativa.py:202`; materialização retroativa também em `app/ponto/services/justificativa.py:393`; usa `chamado_origem` em `app/ponto/services/justificativa.py:38`. |
| `app/chamados/reconciliador.py:496` → `encerrar()` | Cascata do ajuste retroativo chama `encerrar_com_retencao` em `app/ponto/services/justificativa.py:385`; queryset inclui explicitamente `batida_ausente` em `app/ponto/services/justificativa.py:51`. |
| `app/colaboradores/services/desligamento.py:155` → `encerrar()` | Cascata do desligamento percorre todos `NAO_FECHADOS` do colaborador em `app/colaboradores/services/desligamento.py:150`. |
| `app/chamados/services/competencia_trancada.py:184` | Trancar período dispara em `app/ponto/services/fechamento.py:1412`; command importa executor em `app/chamados/management/commands/encerrar_por_competencia_trancada.py:20`. |
| `app/chamados/services/regularizacao_ext.py:205` → `encerrar()` | Materialização confirmada da regularização, condição em `app/chamados/services/regularizacao_ext.py:51`; FK `reg.chamado` em `app/chamados/services/regularizacao_ext.py:201`. Vínculo concreto a `batida_ausente`: [não verificado]. |
| `app/chamados/signals.py:42` | `post_save` da disputa fechada, FK `instance.chamado`; guards em `app/chamados/signals.py:28` e `app/chamados/signals.py:32`. Módulo não filtrado ali; vínculo concreto a `batida_ausente`: [não verificado]. |
| `app/chamados/management/commands/auditar_chamados_orfaos.py:67` | `--fechar`, filtro categoria `disputa_supervisao` em `app/chamados/management/commands/auditar_chamados_orfaos.py:35`; sem filtro de módulo. Existência de `batida_ausente` com essa categoria: [não verificado]. |
| `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:57` → `cancelar()` | Manual `--apply` em `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:52`; filtra só subcategoria `vinculo_divergente` em `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:18` e `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:42`. Existência com módulo `batida_ausente`: [não verificado]. |

Os sítios do cartório acima são alcançados por `post_save` de Batida, Ausência e Justificativa (`app/ponto/signals.py:131`, `app/ponto/signals.py:143`, `app/ponto/signals.py:161`), `post_delete` de Ausência (`app/ponto/signals.py:150`), criação/mudança de status de chamado (`app/chamados/signals.py:164` e `app/chamados/signals.py:180`), cron `processar_cartorio --apply` (`app/config/crons.py:616`) e vigia de hora (`app/config/crons.py:367` → `app/ponto/services/vigia_de_hora.py:131`).

**Transições humanas/genéricas**

- Dropdown/resposta do gestor: `app/chamados/services/acoes_chamado.py:315`; recebe `novo_status` do POST em `app/chamados/services/acoes_chamado.py:287`; dispatcher em `app/chamados/views.py:547`.
- Resposta rápida DP no fio: `app/chamados/services/fio.py:717`, busca chamado por PK e colaborador em `app/chamados/services/fio.py:707`.
- Aprovar exceção: `app/chamados/services/acoes_chamado.py:70`; dispatcher em `app/chamados/views.py:427`.
- Autorizar/recusar geofence: `app/chamados/services/acoes_chamado.py:222` e `app/chamados/services/acoes_chamado.py:277`; dispatchers em `app/chamados/views.py:541` e `app/chamados/views.py:544`. Alcance concreto a `batida_ausente` por esses handlers: [não verificado].
- Benefício avaliado: `app/colaboradores/views.py:2068`, usa FK `sol.chamado_origem` em `app/colaboradores/views.py:2066`. Vínculo concreto a `batida_ausente`: [não verificado].
- Resposta agenda/escala também transiciona em `app/chamados/services/acoes_chamado.py:108` e `app/chamados/services/acoes_chamado.py:149`, mas exige categoria `agenda_aceite`/`escala_aceite` em `app/chamados/services/acoes_chamado.py:88` e `app/chamados/services/acoes_chamado.py:130`. Existência com módulo `batida_ausente`: [não verificado].

**Chamadas encontradas que têm universo de outro módulo**

- `detectar_ausencias.py:158` é LIMBO `sem_previsao_escala`, seleção imediatamente anterior em `app/ponto/management/commands/detectar_ausencias.py:143`.
- `app/chamados/services/adesao.py:150` é pai `sem_previsao_escala/nunca_bateu`, filtro em `app/chamados/services/adesao.py:142`.
- `app/colaboradores/services/pos_vinculo.py:42` é fechamento do pai de adesão, propósito em `app/colaboradores/services/pos_vinculo.py:29`.
- `app/chamados/services/limbo_folgas.py:101` só `sem_previsao_escala/folga_nao_definida`, filtro em `app/chamados/services/limbo_folgas.py:98`.
- `app/ponto/services/ausencia.py:678` é chamado `ausencia_colab`, universo em `app/ponto/services/ausencia.py:693`.
- `app/ponto/management/commands/detectar_intervalo_ausente.py:138` filtra `MODULO_ORIGEM` em `app/ponto/management/commands/detectar_intervalo_ausente.py:124`.
- `app/ponto/management/commands/fechar_chamados_meio_periodo.py:109` é o caller real de `Chamado.fechar`, mas seus módulos são somente `intervalo_pendente` e `batida_fora_escala_leve` em `app/ponto/management/commands/fechar_chamados_meio_periodo.py:30`.
- `app/colaboradores/services/vinculo.py:387` encerra categoria `escala_aceite`, filtro em `app/colaboradores/services/vinculo.py:384`.
- `app/escala/management/commands/fechar_avisos_regua_na_lista.py:47` cancela só módulo `regua_suspeita`, filtro em `app/escala/management/commands/fechar_avisos_regua_na_lista.py:34`.
- `smoke_chamados` transiciona em `app/chamados/management/commands/smoke_chamados.py:134`, `app/chamados/management/commands/smoke_chamados.py:147` e `app/chamados/management/commands/smoke_chamados.py:163`, mas cria módulo `smoke` em `app/chamados/management/commands/smoke_chamados.py:126` e força rollback em `app/chamados/management/commands/smoke_chamados.py:43`.

Não encontrei `.update(status_local=...)` encerrando chamados nem atribuição literal persistente `status_local='fechado'/'resolvido'` fora das portas centrais; ausência de sítios adicionais em código dinâmico: [não verificado]. O `.update()` de `app/chamados/models.py:602` é rebaixamento de urgência, não mudança de status; método declarado em `app/chamados/models.py:584`.

**4. EventoCelula**

O INSERT concreto é único: `EventoCelula.objects.create(...)` em `app/ponto/portas/celula.py:673`.

| Chamador do emissor | Condição/origem |
|---|---|
| `app/ponto/portas/celula.py:684` | `salvar_e_emitir`: save em `app/ponto/portas/celula.py:682`; campos que acordam são `veredito` e `ata` em `app/ponto/portas/celula.py:663`. |
| `app/chamados/models.py:941` | Nascimento pela porta `abrir`; não confundir com renascimento. |
| `app/chamados/models.py:1060` | Nascimento pela porta `criar`. |
| `app/ponto/nucleo.py:93` | Recompute fechou turno aberto; calcula diferença em `app/ponto/nucleo.py:64` e chama emissor em `app/ponto/nucleo.py:66`; só emite para células existentes, query em `app/ponto/nucleo.py:92`. |
| `app/chamados/reconciliador.py:1286` | Semeadura dos chamados vivos com célula, sem pendente já existente; guards em `app/chamados/reconciliador.py:1283`. |
| `app/chamados/reconciliador.py:1299` | Semeadura das órfãs elegíveis, guards em `app/chamados/reconciliador.py:1292` e `app/chamados/reconciliador.py:1297`. |

`salvar_e_emitir` é chamado em `app/ponto/portas/celula.py:785`, `app/ponto/portas/celula.py:802`, `app/ponto/portas/celula.py:817`, `app/ponto/portas/celula.py:849` e `app/ponto/portas/celula.py:864`.

Consumidor: `app/chamados/reconciliador.py:1304`, invocado pelo command em `app/chamados/management/commands/consumir_eventos_celula.py:31`, agendado a cada minuto em `app/config/crons.py:394`. `--semear` chama a semeadura e retorna, sem consumo, em `app/chamados/management/commands/consumir_eventos_celula.py:28`.

A ordem por colaborador é rejuntar células (`app/chamados/reconciliador.py:1352`), retratar perguntas absolvidas (`app/chamados/reconciliador.py:1353`), retratar chamados absolvidos (`app/chamados/reconciliador.py:1356`), retratar premissa morta (`app/chamados/reconciliador.py:1360`) e encerrar órfãs pela lâmpada (`app/chamados/reconciliador.py:1363`). Falha aumenta tentativas/erro em `app/chamados/reconciliador.py:1374`; teto temporal adia em `app/chamados/reconciliador.py:1375`; sucesso marca `processado_em` em `app/chamados/reconciliador.py:1380`; retenção apaga processados antigos em `app/chamados/reconciliador.py:1387`.

O cron `reconciliar_chamados` está agendado com `--alarme`, não como dreno, em `app/config/crons.py:395`.

## 06/11 04:08 · sessao c1dc1b789861 · - · - · origem=Ronald (tmux)

Item 3 — matriz dos gates e das escritas, com alcance estático. Execução efetiva na produção, colaboradores atingidos e eventos pendentes: [não verificado].

**Decisão compartilhada**

`reconciliar()` e `reconciliaria()` consultam `_decisao_do_fechamento`; esta relê `status_local`, pergunta ao motor, passa primeiro pela premissa e depois pela pergunta viva. Só `reconciliar()` grava o destino, por `_gravar_estado` (`app/chamados/models.py:607`, `app/chamados/models.py:617`, `app/chamados/models.py:620`, `app/chamados/models.py:627`, `app/chamados/models.py:638`, `app/chamados/models.py:648`, `app/chamados/models.py:651`).

**Campos lidos pela premissa → escritor → o que vem depois**

| Estado lido | Escrita e sequência |
|---|---|
| `modulo_origem`, `categoria`, `subcategoria`: determinam se é chamado de furo | Leitura em `app/chamados/models.py:1205`; nascimento pelas portas `abrir` e `criar`, em `app/chamados/models.py:796` e `app/chamados/models.py:1015`. O nascimento dispara o `post_save` que julga a célula; mudança desses campos sem mudar status não passa na guarda desse receiver (`app/chamados/signals.py:165`, `app/chamados/signals.py:169`, `app/chamados/signals.py:180`). |
| Dia forte: `ch.celula.data`; senão data do catálogo/`contexto_json.data_turno` | Leitura em `app/chamados/juizes.py:91`, `app/chamados/juizes.py:107`, `app/chamados/juizes.py:111`, encerrando fallback em `app/chamados/juizes.py:120`. FK do chamado é escrita por `apontar_celula`, que salva e retorna (`app/chamados/services/vinculos.py:92`, `app/chamados/services/vinculos.py:98`). Contexto existente é escrito pelas portas `anotar`, `renovar`, `retirar`, `fundir`, com saves em `app/chamados/services/contexto.py:76`, `app/chamados/services/contexto.py:101`, `app/chamados/services/contexto.py:123`, `app/chamados/services/contexto.py:158`. Elas próprias não reconciliam; o receiver do chamado ignora save sem mudança de status (`app/chamados/services/contexto.py:47`, `app/chamados/signals.py:169`). |
| `justificativa_relacionada_id` e `Justificativa.status == 'aprovada'` | Leitura em `app/chamados/models.py:1227`; FK escrita por `apontar_justificativa`, que salva e retorna (`app/chamados/services/vinculos.py:81`, `app/chamados/services/vinculos.py:87`). A aprovação ocorre em três sítios: serviço individual (`app/ponto/services/justificativa.py:190`), materialização retroativa (`app/ponto/services/justificativa.py:368`), lote da view (`app/ponto/views.py:1796`). Os três chamam `encerrar_chamado_de_origem` depois: `app/ponto/services/justificativa.py:202`, `app/ponto/services/justificativa.py:393`, `app/ponto/views.py:1837`. A função busca a origem viva e passa por `reconciliador.retratar`, não por escrita crua de status (`app/ponto/services/justificativa.py:33`, `app/ponto/services/justificativa.py:38`, `app/ponto/services/justificativa.py:42`). Cada save de Justificativa também julga o dia pelo signal (`app/ponto/signals.py:162`, `app/ponto/signals.py:165`). |
| `CelulaDia.veredito_via == 'admin'` | Regulariza diretamente (`app/chamados/models.py:1243`). `veredito`, `veredito_via`, `veredito_em`, `protestos` e eventual `ata` são escritos pela porta `lavrar_veredito` (`app/ponto/portas/celula.py:751`, `app/ponto/portas/celula.py:810`, `app/ponto/portas/celula.py:812`, `app/ponto/portas/celula.py:813`). A persistência passa por `salvar_e_emitir`; se campos lavrados incluem `veredito` ou `ata`, grava `EventoCelula` no mesmo atomic (`app/ponto/portas/celula.py:663`, `app/ponto/portas/celula.py:676`, `app/ponto/portas/celula.py:680`, `app/ponto/portas/celula.py:684`). |
| `CelulaDia.trabalha`, `ata.tipo_dia`; `contexto_json.marcos_faltantes`; `ata.lampadas.{hora,tipo,acesa}` | São lidos pelo dia sem trabalho e pelos juízes de marco em `app/chamados/models.py:1245`, `app/chamados/models.py:1247`, `app/chamados/models.py:1249`, `app/chamados/models.py:1258`; regra de dia sem trabalho em `app/chamados/juizes.py:413`. O escritor da ata persistida é a mesma porta da lavra, que emite EventoCelula quando a ata muda (`app/ponto/portas/celula.py:777`, `app/ponto/portas/celula.py:810`, `app/ponto/portas/celula.py:817`). Outra escrita de ata é `carimbar_precedencia`, também com emissão (`app/ponto/portas/celula.py:853`, `app/ponto/portas/celula.py:862`, `app/ponto/portas/celula.py:864`). |
| `trabalha`/`dna` mudados por regeneração | Escritas em `app/ponto/portas/celula.py:237`, `app/ponto/portas/celula.py:238`, `app/ponto/portas/celula.py:266`, `app/ponto/portas/celula.py:270`. A regeneração rejulga dias tocados com `cartorio.julgar_celula(..., apply_=True, forcar=True)` e agenda rejunção + reavaliação de faltas espúrias (`app/ponto/portas/celula.py:393`, `app/ponto/portas/celula.py:402`, `app/ponto/portas/celula.py:461`, `app/ponto/portas/celula.py:469`, `app/ponto/portas/celula.py:472`, `app/ponto/portas/celula.py:473`). Reverter regeneração apenas salva DNA/trabalha e trilha, retornando; não possui esses ganchos no corpo (`app/ponto/portas/celula.py:570`, `app/ponto/portas/celula.py:578`, `app/ponto/portas/celula.py:581`, `app/ponto/portas/celula.py:592`). Gerar período também salva DNA/trabalha e retorna sem essa emissão no corpo (`app/ponto/portas/celula.py:644`, `app/ponto/portas/celula.py:645`, `app/ponto/portas/celula.py:652`, `app/ponto/portas/celula.py:655`, `app/ponto/portas/celula.py:657`). |
| Fallback `_celula_furada`: ausência que cobre o dia inteiro, `trabalha`, `ata.lampadas.acesa`, `ata.orfas`/`n_orfas`, confiança da escala | Leituras em `app/chamados/juizes.py:448`, `app/chamados/juizes.py:449`, `app/chamados/juizes.py:465`, `app/chamados/juizes.py:466`, `app/chamados/juizes.py:477`, `app/chamados/juizes.py:480`, `app/chamados/juizes.py:482`, `app/chamados/juizes.py:487`, `app/chamados/juizes.py:488`. As portas de Ausencia criam/salvam em `app/ponto/services/ausencia.py:418`, editam registro em `app/ponto/services/ausencia.py:508`, mudam status em `app/ponto/services/ausencia.py:808` e `app/ponto/services/ausencia.py:819`, decidem período em `app/ponto/services/ausencia.py:911`. Saves/deletes disparam julgamento do cartório e consumidor de ausência (`app/ponto/signals.py:144`, `app/ponto/signals.py:147`, `app/ponto/signals.py:151`, `app/ponto/signals.py:152`, `app/ponto/signals.py:250`, `app/ponto/signals.py:276`, `app/ponto/signals.py:283`). |
| Sem resposta da célula: faixa feriado/folga; batidas do dia; turnos abertos | Fallback em `app/chamados/models.py:1265`, `app/chamados/models.py:1266`, `app/chamados/models.py:1270`, `app/chamados/models.py:1272`, `app/chamados/models.py:1273`. Feriado post_save agenda reavaliação; sua porta termina em `ch.reconciliar()` (`app/chamados/signals.py:269`, `app/chamados/signals.py:279`, `app/chamados/signals.py:280`, `app/chamados/services/feriado_reavaliar.py:47`). Batida save/delete aciona núcleo/cartório, e save também reconciliador por colaborador (`app/ponto/signals.py:35`, `app/ponto/signals.py:49`, `app/ponto/signals.py:132`, `app/ponto/signals.py:135`, `app/ponto/signals.py:139`, `app/ponto/signals.py:140`, `app/ponto/signals.py:207`, `app/ponto/signals.py:215`). |
| `contexto_json.agrupado_em`, existência do pai, `pai.colaborador_id`, `pai.status_local` | Leitura em `app/chamados/models.py:772`, `app/chamados/models.py:778`, `app/chamados/models.py:785`, `app/chamados/models.py:788`, `app/chamados/models.py:793`. Adesão grava agrupado e reconcilia imediatamente (`app/chamados/services/adesao.py:112`, `app/chamados/services/adesao.py:115`). Limbo grava agrupado, reaponta perguntas vivas e reconcilia; pode desfazer reata e repetir (`app/chamados/services/limbo_folgas.py:269`, `app/chamados/services/limbo_folgas.py:276`, `app/chamados/services/limbo_folgas.py:277`, `app/chamados/services/limbo_folgas.py:279`, `app/chamados/services/limbo_folgas.py:280`). |
| Outro chamado vivo do mesmo colab-dia, seu módulo, contexto/célula e ordenação `criado_em,pk` | C8 lê esses estados e escolhe o mais novo (`app/chamados/juizes.py:1897`, `app/chamados/juizes.py:1898`, `app/chamados/juizes.py:1899`, `app/chamados/juizes.py:1902`). Escritor de status é `_gravar_estado`, que salva e dispara os receivers (`app/chamados/models.py:222`, `app/chamados/models.py:279`); o cartório é chamado na criação ou mudança de status (`app/chamados/signals.py:165`, `app/chamados/signals.py:169`, `app/chamados/signals.py:180`). |

**Campos lidos pela pergunta viva → escritor → depois**

O gate coleta perguntas **não validadas**, ligadas por `chamado_destino` ou pela disputa sem destino; lê `data_evento`, empresa, competência entregue, e pergunta ao C6 no **pós-estado** do chamado. Para reatar, lê outra cobrança viva e `resolvedora_valida` (`app/chamados/models.py:327`, `app/chamados/models.py:328`, `app/chamados/models.py:339`, `app/chamados/models.py:347`, `app/chamados/models.py:351`, `app/chamados/models.py:373`, `app/chamados/models.py:375`).

| Estado lido | Escrita e sequência |
|---|---|
| `validada_em` | Validação escreve/salva em `app/chamados/services/validacao.py:131`, `app/chamados/services/validacao.py:133`; devolução limpa em `app/chamados/services/validacao.py:188`, `app/chamados/services/validacao.py:190`. O ato de validar chama recalcular fechamento por evento, materializa a unidade de hora e depois o lote da disputa (`app/chamados/services/validacao.py:395`, `app/chamados/services/validacao.py:396`, `app/chamados/services/validacao.py:406`, `app/chamados/services/validacao.py:407`, `app/chamados/services/validacao.py:424`, `app/chamados/services/validacao.py:430`). Esse recalcular agendado é da folha mensal, não consumo de EventoCelula (`app/ponto/services/fechamento.py:1791`, `app/ponto/services/fechamento.py:1794`, `app/ponto/services/fechamento.py:1808`). |
| `resposta_colab`, `respondida_em`, `tipo_resposta` e reset de `validada_em` | Resposta humana escreve texto+instante, zera validação anterior, desfaz carimbo obsoleto e salva (`app/chamados/services/disputa_emissao.py:1282`, `app/chamados/services/disputa_emissao.py:1283`, `app/chamados/services/disputa_emissao.py:1295`, `app/chamados/services/disputa_emissao.py:1300`, `app/chamados/services/disputa_emissao.py:1301`). Depois audita, ressuscita por resposta e materializa só escapes que viram ausência; folga/outro vão ao admin (`app/chamados/services/disputa_emissao.py:1318`, `app/chamados/services/disputa_emissao.py:1326`, `app/chamados/services/disputa_emissao.py:1327`, `app/chamados/services/disputa_emissao.py:1330`, `app/chamados/services/disputa_emissao.py:1331`). Resposta automática só salva e retorna (`app/chamados/services/disputa_emissao.py:1428`, `app/chamados/services/disputa_emissao.py:1430`, `app/chamados/services/disputa_emissao.py:1431`). Carimbo pela trilha salva, audita e retorna (`app/chamados/services/disputa_emissao.py:1445`, `app/chamados/services/disputa_emissao.py:1447`, `app/chamados/services/disputa_emissao.py:1450`). |
| Correção zera resposta/validação | `reabrir_para_correcao` guarda RespostaAnterior e termina em update; não há post_save desse update (`app/chamados/services/disputa_emissao.py:1393`, `app/chamados/services/disputa_emissao.py:1403`, `app/chamados/services/disputa_emissao.py:1410`, `app/chamados/services/disputa_emissao.py:1412`, `app/chamados/services/disputa_emissao.py:1413`). |
| `materializada_em`, `via_resolucao` | Atribuição concentrada em `_escrever_carimbo`; portas `carimbar`, `carimbar_identidade`, `descarimbar` salvam e retornam (`app/chamados/models.py:2556`, `app/chamados/models.py:2558`, `app/chamados/models.py:2560`, `app/chamados/models.py:2609`, `app/chamados/models.py:2614`, `app/chamados/models.py:2644`, `app/chamados/models.py:2652`). Limpeza em lote é update (`app/chamados/models.py:2663`). Via também escrita isoladamente por `encerrar_por_fato_no_espelho` e `relavrar_via` (`app/chamados/models.py:2673`, `app/chamados/models.py:2674`, `app/chamados/services/relavra_via.py:123`, `app/chamados/services/relavra_via.py:125`). Ressuscitar limpa carimbos em lote e reabre disputas, em vez de reconciliar o chamado (`app/chamados/models.py:1504`, `app/chamados/models.py:1508`). |
| `chamado_destino`, `celula`, `data_evento`, motivo/tipo da pergunta | Nascimento/reuso escreve em `PerguntaDisputa.abrir`, com células e destino conferidos antes; salva/recria e retorna (`app/chamados/models.py:2398`, `app/chamados/models.py:2436`, `app/chamados/models.py:2450`, `app/chamados/models.py:2474`, `app/chamados/models.py:2476`). Reaponte existente escreve destino e eventual data, relavra a pergunta, salva e retorna (`app/chamados/services/fio.py:834`, `app/chamados/services/fio.py:845`, `app/chamados/services/fio.py:846`, `app/chamados/services/fio.py:847`, `app/chamados/services/fio.py:849`). |
| Tranca `PeriodoFechado.reaberto_em`, `fechado_em`, empresa/dia e toque humano posterior em LogAuditoria | Leitura em `app/chamados/juizes.py:931`, `app/chamados/juizes.py:944`, `app/chamados/juizes.py:947`, `app/chamados/juizes.py:954`, `app/chamados/juizes.py:1071`, `app/chamados/juizes.py:1073`, `app/chamados/juizes.py:1079`, `app/chamados/juizes.py:1083`; juiz de tranca em `app/ponto/services/fechamento.py:1454`. Trancar cria/zera reabertura e chama `competencia_trancada.disparar_no_ato`; reabrir salva e chama reabertura da fila (`app/ponto/services/fechamento.py:1392`, `app/ponto/services/fechamento.py:1401`, `app/ponto/services/fechamento.py:1403`, `app/ponto/services/fechamento.py:1412`, `app/ponto/services/fechamento.py:1423`, `app/ponto/services/fechamento.py:1425`, `app/ponto/services/fechamento.py:1432`). A via antiga `competencia_trancada` continua lida mas não tem escritor produtivo novo (`app/chamados/models.py:2525`). |
| Competência entregue: ExportacaoDominio vigente e `invalidada_em` | Gate delega em `app/chamados/services/vigia_fio.py:836`, `app/chamados/services/vigia_fio.py:838`, `app/chamados/services/vigia_fio.py:841`; leitura vigente/revogação em `app/folha/export.py:580`, `app/folha/export.py:583`. `registrar` cria e retorna; `invalidar` salva, grava LogAuditoria e retorna, sem EventoCelula no corpo dessas portas (`app/folha/models.py:174`, `app/folha/models.py:182`, `app/folha/models.py:188`, `app/folha/models.py:158`, `app/folha/models.py:162`, `app/folha/models.py:163`, `app/folha/models.py:171`). |
| Célula: `veredito`, `veredito_via`, `dna.marcos`, `ata.lampadas.{hora,tipo,acesa,luz,pendente}`; batidas plantadas/vivas; testemunha de falta/turno | C6 lê os primeiros em `app/chamados/juizes.py:1147`, `app/chamados/juizes.py:1148`, `app/chamados/juizes.py:1181`, `app/chamados/juizes.py:1194`, `app/chamados/juizes.py:1195`; marco em `app/chamados/juizes.py:893`, `app/chamados/juizes.py:910`, `app/chamados/juizes.py:913`; concordância resposta/luz em `app/chamados/juizes.py:1111`, `app/chamados/juizes.py:1113`, `app/chamados/juizes.py:1125`, `app/chamados/juizes.py:1132`. Testemunha externa lê batidas retratadas/slot e status do destino (`app/chamados/juizes.py:718`, `app/chamados/juizes.py:720`, `app/chamados/juizes.py:743`, `app/chamados/juizes.py:744`) e classifica falta (`app/chamados/juizes.py:1201`). Escritores e eventos de célula/batida são os já indicados acima (`app/ponto/portas/celula.py:817`, `app/ponto/signals.py:132`, `app/ponto/signals.py:207`). |

**A conexão posterior à escrita de pergunta tem condicionais importantes**

Toda `PerguntaDisputa.save()` não raw agenda `fechar_beco_da_disputa`, mas esse receiver **não grava EventoCelula nem reconcilia diretamente o destino** (`app/chamados/signals.py:191`, `app/chamados/signals.py:193`, `app/chamados/signals.py:200`, `app/chamados/signals.py:202`).

O beco só atua se o pai já está resolvido, não há perguntas visíveis e `d.pode_fechar()` permite; então fecha a disputa (`app/chamados/services/becos.py:25`, `app/chamados/services/becos.py:29`, `app/chamados/services/becos.py:31`, `app/chamados/services/becos.py:33`, `app/chamados/services/becos.py:69`, `app/chamados/services/becos.py:73`, `app/chamados/services/becos.py:52`). A Defesa 1 do fechamento de disputa reconcilia somente `instance.chamado_id`, se ainda não encerrado (`app/chamados/signals.py:28`, `app/chamados/signals.py:32`, `app/chamados/signals.py:33`, `app/chamados/signals.py:42`). Portanto não se demonstra, só pelo save genérico de pergunta, tentativa no `batida_ausente` que é apenas `chamado_destino` distinto desse pai; tal tentativa nesse caso é [não verificado].

Existem caminhos explícitos pós-materialização: destino e clones são reconciliados com guarda de irmãs respondidas sem consumo (`app/chamados/services/materializacao.py:709`, `app/chamados/services/materializacao.py:732`, `app/chamados/services/materializacao.py:741`, `app/chamados/services/materializacao.py:743`, `app/chamados/services/materializacao.py:745`, `app/chamados/services/materializacao.py:751`); ausência já decidida também reconcilia destinos (`app/chamados/services/materializacao.py:563`, `app/chamados/services/materializacao.py:564`). Aceitar folga já existente pede novamente o fechamento de todo o fio do dia (`app/chamados/services/validacao.py:263`, `app/chamados/services/validacao.py:264`, `app/chamados/services/validacao.py:271`), cuja porta percorre chamados vivos e reconcilia (`app/ponto/services/veredito_celula.py:182`, `app/ponto/services/veredito_celula.py:183`).

`core.observ.evento(...)` dessas escritas produz trilha; o consumidor aqui investigado busca **EventoCelula**, agrupa por colaborador e processa a fila dessa entidade, não esses textos de LogAuditoria (`app/chamados/reconciliador.py:1324`, `app/chamados/reconciliador.py:1331`, `app/chamados/reconciliador.py:1337`). Se os ganchos condicionais de uma escrita concreta foram executados no ambiente real: [não verificado].

## 06/11 04:07 · sessao dcc6de05028b · - · - · origem=Ronald (tmux)

**6. Leitores diretos da lâmpada, fora de `lastro.py`, com regra própria**

| Sítio | Regra |
|---|---|
| `app/escala/services/leitor_celula.py:68`, `:81`, `:103` | Conta `dispensada`, reconhece dispensa por justificativa e subtrai dispensadas da contagem de casadas. |
| `app/escala/services/leitor_celula.py:119` | `acesa` usa fallback `luz is not None`; valor `None` vira verdadeiro para não produzir furo; escreve `missing=not acesa`. Transporta `isencao`/`dispensada` em `:139`/`:141`. |
| `app/chamados/juizes.py:480` | Só `acesa is False` acusa furo. |
| `app/chamados/juizes.py:910` | Para os marcos da pergunta, calcula `all(bool(acesa))`; lâmpada inexistente devolve `None` em `:914`. |
| `app/chamados/juizes.py:1111` | Lê `luz` do marco para comparar com a resposta; exige marco provado aceso antes, em `:1101`. |
| `app/chamados/models.py:2753` | SQL: qualquer lâmpada `pendente=True` impede que a célula absolva em `:2755`. |
| `app/chamados/services/materializacao.py:172` | “Marca” exige `acesa` e `luz` válida em HH:MM (`:179`); agrega órfãs em `:180`. |
| `app/ponto/juiz_batida.py:215` | No posicional, exige lâmpada acesa e luz interpretável (`:217`). |
| `app/ponto/juiz_batida.py:237` | Só calcula instante da luz se acesa (`:241`); bordas exigem acesa (`:259`, `:260`, `:263`); intervalo batido exige ambos acesos e instantes (`:282`, `:291`). |
| `app/ponto/services/cartorio.py:422` | Marco novo vencido conta se `acesa is False`. |
| `app/ponto/services/cartorio.py:773` | Dispensa lâmpada que não seja `True`, gravando `acesa=True`, `luz=None`, `dispensada=motivo`; reconta apagadas em `:778`. |
| `app/ponto/services/cartorio.py:815` | Entrada/saída: `True` prevalece; depois `None`; senão `False`. |
| `app/ponto/services/cartorio.py:869` | Apagada e ainda pendente vira `acesa=None, pendente=True`; reconta somente `False` em `:872`. |
| `app/ponto/management/commands/detectar_intervalo_ausente.py:68` | Acesa com fallback `luz is not None`; intervalo faltante exige `acesa is False` em `:74`. |
| `app/pautas/services.py:421` | `False` produz texto “sem batida” em `:424`. |
| `app/relatorios/dia_da_frota.py:21` | Lista faltantes somente com `acesa is False` (`:22`). |
| `app/relatorios/cartao_pela_celula.py:289` | Extrai horas sintéticas marcadas `isencao` (`:290`). |
| `app/folha/previa.py:84` | Só `acesa is False` gera proposta (`:86`). |
| `app/folha/services/sem_furo.py:20` | Dia furado se alguma lâmpada for `False` (`:21`). |
| `app/colaboradores/services/feriado_do_posto.py:115` | `any(acesa)` vira dia “bateu” em `:118`. |
| `app/ponto/services/cluster_espurio.py:32` | Número de faltantes é a contagem de `acesa is False`. |
| `app/ponto/services/vigia_entrada.py:50` | Classifica por `any(acesa)` entre entradas e saídas (`:52`, `:53`); lista entrada faltante com ausência de qualquer entrada acesa em `:132`. |
| `app/ponto/services/vigia_pareador_celula.py:107` | Divergência na borda exige lâmpada `acesa is False`. |
| `app/ponto/services/esmeril_espelho.py:129` | Usa acesa + luz/hora interpretáveis e distância entre 45 e 600 minutos (`:131`). |
| `app/ponto/services/dia_do_colab.py:38` | Apresenta `bateu=bool(acesa)` e `marcou_as=luz` (`:39`). |
| `app/ponto/services/diagnostico_escala.py:155` | Mesma transformação para `bateu`/`marcou_as`. |
| `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:62` | Qualquer lâmpada `False`, ou veredito acusador, conta “sem efeito”. |
| `app/ponto/management/commands/tripwire_celulas.py:171` | Borda cuja `acesa` não seja verdadeira pode ser denunciada com órfã próxima (`:172`, `:177`). |

**Leitores derivados de `missing` e marcadores transportados**

| Sítio | Regra |
|---|---|
| `app/relatorios/furos.py:31` | Exclui dispensada dos marcos em jogo (`:32`); apagado é `missing` (`:39`). |
| `app/inteligencia/calculadores.py:143` | Usa `grade_da_celula`; conta batido como `not missing` após `marcos_do_jogo` (`:156`, `:161`). |
| `app/escala/services/furos_diarios.py:17` | Mesmos marcos em jogo; conta missing (`:21`), lendo `grade_da_celula` em `:43`. |
| `app/chamados/services/declaracao_texto.py:82` | Lê `grade_da_celula`; só marco `missing` entra nos faltantes (`:110`). |
| `app/ponto/propositor.py:40` | Casada é `not missing`; exige hora conhecida em `:52`. Deltas só de não-missing em `:213`. |
| `app/ponto/turnos.py:2659` | Hora real exige `not missing` e hora; agrega órfãs em `:2665`. |
| `app/ponto/supra_juiz.py:88` | Batida casada exige `not missing`, `not isencao`, hora (`:89`, `:90`); parcial com dispensada e nenhum missing significa ausência de resto em `:306`–`:312`. |
| `app/ponto/status_dia.py:185` | Contagem real exclui missing, isencao e dispensada (`:186`, `:187`), acrescenta órfãs; considera também minutos realizados em `:190`. |
| `app/ponto/services/cartorio.py:1373` | Contagem de batidas exclui missing e dispensada (`:1374`), soma órfãs (`:1375`). |
| `app/ponto/management/commands/propor_reorganizacao.py:65` | Usa `grade_da_celula`; marcas reais exigem não-missing e hora (`:101`–`:104`). |
| `app/folha/causas.py:24` | Conta missing (`:25`). |
| `app/escala/utils.py:1114` | Adaptador separa missing, associando cobrança e hora do marco em vez de hora real. |
| `app/ponto/services/espelho.py:762` | Seleciona missing para a lista do badge (`:763`). |
| `app/colaboradores/services/calendario.py:377` | Mesmo transporte para badge (`:378`, `:379`). |
| `app/ponto/views.py:165` | `tem_furo=any(missing)`. |
| `app/templates/ponto/espelho.html:151`, `:233` | Renderização separa missing de batida, com exceção de isenção (`:237`). |
| `app/ponto/services/espelho.py:359`, `:362`; `app/relatorios/pdf_espelho.py:553`, `:556`, `:925` | Extração/apresentação das horas e identificação de isenção. |

`app/ponto/worklist.py:22` apenas exige presença de lâmpadas e delega a `marcos_apagados` em `:24`, portanto não tem regra própria de acesa. Os `missing` de `app/chamados/detector_anomalias.py:210`, `app/ponto/motor_calculo_v2.py:3062`, `app/ponto/services/simular_dia.py:6` e `app/ponto/management/commands/detectar_par_relampago.py:136` vêm do builder/simulação, não da leitura da ata gravada.

**8. Texto/tipo `lastro_encontrado`**

- O escritor é `app/ponto/services/lastro.py:345`–`:350`: só depois de `reconciliar()` verdadeiro anexa o texto com `tipo='lastro_encontrado'`.
- O tipo não vira campo da nota: `app/chamados/services/nota.py:88`–`:103` monta conteúdo/autores e só usa tipo na chave quando existe `objeto_ref` (`:95`–`:100`); a chamada de lastro não fornece esse argumento (`app/ponto/services/lastro.py:347`–`:350`).
- Leitor específico encontrado: `app/ponto/tests/test_bug114_fechador_de_dois_dias.py:304`–`:306`, filtro por prefixo `BUG 114: a batida`. O teste exige zero notas se o juiz recusar (`:316`–`:319`) e uma nota após duas passadas que encerram (`:327`–`:331`).
- Menções ao tipo fora do escritor: documentação em `app/docs/RELATO.md:2703` e docstring do teste em `app/ponto/tests/test_bug114_fechador_de_dois_dias.py:298`; essas referências não comprovam a situação atual de produção [não verificado].
- Telas genéricas exibem `fu.conteudo`, inclusive notas do sistema: `app/templates/chamados/detalhe_local.html:112`/`:135`; `detalhe.html:80`/`:103`; `fio_colaborador.html:235`/`:247`; `partials/modal_fio.html:456`/`:468`; `partials/_card_atendimento.html:43`/`:53`; `partials/painel_chamado_turno.html:45`/`:56`.
- A listagem conta todas as notas e guarda a última: `app/chamados/services/painel.py:360`–`:361`, `:395`–`:396`; apresenta texto e quantidade em `app/templates/chamados/partials/_lista_chamados.html:59`, `:89`–`:90`.
- API lê conteúdo genericamente, mas esconde essa nota de sistema: `app/api/views.py:62`–`:65` só aceita nota de sistema contendo “justificativa”; o filtro é aplicado em `:1128`–`:1130` antes da serialização de conteúdo em `:1137`. O texto de lastro não contém essa palavra (`app/ponto/services/lastro.py:347`–`:350`).
- Outros consumidores específicos do tipo/texto em contadores ou selos não foram encontrados na busca global [não verificado].

## 06/11 03:38 · sessao 873fb725b2fe · O261-ITEM3-PEP · verificador · origem=Code

1. **OBSERVAÇÃO — os PROCEDENTES da v1 foram fechados parcialmente.** Uso **“v2 virtual”** para a numeração após incorporar o anexo em memória; as demais referências são da cópia. Execução dos testes, dos mutantes e ocorrência dos casos em produção: **[não verificado]**. [nao verificado]

   | Procedente da triagem | O v2 fecha? |
   |---|---|
   | **1a — reentrada S143 na mesma transação** | **Fecha a recusa específica:** encontrando o carimbo anterior aceito pelo C6, retorna `True` sem sobrescrevê-lo. Isso evita `_Reprovada` naquele caminho. Porém estende o mesmo sucesso a atos independentes, com efeitos indevidos nos consumidores descritos abaixo. (`app/chamados/models.py:2598`, **v2 virtual**; `app/chamados/services/materializacao.py:1204`, cópia) |
   | **1b — falha no commit próprio** | **Fecha o posicionamento do tratamento:** o `except` agora envolve a saída do `with` e restaura as fotografias existentes. O teste novo provoca falha de commit, mas verifica somente carimbo, sem solicitar lavra. (`app/chamados/models.py:2595`, **v2 virtual**; `app/chamados/models.py:2628`, **v2 virtual**; `app/chamados/tests/test_o261_item3_pep.py:307`, **anexo**) |
   | **2b — resposta nova na ordem inversa** | **Não fecha.** O escritor continua decidindo limpar validação a partir da instância recebida e pode salvar apenas resposta/instante, preservando uma validação gravada depois daquela leitura. Ser anterior ao patch sustenta a classificação como dívida existente; não sustenta dizer que a proteção é bilateral. (`app/chamados/services/disputa_emissao.py:1284`; `app/chamados/services/disputa_emissao.py:1286`; `app/chamados/services/disputa_emissao.py:1301`) |
   | **3 — textos atribuíam toda recusa ao C6** | **Fecha a causa “mudança humana”, mas não toda a veracidade.** Acrescentou essa alternativa; permanecem “segue pendente”, nome de evento “viva” e a causa omitida “linha sumiu”. (`app/chamados/services/materializacao.py:552`, **v2 virtual**; `app/chamados/services/materializacao.py:1206`, **v2 virtual**; `app/chamados/models.py:2661`, **v2 virtual**) |
   | **5 — retirar um campo isolado da guarda sobrevivia** | **Fecha por inspeção dos testes:** o novo `subTest` altera separadamente texto, instante da resposta, instante e autor da validação. A execução dos quatro mutantes continua **[não verificado]**. (`app/chamados/tests/test_o261_item3_pep.py:99`, **anexo**) |

   **OBSERVAÇÃO — a janela da validação também permanece:** lê e julga antes do `atomic`, que começa somente para gravar a assinatura. (`app/chamados/services/validacao.py:359`; `app/chamados/services/validacao.py:374`; `app/chamados/services/validacao.py:395`)

2. **PROCEDENTE-candidato — dois motivos de DESCARTE são abrangentes demais.**

   | Descarte | Conferência do motivo escrito |
   |---|---|
   | **1c e 7b — conteúdo das relações** | **A ressalva sobre julgamento em curso se sustenta.** Existe projeção explícita de célula, e a ressuscitação a utiliza. Mas isso não torna qualquer relação carregada uma fotografia mais nova que o banco. O E2 consulta normalmente célula/disputa/destino, e o cartório também tem caminho que chama E2 **depois** da lavra. Preservar indistintamente essas relações mantém o contraexemplo de cache obsoleto. (`app/chamados/juizes.py:383`; `app/chamados/models.py:1450`; `app/chamados/reconciliador.py:641`; `app/ponto/services/cartorio.py:953`; `app/ponto/services/cartorio.py:959`) |
   | **1c — disputa/destino incluídos no descarte** | **A justificativa da célula não os cobre.** O juiz lê contexto da disputa e status do destino; o laço novo copia apenas campos concretos da pergunta, preservando relações com a mesma FK. (`app/chamados/juizes.py:364`; `app/chamados/juizes.py:743`; `app/chamados/models.py:2663`, **v2 virtual**) |
   | **1c — reler célula exigiria inverter trava** | **Não demonstrado.** Releitura e aquisição de trava são operações distintas; a relavra de referência carrega célula, mas seu `of` declara somente pergunta/disputa. Ciclo concreto com a porta da célula: **[não verificado]**. (`app/chamados/services/relavra_via.py:221`; `app/chamados/services/relavra_via.py:222`) |
   | **1d — campo deferido** | **Sustenta rebaixar a prioridade, não a justificativa geral.** Os chamadores conferidos carregam perguntas completas. Entretanto, decidir por filtro SQL sobre um campo não exige carregar seu atributo: o hipotético `filter(validada_em__isnull=True).only('pk')` já decide sobre ele. Chamador atual com essa projeção: **[não verificado]**. (`app/chamados/services/disputa_emissao.py:1572`; `app/chamados/reconciliador.py:641`; `app/chamados/services/materializacao.py:1343`; `app/chamados/models.py:2657`, **v2 virtual**) |
   | **5b — sempre abrir `atomic()` só mudaria SAVEPOINT** | **Não se sustenta como equivalência comportamental.** Savepoint muda a contenção de erro quando alguém captura a exceção. O repositório documenta justamente rollback externo provocado por erro engolido sem savepoint. S144 captura exceção do materializador sem criar seu próprio savepoint. Isso não obriga mudar a candidata escolhida, mas invalida o motivo do descarte. (`app/core/observ.py:124`; `app/chamados/signals.py:83`; `app/chamados/signals.py:139`; `app/chamados/signals.py:141`) |

3. **PROCEDENTE-candidato — o novo `True` mistura “estado aceito” com “ato gravado”.** O contrato escrito continua `bool (gravou?)`, mas o ramo já carimbado retorna antes de escrever, completar resposta/validação ou lavrar. Também precede a comparação dos campos humanos. (`app/chamados/models.py:2563`; `app/chamados/models.py:2602`, **v2 virtual**; `app/chamados/models.py:2665`, **v2 virtual**; `app/chamados/models.py:2667`, **v2 virtual**)

   Para os oito chamadores, considerando carimbo de **outra via, autor ou instante**:

   | Chamador | Consequência de `True` e avaliação |
   |---|---|
   | **`lavrar_sem_turno_aberto`** | Pode fechar a disputa, grava resolução “sem turno aberto” e retorna sucesso. O fechamento continua condicionado a `pode_fechar`; o retorno já não comprova que esta chamada lavrou aquela via. (`app/chamados/reconciliador.py:405`; `app/chamados/reconciliador.py:411`; `app/chamados/reconciliador.py:412`; `app/chamados/reconciliador.py:414`) |
   | **`retratar_perguntas_absolvidas_por_celula`** | Incrementa `n`, agenda disputa e pode fechá-la atribuindo resolução à célula. O número documentado como “perguntas carimbadas” passa a incluir carimbo alheio encontrado. (`app/chamados/reconciliador.py:551`; `app/chamados/reconciliador.py:588`; `app/chamados/reconciliador.py:590`; `app/chamados/reconciliador.py:603`) |
   | **S143, ramo principal** | Incrementa `perguntas_resolvidas`, inclui disputa e pode fechá-la. Aceitar a reentrada própria resolve 1a; contabilizar ato independente não prova resolução efetuada nesta chamada. A lavra solicitada também é pulada nesse retorno. (`app/chamados/services/disputa_emissao.py:1603`; `app/chamados/services/disputa_emissao.py:1606`; `app/chamados/services/disputa_emissao.py:1607`; `app/chamados/services/disputa_emissao.py:1696`; `app/chamados/models.py:2602`, **v2 virtual**) |
   | **S143, ramo sem credor** | **Emite trilha incorreta:** `fecho_sem_credor`, ator atual, fotografia anterior sem carimbo e `depois.materializada_em` igual ao horário da tentativa. O banco conserva o instante alheio. Esse ramo não incrementa `n_pq`; ainda participa da cauda de fechamento. (`app/chamados/services/disputa_emissao.py:1666`; `app/chamados/services/disputa_emissao.py:1678`; `app/chamados/services/disputa_emissao.py:1683`; `app/chamados/services/disputa_emissao.py:1689`) |
   | **`_materializar_ausencia_de_resposta`** | Não registra recusa; conserva a Ausência já criada, reconcilia destino e pode fechar disputa. Preservar decisão alheia é adequado; isso não significa que a irmã foi absorvida neste ato: o agrupamento da absorção depende do mesmo `materializada_em`. (`app/chamados/services/materializacao.py:520`; `app/chamados/services/materializacao.py:549`; `app/chamados/services/materializacao.py:564`; `app/chamados/services/materializacao.py:568`; `app/chamados/services/materializacao.py:593`) |
   | **`_materializar_uma`** | Conserva plantio/reuso anterior ao carimbo, marca sucesso e aumenta `materializadas`. Correto para a reentrada S143 do próprio plantio; para ato independente, a morte pelo C6 não demonstra que o plantio baseado na leitura anterior continua válido. Nesse ramo nem se aplica a comparação humana. (`app/chamados/services/materializacao.py:1145`; `app/chamados/services/materializacao.py:1147`; `app/chamados/services/materializacao.py:1207`; `app/chamados/services/materializacao.py:1302`; `app/chamados/models.py:2665`, **v2 virtual**) |
   | **`materializar_pendentes_aprovadas`** | Acrescenta PK a `feitas`; não emite evento nesse trecho, e o receiver ignora a lista. Preserva o carimbo, mas a lista deixa de representar exatamente as perguntas carimbadas pela chamada. (`app/chamados/services/materializacao.py:1317`; `app/chamados/services/materializacao.py:1349`; `app/chamados/services/materializacao.py:1350`; `app/chamados/signals.py:140`) |
   | **`encerrar_fantasma`** | Propaga `True`; o consumidor emite evento de encerramento pela lavra, incrementa `encerradas` e agenda disputa. **Contraria o contrato local de trilha somente quando escreveu**, embora o carimbo seja de outro ato. (`app/chamados/models.py:2685`; `app/chamados/reconciliador.py:1107`; `app/chamados/reconciliador.py:1138`; `app/chamados/reconciliador.py:1145`; `app/chamados/reconciliador.py:1146`) |

   **PROCEDENTE-candidato — os textos de `False` ainda não dizem toda a verdade:**

   - Se `validada_em` mudou de nulo para preenchido, a guarda retorna `False` sem chamar C6; o C6 real já considera essa pergunta morta. Portanto “segue pendente” e a ação `irma_viva_na_ausencia` não decorrem do retorno. (`app/chamados/models.py:2667`, **v2 virtual**; `app/chamados/juizes.py:1136`; `app/chamados/services/materializacao.py:553`, **v2 virtual**; `app/chamados/services/materializacao.py:573`, cópia)
   - Linha desaparecida também retorna `False`, sem provar nenhuma das duas causas escritas nos novos textos. (`app/chamados/models.py:2661`, **v2 virtual**; `app/chamados/services/materializacao.py:552`, **v2 virtual**; `app/chamados/services/materializacao.py:1206`, **v2 virtual**)
   - A frase sobre a batida sair do espelho continua sustentada pelo `_Reprovada` dentro do `atomic` da unidade. (`app/chamados/services/materializacao.py:1145`; `app/chamados/services/materializacao.py:1204`; `app/chamados/services/materializacao.py:1208`)

4. **OBSERVAÇÃO — delta de escritores e leitores em relação à v1.**

   | Campos | Quem escreve / quem lê no delta |
   |---|---|
   | **`materializada_em`, `materializada_por`, `via_resolucao`** | Nenhum escritor persistente novo. A atribuição continua em `_escrever_carimbo`; o v2 passa a preservar os três quando encontra carimbo existente. Esse estado agora alimenta sucesso nos oito consumidores, inclusive suas trilhas. (`app/chamados/models.py:2556`; `app/chamados/models.py:2602`, **v2 virtual**; `app/chamados/services/disputa_emissao.py:1683`; `app/chamados/reconciliador.py:1143`) |
   | **`resposta_colab`** | Continua escrita pelo core; não é escrita por `carimbar`. A comparação humana já existia na proposta v1; no v2 ela fica subordinada à ausência de carimbo. O C6 passa a lê-la também no ramo já carimbado. (`app/chamados/services/disputa_emissao.py:1282`; `app/chamados/models.py:2665`, **v2 virtual**; `app/chamados/models.py:2667`, **v2 virtual**; `app/chamados/juizes.py:1150`) |
   | **`respondida_em`, `validada_em`, `validada_por`** | Escritores permanecem core, validação e complemento da própria porta. O v2 deixa de completar esses campos no retorno antecipado. Logo `_CAMPOS_DO_HUMANO` não significa exclusividade humana de escrita. (`app/chamados/services/disputa_emissao.py:1283`; `app/chamados/services/disputa_emissao.py:1295`; `app/chamados/services/validacao.py:131`; `app/chamados/models.py:2611`, **v2 virtual**; `app/chamados/models.py:2602`, **v2 virtual**) |
   | **`veredito_lavrado`, `julgado_em`, `janela_expira_em`** | Escritor/restaurador continuam na porta da lavra. A fotografia mudou para depois da aprovação do C6; a recuperação passou a alcançar o commit próprio. Já carimbada retorna sem lavrar, mesmo com `lavrar_veredito=True`. (`app/chamados/veredito_lavrado.py:227`; `app/chamados/veredito_lavrado.py:235`; `app/chamados/veredito_lavrado.py:240`; `app/chamados/models.py:2622`, **v2 virtual**; `app/chamados/models.py:2602`, **v2 virtual**) |
   | **`_pv_memo`** | Escritor/leitor continuam em `pergunta_viva`. O novo ramo já carimbado executa C6 após o `pop` e pode deixar um memo novo. A chave continua sem cobrir ata/DNA/batidas; invalidar o memo não renova relações preservadas. (`app/chamados/juizes.py:811`; `app/chamados/juizes.py:835`; `app/chamados/juizes.py:847`; `app/chamados/models.py:2660`, **v2 virtual**; `app/chamados/models.py:2664`, **v2 virtual**) |

5. **OBSERVAÇÃO — a recuperação não usa fotografia inexistente, mas seu alcance transacional tem limites.**

   **Antes da fotografia:** `antes = lavra_antes = None` e as guardas do `except` evitam restauração inválida. Releitura, julgamento de carimbo existente, relógio e obtenção do autor precedem o carimbo novo; a lavra é fotografada antes de sua alteração. (`app/chamados/models.py:2594`, **v2 virtual**; `app/chamados/models.py:2607`, **v2 virtual**; `app/chamados/models.py:2622`, **v2 virtual**; `app/chamados/models.py:2629`, **v2 virtual**)

   **Commit do chamador:** quando há transação externa, a porta retorna antes desse commit. Uma falha posterior não atravessa seu `except`; a instância pode conservar estado posteriormente revertido. Essa limitação permanece. (`app/chamados/models.py:2596`, **v2 virtual**; `app/chamados/models.py:2627`, **v2 virtual**)

   **Depois de SQL efetivado:** se um `save()` concluir o UPDATE e algum hook lançar exceção Python depois, `nullcontext` não desfaz SQL. A porta restauraria memória e um chamador que capturasse a exceção poderia confirmar o banco. **Não encontrei esse caminho ordinário demonstrado nos receivers atuais**: o receiver da pergunta agenda trabalho protegido por `engolir`. Ocorrência concreta desse cenário: **[não verificado]**. (`app/chamados/models.py:2596`, **v2 virtual**; `app/chamados/models.py:2626`, **v2 virtual**; `app/chamados/models.py:2632`, **v2 virtual**; `app/chamados/signals.py:197`; `app/core/observ.py:85`)

6. **PROCEDENTE-candidato — os testes mordem, mas ainda há mutante plausível sobrevivente.** [nao verificado]

   **Mordidas por inspeção:** os quatro campos isolados defendem a guarda; o caso já carimbado/vivo exige consultar C6; a reentrada exige aceitar o carimbo anterior; a captura SQL exige trava antes do juiz; o teste transacional exige recuperação após falha de commit. Execução dessas mordidas: **[não verificado]**. (`app/chamados/tests/test_o261_item3_pep.py:99`, **anexo**; `app/chamados/tests/test_o261_item3_pep.py:144`, **anexo**; `app/chamados/tests/test_o261_item3_pep.py:163`, **anexo**; `app/chamados/tests/test_o261_item3_pep.py:263`, **anexo**; `app/chamados/tests/test_o261_item3_pep.py:307`, **anexo**)

   **Mutante:** restaurar lavra no `except` apenas quando `_cx.in_atomic_block`, mantendo recuperação do carimbo incondicional. Os testes antigos de falha de lavra/save herdam `TestCase`; o novo de commit não pede lavra nem verifica seus três campos. Assim, a cobertura apresentada deixa passar lavra residual em memória após commit próprio falho com `lavrar_veredito=True`. Execução do mutante: **[não verificado]**. (`app/chamados/models.py:2629`, **v2 virtual**; `app/chamados/tests/test_o261e_f2_fila.py:143`; `app/chamados/tests/test_o261e_f2_fila.py:190`; `app/chamados/tests/test_o261e_f2_fila.py:200`; `app/chamados/tests/test_o261_item3_pep.py:307`, **anexo**)

   **OBSERVAÇÃO — a reentrada nova é um teste de costura simulada:** substitui plantio e julgamento; não percorre o S143 real nem verifica os eventos dos outros consumidores. Também permanece sem defesa a diferença entre projeção legítima e relação obsoleta com a mesma FK. (`app/chamados/tests/test_o261_item3_pep.py:170`, **anexo**; `app/chamados/tests/test_o261_item3_pep.py:176`, **anexo**; `app/chamados/tests/test_o261_item3_pep.py:233`, **anexo**)

7. **OBSERVAÇÃO — enquadramento nas sete classes.**

   | Classe | Resultado |
   |---|---|
   | **(a) Segundo juiz** | **Não demonstrado.** Reutiliza o C6 registrado; comparar fotografia humana é pré-condição de concorrência. (`app/core/juizes.py:266`; `app/chamados/models.py:2602`, **v2 virtual**; `app/chamados/models.py:2667`, **v2 virtual**) |
   | **(b) Segundo escritor** | **Não há nova porta persistente demonstrada.** Há cópia dinâmica em memória; o selo declara não alcançar nomes vindos de atributo. (`app/chamados/models.py:2664`, **v2 virtual**; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:113`) |
   | **(c) Relógio fora da borda** | **Dívida mantida:** fallback `now()` e `encerrar_fantasma` sem repassar o instante recebido pelo consumidor. (`app/chamados/models.py:2605`, **v2 virtual**; `app/chamados/models.py:2685`, cópia; `app/docs/BACKLOG.md:1154`) |
   | **(d) Derivação paralela** | **Não demonstrada para vitalidade.** O problema encontrado é fornecer insumo relacionado obsoleto ao juiz existente e interpretar sua resposta como autoria do ato. (`app/chamados/models.py:2664`, **v2 virtual**; `app/chamados/juizes.py:1181`; `app/chamados/services/disputa_emissao.py:1683`) |
   | **(e) Selo/RED sem mordida** | **Cobertura parcial, não vacuidade total.** Há testes discriminantes e o mutante residual do item 6. (`app/chamados/tests/test_o261_item3_pep.py:99`, **anexo**; `app/chamados/tests/test_o261_item3_pep.py:307`, **anexo**) |
   | **(f) Ordem de trava** | **Incompleto.** Igualdade de `FOR UPDATE OF` e precedência ao C6 não provam ordem física entre P/D nem ausência de ciclo com chamado. Há gate que trava chamado antes de S143 e fechamento de disputa que alcança chamado. Deadlock concreto: **[não verificado]**. (`app/chamados/services/relavra_via.py:221`; `app/chamados/tests/test_o261_item3_pep.py:250`, **anexo**; `app/chamados/models.py:674`; `app/chamados/signals.py:42`; `app/chamados/signals.py:91`) |
   | **(g) Pouso antes do pré-requisito** | Cronologia de medição, RED/GREEN, suite e pouso: **[não verificado]**. A presença dos testes no anexo não comprova sua execução. (`app/chamados/tests/test_o261_item3_pep.py:250`, **anexo**) |

8. **PROCEDENTE-candidato — contraexemplo novo: `True` produz instante falso na trilha.**

   Considere o ramo sem credor com disputa aberta, pai/destinos fora de `VIVOS`, pergunta sem resposta nem carimbo e célula que permite ao C6 considerá-la morta. Esse estado alcança a tentativa de carimbo. (`app/chamados/services/disputa_emissao.py:1649`; `app/chamados/services/disputa_emissao.py:1653`; `app/chamados/services/disputa_emissao.py:1656`; `app/chamados/services/disputa_emissao.py:1663`; `app/chamados/juizes.py:1195`)

   - B já leu a pergunta e prepara a fotografia com `materializada_em=None`. (`app/chamados/services/disputa_emissao.py:1666`)
   - Antes de B entrar na porta, A grava `admin_decidiu` em `tA`. A porta de identidade grava instante, autor e via. (`app/chamados/models.py:2639`; `app/chamados/models.py:2644`, cópia)
   - B tenta carimbar em `tB`. O v2 relê o carimbo de A, não o altera e devolve `True`, pois o C6 aceita a identidade existente. (`app/chamados/models.py:2665`, **v2 virtual**; `app/chamados/models.py:2602`, **v2 virtual**; `app/chamados/juizes.py:1171`)
   - B emite `fecho_sem_credor` com ator B e **`depois.materializada_em=tB`**, enquanto a pergunta conserva **`materializada_em=tA` e autor A**. (`app/chamados/services/disputa_emissao.py:1678`; `app/chamados/services/disputa_emissao.py:1683`; `app/chamados/models.py:2602`, **v2 virtual**)

   **O carimbo preservado está correto; o

[truncado em 20000 de 20265 caracteres]

_2107 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
