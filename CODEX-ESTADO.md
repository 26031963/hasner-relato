# CODEX-ESTADO

> Ultima resposta processada: **12/11/2026 14:22** · publicado em **12/11/2026 14:22** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 12/11 14:22 · sessao 7e5d0ea3b9d0 · - · - · origem=Ronald (tmux)

Conferência das 10 obras, somente leitura. A cópia não tem `.git`; branches, refs e identidade dos blobs das raias não puderam ser conferidos. [nao verificado]

| Obra | Classe sugerida | Prova no main e síntese |
|---|---|---|
| O275 | PARADA | **não achei** o campo `feriado_e_dia_trabalho`; o leitor atual começa em `app/escala/models.py:1252`. A raia cria cadastro do tipo para tornar feriado previsto; pouso retido por FORMA A × B, segundo `app/docs/RELATO-ARQUIVO.md:753` e `app/docs/RELATO-ARQUIVO.md:755`; trava dupla explicitada em `app/docs/RELATO-ARQUIVO.md:1486`. |
| O276 | NO-MAIN-COM-OUTRO-NOME | `app/ponto/turnos.py:1430` e `app/ponto/turnos.py:1447`: fecho em fluxo pergunta teto do dia; ultrapassado, cria turno aberto e saída órfã em `app/ponto/turnos.py:1453`. Pouso sob **R1-H-PORTAO-DE-26H** documentado em `app/docs/RELATO.md:3293`. **Ressalva:** main aplica exceção L-084, devolvendo teto de 26 h quando ambas as pontas estão deslocadas (`app/ponto/turnos.py:1435`); não é reprodução literal irrestrita da raia. |
| V3-RETOMA-FILA | PARADA | **não achei** o lado v2 obtido da lavra: main ainda monta `m_dia` por laço próprio em `app/ponto/management/commands/diff_calculador.py:245`; CCT continua calculada com `ini` e template do chamador em `app/ponto/calculador/alimentacao.py:389`. Motivo registrado: instrumento pousa depois da V3, `app/docs/RELATO.md:1707`. |
| V3-RETOMA-PERIODOS-DA-CELULA | PARADA | **não achei** a regra completa: main ainda preenche `pares_por_dia` com segmentos em `app/ponto/calculador/alimentacao.py:158`, e declara/continua quando período não tem dia ou carimbo em `app/ponto/calculador/alimentacao.py:79`, em vez da recusa da raia. Motivo: dependência da pista, P4 aberto e base atrasada, `app/docs/RELATO.md:7328`. **P5 isolada foi refeita e pousou** sob outra obra, `app/docs/RELATO.md:7408`, mas isso não absorve a obra inteira. |
| V3-E1 | PARADA | **não achei** os cinco arquivos do stat. É bateria/arnês, não implementação de produção; resultado registrado da validação tem 14 falhas de status em 11 casos, `app/docs/RELATO.md:1805`; registro anterior de não pousar junto à v2 e necessidade de E1 validado em `app/docs/RELATO.md:1815`. Motivo mais recente integral: **não sei**; a linha de validação não declara o destino posterior do arnês. |
| E3-ISENTO-INSUMOS | ABSORVIDA-POR ISENTO-H10 | `app/docs/BACKLOG.md:934` diz explicitamente SUPERADA por ISENTO-H10 e que `c9f16a60` vira sua base; pouso em `app/docs/BACKLOG.md:924`. Produção `app/ponto/services/cartorio.py:548` acrescenta `isento:1` com isenção e `regra >= 4`; o horizonte substitui a aplicação atemporal da raia. |
| CASA-ESCALA-E4 | NO-MAIN-COM-OUTRO-NOME | `app/colaboradores/queries.py:64` usa `q_vinculo_cobre_a_data`; dinheiro usa a juíza em `app/folha/previa.py:81`, `app/ponto/services/fechamento.py:229`; carregamento em `app/ponto/nucleo.py:15`. Parte 5 pousada e dez cópias migradas documentadas em `app/docs/RELATO.md:3384` e `app/docs/RELATO.md:3386`. Mesmo comportamento em main, com nomes dos juízes. |
| QUATRO-PRAZOS | NO-MAIN-COM-OUTRO-NOME | `app/ponto/turnos.py:3186`, `app/ponto/turnos.py:3199`, `app/ponto/turnos.py:3229`; chamadas reais `app/ponto/management/commands/processar_alertas_avancados.py:63` e `app/ponto/management/commands/processar_alertas_turno.py:118`. Gate do detectar pergunta autoridades em `app/ponto/management/commands/detectar_ausencias.py:471` e `app/ponto/management/commands/detectar_ausencias.py:474`. Pousos das partes anexadas: `app/docs/RELATO.md:3350` e `app/docs/RELATO.md:3406`. |
| JUIZ-VARREDURA-1 | NO-MAIN-COM-OUTRO-NOME | Cron atual apenas conta e alarma em `app/ponto/management/commands/reavaliar_ausencias_feriado.py:35` e `app/ponto/management/commands/reavaliar_ausencias_feriado.py:43`; pouso posterior como **JUIZ-1 v5** em `app/docs/RELATO.md:1297`, registro NO AR em `app/docs/RELATO.md:1294`. A raia anexada inclui também JV4A, mas contém a obra JV1; não é raia alheia. [nao verificado quanto aos blobs] |
| JUIZ-VARREDURA-3 | ABSORVIDA-POR C20-JUIZ-3 | `app/docs/RELATO.md:7859` a `app/docs/RELATO.md:7863` descrevem pouso C20-JUIZ-3 e nomeiam o cron vigia, outbox e receiver; `app/docs/BACKLOG.md:1816` registra NO AR. Produção: consumidor chama retratação da premissa em `app/chamados/reconciliador.py:1373`; cron só conta e alarma em `app/chamados/management/commands/reconciliar_chamados.py:118`. |

Arquivos do stat para as quatro PARADAS, distinguindo **ausentes fisicamente** de alterações de raia em arquivos já existentes:

- **O275:** ausentes `app/escala/management/commands/vinculos_tipo_feriado_contra_vinculo.py`, `app/escala/migrations/0043_tipoescala_feriado_e_dia_trabalho.py`, `app/escala/tests/test_o275_sdf_feriado_e_dia.py`, `app/escala/tests/test_tripwire_o275_leitor_unico_do_flag.py`. [nao verificado: ausência não tem linha] Outros arquivos do stat já existem: `app/config/crons.py`, `app/escala/{models.py,services/cadastro_tipo.py,views_wizard.py}`, `app/templates/escala/{_wizard_preview.html,tipos_lista.html,wizard_tipo_escala.html}`; identidade das alterações [nao verificado].
- **V3-RETOMA-FILA:** ausentes `app/ponto/tests/test_diff_calculador_lado_v2_e_a_lavra.py` e `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py`. [nao verificado: ausência não tem linha] Os dois arquivos de produção do stat existem, mas conservam regra anterior nas linhas citadas acima.
- **V3-RETOMA-PERIODOS-DA-CELULA:** ausentes `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py`, `app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py`, `app/ponto/tests/test_v3_periodos_da_celula.py`. [nao verificado: ausência não tem linha] Existem os quatro arquivos de produção do stat e `test_s5b_split_noturno_tem_um_dono.py`, `test_v3_parei_p5_parte_menor_que_todo.py`; a existência do último decorre da P5 posterior, sem comprovar toda a regra (`app/docs/RELATO.md:7408`).
- **V3-E1:** ausentes todos os cinco: `app/docs/E1-PERGUNTA-AO-DP-CONTADOR.md`, `app/ponto/tests/{test_e1_lote1_valor_do_dia.py,test_e1_lote2_status_do_dia.py,test_e1_lote3.py,test_v3_e1_valor.py}`. [nao verificado: ausência não tem linha]

Todos os primeiros diffs deste grupo foram truncados a 5.000 bytes no dado fornecido; os corpos posteriores ao corte não são visíveis e a identidade completa das alterações não é verificável sem refs. [nao verificado] Não encontrei raia evidentemente alheia neste grupo; `wt-jv4a-033135` empilha JV4A sobre JV1, e `wt-v3p5v2-cura4-220249` empilha P5 sobre V3-PERIODOS, conforme os commits anexados. [nao verificado] [nao verificado]

Contagem deste grupo: NO-MAIN-COM-OUTRO-NOME **4**, ABSORVIDA-POR **2**, MORTA-POR-CORTE **0**, PARADA **4** = **10**. [nao verificado]

## 12/11 14:22 · sessao db63305967d7 · - · - · origem=Ronald (tmux)

Classificações recomendadas das 9:

| Obra | Classe | Prova e conclusão |
|---|---|---|
| F1C4-3-RG-SEM-CELULA-CASO | NO-MAIN-COM-OUTRO-NOME | `app/ponto/management/commands/reconciliar_grade.py:281-282`: sob F1C4-3-NONE-COBRA, só inclui dia indefinido nos mortos se existe célula e o juiz diz sem trabalho; sem célula não retrata. A raia acrescenta apenas o caso de teste. [nao verificado: conteúdo integral da raia] |
| O10-C | PARADA | Não achei o lote no main; motivo `app/docs/RELATO.md:1088-1094`: SOLO exige lista do admin e porta de UI alcançável, sai do comboio e precisa da volta 2. |
| DEPLOY-[nome]-ELO | ABSORVIDA-POR DEPLOY-RELOAD-GRACIOSO | `app/docs/BACKLOG.md:930`, `app/docs/RELATO.md:1513`; código `bin/deploy.sh:110-117`, `bin/deploy.sh:338-359`: preserva o elo usando HUP e recusa restart com cron vivo; substituiu o mecanismo antigo de admissão. |
| SUITE-POR-IMPACTO | PARADA | Não achei seleção por cobertura; motivo `app/docs/RELATO.md:1385-1387`: mapa falhou, replay/PRONTO 5 permanece fila; rebase ainda em cópia em `app/docs/RELATO.md:1399`. |
| PADRAO-DE-CIMA | PARADA | Não achei agrupamento de DESENHO por entidade; motivo `app/docs/RELATO.md:1595`, `app/docs/RELATO.md:7310`: 9 voltas, opus reprovou, volta 10 cancelada e obra estacionada. Não há prova de proibição definitiva. |
| CODEX-CONSTRUTOR-PILOTO | ABSORVIDA-POR CODEX-CONSTRUTOR-NEGOCIO | Para o **instrumento anexado**, `app/docs/RELATO.md:1127` declara generalização/rebase do papel antes não pousado, `app/docs/RELATO.md:873` e `app/docs/BACKLOG.md:1780` registram pouso v5; produção `bin/codex_pergunta.sh:197-204`, `bin/codex_pergunta.sh:217-238`, `bin/codex_pergunta.sh:253`, `bin/codex_pergunta.sh:415-419` valida cópia e restringe escrita a WT/app. A tela POSICAO-DO-DESLIGAMENTO não está provada por essa raia; `app/docs/RELATO.md:2239` diz que a WT do piloto ainda não existia. |
| CODEX-CONTEXTO-DA-CASA | PARADA | Não achei gerador de AGENTS/validador do brief; motivo `app/docs/BACKLOG.md:1020`: pouso retido para não recusar briefs de construtores em voo; complemento ainda posterior em `app/docs/RELATO.md:1743`. |
| OTIMIZA-1 | PARADA | Não achei medida inteiras/pouso: `bin/gerar_estado.py:265-270` passa da recusa diretamente ao Codex; `bin/trava_teste.sh:40-42` executa comando sem gravar suites.log. Motivo `app/docs/RELATO.md:1395`: faltam suite inteira, replay PRONTO 5, verificador e pouso próprio. |
| FIO-ESTADO-LE-O-ALCANCE | NO-MAIN-COM-OUTRO-NOME | Reimplementação v2 em `_ler_carimbo` e `linha`: `bin/fio_estado.py:137-156`, `bin/fio_estado.py:175-189`; valida alcance/cegueira/universo em `bin/fio_estado.py:74-95`; integrado em `bin/gerar_estado.py:260-261`. `app/docs/BACKLOG.md:966` registra pouso próprio 9453c4f6 sob o mesmo ID, portanto a premissa de ausência de pouso conflita com o main. A validação de soma das partições mostrada na raia antiga não existe nessa v2; contrato revisto em `app/docs/RELATO.md:1787`. |

Arquivos das PARADAS, conforme os `--stat` fornecidos, sem poder conferir os blobs das raias nesta cópia sem `.git`: **[nao verificado]**.

- **O10-C:** `app/config/crons.py`; `app/ponto/management/commands/trocas_plantao_lote_o10c.py`; `app/ponto/tests/test_o10c_lote_trocas.py`.
- **SUITE-POR-IMPACTO**, primeira raia: `bin/pre-push.sh`, `bin/recursos.sh`, `bin/regua.sh`, `bin/suite.sh`, `bin/suite_impacto.py`, `bin/suite_impacto.sh`, `bin/suite_impacto/{contexto_modulo.py,coveragerc,extrair_mapa.py,selos_so_na_principal.txt,sitecustomize.py}`, `bin/tests/{test_porta_le_env_da_casa.sh,test_suite_impacto_certificado.sh,test_suite_impacto_selecao.sh}`. Segunda raia também traz `bin/gerar_estado.py`, `bin/pista_estado.py`, `bin/trava_teste.sh`, `bin/tests/{test_medida_suites_por_pouso.sh,test_selo_espera_processo_morde.sh,test_suite_sh.sh,test_trava_teste.sh}`.
- **PADRAO-DE-CIMA**, raia pertinente `wt-pdc-v3-170430`: `bin/codex_pergunta.sh`, `bin/disjuntor.py`, `bin/hook_stop_fila1.py`, `bin/padrao_vocab.py`, `bin/tests/{_mordida.sh,test_disjuntor.sh,test_hook_esperando_com_prova.sh,test_lei_akita.sh,test_padrao_de_cima.sh}`. A primeira raia, aparentemente de outra obra, traz `.claude/settings.json`, `CLAUDE.md`, `app/docs/TICKETS.md`, `app/docs/contratos/CONTENCAO-QUE-MORDE-V2.txt`, `bin/{arvore_hash.sh,codex_pergunta.sh,contrato_do_push.py,disjuntor.py,hook_stop_fila1.py,marco_de_clear.py,pre-push.sh,suite.sh}`, `bin/tests/{test_contrato_do_push.sh,test_disjuntor.sh,test_handoff_sessao.sh,test_hora_da_secao_nao_e_futura.sh}`.
- **CODEX-CONTEXTO-DA-CASA:** `AGENTS.md`, `bin/{codex_brief_verificador.py,codex_limite_estado.py,codex_pergunta.sh,gerar_agents_md.py,relato.sh}`, `bin/tests/{test_agents_md_gerado.sh,test_codex_brief_verificador.sh}`.
- **OTIMIZA-1:** os 21 arquivos da segunda raia de SUITE-POR-IMPACTO, enumerados acima. [nao verificado]

**Raia aparentemente errada:** PADRAO-DE-CIMA → `wt-contencao-v4-111700`: `app/docs/BACKLOG.md:870` identifica precisamente seu head `3a743cdd` como **CONTENCAO-QUE-MORDE-V2**; a raia própria do PDC é identificada em `app/docs/RELATO.md:1595`.

**Soma deste recorte:** 2 NO-MAIN-COM-OUTRO-NOME + 2 ABSORVIDA-POR + 0 MORTA-POR-CORTE + 5 PARADA = **9**. [nao verificado: consolidação das outras 20 pelo agente principal]

## 12/11 14:17 · sessao de60d27cadd0 · QUARENTENA-REABRE · censo · origem=Code

O censo encontra **três divergências de sentido para DECISAO**. A divergência do silêncio vencido já está resolvida pelo corte existente: o código ainda precisa migrar. A fila devolve o vencido; verbos o transforma em `HISTORICO`; a lista do colaborador exige silêncio nulo. (`app/docs/CORTES.md:335`; `app/chamados/services/painel.py:190`; `app/chamados/services/verbos.py:51`; `app/chamados/services/janela_colab.py:73`)

A obra continua registrada como reprovada e aguardando reaplicação, com as curas posteriores nomeadas. (`app/docs/BACKLOG.md:765`)

Esta foi uma leitura estática, sem execução de suite, banco ou DIFF de frota. Resultados de execução desses verificadores: [nao verificado]. A identidade Git do arquivo recebido não pôde ser conferida por histórico Git nesta cópia: [nao verificado]. [nao verificado]

**1. Sítio a sítio do diff antigo**

Nesta tabela, os caminhos antigos são relativos a `app/` e vêm do anexo, sem confirmação histórica independente: [nao verificado]. As referências da terceira coluna são da cópia atual.

“Existe” significa que o sítio foi localizado; **não significa igualdade integral do corpo**. “Alterado” identifica evolução relevante. Para adições da especificação que não foram localizadas, registro “não localizado”: ausência atual não prova que o sítio nasceu e depois morreu. [nao verificado]

| Arquivo antigo::função/bloco — hunk antigo | Estado | Sítio no HEAD | O que o diff antigo fazia |
|---|---|---|---|
| `chamados/management/commands/arquivar_competencia_encerrada.py::Command.handle` — `-97` | Existe; guarda preservada | `app/chamados/management/commands/arquivar_competencia_encerrada.py:100` | Documentava `IS NULL` como “nunca teve véu”, evitando reaquivar vencido. |
| `chamados/management/commands/arquivar_prescritos.py::Command.handle` — `-50`, `-89` | Existem os dois blocos | `app/chamados/management/commands/arquivar_prescritos.py:53`; `app/chamados/management/commands/arquivar_prescritos.py:93` | Mesma explicação na seleção e na releitura antes da escrita. |
| `chamados/management/commands/arquivar_sem_dia_irrecuperavel.py::Command.handle` — `-55` | Existe | `app/chamados/management/commands/arquivar_sem_dia_irrecuperavel.py:58` | Preservava a guarda “nunca teve véu”. |
| `chamados/management/commands/auditar_invariantes_chamados.py::Command.handle/I5` — `-641` | Existe | `app/chamados/management/commands/auditar_invariantes_chamados.py:645` | Substituía comparação própria por `ch.silenciado_em(agora)`. |
| `chamados/management/commands/silenciar_chamados_isentos.py::Command` — arquivo | Existe; transformação ainda por fazer | `app/chamados/management/commands/silenciar_chamados_isentos.py:25`; `app/chamados/management/commands/silenciar_chamados_isentos.py:49` | Retirava `--apply` e escrita; passava a vigia da janela. |
| `chamados/models.py::ChamadoColaborador.silenciado_em` — `-303` | Existe, deslocado no arquivo | `app/chamados/models.py:508` | Acrescentava documentação do predicado por instância; a expressão continua `campo > agora`. |
| `chamados/models.py::QuarentenaColab/QuarentenaDiaDevolvido` — `-2667` | Adições não localizadas [nao verificado] | Registro atual da família: `app/core/portas.py:58`; obra pendente: `app/docs/BACKLOG.md:765` | Criava janela, causa, origem sem trilha, levantamento e exceção por dia devolvido. |
| `chamados/services/aviso_pendencia.py::perguntas_pendentes_do_colab` — `-10` | **Responsabilidade mudou de lugar**; wrapper permanece | `app/chamados/services/aviso_pendencia.py:7` → `app/chamados/services/pergunta_no_app.py:61` | Filtrava perguntas pela janela. Hoje a seleção pendente vem de `consulta_do_app`. |
| `chamados/services/cobrar_dia.py::cobrar_dia` — `-26` | Existe, **alterado** | `app/chamados/services/cobrar_dia.py:31`; `app/chamados/services/cobrar_dia.py:39` | Devolvia o dia e julgava em savepoint, desfazendo devolução/trilha sem escala. Hoje há toque registrado antes do cartório. |
| `chamados/services/disputa_emissao.py::gerar_perguntas_disputa` — `-289` | Existe, alterado | `app/chamados/services/disputa_emissao.py:300` | Carregava/cacheava janelas uma vez por disputa/rodada. |
| Mesmo sítio — `-386` | Existe, **fronteira alterada** | `app/chamados/services/disputa_emissao.py:421`; `app/chamados/services/disputa_emissao.py:439`; `app/chamados/services/disputa_emissao.py:444` | Barrava a pergunta isolada antes de abrir. Hoje há portas separadas de conversa e fato. |
| `chamados/services/disputa_emissao.py::criar_ou_atualizar_disputa` — `-594` | Existe | `app/chamados/services/disputa_emissao.py:634`; `app/chamados/services/disputa_emissao.py:662` | Acrescentava savepoint condicionado à janela e rollback da rodada inteiramente isolada. |
| Mesmo hunk::`_TudoIsolado` | Adição não localizada [nao verificado] | Fluxo atual: `app/chamados/services/disputa_emissao.py:669` | Sinalizava rollback sem disputa/agregação/push. |
| `chamados/services/disputa_emissao.py::_criar_pergunta_se_couber` — `-762` | Existe, deslocado | `app/chamados/services/disputa_emissao.py:823` | Perguntava ao juiz antes de abrir pergunta factual. |
| `chamados/services/disputa_emissao.py::criar_disputa_a_partir_de_chamados_orfaos` — `-860` | Existe, **alterado** | `app/chamados/services/disputa_emissao.py:1027`; `app/chamados/services/disputa_emissao.py:1045`; `app/chamados/services/disputa_emissao.py:1059` | Filtrava isolados na entrada. Hoje já filtra lista do admin e trava o conjunto do repasse. |
| `chamados/services/fila.py::imports` — `-30` | Existe | `app/chamados/services/fila.py:33` | Importava formas positiva/negativa do silêncio. |
| `chamados/services/fila.py::ROTULO_EXTRA` — `-42` | Existe | `app/chamados/services/fila.py:42` | Trocava histórico de vencido por quarentena. |
| `chamados/services/fila.py::por_verbo` — `-50` | Existe | `app/chamados/services/fila.py:53` | Propagava `agora` à partição. |
| `chamados/services/fila.py::idade_e_vazao` — `-89` | Existe | `app/chamados/services/fila.py:91` | Passava o mesmo instante à fila. |
| Mesmo sítio — `-119` | Existe | `app/chamados/services/fila.py:122`; `app/chamados/services/fila.py:126` | Contava arquivados/registrados pelas metades do juiz do silêncio. |
| Mesmo sítio — `-138` | Existe | `app/chamados/services/fila.py:141` | Passava o instante também a `por_verbo`. |
| `chamados/services/janela_colab.py::chamados_visiveis_colab` — `-48` | Existe | `app/chamados/services/janela_colab.py:49` | Trocava `IS NULL` por silêncio vigente e excluía quarentena. |
| `chamados/services/materializacao.py::_ressuscitar_por_resposta` — `-653` | Existe, alterado ao redor | `app/chamados/services/materializacao.py:700`; `app/chamados/services/materializacao.py:718` | Resgatava somente silêncio vigente, evitando trilha falsa para vencido. |
| `chamados/services/painel.py::etag_do_placar` — `-82` | Existe | `app/chamados/services/painel.py:79` | Corrigia a documentação da expiração. |
| `chamados/services/painel.py::qs_na_fila` — `-155`, `-167` | Existe; `agora` **já incorporado pela E2** | `app/chamados/services/painel.py:158`; `app/chamados/services/painel.py:186` | Acrescentava relógio recebido e documentava retorno do vencido. |
| Mesmo sítio + helpers — `-184` | Fila existe; helpers novos não localizados [nao verificado] | `app/chamados/services/painel.py:188` | Acrescentava quarentena e extraía `_q_silenciado`, `fora_do_silencio`, `silenciado_agora`. |
| `chamados/services/painel.py::montar_contexto_painel` — `-198`, `-210` | Existem os blocos | `app/chamados/services/painel.py:203`; `app/chamados/services/painel.py:215` | Retirava conta própria de arquivados. |
| Mesmo sítio — `-224` | Existe | `app/chamados/services/painel.py:229` | Fazia arquivados vir da mesma partição da pílula. |
| Mesmo sítio — `-303` | Existe, **alterado** | `app/chamados/services/painel.py:300`; `app/chamados/services/painel.py:339` | Lista padrão passava à fila; drill Arquivados seguia IDs da partição. Hoje também há modo resumo. |
| Mesmo sítio — `-390` | Existe | `app/chamados/services/painel.py:385` | Entregava `silenciado_agora` ao template. |
| `chamados/services/pergunta_no_app.py::CAUSAS` — `-20` | Existe, expandido | `app/chamados/services/pergunta_no_app.py:23` | Acrescentava causa quarentena, dona admin. |
| `chamados/services/pergunta_no_app.py::perguntas_do_app` — `-39` | Existe, **alterado** | `app/chamados/services/pergunta_no_app.py:45` | Filtrava janela; hoje precisa preservar identity map e tranca em lote. |
| `chamados/services/pergunta_no_app.py::retencao` — `-53` | Existe, alterado | `app/chamados/services/pergunta_no_app.py:121` | Quarentena antes de sem-dia/hoje/no-app; exige reaplicação do RED dessa ordem. |
| `chamados/services/pergunta_no_app.py::medir` — `-110` | Existe, alterado | `app/chamados/services/pergunta_no_app.py:178` | Carregava janelas em lote e as passava à retenção. |
| `chamados/services/quarentena.py` — arquivo novo | Não localizado [nao verificado] | Contrato pendente: `app/docs/BACKLOG.md:765` | Criava predicado, lote, porta, trilha, isenção e lista de vigentes. Não há substituto atual comprovado. |
| `chamados/services/silencio_ausencia.py::reconciliar_por_ausencia` — `-77` | Existe | `app/chamados/services/silencio_ausencia.py:81` | Trocava comparação própria por `silenciado_em`. |
| `chamados/services/silencio_isentos.py` — arquivo | Existe; forma antiga | `app/chamados/services/silencio_isentos.py:20`; `app/chamados/services/silencio_isentos.py:34` | Retirava os escritores e introduzia vigia. |
| `chamados/services/verbos.py::constantes` — `-10` | Existe, alterado | `app/chamados/services/verbos.py:13`; `app/chamados/services/verbos.py:18` | Retirava `HISTORICO`, acrescentava `QUARENTENA`. Hoje também há `FORA_DA_LISTA_PADRAO`, lida pela série E. |
| `chamados/services/verbos.py::particionar_verbos` — `-24` | Existe; `agora` já recebido | `app/chamados/services/verbos.py:30` | Usava os conjuntos do juiz para silêncio/quarentena, inclusive vivo sem verbo. |
| `chamados/services/vigia_fio.py::documentação/soma` — `-103` | Existe, evoluído | `app/chamados/services/vigia_fio.py:356`; `app/chamados/services/vigia_fio.py:449` | Acrescentava quarentena à partição do universo. |
| Mesmo arquivo::`censo` — `-211`, `-243`, `-263` | Existe, **alterado substancialmente** | `app/chamados/services/vigia_fio.py:318`; `app/chamados/services/vigia_fio.py:395`; `app/chamados/services/vigia_fio.py:428` | Criava contador, carregava janelas e desviava o dia isolado do alarme. Hoje existe segunda passada do dono. |
| Mesmo arquivo::`linhas` — `-385` | Existe, deslocado | `app/chamados/services/vigia_fio.py:853` | Imprimia quarentena sem alarmar. |
| `chamados/signals.py::_isentos_*` — `-197` | **Continuam vivos** | `app/chamados/signals.py:271`; `app/chamados/signals.py:285` | O diff os removia, substituindo escrita no chamado pela janela da flag. |
| `chamados/urls.py::urlpatterns` — `-28` | Bloco existe; rotas novas não localizadas [nao verificado] | `app/chamados/urls.py:26` | Acrescentava lista, abrir, levantar e corrigir início. |
| `chamados/views_arquivar.py::arquivar_lote` — `-123` | Existe | `app/chamados/views_arquivar.py:135` | Documentava guarda “nunca teve véu”. |
| Mesmo arquivo::`busca_arquivados` — `-166` | Existe | `app/chamados/views_arquivar.py:153` | Anotava `arquivado_agora` por instância. |
| Mesmo arquivo::`meus_arquivados` — começo do hunk `-193` | Existe | `app/chamados/views_arquivar.py:185` | Mesma anotação para o colaborador. |
| Mesmo hunk::`_recusa_da_quarentena`, `_empresa_visivel`, `_data_post`, `quarentenas`, `quarentena_abrir/levantar/corrigir_inicio` | Adições não localizadas [nao verificado] | Superfície atual: `app/chamados/views_arquivar.py:24`; contrato: `app/docs/BACKLOG.md:765` | Criava permissão conjunta, escopo, leitura e comandos HTTP da janela. |
| `colaboradores/services/colaborador.py::OPERACOES` — `-31` | Existe | `app/colaboradores/services/colaborador.py:34` | Retirava flag de cadastro e criava operação específica de isenção. |
| `colaboradores/services/ficha.py::ficha_do_colab` — `-157` | Existe, alterado | `app/colaboradores/services/ficha.py:154`; `app/colaboradores/services/ficha.py:160` | Retirava histórico de vencido; hoje lê `FORA_DA_LISTA_PADRAO`. |
| `colaboradores/services/isencao.py` — arquivo novo | Não localizado [nao verificado] | Escritor atual: `app/colaboradores/views.py:176` | Unificava flag, trilha e operação da janela. |
| `colaboradores/views.py::imports` — `-10` | Existe | `app/colaboradores/views.py:183` | Retirava import de auditoria tornado desnecessário com a delegação. |
| Mesmo arquivo::`passe_livre` — `-171` | Existe | `app/colaboradores/views.py:173` | Delegava troca à porta de isenção. |
| `core/juizes.py::_C16` — `-123` | **Identificador ocupado por outra pergunta** | `app/core/juizes.py:131` | Inscrevia quarentena; hoje `_C16` é veto de nascimento/reabertura. |
| Mesmo arquivo::`JUIZES['chamado']` — `-239` | Registro existe, expandido | `app/core/juizes.py:272`; `app/core/juizes.py:288` | Registrava o juiz já cortado de quarentena. Não copiar a chave antiga. |
| Mesmo arquivo::`PENDENTES['tela']` — `-735` | Pendente continua presente | `app/core/juizes.py:792` | Retirava comparação de relógio do template depois da migração. |
| `core/management/commands/lavrar_badge_navbar.py::Command.handle` — `-34`, `-46` | Existem | `app/core/management/commands/lavrar_badge_navbar.py:33`; `app/core/management/commands/lavrar_badge_navbar.py:47` | Atualizava comentários sobre histórico; esses hunks não acrescentavam escrita. |
| `core/portas.py::FAMILIA_DE` — `-69` | Bloco existe; entidades novas não localizadas [nao verificado] | `app/core/portas.py:70` | Incluía as entidades da janela na família chamado. |
| Mesmo arquivo::`PORTAS` — `-238` | Bloco existe; inscrições novas não localizadas [nao verificado] | `app/core/portas.py:237` | Declarava um escritor para ambas as entidades. |
| `ponto/management/commands/alertar_disputas_pendentes.py::Command.handle` — `-39`, `-117` | Existem | `app/ponto/management/commands/alertar_disputas_pendentes.py:42`; `app/ponto/management/commands/alertar_disputas_pendentes.py:118` | Pulava disputa cujas perguntas estivessem todas isoladas e imprimia a contagem. |
| `ponto/management/commands/escalonar_chamados_supervisao.py::Command.handle` — `-37` | Existe | `app/ponto/management/commands/escalonar_chamados_supervisao.py:42` | Recortava candidatos pelas metades de silêncio e quarentena. |
| `templates/chamados/partials/_busca_arquivados.html` — `-20`, `-43` | Existem os dois blocos | `app/templates/chamados/partials/_busca_arquivados.html:20`; `app/templates/chamados/partials/_busca_arquivados.html:46` | Acrescentava acesso à quarentena e consumia `arquivado_agora`. |
| `templates/chamados/partials/_lista_chamados.html` — `-82` | Existe | `app/templates/chamados/partials/_lista_chamados.html:85` | Botão Resgatar passava a ler silêncio vigente. |
| `templates/chamados/partials/_meus_arquivados.html` — `-30` | Existe | `app/templates/chamados/partials/_meus_arquivados.html:33` | Rótulo Arquivado deixava de depender de campo apenas preenchido. |
| `templates/chamados/partials/_quarentenas.html` — arquivo novo | Não localizado [nao verificado] | Obrigação de tela: `app/docs/BACKLOG.md:765` | Formulário, lista, rótulo sem trilha, correção e levantamento. |

**2. Censo pela regra e por AST**

O censo separa cobrança, verbo, mensagem não lida, universo, medição e ciclo. Essa distinção já existe no contrato; não se deve transformar todo leitor de `VIVOS` em leitor da fila. (`app/core/tests/test_tripwire_contagem_de_pendencia.py:66`)

Foram usados dois universos:

- **Varredura ampla:** 811 arquivos Python de `app/`, excluindo componentes `tests`, `migrations`, `docs` e `__pycache__`; zero erro de parse. Resultado desta execução sem artefato persistido: [nao verificado].
- **Tripwire existente:** 759 arquivos; exclui também nomes começando `test` e diretórios de ativos/logs. Encontrou 58 terminais de contagem por status, dois conformes e 25 funções não conformes, todas declaradas. Resultado desta execução sem artefato persistido: [nao verificado]. Algoritmo e exclusões: `app/core/tests/test_tripwire_contagem_de_pendencia.py:289`; `app/core/tests/test_tripwire_contagem_de_pendencia.py:317`.

A diferença entre os universos não significa “52 testes”: o corte por nome também alcança arquivos como `testar_pwa.py` e configurações de teste. Classificação integral desses 52: [nao verificado]. [nao verificado]

Este foi o script da varredura de contagem/chamadas, executado em memória, sem importar Django nem executar classes de teste: [nao verificado]

```python
import ast, pathlib, collections

p = pathlib.Path('app/core/tests/test_tripwire_contagem_de_pendencia.py')
tree = ast.parse(p.read_text())
tree.body = [
    n for n in tree.body
    if not isinstance(n, ast.ClassDef)
    and not (
        isinstance(n, ast.ImportFrom)
        and n.module and n.module.startswith('django')
    )
]
env = {'__file__': str(p.resolve())}
exec(compile(tree, str(p), 'exec'), env)

pairs = list(env['arquivos_de_producao']())
hits = [
    (r, *h) for r, s in pairs
    for h in env['contagens_por_status'](s)
]
print('UNIVERSO', len(pairs), 'CONTAGENS', len(hits),
      'CONFORMES', sum(h[-1] for h in hits))

nc = env['sitios_nao_conformes'](pairs)
for k, v in nc.items():
    print(k, v, env['DECLARADOS'].get(k, ('NOVO', ''))[0])

watch = {
    'qs_na_fila', 'qs_vivos', 'particionar_verbos',
    'perguntas_do_app', 'perguntas_pendentes_do_colab',
    'pergunta_viva', 'fio_vivo', 'gerar_perguntas_disputa',
    'criar_ou_atualizar_disputa', 'perguntar_pela_celula',
    'tarefa_do_dia', 'porta_alcancavel_do_dia',
    '_portas_da_tarefa', 'alimentar_portas', 'alimentar_fila',
    'colab_em_quarentena', 'fora_do_silencio', 'silenciado_agora'
}
for r, s in pairs:
    t = ast.parse(s)
    aliases = {
        a.asname or a.name: a.name
        for n in ast.walk(t) if isinstance(n, ast.ImportFrom)
        for a in n.names
    }
    out = collections.defaultdict(list)

    class V(ast.NodeVisitor):
        def __init__(self):
            self.stack = []
        def visit_ClassDef(self, n):
            self.stack.append(n.name)
            self.generic_visit(n)
            self.stack.pop()
        def visit_FunctionDef(self, n):
            self.stack.append(n.name)
            self.generic_visit(n)
            self.stack.pop()
        visit_AsyncFunctionDef = visit_FunctionDef
        def visit_Call(self, n):
            f = n.func
            name = (
                aliases.get(f.id, f.id) if isinstance(f, ast.Name)
                else f.attr if isinstance(f, ast.Attribute) else ''
            )
            if name in watch:
                out['.'.join(self.stack) or '<modulo>'].append(
                    f'{name}@{n.lineno}')
            self.generic_visit(n)

    V().visit(t)
    for fn, calls in out.items():
        print(r + '::' + fn, ' '.join(calls))
```

O complemento de campos percorreu os 811 arquivos, visitando `Attribute` com contexto `Load/Store`, argumentos nomeados, literais de campos e definições. Encontrou as atribuições explícitas de silêncio na porta `arquivo`; a flag é passada por dicionário e escrita pelo `setattr` da porta genérica. (`app/chamados/services/arquivo.py:166`; `app/chamados/services/arquivo.py:202`; `app/colaboradores/views.py:176`; `app/colaboradores/services/colaborador.py:59`)

Limite: o tripwire não certifica universos recebidos por parâmetro, filtros montados dinamicamente, `getattr` ou listas acumuladas por `append`. O scanner de chamadas reconhece aliases de `ImportFrom` e nomes finais de atributos, sem análise completa de tipos. Completude dinâmica: [nao verificado]. Limites já declarados do tripwire: `app/core/tests/test_tripwire_contagem_de_pendencia.py:31`.

Para interpretar as tabelas seguintes, os estados e seus escritores são:

| Estado lido | Quem o escreve hoje |
|---|---|
| **C — chamado:

[truncado em 20000 de 59441 caracteres]

## 12/11 14:16 · sessao 6cef734b37fa · AUDITORIA-DE-TRAVAS · leitura · origem=Code

**O v4 ainda não fecha o contrato.** Fecha várias ambiguidades do v3, mas a divisão do publicador continua imprecisa, a regra do disjuntor altera propriedades dos selos e a semente contém identidades não demonstradas, pousos em `[ORDEM]` e faltantes declarados como fila 1 aberta. (`contrato_item3_v4.txt:7`, `:11`, `:13`; `contrato_item3_v4_semente.txt:2`, `:9`, `:27`; `app/docs/RELATO.md:265`; `app/docs/BACKLOG.md:1980`)

Conto as linhas físicas dos anexos fornecidos a partir de 1. Nas referências abreviadas, `BACKLOG.md` e `RELATO.md` são os arquivos de `app/docs/`; os scripts e selos mantêm seus nomes. Pousos abaixo são **declarações documentais**; confirmação em produção: **[nao verificado]**.

**1. Furos do v3: o que o v4 fecha**

| Furo | Veredito sobre o v4 |
|---|---|
| **RED × “sem ser editado”** | **Fecha a contradição contratual:** conserva casos/assertivas e autoriza migrar a fixture, inclusive dirigir o leitor ao `.ids` temporário. A passagem efetiva do RED permanece **[nao verificado]**. (`contrato_item3_v4.txt:10`) |
| **Topo com TRAVA** | **Fecha no desenho:** topo travado continua em curso; seu veto deve excluí-lo; aparecem caso e mutante específicos. (`contrato_item3_v4.txt:4`, `:6`, `:9`) |
| **Gramática de TRAVA** | **Fecha:** nome livre, não vazio, aparado e sem `\|`; a enumeração deixa expressamente de ser fechada. (`contrato_item3_v4.txt:2`) |
| **Cauda/zonas do publicador** | **Não fecha operacionalmente:** limites e primeira migração continuam ambíguos; o intervalo manual começa três linhas antes do declarado; ausência no render atual não identifica autoria manual. Detalho no item 2. (`contrato_item3_v4.txt:7`; `BACKLOG.md:2371`, `:2372`, `:2373`; `app/core/juizes.py:1266`) |
| **945/944** | **Fecha a unidade e corrige o número desta cópia.** Recontagem em memória pelas funções atuais: **945 totais, 944 além do topo**. O “depois” ainda depende de semente válida. (`contrato_item3_v4.txt:12`; `bin/hook_stop_fila1.py:231`, `:270`; `BACKLOG.md:712`, `:2042`) |
| **Identidade fora de OBRAS** | **Fecha por mudança explícita da identidade:** qualquer tabela do BACKLOG, nas três seções, com exatamente uma linha. Isso aceita as linhas externas apontadas no v3; não resolve ids sem linha demonstrada. (`contrato_item3_v4.txt:3`; `BACKLOG.md:2404`, `:2407`, `:2408`, `:2417`; identidades ausentes: **[nao verificado]**) |
| **ErroFonte de IO/BACKLOG** | **Fecha a obrigação contratual:** ausência/ilegibilidade das fontes e decode inválido do `.ids` devem virar `ErroFonte`, bloqueando; outras exceções conservam a falha aberta. Implementação dessa conversão: **[nao verificado]**. (`contrato_item3_v4.txt:8`; `bin/hook_stop_fila1.py:1460`) |
| **Leitores omitidos** | **Fecha a enumeração anteriormente apontada:** inclui interrompidas, primeiras leituras do handoff, wrappers, fabricante, publicação e emendas documentais; distingue as perguntas documentais que ficam. A migração efetiva: **[nao verificado]**. (`contrato_item3_v4.txt:6`; `bin/interrompidas_estado.py:93`, `:105`, `:125`; `bin/relato.sh:115`, `:116`) |
| **Selos em conflito** | **Parcial:** declara corretamente a substituição da antiga autoridade da coluna. A promessa sobre o censo é falsa: a regra nova não preserva todas as propriedades, inclusive algumas que ela cita expressamente. Detalho no item 3. (`contrato_item3_v4.txt:11`; `bin/tests/test_hook_estado_e_a_coluna.sh:53`, `:54`, `:55`; `bin/tests/test_censo_antes_da_vaga.sh:542`, `:578`, `:603`) |
| **Semente** | **Não fecha:** corrigiu APP depois de TRIAGEM e trocou a exclusão indevida do delta O169, mas agora mantém esse delta já pousado em `[ORDEM]`; também mantém estados prováveis como desconhecidos e omite obras abertas. (`contrato_item3_v4_semente.txt:9`, `:14`, `:15`, `:16`, `:18`, `:22`; `RELATO.md:265`, `:266`, `:312`, `:8059`; `BACKLOG.md:1980`) |

Há ainda duas travas da semente sem sustentação nas evidências citadas:

- `LASTRO-FAIXA-SEM-DEFAULT` continua com `PERGUNTA-DE-DESENHO-S1`, embora sua linha diga que o aval responde o desenho e o aval fornecido libere B3 condicionado ao censo antes da c17. Essa pergunta não sustenta a trava mantida. (`contrato_item3_v4_semente.txt:6`; `BACKLOG.md:1813`; `aval_respostas_1211_1230.txt:6`)
- `RAIZ-DA-DISPUTA-F0F1 | TRAVA: ESTACIONADA` atribui à RAIZ o estado que a linha atribui a **BO-DISPUTA-SEM-PERGUNTA**. A RAIZ registra F0/F1 prontos, F1 respondida e c22 esperando vaga. (`contrato_item3_v4_semente.txt:11`; `BACKLOG.md:1882`; `RELATO.md:373`)

**2. G1/G2/G3 e preservação**

**A cláusula “TODO o resto” cobre o complemento em abstrato; a seção não define uma partição operacional inequívoca para a primeira migração.** Portanto não cabe afirmar perda inevitável, nem certificar preservação sem perda/duplicação pelo desenho atual. (`contrato_item3_v4.txt:7`)

O gerador inteiro hoje monta cabeçalho, contagem e tabelas por família; depois recupera uma janela antiga, insere-a antes da contagem e reescreve o arquivo. Essa janela começa no marcador antigo, quando presente, e termina em `OBRAS:FIM`; não preserva a cauda manual atual. (`bin/gerar_backlog.py:65`, `:76`, `:93`, `:100`, `:119`, `:127`, `:138`, `:140`)

| Parte | Problema concreto |
|---|---|
| **G1** | A primeira linha `<!--` é a **linha 7**, manual. “Até a primeira linha” precisa significar **antes dela**; a leitura inclusiva contradiz a preservação expressa dessa mesma linha. (`contrato_item3_v4.txt:7`; `BACKLOG.md:7`) |
| **G2** | É um bloco novo; o gerador atual preserva `ORDEM-VIVA-TOPO`, em vez de renderizar o `.ids`. A migração precisa inserir G2 e realizar a exceção expressa da lápide do marcador antigo. (`contrato_item3_v4.txt:7`; `BACKLOG.md:8`; `bin/gerar_backlog.py:119`, `:131`) |
| **G3** | A contagem começa em **2044**; o corpo gerado antigo termina em **2370**. O novo `GERADO:FIM` ainda precisa ser colocado na primeira migração, separando corpo antigo e conteúdo manual. (`BACKLOG.md:2044`, `:2365`, `:2370`; `bin/gerar_backlog.py:93`, `:100`, `:139`; `contrato_item3_v4.txt:7`) |
| **Migração manual** | O intervalo anunciado começa em **2374**, mas o conteúdo manual já começa em **2371**. (`contrato_item3_v4.txt:7`; `BACKLOG.md:2371`, `:2372`, `:2373`) |
| **Critério “render não produz”** | Não distingue manual de gerado obsoleto. A tabela BATIDA continua no BACKLOG, mas `PENDENTES['batida']` hoje está vazio: o render atual legitimamente deixa de produzi-la. Preservar todo negativo desse diff promoveria conteúdo gerado antigo a conteúdo manual. (`BACKLOG.md:2365`, `:2369`, `:2370`; `app/core/juizes.py:1266`; `bin/gerar_backlog.py:76`, `:78`) |
| **Byte a byte** | O mecanismo atual lê texto, remove/recompõe finais de linha e remonta a janela. A promessa nova exige substituir esse mecanismo, além de ampliar a preservação. (`bin/gerar_backlog.py:111`, `:127`, `:139`; `contrato_item3_v4.txt:7`) |

As linhas pedidas que **não caem em G1/G2/G3, OBRAS, CAUDA já delimitada nem no intervalo de migração anunciado** são:

- `SELO-MORDE-DO-PASSIVO-AFIRMA-MAIS-DO-QUE-MEDE`. (`BACKLOG.md:2371`)
- `MARCO-DO-PAR-DE-1490-MIN-NAO-FOI-MEDIDO`. (`BACKLOG.md:2372`)
- `ORFAOS-L-111-NO-FECHAMENTO-E-NO-ESPELHO`. (`BACKLOG.md:2373`)

Elas ficam cobertas **somente pelo comando geral “TODO o resto”**. Logo o defeito é a delimitação/migração incompleta; dizer que estão fora de toda obrigação de preservação seria incorreto. (`contrato_item3_v4.txt:7`)

**3. Disjuntor: propriedades de pertencimento**

**Não preserva todas.** Há quatro mudanças demonstráveis: positivo da célula qualificada, exato acompanhado de variante, ausência do marcador OBRAS e retorno do predicado chamado diretamente. (`contrato_item3_v4.txt:3`, `:4`, `:6`, `:11`; `bin/tests/test_censo_antes_da_vaga.sh:135`, `:293`, `:542`, `:578`, `:603`)

Na tabela, todas as linhas de selo pertencem a `bin/tests/test_censo_antes_da_vaga.sh`.

| Linhas | Propriedade atual | Efeito do v4 |
|---|---|---|
| **112–115** | FECHADA: wrapper rc0 e trilha FORA. | Preservável excluindo o id de `[ORDEM]`; deixa de decorrer da coluna. (`contrato_item3_v4.txt:4`, `:5`) |
| **116–120** | IDs válidos sem relação: rc0/FORA. | Preservável pela ausência exata e ausência de relação. (`contrato_item3_v4.txt:11`) |
| **121–124** | IDs ilegíveis ou vazios são recusados. | Depende de conservar a validação de entrada; o guard não a substitui. (`bin/disjuntor.py:2288`) |
| **125–130; 281–288** | BACKLOG ausente/ilegível/diretório: NAO SEI, recusa nas duas vias. | Coberto pela falha de fonte. (`contrato_item3_v4.txt:8`) |
| **131–135; 289–293** | Ausência de `OBRAS:INICIO`: NAO SEI/recusa. | **Muda:** identidade em qualquer ponto e lista de falhas não exigem esse marcador. Com `.ids` e linhas intactos, retirar só o marcador deixa de quebrar a fonte definida. (`contrato_item3_v4.txt:3`, `:8`) |
| **250–253** | Agent de FECHADA/FILA2 passa sem censo. | Preservável pela exclusão no `.ids`, não pelas palavras da coluna. (`contrato_item3_v4.txt:4`, `:5`) |
| **301–305** | Membro fora da forma do censo, `O9 (C)`: recusa por forma. | Exige conservar a validação posterior ao pertencimento. (`bin/disjuntor.py:1707`, `:1709`) |
| **534–538** | `OBRA-Q` perante `OBRA-Q (SILENCIADO-ATE-X)`, sem brief: rc1/NEGOU/PAREI. | A recusa pode continuar, mas DENTRO passa a NAO SEI. (`contrato_item3_v4.txt:3`, `:4`, `:11`) |
| **539–543** | **Mesma célula qualificada, brief conforme: rc0/APROVADO.** | **Muda para rc1/NEGOU:** o `.ids` precisa conter a célula inteira; `OBRA-Q` não pertence exatamente e cai no guard mesmo com brief conforme. (`contrato_item3_v4.txt:3`, `:4`, `:11`) |
| **544–548** | `O9` perante `O9 (C)`: NAO SEI; nunca FORA. | Intenção expressamente coberta pelo guard. (`contrato_item3_v4.txt:11`) |
| **549–551** | Caixa trocada: recusa. | Intenção expressamente coberta. (`contrato_item3_v4.txt:11`) |
| **552–555** | `OBRA` não casa `OBRA-Q`/`OBRA-E3`: rc0/FORA. | Depende de conservar a fronteira inteira, incluindo hífen; “token/caixa” não define essa gramática. (`contrato_item3_v4.txt:11`; `bin/hook_stop_fila1.py:110`, `:117`) |
| **556–558** | ID encontrado apenas na cauda: NAO SEI. | Não garantido: o texto novo não define se token interno da cauda conta. Hoje a busca alcança o id inteiro dentro da célula. (`contrato_item3_v4.txt:11`; `bin/disjuntor.py:1689`) |
| **559–563** | Agent com pedido qualificado ou nu: recusa com PAREI da obra normalizada. | Recusa preservável; classificação DENTRO muda para NAO SEI e depende da normalização do pedido. (`contrato_item3_v4.txt:11`; `bin/disjuntor.py:1746`) |
| **564–566** | Agent `O9` contra `O9 (C)`: nega e escreve “nao sei”. | Intenção coberta. (`contrato_item3_v4.txt:11`) |
| **567–574** | Dois candidatos qualificados: rc1/NEGOU, nunca FORA. | Preservável quando algum candidato estiver em `[ORDEM]`; essa condição passa a integrar o guard. (`contrato_item3_v4.txt:11`) |
| **575–578** | **`O9` exato mais `O9 (C)`: NAO SEI mesmo com brief conforme.** | **Muda para rc0/APROVADO:** `pertence(O9)=True` impede executar o guard, que exige **não pertencer**. IDs distintos não violam unicidade exata. (`contrato_item3_v4.txt:3`, `:4`, `:11`) |
| **579–582** | `O9` sozinho, conforme: rc0. | Preservável. (`contrato_item3_v4.txt:4`) |
| **583–591** | Espelho no Agent: pedido qualificado contra célula nua gera NAO SEI/PAREI, nunca FORA. | Não garantido: exige comparação nos dois sentidos, explicitada hoje e ausente da fórmula “token/caixa”. (`contrato_item3_v4.txt:11`; `bin/disjuntor.py:1677`, `:1689`) |
| **592–593** | Espelhos no wrapper: rc1 por forma, antes do pertencimento. | Preservável conservando validação; esse controle não prova o guard. (`bin/disjuntor.py:2288`) |
| **596–608** | **Três espelhos chamados diretamente em `_pertence_fila1` precisam lançar `ValueError`; False também reprova.** | **Muda literalmente para False** se esse predicado virar pertencimento exato. Um guard externo não preserva essa propriedade da função. (`contrato_item3_v4.txt:4`, `:6`, `:11`) |
| **609–619** | `O1`/`O142`, nos dois sentidos: False. | Preservável por igualdade exata; guard também precisa respeitar fronteiras. (`contrato_item3_v4.txt:4`, `:11`; `bin/hook_stop_fila1.py:117`) |
| **621–645** | Negativa conserva ID inteiro; parser e literal do PAREI são únicos. | Continua obrigação independente; o guard não resolve essa propriedade. (`bin/disjuntor.py:1637`; `contrato_item3_v4.txt:11`) |

Também há a propriedade transversal de **duas vias pelo mesmo predicado, com NEGOU e nunca FORA na ambiguidade**. Hoje wrapper e `_censo_nega` usam `_pertence_fila1`; qualquer classificação nova precisa alcançar ambas as vias. (`bin/tests/test_censo_antes_da_vaga.sh:516`, `:519`, `:524`, `:527`; `bin/disjuntor.py:1707`, `:2306`, `:2315`)

Os mutantes de célula crua, exato calando outra, dois candidatos, wrapper próprio, token cru, substring e espelho confirmam que essas obrigações excedem uma troca de fixture. (`bin/tests/test_censo_antes_da_vaga.sh:750`, `:751`, `:752`, `:753`, `:754`, `:757`, `:763`)

**Sim: a regra introduz um segundo classificador de pertinência para decidir a dispensa do censo.** O booleano exato retorna False tanto para variante quanto para id sem relação; distinguir NAO SEI de FORA exige derivar outra relação, por token/caixa, sobre BACKLOG e `[ORDEM]`. O v4 desloca essa classificação para o disjuntor, sem defini-la completamente. (`contrato_item3_v4.txt:4`, `:6`, `:11`; `bin/disjuntor.py:1688`, `:1695`, `:1707`, `:2311`)

**4. Semente: identidade exata e estado documental**

Cada ID abaixo reproduz a primeira célula que deve ser procurada; a coluna seguinte registra o que a linha do BACKLOG declara. (`contrato_item3_v4.txt:3`; `contrato_item3_v4_semente.txt:1–33`)

| Seção | ID exato | Linha e estado declarado |
|---|---|---|
| ORDEM | `SAIDA-NOTURNA-NO-DIA-CERTO` | Linha exata: **[nao verificado]**. Há aval/despacho no RELATO. (`RELATO.md:29`) |
| ORDEM | `QUARENTENA-DO-COLAB` | REPROVADA; espera a vez. Pertencimento literal à fila 1 nessa linha: **[nao verificado]**. (`BACKLOG.md:765`) |
| ORDEM | `BUG-CADASTRO-NAO-ALCANCA-O-FUTURO` | **POS-20, frente da fila 2**; construção na próxima vaga. (`BACKLOG.md:2408`) |
| ORDEM | `FIO-E0-V4-E3-LEITORES` | Fila 1, ABERTA, EM OBRA. (`BACKLOG.md:1873`) |
| ORDEM | `LASTRO-FAIXA-SEM-DEFAULT` | Fila 1, ABERTA, c3; aval responde o desenho. (`BACKLOG.md:1813`) |
| ORDEM | `FIO-E0-V4-E4-ESCRITORES` | Fila 1; etapas E4a–E4d; E4a-1 NO AR, sem fechamento da obra inteira. (`BACKLOG.md:1885`) |
| ORDEM | `C20-O168` | Fila 1, espera vaga, ABERTA. (`BACKLOG.md:2008`) |
| ORDEM | `O169-FECHADA` | Fila 1 REGISTRAR; delta próprio em B3/volta 5; cita pouso antigo de C20-O169. **O próprio v5 também pousou no RELATO.** (`BACKLOG.md:1969`; `RELATO.md:265`) | [nao verificado]
| ORDEM | `RAIZ-DA-DISPUTA-F0-CASO-7279` | Fila 1, F0/sonda, frente da F1. (`BACKLOG.md:2417`) |
| ORDEM | `RAIZ-DA-DISPUTA-F0F1` | Fila 1; F0/F1 prontos, F1 respondida, c22 espera vaga. “ESTACIONADA” qualifica BO-DISPUTA. (`BACKLOG.md:1882`) |
| ORDEM | `AUDITORIA-DE-TRAVAS-CORTE` | Instrumento EM CURSO. (`BACKLOG.md:2404`) |
| ORDEM | `BUG-ORFAO-COM-PAR` | ESTRUTURA fila 1; sonda, classe e cura pendentes. (`BACKLOG.md:2418`) |
| ORDEM | `TRIAGEM-OFFLINE-SEM-CAMERA` | REVERTIDA; retorno condicionado à classe e RED. (`BACKLOG.md:1915`) |
| ORDEM | `APP-MENSAGENS-MSG-0` | Fila 1/raia 2, ABERTA, nada construído; depois da TRIAGEM. (`BACKLOG.md:1916`) |
| ORDEM | `BO-0611-05` | Linha própria ainda B3/PARADA. (`BACKLOG.md:1897`) |
| ORDEM | `BO-DISPUTA-SEM-PERGUNTA` | Linha própria registra construtor lançado. (`BACKLOG.md:1898`) |
| ORDEM | `TRAVA-MENOS-FILA-COM-SELO` | Fila 1 instrumento, ABERTA, preverif v1. (`BACKLOG.md:2396`) |
| ORDEM | `MEDE-CLEAR-X-COMPACT` | Fila 1 medida, em curso, ABERTA. (`BACKLOG.md:1796`) |
| ORDEM | `CODEX-QUEIMA-B` | Fila 1 medida, sondas rodando, ABERTA. (`BACKLOG.md:1797`) |
| ORDEM | `C20-FIO-MUDO` | Fila 1, construtor aberto, ABERTA. (`BACKLOG.md:1817`) |
| ORDEM | `E3-X1-FOLGA-NAO-E-SEM-COBERTURA` | Fila 1, ABERTA, construtor após c8b. (`BACKLOG.md:1876`) |
| ORDEM | `SONDA-SEM-GRANT` | Fila 1 instrumento, ABERTA; sondas congeladas até pouso. (`BACKLOG.md:1979`) | [nao verificado]
| ORDEM | `AUDITORIA-0911` | Fila 1 medida, ABERTA; sondas após SONDA-SEM-GRANT. (`BACKLOG.md:1981`) |
| POS-20 | `UI-PAINEL-FILA-OPUS-V5` | POS-20, fila/registrado; pouso citado pertence à tela original. (`BACKLOG.md:2407`) | [nao verificado]
| POS-20 | `AUDITORIA-ITEM6-CONTADOR-124` | Linha exata: **[nao verificado]**. RELATO diz “não construído → fila”, sem classificá-lo como POS-20. (`RELATO.md:9`) |
| POS-20 | `TRIAGEM-FILA-OPUS-V6` | POS-20, ABERTA. (`BACKLOG.md:1815`) |
| POS-20 | `E4B-FILA-OPUS-C14` | POS-20, fila do opus final. (`BACKLOG.md:1896`) |
| POUSADAS | `FIO-E0-V4-E4B` | POUSOU 12/11 08:46, commit 087fe758a. (`BACKLOG.md:1894`) |
| POUSADAS | `FIO-E0-V4-E4A-2` | NO AR, merge 613a51498. (`BACKLOG.md:1893`) |
| POUSADAS | `C20-O169` | POUSADA 05/11, FECHADA. (`BACKLOG.md:1798`) |

Para os **nove `TRAVA: ESTADO-NAO-PROVADO`**, a reconciliação documental é:

| ID | Estado sustentado por BACKLOG + RELATO |
|---|---|
| `BO-0611-05` | **POUSADA.** O RELATO registra merge fb9269ea9 e deploy rc0, superando o retrato B3 da linha própria. (`BACKLOG.md:1897`; `RELATO.md:451`, `:452`, `:8059`) |
| `BO-DISPUTA-SEM-PERGUNTA` | **ESTACIONADA.** c21 não pousa; raia preservada. (`BACKLOG.md:1882`; `RELATO.md:373`) |
| `TRAVA-MENOS-FILA-COM-SELO` | **POUSADA**, sob o nome curto TRAVA-SELO. A ligação entre os nomes aparece no despacho, cópia e artefatos; depois há pouso/deploy. (`BACKLOG.md:2396`; `RELATO.md:8100`, `:8107`, `:312`) | [nao verificado]
| `MEDE-CLEAR-X-COMPACT` | **ABERTA documentalmente.** Há PARTE 1 e resultados, sem fechamento demonstrado nessas evidências. (`BACKLOG.md:1796`; `RELATO.md:7696`, `:7703`) |
| `CODEX-QUEIMA-B` | **ABERTA documentalmente.** Há despacho/resultados intermediários, sem fechamento demonstrado. (`BACKLOG.md:1797`; `RELATO.md:7694`, `:1241`) |
| `C20-FIO-MUDO` | **ABERTA/incompleta.** O RELATO exige manter a linha aberta pelo escopo incompleto; isso não prova ausência de pouso parcial. (`BACKLOG.md:1817`; `RELATO.md:1218`, `:7995`, `:7999`) | [nao verificado]
| `E3-X1-FOLGA-NAO-E-SEM-COBERTURA` | **Fase X1 POUSADA**, com a ressalva literal “X1 6 de 7” e apply pós-pouso; não fecha toda E3-LEITORES. (`BACKLOG.md:1876`; `RELATO.md:266`, `:185`) | [nao verificado]
| `SONDA-SEM-GRANT` | **ABERTA/congelada.** Ainda faltam volta 8, verificações e pouso. (`BACKLOG.md:1979`; `RELATO.md:591`, `:616`) | [nao verificado]
| `AUDITORIA-0911` | **ABERTA.** Medidas A–E ainda faltantes/congeladas. (`BACKLOG.md:1981`; `RELATO.md:596`, `:698`) |

**ID inequívoco em `[ORDEM]` já pousado: `O169-FECHADA`.** É o pouso do próprio delta v5, com merge, deploy, apply de três casos e DRY seguinte vazio; não depende de confundi-lo com C20-O169. (`contrato_item3_v4_semente.txt:9`; `RELATO.md:265`) [nao verificado]

**Faltante explícito: `SONDA-MATRIZ-DE-TRANSICOES`.** O BACKLOG declara fila 1 instrumento **ABERTA**, e a semente não o contém. Também faltam `SEGREDO-ARGV-RESIDUO` e `SOMBRA-CARIMBO-MENTE`, ambos literalmente fila 1 instrumento ABERTA. (`BACKLOG.md:1980`, `:1982`, `:1983`; `contrato_item3_v4_semente.txt:1–34`)

Por fim, a nota da semente erra o endereço de BUG-CADASTRO: **2417 é RAIZ-F0-CASO-7279; BUG-CADASTRO está em 2408**. O aval fornecido escolhe a candidata 4(b) e exige publicar N antes de construir; não declara transferência de POS-20/fila 2 para fila 1. Essa transferência fica **[nao verificado]**. (`contrato_item3_v4_semente.txt:34`; `BACKLOG.md:2417`, `:2408`; `aval_respostas_1211_1230.txt:2`)

## 12/11 14:13 · sessao e4190b53cf69 · - · - · origem=Ronald (tmux)

Quadro da semente, mantendo a primeira célula exata:

| Seção | ID | BACKLOG e estado declarado |
|---|---|---|
| ORDEM | SAIDA-NOTURNA-NO-DIA-CERTO | Linha exata: **[nao verificado]**. RELATO registra aval e despacho, sem fornecer linha própria (`app/docs/RELATO.md:29`). |
| ORDEM | QUARENTENA-DO-COLAB | REPROVADA, espera a vez; não declara pouso (`app/docs/BACKLOG.md:765`). |
| ORDEM | BUG-CADASTRO-NAO-ALCANCA-O-FUTURO | POS-20/frente da fila 2, próxima vaga de construção (`app/docs/BACKLOG.md:2408`). |
| ORDEM | FIO-E0-V4-E3-LEITORES | Fila 1, ABERTA, EM OBRA (`app/docs/BACKLOG.md:1873`). |
| ORDEM | LASTRO-FAIXA-SEM-DEFAULT | Fila 1, ABERTA; aval responde o DESENHO (`app/docs/BACKLOG.md:1813`). |
| ORDEM | FIO-E0-V4-E4-ESCRITORES | Fila 1, obra em etapas E4a–E4d; E4a-1 NO AR, sem declarar toda a obra fechada (`app/docs/BACKLOG.md:1885`). |
| ORDEM | C20-O168 | Fila 1, espera vaga, ABERTA (`app/docs/BACKLOG.md:2008`). |
| ORDEM | O169-FECHADA | Fila 1 REGISTRAR; delta próprio em B3/volta 5, cita pouso antigo de C20-O169 (`app/docs/BACKLOG.md:1969`). **Seu próprio v5 também pousou**, segundo RELATO (`app/docs/RELATO.md:265`). |
| ORDEM | RAIZ-DA-DISPUTA-F0-CASO-7279 | Fila 1, F0/sonda, frente da F1 (`app/docs/BACKLOG.md:2417`). |
| ORDEM | RAIZ-DA-DISPUTA-F0F1 | Fila 1, F0+F1 prontos, F1 respondida, c22 aguarda vaga. O sujeito de **ESTACIONADA é BO-DISPUTA**, não esta RAIZ (`app/docs/BACKLOG.md:1882`). |
| ORDEM | AUDITORIA-DE-TRAVAS-CORTE | Instrumento EM CURSO (`app/docs/BACKLOG.md:2404`). |
| ORDEM | BUG-ORFAO-COM-PAR | ESTRUTURA fila 1, sonda/classe/cura pendentes (`app/docs/BACKLOG.md:2418`). |
| ORDEM | TRIAGEM-OFFLINE-SEM-CAMERA | REVERTIDA; retorno condicionado à classe e RED (`app/docs/BACKLOG.md:1915`). |
| ORDEM | APP-MENSAGENS-MSG-0 | Fila 1/raia 2, ABERTA, nada construído (`app/docs/BACKLOG.md:1916`). |
| ORDEM | BO-0611-05 | B3/PARADA, retrato documental desatualizado diante do pouso no RELATO (`app/docs/BACKLOG.md:1897`; `app/docs/RELATO.md:451`). |
| ORDEM | BO-DISPUTA-SEM-PERGUNTA | Construtor lançado; a linha própria não informa estacionamento posterior (`app/docs/BACKLOG.md:1898`). |
| ORDEM | TRAVA-MENOS-FILA-COM-SELO | Fila 1 instrumento, ABERTA, preverif v1 (`app/docs/BACKLOG.md:2396`). |
| ORDEM | MEDE-CLEAR-X-COMPACT | Fila 1 medida, em curso, ABERTA (`app/docs/BACKLOG.md:1796`). |
| ORDEM | CODEX-QUEIMA-B | Fila 1 medida, sondas rodando, ABERTA (`app/docs/BACKLOG.md:1797`). |
| ORDEM | C20-FIO-MUDO | Fila 1, construtor aberto, ABERTA (`app/docs/BACKLOG.md:1817`). |
| ORDEM | E3-X1-FOLGA-NAO-E-SEM-COBERTURA | Fila 1, ABERTA, construtor após c8b (`app/docs/BACKLOG.md:1876`). |
| ORDEM | SONDA-SEM-GRANT | Fila 1 instrumento, ABERTA; sondas congeladas até pouso (`app/docs/BACKLOG.md:1979`). |
| ORDEM | AUDITORIA-0911 | Fila 1 medida, ABERTA; sondas após SONDA-SEM-GRANT (`app/docs/BACKLOG.md:1981`). |
| POS-20 | UI-PAINEL-FILA-OPUS-V5 | POS-20, fila/registrado; pouso citado pertence à tela original (`app/docs/BACKLOG.md:2407`). |
| POS-20 | AUDITORIA-ITEM6-CONTADOR-124 | Linha exata: **[nao verificado]**. RELATO diz não construído → fila, sem declarar POS-20 (`app/docs/RELATO.md:9`). |
| POS-20 | TRIAGEM-FILA-OPUS-V6 | POS-20, ABERTA (`app/docs/BACKLOG.md:1815`). |
| POS-20 | E4B-FILA-OPUS-C14 | POS-20, fila do opus final (`app/docs/BACKLOG.md:1896`). |
| POUSADAS | FIO-E0-V4-E4B | POUSOU 12/11 08:46, 087fe758a (`app/docs/BACKLOG.md:1894`). |
| POUSADAS | FIO-E0-V4-E4A-2 | NO AR, merge 613a51498 (`app/docs/BACKLOG.md:1893`). |
| POUSADAS | C20-O169 | POUSADA 05/11, FECHADA (`app/docs/BACKLOG.md:1798`). |

Os nove `ESTADO-NAO-PROVADO`:

| ID | Estado documental sustentado |
|---|---|
| BO-0611-05 | **POUSADA**, apesar da linha própria velha: merge fb9269ea9 e deploy rc=0; push posterior também registrado. Smoke devido não desfaz o pouso documental (`app/docs/RELATO.md:451`, `:452`, `:8059`, `:8073`; `app/docs/BACKLOG.md:1897`). |
| BO-DISPUTA-SEM-PERGUNTA | **ESTACIONADA**, sem pouso da c21; raia preservada (`app/docs/BACKLOG.md:1882`; `app/docs/RELATO.md:373`). |
| TRAVA-MENOS-FILA-COM-SELO | **POUSADA**, sob nome curto TRAVA-SELO. Ponte explícita: despacho com nome longo e a cópia `wt-trava-selo-063008`, artefatos `logs/TRAVA-SELO`; depois pouso 9ca563bb3/deploy OK (`app/docs/RELATO.md:8100`, `:8107`, `:312`; `app/docs/BACKLOG.md:2396`). |
| MEDE-CLEAR-X-COMPACT | **ABERTA documentalmente**; RELATO fornece **PARTE 1**, números e ausência de recomendação, sem declarar encerramento da obra (`app/docs/BACKLOG.md:1796`; `app/docs/RELATO.md:7696`, `:7703`). |
| CODEX-QUEIMA-B | **ABERTA documentalmente**; há despacho e resultados intermediários, sem fechamento demonstrado (`app/docs/BACKLOG.md:1797`; `app/docs/RELATO.md:7694`, `:1241`). |
| C20-FIO-MUDO | **ABERTA/incompleta**. O RELATO exige manter linha aberta pelo escopo incompleto; os registros citados ainda condicionam pouso. Não afirmar “nunca pousou parcialmente” (`app/docs/BACKLOG.md:1817`; `app/docs/RELATO.md:1218`, `:7995`, `:7999`). |
| E3-X1-FOLGA-NAO-E-SEM-COBERTURA | **Fase X1 pousada**, declarada 6/7, com apply pós-pouso; isso não fecha toda E3-LEITORES (`app/docs/RELATO.md:266`, `:185`; linha antiga ABERTA: `app/docs/BACKLOG.md:1876`). |
| SONDA-SEM-GRANT | **ABERTA/congelada**, falta volta 8, preverif, opus e pouso (`app/docs/BACKLOG.md:1979`; `app/docs/RELATO.md:591`, `:616`). |
| AUDITORIA-0911 | **ABERTA**, medida A–E ainda faltante/congelada (`app/docs/BACKLOG.md:1981`; `app/docs/RELATO.md:596`, `:698`). |

**Id inequívoco em ORDEM já pousado:** `O169-FECHADA`, próprio v5, merge ca978f65d, deploy rc=0, apply 3 e DRY seguinte vazio (`app/docs/RELATO.md:265`). Outros pousos documentados indevidamente tratados como desconhecidos: BO-0611-05, TRAVA-MENOS-FILA-COM-SELO e fase X1 (`app/docs/RELATO.md:451`, `:312`, `:266`).

**Faltante explícito forte:** `SONDA-MATRIZ-DE-TRANSICOES` é fila 1 instrumento **ABERTA** e não aparece na semente v4 (`app/docs/BACKLOG.md:1980`; `contrato_item3_v4_semente.txt:1`–`:34`). Outros exemplos literais: `SEGREDO-ARGV-RESIDUO`, `SOMBRA-CARIMBO-MENTE`, as três `ESMERIL-SEM-*` e três `COBPARC-*` abaixo (`app/docs/BACKLOG.md:1982`, `:1983`, `:1830`, `:1831`, `:1832`, `:772`, `:773`, `:774`; `contrato_item3_v4_semente.txt:1`–`:34`).

**Erro na nota da semente:** `:2417` é RAIZ-DA-DISPUTA-F0-CASO-7279; BUG-CADASTRO/POS-20 está em `:2408` (`app/docs/BACKLOG.md:2417`, `:2408`; `contrato_item3_v4_semente.txt:34`).

## 12/11 14:12 · sessao 301c3ab5c937 · - · - · origem=Ronald (tmux)

**A regra v4 não preserva todas as propriedades.** Há quatro conflitos concretos: o positivo de célula qualificada, o exato acompanhado de variante, a exigência de marcador OBRAS e as asserções diretas do predicado. (`bin/tests/test_censo_antes_da_vaga.sh:542`, `:578`, `:135`, `:293`, `:603`; `contrato_item3_v4.txt:3`, `:4`, `:6`, `:11`)

Contagem dos anexos: título do v4 = linha 1; seção 4/conflitos = linha 11. (`contrato_item3_v4.txt:1`, `:11`)

| Propriedade que toca pertencimento | Linhas do selo | Efeito do v4 |
|---|---|---|
| FECHADA fora: wrapper rc0 e trilha FORA | `bin/tests/test_censo_antes_da_vaga.sh:112`–`:115` | Preservável excluindo esse id de `[ORDEM]`; deixa de derivar da coluna. (`contrato_item3_v4.txt:4`, `:5`) |
| IDs válidos sem relação (`ABC`, `09-TURNO-ABERTO-EXPOSTA`) fora, rc0 | `bin/tests/test_censo_antes_da_vaga.sh:116`–`:120` | Preservável: ausência exata e ausência de relação. (`contrato_item3_v4.txt:11`) |
| IDs ilegíveis recusados (`../x`, `OBRA/E3`, vazio) | `bin/tests/test_censo_antes_da_vaga.sh:121`–`:124` | Não é resolvido pelo guard de alias; depende de conservar validação da entrada atual. (`bin/disjuntor.py:2288`) |
| BACKLOG ausente ⇒ NAO SEI/rc1 | `bin/tests/test_censo_antes_da_vaga.sh:125`–`:130` | Coberto pela falha de fonte. (`contrato_item3_v4.txt:8`) |
| BACKLOG sem `OBRAS:INICIO` ⇒ NAO SEI/rc1 | `bin/tests/test_censo_antes_da_vaga.sh:131`–`:135` | **Muda:** v4 valida linhas em qualquer ponto e não exige marcador OBRAS. Mantidos `.ids` e linhas, remover apenas esse marcador deixa de ser erro definido. (`contrato_item3_v4.txt:3`, `:8`) |
| Agent: FECHADA e FILA2 passam | `bin/tests/test_censo_antes_da_vaga.sh:250`–`:253` | Preservável excluindo-os de `[ORDEM]`; classificação não vem mais dessas palavras. (`contrato_item3_v4.txt:4`, `:5`) |
| Agent: BACKLOG ausente ou diretório ⇒ nega | `bin/tests/test_censo_antes_da_vaga.sh:281`–`:288` | Coberto por ausência/ilegibilidade. (`contrato_item3_v4.txt:8`) |
| Agent: BACKLOG sem marcador OBRAS ⇒ nega | `bin/tests/test_censo_antes_da_vaga.sh:289`–`:293` | **Muda pelo mesmo motivo do CLI.** (`contrato_item3_v4.txt:3`, `:8`) |
| Id membro fora da forma do censo (`O9 (C)`) ⇒ nega, com motivo de forma | `bin/tests/test_censo_antes_da_vaga.sh:301`–`:305` | Preservável conservando validação depois de reconhecer membro; alias guard não substitui essa propriedade. (`bin/disjuntor.py:1707`–`:1712`) |
| Célula qualificada `OBRA-Q (SILENCIADO-ATE-X)` reconhece pedido `OBRA-Q`; sem brief rc1, NEGOU, PAREI | `bin/tests/test_censo_antes_da_vaga.sh:534`–`:538` | Recusa externa pode continuar, mas v4 passa a classificá-la como NAO SEI, em vez de DENTRO. (`contrato_item3_v4.txt:3`, `:4`, `:11`) |
| **A mesma célula qualificada com brief conforme ⇒ rc0/APROVADO** | `bin/tests/test_censo_antes_da_vaga.sh:539`–`:543` | **Muda necessariamente:** identidade exata impede colocar `OBRA-Q` no `.ids` perante aquela célula. `[ORDEM]` tem a célula inteira; pedido nu não pertence; guard devolve NAO SEI mesmo com brief conforme. **rc0/APROVADO vira rc1/NEGOU.** (`contrato_item3_v4.txt:3`, `:4`, `:11`) |
| Pedido `O9` perante `O9 (C)` ⇒ NAO SEI, não FORA | `bin/tests/test_censo_antes_da_vaga.sh:544`–`:548` | Intenção explicitamente coberta pelo v4. (`contrato_item3_v4.txt:11`) |
| Caixa trocada `obra-q` ⇒ recusa | `bin/tests/test_censo_antes_da_vaga.sh:549`–`:551` | Intenção explicitamente coberta. (`contrato_item3_v4.txt:11`) |
| `OBRA` não casa `OBRA-Q`/`OBRA-E3`: rc0/FORA | `bin/tests/test_censo_antes_da_vaga.sh:552`–`:555` | Só fica preservada se “token/caixa” mantiver fronteira que exclui hífen; v4 não define essa gramática. Hoje é regex explícito. (`bin/hook_stop_fila1.py:110`–`:117`; `contrato_item3_v4.txt:11`) |
| Id que aparece apenas na cauda (`SILENCIADO-ATE-X`) ⇒ NAO SEI | `bin/tests/test_censo_antes_da_vaga.sh:556`–`:558` | **Não garantido:** “token/caixa” não declara se inclui token interno da cauda; hoje há busca do id inteiro na célula. (`bin/disjuntor.py:1689`; `contrato_item3_v4.txt:11`) |
| Agent com obra qualificada ou nua ⇒ nega com PAREI `OBRA-Q` | `bin/tests/test_censo_antes_da_vaga.sh:559`–`:563` | Recusa preservável pelo guard, mas envolve também normalização do pedido; a propriedade DENTRO muda para NAO SEI. (`bin/disjuntor.py:1746`; `contrato_item3_v4.txt:11`) |
| Agent `O9` contra `O9 (C)` ⇒ nega e declara “nao sei” | `bin/tests/test_censo_antes_da_vaga.sh:564`–`:566` | Intenção coberta. (`contrato_item3_v4.txt:11`) |
| Dois candidatos `OBRA-Q (A)` e `(B)` ⇒ rc1/NEGOU, nunca FORA | `bin/tests/test_censo_antes_da_vaga.sh:567`–`:574` | Preservável se pelo menos um candidato estiver em `[ORDEM]`; o v4 condiciona o guard a isso. Não exige unicidade de alias, só de id exato. (`contrato_item3_v4.txt:3`, `:11`) |
| **Exato `O9` mais `O9 (C)` ⇒ NAO SEI mesmo com brief conforme** | `bin/tests/test_censo_antes_da_vaga.sh:575`–`:578` | **Muda:** estando `O9` em `[ORDEM]`, `pertence(O9)=True`; o guard só roda quando **não pertence**. Com o brief conforme já criado em `:545`, a resposta passa a rc0/APROVADO. IDs distintos não violam identidade exata. (`contrato_item3_v4.txt:3`, `:4`, `:11`) |
| `O9` sozinho, brief conforme ⇒ rc0 | `bin/tests/test_censo_antes_da_vaga.sh:579`–`:582` | Preservável. (`contrato_item3_v4.txt:4`) |
| Espelho Agent: `O9 (C)` ou `O9 NOTURNO-X` contra célula nua `O9` ⇒ nega/PAREI e nunca FORA | `bin/tests/test_censo_antes_da_vaga.sh:583`–`:591` | **Não garantido:** precisa do segundo sentido da comparação. Hoje ele está explicitamente codificado; “token/caixa” não especifica direção. (`bin/disjuntor.py:1677`–`:1689`; `contrato_item3_v4.txt:11`) |
| Os mesmos espelhos no wrapper ⇒ rc1 por forma inválida, antes de pertencer | `bin/tests/test_censo_antes_da_vaga.sh:592`–`:593` | Preservável mantendo a validação do CLI; esse controle não prova o guard. (`bin/disjuntor.py:2288`–`:2290`) |
| **Predicado direto:** três espelhos precisam lançar `ValueError`; tanto False como True reprovam | `bin/tests/test_censo_antes_da_vaga.sh:596`–`:608` | **Muda literalmente:** v4 manda `_pertence_fila1` usar pertencimento exato; resultado será False. Guard separado pode conservar negativa na fronteira, mas não a propriedade desse predicado. (`contrato_item3_v4.txt:4`, `:6`, `:11`) |
| Prefixos numéricos `O1`/`O142`, nos dois sentidos ⇒ False | `bin/tests/test_censo_antes_da_vaga.sh:609`–`:615` | Preservável por igualdade exata, e no guard somente conservando fronteiras inteiras. (`contrato_item3_v4.txt:4`, `:11`; `bin/hook_stop_fila1.py:117`) |
| NEGOU deve conservar ID inteiro, parser único e literal único do PAREI | `bin/tests/test_censo_antes_da_vaga.sh:621`–`:645` | Continua sendo obrigação independente da identidade; não é resolvida pelo guard. (`bin/disjuntor.py:1637`–`:1641`) |

A propriedade transversal é **as duas vias pelo mesmo predicado e NEGOU nunca FORA em ambiguidade**; ela está declarada em `bin/tests/test_censo_antes_da_vaga.sh:516`–`:527`. Hoje wrapper chama `_pertence_fila1` e, se True, passa a mesma fila a `_censo_nega`; esta repete o mesmo predicado. (`bin/disjuntor.py:2306`–`:2315`, `:1707`) A migração precisa aplicar qualquer classificação de variante às duas vias; o texto v4 não fecha uma interface única para essa classificação. (`contrato_item3_v4.txt:6`, `:11`)

Os mutantes também evidenciam que há propriedades além de fixture: célula crua, exato calando outra, dois candidatos, wrapper próprio, token cru, substring e espelho são mortos explicitamente. (`bin/tests/test_censo_antes_da_vaga.sh:750`–`:757`, `:763`) Trocar somente fixture não preserva simultaneamente a identidade exata nova e esses resultados antigos. (`contrato_item3_v4.txt:3`, `:4`, `:11`)

**É uma segunda derivação de pertinência/incerteza para a admissão do censo.** `pertence()` exato devolve False tanto para variante como para id sem relação; a distinção NAO SEI/FORA exige outra relação sobre token/caixa, lendo BACKLOG e cruzando seus ids com `[ORDEM]`. Portanto não se deriva apenas do booleano `pertence()`. O v4 mantém o booleano exato, mas introduz no disjuntor um segundo classificador que decide se esse False dispensa censo; não elimina o juiz fuzzy, apenas o desloca. (`contrato_item3_v4.txt:4`, `:6`, `:11`; mecanismo atual comparável em `bin/disjuntor.py:1688`–`:1695`, `:1707`–`:1708`, `:2311`–`:2314`)

## 12/11 14:12 · sessao 8c26163758bc · - · - · origem=Ronald (tmux)

**Seção 2 não fecha uma partição operacional inequívoca.** A cláusula “TODO o resto” promete preservação suficiente em abstrato, mas a migração e os limites ainda deixam casos sem destinação explícita. (`contrato_item3_v4.txt:7`)

- **G1:** deve terminar **antes** da primeira linha de comentário. Essa primeira linha já é a manual `ESTRUTURAL-DEDICADO-28-18H`; “até a 1ª linha `<!--`”, se inclusivo, manda regenerar justamente a linha que a mesma seção exige preservar. (`contrato_item3_v4.txt:7`; `app/docs/BACKLOG.md:7`)
- **G2 e o fechamento de G3 são novos:** o gerador atual não emite esses marcadores; ele monta cabeçalho, contagem e famílias, injeta a janela antiga e grava tudo. Falta delimitar expressamente a primeira migração antes de poder aplicar G3/CAUDA. (`bin/gerar_backlog.py:65`, `:93`, `:100`, `:119`, `:127`, `:139`; `contrato_item3_v4.txt:7`)
- **Linhas fora das zonas nomeadas e da migração numérica:** `SELO-MORDE-DO-PASSIVO-AFIRMA-MAIS-DO-QUE-MEDE`, `MARCO-DO-PAR-DE-1490-MIN-NAO-FOI-MEDIDO` e `ORFAOS-L-111-NO-FECHAMENTO-E-NO-ESPELHO` estão em **2371–2373**, antes do início **2374** indicado na migração. Não são G1/G2/OBRAS nem CAUDA já delimitada; ficam cobertas somente pelo comando geral “TODO o resto”. Portanto não cabe afirmar perda inevitável, mas cabe afirmar migração incompleta. (`app/docs/BACKLOG.md:2371`, `:2372`, `:2373`; `contrato_item3_v4.txt:7`)
- **“O render não produz” não identifica linha manual.** A tabela gerada BATIDA ainda existe no BACKLOG, embora `PENDENTES['batida']` hoje seja vazio. Preservar todo negativo do diff promoveria conteúdo gerado obsoleto a cauda manual. (`app/docs/BACKLOG.md:2365`, `:2369`, `:2370`; `app/core/juizes.py:1266`; `bin/gerar_backlog.py:76`, `:78`; `contrato_item3_v4.txt:7`)
- **Confirmação em memória, sem executar a parte de escrita:** o render atual produz **245 itens**, enquanto o documento publica **278**. Mudam tela **117→109**, ausência **1→0**, célula **1→0**, turno **1→0**, fechamento **19→0**, feriado **13→12**, batida **2→0**; portas permanece **124**. Logo o diff contém mudanças legítimas do corpo gerado além das linhas manuais. (`bin/gerar_backlog.py:76`, `:85`, `:93`, `:99`; `app/core/juizes.py:362`, `:392`, `:435`, `:469`, `:489`, `:739`, `:1039`, `:1266`; `app/docs/BACKLOG.md:2052`, `:2054`, `:2176`, `:2305`, `:2311`, `:2317`, `:2323`, `:2347`, `:2365`)
- **O corpo gerado antigo termina em BATI-02:2370; a cauda manual começa imediatamente em :2371.** Separar aí a migração manual resolve o exemplo concreto; o contrato ainda precisa definir esse corte e tratar a regeneração do corpo obsoleto, sem inferir autoria somente pela ausência no render atual. (`app/docs/BACKLOG.md:2370`, `:2371`; `contrato_item3_v4.txt:7`)

O código atual também normaliza texto e finais de linha ao ler, recortar e remontar a janela; portanto a promessa futura de preservação **byte a byte** exige trocar esse mecanismo, além de ampliar a janela. (`bin/gerar_backlog.py:111`, `:127`, `:139`; `contrato_item3_v4.txt:7`)

## 12/11 14:12 · sessao e0e623d34103 · E3-LEITORES · sonda · origem=Code

Referências `DADO:N`: linhas do bloco fornecido, contando `sonda_leitura` como linha 1 e incluindo a linha em branco.

A sonda informa **total_erros=0** (`DADO:25`) e **sonda_coorte_atual_conclusiva=1** (`DADO:26`): permite concluir sobre a coorte atual.

| Medida | Resultado | Fonte |
|---|---:|---|
| Coorte atual de vencidos em COBRANÇA | **11** | `DADO:5` |
| É igual a 12? | **Não** (`mesma_quantidade_12=0`) | `DADO:6` |
| Chamados medidos | **11** | `DADO:7` |
| Visíveis no MAIN | **0** | `DADO:8` |
| Visíveis com b4 | **3** | `DADO:9` |
| Passam a aparecer com b4 | **3** | `DADO:10` |
| Chamados de colaboradores isentos na coorte | **8** | `DADO:11` |
| Chamados de isentos visíveis com b4 | **1** | `DADO:12` |
| Chamados de isentos que passam a aparecer | **1** | `DADO:13` |

Na cópia, o juiz `isento_ponto` consulta o booleano `colab.isento_ponto`. [app/core/constants.py:18](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/core/constants.py:18)

| Chamado | MAIN | b4 | Isento | Fonte |
|---|---:|---:|---:|---|
| col41/#3057 | 0 | 0 | 1 | `DADO:14` |
| col41/#3403 | 0 | 0 | 1 | `DADO:15` |
| col440/#11658 | 0 | 0 | 1 | `DADO:16` |
| col291/#11935 | 0 | 1 | 1 | `DADO:17` |
| col331/#16444 | 0 | 1 | 0 | `DADO:18` |
| col368/#16621 | 0 | 1 | 0 | `DADO:19` |
| col216/#23030 | 0 | 0 | 1 | `DADO:20` |
| col427/#23031 | 0 | 0 | 0 | `DADO:21` |
| col257/#23037 | 0 | 0 | 1 | `DADO:22` |
| col225/#23049 | 0 | 0 | 1 | `DADO:23` |
| col142/#27206 | 0 | 0 | 1 | `DADO:24` |

Passam a aparecer **col291/#11935, col331/#16444 e col368/#16621**; entre eles, o chamado de isento é **col291/#11935**. (`DADO:17`, `DADO:18`, `DADO:19`)

A saída **não prova identidade com os 12 históricos**: registra `identidade_com_12_historicos_verificada=0`. Não permite identificar qual chamado saiu ou explicar a diferença em relação aos 12 históricos. (`DADO:27`; [não verificado])

## 12/11 14:10 · sessao 9d59c5cda2eb · E3-LEITORES · sonda · origem=Code

Referências `DADO:linha`: numeração do bloco fornecido, começando em `sonda_leitura:` como linha 1 e contando a linha vazia seguinte.

**A sonda declara resultado conclusivo:** `total_erros = 0` e `sonda_conclusiva = 1`. Portanto, não se aplica a condição de interromper por erro ou inconclusividade. (DADO:152; DADO:153)

| Medida solicitada | Resultado | Fonte |
|---|---:|---|
| Perguntas vivas em `respondida_sem_texto` | **143** | DADO:6 |
| Disputas que mudam `pode_fechar` na comparação solicitada | **0** | DADO:150 |
| Disputas abertas que mudam `pode_fechar` | **0** | DADO:151 |

**O gatilho “Número > 0” dispara pelas 143 perguntas vivas**, não pelas disputas que mudam fechamento, cujo número é zero. O aval manda publicar a lista e parar nesse item; a saída também registra `pare_no_10 = 1`. (DADO:6; DADO:150; DADO:154; DADO:157)

A correção da chamada está de acordo com a cópia: `pode_fechar(self)` não recebe `perguntas` e consulta `respostas_sem_veredito()` e `fio_vivo(...)`. [app/chamados/models.py:1849](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/models.py:1849)

Lista completa das perguntas, agrupada por `colNNN`; cada `#pk` traz sua linha na saída:

| colNNN | #pk e fonte |
|---|---|
| col60 | #31436 (DADO:7) |
| col191 | #29167 (DADO:8), #29168 (DADO:9), #29169 (DADO:10), #31724 (DADO:11), #31725 (DADO:12), #31726 (DADO:13), #31727 (DADO:14), #31728 (DADO:15), #31729 (DADO:16), #31730 (DADO:17), #31731 (DADO:18), #32909 (DADO:19), #32910 (DADO:20), #33593 (DADO:21), #33594 (DADO:22), #34182 (DADO:23), #34183 (DADO:24) |
| col204 | #31584 (DADO:25), #31585 (DADO:26), #31586 (DADO:27), #31587 (DADO:28) |
| col224 | #32291 (DADO:29), #32292 (DADO:30), #32293 (DADO:31), #32294 (DADO:32), #32295 (DADO:33), #32296 (DADO:34), #33925 (DADO:35), #33926 (DADO:36), #33927 (DADO:37) |
| col310 | #32243 (DADO:38), #32244 (DADO:39), #32245 (DADO:40), #32246 (DADO:41), #34141 (DADO:42), #34142 (DADO:43) |
| col331 | #22394 (DADO:44) |
| col349 | #29502 (DADO:45), #29503 (DADO:46), #32131 (DADO:47), #32132 (DADO:48), #32133 (DADO:49), #32134 (DADO:50), #32135 (DADO:51), #32136 (DADO:52), #33749 (DADO:53), #33750 (DADO:54) |
| col437 | #29565 (DADO:55), #32224 (DADO:56), #32225 (DADO:57), #32226 (DADO:58), #32227 (DADO:59), #32228 (DADO:60), #32229 (DADO:61), #32230 (DADO:62), #32231 (DADO:63), #32232 (DADO:64), #32233 (DADO:65), #32234 (DADO:66), #32235 (DADO:67), #32236 (DADO:68), #32237 (DADO:69), #32238 (DADO:70) |
| col446 | #29872 (DADO:71), #32589 (DADO:72), #32590 (DADO:73), #32591 (DADO:74), #32592 (DADO:75), #32593 (DADO:76), #32594 (DADO:77), #32595 (DADO:78), #32596 (DADO:79), #32597 (DADO:80), #32598 (DADO:81), #32599 (DADO:82), #32600 (DADO:83), #32601 (DADO:84), #33146 (DADO:85), #33147 (DADO:86) |
| col592 | #28357 (DADO:87), #31708 (DADO:88), #31709 (DADO:89), #31710 (DADO:90), #31711 (DADO:91), #31712 (DADO:92), #31713 (DADO:93), #31714 (DADO:94), #31715 (DADO:95), #31716 (DADO:96), #31717 (DADO:97) |
| col607 | #29114 (DADO:98), #31334 (DADO:99), #31335 (DADO:100) |
| col782 | #32279 (DADO:101), #32280 (DADO:102), #32281 (DADO:103), #32282 (DADO:104), #32283 (DADO:105), #32284 (DADO:106), #32285 (DADO:107), #32286 (DADO:108), #32287 (DADO:109), #32288 (DADO:110), #32289 (DADO:111), #32290 (DADO:112) |
| col789 | #31839 (DADO:113), #31840 (DADO:114), #31841 (DADO:115), #31842 (DADO:116), #33549 (DADO:117), #33550 (DADO:118) |
| col869 | #28443 (DADO:119), #29270 (DADO:120), #31863 (DADO:121), #31864 (DADO:122), #31865 (DADO:123), #31866 (DADO:124), #31867 (DADO:125), #31868 (DADO:126), #31869 (DADO:127), #31870 (DADO:128), #31871 (DADO:129), #31872 (DADO:130), #31873 (DADO:131) |
| col893 | #32459 (DADO:132), #32460 (DADO:133), #32461 (DADO:134), #32462 (DADO:135), #32463 (DADO:136), #32464 (DADO:137), #32465 (DADO:138), #32466 (DADO:139), #33577 (DADO:140), #33578 (DADO:141), #34171 (DADO:142), #34172 (DADO:143) |
| col900 | #38251 (DADO:144), #38252 (DADO:145) |
| col903 | #31472 (DADO:146), #31473 (DADO:147), #31476 (DADO:148), #31477 (DADO:149) |

Escritor responsável pela gravação de `respondida_em` sem texto: **[não verificado]**.

## 12/11 14:10 · sessao b71a61ef7067 · - · - · origem=Ronald (tmux)

Leitura concluída sem criação/edição de arquivos. O censo por regra precisa conservar a distinção entre **cobrança/exposição**, **vida do fato**, **mensagem não lida**, **emissão**, **medição do universo** e **apresentação da folha**; o próprio tripwire faz essas categorias em `app/core/tests/test_tripwire_contagem_de_pendencia.py:66`.

**Resultados principais comprovados**

- `colab_em_quarentena`, `fora_da_quarentena`, `fora_do_silencio` e `silenciado_agora` não apareceram como chamadas no universo AST executado; completude quanto a chamadas dinâmicas: [nao verificado].
- O juiz da fila ainda exclui somente `silenciado_ate > agora`: `app/chamados/services/painel.py:188`. A partição ainda converte vencido em HISTORICO: `app/chamados/services/verbos.py:48`; a lista do colaborador exige `silenciado_ate IS NULL`: `app/chamados/services/janela_colab.py:70`.
- A série E já acrescentou o compartilhamento de fila e partição: `alimentar_fila` em `app/chamados/juizes.py:1521`; `alimentar_portas` o chama em `app/chamados/juizes.py:1566`; a carga da tarefa o chama em `app/chamados/services/tarefa_do_dia.py:529`.
- Corrigir apenas `qs_na_fila` **não cala as ações da tarefa**: `_pela_decisao` pode produzir `RESPONDER`/`VALIDAR` com `porta_admin=None` em `app/chamados/services/tarefa_do_dia.py:126`; `_portas_da_tarefa` filtra as portas/fila, mas preserva os donos das ações em `app/chamados/services/tarefa_do_dia.py:545`.
- A tarefa recebe o **isento atual** da grade em `app/chamados/services/tarefa_do_dia.py:499` e suprime ações de qualquer dia da carga em `app/chamados/services/tarefa_do_dia.py:188`. Sua equivalência à janela histórica desde a flag é [nao verificado]; é uma DECISAO semântica a perguntar, não resposta a inventar.
- `data_evento` distingue fato de turno e conversa de dia civil: a exposição registra explicitamente essa distinção em `app/chamados/services/pergunta_no_app.py:104`; a emissão separa `abrir_conversa` de `abrir` em `app/chamados/services/disputa_emissao.py:431`. Reaplicar cegamente `na_janela(..., p.data_evento)` do diff antigo mistura os dois universos.
- O E3 ainda tem C17 sem fachada: comentário em `app/chamados/services/tarefa_do_dia.py:27`; plano/condição em `app/docs/BACKLOG.md:1873`; registro dos cinco respondedores a retirar em `app/docs/CORTES.md:426`.

**AST: universo e script exato executado**

O script abaixo foi executado por `python3 - <<'PY'`, exclusivamente em memória. Ele reaproveita as funções AST reais do tripwire sem importar Django ou executar testes. Resultado: **759 arquivos, 58 terminais de contagem que leem status, 2 conformes, 25 funções não conformes; todas as 25 estão declaradas**. Esses números são resultado desta execução, sem arquivo persistido: [nao verificado] enquanto referência `arquivo:linha`; o algoritmo está em `app/core/tests/test_tripwire_contagem_de_pendencia.py:292`.

```python
import ast, pathlib, collections
p = pathlib.Path('app/core/tests/test_tripwire_contagem_de_pendencia.py')
tree = ast.parse(p.read_text())
tree.body = [
    n for n in tree.body
    if not isinstance(n, ast.ClassDef)
    and not (
        isinstance(n, ast.ImportFrom)
        and n.module
        and n.module.startswith('django')
    )
]
env = {'__file__': str(p.resolve())}
exec(compile(tree, str(p), 'exec'), env)
pairs = list(env['arquivos_de_producao']())
hits = [
    (r, *h)
    for r, s in pairs
    for h in env['contagens_por_status'](s)
]
print('UNIVERSO', len(pairs), 'CONTAGENS', len(hits),
      'CONFORMES', sum(h[-1] for h in hits))
print('NAO_CONFORMES')
nc = env['sitios_nao_conformes'](pairs)
for k, v in nc.items():
    print(k, ','.join(map(str, v)),
          env['DECLARADOS'].get(k, ('NOVO', ''))[0])
watch = {
    'qs_na_fila', 'qs_vivos', 'particionar_verbos',
    'perguntas_do_app', 'perguntas_pendentes_do_colab',
    'pergunta_viva', 'fio_vivo', 'gerar_perguntas_disputa',
    'criar_ou_atualizar_disputa', 'perguntar_pela_celula',
    'tarefa_do_dia', 'porta_alcancavel_do_dia',
    '_portas_da_tarefa', 'alimentar_portas', 'alimentar_fila',
    'colab_em_quarentena', 'fora_do_silencio', 'silenciado_agora'
}
print('CHAMADAS')
for r, s in pairs:
    t = ast.parse(s)
    aliases = {
        a.asname or a.name: a.name
        for n in ast.walk(t)
        if isinstance(n, ast.ImportFrom)
        for a in n.names
    }
    out = collections.defaultdict(list)
    class V(ast.NodeVisitor):
        def __init__(self):
            self.stack = []
        def visit_ClassDef(self, n):
            self.stack.append(n.name)
            self.generic_visit(n)
            self.stack.pop()
        def visit_FunctionDef(self, n):
            self.stack.append(n.name)
            self.generic_visit(n)
            self.stack.pop()
        visit_AsyncFunctionDef = visit_FunctionDef
        def visit_Call(self, n):
            f = n.func
            name = (
                aliases.get(f.id, f.id) if isinstance(f, ast.Name)
                else f.attr if isinstance(f, ast.Attribute) else ''
            )
            if name in watch:
                out['.'.join(self.stack) or '<modulo>'].append(
                    f'{name}@{n.lineno}')
            self.generic_visit(n)
    V().visit(t)
    for fn, ls in out.items():
        print(r + '::' + fn, ' '.join(ls))
```

O universo exclui pastas `tests`, `migrations`, `__pycache__`, `staticfiles`, `static`, `node_modules`, `media`, `logs`, além de nomes começando `test`: `app/core/tests/test_tripwire_contagem_de_pendencia.py:325`. A diferença **811 − 759 = 52** decorre destes nomes fora de `tests/`; entre eles estão `app/ponto/management/commands/testar_pwa.py` e `app/config/settings/teste_motor.py`, portanto o corte é sintático, não uma afirmação de que todos os 52 são testes: [nao verificado] para a contagem persistida.

Limites do método: universo por parâmetro, chamadas dinâmicas, `getattr`, filtros em dicionários e listas acumuladas por `append` não ficam certificados pelo tripwire: `app/core/tests/test_tripwire_contagem_de_pendencia.py:32`. O scanner adicional acima reconhece apelidos de `ImportFrom` e atributo final; não é análise de pontos para objetos/retornos nem resolve execução dinâmica: [nao verificado].

Executei também fecho sintático de import/reexport: 22 sementes → 424 funções, mas o fecho inclui cadastro, regeneração, signals e outros escritores upstream; **424 não é número de leitores de cobrança**. Esse resultado não deve ser apresentado como censo semântico completo: [nao verificado]. [nao verificado]

**Censo por regra dos núcleos e consumidores**

| Sítio atual | Pergunta/universo/estado | Quem escreve o estado / migração |
|---|---|---|
| `app/chamados/services/painel.py:158` `qs_na_fila` | Cobrança; vivos em COBRANCA menos silêncio futuro; lê status e timestamp | Estado chamado e porta arquivo; quarentena precisa entrar aqui segundo especificação fornecida [nao verificado]. |
| `app/chamados/services/verbos.py:30` `particionar_verbos` | Partição do universo vivo recebido; crédito das perguntas, módulo, silêncio, idade, urgência | Lê crédito em `raiox`; ainda tem HISTORICO `:51`; migra silêncio e quarentena. |
| `app/chamados/services/janela_colab.py:49` `chamados_visiveis_colab` | Lista de trabalho: janela corrente por criação OU chamado com pergunta viva, exclui módulos internos | Lê `silenciado_ate IS NULL` `:73`; migra predicado de silêncio e quarentena. |
| `app/chamados/juizes.py:1521` `alimentar_fila` | Fila, verbos e chamados por célula, lote; partição de **todos vivos**, fila do juiz | Leitor puro: migração herdada dos dois núcleos; provar que mesmo `agora` chega aos dois `:1530/:1533`. |
| `app/chamados/juizes.py:1542` `alimentar_portas` | Acrescenta perguntas C6 vivas por célula e identity map do colaborador | Leitor puro; herda fila/partição e consulta de app; não duplicar janela. |
| `app/chamados/juizes.py:1580` `_no_app` | Quais perguntas estão no app, memo por colaborador | Chama `perguntas_do_app` `:1590`; herda migração do app. |
| `app/chamados/juizes.py:1612` C17 `porta_alcancavel_do_dia` | Chamado da célula na fila OU pergunta viva no app ancorada na fila | Leitor puro; filtra `FORA_DA_LISTA_PADRAO` `:1655`; HISTORICO vigente repercute aqui. E3 fachada condicionada ao DIFF, `app/docs/BACKLOG.md:1873`. |
| `app/chamados/services/tarefa_do_dia.py:562` C19 `tarefa_do_dia` | Ações exigíveis por instância de marco/dia de turno/instante | Pura; lê grade, decisões, efeito, lavra, cobertura, tranca, fila. Precisa integração/prova própria: ações existem sem porta `:130`; quarentena não pode depender só da fila. |
| `app/chamados/services/tarefa_do_dia.py:538` `_portas_da_tarefa` | Portas das ações já julgadas; fila e verbos visíveis | Pura, usa alimentação da fila e `FORA_DA_LISTA_PADRAO` `:549`; não chama quarentena e não elimina ações. |
| `app/chamados/services/pergunta_no_app.py:45` `perguntas_do_app` | C6 vivas, prefilter `q_vivas`, identidade/tranca alimentadas | Estado pergunta/decisões/lavra; migra quarentena sem perder identidade/mapa/lote. |
| `app/chamados/services/pergunta_no_app.py:61` `consulta_do_app` | Particiona universo C6 em pendentes sem resposta + disputa aberta, demais vivas no histórico | Pura; migração herdada de `perguntas_do_app`; não repetir filtro antigo em `aviso_pendencia`. |
| `app/chamados/services/pergunta_no_app.py:78` `projecao_do_app` | Badge de perguntas pendentes e, sem estas, mensagens DP não lidas em chamados vivos | Perguntas via consulta; Followup escrito pela porta nota. Mensagem não lida é outra pergunta, sem migração automática para cobrança. |
| `app/chamados/services/pergunta_no_app.py:121` `retencao` | Por que chamado de furo vivo não tem pergunta no app | Lê dia pelo catálogo, hoje, app, ata, parcial/ausência, fato não conversa; migra causa de quarentena e ordem das guardas com RED. |
| `app/chamados/services/pergunta_no_app.py:178` `medir` | COBRANCA, módulos `CALADOS_POR_ISENCAO`, colaborador ativo | Medida; migra leitura em lote e causa da retenção. |
| `app/chamados/services/pergunta_no_app.py:211` `do_dia` | VIVOS do colaborador/modulos furo/dia, perguntas no app, retenção | Medida/exposição do copiloto; herda app/retencao; não trocar seu universo vivo por fila sem corte. |
| `app/chamados/services/aviso_pendencia.py:7` | Perguntas pendentes que continuam cobrando após texto livre | Hoje só delega `consulta_do_app` `:9`; migração herdada. |
| `app/chamados/services/fio.py:163` `montar_chamados_fio` | Colab `so_janela=True` usa janela; admin tudo; perguntas de disputas abertas + C6 para fechadas | Sequência E1 carregada `:231`; migração da lista não basta para perguntas anexadas/admin: separar exposição de cobrança. |
| `app/chamados/services/fabrica_da_celula.py:92` `motivos_a_perguntar` | Motivos da célula ainda devidos; dia nunca bateu/sem adesão, trabalho, situação, teto temporal, lampada, parcial, dedup do fato | Autoridade da emissão atual; migra quarentena ou prova herdada, incluindo contador que lê este juiz. |
| `app/chamados/services/fabrica_da_celula.py:160` `perguntar` | Desfaz carimbo, abre/agrega disputa, gera perguntas por motivo/dia | Escritor chama disputa `:172` e geração `:174`; provar lote/rollback/disputa vazia. |
| `app/chamados/services/fabrica_da_celula.py:178` `perguntar_pela_celula` | Emissão no julgamento da célula; motivos e limbo exportado para pauta | Lê juiz `motivos_a_perguntar` `:207`; push silenciado `:218`; preservar essas saídas. |
| `app/chamados/services/disputa_emissao.py:300` `gerar_perguntas_disputa` | Motivos datados/episódios/conversas, dedup, derivação de destino/dia do turno | Escritor usa `abrir_conversa` `:439`, `abrir` `:444`; gate precisa distinguir conversa/fato. |
| `app/chamados/services/disputa_emissao.py:634` `criar_ou_atualizar_disputa` | Disputa nova ou agregação e perguntas/push | Escritor; gate+rollback de rodada vazia e janela lida uma vez precisam ser reaplicados aqui, não corpo antigo. |
| `app/chamados/services/disputa_emissao.py:823` `_criar_pergunta_se_couber` | Pergunta factual datada, após cobertura e dedup | Escritor `PerguntaDisputa.abrir` `:837`; precisa gate de quarentena. |
| `app/chamados/services/disputa_emissao.py:1027` órfãos | Repasse de órfãos agora tem fecho e trava próprios `:945/:993` | Filtrar isolado sem apagar/fechar; preservar domínio da trava e recusas. |
| `app/chamados/services/disputa_emissao.py:1302` âncora | Cria conversa de confirmação sob chamado limbo | `abrir_conversa` `:1371`, push `:1381`; universo deve ser confrontado com item sem dia, não presumido factual. |
| `app/chamados/services/disputa_emissao.py:1757` `_enviar_push_disputa_colab` | Disparo de push da disputa; respeita contexto silenciado | Escritor externo; provar ausência de disparo quando rodada isolada. |
| `app/chamados/management/commands/disparar_perguntas_competencia.py:75` `contador_fio_mudo` | VIVOS ligados à célula na janela; retira pergunta viva; dedup de motivos devidos | **Vigia**, chama `motivos_a_perguntar` `:115`; migra pela mesma autoridade, sem regra paralela de quarentena. |
| `app/chamados/services/vigia_fio.py:200` `censo` | Universo de células/lâmpadas; mede fio e donos | Medida do universo; novo balde de quarentena precisa entrar na soma e não alarmar, conforme anexo [nao verificado]. |
| `app/chamados/services/vigia_fio.py:542` `censo_alcance` | Dias que retêm folha, porta e dono | Lê `alimentar_portas` `:604`, C17 `:635`; precisa classificar quarentena como não cobrança sem inventar balde de lei. |
| `app/chamados/services/na_fila.py:10` | Contadores de inteligência por pessoa/empresa a partir de verbos acionáveis | Lê partição `:19`; migração herdada de verbos, efeitos nos consumidores abaixo. |
| `app/chamados/services/cobranca.py:45` `_pendentes` | Perguntas pendentes para cobrança | Delega consulta do app `:48`; `checar_cobranca` `:75` e `cobrar_colaborador` `:140` herdam migração. |
| `app/chamados/services/fil a_por_causa.py:156` `particionar` | Quadro da fila por causa | Chama juiz `:160`; migração herdada; caminho sem espaço real: `app/chamados/services/fila_por_causa.py:160`. |
| `app/chamados/services/sla.py:53` `vencidos_na_fila` | SLA vencido entre cobrados pela fila | Chama juiz `:62`; migração herdada. |

**Inventário integral das chamadas diretas aos alvos do script**

Cada entrada abaixo é `arquivo :: função → alvo@linha`. Os números são os do HEAD, e não os do anexo.

```text
app/api/views.py
  api_bater_ponto → criar_ou_atualizar_disputa@764
  api_disputa_responder → pergunta_viva@2608
app/api/views_core.py
  api_bater_ponto → criar_ou_atualizar_disputa@758
app/api/views_mensageria.py
  api_mensageria_pendencias → qs_vivos@393 particionar_verbos@395
  api_mensageria_sinais → qs_vivos@452 particionar_verbos@453
  api_mensageria_pessoa → qs_vivos@1038
  api_mensageria_decidir → qs_vivos@1148 particionar_verbos@1149

app/chamados/juizes.py
  pergunta_sem_lastro_ou_viva → pergunta_viva@1486
  perguntas_vivas_de → pergunta_viva@1491
  fio_vivo → pergunta_viva@1503
  alimentar_fila → particionar_verbos@1530 qs_vivos@1530 qs_na_fila@1533
  alimentar_portas → alimentar_fila@1566 pergunta_viva@1573
  _no_app → perguntas_do_app@1590
  porta_alcancavel_do_dia → alimentar_portas@1646
  perguntas_respondidas_a_validar → pergunta_viva@1674
  _dias_das_perguntas_vivas → pergunta_viva@2336
  disputa_viva_do_chamado → fio_vivo@2389

app/chamados/management/commands/alertar_chamados_sla.py
  Command.handle → qs_na_fila@30
app/chamados/management/commands/arquivar_competencia_encerrada.py
  Command.handle → qs_vivos@100
app/chamados/management/commands/arquivar_prescritos.py
  Command.handle → particionar_verbos@50
app/chamados/management/commands/arquivar_sem_dia_irrecuperavel.py
  Command.handle → qs_vivos@58
app/chamados/management/commands/auditar_invariantes_chamados.py
  _contraprova_f1t → pergunta_viva@202
  _i6_pares → pergunta_viva@540
  Command.handle → pergunta_viva@858 @894 @1653 @1710 @1769 @1782
app/chamados/management/commands/backfill_fio_morto_e4b.py
  ato_do_dia → tarefa_do_dia@87
  _executar_dia → perguntar_pela_celula@143
app/chamados/management/commands/diff_e3_sombra.py
  Command.comparar → alimentar_portas@114 tarefa_do_dia@125
app/chamados/management/commands/disparar_perguntas_competencia.py
  contador_fio_mudo → pergunta_viva@109
app/chamados/management/commands/encerrar_perguntas_com_fato.py
  Command.handle → fio_vivo@83
app/chamados/management/commands/migrar_data_evento_dia_do_turno.py
  Command._descarimbados → pergunta_viva@449
app/chamados/management/commands/nomear_marco_por_lampada.py
  Command.handle → qs_na_fila@47
app/chamados/management/commands/reabrir_orfas_enterradas.py
  Command.handle → pergunta_viva@83
app/chamados/management/commands/reatar_beco_folga_ao_limbo.py
  Command._rodar → qs_na_fila@59 @105
app/chamados/management/commands/smoke_chamados.py
  Command._bloco_a_motivos → gerar_perguntas_disputa@103

app/chamados/models.py
  ChamadoColaborador._vivas_entre → pergunta_viva@368
  ChamadoColaborador._universo_da_ressuscitacao → pergunta_viva@1510
  DisputaSupervisao.pode_fechar → fio_vivo@1854
  DisputaSupervisao.fechar → pergunta_viva@1897
  PerguntaDisputa.estado → pergunta_viva@3045
  PerguntaDisputa.carimbar → pergunta_viva@3140
  PerguntaDisputa.viva → pergunta_viva@3376
app/chamados/reconciliador.py
  lavrar_sem_turno_aberto → pergunta_viva@399
  perguntas_absolvidas_pela_celula → pergunta_viva@662
  _viva_em_competencia_aberta → pergunta_viva@759
  veredito_orfa → pergunta_viva@1087
  reatar_pergunta_orfa → pergunta_viva@1595
app/chamados/relatorios_disputa.py
  classificar → pergunta_viva@18

app/chamados/services/acoes_chamado.py
  montar_contexto_detalhe → pergunta_viva@355
app/chamados/services/aviso_pendencia.py
  aviso_texto_livre → perguntas_pendentes_do_colab@13
app/chamados/services/competencia_trancada.py
  medir → pergunta_viva@123 @133 @143
app/chamados/services/disputa_emissao.py
  perguntar_entradas_pendentes → gerar_perguntas_disputa@294
  criar_ou_atualizar_disputa → gerar_perguntas_disputa@670 @682
  _ja_perguntou → fio_vivo@1422
  reabrir_credor_de_pergunta_viva → pergunta_viva@1821
  _resolver_perguntas_por_chamado_resolvido → pergunta_viva@1966 @2048
app/chamados/services/explicador.py
  por_que_nao_fecha → fio_vivo@137
app/chamados/services/fabrica_da_celula.py
  perguntar → criar_ou_atualizar_disputa@172 gerar_perguntas_disputa@174
  perguntar_por_eventos → perguntar_pela_celula@301
app/chamados/services/fila.py
  por_verbo → particionar_verbos@55 qs_vivos@55
  idade_e_vazao → qs_na_fila@92 qs_vivos@117
  gestao → qs_na_fila@150
app/chamados/services/fila_por_causa.py
  particionar → qs_na_fila@160
app/chamados/services/fio.py
  montar_chamados_fio → pergunta_viva@238
  processar_post_fio → pergunta_viva@606
  _costura_nao_mata_o_fio → pergunta_viva@782
  _colisao_ao_datar → pergunta_viva@961
  _migrar_sob_trava → pergunta_viva@2129 @2134
app/chamados/services/limbo_folgas.py
  filhos_no_defeito → fio_vivo@203
app/chamados/services/na_fila.py
  na_fila → particionar_verbos@19
app/chamados/services/painel.py
  qs_na_fila → qs_vivos@188
  montar_contexto_painel → qs_na_fila@212 particionar_verbos@230 qs_vivos@230
app/chamados/services/pergunta_no_app.py
  perguntas_do_app → pergunta_viva@58
  consulta_do_app → perguntas_do_app@70
  _marco_ja_perguntado → pergunta_viva@170
  medir → perguntas_do_app@198
  do_dia → perguntas_do_app@221
app/chamados/services/relavra_via.py
  desfazer_carimbo_sem_lastro → pergunta_viva@242
app/chamados/services/sla.py
  vencidos_na_fila → qs_na_fila@62
app/chamados/services/tarefa_do_dia.py
  _carga_da_tarefa_cls.CargaDaTarefa.__init__ → qs_na_fila@415
  _carga_da_tarefa_cls.CargaDaTarefa._ler_vida → pergunta_viva@491
  _carga_da_tarefa_cls.CargaDaTarefa.tarefa → alimentar_fila@529 _portas_da_tarefa@532
app/chamados/services/verbos.py
  coletar_lente → particionar_verbos@195
app/chamados/services/vigia_fio.py
  fio_do_dia → fio_vivo@166
  censo_alcance → alimentar_portas@604 port

[truncado em 20000 de 30032 caracteres]

## 12/11 14:07 · sessao 58ed5a21b819 · E3-LEITORES · censo · origem=Code

A cópia ainda combina flag atual de isenção, silêncio por chamado e leitores com critérios diferentes. A quarentena por período está registrada como obra reprovada, com sete curas pendentes; o contrato vigente preserva fatos, cálculo, espelho, folha e TXT. `app/core/constants.py:13`, `app/chamados/services/silencio_isentos.py:20`, `app/chamados/services/verbos.py:48`, `app/docs/BACKLOG.md:765`, `app/docs/CORTES.md:348`.

Resultados atuais de produção, autoria histórica dos carimbos e comportamento efetivamente executado no MAIN: **[nao verificado]**.

**A1. Quem decide hoje**

| Caminho | Pergunta e universo | Quem escreve o estado lido |
|---|---|---|
| `isento_ponto(colab)` | “A flag está ligada agora?” Não consulta período nem cargo. `app/core/constants.py:13` | Passe Livre chama `gravar` e registra antes/depois; a flag é booleana. Houve também semente por migration. `app/colaboradores/views.py:173`, `app/colaboradores/views.py:183`, `app/colaboradores/models.py:294`, `app/colaboradores/migrations/0043_isento_ponto.py:13` |
| `silencio_isentos.alvos()` | Chamados em `VIVOS`, de flag ligada, com `silenciado_ate` nulo. Não recorta dia, período ou módulo. `app/chamados/services/silencio_isentos.py:20` | O serviço chama `arquivo.silenciar`, que grava prazo, autor, motivo e trilha. `app/chamados/services/silencio_isentos.py:34`, `app/chamados/services/arquivo.py:141` |
| Evento da flag | Todo save de colaborador com flag verdadeira agenda a vassoura; não exige transição falsa→verdadeira. `app/chamados/signals.py:270` | O escritor da flag dispara o signal; a vassoura escreve o véu após commit. `app/colaboradores/views.py:176`, `app/chamados/signals.py:276` |
| Evento do chamado | Nascimento, retorno a estado vivo ou levantamento do véu podem chamar novamente a vassoura. Isso permite re-silenciar um resgate de isento. `app/chamados/signals.py:285`, `app/chamados/signals.py:289` | Estados passam por `_gravar_estado`; véu por `arquivo.silenciar/levantar`. `app/chamados/models.py:201`, `app/chamados/services/arquivo.py:141`, `app/chamados/services/arquivo.py:177` |
| Cron de isentos | `--alarme` conta os mesmos `alvos`; `--apply` ainda escreve silêncios. Seu universo atual não tem janela de quarentena. `app/chamados/management/commands/silenciar_chamados_isentos.py:25`, `app/chamados/management/commands/silenciar_chamados_isentos.py:36`, `app/chamados/management/commands/silenciar_chamados_isentos.py:49` | Mesma porta do véu. `app/chamados/services/silencio_isentos.py:43` |
| `silenciado_em` / `q_silenciado_em` | “O silêncio deste chamado está vigente neste instante?” Compara prazo futuro. Não decide isenção ou quarentena. `app/chamados/models.py:508`, `app/chamados/models.py:515` | `arquivo.silenciar` põe; `arquivo.levantar` limpa. `app/chamados/services/arquivo.py:166`, `app/chamados/services/arquivo.py:202` |
| `qs_na_fila` | Vivos em `COBRANCA`, excluindo silêncio futuro. Não pergunta flag nem período. `app/chamados/services/painel.py:188` | Lê estado do chamado e véu escritos pelas portas acima. `app/chamados/models.py:201`, `app/chamados/services/arquivo.py:171` |
| `chamados_visiveis_colab` | Janela corrente da empresa, ampliada pelos destinos de perguntas em `q_vivas`; exclui silêncio futuro e módulos internos. Não restringe a consulta a chamados vivos. `app/chamados/services/janela_colab.py:40`, `app/chamados/services/janela_colab.py:65`, `app/chamados/services/janela_colab.py:74` | Lê chamado, véu e perguntas; os escritores de resposta estão em B1. A lista interna inclui `desligamento_revisao`. `app/chamados/services/janela_colab.py:30`, `app/chamados/services/disputa_emissao.py:1467` |
| Card/contagens do painel | Abertos e em análise usam `qs_na_fila`. `app/chamados/services/painel.py:212` | Leitura das mesmas portas de estado/véu. `app/chamados/services/painel.py:188` |
| Pílula/lista padrão | Particiona `qs_vivos`; silêncio vencido ainda vira `HISTORICO`, depois excluído da lista padrão. Diverge do retorno exigido pelo corte. `app/chamados/services/painel.py:230`, `app/chamados/services/verbos.py:48`, `app/chamados/services/painel.py:325`, `app/docs/CORTES.md:335` | `particionar_verbos` deriva o verbo dos chamados e créditos de perguntas; não persiste esse mapa. `app/chamados/services/verbos.py:30` |
| Badge do admin | Prefere valor lavrado; depois `exige_o_admin(particionar_verbos(qs_vivos()))`; só no último caminho usa a fila. Migrar apenas `qs_na_fila` deixa esses caminhos descobertos. `app/core/context_processors.py:80`, `app/core/context_processors.py:88` | O command lavra o placar a partir da partição. `app/core/management/commands/lavrar_badge_navbar.py:39`, `app/core/management/commands/lavrar_badge_navbar.py:72`, `app/core/management/commands/lavrar_badge_navbar.py:91` |
| Alertas | SLA lê a fila. Alerta avançado descarta flag atual e também silêncio vigente do chamado; nenhum desses dois testes responde por período. `app/chamados/management/commands/alertar_chamados_sla.py:24`, `app/ponto/management/commands/processar_alertas_avancados.py:29`, `app/ponto/management/commands/processar_alertas_avancados.py:110` | Lê flag e véu; seus escritores são Passe Livre e `arquivo`. `app/colaboradores/views.py:176`, `app/chamados/services/arquivo.py:171` |
| Disparo pela célula | A fábrica considera situação, dia, ata/veredito e demais guardas; depois chama o gerador. Não consulta o juiz de quarentena. `app/chamados/services/fabrica_da_celula.py:105`, `app/chamados/services/fabrica_da_celula.py:160`, `app/chamados/services/fabrica_da_celula.py:217` | A abertura de perguntas passa pelas portas de `PerguntaDisputa`. `app/chamados/services/disputa_emissao.py:439`, `app/chamados/models.py:2792` |
| Disparo por disputa/agrupador | `gerar_perguntas_disputa`, `_criar_pergunta_se_couber` e criação de âncora abrem perguntas/conversas. `app/chamados/services/disputa_emissao.py:300`, `app/chamados/services/disputa_emissao.py:823`, `app/chamados/services/disputa_emissao.py:1371` | Persistência de nascimento em `_nascer`. `app/chamados/models.py:3030` |
| `detectar_par_relampago` | Na cópia, o trecho indicado como `:335` está em `:336`: seleciona escala ativa e colaborador ativo; abre **chamado de fato**, sem consultar isenção. Isso não prova disparo direto de `PerguntaDisputa`. `app/ponto/management/commands/detectar_par_relampago.py:336`, `app/ponto/management/commands/detectar_par_relampago.py:368` | Usa `ChamadoColaborador.abrir`. A lei vigente mantém a emissão de fatos e isola a cobrança; a antiga regra “não nasce” foi substituída. `app/chamados/models.py:846`, `app/docs/BACKLOG.md:766`, `app/docs/CORTES.md:348` |

**A2. Matriz da porta**

A matriz abaixo é o contrato de pré-desenho, não uma API implementada nesta cópia. Entidade, assinatura e retornos concretos da futura porta: **[nao verificado]**. O juiz autorizado é somente `colab_em_quarentena`; as operações precisam preservar a distinção entre fechar um período, levantar a quarentena e desarquivar um dia. `app/docs/CORTES.md:348`, `app/docs/CORTES.md:349`, `app/docs/CORTES.md:352`.

| Estado → resposta do juiz | Operação | Persistência exigida / retorno esperado | Prova |
|---|---|---|---|
| Sem janela aplicável → falso; dia coberto → verdadeiro | **Abrir** período | Colab, início, fim opcional, motivo, quem/quando; uma porta com trilha. Sem alterar chamados ou cálculo. Retorno concreto da porta **[nao verificado]**. | `app/docs/CORTES.md:348` |
| Flag deixa de valer; passado continua coberto | **Fechar**, não levantar | Registrar fim e preservar o período passado. Não devolver retroativamente toda a cobrança. **Cura 1.** | `app/docs/CORTES.md:352`, `app/docs/BACKLOG.md:765` |
| Janela levantada deixa de isolar seus itens | **Levantar** | Quem/quando/motivo e trilha; itens voltam por leitura, sem reabrir ou reescrever chamados em lote. | `app/docs/CORTES.md:348` |
| Dia coberto passa a não estar isolado; demais dias continuam | **Desarquivar `(colab,dia)`** | Toque administrativo com trilha para aquele par. Não confundir com levantar o véu de todos os chamados. | `app/docs/CORTES.md:349`; porta atual por chamado: `app/chamados/services/arquivo.py:177` |
| Flag ligada → janela de isento | **Porta separada para janela de isento** | Troca da flag abre/fecha pela porta e registra trilha. Não fazer a edição manual do período substituir silenciosamente a flag. Assinatura concreta **[nao verificado]**. **Cura 3.** | `app/docs/BACKLOG.md:765`, `app/docs/CORTES.md:352` |
| Histórico existente → períodos iniciais | **Migration reversível e congelada** | Interpretação congelada; reversão precisa distinguir o que a migration criou. Migration/função concreta **[nao verificado]**. Sem trilha da flag, início na admissão com rótulo explícito. **Cura 4.** | `app/docs/BACKLOG.md:765`, `app/docs/CORTES.md:352` |
| Cobrança escapando em dia coberto | **Vigia dentro da janela** | Contar apenas vazamentos no período; esperado zero. Não acusar todo passado pela flag atual, nem re-silenciar. **Cura 2.** | `app/docs/BACKLOG.md:765`; universo atual amplo: `app/chamados/services/silencio_isentos.py:24` |
| Caso retido atravessa a nova leitura | **RED da retenção** | Provar o ramo exigido antes da cura. Qual função de “retenção” a reprovação nomeou: **[nao verificado]**; não assumir que seja `pergunta_no_app.retencao` só pelo nome. **Cura 5.** | `app/docs/BACKLOG.md:765`; função homônima: `app/chamados/services/pergunta_no_app.py:121` |
| Projeção da migration → antes/depois | **DIFF pela função da migration** | Usar sua seleção/projeção real em leitura; medir itens, colabs e perguntas cujo disparo muda. Não criar seleção paralela. Função disponível **[nao verificado]**. **Cura 6.** | `app/docs/BACKLOG.md:765`, `app/docs/CORTES.md:348` |
| “Última ligação” | Conferência exigida antes do desenho | O material registra a expressão, mas não demonstra algoritmo ou implementação correspondente. Seu significado técnico preciso permanece **[nao verificado]**; não o substituir pela primeira trilha encontrada. **Cura 7.** | `app/docs/BACKLOG.md:765` |
| Item sem dia | Mesmo juiz | Janela aberta cobre; janela fechada não cobre esse item. | `app/docs/CORTES.md:352` |
| `desligamento_revisao` | Fora da quarentena | Ato do DP permanece fora deste isolamento. | `app/docs/CORTES.md:348` |

A tela deve acrescentar modo por período, lista de vigentes, contagem de itens e levantamento. A obra também precisa declarar o fechamento, ou a sobra motivada, dos três itens O44 citados; fechamento efetivo desses itens **[nao verificado]**. `app/docs/CORTES.md:348`, `app/docs/CORTES.md:349`.

**A3. Leitores a migrar e arquivos envolvidos**

| Grupo | Arquivos/sítios |
|---|---|
| Fila, pílula e lista | `app/chamados/services/painel.py:188`; `app/chamados/services/verbos.py:48`; `app/chamados/services/janela_colab.py:74`. |
| Badge e lavra | Conferir todos os caminhos, inclusive lavrado e partição direta: `app/core/context_processors.py:80`, `app/core/context_processors.py:138`, `app/core/management/commands/lavrar_badge_navbar.py:39`, `app/core/management/commands/lavrar_badge_navbar.py:91`. |
| Alertas | `app/chamados/management/commands/alertar_chamados_sla.py:24`; `app/ponto/management/commands/processar_alertas_avancados.py:29`, `app/ponto/management/commands/processar_alertas_avancados.py:110`. |
| Disparo e universo dos vigias | `app/chamados/services/fabrica_da_celula.py:92`, `app/chamados/services/fabrica_da_celula.py:160`; `app/chamados/services/disputa_emissao.py:300`, `app/chamados/services/disputa_emissao.py:823`, `app/chamados/services/disputa_emissao.py:1302`; `app/chamados/management/commands/disparar_perguntas_competencia.py:99`. |
| Flag/vassoura | `app/colaboradores/views.py:176`; `app/chamados/signals.py:270`, `app/chamados/signals.py:285`; `app/chamados/services/silencio_isentos.py:20`; `app/chamados/management/commands/silenciar_chamados_isentos.py:31`. |
| Entidade, juiz, porta, migration e tela | Entidade/juiz/porta/migration precisam nascer pelo contrato; nomes concretos novos **[nao verificado]**. A tela existente está em `app/chamados/views_arquivar.py:24`. Exigência: `app/docs/CORTES.md:348`. |

Os crons chamam o agrupador e o cartório chama a fábrica; cobrir a origem compartilhada evita filtros divergentes por command. `app/ponto/management/commands/detectar_ausencias.py:803`, `app/ponto/management/commands/processar_alertas_turno.py:385`, `app/ponto/management/commands/processar_alertas_avancados.py:159`, `app/ponto/services/cartorio.py:1045`.

Não cabe bloquear indiscriminadamente `PerguntaDisputa.abrir`: ela também serve a declarações e conversas voluntárias; O44 manda não trancar. O censo precisa separar esses atos do disparo automático de cobrança. `app/chamados/services/acoes_disputa.py:263`, `app/chamados/services/declaracao_texto.py:376`, `app/chamados/services/declaracao_texto.py:666`, `app/chamados/models.py:2792`, `app/docs/CORTES.md:349`.

A interseção explicitamente medida no aval é:

| Arquivo | E3, sítios informados | SAÍDA-NOTURNA, sítios informados |
|---|---|---|
| `chamados/juizes.py` | `alimentar_portas`, aproximadamente `:1570`; na cópia, função em `app/chamados/juizes.py:1542`, leitura temporal em `:1575`. | `app/chamados/juizes.py:679`, `app/chamados/juizes.py:941`. |
| `chamados/models.py` | `q_silenciado_em` e `respostas_sem_veredito`; na cópia em `app/chamados/models.py:515`, `app/chamados/models.py:1839`. | `app/chamados/models.py:2511`, `app/chamados/models.py:3179`, `app/chamados/models.py:3738`. |

A atribuição ao diff vem da transcrição fornecida de `aval_saida_noturna_na_frente.txt:3`; comparação efetiva `0a747193e..590f08946` e conteúdo do diff da SAÍDA-NOTURNA **[nao verificado]**. Pertinência dos demais arquivos ao diff completo da E3 **[nao verificado]**.

**B1. Escritores de `PerguntaDisputa.respondida_em`**

O inventário distingue escrever sem **regravar** texto de produzir o estado com texto **vazio**; são situações diferentes nos ramos abaixo. `app/chamados/models.py:3146`, `app/chamados/services/disputa_emissao.py:1652`, `app/chamados/services/disputa_emissao.py:1670`.

| Escritor/caminho persistente | Pode gravar timestamp não nulo com texto vazio? | Ramo e prova |
|---|---|---|
| `responder_pergunta_disputa_core` | Não pelo fluxo normal. | Normaliza e recusa vazio; atribui texto/carimbo juntos, salva e registra resposta. `app/chamados/services/disputa_emissao.py:1432`, `:1467`, `:1508`, `:1511`. |
| `responder_automaticamente` | **Sim, a função aceita texto novo vazio.** | A guarda verifica apenas texto anterior. Depois grava `texto`, `instante` e `ato_texto=texto`, sem validar o novo texto. `app/chamados/services/disputa_emissao.py:1647`, `:1652`, `:1655`, `:1656`. O chamador localizado passa constante preenchida: `app/api/foto_recebida.py:25`, `:48`. Ocorrência efetiva em produção **[nao verificado]**. |
| `carimbar(..., resposta=...)` | Grava o timestamp sem regravar texto, mas exige texto preexistente não vazio. | Só acrescenta `respondida_em` se houver resposta solicitada, texto e carimbo nulo; a gravação ainda depende da aceitação do C6. Não cria `ato_texto` nesse ramo. `app/chamados/models.py:3146`, `:3148`, `:3155`, `:3168`. |
| `carimbar_resposta_pela_trilha` | Grava só o timestamp no modelo, mas exige texto preexistente e o copia ao ato. | Guarda em `app/chamados/services/disputa_emissao.py:1670`; atribuição/save/ato em `:1674`, `:1676`, `:1678`. |
| `reabrir_para_correcao` | Nos usos encontrados, grava `None`, não timestamp novo. | Zera carimbo e texto por `descarimbar_em_lote` ou `update(**campos)`. `app/chamados/services/disputa_emissao.py:1593`, `:1630`, `:1632`. |
| `ressuscitar_perguntas` | Nos usos encontrados, grava `None`. | Universo de texto vazio; passagem explícita de `respondida_em=None`. `app/chamados/models.py:1499`, `:1579`. |
| `descarimbar_em_lote(..., **outros)` | **A porta genérica aceita** timestamp arbitrário sem texto. | A guarda protege os campos de materialização, não o par de resposta; `**outros` chega ao `update`. Chamadores encontrados passam `None`. `app/chamados/models.py:3271`, `:3280`, `:1579`; `app/chamados/services/disputa_emissao.py:1593`, `:1630`. Uso atual não nulo **[nao verificado]**. |
| Nascimento por `abrir`/`abrir_conversa` → `_nascer` | **A passagem genérica de campos aceita** carimbo sem texto. | `**campos` chega ao construtor e ao insert; o `save` não valida esse par. `app/chamados/models.py:2792`, `:2810`, `:2822`, `:3030`, `:3033`, `:3429`. Chamador atual fornecendo carimbo não nulo sem texto **[nao verificado]**. |

As duas atribuições adicionais são somente memória: `_universo_da_ressuscitacao` trabalha sobre cópia; `_desfazer_em_memoria` restaura o valor anterior. `app/chamados/models.py:1507`, `app/chamados/models.py:1521`, `app/chamados/models.py:3175`.

O `bulk_update` da lavra grava seus três campos, não `respondida_em`; a migration inicial declara o campo, sem backfill de resposta nesse sítio. `app/chamados/veredito_lavrado.py:237`, `app/chamados/veredito_lavrado.py:250`, `app/chamados/migrations/0014_perguntadisputa.py:25`.

A varredura AST incluiu atribuições, keywords/dicionários, `save`, `update`, `bulk_update`, `F` e `setattr`, seguida da leitura dos caminhos genéricos acima. Não apareceram outros escritores runtime explícitos de `respondida_em`; a ausência não tem relatório persistido nesta execução **[nao verificado por arquivo:linha]**. Metaprogramação ou argumentos dinâmicos não ficam certificados como impossíveis; o próprio censo estrutural declara o balde não atribuível. `app/core/censo_escritas.py:43`.

O contrato antigo não certifica timestamp→texto: declara carimbo sem texto “inofensivo”, exige somente a direção texto→carimbo e absolve o mutante “só carimbo”. Portanto não serve de prova de saúde para o #10. `app/chamados/tests/test_contract_par_resposta.py:28`, `app/chamados/tests/test_contract_par_resposta.py:69`, `app/chamados/tests/test_contract_par_resposta.py:109`.

**B2. Leitores**

`respostas_sem_veredito` hoje exige ausência de validação/materialização e aceita **carimbo OU texto**, salvo a tranca; `pode_fechar` combina essa lista com `fio_vivo`. `app/chamados/models.py:1857`, `app/chamados/models.py:1863`.

| Leitores diretos de `respostas_sem_veredito` | Uso |
|---|---|
| `app/chamados/models.py:1869`, `app/chamados/models.py:1902` | Juiz de fechamento e guarda de todas as portas de `fechar`. |
| `app/chamados/reconciliador.py:1164` | Segura fechamento de órfãs. |
| `app/chamados/services/fila_por_causa.py:226` | Gaveta respondido a decidir. |
| `app/chamados/services/competencia_trancada.py:114` | Pauta DP, com `ignorar_tranca=True`. |
| `app/colaboradores/services/ficha.py:169` | Contagem da ficha. |
| `app/colaboradores/services/desligamento.py:141` | Guarda na cascata de desligamento. |
| `app/chamados/management/commands/validar_classe_a.py:54` | Universo de classificação/validação. |

Leitores diretos de `pode_fechar`:

- Reconciliador: `app/chamados/reconciliador.py:412`, `:612`, `:1171`.
- Becos: `app/chamados/services/becos.py:33`, `:56`.
- Competência trancada: `app/chamados/services/competencia_trancada.py:154`, `:203`.
- Explicador: `app/chamados/services/explicador.py:139`.
- Emissão: `app/chamados/services/disputa_emissao.py:1278`, `:2091`.
- Materialização: `app/chamados/services/materializacao.py:591`, `:1322`.
- Entrada sincronizada: `app/ponto/services/entrada_sincronizada.py:59`.

A saída do explicador também chega ao desenlace e ao template do fio. `app/chamados/services/desenlace.py:63`, `app/templates/chamados/partials/modal_fio.html:435`, `app/templates/chamados/partials/modal_fio.html:446`.

**C1. Sonda #10**

O lado novo filtra a lista retornada pelo método real usando `respondida_sem_texto`; assim preserva suas condições de validação, materialização e tranca. Os dois lados chamam o `pode_fechar` real. A contagem de vivas usa `pergunta_viva`, porque `q_vivas` declara ser apenas um superconjunto. `app/chamados/models.py:1857`, `app/cha

[truncado em 20000 de 33869 caracteres]

## 12/11 14:07 · sessao 012ce15ae1b2 · - · - · origem=Ronald (tmux)

Relatório do explorer, somente leitura.

- Varredura AST em memória: 811 arquivos `app/**/*.py`, excluídos componentes `tests` e `migrations`; 811 parses, zero erro. Esse resultado é instrumental, sem arquivo de saída; [nao verificado] por referência persistida.
- Não encontrei entidades/serviço `QuarentenaColab`, `QuarentenaDiaDevolvido`, `colab_em_quarentena`, `colaboradores/services/isencao.py` na produção desta cópia. Ausência não tem linha positiva: [nao verificado].
- `silenciado_ate` é `DateTimeField`; comentário do modelo promete volta após vencimento (`app/chamados/models.py:87`, `:90`); `TIME_ZONE='America/Sao_Paulo'`, `USE_TZ=True` (`app/config/settings/base.py:93`, `:95`). Contrato local manda UTC no banco/apresentação UTC−3 (`CLAUDE.md:155` — conferir numeração ao incorporar).

Semântica e escritores:

| Estado | Escritor hoje | Leitura/semântica |
|---|---|---|
| `silenciado_ate`, autor, motivo | `arquivo.silenciar`, atribui em `app/chamados/services/arquivo.py:166`, salva `:171`; exige autor `:156` | Prazo padrão = instante + dias de catálogo (`:107`, `:118`); instante final exclusivo nos leitores `>agora`. |
| Limpar silêncio | `arquivo.levantar`, limpa em `:202`, salva `:210` | Resgate conserva primeiro carimbo `resgatado_em` (`:205`). |
| Trilha do silêncio | `_trilha`, `arquivo.py:130`, chama `core.observ.evento` `:137` | Fotografia dos quatro campos em `:121`; depois inclui chamado/colab `:133`. |
| Flag `isento_ponto` | Único sítio explícito de alteração achado: `colaboradores/views.py:176` → `gravar`; domínio `cadastro` ainda permite flag (`colaboradores/services/colaborador.py:34`); `gravar` usa `setattr/save` `:59` | Flag booleana **atual**, sem data; juiz `app/core/constants.py:13`, `:18`; definição `app/colaboradores/models.py:294`. |
| Trilha da flag | `passe_livre`, `app/colaboradores/views.py:183` | Texto prefixado e JSON antes/depois `:184`, `:185`, `:186`; falha não desfaz flag e é logada `:187`, `:200`. Não há hoje leitor de produção encontrado que date início pela trilha; [nao verificado] para exaustividade dinâmica. |
| Persistência da trilha | `core.observ.evento`, `app/core/observ.py:94`, criação `:134` | Savepoint próprio `:133`, best effort `:105`, retorna `None` `:139`; adaptador `registrar_log` em `app/core/auditoria.py:8`, `:22`; timestamp `auto_now_add` `app/core/models.py:307`. |

Tripwire existente do silêncio: `app/chamados/tests/test_porta_arquivo_silencio.py:415` chama censo AST real; proíbe antigos sítios escritores `:422`; verifica materialização sem escrita do chamado `:432`; confere porta registrada `:437`; caso que morde escrita clandestina `:441`. Declaração da porta: `app/core/portas.py:196`. Nenhum desses testes foi executado.

Divergências **provadas** de `silenciado_ate`:

| Leitor | Sítio | Significado |
|---|---|---|
| Fila | `app/chamados/services/painel.py:190` | Exclui só `>agora`; vencido volta. |
| Instância | `app/chamados/models.py:512` | `bool(campo and campo > agora)`. |
| Partição | `app/chamados/services/verbos.py:48`, `:51` | Se tem verbo e campo preenchido: futuro ARQUIVADO; vencido HISTORICO. |
| Lista do painel | `app/chamados/services/painel.py:328` | Exclui `FORA_DA_LISTA_PADRAO`, que inclui HISTORICO (`verbos.py:18`); não é a fila. |
| Lista do colab | `app/chamados/services/janela_colab.py:73` | Exige NULL; vencido continua invisível. |
| Resposta do colab | `app/chamados/services/materializacao.py:718` | Qualquer preenchido dispara levantamento/resgate, inclusive vencido. |
| Busca admin | `app/templates/chamados/partials/_busca_arquivados.html:46` | Compara futuro diretamente no template. |
| Drawer colab | `app/templates/chamados/partials/_meus_arquivados.html:33` | Qualquer preenchido recebe rótulo Arquivado. |
| Botão Resgatar | `app/templates/chamados/partials/_lista_chamados.html:85` | Qualquer preenchido. |
| Estoque | `app/chamados/services/fila.py:122`, `:127` | Compara futuro diretamente. |
| Arquivados do painel | `app/chamados/services/painel.py:216` | Compara futuro diretamente; universo VIVOS, diferentemente da partição que só arquiva com verbo. |
| Ausência | `app/chamados/services/silencio_ausencia.py:81` | Compara futuro diretamente e cede a outro motivo. |
| Auditoria I5 | `app/chamados/management/commands/auditar_invariantes_chamados.py:645` | Mesmo `>agora`, expressão própria. |
| Escalonamento | `app/ponto/management/commands/escalonar_chamados_supervisao.py:46` | Exclui futuro diretamente. |
| Vassoura isento | `app/chamados/services/silencio_isentos.py:30` | Só NULL = alvo; vencido não é reescrito. |
| Prescrição | `app/chamados/management/commands/arquivar_prescritos.py:53`, `:93` | Só NULL. |
| Competência encerrada | `app/chamados/management/commands/arquivar_competencia_encerrada.py:100` | Só NULL. |
| Sem dia | `app/chamados/management/commands/arquivar_sem_dia_irrecuperavel.py:59` | Só NULL. |
| Lote admin | `app/chamados/views_arquivar.py:136` | Só NULL. |
| Pesquisa admin | `app/chamados/views_arquivar.py:165`, `:166` | Preenchido **ou** encerramento por tranca: universo de pesquisa, não fila. |
| Signal de chamado | `app/chamados/signals.py:294`, `:299` | Acorda no levantamento; qualquer preenchido impede novo silêncio. |

Fim exclusivo para o instante está coerente em quem compara `>agora`; significado do **vencido** não está coerente, como mostram fila/partição/lista acima. Nenhum campo de janela existente foi encontrado para conferir inclusive/fuso/causa: [nao verificado].

Flag e novos leitores:

- `core.constants.isento_ponto` não recebe dia e lê exclusivamente flag atual (`app/core/constants.py:13`, `:18`).
- `grade_da_celula` lê flag uma vez para todo `[ini,fim]`, `app/escala/services/leitor_celula.py:350`, `:360`, devolve em `:445`.
- `tarefa_do_dia._tarefa_pura` retorna todas instâncias sem ações se `isento`, antes da guarda de tranca: `app/chamados/services/tarefa_do_dia.py:188`, `:194`, `:195`. Portanto, janela histórica não equivale ao atual insumo da tarefa.
- Signals ainda silenciam chamados: flag salva `app/chamados/signals.py:271` → callback `:279`; chamado nasce/revive `:285` → callback `:306`; ambos `on_commit` `:281`, `:308`.
- Serviço isento ainda é escritor por delegação, não vigia puro: `app/chamados/services/silencio_isentos.py:34` → `arquivo.silenciar` `:43`; `silenciar_do_colab` `:52`. Flag não desligada não tem levantamento simétrico nesse código (`signals.py:272` simplesmente retorna se falsa).
- Django admin é somente leitura: `app/colaboradores/admin.py:48`.

Todos os leitores explícitos adicionais da flag achados por AST/aliases:

`app/api/views_mensageria.py:1221`, `:1236`, `:1273`; `app/chamados/management/commands/auditar_invariantes_chamados.py:400`; `app/chamados/management/commands/silenciar_chamados_isentos.py:35`; `app/chamados/reconciliador.py:851`; `app/chamados/services/adesao.py:65`; `app/chamados/services/declaracao_texto.py:104`; `app/escala/management/commands/termometro_regua.py:34`; `app/escala/regua_defesa.py:136`, `:355`, `:513`; `app/escala/services/escala_auto.py:171`; `app/escala/services/furos_diarios.py:33`, `:298`; `app/escala/services/previsto_cego.py:143`; `app/escala/services/propositor.py:456`; `app/escala/utils.py:904`; `app/folha/causas.py:35`; `app/folha/export.py:418`, `:1037`, `:1200`; `app/folha/previa.py:74`; `app/inteligencia/calculadores.py:325`; `app/ponto/marcos.py:116`; `app/ponto/management/commands/detectar_ausencias.py:105`; `app/ponto/management/commands/detectar_intervalo_ausente.py:148`; `app/ponto/management/commands/emitir_furo_retroativo.py:106`; `app/ponto/management/commands/mapa_divergencia.py:65`, `:99`; `app/ponto/management/commands/processar_alertas_avancados.py:32`; `app/ponto/management/commands/processar_alertas_turno.py:85`; `app/ponto/management/commands/propor_reorganizacao.py:35`; `app/ponto/management/commands/supra_juiz.py:228`; `app/ponto/management/commands/vigiar_motor_autoridade.py:46`; `app/ponto/services/bordas_realizado.py:71`; `app/ponto/services/cartorio.py:935`, `:1190`, `:1272`, `:1296`, `:1470`, `:1857`; `app/ponto/services/deslogados.py:157`; `app/ponto/services/esmeril_espelho.py:216`; `app/ponto/services/espelho.py:600`; `app/ponto/services/faltas_de_hoje.py:33`; `app/ponto/services/fechamento.py:366`; `app/ponto/services/vigia_de_hora.py:117`; `app/ponto/supra_juiz.py:220`; `app/relatorios/cartao_pela_celula.py:418`; fora de app, `bin/censo_folha.py:34`.

Esses leitores de folha/espelho devem manter o juiz atual da isenção; o dado fornecido não autoriza converter isenção econômica em quarentena de cobrança. Contrato de separação no anexo: [nao verificado] como arquivo do workspace.

Autoridades atuais:

- `app/docs/JUIZES.md:59` situa VIVOS/COBRANCA, `:61` dia do catálogo; `:78` tarefa, `:82` chamado pela fila, `:86` tranca.
- **Colisão de identificador da especificação:** `_C16` hoje já é motivo_do_veto (`app/core/juizes.py:131`, registro `:288`). `_C17` porta alcançável `:289`, `_C18` dia sem trabalho `:290`, `_C19` tarefa `:291`, `_C20` tranca `:292`. O nome simbólico antigo não pode ser transplantado literalmente.
- Não vi juiz registrado de quarentena hoje; [nao verificado] como ausência.
- Dia do item segue catálogo `app/chamados/catalogo/modulos.py:219` e parser `app/chamados/services/cobranca_folha.py:29`, `:34`, sem conversão de fuso (recorta primeiros dez caracteres do valor).
- Exceção DP já tem lista fechada `app/chamados/catalogo/modulos.py:265`.

Fatos do item 3: `suite_6.out`, `recorte_6.out`, `verificador_2910_0024.txt` não apareceram nem em `rg --files -uuu`; tamanho, mtime, conteúdo, resultado e causa histórica não são confirmáveis nesta cópia. “não sei” para confirmação independente; o anexo relata REPROVADO, oito achados e um não verificado [nao verificado].

Perguntas de sentido para consolidação (sem respondê-las):

1. **DECISAO:** como o bloqueio atual `isento=True` de todas as ações da tarefa deve coexistir com a quarentena temporal da cobrança para um dia anterior à última ligação? (`tarefa_do_dia.py:188`, `leitor_celula.py:360`, `core/constants.py:18`).
2. **DECISAO:** a troca da flag continua com trilha best effort atual, ou a exigência “mesmo ato com trilha” do aval exige recusa se a trilha falhar? (`colaboradores/views.py:176`, `:187`; `core/observ.py:105`, `:139`; aval apenas no dado [nao verificado]).

## 12/11 14:07 · sessao 01c4ef29d9a7 · - · - · origem=Ronald (tmux)

Auditoria somente leitura. Nenhum teste Django foi executado; os selos abaixo foram lidos, portanto seu resultado no HEAD está [nao verificado]. [nao verificado]

**Situação da porta:** o HEAD ainda registra a obra como reprovada e pendente, incluindo as sete curas nomeadas; não permite tratar a especificação antiga como implementação pousada — `app/docs/BACKLOG.md:765`. A varredura AST de 811 arquivos de produção, excluindo componentes `tests`, `migrations`, `docs`, sem erro de parse, não encontrou os modelos `QuarentenaColab`/`QuarentenaDiaDevolvido` nem as funções `definir_isencao`, `inicio_da_isencao`, `isencao_ligada`, `isencao_desligada`. Os dois serviços, template `_quarentenas` e migrations antigas também estão ausentes. Ausência sem arquivo:linha deve levar [nao verificado] conforme a exigência literal do usuário; pode ser sustentada adicionalmente pela obra pendente e pelos registros atuais — `app/docs/BACKLOG.md:765`, `app/core/juizes.py:272`, `app/core/portas.py:237`, `app/chamados/urls.py:26`.

**Registro ocupado:** `_C16` já responde ao veto de nascimento/reabertura e aponta para `motivo_do_veto`; copiar a inscrição antiga da quarentena por identificador sobrescreveria outra autoridade — `app/core/juizes.py:131`, `app/core/juizes.py:288`. Isso é incompatibilidade técnica de reaplicação, não pergunta de lei nova.

**Matriz do que existe hoje e do que falta provar**

| Estado/ato | Juiz/operação de hoje | Persistido/retorno | Escritor/tripwire |
|---|---|---|---|
| Abrir janela | Porta de quarentena não implementada [nao verificado]; obrigação está em `app/docs/BACKLOG.md:765` | Modelo/retorno da janela [nao verificado] | Único escritor e selo da entidade [nao verificado] |
| Fechar `fim=hoje` | Obrigação de **abrir/fechar** pela flag em `app/docs/CORTES.md:352`; backlog reforça “fechar e não levantar” em `app/docs/BACKLOG.md:765` | `fim=hoje`, preservação passado e retorno de hoje [nao verificado] | Porta/selo específico [nao verificado] |
| Levantar janela manual | Obrigação de devolver a cobrança em `app/docs/BACKLOG.md:765` | Janela/trilha/retorno [nao verificado] | Porta/selo específico [nao verificado] |
| Ligar flag | `passe_livre` decide e chama `gravar(...,'cadastro',...)` — `app/colaboradores/views.py:173`, `app/colaboradores/views.py:176` | Flag bool; trilha antes/depois — `app/colaboradores/views.py:183`; redirect — `app/colaboradores/views.py:203` | Persistência canônica `gravar` — `app/colaboradores/services/colaborador.py:55`; contrato genérico — `app/colaboradores/tests/test_porta_colaborador.py:28`; **não prova janela** |
| Desligar flag | Mesmo ato — `app/colaboradores/views.py:157`, `app/colaboradores/views.py:176` | Hoje não levanta o véu existente; teste consagra isso — `app/chamados/tests/test_varredura_isentos_evento.py:77` | Janela fechada pela flag [nao verificado] |
| Religar flag | Mesmo ato; receiver pós-save silencia por evento — `app/chamados/signals.py:271` | Hoje sem histórico temporal da flag no modelo/porta: primeiro/último início de janela [nao verificado] | Teste temporal liga/desliga/religa [nao verificado] |
| Admin cobra `(colab,dia)` | Permissão `regularizar_batida` + supervisão — `app/chamados/views_cobrar.py:313`, `app/chamados/views_cobrar.py:316`; trilha **antes do cartório** — `app/chamados/services/cobrar_dia.py:31`, `app/chamados/services/cobrar_dia.py:39` | `cobrar_dia_manual`, depois `julgar_celula(...toque=True)` — `app/chamados/services/cobrar_dia.py:39`; dict `ok/msg/chamados/perguntas` — `app/chamados/services/cobrar_dia.py:70` | Trilha por este ato; O44 casos um dia/dois colabs/idempotência — `app/chamados/tests/test_o44_tranca_nao_barra_o_toque.py:138`, `:186`, `:266`; **devolução de quarentena não provada** |

Três atos humanos já contam como toque do dia: cobrar, resolver e desarquivar — `app/chamados/juizes.py:1083`. O juiz lê usuário humano, instante estritamente posterior e identifica o dia da cobrança pela descrição — `app/chamados/juizes.py:1252`, `app/chamados/juizes.py:1261`, `app/chamados/juizes.py:1267`. A reaplicação precisa preservar a trilha anterior ao cartório; o hunk antigo não contempla a evolução explícita de hoje — `app/chamados/services/cobrar_dia.py:35`.

`registrar_log` captura exceções internamente; envolver flag e janela num `atomic` não prova, sozinho, trilha garantida — `app/core/auditoria.py:17`, `app/core/auditoria.py:25`. É uma divergência a registrar, não afirmar que a transação resolve.

**Oito classes da cura cba2eb66b, sítios atuais**

| Classe | Sítio de hoje a tocar/provar |
|---|---|
| App do colaborador | `perguntas_do_app` agora monta identity map e pode alimentar tranca em lote — `app/chamados/services/pergunta_no_app.py:45`; consulta e projeção novas usam essa seleção — `:61`, `:78`; filtragem de quarentena [nao verificado] |
| Vigias | `retencao` começa por `sem_dia`, turno atual e item no app — `app/chamados/services/pergunta_no_app.py:121`; `vigia_fio.censo` agora possui segunda passada do dono, identity map — `app/chamados/services/vigia_fio.py:395`, `:402`; `censo_alcance` é outro universo — `:493` |
| Permissão | Telas atuais só têm arquivar/desarquivar e suas travas por área — `app/chamados/views_arquivar.py:24`, `:52`; cobrar exige regularização e supervisão — `app/chamados/views_cobrar.py:313`; permissões da janela [nao verificado] |
| Trilha | Cobrança registra antes do julgamento — `app/chamados/services/cobrar_dia.py:39`; ausência de escala hoje retorna após essa trilha — `:42`; rollback de devolução da quarentena/trilha [nao verificado] |
| Pilha da batida | `gerar_perguntas_disputa` — `app/chamados/services/disputa_emissao.py:300`; chamado por rodada — `:634`; tetos atuais FICA são **144/218**, não os da cópia antiga — `app/ponto/tests/test_selo_chokepoint_escrita.py:228`, `:235`; painel operacional segue 18 — `app/core/tests/test_selo_performance.py:262` |
| Janela única | Predicado de janela de quarentena [nao verificado]; drawer ainda entrega queryset e relógio ao template — `app/chamados/views_arquivar.py:178`; registro ainda declara comparação do template pendente — `app/core/juizes.py:792` |
| DIFF | Medidas da cópia velha não certificam HEAD: obrigação atual no backlog — `app/docs/BACKLOG.md:765`; resultado novo em prod [nao verificado] |
| Disputa vazia | Hoje abre/agrega antes das perguntas e envia push da nova disputa sem verificar quantidade criada — `app/chamados/services/disputa_emissao.py:662`, `:669`; agrupador órfão já filtra dias na lista do admin antes de abrir disputa — `:1045`; deve conservar esse filtro ao acrescentar quarentena |

**Oito achados do verificador 29/10, sítios atuais**

1. **Fechar × levantar:** porta antiga ausente [nao verificado]; lei já resolve “abre/fecha” — `app/docs/CORTES.md:352`; cura explicitada — `app/docs/BACKLOG.md:765`; flag atualmente no `passe_livre` — `app/colaboradores/views.py:173`.
2. **Vigia só dentro da janela:** hoje o recorte ainda é isento + VIVOS + `silenciado_ate IS NULL` — `app/chamados/services/silencio_isentos.py:20`; cron `--alarme` às 07:05 — `app/config/crons.py:609`; não transportar o contador antigo `fora_da_janela` sem a cura já escrita — `app/docs/BACKLOG.md:765`.
3. **Segundo escritor da janela isento:** levantar janela pela UI ainda não existe [nao verificado]; obrigação da porta separada está explícita — `app/docs/BACKLOG.md:765`; split de flags sai do escritor atual `passe_livre` — `app/colaboradores/views.py:176`.
4. **Migration congelada e reversível:** migrations antigas ausentes [nao verificado]; requisito já está em `app/docs/BACKLOG.md:765`; não há objeto atual cuja reversão tenha sido provada [nao verificado].
5. **LEI-AKITA commit/RED:** commits da cópia antiga e logs do RED não verificáveis neste workspace [nao verificado]; regra de RED e linha exigida — `CLAUDE.md:30`, `CLAUDE.md:63`. [nao verificado]
6. **Ordem `retencao`:** permanece `dia=None`, hoje/futuro, no_app — `app/chamados/services/pergunta_no_app.py:126`; RED de quarentena antes deles [nao verificado].
7. **DIFF pela função real da migration:** implementação/resultado do DIFF de HEAD [nao verificado]; requisito de não replicar cálculo — `CLAUDE.md:37`; cura explicitada — `app/docs/BACKLOG.md:765`.
8. **Última ligação:** flag atual e logs em `app/colaboradores/views.py:173`, `:183`; janela e algoritmo temporal [nao verificado]; backlog já exige “última ligação” — `app/docs/BACKLOG.md:765`.

**Arquivos em comum**

SAIDA-NOTURNA cruza seguramente os arquivos de produção `app/chamados/models.py` e `app/chamados/juizes.py`, pois a reaplicação antiga cria modelos em `models.py` e altera leitores ligados aos juízes; no HEAD os sítios indicados continuam:

- `_pergunta_materializada_de_fato`, exceções via identidade e reavaliação pela lei — `app/chamados/juizes.py:662`, `:679`, `:689`, `:698`.
- `fato_do_marco_no_espelho`, marcos/duas pausas/lâmpadas — `app/chamados/juizes.py:930`, `:941`, `:955`, `:965`.
- Fonte única de identidade — `app/chamados/models.py:2511`.
- Porta `carimbar_identidade`, valida via — `app/chamados/models.py:3169`, `:3179`.
- Fonte de vias sem testemunha — `app/chamados/models.py:3733`, `:3738`.

E3 cruza `app/chamados/services/janela_colab.py` (função em `:40`, lista em `:50`), `app/chamados/services/verbos.py` e as novas superfícies de tarefa/porta pelo contrato _C19 — `app/core/juizes.py:291`. `porta_da_tarefa` não foi localizada como módulo/função literal nas leituras deste subagente [nao verificado]; o outro explorer está incumbido da busca ampla.

**Possíveis DECISOES para consolidar, apenas perguntas**

- Pergunta de **conversa** tem `data_evento` civil e não dia do fato — `app/chamados/services/pergunta_no_app.py:104`; a emissão agora separa conversa/fato — `app/chamados/services/disputa_emissao.py:421`, `:439`. O filtro de quarentena deve tratar a conversa como item sem dia (apenas janela aberta) ou como dia civil em janela fechada? A spec antiga filtrava `data_evento` cru; não resolver semanticamente sem corte.
- O toque humano O44 já tem três atos — `app/chamados/juizes.py:1083`; a devolução de quarentena por cobrar está no diff antigo, mas a aplicação aos outros dois atos precisa ser demonstrada segundo a regra existente — `app/docs/CORTES.md:349`. Não sugerir lei nova; levantar a lacuna como pergunta somente se o censo integral não encontrar cobertura.
- Garantia de trilha obrigatória × captura best-effort de `registrar_log` — `app/core/auditoria.py:25`: o comportamento já aceito da flag é manter a isenção mesmo se trilha falhar — `app/colaboradores/views.py:188`. A reaplicação precisa explicitar qual comportamento já autorizado conserva, sem afirmar atomicidade de trilha que o código não dá.

## 12/11 14:05 · sessao 85db53f6de83 · AUDITORIA-DE-TRAVAS · leitura · origem=Code

**O v3 ainda não fecha o contrato.** Corrige a distinção entre universo e andabilidade, mas mantém problemas no RED, nos selos, na validação e na semente. (`contrato_item3_v3.txt:1`, `:3`, `:7`, `:8`; `contrato_item3_v3_semente.txt:2`, `:21`)

Nas referências aos **anexos fornecidos**, contei cada linha física, começando pelo título — ou `[ORDEM]`, na semente — como linha 1. Pousos abaixo são declarações documentais; confirmação em produção: **[nao verificado]**.

**1. Os seis furos do v2**

| Furo | Veredito sobre o v3 |
|---|---|
| **Universo × andável / RED ca320b69f** | **Semântica corrigida; promessa do RED não fechada.** Pertencimento passa a incluir todos de `[ORDEM]`, inclusive travados; restantes andáveis são somente os posteriores ao primeiro sem `TRAVA`. Isso permite distinguir os dois casos exigidos. Não demonstra que a fixture antiga passe sem edição. (`contrato_item3_v3.txt:3`, `:8`; `bin/hook_stop_fila1.py:1243`, `:1245`) |
| **Leitores não migrados e selos** | **Parcial.** A cláusula “TODOS os leitores do censo v2” amplia a cobertura, mas a enumeração omite leitores indiretos e há propriedades antigas incompatíveis com a nova autoridade. Migrar apenas fixtures não resolve esses conflitos. (`contrato_item3_v3.txt:5`, `:8`; `bin/interrompidas_estado.py:93`; `bin/tests/test_hook_estado_e_a_coluna.sh:53`; `bin/tests/test_censo_antes_da_vaga.sh:544`) |
| **Janela/cauda do publicador** | **Fecha a obrigação de preservar; não fecha completamente a divisão entre preservar e regenerar.** A “cauda após OBRAS:FIM” contém também tabelas geradas. Preservá-la inteira e acrescentar novamente o corpo gerado duplicaria conteúdo. O contrato precisa delimitar a substituição dessa parte, além da preservação manual. (`contrato_item3_v3.txt:6`; `bin/gerar_backlog.py:100`, `:127`; `app/docs/BACKLOG.md:2042`, `:2044`, `:2369`, `:2371`) |
| **942 × len−1** | **Corrige a unidade da comparação**, adotando “andáveis além do em curso” nos dois lados. **O 941 não corresponde à cópia entregue:** recontagem em memória pelas funções atuais deu **945 totais e 944 além do topo**. O valor de um HEAD externo continua **[nao verificado]**. (`contrato_item3_v3.txt:8`; `bin/hook_stop_fila1.py:231`, `:270`; `app/docs/BACKLOG.md:712`, `:2042`) |
| **Semente ambígua** | **Não fechado.** Há identidades sem linha válida em OBRAS, exclusão indevida de delta e contradição de precedência. A seção 5 encaminha a conferência; não resolve os fatos apresentados. (`contrato_item3_v3.txt:9`; `contrato_item3_v3_semente.txt:2`, `:10`, `:11`, `:21`; `app/docs/BACKLOG.md:1916`, `:1969`, `:2404`, `:2417`) |
| **Vazio/repetição/gramática/falha aberta** | **Parcial.** Vazio válido, repetição intra/entre seções, seções e gramática inválidas passam a ter regra. Restam ilegibilidade/decodificação e falhas da leitura do BACKLOG sem conversão explicitada em `ErroFonte`; a lista de erros também exige linha em OBRAS somente para `[ORDEM]`, embora a identidade da seção 0 valha para todas as seções. (`contrato_item3_v3.txt:2`, `:7`, `:8`; `bin/hook_stop_fila1.py:1460`, `:1461`) |

Há mais dois pontos concretos nessa especificação:

- **Topo com TRAVA:** permitido pelo v3. Se `_ids_que_nao_andam()` virar mecanicamente “ids com TRAVA”, o veto atual rejeitará o PAREI do próprio topo antes de compará-lo com o em curso. A migração precisa mudar esse veto; o selo A/B/C proposto deixa A sem TRAVA e não cobre esse caso. (`contrato_item3_v3.txt:3`, `:5`, `:8`; `bin/hook_stop_fila1.py:1298`, `:1302`, `:1310`)
- **Gramática das travas:** a seção 0 nomeia `aval`, `!`, `lei`, `desenho`, `portao`, mas a semente usa `E3-#4-ATRAS-DA-QUARENTENA` e `PERGUNTA-DE-DESENHO-S1`. Falta dizer se aquela enumeração é fechada ou se admite motivo livre; na leitura fechada, a própria semente é inválida. (`contrato_item3_v3.txt:2`; `contrato_item3_v3_semente.txt:3`, `:5`)

**2. O RED fica verde sem edição?** [nao verificado]

**Não há suporte para essa promessa.** O arquivo `bin/tests/test_hook_andavel_e_a_ordem_viva.sh` não está disponível nesta cópia: seu conteúdo literal e as mudanças exatas nas linhas 51/73 são **[nao verificado]**. O RELATO registra ca320b69f na outra raia `wt-travas-105806`. (`app/docs/RELATO.md:37`)

Para a fixture descrita no anexo — que escreve marcador/prosa/coluna — a semântica nova exige estas mudanças:

| Cenário informado | Mudança necessária na fixture |
|---|---|
| **:51 — restantes travados** | Fornecer o `.ids` da árvore temporária, com o mesmo topo e os restantes em `[ORDEM]` com `TRAVA`; ruído aberto fica fora de `[ORDEM]`, POS-20 em sua seção. Manter a exigência de PAREI do topo liberar. (`contrato_item3_v3.txt:2`, `:3`, `:8`; conteúdo literal do RED: **[nao verificado]**) |
| **:73 — restante aberto** | Alterar o **`.ids`**, retirando a TRAVA do restante que deve andar; alterar somente a coluna não pode mudar a resposta. Manter a exigência de bloquear com um restante andável. (`contrato_item3_v3.txt:3`, `:4`, `:8`; conteúdo literal do RED: **[nao verificado]**) |

Também é necessário dirigir o leitor ao `.ids` da fixture, junto do BACKLOG correspondente; usar a fonte real não representaria os cenários temporários. O selo vizinho verificável hoje substitui apenas `BACKLOG`/`RELATO`, escreve a tabela e alterna a coluna. (`bin/tests/test_hook_parei_com_andavel.sh:22`, `:25`, `:41`, `:56`)

**Isso mantém a propriedade do RED, mas edita sua fixture.** Portanto é compatível com “o RED fica” entendido como conservar os casos e as asserções; é incompatível com **“SEM ser editado”**. O próprio v3 manda migrar fixtures na linha 8 e proíbe editar na linha 1. (`contrato_item3_v3.txt:1`, `:8`)

**3. Censo: omissões e perguntas diferentes**

A lista **nominal** da seção 1 deixa estes sítios de fora; a cláusula geral “TODOS” os abrange, mas precisa alcançar as interfaces e os textos indicados. Vários já delegam, portanto a omissão nominal não prova que sobreviverá um parser próprio. (`contrato_item3_v3.txt:5`; `bin/interrompidas_estado.py:93`; `bin/handoff_sessao.sh:64`)

| Omissão nominal | O que precisa acompanhar a troca |
|---|---|
| `interrompidas_estado.py` | Topo consultado ao hook; preservar separadamente a leitura documental de interrupção/fusão/fechamento. (`bin/interrompidas_estado.py:54`, `:93`, `:105`, `:125`) |
| Primeira parte do handoff | Topo, em curso e PAREI; também o rótulo “MARCADOR”. (`bin/handoff_sessao.sh:64`, `:66`, `:71`, `:120`, `:123`) |
| Wrapper e placar | Delegação do censo e do alarme. (`bin/codex_pergunta.sh:247`; `bin/placar_code.sh:368`) |
| Chamadores do fabricante | Delegação ao seletor. (`bin/fabricante.sh:129`; `bin/fabricante_alvo.py:315`, `:320`) |
| Publicação e entradas | Transporte dos documentos e chamadas dos instrumentos. (`bin/relato.sh:115`, `:116`; `.claude/settings.json:9`, `:20`, `:29`, `:40`, `:53`, `:68`) |

`gerar_backlog.py` está fora da seção 1, mas coberto pela seção 2. As declarações documentais da autoridade antiga continuam sem emenda explicitamente listada no v3. (`contrato_item3_v3.txt:5`, `:6`; `app/docs/LEIS.md:140`; `app/docs/CORTES.json:736`; `app/docs/BACKLOG.md:1576`)

Quanto a **OUTRA pergunta**:

| Leitor | Fronteira correta |
|---|---|
| Handoff: avais de hoje e raia20 | Pergunta fechamento documental, não pertencimento à ORDEM. (`bin/handoff_sessao.sh:171`, `:209`, `:216`) |
| Interrompidas | Pergunta interrupção, fusão, fechamento e retorno; somente o topo vem da ORDEM. (`bin/interrompidas_estado.py:105`, `:116`, `:121`, `:125`) |
| `gerar_backlog_topo.py` | Resume O-números abertos e ordena numericamente; não representa ORDEM VIVA. (`bin/gerar_backlog_topo.py:19`, `:26`, `:39`) |
| Fabricante | Pergunta elegibilidade para sorteio. A exceção “livres/O-F fora da fila 1” precisa excluir efetivamente os membros de `[ORDEM]`; hoje o ramo “livre” também aplica veto pela coluna. (`contrato_item3_v3.txt:5`; `bin/fabricante_alvo.py:256`, `:273`, `:275`, `:282`) |
| Testes de existência/padrão | Podem continuar documentais. (`app/core/tests/test_marca_cron15.py:89`; `app/chamados/tests/e4b_c4_verificar.py:209`; `bin/tests/test_prompt_virou_item.sh:25`) |

**Há conflitos que não se resolvem apenas mudando fixture:**

- `test_hook_estado_e_a_coluna.sh` exige que tachado, FECHADA e aguardando invalidem o topo. O v3 retira essa autoridade da coluna e admite topo com TRAVA. Pode conservar a propriedade documental de fechamento; precisa substituir as antigas propriedades sobre a escolha do topo. (`bin/tests/test_hook_estado_e_a_coluna.sh:53`, `:54`, `:55`, `:59`; `contrato_item3_v3.txt:3`, `:4`, `:8`)
- O censo exige **NAO SEI/nega** para `O9` contra `O9 (C)`, caixa trocada e obra qualificada contra célula nua. `pertence(id)` por igualdade exata pode devolver **False**, que dispensa censo. Logo a promessa de preservar todas essas propriedades conflita com a nova identidade; precisa ser resolvida expressamente. (`bin/tests/test_censo_antes_da_vaga.sh:544`, `:549`, `:575`, `:603`; `bin/disjuntor.py:1707`, `:1708`; `contrato_item3_v3.txt:5`, `:8`)

**4. Conferência da semente**

O bloco OBRAS desta cópia vai de **712 a 2042**. Linha posterior não satisfaz a identidade exigida pela seção 0. (`app/docs/BACKLOG.md:712`, `:2042`; `contrato_item3_v3.txt:2`)

| Seção / ID | Evidência e classificação documental |
|---|---|
| ORDEM — `SAIDA-NOTURNA-NO-DIA-CERTO` | Linha exata no BACKLOG: **[nao verificado]**. O RELATO registra aval/despacho, mas não fornece a linha exigida em OBRAS. (`app/docs/RELATO.md:29`) |
| ORDEM — `FIO-E0-V4-E3-LEITORES` | **Dentro**, fila 1, ABERTA, EM OBRA. (`app/docs/BACKLOG.md:1873`) |
| ORDEM — `QUARENTENA-DO-COLAB` | **Dentro**, REPROVADA, espera a vez. A linha não declara fila 1; esse pertencimento fica **[nao verificado]**. (`app/docs/BACKLOG.md:765`) |
| ORDEM — `LASTRO-FAIXA-SEM-DEFAULT` | **Dentro**, fila 1, ABERTA. A linha diz que o aval responde o DESENHO; não comprova a trava `PERGUNTA-DE-DESENHO-S1`. (`app/docs/BACKLOG.md:1813`; `contrato_item3_v3_semente.txt:5`) |
| ORDEM — `C20-O168` | **Dentro**, fila 1, espera vaga, ABERTA. (`app/docs/BACKLOG.md:2008`) |
| ORDEM — `RAIZ-DA-DISPUTA-F0-CASO-7279` | Linha exata **fora**, fila 1, sonda, frente da F1. (`app/docs/BACKLOG.md:2417`) |
| ORDEM — `RAIZ-DA-DISPUTA-F0F1` | **Dentro**, fila 1; F0/F1 prontos, F1 respondida, c22 aguarda vaga. Não declara pouso. (`app/docs/BACKLOG.md:1882`) |
| ORDEM — `AUDITORIA-DE-TRAVAS-CORTE` | Linha exata **fora**, instrumento EM CURSO; não declara literalmente fila 1. (`app/docs/BACKLOG.md:2404`) |
| ORDEM — `APP-MENSAGENS-MSG-0` | **Dentro**, fila 1/raia 2, ABERTA, nada construído. (`app/docs/BACKLOG.md:1916`) |
| ORDEM — `TRIAGEM-OFFLINE-SEM-CAMERA` | **Dentro**, REVERTIDA, retorno condicionado; sua inclusão na fila 1/raia 2 aparece na RAIA-2-BO-VISIVEL. (`app/docs/BACKLOG.md:1915`, `:1906`) |
| POS-20 — `BUG-CADASTRO-NAO-ALCANCA-O-FUTURO` | Linha exata **fora**, POS-20/frente da fila 2, próxima vaga de construção. (`app/docs/BACKLOG.md:2408`) |
| POS-20 — `UI-PAINEL-FILA-OPUS-V5` | Linha exata **fora**, POS-20. O pouso citado é da tela; esta linha registra os achados restantes. (`app/docs/BACKLOG.md:2407`) |
| POS-20 — `AUDITORIA-ITEM6-CONTADOR-124` | Linha exata no BACKLOG: **[nao verificado]**. O RELATO diz “NAO construido → fila”, sem classificá-lo como POS-20. (`app/docs/RELATO.md:9`) |
| POS-20 — `TRIAGEM-FILA-OPUS-V6` | **Dentro**, POS-20, ABERTA. (`app/docs/BACKLOG.md:1815`) |
| POS-20 — `E4B-FILA-OPUS-C14` | **Dentro**, POS-20. (`app/docs/BACKLOG.md:1896`) |
| POUSADAS — `FIO-E0-V4-E4B` | **Dentro**, POUSOU 12/11 08:46, commit 087fe758a. (`app/docs/BACKLOG.md:1894`) |
| POUSADAS — `FIO-E0-V4-E4A-2` | **Dentro**, NO AR, merge 613a51498. (`app/docs/BACKLOG.md:1893`) |
| POUSADAS — `O169-FECHADA` | **Dentro, mas a classificação não está provada:** é fila 1 REGISTRAR, delta do contador, B3/volta 5. O pouso citado pertence a **C20-O169**. (`app/docs/BACKLOG.md:1969`, `:1798`) |

Portanto, a ORDEM já contém **um id sem linha exata demonstrada e dois com linha somente fora do bloco**. POS-20 também contém duas linhas externas e um id sem linha exata demonstrada. A validação não pode aceitar esses casos como se satisfizessem a seção 0. (`contrato_item3_v3_semente.txt:2`, `:7`, `:9`, `:13`, `:14`, `:15`; `contrato_item3_v3.txt:2`, `:7`; `app/docs/BACKLOG.md:2404`, `:2407`, `:2408`, `:2417`; linhas ausentes: **[nao verificado]**)

**Contradição de precedência, sem escolher ordem:** a semente põe APP antes de TRIAGEM; a própria linha de APP determina **depois da TRIAGEM**. (`contrato_item3_v3_semente.txt:10`, `:11`; `app/docs/BACKLOG.md:1916`)

**Faltante explícito:** `BUG-ORFAO-COM-PAR` está declarado **ESTRUTURA fila 1**, com sonda/classe/cura pendentes, e não está na semente; sua linha também está fora de OBRAS. Há ainda `TRAVA-MENOS-FILA-COM-SELO`, literalmente fila 1 instrumento **ABERTA**. (`app/docs/BACKLOG.md:2418`, `:2396`; `contrato_item3_v3_semente.txt:1`–`:22`)

Outras linhas continuam declarando fila 1/ABERTA sem classificação na semente: `MEDE-CLEAR-X-COMPACT`, `CODEX-QUEIMA-B`, `C20-FIO-MUDO`, `E3-X1-FOLGA-NAO-E-SEM-COBERTURA`, `SONDA-SEM-GRANT` e `AUDITORIA-0911`. Isso demonstra reconciliação documental incompleta; não certifica que todas devam executar agora. (`app/docs/BACKLOG.md:1796`, `:1797`, `:1817`, `:1876`, `:1979`, `:1981`; `contrato_item3_v3_semente.txt:1`–`:22`)

As **DUVIDAS** do fim:

- **E4c/E4d:** são etapas de `FIO-E0-V4-E4-ESCRITORES`: E4c leitores e E4d parar projeção. Não há identidade própria demonstrada para inseri-las separadamente. Usar o id da obra agrega etapas; usar os aliases viola a identidade exata. O contrato precisa resolver essa representação. (`app/docs/BACKLOG.md:1885`; `contrato_item3_v3.txt:2`)
- **BO-0611-05:** a linha própria continua B3/PARADA; o complemento registra bateria e aval de pouso, sem demonstrar sua efetivação. Estado final: **[nao verificado]**. (`app/docs/BACKLOG.md:1897`, `:1901`)
- **BO-DISPUTA-SEM-PERGUNTA:** a linha própria registra construtor lançado; RAIZ-F0F1 diz expressamente **ESTACIONADA**, com o bug seguindo até F2. Não há pouso demonstrado nessas linhas. (`app/docs/BACKLOG.md:1898`, `:1882`)
- **BUG-CADASTRO:** a relação do item 4(b) com essa obra está documentada no RELATO. Existe discrepância entre o BACKLOG POS-20/frente da fila 2 e a construção já despachada/entregue parcialmente, com 4(b) respondido pelo aval. Nenhuma dessas evidências declara sua transferência para fila 1; não cabe inventá-la nem ignorar o aval. (`app/docs/BACKLOG.md:2408`; `app/docs/RELATO.md:39`, `:50`, `:92`; `aval_respostas_1211_1230.txt:2`)
- **DECLARADO-X-GRAVADO:** o RELATO registra despacho de medida/censo e “Nada construido”, mas não demonstra pertencimento à fila 1 nem linha exata em OBRAS. Portanto **entrada em ORDEM: [nao verificado]**; a identidade exigida impede incluí-lo como está. (`app/docs/RELATO.md:6`, `:7`; `contrato_item3_v3.txt:2`)

## 12/11 14:02 · sessao 183b52816031 · - · - · origem=Ronald (tmux)

A conferência exata da primeira coluna deu:

| Seção/ID da semente | Linha do BACKLOG e conclusão documental |
|---|---|
| ORDEM — SAIDA-NOTURNA-NO-DIA-CERTO | Não ocorre literalmente na cópia; nenhuma linha exata em OBRAS. Bloco delimitado por `app/docs/BACKLOG.md:712` e `:2042`; estado/pertencimento **[nao verificado]**. |
| ORDEM — FIO-E0-V4-E3-LEITORES | `app/docs/BACKLOG.md:1873`: fila 1, ABERTA, EM OBRA. |
| ORDEM — QUARENTENA-DO-COLAB | `app/docs/BACKLOG.md:765`: linha própria dentro; REPROVADA, espera a vez. A linha não declara literalmente fila 1; pertencimento à fila 1 **[nao verificado]**. |
| ORDEM — LASTRO-FAIXA-SEM-DEFAULT | `app/docs/BACKLOG.md:1813`: CONGELA-O-20, obra LASTRO, fila 1, ABERTA. |
| ORDEM — C20-O168 | `app/docs/BACKLOG.md:2008`: fila 1, espera vaga, ABERTA. |
| ORDEM — RAIZ-DA-DISPUTA-F0-CASO-7279 | `app/docs/BACKLOG.md:2417`: fila 1, frente da F1, sonda; linha exata **fora** de OBRAS, depois de `:2042`. |
| ORDEM — RAIZ-DA-DISPUTA-F0F1 | `app/docs/BACKLOG.md:1882`: fila 1, F0+F1 prontos e F1 respondida; c22 na vaga após E3 e E4B; não diz pousada. |
| ORDEM — AUDITORIA-DE-TRAVAS-CORTE | `app/docs/BACKLOG.md:2404`: instrumento EM CURSO; linha exata **fora** de OBRAS. Não declara literalmente fila 1. |
| ORDEM — APP-MENSAGENS-MSG-0 | `app/docs/BACKLOG.md:1916`: fila 1, raia 2, ABERTA, nada construído. |
| ORDEM — TRIAGEM-OFFLINE-SEM-CAMERA | `app/docs/BACKLOG.md:1915`: REVERTIDA, volta condicionada a classe nomeada/RED; fila BO. `:1906` a coloca na fila 1 raia 2. |
| POS-20 — BUG-CADASTRO-NAO-ALCANCA-O-FUTURO | `app/docs/BACKLOG.md:2408`: POS-20, frente da fila 2, construção na próxima vaga; linha exata **fora** de OBRAS. |
| POS-20 — UI-PAINEL-FILA-OPUS-V5 | `app/docs/BACKLOG.md:2407`: POS-20; linha exata **fora** de OBRAS. O “pousada df2be6b12” refere a tela, cujos nove achados são esta fila. |
| POS-20 — AUDITORIA-ITEM6-CONTADOR-124 | Não ocorre literalmente na cópia; nenhuma linha exata em OBRAS. Pertencimento/estado **[nao verificado]**. |
| POS-20 — TRIAGEM-FILA-OPUS-V6 | `app/docs/BACKLOG.md:1815`: POS-20, ABERTA; linha dentro de OBRAS. |
| POS-20 — E4B-FILA-OPUS-C14 | `app/docs/BACKLOG.md:1896`: POS-20; linha dentro de OBRAS. |
| POUSADAS — FIO-E0-V4-E4B | `app/docs/BACKLOG.md:1894`: POUSOU 12/11 08:46, commit 087fe758a; linha dentro. Pouso declarado documentalmente, não confirmado em produção. |
| POUSADAS — FIO-E0-V4-E4A-2 | `app/docs/BACKLOG.md:1893`: NO AR, merge 613a51498; linha dentro. Pouso declarado documentalmente. |
| POUSADAS — O169-FECHADA | `app/docs/BACKLOG.md:1969`: **não prova o pouso deste delta**. Diz fila 1 REGISTRAR, delta do contador, B3 desenho e volta 5. O pouso ali citado pertence a **C20-O169**, cuja linha é `:1798`. |

A semente viola a identidade da seção 0 já na ORDEM: primeiro ID inexistente, RAIZ-CASO e AUDITORIA com linha somente fora do bloco. Também há POS-20 com linha fora ou inexistente; a seção 0 exige linha de OBRAS para todos, mas a lista de erros da seção 3 menciona explicitamente só IDs de ORDEM, deixando inconsistente a validação das exclusões. (`contrato_item3_v3.txt:2`, `:7`; `app/docs/BACKLOG.md:2042`, `:2404`, `:2407`, `:2408`, `:2417`)

Há contradição de ordem explícita sem precisar escolher uma: a semente põe APP antes de TRIAGEM, enquanto a própria linha de APP diz **depois da TRIAGEM**. (`contrato_item3_v3_semente.txt:10`, `:11`; `app/docs/BACKLOG.md:1916`)

Faltantes que o documento ainda declara fila 1 aberta incluem **BUG-ORFAO-COM-PAR** (`app/docs/BACKLOG.md:2418`, fora do bloco), **TRAVA-MENOS-FILA-COM-SELO** (`:2396`, fila 1 instrumento ABERTA, fora), **FIO-E0-V4-E4-ESCRITORES** (`:1885`, fila 1, contém E4c/d), **SONDA-SEM-GRANT** (`:1979`, ABERTA), **AUDITORIA-0911** (`:1981`, ABERTA), **MEDE-CLEAR-X-COMPACT** (`:1796`, ABERTA), **CODEX-QUEIMA-B** (`:1797`, ABERTA) e **C20-FIO-MUDO** (`:1817`, ABERTA). Isso não decide execução nem certifica estado atual em produção; demonstra que a semente não é reconciliação documental completa. (`contrato_item3_v3.txt:3`, `:9`)

DUVIDAS:

- **E4c/E4d:** não têm linha própria exata. São etapas da linha **FIO-E0-V4-E4-ESCRITORES**, que declara E4c leitores e E4d parar projeção; usar o ID da obra perde distinção das etapas, usar “E4c/E4d” viola identidade da seção 0. O desenho precisa resolver a identidade, não o parser. (`app/docs/BACKLOG.md:1885`; `contrato_item3_v3.txt:2`)
- **BO-0611-05:** sua linha permanece B3/PARADA; a linha de complemento registra aval de pouso, bateria e manutenção dos 403, mas não prova efetivação do pouso. Estado final **[nao verificado]**. (`app/docs/BACKLOG.md:1897`, `:1901`)
- **BO-DISPUTA-SEM-PERGUNTA:** linha própria registra construtor lançado; RAIZ-F0F1 registra explicitamente **ESTACIONADA**, bug segue até F2. Não há prova documental de pouso nessas linhas. (`app/docs/BACKLOG.md:1898`, `:1882`)
- **BUG-CADASTRO:** linha exata diz POS-20 e próxima vaga, fora de OBRAS; o aval fornecido aprova 4(b), mas não nomeia o ID completo nem declara formalmente sua migração entre seções. Não presumir identidade/pertencimento só pelo prefixo. A ligação entre “BUG-CADASTRO 4(b)” e esta obra específica é **[nao verificado]**. (`app/docs/BACKLOG.md:2408`; `aval_respostas_1211_1230.txt:2`; `contrato_item3_v3.txt:2`)
- **DECLARADO-X-GRAVADO:** o ID não aparece literalmente nem tem primeira coluna exata; entrada em ORDEM/POS-20/POUSADAS **[nao verificado]**. A regra de identidade impede incluí-lo como está. (`contrato_item3_v3.txt:2`; limites de OBRAS em `app/docs/BACKLOG.md:712`, `:2042`)

## 12/11 14:02 · sessao 8e13c5768979 · - · - · origem=Ronald (tmux)

**A lista nominal do §1 não reproduz todo o censo; a cláusula “TODOS os leitores do censo v2” cobre as omissões genericamente.** Faltam nominalmente `interrompidas_estado.py`, as primeiras leituras do handoff e os transportadores/entradas abaixo. Isso não demonstra segundo parser sobrevivente: vários já delegam ao hook e herdarão a mudança se as interfaces forem conservadas. (`contrato_item3_v3.txt:5`; `bin/interrompidas_estado.py:54`, `:93`; `bin/handoff_sessao.sh:64`, `:71`, `:120`)

| Fora da enumeração nominal do §1 | Situação |
|---|---|
| `interrompidas_estado.py` | Pergunta topo ao hook; lê INTERROMPIDA/FUNDIDA e fechamento documental por outra pergunta. Migrar o topo indiretamente, conservar o medidor documental. (`bin/interrompidas_estado.py:93`, `:105`, `:121`, `:125`) |
| Primeira parte de `handoff_sessao.sh` | Topo, em curso e PAREI já vêm do hook; falta atualizar “MARCADOR” e comentários antigos. (`bin/handoff_sessao.sh:58`, `:64`, `:66`, `:71`, `:120`, `:123`) |
| `codex_pergunta.sh` | Delega o censo ao disjuntor; linha atual é **247**, não 228. (`bin/codex_pergunta.sh:247`) |
| `placar_code.sh` | Delega o alarme. (`bin/placar_code.sh:368`) |
| `fabricante.sh`; `fabricante_alvo.py` chamadas finais | Delegam o seletor migrado. (`bin/fabricante.sh:129`; `bin/fabricante_alvo.py:315`, `:320`) |
| `relato.sh` publicação | Transporta os dois documentos, não decide pertencimento. (`bin/relato.sh:115`, `:116`) |
| `.claude/settings.json` | Entradas dos instrumentos e leitura do handoff; continuam apontando para `/home/ronald/saas-hasner`. Execução desta cópia nesses caminhos: [nao verificado]. (`.claude/settings.json:9`, `:20`, `:29`, `:40`, `:53`, `:68`) |
| `gerar_backlog.py` | Não está nominalmente no §1, mas está coberto pelo §2. (`contrato_item3_v3.txt:6`) |
| Selos | Não enumerados no v3; §4 remete genericamente ao censo e exige lista no RELATO. (`contrato_item3_v3.txt:8`) |

**As exceções por “OUTRA pergunta” fazem sentido, com estas fronteiras:**

- Avais de hoje ainda não cumpridos e linhas da raia20 perguntam fechamento documental, não pertencimento. (`bin/handoff_sessao.sh:139`, `:171`, `:209`, `:216`)
- Interrompidas pergunta estado, fusão, fechamento e retorno ao topo: só a parte do topo pertence à migração da ORDEM. (`bin/interrompidas_estado.py:105`, `:116`, `:121`, `:125`)
- `gerar_backlog_topo.py` resume O-números abertos e ordena por número; não é ORDEM VIVA. (`bin/gerar_backlog_topo.py:19`, `:26`, `:39`)
- Fabricante pergunta elegibilidade de sorteio: dinheiro, “só registrar”, espera por Ronald e níveis livre/escalonado. Não equivale simplesmente a “pertence à fila 1”. A exceção do v3 só funciona se o ramo preservado de livres/O-F for efetivamente limitado aos **fora de [ORDEM]**; do contrário, um integrante da fila pode continuar julgado pela coluna no ramo “livre”. (`contrato_item3_v3.txt:5`; `bin/fabricante_alvo.py:256`, `:259`, `:261`, `:273`, `:275`, `:282`)
- Existência do item e padrão declarado da obra são perguntas documentais. (`app/core/tests/test_marca_cron15.py:89`; `app/chamados/tests/e4b_c4_verificar.py:209`; `bin/tests/test_prompt_virou_item.sh:25`)

**Há dois conflitos substantivos na promessa de migrar fixtures preservando a propriedade de todos os selos:**

1. `pertence(id)` por igualdade exata altera a propriedade do censo: hoje `O9` contra `O9 (C)`, caixa trocada e obra qualificada contra célula nua devem resultar em **NAO SEI/nega**, não FORA. Também `O9` exato acompanhado de `O9 (C)` deve negar. Igualdade simples retorna False ou True, e False dispensa censo. Isso exige decisão explícita no contrato; trocar fixture não preserva essa propriedade. (`contrato_item3_v3.txt:2`, `:5`, `:8`; `bin/tests/test_censo_antes_da_vaga.sh:544`, `:549`, `:575`, `:603`; `bin/disjuntor.py:1707`, `:1708`)

2. `test_hook_estado_e_a_coluna.sh` exige que tachado, FECHADA e aguardando façam o topo ser “parado”. O v3 retira essa autoridade da coluna e declara topo o primeiro id com ou sem TRAVA. Conservar o teste documental de `_item_fechado` é possível; conservar as antigas asserções sobre o topo não é. O contrato precisa identificar propriedades revogadas e propriedades preservadas, em vez de prometer apenas migração de fixture. (`contrato_item3_v3.txt:3`, `:4`, `:8`; `bin/tests/test_hook_estado_e_a_coluna.sh:53`, `:54`, `:55`, `:59`)

As referências a `contrato_item3_v3.txt` acima numeram o anexo fornecido, com título na linha 1; as referências ao código usam as linhas desta cópia. As funções atuais do disjuntor estão em `:1623` e `:1672`, diferentes das referências históricas do v2/v3. (`bin/disjuntor.py:1623`, `:1672`)

## 12/11 14:01 · sessao 00795d4fdce6 · - · - · origem=Ronald (tmux)

**O v3 corrige a diferença universo/andabilidade no desenho, mas não sustenta “RED verde sem editar”.** [contrato_item3_v3.txt:1-3, anexo fornecido] [nao verificado]

- `[ORDEM]` inclui itens travados; os restantes só andam se não tiverem `TRAVA`. Portanto A/B/C com B e C travados admite PAREI de A; removendo a trava de C, deve bloquear. Isso coincide com a regra existente: `_relato_parou` exige PAREI do topo e nenhum restante andável. [contrato_item3_v3.txt:3, :8, anexo; bin/hook_stop_fila1.py:1232, :1243, :1245]
- O arquivo `bin/tests/test_hook_andavel_e_a_ordem_viva.sh` não existe nesta cópia; seu conteúdo literal e mudanças exatas nas linhas 51/73 são **[nao verificado]**. O RELATO registra o RED ca320b69f em outra worktree, `wt-travas-105806`. [app/docs/RELATO.md:37]
- Pela descrição fornecida do RED, precisa: criar `.ids` na fixture; direcionar o leitor compartilhado para esse arquivo; manter os mesmos IDs e linhas OBRAS; representar os restantes travados como `ID | TRAVA: !` no primeiro cenário; retirar a TRAVA do restante aberto no segundo; manter ruído/POS-20 fora de ORDEM e preservar as expectativas liberar/bloquear. O marcador/prosa pode permanecer como ruído, mas não pode alimentar a ordem. [item3_contrato_v2.r, seção “Os selos”, entrada do RED; contrato_item3_v3.txt:2-3, :8, anexos; conteúdo literal do RED: **[nao verificado]**]
- Isso preserva a **propriedade** do RED, mas edita sua fixture. A promessa “SEM ser editado” e a ordem de migrar fixtures são incompatíveis. [contrato_item3_v3.txt:1, :8, anexo]
- Há exemplo verificável dessa migração necessária no selo vizinho: ele só escreve BACKLOG e alterna coluna entre livre/travada, depois exige bloquear/liberar. Sem fornecer `.ids`, não descreve a fonte do v3. [bin/tests/test_hook_parei_com_andavel.sh:25, :27, :30, :41, :46, :56, :57]

**Cuidado adicional:** topo com TRAVA é permitido no v3, mas o veto atual de PAREI rejeita qualquer ID de `_ids_que_nao_andam()` antes de comparar com o topo. Migrar essa lista mecanicamente para “IDs com TRAVA” recusaria o PAREI do próprio topo. O contrato manda migrar ambas funções, portanto é implementável, mas precisa distinguir pertencimento, trava e topo; o novo cenário A/B/C mantém A sem TRAVA e não guarda esse caso. [contrato_item3_v3.txt:3, :5, :8, anexo; bin/hook_stop_fila1.py:1298, :1302, :1310, :1313]

**Falha residual:** o v3 não enumera ilegibilidade ou erro de decodificação como `ErroFonte`; sem conversão explícita, alcançam a falha aberta existente. [contrato_item3_v3.txt:7, anexo; bin/hook_stop_fila1.py:1460, :1461]

## 12/11 14:01 · sessao 2b6ccd8a2d08 · DECLARADO-X-GRAVADO · sonda · origem=Code

Referências `saída:N` abaixo numeram somente as linhas iniciadas por `S` do dado fornecido, começando em `S UNIVERSO 6729`; a última é `saída:107`. São referências à transcrição, não a um arquivo criado.

**Não há bloqueio por erro de execução:** `TOTAL_ERROS TOTAL = 0`; todas as etapas informadas também têm zero. Não há exemplo de erro apresentado. As contagens podem ser publicadas como resultados da sonda, preservando as limitações abaixo (`saída:95-107`). [nao verificado]

Item 2 — desfechos das perguntas respondidas:

| Desfecho | Quantidade | Fonte |
|---|---:|---|
| Batida nova | 886 | saída:2 |
| Reuso | 23 | saída:3 |
| Recusa de paridade | 230 | saída:4 |
| Pendente do admin | 121 | saída:5 |
| Não validada | 1210 | saída:6 |
| OUTRO | 4259 | saída:7 |
| **Soma dos desfechos** | **6729** | saída:8 |
| **Universo** | **6729** | saída:1 |
| Indicador soma igual ao universo | 1 | saída:9 |

`OUTRO` está integralmente explicado pela categoria `validada_sem_relacao_nem_recusa`: **4259**. Não cabe redistribuí-la entre os demais desfechos (`saída:10`).

Item 3 — diferença entre instante gravado e resposta com data, preservando as categorias emitidas:

| Medida | `intervalo_saida` | `intervalo_volta` |
|---|---:|---:|
| Novas por origem | 437 — saída:18 | 449 — saída:38 |
| Até 5 minutos | 260 — saída:19 | 283 — saída:39 |
| Mais de 5 até 30 minutos | 24 — saída:20 | 43 — saída:40 |
| Acima de 30 minutos | 137 — saída:21 | 108 — saída:41 |
| Delta não verificado (`delta_nv`) | 16 — saída:22 | 15 — saída:42 |

As faixas, incluindo `delta_nv`, fecham os respectivos totais de novas por origem (`saída:18-22`; `saída:38-42`).

| Datas e sinal do delta | `intervalo_saida` | `intervalo_volta` |
|---|---:|---:|
| `resposta_dmais1` | 44 — saída:23 | 92 — saída:43 |
| Gravada fora do dia do turno | 48 — saída:24 | 93 — saída:44 |
| Datas diferentes | 10 — saída:25 | 5 — saída:45 |
| Delta assinado negativo | 87 — saída:26 | 61 — saída:46 |
| Delta assinado positivo | 83 — saída:27 | 127 — saída:47 |
| Delta zero | 251 — saída:28 | 246 — saída:48 |

A convenção da subtração usada no delta assinado é **[nao verificado]**; os sinais acima não foram traduzidos como “adiantamento” ou “atraso”.

| Proximidade do marco oposto | `intervalo_saida` | `intervalo_volta` |
|---|---:|---:|
| Medidos | 420 — saída:29 | 431 — saída:49 |
| `oposto_ate5` | 35 — saída:30 | 52 — saída:50 |
| `oposto_ate30` | 55 — saída:31 | 86 — saída:51 |
| Não verificados (`oposto_nv`) | 17 — saída:32 | 18 — saída:52 |

A saída fornece proximidade por tolerância; quantidade de coincidências exatas com o marco oposto **[nao verificado]**. A vigência histórica do marco também exige a ressalva de `dna_vigencia_nao_confirmada` (`saída:30-32`; `saída:50-52`; `saída:91`).

Item 4 — **TRIAGEM** dos casos acima de 30 minutos:

| Resultado emitido | `intervalo_saida` | `intervalo_volta` |
|---|---:|---:|
| Resposta mais próxima | 49 — saída:33 | 26 — saída:53 |
| Gravada mais próxima | 9 — saída:34 | 2 — saída:54 |
| Empate | 0 — saída:35 | 0 — saída:55 |
| Sem indício | 79 — saída:36 | 80 — saída:56 |
| Não verificado (`triagem_nv`) | 0 — saída:37 | 0 — saída:57 |

A triagem fecha nos respectivos totais acima de 30 minutos; ela permanece indício, conforme o item 4 do aval, sem determinar qual horário foi o efetivamente trabalhado (`saída:21,33-37`; `saída:41,53-57`; `aval_declarado_x_gravado_v2.txt:7`, contando as linhas do anexo transcrito).

Item 5 — regeneração da célula depois da resposta:

| Categoria emitida | Quantidade | Fonte |
|---|---:|---|
| Depois da resposta | **403** | saída:11 |
| Não depois da resposta | 5984 | saída:12 |
| Sem data | 0 | saída:13 |
| Sem célula | 342 | saída:14 |
| Erro de célula | 0 | saída:15 |
| Erro | 0 | saída:16 |
| Não processadas | 0 | saída:17 |

Essas categorias fecham o universo. “Sem célula” permanece separado de “não depois da resposta” (`saída:1,11-17`).

Item 6 — carga semanal ISO, com os três campos numéricos na ordem exata da saída:

| Semana ISO | Primeiro campo | Segundo campo | Terceiro campo | Fonte |
|---|---:|---:|---:|---|
| 2026-W39 | 280 | 44 | 5 | saída:58 |
| 2026-W40 | 148 | 13 | 6 | saída:59 |
| 2026-W41 | 147 | 34 | 0 | saída:60 |
| 2026-W42 | 221 | 28 | 1 | saída:61 |
| 2026-W43 | 160 | 30 | 7 | saída:62 |
| 2026-W44 | 180 | 37 | 0 | saída:63 |
| 2026-W45 | 81 | 15 | 3 | saída:64 |
| 2026-W46 | 93 | 6 | 0 | saída:65 |

**Os cabeçalhos desses três campos são [nao verificado].** As linhas fornecidas não os identificam; portanto, a associação de cada coluna a “validações”, “acima de 30 minutos” ou outra medida não pode ser publicada como confirmada (`saída:58-65`). O recorte temporal efetivamente aplicado pela consulta é **[nao verificado]**. [nao verificado]

| Colaborador | Contagem emitida | Fonte |
|---|---:|---|
| col569 | 32 | saída:66 |
| col723 | 22 | saída:67 |
| col926 | 14 | saída:68 |
| col438 | 11 | saída:69 |
| col696 | 9 | saída:70 |
| col993 | 8 | saída:71 |
| col518 | 7 | saída:72 |
| col055 | 6 | saída:73 |
| col502 | 5 | saída:74 |
| col909 | 5 | saída:75 |

| Escala modelo | Contagem emitida | Fonte |
|---|---:|---|
| #431 | 32 | saída:76 |
| #276 | 22 | saída:77 |
| #178 | 15 | saída:78 |
| #496 | 14 | saída:79 |
| #197 | 11 | saída:80 |

| Vínculo | Contagem emitida | Fonte |
|---|---:|---|
| #484 | 32 | saída:81 |
| #924 | 22 | saída:82 |
| #1181 | 14 | saída:83 |
| #376 | 11 | saída:84 |
| #970 | 9 | saída:85 |

O filtro exato usado para selecionar os casos desses rankings é **[nao verificado]**; as tabelas preservam as contagens e identificadores emitidos (`saída:66-85`).

Limitações e sobreposições declaradas pela própria saída:

| Aviso | Quantidade e fonte | O que impede afirmar |
|---|---|---|
| `criacao_por_esta_pergunta_nv` | 886 — saída:86 | Que cada batida classificada como nova foi criada pela própria pergunta examinada. |
| `dna_anterior` | 24 — saída:87 | São casos sinalizados com DNA anterior; o rótulo sozinho não comprova sua vigência no instante do plantio. |
| `dna_atual` | 831 — saída:88 | São casos sinalizados com DNA atual; isso sozinho não comprova que ele era o vigente no plantio. |
| `dna_dna_sem_marcos` | 5 — saída:89 | Concluir a comparação com o marco nesses casos. |
| `dna_historico_multiplas_regeneracoes` | 13 — saída:90 | Reconstruir toda a sequência histórica apenas com o DNA anterior disponível. |
| `dna_vigencia_nao_confirmada` | 13 — saída:91 | Afirmar atendimento integral ao requisito de usar o marco vigente no plantio. |
| `origem_disputa_preexiste_resposta_atual` | 3 — saída:92 | Atribuir a criação dessa batida à resposta atual. |
| `pendencia_admin_com_batida` | 14 — saída:93 | Tratar pendência administrativa como sinônimo de ausência de batida. |
| `relacao_com_batida_retratada` | 5 — saída:94 | Tratar toda relação encontrada como batida atualmente apurável. |

O código confirma que `dna_anterior` guarda somente a versão imediatamente anterior, enquanto o histórico completo fica fora desse campo; `regenerada_em` registra a última regeneração. Portanto, o item de regeneração e a comparação histórica não demonstram, por si, todas as versões passadas ([models.py](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/escala/models.py:1714), [models.py](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/escala/models.py:1718)).

Esses avisos não constituem uma partição demonstrada: suas interseções são **[nao verificado]**. Zero erros de execução não resolve as limitações declaradas (`saída:86-107`).

## 12/11 14:01 · sessao e74da5cb4d3c · - · - · origem=Ronald (tmux)

Parte A — provas locais para consolidação (todos os caminhos abaixo relativos à raiz da cópia).

A lei vigente está em `app/docs/CORTES.md:348`, complementada por O44 em `app/docs/CORTES.md:349`; data da flag sem trilha e item sem dia estão decididos em `app/docs/CORTES.md:352`. A reprovação e as sete curas constam literalmente em `app/docs/BACKLOG.md:765`. As alternativas de fechar chamados / impedir nascimento foram substituídas: `app/docs/CORTES.md:345`, `app/docs/CORTES.md:346`, `app/docs/CORTES.md:347`; a substituição do item do emissor também consta em `app/docs/BACKLOG.md:766`.

A1 — quem decide hoje

| Leitor/estado | Pergunta, universo e escritor |
|---|---|
| `isento_ponto` | Responde **flag atual**, não “isento nesta data”. Retorna bool de `Colaborador.isento_ponto`, sem cargo nem período: `app/core/constants.py:13-18`. Campo booleano sem data: `app/colaboradores/models.py:294`. Tela Passe Livre escreve pela porta `gravar(...,'cadastro',...)` em `app/colaboradores/views.py:173-176`, tenta trilha com antes/depois/motivo em `:183-186`; falha da trilha não desfaz alteração, `:187-202`. Semente histórica escreve flag sem trilha em `app/colaboradores/migrations/0043_isento_ponto.py:13-14`. |
| `silencio_isentos.alvos` | “Quais chamados vivos de flag isento ainda não têm **nenhum** `silenciado_ate`?” Filtra flag diretamente e todos VIVOS; não restringe módulo, dia ou janela: `app/chamados/services/silencio_isentos.py:20-31`. `silenciar` põe prazo padrão pela porta `arquivo.silenciar`: `:34-49`. Ignora resgate por política declarada: `:5-6`. |
| Gatilho da flag e do chamado | Todo save de colab com flag true agenda `silenciar_do_colab`, sem detectar transição false→true: `app/chamados/signals.py:269-281`. Chamado nascido/voltando a VIVOS ou com véu levantado agenda a mesma porta se `silenciado_ate is None`: `:285-308`. Portanto levantar véu de isento pode chamar o ressequenciamento que o põe de novo: `:290-301`. |
| Cron isentos | Modo `--alarme` é leitura e retorna 2 se houver alvos; `--apply` continua autorizado pelo próprio command a escrever véus: `app/chamados/management/commands/silenciar_chamados_isentos.py:25-49`. Seu universo atual é `alvos()`, sem janela: `:36`. |
| `silenciado_em` / `q_silenciado_em` | “O véu do **chamado** vale neste instante?” Ambos usam prazo futuro: `app/chamados/models.py:508-524`. O escritor é `arquivo.silenciar` (`app/chamados/services/arquivo.py:141-174`) ou `arquivo.levantar` (`:177-212`), não um estado de quarentena. |
| `qs_na_fila` | “Quais chamados cobram agora?” VIVOS → COBRANÇA → exclui silêncio futuro: `app/chamados/services/painel.py:188-190`; VIVOS definido pela consulta em `:153-155`. Não pergunta isenção nem período. |
| `chamados_visiveis_colab` | Lista de trabalho da pessoa: competência corrente por empresa (`app/chamados/services/janela_colab.py:40-46`), ampliada por perguntas `q_vivas` com destino (`:65-73`), exclui silêncio futuro e módulos internos (`:74-79`). Não filtra status vivo. `desligamento_revisao` já é interno à lista do colab: `:30-37`. |
| Contadores/pílulas | Ativos e abertos/em análise usam `qs_na_fila`: `app/chamados/services/painel.py:212-225`. Pílula calcula `particionar_verbos(qs_vivos(...))`: `:229-231`. **Divergência ainda presente**: qualquer `silenciado_ate` vencido vira HISTORICO, `app/chamados/services/verbos.py:48-51`; lista padrão exclui ARQUIVADO e HISTORICO, `app/chamados/services/painel.py:325-331`. Contraria retorno exigido em `app/docs/CORTES.md:335`. |
| Badge admin | Prefere `PlacarNavbar.badge_chamados`, depois `exige_o_admin(particionar_verbos(qs_vivos()))`, depois fila: `app/core/context_processors.py:80-93`. Badge “registrados” lê lavrado ou fila: `:138-150`. Badge colab lê fila: `:185-187`. Escritor do lavrado: `app/core/management/commands/lavrar_badge_navbar.py:91-93`; origem painel: `:39-42`, exigência admin: `:72-74`. Logo só migrar fila deixa caminho da pílula/badge admin descoberto. |
| Alerta SLA | Universo de cobrança lido via fila: `app/chamados/management/commands/alertar_chamados_sla.py:24-32`. |
| Alerta avançado | Isento atual é descartado pelo juiz `isento_ponto`, `app/ponto/management/commands/processar_alertas_avancados.py:29-33`; N5 também descarta chamado com silêncio vigente pelo método, `:110-113`. O primeiro é flag global, o segundo véu do chamado; nenhum pergunta período. |
| Disparo pela célula | `motivos_a_perguntar` usa dia sem trabalho, situação ativo, tempo, veredito e lista admin: `app/chamados/services/fabrica_da_celula.py:105-125`; `perguntar_pela_celula` chama `perguntar` com dia da célula, `:217-220`. `perguntar` chama `gerar_perguntas_disputa` em `:160-174`; não há consulta ao novo juiz. |
| Disparo por disputa/agrupador | `gerar_perguntas_disputa` (`app/chamados/services/disputa_emissao.py:300`) abre conversa ou pergunta datada em `:439-451`. Agrupador `_criar_pergunta_se_couber` guarda ausência e duplicata e abre em `:823-847`; âncora abre conversa em `:1371`. |
| `detectar_par_relampago` | Na cópia linhas antigas deslocaram: universo é escala ativa sem fim + colab ativo, `app/ponto/management/commands/detectar_par_relampago.py:336-338`; abre **fato chamado** em `:368-385`, sem consulta a isento/quarentena. Pela lei vigente emissão de fato continua; isolar cobrança/disparo, não reintroduzir a regra substituída de “não nasce”: `app/docs/BACKLOG.md:766`, `app/docs/CORTES.md:348`. |

A2 — matriz exigida de pré-desenho (não há implementação desta entidade/juiz/porta localizada; status de implementação das sete curas nesta cópia: **[nao verificado]**, a linha da obra é reprovada em `app/docs/BACKLOG.md:765`).

| Estado → juiz | Operação | Persistência/retorno a exigir | Prova do contrato |
|---|---|---|---|
| Sem janela → falso; dia coberto → verdadeiro | Abrir período manual | Entidade com colab/início/fim/motivo/quem/quando e uma porta com trilha; não fechar chamados nem pôr véu. Retorno real/API inexistente **[nao verificado]**. | `app/docs/CORTES.md:348` |
| Janela de isento aberta → verdadeira só no período | **Fechar**, não levantar, ao desligar flag | Preservar passado do período e registrar fim; distinguir do levantamento administrativo que devolve cobrança. A distinção é cura exigida, assinatura concreta e bordas inclusivas **[nao verificado]**. | `app/docs/BACKLOG.md:765`; flag atual só bool em `app/core/constants.py:18` |
| Quarentena vigente → verdadeiro | Levantar | Quem/quando/motivo + trilha; retorno automático dos itens, sem mutar chamados/cálculo. | `app/docs/CORTES.md:348` |
| Dia isolado → falso **só para aquele dia** | Desarquivar `(colab,dia)` por toque admin | Exceção persistida/trilhada para o par; restante da janela segue isolado. Não chamar `arquivo.levantar` de todos os chamados. | `app/docs/CORTES.md:349`; porta atual levanta um chamado em `app/chamados/services/arquivo.py:177-212` |
| Flag true → janela de isento | Porta separada da janela de isento | Efeito da troca de flag passa pela porta; impedir que alteração manual de período vire segundo escritor da semântica da flag. Detalhe técnico da porta **[nao verificado]**. | `app/docs/BACKLOG.md:765`; trocas com trilha/abre-fecha em `app/docs/CORTES.md:352`; escritor atual `app/colaboradores/views.py:176` |
| Histórico de flag → início da janela | Migration reversível e congelada; última ligação | Selecionar **última ligação** relevante do histórico, não primeira flag true; sem trilha usar admissão e rótulo explícito. Congelar interpretação para não importar juiz runtime mutável e guardar reversão que remova só o que migration criou. Implementação/migration específica **[nao verificado]**. | sete curas `app/docs/BACKLOG.md:765`; fallback autorizado `app/docs/CORTES.md:352`; seed histórica sem trilha `app/colaboradores/migrations/0043_isento_ponto.py:13-14` |
| Chamado de isento com dia coberto pelo juiz | Vigia só dentro da janela | Esperado 0 para cobrança que escapou **dentro** da janela; chamado anterior ao início não pode ser chamado vazamento por flag atual. Não escrever nem re-silenciar. | `app/docs/BACKLOG.md:765`; universo atual demasiado amplo `app/chamados/services/silencio_isentos.py:24-31` |
| Pergunta viva ou retida em item isolado | RED da retenção | Cobrir que retenção não devolve cobrança pela lateral, e que levantar/desarquivar restaura conforme lei. Qual `retencao` a reprovação nomeou especificamente é **[nao verificado]**: há `pergunta_no_app.retencao`, `app/chamados/services/pergunta_no_app.py:121-153`, e retenção de retratação, `app/chamados/reconciliador.py:438-455`. Não afirmar que uma delas é a nomeada sem anexo adicional. | `app/docs/BACKLOG.md:765` |
| Candidatos migration → antes/depois | DIFF pela **função real da migration** | Executar seleção/projeção pura da função congelada em modo leitura e comparar itens/colabs/perguntas; não reconstruir seleção paralela. Função concreta não localizada **[nao verificado]**. | `app/docs/BACKLOG.md:765`; grandezas do DIFF `app/docs/CORTES.md:348` |
| Item sem dia | Perguntar o mesmo juiz | Janela aberta cobre item sem dia; fechada não cobre. | `app/docs/CORTES.md:352` |
| `desligamento_revisao` | Fora da quarentena | Sem isolar ato de DP por esta obra; não criar juiz extra de dono. | `app/docs/CORTES.md:348` |

A3 — arquivos e sobreposição

Núcleo necessariamente toca `app/chamados/models.py` (entidade histórica; estado atual só véu em `:508-524`), `app/chamados/juizes.py` (corte autoriza **apenas** `colab_em_quarentena`, `app/docs/CORTES.md:348`), `app/core/juizes.py` (registro previsto na política local; nova entrada **[nao verificado]**), migration nova (nome **[nao verificado]**), porta única nova (nome **[nao verificado]**), `app/chamados/views_arquivar.py`/template (modo/lista/botão exigidos por `app/docs/CORTES.md:348`), `app/colaboradores/views.py:176` e `app/chamados/signals.py:269-308` (troca de flag e vassoura atual). Preservar espelho/folha/TXT e não trocar `isento_ponto` de cálculo por novo significado de cobrança: `app/docs/CORTES.md:348`.

Leitores diretos a migrar: fila `app/chamados/services/painel.py:188-190`; pílula `app/chamados/services/verbos.py:48-51`; lista do colab `app/chamados/services/janela_colab.py:74-79`; alerta avançado `app/ponto/management/commands/processar_alertas_avancados.py:110-113`; fábrica `app/chamados/services/fabrica_da_celula.py:92`, `:160`, `:178`, `:234`; gerador e agrupador `app/chamados/services/disputa_emissao.py:300`, `:823`, `:1302`; vigia e universo fio mudo `app/chamados/management/commands/disparar_perguntas_competencia.py:99-120`; vassoura `app/chamados/services/silencio_isentos.py:20-31` e seu command `app/chamados/management/commands/silenciar_chamados_isentos.py:31-49`. Caminhos indiretos/badge precisam validação após migração porque preferem valor lavrado: `app/core/context_processors.py:80-93`, `:138-150`; lavrador `app/core/management/commands/lavrar_badge_navbar.py:39-42`, `:91-93`.

Outros disparadores a conferir sem trancar declaração/ação voluntária: `app/chamados/services/acoes_disputa.py:263` abre conversa; `app/chamados/services/declaracao_texto.py:376`, `:666` abre pergunta no ato de declaração. A porta genérica `PerguntaDisputa.abrir` atende ambos, `app/chamados/models.py:2792-2807`; pôr veto universal nela arriscaria contrariar “nada tranca”, `app/docs/CORTES.md:349`. O censo deve distinguir disparo de cobrança e ato voluntário por chamador, sem juiz novo.

Crons chamadores do agrupador: `app/ponto/management/commands/detectar_ausencias.py:803`, `app/ponto/management/commands/processar_alertas_turno.py:385`, `app/ponto/management/commands/processar_alertas_avancados.py:159`; célula/cartório chama fábrica em `app/ponto/services/cartorio.py:1045`. `detectar_par_relampago.py:368` continua emissão de fato, não chamar isso de disparador direto de PerguntaDisputa sem prova.

Sobreposição E3: usar o aval recebido como fonte do diff, pois comparação main não está disponível. Ele nomeia `chamados/juizes.py` alimentar_portas e `chamados/models.py` q_silenciado_em/respostas_sem_veredito; fonte da cópia confirma localizações atuais em `app/chamados/models.py:515`, `:1836` e `app/chamados/juizes.py:1541` em diante. Sobreposição SAÍDA-NOTURNA nos mesmos **arquivos**, outras regiões dadas pelo usuário: `juizes.py:679-705/:941-971`, `models.py:2511/:3179/:3738`; conteúdo do diff SAÍDA-NOTURNA **[nao verificado]**.

Observação para C2: `app/docs/CORTES.md:345` lista os oito históricos nominalizados por #pk: `3403,3057,11935,23036,11658,23049,23037,23030`. Esses não são lista completa dos 12 nem medição atual. Só o dado/snapshot sustenta “8+4” à época; resultado atual permanece **[nao verificado]**.

## 12/11 14:00 · sessao 667377a17498 · - · - · origem=Ronald (tmux)

B1 — Varri **3.566 arquivos `.py` por AST**, sem erro de parse; procurei Store de atributo, keyword `respondida_em`, dict/subscript, `save(update_fields)`, `bulk_update`, `F()` e `setattr`, e complementei com fluxo de `**campos`/`**outros` e o censo estrutural existente. O censo existente reconhece aliases/imports e gerentes reversos, mas declara limites de atribuição em `app/core/censo_escritas.py:43`, `:451`, `:469`. Resultado da execução só em memória: 10 sítios literais runtime, 317 sítios em testes (160 arquivos); o total bruto inclui atribuições em memória e descritores de campo, portanto **não equivale a 10 escritores persistentes**. A execução do censo não tem arquivo de resultado persistido: `[nao verificado]` por arquivo:linha.

| Escritor / fluxo | Pode persistir timestamp sem texto? | Ramo e prova |
|---|---|---|
| `responder_pergunta_disputa_core` | Não com entrada vazia no fluxo normal | `resposta` é normalizada e vazia retorna antes (`app/chamados/services/disputa_emissao.py:1432`); texto e timestamp atribuídos juntos `:1467`, `:1468`; campos `:1469`, save `:1508`; registra ato de resposta em seguida `:1511`. |
| `responder_automaticamente` | **A função aceita** `texto=''`/espaços; grava ambos vazios e ato vazio se `instante` não nulo | Guarda apenas texto **anterior** `app/chamados/services/disputa_emissao.py:1647`; grava `texto` recebido sem validar `:1652`, timestamp `:1653`, save `:1655`, `ato_texto=texto` `:1656`. Único chamador runtime encontrado passa constante não vazia: `app/api/foto_recebida.py:25`, `:48`. Escrita vazia efetiva em produção: `[nao verificado]`. |
| `carimbar(...,resposta='responder'/'validar')` | Pode escrever timestamp sem **reescrever** texto, mas só com texto preexistente não vazio | Guarda real `texto_vazio` em `app/chamados/models.py:3146`, timestamp só quando nulo `:3147`, `:3148`, save `:3168`; não escreve `ato_texto` aqui. Ausência semântica de texto não é permitida por esse ramo. |
| `carimbar_resposta_pela_trilha` | Pode gravar só o timestamp no modelo, mas exige texto preexistente e registra-o no ato | Recusa carimbo existente ou texto vazio em `app/chamados/services/disputa_emissao.py:1670`; atribui `:1674`; save somente `respondida_em` `:1676`; `ato_texto=p.resposta_colab` `:1678`. |
| `reabrir_para_correcao` | Só grava `None` no timestamp nos usos atuais | Dicionário com `respondida_em=None` e `resposta_colab=''` em `app/chamados/services/disputa_emissao.py:1593`; preserva foto anterior `:1615`; `descarimbar_em_lote(**campos)` `:1630` ou `qs.update(**campos)` `:1632`. |
| `ressuscitar_perguntas` | Só grava `None` nos usos atuais; não cria timestamp não nulo | Universo restrito a texto vazio/null em `app/chamados/models.py:1499`; chamada persistente com `respondida_em=None` em `:1579`. |
| `descarimbar_em_lote(qs,**outros)` | **Porta genericamente aceita** timestamp arbitrário sem texto | Só proíbe chaves de carimbo, não o par de resposta: `app/chamados/models.py:3271`; `update(...,**outros)` em `:3280`–`:3282`. Chamadores encontrados passam apenas `None`: `models.py:1579`, `disputa_emissao.py:1593`, `:1630`. Uso não nulo atual: `[nao verificado]`. |
| Nascimento `abrir` / `abrir_conversa` → `_sob_contrato` → `_nascer` | **Porta genericamente aceita** `respondida_em` em `**campos` sem texto | `app/chamados/models.py:2792`, `:2810`, `:2822`, `:2841`; constrói `cls(...,**campos)` `:3030`–`:3031`, insere `:3033`; `save` não valida o par `:3429`–`:3439`. A conservação da projeção gera ato até com texto vazio em `:3947`–`:3950`. Não localizei chamador runtime que passe explicitamente assinatura não nula no nascimento; origem real em produção `[nao verificado]`. |

Não são escritores persistentes:

- `_universo_da_ressuscitacao` atribui `None` numa **cópia** para consulta ao juiz: `app/chamados/models.py:1507`, `:1521`, `:1522`.
- `_desfazer_em_memoria` restaura o valor antigo sem save: `app/chamados/models.py:3175`, `:3179`; usado nos ramos de recusa/exceção `:3157`, `:3160`, `:3171`.
- `DecisaoPergunta.linhas_da_projecao_sem_linha` **lê** `respondida_em` e pode copiar legado sem texto para `ato_texto=''`, não escreve `PerguntaDisputa.respondida_em`: `app/chamados/models.py:3947`–`:3950`.
- O `bulk_update` de PerguntaDisputa encontrado grava apenas os três campos da lavra, não `respondida_em`: `app/chamados/veredito_lavrado.py:237`, `:250`.
- `setattr` genérico de reexibição só muta memória, sem save: `app/core/reexibir.py:21`, `:49`, `:50`.
- Migration só declara o campo, não contém backfill dele: `app/chamados/migrations/0014_perguntadisputa.py:25`.
- Não encontrei `F('respondida_em')`, `setattr(...,'respondida_em',...)`, `bulk_update(...,'respondida_em')` nem update runtime direto não nulo; a ausência resulta da varredura em memória e não tem artefato persistido `[nao verificado]`.

A suíte antiga não certifica a simetria pedida: o contrato declara literalmente o inverso “inofensivo”, só exige `respondida_em` quando encontra escrita de `resposta_colab`, e só inspeciona atribuição/update no corpo: `app/chamados/tests/test_contract_par_resposta.py:28`–`:37`, `:69`–`:83`; mutante absolve “só carimbo” em `:109`, `:113`. Isso conflita com o corte novo do anexo e não prova a saúde do sentido timestamp→texto.

B2 — Leitores diretos runtime de `respostas_sem_veredito`:

| Arquivo:linha | Uso |
|---|---|
| `app/chamados/models.py:1869` | Parte do juiz `pode_fechar`. |
| `app/chamados/models.py:1902` | Guarda de **todas** as portas de `fechar`, inclusive exceções. |
| `app/chamados/reconciliador.py:1164` | Segura disputa e relata perguntas no fecho das órfãs pela lâmpada. |
| `app/chamados/services/fila_por_causa.py:226` | Gaveta `respondido_a_decidir`. |
| `app/chamados/services/competencia_trancada.py:114` | `ignorar_tranca=True` para pauta do DP. |
| `app/colaboradores/services/ficha.py:169` | Conta respostas sem veredito restritas a perguntas vivas da ficha; saída `:246`. |
| `app/colaboradores/services/desligamento.py:141` | Pula fechamento automático da disputa na cascata de desligamento. |
| `app/chamados/management/commands/validar_classe_a.py:54` | Compõe conjunto para classificação/validação assinada. |

Corpo atual divergente: validação/materialização nulas e `(respondida_em não nulo OU resposta_colab.strip())`, excetuando tranca salvo parâmetro: `app/chamados/models.py:1857`–`:1861`. `pode_fechar` combina essa lista com `fio_vivo` real: `:1867`–`:1869`.

Leitores diretos runtime de `pode_fechar`:

- `app/chamados/reconciliador.py:412`, `:612`, `:1171`.
- `app/chamados/services/becos.py:33` (classificação), `:56` (fecho).
- `app/chamados/services/competencia_trancada.py:154` (medição), `:203` (aplicação).
- `app/chamados/services/explicador.py:139` (retorna booleano na explicação em `:165`).
- `app/chamados/services/disputa_emissao.py:1278`, `:2091`.
- `app/chamados/services/materializacao.py:591`, `:1322`.
- `app/ponto/services/entrada_sincronizada.py:59`.

Leitores indiretos da saída do explicador: `app/chamados/services/desenlace.py:63`; template `app/templates/chamados/partials/modal_fio.html:435`, `:446`. Registro de autoridade, não chamada: `app/core/juizes.py:283`.

Não há banco/log disponível nesta tarefa; nenhum escritor histórico responsável por perguntas reais pode ser identificado só pela possibilidade estática `[nao verificado]`.

_4283 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
