# CODEX-ESTADO

> Ultima resposta processada: **11/11/2026 11:54** · publicado em **11/11/2026 11:54** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 11/11 11:54 · sessao e0331546fc34 · UI-PAINEL-MOSTRA-O-APP · construtor · origem=Code

A cura permanece pendente: a captura de `setUpClass` pelo `freeze_time` herdado é hipótese **[nao verificado]**. Preservei a fixture e o assert, conforme exigido ([investigação](/home/ronald/wt-codex-ui-painel-mostra-o-app-100626/app/docs/UI-PAINEL-MOSTRA-O-APP-v2.txt:27)).

Os 7 testes em memória passaram; os quatro mutantes isolados falharam por asserção. Comandos e resultados estão no [recibo](/home/ronald/wt-codex-ui-painel-mostra-o-app-100626/app/docs/UI-PAINEL-MOSTRA-O-APP-v2.txt:56). [nao verificado]

Falta o caminho de um Python com Django/Freezegun para comprovar a causa ([pendência](/home/ronald/wt-codex-ui-painel-mostra-o-app-100626/app/docs/UI-PAINEL-MOSTRA-O-APP-v2.txt:120)). Não fiz commit pela proibição explícita nas REGRAS ([registro](/home/ronald/wt-codex-ui-painel-mostra-o-app-100626/app/docs/UI-PAINEL-MOSTRA-O-APP-v2.txt:3)). [nao verificado]

## 11/11 11:53 · sessao 558816c733bd · - · - · origem=Ronald (tmux)

O campo é `ChamadoColaborador.status_local`, com sete escolhas em `app/chamados/models.py:28` e declaração em `app/chamados/models.py:52`. O catálogo compara códigos (`aberto`, `resolvido`, `fechado` etc.), definidos em `app/chamados/catalogo/motor.py:19`, com conjuntos em `:24`, `:29`, `:30`, `:31`, `:32` e decisões em `:125`.

Escritores persistidos e temporários:

- Transição: `_gravar_estado`, atribuição em `app/chamados/models.py:228`, trilha em `:292`, persistência em `:296`.
- Gates que chegam ao escritor: `_aplicar` `app/chamados/models.py:450`; `reconciliar` `:654`; `cancelar` `:788`; `analisar` `:806`; `reabrir` `:1442`; `encerrar` `:1619`; `encerrar_por_competencia_trancada` `:1639`.
- Nascimento por `abrir`: prepara estado em `app/chamados/models.py:938`, instancia provisória em `:961`, `get_or_create` em `:971`.
- Nascimento por `criar`: resolve estado em `app/chamados/models.py:1060`, instancia provisória em `:1070`, persiste em `:1095`.
- Posestado só em memória: `app/chamados/models.py:356`, restaurado no `finally` em `:371`. O selo declara explicitamente essa exceção em `app/chamados/tests/test_contract_status_nucleo.py:10`.
- Chamadores humanos de `transicionar`: `app/chamados/services/acoes_chamado.py:70,108,149,222,233,277,315`; `app/chamados/services/fio.py:721`; `app/colaboradores/views.py:2068`. Smoke operacional: `app/chamados/management/commands/smoke_chamados.py:134,147,163`.
- Escritores históricos de dados, separados de runtime: `app/chamados/migrations/0038_a3_registrado_backfill.py:10,15`; `app/chamados/migrations/0039_a3_via_backfill.py:24,33`.

Não encontrei comparação de **decisão de estado** pelos textos humanos na superfície pesquisada; decisões demonstradas usam códigos: `app/chamados/catalogo/motor.py:136,140,148,150,153`; validação de `dict(STATUS_CHOICES)` por chave em `app/chamados/services/fio.py:719` e `app/chamados/services/acoes_chamado.py:313`. O JS compara `.value` e `dataset.status` em `app/templates/chamados/painel_gestao.html:376,382`, produzido em `app/templates/chamados/partials/_lista_chamados.html:33`. Consumidores externos da API: [não verificado].

Há busca/ordenação genérica pelo texto, sem decidir estado: `app/static/js/hasner-table.js:191,194,209,235,236`; a coluna local fornece código/verbo em `app/templates/chamados/partials/_lista_chamados.html:79`. Isso deve ser distinguido de comparar `"Encerrado"` para autorizar uma transição. [referências acima]

Inventário compacto de leituras explícitas de `status_local` em Python de produção, incluindo atributos, `getattr`, filtros ORM e chaves de dicionários/trilha; testes, migrations e documentação foram separados. Nas linhas do próprio escritor, também aparecem leituras do estado anterior: [nao verificado]

```text
app/ponto/worklist.py:40
app/ponto/views.py:284,395,1616
app/ponto/signals.py:303
app/relatorios/views.py:745,920
app/relatorios/pdf_chamados.py:116
app/escala/utils.py:1059
app/api/views_core.py:309
app/api/views.py:320,1044,1142,1146,1152,1972,1982,3127
app/api/views_mensageria.py:417,1082
app/chamados/reconciliador.py:246,839,936,938,997,1204,1454,1587
app/chamados/views_cobrar.py:76
app/chamados/rejuncao.py:61,79
app/chamados/models.py:109,112,124,165,207,219,229,294,355,417,427,673,712,713,714,736,760,768,783,803,822,830,921,927,938,1070,1095,1219,1220,1221,1337,1342,1348,1358,1373,1374,1375,1398,1404,1440,1614,1630,1632,2683
app/chamados/juizes.py:780,858,2017,2162,2201
app/chamados/views.py:976,1477
app/chamados/views_arquivar.py:121
app/chamados/signals.py:33,70,86,87,96,184,192,201,285,293,354,358
app/colaboradores/services/calendario.py:437,441,442,444
app/colaboradores/services/pos_vinculo.py:36
app/colaboradores/services/drawer.py:89
app/colaboradores/services/vinculo.py:385
app/colaboradores/services/desligamento.py:152,155
app/colaboradores/services/ficha.py:163
app/colaboradores/management/commands/corrigir_escala_retroativa.py:141
app/ponto/services/faltas_de_hoje.py:64
app/ponto/services/cobertura.py:114,226
app/ponto/services/cartorio.py:473,1370,1629,1648
app/ponto/services/justificativa.py:43,61,77,118,121
app/ponto/services/entrada_sincronizada.py:49
app/ponto/services/furos_sem_cobranca.py:69,70,129
app/ponto/services/cluster_espurio.py:67,255,285,289,294
app/ponto/services/furos_vetados.py:95
app/ponto/services/vigia_de_hora.py:111
app/ponto/services/geofence.py:82,355
app/ponto/services/ausencia.py:709
app/ponto/services/veredito_celula.py:149,155
app/ponto/services/esmeril_espelho.py:182
app/ponto/services/triagem_batida.py:105,290,328
app/ponto/services/regularizacao.py:133
app/ponto/management/commands/supra_juiz.py:189,212
app/ponto/management/commands/escalonar_chamados_supervisao.py:45
app/ponto/management/commands/reavaliar_ausencias_lancadas.py:30,43
app/ponto/management/commands/processar_alertas_avancados.py:103,108,155
app/ponto/management/commands/reconciliar_grade.py:96,207,529
app/ponto/management/commands/reconciliar_fantasmas.py:159
app/ponto/management/commands/emitir_furo_retroativo.py:95
app/ponto/management/commands/reconciliar_vinculo.py:33,57
app/ponto/management/commands/detectar_ausencias.py:152,193,258,300,348,373,404,514,672,739,743,798
app/ponto/management/commands/emitir_turno_aberto_retroativo.py:119,123
app/ponto/management/commands/fechar_chamados_meio_periodo.py:56
app/ponto/management/commands/reconciliar_geofence.py:29,36
app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:42
app/ponto/management/commands/gerar_celulas.py:245
app/ponto/management/commands/reavaliar_ausencias_feriado.py:42
app/ponto/management/commands/processar_alertas_turno.py:165,179,381
app/ponto/management/commands/detectar_intervalo_ausente.py:125
app/ponto/management/commands/fechar_chamados_ausencia.py:28
app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:31
app/escala/services/regeneracao.py:170
app/escala/services/cadastro_realidade.py:494
app/escala/management/commands/fechar_avisos_regua_na_lista.py:34
app/chamados/services/vigia_loop.py:34,117,120
app/chamados/services/silencio_ausencia.py:60,125
app/chamados/services/na_fila.py:17
app/chamados/services/furo_sem_canal.py:83
app/chamados/services/vigia_fio.py:335,338,607,608,618
app/chamados/services/celula_carimbada.py:42,89,146
app/chamados/services/abertura.py:166
app/chamados/services/materializacao.py:778,784
app/chamados/services/silencio_isentos.py:29
app/chamados/services/resposta_tardia.py:35,57
app/chamados/services/pergunta_no_app.py:151,184
app/chamados/services/limbo_folgas.py:100,220,278,302,314
app/chamados/services/feriado_reavaliar.py:17
app/chamados/services/acoes_chamado.py:25,90,132,176,232,252
app/chamados/services/verbos.py:230
app/chamados/services/disputa_emissao.py:200,572,755,963,1109,1318,1344,1884,1910,1913
app/chamados/services/competencia_trancada.py:88,155,183,192,193,270
app/chamados/services/becos.py:22
app/chamados/services/painel.py:155,189,213,214,215,217,218,224,245,303,305,307,316,379,384,465
app/chamados/services/juizes_do_dia.py:92
app/chamados/services/acoes_disputa.py:269
app/chamados/services/fio.py:389,403,432,770
app/chamados/services/historico.py:156,180
app/chamados/services/adesao.py:89,102,143
app/chamados/services/fila.py:126
app/chamados/services/sla.py:37
app/chamados/management/commands/carimbar_por_batida_coincidente.py:61,68
app/chamados/management/commands/silenciar_chamados_isentos.py:41
app/chamados/management/commands/disparar_perguntas_competencia.py:99
app/chamados/management/commands/auditar_chamados_orfaos.py:36
app/chamados/management/commands/auditar_invariantes_chamados.py:573,589,602,616,637,653,683,807,978,1015,1041,1068,1070,1072,1339,1576,1577,1607,1628
app/chamados/management/commands/arquivar_prescritos.py:49
app/chamados/management/commands/reabrir_orfas_enterradas.py:98,134,170,171,173,174
app/chamados/management/commands/geofence_registrados_para_analise.py:22,36
app/chamados/management/commands/retratar_chamados_lei_cala.py:34
app/chamados/management/commands/reconciliar_chamados.py:59
app/chamados/management/commands/lavrar_perguntas_stale.py:57
app/chamados/management/commands/smoke_chamados.py:136,137,149,150
bin/snapshot_o118_col221.py:22
bin/r5_idempotencia_frota.py:61
bin/censo_folha.py:30
```

Leitores de template do campo bruto e/ou display, incluindo condicionais de cor e existência de ação:

```text
app/templates/chamados/fio_colaborador.html:167,188,189,191,193,254
app/templates/chamados/partials/painel_chamado_turno.html:18,19,21,23,65
app/templates/chamados/partials/modal_fio.html:76,102,103,105,107,500,544,559,588,636,651
app/templates/chamados/partials/_card_atendimento.html:13,19,20,21,23,203
app/templates/chamados/partials/_lista_chamados.html:33,37,38,40,41,79
app/templates/chamados/detalhe.html:36,37,39,41,117
app/templates/chamados/detalhe_local.html:68,69,71,73,153,176,244,279,343,375
app/templates/relatorios/historico_chamados.html:121
app/templates/ponto/partials/painel_justificativa.html:79
```

Leitores indiretos que recebem o código por argumento e geram rótulo: `app/chamados/services/verbos.py:125,138`; filtro intermediário em `app/chamados/templatetags/chamados_tags.py:54,64`. Os demais consumidores de rótulos estão na tarefa do outro agente. [referências citadas]

Selos do estado, úteis para demonstrar o limite da cura: escritor único em `app/chamados/tests/test_contract_status_nucleo.py:18` e asserção em `:33`; conjuntos canônicos em `app/chamados/tests/test_contract_status_literal.py:1`; scanner AST de escritores distingue atribuição de filtros em `bin/esmeril_chamado_censo.py:99,108,113`.

## 11/11 11:53 · sessao bc1033217c1a · - · - · origem=Ronald (tmux)

Censo dos rótulos de `ChamadoColaborador.status_local`, com fontes e consumidores:

| Fonte | Consumidores |
|---|---|
| Choices do modelo: `app/chamados/models.py:28-36`; campo associado em `:52` | `app/templates/chamados/partials/_card_atendimento.html:23`; `app/templates/relatorios/historico_chamados.html:121`; `app/templates/ponto/partials/painel_justificativa.html:79`; pauta de resposta tardia em `app/chamados/services/resposta_tardia.py:63-67` |
| `ESPERA_COLAB`: `app/chamados/services/verbos.py:114-122`; função `rotulo_para_pessoa` em `:138-142`; filtro `espera` em `app/chamados/templatetags/chamados_tags.py:54-64` | Badges: `detalhe.html:36-46`, `detalhe_local.html:68-78`, `fio_colaborador.html:188-198`, `partials/painel_chamado_turno.html:18-28`, `partials/modal_fio.html:102-112`, todos sob `app/templates/chamados/`. Seletores: `detalhe.html:132-134`, `detalhe_local.html:395-397`, `fio_colaborador.html:268-270`. Os dois estados terminais recebem “Encerrado” em `verbos.py:118-119`. |
| `ESTADO_TITLE`: `app/chamados/services/verbos.py:100-108`; distingue “Resolvido” e “Fechado” em `:103-104` | Segundo elemento do filtro `espera`, exibido nos `title` dos badges acima; também `rotulo_de_linha` em `:125-135`. |
| `rotulo_de_linha`: `app/chamados/services/verbos.py:125-135` | Produção do painel: `app/chamados/services/painel.py:384-394` e `:485-487`; consumo em `app/templates/chamados/partials/_lista_chamados.html:80-87` e `_lista_resumo.html:68-72`. Texto principal é verbo/“Registro”; estado fica no `title`. |
| Dicionário próprio da API | `app/api/views.py:1143-1146`: Aberto, Em atendimento, Resolvido, Fechado; fallback devolve código cru. |
| Literais do modal | `app/templates/chamados/partials/modal_fio.html:668-670`: “Assumir a apuração”, “Encerrar”, “Encerrar e arquivar”; títulos “Em analise”, “Resolvido”, “Fechado”. Não consulta mapa nesses elementos. |
| Literais do painel de turno | `app/templates/chamados/partials/painel_chamado_turno.html:76-78`: mesmas três opções e títulos do modal. |
| Literais do filtro de relatório | `app/templates/relatorios/historico_chamados.html:56-59`: Aberto, Em análise, Resolvido, Fechado. |
| Literais de documentação humana | `app/templates/core/manual.html:207`, `:215-217`, `:219`, `:222`, `:225`, `:249-254`, `:278`, `:282-286`, `:294`. |

Outros textos manuais próximos do estado: “Reabrir” nos seletores `app/templates/chamados/detalhe.html:135` e `detalhe_local.html:398`; “Este chamado está fechado” em `detalhe.html:148` e `detalhe_local.html:412`; “Resolvido em” em `detalhe.html:56`, `detalhe_local.html:88` e relatório `app/templates/relatorios/historico_chamados.html:127`. Os títulos “Em analise” escritos independentemente do estado estão em `app/templates/chamados/detalhe.html:101`, `fio_colaborador.html:225` e `partials/_relatorio_geo.html:22`.

Exposição do código cru:

| Sítio | Fonte |
|---|---|
| PDF: `app/relatorios/pdf_chamados.py:116`, renderizado em `:122-125` | `ch.status_local`, sem choices/mapa |
| Calendário: `app/templates/colaboradores/partials/_calendario_grade.html:108` | `dia.ticket_falta.status`, produzido a partir de `_c.status_local` em `app/colaboradores/services/calendario.py:441,444` |
| API mensageria: `app/api/views_mensageria.py:417` e `:1082` | `status_local` direto, campos `status_local` e `situacao` |
| Histórico do chamado: `app/chamados/services/historico.py:156`; API em `app/api/views_mensageria.py:2166` | `estado` recebe `status_local` cru |
| Eventos históricos | `app/chamados/models.py:292-294` grava descrição `status anterior -> novo via ...` com códigos; `services/historico.py:86-95` lê descrição; `:204-209` monta texto; API entrega em `views_mensageria.py:2168` |
| Comandos humanos | Código cru em `app/chamados/management/commands/silenciar_chamados_isentos.py:39-41`; `app/ponto/management/commands/reconciliar_vinculo.py:55-57`, `reavaliar_ausencias_feriado.py:42`, `reavaliar_ausencias_lancadas.py:43`, `reconciliar_geofence.py:36`; erros em `app/chamados/management/commands/smoke_chamados.py:137,150` |

GLPI tem outro domínio de estado: `app/chamados/glpi_client.py:126-133` define `STATUS_MAP` numérico; `:135-143` monta `status_label`; `:198,202` formatam listagem/detalhe; `app/templates/chamados/partials/tabela_chamados.html:40` exibe, com render da view em `app/chamados/views.py:340`.

A aba agrupada escreve “Histórico/Cancelados/Encerrados” em `app/templates/chamados/painel_gestao.html:153`; não é correspondência individual de estado. “registrados” do placar está em `:191`.

O JS do painel compara códigos: `app/templates/chamados/painel_gestao.html:382` usa `r.dataset.status === status`; o atributo vem de `app/templates/chamados/partials/_lista_chamados.html:33`. Renderização do consumidor externo da API/mobile: [não verificado].

## 11/11 11:52 · sessao aad120ef0dda · - · - · origem=Ronald (tmux)

1. **Detalhe: duas páginas produzem as opções.**

| Sítio | Texto produzido |
|---|---|
| `app/templates/chamados/detalhe_local.html:394` | Literal “— manter atual —”. |
| `app/templates/chamados/detalhe_local.html:395` | `"em_analise"|espera|first`. |
| `app/templates/chamados/detalhe_local.html:396` | `"resolvido"|espera|first`. |
| `app/templates/chamados/detalhe_local.html:397` | `"fechado"|espera|first`. |
| `app/templates/chamados/detalhe_local.html:398` | Literal “Reabrir”, value `aberto`. |
| `app/templates/chamados/detalhe.html:131` | Literal “— manter atual —”. |
| `app/templates/chamados/detalhe.html:132` | `"em_analise"|espera|first`. |
| `app/templates/chamados/detalhe.html:133` | `"resolvido"|espera|first`. |
| `app/templates/chamados/detalhe.html:134` | `"fechado"|espera|first`. |
| `app/templates/chamados/detalhe.html:135` | Literal “Reabrir”, value `aberto`. |

O filtro delega para `rotulo_para_pessoa`, que consulta `ESPERA_COLAB`; nesse mapa, `resolvido` e `fechado` têm ambos “Encerrado”. `app/chamados/templatetags/chamados_tags.py:53`, `app/chamados/templatetags/chamados_tags.py:63`, `app/chamados/services/verbos.py:138`, `app/chamados/services/verbos.py:118`, `app/chamados/services/verbos.py:119`.

A view local renderiza `detalhe_local.html` com `montar_contexto_detalhe`; o contexto entrega o chamado e permissões, sem lista de opções. `app/chamados/views.py:549`, `app/chamados/views.py:550`, `app/chamados/services/acoes_chamado.py:399`. A outra view renderiza `detalhe.html` diretamente. `app/chamados/views.py:262`. Os formulários são HTML nesses próprios templates. `app/templates/chamados/detalhe_local.html:385`, `app/templates/chamados/detalhe.html:122`. Gerador JS dessas opções: [não verificado].

2. **O modal distingue por literais no template; não consulta mapa compartilhado para suas opções.**

| Value | Texto | Title | Referência |
|---|---|---|---|
| vazio | — mover para — | — | `app/templates/chamados/partials/modal_fio.html:667` |
| `em_analise` | Assumir a apuração | Em analise | `app/templates/chamados/partials/modal_fio.html:668` |
| `resolvido` | Encerrar | Resolvido | `app/templates/chamados/partials/modal_fio.html:669` |
| `fechado` | Encerrar e arquivar | Fechado | `app/templates/chamados/partials/modal_fio.html:670` |

A view identifica esse componente como modal do painel e o renderiza. `app/chamados/views.py:869`, `app/chamados/views.py:870`, `app/chamados/views.py:927`. O painel lateral de turnos repete os mesmos três rótulos e titles, também à mão. `app/templates/chamados/partials/painel_chamado_turno.html:76`, `app/templates/chamados/partials/painel_chamado_turno.html:77`, `app/templates/chamados/partials/painel_chamado_turno.html:78`; renderizador em `app/chamados/views.py:347`, `app/chamados/views.py:353`.

Os choices do modelo distinguem “Resolvido” e “Fechado”, mas as opções dos quatro templates acima não os leem. `app/chamados/models.py:31`, `app/chamados/models.py:32`, `app/templates/chamados/detalhe_local.html:396`, `app/templates/chamados/detalhe.html:133`, `app/templates/chamados/partials/modal_fio.html:669`, `app/templates/chamados/partials/painel_chamado_turno.html:77`.

5. **Testes e leis relevantes.** [nao verificado]

- `test_o137_esmeril_chamado.py` varre texto de espera escrito à mão; exclui deliberadamente “Encerrado” do padrão. `app/chamados/tests/test_o137_esmeril_chamado.py:34`, `app/chamados/tests/test_o137_esmeril_chamado.py:99`.
- O mesmo teste exige que `espera(st)` devolva exatamente `rotulo_para_pessoa(st)` para todo estado conhecido e entradas de fallback. `app/chamados/tests/test_o137_esmeril_chamado.py:115`.
- Fixa literalmente `Registrado|Registrado (auditoria, sem acao devida)`, `Encerrado|Superado — o fato vive em outro chamado` e `Com o DP|Aberto (aguardando primeira analise)`. `app/chamados/tests/test_o137_esmeril_chamado.py:122`.
- Há guarda contra estado de banco visível nos templates de chamados: enumera “Resolvido”/“Fechado” e exige lista vazia de ofensores. Alterar opções para esses textos crus encontra essa guarda. `app/chamados/tests/test_ui_fatia2_taxonomia.py:25`, `app/chamados/tests/test_ui_fatia2_taxonomia.py:60`.
- Há teste exigindo o estado “Resolvido” no `title` da fila. `app/chamados/tests/test_ui_fatia2_taxonomia.py:131`.
- Teste existente que exija especificamente unicidade das opções `resolvido`/`fechado`: [não verificado]. [nao verificado]
- L2 determina a mesma função para tela e ação; o selo documental citado é `holerite/tests/test_selo_badge_viaja.py::test_o_numero_aparece_na_TELA_nao_so_no_contexto`. `app/docs/LEIS-UI.md:23`, `app/docs/LEIS-UI.md:29`.
- L7 determina texto vindo do mesmo mapa e proíbe hardcode por tela; declara **“sem selo ainda”**. `app/docs/LEIS-UI.md:77`, `app/docs/LEIS-UI.md:85`.

6. **Intersecção direta entre produtores desses textos e os cinco caminhos informados: nenhuma.** Os produtores identificados são os quatro templates, `chamados_tags.py` e `services/verbos.py` acima. `app/templates/chamados/detalhe_local.html:395`, `app/templates/chamados/detalhe.html:132`, `app/templates/chamados/partials/modal_fio.html:668`, `app/templates/chamados/partials/painel_chamado_turno.html:76`, `app/chamados/templatetags/chamados_tags.py:63`, `app/chamados/services/verbos.py:114`.

Há dependência de `app/chamados/juizes.py` na montagem de contexto, mas para `folha_julga_pontualidade`, sem produção desses rótulos. `app/chamados/services/acoes_chamado.py:331`, `app/chamados/views.py:926`. O caminho literal `app/ponto/cartorio.py` não foi encontrado [não verificado]; o arquivo existente é `app/ponto/services/cartorio.py` [não verificado].

## 11/11 11:50 · sessao 39b2c713671a · MAPA-20 · verificador · origem=Code

REPROVADO para pouso [nao verificado]

Referências `base:` apontam para a cópia disponível; `diff:` usa as linhas de destino do v3, sem aplicá-lo. Identidade da cópia com o HASH informado: [nao verificado]. Execução integral dos selos e publicação real: [nao verificado].

1. **Fechamento dos achados numerados da volta 2.**

   | Achado da v2 | Resultado no v3 |
   |---|---|
   | **1 — triagem 1–5** | **Nome fecha**: universo literal correto. **Tabela permanece parcial**: fecha separador estreito, cercas, cabeçalho vazio e pontuação; ainda aceita tabela escondida em comentário HTML. **Retido fecha os defeitos apontados**, agora incluindo conferência da remoção. **Foto fecha no publicador atual**. **Selo permanece parcial**, pelos mutantes do item 4 abaixo. (`diff:bin/mapa20_valida.py:29–39,82–103,120–122`; `diff:bin/relato.sh:94–114`; `diff:bin/tests/test_mapa20_valida.sh:151–164`.) | [nao verificado]
   | **2 — saídas e falhas** | Fecha a falha de commit seguida de sucesso aparente: agora há `|| exit 1`. São **14 comandos `exit`**, discriminados no item 3. `cd` e os `git add` continuam sem guarda de falha. (`diff:bin/relato.sh:146–164`.) | [nao verificado]
   | **3 — parser e conformidade** | **Parcial**: exige as 12 colunas, mas ainda aceita conteúdo invisível e fonte que não fundamenta N. As recusas a `_E3_`, ausência de pipes externos e barras invertidas permanecem, agora como restrições explícitas do formato. (`diff:bin/mapa20_valida.py:10–21,82–103,110–135`.) |
   | **4 — selo e mutantes** | Fecha o mutante específico sem reconhecimento de tils; acrescenta fiscalização textual da foto. Permanecem outros mutantes de validador, foto e instalação do retido. Recorte literalmente vazio continua impedindo verde. (`diff:bin/tests/test_mapa20_valida.sh:76–79,105–109,160–168`.) | [nao verificado]
   | **5 — selos vizinhos** | Nenhuma quebra demonstrada nos predicados que leem `relato.sh`; execução integral [nao verificado]. (`base:bin/tests/test_codex_estado.sh:104–111`; `base:bin/tests/test_relato_publica_claude_md.sh:15–20`; demais referências no item 5.) |
   | **6 — escritores e leitores** | A foto agora tem variável própria. O escritor efetivo da fonte e seus gatilhos continuam [nao verificado]; o comentário afirma atualização por sessão/evento, sem implementá-la nesse bloco. (`diff:bin/relato.sh:89–98`; `diff:bin/mapa20_valida.py:10`.) |
   | **7 — classes históricas** | Persistem **selo sem mordida** e interpretação de conteúdo de outras colunas como encerramento da obra. Pré-requisitos efetivos de pouso continuam [nao verificado]. (`diff:bin/mapa20_valida.py:141–142`; `diff:bin/tests/test_mapa20_valida.sh:151–164`; `base:CLAUDE.md:377–381,394`.) | [nao verificado]

2. **Complemento X-de-N: cumprimento e contraexemplos.**

   Nos exemplos, **G** é o mapa completo de 12 colunas, com 18 obras, SEM DONO e seção inicial `PRONTO AUSENTE`, conforme a fixture do selo. Os contraexemplos do parser foram conferidos em memória, sem arquivos. (`diff:bin/tests/test_mapa20_valida.sh:20–39`; `diff:bin/mapa20_valida.py:80–149`.)

   | Regra | Conferência |
   |---|---|
   | **(i) Sem X de N nem PRONTO AUSENTE → rc 1** | **Cumpre para as 18 obras**: exige o literal, N≥1 e X≤N; a regex também impede X negativo. **Não cumpre literalmente para toda linha**: SEM DONO recebe exceção e passa com `n/a (SEM DONO)`. (`diff:bin/mapa20_valida.py:43,123–140`.) |
   | **(ii) N sem fonte citada → rc 1** | **Cumpre apenas a aparência de citação**. Ausência do padrão reprova; existência, conteúdo, autoridade e número de itens da fonte não são conferidos. (`diff:bin/mapa20_valida.py:44,132–135`.) |
   | **(iii) Encerrada com X<N → rc 1** | **Cumpre para palavra reconhecida nas colunas após `obra`**, inclusive maiúsculas e flexões; também reprova encerramento com PRONTO AUSENTE. A varredura não distingue estado da obra de nomes, arquivos ou negações. (`diff:bin/mapa20_valida.py:48,129–142`.) |
   | **(iv) Lista PRONTO AUSENTE no topo** | Exige seção antes da tabela e igualdade com as obras marcadas. **Parcial**: lê somente a primeira seção; uma segunda seção contraditória é ignorada. (`diff:bin/mapa20_valida.py:55–77,104–106,146–148`.) |

   **Aceitos indevidamente:**

   - Em G, E3 com `999 de 999`, fonte `app/docs/BACKLOG.md:1` e falta `nada` passa. A linha citada é o título do BACKLOG, não uma lista numerada de PRONTO. Também passa `a.md:0`. O código não confere N nem prova escrita de X. (`base:app/docs/BACKLOG.md:1`; `diff:bin/mapa20_valida.py:44,132–137`.)
   - `<!--` numa linha, seguido de G e depois `-->`, passa: a tabela fica escondida no comentário. G inteiramente indentado com quatro espaços também passa como conteúdo de código Markdown. O parser veta cercas, mas aplica `strip()` às linhas e não reconhece comentários. (`diff:bin/mapa20_valida.py:82–89`.)
   - Duas seções iniciais: primeira com `- nenhuma`, segunda com `- O168`, tabela sem ausentes. Passa, embora o documento apresente listas contraditórias. (`diff:bin/mapa20_valida.py:58–77,146–148`.)

   **Recusados indevidamente como encerramento da obra:**

   - E3 com `1 de 2` e `cruza com = BO-ROTULO-ENCERRADO` reprova. O nome dessa obra fica corretamente isento na própria coluna `obra`, mas dispara a regex quando citado em outra coluna. (`diff:bin/mapa20_valida.py:33,48,141–142`.)
   - E3 com `1 de 2` e texto `nao encerrada`, ou caminho `logs/encerrada.md:10`, reprova sem afirmar encerramento de E3. (`diff:bin/mapa20_valida.py:48,141–142`.)

   A exigência de falta **item a item** não é comprovada: para X<N, só há veto ao literal `nada`, além da checagem geral de célula preenchida. A gravação de `pousou fase <F>, X de N` no RELATO também não é implementada pelo novo bloco. (`diff:bin/mapa20_valida.py:120–122,136–137`; `diff:bin/relato.sh:89–114`.) [nao verificado]

3. **Todos os `exit` de `relato.sh`, com seus valores.**

   | Local | Valor e condição |
   |---|---|
   | `base:bin/relato.sh:27` | `1`: falha da trava. |
   | `base:bin/relato.sh:28` | `1`: destino sem `.git`. |
   | `base:bin/relato.sh:32` | `1`: falha da lista de nomes. |
   | `base:bin/relato.sh:35` | `1`: falha do BACKLOG-TOPO. |
   | `base:bin/relato.sh:38` | `1`: falha do ESTADO. |
   | `base:bin/relato.sh:42` | `1`: falha da SESSAO. |
   | `base:bin/relato.sh:86` | `1`: falha da raspagem do RELATO retido. |
   | `diff:bin/relato.sh:110` | `1`: MAPA não raspado continua existindo após tentativa de remoção. |
   | `diff:bin/relato.sh:128` | `1`: falha de `gerar_avais.py`. |
   | `diff:bin/relato.sh:143` | `1`: falha da raspagem geral. |
   | `diff:bin/relato.sh:157` | `$MAPA_RC`: `0` ou `1`, índice sem mudança. |
   | `diff:bin/relato.sh:162` | `1`: falha do commit. | [nao verificado]
   | `diff:bin/relato.sh:163` | `1`: falha da expressão push/echo. | [nao verificado]
   | `diff:bin/relato.sh:164` | `$MAPA_RC`: `0` ou `1`, saída final. |

   **Validador vermelho executado → rc 0:** não há esse caminho no fluxo mostrado. `MAPA_RC` começa em zero, recebe um na retenção e não é zerado depois. (`diff:bin/relato.sh:94–100,157–164`.)

   **MAPA público pode sumir sem o Python reprovar:** sim, **sem executar o validador**: cópia da fonte falha, Python é pulado, raspagem do retido falha e o público é removido. Entretanto, o ciclo já está com rc 1. A remoção é incluída no commit; não há saída normal zero nesse caminho. (`diff:bin/relato.sh:97–113,151–164`.) [nao verificado]

   **Foto:** no publicador atual, sim: copia o vivo para `$MAPA_FOTO`, valida essa foto e entrega o mesmo caminho ao raspador. O Python isoladamente aceita qualquer caminho recebido na CLI; ele não impõe essa propriedade. (`diff:bin/relato.sh:94–98,143`; `diff:bin/mapa20_valida.py:152–167`.) [nao verificado]

   **Rc 0 não comprova toda a publicação:** se o `git add` do MAPA falhar e o índice estiver sem mudanças, a saída de “nada mudou” ainda pode retornar zero com mapa verde. Esse erro não é guardado. (`base:bin/relato.sh:21`; `diff:bin/relato.sh:151–157`.) [nao verificado]

4. **O selo ainda deixa mutantes plausíveis passarem.**

   A sobrevivência abaixo decorre dos cenários e predicados do selo; execução integral dos mutantes: [nao verificado].

   - **Validador:** retirar `re.I` de `_ENCERRADA`. Os casos de encerramento usam minúsculas e preservam seus resultados; `ENCERRADA` com `1 de 2` passa no mutante. (`diff:bin/mapa20_valida.py:48`; `diff:bin/tests/test_mapa20_valida.sh:41,53–56`.)
   - **Foto:** inserir `MAPA_FOTO="$RAIZ/logs/MAPA-20/mapa.md"` imediatamente antes de formar `MAPA_PAR`. Valida a foto e publica o vivo. Os sete cenários não alteram a fonte entre esses pontos; os greps exigem presença textual das referências, sem impedir reatribuição. (`diff:bin/relato.sh:97–98`; `diff:bin/tests/test_mapa20_valida.sh:151–164`.)
   - **Higiene do retido:** retirar o `&& mv`, ajustando a continuação. O raspador falso apenas copia os mesmos bytes de `MAPA VELHO`; o selo não distingue instalar a versão raspada de conservar a original. (`diff:bin/relato.sh:105–106`; `diff:bin/tests/test_mapa20_valida.sh:115–123,127,144,153–154`.)
   - **Falha de remoção:** retirar a guarda da linha 110 também preserva os cenários, pois nenhum injeta falha de `rm`. (`diff:bin/relato.sh:110`; `diff:bin/tests/test_mapa20_valida.sh:151–157`.)

   **Recorte literalmente vazio com selo verde:** não. Os três recortes têm guardas que incrementam `FALHAS` quando falta a expressão exigida; verde exige zero. Isso comprova presença mínima, não integridade dos recortes. (`diff:bin/tests/test_mapa20_valida.sh:105–109,168–169`.) [nao verificado]

5. **Selos vizinhos que leem `relato.sh`.**

   Nenhuma quebra estática demonstrada nos cinco arquivos; execução integral [nao verificado].

   | Selo | Conferência |
   |---|---|
   | `test_codex_estado.sh` | Recorta a partir de `CODEX_OK=""`, depois do novo bloco; seu veto a `exit` não captura a nova guarda. (`base:bin/tests/test_codex_estado.sh:104–111`; `diff:bin/relato.sh:110,122–124`.) |
   | `test_relato_publica_claude_md.sh` | A regex continua selecionando a raspagem geral; CLAUDE permanece nos três pontos. O novo `git add` indentado não altera a contagem por início de linha. (`base:bin/tests/test_relato_publica_claude_md.sh:15–20`; `diff:bin/relato.sh:105–106,129–153,162`.) |
   | `test_codex_estado_evento.sh` | Permanecem as chamadas exigidas, sem gerador CODEX inline. (`base:bin/tests/test_codex_estado_evento.sh:27–30`; `base:bin/relato.sh:32`; `diff:bin/relato.sh:123`.) |
   | `test_relato_afirma_com_prova.sh` | A chamada exigida permanece. (`base:bin/tests/test_relato_afirma_com_prova.sh:43–45`; `base:bin/relato.sh:55–56`.) |
   | `test_publicacao_sem_nome.sh` | O universo vem de `git ls-files`; MAPA rastreado entra automaticamente. Resultado de privacidade real [nao verificado]. (`base:bin/tests/test_publicacao_sem_nome.sh:58–73`.) |

6. **Quem escreve e quem lê.**

   | Campo/arquivo | Escritor | Leitor |
   |---|---|---|
   | `MAPA_RC` | Shell: `0` inicialmente; `1` por falha da cópia ou validação. | Saídas sem mudança e final. (`diff:bin/relato.sh:94–100,157,164`.) |
   | `MAPA_PAR` | Shell: vazio ou par foto→público após validação verde. | Argumento opcional da raspagem geral. (`diff:bin/relato.sh:94–98,143`.) | [nao verificado]
   | `MAPA_FOTO` | Shell define caminho; `cp` escreve os bytes; trap limpa o temporário. | Validador e raspador geral. (`base:bin/relato.sh:25`; `diff:bin/relato.sh:94–98,143`.) |
   | `logs/MAPA-20/mapa.md` | Comentários atribuem à sessão/`montar_mapa.py`; escritor efetivo e gatilhos [nao verificado]. | `cp` do publicador. (`diff:bin/mapa20_valida.py:10`; `diff:bin/relato.sh:89–97`.) |
   | Público `MAPA-20.md` | Raspador geral; no retido, raspador→arquivo ao lado→`mv`; falha pode removê-lo. | Raspagem do retido, Git e selo de privacidade. (`base:bin/raspar_publico.py:23–40`; `diff:bin/relato.sh:104–110,143,151–163`; `base:bin/tests/test_publicacao_sem_nome.sh:66–73`.) |
   | `MAPA_PATH` | Shell: vazio ou nome após `git add` bem-sucedido. | Mensagem e pathspec do commit, incluindo retirada. (`diff:bin/relato.sh:150–162`.) | [nao verificado]

7. **Classes históricas, uma a uma.**

   | Classe | Resultado |
   |---|---|
   | **2º juiz** | Não há novo juiz de nomes: máscara e conferência reutilizam `nome_no_texto`. A fonte e a prova de X/N não são verificadas semanticamente. (`base:bin/raspar_publico.py:15–16,31,36`; `diff:bin/mapa20_valida.py:132–135`.) |
   | **2º escritor fora da porta** | As novas escritas públicas estão no publicador, depois da trava existente. Escritor efetivo da fonte [nao verificado]. (`base:bin/relato.sh:27`; `diff:bin/relato.sh:97–114,143`.) | [nao verificado]
   | **Relógio fora da borda** | O diff não introduz relógio decisório; `date` permanece na mensagem de commit. O timer já existia. Atualização efetiva da fonte por evento [nao verificado]. (`diff:bin/relato.sh:89–90,161`; `base:bin/hasner-relato.timer:5–7`.) | [nao verificado]
   | **Derivação paralela de fonte única** | O publicador atual valida e publica a mesma foto, pelo raspador existente. Reuso dos censos e medições reais [nao verificado]. (`diff:bin/relato.sh:94–98,143`; `base:bin/raspar_publico.py:19–40`.) | [nao verificado]
   | **Selo sem mordida** | **Presente**, pelos mutantes específicos do item 4. (`diff:bin/tests/test_mapa20_valida.sh:53–56,115–123,151–164`.) |
   | **Ordem de trava** | Nenhuma aquisição nova. `codex_estado.sh --gerar` retorna antes da trava do ramo evento. (`base:bin/relato.sh:27`; `diff:bin/relato.sh:123`; `base:bin/codex_estado.sh:54–56,65–66`.) |
   | **Pouso antes do pré-requisito** | Contrato efetivo, mapa completo no ar, suíte e opus final [nao verificado]. Comentário apontando contrato não comprova seu conteúdo; a base exige contrato e suíte antes do opus. (`diff:bin/relato.sh:93`; `base:CLAUDE.md:377–381,394`.) | [nao verificado]
   | **Campo com dois significados** | **Presente na decisão de encerramento**: nome de dependência ou caminho é interpretado como estado da obra. `MAPA_RC` representa falha da etapa, incluindo cópia; `MAPA_PATH` inclui retirada e não prova publicação de mapa novo. (`diff:bin/mapa20_valida.py:141–142`; `diff:bin/relato.sh:94–100,150–164`.) |

## 11/11 11:48 · sessao 9f78342994b3 · - · - · origem=Ronald (tmux)

- **14 `exit` após o diff:** base `bin/relato.sh:27,28,32,35,38,42,86` → `1`; diff `bin/relato.sh:110,128,143,162,163` → `1`; diff `bin/relato.sh:157,164` → `$MAPA_RC`. Inicialização `0` em `diff:bin/relato.sh:94`, falha fixa `1` em `:100`; não há reset posterior, portanto validador vermelho executado não termina `0`.

- **Mutante FOTO sobrevivente, por inspeção:** inserir `MAPA_FOTO="$RAIZ/logs/MAPA-20/mapa.md"` imediatamente antes de `MAPA_PAR=...` em `diff:bin/relato.sh:98`. A validação ainda usa a foto, publicação usa o vivo. Os sete cenários não alteram a fonte entre esses pontos, e os greps apenas exigem presença textual das duas referências a `$MAPA_FOTO`. (`diff:bin/tests/test_mapa20_valida.sh:151–164`.)

- **Mutante de higiene sobrevivente, por inspeção:** remover o `&& mv` de `diff:bin/relato.sh:106`, ajustando a continuação da linha anterior. O stub só copia bytes idênticos e o teste espera exatamente `MAPA VELHO`; não distingue instalação da cópia raspada de preservação da cópia original. Em uso real, perde a troca do arquivo antigo pelo higienizado. (`diff:bin/tests/test_mapa20_valida.sh:115–123,127,144,153–154`.) [nao verificado]

- **Outro sobrevivente:** retirar a guarda de remoção fracassada em `diff:bin/relato.sh:110`; nenhum cenário injeta falha em `rm`. (`diff:bin/tests/test_mapa20_valida.sh:151–157`.)

- **Recorte literalmente vazio não dá selo verde:** os três `awk` estão em `diff:bin/tests/test_mapa20_valida.sh:105–107`; a guarda `:108–109` incrementa `FALHAS` se faltar a palavra exigida em qualquer recorte; a saída verde exige zero em `:168`. [nao verificado]

- Contagem em memória confirmou **169 linhas** no teste; cabeçalho do hunk correto. Execução integral dos testes e mutantes: **[nao verificado]**. [nao verificado]

## 11/11 11:47 · sessao 7f8232774c9a · FIO-E0-V4-E3 · sonda · origem=Code

```text
sonda_leitura: sonda_cel_apos_desl.py [fb90b86fb968e4b4] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

celulas_apos_desligamento_total n 4811
trabalha_true_apos_desligamento n 2546
colabs_distintos n 108
trabalha_true_passado_ate_hoje n 2110
trabalha_true_futuro n 436
por_origem 'gerada' n 2546
por_empresa 2 n 2092
por_empresa 3 n 454
por_mes_civil 2026-07 n 41
por_mes_civil 2026-08 n 230
por_mes_civil 2026-09 n 486
por_mes_civil 2026-10 n 1026
por_mes_civil 2026-11 n 642
por_mes_civil 2026-12 n 121
top_col col305 n 92
top_col col946 n 75
top_col col316 n 68
top_col col746 n 64
top_col col870 n 62
top_col col393 n 61
top_col col963 n 60
top_col col518 n 57
top_col col951 n 57
top_col col417 n 51
top_col col931 n 50
top_col col506 n 47
top_col col880 n 47
top_col col666 n 43
top_col col929 n 41
top_col col939 n 41
top_col col143 n 32
top_col col437 n 32
top_col col894 n 32
top_col col840 n 29
top_col col338 n 28
top_col col370 n 28
top_col col862 n 28
top_col col270 n 27
top_col col782 n 27
top_col col846 n 26
top_col col847 n 26
top_col col126 n 25
top_col col367 n 25
top_col col408 n 25
top_col col914 n 25
top_col col138 n 24
top_col col325 n 24
top_col col400 n 24
top_col col422 n 24
top_col col744 n 24
top_col col927 n 24
top_col col432 n 23
top_col col698 n 23
top_col col997 n 23
conferencia col318 n 10
conferencia col997 n 18
agora 2026-11-11 11:46:55
```

## 11/11 11:46 · sessao 44743de30430 · - · - · origem=Ronald (tmux)

Confirmado em Python em memória, sem criar arquivos:

- **Tabela invisível ainda aceita:** `<!--\n` + G + `-->\n` retorna lista de problemas vazia. G inteiramente indentada com quatro espaços também passa, embora seja bloco de código Markdown. O parser só veta cercas e aplica `strip()` às linhas. (`diff:bin/mapa20_valida.py:82–89`.)
- **Fonte inválida aceita:** `a.md:0` passa. A regex exige apenas aparência de citação; não valida linha positiva, existência nem pertencimento a aval/TICKETS/BACKLOG. (`diff:bin/mapa20_valida.py:44,134–135`.)
- **Fonte sem extensão recusada:** `BACKLOG:10` reprova pela regex. A validade concreta dessa referência como fonte numerada permanece [nao verificado]. (`diff:bin/mapa20_valida.py:44,134–135`.)
- **Falso vermelho de encerramento:** com `1 de 2`, células `nao encerrada` e `logs/encerrada.md:10` reprovam. A regex examina todas as células exceto o nome, sem distinguir status, negação ou caminho. O próprio nome `BO-ROTULO-ENCERRADO` fica corretamente excluído dessa busca. (`diff:bin/mapa20_valida.py:48,141–142`.)
- **Lista duplicada/contraditória aceita:** primeira seção `## PRONTO AUSENTE` com `- nenhuma`, seguida de segunda seção com `- O168`, e tabela sem ausentes: passa. Só a primeira seção é lida. (`diff:bin/mapa20_valida.py:58–77,104,146–148`.)
- **Mutante sobrevivente:** remover `re.I` da regex de encerramento preserva todos os casos do selo, escritos em minúsculas, mas aceita `ENCERRADA` com `1 de 2`. Sobrevivência por inspeção dos casos; suíte integral [nao verificado]. (`diff:bin/mapa20_valida.py:48`; `diff:bin/tests/test_mapa20_valida.sh:41,53–56`.) [nao verificado]

Fechamentos específicos da v2: universo literal corrigido; largura do separador, cabeçalho vazio e células só com pontuação barrados; cercas de crases/tils e casos 4/3 barrados. Permanecem, por decisão do formato fixo, as recusas a `_E3_`, tabela sem pipes externos e qualquer barra invertida. (`diff:bin/mapa20_valida.py:29–39,82–103,110–122`.)

## 11/11 11:44 · sessao 40e2b3668b76 · - · - · origem=Ronald (tmux)

Nenhuma quebra estática dos cinco selos vizinhos foi encontrada; execução integral deles fica [nao verificado], pois criam arquivos temporários.

- `test_codex_estado.sh`: recorta de `CODEX_OK=""` ao passo 3; o novo 2f termina antes disso. Portanto, o novo `exit 1` da guarda de remoção não entra no bloco proibido. (`base:bin/tests/test_codex_estado.sh:104–111`; `diff:bin/relato.sh:110,114,122–124`.)
- `test_relato_publica_claude_md.sh`: a regex da raspagem continua selecionando a chamada geral com CLAUDE; a nova re-raspagem de MAPA tem argumento depois de `$DEST` antes da continuação, não satisfazendo o prefixo dessa regex. O novo `git add` está indentado e não aumenta o número de linhas iniciadas por `git add -A `. CLAUDE permanece no pathspec. (`base:bin/tests/test_relato_publica_claude_md.sh:15–20`; `diff:bin/relato.sh:105–106,129–143,147,153,162`.)
- `test_codex_estado_evento.sh`: as chamadas de `codex_estado.sh` e `relato_nomes.sh` permanecem; o diff não introduz `gerar_codex_estado.py` inline. (`base:bin/tests/test_codex_estado_evento.sh:27–30`; `base:bin/relato.sh:32`; `diff:bin/relato.sh:123`.)
- `test_relato_afirma_com_prova.sh`: a chamada exigida não foi alterada. (`base:bin/tests/test_relato_afirma_com_prova.sh:43–45`; `base:bin/relato.sh:55–56`.)
- `test_publicacao_sem_nome.sh`: o universo continua vindo de `git ls-files` no público; MAPA rastreado é incluído automaticamente. Privacidade no público real: [nao verificado]. (`base:bin/tests/test_publicacao_sem_nome.sh:58–73`.)

Classes:

- **Segundo juiz:** a raspagem e sua conferência continuam usando `nome_no_texto`, sem novo predicado paralelo de nomes. (`base:bin/raspar_publico.py:15–16,31,36`; `diff:bin/relato.sh:105,129–143`.)
- **Segundo escritor fora da porta:** as duas escritas públicas novas estão dentro do publicador, depois da trava existente; escritor efetivo da fonte e outros escritores no ambiente: [nao verificado]. (`base:bin/relato.sh:27`; `diff:bin/relato.sh:97–114,129–143`.) [nao verificado]
- **Relógio fora da borda:** o diff não adiciona relógio decisório; `date` continua na mensagem de commit. O timer de cinco minutos já existe. (`diff:bin/relato.sh:161`; `base:bin/hasner-relato.timer:5–7`.) [nao verificado]
- **Ordem de trava:** nenhuma aquisição nova no diff. A rede `codex_estado.sh --gerar` retorna antes de adquirir a trava do ramo evento, evitando autoespera nesse caminho. (`base:bin/relato.sh:27`; `diff:bin/relato.sh:123`; `base:bin/codex_estado.sh:54–56,65–66`.)
- **Pouso antes do pré-requisito:** contrato efetivo, suíte completa e opus final: [nao verificado]. A base exige contrato antes do instrumento e suíte antes do opus; Codex não aprova sozinho o pouso. (`base:CLAUDE.md:377–381,394`; `diff:bin/relato.sh:93`.) [nao verificado]

Numeração confirmada do `relato.sh` após v3: o bloco novo ocupa **89–114**; `gerar_avais` fica em **128**; raspagem geral **129–143**; `git add` MAPA **153**; saídas normais **157/164**; commit com guarda **161–162**; push com guarda **163**. (`diff:bin/relato.sh:89–164`.) [nao verificado]

## 11/11 11:37 · sessao 70d2beb073f4 · FABLE-GABARITO · leitura · origem=Code

| caso | classe | arquivo do veredito opus | veredito em 1 linha | PROVA da resposta | PACOTE EXATO recebido | defeito real |
|---|---|---|---|---|---|---|
| TOTAL: 0 casos elegíveis | a: **0/6**; b: **0/3**; c: **0 comprovados** | — | Mínimos não atingidos | Nenhum candidato abaixo reúne pacote integral identificável e prova exigida; pacote obrigatório em `aval_gabarito_fable_1111.txt:1`. | Completude dos pacotes [nao verificado]; lacunas discriminadas em FORA. | — |

FORA

- **E3 08:3x horizonte** — REPROVADO; cura identificada: `2e9d534d8`, “E3 fix9 (copia): sem_adesao so do HORIZONTE_SEM_ADESAO 2026-11 em diante (opus final B1)”; prompt e diff exatos recebidos [nao verificado]. O `.raw` tem registros truncados e não permite recuperar o pacote. (`logs/FIO-E0-V4-E3/opus_final_e3_b1.txt:1-7`; `logs/FIO-E0-V4-E3/opus_final_e3.raw:1-12`.)
- **BO-0611-05 opus_final2 0.0** — o veredito é **APROVA**, com o problema dos mapas já registrado; cura posterior `fdf9fc05b`, “BO-0611-05 volta 8: modal_fio e _relatorio_geo perguntam ao juiz tem_gps (0.0 nao desenha mapa)”; prompt/diff exatos [nao verificado]. (`logs/BO-0611-05/opus_final2.txt:2-3`.)
- **BO-0611-05 opus_final1** — REPROVADO sobre `4ffb05150`; pacote do opus [nao verificado]: os nomes visíveis de censo não identificam sua pergunta final nem seu diff. (`logs/BO-0611-05/opus_final1.txt:1`.)
- **BO-0611-05 opus_final3** — APROVADO sobre `fdf9fc05b`, pouso `fb9269ea9`; pacote exato [nao verificado] e janela de 48 h ainda incompleta no gitlog fornecido. (`logs/BO-0611-05/opus_final3.txt:1-4`; `gitlog_14d.txt`, commit `fb9269ea9`.)
- **E4a-1** — REPROVADO sobre diff `f2962ab8d..629f2d244`; cura `5578212ef`, “FIO-E0-V4-E4 E4a-1: curas P1-P5 da 1a reprovacao do opus final (verificador)”; arquivo do prompt e diff salvo exatos [nao verificado]. (`logs/FIO-E0-V4-E4-ESCRITORES/opus_final_e4a1_1.txt:1-3`.)
- **E4A-2 final1** — REPROVADO sobre `11f82cfec`; pacote final integral [nao verificado]. (`logs/FIO-E0-V4-E4A-2/opus_final1.txt:1`.)
- **TRAVA-SELO v11** — APROVADO sobre `04ec2b8b0`, pouso `9ca563bb3`; `diff_v1.txt` e `preverif_v1.q` não demonstram o pacote final v11; tampouco há 48 h posteriores no gitlog. (`logs/TRAVA-SELO/opus_final_v11.r:1`; `gitlog_14d.txt`, commit `9ca563bb3`; pacote v11 [nao verificado].)
- **VERIFICADOR-EM-FABLE v3** — REPROVA sobre `238cd5cdf`; cura `5ab7b0ff3`, “VERIFICADOR-EM-FABLE (1) volta 4: a negativa sem obra tem prefixo proprio B3-SEM-OBRA”; `c1.diff` e `preverif1.q` não identificam o pacote final v3. (`logs/VERIFICADOR-EM-FABLE/opus_final_v3.txt:1-2`; pacote v3 [nao verificado].)
- **L-123-MEDIDA-EM-PY** — REPROVADO sobre `7a2e3bc5`; aparecem `diff.patch` e `v1.q`, mas sua vinculação ao pedido final e a relação completa de anexos [nao verificado]. (`logs/akmed/opus_final.txt:1`.)
- **AUDITORIA v5** — bloqueio sobre `500bbdde3`; pergunta/diff finais e commit da cura seguinte [nao verificado]. (`logs/auditoria_0911/opus_final_v5.txt:1-2`.)
- **C20-FIO-MUDO final v5** — REPROVA sobre `5f3ea8c2`; as três entradas reproduzem o mesmo laudo, sem demonstrar pacote final integral ou commit da cura seguinte. (`logs/c20_fiomudo/codex/opus_final_v5_achados.txt:1`; `logs/c20_fiomudo/opus_final_1d78ef7f/opus_final_v5_achados.txt:1`; `logs/c20_fiomudo/opus_final_3ee00412/opus_final_v5_achados.txt:1`; pacote/cura [nao verificado].)
- **C20-O169** — REPROVA sobre `6afa8aa1/e76e0419`; cura `e13209863`, “O169 volta opus: L-125 CLUSTER-NAO-FECHA-POR-IDADE entra no LEIS.md + ADR (bloqueante do opus final)”; pacote final exato [nao verificado]. (`logs/c20_o169/opus_final_1.txt:1-2`.)
- **COBPARC v4** — REPROVADO; o laudo distingue `113fd774` de `129bfeaa`, e suas duas cópias não identificam o pacote recebido integralmente. (`logs/cobparc/v4/opus_final.md:1-6`; `logs/cobparc/v4/pm/opus_final_achados.md:1-6`; pacote [nao verificado].)
- **COBPARC v7** — REPROVADO sobre `8f21ec29`; cura `d03410912`, “[COBERTURA-PARCIAL-UM-JUIZ] v8 achados 1 e 2 (opus final v7): o leitor da ata transporta justificado; o vinculo volta a contar”; prompt/diff finais exatos [nao verificado]. (`logs/cobparc/v7/opus_final.txt:1-4`.)
- **COBPARC v8** — REPROVADO sobre `f05713f5`, com amend `72eb11be` relatado; pacote exato e assunto do commit de cura no gitlog [nao verificado]. (`logs/cobparc/v8/opus_final.txt:1-3`.)
- **COBPARC-FILA-RELOGIO v1** — REPROVADO sobre `69a6a682`; pergunta/diff finais e commit da volta seguinte que cite o achado [nao verificado]. (`logs/cobparc_relogio/opus_final.txt:1-3`.)
- **COBPARC-FILA-RELOGIO v2** — REPROVADO sobre `12200105`; pacote exato e commit de cura seguinte [nao verificado]. (`logs/cobparc_relogio/opus_final.txt:10-11`.)
- **CODEX-CONSTRUTOR inicial** — REPROVA sobre `d63b837f`; a cura aparece em `bbdbceb91`, “CODEX-CONSTRUTOR-NEGOCIO volta 4: o inventario do construtor varre o FS da copia e .git em qualquer nivel e PROIBIDO”; porém, o prompt corta o nome do anexo em `logs/codexneg/mutantes_selo`. Completar esse nome pelo veredito reconstruiria o pacote. (`logs/codexneg/opus_final_r.md:1`; `logs/codexneg/opus_final_prompt.txt:3`; identidade integral do anexo [nao verificado].)
- **CODEX-CONSTRUTOR v4** — REPROVA sobre `9995e2a6`; cura `cf6360832`, “CODEX-CONSTRUTOR-NEGOCIO volta 5 (opus final v4 1b): todo python3 do wrapper sob -I”; prompt, diff e delta estão nomeados, mas a relação de triagens está truncada após `logs/codex`. (`logs/codexneg/opus_final_v4_r.md:1-3`; `logs/codexneg/opus_final_v4_prompt.txt:3`; pacote integral [nao verificado].)
- **CODEX-CONSTRUTOR v5** — APROVA sobre `37880aa1`, pouso `a1d70f619`; o catálogo mostra somente a cabeça de 12 das 20 linhas do prompt, com referências truncadas: completude dos anexos [nao verificado]. (`logs/codexneg/opus_final_v5_r.md:1`; `logs/codexneg/opus_final_v5_prompt.txt:6-8`.)
- **CONTENCAO v2/p1** — REPROVADO sobre `3a743cdd`; pacote final integral e commit da cura seguinte [nao verificado]. (`logs/contencao/v2/p1/opus_final.md:1-3`.)
- **CRONS-INSTALL-SOB-MARCA** — APROVADO sobre `073d840f`, pouso `3ce6f52b5`; pergunta/diff exatos do opus [nao verificado]. (`logs/cronsmarca/opus_final_1.txt:1`; `gitlog_14d.txt`, commit `3ce6f52b5`.)
- **CXR-TELA final1** — REPROVA sobre `71c91c97`; cura `bab61829f`, “CXR-TELA-LE-O-MOTOR volta 4 (opus final 1 REPROVA 71c91c97): selo C3 no caminho C11 e relogio na borda”; pacote final exato [nao verificado]. (`logs/cxr_tela/opus_final_71c91c97.md:1-4`.)
- **CXR-TELA final2** — APROVA sobre `6a5e5ad8`, pouso `94d41ea56`; pergunta/diff finais exatos [nao verificado]. (`logs/cxr_tela/opus_final_6a5e5ad8.md:1`; `gitlog_14d.txt`, commit `94d41ea56`.)
- **E4-CALENDARIO-L1** — REPROVADO sobre `15d4f10b`; há três perguntas nomeadas, sem identificação verificável de qual foi recebida e de todos os anexos/diffs. (`logs/e4cal/postmortem_opus/opus_final.txt:2`; pacote [nao verificado].)
- **E-SEM-VIVA-B final sobre 97411e5c** — REPROVA; cura `3be9c5423`, “E-SEM-VIVA-B volta 4: a C5 do desligado le a LAVRA do juiz _K2 (RetratoFolha) e nao recompoe rescisao + retentor”; pacote final exato [nao verificado]. (`logs/esemvivab/opus_final_97411e5c.md:1-3`.)
- **E-SEM-VIVA-B final sobre 1936c9d2** — APROVA, pouso `40a005457`; pergunta/diff finais exatos [nao verificado]. (`logs/esemvivab/opus_final_1936c9d2.md:1-2`; `gitlog_14d.txt`, commit `40a005457`.)
- **F3-BECO-1 final1** — REPROVADO sobre `e2e3d901`; cura `f10f8328a`, “F3-BECO-1 retrabalho: B1+F1+F5+F6 do laudo opus_final_1 (C11 em lote, dono unico do dia)”; pacote final exato [nao verificado]. (`logs/f3_beco1/opus_final_1.txt:1-2`.)
- **F3-BECO-1 final2** — APROVA o código sobre `59bbe2e3`, pouso `9c0fd9d3e`; pacote final exato [nao verificado]. (`logs/f3_beco1/opus_final_2.txt:1-3`; `gitlog_14d.txt`, commit `9c0fd9d3e`.)
- **E1 final sobre afb67623** — REPROVA; cura `6e53526de`, “FIO-E0-V4-E1 volta 12 (opus final 1 REPROVA afb67623, reprova 1 de 3, PISTA-LIVRE): S143 so trava quando o save ESCREVE o status”; pacote exato [nao verificado]. (`logs/fio_e0_v4/opus_final_e1_afb67623.md:1`.)
- **E1 final2 sobre 6695e972** — REPROVADO; pergunta/diff finais exatos [nao verificado]. (`logs/fio_e0_v4/opus_final2_e1_6695e972.md:1-2`.)
- **E1 final3 sobre 76644dc8** — REPROVADO; cura `d73d84a92`, “FIO-E0-V4-E1 volta 15d (opus final 3 reprovou 76644dc8): o reuso de abrir re-chaveia pela porta unica”; pacote exato [nao verificado]. (`logs/fio_e0_v4/opus_final3_e1_76644dc8.md:1-3`.)
- **E1 final4 sobre b0c4297c** — REPROVADO; pergunta/diff finais exatos [nao verificado]. (`logs/fio_e0_v4/opus_final4_e1_b0c4297c.md:1-2`.)
- **DEPLOY-PORTAO final5** — APROVA o intervalo `a5df8ed5..c24a1914`; pacote final exato [nao verificado], e as voltas posteriores `e04870002`/`20df97548` impedem presumir aprovação limpa desse intervalo. (`logs/fio_e0_v4/opus_final_deployportao5.md:1`; `gitlog_14d.txt`, commits `e04870002`, `20df97548`.)
- **E2 final sobre 3b853907** — REPROVADO; pergunta/diff finais exatos [nao verificado]. (`logs/fio_e0_v4/opus_final_e2.txt:1-6`.)
- **E2 final sobre d912345b** — REPROVA; `e2_opus_final_c2833d85_d912345b.diff` está nomeado, mas pergunta final e conjunto integral dos anexos [nao verificado]. (`logs/fio_e0_v4/opus_final_e2_d912345b.md:1-3`; `logs/fio_e0_v4/e2_opus_final_c2833d85_d912345b.diff:1`.)
- **GRADE-v5** — REPROVADO sobre `391b74dc`; pergunta/diff finais exatos [nao verificado]. (`logs/grade_folga/v5/pm/opus_final.txt:1`.)
- **GRADE-FOLGA-JUIZ sobre abd37234** — REPROVADO; `pv/` e `v10/` contêm cópias do mesmo laudo, não dois casos; pacote final exato [nao verificado]. (`logs/grade_juiz/pv/opus_final.txt:1`; `logs/grade_juiz/v10/opus_final.txt:1`.)
- **GRADE-FOLGA-JUIZ va** — REPROVADO sobre `d9cb0e42`; cura `60258b809`, “GRADE-FOLGA-JUIZ vb: cura dos 3 achados do opus final (prova e documento; codigo do juiz intocado)”; o brief identifica o ensaio por diretório, sem enumerar cada arquivo recebido. (`logs/grade_juiz/va/opus_final_achados.md:1-4`; `logs/grade_juiz/va/opus_final_brief.md:7`; pacote integral [nao verificado].)
- **GRADE-FOLGA-JUIZ vb** — brief identifica `60258b80`, diff e delta, mas arquivo do veredito final vb e commit do pouso no gitlog fornecido [nao verificado]. (`logs/grade_juiz/vb/opus_final_brief.md:1-3`.)
- **H12H06-B sobre 4e9380e5** — REPROVA; cura `90747d3a7`, “H12H06-B volta 5: editar_ausencia_registro vence a projecao do range antigo e do novo; o GRAVADO afirma a regra no DSR”; pacote final exato [nao verificado]. (`logs/h12h06b/opus_final_4e9380e5.md:1-3`.)
- **H12H06-B-PAUTA-08 v7** — APROVADO sobre `dbabd2ad`, pouso `967d48211`; pergunta/diff finais exatos [nao verificado]. (`logs/h12h06b/pauta08/v7/opus_final_dbabd2ad.md:1`; `gitlog_14d.txt`, commit `967d48211`.)
- **JUIZ-1 v5** — APROVADO sobre `d38e6b91`, pouso `630131b1f`; pacote final exato [nao verificado]. (`logs/jv1/opus_final_v5.txt:1`; `gitlog_14d.txt`, commit `630131b1f`.)
- **JUIZ-1 resolução de merge** — APROVADO sobre `ed9cd506 + c4525ff4`; pergunta/diff finais exatos e correspondência com o commit de pouso [nao verificado]. (`logs/jv1/opus_final_merge.txt:1`.)
- **LEN-VIVOS** — APROVADO sobre `f71d570e`, pouso `f9b89feaa`; pacote final exato [nao verificado]. (`logs/lenvivos/v2/opus_final.txt:1-2`; `gitlog_14d.txt`, commit `f9b89feaa`.)
- **MOTOR-LE-O-TURNO-INSTRUMENTO final2** — APROVA sobre `bc9db924`, pouso `935c8d961`; pacote final exato [nao verificado]. (`logs/motor_turno/piloto/opus_final2.md:1`; `gitlog_14d.txt`, commit `935c8d961`.)
- **O261-E-F2 final1** — REPROVADO sobre `7882a6e6`; cura `e9690d0c1`, “[O261-E-F2] volta 4: quem chama a porta do carimbo (selo AST), testemunha viva da classe (f) e DOSSIES:46”; pacote final exato [nao verificado]. (`logs/o261e/f2/opus_final_v1.md:1-2`.)
- **O261-E-F2 final2** — APROVADO sobre `5c2bb8a4`; pacote final exato e commit do pouso correspondente no gitlog [nao verificado]. (`logs/o261e/f2/opus_final_v2.md:1-2`.)
- **PADRAO-DE-CIMA final** — REPROVADO sobre `b3c0f8ec`; pergunta/diff finais exatos e commit da cura seguinte [nao verificado]. (`logs/padrao_de_cima/v6/r9/opus_final.txt:1-2`.)
- **PAR-NAO-ADJACENTE final1** — REPROVA sobre `795cafdf`; pacote final exato e commit da volta seguinte que cite o achado [nao verificado]. (`logs/par_nao_adjacente/opus_final_1.txt:1-2`.)
- **PAR-NAO-ADJACENTE final2** — APROVA o diff `d361dfee`; pergunta/diff salvos exatos e commit do pouso no gitlog [nao verificado]. (`logs/par_nao_adjacente/opus_final_2.txt:1`.)
- **BUG-PLANTIO-NOTURNO-MAIS-1 final** — BLOQUEIA; o laudo nomeia `final.diff` e `f1.r`, mas não a pergunta final salva nem o pacote integral; commit avaliado [nao verificado]. (`logs/plantio_mais1/opus_final.txt:1-4`, `:11`.)
- **SELOS-CANON-I1 final1** — REPROVA sobre `f0103befd`; cura `f56dc9628`, “SELOS-CANON-I1 volta 5 (construtor): pertenca da fila 1 pelo TOKEN da celula nas duas vias”; pacote final exato [nao verificado]. (`logs/selos_canon/i1_opus_final.md:1-2`.)
- **SELOS-CANON-I1 final2** — REPROVA sobre `788ae3e28`; cura `d1502c4ae`, “SELOS-CANON-I1 volta 6 (construtor opus): o espelho da pertenca da fila 1”; pacote final exato [nao verificado]. (`logs/selos_canon/i1_opus_final_2.md:1-2`.)
- **SELOS-CANON-I1 final3** — APROVADO, pouso `3803e6623`; pergunta/diff finais exatos [nao verificado]. (`logs/selos_canon/i1_opus_final_3.md:1`; `gitlog_14d.txt`, commit `3803e6623`.)
- **SNC2 final1** — APROVA sobre `e3ef579a`, pouso `ad28270d8`; pergunta/diff finais exatos [nao verificado]. (`logs/snc2/opus_final_1.txt:1-2`; `gitlog_14d.txt`, commit `ad28270d8`.)
- **SOMBRA (c)(1)+motor feriado** — APROVADO sobre `f40d356e+47691f0b`, pouso `0d234d122`; pacote final exato [nao verificado]. (`logs/sombra_c1/opus_final_1.txt:1`; `gitlog_14d.txt`, commit `0d234d122`.)
- **V2-BUGS-DO-MOTOR i2 final1** — REPROVADO sobre `2cb3a055`; pergunta/diff finais exatos [nao verificado]. (`logs/v2bugs/i2/opus_final.txt:1-5`.)
- **V2-BUGS-DO-MOTOR i2 final v6** — APROVADO sobre `a23831c3`, pouso `ffbda473e`; pergunta/diff finais exatos [nao verificado]. (`logs/v2bugs/i2/opus_final_v6.txt:1-2`; `gitlog_14d.txt`, commit `ffbda473e`.)
- **V3-P5 final** — REPROVADO sobre `c71b1038`; pergunta/diff finais exatos [nao verificado]. (`logs/v3p5/pv/opus_final.txt:1-8`.)

## 11/11 11:35 · sessao b1f0518d314b · - · - · origem=Ronald (tmux)

| candidato | motivo de exclusão |
|---|---|
| CODEXNEG inicial | O prompt e o diff estão nomeados, mas a referência ao anexo de mutantes termina em `logs/codexneg/mutantes_selo`; a identidade completa desse anexo recebido está [nao verificado]. `logs/codexneg/opus_final_prompt.txt:3`. |
| CODEXNEG v4 | O prompt identifica diff e delta, mas a relação de triagens termina truncada em `logs/codex`; pacote integral [nao verificado]. `logs/codexneg/opus_final_v4_prompt.txt:3`. |
| CODEXNEG v5 | O catálogo mostra apenas 12 das 20 linhas do prompt, com referências truncadas; não permite certificar todos os anexos recebidos. `logs/codexneg/opus_final_v5_prompt.txt:6`, `:8`; completude [nao verificado]. |
| GRADE va | Os nomes relativos `v1.r`, `v2.r`, `v1_triagem.md` são utilizáveis literalmente; porém, o ensaio é identificado por diretório, sem enumeração dos arquivos recebidos. `logs/grade_juiz/va/opus_final_brief.md:5`, `:7`; apenas `fase_C.txt` aparece em `:12`; pacote integral [nao verificado]. |
| GRADE vb | Há prompt e diffs nomeados, mas nenhum arquivo de veredito final vb é apresentado no catálogo. `logs/grade_juiz/vb/opus_final_brief.md:1`, `:3`; arquivo do veredito [nao verificado]. |

Zero desses cinco casos têm pacote e veredito integralmente certificáveis pelos recortes fornecidos; isso não afirma inexistência física dos arquivos. [nao verificado]

## 11/11 11:35 · sessao be6027cf7941 · FIO-E0-V4-E4B · construtor · origem=Code

Restaurei a releitura por disputa no C7, mantendo o memo das cargas e o N1 exigindo **2 leituras**. [disputa_emissao.py:2076](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/services/disputa_emissao.py:2076)

A perda ocorreu na **c4v5**; a c4v6 acrescentou a semente do E2. O memo podia divergir após reabertura do período. A cadeia completa, escritores e leitores estão no [recibo:8](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4-v14.txt:8).

Validação: RED na base, três mutantes falhando pela asserção comportamental, 94 testes de memória/protocolo e 40 selos passando. [recibo:105](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4-v14.txt:105) [nao verificado]

O comando ORM completo está no [recibo:149](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4-v14.txt:149). ORM, tetos C8 e suite integral: **[nao verificado]**. [nao verificado]

Sem commit, respeitando a proibição inicial de commit e escrita em `.git*`. [recibo:158](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4-v14.txt:158) [nao verificado]

## 11/11 11:31 · sessao 9c6b12fbf335 · MAPA-ATE-O-20 · censo · origem=Code

**E3-LEITORES: 1 de 10; troca do vigia pendente.** A linha 635 ainda chama `porta_alcancavel_do_dia`; isso corresponde ao **item 4** do PRONTO de E3-TROCA-PELO-DIFF. (`app/chamados/services/vigia_fio.py:635`; `avais_obras.txt:55`, bloco `=== ../FIO-E0-V4-E3/aval_e3_troca_pelo_diff_1111.txt`)

Nos anexos, contei linhas preservando os brancos: `avais_obras.txt:1` começa no primeiro `===`; `relato_topo.txt:1`, no título `# RELATO`. Abaixo, `BACKLOG.md`, `TICKETS.md` e `RELATO.md` significam os arquivos em `app/docs/`. A identidade Git da cópia com o HASH informado é **[não verificado]**.

A busca de PRONTO abrangeu os avais fornecidos, `app/docs/TICKETS.md` e `app/docs/BACKLOG.md`. Resultados únicos em prosa foram aceitos explicitamente; enumerações de decisões, fases ou ordem não viraram listas de PRONTO. (`avais_obras.txt:138`; fontes específicas abaixo)

| obra | X de N (ou PRONTO AUSENTE) | fonte do N (arquivo:linha) | itens provados (com prova:linha) | o que falta, item a item |
|---|---|---|---|---|
| **E3 — LEITORES** | **1 de 10** | `avais_obras.txt:55`, bloco `=== ../FIO-E0-V4-E3/aval_e3_troca_pelo_diff_1111.txt` | **2. Classes V/T/X com número:** V=15, T=0, X=217, discriminado em X1–X4. (`RELATO.md:62`, `RELATO.md:63`, `RELATO.md:64`, `RELATO.md:65`, `RELATO.md:66`) | **1. Sonda com soma 230:** medida foi **232**; divergência explicada, sem contar cumprimento literal. (`RELATO.md:74`)<br>**3. V curada ou zero:** falta prova posterior à cura cobrindo os 15 V; a decomposição anterior é A3/B2/P8/S2. (`RELATO.md:53`; conclusão integral **[não verificado]**)<br>**4. Três chamadores lendo só tarefa_do_dia:** falta a troca; vigia ainda consulta a porta antiga. (`app/chamados/services/vigia_fio.py:635`)<br>**5. Cinco respondedores mortos com certidão:** seguem declarados vivos. (`RELATO.md:99`)<br>**6. Catraca e paridade verdes:** falta prova depois da troca. **[não verificado]**<br>**7. DIFF do alcance antes/depois, por classe:** falta essa medição depois da troca. **[não verificado]**<br>**8. Suíte inteira OK:** há prova da fase `999a7966c`, sem a troca concluída. (`relato_topo.txt:28`; `app/chamados/services/vigia_fio.py:635`)<br>**9. Opus final aprova:** aprovação da mesma fase, sem prova da obra LEITORES completa. (`relato_topo.txt:27`)<br>**10. Pouso:** pousou a fase; falta pouso com os resultados da troca comprovados. (`relato_topo.txt:7`; `avais_obras.txt:138`) |
| **E3 — sublista X1, separada** | **6 de 7 — não somados aos 10** | `avais_obras.txt:37`, bloco `=== ../FIO-E0-V4-E3/aval_e3_x1_folga_1111.txt` | **1. Censo dos 15:** sonda identificada, n=15, rc=0. (`RELATO.md:41`)<br>**2. Cura no ramo:** alteração do cartório descrita na aprovação. (`relato_topo.txt:27`)<br>**3. RED/GREEN + mutante:** base morta, GREEN 14 OK, mutantes X1 7/7. (`relato_topo.txt:31`)<br>**5. Suíte inteira OK:** Ran 15134, rc=0. (`relato_topo.txt:28`)<br>**6. Opus final:** aprovado. (`relato_topo.txt:27`)<br>**7. Pouso:** merge e deploy rc=0 registrados. (`relato_topo.txt:7`) | **4. DIFF da sombra por colaborador e zero trabalho mudando:** falta a discriminação completa. O topo apresenta agregados; o aval dos 35 dias posteriores ao desligamento resolve o corte, mas não apresenta o DIFF completo exigido. (`relato_topo.txt:18`, `relato_topo.txt:21`; `avais_obras.txt:44`) |

Há conflito documental na E3: ORDEM-ATE-O-ZERO referencia **“6 resultados”** de E3-TROCA-PELO-DIFF, mas o próprio aval enumera **10**. Não há seis itens transcritos naquela referência para construir outra contagem. A sublista X1 enumera sete resultados próprios. (`avais_obras.txt:28`, bloco `=== ../FIO-E0-V4-E3/aval_ordem_ate_o_zero_1111.txt`; `avais_obras.txt:55`; `avais_obras.txt:37`)

| obra | X de N (ou PRONTO AUSENTE) | fonte do N (arquivo:linha) | itens provados (com prova:linha) | o que falta, item a item |
|---|---|---|---|---|
| **E4B** | **PRONTO AUSENTE — não numerada** | `BACKLOG.md:1866`, `BACKLOG.md:1875`; `avais_obras.txt:92`, bloco `=== ../E4C/aval_expand_contract.txt`; busca também em `TICKETS.md` | Há GREEN e mutantes da c4, mas a suíte seguinte ficou vermelha; não fornecem N da obra. (`relato_topo.txt:9`, `relato_topo.txt:4`) | Formalizar a lista: o BACKLOG reúne A24, C3, três rodadas e `alarma_em_0` sem numerá-los como PRONTO. O aval ainda exige classe e, limbo, ramo sem porta e backfill da 11 com DIFF; não converti esse conjunto em N. (`BACKLOG.md:1866`; `avais_obras.txt:28`) |

| obra | X de N (ou PRONTO AUSENTE) | fonte do N (arquivo:linha) | itens provados (com prova:linha) | o que falta, item a item |
|---|---|---|---|---|
| **E4C** | **PRONTO AUSENTE — não numerada** | `BACKLOG.md:1866`; `avais_obras.txt:92`, bloco `=== ../E4C/aval_expand_contract.txt`; busca também em `TICKETS.md` | Censo identificado, com amostra 10/10 conferida; prova de preparação, sem contar migração concluída. (`relato_topo.txt:36`) | A prosa reúne **migração dos DIVERGE para o ato** e **selo 2**, sem numeração de PRONTO. Falta lista e prova desses resultados; D04/D05 seguem registradas. O PRONTO do censo conjunto é outra entrega, também não numerada. (`BACKLOG.md:1866`; `relato_topo.txt:36`; `avais_obras.txt:87`, bloco `=== ../E4C/aval_censo_adiantado_1111.txt`) |

| obra | X de N (ou PRONTO AUSENTE) | fonte do N (arquivo:linha) | itens provados (com prova:linha) | o que falta, item a item |
|---|---|---|---|---|
| **E4D** | **0 de 1 — resultado único em prosa** | `BACKLOG.md:1866`; `avais_obras.txt:92`, bloco `=== ../E4C/aval_expand_contract.txt` | Nenhum resultado de CONTRACT comprovado. **[não verificado]** | **1. Parar de escrever a projeção:** falta registro no RELATO de commit/sonda que prove a interrupção da escrita. Retirada física e migration estão separadamente registradas, fora deste resultado. (`BACKLOG.md:1866`, `BACKLOG.md:1867`) |

| obra | X de N (ou PRONTO AUSENTE) | fonte do N (arquivo:linha) | itens provados (com prova:linha) | o que falta, item a item |
|---|---|---|---|---|
| **RAIZ F1** | **PRONTO AUSENTE** | `BACKLOG.md:1883`; `avais_obras.txt:98`, bloco `=== ../RAIA-2-BO-VISIVEL/aval_raiz_f1_um_eleitor_1111.txt`; busca também em `TICKETS.md` | Censo `f0f1.r`, rc=0, identifica três sítios persistentes e três eleitores divergentes. (`RELATO.md:19`) | Os números (1)–(4) do aval organizam decisões/instruções, sem lista de PRONTO. Falta essa lista e prova da implementação do escritor/eleitor único; o censo não a demonstra. (`avais_obras.txt:98`; `RELATO.md:19`) |

| obra | X de N (ou PRONTO AUSENTE) | fonte do N (arquivo:linha) | itens provados (com prova:linha) | o que falta, item a item |
|---|---|---|---|---|
| **RAIZ F2** | **PRONTO AUSENTE — não numerada** | `BACKLOG.md:1883`, `BACKLOG.md:1877`; busca nos avais e em `TICKETS.md` | Há registro de c21 estacionada, c22 não lançada e suíte17 vermelha. (`RELATO.md:27`) | F2=BO-DISPUTA reúne cura, invariante, selo e contadores em prosa. Falta lista numerada e prova de cumprimento de cada resultado; não transformei as fases em itens de PRONTO. (`BACKLOG.md:1883`, `BACKLOG.md:1877`) |

| obra | X de N (ou PRONTO AUSENTE) | fonte do N (arquivo:linha) | itens provados (com prova:linha) | o que falta, item a item |
|---|---|---|---|---|
| **RAIZ F3** | **PRONTO AUSENTE** | `BACKLOG.md:1883`; `avais_obras.txt:87`, bloco `=== ../E4C/aval_censo_adiantado_1111.txt`; `avais_obras.txt:104`, bloco `=== ../RAIA-2-BO-VISIVEL/aval_f4_par_pela_celula_1111.txt`; busca também em `TICKETS.md` | Censo e sonda conjuntos estão registrados. (`relato_topo.txt:37`, `relato_topo.txt:38`) | Falta PRONTO específico da fase. O aval define autor do fecho automático; o topo registra dependência por arquivo comum, sem prova de cura pousada. (`avais_obras.txt:104`; `relato_topo.txt:11`) |

| obra | X de N (ou PRONTO AUSENTE) | fonte do N (arquivo:linha) | itens provados (com prova:linha) | o que falta, item a item |
|---|---|---|---|---|
| **RAIZ F4** | **PRONTO AUSENTE** | `BACKLOG.md:1883`; `avais_obras.txt:102`, bloco `=== ../RAIA-2-BO-VISIVEL/aval_f4_par_pela_celula_1111.txt`; busca também em `TICKETS.md` | A sonda identifica perda do par e a fonte `PARES_PAUSA_DNA`; isso prova o diagnóstico. (`relato_topo.txt:38`) | Falta lista de PRONTO e prova da leitura do par pela célula implementada. O aval define autoridade e proibições, sem enumerar resultados de conclusão. (`avais_obras.txt:102`, `avais_obras.txt:106`) |

| obra | X de N (ou PRONTO AUSENTE) | fonte do N (arquivo:linha) | itens provados (com prova:linha) | o que falta, item a item |
|---|---|---|---|---|
| **RAIZ F5** | **0 de 1 — herdado de E4D** | Equivalência E4d=F5: `BACKLOG.md:1756`; resultado: `BACKLOG.md:1866`, `avais_obras.txt:92`, bloco `=== ../E4C/aval_expand_contract.txt` | Nenhum resultado de CONTRACT comprovado. **[não verificado]** | **1. Parar de escrever a projeção:** falta prova escrita no RELATO. É o mesmo resultado da E4D, sem lista própria adicional nem soma entre obras. (`BACKLOG.md:1756`, `BACKLOG.md:1866`) |

| obra | X de N (ou PRONTO AUSENTE) | fonte do N (arquivo:linha) | itens provados (com prova:linha) | o que falta, item a item |
|---|---|---|---|---|
| **RAIZ F6** | **PRONTO AUSENTE — não numerada** | `BACKLOG.md:1883`; `avais_obras.txt:10`, bloco `=== ../META-18/aval_meta18_duas_raias_1111.txt`; busca também em `TICKETS.md` | Conclusão dos resultados: **[não verificado]** | A prosa exige DRY, DIFF por classe, reversão, lista de exportadas e medição de idade/efeito das duplicadas. São vários resultados sem numeração; faltam lista e provas correspondentes. (`avais_obras.txt:10`) |

| obra | X de N (ou PRONTO AUSENTE) | fonte do N (arquivo:linha) | itens provados (com prova:linha) | o que falta, item a item |
|---|---|---|---|---|
| **RAIZ F7** | **PRONTO AUSENTE** | `BACKLOG.md:1883`; `avais_obras.txt:5`, bloco `=== ../META-18/aval_meta18_duas_raias_1111.txt`; busca também em `TICKETS.md` | Selo F7 verde: **[não verificado]** | “Selo esperado 0” nasce com cada fase, mas essa referência não identifica uma lista de resultados nem a condição concreta medida pelo selo. Falta definição de PRONTO e prova identificável. (`avais_obras.txt:5`) |

| obra | X de N (ou PRONTO AUSENTE) | fonte do N (arquivo:linha) | itens provados (com prova:linha) | o que falta, item a item |
|---|---|---|---|---|
| **O168** | **PRONTO AUSENTE — não numerada** | `BACKLOG.md:1395`; busca nos avais e em `TICKETS.md` | Censo/sonda registrados; `CSF_RETRATADO_30d` consta **não mensurável, null**, sem prova de zero. (`RELATO.md:903`) | A prosa reúne guarda compartilhada, aplicação no evento, retirada posterior do `--executar` e `CSF_RETRATADO=0`. Falta lista numerada e prova desses resultados; “nada construído” está registrado. (`BACKLOG.md:1395`; `RELATO.md:173`) |

| obra | X de N (ou PRONTO AUSENTE) | fonte do N (arquivo:linha) | itens provados (com prova:linha) | o que falta, item a item |
|---|---|---|---|---|
| **LASTRO** | **PRONTO AUSENTE para a obra inteira** | `BACKLOG.md:1731`, `BACKLOG.md:1760`, `BACKLOG.md:1762`, `BACKLOG.md:1807`; busca nos avais e em `TICKETS.md` | Parte 2: deploy rc=0 e `sem_evento=0`. (`RELATO.md:863`)<br>Parte 3: pouso registrado. (`RELATO.md:799`) | Falta lista de PRONTO da LASTRO inteira. Partes 2/3 e FOLGA-FORA-DO-CATALOGO têm registros separados; não somei suas entregas para fabricar N. (`BACKLOG.md:1760`, `BACKLOG.md:1762`, `BACKLOG.md:1807`) | [nao verificado]

| obra | X de N (ou PRONTO AUSENTE) | fonte do N (arquivo:linha) | itens provados (com prova:linha) | o que falta, item a item |
|---|---|---|---|---|
| **O169-FECHADA** | **PRONTO AUSENTE — não numerada** | `BACKLOG.md:1921`; `avais_obras.txt:132`, bloco `=== ../O169-FECHADA/aval_tres_respostas_1110_0040.txt`; busca também em `TICKETS.md` | Suíte v5 verde; GREEN/mutantes registrados; merge/deploy, APPLY 3 e DRY seguinte vazio. (`relato_topo.txt:8`, `relato_topo.txt:12`, `relato_topo.txt:6`) | Falta lista numerada que permita N. O `(3)` do aval é a terceira decisão de um aval misto; não é uma lista de três resultados da O169. O nome vigente do contador é `cluster_encerrado_sem_lastro`. (`avais_obras.txt:132`) |

| obra | X de N (ou PRONTO AUSENTE) | fonte do N (arquivo:linha) | itens provados (com prova:linha) | o que falta, item a item |
|---|---|---|---|---|
| **UI-PAINEL** | **PRONTO AUSENTE — quatro resultados declarados, não numerados** | `avais_obras.txt:124`, bloco `=== ../UI-PAINEL/aval_ui_painel_raspado.txt`; busca também em `TICKETS.md`/`BACKLOG.md` | O GREEN falhou em quatro testes; mutantes não medidos porque caíram nas mesmas falhas. (`relato_topo.txt:5`) | O PRONTO lista, sem números: **faixa na aba Mensagens; contador do botão; função única com três leitores; selo API×painel verde com mutante morto**. Falta numeração e prova de cada resultado. A enumeração MUDA cobre somente 1–3, sem incluir o selo; não a completei por inferência. (`avais_obras.txt:117`, `avais_obras.txt:118`, `avais_obras.txt:119`, `avais_obras.txt:124`) |

| obra | X de N (ou PRONTO AUSENTE) | fonte do N (arquivo:linha) | itens provados (com prova:linha) | o que falta, item a item |
|---|---|---|---|---|
| **BO-ROTULO-ENCERRADO** | **0 de 1 — resultado único em prosa** | `BACKLOG.md:1884` | Há mapeamento da fonte do modal e dos leitores; não prova da troca concluída. (`RELATO.md:7699`) | **1. Rótulo lido da mesma fonte do modal, sem mudar estado:** falta prova escrita da alteração e do comportamento do seletor para resolvido/fechado. (`BACKLOG.md:1884`; conclusão **[não verificado]**) |

| obra | X de N (ou PRONTO AUSENTE) | fonte do N (arquivo:linha) | itens provados (com prova:linha) | o que falta, item a item |
|---|---|---|---|---|
| **TRIAGEM-OFFLINE-SEM-CAMERA** | **PRONTO AUSENTE — não numerada** | `avais_obras.txt:95`, bloco `=== ../RAIA-2-BO-VISIVEL/aval_raia2_bo_visivel_1111.txt`; contexto em `BACKLOG.md:1889`; busca também em `TICKETS.md`/`BACKLOG.md` | Consta na fila depois de BO-ROTULO. (`relato_topo.txt:17`) | Falta lista de PRONTO. O aval exige juiz único de selfie impeditiva e smoke nas duas cascas; conclusão desses resultados e preservação da batida offline sem câmera: **[não verificado]**. (`avais_obras.txt:95`) |

| obra | X de N (ou PRONTO AUSENTE) | fonte do N (arquivo:linha) | itens provados (com prova:linha) | o que falta, item a item |
|---|---|---|---|---|
| **LOTE-VALIDAR-REGRESSAO-0311** | **PRONTO AUSENTE** | `BACKLOG.md:1892`; `avais_obras.txt:95`, bloco `=== ../RAIA-2-BO-VISIVEL/aval_raia2_bo_visivel_1111.txt`; busca também em `TICKETS.md` | Medição citada: custo por referência estável, crescimento pelo tamanho do lote e reoferta de recusados; nenhuma cura pontual, retorno a LOTE-VALIDAR-VIRA-JOB. Prova identificada: `sonda_lote0311.r`. (`relato_topo.txt:10`) | Falta lista de resultados de conclusão. “Medir a causa antes” é precondição; a medição realizada não autoriza inventar N nem declarar a obra encerrada. (`BACKLOG.md:1892`; `avais_obras.txt:95`, `avais_obras.txt:138`) |

**Obras PRONTO AUSENTE:**

- **E4B e E4C** — prosa com múltiplos resultados, sem lista numerada. (`BACKLOG.md:1866`)
- **RAIZ F1, F2, F3, F4, F6 e F7** — sem lista específica de PRONTO nas fontes examinadas. (`BACKLOG.md:1883`; `avais_obras.txt:98`, `avais_obras.txt:87`, `avais_obras.txt:102`, `avais_obras.txt:10`, `avais_obras.txt:5`)
- **O168**. (`BACKLOG.md:1395`)
- **LASTRO**. (`BACKLOG.md:1760`, `BACKLOG.md:1762`, `BACKLOG.md:1807`)
- **O169-FECHADA**. (`BACKLOG.md:1921`; `avais_obras.txt:132`)
- **UI-PAINEL** — quatro resultados declarados, não numerados. (`avais_obras.txt:124`)
- **TRIAGEM-OFFLINE-SEM-CAMERA**. (`avais_obras.txt:95`)
- **LOTE-VALIDAR-REGRESSAO-0311**. (`BACKLOG.md:1892`; `avais_obras.txt:95`)

## 11/11 11:28 · sessao b11c41e475b0 · - · - · origem=Ronald (tmux)

Censo concluído só por leitura dos três documentos do HEAD e anexos do prompt. As linhas do BACKLOG do anexo estão deslocadas em relação à cópia; usar abaixo as linhas reais do HEAD.

| Obra | Lista de PRONTO encontrada | Fonte HEAD | Prova escrita disponível / falta |
|---|---|---|---|
| O168 | Não há lista numerada. A prosa reúne vários resultados: guarda compartilhada, valer no evento, retirada posterior do `--executar`, `CSF_RETRATADO=0`; portanto não converter em N=1. | BACKLOG.md:1395; BACKLOG.md:1960 | RELATO.md:903 registra censo/sonda: guarda só no supra; `CSF_RETRATADO_30d` não mensurável, `null`, não zero. RELATO.md:173 diz nada construído. Falta lista formal de PRONTO e provas dos resultados. |
| LASTRO | Não há lista única numerada de PRONTO para a obra inteira. Há partes e complementos separados; não somar. | BACKLOG.md:1731; BACKLOG.md:1760; BACKLOG.md:1762; BACKLOG.md:1807 | Fase 2: construção, RED/GREEN, mutantes, sombra e suíte em RELATO.md:860; opus em :861; pouso em :862; deploy e `sem_evento=0` em :863. Parte 3 pousou, opus e suíte verdes em RELATO.md:799. Essas provas não dão N da LASTRO inteira. Folga fora do catálogo ainda foi encaminhada a censo em RELATO.md:7353. | [nao verificado]
| O169-FECHADA | Não há lista numerada de PRONTO. BACKLOG tem contador + mutante + reparo do legado; múltiplos resultados sem N. O `(3)` do aval TRES RESPOSTAS é a terceira decisão de um aval misto, não três itens PRONTO da obra. | BACKLOG.md:1921 | Anexo `relato_topo.txt`, linha “O169-FECHADA NO AR 10:57”: merge `ca978f65d`, deploy rc0, DRY 3, APPLY 3, DRY seguinte vazio. Suíte v5 e mutantes também no topo. Falta N autorizado; não escrever X de N com base no pouso. Nome antigo `fechados_por_idade` do BACKLOG foi substituído por `cluster_encerrado_sem_lastro` no aval anexado; mudança explicada em RELATO.md:194. |
| UI-PAINEL | Aval traz **“PRONTO (4 resultados)”**, mas os quatro estão separados por `;`, **sem numeração**: faixa; contador; função única com três leitores; selo API/painel verde com mutante morto. Pela exigência literal do usuário, informar **“não numerada, quatro resultados declarados”**, sem inventar N a partir dela. `MUDA` numera somente 1–3 e não cobre o selo. | Não há ocorrência específica em TICKETS/BACKLOG HEAD; fonte é bloco `=== ../UI-PAINEL/aval_ui_painel_raspado.txt` do anexo. | `relato_topo.txt`, linha “UI-PAINEL v1 ORM 10:55”: quatro falhas no GREEN; mutantes não medidos porque caem nas mesmas falhas. Não prova nenhum dos quatro resultados finalizados. Construção iniciada não basta. |
| BO-ROTULO-ENCERRADO | Nenhuma lista numerada. **Candidato à exceção N=1**: “cura = rótulo lido da mesma fonte do modal, sem mexer em estado” é um único resultado observável, embora não venha rotulado PRONTO. Se aceitar prosa de escopo como lista singular, usar **0 de 1**, sem somar outras exigências. | BACKLOG.md:1884 | RELATO.md:7699 só registra mapa, fonte modal e 13 leitores/escritores; não comprova troca. Topo põe a obra depois do pouso O169. Falta prova de que seletor lê a mesma fonte do modal e distingue resolvido/fechado. | [nao verificado]
| TRIAGEM-OFFLINE-SEM-CAMERA | Sem lista numerada. Aval exige juiz único online/offline/v2 e smoke nas duas cascas: vários resultados, não N=1. | Entrada específica só no anexo; BACKLOG.md:1889 contém “selfie impeditiva” dentro do resto da classe, sem PRONTO. | Topo a mantém na fila depois de BO-ROTULO. Não há prova escrita de cura/smoke. Falta lista e prova de admissibilidade única, batida preservada offline sem câmera e smoke das duas cascas. |
| LOTE-VALIDAR-REGRESSAO-0311 | Sem lista numerada de PRONTO. BACKLOG “medir a causa antes” é precondição; aval RAIA2 determina conduta condicional, não enumera resultados de conclusão. | BACKLOG.md:1892 | Medição comprovada no topo, linha “LOTE-VALIDAR-REGRESSAO-0311 MEDIDA 10:3x”, com `logs/RAIA-2-BO-VISIVEL/sonda_lote0311.r`: custo por referência estável; aumento pelo lote e reoferta de recusados; nenhuma cura pontual; volta à LOTE-VALIDAR-VIRA-JOB. RELATO.md:7695 era o estado anterior: causa por medir. Não transformar essa medição em X de N sem lista. |

Busquei todas as ocorrências dos nomes e dos termos `PRONTO`/`resultados` em `app/docs/TICKETS.md` (1323 linhas), `app/docs/BACKLOG.md` (2339 linhas) e `app/docs/RELATO.md` (7726 linhas); não localizei outra lista numerada para essas sete obras.

Recomendação estrita para a consolidação: PRONTO AUSENTE para O168, LASTRO, O169-FECHADA, UI-PAINEL, TRIAGEM e LOTE; BO-ROTULO pode ser **0 de 1** apenas pela exceção expressa de resultado único, com fonte BACKLOG.md:1884.

## 11/11 11:28 · sessao c7cd088f0fd1 · - · - · origem=Ronald (tmux)

E3-LEITORES: **1 de 10**, em leitura estrita da lista E3-TROCA-PELO-DIFF. Só o item 2 tem prova completa inequívoca: V=15, T=0, X=217, discriminados em `app/docs/RELATO.md:62–66`.

- Item 1: aval exige soma 230; medida foi 232. Divergência explicada em `app/docs/RELATO.md:74`, mas não conto cumprimento literal.
- Item 3: falta medida pós-cura demonstrando solução dos 15 V. A decomposição A3/B2/P8/S2 está em `app/docs/RELATO.md:53`; a entrada de A/S no c8b está em `app/docs/RELATO.md:35` e `:38`; entrega c8c genérica em `:9` não demonstra o resultado dos 15.
- Item 4: falta trocar os três chamadores. Confirmado no código: `app/chamados/services/vigia_fio.py:635` ainda chama `porta_alcancavel_do_dia`.
- Item 5: falta certidão dos cinco respondedores. Sua sobrevivência está declarada em `app/docs/RELATO.md:238`.
- Item 6: falta prova da catraca “respondedores=1” e paridade após troca.
- Item 7: falta DIFF de alcance antes/depois da troca por classe.
- Itens 8–10: suite, opus e pouso do topo vivo demonstram a fase c8+X1, não o fechamento da obra LEITORES com a troca concluída. [nao verificado]

E3-X1 tem **lista separada de 7 itens**, sem somar ao N=10. Recomendo **6 de 7**:

1. Censo dos 15: `app/docs/RELATO.md:41`.
2. Cura no ramo: relato_topo, linha do opus final 999a7966c.
3. RED/GREEN e mutantes: relato_topo, linha e3fix11 ORM 09:21.
4. **Falta** DIFF completo por colaborador, demonstrando zero dia de trabalho exigível mudando. O topo dá agregados e os 35 pós-desligamento; o aval libera esses 35, mas não substitui a discriminação exigida.
5. Suite inteira: relato_topo, linha suite pre-opus 09:46. [nao verificado]
6. Opus final: relato_topo, linha opus final 09:54.
7. Pouso: relato_topo, linha FIO-E0-V4-E3 NO AR 11:0x. [nao verificado]

E4B: **PRONTO AUSENTE**. `app/docs/BACKLOG.md:1866` traz prosa composta — A24, C3, três rodadas, alarma — sem lista numerada. `:1875` descreve backfill, sem fornecer lista de PRONTO. Não há lista pertinente em TICKETS.

E4C: **PRONTO AUSENTE**. `app/docs/BACKLOG.md:1866` traz dois resultados em prosa: migração dos DIVERGE e selo 2. O aval do censo adiantado é entrega conjunta de censo, não PRONTO da migração.

E4D: **0 de 1**, caso se aplique a exceção de resultado único em prosa: parar de escrever a projeção, `app/docs/BACKLOG.md:1866` e aval E4-EXPAND-CONTRACT. Não há prova escrita de conclusão do CONTRACT no RELATO fornecido.

A referência HEAD correta de E3-LEITORES é `app/docs/BACKLOG.md:1865`; as linhas do anexo backlog_obras não correspondem à numeração da cópia.

## 11/11 11:28 · sessao 0cccfd47af33 · - · - · origem=Ronald (tmux)

Censo RAIZ concluído, sem escrita. As linhas abaixo são do HEAD da cópia; diferem do anexo `backlog_obras.txt`. (`app/docs/BACKLOG.md:1883`)

| Fase | PRONTO encontrado | Prova / limite |
|---|---|---|
| RAIZ F1 | **PRONTO AUSENTE** específico. A sequência chama F1 de “matriz”, sem lista de resultados. O aval UM-ELEITOR enumera decisões de implementação, não PRONTO. (`app/docs/BACKLOG.md:1883`; anexo `aval_raiz_f1_um_eleitor_1111.txt`) | Existe censo `f0f1.r`, rc 0, com 3 sítios persistentes e 3 eleitores divergentes; isso prova o censo, não a implementação do escritor/eleitor único. (`app/docs/RELATO.md:19`) |
| RAIZ F2 | **PRONTO AUSENTE** específico. F2 é BO-DISPUTA; sua linha mistura cura, invariante, selo e contadores em prosa, sem lista PRONTO numerada. (`app/docs/BACKLOG.md:1883`; `app/docs/BACKLOG.md:1877`) | c21 estacionada, c22 não lançada; suite17 terminou vermelha. (`app/docs/RELATO.md:27`) |
| RAIZ F3 | **PRONTO AUSENTE** de produto. Há o censo conjunto F3/F4 no anexo; seu PRONTO é entrega de leitura, não encerramento da fase. (`app/docs/BACKLOG.md:1883`; anexo `aval_censo_adiantado_1111.txt`) | O topo fornecido relata censo, sonda e bloqueio por arquivo comum; não relata cura pousada. (anexo `relato_topo.txt`, linhas a numerar pelo pai) |
| RAIZ F4 | **PRONTO AUSENTE** de produto. O aval PAR-PELA-CELULA define autoridade e proibições; não enumera PRONTO. A linha BUG-PAR descreve ato do par, RED e DRY em prosa. (`app/docs/BACKLOG.md:1929`; anexo `aval_f4_par_pela_celula_1111.txt`) | Censo/sonda do topo provam descoberta da fonte `PARES_PAUSA_DNA`, não implementação. (anexo `relato_topo.txt`, linhas a numerar) |
| RAIZ F5 | **Sem lista própria.** Pode herdar **N=1** de E4d, se aplicado o critério de resultado único em prosa: “parar de escrever a projeção”. A equivalência E4d = F5 está explícita. (`app/docs/BACKLOG.md:1756`; `app/docs/BACKLOG.md:1866`) | **X=0**: nenhuma prova escrita de contract concluído encontrada no RELATO; o topo mantém E4c dependente da E4b. (anexo `relato_topo.txt`, linhas a numerar) |
| RAIZ F6 | **PRONTO AUSENTE** numerado. O aval META descreve vários resultados sem numeração: DRY, DIFF por classe, reversão, lista de exportadas e medição das duplicadas; não cabe transformar tudo em um item. (`app/docs/BACKLOG.md:1883`; anexo `aval_meta18_duas_raias_1111.txt`) | Não há prova de conclusão desses resultados no RELATO pesquisado. [não verificado] |
| RAIZ F7 | **PRONTO AUSENTE** específico. “Selo esperado 0 nasce com cada fase” não fornece lista nem define, nessa frase, a condição medida. (`app/docs/BACKLOG.md:1883`; anexo `aval_meta18_duas_raias_1111.txt`) | Não há prova citada de selo RAIZ F7 verde no material fornecido. [não verificado] |

Fontes locais pesquisadas integralmente: `app/docs/TICKETS.md` (1323 linhas), `app/docs/BACKLOG.md` (2339 linhas), `app/docs/RELATO.md` (7726 linhas). `TICKETS.md` não tem entradas RAIZ-DA-DISPUTA, BO-DISPUTA-SEM-PERGUNTA, BUG-PAR ou DISPUTA-ZUMBI. [não verificado — resultado negativo da busca]

Atenção para não converter a ordem F0→F7 em sete itens de PRONTO: ela descreve fases da obra. (`app/docs/BACKLOG.md:1883`)

## 11/11 10:57 · sessao 5a702a9c8f16 · MAPA-20 · verificador · origem=Code

REPROVADO para pouso [nao verificado]

Referências `diff:arquivo:linha` usam as linhas de destino do patch fornecido; `base:arquivo:linha`, a cópia disponível. A identidade Git dessa cópia com o HASH informado ficou [nao verificado].

1. **Triagem 1–5: três correções fecham o achado; duas permanecem parciais.**

   | Achado | Conferência |
   |---|---|
   | **1 — nome da obra** | **Fecha.** `UI-PAINEL` foi corrigido e as 18 obras correspondem nominalmente ao aval. (`diff:bin/mapa20_valida.py:19–24`; `aval_mapa_ate_o_20_1111.txt:1`, item 1.) |
   | **2 — tabela torta** | **Parcial.** Há checagem de largura das linhas, duplicatas e células vazias, mas continuam passando separador com largura errada e tabela ainda dentro de cerca; exemplos no item 3. (`diff:bin/mapa20_valida.py:47–52,64–67,82–94`.) |
   | **3 — retido não atômico/saída antecipada** | **Fecha os dois defeitos indicados.** A re-raspagem escreve ao lado, troca por `mv`, e a falha segue para os passos seguintes com `MAPA_RC=1`. O `rm` é tentado, mas seu sucesso não é conferido. (`diff:bin/relato.sh:99–112`.) |
   | **4 — falta de foto** | **Fecha no código.** O validador lê `$TMP/mapa20.md`, e esse mesmo arquivo é a origem entregue ao raspador; regravar a fonte depois não troca o documento deste ciclo. (`diff:bin/relato.sh:96–97,141`.) |
   | **5 — selo por texto/mutantes** | **Parcial.** Agora há execução de cenários, mas sobrevivem mutantes plausíveis; exemplos no item 4. A alegação de “12 mortos” não veio acompanhada dos mutantes/resultados: [nao verificado]. (`diff:bin/tests/test_mapa20_valida.sh:54–109`.) |

2. **Todos os `exit` de `relato.sh` após o diff: 12 comandos.**

   | Local | Valor e condição |
   |---|---|
   | `base:bin/relato.sh:27` | `1`: falha da trava. |
   | `base:bin/relato.sh:28` | `1`: destino sem `.git`. |
   | `base:bin/relato.sh:32` | `1`: falha da lista de nomes. |
   | `base:bin/relato.sh:35` | `1`: falha do BACKLOG-TOPO. |
   | `base:bin/relato.sh:38` | `1`: falha do ESTADO. |
   | `base:bin/relato.sh:42` | `1`: falha da SESSAO. |
   | `base:bin/relato.sh:86` | `1`: falha da raspagem do RELATO retido. |
   | `diff:bin/relato.sh:126` | `1`: falha de `gerar_avais.py`. |
   | `diff:bin/relato.sh:141` | `1`: falha da raspagem geral. |
   | `diff:bin/relato.sh:155` | `$MAPA_RC`, portanto `0` ou `1`: índice sem mudança. |
   | `diff:bin/relato.sh:161` | `1`: falha da expressão `git push && echo`. | [nao verificado]
   | `diff:bin/relato.sh:162` | `$MAPA_RC`, portanto `0` ou `1`: saída final. |

   **Validador executado vermelho → rc final 0:** não há esse caminho no fluxo mostrado. O ramo fixa `MAPA_RC=1`, nenhuma atribuição posterior o zera, e ambas as saídas normais o devolvem. **Validador verde → rc 1 sem outra falha:** também não há; as saídas posteriores com `1` dependem de outra falha. (`diff:bin/relato.sh:94–99,126,141,155,161–162`.) [nao verificado]

   **O público pode perder MAPA-20 sem o validador falhar:** sim, **sem ele executar**. Fonte ausente/ilegível → `cp` falha → o `&&` pula Python → re-raspagem do público falha → `rm` → remoção rastreada entra no commit. O ciclo sai `1`; isso não constitui validador verde. (`diff:bin/relato.sh:96–108,149–162`.) [nao verificado]

   Continua possível falha de `git commit` seguida de push bem-sucedido terminar `0` com mapa verde; existe outra falha nesse caso. A ausência de conferência de `cd`/commit já era anterior, conforme o descarte 6, e não foi curada pelo diff. (`base:bin/relato.sh:21,120–132`; `diff:bin/relato.sh:144–162`; `triagem_v1.txt:7`.) [nao verificado]

3. **Validador: universo correto, parser ainda insuficiente.**

   Nome a nome, a tupla contém: **E3, E4B, E4C, E4D; RAIZ F1, F2, F3, F4, F5, F6, F7; O168; LASTRO; O169-FECHADA; UI-PAINEL; BO-ROTULO-ENCERRADO; TRIAGEM-OFFLINE-SEM-CAMERA; LOTE-VALIDAR-REGRESSAO-0311**. São as 18 do aval; `SEM DONO` é exigido separadamente. (`diff:bin/mapa20_valida.py:19–25,92–94`; `aval_mapa_ate_o_20_1111.txt:1`, itens 1–2.)

   Nos exemplos seguintes, **G** significa uma tabela completa com cabeçalho de três colunas, separador de três colunas e as 19 linhas obrigatórias preenchidas. Essa forma passa no próprio selo. (`diff:bin/tests/test_mapa20_valida.sh:20–29`.)

   **Aceita indevidamente:**

   - **Separador estreito:** em G, substituir somente o separador por `|---|---|` continua verde. O código confere o conteúdo das células do separador, mas não sua quantidade contra o cabeçalho. (`diff:bin/mapa20_valida.py:64–67`.) [nao verificado]
   - **Tabela dentro de cerca:** quatro crases de abertura, depois três crases, depois G e finalmente quatro crases. Continua verde, embora as três crases intermediárias não encerrem a cerca de quatro. O mesmo ocorre com quatro/três tils. O parser guarda só o caractere da cerca. (`diff:bin/mapa20_valida.py:47–52`.) [nao verificado]
   - **Fechamento com texto:** uma linha de três crases seguida de `python` também é tratada como fechamento. (`diff:bin/mapa20_valida.py:47–52`.)
   - **Cabeçalho vazio:** `| | | |`, separador normal e as 19 linhas preenchidas passam; o cabeçalho só tem sua quantidade de colunas examinada. (`diff:bin/mapa20_valida.py:79–89`.)
   - **Pontuação sem dado:** `…`, `!!!` e `___` passam. O filtro reconhece apenas o conjunto restrito `-?.–— `, além da retirada de asteriscos/crases; `...` reprova, mas `…` passa. (`diff:bin/mapa20_valida.py:27,70–71`.)

   **Recusa indevidamente formas de apresentação equivalentes:**

   - G sem os pipes externos: retorna `mapa sem tabela`, porque só coleta linhas iniciadas por `|`. (`diff:bin/mapa20_valida.py:58–63`.)
   - Obra escrita `_E3_`: acusa E3 ausente, enquanto `**E3**` é normalizado. A limpeza trata asteriscos/crases, mas não underscores. (`diff:bin/mapa20_valida.py:30–31,92–94`.)

   **Escape:** `a\|b` funciona como uma célula. Entretanto, a substituição é cega: na linha `| E3 | caminho\\| nao medido |`, com **duas barras reais**, funde as duas últimas células e acusa largura 2 contra 3; não examina a paridade das barras. (`diff:bin/mapa20_valida.py:34–39,87–88`.)

   **Limite de conformidade:** uma tabela de apenas `obra | dado`, com todas as linhas dizendo `nao medido`, também passa. Isso satisfaz o mínimo implementado, mas não comprova as colunas específicas, a medição ou o tamanho de SEM DONO exigidos pelo aval. (`diff:bin/mapa20_valida.py:79–94`; `aval_mapa_ate_o_20_1111.txt:1`, itens 1–2.)

4. **O selo não fica verde com recorte literalmente vazio, mas ainda deixa regressões passarem.** [nao verificado]

   Os três `awk` dependem de prefixos textuais dos marcadores. Se um recorte ficar vazio, as guardas por `mapa20_valida.py`, `raspar_publico.py` e `git push` incrementam `FALHAS`; portanto, a hipótese específica “bloco vazio e selo verde” está barrada. As guardas, porém, não comprovam os limites exatos dos recortes. (`diff:bin/tests/test_mapa20_valida.sh:54–58,112`.) [nao verificado]

   **Mutante plausível que passa:** retirar `|~{3,}` da regex de cercas do validador. A parte (a) só testa cerca de crases; os seis cenários da parte (b) também não usam tils. O mutante preserva esses resultados e aceita G inteiramente cercado por `~~~`. (`diff:bin/mapa20_valida.py:47`; `diff:bin/tests/test_mapa20_valida.sh:29–49,83–109`.)

   **Outro mutante sobrevivente:** validar e publicar diretamente `logs/MAPA-20/mapa.md`, deixando a foto sem uso. Os seis cenários não alteram a fonte entre validação e raspagem; assim, não distinguem esse mutante da correção do achado 4. (`diff:bin/relato.sh:96–97`; `diff:bin/tests/test_mapa20_valida.sh:83–109`.)

   A reprodução em memória dos 48 textos da parte (a) não encontrou divergências; o caso ausente é tratado com rc 1. Execução integral do selo, incluindo seus repositórios temporários: [nao verificado]. (`diff:bin/tests/test_mapa20_valida.sh:29–49`; `diff:bin/mapa20_valida.py:99–104`.)

5. **Selos vizinhos: nenhuma quebra demonstrada nas verificações que leem `relato.sh`.**

   - **`test_codex_estado`:** o recorte começa em `CODEX_OK=""`, depois do novo 2f, e termina no passo 3. Os novos comandos não entram na região examinada por `exit`. (`base:bin/tests/test_codex_estado.sh:104–111`; `diff:bin/relato.sh:89–122`.)
   - **`test_relato_publica_claude_md`:** mantém o par CLAUDE e o pathspec. O novo `git add` de MAPA está indentado e não aumenta a contagem de linhas iniciadas na coluna 1 por `git add -A `. (`base:bin/tests/test_relato_publica_claude_md.sh:13–20`; `diff:bin/relato.sh:141,145,151,160`.)
   - **`test_codex_estado_evento`:** continuam as chamadas de `codex_estado.sh` e `relato_nomes.sh`, sem introduzir `gerar_codex_estado.py` inline. (`base:bin/tests/test_codex_estado_evento.sh:27–30`; `base:bin/relato.sh:32,97`.)
   - **`test_relato_afirma_com_prova`:** a chamada exigida permanece. (`base:bin/tests/test_relato_afirma_com_prova.sh:43–45`; `base:bin/relato.sh:55`.)
   - **`test_publicacao_sem_nome`:** o universo vem de `git ls-files` do público; MAPA rastreado entra automaticamente. Não depende de um recorte do script. (`base:bin/tests/test_publicacao_sem_nome.sh:58–72`.)

   Execução completa desses cinco selos e resultado de privacidade em produção: [nao verificado].

6. **Quem escreve e quem lê.**

   | Campo/arquivo | Escritor | Leitor |
   |---|---|---|
   | `MAPA_RC` | Shell: inicializa `0`; falha de cópia **ou** validação fixa `1`. | Saídas sem mudança e final. (`diff:bin/relato.sh:94–99,155,162`.) |
   | `MAPA_PAR` | Shell: vazio ou par foto→público. | Argumento opcional do raspador geral. (`diff:bin/relato.sh:94,97,141`.) |
   | `MAPA_PATH` | Shell: vazio ou nome após `git add` bem-sucedido. | Mensagem e pathspec do commit. (`diff:bin/relato.sh:149–160`.) | [nao verificado]
   | `$TMP/mapa20.md` | `cp` da fonte; limpeza pelo trap. | Validador e raspador geral. (`diff:bin/relato.sh:96–97,141`; `base:bin/relato.sh:25`.) |
   | `logs/MAPA-20/mapa.md` | “Sessão por evento” está declarado no comentário; escritor efetivo e gatilhos: [nao verificado]. | `cp`. (`diff:bin/relato.sh:89–96`.) |
   | Público `MAPA-20.md` | Raspador geral; no retido, raspador→arquivo ao lado→`mv`; falha tenta `rm`. | Re-raspagem do retido e Git para inclusão/remoção/publicação. (`base:bin/raspar_publico.py:23–40`; `diff:bin/relato.sh:102–108,141,150–161`.) |

7. **Classes históricas, uma a uma.**

   | Classe | Resultado |
   |---|---|
   | **2º juiz** | Não surge segundo juiz de nomes: raspagem reutiliza `nome_no_texto`. A lista independente no teste serve de fixture contra troca de obra. (`base:bin/raspar_publico.py:15–16,31,36`; `diff:bin/tests/test_mapa20_valida.sh:3–4,20–22`.) | [nao verificado]
   | **2º escritor fora da porta** | Os dois caminhos de escrita pública estão no mesmo publicador, sob a trava existente. Escritor efetivo da fonte: [nao verificado]. (`base:bin/relato.sh:27`; `diff:bin/relato.sh:96–112,141`.) | [nao verificado]
   | **Relógio fora da borda** | Nenhum relógio de decisão novo; o `date` permanece na mensagem de commit. O timer de publicação já existe. (`base:bin/relato.sh:130`; `diff:bin/relato.sh:159`; `base:bin/hasner-relato.timer:5–7`.) | [nao verificado]
   | **Derivação paralela de fonte única** | O publicador consome a foto validada e conserva o raspador existente. Reuso dos censos e medidas pela função real: [nao verificado]. (`diff:bin/relato.sh:96–97,141`; `aval_mapa_ate_o_20_1111.txt:1`, item 1.) | [nao verificado]
   | **Selo sem mordida** | **Presente:** cercas de tils e a propriedade da foto admitem os mutantes descritos no item 4. (`diff:bin/tests/test_mapa20_valida.sh:29–49,83–109`.) |
   | **Ordem de trava** | Não aparece nova aquisição. A chamada `codex_estado.sh --gerar` retorna antes da trava do ramo evento, evitando autoespera nessa costura. (`base:bin/relato.sh:27,97`; `base:bin/codex_estado.sh:54–56,65–66`.) |
   | **Pouso antes do pré-requisito** | O comentário referencia contrato, mas contrato efetivo, mapa completo/publicado, suíte e opus desta volta: [nao verificado]. Esses requisitos constam do aval e das regras da base. (`diff:bin/relato.sh:93`; `aval_mapa_ate_o_20_1111.txt:1`; `base:CLAUDE.md:377–381,394`.) | [nao verificado]
   | **Campo com dois significados** | `MAPA_RC` significa falha da etapa MAPA, não rc puro do Python; `MAPA_PATH` também representa remoção, não prova de mapa novo publicado. O fluxo é coerente com esses usos, mas nenhum deles comprova “validador executou” ou “mapa novo está no ar”. (`diff:bin/relato.sh:94–99,148–162`.) | [nao verificado]

## 11/11 10:55 · sessao f8e3618b4d7e · - · - · origem=Ronald (tmux)

Contraexemplos confirmados em memória, sem criar arquivos:

- **Aceita tabela torta**: cabeçalho de três colunas com separador de duas; retorna `P=[]`, pois só verifica o conteúdo das células do separador, sem comparar sua largura. Também aceita `|- - -|- - -|- - -|`. (`diff bin/mapa20_valida.py:64-67`)
- **Aceita tabela ainda cercada**: abertura de quatro crases, fechamento de três e mapa completo; idem com quatro/três tils. Também considera ` ```python ` fechamento. Guarda somente o caractere, sem comprimento nem condição sobre texto restante. (`diff bin/mapa20_valida.py:47-52`)
- **Aceita pontuação sem dado**: células `!!!`, `___`, `…`; o conjunto só contempla `-?.–— ` e a limpeza adicional de asteriscos/crases. (`diff bin/mapa20_valida.py:27`, `:70-71`)
- **Aceita cabeçalho vazio** `| | | |` e mapa de somente duas colunas com todas as 19 linhas. Isso satisfaz o mínimo implementado, mas não comprova as colunas exigidas pelo aval. (`diff bin/mapa20_valida.py:79-89`; `aval_mapa_ate_o_20_1111.txt:item 1`)
- **Recusa tabela sem pipes externos**, produzindo `mapa sem tabela`. (`diff bin/mapa20_valida.py:58-63`)
- **Recusa `_E3_`**, embora aceite `**E3**`; sua normalização não remove underscores. (`diff bin/mapa20_valida.py:30-31`, `:92-94`)
- **Recusa separador precedido por duas barras reais**: na linha `| E3 | caminho\\| nao medido |`, funde as duas últimas células e acusa `linha E3: 2 coluna(s), o cabecalho tem 3`. Não considera a paridade das barras anteriores ao pipe. (`diff bin/mapa20_valida.py:38-39`, `:87-88`)
- **Pipe simples escapado funciona**: `a\|b` permanece uma célula e o mapa completo fica verde. (`diff bin/mapa20_valida.py:34-39`)

A tupla corresponde nominalmente às 18 obras do aval, incluindo `UI-PAINEL`; `SEM DONO` é exigido separadamente. (`diff bin/mapa20_valida.py:19-25`, `:92`; `aval_mapa_ate_o_20_1111.txt:item 1-2`)

Reproduzi os 48 mapas da fixture: nenhuma divergência; o caso adicional de arquivo ausente gera a exceção capturada com retorno 1. Um mutante que remove apenas `|~{3,}` da regex de cercas continua acertando esses 49 casos e aceita um mapa inteiramente entre `~~~`. A fixture testa exclusivamente a cerca de três crases. (`diff bin/mapa20_valida.py:47`, `:99-104`; `diff bin/tests/test_mapa20_valida.sh:29-49`)

## 11/11 10:54 · sessao 1623f482cb44 · - · - · origem=Ronald (tmux)

Fluxo após diff, com linhas **novas** de `bin/relato.sh`:

- `MAPA_RC=0; MAPA_PAR=""` em **94**; `cp` e validador em **96**; sucesso escreve `MAPA_PAR` em **97**; falha de qualquer um escreve `MAPA_RC=1` em **99**. Nenhuma outra atribuição a `MAPA_RC` no diff. Portanto validador executado vermelho não termina com rc 0; verde termina com rc 0 salvo outra falha. (`diff bin/relato.sh:94–99,155,161–162`.)
- Todos os `exit`: base **27,28,32,35,38,42,86**, todos `1`; novos **126** (`gerar_avais`, `1`), **141** (raspagem geral, `1`), **155** (`"$MAPA_RC"`: `0` ou `1`), **161** (falha do push/echo: `1`), **162** (`"$MAPA_RC"`: `0` ou `1`). A inserção de 24 linhas deixa as primeiras sete posições intactas. (`bin/relato.sh` base citada; `diff bin/relato.sh` linhas novas citadas.)
- Pode remover público **sem executar o validador**: fonte ausente/ilegível faz `cp` falhar, o `&&` pula Python; re-raspagem da cópia existente falha, `rm` apaga `MAPA-20.md`, `MAPA_RC` permanece `1`; etapa 4 inclui remoção rastreada. Isso não é caminho com **validador verde**. (`diff bin/relato.sh:96,99,102–109,148–152,159–162`.)
- Não encontrei caminho interno com validador verde que entre nesse `rm`: está exclusivamente no `else` do `cp && validar`. A raspagem geral ainda pode falhar por motivos próprios e sai `1`. (`diff bin/relato.sh:96–112,127–141`.)

QUEM ESCREVE / QUEM LÊ:

| Valor | Escritor | Leitor |
|---|---|---|
| `MAPA_RC` | shell: inicializa `0`, falha fixa `1` | saídas de “nada mudou” e final |
| `MAPA_PAR` | shell: vazio / par da foto | argumento opcional da raspagem geral |
| `MAPA_PATH` | shell: vazio / nome após `git add` bem sucedido | mensagem e pathspec do commit | [nao verificado]
| `$TMP/mapa20.md` | `cp` da fonte | validador; raspador pela lista de pares |
| `logs/MAPA-20/mapa.md` | sessão, **apenas declarado em comentário**, produtor real não fornecido | `cp` |
| `$DEST/MAPA-20.md` | `raspar_publico.py:40`, ou troca do arquivo re-raspado com `mv`; remoção com `rm` | raspador no ramo retido, teste `-f`, Git | [nao verificado]

Refs: `diff bin/relato.sh:89–112,141,148–162`; `bin/raspar_publico.py:23–40`. Escritor efetivo/eventos da fonte: **[nao verificado]**. O diff não cria gerador nem gatilho para regravar a fonte em pouso/reprovação/lei: o comentário delega à sessão. (`diff bin/relato.sh:89–90`.)

Classes:

- **Segundo juiz:** raspagem reaproveita o juiz único de privacidade: `raspar_publico.py:15–16,31,36`, `nome_no_texto.py:59,87`; não introduz uma segunda regex de nomes.
- **Segundo escritor:** duas rotas de atualização do público estão no mesmo publicador sob a trava já existente: `bin/relato.sh` base **27**, diff **96–112,141**; produtor da fonte fora desse bloco não fornecido.
- **Relógio:** nenhum relógio novo de decisão no diff; `date` continua só na mensagem do commit (`base bin/relato.sh:130`, diff **159**). Timer já existente em `bin/hasner-relato.timer:5–7`; chama script por `bin/hasner-relato.service:15`.
- **Fonte única / derivação:** `MAPA_PAR` faz publicador consumir a foto efetivamente validada (`diff bin/relato.sh:96–97,141`); censo/fonte/medidas de negócio não são computados pelo diff **[nao verificado]**.
- **Trava:** nenhuma trava nova; `codex_estado.sh --gerar` sai antes da aquisição de lock do ramo evento, logo não autoespera na trava que `relato.sh` já segura. (`bin/relato.sh:27`; `bin/codex_estado.sh:54–56,65–66`.)
- **Pouso/pré-requisito:** contrato MAPA-20 é apenas referenciado pelo novo comentário; não consta no texto físico desta base. Comprovação do contrato, fonte pronta, mapa publicado, selos executados e opus: **[nao verificado]**. A regra de contrato está em `CLAUDE.md:394`, suíte/opus em **377–381**. [nao verificado]
- **Campo com dois significados:** `MAPA_RC` é resultado acumulado da etapa MAPA (cópia **ou** validação), não rc puro do Python; `MAPA_PATH` identifica artefato rastreado inclusive para remoção, não prova de publicação nova. Isso está explícito no uso (`diff bin/relato.sh:94–99,148–152`). Confundir os nomes com “validador verde”/“mapa publicado” produziria conclusões erradas.

Limite preexistente útil: `set -uo pipefail` sem `-e`, `cd`, `git add` geral e `git commit` sem checagem continuam na base (`bin/relato.sh:21,120–132`), agora linhas novas **144–161**. Não são cura completa pelo novo `push || exit 1`; falha de commit seguida de push bem sucedido pode ainda terminar `0` quando mapa verde, embora exista outra falha.

## 11/11 10:52 · sessao 1239d5bfa276 · - · - · origem=Ronald (tmux)

Nenhum dos cinco selos vizinhos tem quebra estática causada pelo diff; execução completa dos selos ficou [nao verificado], pois eles criam arquivos e/ou acessam recursos externos. A busca equivalente a `git grep 'relato.sh' bin/tests` encontra referências nos cinco arquivos abaixo. (`bin/tests/test_codex_estado.sh:100`; `bin/tests/test_codex_estado_evento.sh:28`; `bin/tests/test_relato_publica_claude_md.sh:8`; `bin/tests/test_relato_afirma_com_prova.sh:44`; `bin/tests/test_publicacao_sem_nome.sh:6`.)

1. **`test_codex_estado.sh`: não quebra.** O recorte começa na primeira ocorrência literal de `CODEX_OK=""` e termina no marcador do passo 3; só busca `exit` nesse intervalo, excluindo comentários. O bloco 2f novo fica antes do bloco 2e e de `CODEX_OK=""`, portanto seus novos comandos não entram nessa região. (`bin/tests/test_codex_estado.sh:104-111`; base `bin/relato.sh:89-98`; diff `bin/relato.sh:89-122`, linhas novas.)

2. **`test_relato_publica_claude_md.sh`: não quebra.** A raspagem geral mantém o par CLAUDE; o novo raspador de MAPA retido não casa com a regex que exige continuação de linha imediatamente depois de `"$DEST"`. O teste conta apenas linhas começando na coluna 1 por `git add -A `; o novo `git add -A -- MAPA-20.md` tem indentação e não aumenta essa contagem. O pathspec do commit continua contendo CLAUDE. (`bin/tests/test_relato_publica_claude_md.sh:13-20`; diff `bin/relato.sh:104-105`, `141`, `145`, `151`, `160`, linhas novas.)

3. **`test_codex_estado_evento.sh`: não quebra na costura.** Continua sem `gerar_codex_estado.py` em `relato.sh`, mantendo chamadas de `codex_estado.sh` e `relato_nomes.sh`. (`bin/tests/test_codex_estado_evento.sh:27-30`; base `bin/relato.sh:32`, `97`; diff não altera essas chamadas.)

4. **`test_relato_afirma_com_prova.sh`: não quebra na costura.** Seu predicado apenas exige a presença de `relato_afirma_com_prova.py`, preservada pelo diff. (`bin/tests/test_relato_afirma_com_prova.sh:43-45`; base `bin/relato.sh:55`.)

5. **`test_publicacao_sem_nome.sh`: não recorta `relato.sh`.** As referências são comentários; o universo efetivo vem de `git ls-files` do público. MAPA-20 rastreado entra automaticamente nesse universo. Resultado de privacidade em produção permanece [nao verificado]. (`bin/tests/test_publicacao_sem_nome.sh:58-72`.)

_3812 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
