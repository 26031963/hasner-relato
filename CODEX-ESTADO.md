# CODEX-ESTADO

> Ultima resposta processada: **11/11/2026 21:59** · publicado em **11/11/2026 22:00** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 11/11 21:59 · sessao 0ffd773054d2 · LASTRO-FAIXA-SEM-DEFAULT · censo · origem=Code

**O abono materializado no fechamento vem da grade.** A prontidão usa esse snapshot ou calcula diretamente pela grade. O efeito cadastrado também alimenta o motor, em etapa separada. (`app/ponto/services/fechamento.py:697–701`; `app/ponto/services/fechamento.py:1060–1064`; `app/folha/export.py:1078`; `app/folha/export.py:1095–1106`)

Não reexecutei a sonda de produção; consolidei o JSON fornecido. Todos os valores das duas tabelas abaixo têm como fonte esse dado raspado, sem validação independente: **[nao verificado]**. `—` significa combinação não informada no JSON. **[nao verificado]**

| Tipo | Efeito cadastrado | Linhas aprovadas | Linhas rejeitadas | Linhas aguardando_decisao |
|---|---|---:|---:|---:|
| `folga` | abona | 55 | 2 | — |
| `folga_abonada` | abona | 9 | 1 | — |
| `folga_compensatoria` | abona | 48 | 1 | — |
| `feriado_folga` | suprime | 1 | — | — |
| `troca_de_plantao` | abona | 40 | 5 | 1 |
| **Total: 163 linhas** | — | **153** | **9** | **1** |

No corte 21→20, as janelas são: 08/2026 = 21/07–20/08; 09/2026 = 21/08–20/09; 10/2026 = 21/09–20/10; 11/2026 = 21/10–20/11; 12/2026 = 21/11–20/12. (`app/ponto/janelas.py:10–21`)

Cada célula abaixo contém **DIAS / COLABS**, conforme o agregado recebido. **[nao verificado]**

| Tipo | Status | 08/2026 anterior | 09/2026 anterior | 10/2026 anterior | **11/2026 aberta** | 12/2026 futura |
|---|---|---:|---:|---:|---:|---:|
| `folga` | aprovada | — | 3 / 1 | 40 / 22 | **10 / 7** | 2 / 1 |
| `folga` | rejeitada | — | — | 2 / 2 | — | — |
| `folga_abonada` | aprovada | — | 4 / 2 | 5 / 4 | — | — |
| `folga_abonada` | rejeitada | — | — | 1 / 1 | — | — |
| `folga_compensatoria` | aprovada | 17 / 13 | 29 / 19 | 5 / 4 | — | — |
| `folga_compensatoria` | rejeitada | — | — | 2 / 1 | — | — |
| `feriado_folga` | aprovada | 1 / 1 | — | — | — | — |
| `troca_de_plantao` | aprovada | — | 9 / 9 | 20 / 16 | **14 / 14** | — |
| `troca_de_plantao` | rejeitada | — | 1 / 1 | 5 / 4 | — | — |
| `troca_de_plantao` | aguardando_decisao | — | 1 / 1 | — | — | — |

Em **11/2026**, a soma das células aprovadas é **24 dias**; nas anteriores, **18 em 08/2026, 45 em 09/2026 e 70 em 10/2026**. Total de colaboradores distintos entre tipos ou competências: **[nao verificado]**. Situação financeira das competências anteriores — fechadas, exportadas ou pagas: **[nao verificado]**.

**(1) O que fechamento e prontidão usam para abonar**

| Sítio | Comportamento concreto | Arquivo:linha |
|---|---|---|
| Grade | A cobertura para folha seleciona aprovadas; o ramo não-falta escreve `tipo_dia='ausencia'`, sem separar os cinco tipos pelo efeito cadastrado. | `app/ponto/turnos.py:2516–2517`; `app/escala/utils.py:1188`; `app/escala/utils.py:1210–1223` |
| Mapas por dia | Inclui somente `trabalho` e `ausencia`. Para ausência: **abono = previsto − min(realizado, previsto)**. Não consulta efeito nem `ausencia_tipo`. | `app/ponto/services/dia_pago.py:387–401` |
| Fechamento: abono e previsto gravados | Obtém os mapas da grade e atribui a soma do abono a `minutos_abonados`. O previsto também seleciona somente trabalho/ausência. | `app/ponto/services/fechamento.py:1046–1049`; `app/ponto/services/fechamento.py:1060–1064`; `app/ponto/services/fechamento.py:1083` |
| Prontidão com snapshot | Lê previsto, realizado e abono materializados; soma realizado + abono no numerador. | `app/folha/export.py:1045–1050`; `app/folha/export.py:1076–1079` |
| Prontidão sem snapshot | Lê a grade. Soma previsto de trabalho/ausência, abona o **previsto inteiro** das ausências e conta realizado somente dos dias de trabalho. | `app/folha/export.py:1095–1111` |
| Outra etapa do fechamento | Consulta `_a.efeito`: `ABONA` acrescenta a `datas_justificadas`; efeito diferente de `DESCONTA` acrescenta a `_datas_suprimidas`, ressalvado o desvio das parciais. Esses conjuntos alimentam o motor. | `app/ponto/services/fechamento.py:693–701`; `app/ponto/services/fechamento.py:729–740` |

Portanto, **um dia recebido como `folga` fica fora dos três mapas**, inclusive abono e previsto; o fallback da prontidão também o exclui. Isso vale nesse caminho inclusive para `feriado_folga`: o ramo da grade escreve `ausencia` para não-falta, sem consultar seu efeito `suprime`. (`app/ponto/services/dia_pago.py:387–402`; `app/folha/export.py:1096–1106`; `app/escala/utils.py:1213–1223`)

Minutos e valores financeiros correspondentes às contagens raspadas: **[nao verificado]**.

**(2) `folga_abonada` já recebe tratamento monetário diferente de `folga`?**

**Não encontrei distinção monetária específica entre esses nomes nos caminhos examinados.** Ambos têm faixa A2 `folga`; o ramo da grade os trata como não-falta, e o cálculo monetário por grade consulta `tipo_dia`, sem consultar o slug da ausência. (`app/ponto/precedencia.py:81–82`; `app/escala/utils.py:1213–1223`; `app/ponto/services/dia_pago.py:387–401`)

No caminho cadastral, o motor usa o efeito; a rubrica **8932** seleciona os tipos que abonam e exclui apenas `ferias`, `declaracao` e `licenca_maternidade`, sem exceção para nenhum dos dois. (`app/ponto/services/fechamento.py:697–701`; `app/folha/export.py:193–205`; `app/ponto/catalogo/ausencias.py:290`; `app/ponto/catalogo/ausencias.py:308–310`)

O raspado informa **`abona` para ambos**. Diferenças de minutos, parcialidade, batidas ou pagamento efetivo entre seus registros: **[nao verificado]**.

## 11/11 21:58 · sessao 035564513909 · - · - · origem=Ronald (tmux)

Não encontrei regra monetária especial para `folga_abonada` em relação a `folga`. Os caminhos concretos consultados aplicam os mesmos critérios aos dois:

- A2 atribui a ambos a faixa `folga`. `app/ponto/precedencia.py:81-82`.
- A grade marca ausência aprovada não-falta como `tipo_dia='ausencia'`, preservando o slug em `ausencia_tipo`; esse ramo não separa os dois. `app/escala/utils.py:1210-1223`.
- O mapa de dinheiro filtra exclusivamente `tipo_dia in ('trabalho','ausencia')`; para `ausencia`, abona previsto menos realizado coberto. Não consulta `ausencia_tipo` nem o efeito cadastrado. `app/ponto/services/dia_pago.py:387-401`.
- A alimentação do motor usa o efeito cadastrado: `ABONA` inclui em `datas_justificadas`; efeito diferente de `DESCONTA` suprime. A propriedade lê `efeito_vigente(self.tipo)`. `app/ponto/services/fechamento.py:693-701`; `app/ponto/models.py:839-842`.
- A rubrica 8932 seleciona todos os códigos cujo efeito vigente é `ABONA`, excetuando apenas `ferias`, `declaracao` e `licenca_maternidade`. Não há exceção entre `folga` e `folga_abonada`. `app/folha/export.py:193-205`; `app/ponto/catalogo/ausencias.py:288-290`; `app/ponto/catalogo/ausencias.py:308-310`.

O dado fornecido informa `abona` para ambos; validade atual no banco: **[nao verificado]**.

Existe distinção fora do dinheiro: `folga_humana_ou_feriado` considera `tipo_do_dia` diferente de `None` e de `'folga'` como folga humana; assim, `folga_abonada` satisfaz esse termo, e `folga` depende de `folga_lancada`. Seu chamador encontrado decide status. `app/ponto/precedencia.py:211-216`; `app/ponto/status_dia.py:180-181`.

## 11/11 21:58 · sessao 4032abe3bb5a · - · - · origem=Ronald (tmux)

Para os campos indicados, o abono vem da **classificação da grade**, com a ressalva de que o fechamento também usa o efeito do cadastro em outra etapa do motor. (`app/ponto/services/fechamento.py:693-701`; `app/ponto/services/fechamento.py:1060-1064`)

| Sítio | Comportamento concreto | Fonte |
|---|---|---|
| Origem da cobertura na grade | Obtém cobertura com `para='folha'`; esse modo filtra somente `status='aprovada'`, sem filtrar pelo efeito `abona/suprime`. | `app/escala/utils.py:1188`; `app/ponto/turnos.py:2516-2517`; `app/ponto/turnos.py:2753-2754` |
| Classificação da grade | Cobertura não-falta escreve `tipo_dia='ausencia'`, além do tipo, rótulo e ID da ausência. Não há consulta ao efeito nesse ramo. | `app/escala/utils.py:1210-1223` |
| Grade consumida pelo fechamento | `grade_do_fechamento` chama `grade_da_celula` na janela da competência; o leitor recupera `tipo_dia` da ata, com fallback pela célula. | `app/folha/export.py:329-338`; `app/escala/services/leitor_celula.py:397-398` |
| Previsto materializado no fechamento | Soma minutos e conta dias apenas para `tipo_dia in ('trabalho', 'ausencia')`. | `app/ponto/services/fechamento.py:1046-1049`; `app/ponto/services/fechamento.py:1083-1086` |
| Abono materializado no fechamento | Chama `por_dia_da_grade(_g)` e grava `sum(_abono_dia.values())`. A função inclui somente trabalho/ausência; para ausência, abono = previsto − `min(realizado, previsto)`. Não lê efeito do cadastro. | `app/ponto/services/fechamento.py:1060-1064`; `app/ponto/services/dia_pago.py:387-402` |
| Prontidão com snapshot | Se há `previsto_em` e `minutos_previstos`, usa os campos gravados de previsto, realizado e abono; o numerador soma realizado e abono. | `app/folha/export.py:1045-1050`; `app/folha/export.py:1075-1079` |
| Prontidão sem snapshot | Lê diretamente a grade, seleciona trabalho/ausência, soma previsto desses dois; abona o previsto inteiro das ausências e soma realizado com teto somente dos dias de trabalho. Não lê efeito do cadastro nesse ramo. | `app/folha/export.py:1095-1111` |
| Outro uso do efeito no fechamento | Em etapa anterior, lê `_a.efeito`: `ABONA` acrescenta a `datas_justificadas`; efeito diferente de `DESCONTA` acrescenta a `_datas_suprimidas`. Dias suprimidos ficam fora de `datas_previstas_trabalho`; os conjuntos alimentam o motor. | `app/ponto/services/fechamento.py:693-701`; `app/ponto/services/fechamento.py:729-740` |

Portanto, mudar a grade para `folga` exclui o dia dos três mapas de `por_dia_da_grade` e dos cálculos diretos de previsto/abono da prontidão. Isso não altera por si só o ramo anterior que consulta `_a.efeito`. (`app/ponto/services/dia_pago.py:387-402`; `app/folha/export.py:1095-1106`; `app/ponto/services/fechamento.py:693-701`)

Atenção para a precisão do anexo: nesta cópia, `por_dia_da_grade` já calcula abono descontando minutos cobertos por batidas; o fallback da prontidão ainda calcula abono inteiro e realizado apenas para trabalho. Para o exemplo sem batidas do anexo, ambas as formas dão os mesmos 480 minutos de abono. (`app/ponto/services/dia_pago.py:393-401`; `app/folha/export.py:1100-1106`)

## 11/11 21:58 · sessao 1f44a57f3f36 · FIO-E0-V4-E4B · censo · origem=Code

Os dois bloqueantes estão presentes: fechamentos humanos diretos sem coletor e comparação da competência corrente contra `--hoje` (`app/chamados/services/acoes_chamado.py:70,108,149,222,277`; `app/colaboradores/views.py:2068`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:218-230`).

Referências abaixo relativas a `app/`. “Prova” identifica o teste e sua asserção; execução dos testes nesta sessão, banco, logs e identidade Git da árvore: `[nao verificado]`. [nao verificado]

**1. Porta A — inventário das entradas humanas**

| ID | Caminho | Transição / coletor | Nota própria do caminho | Prova de S143 + Pauta |
|---|---|---|---|---|
| A1 | Detalhe → aprovar exceção: `chamados/views.py:426` | `resolvido`, direto, sem coletor: `chamados/services/acoes_chamado.py:70` | `excecao_aprovada`, antes da transição: `chamados/services/acoes_chamado.py:69` | **SEM PROVA** |
| A2 | Detalhe → aceitar/recusar agenda: `chamados/views.py:460` | `resolvido`, direto, sem coletor: `chamados/services/acoes_chamado.py:108` | `agenda_respondida`, no atomic: `chamados/services/acoes_chamado.py:102-108` | **SEM PROVA** |
| A3 | Detalhe → aceitar/recusar escala: `chamados/views.py:463` | `resolvido`, direto, sem coletor: `chamados/services/acoes_chamado.py:149` | `escala_respondida`, no atomic: `chamados/services/acoes_chamado.py:139-149` | **SEM PROVA** |
| A4 | Detalhe/fio → autorizar geofence, com/sem advertência: `chamados/views.py:540`; `chamados/services/fio.py:533` | `resolvido`, direto, sem coletor: `chamados/services/acoes_chamado.py:222` | `geofence_autorizada`: `chamados/services/acoes_chamado.py:218-220` | **SEM PROVA** S143/Pauta; fluxo normal: `chamados/tests/test_s127_fluxo_geofence.py:31,38` |
| A5 | Detalhe/fio → recusar geofence: `chamados/views.py:543`; `chamados/services/fio.py:544` | `resolvido`, direto, sem coletor: `chamados/services/acoes_chamado.py:277` | Retrata batida e escreve `geofence_recusada`: `chamados/services/acoes_chamado.py:269-275` | **SEM PROVA** S143/Pauta; fluxo normal: `chamados/tests/test_geofence_validar_e_recusar.py:87` |
| A6 | Avaliar benefício: `colaboradores/views.py:2046` | Aprovar → `resolvido`; demais ações → `fechado`; direto, sem coletor: `colaboradores/views.py:2068` | `beneficio_avaliado`, depois da transição: `colaboradores/views.py:2069-2071` | **SEM PROVA** |
| A7 | Detalhe → resposta livre/dropdown: `chamados/views.py:547` | Usa coletor, com import local: `chamados/services/acoes_chamado.py:315-316` | `resposta_livre`, quando há conteúdo: `chamados/services/acoes_chamado.py:289-292` | Exportada e limbo mistos: `chamados/tests/test_e4b_c6_orm.py:90-100,148-154` |
| A8 | Fio → resposta rápida/dropdown; exige chamado e conteúdo: `chamados/services/fio.py:708-719` | Usa coletor, com import local: `chamados/services/fio.py:721-722` | `mensagem_dp`: `chamados/services/fio.py:718` | `chamados/tests/test_e4b_c6_orm.py:96-99,156-157` |
| A9 | `fechar_chamados_meio_periodo --apply`: `ponto/management/commands/fechar_chamados_meio_periodo.py:94-109` | `fechar` → `transicionar('fechado')`, sem coletor: `chamados/models.py:452-454` | `fechamento`, depois da transição: `chamados/models.py:455-457` | **SEM PROVA** S143/Pauta |

Os demais chamadores internos de `transicionar` são o bump de geofence para `em_analise` e as duas saídas do wrapper (`chamados/services/acoes_chamado.py:229-233`; `chamados/services/fecho_runtime.py:149-156`). Chamadas por atributo atribuído a apelido ou despacho dinâmico: `[nao verificado]`.

`smoke_chamados` também chama `transicionar`, mas exige banco com nome de teste e desfaz a transação; não constitui persistência produtiva (`chamados/management/commands/smoke_chamados.py:30-44,134,147,163`).

| Outras entradas humanas | Gate e efeito | Coletor / prova pertinente |
|---|---|---|
| Abrir registro de ausência | `analisar` → `em_analise`: `chamados/views.py:1494,1513`; `chamados/models.py:800-806` | Não encerra; **SEM PROVA** S143/Pauta |
| Desarquivar chamado encerrado pela tranca | `reabrir(humano=True)`: `chamados/views_arquivar.py:64-70` | **SEM PROVA** específica das recusas E4B |
| Reabrir disputa como admin | `reabrir_admin` e `ch.reabrir(humano=True)`: `chamados/services/acoes_disputa.py:259,270` | **SEM PROVA** específica das recusas E4B |
| Reabrir competência | Devolve chamados por `reabrir(humano=True)`: `ponto/services/fechamento.py:1447`; `chamados/services/competencia_trancada.py:265` | **SEM PROVA** específica das recusas E4B |
| Aplicar vínculo substituindo aceite anterior | `encerrar` os `escala_aceite` vivos: `colaboradores/services/vinculo.py:384-387` | Sem coletor; **SEM PROVA** S143/Pauta |
| Confirmar regularização externa, inclusive API | API chama a porta; porta encerra `reg.chamado`: `api/views.py:2985-3000`; `chamados/services/regularizacao_ext.py:197-205` | Sem coletor; **SEM PROVA** S143/Pauta |
| Desligamento | Fecha disputas e encerra chamados restantes: `colaboradores/services/desligamento.py:139-155` | Viva segurando encerramento: `chamados/tests/test_o261e_f1_porta_encerramento.py:272` |
| Trancar competência | `encerrar_por_competencia_trancada`: `ponto/views.py:1367`; `ponto/services/fechamento.py:1427`; `chamados/services/competencia_trancada.py:184` | Resolver sai sem escrever para essa via: `chamados/services/disputa_emissao.py:1899-1900` |
| Upload de foto da API | Fecha **Disputa**; signal reconcilia o chamado-pai: `api/foto_recebida.py:80`; `chamados/signals.py:18-45` | Sem coletor; **SEM PROVA** S143/Pauta |

Arquivar comum e arquivar em lote escrevem silêncio, sem mudar `status_local` (`chamados/views_arquivar.py:24-42,140-146`). Registro de `ModelAdmin` que permita editar status de Chamado ou outra API de transição direta: `[nao verificado]`.

O inventário dos consumidores dos demais gates, incluindo comandos acionáveis por humano e cascatas, é:

| Gate / destino | Consumidores de produção localizados |
|---|---|
| `reconciliar` → `resolvido`; escritor: `chamados/models.py:641-654` | `chamados/signals.py:42`; `chamados/reconciliador.py:523`; `chamados/services/adesao.py:115,150`; `chamados/services/feriado_reavaliar.py:47`; `chamados/services/limbo_folgas.py:101,283,286`; `chamados/services/materializacao.py:582,771`; `chamados/services/silencio_ausencia.py:101`; `colaboradores/services/pos_vinculo.py:42`; `ponto/services/ausencia.py:694`; `ponto/services/cartorio.py:986,1511,1524,1545,1623,1638`; `ponto/services/cobertura.py:122,317`; `ponto/services/lastro.py:400`; `ponto/services/veredito_celula.py:202`; `ponto/services/triagem_batida.py:597`; `ponto/services/entrada_sincronizada.py:31`; `ponto/services/cluster_espurio.py:124` |
| Comandos que chegam a `reconciliar` | `chamados/management/commands/auditar_chamados_orfaos.py:67`; `ponto/management/commands/detectar_intervalo_ausente.py:138`; `ponto/management/commands/curar_residuo_bug115.py:151`; `ponto/management/commands/detectar_ausencias.py:158,824,834,845`; `colaboradores/management/commands/marcar_feriado_do_posto.py:140`; `ponto/management/commands/reconciliar_fantasmas.py:274-281` |
| `encerrar` → `fechado`; escritor: `chamados/models.py:1605-1619` | `chamados/reconciliador.py:496`; `chamados/services/regularizacao_ext.py:205`; `colaboradores/services/desligamento.py:155`; `colaboradores/services/vinculo.py:387` |
| `cancelar` → `cancelado`; escritor: `chamados/models.py:773-788` | `escala/management/commands/fechar_avisos_regua_na_lista.py:47`; `ponto/management/commands/fechar_chamados_vinculo_na_lista.py:57` |
| `analisar` → `em_analise`; escritor: `chamados/models.py:800-806` | `chamados/views.py:1513`; `chamados/services/disputa_emissao.py:1085,1159,1178,1223,1252,1379`; `ponto/services/regularizacao.py:137`; `chamados/management/commands/geofence_registrados_para_analise.py:32` |
| `reabrir` → `em_analise`; escritor: `chamados/models.py:1362-1442` | `chamados/views_arquivar.py:70`; `chamados/services/acoes_disputa.py:270`; `chamados/services/competencia_trancada.py:265`; `chamados/services/adesao.py:92`; `chamados/services/disputa_emissao.py:1849,2009`; renascimento: `chamados/models.py:1019`; comandos: `chamados/management/commands/reabrir_orfas_enterradas.py:172`; `ponto/management/commands/reabrir_chamados_ausencia_aberta.py:49`; `ponto/management/commands/detectar_ausencias.py:301`; `ponto/management/commands/reconciliar_grade.py:530` |
| Encerramento pela tranca | `chamados/services/competencia_trancada.py:184` → `chamados/models.py:1622-1639`; comando: `chamados/management/commands/encerrar_por_competencia_trancada.py:20` |

O disparo humano de cada cascata acima, em produção, não consta do dado: `[nao verificado]`. Os destinos dos gates são definidos no motor (`chamados/catalogo/motor.py:99-116`).

**2. Porta A — matriz de transições**

As dimensões pedidas se cruzam: destino/pai/não listado é a âncora; exportada/aberta é a competência; limbo pode coexistir com ambas. Limbo prevalece no motivo quando coexistir com exportada (`chamados/models.py:344-345`; `chamados/services/disputa_emissao.py:1835-1844`).

Cada caminho A1–A9 cruza todas as linhas abaixo. A7/A8 são **C**; A1–A6/A9 são **D**. O wrapper só instala coleta em passagem de `VIVOS` para `ENCERRADOS` (`chamados/services/fecho_runtime.py:149-159`).

| Caminho × entrada | Juiz que decide antes/no S143 | O que se escreve: status, nota, Pauta, trilha | Prova |
|---|---|---|---|
| C ou D × pergunta viva por `chamado_destino`, competência aberta, continua viva no pós-estado | Guarda humana `_vivas_ancoradas` → C6 + entregue `is False`: `chamados/models.py:165-171,349-369` | Recusa antes de `_aplicar`; sem escrita de status/S143/Pauta por essa transição. A nota própria eventualmente anterior depende do atomic do caminho: `chamados/models.py:199`; notas na tabela A1–A8 | Gate por destino preserva aberto: `chamados/tests/test_o261e_f1_porta_encerramento.py:193-198`. Por A1–A9 individualmente: **SEM PROVA** |
| C ou D × pergunta viva pelo pai, sem destino, disputa aberta | S127: disputa aberta sem perguntas ou com `fio_vivo`; **sem filtro de entregue nessa guarda**: `chamados/models.py:153-161`; `chamados/juizes.py:2314-2327` | Recusa antes do status/S143; sem Pauta dessa transição. Vale também se a viva do pai for exportada: `chamados/juizes.py:2322-2327` | Container pelo gate de sistema: `chamados/tests/test_o261e_f1_porta_encerramento.py:94-99`. Transição humana/exportada pelo pai: **SEM PROVA** |
| C ou D × não listado como âncora | Reabridor percorre `_ancoradas`, não toda pergunta viva do colaborador: `chamados/services/disputa_emissao.py:1938-1945`; `chamados/models.py:344-345` | Sem recusa de reabertura dessa pergunta nessa lista. Aceito o fecho, grava destino e trilha; S128/HX ainda podem atuar em seus universos próprios: `chamados/models.py:228-296`; `chamados/services/disputa_emissao.py:1904-1908,2024-2026` | Não varrer destino terminal de outro fecho: `chamados/tests/test_e4b_classe_e_memoria.py:399`. Por caminho humano: **SEM PROVA** |
| **D** × credor toca exportada; demais guardas deixam fechar | Reabridor examina toda carga e retorna `credor_toca_exportada`: `chamados/services/disputa_emissao.py:1829-1844` | Status solicitado pela porta humana, via `admin`, trilha `estado_chamado`; nota `recusa_reabertura_pergunta_viva`; **nenhuma Pauta pelo coletor ausente**: `chamados/models.py:450`; `chamados/catalogo/motor.py:95`; `chamados/services/disputa_emissao.py:1855-1859`; `chamados/services/fecho_runtime.py:18-20` | Reabridor/exportada em memória: `chamados/tests/test_e4b_classe_e_memoria.py:831-841`. A1–A6/A9 ponta a ponta: **SEM PROVA** |
| **C** × mesma recusa exportada | Mesmo reabridor; S143 registra a recusa: `chamados/services/disputa_emissao.py:1945-1950` | Mesmos status/nota/trilha; Pauta DP pela competência datável, salvo marcador vivo já existente: `chamados/services/fecho_runtime.py:114-134`; trilha `pauta_escrita`: `pautas/services.py:180-188` | Mortas carimbadas, órfã fechada, chamado fechado, zero reabertura, uma Pauta: `chamados/tests/test_e4b_c6_orm.py:112-136,148,156` |
| **D** × credor carrega limbo; demais guardas deixam fechar | Texto não vazio sem `respondida_em` na carga; motivo inclui PKs e eventual exportada: `chamados/services/disputa_emissao.py:1837-1842` | Fecho + trilha; nota com motivo próprio; sem Pauta pelo coletor ausente: `chamados/services/disputa_emissao.py:1855-1859`; `chamados/services/fecho_runtime.py:18-20` | Limbo próprio/irmã: `chamados/tests/test_e4b_classe_e_memoria.py:904-924`. A1–A6/A9 ponta a ponta: **SEM PROVA** |
| **C** × mesma recusa limbo | Mesmo juiz e coleta: `chamados/services/disputa_emissao.py:1837-1842,1946-1950` | Fecho + nota + trilha; Pauta com motivo limbo, sujeita às mesmas exclusões de data/marcador: `chamados/services/fecho_runtime.py:64-79,114-134` | Limbo misto com viva exportada e irmã morta da aberta: `chamados/tests/test_e4b_c6_orm.py:151-154`. Limbo com viva exclusivamente aberta bloqueada antes do S143: **SEM PROVA** específica |
| C ou D × competência aberta, mas pergunta morre no pós-estado | C6 no pós-estado não a inclui na trava: `chamados/models.py:356-369` | Pode fechar; S128 só carimba se C6 a der por morta, não trancada e passar as guardas de resposta/materialização: `chamados/services/disputa_emissao.py:1964-1986` | Pós-estado pelo gate de sistema: `chamados/tests/test_o261e_f1_porta_encerramento.py:172-188`. Cada entrada humana: **SEM PROVA** |
| C ou D × credor já vivo quando o reabridor o lê | Reabridor retorna sem ato: `chamados/services/disputa_emissao.py:1826-1828` | Nenhuma nota/Pauta de recusa por essa chamada: `chamados/services/disputa_emissao.py:1828,1855` | Âncora viva: `chamados/tests/test_e4b_classe_e_memoria.py:385` |
| C ou D × credor aberto elegível a reabrir, sem exportada/limbo | `recusa_da_reabertura`, depois `reabrir`: `chamados/services/disputa_emissao.py:1846-1849` | Reabre para `em_analise` e escreve nota; ou registra outra recusa, como `admin_sem_fato_novo`, sem coleta E4B: `chamados/models.py:1390-1442`; `chamados/services/disputa_emissao.py:1850-1859,1946` | Reabertura: `chamados/tests/test_e4b_classe_e_memoria.py:358`; recusa admin: `chamados/tests/test_o261e_f1_porta_encerramento.py:231-245` |

A âncora pelo pai só lista quando não há destino; ser `disputa.chamado` com destino em outro chamado não basta (`chamados/services/fio.py:97-103`; `chamados/models.py:344-345`).

| Condição adicional da matriz | Efeito e prova |
|---|---|
| Justificativa pendente; transição proibida; furo ainda não regularizado | Podem impedir o fecho antes do S143, independentemente de exportada/limbo: `chamados/models.py:123-141,181-198`. Resultado concreto sem esses insumos: `[nao verificado]` |
| Conversa ou pergunta sem dia canônico | A recusa pode ser produzida usando `data_evento` cru, mas o agrupador pula a pergunta sem dia de turno: `chamados/services/disputa_emissao.py:1835-1836`; `chamados/juizes.py:332-336`; `chamados/services/fecho_runtime.py:114-116`. Exclusão do agrupador: `chamados/tests/test_e4b_c6_orm.py:173-187`; coerência completa reabridor→Pauta: **SEM PROVA** |
| Pauta viva já existe na competência | `continue`, sem acrescentar a nova recusa: `chamados/services/fecho_runtime.py:128-130`. Teste exige uma Pauta mesmo após outro chamado/pergunta: `chamados/tests/test_e4b_c6_orm.py:205-214`. Incorporação da segunda recusa: **SEM PROVA** |
| Resolver falha no savepoint | S143 preserva o fechamento e registra erro; coleta do lote não é publicada: `chamados/signals.py:106-118`; `chamados/services/fecho_runtime.py:24-40`. Prova de descarte: `chamados/tests/test_e4b_c6_orm.py:294-312` |
| S128, HX e C7 depois da recusa | Não há retorno antecipado nesse ramo; seguem seus próprios juízes: `chamados/services/disputa_emissao.py:1946-1953,1964,2007,2046,2089`. Prova mista: `chamados/tests/test_e4b_c6_orm.py:115-126` |
| Chamadores fora do runtime | Beco, reconciliador e fantasmas chamam o resolver compartilhado, sem instalar coleta: `chamados/services/becos.py:54`; `chamados/reconciliador.py:536,1648`; `ponto/management/commands/reconciliar_fantasmas.py:281`; coletor: `chamados/services/fecho_runtime.py:18-20`. Prova concreta de passivo sem Pauta cobre save, resolver direto e beco: `chamados/tests/test_e4b_c6_orm.py:159-171`; demais chamadores: **SEM PROVA** específica |

Para `reconciliar`/`encerrar`/`cancelar`, a viva da aberta pode ser reatada ao sobrevivente antes do fecho; sem sobrevivente, há recusa com trilha `fechamento_recusado`. Isso difere da recusa humana por `ValueError` (`chamados/models.py:410-444`; `chamados/tests/test_o261e_f1_porta_encerramento.py:82-110,135-141`). As entradas `analisar`/`reabrir` escrevem estado vivo e não acionam S143 por esse save (`chamados/models.py:806,1442`; `chamados/signals.py:96-97`).

**3. Categorias dos caminhos sem coletor**

| Caminho | Código que cria/seleciona a categoria | Pode excluir pergunta viva ancorada? |
|---|---|---|
| A1, exceção | Normalmente `batida_fora_horario`/`horario_nao_previsto`: `ponto/services/triagem_batida.py:336` | A ação não exige essa categoria; guarda de papel na view: `chamados/services/acoes_chamado.py:21-25`; `chamados/views.py:426`. Exclusão geral: não |
| A2, agenda | `agenda_aceite`/`proposta`, módulo `agenda_dia`: `colaboradores/views.py:2363-2374` | A ação exige categoria, mas o nascimento não estabelece veto a futuras âncoras: `chamados/services/acoes_chamado.py:88`. Relações existentes: **não sei pelo código** |
| A3 e substituição de aceite | `escala_aceite`/`mudanca`, módulo `escala_colaborador`: `colaboradores/services/vinculo.py:393-409`; seleção dos antigos: `colaboradores/services/vinculo.py:384-387` | A ação exige categoria: `chamados/services/acoes_chamado.py:130`. Relações existentes: **não sei pelo código** |
| A4/A5, geofence | Nascimento normal `auditoria_clt`/`gps_fora_geofence`: `ponto/services/geofence.py:361` | Ações não possuem guarda de categoria: `chamados/services/acoes_chamado.py:165-181,243-265`. Exclusão geral: não |
| A6, benefício | Cria módulo `beneficio`, sem categoria explícita: `colaboradores/views.py:1978-1985` | Avaliação aceita `sol.chamado_origem` sem filtro de categoria: `colaboradores/views.py:2066-2068`. FK efetiva: **não sei pelo código** |
| A9, meio período | Filtra módulos `intervalo_pendente` e `batida_fora_escala_leve`, sem filtro de categoria: `ponto/management/commands/fechar_chamados_meio_periodo.py:30,55-57` | Categorias e âncoras dos alvos: **não sei pelo código** |
| Desligamento | Seleciona chamados `NAO_FECHADOS`, sem whitelist de categoria: `colaboradores/services/desligamento.py:150-155` | O próprio fluxo admite viva que impede encerrar: `colaboradores/services/desligamento.py:147-155` |
| Regularização externa | Usa a FK `reg.chamado`: `chamados/services/regularizacao_ext.py:201-205` | Categoria e âncoras dessa FK: **não sei pelo código** |
| Demais comandos/cascatas | Gates genéricos não impõem whitelist de categoria: `chamados/models.py:641,773,1605` | Exclusão geral por categoria: não; relações concretas: **não sei pelo código** |

O derivador automático por categoria não cobre agenda/escala/geofence/exceção; isso demonstra o alcance daquele derivador, não impossibilidade de ancoragem (`chamados/services/disputa_emissao.py:198-213`; `chamados/models.py:3664-3681`). As portas genéricas aceitam destino e o juiz admite conversa, pergunta sem dia e módulo não-DIA, respeitada a identidade do colaborador (`chamados/models.py:2776-2802`; `chamados/juizes.py:2187-2213`).

Os nascimentos de disputa localizados criam pai `disputa_supervisao`; categoria de toda FK histórica não decorre desses nascimentos (`chamados/services/disputa_emissao.py:568-587,1105-1123,1340-1358`). Portanto, “essas categorias nunca ancoram viva em PROD”: `[nao verificado]`.

**4. Porta B — matriz do `--aplicar`**

O juiz registrado de competência de hoje é `janela_atual`; o backfill o consulta com a data de argv, enquanto captura `agora` separadamente (`docs/JUIZES.md:94,98`; `core/ju

[truncado em 20000 de 33201 caracteres]

## 11/11 21:55 · sessao d0425c51ff1b · LASTRO-FAIXA-SEM-DEFAULT · construtor · origem=Code

**perguntas de lei: 1**

**1 — Sítio `ponto/services/dia_pago.py:388`: a folga humana deve retirar o dia dos mapas de abono, previsto e realizado?** Mudar a grade de `ausencia` para `folga` faz exatamente isso. O fechamento grava o abono resultante; portanto, a cura de B1 alcança dinheiro e aciona sua condição de parada. (`ponto/services/dia_pago.py:395`; `ponto/services/fechamento.py:1062`)

Executei a função real extraída por AST, somente em memória, com um dia **sintético** de 480 minutos previstos e zero realizados: [nao verificado]

| `tipo_dia` | Abono | Previsto | Realizado |
|---|---:|---:|---|
| `ausencia` | 480 | 480 | sem entrada |
| `folga` | sem entrada | sem entrada | sem entrada |

Esse resultado decorre do filtro e das somas em `ponto/services/dia_pago.py:387–402`. Impacto quantitativo em produção: **[nao verificado]**.

O censo anterior à edição encontrou:

| Escritor/leitor | Efeito da troca proposta |
|---|---|
| Grade escreve `tipo_dia`, `ausencia_label/id` | Hoje escreve `ausencia` para não-falta. (`escala/utils.py:1220`; `escala/utils.py:1222`; `escala/utils.py:1223`) |
| Dinheiro: `por_dia_da_grade` | Exclui o dia dos três mapas. (`ponto/services/dia_pago.py:388`) |
| Fechamento | Recebe abono/realizado desses mapas e também exclui `folga` da soma de previsto. (`ponto/services/fechamento.py:1047`; `ponto/services/fechamento.py:1061`) |
| Prontidão | Exclui o dia do previsto e do abono calculados pela grade. (`folha/export.py:1097`; `folha/export.py:1101`) |
| Status lavrado | Sem batida, pode mudar de `ausencia` para `folga`. (`ponto/status_dia.py:141`; `ponto/status_dia.py:148`) |
| Suprajuiz / cobertura parcial | `folga` sai do predicado de cobrança parcial; o predicado compartilhado também exige `ausencia`. (`ponto/supra_juiz.py:304`; `chamados/juizes.py:438`) |
| Cartório / releitura | Copia `tipo_dia` para a ata; o leitor recupera esse campo. (`ponto/services/cartorio.py:728`; `escala/services/leitor_celula.py:397`) |

O calendário renderiza o link pelo objeto `dia.ausencia`, antes do ramo visual de folga. Preservação completa do drawer após uma cura: **[nao verificado]**. (`templates/colaboradores/partials/_calendario_grade.html:97`)

Quanto às capturas, a mudança indicada seria deixar **somente `TipoSemFaixa` propagar**:

- Semana do posto: hoje degrada para `trabalho`. (`escala/services/semana_do_posto.py:178`)
- Férias: hoje degrada para `False`. (`ferias/situacao.py:95`)
- Espelho: hoje captura e continua com mapas vazios quando A2 recusa. (`ponto/services/espelho.py:644`; `ponto/services/espelho.py:650`)

Parei sem editar a cópia; B2, B3 e as capturas ficaram pendentes pela parada de B1. Não há cura nem RED/GREEN/mutantes novos entregues ou certificação nesta volta. **[nao verificado por suíte]** [nao verificado]

## 11/11 21:54 · sessao 2b55b7c8a142 · FIO-E0-V4-E4B · censo · origem=Code

Auditoria primeiro: as perguntas identificam o **Codex como verificador**, mas não identificam quem as redigiu; autoria de quem perguntou e identidade de quem decidiu a triagem: **[nao verificado]** (`preverif_c4.q:1`; `preverif_c6.q:1`; `preverif_c7.q:1`). O recibo registra “FILA solicitada / sem cura nesta volta”, sem identificar o solicitante (`app/docs/FIO-E0-V4-E4B-c8.txt:90`). A regra exige motivo para descarte e auditoria prévia de perguntador e triador (`CLAUDE.md:372-384`).

A justificativa “HERDADO, nao regressao” pertence ao **ciclo C↔Cel**; atribuí-la aos achados abaixo seria **[nao verificado]** (`triagem_c9_c10.txt:40`). Para `.q/.r`, contei as linhas dos anexos desde a primeira, incluindo linhas vazias; correspondência com a numeração física dos originais: **[nao verificado]**.

1. **BLOQUEANTE 1 — (b): visto e colocado em FILA.**

   **Pergunta:** “Caminhos que fecham chamado SEM passar pelas duas bordas humanas […] Algum fecha o chamado com pergunta viva da exportada sem que ninguem a entregue?” (`preverif_c6.q:8`).

   **Resposta:** “Fechamentos futuros fora das duas bordas sem entrega E4B | **Não curado — FILA de alcance.**” (`preverif_c7.r:24`).

   **Triagem/motivo registrado:** delimitação às duas bordas; o recibo preserva explicitamente o fechamento direto sem coletor (`app/docs/FIO-E0-V4-E4B-c8.txt:90`, `:93`). Justificativa adicional demonstrando que essas categorias nunca ancoram pergunta viva: **[nao verificado]**. As transições humanas diretas continuam na cópia (`app/chamados/services/acoes_chamado.py:70`, `:108`, `:149`, `:222`, `:277`; `app/colaboradores/views.py:2068`).

2. **BLOQUEANTE 2 — (a): tema presente; contorno pelo `--hoje` não apontado.**

   **Pergunta:** “apply so em competencia aberta […] comp 10 sem apply” (`preverif_c4.q:10`); “reconferencia antes da escrita” (`preverif_c4.q:20`).

   **Resposta sobre o contorno pelo argv:** **ausente**. O Codex havia visto o problema anterior: “**comp 10 sem `!` não possui impedimento próprio**”; portanto, não cabe dizer que ignorou toda a proteção da competência 10 (`preverif_c4.r:9`).

   A classe **(a)** corresponde especificamente à autorização contornável: a cópia compara a competência com a janela de `hoje`, substituível pelo argumento, enquanto `agora` vem do relógio (`app/chamados/management/commands/backfill_fio_morto_e4b.py:218-230`). A revalidação posterior verifica entrega; execução concreta do contorno: **[nao verificado]** (`app/chamados/management/commands/backfill_fio_morto_e4b.py:301-305`).

3. **FILA a — (b): leitores temporais divergentes vistos e mantidos em FILA.**

   **Pergunta:** “nenhum data_evento cru restante no caminho do S143?” (`preverif_c7.q:5`).

   **Resposta:** “A recusa de exportada lê `q.data_evento` […] sem concluir a migração integral — **FILA preexistente**.” (`preverif_c7.r:29`).

   **Triagem/motivo registrado:** “Leitores crus **preexistentes** de data_evento: FILA”; exaustividade declarada não verificada (`app/docs/FIO-E0-V4-E4B-c8.txt:96`). A divergência permanece: recusa pelo atributo cru, agrupador pelo juiz temporal, descartando `None` (`app/chamados/services/disputa_emissao.py:1835-1836`; `app/chamados/services/fecho_runtime.py:114-116`).

4. **FILA b — (b): perda de novas recusas na Pauta existente vista e mantida em FILA.**

   **Pergunta:** “Pauta DP […] idempotente por marcador+ancora” (`preverif_c6.q:6`); depois, “Cada achado do preverif_c6.r […] nao curado” (`preverif_c7.q:5`).

   **Resposta:** “Reentrega não acrescenta novos IDs à Pauta existente | **Não curado — FILA.** […] a nota S143 conserva o rastro individual.” (`preverif_c7.r:23`).

   **Triagem:** “Reentrega nao agrega novos IDs: continue na Pauta viva”, sob “FILA solicitada” (`app/docs/FIO-E0-V4-E4B-c8.txt:90`, `:92`). Motivo adicional do triador: **[nao verificado]**; a preservação da nota é observação da resposta, sem assinatura de triagem (`preverif_c7.r:23`). O `continue` permanece (`app/chamados/services/fecho_runtime.py:128-130`).

5. **FILA c — (e): NAO PERGUNTADO quanto à estabilidade posterior contra o reconciliador.**

   **Pergunta específica sobre terminar em `em_analise` e depois executar o reconciliador:** **ausente**. Há pergunta sobre caminhos do reconciliador e outra sobre falso verde, mas sem esse ensaio de estabilidade (`preverif_c6.q:8`; `preverif_c7.q:9`). [nao verificado]

   **Resposta sobre esse flap:** **ausente** nos anexos fornecidos **[nao verificado: histórico além dos anexos]**.

   O teste exige `em_analise`, confere a trilha e termina sem uma passagem posterior pelo reconciliador; isso permite constatar a cobertura ausente por leitura, sem afirmar que o flap ocorreu (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:211-229`). Ocorrência do flap: **[nao verificado]**.

6. **FILA d — (a): tema dos tetos presente; ressalva de desenho não apontada.**

   **Pergunta:** “Teto de queries: o teste mede o caminho real ou um proxy?” (`preverif_c4.q:24`). [nao verificado]

   **Resposta:** “**O teto global mede caminho real**” (`preverif_c4.r:100`). Reconhecimento dos aumentos e da “PERGUNTA DE DESENHO” aberta: **ausente** nessa resposta.

   A ressalva é legível na cópia: expectativas 213/269 com `[nao verificado]` e pergunta de desenho aberta (`app/ponto/tests/test_selo_chokepoint_escrita.py:192-195`, `:207-209`). Porém, o opus generalizou: **27→28 e 59→60 estão descritos como medidos pela sessão**, não como não verificados (`app/ponto/tests/test_selo_chokepoint_escrita.py:122`, `:132`). A classe refere-se à omissão dessa ressalva documental; certificação independente dos números: **[nao verificado]**.

7. **FILA e — (c): fora do alcance quanto à execução histórica do RED.**

   **Trecho apresentado na pergunta:** “mutantes retorno_antecipado e sem_pauta morrem nos dois RED” (`preverif_c6.q:3`).

   **Resposta que comprove reexecução após c7–c9:** **ausente**. A afirmação “não re-rodado” aparece no opus, mas os logs dessas execuções não foram fornecidos para conferência (`opus_final_c10.r:4`; execução posterior: **[nao verificado]**).

   Há uma limitação estática verificável: o roteiro c9 inclui c8/c7, mas o retorno de c7 não incorpora o ramo padrão c6 que contém `retorno_antecipado`. Isso não exclui execução independente pela sessão (`app/chamados/tests/e4b_c6_verificar.py:320-333`, `:388-389`).

8. **FILA f — (e): NAO PERGUNTADO sobre conciliação dos padrões documentais.**

   **Pergunta sobre `append-only` × `Single Writer Principle` × `Parallel Change`: ausente. Resposta correspondente: ausente.** A c4 inclusive declara “Diff (sem app/docs)”; isso não equivale a uma pergunta sobre coerência dos rótulos (`preverif_c4.q:6`).

   A cópia mostra E4B com `Parallel Change / Expand-Contract`; a linha 1889 pertence a outra obra (`app/docs/BACKLOG.md:1875`, `:1889`). O recibo c4 registra substituição de `append-only`, enquanto c8/c9 declaram `Single Writer Principle` (`app/docs/FIO-E0-V4-E4B-c4.txt:33`; `app/docs/FIO-E0-V4-E4B-c8.txt:5`; `app/docs/FIO-E0-V4-E4B-c9.txt:5`). O merge específico descrito pelo opus e o conteúdo do `recibo_c6.txt`: **[nao verificado]**.

Contagem por classe, nos oito itens acima: **(a) 2 | (b) 3 | (c) 1 | (d) 0 | (e) 2.**

## 11/11 21:53 · sessao 60e1f9eb6616 · - · - · origem=Ronald (tmux)

Inventário direto de chamadas humanas e gates de mudança de estado, somente leitura. Caminho humano de todas as cascatas de fato abaixo: `[nao verificado]` onde não há chamador de borda indicado.

### Fechamentos pedidos diretamente por humano

| Caminho / porta | Escritor de status | Coletor | Prova S143 + Pauta |
|---|---|---|---|
| Detalhe → aprovar exceção | `app/chamados/views.py:426` → `app/chamados/services/acoes_chamado.py:70`, `resolvido` | Não | SEM PROVA |
| Detalhe → aceitar/recusar agenda | `app/chamados/views.py:460` → `app/chamados/services/acoes_chamado.py:108`, `resolvido` | Não | SEM PROVA |
| Detalhe → aceitar/recusar escala | `app/chamados/views.py:463` → `app/chamados/services/acoes_chamado.py:149`, `resolvido` | Não | SEM PROVA |
| Detalhe e fio → autorizar geofence, com/sem advertência | `app/chamados/views.py:540`; `app/chamados/services/fio.py:533` → `app/chamados/services/acoes_chamado.py:222`, `resolvido` | Não | SEM PROVA S143/Pauta; fluxo normal `app/chamados/tests/test_s127_fluxo_geofence.py:31`, `:38` |
| Detalhe e fio → recusar geofence | `app/chamados/views.py:543`; `app/chamados/services/fio.py:544` → `app/chamados/services/acoes_chamado.py:277`, `resolvido` | Não | SEM PROVA S143/Pauta; retratação/fecho normal `app/chamados/tests/test_geofence_validar_e_recusar.py:87` |
| Avaliar benefício | `app/colaboradores/views.py:2046` → `:2068`, `resolvido` se aprovar, `fechado` senão | Não | SEM PROVA |
| Detalhe → resposta livre / dropdown | `app/chamados/views.py:547` → `app/chamados/services/acoes_chamado.py:316` | Sim | `app/chamados/tests/test_e4b_c6_orm.py:90`, `:100`, `:148`, `:151` |
| Fio → resposta rápida com conteúdo / dropdown | `app/chamados/services/fio.py:708`, `:719`, `:722` | Sim | `app/chamados/tests/test_e4b_c6_orm.py:96`, `:156` |
| Command `fechar_chamados_meio_periodo --apply` | `app/ponto/management/commands/fechar_chamados_meio_periodo.py:94`, `:109` → `app/chamados/models.py:454`, `fechado` | Não | SEM PROVA S143/Pauta |

Os chamadores do coletor usam import local em `app/chamados/services/acoes_chamado.py:315` e `app/chamados/services/fio.py:721`; o coletor chama `transicionar` em `app/chamados/services/fecho_runtime.py:150` e `:156`. A outra chamada interna é `Chamado.fechar` em `app/chamados/models.py:454`.

`smoke_chamados` contém chamadas `transicionar` em `app/chamados/management/commands/smoke_chamados.py:134`, `:147`, `:163`, porém recusa banco cujo nome não contém `test` (`:30`) e reverte o atomic ao fim (`:39`); portanto não é caminho produtivo de persistência.

### Outras mudanças humanas diretas

| Caminho | Gate / mudança | Prova específica de S143/Pauta |
|---|---|---|
| Geofence antes da decisão, quando `registrado` | `app/chamados/services/acoes_chamado.py:229`, `:233`: `em_analise` | Fora do fecho; fluxo registrado dos três botões em `app/chamados/tests/test_geofence_validar_e_recusar.py:121` |
| Abrir registro de ausência | `app/chamados/views.py:1494`, `:1513` → `analisar`: `em_analise` | Não fecha; nota em `:1516` |
| Desarquivar encerrado por tranca | `app/chamados/views_arquivar.py:64`, `:70` → `reabrir(humano=True)`: `em_analise` | SEM PROVA S143/Pauta |
| Reabrir disputa como admin | `app/chamados/services/fio.py:581` → `app/chamados/services/acoes_disputa.py:259`, `:270` → `reabrir(humano=True)` | SEM PROVA S143/Pauta |
| Reabrir competência / devolver fila | `app/ponto/services/fechamento.py:1447` → `app/chamados/services/competencia_trancada.py:265` → `reabrir(humano=True)` | SEM PROVA S143/Pauta |
| Substituir aceite antigo de escala ao aplicar vínculo | `app/colaboradores/services/vinculo.py:384`, `:387` → `encerrar`: `fechado` | SEM PROVA S143/Pauta |
| Confirmar regularização externa | `app/chamados/services/regularizacao_ext.py:197`, `:205` → `encerrar`: `fechado` | SEM PROVA S143/Pauta |
| Desligamento | `app/colaboradores/services/desligamento.py:139`, `:142`, `:155` → disputa fecha; pai reconcilia; restantes encerram | `app/chamados/tests/test_o261e_f1_porta_encerramento.py:272` |

Arquivar comum não altera `status_local`; a porta declara e executa silêncio em `app/chamados/views_arquivar.py:24`, `:37`. O `admin_encerra` da **Disputa** existe em `app/chamados/models.py:1943`, mas seu chamador produtivo não foi encontrado: `[nao verificado]`. Não foi localizado registro `ModelAdmin` de Chamado; como ausência sem linha demonstrativa, `[nao verificado]`. API não possui chamada direta a `Chamado.transicionar` localizada; há fechamento de **Disputa** no upload de foto (`app/api/foto_recebida.py:80`) e o signal reconcilia pai (`app/chamados/signals.py:18`, `:42`).

### Censo global por gate, incluindo cascatas e commands

Todos os destinos são decididos no motor: `app/chamados/catalogo/motor.py:99` (humana), `:111` (`RECONCILIAR`), `:112` (`ANALISAR`), `:113` (`REABRIR`), `:114` (`ENCERRAR`), `:116` (`CANCELAR`).

- **`reconciliar` → resolvido**, gate `app/chamados/models.py:641`, escrita `:654`. Chamadores produtivos: `app/chamados/signals.py:42`; `app/chamados/services/adesao.py:115`, `:150`; `app/chamados/services/feriado_reavaliar.py:47`; `app/chamados/services/limbo_folgas.py:101`, `:283`, `:286`; `app/chamados/services/materializacao.py:582`, `:771`; `app/chamados/reconciliador.py:523`; `app/chamados/services/silencio_ausencia.py:101`; `app/ponto/services/cartorio.py:986`, `:1511`, `:1524`, `:1545`, `:1623`, `:1638`; `app/ponto/services/cobertura.py:122`, `:317`; `app/ponto/services/lastro.py:400`; `app/ponto/services/veredito_celula.py:202`; `app/ponto/services/ausencia.py:694`; `app/ponto/services/triagem_batida.py:597`; `app/ponto/services/entrada_sincronizada.py:31`; `app/ponto/services/cluster_espurio.py:124`; `app/colaboradores/services/pos_vinculo.py:42`. Commands: `app/chamados/management/commands/auditar_chamados_orfaos.py:67`; `app/ponto/management/commands/detectar_intervalo_ausente.py:138`; `app/ponto/management/commands/curar_residuo_bug115.py:151`; `app/ponto/management/commands/detectar_ausencias.py:158`, `:824`, `:834`, `:845`; `app/colaboradores/management/commands/marcar_feriado_do_posto.py:140`.
- **`reabrir` → em_analise**, gate `app/chamados/models.py:1362`, escrita `:1442`. Chamadores: `app/chamados/views_arquivar.py:70`; `app/chamados/services/acoes_disputa.py:270`; `app/chamados/services/competencia_trancada.py:265`; `app/chamados/services/adesao.py:92`; `app/chamados/services/disputa_emissao.py:1849`, `:2009`; `app/chamados/models.py:1019` (renascimento por premissa). Commands: `app/chamados/management/commands/reabrir_orfas_enterradas.py:172`; `app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:49`; `app/ponto/management/commands/detectar_ausencias.py:301`; `app/ponto/management/commands/reconciliar_grade.py:530`.
- **`analisar` → em_analise**, gate `app/chamados/models.py:800`, escrita `:806`. Chamadores: `app/chamados/views.py:1513`; `app/chamados/services/disputa_emissao.py:1085`, `:1159`, `:1178`, `:1223`, `:1252`, `:1379`; `app/ponto/services/regularizacao.py:137`; command `app/chamados/management/commands/geofence_registrados_para_analise.py:32` (só aplica após `:27`).
- **`encerrar` → fechado**, gate `app/chamados/models.py:1605`, escrita `:1619`. Chamadores: `app/chamados/reconciliador.py:496`; `app/chamados/services/regularizacao_ext.py:205`; `app/colaboradores/services/desligamento.py:155`; `app/colaboradores/services/vinculo.py:387`.
- **`cancelar` → cancelado**, gate `app/chamados/models.py:773`, escrita `:788`. Commands: `app/escala/management/commands/fechar_avisos_regua_na_lista.py:47`; `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:57`.
- **`encerrar_por_competencia_trancada` → fechado**, gate `app/chamados/models.py:1622`, escrita `:1639`; chamador `app/chamados/services/competencia_trancada.py:184`; dispara no ato de trancar em `app/ponto/services/fechamento.py:1427` ou command `app/chamados/management/commands/encerrar_por_competencia_trancada.py:20`.

### Fatores da matriz A

Os estados pedidos não são classes disjuntas: destino/container é uma dimensão, exportada/aberta é outra, limbo é outra; o motivo limbo prevalece sobre exportada quando coexistem (`app/chamados/services/disputa_emissao.py:1835`, `:1839`, `:1841`).

- Listagem = destino deste chamado OU, **sem destino**, pai da disputa deste chamado (`app/chamados/models.py:344`). Pai com destino em outro chamado não pertence à lista desse pai (`:345`).
- Toda transição humana passa pelo motor e guards de justificativa, disputa, vivas ancoradas e furo (`app/chamados/models.py:123`, `:135`, `:153`, `:165`, `:181`). A guarda de disputa pai é própria e antecede a guarda de destino (`:153`).
- Guarda humana de viva na aberta recusa antes de S143 (`app/chamados/models.py:165`); prova do gate por destino e estado preservado em `app/chamados/tests/test_o261e_f1_porta_encerramento.py:193`.
- A guarda de ancoradas usa competência entregue **is False** e viva no pós-estado; exportada e dia indecidível ficam fora (`app/chamados/models.py:319`, `:327`, `:349`).
- Aceito o fecho, status/campos/trilha saem pelo núcleo (`app/chamados/models.py:228`, `:229`, `:292`, `:296`); via humana é `admin` (`app/chamados/catalogo/motor.py:95`).
- S143 roda para save que escreve estado encerrado e preserva fecho se seu savepoint falhar (`app/chamados/signals.py:90`, `:96`, `:104`, `:115`, `:117`).
- Recusa exportada/limbo escreve nota no credor (`app/chamados/services/disputa_emissao.py:1855`) e só se coleta no contexto do chamado atual (`app/chamados/services/fecho_runtime.py:18`); sem coletor, não há Pauta dessa entrega (`:19`).
- Com coletor, existe Pauta por competência datável, exceto conversa/sem dia e competência já com marcador vivo (`app/chamados/services/fecho_runtime.py:114`, `:128`, `:132`).
- Ausência de listagem não impede S128 sobre outras perguntas de destino nem toda cascata de disputa; o universo S128 é separado (`app/chamados/services/disputa_emissao.py:1904`, `:1938`).
- “Abriu/reabriu após fecho” pode acontecer em terminal→terminal: prova atual acaba em `em_analise`, com trilhas `fechado`, `em_analise` (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:211`, `:222`, `:225`). Reconciliador posterior e flap: SEM PROVA.

### Categorias dos caminhos sem coletor

| Caminho | Nascimento normal | Pode excluir ancoragem pelo código? |
|---|---|---|
| Aprovar exceção | `batida_fora_horario` / `horario_nao_previsto`, `app/ponto/services/triagem_batida.py:336` | Não: ação não guarda categoria (`app/chamados/services/acoes_chamado.py:21`, `:25`); a view só exige papel (`app/chamados/views.py:426`). |
| Agenda | `agenda_aceite` / `proposta`, módulo `agenda_dia`, `app/colaboradores/views.py:2363` | Criador não cria pergunta; derivador por mapa a exclui (`app/chamados/services/disputa_emissao.py:211`; mapa `app/chamados/models.py:3664`). Ação guarda categoria (`app/chamados/services/acoes_chamado.py:88`). Banco legado: não sei pelo código. |
| Escala | `escala_aceite` / `mudanca`, `app/colaboradores/services/vinculo.py:393` | Mesma situação do mapa; ação guarda categoria (`app/chamados/services/acoes_chamado.py:130`). Banco legado: não sei pelo código. |
| Autorizar/recusar geofence | Normal `auditoria_clt` / `gps_fora_geofence`, `app/ponto/services/geofence.py:361` | Ações não guardam categoria (`app/chamados/services/acoes_chamado.py:165`, `:243`); não se pode excluir categorias outras. |
| Benefício | Módulo `beneficio`, categoria omitida, `app/colaboradores/views.py:1978` | Avaliação lê FK `sol.chamado_origem` sem guarda de categoria (`app/colaboradores/views.py:2066`); banco só responde qual FK efetiva. |
| Meio período | Command filtra módulos `intervalo_pendente`, `batida_fora_escala_leve` (`app/ponto/management/commands/fechar_chamados_meio_periodo.py:30`, `:56`) | Não filtra categoria; categorias existentes dos alvos: não sei pelo código. |
| Substituição de aceite de escala | Filtra categoria `escala_aceite`, `app/colaboradores/services/vinculo.py:384` | Criador normal acima; relações existentes: não sei pelo código. |
| Desligamento | Fecha chamados `NAO_FECHADOS` de qualquer categoria (`app/colaboradores/services/desligamento.py:150`) | Não exclui ancoragem; o próprio código prevê resposta viva que segura chamado (`:147`). |
| Regularização externa | Fecha `reg.chamado` por FK (`app/chamados/services/regularizacao_ext.py:201`) | Categoria/ancoragem dessa FK: não sei pelo código. |
| Commands/cascatas genéricas | Gates sem whitelist de categoria (`app/chamados/models.py:641`, `:773`, `:1605`) | Não existe exclusão geral por categoria nesses gates. |

A porta genérica de pergunta aceita `chamado_destino` (`app/chamados/models.py:2776`, `:2794`); `resolvedora_valida` permite conversa, sem dia e módulo não-DIA (`app/chamados/juizes.py:2197`, `:2207`, `:2211`). Portanto exclusão do derivador automático **não prova impossibilidade de ancoragem** pela porta genérica.

Os três nascimentos produtivos de disputa localizados escolhem pai `disputa_supervisao`: núcleo comum (`app/chamados/services/disputa_emissao.py:568`, `:587`), agrupador manual (`:1105`, `:1123`) e âncora ausente (`:1340`, `:1358`). Isso demonstra o nascimento desses caminhos; não demonstra categoria de toda FK histórica em produção.

## 11/11 21:51 · sessao b3dfb46b440f · - · - · origem=Ronald (tmux)

PORTA B — matriz fatorada, somente leitura. Não executei testes; resultados de execução nesta sessão são `[nao verificado]`. [nao verificado]

**Juiz e relógio**

- O juiz registrado de “competência de hoje” é `ponto/janelas.py::janela_atual`: `app/docs/JUIZES.md:98`, `app/core/juizes.py:179`, `app/core/juizes.py:329`. A função responde à janela que contém a **data recebida**, respeitando o corte da empresa: `app/ponto/janelas.py:36`, `app/ponto/janelas.py:60`.
- O backfill pergunta a esse juiz, porém com `hoje` derivado de `--hoje`; o instante real `agora` é capturado separadamente: `app/chamados/management/commands/backfill_fio_morto_e4b.py:218`, `:219`, `:226`.
- “Entregue” é outra pergunta: K6 aponta a `folha/export.py::competencia_entregue`, cujo predicado é ExportacaoDominio vigente e não invalidada; pagamento externo não é um campo consultado: `app/core/juizes.py:177`, `:327`; `app/folha/export.py:580`.
- Dentro do ato, há revalidação de entrega, não de competência corrente: `app/chamados/management/commands/backfill_fio_morto_e4b.py:299`. A composição chamada resolve a competência **do dia do plano**, depois consulta K6: `app/chamados/services/vigia_fio.py:834`.
- A cerca não conhece relógio nem competência de hoje: confere alcance determinado, `comp == alvo` e `entregue is False`: `app/chamados/services/foto_e4b.py:145`.

**Forma exata das combinações**

Sejam `R(e)` a competência de `janela_atual(agora.date(), e)`, `A(e)` a competência de `janela_atual(hoje, e)` e `C` a competência pedida. A guarda permite continuar se, e somente se, `C == A(e)` para **cada empresa visível selecionada**; basta uma divergência para recusar o comando antes do censo: `app/chamados/management/commands/backfill_fio_morto_e4b.py:221`. Essa condição combina-se com a tabela de entrega e a tabela de empresa abaixo; assim cobre o produto de argv × estado × empresa sem assumir cortes de PROD `[nao verificado]`.

| `--hoje` | `--competencia`, relativa ao relógio real | Resultado da guarda de corrente | Prova existente |
|---|---|---|---|
| Ausente | Corrente `C=R(e)` para todas as empresas selecionadas | Passa; `hoje=agora.date()`: `backfill_fio_morto_e4b.py:219`, `:226` | Há teste com corrente simulada, mas o helper sempre fornece uma string `hoje`: `test_e4b_classe_e_memoria.py:646`; caso argv realmente ausente: **SEM PROVA** | [nao verificado]
| Ausente | Anterior | Recusa quando diferente de `R(e)`: `backfill_fio_morto_e4b.py:228` | RED testa `10/2026` contra corrente simulada `11/2026`: `test_e4b_c4v7_memoria.py:131` |
| Ausente | Exportada | “Exportada” não determina corrente; se `C != R(e)`, recusa; se igual, passa para entrega: `backfill_fio_morto_e4b.py:228`, `:253` | Exportada simulada corrente sai BANG sem porta: `test_e4b_classe_e_memoria.py:701`; argv ausente + janela real: **SEM PROVA** |
| Ausente | Paga fora do sistema | Rótulo não lido; se `C != R(e)`, recusa; se igual, passa para entrega: `backfill_fio_morto_e4b.py:228`, `:253`; `folha/export.py:580` | **SEM PROVA** específica de pagamento externo |
| Passado | Corrente real | Passa somente se o passado ainda cai em `C=A(e)` para todas; caso contrário recusa: `backfill_fio_morto_e4b.py:226` | **SEM PROVA** com janela real e passado |
| Passado | Anterior real | **Passa** se argv retrocede à janela dessa anterior; depois aplica o filtro de entrega: `backfill_fio_morto_e4b.py:219`, `:226`, `:257` | Teste existente exige aplicação de comp09 com agora 25/09 e argv 15/09: `test_e4_backfill.py:97`, `:109`, `:142`; ele prova a aceitação pretendida pelo teste, não a recusa exigida pelo aval | [nao verificado]
| Passado | Exportada | Passa a guarda se argv faz `C=A(e)`; entregue=True barra escrita depois: `backfill_fio_morto_e4b.py:226`, `:257`, `:302` | Entrega no ato é coberta, mas não essa combinação temporal exata: `test_e4_backfill.py:160`, `:223`; **SEM PROVA** de corrente real |
| Passado | Paga fora do sistema / entregue=False | Passa se argv faz `C=A(e)`; pagamento externo não muda K6; torna-se elegível a executar portas: `backfill_fio_morto_e4b.py:226`, `:257`, `:259`, `:313`; `folha/export.py:580` | **SEM PROVA** específica |
| Futuro | Corrente real | Passa somente se o futuro ainda cai em `C=A(e)` para todas; se cruzou a janela, recusa: `backfill_fio_morto_e4b.py:226` | **SEM PROVA** com janela real e futuro |
| Futuro | Anterior real | Regra continua `C=A(e)`; o código não compara a data com agora: `backfill_fio_morto_e4b.py:218`, `:219`, `:226` | **SEM PROVA** específica |
| Futuro | Exportada | Mesma igualdade contra argv; True barra escrita depois: `backfill_fio_morto_e4b.py:226`, `:257` | **SEM PROVA** temporal específica |
| Futuro | Paga fora do sistema / entregue=False | Mesma igualdade contra argv; False permite continuar, sem leitura de pagamento: `backfill_fio_morto_e4b.py:226`, `:257`; `folha/export.py:580` | **SEM PROVA** específica |
| Qualquer | Competência futura escolhida pelo próprio argv | Passa se `C=A(e)` para todas; não há comparação com `R(e)`: `backfill_fio_morto_e4b.py:226` | O RED usa janela fixa e não testa futuro via argv: `test_e4b_classe_e_memoria.py:588`; **SEM PROVA** |

Na tabela, os nomes curtos correspondem a `app/chamados/management/commands/backfill_fio_morto_e4b.py`, `app/chamados/tests/...` e `app/folha/export.py`.

| Estado de entrega, depois de a guarda aceitar | Resultado | Prova que morde |
|---|---|---|
| True no plano | `bang:exportada`; fora de `executaveis`: `app/chamados/management/commands/backfill_fio_morto_e4b.py:257`, `:259` | `app/chamados/tests/test_e4b_classe_e_memoria.py:701` |
| False no plano e no ato | Elegível a aplicar; exige ato em `_PORTA`, ausência de limbo/BANG e aprovação da cerca; pode seguir sem porta ou recusar por outros efeitos: `app/chamados/management/commands/backfill_fio_morto_e4b.py:259`, `:308`, `:313`, `:327`, `:366` | Apply/idempotência simulados: `app/chamados/tests/test_e4b_classe_e_memoria.py:681`; ORM aplica anterior real usando argv recuado: `app/chamados/tests/test_e4_backfill.py:142` |
| None no plano | `bang:indeterminada`; fora de `executaveis`: `app/chamados/management/commands/backfill_fio_morto_e4b.py:257`, `:259` | Cerca None: `app/chamados/tests/test_e4b_foto_memoria.py:24`; caso None do comando no plano: **SEM PROVA** específica |
| False no plano → True no ato | Rollback do savepoint antes da porta; BANG exportada: `app/chamados/management/commands/backfill_fio_morto_e4b.py:301` | Spy exige nenhuma entrada do dia em `_executar_dia`: `app/chamados/tests/test_e4_backfill.py:229`; mutante: `app/chamados/tests/test_e4b_c4v7_memoria.py:442` |
| False no plano → None no ato | Rollback antes da porta; BANG indeterminada: `app/chamados/management/commands/backfill_fio_morto_e4b.py:302` | **SEM PROVA** específica do comando |
| Linha alterada alcança True / None / outra competência | Rollback pela cerca: `app/chamados/services/foto_e4b.py:157`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:327` | `app/chamados/tests/test_e4b_foto_memoria.py:18`, `:24`, `:73` |

`competencia_do_dia_entregue` só devolve None quando `dia is None`; para dia presente converte K6 em bool. O plano lê `dia` com `date.fromisoformat`, portanto None na coluna de entrega do plano não é um estado normalmente produzido por esse carregamento; None de alcance da foto continua possível e tratado: `app/chamados/services/vigia_fio.py:834`, `:841`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:251`; `app/chamados/services/foto_e4b.py:157`.

| `--empresa` | Universo/efeito |
|---|---|
| Ausente | Guarda percorre todas visíveis; todas devem concordar com `C`; corte de cada empresa pode diferir: `app/chamados/management/commands/backfill_fio_morto_e4b.py:223`; `app/ponto/janelas.py:36` |
| Uma ou várias visíveis | Guarda apenas selecionadas; uma divergência recusa todo comando: `app/chamados/management/commands/backfill_fio_morto_e4b.py:224`, `:228` |
| Apenas inexistente/invisível | Nenhuma empresa passa na guarda; censo e complemento fazem interseção com visíveis, produzindo universo vazio, sem executar portas: `app/chamados/management/commands/backfill_fio_morto_e4b.py:224`, `:164`; `app/chamados/services/vigia_fio.py:566`; validação explícita de pk solicitado: **SEM PROVA** |
| Visíveis com cortes que geram competências diferentes no mesmo `hoje` | Uma única `C` não satisfaz todas; recusa na primeira divergência: `app/chamados/management/commands/backfill_fio_morto_e4b.py:223`, `:228`; cortes: `app/ponto/janelas.py:50` |

Empresas visíveis são ativas e excluem CNPJ diagnóstico: `app/colaboradores/queries.py:78`. Testes de `--empresa` ausente, seleção mista e cortes divergentes: **SEM PROVA** localizada; o helper de memória fixa empresa `[2]` e janela única: `app/chamados/tests/test_e4b_classe_e_memoria.py:585`, `:588`, `:646`.

**Ponto da prova que não morde o bloqueante**

O RED `test_RED_apply_so_competencia_corrente_falante` só troca `competencia`; usa `janela_atual=lambda *a: (..., 20/11/2026)`, indiferente à data argv. Seu mutante remove a comparação `if comp != corrente`, cobrindo a existência da comparação, sem distinguir relógio real de `--hoje`: `app/chamados/tests/test_e4b_c4v7_memoria.py:131`, `:449`; `app/chamados/tests/test_e4b_classe_e_memoria.py:588`.

Mais forte: a fixture ORM congela agora em 25/09, documenta que isso é comp10, injeta 15/09 para aplicar comp09 e exige escrita; há expectativa de teste favorável ao comportamento contornável: `app/chamados/tests/test_e4_backfill.py:27`, `:97`, `:109`, `:144`, `:147`.

Dependências de uma volta B, para a consolidação: guarda de produção em `app/chamados/management/commands/backfill_fio_morto_e4b.py:218`; expectativa temporal do helper ORM em `app/chamados/tests/test_e4_backfill.py:97`; RED corrente e mutante em `app/chamados/tests/test_e4b_c4v7_memoria.py:131`, `:449`; double de janela e passagem de argv em `app/chamados/tests/test_e4b_classe_e_memoria.py:588`, `:646`. A cerca e K6 não respondem competência de hoje e não precisam receber essa pergunta para caracterizar B: `app/chamados/services/foto_e4b.py:145`; `app/folha/export.py:556`.

## 11/11 21:51 · sessao ccb68bd8192e · - · - · origem=Ronald (tmux)

Somente leitura; não executei testes. [nao verificado]

- **FILA c — confirmação limitada:** `test_i_terminal_para_terminal_passa` termina exigindo `em_analise` e trilha `['fechado', 'em_analise']`; não chama reconciliador depois desse estado. O trecho encerra verificando nota e `materializada_em=None`. Portanto o teste prova a reabertura, não mede o flap posterior. (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:211`, `:222`, `:225`, `:226`, `:229`) O recibo c4 já nomeia esse teste, mas não documenta ensaio posterior contra o reconciliador. (`app/docs/FIO-E0-V4-E4B-c4.txt:121`)

- **FILA d — confirmado no código, com distinção:** 27→28 e 59→60 estão descritos como **medidos pela sessão**, não `[nao verificado]`. (`app/ponto/tests/test_selo_chokepoint_escrita.py:117`, `:122`, `:127`, `:132`) Os valores 213 e 269 estão mantidos como tetos; sua expectativa após cura aparece `[nao verificado]`, e o custo da carga canônica continua `PERGUNTA DE DESENHO`. (`app/ponto/tests/test_selo_chokepoint_escrita.py:184`, `:192`, `:193`, `:196`, `:204`, `:207`, `:208`, `:210`) Logo não generalizar o rótulo `[nao verificado]` aos quatro deltas.

- **FILA e — execução histórica não verificável; roteiro explica lacuna:** `retorno_antecipado` está definido com assertion-alvo `"S128 nao carimbou a morta"`. (`app/chamados/tests/e4b_c6_verificar.py:45`) `ensaios(c9)` inclui c8, c8 inclui c7, porém c7 retorna somente casos/mutantes C7 e não inclui o ramo padrão c6, onde estão `retorno_antecipado` e `sem_pauta`. (`app/chamados/tests/e4b_c6_verificar.py:320`, `:325`, `:328`, `:332`) `--orm --c9` usa esse roteiro. (`app/chamados/tests/e4b_c6_verificar.py:388`) A ausência de rerun independente depois c7–c9 continua **[nao verificado]**; `c6_orm.log:15,38` é apenas referência do anexo opus, não log disponível para inspeção.

- **FILA f — a referência do opus diverge da cópia:** E4B está em `app/docs/BACKLOG.md:1875`, com `padrao=Parallel Change / Expand-Contract, fase BACKFILL`; `:1889` é outra obra. O recibo c4 registra literalmente `"antes de substituir append-only"` e a substituição para Parallel Change. (`app/docs/FIO-E0-V4-E4B-c4.txt:33`, `:9`) Os recibos c8/c9 declaram `Single Writer Principle`. (`app/docs/FIO-E0-V4-E4B-c8.txt:5`, `app/docs/FIO-E0-V4-E4B-c9.txt:5`) Assim há evidência de três rótulos ao longo dos documentos, mas **não** de `append-only` na linha E4B atual. Conteúdo específico do `recibo_c6.txt` e merge que preservaria append-only: **[nao verificado]**.

Para classificar pelos anexos: FILA c/f não são nomeadas especificamente nas perguntas fornecidas; d tem tema de teto explicitamente na c4 (`preverif_c4.q`, item 8); e envolve prova histórica/RED, com tema genérico nas c6–c9 e alegações de execução nos cabeçalhos. Atribuir c/e/f a falha de leitura sem separar pergunta e prova seria extrapolação. [nao verificado: classificação final depende da numeração literal dos anexos pelo agente principal]

## 11/11 21:50 · sessao 2039fd776329 · - · - · origem=Ronald (tmux)

Caminhos abaixo relativos à raiz, todos somente lidos.

| FILA | Cruza A/B? | Constatação e prova |
|---|---|---|
| (a) `q.data_evento` cru versus dia canônico | **A e B** no reabridor compartilhado: S143 chama `reabrir_credor_de_pergunta_viva` (`app/chamados/services/disputa_emissao.py:1945`); o reabridor avalia toda carga usando `competencia_do_dia_entregue(q.data_evento, ...)` (`:1835`). A chamada do backfill ao mesmo reabridor deve ser citada pelo agente B. | O juiz canônico devolve `None` para conversa (`app/chamados/juizes.py:332`); agrupador pula `None` (`app/chamados/services/fecho_runtime.py:114`). Assim, recusa coletada de conversa/sem dia não gera Pauta (`:115`). Testes confirmam a exclusão no agrupador, **não** a coerência ponta a ponta com o reabridor: `app/chamados/tests/test_e4b_c6_memoria.py:173`; ORM conversa `app/chamados/tests/test_e4b_c6_orm.py:173`. |
| (b) uma Pauta viva por competência | **A**, mesma função `_escrever_pautas` (`app/chamados/services/fecho_runtime.py:92`). Fora do contexto runtime, coletor não recebe nada (`:18`). | Encontrada qualquer Pauta viva marcador+âncora, dá `continue`, sem incluir nova pergunta, chamado ou motivo (`:128`). Prova atual **consagra** essa dedup: teste entrega segunda pergunta `pk=999999` de outro colaborador/chamado na mesma competência e exige total 1 (`app/chamados/tests/test_e4b_c6_orm.py:205`, `:211`, `:214`). Não há assert de incorporação da segunda recusa nesse caso: **SEM PROVA** (`:212`). |
| (c) test_i acaba em `em_analise` | **A**, núcleo de estado e S143: teste invoca `_gravar_estado('fechado', via=ENCERRAR)` (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:221`). | O teste exige estado final `em_analise`, trilha `['fechado','em_analise']` e nota “Reaberto: pergunta viva” (`:222`, `:225`, `:226`). Não roda reconciliador depois dessa reabertura; flap posterior **SEM PROVA** (corpo `:211`–`:228`). |
| (d) tetos aumentados | **A**, porque custo vem da mesma carga canônica do resolver/S143; os comentários produtivos apontam para a carga `chamado._ancoradas()` (`app/chamados/services/disputa_emissao.py:1938`). Nenhum cruzamento direto com `--aplicar` demonstrado. | Tetos 28 e 60 estão em `app/ponto/tests/test_selo_chokepoint_escrita.py:124`, `:134`; comentários atribuem +1 `_ancoradas` por S143 (`:121`, `:131`). Tetos 213 e 269 constam `:196`, `:210`, com “Esperado ... [nao verificado]” e “PERGUNTA DE DESENHO” (`:192`, `:193`, `:207`, `:208`). As medições narradas nos comentários não são execução conferida nesta cópia: `[nao verificado]`. |
| (e) mutante retorno antecipado | **A**, muta exatamente `_resolver_perguntas_por_chamado_resolvido`, substituindo `registrar_recusa` por retorno `{0,0}` (`app/chamados/tests/e4b_c6_verificar.py:45`). | O alvo é “S128 nao carimbou a morta”; testes ORM assertam mortas carimbadas, órfã fechada, chamado fechado, 0 reabrir e 1 Pauta (`app/chamados/tests/test_e4b_c6_orm.py:115`, `:118`, `:120`, `:122`, `:126`). `sem_pauta` também existe e troca retorno do agrupador por `return 0` (`app/chamados/tests/e4b_c6_verificar.py:48`). Reexecução ORM pós-c7–c9: `[nao verificado]`; `c6_orm.log` ausente no inventário local. O recibo c10 **narra** protocolo memória c9 com 17 ensaios após GREEN (`app/chamados/tests/e4b_c10_entrega.txt:75`), não prova ORM pós-c9 (`:5`). |
| (f) três padrões / merge BACKLOG | **Não cruza campo ou função de A/B**: são metadados de obra. | **Divergência do anexo com a cópia:** linha E4B da cópia é `app/docs/BACKLOG.md:1875`, já `Parallel Change / Expand-Contract, fase BACKFILL`; linha `:1889` é outro BO. `append-only` está na obra mãe `FIO-E0-V4-E4-ESCRITORES` (`:1866`). Recibos c8/c9 registram `Single Writer Principle` (`app/docs/FIO-E0-V4-E4B-c8.txt:5`; `app/docs/FIO-E0-V4-E4B-c9.txt:5`). `recibo_c6` não encontrado; conteúdo dele `[nao verificado]`. |

Prova de isolamento do passivo já existente: o ORM prepara chamado encerrado, chama save/S143, resolver direto e `fechar_beco`, e exige nenhuma Pauta (`app/chamados/tests/test_e4b_c6_orm.py:159`). Isso cobre esses atos concretos; não cobre todos os chamadores de reconciliador/fantasmas citados pelo aval (`:167`–`:170`).

Dependências para o pai consolidar, sem prescrever implementação:

1. Estado/dia/competência e fronteira do runtime precedem a revisão das células de prova: reabridor compartilhado (`app/chamados/services/disputa_emissao.py:1805`), agrupador (`app/chamados/services/fecho_runtime.py:92`), duas bordas atuais (`app/chamados/tests/test_e4b_c6_orm.py:90`).
2. Depois, prova de cada porta humana e reexecução dos mesmos mutantes sobre a árvore final: catálogo dos mutantes (`app/chamados/tests/e4b_c6_verificar.py:44`); testes de transições e trilha (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:211`).
3. Tetos só podem ser avaliados sobre a árvore final; a cópia registra os quatro tetos e duas perguntas em aberto (`app/ponto/tests/test_selo_chokepoint_escrita.py:124`, `:134`, `:193`, `:208`).

Arquivos de prova diretamente implicados por (a)–(e): `app/chamados/tests/test_e4b_c6_memoria.py:92`, `app/chamados/tests/test_e4b_c6_orm.py:148`, `app/chamados/tests/e4b_c6_verificar.py:44`, `app/chamados/tests/test_e4b_c6_protocolo.py:128`, `app/chamados/tests/test_o261e_f1_porta_encerramento.py:211`, `app/ponto/tests/test_selo_chokepoint_escrita.py:124`. Fechar a lista com os arquivos de B e dos demais caminhos de A exige as respostas dos outros agentes `[nao verificado]`.

Possível pergunta de lei a **não confundir com bug técnico**: quando já existe Pauta viva marcador+competência, a recusa posterior precisa constar nessa Pauta, ou a primeira entrega basta? Código atual pula a posterior (`app/chamados/services/fecho_runtime.py:128`); teste exige só unicidade (`app/chamados/tests/test_e4b_c6_orm.py:214`); o aval fornecido pede Pauta ancorada na competência, mas não especifica acumulação `[nao verificado]`.

## 11/11 21:50 · sessao 8d0587f714b2 · - · - · origem=Ronald (tmux)

Evidências da cópia, somente leitura:

- **B1:** adaptador em `app/chamados/services/acoes_chamado.py:316` e `app/chamados/services/fio.py:722`; demais transições diretas nos cinco pontos de `acoes_chamado.py:70/108/149/222/277` e `app/colaboradores/views.py:2068`. Coleta exige contexto (`app/chamados/services/fecho_runtime.py:18-20`), instalado pelo adaptador (`:153-159`). Registro de fila literal: “fechar() fora das duas bordas continua na transicao direta e sem coletor instalado nao entrega Pauta” (`app/docs/FIO-E0-V4-E4B-c8.txt:93`).
- **B2:** `--hoje` aceito (`app/chamados/management/commands/backfill_fio_morto_e4b.py:199`); relógio real em `:218`, data substituível em `:219`; autorização compara competência com `janela_atual(hoje, empresa)` (`:226-230`). Revalidação posterior consulta somente entrega (`:301-305`). Cerca exige alvo e `entregue=False`, sem conferir competência corrente real (`app/chamados/services/foto_e4b.py:145-163`). Execução do bypass: [nao verificado].
- **FILA a:** recusa usa `q.data_evento` cru (`app/chamados/services/disputa_emissao.py:1835-1836`); ausência de dia retorna `None` (`app/chamados/services/vigia_fio.py:834-835`), que satisfaz `is not False`. Agrupador usa juiz temporal e pula `None` (`app/chamados/services/fecho_runtime.py:114-116`); juiz devolve `None` para conversa (`app/chamados/juizes.py:332-336`). Registro de fila literal: “Leitores crus preexistentes de data_evento: FILA” (`app/docs/FIO-E0-V4-E4B-c8.txt:96`).
- **FILA b:** Pauta viva com marcador/âncora causa `continue` (`app/chamados/services/fecho_runtime.py:128-130`). Registro literal: “Reentrega nao agrega novos IDs: continue na Pauta viva” (`app/docs/FIO-E0-V4-E4B-c8.txt:92`).
- **Âncora de B1:** `return p.chamado_destino_id or (d.chamado_id if d is not None else None)` (`app/chamados/services/fio.py:117`).

Os registros acima estão sob “6. FILA solicitada / sem cura nesta volta” (`app/docs/FIO-E0-V4-E4B-c8.txt:90`); identificação pessoal de quem solicitou essa fila: [nao verificado].

## 11/11 21:50 · sessao 648439db674a · - · - · origem=Ronald (tmux)

Auditoria normativa:

- `CLAUDE.md:363-365`: “Sem a hipotese de quem pergunta”; exige QUEM ESCREVE/QUEM LE.
- `CLAUDE.md:372-374`: achado descartado, inclusive “fila”, exige “1 linha de motivo” e anexo ao opus.
- `CLAUDE.md:379-384`: classificação por achado e auditoria “PRIMEIRO quem perguntou e quem triou”.
- Autoria pessoal/modelo de quem escreveu cada `.q` e decidiu cada triagem: **[nao verificado]**. Os cabeçalhos identificam obra, papel e voltas, sem assinatura; o recibo não identifica automaticamente o triador. `app/docs/FIO-E0-V4-E4B-c8.txt:1` identifica apenas “recibo do construtor”.

Evidência local de descarte:

- `app/docs/FIO-E0-V4-E4B-c8.txt:90`: “6. FILA solicitada / sem cura nesta volta”.
- `:92`: “Reentrega nao agrega novos IDs: continue na Pauta viva”.
- `:93`: “fechar() fora das duas bordas continua na transicao direta e sem coletor instalado nao entrega Pauta”.
- `:96`: “Leitores crus preexistentes de data_evento: FILA [...] Exaustividade [nao verificado].”
- `app/docs/FIO-E0-V4-E4B-c9.txt:93`: sete comparações condicionais permanecem em fila, guards/FKs/ciclos concretos não verificados.
- `:103`: prova concorrente, criação concorrente do contador, suíte e opus em fila. [nao verificado]

A linha fornecida `triagem_c9_c10.txt:40` descarta **o ciclo C↔Cel da c9**, como “HERDADO, nao regressao”, sob “CONGELA-O-20: sem numero de celula/fio morto”. Não aplicar esse motivo automaticamente a B1/FILA a,b: essa atribuição não consta da linha.

Padrões e divergência de localização:

- **Na cópia atual**, E4B está em `app/docs/BACKLOG.md:1875`, com “padrao=Parallel Change / Expand-Contract, fase BACKFILL”.
- **Na cópia atual**, `app/docs/BACKLOG.md:1889` é BO-0611-05-RESTO-DA-CLASSE, não E4B.
- `app/docs/FIO-E0-V4-E4B-c4.txt:33`: “R5: assert do padrao da linha E4B deu rc=1 antes de substituir append-only”.
- `app/docs/FIO-E0-V4-E4B-c4.txt:9` e `:102`: Parallel Change / Expand-Contract, BACKFILL.
- `app/docs/FIO-E0-V4-E4B-c8.txt:5` e `app/docs/FIO-E0-V4-E4B-c9.txt:5`: Single Writer Principle.
- `recibo_c6.txt` e arquivos `.q/.r` não encontrados como arquivos locais na busca; usar os anexos do prompt e numeração própria explicitada. Merge e procedência “append-only do main” alegados pelo opus: **[nao verificado]** pela cópia.

## 11/11 21:50 · sessao 4902a7374e30 · LASTRO-FAIXA-SEM-DEFAULT · verificador · origem=Code

**REPROVADO.** Há bloqueantes de coerência entre leitores e de significado de `folga`; a classificação incondicional de `troca_de_plantao` também não preserva as condições do corte existente. (`app/escala/utils.py:1220`; `app/ponto/status_dia.py:141`; `app/ponto/precedencia.py:215`; `app/docs/CORTES.md:187`)

A análise abaixo é da cópia e dos anexos fornecidos. Execuções RED/GREEN/mutante, identidade Git dos commits e situação efetiva da produção: **[nao verificado]**. Não rodei suíte nem acessei banco/log. [nao verificado]

1. **Cumprimento do aval**

| Exigência | Avaliação | Evidência e limite |
|---|---|---|
| Mapear cada tipo ativo, um a um | **Cumprido em parte** | Os 17 códigos enumerados no censo fornecido estão explícitos; o mapa também cobre todos os 17 `Ausencia.TIPO_CHOICES`, as 17 sementes e os três históricos indicados. Cobertura de outro cadastro ativo não apresentado: **[nao verificado]**. (`app/ponto/precedencia.py:53`; `app/ponto/models.py:661`; `app/ponto/migrations/0052_seed_tipoausencia.py:9`) |
| Justificar cada faixa em uma linha no RELATO | **Cumprido em parte** | Há comentários individuais no mapa. Registro correspondente da c3 no RELATO: **não achei [nao verificado]**. (`app/ponto/precedencia.py:53`; `app/ponto/precedencia.py:83`) |
| Recusa nomeada fora do mapa | **Cumprido** | `TipoSemFaixa(ValueError)`, mensagem com código e acesso sem default; não herda de `VetoDePrecedencia`. (`app/ponto/precedencia.py:96`; `app/ponto/precedencia.py:100`) |
| Contador por tipo ativo, mesmo sem lançamento | **Cumprido** | Percorre cadastro, filtra `ativo`, chama a autoridade real e captura especificamente `TipoSemFaixa`; publica quantidade, códigos, “Esperado 0” e URL. (`app/ponto/services/vigia_ausencia.py:144`; `app/ponto/services/vigia_ausencia.py:155`; `app/ponto/services/vigia_ausencia.py:168`) |
| Contador com caso que morde | **Cumprido no código do teste** | Tipo fantasma ativo, nenhuma `Ausencia`, exige 1; inativação exige 0. Execução: **[nao verificado]**. (`app/ponto/tests/test_lastro_faixa_sem_default.py:108`) |
| RED pk 5356 | **Cumprido em parte** | Persiste ausência aprovada `folga`, chama A2 real e exige faixa/rótulo corretos. Não atravessa grade/lavra/tela, onde restam divergências. (`app/ponto/tests/test_lastro_faixa_sem_default.py:92`; `app/escala/utils.py:1220`; `app/ponto/status_dia.py:141`) |
| RED tipo fantasma | **Cumprido no código do teste** | Chamada real exige exceção com código. Execução: **[nao verificado]**. (`app/ponto/tests/test_lastro_faixa_sem_default.py:13`) |
| Mutante “default de volta” | **Cumprido em parte** | As asserções distinguem o mutante: fantasma deixa de levantar e contador deixa de contar. Os dois failures informados são compatíveis com isso; comando/log efetivo: **não achei [nao verificado]**. (`app/ponto/tests/test_lastro_faixa_sem_default.py:16`; `app/ponto/tests/test_lastro_faixa_sem_default.py:114`; `app/ponto/services/vigia_ausencia.py:163`) |
| PROIBIDO campo novo de cadastro | **Cumprido no diff fornecido** | A implementação consulta os campos existentes `codigo/ativo`; não acrescenta faixa ao modelo. (`app/ponto/services/vigia_ausencia.py:160`; `app/ponto/models.py:577`; `app/ponto/models.py:607`) |
| PROIBIDO fallback | **Cumprido na origem; em parte nos efeitos** | `faixa_de` recusa. Capturas antigas agora transformam essa recusa em `False`, `trabalho` ou mapas vazios; detalhado abaixo. (`app/ponto/precedencia.py:103`; `app/ferias/situacao.py:95`; `app/escala/services/semana_do_posto.py:178`; `app/ponto/services/espelho.py:644`) |
| PROIBIDO juiz novo | **Cumprido no diff fornecido** | A classificação continua na A2 registrada; vigia consulta essa autoridade. Restam derivações anteriores divergentes. (`app/core/juizes.py:186`; `app/ponto/services/vigia_ausencia.py:162`; `app/escala/utils.py:1220`) |
| PROIBIDO tocar exportada | **Cumprido quanto ao diff fornecido** | As alterações produtivas são classificação e medição; não incluem operação sobre exportada. Invariância financeira/TXT após eventual pouso: **[nao verificado]**. O fechamento tem regras próprias de cobertura/efeito. (`app/ponto/precedencia.py:100`; `app/ponto/services/vigia_ausencia.py:155`; `app/ponto/services/fechamento.py:693`; `app/folha/export.py:4`) |

O requisito final de suíte inteira e opus final continua pendente conforme o enunciado; comprovação local: **[nao verificado]**. Isso é pendência de **PRONTO**, além dos bloqueantes de código desta preverificação. [nao verificado]

2. **Quem escreve, quem lê e propagação**

| Elemento | Quem escreve/define | Quem lê |
|---|---|---|
| `_FAIXA_DE_TIPO` | Literal em `app/ponto/precedencia.py:52` | Único acesso produtivo encontrado: `faixa_de`, em `app/ponto/precedencia.py:103`. |
| `faixa_de` | Definição em `app/ponto/precedencia.py:100` | Quatro chamadas produtivas: `app/ponto/precedencia.py:406`, `app/ponto/precedencia.py:522`, `app/ponto/precedencia.py:543`, `app/ponto/services/vigia_ausencia.py:162`. Patch temporário em teste: `app/ponto/tests/test_lastro_faixa_sem_default.py:144`. |
| `TipoSemFaixa` | Classe em `app/ponto/precedencia.py:96`; lançada em `app/ponto/precedencia.py:105` | Captura específica no vigia: `app/ponto/services/vigia_ausencia.py:163`; testes de recusa e mock: `app/ponto/tests/test_lastro_faixa_sem_default.py:16`, `app/ponto/tests/test_lastro_faixa_sem_default.py:140`, `app/colaboradores/tests/test_calendario_ausencia_pela_autoridade.py:59`. |
| `_nome_do_tipo` | Definição em `app/ponto/precedencia.py:108` | Veto em `app/ponto/precedencia.py:492`; teste em `app/ponto/tests/test_lastro_faixa_sem_default.py:59`. Retorna display ou código cru, sem consultar faixa: `app/ponto/precedencia.py:122`, `app/ponto/precedencia.py:128`. |
| `tipos_ativos_sem_faixa` | Derivação local em `app/ponto/services/vigia_ausencia.py:158`; publicação em `app/ponto/services/vigia_ausencia.py:168` | Agregados em `app/ponto/services/vigia_ausencia.py:231`; revisão em `app/ponto/services/vigia_ausencia.py:259`; API em `app/api/views_mensageria.py:1417`; teste em `app/ponto/tests/test_lastro_faixa_sem_default.py:90`. |

**Todas as chamadas produtivas de `faixa_de` encontradas:**

| Chamada | Tipo fora do mapa |
|---|---|
| `fatos_do_periodo`: `app/ponto/precedencia.py:406` | Propaga, interrompendo a **janela inteira**. Classifica todas as ausências antes de ordenar; outra ausência conhecida mais forte não evita a recusa. (`app/ponto/precedencia.py:404`; `app/ponto/precedencia.py:421`) |
| `veto_de_lancamento`: `app/ponto/precedencia.py:522` | Propaga antes do laço das folgas. Um veto por sobreposição pode acontecer antes da consulta à faixa. (`app/ponto/precedencia.py:488`; `app/ponto/precedencia.py:531`) |
| Texto do veto: `app/ponto/precedencia.py:543` | Sem captura local. Um código desconhecido estável já teria recusado na chamada anterior, portanto não chega aqui. (`app/ponto/precedencia.py:522`; `app/ponto/precedencia.py:539`) |
| Vigia: `app/ponto/services/vigia_ausencia.py:162` | Captura `TipoSemFaixa`, acrescenta o código e publica o problema. Outras exceções propagam. (`app/ponto/services/vigia_ausencia.py:163`; `app/ponto/services/vigia_ausencia.py:168`) |

Import local do vigia: `app/ponto/services/vigia_ausencia.py:157`. Outro chamador direto por apelido/import local: **não achei**. Chamada construída dinamicamente: **[nao verificado]**.

As chamadas diretas de testes são fantasma, enumeração do mapa, choices, sementes, controle `abono` e delegação do wrapper mockado; neste último, a referência real foi guardada antes do patch. (`app/ponto/tests/test_lastro_faixa_sem_default.py:17`; `app/ponto/tests/test_lastro_faixa_sem_default.py:40`; `app/ponto/tests/test_lastro_faixa_sem_default.py:47`; `app/ponto/tests/test_lastro_faixa_sem_default.py:54`; `app/ponto/tests/test_lastro_faixa_sem_default.py:136`; `app/ponto/tests/test_lastro_faixa_sem_default.py:141`)

**Depois da chamada indireta:**

| Leitor | Captura e resultado |
|---|---|
| `fatos_do_dia` | Delega ao período e propaga. (`app/ponto/precedencia.py:281`) |
| Calendário | Warning com traceback e relançamento; não entrega calendário degradado. (`app/colaboradores/services/calendario.py:507`; `app/colaboradores/services/calendario.py:515`) |
| Espelho, inclusive alias `_fatos_periodo` | Captura genérica, warning e continuação com ambos os mapas vazios, pois a primeira atribuição não terminou. Toda a janela perde essa alimentação. (`app/ponto/services/espelho.py:644`; `app/ponto/services/espelho.py:646`; `app/ponto/services/espelho.py:651`) |
| Palavra do espelho | Recebe `_fatos_do_dia=None` e pula a tradução de palavra/cor/veredito. (`app/ponto/services/espelho.py:870`; `app/ponto/services/espelho.py:339`) |
| Cartório | `aplicar_precedencia_na_celula` propaga antes de carimbar; `julgar_celula` captura, loga e retorna o resultado anterior, sem atualizar esse carimbo. (`app/ponto/services/cartorio.py:1985`; `app/ponto/services/cartorio.py:2032`; `app/ponto/services/cartorio.py:2040`) |
| Situação de férias | Captura silenciosamente e retorna `False` para descanso; pode omitir data do aviso de véspera. (`app/ferias/situacao.py:93`; `app/ferias/situacao.py:108`) |
| Semana do posto | Captura silenciosamente e retorna **`trabalho`**; candidato previsto sem batida vai para **buracos**, em vez de explicado por `atestado`. (`app/escala/services/semana_do_posto.py:175`; `app/escala/services/semana_do_posto.py:151`) |
| Status do dia | Propaga na alimentação da lavra e nos caminhos sem lavra. (`app/ponto/status_dia.py:181`; `app/ponto/status_dia.py:298`; `app/ponto/status_dia.py:304`) |
| Folha sem furo | Propaga quando consulta a janela das lâmpadas apagadas. (`app/folha/services/sem_furo.py:20`; `app/folha/services/sem_furo.py:25`) |
| Dia do colaborador | Propaga localmente. (`app/ponto/services/dia_do_colab.py:15`) |
| Deslogados / faltas de hoje / abriu e não bateu | Propagam localmente, quando alcançam a consulta. (`app/ponto/services/deslogados.py:166`; `app/ponto/services/faltas_de_hoje.py:61`; `app/ponto/services/abriu_nao_bateu.py:123`) |
| Detector / reavaliação feriado-folga | Propagam localmente. (`app/ponto/management/commands/detectar_ausencias.py:616`; `app/chamados/services/feriado_reavaliar.py:33`) |

Os tradutores do veto capturam somente `VetoDePrecedencia`; `TipoSemFaixa` escapa, sem virar “sobreposição”. As chamadas de folgas não têm essa captura local. (`app/ponto/services/ausencia.py:382`; `app/ponto/services/declarar_dia.py:121`; `app/ferias/models.py:438`; `app/ferias/views.py:255`; `app/escala/folgas.py:310`; `app/escala/folgas.py:336`)

**Sim, há degradação agora alcançável:** `trabalho`, `False` e mapas vazios. As capturas são anteriores e foram deliberadamente mantidas pelo brief; não atribuo ao diff a criação dessas capturas. Elas passam a receber a nova recusa. (`app/escala/services/semana_do_posto.py:178`; `app/ferias/situacao.py:95`; `app/ponto/services/espelho.py:650`; `app/ponto/precedencia.py:105`)

O contador segue a entrega existente: revisão → escritor semanal → pauta; comando e cron existentes. (`app/ponto/services/vigia_ausencia.py:259`; `app/pautas/semanal.py:202`; `app/ponto/management/commands/vigiar_familia_ausencia.py:23`; `app/config/crons.py:739`)

Quanto aos insumos: `TipoAusencia.codigo/ativo` entram pela UI e pela porta `gravar_tipo_ausencia`, além da semeadura; `Ausencia.tipo` é escrito na criação/edição da porta existente. A porta de cadastro **continua sem validar faixa**, conforme a exclusão explícita do brief. (`app/ponto/views_tipos_ausencia.py:67`; `app/ponto/views_tipos_ausencia.py:89`; `app/ponto/services/ausencia.py:1301`; `app/ponto/migrations/0052_seed_tipoausencia.py:33`; `app/ponto/services/ausencia.py:342`; `app/ponto/services/ausencia.py:498`)

3. **Mapa, significado e precedência**

A regra operacional é esta: férias vencem todas; afastamento vence atestado/feriado/folga/trabalho; atestado vence feriado/folga/trabalho; folga vence somente trabalho. Empates de faixa conservam a ordem de entrada na ordenação. (`app/ponto/precedencia.py:46`; `app/ponto/precedencia.py:421`)

| Tipo | Faixa atribuída | Justificativa escrita no mapa |
|---|---|---|
| `ferias` | férias | Precedência própria. (`app/ponto/precedencia.py:53`) |
| `afastamento_inss` | afastamento | Afastamento acima de atestado. (`app/ponto/precedencia.py:54`) |
| `suspensao` | afastamento | Afasta comparecimento. (`app/ponto/precedencia.py:55`) |
| `licenca_maternidade` | afastamento | Segue licenças. (`app/ponto/precedencia.py:56`) |
| `licenca_paternidade` | afastamento | Segue licenças. (`app/ponto/precedencia.py:57`) |
| `licenca_luto` | afastamento | Analogia às licenças já mapeadas. (`app/ponto/precedencia.py:58`) |
| `atestado` | atestado | Fato humano. (`app/ponto/precedencia.py:59`) |
| `atestado_inss` | atestado | Fato humano, independentemente do pagador. (`app/ponto/precedencia.py:60`) |
| `abon_hr` | atestado | Abono parcial. (`app/ponto/precedencia.py:61`) |
| `abono` | atestado | Abono. (`app/ponto/precedencia.py:62`) |
| `atraso` | atestado | Ausência parcial lançada. (`app/ponto/precedencia.py:63`) |
| `aviso_home_office` | atestado | Lançado como ausência. (`app/ponto/precedencia.py:64`) |
| `declaracao` | atestado | Comparecimento. (`app/ponto/precedencia.py:65`) |
| `declaracao_acompanhante_sem_ab` | atestado | Fato humano independentemente de abono. (`app/ponto/precedencia.py:66`) |
| `declaracao_de_acompanhante` | atestado | Acompanhante. (`app/ponto/precedencia.py:67`) |
| `desligamento` | atestado | Lançado como ausência. (`app/ponto/precedencia.py:68`) |
| `falta` | atestado | Fato humano mesmo descontando. (`app/ponto/precedencia.py:69`) |
| `reciclagem` | atestado | Lançada como ausência. (`app/ponto/precedencia.py:70`) |
| `reducao_de_7_dias_do_aviso` | atestado | Redução lançada. (`app/ponto/precedencia.py:71`) |
| `saida_antecipada` | atestado | Ausência parcial lançada. (`app/ponto/precedencia.py:72`) |
| `treinamento` | atestado | Lançado como ausência. (`app/ponto/precedencia.py:73`) |
| `atestado_medico` | atestado | Código histórico. (`app/ponto/precedencia.py:74`) |
| `obito` | atestado | Código histórico. (`app/ponto/precedencia.py:75`) |
| `outros` | atestado | Código histórico. (`app/ponto/precedencia.py:76`) |
| `folga_compensatoria` | folga | A própria folga do dia. (`app/ponto/precedencia.py:79`) |
| `feriado_folga` | folga | A própria folga do dia. (`app/ponto/precedencia.py:80`) |
| `folga` | folga | Folga lançada como ausência. (`app/ponto/precedencia.py:81`) |
| `folga_abonada` | folga | Folga independentemente de abono. (`app/ponto/precedencia.py:82`) |
| `troca_de_plantao` | folga | Presume representar o dia cedido. (`app/ponto/precedencia.py:83`) |

**Contradições e escolhas relevantes:**

- **`troca_de_plantao`: achei condição de negócio omitida.** O mapa presume dia cedido para qualquer ocorrência. O corte exige par cedido/assumido, confirmação administrativa e dois anexos; o tipo de ausência é pedido até confirmação. A A2 exclui somente rejeitadas e não verifica essas condições. Portanto, a equivalência incondicional a folga é uma hipótese de negócio adicional, embora tenha sido mandada pelo brief. Confirmação/natureza dos lançamentos raspados: **[nao verificado]**. (`app/ponto/precedencia.py:83`; `app/docs/CORTES.md:187`; `app/ponto/precedencia.py:298`; `app/ponto/precedencia.py:406`)

- **`licenca_luto`: escolha substantiva explícita.** Sai do comportamento anterior de atestado e passa a vencer atestado; o catálogo confirma licença paga de dois dias, mas não estabelece essa superioridade. O brief escolhe a analogia; fundamento adicional dessa precedência: **não achei [nao verificado]**. (`app/ponto/precedencia.py:58`; `app/ponto/catalogo/ausencias.py:98`; `app/ponto/precedencia.py:46`)

- **`desligamento`: contradição comprovada apenas pelo nome, não achei.** O comentário só repete “lançado como ausência”. Não há descrição específica desse código no catálogo/choices examinados; tratá-lo como término do vínculo seria inferência **[nao verificado]**. Sua classificação vence feriado/folga/trabalho. (`app/ponto/precedencia.py:68`; `app/ponto/catalogo/ausencias.py:87`; `app/ponto/models.py:661`; `app/ponto/precedencia.py:46`)

- **`aviso_home_office`: tensão semântica, sem prova de faixa errada.** O modelo diz “cumprido em home office”; o catálogo o mantém como ausência abonada. Trabalho efetivamente prestado versus dispensa de comparecimento nos eventos fornecidos: **[nao verificado]**. A faixa escolhida suprime trabalho/feriado/folga na A2. (`app/ponto/models.py:678`; `app/ponto/catalogo/ausencias.py:104`; `app/ponto/precedencia.py:64`; `app/ponto/precedencia.py:421`)

- **`atraso` e `saida_antecipada`: parcialidade não prova outra faixa.** O catálogo os declara parciais; efeito financeiro, duração e precedência são perguntas diferentes. A faixa atestado preserva o comportamento anterior desses códigos, mas a A2 não representa por essa faixa o restante trabalhável do dia. (`app/ponto/catalogo/ausencias.py:9`; `app/ponto/catalogo/ausencias.py:75`; `app/ponto/catalogo/ausencias.py:90`; `app/ponto/precedencia.py:63`; `app/ponto/precedencia.py:72`; `app/ponto/precedencia.py:406`)

- **`folga`, `folga_abonada`, `troca_de_plantao`: mudança operacional real.** Agora perdem para feriado e ganham somente de trabalho; anteriormente recebiam atestado pelo default informado no diff. O status sem lavra passa a `folga`, e o reavaliador passa a poder selecionar a cobrança como espúria. (`app/ponto/precedencia.py:81`; `app/ponto/precedencia.py:46`; `app/ponto/status_dia.py:312`; `app/chamados/services/feriado_reavaliar.py:33`)

Nenhuma dessas faixas, inclusive `afastamento`, passa a autorizar atravessar FolgaDia humana: a permissão continua exclusiva de férias. (`app/ponto/precedencia.py:521`; `app/ponto/precedencia.py:539`)

4. **Classes que já reprovaram obra**

| Classe | Resultado |
|---|---|
| Segundo juiz duplicando registrado | **Não achei juiz novo no diff. Achei decisão paralela anterior ainda divergente:** A2 atribui folga; a grade classifica toda ausência aprovada não-falta como `ausencia`, sem consultar a faixa. (`app/core/juizes.py:186`; `app/ponto/precedencia.py:81`; `app/escala/utils.py:1210`; `app/escala/utils.py:1220`) |
| Segundo escritor fora da porta | **Não achei introduzido.** A2 devolve fatos; carimbo continua pela porta da célula. O contador usa o escritor semanal existente. (`app/ponto/services/cartorio.py:1985`; `app/ponto/portas/celula.py:1076`; `app/ponto/services/vigia_ausencia.py:260`) |
| Relógio fora da borda | **Não achei relógio novo.** Testes DB congelam tempo e passam `HOJE`; há leituras anteriores quando data não é fornecida ao vigia. (`app/ponto/tests/test_lastro_faixa_sem_default.py:62`; `app/ponto/tests/test_lastro_faixa_sem_default.py:89`; `app/ponto/services/vigia_ausencia.py:58`; `app/ponto/services/vigia_ausencia.py:258`) |
| Derivação paralela de fonte única | **Achei.** Grade → fatos do status → lavra produz `ausencia`; ramo sem lavra lê A2 e produz `folga`. O carimbo de precedência posterior só altera `ata`, não corrige `status_dia`. (`app/escala/utils.py:1220`; `app/ponto/status_dia.py:189`; `app/ponto/status_dia.py:141`; `app/ponto/status_dia.py:312`; `app/ponto/portas/celula.py:1084`) |
| Selo/RED sem mordida | **Não achei ausência total de mordida.** Fantasma e contador distinguem o default. **Achei cobertura insuficiente da cura entre leitores:** pk 5356 testa somente A2. (`app/ponto/tests/test_lastro_faixa_sem_default.py:16`; `app/ponto/tests/test_lastro_faixa_sem_default.py:114`; `app/ponto/tests/test_lastro_faixa_sem_default.py:100`) |
| Campo com dois significados — ESMERIL | **Achei, concretamente.** `tipo_do_dia='folga'` significa tanto código de uma ausência humana quanto marcador genérico da folga automática. O predicado exclui esse valor para reconhecer folga humana. (`app/ponto/precedencia.py:81`; `app/ponto/precedencia.py:410`; `app/ponto/precedencia.py:

[truncado em 20000 de 24752 caracteres]

## 11/11 21:46 · sessao 363a49956f66 · - · - · origem=Ronald (tmux)

Achados principais para consolidação:

1. **Divergência estrutural entre A2 e grade/lavra: achei.** `folga`, `folga_abonada` e `troca_de_plantao` agora pertencem à faixa `folga`; A2 publica faixa e rótulo correspondentes. (`app/ponto/precedencia.py:81`, `app/ponto/precedencia.py:421`, `app/ponto/precedencia.py:430`) Entretanto, `_marcar_ausencias` consulta cobertura aprovada e transforma todo tipo diferente de `falta` em `tipo_dia='ausencia'`, sem consultar a faixa. (`app/ponto/turnos.py:2516`, `app/ponto/turnos.py:2753`, `app/escala/utils.py:1188`, `app/escala/utils.py:1210`, `app/escala/utils.py:1220`) O cartório monta essa grade e passa seu `tipo_dia` para a lavra; sem batida, o status resultante é `ausencia`. (`app/ponto/services/cartorio.py:1161`, `app/ponto/services/cartorio.py:1843`, `app/ponto/status_dia.py:189`, `app/ponto/status_dia.py:141`, `app/ponto/portas/celula.py:963`) Já o ramo sem lavra lê faixa `folga` e responde `folga`. (`app/ponto/status_dia.py:308`) Não achei sobreposição posterior que corrija isso: nos dois builders, FolgaDia entra antes da ausência e depois vem somente isenção antes dos demais cálculos. (`app/escala/utils.py:806`, `app/escala/utils.py:1844`, `app/escala/utils.py:1850`) É uma divergência demonstrável por leitura estática; reprodução integrada está **[nao verificado]**. A coerência entre leitores é exigida pela L-099. (`app/docs/LEIS.md:122`)

2. **ESMERIL-SEMANTICA: achei colisão concreta de `tipo_do_dia='folga'`.** A2 usa esse valor tanto para o código da `Ausencia` quanto para a folga genérica da escala; `folga_humana_ou_feriado` exclui expressamente `tipo_do_dia='folga'` para reconhecer a genérica. (`app/ponto/precedencia.py:81`, `app/ponto/precedencia.py:409`, `app/ponto/precedencia.py:428`, `app/ponto/precedencia.py:215`) Para uma `Ausencia(tipo='folga')` sem FolgaDia, o predicado retorna `False`; para `folga_abonada`, retorna `True`. (`app/ponto/precedencia.py:215`, `app/ponto/precedencia.py:437`) O status consulta esse predicado no veredito `indefinida`, cuja decisão distingue `folga` de `escala_suspeita`. (`app/ponto/status_dia.py:143`, `app/ponto/status_dia.py:177`) Execução desse cenário está **[nao verificado]**.

3. **`troca_de_plantao`: hipótese de negócio acrescentada.** O comentário afirma que qualquer lançamento desse tipo representa o dia cedido. (`app/ponto/precedencia.py:83`) O corte exige ato com par cedido/assumido, confirmação administrativa e dois documentos; o tipo de ausência representa pedido até confirmação. (`app/docs/CORTES.md:187`) A2 exclui apenas rejeitadas e não verifica essas condições antes de classificar a ocorrência. (`app/ponto/precedencia.py:298`, `app/ponto/precedencia.py:406`) Assim, a classificação incondicional elimina distinção exigida pelo corte; natureza e confirmação dos lançamentos raspados estão **[nao verificado]**.

4. **Efeito de cada grupo do mapa:** férias vencem todas as outras faixas; afastamento vence atestado/feriado/folga/trabalho; atestado vence feriado/folga/trabalho; folga vence somente trabalho e perde para feriado. (`app/ponto/precedencia.py:46`, `app/ponto/precedencia.py:421`) Os grupos efetivamente declarados são:
   - `ferias`: `ferias`. (`app/ponto/precedencia.py:53`)
   - `afastamento`: `afastamento_inss`, `suspensao`, `licenca_maternidade`, `licenca_paternidade`, `licenca_luto`. (`app/ponto/precedencia.py:54`)
   - `atestado`: `atestado`, `atestado_inss`, `abon_hr`, `abono`, `atraso`, `aviso_home_office`, `declaracao`, `declaracao_acompanhante_sem_ab`, `declaracao_de_acompanhante`, `desligamento`, `falta`, `reciclagem`, `reducao_de_7_dias_do_aviso`, `saida_antecipada`, `treinamento`, `atestado_medico`, `obito`, `outros`. (`app/ponto/precedencia.py:59`)
   - `folga`: `folga_compensatoria`, `feriado_folga`, `folga`, `folga_abonada`, `troca_de_plantao`. (`app/ponto/precedencia.py:79`)

5. **Limites semânticos dos outros exemplos:** efeito financeiro, duração e parcialidade são perguntas distintas de precedência; o catálogo não fornece conversão automática desses atributos em faixa. (`app/ponto/catalogo/ausencias.py:9`, `app/ponto/catalogo/ausencias.py:75`, `app/ponto/precedencia.py:48`) Portanto, `atraso` e `saida_antecipada` serem parciais/SUPRIME não prova que devam receber outra faixa. (`app/ponto/catalogo/ausencias.py:90`, `app/ponto/precedencia.py:63`, `app/ponto/precedencia.py:72`) `aviso_home_office` é descrito como aviso “cumprido em home office”, mas mantido como ausência abonada; o sentido real dos eventos está **[nao verificado]**. (`app/ponto/models.py:678`, `app/ponto/catalogo/ausencias.py:104`) Para `desligamento`, fundamentação específica além do comentário genérico do mapa: **não achei**; sua natureza nos dados está **[nao verificado]**. (`app/ponto/precedencia.py:68`) `licenca_luto` recebe afastamento por analogia às demais licenças; o catálogo confirma licença paga de dois dias, mas não estabelece que deva vencer atestado. (`app/ponto/precedencia.py:58`, `app/ponto/catalogo/ausencias.py:98`)

6. **RED não cobre essas divergências.** A fixture cria diretamente a ausência `folga` e chama somente `fatos_do_dia`; não atravessa grade, status lavrado ou renderização. (`app/ponto/tests/test_lastro_faixa_sem_default.py:92`) O setup não cria cadastro ativo de `folga`, e a semeadura também não o contém. (`app/ponto/tests/test_lastro_faixa_sem_default.py:67`, `app/ponto/migrations/0052_seed_tipoausencia.py:9`) Isso não invalida a mordida da classificação A2, mas deixa o comportamento dos demais leitores sem cobertura nesse teste. (`app/ponto/tests/test_lastro_faixa_sem_default.py:100`)

## 11/11 21:45 · sessao 0e55e745c27f · - · - · origem=Ronald (tmux)

Censo fechado por ocorrências estáticas na cópia; não achei chamada adicional por apelido de import nem import local além do vigia. Chamadas construídas dinamicamente: [nao verificado].

| Símbolo | Quem escreve/define | Quem lê |
|---|---|---|
| `_FAIXA_DE_TIPO` | Literal único em `app/ponto/precedencia.py:52` | Apenas `faixa_de`, por indexação direta, em `app/ponto/precedencia.py:103`. |
| `faixa_de` | Função em `app/ponto/precedencia.py:100` | Produção: `app/ponto/precedencia.py:406`, `:522`, `:543`; import local `app/ponto/services/vigia_ausencia.py:157`, chamada `:162`. |
| `TipoSemFaixa` | Classe `ValueError`, independente de `VetoDePrecedencia`, em `app/ponto/precedencia.py:96` | Lançada em `app/ponto/precedencia.py:105`; capturada especificamente pelo contador em `app/ponto/services/vigia_ausencia.py:163`; testes em `app/ponto/tests/test_lastro_faixa_sem_default.py:15`, `:16`, `:140` e `app/colaboradores/tests/test_calendario_ausencia_pela_autoridade.py:59`. |
| `_nome_do_tipo` | Função em `app/ponto/precedencia.py:108` | Única chamada de produção no texto de veto, `app/ponto/precedencia.py:492`; teste `app/ponto/tests/test_lastro_faixa_sem_default.py:59`. Não consulta mais faixa: retorna display ou código cru (`app/ponto/precedencia.py:122`, `:128`). |
| `tipos_ativos_sem_faixa` | Derivado de todos os tipos cadastrados, filtrando ativo e perguntando à autoridade real (`app/ponto/services/vigia_ausencia.py:144`, `:159`, `:160`, `:162`, `:168`) | `linhas`, `acesos`, `total_aceso` em `app/ponto/services/vigia_ausencia.py:231`; revisão em `:259`; API `app/api/views_mensageria.py:1417`; testes `app/ponto/tests/test_lastro_faixa_sem_default.py:90`. |

Cada chamada de produção de `faixa_de`:

- `app/ponto/precedencia.py:406`: ausência desconhecida interrompe **a janela inteira**, sem captura local; nem mesmo outra ausência conhecida de faixa mais forte evita a recusa, pois todos os tipos são classificados antes da ordenação (`app/ponto/precedencia.py:404`, `:421`).
- `app/ponto/precedencia.py:522`: desconhecido recusa antes de percorrer folgas; propaga. Pode haver veto de sobreposição anterior, que ocorre antes da consulta de faixa (`app/ponto/precedencia.py:488`, `:522`).
- `app/ponto/precedencia.py:543`: chamada para texto do veto, sem captura local; desconhecido estável já teria recusado em `:522`, portanto não alcança este ramo (`app/ponto/precedencia.py:522`, `:539`).
- `app/ponto/services/vigia_ausencia.py:162`: captura **somente** `TipoSemFaixa`, agrega o código e publica quantidade/frases/URL; outras exceções propagam (`app/ponto/services/vigia_ausencia.py:163`, `:168`, `:175`).

Chamadas diretas de testes: fantasma real (`app/ponto/tests/test_lastro_faixa_sem_default.py:17`); classificação enumerada (`:40`); choices (`:47`); sementes (`:54`); controle conhecido (`:136`); wrapper do mock chamando a referência real guardada (`:141`). A recusa é esperada só no caso fantasma; nos demais controles reais inesperados ela falha o teste; o wrapper gera a recusa proposital de `abono`, e o vigia deve capturá-la (`app/ponto/tests/test_lastro_faixa_sem_default.py:16`, `:139`, `:144`, `:147`).

Leitores indiretos e degradações:

| Leitor | Resultado para desconhecido na janela |
|---|---|
| `fatos_do_dia` | Delega e propaga (`app/ponto/precedencia.py:281`). |
| Calendário | Captura, warning com traceback e relança; não entrega calendário degradado (`app/colaboradores/services/calendario.py:507`, `:515`, `:519`). |
| Espelho | Captura genérica, warning e segue com **ambos mapas vazios**, porque a primeira atribuição falhou antes de retornar (`app/ponto/services/espelho.py:644`, `:648`, `:651`). `_fatos_do_dia` fica `None` (`:870`); tradução de palavra/cor/veredito é pulada (`:331`, `:339`, `:344`). Isso afeta toda a janela carregada, não só o dia desconhecido (`:648`). |
| Cartório | `aplicar_precedencia_na_celula` propaga antes de carimbar (`app/ponto/services/cartorio.py:1985`, `:1986`). `julgar_celula` captura, loga e retorna o julgamento anterior (`:2026`, `:2035`, `:2040`); precedência da ata não é atualizada nesse passo (`:1985`). |
| Férias | Captura silenciosamente e retorna `False` para descanso (`app/ferias/situacao.py:93`, `:96`); pode omitir o aviso de véspera de repouso (`:108`, `:110`). |
| Semana do posto | Captura silenciosamente e retorna **`trabalho`** (`app/escala/services/semana_do_posto.py:175`, `:179`); candidato previsto sem batida entra em **buracos**, antes explicado pela faixa `atestado` (`:151`, `:152`, `:157`). |
| Status do dia | Propaga nos pontos de alimentação (`app/ponto/status_dia.py:181`, `:298`, `:304`). |
| Folha sem furo | Propaga quando existem datas com lâmpadas apagadas (`app/folha/services/sem_furo.py:20`, `:25`). |
| Dia do colaborador | Propaga localmente (`app/ponto/services/dia_do_colab.py:15`). |
| Deslogados, faltas de hoje, abriu e não bateu | Propagam localmente (`app/ponto/services/deslogados.py:166`; `app/ponto/services/faltas_de_hoje.py:61`; `app/ponto/services/abriu_nao_bateu.py:123`). |
| Detector e reavaliação feriado/folga | Propagam localmente (`app/ponto/management/commands/detectar_ausencias.py:616`; `app/chamados/services/feriado_reavaliar.py:33`). |

Tradutores do veto capturam somente `VetoDePrecedencia`; `TipoSemFaixa` escapa desses tradutores sem ser rotulada sobreposição: `app/ponto/services/ausencia.py:382`, `app/ponto/services/declarar_dia.py:121`, `app/ferias/models.py:438`, `app/ferias/views.py:255`. Folgas delegam sem essa captura: `app/escala/folgas.py:310`, `:336`.

A entrega do contador usa a revisão/escritor existentes (`app/ponto/services/vigia_ausencia.py:259`, `:260`; `app/pautas/semanal.py:214`), comando existente (`app/ponto/management/commands/vigiar_familia_ausencia.py:23`, `:28`) e cron existente (`app/config/crons.py:739`). API direta publica inclusive as linhas zeradas e prefere a frase longa (`app/api/views_mensageria.py:1422`); outra API usa alias `_rev_dp` e transforma falha genérica de medição em erro explícito (`app/api/views_mensageria.py:2209`, `:2215`, `:2216`).

Há docstrings antigas dizendo “CINCO” no comando e API, embora o consumo seja genérico por linhas (`app/ponto/management/commands/vigiar_familia_ausencia.py:3`, `:16`, `:28`; `app/api/views_mensageria.py:1405`, `:1424`).

## 11/11 21:45 · sessao e61313ac5334 · - · - · origem=Ronald (tmux)

- **RED com mordida real:** `pk=5356`, tipo `folga`, aprovada, persiste uma `Ausencia` e chama `fatos_do_dia` sem mock; exige tipo/faixa `folga` e rótulo `Folga`, recusando `Atestado`. Não achei aprovação por motivo alheio à faixa nesse teste. (`app/ponto/tests/test_lastro_faixa_sem_default.py:92-106`; `app/ponto/precedencia.py:405-430`)

- **Limite desse RED:** a fixture não cadastra `TipoAusencia('folga')`, não cria escala/célula e não passa por calendário/UI ou pela porta de criação. `folga` também não integra a semeadura. Portanto prova o contrato da A2 para a linha histórica, mas não valida o cadastro ativo nem um conflito concreto com feriado/trabalho. (`app/ponto/tests/test_lastro_faixa_sem_default.py:66-106`; `app/ponto/migrations/0052_seed_tipoausencia.py:9-27`; `app/ponto/precedencia.py:298-303`)

- **Fantasma e contador têm mordida independente:** chamada real `faixa_de('fantasma_xyz')` exige exceção e código; outro teste cria tipo ativo real, exige zero `Ausencia`, contador igual a 1 e código nas frases; inativar exige zero. Não são provas sustentadas apenas por mock. (`app/ponto/tests/test_lastro_faixa_sem_default.py:13-17`; `app/ponto/tests/test_lastro_faixa_sem_default.py:108-121`)

- **Mutante default:** inferência pelo código: `_FAIXA_DE_TIPO.get(tipo, 'atestado')` faz falhar a recusa do fantasma e o contador real, porque este depende de capturar `TipoSemFaixa`; isso explica dois testes falhos se somente o módulo novo foi executado. O comando e a composição exata dessa execução permanecem **[nao verificado]**. (`app/ponto/tests/test_lastro_faixa_sem_default.py:16-17`; `app/ponto/tests/test_lastro_faixa_sem_default.py:114`; `app/ponto/services/vigia_ausencia.py:157-168`)

- **Mock com finalidade delimitada:** o teste de autoridade intercepta somente `abono`, já existente no mapa e no cadastro; exige que o contador chame a autoridade e observe sua recusa. Esse teste mata o mutante “contador lê o dict”, pela ausência da chamada e/ou pelo contador zero; isoladamente não provaria a recusa real, já coberta nos testes anteriores. (`app/ponto/tests/test_lastro_faixa_sem_default.py:132-148`)

- **Semeadura não fica inteiramente invisível:** o teste exige igualdade exata entre tipos ativos e `seed.TIPOS` antes de exigir zero; outro teste percorre todos os tipos da seed pela `faixa_de` real. A seed cria tipos com `ativo=True`; o teste de autoridade exige explicitamente a presença de `abono`. (`app/ponto/tests/test_lastro_faixa_sem_default.py:49-54`; `app/ponto/tests/test_lastro_faixa_sem_default.py:123-136`; `app/ponto/migrations/0052_seed_tipoausencia.py:30-37`)

- **Teste anterior não foi apagado:** o calendário agora exige a recusa nomeada para o tipo cadastrado `desconto_teste`; continua chamando o calendário real. O retorno anterior de falta/resumo não é mais verificado nesse caso, pois a leitura interrompe antes. (`app/colaboradores/tests/test_calendario_ausencia_pela_autoridade.py:43-60`)

- **Relógio novo solto: não achei:** classe DB inteira congelada e contador recebe `HOJE`; os testes simples não leem relógio. Existem relógios anteriores em `contadores` e `revisao_semanal`, acionados somente quando não recebem data; essas linhas não são acrescentadas pelo diff fornecido. (`app/ponto/tests/test_lastro_faixa_sem_default.py:62-64`; `app/ponto/tests/test_lastro_faixa_sem_default.py:87-90`; `app/ponto/services/vigia_ausencia.py:58`; `app/ponto/services/vigia_ausencia.py:258`)

- **Contador ligado à entrega existente:** aparece entre as oito linhas e entra nos `acesos`/`total_aceso`; revisão semanal entrega todas as linhas ao escritor semanal existente. O teste vizinho atualiza o número esperado para oito. Não achei segundo escritor introduzido por esse contador. (`app/ponto/services/vigia_ausencia.py:167-175`; `app/ponto/services/vigia_ausencia.py:228-233`; `app/ponto/services/vigia_ausencia.py:257-262`; `app/ponto/tests/test_a6_vigia_familia_ausencia.py:124-130`)

- **Achado B2 da reprova anterior:** há agora fixture explícita e testes que distinguem o comportamento curado; afirmar que toda a classe de fixture defeituosa foi eliminada exige o RED anterior e seu log, ausentes na cópia examinada **[nao verificado]**. Os testes atuais sustentam cura do caso, não uma prova universal sobre fixtures. (`app/ponto/tests/test_lastro_faixa_sem_default.py:66-106`)

## 11/11 21:19 · sessao 3c18a0449066 · PERGUNTA-DUPLA · construtor · origem=Code

Criado somente [test_pergunta_dupla_saida_noturna.py](/home/ronald/wt-codex-pergunta-dupla-211041/app/chamados/tests/test_pergunta_dupla_saida_noturna.py:1). O ensaio usa API real, relógio congelado por etapa, signals preservados e observador aditivo de nascimento; exige exatamente uma pergunta `orfao_14h`, não conversa, em D, incluindo linhas carimbadas na contagem (`chamados/tests/test_pergunta_dupla_saida_noturna.py:85,120,135,188,234`).

**Esperado no HEAD: VERDE.** Não encontrei produtor alcançável que entregue D+1 para essa saída ausente; não fabriquei contexto para obter RED. Execução Django e mordida do mutante: **[nao verificado]**, pois banco está vedado nesta construção. Sintaxe e compilação em memória passaram; isso não certifica comportamento (`chamados/tests/test_pergunta_dupla_saida_noturna.py:1`).

Censo dos escritores e da origem do dia:

| Escritor real | Valor e semântica |
|---|---|
| Detector `orfao_14h` | `data` vem de `resolver_data_evento` → juiz da batida: **D** (`chamados/detector_anomalias.py:41`; `escala/servico_jornada.py:508`). |
| Fábrica da célula | `contexto[motivo].data` recebe o dia da célula: **D** (`chamados/services/fabrica_da_celula.py:172`). |
| Pendentes adicionais | Consulta `dia_do_turno_da_batida`; grava contexto quando cria pergunta: **D** (`chamados/services/disputa_emissao.py:283,292,296`). |
| Alertas de turno | Grava `tm.data_turno` e timestamp da entrada; **D** vem do turno materializado (`ponto/management/commands/processar_alertas_turno.py:316,349`). |
| Alertas avançados | `ontem` é seleção pelo calendário, mas consulta trabalho, turno materializado e juiz da aberta antes de gravar: **D** neste cenário (`ponto/management/commands/processar_alertas_avancados.py:49,58,63,127`). |
| Detector de ausências/cartório | Grava o dia previsto da passada/célula e `horario_previsto`; não converte uma saída civil em dia do turno (`ponto/management/commands/detectar_ausencias.py:138,706`). |
| Retroativo de turno aberto | Grava `t.data_turno` e timestamp da entrada; **D**. É ato sem agenda de cron (`ponto/management/commands/emitir_turno_aberto_retroativo.py:88,128`; `config/crons.py:934`). |
| Reconciliador c2 | Copia o dia recebido do fio; não faz nova conversão pelo pareador. O chamador prioriza `p.data_evento` (`chamados/reconciliador.py:690,811,920,1567`). |
| Reconciliador de grade | Grava o dia da grade; cron está em sombra, sem `--apply` (`ponto/management/commands/reconciliar_grade.py:517`; `config/crons.py:462`). |
| Web: entrada bloqueada | `pendentes` contém dia **civil da entrada**, D para E às 19h. Exige outra tentativa de E bloqueada, ausente deste ensaio (`ponto/views.py:275,279,301`; `ponto/services/triagem_batida.py:180`). |

Os escritores civis encontrados não demonstram D+1 para a saída faltante:

- `data_batida` do detector descreve batida **existente** em folga ou fora da escala (`chamados/detector_anomalias.py:174,220`).
- A API grava `data_turno=localdate(batida.timestamp)` para desvio **leve**, categoria fora do mapa de regularização de saída (`api/views_core.py:710,717`; `chamados/models.py:3664`).
- `saida_sem_entrada` tem a data corrigida pelo juiz nas duas APIs e na triagem; cobra a entrada ausente e exige S existente (`api/views_core.py:741,747`; `api/views.py:747,753`; `ponto/services/triagem_batida.py:729,739`).
- Os contextos de justificativa copiam data informada ou `j.data_turno`; suas categorias não constam do mapa de saída (`api/views.py:1948,1968,1978`; `ponto/services/justificativa.py:297,302`; `chamados/models.py:3664`).

Quem escreve e quem lê os campos envolvidos:

| Campo | Escrita | Leitura |
|---|---|---|
| Pergunta: `motivo`, `conversa`, `data_evento` | `_nascer` recebe motivo/conversa; `fio.gravar_dia_do_fato` atribui a data; INSERT em `save` (`chamados/models.py:3014,3017`; `chamados/services/fio.py:800`). | Identidade/dedup, fábrica, fio e plantio (`chamados/models.py:2753`; `chamados/services/fabrica_da_celula.py:148`; `chamados/services/fio.py:1630`; `chamados/services/materializacao.py:280`). |
| `DisputaSupervisao.contexto[motivo].data` | Produtores acima; agregador persiste nascimento/merge (`chamados/services/disputa_emissao.py:559,562,590`). | Gerador, derivação do destino e reconciliador (`chamados/services/disputa_emissao.py:191,372`; `chamados/reconciliador.py:1560`). |
| `ChamadoColaborador.contexto_json`, cinco chaves | Produtores acima; atualização pela porta de contexto (`chamados/services/contexto.py:102`). | Parser, repasse sem origem e derivação do destino (`chamados/data_evento.py:16`; `chamados/services/disputa_emissao.py:1232,145`). |

Os dois sítios ainda preservam uma data recebida: leitura crua do contexto e parser sem origem; `dia_do_turno_da_celula` apenas declara o valor (`chamados/services/disputa_emissao.py:372,451,1232`; `ponto/turnos.py:2939`).

Comando para o Code:

```bash
PYTHONDONTWRITEBYTECODE=1 python manage.py test chamados.tests.test_pergunta_dupla_saida_noturna --settings=config.settings.ci
```

Mutante descrito, sem aplicação: em `chamados/services/disputa_emissao.py:451`, trocar `_dtc_g(_dia_p)` por `_dtc_g(_dia_p + datetime.timedelta(days=1))`. Executar o mesmo comando; o repasse conserva D, enquanto o gerador entrega D+1. O assert deve reprovar as duas chaves; mordida executada **[nao verificado]** (`chamados/tests/test_pergunta_dupla_saida_noturna.py:33,240`).

Texto para o RELATO: **cura do +1 presente nesta cópia** (`escala/servico_jornada.py:508`); estado efetivamente “no ar” **[nao verificado]**. Atribuição histórica dos pares e situação atual da batida 147824 continuam **[nao verificado]**, no passivo BATIDA/histórico, L-099. Certificação permanece com Code e opus final.

## 11/11 21:07 · sessao 4be47f121636 · BUG-ORFAO-COM-PAR · construtor · origem=Code

Arquivo: [test_bug_orfao_com_par_ensaio.py:1](/home/ronald/wt-codex-bug-orfao-com-par-210341/app/ponto/tests/test_bug_orfao_com_par_ensaio.py:1). Classe candidata nomeada com `RED`; ensaio pela API real, relógio por passo e captura da pilha de nascimento ([arquivo:84](/home/ronald/wt-codex-bug-orfao-com-par-210341/app/ponto/tests/test_bug_orfao_com_par_ensaio.py:84), [arquivo:122](/home/ronald/wt-codex-bug-orfao-com-par-210341/app/ponto/tests/test_bug_orfao_com_par_ensaio.py:122)).

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test ponto.tests.test_bug_orfao_com_par_ensaio --settings=config.settings.ci
```

Expectativa estática no HEAD:

| ASSERT | Esperado | Por quê |
|---|---|---|
| A, após E | Zero perguntas nascidas, inclusive carimbadas | Turno ainda em curso: `escala/servico_jornada.py:385`. |
| B, 06:30:48 | Zero nascimentos após cartório e reconciliação | Fábrica exige marco vencido; S vence às 07:30: `chamados/services/fabrica_da_celula.py:69`, `ponto/services/cartorio.py:882`. |
| C, após S | Um turno fechado ancorado em D; zero nascimentos acumulados | Seleção pelo dia do turno: `ponto/turnos.py:1992`. |

**Execução comportamental no HEAD: [nao verificado].** Não executei Django porque esta obra proíbe banco. Não afirmo RED nem “classe não reproduz no HEAD”; essa conclusão depende da suite. [nao verificado]

**Mutante descrito, sem aplicação:** emitir `orfao_14h` imediatamente após E, dispensando vencimento, em `api/views_core.py:758`, alcançando `chamados/services/disputa_emissao.py:444`. Executar o mesmo comando na cópia mutante; ASSERT A deve falhar. Morte efetiva: [nao verificado]. Especificação em [arquivo:20](/home/ronald/wt-codex-bug-orfao-com-par-210341/app/ponto/tests/test_bug_orfao_com_par_ensaio.py:20).

| Campo lido | Quem escreve | Quem lê em produção |
|---|---|---|
| Pergunta: `motivo` | `chamados/models.py:3014`, INSERT `:3017` | `chamados/juizes.py:941` |
| Pergunta: `data_evento` | `chamados/services/fio.py:800` | `chamados/services/fabrica_da_celula.py:148` |
| Pergunta: `materializada_em` | Carimbo `chamados/models.py:3099`; limpeza `:3208`, `:3266` | C6 `chamados/juizes.py:1370`; E2 `chamados/reconciliador.py:629` |
| Batida: `timestamp`, `tipo` | `ponto/registro_batida.py:157` | `ponto/turnos.py:1217` |
| Célula: `ata` | Cartório `ponto/services/cartorio.py:1837` → porta `ponto/portas/celula.py:1033` | `chamados/juizes.py:965` |

Certificação permanece com o Code e o opus final. [nao verificado]

## 11/11 21:04 · sessao ae9a66b9c290 · DATA-EVENTO-CRU · censo · origem=Code

Numeração dos anexos: primeira linha do conteúdo = 1, incluindo linhas vazias e excluindo `== anexo ==`. Em **b**, considero a triagem da própria resposta; descarte posterior pela sessão é **[nao verificado]**.

| Achado | Letra | Trecho q | Trecho r | Motivo em 1 linha |
|---|---|---|---|---|
| **1 — templates sem ponto** (`opus_final.r:3`) | **a** | “o regex de template deixa escapar algum leitor” (`contrato_catraca.q:1`) | Reconhece `with` e aponta `attr` como escape (`preverif_catraca.r:9`, `:11`) | A cobertura dos escapes foi perguntada; `regroup` e `dictsort` ficaram sem identificação. |
| **2 — dict de filtro em variável** (`opus_final.r:4`) | **a** | “Algum leitor real NOVO ficou de fora por regra nova (chave de dict literal como rotulo...)?” (`preverif_catraca_v2.q:1`) | “não localizei”; “As chaves literais encontradas são payload/trilha” (`preverif_catraca_v2.r:14`) | A exclusão de chaves foi expressamente questionada; o dict usado como filtro não foi identificado. |
| **3 — isenção ampla de return** (`opus_final.r:5`) | **e** | A v3 informa “sobrecontagem admitida” do retorno e pergunta sua estabilidade (`preverif_catraca_v3.q:2`, `:6`) | “`return 'data_evento'`, continua contado”; depois bloqueia a compensação (`preverif_catraca_v3.r:5`, `:19`, `:25`) | A pergunta examinava a sobrecontagem; a isenção defeituosa pertence à cura posterior, registrada na [volta 4](/home/ronald/codex_ws/29e201d0ca2c9a6534ece9880e4a989a629f9987/app/docs/RELATO.md:7968). |
| **4 — comentário multilinha** (`opus_final.r:6`) | **a** | “o regex de template deixa escapar algum leitor” (`contrato_catraca.q:1`) | Recomenda “retirar comentários antes de contar”, mencionando `{# ... #}` (`contrato_catraca.r:40`) | A revisão do regex estava no alcance; a resposta não distinguiu a semântica de comentário em uma linha da multilinha. |
| **5 — ESCRITA pelo nome da chamada** (`opus_final.r:7`) | **a** | “Algum leitor real NOVO ficou de fora por regra nova (... destino de escrita, DESTINO_KW)?” (`preverif_catraca_v2.q:1`) | “Leitor real novo omitido pelas exclusões novas: não localizei”; demonstra apenas a regressão do subscrito (`preverif_catraca_v2.r:14`) | As exclusões de escrita foram perguntadas; a isenção indistinta pelo nome do método não foi apontada. |
| **6 — total no ESTADO** (`opus_final.r:8`) | **e** | Pergunta “Quem escreve e quem le o JSON” e colisões com enumeradores (`preverif_catraca.q:7`) | “Publicação desse total no BACKLOG/ESTADO: **[nao verificado]**” (`preverif_catraca.r:36`) | Nenhuma q cobra explicitamente o produtor do total no ESTADO; a resposta menciona a publicação sem verificá-la. |
| **7 — código e JSON sobem juntos** (`opus_final.r:9`) | **b** | Pergunta se acrescentar leitor fica vermelho e se a catraca “so desce” (`preverif_catraca.q:3`) | “aumentar código **e** allowlist juntos pode passar; não há comparação com o baseline anterior” (`preverif_catraca.r:7`) | O Codex viu e tratou como limite, deixando-o fora da lista final de bloqueantes (`preverif_catraca.r:7`, `:40`). |

**Contagem da classificação acima:** **a = 4; b = 1; c = 0; d = 0; e = 2. Total = 7** (`opus_final.r:3–9`).

A promessa do achado 6 está em [RELATO.md:7950](/home/ronald/codex_ws/29e201d0ca2c9a6534ece9880e4a989a629f9987/app/docs/RELATO.md:7950) nesta cópia; [RELATO.md:7966](/home/ronald/codex_ws/29e201d0ca2c9a6534ece9880e4a989a629f9987/app/docs/RELATO.md:7966) registra a volta 3.

## 11/11 21:03 · sessao 8e89de2fb777 · - · - · origem=Ronald (tmux)

| Achado | Letra sugerida | Evidência |
|---|---|---|
| 1 | **a** | `contrato_catraca.q:1`: “o regex de template deixa escapar algum leitor”; `preverif_catraca.q:4`: “quais formas de ler data_evento o varredor nao ve”. A resposta enumera `with` e `attr`, sem `regroup`/`dictsort` (`preverif_catraca.r:9,11,20`). |
| 2 | **a** | `preverif_catraca_v2.q:1`: “Algum leitor real NOVO ficou de fora por regra nova (chave de dict literal como rotulo...)?”; resposta: “não localizei [...] As chaves literais encontradas são payload/trilha” (`preverif_catraca_v2.r:14`). A pergunta cobre a exclusão; a resposta não identifica o dict de filtro. |
| 3 | **e**, com distinção temporal | A última pergunta trata a **sobrecontagem** de `return 'data_evento'` (`preverif_catraca_v3.q:2,4,6`). O Codex encontra compensação e a bloqueia (`preverif_catraca_v3.r:19,27,29`). A isenção ampla de `return` do opus é defeito da cura posterior, sem pergunta de revisão correspondente nos anexos; a passagem v3→v4 registra “PROCEDE e curado” (`app/docs/RELATO.md:7968`). Não classificar **b**: o achado visto pelo Codex foi acolhido, não descartado (`app/docs/RELATO.md:7968`). |

Numeração considerada: primeira linha do conteúdo de cada anexo = 1; linhas vazias incluídas.

Para o achado 2, a resposta inicial já reconhecia escapes por `**` (`preverif_catraca.r:11,16`), mas não há trecho demonstrando descarte pela triagem do caso específico do dict literal guardado em variável **[nao verificado]**.

_4042 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
