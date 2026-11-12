# CODEX-ESTADO

> Ultima resposta processada: **12/11/2026 16:53** · publicado em **12/11/2026 16:56** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 12/11 16:53 · sessao 55d549d73138 · - · - · origem=Ronald (tmux)

Censo por AST: **200 chamadas** em **185 linhas de 59 arquivos** de `app/`, excluindo diretórios `tests`, `__pycache__`, `.venv`, `venv` e arquivos `test_*.py`; zero erros de parse. São 162 chamadas fora de `management` e 38 dentro. A diferença contra 161 é **+39 chamadas**; o comando/recorte do grep do aval não foi fornecido, portanto equivalência entre as medições é [não verificado]. Incluindo o repositório inteiro com os mesmos filtros há mais uma chamada de render em `bin/disjuntor.py:2266`.

A classificação primária usa **P > C > R** quando há mais de um destino demonstrado: **P62 + C53 + R83 + U2 = 200**. `U` identifica valor descartado, fora da taxonomia solicitada. Sobreposição explícita P+C: seis chamadas — `app/escala/utils.py:291`, `app/escala/utils.py:730`, `app/escala/utils.py:749`, `app/ponto/motor_calculo_v2.py:1671`, `app/ponto/motor_calculo_v2.py:1672`, `app/ponto/management/commands/detectar_ausencias.py:484`. Contando essas sobreposições, são 59 chamadas com uso comparativo. Os totais são a soma da tabela abaixo; classificação de todos os destinos possíveis em execução é [não verificado].

Na tabela, linhas repetidas significam duas chamadas na mesma linha; cada número compõe `arquivo:linha`.

```text
arquivo                                                       PERSISTIDO                 COMPARADO                       RENDER                              SEM USO
app/api/views.py                                              627,645                                                     355,535,681,800,1457,2035,2959,2960
app/api/views_core.py                                         618,636                                                     343,521,672,794,1280,1281
app/chamados/detector_anomalias.py                             234,235                    213                                                                 222
app/chamados/juizes.py                                                                   1814
app/chamados/models.py                                                                                                   4070,4072,4076,4078
app/chamados/reconciliador.py                                                            210
app/chamados/services/acoes_chamado.py                                                                                    383
app/chamados/services/explicador.py                                                                                       57
app/chamados/services/materializacao.py                                                   182                            465
app/chamados/services/painel.py                                                                                          395
app/chamados/services/tarefa_do_dia.py                                                     78
app/chamados/views_cobrar.py                                                                                             42,57
app/colaboradores/services/calendario.py                                                   625
app/colaboradores/services/ficha.py                                                                                      28
app/colaboradores/views.py                                                                                              2312,2313
app/core/esteira_vigia.py                                     1304,1560,1569,1572,1580,1590
app/core/templatetags/hasner_filters.py                                                                                  102,102,147
app/core/views.py                                                                                                       825
app/escala/models.py                                                                                                    415,415,444,444,452,452,509,509,
                                                                                                                       517,517,546,546,664,664,665,665
app/escala/servico_jornada.py                                                             455,456                        201,201
app/escala/utils.py                                          291,326,621,662,712,730,749,  378,429,782,1675,1707,1719,
                                                            799,1549,1828,1832,1834     1724,1742,1754,1755,2005,2028
app/pautas/services.py                                                                                                  431
app/ponto/calculador/alimentacao.py                                                       119
app/ponto/calculador/nucleo.py                               142,146,238
app/ponto/management/commands/censo_col900.py                 138
app/ponto/management/commands/desvio_o68b.py                                                                              125,126
app/ponto/management/commands/detectar_ausencias.py           484,848                                                     410,418,429,436,452,782
app/ponto/management/commands/detectar_intervalo_ausente.py                                67,74,79,82,84                                                      81
app/ponto/management/commands/detectar_par_relampago.py       97,98                                                       393,394
app/ponto/management/commands/diff_calculador.py                                                                          460
app/ponto/management/commands/diff_reclassificar_partido.py                                                               97,98
app/ponto/management/commands/dna_x_batida_real.py            122,123
app/ponto/management/commands/emitir_furo_retroativo.py                                                                    152
app/ponto/management/commands/emitir_turno_aberto_retroativo.py 167
app/ponto/management/commands/gerar_celulas.py               33
app/ponto/management/commands/processar_alertas_avancados.py 136
app/ponto/management/commands/reconciliar_fantasmas.py        121,123,134,134
app/ponto/management/commands/tripwire_celulas.py                                         192,195,280
app/ponto/management/commands/tripwire_tipo_batida.py                                                                     64
app/ponto/marcos.py                                                                     11
app/ponto/motor_calculo_v2.py                                1671,1672                   1488,3069                      2221,2222,2229,2230
app/ponto/propositor.py                                                                 340,342,342,343,345
app/ponto/services/ausencia.py                               910,911,934,934
app/ponto/services/cartorio.py                                                          878,1927
app/ponto/services/dia_do_colab.py                                                                                       57
app/ponto/services/diagnostico_escala.py                                                 26
app/ponto/services/entrada_sincronizada.py                    28
app/ponto/services/espelho.py                                                                                           210
app/ponto/services/faltas_de_hoje.py                                                     45
app/ponto/services/flip_auto.py                                                         219,221,221,222,232            319
app/ponto/services/lastro.py                                                                                            313
app/ponto/services/passivo_s84.py                            103
app/ponto/services/teto_na_lista.py                          82,83
app/ponto/services/triagem_batida.py                         472,473,479,480,492,540,
                                                            586,611,741                                               187
app/ponto/services/veredito_celula.py                        320,329                     252
app/ponto/services/vigia_pareador_celula.py                                              110
app/ponto/utils_turno_orfao.py                                                                                          40
app/relatorios/pdf_espelho.py                                                           178,609,716,960,963             732,733,881,885,907,907,955
app/relatorios/services.py                                                                                             133
```

Os dois valores sem uso são:

- `app/chamados/detector_anomalias.py:222`: `horario_real` no retorno `informativa`. Os dois chamadores tratam `grave` e `leve`, sem consumir esse retorno: `app/api/views.py:701`, `app/api/views.py:702`, `app/api/views.py:705`, `app/api/views_core.py:692`, `app/api/views_core.py:693`, `app/api/views_core.py:696`.
- `app/ponto/management/commands/detectar_intervalo_ausente.py:81`: a hora compõe `marcos_faltantes`, mas o chamador usa apenas as datas e a quantidade de furos; a emissão está encerrada por `continue`: `app/ponto/management/commands/detectar_intervalo_ausente.py:120`, `app/ponto/management/commands/detectar_intervalo_ausente.py:121`, `app/ponto/management/commands/detectar_intervalo_ausente.py:156`, `app/ponto/management/commands/detectar_intervalo_ausente.py:157`.

Persistidos: fato e destino demonstrado:

| Origem | Fato/previsão | Destino/prova |
|---|---|---|
| `app/escala/utils.py:326`, `:1549`, `:1828` | Batida/marco realizado | A hora realizada da célula torna-se `lampadas[].luz`: `app/ponto/services/cartorio.py:711`, `:712`, `:736`, `:738`. |
| `app/escala/utils.py:621`, `:662`, `:799`, `:1832`, `:1834` | Órfã | A lista da grade vira `ata['orfas']` sem instante: `app/ponto/services/cartorio.py:743`, `:745`. |
| `app/escala/utils.py:291`, `:712`, `:730`, `:749` | Marco previsto | Produzem a régua `tipo/hora`; a régua alimenta a hora prevista das lâmpadas: `app/escala/utils.py:734`, `:753`, `:1401`; `app/ponto/services/cartorio.py:638`, `:648`, `:711`. |
| `app/chamados/detector_anomalias.py:234`, `:235` | Previsto e batida, respectivamente | `ctx_desvio` vira `contexto_json` de chamado: `app/api/views.py:714`, `:717`, `:726`; `app/api/views_core.py:708`, `:720`. |
| `app/api/views.py:627`, `:645`; `app/api/views_core.py:618`, `:636` | Hora da tentativa/recepção, pelo relógio do servidor | Compõem contexto de disputa: `app/api/views.py:497`, `:498`, `:625`, `:642`, `:765`. Não são necessariamente o instante efetivo offline da batida: `app/api/views.py:508`. |
| `app/ponto/services/triagem_batida.py:472`, `:479`, `:741` | Tentativa/batida web | Chamado JSON, sessão, disputa: `app/ponto/services/triagem_batida.py:470`, `:486`, `:735`. |
| `app/ponto/services/triagem_batida.py:473`, `:480`, `:492` | Previsto | Chamado/sessão e texto da justificativa da batida: `app/ponto/services/triagem_batida.py:470`, `:486`, `:690`, `:697`, `:702`. |
| `app/ponto/services/triagem_batida.py:540`, `:586`, `:611` | Batida | Texto humano salvo em ocorrência e descrição de justificativa: `app/ponto/services/triagem_batida.py:603`, `:609`, `:613`. |
| `app/ponto/services/entrada_sincronizada.py:28` | Batida sincronizada | Ocorrência, justificativa e resolução: `app/ponto/services/entrada_sincronizada.py:35`, `:42`, `:43`, `:60`. |
| `app/ponto/motor_calculo_v2.py:1671`, `:1672` | Batida real e previsto | Lista do motor levada a `MetricaSnapshot.detalhe`: `app/ponto/motor_calculo_v2.py:2719`; `app/ponto/services/cxr_na_lista.py:79`, `:86`, `:130`, `:145`, `:148`. |
| `app/ponto/services/teto_na_lista.py:82`, `:83` | Marcos previstos `hi/hf` | Snapshot: `app/ponto/services/teto_na_lista.py:113`, `:116`. |
| `app/ponto/services/ausencia.py:910`, `:911`, `:934` duas chamadas | Período declarado de ausência | Auditoria antes/depois: `app/ponto/services/ausencia.py:932`, `:933`, `:934`; `app/core/auditoria.py:22`. Realização do período é [não verificado]. |
| `app/ponto/services/veredito_celula.py:320`, `:329` | Batida corrigida; tentativa de materializar marco | Ações/erros salvos em auditoria: `app/ponto/services/veredito_celula.py:356`, `:358`. Sucesso da materialização do caso `:329` é [não verificado]. |
| `app/ponto/management/commands/detectar_par_relampago.py:97`, `:98` | Batidas do par | `contexto_json['par']`: `app/ponto/management/commands/detectar_par_relampago.py:383`. |
| `app/ponto/management/commands/detectar_ausencias.py:484` | Marco previsto | `contexto_json['marcos_faltantes']`: `app/ponto/management/commands/detectar_ausencias.py:706`, `:710`, `:712`. |
| `app/ponto/management/commands/detectar_ausencias.py:848` | Batida | Ocorrência textual: `app/ponto/management/commands/detectar_ausencias.py:852`. |
| `app/ponto/management/commands/gerar_celulas.py:33` | Marco previsto | Helper alimenta `dna.marcos` e `dna.lampadas[].hora`: `app/ponto/management/commands/gerar_celulas.py:68`, `:83`, `:93`, `:100`, `:103`. |
| `app/ponto/management/commands/processar_alertas_avancados.py:136`; `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:167` | Saída prevista | Descrição do chamado: `app/ponto/management/commands/processar_alertas_avancados.py:121`, `:133`; `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:151`, `:159`. |
| `app/ponto/management/commands/reconciliar_fantasmas.py:121`, `:123`, `:134` duas chamadas | Entrada/saída reais; saída real e saída prevista na linha 134 | Diagnóstico `detalhe` salvo em ocorrência/auditoria: `app/ponto/management/commands/reconciliar_fantasmas.py:205`, `:206`, `:278`, `:280`, `:286`, `:299`. |
| `app/ponto/calculador/nucleo.py:142`, `:146`, `:238` | Batidas | Arquivo CSV diagnóstico: `app/ponto/management/commands/e6_oraculo.py:330`, `:404`, `:444`, `:445`. |
| `app/ponto/management/commands/censo_col900.py:138` | Primeira batida | Arquivo JSON opcional: `app/ponto/management/commands/censo_col900.py:178`, `:179`. |
| `app/ponto/management/commands/dna_x_batida_real.py:122`, `:123` | Marcos previstos | Arquivo JSON opcional: `app/ponto/management/commands/dna_x_batida_real.py:155`, `:156`, `:157`. |
| `app/ponto/services/passivo_s84.py:103` | Batida | Arquivo de reversão: `app/ponto/management/commands/passivo_disputa_retro.py:139`, `:141`, `:146`, `:148`. |
| `app/core/esteira_vigia.py:1304`, `:1560`, `:1569`, `:1572`, `:1580`, `:1590` | Eventos da esteira; não batida/órfã/resposta/marco | Arquivos de relato/fim: `app/core/esteira_vigia.py:1329`, `:1559`, `:1570`, `:1578`, `:1589`; o argumento de `destravar` chega a `f.write`: `app/core/esteira_vigia.py:1027`, `:1028`. |

Comparados: fato e operação demonstrada:

| Origem | Fato/previsão e uso |
|---|---|
| `app/escala/utils.py:291`, `:730`, `:749`, `:782`, `:1707`, `:1719`, `:1724`, `:1742`, `:1754`, `:1755` | Marcos previstos: chaves de dict, pertinência e busca de coluna. `app/escala/utils.py:292`, `:730`, `:749`, `:785`, `:1708`, `:1719`, `:1724`, `:1745`, `:1754`, `:1755`. |
| `app/escala/utils.py:378` | Marco previsto encadeado: chave de dict para casar régua e instante. `app/escala/utils.py:378`, `:381`, `:382`. |
| `app/escala/utils.py:429` | Batida: igualdade com hora realizada da célula. `app/escala/utils.py:431`. |
| `app/escala/utils.py:1675` | Batida/órfã: pertinência na lista de órfãs convertida em conjunto. `app/escala/utils.py:1650`, `:1651`, `:1675`. |
| `app/escala/utils.py:2005`, `:2028` | Batidas/órfãs: pertinência, igualdade de listas e ordem textual da hora realizada. `app/escala/utils.py:2006`, `:2032`, `:2034`, `:2039`. |
| `app/ponto/motor_calculo_v2.py:1488`; `app/ponto/calculador/alimentacao.py:119` | Entrada real: chave `(dia, HH:MM)` e pertinência no conjunto de pares bloqueados. `app/ponto/calculador/alimentacao.py:74`, `:119`. |
| `app/ponto/motor_calculo_v2.py:1671`, `:1672` | Real/previsto: deduplicação por igualdade do item; previsto também casa com marco da célula. `app/ponto/services/cxr_na_lista.py:86`, `:107`; `app/ponto/janela_he.py:296` e continuação do corpo. |
| `app/ponto/motor_calculo_v2.py:3069` | Entrada real: igualdade com hora realizada do alinhamento. `app/ponto/motor_calculo_v2.py:3069`. |
| `app/ponto/propositor.py:340`, `:342` duas chamadas, `:343`, `:345`; `app/escala/servico_jornada.py:455`, `:456`; `app/ponto/services/flip_auto.py:219`, `:221` duas chamadas, `:222`, `:232` | Previstos e batidas convertidos à mesma representação de hora do dia para atribuição posicional. `app/ponto/propositor.py:351`; `app/escala/servico_jornada.py:455`; `app/ponto/services/flip_auto.py:233`, `:238`. |
| `app/ponto/marcos.py:11` | Previsto: helper utilizado em igualdades de hora. `app/ponto/marcos.py:31`, `:33`, `:37`, `:39`, `:47`, `:49`, `:63`, `:65`, `:108`. |
| `app/chamados/reconciliador.py:210` | Batida: chave de dict usada para recuperar os integrantes do par por HH:MM. `app/chamados/reconciliador.py:209`, `:213`. |
| `app/chamados/detector_anomalias.py:213` | Batida: igualdade com hora realizada do alinhamento. `app/chamados/detector_anomalias.py:214`. |
| `app/chamados/juizes.py:1814` | Previsto: retorna HH:MM para comparação com resposta; a resposta é reduzida a minutos do dia. `app/chamados/juizes.py:1852`, `:1858`, `:1863`, `:1868`, `:1878`. |
| `app/chamados/services/materializacao.py:182` | Instante pretendido pela resposta/plantio: atribuição contra slots e horas reais/órfãs. `app/chamados/services/materializacao.py:179`, `:180`, `:181`, `:182`. |
| `app/chamados/services/tarefa_do_dia.py:78` | Previsto: igualdade com a hora prevista da lâmpada. `app/chamados/services/tarefa_do_dia.py:78`. |
| `app/ponto/services/cartorio.py:878`, `:1927` | Previstos: conjunto de marcos vencidos e dict de instantes por `(tipo, HH:MM)`. `app/ponto/services/cartorio.py:447`, `:450`, `:1927`, `:1930`, `:1931`, `:1936`. |
| `app/ponto/services/veredito_celula.py:252` | Previsto: chave de dict para materialização do marco faltante. `app/ponto/services/veredito_celula.py:275`, `:276`. |
| `app/ponto/services/faltas_de_hoje.py:45` | Previsto: volta a `datetime` para comparação com agora. `app/ponto/services/faltas_de_hoje.py:48`, `:49`. |
| `app/ponto/services/diagnostico_escala.py:26` | Batida: helper serve render e chaves de dict; também distância entre primeira entrada e `dna.marcos.hi`. `app/ponto/services/diagnostico_escala.py:160`, `:163`, `:165`, `:167`, `:172`. |
| `app/colaboradores/services/calendario.py:625` | Batida: pertinência em órfãs e lookup por hora. `app/colaboradores/services/calendario.py:626`, `:631`. |
| `app/ponto/services/vigia_pareador_celula.py:110` | Batida: pertinência em órfãs civis. `app/ponto/services/vigia_pareador_celula.py:111`. |
| `app/relatorios/pdf_espelho.py:178`, `:609`, `:716`, `:960`, `:963` | Batidas/órfãs: pertinência por HH:MM, inclusive filtragem textual de anomalias. `app/relatorios/pdf_espelho.py:178`, `:609`, `:720`, `:964`. |
| `app/ponto/management/commands/tripwire_celulas.py:192`, `:195`, `:280` | Previsto, órfã e previsto, respectivamente: igualdade HH:MM antes de teste por instante. `app/ponto/management/commands/tripwire_celulas.py:192`, `:195`, `:280`, `:285`. |
| `app/ponto/management/commands/detectar_intervalo_ausente.py:67`, `:74`, `:79`, `:82`, `:84` | Previstos: lookup de lâmpadas e de instante encadeado por `(tipo, HH:MM)`. `app/ponto/management/commands/detectar_intervalo_ausente.py:67`, `:74`, `:79`, `:82`, `:84`. |
| `app/ponto/management/commands/detectar_ausencias.py:484` | Previsto: traduzido/comparado por `marco_para_motivo` e persistido nos marcos faltantes. `app/ponto/management/commands/detectar_ausencias.py:482`, `:484`, `:712`; `app/ponto/marcos.py:31`. |

A lei fornecida exclui `dna.hi/hf` previstos; o produtor desta cópia efetivamente os escreve como hora do dia por `_hhmm`: `app/ponto/management/commands/gerar_celulas.py:33`, `:83`. O principal escritor de fato em HH:MM está antes da lavra: `_celula_do_marco` grava a batida em `hora`, e o cartório a transporta para `lampadas[].luz`; órfãs são transportadas como strings: `app/escala/utils.py:326`; `app/ponto/services/cartorio.py:712`, `:745`.

A classificação P inclui texto humano salvo, CSV/JSON diagnóstico e arquivos da est

[truncado em 20000 de 20154 caracteres]

## 12/11 16:52 · sessao c79798754d32 · C20-O168 · verificador · origem=Code

**A guarda cobre os quatro sítios alterados e a cascata E2, mas não todos os fechamentos automáticos alcançados pelo evento. Há também divergência entre “pai vivo preservado” e o universo efetivamente contado.** (`app/ponto/services/cartorio.py:965`, `app/chamados/reconciliador.py:651`, `app/ponto/services/lastro.py:401`, `app/chamados/services/disputa_emissao.py:2091`.)

Execução da suíte, execução de mutantes e ocorrência dos achados em produção: **[nao verificado]**. [nao verificado]

**1. Caminhos que fecham e cobertura da guarda**

| Caminho | Passa por `pode_executar_csf`? | Prova |
|---|---|---|
| Cartório: adiados pela lâmpada | **Sim** | `app/ponto/services/cartorio.py:1054` → guarda em `app/ponto/services/cartorio.py:965`. |
| Cartório: RECONCILIA-MARCO | **Sim** | `app/ponto/services/cartorio.py:1589`. |
| Cartório: MARCO-INEXISTENTE | **Sim** | `app/ponto/services/cartorio.py:1602`. |
| Cartório: B5.3b | **Sim** | `app/ponto/services/cartorio.py:1615`, `app/ponto/services/cartorio.py:1623`. |
| E2 → `disputa.fechar` → S88 | **Sim, antes de fechar a disputa** | Lote consulta o juiz em `app/chamados/reconciliador.py:649`; preservação precede `.fechar()` em `app/chamados/reconciliador.py:621`, `app/chamados/reconciliador.py:629`. |
| Cartório: FIO-DUP e FIO-DUP-C8 | **Não** | Fechos em `app/ponto/services/cartorio.py:1701`, `app/ponto/services/cartorio.py:1716`; isenção própria por texto em `app/ponto/services/cartorio.py:1687`. |
| `reconciliar_chamados_colab` → premissa morta → `_retratar` | **Não** | `app/chamados/reconciliador.py:1475`, `app/chamados/reconciliador.py:1498`, `app/chamados/reconciliador.py:524`. |
| `reconciliar_chamados_colab` → órfãs → `lavrar_sem_turno_aberto` → disputa → S88 | **Não** | `app/chamados/reconciliador.py:1467`, `app/chamados/reconciliador.py:1653`, `app/chamados/reconciliador.py:413`. |
| Consumidor `EventoCelula` → E2 | **Sim** | `app/chamados/reconciliador.py:1399` → proteção central em `app/chamados/reconciliador.py:621`. |
| Consumidor → chamados absolvidos pela célula → `_retratar` | **Não** | `app/chamados/reconciliador.py:1402`, `app/chamados/reconciliador.py:1038`, `app/chamados/reconciliador.py:524`. |
| Consumidor → premissa morta → `_retratar` | **Não** | `app/chamados/reconciliador.py:1406`, `app/chamados/reconciliador.py:1498`. |
| Consumidor → órfãs pela lâmpada → disputa → S88 | **Não** | `app/chamados/reconciliador.py:1409`, `app/chamados/reconciliador.py:1205`. |
| Consumidor → lastro por marco → `reconciliar` | **Não** | `app/chamados/reconciliador.py:1417`, `app/ponto/services/lastro.py:395`, `app/ponto/services/lastro.py:401`. |
| S143/S128 → resolver perguntas → disputa → S88 | **Não** | S143 chama resolver em `app/chamados/signals.py:117`; candidatos incluem disputas abertas do colaborador em `app/chamados/services/disputa_emissao.py:2026`; fechamento em `app/chamados/services/disputa_emissao.py:2092`. |
| Fábrica → geração → reatar órfã | **Não nos fechos alcançados** | Geração chama reata em `app/chamados/services/disputa_emissao.py:459`; reata chega aos fechos em `app/chamados/reconciliador.py:1653`, `app/chamados/reconciliador.py:1682`. |
| Emissor do cartório → repasse de órfãos → auto-fecho da disputa | **Não** | Emissor chama repasse em `app/ponto/management/commands/detectar_ausencias.py:792`; auto-fecho em `app/chamados/services/disputa_emissao.py:1278`, `app/chamados/services/disputa_emissao.py:1284`. |

Há ainda fechamento por agrupamento LIMBO e por premissa de cluster, ambos sem CSF. O enquadramento desses dois domínios na lei específica de **cobrança sem furo** é **[nao verificado]**. (`app/chamados/services/limbo_folgas.py:284`, `app/ponto/services/cartorio.py:1085`, `app/ponto/services/cluster_espurio.py:190`.)

**S88 e o gate não completam a proteção ausente nos chamadores:** S88 chama a porta; `reconciliar` consulta motor, premissa e pergunta viva, sem CSF. (`app/chamados/signals.py:43`, `app/chamados/models.py:672`, `app/chamados/models.py:683`, `app/chamados/models.py:686`.)

As guardas existentes são diferentes: `_retratar` retém resposta **sem consumo**, enquanto CSF retém qualquer `respondida_em`, inclusive resposta já validada/materializada. C7 pergunta por fio vivo e resposta sem veredito. (`app/chamados/reconciliador.py:448`, `app/chamados/juizes.py:2018`, `app/ponto/supra_juiz.py:545`, `app/chamados/models.py:1843`, `app/chamados/models.py:1854`.)

O FIO-DUP está explicitamente declarado como lacuna em fila no próprio helper; não o apresento como cura prometida pelo delta. (`app/ponto/services/cartorio.py:948`.)

**2. Constância e mordida do teste** [nao verificado]

- **O desenho de consultas em lote existe nos dois helpers.** `_pais_com_conversa` carrega as duas famílias de perguntas; `_fecham_cobrancas_sem_furo` carrega origem/perguntas, recebendo as resolvedoras já carregadas pelo caminho real do cartório. (`app/chamados/reconciliador.py:649`, `app/ponto/services/cartorio.py:962`, `app/ponto/services/cartorio.py:128`, `app/ponto/services/cartorio.py:1369`.)
- **A comparação explícita 1 × 3 mede somente `_pais_com_conversa`.** Não mede o E2 completo nem os quatro sítios diretos. O E2 conserva, inclusive, `refresh_from_db` dentro do laço por disputa. (`app/ponto/tests/test_o168_guarda_csf_no_evento.py:281`, `app/ponto/tests/test_o168_guarda_csf_no_evento.py:286`, `app/chamados/reconciliador.py:620`.)
- **Um mutante que acrescente uma query por disputa dentro do helper é alcançado pela igualdade:** acrescentaria +1 versus +3 às capturas. Isso é conclusão estática; execução do mutante **[nao verificado]**. Mutação fora desse trecho capturado não é demonstrada por esse teste. (`app/ponto/tests/test_o168_guarda_csf_no_evento.py:275`, `app/ponto/tests/test_o168_guarda_csf_no_evento.py:282`, `app/ponto/tests/test_o168_guarda_csf_no_evento.py:286`.)
- Os tetos **222/282** estão registrados, mas o selo geral cobra `medido <= teto`; não compara o delta da guarda entre os cenários. CPU e volume de perguntas também não são constantes: o juiz percorre as coleções. (`app/ponto/tests/test_selo_chokepoint_escrita.py:196`, `app/ponto/tests/test_selo_chokepoint_escrita.py:210`, `app/ponto/tests/test_selo_chokepoint_escrita.py:293`, `app/ponto/supra_juiz.py:543`.)

Portanto: **lote nos novos sítios; prova comparativa de constância somente no helper E2**, sem certificação do custo completo do evento. (`app/ponto/services/cartorio.py:962`, `app/chamados/reconciliador.py:649`, `app/ponto/tests/test_o168_guarda_csf_no_evento.py:282`.)

**3. Contador `csf_com_conversa_preservado`**

| Aspecto | Escritor/leitor e prova |
|---|---|
| Unidade implementada | `pk` de chamado distinto **por ato**, pelo conjunto em `app/ponto/services/cartorio.py:978`. |
| Incrementador | `_contar_csf_preservado`, em `app/ponto/services/cartorio.py:981`. |
| Chamador direto | Guarda negativa em `app/ponto/services/cartorio.py:966`. |
| Chamador pela cascata | Callback fornecido em `app/ponto/services/cartorio.py:1050`, invocado em `app/chamados/reconciliador.py:626`. |
| Releitura | Compartilha o conjunto e soma somente incrementos novos em `app/ponto/services/cartorio.py:1131`, `app/ponto/services/cartorio.py:1134`. |
| Duração | Conjunto removido no `finally`; o número permanece acumulado no `out` do lote. (`app/ponto/services/cartorio.py:1150`, `app/ponto/services/cartorio.py:977`.) |
| Inicialização | Surge no primeiro incremento; não consta no retorno inicial de `contadores()`. (`app/ponto/services/cartorio.py:981`, `app/ponto/services/cartorio.py:2005`.) |
| Leitores | Consolidação da releitura e testes de resultado. (`app/ponto/services/cartorio.py:1133`, `app/ponto/tests/test_o168_guarda_csf_no_evento.py:244`, `app/ponto/tests/test_o168_guarda_csf_no_evento.py:298`.) |
| Exposição | `julgar_celula` devolve `out`; os resumos dos dois comandos não imprimem esse contador. (`app/ponto/services/cartorio.py:2105`, `app/ponto/management/commands/processar_cartorio.py:66`, `app/ponto/management/commands/lavrar_status_competencia.py:58`.) |

**Defeito de universo:** `_pais_com_conversa` anuncia pai VIVO, mas usa `not in ENCERRADOS`. Isso admite `cancelado` e `superado`, que não são vivos e já seriam recusados por RECONCILIAR. Esses pais podem segurar a disputa e alcançar o contador. Ocorrência real: **[nao verificado]**. (`app/chamados/reconciliador.py:638`, `app/chamados/reconciliador.py:651`, `app/chamados/catalogo/motor.py:24`, `app/chamados/catalogo/motor.py:30`, `app/chamados/catalogo/motor.py:111`, `app/chamados/reconciliador.py:626`.)

Também há divergência no contrato do callback: a documentação promete uma chamada por pai, mas a implementação chama por disputa preservada. O contador consegue deduplicar essas chamadas; o callback, isoladamente, não oferece essa garantia. (`app/chamados/reconciliador.py:559`, `app/chamados/reconciliador.py:618`, `app/chamados/reconciliador.py:626`, `app/chamados/models.py:1750`, `app/ponto/services/cartorio.py:979`.)

**4. Quem escreve e quem lê**

**É o mesmo juiz**, importado pelos dois helpers e pelo executor supra. C7 permanece outra pergunta, registrada: “a disputa pode fechar?”. (`app/ponto/services/cartorio.py:958`, `app/chamados/reconciliador.py:645`, `app/ponto/management/commands/supra_juiz.py:257`, `app/core/juizes.py:279`.)

| Campo/função | Quem escreve ou define | Quem lê na decisão auditada |
|---|---|---|
| `respondida_em` | Resposta humana, automática e recuperação pela trilha: `app/chamados/services/disputa_emissao.py:1468`, `:1653`, `:1674`; carimbo: `app/chamados/models.py:3133`; reversões: `app/chamados/services/disputa_emissao.py:1593`, `app/chamados/models.py:1567`. | CSF: `app/ponto/supra_juiz.py:545`, `:548`; C7: `app/chamados/models.py:1845`; retenção: `app/chamados/juizes.py:2020`. |
| `resposta_colab` | `app/chamados/services/disputa_emissao.py:1467`, `:1652`; limpeza em `:1593`. | Isenção FIO-DUP: `app/ponto/services/cartorio.py:1691`; **CSF não lê texto**, lê carimbo em `app/ponto/supra_juiz.py:545`. |
| `disputa_origem` / `Disputa.chamado` | Relação declarada em `app/chamados/models.py:1750`; nascimento em `app/chamados/services/disputa_emissao.py:587`, `:1123`, `:1358`. | CSF: `app/ponto/supra_juiz.py:543`; lote E2: `app/chamados/reconciliador.py:649`; S88: `app/chamados/signals.py:33`. |
| `Disputa.perguntas` / `Pergunta.disputa` | Relação em `app/chamados/models.py:2417`; nascimento em `app/chamados/models.py:3015`. | CSF: `app/ponto/supra_juiz.py:544`; C7: `app/chamados/models.py:1854`. |
| `perguntas_que_resolvem` / `chamado_destino` | Relação em `app/chamados/models.py:3386`; nascimento/reuso em `:3015`, `:2983`; reaponte em `app/chamados/services/fio.py:926`. | CSF: `app/ponto/supra_juiz.py:547`; cartório: `app/ponto/services/cartorio.py:1439`, `:1695`; lote E2: `app/chamados/reconciliador.py:649`. |
| `status_local` | Núcleo `_gravar_estado`: `app/chamados/models.py:228`; nascimento: `app/chamados/models.py:1096`. | Motor: `app/chamados/models.py:674`; filtro novo: `app/chamados/reconciliador.py:651`; S88: `app/chamados/signals.py:34`. |
| `VIVOS` / `ENCERRADOS` | Fonte: `app/chamados/catalogo/motor.py:24`, `:30`. | Cartório usa VIVOS em `app/ponto/services/cartorio.py:1434`; helper E2 usa ENCERRADOS em `app/chamados/reconciliador.py:651`. |
| `fechada_em` | Fecha em `app/chamados/models.py:1933`; reabre em `:2150`, `:2174`. | E2: `app/chamados/reconciliador.py:620`, `:621`; S88: `app/chamados/signals.py:29`. |
| `materializada_em`, `materializada_por`, `via_resolucao` | Atribuição conjunta em `app/chamados/models.py:3098`; persistência do carimbo em `:3153`; reversão em lote em `:3265`. | Seleção E2 em `app/chamados/reconciliador.py:663`; C7 em `app/chamados/models.py:1844`; retenção em `app/chamados/juizes.py:2019`. |
| `pode_executar_csf` | Definição pura em `app/ponto/supra_juiz.py:539`. | Supra em `app/ponto/management/commands/supra_juiz.py:261`; cartório em `app/ponto/services/cartorio.py:965`; E2 em `app/chamados/reconciliador.py:651`. |
| `_pais_com_conversa` | Produz mapa por disputa em `app/chamados/reconciliador.py:650`. | E2 em `app/chamados/reconciliador.py:617`, `:621`, `:626`; teste de custo em `app/ponto/tests/test_o168_guarda_csf_no_evento.py:282`. |
| `_fecham_cobrancas_sem_furo` | Produz lista apenas dos fechamentos confirmados em `app/ponto/services/cartorio.py:968`. | Quatro sítios: `app/ponto/services/cartorio.py:1054`, `:1589`, `:1602`, `:1623`. |
| E2: retorno e `ao_preservar` | Retorno continua número de perguntas carimbadas em `app/chamados/reconciliador.py:607`, `:634`; callback em `:626`. | Cartório em `app/ponto/services/cartorio.py:1050`; consumidor em `app/chamados/reconciliador.py:1399`; reconciliador em `:1466`. |
| `_csf_preservados_vistos` / `CHAVES_DO_ATO` | Criação/adição em `app/ponto/services/cartorio.py:978`; compartilhamento em `:1131`; declaração em `:2001`. | Deduplicação em `app/ponto/services/cartorio.py:979`; drenagem em `:1150`; selo em `app/ponto/tests/test_out_do_lote.py:195`, `:203`. |
| Filas/contadores existentes | Fechos alimentam `reconciliados`/`rejulgar` em `app/ponto/services/cartorio.py:1056`; corpo escreve `cods` em `:1527`. | Segunda passada em `app/ponto/services/cartorio.py:1060`; releitura/consolidação em `:1136`; devolução ao lote em `:1166`. |
| Tetos, registro de linhas e manifesto E4B | Literais alterados em `app/ponto/tests/test_selo_chokepoint_escrita.py:196`, `:210`; `app/core/tests/test_medida_turno_do_par.py:322`; `app/chamados/tests/e4b_c12_sitios.json:261`. | Cobrança do teto em `app/ponto/tests/test_selo_chokepoint_escrita.py:292`; igualdade das linhas em `app/core/tests/test_medida_turno_do_par.py:547`; manifesto em `app/chamados/tests/e4b_c12_verificar.py:17`, `:85`. |

O quadro identifica as portas e consumidores discriminados nesta auditoria; **censo global exaustivo de todos os leitores legados desses campos: [nao verificado]**.

**5. Classes que já reprovaram obra**

| Classe | Resultado |
|---|---|
| Segundo juiz duplicando autoridade | **Ausente nos helpers novos:** ambos chamam CSF; C7 continua registrado separadamente. (`app/ponto/services/cartorio.py:965`, `app/chamados/reconciliador.py:651`, `app/core/juizes.py:279`.) |
| Segundo escritor fora da porta | **Ausente no delta lido:** mantém `transicionar_com_pauta`, `carimbar` e `fechar`. (`app/ponto/services/cartorio.py:968`, `app/chamados/reconciliador.py:604`, `:629`.) |
| Relógio fora da borda | **Nenhuma leitura nova nos helpers.** A cadeia preexistente tem relógios em `carimbar`, `fechar` e `julgar_celula`; conformidade global com a borda **[nao verificado]**. (`app/chamados/models.py:3125`, `:1933`, `app/ponto/services/cartorio.py:2067`, `:2091`.) |
| Derivação paralela de fonte única | **Ausente nos helpers; permanece isenção não equivalente no FIO-DUP:** texto nas resolvedoras versus carimbo também na origem. (`app/ponto/services/cartorio.py:1691`, `app/ponto/supra_juiz.py:543`, `:547`.) |
| Selo/RED sem mordida | **Há pares controle/protegido de estado final** para os quatro sítios e cascata; o teste do evento seguinte declara provar somente estabilidade. Execução dos REDs/mutantes **[nao verificado]**. (`app/ponto/tests/test_o168_guarda_csf_no_evento.py:114`, `:122`, `:131`, `:136`, `:143`, `:148`, `:157`, `:162`, `:227`, `:236`, `:259`.) |
| Ordem de trava | **Nenhuma inversão nova identificada:** E2 mantém trava do colaborador antes do carimbo; fechamento chama a porta de ordem. Implementação ordena Colaborador→Disputa→Chamado→Pergunta. (`app/chamados/reconciliador.py:601`, `app/chamados/models.py:1911`, `:2667`, `:2676`, `:2689`, `:2693`.) |
| Pouso antes do pré-requisito | **[nao verificado]**; suíte inteira precede opus, e Codex não aprova sozinho o pouso. (`CLAUDE.md:377`, `CLAUDE.md:380`.) | [nao verificado]
| ESMERIL-SEMÂNTICA | **Presente:** “pai vivo preservado” admite terminais absolutos e pode alimentar o contador. (`app/chamados/reconciliador.py:638`, `:651`, `:626`, `app/chamados/catalogo/motor.py:31`.) |

**6. Leis dos sítios**

- **O168 não está universalmente atendida nos caminhos solicitados:** persistem fechamentos sem consulta à guarda, especialmente lastro no consumidor e cascata S143/S128. (`app/ponto/services/lastro.py:401`, `app/chamados/services/disputa_emissao.py:2092`.)
- **Contador == universo:** há divergência demonstrável entre o rótulo “vivo preservado” e a seleção de pais terminais; é incompatível com o contrato anunciado e com a exigência de o rótulo dizer o que a conta faz. (`CLAUDE.md:35`, `app/ponto/services/cartorio.py:974`, `app/chamados/reconciliador.py:651`.)
- **L-061 e C7:** os helpers novos conservam leitura da célula e consulta aos juízes existentes; não identifiquei novo julgamento paralelo da lâmpada nesses helpers. (`app/docs/LEIS.md:95`, `app/chamados/reconciliador.py:604`, `:628`, `:661`.)
- **RELÓGIO-INJETADO:** o delta não acrescenta relógio; conformidade da cadeia inteira **[nao verificado]**, pelos sítios preexistentes discriminados acima. (`app/docs/CORTES.md:370`, `app/chamados/models.py:3125`, `app/ponto/services/cartorio.py:2067`.)
- **L-073, L-124, L-115 e L-113:** conformidade geral **[nao verificado]**; esta leitura não certifica teto temporal, horizonte, pareamento e cálculo/folha por ausência de alteração nesses mecanismos. (`app/docs/LEIS.md:100`, `:145`, `:151`, `:153`.)
- **E2-EMISSORES-COMPLEMENTO, LEI-TRANCA-E2-JUIZ e DOIS-AGENTES:** nenhuma violação adicional demonstrada nesta leitura; atendimento integral **[nao verificado]**. (`app/docs/CORTES.md:360`, `:427`, `:70`, `:259`.)

## 12/11 16:50 · sessao 151f9586a29f · - · - · origem=Ronald (tmux)

Censo ATA no perímetro literal conferido: **12 chaves de topo, 95 operações R/W, 27 arquivos**; excluindo `management`, **86 operações em 23 arquivos**. Unidade: operação AST com chave literal, incluindo escrita em construtor e em cópia apenas em memória; excluídos cópia integral `*`, chaves dinâmicas, aninhadas e lookup ORM. [não verificado: estatística reproduzida em Python em memória, sem arquivo de resultado]

| Chave | R | W | Tipo produzido | Escritor |
|---|---:|---:|---|---|
| `tipo_dia` | 5 | 1 | string ou `None` | `app/ponto/services/cartorio.py:738:ata_do_dia` |
| `lampadas` | 40 | 3 | lista de dict | `app/ponto/services/cartorio.py:738:ata_do_dia`; `app/ponto/services/cartorio.py:917:_ata_ate_o_teto`; `app/escala/utils.py:1892:montar_grade_prevista_periodo_por_turno` |
| `minutos_previstos` | 1 | 1 | int | `app/ponto/services/cartorio.py:739:ata_do_dia` |
| `minutos_realizados` | 3 | 1 | int | `app/ponto/services/cartorio.py:740:ata_do_dia` |
| `sem_ancora` | 1 | 1 | bool | `app/ponto/services/cartorio.py:741:ata_do_dia` |
| `n_orfas` | 4 | 1 | int | `app/ponto/services/cartorio.py:742:ata_do_dia` |
| `orfas` | 10 | 2 | lista de strings HH:MM | `app/ponto/services/cartorio.py:745:ata_do_dia`; `app/escala/utils.py:1895:montar_grade_prevista_periodo_por_turno`; formatação em `app/escala/utils.py:1832` |
| `n_missing` | 3 | 3 | int | `app/ponto/services/cartorio.py:746:ata_do_dia`; `app/ponto/services/cartorio.py:812:dispensar_por_ausencia`; `app/ponto/services/cartorio.py:917:_ata_ate_o_teto` |
| `n_celulas` | 4 | 1 | int | `app/ponto/services/cartorio.py:762:ata_do_dia` |
| `cobertura_parcial` | 3 | 1 | bool, `True` quando presente | `app/ponto/services/cartorio.py:767:ata_do_dia` |
| `periodo_parcial` | 3 | 1 | enum string: `sem_periodo`, `dentro`, `fora`, `nao_sei` | `app/ponto/services/cartorio.py:771:ata_do_dia`; origem `app/ponto/turnos.py:2618` |
| `precedencia` | 1 | 1 | dict | `app/ponto/portas/celula.py:1084:carimbar_precedencia`; conteúdo em `app/ponto/services/cartorio.py:2000` |

Os dois escritores de `app/escala/utils.py` são cópia em memória: a cópia do objeto está em `app/escala/utils.py:1886`; os demais caminhos de persistência passam pela porta que atribui `cel.ata` em `app/ponto/portas/celula.py:1006` e `app/ponto/portas/celula.py:1033`.

`lampadas` contém `tipo`, `hora`, `acesa`, `luz`, `tipo_real`, mais opcionais `isencao`, `dispensada`, `pendente`: construção em `app/ponto/services/cartorio.py:702`, `app/ponto/services/cartorio.py:720`; opcionais em `app/ponto/services/cartorio.py:591`, `app/ponto/services/cartorio.py:596`, `app/ponto/services/cartorio.py:915`. `luz` guarda HH:MM ou `None`; `hora` é marco previsto ou rótulo posicional `·I#`. `app/ponto/services/cartorio.py:703`; `app/ponto/juiz_batida.py:193`

Comparações de hora sem data comprovadas:

- `orfas`: ordenação e distância de minutos em `app/ponto/services/cluster_espurio.py:34` e `app/ponto/services/cluster_espurio.py:35`; pertinência de HH:MM em `app/ponto/services/vigia_pareador_celula.py:110` e `app/ponto/services/vigia_pareador_celula.py:111`.
- `lampadas.hora`: igualdade com hora pedida em `app/ponto/services/lastro.py:90`; igualdade entre marcos ATA/DNA em `app/ponto/juiz_batida.py:230`; comparação com DNA em `app/ponto/services/cartorio.py:848`.
- `lampadas.luz`: resposta `data_hora` perde a data em `app/chamados/juizes.py:1349`; depois compara minutos circulares com a luz em `app/chamados/juizes.py:1358` e `app/chamados/juizes.py:1362`.
- A reconstrução atual da data da luz está em `app/ponto/juiz_batida.py:97` e `app/ponto/juiz_batida.py:215`.

Fora do perímetro literal informado, há leitor ORM de `lampadas.pendente` em `app/chamados/models.py:3356`, projeção `values('ata')` em `app/chamados/reconciliador.py:879` e cópia dinâmica da chave `precedencia` por laço em `app/ponto/portas/celula.py:997`, cuja declaração está em `app/ponto/portas/celula.py:104`.

A diferença aritmética em relação às 72 do aval é **−60 chaves de topo**; a razão da divergência é [não verificado]. A lista completa de leitores por chave e declarações de parâmetros/retornos foi enviada nas mensagens anteriores. [não verificado: registro das mensagens]

Os tipos acima são os produzidos pelo código; o conteúdo efetivamente presente no banco é [não verificado]. A porta aceita `dict(ata)` em `app/ponto/portas/celula.py:995`, e o campo é JSON sem esquema declarado em `app/escala/models.py:1778`.

## 12/11 16:49 · sessao e2e181b5434c · - · - · origem=Ronald (tmux)

Complemento da exploração, só leitura:

1. **Fábrica do consumidor e do cartório alcança a cascata c1 já descoberta.** Consumidor chama fábrica em `app/chamados/reconciliador.py:1421`; fábrica percorre `perguntar_por_eventos` → `perguntar_pela_celula` → `perguntar` em `app/chamados/services/fabrica_da_celula.py:301`, `:219`, `:172-174`. A geração de perguntas chama `reatar_pergunta_orfa` em `app/chamados/services/disputa_emissao.py:459-464`; este pode chamar `lavrar_sem_turno_aberto` em `app/chamados/reconciliador.py:1653`, que fecha disputa sem consultar CSF em `:412-414`. O cartório chama a mesma fábrica em `app/ponto/services/cartorio.py:1076-1088`.

2. **Há cascata adicional S143/S128 → disputa.fechar → S88 sem CSF.** O ramo histórico de reatar chama `resolver_perguntas_por_chamado_resolvido` em `app/chamados/reconciliador.py:1682`; `_retratar` também o chama em `:537`; qualquer save terminal que escreva status acorda S143 em `app/chamados/signals.py:91-117`. O resolver forma candidatos incluindo todas as disputas abertas do colaborador em `app/chamados/services/disputa_emissao.py:2026-2028`; fecha candidatos pelo C7, com veto de pergunta em voo, em `:2089-2092`. Não chama CSF nesse fecho. O filtro de pai/destino vivo em `:2032-2037` limita o **carimbo sem credor**, não o laço final de fechamento em `:2074-2092`.

3. **Emissor chamado pelo cartório alcança outro auto-fecho.** `julgar_celula` injeta `detectar_ausencias.Command` em `app/ponto/services/cartorio.py:2066`, `:2091-2092`; corpo chama `_abrir_chamado` em `:1841`, `:1879`. Esse emissor chama `criar_disputa_a_partir_de_chamados_orfaos` em `app/ponto/management/commands/detectar_ausencias.py:792-803`. A porta de repasse fecha a disputa automaticamente quando C7 permite e não houve abstenção por ata em `app/chamados/services/disputa_emissao.py:1270-1287`, sem CSF.

4. **Agrupamento LIMBO também pode fechar pelo ramo do emissor**, com regra própria: emissor chama `agrupar_furo` em `app/ponto/management/commands/detectar_ausencias.py:759-763`; agrupador reconcilia em `app/chamados/services/limbo_folgas.py:284-287`, depois da costura das vivas e do carimbo de agrupamento (`:275-283`), sem CSF. É agrupamento sob pai vivo, portanto não afirmo que seja fecho “sem furo” abrangido pela lei O168: **[nao verificado]**.

5. **Outro ramo explícito do cartório é cluster:** chama `cluster_espurio.reconciliar_colab` em `app/ponto/services/cartorio.py:1081-1085`; esse reconcilia chamados que a premissa matou em `app/ponto/services/cluster_espurio.py:187-191`, sem CSF. O módulo é `cluster_batida_espuria` (`:11`), diferente da cobrança sem furo tratada no novo helper; aplicabilidade exata da lei O168 a esse domínio: **[nao verificado]**.

6. **Não encontrei fecho direto no dedup de nascimento de pergunta lido.** O reuso atualiza célula/destino e retorna em `app/chamados/models.py:2922-3009`; nascimento grava pergunta e retorna em `:3014-3032`. O núcleo de criação/agregação de disputa reutiliza aberta ou cria nova, sem fechar, em `app/chamados/services/disputa_emissao.py:530-603`. O fecho relevante está nos ramos acima, e não nessas operações de dedup.

São caminhos estruturais com chamada real de fechamento, sujeitos às demais guardas. Ocorrência concreta, resultado de execução e existência de dados que atravessem todas as condições: **[nao verificado]**.

## 12/11 16:49 · sessao b4c3d76f5cee · - · - · origem=Ronald (tmux)

DNA: 9 chaves de topo escritas pelo produtor; `DNA_CHAVES` declara apenas 8 e omite `extra_declarada`. Referências: `app/ponto/management/commands/gerar_celulas.py:17`, `:95`, `:122`.

| Chave | Escritor em `gerar_celulas.py:montar_dna` | Tipo produzido | Leitor por chave |
|---|---|---|---|
| `dna_versao` | :96 | int, atualmente 2 (:16) | `app/ponto/services/cartorio.py:484:impressao_insumos` |
| `vinculo_id` | :97 | ID do vínculo | Nenhum acesso por chave identificado; blob inteiro é comparado em `app/ponto/portas/celula.py:263`. |
| `tipo_escala_id` | :98 | ID do tipo | Nenhum acesso por chave identificado; comparação do blob em `app/ponto/portas/celula.py:263`. |
| `tipo_ciclo` | :99 | string (`app/escala/models.py:117`) | `app/ponto/services/vigia_entrada.py:129:do_dia` |
| `marcos` | :100 | dict ou null (:77,:83) | 26 acessos AST, lista abaixo. |
| `minutos_jornada` | :101 | int (`app/escala/models.py:49`) | Nenhum acesso por chave identificado; comparação do blob em `app/ponto/portas/celula.py:263`. |
| `intervalo_livre` | :102 | bool | Nenhum acesso por chave identificado; comparação do blob em `app/ponto/portas/celula.py:263`. |
| `lampadas` | :103 | list[dict] ou null (:78,:94) | `app/escala/utils.py:153:lampadas_de_dna` |
| `extra_declarada` | :122 | dict `{antes:int,depois:int}` (:119,:120) | `app/escala/models.py:1235:extra_declarada_do_dia` |

Esses são os tipos produzidos pelo código; conteúdo efetivamente armazenado em produção: [não verificado].

Leitores de `marcos`, contando chamadas AST e preservando duas chamadas na linha 159:

```text
app/chamados/juizes.py:948:fato_do_marco_no_espelho
app/chamados/juizes.py:1338:_resposta_concorda_com_a_luz
app/chamados/services/adesao.py:36:universo_adesao
app/escala/models.py:1124:marcos_do_dia
app/escala/models.py:1189:pausas_do_dia
app/escala/utils.py:53:pausas_dna_periodo
app/escala/utils.py:104:marcos_dna_periodo
app/escala/utils.py:137:pausas_de_celulas
app/pautas/services.py:424:contexto_do_dia_texto
app/ponto/juiz_batida.py:159:periodos_do_dia [2 chamadas]
app/ponto/management/commands/aplicar_col900.py:172:handle
app/ponto/management/commands/censo_col900.py:118:handle
app/ponto/management/commands/censo_col900.py:119:handle
app/ponto/management/commands/detectar_intervalo_ausente.py:53:furos_intervalo
app/ponto/management/commands/tripwire_celulas.py:99:medir_pausa_faltando
app/ponto/management/commands/tripwire_celulas.py:259:medir_sobra_com_tipo_trocado
app/ponto/services/cartorio.py:484:impressao_insumos
app/ponto/services/cartorio.py:615:lampadas_do_dia
app/ponto/services/cartorio.py:840:insumos_da_ata
app/ponto/services/dia_do_colab.py:46:dia_do_colab
app/ponto/services/diagnostico_escala.py:153:diagnostico_escala
app/ponto/services/esmeril_espelho.py:116:assinaturas_do_colab
app/ponto/services/faltas_de_hoje.py:43:faltas_de_hoje_sem_cobranca
app/ponto/services/veredito_celula.py:31:_marcos_do_dia
app/ponto/services/vigia_entrada.py:126:do_dia
```

As subchaves `marcos.hi/hf/hii/hfi` contêm HH:MM ou null; `hii2/hfi2` são opcionais. Escritas: `app/ponto/management/commands/gerar_celulas.py:83`, `:93`; vocabulário: `app/escala/models.py:1652`.

`lampadas[]` contém `{chave,tipo,hora,instante}`: `hora` é HH:MM; `instante` é ISO **sem fuso**, obtido por `datetime.combine(...).isoformat(...)`. Referências: `app/ponto/management/commands/gerar_celulas.py:68`, `:69`.

Comparação de hora sem data existe para `marcos`:

```text
app/ponto/services/cartorio.py:640:lampadas_do_dia [pertinência]
app/ponto/services/cartorio.py:648:lampadas_do_dia [chave de dict]
app/ponto/services/cartorio.py:650:lampadas_do_dia [consulta de dict]
app/ponto/services/cartorio.py:657:lampadas_do_dia [consulta de dict]
app/ponto/services/cartorio.py:666:lampadas_do_dia [distância em minutos]
app/ponto/services/cartorio.py:846:insumos_da_ata._um [igualdade; hi/hf passados em :855]
app/chamados/juizes.py:965:fato_do_marco_no_espelho [chave de dict]
app/chamados/juizes.py:968:fato_do_marco_no_espelho [consulta de dict]
app/chamados/juizes.py:1345:_resposta_concorda_com_a_luz [igualdade]
app/ponto/juiz_batida.py:230:periodos_do_dia [igualdade dos minutos]
app/ponto/services/vigia_entrada.py:127:do_dia [comparação hi-hf]
app/ponto/services/dia_do_colab.py:47:dia_do_colab [pertinência]
app/ponto/management/commands/censo_col900.py:120:handle [igualdade do dict]
```

Também existe para `lampadas`: `app/escala/utils.py:155:lampadas_de_dna` usa `lamp['hora']` como chave de dict.

Comparações do blob inteiro incluem `app/ponto/portas/celula.py:263`, `:594`, `:872` e `app/ponto/management/commands/tripwire_celulas.py:30`; portanto ausência de leitor individual não significa ausência de uso.

Mapa para o instrumento:

```text
PRODUTOR:
app/ponto/management/commands/gerar_celulas.py:73:montar_dna
  retorna dna em :123

PARÂMETROS DECLARADOS DNA:
app/escala/utils.py:149:lampadas_de_dna                 posição0 dna
app/ponto/services/cartorio.py:471:impressao_insumos    posição3 dna
app/ponto/services/cartorio.py:599:lampadas_do_dia      posição2 dna
app/ponto/services/cartorio.py:730:ata_do_dia           posição2 dna
app/ponto/services/cartorio.py:816:insumos_da_ata       posição1 dna

DESEMPACOTAMENTO ORM:
app/escala/utils.py:52:pausas_dna_periodo
app/escala/utils.py:103:marcos_dna_periodo
app/escala/utils.py:136:pausas_de_celulas
app/escala/utils.py:164:lampadas_dna_periodo
app/chamados/services/adesao.py:33:universo_adesao

APELIDO DE RETORNO:
app/ponto/management/commands/censo_col900.py:115:handle
  cand = montar_dna(...); leitura cand.get('marcos') em :118
```

`alvo` em `app/ponto/portas/celula.py:783` vem de `dna_anterior`; a restauração escreve o blob inteiro em `:786`, lê `marcos` em `:788` e `vinculo_id` em `:789`. Esses leitores não foram contados como acessos ao DNA atual.

Contagem AST limitada aos acessos explícitos às nove chaves de topo, em `app/**/*.py`, sem testes nem migrations: 30 leitores em 17 arquivos + 9 escritores em um arquivo adicional = 39 acessos em 18 arquivos; subtotal management = 15; restante = 24. Os locais acima e o literal escritor em `app/ponto/management/commands/gerar_celulas.py:95` sustentam a contagem. Não inclui blob inteiro, DNA anterior ou chaves aninhadas.

## 12/11 16:48 · sessao 345dbb2b82ed · TRANCA-VIVAS-08 · sonda · origem=Code

**O dado recebido não lista nenhuma VIVA para autopsiar; as quatro citadas no aval não vêm acompanhadas de PKs ou campos individuais. A causa delas permanece [não verificado].** A exceção para toque humano posterior à tranca é lei expressa na cópia, mas isso não prova que tenha ocorrido nesses quatro casos. [app/docs/CORTES.md:83](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/docs/CORTES.md:83)

**1. Saída da sonda**

Não reexecutei a sonda nesta sessão. O mecanismo `--sonda` executa o envelope antes de chamar este leitor e entrega a saída raspada no prompt. [bin/codex_pergunta.sh:15](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/bin/codex_pergunta.sh:15), [bin/codex_pergunta.sh:325](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/bin/codex_pergunta.sh:325)

Segue a transcrição integral da saída fornecida; execução e valores não foram verificados independentemente. [não verificado]

```text
sonda_leitura: sonda_vivas08.py [796d8c68faf664cb] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

agora 2026-11-12T16:38:40.927867-03:00
FROTA PF vivos=15 vivas_apesar_da_tranca total=0
  emp2 04/2026 PF#7 fechado_em=2026-11-12 18:05:50.735140+00:00 vivas=0 []
  emp2 05/2026 PF#8 fechado_em=2026-11-12 18:06:50.433372+00:00 vivas=0 []
  emp2 06/2026 PF#9 fechado_em=2026-11-12 18:07:49.785772+00:00 vivas=0 []
  emp2 07/2026 PF#10 fechado_em=2026-11-12 18:09:18.110150+00:00 vivas=0 []
  emp2 08/2026 PF#3 fechado_em=2026-09-16 12:11:43.807307+00:00 vivas=0 []
  emp3 04/2026 PF#11 fechado_em=2026-11-12 18:10:56.216540+00:00 vivas=0 []
  emp3 05/2026 PF#12 fechado_em=2026-11-12 18:11:08.343652+00:00 vivas=0 []
  emp3 06/2026 PF#13 fechado_em=2026-11-12 18:11:20.561701+00:00 vivas=0 []
  emp3 07/2026 PF#4 fechado_em=2026-09-16 12:11:43.836459+00:00 vivas=0 []
  emp3 08/2026 PF#5 fechado_em=2026-09-16 12:11:43.853839+00:00 vivas=0 []
  emp4 04/2026 PF#14 fechado_em=2026-11-12 18:11:33.013144+00:00 vivas=0 []
  emp4 05/2026 PF#15 fechado_em=2026-11-12 18:11:35.049693+00:00 vivas=0 []
  emp4 06/2026 PF#16 fechado_em=2026-11-12 18:11:59.631123+00:00 vivas=0 []
  emp4 07/2026 PF#2 fechado_em=2026-07-31 18:45:51.703839+00:00 vivas=0 []
  emp4 08/2026 PF#6 fechado_em=2026-09-16 12:11:43.867520+00:00 vivas=0 []
total_erros=0
```

O texto reporta `total_erros=0`, portanto não apresenta a condição `total_erros>0` indicada no pedido. A cobertura efetiva da sonda, sem seu código-fonte, permanece [não verificado].

**2. Degraus, competência e exceção do admin**

Não existe linha `VIVA` no recorte para atribuir um retorno concreto de `_pergunta_viva_calc`. Atribuir resposta, conversa, veredito ou toque às quatro históricas seria [não verificado].

A ordem comprovada no código é:

| Etapa | Resultado e condição |
|---|---|
| Validação | `validada_em` preenchido devolve `False`. [juizes.py:1391](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1391) |
| Tranca | `dia_da_pergunta_trancado(p)` verdadeiro devolve `False`, **antes da resposta**. [juizes.py:1395](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1395) |
| Respondida | Havendo carimbo e texto, sem decisão explícita do admin na célula, devolve `True` quando a concordância com a luz não é comprovadamente verdadeira. [juizes.py:1405](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1405), [juizes.py:1419](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1419) |
| Célula acusando | Superadas as guardas anteriores, `veredito` em `VEREDITOS_ACUSAM` devolve `True`. [juizes.py:1440](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1440) |
| Falta pela lei | Dia dentro da janela viva e `classificar_falta(...)[1]` não nulo devolvem `True`. [juizes.py:1454](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1454) |
| Sem materialização | Superados os anteriores, `materializada_em is None` devolve `True`. [juizes.py:1466](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1466) |
| Testemunha externa | Último retorno é a negação de `_pergunta_materializada_de_fato`; portanto devolve `True` quando essa autoridade devolve falso. [juizes.py:1473](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1473) |

A tranca pode não calar porque:

- **É conversa:** `_dia_de_turno_da_pergunta` devolve `None`, mesmo havendo `data_evento` civil. [juizes.py:334](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:334)
- **Faltam dia, colaborador ou empresa:** o par não entra na avaliação e conserva `False`. [juizes.py:1023](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1023)
- **Não há PF vivo para a competência calculada:** somente PF encontrado segue para a decisão do toque. [juizes.py:1062](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1062)
- **Há toque humano posterior reconhecido:** o resultado da tranca é `not tocou`. [juizes.py:1074](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1074)

“Humano” nesse predicado significa `LogAuditoria.usuario_id` não nulo e diferente do usuário de sistema, com `criado_em` **estritamente maior** que `PF.fechado_em`. As ações reconhecidas são cobrar aquele dia, resolver a célula daquele dia ou desarquivar chamado ligado à célula daquele colaborador/dia. O predicado não exige consulta a `is_staff` nesses filtros. [juizes.py:1252](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1252), [juizes.py:1261](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1261), [juizes.py:1287](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1287), [juizes.py:1300](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1300)

Isso é **exceção legal ao silêncio daquele dia**, registrada no C20; não reabre o PF. A reabertura do período ocorre em outra porta, escrevendo `reaberto_em`. [core/juizes.py:156](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/core/juizes.py:156), [fechamento.py:1432](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/services/fechamento.py:1432)

A comparação com `medir()` revela duas questões distintas:

| Questão | Constatação |
|---|---|
| Pergunta de fato | O juiz lê `data_evento` como dia do turno. Tanto sua `janela_atual` quanto a `janela_fechamento` de `medir()` usam o corte cadastrado da empresa; não são dois cálculos independentes de competência. [juizes.py:338](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:338), [ponto/janelas.py:54](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/janelas.py:54) |
| Conversa | `medir()` seleciona por `data_evento` bruto, sem excluir `conversa`; a tranca devolve dia `None` para conversa. **Há diferença comprovada de universo e significado nesse caminho.** [competencia_trancada.py:107](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/services/competencia_trancada.py:107), [juizes.py:338](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:338) |
| Contador | `medir()` acrescenta VIVAs a `vivas_apesar_da_tranca` somente no ramo de destino/pai resgatado. Fora dele, acrescenta apenas as mortas a `perguntas_lidas_trancadas`; uma viva não recebe entrada nesse contador. Logo seu zero não certifica sozinho todas as perguntas selecionadas. [competencia_trancada.py:119](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/services/competencia_trancada.py:119), [competencia_trancada.py:130](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/services/competencia_trancada.py:130) |

Portanto: **toque posterior reconhecido é lei; interpretar o dia civil de conversa como competência de fato seria ESMERIL-SEMÂNTICA entre esses leitores.** Qual dessas situações explica as quatro perguntas — e se a sonda usa esse contador de `medir()` — permanece [não verificado].

**3. Quem escreve e quem lê os campos**

O mapa abaixo identifica os campos diretos do C6 e da tranca; os leitores citados são os pertinentes à decisão investigada. [juizes.py:1389](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1389), [juizes.py:1004](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1004)

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `data_evento` | Atribuição em `fio.gravar_dia_do_fato`; nascimento salva em `_nascer`. [fio.py:801](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/services/fio.py:801), [models.py:3017](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/models.py:3017) | Discriminador do turno e filtro de `medir`. [juizes.py:338](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:338), [competencia_trancada.py:108](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/services/competencia_trancada.py:108) |
| `conversa` | `abrir` passa falso; `abrir_conversa` passa verdadeiro e dia civil; `_nascer` persiste. [models.py:2790](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/models.py:2790), [models.py:2802](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/models.py:2802), [models.py:3015](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/models.py:3015) | `_dia_de_turno_da_pergunta`. [juizes.py:338](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:338) |
| `validada_em` | Validação, devolução e `carimbar(resposta='validar')`. [validacao.py:134](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/services/validacao.py:134), [validacao.py:199](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/services/validacao.py:199), [models.py:3136](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/models.py:3136) | Projeção temporal e primeira guarda. [juizes.py:1375](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1375), [juizes.py:1391](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1391) |
| `resposta_colab`, `respondida_em` | Resposta humana, automática e recuperação pela trilha; `carimbar` também preenche o timestamp. [disputa_emissao.py:1467](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/services/disputa_emissao.py:1467), [disputa_emissao.py:1652](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/services/disputa_emissao.py:1652), [disputa_emissao.py:1674](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/services/disputa_emissao.py:1674), [models.py:3133](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/models.py:3133) | Projeção e ramo da respondida. [juizes.py:1375](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1375), [juizes.py:1407](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1407) |
| `materializada_em`, `via_resolucao` | `_escrever_carimbo`, portas que salvam, limpeza em lote e alteração isolada da via por fato no espelho. [models.py:3098](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/models.py:3098), [models.py:3153](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/models.py:3153), [models.py:3265](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/models.py:3265), [models.py:3276](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/models.py:3276) | Guardas de via, marco e materialização. [juizes.py:1402](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1402), [juizes.py:1428](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1428), [juizes.py:1466](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1466) |
| `motivo`, `tipo_resposta`, `celula`, `disputa` | Nascimento da pergunta; reuso pode reapontar a célula. [models.py:3015](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/models.py:3015), [models.py:2967](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/models.py:2967) | Marco, comparação da resposta e colaborador da tranca. [juizes.py:941](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:941), [juizes.py:1332](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1332), [juizes.py:991](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:991) |
| Célula `veredito`, `veredito_via` | Porta `lavrar_veredito`. [celula.py:1035](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/portas/celula.py:1035) | Decisão do admin e acusação do dia. [juizes.py:1405](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1405), [juizes.py:1438](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1438) |
| Célula `dna.marcos`, `ata.lampadas` | Portas de geração/edição do DNA e lavra da ata; cartório marca pendência temporal. [celula.py:260](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/portas/celula.py:260), [celula.py:604](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/portas/celula.py:604), [celula.py:1033](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/portas/celula.py:1033), [cartorio.py:915](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/services/cartorio.py:915) | Lê `hora`, `tipo`, `acesa`, `luz` e `pendente`. [juizes.py:948](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:948), [juizes.py:1344](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1344), [lastro.py:148](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/services/lastro.py:148) |
| Empresa `dia_inicio_competencia` | CRUD repassa às portas de estrutura, que persistem os campos. [colaboradores/views.py:520](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/colaboradores/views.py:520), [estrutura.py:12](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/colaboradores/services/estrutura.py:12) | `corte_da_empresa`, usado pelas duas janelas. [janelas.py:50](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/janelas.py:50) |
| PF `empresa_id`, `mes`, `ano`, `reaberto_em` | `trancar_periodo` cria/retranca; `reabrir_periodo` grava reabertura. [fechamento.py:1408](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/services/fechamento.py:1408), [fechamento.py:1417](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/services/fechamento.py:1417), [fechamento.py:1439](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/services/fechamento.py:1439) | `periodo_trancado`: exige `reaberto_em IS NULL`; `ativo` não decide. [fechamento.py:1464](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/services/fechamento.py:1464) |
| PF `fechado_em` | Automático na criação; atualizado na retranca. [ponto/models.py:926](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/models.py:926), [fechamento.py:1415](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/services/fechamento.py:1415) | Limite do toque e comparação com resgate. [juizes.py:1070](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1070), [competencia_trancada.py:105](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/services/competencia_trancada.py:105) |
| Chamado `resgatado_em` | `arquivo.levantar` conserva o primeiro carimbo. [arquivo.py:191](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/services/arquivo.py:191), [arquivo.py:205](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/services/arquivo.py:205) | `medir` monta `resgatados`; esse campo pode representar mão humana **ou do colaborador**, portanto não prova sozinho toque de admin. [competencia_trancada.py:105](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/services/competencia_trancada.py:105), [arquivo.py:180](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/services/arquivo.py:180) |
| Log `usuario`, `acao`, `modelo`, `objeto_id`, `descricao`, `criado_em` | Escritor comum `core.observ.evento`; timestamp automático do modelo. [observ.py:134](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/core/observ.py:134), [core/models.py:307](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/core/models.py:307) | Filtros de humano, ação, alvo e instante em `_toque_humano_nos_dias`. [juizes.py:1252](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:1252) |

Os produtores concretos das três ações reconhecidas são `cobrar_dia_manual`, `veredito_celula` e a porta de desarquivar que chama `arquivo.levantar`. [cobrar_dia.py:39](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/services/cobrar_dia.py:39), [veredito_celula.py:356](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/services/veredito_celula.py:356), [views_arquivar.py:77](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/views_arquivar.py:77)

O último degrau também consulta testemunhas externas de batida/turno; o mapa acima não é um censo transitivo completo dessas dependências. [juizes.py:662](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:662), [juizes.py:752](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/chamados/juizes.py:752)

## 12/11 16:48 · sessao c2950311afaf · LASTRO-FAIXA-SEM-DEFAULT · leitura · origem=Code

A troca precisa começar antes da lavra: hoje a grade reduz a órfã a `HH:MM`; o cartório recebe essa string e a grava. Alterar apenas a serialização em `ata_do_dia` não recupera data, segundos nem offset. (`app/escala/utils.py:621`, `app/escala/utils.py:662`, `app/ponto/services/cartorio.py:745`)

Usei o hash informado como identificação da cópia; sua igualdade com o HEAD de `main` é [não verificado]. Os anexos foram lidos na mensagem; sua correspondência com arquivos locais é [não verificado].

1. **Fonte e escritores atuais**

A fonte da lavra é `ata_do_dia`, em [cartorio.py:730](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/ponto/services/cartorio.py:730). Ela grava `n_orfas=len(...)` e `orfas=[str(h)...]`; `CelulaDia.ata` é um `JSONField`. (`app/ponto/services/cartorio.py:742`, `app/ponto/services/cartorio.py:745`, `app/escala/models.py:1778`)

O conteúdo produzido hoje é `list[str]` de **HH:MM, sem data, segundos ou fuso**. Antes desse descarte, a grade tem datetimes provenientes de `timezone.localtime(b.timestamp)`; a configuração da cópia declara `America/Sao_Paulo` e `USE_TZ=True`. O fuso efetivamente ativo numa execução de produção é [não verificado]. (`app/escala/utils.py:621`, `app/escala/utils.py:662`, `app/escala/utils.py:683`, `app/escala/utils.py:1323`, `app/config/settings/base.py:93`, `app/config/settings/base.py:95`)

| Escritor ou transportador da ata | Operação atual |
|---|---|
| Cartório — construção | `ata_do_dia` constrói o dict e escreve `orfas` e `n_orfas`. (`app/ponto/services/cartorio.py:738`, `app/ponto/services/cartorio.py:742`, `app/ponto/services/cartorio.py:745`) |
| Porta — substituição persistente | `lavrar_veredito` atribui a ata inteira nos dois ramos; conserva da ata anterior somente chaves de outra operação, atualmente `precedencia`. (`app/ponto/portas/celula.py:1006`, `app/ponto/portas/celula.py:1033`, `app/ponto/portas/celula.py:997`, `app/ponto/portas/celula.py:104`) |
| Porta — precedência | `carimbar_precedencia` copia a ata, muda `precedencia`, atribui a ata inteira e salva; transporta as órfãs sem interpretá-las. (`app/ponto/portas/celula.py:1083`, `app/ponto/portas/celula.py:1084`, `app/ponto/portas/celula.py:1085`, `app/ponto/portas/celula.py:1087`) |
| Grade — ata sintética | Constrói outra ata, **em memória**, para consultar o juiz do turno; inclui `orfas` e também precisa migrar para ISO. A cópia é explicitamente não salva. (`app/escala/utils.py:1886`, `app/escala/utils.py:1887`, `app/escala/utils.py:1895`) |
| Porta — projeção | `celula_como_o_julgamento_deixa` copia a ata inteira para uma célula de leitura, sem persistência. (`app/ponto/portas/celula.py:933`, `app/ponto/portas/celula.py:945`) |
| Cartório — julgamento corrente | Constrói `SimpleNamespace(ata=...)` e publica a ata transitória do julgamento. (`app/ponto/services/cartorio.py:1436`, `app/ponto/services/cartorio.py:1440`) |
| Cartório — alterações internas | `dispensar_por_ausencia` e `_ata_ate_o_teto` alteram lâmpadas/`n_missing`; não substituem a lista de órfãs nesses atos. (`app/ponto/services/cartorio.py:805`, `app/ponto/services/cartorio.py:812`, `app/ponto/services/cartorio.py:913`, `app/ponto/services/cartorio.py:917`) |

O modelo encaminha a lavra à porta. No caminho normal, o cartório chama com `save=False`; `carimbar` efetiva a gravação junto dos demais campos, chegando a `cel.save(update_fields=campos)` e à emissão do evento. (`app/escala/models.py:1819`, `app/escala/models.py:1827`, `app/ponto/services/cartorio.py:1853`, `app/ponto/services/cartorio.py:1873`, `app/ponto/portas/celula.py:1072`, `app/ponto/portas/celula.py:905`, `app/ponto/portas/celula.py:907`)

O censo estático não encontrou outro escritor produtivo de `.ata`, `update(ata=...)` ou `bulk_update` de ata; ausência de escrita por metaprogramação arbitrária é [não verificado]. Os snapshots encontrados leem a ata e escrevem arquivos externos; não são escritores da ata no banco. (`bin/snapshot_o118_col221.py:18`, `bin/snapshot_o118_col221.py:30`, `app/ponto/management/commands/celula_veredito_velho.py:108`, `app/ponto/management/commands/celula_veredito_velho.py:112`)

Para executar a matriz, proponho mudar o transporte **da própria lista `orfas`** para datetimes aware desde `_match_marcos`; preservar o instante nas promoções de célula para órfã; e serializar `t.isoformat()` somente na construção da ata persistível. A ata sintética deve usar a mesma forma ISO. Não há necessidade de outra lista. Os pontos atuais de perda estão em `app/escala/utils.py:621`, `app/escala/utils.py:662`, `app/escala/utils.py:799`, `app/escala/utils.py:1722`, `app/escala/utils.py:1733`, `app/escala/utils.py:1832`, `app/escala/utils.py:1834`.

Há um consumidor **dentro da própria lavra**: `lampadas_do_dia` transforma órfãs em minutos por fatias de string e procura proximidade de até 30 minutos. Ele também precisa receber instantes e compará-los com marcos absolutos da autoridade existente; passar ISO para `_min` quebraria esse caminho. A invariante já documentada é “acesa e órfã são disjuntas”. (`app/ponto/services/cartorio.py:623`, `app/ponto/services/cartorio.py:628`, `app/ponto/services/cartorio.py:666`, `app/ponto/services/cartorio.py:702`)

2. **Leitor único e desconhecimento**

Já existe algo próximo: `_orfas_da_ata(ata)`, em [leitor_celula.py:180](/home/ronald/codex_ws/0a747193e3baedc1ee65ec1bb0b2fd4547728ba2/app/escala/services/leitor_celula.py:180). Hoje ele repassa strings ou fabrica marcadores `'?'` pela contagem; `grade_da_celula` o chama para publicar `dia['orfas']`. (`app/escala/services/leitor_celula.py:187`, `app/escala/services/leitor_celula.py:190`, `app/escala/services/leitor_celula.py:420`)

Proponho substituí-lo pela assinatura pública abaixo, no mesmo módulo; é o encaixe técnico da porta solicitada sobre o leitor existente. (`app/escala/services/leitor_celula.py:180`, `app/escala/services/leitor_celula.py:420`)

```python
def orfas_da_ata(celula: CelulaDia) -> list[datetime.datetime] | None:
    ...
```

O contrato proposto distingue três situações, sem usar a data da linha para completar o dado. A necessidade da distinção já aparece no leitor atual, que recebe atas com apenas contagem. (`app/escala/services/leitor_celula.py:183`, `app/escala/services/leitor_celula.py:190`)

| Retorno proposto | Significado |
|---|---|
| `[]` | Lista explicitamente vazia e sem contagem positiva contraditória: zero órfãs conhecido. Proposta sobre os campos atuais. (`app/ponto/services/cartorio.py:742`, `app/ponto/services/cartorio.py:745`) |
| `[datetime aware, ...]` | Todos os elementos são instantes ISO completos com offset, preservando segundos e ocorrências. Proposta de substituição da leitura atual. (`app/escala/services/leitor_celula.py:187`) |
| `None` | Sem célula/ata/lista; somente HH:MM; somente contagem positiva; elemento inválido ou sem offset; lista incompleta/contraditória. Nenhum elemento parcial é publicado como lista completa. Proposta para substituir a degradação por `'?'`. (`app/escala/services/leitor_celula.py:190`) |

`None` precisa atravessar grade, tela e PDF sem `or []`, `or set()` ou `bool(...)` que o transforme em ausência conhecida; esses padrões existem hoje nos consumidores. (`app/escala/utils.py:1110`, `app/relatorios/pdf_espelho.py:607`, `app/ponto/supra_juiz.py:379`)

Para consumidores que perguntam **quantidade**, proponho transportar também o escalar já lavrado `n_orfas`, hoje ausente da projeção de `grade_da_celula`. Uma ata antiga pode saber “há duas” sem saber quais instantes; isso não autoriza inventar identidades nem exige uma lista paralela. (`app/ponto/services/cartorio.py:742`, `app/escala/services/leitor_celula.py:418`, `app/escala/services/leitor_celula.py:420`, `app/escala/services/furos_diarios.py:22`)

3. **Migração dos leitores atuais**

O censo por AST foi executado em memória sobre os Python da cópia, complementado por busca dos templates; não foi criado artefato de censo. Reprodutibilidade por arquivo salvo e cobertura de acessos dinâmicos são [não verificado].

Os cinco caminhos pedidos exigem as seguintes mudanças nos sítios atuais:

| Caminho | Hoje | Comparação por instante e render final propostos |
|---|---|---|
| Tela/espelho | Lê `grade_da_celula`, adapta a lista e publica `orfas_grade`. Há caminhos que recorrem ao builder para geometria antiga. (`app/ponto/services/espelho.py:519`, `app/ponto/services/espelho.py:489`, `app/ponto/services/espelho.py:689`, `app/ponto/services/espelho.py:844`) | `orfas_grade` transporta a resposta do leitor único, inclusive `None`. Geometria reconstruída não prova órfã lavrada. O HH:MM fica nos templates, que hoje imprimem `o` diretamente. (`app/templates/ponto/espelho.html:151`, `app/templates/ponto/espelho.html:240`) |
| Calendário | Guarda órfãs em set por `(dia,mês)`; compara `b.timestamp.strftime('%H:%M')` e escreve `b.orfa=True`. (`app/colaboradores/services/calendario.py:380`, `app/colaboradores/services/calendario.py:381`, `app/colaboradores/services/calendario.py:622`, `app/colaboradores/services/calendario.py:625`, `app/colaboradores/services/calendario.py:627`) | Localizar a resposta por data completa; reconhecer a batida por timestamp aware. Desconhecimento não vira `False`. O render final já usa `timestamp|date:"H:i"`. (`app/templates/colaboradores/partials/_calendario_grade.html:121`) |
| PDF — entrada espúria | Remonta a grade e une HH:MM de toda a janela; `_entrada_espuria` testa a entrada por HH:MM. (`app/relatorios/pdf_espelho.py:475`, `app/relatorios/pdf_espelho.py:477`, `app/relatorios/pdf_espelho.py:178`, `app/relatorios/pdf_espelho.py:513`) | Consumir a publicação da tela oriunda da porta; testar `periodo.entrada` contra instantes. Entrada de classificação desconhecida continua desconhecida. Proposta nos mesmos sítios. |
| PDF — marcas e alertas | Remonta a grade novamente, associa por `data_fim - índice`, marca `b.orfa` por HH:MM e suprime alertas por HH:MM. (`app/relatorios/pdf_espelho.py:543`, `app/relatorios/pdf_espelho.py:551`, `app/relatorios/pdf_espelho.py:552`, `app/relatorios/pdf_espelho.py:609`, `app/relatorios/pdf_espelho.py:960`, `app/relatorios/pdf_espelho.py:964`) | Usar a resposta já publicada, comparar `b.timestamp`/`pp.entrada` e preservar o estado desconhecido. A data localiza a célula; não completa o instante. HH:MM final permanece na impressão das marcas. (`app/relatorios/pdf_espelho.py:881`, `app/relatorios/pdf_espelho.py:885`) |
| Dry-run de pontas | Obtém `_orf2`, transforma em set e testa `strftime('%H:%M') in _orfset`. (`app/escala/utils.py:1650`, `app/escala/utils.py:1651`, `app/escala/utils.py:1675`) | `_match_marcos` devolve instantes; o teste passa a `locs[k][0] in _orfset`. Manter a cópia de `consumed` e os raios existentes. (`app/escala/utils.py:1649`, `app/escala/utils.py:1672`, `app/escala/utils.py:1676`) |
| Reconciliação | Reconhece/remove órfã por `_hhmm_t`; a exceção de entrada também intersecta horas do turno e ordena strings. (`app/escala/utils.py:2005`, `app/escala/utils.py:2006`, `app/escala/utils.py:2028`, `app/escala/utils.py:2032`, `app/escala/utils.py:2034`, `app/escala/utils.py:2039`) | Reconhecer e remover **uma ocorrência do instante exato**; conjunto de timestamps do turno; comparação com o instante preservado da célula. Conservar as guardas de negócio, inclusive a exclusão noturna atual dessa exceção. (`app/escala/utils.py:2027`) |
| Grade — formação/conservação | `_match_marcos`, sobras e promoções produzem HH:MM; a ordenação é textual. (`app/escala/utils.py:621`, `app/escala/utils.py:662`, `app/escala/utils.py:799`, `app/escala/utils.py:803`, `app/escala/utils.py:1722`, `app/escala/utils.py:1733`, `app/escala/utils.py:1838`) | Conservar datetimes aware na única lista; ordenar cronologicamente. `_celula_do_marco` precisa conservar o instante em memória para promoções posteriores, pois hoje guarda somente a hora formatada. (`app/escala/utils.py:326`) |

Na reconciliação noturna, a seleção atual da lista de saída usa `cross_meianoite` do juiz e `data_turno + 1`. Isso pode continuar localizando a célula correspondente; não pode servir para fabricar a data de uma órfã HH:MM. (`app/escala/utils.py:1958`, `app/escala/utils.py:1971`, `app/escala/utils.py:1973`)

Há também duas perdas de apresentação: `montar_realizado_grade` devolve `[], []` quando faltam marcos, descartando órfãs; os templates colocam as órfãs dentro do ramo que exige `realizado_grade`. A lista/ignorância precisa sobreviver ao adaptador e renderizar independentemente de haver células realizadas. (`app/escala/utils.py:1084`, `app/escala/utils.py:1110`, `app/templates/ponto/espelho.html:151`, `app/templates/ponto/espelho.html:231`, `app/templates/ponto/espelho.html:240`)

O PDF ainda suprime anomalias procurando **substring HH:MM**. A fonte dessas mensagens já descartou o instante no adaptador do motor; portanto acrescentar `DD/MM` ao filtro não atende à matriz. Proponho preservar no produtor um fato de anomalia com tipo e instante, transportar esse fato e formatar a mensagem no render; mensagens antigas sem identidade não podem ser casadas por texto. (`app/relatorios/pdf_espelho.py:716`, `app/relatorios/pdf_espelho.py:719`, `app/relatorios/pdf_espelho.py:720`, `app/ponto/adaptador_motor.py:33`, `app/ponto/adaptador_motor.py:35`, `app/ponto/adaptador_motor.py:40`, `app/ponto/adaptador_motor.py:42`)

O restante do censo de **leitores diretos da ata**, além de `_orfas_da_ata`, é este:

| Leitor direto | Uso atual e migração necessária |
|---|---|
| `periodos_do_dia` | Copia strings para seu resultado. Passar pelo leitor único e conservar `None`. (`app/ponto/juiz_batida.py:161`, `app/ponto/juiz_batida.py:164`) |
| `_celula_furada` | Testa lista ou `n_orfas`. Retirar o acesso direto à lista; conservar a informação de existência quando a contagem lavrada a prova. (`app/chamados/juizes.py:510`) |
| Materialização | Filtra órfãs HH:MM, mistura com luzes e alimenta o propositor; o plantio também vira HH:MM. Migrar a atribuição para coordenadas temporais que preservem o instante; dado incompleto não absolve por uma lista parcial. (`app/chamados/services/materializacao.py:177`, `app/chamados/services/materializacao.py:181`, `app/chamados/services/materializacao.py:182`) |
| Cluster espúrio | Lê a lista e calcula rajada por minutos extraídos da string. Usar o leitor único e diferenças entre instantes; `None` conserva “não medido”, já previsto nessa API. (`app/ponto/services/cluster_espurio.py:17`, `app/ponto/services/cluster_espurio.py:29`, `app/ponto/services/cluster_espurio.py:31`, `app/ponto/services/cluster_espurio.py:34`, `app/ponto/services/cluster_espurio.py:35`) |
| Esmeril A5 | Agrupa recorrência em faixas de 30 minutos, com pelo menos três ocorrências. Ler instantes pela porta e classificá-los em faixas temporais aware da data real; agregar a categoria da faixa entre dias, preservando a regra de recorrência. HH:MM fica no rótulo final. (`app/ponto/services/esmeril_espelho.py:19`, `app/ponto/services/esmeril_espelho.py:125`, `app/ponto/services/esmeril_espelho.py:128`, `app/ponto/services/esmeril_espelho.py:139`) |
| Vigia pareador/célula | Carrega órfãs diretamente e identifica borda por HH:MM. Carregar pela porta, comparar `b.timestamp` e registrar legado como não medido. (`app/ponto/services/vigia_pareador_celula.py:74`, `app/ponto/services/vigia_pareador_celula.py:109`, `app/ponto/services/vigia_pareador_celula.py:111`) |
| Tripwire de células | Peneira por HH:MM circular; até `confirmada_por_instante` identifica a órfã pela hora antes de conferir distância. Passar o instante exato e compará-lo aos marcos absolutos. (`app/ponto/management/commands/tripwire_celulas.py:165`, `app/ponto/management/commands/tripwire_celulas.py:177`, `app/ponto/management/commands/tripwire_celulas.py:195`, `app/ponto/management/commands/tripwire_celulas.py:264`) |
| Cartão pela célula | `orfas_e_isencao_da_ata` devolve sets de strings. Migrar para a porta ou retirar se confirmado sem chamador produtivo; foi localizada chamada no teste. Completude de chamadas dinâmicas é [não verificado]. (`app/relatorios/cartao_pela_celula.py:277`, `app/relatorios/cartao_pela_celula.py:288`, `app/relatorios/tests/test_cartao_pela_celula.py:166`) |
| Snapshot O118 | Conta células pela lista não vazia; também deve abandonar a leitura direta. Essa ferramenta escreve snapshot quando executada. (`bin/snapshot_o118_col221.py:32`, `bin/snapshot_o118_col221.py:30`) |

Os demais leitores da **projeção `dia['orfas']`** também precisam acompanhar o tipo novo:

| Leitor | Ajuste necessário |
|---|---|
| Classificador de órfãs | Chaves do resultado por instante; rajadas por diferença temporal, substituindo o parser HH:MM. (`app/escala/orfa_classe.py:31`, `app/escala/orfa_classe.py:44`, `app/escala/orfa_classe.py:58`, `app/escala/orfa_classe.py:74`) |
| Propositor | Substituir validação e cluster por HH:MM; preservar o estado já existente de “não avaliável”. A DP atual também recebe strings de horas e precisa de adaptação temporal. (`app/ponto/propositor.py:39`, `app/ponto/propositor.py:44`, `app/ponto/propositor.py:52`, `app/ponto/propositor.py:55`, `app/ponto/propositor.py:112`, `app/ponto/propositor.py:123`) |
| Comando do propositor | Não filtrar os instantes por `_h_ok` de tamanho cinco nem reconvertê-los a HH:MM para chamar a DP. (`app/ponto/management/commands/propor_reorganizacao.py:93`, `app/ponto/management/commands/propor_reorganizacao.py:104`, `app/ponto/management/commands/propor_reorganizacao.py:107`) |
| Ausência parcial | `horas_reais_do_dia` assume explicitamente a data da linha para a órfã. Calcular deslocamento a partir do instante real e da origem temporal declarada; legado permanece desconhecido. (`app/ponto/turnos.py:2649`, `app/ponto/turnos.py:2667`, `app/ponto/turnos.py:2643`) |
| Suprajuiz | Usa existência, quantidade e descrição; formatar somente a descrição. `None` não deve ativar reconstrução pelas batidas. (`app/ponto/supra_juiz.py:379`, `app/ponto/supra_juiz.py:514`, `app/ponto/supra_juiz.py:520`, `app/ponto/supra_juiz.py:521`) |
| Causas da folha | Usa presença de órfãs; distinguir presença provada por contagem de identidade desconhecida. (`app/folha/causas.py:43`, `app/folha/causas.py:100`) |
| Fatos do status | Conta órfãs para `tem_batida`; conservar contagem conhecida, sem transformar desconhecimento em zero. (`app/ponto/status_dia.py:188`, `app/ponto/status_dia.py:190`) |
| Furos diários | Conta para o agregado; transportar a contagem lavrada quando faltar identidade. (`app/escala/services/furos_diarios.py:22`, `app/escala/services/furos_diarios.py:54`) |
| Bordas do realizado | Testa existência; preservar a distinção entre presença conhecida e não medido. (`app/ponto/services/bordas_realizado.py:49`) |
| Simulação | Órfã impede “dia perfeito”; identidade desconhecida não pode provar perfeição. (`app/ponto/services/simular_dia.py:12`) |
| Detector de par relâmpago | Usa quantidade no score; não precisa renderizar hora para contar. (`app/ponto/management/commands/detectar_par_relampago.py:140`) |
| Cartório | Conta as órfãs da grade para `n_bat`, além do consumidor geométrico já listado. (`app/ponto/services/cartorio.py:1426`, `app/ponto/services/cartorio.py:628`) |

As limpezas da grade devem operar sobre a única lista: clip, folga, ausência e justificativa. O transporte legacy também precisa preservar o tipo e a ignorância. (`app/escala/utils.py:962`, `app/escala/utils.py:966`, `app/escala/utils.py:1153`, `app/escala/utils.py:1218`, `app/escala/utils.py:1253`, `app/escala/utils.py:1258`, `app/escala/utils.py:1043`)

Os homônimos de `orfas` referentes a perguntas e autoridades não pertencem a este fluxo de batidas. (`app/chamados/reconciliador.py:1319`, `app/chamados/reconciliador.py:1377`, `app/core/management/commands/gerar_diagrama.py:783`, `app/core/management/commands/gerar_diagrama.py:854`)

4. **O que aproveitar e o que rejeitar de c13–c19**

Servem como **especificação de fronteiras**: preservar o instante antes da 

[truncado em 20000 de 27277 caracteres]

## 12/11 16:47 · sessao e0b666b05179 · - · - · origem=Ronald (tmux)

Matriz das classes, restrita ao código lido:

| Classe | Resultado e prova |
|---|---|
| Segundo juiz novo | **Ausente nos helpers novos**: ambos importam e chamam `pode_executar_csf`, cujo critério é `respondida_em` nas perguntas da origem ou destinadas ao chamado (`app/ponto/services/cartorio.py:958`, `:965`; `app/chamados/reconciliador.py:645`, `:651`; `app/ponto/supra_juiz.py:539`). C7 continua sendo `DisputaSupervisao.pode_fechar` (`app/core/juizes.py:279`; `app/chamados/reconciliador.py:628`). |
| Segundo escritor fora da porta | **Ausente no delta lido**: helper usa `transicionar_com_pauta(... ato=x.reconciliar)`; E2 conserva `carimbar` e `fechar` (`app/ponto/services/cartorio.py:968`; `app/chamados/reconciliador.py:604`, `:629`). A escrita efetiva permanece nas portas (`app/chamados/models.py:228`, `:1933`, `:3153`). |
| Relógio fora da borda | **Nenhuma leitura nova nos helpers/contador** (`app/ponto/services/cartorio.py:956`; `app/chamados/reconciliador.py:644`). Não certificar a cadeia inteira: `carimbar(quando=None)` lê `now()`, `fechar` também; `julgar_celula` lê `localdate()` e `now()` (`app/chamados/models.py:3125`, `:1933`; `app/ponto/services/cartorio.py:2067`, `:2088`). São trechos preexistentes, não introduzidos pelo anexo. A lei limita a borda a view/comando/cron/consumidor OUTBOX (`app/docs/CORTES.md:370`). |
| Derivação paralela da fonte | **Ausente nos helpers novos**, que consultam o juiz; **presente como caminho paralelo preexistente no FIO-DUP**: sua isenção lê texto apenas nas resolvedoras, enquanto CSF lê `respondida_em` também na origem (`app/ponto/services/cartorio.py:1687`, `:1695`, `:1698`, `:1715`; `app/ponto/supra_juiz.py:543`, `:547`). |
| Selo/RED sem mordida | Há pares controle/protegido de estado final para quatro sítios e cascata (`app/ponto/tests/test_o168_guarda_csf_no_evento.py:114`, `:122`, `:131`, `:136`, `:143`, `:148`, `:157`, `:162`, `:227`, `:236`). **O caso do evento seguinte não prova guarda**: começa depois do carimbo, e sua própria documentação o identifica como estabilidade (`:259`). Execução de RED/mutantes [nao verificado]. |
| Ordem de trava | **Nenhuma inversão nova identificada**: helper só lê; E2 mantém atomic + trava do colaborador antes de carimbar; fechar usa `trava_na_ordem` (`app/chamados/reconciliador.py:601`, `:602`, `:604`; `app/chamados/models.py:1911`). Ordem implementada: Colaborador `:2667`, Disputa `:2676`, Chamado `:2689`, Pergunta `:2693`. |
| Pouso antes do pré-requisito | **[nao verificado]**. A regra exige suíte inteira verde antes do opus final e proíbe aprovação isolada pelo Codex (`CLAUDE.md:377`, `:380`). | [nao verificado]
| Campo com dois significados | **Presente: “pai vivo preservado” inclui terminal absoluto.** `_pais_com_conversa` promete pai VIVO, mas usa `status_local not in ENCERRADOS` (`app/chamados/reconciliador.py:638`, `:651`). `cancelado`/`superado` pertencem a TERMINAIS, não a VIVOS nem a ENCERRADOS (`app/chamados/catalogo/motor.py:24`, `:30`, `:31`). Assim, o helper admite esses pais com conversa, impede o fechamento da disputa e chama o contador (`app/chamados/reconciliador.py:621`, `:626`); o cartório contabiliza por esse callback (`app/ponto/services/cartorio.py:1050`, `:981`). Contudo, RECONCILIAR já não poderia fechar esses pais (`app/chamados/catalogo/motor.py:111`, `:161`; `app/chamados/models.py:674`). Ocorrência real desses estados [nao verificado]. |

Leis dos sítios:

- **L-061:** não identifiquei rejulgamento novo da célula nos helpers; E2 mantém leitura da célula julgada e passa pelo juiz/carimbo existente (`app/docs/LEIS.md:95`; `app/chamados/reconciliador.py:604`, `:661`).
- **C7 e juiz único:** conservados nos caminhos novos (`app/docs/JUIZES.md:62`; `app/core/juizes.py:279`; `app/chamados/reconciliador.py:628`).
- **RELOGIO-INJETADO:** nenhum relógio acrescentado; cadeia existente conserva leituras mencionadas acima, portanto não cabe certificar conformidade global (`app/docs/CORTES.md:370`; `app/chamados/models.py:3125`; `app/ponto/services/cartorio.py:2067`).
- **L-073, L-124, L-115 e L-113:** o delta não acrescenta cálculo de teto, horizonte, pareamento ou folha; conformidade geral dessas leis [nao verificado] (`app/docs/LEIS.md:100`, `:145`, `:151`, `:153`).
- **E2-EMISSORES-COMPLEMENTO:** trata dos emissores e precedência no nascimento, não da guarda nova de fechamento; nenhum emissor é alterado no diff fornecido (`app/docs/CORTES.md:360`).
- **DOIS-AGENTES:** os cortes citados referem-se às raias O261/O256 após O206, não estabelecem requisito técnico novo para O168 (`app/docs/CORTES.md:70`, `:259`).

O achado novo comprovado é a divergência **VIVO versus não ENCERRADO**, que afeta tanto a retenção da disputa quanto o significado do contador; quantidade afetada em produção [nao verificado]. (`app/chamados/reconciliador.py:621`, `:638`, `:651`; `app/ponto/services/cartorio.py:974`, `:1050`.)

## 12/11 16:46 · sessao 361cf60ab2b9 · - · - · origem=Ronald (tmux)

2. **Constância SQL da guarda: desenho em lote nos dois helpers; prova 1×3 só em `_pais_com_conversa`.**

- `_pais_com_conversa` carrega em lote `chamado`, disputas/perguntas de origem e perguntas resolvedoras, depois consulta o mesmo juiz: `app/chamados/reconciliador.py:649-651`.
- `_fecham_cobrancas_sem_furo` também faz prefetch da lista inteira para origem/perguntas antes do laço: `app/ponto/services/cartorio.py:959-965`. As resolvedoras já vêm prefetched pelo carregador real: `app/ponto/services/cartorio.py:127-128`; o corpo usa esse carregador em `:1369`, filtra `vivos` em `:1434` e chama o helper nos quatro sítios `:1054`, `:1589`, `:1602`, `:1623`.
- Portanto, as dependências SQL da guarda estão em lote em todos esses sítios, por invocação do helper; o juiz apenas percorre coleções e lê `respondida_em`: `app/ponto/supra_juiz.py:539-550`. Isso não torna CPU, volume de linhas ou evento inteiro constantes: há laços em `:543-548`, e o E2 contém `refresh_from_db` por disputa em `app/chamados/reconciliador.py:618-620`.
- O teste captura **somente** `_pais_com_conversa(lista)` e compara a contagem com 1 e 3 disputas: `app/ponto/tests/test_o168_guarda_csf_no_evento.py:275-286`. Um mutante que acrescente uma query por disputa **dentro desse helper** produz acréscimos +1/+3 e reprova pela igualdade de `:286` — conclusão estática; execução do mutante [nao verificado]. Mutação fora do helper não é coberta por essa captura: `:281-282`.
- Não há comparação equivalente de custo da guarda para os quatro sítios diretos nesse teste: ele importa e mede apenas `_pais_com_conversa` em `:273/:282`. Os tetos gerais apenas exigem `medido <= teto`, sem comparar o delta da guarda: `app/ponto/tests/test_selo_chokepoint_escrita.py:287-298`, `:435-443`. A medição efetiva nesta cópia [nao verificado].

3. **Contador: chamado distinto por ato, acumulado numericamente no lote.**

| Elemento | Prova |
|---|---|
| Incrementador | `_contar_csf_preservado` cria/consulta o conjunto de pk, adiciona o novo pk e incrementa o número: `app/ponto/services/cartorio.py:978-981`. |
| Chamador direto | Guarda negativa chama incrementador antes do `continue`: `app/ponto/services/cartorio.py:965-967`. |
| Chamador E2 | Callback fornecido pelo cartório: `app/ponto/services/cartorio.py:1050`; chamado quando a disputa é preservada: `app/chamados/reconciliador.py:621-627`. |
| Consolidação da releitura | Compartilha o mesmo conjunto e soma somente os incrementos locais: `app/ponto/services/cartorio.py:1131-1135`. |
| Vida do conjunto | Removido no `finally` do ato: `app/ponto/services/cartorio.py:1150`; declarado em `CHAVES_DO_ATO`: `:2001-2002`. |
| Unidade efetiva | O conjunto deduplica por `pk` no ato; como sai no `finally`, não deduplica globalmente entre atos diferentes: `app/ponto/services/cartorio.py:978-981`, `:1150`. |
| Leitor nominal de produção | A consolidação da releitura em `app/ponto/services/cartorio.py:1133-1135`. |
| Exposição genérica | `julgar_celula` devolve `out`: `app/ponto/services/cartorio.py:2105`. |
| Leitores de teste | `app/ponto/tests/test_o168_guarda_csf_no_evento.py:126`, `:140`, `:152`, `:166`, `:234`, `:244`, `:257`, `:298`. |
| Saída operacional | O resumo de `processar_cartorio` não imprime esse campo: `app/ponto/management/commands/processar_cartorio.py:66-70`; o de `lavrar_status_competencia` também não: `app/ponto/management/commands/lavrar_status_competencia.py:58-65`. |
| Inicialização | `contadores()` não declara `csf_com_conversa_preservado`; o campo surge no primeiro incremento: `app/ponto/services/cartorio.py:2005-2021`, `:981`. |

O selo unitário verifica `(7,7,8,7) → 2`: `app/ponto/tests/test_o168_guarda_csf_no_evento.py:293-298`; a cascata exige exatamente 1 e a repetição do ato novamente 1: `:244`, `:257`. O contador registra a retenção pela guarda no ponto de avaliação; certificação de que o chamado permanece vivo após todos os demais caminhos [nao verificado].

## 12/11 16:46 · sessao e2e181b5434c · - · - · origem=Ronald (tmux)

**Cobertura estática da guarda: parcial.** O juiz é `app/ponto/supra_juiz.py:539-550`; barra qualquer `respondida_em` na disputa de origem ou nas resolvedoras, inclusive resposta já validada/materializada.

| Caminho | Guarda CSF? | Prova |
|---|---|---|
| Cartório: adiados | Sim | `app/ponto/services/cartorio.py:1050-1054` → helper `:958-968`. |
| Cartório: RECONCILIA-MARCO | Sim | `app/ponto/services/cartorio.py:1583-1589`. |
| Cartório: MARCO-INEXISTENTE | Sim | `app/ponto/services/cartorio.py:1600-1602`. |
| Cartório: B5.3b | Sim | `app/ponto/services/cartorio.py:1615-1623`. |
| E2 → disputa.fechar → S88 | Sim, antes de fechar a disputa | `app/chamados/reconciliador.py:617-629`; lote chama juiz em `:645-651`. |
| FIO-DUP / FIO-DUP-C8 | Não | Fechos diretos pela porta em `app/ponto/services/cartorio.py:1701` e `:1716`; isenção própria lê apenas texto das resolvedoras, `:1687-1699`. |
| `reconciliar_chamados_colab` → premissa morta → `_retratar` | Não | Encadeamento em `app/chamados/reconciliador.py:1475`, `:1492-1498`, `:519-524`. |
| `reconciliar_chamados_colab` → órfãs → lavrar sem turno → disputa.fechar → S88 | Não | `app/chamados/reconciliador.py:1467`, `:1558-1560`, `:1653`, `:412-414`. |
| Consumidor → E2 | Sim | `app/chamados/reconciliador.py:1399`; proteção central em `:617-651`. |
| Consumidor → absolvidos pela célula → `_retratar` | Não | `app/chamados/reconciliador.py:1402`, `:1034-1043`, `:519-524`. |
| Consumidor → premissa morta → `_retratar` | Não | `app/chamados/reconciliador.py:1406`, `:1492-1498`. |
| Consumidor → órfãs pela lâmpada → disputa.fechar → S88 | Não | `app/chamados/reconciliador.py:1409`, `:1192-1206`. |
| Consumidor → lastro → reconciliar | Não | `app/chamados/reconciliador.py:1416-1417`; `app/ponto/services/lastro.py:388-403`. |

As chamadas de produção do E2 lidas são cartório, consumidor e `reconciliar_chamados_colab`: `app/ponto/services/cartorio.py:1049-1050`; `app/chamados/reconciliador.py:1399`, `:1466`. O receiver da batida chama este último em `app/ponto/signals.py:206-215`; o filho morto também, após commit, em `app/chamados/signals.py:352-371`.

**S88 não tem guarda CSF própria:** recebe qualquer save de disputa fechada, busca pai vivo e pede reconciliação em `app/chamados/signals.py:19-43`. A disputa escreve seu fechamento em `app/chamados/models.py:1933-1937`, disparando esse receiver. Além do E2, os dois sítios automáticos do reconciliador acima chegam à mesma porta em `app/chamados/reconciliador.py:413`, `:1205`.

**As outras guardas não são equivalentes.**

- `_retratar` retém somente respostas sem consumo ou justificativa alheia pendente: `app/chamados/reconciliador.py:438-455`, `:519-524`. O discriminador de respostas exige `materializada_em IS NULL`, `validada_em IS NULL`, `respondida_em IS NOT NULL`: `app/chamados/juizes.py:2018-2020`.
- C7 exige ausência de resposta sem veredito e de fio vivo: `app/chamados/models.py:1843-1854`. A conversa já consumida não é, por definição desse código, a mesma pergunta que C7 responde.
- `reconciliar` passa pelo motor, premissa e pergunta viva, sem chamar CSF: `app/chamados/models.py:642-655`, `:672-694`. A premissa regularizada autoriza antes do dedup em `:739-740`; ausência de vivas autoriza em `:419-421`.
- `transicionar_com_pauta` executa o ato e escreve pautas em atomic; não acrescenta guarda CSF: `app/chamados/services/fecho_runtime.py:156-169`.

Isso prova **caminhos estruturais de fechamento sem consulta ao juiz CSF**; não prova ocorrência concreta em produção nem que uma fixture específica atravessará todas as outras condições: **[nao verificado]**. Nenhum teste foi executado e nenhum banco/log foi consultado: **[nao verificado]**. [nao verificado]

## 12/11 16:45 · sessao bab0c9032b66 · - · - · origem=Ronald (tmux)

Mapa de escrita/leitura dos campos do C6, verificado apenas no código:

| Campos | Quem escreve | Quem lê na decisão |
|---|---|---|
| `validada_em` | `_carimbar_validacao`: `app/chamados/services/validacao.py:134`, salva em `:137`; devolução limpa em `:199`; `PerguntaDisputa.carimbar(resposta='validar')` também atribui em `app/chamados/models.py:3136` | Projeção temporal: `app/chamados/juizes.py:1375`; morte imediata: `:1391` |
| `resposta_colab`, `respondida_em` | Resposta humana: `app/chamados/services/disputa_emissao.py:1467`; resposta automática: `:1652`; recuperação de carimbo pela trilha: `:1674`; `carimbar` também preenche `respondida_em`: `app/chamados/models.py:3133` | Projeção: `app/chamados/juizes.py:1375`; ramo respondida: `:1407`; comparação com luz: `:1346` |
| `materializada_em`, `via_resolucao` | Atribuição conjunta `_escrever_carimbo`: `app/chamados/models.py:3098`; portas persistem em `:3153` e `:3191`; limpeza em lote: `:3265`; mudança apenas da via por fato no espelho: `:3276` | Projeção: `app/chamados/juizes.py:1376`; precedência de via: `:1402`, `:1428`; marco materializado: `:1434`; sem carimbo: `:1466`; testemunha externa: `:1473` |
| `motivo`, `tipo_resposta`, `celula` | Emissor passa motivo/tipo: `app/chamados/services/disputa_emissao.py:444`; `_nascer` monta e salva a pergunta, incluindo célula e demais campos: `app/chamados/models.py:3015` | Motivo e célula para identificar marco: `app/chamados/juizes.py:941`; tipo para comparar resposta/luz: `:1332`; testemunha por tipo: `:706` |
| `celula.veredito`, `celula.veredito_via` | Porta `lavrar_veredito`: `app/ponto/portas/celula.py:974`, atribui em `:1035` e persiste em `:1040` | Decisão explícita do admin: `app/chamados/juizes.py:1405`; acusação do dia: `:1438`; concordância do dia: `:1365` |
| `celula.dna.marcos` | Nascimento: `app/ponto/portas/celula.py:260`; regeneração: `:272`; edição: `:604`; reversão: `:786`; geração de período: `:857`, `:865`, `:875` | Identificação das lâmpadas: `app/chamados/juizes.py:948`; comparação resposta/luz: `:1338` |
| `celula.ata.lampadas` (`hora`, `tipo`, `acesa`, `luz`, `pendente`) | Cartório prepara e entrega a ata: `app/ponto/services/cartorio.py:1851`; `_ata_ate_o_teto` marca `pendente=True`: `:915`; porta persiste ata: `app/ponto/portas/celula.py:1033` | Marco aceso: `app/chamados/juizes.py:965`; luz para comparar resposta: `:1344`; `marcos_pendentes`: `app/ponto/services/lastro.py:148`, chamado pelo C6 em `app/chamados/juizes.py:1452` |

Não afirmar escritor único literal de resposta/validação: há caminhos explícitos adicionais nas referências acima. Para carimbo, `_escrever_carimbo` concentra a atribuição conjunta, mas existem limpeza em lote e mudanças isoladas de via explicitadas acima. `app/chamados/models.py:3098`, `app/chamados/models.py:3265`, `app/chamados/models.py:3276`.

O último degrau delega a `_pergunta_materializada_de_fato`, que lê também testemunhas externas de batida/turno; sua investigação completa excede os campos diretos mapeados aqui. `app/chamados/juizes.py:662`, `app/chamados/juizes.py:708`, `app/chamados/juizes.py:1473`.

## 12/11 16:45 · sessao b1de5a0cb52e · - · - · origem=Ronald (tmux)

`medir()` usa a janela de `janela_fechamento(mes, ano, Empresa)` e seleciona perguntas por `data_evento` cru entre os extremos, sem filtro `conversa=False` — `app/chamados/services/competencia_trancada.py:87`, `:107-109`. A tranca usa `_dia_de_turno_da_pergunta`, que devolve `None` para conversa e `data_evento` para pergunta de fato — `app/chamados/juizes.py:334-338`, `:991-992`. Portanto há uma diferença demonstrável de universo para conversas; não há linha de produção fornecida que prove ocorrência dela neste caso [não verificado].

Para pergunta de fato, ambos usam o corte cadastrado da empresa: `janela_fechamento` chama `periodo_apuracao(..., corte_da_empresa(...))`, e `janela_atual` chama `periodo_apuracao_atual(..., corte_da_empresa(...))` — `app/ponto/janelas.py:54-63`. O juiz extrai mês/ano do fim da janela que contém o dia e consulta a tranca — `app/chamados/juizes.py:1062-1070`.

O toque humano posterior é lei documentada, não hipótese: OPÇÃO (A), menos `(colaborador,dia)` tocado pelo administrador depois da tranca, em `app/docs/CORTES.md:83`, repetida em `:272`. O código efetua essa exceção com `out[i] = not tocou` — `app/chamados/juizes.py:1071-1079`. Isso não reabre o PF: a reabertura do período escreve `reaberto_em` em outra porta — `app/ponto/services/fechamento.py:1432-1441`.

**Limitação material do contador da cópia:** `vivas_apesar_da_tranca` recebe perguntas vivas somente no ramo em que chamado destino ou pai está no conjunto `resgatados` — `app/chamados/services/competencia_trancada.py:119-125`. Fora desse ramo, o código apenas acrescenta as perguntas mortas a `perguntas_lidas_trancadas`; não acrescenta a viva a `vivas_apesar_da_tranca` — `:130-134`. Assim, o contador de `medir()` pode dar zero sem esgotar todas as vivas selecionadas, por inspeção do fluxo — `:119-134`. **Não se pode atribuir essa limitação à sonda raspada:** sem fonte de `sonda_vivas08.py`, não sabemos se ela chama `medir()` ou faz censo próprio [não verificado].

Mapa de campos:

| Campo | Escrita | Leitura pertinente |
|---|---|---|
| `PerguntaDisputa.data_evento` | Ponto de atribuição `fio.gravar_dia_do_fato`, `app/chamados/services/fio.py:791-802`; nascimento chama-o e salva em `app/chamados/models.py:3014-3018`. Portas: pergunta de fato recebe dia do turno em `:2777-2791`; conversa recebe `localdate()` em `:2795-2803`. | `medir`: `app/chamados/services/competencia_trancada.py:108`; tranca pelo discriminador: `app/chamados/juizes.py:338`, `:991-992`. |
| `PerguntaDisputa.conversa` | `_nascer` recebe a porta e insere em `app/chamados/models.py:3015`; `abrir` passa `False` em `:2790`, `abrir_conversa` passa `True` em `:2802`. | `_dia_de_turno_da_pergunta`: `app/chamados/juizes.py:338`. |
| `Empresa.dia_inicio_competencia` | Form lê/valida em `app/colaboradores/views.py:496-499`, repassa para atualizar em `:520-523`, criar em `:531-538`; portas persistem em `app/colaboradores/services/estrutura.py:12-20`. | `corte_da_empresa`: `app/ponto/janelas.py:42-51`; ambas as janelas: `:57`, `:63`. |
| PF `empresa_id`, `mes`, `ano` | `trancar_periodo.get_or_create`: `app/ponto/services/fechamento.py:1408-1410`. | `periodo_trancado`: `:1470`; frota: `app/chamados/services/competencia_trancada.py:312-313`. |
| PF `fechado_em` | Inserção automática: `app/ponto/models.py:926`; retranca: `app/ponto/services/fechamento.py:1415`, salva `:1419`. | Exceção resgatado: `app/chamados/services/competencia_trancada.py:105`; juiz do toque: `app/chamados/juizes.py:1070`. |
| PF `reaberto_em` | Retranca limpa: `app/ponto/services/fechamento.py:1417`; reabertura grava: `:1439`, salva `:1441`. | Tranca viva: `:1470`; frota: `app/chamados/services/competencia_trancada.py:312`. `ativo` não é autoridade da tranca — `app/ponto/services/fechamento.py:1464-1470`. |
| Chamado `resgatado_em` | `arquivo.levantar` guarda o primeiro carimbo e salva: `app/chamados/services/arquivo.py:197-210`. A documentação da porta diz que pode representar mão humana **ou do colaborador** — `:180-184`; portanto não prova sozinho toque de admin. | `medir` compara com PF `fechado_em`: `app/chamados/services/competencia_trancada.py:105-106`. |

O dado enviado contém listas vazias em todos os 15 PF e `total_erros=0`, mas não traz nenhum PK ou campo individual das quatro perguntas históricas citadas no aval; atribuir degrau concreto, conversa ou toque humano a qualquer delas seria [não verificado].

## 12/11 16:45 · sessao 08d6f68761e0 · - · - · origem=Ronald (tmux)

A exceção de toque humano depois da tranca é **lei existente**, documentada em `app/docs/CORTES.md:83` e registrada como predicado C20 em `app/core/juizes.py:156`. Não reabre o `PeriodoFechado`: deixa aquele `(colaborador, dia)` fora do silêncio da tranca, por `out[i] = not tocou` em `app/chamados/juizes.py:1079`.

O caminho é:

- `_pergunta_viva_calc` consulta a tranca antes da resposta e retorna `False` se ela calar a pergunta: `app/chamados/juizes.py:1395`.
- `dia_da_pergunta_trancado` usa `_dia_de_turno_da_pergunta`, explicitamente distinguindo a conversa cujo `data_evento` é dia civil: `app/chamados/juizes.py:989`.
- `dias_trancados` exige dia, colaborador e empresa; resolve a janela pela empresa, consulta o PF da competência e passa `pf.fechado_em` ao toque: `app/chamados/juizes.py:1024`, `app/chamados/juizes.py:1062`, `app/chamados/juizes.py:1070`.
- O toque reconhecido vem de `LogAuditoria`: usuário não nulo e diferente do sistema; timestamp **estritamente posterior** à tranca: `app/chamados/juizes.py:1252`. Não lê `CelulaDia.editada_em` ou `veredito_em` para esse predicado: os três conjuntos de campos consultados estão em `app/chamados/juizes.py:1261`, `app/chamados/juizes.py:1287`, `app/chamados/juizes.py:1310`.

| Campos usados | Escritor | Leitor |
|---|---|---|
| PF `empresa_id`, `mes`, `ano`, `fechado_por`; nascimento | `trancar_periodo`, `app/ponto/services/fechamento.py:1408` | Busca por empresa/competência e `reaberto_em IS NULL`, `app/ponto/services/fechamento.py:1470` |
| PF `fechado_em` | `auto_now_add`, `app/ponto/models.py:926`; nova tranca após reabertura, `app/ponto/services/fechamento.py:1415` | Limite do toque, `app/chamados/juizes.py:1070` |
| PF `reaberto_em` | Limpa ao trancar novamente, `app/ponto/services/fechamento.py:1417`; grava ao reabrir, `app/ponto/services/fechamento.py:1439` | Decide tranca viva, `app/ponto/services/fechamento.py:1470` |
| Log `usuario`, `acao`, `modelo`, `objeto_id`, `descricao` | Escritor comum `core.observ.evento`, `app/core/observ.py:134`; adaptador `registrar_log`, `app/core/auditoria.py:22` | Identificação de humano, ação e alvo, `app/chamados/juizes.py:1252`, `app/chamados/juizes.py:1261`, `app/chamados/juizes.py:1287`, `app/chamados/juizes.py:1310` |
| Log `criado_em` | `auto_now_add`, `app/core/models.py:307` | Corte estrito contra PF, `app/chamados/juizes.py:1268`, `app/chamados/juizes.py:1292`, `app/chamados/juizes.py:1315` |
| Ação `cobrar_dia_manual`, alvo colaborador e `dia=AAAA-MM-DD` na descrição | `app/chamados/services/cobrar_dia.py:39` | `app/chamados/juizes.py:1261` |
| Ação `veredito_celula`, alvo célula | `app/ponto/services/veredito_celula.py:356` | Resolve célula por `(colaborador_id,data)`, `app/chamados/juizes.py:1276`; consulta ação, `app/chamados/juizes.py:1287` |
| Ação `desarquivar_chamado`, alvo chamado | `app/chamados/views_arquivar.py:77` → `app/chamados/services/arquivo.py:211` → trilha `app/chamados/services/arquivo.py:137` | Liga chamado ao colaborador e célula, `app/chamados/juizes.py:1300`; consulta ação, `app/chamados/juizes.py:1310` |

Os testes codificam: toque humano posterior mantém viva (`app/chamados/tests/test_c7_red_minimos.py:391`); sistema/usuário nulo não contam (`app/chamados/tests/test_c7_red_minimos.py:399`); toque anterior não conta (`app/chamados/tests/test_c7_red_minimos.py:408`). Esses testes foram lidos; execução nesta auditoria [não verificado].

O dado fornecido contém zero VIVAs e nenhum campo individual de pergunta; atribuir algum degrau ou toque às quatro históricas permanece [não verificado].

## 12/11 16:44 · sessao fcbb067fed4f · - · - · origem=Ronald (tmux)

Censo primário de (i): **60 sítios por arquivo/linha**, fundindo leitura/escrita na mesma linha e incluindo derivados da grade e cópia para contexto de chamado. Esse total deriva de 70 ocorrências AST literais, exclui cinco homônimos, funde seis duplicidades e acrescenta `orfas=` em `app/escala/services/leitor_celula.py:420`.

Homônimos excluídos:

- `app/chamados/reconciliador.py:1340`, `:1377`: contador de eventos.
- `app/core/management/commands/gerar_diagrama.py:783`, `:854`: perguntas sem juiz.
- `app/ponto/portas/celula.py:1132`: campo numérico de `FuroDiario`.

(ii), produção Python: `_orfas_da_ata` definido em `app/escala/services/leitor_celula.py:180`, chamado em `:420`; transporte de `orfas_grade` em `app/ponto/services/espelho.py:689`, `:692`, `:844`. Templates, por grep e sem AST: `app/templates/ponto/espelho.html:151`, `:240`. Função pública `orfas_da_ata` e campo `orfas_instantes` não foram encontrados no levantamento textual de `app/` [não verificado quanto a construções dinâmicas].

Complemento de aliases/transportes, fora dos 60 primários:

| Arquivo e função | Linhas e operação |
|---|---|
| `app/escala/utils.py::_match_marcos` | `621`, `662`: construção HH:MM; `624`, `665`: retorno das listas |
| `app/escala/utils.py::marco_ocupado_pela_batida` | `427`: recebe lista pelo desempacotamento; `_orfas` descartada |
| `app/escala/utils.py::montar_grade_prevista` | `776`: desempacota lista |
| `app/escala/utils.py::montar_grade_prevista_periodo_por_turno` | `1438`, `1567`: mapa e listas vazias; `1650`: desempacota; `1651`: copia para set; `1675`: membership HH:MM; `1701`: desempacota; `1722`, `1733`: append HH:MM; `1758`: mapa recebe lista; `1970`, `1996`: aliases; `1997`: teste None; `2006`: membership; `2032`: filtra órfãs do turno e compara lista; `2039`: remove pelo HH:MM |
| `app/colaboradores/services/calendario.py::contexto_calendario` | `297`: mapa vazio; `622`: extrai coleção em `_ho`; `626`: membership HH:MM |
| `app/escala/orfa_classe.py::classificar_orfas` | `59`: teste vazio; `66`, `71`: mapas HH:MM→classe; `74`: ordenação e chamada do agrupador |
| `app/ponto/propositor.py::dia_degenerado` | `42`, `59`, `61`: presença; `53`: valida hora; `55`: converte e ordena minutos |
| `app/ponto/services/cartorio.py::lampadas_do_dia` | `599`: parâmetro `orfas`; `628`: converte órfãs para minutos em `_reais` |
| `app/ponto/services/cluster_espurio.py::_leitura_da_celula` | `33`: quantidade; `34`: minutos ordenados; `36`: retorna coleção |
| `app/ponto/services/cluster_espurio.py::dias_com_cluster` | `49`: chama leitor; `51`: transporta lista recebida |
| `app/ponto/services/cluster_espurio.py::reconciliar_colab` | `193`: desempacota; `210`: texto por join |
| `app/ponto/services/vigia_pareador_celula.py::censo` | `78`: mapa transporta lista/set; `79`: desempacotamento; `109`: extrai órfãs civis; `111`: membership; `112`: publica caso |
| `app/relatorios/cartao_pela_celula.py::orfas_e_isencao_da_ata` | `285`: mapas vazios; `293`: retorna mapa de órfãs |
| `app/relatorios/pdf_espelho.py::_coletar_dados_espelho` | `472`: set vazio; `513`: passa set global ao predicado; `544`: mapa vazio; `607`: extrai `_orf`; `609`: membership HH:MM |
| `app/ponto/supra_juiz.py::classificar_dia` | `516`, `517`: testes de presença; `520`: texto da coleção por join |

Cópias de ata/dia inteiros confirmadas, para complemento opaco:

- `app/escala/services/leitor_celula.py::grade_da_celula`: `393`, `395`, `418` copiam `base`; `418` também transporta ata completa no envelope de insumos.
- `app/escala/utils.py::_montar_grade_prevista_periodo_legacy`: `1043`, `**sd`, seguido de sobrescrita explícita de `orfas` na mesma linha.
- `app/ponto/portas/celula.py::celula_como_o_julgamento_deixa`: `943`, cópia de célula; `945`, cópia da ata.
- `app/ponto/portas/celula.py::lavrar_veredito`: `995`, cópia; `1000`, comparação integral; `1006`, `1033`, atribuição.
- `app/ponto/portas/celula.py::carimbar_precedencia`: `1083`, cópia integral; `1085`, atribuição.
- `app/ponto/services/cartorio.py::ata_do_dia`: `772`, retorno integral.
- `app/ponto/services/cartorio.py::dispensar_por_ausencia`: `803`, `813`, retorno integral.
- `app/ponto/services/cartorio.py::_dia_ate_o_teto`: `904`, cópia integral do dia.
- `app/ponto/services/cartorio.py::_ata_ate_o_teto`: `910`, retorno; `917`, cópia.
- `app/ponto/services/cartorio.py::_julgar_colab_corpo`: `1251`, `1293`, `1436`, `1437`, `1440`, `1851`, `1853`, composição, cache e passagem de atas.
- `app/ponto/services/cartorio.py::ata_deste_julgamento`: `57`, leitura de ata integral no cache; `_tl.atas` limpo em `1124`.
- `app/chamados/juizes.py::celula_deste_julgamento`: `422`, import com alias do leitor de ata integral; `424`, transporte para projeção.
- `app/ponto/supra_juiz.py::com_insumo_dia_sem_trabalho`: `62`, cópia do dia e envelope de ata integral.

Esses transportes conservam o valor HH:MM já sem data; as conversões que efetivamente retiram data/fuso estão na construção das listas, por exemplo `app/escala/utils.py:621`, `:662`, `:799`, `:1832`, `:1834`. O leitor aceita strings sem validar formato e usa `'?'` como fallback em `app/escala/services/leitor_celula.py:187`, `:190`; conteúdo real das atas [não verificado].

## 12/11 16:44 · sessao b4426e614e40 · - · - · origem=Ronald (tmux)

O censo AST dos acessos literais a `orfas`, `orfas_grade` e `_orfas_da_ata` revela que a mudança alcança também classificadores, vigias e propositor, além dos cinco caminhos nomeados pelo aval. Os sítios concretos seguem abaixo. (`app/escala/orfa_classe.py:58`; `app/ponto/propositor.py:39`; `app/ponto/services/vigia_pareador_celula.py:74`)

**Grade, dry-run e reconciliação**

| Sítio atual | Comportamento e perda de lastro | Encaixe proposto |
|---|---|---|
| `app/escala/utils.py:621`, `:662` | `_match_marcos` recebe datetimes em `locs`/`B`, mas devolve órfãs por `strftime('%H:%M')`. | Devolver diretamente `list[datetime]` aware; o cartório serializa ISO. Não acrescentar coletor/lista paralela. |
| `app/escala/utils.py:683` | Builder single monta `locs` com `timezone.localtime(b.timestamp)`. | Conservar esse objeto até a lavra. |
| `app/escala/utils.py:799`, `:803` | Sobras fora dos envelopes viram HH:MM e são ordenadas como strings. | Acrescentar instante original e ordenar cronologicamente. |
| `app/escala/utils.py:1650`, `:1651`, `:1675` | Dry-run copia `consumed`, obtém órfãs HH:MM, cria set e reconhece candidato por `strftime('%H:%M') in _orfset`. | Set dos próprios datetimes retornados por `_match_marcos`; `locs[k][0] in _orfset`. |
| `app/escala/utils.py:1722`, `:1733` | Ponta fora da régua e intervalo excedente são promovidos a órfã copiando `_cc['hora']`. | Preservar instante em `_celula_do_marco`, antes dessa promoção; hoje a função só guarda hora, delta, status e tipo. (`app/escala/utils.py:314`, `:326`) |
| `app/escala/utils.py:1832`, `:1834`, `:1838` | Conservação por turno novamente produz HH:MM e ordena string. | Usar instantes de `locs`, inclusive segundos/microssegundos. |
| `app/escala/utils.py:1887`, `:1895` | Há **ata sintética em memória**, usada para perguntar ao juiz do par, além da ata persistida. | A ata sintética também precisa de `orfas` ISO, para que `periodos_do_dia` use o mesmo leitor único. A cópia não é salva. (`app/escala/utils.py:1886`) |
| `app/escala/utils.py:1968`, `:1973` | Reconciliação escolhe a lista do dia do turno; para saída noturna, a lista de `data_turno + 1`, pelo juiz `cross_meianoite`. | A seleção da célula permanece a que o juiz já dá; não serve para fabricar instante. |
| `app/escala/utils.py:2005`, `:2006`, `:2039` | Borda é reconhecida e removida por HH:MM. | Procurar/remover **uma ocorrência do instante exato** `_inst_t`. |
| `app/escala/utils.py:2028`, `:2032`, `:2034` | Exceção de entrada também monta horas do turno, intersecta órfãs por hora e ordena por string. | Conjunto de timestamps do turno; interseção por instante; comparar instante preservado da célula com `_inst_t`. A guarda atual que exclui noturno continua existindo até alteração explícita de sua regra. (`app/escala/utils.py:2027`) |

As limpezas atuais devem limpar a única lista nova, sem sincronização com lista auxiliar: clip `app/escala/utils.py:962`, `:966`; folga `:1153`; ausência/falta/justificativa `:1218`, `:1253`, `:1258`. O transporte legacy permanece em `:1043`, e o adaptador de realizado devolve a lista em `:1110`. Hoje esse adaptador descarta órfãs quando faltam marcos, devido ao retorno antecipado; o caso sem marcos do diff é uma especificação útil. (`app/escala/utils.py:1084`)

**Todos os outros leitores diretos da ata encontrados**

| Leitor | O que faz hoje | Migração necessária |
|---|---|---|
| `app/ponto/juiz_batida.py:161` | Converte cada órfã em string e publica no resultado de `periodos_do_dia`. | Usar `orfas_da_ata(celula)` e preservar `None`; não transformar desconhecimento em `[]`. Resultado publica órfãs em `:164`. |
| `app/chamados/juizes.py:510` | Testa existência por lista ou `n_orfas`; prende pelo juiz da régua em `:516`. | Eliminar acesso direto à lista. Existência pode continuar pela contagem lavrada quando conhecida, mesmo quando a identidade é desconhecida. |
| `app/chamados/services/materializacao.py:181` | Filtra órfãs para HH:MM legível, mistura com luzes e entrega ao propositor; o plantio também vira HH:MM em `:182`. | Ler instantes pela porta; adaptar a atribuição para coordenadas temporais que conservem a data real; `None` impede absolvição por marcas incompletas. Não datar órfãs pela célula. |
| `app/ponto/services/cluster_espurio.py:29`, `:31` | Exige lista; corta HH:MM em minutos em `:34`; reconhece intervalo ≤15 min em `:35`. | Leitor único; `None` mantém “não medido”; ordenar instantes e subtrair datetimes. A API já distingue `None`, `()` e assinatura. (`:15`–`:20`) |
| `app/ponto/services/esmeril_espelho.py:125` | A5 acumula recorrência em faixas de 30 min e emite quando ≥3 ocorrências. (`:126`–`:139`) | Instantes pela porta; pertença a faixas aware da **data real do instante**, preservando as mesmas 48 faixas e o mesmo limiar; agregar somente a categoria da faixa entre dias. HH:MM é rótulo final em `:139`. Não substituir a regra de recorrência por igualdade de instantes. A5 é explicitamente “recorrente no mesmo horário”. (`:19`) |
| `app/ponto/services/vigia_pareador_celula.py:74` | Carrega listas por célula; encontra data civil por limites de instantes em `:82`–`:87`, mas reconhece a borda por HH:MM em `:110`–`:111`. | Carregar células/representações aceitas pela porta e comparar `b.timestamp in instantes`; registrar “não medido” para legado, sem fingir universo vazio. |
| `app/ponto/management/commands/tripwire_celulas.py:264` | Peneira circular HH:MM em `:165`–`:177`; confirmação ainda reconhece órfã por HH:MM em `:195`, apesar de conferir distância por instante. | Porta única; passar o instante da órfã à peneira/confirmação e comparar diretamente com marcos absolutos. Não procurar uma batida qualquer com a mesma hora. |
| `app/relatorios/cartao_pela_celula.py:288` | `orfas_e_isencao_da_ata` devolve set de strings por célula. | Porta única e sets de instantes, mantendo desconhecimento. Nenhum chamador fora dos testes foi encontrado pelo nome; completude de chamadas dinâmicas: [não verificado]. |
| `bin/snapshot_o118_col221.py:32` | Conta células com lista não vazia para imprimir “ANTES”. | Ferramenta também deve deixar de ler diretamente a lista; não foi executada. Ela escreve snapshot em `:30`. |

O contexto de chamado recebe cópia de órfãs em `app/ponto/services/cluster_espurio.py:211`, e a descrição concatena suas strings em `:210`; esse transporte precisa manter ISO no dado e formatar HH:MM somente na descrição. Não é outro escritor de `CelulaDia.ata`. (`app/ponto/services/cluster_espurio.py:196`, `:211`)

**Leitores de projeção `dia['orfas']`**

| Grupo | Sítios | Tratamento |
|---|---|---|
| Identidade na tela calendário | `app/colaboradores/services/calendario.py:380`, `:381` → comparação `:625`, `:626` → marca `b.orfa` `:627` | Projeção carrega instantes; marcar pela igualdade com `b.timestamp`; guardar desconhecimento separado do booleano conhecido. |
| Classificação de órfãs | `app/escala/orfa_classe.py:58` | Chave do resultado deve ser instante; rajada deixa de usar `_minutos` `:31`, `:44`, `:74` e passa a diferenças entre instantes. |
| Propositor | `app/ponto/propositor.py:39` | Hoje valida HH:MM em `:52`–`:54` e calcula rajada por minutos de relógio em `:55`–`:57`; migrar cálculos temporais conservando data e manter o bucket não avaliável para `None`. Esse bucket já é contrato explícito. (`:18`–`:21`, `:44`–`:50`) |
| Command do propositor | `app/ponto/management/commands/propor_reorganizacao.py:104` | Hoje filtra HH:MM e chama `atribuir_global` em `:107`; não pode descartar instantes porque `_h_ok` espera tamanho cinco. Preservar identidade no adaptador temporal; desconhecido vai ao bucket já contado em `:78`–`:79`, `:123`–`:126`. |
| Ausência parcial | `app/ponto/turnos.py:2667` | `horas_reais_do_dia` lê órfã como minuto do próprio dia; a reconstrução pela linha é explícita em `:2649`. Substituir por deslocamento calculado do instante real em relação à origem temporal declarada do turno, preservando dia seguinte; legado gera desconhecimento, que o juiz já expressa como `nao_sei`. (`:2616`–`:2643`) |
| Existência/contagem | `app/folha/causas.py:43`, `:100`; `app/ponto/status_dia.py:188`; `app/escala/services/furos_diarios.py:22`; `app/ponto/services/bordas_realizado.py:49`; `app/ponto/services/simular_dia.py:12`; `app/ponto/management/commands/detectar_par_relampago.py:140` | Não precisam converter instantes para horas. Precisam distinguir lista conhecida, ausência conhecida e identidade desconhecida; preservar contagem lavrada quando o consumidor pergunta somente quantidade. |
| Suprajuiz | `app/ponto/supra_juiz.py:379`, `:514`, `:520` | Existência/contagem e descrição; formatar só descrição. Não deixar `None` de legado ativar o recount por batidas em `:521`, nem virar falso em `bool`. |

A grade atual **não publica** `n_orfas` no resultado do leitor; publica `n_ok`, `n_casadas` e `orfas`. Para conservar perguntas de quantidade sem confundir legado com zero, o pré-desenho deve prever transportar a contagem já lavrada, separada da disponibilidade de identidades; isso não cria outra lista de órfãs. (`app/escala/services/leitor_celula.py:417`–`:424`; lavra da contagem: `app/ponto/services/cartorio.py:742`)

`chamados/reconciliador.py:1377` e `core/management/commands/gerar_diagrama.py:854` usam homônimo `orfas` para perguntas/autoridades, respectivamente; não são leitores da órfã de batida. (`app/chamados/reconciliador.py:1319`; `app/core/management/commands/gerar_diagrama.py:783`)

**Autoridades e fronteiras**

O juiz do marco já é `_match_marcos`, e o juiz da pertença ao turno já é `turnos_do_colab`; a mudança de representação não exige outro juiz. (`app/core/juizes.py:208`, `:212`; `app/docs/JUIZES.md:20`, `:27`) A cadeia absoluta já tem escritor único `encadear_marcos`, inclusive virada de meia-noite. (`app/escala/utils.py:330`–`:340`)

Servem como especificação do diff: conservação das promoções de célula a órfã, dry-run com instante, retirada de uma única ocorrência na reconciliação, órfãs sem marcos e ignorância explícita do legado; a implementação paralela não serve como base. [não verificado na cópia: classificação do anexo fornecido]

REDs a exigir: mesma HH:MM em datas diferentes; órfãs no mesmo minuto com segundos distintos; noturno com órfã antes/depois da meia-noite; mesmo instante com offsets diferentes; legado HH:MM/count-only/ata ausente; lista vazia explícita; promoção por régua; dry-run sem consumir a passagem definitiva; remoção preservando outras ocorrências; `None` atravessando todos os consumidores sem `or []`. São propostas de prova, não testes executados. [não verificado] [nao verificado]

Perguntas de lei restantes neste recorte: **0**; A5 pode preservar a recorrência já declarada por pertença a faixas temporais aware, sem casar identidades por HH:MM e sem fabricar datas. (`app/ponto/services/esmeril_espelho.py:19`, `:125`–`:139`)

## 12/11 16:43 · sessao 483dbe76f6e0 · - · - · origem=Ronald (tmux)

Censo de (iii), com identificação de funções por AST e inspeção das operações; a lista abaixo usa **uma linha física por sítio**, inclui projeção/parse que alimenta casamento e exclui mero render final. A exaustividade de análise dinâmica/reflexiva é [não verificado].

```python
{
  "app/escala/orfa_classe.py": [32,44,74],
  "app/ponto/services/cluster_espurio.py": [34,35],
  "app/ponto/services/cartorio.py": [623,628,666,667],
  "app/ponto/propositor.py": [
    26,27,31,55,57,94,98,99,107,122,123,124,
    135,136,155,156,176,177,249,250,273,276,
    289,297,315,340,342,343,345,351
  ],
  "app/ponto/turnos.py": [2611,2640,2641,2667],
  "app/ponto/management/commands/tripwire_celulas.py": [
    129,165,174,177,192,195,280,285
  ],
  "app/ponto/services/vigia_pareador_celula.py": [110,111],
  "app/chamados/services/materializacao.py": [177,178,182],
  "app/ponto/management/commands/propor_reorganizacao.py": [93,94,95,107],
  "app/chamados/detector_anomalias.py": [213,214],
  "app/ponto/motor_calculo_v2.py": [3069],
  "app/escala/servico_jornada.py": [455,456],
  "app/colaboradores/services/calendario.py": [625,626],
  "app/relatorios/pdf_espelho.py": [178,609,716,720,960,963,964],
  "app/escala/utils.py": [429,431,1675,2005,2006,2028,2032,2034,2039],
  "app/ponto/services/flip_auto.py": [155,161,162,219,221,222,232,233,238],
  "app/escala/regua_defesa.py": [415,418,419,433,436,438,455,456]
}
```

Funções e circulação:

| Arquivo | Funções correspondentes | Valor / perda da data |
|---|---|---|
| `escala/orfa_classe.py` | `_minutos`, `_runs_de_rajada`, `classificar_orfas` | HH:MM → minutos; data já ausente. `:32`, `:44`, `:74` |
| `ponto/services/cluster_espurio.py` | `_leitura_da_celula` | HH:MM → minutos → diferença ≤15; data ausente. `:34`, `:35` |
| `ponto/services/cartorio.py` | `lampadas_do_dia._min`, `lampadas_do_dia` | Órfas HH:MM → `_reais` → proximidade de marco ≤30 min; data ausente. `:623`, `:628`, `:666`, `:667` |
| `ponto/propositor.py` | `_tem_hora`, `_min`, `dia_degenerado`, `atribuir_global_turno`, `atribuir_global`, `_tem_empate`, `plantio_preenche_furo`, `primeiro_furo_posicional`, `dia_posicional_completo` | HH:MM/minutos; a DP casa marcas e classifica sobras; coletor elimina data de timestamp em `:345`; reconstrução +1440 em `:107` não recupera data original. |
| `ponto/turnos.py` | `_minutos_do_dia`, `periodo_da_parcial_no_dia`, `horas_reais_do_dia` | HH:MM → minutos; órfa recebe deslocamento da própria hora sem data, conforme `:2649` e implementação `:2667`; comparação com parcial em `:2641`. |
| `tripwire_celulas.py` | `_hora_hhmm`, `sobra_na_borda`, `confirmada_por_instante`, `medir_sobra_com_tipo_trocado` | HH:MM → `time`; casamento circular via `perto_do_marco` em `:177`; datetime → HH:MM em `:192`, `:195`, `:280`. `:195` também exige distância por instante. |
| `vigia_pareador_celula.py` | `censo` | Timestamp → HH:MM → pertença à lista de órfas civis; perde data no valor comparado. `:110`, `:111` |
| `chamados/services/materializacao.py` | `_plantio_preenche_furo_na_data` | Marcas incluem `ata['orfas']` em `:181`; plantio timestamp → HH:MM e DP em `:182`. |
| `propor_reorganizacao.py` | `Command.handle` | Validação HH:MM `:93`–`:95`; órfas entram em `marcas` `:104`, comparadas pela DP chamada em `:107`. |
| `chamados/detector_anomalias.py` | `detectar_batida_fora_escala` | Candidata timestamp → HH:MM → igualdade com célula; ausência de casamento produz `batida_fora_escala`. `:213`, `:214`, `:218` |
| `motor_calculo_v2.py` | `calcular_mes` | Entrada candidata datetime → HH:MM → igualdade com célula retornada pelo matcher. `:3062`, `:3069` |
| `escala/servico_jornada.py` | `classificar_falta` | Timestamp → HH:MM → pareamento posicional. `:455`, `:456` |
| `colaboradores/services/calendario.py` | `contexto_calendario` | Timestamp → HH:MM → pertença; marca `b.orfa=True`. `:625`, `:626`, `:627` |
| `relatorios/pdf_espelho.py` | `_entrada_espuria`, `_coletar_dados_espelho`, `_build_story` | Timestamp → HH:MM; igualdade/pertença de órfa `:178`, `:609`, `:964`; filtro textual de anomalias usa somente hora `:716`, `:720`. |
| `escala/utils.py` | `marco_ocupado_pela_batida`, `montar_grade_prevista_periodo_por_turno` | Timestamp → HH:MM; igualdade `:431`; pertença órfa `:1675`, `:2006`, `:2032`; ordenação de horas `:2034`; remoção por hora `:2039`. |
| `ponto/services/flip_auto.py` | `_mn`, `_dist_turno`, `etiquetas_pela_celula` | Timestamp → HH:MM `:232`; DP gera sobra sem slot `:233`, `:236`; distância circular em HH:MM `:238`, `:161`, `:162`. |
| `escala/regua_defesa.py` | `distancia_do_marco`, `_min_do_dia_f`, `perto_do_marco` | datetime/time → minutos do dia `:436`, `:438`; distância circular `:418`, `:419`; caminho de órfa é chamado pelo tripwire `:177`. |

Todas são LEITURA de identidade/tempo, exceto mutações de classificação/coleções: `calendario.py:627` escreve `b.orfa`, `pdf_espelho.py:609` escreve `_b.orfa`, `utils.py:2039` remove órfa por HH:MM; atribuições locais e produções de listas escrevem valores intermediários nos sítios indicados.

Produção pura de órfas, separável dos casamentos acima:

```python
{"app/escala/utils.py": [326,621,662,799,1832,1834]}
```

`_celula_do_marco:326` produz HH:MM que pode ser promovido a órfa em `:1722`/`:1733`; `_match_marcos:621` e `:662` eliminam a data diretamente; conservação faz o mesmo em `montar_grade_prevista:799` e no builder por turno `:1832`/`:1834`.

Casamento de **marco**, que determina promoção de batida a órfa, merece inventário separado: `app/escala/utils.py:1719` e `:1724`, com promoção em `:1722` e `:1733`. São datetime de marco → HH:MM para chave da régua; não comparam diretamente identidade de órfa.

Não classificar como perda da data o mapa `papel_por_minuto_da_ata`: seu normalizador preserva a data, embora retire fuso/segundos em `app/ponto/turnos.py:1604`; a mesma chave datada identifica candidatas/órfas em `:1616`, `:1644`, `:1804`.

HTML/JS por busca textual, **não AST**: transporte/render de `orfas_grade` em `app/templates/ponto/espelho.html:151` e `:240`; render de `b.orfa` em `app/templates/colaboradores/partials/_calendario_grade.html:121`. Resultado JS relevante às chaves/funções pedidas: nenhum encontrado pela busca textual [não verificado quanto a nomes computados].

## 12/11 16:43 · sessao 842846aab3c0 · - · - · origem=Ronald (tmux)

Censo da fonte e escritores, somente leitura.

- A lavra atual é `app/ponto/services/cartorio.py:730::ata_do_dia`; grava `n_orfas=len(...)` em `:742` e `'orfas': [str(h) ...]` em `:745`. O campo persistido é JSON, sem validação de formato na declaração: `app/escala/models.py:1778`.
- O conteúdo produtivo chega como **strings HH:MM**, sem data, segundos ou offset: `_match_marcos` perde essas partes em `app/escala/utils.py:621,:662`; sobras fora do envelope em `:799,:1832,:1834`; promoção de célula a órfã em `:1722,:1733`. Antes da perda, são datetimes aware derivados de `timezone.localtime(b.timestamp)` em `:683,:1323`; configuração padrão `America/Sao_Paulo` e `USE_TZ=True` em `app/config/settings/base.py:93,:95`. O fuso efetivamente ativo em uma execução de produção é [não verificado].
- Portanto trocar apenas o `str(h)` da ata por outra serialização não recupera o instante; é necessário preservar o datetime desde os pontos acima, inclusive nas promoções, e serializar `isoformat()` **no mesmo campo `orfas`**. O instante original da célula casada também se perde hoje em `_celula_do_marco`, `app/escala/utils.py:326`; a promoção precisa conservá-lo, em vez de reutilizar `hora` (`:1722,:1733`).

**Todos os escritores produtivos da ata identificados pelo AST e leitura:**

| Escritor | Efeito e fonte |
|---|---|
| `cartorio.ata_do_dia` | Constrói a ata, incluindo órfãs: `app/ponto/services/cartorio.py:738,:745`. |
| `lavrar_veredito` | Atribui ata inteira em `app/ponto/portas/celula.py:1006,:1033`; preserva apenas chaves declaradas de outra operação em `:997` — atualmente `precedencia`, `:104`. |
| `carimbar_precedencia` | Copia a ata, troca `precedencia`, atribui a ata inteira e salva: `app/ponto/portas/celula.py:1083,:1084,:1085,:1087`; transporta órfãs sem interpretá-las. |
| `escala.utils` — memória | Cria ata sintética com órfãs em `app/escala/utils.py:1887,:1895` numa cópia explicitamente não salva (`:1886`). Deve migrar junto para não apresentar outra forma de ata ao leitor único. |
| `celula_como_o_julgamento_deixa` — memória | Copia ata inteira para projeção, `app/ponto/portas/celula.py:945`; cópia sem persistência declarada em `:933,:934`. |
| Cartório — memória | `SimpleNamespace(ata=...)` com ata do julgamento em `app/ponto/services/cartorio.py:1436`; publicação transitória da mesma ata em `:1440`. |

A fachada do modelo apenas encaminha: `app/escala/models.py:1819,:1827`. O cartório chama a porta com `save=False` nos ramos “nunca bateu” e normal (`app/ponto/services/cartorio.py:1292,:1853`); `carimbar(...,lavrados=_cv)` efetiva a gravação em `:1314,:1873`, via `app/ponto/portas/celula.py:1072::salvar_e_emitir`, cujo `cel.save(update_fields=campos)` está em `:905`, junto com emissão do evento (`:904,:907`).

`dispensar_por_ausencia` e `_ata_ate_o_teto` modificam lâmpadas/`n_missing`, sem escrever órfãs (`app/ponto/services/cartorio.py:805,:812,:913,:917`). A reversão de regeneração grava DNA/trabalho/carimbos, sem ata (`app/ponto/portas/celula.py:786,:794`).

AST executado em memória sobre 3.559 arquivos Python da cópia, buscando atribuições `.ata`, subscritos `['ata']`, keywords `ata=`, `setattr(...,'ata',...)`, `update` e `bulk_update` com ata; nenhum `update(ata=...)`/`bulk_update(...,'ata')` produtivo adicional apareceu [não verificado: instrumento não salvo, saída da execução disponível no histórico]. Censo de sintaxe não prova ausência de metaprogramação arbitrária [não verificado].

Snapshots **leem** a ata e escrevem arquivo externo; não são escritores da CelulaDia: `bin/snapshot_o118_col221.py:18,:30`; `app/ponto/management/commands/celula_veredito_velho.py:108,:112,:113`. O primeiro descreve reversão em texto (`bin/snapshot_o118_col221.py:27`), sem implementá-la nesse arquivo; o segundo relavra pelo cartório (`app/ponto/management/commands/celula_veredito_velho.py:144`) e desfaz o ensaio por rollback (`:156,:157`).

**Leitor importante dentro da própria fonte:** `lampadas_do_dia` recebe órfãs em `app/ponto/services/cartorio.py:599`. O parser `_min` fatia HH:MM (`:621,:623`) e alimenta `_reais` com órfãs em `:628`. O ramo `j is None` procura proximidade de até 30 minutos (`:666`) e acende lâmpada com luz HH:MM (`:672,:673`), sem retirar a batida da lista de órfãs; o próprio comentário registra a obrigação “acesa e órfã disjuntas” (`:702,:703`). Esse consumidor precisa da migração por instante e do juiz geométrico existente; passar ISO diretamente para `_min` é incorreto (`:623`).

**Autoridades para a implementação:** `_match_marcos` já responde qual marco previsto a batida ocupa (`app/core/juizes.py:212`, `app/docs/JUIZES.md:27`); turno/data_turno é `turnos_do_colab` (`app/core/juizes.py:208`); marcos congelados são `EscalaColaborador.marcos_do_dia` (`:213`); `CelulaDia` só pode ser escrita pela porta (`app/core/portas.py:107,:112`). A fonte única já é padrão registrado (`app/docs/GLOSSARIO.md:11`); não precisa nascer outro juiz.

**Avisos enviados ao pai para o censo de leitores:** leitura direta também em `app/ponto/juiz_batida.py:161`; `cluster_espurio` fatia HH:MM em `app/ponto/services/cluster_espurio.py:29,:31,:34`; `app/ponto/turnos.py:2667` converte órfãs a minutos relativos, com pressuposto de data da linha explícito em `:2649`. Esses sítios entram na migração, embora não sejam escritores.

## 12/11 16:43 · sessao ffcae5163e6e · - · - · origem=Ronald (tmux)

HTML/JS — busca textual, **não AST**:

| Arquivo:linha | Sítio | Operação/tipo | Perda da data |
|---|---|---|---|
| `app/templates/ponto/espelho.html:151` | calendário: `{% for o in d.orfas_grade %}` | LEITURA, HH:MM str, render | Já chega sem data |
| `app/templates/ponto/espelho.html:240` | detalhe: `{% for o in dia.orfas_grade %}` | LEITURA, HH:MM str, render em `:241` | Já chega sem data |
| `app/templates/colaboradores/partials/_calendario_grade.html:121` | `{% if b.orfa %}`, render `b.timestamp\|date:"H:i"` | LEITURA de classificação bool; datetime → texto no render | Texto sem data; objeto conserva datetime |

A busca integral de `*.html`/`*.js` pelos nomes `orfas_grade`, `_orfas_da_ata`, `orfas_da_ata`, `orfas_instantes`, `\borfas\b`, `\borfa\b` encontrou somente os sítios acima e o texto explicativo de ranking, sem leitura de órfã, em `app/templates/relatorios/ranking_furos.html:42`.

Falsos positivos examinados, fora do casamento de órfã:

- Confirmação de resposta `time`/`datetime-local`: `app/templates/chamados/meu_atendimento.html:252`, `:253`, `:255`.
- Filtro pelo horário inicial cadastrado da escala: `app/static/js/hasner-filter.js:24`, `:30`.
- Endereço `tipo:id`, não hora: `app/static/js/live-cell.js:36`.
- Conversão de horário da agenda: `app/templates/colaboradores/partials/agenda_edit_dia.html:255`.
- Exibição do relógio e confirmação local de batida salva: `app/templates/ponto/bater_ponto.html:317`, `:547`, `:599`; `app/templates/ponto/bater_ponto_v2.html:164`, `:336`, `:349`.

**Spec reprovada — lista de acréscimos, separada da cópia.** Nas referências seguintes, `+N` é o início do hunk **do anexo**, não linha da cópia. A ancoragem por hunk é necessária porque o anexo não existe como arquivo local [não verificado].

| Arquivo no anexo / hunk | Novos sítios, transportes ou operações |
|---|---|
| `escala/services/leitor_celula.py`, `+177` | Parâmetro `orfas_instantes` em `_orfas_da_ata`; novo `ata.get('orfas_instantes')`; validação de lista/tamanho; `orfas_instantes.extend(...)` acoplado por `zip(horas, instantes)`. HH:MM e ISO str seguem em listas paralelas. A leitura `ata.get('orfas')` é modificada, não adicionada. |
| mesmo, `+393` | Novo literal `'orfas_instantes': []` na base de dia. |
| mesmo, `+421` | Novo coletor `_orfas_instantes=[]`; chamada `_orfas_da_ata(..., orfas_instantes=...)`; novo campo de saída `orfas_instantes=...`. |
| `escala/utils.py`, `+320` | `_celula_do_marco`: novo `'instante': instante.isoformat()`; mantém `'hora': instante.strftime('%H:%M')`. |
| mesmo, `+584` | `_match_marcos`: novo parâmetro `orfas_instantes`. |
| mesmo, `+622` | Nova lista `_sobras` de datetimes; novo `extend(t.isoformat()...)`; mantém geração HH:MM separada. |
| mesmo, `+667` | Novo coletor de `B[j].isoformat()` para órfãs. |
| mesmo, `+768` | `montar_grade_prevista`: novo campo vazio `orfas_instantes` em folga. |
| mesmo, `+782` | Novo coletor e argumento para `_match_marcos`. |
| mesmo, `+796` | Novo campo `orfas_instantes` no dia de trabalho. |
| mesmo, `+808` | Novo append ISO para sobra externa; nova leitura conjunta via `zip(_d['orfas'], _d['orfas_instantes'])`; ordenação pelo primeiro componente, **HH:MM**; reescrita sincronizada das duas listas. |
| mesmo, `+975` | `clipar_grade_janela`: duas novas escritas zerando `orfas_instantes`, em futuro e fora da janela. |
| mesmo, `+1021` | `_montar_grade_prevista_periodo_legacy`: novo campo vazio em segmento sem escala. |
| mesmo, `+1050` | Mesmo builder: novo campo vazio em dia sem dados. |
| mesmo, `+1100` | `montar_realizado_grade`: leitura antecipada `orfas = list(gd.get('orfas') or [])` e retorno dessas órfãs sem marcos. A leitura substitui a leitura antiga no retorno final; muda também o fluxo. |
| mesmo, `+1173` | `_aplicar_folgas_foto`: nova escrita zerando `orfas_instantes`. |
| mesmo, `+1252` | `_marcar_ausencias`: nova escrita zerando `orfas_instantes` em falta coberta. |
| mesmo, `+1296` | Mesmo overlay: duas novas escritas zerando `orfas_instantes`, em ausência integral e justificativa. |
| mesmo, `+1482` | `montar_grade_prevista_periodo_por_turno`: novo mapa `instantes_orfas_por_data`. |
| mesmo, `+1613` | Novo armazenamento de lista vazia nesse mapa. |
| mesmo, `+1696` | Novo coletor `_orf2_instantes`; argumento para `_match_marcos`; novo set de datetimes via `fromisoformat`. Substitui o set de HH:MM. |
| mesmo, `+1723` | Nova condição de candidatura `locs[k][0] in _orfset`, datetime, substituindo membership HH:MM. |
| mesmo, `+1749` | Novo coletor `orfas_instantes` e argumento para `_match_marcos`. |
| mesmo, `+1773` | Novo append de `_cc['instante']` quando célula vira órfã. |
| mesmo, `+1785` | Segundo novo append de `_cc['instante']`, excedente de intervalo. |
| mesmo, `+1811` | Novo armazenamento do coletor em `instantes_orfas_por_data[d]`. |
| mesmo, `+1855` | Novo campo vazio em dia de folga. |
| mesmo, `+1864` | Novo campo preenchido do mapa em dia de trabalho. |
| mesmo, `+1887` | Dois novos appends ISO na conservação de sobras; nova leitura `zip` e reescrita sincronizada das duas listas, ainda ordenadas por HH:MM. |
| mesmo, `+2064` | Nova leitura `dias_out[...].get('orfas_instantes')`; localização por `fromisoformat(_iso) == _inst_t`; mantém `_hhmm_t = _inst_t.strftime('%H:%M')` mais abaixo. |
| mesmo, `+2101` | Nova mutação `orfas_instantes.pop(_ix_orfa)` e remoção posicional no alias `_lista_orf`; substitui remoção pelo valor HH:MM. |
| `ponto/services/cartorio.py`, `+737` | `ata_do_dia`: nova escrita do campo da ata `'orfas_instantes'` e nova leitura `dia.get('orfas_instantes')`, ISO str. Mantém escrita HH:MM em `ata['orfas']`. |
| `ponto/services/espelho.py`, `+494` | `_grade_builder`: nova escrita `dia_grade['orfas_instantes']=[]`, descartando os instantes não lavrados. |
| mesmo, `+857` | `montar_dias`: nova leitura de `_gd.get('orfas_instantes')`, nova publicação `'orfas_instantes_grade'`. |
| `relatorios/pdf_espelho.py`, `+171` | Nova função `_orfas_publicadas`: novas leituras `dia.get('orfas_grade')` e `dia.get('orfas_instantes_grade')`; validação de tamanho/ISO/fuso; retorna `set[datetime]`, vazio ou `None`. `_entrada_espuria` passa a receber e comparar instantes. |
| mesmo, `+485` | Novas chamadas `_orfas_publicadas(d)` por dia; novo set `_orfas_instantes` filtrado por timestamps de batidas da tela. |
| mesmo, `+534` | Chamada de `_entrada_espuria` alterada para novo conjunto de instantes. |
| mesmo, `+584` | Nova escrita `orfas_sem_instantes`; nova comparação `_b.timestamp in _orfas_instantes` para escrever `_b.orfa`. |
| mesmo, `+694` | **Novo casamento textual ainda truncado:** conjunto `_espurias_datadas` com `strftime('%d/%m %H:%M')`; filtro por substring no texto da anomalia. Preserva dia/mês; perde ano, segundos e offset. Não compara datetime. |
| mesmo, `+896` | Nova leitura `dia.get('orfas_sem_instantes')` para render “não sei”. |
| mesmo, `+941` | Novo conjunto `_espurias_instantes` de `b.timestamp`; leitura `pp.entrada` e membership por datetime, substituindo HH:MM. |

Diferença material entre anexo e cópia: o PDF atual ainda remonta grades e lê `orfas` em `app/relatorios/pdf_espelho.py:472`, `:477`, `:544`, `:552`; o hunk `+485` do anexo pressupõe um contexto antigo já baseado na tela. Logo, não afirmar aplicabilidade do patch ao HEAD [não verificado].

O texto HH:MM continua sendo a fonte atual em `app/escala/services/leitor_celula.py:187` e `app/ponto/services/cartorio.py:745`; a spec adiciona transporte paralelo de ISO nos hunks indicados, sem substituir a chave `orfas`.

## 12/11 16:43 · sessao 2746476993e6 · - · - · origem=Ronald (tmux)

Censo AST executado em memória com `python3 -B` (sem gerar arquivos); no escopo de tela/PDF, os sítios atuais e a migração proposta são os seguintes. [não verificado: execução não persistiu relatório]

| Sítio atual | Comportamento hoje | Pré-desenho |
|---|---|---|
| `app/escala/services/leitor_celula.py:180`, `:187`, `:190` | `_orfas_da_ata(ata)` entrega strings da ata ou `'?' * n_orfas`. | Substituir pela porta pública `orfas_da_ata(celula) -> list[datetime] \| None`; todo elemento exige ISO completo e offset; HH:MM, valor inválido ou ausência de lastro retorna `None`, preservando `[]` como zero conhecido. Proposta ancorada no sítio a substituir. |
| `app/escala/services/leitor_celula.py:420` | `grade_da_celula` chama o leitor privado e publica `dia['orfas']`. | Chamar a porta com `cel`; transportar a mesma lista de datetimes/`None`, sem recompor pela data do dia. |
| `app/escala/utils.py:1084`, `:1110` | `montar_realizado_grade` devolve `[], []` sem marcos; no caminho normal faz `list(gd.get('orfas') or [])`. | Não descartar órfãs em dia sem marcos; preservar `None` explicitamente. O ajuste equivalente no diff serve como caso/spec, sem sua dupla lista. |
| `app/ponto/services/espelho.py:519`, `:689`, `:844` | Tela lê grade da célula, passa pelo adaptador e escreve `orfas_grade`. | `orfas_grade` pode conservar o nome, mudando seu tipo para lista aware/`None`; nenhuma lista paralela de instantes. |
| `app/ponto/services/espelho.py:489`, `:530`, `:537`, `:560` | Trecho visual anterior à competência ou ata agregada passa pelo builder antigo. | A degradação de geometria não autoriza publicar órfãs reconstruídas como lavradas: consultar o mesmo leitor da célula e manter `None` se a ata não sabe. |
| `app/templates/ponto/espelho.html:151`, `:240` | Render imprime a string `o` diretamente; órfãs estão dentro do ramo que exige `realizado_grade`. | Render final `date:"H:i"` sobre instante e ramo explícito de `None`; desenhar órfã mesmo quando o dia não tem células realizadas. |
| `app/colaboradores/services/calendario.py:380`, `:381`, `:622`, `:625` | Calendário guarda set de órfãs em mapa por `(dia, mês)` e compara com `b.timestamp.strftime('%H:%M')`, escrevendo `b.orfa=True` em `:627`. | Mapa por data completa para localizar a resposta, mas pertencimento por `b.timestamp` aware; `None` não vira set vazio nem `False`. |
| `app/templates/colaboradores/partials/_calendario_grade.html:121` | Lê `b.orfa` e desenha timestamp com `date:"H:i"`. | Manter HH:MM apenas nesse render; exibir estado desconhecido sem afirmar classificação negativa. |

PDF atual difere da base mostrada no diff: importa `grade_espelho_janela` como `_grade` (`app/relatorios/pdf_espelho.py:193`), remonta o builder para obter órfãs duas vezes (`:475`, `:543`) e só depois consulta a tela (`:598`). Logo, não aplicar o hunk do anexo por transplante; reescrever o caminho atual para consumir a resposta da tela oriunda do leitor único (`app/relatorios/pdf_espelho.py:475`, `:543`, `:598`; desenho do anexo: [não verificado no filesystem]).

Todos os comparadores do PDF que precisam migrar:

- União de HH:MM de toda a janela em `app/relatorios/pdf_espelho.py:472`, `:477`; `_entrada_espuria` compara entrada por HH:MM em `:178`, chamado em `:513`. Trocar por pertencimento do datetime aware e resultado triestado; desconhecido não pode virar “entrada não espúria”. Proposta nos sítios citados.
- Atribuição da resposta à linha por `data_fim - índice` em `app/relatorios/pdf_espelho.py:550`, `:551`, `:552`, seguida de `b.orfa` por HH:MM em `:609`. Consumir a resposta já publicada pela tela e comparar timestamp real; a data localiza a célula, não fabrica o instante. Proposta nos sítios citados.
- Supressão de anomalia por substring HH:MM em `app/relatorios/pdf_espelho.py:716`, `:719`, `:720`. **Também precisa sair**; o diff apenas adiciona `%d/%m`, mantendo identidade textual sem ano/segundos/offset. A fonte atual já perde o instante ao fabricar mensagens em `app/ponto/adaptador_motor.py:33`, `:35`, `:40`, `:42`; para supressão exata, transportar a identidade estruturada do fato desde esse adaptador e renderizar a mensagem no fim. Não reconstruir instante parseando texto. Proposta fundamentada nos pontos de perda citados.
- Supressão dos alertas de turno por HH:MM em `app/relatorios/pdf_espelho.py:960`, `:963`, `:964`. Trocar o conjunto por `b.timestamp` e testar `pp.entrada` diretamente; `None` precisa chegar ao render como desconhecido. Proposta nos sítios citados.
- O HH:MM final legítimo fica em `app/relatorios/pdf_espelho.py:881` e `:885`; `b.orfa` controla marca e legenda em `:876`, `:877`, `:1024`. O campo é escrito pelo coletor em `:609`.

Leitor direto adicional do escopo PDF: `relatorios.cartao_pela_celula.orfas_e_isencao_da_ata` lê `ata.get('orfas')` em `app/relatorios/cartao_pela_celula.py:288`. A busca de referência encontrou somente seu teste (`app/relatorios/tests/test_cartao_pela_celula.py:159`, `:166`) além da definição (`app/relatorios/cartao_pela_celula.py:277`); candidato à remoção pela L-111, não à criação de outro parser (`CLAUDE.md:191`).

Casos/spec aproveitáveis: órfã sem marcos não pode sumir (`app/escala/utils.py:1084`); somente órfã sem `realizado_grade` precisa aparecer no template (`app/templates/ponto/espelho.html:151`, `:231`, `:240`); mesmo HH:MM em dias distintos deve marcar somente o instante correspondente, atingindo os quatro comparadores do PDF acima; ata HH:MM antiga deve devolver desconhecido na tela, calendário e PDF (`app/escala/services/leitor_celula.py:187`; proposta [não verificado]).

Autoridades preservadas: grade/match e cluster guard estão em `app/docs/JUIZES.md:27`; geometria de turno em `:20`; encadeamento cross-midnight em `:24`. A mudança de transporte/leitura não deve criar novo julgamento nesses consumidores (`CLAUDE.md:20`, `:21`).

## 12/11 16:39 · sessao a061d30af7be · - · - · origem=Ronald (tmux)

Recenso na árvore após a cura, com `ast.parse` e inspeção dos operandos e consumidores, sem editar arquivos.

| Sítio | Classe | Interpretação |
|---|---|---|
| `relatorios/pdf_espelho.py:698` e `:702` | **IDENTIDADE — residual** | O conjunto `DD/MM HH:MM` alimenta substring que elimina anomalia. Ainda perde segundos. |
| `escala/utils.py:1760` | EXIBIÇÃO/projeção | Chave `(tipo, HH:MM do marco previsto)` localiza coluna da régua. Não compara a hora da batida. |
| `escala/utils.py:1772` | EXIBIÇÃO/projeção com promoção | A ausência do **marco previsto** na régua promove a célula correspondente; hora e instante são transportados juntos em `:1775-1776`. Não identifica a órfã pela sua hora. |
| `escala/utils.py:1778` | EXIBIÇÃO/projeção com promoção | Marco interno fora da régua ocupa slot `I`; se faltam slots, a célula inteira vira órfã, conservando o instante em `:1787-1788`. |
| `escala/utils.py:1797`, `:1800` | EXIBIÇÃO/projeção | Chave prevista seleciona e deduplica colunas das pontas. |
| `escala/utils.py:1801` | EXIBIÇÃO/projeção | `.index(_key4)` retorna a coluna do marco previsto. |
| `escala/utils.py:1809-1810` | EXIBIÇÃO/projeção | `in` e `.index` mapeiam cada marco previsto à coluna. A pertença da **batida** ao turno é decidida depois por instante em `:2104`. |
| `escala/utils.py:1897` | ORDENAÇÃO | Ordena pares hora/instante pela hora de exibição; conserva os pares em `:1898-1899`. |
| `escala/utils.py:626`, `:669` | EXIBIÇÃO | HH:MM serializado em paralelo com os instantes em `:628`, `:671`; identificação usa índices e datetimes. |
| `escala/services/leitor_celula.py:287`, `:302-303` | EXIBIÇÃO/projeção | Chaves de marcos previstos remapeiam células na régua. |
| `escala/services/leitor_celula.py:347-350` | EXIBIÇÃO/projeção | Deduplicação de colunas previstas por `(tipo, HH:MM)`. |
| `escala/services/leitor_celula.py:25`, `:30-35`, `:121` | EXIBIÇÃO/status | Truncamento para calcular desvio da luz em relação ao previsto; não decide identidade ou pertença de órfã. |
| `relatorios/pdf_espelho.py:714-715`, `:863`, `:867`, `:889`, `:939`, `:1030`, `:1077` | EXIBIÇÃO | Formata escala, batidas, procedência e emissão. Os filtros usam `b.orfa`, decidido em `:590`. |

Verificação dos sítios de identidade após a cura:

- Dry-run compara `datetime` em `escala/utils.py:1702`, `:1726`.
- Localizador da órfã compara ISO reconstruído com instante em `escala/utils.py:2068-2069`.
- Guarda usa timestamps do turno em `escala/utils.py:2092`; interseção por instante em `:2093-2095`; instante da célula em `:2100-2102`; unicidade em `:2103`; pertença e ordem em `:2104`.
- Leitor mantém pareamento posicional em `escala/services/leitor_celula.py:191-193`.
- PDF identifica órfãs e períodos por timestamp em `relatorios/pdf_espelho.py:195`, `:500`, `:590`, `:944-947`.

O achado de identidade reduzida remanescente neste escopo é o filtro textual do cabeçalho do PDF (`relatorios/pdf_espelho.py:698-702`).

_4352 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
