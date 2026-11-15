# MAPA DOS RIOS — COMPETENCIA

## 1. RESUMO

1. A janela padrao e **21 a 20**, configuravel por empresa; o corte efetivo fica entre 2 e 28. (`app/ponto/janelas.py:10`, `app/ponto/janelas.py:36`)
2. Ha **3 status** em `FechamentoMensal`: `aberto`, `aprovado`, `exportado`; e **4 marcos independentes** da competencia: `aprovada`, `exportada`, `trancada`, `paga`. (`app/ponto/models.py:399`, `app/ponto/services/fechamento.py:1461`)
3. O marco exibido e o verdadeiro mais adiantado; sem nenhum, `aberta`. Portanto, as transicoes abaixo nao formam uma cadeia obrigatoria. (`app/ponto/services/fechamento.py:1532`, `app/ponto/services/fechamento.py:1543`)
4. O recalculo tem **10 chamadas diretas capazes de persistir**, enumeradas em N1; seu adaptador por evento recebe **5 chamadas diretas**. (`app/ponto/views.py:1255`, `app/ponto/services/fechamento.py:1810`, `app/ponto/registro_batida.py:178`; demais sitios em N1)
5. Aprovacao tem **3 nascentes diretas**; retorno de aprovado para aberto tem **3 escritores**. (`app/ponto/views.py:1365`, `app/ponto/views.py:1518`, `app/folha/views.py:252`, `app/ponto/services/fechamento.py:1344`, `app/ponto/services/fechamento.py:1349`, `app/ponto/services/fechamento.py:1449`)
6. TXT tem **2 nascentes de registro** e **1 de invalidacao**; registrar novamente cria outra versao, inclusive pela mesma view. (`app/folha/views.py:297`, `app/folha/management/commands/regerar_txt_dominio.py:118`, `app/folha/management/commands/regerar_txt_dominio.py:122`, `app/folha/models.py:182`)
7. Invalidacao de projecao tem **7 chamadas diretas**; restauracao acrescenta **5 chamadas em 4 commands**. (`app/ponto/services/fechamento.py:1656`; sitios completos em N2)
8. Existe **1 competencia explicitamente guardada**, `(9, 2026)`, cujo recalculo exclui empresas que ja exportaram. (`app/ponto/services/fechamento.py:36`, `app/ponto/services/fechamento.py:560`)
9. Campo de estado fora dos escritores identificados: **0 sitios confirmados**; existencia da linha: **2 exclusoes fora da porta**, em `bin/`. O selo cobre `app/`, por modulo. (`app/core/portas.py:500`, `app/folha/tests/test_chokepoint_folha_gate.py:26`, `bin/restore_relavratura_10_2026.py:83`, `bin/restore_o263_comp10.py:52`)
10. Estados terminais da **competencia inteira** demonstrados: **0**; resultados de execucao, volumes e ocorrencias reais de producao permanecem **[nao verificado]**. (`app/ponto/services/fechamento.py:1344`, `app/ponto/services/fechamento.py:1432`, `app/folha/models.py:182`)

## 2. TABELA DE TRANSICOES

A unidade abaixo e a mudanca na competencia; linhas de fechamento, tranca e versoes do TXT sao seus suportes persistidos. `derivado_em` e `previsto_em` aparecem como carimbos, sem inventar novos valores de `status`. (`app/ponto/services/fechamento.py:1518`, `app/ponto/services/fechamento.py:1679`, `app/ponto/services/fechamento.py:1607`)

**N1/N2**, referidos na tabela, enumeram as nascentes logo abaixo dela. Provas sao testes **lidos**, sem resultado de execucao **[nao verificado]**.

| # | estado de partida -> estado de chegada | juiz (arquivo:linha) | operacao (funcao) | escritor (arquivo:linha) | o que persiste (campos/modelos/trilha) | leitores (arquivo:linha) | prova (teste ou selo que morde, arquivo::nome, ou "nenhum") |
|---|---|---|---|---|---|---|---|
| 1 | Sem linha de fechamento -> linha `aberto` | Universo, escopo nomeado e guarda de competencia: `app/ponto/services/fechamento.py:554`, `app/ponto/services/fechamento.py:560`; modo leitura separado: `app/ponto/services/fechamento.py:891` | `recalcular_fechamento_mes`; nascentes N1 | `get_or_create`, `app/ponto/services/fechamento.py:898` | Chave colaborador/mes/ano, `status='aberto'`; depois numeros e carimbo da derivacao: `app/ponto/services/fechamento.py:900`, `app/ponto/services/fechamento.py:1143` | Classificacao do export: `app/folha/export.py:783`; marcos: `app/ponto/services/fechamento.py:1533` | `app/ponto/tests/test_o48p6_projecao_carimbada.py::test_RED_o_carimbo_nasce_no_ramo_ABERTO` (:92) |
| 2 | `aberto` -> `aprovado` nos carimbos da competencia | Lote: `classificar_export` e `trava_do_grupo`, `app/ponto/views.py:1299`, `app/ponto/views.py:1311`; individual: leitura atual e dispensa selecionada, `app/ponto/views.py:1483`, `app/ponto/views.py:1512`; TXT: crivo proprio do carimbo, `app/folha/views.py:244` | `aprovar_lote` / `aprovar`. Nascentes: `app/ponto/views.py:1365`, `app/ponto/views.py:1518`, `app/folha/views.py:252` | `app/ponto/services/fechamento.py:1329`, `app/ponto/services/fechamento.py:1337` | `status`, `aprovado_por`, `aprovado_em`; observacao opcional no lote. TXT deixa `aprovacao_por_geracao`: `app/ponto/services/fechamento.py:1329`, `app/folha/views.py:254` | Marco aprovada: `app/ponto/services/fechamento.py:1533`; guarda por status: `app/ponto/janelas.py:87` | `app/folha/tests/test_porta_folha.py::test_aprovar_lote_2x_grava_uma_vez` (:33); `::test_aprovacao_pelo_txt_nao_toca_a_observacao` (:42) |
| 3 | `aprovado` -> `aberto` nos carimbos da competencia | Vazios: classificacao e filtro aprovado, `app/ponto/views.py:1412`; individual: aprovado obrigatorio, `app/ponto/views.py:1450`; periodo: filtro aprovado, `app/ponto/services/fechamento.py:1451` | `reabrir_lote`, `reabrir`, cauda de `reabrir_periodo`. Nascentes: `app/ponto/views.py:1426`, `app/ponto/views.py:1455`, `app/ponto/views.py:1546` | `app/ponto/services/fechamento.py:1346`, `app/ponto/services/fechamento.py:1351`, `app/ponto/services/fechamento.py:1452` | `status='aberto'`, limpa autor/data de aprovacao; trilhas nas views: `app/ponto/views.py:1427`, `app/ponto/views.py:1456`, `app/ponto/views.py:1548` | `app/ponto/services/fechamento.py:1533`; `app/ponto/janelas.py:87` | `app/folha/tests/test_porta_folha.py::test_reabrir_lote_2x` (:49); `::test_aprovar_e_reabrir_um_colaborador_2x` (:58) |
| 4 | Sem linha de tranca -> `trancada` | Empresa informada e nenhum pulado na aprovacao: `app/ponto/views.py:1369`; criacao: `app/ponto/services/fechamento.py:1411` | `trancar_periodo`; nascente direta: `app/ponto/views.py:1370` | `app/ponto/services/fechamento.py:1408` | `PeriodoFechado`, empresa/mes/ano, autor, `ativo=True`; unicidade da chave: `app/ponto/models.py:949`. Gancho para fila somente no ato: `app/ponto/services/fechamento.py:1420` | `periodo_trancado`, `app/ponto/services/fechamento.py:1470`; `competencia_trancada`, `app/ponto/services/fechamento.py:1480` | `app/folha/tests/test_porta_folha.py::test_trancar_periodo_2x_e_uma_linha` (:74) |
| 5 | Tranca reaberta -> `trancada` novamente | Mesma nascente; porta exige `reaberto_em` preenchido para retrancar: `app/ponto/views.py:1370`, `app/ponto/services/fechamento.py:1412` | `trancar_periodo` | `app/ponto/services/fechamento.py:1413` | Renova autor/data do fechamento, `ativo=True`; limpa `reaberto_por/em/motivo`: `app/ponto/services/fechamento.py:1413` | `app/ponto/services/fechamento.py:1470`, `app/ponto/services/fechamento.py:1480` | nenhum especifico de retranca identificado **[nao verificado]** |
| 6 | `trancada` -> reaberta | POST/permissao, motivo nao vazio e tranca viva: `app/ponto/views.py:1525`, `app/ponto/views.py:1539`, `app/ponto/views.py:1543` | `reabrir_periodo`; nascente direta: `app/ponto/views.py:1546` | `app/ponto/services/fechamento.py:1437` | `ativo=False`, autor/data/motivo da reabertura; cauda executa #3. Trilha antes/depois na view: `app/ponto/views.py:1548`; gancho de fila: `app/ponto/services/fechamento.py:1447` | Verdade da tranca e `reaberto_em IS NULL`, nao `ativo`: `app/ponto/services/fechamento.py:1464` | `app/folha/tests/test_porta_folha.py::test_reabrir_periodo_devolve_os_aprovados_e_a_2a_nao_grava` (:81) |
| 7 | Sem TXT entregue valido -> `exportada`/entregue | View: selo, existencia, classificacao, nao vazio e layout: `app/folha/views.py:129`, `app/folha/views.py:163`, `app/folha/views.py:212`, `app/folha/views.py:270`, `app/folha/views.py:275`. Command: aplicar/autor/motivo e validacao: `app/folha/management/commands/regerar_txt_dominio.py:52`, `app/folha/management/commands/regerar_txt_dominio.py:85` | `ExportacaoDominio.registrar`; **2 nascentes**: `app/folha/views.py:297`, `app/folha/management/commands/regerar_txt_dominio.py:118` | `app/folha/models.py:182` | Empresa/competencia, autor/data, nome, conteudo, hash, total de linhas, `reexportacao`; trilha `export_txt` na view: `app/folha/models.py:183`, `app/folha/views.py:289` | `competencia_entregue`, `app/folha/export.py:588`; marco exportada, `app/ponto/services/fechamento.py:1537` | `app/folha/tests/test_k6_competencia_entregue.py::test_txt_vigente_entregue` (:65) |
| 8 | TXT vigente -> substituido; novo TXT -> vigente | Mesmas nascentes de #7; command compara hash antes de registrar: `app/folha/management/commands/regerar_txt_dominio.py:106`. Vigencia pelo maior `(gerado_em, pk)`: `app/folha/models.py:134`, `app/folha/models.py:220` | `registrar`; reexportacao pela view ou command: `app/folha/views.py:297`, `app/folha/management/commands/regerar_txt_dominio.py:118` | Novo registro: `app/folha/models.py:182`; substituicao anterior e derivada: `app/folha/models.py:140` | Nova versao completa; `reexportacao=True` se ja existia qualquer registro. A view nao escreve invalidacao do anterior: `app/folha/models.py:178`, `app/folha/models.py:186`, `app/folha/views.py:297` | Historico: `app/folha/views.py:541`; anotador: `app/folha/models.py:203` | `app/folha/tests/test_export_vigente.py::test_doze_registros_um_so_vigente` (:45); `app/folha/tests/test_sm1_monster_export.py::test_07_idempotencia_reexportacao` (:221) |
| 9 | TXT anterior nao invalidado -> invalidado com substituta | Command exige motivo; porta repete exigencia e recusa repetir o fato: `app/folha/management/commands/regerar_txt_dominio.py:53`, `app/folha/models.py:151`, `app/folha/models.py:154` | `invalidar`; unica nascente encontrada registra o novo antes: `app/folha/management/commands/regerar_txt_dominio.py:118`, `app/folha/management/commands/regerar_txt_dominio.py:122` | `app/folha/models.py:158` | `invalidada_em`, `invalidada_motivo`, `substituida_por`; evento `txt_invalidado`, antes/depois/ator; conteudo/hash preservados: `app/folha/models.py:162`, `app/folha/models.py:163` | `app/folha/export.py:591`; `app/folha/services/vigia_txt.py:220` | `app/folha/tests/test_txt_invalidado_com_trilha.py::test_RED_invalidar_grava_data_motivo_e_quem_substituiu` (:37); `::test_MORDE_invalidar_duas_vezes_e_UMA_trilha_e_UMA_data` (:70) |
| 10 | Projecao vencida (`derivado_em=NULL`) -> derivada | Universo, guarda e modo de escrita: `app/ponto/services/fechamento.py:554`, `app/ponto/services/fechamento.py:560`, `app/ponto/services/fechamento.py:1128`; regeneracao exportada acrescenta staff/motivo/intervalo: `app/ponto/portas/regen_exportada.py:107` | `recalcular_fechamento_mes`; mesmas nascentes N1; `regenerar_em_exportada` chama o recalculo: `app/ponto/portas/regen_exportada.py:184` | Ramo aberto: `app/ponto/services/fechamento.py:1143`; demais status: `app/ponto/services/fechamento.py:1160`, `app/ponto/services/fechamento.py:1189` | Numeros, `derivado_em`, atualizado; update preserva status/aprovacao. Regen acrescenta trilha com hashes/campos dos dois lados: `app/ponto/services/fechamento.py:1151`, `app/ponto/portas/regen_exportada.py:204` | `projecoes_vencidas_qs`, `app/ponto/services/fechamento.py:1692`; contador: `app/ponto/services/fechamento.py:1721` | `app/ponto/tests/test_o48p6_projecao_carimbada.py::test_RED_o_carimbo_nasce_tambem_no_ramo_APROVADO` (:100) |
| 11 | Projecao derivada -> vencida (`derivado_em=NULL`) | Invalidacao: competencias do fato e carimbo nao nulo, `app/ponto/services/fechamento.py:1649`. Restauracao: campos reais e motivo obrigatorio, `app/ponto/services/fechamento.py:1374`, `app/ponto/services/fechamento.py:1378` | `invalidar_fechamento` / `restaurar_fechamento`; nascentes N2 | `app/ponto/services/fechamento.py:1656`, `app/ponto/services/fechamento.py:1391` | Invalidacao zera carimbo e toca atualizado; restauracao tambem escreve os campos pedidos. Ambas usam logger: `app/ponto/services/fechamento.py:1657`, `app/ponto/services/fechamento.py:1395` | `app/ponto/services/fechamento.py:1696`, `app/ponto/services/fechamento.py:1721` | `app/ponto/tests/test_o48p6_projecao_carimbada.py::test_MORDE_invalidar_mata_o_carimbo_e_MOVE_o_contador` (:113); `::test_MORDE_restaurar_fechamento_mata_o_carimbo` (:165) |
| 12 | Previsto sem carimbo -> previsto materializado | Derivacao da grade concluida, dentro do recalculo: `app/ponto/services/fechamento.py:1083`; persistencia depende do modo: `app/ponto/services/fechamento.py:1128` | `recalcular_fechamento_mes`; N1 | Atribui: `app/ponto/services/fechamento.py:1086`; persiste: `app/ponto/services/fechamento.py:1144`, `app/ponto/services/fechamento.py:1188` | Valores previstos e `previsto_em`; nao e carimbo da derivacao inteira: `app/ponto/services/fechamento.py:1083`, `app/ponto/services/fechamento.py:1679` | Classificacao do fechamento: `app/folha/export.py:765` | nenhum especifico identificado **[nao verificado]** |
| 13 | Previsto materializado -> sem carimbo | Signal de escala ou falha da grade no recalculo: `app/escala/signals.py:137`, `app/ponto/services/fechamento.py:1087` | `invalidar_previsto`, chamado em `app/escala/signals.py:146`; ou `recalcular_fechamento_mes`, N1 | `app/ponto/services/fechamento.py:1613`; ramo da grade: `app/ponto/services/fechamento.py:1099`, persistido em `app/ponto/services/fechamento.py:1144`, `app/ponto/services/fechamento.py:1188` | Signal zera somente `previsto_em` e toca atualizado; falha da grade tambem torna seus valores `None`: `app/ponto/services/fechamento.py:1613`, `app/ponto/services/fechamento.py:1094` | Classificacao: `app/folha/export.py:765`; projecao vencida continua perguntando outro campo: `app/ponto/services/fechamento.py:1696` | `app/folha/tests/test_porta_folha.py::test_invalidar_previsto_2x` (:92) |
| 14 | Linha de fechamento existente -> ausente | Reversao com `DRY=False` e filtros do snapshot: `bin/restore_relavratura_10_2026.py:66`, `bin/restore_o263_comp10.py:37` | Exclusao nos dois scripts; nao passa por `apagar_em_ensaio`: `bin/restore_relavratura_10_2026.py:83`, `bin/restore_o263_comp10.py:52`; porta de ensaio: `app/ponto/services/fechamento.py:1764` | `bin/restore_relavratura_10_2026.py:83`, `bin/restore_o263_comp10.py:52` | Remove linha inteira. Pode retirar testemunho de aprovacao, cujo leitor usa `exists`; demais marcos sao independentes: `app/ponto/services/fechamento.py:1533` | `app/ponto/services/fechamento.py:1533`; classificacao: `app/folha/export.py:785` | nenhum para essas exclusoes identificado **[nao verificado]** |

### N1 — Nascentes de nascimento, recalculo e regeneracao

**10 chamadas diretas capazes de persistir fechamento**, contando sitios de chamada, sem contar dias ou colaboradores:

- View: `app/ponto/views.py:1255`.
- Porta de regeneracao exportada: `app/ponto/portas/regen_exportada.py:184`.
- Adaptador por evento: `app/ponto/services/fechamento.py:1810`.
- Commands: `app/ponto/management/commands/aplicar_09_corte_b.py:212`; `app/ponto/management/commands/celula_veredito_velho.py:149`; `app/ponto/management/commands/diff_reclassificar_partido.py:206`; `app/ponto/management/commands/recalcular_fechamento.py:118`.
- Scripts: `bin/aplicar_o263_comp10.py:51`; `bin/relavrar_o118_col221.py:53`; `bin/relavrar_o119_col221.py:52`.

Os tres primeiros commands condicionam o commit ao modo de aplicacao; o ultimo retorna antes do recalculo sem `--apply`. (`app/ponto/management/commands/aplicar_09_corte_b.py:274`, `app/ponto/management/commands/celula_veredito_velho.py:156`, `app/ponto/management/commands/diff_reclassificar_partido.py:213`, `app/ponto/management/commands/recalcular_fechamento.py:110`)

O command `aplicar_janela_he_total` acrescenta uma nascente indireta, delegando ao command de recalculo com `apply=True`. (`app/ponto/management/commands/aplicar_janela_he_total.py:145`)

**5 nascentes diretas do adaptador por evento**, que agenda o recalculo em `on_commit`: `app/ponto/registro_batida.py:178`; `app/ponto/portas/he.py:202`; `app/ponto/portas/celula.py:496`; `app/chamados/services/validacao.py:384`; `app/chamados/services/validacao.py:469`. O agendamento fica em `app/ponto/services/fechamento.py:1824`.

A regeneracao exportada tambem e alcancada pelo sitio `app/chamados/services/passivo_validado.py:444`, alimentado pelo command em `app/chamados/management/commands/materializar_passivo_validado.py:113`. A porta exige staff, motivo de pelo menos dez caracteres e intervalo valido; grava trilha antes/depois. (`app/ponto/portas/regen_exportada.py:111`, `app/ponto/portas/regen_exportada.py:115`, `app/ponto/portas/regen_exportada.py:118`, `app/ponto/portas/regen_exportada.py:204`)

**Chamadas fora de tests que escrevem somente em ensaio/rollback**, sem demonstrar transicao persistida em producao: `app/folha/management/commands/simular_folha.py:110`, `app/folha/management/commands/simular_folha.py:123`; `app/ponto/management/commands/plano_b_no_dinheiro.py:110`; `app/ponto/management/commands/folga_que_sumiu.py:70`; `app/ponto/management/commands/desvio_o68b.py:94`; `app/ponto/management/commands/diff_reclassificar_partido.py:161`; `bin/r5_idempotencia_frota.py:117`.

**Crons:** o catalogo declara recalculo e regeracao de TXT como atos, fora da rotina; o cron do TXT e vigia com `--alarme`. O gerador de celulas agendado chama regeneracao com `recalculo=False` nesse caminho. (`app/config/crons.py:1148`, `app/config/crons.py:1019`, `app/config/crons.py:1610`, `app/config/crons.py:426`, `app/ponto/management/commands/gerar_celulas.py:323`)

### N2 — Nascentes de invalidacao da projecao

**7 chamadas diretas de `invalidar_fechamento`:**

- `app/escala/services/cadastro_tipo.py:363`.
- `app/colaboradores/services/vinculo.py:959`.
- `app/ponto/views.py:1808`.
- `app/ponto/services/ausencia.py:587`.
- `app/ponto/services/ausencia.py:1013`.
- `app/ponto/services/justificativa.py:215`.
- `app/ponto/services/justificativa.py:390`.

**5 chamadas de `restaurar_fechamento`, em 4 commands:** `app/ponto/management/commands/aplicar_09_corte_b.py:220`, `app/ponto/management/commands/aplicar_09_corte_b.py:266`, `app/ponto/management/commands/aplicar_09_he_intra.py:149`, `app/ponto/management/commands/diff_reclassificar_partido.py:224`, `app/ponto/management/commands/desvio_o68b.py:197`.

A restauracao mata `derivado_em` por default; o chamador pode fornecer explicitamente outro valor desse campo. (`app/ponto/services/fechamento.py:1391`)

## 3. TRES LISTAS

### L1 TRANSICAO COM MAIS DE UM JUIZ — 10 grupos de transicoes

A lista inclui guardas complementares e caminhos divergentes; pluralidade de juiz nao significa, sozinha, contradicao. Os exemplos concretos sao a composicao da aprovacao e os caminhos diferentes de exportacao. (`app/ponto/views.py:1299`, `app/ponto/views.py:1311`, `app/folha/views.py:129`, `app/folha/management/commands/regerar_txt_dominio.py:106`)

1. **Nascimento e rederivacao (#1/#10/#12):** `universo_da_competencia`, filtro nomeado, guarda de competencia e modo de escrita decidem alcances distintos antes da persistencia. A regeneracao exportada acrescenta seus vetos proprios. (`app/ponto/services/fechamento.py:554`, `app/ponto/services/fechamento.py:556`, `app/ponto/services/fechamento.py:560`, `app/ponto/services/fechamento.py:1128`, `app/ponto/portas/regen_exportada.py:107`)

2. **Aprovacao (#2):** lote consulta o gravado; individual deriva leitura atual; TXT deriva leitura atual e aplica seu crivo de carimbo. Ha dispensas de `conta_vazia` no selecionado e no TXT. (`app/ponto/views.py:1299`, `app/ponto/views.py:1483`, `app/ponto/views.py:1509`, `app/folha/views.py:208`, `app/ponto/views.py:33`, `app/folha/export.py:762`)

3. **Retorno aprovado -> aberto (#3):** reavaliacao usa `conta_vazia`; individual exige status aprovado; reabertura do periodo alcança todos os aprovados da competencia/empresa. (`app/ponto/views.py:1412`, `app/ponto/views.py:1450`, `app/ponto/services/fechamento.py:1449`)

4. **Tranca inicial e retranca (#4/#5):** a view decide empresa/ausencia de pulados; a porta distingue criacao, retranca e repeticao sem novo ato. (`app/ponto/views.py:1369`, `app/ponto/services/fechamento.py:1411`, `app/ponto/services/fechamento.py:1412`)

5. **Reabertura do periodo (#6):** permissao, motivo e existencia de tranca sao decididos na view; o escritor recebe o objeto e grava. (`app/ponto/views.py:1527`, `app/ponto/views.py:1539`, `app/ponto/views.py:1543`, `app/ponto/services/fechamento.py:1437`)

6. **Entrega e nova versao do TXT (#7/#8):** a view consulta `conferir`, exige fechamento existente e pode aprovar; o command monta, valida, compara o hash e registra sem essas mesmas chamadas. (`app/folha/views.py:129`, `app/folha/views.py:163`, `app/folha/views.py:252`, `app/folha/management/commands/regerar_txt_dominio.py:85`, `app/folha/management/commands/regerar_txt_dominio.py:106`, `app/folha/management/commands/regerar_txt_dominio.py:118`)

7. **Invalidacao do TXT (#9):** command e modelo validam motivo; o modelo ainda decide idempotencia pela data ja preenchida. (`app/folha/management/commands/regerar_txt_dominio.py:53`, `app/folha/models.py:151`, `app/folha/models.py:154`)

8. **Invalidacao da projecao (#11):** `invalidar_fechamento` seleciona competencias do fato e carimbos preenchidos; `restaurar_fechamento` valida campos/motivo e derruba o carimbo por default. (`app/ponto/services/fechamento.py:1649`, `app/ponto/services/fechamento.py:1653`, `app/ponto/services/fechamento.py:1374`, `app/ponto/services/fechamento.py:1391`)

9. **Invalidacao do previsto (#13):** signal invalida depois de salvar escala; o recalculo pode produzir o mesmo `NULL` pela falha da grade. (`app/escala/signals.py:137`, `app/escala/signals.py:146`, `app/ponto/services/fechamento.py:1087`, `app/ponto/services/fechamento.py:1099`)

10. **Exclusao da linha (#14):** os dois scripts decidem por `DRY` e por conjuntos diferentes do snapshot, fora da guarda de banco de ensaio da porta declarada. (`bin/restore_relavratura_10_2026.py:66`, `bin/restore_relavratura_10_2026.py:83`, `bin/restore_o263_comp10.py:37`, `bin/restore_o263_comp10.py:52`, `app/ponto/services/fechamento.py:1766`)

### L2 ESCRITOR FORA DA PORTA — 0 atribuicoes de estado confirmadas; 2 desvios de existencia

- **Campo de estado:** nao foi confirmado sitio externo aos escritores identificados para `status`, carimbos de aprovacao/reabertura, `derivado_em`, `previsto_em` ou invalidacao de TXT. O selo existente aceita **modulos**, nao funcoes; portanto nao prova escritor unico por funcao. (`app/core/portas.py:500`, `app/core/portas.py:505`, `app/folha/tests/test_chokepoint_folha_gate.py:40`, `app/folha/tests/test_chokepoint_folha_gate.py:58`)

- **Desvio de existencia 1:** exclusao direta de fechamentos em `bin/restore_relavratura_10_2026.py:83`. O script tambem salva valores fora da porta, mas a lista importada nao inclui os campos de estado/carimbo. (`bin/restore_relavratura_10_2026.py:34`, `bin/restore_relavratura_10_2026.py:81`, `app/ponto/management/commands/aplicar_09_corte_b.py:67`)

- **Desvio de existencia 2:** exclusao direta de fechamentos em `bin/restore_o263_comp10.py:52`; mesma distincao para seu `save` de valores. (`bin/restore_o263_comp10.py:27`, `bin/restore_o263_comp10.py:50`, `app/ponto/management/commands/aplicar_09_corte_b.py:67`)

- **Capacidade nao comprovada em ocorrencia:** `restaurar_fechamento` declara nao tocar status, mas aceita qualquer campo real e exclui somente identificadores. `desvio_o68b` encaminha chaves de JSON externo; logo o codigo admite restauracao de estado por esse caminho. Conteudo efetivamente usado desse snapshot: **[nao verificado]**. (`app/ponto/services/fechamento.py:1370`, `app/ponto/services/fechamento.py:1374`, `app/ponto/services/fechamento.py:1380`, `app/ponto/management/commands/desvio_o68b.py:148`, `app/ponto/management/commands/desvio_o68b.py:196`)

### L3 ESTADO ALCANCAVEL SEM SAIDA — 0 terminais da competencia demonstrados

- Aprovacao tem reabertura; tranca tem reabertura; projecao vencida admite rederivacao; exportacao admite nova versao. (`app/ponto/services/fechamento.py:1344`, `app/ponto/services/fechamento.py:1432`, `app/ponto/services/fechamento.py:1143`, `app/folha/models.py:182`)

- `invalidado` e preservado no **registro historico do TXT**: a porta nao revalida a mesma linha. Isso nao encerra a competencia, que recebe outro registro; a nascente produtiva encontrada registra a substituta antes de invalidar o anterior. (`app/folha/models.py:154`, `app/folha/models.py:182`, `app/folha/management/commands/regerar_txt_dominio.py:118`, `app/folha/management/commands/regerar_txt_dominio.py:122`)

- `paga` e uma leitura cruzada de outra entidade, sem terminalidade demonstrada aqui. Sitios cruzados: publicacao em `app/holerite/management/commands/processar_lote_holerite.py:126`; substituicao de documentos em `app/holerite/views.py:1191`; leitor do marco em `app/ponto/services/fechamento.py:1539`.

- `exportado` existe no campo, mas nenhuma nascente literal de producao foi identificada nesta leitura; alcance real desse valor **[nao verificado]**. Nao foi promovido a terminal alcancavel. (`app/ponto/models.py:402`, `app/ponto/services/fechamento.py:1534`)

## 4. O QUE NAO CONSEGUI CONFIRMAR OU FICOU INCERTO

- **Identidade Git:** a copia nao ofereceu repositorio Git utilizavel na raiz consultada; correspondencia exata com o hash informado **[nao verificado]**.

- **Producao:** sem banco, logs ou sonda especifica desta entidade, nao ha contagem de transicoes executadas, competencias em cada marco, loops ou arquivos realmente importados no sistema externo. **[nao verificado]**

- **Execucao das provas:** testes foram lidos, nao executados; nenhum resultado verde e afirmado. **[nao verificado]**

- **Chamadas dinamicas e snapshots externos:** cobertura de chamadas construidas dinamicamente, shells externos e conteudo dos JSON de reversao **[nao verificado]**. O encaminhamento de campos externos existe em `app/ponto/management/commands/desvio_o68b.py:148` e `app/ponto/management/commands/desvio_o68b.py:196`.

- **Revogacao isolada de entrega:** a porta admite invalidar sem substituta, e o teste cobre esse estado; a unica nascente produtiva encontrada sempre fornece substituta. Ocorrencia real sem substituta **[nao verificado]**. (`app/folha/models.py:143`, `app/folha/tests/test_k6_competencia_entregue.py:72`, `app/folha/management/commands/regerar_txt_dominio.py:122`)

- **Vigente nao equivale sozinho a entregue:** a propriedade escolhe a versao mais recente; `competencia_entregue` ainda exige que ela nao esteja invalidada. Essa diferenca esta no codigo, sem medicao de ocorrencia aqui. (`app/folha/models.py:134`, `app/folha/export.py:591`)

- **Comentarios e comportamento divergem em sitios lidos:** ha comentarios citando cron de madrugada, enquanto o catalogo declara recalculo fora de cron; o command de rejulgamento tambem pode recalcular quando `sem_recalculo` e falso. A configuracao efetivamente instalada no host permanece **[nao verificado]**. (`app/ponto/services/fechamento.py:1786`, `app/config/crons.py:1148`, `app/ponto/management/commands/celula_veredito_velho.py:146`)

- **Script incompatível com a assinatura atual:** `bin/relavrar_o120_col221_09.py:74` passa `permitir_exportada`, ausente em `app/ponto/services/fechamento.py:448`. **ESPECULACAO:** essa chamada falha antes de entrar no escritor; nao foi executada.