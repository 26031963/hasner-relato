# CODEX-ESTADO

> Ultima resposta processada: **09/11/2026 10:10** · publicado em **09/11/2026 10:13** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 09/11 10:10 · sessao 86b79a2759f4 · FIO-E0-V4-E4-ESCRITORES · censo · origem=Code

**O número 16 confere para as portas de alteração do estado. Incluindo os nascimentos por `create`/`get_or_create`/inserção, como o pedido exige, são 22 portas produtivas: 16 + 2 criadores de Chamado + 3 de Disputa + 1 de Pergunta.** Há ainda o executor genérico `PerguntaDisputa.save` e uma exclusão restrita a ensaio, discriminados abaixo. A diferença não é uma porta nova da E4: é a inclusão dos nascimentos que o recorte histórico separava como identidade. (`app/docs/RELATO.md:242`; `app/chamados/models.py:833`; `app/chamados/models.py:1054`; `app/chamados/models.py:2822`; `app/chamados/services/disputa_emissao.py:474`; `app/chamados/services/disputa_emissao.py:1024`; `app/chamados/services/disputa_emissao.py:1299`; `app/chamados/models.py:3402`; `app/chamados/models.py:563`)

A classificação usa a E1: **decisão** é o ato preservado na sequência append-only; **efeito** é sua consequência, conferida contra o fato/lavra; **fica** é identidade, data, cache ou metadado do continente. Uma porta pode ter ramos diferentes, explicitados sem contá-la duas vezes. A E1 declara que os campos atuais são projeção até a E4; a tarefa exige conferir decisões e efeitos, sem escritor próprio. (`app/chamados/models.py:3828`; `app/chamados/models.py:3833`; `app/chamados/models.py:3840`; `app/chamados/services/tarefa_do_dia.py:118`; `app/chamados/services/tarefa_do_dia.py:140`; `app/docs/CORTES.md:426`)

A análise foi estática por AST, incluindo imports com apelido, imports relativos, módulos `__init__` e imports locais. Não houve importação da aplicação, consulta a banco/log nem escrita de arquivo. Completude perante despacho inteiramente dinâmico, SQL construído dinamicamente, templates e execução real: **[nao verificado]**. Nos chamadores abaixo, **P** significa código produtivo/operacional; **T** identifica testemunhas de teste, sem afirmar execução delas nem esgotar todas as fixtures de nascimento. **[nao verificado]** [nao verificado]

Para compactar os campos repetidos:

- **C** = `materializada_em`, `materializada_por`, `via_resolucao`. (`app/chamados/models.py:3117`)
- **R** = `resposta_colab`, `respondida_em`; **V** = `validada_em`, `validada_por`. (`app/chamados/models.py:2444`; `app/chamados/models.py:3366`)
- **L** = `veredito_lavrado`, `julgado_em`, `janela_expira_em`. (`app/chamados/veredito_lavrado.py:250`)
- **F** = `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento`. (`app/chamados/models.py:1933`)
- **DP** = `DecisaoPergunta`; “sem DP próprio” não significa ausência de trilha nem ausência de decisão registrada pelo chamador. (`app/chamados/models.py:3836`; `app/chamados/services/disputa_emissao.py:1612`)

Leis/cortes conferidos para as decisões abaixo: L-002, L-007, L-051, L-061, L-073, L-080, L-085, L-088, L-110, L-111, L-113, L-117 e L-124; FIO-E0-V4 e LEI-TRANCA-E2-JUIZ. (`app/docs/LEIS.md:60`; `app/docs/LEIS.md:65`; `app/docs/LEIS.md:92`; `app/docs/LEIS.md:95`; `app/docs/LEIS.md:100`; `app/docs/LEIS.md:107`; `app/docs/LEIS.md:117`; `app/docs/LEIS.md:120`; `app/docs/LEIS.md:136`; `app/docs/LEIS.md:137`; `app/docs/LEIS.md:151`; `app/docs/LEIS.md:138`; `app/docs/LEIS.md:145`; `app/docs/CORTES.md:426`; `app/docs/CORTES.md:427`)

## CENSO

escritores:

- `app/chamados/models.py:201` `ChamadoColaborador._gravar_estado` -- `status_local=novo`; ao terminar, `resolvido_em` recebe instante, `via_encerramento=via`, urgência é rebaixada; ao reviver, limpa resolução/via e restaura urgência; pode gravar `respondido_por`; grava `atualizado_em` -- **efeito** nas transições derivadas; **decisao** no ramo de transição humana. Persiste em `app/chamados/models.py:296`; sem DP próprio, com trilha `estado_chamado` e relavra L das perguntas atingidas. P: `_aplicar` (`app/chamados/models.py:450`), `analisar` (`app/chamados/models.py:806`), `reconciliar` (`app/chamados/models.py:654`), `cancelar` (`app/chamados/models.py:788`), `reabrir` (`app/chamados/models.py:1442`), `encerrar` (`app/chamados/models.py:1616`), `encerrar_por_competencia_trancada` (`app/chamados/models.py:1636`). T: `app/chamados/tests/test_hx_b7_trilha.py:32`; `app/chamados/tests/test_o261e_f1_porta_encerramento.py:206`; `app/chamados/tests/test_o261e_f1_codex_v5.py:89`.

- `app/chamados/models.py:1852` `DisputaSupervisao.fechar` -- F recebe instante, autor, texto e motivo; excepcionalmente carimba perguntas vivas -- **efeito** no fechamento normal, após nenhuma viva/resposta sem veredito; **decisao** nas exceções `admin_encerra`/`desligamento`, que já registram DP `SILENCIO` no mesmo atomic (`app/chamados/models.py:1915`; `app/chamados/models.py:1921`; `app/chamados/models.py:1933`). P: `admin_encerra` e `remover_motivo` (`app/chamados/models.py:1947`; `app/chamados/models.py:2069`); reconciliador (`app/chamados/reconciliador.py:412`; `app/chamados/reconciliador.py:612`; `app/chamados/reconciliador.py:1171`); foto (`app/api/foto_recebida.py:80`); desligamento (`app/colaboradores/services/desligamento.py:142`); entrada sincronizada (`app/ponto/services/entrada_sincronizada.py:59`); materialização (`app/chamados/services/materializacao.py:586`; `app/chamados/services/materializacao.py:1333`); emissão (`app/chamados/services/disputa_emissao.py:1281`; `app/chamados/services/disputa_emissao.py:1961`); tranca (`app/chamados/services/competencia_trancada.py:204`); beco (`app/chamados/services/becos.py:57`); comandos (`app/ponto/management/commands/fechar_chamados_meio_periodo.py:109`; `app/chamados/management/commands/encerrar_perguntas_com_fato.py:88`). T: `app/chamados/tests/test_porta_disputa_ciclo.py:285`; o contrato e as exceções estão na própria porta (`app/chamados/models.py:1855`).

- `app/chamados/models.py:2123` `DisputaSupervisao.reabrir_admin` -- limpa F; incrementa `reaberturas_admin`; pode agregar `motivos` -- **decisao** no ato administrativo; **fica** o contador/metadado. Persiste em `app/chamados/models.py:2158`; sem DP próprio. P: `app/chamados/services/acoes_disputa.py:259`. T: `app/chamados/tests/test_disputa_transicoes.py:134`; `app/chamados/tests/test_disputa_transicoes.py:148`; `app/chamados/tests/test_disputa_transicoes.py:156`; `app/chamados/tests/test_disputa_transicoes.py:163`; `app/chamados/tests/test_disputa_transicoes.py:172`; `app/chamados/tests/test_disputa_transicoes.py:181`.

- `app/chamados/models.py:2160` `DisputaSupervisao.reabrir_sistema` -- limpa F -- **efeito**, religando o continente após a pergunta voltar a exigir ação. Persiste em `app/chamados/models.py:2174`; sem DP próprio. P: `app/chamados/models.py:1572`; `app/chamados/services/materializacao.py:686`; `app/chamados/services/validacao.py:218`; `app/chamados/services/relavra_via.py:261`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:165`. Chamador direto de teste não localizado: **[nao verificado]**.

- `app/chamados/models.py:3092` `PerguntaDisputa.carimbar` -- C recebe instante/autor/via; opcionalmente completa `respondida_em`, V e L -- **efeito** para o carimbo; **decisao** quando completa resposta/validação. Persiste em `app/chamados/models.py:3141`; sem DP próprio, inclusive nos ramos que completam assinaturas (`app/chamados/models.py:3119`). P: `app/chamados/reconciliador.py:405`; `app/chamados/reconciliador.py:595`; `app/chamados/models.py:3276`; `app/chamados/services/materializacao.py:561`; `app/chamados/services/materializacao.py:1277`; `app/chamados/services/materializacao.py:1431`; `app/chamados/services/disputa_emissao.py:1860`; `app/chamados/services/disputa_emissao.py:1934`. T: `app/chamados/tests/test_o261e_f2_porta_carimbo.py:725`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:738`; `app/chamados/tests/test_o261e_f2_fila.py:194`; `app/chamados/tests/test_o261e_f2_fila.py:205`; `app/chamados/tests/test_o261e_f2_fila.py:221`; `app/chamados/tests/test_o261e_f2_fila.py:228`.

- `app/chamados/models.py:3157` `PerguntaDisputa.carimbar_identidade` -- C recebe via terminal/autor/instante, ou preserva autor/instante anteriores; pode gravar `recusa_motivo` -- **efeito** da identidade/decisão aplicável; o ato humano que o determina é **decisao**. Persiste em `app/chamados/models.py:3179`; sem DP próprio. P: `fechar_por_admin` (`app/chamados/models.py:3053`), `remover_motivo` (`app/chamados/models.py:2058`), recusa/paridade (`app/chamados/services/materializacao.py:924`; `app/chamados/services/materializacao.py:1099`), duplicata (`app/chamados/services/fio.py:1350`). O chamador `remover_motivo` já registra DP `RETRATACAO` (`app/chamados/models.py:2053`); `Disputa.fechar` registra `SILENCIO` (`app/chamados/models.py:1921`). T: `app/chamados/tests/test_o261e_f2_porta_carimbo.py:744`; `app/chamados/tests/test_e1_v5_portas_backfill.py:166`; `app/chamados/tests/test_e1_volta2.py:131`.

- `app/chamados/models.py:3192` `PerguntaDisputa.descarimbar` -- C passa a `None/None/''`; recusa linha fora do fato; `salvar=False` só prepara memória -- **efeito**. Persiste em `app/chamados/models.py:3201`; sem DP próprio. P: `app/chamados/models.py:3287`; `app/chamados/models.py:3304`; `app/chamados/models.py:3311`; simulação sem persistência em `app/chamados/models.py:1506`; `app/chamados/services/relavra_via.py:246`. T: `app/chamados/tests/test_o261e_f2_porta_carimbo.py:758`; `app/chamados/tests/test_e1_v5_portas_backfill.py:168`; `app/chamados/tests/test_e1_v5_portas_backfill.py:176`.

- `app/chamados/models.py:3241` `PerguntaDisputa.descarimbar_em_lote` -- C passa a `None/None/''`, mais campos recebidos em `outros`; exclui vias fora do fato -- **efeito**. `QuerySet.update` em `app/chamados/models.py:3253`; sem DP próprio. P: `app/chamados/models.py:1565`; `app/chamados/services/disputa_emissao.py:1627`, cujo chamador registra DP antes do update (`app/chamados/services/disputa_emissao.py:1612`). T: `app/chamados/tests/test_o261e_f2_porta_carimbo.py:752`; `app/chamados/tests/test_e1_v13_ordem.py:90`; `app/chamados/tests/test_e1_v13_ordem.py:101`; `app/chamados/tests/test_e1_v13_ordem.py:106`.

- `app/chamados/models.py:3257` `PerguntaDisputa.encerrar_por_fato_no_espelho` -- somente `via_resolucao=fato_materializado` -- **efeito**. Persiste em `app/chamados/models.py:3265`; sem DP próprio. P: `app/chamados/management/commands/encerrar_perguntas_com_fato.py:68`. T: `app/chamados/tests/test_porta_chamado_e3.py:72`; `app/chamados/tests/test_porta_chamado_e3.py:74`; `app/chamados/tests/test_e1_v5_portas_backfill.py:170`.

- `app/chamados/services/validacao.py:127` `_carimbar_validacao` -- V recebe instante/autor -- **decisao**. Save em `app/chamados/services/validacao.py:137`; DP `VALIDACAO`, mesmo instante/atomic, em `app/chamados/services/validacao.py:139`. P: `_aceitar_a_folga` (`app/chamados/services/validacao.py:288`; `app/chamados/services/validacao.py:320`) e `validar_pergunta` (`app/chamados/services/validacao.py:435`). T: `app/chamados/tests/test_e1_entradas.py:98`.

- `app/chamados/services/validacao.py:163` `devolver_validacao_recusada` -- V passa a nulo, preservando a resposta; pode reabrir o continente pela porta -- **decisao** `DEVOLUCAO`. Save em `app/chamados/services/validacao.py:202`; DP no mesmo atomic em `app/chamados/services/validacao.py:205`. P: `app/chamados/services/validacao.py:464`. T: `app/chamados/tests/test_pvmuda_carimbo_em_voo.py:144`; `app/chamados/tests/test_o261_recusa_fala.py:178`; `app/chamados/tests/test_o261_recusa_fala.py:186`; `app/chamados/tests/test_o261_recusa_fala.py:199`; `app/chamados/tests/test_o261_recusa_fala.py:210`; `app/chamados/tests/test_e1_volta2.py:108`.

- `app/chamados/services/disputa_emissao.py:1422` `responder_pergunta_disputa_core` -- grava R; limpa V anterior; pode descarimbar e gravar recusa explicativa de escape -- **decisao** `RESPOSTA`; limpeza dos carimbos é consequência da resposta nova. Save em `app/chamados/services/disputa_emissao.py:1505`; DP em `app/chamados/services/disputa_emissao.py:1508`, preservando projeção legada sem linha. P: `app/api/views.py:2651`; `app/chamados/views.py:1283`; `app/chamados/services/declaracao_texto.py:419`; `app/chamados/services/declaracao_texto.py:693`; `app/chamados/services/acoes_disputa.py:371`. T: `app/chamados/tests/test_porta_resposta_pergunta.py:177`; `app/chamados/tests/test_porta_resposta_pergunta.py:209`; `app/chamados/tests/test_porta_resposta_pergunta.py:229`; `app/chamados/tests/test_e1_volta2.py:62`; `app/chamados/tests/test_e1_volta2.py:75`.

- `app/chamados/services/disputa_emissao.py:1559` `reabrir_para_correcao` -- limpa R e V; opcionalmente desfaz C da absorção -- **decisao** `REABERTURA` ou `RECUSA`. DP com foto anterior em `app/chamados/services/disputa_emissao.py:1612`; update em `app/chamados/services/disputa_emissao.py:1629`, ou porta de lote em `app/chamados/services/disputa_emissao.py:1627`; emite evento da célula no mesmo atomic. P, incluindo apelidos locais: `app/chamados/models.py:2234`; `app/chamados/views.py:1436`; `app/chamados/services/materializacao.py:682`; `app/chamados/services/validacao.py:593`; `app/chamados/services/acoes_disputa.py:103`; `app/chamados/management/commands/curar_perguntas_em_limbo.py:69`. T: `app/chamados/tests/test_reabrir_nao_destroi.py:42`; `app/chamados/tests/test_reabrir_nao_destroi.py:102`; `app/chamados/tests/test_e1_v13_ordem.py:64`; `app/chamados/tests/test_e1_volta2.py:210`; `app/chamados/tests/test_e1_volta2.py:226`.

- `app/chamados/services/disputa_emissao.py:1635` `responder_automaticamente` -- R recebe texto/instante do fato entregue por outro caminho; não sobrescreve texto existente -- **decisao** de sistema `RESPOSTA`. Save em `app/chamados/services/disputa_emissao.py:1652`; DP em `app/chamados/services/disputa_emissao.py:1653`. P: `app/api/foto_recebida.py:48`. T: `app/chamados/tests/test_porta_chamado_e3.py:46`; `app/chamados/tests/test_porta_chamado_e3.py:48`; `app/chamados/tests/test_porta_chamado_e3.py:53`.

- `app/chamados/services/disputa_emissao.py:1658` `carimbar_resposta_pela_trilha` -- `respondida_em` recebe o instante original testemunhado, sem inventar instante da resposta -- **decisao** reconstruída `RESPOSTA`. Save em `app/chamados/services/disputa_emissao.py:1673`; DP em `app/chamados/services/disputa_emissao.py:1674`. P: `app/chamados/management/commands/curar_perguntas_em_limbo.py:67`. T: `app/chamados/tests/test_curar_perguntas_em_limbo.py:37`; `app/chamados/tests/test_curar_perguntas_em_limbo.py:50`; `app/chamados/tests/test_curar_perguntas_em_limbo.py:51`; `app/chamados/tests/test_porta_resposta_pergunta.py:247`; `app/chamados/tests/test_porta_resposta_pergunta.py:251`; `app/chamados/tests/test_porta_resposta_pergunta.py:262`.

- `app/chamados/services/relavra_via.py:106` `relavrar` -- via derivada atual e L, no mesmo save -- **efeito** para a via; **fica** L como cache. Save em `app/chamados/services/relavra_via.py:127`; sem DP próprio; exclui vias que não pertencem a `VIAS_DERIVADAS` (`app/chamados/services/relavra_via.py:118`). P: `app/chamados/services/relavra_via.py:157`; `app/chamados/management/commands/relavrar_via_i6.py:102`. T: `app/chamados/tests/test_porta_veredito_pergunta.py:363`; `app/chamados/tests/test_porta_veredito_pergunta.py:383`; `app/chamados/tests/test_porta_veredito_pergunta.py:386`; `app/chamados/tests/test_porta_veredito_pergunta.py:404`.

- `app/chamados/models.py:833` `ChamadoColaborador.abrir` -- cria identidade/contexto e `status_local` inicial decidido pelo motor; `get_or_create` em `app/chamados/models.py:971`, defaults em `app/chamados/models.py:938` -- **fica** o nascimento/identidade; não é escritor da tarefa. Sem DP próprio. P: `app/ponto/views.py:3012`; `app/escala/regua_defesa.py:51`; `app/api/views.py:1997`; `app/chamados/reconciliador.py:910`; `app/chamados/views.py:973`; `app/ponto/services/cluster_espurio.py:130`; `app/ponto/services/ausencia.py:659`; `app/ponto/management/commands/detectar_par_relampago.py:368`; `app/ponto/management/commands/reconciliar_grade.py:507`; `app/ponto/management/commands/detectar_ausencias.py:190`; `app/ponto/management/commands/detectar_ausencias.py:271`; `app/ponto/management/commands/detectar_ausencias.py:510`; `app/ponto/management/commands/detectar_ausencias.py:693`; `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:151`; `app/ponto/management/commands/processar_alertas_turno.py:335`; `app/chamados/services/disputa_emissao.py:568`; `app/chamados/services/disputa_emissao.py:1101`; `app/chamados/services/disputa_emissao.py:1336`; `app/chamados/services/adesao.py:74`. T: `app/ponto/tests/test_o_outbox_celula.py:287`; `app/ponto/tests/test_o_outbox_celula.py:555`.

- `app/chamados/models.py:1054` `ChamadoColaborador.criar` -- cria identidade/contexto/status inicial; auditoria aberta pode nascer `registrado` pela decisão do motor (`app/chamados/models.py:1064`); create em `app/chamados/models.py:1095` -- **fica**. Sem DP próprio. P, incluindo `_Chamado`, `_Ch9` e `_ChG`: `app/colaboradores/views.py:1978`; `app/colaboradores/views.py:2363`; `app/ponto/utils.py:40`; `app/ponto/utils.py:103`; `app/ponto/views.py:291`; `app/api/views_core.py:747`; `app/api/views.py:747`; `app/api/views.py:2201`; `app/colaboradores/services/vinculo.py:393`; `app/colaboradores/services/desligamento.py:163`; `app/ponto/services/justificativa.py:291`; `app/ponto/services/geofence.py:339`; `app/ponto/services/triagem_batida.py:107`; `app/ponto/services/triagem_batida.py:336`; `app/ponto/services/triagem_batida.py:459`; `app/ponto/management/commands/processar_alertas_avancados.py:121`; `app/chamados/services/regularizacao_ext.py:246`; `app/chamados/services/abertura.py:47`; `app/chamados/services/abertura.py:172`. Ensaio: `app/chamados/management/commands/smoke_chamados.py:123`. T: `app/colaboradores/tests/test_vincular_escala_selos.py:26`; `app/ponto/tests/test_cobertura_followup.py:45`.

- `app/chamados/services/disputa_emissao.py:474` `_abrir_ou_agregar_disputa` -- cria continente, motivos/contexto e estado inicial aberto, `fechada_em=NULL` pelo campo; create em `app/chamados/services/disputa_emissao.py:587` -- **fica**. Default em `app/chamados/models.py:1754`; sem DP próprio. P: `app/chamados/services/disputa_emissao.py:627`; `app/chamados/services/disputa_emissao.py:662`; `app/ponto/management/commands/reconciliar_grade.py:424`. Ensaio: `app/chamados/management/commands/smoke_chamados.py:99`. T: `app/chamados/tests/test_e1_fila_v2_2_integracao.py:132`.

- `app/chamados/services/disputa_emissao.py:1024` `criar_disputa_a_partir_de_chamados_orfaos` -- cria continente aberto, identidade, motivos/contexto; create em `app/chamados/services/disputa_emissao.py:1120` -- **fica**. `fechada_em=NULL` pelo default (`app/chamados/models.py:1754`); sem DP próprio. P: `app/ponto/management/commands/processar_alertas_avancados.py:159`; `app/ponto/management/commands/detectar_ausencias.py:802`; `app/ponto/management/commands/processar_alertas_turno.py:385`; `app/chamados/services/cobrar_dia.py:49`; `app/chamados/services/pergunta_no_app.py:208`; `app/chamados/services/acoes_disputa.py:222`. T: `app/chamados/tests/test_chamados_models.py:135`; `app/chamados/tests/test_chamados_models.py:155`; `app/chamados/tests/test_chamados_models.py:157`; `app/tenants/tests.py:74`; `app/tenants/tests.py:85`; `app/chamados/tests/test_e1_fila_v2_2_integracao.py:78`; `app/chamados/tests/test_e1_fila_v2_2_integracao.py:244`.

- `app/chamados/services/disputa_emissao.py:1299` `criar_disputa_ancora_de_chamado` -- cria continente aberto e vínculo com chamado, motivos/contexto; create em `app/chamados/services/disputa_emissao.py:1355` -- **fica**. `fechada_em=NULL` pelo default (`app/chamados/models.py:1754`); sem DP próprio. P: `app/chamados/services/acoes_disputa.py:189`. T: `app/chamados/tests/test_fio_ancora.py:74`; `app/chamados/tests/test_fio_ancora.py:92`; `app/chamados/tests/test_fio_ancora.py:93`; `app/chamados/tests/test_fio_ancora.py:104`; `app/chamados/tests/test_fio_ancora.py:151`; `app/chamados/tests/test_3b_reuso_1061.py:30`.

- `app/chamados/models.py:2822` `PerguntaDisputa._nascer` -- insere identidade, dia, instância, célula,

[truncado em 20000 de 75733 caracteres]

## 09/11 10:02 · sessao 0516585baa5b · - · - · origem=Ronald (tmux)

Inventário enviado em mapas 1–4, com extensões de autoria, silêncio, identidade e datas, sempre com referências de código.

Divergências principais:

- `materializada_em` significa processamento, inclusive recusa sem batida; não prova materialização de fato. `app/chamados/models.py:3382`; `app/chamados/services/materializacao.py:435`; `app/chamados/services/raiox.py:175`.
- `estado`, `q_vivas`, vivacidade e pendência no app respondem perguntas diferentes. `app/chamados/models.py:3030`; `app/chamados/models.py:3325`; `app/chamados/services/pergunta_no_app.py:67`.
- `data_evento` guarda dia do turno ou dia civil da conversa; leitores remanescentes sem discriminador já estão declarados no censo. `app/chamados/services/fio.py:1380`; `app/chamados/services/fio.py:1608`.
- Respostas incluem hora, data/hora, escapes, escolhas e identificadores; cada consumidor precisa preservar o discriminador. `app/chamados/services/materializacao.py:356`; `app/chamados/services/materializacao.py:995`; `app/chamados/models.py:3512`.
- Janela viva inclui ambas as extremidades; silêncio termina no instante limite; expiração usa dia local. `app/chamados/juizes.py:601`; `app/chamados/models.py:511`; `app/chamados/veredito_lavrado.py:116`.

O ajuste noturno atual já lê dia do turno e cadeia de marcos; o antigo recuo civil está removido. `app/chamados/juizes.py:296`; `app/chamados/services/materializacao.py:418`.

O inventário sintático não certifica leitores dinâmicos, templates, SQL cru ou execução em produção. [não verificado]

## 09/11 10:00 · sessao 502871ba1ad1 · - · - · origem=Ronald (tmux)

Chamadores por AST dos seis nascimentos:

- `Chamado.abrir`: `app/ponto/views.py:3012`; `app/escala/regua_defesa.py:51`; `app/api/views.py:1997`; `app/chamados/reconciliador.py:910`; `app/chamados/views.py:973`; `app/ponto/services/cluster_espurio.py:130`; `app/ponto/services/ausencia.py:659`; comandos de ponto `detectar_par_relampago.py:368`, `reconciliar_grade.py:507`, `detectar_ausencias.py:190`, `:271`, `:510`, `:693`, `emitir_turno_aberto_retroativo.py:151`, `processar_alertas_turno.py:335`; `app/chamados/services/disputa_emissao.py:568`, `:1101`, `:1336`; `app/chamados/services/adesao.py:74`. Testes exemplares: `app/ponto/tests/test_o_outbox_celula.py:287`, `:555`.
- `Chamado.criar`: `app/colaboradores/views.py:1978`, `:2363`; `app/ponto/utils.py:40`, `:103`; `app/ponto/views.py:291` (`_Chamado`); `app/api/views_core.py:747` e `app/api/views.py:747` (`_Ch9`); `app/api/views.py:2201`; `app/colaboradores/services/vinculo.py:393`; `app/colaboradores/services/desligamento.py:163`; `app/ponto/services/justificativa.py:291` (`_Chamado`); `app/ponto/services/geofence.py:339` (`_ChG`); `app/ponto/services/triagem_batida.py:107`, `:336`, `:459`; `app/ponto/management/commands/processar_alertas_avancados.py:121`; `app/chamados/services/regularizacao_ext.py:246`; `app/chamados/services/abertura.py:47`, `:172`. Ensaio: `app/chamados/management/commands/smoke_chamados.py:123`. Testes exemplares: `app/colaboradores/tests/test_vincular_escala_selos.py:26`, `app/ponto/tests/test_cobertura_followup.py:45`.
- `_abrir_ou_agregar_disputa`: `app/chamados/services/disputa_emissao.py:627` (`disputa_para_declaracao`), `:662` (`criar_ou_atualizar_disputa`); `app/ponto/management/commands/reconciliar_grade.py:424`. Ensaio: `app/chamados/management/commands/smoke_chamados.py:99`; teste com alias `de`: `app/chamados/tests/test_e1_fila_v2_2_integracao.py:132`.
- `criar_disputa_a_partir_de_chamados_orfaos`: comandos de ponto `processar_alertas_avancados.py:159`, `detectar_ausencias.py:802`, `processar_alertas_turno.py:385`; `app/chamados/services/cobrar_dia.py:49`, `pergunta_no_app.py:208`, `acoes_disputa.py:222`. Testes: `app/chamados/tests/test_chamados_models.py:135`, `:155`, `:157`; teste fora diretório `tests/`: `app/tenants/tests.py:74`, `:85`; alias `de`: `app/chamados/tests/test_e1_fila_v2_2_integracao.py:78`, `:244`.
- `criar_disputa_ancora_de_chamado`: produção `app/chamados/services/acoes_disputa.py:189`; testes `app/chamados/tests/test_fio_ancora.py:74`, `:92`, `:93`, `:104`, `:151`; `app/chamados/tests/test_3b_reuso_1061.py:30`.
- `Pergunta._nascer`: único chamador direto `app/chamados/models.py:2817`, `_sob_contrato`; este recebe `abrir:2786` e `abrir_conversa:2798`. Pergunta datada nasce por `abrir`; conversa usa dia civil `localdate`, explicitamente (`app/chamados/models.py:2791`).

Correlatos pedidos:

- `app/chamados/models.py:2179` `reabrir_por_colab`: `save:2237` grava somente `reaberturas_colab=usadas+1`; delega reabertura da resposta em `2234`, mesmo atomic de `2232`, gerando DP pela porta delegada. Não reabre disputa. Produção: `app/api/views.py:2710`, `app/chamados/views.py:1334`; testes: `app/chamados/tests/test_porta_disputa_ciclo.py:285`, `:302`, `:303`, `:315`, `:335`, `:343`, `:355`; `test_e1_v13_cadeia.py:125`.
- `app/chamados/models.py:2247` `limpar_resolucao_residual`: `save:2263` grava apenas `resolucao=''`; exige disputa aberta com texto (`2259`); trilha em `2266`. Nenhum chamador AST localizado, nem produção nem teste; chamada dinâmica: [não verificado].
- `app/chamados/models.py:1968` `remover_motivo`: `save:2062` grava `motivos/contexto`; DP `RETRATACAO:2053` e `carimbar_identidade:2058`, mesmo atomic. Não apaga perguntas na cópia. Produção: `app/colaboradores/signals.py:77`, `app/escala/signals.py:161`; testes exemplares `test_e1_entradas.py:138`, `:149`, `:163`; `test_e1_volta2.py:257`, `:281`; `test_porta_remocao_motivo.py:255`.
- `app/chamados/models.py:1949` `aparar_motivo_respondido`: `save:1965` grava somente `motivos/contexto`, sem fechar disputa; produção `app/api/foto_recebida.py:72`. O comentário que diz que `remover_motivo` apaga está desatualizado (`1952`), contradito pelo código de retração (`2053`).
- `Chamado.apagar_em_ensaio`: chamador em arquivo de comando, **não teste Django**: `app/ponto/management/commands/zerar_dados_operacionais.py:84`; execução é ensaio condicionado pela própria porta `app/chamados/models.py:563`.

Estado inicial de Chamado: `abrir` monta `status_local=_d.novo_estado` em `app/chamados/models.py:938`; `criar` converte aberto para estado do motor em `1064`. Disputa inicia `fechada_em=NULL` pelo campo `app/chamados/models.py:1754`.

## 09/11 09:58 · sessao 502871ba1ad1 · - · - · origem=Ronald (tmux)

AST confirma o núcleo histórico de **16 portas = 1 Chamado + 3 Disputa + 12 Pergunta**, desde que se conte alteração dos campos principais e se explicitem separadamente nascimentos, executores genéricos e complementos. O próprio RELATO distingue “16”, “24” e “mais 3 de identidade/data”. `app/docs/RELATO.md:242`

| Porta (definição) | Persistência / campos | DP junto |
|---|---|---|
| `app/chamados/models.py:201` `_gravar_estado` | `save:296`: `status_local`, `resolvido_em`, `via_encerramento`, `atualizado_em`; autor e urgência condicionais (`228–253`) | Não direto |
| `app/chamados/models.py:1852` `Disputa.fechar` | `save:1933`: `fechada_em=now`, `fechada_por`, `resolucao`, `motivo_fechamento` | Só exceção que carimba perguntas: `SILENCIO:1921` |
| `app/chamados/models.py:2123` `reabrir_admin` | `save:2158`: zera os quatro campos do fechamento; incrementa `reaberturas_admin`; pode agregar `motivos` | Não direto |
| `app/chamados/models.py:2160` `reabrir_sistema` | `save:2174`: zera os quatro campos do fechamento | Não direto |
| `app/chamados/models.py:3092` `carimbar` | `save:3141`: `materializada_em/por`, `via_resolucao`; pode completar resposta/validação e lavrar cache | Não direto |
| `app/chamados/models.py:3157` `carimbar_identidade` | `save:3179`: trio do carimbo + `recusa_motivo` opcional | Não direto |
| `app/chamados/models.py:3192` `descarimbar` | `save:3201`: trio `None/None/''`; `salvar=False` só prepara campos | Não direto |
| `app/chamados/models.py:3241` `descarimbar_em_lote` | `update:3253`: trio `None/None/''` + `outros`; exclui vias fora do fato | Não próprio; chamador pode registrar |
| `app/chamados/models.py:3257` `encerrar_por_fato_no_espelho` | `save:3265`: somente `via_resolucao=fato_materializado` | Não direto |
| `app/chamados/services/validacao.py:127` `_carimbar_validacao` | `save:137`: `validada_em=now`, `validada_por=autor` | `VALIDACAO:139`, mesmo atomic |
| `app/chamados/services/validacao.py:163` `devolver_validacao_recusada` | `save:202`: `validada_em/por=None` | `DEVOLUCAO:205`, mesmo atomic |
| `app/chamados/services/disputa_emissao.py:1422` `responder_pergunta_disputa_core` | `save:1505`: resposta/texto+instante; zera validação se mudou; descarimbo em memória e recusa especial condicionais | `RESPOSTA:1508` |
| `app/chamados/services/disputa_emissao.py:1559` `reabrir_para_correcao` | `update:1629`: resposta vazia e resposta/validação nulas; com absorção delega descarimbo `1627` | `registrar:1612`, ato `reabertura` ou `recusa` |
| `app/chamados/services/disputa_emissao.py:1635` `responder_automaticamente` | `save:1652`: texto+instante somente sem texto anterior | `RESPOSTA:1653` |
| `app/chamados/services/disputa_emissao.py:1658` `carimbar_resposta_pela_trilha` | `save:1673`: `respondida_em=instante` testemunhado pela trilha | `RESPOSTA:1674` |
| `app/chamados/services/relavra_via.py:106` `relavrar` | `save:127`: via calculada hoje + trio cache | Não direto |

**Oito complementos**, fechando o recorte de 24 do RELATO:

- `app/chamados/services/materializacao.py:784` `_materializar_resposta_hora`: `save:851` grava **somente** `pendencia_admin/recusa_motivo`; nesse ponto não grava carimbo.
- `app/chamados/services/materializacao.py:1083` `_carimbar_recusa`: `save:1103` grava **somente** `recusa_motivo`; colisão delega carimbo de identidade em `1098`.
- `app/chamados/services/validacao.py:533` `recusar_pergunta`: `save:590` grava `recusa_motivo`; alteração da resposta delegada à reabertura, com ato recusa.
- `app/chamados/services/validacao.py:625` `conferir_data_do_fato`: `save:666` grava `data_conferida_em/por`; humano aceita `data_evento` quando diverge do chamado, com `LogAuditoria:667`; sem DP direto.
- `app/chamados/veredito_lavrado.py:240` `gravar_lavras_em_lote`: `bulk_update:250`, trio cache.
- `app/chamados/veredito_lavrado.py:254` `lavrar`: `save:275`, `veredito_lavrado/julgado_em/janela_expira_em`.
- `app/chamados/services/arquivo.py:141` `silenciar`: `save:171`, prazo/autor/motivo do silêncio+atualização.
- `app/chamados/services/arquivo.py:177` `levantar`: `save:210`, limpa silêncio; `resgatado_em` opcional. O módulo declara silêncio como véu, sem alterar estado (`154`).

**Três identidade/data**, fechando o recorte de 27:

- `app/chamados/models.py:2822` `_nascer`: `save:2998` atualiza célula/destino+cache no reuso; `save(force_insert=True):3008` insere pergunta.
- `app/chamados/services/fio.py:804` `reapontar_resolvedora`: `save:924`, destino/costura e cache.
- `app/chamados/services/fio.py:970` `rechavear_data_evento`: `save:1034`, dia/célula e cache.

**O pedido literal inclui `create/delete`; não esconder estes por trás do número 16:**

- `app/chamados/models.py:833` `Chamado.abrir`: `get_or_create:971`, estado inicial pelo `defaults`.
- `app/chamados/models.py:1054` `Chamado.criar`: `create:1095`, `status_local=status`; motor converte aberto de auditoria para registrado (`1059–1064`).
- `app/chamados/services/disputa_emissao.py:474` `_abrir_ou_agregar_disputa`: `create:587`.
- `app/chamados/services/disputa_emissao.py:1024` `criar_disputa_a_partir_de_chamados_orfaos`: `create:1120`.
- `app/chamados/services/disputa_emissao.py:1299` `criar_disputa_ancora_de_chamado`: `create:1355`.
- Esses três criadores de disputa iniciam `fechada_em=NULL` pelo default do campo (`app/chamados/models.py:1754`).
- `app/chamados/models.py:3402` `Pergunta.save`: `super().save:3412` é executor genérico e garante atomicidade com outbox, além de derivar identidade na inserção (`3410`); não representa novo ato semântico.
- `app/chamados/models.py:563` `Chamado.apagar_em_ensaio`: `delete:571`, explicitamente porta de ensaio.

**Exclusões por ausência de persistência:**

- `_vivas_entre`: troca `status_local` em memória e restaura (`app/chamados/models.py:356`, `:371`).
- `_universo_da_ressuscitacao`: simula validação/resposta nulas (`app/chamados/models.py:1507`).
- `_escrever_carimbo`: só atribui em memória (`app/chamados/models.py:3086`).
- `_desfazer_em_memoria`: só rollback da instância (`app/chamados/models.py:3148`).

Varredura AST global dos campos principais não localizou persistência principal de Pergunta/Disputa/Chamado fora de `chamados/`; `regularizacao_ext.reg.materializada_em` pertence a outro modelo (`app/chamados/services/regularizacao_ext.py:198`). Completude frente a despacho inteiramente dinâmico que não exponha modelo nem campo no AST: [não verificado].

## 09/11 09:56 · sessao 8573dd838240 · - · - · origem=Ronald (tmux)

Autoridade lida e critério:

- **DECISÃO** = fato próprio do ato humano/sistema; sequência única imutável, com instante, autor, texto e motivo; a projeção atual será encolhida pela E4. Atos existentes: resposta, validação, devolução, recusa, reabertura, silêncio, retratação e absorção. `app/chamados/models.py:3828`, `app/chamados/models.py:3850`.
- **EFEITO** = consequência cuja ocorrência precisa ser conferida externamente; resposta validada não equivale a materialização. E2 distingue sem lavra vigente, validada sem efeito, efeito ainda não visto pela célula e encerrada pelo juiz. `app/chamados/services/tarefa_do_dia.py:118`, `app/chamados/services/tarefa_do_dia.py:140`.
- **FICA** = identidade/data ou cache reconstruível: identidade da pergunta em `app/chamados/models.py:2424`; dia do turno atribuído exclusivamente por `gravar_dia_do_fato`, sem persistência própria, em `app/chamados/services/fio.py:790`; cache explícito da lavra em `app/chamados/veredito_lavrado.py:58`, `app/chamados/veredito_lavrado.py:74`.
- `DecisaoPergunta.registrar` é a porta única da sequência; update/delete/bulk_update e edição de linha existente são recusados. `app/chamados/models.py:3814`, `app/chamados/models.py:3892`, `app/chamados/models.py:3931`.

Ramos que impedem classificação ingênua por função:

- `DisputaSupervisao.fechar`: default é **efeito** da inexistência de pergunta viva/resposta sem veredito; exceções `desligamento`/`admin_encerra` são **decisão** e já registram `SILENCIO` E1. `app/chamados/models.py:1855`, `app/chamados/models.py:1892`, `app/chamados/models.py:1915`.
- `DisputaSupervisao.remover_motivo`: **decisão** `RETRATACAO` já registrada; carimbo terminal da pergunta e atualização da lista de motivos são consequências do ato. `app/chamados/models.py:2043`, `app/chamados/models.py:2053`.
- `_materializar_ausencia_de_resposta`: vínculo de absorção já é fato `ABSORCAO` E1, com escape e ausência; carimbo da pergunta vem no mesmo atomic. `app/chamados/services/materializacao.py:549`, `app/chamados/services/materializacao.py:557`.
- `responder_automaticamente`: **decisão** `RESPOSTA` de sistema sobre fato entregue por outro caminho, não cache descartável. `app/chamados/services/disputa_emissao.py:1635`, `app/chamados/services/disputa_emissao.py:1653`.
- `carimbar_resposta_pela_trilha`: **decisão** reconstruída com o instante original testemunhado pela nota, não instante atual da reparação. `app/chamados/services/disputa_emissao.py:1658`, `app/chamados/services/disputa_emissao.py:1674`.

Divergências semânticas para DECISÕES numeradas:

1. **Carimbo de processamento ≠ prova de batida/fechamento.** Help text de `materializada_em` diz “batida criada + chamado fechado”; `carimbar_identidade` também escreve esse campo para identidade sem plantio e pode manter instante/autor anteriores. A E2 exige testemunha/lavra vigente. `app/chamados/models.py:3382`, `app/chamados/models.py:3157`, `app/chamados/models.py:3171`, `app/chamados/services/tarefa_do_dia.py:118`.
2. **Estado visual por timestamps ≠ exigibilidade do ato.** `estado` chama validada+materializada de resolvida; E2 distingue validação sem efeito ou sem lavra vigente. `app/chamados/models.py:3030`, `app/chamados/services/tarefa_do_dia.py:140`.
3. **`data_evento` = dia do turno, não dia civil da batida.** Help text antigo fala data do evento; identidade e E2 explicitam dia do turno. `app/chamados/models.py:2420`, `app/chamados/models.py:2424`, `app/chamados/services/tarefa_do_dia.py:213`, `app/chamados/services/fio.py:790`.
4. **Expiração = dia civil local; julgamento = instante.** `relogio_virou` usa `janela_expira_em <= hoje_local`, mas a lavra só vence pelo relógio quando `localdate(julgado_em) < janela_expira_em`. Não se compara a data UTC. `app/chamados/veredito_lavrado.py:95`, `app/chamados/veredito_lavrado.py:107`.
5. **Janela inclusiva e expiração exclusiva.** Janela viva inclui início da anterior menos um dia e fim da corrente; primeiro dia fora é a expiração. Não corresponde a duração fixa de 30/60 dias. `app/chamados/veredito_lavrado.py:119`, `app/chamados/veredito_lavrado.py:145`.
6. **Resposta/validação legadas sem linha ≠ decisão E1 demonstrada.** E1 preserva a projeção anterior como linhas antes de sobrescrevê-la; E2 devolve `passivo_sem_linha`, sem inventar ação. `app/chamados/models.py:3903`, `app/chamados/services/tarefa_do_dia.py:127`.
7. **`via_resolucao` não significa sempre declaração de hora.** Escape ATESTADO/FALTA_DIA e irmã muda usam ausência; o escritor pergunta `declarou_hora`, e a sequência liga a absorção à ausência concreta. `app/chamados/services/materializacao.py:534`, `app/chamados/services/materializacao.py:549`.
8. **Fechamento de disputa ≠ julgamento de resposta.** Default só fecha após inexistência de viva e resposta sem veredito; fechamento excepcional decide silêncio e registra E1. `app/chamados/models.py:1823`, `app/chamados/models.py:1852`, `app/chamados/models.py:1915`.

Leis/cortes conferidos: L-002 (`app/docs/LEIS.md:60`), L-007 (`:65`), L-051 (`:92`), L-061 (`:95`), L-073 (`:100`), L-080 (`:107`), L-085 (`:117`), L-088 (`:120`), L-110 (`:136`), L-111 (`:137`), L-117 (`:138`), L-124 (`:145`), L-113 (`:151`); FIO-MORTO-E-ESTRUTURAL (`app/docs/CORTES.md:253`), FIO-E0-V4 (`:426`), LEI-TRANCA-E2-JUIZ (`:427`). Essas autoridades respondem os pontos examinados; **perguntas de lei: 0** neste recorte. Não concluí censo integral de escritores/leitores, responsabilidade dos outros levantamentos. [não verificado]

## 09/11 09:55 · sessao 3fe3faa2e17b · FIO-E0-V4-E3 · construtor · origem=Code

Entrega: [F1–F8, causas, hunks, escritores/leitores e comandos de RED/mutantes](/home/ronald/wt-codex-fio-e0-v4-e3-084151/app/docs/ENTREGA_FIO_E0_V4_E3_V3.md:5).

**F2: código. F1/F3/F4/F5/F6/F7/F8: selo.**

`py_compile`: **10 arquivos, rc 0**, sem bytecode; `git diff --check`: rc 0. [Verificação e comandos](/home/ronald/wt-codex-fio-e0-v4-e3-084151/app/docs/ENTREGA_FIO_E0_V4_E3_V3.md:70). Suíte não executada; mordida dos RED/mutantes **[nao verificado]**. Ruff indisponível. [nao verificado]

**DECISAO-ABERTA:** DIFF/L-099 antes da fachada; crons e diagramas com a sessão. [Referência](/home/ronald/wt-codex-fio-e0-v4-e3-084151/app/docs/ENTREGA_FIO_E0_V4_E3_V3.md:90).

## 09/11 09:17 · sessao bbec4829eb42 · BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS · construtor · origem=Code

5. Entrega no [relatório por hunk, escritores/leitores e comandos individuais](docs/BUG-PAR-INTERVALO-V2.txt:9).

- **DECISÃO-ABERTA — P2:** ligação de C7 ao predicado reservada à sessão; import e linha exata em [entrega:205](docs/BUG-PAR-INTERVALO-V2.txt:205). Teste: [test_P2_C7_nao_fecha_se_irma_sai_do_fato](chamados/tests/test_bug_par_intervalo.py:388).
- **Verificação:** `py_compile` sem persistência retornou rc 0; âncoras e sintaxe de M1–M37 conferidas. [entrega:301](docs/BUG-PAR-INTERVALO-V2.txt:301)
- **Sem execução da suíte ou dos mutantes:** RED/GREEN e mortes por asserção **[nao verificado]**. F1–F4 e produção permanecem fora da cobertura. [entrega:218](docs/BUG-PAR-INTERVALO-V2.txt:218)

## 09/11 09:10 · sessao 34f994372353 · SONDA-SEM-GRANT · verificador · origem=Code

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 09/11 09:08 · sessao 790af1e10d16 · - · - · origem=Ronald (tmux)

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 09/11 09:08 · sessao be6e6f7b42d9 · - · - · origem=Ronald (tmux)

- **P1 ainda PROCEDENTE, fechamento parcial.** A conferência passou a contar ACL direto/PUBLIC de tabelas e colunas, sequências, schemas, banco e defaults; também membership e ownership. Porém ignora `is_grantable`: SELECT/USAGE/CONNECT com grant option continua considerado permitido, embora habilite DCL. Conserto mínimo: contar `e.is_grantable OR e.privilege_type <> '<permitido>'` nas respectivas chaves e acrescentar caso comportamental ao selo. (`bin/sonda_leitor_comum.sh:87`, `bin/sonda_leitor_comum.sh:101`, `bin/sonda_leitor_comum.sh:107`; validade em PostgreSQL 17 executado: [nao verificado])

- **Cura de P1 incompleta, com recusa segura.** O provisionador revoga ACL de tabela/sequência/schema do leitor e defaults de `saas_user`; não remove ACL de coluna, DML de PUBLIC em tabelas/sequências, defaults de outros donos nem defaults de schema. A conferência verifica essas classes mais amplas e a pós-conferência impede publicação quando restam privilégios. Conserto mínimo: explicitar que esses casos exigem saneamento separado, ou revogá-los deliberadamente pela porta, contemplando o impacto nos demais papéis. (`bin/sonda_provisionar.sh:105`, `bin/sonda_provisionar.sh:110`, `bin/sonda_provisionar.sh:129`; `bin/sonda_leitor_comum.sh:92`, `bin/sonda_leitor_comum.sh:107`)

- **P3: fechado no fluxo serial; concorrência permanece PROCEDENTE.** O arquivo só é criado/promovido depois da pós-conferência, mediante temporário único e `mv`; falhas anteriores preservam o arquivo anterior. Contudo a senha é escolhida antes das alterações no banco e não há exclusão entre duas chamadas: dois provisionamentos sem arquivo podem escolher senhas distintas e ordenar ALTER ROLE/mv de formas incompatíveis. Conserto mínimo: trava compartilhada entre destinos envolvendo leitura/geração da senha até promoção do arquivo. (`bin/sonda_provisionar.sh:63`, `bin/sonda_provisionar.sh:66`, `bin/sonda_provisionar.sh:88`, `bin/sonda_provisionar.sh:129`, `bin/sonda_provisionar.sh:140`, `bin/sonda_provisionar.sh:143`)

- **P5: correção estática presente.** A senha admite apenas caracteres alfanuméricos, entra por `\set pw` e é referenciada como `:'pw'` fora de DO; stdout é descartado e stderr é mascarado antes de voltar ao chamador. Conserto adicional para esse achado específico: nenhum identificado na análise estática. Execução e comportamento de logs do PostgreSQL real: [nao verificado]. (`bin/sonda_provisionar.sh:70`, `bin/sonda_provisionar.sh:78`, `bin/sonda_provisionar.sh:88`, `bin/sonda_provisionar.sh:92`)

- **ESMERIL-SEMANTICA: a chave `ro` tem um sentido compartilhado, mas “pronto” operacional diverge.** `ro` significa somente configuração global da role, com `setdatabase=0`; leitura e provisionamento usam o mesmo produtor/parser. Entretanto a leitura exige também `default_transaction_read_only=on` na sessão TCP, enquanto o provisionador altera apenas a configuração global e anuncia pronto sem esse login. Um override específico de banco não é inspecionado nem removido por esses trechos. Conserto mínimo: conferir/sanear o override do destino e exigir o mesmo login antes do `mv`; o fake atual devolve invariavelmente `leitor|on`. (`bin/sonda_leitor_comum.sh:64`, `bin/sonda_leitor_comum.sh:119`; `bin/sonda_provisionar.sh:93`, `bin/sonda_provisionar.sh:129`, `bin/sonda_provisionar.sh:146`; `bin/sonda_leitura.sh:205`, `bin/sonda_leitura.sh:211`; `bin/tests/test_sonda_sem_grant.sh:60`)

- **Demais chaves: identidade compartilhada, sem tradução encontrada nesses consumidores.** `role/super/poderes/login` vêm de `pg_roles`; `connect` verifica CONNECT efetivo; `usage_falta`, `tab_falta` e `seq_falta` contam ausência de leitura em `public juliani`; `default_falta` exige os quatro pares schema×tipo de defaults de `saas_user`; `esc_*` contam ACL diferente da operação permitida; `default_escrita` inspeciona defaults de qualquer dono; `membro` conta memberships diretos; `dono` conta dependências de ownership no cluster. O parser exige as 17 chaves e compara presença com 1 e ausência com 0. (`bin/sonda_leitor_comum.sh:60`, `bin/sonda_leitor_comum.sh:69`, `bin/sonda_leitor_comum.sh:80`, `bin/sonda_leitor_comum.sh:87`, `bin/sonda_leitor_comum.sh:107`, `bin/sonda_leitor_comum.sh:111`, `bin/sonda_leitor_comum.sh:119`, `bin/sonda_leitor_comum.sh:126`)

- **SQL PostgreSQL 17: nenhum erro sintático identificado nesses pontos por análise estática; execução real [nao verificado].** `printf` transforma `%%I` em `%I` para `format`; os comandos dinâmicos estão em `DO ... EXECUTE`; as variáveis psql ficam fora de corpos dollar-quoted; defaults usam `FOR ROLE saas_user`, coerente com `PGU`. O selo fake grava texto e retorna resultados preparados, sem interpretar esse SQL. (`bin/sonda_provisionar.sh:88`, `bin/sonda_provisionar.sh:92`, `bin/sonda_provisionar.sh:94`, `bin/sonda_provisionar.sh:105`, `bin/sonda_provisionar.sh:110`, `bin/sonda_provisionar.sh:120`; `bin/sonda_leitor_comum.sh:8`; `bin/tests/test_sonda_sem_grant.sh:56`, `bin/tests/test_sonda_sem_grant.sh:59`, `bin/tests/test_sonda_sem_grant.sh:71`)

## 09/11 09:08 · sessao 437ffdfdf483 · - · - · origem=Ronald (tmux)

[resposta retida: senha_atribuida -- nao se publica credencial nem caminho de segredo]

## 09/11 09:03 · sessao cc20c7413249 · CODEX-2-RAIAS-PORTAO-CPU · leitura · origem=Code

**O contrato cobre o núcleo do aval, mas deixa lacunas de execução; a cópia ainda implementa uma única vaga, sem os quatro portões da segunda.** (contrato.txt:2-6; bin/codex_pergunta.sh:228-239.)

Nas referências aos anexos, contei o cabeçalho do contrato como linha 1, os cinco itens como linhas 2–6 e o aval como linha 1. São referências ao texto fornecido; presença desses arquivos na cópia: [nao verificado].

**1. Cobertura de cada exigência literal**

Todas as exigências abaixo vêm de `aval_codex_2_raias_portao_cpu.txt:1`.

| Exigência do aval | Atendimento no contrato |
|---|---|
| “até 2 construtores Codex simultâneos” | Duas vagas nomeadas; a segunda só é tentada quando a primeira está presa. Falta definir a coordenação que garante essa condição durante a abertura. (contrato.txt:2.) |
| “2a vaga SO abre com load de 5 min < 6 (nproc 8 - 2)” | Prevê `load5 < nproc-2`; a mensagem exemplificada usa `>=6`. Equivale ao corte 6 quando `nproc=8`; valor efetivo no host: [nao verificado]. (contrato.txt:2-3.) |
| “medido no /proc/loadavg na hora da abertura” | Fonte e momento constam; ilegibilidade recusa. (contrato.txt:2,4.) |
| “fora de 06:00-07:30” | Consta, mas sem fuso ou definição da inclusão exata de 07:30. (contrato.txt:2-3.) |
| “sem arquivo em comum com a 1a raia” | Prevê interseção, `--toca` obrigatório para a segunda e reconferência final. Não define quando a primeira publica/atualiza o conjunto nem quanto tempo o preserva para a outra conferir. (contrato.txt:4.) |
| “teto total de construtores = 3 (2 Codex + 1 opus)” | Duas vagas Codex e opus 1 por conduta compõem esse teto; não há guarda conjunta prometida. A cópia também declara opus como conduta. (contrato.txt:2,4; CLAUDE.md:350-353.) |
| “Suite inteira continua serializada pela trava unica” | Preservado expressamente; o instrumento existente prende `/tmp/juliani_db_test.lock` durante o comando. (contrato.txt:4; bin/trava_teste.sh:21,35-42.) |
| “Medida (…) no RELATO 1x por dia” | Os quatro indicadores e a frequência constam. Faltam responsável, acionamento e modo de produzir média diária, ocupação e `%/h`. (contrato.txt:5.) |
| “load medio do dia passar de 7” | Mesmo limiar, avaliado na abertura da segunda vaga. Não descreve o tratamento de uma segunda vaga já em execução. (contrato.txt:6.) |
| “Codex passar de 85% antes do reset, volta a 1 sozinho” | Mesmo limiar e condição; não especifica qual janela de `used_percent`, validade da leitura ou duração do desligamento. (contrato.txt:6.) |
| “wrapper recusa a 2a vaga sem o portao”; mutante removido vermelho | Repetido expressamente, mas ainda sem esse caso no selo atual. (contrato.txt:6; bin/tests/test_codex_pergunta.sh:410-415,437-505.) |
| Custo: carga adicional e cota “~2x mais rapido” | Não repetido nas cinco cláusulas. Comprovação desse custo pelo dado fornecido: [nao verificado]. (contrato.txt:2-6.) |
| “Segue a fila; PAREI so em lei ou !” | Não repetido nas cinco cláusulas; elas tampouco descrevem alteração dessa orientação. (contrato.txt:2-6.) |

As fontes de medida precisam ser distinguidas: `ler_semanal()` entrega porcentagem e duração da janela, **sem `resets_at`**; `leituras_semanais()` já oferece histórico com hora e reset da janela semanal principal. `statusline_cota.py` grava uma fotografia de porcentagem/hora/reset, sobrescrevendo a anterior, e não calcula `%/h`. A trava registra ocupante e início atuais, sem produzir histórico de ocupação. (bin/codex_limite_estado.py:118-123,131-161; bin/statusline_cota.py:45-63; bin/trava_teste.sh:40-41.)

**2. Onde os portões entram e qual estado falta**

O sítio comum vem do `git-common-dir`, em `#G:vaga_sitio`; não é simplesmente `$R`, `$VIVO` ou `$LOGD`. A abertura atual usa fd 6 em append, seguida de `flock -n`; sua falha encerra a chamada em `#G:vaga`. **É nesse ramo de falha que o contrato coloca load, janela, arquivos e desligador, antes da admissão na vaga 2.** Não há esse ramo adicional hoje. (bin/codex_pergunta.sh:72-78,228-239; contrato.txt:2.)

- **Load e janela:** pertencem à decisão de abertura da vaga 2. Depois da vaga atual ainda há espera pelo slot geral e pela trava da resposta; portanto abertura da vaga e início do Codex são momentos distintos. (contrato.txt:2; bin/codex_pergunta.sh:341-369,394-398.)
- **Arquivo em comum:** comparação na abertura e reconferência junto ao inventário final, antes da liberação da resposta para triagem. O descarte atual por inventário já ocorre antes do `.id`. (contrato.txt:4; bin/codex_pergunta.sh:413-426,442-445.)
- **Desligador:** o automático pertence à abertura da segunda vaga. O manual tem caminho próprio no contrato; hoje o wrapper consulta apenas a pausa geral `logs/pausas/codex-construtor`. `pausar.sh` já exige dono, motivo e saída e registra trilha. (contrato.txt:6; bin/codex_pergunta.sh:191,387; bin/pausar.sh:31-47.)

**Hoje o wrapper não publica na vaga o que a raia 1 toca.** O arquivo recebe abertura e lock; `_INV0` é uma fotografia inicial obrigatoriamente vazia, `_INV1` fica em variável na revalidação, e só depois do Codex o inventário é persistido em `<resposta>.inventario`. (bin/codex_pergunta.sh:232-239,386-392,413.)

Para cumprir a comparação descrita, falta vincular ao ocupante atual: identidade da chamada, WT, base e conjunto de caminhos comparáveis, distinguindo estado ainda não publicado de conjunto vazio. Também falta preservar a informação necessária até a reconferência das duas raias; uma fotografia vazia na abertura não informa os próximos arquivos que a primeira tocará. São lacunas do mecanismo prometido, não novos critérios de admissão. (contrato.txt:4; bin/codex_pergunta.sh:237-239,413.)

O inventário existente já compara arquivos com a base, inclui status, ignorados e varredura de filesystem, e emite `TOCADO`/`PROIBIDO`. **`diff base..HEAD` sozinho não resolve:** no fluxo aceito, HEAD deve continuar igual à base; HEAD diferente já é proibido. Não existe opção `--toca` no parser atual. (bin/codex_pergunta.sh:58-66,137-178,234.)

**3. Riscos pedidos**

- **Corrida entre aberturas:** hoje não existe vaga 2. No desenho anexado, observar a vaga 1 presa e depois tomar a 2 não garante que a primeira continue presa: seu dono pode terminar no intervalo. Outra abertura pode então tomar a 1, enquanto a 2 carrega informação do ocupante anterior. O contrato não especifica coordenação dessa transição. Há ainda o intervalo entre adquirir a primeira vaga e terminar sua validação, quando o estado pode não estar pronto. (contrato.txt:2,4; bin/codex_pergunta.sh:233-242.)

- **Recusa calada:** a recusa atual fala por stderr. Entretanto, ocorre antes do trap e do registro `ini`; assim, não deixa `ini/fim` nem `.rc` dessa tentativa. Um acompanhamento limitado a esses artefatos não a verá. (bin/codex_pergunta.sh:183,233,249-262,285-286.)

- **Relógio e borda:** os `date` atuais não fixam fuso; o contrato não resolve fuso nem extremos da janela. As esperas posteriores podem atravessar 06:00 ou 07:30. Isso exige distinguir “hora da abertura”, exigida pelo aval, de “hora de iniciar o Codex”; o contrato não exige encerramento de uma execução que atravesse a janela. Relógio real do host: [nao verificado]. (contrato.txt:2; bin/codex_pergunta.sh:245,341-369,374,394.)

- **Espera bloqueante:** o contrato garante recusa imediata pelo portão fechado, mas isso não elimina as esperas existentes por slot geral e resposta depois de admitido. Se “ninguém espera” pretendia abranger a chamada inteira, há incompatibilidade. (contrato.txt:3; bin/codex_pergunta.sh:341-369.)

- **RC reaproveitado:** rc 2 já cobre uso/validação e vaga ocupada; rc 3 já cobre cópia, trava de resposta/B3 e inventário; rc 75 já cobre pausa geral e limite. Os novos rc 2/3 não identificariam a causa sozinhos: o motivo precisa sobreviver ao chamador. (contrato.txt:3-4; bin/codex_pergunta.sh:66,183,191,233,294-296,367-371,415-435.)

- **Arquivo da vaga/inode:** a proteção atual reserva somente o caminho exato de `construtor.vaga`; não reservaria automaticamente `construtor.vaga2`. Publicar estado substituindo o arquivo por rename pode separar o caminho do inode ainda preso pelo flock — justamente o acidente que a reserva atual evita. O rename de `pausar.sh` serve ao desligador separado; reutilizá-lo no arquivo travado teria esse risco. Leitores externos à cópia: [nao verificado]. (bin/codex_pergunta.sh:69-78,232-233; bin/pausar.sh:45-46.)

- **`chamadas.log`:** a quebra depende da posição do campo novo. O parser fixa obra/papel/hash/pid/tipo/sessão/rc/duração por índice; acrescentar `vaga=1` ou `vaga=2` ao fim preserva essas posições, inserir no meio pode corromper a leitura. O parser continuará sem distinguir vagas. Contagem, últimas chamadas, procedência e tokens dependem dele; o contrato não especifica a posição. (contrato.txt:5; bin/codex_limite_estado.py:55-78,82-96,185-194.) Os próprios selos fixam posições como `$3`, `$6`, `$7` e `$8`. (bin/tests/test_codex_pergunta.sh:265.)

**4. Selos existentes e mutantes**

| Cobertura existente | Caso e mutante |
|---|---|
| Vaga ocupada, inclusive com outro LOGD | `c_vaga` exige rc 2, motivo e Codex não chamado. **O mutante direto de `#G:vaga` troca `_recusa2` por `true`**, deixando prosseguir após falhar o flock. (bin/tests/test_codex_pergunta.sh:116-120,193-198,468.) |
| Sítio independente da raiz do script | `c_vaga_copia`; mutante de `#G:vaga_sitio` troca `dirname($_CDIR)` por `$R`. (bin/tests/test_codex_pergunta.sh:374-378,498.) |
| Mesmo sítio entre worktrees do repositório | `c_vaga_outra_wt`; mutante troca `dirname($_CDIR)` por `$VIVO`. (bin/tests/test_codex_pergunta.sh:351-355,499.) |
| Reserva do caminho e falha de abertura | `c_vaga_reservada` e `c_vaga_abre`, incluindo fd 6 herdado; mutantes neutralizam as recusas correspondentes. (bin/tests/test_codex_pergunta.sh:230-238,454-455.) |
| Topologia válida e leitura independente da vaga | Construtor sem sítio recusa; papéis de leitura continuam. Mutantes `vaga_topologia` e `vaga_leitura`. (bin/tests/test_codex_pergunta.sh:361-371,500-501.) |
| Inventário/descarte individual | Positivo verifica inventário, `.rc`, `.id` e log; recusa verifica rc 3 e ausência de `.id`; mutante neutraliza a decisão final. **Não comprova interseção entre raias.** (bin/tests/test_codex_pergunta.sh:122-128,245-266,410-415,487.) |
| Trava única de suite | `test_trava_teste.sh` usa lock isolado, comprova ordem entre dois chamadores e rc 75 sem vez. (bin/tests/test_trava_teste.sh:103-137.) |

O mecanismo do selo exige mutante para cada etiqueta `#G:` e acusa quando o caso correspondente continua verde após a mutação. **O mutante atual da vaga comprova a proibição antiga de qualquer segundo construtor; não comprova o portão da segunda vaga autorizado pelo aval.** (bin/tests/test_codex_pergunta.sh:193-198,468,507-527.)

Resultados de execução desses selos e do mutante novo “portão removido”: [nao verificado].

## 09/11 09:01 · sessao 692d6935cbf8 · - · - · origem=Ronald (tmux)

- **Sítio e entrada:** a vaga é derivada do `git-common-dir` do repo vivo em `bin/codex_pergunta.sh:72-78`; o construtor confere topologia em `:189`, abre o arquivo em append em `:231-232` e tenta `flock -n` em `:233`. Hoje só há uma vaga, com recusa falante rc 2; a vaga 2 e os quatro portões não existem nesse ramo (`bin/codex_pergunta.sh:228-239`).

- **Posição dos portões:** no desenho anexado, entram no ramo de falha do primeiro `flock`, hoje `#G:vaga`, antes de admitir a vaga 2 (`bin/codex_pergunta.sh:233`). Entretanto, ainda existem esperas pelo slot geral e pela trava da resposta antes do Codex; uma leitura de load/hora nesse ponto pode envelhecer antes da execução (`bin/codex_pergunta.sh:341-369`, `:394-398`).

- **Estado disponível:** o arquivo da vaga apenas é aberto, sem receber identificação ou arquivos tocados (`bin/codex_pergunta.sh:232`). O precheck exige inventário vazio; a revalidação também guarda a saída apenas em variável; somente o inventário final é persistido em `<resposta>.inventario` (`bin/codex_pergunta.sh:237-239`, `:386-392`, `:413`). Portanto o wrapper não publica hoje o que a raia 1 toca enquanto trabalha (`bin/codex_pergunta.sh:232`, `:413`).

- **Dados necessários para cumprir a comparação:** identificação da chamada/WT/base e conjunto de caminhos ligados ao ocupante atual, com indicação de publicação pronta/em curso/final. O contrato não especifica protocolo de publicação nem preservação do estado até ambas reconferirem [nao verificado]. A fotografia vazia da abertura não prevê os toques posteriores (`bin/codex_pergunta.sh:237-239`). O inventário atual produz `TOCADO<TAB>caminho<TAB>motivo`, inclui status e varredura contra a base; HEAD diferente da base é proibido (`bin/codex_pergunta.sh:137-178`). Não existe `--toca` no parser (`bin/codex_pergunta.sh:58-66`).

- **Reconferência final:** o ponto natural é junto ao inventário, antes de emitir `.id`; a recusa já vence outros rc e impede o sidecar (`bin/codex_pergunta.sh:408-426`, `:442-445`). Se a primeira raia apagar seu estado ao terminar antes da segunda, a segunda perde a comparação; a duração desse estado não foi definida no contrato [nao verificado].

- **Corrida:** hoje não há vaga 2 (`bin/codex_pergunta.sh:233`). No contrato, entre observar a vaga 1 presa e tomar a vaga 2, a primeira pode terminar; faltou tornar verificável que continua ocupada [nao verificado]. Além disso, a primeira adquire o lock antes de validar/publicar qualquer inventário, portanto a segunda pode encontrá-la antes de haver estado pronto (`bin/codex_pergunta.sh:233-242`).

- **Recusa calada:** `_recusa2` escreve motivo no stderr (`bin/codex_pergunta.sh:183`). Contudo, a recusa da vaga ocorre antes de instalar o trap e de registrar `ini`; não produz `.rc` ou `ini/fim`, o que a torna invisível para leitores que só acompanham esses artefatos (`bin/codex_pergunta.sh:233`, `:245-262`, `:285-286`).

- **RC:** 2 já significa uso/validação; 3 já cobre cópia, trava da resposta e inventário; 75 já significa pausa geral e limite. Acrescentar portão em 2 e sobreposição em 3 mantém ambiguidades que exigem motivo/artefato para distinguir (`bin/codex_pergunta.sh:66`, `:183`, `:191`, `:294-296`, `:367-369`, `:415-435`).

- **Relógio:** os `date` atuais não explicitam fuso; o contrato não define fuso nem inclusão exata das bordas [nao verificado] (`bin/codex_pergunta.sh:245`, `:252-255`, `:285`, `:343`, `:374`). As esperas posteriores à vaga podem atravessar a borda (`bin/codex_pergunta.sh:341-369`).

- **Leitores:** `codex_limite_estado.py` lê campos posicionais: tipo em `p[5]`, sessão em `p[6]`, rc em `p[7]`, duração em `p[8]`; inserir `vaga=...` antes deles quebra a leitura, enquanto acrescentar após os campos existentes preserva esse parser, mas ele não passa a distinguir vagas (`bin/codex_limite_estado.py:55-67`). Tokens e procedência dependem da sessão assim obtida (`bin/codex_limite_estado.py:185-194`; `bin/gerar_codex_estado.py:153-155`).

- **Desligador manual:** `pausar.sh` aceita caminho arbitrário e registra QUEM/QUANDO/POR_QUE/SAIDA, publica por rename e mantém trilha; isso serve ao arquivo novo descrito no contrato, mas o wrapper atual só consulta a pausa geral `logs/pausas/codex-construtor` (`bin/pausar.sh:31-47`; `bin/codex_pergunta.sh:191`, `:387`).

## 09/11 09:01 · sessao 6661071fd3d5 · - · - · origem=Ronald (tmux)

- **Não há quebra demonstrada apenas por adicionar `vaga=1|2`**: o leitor atual ignora campos finais, pois aceita `len(p)>=9`; inserir o novo campo antes das posições existentes deslocaria a interpretação. Chave `(obra,hash,pid)` em `p[1],p[3],p[4]`; tipo em `p[5]`; sessão, rc e duração em `p[6:9]`. O contrato não informa a posição da coluna. (`bin/codex_limite_estado.py:56-67`; contrato fornecido, item 4 [nao verificado como arquivo local]).

- **Leitores afetados por deslocamento**: contagem por papel (`bin/codex_limite_estado.py:71-78`), últimas chamadas/rc (`bin/codex_limite_estado.py:82-88`), origem Code (`bin/codex_limite_estado.py:92-96`; `bin/gerar_codex_estado.py:154-155`; `bin/gerar_codex_estado.py:176`), tokens por papel (`bin/codex_limite_estado.py:185-194`) e painel/disjuntor (`bin/gerar_estado.py:269`; `bin/disjuntor.py:1603`; `bin/disjuntor.py:1611`). O selo também fixa `$3`, `$6`, `$7`, `$8`. (`bin/tests/test_codex_pergunta.sh:265-266`).

- **Arquivo da vaga hoje não contém estado da raia**: o trecho abre em append e adquire lock; o inventário inicial fica em `_INV0`, e o final vai para `$RESPOSTA.inventario`. Não há publicação desses dados na vaga nesse fluxo. (`bin/codex_pergunta.sh:231-239`; `bin/codex_pergunta.sh:413`). Consumidores externos à cópia [nao verificado].

- **Identidade do inode importa**: o próprio comentário explica que usar o nome da vaga como resposta substituiria o inode preso; a proteção reserva somente o caminho exato da vaga atual. Uma futura `construtor.vaga2` não fica reservada por essa comparação atual. (`bin/codex_pergunta.sh:69-78`). Publicar estado da vaga por rename pode deixar o flock preso no inode anterior, pelo mesmo mecanismo documentado. (`bin/codex_pergunta.sh:69`; `bin/codex_pergunta.sh:232-233`).

- **`pausar.sh` atende trilha do desligador separado**: exige quem, motivo e condição de saída, escreve temporário e renomeia, registra pausa; soltura também registra trilha. (`bin/pausar.sh:25-28`; `bin/pausar.sh:31-47`). Usar esse rename no próprio arquivo de lock teria o risco anterior. (`bin/pausar.sh:46`; `bin/codex_pergunta.sh:69`).

- **rc reaproveitados**: rc 2 já significa validação/recusa anterior à chamada, não especificamente portão; rc 3 já significa inventário proibido/falho, mas também trava de resposta recusada ou B3. (`bin/codex_pergunta.sh:183`; `bin/codex_pergunta.sh:367-371`; `bin/codex_pergunta.sh:410-426`). rc 75 também serve à pausa do construtor e ao LIMITE; distinguir só por número não distingue causa. (`bin/codex_pergunta.sh:191`; `bin/codex_pergunta.sh:432-435`).

- **Recusa não é registrada em `chamadas.log` quando acontece na vaga**: a guarda da vaga está antes da instalação do trap e da linha `ini`; portanto o motivo atual sai por stderr, mas não produz par `ini/fim` nem `.rc` desse fluxo. (`bin/codex_pergunta.sh:233`; `bin/codex_pergunta.sh:249-262`; `bin/codex_pergunta.sh:285-286`). `.rc` depende de `INI=1`. (`bin/codex_pergunta.sh:251-256`).

- **Teto opus é conduta declarada, sem guarda no wrapper**. (`CLAUDE.md:350-353`).

## 09/11 09:01 · sessao 87cc07e0ba8f · - · - · origem=Ronald (tmux)

Inspeção estática apenas; veredictos de execução e situação no host: [nao verificado].

- O selo direto da vaga é `bin/tests/test_codex_pergunta.sh`: `c_vaga` prende `logs/construtor.vaga` e exige recusa também com outro `LOGD` (`:193–198`). O helper exige rc esperado, motivo e ausência de chamada ao Codex (`:116–120`).
- O mutante que morde especificamente `#G:vaga` troca `_recusa2` por `true`, preservando o `flock` que falhou e permitindo prosseguir (`bin/tests/test_codex_pergunta.sh:468`; guarda em `bin/codex_pergunta.sh:233`). O teste detecta a continuação pelo rc/motivo/chamada (`bin/tests/test_codex_pergunta.sh:116–120`).
- Dois mutantes mordem `#G:vaga_sitio`: raiz do script `$R` em vez de `dirname($_CDIR)` (`bin/tests/test_codex_pergunta.sh:498`, caso de cópia em outra raiz `:374–378`); `$VIVO` em vez do common-dir (`:499`, caso de outra worktree do mesmo repositório `:351–355`).
- Há proteção contra resposta substituir o inode da vaga (`bin/tests/test_codex_pergunta.sh:230–232`, mutante `:454`); controle de basename semelhante permitido (`:218–220`); abertura impossível com fd6 herdado deve recusar explicitamente (`:234–238`, mutante `:455`); topologia sem `<raiz>/.git` recusa construtor (`:361–364`, mutante `:500`), mas permite leitura (`:366–371`, mutante `:501`).
- O mecanismo exige mutante para cada etiqueta `#G:` (`bin/tests/test_codex_pergunta.sh:507–508`), aplica a alteração por etiqueta (`:511–520`), executa o caso correspondente e exige que fique vermelho (`:523–527`). O verde atual do host continua [nao verificado].
- Os casos e mutantes listados não contêm portão CPU da vaga 2, `--toca` ou desligador da segunda vaga (`bin/tests/test_codex_pergunta.sh:410–415`, `:437–505`). O mutante atual `vaga` prova exclusão de **todo segundo construtor** pelo comportamento antigo; não é ainda o selo “vaga 2 sem portão” do novo contrato (`bin/tests/test_codex_pergunta.sh:193–198`, `:468`).
- O inventário e descarte já têm selo: positivo exige `.rc`, `.inventario`, `.id` e log final (`bin/tests/test_codex_pergunta.sh:245–266`); recusa exige rc3, motivo, ausência de `.id` (`:122–128`), e mutante da decisão final neutraliza `if [ "$_IRC" -ne 0 ]` (`:487`). Interseção entre raias não aparece nesses casos (`:410–415`).
- A trava única tem `bin/tests/test_trava_teste.sh`: varredura de runners da suite e molde (`:39–71`), execução em cópia com lock isolado (`:103–115`), dois chamadores serializados (`:117–127`) e ausência de vez com rc75 (`:130–137`). `bin/tests/test_suite_sh.sh` cobre o comando canônico da documentação, sua passagem por `suite.sh` e por `trava_teste.sh` (`:37–58`), com fixtures nos dois sentidos (`:65–85`).
- Os selos `test_*.sh` são chamados pela régua antes da suite (`bin/regua.sh:138–145`); execução efetiva nesta produção: [nao verificado].
- Inserir `vaga=1|2` deslocando campos de `chamadas.log` quebra ao menos leitores posicionais dos próprios testes: papel `$3`, fase `$6`, thread `$7`, rc `$8` (`bin/tests/test_codex_pergunta.sh:265`) e duração `$9` (`bin/tests/test_codex_sonda.sh:96–98`). Acrescentar campo ao fim preserva essas posições específicas; compatibilidade de todos os consumidores depende da inspeção deles [nao verificado].
- `bin/tests/prova_codex_pergunta_real.sh` é prova manual que chama Codex real e verifica efeito do sandbox, não selo automático de concorrência (`:2–14`, `:85–86`, `:134–141`).

## 09/11 08:55 · sessao fb013bdd83d7 · FIO-E0-V4-E3 · construtor · origem=Code

| Hunk | ESCRITA / LEITURA |
|---|---|
| `chamados/juizes.py:1420,1448,1451` | `alimentar_portas` recebe `agora`; lê `qs_na_fila` (`chamados/services/painel.py:158`) e `particionar_verbos`. |
| `chamados/services/verbos.py:30,35` | Partição aceita relógio explícito; continua escrevendo `mapa/contagens` (`:36`). |
| `chamados/services/tarefa_do_dia.py:27,34,373,403,418` | Acrescenta `rota/fila/verbos/fio` (`:433`), selecionando portas das ações exigíveis. Lê `alimentar_portas` (`:409`). |
| `chamados/management/commands/diff_e3_sombra.py:30,41,46,60,81,101` | Lê C17 pelo chamador existente (`:117`) e tarefa (`:118`); escreve somente agregados em stdout (`:99`). Guarda SQL em `:92`. |
| `chamados/tests/e3_fixtures.py:13,16,40,65` | Fixtures ORM, `colNNN`, relógio congelado; lidas pelas três baterias novas. |
| `chamados/tests/test_e3_app_orm.py:12,20,28,47` | M1–M5 pelas views completas dos dois kernels, endpoint e dashboard. |
| `chamados/tests/test_e3_portas_tarefa.py:1,32,40,49,57` | Quatro respostas esperadas escritas antes da implementação; casos adicionais em `:67,75,84,100`. |
| `chamados/tests/test_badge_disputa_pendente.py:6` | Substitui casos antigos pelo selo `test_MORDE_pendentes_do_colaborador_nao_pode_voltar`. |
| `chamados/tests/test_esmeril2_left_join.py:10,19,27` | Migra para `consulta_do_app`: sem filhos conta zero; duas perguntas contam duas. |
| `chamados/tests/test_fio_le_o_juiz.py:40,263,267,272` | Migra preservando a prova de vitalidade C6. |
| `core/tests/test_dashboard_badge.py:10,17,40,45` | Migra para a mesma consulta lida pelo dashboard (`core/views.py:253`). |
| `chamados/tests/test_e3_pendentes_app.py:81,98,113` | Remove matriz com ORM dublado; preserva catraca M7 e contrato puro C17. |
| `chamados/tests/test_e3_diff_sombra.py:39,54,68,96,115` | Selos contra escrita SQL/arquivo e contra perda das projeções/casos. |
| `chamados/tests/e3_mutantes.py:19,69,83` | Define mutantes em memória e imprime cada comando individual (`:75`). |

Leitores externos de `alimentar_portas`: `folha/fora_da_folha.py:62`, `chamados/services/vigia_fio.py:604`, `escala/services/cadastro_realidade.py:484`, C17 em `chamados/juizes.py:1536`.

Leitores externos da partição: `chamados/services/painel.py:230`, `fila.py:55`, `na_fila.py:19`, `verbos.py:171`; `chamados/views_arquivar.py:124`, `views_cobrar.py:77`, `management/commands/arquivar_prescritos.py:50`; `colaboradores/services/ficha.py:155`, `core/context_processors.py:90`, `api/views_mensageria.py:395,453,1149`.

Os campos novos são lidos pelo instrumento (`chamados/management/commands/diff_e3_sombra.py:41,46`). Os três leitores atuais continuam chamando C17: `folha/fora_da_folha.py:67`, `chamados/services/vigia_fio.py:635`, `escala/services/cadastro_realidade.py:488`.

**DECISAO-ABERTA:** remoção do método e baixa no registro ficam com a sessão; DIFF com L-099 antes da ligação. O selo L-111 exige ausência do método (`chamados/tests/test_badge_disputa_pendente.py:6`); resultado RED ainda **[nao verificado]**.

Comandos preparados para a sessão; não executados:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_e3_portas_tarefa chamados.tests.test_badge_disputa_pendente --settings=config.settings.ci

# Imprime o comando individual de cada mutante:
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e3_mutantes

# DIFF — executar somente após a sonda:
PYTHONDONTWRITEBYTECODE=1 python3 manage.py diff_e3_sombra --colab ID --inicio AAAA-MM-DD --fim AAAA-MM-DD --agora INSTANTE_COM_FUSO
```

`py_compile` executado sem gravar bytecode:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -c 'import py_compile, sys; from unittest.mock import patch; patch("importlib._bootstrap_external._write_atomic").start(); [py_compile.compile(p,cfile=p+".verificacao",doraise=True) for p in sys.argv[1:]]' $(GIT_OPTIONAL_LOCKS=0 git ls-files -m -o --exclude-standard -- '*.py')
```

Log: **14 arquivos compilados; `git diff --check` rc 0**. Suíte, RED, mutantes e DIFF não executados; comportamento **[nao verificado]**. [nao verificado]

## 09/11 08:50 · sessao 6499c93501d2 · SELOS-CANON-I1 · verificador · origem=Code

**O item 1 está curado quanto à abertura indevida da vaga.** O espelho está no predicado comum; o Agent chega nele, e o wrapper usa o mesmo predicado, embora recuse previamente os IDs com espaço. Há resíduos fora do diff e um limite na prova do PAREI, detalhados abaixo. (`bin/disjuntor.py:1686`, `:1850`, `:2248`, `:2267`.)

1. **Cura nas duas vias e caminhos alternativos**

   - O Agent chama `_censo_nega`, que consulta `_pertence_fila1`. Agora, célula `O9` contra obra `O9 (C)` ou `O9 NOTURNO-X` produz `ValueError`, convertido em negativa NAO SEI. A saída antecipada como FORA deixou de ocorrer nesses pares. (`bin/disjuntor.py:1850`, `:1704`, `:1686`, `:1691`, `:1730`.)
   - O wrapper chama `--censo` antes de prender a vaga; `main_censo` consulta `_pertence_fila1` e passa a mesma lista de células para `_censo_nega`. Os exemplos com espaço já são recusados antes dessa consulta; no wrapper real, a validação é ainda anterior, no shell. (`bin/codex_pergunta.sh:188`, `:234`, `:240`; `bin/disjuntor.py:2248`, `:2266–2275`.)
   - Nos caminhos de autorização de censo examinados, a decisão passa pelo predicado comum. A igualdade existente nele compara **obra com token**, dentro do próprio juiz. (`bin/disjuntor.py:1685`, `:1704`, `:2267`.)
   - **Existem igualdades fora dele:** o hook compara a célula crua com o alvo do marcador para localizar o item em curso e remove esse item da lista por igualdade. São decisões de marcador/ordem, distintas da autorização do censo. (`bin/hook_stop_fila1.py:223`, `:273`.)
   - **Existe uma derivação própria de fila no alarme**, descrita no item 4. Ela alimenta o disparo do alarme, sem autorizar vaga de construtor. (`bin/alarme_sessao_ociosa.py:89–116`, `:148–154`.)

2. **Quem define, quem lê e significado dos retornos**

   | Função | Definição e leitores encontrados |
   |---|---|
   | `_pertence_fila1` | Definida em `bin/disjuntor.py:1669`; leitores produtivos em `:1704` e `:2267`; chamadas diretas do selo em `bin/tests/test_censo_antes_da_vaga.sh:605` e `:611`. |
   | `_token_da_celula` | Definida em `bin/disjuntor.py:1651`; lida nas listas `exatas` e `nomeiam`, em `:1685–1686`. |
   | `_casa_id_inteiro` | Definida em `bin/hook_stop_fila1.py:110`; lida pelo predicado em `bin/disjuntor.py:1686` e pelo julgamento do PAREI em `bin/hook_stop_fila1.py:1301` e `:1313`. |

   As versões mutantes são escritas pelo mecanismo do selo em cópias de `bin/`; há mutações desses critérios em `bin/tests/test_censo_antes_da_vaga.sh:750–763`, aplicadas por `:669–678`.

   Os dois leitores produtivos de `_pertence_fila1` concordam: `False` permite seguir como FORA; `True` exige o censo; exceção gera negativa NAO SEI. (`bin/disjuntor.py:1704–1731`, `:2267–2281`.) `_token_da_celula` retorna string ou `None`; o espelho não transforma `None` em correspondência, porque `_casa_id_inteiro` rejeita ID vazio. (`bin/disjuntor.py:1660–1666`; `bin/hook_stop_fila1.py:115`.)

   A nomeação ignora caixa e usa fronteira ASCII que exclui letra, dígito e hífen; underscore é separador. A extração do token usa a mesma classe de fronteira. A igualdade exata permanece sensível à caixa, razão de `o9 × O9` resultar em ambiguidade. (`bin/hook_stop_fila1.py:117`; `bin/disjuntor.py:1664`, `:1685–1691`.)

   Alarme e handoff chegam a esses critérios indiretamente pelo hook. Para a prova de PAREI censo, ambos respeitam o contrato próprio do reconhecedor: `None` significa “não é linha de censo”; `''`, prova válida; texto, motivo de reprovação. (`bin/alarme_sessao_ociosa.py:127–140`; `bin/handoff_sessao.sh:129–134`; `bin/hook_stop_fila1.py:773–785`.) O handoff também consome os julgamentos booleanos do hook como booleanos. (`bin/handoff_sessao.sh:120–123`.)

3. **Matriz do predicado literal**

   Cada linha considera `_pertence_fila1(obra, [celula])`. A coluna anterior corresponde à retirada do espelho, exatamente a mutação declarada no selo. (`bin/disjuntor.py:1685–1692`; `bin/tests/test_censo_antes_da_vaga.sh:763`.)

   | Obra | Célula | Atual | Sem espelho | Sítio da saída atual |
   |---|---|---|---|---|
   | `O1` | `O142` | `False` — FORA | `False` | `bin/disjuntor.py:1687–1688` |
   | `O142` | `O1` | `False` — FORA | `False` | `bin/disjuntor.py:1687–1688` |
   | `O9` | `O9` | `True` — DENTRO | `True` | `bin/disjuntor.py:1689–1690` |
   | `O9 (C)` | `O9` | `ValueError` — NAO SEI | `False` | `bin/disjuntor.py:1691` |
   | `O9 NOTURNO-X` | `O9` | `ValueError` — NAO SEI | `False` | `bin/disjuntor.py:1691` |
   | `OBRA-Q (A)` | `OBRA-Q` | `ValueError` — NAO SEI | `False` | `bin/disjuntor.py:1691` |
   | `09-TURNO-ABERTO-EXPOSTA` | `O9` | `False` — FORA | `False` | `bin/disjuntor.py:1687–1688` |
   | `O67b` | `O9` | `False` — FORA | `False` | `bin/disjuntor.py:1687–1688` |
   | `o9` | `O9` | `ValueError` — NAO SEI | `ValueError` | `bin/disjuntor.py:1691` |

   **Há três conversões de FORA para NAO SEI**, e elas passam a negar na porta do Agent. O código as declara ambiguidades pretendidas; demonstrar que alguma corresponde a uma obra **de fato fora da fila** em produção é **[nao verificado]** com o dado fornecido. Também não há base para garantir ausência de toda negativa legítima afetada. (`bin/disjuntor.py:1674–1680`, `:1730–1731`.)

   A matriz não equivale à extração do prompt: `OBRA-Q (A)` vira `OBRA-Q` no Agent; `09-TURNO-ABERTO-EXPOSTA`, `O67b` e `o9` não satisfazem o reconhecedor de obra desse caminho. Obra ilegível é negada antes da pertença. (`bin/disjuntor.py:157`, `:602–627`, `:1847–1849`.)

4. **Classes de reprovação**

   - **Derivação paralela presente:** o alarme conta sua própria fila, excluindo `☑`, `FEITO` e `FECHAD`; o hook usa marcadores e `_item_parado`, incluindo exclusões como CONGELADA, FILA 2 e PAREI. Uma linha apenas CONGELADA/FILA 2 pode ser contada pelo alarme e excluída pelo hook. (`bin/alarme_sessao_ociosa.py:89–116`; `bin/hook_stop_fila1.py:95–97`, `:171–180`, `:231–252`.)
   - **Escritores fora da porta presentes na cópia:** `gerar_cortes.py` e `deploy_agendado.sh` regravam RELATO diretamente; o escritor canônico declara esse resíduo. A cura examinada usa `no_relato`/`no_topo`. (`bin/gerar_cortes.py:117–136`; `bin/deploy_agendado.sh:31–44`; `app/core/esteira_vigia.py:839–840`; `bin/disjuntor.py:294`, `:324`.)
   - **Segundo juiz de negócio concorrendo com autoridade registrada:** não demonstrado nesta cura; conclusão abrangente sobre todos os registros: **[nao verificado]**. O caminho examinado reutiliza o hook, o predicado, o coletor e o juiz de brief. (`bin/disjuntor.py:1637`, `:1704`, `:1710`, `:1715`.)
   - **Relógio fora da borda:** não aparece na decisão de pertença/token; a medida recebe `agora` e filtra futuro e janela. (`bin/disjuntor.py:1651–1692`, `:2066`, `:2094`, `:2127–2131`.)
   - **Ordem de trava:** o censo antecede a vaga; as portas de RELATO compartilham trava, liberada ao retornar. Não identifiquei inversão nesse percurso. (`bin/codex_pergunta.sh:234–240`; `app/core/esteira_vigia.py:843`, `:846–862`, `:892`.)
   - **Campo com dois significados:** `_censo_nega` usa `None` para “porta passa”, abrangendo FORA e aprovação; isso está documentado, e `main_censo` distingue FORA pelo predicado. Não encontrei leitor tomando esse `None` por DENTRO. (`bin/disjuntor.py:1696`, `:1704–1705`, `:1724`, `:2267–2275`.)

5. **Mordida dos casos novos e seu limite**

   - **Laço Agent:** prepara célula `O9` sem `O9 (C)` e exige negativa JSON, motivo de censo, PAREI com a obra inteira e negativa correspondente na trilha. Saída vazia reprova. Sem espelho, o predicado retorna FORA antes da validação de forma, retirando a negativa cobrada. (`bin/tests/test_censo_antes_da_vaga.sh:580`, `:520–527`, `:588–591`; `bin/disjuntor.py:1687–1688`, `:1704–1706`.)
   - **ESPELHO direto:** exige `ValueError` nos três pares e exatamente `False` nos dois controles de prefixo. Portanto, “sempre retornar FORA” e “sempre levantar” reprovam. Aceita qualquer `ValueError`, sem validar a mensagem. (`bin/tests/test_censo_antes_da_vaga.sh:603–619`.)
   - **Mutante:** remove precisamente o espelho e executa `casos_pertenca`; há exigência de âncora única, controle sem mutação e propagação de falha pelo retorno dos casos. (`bin/tests/test_censo_antes_da_vaga.sh:763`, `:675–686`, `:662–665`, `:647`.)
   - **Wrapper:** os dois casos são controles expressamente declarados, inclusive no nome; passam com ou sem a cura porque a forma é recusada antes da pertença. (`bin/tests/test_censo_antes_da_vaga.sh:585–593`; `bin/disjuntor.py:2248–2250`.)
   - **Limite material:** o laço Agent prova o PAREI **na mensagem**. Para esses IDs com espaço, `_parei_censo_no_relato` retorna antes de gravar; o laço não cobra persistência no topo. Logo, a mordida demonstra negativa/trilha, mas não demonstra PAREI gravado desses exemplos. (`bin/tests/test_censo_antes_da_vaga.sh:520–527`, `:590–591`; `bin/disjuntor.py:309–311`.)

A execução integral do selo e seu resultado verde permanecem **[nao verificado]**; o script cria e altera arquivos de teste. (`bin/tests/test_censo_antes_da_vaga.sh:27–35`, `:669–678`.) O conteúdo integral da lei citada como `CORTES.md:428` também permanece **[nao verificado]**: a cópia termina em `app/docs/CORTES.md:425`, e o anexo fornecido traz somente o início daquela linha.

## 09/11 08:48 · sessao 400ac2cb4b5d · - · - · origem=Ronald (tmux)

Não encontrei classe nova dessas no caminho da cura do censo, com os limites abaixo.

- **Juiz/derivação única:** `_fila1_viva` delega o universo ao hook; `_censo_nega` usa `_pertence_fila1`, `_briefs_do_censo` e `censo_do_brief`. Agent chama essa porta; wrapper chama `--censo`. A classificação adicional em `main_censo` repete o mesmo predicado e reutiliza a mesma lista `fila`. (`bin/disjuntor.py:1633`, `bin/disjuntor.py:1704`, `bin/disjuntor.py:1710`, `bin/disjuntor.py:1715`, `bin/disjuntor.py:1850`, `bin/disjuntor.py:2266`, `bin/disjuntor.py:2275`; `bin/codex_pergunta.sh:234`.)
- **Escritor/ordem:** a negativa passa por `ev.no_relato`; PAREI passa por `ev.no_topo` e é relido pelo reconhecedor do Stop. Ambas as portas usam o mesmo flock; leitura, classificação, idempotência e gravação do topo ficam sob a trava. O wrapper executa censo antes de tentar vaga; a trava do RELATO já foi fechada quando chega à vaga. (`bin/disjuntor.py:293`, `bin/disjuntor.py:323`; `app/core/esteira_vigia.py:842`, `app/core/esteira_vigia.py:859`, `app/core/esteira_vigia.py:877`, `app/core/esteira_vigia.py:893`; `bin/codex_pergunta.sh:234`, `bin/codex_pergunta.sh:240`.)
- **Relógio:** pertença/token não consultam relógio; `_censo_24h` recebe `agora`, exclui futuro e aplica a janela ao casar a abertura. `trilha` consulta relógio para carimbar o registro. (`bin/disjuntor.py:1651`, `bin/disjuntor.py:1669`, `bin/disjuntor.py:2066`, `bin/disjuntor.py:2094`, `bin/disjuntor.py:2127`, `bin/disjuntor.py:198`.)
- **Campo/retorno:** `None` de `_censo_nega` significa “porta passa”, tanto FORA quanto APROVADO; `main_censo` distingue FORA pelo bool do mesmo `_pertence_fila1`, portanto não interpreta `None` como DENTRO. (`bin/disjuntor.py:1696`, `bin/disjuntor.py:1704`, `bin/disjuntor.py:1723`, `bin/disjuntor.py:2267`, `bin/disjuntor.py:2271`.)
- **Resíduo explícito fora dessa cura:** o escritor declara escritores externos que não pegam sua trava: edição manual, `gerar_cortes.py` e `deploy_agendado.sh`. O alcance desses caminhos não foi auditado por este agente. (`app/core/esteira_vigia.py:838`.) A lista usada pelo censo contém somente andáveis; a situação de obra PARADA continua dependente desse recorte. (`bin/disjuntor.py:1637`.)

Autoridade registrada para esta pergunta específica em outro lugar e efeitos atuais em produção: [nao verificado].

## 09/11 08:47 · sessao fb09f4e39450 · - · - · origem=Ronald (tmux)

Os três novos elementos têm mordida por leitura do código; execução integral do selo permanece [nao verificado].

- **Laço Agent:** a fixture remove `O9 (C)` e mantém `O9` sozinho antes dos casos (`bin/tests/test_censo_antes_da_vaga.sh:576`, `:580`). Os prompts preservam as identidades `O9 (C)` e `O9 NOTURNO-X`, pela cauda e pela identidade de id nu (`bin/disjuntor.py:164`, `:616`, `:624`). Os casos exigem JSON `deny`, `DISJUNTOR CENSO` e PAREI da obra completa (`bin/tests/test_censo_antes_da_vaga.sh:520`), além de última negativa correspondente e ausência de FORA (`:524`); saída vazia reprova (`:522`). O laço executa essas verificações nas duas obras (`:588`–`:593`).

- **Por que mata a ausência do espelho:** sem o segundo sentido, essas obras não aparecem dentro da célula `O9`, logo `nomeiam` e `exatas` ficam vazios e a função retorna `False` (`bin/disjuntor.py:1685`–`:1688`). `_censo_nega` então retorna `None` antes da validação da forma (`:1704`–`:1706`), deixando de fornecer a negativa exigida pelo caso Agent (`bin/tests/test_censo_antes_da_vaga.sh:590`). Com o espelho, há nomeação sem correspondência exata, e a função levanta `ValueError`, convertido em negativa NAO SEI (`bin/disjuntor.py:1686`, `:1691`, `:1730`).

- **Wrapper:** são controles sem poder de distinguir a cura, expressamente declarados assim no comentário e no nome do caso (`bin/tests/test_censo_antes_da_vaga.sh:585`, `:593`). `main_censo` recusa espaço antes da leitura da fila e da pertença (`bin/disjuntor.py:2248`, `:2266`). Não há controle ocultamente apresentado como mordida nova nesses dois casos (`bin/tests/test_censo_antes_da_vaga.sh:585`–`:586`).

- **ESPELHO direto:** chama o predicado efetivamente importado do diretório `$B`, exige `ValueError` nos três pares e reprova qualquer retorno, inclusive `False` (`bin/tests/test_censo_antes_da_vaga.sh:598`–`:608`). Os controles `O1/O142` e inverso exigem exatamente `False`; um predicado que sempre levante também reprova (`:609`–`:619`). A chamada direta cobre `OBRA-Q (A)`, que o Agent normalizaria para `OBRA-Q` (`bin/disjuntor.py:624`–`:627`; justificativa no selo `:596`).

- **Mutante:** remove exatamente o segundo sentido e roda `casos_pertenca` (`bin/tests/test_censo_antes_da_vaga.sh:763`). A infraestrutura exige âncora única (`:675`–`:678`), exige retorno não zero dos casos (`:681`–`:686`) e roda previamente uma cópia sem mutação nos mesmos grupos (`:662`–`:665`). `casos_pertenca` acumula `ok=1` e retorna esse valor, de modo que suas falhas chegam ao julgador do mutante (`:590`, `:619`, `:647`).

Confirmei apenas em memória, com `python3 -B`, o resultado do predicado vivo e dessa substituição: os três pares espelho dão `ValueError` no vivo e `False` no mutante; os dois controles de prefixo dão `False` em ambos. Esses resultados são explicados por `bin/disjuntor.py:1685`–`:1692` e pela substituição de `bin/tests/test_censo_antes_da_vaga.sh:763`; a suite completa e o seu resultado final são [nao verificado].

Limite: a asserção direta aceita qualquer `ValueError`, sem conferir sua mensagem (`bin/tests/test_censo_antes_da_vaga.sh:606`). Na implementação lida, para essas entradas não vazias, o `ValueError` explícito é o de pertença ambígua (`bin/disjuntor.py:1681`, `:1691`); não encontrei vacuidade concreta nesse trecho.

## 09/11 08:47 · sessao 1b91c105ad23 · - · - · origem=Ronald (tmux)

Rastreio das três funções, incluindo arquivos fora do diff:

- **`_pertence_fila1`**: definição única em `bin/disjuntor.py:1669`. Leitores produtivos encontrados: `_censo_nega`, em `bin/disjuntor.py:1704`, e `main_censo`, em `bin/disjuntor.py:2267`. Ambos interpretam `False` como FORA/passagem e exceção como NAO SEI/negativa; `True` leva ao censo (`bin/disjuntor.py:1704-1731`, `bin/disjuntor.py:2267-2281`).
- **`_token_da_celula`**: definição única em `bin/disjuntor.py:1651`; leitores produtivos encontrados somente nas listas `exatas` e `nomeiam` de `_pertence_fila1`, em `bin/disjuntor.py:1685-1686`. Retorna string ou `None`; a nomeação não aceita id vazio, portanto o `None` não casa pelo espelho (`bin/disjuntor.py:1660-1666`, `bin/hook_stop_fila1.py:115-117`).
- **`_casa_id_inteiro`**: definição única em `bin/hook_stop_fila1.py:110`. Leitores diretos encontrados: `_pertence_fila1`, em `bin/disjuntor.py:1686`, e `_parei_do_em_curso`, em `bin/hook_stop_fila1.py:1301` e `bin/hook_stop_fila1.py:1313`. Todos consomem bool; a implementação usa caixa normalizada e fronteira `[A-Z0-9-]`, inclusive permitindo underscore como separador (`bin/hook_stop_fila1.py:115-117`). A fronteira de extração da célula usa a mesma classe ASCII (`bin/disjuntor.py:1664`).

**Fora do diff:** o handoff não lê diretamente essas três funções: pede item em curso ao hook, e usa seus bools como bools, sem a antiga confusão bool/lista (`bin/handoff_sessao.sh:64`, `bin/handoff_sessao.sh:71`, `bin/handoff_sessao.sh:120-123`). Para PAREI censo, pede o reconhecedor compartilhado e distingue `None`, `''` e motivo (`bin/handoff_sessao.sh:129-134`). O alarme também não lê diretamente as três funções; pede `prova_parei_censo` e só considera provado o retorno `''` (`bin/alarme_sessao_ociosa.py:127-140`).

**Classe presente fora do diff: derivação paralela da fila.** O alarme possui `_fila` próprio: procura `## OBRAS`, conta bullets/linhas e exclui `☑`, `FEITO` ou `FECHAD` em qualquer posição (`bin/alarme_sessao_ociosa.py:89-116`). O juiz compartilhado do hook delimita por marcador e usa `_item_parado`, que também exclui CONGELADA, FILA 2, PAREI e outras marcas (`bin/hook_stop_fila1.py:95-97`, `bin/hook_stop_fila1.py:171-180`, `bin/hook_stop_fila1.py:231-252`). Assim, são respostas potencialmente divergentes à contagem da fila; o alarme usa sua resposta para decidir o disparo (`bin/alarme_sessao_ociosa.py:40-45`, `bin/alarme_sessao_ociosa.py:148-150`). Isso é herdado e não constitui outro caminho de autorização da vaga do censo (`bin/alarme_sessao_ociosa.py:144-164`).

**Igualdades fora do predicado existem, mas respondem outra pergunta:** o hook compara célula crua com o alvo declarado no marcador para localizar o item em curso (`bin/hook_stop_fila1.py:209-227`) e remove esse item por igualdade da lista de andáveis (`bin/hook_stop_fila1.py:270-273`). Não convém afirmar genericamente “não há igualdade obra × célula”; há essa igualdade, sem decidir a pertença usada pelo censo (`bin/disjuntor.py:1704`, `bin/disjuntor.py:2267`).

Leitores do selo encontrados: chamadas diretas a `_pertence_fila1` em `bin/tests/test_censo_antes_da_vaga.sh:605` e `bin/tests/test_censo_antes_da_vaga.sh:611`; mutações textuais das listas em `bin/tests/test_censo_antes_da_vaga.sh:750` e `bin/tests/test_censo_antes_da_vaga.sh:763`. Chamadas dinâmicas não identificáveis por referências nominais permanecem [nao verificado].

_3094 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
