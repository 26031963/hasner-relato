# CODEX-ESTADO

> Ultima resposta processada: **07/11/2026 12:18** · publicado em **07/11/2026 12:18** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 07/11 12:18 · sessao 5184053a97bf · - · - · origem=Ronald (tmux)

C — **PROCEDENTE: a ordem global ainda quebra em caminhos alcançados pelos cinco sítios.**

1. **`DisputaSupervisao.fechar`:** o corpo antecipa `self`, pai e destinos das vivas (`app/chamados/models.py:1874`). Porém `save()` dispara S88 (`app/chamados/models.py:1899`; `app/chamados/signals.py:42`), que reconcilia pai e dispara S143 (`app/chamados/signals.py:92`). O resolver trava **todas** as disputas abertas do colaborador, incluindo outras ausentes da lista inicial (`app/chamados/services/disputa_emissao.py:1663`). Havendo outra disputa aberta, ela entra depois do Chamado e possivelmente depois da Pergunta já atualizada (`app/chamados/models.py:1884`). O modelo permite múltiplas disputas abertas: seu `Meta` só declara índice, sem unicidade (`app/chamados/models.py:1765`).

2. **`fechar_o_fio_do_dia`:** comentário promete pais, mas a lista contém somente chamados vivos do dia e destinos; pais das disputas não são acrescentados (`app/ponto/services/veredito_celula.py:179`; `app/ponto/services/veredito_celula.py:184`). Depois do UPDATE das perguntas (`app/ponto/services/veredito_celula.py:186`), `reconciliar()` pode alcançar S143 e acrescentar pais e outras disputas (`app/ponto/services/veredito_celula.py:199`; `app/chamados/services/disputa_emissao.py:1663`).

3. **Validação:** o ramo `FOLGA_CONTESTA` desvia antes da trava nova (`app/chamados/services/validacao.py:404`). Folga pronta carimba Pergunta antes de chamar o fechamento do fio (`app/chamados/services/validacao.py:284`; `app/chamados/services/validacao.py:292`); folga nova também carimba primeiro (`app/chamados/services/validacao.py:312`). No ramo normal, a lista só inclui pai e destino (`app/chamados/services/validacao.py:423`), mas o materializador reconcilia clones de `chamados_relacionados_ids`, ausentes dela, depois do carimbo da Pergunta (`app/chamados/services/validacao.py:425`; `app/chamados/services/materializacao.py:965`; `app/chamados/services/materializacao.py:766`). O materializador geral roda após o atomic local e varre irmãs, sem cobertura da lista antecipada (`app/chamados/services/validacao.py:459`; `app/chamados/services/materializacao.py:1360`).

4. **`fechar_beco`:** trava somente `d` e os chamados coletados (`app/chamados/services/becos.py:51`), depois chama resolver (`app/chamados/services/becos.py:53`), que trava todas as disputas abertas (`app/chamados/services/disputa_emissao.py:1663`). Outra disputa aberta entra depois de Chamado.

5. **Resolver:** via S143, a nova trava chega tarde. `reconciliar()` já trava Chamado (`app/chamados/models.py:628`; `app/chamados/models.py:688`), salva (`app/chamados/models.py:291`) e executa S143 (`app/chamados/signals.py:92`); só então o wrapper trava Disputa (`app/chamados/services/disputa_emissao.py:1665`). Há também janela concorrente: `_disp` é coletada antes da trava do colaborador, enquanto o corpo posteriormente consulta novamente todas as abertas (`app/chamados/services/disputa_emissao.py:1663`; `app/chamados/services/disputa_emissao.py:1773`).

**Commit parcial / BUG120 — NÃO PROCEDE como regressão demonstrada.** S143 já envolve o resolver inteiro em savepoint próprio e captura a exceção fora dele; o atomic adicional não remove essa proteção (`app/chamados/signals.py:83`). `_retratar` e `fechar_beco` já envolviam o ato inteiro em atomic (`app/chamados/reconciliador.py:522`; `app/chamados/services/becos.py:41`). Existe chamador direto no fallback de `reconciliar_fantasmas`, mas dependência de sucesso parcial é [nao verificado] (`app/ponto/management/commands/reconciliar_fantasmas.py:280`).

H — **PROCEDENTE: cobertura insuficiente da ordem.** O instrumento memoriza tabelas, não linhas: qualquer segunda trava da mesma tabela desaparece (`app/chamados/tests/test_e1_v5_ordem_trava.py:38`; `app/chamados/tests/test_e1_v5_ordem_trava.py:45`). Assim não percebe clone ou segunda disputa travados após Pergunta. O teste do resolver fecha o chamado por `.update()` fora do trecho observado, chama o resolver diretamente e exige apenas Disputa e Chamado; pode passar sem escrita de Pergunta e não exercita S143 (`app/chamados/tests/test_e1_v5_ordem_trava.py:331`; `app/chamados/tests/test_e1_v5_ordem_trava.py:335`). O mutante cobre apenas `admin_encerra` (`app/chamados/tests/test_e1_v5_ordem_trava.py:337`).

I — **PROCEDENTE: ordem de trava e retorno contra a lâmpada permanecem.** Os caminhos síncronos Chamado → Pergunta, Disputa → materialização e Chamado → cartório seguem dentro de `post_save`, sem `on_commit` (`app/chamados/signals.py:92`; `app/chamados/signals.py:140`; `app/chamados/signals.py:180`). Beco e retomada de regularizações usam `on_commit`, portanto o cumprimento é parcial (`app/chamados/signals.py:121`; `app/chamados/signals.py:202`).

## 07/11 12:18 · sessao 52e5ef2b6986 · FIO-E0-V4-E2 · verificador · origem=Code

**Há achados procedentes de horizonte incompleto e de relógio transitivo fora da borda. Não achei violação do teto no ramo novo `nao_venceu`, nem segundo escritor no caminho da calibração da âncora.** (`app/chamados/services/tarefa_do_dia.py:202`, `app/chamados/services/tarefa_do_dia.py:305`, `app/ponto/catalogo/ausencias.py:275`, `app/ponto/services/cartorio.py:858`, `app/escala/signals.py:161`)

A atribuição histórica exata de cada alteração aos três commits, os RED/GREEN originais, a suíte Django e a ocorrência dos contraexemplos em produção estão **[nao verificado]**. [nao verificado]

1. **Retratação posterior ainda muda a tarefa do passado — classe: horizonte incompleto; procedente.**

   O filtro de decisões corta `ato_em > agora`, mas `_tarefa_pura` descarta primeiro a pergunta pela **`via_resolucao` atual**. Portanto, cortar a decisão de retratação não basta para recuperar a pergunta anterior. (`app/chamados/services/tarefa_do_dia.py:301`, `app/chamados/services/tarefa_do_dia.py:202`)

   A porta `remover_motivo` registra a decisão de retratação e muda a via para motivo retratado. Se esse ato aconteceu depois do `agora` consultado, a via atual continua sendo lida: com marco vencido e apagado, a pergunta anterior desaparece da seleção e nasce `sem_pergunta/perguntar`. (`app/chamados/models.py:1989`, `app/chamados/models.py:1994`, `app/chamados/services/tarefa_do_dia.py:251`)

   Confirmei, executando os corpos extraídos em memória, a mudança de `aguarda_validacao/validar` para `sem_pergunta/perguntar`, mantendo a mesma sequência anterior a `agora` e trocando apenas a via atual. Esses resultados decorrem dos ramos citados; ocorrência produtiva **[nao verificado]**. (`app/chamados/services/tarefa_do_dia.py:129`, `app/chamados/services/tarefa_do_dia.py:202`, `app/chamados/services/tarefa_do_dia.py:253`)

   Há ainda uma incompatibilidade com usar `materializada_em` como relógio da última alteração da via: `manter_carimbo=True` pode trocar a via preservando o instante antigo. Esse campo é o instante do **carimbo preservado**, não necessariamente da última mudança de identidade; `marcos_do_lote` agrega somente esse instante. (`app/chamados/models.py:2738`, `app/chamados/veredito_lavrado.py:176`)

2. **Passivo posterior atravessa o horizonte — classe: horizonte incompleto; procedente.**

   A carga chama `linhas_da_projecao_sem_linha(_ps)` sem `agora`. A autoridade lê os carimbos atuais `respondida_em/validada_em` e procura correspondência na sequência inteira, também sem corte temporal. (`app/chamados/services/tarefa_do_dia.py:305`, `app/chamados/models.py:3409`, `app/chamados/models.py:3412`)

   Contraexemplo: projeção respondida às 22h, sem linha correspondente, consulta às 21h. A função devolve passivo com `ato_em=22h`; `_pela_decisao` consulta esse passivo antes de verificar se havia decisão e responde `passivo_sem_linha/nao_sei`, em vez de `aguarda_resposta/responder`. Confirmei esses dois resultados nos corpos extraídos em memória. (`app/chamados/models.py:3416`, `app/chamados/models.py:3418`, `app/chamados/services/tarefa_do_dia.py:121`, `app/chamados/services/tarefa_do_dia.py:123`)

   Esse limite não está entre os três declarados no docstring do horizonte. (`app/chamados/services/tarefa_do_dia.py:273`)

3. **Ausência e absorção usam status atual — classe: horizonte incompleto; procedente.**

   A cobertura não corta o nascimento da ausência por `criado_em`, embora esse instante exista. Ela seleciona pelo período e pelo status atual — para cobrança, exclui somente `rejeitada`. Uma ausência criada depois de `agora` pode, assim, suspender a cobrança daquele instante passado. (`app/ponto/turnos.py:3072`, `app/ponto/turnos.py:2761`, `app/ponto/turnos.py:2519`, `app/ponto/models.py:813`)

   A decisão de absorção é cortada por `ato_em`, mas sua FK é carregada com o estado atual da ausência. `_pela_decisao` lê diretamente `ult.absorcao_ausencia.status`; uma aprovação, rejeição ou entrega de documento posterior pode mudar o dono da tarefa histórica. (`app/chamados/services/tarefa_do_dia.py:302`, `app/chamados/services/tarefa_do_dia.py:145`)

   O status efetivamente muda pela porta `mudar_estado`; a leitura da tarefa não usa `aprovada_em` nem reconstrói o status anterior. Não achei corte temporal nesses caminhos. (`app/ponto/services/ausencia.py:808`, `app/ponto/services/ausencia.py:824`, `app/ponto/models.py:810`, `app/chamados/services/tarefa_do_dia.py:233`)

4. **Fila, cadastro e identidade continuam atuais — classe: horizonte incompleto; procedente.**

   | Fato lido | O que ainda não é cortado por `agora` |
   |---|---|
   | **Fila** | `agora` entra somente na comparação com `silenciado_ate`. `status_local` continua atual, e não há corte por nascimento do chamado nessa seleção. Resolver ou silenciar posteriormente pode alterar a porta histórica. (`app/chamados/services/painel.py:155`, `app/chamados/services/painel.py:188`, `app/chamados/services/tarefa_do_dia.py:321`) |
   | **Âncora da pergunta** | `chamado_que_lista` usa o destino/container atual; a tarefa não recupera o vínculo anterior da pergunta com o chamado. (`app/chamados/services/fio.py:116`, `app/chamados/services/tarefa_do_dia.py:320`) |
   | **Cadastro** | A carga lê o colaborador atual. A grade aplica admissão, demissão e isenção atuais; não recebe `agora`. Demissão retroativa ou alteração posterior de isenção podem mudar a resposta histórica. (`app/chamados/services/tarefa_do_dia.py:286`, `app/escala/services/leitor_celula.py:360`, `app/escala/services/leitor_celula.py:365`, `app/escala/services/leitor_celula.py:389`) |
   | **Identidade/data da pergunta** | A seleção usa `conversa` e `data_evento` atuais. Existe porta que altera `data_evento`; cortar decisões não recupera a identidade anterior. (`app/chamados/services/tarefa_do_dia.py:290`, `app/chamados/services/fio.py:859`, `app/chamados/services/fio.py:898`) |
   | **Vínculo e catálogo de ausência** | Os vínculos são carregados do cadastro atual; o conjunto de tipos em aberto vem do catálogo vigente, sem versão em `agora`. (`app/ponto/turnos.py:2990`, `app/ponto/turnos.py:2762`, `app/ponto/catalogo/ausencias.py:267`) |

   Portanto, “todo fato carregado é o que existia em `agora`” é mais amplo que o comportamento implementado; os limites excedem nascimento da pergunta, retratação de Batida e história da ata. (`app/chamados/services/tarefa_do_dia.py:273`)

5. **79315d64: relógio real escapa pelos helpers — classe: relógio fora da borda / selo incompleto; procedente.**

   O caminho encontrado é:

   `tarefa → cobertura → TIPOS_EM_ABERTO → _SetVivo → em_aberto_vigentes → vigente → time.monotonic()`.

   A tarefa chama a cobertura; o filtro e o corpo da cobertura usam o conjunto vivo; esse conjunto chega a `vigente()`, que lê `monotonic` e atualiza o cache. (`app/chamados/services/tarefa_do_dia.py:354`, `app/ponto/turnos.py:2762`, `app/ponto/turnos.py:2774`, `app/ponto/catalogo/ausencias.py:403`, `app/ponto/catalogo/ausencias.py:433`, `app/ponto/catalogo/ausencias.py:275`)

   É relógio para TTL do catálogo, não para julgar diretamente o vencimento do marco. Entretanto, o próprio selo inclui expressamente `time.monotonic` entre as leituras proibidas. (`app/ponto/catalogo/ausencias.py:276`, `app/chamados/tests/test_e2_tarefa_do_dia.py:890`)

   O detector AST examina apenas os módulos da tarefa e da lavra. O veneno dinâmico intercepta somente `django.utils.timezone.now`. Ambos deixam passar esse helper real. Confirmei também que `vigente()` lê `monotonic` mesmo com cache preenchido. (`app/chamados/tests/test_e2_tarefa_do_dia.py:958`, `app/chamados/tests/test_e2_tarefa_do_dia.py:680`, `app/ponto/catalogo/ausencias.py:275`)

   **“Não morde os mutantes declarados” não procede para o detector isolado:** as 16 formas declaradas foram identificadas na execução em memória. A falha é de alcance transitivo. Não achei `timezone.now/localdate` sem instante ou `date.today` no caminho efetivo da tarefa que rastreei; achei o `monotonic` acima. (`app/chamados/tests/test_e2_tarefa_do_dia.py:966`, `app/escala/services/leitor_celula.py:350`, `app/escala/utils.py:2256`, `app/escala/alimentacao.py:269`, `app/chamados/services/painel.py:190`)

6. **57acffd2: `nao_venceu` sem lâmpadas — classe: silenciamento indevido por teto; não procedente no ramo examinado.**

   A lei diz comparar **INSTANTE OU exigir FATO ENCERRADO**. Não exige os dois simultaneamente. O corpo do cartório compara `quando + teto <= agora`, preservando o instante do marco. (`CLAUDE.md:185`, `app/ponto/services/cartorio.py:858`)

   O ramo novo exige marcos **não vazios** e nenhum vencido. Se houver qualquer vencido, ou se os marcos forem `[]`/`None`, responde `ata_sem_lampadas/nao_sei`. Não achei marco vencido silenciado por esse ramo. (`app/chamados/services/tarefa_do_dia.py:187`, `app/chamados/services/tarefa_do_dia.py:193`)

   No cross-meia-noite, `marcos_turno` encadeia datetimes, avançando o dia quando necessário. A saída conserva o instante do dia seguinte; `data_civil` só é extraída depois. O vencimento é inclusivo na igualdade com o teto. (`app/escala/servico_jornada.py:61`, `app/escala/servico_jornada.py:81`, `app/ponto/services/cartorio.py:858`, `app/chamados/services/tarefa_do_dia.py:96`)

7. **d912345b: calibração da âncora — classe: segundo escritor fora da porta; não achei.**

   O signal reconhece `None → valor` e chama `DisputaSupervisao.remover_motivo`. A consequência **é escrita**: registra retratação, carimba identidade, remove motivo/contexto e, quando cabível, fecha pela porta do ciclo. (`app/escala/signals.py:143`, `app/escala/signals.py:161`, `app/chamados/models.py:1989`, `app/chamados/models.py:1994`, `app/chamados/models.py:1996`, `app/chamados/models.py:2005`)

   A sequência passa por `DecisaoPergunta.registrar`; o carimbo passa por `carimbar_identidade → _escrever_carimbo`. Não achei escrita paralela no signal. (`app/chamados/models.py:3428`, `app/chamados/models.py:2658`, `app/chamados/models.py:2738`)

   O trecho do diff da âncora altera a testemunha: exige uma pergunta conservada/retratada e uma decisão de retratação, substituindo a expectativa de exclusão. Esse teste não cria escritor de produção. (`app/escala/tests/test_signal_ancora.py:52`, `app/escala/tests/test_signal_ancora.py:59`, `app/escala/tests/test_signal_ancora.py:62`)

8. **Selos, fonte única e consultas — resultado por classe.**

   | Classe | Procedência |
   |---|---|
   | **Segundo juiz duplicando autoridade registrada** | **Não achei nos caminhos examinados.** Há corte e registro para `_C19`; a coexistência dos cinco respondedores até E3 está declarada. Teto e vínculo delegam às autoridades existentes. (`app/docs/CORTES.md:424`, `app/core/juizes.py:285`, `app/core/juizes.py:153`, `app/ponto/turnos.py:3052`, `app/ponto/turnos.py:3059`) |
   | **Universo de `batidas_apuraveis` rederivado** | **Não achei.** O efeito chama a porta e acrescenta origem/horizonte. As agregações de invalidação usam Batida crua porque precisam incluir retratações; não constituem o universo de apuração. (`app/chamados/services/tarefa_do_dia.py:316`, `app/ponto/turnos.py:2876`, `app/chamados/veredito_lavrado.py:151`, `app/chamados/veredito_lavrado.py:154`) |
   | **Selo do horizonte sem mordida sobre os dois filtros** | **Não procede por leitura contrafactual.** Sem cortar decisões, a resposta/validação futura contraria os resultados exigidos; sem cortar criação da Batida, o efeito futuro contraria `encerrada_pelo_juiz`. Execução desses testes com Django: **[nao verificado]**. (`app/chamados/tests/test_e2_tarefa_do_dia.py:710`, `app/chamados/tests/test_e2_tarefa_do_dia.py:720`, `app/chamados/tests/test_e2_tarefa_do_dia.py:730`) |
   | **Selo T sem mordida sobre a cura nova** | **Não procede.** Remover o ramo em memória faz 08h10 voltar a `ata_sem_lampadas/nao_sei`, contrariando a asserção. U/C/L testam mecanismos distintos e não precisam falhar sem essa cura. (`app/chamados/tests/test_e2_tarefa_do_dia.py:793`, `app/chamados/tests/test_e2_tarefa_do_dia.py:773`, `app/chamados/tests/test_e2_tarefa_do_dia.py:797`, `app/chamados/tests/test_e2_tarefa_do_dia.py:815`) |
   | **Queries crescem com N** | **Não achei prova de crescimento nos caminhos inspecionados.** O teste inclui validações e compara lote maior com menor; não exercita ausência, absorção, âncora ou ata sem lâmpadas. Não certifica esses cenários. (`app/chamados/tests/test_e2_tarefa_do_dia.py:861`, `app/chamados/tests/test_e2_tarefa_do_dia.py:869`, `app/chamados/tests/test_e2_tarefa_do_dia.py:886`) |

   A leitura do conjunto vivo por ausência não prova uma query por ausência: `vigente()` tem cache TTL de 60 segundos. A contagem pode depender desse cache; não medi SQL nesta cópia. (`app/ponto/turnos.py:2774`, `app/ponto/catalogo/ausencias.py:257`, `app/ponto/catalogo/ausencias.py:276`)

9. **Censo dos campos e significados usados pelo diff.**

   | Campo/valor | Quem escreve | Quem lê e significado |
   |---|---|---|
   | **`DecisaoPergunta.ato_em`** | Porta `registrar`; recebe resposta, reabertura/recusa, validação, devolução, absorção, silêncio, retratação e recuperação histórica. (`app/chamados/models.py:3428`, `app/chamados/services/disputa_emissao.py:1369`, `app/chamados/services/disputa_emissao.py:1480`, `app/chamados/services/validacao.py:139`, `app/chamados/services/validacao.py:205`, `app/chamados/services/materializacao.py:560`, `app/chamados/models.py:1870`, `app/chamados/models.py:1990`, `app/chamados/models.py:3418`) | Ordenação da sequência, histórico da tela, correspondência do passivo e tarefa. É **instante do ato**, podendo ser recuperado de fato antigo; não é nascimento físico da linha. (`app/chamados/models.py:3374`, `app/chamados/models.py:2993`, `app/chamados/models.py:3413`, `app/chamados/services/tarefa_do_dia.py:301`, `app/chamados/services/disputa_emissao.py:1524`) |
   | **`Batida.criado_em`** | `auto_now_add`; inserção pela porta de registro. (`app/ponto/models.py:79`, `app/ponto/registro_batida.py:152`, `app/ponto/registro_batida.py:157`) | Tarefa e invalidadores usam o nascimento da linha, com `<=agora/ate`; não usam o instante da marcação `timestamp`. (`app/chamados/services/tarefa_do_dia.py:316`, `app/chamados/veredito_lavrado.py:151`) |
   | **`Batida.retratada_em`** | `retratar_batida → anotar`; desfazer retratação também passa por anotação. (`app/ponto/registro_batida.py:274`, `app/ponto/registro_batida.py:252`, `app/ponto/services/retratar_humano.py:207`) | `batidas_apuraveis` lê participação **atual**, exigindo NULL; `marcos_do_lote` lê o instante da retratação até `ate`. São usos distintos e o limite histórico da porta está declarado. (`app/ponto/turnos.py:2876`, `app/chamados/veredito_lavrado.py:154`, `app/chamados/services/tarefa_do_dia.py:313`) |
   | **`PerguntaDisputa.veredito_lavrado`** | `lavrar` pergunta ao juiz do fechamento; backfill também calcula/atribui e persiste pela porta de lote. (`app/chamados/veredito_lavrado.py:236`, `app/chamados/veredito_lavrado.py:244`, `app/chamados/management/commands/backfill_veredito_lavrado.py:138`, `app/chamados/veredito_lavrado.py:224`) | Tarefa aceita somente `sim/nao` vigentes; `esta_stale`, varredura e auditoria verificam confiança na lavra. (`app/chamados/services/tarefa_do_dia.py:309`, `app/chamados/veredito_lavrado.py:196`, `app/chamados/services/raiox.py:40`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1165`) |
   | **`PerguntaDisputa.julgado_em`** | Mesmos escritores da lavra; instante em que foi lavrada. (`app/chamados/veredito_lavrado.py:245`, `app/chamados/management/commands/backfill_veredito_lavrado.py:139`) | Lavra futura é rejeitada; insumo invalida quando estritamente posterior, `marco > julgado_em`. A data usada na expiração é local. (`app/chamados/veredito_lavrado.py:196`, `app/chamados/veredito_lavrado.py:202`, `app/chamados/veredito_lavrado.py:206`) |
   | **`PerguntaDisputa.janela_expira_em`** | `expira_em(data_evento, hoje)` alimenta lavra/backfill. (`app/chamados/veredito_lavrado.py:95`, `app/chamados/veredito_lavrado.py:246`, `app/chamados/management/commands/backfill_veredito_lavrado.py:140`) | É **data de competência**, não instante nem dia do turno. Expira com `<= data local de agora`, quando a lavra é de data local anterior. (`app/chamados/models.py:2399`, `app/chamados/veredito_lavrado.py:198`, `app/chamados/veredito_lavrado.py:202`) |
   | **`PerguntaDisputa.via_resolucao/materializada_em`** | `_escrever_carimbo`, usado por `carimbar`, `carimbar_identidade` e descarimbo. (`app/chamados/models.py:2658`, `app/chamados/models.py:2664`, `app/chamados/models.py:2727`, `app/chamados/models.py:2748`) | Juiz lê via+carimbo; tarefa lê via atual; invalidadores leem o instante. **Não equivale necessariamente à última alteração da via**, conforme achado 1. (`app/chamados/juizes.py:648`, `app/chamados/services/tarefa_do_dia.py:202`, `app/chamados/veredito_lavrado.py:176`, `app/chamados/models.py:2738`) |
   | **`PerguntaDisputa.data_evento`** | Abertura, preenchimento na costura e porta `rechavear_data_evento`. (`app/chamados/models.py:2578`, `app/chamados/services/fio.py:838`, `app/chamados/services/fio.py:898`) | Tarefa e ajuste overnight assumem **dia do turno**; a data civil do marco resulta da cadeia de instantes. Não achei releitura como dia civil no código novo; migração efetiva do acervo **[nao verificado]**. (`app/chamados/services/tarefa_do_dia.py:197`, `app/chamados/juizes.py:301`, `app/chamados/juizes.py:319`) |
   | **`CelulaDia.veredito_em`** | `lavrar_veredito`; não anda em toda operação sobre a ata. (`app/ponto/portas/celula.py:1032`, `app/ponto/portas/celula.py:997`) | `marcos_do_lote` agrega esse instante até `ate`; ele não é histórico completo da ata. (`app/chamados/veredito_lavrado.py:165`, `app/ponto/portas/celula.py:1001`) |
   | **`CelulaDia.ata`** | Lavra e operação de precedência dentro da porta da célula. (`app/ponto/portas/celula.py:1028`, `app/ponto/portas/celula.py:1071`) | O leitor único transporta fotografia atual, lâmpadas e insumos; a tarefa recebe essa grade. Não há reconstrução da ata em `agora`. (`app/escala/services/leitor_celula.py:379`, `app/escala/services/leitor_celula.py:413`, `app/chamados/services/tarefa_do_dia.py:331`) |
   | **`marcos_com_teto: tipo, instante, vencido`** | Corpo do cartório usa `marcos_turno` e calcula `instante+teto<=agora`. (`app/ponto/services/cartorio.py:849`, `app/ponto/services/cartorio.py:858`) | `marcos_vencidos` separa/ formata; carga delega; tarefa casa tipo+hora consumindo na ordem e lê vencimento/data civil. `tipo` é E/S, instante é datetime e vencimento é inclusivo. (`app/ponto/services/cartorio.py:843`, `app/ponto/turnos.py:3056`, `app/chamados/services/tarefa_do_dia.py:63`, `app/chamados/services/tarefa_do_dia.py:96`) |

   **Resposta direta sobre os dois cortes:** ambos usam comparação inclusiva de instantes, como `marcos_do_lote(ate=)`. Porém, `ato_em` é tempo do ato e `criado_em` é nascimento da Batida; não representam o mesmo fato. Além disso, `marcos_do_lote` **não lê `DecisaoPergunta.ato_em`**: agrega criação/retratação de Batida, carimbos da célula e materialização/resposta da projeção da pergunta. (`app/chamados/services/tarefa_do_dia.py:301`, `app/chamados/services/tarefa_do_dia.py:316`, `app/chamados/veredito_lavrado.py:137`, `app/chamados/veredito_lavrado.py:151`, `app/chamados/veredito_lavrado.py:165`, `app/chamados/veredito_lavrado.py:176`, `app/chamados/veredito_lavrado.py:180`)

   A configuração usa `USE_TZ=True` e fuso padrão `America/Sao_Paulo`; a correção nova usa datas locais explicitamente. Fuso e consistência de cada registro produtivo **[nao verificado]**. (`app/config/settings/base.py:93`, `app/config/settings/base.py:95`, `app/chamados/veredito_lavrado.py:198`)

Os “337 dias” e a correspondência das “27 classes” com linhas produtivas permanecem **[nao verificado]**: aparecem como descrição dos casos, sem as linhas da sonda no dado fornecido. (`app/chamados/tests/test_e2_tarefa_do_dia.py:7

[truncado em 20000 de 20055 caracteres]

## 07/11 12:18 · sessao a014a70d2d37 · - · - · origem=Ronald (tmux)

**F — PROCEDENTE para “em todo caminho”; NÃO PROCEDE no caminho CLI estável.**

- O CLI mede mérito/exportada e recusa antes de chamar `aplicar`: `app/chamados/management/commands/backfill_decisoes_pergunta.py:283-291`. O SILENCIO vem antes das retratações: `:139-151`.
- `aplicar(grupos, sem_ato)` direto escreve e conclui os atomics de SILENCIO antes de examinar mérito; depois apenas acrescenta `merito_recusado` e continua outros grupos: `:139-163`. O teste legitima chamada direta, mas passa `sem_ato=[]`, ocultando precisamente esse cenário: `app/chamados/tests/test_e1_v6_horizonte.py:196-203`.
- Uma perdedora `admin_decidiu` com resposta e sem validação entra em `admin_decidiu_sem_ato`: `backfill_decisoes_pergunta.py:90-95`; assim, chamada direta com a lista real pode gravar SILENCIO antes de recusar seu grupo: `:147-163`.
- A função `aplicar` não verifica competência exportada; a guarda existe só em `handle`: `:125-172`, `:238-243`, `:283-286`. Uso direto em produção e mudança concorrente entre medição e aplicação: **[nao verificado]**.
- O CLI não conserva uma transação que una preflight e aplicação; mérito é relido sob trava, exportada não: `:245-249`, `:283-291`, `:152-163`. Portanto, a garantia de recusa inteira não cobre alteração concorrente após o preflight: mesmos pontos; ocorrência **[nao verificado]**.

**H — PROCEDENTE: há lacunas nos selos e relógios soltos.**

- O rastreador registra somente a **primeira ocorrência por tabela** e ignora toda trava posterior daquela tabela: `app/chamados/tests/test_e1_v5_ordem_trava.py:38-47`. Logo, não detecta outro Chamado/Disputa adquirido depois da Pergunta se a tabela já apareceu antes: `:45-47`.
- O RED de `resolver_perguntas_por_chamado_resolvido` só exige Disputa e Chamado, sem exigir Pergunta, alteração persistida ou retorno: `:326-335`. O wrapper adquire essas tabelas antecipadamente: `app/chamados/services/disputa_emissao.py:1662-1667`; o corpo pode pular pergunta viva: `:1712-1713`. O teste admite passagem sem exercitar o fechamento anunciado: esses pontos.
- O mutante prova a remoção completa da subordem em um caso de `admin_encerra`, mas não cobre aquisição tardia de outra linha da mesma tabela: `test_e1_v5_ordem_trava.py:337-346`, combinado com `:38-47`.
- A declaração “Sem relógio solto: todo instante é literal” é falsa: `app/chamados/tests/test_e1_v6_horizonte.py:14`. A fixture cria disputa com `aberta_em=auto_now_add`: `:73-74`, `app/chamados/models.py:1709`; chama `fechar_por_admin` sem congelamento: teste `:174`, `:185`, `:201`, método lê `timezone.now()` em `models.py:2733`.
- Os testes do contador dependem da hora real de abertura e do `applied` real da migration sem fixar essa fronteira: `test_e1_v6_horizonte.py:210-224`, `app/chamados/services/fio.py:1097-1103`. O caso “2020 = acervo” também supõe que a 0052 foi aplicada depois de 2020: teste `:221-224`, comparação `fio.py:1101`.
- Os testes herdados de fábrica passam HOJE/AGORA literais ao leitor, mas a fixture lavra a célula sem `agora`, usando relógio real para `veredito_em`: `test_e1_v6_horizonte.py:262-294`, `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:65-77`, `app/ponto/portas/celula.py:1032`.
- O teste de condição inspeciona presença de folhas no `Q` do modelo; não comprova a estrutura booleana inteira nem executa a condição congelada da migration: `test_e1_v6_horizonte.py:47-60`. Os testes reais de banco abaixo/no horizonte exercitam MARCO: `:94-102`; BATIDA não recebe o mesmo teste de fronteira nesse arquivo: `:132-139`, `:226-232`. [nao verificado]

**H — NÃO PROCEDE quanto a vacuidade dos demais REDs examinados.**

- Horizonte, porta antiga, canônica e backfill têm testemunhas positivas e verificam identidade/contagens concretas: `test_e1_v6_horizonte.py:94-130`, `:146-161`.
- SILENCIO verifica a linha exata e a via duplicata; mérito verifica `SystemExit(4)` e preservação da via: `:171-194`. Esses testes não cobrem o preflight concorrente nem a lista `sem_ato` não vazia no helper direto: `:196-203`. [nao verificado]
- Emissor verifica mesma disputa, dois IDs distintos de batida e repetição sem terceira pergunta: `:245-259`. O caso sem origem não é exercitado nesse teste: `:250-259`. [nao verificado]
- Fábrica e marco perguntado têm controles positivos e negativos: `:272-294`.
- O RED do fio mudo exige primeira pergunta e, após retratação, segunda pergunta e saída `FIO MUDO: 1`: `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:169-175`; a classe congela o relógio: `:44`.
- A API passa a criar perguntas de motivos/instâncias diferentes e continua verificando materialização da irmã: `app/api/tests/test_falta_na_resposta.py:29-41`, `:62-67`. Não prova o índice do horizonte, porque usa `2026-05-28`: `:10`.

Execução dos testes, resultados e ocorrência em produção: **[nao verificado]**. [nao verificado]

## 07/11 12:14 · sessao f03c646da6ac · - · - · origem=Ronald (tmux)

- **Teto temporal — não procedente como defeito do ramo novo.** A lei permite comparar **instante OU exigir fato encerrado**, não exige ambos (`CLAUDE.md:185`). O teto compara `quando + 30 minutos <= agora`, inclusivo na igualdade (`app/ponto/services/cartorio.py:828`, `:858`). Sem lâmpadas, `nao_venceu` exige marcos **não vazios** e nenhum vencido; qualquer vencido, `[]` ou `None` resulta em `ata_sem_lampadas/nao_sei` (`app/chamados/services/tarefa_do_dia.py:187`, `:193`). Não achei silenciamento de marco vencido nesse ramo.

- **Cross-meia-noite — não achei corte por data.** `marcos_turno` constrói datetime aware e avança o dia enquanto o próximo instante não for posterior ao anterior (`app/escala/servico_jornada.py:61`, `:81`). `marcos_com_teto` preserva esse instante; o leitor transforma em data civil apenas para a saída (`app/ponto/services/cartorio.py:858`; `app/chamados/services/tarefa_do_dia.py:96`). Executei esses corpos extraídos por AST, em memória e com dependências simuladas: turno 21/10 22h–22/10 06h mantém a saída em 22/10; vence às 06h30 inclusive. Execução da suíte Django: **[nao verificado]**.

- **Selo T sem mordida — não procedente para a cura nova.** O caso exige `(instancias=[], nao_sei=False, razao='nao_venceu')` às 08h10 e `nao_sei` às 09h05 (`app/chamados/tests/test_e2_tarefa_do_dia.py:788`, `:793`, `:795`). Removi somente o ramo novo em AST na memória: o primeiro resultado virou `ata_sem_lampadas/True`, contrariando a asserção. As classes U/C/L não dependem dessa cura; exercitam outros mecanismos (`:773`, `:797`, `:815`). A atribuição das “337” linhas de produção ao ramo: **[nao verificado]**, pois só consta como prosa (`:756`).

- **Segundo juiz / universo paralelo de batidas — não achei nos trechos inspecionados.** O novo juiz tem corte explícito e registro `_C19`; a coexistência dos cinco respondedores até E3 é declarada (`app/docs/CORTES.md:424`; `app/core/juizes.py:150`, `:285`). Teto e vínculo delegam às autoridades existentes (`app/ponto/turnos.py:3052`, `:3059`); a leitura do efeito chama `batidas_apuraveis(None)` e acrescenta identificação/horizonte, preservando o predicado único de retratação (`app/chamados/services/tarefa_do_dia.py:316`; `app/ponto/turnos.py:2876`). Não confundir esse efeito com os marcos de invalidação, que legitimamente incluem batidas retratadas (`app/chamados/veredito_lavrado.py:151`, `:154`).

- **Queries crescem com N — não achei prova.** O selo compara 2 colaboradores × 3 dias × 3 perguntas com 1 × 1 × 1, incluindo decisões de validação (`app/chamados/tests/test_e2_tarefa_do_dia.py:861`, `:869`, `:879`, `:886`). Os caminhos novos fornecem células e escalas completas, carregam disputa e ausência por `select_related`, e usam IDs para a âncora (`app/chamados/services/tarefa_do_dia.py:293`, `:302`, `:331`; `app/chamados/services/fio.py:116`). Os helpers só consultam células/escalas quando a alimentação é `None` (`app/escala/utils.py:2179`, `:2189`; `app/escala/models.py:1117`, `:1182`). O selo **não exercita ausências, absorção, âncoras nem ata sem lâmpadas**: sua preparação contém lâmpadas e três validações, sem esses fatos (`app/chamados/tests/test_e2_tarefa_do_dia.py:864`, `:869`). Contagem efetiva da suíte: **[nao verificado]**.

- **Falsa pista N+1 / dependência real do relógio — procedente apenas a segunda.** `_cobertura_pura` consulta o conjunto vivo para cada ausência sem `data_fim` (`app/ponto/turnos.py:2774`); `_SetVivo.__contains__` chama o catálogo (`app/ponto/catalogo/ausencias.py:403`, `:430`). Entretanto, `vigente()` tem cache TTL de 60 segundos, portanto não há fundamento para afirmar uma query por ausência (`:257`, `:267`, `:276`). Há leitura transitiva real de `time.monotonic()` e dependência do cache (`:274`, `:275`), inclusive pelo filtro `_q_cobertura`, que consome o mesmo conjunto vivo (`app/ponto/turnos.py:2762`). A designação “corpo puro” da cobertura é inexata nessa dependência (`:2765`).

## 07/11 12:14 · sessao 546e98ac8399 · - · - · origem=Ronald (tmux)

Achados do eixo escritores/semântica, na cópia indicada:

1. **Classe: horizonte incompleto — procedente.** A retratação futura pode apagar uma pergunta do passado mesmo com `ato_em__lte=agora`: a seleção descarta a **via atual** antes de considerar a sequência cortada (`app/chamados/services/tarefa_do_dia.py:202`, `:204`). `remover_motivo` registra retratação e troca a via no mesmo ato (`app/chamados/models.py:1985`, `:1989`, `:1994`). Assim, com lâmpada apagada e marco vencido, a pergunta antes existente vira `sem_pergunta` e `perguntar` (`app/chamados/services/tarefa_do_dia.py:250`). A existência de caso produtivo correspondente é **[nao verificado]**.

2. **Classe: segundo escritor fora da porta — não achei no caminho da calibração.** O signal identifica `None → valor` e chama `DisputaSupervisao.remover_motivo` (`app/escala/signals.py:143`, `:161`). Essa porta:
   - registra a decisão por `DecisaoPergunta.registrar` (`app/chamados/models.py:1989`), cujo único corpo de escrita é `bulk_create` (`:3428`);
   - chama `carimbar_identidade` (`:1994`), que usa `_escrever_carimbo` (`:2658`, `:2738`) e salva (`:2746`);
   - remove motivo/contexto (`:1996`) e fecha pela porta `fechar` (`:2005`).

   O diff anexado altera o **teste do signal**, exigindo uma pergunta conservada/retratada e uma decisão, em vez de exigir exclusão (`app/escala/tests/test_signal_ancora.py:52`, `:59`, `:62`). Não achei alteração do escritor de produção nesse trecho do diff; atribuição histórica exata ao commit d912345b sem objetos Git é **[nao verificado]**.

3. **Classe: campo tratado como nascimento — ressalva procedente.** `ato_em` significa instante do **ato**, não instante físico da inserção da linha: os escritores recuperam instantes antigos da projeção (`app/chamados/models.py:3418`, `:3422`) e da trilha (`app/chamados/services/disputa_emissao.py:1524`, `:1540`). `criado_em` da Batida é `auto_now_add`, portanto nascimento da linha (`app/ponto/models.py:79`). O filtro usa o mesmo operador inclusivo `<=`, mas a frase “existia em agora” tem essas duas referências temporais diferentes (`app/chamados/services/tarefa_do_dia.py:301`, `:316`). Não achei neste trecho uma troca indevida de dia do turno por dia civil.

4. **Classe: semântica de carimbo/invalidadores — procedente como limite do horizonte.** A via pode mudar mantendo `materializada_em` antigo (`app/chamados/models.py:2738`). Portanto `materializada_em` é instante do carimbo preservado, não necessariamente instante da última alteração da via. `marcos_do_lote` só agrega esse instante, não o instante da alteração de via nem `DecisaoPergunta.ato_em` (`app/chamados/veredito_lavrado.py:176`, `:180`). Isso reforça o achado 1.

Censo dos campos solicitado:

| Campo | Escrita comprovada | Leitura relevante e significado |
|---|---|---|
| `DecisaoPergunta.ato_em` | Porta `registrar` (`app/chamados/models.py:3428`); chamadores: resposta (`app/chamados/services/disputa_emissao.py:1369`), reabertura/recusa (`:1478`), resposta automática (`:1519`), trilha (`:1540`), validação (`app/chamados/services/validacao.py:139`), devolução (`:205`), absorção (`app/chamados/services/materializacao.py:560`), silêncio (`app/chamados/models.py:1870`), retratação (`:1990`), passivo histórico (`:3418`, `:3422`). | Ordenação `(ato_em, pk)` (`app/chamados/models.py:3374`); histórico da tela (`:2993`); correspondência do passivo (`:3413`); tarefa corta inclusivamente e ordena (`app/chamados/services/tarefa_do_dia.py:301`). **Não é lido por `marcos_do_lote`**, cujas cinco agregações estão em `app/chamados/veredito_lavrado.py:151`, `:154`, `:165`, `:176`, `:180`. |
| `Batida.criado_em` | `auto_now_add` (`app/ponto/models.py:79`); inserções pela porta de registro (`app/ponto/registro_batida.py:152`, `:157`). | Nascimento, distinto de `timestamp` da marcação; tarefa usa `<=agora` (`app/chamados/services/tarefa_do_dia.py:316`); invalidadores usam o mesmo campo e `<=ate` (`app/chamados/veredito_lavrado.py:137`, `:151`). |
| `Batida.retratada_em` | `retratar_batida → anotar`, com `timezone.now()` (`app/ponto/registro_batida.py:267`, `:274`); `anotar` escreve/salva (`:252`). Desfazer retratação zera pela mesma porta `anotar` (`app/ponto/services/retratar_humano.py:207`). | Estado atual de participação: `batidas_apuraveis` exige NULL (`app/ponto/turnos.py:2876`); invalidação por instante `<=ate` (`app/chamados/veredito_lavrado.py:154`). As duas leituras têm propósitos diferentes; o limite da retratação posterior está declarado (`app/chamados/services/tarefa_do_dia.py:313`). |
| `PerguntaDisputa.veredito_lavrado/julgado_em/janela_expira_em` | `lavrar` pergunta ao juiz e atribui os três (`app/chamados/veredito_lavrado.py:236`, `:244`); grava diretamente (`:248`) ou em lote pela porta (`:214`). O command de backfill também atribui o trio, depois chama a porta de persistência (`app/chamados/management/commands/backfill_veredito_lavrado.py:138`, `:143`). É escritor anterior/contextual declarado no contrato (`app/core/tests/test_contract_escritores_estado.py:72`). | `esta_stale` rejeita lavra futura (`app/chamados/veredito_lavrado.py:196`), compara data **local** da expiração inclusive (`:198`), data local da lavra estritamente anterior (`:202`) e insumo estritamente posterior (`:206`). Tarefa só admite `sim/nao` vigentes (`app/chamados/services/tarefa_do_dia.py:309`); varredura espelha as comparações (`app/chamados/services/raiox.py:40`, `:42`, `:45`); auditoria passa mesmo agora (`app/chamados/management/commands/auditar_invariantes_chamados.py:1160`). |
| `PerguntaDisputa.via_resolucao/materializada_em` | `_escrever_carimbo` atribui o par (`app/chamados/models.py:2658`); portas `carimbar` (`:2664`), `carimbar_identidade` (`:2727`), `descarimbar` (`:2748`), `descarimbar_em_lote` (`:2793`). | Juiz lê via com carimbo presente (`app/chamados/juizes.py:648`); tarefa lê via atual (`app/chamados/services/tarefa_do_dia.py:202`); invalidadores leem `materializada_em<=ate` (`app/chamados/veredito_lavrado.py:176`). `manter_carimbo` preserva instante/autor anterior (`app/chamados/models.py:2738`). |
| `PerguntaDisputa.data_evento` | Nascimento (`app/chamados/models.py:2578`), preenchimento na costura (`app/chamados/services/fio.py:838`), migração pela porta `rechavear_data_evento` (`:859`, `:898`). | Tarefa identifica pelo **dia do turno**, sem recuar dia civil (`app/chamados/services/tarefa_do_dia.py:197`, `:207`); juiz de data da batida usa a cadeia de marcos para cruzar meia-noite (`app/chamados/juizes.py:301`, `:319`); expiração usa a data e a janela (`app/chamados/veredito_lavrado.py:96`). |
| `CelulaDia.veredito_em/ata` | `lavrar_veredito` escreve ata e instante (`app/ponto/portas/celula.py:969`, `:1028`, `:1032`); ata também recebe chave `precedencia` por operação própria na **mesma porta** (`:1071`, `:1080`). Cópia de julgamento atribui ata apenas em memória (`:927`, `:940`). | `veredito_em` é instante de julgamento/alteração da lavra, agregado e cortado em `marcos_do_lote` (`app/chamados/veredito_lavrado.py:165`). Ata é fotografia atual do julgamento, sem histórico (`app/escala/models.py:1774`); leitor único materializa lâmpadas (`app/escala/services/leitor_celula.py:130`), alimentando tarefa (`app/chamados/services/tarefa_do_dia.py:328`). A porta pode atualizar ata com decisão admin preservada sem mover `veredito_em` (`app/ponto/portas/celula.py:997`). |

Unidade/fuso do modelo: instantes são `DateTimeField` e datas são `DateField` nos campos citados (`app/chamados/models.py:3358`, `:2393`, `:2399`, `:2238`; `app/ponto/models.py:67`, `:79`); configuração usa `USE_TZ=True` e `America/Sao_Paulo` (`app/config/settings/base.py:93`, `:95`). O dado de produção não demonstra fuso ou invariantes de cada linha armazenada **[nao verificado]**.

Não executei testes com banco; resultados da suite e mutações reais dos commits são **[nao verificado]**. [nao verificado]

## 07/11 12:13 · sessao 55ad8497c8f2 · - · - · origem=Ronald (tmux)

**Achado — relógio transitivo fora da borda; procedente.** A tarefa chama `self.cobertura` em `app/chamados/services/tarefa_do_dia.py:354`; o carregador monta `_q_cobertura` em `app/ponto/turnos.py:3072`, que usa `Ausencia.TIPOS_EM_ABERTO` em `app/ponto/turnos.py:2762`. Esse nome aponta para o conjunto vivo do catálogo (`app/ponto/models.py:689`); sua iteração chama a função de catálogo (`app/ponto/catalogo/ausencias.py:406`), que chega a `vigente()` (`app/ponto/catalogo/ausencias.py:433`). Ali há **`time.monotonic()`** (`app/ponto/catalogo/ausencias.py:275`) e atualização de cache (`app/ponto/catalogo/ausencias.py:279`). O selo declara expressamente `monotonic` como leitura de relógio (`app/chamados/tests/test_e2_tarefa_do_dia.py:890`).

**Achado — selo incompleto para helpers importados; procedente.** A varredura AST examina somente os módulos da tarefa e de `veredito_lavrado` (`app/chamados/tests/test_e2_tarefa_do_dia.py:958`); o veneno dinâmico intercepta exclusivamente `django.utils.timezone.now` (`app/chamados/tests/test_e2_tarefa_do_dia.py:680`). Assim, nenhum dos dois instrumentos intercepta o `monotonic` real acima (`app/ponto/catalogo/ausencias.py:275`). O mesmo catálogo usa TTL de 60 segundos; portanto **não acusar uma query por ausência** sem medir a expiração desse cache (`app/ponto/catalogo/ausencias.py:257`, `app/ponto/catalogo/ausencias.py:276`).

**Achado — “não morde os mutantes declarados”; não procedente na análise isolada do detector.** O detector identifica as 16 formas declaradas no teste: chamadas diretas, aliases, importações e `time` (`app/chamados/tests/test_e2_tarefa_do_dia.py:926`, `app/chamados/tests/test_e2_tarefa_do_dia.py:966`). Extraí AST/constantes em memória e confirmei 16/16; suíte Django e comportamento com banco: **[nao verificado]**.

**Quanto a `timezone.now`/`localdate`/`date.today` em helpers reais:** não achei leitura dessas três formas no caminho da tarefa que rastreei: leitor alimentado (`app/escala/services/leitor_celula.py:350`), previsto (`app/escala/utils.py:2256`), vínculo (`app/escala/alimentacao.py:269`), marcos (`app/escala/servico_jornada.py:68`), fila com `agora` (`app/chamados/services/painel.py:190`), âncora (`app/chamados/services/fio.py:116`), passivo (`app/chamados/models.py:3409`), dia sem trabalho (`app/chamados/juizes.py:423`) e lavra (`app/chamados/veredito_lavrado.py:196`). O escape encontrado é o **`monotonic`**, já incluído no vocabulário do próprio selo (`app/chamados/tests/test_e2_tarefa_do_dia.py:891`).

## 07/11 11:28 · sessao b1dd9a2cb159 · OTIMIZA-3-LOADER · censo · origem=Code

**1a, 1b e 1c cabem em (a) nas respostas fornecidas. Para 3 e 4, o tema foi perguntado, mas falta a resposta da v11 para fechar a atribuição: [não verificado].** A regra exige tanto tema presente quanto omissão do Codex (`CLAUDE.md:377–379`).

Nos anexos abaixo, conto as linhas desde a primeira linha do conteúdo, excluindo o marcador `== anexo: ... ==`.

| achado | classe | trecho da pergunta | trecho da resposta/triagem |
|---|---|---|---|
| **1a — `find_spec` → `module_from_spec` → `exec_module`** | **(a), nas respostas disponíveis.** O universo perguntado abrangia loaders fora da lista; a resposta permaneceu nas formas enumeradas. Omissão na resposta final: **[não verificado]**. | “**todas as formas pelas quais um modulo-alvo e ligado SEM ast.Import/ast.ImportFrom**” (`v9_codex_q1.txt:14`). | “**`spec_from_file_location`, `SourceFileLoader`, `SourcelessFileLoader`, `imp.load_source` e `run_path` são reconhecidos pelo nome**” (`v9_codex_r1.md:31`). A cópia mantém a lista fechada e o despacho por ela (`bin/quem_chama.py:104–111`, `bin/quem_chama.py:653–659`). |
| **1b — `import_module` dentro de string `.py`, nome recebido por argv do `-c`** | **(a), nas respostas disponíveis.** Código dentro de string e nome vindo de parâmetro estavam expressamente cobertos; a ligação específica pelo argv não foi apontada. Omissão na resposta final: **[não verificado]**. | “**import_module(nome) com nome vindo [...] de parametro**”; “**loader em texto embutido: heredoc de .sh, python3 -c "...", string de .py que e codigo, config**” (`v9_codex_q1.txt:17–18`, `:27`). | “**string de código em `.py` e config são cobertos quando o texto contém um loader reconhecido e caminho/basename ou nome completo reconhecível**” (`v9_codex_r1.md:49`). A triagem declara curada outra forma: “**-c "exec(open(...))" em texto**”; não registra descarte do caso com `import_module(sys.argv[1])` (`v11_triagem_r2.txt:9–11`). |
| **1c — `import_string` montado; `pkgutil.get_loader().load_module()`** | **(a), nas respostas disponíveis.** `import_string` foi nominalmente perguntado; `pkgutil` estava no universo de “todas as formas”. Omissão na resposta final: **[não verificado]**. | “**django.utils.module_loading.import_string**” (`v9_codex_q1.txt:19`); “**todas as formas**” (`v9_codex_q1.txt:14`). | A resposta aborda APIs vizinhas: “**`include('x.urls')`, settings e `apps.get_model` não ganham reconhecimento de loader**”, sem individualizar os dois escapes do achado (`v9_codex_r1.md:75`). Ambos ficam fora dos conjuntos operacionais da cópia (`bin/quem_chama.py:104–111`, `:653–659`). |
| **3 — negativa `r11-exec-string-nao-e-opaco` sem par** | **[não verificado] para fechar (a).** Tema explicitamente presente; falta a resposta que permitiria estabelecer se o Codex viu a vacuidade. | “**alguma assercao r11-* passa por acaso de fixture ou e vacua?**” (`v11_codex_q3.txt:28`). | “**exec/compile de STRING de codigo nao e opaco (r11-exec-string-nao-e-opaco; M91)**” (`v11_triagem_r2.txt:17`). Isso registra o comportamento pretendido, sem registrar descoberta/descarte da falta de par. Na cópia, a negativa exige somente ausência do sítio (`bin/tests/test_quem_chama.sh:788`). |
| **4 — `python` associa caminho, não argv do `-c`; `conta['opacos']` sem leitor** | **[não verificado] para fechar a classe do item inteiro.** A parte argv coincide com a omissão de 1b; a auditoria final do contador depende da resposta v11 ausente. | “**QUEM ESCREVE e QUEM LE [...] o tipo 'python' em ldr/ldr_txt [...] opacos**”; “**um campo ganhou dois significados?**” (`v11_codex_q3.txt:23–26`). | “**'python' separado de 'caminho'**” (`v11_triagem_r2.txt:26`). A cópia ainda associa `python` a citações de caminho (`bin/quem_chama.py:870–874`); grava `conta['opacos']`, enquanto o resumo usa outros campos (`bin/quem_chama.py:1232`, `:1243–1247`). |

O **“PARCIAL”** da triagem trata de constante entre arquivos que fica **“OPACA e LISTADA”**; não documenta descarte desses escapes sem OPACO, nem do negativo sem par ou do contador sem leitor (`v11_triagem_r2.txt:16–20`). Portanto, esse trecho não sustenta **(b)** para os cinco achados.

Atribuir **(d), MODO-COMUM**, exigiria comprovar também a omissão dos opus anteriores; essa comprovação não está no material fornecido: **[não verificado]**.

## 07/11 11:25 · sessao f8d50d317e38 · - · - · origem=Ronald (tmux)

| achado | avaliação | evidência literal |
|---|---|---|
| 3 — negativo sem par | Tema **presente**, portanto não cabe (e); omissão do Codex necessária para (a) **[não verificado]**, porque falta a resposta v11. | Pergunta v11 P5: “alguma assercao r11-* passa por acaso de fixture ou e vacua?” Triagem v11 item 6: “exec/compile de STRING de codigo nao e opaco (r11-exec-string-nao-e-opaco; M91)”. Esse trecho declara a forma curada, mas não relata descoberta nem descarte da falta de par **[não verificado]**. |
| 4 — `python`/`-c` | Tema **presente**; não há descarte específico demonstrado **[não verificado]**. | Pergunta v11 P4: “o tipo 'python' em ldr/ldr_txt [...] Algum leitor ficou com a semantica de e65767b4 [...] ou um campo ganhou dois significados?” Resposta v10, achado 3: “Não examina a expressão do argumento do script nem resolve aliases de `sys`.” Triagem v11 item 3: “1o arg nao-flag apos o interpretador e caminho, -m = nome”. Isso cobre a alteração, sem demonstrar que o Codex viu a lacuna específica de nome vindo de argv em `-c` **[não verificado]**. |
| 4 — `conta['opacos']` sem leitor | Tema do fluxo de `opacos` **presente**; descoberta/omissão final **[não verificado]**. | Pergunta v11 P4 inclui “QUEM ESCREVE e QUEM LE [...] opacos”. Não foi fornecida resposta v11; as respostas anteriores não podem atestar a leitura desse contador na implementação final **[não verificado]**. |

Confirmação estática da cópia:

- A fixture existe em `bin/tests/test_quem_chama.sh:486–488`; sua negativa apenas procura a ausência do sítio em `bin/tests/test_quem_chama.sh:788`. A asserção não exige a existência da fixture nem um resultado positivo dela (`bin/tests/test_quem_chama.sh:788`).
- O ramo subprocesso distingue `-m`, depois remove flags e trata o primeiro elemento restante como caminho; não possui ramo específico de argv do `-c` (`bin/quem_chama.py:781–793`).
- A associação lexical considera `'python'` junto de `'caminho'` (`bin/quem_chama.py:870–871`).
- `conta['opacos']` é inicializado em `bin/quem_chama.py:1232`; o resumo usa `varridas`, `zeros` e `nao` (`bin/quem_chama.py:1243–1247`), e `main` usa somente `conta['nao']` (`bin/quem_chama.py:1273–1274`).
- A regra exige “nao viu com o tema PRESENTE” para (a), e “viu e a triagem descartou” para (b) (`CLAUDE.md:377–379`). **Tema presente não basta para provar omissão quando a resposta final falta.**
- Não há base para (d): laudos anteriores completos não foram fornecidos **[não verificado]**.

## 07/11 11:25 · sessao b2ee29e03326 · - · - · origem=Ronald (tmux)

Classificação sugerida: **1a = (a); 1b = (a); 1c = (a)**, limitada às respostas fornecidas. A resposta da v11 não está no dado; afirmar omissão nessa resposta é **[não verificado]**.

- **1a — tema presente, forma omitida.** Pergunta v9 P1: **“LOADER DE STRING -- todas as formas pelas quais um modulo-alvo e ligado SEM ast.Import/ast.ImportFrom.”** Resposta v9, achado 2: **“`spec_from_file_location`, `SourceFileLoader`, `SourcelessFileLoader`, `imp.load_source` e `run_path` são reconhecidos pelo nome.”** O contraexemplo `find_spec/module_from_spec/exec_module` não aparece nas respostas fornecidas **[não verificado quanto à resposta v11 ausente]**. A cópia confirma a lista fechada sem esses nomes: `bin/quem_chama.py:104–111`; despacho limitado a essa lista: `bin/quem_chama.py:653–659`.

- **1b — tema explicitamente presente.** Pergunta v9 P1(e): **“loader em texto embutido: heredoc de .sh, python3 -c "...", string de .py que e codigo, config.”** Pergunta v10 P1(d): **“subprocess com 'python3'/sys.executable (_RX_PYTHON, _cadeia)”**. Resposta v10, achado 3: **“Não examina a expressão do argumento do script nem resolve aliases de `sys`.”** Isso identifica uma lacuna próxima, mas não o `-c` cujo import recebe nome por `sys.argv[1]`. Triagem v11 item 3 declara **“forma curada”** para outros exemplos; não descarta esse contraexemplo exato. A cópia trata a lista do interpretador como caminho (`bin/quem_chama.py:777–794`) e procura nome do loader dentro de cada string, sem vincular argv (`bin/quem_chama.py:488–508`, `bin/quem_chama.py:875–881`).

- **1c — tema explicitamente presente para `import_string`; universo amplo para `pkgutil`.** Pergunta v9 P1(a) cita literalmente **“django.utils.module_loading.import_string”**; o cabeçalho P1 pede **“todas as formas”** sem imports AST. Resposta v9, achado 5, discute outras APIs: **“`include('x.urls')`, settings e `apps.get_model` não ganham reconhecimento de loader”**; não registra `import_string` nem `pkgutil.get_loader().load_module()` nas respostas fornecidas **[não verificado quanto à resposta v11 ausente]**. Esses nomes estão fora da lista operacional (`bin/quem_chama.py:104–111`, `bin/quem_chama.py:653–659`).

**Não há base para (b):** nenhuma triagem fornecida descarta esses três contraexemplos exatos **[não verificado quanto a documentos ausentes]**. **Não há base suficiente para (d):** os laudos completos dos Opus anteriores e a resposta v11 não foram fornecidos **[não verificado]**.

## 07/11 10:38 · sessao 58b366715336 · FIO-E0-V4-E1 · verificador · origem=Code

**A. PROCEDENTE: ainda há significados e consultas de identidade divergentes.** O índice usa a instância persistida e o episódio; parte das portas continua usando a posição fixa do motivo ou disputa+motivo+dia (`app/chamados/models.py:2508`; `app/chamados/services/disputa_emissao.py:321`; `app/chamados/migrations/0053_pergunta_unica_por_familia.py:53`; `app/chamados/migrations/0053_pergunta_unica_por_familia.py:58`).

O censo abaixo identifica escritores e cadeias leitoras encontrados; exaustividade de acessos dinâmicos, execução dos testes e incidência em produção: **[nao verificado]**. [nao verificado]

| Campo | Quem escreve | Quem lê e significado |
|---|---|---|
| `via_resolucao` | `_escrever_carimbo`, pelas portas `carimbar`, `carimbar_identidade` e `descarimbar`; também `descarimbar_em_lote`, `encerrar_por_fato_no_espelho` e a porta de relavratura (`app/chamados/models.py:2720`; `app/chamados/models.py:2884`; `app/chamados/models.py:2893`; `app/chamados/services/relavra_via.py:123`). | Identidade: `do_fato`, `linhas_dos_fatos`, `_no_fato`, retratação e backfill (`app/chamados/models.py:2486`; `app/chamados/models.py:2510`; `app/chamados/services/fio.py:1030`; `app/chamados/services/fio.py:993`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:90`). Vitalidade/materialização: C6, `q_vivas`, absorção e relavratura (`app/chamados/juizes.py:1147`; `app/chamados/juizes.py:1173`; `app/chamados/models.py:2968`; `app/chamados/services/materializacao.py:623`; `app/chamados/services/relavra_via.py:179`). **Não há exclusão uniforme**: a fábrica exclui somente `duplicata`, deixando `motivo_retratado` segurar emissão (`app/chamados/services/fabrica_da_celula.py:119`). |
| `batida_origem` da Pergunta | `_nascer` e `_chave_de_identidade`, pelo derivador que lê `ultima_batida.id`; a 0052 deixa o acervo NULL (`app/chamados/models.py:2591`; `app/chamados/models.py:3057`; `app/chamados/models.py:3072`; `app/chamados/migrations/0052_pergunta_batida_origem.py:19`). | `do_fato`, `fato_livre_para`, `_colisao_ao_datar`, migrador, agrupamento e índice (`app/chamados/models.py:2483`; `app/chamados/models.py:2525`; `app/chamados/services/fio.py:890`; `app/chamados/services/fio.py:1108`; `app/chamados/services/fio.py:1017`; `app/chamados/models.py:3095`). **Não há consenso operacional**: o emissor pode barrar antes de consultar episódio; `do_fato` também agrega acervo sem episódio pelo dia (`app/chamados/services/disputa_emissao.py:321`; `app/chamados/models.py:2482`). `_achar_batida_origem(chamado)` é outra função, referente à entrada do turno, e não lê essa coluna (`app/chamados/services/materializacao.py:109`). |
| `data_evento` | Nascimento datado/conversa; posteriormente, `reapontar_resolvedora` e `rechavear_data_evento` (`app/chamados/models.py:2541`; `app/chamados/models.py:2552`; `app/chamados/models.py:2641`; `app/chamados/services/fio.py:841`; `app/chamados/services/fio.py:948`). | Identidade e competência: lote, agrupamento, backfill e índice (`app/chamados/models.py:2508`; `app/chamados/services/fio.py:1046`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:107`; `app/chamados/models.py:3090`). Plantio/ausência: `materializacao` (`app/chamados/services/materializacao.py:344`; `app/chamados/services/materializacao.py:473`; `app/chamados/services/materializacao.py:517`). Validação, C6, lavra, reconciliação e apresentação (`app/chamados/services/validacao.py:269`; `app/chamados/services/validacao.py:433`; `app/chamados/juizes.py:1199`; `app/chamados/veredito_lavrado.py:229`; `app/chamados/reconciliador.py:1543`; `app/chamados/services/fio.py:87`). **Dois significados explícitos**: dia do turno no fato e dia da pergunta na conversa; leitores de identidade que não filtram `conversa` os misturam (`app/chamados/models.py:2552`; `app/chamados/services/disputa_emissao.py:394`). |
| `instancia_marco` | `_chave_de_identidade` deriva somente quando vazia; valor explícito é preservado. A 0051 preenche posições fixas (`app/chamados/models.py:3054`; `app/chamados/migrations/0051_decisao_pergunta.py:27`). | Índice/agrupamento usam a coluna persistida; `linhas_dos_fatos`, alcançada pelas portas e migrador, substitui-a por `instancia_do_motivo(m)` (`app/chamados/models.py:3090`; `app/chamados/services/fio.py:1016`; `app/chamados/models.py:2508`; `app/chamados/services/fio.py:1107`). **Divergência comprovada**: a coluna admite `I1/I2`, mas a consulta procura `int_ida/int_volta`; o próprio derivador declara que ainda não carrega índice da pausa (`app/chamados/tests/test_e1_v5_indice.py:124`; `app/chamados/tests/test_e1_v5_indice.py:125`; `app/chamados/services/declaracao_texto.py:45`; `app/chamados/services/declaracao_texto.py:50`). |
| `colaborador` da Pergunta | Derivado e conferido contra a disputa **na inserção**; preenchimento histórico pela 0051 (`app/chamados/models.py:3038`; `app/chamados/models.py:3048`; `app/chamados/migrations/0051_decisao_pergunta.py:25`). | Portas, agrupamento, backfill e desfazer usam a coluna (`app/chamados/models.py:2509`; `app/chamados/services/fio.py:936`; `app/chamados/services/fio.py:1044`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:100`; `app/chamados/services/relavra_via.py:221`). C6, validação, fábrica e materialização continuam alcançando a pessoa pela disputa (`app/chamados/juizes.py:665`; `app/chamados/services/validacao.py:421`; `app/chamados/services/fabrica_da_celula.py:118`; `app/chamados/services/materializacao.py:282`). O nascimento exige igualdade; preservação universal após escritas dinâmicas: **[nao verificado]**. |
| `conversa` | `abrir=False`, `abrir_conversa=True`, persistidos por `_nascer`; acervo recebe default `False` na 0051 (`app/chamados/models.py:2541`; `app/chamados/models.py:2552`; `app/chamados/models.py:2642`; `app/chamados/migrations/0051_decisao_pergunta.py:99`). | Porta, derivador, migrador, agrupamento e índices distinguem conversa (`app/chamados/models.py:2523`; `app/chamados/models.py:2566`; `app/chamados/models.py:3054`; `app/chamados/services/fio.py:1104`; `app/chamados/services/fio.py:1029`; `app/chamados/models.py:3091`). **Outros dedups não distinguem**, relacionados em C (`app/chamados/services/disputa_emissao.py:322`; `app/chamados/services/declaracao_texto.py:237`). Identificação correta das conversas antigas: **[nao verificado]**; a migração não as classifica individualmente (`app/chamados/migrations/0051_decisao_pergunta.py:99`). |
| `chamado_destino` | Nascimento/reuso em `_nascer` e reapontamento pela porta do fio (`app/chamados/models.py:2613`; `app/chamados/models.py:2642`; `app/chamados/services/fio.py:869`). As atribuições da simulação de costura e de `_vivas_entre` são em memória (`app/chamados/services/fio.py:780`; `app/chamados/services/fio.py:784`; `app/chamados/models.py:361`). | Costura/E4-c, migrador, materialização, C6, relavratura, declaração, reconciliação e listagem (`app/chamados/services/fio.py:857`; `app/chamados/services/fio.py:1095`; `app/chamados/services/materializacao.py:1254`; `app/chamados/juizes.py:744`; `app/chamados/services/relavra_via.py:101`; `app/chamados/services/declaracao_texto.py:324`; `app/chamados/reconciliador.py:1487`; `app/chamados/services/fio.py:80`). **O desvio está em usá-lo como identidade**: `_derivar_chamado_destino` trata chamado+motivo+carimbo nulo como impedimento do mesmo fato (`app/chamados/services/disputa_emissao.py:214`; `app/chamados/services/disputa_emissao.py:226`). |

Há ainda leitores externos dos valores projetados: API, calendário, prévia, histórico, explicador, cobrança e comandos de auditoria/migração (`app/api/views.py:2606`; `app/api/views_mensageria.py:568`; `app/colaboradores/services/calendario.py:473`; `app/folha/previa.py:65`; `app/chamados/services/historico.py:108`; `app/chamados/services/explicador.py:95`; `app/chamados/services/cobranca.py:83`; `app/chamados/management/commands/auditar_invariantes_chamados.py:955`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:130`).

**Achado adicional do backfill — PROCEDENTE.** Uma `admin_decidiu` sem ato e sem resposta pode integrar colisão com uma irmã respondida. A irmã respondida vence; o primeiro laço retrata a `admin_decidiu`; o segundo a pula porque a via agora é `duplicata`. Assim, esse caminho termina sem escrever seu SILENCIO (`app/chamados/services/fio.py:985`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:131`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:142`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:145`). Existência desse caso no col90: **[nao verificado]**.

**B. NAO PROCEDE para a recusa sequencial corrigida; PROCEDENTE para canônica desatualizada.**

Todos os chamadores encontrados de `retratar_duplicata`:

| Chamador | Referência |
|---|---|
| `reapontar_resolvedora` | `app/chamados/services/fio.py:868` |
| `migrar_resolvedoras_vivas` | `app/chamados/services/fio.py:1122` |
| `dedup_perguntas_fato.handle` | `app/chamados/management/commands/dedup_perguntas_fato.py:83` |
| `backfill_decisoes_pergunta.aplicar` | `app/chamados/management/commands/backfill_decisoes_pergunta.py:131` |
| Testes diretos | `app/chamados/tests/test_e1_v5_portas_backfill.py:159`; `app/chamados/tests/test_porta_reapontamento_pergunta.py:390`; `app/chamados/tests/test_porta_reapontamento_pergunta.py:398` |

- **NAO PROCEDE a antiga antecipação em `reapontar_resolvedora`:** E4-c e costura são aceitos antes da retratação; recusa retorna antes dela. Retratação e gravação da substituta permanecem no mesmo `atomic` (`app/chamados/services/fio.py:844`; `app/chamados/services/fio.py:857`; `app/chamados/services/fio.py:863`; `app/chamados/services/fio.py:867`; `app/chamados/services/fio.py:871`).
- **PROCEDENTE a janela do migrador:** captura as vivas do destino sem travar o conjunto; quando a canônica já está no destino, pula o reapontamento e retrata diretamente a fonte. O chamador `reabrir` não envolve esse ramo em transação própria (`app/chamados/services/fio.py:1095`; `app/chamados/services/fio.py:1113`; `app/chamados/services/fio.py:1122`; `app/chamados/models.py:1387`).
- **Contraexemplo concorrente permitido pelo código:** depois da consulta do fato, a canônica sai por `remover_motivo`; o migrador então retrata a fonte contra a instância antiga. `remover_motivo` não toma a trava do colaborador nesse ato, e `retratar_duplicata` não confere a canônica atual nem igualdade de chave (`app/chamados/models.py:2000`; `app/chamados/models.py:2008`; `app/chamados/services/fio.py:993`; `app/chamados/services/fio.py:1002`). Resultado possível: nenhuma linha permanece na identidade do fato; ocorrência efetiva: **[nao verificado]**.
- **A mesma classe alcança os commands:** dedup escolhe canônica antes do `atomic`; backfill relê os PKs travados, mas não reaplica `_no_fato` antes de escolher a canônica (`app/chamados/management/commands/dedup_perguntas_fato.py:66`; `app/chamados/management/commands/dedup_perguntas_fato.py:81`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:127`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:128`).

**C. PROCEDENTE: sobrevivem estes agrupadores/gatilhos com identidade diferente.**

| Sítio | Diferença para a 0053 |
|---|---|
| `linhas_dos_fatos` → `do_fato` → portas/migrador | Deriva instância fixa pelo motivo, em vez de receber a instância persistida (`app/chamados/models.py:2508`; `app/chamados/models.py:2525`; `app/chamados/services/fio.py:890`; `app/chamados/services/fio.py:1107`). |
| `do_fato`, BATIDA | Une episódio conhecido ao acervo sem episódio do mesmo dia; sem episódio, retorna à chave por dia (`app/chamados/models.py:2482`; `app/chamados/models.py:2488`). |
| `gerar_perguntas_disputa`, ramo BATIDA | Disputa+motivo, sem episódio e sem excluir conversa (`app/chamados/services/disputa_emissao.py:321`). |
| `gerar_perguntas_disputa`, ramos datados | Disputa+motivo+dia; exclui as vias, mas não conversa nem instância (`app/chamados/services/disputa_emissao.py:393`; `app/chamados/services/disputa_emissao.py:410`). |
| `_derivar_chamado_destino` | Chamado+motivo+carimbo nulo, sem chave completa do fato (`app/chamados/services/disputa_emissao.py:226`). |
| `_ja_perguntou` | Disputa+motivo+dia antes de consultar `fio_vivo`; sem os filtros de identidade (`app/chamados/services/disputa_emissao.py:1297`; `app/chamados/services/disputa_emissao.py:1304`). |
| `_candidata_reusavel` | Disputa+motivo+dia antes do predicado de reuso (`app/chamados/services/declaracao_texto.py:236`). |
| `fabrica_da_celula.motivos_a_perguntar` | Colaborador+dia, reduzido a conjunto de motivos; exclui só duplicata. Uma retratada/conversa pode calar emissão (`app/chamados/services/fabrica_da_celula.py:118`; `app/chamados/services/fabrica_da_celula.py:123`; `app/chamados/services/fabrica_da_celula.py:126`). |
| `disparar_perguntas_competencia` | Colaborador+motivo+dia, sem vias/conversa/instância; abandona como fato existente (`app/chamados/management/commands/disparar_perguntas_competencia.py:211`). |
| `_marco_ja_perguntado` | Colaborador+dia+motivos; qualquer pergunta morta pode representar marco encerrado, sem os filtros do índice (`app/chamados/services/pergunta_no_app.py:109`). |
| `insumos_em_regularizacao` | Agrupa motivo+dia após restringir à instância fixa do derivador; não representa instâncias distintas (`app/chamados/services/declaracao_texto.py:547`; `app/chamados/services/declaracao_texto.py:550`). |

O caso BATIDA tem consequência direta: a disputa aberta é reutilizada e o gerador é chamado novamente, mas seu filtro disputa+motivo cala a segunda batida antes da porta (`app/chamados/services/disputa_emissao.py:515`; `app/chamados/services/disputa_emissao.py:666`; `app/chamados/services/disputa_emissao.py:321`). O teste alterado fecha a disputa entre as batidas e não cobre esse caminho (`app/chamados/tests/test_a2_data_evento_batida.py:33`; `app/chamados/tests/test_a2_data_evento_batida.py:42`).

**D. PROCEDENTE: a ordem literal ainda não é “SEMPRE”.**

- **Pergunta antes de Colaborador:** dedup abre `atomic` e retrata sem tomar a trava do colaborador; chega ao `save` da Pergunta por `carimbar_identidade` (`app/chamados/management/commands/dedup_perguntas_fato.py:81`; `app/chamados/services/fio.py:1002`; `app/chamados/models.py:2812`).
- **Pergunta antes de Disputa:** `Disputa.fechar`, no encerramento administrativo/desligamento, carimba perguntas antes de atualizar a disputa (`app/chamados/models.py:1880`; `app/chamados/models.py:1895`).
- **Pergunta antes de Chamado:** `fechar_o_fio_do_dia` carimba perguntas e depois reconcilia chamados; o reconciliar pode travar/atualizar Chamado (`app/ponto/services/veredito_celula.py:180`; `app/ponto/services/veredito_celula.py:192`; `app/chamados/models.py:687`; `app/chamados/models.py:285`).
- **Validação:** atualiza Pergunta antes de uma devolução que pode reabrir Disputa no mesmo `atomic` (`app/chamados/services/validacao.py:417`; `app/chamados/services/validacao.py:422`; `app/chamados/services/validacao.py:451`; `app/chamados/services/validacao.py:218`; `app/chamados/models.py:2124`).
- **Desfazer:** toma Colaborador primeiro, mas usa uma única consulta `FOR UPDATE OF self, disputa`; não estabelece separadamente Disputa→Pergunta. Ordem efetiva dessas duas aquisições no banco: **[nao verificado]** (`app/chamados/services/relavra_via.py:226`; `app/chamados/services/relavra_via.py:230`).

**NAO PROCEDE nos chamadores de produção encontrados a acusação de `trava_do_colaborador` fora de transação.** Os chamadores de `fechar_o_fio_do_dia` e `travar_origem_antes` estão sob `atomic`; o helper não cria sua própria transação (`app/ponto/services/veredito_celula.py:214`; `app/ponto/services/declarar_dia.py:188`; `app/chamados/services/validacao.py:281`; `app/ponto/services/justificativa.py:131`; `app/ponto/services/justificativa.py:264`; `app/ponto/services/justificativa.py:333`; `app/ponto/views.py:1792`; `app/chamados/models.py:2456`).

**E. PROCEDENTE parcialmente: há mordida real, mas também provas insuficientes.**

| Teste | Aval | [nao verificado]
|---|---|
| `test_e1_v5_chave_familia`, dedup | **NAO PROCEDE “vazio”**: cria fatos, executa command e confere vias persistidas; MORDE remove o índice para montar acervo duplicado (`app/chamados/tests/test_e1_v5_chave_familia.py:54`; `app/chamados/tests/test_e1_v5_chave_familia.py:67`; `app/chamados/tests/test_e1_v5_chave_familia.py:73`). |
| Migração, episódios distintos | **PROCEDENTE a insuficiência**: só exige ausência de duplicatas; migrador sem ação satisfaria essas asserções. Não exige destino final nem `migradas` (`app/chamados/tests/test_e1_v5_chave_familia.py:82`; `app/chamados/tests/test_e1_v5_chave_familia.py:88`). |
| Migração, mesmo episódio | **PROCEDENTE a cobertura parcial**: prova retratação da irmã, mas não a canônica efetivamente no destino (`app/chamados/tests/test_e1_v5_chave_familia.py:91`; `app/chamados/tests/test_e1_v5_chave_familia.py:98`). |
| Migração, destino recusado | **PROCEDENTE a limitação do mock**: substitui integralmente `reapontar_resolvedora`; prova reação ao retorno False, não recusa real/atomicidade (`app/chamados/tests/test_e1_v5_chave_familia.py:108`). |
| `OrdemDeTravaDaValidacaoEDoDesfazerTest` | **Não é vazio para Colaborador antes**: exige Pergunta na captura e possui mutantes removendo a trava. **É insuficiente para o aval completo**: filtra deliberadamente todas as violações que não sejam “sem o Colaborador antes” (`app/chamados/tests/test_e1_v5_ordem_trava.py:231`; `app/chamados/tests/test_e1_v5_ordem_trava.py:249`; `app/chamados/tests/test_e1_v5_ordem_trava.py:253`; `app/chamados/tests/test_e1_v5_ordem_trava.py:259`). |
| Capturador de travas | **PROCEDENTE a cegueira**: registra apenas a tabela do `FROM` e elimina repetições por tabela. Não captura a Disputa adicional do `FOR UPDATE OF self, disputa`, nem distingue novas linhas da mesma tabela (`app/chamados/tests/test_e1_v5_ordem_trava.py:29`; `app/chamados/tests/test_e1_v5_ordem_trava.py:38`; `app/chamados/tests/test_e1_v5_ordem_trava.py:43`; `app/chamados/services/relavra_via.py:230`). |
| `ColisaoAoDatarTest` MORDE | **NAO PROCEDE “vazio/mock”**: usa destino real de outro dia, exige recusa, irmã preservada e uma linha no fato. Morde a antiga retratação antecipada (`app/chamados/tests/test_e1_v5_portas_backfill.py:137`; `app/chamados/tests/test_e1_v5_portas_backfill.py:142`; `app/chamados/tests/test_e1_v5_portas_backfill.py:146`). |
| `FatoRetratadoVoltaTest` RED datado | **NAO PROCEDE “vazio”**: passa por emissão→remoção→emissão reais e exige PK nova. Cobre o ramo de intervalo; não o outro filtro dos motivos com destino (`app/chamados/tests/test_e1_volta2.py:271`; `app/chamados/tests/test_e1_volta2.py:274`; `app/chamados/tests/test_e1_volta2.py:284`; `app/chamados/services/disputa_emissao.py:410`). |
| Instâncias `I1/I2` | **PROCEDENTE a lacuna**: testa somente inserções cruas; não passa pelas portas que substituem a instância pela posição do motivo (`app/chamados/tests/test_e1_v5_indice.py:124`; `app/chamados/models.py:2508`). |

Resultado de execução desses testes: **[nao verificado]**. [nao verificado]

**F. PROCEDENTE para `--ate` e para exportada sem PAREI.**

- **`--ate` fraciona grupo BATIDA:** data não pertence à chave, mas o filtro é aplicado antes da agregação; a busca dos PKs reutiliza a mesma base recortada (`app/chamados/services/fio.py:1017`; `app/chamados/services/fio.py:1045`; `app/chamados/services/fio.py:1049`).
- **Contraexemplo estático:** três irmãs do episódio, duas antes do corte e a terceira respondida depois. A canônica global respondida fica fora; o command retrata uma irmã usando outra canônica local. Se houver apenas uma irmã dentro e outra fora, nem detecta colisão (`app/chamados/services/fio.py:1047`; `app/chamados/services/fio.py:985`; `app/chamados/management/commands/dedup_perguntas_fato.py:68`; `app/chamados/management/commands/dedup_perguntas_fato.py:83`).
- **`--col

[truncado em 20000 de 23734 caracteres]

## 07/11 10:35 · sessao 308a91c03dbd · - · - · origem=Ronald (tmux)

A/C — **PROCEDENTE: os leitores ainda não têm uma identidade única compatível com o índice.**

- **`instancia_marco` tem dois alcances.** O índice aceita a instância persistida (`app/chamados/models.py:3090`); o escritor preserva instância explícita e só deriva quando vazia (`app/chamados/models.py:3054`). Já `linhas_dos_fatos` sempre impõe a posição fixa derivada do motivo, sem receber a instância da pergunta (`app/chamados/models.py:2508`; `app/chamados/services/declaracao_texto.py:42`). Portanto `fato_livre_para` e `_colisao_ao_datar` podem consultar `int_ida` ao tratar uma pergunta `I2` (`app/chamados/models.py:2525`; `app/chamados/services/fio.py:890`), e `migrar_resolvedoras_vivas` pode retratar fatos de instâncias diferentes (`app/chamados/services/fio.py:1107`; `app/chamados/services/fio.py:1122`).
- **BATIDA ainda sofre dedup por disputa+motivo.** `gerar_perguntas_disputa` descarta qualquer motivo não datado se já existe na disputa, sem episódio nem `conversa=False` (`app/chamados/services/disputa_emissao.py:321`). O conjunto datado contém os motivos de destino e os intervalos (`app/chamados/services/disputa_emissao.py:26`). A emissão repetida reutiliza a disputa aberta e volta a chamar esse gerador (`app/chamados/services/disputa_emissao.py:515`; `app/chamados/services/disputa_emissao.py:666`), portanto uma segunda batida com `gps_ausente` na mesma disputa é calada antes da porta. O teste alterado fecha a disputa entre as batidas (`app/chamados/tests/test_a2_data_evento_batida.py:33`; `app/chamados/tests/test_a2_data_evento_batida.py:42`).
- **Fallback BATIDA por dia permanece deliberadamente diferente do índice.** Com episódio informado, `do_fato` une o episódio às linhas antigas sem origem daquele dia; sem episódio, usa exclusivamente a chave por dia (`app/chamados/models.py:2482`; `app/chamados/models.py:2488`). Isso permite que uma linha antiga sem episódio retenha episódios distintos do mesmo dia; o teste o exige explicitamente (`app/chamados/tests/test_e1_v5_indice.py:111`).
- **Outros gatilhos/leitores de identidade divergente:**
  - Ramos datados de `gerar_perguntas_disputa`: disputa+motivo+dia, sem conversa/instância (`app/chamados/services/disputa_emissao.py:393`; `app/chamados/services/disputa_emissao.py:410`).
  - `_derivar_chamado_destino`: chamado+motivo+carimbo nulo; o comentário chama isso de “MESMO FATO”, mas não confere instância/conversa (`app/chamados/services/disputa_emissao.py:214`; `app/chamados/services/disputa_emissao.py:226`).
  - `_ja_perguntou`: disputa+motivo+dia sem excluir vias/conversa/instância antes do `fio_vivo`; chamado pelo agrupador e pelo cron (`app/chamados/services/disputa_emissao.py:1297`; `app/chamados/services/disputa_emissao.py:813`; `app/chamados/management/commands/disparar_perguntas_competencia.py:173`).
  - `_candidata_reusavel`: disputa+motivo+dia sem vias/conversa/instância (`app/chamados/services/declaracao_texto.py:236`).
  - `fabrica_da_celula.motivos_a_perguntar`: colab+dia e conjunto de motivos, excluindo só duplicata; retratada/conversa podem calar a volta (`app/chamados/services/fabrica_da_celula.py:118`; `app/chamados/services/fabrica_da_celula.py:123`; `app/chamados/services/fabrica_da_celula.py:126`).
  - Cron `disparar_perguntas_competencia`: colab+motivo+dia sem exclusões; classifica como fato já existente e abandona (`app/chamados/management/commands/disparar_perguntas_competencia.py:211`).
  - Diagnóstico `_marco_ja_perguntado`: colab+dia+motivos sem vias/conversa/instância; qualquer morta satisfaz “marco encerrado” (`app/chamados/services/pergunta_no_app.py:109`).
  - `insumos_em_regularizacao` agrupa por motivo+dia, após restringir à instância fixa de `linhas_dos_fatos`; também não representa I1/I2 (`app/chamados/services/declaracao_texto.py:547`; `app/chamados/services/declaracao_texto.py:550`).

Inventário A dos quatro campos delegados:

| Campo | Escritores de produção/históricos | Leitores |
|---|---|---|
| `instancia_marco` | `_chave_de_identidade`, preservando explícito (`app/chamados/models.py:3054`); backfill histórico (`app/chamados/migrations/0051_decisao_pergunta.py:27`) | `linhas_dos_fatos` (`app/chamados/models.py:2508`), constraint (`app/chamados/models.py:3090`), chave/grupos (`app/chamados/services/fio.py:1016`; `app/chamados/services/fio.py:1047`), guarda/índice histórico (`app/chamados/migrations/0053_pergunta_unica_por_familia.py:33`; `app/chamados/migrations/0053_pergunta_unica_por_familia.py:53`) |
| `batida_origem` **da PD** | `_nascer` (`app/chamados/models.py:2591`), `_chave_de_identidade` (`app/chamados/models.py:3057`), ambos pelo derivador de contexto que valida colab (`app/chamados/models.py:3069`); migração cria NULL (`app/chamados/migrations/0052_pergunta_batida_origem.py:19`) | `do_fato` (`app/chamados/models.py:2483`), `fato_livre_para` (`app/chamados/models.py:2525`), constraint (`app/chamados/models.py:3095`), `_colisao_ao_datar` (`app/chamados/services/fio.py:890`), chave/null/grupos (`app/chamados/services/fio.py:1017`; `app/chamados/services/fio.py:1021`; `app/chamados/services/fio.py:1042`), migrar (`app/chamados/services/fio.py:1108`), migração0053 (`app/chamados/migrations/0053_pergunta_unica_por_familia.py:35`) |
| `conversa` | `abrir=False` (`app/chamados/models.py:2541`), `abrir_conversa=True` (`app/chamados/models.py:2552`), persistência `_nascer` (`app/chamados/models.py:2642`); acervo defaultFalse (`app/chamados/migrations/0051_decisao_pergunta.py:99`) | `do_fato`/lote (`app/chamados/models.py:2484`; `app/chamados/models.py:2509`), `fato_livre_para` (`app/chamados/models.py:2523`), nascimento/trava (`app/chamados/models.py:2566`; `app/chamados/models.py:2586`), derivador de identidade (`app/chamados/models.py:3054`), constraints (`app/chamados/models.py:3091`; `app/chamados/models.py:3096`), fio `_no_fato`/migrar (`app/chamados/services/fio.py:1029`; `app/chamados/services/fio.py:1104`), migração0053 (`app/chamados/migrations/0053_pergunta_unica_por_familia.py:26`) |
| `colaborador` **denormalizado da PD** | `_chave_de_identidade` deriva/valida contra disputa só na inserção (`app/chamados/models.py:3038`; `app/chamados/models.py:3048`); migração0051 (`app/chamados/migrations/0051_decisao_pergunta.py:25`) | lote/porta (`app/chamados/models.py:2509`; `app/chamados/models.py:2521`), reapontar/colisão/rechavear/migrar (`app/chamados/services/fio.py:856`; `app/chamados/services/fio.py:889`; `app/chamados/services/fio.py:936`; `app/chamados/services/fio.py:1107`), grupos (`app/chamados/services/fio.py:1044`), backfill (`app/chamados/management/commands/backfill_decisoes_pergunta.py:62`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:100`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:124`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:139`), desfazer (`app/chamados/services/relavra_via.py:221`) |

`_achar_batida_origem` é função legada que recebe chamado, não leitora da nova coluna PD (`app/chamados/services/materializacao.py:109`). Na cópia não há outros escritores de produção encontrados desses quatro campos além dos acima; exaustividade de acessos dinâmicos [nao verificado].

F — **PROCEDENTE para `--ate`; `--colab` não fraciona um grupo íntegro por origem.**

- A chave BATIDA é batida+motivo (`app/chamados/services/fio.py:1017`), mas o código filtra linhas por data **antes** de formar o grupo e continua usando a base recortada para os PKs (`app/chamados/services/fio.py:1045`; `app/chamados/services/fio.py:1049`). Contraexemplo estático: três irmãs do mesmo episódio, duas antes do corte e a terceira respondida depois; a canônica global respondida some do cálculo, e uma das anteriores é retratada pela canônica local (`app/chamados/management/commands/dedup_perguntas_fato.py:66`; `app/chamados/management/commands/dedup_perguntas_fato.py:68`; `app/chamados/management/commands/dedup_perguntas_fato.py:83`).
- `--colab` é aplicado da mesma forma (`app/chamados/services/fio.py:1043`), porém uma BATIDA íntegra pertence a uma pessoa, e o derivador valida a origem contra o colaborador (`app/chamados/models.py:3076`). Há criação explícita por `**campos` preservando origem informada sem validação correspondente, portanto integridade universal dessa relação [nao verificado] (`app/chamados/models.py:2591`; `app/chamados/models.py:3057`).
- `dedup_perguntas_fato --apply` não consulta competência exportada nem emite PAREI: decide e grava diretamente (`app/chamados/management/commands/dedup_perguntas_fato.py:48`; `app/chamados/management/commands/dedup_perguntas_fato.py:81`). Também não toma trava do colaborador antes de retratar (`app/chamados/management/commands/dedup_perguntas_fato.py:81`; `app/chamados/management/commands/dedup_perguntas_fato.py:83`).
- O backfill usa outra proteção: verifica qualquer membro exportado (`app/chamados/management/commands/backfill_decisoes_pergunta.py:209`) e recusa antes de aplicar (`app/chamados/management/commands/backfill_decisoes_pergunta.py:234`); essa proteção não envolve o command dedup (`app/chamados/management/commands/dedup_perguntas_fato.py:35`).

E — limites dos testes da chave: [nao verificado]

- Os testes dedup exercitam escrita real e conferem vias persistidas; o MORDE remove o índice para montar duas irmãs (`app/chamados/tests/test_e1_v5_chave_familia.py:73`). Não são vazios para distinguir episódio e dia (`app/chamados/tests/test_e1_v5_chave_familia.py:67`).
- O RED de migração dos episódios distintos só verifica ausência de duplicata; uma migração inteiramente recusada também satisfaz essas asserções, pois não verifica destino ou `out['migradas']` (`app/chamados/tests/test_e1_v5_chave_familia.py:82`).
- O RED de recusa usa mock da porta e cobre apenas a reação do chamador ao retorno False (`app/chamados/tests/test_e1_v5_chave_familia.py:108`).
- A prova I1/I2 testa inserções cruas, sem exercitar `abrir`, `fato_livre_para`, reapontar ou migrar, onde a instância é perdida (`app/chamados/tests/test_e1_v5_indice.py:121`).
- Execução ou resultado dos testes [nao verificado]. [nao verificado]

## 07/11 10:34 · sessao edabd9a5f904 · - · - · origem=Ronald (tmux)

B — **NAO PROCEDE no caminho sequencial corrigido de reapontar**: `resolvedora_valida` e `_costura_nao_mata_o_fio` aceitam antes da retratação; recusa retorna sem retratar. A retratação e o save da substituta pertencem ao mesmo `atomic`. (`app/chamados/services/fio.py:844`, `:857`, `:860`, `:867`, `:871`.)

B — **PROCEDENTE quanto a caminho concorrente por canônica desatualizada**, inferência estática; ocorrência em produção [nao verificado]:

- `migrar_resolvedoras_vivas` captura `vivas_destino`, consulta o fato e escolhe canônica sem travar o conjunto; quando a canônica já está no destino, pula o reapontamento e retrata a fonte diretamente. (`app/chamados/services/fio.py:1095`, `:1107`, `:1112`, `:1113`, `:1122`.)
- O chamador real `reabrir` também não abre `atomic` ao redor desse ramo. (`app/chamados/models.py:1338`, `:1387`, `:1390`.)
- Entre a consulta de identidade e a retratação, a canônica pode sair por `remover_motivo`; essa porta retrata perguntas antes de qualquer trava de colaborador. A fonte depois pode virar duplicata de uma linha que já saiu do fato. (`app/chamados/models.py:2000`, `:2001`, `:2008`; `app/chamados/services/fio.py:1122`.)
- O backfill também relê por PK, mas não reaplica o predicado do fato: se a canônica do censo já saiu do fato, ainda pode ser escolhida. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:127`, `:128`, `:131`.)
- O dedup lê grupo e escolhe canônica antes do `atomic`, sem reler dentro. (`app/chamados/management/commands/dedup_perguntas_fato.py:66`, `:68`, `:81`, `:83`.)
- `retratar_duplicata` não verifica a existência atual da canônica no fato nem igualdade das chaves. (`app/chamados/services/fio.py:989`, `:993`, `:1002`.)

Chamadores runtime de `retratar_duplicata`: `reapontar_resolvedora` (`app/chamados/services/fio.py:868`); `migrar_resolvedoras_vivas` (`app/chamados/services/fio.py:1122`); `dedup_perguntas_fato.handle` (`app/chamados/management/commands/dedup_perguntas_fato.py:83`); `backfill_decisoes_pergunta.aplicar` (`app/chamados/management/commands/backfill_decisoes_pergunta.py:131`). Chamadas diretas em testes: `app/chamados/tests/test_e1_v5_portas_backfill.py:159`; `app/chamados/tests/test_porta_reapontamento_pergunta.py:390`, `:398`.

E — **NAO PROCEDE acusação de vazio nos casos concretos principais**:

- Dedup MORDE remove o índice, cria duplicatas e verifica `via_resolucao` recarregada do banco. (`app/chamados/tests/test_e1_v5_chave_familia.py:73`, `:75`, `:77`; leitura em `:54`.)
- ColisaoAoDatar MORDE monta destino de outro dia real, chama a porta sem mock e exige recusa, irmã preservada e uma linha no fato. Morde a antiga retratação anterior ao E4-c. (`app/chamados/tests/test_e1_v5_portas_backfill.py:133`, `:139`, `:142`, `:146`, `:148`.)
- FatoRetratadoVolta DATADO executa emissor, remoção e emissor reais, exigindo nova PK. Cobre o ramo de intervalo sem destino; não cobre a outra consulta de dedup dos motivos que precisam de destino. (`app/chamados/tests/test_e1_volta2.py:264`, `:271`, `:274`, `:282`, `:284`; `app/chamados/services/disputa_emissao.py:393`, `:410`.)

E — **PROCEDENTE como limitação de cobertura**:

- RED de migração com episódios distintos só exige ausência de duplicatas; não exige migração efetiva nem destino final. Uma implementação sem ação satisfaria essas asserções. (`app/chamados/tests/test_e1_v5_chave_familia.py:82`, `:87`, `:88`, `:89`.)
- MORDE da migração comprova a irmã retratada, mas não confere a canônica no destino. (`app/chamados/tests/test_e1_v5_chave_familia.py:91`, `:97`, `:98`.)
- RED de recusa mocka integralmente `reapontar_resolvedora`; prova o fluxo de retorno do migrador, não a recusa real ou sua atomicidade. O teste concreto ColisaoAoDatar fornece evidência complementar dessa classe. (`app/chamados/tests/test_e1_v5_chave_familia.py:108`, `:110`, `:111`; `app/chamados/tests/test_e1_v5_portas_backfill.py:142`.)

C — leitores adicionais encontrados e encaminhados ao explorador de identidade: fábrica usa `colab+dia→motivos` e só exclui duplicata (`app/chamados/services/fabrica_da_celula.py:118`, `:119`, `:123`, `:126`); command de disparo usa `colab+motivo+dia` sem vias/conversa/instância (`app/chamados/management/commands/disparar_perguntas_competencia.py:211`); candidata reusável usa `disputa+motivo+dia` (`app/chamados/services/declaracao_texto.py:236`); dedup do emissor usa `disputa+motivo+dia` (`app/chamados/services/disputa_emissao.py:1297`).

Não executei testes nem acessei banco/log; resultado de execução [nao verificado]. [nao verificado]

## 07/11 10:33 · sessao f3e0660912ad · - · - · origem=Ronald (tmux)

D — **PROCEDENTE**: `dedup_perguntas_fato --apply` abre `atomic` e chama `retratar_duplicata` sem travar Colaborador (`app/chamados/management/commands/dedup_perguntas_fato.py:81-83`). A chamada chega ao `UPDATE` da Pergunta por `carimbar_identidade` (`app/chamados/services/fio.py:1002`; `app/chamados/models.py:2812`); o `save` da Pergunta apenas abre outro `atomic`, sem pegar Colaborador (`app/chamados/models.py:3031-3041`).

D — **PROCEDENTE para a ordem completa**, apesar de vários caminhos agora pegarem Colaborador primeiro:

- `Disputa.fechar` no ramo administrativo carimba Pergunta antes de atualizar Disputa (`app/chamados/models.py:1877-1880`; `app/chamados/models.py:1895`).
- `fechar_o_fio_do_dia` carimba Pergunta antes de reconciliar Chamado (`app/ponto/services/veredito_celula.py:180`; `app/ponto/services/veredito_celula.py:192-193`); o reconciliar pode chegar à trava/UPDATE de Chamado (`app/chamados/models.py:687`; `app/chamados/models.py:277-285`).
- `_aceitar_a_folga` carimba Pergunta antes de chamar esse fechamento (`app/chamados/services/validacao.py:284`; `app/chamados/services/validacao.py:292`).
- `validar_pergunta` atualiza Pergunta por `_carimbar_validacao` antes de `devolver_validacao_recusada`, que pode reabrir Disputa ainda dentro do mesmo `atomic` (`app/chamados/services/validacao.py:417-422`; `app/chamados/services/validacao.py:451-453`; `app/chamados/services/validacao.py:217-218`; UPDATE da Disputa: `app/chamados/models.py:2124`).
- `desfazer_carimbo_sem_lastro` faz uma SELECT que trava **Pergunta e Disputa juntas**, por `select_for_update(of=('self', 'disputa'))`; não estabelece separadamente Disputa → Pergunta (`app/chamados/services/relavra_via.py:230-233`). A ordem efetiva de aquisição dessas duas linhas no banco é **[nao verificado]**.

D — **NÃO PROCEDE nos chamadores de produção encontrados quanto a `trava_do_colaborador` fora de transação**: o helper exige transação e avalia imediatamente a SELECT (`app/chamados/models.py:2450-2457`). Os três chamadores de `fechar_o_fio_do_dia` o invocam dentro de `atomic` (`app/ponto/services/veredito_celula.py:214`, `:337`; `app/ponto/services/declarar_dia.py:188`, `:206`; `app/chamados/services/validacao.py:281`, `:292`). Os quatro chamadores de `travar_origem_antes` também (`app/ponto/services/justificativa.py:131-132`, `:264-267`, `:333-334`; `app/ponto/views.py:1792-1793`). O helper não contém guarda explícita de `in_atomic_block`; uso externo diverso é **[nao verificado]** (`app/chamados/models.py:2456-2457`).

E — **PROCEDENTE quanto à cobertura insuficiente da ordem global**, sem vacuidade total: os testes novos da validação/desfazer exigem que Pergunta apareça na captura (`app/chamados/tests/test_e1_v5_ordem_trava.py:249`; `:259`) e possuem mutantes que removem a trava do Colaborador (`:252-255`; `:262-265`). Porém descartam deliberadamente violações da subordem entre Disputa/Chamado/Pergunta: `_sem_colab_antes` filtra somente a mensagem `"sem o Colaborador antes"` (`:230-231`), conforme a própria docstring (`:201-204`).

E — **PROCEDENTE quanto às cegueiras do capturador**: `travas()` registra somente a tabela do `FROM` para SELECT FOR UPDATE (`app/chamados/tests/test_e1_v5_ordem_trava.py:29`; `:43-47`), ignorando a Disputa também travada pelo JOIN de `relavra_via.py:230`. Além disso, elimina repetições por **tabela**, embora a reentrância dependa da linha (`app/chamados/tests/test_e1_v5_ordem_trava.py:38`; `:45-47`). O teste de fechar fio não monta Chamado (`:162-168`), e o teste de fechar disputa monta disputa vazia (`:170-174`): não cobrem os ramos acima com Pergunta → Chamado ou Pergunta → Disputa.

Execução dos testes, incidência em produção e ocorrência efetiva de deadlock: **[nao verificado]**. [nao verificado]

## 07/11 10:15 · sessao 3be797b389d4 · FIO-E0-V4-E1 · verificador · origem=Code

**Não aprovo a árvore atual:** há recusa com escrita persistida, inversão de trava e consumidores que ainda usam uma identidade diferente da declarada por família. Fontes: `app/chamados/services/fio.py:849`, `app/chamados/services/validacao.py:417`, `app/chamados/services/disputa_emissao.py:321`.

A comparação dos dois diffs ficou **[nao verificado]**: `git diff` falhou porque esta cópia não contém metadados Git. Portanto, não atribuo os achados especificamente ao delta da rodada 1. Execução da suíte, ocorrência em produção e quantidades de passivo: **[nao verificado]**. [nao verificado]

**Achados**

1. **BLOQUEIA — reapontamento recusado pode deixar outra pergunta retratada.**  
   `reapontar_resolvedora` resolve a colisão em `app/chamados/services/fio.py:849`; `_colisao_ao_datar` já retrata as outras perguntas em `app/chamados/services/fio.py:893`. Só depois são verificados o dia e o estado do destino, em `app/chamados/services/fio.py:856` e `app/chamados/services/fio.py:858`.

   **Contraexemplo:** P é antiga, sem dia e sem resposta; Q é mais nova, datada D e viva. P vence como canônica pelo menor PK (`app/chamados/services/fio.py:979`). Ofereça destino de D+7: Q vira `duplicata`, o juiz recusa o destino (`app/chamados/juizes.py:2000`), e a função restaura somente a data de P e retorna `False`, sem rollback (`app/chamados/services/fio.py:859`). O retorno normal confirma a retratação dentro do `atomic` (`app/chamados/services/fio.py:844`).

   Os selos exercitam sucesso e recusa **anterior** à retratação; falta a recusa posterior por destino inválido ou terminal. Fontes: `app/chamados/tests/test_e1_v5_portas_backfill.py:106`, `app/chamados/tests/test_e1_v5_portas_backfill.py:119`.

2. **BLOQUEIA — validação de HORA ainda trava Pergunta antes de Colaborador.**  
   O caminho começa no `atomic` de `validar_pergunta`, chama `_carimbar_validacao` e executa UPDATE da Pergunta antes de adquirir Colaborador. Fontes: `app/chamados/services/validacao.py:417`, `app/chamados/services/validacao.py:418`, `app/chamados/services/validacao.py:137`.

   Depois, no mesmo ato, segue para materialização → registro da batida → reconciliação → aquisição de Colaborador. Caminho: `app/chamados/services/validacao.py:446` → `app/chamados/services/materializacao.py:1213` → `app/chamados/services/materializacao.py:918` → `app/ponto/registro_batida.py:157` → `app/ponto/signals.py:215` → `app/chamados/reconciliador.py:1427` → `app/chamados/reconciliador.py:1450` → `app/chamados/reconciliador.py:523` → `app/chamados/models.py:686`.

   **Contraexemplo concorrente:** T1 valida HORA, segura Pergunta P e pede Colaborador C. T2 fecha o fio do dia, segura C e pede UPDATE de P. O caminho inverso está em `app/ponto/services/veredito_celula.py:175`, `app/ponto/services/veredito_celula.py:180` e `app/chamados/models.py:2812`. A ocorrência efetiva de deadlock em produção é **[nao verificado]**.

3. **BLOQUEIA pelo “SEMPRE” literal — a ordem completa também se inverte depois do Colaborador.**

   | Caminho | Contraexemplo e inversão |
   |---|---|
   | `DisputaSupervisao.fechar` | `admin_encerra` com pergunta muda viva grava Pergunta antes do UPDATE da Disputa: `app/chamados/models.py:1878`, `app/chamados/models.py:1895`. |
   | `fechar_o_fio_do_dia` | Dia com pergunta e chamado vivos grava Pergunta antes de reconciliar Chamado: `app/ponto/services/veredito_celula.py:180`, `app/ponto/services/veredito_celula.py:192`. |
   | Reapontamento com colisão | Retrata Pergunta antes de adquirir Chamado: `app/chamados/services/fio.py:894`, `app/chamados/services/fio.py:858`, `app/chamados/services/fio.py:769`. |
   | `desfazer_carimbo_sem_lastro` | Trava Pergunta e Disputa conjuntamente, sem adquirir Colaborador nessa entrada: `app/chamados/services/relavra_via.py:217`, `app/chamados/services/relavra_via.py:221`. Pode depois reabrir a Disputa: `app/chamados/services/relavra_via.py:246`. |

   Adquirir Colaborador primeiro nos três primeiros caminhos serializa as entradas conformes daquela pessoa, mas essas sequências continuam diferentes da ordem integral prometida em `app/chamados/models.py:2451`.

4. **BLOQUEIA — emissor de BATIDA ainda deduplica por disputa/motivo.**  
   `gerar_perguntas_disputa` interrompe pelo `exists()` de `(disputa,motivo)` antes de chegar à porta. GPS e foto estão nesse ramo, pois não pertencem a `MOTIVOS_DATADOS_SET`. Fontes: `app/chamados/services/disputa_emissao.py:26`, `app/chamados/services/disputa_emissao.py:321`, `app/chamados/models.py:3262`.

   **Contraexemplo:** B1 e B2, ambas sem GPS, na mesma disputa: a pergunta de B1 impede a de B2. A chave declarada admite ambos os episódios: `(batida_origem,motivo)`. A entrada de produção volta a chamar o emissor em `app/chamados/services/disputa_emissao.py:662`; o índice está em `app/chamados/models.py:3095`.

   O selo de episódios distintos cria as duas linhas diretamente, sem atravessar esse emissor; portanto, não morde essa falha. Fonte: `app/chamados/tests/test_e1_v5_indice.py:84`.

5. **BLOQUEIA — migração de resolvedoras trata episódios distintos como duplicatas.**  
   `migrar_resolvedoras_vivas` compara apenas motivo e dia; depois escolhe uma canônica e retrata a outra. Fontes: `app/chamados/services/fio.py:1043`, `app/chamados/services/fio.py:1047`, `app/chamados/services/fio.py:1049`.

   **Contraexemplo:** perguntas vivas `gps_ausente` de B1 e B2, no mesmo dia, em chamados diferentes. A reabertura redirecionada migra as resolvedoras e elimina uma cobrança legítima como duplicata. O chamador de produção está em `app/chamados/models.py:1387`; a identidade por episódio está em `app/chamados/models.py:2484`.

   O executor antigo de dedup conserva a mesma derivação por colaborador/motivo/dia, também sem episódio, conversa ou instância. Um `--apply --ate` abrangendo esses fatos pode repetir o erro. Fontes: `app/chamados/management/commands/dedup_perguntas_fato.py:57`, `app/chamados/management/commands/dedup_perguntas_fato.py:68`, `app/chamados/management/commands/dedup_perguntas_fato.py:88`.

6. **BLOQUEIA — pergunta datada retratada ainda impede o fato que retorna.**  
   Os dedups de intervalo e de motivos com chamado-destino fazem `exists()` sem excluir `VIAS_FORA_DO_FATO`. Fontes: `app/chamados/services/disputa_emissao.py:391`, `app/chamados/services/disputa_emissao.py:407`.

   **Contraexemplo:** uma disputa conserva outro motivo; `intervalo_saida` é removido e sua pergunta é retratada. O intervalo volta a ser cobrado no mesmo dia: o emissor encontra a linha retratada e para antes da porta. A retratação ocorre em `app/chamados/models.py:2001`; o contrato “fato voltou → pergunta nova” está em `app/chamados/models.py:1993`.

   O teste existente cobre o retorno de `posto_desativado`, que passa pelo dedup não datado já corrigido; não cobre esses dois ramos datados. Fontes: `app/chamados/tests/test_e1_volta2.py:244`, `app/chamados/services/disputa_emissao.py:322`.

7. **FILA — selo de trava confunde tabela com linha.**  
   O coletor descarta toda segunda aquisição da mesma tabela e observa somente a primeira tabela do `FROM`. Fontes: `app/chamados/tests/test_e1_v5_ordem_trava.py:29`, `app/chamados/tests/test_e1_v5_ordem_trava.py:38`, `app/chamados/tests/test_e1_v5_ordem_trava.py:45`.

   **Contraexemplo:** `Colaborador A → Chamado A → Colaborador B`. A última aquisição desaparece de `seq`, porque a tabela Colaborador já foi vista. O mesmo coletor não registra separadamente a Disputa adquirida no JOIN de `FOR UPDATE OF self,disputa`. Fontes: `app/chamados/tests/test_e1_v5_ordem_trava.py:46`, `app/chamados/services/relavra_via.py:221`.

   O caso de fechamento usa disputa vazia e deixa de exercer a inversão com perguntas vivas do achado 3. Fonte: `app/chamados/tests/test_e1_v5_ordem_trava.py:170`.

**A — campos: escritores, leitores e significado**

| Campo | Escrita encontrada | Leitura e significado |
|---|---|---|
| `batida_origem` | Nascimento por `_nascer` e `_chave_de_identidade`; deriva o ID do contexto e confere o colaborador. `app/chamados/models.py:2591`, `app/chamados/models.py:3057`, `app/chamados/models.py:3076`. | Episódio de BATIDA em `do_fato`, índice e backfill. Há divergência nos achados 4/5. `app/chamados/models.py:2484`, `app/chamados/models.py:3095`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:83`. |
| `data_evento` | Nascimento; reapontamento quando ausente; rechaveamento explícito. `app/chamados/models.py:2641`, `app/chamados/services/fio.py:841`, `app/chamados/services/fio.py:943`. | **Dois significados discriminados por `conversa`:** dia do turno para fato; dia civil em que foi feita para conversa. Plantio lê como dia do fato; resolvedora compara turno; lavra deriva janela. `app/chamados/models.py:2535`, `app/chamados/models.py:2546`, `app/chamados/services/materializacao.py:344`, `app/chamados/juizes.py:1965`, `app/chamados/veredito_lavrado.py:95`. |
| `instancia_marco` | Derivada no nascimento quando factual e vazia. `app/chamados/models.py:3054`. | Posição do marco, pelo mapa de quatro motivos; lida pelo domínio do fato e índice MARCO. `app/chamados/services/declaracao_texto.py:42`, `app/chamados/models.py:2508`, `app/chamados/models.py:3090`. |
| `colaborador` | Inserção exige igualdade com o colaborador da Disputa. `app/chamados/models.py:3048`. | Identidade lê a coluna; consumidores também chegam pela Disputa. O significado declarado é a mesma pessoa. `app/chamados/models.py:2509`, `app/chamados/services/materializacao.py:522`. |
| `conversa` | `_nascer`; `abrir` fixa `False`, `abrir_conversa` fixa `True`. `app/chamados/models.py:2541`, `app/chamados/models.py:2552`, `app/chamados/models.py:2641`. | Conversa fica fora do domínio do fato e dos índices. `app/chamados/models.py:2509`, `app/chamados/models.py:3091`, `app/chamados/models.py:3096`. |
| `via_resolucao` | `_escrever_carimbo`; descarimbo em lote; encerramento por fato no espelho; relavra. `app/chamados/models.py:2720`, `app/chamados/models.py:2882`, `app/chamados/models.py:2893`, `app/chamados/services/relavra_via.py:123`. | C6, filtro SQL, domínio do fato, índices e backfill. `app/chamados/juizes.py:1147`, `app/chamados/models.py:2968`, `app/chamados/models.py:2486`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:127`. |
| `DecisaoPergunta.ato` | `registrar`, com `bulk_create`; recebe resposta, validação, devolução, recusa/reabertura, absorção, retratação e silêncio dos respectivos escritores. `app/chamados/models.py:3550`, `app/chamados/services/disputa_emissao.py:1369`, `app/chamados/services/validacao.py:139`, `app/chamados/services/materializacao.py:560`, `app/chamados/models.py:2003`, `app/ponto/services/veredito_celula.py:185`. | Histórico, recuperação da projeção, checks e backfill distinguem o tipo do ato. `app/chamados/models.py:3113`, `app/chamados/models.py:3532`, `app/chamados/models.py:3497`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:125`. |
| `DecisaoPergunta.via_antes` | **Não é campo do modelo.** Há variável local para auditoria e via anterior embutida no texto da retratação. `app/chamados/models.py:3476`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:167`, `app/chamados/models.py:2006`. | Não há coluna estruturada `via_antes` entre os campos declarados. `app/chamados/models.py:3476`. |

A competência do backfill é `(empresa, mês, ano)`; `_exportada` passa essa mesma ordem para `competencia_entregue`. O grupo BATIDA é barrado se **qualquer membro** estiver em competência exportada. Fontes: `app/chamados/management/commands/backfill_decisoes_pergunta.py:145`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:239`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:246`, `app/folha/export.py:556`.

O intervalo do adaptador de competências é inclusivo `[de,ate]`; nesse backfill a consulta é de um único dia, via `competencia_do_fato`. Fontes: `app/ponto/services/fechamento.py:1567`, `app/ponto/services/fechamento.py:1595`.

**A — funções solicitadas**

| Função | Quem chama / o que lê e escreve |
|---|---|
| `_colisao_ao_datar` | Chamada pelo reapontamento; lê domínio do fato, canônica e vitalidade; retrata outras linhas. `app/chamados/services/fio.py:850`, `app/chamados/services/fio.py:883`, `app/chamados/services/fio.py:894`. |
| `reapontar_resolvedora` | Consumida por modelo, reconciliador, declaração e limbo; escreve destino/data e lavra. `app/chamados/models.py:431`, `app/chamados/reconciliador.py:944`, `app/chamados/services/declaracao_texto.py:330`, `app/chamados/services/limbo_folgas.py:243`, `app/chamados/services/fio.py:865`. |
| `carimbar` | Reconciliação, materialização e resposta leem seu resultado; consulta C6 e grava o carimbo, com resposta/validação opcionais. `app/chamados/reconciliador.py:586`, `app/chamados/services/materializacao.py:1268`, `app/chamados/services/disputa_emissao.py:1781`, `app/chamados/models.py:2762`, `app/chamados/models.py:2775`. |
| `carimbar_identidade` | Fechamento administrativo, dedup e retratação; grava carimbo/recusa sem rejulgar C6. `app/chamados/models.py:2687`, `app/chamados/services/fio.py:997`, `app/chamados/models.py:2008`, `app/chamados/models.py:2812`. |
| `descarimbar` / lote | Volta da resolução e reabertura; limpam os três campos juntos; o lote exclui vias fora do fato no próprio UPDATE. `app/chamados/services/relavra_via.py:231`, `app/chamados/models.py:2830`, `app/chamados/models.py:2882`. |
| `encerrar_por_fato_no_espelho` | Chamado pelo executor específico; escreve somente a via, com guarda de fora do fato na instância. `app/chamados/management/commands/encerrar_perguntas_com_fato.py:68`, `app/chamados/models.py:2891`, `app/chamados/models.py:2893`. |
| `fechar_por_admin` | Disputa, aceite de folga e fio do dia; delega ao carimbo de identidade. `app/chamados/models.py:1880`, `app/chamados/services/validacao.py:285`, `app/ponto/services/veredito_celula.py:180`, `app/chamados/models.py:2687`. |
| `DisputaSupervisao.fechar` | Lê respostas pendentes/vitalidade; fecha mudas, registra SILENCIO e grava fechamento. `app/chamados/models.py:1849`, `app/chamados/models.py:1859`, `app/chamados/models.py:1883`, `app/chamados/models.py:1895`. |
| `travar_origem_antes` | Aprovação, rejeição, materialização de justificativa e view; adquire Colaborador antes dos Chamados. `app/ponto/services/justificativa.py:132`, `app/ponto/services/justificativa.py:267`, `app/ponto/services/justificativa.py:334`, `app/ponto/views.py:1793`, `app/ponto/services/justificativa.py:111`. |
| `validacao` | `validar_pergunta` carimba e materializa; folga passa pelo declarador. A entrada HORA conserva o achado 2. `app/chamados/services/validacao.py:365`, `app/chamados/services/validacao.py:313`, `app/chamados/services/validacao.py:418`. |
| `veredito_celula` | Adquire Colaborador na entrada e fecha o fio no ato do veredito. `app/ponto/services/veredito_celula.py:216`, `app/ponto/services/veredito_celula.py:337`. |
| `fechar_o_fio_do_dia` | Consumido pelo veredito e declarador; fecha perguntas, registra SILENCIO e reconcilia chamados. `app/ponto/services/veredito_celula.py:185`, `app/ponto/services/veredito_celula.py:192`, `app/ponto/services/declarar_dia.py:206`. |
| `declarar_dia` | Percorre os dias; folga adquire Colaborador e fecha fio; falta delega ao veredito. `app/ponto/services/declarar_dia.py:165`, `app/ponto/services/declarar_dia.py:190`, `app/ponto/services/declarar_dia.py:206`, `app/ponto/services/declarar_dia.py:215`. |
| `reatar_beco_folga_ao_limbo` | Executor adquire Colaborador antes do Chamado; usa a porta de reapontamento. `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:104`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:105`, `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:127`. |
| `relavra_via` | Relavra apenas vias derivadas; escreve via e lavra. Desfazer carimbo relê Pergunta/Disputa travadas e pode reabrir a Disputa. `app/chamados/services/relavra_via.py:116`, `app/chamados/services/relavra_via.py:125`, `app/chamados/services/relavra_via.py:221`, `app/chamados/services/relavra_via.py:246`. |
| `backfill._exportada` | Consumida na classificação dos grupos e decisão de recusar apply; delega à autoridade de exportação. `app/chamados/management/commands/backfill_decisoes_pergunta.py:235`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:246`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:255`. |

**B — escritores fora da porta e identidade vazia**

A criação produtiva encontrada converge em `_nascer`; os dois escritores de data existente estão em `fio`. O UPDATE em lote de carimbo permanece na porta, e o `bulk_update` encontrado atualiza a lavra. Fontes: `app/chamados/models.py:2641`, `app/chamados/services/fio.py:865`, `app/chamados/services/fio.py:949`, `app/chamados/models.py:2882`, `app/chamados/veredito_lavrado.py:207`. Ausência absoluta de escrita por SQL dinâmico: **[nao verificado]**.

Não encontrei conversão produtiva `conversa=True → False`; as portas fixam o valor no nascimento. **BATIDA factual pode nascer com `instancia_marco=''`**, coerentemente com seu índice por episódio; os quatro motivos MARCO recebem a posição do mapa. Fontes: `app/chamados/models.py:2541`, `app/chamados/models.py:2552`, `app/chamados/models.py:3054`, `app/chamados/services/declaracao_texto.py:42`, `app/chamados/models.py:3095`.

As guardas individuais de terminalidade leem a instância recebida, enquanto o lote filtra o estado no banco. A proteção contra instância antiga após retratação concorrente permanece **[nao verificado]**; fontes da diferença: `app/chamados/models.py:2802`, `app/chamados/models.py:2828`, `app/chamados/models.py:2891`, `app/chamados/models.py:2883`.

**D — fixtures**

Na fixture das órfãs, reaproveitar a linha emitida e restaurá-la viva preserva a prova local: ela exige uma muda encerrada, disputa barrada identificando a respondida e resposta ainda sem materialização. Fontes: `app/chamados/tests/test_orfas_nao_morre_na_guarda_da_porta.py:86`, `app/chamados/tests/test_orfas_nao_morre_na_guarda_da_porta.py:92`, `app/chamados/tests/test_orfas_nao_morre_na_guarda_da_porta.py:107`, `app/chamados/tests/test_orfas_nao_morre_na_guarda_da_porta.py:116`, `app/chamados/tests/test_orfas_nao_morre_na_guarda_da_porta.py:129`.

Os testes de acervo duplicado continuam montando duplicatas sem índice e exigindo sua reinstalação depois da cura. Fontes: `app/chamados/tests/test_e1_v5_portas_backfill.py:167`, `app/chamados/tests/test_e1_v5_portas_backfill.py:189`, `app/chamados/tests/test_e1_v5_portas_backfill.py:205`. Comparação antes/depois dos aproximadamente 30 módulos e eventual perda de outras mordidas: **[nao verificado]**.

**E — seis tetos**

A justificativa de **um SELECT FOR UPDATE por entrada** corresponde aos três pontos de aquisição; não há laço nesses trechos. Fontes: `app/chamados/models.py:285`, `app/chamados/models.py:686`, `app/chamados/services/fio.py:768`, `app/chamados/models.py:2456`.

Isso não significa uma aquisição por transição inteira: `cancelar` pode passar pelos dois helpers, e a costura de pergunta existente ocorre dentro da emissão por motivo. Fontes: `app/chamados/models.py:758`, `app/chamados/models.py:764`, `app/chamados/models.py:2611`, `app/chamados/services/disputa_emissao.py:312`.

Os tetos publicados são 20, 52, 12, 192, 208 e 8; a re-medição dos números citados nos comentários ficou **[nao verificado]**. Fontes: `app/ponto/tests/test_selo_chokepoint_escrita.py:116`, `:118`, `:119`, `:161`, `:187`, `:193`.

**F — mordida dos selos**

Há asserções úteis sobre índice real, famílias, preservação das linhas, idempotência e recusa de exportada. Fontes: `app/chamados/tests/test_e1_v5_indice.py:62`, `app/chamados/tests/test_e1_v5_familia.py:40`, `app/chamados/tests/test_e1_v5_portas_backfill.py:189`, `app/chamados/tests/test_e1_v5_portas_backfill.py:247`.

Escapam os mutantes concretos dos achados: manter o dedup antigo no emissor, comp

[truncado em 20000 de 21365 caracteres]

## 07/11 10:11 · sessao fa977c4870a5 · - · - · origem=Ronald (tmux)

Achados confirmados no código atual; pertencimento ao delta `[nao verificado]`, pois a cópia não tem `.git`.

1. **Bloqueia — emissor ainda deduplica BATIDA por disputa/motivo.** `gerar_perguntas_disputa` interrompe em `app/chamados/services/disputa_emissao.py:321-324`; `gps_ausente` não pertence aos datados (`:26`, `app/chamados/models.py:3262`). Contraexemplo: B1 e B2 no mesmo dia, ambas sem GPS, mesma disputa: a primeira pergunta impede a segunda, embora a identidade declarada permita dois episódios (`app/chamados/models.py:3095`). Entrada real: `criar_ou_atualizar_disputa` chama o emissor em `app/chamados/services/disputa_emissao.py:662`. O teste de episódios distintos cria linhas diretamente, sem atravessar esse emissor (`app/chamados/tests/test_e1_v5_indice.py:84-88`).

2. **Bloqueia — segundo dedup destrói episódio distinto.** `migrar_resolvedoras_vivas` identifica conflito apenas por motivo e dia (`app/chamados/services/fio.py:1043-1044`), escolhe canônica e retrata outra (`:1046-1050`). Contraexemplo: perguntas vivas `gps_ausente` de B1 e B2, mesmo dia, em chamados diferentes; reabrir o antigo migra suas resolvedoras e declara um episódio distinto “duplicata”. Cadeia real: `ChamadoColaborador.reabrir` → migração (`app/chamados/models.py:1387-1391`). Diverge da chave `(batida_origem,motivo)` (`app/chamados/models.py:3095`).

3. **Bloqueia — dedups datados conservam a pergunta retratada como impedimento.** Os `exists()` de intervalo (`app/chamados/services/disputa_emissao.py:391-393`) e motivos com destino (`:407-409`) não excluem `VIAS_FORA_DO_FATO`. Contraexemplo: retirar `intervalo_saida` de disputa ainda aberta retrata a pergunta (`app/chamados/models.py:2001-2009`); o fato retorna naquele dia e o emissor para antes da porta. Contraria a semântica explícita “fato voltou → pergunta nova” (`app/chamados/models.py:2347-2349`). O dedup não datado já exclui essas vias (`app/chamados/services/disputa_emissao.py:322-324`).

4. **Bloqueia — confirmação independente do pouso antes do requisito.** P sem dia, menor PK, e Q datada viva: `canonica_do_grupo` escolhe P (`app/chamados/services/fio.py:979-981`); `_colisao_ao_datar` retrata Q (`:893-894`). Depois o destino de D+7 é recusado (`app/chamados/juizes.py:2000`), e `reapontar_resolvedora` retorna normalmente, restaurando apenas o dia em memória de P (`app/chamados/services/fio.py:859-862`). O `atomic` aberto em `:844` confirma Q como duplicata mesmo sem completar a costura.

Mapa de identidade e escrita:

| Campo | Escritor | Leitor/semântica |
|---|---|---|
| `batida_origem` | Nascimento: `app/chamados/models.py:3057-3058`; derivação pelo ID do contexto, validado contra colaborador em `:3072-3076`; porta em `:2591-2594` | `do_fato` por episódio em `:2484`; índice em `:3095`; backfill em `app/chamados/management/commands/backfill_decisoes_pergunta.py:83`. Emissor e migração divergem nos achados 1/2. |
| `instancia_marco` | Apenas nascimento em `app/chamados/models.py:3054-3056` | Quatro posições em `app/chamados/services/declaracao_texto.py:42-57`; leitura em `app/chamados/models.py:2508`; índice em `:3090`. BATIDA factual nasce normalmente com `''`; os quatro motivos MARCO recebem posição. |
| `conversa` | Nascimento em `app/chamados/models.py:2641-2642`; portas fixam False em `:2541` ou True em `:2552` | Fato exclui conversa em `:2509`, índices em `:3091/:3096`. Não encontrei escritor de produção que converta True→False `[nao verificado]` quanto a SQL dinâmico fora da análise estática. |
| `colaborador` | Inserção em `app/chamados/models.py:3048-3053`, exigindo igualdade com disputa | Identidade em `:2509`; backfill em `app/chamados/management/commands/backfill_decisoes_pergunta.py:82`; consumidores também usam colaborador da disputa, por exemplo `app/chamados/services/materializacao.py:522`. |
| `data_evento` | Nascimento em `app/chamados/models.py:2641`; reapontar em `app/chamados/services/fio.py:841/:865`; rechavear em `:943/:949` | Dia do turno para fato (`app/chamados/models.py:2535-2541`), dia civil para conversa (`:2546-2552`). Plantio lê em `app/chamados/services/materializacao.py:344-349`; juiz de destino em `app/chamados/juizes.py:1965`; janela inclusive em `:589`. |
| `via_resolucao` | `_escrever_carimbo` em `app/chamados/models.py:2720-2724`; lote em `:2882-2884`; encerramento em `:2893-2894`; relavra em `app/chamados/services/relavra_via.py:123-125` | C6 em `app/chamados/juizes.py:1147/:1173`; SQL em `app/chamados/models.py:2968-2970`; exclusão da identidade em `:2486/:2509`; índices em `:3092/:3097`. |
| `DecisaoPergunta.ato` | `registrar`, único `bulk_create`, em `app/chamados/models.py:3548-3550` | Histórico em `:3113-3119`; recuperação da projeção em `:3532-3544`; backfill em `app/chamados/management/commands/backfill_decisoes_pergunta.py:125/:180`. |
| `DecisaoPergunta.via_antes` | **Campo inexistente** no modelo (`app/chamados/models.py:3476-3489`) | Há variável de auditoria no backfill (`app/chamados/management/commands/backfill_decisoes_pergunta.py:167/:172`) e via anterior embutida em `ato_motivo` da retratação (`app/chamados/models.py:2006-2007`). |

Não encontrei criação de produção de `PerguntaDisputa` fora de `_nascer` (`app/chamados/models.py:2641`). Os `bulk_update` de perguntas encontrados atualizam a lavra (`app/chamados/veredito_lavrado.py:207`), e o lote que muda via passa por `descarimbar_em_lote` (`app/chamados/models.py:2882-2884`). Ausência absoluta de escritor dinâmico `[nao verificado]`.

Risco adicional a confirmar: guardas `fora_do_fato()` leem a instância recebida, sem recarga sob trava (`app/chamados/models.py:2743/:2802/:2828/:2891`). Uma instância carregada antes de retratação concorrente pode sobrepor a via terminal; o interleaving completo de produção `[nao verificado]`.

## 07/11 10:11 · sessao d454790e366b · - · - · origem=Ronald (tmux)

**bloqueia — validação HORA ainda prende Pergunta antes de Colaborador.**  
`validar_pergunta` abre transação e chama `_carimbar_validacao` antes de qualquer trava do colaborador (`app/chamados/services/validacao.py:417-418`); o primeiro ato é UPDATE da pergunta (`app/chamados/services/validacao.py:137`). Depois materializa a resposta (`app/chamados/services/validacao.py:446` → `app/chamados/services/materializacao.py:1211-1213` → `app/chamados/services/materializacao.py:918`), cuja batida aciona reconciliação (`app/ponto/signals.py:207-215`). Um destino cuja premissa morreu chega a `_retratar` (`app/chamados/reconciliador.py:1450`, `:522-523`) e trava Colaborador (`app/chamados/models.py:684-687`).

Contraexemplo: T1 valida HORA, segura Pergunta P e depois pede Colaborador C; T2 executa `fechar_o_fio_do_dia`, segura C (`app/ponto/services/veredito_celula.py:175`) e pede UPDATE de P (`:180` → `app/chamados/models.py:2812`). Há ciclo estático; ocorrência real em produção `[nao verificado]`.

**bloqueia — colisão retrata antes de verificar se o reapontamento pode acontecer.**  
`reapontar_resolvedora` chama `_colisao_ao_datar` (`app/chamados/services/fio.py:849-850`), que retrata outras perguntas (`:893-894`), antes de conferir a resolvedora e o estado do destino (`:856-858`). A recusa apenas restaura `p.data_evento` e retorna False (`:859-862`); o atomic não recebe rollback (`:844`).

Contraexemplo: P antiga, sem dia, canônica; Q mais nova e viva, já datada D; destino com dia incompatível D+10. Q ganha `duplicata`; o juiz recusa o destino; P continua sem dia e Q permanece retratada. A recusa produz escrita persistente e deixa o fato D sem sua linha anterior (`app/chamados/services/fio.py:883-894`, `:859-862`).

**bloqueia pelo pedido literal — a ordem completa ainda se inverte após Colaborador.**

- `Disputa.fechar(admin_encerra)` grava perguntas (`app/chamados/models.py:1877-1880`) antes do UPDATE da Disputa (`:1895`); seu signal depois reconcilia o Chamado-pai (`app/chamados/signals.py:41-42`). Contraexemplo: disputa com uma pergunta muda viva.
- `fechar_o_fio_do_dia` grava perguntas (`app/ponto/services/veredito_celula.py:180`) antes de reconciliar chamados (`:192-193`). Contraexemplo: dia com pergunta e chamado vivos.
- Colisão ao datar grava Pergunta (`app/chamados/services/fio.py:893-894` → `app/chamados/models.py:2812`) antes de travar Chamado (`app/chamados/services/fio.py:858`, `:769`).
- `desfazer_carimbo_sem_lastro` trava Pergunta e Disputa conjuntamente sem Colaborador (`app/chamados/services/relavra_via.py:217-224`), descarimba (`:231`) e pode atualizar Disputa (`:246` → `app/chamados/models.py:2124`).

O Colaborador previamente adquirido nos três primeiros caminhos serializa entradas conformes da mesma pessoa, mas as sequências não cumprem a ordem literal solicitada (`app/chamados/models.py:2451-2455`).

**fila — selo de trava não cobre os contraexemplos e confunde tabela com linha.**  
O teste de fechar disputa usa disputa vazia (`app/chamados/tests/test_e1_v5_ordem_trava.py:170-174`); não exerce `admin_encerra` com pergunta viva. Os demais casos cobrem analisar, nascimento, reapontamento simples, rechaveamento, fio e justificativa (`:88-119`, `:162-185`), deixando a validação HORA acima fora do selo.

O coletor elimina repetições por **tabela** (`app/chamados/tests/test_e1_v5_ordem_trava.py:38`, `:45-47`). Mutante não detectado: após Colaborador A → Chamado A, adquirir Colaborador B; a tabela Colaborador já consta de `vistas`. Também registra somente o primeiro FROM do SELECT (`:29`, `:44`), deixando invisível a trava adicional da Disputa num `FOR UPDATE OF self,disputa` (`app/chamados/services/relavra_via.py:221-222`).

**Mapa solicitado, recorte de funções/travas.**

| Função | Leitura/escrita e consumidores |
|---|---|
| `reapontar_resolvedora` | Lê identidade/destino; escreve `data_evento`, `chamado_destino` e lavra (`app/chamados/services/fio.py:839-866`). Consumidores: modelo (`app/chamados/models.py:431`), reconciliador (`app/chamados/reconciliador.py:944`, `:1628`, `:1641`), declaração (`app/chamados/services/declaracao_texto.py:330`), limbo (`app/chamados/services/limbo_folgas.py:243`, `:276`). |
| `carimbar` | Escreve trio do carimbo e resposta/validação opcionais; consulta C6 antes do save (`app/chamados/models.py:2751-2775`). |
| `carimbar_identidade` | Escreve trio e recusa opcionais, sem C6; bloqueia retorno de linha fora do fato (`app/chamados/models.py:2800-2812`). |
| `descarimbar` / lote | Escrevem trio vazio; recusam/excluem linhas fora do fato (`app/chamados/models.py:2825-2834`, `:2874-2884`). |
| `fechar_por_admin` | Delega ao carimbo de identidade (`app/chamados/models.py:2673-2687`); chamado pelo fechamento de disputa (`:1880`), validação de folga (`app/chamados/services/validacao.py:285`, `:321`) e fio do dia (`app/ponto/services/veredito_celula.py:180`). |
| `encerrar_por_fato_no_espelho` | Porta em `app/chamados/models.py:2886`; gatilho em `app/chamados/management/commands/encerrar_perguntas_com_fato.py:68`. |
| `travar_origem_antes` | Colaborador antes dos Chamados (`app/ponto/services/justificativa.py:111`, `:120`); chamadas nas aprovações/rejeição/materialização (`:132`, `:267`, `:334`) e view (`app/ponto/views.py:1793`). |
| `veredito_celula` | Atomic e Colaborador na entrada (`app/ponto/services/veredito_celula.py:214-216`); fecha fio no mesmo ato (`:337`). |
| `declarar_dia` | Percorre dias (`app/ponto/services/declarar_dia.py:165-168`); folga trava Colaborador (`:188-190`) e fecha fio (`:206`); falta delega ao veredito (`:215`). |
| `reatar_beco_folga_ao_limbo` | Colaborador antes do pai Chamado (`app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:99-105`); reversão usa reapontamento (`:127`). |
| `relavra_via` | `relavrar` escreve via e lavra diretamente (`app/chamados/services/relavra_via.py:123-126`), dentro do escritor Q5 declarado (`app/docs/DOSSIES.md:46`); `desfazer_carimbo_sem_lastro` tem a inversão descrita acima (`app/chamados/services/relavra_via.py:217-246`). |

A lei de escopo literal consta em `app/docs/LEIS.md:62`; a exigência de selo que morde, em `app/docs/LEIS.md:97`. A comparação histórica dos diffs e execução concorrente permanecem `[nao verificado]`.

## 07/11 10:11 · sessao 01179be24412 · - · - · origem=Ronald (tmux)

D/E/F — leitura estática, sem banco nem execução da suíte `[nao verificado]`; comparação dos ~30 módulos com versões anteriores indisponível porque a cópia não tem `.git` `[nao verificado]`. [nao verificado]

1. **Bloqueia — recusa tardia perde a pergunta datada; selos não mordem.** `_colisao_ao_datar` retrata outras perguntas antes da validação final da resolvedora: `app/chamados/services/fio.py:849`, `:856`, `:859`, `:893`. Contraexemplo: `p` antiga sem dia é canônica; `q` viva já ocupa o fato; destino oferecido é de outro dia. `q` vira duplicata, a resolvedora é recusada e `p` continua sem dia; o retorno normal não desfaz a retratação. Os testes exercitam sucesso e recusa anterior à retratação, sem recusa posterior: `app/chamados/tests/test_e1_v5_portas_backfill.py:106`, `:119`.

2. **Fila — selo de trava confunde tabela já vista com linha já travada.** O coletor descarta qualquer segunda trava da mesma tabela: `app/chamados/tests/test_e1_v5_ordem_trava.py:38`, `:45`. Contraexemplo reproduzido usando o próprio helper, extraído por AST, sem banco: `Colaborador(pk11) → Chamado(pk22) → Colaborador(pk33)` produz `violacoes=[]`. Logo, o mutante que pega outro colaborador depois do chamado escapa; os casos usam um único colaborador: `:73`, `:151`.

3. **E — justificativa correta por entrada, limitada por chamada.** Cada entrada nos três helpers acrescenta um `SELECT FOR UPDATE`: `app/chamados/models.py:285`, `:686`, `:2456`; `app/chamados/services/fio.py:768`. Não há laço nesses trechos. Entretanto, uma transição pode entrar duas vezes (`cancelar`: `models.py:758`, `:764`), e costura ocorre por pergunta existente (`models.py:2611`), dentro da emissão por motivo (`app/chamados/services/disputa_emissao.py:312`, `:426`). Os seis números publicados nos tetos não foram re-medidos `[nao verificado]`.

4. **D — restaurar a órfã viva preserva a prova local.** A fixture reutiliza a pergunta emitida, remove os carimbos e fixa a célula acusando: `app/chamados/tests/test_orfas_nao_morre_na_guarda_da_porta.py:86`, `:92`, `:107`. As asserções exigem a muda encerrada, a disputa barrada com identificação da respondida e a respondida ainda sem materialização: `:116`, `:118`, `:120`, `:124`, `:129`. Essa montagem impede o caso de passar apenas porque a pergunta já estava morta. A guarda direta também continua cobrada: `:164`.

5. **F — mordidas reais presentes.** Corrida MARCO exige uma linha e um `IntegrityError`: `app/chamados/tests/test_e1_v5_indice.py:62`; episódios distintos e instâncias distintas são aceitos: `:84`, `:121`. Família ausente é cobrada tanto no catálogo de motivos quanto no catálogo de perguntas: `app/chamados/tests/test_e1_v5_familia.py:40`, `:71`. Backfill exige preservação das linhas, idempotência e reinstalação do índice: `app/chamados/tests/test_e1_v5_portas_backfill.py:189`; competência exportada exige saída 3 e nenhuma retratação: `:247`, `:267`.

As leis pertinentes declaram origem única, testemunha pela autoridade, medição real e escritor único: `app/docs/LEIS.md:59`, `:60`, `:61`, `:65`; o dossiê exige censo por campo e portas com prova: `app/docs/DOSSIES.md:46`, `:57`, `:61`.

## 07/11 09:14 · sessao 7c456cbb062c · FIO-E0-V4-E2 · verificador · origem=Code

1. **Bloqueia — apelido do módulo `time` escapa.** `_do_time` exige literalmente `f.value.id == 'time'`; `import time as _clock` não passa pelo tratamento de `ImportFrom`. (app/chamados/tests/test_e2_tarefa_do_dia.py:768; app/chamados/tests/test_e2_tarefa_do_dia.py:783)

   Mutante inserido **em memória** no início de `tarefa_do_dia` e, separadamente, de `esta_stale`:

   ```python
   import time as _clock
   import datetime
   agora = datetime.datetime.fromtimestamp(
       _clock.time(), datetime.timezone.utc)
   ```

   Nos dois módulos completos, o detector retornou `[]`, embora o mutante substitua o instante recebido pelo relógio corrente. Isso viola o contrato de relógio pela borda. (app/chamados/tests/test_e2_tarefa_do_dia.py:778; app/chamados/services/tarefa_do_dia.py:345; app/chamados/veredito_lavrado.py:187; app/core/juizes.py:152)

2. **Bloqueia — `None` indireto também escapa.** `_sem_instante` reconhece ausência de argumento e `ast.Constant(None)`; uma variável contendo `None` é aceita como instante. (app/chamados/tests/test_e2_tarefa_do_dia.py:762)

   Mutante executado em memória no módulo real: atribuir `instante = None` em `esta_stale` e substituir `timezone.localdate(agora)` por `timezone.localdate(instante)`. O detector retornou `[]`; nesse ramo, a conversão recebe `None` e lê o relógio, comportamento explicitamente reconhecido pelo próprio selo. (app/chamados/veredito_lavrado.py:198; app/chamados/tests/test_e2_tarefa_do_dia.py:741; app/chamados/tests/test_e2_tarefa_do_dia.py:766)

**Falsos positivos atuais: nenhum na execução pedida.** Extraí o detector por AST e executei sobre os arquivos completos: tarefa → `[]`; veredito excluindo `lavrar` → `[]`; veredito sem exclusão → `[('lavrar', 245)]`. O autoteste `test_MORDE_mutantes` também passou nessa execução isolada. (app/chamados/tests/test_e2_tarefa_do_dia.py:755; app/chamados/tests/test_e2_tarefa_do_dia.py:803; app/chamados/tests/test_e2_tarefa_do_dia.py:811; app/chamados/veredito_lavrado.py:245)

**Triagem:** as duas grafias anteriores — `now(tz)` e importação de `now` com apelido — estão cobertas, mas encerrar a cobertura do selo deixa passar os bloqueios 1 e 2. (app/chamados/tests/test_e2_tarefa_do_dia.py:778; app/chamados/tests/test_e2_tarefa_do_dia.py:785)

Não identifiquei fundamento adicional para promover os demais itens a bloqueio: a ausência de validação de `agora` naive e os dois relógios continuam presentes; a permanência dos leitores antigos está declarada no escopo da E2. (app/chamados/services/tarefa_do_dia.py:349; app/chamados/tests/test_selo_concordancia_varredura_stale.py:56; app/chamados/tests/test_selo_concordancia_varredura_stale.py:58; app/core/juizes.py:154) Ocorrência em produção e execução integrada Django: [nao verificado].

## 07/11 09:09 · sessao 9c3f52998fa8 · FIO-E0-V4-E2 · verificador · origem=Code

1. **Bloqueia — #3 cura os escapes anteriores, mas o selo ainda aceita leitura do relógio.** `_sem_instante` considera qualquer primeiro argumento diferente de `None` como instante; isso confunde **fuso** com **instante** (`app/chamados/tests/test_e2_tarefa_do_dia.py:756`; `app/chamados/tests/test_e2_tarefa_do_dia.py:759`).

   **Caso executado em memória contra o detector real:**
   ```python
   def f():
       return datetime.datetime.now(datetime.timezone.utc)
   ```
   O detector devolve `[]`, embora a expressão leia o relógio. Também passa `from django.utils.timezone import now as clock; clock()`: o import usa `ast.alias`, enquanto a busca cobre `Name`/`Attribute`, e `clock` não pertence à lista reconhecida (`app/chamados/tests/test_e2_tarefa_do_dia.py:745`; `app/chamados/tests/test_e2_tarefa_do_dia.py:754`; `app/chamados/tests/test_e2_tarefa_do_dia.py:767`; `app/chamados/tests/test_e2_tarefa_do_dia.py:770`).

   Portanto, **não considero #3 encerrado**. Os mutantes novos mordem suas grafias declaradas, mas não sustentam a garantia do selo (`app/chamados/tests/test_e2_tarefa_do_dia.py:739`; `app/chamados/tests/test_e2_tarefa_do_dia.py:795`).

2. **Fila — #1 e #2 estão curados nos dois predicados; igualdade e `None` não escapam.** Para os mesmos objetos, marcos e instante aware, as comparações coincidem (`app/chamados/veredito_lavrado.py:196`; `app/chamados/veredito_lavrado.py:198`; `app/chamados/veredito_lavrado.py:202`; `app/chamados/veredito_lavrado.py:206`; `app/chamados/services/raiox.py:40`; `app/chamados/services/raiox.py:42`; `app/chamados/services/raiox.py:45`).

   | Caso, sem outro motivo de invalidação | Resultado dos dois |
   |---|---|
   | `julgado_em == agora` | Fresca: a existência é inclusiva. |
   | `julgado_em > agora`, expiração `None` | Stale: o futuro é rejeitado antes da expiração. |
   | Expiração em D+1; julgamento D 22:30 local, armazenado em UTC; consulta D+1 09h | Stale: usa a data local do julgamento. |
   | Julgamento no próprio dia da expiração | Não cai novamente pelo relógio: a comparação é `<`. |
   | Marco igual ao julgamento | Não invalida: exige `>`. |

   Esses resultados foram reproduzidos com os corpos extraídos em memória, substituindo consultas e conversão de fuso; execução integrada Django: **[nao verificado]** (`app/chamados/veredito_lavrado.py:196`; `app/chamados/veredito_lavrado.py:202`; `app/chamados/services/raiox.py:40`).

3. **Fila — `agora` naive continua sem validação na borda.** `tarefa_do_dia` rejeita somente `None`; com julgamento aware e `agora` naive, a nova comparação levanta `TypeError`. Reproduzi esse caso no corpo isolado; não há retorno conservador de stale (`app/chamados/services/tarefa_do_dia.py:349`; `app/chamados/veredito_lavrado.py:196`).

   A cópia configura `USE_TZ=True`; chamador real passando naive: **[nao verificado]**. Isso é uma lacuna de contrato/validação, sem evidência para promovê-la a regressão de produção (`app/config/settings/base.py:95`).

4. **Fila — o teste de concordância ainda compara instantes diferentes.** O delta alinha os marcos e o juízo do segundo caminho, mas `_confere` executa a varredura antes de capturar seu próprio `agora`; a varredura captura outro relógio internamente (`app/chamados/tests/test_selo_concordancia_varredura_stale.py:56`; `app/chamados/tests/test_selo_concordancia_varredura_stale.py:58`; `app/chamados/services/raiox.py:35`).

   **Caso:** julgamento em `t`, expiração `None`, sem marcos; relógio da varredura em `t−1µs`, relógio do juízo em `t+1µs`. A primeira responde stale; o segundo, fresca. Não demonstra divergência da lei: demonstra que o selo não fixa a mesma entrada temporal (`app/chamados/services/raiox.py:40`; `app/chamados/veredito_lavrado.py:196`).

5. **Fila — os testes funcionais novos mordem os mutantes pretendidos, com lacunas específicas.** [nao verificado]

   - **Noite:** trocar somente `localdate(p.julgado_em)` por `.date()` muda o caso isolado de stale para fresca, contrariando a expectativa do teste. Ele cobre o caminho da tarefa; não exercita a varredura (`app/chamados/tests/test_e2_tarefa_do_dia.py:650`; `app/chamados/tests/test_e2_tarefa_do_dia.py:652`; `app/chamados/services/tarefa_do_dia.py:298`; `app/chamados/veredito_lavrado.py:202`).
   - **Futuro na tarefa:** retirar `> agora` aceita a lavra das 22h às 21h; o contracaso das 23h impede invalidar indiscriminadamente toda lavra (`app/chamados/tests/test_e2_tarefa_do_dia.py:658`; `app/chamados/tests/test_e2_tarefa_do_dia.py:660`; `app/chamados/tests/test_e2_tarefa_do_dia.py:662`).
   - **Futuro na concordância:** retirar a guarda de uma implementação quebra a igualdade; retirar das duas deixa ambas vazias, mas quebra a exigência de exatamente um stale (`app/chamados/tests/test_selo_concordancia_varredura_stale.py:61`; `app/chamados/tests/test_selo_concordancia_varredura_stale.py:97`; `app/chamados/tests/test_selo_concordancia_varredura_stale.py:99`).

   Falta caso específico de igualdade com `agora` e de julgamento noturno na varredura; os casos novos usam horários estritamente anteriores/posteriores (`app/chamados/tests/test_e2_tarefa_do_dia.py:659`; `app/chamados/tests/test_e2_tarefa_do_dia.py:661`; `app/chamados/tests/test_selo_concordancia_varredura_stale.py:97`).

6. **Fila — escritores e leitores dos dois campos: concordância decisória, dívida de significado preservada.**

   | Papel | Sítio |
   |---|---|
   | Produz valores novos | `lavrar`: instante corrente e expiração calculada pela data local desse instante (`app/chamados/veredito_lavrado.py:245`; `app/chamados/veredito_lavrado.py:246`). |
   | Exceção de backfill | Captura dia/instante no início e atribui os campos às linhas selecionadas (`app/chamados/management/commands/backfill_veredito_lavrado.py:94`; `app/chamados/management/commands/backfill_veredito_lavrado.py:139`; `app/chamados/management/commands/backfill_veredito_lavrado.py:140`). |
   | Persiste/restaura | Porta de lote e restauração da foto (`app/chamados/veredito_lavrado.py:224`; `app/chamados/veredito_lavrado.py:261`; `app/chamados/veredito_lavrado.py:262`). |
   | Decide vigência | `esta_stale` e varredura, com as mesmas desigualdades (`app/chamados/veredito_lavrado.py:196`; `app/chamados/services/raiox.py:40`). |
   | Transporta/compara | Foto da lavra, comparação do backfill e foto de reversão (`app/chamados/veredito_lavrado.py:254`; `app/chamados/management/commands/backfill_veredito_lavrado.py:121`; `app/chamados/services/passivo_validado.py:83`). |

   Os saves de abertura/carimbo, troca de via e costura persistem o resultado de `lavrar`; não calculam outra expiração (`app/chamados/models.py:2572`; `app/chamados/models.py:2709`; `app/chamados/services/relavra_via.py:124`; `app/chamados/services/fio.py:850`; `app/chamados/services/fio.py:903`).

   **Caso da dívida #9:** evento remoto recebe `hoje`, porque o cálculo começa em `max(hoje, data_evento)`. Isso contradiz “determinístico por data_evento”, mas não cria laço: após lavrar, data local do julgamento iguala a expiração e não satisfaz `<`. Também torna incorreto o comentário “data futura de novo” (`app/chamados/models.py:2402`; `app/chamados/veredito_lavrado.py:115`; `app/chamados/veredito_lavrado.py:120`; `app/chamados/veredito_lavrado.py:200`; `app/chamados/veredito_lavrado.py:202`).

7. **Fila — futuro stale não cria relavra em laço pelo mesmo motivo; leitores antigos continuam diferentes da tarefa.** O cron seleciona os stale uma vez e chama `lavrar` uma vez por alvo. A lavratura substitui o timestamp futuro pelo instante corrente; a rodada seguinte captura novo `agora` (`app/chamados/management/commands/lavrar_perguntas_stale.py:58`; `app/chamados/management/commands/lavrar_perguntas_stale.py:85`; `app/chamados/management/commands/lavrar_perguntas_stale.py:87`; `app/chamados/veredito_lavrado.py:245`; `app/chamados/services/raiox.py:35`).

   **Caso com relógio normal:** borda da tarefa em `t0`, lavratura concorrente em `t1>t0`, SELECT depois. A tarefa passa a responder `veredito_sem_lavra_vigente`, sem relavrar; ela não conserva a lavra anterior para reconstruir aquele instante (`app/chamados/services/tarefa_do_dia.py:280`; `app/chamados/services/tarefa_do_dia.py:298`; `app/chamados/services/tarefa_do_dia.py:137`).

   Raiox/fio antigos leem diretamente o veredito preenchido, sem validar os dois campos. Portanto uma lavra futura `sim` ainda pode esconder pergunta materializada nesses caminhos até a manutenção; a auditoria apenas pula o stale (`app/chamados/services/raiox.py:134`; `app/chamados/services/raiox.py:183`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1165`). Ocorrência e volume em produção: **[nao verificado]**.

8. **Fila — não encontrei fundamento para promover #9 ou #13 a bloqueio de produção; #3 permanece bloqueador.** #13 continua incompleto: o juiz lê `date.today()`, enquanto a expiração recebe a data local do carimbo. O efeito depende da execução e do fuso efetivo, que o dado fornecido não demonstra (`app/chamados/juizes.py:563`; `app/chamados/juizes.py:661`; `app/chamados/veredito_lavrado.py:246`). Fuso efetivo, execução da suíte Django e impacto real em produção: **[nao verificado]**.

## 07/11 09:08 · sessao 23fa3c758f64 · - · - · origem=Ronald (tmux)

1. **Bloqueia — #3 permanece incompleto: argumento de fuso é confundido com instante.** `_sem_instante` só marca chamada vazia ou primeiro argumento/keyword `value` literalmente `None`; a função chamada também é excluída da busca por nomes (`app/chamados/tests/test_e2_tarefa_do_dia.py:756`; `:759`; `:760`; `:765`; `:770`).

   **Caso executado contra o helper real, extraído por AST em memória:**
   ```python
   def f():
       return datetime.datetime.now(datetime.timezone.utc)
   ```
   Resultado do detector: `[]`. Executei também a expressão: retorna instante aware, lendo efetivamente o relógio. O argumento é fuso, não instante. Portanto o selo continua aceitando leitura banal do relógio (`app/chamados/tests/test_e2_tarefa_do_dia.py:759`; `:767`).

   Os casos `timezone.localdate(timezone=datetime.timezone.utc)` e `timezone.localtime(timezone=datetime.timezone.utc)` também retornam `[]` no detector, pela mesma omissão do `value`; execução real dessas APIs Django aqui é **[nao verificado]**, pois Django não está instalado (`app/requirements.txt:13`; detector em `app/chamados/tests/test_e2_tarefa_do_dia.py:759`).

2. **Fila — os três escapes da volta 1 foram efetivamente curados, e seus mutantes mordem.** Executei em memória o `test_MORDE_mutantes` extraído por AST, usando `unittest.TestCase`; todas as assertions passaram, incluindo `localdate(None)`, `localtime(value=None)`, alias por atribuição, helper `_hoje` e chamada no módulo. Isso confirma as grafias concretamente listadas, sem corrigir o escape do item 1 (`app/chamados/tests/test_e2_tarefa_do_dia.py:795`; `:799`; `:800`; `:801`; `:802`; `:803`; `:805`).

   **Outro escape executado:** `from django.utils.timezone import now as clock; def f(): return clock()` produz `[]`: o import usa `ast.alias`, enquanto o detector só pesquisa `ast.Name`/`ast.Attribute`, e `clock` não consta de `_RELOGIO` (`app/chamados/tests/test_e2_tarefa_do_dia.py:745`; `:754`; `:770`).

3. **Fila — `test_julgado_a_noite_e_a_data_local_da_lavra` morde o mutante `.date()` no caminho da tarefa.** O teste grava julgamento local D 22:30, expiração D+1 e consulta D+1 09:00; exige ausência de lavra vigente. A carga aplica `esta_stale`, e validação sem entrada em `_lavra` produz exatamente essa razão (`app/chamados/tests/test_e2_tarefa_do_dia.py:650`; `:651`; `:652`; `app/chamados/services/tarefa_do_dia.py:298`; `:135`; `:137`).

   **Caso reproduzido em memória com a função real extraída por AST**, `julgado_em` convertido para UTC e um adaptador de `localdate` com `ZoneInfo('America/Sao_Paulo')`: atual retorna `True`; substituindo somente `timezone.localdate(p.julgado_em)` por `p.julgado_em.date()`, retorna `False`. A configuração da cópia usa São Paulo e `USE_TZ=True` (`app/chamados/veredito_lavrado.py:202`; `app/config/settings/base.py:93`; `:95`). O teste não exercita a varredura (`app/chamados/tests/test_e2_tarefa_do_dia.py:651`).

4. **Fila — os dois testes de futuro mordem a remoção da guarda `> agora`.** No teste da tarefa, expiração permanece `None` pela fixture, julgamento passa a D 22h, consulta D 21h exige ignorância e D 23h exige validade; isso impede atribuir a primeira queda à expiração e impede um mutante que invalide toda lavra (`app/chamados/tests/test_e2_tarefa_do_dia.py:623`; `:658`; `:659`; `:660`; `:661`; `:662`).

   **Caso reproduzido em memória com a função real:** às 21h, atual `True`, remoção apenas de `or p.julgado_em > agora` dá `False`; às 23h, atual `False` (`app/chamados/veredito_lavrado.py:196`).

   O teste de concordância usa julgamento uma hora futuro e expiração 30 dias futura; exige exatamente um stale. Remover a guarda de só uma implementação quebra a igualdade; remover das duas mantém igualdade vazia, mas quebra a cardinalidade exigida (`app/chamados/tests/test_selo_concordancia_varredura_stale.py:97`; `:98`; `:99`; `:61`; `app/chamados/services/raiox.py:40`; `app/chamados/veredito_lavrado.py:196`).

5. **Fila — igualdade com `agora` está correta no código, mas sem mordida específica nos testes novos.** Com expiração `None`, marcos `{}` e julgamento exatamente igual a `agora`, reproduzi `esta_stale=False`; alterando `>` para `>=`, reproduzi `True`. Os casos novos usam 22h contra 21h/23h e futuro de uma hora, sem igualdade (`app/chamados/veredito_lavrado.py:196`; `:198`; `:204`; `app/chamados/tests/test_e2_tarefa_do_dia.py:658`; `:659`; `:661`; `app/chamados/tests/test_selo_concordancia_varredura_stale.py:97`).

Execução efetiva da suíte Django e passagem desses testes no banco: **[nao verificado]**. [nao verificado]

_2709 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
