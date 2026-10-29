# CODEX-ESTADO

> Ultima resposta processada: **29/10/2026 18:38** · publicado em **29/10/2026 18:39** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 29/10 18:38 · sessao d7b0b2d362e8 · justif · verificador · origem=Code

**(a) Há regressões no fechamento do bloqueio.** Ocorrência em produção e execução da suíte: `[nao verificado]`. [nao verificado]

1. **P1 — pouso antes do pré-requisito: fechamento com premissa viva.** Contraexemplo: bloqueio B aberto, `data_turno=D2`, entrada sem saída, sem célula, perguntas, justificativa relacionada ou outra cobrança; aprovar J de D1, tipo `outro`, sem saída. Antes, `retratar → reconciliar` recusava porque o turno continuava aberto; depois, a retenção retorna `None` e `encerrar()` fecha B sem consultar a premissa. O filtro sem dia já existia; **a retirada do gate é nova**. O cartório não corrige esse exemplo porque não encontra célula. (`app/ponto/services/justificativa.py:71-83`; `app/chamados/models.py:467-471,495-525,978-987,1192-1202`; `app/chamados/catalogo/premissa.py:48`; `app/ponto/services/cartorio.py:1547-1549`)

2. **P2 — perda da trilha que associa o fechamento à aprovação.** Entrada: B aberto, sem dia nem retenção; J sem chamado de origem. Antes, o conteúdo “Justificativa #J aprovada…” chegava à ocorrência de retratação; depois, B fecha com apenas a trilha genérica de estado, sem essa ocorrência. O followup posterior da aprovação também não alcança B, pois depende de `j.chamado_origem`. (`app/ponto/services/justificativa.py:83,221-222`; `app/chamados/reconciliador.py:523-525`; `app/chamados/models.py:227-231,1201`; `app/chamados/utils.py:7-9`)

3. **P2 — enfraquecimento da atomicidade sob falha na resolução das perguntas.** Entrada: B com pergunta morta elegível, cujo `save()` falha persistentemente. Antes, o receiver engolia a primeira falha, mas a chamada explícita de `_retratar` repetia a resolução e propagava o erro, revertendo aprovação e fechamento. Depois, resta apenas o receiver best-effort: pode terminar com J aprovada, B fechado e pergunta sem materialização. É contrafactual de falha; ocorrência real `[nao verificado]`. (`app/chamados/signals.py:88-94`; `app/chamados/reconciliador.py:521`; `app/chamados/services/disputa_emissao.py:1547-1583`; `app/ponto/services/justificativa.py:207-211`; `app/chamados/models.py:1201-1202`)

4. **P3 — contrato de retorno incorreto no escritor novo.** Retenção inédita + falha ao criar o log → `evento()` devolve `None`, nenhuma linha nasce, mas `registrar_retencao()` responde `True` (“gravou agora”). Os três consumidores atuais ignoram esse retorno, limitando o impacto presente. (`app/chamados/reconciliador.py:461,484-485,505`; `app/core/observ.py:105,133-139`; `app/ponto/services/justificativa.py:81,390`)

Não sustento perda do Fix A nesta porta: J já está aprovada quando o bloqueio é tratado; outra justificativa pendente continua retendo o chamado. (`app/ponto/services/justificativa.py:193-206`; `app/chamados/reconciliador.py:453-455`)

**(b) `_retratar`: comportamento preservado para as saídas reais do produtor.**

- **Dedup igual:** consulta o último log por ação/modelo/chamado; compara os primeiros oito IDs para `irma_orfa`, ou somente `justificativa_pendente` para justificativa alheia. Continua ignorando mudanças depois do oitavo ID e mudanças apenas em `em_aprovacao`. Essas limitações são anteriores. (`app/chamados/reconciliador.py:470-482`)
- **Retorno igual:** retenção sempre resulta em `False`, independentemente do retorno do escritor; sem retenção, permanece o mesmo caminho de reconciliação. (`app/chamados/reconciliador.py:503-509,526`)
- **Ordem dos efeitos igual:** retenção/trilha antes do `atomic`; depois reconciliação, Fix A, resolução das perguntas e ocorrência. Mudou apenas a preparação: o auxiliar importa `evento` e monta descrição/payload antes de consultar a dedup, inclusive quando não escreverá. (`app/chamados/reconciliador.py:466-484,503-525`)
- **Exceção sintática, não alcançável pelo produtor atual:** `('irma_orfa', [])` antes prosseguiria e agora reteria; o produtor só devolve essa categoria com lista não vazia. (`app/chamados/reconciliador.py:449-451,504-506`)

**(c) Os testes não demonstram cada cura isoladamente.** Os contrafactuais abaixo são de leitura estática; execução `[nao verificado]`. [nao verificado]

| Teste novo/alterado | Falharia sem a cura? | [nao verificado]
|---|---|
| `test_BLOQUEIO_aberto_e_resolvido_FECHAM_pela_porta_sem_trilha_de_dia` | **Sim**, ao restaurar `retratar`: exige `fechado`, enquanto `RECONCILIAR` leva aberto a resolvido e não sai de resolvido. (`app/ponto/tests/test_justificativa_encerra_origem.py:486-502`; `app/chamados/catalogo/motor.py:111-115`) |
| `test_BLOQUEIO_com_irma_respondida_fica_e_a_retencao_se_diz` | **Já passaria no pai**, cuja unidade já retinha e registrava. Protege a guarda no novo caminho, mas não prova uma cura exclusiva deste delta. (`app/ponto/tests/test_justificativa_encerra_origem.py:504-516`; `app/chamados/reconciliador.py:503-506`) |
| `test_TRAVA_inclui_o_bloqueio_RESOLVIDO` | **Falha no pai por receber `None`**, embora a inclusão do resolvido já constasse no queryset. Remover `select_for_update()` mantendo a lista deixa a asserção passar: não prova trava nem ordem. (`app/ponto/tests/test_justificativa_encerra_origem.py:518-527`; `app/ponto/services/justificativa.py:110-117`) |
| `test_CASCATA_retida_grava_a_trilha_da_retencao` | **Falso verde sem a chamada da cascata.** A fixture também faz `ch` ser origem; depois da cascata, `encerrar_chamado_de_origem → retratar → registrar_retencao` satisfaz `m.called` e `.exists()`. (`app/ponto/tests/test_justificativa_encerra_origem.py:59-61,536,550-551`; `app/ponto/services/justificativa.py:390,400,42`; `app/chamados/reconciliador.py:505`) |
| `test_PORTAS_UM_A_UM_saida_None_dizem_a_recusa` | **Morde as guardas de `None`, mas elas já existiam no pai** e não são curas deste delta. (`app/ponto/tests/test_justificativa_encerra_origem.py:553-580`; `app/ponto/services/justificativa.py:183-187,358-362`) |
| Fixture adicional em `test_LOTE_saida_RECUSADA_pela_porta_desfaz_o_item` | Fortalece a prova da **cura anterior**: a justificativa com saída nula evita rollback acidental por anotação de `None`. (`app/ponto/tests/test_justificativa_encerra_origem.py:321-340`; `app/ponto/views.py:1822-1835`) |
| `bulk_create` em `test_LOTE_saida_de_OUTRA_justificativa_desfaz_o_item` | Corrige a preparação para chegar ao lote; a recusa de saída pertencente a outra justificativa já estava implementada antes deste delta. (`app/ponto/tests/test_justificativa_encerra_origem.py:453-474`; `app/ponto/views.py:1826-1829`) |

Os novos testes de trilha verificam existência, sem repetição/contagem; não demonstram a dedup do escritor nem seu retorno sob falha. (`app/ponto/tests/test_justificativa_encerra_origem.py:516,548-551`; `app/chamados/reconciliador.py:482-485`)

## 29/10 18:37 · sessao ae7a33a452e5 · - · - · origem=Ronald (tmux)

Achado principal: **P1 — fechamento antes da premissa regularizada**, introduzido por `return ch.encerrar()` em `app/ponto/services/justificativa.py:83`.

Contraexemplo permitido pelo código: B aberto, módulo `entrada_bloqueada_turno_aberto`, `contexto_json={'data_turno': D2}`, sem justificativa relacionada, perguntas ou outra cobrança viva de D2; E de D2 sem S; nenhuma `CelulaDia` de D2; aprovar J de D1, tipo `outro`, sem saída. A associação com célula pode legitimamente permanecer nula (`app/chamados/models.py:895`, `app/chamados/models.py:911`).

Antes, `retratar → reconciliar → _gate_premissa_do_fechamento` recusava B: o dia conhecido permanecia com turno aberto, sem exceção por dedup (`app/chamados/models.py:467`, `app/chamados/models.py:495`, `app/chamados/models.py:978`, `app/chamados/catalogo/premissa.py:48`). Depois, retenção retorna `None`, e `encerrar` força B para fechado sem consultar a premissa (`app/ponto/services/justificativa.py:79`, `app/chamados/models.py:1192`). O receiver não corrige este caso: consulta D2, mas cartório retorna sem agir quando não encontra célula (`app/chamados/signals.py:177`, `app/ponto/services/cartorio.py:1547`). Filtro sem dia é anterior; **remoção da guarda é nova**. Ocorrência em produção `[nao verificado]`.

Outras diferenças concretas da troca:

- **P2 — perda da ocorrência que identifica J no bloqueio.** Antes, o conteúdo customizado da chamada antiga chegava à ocorrência `tipo='retratado'`; agora somente a transição de estado é escrita. A unidade anexa a ocorrência em `app/chamados/reconciliador.py:523`; o novo caminho termina em `app/ponto/services/justificativa.py:83`, passando por `app/chamados/models.py:1201`. Contraexemplo: bloqueio aberto sem dia e sem retenção → antes havia ocorrência com “Justificativa #J aprovada”; depois, apenas trilha genérica de estado (`app/chamados/models.py:227`).

- **P2 — enfraquecimento da atomicidade das perguntas sob falha.** Se uma pergunta morta elegível falhar persistentemente no `save`, antes o receiver engolia a primeira falha, mas a chamada explícita de `_retratar` repetia a resolução e propagava a falha, desfazendo aprovação e fecho; agora somente o receiver executa, engole a falha e permite aprovada+fechado com pergunta não materializada (`app/chamados/signals.py:88`, `app/chamados/reconciliador.py:521`, `app/ponto/services/justificativa.py:207`, `app/chamados/services/disputa_emissao.py:1583`). Falha real em produção `[nao verificado]`.

**Não sustento regressão do Fix A nesta porta:** ao chegar ao bloqueio, J já foi aprovada; outra justificativa pendente é barrada pela retenção (`app/ponto/services/justificativa.py:193`, `app/ponto/services/justificativa.py:206`, `app/chamados/reconciliador.py:453`).

**Retorno do escritor:** `registrar_retencao` promete “gravou agora”, mas retorna `True` mesmo se `evento` falhar e devolver `None`; nenhum dos três consumidores usa esse retorno (`app/chamados/reconciliador.py:461`, `app/chamados/reconciliador.py:484`, `app/core/observ.py:139`, `app/ponto/services/justificativa.py:81`, `app/ponto/services/justificativa.py:390`, `app/chamados/reconciliador.py:505`).

## 29/10 18:37 · sessao dd83c7dae16a · grupo1p1 · verificador · origem=Code

- **Classe 3 — `app/api/tests/test_o21_app_diz_o_que_foi_decidido.py:169`**: acrescenta `timezone.now()` ao caso que já lê `_tz.localdate()` na linha 167. São duas leituras, sob o congelamento da linha 39.

- **Classe 3 — `app/ponto/tests/test_piso_nao_sobe_por_batida.py:123`**: acrescenta `timezone.now()` depois de `timezone.localdate()` na linha 122. São duas leituras, sob o congelamento da linha 37.

- **Classe 3 — `app/core/tests/test_espelho_tela_le_a_folha.py:70`**: o caso que chama `_dois_resumos()` na linha 81 executa dois `timezone.now()`, nas linhas 69 e 70, sob o congelamento da linha 39.

- **Classe 3 — `app/core/tests/test_espelho_tela_le_a_folha.py:133`**: este outro caso executa três leituras: duas pelo helper chamado na linha 126, nas linhas 69 e 70, e outra na linha 133; o congelamento está na linha 39.

- **Classe 3 — `app/relatorios/tests/test_r3_em_aberto_alcanca_turno_aberto.py:251`**: `test_05` chama `_tela()` e `_cartao()`, que leem `timezone.now()` separadamente nas linhas 187 e 191.

- **Classe 3 — `app/relatorios/tests/test_r3_em_aberto_alcanca_turno_aberto.py:378`**: `test_11` lê pelo `_tela()` da linha 377, cujo relógio está na linha 187, e novamente para o cartão na linha 378.

- **Classe 3 — `app/relatorios/tests/test_r3_em_aberto_alcanca_turno_aberto.py:391`**: `test_12` lê pelo `_tela()` da linha 390, cujo relógio está na linha 187, e novamente para o cartão na linha 391.

Total por classe: 3 = 7 casos; 5 = 0; 7 = 0; 8 = 0.

## 29/10 18:37 · sessao f88ca1018fe0 · - · - · origem=Ronald (tmux)

Leitura estática; execução dos testes `[nao verificado]`. [nao verificado]

| Teste novo/alterado | Sem a cura correspondente | [nao verificado]
|---|---|
| `test_BLOQUEIO_aberto_e_resolvido_FECHAM_pela_porta_sem_trilha_de_dia` | **Morde** a troca `retratar → encerrar`: exige `fechado` e ausência de `fechamento_sem_dia_do_fato`; `reconciliar` produz `resolvido` e não transiciona de `resolvido`. `app/ponto/tests/test_justificativa_encerra_origem.py:486`, `:501`, `:502`; `app/chamados/catalogo/motor.py:111`; `app/ponto/services/justificativa.py:83`. |
| `test_BLOQUEIO_com_irma_respondida_fica_e_a_retencao_se_diz` | **Já passaria no pai**: o antigo `retratar` já consultava retenção e escrevia a trilha, conforme patch. Protege o novo `encerrar` contra perder a guarda, mas não demonstra cura de defeito introduzido no pai. `app/ponto/tests/test_justificativa_encerra_origem.py:504`, `:515`, `:516`; `app/chamados/reconciliador.py:503`. |
| `test_TRAVA_inclui_o_bloqueio_RESOLVIDO` | **Falha no pai pelo retorno `None`**, embora o bloqueio resolvido já integrasse o queryset no pai, conforme contexto do patch. Não prova lock nem ordem: remover `select_for_update()` preservando a lista deixa a asserção satisfeita. `app/ponto/tests/test_justificativa_encerra_origem.py:518`, `:526`, `:527`; `app/ponto/services/justificativa.py:112`, `:117`. |
| `test_CASCATA_retida_grava_a_trilha_da_retencao` | **Falso verde removendo somente a chamada da cascata** em `app/ponto/services/justificativa.py:390`: a fixture faz `ch` ser origem de `j` (`app/ponto/tests/test_justificativa_encerra_origem.py:536`, `:59`). Após a cascata, `encerrar_chamado_de_origem` chama `retratar`, que chama o mesmo escritor; `m.called` e a existência da trilha ficam satisfeitos por esse caminho. `app/ponto/services/justificativa.py:400`, `:42`; `app/chamados/reconciliador.py:503`; `app/ponto/tests/test_justificativa_encerra_origem.py:550`, `:551`. |
| `test_PORTAS_UM_A_UM_saida_None_dizem_a_recusa` | **Morde as guardas de `None`, mas elas são anteriores ao delta**: exige mensagem contendo `recusada`; sem as guardas, a outra justificativa com `batida_gerada=NULL` desvia para “Saida ja materializada”. `app/ponto/tests/test_justificativa_encerra_origem.py:553`, `:561`, `:577`, `:579`; `app/ponto/services/justificativa.py:183`, `:185`, `:358`, `:360`. |
| Fixture adicional de `test_LOTE_saida_RECUSADA_pela_porta_desfaz_o_item` | Fortalece teste de **cura anterior** em `app/ponto/views.py:1822`: outra justificativa com saída nula evita que uma exceção acidental em anotação produza rollback e falso verde. `app/ponto/tests/test_justificativa_encerra_origem.py:321`, `:331`, `:335`, `:340`; `app/ponto/views.py:1826`, `:1833`. |
| `bulk_create` em `test_LOTE_saida_de_OUTRA_justificativa_desfaz_o_item` | Corrige a preparação para medir o lote, mantendo J pendente até o mock; o teste exige que `registrar_batida` seja chamado e que o item volte inteiro. A cura correspondente `_outra is not None → raise` já está fora do delta. `app/ponto/tests/test_justificativa_encerra_origem.py:453`, `:461`, `:469`, `:471`, `:474`; `app/ponto/views.py:1827`. |

Os testes novos não exercitam deduplicação repetida nem retenção por `justificativa_alheia`: as novas verificações de trilha usam apenas `irma_orfa` e `.exists()`, sem contagem ou segunda tentativa. `app/ponto/tests/test_justificativa_encerra_origem.py:516`, `:551`; a dedup reside em `app/chamados/reconciliador.py:473`, `:479`, `:482`.

## 29/10 18:36 · sessao 761d79942cbe · - · - · origem=Ronald (tmux)

Para os retornos efetivamente produzidos por `retencao_da_retratacao`, `_retratar` preservou comportamento: `None` segue para `reconciliar`; retenção registra quando necessário e devolve `False`, antes de entrar no `atomic`. O produtor só emite `irma_orfa` com lista não vazia ou `justificativa_alheia`. (`app/chamados/reconciliador.py:449`, `app/chamados/reconciliador.py:453`, `app/chamados/reconciliador.py:503`, `app/chamados/reconciliador.py:507`)

A deduplicação mantém exatamente as chaves anteriores: primeiros oito IDs em `irma_orfa`; ID da justificativa em `justificativa_alheia`, sem considerar `em_aprovacao`; busca do último registro por ação, modelo e chamado, ordenado por `-id`. Isso preserva inclusive limitações antigas: mudança após o oitavo ID não aparece e outra aprovação retida pela mesma justificativa não cria linha. (`app/chamados/reconciliador.py:473`, `app/chamados/reconciliador.py:479`, `app/chamados/reconciliador.py:480`)

Há um defeito novo, de contrato do auxiliar, com impacto baixo nos consumidores atuais: `registrar_retencao` promete “gravou agora”, mas retorna `True` incondicionalmente após `evento`. Entrada: retenção inédita e falha na criação do log → `evento` engole a falha e devolve `None` → nenhuma trilha gravada, auxiliar informa `True`. Os três consumidores ignoram esse retorno, portanto isso não altera `_retratar`. Confirmei a contraprova em execução isolada em memória do corpo da função, com `evento` devolvendo `None`. (`app/chamados/reconciliador.py:461`, `app/chamados/reconciliador.py:484`, `app/core/observ.py:105`, `app/core/observ.py:133`, `app/core/observ.py:139`, `app/chamados/reconciliador.py:505`, `app/ponto/services/justificativa.py:81`, `app/ponto/services/justificativa.py:390`)

A distinção sintática `('irma_orfa', [])` passaria antes e reteria depois, mas esse valor não sai do produtor real; não constitui regressão alcançável demonstrada. (`app/chamados/reconciliador.py:449`, `app/chamados/reconciliador.py:450`, `app/chamados/reconciliador.py:504`)

Os testes novos não exercitam deduplicação nem retorno do auxiliar: o de bloqueio verifica existência da trilha; o de cascata verifica uma chamada genérica e existência. Os testes antigos de deduplicação cobrem duas passadas e uma nova pendência, mas não foram introduzidos neste delta. (`app/ponto/tests/test_justificativa_encerra_origem.py:516`, `app/ponto/tests/test_justificativa_encerra_origem.py:550`, `app/ponto/tests/test_justificativa_encerra_origem.py:551`, `app/chamados/tests/test_laco_hx_x_bug31.py:138`, `app/chamados/tests/test_laco_hx_x_bug31.py:151`)

## 29/10 18:36 · sessao 34927c8ab64c · - · - · origem=Ronald (tmux)

- **Classe 3 — app/api/tests/test_o21_app_diz_o_que_foi_decidido.py:169:** acrescenta `timezone.now()` ao caso que já lê o relógio em `:167` (`_tz.localdate()`). O congelamento está em `:39`; são duas leituras do mesmo instante.
- **Classe 3 — app/ponto/tests/test_piso_nao_sobe_por_batida.py:123:** acrescenta `timezone.now()` depois de `timezone.localdate()` em `:122`. O congelamento está em `:37`; são duas leituras do mesmo instante.

Nos sítios revisados, nenhum achado novo das classes 5, 7 ou 8. O `sorted(out)` em app/escala/tests/test_selo_grade_pede_o_instante.py:60 corrige a ordem exigida pela asserção `:188-190`; a execução em memória confirmou que retirar a ordenação faz essa asserção divergir.

Total dos sítios revisados: classe 3 = 2; classe 5 = 0; classe 7 = 0; classe 8 = 0.

## 29/10 18:36 · sessao 541cd319928f · - · - · origem=Ronald (tmux)

Classe 3, verificável sem depender de `freeze_time`:

- `app/relatorios/tests/test_r3_em_aberto_alcanca_turno_aberto.py:251`: `test_05` chama `_tela()` e `_cartao()`; cada helper lê `timezone.now()` separadamente, nas linhas 187 e 191.
- `app/relatorios/tests/test_r3_em_aberto_alcanca_turno_aberto.py:378`: `test_11` combina `_tela()` (leitura na linha 187) com outra leitura explícita de `timezone.now()`.
- `app/relatorios/tests/test_r3_em_aberto_alcanca_turno_aberto.py:391`: `test_12` lê por `_tela()` na linha 390/187 e novamente para o cartão na linha 391.

A classe base e esses três métodos não possuem congelamento (`app/relatorios/tests/test_r3_em_aberto_alcanca_turno_aberto.py:76`, `:240`, `:370`, `:385`); são leituras do relógio real.

Não demonstrei achado adicional nas classes 5, 7 ou 8. A alteração de `_resumo` sob congelamento não remove a mordida dos casos 13/14: continuam exigindo presença do fato e retirada da palavra enquanto vivo, e retorno da palavra depois (`app/relatorios/tests/test_r3_em_aberto_alcanca_turno_aberto.py:426`, `:433`, `:436`, `:441`, `:446`).

## 29/10 18:35 · sessao 3bb17ba4bb04 · justif · sonda · origem=Code

Pela saída fornecida:

- **Total: 112 chamados.** [nao verificado]
- **Vivos: 0; encerrados: 112**, sendo 82 `fechado` e 30 `resolvido`. [nao verificado] Esses dois estados são classificados como encerrados em [motor.py:30](/home/ronald/codex_ws/02c3297c2af5f7c6be04f5dc4cf4f704f6e09de4/app/chamados/catalogo/motor.py:30).
- **Com `data_turno` no contexto: 0; sem `data_turno`: 112.** [nao verificado]
- **Por mês de criação:** maio/2026: 22; junho/2026: 89; outubro/2026: 1. [nao verificado]
- **Nenhuma incoerência interna aparente:** estados somam `82 + 30 = 112`; meses somam `22 + 89 + 1 = 112`; todos os 112 aparecem sem `data_turno`; consta `ERROS=0` e está presente `FIM-DA-SONDA`. [nao verificado]

## 29/10 18:35 · sessao 4ef33dfda508 · - · - · origem=Ronald (tmux)

Nenhum achado verificável nas quatro áreas delegadas.

- HE100: congelamento em `app/ponto/tests/test_he100_dia_sem_previsto.py:108`; única leitura explícita no helper em `:117`; mantém asserções positiva e negativa em `:122` e `:130`.
- O145: congelamento em `app/ponto/tests/test_o145_testemunha_le_a_acusacao_do_motor.py:56`; única leitura explícita em `:103`; fixture medida de 02/10 em `:47` é anterior ao congelamento.
- O146: congelamento em `app/ponto/tests/test_o146_extra_declarada_da_escala.py:541`; única leitura explícita em `:610`; casos continuam cobrando 7 e 67 minutos em `:622` e `:631`.
- Juiz do dia: subclasses congeladas em `app/ponto/tests/test_espelho_tela_le_o_juiz_do_dia.py:88` e `:116`; única leitura explícita do helper em `:84`; asserções de batidas/card seguem presentes em `:96`, `:105`, `:112`, `:126`.

O `agora` recebido pelo builder ainda não seleciona o papel do turno: implementação em `app/escala/utils.py:1531`–`:1539` só consulta existência, unicidade e fechamento do turno. A passagem por `grade_espelho_janela` fornece `ref=fim` e `hoje` explícitos (`app/escala/utils.py:2277`; `app/ponto/services/espelho.py:503`–`:504`), preservando as datas das janelas destes casos.

## 29/10 18:35 · sessao 01bb042eda0b · espera · verificador · origem=Code

**Reprova. Há caminhos que liberam sem produtor vivo e falhas de leitura que acabam em liberação.** O selo também aceita como positivo um arquivo tocado sem agente correspondente. — `bin/hook_stop_fila1.py:488`, `bin/hook_stop_fila1.py:476`, `bin/hook_stop_fila1.py:670`, `bin/tests/test_hook_esperando_com_prova.sh:105`.

Os achados abaixo vêm do arquivo da cópia e do diff fornecido. A identidade do commit e a comparação direta com o pai não puderam ser conferidas por Git nesta cópia. [nao verificado] [nao verificado]

1. **Mtime avançado não prova produtor vivo.** Entrada: `ESPERANDO: agente-x | arquivo=/tmp/agente.out`; memória guarda mtime `100`; alguém toca o arquivo para `200` e encerra antes do Stop. O hook aceita `200 > 100`, sem consultar processo, agente, contador ou conteúdo. Também aceita um avanço antigo ocorrido entre avaliações distantes. **Efeito:** libera com `agente-x` inexistente ou morto. — `bin/hook_stop_fila1.py:488`, `bin/hook_stop_fila1.py:505`, `bin/hook_stop_fila1.py:538`.

2. **O próprio hook pode fabricar a prova por diretório.** Entrada: `ESPERANDO: agente-x | arquivo=logs`, com os arquivos de estado ainda ausentes. A primeira avaliação mede o diretório e depois cria `hook_stop_espera.json`, a trilha e o contador dentro dele; essas criações alteram seu mtime. A próxima avaliação reconhece esse avanço como prova. Não existe verificação de arquivo regular nem exclusão dos diretórios que contêm os arquivos protegidos. **Efeito:** o hook prova a própria atividade e libera sem produtor. — `bin/hook_stop_fila1.py:485`, `bin/hook_stop_fila1.py:488`, `bin/hook_stop_fila1.py:499`, `bin/hook_stop_fila1.py:48`, `bin/hook_stop_fila1.py:716`.

3. **Hardlink contorna a exclusão da trilha.** Entrada: `/tmp/sinal` é hardlink de `logs/hook_stop_fila1.log`; topo declara `arquivo=/tmp/sinal`. `realpath` mantém caminhos distintos para hardlinks, portanto a comparação de nomes não identifica o mesmo inode. Após a primeira observação, o próprio `NOPROVA`/`BLOCK` modifica esse inode; a avaliação seguinte aceita o avanço. **Efeito:** outra prova produzida pelo hook, sem agente externo. — `bin/hook_stop_fila1.py:484`, `bin/hook_stop_fila1.py:485`, `bin/hook_stop_fila1.py:672`, `bin/hook_stop_fila1.py:48`.

4. **Um argumento inerte identifica falsamente o produtor.** Entrada: processo vivo `python3 -c 'import time; time.sleep(300)' /tmp/produtor.sh`; topo `ESPERANDO: produtor.sh | pid=<PID desse Python>`. O arquivo nem precisa existir: qualquer argumento com esse basename basta. Além disso, declarar `/obra-A/produtor.sh` aceita um processo de `/obra-B/produtor.sh`. **Efeito:** um processo alheio libera o Stop como se fosse o produtor esperado. — `bin/hook_stop_fila1.py:469`, `bin/hook_stop_fila1.py:476`.

5. **Falha ao ler ancestrais vira prova aceitável.** `_ancestrais()` adiciona o PID antes da leitura; se `/proc/<hook>/stat` der `PermissionError`, devolve um conjunto parcial contendo o próprio hook. Assim, `if not anc` não detecta a falha. Entrada: cadeia de pais ilegível, mas `/proc/<produtor>/stat` e `cmdline` legíveis e compatíveis. **Efeito:** aceita a prova sem conseguir excluir ancestrais, contrariando “falha de leitura bloqueia”. O limite de profundidade também devolve uma cadeia truncada sem indicar incompletude. — `bin/hook_stop_fila1.py:443`, `bin/hook_stop_fila1.py:444`, `bin/hook_stop_fila1.py:448`, `bin/hook_stop_fila1.py:461`.

6. **Provas declaradas inválidas são silenciosamente descartadas.** Entradas:
   - `ESPERANDO: produtor.sh | pid=<vivo> | arquivo=/tmp/sinal com espaco`
   - `ESPERANDO: agente-x | pid=invalido | arquivo=/tmp/sinal`

   No primeiro caso, só o PID é exigido; no segundo, só o arquivo. Campos desconhecidos ou malformados não geram erro. **Efeito:** libera sem validar as duas provas declaradas, apesar da condição expressa no código. — `bin/hook_stop_fila1.py:432`, `bin/hook_stop_fila1.py:525`, `bin/hook_stop_fila1.py:529`, `bin/hook_stop_fila1.py:535`.

7. **Campos repetidos sobrescrevem provas anteriores.** Entrada: `ESPERANDO: produtor.sh | pid=<morto> | pid=<vivo compatível>`. Só o último PID é consultado; vale o mesmo para `arquivo=<ausente> | arquivo=<que avançou>`. **Efeito:** uma declaração que contém prova inválida libera conforme a ordem dos campos. — `bin/hook_stop_fila1.py:528`, `bin/hook_stop_fila1.py:531`.

8. **A releitura final pode falhar e ainda liberar.** Entrada: prova aceita; antes da chamada em `main`, o painel é removido ou perde permissão. `_linha_do_topo()` retorna `None`, mas esse resultado vira apenas o argumento de auditoria de `_libera()`. Se a releitura lançar erro de decodificação, a exceção externa também libera por “hook falhou aberto”. Se o topo mudar para uma linha sem ESPERANDO, ela igualmente não é revalidada. **Efeito:** falha de leitura ou retirada da declaração não impede a liberação. — `bin/hook_stop_fila1.py:414`, `bin/hook_stop_fila1.py:670`, `bin/hook_stop_fila1.py:733`.

9. **A prova por PID não permanece vinculada à liberação.** Corrida concreta: lê `stat` e `cmdline` de N vivo; N encerra; o hook relê o painel e libera. A identidade também não é vinculada ao início do processo, permitindo misturar observações se houver reutilização do PID entre as leituras. **Efeito:** pode registrar “com prova” quando o produtor já morreu. — `bin/hook_stop_fila1.py:466`, `bin/hook_stop_fila1.py:468`, `bin/hook_stop_fila1.py:478`, `bin/hook_stop_fila1.py:670`.

10. **A memória pode regredir por concorrência e fabricar outro avanço.** Intercalação:
    - B mede mtime `95` e pausa.
    - O arquivo avança para `100`; A mede e grava `100`.
    - B retoma, lê `100`, sobrescreve a memória com `95` e bloqueia.
    - C vê o arquivo ainda em `100` e libera, sem nova escrita no sinal.

    A leitura, comparação e gravação não têm exclusão nem atualização monotônica. Escritas concorrentes também podem perder observações de outros arquivos. **Efeito:** falsa prova de movimento ou bloqueios por perda de histórico. — `bin/hook_stop_fila1.py:488`, `bin/hook_stop_fila1.py:495`, `bin/hook_stop_fila1.py:497`, `bin/hook_stop_fila1.py:499`.

11. **Memória com `NaN` aceita arquivo parado.** Entrada em `hook_stop_espera.json`: `{"/tmp/sinal": NaN}`; sinal com mtime normal e sem alteração. O carregamento aceita o valor; ele passa por `isinstance(..., float)` e `mt <= anterior` resulta falso. **Efeito:** memória semanticamente inválida produz liberação, em vez de bloquear. — `bin/hook_stop_fila1.py:495`, `bin/hook_stop_fila1.py:505`.

12. **A memória pode impedir chegar à fila vazia ou ao TETO.** Entrada: `logs/hook_stop_espera.json` é uma FIFO sem escritor; topo declara `ESPERANDO: agente-x | arquivo=/tmp/sinal`, sendo o sinal existente. A abertura/leitura da memória espera indefinidamente, sem verificar tipo nem impor limite. **Efeito:** o hook não alcança a liberação por fila vazia nem a válvula do TETO, caminhos que antes não dependiam desse arquivo novo. — `bin/hook_stop_fila1.py:494`, `bin/hook_stop_fila1.py:495`, `bin/hook_stop_fila1.py:683`, `bin/hook_stop_fila1.py:721`.

13. **Há falsos bloqueios para nomes válidos e produtores reais.** Entrada: script vivo `/tmp/agente atual.sh`, declarado pelo mesmo caminho; o nome vira apenas `/tmp/agente`. Um sinal `/tmp/sinal atual.out` também não pode ser representado pelo regex. Separadamente, um agente `claude` filho ou um `git push` real é rejeitado pela lista de nomes genéricos, mesmo com PID vivo. **Efeito:** provas legítimas ficam sem representação ou são rejeitadas por restrições adicionais ao aval. — `bin/hook_stop_fila1.py:521`, `bin/hook_stop_fila1.py:529`, `bin/hook_stop_fila1.py:436`, `bin/hook_stop_fila1.py:458`.

14. **A comparação perde movimentos menores que a precisão do float.** Entrada: num filesystem com nanossegundos, mtime muda de `1793296800000000100 ns` para `1793296800000000101 ns`; ambos têm a mesma representação em segundos como float. **Efeito:** o avanço existe, mas `st_mtime` o apaga e o hook responde “arquivo nao andou”. — `bin/hook_stop_fila1.py:488`, `bin/hook_stop_fila1.py:505`.

15. **Relógio lido dentro da prova e margem arbitrária.** Entrada: memória anterior menor e mtime `agora + 30 segundos`; o arquivo futuro é aceito. Um ajuste do relógio para trás pode, inversamente, rejeitar um produtor legítimo. A decisão chama `time.time()` dentro do auxiliar e fixa margem de `60`, sem medição apresentada no diff. **Efeito:** a prova depende do relógio ambiente e tolera timestamps futuros. — `bin/hook_stop_fila1.py:503`; regra de medição: `CLAUDE.md:35`.

16. **O positivo de arquivo do selo certifica o contraexemplo proibido.** O selo cria arquivo vazio, muda sua data com `touch` e exige liberação para `agente-x`; nenhum `agente-x` é iniciado nem existe contador sendo produzido por ele. **Efeito:** uma implementação que libera sem produtor correspondente passa no caso positivo. Falta o par “arquivo avançou, produtor morreu/ausente → bloqueia”. — `bin/tests/test_hook_esperando_com_prova.sh:102`, `bin/tests/test_hook_esperando_com_prova.sh:105`, `bin/tests/test_hook_esperando_com_prova.sh:106`.

17. **O verificador positivo aceita motivo de falha aberta.** Contraexemplo de mutação: no ramo positivo, liberar com motivo `hook falhou aberto: ESPERANDO`. A saída continua vazia e `ALLOW.*ESPERANDO` casa, embora a liberação não tenha ocorrido pelo motivo exigido. **Efeito:** o selo não distingue o sucesso correto de uma falha aberta contendo essa palavra. — `bin/tests/test_hook_esperando_com_prova.sh:66`, `bin/tests/test_hook_esperando_com_prova.sh:69`, `bin/hook_stop_fila1.py:734`.

18. **O cleanup mantém PID já encerrado e recolhido.** Entrada: depois do `kill`/`wait` do produtor, seu PID é reutilizado antes do `trap EXIT`. Ele continua em `_pids`, e `_fim()` envia novo `kill` ao número. **Efeito:** o selo pode sinalizar processo alheio. — `bin/tests/test_hook_esperando_com_prova.sh:28`, `bin/tests/test_hook_esperando_com_prova.sh:52`, `bin/tests/test_hook_esperando_com_prova.sh:117`.

Quanto às classes solicitadas:

- **Segundo juiz duplicando juiz registrado: nada.** As novas perguntas não demonstram duplicação do juiz de fila; PAREI e ESPERANDO compartilham o leitor do topo. — `bin/hook_stop_fila1.py:400`, `bin/hook_stop_fila1.py:517`, `bin/hook_stop_fila1.py:572`.
- **Segundo escritor fora da porta: nada identificado.** A escrita nova está na memória própria de espera; os problemas encontrados nela são concorrência e identificação de arquivos. — `bin/hook_stop_fila1.py:434`, `bin/hook_stop_fila1.py:499`.
- **Relógio fora da borda:** achado 15. — `bin/hook_stop_fila1.py:503`.
- **Derivação paralela de fonte única: nada identificado.** O topo continua sendo interpretado a partir do auxiliar compartilhado. — `bin/hook_stop_fila1.py:517`, `bin/hook_stop_fila1.py:572`.
- **Selo sem mordida:** achados 16 e 17. — `bin/tests/test_hook_esperando_com_prova.sh:106`, `bin/tests/test_hook_esperando_com_prova.sh:69`.
- **Ordem de trava:** a checagem inicial de PROVA permanece antes de PAREI e ESPERANDO; o defeito está na liberação após releitura inválida e na espera que pode impedir alcançar o TETO. — `bin/hook_stop_fila1.py:657`, `bin/hook_stop_fila1.py:670`, `bin/hook_stop_fila1.py:494`.
- **Pouso antes do pré-requisito: nada demonstrável no diff.** O pouso efetivo não foi verificado. [nao verificado] [nao verificado]

**PAREI: nada de regressão identificado na extração. TETO e fila vazia: decisões preservadas quando o fluxo chega até elas**, com a ressalva de travamento do achado 12. **PROVA do RELATO: nada de desvio sequencial da checagem inicial**; a corrida de releitura permanece no achado 8. — `bin/hook_stop_fila1.py:572`, `bin/hook_stop_fila1.py:657`, `bin/hook_stop_fila1.py:683`, `bin/hook_stop_fila1.py:721`.

## 29/10 18:33 · sessao 53bc2144c427 · grupo1p1 · verificador · origem=Code

- **Classe 3 — cartão/tela:** `app/core/tests/test_espelho_tela_le_a_folha.py:69` e `:70` leem `timezone.now()` separadamente. Os casos chamam esse helper em `:81` e `:126`; o segundo ainda faz outra leitura em `:133`.

- **Classe 3 — edição da ausência:** `app/escala/tests/test_cobertura_parcial_periodo.py:175` e `:178` consultam o relógio novamente antes e depois da edição, dentro do mesmo caso.

- **Classe 3 — app/espelho:** `app/api/tests/test_o21_app_diz_o_que_foi_decidido.py:167` lê `localdate()` sem instante; `:169` faz outra leitura com `now()`.

- **Classe 3 — piso:** `app/ponto/tests/test_piso_nao_sobe_por_batida.py:122` lê `localdate()` sem instante; `:123` consulta `now()` novamente.

- **Classe 3 — comparações dos produtores R3:** `app/relatorios/tests/test_r3_em_aberto_alcanca_turno_aberto.py:251` combina helpers que leem o relógio em `:187` e `:191`. Os casos em `:377`–`:378` e `:390`–`:391` também combinam a leitura de `:187` com outro `now()`.

- **Classe 3 — premissa e julgamento R3:** `app/relatorios/tests/test_r3_em_aberto_alcanca_turno_aberto.py:428` e `:443` chamam `turno_aberto_de` sem instante, acionando `app/ponto/turnos.py:2255`; depois, `:431` e `:445` chamam o helper que relê o relógio em `:421`.

- **Classe 3 — ficha/espelho:** `app/colaboradores/tests/test_ficha_fonte_dos_totais.py:111` lê `now()`; `:112` chama `_horas()` (`:48`–`:50`), cuja execução lê novamente em `app/colaboradores/services/ficha.py:117`.

- **Classe 3 — motor/card:** `app/api/tests/test_espelho_app_fonte_e_geometria.py:317` chama o helper que lê `now()` em `:294`; `:324` consulta o payload pelos helpers de `:281`–`:282` e `:73`–`:77`, acionando outra leitura em `app/api/views.py:1350`.

- **Classe 8 — geometria:** `app/api/tests/test_espelho_app_fonte_e_geometria.py:294` substitui o instante fixo posterior às fixtures por relógio real; a classe de `:278` e sua base de `:47` não congelam o tempo. A garantia retirada está em `app/escala/tests/instante.py:6`–`:12`.

- **Classe 8 — realizado:** `app/ponto/tests/test_bordas_realizado.py:61` passa relógio real para a fixture fixa de `:60`, sem congelamento na classe de `:15`; deixa de fixar o julgamento posterior garantido por `app/escala/tests/instante.py:6`–`:12`.

- **Classe 8 — cobertura parcial:** `app/escala/tests/test_cobertura_parcial_periodo.py:111`, `:175`, `:178` e `:323` passam relógio real, sem congelamento nas classes de `:85`, `:147` e `:294`; retiram o instante posterior fixo definido em `app/escala/tests/instante.py:6`–`:12`.

- **Classe 8 — R3:** `app/relatorios/tests/test_r3_em_aberto_alcanca_turno_aberto.py:187` e `:362` passam relógio real na classe sem congelamento de `:76`, embora o cenário declare `HOJE` em `:54`; retiram o instante posterior fixo definido em `app/escala/tests/instante.py:6`–`:12`.

Total por classe, contando os achados agrupados acima: **3 = 8; 5 = 0; 7 = 0; 8 = 4.**

## 29/10 18:31 · sessao 1a66099671f2 · - · - · origem=Ronald (tmux)

Classe 3, leituras verificáveis:

- `app/core/tests/test_espelho_tela_le_a_folha.py:69` e `:70`: `_dois_resumos()` lê `timezone.now()` duas vezes; chamado pelos testes em `:81` e `:126`. O segundo ainda lê novamente em `:133` — três leituras nesse caso.
- `app/escala/tests/test_cobertura_parcial_periodo.py:175` e `:178`: duas leituras no mesmo teste de edição.
- `app/api/tests/test_o21_app_diz_o_que_foi_decidido.py:167` e `:169`: `_tz.localdate()` seguido de `timezone.now()`.
- `app/ponto/tests/test_piso_nao_sobe_por_batida.py:122` e `:123`: `timezone.localdate()` seguido de `timezone.now()`.
- `app/relatorios/tests/test_r3_em_aberto_alcanca_turno_aberto.py:251`: chama `_tela()` e `_cartao()`, que leem separadamente em `:187` e `:191`.
- `app/relatorios/tests/test_r3_em_aberto_alcanca_turno_aberto.py:377` e `:378`: `_tela()` lê em `:187`; o cartão lê novamente em `:378`.
- `app/relatorios/tests/test_r3_em_aberto_alcanca_turno_aberto.py:390` e `:391`: mesmo problema no teste seguinte.
- `app/colaboradores/tests/test_ficha_fonte_dos_totais.py:111` e `:112`: leitura direta seguida de `_horas()` (`:48–50`), que chama `ficha_do_colab()`, cujo relógio é lido em `app/colaboradores/services/ficha.py:117`.
- `app/api/tests/test_espelho_app_fonte_e_geometria.py:317` e `:324`: primeiro chama `_o_que_o_motor_diria()`, cujo relógio é lido em `:294`; depois `_dia()` (`:281–282`) chama `_payload()` (`:73–77`), executando a nova leitura em `app/api/views.py:1350`.

São nove grupos acima, correspondendo a dez definições de testes, pois o helper do primeiro grupo é usado por dois casos. A subclasse R3 herda os testes da classe base (`app/relatorios/tests/test_r3_em_aberto_alcanca_turno_aberto.py:401`).

Não encontrei chamada efetivamente sem migração entre os alvos do selo: a única chamada sem `agora=` explícito encontrada por AST passa `**kw` em `app/escala/tests/test_sobra_na_borda_nao_vira_intervalo.py:53`, após inserir `agora` em `:52`; portanto não constitui classe 7.

A mudança `return sorted(out)` conserva mordida no próprio teste: `app/escala/tests/test_selo_grade_pede_o_instante.py:178–190` espera chamadas ordenadas, incluindo uma chamada no nível do módulo (`:186–187`) e outras dentro da função (`:182–185`). Não identifiquei defeito classe 5 concreto nessa alteração.

## 29/10 18:31 · sessao f7413f11c7e3 · - · - · origem=Ronald (tmux)

Classe 8, considerando a retirada do instante fixo posterior às fixtures como mudança do cenário de entrada:

- `app/ponto/tests/test_bordas_realizado.py:61`: consulta agora depende do relógio real; fixture fixa em `:60`, classe sem congelamento em `:15`.
- `app/escala/tests/test_cobertura_parcial_periodo.py:111`, `:175`, `:178`: relógio real substitui o instante posterior fixo; cenário declara `D=14/10/2026` e `HOJE=27/10/2026` em `:25–26`, classes sem congelamento em `:85` e `:147`.
- `app/escala/tests/test_cobertura_parcial_periodo.py:323`: mesmo problema para a fixture noturna de outubro, definida em `:311`; classe sem congelamento em `:294`.
- `app/relatorios/tests/test_r3_em_aberto_alcanca_turno_aberto.py:187`, `:362`: relógio real substitui o instante fixo, embora o cenário declare `HOJE=31/07/2026` em `:54`; classe base sem congelamento em `:76`.

A garantia removida é expressa em `app/escala/tests/instante.py:6–7`; a constante vale janeiro de 2100 em `:12`. Não há prova de mudança atual das asserções apenas por essas substituições: o seletor ainda não lê `agora` (`app/escala/utils.py:1528–1539`).

Nenhum achado adicional comprovado de classe 5 nos três arquivos priorizados.

## 29/10 18:30 · sessao 90893cc08fbf · - · - · origem=Ronald (tmux)

Possível achado de **classe 8**: `app/api/tests/test_espelho_app_fonte_e_geometria.py:294` troca o instante fixo de 2100 (`app/escala/tests/instante.py:12`) por relógio real. `TurnoAbertoPelaGeometriaTest` não tem `freeze_time` (`app/api/tests/test_espelho_app_fonte_e_geometria.py:278`), nem sua base (`app/api/tests/test_espelho_app_fonte_e_geometria.py:47`); portanto perde o cenário temporal fixo posterior às fixtures (`app/api/tests/test_espelho_app_fonte_e_geometria.py:314`).

Não provei perda de mordida atual nesse caso: o seletor da grade ainda não usa `agora` (`app/escala/utils.py:1528`). Nos demais arquivos atribuídos, não identifiquei achados verificáveis de classes 5/8.

## 29/10 18:22 · sessao 0e25a18be1e0 · justif · sonda · origem=Code

Números conforme o dado raspado; confirmação independente em produção: [nao verificado].

- **Classe:** 98 justificativas aprovadas com origem viva. **Alvo em competência aberta:** 45, esperado 45; fora do aval: 0. [nao verificado]
- **Exportadas / Pauta DP:** empresa 2 × 09/2026: **46**; empresa 3 × 09/2026: **6**; empresa 4 × 09/2026: **1**. Total: **53**; as duas apresentações da pauta coincidem. [nao verificado]
- **Guarda `irma_orfa`: 5 dos 45 itens**, com **7 perguntas pendentes**, distribuídas assim: [nao verificado]
  - 2449 → 34824. [nao verificado]
  - 2748 → 39729. [nao verificado]
  - 3201 → 48775, 48776, 48777. [nao verificado]
  - 3273 → 50091. [nao verificado]
  - 3301 → 50392. [nao verificado]

No código, essa guarda consulta perguntas respondidas, não validadas, não materializadas e com resposta não vazia; havendo pendências, impede a retratação. [app/chamados/juizes.py:1471](/home/ronald/codex_ws/cbedeb91b8bf7eeb460546b8cb69b408a01d3bb3/app/chamados/juizes.py:1471), [app/chamados/reconciliador.py:435](/home/ronald/codex_ws/cbedeb91b8bf7eeb460546b8cb69b408a01d3bb3/app/chamados/reconciliador.py:435), [app/chamados/reconciliador.py:454](/home/ronald/codex_ws/cbedeb91b8bf7eeb460546b8cb69b408a01d3bb3/app/chamados/reconciliador.py:454).

As conferências internas fecham: **45 + 53 = 98**; a lista de perguntas-destino contém **45 itens**, somando **10 respondidas e 3 validadas**, diferença **7**, igual à quantidade de perguntas retidas. O dry informa **45 `True`**; a prova de origem viva informa **45**; os vereditos somam **44 + 1 = 45**, todos com antes igual ao depois. **Nenhuma incoerência aritmética aparente.** [nao verificado]

Também constam: M1 já encerrado **92**, em **2026-10-26**; exportadas vigentes intactas **`True (7)`**; **`ERROS=0`** e presença de **`FIM-DA-SONDA`**. O universo do `(7)` não está explicitado, portanto não cabe equipará-lo aos 53 itens exportados. [nao verificado]

A retenção por **`justificativa_alheia` não foi quantificada no anexo**: há somente referência à medição em sombra. Seu resultado permanece [nao verificado].

## 29/10 18:20 · sessao 311e93877dc2 · justif · verificador · origem=Code

**Não aprovaria esta versão:** a retenção pode ser contornada, há ciclos de trava e a materialização pode gerar duas transições para o mesmo ato. (`app/ponto/services/cartorio.py:1163`; `app/ponto/services/justificativa.py:69`; `app/ponto/services/justificativa.py:379`)

As conclusões são por leitura da cópia e análise AST em memória. Reprodução funcional, concorrência em banco e REDs citados nos comentários: **[nao verificado]**.

**1. Defeitos e contraexemplos**

- **Retenção não alcança todo fechamento do ato — outra: guarda contornada.** A regra nova é consultada por `_retratar` e pela cascata, mas o cartório chama `reconciliar()` diretamente; o gate do modelo não consulta essa retenção. (`app/chamados/reconciliador.py:474`; `app/ponto/services/justificativa.py:377`; `app/ponto/services/cartorio.py:1163`; `app/chamados/models.py:474`)

  **Entrada → erro:** C vivo aponta J2 pendente, tem célula/escala e cobra o marco S19; aprovar J1 registra S19. O signal da batida chama o cartório, a lâmpada acesa permite `reconciliar()` e C termina resolvido, embora a regra nova determine preservar C durante esse ato. J2 pode continuar pendente: este desvio não passa pela auto-rejeição de `_retratar`. O save da própria justificativa também aciona o cartório. (`app/ponto/signals.py:135`; `app/ponto/services/cartorio.py:1163`; `app/chamados/models.py:959`; `app/chamados/reconciliador.py:445`; `app/chamados/reconciliador.py:514`; `app/ponto/signals.py:165`)

- **Conjunto antecipado de travas é incompleto — ordem de trava.** `travar_origem_antes` cobre VIVOS + origem; `_encerrar_bloqueio_de_entrada` busca NAO_FECHADOS, que inclui `resolvido`. Portanto, pode adquirir depois uma trava ausente do conjunto antecipado. (`app/ponto/services/justificativa.py:101`; `app/ponto/services/justificativa.py:69`; `app/chamados/catalogo/motor.py:32`)

  **Entrada → erro:** mesmo colaborador sem chamados vivos; C1 e C2 são bloqueios de entrada resolvidos; J1/J2 pendentes apontam respectivamente C1/C2 e não têm saída informada. T1 trava C1/J1; T2 trava C2/J2. O helper da origem retorna sem fechar; o helper do bloqueio exclui a própria origem, fazendo T1 esperar C2 e T2 esperar C1. Há deadlock entre as próprias aprovações, sem nascimento concorrente de chamado. (`app/ponto/services/justificativa.py:106`; `app/ponto/services/justificativa.py:119`; `app/ponto/services/justificativa.py:39`; `app/ponto/services/justificativa.py:72`; `app/ponto/services/justificativa.py:195`)

- **A nova ordem cruza com a regularização — ordem de trava.** A aprovação faz C → J; a regularização atualiza J dentro de `atomic` e depois analisa C, fazendo J → C. (`app/ponto/services/justificativa.py:106`; `app/ponto/services/justificativa.py:119`; `app/ponto/services/regularizacao.py:89`; `app/ponto/services/regularizacao.py:106`; `app/ponto/services/regularizacao.py:137`)

  **Entrada → erro:** C aberto, J pendente com entrada e sem saída informada. A regularização grava J; a aprovação trava C e espera J; a regularização tenta analisar C e espera C. Uma operação precisa abortar. O caminho externo é evidência da interação; o ponto introduzido nesta obra é a nova ordem da aprovação. (`app/ponto/services/regularizacao.py:102`; `app/chamados/models.py:572`; `app/ponto/services/justificativa.py:118`)

- **Materialização pode fechar duas vezes — outra: duas transições para um fato.** A cascata continua incluindo `resolvido`, e `encerrar()` permite `resolvido → fechado`. É comportamento preexistente conservado no trecho tocado. (`app/ponto/services/justificativa.py:371`; `app/ponto/services/justificativa.py:379`; `app/chamados/catalogo/motor.py:32`; `app/chamados/catalogo/motor.py:114`)

  **Entrada → erro:** origem C de `turno_aberto`, ligada à própria J, entrada 07h e saída materializada 19h, sem retenção. A batida dispara o reconciliador e resolve C; depois a cascata transforma C em fechado. Cada passagem grava `estado_chamado`: duas linhas no mesmo ato. O helper posterior da origem ser no-op não elimina a segunda transição anterior. (`app/ponto/services/justificativa.py:338`; `app/ponto/signals.py:215`; `app/chamados/reconciliador.py:1266`; `app/chamados/models.py:444`; `app/ponto/services/justificativa.py:379`; `app/chamados/models.py:1201`; `app/chamados/models.py:227`)

- **“Batida não nula” não garante saída válida — pouso antes do pré-requisito.** Os guards novos recusam `None`, mas `registrar_batida` também pode devolver uma batida de tipo oposto ou retratada quando encontra a mesma chave colaborador/instante. A aceitação já existia; permanece nos trechos tocados e agora pode acompanhar o fechamento novo da origem. (`app/ponto/services/justificativa.py:172`; `app/ponto/services/justificativa.py:347`; `app/ponto/views.py:1822`; `app/ponto/registro_batida.py:120`)

  **Entrada → erro:** J tem entrada às 07h e solicita saída às 19h; já existe uma **E** às 19h, sem vínculo com outra justificativa. A porta devolve essa E; os guards passam; J fica aprovada com `batida_gerada` apontando uma entrada, e a aprovação pode resolver sua origem pela premissa da própria justificativa aprovada. (`app/ponto/registro_batida.py:122`; `app/ponto/services/justificativa.py:174`; `app/ponto/services/justificativa.py:181`; `app/ponto/services/justificativa.py:194`; `app/chamados/models.py:937`)

**2. Escritores não alcançados e segundo fechador**

Há escritor de aprovação não alcançado: **Django admin — FORA-DO-ESCOPO, preexistente**. `JustificativaAdmin` é editável, não delega às portas da aprovação e o campo `status` não é somente leitura. O selo novo declara excluir esse caminho. (`app/ponto/admin.py:18`; `app/ponto/models.py:159`; `app/ponto/tests/test_selo_aprovacao_encerra_origem.py:13`)

As três atribuições explícitas de produção encontradas estão costuradas: aprovação individual, materialização e lote. (`app/ponto/services/justificativa.py:182`; `app/ponto/services/justificativa.py:194`; `app/ponto/services/justificativa.py:360`; `app/ponto/services/justificativa.py:387`; `app/ponto/views.py:1796`; `app/ponto/views.py:1837`)

O censo AST da cópia não encontrou escrita de `status_local` fora das portas reconhecidas; isso conserva os limites de resolução declarados pelo próprio selo, não constitui garantia contra qualquer escrita dinâmica. (`app/chamados/tests/test_selo_status_so_pelas_portas.py:317`; `app/chamados/tests/test_selo_status_so_pelas_portas.py:337`; `app/chamados/tests/test_selo_status_so_pelas_portas.py:17`)

Na aprovação individual, não sobrou o antigo fecho forçado externo: origem e bloqueio delegam a `retratar`, e o seletor do bloqueio exclui a origem. O fechamento forçado permanece na cascata da materialização, responsável pelo caso de dupla transição acima. (`app/ponto/services/justificativa.py:42`; `app/ponto/services/justificativa.py:72`; `app/ponto/services/justificativa.py:75`; `app/ponto/services/justificativa.py:379`)

**3. Mordida dos testes** [nao verificado]

O selo estrutural **morde a retirada das três costuras**: removendo as chamadas em memória, acusa os três escritores. Entretanto, seu critério verifica ancestral `atomic`, receptor e posição textual; aceita chamada sob `if False`, falha engolida por `except` e fechamento colocado antes da saída. A limitação do ramo morto está declarada. (`app/ponto/tests/test_selo_aprovacao_encerra_origem.py:94`; `app/ponto/tests/test_selo_aprovacao_encerra_origem.py:102`; `app/ponto/tests/test_selo_aprovacao_encerra_origem.py:8`)

Há estes pontos de **selo/RED sem mordida suficiente**:

- `test_J2_nao_e_rejeitada_pela_BATIDA_do_proprio_ato` exige uma batida vinculada, mas não exige que o receiver/reconciliador tenha executado. Sem esse receiver, a costura explícita ainda retém C, permitindo sucesso por ausência do caminho anunciado. (`app/ponto/tests/test_justificativa_encerra_origem.py:413`; `app/ponto/tests/test_justificativa_encerra_origem.py:422`; `app/ponto/services/justificativa.py:194`)
- Esse teste e o da cascata usam fixture sem célula/escala; o cartório retorna antes de julgar. Portanto, não cobrem o desvio do primeiro defeito. (`app/ponto/tests/test_justificativa_encerra_origem.py:392`; `app/ponto/tests/test_justificativa_encerra_origem.py:426`; `app/ponto/services/cartorio.py:1547`)
- O teste positivo da materialização escolhe origem `justificativa_colab` e não conta trilhas; não acusa a dupla transição da origem de turno. (`app/ponto/tests/test_justificativa_encerra_origem.py:203`; `app/ponto/tests/test_justificativa_encerra_origem.py:213`; `app/ponto/tests/test_justificativa_encerra_origem.py:229`)
- `test_erro_de_banco_repetido_nas_perguntas_derruba_a_aprovacao_INTEIRA` pode passar sem o savepoint S143, porque a segunda chamada ao resolver já propaga a falha. O teste específico do savepoint é `test_erro_de_banco_engolido_no_receiver_S143_nao_mente_o_fechamento`. (`app/ponto/tests/test_justificativa_encerra_origem.py:256`; `app/chamados/reconciliador.py:521`; `app/ponto/tests/test_justificativa_encerra_origem.py:231`)
- Os tetos aceitam contagens menores; retirar o savepoint reduz queries e continua passando. Isso é adequado para um teto, mas não certifica a existência da cura. (`app/ponto/tests/test_selo_chokepoint_escrita.py:213`)

Há sentinelas úteis contra ausência de sinal: chamada obrigatória da porta no teste de rollback, chamada obrigatória de `registrar_batida` e trilha obrigatória da recusa. Execução desses funcionais contra árvores sem cada cura: **[nao verificado]**. (`app/ponto/tests/test_justificativa_encerra_origem.py:170`; `app/ponto/tests/test_justificativa_encerra_origem.py:197`; `app/ponto/tests/test_justificativa_encerra_origem.py:159`)

**4. Ordem de trava e duração do contexto**

O caminho comum deixou de começar pela trava de J: as portas antecipam chamados por PK antes de travar a justificativa. Isso acompanha a ordem efetiva de `_retratar`, que escreve C e depois pode atualizar J pendente. Não elimina os dois ciclos concretos descritos acima. (`app/ponto/services/justificativa.py:106`; `app/ponto/services/justificativa.py:119`; `app/chamados/reconciliador.py:508`; `app/chamados/reconciliador.py:520`)

O `ContextVar` tem limpeza por `finally`, mas `with atomic(), no_ato_de_aprovar(...)` remove a marca **antes** da saída do `atomic` e dos callbacks de commit. Assim, cobre a cadeia síncrona que efetivamente consulta a regra, porém não permanece nos callbacks registrados por perguntas/retomada. Dano adicional concreto desses callbacks ou de troca de thread: **[nao verificado]**. (`app/chamados/reconciliador.py:430`; `app/ponto/services/justificativa.py:117`; `app/ponto/services/justificativa.py:319`; `app/ponto/views.py:1792`; `app/chamados/signals.py:121`; `app/chamados/signals.py:202`)

**5. Efeitos colaterais dentro da aprovação**

**Sim, uma falha persistente na resolução das perguntas pode desfazer a aprovação inteira.** S143 agora captura a falha dentro de savepoint, mas `_retratar` chama o resolver novamente sem captura. Essa segunda falha atravessa o `atomic` da aprovação: rollback integral, compatível com a proibição de meio-estado. (`app/chamados/signals.py:88`; `app/chamados/reconciliador.py:507`; `app/chamados/reconciliador.py:521`; `app/ponto/services/justificativa.py:196`)

O receiver de rebaixamento não captura exceções; uma exceção propagaria. No caminho normal, porém, o núcleo já rebaixou a urgência antes do save, e o receiver retorna sem escrever. (`app/chamados/signals.py:72`; `app/chamados/models.py:210`; `app/chamados/models.py:278`; `app/chamados/models.py:424`)

O cartório é protegido por `engolir`, que captura exceções sem criar savepoint. Não confirmei um novo caso adicional de sucesso silencioso com rollback nesse percurso; isolamento completo de toda falha de banco: **[nao verificado]**. O erro concreto demonstrável é ele fechar C sem consultar a retenção, conforme o primeiro achado. (`app/chamados/signals.py:172`; `app/core/observ.py:85`; `app/ponto/services/cartorio.py:1163`)

Em `invalidar_fechamento`, leitura e updates ficaram em savepoints próprios, com captura externa; não identifiquei defeito novo seguro nessa alteração. Gravação incorreta causada apenas por ler os próprios dados ainda não commitados, além do desvio de retenção descrito: **[nao verificado]**. (`app/ponto/services/fechamento.py:1616`; `app/ponto/services/fechamento.py:1619`; `app/ponto/services/fechamento.py:1629`)

## 29/10 18:19 · sessao f208ba29631a · justif · leitura · origem=Code

**O chamado `entrada_bloqueada_turno_aberto` VIVO, por si só, não impede a batida.** No web, ele é reutilizado para atualizar o contexto e sinalizar revisão; o fluxo continua até o registro. Na API, a anomalia é coletada, a batida passa pelo chokepoint e depois pode alimentar uma disputa. **Isso não significa criação incondicional:** existem recusas, confirmação e retornos de batida existente. (`app/ponto/views.py:274–314`, `345–353`; `app/api/views_core.py:432–458`, `679–718`, `769–781`.)

`VIVOS` inclui `aberto`, `em_analise` e `registrado`. (`app/chamados/catalogo/motor.py:24`.)

**Caminho do app/PWA pela API**

| Passo | Código e resultado |
|---|---|
| Entrada HTTP | O proxy declarado encaminha `/api/ponto/*` ao core; `/api/ponto/bater/` chega a `views_core.api_bater_ponto`. (`Caddyfile:8–9`; `app/config/urls_core.py:6`; `app/api/urls_core.py:5`, `14`.) |
| Autenticação/colaborador | Endpoint POST autenticado; colaborador inexistente retorna **404**, antes do registro. (`app/api/views_core.py:385–391`.) |
| Situação cadastral | Situação fora de `ativo`/`afastado` retorna **403**, incluindo os ramos `desligado` e `ferias`. (`app/api/views_core.py:402–410`.) |
| Tipo | Tipo diferente de `E`/`S` retorna **400**. (`app/api/views_core.py:419–426`.) |
| Turno órfão | O detector consulta entradas sem saída; havendo bloqueio e nenhuma regularização pendente, acrescenta `orfao_14h`. Esse teste consulta a geometria/justificativas, não a existência do chamado desse módulo. (`app/api/views_core.py:432–433`; `app/chamados/detector_anomalias.py:29–53`; `app/ponto/utils_turno_orfao.py:28–58`.) |
| GPS | Latitude ou longitude ausente retorna **403 `gps_obrigatorio`**, sem chegar ao chokepoint. (`app/api/views_core.py:440–458`.) |
| Foto/assinatura | Foto ausente e assinatura inválida acrescentam anomalias; esses blocos continuam o processamento. (`app/api/views_core.py:459–500`.) |
| Instante | `hora_do_aparelho` fornece `ts_efetivo` e o timestamp recebido do aparelho. (`app/api/views_core.py:528–531`.) |
| Deduplicação | Mesmo tipo em **±5 minutos** retorna **`ja_registrada`**, com ID existente e `sucesso=True`; não cria outra batida. (`app/api/views_core.py:549–568`.) |
| Escolha E/S | Ao vivo, `proximo_tipo_de` pode corrigir o tipo enviado; no retroativo, os marcos podem corrigi-lo. (`app/api/views_core.py:592–623`.) |
| Entrada/saída anômala | Saída sem entrada ou nova entrada sobre turno aberto acrescentam motivo/contexto; esses blocos não retornam recusa. (`app/api/views_core.py:632–672`.) |
| Escrita | Chama `registrar_batida(..., origem='app', autoridade_humana=False, canal=...)`. (`app/api/views_core.py:679–694`.) |
| Chokepoint recusou | `batida is None` retorna **`ignorada_intervalo`**, com `sucesso=True` e referência à oposta existente; não cria nova batida. (`app/api/views_core.py:695–718`.) |
| Depois do registro | Havendo anomalias, abre/atualiza disputa e marca `em_disputa=True`; a resposta normal contém `status='ok'` e ID da batida. Portanto, **disputa não substitui a criação já realizada**. (`app/api/views_core.py:769–781`, `823–834`.) |

A outra casca também expõe `/api/ponto/bater/`, por `api.views.api_bater_ponto`, com os mesmos ramos relevantes: situação/tipo, GPS, deduplicação, chokepoint e disputa. (`app/config/urls.py:37`; `app/api/urls.py:67`; `app/api/views.py:398–466`, `557–576`, `683–721`, `769–781`.)

**Caminho do formulário web/PWA**

1. `/ponto/bater/` e `/ponto/bater-v2/` chegam a `bater_ponto`; onboarding incompleto redireciona antes do processamento. A triagem reconstrói entradas sem saída e calcula `tem_bloqueio_turno` por **14 ≤ horas < 96**. (`app/ponto/urls.py:11–12`; `app/ponto/views.py:240–259`; `app/ponto/services/triagem_batida.py:17–30`, `175–206`.)

2. Regularização pendente significa justificativa `pendente`, vinculada à entrada, com `saida_informada`; **não significa simplesmente chamado aberto**. (`app/ponto/services/triagem_batida.py:208–223`.)

3. No POST `E`, com bloqueio e sem regularização pendente, procura o chamado desse módulo em `VIVOS`. Encontrando, renova seu contexto; não encontrando, cria. Depois define `_sinal_acesso` e continua. **Não há `return` de recusa nesse ramo.** (`app/ponto/views.py:274–314`.)

4. Existe um desvio sem gravar: o `elif` de entrada com `turno_aberto` e sem `confirmar` salva GPS/foto na sessão e **retorna a tela de confirmação**. A confirmação reenvia `E` com `confirmar=sim`. (`app/ponto/views.py:315–329`; `app/templates/ponto/triagem/turno_confirmar.html:8–17`.)

5. Para `E`/`S`, chama `ramo_registrar_es`. Mesmo tipo nos cinco minutos anteriores retorna `ja_registrada`, sem criação. Fora disso, o sinal de acesso torna a batida **`aprovada=False`**, com justificativa de revisão, e chama `registrar_batida`. (`app/ponto/views.py:345–353`; `app/ponto/services/triagem_batida.py:669–720`.)

6. Se o chokepoint devolver `None`, retorna `ignorada_intervalo`; se devolver batida, prepara o contexto de sucesso. (`app/ponto/services/triagem_batida.py:721–753`.)

A UI v2 envia pelo endpoint da API e tem barreiras próprias: sem selfie ou GPS, aborta o envio. Também mostra sucesso para qualquer HTTP OK, sem interpretar o corpo — incluindo respostas `ja_registrada`/`ignorada_intervalo`. (`app/templates/ponto/bater_ponto_v2.html:285–300`, `325–340`.)

No PWA offline, guardar na IndexedDB é registro **local**; a criação no servidor depende da sincronização posterior pelo mesmo `/api/ponto/bater/`. (`app/static/pwa.js:107–157`.)

**O que decide definitivamente em `ponto/registro_batida.py`**

| Condição | Resultado |
|---|---|
| Mesmo tipo não retratado em ±60 segundos | Retorna a existente; **não cria**. (`app/ponto/registro_batida.py:106–115`.) |
| Mesma chave colaborador/timestamp | Retorna a existente; **não cria**. (`app/ponto/registro_batida.py:120–122`.) |
| `fato_de_chao=False` e competência fechada | Retorna `None, erro`; **não cria**. App/web passam `autoridade_humana=False` e omitem `fato_de_chao`, portanto este deriva para `True` nesses caminhos. (`app/ponto/registro_batida.py:130–137`; `app/api/views_core.py:680–694`; `app/ponto/services/triagem_batida.py:708–720`.) |
| Batida oposta na janela, sem casar marco do próprio tipo, e sem autoridade humana | Retorna `None, conflito`; **não cria**. A janela é 300 segundos com escala/marcos, senão 60 segundos. (`app/ponto/registro_batida.py:21–50`, `138–151`.) |
| Passou pelas condições anteriores | Executa **`Batida.objects.create`**, chama fechamento por evento e retorna a batida. (`app/ponto/registro_batida.py:157–161`.) |

**Portanto, há ramos reais que recusam ou não gravam.** O chamado vivo não é a condição dessas recusas; os pontos decisivos são os retornos dos endpoints e as guardas acima. (`app/ponto/views.py:274–329`; `app/api/views_core.py:402–458`, `549–568`, `695–718`; `app/ponto/registro_batida.py:106–161`.)

Quanto ao **“relógio”**, o código denomina `origem='app'` como `DO_RELOGIO`, e tanto API quanto formulário web gravam essa origem; `device_id` distingue os canais APK/PWA. Um fluxo separado de aparelho físico até esse chokepoint é **[não verificado]**. A rota AEJ existente devolve HTTP 410. (`app/ponto/services/procedencia_batida.py:30`; `app/ponto/registro_batida.py:53–61`; `app/api/views_core.py:680–684`; `app/ponto/services/triagem_batida.py:708–712`; `app/ponto/urls.py:9`; `app/ponto/views.py:56–71`.)

**Quem fecha o chamado**

As portas convergem principalmente para:

- **`encerrar()` → `fechado`**, sem guarda de premissa. (`app/chamados/models.py:1192–1202`.)
- **`reconciliar()` → `resolvido`**, condicionado ao motor e ao gate da premissa. (`app/chamados/models.py:434–472`; `app/chamados/catalogo/motor.py:111`.)
- **`transicionar()`**, decisão humana com guardas de perfil, justificativa, disputa e regularização. (`app/chamados/models.py:122–183`.)

| Porta/função | Quem chama e condição de fechamento |
|---|---|
| **Aprovação DP — `aprovar_justificativa_dp`** | `ponto.views.aprovar_justificativa` chama o serviço. Ele seleciona **somente o chamado mais recente** desse módulo, do colaborador, em `NAO_FECHADOS`, **sem filtro de data**, e chama `encerrar()`. O fechamento é best-effort depois da aprovação. (`app/ponto/views.py:1894–1899`; `app/ponto/services/justificativa.py:121–142`.) |
| **Evento de batida — `reconciliar_chamados_colab`** | Receiver `_reconciliador_post_save_batida` e comando `reconciliar_chamados`. Varre VIVOS; premissa morta chama `_retratar` → `reconciliar()`. Esse módulo usa `FATO_TURNO_ABERTO`; com dia, consulta o turno do chamado; sem dia, consulta globalmente. (`app/ponto/signals.py:206–218`; `app/chamados/management/commands/reconciliar_chamados.py:51–52`; `app/chamados/reconciliador.py:264–289`, `1203–1214`; `app/chamados/catalogo/premissa.py:48`.) |
| **`processar_alertas_turno._resolver_chamado_do_turno`** | `handle`, quando o turno materializado está fechado. Busca explicitamente `turno_aberto`/`entrada_bloqueada_turno_aberto`, por dia ou fallback legado; consulta outros turnos abertos e chama `retratar()`. (`app/ponto/management/commands/processar_alertas_turno.py:88–89`, `153–184`, `203–218`.) |
| **`reconciliar_fantasmas.handle`** | Com `--apply`, deriva datas do chamado, inclusive de `pendentes`; exige nenhuma delas aberta no núcleo e premissa morta; chama `retratar()`. O cron declarado usa **`--alarme`**, não aplicação. (`app/ponto/management/commands/reconciliar_fantasmas.py:147–158`, `178–184`, `247–279`; `app/config/crons.py:373`.) |
| **`reconciliar_grade.handle`** | Inclui o módulo por `FURO_MODULES`; quando todas as datas passam às condições de retratação e o catálogo concorda, com aplicação chama `retratar()`. (`app/chamados/catalogo/furo_modulos.py:15–16`; `app/ponto/management/commands/reconciliar_grade.py:206–208`, `281–300`.) |
| **`retratar_chamados_lei_cala.handle`** | Com `--modulo entrada_bloqueada_turno_aberto --apply`, seleciona VIVOS com dia passado em que `classificar_falta` não aponta falta; chama `retratar()`. (`app/chamados/management/commands/retratar_chamados_lei_cala.py:20–24`, `33–59`.) |
| **Absolvição pela célula — `retratar_chamados_absolvidos_por_celula`** | Consumidor do outbox chama a função; o módulo está em `MODULOS_DE_TURNO`. Exige célula com veredito que não acusa e sem marcos pendentes; chama `_retratar()`. (`app/chamados/reconciliador.py:336–337`, `792–827`, `1145`; `app/config/crons.py:375`.) |
| **Cartório — `_julgar_colab_corpo`** | Entradas `julgar_colab`/`julgar_celula`, acionadas por signals e `processar_cartorio`. Reconcilia por marcos acesos, marcos inexistentes, cobrança sem furo/dia sem trabalho ou perguntas todas duplicatas. O universo inclui esse módulo por `FURO_MODULES`. (`app/ponto/services/cartorio.py:70–94`, `709–744`, `1163`, `1175`, `1188–1196`, `1257–1266`, `1531–1560`; `app/ponto/signals.py:88–93`; `app/chamados/signals.py:166–174`; `app/ponto/management/commands/processar_cartorio.py:65`.) |
| **Decisão do dia — `fechar_o_fio_do_dia`** | Chamado por `veredito_celula` e `declarar_dia`. Seleciona VIVOS pela data ou célula e chama `reconciliar()`. (`app/ponto/services/veredito_celula.py:148–186`, `324`; `app/ponto/services/declarar_dia.py:197–198`.) |
| **Cobertura — `designar_cobertura` / `declarar_cobertura_passada`** | A view chama `designar_cobertura`; o serviço considera `chamado_origem_id`. A declaração passada considera VIVOS do dia. Ambos passam pelo catálogo e por `reconciliar()`, que pode recusar. (`app/ponto/views.py:3013–3020`; `app/ponto/services/cobertura.py:95`, `114–122`, `218–230`, `246`, `310–319`.) |
| **Competência trancada** | `trancar_periodo` chama `disparar_no_ato`; execução direta ou comando `encerrar_por_competencia_trancada` chama a porta homônima do model. Seleciona VIVOS pela competência e grava fechamento com via própria. (`app/ponto/services/fechamento.py:1395–1396`; `app/chamados/services/competencia_trancada.py:88–103`, `174–177`, `269–285`; `app/chamados/management/commands/encerrar_por_competencia_trancada.py:19–29`; `app/chamados/models.py:1204–1215`.) |
| **Desligamento — `executar_desligamento`** | View de desligamento ou comando de desligamentos agendados; encerra **todos** os chamados do colaborador em `NAO_FECHADOS`. (`app/colaboradores/views.py:1430–1434`; `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:35`, `57`; `app/colaboradores/services/desligamento.py:132–139`.) |
| **Transição humana genérica** | Detalhe → `acao_resposta_livre`; fio → `processar_post_fio`; ambos chamam `transicionar(novo_status, user)` sem filtro desse módulo, sujeitos às guardas. Outras ações do roteador também convergem para `transicionar('resolvido')`. (`app/chamados/views.py:546–547`, `669–674`; `app/chamados/services/acoes_chamado.py:70`, `222`, `277`, `313–321`; `app/chamados/services/fio.py:703–717`; `app/chamados/models.py:122–183`.) |
| **Materialização de pergunta — destino e clones** | Materializador, chamado por ações de disputa, fio e validação, reconcilia destino/clones por PK, sem filtro de módulo. Retém enquanto houver irmãs respondidas sem consumo. Resposta de ausência já decidida também reconcilia o destino; ausência aberta não o faz. (`app/chamados/services/materializacao.py:537–539`, `679–721`, `919`, `1004`, `1024`, `1255`; `app/chamados/services/acoes_disputa.py:139`; `app/chamados/services/fio.py:664`; `app/chamados/services/validacao.py:423`.) |
| **Fechamento da disputa vinculada — `disputa_fechada_fecha_chamado_pai`** | Signal de `DisputaSupervisao` fechada; considera o chamado-pai por FK, sem filtro de módulo, e chama `reconciliar()`. (`app/chamados/signals.py:18–45`.) |

**Fechar automaticamente não é garantido.** `_retratar` retém o chamado quando há irmãs respondidas sem consumo. `reconciliar()` pode recusar quando o dia segue furado para a folha, sem regularização, agrupamento ou outra cobrança viva que permita deduplicação. Sem dia apurável, o gate permite fechamento e registra essa ausência. O chamado emitido pelo web tem categoria `turno_aberto`, que o inclui na guarda de furo. (`app/chamados/reconciliador.py:435–457`; `app/chamados/models.py:484–525`, `915–921`, `2472`; `app/ponto/views.py:296–298`.)

**Enviar a regularização não fecha esse chamado:** no formulário, cria/atualiza justificativas e eleva a urgência; no serviço de regularização, passa para análise. A lista do fechamento explícito por saída retroativa também não contém esse módulo. (`app/ponto/services/triagem_batida.py:258–293`; `app/ponto/services/regularizacao.py:126–141`; `app/ponto/services/justificativa.py:274–285`.)

## 29/10 18:18 · sessao 1c944a938d1d · - · - · origem=Ronald (tmux)

Achados confirmados por leitura:

1. **Retenção de J2 não cobre cartório — outra / costura incompleta.** A regra só é consultada por `_retratar` e pela cascata (`app/chamados/reconciliador.py:474`; `app/ponto/services/justificativa.py:377`). O cartório chama `reconciliar()` diretamente (`app/ponto/services/cartorio.py:1163`, `:1175`, `:1196`, `:1266`), e o gate não consulta a retenção (`app/chamados/models.py:474`).

   Contraexemplo: C vivo aponta J2 pendente, cobra S19 em `marcos_faltantes`, existe célula/escala do dia, e aprovação de J1 registra S19. O signal julga a célula (`app/ponto/signals.py:131`); a lâmpada acesa satisfaz `marcos_acesos` (`app/ponto/services/lastro.py:170`), e o cartório fecha C diretamente (`app/ponto/services/cartorio.py:1163`), autorizado pela premissa por marco (`app/chamados/models.py:957`). Resultado: J2 pendente com C resolvido, contrariando a retenção declarada (`app/chamados/reconciliador.py:445`). O próprio save da justificativa também entra nesse desvio (`app/ponto/signals.py:161`).

2. **Deadlock entre duas aprovações — ordem de trava.** A pré-trava cobre VIVOS e a origem (`app/ponto/services/justificativa.py:100`), mas `_encerrar_bloqueio_de_entrada` seleciona NAO_FECHADOS (`app/ponto/services/justificativa.py:69`), conjunto que também contém `resolvido` (`app/chamados/catalogo/motor.py:32`).

   Contraexemplo: mesmo colaborador sem chamados VIVOS; C1 e C2 `entrada_bloqueada_turno_aberto`, ambos resolvidos; J1/J2 pendentes, respectivas origens C1/C2, sem saída informada. T1 pré-trava C1 e J1; T2 pré-trava C2 e J2 (`app/ponto/services/justificativa.py:106`, `:119`). O helper da origem retorna sem fechar (`:39`). O helper do bloqueio exclui a própria origem (`:72`): T1 tenta C2, T2 tenta C1. Uma aprovação precisa abortar; a prometida serialização entre aprovações não cobre esse conjunto (`:83`).

3. **Outra inversão real com regularização.** A regularização abre atomic, atualiza J existente sem saída e depois analisa chamados: J → C (`app/ponto/services/regularizacao.py:89`, `:106`, `:137`). A aprovação nova faz C → J (`app/ponto/services/justificativa.py:106`, `:119`). Com C aberto e J pendente sem saída, as duas transações podem esperar uma pela outra; `analisar()` chega ao escritor de estado (`app/chamados/models.py:566`). Trata-se de limite remanescente no sistema, já admitido genericamente na documentação da nova pré-trava (`app/ponto/services/justificativa.py:89`).

Sobre testes e contexto: [nao verificado]

- Os testes de J2 pela batida e pela cascata usam fixture sem célula nem escala (`app/ponto/tests/test_justificativa_encerra_origem.py:380`, `:404`, `:419`). O cartório retorna antes de julgar quando a célula não existe (`app/ponto/services/cartorio.py:1547`). Portanto, não demonstram cobertura do desvio do item 1.
- A marca é restaurada por `finally`, inclusive em exceções (`app/chamados/reconciliador.py:430`). Com `with atomic(), no_ato_de_aprovar(...)`, ela é removida antes da saída do atomic e dos callbacks de commit (`app/ponto/services/justificativa.py:117`, `:319`; `app/ponto/views.py:1792`). Existem callbacks de perguntas e retomada (`app/chamados/signals.py:121`, `:202`); dano adicional específico desses callbacks: [nao verificado]. Nenhuma troca de thread foi identificada nesses corpos; comportamento em threads externas: [nao verificado].

Sobre efeitos colaterais:

- O receiver de rebaixamento não captura exceções (`app/chamados/signals.py:68`), mas o núcleo já rebaixa a urgência antes do save, tornando a execução normal do receiver um no-op (`app/chamados/models.py:210`, `:278`).
- S143 agora tem savepoint e captura a falha (`app/chamados/signals.py:88`). `_retratar` chama o resolver novamente, sem captura; falha persistente desfaz a aprovação inteira (`app/chamados/reconciliador.py:507`, `:521`; `app/ponto/services/justificativa.py:196`).
- O cartório é chamado dentro de `engolir`, que não cria savepoint (`app/chamados/signals.py:171`; `app/core/observ.py:85`). Entretanto, as escritas centrais examinadas têm seus atomics (`app/ponto/portas/celula.py:639`; `app/chamados/models.py:226`); não confirmei contraexemplo adicional de sucesso silencioso com rollback causado pelo cartório. A leitura dos próprios dados ainda não commitados, isoladamente, não demonstrou gravação incorreta; o desvio concreto é o fechamento sem consultar a retenção, item 1.

## 29/10 18:18 · sessao bf4283445a42 · - · - · origem=Ronald (tmux)

Inventário enviado: 12 portas específicas/por dia, mais transição humana, materialização de destino/clones e fechamento do pai por disputa. As três genéricas não filtram módulo; dependem do vínculo e das guardas da porta (`app/chamados/services/acoes_chamado.py:313`, `app/chamados/services/fio.py:703`, `app/chamados/services/materializacao.py:702`, `app/chamados/signals.py:28`).

Exclusões confirmadas: EB não integra `CALADOS_POR_ISENCAO`, a lista da varredura LIMBO nem o universo de `lastro.fechar` (`app/chamados/catalogo/modulos.py:235`, `app/chamados/services/limbo_folgas.py:117`, `app/ponto/services/lastro.py:216`).

## 29/10 18:18 · sessao d3f044fc1ef0 · - · - · origem=Ronald (tmux)

Achados e censo:

- **Ordem de trava — deadlock entre duas aprovações, sem chamado novo.** `travar_origem_antes` trava VIVOS + origem, por PK, mas `_encerrar_bloqueio_de_entrada` seleciona NAO_FECHADOS, que inclui `resolvido`; resolvido não-origem fica fora da trava antecipada. Contraexemplo: mesmo colaborador; C1/pk1 `resolvido`, módulo `entrada_bloqueada_turno_aberto`; C100/pk100 vivo. J1 aponta C100, J2 aponta C1. T1 aprovar J1 trava C100; T2 aprovar J2 trava C1 e espera C100; T1 tenta `_encerrar_bloqueio_de_entrada` e espera C1. Ciclo C100→C1/C1→C100. Sítios: `app/ponto/services/justificativa.py:101`, `:106`, `:117`, `:119`, `:194`, `:195`, `:69`; universo em `app/chamados/catalogo/motor.py:32`. A declaração “duas aprovações ... se serializam” em `app/ponto/services/justificativa.py:85` não cobre esse caso.

- **Outra — duas transições/trilhas na materialização da origem de turno.** Entrada 07h + saída materializada 19h, C de `turno_aberto` vivo apontando a própria J: criar Batida dispara reconciliador, que retrata C para `resolvido`; cascata posterior inclui `resolvido` e o passa a `fechado`. São duas linhas `estado_chamado` para o ato. Caminho: `app/ponto/services/justificativa.py:338`; `app/ponto/registro_batida.py:157`; `app/ponto/signals.py:206`; `:215`; `app/chamados/reconciliador.py:286`, `:1255`, `:1266`, `:508`; `app/chamados/models.py:444`; depois `app/ponto/services/justificativa.py:371`, `:379`; `app/chamados/catalogo/motor.py:32`, `:114`; `app/chamados/models.py:1201`, `:227`. A cascata já existia, mas é sítio tocado e continua contrariando o contrato de uma trilha. O teste novo de materialização escolhe `justificativa_colab` e não mede trilhas: `app/ponto/tests/test_justificativa_encerra_origem.py:203`, `:213`, `:229`.

- **Escritor de aprovada não alcançado: Django admin, preexistente — FORA-DO-ESCOPO.** `JustificativaAdmin` herda diretamente `ModelAdmin`, sem restrição de edição ou delegação; `status` é campo editável. Admin está exposto nas URLs. `app/ponto/admin.py:18`, `:19`, `:20`; `app/ponto/models.py:159`; `app/config/urls.py:24`. O selo novo exclui expressamente esse escritor: `app/ponto/tests/test_selo_aprovacao_encerra_origem.py:13`. As três atribuições explícitas são alcançadas: `app/ponto/services/justificativa.py:182`, `:194`, `:360`, `:387`; `app/ponto/views.py:1796`, `:1837`.

- **status_local fora das portas:** censo AST executado em memória, sem Django/DB/escrita, percorreu 739 arquivos e retornou zero violações. Usou as funções em `app/chamados/tests/test_selo_status_so_pelas_portas.py:317`, `:322`; allowlist em `:37`. Escritores detectados: `_gravar_estado`, `abrir`, `criar` (e as três portas de DisputaSupervisao para o outro campo). Limites de resolução declarados pelo selo em `app/chamados/tests/test_selo_status_so_pelas_portas.py:17`. Nenhuma escrita direta nova do diff fora da porta: origem e bloqueio delegam `retratar`, cascata usa `encerrar`: `app/ponto/services/justificativa.py:42`, `:75`, `:379`.

- **Segundo fechador em aprovar DP:** o fecho antigo externo foi substituído pela chamada interna `_encerrar_bloqueio_de_entrada`; origem é excluída desse seletor e ambos passam pela unidade `retratar`: `app/ponto/services/justificativa.py:72`, `:75`, `:194`, `:195`. Não encontrei segundo fecho forçado remanescente nessa função; o forçado permanece na cascata da materialização: `app/ponto/services/justificativa.py:379`.

- **invalidar_fechamento:** as operações suscetíveis a erro estão em savepoints próprios, com captura fora deles: leitura `app/ponto/services/fechamento.py:1616`, UPDATE `:1619`, captura `:1629`. Não identifiquei defeito novo seguro nessa alteração; prova de execução transacional contra banco não realizada [nao verificado].

_219 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
