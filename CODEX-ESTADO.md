# CODEX-ESTADO

> Ultima resposta processada: **14/11/2026 19:27** · publicado em **14/11/2026 19:31** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 14/11 19:27 · sessao c4cb8db0cbc8 · BO-DISPUTA-SEM-PERGUNTA · leitura · origem=Code

Referências relativas a `app/`, salvo indicação contrária. **Cópia `arquivo:+N`** significa a linha do lado novo do `diff_obra.patch` fornecido. “Push sim” significa tentativa de envio; entrega real e execução dos testes citados: **[nao verificado]**. Os testes novos da cópia foram apenas listados no anexo; sua cobertura: **[nao verificado]**. [nao verificado]

Estados: **a** ata lavrada; **b** sem ata; **c** sem marcos/lâmpadas na ata; **d** pergunta respondida; **e** pergunta morta pelo C6. Respondida pode continuar viva, mas deixa de ser pendência do colaborador no app (`chamados/juizes.py:1417`; `chamados/services/pergunta_no_app.py:71`).

**Porta 1 — criar ou atualizar disputa**

`chamados/services/disputa_emissao.py::criar_ou_atualizar_disputa:644` → `_abrir_ou_agregar_disputa:474`. Chamadores: views de batida `api/views.py:764`, `api/views_core.py:758`; triagem `ponto/services/triagem_batida.py:735`; command `marcar_foto_ausente_retro.py:96`; fábrica da célula `chamados/services/fabrica_da_celula.py:172`. Esta última é chamada pelo cartório, reconciliador e backfill (`ponto/services/cartorio.py:1045`; `chamados/reconciliador.py:1387`; `chamados/management/commands/backfill_fio_morto_e4b.py:143`).

| ID — entrada | Juiz consultado | O que persiste no hash | Push / destinatário | Prova; teste existente | Na cópia | [nao verificado]
|---|---|---|---|---|---|
| N0 — motivos vazios; qualquer estado | Nenhum; guarda local | Nada | Não | `disputa_emissao.py:656` | Guarda preservada. |
| N1 — a; DS inexistente ou somente fechadas | Nenhum para nascer; procura apenas DS aberta | Cria/reusa chamado-pai; cria `DS.motivos`, `contexto`, `chamado` **antes** da geração | Sim, ao colaborador, mesmo se o gerador não entregar pergunta local | `disputa_emissao.py:531`, `:568`, `:587`, `:669` | Nascimento passa por ensaio; sem pergunta local admitida, desfaz o savepoint. Reuso estrangeiro pode ser repetido na dona (`disputa_emissao.py:+435`). |
| N2 — b/c; DS inexistente ou somente fechadas | Nascimento não consulta ata; geração usa contexto/grade conforme insumo disponível | DS/pai já persistem. Pode criar pergunta com data do contexto; pode terminar sem pergunta | Sim, ao colaborador | `disputa_emissao.py:345`, `:350`, `:360`, `:371`, `:425`, `:669` | Resolvedor comum e ensaio evitam parte dos nascimentos vazios. **“Pergunta local admitida” não significa C6 viva**: `locais()` exclui vias, sem consultar C6 (`disputa_emissao.py:+435`). |
| N3 — a/b/c; DS aberta | Nenhum para agregar; união e merge locais | Atualiza `DS.motivos/contexto`; contexto registra `qtd/ultima_em` | Ao colaborador somente com motivo novo; outro dia do mesmo motivo pode gerar pergunta sem push | `disputa_emissao.py:535`, `:548`, `:565`, `:682`, `:685`, `:686` | Agrega motivos locais; encaminha estrangeiros à dona. Contexto continua podendo ser salvo antes da geração (`disputa_emissao.py:+387`, `:+408`, `:+435`). | [nao verificado]
| N4 — d/e na própria DS | Dedup local por existência; nenhum C6 nesse predicado | Pode atualizar DS como N3, sem criar pergunta nova | Regra N1/N3; resposta ou morte não são guarda desse push | `disputa_emissao.py:325`, `:398`, `:417`, `:669`, `:686` | Dedup por existência permanece; efeito passa pela pendência do app (`disputa_emissao.py:+213`, `:+1828`; `pergunta_no_app.py:+84`). | [nao verificado]
| N5 — pergunta canônica pertence a outra DS aberta/fechada | Identidade do fato; juiz da resolvedora e guarda da costura | Pode reutilizar/atualizar pergunta estrangeira e deixar a DS nova sem filhos; `_criada` não governa o retorno do gerador | Pode enviar ao colaborador mesmo com DS local vazia | `disputa_emissao.py:444`, `:470`; `chamados/models.py:2977`, `:3035`, `:3064` | Distingue `ReusadaDeOutraDS`; desfaz nascimento sem pergunta local; agrega na dona aberta. Na fechada, não salva motivo: registra descarte após commit (`disputa_emissao.py:+435`; `descarte_disputa.py:+5`). |

**Porta 2 — gerar/reaproveitar perguntas e entradas pendentes**

`chamados/services/disputa_emissao.py::gerar_perguntas_disputa:300`; `::perguntar_entradas_pendentes:257`. Chamadores internos: wrapper e agrupador (`disputa_emissao.py:670`, `:682`, `:685`, `:1270`). Há geração adicional pela fábrica e pelo command smoke (`chamados/services/fabrica_da_celula.py:174`; `chamados/management/commands/smoke_chamados.py:103`).

| ID — entrada | Juiz consultado | O que persiste no hash | Push / destinatário | Prova; teste existente | Na cópia | [nao verificado]
|---|---|---|---|---|---|
| G1 — a/b/c; texto/data resolvíveis | GEOMETRIA DE TURNO para batida; dados da célula/contexto; destino derivado separadamente | `PerguntaDisputa.abrir` cria ou retorna canônica; grava dia/destino/célula/lavra conforme nascimento ou reuso | Nenhum dentro do gerador; wrapper decide | `disputa_emissao.py:345`, `:350`, `:371`, `:391`, `:411`, `:444` | `_resolver_pergunta_por_motivo` resolve dia antes de texto/dedup/destino (`disputa_emissao.py:+213`). |
| G2 — sem dia para pergunta de hora | Nenhum; ausência local de insumo | Nenhuma pergunta; não desfaz DS já criada pelo wrapper | Wrapper ainda pode enviar | `disputa_emissao.py:424`, `:425`, `:430` | Causa `InsumoAusente`; núcleo pode desfazer o nascimento (`disputa_emissao.py:+213`, `:+435`). |
| G3 — d/e local, mesma identidade, fora das vias excluídas | Nenhum; existência local | Não cria; dedup não lê resposta nem C6 | Nenhum próprio | `disputa_emissao.py:325`, `:398`, `:417` | Predicado permanece no resolvedor (`disputa_emissao.py:+213`). |
| G4 — canônica de outra DS, aberta ou fechada | Porta do fato `PerguntaDisputa.abrir` | Retorna a pergunta estrangeira; não transfere sua `disputa` | Nenhum próprio | `chamados/models.py:2833`, `:2977`, `:3064`; `disputa_emissao.py:470` | Retorno recebe causa própria e argumentos para replay; wrapper trata propriedade da pergunta (`disputa_emissao.py:+369`, `:+435`). |
| G5 — lista de entradas pendentes | GEOMETRIA DE TURNO / `dia_do_turno_da_batida`; FURO DO DIA / `classificar_falta` | Injeta contexto por dia, chama gerador; salva `DS.contexto` quando há retorno | Nenhum próprio | `disputa_emissao.py:282`, `:288`, `:291`, `:295`, `:296` | Restaura contexto em memória, agrega conforme dona local/estrangeira e coleta criadas (`disputa_emissao.py:+136`). |
| G6 — fábrica da célula | CÉLULA/PRECEDÊNCIA; motivos pela ata | Desfaz carimbo sem lastro; chama wrapper e depois chama gerador novamente se recebeu DS | Silenciado no julgamento da célula | `chamados/services/fabrica_da_celula.py:139`, `:169`, `:172`, `:174`, `:218` | Arquivo não alterado no diff; chamada adicional permanece. |

O contrato final do dia já existe no hash: `_sob_contrato` exige saída com dia do juiz e desfaz a chamada se o contrato falhar; isso não uniformiza as decisões anteriores do gerador (`chamados/models.py:2862`; `chamados/services/fio.py:1140`). Testes dessa porta: `chamados/tests/test_e1_v15e_contrato_de_saida.py`, referenciado em `chamados/models.py:2872`; teste de derivação pelo dia da pergunta em `chamados/tests/test_selo_derivacao_pela_data_da_pergunta.py:80`.

**Porta 3 — órfãos, `orfao_14h` e complementação**

`chamados/services/disputa_emissao.py::criar_disputa_a_partir_de_chamados_orfaos`; auxiliares `::_criar_pergunta_se_couber:823`, `::_complementar_marcos_faltantes:850`, `::_ata_do_dia:887`.

Chamadores: commands `processar_alertas_turno.py:385`, `processar_alertas_avancados.py:159`, `detectar_ausencias.py:803`, agendados respectivamente em `*/5`, `*/30`, `*/5` (`config/crons.py:362`, `:363`, `:364`); ação de view `chamados/services/acoes_disputa.py:222`; cobrar-dia `chamados/services/cobrar_dia.py:49`; passivo `chamados/services/pergunta_no_app.py:246`, acionado pelo command `perguntar_chamados_sem_pergunta.py:20`.

| ID — entrada | Juiz consultado | O que persiste no hash | Push / destinatário | Prova; teste existente | Na cópia | [nao verificado]
|---|---|---|---|---|---|
| O1 — elegível; DS inexistente/somente fechadas | Nenhum para nascer; busca DS aberta | Cria pai e DS antes das guardas; motivo inicial `orfao_14h` | Depende dos ramos seguintes | `disputa_emissao.py:1096`, `:1101`, `:1123` | Nascimento antecipado permanece. Rollback específico: nova + retorno estrangeiro + nenhuma pergunta local (`disputa_emissao.py:+1146`). |
| O2 — a, marco apagado, rota por ata; DS aberta/nova | DIA DO CHAMADO / `data_do_chamado`; leitura dos marcos/lâmpadas | Cria/reusa pergunta por marco; agrega `DS.motivos`; chamado vai a análise | Sim ao colaborador, se não fechar e `notificar` | `disputa_emissao.py:1166`, `:1170`, `:1175`, `:873`, `:837`, `:1289` | Encaminha estrangeira à dona; push exige pendentes da DS e DS ainda aberta (`disputa_emissao.py:+1146`, `:+1363`). | [nao verificado]
| O3 — b, com escala vigente; DS aberta/nova | `_marcos_faltantes`; guarda local de `sem_celula` e escala vigente | DS/pai ficam; nenhuma pergunta; abstenção impede auto-fecho | **Sim ao colaborador no hash**, se `notificar` | `disputa_emissao.py:902`, `:908`, `:911`, `:1279`, `:1289`; teste espera DS aberta com zero perguntas: `chamados/tests/test_hx_disputa_muda_sem_ata.py:51` | **DS aberta vazia permanece.** Novo filtro suprime push sem pendentes (`disputa_emissao.py:+1363`). |
| O4 — c: ata de trabalho sem pares de marco/lâmpada | Leitor devolve `sem_celula` | Mesmo resultado de O3, havendo escala vigente | Mesmo resultado de O3 | `chamados/services/declaracao_texto.py:121`, `:126`; `disputa_emissao.py:902` | Predicado preservado; efeito filtrado como O3. |
| O5 — b, sem escala vigente | FURO DO DIA; exceção local da guarda de ata | Permite pergunta por batidas; não exige ata impossível dessa escala | Sim se permanece aberta | `disputa_emissao.py:908`, `:910`, `:1200`, `:1205`, `:1213` | Exceção preservada no hunk do agrupador (`disputa_emissao.py:+1146`). |
| O6 — falha ao ler ata | Nenhum; `except` local | `_ata_do_dia` devolve `True, None`; não bloqueia pergunta | Pode enviar se permanece aberta | `disputa_emissao.py:897`, `:901` | Ramo não alterado. |
| O7 — a, sem falta acusada; DS aberta/nova | FURO DO DIA; depois DISPUTA FECHA / C7 | Pode ter criado DS/pai; fecha DS se C7 permite e não houve abstenção | Não depois do auto-fecho | `disputa_emissao.py:1201`, `:1238`, `:1278`, `:1282`, `:1284`; `chamados/tests/test_hx_disputa_muda_sem_ata.py:63` | Conserva auto-fecho e exceção por falta de ata (`disputa_emissao.py:+1146`). |
| O8 — d, pergunta ainda viva; DS aberta | C6 por `fio_vivo` | `_ja_perguntou` bloqueia nova; resposta permanece | Hash não exige que o colaborador ainda deva resposta | `disputa_emissao.py:1415`, `:1422`, `:832`, `:1289` | Push exige pendência do app, que exclui respondida (`disputa_emissao.py:+1363`; `pergunta_no_app.py:71`). | [nao verificado]
| O9 — e, colaborador ativo | C6 por `fio_vivo` | Permite tentar `abrir`; canônica pode ser reutilizada em vez de nascer outra | Depende de permanecer aberta no hash | `disputa_emissao.py:1415`, `:1422`, `:837`; `chamados/models.py:2977` | Trata retorno estrangeiro e filtra push (`disputa_emissao.py:+840`, `:+1363`). |
| O10 — e, colaborador não ativo | Nenhum; guarda local de existência | Linha existente bloqueia geração antes de consultar C6 | Depende das demais perguntas/fluxo | `disputa_emissao.py:1416`, `:1420` | Predicado não alterado. |
| O11 — intervalo explícito; a/b/c | Nenhum juiz de ata nesse ramo; lê `contexto.marcos_faltantes` | Pergunta por marco com data do contexto e destino `_chi`; não passa por `_ata_do_dia` | Sim se permanece aberta | `disputa_emissao.py:1138`, `:1155`, `:1289` | Fotografa fatos em `_marcos_de_intervalo`; destino passa pelo eleitor comum (`disputa_emissao.py:+1146`; `fio.py:+752`). |
| O12 — chamado já tem pergunta não materializada; dona aberta **ou fechada** | Proxy SQL `materializada_em IS NULL`; depois leitor da ata | Complementa e pode salvar `DS.motivos` da dona fechada | Pode retornar antes do push principal | `disputa_emissao.py:1070`, `:1073`, `:1081`, `:1084`, `:1093` | Agregador não salva dona fechada; registra descarte. Complementos só notificam dona aberta (`disputa_emissao.py:+1092`, `:+408`; `descarte_disputa.py:+5`). | [nao verificado]
| O13 — sem origem e sem data/base | DIA DO CONTEXTO; GEOMETRIA DE TURNO ou DIA DE TRABALHO | Pergunta não nasce; DS/pai já existem; C7 pode fechar | Não se auto-fechar | `disputa_emissao.py:1232`, `:1235`, `:816`, `:820`, `:1282` | Guardas preservadas no agrupador (`disputa_emissao.py:+1146`). |

**Porta 4 — âncora, eleição e escrita do destino**

`chamados/services/disputa_emissao.py::criar_disputa_ancora_de_chamado:1302`, chamada pela ação de view `chamados/services/acoes_disputa.py::nucleo_pedir_confirmacao_ancora:178`, em `:189`.

| ID — entrada/porta | Juiz consultado | O que persiste no hash | Push / destinatário | Prova | Na cópia | [nao verificado]
|---|---|---|---|---|---|
| A1 — chamado não é `ancora_limbo` ou está encerrado | Nenhum; predicado local | Nada | Não | `disputa_emissao.py:1316`, `:1318` | Guarda preservada. |
| A2 — d/e, pergunta de âncora não materializada já existe | Nenhum; proxy SQL, sem C6 | Nada nessa chamada | Não | `disputa_emissao.py:1323` | Guarda preservada. |
| A3 — a/b/c; sem pergunta bloqueadora; DS aberta/nova | Nenhuma consulta à ata/marcos; busca DS aberta | Cria/agrega DS; cria **conversa**, destino `chamado_limbo`; chamado vai a análise | Sim ao colaborador | `disputa_emissao.py:1332`, `:1358`, `:1367`, `:1371`, `:1377`, `:1381` | Agregação central; push filtrado após commit. Conversa continua recebendo limbo diretamente (`disputa_emissao.py:+1411`, `:+1828`). | [nao verificado]
| A4 — eleição na emissão datada | CHAMADO: dia/motivos atendidos; desempate local por turno | Escolhe chamado vivo; exclui candidato já com pergunta não materializada do mesmo motivo; pode devolver nenhum | Nenhum próprio | `disputa_emissao.py:156`, `:202`, `:227`, `:241` | `_derivar_chamado_destino` delega ao novo eleitor; intervalo explícito precede chamado genérico; empate usa menor PK (`disputa_emissao.py:+98`; `fio.py:+752`). |
| A5 — eleição em outros caminhos | Órfãos escolhem primeiro do grupo; declaração pede seu chamado; âncora pede limbo | Destino recebido pela pergunta pode depender da porta de entrada | Nenhum próprio | `disputa_emissao.py:1197`, `:1377`; `chamados/services/declaracao_texto.py:376` | Emissores datados passam pelo eleitor; outros caminhos abaixo continuam escolhendo destino. |
| A6 — reatar histórico / substituir chamado terminal | CHAMADO: resolvedora válida; C8 para sobrevivente | Reaponta pergunta ao histórico único ou sobrevivente escolhido fora do emissor | Não próprio | `chamados/reconciliador.py:1586`, `:1624`, `:1643`; `chamados/models.py:374`, `:390`, `:437` | Esses eleitores permanecem; guarda nova preserva intervalo já eleito, sem substituir todas as eleições (`fio.py:+808`). |
| A7 — costura existente | `resolvedora_valida` + `_costura_nao_mata_o_fio` | Atualiza `PerguntaDisputa.chamado_destino`, eventualmente dia, e relavra | Não próprio | `chamados/services/fio.py::reapontar_resolvedora:804`, `:906`, `:926` | Atribuição/persistência passam por `gravar_destino_da_pergunta`; guarda protege intervalo eleito (`fio.py:+785`, `:+886`). |

O pai da DS também é escolhido por três nascimentos com argumentos distintos de `ChamadoColaborador.abrir`: núcleo comum, agrupador manual e âncora (`disputa_emissao.py:568`, `:1104`, `:1339`). O diff não os transforma em um único nascimento (`disputa_emissao.py:+435`, `:+1146`, `:+1411`).

**Porta 5 — declaração de horário**

`chamados/services/declaracao_texto.py::declarar_hora_colab:307`, `::aplicar_declaracao:631` → `disputa_emissao.py::disputa_para_declaracao:610`. São caminhos do colaborador e da aplicação administrativa da proposta, respectivamente (`declaracao_texto.py:313`, `:632`).

| ID — entrada | Juiz consultado | O que persiste no hash | Push / destinatário | Prova; teste existente | Na cópia | [nao verificado]
|---|---|---|---|---|---|
| D1 — sem dia; b/c; ou hora não casa com marco | DIA DO CHAMADO; leitor da ata; casamento hora→marco | Recusa antes de criar DS/pergunta | Não | `declaracao_texto.py:330`, `:334`, `:341` | Guardas anteriores preservadas. |
| D2 — a, marco faltante; DS aberta/nova | Núcleo da DS; canônica do fato; depois resposta/validação | Cria/agrega DS; cria/reusa pergunta com dia da célula e destino solicitado; grava resposta; admin valida | Não pela declaração | `declaracao_texto.py:360`, `:363`, `:376`, `:659`, `:666`, `:696`; ausência de push coberta em `chamados/tests/test_porta_disputa_emissao.py:223` | DS/pergunta ficam no mesmo ensaio; retorno estrangeiro desfaz ensaio local e repete na dona (`disputa_emissao.py:+614`). |
| D3 — d, mesma resposta; ou e por validação sem efeito no marco | Comparação local / `_resposta_validada` | Pode ter agregado/criado DS antes de retornar `ja_declarada` ou `validada_sem_registro` | Não | `declaracao_texto.py:378`, `:412`, `:417` | Recusas permanecem depois do novo ato conjunto (`declaracao_texto.py:+357`; `disputa_emissao.py:+614`). |
| D4 — canônica de outra DS aberta/fechada | Identidade do fato | Reuso conserva dona original; DS local pode ter sido criada anteriormente | Não | `chamados/models.py:2833`; `declaracao_texto.py:360`, `:376` | Replay na dona; fechada recebe descarte de motivo, sem reabertura pelo agregador (`disputa_emissao.py:+614`, `:+408`). |

**Porta 6 — demais atualizações de DisputaSupervisao**

O censo atribui os escritores a `chamados/models.py` e `chamados/services/disputa_emissao.py`; os atos são declarados nos selos (`chamados/tests/test_porta_disputa_ciclo.py:81`, `:86`; `chamados/tests/test_porta_disputa_emissao.py:84`). As portas abaixo não distinguem a/b/c salvo pelas perguntas/guardas indicadas.

| ID — porta / entrada | Juiz ou predicado | Persistência | Chamadores; push | Na cópia | [nao verificado]
|---|---|---|---|---|
| U1 — `DisputaSupervisao.fechar:1911`; aberta, inclusive d/e | C7: nenhuma viva e nenhuma resposta sem veredito; exceções nomeadas não dispensam resposta sem veredito | `fechada_em/por`, `resolucao`, `motivo_fechamento`; exceções podem encerrar perguntas mudas | Escrita `models.py:1992`; reconciliador `:413`, `:613`, `:1172`; materialização `materializacao.py:592`, `:1339`; tranca `competencia_trancada.py:229`; desligamento `colaboradores/services/desligamento.py:167`. Signal tenta resolver pai, sem push próprio (`chamados/signals.py:20`, `:43`). | C7 e signal não alterados no diff. |
| U2 — `aparar_motivo_respondido:2008`; motivo presente | Nenhum; fato respondido por outro caminho | Remove motivo e contexto; não fecha sozinha (`models.py:2024`) | Upload de foto (`api/foto_recebida.py:72`); sem push nessa porta | Sem mudança. |
| U3 — `remover_motivo:2027`; aberta e motivo presente | Predicados locais; C7 no eventual fechamento | Retrata perguntas; atualiza motivos/contexto; tenta fechar se não sobrou motivo (`models.py:2121`, `:2128`) | Signals de posto e âncora (`colaboradores/signals.py:77`; `escala/signals.py:174`); sem push próprio | Sem mudança. |
| U4 — `anotar_contexto`; a/b/c/d/e | Nenhum; primeira presença da chave | Atualiza contexto uma vez (`models.py:2157`, `:2161`) | View `chamados/views.py:1343`; command escalonamento `escalonar_chamados_supervisao.py:159`; efeitos ficam nos chamadores | Sem mudança. |
| U5 — `renovar_contexto:2164`; a/b/c/d/e | Nenhum; valor diferente | Atualiza contexto (`models.py:2175`, `:2179`) | Cron diário (`alertar_disputas_pendentes.py:48`), detalhado abaixo | Cópia desloca chamada do cron para callback condicionado a pendência (`alertar_disputas_pendentes.py:+41`). |
| U6 — `reabrir_admin:2182`; fechada | Nenhum; exige usuário e teto de duas | Limpa fechamento/resolução; incrementa contador; motivo opcional (`models.py:2194`, `:2217`) | Ação de view `acoes_disputa.py:259`, delegada pelo fio `fio.py:583`; depois cria conversa em `acoes_disputa.py:263`; push supervisão/colaborador em `:277`, `:288` | Só aviso ao cola

[truncado em 20000 de 34735 caracteres]

## 14/11 19:23 · sessao c50cfa1332dd · - · - · origem=Ronald (tmux)

Censo de escrita DS executado **sem Django, banco ou logs**, com `python3 -B`, usando o varredor puro existente; ele classifica construções, saves, gerentes e inferências, sem garantir resolução de todo alias dinâmico (`app/core/censo_escritas.py:1`, `app/core/censo_escritas.py:451`, `app/core/censo_escritas.py:469`).

**19 sítios atribuídos de escrita DS no hash**, todos nestes dois módulos (`app/core/portas.py:443`):

| Porta | Persistência | Chamadores identificados |
|---|---|---|
| `DisputaSupervisao.fechar` | `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento`; save `app/chamados/models.py:1992` | Reconciliador `app/chamados/reconciliador.py:413`, `:613`, `:1172`; materialização `app/chamados/services/materializacao.py:592`, `:1339`; tranca `app/chamados/services/competencia_trancada.py:229`; becos `app/chamados/services/becos.py:57`; upload foto `app/api/foto_recebida.py:80`; entrada sincronizada `app/ponto/services/entrada_sincronizada.py:60`; emissão `app/chamados/services/disputa_emissao.py:1284`, `:2092`; desligamento `app/colaboradores/services/desligamento.py:167`; comandos `app/chamados/management/commands/encerrar_perguntas_com_fato.py:88`. |
| `aparar_motivo_respondido` | `motivos`, `contexto`; save `app/chamados/models.py:2024` | Upload de foto, `app/api/foto_recebida.py:72`. |
| `remover_motivo` | retrata perguntas; `motivos`, `contexto`; save `app/chamados/models.py:2121`; chama `fechar` se nenhum motivo, `:2128` | Signals de posto `app/colaboradores/signals.py:77` e âncora `app/escala/signals.py:174`. |
| `anotar_contexto` | primeira anotação apenas; save `app/chamados/models.py:2161` | View `app/chamados/views.py:1343`; comando escalonamento `app/ponto/management/commands/escalonar_chamados_supervisao.py:159`. |
| `renovar_contexto` | se valor mudou; save `app/chamados/models.py:2179` | Alerta diário `app/ponto/management/commands/alertar_disputas_pendentes.py:48`. |
| `reabrir_admin` | limpa fechamento/resolução, incrementa teto administrativo, agrega motivo opcional; save `app/chamados/models.py:2217` | Ação de view `app/chamados/services/acoes_disputa.py:259`, delegada pelo fio `app/chamados/services/fio.py:583`. |
| `reabrir_sistema` | limpa fechamento/resolução; save `app/chamados/models.py:2233` | Ressurreição no chamado `app/chamados/models.py:1622`; relavra `app/chamados/services/relavra_via.py:261`; validação recusada `app/chamados/services/validacao.py:241`; materialização de ausência desfeita `app/chamados/services/materializacao.py:692`; comando `app/chamados/management/commands/reabrir_orfas_enterradas.py:165`. |
| `reabrir_por_colab` | questionário volta a pendente e incrementa `reaberturas_colab`; save `app/chamados/models.py:2296` | Views HTML `app/chamados/views.py:1334` e API `app/api/views.py:2679`. |
| `limpar_resolucao_residual` | apenas `resolucao` de aberta; save `app/chamados/models.py:2322` | Nenhum chamador produtivo encontrado; contrato declara a porta (`app/core/portas.py:405`); alcance de invocação externa [nao verificado]. |
| `perguntar_entradas_pendentes` | `contexto`; save `app/chamados/services/disputa_emissao.py:296` | Wrapper `app/chamados/services/disputa_emissao.py:685`; órfãos `:1270`. |
| `_abrir_ou_agregar_disputa` | atualiza motivos/contexto `app/chamados/services/disputa_emissao.py:565`; cria DS `:587` | Wrapper `:662`; declaração `:628`; comando smoke `app/chamados/management/commands/smoke_chamados.py:100`; helper sem chamador `_disputa` `app/ponto/management/commands/reconciliar_grade.py:424`. |
| Agrupador de órfãos | saves/criação `app/chamados/services/disputa_emissao.py:1084`, `:1123`, `:1132`, `:1161`, `:1256` | Fluxo auditado pelo outro explorer; cinco sítios também declarados em `app/chamados/tests/test_porta_disputa_emissao.py:86`. |
| Âncora | cria DS `app/chamados/services/disputa_emissao.py:1358`; atualiza motivos `:1367` | `nucleo_pedir_confirmacao_ancora`, `app/chamados/services/acoes_disputa.py:189`; cascas de detalhe e fio declaradas em `:179`. |

O comentário antigo que diz desligamento por `.update()` está ultrapassado: o código atual percorre as abertas e chama `fechar`, retendo resposta sem veredito (`app/colaboradores/services/desligamento.py:157`, `:164`, `:167`). Smoke e `_disputa` são **chamadores do núcleo**, não criações DS adicionais; smoke chama também o gerador, e `_disputa` é removido pelo diff (`app/chamados/management/commands/smoke_chamados.py:100`, `:103`; `app/ponto/management/commands/reconciliar_grade.py:424`; `diff_obra.patch`, hunk `@@ -409,23 +409,7 @@`).

**Portas adicionais de pergunta/âncora**

| Estado/porta | Juiz/predicado | Persistência e push | Cópia | [nao verificado]
|---|---|---|---|
| Declaração do colaborador sem dia | `dia_do_fato`; sem dia recusa | Nada, retorna `sem_data` (`app/chamados/services/declaracao_texto.py:330`). | Preserva guarda; altera operação posterior (`diff_obra.patch`, declaração, hunk `@@ -357,24 +357,13 @@`). |
| Sem ata | `_marcos_faltantes`; `sem_celula` | Recusa antes DS/pergunta; acende falta de ata (`app/chamados/services/declaracao_texto.py:334`). | Guarda anterior preservada pelo diff. |
| Com ata, sem marco que case | `_casar_hora` | Nada; `nao_casa` (`app/chamados/services/declaracao_texto.py:341`). | Guarda anterior preservada pelo diff. |
| Com marco faltante; DS aberta / inexistente | Núcleo DS; depois canônica do universo do fato, `fio.canonica_do_grupo` | DS criada/agregada; pergunta reusada localmente ou `abrir` devolve canônica até de outra DS; destino pedido é `ch`, dia é célula (`app/chamados/services/declaracao_texto.py:360`, `:363`, `:376`; `app/chamados/models.py:2833`). Declaração não manda push (`app/chamados/tests/test_porta_disputa_emissao.py:223`). | DS e pergunta entram no mesmo ensaio; retorno estrangeiro desfaz ensaio e repete na dona; destino passa `_destino_do_intervalo` (`diff_obra.patch`, `_pergunta_para_declaracao`, novas linhas a partir de `:614`). |
| Pergunta validada embora marco siga faltante | `_resposta_validada` | Retorna `validada_sem_registro`; **DS já foi criada/agregada antes** (`app/chamados/services/declaracao_texto.py:378`, `:412`). | Mantém a recusa, reorganiza nascimento (`diff_obra.patch`, declaração, hunk `@@ -357,24 +357,13 @@`). |
| Pergunta já respondida igual à hora | comparação local | `ja_declarada`; sem nova resposta (`app/chamados/services/declaracao_texto.py:417`). | Comparação posterior preservada pelo diff. |
| Declaração administrativa | proposta válida, mesma canônica; depois `validar_pergunta` | DS/pergunta como acima; responde se necessário e valida; sem proposta não escreve (`app/chamados/services/declaracao_texto.py:649`, `:659`, `:666`, `:687`, `:696`). | Passa argumentos da pergunta ao ato DS; replay em dona estrangeira (`diff_obra.patch`, hunk `@@ -656,15 +645,13 @@`). |
| Âncora: categoria diferente/encerrada | nenhum: predicados locais | Nada (`app/chamados/services/disputa_emissao.py:1316`). | Sem alteração desses predicados no diff. |
| Âncora: pergunta não materializada existente, respondida ou C6 morta | nenhum: `materializada_em IS NULL` | Nada, **não consulta C6** (`app/chamados/services/disputa_emissao.py:1323`). | Guarda preservada no diff. |
| Âncora sem tal pergunta; DS aberta / inexistente ou só fechada | nenhuma consulta à ata/marcos; busca DS aberta | Agrega ou cria DS; cria conversa `ancora_ausente`, destino literal `chamado_limbo`; chamado vai a análise; solicita push ao colab (`app/chamados/services/disputa_emissao.py:1332`, `:1358`, `:1371`, `:1377`, `:1379`, `:1381`). | Agregação passa `_agregar_motivos_na_dona`; push herda filtro de pendência pós-commit; eleição datada não entra porque conversa (`diff_obra.patch`, hunk `@@ -1332,7 +1411,7 @@`; `@@ -1755,38 +1828,36 @@`). |
| Reabertura administrativa de DS fechada | predicados locais: staff/grupo, motivo válido, justificativa, teto dois | Reabre DS primeiro; depois cria conversa nova, sem destino explícito; reabre chamado se encerrado; push supervisão e colab (`app/chamados/services/acoes_disputa.py:245`, `:255`, `:259`, `:263`, `:269`, `:276`, `:288`). | Filtro de pendência pós-commit no push ao colab; ordem DS→pergunta permanece (`diff_obra.patch`, hunk `@@ -285,18 +293,18 @@`). |
| Reabertura de questionário | `perguntas_corrigiveis` | Limpa texto/resposta/validação via `reabrir_para_correcao`; guarda foto na sequência; não gera pergunta nova (`app/chamados/services/disputa_emissao.py:1582`, `:1593`, `:1615`, `:1632`). View própria e ação fio mandam push (`app/chamados/views.py:1435`, `:1444`; `app/chamados/services/acoes_disputa.py:102`, `:109`). | Push ao colab filtrado depois do commit (`diff_obra.patch`, ações hunk `@@ -90,6 +90,7 @@`; views hunk `@@ -1444,12 +1444,13 @@`). |
| Fábrica de célula | motivos emitidos pela ata; remove carimbo sem lastro | Chama wrapper e **chama gerador novamente se DS retornou** (`app/chamados/services/fabrica_da_celula.py:160`, `:169`, `:172`, `:174`). Julgamento silencia push (`:218`). | Arquivo fora do diff; chamada duplicada preservada. |

Testes de existência/contrato, sem executá-los: declaração e ausência de push `app/chamados/tests/test_porta_disputa_emissao.py:185`, `:199`, `:211`, `:223`; reabertura colab e teto `app/chamados/tests/test_porta_disputa_ciclo.py:283`, `:297`, `:311`, `:322`; porta de dia `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:165`; cobertura dos novos testes listados no anexo [nao verificado].

**I3: NÃO FECHA globalmente, hash e cópia.**

No hash, eleição da emissão é `_derivar_chamado_destino`; órfãos escolhem primeiro chamado da ordem recebida; declaração pede seu chamado; âncora pede limbo (`app/chamados/services/disputa_emissao.py:156`, `:1197`, `:1377`; `app/chamados/services/declaracao_texto.py:376`). Escrita persistente está em dois corpos: `models._nascer` e `fio.reapontar_resolvedora`, explicitamente reconhecido (`app/chamados/services/fio.py:808`, `app/chamados/models.py:3038`, `:3070`, `app/chamados/services/fio.py:926`).

A cópia cria um eleitor comum aos emissores datados e um escritor de destino, mas conserva outros escolhendo destino: C8 escolhe sobrevivente no gate do chamado; reconciliador escolhe histórico único; limbo recebe pai imposto; migração recebe novo chamado (`app/chamados/models.py:374`, `:390`, `:437`; `app/chamados/reconciliador.py:1586`, `:1624`, `:1643`; `app/chamados/services/limbo_folgas.py:226`, `:245`; `app/chamados/services/fio.py:2175`). Esses sítios não são alterados no diff; guarda nova só preserva intervalo já eleito, não elege universalmente (`diff_obra.patch`, fio hunk `@@ -752,7 +808,32 @@`).

**I4: NÃO FECHA no ciclo inteiro, hash e cópia; o escritor final do campo já é único.**

`gravar_dia_do_fato` já é o ponto único de atribuição, e o contrato final exige dia do juiz para fato/conversa civil separadamente (`app/chamados/services/fio.py:791`, `:801`; `app/chamados/models.py:2862`, `:2868`, `:2875`). Isso não unifica as decisões anteriores: órfão com origem classifica o dia **civil** e pode sair antes de resolver dia **do turno** e ler sua ata (`app/chamados/services/disputa_emissao.py:1199`, `:1200`, `:1201`, `:1205`, `:1209`). A sequência permanece na cópia (`diff_obra.patch`, órfãos hunk `@@ -1098,176 +1146,202 @@`). Ocorrência/frequência em produção [nao verificado].

O gerador do hash também monta texto/dedup antes do dia final do modelo; a cópia resolve esses três insumos por `_resolver_pergunta_por_motivo`, melhoria local que não elimina a sequência civil→turno dos órfãos (`app/chamados/services/disputa_emissao.py:345`, `:372`, `:399`; `app/chamados/models.py:2931`; `diff_obra.patch`, `_resolver_pergunta_por_motivo`, novas linhas `:209` seguintes).

## 14/11 19:22 · sessao b468514477ca · E3-V10-DEGRAU-DONO-E-PORTA · verificador · origem=Code

**Veredito: REPROVADO nesta volta.** Há contraexemplo à L-129, divergência entre os universos da carga e do app, dois mutantes obsoletos e insuficiência do selo AST para provar consumo da tarefa. Referências: `app/chamados/services/tarefa_do_dia.py:277`, `app/chamados/services/pergunta_no_app.py:52`, `app/chamados/tests/e3_degrau_mutantes.py:41`, `app/folha/tests/test_l129_fora_da_folha_so_le_a_tarefa.py:57`.

**1. Escritores e leitores**

| Campo/valor | Quem fornece/escreve | Quem lê |
|---|---|---|
| `fio` | Seleção em `app/chamados/services/tarefa_do_dia.py:622`; publicação em `:635` | Projeção em `app/chamados/services/tarefa_do_dia.py:645`; Folha em `app/folha/fora_da_folha.py:85`; vigia em `app/chamados/services/vigia_fio.py:663`; instrumento em `app/chamados/management/commands/diff_e3_sombra.py:54` |
| `rota` | `app/chamados/services/tarefa_do_dia.py:629`; publicação em `:635` | Projeção em `app/chamados/services/tarefa_do_dia.py:645`; furo em `app/chamados/services/furo_sem_canal.py:99`; vigia em `app/chamados/services/vigia_fio.py:662`; instrumento em `app/chamados/management/commands/diff_e3_sombra.py:53` |
| `fila` | Calculada em `app/chamados/services/tarefa_do_dia.py:609`, a partir da alimentação de `app/chamados/juizes.py:1589` | Folha nomeia chamados por ela em `app/folha/fora_da_folha.py:244`; instrumento em `app/chamados/management/commands/diff_e3_sombra.py:143` |
| `verbos` | Painel fornece por `app/chamados/juizes.py:1570`; tarefa seleciona em `app/chamados/services/tarefa_do_dia.py:608` | Residual do próprio juiz em `app/chamados/services/tarefa_do_dia.py:618`; projeção em `:645`; instrumento em `app/chamados/management/commands/diff_e3_sombra.py:144` |
| `donos` | **Conjunto local, não campo publicado**: `app/chamados/services/tarefa_do_dia.py:615` | Seleção do fio em `app/chamados/services/tarefa_do_dia.py:622`; publicação não inclui `donos`, em `:635` |
| Retorno de `porta_da_tarefa` | Cópia dos cinco campos ou `None`: `app/chamados/services/tarefa_do_dia.py:645` | Folha `app/folha/fora_da_folha.py:70`, `:238`; vigia `app/chamados/services/vigia_fio.py:649`; furo `app/chamados/services/furo_sem_canal.py:97`; beco `app/escala/services/cadastro_realidade.py:469`; sombra `app/chamados/management/commands/diff_e3_sombra.py:43` |
| Pendentes/histórico | `particao_do_app`, em `app/chamados/services/pergunta_no_app.py:76`; consulta compõe em `:73` | API `app/api/views.py:994`, `:2534`; dashboard `app/core/views.py:253`; aviso `app/chamados/services/aviso_pendencia.py:9`; cobrança `app/chamados/services/cobranca.py:48`; projeção/badge `app/chamados/services/pergunta_no_app.py:100`; carga `app/chamados/services/tarefa_do_dia.py:514` |
| Vivas do app | Consulta ORM e C6 em `app/chamados/services/pergunta_no_app.py:52`, `:61` | Consulta em `app/chamados/services/pergunta_no_app.py:73`; ficha `app/colaboradores/services/ficha.py:166`; C17 `app/chamados/juizes.py:1642`; diagnósticos `app/chamados/services/pergunta_no_app.py:208`, `:231` |
| C6 na carga | Corpo único em `app/chamados/juizes.py:1399`; fachada em `:862`; alimentação em `app/chamados/services/carga_pergunta_viva.py:69` | Carga chama em `app/chamados/services/tarefa_do_dia.py:521`, `:525`; `viva_c6` alimenta exposição administrativa em `:260`, `:347`; `_pendentes_app` alimenta publicação em `:568`, `:610` |

A pauta lê indiretamente o fio da tarefa e compara com `FIO_SEM_PORTA`; o E4b lê instâncias e ações para escolher atos. Referências: `app/ponto/services/pautas_dos_discordantes.py:79`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:97`.

O retrato grava outro campo, `dono`, a partir do grupo agregado da Folha; depois prefere esse valor gravado. Templates renderizam o resultado e o PDF usa o rótulo do LED. Referências: `app/folha/retrato_folha.py:1158`, `:1190`, `:438`; `app/templates/folha/_retrato_lista.html:47`; `app/templates/folha/_retrato_o_que_fazer.html:33`; `app/folha/views_retrato.py:413`.

**Achado de segundo escritor dos quatro campos publicados: NÃO PROCEDENTE nos caminhos rastreados.** A montagem fica em `_portas_da_tarefa`; os leitores citados projetam, contam ou agregam sua saída. Referências: `app/chamados/services/tarefa_do_dia.py:635`, `app/folha/fora_da_folha.py:85`, `app/chamados/services/vigia_fio.py:662`.

**2. Uma definição de pendência, mas conjuntos diferentes — PROCEDENTE**

O predicado local é compartilhado: `respondida_em is None` e disputa aberta. Não há segunda cópia desse predicado na carga. Referências: `app/chamados/services/pergunta_no_app.py:81`, `app/chamados/services/tarefa_do_dia.py:514`.

O universo, porém, difere:

- A carga seleciona `colaborador_id`, `conversa=False` e `data_evento` dentro da janela. O app seleciona `disputa__colaborador` e `q_vivas`, sem esses recortes de conversa/data. Referências: `app/chamados/services/tarefa_do_dia.py:395`, `app/chamados/services/pergunta_no_app.py:52`.
- Mesmo dentro da janela, um `agora` histórico pode divergir: o app elimina previamente perguntas com `validada_em` preenchido; a carga admite a pergunta e C6 desconsidera validação posterior ao instante consultado. Referências: `app/chamados/models.py:3468`, `app/chamados/juizes.py:1387`, `app/chamados/services/tarefa_do_dia.py:395`.
- Resposta e fechamento da disputa são lidos **no estado atual** pela partição. C6 projeta resposta futura para inexistente, mas a partição continua vendo seu carimbo preenchido. Isso afeta ambas as composições. Referências: `app/chamados/services/pergunta_no_app.py:81`, `app/chamados/juizes.py:1390`.
- Os dois campos de colaborador devem coincidir pela porta de identidade. Divergência efetiva de colaborador/empresa em dados válidos: **[nao verificado]**. Referências: `app/chamados/models.py:3573`, `app/chamados/services/tarefa_do_dia.py:390`, `app/chamados/services/pergunta_no_app.py:59`.

A igualdade alegada nos comentários, inclusive os números de frota, não constitui prova disponível de equivalência: **[nao verificado]**. Referência da alegação: `app/chamados/services/tarefa_do_dia.py:44`.

**3. Juízes, escritores e relógios**

- **C6 duplicado na carga: NÃO PROCEDENTE.** A carga chama o corpo registrado e reaproveita o resultado quando a pergunta já foi julgada como candidata. Referências: `app/chamados/services/tarefa_do_dia.py:518`, `:521`, `:523`; `app/chamados/juizes.py:888`.
- **C17 paralelo nos leitores migrados: NÃO PROCEDENTE.** O chamado remanescente encontrado é o lado anterior do instrumento de comparação. Referências: `app/chamados/management/commands/diff_e3_sombra.py:127`, `app/core/juizes.py:136`.
- **Tabela paralela de verbo no novo juiz: NÃO PROCEDENTE.** Ele reutiliza `fio_pelos_verbos`, excluindo `cobrar` antes. Referências: `app/chamados/services/tarefa_do_dia.py:618`, `:620`; `app/chamados/juizes.py:1650`.
- **C9 substituído por `sla.vencido` na escolha do fio: NÃO PROCEDENTE.** A escolha não consulta prazo. Entretanto, o painel continua exibindo outro SLA, calculado por referência/urgência; esse valor não comprova vencimento C9. Referências: `app/chamados/services/tarefa_do_dia.py:610`, `app/chamados/catalogo/motor.py:54`, `app/chamados/services/painel.py:378`, `app/chamados/services/sla.py:26`.
- **Relógio novo dentro da tarefa: NÃO PROCEDENTE.** O instante segue à carga, ao C6 e à alimentação; a casca rejeita instante nulo ou carga incompatível. A API atual não fornece esse instante, e C6 conserva fallback de calendário. Referências: `app/chamados/services/tarefa_do_dia.py:519`, `:565`, `:657`; `app/api/views.py:2534`; `app/chamados/juizes.py:592`.

Há derivações de “dono” com **outra unidade**: a ficha traduz verbo do chamado; o relatório classifica disputa por `aguarda_o_colaborador`. Uma resposta vazia carimbada pode estar no histórico do app e ainda sustentar `AGUARDA_COLAB` no relatório. Isso é diferença semântica, não prova de outro juiz do colab-dia. Referências: `app/colaboradores/services/ficha.py:159`, `app/chamados/relatorios_disputa.py:26`, `app/chamados/services/fio.py:187`, `app/chamados/services/pergunta_no_app.py:81`.

**4. Residual `cobrar` e preservação do admin**

**Residual sozinho dar dono: NÃO PROCEDENTE.** O filtro exclui `cobrar`; sem outro dono, não publica rota. A porta `validar` do chamado ligado à ação do colab continua residual quando falta pendência no app, preservando o admin. Referências: `app/chamados/services/tarefa_do_dia.py:613`, `:618`, `:629`; testes em `app/chamados/tests/test_e3_degrau_dono_e_porta.py:58`, `:105`, `:113`.

**E4b esconder classe e como resolvida: NÃO PROCEDENTE por leitura.** O teste exige `segue_sem_porta`, reabertura, nota única, idempotência e manutenção de um dia sem porta no universo. Execução ORM: **[nao verificado]**. Referências: `app/chamados/tests/test_e4_passivo.py:76`, `:81`, `:86`, `:89`, `:117`, `:125`.

A cobertura denominada “B” exige cuidado: o agrupador produz grupo B a partir de fio colab; já `RedBRespostaTextual` usa perguntas respondidas e hipótese de resposta E1, esperando `nada`. Esse teste não certifica a classe B pendente da lei fornecida. Referências: `app/folha/fora_da_folha.py:97`, `app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:108`, `:120`, `:128`.

**Há caminho que perde o colab apesar da pendência: PROCEDENTE**, detalhado no item 8. Referências: `app/chamados/services/tarefa_do_dia.py:277`, `:610`.

**5. Mordida dos testes e mutantes** [nao verificado]

**PROCEDENTE — `--todos` não conclui.** `sem_memo_por_colab` e `carga_sem_relogio` ainda apontam para implementação/teste removidos na passagem para lote. A validação textual levanta antes do controle verde. Referências: `app/chamados/tests/e3_degrau_mutantes.py:41`, `:43`, `:87`, `:92`, `:112`.

**PROCEDENTE — selo AST insuficiente.** Proíbe imports/nomes, mas a anti-vacuidade exige apenas presença dos imports permitidos. Não exige chamadas nem consumo da resposta; preservar esses imports permite passar sem ler a tarefa. O degrau apenas executa esse selo. Referências: `app/folha/tests/test_l129_fora_da_folha_so_le_a_tarefa.py:24`, `:57`, `:61`; `app/chamados/tests/test_e3_degrau_dono_e_porta.py:293`.

| Teste alterado | Veredito sobre a mordida | [nao verificado]
|---|---|
| `n1` | **NÃO PROCEDENTE** a vacuidade “ninguém foi chamado”: junto ao mapa vazio exige uma chamada à partição. Não prova equivalência dos conjuntos. `app/chamados/tests/test_n1_alimentacao_unica.py:114`, `:115`, `:164` |
| `portas_tarefa` | **NÃO PROCEDENTE** ficar vazio: exige C6 exatamente para `[p.pk]`, alimentação única e repetição sem consultas. `app/chamados/tests/test_e3_portas_tarefa.py:139`, `:144`, `:146`, `:152` |
| `pos_suite` | **NÃO PROCEDENTE** perder toda mordida: agora protege a grade respondida, fora dos pendentes. O contrato ficou mais estreito. `app/chamados/tests/test_e3_pos_suite_sem_banco.py:50`, `:65`, `:67` |
| `e4_passivo` | **NÃO PROCEDENTE** mascarar ausência de efeito: exige que a classe e continue contada sem porta. `app/chamados/tests/test_e4_passivo.py:81`, `:122`, `:125` |
| `o261e_f5_alcance` | **PROCEDENTE, limitado:** prova influência da partição, mas deixou de provar ausência de consulta paralela pelo vigia; um leitor paralelo da mesma partição também poderia acompanhar o mock. `app/chamados/tests/test_o261e_f5_alcance.py:273`, `:274`, `:278`, `:280` |

Os testes C1/C2 fornecem um campo sintético `prazo_c9_estourou` que o juiz não lê; demonstram independência desse campo, não integração com C9 real. Referências: `app/chamados/tests/test_e3_degrau_dono_e_porta.py:44`, `:48`, `:300`.

Na execução sem banco, degrau+pós-suite+portas-v6 deram **50 testes OK**; o runner completo abortou no alvo obsoleto. Isoladamente, 19 mutantes tiveram controle verde e morreram; dois falharam por alvo inexistente. Fontes: `app/chamados/tests/test_e3_degrau_dono_e_porta.py:40`, `app/chamados/tests/test_e3_pos_suite_sem_banco.py:19`, `app/chamados/tests/test_e3_portas_v6_sem_banco.py:15`, `app/chamados/tests/e3_degrau_mutantes.py:84`. Registro persistente desses resultados: **[nao verificado]**.

**6. Semântica**

`rota` é classificação da composição de portas do dia; não identifica necessariamente a porta particular do dono vencedor. O fio é escolhido antes da rota. `porta_app` também não implica fila vazia: `cobrar` pode permanecer em `fila/verbos`. Referências: `app/chamados/services/tarefa_do_dia.py:608`, `:622`, `:632`, `:634`.

`nada` e `nao_sei` são distintos na tarefa, mas ambos ficam sem rota. A Folha os projeta para `FIO_SEM_PORTA='nada'`, preservando separadamente a flag de desconhecimento. Há ainda `sistema` sem rota. Referências: `app/chamados/services/tarefa_do_dia.py:623`, `:629`, `:634`; `app/chamados/juizes.py:1652`; `app/folha/fora_da_folha.py:85`, `:248`.

Os leitores diretos aceitam a presença da projeção: Folha lê fio, beco testa presença, furo reconhece as três rotas e vigia conta pela chave publicada. Referências: `app/folha/fora_da_folha.py:85`, `app/escala/services/cadastro_realidade.py:469`, `app/chamados/services/furo_sem_canal.py:114`, `app/chamados/services/vigia_fio.py:662`.

**PERGUNTA:** “sem porta explícito” também substitui o agrupamento do retrato/PDF? O código mantém `ninguem → Admin/Hoje`; “SEM PORTA” aparece no drawer, enquanto o PDF imprime o LED. Referências: `app/folha/retrato_folha.py:192`, `:477`, `:1069`; `app/folha/views_retrato.py:413`.

**7. Comentários/docstrings incompatíveis — PROCEDENTE**

- “Uma consulta do app por colab, memo da carga” descreve implementação abandonada; o corpo faz partição em lote. Referências: `app/chamados/services/tarefa_do_dia.py:362`, `:514`.
- “TODO chamado vivo na fila” publica porta e “CHAMADO VIVO NA FILA TEM PORTA” não ressalvam o residual `cobrar` agora excluído. Referências: `app/chamados/services/tarefa_do_dia.py:28`, `:580`, `:618`, `:629`.
- A promessa de conservar portas de chamados vivos também necessita dessa ressalva. Referências: `app/chamados/services/tarefa_do_dia.py:641`, `:618`.
- Docstrings dos testes `n1` ainda descrevem consulta/memo por colaborador, contrariando seus próprios comentários e asserções de lote. Referências: `app/chamados/tests/test_n1_alimentacao_unica.py:106`, `:112`, `:143`, `:163`.

**8. Contraexemplo à lei — PROCEDENTE**

Caso **sintético**, executado em memória com corpos reais: pergunta 303 de `orfao_14h`, sem resposta, validação ou materialização; disputa aberta; célula `furo`; marco `fim` aceso e vencido; sem tranca e fila vazia. O SQL admite a pergunta e C6 retorna viva porque a célula acusa. Referências: `app/chamados/models.py:3467`, `app/chamados/juizes.py:1444`, `:1450`; mecanismo de execução em `app/chamados/tests/e4_apoio.py:8`.

Resultado: pendentes `[303]`, mas tarefa com **`fio='nada', rota=None`**. A tarefa pula a lâmpada acesa antes de associar a pergunta; depois exige simultaneamente pendência e ação `RESPONDER` na instância. Referências: `app/chamados/services/pergunta_no_app.py:73`, `:81`; `app/chamados/services/tarefa_do_dia.py:277`, `:321`, `:610`, `:622`.

A API serve as pendentes e sua porta de resposta consulta C6, sem acrescentar guarda de lâmpada acesa. Portanto não basta alegar que essa pergunta estaria inacessível por estar acesa. Referências: `app/api/views.py:2535`, `:2572`, `:2608`, `:2611`.

Também existe contraexemplo histórico na direção inversa: validação posterior a `agora` exclui a pergunta do app pelo pré-filtro atual, mas C6 da carga pode considerá-la pendente e permitir fio colab. Referências: `app/chamados/models.py:3468`, `app/chamados/juizes.py:1387`, `app/chamados/services/tarefa_do_dia.py:523`.

Incidência desses casos em produção: **[nao verificado]**.

**9. Pouso** [nao verificado]

**Dependência nova de migration ou cron: NÃO PROCEDENTE nos caminhos alterados examinados.** Os novos valores são montados em memória; a saída do comando acrescenta o contador app ao fluxo existente. Referências: `app/chamados/services/tarefa_do_dia.py:391`, `:635`, `app/chamados/management/commands/lavrar_furo_sem_canal.py:37`.

Estado real de migrations, implantação conjunta, outras raias e cumprimento das condições de produção/exportação: **[nao verificado]**. A reprodução `--base` também permanece **[nao verificado]**, pois depende de `git show` da base. Referência: `app/chamados/tests/e3_degrau_mutantes.py:107`.

**PERGUNTA:** conversas pendentes do app pertencem a algum colab-dia para esta lei? O app as admite; a projeção declara que não têm dia de fato, e a carga as exclui. Referências: `app/chamados/services/pergunta_no_app.py:52`, `:114`, `app/chamados/services/tarefa_do_dia.py:396`.

## 14/11 19:22 · sessao 88570989b070 · - · - · origem=Ronald (tmux)

Referências abaixo relativas à raiz; nada foi executado contra banco/log [nao verificado].

**Portas e chamadores**

| Porta | Chamador comprovado |
|---|---|
| `app/chamados/views_cobrar.py::cobrar_chamado:24` | View POST, URL `app/chamados/urls.py:25`; permissão/área em `views_cobrar.py:25` e `:31`. |
| `app/chamados/views_cobrar.py::cobrar_massa:61` | View POST, URL `app/chamados/urls.py:31`; seleção VIVOS/verbo/folha em `views_cobrar.py:76`, `:78`, `:95`. |
| `app/chamados/services/cobranca.py::cobrar_colaborador:140` | View IA `_aplicar_cobranca:834`, chamada indireta `app/core/views_ia.py:850`, escritor registrado `app/core/intake.py:67`. |
| `app/chamados/services/disputa_emissao.py::recobrar_questionario:1690` | View `reabrir_disputa_supervisao`, chamada `app/chamados/views.py:1425`, somente após guarda `pend_vivas` em `:1417` e ausência de corrigíveis `:1421`. |
| `app/ponto/management/commands/alertar_disputas_pendentes.py::Command.handle:30` | Cron 08:00, `app/config/crons.py:745`. |
| `app/chamados/services/disputa_emissao.py::_enviar_push_disputa_colab:1757` | Emissor central; não consulta C6, somente silenciamento em `:1759`; envia webpush `:1777` e FCM `:1785`. |

**Matriz — cobrar individual**

| Estado | Juiz | Persiste | Push | Prova/teste | Cópia | [nao verificado]
|---|---|---|---|---|---|
| a ata; b sem ata; c sem marco; d respondida; e morta; DS aberta/fechada/inexistente | Nenhum juiz de pergunta/DS: recebe qualquer chamado existente `app/chamados/views_cobrar.py:29`; guarda é somente permissão, área e throttle `:25`, `:31`, `:36`. | Nenhuma escrita DS/P; sucesso cria `FollowupChamado` em `:55`, pelo escritor `app/chamados/services/nota.py:103`. | Tenta FCM ao `ch.colaborador`, todos os estados, em `views_cobrar.py:44`; entrega depende driver `app/chamados/fcm_utils.py:104`. | `app/chamados/tests/test_e5_cobrar.py:34` cobre envio/trilha/throttle; `:48` falha sem token. | Novo `_enviar_cobranca` consulta `_disputa_do_chamado` e app quando encontra DS aberta; **se não encontra DS e categoria do chamado não é `disputa_supervisao`, envia diretamente** (anexo diff, `app/chamados/views_cobrar.py`, hunk `+20`). |

**Contraexemplo importante da cópia para I2:** chamado de furo, categoria diferente de `disputa_supervisao`, somente dona FECHADA, pergunta respondida ou morta. `_disputa_do_chamado` só busca dona ABERTA (`app/chamados/services/fio.py:131`, `:135`); retorna `None`; novo `elif ch.categoria != 'disputa_supervisao': enviar()` manda FCM sem pergunta pendente. Individual não exige VIVOS (`app/chamados/views_cobrar.py:29`). Portanto **I2 NÃO FECHA na cópia** mesmo restringindo a cobrança ao colaborador (anexo diff, `views_cobrar.py`, hunk `+20`; hash `fio.py:131`).

**Matriz — massa**

| Estado | Juiz | Persiste/push | Prova/teste | Cópia | [nao verificado]
|---|---|---|---|---|
| a/b/c, DS aberta/fechada/inexistente, desde que crédito da consulta produza verbo cobrar | Nenhum C6 atual no caminho quente: `particionar_verbos` lê créditos (`app/chamados/services/verbos.py:34`); crédito usa `q_vivas` (`app/chamados/services/raiox.py:74`) e exclui materializada+VL_SIM `:135`; não filtra DS fechada (`:56`). | FCM ao colab `views_cobrar.py:123`, uma seleção por pessoa `:97`; sucesso Followup `:132`. DS/P sem escrita nessa porta. | `app/chamados/tests/test_e5b_cobrar_massa.py:52` dedup/placar; `:74` prévia sem enviar. | Acrescenta `pendentes_da_disputa` se `_disputa_do_chamado` encontra DS aberta; preserva envio direto não-pai sem DS aberta (diff `views_cobrar.py`, hunks `+118`, `+20`). |
| d respondida creditada | Resposta ganha verbo validar: `raiox.py:139`; `catalogo/natureza.py:48` e `:59`. | Não entra no conjunto cobrar `views_cobrar.py:78`, salvo outra pergunta que produza crédito cobrar sob outro chamado [nao verificado]. | Predicados acima. | Gate app adicional no recorte DS aberta (diff `views_cobrar.py`, `+118`). |
| e morta C6 | A consulta de crédito pode ficar stale: código declara que conta o campo como está até próximo ciclo (`raiox.py:117`) e não consulta juiz no caminho quente (`:120`). | Possível crédito SQL divergente de C6 atual; envio permanece possível se verbo cobrar (`views_cobrar.py:78`, `:123`). | `raiox.py:117`. | C6 reaplicado se encontra DS aberta; bypass de dona fechada permanece (diff `views_cobrar.py`, `+20`, `+118`). |

**Matriz — cobrança por pessoa**

| Estado | Juiz | Persiste/push | Prova/teste | Cópia | [nao verificado]
|---|---|---|---|---|
| a/b/c, pergunta viva sem resposta, DS aberta | C6 através `_pendentes`→`consulta_do_app` (`cobranca.py:45`; `pergunta_no_app.py:70`). Ata/marco não são portas diretas desta cobrança. | `ChamadoColaborador.prazo_sla` dos pais via `definir_prazo_sla`, `cobranca.py:178`; Followup `:181`; LogAuditoria `:188`; FCM primeiro/webpush fallback ao colab em `:107`, `:112`. | `test_cobranca.py:102` um toque; `:115` prazo; `:139` trilha. | Mesmo prazo/trilha; aviso passa por lista app antes/depois commit (diff `cobranca.py`, `+179`; novo `pergunta_no_app.py:82`). | [nao verificado]
| d respondida, e morta, DS fechada, ou nenhuma pergunta | App exclui morta antes (`pergunta_no_app.py:58`); respondida/DS fechada vão histórico `:71`; zero pendente retorna sem cobrar `cobranca.py:156`. | Nada/push não, se não existe outra pendente da pessoa (`cobranca.py:156`). | `test_cobranca.py:86` sem pendência. | Conserva e acrescenta rechecagem após commit (diff `pergunta_no_app.py:82`). | [nao verificado]
| Prazo futuro em algum pai das pendentes | Predicado local `cobranca.py:86`. | Nada/push não `:159`. | `test_cobranca.py:127`. | Conservado, diff não altera este bloco. | [nao verificado]

A cobrança por pessoa já utiliza a lista correta no hash, mas o envio fica depois de seu atomic interno e pode ainda ocorrer dentro de atomic externo, sem rechecagem imediatamente após commit (`cobranca.py:176`, `:187`). Cópia acrescenta esse controle (diff `cobranca.py`, hunk `+179`). [nao verificado]

**Matriz — recobrar questionário**

| Estado | Juiz | Persiste/push | Prova/teste | Cópia | [nao verificado]
|---|---|---|---|---|
| a/b/c, pendente viva, DS aberta, sem corrigível | A view consulta C6 `app/chamados/views.py:1417`; o serviço conta apenas `respondida_em IS NULL` `disputa_emissao.py:1712`. | Prazo do chamado `:1718`; Followup `:1721`; LogAuditoria `:1734`; FCM ao colab da DS `:1728`. | `test_recobrar_questionario.py:88` prazo; `:96` push. | Serviço usa `pendentes_da_disputa`, retorna imediatamente se zero; usa `_enviar_push_disputa_colab` com gate após commit (diff `disputa_emissao.py`, `+1785`). |
| d respondida corrigível | View segue reabertura, não recobrança `views.py:1435`; FCM após reabertura `:1447`. | Reabertura canônica e Followup `:1440`. | `test_recobrar_questionario.py:112`. | FCM envolve `notificar_pendentes_apos_commit` (diff `views.py`, `+1444`). |
| e morta sem resposta | View não chama serviço se nenhuma pendente viva `views.py:1431`; **serviço avulso não possui essa guarda**, conta cru e sempre tenta FCM `disputa_emissao.py:1712`, `:1728`. | Pela view: nada/push não; serviço direto: prazo/trilha/push mesmo zero pendentes, `:1716`, `:1728`. | `test_recobrar_questionario.py:122` cobre não apagar resposta, não cobre morto/zero. | Serviço avulso também protegido por app/gate (diff `disputa_emissao.py`, `+1785`). | [nao verificado]
| DS fechada/inexistente | `_disputa_do_chamado` exclui fechada `fio.py:131`, `:135`; view retorna `views.py:1411`. | Nada/push não pela view. | `test_recobrar_questionario.py:102`. | Conservado. | [nao verificado]

**Matriz — cron 08:00**

| Estado | Juiz | Persiste/push | Prova/teste | Cópia | [nao verificado]
|---|---|---|---|---|
| a/b/c/d/e, inclusive zero perguntas, DS aberta | Nenhum C6/ata/marco: universo é só `fechada_em IS NULL` (`alertar_disputas_pendentes.py:33`). | `DisputaSupervisao.contexto.ultimo_alerta_diario` por `renovar_contexto` `:48`, escritor `models.py:2177`; webpush colab `:59`. Empresa agrupa todas as abertas `:70`; webpush supervisão `:103`; LogAuditoria `:109`; >7 dias log `:75`. | `test_porta_anotacao_contexto.py:214` mudança de data; `:219` idempotência; não verificado teste C6 deste cron. | Somente aviso colab/carimbo passam app antes/depois commit (diff `alertar_disputas_pendentes.py`, `+41`); supervisão/agrupamento/log não alterados no diff. | [nao verificado]
| DS fechada/inexistente | Filtro inicial `alertar_disputas_pendentes.py:33`. | Nada/push não. | Mesmo filtro. | Conservado. | [nao verificado]

**C6 e app — fonte comum da cópia**

C6 mata validada, dia trancado e vias terminais (`app/chamados/juizes.py:1401`, `:1405`, `:1412`); resposta com texto pode continuar viva para decisão (`:1417`), porém app a coloca em histórico (`pergunta_no_app.py:71`). Sem ata/veredito, pergunta não materializada pode permanecer viva (`juizes.py:1476`). Portanto “sem ata” e “sem marco” isoladamente **não determinam** push; é preciso estado integral C6 e pendência app (`pergunta_no_app.py:58`, `:71`).

**Outros pushes relacionados** [nao verificado]

| Porta | Hash | Cópia |
|---|---|---|
| `acoes_disputa::acao_reabrir_disputa:90` | Corrigíveis `:97`, reabre `:103`, FCM colab `:109`; sem rechecagem C6 depois. | Gate app/commit (diff `acoes_disputa.py`, `+90`). | [nao verificado]
| `acoes_disputa::acao_regularizar_disputa:118` | Bloqueia viva sem resposta `:130`; materializa `:139`; mesmo após fechar tenta webpush/FCM “regularizada” `:150`, `:156`. | Ambos envolvidos no gate pendência, portanto fechamento/zero pendente suprime esses avisos (diff `+149`). |
| `fio::processar_post_fio:485`, regularizar | Guarda viva sem resposta `:660`; webpush/FCM regularização `:681`, `:688`. | Gate app/commit (diff `fio.py`, `+678`). | [nao verificado]
| `acoes_disputa::acao_reabrir_disputa_admin:235` | DS fechada `:255`, reabre `:259`, conversa `:263`, push supervisão `:277`, FCM colab `:288`. | Só FCM colab passa gate; supervisão permanece (diff `+293`). | [nao verificado]
| `validacao::recusar_pergunta:573` | Recusa somente respondida não validada `:600`, `:603`; recusa+reabertura atomic `:615`; FCM colab `:640`. | Gate app/commit (diff `validacao.py`, `+638`). | [nao verificado]
| `fio::processar_post_fio`, mensagem DP | Nota `:718`, webpush `:726`, FCM `:734`, sem condição C6. | Sem mudança nesse bloco no diff fornecido. |

Driver FCM verifica somente token, não pergunta (`app/chamados/fcm_utils.py:101`, `:104`); envio efetivo chama `messaging.send` em `:85`. Driver webpush verifica usuário/assinatura, não pergunta (`app/chamados/push_utils.py:114`, `:24`); supervisão recebe por setores ativos da empresa `:98`, `:100`, `:106`.

**Para invariantes:** I2 hash **NÃO FECHA**, contraexemplos individual/cron/serviço recobrar avulso (`views_cobrar.py:44`; `alertar_disputas_pendentes.py:59`; `disputa_emissao.py:1728`). Cópia **NÃO FECHA** pelo bypass do filho com dona fechada (diff `views_cobrar.py`, hunk `+20`, cruzado com `fio.py:131`). Se I2 se aplicar literalmente a todo push, mensagem do DP e supervisão também ficam fora dessa restrição (`fio.py:734`; `alertar_disputas_pendentes.py:103`). [nao verificado]

## 14/11 19:21 · sessao 2a4e6c031622 · - · - · origem=Ronald (tmux)

Escopo: `E = app/chamados/services/disputa_emissao.py`; `D = app/chamados/services/declaracao_texto.py`. Referências “cópia” abaixo são linhas novas dos hunks fornecidos, não código executado. Testes da cópia só constam na lista; cobertura e resultados deles são [nao verificado].

| ID / estado | Juiz / predicado | Persistência no hash | Push | Prova / cópia | [nao verificado]
|---|---|---|---|---|
| N1 — motivos vazios, qualquer DS | nenhum: guarda local | nada | não | E:656. Sem mudança no diff. |
| N2 — DS inexistente ou só fechadas; com ata e dia resolvível | nenhum para nascer; busca somente DS aberta | cria/reusa `ChamadoColaborador`; cria `DisputaSupervisao(motivos, contexto, chamado)` antes de perguntar | sim, para colab, independentemente do resultado do gerador | E:531,568,587,669. Cópia E::_abrir_ou_agregar_disputa novo :474: ensaio/savepoint; rollback sem motivo local, replay estrangeiro. |
| N3 — mesma entrada, sem ata ou sem marco | nascimento não consulta ata; gerador só consulta grade se texto não pode ser montado | DS/pai já persistidos; pergunta pode nascer se contexto tem data; sem dia/texto, zero perguntas | sim, para colab | E:345,350,360,371,425,669. Cópia passa pelo resolvedor novo :213 e ensaio; não equivale a exigir pergunta **viva**, pois `locais()` só exclui `VIAS_FORA_DO_FATO`. |
| N4 — DS aberta | nenhum para agregar: união local de motivos + merge contexto | atualiza `DS.motivos`, `DS.contexto`; incremento `qtd`, `ultima_em` | só se motivo novo; repetição do motivo pode produzir pergunta de outro dia sem push | E:535,548,565,682,685,686. Cópia `_contexto_do_ato` novo :387 e agregador novo :408; motivos admitidos são os das perguntas locais; contexto é salvo antes do gerador. | [nao verificado]
| G1 — pergunta do dia respondida, viva ou morta, na própria DS | nenhum/C6 ausente do dedup do gerador | dedup por existência, sem ler `respondida_em` nem `pergunta_viva`; zero nova se identidade e via correspondem | gerador não envia; wrapper segue N2/N4 | E:325,398,417. Cópia `_resolver_pergunta_por_motivo` novo :213 mantém dedup por existência. |
| G2 — pergunta canônica pertence a outra DS aberta/fechada | porta da identidade dos fatos; resolvedora + costura no reuso | `PerguntaDisputa.abrir` pode devolver pergunta estrangeira e atualizar célula/destino/lavra; emissor ignora `_criada` e sempre inclui retorno; DS recém-criada pode ficar vazia | wrapper envia mesmo assim no hash | E:444,470; models.py:2977,3020,3024,3053,3064. Cópia distingue `ReusadaDeOutraDS`, desfaz nascimento sem pergunta local e agrega à dona; dona fechada não é salva, descarte em trilha pós-commit (diff descarte_disputa.py:5). | [nao verificado]
| G3 — nenhum dia para pergunta de hora | nenhum: predicado local de ausência de dia | nenhuma pergunta; DS já nasceu no wrapper | wrapper envia no hash | E:424–430; teste existente `test_3b_reuso_746.py:23` chama com contexto vazio e espera DS. Cópia recusa `InsumoAusente` e rollback do nascimento. | [nao verificado]
| P1 — entradas pendentes | GEOMETRIA DE TURNO / `dia_do_turno_da_batida`; FURO DO DIA / `classificar_falta` | para cada dia acusado, injeta contexto e chama gerador; salva `DS.contexto` se lista retornada não vazia | nenhum nesta função | E:257,282,288,291,295. Cópia novo :136 conserva contexto anterior, encaminha motivo de estrangeira à dona e coleta criadas. |

| ID / órfãos | Juiz / predicado | Persistência no hash | Push | Prova / cópia | [nao verificado]
|---|---|---|---|---|
| O0 — parcial na lista do admin | `cartorio.parcial_so_lista` (não registrado com esse nome em JUIZES fornecido) | nada; filtro antes de DS/pai | não | E:916,940,1047. Sem mudança. |
| O1 — elegível, DS inexistente/só fechadas | nenhum para nascer: filtro aberto | cria pai e DS antes das guardas de ata/falta; seed `orfao_14h` | depende das próximas linhas | E:1096,1101,1123. Cópia mantém nascimento antecipado no agrupador; rollback específico apenas nova + estrangeiras + zero perguntas. |
| O2 — com ata, marco apagado, rota “por ata” | DIA DO CHAMADO (`data_do_chamado`); MARCOS / leitor da célula + lâmpadas | uma pergunta por marco, via `_complementar_marcos_faltantes` → `_criar_pergunta_se_couber`; agrega motivos e `ch.analisar()` | sim se não fechar e `notificar` | E:1166,1170,1171,1175,873,837,1289. Cópia admite local e encaminha estrangeira à dona; somente push com pendentes da DS nova :1365. | [nao verificado]
| O3 — sem ata, com escala vigente | `_marcos_faltantes` retorna `sem_celula`; guarda local + escala vigente | DS/pai permanecem; nenhuma pergunta; `_absteve_por_ata` impede auto-fecho | **sim no hash**, se `notificar` | E:902,908,911,1172,1279,1289. Teste prova explicitamente DS aberta com zero perguntas: `test_hx_disputa_muda_sem_ata.py:51–61`. **Cópia mantém essa DS aberta**, mas não envia push sem pendentes (novo :1365). I1 não fecha na cópia por esta linha. | [nao verificado]
| O4 — ata de trabalho sem marcos/lâmpadas | `_marcos_faltantes` trata como `sem_celula` | igual O3 | igual O3 | D:121–126; E:902. Diff não muda esse predicado. |
| O5 — sem ata **e sem escala vigente** | FURO DO DIA; exceção local `_ata_do_dia` | permite perguntar só por batidas; sem irmãs da ata | sim se sobra aberta | E:908–910; E:1200,1205,1213. Sem alteração dessa exceção. |
| O6 — falha ao ler ata | nenhum: `except` local | `return True,None`; mantém comportamento de perguntar sem bloqueio da ata | sim se sobra aberta | E:897–901. Sem alteração no diff. |
| O7 — com ata, nenhum marco faltante / classificador não acusa | `classificar_falta`, depois DISPUTA FECHA / `DS.pode_fechar` | DS/pai podem ter nascido; auto-fecha DS se C7 permite e não houve abstenção | não após auto-fecho | E:1201,1238,1278,1282,1284,1288; `test_hx_disputa_muda.py:24,44`; `test_hx_disputa_muda_sem_ata.py:63`. Cópia conserva. |
| O8 — pergunta respondida mas viva | FIO VIVO / `fio_vivo` → C6 | `_ja_perguntou` bloqueia nova; resposta não basta para decidir | push do agrupador não distingue resposta no hash | E:1415–1422; `_criar_pergunta_se_couber`:832; push:1289. Cópia usa pendência efetiva no app para push. | [nao verificado]
| O9 — pergunta morta C6=false, colaborador ativo | FIO VIVO / C6 | `_ja_perguntou` devolve falso, permite chamar `abrir`; resultado final sujeito à identidade canônica, não necessariamente nova pergunta | hash pode enviar se DS não fecha | E:1415–1422,837; models.py:2977,3064. Cópia encaminha retorno estrangeiro e restringe push; dedup do gerador direto continua G1. | [nao verificado]
| O10 — pergunta morta, colaborador não ativo | nenhum: guarda local | existência da linha bloqueia nova pergunta antes de C6 | nenhum garantido; resto da DS determina | E:1416–1420. Sem mudança. |
| O11 — intervalo explícito | nenhum juiz de ata nesta rota; contexto `marcos_faltantes` → motivo/data | cria pergunta com data do marco e destino próprio `_chi`; não chama `_ata_do_dia` nessa rota | sim se permanece aberta | E:1138–1155. Cópia fotografa fatos em `_marcos_de_intervalo`, depois passa pelo eleitor central. |
| O12 — pergunta não materializada já ligada ao chamado; dona inclusive fechada | **proxy SQL**, não C6: `materializada_em is null`; depois leitor de ata | complemento pode salvar `DS.motivos` da dona fechada | fluxo pode retornar antes do push principal | E:1070,1073,1081,1084,1093. Cópia `_agregar_motivos_na_dona` recusa salvar dona fechada e registra descarte; notifica complementos somente dona aberta. | [nao verificado]
| O13 — sem origem e sem base/data | DIA DO CONTEXTO; base por batidas do turno ou `eh_dia_trabalho` | DS/pai já nascidos, pergunta não nasce; C7 pode fechar | não se auto-fecha | E:1232,1235; `_tem_base`:816–820; E:1282. Cópia conserva. |

Chamadores:

- Nascimento: view `api/views.py::api_bater_ponto:764`; duplicata `api/views_core.py::api_bater_ponto:758`; `ponto/services/triagem_batida.py::ramo_registrar_es:735`; command `marcar_foto_ausente_retro.py:96`; `fabrica_da_celula.py::perguntar:172` e gerador novamente `:174`; este serviço é alcançado por `ponto/services/cartorio.py:1045`, `chamados/reconciliador.py:1387` e command `backfill_fio_morto_e4b.py:143`.
- Órfãos: command `processar_alertas_turno.py::_abrir_chamado_critico:385` (cron */5, `config/crons.py:362`); command `processar_alertas_avancados.py::handle:159` (*/30, `:363`); command `detectar_ausencias.py::_abrir_chamado:803` (*/5, `:364`); ação de view `acoes_disputa.py::acao_iniciar_disputa_orfaos:222`; serviço/view `cobrar_dia.py::cobrar_dia:49`; passivo `pergunta_no_app.py::perguntar:246`, alcançado pelo command `perguntar_chamados_sem_pergunta.py:20`.
- Gerador direto adicional: command `smoke_chamados.py:103`.
- `DisputaSupervisao` fechada não é selecionada como recipiente nos wrappers de nascimento (`E:531,1096`); pode ser dona de pergunta canônica estrangeira (`models.py:2977,3064`) ou recipiente do complemento escolhido por FK (`E:1073`).

F1/F2+: o diff implementa taxonomia/contrato de saída em `_gerar_perguntas_com_causas` (`JaExisteLocal`, `InsumoAusente`, `ReusadaDeOutraDS`, novo E:198), resolvedor compartilhado (`novo E:213`), eleição explícita “RAIZ-F1” em `fio.py::eleger_destino_da_pergunta` novo :752 e tratamento observado de motivo em dona fechada (`descarte_disputa.py:5`; contador I23 novo auditor :78). Mas ensaio+rollback+replay do nascimento, atomic unindo DS/pergunta em declarações e órfãos são implementação F2; writer central de destino/selos aproximam F5; contadores I7a/I7b/I23 aproximam F7. Uma matriz F1 documental completa e aprovação de contrato no anexo não são demonstradas [nao verificado].

Pergunta de lei candidata: “Quando o marco ainda exige resposta, mas a pergunta canônica pertence a uma DS fechada e o novo motivo não consta nela, qual deve ser a transição do ciclo?” O código da cópia publica I23 como “decisão na RAIZ-DA-DISPUTA F1” e só registra descarte (diff auditor novo :79; descarte_disputa.py:5); hash reusa sem mudar dona (`models.py:3064`). Não propor resposta.

## 14/11 19:19 · sessao e4253e35603f · - · - · origem=Ronald (tmux)

Achados para perguntas 2/3/8:

1. **PROCEDENTE — carga e consulta não têm o mesmo universo.** A carga lê `colaborador_id`, `conversa=False` e `data_evento` na janela (`app/chamados/services/tarefa_do_dia.py:395`); o app lê `disputa__colaborador` e `q_vivas`, sem recorte de data/conversa (`app/chamados/services/pergunta_no_app.py:52`). A identidade dos dois colaboradores é exigida na porta de escrita (`app/chamados/models.py:3573`), portanto divergência nesse campo em dados válidos não está demonstrada [nao verificado].

2. **PROCEDENTE — mesmo recorte e mesmo `agora` histórico também podem divergir.** `q_vivas` exige `validada_em IS NULL` no estado atual (`app/chamados/models.py:3468`), enquanto C6 considera inexistente a validação posterior a `agora` (`app/chamados/juizes.py:1387`). A carga não aplica `q_vivas` (`app/chamados/services/tarefa_do_dia.py:395`). Pergunta sem resposta, disputa aberta e célula acusadora, validada depois do instante consultado, pode estar nas pendentes da carga e ausente de `consulta_do_app(..., agora=agora)` (`app/chamados/services/tarefa_do_dia.py:514`; `app/chamados/juizes.py:1450`; `app/chamados/models.py:3468`).

3. **NÃO PROCEDENTE — predicado local duplicado de partição.** Ambos usam `particao_do_app`: sem resposta e disputa aberta (`app/chamados/services/pergunta_no_app.py:73`; `app/chamados/services/pergunta_no_app.py:81`; `app/chamados/services/tarefa_do_dia.py:514`). Contudo, a partição lê `respondida_em` e `fechada_em` atuais, sem projeção temporal; resposta futura carimbada já manda a pergunta ao histórico em ambos (`app/chamados/services/pergunta_no_app.py:81`). C6 projeta resposta futura para inexistente, criando duas semânticas temporais dentro da composição (`app/chamados/juizes.py:1390`).

4. **PROCEDENTE — contraexemplo sintético reproduzido sem banco e sem arquivos.** Executei corpos reais por AST usando os helpers existentes (`app/chamados/tests/e4_apoio.py:8`; `app/chamados/tests/e3_portas_apoio.py:18`). Hipótese: pergunta 303 de `orfao_14h`, sem resposta/validação/materialização, disputa aberta, célula `furo`, marco `fim` aceso e vencido, sem tranca, fila vazia. C6 retornou viva e a consulta/partição retornou pendente 303; a tarefa publicou `fio='nada', rota=None`, com instância `razao='aceso', pergunta_id=None, acoes=[]`. C6 mantém viva a pergunta sem materialização sobre célula acusadora (`app/chamados/juizes.py:1444`; `app/chamados/juizes.py:1450`); a tarefa pula a lâmpada acesa antes de associar a pergunta (`app/chamados/services/tarefa_do_dia.py:277`; `app/chamados/services/tarefa_do_dia.py:321`); `_portas` exige simultaneamente pendência e instância RESPONDER (`app/chamados/services/tarefa_do_dia.py:610`). Incidência em produção [nao verificado].

   O mesmo bloqueio estrutural pode ocorrer em isento, sem célula/ata, sem trabalho, marco sem luz/teto, ausência ou motivo fora do mapa: esses ramos removem/excluem RESPONDER antes da publicação (`app/chamados/services/tarefa_do_dia.py:203`; `app/chamados/services/tarefa_do_dia.py:219`; `app/chamados/services/tarefa_do_dia.py:277`; `app/chamados/services/tarefa_do_dia.py:287`; `app/chamados/services/tarefa_do_dia.py:298`; `app/chamados/services/tarefa_do_dia.py:333`). Cada classe exige confirmar C6 vivo; não são ocorrências de produção demonstradas [nao verificado].

5. **NÃO PROCEDENTE — cópia do corpo C6 na carga.** A carga chama o juiz registrado com `hoje`, `agora` e carregador, reutilizando resultado para perguntas candidatas já julgadas (`app/chamados/services/tarefa_do_dia.py:518`; `app/chamados/services/tarefa_do_dia.py:521`; `app/chamados/services/tarefa_do_dia.py:523`). C6 encaminha carga ao corpo único (`app/chamados/juizes.py:888`), cuja tranca lê a alimentação da carga (`app/chamados/juizes.py:1405`; `app/chamados/services/carga_pergunta_viva.py:146`).

6. **NÃO PROCEDENTE — C17 ativo como juiz paralelo nos chamadores de produção rastreados.** A busca de chamadas encontrou a definição e somente o instrumento de comparação `diff_e3_sombra`, que o lê como “antes” (`app/chamados/juizes.py:1664`; `app/chamados/management/commands/diff_e3_sombra.py:127`). Seu corpo antigo continua aplicando `cobrar -> colab` (`app/chamados/juizes.py:1650`; `app/chamados/juizes.py:1709`); `_portas` exclui cobrar antes de consultar essa tabela (`app/chamados/services/tarefa_do_dia.py:618`).

7. **NÃO PROCEDENTE — relógio novo interno no caminho tarefa/carga examinado.** O instante da carga segue ao C6 e ao alimentador (`app/chamados/services/tarefa_do_dia.py:519`; `app/chamados/services/tarefa_do_dia.py:521`; `app/chamados/services/tarefa_do_dia.py:565`), e a casca recusa instante nulo ou carga de instante diferente (`app/chamados/services/tarefa_do_dia.py:657`). A API chama a consulta sem instante, conservando C6 atual e seu fallback de calendário (`app/api/views.py:994`; `app/api/views.py:2534`; `app/chamados/juizes.py:592`). Igualdade de relógio com uma carga externa, portanto, não é garantida nesses leitores (`app/api/views.py:2534`).

8. **PROCEDENTE — docstring da carga contradiz implementação.** Promete “uma consulta do app por colab (memo da carga)” (`app/chamados/services/tarefa_do_dia.py:362`), mas implementa uma partição em lote seguida do carregador C6 (`app/chamados/services/tarefa_do_dia.py:514`; `app/chamados/services/tarefa_do_dia.py:518`). A afirmação de igualdade integral com o universo do app também não vale sem as ressalvas acima (`app/chamados/services/tarefa_do_dia.py:512`).

**PERGUNTA:** a conversa pendente do app participa de algum colab-dia? O app admite conversa, mas o juiz de dia dela devolve ausência de dia de turno e a carga exclui conversa (`app/chamados/services/pergunta_no_app.py:52`; `app/chamados/services/pergunta_no_app.py:114`; `app/chamados/services/tarefa_do_dia.py:396`). Não atribuí esse caso a um dia sem corte adicional [nao verificado].

## 14/11 19:19 · sessao 9a633ee7c38b · - · - · origem=Ronald (tmux)

**Pergunta 1 — mapa dos campos e leitores.**

| Valor | Escritor/fornecedor | Leitores de produção |
|---|---|---|
| `fio`, `rota`, `fila`, `verbos` publicados | `_portas_da_tarefa`: computa `verbos`/`fila` em `app/chamados/services/tarefa_do_dia.py:608`, donos em `:615`, fio em `:622`, rota em `:629`, publica todos em `:635`. | Projeção `porta_da_tarefa` em `app/chamados/services/tarefa_do_dia.py:645`; leitores abaixo. |
| `donos` | **Não é campo da tarefa**: conjunto local, criado em `app/chamados/services/tarefa_do_dia.py:615`, acrescido em `:617` e `:621`, consumido para `fio` em `:622`; não aparece no `out.update` de `:635`. | Apenas a seleção de fio do próprio juiz, `app/chamados/services/tarefa_do_dia.py:622`. |
| `porta_da_tarefa` | Copia `rota`, `fila`, `verbos`, `fio`, `nao_sei`, ou retorna `None` quando `rota` é falsa, `app/chamados/services/tarefa_do_dia.py:645`. | `fora_da_folha`, `vigia_fio`, `furo_sem_canal`, `beco_em_lote` e instrumento sombra, endereços abaixo. |
| Pendentes/histórico | `particao_do_app`, `app/chamados/services/pergunta_no_app.py:79`; predicado único sem resposta e disputa aberta, `:81`; `consulta_do_app` fornece vivas em `:73`. | API `app/api/views.py:994`, `:2534`; dashboard `app/core/views.py:253`; aviso `app/chamados/services/aviso_pendencia.py:9`; cobrança `app/chamados/services/cobranca.py:48`; projeção/badge `app/chamados/services/pergunta_no_app.py:100`; carga `app/chamados/services/tarefa_do_dia.py:514`. |
| Vivas do app | `perguntas_do_app` seleciona pelo colaborador e `q_vivas`, `app/chamados/services/pergunta_no_app.py:52`; C6 em `:61`. | Consulta `:73`; C17 `_no_app`, `app/chamados/juizes.py:1642`; ficha, `app/colaboradores/services/ficha.py:166`; diagnóstico `pergunta_no_app.py:208`, `:231`. |

**Leitores diretos da porta, com o que efetivamente usam:**

- Folha `_porta` chama projeção em `app/folha/fora_da_folha.py:70`; `_fio_da_porta` usa `porta['fio']` ou `FIO_SEM_PORTA` em `:85`; o agregado lê esse fio em `:193`; `o_que_segura` lê fio, fila e `nao_sei` em `:238`–`:248`.
- Vigia monta portas em `app/chamados/services/vigia_fio.py:649`, preserva `nao_sei` em `:656`, conta pela rota em `:662`, admin pelo fio em `:663`, publica rota/fio em `:664`.
- Furo lê presença e rota em `app/chamados/services/furo_sem_canal.py:97`–`:99`, preserva desconhecimento em `:100`–`:102`, conta ausências em `:104`, divide rotas em `:114`–`:116`.
- Beco exclui dia com qualquer porta em `app/escala/services/cadastro_realidade.py:469`; não traduz rota/fio.
- Pauta usa indiretamente `_fio_do_dia` e compara com `FIO_SEM_PORTA`, `app/ponto/services/pautas_dos_discordantes.py:79`–`:82`.
- Sombra projeta fio/fila/rota em `app/chamados/management/commands/diff_e3_sombra.py:49`–`:55`, imprime rota/fio/fila/verbos em `:143`–`:144`.
- E4b lê **instâncias e ações**, não os campos publicados, em `app/chamados/management/commands/backfill_fio_morto_e4b.py:97`–`:107`.

**Pergunta 3 — segundo escritor publicado: NÃO PROCEDENTE nos leitores acima.** A publicação fica em `app/chamados/services/tarefa_do_dia.py:635`, e a projeção apenas copia em `:645`; folha lê em `app/folha/fora_da_folha.py:85`, vigia lê em `app/chamados/services/vigia_fio.py:662`, furo lê em `app/chamados/services/furo_sem_canal.py:99`, beco testa presença em `app/escala/services/cadastro_realidade.py:469`.

**C17 ainda existe, mas o chamado de produção encontrado é o instrumento sombra:** `app/chamados/management/commands/diff_e3_sombra.py:127`; sua definição e escrita antigas estão em `app/chamados/juizes.py:1664` e `:1709`. A tarefa reutiliza a autoridade `fio_pelos_verbos` em `app/chamados/services/tarefa_do_dia.py:620`–`:621`, excluindo residual cobrar antes em `:618`; tabela única em `app/chamados/juizes.py:1650`–`:1661`.

**Há outros “donos” derivados, com outra unidade — PROCEDENTE como diferença semântica, não prova de segundo juiz do colab-dia:**

- Ficha deriva dono **do chamado** diretamente do verbo: cobrar → colaborador, `app/colaboradores/services/ficha.py:159`–`:162`.
- Relatório classifica **disputa** como `AGUARDA_COLAB` por `aguarda_o_colaborador`, `app/chamados/relatorios_disputa.py:24`–`:28`; essa função usa C6 e `espera_o_texto_do_colaborador`, `app/chamados/services/fio.py:172`–`:174`; respondida sem texto ainda espera colaborador, `:187`.
- Portanto uma pergunta com `respondida_em` e texto vazio está no histórico da consulta (`app/chamados/services/pergunta_no_app.py:81`–`:84`), mas pode sustentar `AGUARDA_COLAB` no relatório (`app/chamados/services/fio.py:187`, `app/chamados/relatorios_disputa.py:26`). Não chamar esses rótulos de “fio da tarefa”.

**Pergunta 6 — semântica dos leitores.**

- `rota=None` implica `porta_da_tarefa=None`, independentemente de `fio`, por `app/chamados/services/tarefa_do_dia.py:645`; há exceção explícita `fio='sistema', rota=None` em `:634`.
- `porta_app` nasce quando colab tem porta e não há `portas_vivas`, `app/chamados/services/tarefa_do_dia.py:628`–`:634`; por isso não significa necessariamente fila vazia: o cobrar residual pode continuar em `fila`/`verbos`, calculados em `:608`–`:609`.
- `porta_fila`/`porta_pergunta` são escolhidas conforme a composição de portas, `app/chamados/services/tarefa_do_dia.py:632`; não identificam necessariamente a porta particular do dono vencedor: fio é escolhido antes em `:622`.
- Folha não restringe quais strings de rota aceita: só recebe a projeção e lê fio, `app/folha/fora_da_folha.py:70`, `:85`; beco só testa presença, `app/escala/services/cadastro_realidade.py:469`; furo reconhece as três rotas no contador, `app/chamados/services/furo_sem_canal.py:114`–`:116`; vigia conta dinamicamente a chave da rota em `app/chamados/services/vigia_fio.py:662` e inicializa app em `:577`.
- `nada` e `nao_sei` são distintos na tarefa, `app/chamados/services/tarefa_do_dia.py:623`; ambos ficam sem rota, `:629`–`:630`. Folha os colapsa para `FIO_SEM_PORTA='nada'` pela ausência de porta (`app/folha/fora_da_folha.py:85`, `app/chamados/juizes.py:1652`), mas `o_que_segura` preserva a flag `nao_sei` separadamente, `fora_da_folha.py:248`. Vigia e furo também preservam a flag, `vigia_fio.py:656`, `furo_sem_canal.py:100`–`:102`.

**Retrato, templates e PDF — detalhe importante para “sem porta explícito”.**

- Retrato é agregado por pessoa: lê `grupo` do `fora_da_folha` e grava `dono` por `_fio_cru`, `app/folha/retrato_folha.py:1158`–`:1190`; grupo D torna-se `ninguem`, `:177`–`:178`, `:368`. Leitor prefere o gravado, mas tem derivação legada por status/motivo sem grupo, `:428`–`:441`.
- `ninguem` vira **balde Admin**, `app/folha/retrato_folha.py:192`; se todos os dias forem atuais/futuros, pode virar **Hoje**, `:477`–`:481`. A UI rotula esses baldes em `:197`.
- Drawer mostra a frase SEM PORTA por `FIO_DO_CARTORIO` (`app/folha/retrato_folha.py:183`) e `DESTRAVA` (`:1068`–`:1069`; origem `app/folha/fora_da_folha.py:54`); template apenas renderiza os valores em `app/templates/folha/_retrato_o_que_fazer.html:33`–`:40`.
- Lista mostra LED já montado, `app/templates/folha/_retrato_lista.html:47`; PDF mostra o mesmo rótulo do LED em `app/folha/views_retrato.py:411`–`:413`. O PDF **não imprime a frase SEM PORTA** nesse registro: colunas em `:407`–`:418`.
- Vigia de retrato conta `ninguem` pelo gravado, `app/folha/vigia_retido_calado.py:46`–`:49`.

**PERGUNTA de lei:** a exigência nova “sem porta explícito” substitui também o agrupamento legado `ninguem → Admin/Hoje` da barra/PDF, ou admite o balde com identificação no drawer? O código mantém agrupamento em `app/folha/retrato_folha.py:192`, `:477`–`:481`, rótulo Admin em `:197`, PDF pelo LED em `app/folha/views_retrato.py:413`, e SEM PORTA apenas na frase do drawer em `app/folha/retrato_folha.py:1069`.

**Pergunta 7 — comentários que permanecem incompatíveis/incompletos.**

- **PROCEDENTE:** “TODO chamado VIVO na fila ... são publicadas” no cabeçalho, `app/chamados/services/tarefa_do_dia.py:27`–`:29`, e “CHAMADO VIVO NA FILA TEM PORTA”, `:580`; residual cobrar perde dono e rota em `:618`, `:629`–`:630`.
- **PROCEDENTE:** “ações exigíveis e chamados vivos ligados ao dia conservam suas portas”, `app/chamados/services/tarefa_do_dia.py:641`, não ressalva residual cobrar que não publica rota, `:618`, `:629`–`:630`.
- **PROCEDENTE:** `_fio_do_dia` documenta somente admin/colab/nada, `app/folha/fora_da_folha.py:74`, mas retorna qualquer fio publicado, `:85`, inclusive chamado do juiz, `app/chamados/services/tarefa_do_dia.py:622`.
- **PROCEDENTE:** retrato ainda descreve grupo D como “ninguém está esperando ninguém”, `app/folha/retrato_folha.py:361`–`:362`; nova frase diz ausência de ação alcançável, `app/folha/fora_da_folha.py:54`. Há chamado cobrar e ação interna possíveis sem porta, `app/chamados/services/tarefa_do_dia.py:618`–`:635`.
- **PROCEDENTE:** doc do beco diz `None = nenhuma ação ... por porta da fila`, `app/escala/services/cadastro_realidade.py:419`–`:420`; a projeção agora também publica app sem fila, `app/chamados/services/tarefa_do_dia.py:634`, e None também cobre sistema sem rota, `:614`, `:634`.

## 14/11 19:19 · sessao 42afa79ed43b · - · - · origem=Ronald (tmux)

Achados para consolidar:

1. **PROCEDENTE — bateria de mutantes incompleta.** `--todos` aborta no 12º registro, `sem_memo_por_colab`, porque o texto alvo já não existe; `carga_sem_relogio` também está obsoleto. Ambos ainda apontam para um teste removido. A validação do alvo ocorre antes do controle verde, portanto esses dois nem chegam a executar teste. Referências: `app/chamados/tests/e3_degrau_mutantes.py:41`, `:43`, `:87`, `:92`, `:112`; implementação atual em `app/chamados/services/tarefa_do_dia.py:514`. Execução nesta sessão: 19 registros morreram por assertion após controle verde; dois produziram `ValueError`. Resultado sem artefato persistido: [nao verificado em arquivo].

2. **PROCEDENTE — selo L129 não prova leitura efetiva da tarefa.** Ele proíbe imports/nomes e exige a presença dos imports autorizados, mas não exige chamadas nem consumo da resposta. Em mutação somente em memória, substituí todas as chamadas `carga_da_tarefa(...)` e `porta_da_tarefa(...)` por `None`, preservando imports: os três testes do selo passaram. Referências: `app/folha/tests/test_l129_fora_da_folha_so_le_a_tarefa.py:24`, `:57`, `:61`; o degrau apenas executa esse selo em `app/chamados/tests/test_e3_degrau_dono_e_porta.py:293`. Resultado experimental sem artefato persistido: [nao verificado em arquivo].

3. **NÃO PROCEDENTE — vacuidade específica de `assertEqual({}, por_colab)` por ninguém ser chamado.** A mesma execução exige `particao_do_app` exatamente uma vez. O teste de `o_que_segura` exige também um dia produzido. Ainda assim, esses testes medem chamadas; não demonstram equivalência do conjunto ao app nem consumo das pendentes retornadas. Referências: `app/chamados/tests/test_n1_alimentacao_unica.py:110`, `:114`, `:115`, `:161`, `:164`.

4. **NÃO PROCEDENTE — nova asserção de `test_e3_portas_tarefa` ficou vazia.** Exige exatamente `[p.pk]` no C6, uma alimentação do painel, verbos específicos e repetição sem queries; C6 ausente ou adicional nesta fixture falha. Referências: `app/chamados/tests/test_e3_portas_tarefa.py:139`, `:144`, `:146`, `:148`, `:152`.

5. **NÃO PROCEDENTE — `pos_suite` perdeu toda mordida contra C6 redundante.** O contrato ficou mais estreito: pergunta respondida na grade, fora dos pendentes. O mutante registrado que remove a exclusão das perguntas da grade continua produzindo assertion após controle verde. Referências: `app/chamados/tests/test_e3_pos_suite_sem_banco.py:50`, `:52`, `:65`, `:67`; mutante em `app/chamados/tests/e3_pos_suite_sem_banco.py:31`. Resultado experimental sem artefato persistido: [nao verificado em arquivo].

6. **NÃO PROCEDENTE — E4b mascara residual `cobrar` como resolvido.** A fixture mudou para disputa fechada; o teste agora exige `segue_sem_porta`, chamado reaberto, nota única e idempotência. O teste do universo exige dois dias antes e um sem porta depois. Referências: `app/chamados/tests/test_e4_passivo.py:42`, `:76`, `:81`, `:83`, `:86`, `:89`, `:117`, `:122`, `:125`.

7. **PROCEDENTE, alcance limitado — teste `o261e` deixou de demonstrar ausência de consulta paralela pelo vigia.** Antes o mock de app devia receber zero chamadas; agora a partição deve receber chamadas e a saída deve mudar. Isso demonstra influência do insumo sobre tarefa e vigia, mas um vigia com consulta paralela à mesma partição também poderia mudar e satisfazer esse teste. Referências: `app/chamados/tests/test_o261e_f5_alcance.py:264`, `:273`, `:274`, `:278`, `:280`.

8. **Item 3 coberto nos corpos testados.** C4 elimina o dono por `cobrar`; C14 conserva a porta `validar` do admin quando o colab não tem pendência; C15 publica app quando o único residual é `cobrar`. Os respectivos mutantes morreram com controle verde nesta sessão. Referências: `app/chamados/tests/test_e3_degrau_dono_e_porta.py:58`, `:105`, `:113`; implementação em `app/chamados/services/tarefa_do_dia.py:610`, `:618`, `:629`. Equivalência real de universo/C6 não é demonstrada pela matriz: o teste da carga usa C6 mockado e `_perg` já pronto, em `app/chamados/tests/test_e3_degrau_dono_e_porta.py:229`, `:236`, `:246`.

Executados juntos, sem banco: degrau 33, pós-suite 7 e portas-v6 10, total 50 testes verdes, processo rc 0. Referências das suítes: `app/chamados/tests/test_e3_degrau_dono_e_porta.py:40`, `app/chamados/tests/test_e3_pos_suite_sem_banco.py:19`, `app/chamados/tests/test_e3_portas_v6_sem_banco.py:15`; resultado sem artefato persistido: [nao verificado em arquivo]. Os testes ORM n1/portas/E4/o261 foram avaliados por leitura; execução: [nao verificado].

## 14/11 19:04 · sessao 062f239e93bc · BO-DISPUTA-SEM-PERGUNTA · construtor · origem=Code

Cura local implementada para revisão. **18 testes sem banco passaram; RED contra a base: 19 assertions, zero errors; 15 mutantes mortos.** Runner e catálogo: [test_bo_disputa_c25_sem_banco.py](/home/ronald/wt-codex-bo-disputa-sem-pergunta-183316/app/chamados/tests/test_bo_disputa_c25_sem_banco.py:410), [bo_disputa_c25_mutacoes.py](/home/ronald/wt-codex-bo-disputa-sem-pergunta-183316/app/chamados/tests/bo_disputa_c25_mutacoes.py:2). [nao verificado]

**O selo real permanece VERMELHO em models.py**, que ficou intocado. O hunk abaixo zera essas violações quando aplicado somente em memória. Há também uma expectativa contraditória no terceiro teste HX: ele exige aberta com o mesmo classificador silencioso dos dois testes restaurados; permaneceu intocado (`chamados/tests/test_hx_disputa_muda.py:44`, `:64`, `:72`). Suíte ORM e certificação: [nao verificado]. Ruff não executou: módulo indisponível; lint: [nao verificado].

Arquivos tocados por item:

| Item | Arquivos |
|---|---|
| 1 — eleitor/escritor | `chamados/services/fio.py:752`, `:784`; `chamados/services/disputa_emissao.py:97`; `chamados/services/declaracao_texto.py:412` |
| 2 — push | `chamados/services/pergunta_no_app.py:84`; `disputa_emissao.py:1830`; `acoes_disputa.py:110`; `cobranca.py:197`; `validacao.py:641`; `chamados/views.py:1448`; `chamados/views_cobrar.py:23`; `ponto/management/commands/alertar_disputas_pendentes.py:56`; `fio.py:681` |
| 3 — restaurar os dois protegidos | `chamados/tests/test_hx_disputa_muda.py:44`, `:64`: removidas somente as duas chamadas de `_lavrar_ata_do_dia` |
| 4 — autoridade do dia | `chamados/services/disputa_emissao.py:212` |
| RED, mutantes e censos | `chamados/tests/test_bo_disputa_c25_sem_banco.py:89`; `bo_disputa_c25_mutacoes.py:2`; `bo_disputa_c25_censos.py:159` |

O eleitor único considera o fato explícito do intervalo antes do chamado genérico do dia; empate dentro do fato usa menor PK. Pergunta existente conserva seu candidato. `_destino_do_intervalo` e S160 agora delegam a ele (`chamados/services/fio.py:747`, `:752`; `chamados/services/disputa_emissao.py:97`). O escritor recebe o destino decidido, inclusive `None`, sem reeleger (`chamados/services/fio.py:784`).

Na base, S160 filtrava chamados vivos por `motivos_atendidos`, excluía candidato com pergunta não materializada, aceitava comparação de dia parcialmente desconhecida e terminava com `None` na ambiguidade após o desempate por turno/estágio (`0c8adfe5b:app/chamados/services/disputa_emissao.py:167`). Dependiam desse `None` o HIST/c1/c2 do reatamento e o reatamento imediato da pergunta recém-criada (`chamados/reconciliador.py:1559`; `chamados/services/disputa_emissao.py:361`). Agora:

- Ambiguidade do mesmo fato resolve por menor PK; esses caminhos deixam de receber esse caso (`chamados/services/fio.py:773`).
- Zero candidatos continua chegando ao caminho de órfã; candidato sem dia forte não entra na eleição (`chamados/services/fio.py:774`; `chamados/services/disputa_emissao.py:109`).
- Pergunta sem dia e sem chave deixa de obter candidato/data pela existência de um único chamado; continua sem destino derivado (`chamados/services/fio.py:767`; `chamados/reconciliador.py:1574`).
- `ESTAGIOS_TURNO` foi preservado para os outros leitores C8/I4b (`chamados/services/disputa_emissao.py:94`; `chamados/juizes.py:2212`; `chamados/management/commands/auditar_invariantes_chamados.py:698`).

A autoridade do dia é **`fio.dia_do_turno_da_linha`**, já chamada pelo nascimento: episódio passa pelo turno da batida; marco passa pelo dia da célula (`chamados/services/fio.py:1675`; `chamados/models.py:2936`; `ponto/turnos.py:2927`, `:2939`). O contexto fornece somente a chave da consulta. Texto e dedup usam a resposta da autoridade; sem resposta, o motivo é recusado como `InsumoAusente`, com `motivo_sem_dia_do_fato`, sem cascata grade/contexto/batida (`chamados/services/disputa_emissao.py:212`, `:239`).

O caminho genérico de órfãos voltou a ouvir o classificador antes de aplicar a espera por ata. Silêncio fecha a disputa muda; motivo efetivamente solicitado sem ata continua aberto, sem pergunta e sem push, contado em I7b (`chamados/services/disputa_emissao.py:1278`, `:1353`; `chamados/tests/test_bo_disputa_c25_sem_banco.py:332`, `:344`; `chamados/management/commands/auditar_invariantes_chamados.py:74`). Nos anexos fornecidos não há aval literal que autorize estender a espera ao primeiro caminho; a extensão saiu (`chamados/services/disputa_emissao.py:1278`).

Todos os envios de disputa abaixo passam pela lista **`consulta_do_app()['pendentes']`**: C6 `pergunta_viva`, resposta nula e disputa aberta. O callback relê essa lista após o commit; rollback descarta o envio (`chamados/services/pergunta_no_app.py:45`, `:61`, `:84`).

| Sítio de envio | Predicado adicional conservado |
|---|---|
| `chamados/services/disputa_emissao.py:706` | motivos novos e disputa aberta |
| `chamados/services/disputa_emissao.py:1104` | `notificar`, complemento, dona diferente da principal |
| `chamados/services/disputa_emissao.py:1369` | `notificar`, motivos e pendentes da disputa |
| `chamados/services/disputa_emissao.py:1459` | emissão da âncora; gate comum decide o aviso |
| `chamados/services/disputa_emissao.py:1804` | recobrança com pendentes; zero retorna sem cobrar |
| `chamados/services/disputa_emissao.py:1830` | silêncio por ContextVar; WebPush/FCM dentro do callback |
| `ponto/management/commands/alertar_disputas_pendentes.py:56` | carimbo diário; disputa muda nem recebe carimbo |
| `chamados/services/acoes_disputa.py:110` | reabertura de perguntas corrigíveis |
| `chamados/services/acoes_disputa.py:296` | reabertura administrativa autorizada |
| `chamados/views.py:1448` | reabertura pela supervisão |
| `chamados/services/validacao.py:641` | recusa da resposta |
| `chamados/services/cobranca.py:197` | pendências e prazo; driver compartilhado permanece em `:107`, `:112` |
| `chamados/views_cobrar.py:47` | cobrança individual/massa, permissões e throttle dos chamadores |
| `chamados/services/acoes_disputa.py:152`, `:158`; `chamados/services/fio.py:681`, `:688` | avisos de regularização também passam pelo gate literal; sem pendente são suprimidos |

“Colaborador notificado” depende agora de entrega confirmada por algum canal; a quantidade vem das perguntas pendentes, sem usar `len(orfaos)` (`chamados/services/acoes_disputa.py:233`). Agendamento e entrega têm campos separados no recibo (`chamados/services/pergunta_no_app.py:98`).

O censo completo, com **arquivo:linha, função, campo e forma de acesso**, é reproduzível pelo [censo AST](/home/ronald/wt-codex-bo-disputa-sem-pergunta-183316/app/chamados/tests/bo_disputa_c25_censos.py:170):

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B chamados/tests/bo_disputa_c25_censos.py --censo
```

Ele inclui atributos, filtros, argumentos, literais de `values`/`only`/`save` e templates; a classificação do modelo exige ler o sítio indicado (`chamados/tests/bo_disputa_c25_censos.py:170`). Os escritores de interesse depois da mudança são:

| Campo | Escritores |
|---|---|
| `PerguntaDisputa.chamado_destino` | `chamados/services/fio.py:792`, `:794`, `:797`; pendências reais em `chamados/models.py:367`, `:3043`, `:3067`, `:3075` |
| `DS.fechada_em` | `chamados/models.py:1988` (`fechar`), `:2205` (`reabrir_admin`), `:2229` (`reabrir_sistema`) |
| `DS.motivos` | `chamados/models.py:2022`, `:2119`, `:2215`; `chamados/services/disputa_emissao.py:422`; nascimentos em `:533`, `:1175`, `:1439` |
| `PerguntaDisputa.respondida_em` | `chamados/models.py:1555` (simulação), `:1613` (ressuscitação delegada), `:3193`, `:3224`, `:3323`; `chamados/services/disputa_emissao.py:1546`, `:1671`/`:1710`, `:1731`, `:1752` |
| push | drivers dos callbacks inventariados na tabela anterior; porta comum em `chamados/services/pergunta_no_app.py:84` |

Os principais consumidores de `chamado_destino`, incluindo FK, consultas e projeções, ficam nos sítios abaixo; o comando de censo discrimina cada ocorrência por função (`chamados/tests/bo_disputa_c25_censos.py:170`):

```text
api/views.py:1031,2548,2567,2568
chamados/models.py:304,310,311,345,361,362,366,436,443,444,1967,2083,3010,3029,3051,3057
chamados/juizes.py:779,843,857,1860,1987,2034,2346
chamados/reconciliador.py:652,1052,1502
chamados/relatorios_disputa.py:43
chamados/views_cobrar.py:188
chamados/services/acoes_chamado.py:367
chamados/services/acoes_disputa.py:362
chamados/services/becos.py:30,46,47
chamados/services/competencia_trancada.py:121,132,165
chamados/services/declaracao_texto.py:302,365,412,653
chamados/services/disputa_emissao.py:345,350,361,630,631,633,635,867,878,1004,1005,1116,1137,1333,1403,1455,1500,1870,1978,2107
chamados/services/explicador.py:122
chamados/services/fio.py:79,80,116,134,225,226,330,395,396,816,817,819,827,833,860,1079,1106,1262,1263,1264,1269,2113
chamados/services/foto_e4b.py:259
chamados/services/historico.py:96
chamados/services/janela_colab.py:64,65
chamados/services/limbo_folgas.py:205,240,241
chamados/services/materializacao.py:342,533,587,588,717,800,1000,1082,1111,1113,1248,1269,1271
chamados/services/pergunta_no_app.py:240,263,266
chamados/services/raiox.py:57,62,137
chamados/services/relavra_via.py:101,103,156
chamados/services/tarefa_do_dia.py:377
chamados/services/validacao.py:309,341,455,870
chamados/services/vigia_carimbo.py:103
chamados/management/commands/auditar_disputas.py:151
chamados/management/commands/auditar_invariantes_chamados.py:610,616,621,672,673,679,942,978,979,982,1247,1662
chamados/management/commands/backfill_veredito_lavrado.py:101
chamados/management/commands/lavrar_perguntas_stale.py:58,88
chamados/management/commands/migrar_data_evento_dia_do_turno.py:377,472
chamados/management/commands/reabrir_orfas_enterradas.py:69
chamados/management/commands/reatar_beco_folga_ao_limbo.py:87,122,124
chamados/management/commands/relavrar_via_i6.py:56
colaboradores/services/calendario.py:474
ponto/services/veredito_celula.py:187
ponto/management/commands/reconciliar_fantasmas.py:212
ponto/management/commands/reconciliar_grade.py:444
```

Leitores de `DS.fechada_em`:

```text
api/foto_recebida.py:62
api/views.py:1013,1029,2671
chamados/models.py:190,1621,1871,1935,2087,2194,2223,2280,2318,2729
chamados/juizes.py:2396
chamados/reconciliador.py:412,551,600,611,612,1050,1161,1162,1504
chamados/relatorios_disputa.py:41
chamados/signals.py:29,131,155
chamados/views.py:471,495,512,530,911,1329
chamados/views_cobrar.py:184,187
chamados/services/acoes_chamado.py:346
chamados/services/acoes_disputa.py:263,355
chamados/services/becos.py:27,56,59
chamados/services/cobranca.py:238
chamados/services/competencia_trancada.py:176,221
chamados/services/descarte_disputa.py:9
chamados/services/disputa_emissao.py:42,414,468,492,695,705,1013,1103,1145,1368,1412,1600,1980,2075,2099,2101,2160,2162
chamados/services/fila_por_causa.py:225,259,262
chamados/services/fio.py:130,134,600,633
chamados/services/foto_e4b.py:182
chamados/services/materializacao.py:591,646,1322
chamados/services/pergunta_no_app.py:71
chamados/services/regularizacao_ext.py:60
chamados/services/relavra_via.py:183,260
chamados/services/validacao.py:171,237,240,242
chamados/services/vigia_fio.py:753
chamados/management/commands/auditar_chamados_orfaos.py:41
chamados/management/commands/auditar_disputas.py:72,87,136,152,167
chamados/management/commands/auditar_invariantes_chamados.py:63,620,676,890,1851
chamados/management/commands/encerrar_perguntas_com_fato.py:82
chamados/management/commands/fechar_becos_disputa.py:32
chamados/management/commands/reabrir_orfas_enterradas.py:96,131,164,165
colaboradores/painel_cell.py:32
colaboradores/services/desligamento.py:164
colaboradores/services/drawer.py:70
colaboradores/services/situacional.py:117
colaboradores/signals.py:73
escala/signals.py:170
ponto/services/entrada_sincronizada.py:57
ponto/management/commands/alertar_disputas_pendentes.py:35
ponto/management/commands/escalonar_chamados_supervisao.py:58,149
ponto/management/commands/reconciliar_fantasmas.py:214
templates/chamados/partials/modal_fio.html:379
```

Leitores de `PerguntaDisputa.respondida_em`:

```text
api/views.py:1160,1172,2556,2570,2613,2630
api/views_mensageria.py:569,570
chamados/models.py:1900,3106,3109,3188,3192,3409,3559,3985,3992,3993,3994
chamados/juizes.py:348,852,1385,1528,1673,1680,1683
chamados/relatorios_disputa.py:21
chamados/veredito_lavrado.py:207,208
chamados/views.py:1280,1416
chamados/views_cobrar.py:192,199
chamados/services/acoes_disputa.py:96,132
chamados/services/becos.py:30
chamados/services/cobranca.py:237
chamados/services/declaracao_texto.py:406,408,674
chamados/services/disputa_emissao.py:1697,1748,1892,1910,2050,2123,2140
chamados/services/fio.py:339,660
chamados/services/foto_e4b.py:180
chamados/services/furo_sem_canal.py:81
chamados/services/historico.py:99
chamados/services/materializacao.py:675,1376
chamados/services/painel.py:67
chamados/services/passivo_validado.py:66
chamados/services/pergunta_no_app.py:71
chamados/services/raiox.py:138
chamados/services/validacao.py:416,604,609,633,952
chamados/services/verbos.py:335,336,342,345
chamados/services/vigia_fio.py:680,788
chamados/management/commands/auditar_invariantes_chamados.py:261,676,1191,1658,1689
chamados/management/commands/backfill_decisoes_pergunta.py:56,60
chamados/management/commands/curar_perguntas_em_limbo.py:46,69
ponto/management/commands/reconciliar_grade.py:445
templates/chamados/partials/modal_fio.html:185,217,220
templates/chamados/partials/_card_atendimento.html:94,100,137,155,384,385
templates/chamados/partials/_pergunta_card.html:24,29,32
```

Leitores de `DS.motivos`, somente respondendo à fila:

| Leitor | Uso / pressuposto |
|---|---|
| `chamados/models.py:2017`, `:2088`, `:2212` | lista restante, remoção/fecho e acréscimo; pressupõem agregado completo |
| `chamados/services/disputa_emissao.py:400`, `:418`, `:1127`, `:1148`, `:1804` | recorrência, união, complemento, seed e mensagem |
| `chamados/services/descarte_disputa.py:9`, `:10` | fotografia e diferença do agregado fechado |
| `chamados/services/fio.py:680`; `acoes_disputa.py:151` | corpo da notificação |
| `chamados/relatorios_disputa.py:52` | exibição |
| `chamados/management/commands/relatorio_pendencias_disputa.py:39` | exibição dos labels recebidos |
| `chamados/management/commands/auditar_disputas.py:88`, `:142`, `:169`, `:257` | contexto, relatório e comparação com perguntas; `:169` pressupõe todos agregados |
| `chamados/management/commands/auditar_invariantes_chamados.py:50` | universo dos dias; pressupõe todos agregados |
| `ponto/management/commands/alertar_disputas_pendentes.py:74` | log |
| `api/foto_recebida.py:63`, `:70` | descoberta por foto e restantes para aparar/fechar; pressupõe agregado |
| `escala/signals.py:171` | descoberta exige âncora agregada |
| `colaboradores/signals.py:74` | descoberta exige posto agregado |
| `colaboradores/management/commands/pauta_aparelhos_compartilhados.py:49` | descoberta exige device agregado |
| `templates/chamados/partials/modal_fio.html:327` | habilita fluxo da âncora pelo agregado |
| `templates/chamados/detalhe_local.html:301` | exibição |

Portanto, omitir motivo sem pergunta local pode excluir a disputa dos filtros de foto, âncora, posto e aparelhos; esse comportamento ficou na fila (`api/foto_recebida.py:63`; `escala/signals.py:171`; `colaboradores/signals.py:74`; `colaboradores/management/commands/pauta_aparelhos_compartilhados.py:49`).

O selo por função detecta atribuição, `setattr`, constructor/create/update, dict local, `save(update_fields)` e apelidos de import. Hoje acusa os quatro sítios abaixo; os encaminhamentos de `models.py:2846` e `:2858` também estão no hunk, embora sejam passagem de argumento (`chamados/tests/bo_disputa_c25_censos.py:58`):

```text
chamados/models.py:367   ChamadoColaborador._vivas_entre: atributo
chamados/models.py:3043  PerguntaDisputa._nascer: atributo
chamados/models.py:3067  PerguntaDisputa._nascer: save(update_fields)
chamados/models.py:3075  PerguntaDisputa._nascer: create/update
```

Hunk exato para a sessão, com âncoras únicas verificadas pelo gerador; predicados e campos de persistência preservados (`chamados/tests/bo_disputa_c25_censos.py:17`; `chamados/tests/test_bo_disputa_c25_sem_banco.py:398`):

```diff
--- a/app/chamados/models.py
+++ b/app/chamados/models.py
@@ -364,7 +364,8 @@
                 if competencia_do_dia_entregue(_p.data_evento, _p.disputa.colaborador.empresa, _memo) is not False:
                     continue
                 if _p.chamado_destino_id == self.pk:
-                    _p.chamado_destino = self
+                    from chamados.services.fio import gravar_destino_da_pergunta
+                    gravar_destino_da_pergunta(_p, self)
                 if pergunta_viva(_p):
                     _vivas.append(_p)
         finally:
@@ -2842,8 +2843,9 @@
         PROPRIA, `abrir_conversa`. Selo: chamados/tests/test_selo_p13_data_evento_porta_unica.py."""
         if data_evento is None:
             raise ValueError('PerguntaDisputa.abrir exige data_evento (dia do turno); conversa e abrir_conversa')
+        from chamados.services.fio import gravar_destino_da_pergunta
         p, criada, _dia = cls._sob_contrato(disputa=disputa, motivo=motivo, data_evento=data_evento, conversa=False,
-                                            chamado_destino=chamado_destino, **campos)
+                                            **gravar_destino_da_pergunta(campos, chamado_destino))
         return p, criada
 
     @classmethod
@@ -2854,8 +2856,9 @@
         de plantio nao a le como dia do turno; `ancora_ausente` e `escolha` nao plantam por hora do dia).
         Devolve (pergunta, criada)."""
         from django.utils import timezone as _tz_b2
+        from chamados.services.fio import gravar_destino_da_pergunta
         p, criada, _dia = cls._sob_contrato(disputa=disputa, motivo=motivo, data_evento=_tz_b2.localdate(), conversa=True,
-                                            chamado_destino=chamado_destino, **campos)
+                                            **gravar_destino_da_pergunta(campos, chamado_destino))
         return p, criada
 
     @classmethod
@@ -3040,8 +3043,8 @@
                         from chamados.services.fio import _costura_nao_mata_o_fio
                         _ok_f, _mot_f = _costura_nao_mata_o_fio(ex, chamado_destino)
                         if _ok_f:
-                            ex.chamado_destino = chamado_destino
-                            upd.append('chamado_destino')
+                            from chamados.services.fio import gravar_destino_da_pergunta
+                            upd.append(gravar_destino_da_pergunta(ex, chamado_destino))
                         else:
                             from core.observ import engolir as _eng_cr
                             with _eng_cr('chamados.abrir', 'trilha_costura_recusada', pergunta=ex.pk):
@@ -3064,7 +3067,8 @@
                         from chamados.veredito_lavrado import lavrar as _lv_s2
                         _lv_s2(ex, salvar=False)
                         upd += ['veredito_lavrado', 'julgado_em', 'janela_expira_em']
-                        ex.save(update_fields=upd)
+                        from chamados.services.fio import gravar_destino_da_pergunta
+                        gravar_destino_da_pergunta(ex, ex.chamado_destino, salvar=True, campos=upd)
                         cls.acordar_celulas(_linhas_antes_reuso)
                     return ex, False, _dia_fato
             # FIO-E0-V4-E1 (b): `colaborador` e `instancia_marco` da identidade sao derivados no `save` de insercao
@@ -3072,10 +3076,10 @@
             # volta 15d2 (aval E1-VOLTA-15D-UM-PONTO-DE-ESCRITA): o dia do juiz entra pelo UNICO ponto de escrita
             # (`fio.gravar_dia_do_fato`); a insercao e a mesma do `create` (instancia + `save(force_insert=True)`).
             from chamados.services.fio import gravar_dia_do_fato
-            p = cls(disputa=disputa, motivo=motivo, chamad

[truncado em 20000 de 24327 caracteres]

## 14/11 19:00 · sessao 0c7544dcf12d · PORTA-DO-ADMIN-NA-PERGUNTA-VENCIDA · censo · origem=Code

# CENSO — PORTA-DO-ADMIN-NA-PERGUNTA-VENCIDA

**Fechamento global: `[nao verificado]`.** A leitura identificou portas administrativas que corrigem resposta, decidem o dia, renovam cobrança e encerram perguntas; portanto, a validação não é a única intervenção possível. A correção pode partir de `'(sem resposta)'`, enquanto “Resolver dia” pode registrar fatos e encerrar perguntas com decisão de silêncio. (`app/chamados/services/acoes_disputa.py:353`; `app/chamados/services/acoes_disputa.py:371`; `app/ponto/services/veredito_celula.py:190`; `app/ponto/services/veredito_celula.py:323`)

**Alcances reais nos 2.381 dias: `[nao verificado]`.** Não houve execução contra banco. A sonda abaixo distingue guardas chamáveis de corpos que misturam guarda, gravação, sinais e notificações; estes últimos não são executados. (`app/chamados/services/validacao.py:398`; `app/chamados/services/cobrar_dia.py:39`; `app/chamados/services/disputa_emissao.py:1733`)

**Denominador do anexo não reconciliado:** a transcrição conferida por grupos somou **2.357 dias**, diferença de **24** para os 2.381 declarados. Não atribuí colaborador, prazo ou comportamento aos 24 não contabilizados. A contagem não equivale a provar que faltam linhas no original. `[nao verificado]`

Árvore/hash identificado pelo pedido: `3a95114a5e25f0212bff04e861bdc62960d5c042`; correspondência independente entre conteúdo e hash `[nao verificado]`. Nenhum arquivo foi criado, editado ou apagado; este Markdown é a entrega textual para gravação pela sessão. `[nao verificado]`

## (a) Método e inventário das portas

A varredura executou `ast.parse` nos Python de produção sob `app`, inspecionou `Import`, `ImportFrom`, `Call`, funções, classes, registros de admin e declarações de URLs. Foram parseados 765 arquivos, sem erro de sintaxe nesse recorte. Esse resultado não demonstra sozinho fechamento de chamadas dinâmicas, herança externa e todas as ações indiretas da árvore. `[nao verificado]`

O censo legado não foi usado como prova de fechamento: ele exclui `api/` e `admin/` e identifica POST/guardas por texto e regex. (`app/core/portas_da_tela.py:20`; `app/core/portas_da_tela.py:53`; `app/core/portas_da_tela.py:58`)

### Apelidos de import resolvidos

| Apelido | Destino | Evidência |
|---|---|---|
| `vreg` | `chamados.views_regularizacao` | `app/chamados/urls.py:6` |
| `_ac` | `chamados.services.acoes_chamado` | `app/chamados/views.py:423` |
| `_ad` | `chamados.services.acoes_disputa` | `app/chamados/services/fio.py:665` |
| `_ac_geo` | `chamados.services.acoes_chamado` | `app/chamados/services/fio.py:572` |
| `_corrig` | `chamados.models.perguntas_corrigiveis` | `app/chamados/views.py:1419` |
| `_reabrir2` | `chamados.models.reabrir_para_correcao` | `app/chamados/views.py:1435` |
| `_muda_a_folha` | `chamados.services.cobranca_folha.muda_a_folha` | `app/chamados/views_cobrar.py:88` |
| `_vc` | `ponto.services.veredito_celula.veredito_celula` | `app/ponto/views.py:3354` |

### Autoridade e significado de “sem resposta”

`is_staff` permite entrar no fio como gestor, mas várias ações exigem também permissão nominal e autoridade sobre o departamento do chamado; staff, permissão de ação e autoridade contextual são guardas distintas. (`app/chamados/views.py:35`; `app/colaboradores/mixins.py:322`; `app/chamados/utils.py:68`)

O universo de pergunta devida não pode ser convertido em `respondida_em IS NULL`: a tarefa mantém `responder/colaborador` quando a última decisão de resposta tem texto vazio. Já a validação examina o carimbo `respondida_em`. Logo, **validar = 0 nos 2.381** não está demonstrado pelo anexo. (`app/chamados/services/tarefa_do_dia.py:145`; `app/chamados/services/validacao.py:418`)

Os três relógios também não são equivalentes: C9 compara `prazo_sla`; o prazo de cobrança admite derivação pelo nascimento; `sla.vencido` usa referência de followup/nascimento e urgência. (`app/chamados/catalogo/motor.py:54`; `app/chamados/catalogo/motor.py:59`; `app/chamados/services/sla.py:17`; `app/chamados/services/sla.py:33`)

### Pergunta, disputa, fio e painel

As rotas abaixo partem de `/chamados/`, prefixo publicado no projeto. Alcance quantitativo de cada linha: `[nao verificado]`. (`app/config/urls.py:28`)

| Ação e porta | Guarda que decide | O que grava / trilha |
|---|---|---|
| **Validar pergunta** — detalhe `local/<id>/`; fio `colaborador/<id>/`; modal `modal/<id>/` | Contexto por chamado; pergunta deve estar listada. Serviço exige resposta carimbada, não validada e confirmação quando implausível. Detalhe e fio diferem quanto à disputa fechada. (`app/chamados/services/fio.py:604`; `:636`; `app/chamados/views.py:491`; `app/chamados/services/validacao.py:418`; `:429`) | `PerguntaDisputa.validada_em/validada_por`; `DecisaoPergunta.VALIDACAO`; materialização; logs. A recusa da materialização pode desfazer a validação. (`app/chamados/services/validacao.py:159`; `:164`; `:488`; `:489`; `:504`) |
| **Recusar pergunta** — mesmas telas | Motivo; não validada; respondida; não fora do fato. No fio, pergunta de disputa aberta e listada pelo chamado. (`app/chamados/services/validacao.py:591`; `:602`; `:605`; `:609`; `app/chamados/services/fio.py:671`) | `recusa_motivo`; reabertura com decisão `recusa`; nota, evento, log e push. (`app/chamados/services/validacao.py:619`; `:628`; `:633`; `:637`; `:643`; `:647`) |
| **Corrigir resposta** — mesmas telas | HH:MM, motivo, pergunta existente, não validada, disputa aberta e tipo `hora`. **Não exige resposta anterior.** (`app/chamados/services/acoes_disputa.py:336`; `:345`; `:347`; `:349`; `:353`) | Nota com conteúdo anterior; core escreve `resposta_colab/respondida_em` e decisão `RESPOSTA`; log `corrigir_resposta_admin`; depois valida/materializa. Pode ficar resposta corrigida com materialização recusada. (`app/chamados/services/acoes_disputa.py:359`; `:371`; `:375`; `:379`; `:382`; `app/chamados/services/disputa_emissao.py:1508`; `:1511`) |
| **Regularizar disputa — detalhe** | Resolução e nenhuma pergunta viva sem carimbo de resposta; guarda dentro da ação. (`app/chamados/services/acoes_disputa.py:124`; `:130`) | Materialização granular; fechamento conforme resultado; log. (`app/chamados/services/acoes_disputa.py:139`; `:144`) |
| **Regularizar disputa — fio/modal** | Possui guarda própria inline: resolução e nenhuma viva sem resposta. Não delega a guarda do detalhe. (`app/chamados/services/fio.py:684`; `:686`; `:698`) | Materialização granular; log; notificações. (`app/chamados/services/fio.py:708`; `:713`; `:719`) |
| **Reabrir disputa — fio/modal** | Usa `perguntas_corrigiveis`; sem corrigíveis recusa. Este ramo não executa a alternativa de recobrança do endpoint específico. (`app/chamados/services/acoes_disputa.py:95`; `:97`; `:98`) | `reabrir_para_correcao`; nota, push e log. (`app/chamados/services/acoes_disputa.py:103`; `:106`; `:108`; `:113`) |
| **Reabrir ou recobrar** — `local/<id>/reabrir-disputa/` | Gate condicional: só consulta `pode_agir_chamado` se não tiver `gerir_chamados`. Resolve disputa, corrigíveis e pendentes vivas dentro da view. (`app/chamados/urls.py:20`; `app/chamados/views.py:1404`; `:1410`; `:1417`; `:1420`) | Corrigíveis: reabre perguntas, nota e log. Sem corrigíveis, com pendente viva: recobra. (`app/chamados/views.py:1425`; `:1436`; `:1440`; `:1456`) |
| **Recobrança efetiva** — ramo anterior | Seleção fica inline na view; serviço não é guarda pura. (`app/chamados/views.py:1421`; `app/chamados/services/disputa_emissao.py:1697`) | Renova `ChamadoColaborador.prazo_sla`; nota, push e log `recobrar_questionario`. (`app/chamados/services/disputa_emissao.py:1725`; `:1728`; `:1735`; `:1741`) |
| **Reabertura administrativa** — fio/modal | Staff/admin_ti; motivo; justificativa mínima; disputa fechada. (`app/chamados/services/acoes_disputa.py:245`; `:251`; `:253`; `:255`) | Reabre disputa; cria pergunta de conversa; reabre chamado; nota, pushes e log. (`app/chamados/services/acoes_disputa.py:259`; `:263`; `:270`; `:274`; `:297`) |
| **Conferir data** — `disputa/pergunta/<id>/conferir-data/` | POST e `gerir_chamados`; serviço exige pergunta, justificativa, divergência pelo juiz e ausência de conferência anterior. (`app/chamados/urls.py:18`; `app/chamados/views.py:1307`; `:1309`; `app/chamados/services/validacao.py:686`; `:690`; `:693`; `:697`) | `data_conferida_em/por`; log com antes/depois e justificativa. (`app/chamados/services/validacao.py:699`; `:701`; `:702`) |
| **Aplicar declaração de texto — detalhe** | Consulta propostas pelo followup; guardas adicionais no aplicador. (`app/chamados/views.py:478`; `app/chamados/services/declaracao_texto.py:649`; `:668`) | Emite/reusa disputa/pergunta; responde pelo core; valida pelo serviço canônico. (`app/chamados/services/declaracao_texto.py:659`; `:666`; `:693`; `:696`) |
| **Cobrar por push** — `cobrar/<id>/` | `gerir_chamados`, POST, departamento, throttle por colaborador/dia; envio precisa ter sucesso. (`app/chamados/urls.py:25`; `app/chamados/views_cobrar.py:25`; `:31`; `:36`; `:51`) | Push e nota `[COBRANCA]`; esta porta não chama o renovador de prazo. (`app/chamados/views_cobrar.py:44`; `:55`) |
| **Cobrar em massa** — `cobrar/massa/` | Fila/verbo, contexto, `muda_a_folha`, throttle; `previa=1` retorna a prévia antes do envio. (`app/chamados/urls.py:31`; `app/chamados/views_cobrar.py:78`; `:92`; `:95`; `:102`; `:117`) | Push e nota por colaborador alcançado. (`app/chamados/views_cobrar.py:123`; `:132`) |
| **Cobrar colaborador pelo copiloto** — `/ia/perguntar/` | `tem_acao` e guarda real `checar_cobranca`: pergunta pendente no app e ausência de prazo futuro entre os pais. (`app/core/urls.py:15`; `app/core/views_ia.py:841`; `app/chamados/services/cobranca.py:75`; `:88`) | Prazo nos pais, notas, push e log `cobrar_colaborador`. (`app/chamados/services/cobranca.py:178`; `:181`; `:187`; `:188`) |
| **Cobrar dia** — `cobrar/dia/` | POST, `regularizar_batida`, departamento/data; serviço recusa dia ainda não terminado. Guardas posteriores estão misturadas à execução. (`app/chamados/urls.py:32`; `app/chamados/views_cobrar.py:314`; `:317`; `app/chamados/services/cobrar_dia.py:28`) | **Log antes do resultado final**; cartório com `toque=True`; possível emissão de disputa/perguntas. (`app/chamados/services/cobrar_dia.py:39`; `:41`; `:49`) |
| **Validar inline** — `validar-inline/<id>/` | Permissão, área, disputa e seleção de perguntas vivas respondidas. (`app/chamados/urls.py:33`; `app/chamados/views_cobrar.py:149`; `:158`; `:162`; `:169`) | Delega à validação canônica. (`app/chamados/views_cobrar.py:208`) |
| **Validar lote JSON/tela** — `validar-lote/`, `validar-lote/tela/` | Permissão/contexto, pergunta elegível e classe real do lote; guarda integral não isolada. (`app/chamados/urls.py:34`; `:37`; `app/chamados/views_cobrar.py:249`; `app/chamados/services/validacao.py:767`; `:770`) | Validação canônica; tela acrescenta log do lote. (`app/chamados/services/validacao.py:781`; `app/chamados/views_cobrar.py:383`) |
| **Arquivar individual** — `arquivar/<id>/` | Permissão, POST e área; endpoint individual não exige idade. (`app/chamados/urls.py:26`; `app/chamados/views_arquivar.py:26`; `:28`; `:32`) | `silenciado_ate/por/motivo/atualizado_em`, evento e nota. (`app/chamados/services/arquivo.py:166`; `:171`; `:173`; `app/chamados/views_arquivar.py:44`) |
| **Arquivar lote** — `arquivar/lote/` | Corte positivo; filtro, verbo, idade, silêncio e contexto. (`app/chamados/urls.py:28`; `app/chamados/views_arquivar.py:105`; `:125`; `:135`; `:137`) | Mesmo escritor do véu; evento por chamado. (`app/chamados/views_arquivar.py:140`; `app/chamados/services/arquivo.py:173`) |
| **Desarquivar** — `desarquivar/<id>/` | Permissão/POST/área; via de tranca tenta reabertura que pode recusar. (`app/chamados/urls.py:27`; `app/chamados/views_arquivar.py:54`; `:60`; `:70`) | Limpa véu, carimba `resgatado_em`; pode ressuscitar perguntas; nota. (`app/chamados/services/arquivo.py:202`; `:206`; `app/chamados/views_arquivar.py:86`; `:88`) |
| **Assumir/liberar responsável** — `local/<id>/atribuir/` | Gestor e contexto do chamado. (`app/chamados/urls.py:41`; `app/chamados/views.py:1010`; `:1015`) | `atribuido_a/em/atualizado_em`; nota e log quando muda. (`app/chamados/services/vinculos.py:132`; `app/chamados/views.py:1027`; `:1034`) |
| **Mensagem rápida** — `colaborador/<id>/mensagem-rapida/` | Permissão e mensagem não vazia. (`app/chamados/urls.py:40`; `app/chamados/views.py:967`; `:971`) | Abre chamado `mensagem_dp`, nota e push. (`app/chamados/views.py:973`; `:986`; `:989`) |
| **Resposta livre/status** — detalhe/fio | Contexto; conteúdo/status; transição tem guardas posteriores próprias. (`app/chamados/services/fio.py:747`; `:752`; `app/chamados/models.py:128`) | Nota pode preceder transição recusada; transição e pauta pelo executor; logs. (`app/chamados/services/fio.py:756`; `:760`; `app/chamados/services/acoes_chamado.py:304`; `:328`) |
| **Abrir modal / marcar lidas** | Gestor. **GET tem escrita.** (`app/chamados/views.py:872`; `:877`) | `FollowupChamado.lido_dp_em`, via `QuerySet.update`. (`app/chamados/views.py:880`; `app/chamados/services/nota.py:126`; `:132`) |
| **Regularização externa: criar** | Gestor/departamento e motivo. (`app/chamados/urls.py:49`; `app/chamados/views_regularizacao.py:19`; `:49`) | `RegularizacaoExterna`, chamado de rastro e vínculo. (`app/chamados/services/regularizacao_ext.py:237`; `:247`; `:257`; `:310`) |
| **Regularização externa: adicionar item** | Estado do lote, data/horários, alvo pertencente ao colaborador e consistência dos horários. (`app/chamados/urls.py:51`; `app/chamados/views_regularizacao.py:80`; `:87`; `:109`; `:120`) | `RegularizacaoItemBatida`: data, horas, ação, alvo e intervalo. (`app/chamados/services/regularizacao_ext.py:326`) |
| **Regularização externa: excluir item** | Estado do lote. (`app/chamados/urls.py:52`; `app/chamados/views_regularizacao.py:143`) | Exclui o item. (`app/chamados/services/regularizacao_ext.py:336`) |
| **Regularização externa: enviar** | Estado e existência de itens. (`app/chamados/urls.py:53`; `app/chamados/views_regularizacao.py:160`; `:163`) | Estado `aguardando` e notificações. A materialização exige confirmação e recusa qualquer disputa aberta do colaborador. (`app/chamados/services/regularizacao_ext.py:343`; `app/chamados/views_regularizacao.py:170`; `app/chamados/services/regularizacao_ext.py:52`; `:59`) |

### Outros verbos dos roteadores

São ramos reais, e sua aplicabilidade aos dias do universo depende de dados não presentes no anexo. Alcances: `[nao verificado]`.

| Ação | Porta / guarda | Gravação / trilha |
|---|---|---|
| `aprovar_excecao` | Detalhe; `_pode_aprovar`; serviço recusa encerrado, sem exigir categoria nesse ponto. (`app/chamados/views.py:426`; `app/chamados/services/acoes_chamado.py:25`) | Batida, foto, nota, transição e log. (`app/chamados/services/acoes_chamado.py:44`; `:67`; `:69`; `:71`; `:77`) |
| `autorizar_geofence` / `_advertir` | Detalhe e fio; contexto e estado. (`app/chamados/views.py:537`; `app/chamados/services/fio.py:571`; `app/chamados/services/acoes_chamado.py:182`) | Pode criar `Advertencia`; nota, transição e log. (`app/chamados/services/acoes_chamado.py:199`; `:224`; `:229`; `:230`) |
| `recusar_geofence` | Estado, motivo e batida vinculada ao colaborador. (`app/chamados/services/acoes_chamado.py:260`; `:263`; `:265`) | Retratação da batida; nota, transição e log. (`app/chamados/services/acoes_chamado.py:277`; `:281`; `:286`; `:287`) |
| `aceitar_folga_limbo` | Sugestão e template existentes. (`app/chamados/views.py:432`; `app/chamados/services/limbo_folgas.py:85`; `:88`) | Altera folgas de `TipoEscala`; log e reconciliação. (`app/chamados/services/limbo_folgas.py:92`; `:95`; `:99`) |
| `corroborar_regua` | Vínculo existente e ainda não corroborado. (`app/chamados/views.py:441`; `app/escala/regua_defesa.py:655`; `:657`) | Régua, confiança e carimbos de corroboração; nota. (`app/escala/regua_defesa.py:662`; `:666`; `:672`) |
| `realocar_regua` | Destino/template, restrição de ciclo e `julgar_vinculo`. (`app/chamados/views.py:452`; `app/escala/regua_defesa.py:305`; `:310`; `:316`) | `executar_vinculo`; nota. (`app/escala/regua_defesa.py:321`; `:333`) |
| `pedir_confirmacao_ancora` | Categoria, área e resultado da fábrica. (`app/chamados/services/acoes_disputa.py:185`; `:187`; `:190`) | Cria disputa de âncora; log. (`app/chamados/services/acoes_disputa.py:189`; `:192`) |
| `iniciar_disputa_orfaos` | Guarda real `chamado_orfao_e_regularizavel`; recusa sem alvos ou repasse recusado. (`app/chamados/services/acoes_disputa.py:219`; `:220`; `:225`) | Fábrica de disputa/perguntas; log. (`app/chamados/services/acoes_disputa.py:222`; `:229`) |
| `aplicar_proposta_escala` | Gate do registro de intenções, assinatura/vínculo recalculados e restrições da proposta. (`app/chamados/services/proposta_no_fio.py:136`; `:138`; `:141`) | Despacha ao escritor `escala.services.propositor.aplicar`; campos finais completos `[nao verificado]`. (`app/chamados/services/proposta_no_fio.py:151`; `app/core/intake.py:51`) |
| `recusar_proposta_escala` | Gate, motivo e proposta recalculada. (`app/chamados/services/proposta_no_fio.py:168`; `:170`; `:173`) | `propositor.recusar`; campos finais completos `[nao verificado]`. (`app/chamados/services/proposta_no_fio.py:176`) |

### Formulários anunciados sem ramo correspondente

| Form do modal | Resultado do rastreio |
|---|---|
| `aplicar_declaracao_texto` | Form em `app/templates/chamados/partials/modal_fio.html:140`; handler encontrado no detalhe, `app/chamados/views.py:478`, mas não no roteador do fio. |
| `confirmar_correcao_pergunta` | Form em `app/templates/chamados/partials/modal_fio.html:190`; handler de produção correspondente `[nao verificado]`. |
| `apagar_pergunta_cega` | Form em `app/templates/chamados/partials/modal_fio.html:197`; handler de produção correspondente `[nao verificado]`. |

O POST desses forms, sem conteúdo que acione o ramo de mensagem, chega ao retorno “Nada a fazer” do roteador; não foi contado como operação funcional sobre a pergunta. (`app/chamados/services/fio.py:746`; `:781`)

### Portas sobre o dia fora de chamados

| Ação / URL | Guarda | Gravação / trilha |
|---|---|---|
| **Resolver dia:** `/ponto/celula/veredito/` — trabalhou/faltou/justificado | POST e `regularizar_batida`; guardas de negócio inline no serviço. (`app/ponto/urls.py:58`; `app/ponto/views.py:3355`; `app/ponto/services/veredito_celula.py:221`; `:243`; `:249`; `:309`; `:334`) | Batidas ou ausência aprovada; `fechar_por_admin`; decisão `SILENCIO`; chamados; lavra da célula; log. (`app/ponto/services/veredito_celula.py:323`; `:337`; `:190`; `:195`; `:203`; `:352`; `:356`) |
| **Declarar folga/falta pelo copiloto** — `/ia/perguntar/` | Gates diferentes: `editar_escalas`/`regularizar_batida`; juiz real `checar_declaracao`. (`app/core/intake.py:76`; `:80`; `:90`; `:94`; `app/ponto/services/declarar_dia.py:126`) | Folga pelo escritor de folgas e fechamento do fio; falta pelo veredito da célula. A escrita pode recusar depois da prévia. (`app/ponto/services/declarar_dia.py:167`; `:169`; `:191`; `:206`; `:215`) |
| **Inverter batida** — `/ponto/batida/<id>/flip/` | Retratação, autoria, competência e tipo dentro do executor. (`app/ponto/urls.py:59`; `app/ponto/services/flip_batida.py:15`; `:19`; `:22`; `:26`) | `Batida.tipo`, trilha e recomputação. (`app/ponto/services/flip_batida.py:30`; `:39`) |
| **Retratar batida** — `/ponto/batida/<id>/retratar/` | `_pode` chamável; motivo e estado inline. (`app/ponto/urls.py:61`; `app/ponto/services/retratar_humano.py:131`; `:166`; `:170`) | Retratação e motivo estruturado pela porta canônica. (`app/ponto/services/retratar_humano.py:179`; `:181`) |
| **Desfazer retratação** — `/ponto/batida/<id>/desfazer-retratacao/` | Autoria/permissão, estado, origem e janela; `pode_desfazer` sozinho não cobre tudo. (`app/ponto/urls.py:62`; `app/ponto/services/retratar_humano.py:194`; `:197`; `:199`; `:203`) | Aprovação e limpeza da retratação, com trilha.

[truncado em 20000 de 49883 caracteres]

## 14/11 18:52 · sessao c3a4441e6bc6 · - · - · origem=Ronald (tmux)

A transcrição reconciliada soma **2.357 dias de 300 colabs**, diferença de **24** para os 2.381 declarados; não ajustei contagens para fechar o universo. `[nao verificado]`

| Emp | 09/2026 | 10/2026 | 11/2026 |
|---|---:|---:|---:|
| 1 | 0 | 12 | 5 |
| 2 | 545 | 615 | 790 |
| 3 | 94 | 117 | 156 |
| 4 | 6 | 8 | 9 |

Fonte: anexo textual transcrito; conferência independente dos grupos pelos outros explorers. `[nao verificado]`

Top 10, desempate pelo menor colab: **534:77; 607:72; 667:61; 717:54; 317:45; 832:40; 650:39; 114:38; 144:38; 915:38**. Soma: **502 dias**. `[nao verificado]`

No corpus de 2.357 dias:

- **37 colabs → 1.186 dias (50,32%)**; os primeiros 36 somam 1.168. `[nao verificado]`
- **108 colabs → 1.888 dias (80,10%)**; os primeiros 107 somam 1.882. `[nao verificado]`

Para os limiares do universo declarado de 2.381, usando somente os dias transcritos:

- **38 colabs → 1.203 dias**, ultrapassando 1.191; os primeiros 37 somam 1.186. `[nao verificado]`
- **111 colabs → 1.906 dias**, ultrapassando 1.905; os primeiros 110 somam 1.900. `[nao verificado]`

Esses limiares não certificam concentração do universo completo, pois faltam reconciliar os 24 dias. `[nao verificado]`

Assumindo prazos em UTC e idade em dias completos, a distribuição dos 2.357 dias é: `[nao verificado]`

| Faixa | Dias |
|---|---:|
| <0 | 0 |
| 0–7 | 33 |
| 8–15 | 111 |
| 16–30 | 99 |
| >30 | 2.082 |
| Prazo `None` | 32 |
| Total | 2.357 |

O anexo removeu o offset dos prazos. Interpretá-los em `-03:00` transfere os três dias de col988, prazo `2026-10-14T17:30`, de `>30` para `16–30`: essas faixas passam a **2.079** e **102**, respectivamente. `[nao verificado]`

## 14/11 18:51 · sessao 32d3120f3ba2 · - · - · origem=Ronald (tmux)

**Emp2 / 09/2026: 545 linhas, 123 colabs**, após cruzamento dos mapas por colaborador. Fonte: TSV colado; arquivo:linha `[nao verificado]`.

A diferença para os 544 da outra agente é **col836 / 2026-09-19**, uma linha omitida do mapa dela. Col832 tem 12 linhas; col667 tem 6. Fonte: anexo colado; arquivo:linha `[nao verificado]`.

Maiores: col534=31; col788=21; col607=18; col312=15; col650=15; col209=14; col832=12; col883=11; col346/362/599/820/877=10 cada. Fonte: anexo colado; arquivo:linha `[nao verificado]`.

Não ajustei a soma ao universo declarado de 2381. As demais fatias não foram recontadas nesta conferência `[nao verificado]`.

## 14/11 18:50 · sessao 8dc621e17fdc · GRADE-RESOLVE · censo · origem=Code

(A) `resolve.tsv`

```tsv
caso_fonte	caso_id	obra_id	estado_obra	prova_arquivo:linha	lateral_cen	lateral_lei	toca_congelado
HJ-INGESTAO-0511	HJ-005-U1	candidata sem prova: BO-ADMIN-2710	SEM OBRA	app/docs/BACKLOG.md:820;app/docs/GRADE-MESTRA.md:137;cobertura individual [nao verificado]	—	—	—
HJ-INGESTAO-0511	HJ-005-U2	COBERTURA-PARCIAL-UM-JUIZ	NO AR	app/docs/BACKLOG.md:829;app/docs/BACKLOG.md:993;app/docs/RELATO.md:8127	—	—	—
HJ-INGESTAO-0511	HJ-005-U4	COBERTURA-PARCIAL-UM-JUIZ	NO AR	app/docs/BACKLOG.md:829;app/docs/BACKLOG.md:993;app/docs/RELATO.md:8127	—	—	—
HJ-INGESTAO-0511	HJ-012-U2	nenhuma obra registrada	SEM OBRA	app/docs/RELATO.md:8390;cobertura [nao verificado]	—	—	—
HJ-INGESTAO-0511	HJ-017-U1	O232	LATERAL-ESTRUTURA	app/docs/BACKLOG.md:1239;app/docs/GRADE-MESTRA.md:853	sim	sim	não, nas linhas citadas
HJ-INGESTAO-0511	HJ-030-U1	FIO-LE-O-JUIZ	NO AR	app/docs/BACKLOG.md:2098;app/docs/BACKLOG.md:837	—	—	—
BO2910-ESTRUTURA	01	BO2910-ESTRUTURA	LATERAL-ESTRUTURA	app/docs/BACKLOG.md:1102;app/docs/GRADE-MESTRA.md:170	não	sim	não, nas linhas citadas
BO2910-ESTRUTURA	02	BO2910-ESTRUTURA	LATERAL-ESTRUTURA	app/docs/BACKLOG.md:1102;app/docs/GRADE-MESTRA.md:170	não	sim	não, nas linhas citadas
BO2910-ESTRUTURA	05	BO2910-ESTRUTURA	LATERAL-ESTRUTURA	app/docs/BACKLOG.md:1102;app/docs/GRADE-MESTRA.md:170	não	sim	não, nas linhas citadas
BO2910-ESTRUTURA	06	BO2910-ESTRUTURA	LATERAL-ESTRUTURA	app/docs/BACKLOG.md:1102;app/docs/GRADE-MESTRA.md:170	não	sim	não, nas linhas citadas
BO2910-ESTRUTURA	08	BO2910-ESTRUTURA	LATERAL-ESTRUTURA	app/docs/BACKLOG.md:1102;app/docs/GRADE-MESTRA.md:170	não	sim	não, nas linhas citadas
BO-ADMIN-2710	1	COBERTURA-PARCIAL-UM-JUIZ	NO AR	app/docs/BACKLOG.md:829;app/docs/BACKLOG.md:993;app/docs/RELATO.md:8127	—	—	—
BO-ADMIN-2710	2	JUIZ-DO-PAR-DUAS-ENTRADAS	NO AR	app/docs/GRADE-MESTRA.md:38;app/docs/BACKLOG.md:819	—	—	—
BO-ADMIN-2710	3	JUSTIFICATIVA-PAGA-O-PERIODO	ESPINHA A FAZER	app/docs/BACKLOG.md:1918;app/docs/BACKLOG.md:9;app/docs/GRADE-MESTRA.md:269	sim	sim	não, nas linhas citadas
BO-ADMIN-2710	5	JUIZ-DO-PAR-DUAS-ENTRADAS	NO AR	app/docs/BACKLOG.md:819	—	—	—
BO-ADMIN-2710	6	FIO-LE-O-JUIZ	NO AR	app/docs/BACKLOG.md:837	—	—	—
BO-ADMIN-2710	7	REGISTRADO-NAO-E-VIVO	NO AR	app/docs/BACKLOG.md:823;app/docs/RELATO.md:3371;app/docs/GRADE-MESTRA.md:139	não	sim	não, nas linhas citadas
BO-ADMIN-2710	8.1	TRILHA-DO-VINCULO-MENTE	NO AR	app/docs/BACKLOG.md:821;app/docs/GRADE-MESTRA.md:138	sim	sim	não, nas linhas citadas
```

Cen/lei seguem as duas primeiras casas da grade: `+` feito, `.` falta. “Toca congelado” considera somente os arquivos citados nas linhas das obras. app/docs/GRADE-MESTRA.md:7; app/docs/BACKLOG.md:1102; app/docs/BACKLOG.md:1239; app/docs/BACKLOG.md:1918; app/docs/BACKLOG.md:823; app/docs/BACKLOG.md:821.

Ressalvas do cruzamento:

- **HJ-005-U1:** BO-ADMIN-2710 registra medição e encaminhamento; seu `fecha BO=3` agregado não individualiza cobertura de 01/10. A COBPARC nomeia col42 em 08/15/22-10. Por isso U1 permanece candidata sem prova, contada como SEM OBRA. app/docs/BACKLOG.md:820; app/docs/GRADE-MESTRA.md:137; app/docs/BACKLOG.md:829.
- **HJ-017-U1:** O232 combina a indicação do mapa fornecido com o caso nominal col923 e `fecha BO=1` do mapa HJ. O dia 27/09 não aparece na linha da obra: cobertura específica dessa data [nao verificado]. O complemento da ata permanece pendente; não foi contado como NO AR pelos pousos A/B. app/docs/BACKLOG.md:1239; app/docs/GRADE-MESTRA.md:853.
- **Estados divergentes:** a COBPARC tem fechamento posterior ao PARADA da grade; JUSTIFICATIVA-PAGA-O-PERIODO voltou à ordem viva e recebe o item 3, expressamente separado da obra A. app/docs/GRADE-MESTRA.md:39; app/docs/RELATO.md:8127; app/docs/BACKLOG.md:993; app/docs/BACKLOG.md:1918; app/docs/BACKLOG.md:9.
- **BO-ADMIN/2:** a prova nas fontes autorizadas é pela classe de divergência entre motor e ata; o encaminhamento nominal do caso está registrado no relato. app/docs/GRADE-MESTRA.md:38; app/docs/BACKLOG.md:819; app/docs/RELATO.md:3423.

(B) Coluna `resolve` por obra

| obra_id | resolve | Prova |
|---|---|---|
| COBERTURA-PARCIAL-UM-JUIZ | HJ-005-U2; HJ-005-U4; BO-ADMIN-2710/1 | app/docs/BACKLOG.md:829 |
| JUIZ-DO-PAR-DUAS-ENTRADAS | BO-ADMIN-2710/2; BO-ADMIN-2710/5 | app/docs/GRADE-MESTRA.md:38; app/docs/BACKLOG.md:819 |
| FIO-LE-O-JUIZ | HJ-030-U1; BO-ADMIN-2710/6 | app/docs/BACKLOG.md:2098; app/docs/BACKLOG.md:837 |
| REGISTRADO-NAO-E-VIVO | BO-ADMIN-2710/7 | app/docs/BACKLOG.md:823 |
| TRILHA-DO-VINCULO-MENTE | BO-ADMIN-2710/8.1 | app/docs/BACKLOG.md:821 |
| JUSTIFICATIVA-PAGA-O-PERIODO | BO-ADMIN-2710/3 | app/docs/BACKLOG.md:1918 |
| O232 | HJ-017-U1 | app/docs/BACKLOG.md:1239; app/docs/GRADE-MESTRA.md:853 |
| BO2910-ESTRUTURA | BO2910-ESTRUTURA/01; /02; /05; /06; /08 | app/docs/BACKLOG.md:1102; app/docs/GRADE-MESTRA.md:170 |

(C) QUADRO

| Parcela | Casos | Lastro |
|---|---:|---|
| NO AR | 9 | app/docs/BACKLOG.md:829; app/docs/BACKLOG.md:993; app/docs/BACKLOG.md:819; app/docs/BACKLOG.md:837; app/docs/BACKLOG.md:2098; app/docs/BACKLOG.md:823; app/docs/RELATO.md:3371; app/docs/BACKLOG.md:821 |
| EM VOO | 0 | Nenhum vínculo acima possui esse estado; fontes das respectivas linhas em (A). |
| ESPINHA A FAZER | 1 | app/docs/BACKLOG.md:1918; app/docs/BACKLOG.md:9 |
| LATERAL PRONTA | 1 | O232: cen+lei, nenhum congelado citado. app/docs/GRADE-MESTRA.md:853; app/docs/GRADE-MESTRA.md:7; app/docs/BACKLOG.md:1239 |
| LATERAL NÃO PRONTA | 5 | BO2910-ESTRUTURA: cen não fechado. app/docs/GRADE-MESTRA.md:170; app/docs/GRADE-MESTRA.md:7; app/docs/BACKLOG.md:1102 |
| SEM OBRA | 2 | HJ-005-U1: candidata sem prova; HJ-012-U2: cobertura [nao verificado]. app/docs/BACKLOG.md:820; app/docs/RELATO.md:8400 |

**ESTRUTURA: 18 = 9 + 0 + 1 + 1 + 5 + 2.** Soma dos casos discriminados em (A), com as provas acima. Encerramento atual dos casos em produção: **[nao verificado]**.

## 14/11 18:50 · sessao 58e554e940f1 · E3-X2-NUNCA-JULGADAS · censo · origem=Code

**E3-X2-NUNCA-JULGADAS — leitura**

A lista contém **136 colab-dias únicos**. Quantos são nunca lavrados, vencidos ou indeterminados, e os exemplos de cada estado, permanecem **[nao verificado]**: o anexo não contém `veredito_lavrado`, `julgado_em`, `janela_expira_em`, marcos ou perguntas. Fonte: anexo `u136.tsv:1–137` — numeração considerando o cabeçalho como linha 1.

| Empresa | Competência | Colab-dias | Fonte no anexo |
|---|---|---:|---|
| 1 | 10/2026 | 17 | `u136.tsv:2–18` |
| 1 | 11/2026 | 20 | `u136.tsv:19–38` |
| 2 | 09/2026 | 56 | `u136.tsv:39–94` |
| 2 | 10/2026 | 21 | `u136.tsv:95–115` |
| 2 | 11/2026 | 9 | `u136.tsv:116–124` |
| 3 | 09/2026 | 9 | `u136.tsv:125–133` |
| 3 | 10/2026 | 4 | `u136.tsv:134–137` |

**O recorte exige uma ressalva.** Os 136 representam o recorte sem dono informado. Não representam necessariamente todos os dias com `t['nao_sei'] == True`: a escolha do dono precede o fallback `nao_sei`, e uma porta publicada conserva essa flag. Excluir os outros 390 é coerente com “sem dono”, mas reduz o universo se o item 4 pretendia **todos** os dias com a flag. O número 390 não foi recontado aqui **[nao verificado]**. Fontes: `app/chamados/services/tarefa_do_dia.py:604–614`, `:629–636`; anexo `leitura_nao_fica_ociosa_complemento.txt:3`.

**Como nasce o desconhecimento**

A implementação de `esta_stale` está em `app/chamados/veredito_lavrado.py:213`. A carga admite em `_lavra` somente valores `sim/nao` que não estejam stale. Quando a última decisão é validação e a pergunta ficou fora desse mapa, `_pela_decisao` devolve `veredito_sem_lavra_vigente`; a tarefa então marca `nao_sei=True`. Fontes: `app/chamados/services/tarefa_do_dia.py:153–156`, `:320–325`, `:419–422`.

| Situação da pergunta | Ramo real |
|---|---|
| Campo vazio ou `julgado_em` ausente | `esta_stale=True`; `app/chamados/veredito_lavrado.py:224–225` |
| Lavra posterior ao instante consultado | Também stale; não prova que nunca houve lavra. `app/chamados/veredito_lavrado.py:224–225` |
| Relógio venceu a janela e a lavra antecede a expiração | Stale. A comparação usa datas locais. `app/chamados/veredito_lavrado.py:114–116`, `:228–229` |
| Marco do colaborador posterior à lavra | Stale. `app/chamados/veredito_lavrado.py:230–233` |
| Nenhum desses motivos | `esta_stale=False`. `app/chamados/veredito_lavrado.py:234` |
| Valor persistido `indeterminado` | Nasce quando o juiz lança exceção; não é, sozinho, motivo de stale. `app/chamados/veredito_lavrado.py:261–269`, `:224–234` |

`nao_sei` também nasce fora da lavra: célula ausente, ata sem lâmpadas, lâmpada desconhecida, marco sem teto, identidade insuficiente e outros retornos sem decisão. Portanto, não cabe converter automaticamente os 136 em “perguntas nunca julgadas”. Fontes: `app/chamados/services/tarefa_do_dia.py:216–226`, `:140–141`, `:173`, `:284–290`, `:305–310`, `:322–325`.

O próprio anexo contém **20 dias do colab 924 sem célula identificada**; isso não comprova ausência de pergunta ou de lavra. Fonte: `u136.tsv:74–93`; situação das perguntas desses dias **[nao verificado]**.

**Quem deveria lavrar**

| Estado/evento | Rotina e alcance demonstrado |
|---|---|
| Nunca lavrado ou vencido, com destino vivo | `lavrar_perguntas_stale`: seleciona perguntas cujo `chamado_destino` está em `VIVOS`, limita a rodada a 500 por padrão e chama `lavrar(p)` com `--apply`. `app/chamados/management/commands/lavrar_perguntas_stale.py:35`, `:57–62`, `:82–89` |
| Papel do cron | Cadastrado a cada cinco minutos, com `--apply`, estágio `continuo`, sem dependências; papel **`lavra`**. Cadastro não comprova execução. `app/config/crons.py:390–391`, `:2264`; execução **[nao verificado]** |
| Indeterminado com carimbos vigentes | Não há nova tentativa nesse cron apenas pelo valor `indeterminado`; precisa entrar por algum motivo de stale. `app/chamados/services/raiox.py:40–47` |
| Materialização | O materializador chama `carimbar(..., lavrar_veredito=True)`; a porta chama `lavrar(salvar=False)` e salva os campos juntos. `app/chamados/services/materializacao.py:1283–1284`; `app/chamados/models.py:3232–3235` |
| Fechamento do chamado | O signal S143 aciona o resolvedor; este seleciona perguntas do destino, não materializadas e com disputa aberta, e pode carimbar com lavra. `app/chamados/signals.py:91–117`; `app/chamados/services/disputa_emissao.py:1913–1917`, `:1993–1994` |
| Mudança do estado do destino | `_gravar_estado` relavra no ramo de cruzamento da fronteira de encerramento, para perguntas daquele destino sem validação. `app/chamados/models.py:303–312` |
| Reapontamento, mudança de dia ou via derivada | Relavram junto da alteração correspondente. `app/chamados/services/fio.py:964–967`, `:1081–1082`; `app/chamados/services/relavra_via.py:118–128` |
| Backfill | É sob ordem, não rotina; seu recorte compara o valor do veredito. `app/config/crons.py:1420`; `app/chamados/management/commands/backfill_veredito_lavrado.py:115–132` |

Assim, sem os destinos e campos das perguntas, atribuir falha individual a cron, signal ou porta permanece **[nao verificado]**. O leitor GET não é o lavrador atual. Fonte: `app/chamados/services/raiox.py:112–119`.

**Sonda proposta**

A sonda abaixo usa o mesmo universo, janela, `_carga` e chamadas de tarefa da remedida; reutiliza as perguntas carregadas e chama `esta_stale` real. Fontes: `app/folha/fora_da_folha.py:58–79`, `:109–144`; `app/chamados/services/tarefa_do_dia.py:392–422`, `:533–560`.

Para não inventar prioridade entre perguntas, o dia recebe `nunca_lavrado` ou `vencido` somente quando suas perguntas causais concordam. Ausência de causa de lavra exposta ou causas mistas ficam em `indeterminado`, identificadas como **classificação não resolvida [nao verificado]**, separadamente do valor persistido `VL_INDETERMINADO`. A necessidade dessa distinção decorre dos vários ramos de `nao_sei`. Fonte: `app/chamados/services/tarefa_do_dia.py:216–325`.

O congelamento abrange o shell porque o universo contém `localdate()` implícito. O bootstrap desativa os handlers de arquivo antes do Django iniciar e usa `-B`. Fontes: `app/folha/export.py:1238`; `app/requirements.txt:43`; `app/config/settings/saas.py:136–166`. Execução no ambiente de banco e resultados: **[nao verificado]**.

```bash
# Executar a partir de app/. Saida apenas em stdout; sem redirecionamento.
PYTHONDONTWRITEBYTECODE=1 python3 -B - <<'PY'
import datetime as dt
import importlib
import json
import os
import sys
from collections import Counter, defaultdict

# app/manage.py:9; app/config/settings/saas.py:136,160.
os.environ.setdefault("DJANGO_SETTINGS_MODULE", "config.settings.saas")
os.environ["PGOPTIONS"] = (
    os.environ.get("PGOPTIONS", "") + " -c default_transaction_read_only=on"
).strip()
config = importlib.import_module(os.environ["DJANGO_SETTINGS_MODULE"])
config.LOGGING_CONFIG = None

# Anexo u136.tsv:2-137. Cada linha agrupa datas do mesmo emp/comp/colab.
DADOS = """1 10/2026 950 2026-09-29 2026-10-08 2026-10-09 2026-10-14 2026-10-15 2026-10-16 2026-10-19 2026-10-20
1 10/2026 969 2026-10-07 2026-10-08 2026-10-09 2026-10-13 2026-10-14 2026-10-15 2026-10-16 2026-10-18 2026-10-20
1 11/2026 950 2026-10-21 2026-10-22 2026-10-23 2026-10-27 2026-10-30 2026-11-03 2026-11-11 2026-11-12 2026-11-13
1 11/2026 969 2026-10-22 2026-10-24 2026-10-26 2026-10-28 2026-10-30 2026-11-01 2026-11-03 2026-11-05 2026-11-07 2026-11-09 2026-11-13
2 09/2026 192 2026-08-23
2 09/2026 207 2026-08-22 2026-08-26 2026-08-30 2026-09-05 2026-09-13 2026-09-19
2 09/2026 209 2026-08-21 2026-08-23
2 09/2026 268 2026-08-21 2026-08-23
2 09/2026 301 2026-08-23
2 09/2026 327 2026-08-23
2 09/2026 382 2026-08-27
2 09/2026 435 2026-09-20
2 09/2026 438 2026-09-07
2 09/2026 439 2026-09-15
2 09/2026 451 2026-08-21 2026-08-23
2 09/2026 468 2026-09-17
2 09/2026 491 2026-09-20
2 09/2026 556 2026-09-14
2 09/2026 726 2026-09-08
2 09/2026 789 2026-08-26 2026-09-01 2026-09-13
2 09/2026 822 2026-08-28
2 09/2026 849 2026-09-18 2026-09-19
2 09/2026 915 2026-09-07
2 09/2026 921 2026-09-05 2026-09-12 2026-09-19
2 09/2026 922 2026-09-18
2 09/2026 923 2026-09-06
2 09/2026 924 2026-09-01 2026-09-02 2026-09-03 2026-09-04 2026-09-05 2026-09-06 2026-09-07 2026-09-08 2026-09-09 2026-09-10 2026-09-11 2026-09-12 2026-09-13 2026-09-14 2026-09-15 2026-09-16 2026-09-17 2026-09-18 2026-09-19 2026-09-20
2 09/2026 953 2026-09-16
2 10/2026 207 2026-10-10
2 10/2026 243 2026-09-27
2 10/2026 252 2026-09-29
2 10/2026 297 2026-09-21
2 10/2026 821 2026-09-30 2026-10-10 2026-10-12
2 10/2026 841 2026-10-10
2 10/2026 849 2026-10-02 2026-10-03
2 10/2026 922 2026-09-29 2026-10-17
2 10/2026 924 2026-09-22
2 10/2026 957 2026-09-21
2 10/2026 988 2026-10-10 2026-10-15 2026-10-16 2026-10-18 2026-10-19
2 10/2026 991 2026-10-11 2026-10-14
2 11/2026 265 2026-10-31 2026-11-14
2 11/2026 407 2026-10-21 2026-10-29 2026-11-02
2 11/2026 516 2026-10-22
2 11/2026 673 2026-10-21
2 11/2026 821 2026-10-24
2 11/2026 922 2026-10-23
3 09/2026 129 2026-08-28 2026-08-30 2026-09-08 2026-09-13 2026-09-15 2026-09-20
3 09/2026 515 2026-09-09
3 09/2026 829 2026-08-25 2026-09-15
3 10/2026 155 2026-09-27 2026-10-04 2026-10-11 2026-10-12"""

AGORA = dt.datetime.fromisoformat("2026-11-14T18:45:00+00:00")
ESTADOS = ("nunca_lavrado", "vencido", "indeterminado")


def sonda():
    from django.db import connection, transaction
    from django.utils import timezone
    from django_tenants.utils import schema_context
    from folha import fora_da_folha as F
    from colaboradores.models import Empresa
    from chamados.catalogo.motor import VIVOS
    from chamados.veredito_lavrado import (
        esta_stale, marcos_do_lote, relogio_virou,
    )
    from ponto.janelas import janela_fechamento

    alvos = []
    for linha in DADOS.splitlines():
        emp, comp, cid, *dias = linha.split()
        alvos.extend(
            (int(emp), comp, int(cid), dt.date.fromisoformat(d))
            for d in dias
        )
    assert len(alvos) == len({
        (cid, d) for _, _, cid, d in alvos
    }) == 136

    def j(obj):
        return json.dumps(
            obj, ensure_ascii=False, sort_keys=True, default=str,
        )

    def diagnostico(p, marcos):
        # Funcao REAL: app/chamados/veredito_lavrado.py:213.
        stale = esta_stale(p, marcos, AGORA)
        if not p.veredito_lavrado or p.julgado_em is None:
            estado = "nunca_lavrado"
            motivo = "campo_vazio_ou_julgado_ausente"
        elif stale:
            estado = "vencido"
            if p.julgado_em > AGORA:
                motivo = "lavra_posterior_a_agora"
            elif relogio_virou(
                p.julgado_em, p.janela_expira_em, AGORA,
            ):
                motivo = "relogio"
            else:
                motivo = "marco_posterior"
        elif p.veredito_lavrado not in ("sim", "nao"):
            estado = "indeterminado"
            motivo = "valor_da_lavra:" + p.veredito_lavrado
        else:
            estado, motivo = "vigente", "lavra_vigente"

        destino = p.chamado_destino
        no_cron = bool(destino and destino.status_local in VIVOS)
        return {
            "pergunta": p.pk,
            "motivo": p.motivo,
            "instancia": p.instancia_marco,
            "estado": estado,
            "diagnostico": motivo,
            "esta_stale": stale,
            "veredito_lavrado": p.veredito_lavrado,
            "vl_indeterminado_gravado":
                p.veredito_lavrado == "indeterminado",
            "julgado_em": p.julgado_em,
            "janela_expira_em": p.janela_expira_em,
            "marco": marcos.get(p.disputa.colaborador_id),
            "chamado_destino": p.chamado_destino_id,
            "status_destino":
                destino.status_local if destino else None,
            "no_universo_cron": no_cron,
            "elegivel_cron_stale": no_cron and stale,
            "fonte": (
                "app/chamados/veredito_lavrado.py:224; "
                "app/chamados/management/commands/"
                "lavrar_perguntas_stale.py:57"
            ),
        }

    resultados = []
    # Mesma carga/janela da remedida: fora_da_folha.py:58.
    with timezone.override("America/Sao_Paulo"):
        with schema_context("juliani"), transaction.atomic():
            with connection.cursor() as cur:
                cur.execute(
                    "SET TRANSACTION ISOLATION LEVEL "
                    "REPEATABLE READ, READ ONLY"
                )
                cur.execute("SHOW transaction_read_only")
                assert cur.fetchone()[0] == "on"
                cur.execute("SELECT current_schema()")
                assert cur.fetchone()[0] == "juliani"

            for emp_id, comp in sorted({
                (e, c) for e, c, _, _ in alvos
            }):
                mes, ano = map(int, comp.split("/"))
                emp = Empresa.objects.get(pk=emp_id)
                u = F.dias_que_seguram(emp, mes, ano)
                colabs, por = u["colabs"], u["dias"]
                retidos = [colabs[c] for c in por if por[c][1]]
                carga = (
                    F._carga(
                        retidos,
                        *janela_fechamento(mes, ano, emp),
                        AGORA,
                    )
                    if retidos else None
                )
                ids = {c.pk for c in retidos}
                marcos = (
                    marcos_do_lote(ids, ate=AGORA) if ids else {}
                )
                universo = {
                    (cid, d["data"])
                    for cid, (_, ds) in por.items()
                    for d in ds
                }

                for e, c, cid, dia in alvos:
                    if (e, c) != (emp_id, comp):
                        continue

                    r = {
                        "emp": e, "comp": c,
                        "colab": cid, "dia": dia,
                        "no_universo_atual": (cid, dia) in universo,
                        "estado": "indeterminado",
                        "perguntas": [],
                        "diagnostico":
                            "colab_fora_da_carga:[nao verificado]",
                        "fio": None,
                        "nao_sei": None,
                        "confere_recorte": False,
                    }

                    if carga is not None and cid in ids:
                        # As mesmas duas chamadas da remedida.
                        fio = F._fio_do_dia(
                            colabs[cid], dia, carga,
                        )
                        t = carga.tarefa(colabs[cid], dia)

                        causais = {
                            i["pergunta_id"]
                            for i in t["instancias"]
                            if i.get("razao")
                                == "veredito_sem_lavra_vigente"
                            and i.get("pergunta_id") is not None
                        }
                        # Objetos reais da carga:
                        # tarefa_do_dia.py:392-398.
                        ps = carga._perg.get((cid, dia), [])
                        diags = [
                            diagnostico(p, marcos) for p in ps
                        ]
                        escolhidas = [
                            p for p in diags
                            if p["pergunta"] in causais
                        ]
                        estados = {
                            p["estado"] for p in escolhidas
                        }

                        if (
                            escolhidas
                            and len(estados) == 1
                            and estados <= set(ESTADOS)
                            and len(escolhidas) == len(causais)
                        ):
                            r["estado"] = next(iter(estados))
                            r["diagnostico"] = (
                                "lavra_indeterminada"
                                if r["estado"] == "indeterminado"
                                else "perguntas_causais_concordam"
                            )
                        elif escolhidas:
                            r["diagnostico"] = (
                                "causas_mistas_ou_incompletas:"
                                "[nao verificado]"
                            )
                        else:
                            r["diagnostico"] = (
                                "nao_sei_sem_causa_de_lavra_exposta:"
                                "[nao verificado]"
                            )

                        r.update(
                            fio=fio,
                            fio_tarefa=t["fio"],
                            nao_sei=t["nao_sei"],
                            razao=t["razao"],
                            razoes_instancias=[
                                {
                                    "pergunta": i["pergunta_id"],
                                    "razao": i["razao"],
                                }
                                for i in t["instancias"]
                            ],
                            perguntas=escolhidas,
                            perguntas_do_dia=diags,
                            confere_recorte=(
                                r["no_universo_atual"]
                                and fio == "nada"
                                and t["nao_sei"]
                            ),
                        )
                    resultados.append(r)

    # Erro de carga/banco aborta: nao vira contagem aparente.
    assert len(resultados) == 136

    print("**E3-X2-NUNCA-JULGADAS — sonda de leitura**")
    print("\nAGORA=" + AGORA.isoformat() + "; tenant=juliani.")
    print(
        "\nFonte da carga: app/folha/fora_da_folha.py:58; "
        "fonte da tarefa: "
        "app/chamados/services/tarefa_do_dia.py:533; "
        "fonte stale: app/chamados/veredito_lavrado.py:213."
    )
    print(
        "\nIndeterminado do DIA inclui falta de classificacao "
        "unica; o valor persistido fica separado em "
        "veredito_lavrado. Convencao diagnostica "
        "[nao verificado]."
    )

    print(
        "\n| emp | comp | colab | dia | estado | "
        "perguntas causais |"
    )
    print("|---|---|---|---|---|---|")
    for r in resultados:
        pks = ",".join(
            str(p["pergunta"]) for p in r["perguntas"]
        ) or "[nao verificado]"
        print(
            "| {emp} | {comp} | {colab} | {dia} | "
            "{estado} | ".format(**r) + pks + " |"
        )

    print("\nDetalhes por dia, sem nomes, CPF ou respostas:")
    print("\n~~~jsonl")
    for r in resultados:
        print(j(r))
    print("~~~")

    cont = Counter(
        (r["estado"], r["comp"], r["emp"])
        for r in resultados
    )
    exemplos = defaultdict(list)
    for r in resultados:
        chave = r["estado"], r["comp"], r["emp"]
        if len(exemplos[chave]) < 3:
            exemplos[chave].append({
                "colab": r["colab"],
                "dia": r["dia"],
                "perguntas": [
                    p["pergunta"] for p in r["perguntas"]
                ],
                "diagnostico": r["diagnostico"],
            })

    print(
        "\n| estado | comp | emp | colab-dias | "
        "ate 3 exemplos |"
    )
    print("|---|---|---|---|---|")
    for comp, emp in sorted({
        (c, e) for e, c, _, _ in alvos
    }):
        for estado in ESTADOS:
            k = estado, comp, emp
            print(
                "| %s | %s | %s | %s | %s |" % (
                    estado, comp, emp,
                    cont[k], j(exemplos[k]),
                )
            )

    print(
        "\nSubtipos de indeterminado "
        "(nao confundir com VL_INDETERMINADO):"
    )
    for motivo, n in sorted(Counter(
        r["diagnostico"] for r in resultados
        if r["estado"] == "indeterminado"
    ).items()):
        print("- %s: %s" % (motivo, n))

    print("\nE3X2_TOTAL " + j(dict(Counter(
        r["estado"] for r 

[truncado em 20000 de 20915 caracteres]

## 14/11 18:49 · sessao 505e81710761 · - · - · origem=Ronald (tmux)

Estatística independente do anexo, agregada só em memória:

| Empresa | 09/2026 | 10/2026 | 11/2026 | Total |
|---|---:|---:|---:|---:|
| 3 | 94 | 117 | 156 | 367 |
| 4 | 6 | 8 | 9 | 23 |

Fonte: `universo_dias.tsv` do prompt; sem arquivo local numerado, portanto `[nao verificado]` para citação arquivo:linha.

**Correção certa:** empresa 2, competência 10, col607 tem **30 linhas**, de 21/09 a 20/10 consecutivos; contar 10 omite as 20 linhas de outubro. Fonte: anexo do prompt `[nao verificado]`.

Meu mapa preliminar empresa 2/10 soma **615**, encaminhado integralmente para comparação; ainda há diferença de 2 contra a contagem informada de 593 após corrigir col607 (+20). Não ajustei ao universo declarado. `[nao verificado]`

## 14/11 18:49 · sessao 32d3120f3ba2 · - · - · origem=Ronald (tmux)

Conferência independente de **emp2 / 11/2026: 790 linhas, 192 colabs**. Contagem por grupos do TSV colado; arquivo materializado `[nao verificado]`.

O mapa da outra agente divergia somente nestes grupos:

- **col371: 1 dia**, `2026-10-30`; ela havia contado 4.
- **col655: 5 dias**, `2026-10-22`, `2026-10-26`, `2026-11-02`, `2026-11-09`, `2026-11-10`; ela havia contado 9.

Assim, `797 − 3 − 4 = 790`, coincidente com minha transcrição independente. Fonte: anexo `universo_dias.tsv` colado; números de linha `[nao verificado]`.

Maiores grupos dessa fatia: col667=25; col915=25; col607=24; col717=24; col317=20; col921=20; col926=19; col534=16; col532=14; col349/696/820/832/967=13 cada. Fonte: anexo colado; números de linha `[nao verificado]`.

A conferência cobre somente emp2/11; **não ajustei ao total declarado de 2381**. Demais fatias `[nao verificado]` por esta conferência.

## 14/11 18:49 · sessao d4893d2abeca · - · - · origem=Ronald (tmux)

| Caso | Vínculo recomendado | Prova e estado |
|---|---|---|
| HJ-005-U1 | **candidata sem prova → SEM OBRA** | BO-ADMIN-2710 apenas mede/encaminha; não demonstra cobertura de col42 em 01/10. COBPARC nomeia 08/15/22-10 e ausência parcial, característica não demonstrada para U1 pelo anexo. app/docs/BACKLOG.md:820; app/docs/BACKLOG.md:829; app/docs/GRADE-MESTRA.md:137 |
| HJ-005-U2 | **COBERTURA-PARCIAL-UM-JUIZ — NO AR** | Nomeia col42 em 08/10 e FATO_EM_AUSENCIA. Pouso posterior ao PAROU antigo documentado no BACKLOG; fechamento explícito no RELATO. app/docs/BACKLOG.md:829; app/docs/BACKLOG.md:993; app/docs/RELATO.md:8127 |
| HJ-005-U4 | **COBERTURA-PARCIAL-UM-JUIZ — NO AR** | Nomeia col42 em 15/10; mesmas provas de estado. app/docs/BACKLOG.md:829; app/docs/BACKLOG.md:993; app/docs/RELATO.md:8127 |
| HJ-012-U2 | **nenhuma obra registrada — SEM OBRA** | O mapa fornecido diz sem obra. A referência a col239 no feriado apresenta cobrança legítima encaminhada ao DP, sem obra de cobertura demonstrada; preservar dono ESTRUTURA imposto pelo universo. app/docs/BACKLOG.md:1541; app/docs/RELATO.md:8390. Ausência de outra cobertura: [nao verificado]. |
| HJ-017-U1 | **O232 — LATERAL PRONTA** | O mapa fornecido indica O232; a obra nomeia col923, borda/ata e dias estruturais, embora não individualize 27/09. A GRADE confirma uma unidade do mapa HJ e mantém complemento da ATA pendente. app/docs/BACKLOG.md:1239; app/docs/GRADE-MESTRA.md:853 |
| HJ-030-U1 | **FIO-LE-O-JUIZ — NO AR** | Encaminhamento exato “col94 16/10 já em FIO-LE-O-JUIZ”; obra registra NO AR 7c9e4c27 e caso col94/#44764/#6984. app/docs/BACKLOG.md:2098; app/docs/BACKLOG.md:837 |

O232: **cen=sim; lei=sim**, pela grade `+++....++` e legenda. **toca_congelado=não nas linhas examinadas**: aparecem cartorio.py, escala/utils.py, ponto/turnos.py e teste; nenhum dos seis congelados. A colisão com `ponto/turnos.py` não altera esse campo nem o critério literal de LATERAL PRONTA solicitado. app/docs/GRADE-MESTRA.md:7; app/docs/GRADE-MESTRA.md:853; app/docs/BACKLOG.md:1239.

Ressalva de interpretação: O232 tem prova nominal de col923 + vínculo fornecido no mapa, mas não dia 27/09 no registro da obra; se a consolidação exigir também igualdade de data, marcar **candidata sem prova**. O critério do usuário permite caso, classe **ou** sítio; por isso recomendo aceitar o vínculo sem afirmar resolução efetiva em produção. app/docs/BACKLOG.md:1239; app/docs/GRADE-MESTRA.md:853.

## 14/11 18:48 · sessao 98db6cf4ab8a · - · - · origem=Ronald (tmux)

| Caso BO-ADMIN-2710 | Obra | Prova de cobertura | Estado documentado |
|---|---|---|---|
| 1 | COBERTURA-PARCIAL-UM-JUIZ | `app/docs/BACKLOG.md:829` nomeia item 1, col42 e `supra_juiz` RAMO 2 | GRADE registra PARADA (`app/docs/GRADE-MESTRA.md:39`); BACKLOG registra deploy posterior COBPARC `e1c08457`, 02/11 (`app/docs/BACKLOG.md:993`). Divergência documental. |
| 2 | JUIZ-DO-PAR-DUAS-ENTRADAS | Classe “motor e ata formarem os mesmos turnos” em `app/docs/GRADE-MESTRA.md:38`; mesma batida/marco produzindo turnos diferentes em `app/docs/BACKLOG.md:819`. Vínculo nominal col789 e `_match_marcos` em `app/docs/CORTES.md:340`. | NO AR `b3bf529e`, com resíduos (`app/docs/BACKLOG.md:819`). |
| 3 | JUSTIFICATIVA-PAGA-O-PERIODO | Caso RT-04 col789 11/10 e separação da obra A explícitos (`app/docs/BACKLOG.md:1918`). | ESPINHA A FAZER por ordem viva (`app/docs/BACKLOG.md:9`; `app/docs/BACKLOG.md:1918`), embora balde LATERAL na GRADE (`app/docs/GRADE-MESTRA.md:269`). |
| 5 | JUIZ-DO-PAR-DUAS-ENTRADAS | RT-02 col247 24/10, pergunta errada de intervalo explicitada (`app/docs/BACKLOG.md:819`). | NO AR `b3bf529e` (`app/docs/BACKLOG.md:819`). |
| 6 | FIO-LE-O-JUIZ | Carregar perguntas vivas independentemente do estado da disputa (`app/docs/BACKLOG.md:837`). | NO AR `7c9e4c27` (`app/docs/BACKLOG.md:837`). |
| 7 | REGISTRADO-NAO-E-VIVO | Item 7 e chamado #28883 explicitados (`app/docs/BACKLOG.md:823`). | NO AR na linha; hash `475318fa` aparece no RELATO (`app/docs/BACKLOG.md:823`; `app/docs/RELATO.md:3371`). |
| 8.1 | TRILHA-DO-VINCULO-MENTE | Item 8.1 explicitado (`app/docs/BACKLOG.md:821`). | NO AR `5e9a0a51`; resíduos RT-03 são outros problemas (`app/docs/BACKLOG.md:821`). |

A parte de justificativa do item 3 saiu expressamente da COBERTURA-PARCIAL para JUSTIFICATIVA-PAGA-O-PERIODO; não atribuir ambas ao mesmo caso (`app/docs/CORTES.md:352`; `app/docs/BACKLOG.md:1918`).

Para laterais, a convenção das casas é `cen-lei-...`, com `+` feito e `.` falta (`app/docs/GRADE-MESTRA.md:7`):

| Lateral | Cen | Lei | Congelado citado na linha da obra |
|---|---|---|---|
| JUSTIFICATIVA-PAGA-O-PERIODO | sim | sim | não; linha cita motor/efeito de ausência (`app/docs/GRADE-MESTRA.md:269`; `app/docs/BACKLOG.md:1918`). |
| REGISTRADO-NAO-E-VIVO | não | sim | não (`app/docs/GRADE-MESTRA.md:139`; `app/docs/BACKLOG.md:823`). |
| TRILHA-DO-VINCULO-MENTE | sim | sim | não; cita serviços/views de colaboradores e signal de escala (`app/docs/GRADE-MESTRA.md:138`; `app/docs/BACKLOG.md:821`). |

`chamados/services/painel.py`, citado no histórico da obra REGISTRADO, é caminho diferente do congelado `chamados/painel.py` (`app/docs/RELATO.md:3381`).

A linha de prova nominal da obra para item 2 está em CORTES, fora das duas fontes autorizadas para a coluna de prova; nas fontes autorizadas há prova pela classe em GRADE:38/BACKLOG:819. Se a classe for considerada insuficiente, registrar “candidata sem prova”, sem promover CORTES a prova permitida (`app/docs/GRADE-MESTRA.md:38`; `app/docs/BACKLOG.md:819`; `app/docs/CORTES.md:340`).

## 14/11 18:47 · sessao eb0432a81e21 · - · - · origem=Ronald (tmux)

Os cinco casos têm cobertura literal em **BO2910-ESTRUTURA**, cuja coluna `resolve` enumera “casos 01, 02, 05, 06, 08”; balde LATERAL-ESTRUTURA, estado REGISTRADA, grade `.+.......`. Portanto, **cen não; lei sim; LATERAL NÃO PRONTA**. (app/docs/GRADE-MESTRA.md:170; legenda app/docs/GRADE-MESTRA.md:7)

```tsv
caso_fonte	caso_id	obra_id	estado_obra	prova_arquivo:linha	lateral_cen	lateral_lei	toca_congelado
BO2910-ESTRUTURA	01	BO2910-ESTRUTURA	LATERAL-ESTRUTURA (REGISTRADA)	app/docs/BACKLOG.md:1102;app/docs/GRADE-MESTRA.md:170	não (GRADE-MESTRA.md:170;:7)	sim (GRADE-MESTRA.md:170;:7)	não, pelos arquivos citados (BACKLOG.md:1102)
BO2910-ESTRUTURA	02	BO2910-ESTRUTURA	LATERAL-ESTRUTURA (REGISTRADA)	app/docs/BACKLOG.md:1102;app/docs/GRADE-MESTRA.md:170	não (GRADE-MESTRA.md:170;:7)	sim (GRADE-MESTRA.md:170;:7)	não, pelos arquivos citados (BACKLOG.md:1102)
BO2910-ESTRUTURA	05	BO2910-ESTRUTURA	LATERAL-ESTRUTURA (REGISTRADA)	app/docs/BACKLOG.md:1102;app/docs/GRADE-MESTRA.md:170	não (GRADE-MESTRA.md:170;:7)	sim (GRADE-MESTRA.md:170;:7)	não, pelos arquivos citados (BACKLOG.md:1102)
BO2910-ESTRUTURA	06	BO2910-ESTRUTURA	LATERAL-ESTRUTURA (REGISTRADA)	app/docs/BACKLOG.md:1102;app/docs/GRADE-MESTRA.md:170	não (GRADE-MESTRA.md:170;:7)	sim (GRADE-MESTRA.md:170;:7)	não, pelos arquivos citados (BACKLOG.md:1102)
BO2910-ESTRUTURA	08	BO2910-ESTRUTURA	LATERAL-ESTRUTURA (REGISTRADA)	app/docs/BACKLOG.md:1102;app/docs/GRADE-MESTRA.md:170	não (GRADE-MESTRA.md:170;:7)	sim (GRADE-MESTRA.md:170;:7)	não, pelos arquivos citados (BACKLOG.md:1102)
```

Provas específicas, todas em **app/docs/BACKLOG.md:1102**:

- **01:** `vincular_posto` engole exceção e troca posto sem empresa; sítios `colaboradores/views.py:333-351` e `transferir_posto :1366`.
- **02:** chamado de texto livre sem leitor da célula; sítio `reconciliador.py:336`, `MODULOS_DE_TURNO`.
- **05:** parcial aprovada sem período pela tela; período obrigatório na aprovação/edição.
- **06:** desligamento grava `data_fim` sem conferir vigência; reativação não reabre vínculo.
- **08:** veredito `admin` sobrevive à regeneração da célula com marcos novos; sítio `veredito_celula.py:222-224`.

**Resolve por obra:** BO2910-ESTRUTURA → BO2910-ESTRUTURA/01, /02, /05, /06, /08. (app/docs/GRADE-MESTRA.md:170; app/docs/BACKLOG.md:1102)

**Subtotal:** 5 = NO AR 0 + EM VOO 0 + ESPINHA A FAZER 0 + LATERAL PRONTA 0 + LATERAL NÃO PRONTA 5 + SEM OBRA 0. (app/docs/GRADE-MESTRA.md:170; app/docs/GRADE-MESTRA.md:7)

A referência `BACKLOG:1059` impressa na grade está desatualizada: a linha efetiva da obra nesta cópia é **app/docs/BACKLOG.md:1102**. Nenhum dos seis congelados é citado nessa linha; alteração efetiva futura desses arquivos permanece **[nao verificado]**.

## 14/11 18:47 · sessao 32d3120f3ba2 · - · - · origem=Ronald (tmux)

Censo parcial consolidável, todo por leitura; **não sustento fechamento de toda a árvore** com esta fatia. Os caminhos abaixo foram identificados em AST e conferidos no fonte; permissões efetivamente atribuídas em produção permanecem `[nao verificado]`.

**Método executado:** `python3 -B`, `ast.parse`, `ast.walk`, inspeção de `Call`, `FunctionDef` e mapa de `ImportFrom` com `asname`. Leitura de URLs, chamadas e escritores complementar; nenhum arquivo criado.

**Portas especialmente relevantes fora de chamados**

| Ação | Porta e URL | Guarda real | Gravação/trilha |
|---|---|---|---|
| Resolver dia: trabalhou/faltou/justificado | `app/ponto/urls.py:58`; `app/ponto/views.py:3352` | POST + `tem_acao(...,'regularizar_batida')`, `views.py:3355`; alias `_vc` importado em `3354`; guardas de negócio **inline** em `services/veredito_celula.py:220`, `243`, `249`, `309`, `334` | Batida via `registrar_batida`, `:323`; Ausencia aprovada via `criar_ausencia`, `:337`; perguntas via `fechar_por_admin`, `:190`; decisão SILENCIO, `:195`; chamado reconciliado e ocorrência, `:203`; `CelulaDia.lavrar_veredito`, `:352`; LogAuditoria, `:356`. |
| Inverter batida | `urls.py:59`; `views.py:3300` | View `:3303`; guardas inline `flip_batida.py:15` retratada, `:19` autor, `:22` `competencia_fechada`, `:26` tipo | `Batida.tipo`, via `registro_batida.anotar`, `flip_batida.py:30`; trilha antes/depois e recompute `:39`. |
| Retratar batida | `urls.py:61`; `views.py:3314` | `_retratar` → `retratar_humano.retratar`, import `:3316`; `_pode` chamável `retratar_humano.py:131`; motivo/ativo inline `:166`, `:170` | `retratar_batida`, `:179`, motivo estruturado `:181`; efeito no dia. |
| Desfazer retratação | `urls.py:62`; `views.py:3326` | `_desfazer` → `retratar_humano.desfazer`, import `:3328`; `_pode`, `:194`; retratada/origem/prazo inline `:197`, `:199`, `:203`; `pode_desfazer(batida,agora=AGORA)` existe em `:105` mas **não cobre a guarda toda** | `Batida.aprovada=True,retratada_em=None`, `:207`, trilha via `anotar`. |
| Aprovar/rejeitar justificativa | `urls.py:40`, `43`; `views.py:1914`, `1926` | POST + `avaliar_justificativas`, `:1916`, `:1928`; executores `justificativa.py:132`, `262`; guardas inline | Aprovação `status/aprovada_por/aprovada_em/batida_gerada`, `justificativa.py:204`; Batida S, `:188`; invalida fechamento `:215`; encerra origem `:217`; trilha `:230`. Rejeição campos `:283`, chamado `:291`. |
| Aprovar justificativas em lote | `urls.py:41`; `views.py:1766` | Inline `:1769`, pendente `:1783`, `:1797`, batida recusada `:1825`, saída já utilizada `:1829` | Escrita **própria** `Justificativa.status/aprovada_por/aprovada_em`, `:1799`; batida S `:1818`; `batida_gerada` `:1834`; invalidação `:1808`; encerra origem `:1840`. |
| Rejeitar justificativas em lote | `urls.py:42`; `views.py:1862` | Inline POST/permissão `:1865`, pendente `:1877` | Campos próprios `status/aprovada_por/aprovada_em/motivo_rejeicao`, `:1878`; save `:1882`; notificações. Trilha própria de sucesso não observada nessa função `[nao verificado]`. |
| Materializar saída retroativa | `urls.py:44`; `views.py:3146` | `materializar_saida` `:3158`; hora e motivo mínimo `:3173`, `:3175`; executor `justificativa.materializar_saida_dp`, `:3185` | Executor pode registrar saída e justificativa; campos completos `[nao verificado]`. |
| Lançar ausência | `urls.py:57`; `views.py:2400` | `editar_ausencias`, `:2402`; `corpo_chegou_inteiro`, `:2431`; guardas inline `ausencia.criar_ausencia`, `ausencia.py:242`, `247`, `249`, `261` e sobreposição posterior | Ausencia aprovada `views.py:2447`; afastamento altera `Colaborador.situacao`, `:2465`. |
| Operar ausência existente | `urls.py:54`–`56`, `65`–`76`; funções em `views.py:2279`, `2320`, `2356`, `2601`, `2631`, `2697`, `2715`, `2758`, `2791` | `_a4_decidir` `views.py:85` chama `ponto.services.ausencia.decidir`; edição e reversão usam serviços respectivos; guardas completas `[nao verificado]` | Estado/range/dados de Ausencia; campos completos por variante `[nao verificado]`. |
| Recalcular/aprovar/reabrir fechamento | `urls.py:31`–`36`; funções `views.py:1229`, `1260`, `1386`, `1437`, `1464`, `1523` | `editar_folha`/POST; `aprovar_colaborador` lê `fechamento_lido` `:1483` e `_classificar_export` alias `folha.export.classificar_export` `:1508`; seleção de travas inline `:1512` | Serviços `fechamento.aprovar_lote:1323`, `aprovar:1335`, `reabrir_lote:1344`, `reabrir:1349`, `trancar_periodo:1400`, `reabrir_periodo:1432`; status/carimbos e período, com efeitos adicionais. |
| Exportar folha | `folha/urls.py:14`; `folha/views.py:102` | Guarda completa `[nao verificado]` | `ExportacaoDominio.registrar`, `aprovar_lote`, `registrar_log` na função. |

O serviço `veredito_celula` **não pode ser chamado por uma sonda somente leitura**: guarda e execução estão misturadas e ele inicia transação/travas em `app/ponto/services/veredito_celula.py:224`, antes da decisão final. Uma sonda não deve copiar essas condições.

**Porta administrativa adicional**

`/admin/` é publicado em `app/config/urls.py:24`. `JustificativaAdmin` deriva diretamente de `admin.ModelAdmin`, sem `SoLeitura`, sem `readonly_fields` e sem override de permissão (`app/ponto/admin.py:18`). Campos editáveis incluem colaborador, turno, batida de entrada, tipo, saída informada, status, aprovador, carimbo, rejeição, batida gerada e chamado de origem (`app/ponto/models.py:136`–`179`). Quem possui as permissões Django em produção `[nao verificado]`; trilha `LogAuditoria` da aplicação por esta porta `[nao verificado]`.

Batida, ImportacaoAEJ e Ausencia herdam `SoLeitura` (`app/ponto/admin.py:11`, `25`, `37`): recusas reais de add/change/delete retornam `False` em `app/core/admin.py:59`, `62`, `65`; todos os campos readonly em `:68`.

**Comandos de escrita identificados por AST**

Caminhos abaixo relativos a `app/chamados/management/commands/`:

| Comando | Chamada de escrita | Guarda/restrição observada |
|---|---|---|
| `curar_perguntas_em_limbo` | `:67 carimbar_resposta_pela_trilha`; `:69 reabrir_para_correcao` | Comparação exata com conjunto MEDIDAS inline `:46`–`53`; nota encontrada `:55`; `--apply` `:64`. |
| `fechar_becos_disputa` | `:57 becos.fechar_beco` | Guarda **chamável** `becos.classificar(d)` `:39`; implementação `services/becos.py:25`–`33`, usa `d.pode_fechar()`. |
| `carimbar_por_batida_coincidente` | `:98 _ctx.renovar_contexto`; `:100 _vinc.apontar_celula`; trilha `:101` | `_ctx` → `chamados.services.contexto`; `_vinc` → `.vinculos` imports `:58`; seleção inline `:60`–`85`. |
| `encerrar_perguntas_com_fato` | `:68 p.encerrar_por_fato_no_espelho`; `:88 d.fechar` | Seleção inline `:31`–`43` exige materializada, não validada, via sem história e `fato_do_marco_no_espelho(p) is True`. |
| `perguntar_chamados_sem_pergunta` | `:29 pergunta_no_app.perguntar` | Guarda integral `[nao verificado]`. |
| `silenciar_chamados_isentos` | `:49 silencio_isentos.silenciar` | Guarda chamável `silencio_isentos.alvos()` `:36`; implementação `services/silencio_isentos.py:20`: flag isento, estado vivo, ainda sem silêncio. |
| `arquivar_prescritos` | `:94 arquivo.silenciar` | Recortes inline `:43`, `49`, `53`, `60`; verbo validar/cobrar/decidir; idade criado_em; não silenciado nem resgatado. |
| `arquivar_sem_dia_irrecuperavel` | `:94 arquivo.silenciar` | Recortes inline `:58`–`68`; escopo DIA, sem dia do fato e anterior à corrente. |
| `arquivar_competencia_encerrada` | `:288 arquivo.silenciar` | `_disputa_viva` chamável `:148`; conjunto/corte integral inline `handle`. |
| `reabrir_orfas_enterradas` | `:157 p.reabrir_fantasma`; `:165 d.reabrir_sistema`; `:172 pai.reabrir` | Recortes inline `:67`–`112`; via fantasma, lâmpada False, pergunta viva, janela fixa do carimbo, exportada fora, recusa/irmãs do pai. |
| `encerrar_por_competencia_trancada` | `:29 competencia_trancada.encerrar` | Guarda completa de planejamento **chamável** `services/competencia_trancada.medir`, `:90`–`185`; `encerrar(...,aplicar=False)` `:188`–`193` só devolve medida. |
| `nomear_marco_por_lampada` | `:109 contexto.renovar_contexto`; evento `:111` | Guardas inline `:47`–`85`; dia apurável, lâmpada apagada e desempate da hora/origem/única. |
| `relavrar_via_i6` | `:102 relavra_via.relavrar` | `planejar` chamável `:44`–`70`; executa `universo_i6()`, vias derivadas, exclusão por ordem, `via_de_hoje`. |
| `backfill_decisoes_pergunta` | `:324 DecisaoPergunta.registrar` | Guardas do apply misturadas à execução; detalhes `[nao verificado]`. |
| `dedup_perguntas_fato` | `:105 aplicar` → `backfill_decisoes_pergunta.aplicar` import `:104` | Grupos `:58`; `plano_de_retratacao` real `:81`; recorte/horizonte/merito/exportada no apply. |
| `lavrar_perguntas_stale` | `:89 veredito_lavrado.lavrar` | Guarda chamável `raiox.pks_stale_por_varredura` `:60`; candidatos pelo destino vivo `:57`–`58`; teto inline `:61`. |
| `backfill_veredito_lavrado` | `:143`, `:146 gravar_lavras_em_lote` | Juiz real `_pergunta_materializada_de_fato` `:105`; divergência e não apagar lavra por indeterminação inline `:120`, `124`. |
| `backfill_fio_morto_e4b` | `_executar_dia:125`: reabrir credor `:135`, registrar decisão `:141`, perguntar `:143`, materializar `:147` | Planejamento chamável `ato_do_dia:82`; usa `T.tarefa_do_dia` e `reabrir_credor_de_pergunta_viva(...executar=False)` `:90`. |
| `validar_classe_a` | `:100 validacao.validar_pergunta` | Recorte inline `:44`–`67`; só respostas sem veredito, classificação real CLASSE_A; autor existente `:90`. |
| `reatar_beco_folga_ao_limbo` | `limbo_folgas.reatar_perguntas_ao_limbo`, import `:50` | Guarda real de seleção `_folga_nao_definida_sem_porta(...agora=AGORA)` `:54`; pai na `qs_na_fila` `:59`; sem pai/fora da fila `:63`–`68`; reversão obrigatória `:39`–`43`. |
| `retratar_chamados_lei_cala` | `:59 retratar` → `chamados.reconciliador.retratar`, import `:29` | Guardas inline `:32`–`47`: dia passado e `classificar_falta(...)[1] is None`. |
| `auditar_chamados_orfaos --fechar` | `:68 transicionar_com_pauta(...ato=ch.reconciliar)`; ocorrência `:73` | Recorte inline `:35`–`44`; gate real dentro da transição. |
| `materializar_passivo_validado` | `:104`, `:113 passivo_validado.reparar` | Mais restrições/autor em `:70`–`83`; **até dry escreve arquivo** `:85`, `:117`–`130`. |
| `geofence_registrados_para_analise` | `:32 ch.analisar` | Guarda integral `[nao verificado]`. |
| `alertar_chamados_sla` | `:44 nota.anexar_ocorrencia`; push `:48` | Guardas reais chamáveis `sla.vencido`, `sla.ja_avisado`, `sla.vence_em` `:32`–`34`; teto da passada inline `:37`. | [nao verificado]

`arquivo.silenciar` grava `ChamadoColaborador.silenciado_ate`, `.silenciado_por`, `.silenciado_motivo`, `.atualizado_em` e trilha `evento` (`app/chamados/services/arquivo.py:156`–`173`). Não muda `status_local`.

`relavra_via.relavrar` grava `PerguntaDisputa.via_resolucao`, `.veredito_lavrado`, `.julgado_em`, `.janela_expira_em` e evento por pergunta (`app/chamados/services/relavra_via.py:118`–`138`).

`competencia_trancada.encerrar` hoje não carimba perguntas (`app/chamados/services/competencia_trancada.py:133`–`135`, `203`). Encerra chamados pela transição (`:209`), anexa ocorrência e evento (`:210`–`219`), fecha disputa pela porta (`:229`), pode criar Pauta DP (`:232`, `256`).

**Cuidados necessários para a sonda**

- Não chamar comandos em dry presumindo ausência de escrita: `materializar_passivo_validado` cria pasta/JSON mesmo sem apply (`:85`, `118`).
- Não chamar serviço mutador e fazer rollback: isso já executa escrita e sinais; não satisfaz a restrição.
- Guardas inline devem sair como **não sondável**, sem contagem inventada.
- `core.portas_da_tela` não fecha este censo: exclui API/admin em `app/core/portas_da_tela.py:24` e detecta POST por texto em `:52`; guardas por regex em `:57`.
- `core.views.painel_operacional` e seu lote são leitura (`app/core/views.py:589`, `621`), não outra porta de decisão.
- `regularizar_retorno` usa `request.user.colaborador` (`app/ponto/views.py:3208`): é ação do próprio colaborador, não uma porta administrativa genérica.
- Leitura completa dos comandos de outros apps, API, escala/cadastro, férias e ações de IA ainda é necessária para declarar **TODA ação**. `[nao verificado]`

_5240 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
