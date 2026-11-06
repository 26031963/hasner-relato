# CODEX-ESTADO

> Ultima resposta processada: **06/11/2026 19:43** · publicado em **06/11/2026 19:44** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 06/11 19:43 · sessao 340537c22241 · ESMERIL-SEM-DIAS-PREVISTOS · censo · origem=Code

**O fechamento atual conta o rótulo da ata, sem exigir `CelulaDia.trabalha=True`. Há outro significado documentado — “todo dia que a célula diz trabalho” — cujo selo consta como não iniciado.** (`app/ponto/services/fechamento.py:1030-1033`; `app/docs/BACKLOG.md:1365`.)

Os dois valores gravados `15=8+7` e `15=9+6` são informação fornecida na pergunta; o anexo não contém a sonda diária que permita reproduzi-los ou demonstrar qual código os gravou. **[nao verificado]**

**1. Significados encontrados**

Há três contagens distintas com o mesmo nome:

- **`FechamentoMensal.dias_previstos`:** quantidade de linhas da grade cujo `tipo_dia` seja `trabalho` ou `ausencia`, inclusive com zero minutos previstos; não consulta `trabalha`. Se a grade falhar, ou todos os dias dentro do vínculo forem `sem_celula`, recebe `None`. (`app/ponto/services/fechamento.py:1027-1033`, `app/ponto/services/fechamento.py:1067-1083`.)
- **`causa_espelho['dias_previstos']`:** quantidade de linhas com `tipo_dia=='trabalho'`; exclui todas as ausências. Esse dict é materializado no fechamento e transportado para a tela. (`app/folha/causas.py:39-45`; `app/ponto/services/fechamento.py:1061-1064`; `app/ponto/services/fechamento_lista.py:120-122`.)
- **Proposta de escala `['dias_previstos']`:** quantidade de células `trabalha=True`, recortadas aos dias atribuídos ao vínculo analisado; não deriva do fechamento mensal. (`app/escala/services/propositor.py:484-488`, `app/escala/services/propositor.py:511`, `app/escala/services/propositor.py:559`.)

O rótulo do campo é “Dias de trabalho previstos”, mas modelo e migration não estabelecem o predicado da contagem. (`app/ponto/models.py:446`; `app/ponto/migrations/0044_fechamentomensal_minutos_previstos.py:21-25`.)

**2. Quem transforma folga em ausência e quem conserva a distinção**

A cadeia semântica é:

1. A cobertura de ausência expande os dias do período, sem filtro de trabalho/folga; para folha, a cobertura exige ausência aprovada. (`app/ponto/turnos.py:2514`, `app/ponto/turnos.py:2753-2766`.)
2. O builder monta a folga com `minutos_previstos=0`; depois aplica o overlay de ausências. (`app/escala/utils.py:1822-1823`, `app/escala/utils.py:1865-1869`.)
3. **`_marcar_ausencias` é o escritor semântico do rótulo:** aceita cobertura sobre `folga` quando não é `falta` e escreve `tipo_dia='ausencia'`. Esse trecho não aumenta os minutos previstos. (`app/escala/utils.py:1231-1244`.)
4. O cartório monta essa grade, copia `tipo_dia` e minutos para `ata_do_dia`, e entrega a ata à lavra nos ramos normal e `nunca_bateu`. (`app/ponto/services/cartorio.py:1126`, `app/ponto/services/cartorio.py:704-706`, `app/ponto/services/cartorio.py:1210`, `app/ponto/services/cartorio.py:1242-1247`, `app/ponto/services/cartorio.py:1800-1807`.)
5. A escrita efetiva de `cel.ata` pertence à porta `lavrar_veredito`. (`app/ponto/portas/celula.py:986-1003`, `app/ponto/portas/celula.py:1028-1035`.)

O juiz de precedência põe ausência acima de folga, mas conserva separadamente `era_dia_previsto`, obtido de `cel.trabalha` quando preenchido. Seu resultado é gravado em **`ata.precedencia`**; esse caminho não é a atribuição direta de **`ata.tipo_dia`**, feita pelo builder/cartório descrito acima. (`app/ponto/precedencia.py:46`, `app/ponto/precedencia.py:347-363`, `app/ponto/precedencia.py:379-407`; `app/ponto/services/cartorio.py:1947-1956`.)

**Sim, há leitores que distinguem as duas situações:**

- `grade_da_celula` preserva o tipo da ata e seus minutos. Se os minutos estiverem ausentes, só consulta o previsto quando `cel.trabalha` for verdadeiro; caso contrário, usa zero. Portanto, **não revalida minutos já lavrados contra `trabalha=False`**. (`app/escala/services/leitor_celula.py:380-405`.)
- O juiz de minutos retorna zero quando `eh_dia_trabalho(...) is not True`; esse método lê `cel.trabalha` quando existe célula. (`app/escala/utils.py:2281-2288`; `app/escala/models.py:1087-1096`.)
- A grade transporta explicitamente o par `trabalha + ata`; `dia_sem_trabalho` consulta `trabalha is False`. Na ausência parcial, trabalho permite cobrar o restante e folga cala a cobrança. (`app/escala/services/leitor_celula.py:408-411`; `app/chamados/juizes.py:422-428`.)
- `por_dia_da_grade` inclui `trabalho/ausencia`, mas calcula valores por **minutos**: previsto do dia, realizado limitado ao previsto e abono da parte descoberta. Uma ausência com previsto zero contribui zero nessas somas, para realizado não negativo. (`app/ponto/services/dia_pago.py:384-400`.)

**3. Dinheiro: efeito de mudar somente a contagem**

**Mudar apenas o cálculo de `_dias_prev` não altera DSR, falta, abono, banco ou eventos monetários do TXT pelos caminhos encontrados.** Esses cálculos usam outras datas e minutos; a leitura funcional direta de `FechamentoMensal.dias_previstos` está no teste de zero da prontidão. (`app/ponto/services/fechamento.py:722-731`, `app/ponto/services/fechamento.py:1032-1048`; `app/folha/export.py:1042-1043`.)

- **DSR e semanas OK/perdidas:** recebem `datas_previstas_trabalho`, construídas por `esc.eh_dia_trabalho` e exclusão das datas suprimidas. O motor avalia dias obrigatórios sem trabalho e sem justificativa; não usa a cardinalidade `trabalho+ausencia` da grade. (`app/ponto/services/fechamento.py:674-692`, `app/ponto/services/fechamento.py:697-731`; `app/ponto/motor_calculo_v2.py:2593-2630`, `app/ponto/motor_calculo_v2.py:2655`, `app/ponto/motor_calculo_v2.py:3452-3479`.)
- **Reflexo DSR:** usa HE semanal, dias efetivamente trabalhados e dias do período. (`app/ponto/motor_calculo_v2.py:2636-2653`, `app/ponto/motor_calculo_v2.py:3463-3477`.)
- **Desconto de falta:** o `horas_falta` final do fechamento soma `minutos_de_falta_por_dia`; o export expande e rateia ausências que descontam. (`app/ponto/services/fechamento.py:864-866`, `app/ponto/services/fechamento.py:987`; `app/folha/export.py:114-143`, `app/folha/export.py:169-185`.)
- **Proporcionalidade do banco:** já exige `eh_dia_trabalho(...) is True`, contando previstos da semana e a parcela dentro do período. (`app/ponto/motor_calculo_v2.py:3425-3429`.)
- **Prontidão:** redução de uma contagem positiva para outra positiva não muda percentual; o percentual usa minutos. Se a nova contagem chegar a zero com minutos positivos, muda para `cego=True`. O fallback repete `trabalho+ausencia` e também usa a contagem apenas no teste zero. (`app/folha/export.py:1042-1072`, `app/folha/export.py:1087-1105`.)
- **Tela:** faixa e ordenação consomem prontidão; o tique de entrada na folha vem separadamente do juiz do export. (`app/ponto/services/fechamento_lista.py:85-102`, `app/ponto/services/fechamento_lista.py:111-130`.)
- **TXT/PDF do export:** os eventos são enumerados sem `dias_previstos`; faltas seguem suas datas próprias. O PDF traduz o conteúdo do TXT gravado. (`app/folha/export.py:439-461`, `app/folha/export.py:722-736`; `app/folha/pdf_export.py:63-78`.)

**Alterar o filtro `_dt_trab` inteiro tem alcance maior que alterar somente a contagem:** ele também alimenta `minutos_previstos` e `dias_incertos`; os mapas de `DiaPago` possuem filtro próprio. Assim, a primeira mudança pode alterar o fechamento sem alterar os mapas diários correspondentes. (`app/ponto/services/fechamento.py:1030-1048`; `app/ponto/services/dia_pago.py:385-399`.)

Para dias retirados com previsto zero, a soma dos previstos permanece igual; `dias_incertos` pode cair se houver `sem_ancora`. Exigir estritamente `trabalha=True` também exclui `trabalha=None`, além das folgas. Quantidades e impacto nos dois colaboradores reais: **[nao verificado]**, pois faltam as linhas diárias no dado fornecido. (`app/ponto/services/fechamento.py:1032-1034`; `app/escala/models.py:1070-1072`.)

**4. Histórico e testes** [nao verificado]

`git log -S` não pôde ser consultado: o comando retornou `fatal: not a git repository`. Commit e data de introdução efetiva: **[nao verificado]**. O ambiente é documentado como cópia por `git archive`. (`CLAUDE.md:341`; `bin/codex_pergunta.sh:26`.)

Há duas pistas documentais, que **não substituem o histórico Git**:

- O comentário do fechamento atribui a **03/08** a inclusão de ausências para evitar encolher o denominador da prontidão. (`app/ponto/services/fechamento.py:999-1003`.)
- O overlay registra **13/09** para ausência vencer folga; existe teste dessa classificação. A combinação desse overlay com o filtro atual explica mecanicamente a inclusão da ausência em folga na contagem. (`app/escala/utils.py:1227-1241`; `app/escala/tests/test_grade_segue_a_precedencia.py:43-46`; `app/ponto/services/fechamento.py:1030-1033`.)

O documento O155 estabelece o significado baseado na célula e registra o selo como não iniciado; CORTES também registra o selo não construído. (`app/docs/BACKLOG.md:1365`; `app/docs/CORTES.md:200`.)

Referências de testes usadas na tabela abaixo — execução nesta auditoria: **[nao verificado]**: [nao verificado]

- **T1:** `app/escala/tests/test_grade_segue_a_precedencia.py::GradeSegueAPrecedenciaTest.test_MORDE_atestado_aprovado_em_dia_de_folga_e_ausencia`, fixa o rótulo ausência; `::test_falta_aprovada_em_dia_de_folga_segue_folga`, fixa a exceção da falta. (`app/escala/tests/test_grade_segue_a_precedencia.py:43-50`.)
- **T2:** `app/escala/tests/test_leitor_celula_minutos_pela_autoridade.py::LeitorCelulaMinutosPelaAutoridadeTest.test_celula_de_folga_preve_zero` e `::test_minutos_lavrados_na_ata_vencem`, fixam fallback zero e prevalência dos minutos lavrados. (`app/escala/tests/test_leitor_celula_minutos_pela_autoridade.py:45-51`.)
- **T3:** `app/ponto/tests/test_o48_sem_celula_nao_e_zero.py::SemCelulaNaoEZero.test_MORDE_sem_nenhuma_celula_o_objeto_nao_diz_zero_minuto_de_quem_trabalhou`, `::test_MORDE_admitido_no_meio_sem_celula_no_vinculo_tambem_nao_diz_zero` e `::test_fora_do_vinculo_INTEIRO_e_RESPOSTA_zero_nunca_silencio`, fixam `None` versus zero, sem resolver ausência em folga. (`app/ponto/tests/test_o48_sem_celula_nao_e_zero.py:96-116`, `app/ponto/tests/test_o48_sem_celula_nao_e_zero.py:153-207`.)
- **T4:** `app/ponto/tests/test_retrato_exportada.py::PorDiaDaGradeTest.test_MORDE_as_tres_leis`, fixa mapas de minutos por tipo, sem fornecer `trabalha`. (`app/ponto/tests/test_retrato_exportada.py:46-58`.)
- **T5:** `app/ponto/tests/test_fechamento_ausencia.py::test_atestado_semana_nao_perde_dsr` e `::test_afastamento_suprime_previsto`, fixam efeitos de ausência no DSR. (`app/ponto/tests/test_fechamento_ausencia.py:126-164`.)
- **T6:** `app/ponto/tests/test_o208_contador_do_recalculo.py::ContadorDoRecalculoTest.test_MORDE_os_cinco_campos_da_divergencia`, fixa visibilidade da mudança do campo, sem fixar sua derivação. (`app/ponto/tests/test_o208_contador_do_recalculo.py:30-35`, `app/ponto/tests/test_o208_contador_do_recalculo.py:71`.)
- **T7:** `app/folha/tests/test_porta_do_export.py::test_MORDE_o_contador_do_fechamento_NAO_e_cego_a_campo_de_pagamento`, exclui contagens do censo de pagamento. (`app/folha/tests/test_porta_do_export.py:454-481`.)

Teste que confronte diretamente o valor mensal de `dias_previstos` para `ausencia/trabalha=False` contra `ausencia/trabalha=True`: **[nao verificado: não localizado]**. [nao verificado]

Na tabela, “snapshot” significa que o sítio transporta/restaura o número recebido sem decidir o que constitui dia previsto; o comportamento está nos trechos citados em cada linha.

| Sítio | Escritor/leitor | Significado suposto | Dinheiro? | Teste que fixa | [nao verificado]
|---|---|---|---|---|
| `app/ponto/models.py:446`; `app/ponto/migrations/0044_fechamentomensal_minutos_previstos.py:21-25` | Declaração do campo | “Dias de trabalho previstos”; aceita `NULL` | Sem fórmula | T3 fixa parte da nulidade |
| `app/escala/models.py:1087-1096` | Alimentador: `eh_dia_trabalho` | Célula; sem ela, cálculo do vínculo | Alimenta outros cálculos financeiros | Não fixa a contagem mensal `[nao verificado]` |
| `app/escala/utils.py:1822-1831`, `app/escala/utils.py:1865-1869`, `app/escala/utils.py:1231-1244` | Escritor da grade julgada | Folga conserva previsto zero; ausência não-falta vence seu rótulo | Alimenta minutos | T1 |
| `app/ponto/precedencia.py:347-407`; `app/ponto/services/cartorio.py:1947-1956` | Juiz e lavra de `ata.precedencia` | Ausência vence folga; conserva `era_dia_previsto` separado | Não usa a contagem mensal | `app/ponto/tests/test_a2_precedencia.py::FolgaDeTemplateSuprimidaTest.test_MORDE_a_ata_da_celula_diz_que_a_folga_foi_suprimida` (`:466-481`) |
| `app/ponto/services/cartorio.py:704-706`, `app/ponto/services/cartorio.py:1242-1247`, `app/ponto/services/cartorio.py:1800-1807`; `app/ponto/portas/celula.py:1028-1035` | Escritor da ata | Copia tipo e minutos julgados | Alimenta grade/minutos | T1 fixa a classificação de origem |
| `app/escala/services/leitor_celula.py:380-411`; `app/folha/export.py:329-338` | Leitor da ata / grade do fechamento | Tipo da ata vence; minutos lavrados vencem; fallback considera `trabalha` | Alimenta previsto e abono | T2 |
| `app/ponto/services/fechamento.py:1027-1069` | **Escritor semântico de FM** | **Número de linhas `trabalho+ausencia`**, sem exigir `trabalha=True` | Contagem; somas vizinhas são minutos | T3; caso central não localizado `[nao verificado]` |
| `app/ponto/services/fechamento.py:1071-1083` | Escritor de ausência de resposta | `None` por falha/grade sem célula | Também anula minutos | T3 |
| `app/ponto/services/fechamento.py:1121-1128`, `app/ponto/services/fechamento.py:1144-1173` | Persistência por `save`/`update` | Valor derivado acima | Persiste também rubricas | Sem teste semântico específico `[nao verificado]` |
| `app/ponto/services/fechamento.py:881-886`, `app/ponto/services/fechamento.py:1118-1120`, `app/ponto/services/fechamento.py:1743-1745` | Leitura destacada / dict / `fechamento_lido` | Mesma derivação; copia a linha e retorna objeto ou `CAMPOS` | Transporta rubricas também | T3 |
| `app/ponto/services/fechamento.py:1319-1324` | Escrita integral ao aprovar | Preserva o valor já carregado | Carimbo; não recalcula | `[nao verificado]` |
| `app/ponto/services/fechamento.py:1342-1381` | **Escritor genérico de restauração** | Snapshot recebido; aceita `dias_previstos` | Pode restaurar campos financeiros | `[nao verificado]` |
| `bin/restore_relavratura_10_2026.py:34`, `bin/restore_relavratura_10_2026.py:79-81`; `bin/restore_o263_comp10.py:27`, `bin/restore_o263_comp10.py:48-50` | **Escritores diretos adicionais**, por `setattr/save` | Snapshot; não passam por `restaurar_fechamento` | Restauram rubricas também | `[nao verificado]` |
| `app/folha/export.py:1037-1072` | Leitor direto: prontidão | Contagem diferente de zero habilita denominador em minutos | Indicador; não rubrica | Sem caso central `[nao verificado]` |
| `app/folha/export.py:1087-1105` | Escritor/leitor local: fallback `ndias` | Repete `trabalho+ausencia`; testa zero | Indicador em minutos | Sem caso central `[nao verificado]` |
| `app/ponto/services/fechamento_lista.py:85-130` | Tela, faixa e ordenação | Consome prontidão; transporta causa | Apresentação | Não fixa o significado `[nao verificado]` |
| `app/ponto/services/dia_pago.py:384-400`, `app/ponto/services/dia_pago.py:343-358` | Deriva e lavra mapas diários | Mesmo universo `trabalho+ausencia`, mas **minutos**, sem `dias_previstos` | **Sim:** previsto/realizado/abono | T4; T7 exclui contagens |
| `app/ponto/services/dia_pago.py:99-100`, `app/ponto/services/dia_pago.py:403-424` | Retrato da exportada | Transporta três campos de minutos; contagem não integra `DiaPago` | **Sim**, pelos minutos | T4/T7 |
| `app/folha/causas.py:32-45`; `app/ponto/services/fechamento.py:1061-1064` | Escritor do dict homônimo | **Só `tipo_dia=='trabalho'`**; zero sem grade/isento | Diagnóstico | Assert específico da chave `[nao verificado]` |
| `app/escala/services/propositor.py:484-488`, `app/escala/services/propositor.py:511`, `app/escala/services/propositor.py:559` | Escritor do dict homônimo | **Células `trabalha=True`**, dentro dos dias do vínculo | Diagnóstico/proposta | Assert específico da chave `[nao verificado]` |
| `app/ponto/services/fechamento.py:697-731`; `app/ponto/motor_calculo_v2.py:2593-2655`, `app/ponto/motor_calculo_v2.py:3452-3479` | Outro conjunto de previstos / leitor DSR | Dias obrigatórios por célula/vínculo, descontadas supressões | **Sim**, independente da contagem FM | T5 |
| `app/ponto/motor_calculo_v2.py:3425-3429` | Proporcionalidade | `eh_dia_trabalho is True`, semana/período | **Sim**, banco | Teste específico desta pergunta `[nao verificado]` |
| `app/folha/export.py:114-143`, `app/folha/export.py:169-185`; `app/ponto/services/fechamento.py:864-866` | Falta decidida/rateio | Dias e minutos da ausência que desconta | **Sim**, independente da contagem FM | Não fixa `dias_previstos` `[nao verificado]` |
| `app/folha/export.py:439-461`, `app/folha/export.py:722-736`; `app/folha/pdf_export.py:63-78` | TXT e PDF do export | Eventos/datas próprias; PDF lê TXT | **Sim**, sem usar essa contagem | `app/folha/tests/test_export_dsr_reflexo.py::test_dsr_nao_e_emitido` (`:32-36`) |
| `app/ponto/management/commands/aplicar_09_corte_b.py:67-74`, `app/ponto/management/commands/aplicar_09_corte_b.py:184-185`, `app/ponto/management/commands/aplicar_09_corte_b.py:220`, `app/ponto/management/commands/aplicar_09_corte_b.py:266` | Snapshot/diff/restauração | Valor de `CAMPOS`, sem interpretação | Controle/restauração de vários campos | T6 cobre visibilidade |
| `app/ponto/management/commands/aplicar_09_he_intra.py:43-49`, `app/ponto/management/commands/aplicar_09_he_intra.py:125-150` | Leitor de snapshot | Preserva número; `dias_previstos` fica fora do alvo escrito | Controle | `[nao verificado]` |
| `app/ponto/management/commands/recalcular_fechamento.py:34-71`, `app/ponto/management/commands/recalcular_fechamento.py:142-148`; `app/ponto/services/diff_frota.py:62-63`, `app/ponto/services/diff_frota.py:84-88`, `app/ponto/services/diff_frota.py:111-125` | Foto/diff genéricos | Número recebido | Medição | T6 |
| `app/ponto/management/commands/diff_janela_he_total.py:87-115`; `app/ponto/management/commands/diff_passivo_s84.py:42-50`, `app/ponto/management/commands/diff_passivo_s84.py:87-88`; `app/ponto/management/commands/diff_col900.py:38-45`, `app/ponto/management/commands/diff_col900.py:74-78` | Leitores por `CAMPOS`/diff | Snapshot | Medição | `[nao verificado]` |
| `app/ponto/management/commands/carimbo_gravado.py:34-56`, `app/ponto/management/commands/carimbo_gravado.py:87-95`; `app/ponto/management/commands/aplicar_janela_he_total.py:65-88` | Foto e comparação indireta | Snapshot | Auditoria | `[nao verificado]` |
| `app/ponto/management/commands/lavrar_dias_pagos.py:68-78`; `app/ponto/portas/regen_exportada.py:56-84` | Hash e valores do gravado | Incluem o campo dinamicamente | Auditoria/Pauta/retificação | `[nao verificado]` |
| `app/ponto/management/commands/e6_piso_autopsia.py:120-124` | Impressão direta | Valor gravado | Diagnóstico | `[nao verificado]` |
| `bin/snapshot_relavratura_10_2026.py:29-31`; `bin/snapshot_o263_comp10.py:54-58`; `bin/aplicar_o263_comp10.py:28-37`, `bin/aplicar_o263_comp10.py:51-65` | Snapshot/recalculo/comparação | Valor gravado ou derivado pela porta | Medição de vários campos | `[nao verificado]` |
| `bin/diff_o118_col221.py:23-38`; `bin/diff_o119_frota.py:27-50`; `bin/diff_o124_frota.py:31-42`, `bin/diff_o124_frota.py:55-60` | Diffs genéricos | Snapshot, sem interpretação | Medição | `[nao verificado]` |
| `bin/relavrar_o118_col221.py:26-37`, `bin/relavrar_o118_col221.py:49-58`; `bin/relavrar_o119_col221.py:25-36`, `bin/relavrar_o119_col221.py:48-57`; `bin/relavrar_o120_col221_09.py:29-49`, `bin/relavrar_o120_col221_09.py:62-79` | Snapshot/hash/impressão; acionam recálculo | Preservam ou leem derivação canônica | Operação sobre fechamento completo | `[nao verificado]` |
| `app/relatorios/test_espelho_falta.py:28-41` | Helper de teste `_dias_previstos` | `ec.eh_dia_trabalho` | Fixture de falta/abono | `::EspelhoFaltaAbonoTest.test_falta_e_abono_surfam_no_resumo` verifica falta/abono

[truncado em 20000 de 20663 caracteres]

## 06/11 19:43 · sessao 92a8abebbd5b · ESMERIL-SEM-INSTANTE · censo · origem=Code

O filtro de fatias corta o **timestamp cru**; o motor, depois, recorta períodos pelo **dia da jornada**. São critérios diferentes (`app/ponto/services/fechamento.py:245`, `app/ponto/services/fechamento.py:252`, `app/ponto/motor_calculo_v2.py:2403`, `app/ponto/motor_calculo_v2.py:2429`).

A correspondência da árvore com o hash informado permanece **[nao verificado]**: o snapshot disponível não contém metadados Git.

**1. Quem escreve `_meus`, `fatia_ini` e `fatia_fim`**

| Variável | Escritor e origem |
|---|---|
| `fatia_ini`, inicialmente | `max(ini, esc.data_inicio, DATA_INICIO_OPERACAO)` — `app/ponto/services/fechamento.py:204`. |
| `fatia_fim`, inicialmente | `min(fim, esc.data_fim if esc.data_fim else fim)` — `app/ponto/services/fechamento.py:205`. |
| `_dono_do_dia` | Percorre as chaves `(colaborador_id, data)` de `celulas`; quando existe geradora e `vinculo_do_dia` responde, associa essa data ao vínculo resolvido — `app/ponto/services/fechamento.py:189-193`. |
| `_meus`, inicialmente | Datas de `_dono_do_dia` pertencentes a `esc.pk`, dentro dos limites iniciais — `app/ponto/services/fechamento.py:207-208`. |
| `_meus`, complemento | Acrescenta dias sem dono cobertos por exatamente um vínculo, quando há `_julgados` — `app/ponto/services/fechamento.py:221-231`. |
| Limites finais | Havendo `_julgados`, pula a fatia sem `_meus`; caso contrário, escreve `fatia_ini, fatia_fim = _meus[0], _meus[-1]` — `app/ponto/services/fechamento.py:232-236`. |
| Limites de busca | Derivados dos limites finais por −1/+1 dia — `app/ponto/services/fechamento.py:242-243`. |

O significado das fontes é:

- **Célula:** a chave vem de `CelulaDia.data`, um `DateField`; o carregador monta `{(c.colaborador_id, c.data): c}` (`app/escala/models.py:1685`, `app/escala/alimentacao.py:73-78`). Representa o **dia nominal do turno previsto, ancorado no calendário local**: o gerador declara instantes locais e coloca os marcos após meia-noite em D+1, mantendo a data nominal D (`app/ponto/management/commands/gerar_celulas.py:36-40`, `app/ponto/management/commands/gerar_celulas.py:67-69`).
- **Vigência:** `esc.data_inicio` e `esc.data_fim` são `DateField`, datas cadastrais comparadas ao dia da célula, sem conversão de instante; a geradora fora da vigência é recusada (`app/escala/models.py:875-876`, `app/escala/alimentacao.py:269-284`). A interpretação é de calendário cadastral/turno, não de dia UTC — inferência dessas comparações.
- **Piso operacional:** `datetime.date(2026, 5, 21)`, uma data fixa, sem instante ou fuso (`app/core/constants.py:5`).
- **`ini/fim` do chamador:** são limites de calendário, com as origens abaixo.

| Chamador | Origem de `ini/fim` |
|---|---|
| Fechamento | `janela_fechamento(mes, ano, empresa)`; constrói início/fim aware locais e passa `inicio.date(), fim.date()` (`app/ponto/services/fechamento.py:612-614`, `app/ponto/services/fechamento.py:745`). A janela nasce de `datetime.date` pelo corte da empresa (`app/ponto/janelas.py:15-21`, `app/ponto/janelas.py:54-57`). |
| Espelho | `autoridade_do_periodo` recebe e repassa `ini/fim` (`app/ponto/services/espelho.py:971`, `app/ponto/services/espelho.py:1164`). Na tela, recebe `min(piso_visual, apur_ini)` até `hoje` (`app/ponto/services/espelho.py:1225-1227`); `hoje` é data civil local e a competência vem de `janela_fechamento` (`app/ponto/views.py:97-98`, `app/ponto/views.py:153-158`). |
| PDF | Período livre: datas recebidas do chamador (`app/relatorios/pdf_espelho.py:1038-1046`). Mensal: janela da competência (`app/relatorios/pdf_espelho.py:661-663`, `app/relatorios/cartao_pela_celula.py:21-24`). Antes de apurar, limita o fim a ontem local, preservando o piso `data_ini` (`app/relatorios/pdf_espelho.py:285-288`, `app/relatorios/cartao_pela_celula.py:37-41`). |

**2. Quem escreve as listas e qual o `tzinfo`**

| Chamador | Construção da lista passada à apuração |
|---|---|
| Fechamento | `batidas = list(_bap(colab, inicio_busca, fim_busca))`; busca de início local 00:00−1 dia até fim local 23:59:59+1 dia (`app/ponto/services/fechamento.py:612-624`). Passagem em `app/ponto/services/fechamento.py:745`. |
| Espelho | `batidas = list(batidas_apuraveis(colaborador, _lo, _hi))`; mesmo envelope local de ±1 dia (`app/ponto/services/espelho.py:1019-1022`). Passagem em `app/ponto/services/espelho.py:1164`. |
| PDF | `batidas_periodo = list(_bap(colaborador, inicio, fim))`; início pedido 00:00−12h até fim pedido 00:00+1d12h (`app/relatorios/pdf_espelho.py:264-272`). Passagem em `app/relatorios/pdf_espelho.py:419`. A carga acontece **antes** do teto em ontem (`app/relatorios/pdf_espelho.py:287`). |

Nos três caminhos, `batidas_apuraveis` retorna objetos do ORM: consulta `Batida.objects`, exclui retratadas, aplica limites de timestamp/colaborador e ordena por timestamp; não localiza nem substitui os timestamps (`app/ponto/turnos.py:2863-2871`).

`Batida.timestamp` é `DateTimeField`; a base configura `USE_TZ=True` e `TIME_ZONE='America/Sao_Paulo'`; produção herda essa base e usa PostgreSQL (`app/ponto/models.py:23`, `app/config/settings/base.py:93-95`, `app/config/settings/prod.py:1`, `app/config/settings/prod.py:9-17`).

**O esperado nesse fluxo ORM é timestamp aware em UTC**, coerente com o contrato documentado de banco UTC (`app/docs/LEIS.md:107`). A classe concreta do `tzinfo` e os objetos efetivamente retornados em execução permanecem **[nao verificado]**; não houve consulta ao banco.

**3. Censo das expressões que extraem data**

Na tabela, **local** significa o fuso corrente do Django, cujo padrão configurado é `America/Sao_Paulo` (`app/config/settings/base.py:93`). `.date()` cru usa a data já representada no objeto; portanto, no fluxo ORM UTC esperado acima, produz dia UTC, sem conversão local (`app/ponto/services/fechamento.py:245`).

| Local | Forma | Dia produzido |
|---|---|---|
| `app/ponto/services/fechamento.py:245` | `b.timestamp.date()` | Do timestamp cru; **UTC no fluxo ORM esperado**. |
| `app/ponto/services/fechamento.py:252` | `b.timestamp.date()` | Mesmo critério; pertencimento a `_ok`. |
| `app/ponto/services/fechamento.py:253` | `b.timestamp.date()` — **duas ocorrências** | Mesmo critério; comparação com os limites da fatia. |
| `app/ponto/services/fechamento.py:784` | `timezone.localtime(b.timestamp).date()` | **Civil local**, para dias com batida. |
| `app/ponto/services/fechamento.py:830` | `timezone.localtime(_p_orf.entrada).date()` | **Civil local da entrada** do período órfão. |
| `app/ponto/services/fechamento.py:926` | `timezone.localtime(p.entrada).date()` | **Civil local da entrada**, na consulta de escala certa. |
| `app/ponto/services/fechamento.py:938` | `timezone.localtime(_p_f.entrada).date()` | **Civil local da entrada**, na classificação de feriado. |
| `app/ponto/services/fechamento.py:1553` | `timezone.localtime(quando).date()` se aware; senão `quando.date()` | Aware: **local**. Naive: data crua, sem fuso demonstrável. |
| `app/ponto/services/espelho.py:84` | `timezone.localtime(p.entrada).date()` | **Civil local da entrada**, no recorte de `somar_periodos`. |
| `app/ponto/services/espelho.py:426` | `timezone.localtime(p.entrada).date()` | **Civil local da entrada**, no contador de turnos da competência. |

`_so_a_data(quando)` também alcança timestamps de batida: registro/retratação envia o timestamp ao evento, que resolve a competência por `competencia_do_fato` → `competencias_do_fato` → `_so_a_data` (`app/ponto/registro_batida.py:160`, `app/ponto/registro_batida.py:281`, `app/ponto/services/fechamento.py:1793`, `app/ponto/services/fechamento.py:1588`, `app/ponto/services/fechamento.py:1579`).

No **PDF**, as extrações executáveis encontradas são de **relógio ou limites**, não de timestamp de batida:

| Local | Forma | Origem |
|---|---|---|
| `app/relatorios/pdf_espelho.py:287` | `timezone.localtime(agora).date()` | Hoje local para o teto. |
| `app/relatorios/pdf_espelho.py:298` | `timezone.localtime(inicio).date()` e `timezone.localtime(fim).date()` | Limites locais da busca, para feriados. |
| `app/relatorios/pdf_espelho.py:597` | `timezone.localtime(agora).date()` | Hoje local. |

As referências a `timestamp.date()` e `p.entrada.date()` no PDF são comentários históricos (`app/relatorios/pdf_espelho.py:483`, `app/relatorios/pdf_espelho.py:488`). No espelho, as referências históricas também estão em comentários/docstring (`app/ponto/services/espelho.py:57`, `app/ponto/services/espelho.py:444-468`).

Outros `.date()` encontrados recortam **limites**, não batidas: `inicio/fim` no fechamento (`app/ponto/services/fechamento.py:646`, `:648`, `:654`, `:674`, `:745`, `:842`) e `_lo/_hi` no espelho (`app/ponto/services/espelho.py:1029`). `datetime.date(_a, _mm, _dd)` constrói uma data de calendário (`app/ponto/services/espelho.py:258`).

Não foram encontradas chamadas executáveis de `localdate`, `data_local` ou lookups `timestamp__date` nesses três arquivos; cobertura de formas indiretas/dinâmicas **[nao verificado]**.

**4. O que `motor.calcular_mes` faz fora de `[ini, fim]`**

Ele **não recorta primeiro cada batida pelo seu dia civil**. Copia os objetos recebidos, entrega as batidas ao juiz e depois remove duplicatas/monta períodos (`app/ponto/motor_calculo_v2.py:2263-2268`, `app/ponto/motor_calculo_v2.py:2346-2354`). O juiz recebe a lista sem corte por dia nesse adaptador (`app/ponto/motor_calculo_v2.py:882`).

O recorte inclusivo acontece **por período**, usando `data_jornada`:

1. Obtém o `data_turno` do juiz; sem correspondência, usa `localizar(entrada).date()` (`app/ponto/motor_calculo_v2.py:889-894`, `app/ponto/motor_calculo_v2.py:2403-2404`).
2. Pode herdar o dia da jornada anterior quando a entrada ocorre entre 0 e 6 horas depois da saída anterior (`app/ponto/motor_calculo_v2.py:2405-2409`, `app/ponto/motor_calculo_v2.py:2423`).
3. Descarta o período se `data_jornada < data_ini` ou `data_jornada > data_fim` (`app/ponto/motor_calculo_v2.py:2429-2432`).

Portanto, uma saída em dia civil posterior a `fim` pode contribuir para uma jornada dentro da janela; uma jornada fora dela é descartada após o pareamento (`app/ponto/motor_calculo_v2.py:2354`, `app/ponto/motor_calculo_v2.py:2429-2432`). O fallback é **dia civil local**, porque `localizar` converte para `settings.TIME_ZONE` e interpreta naive nesse fuso (`app/ponto/motor_calculo_v2.py:28`, `app/ponto/motor_calculo_v2.py:73-79`).

As especializações `MotorTurnoPartido` e `MotorComercial` passam pelo cálculo base e posteriormente agrupam também pelo dia do turno, com fallback local (`app/ponto/motor_calculo_v2.py:2908`, `app/ponto/motor_calculo_v2.py:2941-2942`, `app/ponto/motor_calculo_v2.py:3301`, `app/ponto/motor_calculo_v2.py:3312-3314`).

**5. Funções públicas candidatas e chamadores**

| Função | Contrato | Chamadores de produção |
|---|---|---|
| `ponto.turnos.data_local(ts)` | Retorna `timezone.localtime(ts).date()` ou `None`; exige instante aware, sem defesa para naive (`app/ponto/turnos.py:43-46`). | Signal de recomposição (`app/ponto/signals.py:21-23`); comando `detectar_par_relampago` (`app/ponto/management/commands/detectar_par_relampago.py:302-303`); chamadas internas abaixo. |
| `core.filtro.dia_de(valor)` | Datetime aware → dia local; se a conversão falha, usa `.date()` cru. Aceita também `date`/`None` (`app/core/filtro.py:118-138`). | `core.filtro.aplicar` (`app/core/filtro.py:194`); caminho de holerite fornece `avisado_em`/`publicado_em` (`app/holerite/views.py:407-411`). |

Chamadores internos de `data_local`:

| Chamador | Local |
|---|---|
| `_data_do_turno` | `app/ponto/turnos.py:284` |
| `_mk` | `app/ponto/turnos.py:320` |
| `_saida_pela_posicao_entre_turnos` | `app/ponto/turnos.py:1013` |
| `_fechar_aberto_com_saida_seguinte` | `app/ponto/turnos.py:1098` |
| `_fechar_aberto_na_pausa_sem_volta` | `app/ponto/turnos.py:1186` |
| `_vespera`, dentro de `parear_turnos` | `app/ponto/turnos.py:1294` |
| `par_completo` | `app/ponto/turnos.py:2198` |
| `_dia_das_batidas_puro` | `app/ponto/turnos.py:2918` |

`dia_das_batidas` responde outra pergunta: **dia atribuído à batida**, usando `data_turno` quando reclamada por turno e data civil local para as restantes (`app/ponto/turnos.py:2874-2881`, `app/ponto/turnos.py:2912-2918`).

**6. Contraexemplo naive**

**Não localizei contraexemplo comprovado chegando a `apurar_por_fatia`; impossibilidade geral [nao verificado].** Os três caminhos identificados recarregam as listas do ORM antes da chamada (`app/ponto/services/fechamento.py:624`, `app/ponto/services/espelho.py:1022`, `app/relatorios/pdf_espelho.py:272`).

A função aceita uma coleção sem validar `tzinfo`; um objeto artificial naive passaria pelos `.date()` crus. Isso é possibilidade da interface, sem origem de produção demonstrada (`app/ponto/services/fechamento.py:132`, `app/ponto/services/fechamento.py:244-253`).

As cinco chamadas diretas de teste passam `batidas=[]` (`app/ponto/tests/test_composicao_de_fatias_e_total.py:86-88`, `:104-106`, `:127-129`, `:165`, `:197`). Existem stubs naive, mas entregues diretamente a `_periodos_pelo_marco`, não a essa porta (`app/ponto/tests/test_selo_motor_nao_pareia_pelo_tipo_gravado.py:94-99`).

O motor e o pareador possuem normalização defensiva de naive, **posterior aos filtros da fatia**; isso comprova suporte defensivo, não uma origem naive alcançando a apuração (`app/ponto/motor_calculo_v2.py:73-79`, `app/ponto/turnos.py:1524-1529`, `app/ponto/services/fechamento.py:277-282`).

## 06/11 19:42 · sessao 34c0fb2b6f22 · LEI-FIO-MUDO-B · censo · origem=Code

**O reaponte muda a composição operacional das disputas, mas também atinge uma associação histórica real:** a absorção de perguntas por ausência usa `disputa_id` como parte da identidade do ato. Portanto, “sem `resposta_colab` e em disputa fechada” não basta para demonstrar que a transferência preserva todos os leitores. (`app/chamados/services/materializacao.py:585-618`)

O efeito em produção, as quantidades atingidas e a execução dos testes permanecem **[nao verificado]**. As consequências abaixo são inferências do código para transferência entre disputas do mesmo colaborador, preservando o PK da pergunta; o gate identifica o fato por colaborador, motivo e data. (`app/chamados/models.py:2392-2395`)

**Respostas diretas:**

- **Escritor existente da FK:** encontrei a criação em `PerguntaDisputa.abrir`. O reapontamento existente no gate escreve `celula` e `chamado_destino`; `fio.reapontar_resolvedora` escreve destino e, quando ausente, data. Nenhum desses reaponta `disputa`. Outro escritor da FK fora da criação não foi localizado no código censado; escrita dinâmica ou externa permanece **[nao verificado]**. (`app/chamados/models.py:2431-2477`; `app/chamados/services/fio.py:831-848`)
- **Integridade declarada:** a FK é obrigatória, com `CASCADE` e `related_name='perguntas'`; o `Meta` da pergunta não declara `unique`, `UniqueConstraint` ou `CheckConstraint` por disputa. O `save` envolve a persistência em transação, sem impedir mudança da FK. O esquema efetivamente instalado é **[nao verificado]**. (`app/chamados/models.py:2211-2215`; `app/chamados/models.py:2847-2860`; `app/chamados/migrations/0014_perguntadisputa.py:27`)
- **A fechada que perde a única pergunta continua fechada nesses caminhos automáticos.** O signal da pergunta captura somente o ID **novo**; o detector de becos ignora fechadas; o fechamento não reclassifica uma disputa já fechada como `vazia`. (`app/chamados/signals.py:191-202`; `app/chamados/services/becos.py:25-33`; `app/chamados/models.py:1820-1824`)
- **Ela pode aparecer vazia quando consultada diretamente**, mas fica fora do relatório de pendências, que só coleta abertas. `disputa_vazia()` não detecta esse esvaziamento: conta o motivo de fechamento gravado, sem recontar perguntas. I6 também parte das perguntas, deixando a fechada sem perguntas fora desse universo. (`app/chamados/relatorios_disputa.py:12-14`; `app/chamados/relatorios_disputa.py:40-43`; `app/chamados/services/vigia_fio.py:727-734`; `app/chamados/management/commands/auditar_invariantes_chamados.py:715-740`)
- **Reabertura humana continua possível**, inclusive de uma fechada sem perguntas; não é consequência automática da perda da pergunta. (`app/chamados/models.py:2040-2075`; `app/chamados/tests/test_reabrir_disputa_admin_sem_aberta.py:22-44`)

Há cinco cuidados materiais para o desenho:

1. **Absorção por ausência:** uma irmã muda pode estar materializada sem ter resposta. Transferi-la separa seu `(disputa_id, materializada_em)` daquele do escape respondido; a rejeição da ausência pode deixar de devolvê-la. O teste atual devolve ambas, mantendo a associação original. (`app/chamados/services/materializacao.py:531-549`; `app/chamados/services/materializacao.py:605-618`; `app/chamados/services/materializacao.py:647-665`; `app/chamados/tests/test_o261_e1_ausencia_rejeitada_devolve.py:88-103`)
2. **Contexto e caches:** materialização lê `disputa.contexto[motivo]`, inclusive `hora_tentativa` no ajuste overnight. A chave do memo de `pergunta_viva` não contém disputa nem contexto; `esta_stale` tampouco detecta a troca da FK por si. Relavra e invalidação dos memos precisam ser verificadas no pós-estado da transferência. (`app/chamados/services/materializacao.py:343-347`; `app/chamados/services/materializacao.py:420`; `app/chamados/juizes.py:811-847`; `app/chamados/veredito_lavrado.py:179-190`)
3. **Transferir não ressuscita:** validação, tranca e vias de identidade continuam podendo matar a pergunta. Uma muda encerrada por admin não passa a ser viva apenas porque recebeu disputa aberta. (`app/chamados/juizes.py:1135-1172`)
4. **Trilha:** o gate retorna a existente antes de registrar `pergunta_emitida`; esse log só nasce na criação. O histórico reconstrói carimbos pelo destino atual, sem evento específico de transferência da FK. Continuidade histórica exige teste próprio de antes/depois e idempotência. (`app/chamados/models.py:2475-2485`; `app/chamados/services/historico.py:86-114`)
5. **Guarda da costura:** `resolvedora_valida` confere dia/turno; `_costura_nao_mata_o_fio` impede ancorar pergunta viva em destino terminal. Nenhuma substitui a elegibilidade específica da transferência de disputa. (`app/chamados/juizes.py:1963-1999`; `app/chamados/services/fio.py:758-780`)

Na tabela, “atual” significa que o leitor usa a associação presente, sem guardar a disputa natal. Os testes citados fixam comportamentos próximos; cobertura específica da **troca da FK** permanece **[nao verificado]**. [nao verificado]

| Sítio | Escritor/leitor | Supõe pertença fixa? | Efeito do reaponte | Teste que fixa | [nao verificado]
|---|---|---|---|---|
| `app/chamados/models.py:2392`, `app/chamados/models.py:2398` | L/E: identidade e nascimento | Não: identidade é pessoa/motivo/data | Conserva dedup global e PK; acrescentaria uma escrita de FK ao ramo existente | `app/chamados/tests/test_porta_pergunta_identidade.py:164`; `app/chamados/tests/test_contract_pergunta_fato.py:10` |
| `app/chamados/models.py:327`, `app/chamados/models.py:345` | L: perguntas ancoradas no chamado | Pai atual é fallback | Sem destino, pergunta deixa de reter o pai antigo e passa ao novo | `app/chamados/tests/test_guarda_disputa_status.py:42`; FK móvel **[nao verificado]** |
| `app/chamados/models.py:1434`, `app/chamados/models.py:1500`, `app/chamados/models.py:1531` | L/E: ressuscitar perguntas e reabrir disputas | Usa FK atual | Ressurreição posterior reabre a disputa atual; não reencontra a origem esvaziada | FK móvel **[nao verificado]** |
| `app/chamados/models.py:1736` | L: badge de pendências | Atual; exige aberta e pergunta viva | Pergunta passa ao conjunto da disputa nova se continuar viva; conta disputas distintas | `app/chamados/tests/test_badge_disputa_pendente.py:29`; `app/chamados/tests/test_fio_le_o_juiz.py:262` |
| `app/chamados/models.py:1783`, `app/chamados/models.py:1794`, `app/chamados/models.py:1836` | L/E: respostas sem veredito, C7 e fechamento | Composição atual | Origem perde a pergunta; destino passa a considerá-la. Fechar depende de ausência de vivas **e** de respostas sem veredito | `app/chamados/tests/test_fechar_disputa_materializada.py:51` |
| `app/chamados/models.py:1976` | L/E: remover motivo e apagar suas perguntas | Composição atual | Remoção no contêiner antigo deixa de alcançar a pergunta movida; na nova, pode alcançá-la | `app/chamados/tests/test_invariantes_disputa.py:139`; FK móvel **[nao verificado]** |
| `app/chamados/models.py:2040`, `app/chamados/models.py:2077`, `app/chamados/models.py:2142` | L/E: reaberturas e contador do colab | Estado/quota pertencem à disputa | Quotas não acompanham a pergunta; correção passa a operar sobre os membros da nova disputa | `app/chamados/tests/test_porta_disputa_ciclo.py:283`; `app/chamados/tests/test_disputa_transicoes.py:131` |
| `app/chamados/models.py:2660`, `app/chamados/models.py:2668` | L: campos que acordam a célula e identidade do evento | Pessoa atual | `disputa`/`disputa_id` já acordam outbox quando salvos por `save`; evento usa pessoa/célula atuais | FK móvel **[nao verificado]** |
| `app/chamados/signals.py:191`; `app/chamados/services/becos.py:29`, `app/chamados/services/becos.py:67` | L/E: fechamento de beco | Só conhece FK atual | Agenda análise da nova disputa; não captura origem. Fechada anterior é ignorada | `app/chamados/tests/test_varredura_becos_evento_pergunta.py:47`, `app/chamados/tests/test_varredura_becos_evento_pergunta.py:94` |
| `app/chamados/signals.py:214` | L/E: outbox da pergunta | Pessoa/célula atuais | Save contendo FK emite evento; não há evento separado para “disputa anterior perdeu membro” | FK móvel **[nao verificado]** |
| `app/chamados/signals.py:18`, `app/chamados/signals.py:124` | Gatilhos de fechamento do pai/materialização | Composição no fechamento | Transferência da pergunta não salva a disputa antiga; fechamento posterior da nova usa seus membros atuais | `app/chamados/tests/test_disputa_transicoes.py:71` |
| `app/chamados/services/fio.py:74`, `app/chamados/services/fio.py:116` | L: data memoizada e âncora | Pai atual como fallback | Sem destino, âncora muda; `_dfd_memo` não compara campos antes de retornar | `app/chamados/tests/test_fio_ancora_destino.py:72`; invalidação por FK **[nao verificado]** |
| `app/chamados/services/fio.py:135` | L: disputa do chamado pelo destino | Exige aberta atual | Passa a localizar a nova disputa pela pergunta transferida | `app/chamados/tests/test_fio_disputa_por_destino.py:21` |
| `app/chamados/services/fio.py:218`, `app/chamados/services/fio.py:229`, `app/chamados/services/fio.py:252` | L: montagem do fio/cards | Universo por contêiner atual | Sai do conjunto de fechadas, perde flag `em_disputa_fechada`, entra no universo de abertas do colab, sujeito aos demais filtros | `app/chamados/tests/test_fio_le_o_juiz.py:105`, `app/chamados/tests/test_fio_le_o_juiz.py:121` |
| `app/chamados/services/fio.py:392` | L: mapa pai → destinos | Relação atual representa filiação | Pai antigo perde filho no mapa; novo ganha | FK móvel **[nao verificado]** |
| `app/chamados/services/fio.py:591`, `app/chamados/services/fio.py:629` | L/E: validar, recusar, corrigir | Estado atual autoriza ação | Deixa a via especial de validação em fechada; pode entrar em recusa/correção de aberta | `app/chamados/tests/test_fio_le_o_juiz.py:192`, `app/chamados/tests/test_fio_le_o_juiz.py:228` |
| `app/chamados/services/fio.py:769`, `app/chamados/services/fio.py:831`, `app/chamados/services/fio.py:931` | L/E: guarda, reaponte e migração de resolvedoras | Conserva FK no código atual | **Escreve destino, não disputa**; guarda terminal e relavra continuam necessárias | `app/chamados/tests/test_o261e_f1_porta_encerramento.py:252`; `app/chamados/tests/test_porta_reapontamento_pergunta.py:245` |
| `app/chamados/juizes.py:72`, `app/chamados/juizes.py:205`, `app/chamados/juizes.py:2029` | L: dias da disputa/chamado e competência | Perguntas atuais são evidência do contêiner | Origem perde datas; nova ganha nos degraus que dependem dessas perguntas | FK móvel **[nao verificado]** |
| `app/chamados/juizes.py:346`, `app/chamados/juizes.py:361` | L: slot/completude | Contexto da disputa é insumo | Contexto novo pode mudar derivação/julgamento; data da pergunta prevalece em alguns fallbacks | FK móvel entre contextos **[nao verificado]** |
| `app/chamados/juizes.py:664`, `app/chamados/juizes.py:752`, `app/chamados/juizes.py:774`, `app/chamados/juizes.py:932`, `app/chamados/juizes.py:1200`, `app/chamados/juizes.py:1528` | L: pessoa, empresa, grade e provas | Identidade consistente da pessoa | Mesma pessoa preserva esses insumos; não exige disputa natal | FK móvel **[nao verificado]** |
| `app/chamados/juizes.py:811`, `app/chamados/juizes.py:835` | L: memo de `pergunta_viva` | FK/contexto não integram chave | Troca isolada da FK na mesma instância pode reutilizar julgamento anterior | `app/chamados/tests/test_selo_memo_pergunta_viva.py:117`; troca da FK **[nao verificado]** |
| `app/chamados/juizes.py:1021`, `app/chamados/juizes.py:1300` | L: mapa de identidade/alimentação por célula | Pessoa/célula | Mantém universo se pessoa/célula forem preservadas; atribuição de `d.colaborador` não escreve FK da pergunta | FK móvel **[nao verificado]** |
| `app/chamados/juizes.py:1230`, `app/chamados/juizes.py:1392`, `app/chamados/juizes.py:1399` | L: vivas, corrigíveis e fila global | Composição atual; fila global por pergunta | Conjuntos locais migram; seleção global por pergunta pode permanecer | FK móvel **[nao verificado]** |
| `app/chamados/juizes.py:1751` | L: irmãs respondidas, fallback ao pai | Pai atual concede crédito | Resposta futura passa a poder ser creditada ao pai novo | FK móvel **[nao verificado]** |
| `app/chamados/juizes.py:1963` | L: `resolvedora_valida` | Não exige disputa natal | Confere dia/turno; não garante identidade da disputa/pessoa para uma transferência arbitrária | `app/chamados/tests/test_selo_resolvedora_do_dia.py:67`, `app/chamados/tests/test_selo_resolvedora_do_dia.py:94` |
| `app/chamados/juizes.py:2080` | L: disputa viva bloqueadora do chamado | Aberta atual; vazia também bloqueia | Origem fechada vazia fica fora; destino passa a considerar pergunta movida | `app/chamados/tests/test_guarda_disputa_status.py:58`, `app/chamados/tests/test_guarda_disputa_status.py:64` |
| `app/chamados/veredito_lavrado.py:162`, `app/chamados/veredito_lavrado.py:187` | L/E: marcos, stale e lavra | Marcos por pessoa; FK não é marco | Mesma pessoa mantém marcos; mudança de contexto exige relavra explícita | `app/chamados/tests/test_porta_veredito_pergunta.py:259`; FK móvel **[nao verificado]** |
| `app/chamados/services/materializacao.py:343`, `app/chamados/services/materializacao.py:420`, `app/chamados/services/materializacao.py:471` | L: data/contexto/overnight/ausência | Contexto atual da disputa | Pode mudar fallback de data e `hora_tentativa`, mesmo preservando pessoa | FK móvel **[nao verificado]** |
| `app/chamados/services/materializacao.py:531`, `app/chamados/services/materializacao.py:1289`, `app/chamados/services/materializacao.py:1344`, `app/chamados/services/materializacao.py:1393` | L/E: irmãs, materialização, contagem e fechamento | Composição atual | Unidade de materialização e quitação migra à nova; fechada antiga não é refechada | `app/chamados/tests/test_fechar_disputa_materializada.py:51`; `app/chamados/tests/test_validar_materializa_wiring.py:51` |
| `app/chamados/services/materializacao.py:609`, `app/chamados/services/materializacao.py:614` | L: grupo histórico de absorção | **Sim: FK integra identidade do ato** | Mover irmã muda pode separá-la do escape e impedir devolução após rejeição | `app/chamados/tests/test_o261_e1_ausencia_rejeitada_devolve.py:88`; com reaponte **[nao verificado]** |
| `app/chamados/services/materializacao.py:660`, `app/chamados/services/materializacao.py:686` | L/E: devolver absorvidas/resgatar pai | FK/pai atuais | Reabre disputa atual; não recupera automaticamente origem vazia | `app/chamados/tests/test_o261_e1_ausencia_rejeitada_devolve.py:99` |
| `app/chamados/services/materializacao.py:931`, `app/chamados/services/materializacao.py:942` | E: trilha de materialização | Relata pai atual | Materialização futura registra pai novo; não reescreve trilha anterior | FK móvel **[nao verificado]** |
| `app/chamados/services/validacao.py:191`, `app/chamados/services/validacao.py:355`, `app/chamados/services/validacao.py:424`, `app/chamados/services/validacao.py:526`, `app/chamados/services/validacao.py:795` | L/E: validação, recusa, materialização e reabertura | Autoriza por FK atual | Pedido amarrado à disputa antiga retorna `nao_encontrada`; próximos atos usam disputa/pai novos | `app/chamados/tests/test_validar_materializa_wiring.py:51`; FK móvel **[nao verificado]** |
| `app/chamados/services/acoes_disputa.py:95`, `app/chamados/services/acoes_disputa.py:129`, `app/chamados/services/acoes_disputa.py:260`, `app/chamados/services/acoes_disputa.py:338`, `app/chamados/services/acoes_disputa.py:375` | L/E: ações, contadores, reabertura admin | Membros atuais | Contagem migra; origem vazia pode informar “Nenhuma pergunta cadastrada”. Reabertura admin chama gate | `app/chamados/tests/test_recobrar_questionario.py:80`; `app/chamados/tests/test_reabrir_disputa_admin_sem_aberta.py:22` |
| `app/chamados/services/acoes_chamado.py:342`, `app/chamados/services/acoes_chamado.py:388` | L: detalhe e “todas validadas” | Membros atuais | Origem perde lista; guarda `bool(_todas_perg)` evita tratar vazio como todas validadas | FK móvel **[nao verificado]** |
| `app/chamados/services/disputa_emissao.py:300`, `app/chamados/services/disputa_emissao.py:371`, `app/chamados/services/disputa_emissao.py:387`, `app/chamados/services/disputa_emissao.py:909`, `app/chamados/services/disputa_emissao.py:1237` | L: dedup local/complementação | Associação atual | Origem perde satisfação do dedup local; gate global continua segurando fato único | `app/chamados/tests/test_selo_repergunta_premissa_viva.py:64`; FK móvel **[nao verificado]** |
| `app/chamados/services/disputa_emissao.py:1088`, `app/chamados/services/disputa_emissao.py:1112` | E: motivos e autofecho | Metadados separados da FK | Emissão pode atualizar motivos da nova; simples troca da FK não recompõe motivos/contexto da origem | `app/chamados/tests/test_hx_disputa_muda.py:24`; `app/chamados/tests/test_hx_disputa_muda_sem_ata.py:51` |
| `app/chamados/services/disputa_emissao.py:1344`, `app/chamados/services/disputa_emissao.py:1485`, `app/chamados/services/disputa_emissao.py:1580`, `app/chamados/services/disputa_emissao.py:1621`, `app/chamados/services/disputa_emissao.py:1657`, `app/chamados/services/disputa_emissao.py:1715` | L/E: resposta, ocorrência, recobrança e resolução | Pai/composição atuais | Resposta futura escreve no pai novo; contadores e fechamento usam nova disputa; vários ramos pulam fechadas | `app/chamados/tests/test_b2d_resposta_visivel.py:66`; FK móvel **[nao verificado]** |
| `app/chamados/services/declaracao_texto.py:227`, `app/chamados/services/declaracao_texto.py:325`, `app/chamados/services/declaracao_texto.py:536`, `app/chamados/services/declaracao_texto.py:614`, `app/chamados/services/declaracao_texto.py:644` | L/E: reutilização, gate e validação | Busca local por disputa; global por fato | Reutilização local migra; pessoa/motivo/data preservam identidade global | FK móvel **[nao verificado]** |
| `app/chamados/services/relavra_via.py:152`, `app/chamados/services/relavra_via.py:181`, `app/chamados/services/relavra_via.py:221`, `app/chamados/services/relavra_via.py:243` | L/E: relavra/descarimbo/reabertura | Estado atual da FK | Entra em recortes de aberta; eventual reabertura visita somente a disputa atual | `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:135`, `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:161` |
| `app/chamados/services/passivo_validado.py:333`, `app/chamados/services/passivo_validado.py:354`, `app/chamados/services/passivo_validado.py:516`, `app/chamados/services/passivo_validado.py:521` | L/E: materialização e quitação em lote | FK atual agrupa fechamento | Crédito de materialização/contagem passa à nova disputa | FK móvel **[nao verificado]** |
| `app/chamados/services/cobranca.py:61`, `app/chamados/services/cobranca.py:91`, `app/chamados/services/cobranca.py:230` | L/E: pais, prazo e agrupamento | Pai atual é dono do prazo | Cobrança passa a consultar prazo e pai novos; quota da origem não migra | `app/chamados/tests/test_recobrar_questionario.py:88`; FK móvel **[nao verificado]** |
| `app/chamados/services/explicador.py:122`, `app/chamados/services/explicador.py:124`, `app/chamados/services/explicador.py:137` | L: explicação por disputa | Membros atuais | Origem vazia produz “Tudo resolvido — a disputa pode fechar”; não escreve estado | FK móvel **[nao verificado]** |
| `app/chamados/services/raiox.py:31`, `app/chamados/services/raiox.py:55`, `app/chamados/services/raiox.py:60`, `app/chamados/services/raiox.py:135` | L: stale/crédito/placar | Pai atual como fallback | Marcos da pessoa permanecem; crédito pelo pai pode migrar | `app/chamados/tests/test_selo_defer_json_creditos.py:12`; FK móvel **[nao verificado]** |
| `app/chamados/services/painel.py:262`, `app/chamados/services/painel.py:264`; `app/chamados/services/verbos.py:310` | L: empresa/praça/atividade | Pessoa, não origem | Mesma pessoa preserva recortes desses leitores | FK móvel **[nao verificado]** |
| `app/chamados/services/pergunta_no_app.py:46`, `app/chamados/services/pergu

[truncado em 20000 de 35196 caracteres]

## 06/11 19:41 · sessao 896c447650ad · - · - · origem=Ronald (tmux)

1. **Escritores dos limites e de `_meus`:**

- `fatia_ini = max(ini, esc.data_inicio, DATA_INICIO_OPERACAO)`; `fatia_fim = min(fim, esc.data_fim ou fim)`: `app/ponto/services/fechamento.py:204-205`. Os campos da vigência são `DateField`: `app/escala/models.py:875-876`. O piso operacional é `date(2026,5,21)`: `app/core/constants.py:5`.
- As chaves `(colaborador_id, data)` das células geram `_dono_do_dia`, após consulta a `vinculo_do_dia`: `app/ponto/services/fechamento.py:189-193`. A origem dessas chaves é `CelulaDia.data`: `app/escala/alimentacao.py:73-78`, campo `DateField` em `app/escala/models.py:1685`.
- `_meus` inicial contém as datas atribuídas ao vínculo dentro dos limites: `app/ponto/services/fechamento.py:207-208`. Recebe também dias sem dono cobertos por exatamente um vínculo: `app/ponto/services/fechamento.py:221-231`. Havendo `_julgados`, substitui os limites por seus extremos: `app/ponto/services/fechamento.py:232-236`. O pad deriva desses limites por ±1 dia: `app/ponto/services/fechamento.py:242-243`.
- A célula usa **dia nominal do turno previsto, ancorado no calendário local**: o gerador declara instantes “ISO local naive” e atribui marcos pós meia-noite a D+1: `app/ponto/management/commands/gerar_celulas.py:36-40`, `app/ponto/management/commands/gerar_celulas.py:67-69`. Exemplo codificado: célula 21/08 pode conter saída 22/08 07:02: `app/ponto/juiz_batida.py:206-222`.
- Vigências são datas cadastrais comparadas diretamente nesse domínio de dias; a resolução da célula recusa geradora fora da vigência: `app/escala/alimentacao.py:269-286`. Não há conversão de timestamp UTC para produzir essas datas nesses pontos: `app/ponto/services/fechamento.py:189-236`.

**Origem dos `ini/fim` dos chamadores:**

| Chamador | Fonte |
|---|---|
| Fechamento | `janela_fechamento(mes,ano,empresa)` gera datas; transforma-as em meia-noite/final de dia aware e passa `.date()` a `apurar_por_fatia`: `app/ponto/services/fechamento.py:612-614`, `app/ponto/services/fechamento.py:745-746`. A janela constrói `date` por corte da empresa, sem timestamp: `app/ponto/janelas.py:15-21`, `app/ponto/janelas.py:54-57`. |
| Espelho | `autoridade_do_periodo` recebe `ini/fim` e os repassa: `app/ponto/services/espelho.py:971`, `app/ponto/services/espelho.py:1164`. No caminho da tela, `ini=vis_ini=min(piso_visual,apur_ini)` e `fim=hoje`: `app/ponto/services/espelho.py:1225-1227`. `hoje` deriva de `localtime(now).date()` e `apur_ini/fim` da janela da empresa: `app/ponto/views.py:97-98`, `app/ponto/views.py:153-158`. O piso deriva de DIO/início do vínculo, limitado a hoje: `app/ponto/services/espelho.py:39-48`. |
| PDF | Recebe datas livres: `app/relatorios/pdf_espelho.py:187`, `app/relatorios/pdf_espelho.py:1038-1046`. No mensal, recebe janela da competência: `app/relatorios/pdf_espelho.py:654-663` → `app/relatorios/cartao_pela_celula.py:21-24` → `app/folha/export.py:47-51` → `app/ponto/janelas.py:54-57`. Antes de apurar, limita `fim` a ontem local, com piso em `data_ini`: `app/relatorios/pdf_espelho.py:285-288`, `app/relatorios/cartao_pela_celula.py:37-41`. |

2. **Escritores das listas `batidas`:**

| Chamador | Construção e janela |
|---|---|
| Fechamento | `batidas = list(_bap(colab,inicio_busca,fim_busca))`, desde início local 00:00−1 dia até fim local 23:59:59+1 dia: `app/ponto/services/fechamento.py:612-624`; lista passada em `app/ponto/services/fechamento.py:745`. |
| Espelho | `batidas = list(batidas_apuraveis(...))`, mesmo envelope local de ±1 dia: `app/ponto/services/espelho.py:1019-1022`; lista passada em `app/ponto/services/espelho.py:1164`. |
| PDF | `batidas_periodo = list(_bap(...))`, início local 00:00−12h até fim pedido local 00:00+1d12h: `app/relatorios/pdf_espelho.py:264-272`; lista passada em `app/relatorios/pdf_espelho.py:419-420`. Atenção: essa carga antecede o teto em ontem de `app/relatorios/pdf_espelho.py:285-288`. |

Nos três, `_bap` consulta `Batida.objects`, filtra `retratada_em IS NULL`, limites por `timestamp` e colaborador, e ordena pelo timestamp, sem reescrever seu fuso: `app/ponto/turnos.py:2863-2871`. O campo é `DateTimeField`: `app/ponto/models.py:23`. `USE_TZ=True` e `TIME_ZONE='America/Sao_Paulo'`: `app/config/settings/base.py:93-95`; produção herda essas configurações e usa PostgreSQL: `app/config/settings/prod.py:1`, `app/config/settings/prod.py:9-17`.

**UTC aware é o contrato esperado do ORM nesse arranjo; a classe concreta do `tzinfo` dos objetos atuais não foi observada [nao verificado].** O próprio repositório declara timestamps UTC no banco: `app/docs/LEIS.md:107`; há registro histórico de observação do banco em UTC em 29/09: `app/docs/RELATO-ARQUIVO.md:21309-21310`. Dependências declaradas: Django 5.2.14 e psycopg 3.2.3: `app/requirements.txt:13`, `app/requirements.txt:21`.

**Nuance para consolidação:** o comentário que diz que o motor muta as batidas do chamador está em `app/ponto/services/fechamento.py:790-799`, mas `calcular_mes` agora faz cópia rasa de cada objeto antes de trabalhar: `app/ponto/motor_calculo_v2.py:2262-2268`. A conversão naive→aware local existe em `app/ponto/motor_calculo_v2.py:458-461`, depois da entrada no motor; portanto ela não normaliza os objetos antes dos filtros de `apurar_por_fatia`: `app/ponto/services/fechamento.py:244-253`, `app/ponto/services/fechamento.py:277-282`.

## 06/11 19:41 · sessao 540b03e22600 · SILENCIADO-ATE-E-PRAZO · censo · origem=Code

A cópia contém **duas regras incompatíveis para o vencido**: `qs_na_fila` devolve chamados de cobrança à fila quando o prazo passa; `particionar_verbos` os transforma em `HISTORICO`, excluído da lista padrão. Há testes exigindo cada comportamento. (`app/chamados/services/painel.py:186-188`; `app/chamados/services/verbos.py:14-18,45-51`; `app/chamados/tests/test_f9_contador_bate_com_a_lista.py:90-96`; `app/chamados/tests/test_f7_janela_arquivo.py:41-46`.)

O anexo fornecido não traz linhas de chamados: quantidade afetada, situação atual de produção e resultado de execução dos testes são **[nao verificado]**. Abaixo, todos os caminhos são relativos a `app/`; “muda” descreve o efeito inferido do código. [nao verificado]

Convenções: **(1)** prazo temporário; **(2)** presença persistente que retira da lista/cobrança; **outro** = auditoria, exibição, consulta histórica ou guarda de escrita, sem decidir por si a fila.

**Testes que efetivamente fixam o vencido como arquivo**, usados como referências na tabela final: [nao verificado]

| Referência | arquivo::teste | O que fixa | [nao verificado]
|---|---|---|
| T1 | `chamados/tests/test_f1_arquivar.py::test_prazo_vencido_vira_historico_e_nao_volta_pra_fila`, linha 48 | Vencido vira `historico`; `decidir=0` nas linhas 52-56. |
| T2 | `chamados/tests/test_f7_janela_arquivo.py::test_vencida_a_janela_vira_historico_e_NAO_volta_pra_fila`, linha 41 | Vencido não conta em `decidir` nem em `arquivado`, linhas 42-46. |
| T3 | `chamados/tests/test_f8_lista_esconde.py::test_lista_padrao_esconde_arquivado_e_historico`, linha 36 | Vencido fica fora da lista padrão, linhas 39-43. |
| T4 | `colaboradores/tests/test_ficha_do_colab.py::test_MORDE_arquivado_e_historico_dao_o_dono_arquivado`, linha 98 | Tanto data passada como futura produzem dono `arquivado`, linhas 109-114. |
| T5 | `chamados/tests/test_o261e_f5b_juiz_porta.py::test_MORDE_silenciado_na_celula_nao_e_numero_da_fila`, linha 144 | Vencido aparece no mapa como `historico`, mas `porta['fila']` fica vazia, linhas 152-165. |
| T6 | `chamados/tests/test_f9_busca_historico.py::test_so_arquivados_nunca_o_vivo`, linha 51; `::test_busca_por_colab`, linha 58 | Vencido continua pesquisável no arquivo. Isso fixa **consulta histórica**, não exclusão da cobrança; fixture passada na linha 32. |

A cura da cobrança contraria **T1–T5**. **T6 pode permanecer**: consultar um silêncio antigo e cobrar novamente são compatíveis. O teste `test_f8_lista_esconde.py::test_pilula_arquivados_mostra_so_os_da_janela`, linha 45, também pode permanecer: ele exige somente futuros na pílula Arquivados. (`chamados/services/verbos.py:48-51`; `chamados/services/painel.py:318-329`; `chamados/views_arquivar.py:153-180`.)

**A janela do colaborador não tem, nos casos localizados, teste que fixe a exclusão do vencido [nao verificado].** Os testes existentes usam silêncio **futuro**: `test_c1_janela_colab.py::test_arquivado_dentro_da_janela_tambem_sai`, linha 55; `test_c2_endpoints_janela.py::test_fio_com_so_janela_recorta`, linha 41; `test_c3_meus_arquivados.py::test_traz_fora_da_janela_e_arquivado_mas_nao_o_da_lista`, linha 45. (`chamados/tests/test_c1_janela_colab.py:58-60`; `chamados/tests/test_c2_endpoints_janela.py:27,44-48`; `chamados/tests/test_c3_meus_arquivados.py:33,48-52`.)

O censo dos testes fica separado da produção: [nao verificado]

| Arquivo de teste | Escrita/leitura localizada | Semântica coberta | [nao verificado]
|---|---|---|
| `api/tests/test_api_mensageria_fila.py:178` | Escrita de fixture futura | API/fila. |
| `chamados/tests/test_agir_por_dono.py:56` | Leitura por assert | Recusa não silenciou. |
| `chamados/tests/test_c1_janela_colab.py:28-34,55-60` | Escrita de fixture; consulta de IDs | Janela; só futuro. |
| `chamados/tests/test_c2_endpoints_janela.py:21-28,41-65` | Escrita; consulta/inspeção de consumidores | Fio e delegação da API; só futuro. |
| `chamados/tests/test_c3_meus_arquivados.py:28-34,45-52` | Escrita; consulta do drawer | Complemento da lista; só futuro. |
| `chamados/tests/test_c4_resgate.py:33,44-71` | Fixture e asserts do campo | Resposta/resgate limpa silêncio e protege contra prescrição. |
| `chamados/tests/test_f1_arquivar.py:35-56` | Escritas; leitura da partição | Futuro e vencido; T1. |
| `chamados/tests/test_f3_arquivar_porta.py:42-50,65-72` | Fixture e asserts | Porta de arquivar/desarquivar e prazo. |
| `chamados/tests/test_f3_beco1_reata_limbo.py:113,140,157` | Escritas futura/passada | Reatamento distingue silêncio vigente de vencido. |
| `chamados/tests/test_f4_arquivar_lote.py:44-45,57` | Asserts do campo | Lote e corte de idade. |
| `chamados/tests/test_f6_prescricao.py:41-60` | Asserts e leitura do primeiro prazo | Dry/apply/idempotência; não exercita vencido. |
| `chamados/tests/test_f7_janela_arquivo.py:26-51` | Escritas; leitura da partição | T2; futuro Arquivado e passado Histórico. |
| `chamados/tests/test_f8_lista_esconde.py:26-51` | Escritas; leitura da lista | T3 e pílula Arquivados. |
| `chamados/tests/test_f9_busca_historico.py:29-41,51-61` | Escritas; leitura da busca | T6. |
| `chamados/tests/test_f9_contador_bate_com_a_lista.py:50,90-96` | Fixture; leitura de `qs_na_fila` | **(1): vencido volta**. |
| `chamados/tests/test_fio_contador_ativos_pela_fila.py:56,64-65` | Fixtures futura/passada | Contador acompanha fila temporal. |
| `chamados/tests/test_o261e_f5_alcance.py:88` | Fixture futura | Alcance da porta. |
| `chamados/tests/test_o261e_f5b_juiz_porta.py:157,202,233` | Fixtures; retornos do juiz | T5 e exclusão de futuro. |
| `chamados/tests/test_porta_arquivo_silencio.py:136,153,181,202,226,264,314-402` | Escritas pela porta e asserts | Autor, motivo, limpeza, trilha, idempotência e prazo. |
| `chamados/tests/test_porta_arquivo_silencio.py:422-505` | Leitura estrutural; fontes sintéticas nas linhas 444-448 e 499-500 | Exclusividade da porta e da soma do prazo; fontes sintéticas não são escritores executados do modelo. |
| `chamados/tests/test_s4_pendente_suspende_cobranca.py:58-59,79,98-113` | Fixtures e asserts | Ausência suspende; rejeição levanta; SLA respeita silêncio futuro. |
| `chamados/tests/test_selo_i6_ouve_a_lei.py:54-55,143-144` | Escritas futuras | Silêncio nas fixtures do selo. |
| `chamados/tests/test_silenciar_isentos.py:12` | Leitura textual do código | Presença do nome do campo; não prova tratamento de vencido. |
| `chamados/tests/test_sla_pela_fila.py:72-74` | Fixture futura; leitura do SLA | Futuro fora da cobrança SLA. |
| `chamados/tests/test_varredura_isentos_evento.py:43-107` | Leitura por asserts; atos pela porta | Eventos silenciam; resgate de isento recebe novo véu; cron vigia. |
| `colaboradores/tests/test_ficha_do_colab.py:110-114,118-130` | Fixtures passada/futura; retorno e AST | T4 e uso de `FORA_DA_LISTA_PADRAO`. |
| `core/tests/test_contract_escritores_estado.py:35-38` | Leitura estrutural | Escritor permitido dos quatro campos. |
| `core/tests/test_registrado_nao_e_pendencia.py:108` | Fixture futura | Contagem de pendências. |
| `escala/tests/test_ranking_chamados_pela_fila.py:54,63-64` | Fixtures futura/passada | **(1): vencido volta** ao ranking. |
| `ponto/tests/test_contract_juiz_feriado.py:55-72` | Fonte sintética e AST | Definição única de `PRAZO_ARQUIVO_DIAS`. |

Sobre o prazo: **`PRAZO_ARQUIVO_DIAS = 30` está em `chamados/catalogo/motor.py:84`; a leitura numérica em produção encontrada está centralizada em `arquivo.prazo_padrao`, linhas 117-118.** Seus consumidores são as duas views de arquivamento, prescrição, isentos e ausência; os dois lotes especiais passam datas próprias de dez e cinco anos. (`chamados/views_arquivar.py:36,129`; `chamados/management/commands/arquivar_prescritos.py:84`; `chamados/services/silencio_isentos.py:39`; `chamados/services/silencio_ausencia.py:88`; `chamados/management/commands/arquivar_competencia_encerrada.py:274-288`; `chamados/management/commands/arquivar_sem_dia_irrecuperavel.py:30,88-94`.)

O registro da autoridade e a catraca também mencionam o prazo, sem consumir seu valor para negócio: `core/juizes.py:285,1387`. Os testes específicos são `test_porta_arquivo_silencio.py::test_MORDE_sem_ate_a_porta_usa_o_juiz_do_prazo`, linha 386; `::test_MORDE_o_teto_do_prazo_mora_SO_na_porta`, linha 456; `::test_MORDE_o_proprio_varredor_do_teto_ve_CODIGO`, linha 492; e `test_contract_juiz_feriado.py::test_MORDE_prazo_de_arquivo_definido_uma_vez`, linha 66. (`chamados/tests/test_porta_arquivo_silencio.py:386-402,456-505`; `ponto/tests/test_contract_juiz_feriado.py:66-72`.)

**Existe outra forma concreta de arquivo:** encerramento com `via_encerramento='competencia_trancada'`, sem exigir silêncio. Não há status `arquivado` nem campo `arquivado_em` na declaração de `ChamadoColaborador`. `silenciado_por`/`silenciado_motivo` são metadados; `resgatado_em` protege contra novo arquivamento. (`chamados/models.py:22-95`; `chamados/catalogo/motor.py:90-94`; `chamados/models.py:1552-1569`; `chamados/views_arquivar.py:163-166`.)

Quem lê essa via: busca do arquivo e resgate (`chamados/views_arquivar.py:64,165`), reabertura da competência (`chamados/services/competencia_trancada.py:259`) e reconciliação de perguntas, que evita tratar a tranca como resolução comum (`chamados/services/disputa_emissao.py:1572-1573`). Os testes são `chamados/tests/test_competencia_trancada.py::test_o_arquivo_lista_o_encerrado_pela_tranca`, linha 203, e `::test_MORDE_desarquivar_devolve_e_a_segunda_passada_respeita_o_admin`, linha 164. Essa modalidade não muda apenas com a cura do relógio de `silenciado_ate`. (`chamados/tests/test_competencia_trancada.py:164-175,203-209`.)

A serialização localizada é manual: a API do colaborador recebe o queryset da janela e monta dicionários; a mensageria publica a partição. Serializer dedicado ou `annotate` diretamente sobre `silenciado_ate`: **[nao verificado: não localizado]**. Há agregações que recebem a fila já filtrada, discriminadas abaixo. (`api/views.py:1016,1121-1145`; `api/views_mensageria.py:395,411-422`; `core/services/painel_op.py:119`; `escala/services/furos_diarios.py:212`.)

Na tabela final, **“sem caso vencido localizado” significa [nao verificado]**. Guardas por presença recebem **outro**, pois impedir repetir uma escrita não equivale a decidir que o vencido deve ficar fora da cobrança.

| Sítio | Escritor/leitor | Supõe (1)/(2)/outro | Teste que fixa | Muda com a cura? | [nao verificado]
|---|---|---|---|---|
| `chamados/models.py:84-95`; `chamados/migrations/0031_chamadocolaborador_silenciado_ate_and_more.py:18-29` | Declaração do campo/schema | Outro; comentário do modelo declara retorno após vencer, linhas 81-83 | Contrato em `core/tests/test_contract_escritores_estado.py:35` | Sem mudança de schema necessária para ler o relógio. |
| `chamados/services/arquivo.py:166-171` — `silenciar` | **Escritor direto:** atribui e salva data, autor e motivo | Outro: executa decisão do chamador; não decide merecimento, linhas 16-18 | `test_porta_arquivo_silencio.py::test_MORDE_silenciar_2x_com_a_mesma_entrada_nao_escreve_de_novo`, linha 314 | Não exige mudar a escrita. |
| `chamados/services/arquivo.py:201-208` — `levantar` | **Escritor direto:** limpa data/autor/motivo; pode carimbar resgate | Outro: limpeza explícita | `test_porta_arquivo_silencio.py::test_MORDE_levantar_2x_nao_recarimba_o_resgate`, linha 336 | Não exige limpeza para o relógio vencer; resgate continua ato distinto. |
| `chamados/services/arquivo.py:123,130-138` | Leitor para fotografia antes/depois e trilha | Outro: auditoria | `test_porta_arquivo_silencio.py::test_MORDE_levantar_pela_porta_deixa_trilha`, linha 233 | Não. |
| `chamados/services/arquivo.py:161-164,193-196` | Leitor de igualdade/presença para idempotência | Outro | Testes de idempotência nas linhas 314 e 336 | Não substituir por validade temporal: igualdade da entrada e limpeza são perguntas diferentes. |
| `chamados/services/arquivo.py:150-152` | Contrato da renovação, implementada nas linhas 160-171 | Outro: ausência pode renovar a cada passada | `test_s4_pendente_suspende_cobranca.py:52-59`; vencimento sem renovação: sem caso localizado | Uma guarda “silêncio ainda válido → não escrever” mudaria a renovação; não é exigência da cura. |
| `chamados/services/arquivo.py:117-118` | Leitor de `PRAZO_ARQUIVO_DIAS` | Outro: converte dias em data | Teste do prazo, linha 386 | Não. |
| `chamados/models.py:476-480` — `silenciado_em` | Leitor direto | **(1):** somente `> agora` | Caso direto do método: sem caso localizado | Já temporal; vencido retorna `False`. |
| `chamados/services/painel.py:186-188` — `qs_na_fila` | Leitor `.exclude(__gt=now)` | **(1)**, condicionado a `COBRANCA` | `test_f9_contador_bate_com_a_lista.py::test_silencio_VENCIDO_volta_sozinho`, linha 90 | Já é a regra pretendida. |
| `chamados/services/painel.py:213-214`; `chamados/services/fila.py:122-127` | Leitores `.filter/.exclude(__gt)` para contadores | **(1)** | `test_f9_contador_bate_com_a_lista.py:90-96` cobre a fila-base | Vencido já não conta como Arquivado. |
| `chamados/services/verbos.py:45-51` | Leitor `values_list`; troca verbo pela presença/data | **(2)**, quando verbo original não é `None` | **T1, T2** | Vencido conserva o verbo efetivo; sai do bucket `historico` e volta à pílula correspondente. |
| `chamados/services/painel.py:228,318-329` | Leitor indireto da partição e de `FORA_DA_LISTA_PADRAO` | **(2)** | **T3** | Vencido elegível reaparece na lista padrão e no filtro do verbo. `f_status` explícito segue seu ramo próprio. |
| `chamados/services/verbos.py:171-195` — `coletar_lente` | Leitor indireto da partição | **(2)** | T1/T2 fixam a dependência; lente com vencido: sem caso localizado | Vencido passa ao grupo/filtro do verbo efetivo. |
| `chamados/services/fila.py:55-67` — `por_verbo` | Leitor indireto para contagens e rótulos | **(2)** | T1/T2; consumidor com vencido: sem caso localizado | Redistribui `historico` para o verbo efetivo. |
| `chamados/services/janela_colab.py:67-74` | Leitor `.filter(silenciado_ate__isnull=True)` | **(2)** | `test_c1_janela_colab.py::test_arquivado_dentro_da_janela_tambem_sai`, linha 55, **só futuro** | Trocar por exclusão de futuros admite vencidos que satisfaçam janela/pergunta e módulos permitidos; não devolve indiscriminadamente todo chamado antigo. |
| `chamados/services/fio.py:169-176`; `chamados/views.py:569` | Leitor indireto da janela com `so_janela=True` | **(2)** | `test_c2_endpoints_janela.py::test_fio_com_so_janela_recorta`, linha 41, **só futuro** | Fio do colaborador passa a incluir vencidos elegíveis. Fio sem a flag já usa outro universo. |
| `api/views.py:1016,1121-1145` | Leitor indireto; serialização manual | **(2)** pela janela | `test_c2_endpoints_janela.py::test_api_v2_usa_o_juiz_unico`, linha 58, estrutural | JSON recebe os vencidos admitidos pela janela curada. |
| `chamados/views_arquivar.py:195-197` — `meus_arquivados` | Leitor indireto: complemento da janela | **(2)** herdado | `test_c3_meus_arquivados.py::test_traz_fora_da_janela_e_arquivado_mas_nao_o_da_lista`, linha 45, **só futuro** | Vencido que retorna à lista deixa esse complemento; demais excluídos continuam consultáveis. |
| `chamados/views_arquivar.py:165-166` — busca | Leitor `Q(isnull=False)` e ordenação `F(silenciado_ate)` | **Outro: acervo histórico**, inclui vencidos e via de tranca | **T6** | Pode permanecer igual com Histórico só como exibição. Trocar aqui presença por futuro retiraria vencidos da pesquisa e contrariaria T6. |
| `templates/chamados/partials/_busca_arquivados.html:46-49` | Leitor direto de exibição/data | Outro: **já lê relógio** | T6 cobre inclusão; texto exato: sem caso localizado | Pode conservar “Histórico” para vencido, sem determinar cobrança. |
| `templates/chamados/partials/_meus_arquivados.html:33-34` | Leitor de presença para rótulo `Arquivado` | Outro: exibição persistente | Vencido/rótulo: sem caso localizado | Ler relógio retiraria `Arquivado` do vencido; poderia exibir Histórico. |
| `templates/chamados/partials/_lista_chamados.html:85` | Leitor de presença para botão Resgatar | Outro: ação de limpeza/resgate | Vencido/botão: sem caso localizado | Com guarda temporal, vencido perde o botão; isso também impede esse caminho de limpeza/carimbo do resgate. |
| `chamados/views_arquivar.py:36-37,77` | Escritores indiretos: arquivar e levantar pela porta | Outro | `test_f3_arquivar_porta.py:42-50,65-72`; `test_c4_resgate.py:61-71` | Escritas podem permanecer. |
| `chamados/views_arquivar.py:124-140` — lote | Leitor da partição, guarda `isnull=True`, escritor indireto | **(2)** pela partição; **outro** na guarda de escrita | T1/T2 na dependência; `test_f4_arquivar_lote.py:44-57` não fixa vencido | Curar só verbos ainda deixa vencido protegido por `isnull=True`; curar também a guarda permite silenciá-lo novamente. |
| `chamados/management/commands/arquivar_prescritos.py:50,53-60,93-95` | Leitor da partição e duas guardas `isnull`; escritor indireto | **(2)** pela partição; **outro** nas guardas | `test_f6_prescricao.py::test_idempotente`, linha 54, só prazo ainda futuro | Curar partição e guardas torna vencidos sem resgate candidatos a novo silêncio; cura só da partição não basta para isso. |
| `chamados/management/commands/arquivar_competencia_encerrada.py:100,274-288` | Leitor `isnull`; escritor indireto com horizonte de dez anos | Outro: seleção/idempotência por presença | Vencido: sem caso localizado | Guarda temporal reabre seleção dos expirados e permite novo arquivamento, sujeitos aos demais critérios. |
| `chamados/management/commands/arquivar_sem_dia_irrecuperavel.py:59,88-94` | Leitor `isnull`; escritor indireto com horizonte de cinco anos | Outro: seleção/idempotência por presença | Vencido: sem caso localizado | Mesmo efeito: expirados voltam a ser selecionáveis. |
| `chamados/services/silencio_isentos.py:30,39-47,55-58` | Leitor `isnull`; escritor indireto pela porta | Outro: não retoca quem já tem data, mesmo vencida | `test_varredura_isentos_evento.py::test_idempotente_uma_linha_de_trilha`, linha 87; vencido: sem caso localizado | Trocar guarda por relógio transforma expirados em novos alvos; execução renova silêncio. |
| `chamados/signals.py:256-262` | Leitor do nome em `update_fields` e da presença no objeto | Outro: evento/guarda contra repetição | `test_varredura_isentos_evento.py::test_MORDE_o_resgate_de_isento_volta_ao_veu_pelo_evento`, linha 57; vencido: sem caso localizado | Alterar só linha 261 permite avançar com vencido, mas `silencio_isentos.alvos` ainda o barra. |
| `chamados/signals.py:242,269` | Escritores indiretos via `silenciar_do_colab` | Outro: eventos de colaborador/chamado | `test_varredura_isentos_evento.py:41-68` | Herdam eventual alteração da seleção dos isentos. |
| `chamados/management/commands/silenciar_chamados_isentos.py:36-49` | Leitor/escritor indireto | Outro: contador/vigia ou `--apply` | `test_varredura_isentos_evento.py::test_o_cron_virou_VIGIA_alarme_nao_escreve_e_sai_2`, linha 96 | Se alvos passar a ler relógio, expirados alarmam; com `--apply`, recebem novo silêncio. |
| `chamados/services/silencio_ausencia.py:66,80-91` | Leitor temporal, do marcador; escritor indireto de suspensão/renovação | **(1)**: silêncio futuro alheio manda; vencido pode ser substituído | `test_s4_pendente_suspende_cobranca.py:52-59,93-107`; vencido específico: sem caso localizado | Guarda já temporal. Ausência aberta renova prazo pela porta; não se deve confundir renovação com arquivo persistente. |
| `chamados/services/silencio_ausencia.py:68-75,93-115,127` | Escritor indireto: levanta ou reconcilia | Outro: propriedade pelo motivo e decisão da ausência | `test_s4_pendente_suspende_cobranca.py::test_MORDE_rejeitar_levanta_so_o_marcador`, linha 93 | Aprovação/rejeição seguem seus critérios; relógio não substitui essa reconciliação. |
| `ponto/signals.py:265-269`; `ponto/management/commands/reavaliar_ausencias_lancadas.py:35,54` | Chamadores indiretos da reconciliação de ausência | **(1)/outro**, herdados | `test_s4_pendente_su

[truncado em 20000 de 27428 caracteres]

## 06/11 19:41 · sessao ab4f7e154a11 · - · - · origem=Ronald (tmux)

O histórico por `git log -S` não está disponível nesta cópia: o comando retorna `fatal: not a git repository`; commit/data de inclusão de `'ausencia'` na contagem ficam `[nao verificado]`. A migration somente documenta materialização do previsto e cria o campo com rótulo “Dias de trabalho previstos”; não estabelece filtro por `trabalha` (`app/ponto/migrations/0044_fechamentomensal_minutos_previstos.py:5`, `:21`).

Há documento explícito para o significado **célula diz trabalho**: O155 determina `FechamentoMensal.dias_previstos == dias que a célula diz trabalho`, mas registra o selo como “nao comecada” (`app/docs/BACKLOG.md:1365`); CORTES registra o selo não construído (`app/docs/CORTES.md:200`). Não encontrei teste que compare diretamente `ausencia + trabalha=False` contra `ausencia + trabalha=True` para `FechamentoMensal.dias_previstos` `[nao verificado]`.

| Sítio | Escritor/leitor | Significado suposto | Dinheiro? | Teste | [nao verificado]
|---|---|---|---|---|
| `app/ponto/services/fechamento.py:1342`, `:1358`, `:1364`, `:1375` | Escritor genérico `restaurar_fechamento` | Valor conhecido recebido; não deriva semântica | Pode restaurar outros campos financeiros; `dias_previstos` é snapshot | `[nao verificado]` |
| `app/ponto/management/commands/aplicar_09_corte_b.py:67`, `:184`, `:231`, `:265` | Lê/fotografa/compara e pode restaurar `dias_previstos` | Sem significado próprio; preserva valor recebido | Mudança pode separar colaborador e provocar restauração integral | `[nao verificado]` |
| `app/ponto/management/commands/aplicar_09_he_intra.py:43`, `:125` | Leitor da fotografia | Sem significado próprio | `dias_previstos` está fora de `ALVO` (`:39`); escrita só do alvo (`:137`) | `[nao verificado]` |
| `app/ponto/management/commands/recalcular_fechamento.py:34`, `:54`, `:57`, `:142` | Leitor genérico `foto`/diff | Campo numérico recebido | Medição; não fórmula financeira | `test_o208_contador_do_recalculo.py::ContadorDoRecalculoTest.test_MORDE_os_cinco_campos_da_divergencia` (`:32`, `:71`) fixa visibilidade da mudança, não significado |
| `app/ponto/services/diff_frota.py:62`, `:84`, `:111` | Leitor genérico | Copia/compara campos solicitados | Medição | `[nao verificado]` |
| `app/ponto/management/commands/diff_janela_he_total.py:87`, `:101`, `:110`; `diff_passivo_s84.py:42`, `:50`, `:87`; `diff_col900.py:38`, `:45`, `:74` | Leitores por `CAMPOS`/`diff_frota` | Sem significado próprio | Medição | `[nao verificado]` |
| `app/ponto/management/commands/carimbo_gravado.py:34`, `:53`, `:87` | Leitor/fotografia/diff | Valor gravado | Auditoria | `[nao verificado]` |
| `app/ponto/management/commands/aplicar_janela_he_total.py:65`, `:75`, `:87` | Leitor indireto por `carimbo_gravado.foto` | Valor gravado | Auditoria do ato | `[nao verificado]` |
| `app/ponto/management/commands/lavrar_dias_pagos.py:68`, `:73` | Leitor em hash de todo campo concreto | Valor gravado; não conta dias | Protege fotografia | `[nao verificado]` |
| `app/ponto/portas/regen_exportada.py:56`, `:71`, `:77`, `:84` | Leitor em `hash_do_gravado` e `valores_do_gravado` | Valor gravado | Auditoria/Pauta/retificação | `[nao verificado]` |
| `app/ponto/management/commands/e6_piso_autopsia.py:120` | Leitor/impressão | Valor gravado | Medição | `[nao verificado]` |
| `app/escala/services/propositor.py:484`, `:511`, `:559` | Escritor de dict homônimo independente | **CelulaDia.trabalha=True**, recortado aos dias do vínculo | Conta para proposta de escala, sem leitura do FM | `[nao verificado]` |
| `app/ponto/services/dia_pago.py:34`, `:43`, `:47`, `:99` | Não possui `dias_previstos` em `CAMPOS`; comenta contagem | Contagem derivada das linhas, distinta dos minutos | Excluído do censo de pagamento | `app/folha/tests/test_porta_do_export.py::test_MORDE_o_contador_do_fechamento_NAO_e_cego_a_campo_de_pagamento` (`:454`, `:467`, `:479`) exclui contagens; não fixa filtro |

Os scripts genéricos também carregam o campo sem interpretar seu significado:

- Snapshots: `bin/snapshot_relavratura_10_2026.py:29`, `bin/snapshot_o263_comp10.py:54`.
- **Escritores diretos de snapshot**: `bin/restore_relavratura_10_2026.py:34`, `:79`; `bin/restore_o263_comp10.py:27`, `:48` — fazem `setattr` e `save(update_fields=VALOR)`.
- Recalcula e compara: `bin/aplicar_o263_comp10.py:28`, `:34`, `:51`, `:59`.
- Diffs genéricos: `bin/diff_o118_col221.py:23`, `:37`; `bin/diff_o119_frota.py:27`, `:42`; `bin/diff_o124_frota.py:31`, `:38`, `:55`.
- Snapshot/hash/impressão/recalculo: `bin/relavrar_o118_col221.py:26`, `:30`, `:37`, `:49`, `:53`; `bin/relavrar_o119_col221.py:25`, `:29`, `:36`, `:48`, `:52`; `bin/relavrar_o120_col221_09.py:29`, `:32`, `:41`, `:62`, `:66`, `:74`, `:79`.

Os testes explícitos sobre o valor de `dias_previstos` fixam **ausência de resposta versus zero**, não os dois significados discutidos: [nao verificado]

- `app/ponto/tests/test_o48_sem_celula_nao_e_zero.py::SemCelulaNaoEZero.test_MORDE_sem_nenhuma_celula_o_objeto_nao_diz_zero_minuto_de_quem_trabalhou`: sem célula → `None` (`:96`, `:110`).
- Mesmo arquivo `::test_MORDE_admitido_no_meio_sem_celula_no_vinculo_tambem_nao_diz_zero`: mistura `fora_vinculo + sem_celula` → `None` (`:153`, `:178`).
- Mesmo arquivo `::test_fora_do_vinculo_INTEIRO_e_RESPOSTA_zero_nunca_silencio`: janela inteira `fora_vinculo` → `0` (`:185`, `:207`).
- `app/relatorios/test_espelho_falta.py::EspelhoFaltaAbonoTest.test_falta_e_abono_surfam_no_resumo` usa `_dias_previstos = ec.eh_dia_trabalho` (`:28`, `:33`, `:41`), mas verifica somente faltas e abonos (`:55`).

Há testes da **classificação**, sem fixar contagem mensal: [nao verificado]

- `app/escala/tests/test_cobparc6_ausencia_sem_vinculo.py::AusenciaSemVinculoPorTurno.test_MORDE_propriedade_ausencia_nao_falta_com_trabalha_false_nao_vira_folga_na_ata`, também herdado por `AusenciaSemVinculoLegacy`: confirma `trabalha=False` e ata `'ausencia'` (`:83`, `:89`, `:91`, `:94`, `:99`).
- `app/ponto/tests/test_retrato_exportada.py::PorDiaDaGradeTest.test_MORDE_as_tres_leis` fixa inclusão de `tipo_dia='ausencia'` em mapa de **minutos previstos**, exclusão de `'folga'` e cap realizado (`:46`, `:49`, `:58`); a fixture não fornece `trabalha`.
- `app/escala/tests/test_montador_realizado_pela_autoridade.py::test_MORDE_a_folga_trabalhada_NAO_entra_no_dinheiro` fixa exclusão de `'folga'` dos três mapas e conservação das somas (`:454`, `:479`, `:484`, `:489`).

## 06/11 19:40 · sessao 7c3ed4001837 · - · - · origem=Ronald (tmux)

**4. `calcular_mes` recebe bordas e filtra períodos depois do pareamento.**

- `MotorBase.calcular_mes` copia os objetos de entrada (`app/ponto/motor_calculo_v2.py:2268`; cópia rasa de cada objeto em `:2263`), entrega a lista ao juiz (`:2346`) e a `remover_duplicatas`/`montar_periodos` (`:2351`, `:2354`). `remover_duplicatas` filtra tipos E/S e duplicatas, sem recorte por data (`:902`, `:908-911`).
- O recorte inclusivo `[data_ini, data_fim]` ocorre **sobre cada período**, descartando se `data_jornada < data_ini` ou `data_jornada > data_fim` (`app/ponto/motor_calculo_v2.py:2429-2432`).
- `data_jornada` parte do `data_turno` fornecido pelo juiz; sem chave, usa `localizar(entrada).date()` (`app/ponto/motor_calculo_v2.py:2403-2404`). O mapa liga cada E ao `t.data_turno` (`:889-894`).
- Depois pode herdar o dia do período anterior se o intervalo entre sua entrada e a saída anterior for de 0 a 6 horas (`app/ponto/motor_calculo_v2.py:2405-2409`); só registra saída anterior quando existe (`:2423`).
- Portanto, uma batida com dia civil fora da janela pode contribuir para um período cujo **dia da jornada** esteja dentro; a guarda não recorta cada batida por seu `.date()` (`app/ponto/motor_calculo_v2.py:2354`, `:2403-2409`, `:2429-2432`).
- O fallback é **dia civil local**, não UTC: `TZ_LOCAL = pytz.timezone(settings.TIME_ZONE)` (`app/ponto/motor_calculo_v2.py:28`); `localizar` atribui o fuso local ao naive e converte aware com `astimezone` (`:73-79`).
- `MotorTurnoPartido.calcular_mes` e `MotorComercial.calcular_mes` passam primeiro pelo `super().calcular_mes` (`app/ponto/motor_calculo_v2.py:2908`, `:3301`). Seus agrupamentos posteriores também usam `data_turno`, com fallback na data local da entrada (`:2941-2942`, `:3312-3314`).

**5. Funções públicas candidatas.**

| Função | Contrato/retorno | Chamadores diretos de produção |
|---|---|---|
| `ponto.turnos.data_local(ts)` | `timezone.localtime(ts).date()`, ou `None`; docstring exige instante aware (`app/ponto/turnos.py:43-46`). Não protege naive (`:46`). | `ponto.signals._recompute_around`: `app/ponto/signals.py:21-23`; comando de produção `detectar_par_relampago`: `app/ponto/management/commands/detectar_par_relampago.py:302-303`; internos abaixo. |
| `core.filtro.dia_de(valor)` | Aware → data local (`app/core/filtro.py:131-133`); naive/erro de conversão → `valor.date()` (`:134-135`); aceita também `date` e `None` (`:129-130`, `:136-138`). | `core.filtro.aplicar`, `app/core/filtro.py:194`. O caminho efetivamente usado para datetimes de holerite vem de `avisado_em`/`publicado_em`, `app/holerite/views.py:407-411`. |

Chamadas internas de `data_local`, todas em produção:

- `_data_do_turno`: `app/ponto/turnos.py:284`.
- `_mk`: `app/ponto/turnos.py:320`.
- `_saida_pela_posicao_entre_turnos`: `app/ponto/turnos.py:1013`.
- `_fechar_aberto_com_saida_seguinte`: `app/ponto/turnos.py:1098`.
- `_fechar_aberto_na_pausa_sem_volta`: `app/ponto/turnos.py:1186`.
- Função aninhada `_vespera` de `parear_turnos`: `app/ponto/turnos.py:1294`.
- `par_completo`: `app/ponto/turnos.py:2198`.
- `_dia_das_batidas_puro`: `app/ponto/turnos.py:2918`.

Candidatas próximas, mas com contrato diferente:

- `core.fmt.hora_local(dt, fmt=...)` converte aware para hora local e devolve **string**, não `date` (`app/core/fmt.py:10-15`). Exemplos de chamadores: `app/chamados/services/materializacao.py:450`, `app/ponto/services/regularizacao.py:139`, `app/ponto/services/justificativa.py:386`, `app/ponto/models.py:1024`.
- `ponto.turnos.dia_das_batidas` responde **dia atribuído à batida**: `data_turno` quando reclamada por turno e data civil local para as restantes (`app/ponto/turnos.py:2874-2881`, `:2912-2918`), logo não é helper puro “dia local de instante”. Chamadores diretos: `app/ponto/services/espelho.py:450-451`, `app/api/views.py:1445-1446`; há também import com alias em `app/api/views.py:2037`.

**Complemento útil para a pergunta 6:** o motor prevê explicitamente timestamps naive, normalizando-os no seu caminho de pareamento (`app/ponto/motor_calculo_v2.py:458-461`). `turnos_de_batidas` também normaliza naive nas cópias (`app/ponto/turnos.py:1524-1529`). Isso comprova suporte defensivo a naive; não comprova, sozinho, uma origem naive alcançando `apurar_por_fatia` [nao verificado].

## 06/11 19:40 · sessao 2d0d6aab18c0 · - · - · origem=Ronald (tmux)

**Dinheiro:** mudar somente a contagem `len(_dt_trab)` de `FechamentoMensal.dias_previstos` não altera DSR, semanas DSR, faltas ou proporcionalidade: esses valores vêm de conjuntos de datas e minutos independentes; `dias_previstos` só participa do teste de denominador zero da prontidão. (`app/ponto/services/fechamento.py:722-731`, `:1033`, `app/ponto/motor_calculo_v2.py:2593-2655`, `:3425-3429`, `app/folha/export.py:1042-1043`.)

| Sítio | Papel / significado | Dinheiro / impacto |
|---|---|---|
| `app/folha/export.py:1037-1072` | Leitor direto de `fech.dias_previstos`; exige valor diferente de zero para aceitar snapshot. O percentual usa `minutos_previstos`, `minutos_realizados` e `minutos_abonados`. | Só muda o estado da prontidão se a nova contagem chegar a zero com minutos positivos; redução entre valores positivos não muda percentual. (`:1042-1043`, `:1068-1072`.) |
| `app/folha/export.py:1087-1105` | Fallback repete a mesma contagem: `tipo_dia in ('trabalho','ausencia')`; `ndias=len(_dt)` apenas no teste zero. | Percentual calculado em minutos; ausências entram como abono. (`:1088-1098`, `:1100-1105`.) |
| `app/ponto/services/fechamento_lista.py:85-86`, `:111-117`, `:127-130` | Tela consome prontidão; dela tira faixa e ordenação. | Efeito de apresentação, não cálculo de rubrica. O tique de folha vem separadamente do juiz de export em `:88-102`. |
| `app/ponto/services/fechamento.py:697-731` | Outro significado de previsto: conjunto `datas_previstas_trabalho` alimentado por `esc.eh_dia_trabalho(cursor_data)` e exclusão de datas suprimidas. | Alimenta motor, DSR e faltas; não lê FM `dias_previstos`. (`:722-731`.) |
| `app/ponto/services/fechamento.py:674-692` | Cobertura de ausência decide justificados e suprimidos; abona adiciona em `datas_justificadas`, demais efeitos que não descontam suprimem previsto. | Determina o que é obrigação para DSR; sem dependência da contagem do snapshot. (`:689-692`, `:722`.) |
| `app/ponto/motor_calculo_v2.py:2593-2622` | DSR olha dias obrigatórios contidos em `datas_previstas_trabalho`; falta injustificada = obrigatório sem trabalho e sem justificativa; falta/abono custam minutos dos respectivos dias. | Dinheiro: `horas_falta` / `horas_abono` intermediários e semanas DSR; não lê `dias_previstos`. |
| `app/ponto/motor_calculo_v2.py:2630-2655` | Reflexo DSR usa HE semanal, dias efetivamente trabalhados e dias do período. | Não usa contagem trabalho+ausência da grade. |
| `app/ponto/motor_calculo_v2.py:3437-3479` | Motor comercial zera e recalcula DSR com o mesmo conjunto de dias obrigatórios. | Mesma independência de FM `dias_previstos`; reflexo usa dias trabalhados. |
| `app/ponto/motor_calculo_v2.py:3425-3429` | Proporcionalidade semanal do banco já usa `eh_dia_trabalho(...) is True`, fração dos dias previstos da semana dentro do período. | Dinheiro: saldo banco; não usa ata `tipo_dia` nem FM `dias_previstos`. |
| `app/ponto/services/fechamento.py:864-866`, `:987` | `horas_falta` FINAL do FM vem da soma de `export.minutos_de_falta_por_dia`, não das horas de falta intermediárias do motor. | Independente da contagem do snapshot. |
| `app/folha/export.py:125-143`, `:169-185`, `:215-220` | Falta decidida vem de ausências aprovadas de catálogo; minutos rateados e datas expandidas pelo juiz. Bloco TXT conta essas datas. | Falta TXT não lê FM `dias_previstos`. Rateio tem denominador da expansão do registro em `:114-122`. |
| `app/folha/export.py:722-736` | TXT emite eventos de `eventos_do_fechamento` e faltas de `datas_falta`. | Lista explícita em `:439-461` não inclui `dias_previstos`; DSR não é emitido (`:453-460`). |
| `app/folha/pdf_export.py:63-65`, `:78`, `:173-185` | PDF traduz o conteúdo TXT gravado. | Não recálcula FM ou dias; valor é o que já está no TXT. |
| `app/ponto/services/dia_pago.py:384-400` | Mesmo universo da contagem: trabalho+ausência, mas deriva mapas de minutos. Ausência dá `abono=previsto-min(realizado,previsto)`. | Usa a mesma seleção, mas não `len` nem FM `dias_previstos`. Se mudar **só a contagem**, estes mapas permanecem iguais. |
| `app/ponto/services/dia_pago.py:343-356` | Linhas DiaPago recebem mapas/rubricas; ajuste recebe reflexo DSR e banco. | Não existe `dias_previstos` nos valores lavrados; o comentário exclui explicitamente a contagem do censo em `:99-100`. |
| `app/folha/causas.py:39-45` | Dict homônimo `dias_previstos` significa somente dias com `tipo_dia=='trabalho'`, excluindo todas as ausências. | Classificação de causa; não é a mesma conta do FM. FM salva o dict em `fechamento.py:1061-1064`; tela o transporta em `fechamento_lista.py:122`. |
| `app/escala/services/propositor.py:485-488`, `:511`, `:559` | Outro dict homônimo: dias de `CelulaDia.trabalha=True` dentro dos dias do vínculo analisado. | Proposta diagnóstica de escala; não lê FM nem calcula rubrica nesse trecho. |

**Escopo do impacto:** trocar só o comprimento em `fechamento.py:1033` difere de alterar `_dt_trab` ou a regra da ata. O filtro também alimenta `_min_prev` e `_dias_inc` em `:1032-1034`; o cálculo de DiaPago tem seu próprio filtro em `dia_pago.py:385-386`. Com `minutos_previstos=0` e realizado não negativo nos dias retirados, os somatórios de previsto, abono e realizado não mudam, pela fórmula de `dia_pago.py:391-399`; os mapas podem perder chaves/linhas zero. Todas essas condições nos dias reais da amostra e os valores exatos de impacto são `[nao verificado]`, pois o anexo fornecido não contém a sonda diária.

Testes existentes pertinentes, sem afirmar execução: [nao verificado]

- `app/ponto/tests/test_fechamento_ausencia.py::test_atestado_semana_nao_perde_dsr` — atestado em dia útil mantém zero semanas perdidas (`:126-149`).
- `app/ponto/tests/test_fechamento_ausencia.py::test_afastamento_suprime_previsto` — afastamento integral mantém zero semanas perdidas (`:151-164`).
- `app/ponto/tests/test_fechamento_ausencia_pela_autoridade.py::test_MORDE_falta_lancada_depois_do_atestado_vence_o_dia` — uma semana perdida (`:57-65`); `::test_atestado_lancado_depois_da_falta_abona` — zero (`:67-74`).
- `app/folha/tests/test_export_dsr_reflexo.py::test_dsr_nao_e_emitido` — DSR fora do TXT (`:32-36`).
- `app/folha/tests/test_prontidao_cap_dia.py::test_he_nao_tapa_furo` — fixa prontidão por minutos com cap por dia (`:31-55`), não conta ausência em folga.
- `app/folha/tests/test_porta_do_export.py::test_MORDE_o_contador_do_fechamento_NAO_e_cego_a_campo_de_pagamento` — documenta e exclui contagens do censo de dinheiro (`:467-481`); não fixa o significado da contagem.
- Nos testes de causas consultados não há assert de `dias_previstos`; o algoritmo fixa trabalho-only (`app/folha/causas.py:41-45`), mas teste explícito dessa chave é `[nao verificado]`.

## 06/11 19:40 · sessao b2392547d996 · - · - · origem=Ronald (tmux)

**6. Não encontrei contraexemplo comprovado de batida NAIVE chegando a `apurar_por_fatia`; a impossibilidade geral permanece `[nao verificado]`.**

- Os três chamadores de produção carregam uma lista do ORM por `batidas_apuraveis`: `app/ponto/services/fechamento.py:623-624`, `app/ponto/services/espelho.py:1019-1022`, `app/relatorios/pdf_espelho.py:264-272`; passam essas listas à porta em `fechamento.py:745`, `espelho.py:1164`, `pdf_espelho.py:419`.
- `batidas_apuraveis` retorna `QuerySet` de `Batida`, sem construir objetos substitutos (`app/ponto/turnos.py:2863-2871`); `timestamp` é `DateTimeField` (`app/ponto/models.py:23`), com `USE_TZ=True` (`app/config/settings/base.py:95`). Os settings de produção herdam a base e usam PostgreSQL (`app/config/settings/prod.py:1`, `:9-17`; `app/config/settings/saas.py:10`, `:83-93`).
- A porta, entretanto, não valida `tzinfo`: recebe `batidas` e usa diretamente `.timestamp.date()` (`app/ponto/services/fechamento.py:132`, `:244-253`). Portanto, aceitar uma coleção artificial com datetimes naive é uma possibilidade da API, não um fluxo de produção encontrado (`app/ponto/services/fechamento.py:132`, `:244-253`).
- As cinco chamadas diretas nos testes passam `batidas=[]`: `app/ponto/tests/test_composicao_de_fatias_e_total.py:86-88`, `:104-106`, `:127-129`, `:165`, `:197`.
- Existem stubs naive, mas destinados ao motor diretamente: a construção sem `tzinfo` está em `app/ponto/tests/test_selo_motor_nao_pareia_pelo_tipo_gravado.py:94-95`, e a chamada é `_periodos_pelo_marco`, em `:99`; outro caso aparece em `:133-135`.
- Há defesas para naive abaixo da porta: `turnos_de_batidas` copia os objetos e aplica `make_aware` (`app/ponto/turnos.py:1524-1529`); `motor.localizar` localiza datetime sem `tzinfo` (`app/ponto/motor_calculo_v2.py:73-79`). Essas defesas não demonstram uma entrada naive em `apurar_por_fatia` `[nao verificado]`.
- `_so_a_data` também aceita naive (`app/ponto/services/fechamento.py:1551-1554`), mas transforma o timestamp em `date` para `competencias_do_fato` (`:1578-1583`); isso não comprova passagem do objeto de batida naive à lista apurada `[nao verificado]`.

## 06/11 19:40 · sessao fced003bd9d8 · - · - · origem=Ronald (tmux)

**Origem de `ata.tipo_dia='ausencia'` em célula de folga**

- O escritor semântico é `escala/utils.py::_marcar_ausencias`: obtém cobertura por pessoa com `cobertura_ausencia_periodo(..., para='folha')` (`app/escala/utils.py:1209`); a condição aceita dia anteriormente `folga` desde que a ausência não seja `falta` (`app/escala/utils.py:1231`); escreve `dia['tipo_dia']='ausencia'` (`app/escala/utils.py:1241`). Não consulta `CelulaDia.trabalha` nesse overlay (`app/escala/utils.py:1225`).
- A cobertura expande **todos os dias corridos** do intervalo, sem filtro de trabalho/folga (`app/ponto/turnos.py:2753`, `app/ponto/turnos.py:2766`); para folha seleciona apenas aprovada (`app/ponto/turnos.py:2514`).
- O builder monta primeiro folga com `minutos_previstos=0` (`app/escala/utils.py:1822`), depois aplica folgas e ausências nessa ordem (`app/escala/utils.py:1865`, `app/escala/utils.py:1869`). O overlay altera tipo/cobertura/células, mantendo os minutos prévios (`app/escala/utils.py:1231`, `app/escala/utils.py:1273`).
- Cartório monta a grade (`app/ponto/services/cartorio.py:1126`), copia tipo e minutos para ata (`app/ponto/services/cartorio.py:704`), lavra no ramo normal (`app/ponto/services/cartorio.py:1800`) e no ramo nunca_bateu (`app/ponto/services/cartorio.py:1210`, `app/ponto/services/cartorio.py:1242`).
- A porta efetiva da escrita é `ponto.portas.celula.lavrar_veredito`: copia a ata (`app/ponto/portas/celula.py:990`), atribui `cel.ata` (`app/ponto/portas/celula.py:1028`) e salva (`app/ponto/portas/celula.py:1035`). Mesmo veredito de admin preservado pode receber ata nova do cartório (`app/ponto/portas/celula.py:998`).

**Juiz e distinção preservada**

- `PRECEDENCIA` põe férias/afastamento/atestado antes de folga (`app/ponto/precedencia.py:46`). O juiz lê `cel.trabalha` e o usa em `previsto` (`app/ponto/precedencia.py:347`, `app/ponto/precedencia.py:363`), adiciona ausência e folga aos fatos (`app/ponto/precedencia.py:379`, `app/ponto/precedencia.py:384`) e conserva separadamente `era_dia_previsto` e `suprimidos` (`app/ponto/precedencia.py:396`, `app/ponto/precedencia.py:407`).
- Esse resultado vai a `ata.precedencia`, **não** é o escritor direto de `ata.tipo_dia` (`app/ponto/services/cartorio.py:1948`, `app/ponto/services/cartorio.py:1949`).
- `grade_da_celula` conserva `ata.tipo_dia` mesmo quando `cel.trabalha=False` (`app/escala/services/leitor_celula.py:380`). Lê minutos lavrados; se estiverem ausentes, pergunta à autoridade somente quando `cel.trabalha` é verdadeiro; senão usa zero (`app/escala/services/leitor_celula.py:382`, `app/escala/services/leitor_celula.py:394`).
- A autoridade `minutos_previstos_do_dia` retorna zero quando `eh_dia_trabalho(...) is not True` (`app/escala/utils.py:2287`).
- `dia_sem_trabalho` distingue explicitamente por `cel.trabalha is False`; na ausência parcial, `trabalha=True` permite cobrar o resto e `False` cala (`app/chamados/juizes.py:426`). Ausência integral cala cobrança independentemente de ser originalmente trabalho ou folga (`app/chamados/juizes.py:428`).
- Há leitor de diagnóstico que expõe ambos `escala.previa_trabalho` e `celula.trabalha` (`app/ponto/services/dia_do_colab.py:85`, `app/ponto/services/dia_do_colab.py:91`).
- `por_dia_da_grade` inclui `trabalho/ausencia`, mas soma **minutos**, com `min(realizado, previsto)`, e abono `previsto-coberto`: ausência em folga com previsto zero aporta zero a esses valores (`app/ponto/services/dia_pago.py:385`, `app/ponto/services/dia_pago.py:391`).

**Testes encontrados, não executados** [nao verificado]

| Teste | O que fixa | [nao verificado]
|---|---|
| `app/escala/tests/test_grade_segue_a_precedencia.py::GradeSegueAPrecedenciaTest.test_MORDE_atestado_aprovado_em_dia_de_folga_e_ausencia` | Folga vira ausência no overlay (`app/escala/tests/test_grade_segue_a_precedencia.py:43`). |
| `app/escala/tests/test_grade_segue_a_precedencia.py::GradeSegueAPrecedenciaTest.test_falta_aprovada_em_dia_de_folga_segue_folga` | Falta aprovada não muda folga (`app/escala/tests/test_grade_segue_a_precedencia.py:48`). |
| `app/escala/tests/test_leitor_celula_minutos_pela_autoridade.py::LeitorCelulaMinutosPelaAutoridadeTest.test_celula_de_folga_preve_zero` | Fallback de previsto de folga é zero (`app/escala/tests/test_leitor_celula_minutos_pela_autoridade.py:49`). |
| `app/escala/tests/test_leitor_celula_minutos_pela_autoridade.py::LeitorCelulaMinutosPelaAutoridadeTest.test_minutos_lavrados_na_ata_vencem` | Minutos da ata vencem o fallback (`app/escala/tests/test_leitor_celula_minutos_pela_autoridade.py:45`). |
| `app/ponto/tests/test_a2_precedencia.py::FolgaDeTemplateSuprimidaTest.test_MORDE_a_ata_da_celula_diz_que_a_folga_foi_suprimida` | Célula `trabalha=False` com férias registra férias e folga suprimida em `ata.precedencia` (`app/ponto/tests/test_a2_precedencia.py:466`, `app/ponto/tests/test_a2_precedencia.py:471`, `app/ponto/tests/test_a2_precedencia.py:481`). |

Esses testes fixam natureza/previsto em minutos; não fixam diretamente `FechamentoMensal.dias_previstos` para ausência em folga (`app/escala/tests/test_grade_segue_a_precedencia.py:43`, `app/escala/tests/test_leitor_celula_minutos_pela_autoridade.py:49`).

## 06/11 19:38 · sessao f4d553ce3cb5 · - · - · origem=Ronald (tmux)

Censo do recorte solicitado, somente por leitura; nenhum teste executado [nao verificado]. [nao verificado]

**Achados que afetam a decisão**

- Há dependência histórica real de `disputa_id`: `perguntas_absorvidas_por()` identifica um ato de absorção por `(disputa_id, materializada_em)`, buscando o escape respondido e suas irmãs; mover apenas a irmã muda pode fazê-la sair desse grupo, impedindo sua devolução quando a ausência for rejeitada. A guarda “sem resposta” não exclui uma irmã muda já absorvida. Fontes: `app/chamados/services/materializacao.py:531`, `:585`, `:605`, `:609`, `:614`, `:647`, `:658`. Teste existente cobre devolução da irmã muda, sem reaponte: `app/chamados/tests/test_o261_e1_ausencia_rejeitada_devolve.py:88`; teste com reaponte [nao verificado].
- O signal de pergunta captura somente `instance.disputa_id` depois do save; não guarda nem visita o ID anterior. A porta de beco ignora disputas fechadas. Assim, um save do reaponte não reabre nem recompõe a origem fechada que ficou vazia: `app/chamados/signals.py:191`, `:195`, `:202`; `app/chamados/services/becos.py:25`, `:27`, `:67`.
- O auditor I6 parte das perguntas precisamente para excluir disputas fechadas sem pergunta; portanto a origem vazia não será acusada por I6. A auditoria de sincronismo `motivos ↔ perguntas` só percorre abertas: `app/chamados/management/commands/auditar_invariantes_chamados.py:715`, `:737`; `app/chamados/management/commands/auditar_disputas.py:166`, `:182`.
- Reabrir disputa e reapontar FK são atos diferentes. Os caminhos existentes de reabertura abaixo leem a FK atual da pergunta e reabrem essa disputa; depois do reaponte não reencontram a origem vazia: `app/chamados/services/materializacao.py:660`, `app/chamados/services/relavra_via.py:243`, `app/chamados/services/validacao.py:191`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:153`.
- A validação/recusa recebendo `(pergunta_id, disputa anterior)` passa a retornar `nao_encontrada`; isso é efeito de vínculo atual, não quebra de integridade: `app/chamados/services/validacao.py:355`, `:359`, `:361`, `:526`, `:531`, `:533`.
- A resposta futura passa a produzir ocorrência no chamado-pai da disputa nova; os textos antigos não são migrados por esse leitor/escritor: `app/chamados/services/disputa_emissao.py:1344`, `:1349`, `:1355`. A cobrança também passa a consultar prazo e pai atuais: `app/chamados/services/cobranca.py:51`, `:61`, `:84`.

**Tabela de serviços**

| Sítio | Escritor/leitor | Supõe pertença fixa? | Efeito do reaponte | Teste que fixa | [nao verificado]
|---|---|---|---|---|
| `services/materializacao.py:531`, `:567`, `:1344`, `:1393` | Lê irmãs/FK/reverse; escreve batidas, carimbos e fechamento | Trabalha com a composição atual | Nova disputa ganha unidade de materialização; origem perde a pergunta. Fechamento conta vivas atuais e só fecha aberta (`:1289`, `:1291`) | `tests/test_validar_materializa_wiring.py:51`; `tests/test_fechar_disputa_materializada.py:51`; reaponte [nao verificado] |
| `services/materializacao.py:585`, `:609`, `:614` | Leitor de vínculo histórico da absorção | **Sim**, grupo usa FK como parte da identidade do ato | Irmã muda movida pode deixar de ser devolvida na rejeição | `tests/test_o261_e1_ausencia_rejeitada_devolve.py:88`; reaponte [nao verificado] |
| `services/materializacao.py:660`, `:686` | Leitor; reabre disputa/desarquiva chamado | FK atual/fallback ao pai atual | Reabre nova disputa, ou usa pai novo no resgate | `tests/test_o261_e1_ausencia_rejeitada_devolve.py:99`; reaponte [nao verificado] |
| `signals.py:191`, `:195`; `services/becos.py:29`, `:45`, `:67` | Leitor; dispara fechamento | Só conhece FK atual; não registra origem | Avalia nova disputa no commit; fechada anterior é ignorada (`becos.py:27`) | `tests/test_varredura_becos_evento_pergunta.py:47`, `:60`, `:81`, `:94` |
| `signals.py:124`, `:139` | Gatilho da materialização no fechamento | Composição atual | Um fechamento posterior da nova disputa alcança a pergunta movida; save de pergunta não equivale a save da origem | `services/materializacao.py:1393`; teste de reaponte [nao verificado] |
| `services/validacao.py:138`, `:191`, `:249`, `:407`, `:424`, `:430`, `:458`, `:471`, `:531`, `:548`, `:566`, `:699`, `:795`, `:828`, `:847` | Leitor; valida/recusa/materializa/loga | Autoriza por vínculo atual; fallback e reabertura usam pai/FK atuais | Contexto novo; requisição amarrada à disputa antiga falha (`:355`, `:526`). Recusa reabre somente disputa atual (`:199`) | `tests/test_validar_materializa_wiring.py:51`; reaponte [nao verificado] |
| `services/acoes_disputa.py:95`, `:97`, `:129`, `:260`, `:338`, `:343`, `:350`, `:375` | Leitor; ações de questionário/reabertura/criação | Composição atual | Contador perde/ganha pergunta; origem vazia pode responder “Nenhuma pergunta cadastrada” (`:101`). Reabrir admin chama `abrir`, não atribui FK (`:259`) | `tests/test_recobrar_questionario.py:80`, `:112`, `:122`; `tests/test_reabrir_disputa_admin_sem_aberta.py:22` |
| `services/acoes_chamado.py:342`, `:348`, `:388` | Leitor de detalhe e `todas_validadas` | Composição atual | Origem fica sem perguntas; `bool(_todas_perg)` impede tratar vazio como “todas validadas” (`:388`) | Reaponte [nao verificado] |
| `services/disputa_emissao.py:300`, `:371`, `:387`, `:909`, `:912`, `:1237` | Leitor de dedup/complementação | Dedup local usa disputa atual; complementação segue FK encontrada | Origem deixa de satisfazer dedup local; complemento passa a ocorrer na nova disputa | Reaponte [nao verificado] |
| `services/disputa_emissao.py:1485`, `:1580`, `:1621`, `:1634`, `:1657`, `:1666`, `:1715` | Leitor; resolve perguntas e fecha disputas | Composição e estado atuais | Pergunta movida entra no universo de disputa aberta; fecho/reabertura do pai usa a nova disputa. Fechadas são explicitamente puladas (`:1635`, `:1661`, `:1717`) | `tests/test_b2d_resposta_visivel.py:66`, `:78`, `:87`, `:113`; `tests/test_n1_perguntas_do_app.py:225` |
| `services/disputa_emissao.py:1088`, `:1112` | Escritor de motivos e autofecho, durante emissão | Metadados não são derivados automaticamente da FK | Emissão pode atualizar motivos da nova disputa; simples atribuição da FK não executa essa escrita | `tests/test_hx_disputa_muda.py:24`, `:35`, `:52`; `tests/test_hx_disputa_muda_sem_ata.py:51`, `:63` |
| `services/declaracao_texto.py:227`, `:325`, `:536`, `:614`, `:644` | Leitor de reutilização/fato; chama `abrir`/validação | Busca local por disputa, busca global por pessoa/fato | Reutilização local segue nova disputa; agrupamento global é preservado se pessoa/motivo/data forem preservados | Reaponte [nao verificado] |
| `services/relavra_via.py:152`, `:181`, `:221`, `:243` | Leitor; relavra/desfaz carimbo/reabre disputa | Estado atual da FK | Pergunta entra no recorte “aberta”; desfazer carimbo reabre somente a disputa atual | `tests/test_o261_julgamento_desfaz_carimbo.py:135`, `:161`, `:208`; `tests/test_porta_veredito_pergunta.py:424` |
| `services/passivo_validado.py:333`, `:354`, `:516`, `:521`, `:522` | Leitor; materializa passivo e agrupa fechamento por disputa | FK atual | Contagem e quitação migram para nova disputa | `tests/test_o317_passivo_pela_porta.py:418`; reaponte [nao verificado] |
| `services/cobranca.py:61`, `:91`, `:230`, `:234` | Leitor; calcula pais/prazos, agrupa pessoa/disputa | Pai atual participa do prazo | Migra crédito da cobrança e prazo lido; aberta passa a integrar escalonamento | `tests/test_recobrar_questionario.py:88`, `:96`; reaponte [nao verificado] |
| `services/explicador.py:33`, `:122`, `:124`, `:132`, `:137` | Leitor por disputa | Composição atual | Origem vazia produz “Tudo resolvido — a disputa pode fechar” (`:139`); não a reabre | Reaponte [nao verificado] |
| `services/raiox.py:31`, `:55`, `:60`, `:135` | Leitor de stale/crédito/placar | Pai atual é fallback de crédito | Pessoa preservada mantém marcos; crédito passa ao pai novo quando destino não vence (`:142`–`:147`) | `tests/test_selo_defer_json_creditos.py:12`; reaponte [nao verificado] |
| `services/painel.py:262`, `:264`; `services/verbos.py:310`, `:312` | Leitores de empresa/praça/atividade | Dependem da pessoa, não do nascimento | Preservar pessoa preserva recorte e contagem desses leitores | Reaponte [nao verificado] |
| `services/pergunta_no_app.py:46`, `:109`; `services/aviso_pendencia.py:10`; `services/janela_colab.py:62` | Leitores por pessoa e vitalidade | Não exigem pertença original | Mesmo fato/pessoa mantém universo; `perguntas_do_app` não filtra disputa aberta nesta cópia (`pergunta_no_app.py:46`–`:53`) | `tests/test_n1_perguntas_do_app.py:58`, `:65`, `:138` |
| `services/fabrica_da_celula.py:118`, `:120`, `:137` | Leitor global do fato e estado de disputa; chama emissão | Fato por pessoa; elegibilidade também lê aberta | Mesmo fato continua no conjunto `ja`; estado “desfazível em aberta” pode mudar | `tests/test_o261_julgamento_desfaz_carimbo.py:88`, `:135` |
| `services/competencia_trancada.py:107`, `:110`, `:118`, `:140`, `:251` | Leitor de pauta/resgate/reabertura de competência | Usa pessoa e pai atuais | Pessoa/data preservam competência; fallback de resgate e associação ao chamado-pai migram | `tests/test_n1_perguntas_do_app.py:213`; reaponte [nao verificado] |
| `services/limbo_folgas.py:202`, `:238`, `:243` | Leitor; escreve **destino**, chamando `reapontar_resolvedora` | Não atribui `disputa_id` | Vínculo com pessoa mantido; reatar limbo continua sendo reaponte da resolvedora | `tests/test_f3_beco1_reata_limbo.py:49` |
| `services/vigia_fio.py:399`, `:748`, `:758`, `:785`, `:809`, `:816` | Leitor/detector | Contador “viva em fechada” depende do estado atual da FK | Pergunta deixa balde de fechadas e respectivo ID de disputa; detector não reabre (`:737`–`:759`) | `tests/test_n1_perguntas_do_app.py:162`; `tests/test_o261_vigia_fio.py:708` |
| `services/vigia_carimbo.py:102`, `:103` | Leitor/detector por empresa e juiz | Pessoa e fatos atuais | Mesmo colaborador mantém recorte; mudança no contexto de disputa pode alterar juiz chamado (`:106`, `:110`) | `tests/test_o261_vigia_fio.py:415`, `:434` |
| `reconciliador.py:368`, `:411`, `:589`, `:618`, `:1038`, `:1079`, `:1146`, `:1185`, `:1292`, `:1296`, `:1487`, `:1542`, `:1545` | Leitor; fecha disputas/carimba/reata **destino** | Filtros de aberta e derivação pelo contexto atual | Novo vínculo amplia elegibilidade de aberta; derivação recebe contexto da nova disputa; fechos só visitam disputas atuais e pulam fechadas (`:601`, `:1150`) | `tests/test_n1_perguntas_do_app.py:151`, `:302`; reaponte [nao verificado] |

**Commands: demais sítios completos do recorte**

| Arquivo/sítio | Tipo e efeito | Teste | [nao verificado]
|---|---|---|
| `management/commands/dedup_perguntas_fato.py:56`, `:57`, `:68`, `:81` | Leitor agrupa por pessoa/motivo/data; mesmo colaborador preserva grupo | Reaponte [nao verificado] |
| `management/commands/backfill_veredito_lavrado.py:101`; `lavrar_perguntas_stale.py:86` | Carregam disputa/pessoa para juiz e lavra; efeito depende dos insumos da nova disputa | `tests/test_porta_veredito_pergunta.py:424` cobre outra porta; reaponte [nao verificado] |
| `management/commands/auditar_disputas.py:72`, `:75`, `:78`, `:152`, `:156`, `:182` | Leitor de abertas, contexto e sincronismo motivos/perguntas; pergunta movida entra no universo da nova; origem fechada fica fora | `tests/test_auditar_disputas_padroes.py:80`, `:94` |
| `management/commands/auditar_invariantes_chamados.py:75`, `:144`, `:162`, `:265`, `:271`, `:297`, `:325`, `:348`, `:528`, `:536`, `:592`, `:739`, `:857`, `:893`, `:910`, `:924`, `:1160`, `:1574`, `:1705`, `:1762`, `:1777`, `:1801` | Leitores de pessoa/fato, fechada/aberta e pai; baldes dependentes do container mudam; os de pessoa preservada não. I6 exclui fechada vazia (`:715`–`:740`) | Reaponte [nao verificado] |
| `management/commands/reatar_beco_folga_ao_limbo.py:117` | Leitor para reatar **destino**, não FK de disputa | `tests/test_f3_beco1_reata_limbo.py:49` |
| `management/commands/arquivar_competencia_encerrada.py:158` | Leitor pergunta se disputa tem alguma viva; origem vazia retorna `None` | Reaponte [nao verificado] |
| `management/commands/reabrir_orfas_enterradas.py:85`, `:96`, `:97`, `:102`, `:117`, `:133`, `:136`, `:144`, `:153` | Leitor; reabre disputa/pai **atuais**, não atribui FK | `tests/test_reabrir_orfas_enterradas.py:119`; reaponte [nao verificado] |
| `management/commands/encerrar_perguntas_com_fato.py:50`, `:83` | Agrupa disputas atuais, verifica fio e fecha por porta | Reaponte [nao verificado] |
| `management/commands/curar_perguntas_em_limbo.py:29`, `:31` | Busca ocorrência no pai atual para recuperar limbo; após reaponte pode perder pista que está no pai antigo | Reaponte [nao verificado] |
| `management/commands/disparar_perguntas_competencia.py:212` | Dedup global por pessoa/motivo/data preservado | Reaponte [nao verificado] |
| `management/commands/validar_classe_a.py:100` | Validação passando disputa; ID+disputa antiga pode deixar de casar com a pergunta | Reaponte [nao verificado] |
| `management/commands/fechar_becos_disputa.py:55` | Mostra quantidade atual; porta ignora fechadas (`services/becos.py:27`) | `tests/test_varredura_becos_evento_pergunta.py:94` |
| `management/commands/smoke_chamados.py:103` | Consulta pergunta por disputa/motivo | Reaponte [nao verificado] |

`app/chamados/detector_anomalias.py` contém `detectar_anomalias` e `detectar_batida_fora_escala`, sem acesso direto encontrado à relação censada; efeito indireto de reaponte [nao verificado]. Funções: `app/chamados/detector_anomalias.py:19`, `:124`.

Neste recorte não encontrei atribuição de `PerguntaDisputa.disputa`/`disputa_id` fora da criação; os reapontamentos existentes do limbo/reconciliador escrevem **`chamado_destino`**, através de `reapontar_resolvedora`: `app/chamados/services/limbo_folgas.py:224`, `:243`; `app/chamados/reconciliador.py:1535`. Ausência absoluta de outro escritor no restante do repositório [nao verificado].

## 06/11 19:38 · sessao b78a0bd72132 · - · - · origem=Ronald (tmux)

Censo somente por leitura; testes não executados. As referências abaixo distinguem teste de vencimento, teste apenas com prazo futuro e teste estrutural. [nao verificado]

**Casos que fixam explicitamente o comportamento (2)**

| Leitor atingido | Caso existente | O que fixa |
|---|---|---|
| `particionar_verbos` | `app/chamados/tests/test_f1_arquivar.py::test_prazo_vencido_vira_historico_e_nao_volta_pra_fila` — definição :48, vencido :52, asserts :55-57 | Vencido vira `historico`; deixa `decidir` e `arquivado` zerados. |
| `particionar_verbos` | `app/chamados/tests/test_f7_janela_arquivo.py::test_vencida_a_janela_vira_historico_e_NAO_volta_pra_fila` — :41-46 | Mesmo contrato de vencido fora da cobrança. |
| `particionar_verbos` | `app/chamados/tests/test_f7_janela_arquivo.py::test_historico_nao_conta_como_registro` — :48-51 | Vencido mantém verbo não nulo; não prova sozinho exclusão da cobrança. |
| Lista padrão do painel | `app/chamados/tests/test_f8_lista_esconde.py::test_lista_padrao_esconde_arquivado_e_historico` — :36-43 | Prazo futuro e vencido ficam fora da lista padrão. |
| Pílula Arquivados | `app/chamados/tests/test_f8_lista_esconde.py::test_pilula_arquivados_mostra_so_os_da_janela` — :45-51 | Apenas futuro entra na pílula; vencido fica fora. Essa expectativa pode continuar válida após a cura. |
| Busca histórica | `app/chamados/tests/test_f9_busca_historico.py::test_so_arquivados_nunca_o_vivo` — :51-56; fixture vencida :32 | Campo preenchido, inclusive vencido, pertence à busca; sem campo não pertence. Isso não prova que o vencido precise sair da cobrança. |
| Busca histórica | `app/chamados/tests/test_f9_busca_historico.py::test_busca_por_colab` — :58-61; fixture :32 | Busca encontra o arquivado vencido. |
| Ficha do colaborador | `app/colaboradores/tests/test_ficha_do_colab.py::test_MORDE_arquivado_e_historico_dao_o_dono_arquivado` — :98-114 | Datas 2000 e 2999 (:109-110) produzem dono `arquivado` (:113); exige exercitar ambos os verbos de `FORA_DA_LISTA_PADRAO` (:114). |
| Contrato estrutural da ficha | `app/colaboradores/tests/test_ficha_do_colab.py::test_MORDE_o_dono_arquivado_le_FORA_DA_LISTA_PADRAO` — :118-130 | Exige uso da constante e presença de `historico` nela (:129-130). |
| Porta alcançável do dia | `app/chamados/tests/test_o261e_f5b_juiz_porta.py::test_MORDE_silenciado_na_celula_nao_e_numero_da_fila` — :144-165 | Com vencido, conserva `{id: 'historico'}` e `fila=()` (:162-164); futuro nem abre porta (:159-160). |

**Leitores com testes apenas de prazo futuro — não atribuir cobertura de vencido** [nao verificado]

- `janela_colab`: `app/chamados/tests/test_c1_janela_colab.py::test_arquivado_dentro_da_janela_tambem_sai`, :55-60; futuro em :58.
- Fio com janela: `app/chamados/tests/test_c2_endpoints_janela.py::test_fio_com_so_janela_recorta`, :41-48; helper põe futuro em :27.
- Complemento “meus arquivados”: `app/chamados/tests/test_c3_meus_arquivados.py::test_traz_fora_da_janela_e_arquivado_mas_nao_o_da_lista`, :45-52; helper põe futuro em :33.
- Porta da pergunta: `app/chamados/tests/test_selo_i6_ouve_a_lei.py::test_MORDE_destino_silenciado_nao_da_porta`, :136-149; futuro em :143.
- API mensageria: `app/api/tests/test_api_mensageria_fila.py::test_MORDE_o_arquivado_viaja_separado_do_total`, :159-189; futuro em :178.
- Isentos/evento: `app/chamados/tests/test_varredura_isentos_evento.py::test_MORDE_o_resgate_de_isento_volta_ao_veu_pelo_evento`, :57-68, fixa ressilenciamento após limpar o campo, sem testar prazo vencido.
- Prescrição: `app/chamados/tests/test_f6_prescricao.py::test_idempotente`, :54-61, repete imediatamente após silenciar; não testa vencido.

**Casos existentes que fixam (1), inclusive vencido**

- `app/chamados/tests/test_f9_contador_bate_com_a_lista.py::test_silencio_VENCIDO_volta_sozinho`, :90-96: vencido pertence a `qs_na_fila`.
- `app/escala/tests/test_ranking_chamados_pela_fila.py::test_silencio_vencido_volta_a_fila`, :62-64: fixture vencida para ranking.
- `app/chamados/tests/test_f3_beco1_reata_limbo.py::test_MORDE_pai_cujo_silencio_ja_venceu_volta_a_fila_e_reata`, :149-160: vencido recebe reata.
- `app/chamados/tests/test_fio_contador_ativos_pela_fila.py::test_o_numero_e_o_da_fila_do_painel_para_o_colab`, :59-65: fixture vencida em :64; contador igual à fila em :65.

**Demais testes indiretos: limites** [nao verificado]

- `app/inteligencia/tests/test_intel_juiz.py::test_01_MORDE_na_fila_e_o_verbo_nao_o_status`, :35-42: mock de mapa com `validar`, `None`, `arquivado`; fixa seleção por verbo, sem testar vencido ou `historico`.
- `app/chamados/tests/test_badge_topo_coerente.py::test_badge_igual_soma_dos_verbos`, :9-21: compara badge com `validar + decidir`, sem fixture silenciada.
- `app/core/tests/test_badge_dois_numeros.py::test_MORDE_o_headline_NAO_e_a_soma_dos_verbos`, :65-74: comentário cita histórico, mas fixture não põe silêncio.
- `app/chamados/tests/test_e5b_cobrar_massa.py`, :52-81: deduplicação, filtros e prévia, sem fixture silenciada; cobertura específica de vencido nesse envio [nao verificado].
- `app/chamados/tests/test_q2ja_v2_corte_por_janela.py`, :90-104, e `app/chamados/tests/test_rotulo_solicitacao.py`, :79-82: testam regra de corte do arquivador, sem testar vencido.

**Inventário dos 29 arquivos de testes que contêm literalmente `silenciado_ate` ou `PRAZO_ARQUIVO_DIAS`** [nao verificado]

| Arquivo | Escrita/leitura encontrada |
|---|---|
| `app/api/tests/test_api_mensageria_fila.py:178` | Fixture de futuro; saída da API em :182-185. |
| `app/colaboradores/tests/test_ficha_do_colab.py:110` | Fixture vencido/futuro; dono e verbos em :113-114; AST em :118-130. |
| `app/core/tests/test_registrado_nao_e_pendencia.py:108` | Fixture de futuro para card do posto. |
| `app/core/tests/test_contract_escritores_estado.py:35` | Declara escritor único permitido. |
| `app/escala/tests/test_ranking_chamados_pela_fila.py:54` | Fixtures futuro :54 e vencido :64. |
| `app/ponto/tests/test_contract_juiz_feriado.py:56` | Fonte sintética; AST da definição única em :66-72. |
| `app/chamados/tests/test_agir_por_dono.py:56` | Assert campo nulo após recusa. |
| `app/chamados/tests/test_sla_pela_fila.py:72` | Fixture futuro; exclusão do SLA em :74. |
| `app/chamados/tests/test_f3_beco1_reata_limbo.py:113` | Fixtures futuro :113/:140 e vencido :157. |
| `app/chamados/tests/test_c1_janela_colab.py:34` | Helper escreve fixture; teste futuro :55-60. |
| `app/chamados/tests/test_c2_endpoints_janela.py:27` | Helper monta atualização; salva em :28. |
| `app/chamados/tests/test_c3_meus_arquivados.py:33` | Helper monta atualização; salva em :34. |
| `app/chamados/tests/test_fio_contador_ativos_pela_fila.py:56` | Fixtures futuro :56 e vencido :64. |
| `app/chamados/tests/test_c4_resgate.py:33` | Fixture; asserts nulo/não nulo em :44/:48/:58/:71. |
| `app/chamados/tests/test_f1_arquivar.py:35` | Fixtures e retorno em :35-57. |
| `app/chamados/tests/test_f9_contador_bate_com_a_lista.py:50` | Helper escreve fixture; contrato vencido em :90-96. |
| `app/chamados/tests/test_porta_arquivo_silencio.py:87` | Leitura genérica dos campos; asserts :136/:153/:181/:202/:226/:264/:400; fontes sintéticas :441-454; regex do prazo :456-505. |
| `app/chamados/tests/test_f6_prescricao.py:41` | Asserts :41/:46-47; captura e compara prazo :57/:60. |
| `app/chamados/tests/test_o261e_f5b_juiz_porta.py:157` | Fixtures vencido/futuro :157; futuros :202/:233. |
| `app/chamados/tests/test_varredura_isentos_evento.py:43` | Asserts :43/:46/:55/:64/:68/:75/:85/:107. |
| `app/chamados/tests/test_selo_i6_ouve_a_lei.py:54` | Fixtures futuro :54-55 e :143-144. |
| `app/chamados/tests/test_f4_arquivar_lote.py:44` | Asserts :44-45/:57. |
| `app/chamados/tests/test_silenciar_isentos.py:12` | Inspeção textual; aceita menção inclusive em comentário. |
| `app/chamados/tests/test_f3_arquivar_porta.py:42` | Asserts :42/:49-50/:72; fixture :65-67. |
| `app/chamados/tests/test_s4_pendente_suspende_cobranca.py:58` | Asserts :58-59/:79/:105; fixtures :98/:113. |
| `app/chamados/tests/test_o261e_f5_alcance.py:88` | Helper de fixture futuro. |
| `app/chamados/tests/test_f9_busca_historico.py:40` | Helper escreve fixture :40-41; vencido preparado em :32. |
| `app/chamados/tests/test_f7_janela_arquivo.py:27` | Helper escreve fixture :27-28; expectativas :35-56. |
| `app/chamados/tests/test_f8_lista_esconde.py:27` | Helper escreve fixture :27-28; expectativas :36-51. |

O teste do prazo padrão é `app/chamados/tests/test_porta_arquivo_silencio.py::test_MORDE_sem_ate_a_porta_usa_o_juiz_do_prazo`, :386-402: importa a constante em :387 e compara `silenciado_ate` com `now + PRAZO_ARQUIVO_DIAS` em :399-402. O gate de soma única é `::test_MORDE_o_teto_do_prazo_mora_SO_na_porta`, :456-490; a autoridade única é `app/ponto/tests/test_contract_juiz_feriado.py::test_MORDE_prazo_de_arquivo_definido_uma_vez`, :66-72.

## 06/11 19:38 · sessao 2434b4ff5fe8 · - · - · origem=Ronald (tmux)

Censo complementar concluído, somente leitura.

**Estado além do silêncio:** não existe `arquivado` nos estados de `ChamadoColaborador`; os campos de arquivo/silêncio são `silenciado_ate`, `silenciado_por`, `silenciado_motivo`, `resgatado_em`, sem `arquivado_em` na declaração do modelo (`app/chamados/models.py:22-95`; estados catalogados em `app/chamados/catalogo/motor.py:19-31`). Há, porém, **arquivo por encerramento da competência trancada**: `via_encerramento='competencia_trancada'`, independente de silêncio (`app/chamados/catalogo/motor.py:90-94`; `app/chamados/models.py:1552-1569`).

| Sítio | Papel / efeito | Cura do relógio |
|---|---|---|
| `app/chamados/views_arquivar.py:165` | Inclui no arquivo `silenciado_ate` preenchido **ou** via `competencia_trancada`. | Via continua no arquivo mesmo sem silêncio. |
| `app/chamados/views_arquivar.py:64` | Detecta via da tranca e chama `reabrir(humano=True)` antes de levantar véu. | Sem alteração só pelo relógio. |
| `app/chamados/services/disputa_emissao.py:1572` | Não resolve perguntas pela reconciliação comum quando encerramento veio da tranca. | Sem alteração. |
| `app/chamados/management/commands/arquivar_prescritos.py:54` | `resgatado_em` preenchido impede prescrição automática. | Vencido retornar à cobrança não implica ser novamente prescrito. |
| `app/chamados/services/competencia_trancada.py:104` | Resgate posterior a `fechado_em` imuniza contra nova tranca; aplicação da proteção em `:136-138`. | Sem alteração. |
| `app/chamados/services/silencio_ausencia.py:66` | Prefixo de `silenciado_motivo` identifica propriedade do silêncio por ausência. | Não constitui arquivo independente. |
| `app/chamados/services/arquivo.py:123-126` | Lê autor, motivo, resgate e prazo para fotografia auditável. | Outro; não decide fila. |

Teste de arquivo **sem silêncio**: `app/chamados/tests/test_competencia_trancada.py::test_o_arquivo_lista_o_encerrado_pela_tranca` (`:203-209`). Teste de resgate/proteção: `::test_MORDE_desarquivar_devolve_e_a_segunda_passada_respeita_o_admin` (`:164-175`).

**Consumidores diretos da partição** — propagam tipo (2), sem ler o campo diretamente:

- `app/colaboradores/services/ficha.py:155`, dono em `:160`: vencido troca `historico`/dono `arquivado` pelo verbo e dono efetivos.
- `app/chamados/views_arquivar.py:124`: seleção da prescrição em massa; ainda existe trava própria `isnull=True` em `:136`.
- `app/chamados/views_cobrar.py:77-78`: vencidos de verbo cobrar entram na cobrança em massa, sujeitos aos demais gates.
- `app/chamados/juizes.py:1287`, filtro visível em `:1385`: vencido deixa de ser escondido de `fila`; verbo alimenta também a escada do fio em `:1386-1387`.
- `app/chamados/services/fila.py:55`: redistribui contagens `historico` para verbo.
- `app/chamados/services/painel.py:228`, filtro em `:327`: vencido volta à lista padrão e à pílula efetiva.
- `app/chamados/services/verbos.py:171`: lente passa tratar vencido pelo verbo efetivo.
- `app/chamados/services/na_fila.py:19-25`: vencido passa nos contadores por colaborador/empresa quando verbo pertence a `VERBOS`.
- `app/chamados/management/commands/arquivar_prescritos.py:50`: partição usada, porém `:53` continua protegendo vencidos pela presença.
- `app/api/views_mensageria.py:395`, saída `:421`: JSON de pendências mantém itens, muda o `verbo`.
- `app/api/views_mensageria.py:453`, buckets `:459-470`: histórico redistribuído por verbo.
- `app/api/views_mensageria.py:1149-1151`: vencidos com verbo decidir entram no endpoint decidir.
- `app/core/context_processors.py:90`: fallback do badge admin inclui vencidos que sejam validar/decidir.

Consumidores de `FORA_DA_LISTA_PADRAO`: `app/chamados/services/painel.py:327`; `app/chamados/juizes.py:1385`; `app/colaboradores/services/ficha.py:160`.

Testes indiretos que fixam **vencido** como tipo (2), encontrados pelo agente de testes: [nao verificado]

- `app/colaboradores/tests/test_ficha_do_colab.py::test_MORDE_arquivado_e_historico_dao_o_dono_arquivado` (`:98-114`).
- `app/chamados/tests/test_o261e_f5b_juiz_porta.py::test_MORDE_silenciado_na_celula_nao_e_numero_da_fila` (`:144-165`).

`na_fila` tem teste de delegação, mas não de vencido/histórico: `app/inteligencia/tests/test_intel_juiz.py::test_01_MORDE_na_fila_e_o_verbo_nao_o_status` (`:35-42`).

**Janela do colaborador:** consumidores diretos são `app/chamados/services/fio.py:171`, `app/chamados/views_arquivar.py:195` e `app/api/views.py:1016`. Fio do colaborador ativa esse recorte em `app/chamados/views.py:569`; o complemento para o drawer exclui IDs visíveis em `app/chamados/views_arquivar.py:196-197`. Curar `janela_colab.py:73` devolve vencidos que também satisfaçam janela/pergunta e módulos permitidos; remove esses mesmos itens do complemento do drawer (`app/chamados/services/janela_colab.py:67-74`).

**Consumidores diretos de `qs_na_fila`**, todos propagando (1):

| Arquivo | Linhas |
|---|---|
| `app/chamados/views.py` | `:901` |
| `app/chamados/juizes.py` | `:1290` |
| `app/chamados/services/sla.py` | `:62` |
| `app/chamados/services/fila.py` | `:92`, `:150` |
| `app/chamados/services/painel.py` | `:210` |
| `app/chamados/services/fila_por_causa.py` | `:160` |
| `app/chamados/management/commands/nomear_marco_por_lampada.py` | `:47` |
| `app/chamados/management/commands/alertar_chamados_sla.py` | `:30` |
| `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py` | `:59`, `:100` |
| `app/core/services/painel_op.py` | `:119`, com `annotate(total=Count(...))` |
| `app/core/views.py` | `:211`, `:1193` |
| `app/core/context_processors.py` | `:93`, `:150`, `:187` |
| `app/escala/services/furos_diarios.py` | `:212`, com `annotate(n=Count(...))` |
| `app/escala/services/propositor.py` | `:632` |
| `app/ponto/services/lastro.py` | `:239`, `:387` |
| `app/colaboradores/management/commands/marcar_feriado_do_posto.py` | `:102` |
| `app/ponto/management/commands/curar_residuo_bug115.py` | `:72` |

**Prazo:** leitor numérico runtime único atual é `arquivo.prazo_padrao`, que importa a constante em `app/chamados/services/arquivo.py:117` e soma em `:118`; definição `app/chamados/catalogo/motor.py:84`. Registro em `app/core/juizes.py:285`, catraca de definições em `:1387`.

Testes do prazo: [nao verificado]

- `app/chamados/tests/test_porta_arquivo_silencio.py::test_MORDE_sem_ate_a_porta_usa_o_juiz_do_prazo` (`:386-402`).
- `::test_MORDE_o_teto_do_prazo_mora_SO_na_porta` (`:456-490`).
- `::test_MORDE_o_proprio_varredor_do_teto_ve_CODIGO` (`:492-505`).
- `app/ponto/tests/test_contract_juiz_feriado.py::test_MORDE_o_selo_ve_corte_e_prazo_na_mao` (`:55-60`).
- `::test_MORDE_prazo_de_arquivo_definido_uma_vez` (`:66-72`).

Serialização de chamados encontrada é manual: `app/api/views.py:1121` em diante; `app/api/views_mensageria.py:411-422`. Não localizei arquivo com nome contendo `serializ` na cópia; eventual serialização dinâmica não identificada é [não verificado].

## 06/11 19:38 · sessao 1b606e328507 · - · - · origem=Ronald (tmux)

Censo externo, somente leitura. “Efeito” abaixo é inferência do código para reaponte entre disputas do **mesmo colaborador**; execução desse desenho em testes ou produção [nao verificado]. [nao verificado]

| Sítio | Leitor/escritor | Supõe pertença fixa? | Efeito do reaponte | Teste que fixa | [nao verificado]
|---|---|---|---|---|
| `app/api/views.py:1007` | Leitor declarado: `_pend`, filtro colaborador/disputa aberta/sem resposta, `values_list(destino,pai)` até 1010 | Não demonstra: `_pend` não é consumido posteriormente | **Esse trecho isolado não lista nada:** constrói QuerySet sem avaliação; listagem efetiva abaixo | Teste desse trecho isolado [nao verificado] |
| `app/api/views.py:1028`, `1043`, `1047`, `1158` | Leitor efetivo da lista v2: somente perguntas com FK em disputas abertas; destino vivo ou fallback para `p.disputa.chamado_id` | Usa contêiner atual, sem memória do nascimento | A pergunta antes excluída pela FK fechada entra no conjunto da viva; fallback de chamado muda para pai novo | `app/api/tests/test_chamados_v2_destino.py:54` fixa destino; `:60` fixa fallback; `:67` fixa permanência na janela |
| `app/api/views.py:1182` | Leitor de disputa e quota do card: `disputa_id`, `reaberturas_colab`, `pode_solicitar_correcao` até 1188 | Quota pertence à disputa atual | Pergunta passa a usar cabeçalho/quota de correção da nova disputa | Teste específico de troca da FK [nao verificado] |
| `app/api/views.py:2551`, `2563`, `2573`, `2586`, `2590` | Lista `/disputa/perguntas/`: fonte `perguntas_do_app`; serializa FK de cada pergunta e usa primeira disputa no envelope/fallback | Não exige disputa natal; envelope depende da primeira pergunta | FK serializada muda; pai de fallback muda; identidade da pergunta permanece `p.pk` em 2575 | `app/chamados/tests/test_exibicao_so_viva.py:60`; `app/chamados/tests/test_pergunta_no_app.py:89` |
| `app/api/views.py:2628`; `app/chamados/views.py:1239` | Leitores de autorização da resposta: PK + colaborador via disputa; vitalidade pelo juiz | Não; não filtram disputa aberta | Mesmo colaborador conserva acesso; não dependem do ID antigo; mudar colaborador alteraria autorização | Teste específico de reaponte [nao verificado] |
| `app/api/views.py:2619`, `2647` | Escritor de **log**, não FK: eventos por modelo `PerguntaDisputa` e PK | Preserva identidade por PK | Trilha de tentativas/sucesso continua associada à mesma pergunta | Teste de continuidade após troca [nao verificado] |
| `app/chamados/views.py:399` | Releitura do cartão HTMX pelo PK, `select_related(disputa__colaborador)` | Não | Re-render usa relação atual | Teste de FK móvel [nao verificado] |
| `app/chamados/views.py:493`, `510`, `528` | Leitores dos gates validar/recusar/corrigir: FK deve apontar disputa aberta e `chamado_que_lista` deve coincidir | Supõem disputa **operacional** atual aberta; não natal | Reaponte torna pergunta elegível nesses gates; validação recebe `_perg_pre.disputa` em 501 | `app/chamados/tests/test_fio_disputa_por_destino.py:21` fixa resolução por destino, não troca FK |
| `app/chamados/views.py:1410`, `1416`, `1420` | Leitor da reabertura/recobrança: resolve disputa do card, consulta perguntas atuais, corrigíveis e pendentes vivas | Trabalha sobre membros atuais | Recobrança e correção passam a agir pela nova disputa; antiga perde membro | Mensagem para conjunto vazio em `app/chamados/views.py:1434`; teste específico [nao verificado] |
| `app/chamados/views_cobrar.py:160`, `163`, `169`, `175` | Leitor inline: pai aberto ou reverse `perguntas__chamado_destino`, depois perguntas da disputa | Não registra origem | Encontra nova disputa pela pergunta; contagem/espera usam os membros atuais | Teste de reaponte [nao verificado] |
| `app/api/foto_recebida.py:51`, `61`, `73`, `79` | Leitor de FK e reverse; escreve resposta/validação/fechamento via portas, não FK | **Contexto é da disputa atual:** motivo e `contexto.ultima_batida`, 63-68 | Nova disputa governa seleção por foto; se contexto novo não corresponde à pergunta antiga, fluxo de foto pode diferir | Teste dessa combinação [nao verificado] |
| `app/api/views_mensageria.py:539`, `560` | Leitor da fila de validação: serializa FK e colaborador até 562 | Não | Próximas leituras devolvem FK nova; mesma pessoa conserva nome/ID | Teste de FK móvel [nao verificado] |
| `app/chamados/canal.py:64`, `69`, `72`, `77` | Leitor cru de perguntas mudas; cruza LogAuditoria por **colaborador + dia**, não disputa | Não | Cruzamento com fala antiga persiste com mesma pessoa/data; `disputa_id` de saída muda | Teste dessa continuidade [nao verificado] |
| `app/colaboradores/services/ficha.py:166`, `169` | Leitor da fonte do app e agrupamento pelas disputas atuais para respostas sem veredito | Não | Grupos mudam, mesma pergunta continua cruzada por PK | `app/chamados/tests/test_n1_perguntas_do_app.py:138` fixa consumo da fonte |
| `app/colaboradores/services/calendario.py:473` | Leitor por colaborador/dia, exclui `resposta_colab=''`, juiz em 476 | Não | Pergunta **sem resposta**, escopo proposto, já está fora dessa seleção; reaponte isolado não a inclui | Teste de reaponte [nao verificado] |
| `app/ponto/services/dia_do_colab.py:64` | Leitor por colaborador/dia, retorna chamado **da disputa**, 66-68 | Apresenta pai atual como `chamado`; não guarda natal | Diagnóstico do dia passa a citar novo pai, mesmo que `chamado_destino` seja outro | `app/chamados/tests/test_pergunta_no_app.py:145` cobre diagnóstico; troca FK [nao verificado] |
| `app/ponto/supra_juiz.py:543` | Leitor de conversa: perguntas via disputas originadas no chamado e via `perguntas_que_resolvem` | O primeiro caminho associa conversa ao contêiner atual | Sem `respondida_em`, mover a pergunta não altera esse veto; se houver carimbo sem texto, perder o membro pode alterar o primeiro caminho | Condição literal em `app/ponto/supra_juiz.py:545`, `548`; teste de meio-estado [nao verificado] |
| `app/ponto/services/veredito_celula.py:174` | Leitor por colaborador/dia e `q_vivas`; escritor de fechamento da pergunta pela porta `fechar_por_admin`, 177 | Não | Mesmo universo por pessoa/data; não escreve FK | Teste de reaponte [nao verificado] |
| `app/folha/previa.py:64` | Leitor por colaborador/data, agrupa por esses eixos em 67 | Não | Universo e agrupamento conservados com mesma pessoa/data | Teste de FK móvel [nao verificado] |
| `app/ponto/management/commands/mapa_divergencia.py:270` | Leitor de cobertura por pergunta, colaborador e data | Não | Medida conserva data/pessoa, sem recorte por disputa aberta | Teste de FK móvel [nao verificado] |
| `app/ponto/management/commands/reconciliar_fantasmas.py:211` | Leitor de diagnóstico: destino + não materializada + disputa aberta; imprime PK→FK em 217 | Não | Pergunta passa a entrar no diagnóstico e imprime nova FK | Teste de FK móvel [nao verificado] |
| `app/ponto/management/commands/reconciliar_grade.py:460` | Leitor por destino/respondida + `fio_vivo`; não filtra FK | Não | Conserva seleção se destino/carimbos preservados | Teste de reaponte [nao verificado] |
| `app/chamados/relatorios_disputa.py:12` | Leitor/classificador do reverse atual | Quantifica membros atuais, sem origem | Antiga sem perguntas classifica **`SEM_PERGUNTA`**, 13-14; nova passa a contá-la | `app/chamados/tests/test_relatorio_pendencias.py:34`, `:37`, `:41` |
| `app/chamados/relatorios_disputa.py:40` | Leitor/coletor só de disputas abertas; prefetch `perguntas__celula`/destino em 43 | Não exige natal | **Fechada vazia não aparece no relatório de pendências**; não a reabre nem escreve estado | `app/chamados/tests/test_relatorio_pendencias.py:61` fixa ignorar fechadas |
| `bin/censo_folha.py:28` | Leitor/contador cru de perguntas sem resposta em disputas abertas; contador de respondidas em 29 | Equipara “viva” a contêiner aberto + carimbo | `pviva` aumenta com reaponte; `pval` conserva seleção se carimbos não mudam | Teste [nao verificado] |
| `app/chamados/views.py:909` | Contador de **disputas** abertas, não perguntas | Não | Mover FK sem mudar `fechada_em` não altera quantidade de disputas abertas | Teste específico [nao verificado] |

Complementos:

- A migração da pergunta declara apenas FK obrigatória `CASCADE`, `related_name='perguntas'`; opções só rótulos/ordenação, sem unicidade nessa criação (`app/chamados/migrations/0014_perguntadisputa.py:27`, `:30`). Nenhuma migração localizada altera a FK ou cria unique sobre `disputa_id`; confirmação do schema efetivamente instalado [nao verificado].
- Nenhum escritor de FK fora da criação foi localizado no escopo externo; portas externas escrevem resposta, validação ou logs, usando a disputa atual (`app/api/views.py:2642`; `app/chamados/views.py:1281`; `app/api/foto_recebida.py:48`). Ausência global de escritor deve ser consolidada com os outros censos [nao verificado].
- PDF de histórico de chamados lê **campos de chamados e followups**, não `PerguntaDisputa.disputa`: `app/relatorios/pdf_chamados.py:113`, `:137`, `:166`. O histórico de followups continua por chamado; nenhuma transferência dessa trilha resulta da troca isolada da FK nesses leitores.
- PDF de espelho consulta `motor_bloqueado_por_disputa(colab)` (`app/relatorios/pdf_espelho.py:1084`), wrapper de `colaborador_em_disputa` (`app/ponto/services/disputa.py:15`, `:31`); não lê FK diretamente. O efeito final depende do helper em models e deve ser consolidado pelo pai.
- Batida aponta **para a pergunta por PK**, não para disputa (`app/ponto/models.py:40`); procedência relê pergunta pelo PK e autores/carimbos (`app/ponto/services/procedencia_batida.py:65`, `:70`, `:74`). Reaponte isolado não muda esse elo.
- `app/ponto/services/flip_auto.py:130` faz `select_related('disputa')`, mas o laço 131-140 não usa a disputa; não demonstra dependência de pertença.
- Templates recebem listas já montadas: `app/templates/chamados/partials/modal_fio.html:156`, `app/templates/chamados/partials/_card_atendimento.html:73`, `app/templates/chamados/detalhe_local.html:307`; IDs/status/cor da disputa exibidos vêm de `chamado.disputa_obj` (`app/templates/chamados/partials/modal_fio.html:75`, `:124`, `:374`). O acesso direto à FK localizado em template é apenas nome do colaborador no lote (`app/templates/chamados/partials/_lote_preview.html:24`).
- Existe reabertura **humana explícita** de disputa fechada até quando vazia: teste cria disputa fechada sem nenhuma pergunta e POST administrativo a reabre (`app/chamados/tests/test_reabrir_disputa_admin_sem_aberta.py:22`, `:30`, `:35`, `:42`). Portanto “não há leitor automático que reabra” não deve virar “jamais pode reabrir”.
- Nenhum dos testes citados executa troca de `PerguntaDisputa.disputa` no cenário proposto; cobertura específica do reaponte e seus efeitos históricos [nao verificado]. [nao verificado]

## 06/11 19:38 · sessao 498c20dfabdf · - · - · origem=Ronald (tmux)

Censo periférico, exclusivamente leitura:

| Sítio | Papel | Supõe / saída com mudança para relógio |
|---|---|---|
| `app/chamados/views_arquivar.py:36`, `:37` | Escritor indireto: `prazo_padrao` → `silenciar` | Prazo padrão; resposta entrega `ate` em `:48`. |
| `app/chamados/views_arquivar.py:77` | Escritor indireto: `levantar(resgatado=True)` | Limpa silêncio; saída específica para tranca exige `reabrir` antes, em `:64`, `:70`. |
| `app/chamados/views_arquivar.py:124`, `:136`, `:140` | Leitor de verbos + guarda de presença + escritor | Verbos fixam universo; `isnull=True` impede repetir escrita sobre vencido. Com ambas as guardas curadas, lote poderia renovar silêncio vencido. |
| `app/chamados/views_arquivar.py:165`, `:166` | Leitor direto: `Q(isnull=False)`, `F(...).desc()` | Consulta inclui vencidos e via de tranca; presença define acervo consultável, sem excluir fila. Pode continuar assim com HISTÓRICO apenas exibido. |
| `app/chamados/views_arquivar.py:195`, `:196` | Leitor indireto da janela; complemento | Vencido hoje fica no drawer; ao curar janela, desaparece do drawer se voltar à lista. |
| `app/templates/chamados/partials/_busca_arquivados.html:46`, `:49` | Leitor direto de exibição | Já lê relógio: futuro “Arquivado até”; vencido “Histórico”. Não decide fila. |
| `app/templates/chamados/partials/_meus_arquivados.html:33` | Leitor direto de presença | Mostra “Arquivado” mesmo vencido. Relógio retiraria esse rótulo ou permitiria substituí-lo por “Histórico”. |
| `app/templates/chamados/partials/_lista_chamados.html:85` | Leitor direto de presença | Exibe botão Resgatar mesmo vencido. Relógio retiraria botão para vencido. |
| `app/chamados/signals.py:256` | Leitor do nome do campo em `update_fields` | Outro: detecta evento de resgate/escrita; não lê prazo. |
| `app/chamados/signals.py:261` | Leitor direto de presença | Outro: impede recursão/repetição do silêncio de isento. Relógio permitiria prosseguir com vencido, mas `silencio_isentos.alvos` ainda o excluiria. |
| `app/chamados/signals.py:242`, `:269` | Escritores indiretos por `silenciar_do_colab` | Evento de colaborador/chamado alimenta mesma porta de isentos. |
| `app/chamados/services/silencio_isentos.py:30` | Leitor direto, `isnull=True` | Outro: idempotência por presença. Relógio faria expirados virar alvos novamente. |
| `app/chamados/services/silencio_isentos.py:39`, `:43`, `:58` | Escritores indiretos | Padrão → `arquivo.silenciar`; mesma execução para evento e command. |
| `app/chamados/management/commands/silenciar_chamados_isentos.py:36`, `:49` | Leitor/escritor indireto | `--alarme` acusa alvos em `:44–47`; guardas curadas fariam expirados alarmarem; `--apply` renovaria. |
| `app/chamados/management/commands/arquivar_prescritos.py:50`, `:53`, `:93` | Leitor indireto de verbos + direto de presença | Outro: não repete arquivo já marcado. Curar verbos e guardas pode selecionar vencidos para novo silêncio. |
| `app/chamados/management/commands/arquivar_prescritos.py:84`, `:94` | Escritor indireto | Usa prazo padrão. |
| `app/chamados/management/commands/arquivar_competencia_encerrada.py:100`, `:274`, `:288` | Leitor de presença + escritor indireto | Evita já marcado; escreve horizonte explícito de dez anos (`:286–287`), independente do prazo padrão. Relógio reabriria seleção de expirados. |
| `app/chamados/management/commands/arquivar_sem_dia_irrecuperavel.py:59`, `:88`, `:94` | Leitor de presença + escritor indireto | Evita já marcado; horizonte explícito de cinco anos (`:30`). Relógio reabriria seleção de expirados. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:645` | Leitor direto | (1): somente outro silêncio futuro dispensa marcador de ausência em `:646`. |
| `app/ponto/management/commands/processar_alertas_avancados.py:112` | Leitor indireto `silenciado_em(agora)` | (1): chama predicado temporal do model. |
| `app/ponto/management/commands/escalonar_chamados_supervisao.py:46` | Leitor direto `exclude(__gt=agora)` | (1): vencido pode escalar. |

API/serialização e agregações:

- `app/api/views.py:1016` lê janela do colaborador; serialização manual começa em `:1121`, objeto em `:1141`. Cura da janela acrescenta vencidos elegíveis à resposta.
- `app/api/views_mensageria.py:395` publica verbos por chamado; `:453–470` agrupa sinais por verbo; `:1149–1152` seleciona `decidir`. Cura de verbos altera rótulos/grupos e inclui vencidos em decidir conforme natureza.
- `app/api/views_mensageria.py:668`, `:687` publica serviços `fila`; `:1589` publica `fila_por_causa`. Universo desses serviços herda `qs_na_fila`.
- `app/escala/services/furos_diarios.py:212` faz `qs_na_fila(...).values(...).annotate(n=Count('id'))`.
- `app/core/services/painel_op.py:119` faz `qs_na_fila().values(...).annotate(total=Count('id'))`.
- Nenhuma ocorrência literal do campo nem arquivo de serializer dedicado apareceu em `app/api`; não confundir isso com ausência de leitores indiretos [não verificado além dos caminhos rastreados].

Outros consumidores indiretos de verbos:

`app/chamados/views_cobrar.py:77–78`; `app/chamados/services/na_fila.py:19–25`; `app/core/management/commands/lavrar_badge_navbar.py:80`; `app/inteligencia/views.py:251`; `app/inteligencia/management/commands/calcular_scores.py:54`; `app/colaboradores/services/ficha.py:155`; `app/core/context_processors.py:90`; `app/chamados/juizes.py:1287–1290`. `na_fila.py:23` só conta verbos em `VERBOS`, portanto vencidos classificados HISTÓRICO ficam fora.

Crons:

- `app/config/crons.py:363`: `processar_alertas_avancados`, a cada 30 minutos.
- `app/config/crons.py:369`: `escalonar_chamados_supervisao`, a cada cinco minutos.
- `app/config/crons.py:609`: `silenciar_chamados_isentos --alarme`, 07:05.
- `app/config/crons.py:1435`: `arquivar_prescritos` explicitamente desligado.
- `app/config/crons.py:1294–1298`, `:1341–1344`: sem-dia e competência encerrada são atos manuais, excluídos de cron.

Estado além de `silenciado_ate`:

- Não há estado `arquivado` no catálogo `app/chamados/catalogo/motor.py:19–20`; campo `status_local` em `app/chamados/models.py:46`.
- Existe arquivo por competência trancada, independente de silêncio: `via_encerramento` em `app/chamados/models.py:48`, valor `VIA_COMPETENCIA_TRANCADA` em `app/chamados/catalogo/motor.py:92`; `app/chamados/models.py:1569` grava estado terminal e via.
- Escritor chamador: `app/chamados/services/competencia_trancada.py:184`.
- Leitores dessa via: `app/chamados/views_arquivar.py:64` e `:165`; `app/chamados/services/competencia_trancada.py:259`; `app/chamados/services/disputa_emissao.py:1572`.
- `resgatado_em` é proteção contra rearquivar, não marca arquivo: `app/chamados/models.py:93–95`, `app/chamados/management/commands/arquivar_prescritos.py:54`; isentos ignoram isso expressamente em `app/chamados/services/silencio_isentos.py:6`.
- `silenciado_por` e `silenciado_motivo` são metadados do véu: `app/chamados/models.py:88–92`.
- Campo `arquivado_em` no model não encontrado; sustentar pela enumeração dos campos `app/chamados/models.py:35–95`, ou marcar ausência global como [não verificado].

## 06/11 19:37 · sessao 335dcc654113 · - · - · origem=Ronald (tmux)

Censo dos arquivos delegados, somente por leitura. As linhas citadas pertencem à cópia; números históricos em comentários não constituem dado de produção verificado. [nao verificado]

| sítio | escritor/leitor | supõe pertença fixa? | efeito do reaponte | teste que fixa | [nao verificado]
|---|---|---|---|---|
| `app/chamados/services/fio.py:116` (`chamado_que_lista`) | Lê FK/container como fallback | Não expressamente; usa relação atual | Com destino, a âncora independe da disputa; sem destino, troca para o chamado da nova disputa. `fio.py:117` | `app/chamados/tests/test_fio_ancora_destino.py:72` cobre fallback; mudança de FK [nao verificado]. |
| `app/chamados/services/fio.py:135` (`_disputa_do_chamado`) | Filtra `disputa__fechada_em` e devolve FK | Não; usa relação atual | A pergunta transferida passa a permitir encontrar a disputa nova pelo destino. `fio.py:135-137` | `app/chamados/tests/test_fio_disputa_por_destino.py:21`. |
| `app/chamados/services/fio.py:218` (`montar_chamados_fio`) | Filtra `disputa_id__in`, lê FK e estado | Universo depende da disputa atual | Sai do universo de fechadas, entra no das abertas; deixa de carregar `em_disputa_fechada`; passa ao universo do colab, sujeito à janela. `fio.py:211-240` | `app/chamados/tests/test_fio_le_o_juiz.py:105`, `:115`, `:121` cobrem situação anterior; reaponte [nao verificado]. |
| `app/chamados/services/fio.py:252` | Herda disputa da pergunta no card sem disputa própria | Não expressamente | Flags do card passam a usar disputa nova. `fio.py:250-254` | Reaponte de FK [nao verificado]. |
| `app/chamados/services/fio.py:392` | Query pai → destinos | Trata relação atual como mapa dos filhos | Filho desaparece do mapa do pai antigo e entra no novo; muda `filhos_vivos` do desenlace. `fio.py:392-403` | Reaponte de FK [nao verificado]. |
| `app/chamados/services/fio.py:591` | POST validar lê pessoa, disputa e fechamento | Não; valida relação atual | Usa disputa nova e via normal; deixa de executar ramo `sem_reabrir` e log específico de validação em fechada. `fio.py:594-613` | `app/chamados/tests/test_fio_le_o_juiz.py:192` cobre via em fechada. |
| `app/chamados/services/fio.py:629` | Recusar/corrigir exige disputa aberta | Não; autoriza por estado atual | Pergunta transferida torna-se elegível a essa seleção. `fio.py:628-635` | Reaponte de FK [nao verificado]. |
| `app/chamados/services/fio.py:769` (`_costura_nao_mata_o_fio`) | Lê empresa pela disputa; julga pós-destino | Preserva identidade da pessoa implicitamente | Mesma pessoa mantém empresa; trocar FK não substitui a guarda do destino terminal. A guarda lê destino fresco e, em transação, trava-o. `fio.py:758-780` | `app/chamados/tests/test_o261e_f1_porta_encerramento.py:252`; `app/chamados/tests/test_o261e_f1_codex_v5.py:49`, `:65`. |
| `app/chamados/services/fio.py:831` (`reapontar_resolvedora`) | **Escreve destino, não disputa** | Conserva FK porque não a escreve | Relavra após destino/data e salva junto; não existe transferência de disputa nesta porta. `fio.py:831-848` | `app/chamados/tests/test_porta_reapontamento_pergunta.py:245`, `:289`, `:309`, `:326`. |
| `app/chamados/services/fio.py:931` (`migrar_resolvedoras_vivas`) | Lê perguntas por destino; chama porta acima | Conserva FK no mecanismo atual | Migra resolvedora entre chamados; não constitui precedente de reaponte de disputa. Audita `resolvedoras_migradas` no chamado velho. `fio.py:931-959` | Teste específico de FK [nao verificado]. |
| `app/chamados/juizes.py:72`, `:205` | `dia_do_fato`/lote leem `d.perguntas` | Usa associação atual como evidência do dia do container | Pai antigo perde os dias dessas perguntas; novo ganha. Só atinge degrau fraco quando os superiores não resolvem. `juizes.py:53-77`, `:199-211` | Reaponte de FK [nao verificado]. |
| `app/chamados/juizes.py:346`, `:361` | Slot/materialização e completude leem disputa | **Contexto da disputa é insumo vivo**, não fotografia da origem | Mesmo mantendo pessoa, mudar `contexto[motivo]` pode alterar derivação; completude usa contexto como fallback quando falta `data_evento`. `juizes.py:343-371` | Transferência entre contextos [nao verificado]. |
| `app/chamados/juizes.py:664`, `:752`, `:774`, `:932`, `:1200`, `:1528` | Leitores da pessoa/empresa/grade pela FK | Supõem identidade consistente | Mesma pessoa preserva esses insumos; não há exigência de “mesma disputa natal” nesses acessos. `juizes.py:664-666`, `:749-774`, `:931-954`, `:1197-1203`, `:1523-1532` | Reaponte de FK [nao verificado]. |
| `app/chamados/juizes.py:811` (`_chave_memo_pv`) | Cache de `pergunta_viva` | **Omissão relevante:** não contém FK nem contexto | Mudar somente disputa na mesma instância pode reaproveitar juízo anterior quando a nova disputa altera insumo de contexto; cache retorna se tupla permanece igual. `juizes.py:811-819`, `:835-847` | Teste específico de troca da FK [nao verificado]. |
| `app/chamados/juizes.py:1021` | Identity map lê disputa e atribui `d.colaborador` | Supõe pk da pessoa constante | Só substitui instância da mesma pessoa; não escreve a FK da pergunta. `juizes.py:1013-1027` | Transferência de FK [nao verificado]. |
| `app/chamados/juizes.py:1230`, `:1392` | `perguntas_vivas_de`, `perguntas_corrigiveis` | Associação atual define conjunto | A disputa antiga perde pergunta e a nova ganha nos conjuntos. `juizes.py:1228-1230`, `:1390-1394` | Transferência de FK [nao verificado]. |
| `app/chamados/juizes.py:1300`, `:1399` | Universo por célula/fila global, com FK carregada | Não exige origem fixa | Mantendo célula/pessoa/carimbos, seleção principal não muda; juízo continua sujeito aos insumos via disputa. `juizes.py:1300-1302`, `:1399-1403` | Transferência de FK [nao verificado]. |
| `app/chamados/juizes.py:1751` | Irmãs respondidas com fallback para pai | Usa container atual como crédito quando destino não está vivo | Resposta futura passa ao pai novo; não ao antigo. A proposta sem resposta não ativa imediatamente esse filtro de respondidas. `juizes.py:1747-1753` | Referências dos selos em `juizes.py:1741-1742`; transferência [nao verificado]. |
| `app/chamados/juizes.py:1948` (`resolvedora_valida`) | Juiz de dia/turno | **Não valida identidade da disputa nem da pessoa** | Mesmo dia permite; ±1 depende do turno do colaborador do chamado; não protege sozinho uma troca indevida da FK. `juizes.py:1963-1999` | `app/chamados/tests/test_selo_resolvedora_do_dia.py:67`, `:81`, `:94`, `:109`, `:146`. |
| `app/chamados/juizes.py:2029` | Datas vivas por `disputa__chamado_id`; competência | Usa associação atual como fatos do container | Antigo perde e novo ganha datas que compõem competência do chamado. `juizes.py:2024-2041`, `:2050-2051` | Reaponte de FK [nao verificado]. |
| `app/chamados/juizes.py:2080` | `disputa_viva_do_chamado` | Aberta sem pergunta trava encerramento | **Não olha disputa fechada**: fechada esvaziada não é reaberta por esse leitor; aberta esvaziada seria bloqueadora. `juizes.py:2080-2086` | Reaponte de FK [nao verificado]. |
| `app/chamados/veredito_lavrado.py:162` | Marcos agrupados por `disputa__colaborador_id` | Identidade da pessoa | Mesma pessoa mantém agrupamento; FK não é um marco temporal. `veredito_lavrado.py:162-169` | `app/chamados/tests/test_selo_lavrado_orfaos.py:121`. |
| `app/chamados/veredito_lavrado.py:187` | `esta_stale` lê pessoa da FK | **Mudança da FK não invalida por si** | Só compara relógio/lavra e marcos da pessoa; troca de contexto requer relavra explícita para evitar cache anterior. `veredito_lavrado.py:179-190`; mecanismo de relavra em `fio.py:845-848` | `app/chamados/tests/test_selo_lavrado_orfaos.py:121`; transferência [nao verificado]. |
| `app/chamados/services/historico.py:96` | Histórico seleciona por destino, apenas carrega disputa | Não usa disputa como origem histórica | Trocar só FK preserva seleção; trocar também destino move os carimbos ao histórico do novo chamado. Logs considerados ali são mudanças de estado de chamado, não evento de transferência da pergunta. `historico.py:86-114` | `app/chamados/tests/test_historico.py:130`, `:138`; transferência [nao verificado]. |
| `app/chamados/juizes.py:280` | Testemunha de retratação por `LogAuditoria` | Identidade é pk da pergunta | Preservando pk, mantém testemunha externa; não depende de disputa natal. `juizes.py:277-282` | Transferência de FK [nao verificado]. |

Dois pontos para a consolidação:

- A lista dos escritores protegidos declara `chamado_destino` e `data_evento`, com `models.py` e `fio.py`; essa passagem não declara `disputa_id`. Portanto, não confundir a exclusividade de resolvedora com uma trava já existente para a FK. `app/core/tests/test_contract_escritores_estado.py:48-62`.
- O memo de `data_do_fato_de` retorna `_dfd_memo` sem comparar campos; depois usa o fallback para disputa quando falta destino. Uma instância reapontada após essa primeira leitura merece revisão do memo, além do memo de `pergunta_viva`. `app/chamados/services/fio.py:74-88`, `:116-117`.

## 06/11 19:36 · sessao 4d0acf7e758f · H12H06-B-PAUTA-08 · leitura · origem=Code

**O código atual usa a mesma coluna e a mesma unidade nominal nos dois lados. Isso não comprova que o PAGO tenha sido produzido com o mesmo algoritmo e os mesmos insumos históricos.** O campo é inteiro anulável; a origem histórica dos valores do anexo é **[nao verificado]**. (`app/ponto/models.py:446`)

**1. Origem, unidade e universo de `dias_previstos`**

- **PAGO:** `valores_do_gravado()` busca o `FechamentoMensal` do colaborador/mês/ano e lê seus atributos segundo `CAMPOS`. Não reconstrói os dias que produziram aquele número. (`app/ponto/portas/regen_exportada.py:77–84`)
- **DEVIDO:** monta a grade e conta suas linhas com `tipo_dia` igual a **`trabalho` ou `ausencia`**. Não exige minutos previstos positivos. (`app/ponto/services/fechamento.py:997–998`, `app/ponto/services/fechamento.py:1030–1033`, `app/ponto/services/fechamento.py:1067–1069`)
- **Escritor do fluxo atual:** o recalculador atribui o campo e persiste por `save()` ou `update()`. Em leitura, devolve o atributo sem persistir. Quem efetivamente escreveu os números históricos desses três colaboradores é **[nao verificado]**. (`app/ponto/services/fechamento.py:1068`, `app/ponto/services/fechamento.py:1112–1128`, `app/ponto/services/fechamento.py:1144–1166`)
- **Leitores:** o comando lê os dois dicionários e compara suas chaves; a prontidão da folha também consulta `dias_previstos` no fechamento. (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:130–139`, `app/folha/export.py:1037–1043`)

O universo diário do DEVIDO resulta destas etapas:

| Etapa | Regra |
|---|---|
| Competência | Usa o corte da empresa atual. Corte 21 produz **21/07–20/08** para 08/2026; o corte efetivo dos três casos não consta do anexo: **[nao verificado]**. (`app/ponto/janelas.py:10–21`, `app/ponto/janelas.py:36–57`) |
| Janela da grade | `grade_do_fechamento()` resolve a competência pela empresa e chama `grade_da_celula()`. (`app/folha/export.py:329–338`) |
| Admissão/demissão | A grade aplica as datas atuais do colaborador e fatos da ata para decidir `fora_vinculo`. Aqui, esse nome não significa simplesmente “fora da vigência de EscalaColaborador”. (`app/escala/services/leitor_celula.py:351–376`) |
| Célula e ata | Sem célula: `sem_celula`. Havendo célula: prevalece `ata.tipo_dia`; sem esse valor, usa `cel.trabalha`. (`app/escala/services/leitor_celula.py:377–381`) |
| Contagem | Entram somente `trabalho` e `ausencia`; folga, indefinido, sem célula e fora do vínculo ficam fora. (`app/ponto/services/fechamento.py:1030–1033`) |

Portanto, há **a mesma unidade pretendida — contagem de dias —**, mas a igualdade do universo histórico do PAGO com o universo atual do DEVIDO é **[nao verificado]**. O PAGO contém um agregado; o DEVIDO conta a grade disponível hoje. (`app/ponto/portas/regen_exportada.py:81–84`, `app/ponto/services/fechamento.py:1030–1033`)

**2. Entradas que podem alterar uma competência passada**

| Entrada | Como interfere |
|---|---|
| Célula regenerada/editada ou ata alterada | Pode alterar `trabalha` ou `tipo_dia`, que alimentam diretamente a classificação contada. A regeneração escreve `trabalha`, geradora e DNA. (`app/ponto/portas/celula.py:263–277`, `app/escala/services/leitor_celula.py:380–381`) |
| Vínculo encerrado/novo | Muda a resolução do vínculo do dia. **Isoladamente, não muda o tipo contado quando a ata/célula continua igual**; pode interferir por regeneração posterior ou no fallback de minutos. (`app/escala/alimentacao.py:269–286`, `app/escala/services/leitor_celula.py:380–395`) |
| Escala/template trocado | Pode chegar à contagem por regeneração das células e julgamento posterior. No leitor, consultar a escala para obter minutos não substitui a regra de classificação do tipo. (`app/ponto/portas/celula.py:255–277`, `app/escala/services/leitor_celula.py:380–395`) |
| Admissão/demissão alterada | Interfere diretamente em `fora_vinculo`: dias posteriores à demissão saem; o próprio dia depende de fato lavrado. Antes da admissão, fato lavrado pode preservar o dia. (`app/escala/services/leitor_celula.py:368–378`) |
| Desligamento | O serviço escreve situação/demissão e fecha vínculos ativos. São efeitos distintos sobre os insumos. (`app/colaboradores/services/desligamento.py:103–124`) |
| Empresa/corte alterado | Pode mudar a própria janela lida para a competência passada. (`app/folha/export.py:336–338`, `app/ponto/janelas.py:36–57`) |

Há ainda um filtro de **pessoas**: desligado com demissão anterior ao início da janela, ou sem data de demissão, fica fora do universo. Isso produz ausência do colaborador no retorno; o comando recusa o ato. Não produz `dias_previstos=1`. (`app/ponto/services/fechamento.py:422–433`, `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:135–144`)

**Qual delas levou 16 a 1 no col842: [nao verificado].** Pelo caminho normal desta cópia, o resultado 1 significa exatamente **uma linha da grade classificada como `trabalho`/`ausencia`**. Essa linha pode ter zero minuto previsto, pois contagem e soma são operações separadas. (`app/ponto/services/fechamento.py:1030–1033`)

Também não cabe deduzir que `horas_trabalhadas × 60` deva igualar `minutos_realizados`: estes vêm da grade, com teto por dia, enquanto horas trabalhadas recebem outros componentes, inclusive minutos órfãos de vínculo. (`app/ponto/services/dia_pago.py:384–400`, `app/ponto/services/fechamento.py:893–908`)

**3. Comparação e ESMERIL-SEMANTICA**

O comando compara **24 chaves**, excluindo `mes/ano` das 26 de `CAMPOS`, sem reconciliar universo histórico ou aplicar conversão específica por campo. Entretanto, não compara por igualdade literal. (`app/ponto/management/commands/aplicar_09_corte_b.py:67–74`, `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:139`)

`_diferente()` converte `a or 0` e `b or 0` para `float` e exige diferença absoluta **maior que 0,015**. Logo, aplica a mesma tolerância numérica a horas, minutos e contagens, além de equiparar **`None` e zero**. (`app/ponto/portas/regen_exportada.py:248–252`)

Esse último é um problema semântico demonstrável: o recalculador usa `None` para ausência de resposta da grade; zero pode ser uma resposta calculada. O comparador apaga essa distinção. (`app/ponto/services/fechamento.py:1027–1033`, `app/ponto/services/fechamento.py:1071–1083`)

**Campo homônimo comprovadamente com definição diferente entre PAGO e DEVIDO: [nao verificado].** Na implementação atual, ambos recortam os mesmos atributos, e o modo de escrita persiste os atributos calculados. Isso não demonstra a definição usada pelo escritor histórico do PAGO. (`app/ponto/portas/regen_exportada.py:77–84`, `app/ponto/services/fechamento.py:1112–1128`, `app/ponto/services/fechamento.py:1144–1169`)

**4. Sonda de leitura na SOMBRA**

A sonda abaixo mostra **quais insumos atuais produzem a contagem**, os dias incluídos/excluídos, vínculos resolvidos, atas e vestígios de regeneração. Identificar **qual entrada mudou desde o PAGO** depende de evidência histórica correspondente: `dna_anterior` guarda somente a versão imediatamente anterior, sem garantir correspondência com aquele fechamento. (`app/escala/models.py:1714–1729`)

Para colar em um **shell Django já iniciado na SOMBRA**, sem salvar arquivo. A configuração da cópia fixa banco `sombra`, host `sombra_db` e mantém o tenant `juliani`; a disponibilidade desse ambiente é **[nao verificado]**. (`app/config/settings/sombra.py:13–14`, `app/config/settings/sombra.py:29`, `app/config/settings/sombra.py:47–51`)

Usa as funções reais e mantém `lavrar_dias_pagos=False`: essa opção importa porque `somente_leitura=True` combinado com lavratura pode escrever. A execução da sonda e seus resultados permanecem **[nao verificado]**. (`app/ponto/services/fechamento.py:524–531`)

```python
import json
import logging
from collections import Counter
from django.conf import settings
from django.db import connection, transaction
from django_tenants.utils import schema_context

ERROS = {}
# Anexo fornecido; reprodução no banco [nao verificado].
ANEXO = {842: (16, 1), 284: (15, 21), 433: (15, 18)}

def emitir(rotulo, valor):
    print(rotulo, json.dumps(valor, ensure_ascii=False, default=str))

def campos(obj, nomes):
    return {k: getattr(obj, k) for k in nomes}

def dna_resumo(dna):
    if dna is None:
        return None
    return {k: dna.get(k) for k in (
        "dna_versao", "vinculo_id", "tipo_escala_id", "tipo_ciclo", "marcos"
    )}

# Intercepta logging em memória, sem emitir para handlers de arquivo.
# Erros registrados pelo recalculador também entram no resultado final.
def colher_log(self, record):
    if record.levelno >= logging.ERROR:
        ERROS["log_%s" % len(ERROS)] = {
            "arquivo": record.pathname, "linha": record.lineno,
            "funcao": record.funcName,
        }

logging_anterior = logging.Logger.callHandlers
logging.Logger.callHandlers = colher_log
try:
    assert getattr(settings, "HASNER_SOMBRA", False), "settings fora da SOMBRA"
    assert connection.settings_dict["NAME"] == "sombra", "banco inesperado"
    assert connection.settings_dict["HOST"] == "sombra_db", "host inesperado"
    assert not connection.in_atomic_block, "exige transacao nova"

    with schema_context("juliani"), transaction.atomic():
        with connection.cursor() as cur:
            cur.execute(
                "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY"
            )
            cur.execute("SHOW transaction_read_only")
            assert cur.fetchone()[0] == "on"
            cur.execute("SELECT current_database(), current_schema()")
            identidade = cur.fetchone()
            assert identidade == ("sombra", "juliani"), identidade
            emitir("AMBIENTE", identidade)

        from colaboradores.models import Colaborador
        from escala.models import EscalaColaborador
        from escala.alimentacao import (
            celulas_do_periodo, escalas_do_periodo,
            vinculo_do_dia, vinculo_cobre_a_data,
        )
        from folha.export import grade_do_fechamento
        from ponto.janelas import janela_fechamento
        from ponto.models import FechamentoMensal
        from ponto.portas.regen_exportada import (
            valores_do_gravado, hash_do_gravado, _diferente,
        )
        from ponto.services.fechamento import (
            recalcular_fechamento_mes, universo_da_competencia,
        )

        for pk in (842, 284, 433):
            try:
                with transaction.atomic():
                    c = Colaborador.objects.select_related("empresa").get(pk=pk)
                    ini, fim = janela_fechamento(8, 2026, c.empresa)
                    h0 = hash_do_gravado(c, 8, 2026)
                    fe = FechamentoMensal.objects.filter(
                        colaborador=c, mes=8, ano=2026
                    ).first()
                    assert fe is not None, "sem PAGO"
                    pago = valores_do_gravado(c, 8, 2026)

                    emitir("CADASTRO", {
                        "col": pk, **campos(c, (
                            "empresa_id", "situacao",
                            "data_admissao", "data_demissao",
                        )),
                        "corte": c.empresa.dia_inicio_competencia,
                        "janela": [ini, fim],
                        "no_universo": universo_da_competencia(8, 2026)
                            .filter(pk=pk).exists(),
                        "carimbos_fechamento": campos(fe, (
                            "previsto_em", "derivado_em", "atualizado_em",
                        )),
                    })

                    cels = celulas_do_periodo([pk], ini, fim)
                    geradoras = {
                        v.escala_geradora_id for v in cels.values()
                        if v.escala_geradora_id
                    }
                    selecionados = sorted(
                        escalas_do_periodo(pk, ini, fim, mais=geradoras),
                        key=lambda e: e.data_inicio,
                    )
                    todos = list(
                        EscalaColaborador.objects.filter(colaborador=c)
                        .select_related("tipo_escala")
                        .order_by("data_inicio", "pk")
                    )
                    emitir("VINCULOS", {
                        "col": pk,
                        "selecionados": [e.pk for e in selecionados],
                        "todos": [{
                            **campos(e, (
                                "pk", "data_inicio", "data_fim", "ativa",
                                "tipo_escala_id", "posto_id",
                                "data_ancora_colaborador",
                                "trabalha_em_feriado",
                            )),
                            "ciclo_atual": e.tipo_escala.tipo_ciclo,
                        } for e in todos],
                    })

                    # folha/export.py:329-338; fechamento.py:997-998.
                    grade = grade_do_fechamento(fe)
                    dias = sorted(grade.get("dias") or [], key=lambda d: d["data"])
                    emitir("TIPOS", {
                        "col": pk,
                        "contagens": dict(Counter(d["tipo_dia"] for d in dias)),
                    })

                    for d in dias:
                        cel = cels.get((pk, d["data"]))
                        ata = (cel.ata or {}) if cel else {}
                        ec = vinculo_do_dia(
                            pk, d["data"], selecionados, celulas=cels
                        )
                        emitir("DIA", {
                            "col": pk, "data": d["data"],
                            **{k: d.get(k) for k in (
                                "tipo_dia", "minutos_previstos",
                                "minutos_realizados", "sem_ancora",
                            )},
                            "conta": d.get("tipo_dia") in ("trabalho", "ausencia"),
                            "cobrem_por_cadastro": [
                                e.pk for e in todos
                                if vinculo_cobre_a_data(e, d["data"])
                            ],
                            "vinculo_resolvido": ec.pk if ec else None,
                            "celula": {
                                **campos(cel, (
                                    "pk", "trabalha", "origem",
                                    "escala_geradora_id", "gerada_em",
                                    "regenerada_em", "regeneracoes",
                                    "editada_em", "julgada_em",
                                )),
                                "dna": dna_resumo(cel.dna),
                                "dna_anterior": dna_resumo(cel.dna_anterior),
                            } if cel else None,
                            "ata": {k: ata.get(k) for k in (
                                "tipo_dia", "minutos_previstos",
                                "minutos_realizados", "n_celulas", "n_missing",
                            )},
                        })

                    # Mesma chamada do comando: pauta...py:135.
                    retorno = recalcular_fechamento_mes(
                        8, 2026, colaborador_ids=[pk],
                        somente_leitura=True, lavrar_dias_pagos=False,
                    )
                    assert pk in retorno, "recalculo omitiu colaborador"
                    dev = retorno[pk]

                    # Conferência: fechamento.py:1027-1033,1071-1083.
                    dentro = [
                        d for d in dias if d.get("tipo_dia") != "fora_vinculo"
                    ]
                    sem_resposta = bool(dentro) and all(
                        d.get("tipo_dia") == "sem_celula" for d in dentro
                    )
                    contagem = sum(
                        d.get("tipo_dia") in ("trabalho", "ausencia")
                        for d in dias
                    )
                    esperado = None if sem_resposta else contagem
                    assert dev["dias_previstos"] == esperado, (
                        "DEVIDO diverge da grade lida separadamente"
                    )

                    emitir("COMPARACAO", {
                        "col": pk, "pago": pago, "devido": dev,
                        "dias_no_anexo": ANEXO[pk],
                        "reproduziu_dias_anexo": (
                            pago["dias_previstos"], dev["dias_previstos"]
                        ) == ANEXO[pk],
                        "campos_pauta": [
                            k for k in pago if k not in ("mes", "ano")
                            and _diferente(pago[k], dev.get(k))
                        ],
                        "mudanca_none": [
                            k for k in pago if k not in ("mes", "ano")
                            and ((pago[k] is None) != (dev.get(k) is None))
                        ],
                    })
                    assert hash_do_gravado(c, 8, 2026) == h0, "hash mudou"

            except Exception as exc:
                ERROS["col%s" % pk] = {
                    "tipo": type(exc).__name__, "erro": str(exc),
                }

        transaction.set_rollback(True)

except Exception as exc:
    ERROS["sonda"] = {"tipo": type(exc).__name__, "erro": str(exc)}
finally:
    logging.Logger.callHandlers = logging_anterior
    emitir("ERROS", ERROS)
```

A última linha imprime `ERROS {}` quando nenhuma falha foi capturada; se houver falhas, imprime o dicionário correspondente, sem ocultá-las. **Resultado dessa execução: [nao verificado].**

## 06/11 19:33 · sessao 85381df901b3 · - · - · origem=Ronald (tmux)

A sonda pode explicar **qual entrada atual produz 1/21/18**, mas identificar **qual entrada mudou desde o PAGO** exige uma versão histórica vinculada à gravação. `dna_anterior` guarda apenas a versão imediatamente anterior, sem equivalência garantida com o PAGO (`app/escala/models.py:1714-1717`).

Referências essenciais:

- Modo correto: `somente_leitura=True, lavrar_dias_pagos=False`; a combinação com `lavrar_dias_pagos=True` escreve (`app/ponto/services/fechamento.py:524-531`).
- Retorno canônico: `dict` dos campos de `CAMPOS` (`app/ponto/services/fechamento.py:1112-1120`).
- Grade real: `folha.export.grade_do_fechamento` chama `grade_da_celula`, na janela da empresa (`app/folha/export.py:329-338`).
- O contador depende de `tipo_dia in ('trabalho','ausencia')`, inclusive com minutos previstos zero (`app/ponto/services/fechamento.py:1030-1033`).
- O cadastro de admissão/demissão determina `fora_vinculo`; os fatos lavrados interferem nessa decisão (`app/escala/services/leitor_celula.py:351-376`).
- `ata.tipo_dia` prevalece; sem ele, entra `cel.trabalha` (`app/escala/services/leitor_celula.py:380-381`).
- O vínculo interfere no fallback de **minutos** quando a ata não tem `minutos_previstos`, através da função real `minutos_previstos_do_dia` (`app/escala/services/leitor_celula.py:382-395`).
- Essa função devolve zero sem vínculo vigente ou sem marcos completos (`app/escala/utils.py:2251-2255`, `2281-2292`).
- A rotina pode engolir erro da grade e devolver campos `None`, e pode engolir erro do colaborador omitindo seu retorno; a sonda precisa verificar ambos (`app/ponto/services/fechamento.py:1071-1083`, `1287-1294`).

**Não usar `bin/sonda_leitura.sh` para cumprir “nenhum arquivo escrito”**: ele reescreve `logs/.env_leitor`, configura role/GRANT e pode iniciar a sombra (`bin/sonda_leitura.sh:103-104`, `121-143`, `157-159`).

Segue núcleo para shell Django **já iniciado** na SOMBRA. A configuração fixa banco `sombra`, host `sombra_db`; o schema continua `juliani` (`app/config/settings/sombra.py:13-14`, `29`, `47-51`). Não executei.

```python
# Executar como texto em shell SOMBRA existente; não salvar arquivo.
import json
import logging
from collections import Counter
from django.conf import settings
from django.db import connection, transaction
from django_tenants.utils import schema_context

ERROS = {}

def emitir(rotulo, valor):
    print(rotulo, json.dumps(valor, ensure_ascii=False, default=str))

def campos(obj, nomes):
    return {nome: getattr(obj, nome) for nome in nomes}

# Evita handlers de logging escreverem arquivos durante a sonda.
_logging_anterior = logging.root.manager.disable
logging.disable(logging.CRITICAL)

try:
    cfg = connection.settings_dict
    assert getattr(settings, "HASNER_SOMBRA", False)
    assert cfg["NAME"] == "sombra"
    assert cfg["HOST"] == "sombra_db"
    assert not connection.in_atomic_block

    with schema_context("juliani"), transaction.atomic():
        with connection.cursor() as cursor:
            cursor.execute(
                "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY"
            )
            cursor.execute("SHOW transaction_read_only")
            assert cursor.fetchone()[0] == "on"

        from colaboradores.models import Colaborador
        from escala.models import EscalaColaborador
        from escala.alimentacao import (
            celulas_do_periodo, escalas_do_periodo, vinculo_do_dia,
        )
        from folha.export import grade_do_fechamento
        from ponto.janelas import janela_fechamento
        from ponto.models import FechamentoMensal
        from ponto.portas.regen_exportada import (
            valores_do_gravado, hash_do_gravado,
        )
        from ponto.services.fechamento import (
            recalcular_fechamento_mes, universo_da_competencia,
        )

        for pk in (842, 284, 433):
            try:
                with transaction.atomic():
                    c = Colaborador.objects.select_related("empresa").get(pk=pk)
                    ini, fim = janela_fechamento(8, 2026, c.empresa)
                    antes = hash_do_gravado(c, 8, 2026)
                    fe = FechamentoMensal.objects.filter(
                        colaborador=c, mes=8, ano=2026
                    ).first()
                    assert fe is not None, "sem FechamentoMensal"

                    emitir("COLAB", {
                        "id": pk,
                        **campos(c, (
                            "empresa_id", "situacao",
                            "data_admissao", "data_demissao",
                        )),
                        "janela": [ini, fim],
                        "dia_inicio_competencia":
                            c.empresa.dia_inicio_competencia,
                        "no_universo":
                            universo_da_competencia(8, 2026)
                            .filter(pk=pk).exists(),
                    })
                    emitir("PAGO", {
                        "id": pk,
                        "valores": valores_do_gravado(c, 8, 2026),
                        "carimbos": campos(fe, (
                            "previsto_em", "derivado_em", "atualizado_em",
                        )),
                        "hash": antes,
                    })

                    celulas = celulas_do_periodo([pk], ini, fim)
                    geradoras = {
                        cel.escala_geradora_id
                        for cel in celulas.values()
                        if cel.escala_geradora_id
                    }
                    selecionadas = escalas_do_periodo(
                        pk, ini, fim, mais=geradoras
                    )
                    emitir("VINCULOS", {
                        "id": pk,
                        "selecionados": [e.pk for e in selecionadas],
                        "todos": list(
                            EscalaColaborador.objects.filter(colaborador=c)
                            .order_by("data_inicio", "pk").values(
                                "id", "data_inicio", "data_fim", "ativa",
                                "tipo_escala_id", "posto_id", "regua_classe",
                            )
                        ),
                    })

                    # Mesma função que a derivação usa; não remonta a grade.
                    grade = grade_do_fechamento(fe)
                    dias = sorted(
                        grade.get("dias") or [], key=lambda d: d["data"]
                    )
                    emitir("TIPOS_GRADE", {
                        "id": pk,
                        "tipos": dict(Counter(d["tipo_dia"] for d in dias)),
                    })

                    for dia in dias:
                        data = dia["data"]
                        cel = celulas.get((pk, data))
                        ec = vinculo_do_dia(
                            pk, data, selecionadas, celulas=celulas
                        )
                        ata = (cel.ata or {}) if cel else {}
                        emitir("DIA", {
                            "id": pk,
                            "data": data,
                            "tipo_grade": dia.get("tipo_dia"),
                            "conta_dias_previstos":
                                dia.get("tipo_dia") in ("trabalho", "ausencia"),
                            "previsto_grade": dia.get("minutos_previstos"),
                            "realizado_grade": dia.get("minutos_realizados"),
                            "vinculo_resolvido": ec.pk if ec else None,
                            "celula": campos(cel, (
                                "pk", "trabalha", "origem",
                                "escala_geradora_id", "gerada_em",
                                "regenerada_em", "regeneracoes",
                                "editada_em", "julgada_em",
                                "dna", "dna_anterior",
                            )) if cel else None,
                            "ata": {
                                k: ata.get(k) for k in (
                                    "tipo_dia", "minutos_previstos",
                                    "minutos_realizados", "n_celulas",
                                    "n_missing", "sem_ancora",
                                )
                            },
                        })

                    retorno = recalcular_fechamento_mes(
                        8, 2026, colaborador_ids=[pk],
                        somente_leitura=True, lavrar_dias_pagos=False,
                    )
                    assert pk in retorno, "recalculo omitiu colaborador"
                    dev = retorno[pk]
                    emitir("DEVIDO", {"id": pk, "valores": dev})

                    dentro = [
                        d for d in dias
                        if d.get("tipo_dia") != "fora_vinculo"
                    ]
                    sem_resposta = bool(dentro) and all(
                        d.get("tipo_dia") == "sem_celula" for d in dentro
                    )
                    esperado = None if sem_resposta else sum(
                        d.get("tipo_dia") in ("trabalho", "ausencia")
                        for d in dias
                    )
                    assert dev["dias_previstos"] == esperado, (
                        "dias_previstos diverge da grade independente"
                    )
                    assert hash_do_gravado(c, 8, 2026) == antes, (
                        "gravado mudou"
                    )
            except Exception as exc:
                ERROS["col%s" % pk] = {
                    "tipo": type(exc).__name__, "erro": str(exc),
                }
        transaction.set_rollback(True)
except Exception as exc:
    ERROS["sonda"] = {
        "tipo": type(exc).__name__, "erro": str(exc),
    }
finally:
    logging.disable(_logging_anterior)
    emitir("ERROS", ERROS)
```

`ERROS {}` só será impresso se não houver falhas. A sonda não atribui causalidade histórica por aproximação.

## 06/11 19:32 · sessao e88d81ca5b20 · - · - · origem=Ronald (tmux)

Achados para consolidar, todos somente por leitura:

- O PAGO lê atributos do `FechamentoMensal` gravado, sem transformar valores: `app/ponto/portas/regen_exportada.py:77-84`. O DEVIDO entrega os mesmos atributos do objeto recalculado, recortados pelo mesmo `CAMPOS`: `app/ponto/services/fechamento.py:1112-1120`. São 26 chaves, incluindo `mes/ano`: `app/ponto/management/commands/aplicar_09_corte_b.py:67-74`.
- O comando compara 24 dessas chaves, excluindo `mes/ano`, sem reconciliar universo histórico nem converter unidades específicas de cada campo: `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:139`. Contudo, não é igualdade literal: `_diferente` converte `a or 0` e `b or 0` para `float`, usando tolerância absoluta `> 0.015`; na exceção compara `(a or None) != (b or None)`: `app/ponto/portas/regen_exportada.py:248-252`. Assim, `None` e zero são equiparados.
- Essa equiparação perde distinção real: a derivação grava `None` quando não consegue obter resposta da grade ou todos os dias internos são `sem_celula`; zero pode ser resposta da grade: `app/ponto/services/fechamento.py:1027-1033`, `app/ponto/services/fechamento.py:1071-1083`.
- `_fmt` só apresenta valores com até quatro casas; não corrige semântica ou universo: `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:45-50`, `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:176-177`.
- No código atual, não achei prova de que algum campo homônimo tenha definição diferente entre os dois lados: a derivação escreve os mesmos atributos que o modo de escrita persiste: `app/ponto/services/fechamento.py:1067-1087`, `app/ponto/services/fechamento.py:1121-1128`, `app/ponto/services/fechamento.py:1144-1169`. Sem histórico de entrada/escrita, a semântica efetivamente usada quando o PAGO nasceu permanece **[nao verificado]**.
- `dias_previstos` conta **linhas da grade cujo `tipo_dia` é `trabalho` ou `ausencia`**, mesmo que tenham zero minutos: `app/ponto/services/fechamento.py:1030-1033`. A grade é lida na janela da competência: `app/folha/export.py:329-338`. Não é contagem de batidas, marcos ou turnos.
- O tipo vem prioritariamente de `ata.tipo_dia`, depois de `cel.trabalha`; os minutos vêm de `ata.minutos_previstos`, e só na ausência dessa chave consultam a autoridade atual: `app/escala/services/leitor_celula.py:380-395`. A grade aplica admissão/demissão atuais, com exceções por fato lavrado: `app/escala/services/leitor_celula.py:351`, `app/escala/services/leitor_celula.py:368-378`. Portanto, encerrar ou criar vínculo de escala não implica, sozinho, alterar a contagem se a célula/ata e o envelope de emprego continuarem iguais.
- Para col842, **condicionalmente a esse caminho normal**, `dias_previstos=1` significa exatamente uma linha contável; `minutos_previstos=0` não impede essa contagem: `app/ponto/services/fechamento.py:1030-1033`. Qual linha e qual mudança fizeram isso: **[nao verificado]**.
- Evitar supor `horas_trabalhadas × 60 == minutos_realizados`: estes últimos vêm da grade, com teto por dia e universo `trabalho/ausencia`: `app/ponto/services/dia_pago.py:384-400`, `app/ponto/services/fechamento.py:1044-1048`. `horas_trabalhadas` inclui minutos órfãos de vínculo e folga trabalhada sem escala certa: `app/ponto/services/fechamento.py:893-908`, `app/ponto/services/fechamento.py:964-969`. O próprio modelo distingue relógio da grade de hora líquida da folha: `app/ponto/models.py:440-445`.
- Leitor de `dias_previstos` além do comando: `prontidao` verifica o snapshot e usa o campo para detectar denominador zero: `app/folha/export.py:1037-1043`; no fallback reconta o mesmo universo `trabalho/ausencia`: `app/folha/export.py:1087-1091`.
- DSR vem do resultado do motor, e não da contagem da grade: `app/ponto/services/fechamento.py:1085-1087`; na composição por fatias, semanas e reflexo são somados: `app/ponto/services/fechamento.py:98-99`. Não confundir aumento de `semanas_dsr_ok` com prova de aumento dos mesmos dias contados em `dias_previstos`.

## 06/11 19:32 · sessao 192804a2c8ca · - · - · origem=Ronald (tmux)

Achados com referências da cópia:

- **PAGO** é o valor atual de `FechamentoMensal.dias_previstos`: `valores_do_gravado` consulta colaborador/mês/ano e devolve os atributos definidos em `CAMPOS`, sem recalcular nem transformar (`app/ponto/portas/regen_exportada.py:77-84`). O campo é inteiro anulável, descrito como “Dias de trabalho previstos” (`app/ponto/models.py:446`).
- **DEVIDO** atribui `dias_previstos = len(_dt_trab)`, com `_dt_trab` sendo os dias da grade cujo `tipo_dia` é `trabalho` ou `ausencia`; não exige minutos positivos (`app/ponto/services/fechamento.py:1030-1034`, `app/ponto/services/fechamento.py:1067-1069`).
- A grade vem da função real `grade_do_fechamento`, que resolve a janela pela empresa atual e chama `grade_da_celula` (`app/folha/export.py:329-338`). O corte é lido de `empresa.dia_inicio_competencia`, com default 21 e clamp 2–28 (`app/ponto/janelas.py:36-57`); corte 21 produz 21 do mês anterior a 20 do mês da competência (`app/ponto/janelas.py:10-21`). Corte efetivo histórico dos três colabs: **[nao verificado]**.
- O universo diário da contagem é decidido por `grade_da_celula`: primeiro admissão/demissão atuais e fatos da ata determinam `fora_vinculo`; depois ausência de célula vira `sem_celula`; havendo célula, prevalece `ata.tipo_dia`, com fallback para `cel.trabalha` (`app/escala/services/leitor_celula.py:350-381`). `fora_vinculo` aqui usa datas de vida do colaborador; não corresponde simplesmente à vigência de `EscalaColaborador` (`app/escala/services/leitor_celula.py:351-381`).
- A demissão exclui dias posteriores sempre; no próprio dia exclui quando não há fato na ata. Antes da admissão, fato na ata pode preservar o dia (`app/escala/services/leitor_celula.py:368-376`).
- A alteração isolada de `EscalaColaborador` não aparece na seleção de `tipo_dia` que alimenta a contagem; a escala entra no fallback de **minutos**, quando a ata não os contém (`app/escala/services/leitor_celula.py:380-395`). Troca/encerramento de vínculo pode mudar a contagem por alterações subsequentes nas células/atas; atribuir isso aos três casos é **[nao verificado]**.
- Regeneração efetivamente altera `cel.trabalha`, geradora e DNA, preservando apenas o DNA imediatamente anterior, com carimbo/contador (`app/ponto/portas/celula.py:263-277`). A regeneração normalmente pula competência lavrada (`app/ponto/portas/celula.py:250-254`); existe abertura por ato com motivo (`app/ponto/portas/celula.py:197-210`). Ter ocorrido abertura nesses casos: **[nao verificado]**.
- A unidade pretendida é contagem de dias nos dois lados, e o escritor atual persiste exatamente a mesma derivação: `save()` quando aberto; `update(... dias_previstos=...)` nos demais estados (`app/ponto/services/fechamento.py:1121-1128`, `app/ponto/services/fechamento.py:1144-1166`). A versão do algoritmo e as entradas que produziram o PAGO histórico não constam do anexo: **[nao verificado]**.
- Uma grade com todos os dias internos `sem_celula` produz `None`, não zero nem 1; cobertura parcial não aciona essa abstenção (`app/ponto/services/fechamento.py:1027-1029`, `app/ponto/services/fechamento.py:1071-1083`).
- Portanto, **o caminho normal que devolveu 1 contou exatamente um dia `trabalho`/`ausencia`, mesmo com minutos previstos zero** (`app/ponto/services/fechamento.py:1030-1033`). Qual entrada histórica mudou de 16 para 1 é **[nao verificado]**. O anexo não permite escolher entre mudança nas datas de vida, tipos nas atas/células, cobertura de células, janela empresarial ou algoritmo histórico.
- O universo de pessoas é outra etapa: desligados entram somente com demissão não anterior ao início da janela; desligado com demissão nula fica fora (`app/ponto/services/fechamento.py:422-433`). O filtro `colaborador_ids` apenas restringe esse universo (`app/ponto/services/fechamento.py:545-548`). Estar fora não gera `dias_previstos=1`: gera ausência do retorno, e o comando recusa o ato (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:135-144`).
- Leitor adicional do campo: prontidão utiliza `dias_previstos` junto a `minutos_previstos` quando aceita o snapshot (`app/folha/export.py:1037-1043`).

_2479 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
