# CODEX-ESTADO

> Ultima resposta processada: **25/11/2026 20:25** · publicado em **25/11/2026 20:27** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 25/11 20:25 · sessao cdd477938fd6 · PARCIAL-BORDA-SEMIABERTA · verificador · origem=Code

**Volta 1 — reprova por escopo.** A comparação estrita realiza o aval para as batidas do caso col306, na precisão de minuto usada pelo juiz. Ela também muda a resposta à pergunta “este **marco previsto sem batida** está dispensado?”. O aval fala da batida na borda, e a regra escrita no sítio da grade diz que a dispensa do marco usa bordas inclusivas. `aval.txt:1` · `app/ponto/turnos.py:2642` · `app/ponto/turnos.py:2662` · `app/escala/utils.py:1245` · `app/escala/utils.py:1249`

Os três hashes de *blob* da cópia coincidem com os do diff fornecido. A identidade do commit `HEAD` ficou **[não verificado]** porque a cópia não contém metadados Git. A contagem prévia exigida para a frota inteira também ficou **[não verificado]**. O dado fornecido traz um caso, col306. `obra.diff` · `aval.txt:1` [nao verificado]

## 1. Escopo, escritores e leitores

**Procedente.** Para as quatro batidas informadas, `15:54` era a única dentro pelo comparador antigo. Com o novo, o período sai `fora` e o ramo de `FATO_EM_AUSENCIA` cede lugar ao julgamento do resto. Exemplo do efeito fora do aval: com parcial `13:00–17:00` e E13/S17 sem batida, ambos eram dispensados e ambos passam a `missing`. O fim às `19:00` do caso nominal também pode voltar a ser cobrado se o marco estiver `missing` na grade. `aval.txt:1` · `app/ponto/turnos.py:2655` · `app/ponto/supra_juiz.py:392` · `app/escala/utils.py:1255` · `app/ponto/tests/test_cobparc_marco_no_periodo.py:176`

| Campo ou valor | Quem escreve | Quem lê |
|---|---|---|
| `Ausencia.hora_ini` / `hora_fim` | A aprovação e a edição pelo DP chamam `decidir`. `_aplicar_periodo` atribui os valores e `_decidir_no_ato` salva. `app/ponto/views.py:2800` · `app/ponto/views.py:2844` · `app/ponto/services/ausencia.py:1051` · `app/ponto/services/ausencia.py:1099` | A validação da porta, a trilha antes/depois, a captura do insumo da folha, o formulário e o juiz do período. A materialização de chamados chama o juiz com `()` e usa exclusivamente `sem_periodo`, portanto essa chamada independe das bordas. `app/ponto/services/ausencia.py:119` · `app/ponto/services/ausencia.py:1039` · `app/ponto/services/ausencia.py:1071` · `app/templates/ponto/partials/painel_ausencia.html:313` · `app/ponto/turnos.py:2655` · `app/chamados/services/materializacao.py:76` |
| `periodo_parcial` | `_marcar_ausencias` grava no dia a resposta do juiz sobre horas reais. `ata_do_dia` copia para a ata, que `lavrar_veredito` grava. `grade_da_celula` transporta de volta o valor lavrado. `app/escala/utils.py:1245` · `app/ponto/services/cartorio.py:784` · `app/ponto/portas/celula.py:1127` · `app/escala/services/leitor_celula.py:433` | A própria grade usa `sem_periodo` para pular a dispensa. `classificar_dia` lê para decidir fato, cadastro e cobrança do resto. `parcial_so_lista` lê para a emissão, e `_cob_parcial_da_impressao` usa o valor no hash. `app/escala/utils.py:1255` · `app/ponto/supra_juiz.py:394` · `app/ponto/supra_juiz.py:452` · `app/ponto/supra_juiz.py:543` · `app/ponto/services/cartorio.py:442` · `app/ponto/services/cartorio.py:482` |
| `cobertura_parcial` | `_marcar_ausencias` grava o booleano de `cobre_parte_do_dia`, que depende de tipo, duração em dias e minutos declarados. Cartório e leitor transportam o carimbo. `app/escala/utils.py:1198` · `app/escala/utils.py:1241` · `app/ponto/turnos.py:2602` · `app/ponto/services/cartorio.py:784` · `app/escala/services/leitor_celula.py:433` | `classificar_dia`, `parcial_so_lista`, impressão, dispensa integral, ramos de emissão/reconciliação do cartório, status da lavra e `ausencia_parcial_cobra_o_resto`, usado pelo juiz de dia sem trabalho, texto da declaração e tarefa do dia. `app/ponto/supra_juiz.py:281` · `app/ponto/services/cartorio.py:442` · `app/ponto/services/cartorio.py:482` · `app/ponto/services/cartorio.py:818` · `app/ponto/services/cartorio.py:1266` · `app/ponto/services/cartorio.py:1830` · `app/ponto/services/cartorio.py:1889` · `app/ponto/status_dia.py:141` · `app/ponto/status_dia.py:192` · `app/chamados/juizes.py:487` · `app/chamados/juizes.py:503` · `app/chamados/services/declaracao_texto.py:120` · `app/chamados/services/tarefa_do_dia.py:248` |
| `dispensada` | Na parcial, `_marcar_ausencias` escreve na célula da grade. O cartório copia para a lâmpada, e `_celulas_da_ata` repõe a chave ao ler. `dispensar_por_ausencia` escreve na lâmpada por ausência integral ou justificativa aprovada. `app/escala/utils.py:1255` · `app/ponto/services/cartorio.py:613` · `app/escala/services/leitor_celula.py:145` · `app/ponto/services/cartorio.py:818` | Impressão, contagem de batidas do cartório, status, supra-juiz, contagem de casadas na ata, lastro e `marcos_do_jogo`, usado em relatórios, acompanhamento e indicadores. Os emissores recebem o efeito via lâmpadas apagadas e `missing`. `app/ponto/services/cartorio.py:483` · `app/ponto/services/cartorio.py:1452` · `app/ponto/status_dia.py:185` · `app/ponto/supra_juiz.py:324` · `app/escala/services/leitor_celula.py:63` · `app/escala/services/leitor_celula.py:105` · `app/ponto/services/lastro.py:336` · `app/relatorios/furos.py:15` · `app/escala/services/furos_diarios.py:19` · `app/inteligencia/calculadores.py:57` · `app/inteligencia/calculadores.py:156` · `app/ponto/services/cartorio.py:1889` |
| `cob_parcial` / `CelulaDia.impressao` | `_cob_parcial_da_impressao` compõe o resultado agregado e os índices dispensados. `impressao_insumos` incorpora `cobparcial:1...` no hash. `carimbar` grava o hash. `app/ponto/services/cartorio.py:473` · `app/ponto/services/cartorio.py:550` · `app/ponto/services/cartorio.py:574` · `app/ponto/portas/celula.py:1218` | `impressao_insumos` recebe o valor nos dois ramos do cartório. Ambos comparam o hash para decidir se pulam o julgamento. A porta também o lê na checagem de idempotência, e `celula_veredito_velho` o expõe no diagnóstico. `app/ponto/services/cartorio.py:1285` · `app/ponto/services/cartorio.py:1426` · `app/ponto/services/cartorio.py:1437` · `app/ponto/portas/celula.py:1227` · `app/ponto/management/commands/celula_veredito_velho.py:110` |

## 2. Lei da dispensa e testes existentes [nao verificado]

**Procedente.** Os cortes originais tratam do cadastro, da classificação das **batidas**, da lista sem push e da virada noturna. O EMIS-ORDEM manda cobrar só o marco fora do período. A grade explicita como esse corte foi implementado: incluiu as bordas para marcos previstos. Os testes antigos documentam E13 e S17 dispensados na parcial `13:00–17:00` e S07 de D+1 dispensada na parcial `23:00–07:00`. O novo aval exclui as bordas para batidas reais. Um corte escrito que estenda essa exclusão aos marcos previstos ficou **[não verificado]**. `app/docs/CORTES.md:460` · `app/docs/CORTES.md:464` · `app/docs/CORTES.md:471` · `app/docs/CORTES.md:475` · `app/docs/RELATO.md:12086` · `app/escala/utils.py:1246` · `app/ponto/tests/test_cobparc_marco_no_periodo.py:10` · `aval.txt:1`

**12 testes existentes com asserção atingida pela mudança na dispensa**, identificados por execução isolada do primeiro caso e inspeção dos outros 11. A execução da suíte Django ficou **[não verificado]**. [nao verificado]

| Teste | Asserção atingida | [nao verificado]
|---|---|
| `test_RED_noturna_a_saida_da_madrugada_dentro_de_23_07_e_dispensada` | S07 de D+1 perde a dispensa. Confirmado em memória com o código da grade. `app/ponto/tests/test_cobparc_marco_no_periodo.py:72` |
| `test_RED_marcos_DENTRO_do_periodo_saem_dispensados_e_o_dia_nao_acusa` | E13/S17 perdem a dispensa. `app/ponto/tests/test_cobparc_marco_no_periodo.py:176` |
| `test_RED_marco_FORA_do_periodo_segue_cobrado` | S17 também fica apagada, além de E13. `app/ponto/tests/test_cobparc_marco_no_periodo.py:187` |
| `test_RED_dia_parcial_sem_batida_o_resto_e_FURO_SEM_COBRANCA_nomeando_so_os_marcos_de_fora` | A lista esperada de dois marcos ganha 13:00 e 17:00. `app/ponto/tests/test_cobparc_marco_no_periodo.py:322` |
| `test_RED_mudar_o_periodo_rejulga_a_celula` | A primeira ata ganha dois `missing`. A edição de `13–17` para `14–17` deixa o mesmo conjunto de dispensas, vazio. `app/ponto/tests/test_cobparc_marco_no_periodo.py:374` · `app/ponto/services/cartorio.py:482` |
| `test_RED_marcos_faltantes_do_dia_parcial_sao_os_de_fora` | O texto passa a receber quatro marcos faltantes. `app/ponto/tests/test_cobparc_marco_no_periodo.py:416` |
| `test_RED_vivo_do_dia_parcial_espelha_so_os_marcos_de_fora` | O chamado vivo fica com quatro marcos faltantes. `app/ponto/tests/test_cobparc_marco_no_periodo.py:445` |
| `test_RED_E03_e_S07_de_D1_dentro_de_23_07_saem_dispensados` | E03 segue dispensada e S07 perde a dispensa. `app/ponto/tests/test_cobparc_v4_procedentes.py:96` |
| `test_RED_dia_parcial_aparece_com_o_resto_furado` | O relatório ganha E13 e S17 na lista de furos. `app/ponto/tests/test_cobparc_v4_procedentes.py:292` |
| `test_RED_previstos_do_furo_diario_batem_o_lavrado` | Some a única dispensa, S17. O par esperado `(3, 1)` passa a `(4, 2)`. `app/escala/tests/test_cobparc_rotulo_furos_diarios.py:53` · `app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:50` |
| `test_RED_marco_dispensado_nao_e_batido` | Some a única dispensa, S17. O denominador esperado passa de 3 para 4. `app/relatorios/tests/test_cobparc_rotulo_dispensada.py:71` · `app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:50` |
| `test_RED_periodo_fora_o_dia_concorde_sai_da_retencao` | S14 volta a `missing`. A asserção de que nenhum marco no conjunto inclusivo fica `missing` falha. `app/escala/tests/test_cobertura_parcial_periodo.py:103` · `app/escala/tests/test_cobertura_parcial_periodo.py:289` |

À parte, são **dois métodos existentes com quatro asserts invertidos no diff** para a pergunta sobre batidas: `test_dentro_e_fora_com_bordas_inclusivas`, renomeado para `test_dentro_e_fora_com_bordas_semiabertas`, e `test_RED_noturna_o_fim_menor_cruza_a_meia_noite_e_e_do_dia_de_inicio`. `obra.diff` · `app/escala/tests/test_cobertura_parcial_periodo.py:44` · `app/escala/tests/test_cobertura_parcial_periodo.py:53`

## 3. Classes de reprovação

| Classe | Resultado |
|---|---|
| Segundo juiz registrado | **Duvidoso como achado.** As duas chamadas da grade delegam à função já registrada para o julgamento da batida. O defeito é a extensão da nova regra à outra pergunta. `app/core/juizes.py:265` · `app/escala/utils.py:1245` · `app/escala/utils.py:1260` |
| Segundo escritor fora da porta | **Duvidoso em produção.** A escrita continua na grade e a persistência na porta da célula. As inserções diretas de batida e ausência estão nas *fixtures* do teste novo. `app/escala/utils.py:1241` · `app/ponto/services/cartorio.py:1901` · `app/ponto/portas/celula.py:1231` · `app/ponto/tests/test_parcial_borda_semiaberta.py:103` · `app/ponto/tests/test_parcial_borda_semiaberta.py:111` |
| Relógio fora da borda / `timezone.now()` solto | **Duvidoso no juiz e na chamada direta do teste.** O juiz é puro, e o teste passa `DEPOIS_DE_TUDO`, `HOJE` e `turno_encerrado=True`. **Procedente no caminho indireto da fixture:** os `create()` sem relógio congelado disparam o cartório via sinais, e a entrada do cartório consulta a data e o instante reais. Dependência das asserções em relação a esse efeito indireto **[não verificado]**. `app/ponto/turnos.py:2648` · `app/ponto/tests/test_parcial_borda_semiaberta.py:103` · `app/ponto/tests/test_parcial_borda_semiaberta.py:114` · `app/escala/tests/instante.py:12` · `app/ponto/signals.py:85` · `app/ponto/signals.py:131` · `app/ponto/signals.py:143` · `app/ponto/services/cartorio.py:2064` · `app/ponto/services/cartorio.py:2088` |
| Derivação paralela | **Duvidoso em produção.** As duas comparações de borda da grade vão ao mesmo juiz. O leitor da ata apenas transporta. Há uma comparação inclusiva remanescente no oráculo do teste de retenção, que torna visível a regressão. `app/escala/utils.py:1245` · `app/escala/utils.py:1260` · `app/escala/services/leitor_celula.py:429` · `app/escala/tests/test_cobertura_parcial_periodo.py:299` |
| RED sem mordida | **Duvidoso para os dois mutantes.** Em memória, os nove testes puros novos passaram com `ini < m < fim`. `ini <= m < fim` falhou em cinco, inclusive o caso nominal. `ini < m <= fim` falhou em três, inclusive `19:00` isolado e `02:00` de D+1. **Procedente como lacuna de cobertura:** os dois testes de integração novos param no supra-juiz e deixam sem asserção a dispensa na borda, a impressão e a relavra. `app/ponto/tests/test_parcial_borda_semiaberta.py:43` · `app/ponto/tests/test_parcial_borda_semiaberta.py:122` |
| Ordem de trava | **Duvidoso como regressão da ordem de aquisição.** A comparação é pura e o diff conserva as chamadas e os blocos de persistência existentes. A lacuna de alcance histórico é outra, descrita abaixo. `app/ponto/turnos.py:2648` · `app/ponto/services/ausencia.py:1079` · `app/ponto/portas/celula.py:994` |
| Pouso antes do pré-requisito | **Duvidoso quanto à cronologia real [não verificado].** O aval condiciona a vaga à GUARDA-10-B. A cópia ainda contém `(10, 2026)`, cuja retirada está registrada como parte dessa obra. O censo prévio pedido no aval também está **[não verificado]**. `aval.txt:1` · `app/docs/PROMPTS.md:2967` · `app/ponto/services/fechamento.py:45` |
| ESMERIL-SEMÂNTICA | **Procedente.** Os dois insumos usam minutos desde o início do dia do turno. Um é fato ocorrido, o outro é obrigação prevista. A docstring define `horas` como batidas, enquanto a grade fornece `_off[j]` para decidir a dispensa inclusiva de um marco. A unidade coincide e o contrato de borda diverge. `app/ponto/turnos.py:2648` · `app/escala/utils.py:366` · `app/escala/utils.py:1246` · `app/escala/utils.py:1260` |
| `except` novo | **Duvidoso como achado.** O `except` de leitura de hora ilegível já está em `_minutos_do_dia`. O trecho alterado recebe o resultado dessa conversão. `obra.diff` · `app/ponto/turnos.py:2622` · `app/ponto/turnos.py:2661` |

## 4. Noturna, meia-noite e precisão

- **Duvidoso como defeito de virada.** `23:00–00:00` termina em `1440`, com `23:59` dentro e `00:00` de D+1 fora. `00:00–00:02` começa no próprio D, com só `00:01` dentro. Em `23:59–00:01`, `00:00` de D+1 fica dentro. `24:00` como `hora_fim` é recusada pelo parser `%H:%M` da aprovação e da edição. O campo é `TimeField`. `app/ponto/turnos.py:2655` · `app/ponto/views.py:2803` · `app/ponto/views.py:2844` · `app/ponto/models.py:768`
- **Procedente na precisão, se a lei for pelo instante real.** Um período `23:59–00:00` tem zero minutos inteiros no interior. A batida real `23:59:30` está temporalmente dentro, mas o juiz recebe `1439` e devolve `fora`. O mesmo vale para `15:54:30` na parcial `15:54–19:00`. A resolução em minutos já existia. Excluir os extremos torna observável a perda dos segundos no primeiro minuto. Os segundos do caso col306 e a intenção do aval quanto a essa precisão estão **[não verificado]**. `app/ponto/turnos.py:2622` · `app/ponto/turnos.py:2657` · `app/ponto/turnos.py:2681` · `aval.txt:1`
- **Procedente na documentação.** A comparação implementa um intervalo **aberto nas duas pontas**, chamado de “semiaberto” no texto. A docstring também dá `1500` para `01:10` de D+1, que vale `1510`. E “sem mudança” no caso de hora ilegível descreve, no máximo, a acusação: com `['?', '15:54']`, o valor muda de `dentro` para `nao_sei`, e ambos ainda podem gerar `FATO_EM_AUSENCIA`. `app/ponto/turnos.py:2640` · `app/ponto/turnos.py:2653` · `app/ponto/turnos.py:2662` · `app/ponto/tests/test_parcial_borda_semiaberta.py:16` · `app/ponto/supra_juiz.py:403` · `app/ponto/supra_juiz.py:456`

## 5. Atas já lavradas, competência e dinheiro

- **Procedente — alcance retroativo.** A leitura normal entrega o `dentro` já lavrado até o cartório passar de novo. Na próxima passagem, ele remonta a grade. Se a única batida conhecida dentro era de borda, a resposta vira `fora` ou `nao_sei` se sobrar hora ilegível. A perda de um índice dispensado também muda a impressão, mesmo quando a resposta agregada permanece. O hash diferente evita o pulo sem `--forcar`. O lote `processar_cartorio --apply` percorre todas as células passadas da empresa, e os sinais de batida/ausência oferecem a entrada avulsa. A porta salva a nova ata e o hash, inclusive quando preserva decisão do admin. Campo de impressão novo ou *bump* de versão são dispensáveis para esse alcance. A impressão da parcial entra sem recorte próprio de competência, em conflito com a foto do passado prevista pela L-124. Sem fato novo, o resto que voltou a ser cobrado segue o portão de lista sem push. `app/escala/services/leitor_celula.py:429` · `app/ponto/services/cartorio.py:482` · `app/ponto/services/cartorio.py:1200` · `app/ponto/services/cartorio.py:1426` · `app/ponto/services/cartorio.py:1805` · `app/ponto/services/cartorio.py:1901` · `app/ponto/portas/celula.py:1095` · `app/ponto/portas/celula.py:1231` · `app/ponto/management/commands/processar_cartorio.py:49` · `app/config/crons.py:616` · `app/ponto/signals.py:131` · `app/docs/LEIS.md:145`
- **Procedente — a guarda financeira deixa passar a relavra da célula.** Na cópia, `GUARDA_COMPETENCIA` lista 09 e 10/2026 e exclui da **escrita do recálculo** os colaboradores das empresas que efetivamente exportaram a competência. Há checagem equivalente no retrato de `DiaPago`. A ata, o veredito e a impressão têm outra porta. O export lê o veredito, os protestos e a exceção da pausa. Por isso, tirar `FATO_EM_AUSENCIA` pode remover uma retenção ou deixá-la sob outro motivo, como `FURO_PARCIAL`. O conteúdo e o hash do TXT já entregue são guardados pelo modelo de exportação. Competências, empresas e retenções efetivamente atingidas na frota **[não verificado]**. `app/ponto/services/fechamento.py:36` · `app/ponto/services/fechamento.py:548` · `app/ponto/services/fechamento.py:567` · `app/ponto/services/dia_pago.py:446` · `app/ponto/portas/celula.py:1218` · `app/folha/export.py:1273` · `app/folha/export.py:1293` · `app/folha/export.py:968` · `app/folha/models.py:143`
- **Procedente — há caminho até dinheiro em recálculo.** O comparador só muda a classificação e a dispensa na grade. Depois de relavrada, a lâmpada passa de acesa sem luz a apagada. O motor conta lâmpadas acesas para decidir se usa os períodos da ata. Com duas luzes reais, uma dispensa de borda e três batidas apuráveis entre entrada e saída, a contagem antiga permite passar pela guarda (`3 > 3` é falso), e a nova toma o outro caminho (`3 > 2`). Há também um efeito possível em `DiaPago`: sem realizado e com todas as lâmpadas agora apagadas, o leitor pode classificar o dia da demissão como `fora_vinculo`, tirando o dia do mapa de previsto e abono. Delta de rubricas ou de pagamento, inclusive para col306, **[não verificado]**. `app/escala/utils.py:1255` · `app/ponto/services/cartorio.py:613` · `app/ponto/juiz_batida.py:241` · `app/ponto/motor_calculo_v2.py:660` · `app/ponto/motor_calculo_v2.py:669` · `app/escala/services/leitor_celula.py:379` · `app/ponto/services/dia_pago.py:386` · `app/ponto/services/fechamento.py:1073`

## 25/11 20:20 · sessao 8f3e55121bda · PARCIAL-BORDA-SEMIABERTA · censo · origem=Code

# Censo da PARCIAL-BORDA-SEMIABERTA

A cópia usa `ini < m < fim`. As duas bordas ficam excluídas, tanto para **batidas reais** quanto para **marcos previstos faltantes**, porque a grade passa os dois ao mesmo juiz. A identidade Git da cópia, a execução do deploy e o estado das atas em produção ficam **[nao verificado]**. `app/ponto/turnos.py:2655–2664`, `app/escala/utils.py:1245–1261`.

## 1. Chamadores do juiz

| Chamador | O que passa em `horas` | O que faz com a resposta |
|---|---|---|
| `escala.utils._marcar_ausencias`, via import local com apelido `_periodo_parcial` | Batidas reais que a grade colocou no dia, casadas ou órfãs. `_horas_reais` devolve minutos ou `None` para hora ilegível. | Grava a resposta agregada em `dia['periodo_parcial']`. `app/escala/utils.py:1188,1241–1245`, `app/ponto/turnos.py:2667–2688`. |
| A segunda chamada de `_marcar_ausencias`, pelo mesmo apelido | `[_off[j]]`, lista de um **marco previsto**, só se a célula está `missing` e a coluna tem deslocamento na cadeia do turno. | Se der `dentro`, grava `dispensada` e `missing=False`, com `hora=None`. Se o resultado agregado for `sem_periodo`, pula essa etapa. `app/escala/utils.py:1253–1261`. |
| `chamados.services.materializacao._coberto_por_ausencia`, via import local sem apelido | `()`, tupla vazia. | Depois de selecionar a ausência vencedora, considera coberto para a disputa se ela explica o dia inteiro ou se é de tipo diferente de `falta` e o juiz devolve `sem_periodo`. `app/chamados/services/materializacao.py:59–86`. |
| `JuizPuroTest` | Strings `HH:MM`, lista vazia, minutos de D e D+1 e horas ilegíveis produzidas por `horas_reais_do_dia`. | Compara o retorno com os quatro valores esperados. `app/escala/tests/test_cobertura_parcial_periodo.py:23,35–88`. |
| `BordaSemiabertaJuizPuroTest` | Strings `HH:MM`, minutos de D e D+1, `'?'` e `None`. | Testa as duas bordas, o interior e a precedência de uma hora conhecida dentro sobre uma ilegível. `app/ponto/tests/test_parcial_borda_semiaberta.py:28,43–78`. |

`core/juizes.py` registra o endereço do juiz como string. O teste ponta a ponta de col306 chama a grade e o supra-juiz. `app/core/juizes.py:265–269`, `app/ponto/tests/test_parcial_borda_semiaberta.py:107–131`.

## 2. Escritores e leitores das duas chaves

| Local | Escrita ou leitura |
|---|---|
| Grade viva | `_marcar_ausencias` escreve `cobertura_parcial` como booleano no ramo de ausência diferente de `falta`, escreve `periodo_parcial` quando parcial e lê `sem_periodo` para controlar a dispensa por marco. O builder legado transporta o dict do segmento inteiro ao remontar as colunas. `app/escala/utils.py:1043–1055,1198–1202,1230–1261`. |
| Ata do cartório | `ata_do_dia` lê o dia da grade, escreve `cobertura_parcial=True` e copia `periodo_parcial` quando preenchido. A porta copia o dict para `CelulaDia.ata` e o carimbo o salva junto com o julgamento. `app/ponto/services/cartorio.py:748–789,1898–1923`, `app/ponto/portas/celula.py:1085–1104,1127–1135,1218–1239`. |
| Leitor da célula | `grade_da_celula` lê as duas chaves da ata e as repõe no dia de saída. Também transporta `dispensada` por marco. O remapeamento preserva as chaves do dia. A antiga função que consultava a ausência atual e recalculava os carimbos foi retirada. `app/escala/services/leitor_celula.py:114–146,281–299,418–451`. |
| Impressão do cartório | `_cob_parcial_da_impressao` lê `periodo_parcial`, usa `bool(cobertura_parcial)` se o período estiver ausente e acrescenta os índices das células `dispensada`. `impressao_insumos` inclui isso sob `cobparcial:1` antes do hash. `app/ponto/services/cartorio.py:473–486,550–554,574,1285–1288,1426–1432`. |
| Supra-juiz | `classificar_dia` lê `cobertura_parcial` para decidir integralidade, dia de cobrança e existência de resto. Lê `periodo_parcial` nos ramos com realizado/órfã e com batida sem par, e usa a chave com `missing` em `COBRANCA_EM_DIA_COBERTO`. O comando `supra_juiz` recebe os carimbos pelo leitor da ata. `app/ponto/supra_juiz.py:281–282,317–327,375–418,450–463,538–547`, `app/ponto/management/commands/supra_juiz.py:129,223–234`. |
| Cartório, dispensa, status e chamados | `dispensar_por_ausencia` lê `cobertura_parcial` para a dispensa integral. O ramo sem adesão também lê a chave. A emissão e o espelhamento de chamado vivo usam a chave e os marcos apagados da ata. `fatos_do_status` repassa o booleano a `status_da_lavra`, que o usa ao classificar ausência sem batida. `app/ponto/services/cartorio.py:815–831,1264–1268,1825–1841,1889–1897`, `app/ponto/status_dia.py:104–110,135–142,166–192`. |
| Chamados, lista do admin | `parcial_so_lista` lê as duas chaves. `sem_periodo` vai direto à lista. Para parcial com período, a decisão consulta o julgamento anterior e `houve_fato_novo`. O cartório passa o dia da grade. Fábrica, reconciliação C2, disputa, cluster e emissão retroativa chegam pelo mesmo predicado sobre a ata ou o dia projetado dela. `app/ponto/services/cartorio.py:416–446,1805–1806`, `app/chamados/services/fabrica_da_celula.py:122–125`, `app/chamados/reconciliador.py:995–1008`, `app/chamados/services/disputa_emissao.py:930–955`, `app/ponto/services/cluster_espurio.py:164–171`, `app/ponto/management/commands/emitir_furo_retroativo.py:115–123`. |
| Chamados, resto cobrável | `ausencia_parcial_cobra_o_resto` lê `tipo_dia` e `cobertura_parcial`. É usado por `dia_sem_trabalho`, pelo texto de declaração e pela tarefa do dia. Acompanhamento, relatório de furos e os dois indicadores também chamam esse predicado. `app/chamados/juizes.py:477–506`, `app/chamados/services/declaracao_texto.py:104–125`, `app/chamados/services/tarefa_do_dia.py:248`, `app/escala/services/furos_diarios.py:42–52`, `app/relatorios/furos.py:195–209`, `app/inteligencia/calculadores.py:29–50,131–149`. |
| Espelho, PDF e API | O espelho lê a grade da célula e usa o builder nos caminhos previstos no seu código. Projeta marcos, realizado, faltantes e status em outro dict. O PDF usa os dias do espelho e também chama a grade para órfãs e isenção. `api_espelho_v2` consulta o espelho e o status lavrado, e monta o payload com campos próprios. Esses caminhos consomem efeitos da grade ou da ata, sem consultar diretamente as duas chaves. `app/ponto/services/espelho.py:167–170,503–575,688–704,776–786`, `app/relatorios/pdf_espelho.py:543–557,596–609`, `app/api/views.py:1355–1366,1491–1498,1542–1564,1612–1632`. |
| Retenção da folha | `motivos_retencao_celula` usa o veredito lavrado. Uma mudança em `periodo_parcial` pode chegar à retenção pelo veredito que o cartório deriva dos códigos do dia. `app/folha/export.py:1176–1184,1205–1213,1231`, `app/ponto/portas/celula.py:1007–1021`. |

A grade faz os dois julgamentos de pertinência pelo mesmo juiz. Os leitores das chaves usam os carimbos ou as dispensas da grade. `_coberto_por_ausencia` consulta a ausência vencedora por outra via e testa se o período foi declarado, com `horas=()`. `app/escala/utils.py:1241–1261`, `app/escala/services/leitor_celula.py:429–451`, `app/ponto/supra_juiz.py:392–418,450–463`, `app/chamados/services/materializacao.py:76–86`.

## 3. Comparações fora do juiz

**Nenhuma outra comparação de pertinência em código de produção foi localizada** entre batida ou marco e `Ausencia.hora_ini`/`hora_fim`, inclusive por queryset. Os pontos examinados fazem o seguinte:

| Ponto | Operador e significado |
|---|---|
| `deslocamentos_na_cadeia` | Associa coluna a instante pela chave `(tipo, HH:MM)` e converte para minutos com `// 60`. O `dt <= prev` de `encadear_marcos` avança a data quando dois marcos empatam ou o seguinte vem antes. É comparação entre marcos para encadear o turno. `app/escala/utils.py:330–350,366–384`. |
| `_marcar_ausencias` | `!= 'sem_periodo'` habilita o exame dos marcos e `== 'dentro'` lê a resposta. A exclusão das duas bordas vem do `<` dentro do juiz. `app/escala/utils.py:1245–1261`, `app/ponto/turnos.py:2662`. |
| Materialização, leitor da célula, supra-juiz e ata | A materialização usa `== 'sem_periodo'`. O leitor e a ata copiam os carimbos. O supra-juiz compara o rótulo com `sem_periodo` e `fora`. Nenhum desses operadores compara horas. `app/chamados/services/materializacao.py:82–86`, `app/escala/services/leitor_celula.py:424–436`, `app/ponto/supra_juiz.py:392–418,450–463`, `app/ponto/services/cartorio.py:784–789`. |
| Cartório, lista e impressão | `== 'sem_periodo'` controla a lista. A impressão usa o rótulo e os índices de dispensa. `__gt`/`__lte` testa a janela de **carimbos de alteração** `(desde, ate]`, com início exclusivo e fim inclusivo. `data__lte`/`data_fim__gte` recortam **datas** de ausência, com bordas inclusivas. `app/ponto/services/cartorio.py:393–406,416–446,473–489`, `app/ponto/turnos.py:2782–2787`. |
| Validação do cadastro | `hora_fim == hora_ini` recusa pontas iguais. O `fim < inicio` na documentação da porta descreve a noturna aceita. A normalização efetiva está no juiz. `app/ponto/services/ausencia.py:1039–1054`, `app/ponto/turnos.py:2655–2660`. |

Na suíte há uma comparação independente, com **as duas bordas inclusivas**. `test_RED_periodo_fora_o_dia_concorde_sai_da_retencao` monta `_dentro` com `'14:00' <= m['hora'] <= '18:00'` e verifica se algum desses marcos está faltando. `app/escala/tests/test_cobertura_parcial_periodo.py:289–305`.

## 4. ESMERIL-SEMÂNTICA

- **Cadastro e persistência.** `hora_ini` e `hora_fim` são `TimeField` anuláveis. As duas entradas do DP fazem `HH:MM → time` e passam pela mesma porta. Ela valida o par e grava as horas de relógio. A trilha formata `%H:%M`, o painel exibe `H:i` e o insumo da folha lê os valores armazenados para detectar mudança. A conversão para minuto do dia e o `+1440` pertencem ao juiz. `app/ponto/models.py:765–771`, `app/ponto/views.py:2797–2815,2836–2849`, `app/ponto/services/ausencia.py:119–139,1039–1054,1068–1072,1099–1110`, `app/templates/ponto/partials/painel_ausencia.html:313–329`, `app/ponto/turnos.py:2655–2660`.
- **Batida real.** Inteiro em `horas` já é deslocamento desde 00:00 do dia de início. Para batida casada com deslocamento e `delta`, vale `marco + delta`. O `delta` é arredondado a partir de segundos. Para órfã, ou célula sem esse deslocamento, vale `HH:MM` como minuto do próprio dia. Por isso `01:10` vale `70` como string e `1510` quando entregue como minuto de D+1. `app/ponto/turnos.py:2622–2630,2648–2654,2667–2688`, `app/escala/utils.py:314–327,366–384`.
- **Marco previsto.** O segundo uso de `horas` passa um instante previsto em minutos na mesma régua. A cadeia de marcos avança um dia com `dt <= prev`, inclusive em horários iguais. A ausência avança **só o fim** e só com `fim < ini`. Assim, `00:30–02:00` é avaliado no próprio D, enquanto `23:00–02:00` alcança D+1. Sem deslocamento da coluna, a grade não entra no julgamento de dispensa daquele marco. O comentário de `escala/utils.py:1249–1250` ainda diz “bordas inclusivas”, mas a chamada executa `<` nas duas pontas. `app/escala/utils.py:342–350,366–384,1249–1261`, `app/ponto/turnos.py:2655–2664`.
- **Cadastro existente e duração.** Com `()`, a materialização só distingue par ausente de par presente. Já `Ausencia.minutos` é duração. Junto com o tipo e o recorte de um dia, ele qualifica a ausência como parcial pelo teste `0 < minutos < 480`. `cobertura_parcial` carimba essa classificação. `periodo_parcial` carimba o resultado sobre as batidas. `app/chamados/services/materializacao.py:82–86`, `app/ponto/models.py:754–760`, `app/ponto/turnos.py:2599–2619,2655–2664`, `app/escala/utils.py:1198–1202,1241–1245`.

## 5. Resultados e testes afetados [nao verificado]

**(a) Batida real.** Quando as únicas horas conhecidas que o juiz antigo aceitava estão na borda, `dentro` passa a `fora`, ou a `nao_sei` se houver hora ilegível. Uma hora estritamente interna ainda produz `dentro`. Nos ramos de batida do supra-juiz, `fora` pode retirar `FATO_EM_AUSENCIA` e deixar o resto faltante em julgamento. `nao_sei` ainda pode produzir `FATO_EM_AUSENCIA`. O cenário de col306 na suíte espera `fora` para a saída às `15:54` e a retirada daquele código. `app/ponto/turnos.py:2655–2664`, `app/ponto/supra_juiz.py:392–418,450–463`, `app/ponto/tests/test_parcial_borda_semiaberta.py:103–131`.

Na comparação em memória das duas versões, **nenhum teste puro de batida passou a falhar**. No sentido inverso, os seis `test_RED_*` de `BordaSemiabertaJuizPuroTest` e os testes `test_dentro_e_fora_com_bordas_semiabertas` e `test_RED_noturna_o_fim_menor_cruza_a_meia_noite_e_e_do_dia_de_inicio` falharam com `<=` e passaram com `<`. O resultado da execução Django de `test_RED_debora_saida_na_borda_o_dia_nao_e_FATO_EM_AUSENCIA` está **[nao verificado]**. `app/ponto/tests/test_parcial_borda_semiaberta.py:43–78,122–126`, `app/escala/tests/test_cobertura_parcial_periodo.py:44–61`.

**(b) Marco previsto.** Um marco `missing` exatamente no início ou no fim perde a dispensa. Isso pode aumentar `n_missing`, mudar a lista e a âncora dos marcos cobrados e mudar a impressão mesmo quando `periodo_parcial` continua `fora`. A pergunta de cadastro com `()` produz os mesmos resultados nas duas versões. `app/escala/utils.py:1253–1261`, `app/ponto/services/cartorio.py:482–486,753–765,1825–1841,1934–1949`, `app/chamados/services/materializacao.py:82–86`, `app/ponto/turnos.py:2655–2664`.

| Verificação | Teste e expectativa afetada | [nao verificado]
|---|---|
| Confirmado em memória | `test_RED_noturna_a_saida_da_madrugada_dentro_de_23_07_e_dispensada` espera dispensa de `S 07:00` na borda final em D+1. `app/ponto/tests/test_cobparc_marco_no_periodo.py:64–78`. |
| Pela leitura da asserção | `test_RED_marcos_DENTRO_do_periodo_saem_dispensados_e_o_dia_nao_acusa` espera dispensar `E 13:00` e `S 17:00`. `test_RED_marco_FORA_do_periodo_segue_cobrado` espera só `E 13:00` apagada. `app/ponto/tests/test_cobparc_marco_no_periodo.py:174–194`. |
| Pela leitura da asserção | `test_RED_dia_parcial_sem_batida_o_resto_e_FURO_SEM_COBRANCA_nomeando_so_os_marcos_de_fora`, `test_RED_marcos_faltantes_do_dia_parcial_sao_os_de_fora` e `test_RED_vivo_do_dia_parcial_espelha_so_os_marcos_de_fora` esperam listas restritas a `08:00` e `12:00`. `app/ponto/tests/test_cobparc_marco_no_periodo.py:322–336,411–423,445–458`. |
| Pela leitura da asserção | `test_RED_mudar_o_periodo_rejulga_a_celula` espera `n_missing` de `0` para `1` ao editar `13–17` para `14–17`. Com as duas bordas excluídas, a expectativa inicial já é afetada. `app/ponto/tests/test_cobparc_marco_no_periodo.py:361–381`. |
| Pela leitura da asserção | `test_RED_E03_e_S07_de_D1_dentro_de_23_07_saem_dispensados` espera dispensar `S 07:00`. `test_RED_dia_parcial_aparece_com_o_resto_furado` espera só `E 08:00` e `S 12:00` no relatório. `app/ponto/tests/test_cobparc_v4_procedentes.py:83–100,277–295`. |
| Pela leitura da asserção | `test_RED_previstos_do_furo_diario_batem_o_lavrado` espera uma dispensa e `(3, 1)` de previstos/furados. `test_RED_marco_dispensado_nao_e_batido` espera uma dispensa e `(2, 3)` de batidos/previstos. Ambos usam `14–17` com `S 17:00` faltante. `app/escala/tests/test_cobparc_rotulo_furos_diarios.py:53–72`, `app/relatorios/tests/test_cobparc_rotulo_dispensada.py:71–89`. |

O teste de retenção com o filtro inclusivo também fica sensível ao cruzamento se `S 14:00` chegar `missing`. O resultado desse teste depende do casamento das batidas da fixture e está **[nao verificado]**. A lista acima distingue o teste puro executado em memória das regressões identificadas por leitura. A execução completa da suíte Django está **[nao verificado]**. `app/escala/tests/test_cobertura_parcial_periodo.py:101–118,289–305`, `app/escala/utils.py:595–629,635–676`.

## 6. Rejulgamento de uma ata antiga com `dentro`

1. **Cron.** A configuração agenda `processar_cartorio --apply` às **06:27**, por empresa, sem `--forcar`. O comando seleciona todas as células com `data < hoje` da empresa e chama `julgar_colab`. O cartório monta a grade com as batidas apuráveis e calcula a impressão nova antes de decidir se pula a célula. `app/config/crons.py:616`, `app/ponto/management/commands/processar_cartorio.py:25–29,45–65`, `app/ponto/services/cartorio.py:1184–1207,1426–1446`.
2. **Condição para relavrar.** Se o carimbo antigo codificou `dentro` e a grade agora dá `fora` ou `nao_sei`, muda o insumo da impressão. Também muda se a lista de índices dispensados mudar, ainda que o agregado fique `dentro`. Com impressão divergente, o cartório classifica e grava a ata nova em `--apply`, sem precisar de `--forcar` ou de alteração em `REGRA_VERSOES`. Com impressão igual, julgamento e veredito válidos acionam o pulo normal. `--forcar` contorna esse pulo. Se houver veredito de admin, a porta pode atualizar a ata preservando esse veredito. `app/ponto/services/cartorio.py:160,473–486,550–554,574,1285–1294,1426–1446,1499–1511,1898–1923`, `app/ponto/portas/celula.py:1085–1104,1218–1239`.
3. **Sinais.** Save/delete de batida ou ausência, save de justificativa e criação ou mudança de status de chamado também chegam a `julgar_celula`, com `forcar=False` por padrão. Batida chama o dia local e a véspera. Ausência chama as datas derivadas da cobertura. A porta exige célula existente, recusa dia futuro e, para dia de trabalho corrente sem teto explícito, espera o fim do turno. A edição do período pelo DP salva os campos e aciona esse caminho. O `vigia_de_hora` usa a mesma porta para candidatos de hoje e de ontem com `teto_de_hoje=True`. `app/ponto/signals.py:85–93,108–165`, `app/chamados/signals.py:189–218`, `app/ponto/services/ausencia.py:1099–1104`, `app/ponto/services/cartorio.py:2059–2090`, `app/config/crons.py:367`, `app/ponto/services/vigia_de_hora.py:162–182`.
4. **Competência e emissão.** A seleção do cron, a porta avulsa e o pulo por impressão permitem a relavra da **ata** em competência exportada. `GUARDA_COMPETENCIA` está cadastrada para 09/2026 e 10/2026 e barra **escrita de fechamento e de `DiaPago`** nas empresas que exportaram a competência, segundo o juiz de exportação vigente. A emissão automática tem piso próprio. Se a mudança deixar resto cobrável e não houver fato novo desde o julgamento anterior, `parcial_so_lista` encaminha o dia à lista do admin sem novo chamado, pergunta ou push. `app/ponto/management/commands/processar_cartorio.py:49–65`, `app/ponto/services/cartorio.py:316,356–372,375–446,1437–1446,1775,1805–1824,1861–1879,2059–2090`, `app/ponto/services/fechamento.py:36–52,548–573,1509–1550`, `app/ponto/services/dia_pago.py:446–460`, `app/folha/export.py:556–574`.

A impressão é o critério de reentrada do cartório. A leitura da ata pelo `leitor_celula` continua devolvendo o valor lavrado até uma nova gravação. O anexo fornecido não traz o carimbo da célula, a competência efetiva nem o resultado de uma passada após o deploy. Esses fatos de produção ficam **[nao verificado]**. `app/ponto/services/cartorio.py:1426–1446,1898–1923`, `app/escala/services/leitor_celula.py:429–451`.

## 25/11 20:17 · sessao 25c43a3f129b · WRAPPER-N-VAGAS · verificador · origem=Code

**REPROVA — WRAPPER-N-VAGAS, volta 13.** Há um caminho para `0/OK` após a perda da lease, a recusa de inventário pode mandar remover uma worktree já reutilizada, e o selo declara um mutante que o aplicador nunca consegue aplicar. `bin/codex_pergunta.sh:404–406, 563–576, 802–815` · `bin/tests/test_codex_n_vagas.sh:1115, 1148–1155` · `bin/codex_admissao.py:294–295`

A revisão foi feita em leitura, pela cópia de `bin/` e pelo diff fornecido. Identidade Git do hash e da merge-base, execução integral dos selos e ocorrências em produção: [nao verificado].

## 1. Escritores, leitores e significado

| Interface | Escrita, leitura e significado |
|---|---|
| `vaga.<k>.json` | O wrapper gera `id_chamada` com obra, data/hora e PID do wrapper. `_PROJ_PY abre` grava o ID, `abriu` e quatro nulos em `iniciou`, `fechou`, `rc` e `motivo`. `inicia` e `fecha` leem o ID para conferir o dono. `inicia` confere os hashes dos insumos e carimba a autorização M6, antes do `GO`. `fecha` grava hora local, rc inteiro e motivo final calculado pelo trap. O relógio da projeção é lido antes das verificações e do `replace`. `bin/codex_pergunta.sh:241–245, 259–305, 545–551, 765–784` |
| `historico/<id>.json` | O próximo `abre`, sob a meta, lê a projeção anterior, chama `codex_admissao.id_legivel` e arquiva o texto anterior antes de publicar o novo JSON. Se o histórico já existe, compara o conteúdo e recusa divergência. O selo lê o histórico e confere o ID com o `.rc`. O portão de produção usa os flocks e a resposta do titular. `bin/codex_pergunta.sh:267–291, 515–546` · `bin/codex_admissao.py:53–58, 238–268` · `bin/tests/test_codex_n_vagas.sh:585–603` |
| `logs/codex/vagas.log` | `_LOG_PY` faz append de `hora<TAB>obra<TAB>pid_wrapper<TAB>k<TAB>evento`. `abriu` é gravado na M4 sob a meta, após a admissão. `fechou` sai do trap, antes da prova de vida do fecho e, no caminho normal, antes de retomar a meta. Ambos podem ser gravados depois da morte do titular. `codex_vagas_estado.ler` casa por `(pid,k)` e conta aberturas e pares faltantes em 24 h. `gerar_estado` delega ao leitor. `bin/codex_pergunta.sh:313–324, 379–411, 545–550` · `bin/codex_vagas_estado.py:18–53` · `bin/gerar_estado.py:279–285` |
| `<resposta>.rc` e `chamadas.log` | `_saida` escreve `fim` após `INI=1`, com rc no campo 8, duração em segundos desde `T0C` no 9 e motivo no 10 apenas para construtor. O `.rc` exige também `_LK7=1` e leva `chamada`, `rc` e hora de fim. O ID vem do basename do JSONL. `codex_limite_estado` lê os campos até o 9 e seus consumidores usam a mesma leitura no ESTADO, no CODEX-ESTADO e no disjuntor. Os selos leem `.rc` e campo 10. `bin/codex_pergunta.sh:555–575, 604–605, 730–736` · `bin/codex_limite_estado.py:49–96` · `bin/gerar_estado.py:275–278` · `bin/gerar_codex_estado.py:176` · `bin/disjuntor.py:2137, 2227` · `bin/tests/test_codex_n_vagas.sh:573–599, 996–998` |
| `CODEX_RESULTADO` | `_resultado` escreve em stderr a partir de `_motivo`, a mesma função usada no registro e no campo 10. Os selos conferem a última linha. A prova real compara o rc no prefixo, mas a divergência do marcador apenas imprime `INCONCLUSIVO`, sem alterar seu veredito. Erros de argumento, hash, nome reservado e outras saídas antes da instalação do trap ficam sem marcador. Consumidor operacional externo: [nao verificado]. `bin/codex_pergunta.sh:86–136, 333–339, 400–413, 565–576` · `bin/tests/test_codex_n_vagas.sh:211–224` · `bin/tests/prova_codex_pergunta_real.sh:85–97` |

As diferenças que importam são estas. `vivas` conta ocupação por flock na abertura, incluindo reserva antes do executor e a vaga legada. `iniciou` é a autorização. `fechou` no log registra a passagem pelo trap, inclusive sem titular. O leitor do log de vagas preserva essa distinção, mas o leitor de `chamadas.log` exibe `em curso` quando falta `fim`, inclusive se a chamada morreu sem executar o trap. Além disso, o leitor de vagas aceita datas futuras na janela e uma reutilização do mesmo `(pid,k)` pode apagar da contagem uma abertura antiga sem par. `bin/codex_admissao.py:238–268` · `bin/codex_pergunta.sh:379–411, 745–784` · `bin/codex_vagas_estado.py:42–60` · `bin/codex_limite_estado.py:82–88, 239–243`

| Mensagem | Escritor → leitor |
|---|---|
| `VAGA` | Titular, após escolher k e travar o diretório da wt → wrapper, que extrai `k`, `vivas`, `ocupadas` e `legado`. A mesma leitura inicial atende `SEM_VAGA`, `MESMA_WT` e `ERRO`. `bin/codex_admissao.py:223–268` · `bin/codex_pergunta.sh:501–516` |
| `CONFERE <N>` / `OK outras=<n>` / `NAO <motivo>` | Wrapper → titular, que confere N, inodes e quantidade de leases → wrapper, que exige `OK` na M6. `bin/codex_pergunta.sh:748–757` · `bin/codex_admissao.py:193–215, 269–276` |
| `VIVO` / `SIM` | Wrapper → titular → wrapper. O titular responde antes do GO, durante o executor e na espera após o RC. O wrapper pula `PID`, `RC` e `ERRO` enquanto procura `SIM`. `bin/codex_pergunta.sh:359–365, 400–405, 802–811` · `bin/codex_admissao.py:147–175, 269–276` |
| `GO <kill_s> <arquivo>` / `PID` / `RC` / `ERRO` | Wrapper → titular. `kill_s` é a janela em segundos da escalada ao matar o grupo. O arquivo contém stdin, stdout, stderr e argv separados por NUL. O titular lança `timeout … codex`, transmite PID e rc bruto, com `128+sinal` para término por sinal observado no filho. O wrapper ignora `PID`, lê `RC` ou termina a leitura em `ERRO`, e usa rc 1 se faltar rc numérico. `bin/codex_pergunta.sh:773–787` · `bin/codex_admissao.py:134–144, 279–305` |
| `[nome]` / EOF | O trap escreve `[nome]` se marcou GO e ainda aguarda resposta. O titular manda TERM e depois KILL ao grupo e colhe o líder. Após o RC, `[nome]` repete o RC. EOF antes do GO libera as leases do titular, durante a execução entra no caminho de matar, e após o RC encerra `_segura`. O wrapper fecha seu descritor de escrita no fim. `bin/codex_pergunta.sh:367–373, 409–413, 574` · `bin/codex_admissao.py:125–144, 147–175, 271–278, 300–306` |

## 2. Teto, mesma wt e ordem das mortes

- **N fixo, para os executores construtores lançados pelo titular.** Cada lançamento carrega os descritores da vaga, da wt e da trava compartilhada do legado. Dois titulares/executores que conservam esses descritores disputam os mesmos inodes. O legado com `EX` impede uma nova admissão, e o `SH` de qualquer k impede o legado de entrar, inclusive após a morte do titular se o executor ainda conserva o fd. A conservação pelo Codex real e por todos os descendentes é [nao verificado]. `bin/codex_admissao.py:238–268, 293–305` · `bin/codex_pergunta.sh:516–534`
- **N corrente pode ser excedido.** Com A viva e B autorizada em N=2, `main` pode mudar para 1 depois do `CONFERE` de B e antes de B enviar GO. B ainda lança. A drenagem permitida pelo comentário começa na autorização, de modo que essa janela inclui uma chamada cujo executor ainda vai nascer. `bin/codex_pergunta.sh:745–757, 765–785` · `bin/codex_admissao.py:271–295`
- **SIGKILL antes do `CONFERE`.** Sem titular, o wrapper pode continuar no inventário inicial e publicar `abriu` após ter recebido `VAGA`. A meta serializa as admissões novas nessa fase, mas o legado pode adquirir `EX` quando o `SH` cai. Na M6, pipe quebrado ou falta de `OK` produz `2/REVALIDACAO`. Se o coproc já sumiu na captura dos fds, o ramo explícito produz `2/PRE_CHECK`. `bin/codex_pergunta.sh:490–510, 535–550, 748–757` · `bin/codex_admissao.py:238–268`
- **SIGKILL entre `CONFERE` e `GO`.** Durante o inventário M6, a meta ainda bloqueia outro wrapper, mas a lease de wt e o `SH` podem já ter caído. O legado pode entrar. O wrapper ainda pode publicar `iniciou`, e o GO ao pipe sem leitor recusa com `3/LEASE_PERDIDA`. Se a morte vier depois do GO aceito, a proteção passa a depender de o executor ter recebido e conservado os fds. `bin/codex_pergunta.sh:755–785` · `bin/codex_admissao.py:238–268, 279–305`
- **SIGKILL com executor vivo ou depois do RC.** Se o titular morre durante a execução, o executor pode continuar com as leases, enquanto o wrapper recebe EOF e começa o inventário mesmo sem RC. Depois do RC e da saída de quem herdou os fds, matar o titular libera vaga, wt e legado. Outra chamada pode então entrar na mesma wt enquanto A ainda faz inventário. O `VIVO` posterior altera o resultado de A, mas o inventário já aconteceu. `bin/codex_admissao.py:293–305` · `bin/codex_pergunta.sh:783–815`
- **TERM/INT/HUP no wrapper.** Depois de instalados os traps, a intenção é `143/130/129 SINAL`. Antes de GO o trap segue para o fecho sem lançar. Com GO pendente, tenta `[nome]` e espera RC/ERRO. Depois de ler RC/ERRO, pula `[nome]`. Com titular morto, escrita quebrada ou espera vencida, segue para `fechou` sem prova de que o executor parou. Há também uma janela de sinal entre fechar `_MFD` e zerar `_META_PRESA`, tanto em M4 quanto em M6. O trap pode então escolher o ramo que presume a meta presa. `bin/codex_pergunta.sh:349–373, 398–413, 550, 771, 783–787`
- **`abre` arquivou e morreu antes de publicar.** Se a projeção e o histórico ainda têm os mesmos bytes, a próxima abertura aceita o reuso. Se um fecho atrasado mudar a projeção nesse intervalo, a próxima abertura recusa `FALHA_TRILHA`. A prova no fecho cobre o titular já morto antes do `VIVO`. A janela de sinal acima deixa um caminho sem meta: A recebe `SIM` no fecho, o titular morre, B arquiva A e morre antes de publicar, e A grava o fecho sobre a projeção que ainda tem seu ID. O histórico passa a divergir. `bin/codex_pergunta.sh:261–285, 301–305, 398–406, 546–550, 771`

## 3. `0/OK` e ordem de remoção

**Sim, nos dois casos.**

1. A terminou o executor e recebeu o último `SIM`, já no fecho sob a meta. O titular morre antes do `fecha`. O Python confere o ID e grava `fechou`, `rc=0` e `motivo=OK` sem verificar de novo a lease. A meta protege a projeção de outro `abre` naquele instante. O trap depois publica `.rc`, `fim` e `0/OK`. `bin/codex_pergunta.sh:261–266, 301–305, 398–406, 563–576`
2. A tem uma falha transitória de inventário numa wt limpa e recebe `SIM` no primeiro teste. O titular morre antes de A escrever `.recusa`. B pode entrar na wt, enquanto A grava a instrução de `worktree remove --force`. O teste do fecho pode trocar o motivo final para `LEASE_PERDIDA`, mas deixa o texto de remoção gravado. Quem executa essa remoção em produção é [nao verificado]. `bin/codex_pergunta.sh:174–187, 214–216, 382–406, 802–822`

O `.id` também é publicado antes da decisão final do trap. Assim, `fecho_sem_titular` pode resultar em `.id` de uma chamada que termina com rc 3. A triagem de `bin/` exige papel `verificador`. Aceitação desse `.id` de construtor por outro consumidor: [nao verificado]. `bin/codex_pergunta.sh:563–576, 845–853` · `bin/disjuntor.py:897–904`

## 4. Pipe sem leitor

Os quatro writes do wrapper ao titular têm rc conferido: `VIVO` devolve falha, `CONFERE` recusa, `GO` recusa e `[nome]` abandona a tentativa. O problema de `[nome]` está na consequência: falha de escrita e ausência de RC fazem `_mata_exec` voltar sucesso ao trap, que grava `fechou` mesmo assim. No sentido inverso, `_diz("PID")` pode sofrer EPIPE depois de `Popen` e antes de `_executa`, deixando o executor vivo sem o titular que trataria EOF. `bin/codex_pergunta.sh:361–373, 411, 755, 784` · `bin/codex_admissao.py:102–106, 293–305`

`trap ':' PIPE` instala um tratador no Bash, sem pôr `SIGPIPE` em `SIG_IGN` nos filhos executados. Git, Python, timeout e Codex conservam o tratamento que teriam ao iniciar. O titular também usa `Popen` sem desativar a restauração padrão de sinais. A disposição efetiva do Codex real em execução é [nao verificado]. `bin/codex_pergunta.sh:352–354, 501, 522–535, 775–781` · `bin/codex_admissao.py:293–295`

## 5. `except` do diff e falhas caladas

| Captura | Veredito |
|---|---|
| `BlockingIOError` em `_tenta` | Estreita e declarada. Só converte contenção em `False`. `bin/codex_admissao.py:92–99` |
| `(ValueError, OSError)` na abertura | Limitada à abertura e validação dos objetos. Declara `ERRO lease nao abriu` com o detalhe. Outros erros de sondagem ficam fora desse bloco. `bin/codex_admissao.py:228–245` |
| `(ValueError, OSError)` ao lançar | Limitada ao arquivo do GO, canais e `Popen`, incluindo o fechamento dos arquivos pelos context managers. Declara `ERRO`, mas manda só a classe, omitindo operação, caminho e errno. O wrapper consome e descarta o texto; o diagnóstico visível é o genérico de rc ausente. `bin/codex_admissao.py:285–299` · `bin/codex_pergunta.sh:785–787` |
| `ValueError` no leitor | Estreita ao parse da data. Incrementa `ilegiveis`, publicado na frase. `bin/codex_vagas_estado.py:36–41, 63–64` |

O `except Exception` do inventário aparece na cópia, fora das adições do diff fornecido. É amplo e emite `FALHA` com rc 1. Fora das capturas, `fim` pode falhar sem alterar o rc final, o `.rc` pode falhar sem o trap promovê-lo a erro, e `pausar --soltar` pode imprimir sucesso após falha do `rm`. O portão também decide pelo texto `STATUS 200` sem usar o status do comando que o produziu. `bin/codex_pergunta.sh:184–216, 530–531, 563–576` · `bin/pausar.sh:45–52`

## 6. Classes de reprovação

| Classe | Achado |
|---|---|
| Segundo juiz | O inventário continua com três consumidores do mesmo corpo. O teto 2 está em `n_fora`, mas o formato de N `1..3` é julgado também duas vezes no Bash. Duplicação de juiz registrado do anexo: [nao verificado]. `bin/codex_pergunta.sh:425, 538, 754, 758, 802` · `bin/codex_admissao.py:61–67, 200–201, 223–226` |
| Segundo escritor fora da porta | A reserva de `$R/logs/codex/vagas.log` depende de `VAGA` não vazia. Um papel de leitura com common-dir separado passa sem essa reserva e pode publicar uma resposta nesse caminho. Em `pausar.sh`, duas publicações também escrevem o mesmo `$f.novo` antes de tomar a meta. `bin/codex_pergunta.sh:103–112, 414–418, 788–795` · `bin/pausar.sh:63–74` |
| Relógio fora da borda | O leitor de vagas recebe `agora` opcional, mas o gerador omite esse argumento e o leitor consulta `now()` internamente. O prazo de gate/slot usa relógio de parede e só julga tentativas malsucedidas. Uma aquisição tardia bem-sucedida passa sem conferir o prazo na borda. `bin/gerar_estado.py:283–285, 412–416` · `bin/codex_vagas_estado.py:22–24` · `bin/codex_pergunta.sh:665–700` |
| Derivação paralela | `vagas.log` casa por `(pid,k)` e `chamadas.log` por `(obra,hash,pid)`. O primeiro publica falta de par e o segundo deriva `em curso` da falta de `fim`. `_motivo`, por sua vez, é compartilhado pelos três emissores finais. `bin/codex_vagas_estado.py:42–53` · `bin/codex_limite_estado.py:63–67, 82–88` · `bin/codex_pergunta.sh:333–339, 400–405, 565` |
| Selo ou RED sem mordida | Há mutante inaplicável, mutante de sessão que mede apenas término por grupo e falsa prova de prazo por ausência de medida. Detalhes abaixo. `bin/tests/test_codex_n_vagas.sh:986–1031, 1115–1120, 1148–1158` |
| Ordem de trava | A ordem normal solta a meta antes de esperar gate/slot/resposta e só a retoma na M6 ou no fecho. A janela de sinal entre fechar o fd e zerar o indicador permite que o fecho acredite que ainda a segura. `bin/codex_pergunta.sh:398–406, 550, 670–730, 748–771` |
| Pouso antes do pré-requisito | A resposta pública é movida antes do inventário e o `.id` precede a prova do fecho. O prompt também é montado antes da M6: conferir o hash do caminho depois não garante que aqueles bytes foram os lidos para o prompt, caso o conteúdo mude e volte no intervalo. `bin/codex_pergunta.sh:286–300, 619–655, 765, 793–815, 845–853` |
| Campo com dois significados | `abriu` e `fechou` têm instantes diferentes no JSON e na trilha. O `RC` do pipe é bruto, e o `rc` final é normalizado pelo wrapper. O desvio semântico no leitor é `em curso` também significar ausência de `fim` após queda. `bin/codex_pergunta.sh:259–305, 379–411, 545–549, 785–787, 822–841` · `bin/codex_limite_estado.py:82–88` |

## 7–8. Selos e mutantes

| Caso | O que prova e onde pode errar |
|---|---|
| `gemea_n2` | Prova vaga 2 presa e vaga 1 livre em uma amostra. Após `kill -9`, o laço pode esgotar e o caso prossegue sem exigir que wrapper e titular morreram. O `SH` ainda no titular pode dar verde a uma falha de herança do executor. `bin/tests/test_codex_n_vagas.sh:877–892` |
| `mesma_wt_morte` | B roda em primeiro plano até sair, enquanto A está parada no `sha256sum` que vem **depois** da varredura. Cobra `LEASE_PERDIDA`, proibição de remover e registro de B intacto. Pode ficar verde sem exclusão entre inventário e executor vivo. `bin/tests/test_codex_n_vagas.sh:131–136, 894–920` · `bin/codex_pergunta.sh:802–806` |
| `vaga_inutil`, `fecho_sem_titular` | O primeiro fabrica o histórico igual à projeção e cobra reuso. O segundo mata no `sha256sum` do `.id`, antes do último `VIVO`. Mordem a ausência de prova nos pontos montados. Nenhum mata o titular depois do último `SIM`. `bin/tests/test_codex_n_vagas.sh:923–947` · `bin/codex_pergunta.sh:400–405, 847–850` |
| `go_morto`, `sigpipe` | `go_morto` sinaliza o PID e solta o `inicia` sem aguardar morte ou EOF, então sob carga pode cair no diagnóstico posterior e dar vermelho falso. `sigpipe` realmente cobra `2/REVALIDACAO` e projeção sem `fechou` na ramificação com meta presa. Se o Bash colher o coproc antes da duplicação dos fds, o caso cai em `PRE_CHECK` e falha pela fase errada. `bin/tests/test_codex_n_vagas.sh:950–983` · `bin/codex_pergunta.sh:504–510, 755–757` |
| `sigterm` | Cobra `143/SINAL`, os artefatos finais e o fake morto quando o wrapper já saiu. Pode passar se `fechou` foi gravado cedo e o executor morreu antes da observação. Só envia TERM direto ao wrapper. `bin/tests/test_codex_n_vagas.sh:986–1000` |
| `prazo_espera` | Cobra `75/ESPERA_SLOT`, ausência de executor e lease solta. Aceita recusa imediata, porque não mede os três segundos e não prova o `flock` do selo. Também não cobre sucesso tardio de gate ou slot. `bin/tests/test_codex_n_vagas.sh:1024–1032` · `bin/codex_pergunta.sh:676–700` |
| `vaga_reservada/vr` | Exige rc, diagnóstico, ausência de Codex e preservação do inode e conteúdo da trilha da raiz do script. Morde no cenário montado. Não cobre `VAGA` vazia, e o mutante `#G` pode ser dado por morto pelo primeiro subcaso sem provar `vr`. `bin/tests/test_codex_pergunta.sh:147–152, 338–352, 630, 714–719` · `bin/codex_pergunta.sh:103–111` |
| `coproc_morto` | O titular que sai 0 e o atraso de 0,5 s exercitam a guarda se a coleta ocorreu. Sob carga o titular pode ainda não ter saído. O código também usa `${ADM[0]}` e `${ADM[1]}` sem default no `exec`, após os testes com default. O selo não cobre a coleta entre os testes e essas expansões. `bin/tests/test_codex_n_vagas.sh:1003–1021` · `bin/codex_pergunta.sh:501–505` |
| `fecho_meta(b)` e `teto` | O controle (b) agora exige contenção amostrada, mas os 2 s podem acabar antes de o wrapper chegar ao fecho e ainda deixar o controle verde. Em (a), carga pode colocar o fecho depois dos 8 s do bloqueador. `teto` confere o `exec 6>>`, o `flock`, um `abriu` novo e a mudança do blob antes de liberar a resposta. O subcaso ainda depende de B liberar a vaga dentro da espera e de D chegar dentro dos limites do selo. `bin/tests/test_codex_n_vagas.sh:274–291, 623–648, 701–710` |

**A tabela `#V` não morde toda pelo motivo esperado.** Conferi as 84 substituições em memória pelo mesmo critério de linha do aplicador. Entre as aplicáveis, nenhuma tinha duas ocorrências de `de` na mesma linha marcada. `slot_abre`, `provisao` e o mutante genérico de `janela` atingem múltiplas linhas marcadas de uma vez. `adm:pass_fds` tem **zero** correspondências: o `de` está na linha 294 e o `#V:pass_fds` na 295. O selo deve acusar “nao aplicado” e falhar também na contagem. `bin/tests/test_codex_n_vagas.sh:1048–1131, 1148–1155, 1164–1171` · `bin/codex_admissao.py:294–295` · `bin/codex_pergunta.sh:484–486, 527–529, 688–703`

- `vivo_fecha_presa` e `vivo_fecha_mf` substituem a chamada certa. Nos cenários montados, o primeiro permite fechar o registro que `sigpipe_registro` exige aberto, e o segundo altera a projeção que `vaga_inutil_fecho` compara. `coproc` substitui o primeiro teste certo e, co

[truncado em 20000 de 22160 caracteres]

## 25/11 19:57 · sessao 12e9dc2dcbcb · PARCIAL-BORDA-SEMIABERTA · leitura · origem=Code

O veredito gravado é **`fato_em_ausencia`**. Na sonda de col306, ausência `5775`, em `24/11/2026`, o juiz real retorna `dentro`. A única batida que satisfaz a comparação inclusiva é `15:54`, igual ao início do período. Resultado da sonda: **só na borda inicial**. O total da frota em PROD é [nao verificado]. `aval.txt:1` (anexo fornecido), [ponto/turnos.py:2651](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/ponto/turnos.py:2651), [escala/models.py:1630](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/escala/models.py:1630).

### Onde nasce, quem grava e quem lê

- **Chamada.** `escala.utils._marcar_ausencias` seleciona a ausência aprovada pela cobertura `para='folha'`, com desempate pelo maior `(data, pk)`, e consulta `cobre_parte_do_dia`. Para a parcial, chama `periodo_da_parcial_no_dia(_aus_por_dia[d], horas_reais_do_dia(dia, deslocamentos.get(d)))`. O segundo argumento traz as batidas casadas e órfãs em minutos, com os deslocamentos da cadeia do turno para a madrugada. A comparação `ini <= m <= fim` está na linha 2658. A outra chamada da grade verifica marcos previstos para dispensa. `chamados.services.materializacao` também chama o juiz, com `()`, para verificar `sem_periodo`. [escala/utils.py:1198](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/escala/utils.py:1198), [escala/utils.py:1245](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/escala/utils.py:1245), [escala/utils.py:1260](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/escala/utils.py:1260), [escala/utils.py:1763](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/escala/utils.py:1763), [ponto/turnos.py:2533](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/ponto/turnos.py:2533), [ponto/turnos.py:2658](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/ponto/turnos.py:2658), [ponto/turnos.py:2663](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/ponto/turnos.py:2663), [ponto/turnos.py:2773](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/ponto/turnos.py:2773), [chamados/services/materializacao.py:86](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/chamados/services/materializacao.py:86).
- **Veredito e escritor.** O cartório chama `classificar_dia`. O suprajuiz lê `periodo_parcial` e pode emitir `FATO_EM_AUSENCIA`, inclusive no ramo de batida sem par. `derivar_veredito` mapeia o código para `fato_em_ausencia` pela ordem de precedência. O cartório copia `cobertura_parcial` e `periodo_parcial` para a ata, chama `CelulaDia.lavrar_veredito` e persiste pela porta `carimbar`. [ponto/services/cartorio.py:1499](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/ponto/services/cartorio.py:1499), [ponto/supra_juiz.py:392](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/ponto/supra_juiz.py:392), [ponto/supra_juiz.py:450](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/ponto/supra_juiz.py:450), [escala/models.py:1622](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/escala/models.py:1622), [ponto/services/cartorio.py:784](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/ponto/services/cartorio.py:784), [ponto/services/cartorio.py:1901](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/ponto/services/cartorio.py:1901), [ponto/services/cartorio.py:1920](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/ponto/services/cartorio.py:1920), [ponto/portas/celula.py:1231](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/ponto/portas/celula.py:1231).
- **Leitores.** `grade_da_celula` repassa o veredito e o período lavrado. `parcial_so_lista` lê a cobertura e o período da ata. A tarefa do dia, o relatório da frota e a retenção da folha consomem o veredito. [escala/services/leitor_celula.py:424](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/escala/services/leitor_celula.py:424), [escala/services/leitor_celula.py:433](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/escala/services/leitor_celula.py:433), [ponto/services/cartorio.py:441](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/ponto/services/cartorio.py:441), [chamados/services/tarefa_do_dia.py:363](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/chamados/services/tarefa_do_dia.py:363), [relatorios/dia_da_frota.py:54](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/relatorios/dia_da_frota.py:54), [folha/export.py:1273](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/folha/export.py:1273).

### Script para `manage.py shell`

O universo é todo `CelulaDia.veredito='fato_em_ausencia'`, sem recorte de data ou situação cadastral. A quebra usa a empresa atual do colaborador. Para as atas que registram parcial `dentro`, escolhi reexecutar a montagem unitária real do cartório com os insumos atuais. O observador captura a ausência, a lista de minutos e o retorno nessa chamada, antes de a grade poder mover órfãs. O ID impresso vem da ausência usada pelo juiz. O FK da célula é carimbado pela consulta de cobrança. [escala/models.py:1781](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/escala/models.py:1781), [colaboradores/models.py:321](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/colaboradores/models.py:321), [ponto/services/cartorio.py:1184](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/ponto/services/cartorio.py:1184), [ponto/services/cartorio.py:2087](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/ponto/services/cartorio.py:2087), [escala/utils.py:1245](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/escala/utils.py:1245), [escala/utils.py:2050](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/escala/utils.py:2050), [ponto/services/cartorio.py:1384](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/ponto/services/cartorio.py:1384), [ponto/services/cartorio.py:1921](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/ponto/services/cartorio.py:1921).

A classificação de borda histórica exata fica [nao verificado]. A ata guarda o resultado agregado, e a janela original pode ter sido a do lote. O script mostra separadamente atas sem carimbo de período parcial, outros períodos lavrados e releituras divergentes. Uma hora ilegível sem interior conhecido também fica [nao verificado]. Use o bloco em um `manage.py shell` já aberto. A inicialização padrão do Django neste projeto prepara diretório e handlers de log em arquivo. [ponto/services/cartorio.py:748](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/ponto/services/cartorio.py:748), [ponto/services/cartorio.py:784](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/ponto/services/cartorio.py:784), [ponto/services/cartorio.py:975](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/ponto/services/cartorio.py:975), [ponto/turnos.py:2657](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/ponto/turnos.py:2657), [app/manage.py:9](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/manage.py:9), [config/settings/saas.py:118](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/config/settings/saas.py:118).

```python
import logging
import sys
from collections import Counter
from datetime import date, datetime, time, timedelta
from types import SimpleNamespace

from django.db import connection, transaction
from django.utils import timezone
from django_tenants.utils import schema_context

from colaboradores.models import Empresa
from escala import utils as grade
from escala.models import CelulaDia
from ponto import turnos

VEREDITO = "fato_em_ausencia"
CASO = (306, date(2026, 11, 24), 5775)
BORDAS = ("so_inicio", "so_fim", "ambas")
CATEGORIAS = BORDAS + (
    "estritamente_dentro",
    "outro_periodo_lavrado",
    "sem_carimbo_parcial [nao verificado]",
    "releitura_divergente [nao verificado]",
    "hora_ilegivel [nao verificado]",
)


def capturar_periodo(colaborador, data, agora):
    # Janela unitária do cartório, app/ponto/services/cartorio.py:1184-1203.
    inicio = timezone.make_aware(datetime.combine(data - timedelta(days=1), time.min))
    fim = timezone.make_aware(datetime.combine(data + timedelta(days=3), time.min))
    batidas = list(turnos.batidas_apuraveis(
        colaborador, inicio, fim - timedelta(microseconds=1)
    ))

    codigo_overlay = grade._marcar_ausencias.__code__
    codigo_horas = turnos.horas_reais_do_dia.__code__
    codigo_juiz = turnos.periodo_da_parcial_no_dia.__code__
    horas = captura = None

    def observar(frame, evento, retorno):
        nonlocal horas, captura
        if evento != "return" or frame.f_code not in (codigo_horas, codigo_juiz):
            return
        pai = frame.f_back
        if pai is None or pai.f_code is not codigo_overlay or pai.f_locals["d"] != data:
            return
        # No legado, o segmento dono do dia vem antes do aquecimento do
        # segmento seguinte, app/escala/utils.py:1000-1033.
        if captura is not None:
            return
        if frame.f_code is codigo_horas:
            horas = retorno
        elif horas is not None and frame.f_locals["horas"] is horas:
            # A lista dos marcos previstos é outra, app/escala/utils.py:1245,1260.
            captura = (frame.f_locals["a"], tuple(horas), retorno)

    perfil_anterior = sys.getprofile()
    sys.setprofile(observar)
    try:
        grade.montar_grade_prevista_periodo(
            colaborador, batidas, dias=2, ref=data + timedelta(days=1), agora=agora
        )
    finally:
        sys.setprofile(perfil_anterior)
    return captura


def posicao(ausencia, minutos):
    # Mesma unidade e virada do juiz, app/ponto/turnos.py:2653-2658.
    ini = ausencia.hora_ini.hour * 60 + ausencia.hora_ini.minute
    fim = ausencia.hora_fim.hour * 60 + ausencia.hora_fim.minute
    if fim < ini:
        fim += 1440
    if any(m is not None and ini < m < fim for m in minutos):
        return "estritamente_dentro"
    if None in minutos:
        return "hora_ilegivel [nao verificado]"
    return {
        (True, False): "so_inicio",
        (False, True): "so_fim",
        (True, True): "ambas",
    }[(ini in minutos, fim in minutos)]


def classificar(celula, agora):
    # Carimbo agregado, app/ponto/services/cartorio.py:784-789.
    ata = celula.ata or {}
    periodo = ata.get("periodo_parcial")
    if not ata.get("cobertura_parcial") or not periodo:
        return "sem_carimbo_parcial [nao verificado]", None
    if periodo != "dentro":
        return "outro_periodo_lavrado", None

    captura = capturar_periodo(celula.colaborador, celula.data, agora)
    if captura is None:
        return "releitura_divergente [nao verificado]", None
    ausencia, minutos, resultado = captura
    if resultado != "dentro":
        return "releitura_divergente [nao verificado]", ausencia.pk
    return posicao(ausencia, minutos), ausencia.pk


def imprimir_placar(rotulo, contagem):
    print(rotulo, "universo=", sum(contagem.values()),
          "so_na_borda=", sum(contagem[k] for k in BORDAS),
          {k: contagem[k] for k in CATEGORIAS})


def censo():
    # Sonda fornecida, aval.txt:1. Usa o mesmo conversor e o mesmo juiz.
    ausencia_sonda = SimpleNamespace(hora_ini=time(15, 54), hora_fim=time(19))
    horas_sonda = ("06:57", "13:43", "14:46", "15:54")
    minutos = turnos.horas_reais_do_dia({
        "celulas": [{"hora": hora} for hora in horas_sonda],
    })
    resultado = turnos.periodo_da_parcial_no_dia(ausencia_sonda, minutos)
    if resultado != "dentro" or posicao(ausencia_sonda, minutos) != "so_inicio":
        raise RuntimeError("A sonda fornecida divergiu de dentro / so_inicio")
    print("SONDA_FORNECIDA", (CASO[0], CASO[1].isoformat(), CASO[2]), "so_inicio")

    total = Counter()
    exemplos_borda, exemplos_dentro = [], []
    caso_prod = (None, "fora_do_universo [nao verificado]")

    # Tenant e backend em app/tenants/cron.py:25 e app/config/settings/saas.py:83-94.
    with schema_context("juliani"), transaction.atomic(durable=True):
        with connection.cursor() as cursor:
            cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")
            cursor.execute("SELECT current_schema()")
            if cursor.fetchone() != ("juliani",):
                raise RuntimeError("O schema ativo diverge de juliani")

        agora = timezone.localtime(timezone.now())
        empresas = {
            pk: Counter()
            for pk in Empresa.objects.order_by("pk").values_list("pk", flat=True)
        }
        celulas = (
            CelulaDia.objects.filter(veredito=VEREDITO)
            .select_related("colaborador")
            .order_by("colaborador_id", "data")
        )
        for celula in celulas.iterator(chunk_size=200):
            classe, ausencia_id = classificar(celula, agora)
            total[classe] += 1
            empresas[celula.colaborador.empresa_id][classe] += 1
            ids = (celula.colaborador_id, celula.data.isoformat(), ausencia_id)
            if classe in BORDAS and len(exemplos_borda) < 5:
                exemplos_borda.append(ids)
            elif classe == "estritamente_dentro" and len(exemplos_dentro) < 5:
                exemplos_dentro.append(ids)
            if (celula.colaborador_id, celula.data) == CASO[:2]:
                caso_prod = (ausencia_id, classe)

    if not total:
        print("UNIVERSO VAZIO")
    imprimir_placar("FROTA", total)
    for empresa_id, contagem in empresas.items():
        imprimir_placar(f"EMPRESA {empresa_id}", contagem)
    print("EXEMPLOS_SO_NA_BORDA", exemplos_borda)
    print("EXEMPLOS_ESTRITAMENTE_DENTRO", exemplos_dentro[:5 - len(exemplos_borda)])
    print("CASO_NO_REPLAY", (CASO[0], CASO[1].isoformat(), caso_prod[0]), caso_prod[1])
    if caso_prod != (CASO[2], "so_inicio"):
        print("CASO_ESPERADO", (CASO[0], CASO[1].isoformat(), CASO[2]),
              "so_inicio [nao verificado no replay]")


nivel_anterior = logging.root.manager.disable
logging.disable(sys.maxsize)
try:
    censo()
finally:
    logging.disable(nivel_anterior)
```

## 25/11 19:57 · sessao db299125f767 · E1-PREVISAO-24H · leitura · origem=Code

# E1-PREVISAO-24H — triagem dos 11 testes [nao verificado]

1. **`test_MORDE_grade_marca_o_dia_zera_previsto_e_guarda_realizado` — CAUSADO PELO DIFF**, hunk `@@ -0,0 +1,380 @@` de `app/escala/tests/test_e1_previsao_do_dia_valida.py`.  
   **Causa.** O teste lê `g['dias'][0]['data']`, mas a grade identifica esse dia pela posição `ref - i` e fornece `label`, daí o `KeyError`. `app/escala/tests/test_e1_previsao_do_dia_valida.py:202`, `app/escala/tests/test_e1_previsao_do_dia_valida.py:216`, `app/escala/utils.py:1878`, `app/escala/utils.py:1922`.  
   **Cura mínima.** Na asserção de identidade, conferir `d['label'] == 'Qua 30/09'` e manter as três verificações seguintes de marca, previsto zero e realizado de 210 minutos. O resultado dessas três verificações após a troca é [nao verificado]. `app/escala/utils.py:1400`, `app/escala/utils.py:1891`, `app/escala/tests/test_e1_previsao_do_dia_valida.py:216`.

2. **`test_carga_da_tarefa_pergunta_ao_c6_avulso_lote_e_idempotencia` — CAUSADO PELO DIFF**, hunk `@@ -236,6 +240,16 @@` de `app/chamados/services/tarefa_do_dia.py`.  
   **Causa.** `_tarefa_pura` passou a importar `MOTIVO_PREVISAO_INVALIDA`, que existe no módulo real e falta no dublê de `escala.utils` instalado pelo teste. `app/chamados/services/tarefa_do_dia.py:243`, `app/escala/utils.py:369`, `app/chamados/tests/e3_c8b_apoio.py:184`.  
   **Cura mínima.** Expor `MOTIVO_PREVISAO_INVALIDA=literais('escala/utils.py')['MOTIVO_PREVISAO_INVALIDA']` no dublê compartilhado. O teste passou com essa mudança em memória, com as asserções originais. `app/chamados/tests/e3_c8b_apoio.py:184`, `app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:55`, `app/chamados/tests/test_e3_c8b_sem_banco.py:272`.

3. **`test_atomic_template_desfaz_save` — CAUSADO PELO DIFF**, hunk `@@ -303,6 +329,7 @@` de `app/escala/services/cadastro_tipo.py`.  
   **Causa.** `salvar_tipo` chama o novo validador antes do `save()`, mas o carregador AST do teste omite esse nome do namespace e para com `NameError` antes da falha de regeneração que deveria provocar o rollback. `app/escala/services/cadastro_tipo.py:332`, `app/ponto/tests/test_cadastro_futuro_c3.py:25`, `app/ponto/tests/test_cadastro_futuro_c3.py:104`.  
   **Cura mínima.** Injetar um `Mock(return_value=None)` para `validar_previsao_na_porta` nos globais de `atomica()` desse teste, conferir a chamada com `self.tipo` e preservar as asserções de erro e rollback. O teste passou com essa injeção em memória. `app/ponto/tests/test_cadastro_futuro_c3.py:94`, `app/ponto/tests/test_cadastro_futuro_c3.py:128`, `app/ponto/tests/test_cadastro_futuro_c3.py:135`.

4. **`test_MORDE_dia_marcado_sai_do_previsto_e_da_falta` — CAUSADO PELO DIFF**, hunk `@@ -0,0 +1,380 @@` de `app/escala/tests/test_e1_previsao_do_dia_valida.py`.  
   **Causa.** A asserção de 8 h lê `FechamentoMensal.horas_falta`, que soma ausências aprovadas de tipos que descontam, e a fixture não cria nenhuma, daí `0.0 × 0.0`. `app/escala/tests/test_e1_previsao_do_dia_valida.py:331`, `app/escala/tests/test_e1_previsao_do_dia_valida.py:361`, `app/ponto/services/fechamento.py:886`, `app/ponto/services/fechamento.py:1020`, `app/folha/export.py:87`, `app/folha/export.py:138`.  
   **Cura mínima.** Passar `coletar_furos={}` ao fechamento e exigir `DIA` nos furos do gêmeo válido e sua ausência nos do inválido. Manter a marca na ata e os deltas de 1 dia e 480 minutos. Isso exercita a exclusão do dia dos previstos entregues ao motor no hunk `@@ -737,7 +738,10 @@` de `app/ponto/services/fechamento.py`. O resultado do teste ajustado é [nao verificado]. `app/escala/tests/test_e1_previsao_do_dia_valida.py:352`, `app/ponto/services/fechamento.py:742`, `app/ponto/services/fechamento.py:1135`, `app/ponto/motor_calculo_v2.py:2611`.

5. **`test_MORDE_nenhuma_data_que_decide_vence_em_menos_de_7_dias` — DATA CRAVADA/RELÓGIO**, introduzida pelos hunks `@@ -635,34 +638,42 @@` de `app/escala/models.py` e `@@ -363,6 +363,104 @@` de `app/escala/utils.py`.  
   **Causa.** A heurística procura sinais de relógio no arquivo inteiro e classificou as duas âncoras de `2026-01-05` como `DECIDE`, ambas a −324 dias no relógio do pouso. `app/escala/models.py:668`, `app/escala/utils.py:370`, `app/core/datas_cravadas.py:100`, `app/core/datas_cravadas.py:127`, `app/core/datas_cravadas.py:141`.  
   **Cura mínima.** Registrar no mesmo commit as chaves `('escala/models.py', '_seg')` e `('escala/utils.py', '_DIA_NEUTRO_DA_CADEIA')` como `APOIO` em `DECLARADAS`. A primeira ancora os sete weekdays e a segunda ancora a cadeia de marcos quando falta o dia civil. Nenhuma das duas é comparada com o relógio nesses usos. `app/core/datas_cravadas.py:34`, `app/escala/models.py:668`, `app/escala/models.py:230`, `app/escala/utils.py:395`.

6. **`test_MORDE_o_censo_nao_fica_cego` — SELO DE CONTRATO**, registro do censo no mesmo commit.   [nao verificado]
   **Causa.** O censo encontra as duas âncoras, mas a asserção exige zero entradas `DECIDE` em qualquer dia de execução. `app/core/tests/test_selo_sem_data_cravada.py:47`, `app/core/tests/test_selo_sem_data_cravada.py:50`, `app/escala/models.py:668`, `app/escala/utils.py:370`.  
   **Cura mínima.** Aplicar as mesmas duas declarações `APOIO` em `app/core/datas_cravadas.py:34`. Conferi com `censo(hoje=date(2026, 11, 25))` e `vencendo(...)`: com o registro só em memória, o censo manteve 18 achados e passou a ter zero `DECIDE` e zero vencendo. `app/core/datas_cravadas.py:76`, `app/core/datas_cravadas.py:129`, `app/core/datas_cravadas.py:141`.

7. **`test_MORDE_sitio_novo_e_lista_velha_ficam_vermelhos` — SELO DE CONTRATO**, lista `HOJE`.  
   **Causa.** A sentinela acrescenta `regua_para` ao censo real, que já traz `previsao_invalida.py::medir` como sítio sem registro, e recebe dois achados onde esperava um. `app/escala/tests/test_semselo5_ativo_nao_responde_por_dia.py:418`, `app/escala/services/previsao_invalida.py:46`.  
   **Cura mínima.** Fazer o registro de `medir` em `HOJE` descrito no item 8 e preservar as duas provas da sentinela, a de sítio novo e a de lista velha. Ambas passaram com o registro em memória. `app/escala/tests/test_semselo5_ativo_nao_responde_por_dia.py:46`, `app/escala/tests/test_semselo5_ativo_nao_responde_por_dia.py:418`.

8. **`test_a_arvore_nao_tem_sitio_novo_nem_lista_velha` — SELO DE CONTRATO**, lista `HOJE`.  
   **Causa.** O hunk `@@ -0,0 +1,83 @@` de `app/escala/services/previsao_invalida.py` introduz `EscalaColaborador.filter(ativa=True)` em `medir` para sinalizar o uso cadastral atual dos templates. A contagem de dias já consulta o vínculo da data. `app/escala/services/previsao_invalida.py:41`, `app/escala/services/previsao_invalida.py:46`, `app/escala/services/previsao_invalida.py:51`, `app/escala/utils.py:454`, `app/escala/utils.py:2323`.  
   **Cura mínima.** Registrar `escala/services/previsao_invalida.py::medir` em `HOJE` no mesmo commit, com o motivo “censo de cadastro corrente: informa se o template tem vínculo marcado ativo”. O registro é no próprio arquivo do selo. `veredito(censo_da_arvore())` conferiu a lista com a entrada em memória. `app/escala/tests/test_semselo5_ativo_nao_responde_por_dia.py:46`, `app/escala/tests/test_semselo5_ativo_nao_responde_por_dia.py:348`, `app/escala/tests/test_semselo5_ativo_nao_responde_por_dia.py:365`.

9. **`test_CATRACA_linhas_reais_congeladas_exigem_registro_no_mesmo_commit` — SELO DE CONTRATO**, catraca de linhas.  
   **Causa.** Os hunks de `escala/utils.py`, `ponto/services/cartorio.py` e `ponto/services/fechamento.py` deslocaram sete entradas do registro, mantendo as identidades e as contagens de 9 pareadores e 81 derivações. O A1 de `_alinhar_por_papel`, por exemplo, foi de 529 para 627. `app/escala/utils.py:366`, `app/escala/utils.py:627`, `app/ponto/services/cartorio.py:790`, `app/ponto/services/fechamento.py:733`, `app/core/tests/test_medida_turno_do_par.py:251`, `app/core/tests/test_medida_turno_do_par.py:563`.  
   **Cura mínima.** Depois das curas de código, rodar `core.medida_turno_do_par.censo("app")` a partir da raiz com `PYTHONPATH=app`, projetar os `itens` por `(chave, sitio ou criterio) → linhas` como na asserção e registrar o resultado em `LINHAS_REGISTRADAS` no mesmo commit. `app/core/medida_turno_do_par.py:448`, `app/core/tests/test_medida_turno_do_par.py:251`, `app/core/tests/test_medida_turno_do_par.py:563`.

10. **`test_MORDE_o_mmd_commitado_bate_com_o_codigo` — SELO DE CONTRATO**, documentação gerada.  
    **Causa.** O hunk `@@ -210,6 +210,11 @@` de `app/core/juizes.py` acrescentou a 11ª pergunta com juiz em `celula/precedencia`. O `.mmd` e o `MAPA.md` ainda mostram 10. Conferi em memória que os trechos dessa família voltam a coincidir com os arquivos quando se retira só a entrada nova. `app/core/juizes.py:217`, `app/docs/ARQUITETURA.mmd:324`, `app/docs/MAPA.md:85`, `app/core/management/commands/gerar_diagrama.py:344`, `app/core/management/commands/gerar_diagrama.py:849`.  
    **Cura mínima.** Rodar `python3 bin/gerar_diagrama.py` e incluir `app/docs/ARQUITETURA.mmd` e `app/docs/MAPA.md` no mesmo commit. O `--check` confere os dois. `bin/gerar_diagrama.py:15`, `app/core/management/commands/gerar_diagrama.py:921`, `app/core/management/commands/gerar_diagrama.py:931`.

11. **`test_MORDE_uma_aresta_a_mais_no_mmd_derruba_o_selo` — SELO DE CONTRATO**, mesma documentação gerada.  
    **Causa.** A falha ocorre no segundo `--check`, depois de restaurar o `.mmd` original, porque o original e o `MAPA.md` já divergem do gerador. `app/core/tests/test_selo_diagrama_do_codigo.py:88`, `app/core/tests/test_selo_diagrama_do_codigo.py:94`, `app/core/tests/test_selo_diagrama_do_codigo.py:98`, `app/core/management/commands/gerar_diagrama.py:923`.  
    **Cura mínima.** Regenerar os dois arquivos com `python3 bin/gerar_diagrama.py` no mesmo commit, como no item 10, e preservar a injeção da aresta e as duas asserções. `bin/gerar_diagrama.py:15`, `app/core/management/commands/gerar_diagrama.py:931`, `app/core/tests/test_selo_diagrama_do_codigo.py:90`.

**Verificação pendente.** A suíte inteira após as curas e uma execução da revisão base no mesmo ambiente e relógio são [nao verificado]. Para provar pré-existência de um teste já presente na base, é preciso reproduzir a mesma falha nessa revisão sob essas condições. [nao verificado]

## 25/11 19:55 · sessao f7d727364eb8 · - · - · origem=Ronald (tmux)

O caminho de leitura é `batidas_apuraveis` → `montar_grade_prevista_periodo` → `_marcar_ausencias`. Para reexecutar a montagem unitária de uma célula do dia `d`, o Cartório usa batidas de `d−1` à meia-noite até `d+3` à meia-noite menos 1 μs, `ref=d+1`, `dias=2` e o instante `agora`. A célula está em `grade['dias'][1]`. `app/ponto/services/cartorio.py:1184-1207`, `app/escala/utils.py:2095-2101`.

O objeto `a` vem de `cobertura_ausencia_periodo(..., para='folha')`, com aprovação e desempate pelo maior `(data, pk)`. `CelulaDia.ausencia_id` vem da consulta para cobrança, que pode escolher outra ausência. `app/escala/utils.py:1198-1202`, `app/ponto/turnos.py:2528-2537`, `app/ponto/turnos.py:2752-2804`, `app/ponto/services/cartorio.py:1384`, `app/ponto/services/cartorio.py:1921`.

Dois wrappers em memória, restaurados via `try/finally`, capturam os argumentos exatos. O de `horas_reais_do_dia` mantém o último par `(dia, lista)`. O de `periodo_da_parcial_no_dia` delega ao original e anexa ao dicionário `dia` os argumentos e o resultado somente se `horas is lista`. A identidade exclui a chamada que usa `[offset]` para dispensar marcos. O builder por turno devolve o dia original e o legado propaga suas chaves com `**sd`. Capturar no ponto da chamada também preserva a posição original de batidas órfãs, que podem ser remanejadas depois. `app/escala/utils.py:1245-1261`, `app/escala/utils.py:1054`, `app/escala/utils.py:1859`, `app/escala/utils.py:2050-2051`, `app/escala/utils.py:2082`.

O universo vem de `CelulaDia.veredito='fato_em_ausencia'`. A ata confirma `cobertura_parcial` e `periodo_parcial`, mas a reconstrução dos mesmos argumentos históricos é [nao verificado]. A decisão de recontar sob insumos atuais deve ser explícita. Um `None` sem outra batida estritamente interior torna a exclusividade de borda [nao verificado]. `app/escala/models.py:1762-1789`, `app/ponto/services/cartorio.py:754-789`, `app/ponto/turnos.py:2651-2684`.

## 25/11 19:55 · sessao 3047610d7a79 · - · - · origem=Ronald (tmux)

O veredito persistido é `fato_em_ausencia`. O suprajuiz emite `FATO_EM_AUSENCIA` nos dois ramos de fato em dia com cobertura aprovada. O mapa de vereditos aplica sua ordem de precedência para chegar ao valor persistido. [app/ponto/supra_juiz.py:392](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/ponto/supra_juiz.py:392), [app/ponto/supra_juiz.py:450](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/ponto/supra_juiz.py:450), [app/escala/models.py:1622](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/escala/models.py:1622)

- A grade seleciona a ausência aprovada com `para='folha'` e chama o juiz com essa ausência e `horas_reais_do_dia(dia, deslocamentos.get(d))`. A segunda chamada da grade pergunta sobre marco previsto faltante. A comparação inclusiva fica em `app/ponto/turnos.py:2658`. [app/escala/utils.py:1198](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/escala/utils.py:1198), [app/escala/utils.py:1241](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/escala/utils.py:1241), [app/escala/utils.py:1259](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/escala/utils.py:1259), [app/ponto/turnos.py:2658](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/ponto/turnos.py:2658)
- O cartório recebe os códigos de `classificar_dia`, copia `periodo_parcial` para a ata, chama `lavrar_veredito` e persiste em `carimbar`. O leitor `grade_da_celula` transporta a ata e o veredito. `parcial_so_lista`, a tarefa do dia, o relatório da frota e a retenção da folha são consumidores. [app/ponto/services/cartorio.py:1499](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/ponto/services/cartorio.py:1499), [app/ponto/services/cartorio.py:784](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/ponto/services/cartorio.py:784), [app/ponto/services/cartorio.py:1898](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/ponto/services/cartorio.py:1898), [app/ponto/portas/celula.py:1218](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/ponto/portas/celula.py:1218), [app/escala/services/leitor_celula.py:424](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/escala/services/leitor_celula.py:424), [app/ponto/services/cartorio.py:444](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/ponto/services/cartorio.py:444), [app/chamados/services/tarefa_do_dia.py:363](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/chamados/services/tarefa_do_dia.py:363), [app/relatorios/dia_da_frota.py:54](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/relatorios/dia_da_frota.py:54), [app/folha/export.py:1273](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/folha/export.py:1273)
- Para o censo, capture os argumentos da chamada efetiva durante a construção da grade, antes do tratamento posterior de batidas órfãs. Use `a.pk` da chamada, pois `CelulaDia.ausencia` vem da cobertura para cobrança. A ata e a impressão armazenam a classificação agregada, insuficiente para comprovar que os limites atuais são os da lavra histórica. [app/escala/utils.py:1245](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/escala/utils.py:1245), [app/escala/utils.py:2050](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/escala/utils.py:2050), [app/ponto/services/cartorio.py:1384](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/ponto/services/cartorio.py:1384), [app/ponto/services/cartorio.py:473](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/ponto/services/cartorio.py:473), [app/ponto/services/cartorio.py:784](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/ponto/services/cartorio.py:784)

As funções reais, executadas em memória com a sonda fornecida, devolveram os minutos `[417, 823, 886, 954]` e `dentro` para o intervalo `[954, 1140]`. Só `954` toca o período, na borda inicial. A cardinalidade de produção permanece [nao verificado]. [app/ponto/turnos.py:2651](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/ponto/turnos.py:2651), [app/ponto/turnos.py:2675](/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/app/ponto/turnos.py:2675)

## 25/11 19:55 · sessao 6a0e1bf32755 · - · - · origem=Ronald (tmux)

- **`test_a_arvore_nao_tem_sitio_novo_nem_lista_velha` — SELO DE CONTRATO.** O hunk de criação de `previsao_invalida.py` introduz `EscalaColaborador.objects.filter(ativa=True)` no censo de uso atual dos templates. O vigia dos dias já escolhe o vínculo de cada data. Registrar `escala/services/previsao_invalida.py::medir` em `HOJE` no mesmo commit, com o motivo “censo do cadastro corrente: sinaliza se o template tem algum vínculo marcado ativo”. O censo AST devolveu somente essa entrada nova. `app/escala/services/previsao_invalida.py:41`, `app/escala/services/previsao_invalida.py:46`, `app/escala/services/previsao_invalida.py:51`, `app/escala/utils.py:454`, `app/escala/tests/test_semselo5_ativo_nao_responde_por_dia.py:46`, `app/escala/tests/test_semselo5_ativo_nao_responde_por_dia.py:367`.
- **`test_MORDE_sitio_novo_e_lista_velha_ficam_vermelhos` — SELO DE CONTRATO.** A sentinela injeta `regua_para` sobre o censo real, que já traz `medir` como nova entrada. O mesmo registro em `HOJE` resolve a falha e conserva as duas provas. A inclusão em memória passou nos sete testes do módulo. `app/escala/tests/test_semselo5_ativo_nao_responde_por_dia.py:358`, `app/escala/tests/test_semselo5_ativo_nao_responde_por_dia.py:418`.
- **`test_CATRACA_linhas_reais_congeladas_exigem_registro_no_mesmo_commit` — SELO DE CONTRATO.** Os hunks de `utils.py`, `cartorio.py` e `fechamento.py` deslocaram as linhas de sete entradas já registradas, preservando as identidades e as contagens de 9 e 81. Por exemplo, o A1 de `_alinhar_por_papel` passou de `529` a `627`. Depois das curas de código, extrair com `core.medida_turno_do_par.censo("app")` e atualizar `LINHAS_REGISTRADAS` no mesmo commit, pela projeção usada na asserção. `app/escala/utils.py:627`, `app/core/medida_turno_do_par.py:448`, `app/core/tests/test_medida_turno_do_par.py:251`, `app/core/tests/test_medida_turno_do_par.py:563`.

## 25/11 19:55 · sessao 980129d350e6 · - · - · origem=Ronald (tmux)

- `test_MORDE_grade_marca_o_dia_zera_previsto_e_guarda_realizado` — **CAUSADO PELO DIFF**, hunk do novo arquivo `@@ -0,0 +1,380 @@`. O teste lê `d['data']` no builder cujo contrato é `dias[i] = ref - i` e cujo dia identifica a posição com `label`. Ajustar a asserção de identidade para `d['label'] == 'Qua 30/09'` e preservar as verificações de marca, previsto zero e realizado de 210 min. A execução das asserções seguintes é [nao verificado]. `app/escala/tests/test_e1_previsao_do_dia_valida.py:200–220`, `app/escala/utils.py:1400,1878–1891,1916–1926`, `app/ponto/services/cartorio.py:1209–1213`.
- `test_MORDE_dia_marcado_sai_do_previsto_e_da_falta` — **CAUSADO PELO DIFF**, hunk do novo arquivo `@@ -0,0 +1,380 @@`. A fixture não cria ausência aprovada que desconta, única fonte de `horas_falta`, de modo que os dois totais são zero. Na asserção de origem, usar `coletar_furos` do fechamento para exigir `DIA` no gêmeo válido e ausência no inválido, preservando a marca na ata e os deltas de um dia e 480 min. Isso cobre o hunk `fechamento.py @@ -737,7 +738,10 @@`, que exclui o dia recusado dos previstos entregues ao motor. A execução do teste assim corrigido é [nao verificado]. `app/escala/tests/test_e1_previsao_do_dia_valida.py:331–362`, `app/folha/export.py:87–92,125–143`, `app/ponto/services/fechamento.py:743–753,886–888,1020,1135–1147`, `app/ponto/motor_calculo_v2.py:2611–2627`.

## 25/11 19:55 · sessao 04e413a9dd26 · - · - · origem=Ronald (tmux)

- **`test_carga_da_tarefa_pergunta_ao_c6_avulso_lote_e_idempotencia` — CAUSADO PELO DIFF.** O hunk `@@ -236,6 +240,16` acrescentou a importação em `app/chamados/services/tarefa_do_dia.py:243`. O teste instala um dublê de `escala.utils` que só fornece `marcos_dna_periodo`, disparando `ImportError` antes da consulta à marca. Cure em `app/chamados/tests/e3_c8b_apoio.py:184`, expondo `MOTIVO_PREVISAO_INVALIDA=literais('escala/utils.py')['MOTIVO_PREVISAO_INVALIDA']`. O valor sai da declaração real em `app/escala/utils.py:369` e as asserções de C6, avulso, lote e idempotência continuam em `app/chamados/tests/test_e3_c8b_sem_banco.py:272`.

- **`test_atomic_template_desfaz_save` — CAUSADO PELO DIFF.** O hunk `@@ -303,6 +329,7` chama o validador antes do `save` em `app/escala/services/cadastro_tipo.py:332`. O carregador AST restringe os símbolos a uma lista sem o validador em `app/ponto/tests/test_cadastro_futuro_c3.py:25`, daí o `NameError`. Injete no próprio teste um `Mock(return_value=None)` como `validar_previsao_na_porta`, pelos globais que `atomica()` recebe em `app/ponto/tests/test_cadastro_futuro_c3.py:94`. Confira a chamada única junto às asserções de exceção e rollback em `app/ponto/tests/test_cadastro_futuro_c3.py:128`. A aceitação e a recusa do validador real estão cobertas em `app/escala/tests/test_e1_previsao_do_dia_valida.py:120` e `app/escala/tests/test_e1_previsao_do_dia_valida.py:127`.

Reproduzi os dois erros da cópia e os dois testes passaram com os ajustes aplicados em memória, sob bloqueio de escrita no processo. `app/chamados/tests/test_e3_c8b_sem_banco.py:211`, `app/ponto/tests/test_cadastro_futuro_c3.py:128`.

## 25/11 19:54 · sessao aae1b974ab2a · - · - · origem=Ronald (tmux)

O universo é `CelulaDia.objects.filter(veredito='fato_em_ausencia')`, com uma célula por colaborador/data. Agrupamento pelo `empresa_id` cadastral, empregado pela própria consulta de frota. `app/escala/models.py:1769`, `app/escala/models.py:1781`, `app/escala/models.py:1844`, `app/colaboradores/models.py:321`, `app/relatorios/dia_da_frota.py:34-37`.

A reprodução fiel dos argumentos de uma nova execução usa `batidas_apuraveis`, o dispatcher de grade e um observador `sys.setprofile` temporário sobre o retorno de `horas_reais_do_dia` quando chamada por `_marcar_ausencias`. O retorno contém os minutos recebidos pelo juiz e o frame chamador fornece `_aus_por_dia[d]`. A grade terminada pode divergir da entrada usada pelo juiz porque a reconciliação de órfãs ocorre depois. `app/ponto/services/cartorio.py:1184-1203`, `app/escala/utils.py:1199`, `app/escala/utils.py:1245`, `app/escala/utils.py:1974-2051`, `app/escala/utils.py:2095-2101`, `app/ponto/turnos.py:2663-2684`.

A ata preserva a categoria agregada, as luzes e as horas de órfãs. A seleção histórica exata de janela e os argumentos originais permanecem `[nao verificado]`. O FK da célula tem origem na consulta de cobrança. A ausência que alimenta o juiz tem origem na cobertura de folha. `app/ponto/services/cartorio.py:748-789`, `app/ponto/services/cartorio.py:975-978`, `app/ponto/services/cartorio.py:1384`, `app/ponto/services/cartorio.py:1921`, `app/escala/utils.py:1199`.

## 25/11 19:31 · sessao b07690a9ec74 · GUARDA-10-B · verificador · origem=Code

**REPROVADO no texto e na LEI-AKITA.** A opção (b) está implementada. O vigia sai do `--apply`, direto e delegado. A mensagem da cura ainda garante demais sobre o laço, e `padrao=lampada` existe no glossário, mas descreve outro mecanismo. `app/ponto/management/commands/recalcular_fechamento.py:121`, `app/ponto/management/commands/aplicar_janela_he_total.py:144`, `msgs_vigia_seco.txt:21`, `app/docs/GLOSSARIO.md:13`

Conferi em memória os hashes dos dois arquivos da cópia e dos dois arquivos-base reconstruídos pelo diff. Os quatro batem com o anexo. A identidade dos commits, o RED executado, a suíte verde e o pouso estão [nao verificado]. `diff_vigia_seco_v2.diff:2`, `diff_vigia_seco_v2.diff:29` [nao verificado]

### 1. Comentário e mensagens

- **Comentário final e `c2969794`.** A correção de escopo procede. O vigia conta chaves da guarda sem TXT vigente em qualquer empresa ativa. A consulta ignora todos os filtros do comando, inclusive um `--empresa` que nomeie uma empresa ativa. Já a guarda de escrita pode barrar uma inativa indicada por `--empresa` ou alcançada por `--colabs`, se houver TXT vigente e colaboradores elegíveis. A generalização antiga ainda aparece na docstring do serviço e no nome do teste de empresa inativa. A atribuição inicial ao aval `GUARDA-10-B-2511` é imprecisa se lida como autorização do vigia. O literal reproduzido no código trata da guarda de 09 e 10 e da lista ao DP. O aval específico da opção (b) aparece no comentário e na mensagem. Seu texto integral está [nao verificado]. A alegação de que `c2969794` só mudou o comentário descreve a volta informada, mas o diff acumulado não isola esse commit. `recalcular_fechamento.py:99`, `fechamento.py:43`, `fechamento.py:54`, `fechamento.py:580`, `fechamento.py:1532`, `test_guarda_10_b.py:60`, `msgs_vigia_seco.txt:4`
- **Cura `026b4157`.** A retirada da chamada e a correção de “última linha” conferem. “O comentário passa a dizer o que o código faz” ainda era impreciso nessa volta, porque mantinha a generalização sobre inativas. “Sem o vigia […] o laço não para” é falso como garantia. A retirada elimina aquela causa de parada. Uma falha na consulta do próprio escritor, por exemplo, ainda pode subir e interromper o laço. “Único chamador com `apply=True`” vale para o chamador executável de produção encontrado na árvore. Há testes com `apply=True` e entrada manual. “Nenhum leitor automatizado” exige a mesma ressalva, pois há asserções sobre o literal. `msgs_vigia_seco.txt:18`, `msgs_vigia_seco.txt:26`, `diff_vigia_seco_v2.diff:20`, `recalcular_fechamento.py:3`, `fechamento.py:559`, `aplicar_janela_he_total.py:144`, `test_guarda_10_b.py:85`, `test_guarda_10_b.py:96`
- **RED `cdea0d94`.** “Os casos novos têm de cair” é amplo demais. Os **três casos de apply** têm motivo para cair na base. O quarto caso novo, de DRY, deve passar na base porque o DRY já chamava o vigia. O diff acumulado não comprova o conteúdo isolado de `cdea0d94` nem a execução do RED. `msgs_vigia_seco.txt:35`, `msgs_vigia_seco.txt:37`, `diff_vigia_seco_v2.diff:20`, `test_guarda_10_b.py:90`, `test_guarda_10_b.py:107`, `test_guarda_10_b.py:114`
- **LEI-AKITA, nos três textos.** `origem` identifica o método do vigia. O ponto da correção de fluxo é a chamada em `Command.handle`. `testemunha=guarda_sem_export` nomeia a função que fornece o resultado ao comando. A autoridade de exportação que ela consulta é K6, `competencia_entregue`. `RED` aponta para os testes pertinentes e `juizes novos=0` confere. `quem-mais-le=aplicar_janela_he_total.py:145` identifica um **chamador** do comando. Como censo de leitores, omite as asserções que leem o stdout. `padrao=lampada`, na cura e no comentário, passa pelo vocabulário. No glossário, porém, `lampada` é o sinal derivado do estado da célula/ata. O delta aplica a regra do vigia da seção 4a ao tirar o contador da cadeia de escrita. O padrão concretamente realizado pelos testes é `selo`, também cadastrado. No RED, `n/a (medida)` é aceito para um commit só de testes, conforme descrito. `msgs_vigia_seco.txt:9`, `msgs_vigia_seco.txt:30`, `msgs_vigia_seco.txt:39`, `CLAUDE.md:62`, `CLAUDE.md:170`, `app/docs/GLOSSARIO.md:13`, `app/docs/PLANO-PADROES.md:61`, `bin/tests/test_lei_akita.sh:337`, `fechamento.py:64`, `fechamento.py:1561`, `test_guarda_10_b.py:74`

### 2. Caminhos, escritores e leitores

- **Nenhum caminho encontrado para o vigia durante o `--apply`.** `handle` chama `_vigia_guarda()` no DRY e retorna. O wrapper chama o filho com `apply=True` por empresa. A guarda efetiva continua no serviço, que consulta K6 e exclui os colaboradores barrados antes do recálculo. O apply também lê a trilha pelo contador separado `barrados_pela_guarda()`. `recalcular_fechamento.py:105`, `recalcular_fechamento.py:121`, `recalcular_fechamento.py:130`, `recalcular_fechamento.py:135`, `aplicar_janela_he_total.py:144`, `fechamento.py:67`, `fechamento.py:559`, `fechamento.py:580`
- **Quem escreve e quem lê.** `_vigia_guarda` escreve `guarda_sem_export=` no stdout. Os testes leem presença no DRY e ausência no apply. O próprio `recalcular_fechamento` escreve `processados=`, `foto=` ou o aviso, a tabela, `fechamentos_mexidos=`, `fechamentos_novos=` e o detalhe. Os selos de apply leem três desses marcadores. O wrapper faz seus próprios carimbos e compara os valores capturados por ele. A chamada ignora o retorno do filho e dispensa captura ou interpretação de sua saída. O relato usa os contadores para conferência e o JSON para reversão manual. A busca estática não encontrou leitor executável desatualizado na cópia. Consumo por sonda, scripts externos ou crontab do host [nao verificado]. `recalcular_fechamento.py:107`, `recalcular_fechamento.py:133`, `recalcular_fechamento.py:147`, `recalcular_fechamento.py:153`, `recalcular_fechamento.py:163`, `test_guarda_10_b.py:90`, `test_guarda_10_b.py:107`, `aplicar_janela_he_total.py:75`, `aplicar_janela_he_total.py:144`, `app/docs/RELATO-ARQUIVO.md:18991`, `app/docs/RELATO-ARQUIVO.md:18999`

### 3. Mordida e escrita dos testes [nao verificado]

- **Sim, em qualquer posição executada pelas fixtures**, inclusive depois dos contadores finais. O selo de acusação examina todo o stdout. O de exceção injeta `RuntimeError` na função que o vigia importa e exige que `call_command` retorne. O do wrapper exige duas chamadas reais ao serviço, na ordem, e o carimbo `depois` da segunda empresa. Ele prova que o laço chega à segunda chamada, sem exigir um colaborador gravado nessa empresa. Um ramo que as fixtures não percorrem escapa à prova. O quarto selo protege a emissão no DRY. `recalcular_fechamento.py:105`, `test_guarda_10_b.py:80`, `test_guarda_10_b.py:90`, `test_guarda_10_b.py:98`, `test_guarda_10_b.py:107`, `test_guarda_10_b.py:114`
- **Fotos e carimbos dos selos ficam em diretórios temporários.** Há escrita no banco de teste. No selo do wrapper, as duas fotos do filho podem usar o mesmo nome dentro da pasta temporária, pois ele omite a empresa e o relógio está congelado. A suíte da casa usa `config.settings.ci`. Com settings SaaS, a configuração pode abrir logs em `/app/logs` fora da pasta do teste. `test_guarda_10_b.py:35`, `test_guarda_10_b.py:80`, `test_guarda_10_b.py:119`, `recalcular_fechamento.py:140`, `aplicar_janela_he_total.py:75`, `bin/suite.sh:373`, `app/config/settings/saas.py:118`, `app/config/settings/saas.py:136`, `app/config/settings/saas.py:160`

### 4. Checklist

| Item | Resultado |
|---|---|
| Segundo juiz, segundo escritor e derivação paralela | **Zero introduzidos.** O contador e a guarda consultam K6. O apply delega ao escritor existente. `fechamento.py:64`, `fechamento.py:559`, `fechamento.py:1561`, `app/core/juizes.py:342`, `recalcular_fechamento.py:130`, `aplicar_janela_he_total.py:144` |
| Relógio, ordem de trava, `except` e fallback | **Zero leituras novas de relógio, zero `except` novos, zero fallbacks novos.** O relógio e o `except OSError` da foto já existiam. A guarda continua antes da escrita. `diff_vigia_seco_v2.diff:20`, `recalcular_fechamento.py:139`, `recalcular_fechamento.py:148`, `fechamento.py:580`, `fechamento.py:1168` |
| Selo/RED e campo com dois significados | **Mordida estática nos três applies exercitados.** Zero campos com significado duplo introduzidos. `guarda_sem_export` conta chaves da guarda, `processados` vem do serviço e `fechamentos_mexidos` vem das fotos. `test_guarda_10_b.py:90`, `test_guarda_10_b.py:114`, `fechamento.py:64`, `fechamento.py:1335`, `recalcular_fechamento.py:158` |
| Constante copiada | As chaves da guarda são sintéticas, `HOJE` é importado e os caminhos são temporários. A segunda empresa usa o literal `21`, que coincide com o default do modelo e serve de cadastro da fixture. `test_guarda_10_b.py:28`, `test_guarda_10_b.py:119`, `app/colaboradores/models.py:46` |
| Pouso e escopo | O delta cobre a opção (b) descrita nos anexos com a remoção da chamada, comentário e selos. A lista ao DP pertence ao aval original e está fora deste diff. Correção documental no merge, ordem efetiva do pouso, cobertura da emp4 pela lista e zero exportações de outubro em produção [nao verificado]. `diff_vigia_seco_v2.diff:5`, `diff_vigia_seco_v2.diff:20`, `diff_vigia_seco_v2.diff:32`, `fechamento.py:43`, `msgs_vigia_seco.txt:27`, `opus_final.md:5`, `opus_final.md:8`, `CLAUDE.md:389` | [nao verificado]

## 25/11 19:29 · sessao 28f53074a57f · A4-ESPELHO-AUSENCIA-ABERTA · sonda · origem=Code

A tentativa de executar a sonda retornou este erro. Execução em PROD [nao verificado].

```text
python3: can't open file '/home/ronald/codex_ws/1fdc769869d022ce2ea9cdf3f758d5c0456824e7/sonda_smoke.py': [Errno 2] No such file or directory
```

Saída raspada fornecida, na íntegra [nao verificado].

```text
sonda_leitura: sonda_smoke.py [b73c9b448b44def5] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

TIPO atestado abertas= 77
  colab_id=188 nome=[nome] empresa_id=2 data=2026-12-05 data_fim=None status=aguardando_decisao palavra='Atestado aguardando decisao do DP'
  colab_id=347 nome=[nome] empresa_id=2 data=2026-11-25 data_fim=2026-11-26 status=aguardando_decisao palavra='Atestado aguardando decisao do DP'
  colab_id=82 nome=[nome] empresa_id=3 data=2026-11-25 data_fim=2026-12-03 status=aguardando_decisao palavra='Atestado aguardando decisao do DP'
TIPO declaracao abertas= 7
  colab_id=631 nome=[nome] empresa_id=2 data=2026-06-04 data_fim=None status=aguardando_decisao palavra='Declaração aguardando decisao do DP'
```

Para o smoke de tela, pela data mais recente de cada tipo no recorte fornecido [nao verificado em PROD]:

- `atestado` — `colab_id=188` · `Atestado aguardando decisao do DP` [nao verificado]
- `declaracao` — `colab_id=631` · `Declaração aguardando decisao do DP` [nao verificado]

## 25/11 19:26 · sessao 0abc9cc4d19a · GUARDA-10-B · verificador · origem=Code

**GUARDA-10-B · VIGIA-SECO. A opção (b) está implementada.** O vigia só roda no DRY, e os três selos de apply detectam a volta de uma chamada a `_vigia_guarda()` que seja executada nas respectivas fixtures. O comentário ainda generaliza a regra das empresas ativas. Os dois arquivos da cópia conferem com os identificadores de conteúdo do diff fornecido. Mensagem e identidade do commit, execução da suíte e dados de produção [nao verificado]. [recalcular_fechamento.py:99](/home/ronald/codex_ws/026b415726b43c67766388fcbd7facce452d9fc1/app/ponto/management/commands/recalcular_fechamento.py:99) · [test_guarda_10_b.py:90](/home/ronald/codex_ws/026b415726b43c67766388fcbd7facce452d9fc1/app/ponto/tests/test_guarda_10_b.py:90) · `diff_vigia_seco.diff:2`, `:26`

### 1. Caminhos de execução

- **Entrada comum e DRY.** O comando resolve mês, ano, empresa e IDs, lê a foto inicial e imprime o universo. No DRY, imprime as rubricas e `recalcular_fechamento=0 (DRY -- nada escrito)`, chama `_vigia_guarda()` e retorna. O vigia percorre as chaves de `GUARDA_COMPETENCIA` no escopo de todas as empresas ativas e emite a contagem e as competências encontradas. Uma contagem positiva só altera o texto. Uma exceção na consulta ou na emissão ainda sobe pelo DRY. [recalcular_fechamento.py:104](/home/ronald/codex_ws/026b415726b43c67766388fcbd7facce452d9fc1/app/ponto/management/commands/recalcular_fechamento.py:104) · `recalcular_fechamento.py:109–125` · `fechamento.py:54–64`, `:1538–1539`
- **`--apply`.** O delta remove a chamada que ficava entre a tabela de rubricas e o cálculo de `mexidos`. A sequência é recálculo pela porta existente, foto posterior em memória, `processados=`, contador acumulado da guarda quando a competência está cadastrada, tentativa de gravar o JSON, tabela e contadores `fechamentos_mexidos=` e `fechamentos_novos=`, seguidos do detalhamento. `foto=` sai quando o JSON é gravado. O `except OSError` da foto já existia. [recalcular_fechamento.py:129](/home/ronald/codex_ws/026b415726b43c67766388fcbd7facce452d9fc1/app/ponto/management/commands/recalcular_fechamento.py:129) · `recalcular_fechamento.py:132–170` · `diff_vigia_seco.diff:17`
- **Apply indireto.** Na cópia, o único chamado de `_vigia_guarda()` está no DRY e o único chamado de produção a `guarda_sem_export()` está dentro desse método. `aplicar_janela_he_total` chama o filho com `apply=True` por empresa, depois faz os carimbos e a comparação da competência intacta. A consulta de exportação do próprio escritor e a leitura de `barrados_pela_guarda()` continuam no apply. A primeira ainda pode propagar falha antes do recálculo. O delta elimina o ponto de interrupção **pelo vigia** entre empresas. [aplicar_janela_he_total.py:144](/home/ronald/codex_ws/026b415726b43c67766388fcbd7facce452d9fc1/app/ponto/management/commands/aplicar_janela_he_total.py:144) · `recalcular_fechamento.py:104–107`, `:120–137` · `fechamento.py:67–71`, `:559–560`, `:578–595`

### 2. Quem escreve e quem lê

| Alvo | Escritor ou definição | Leitores na cópia |
|---|---|---|
| `guarda_sem_export=` | `_vigia_guarda` escreve no stdout. `recalcular_fechamento.py:99–107` | Asserções de presença no DRY e de ausência no apply. `test_guarda_10_b.py:69–74`, `:90–112` |
| `guarda_sem_export()` | `fechamento.py` define e devolve as chaves ordenadas. `fechamento.py:54–64` | `_vigia_guarda` importa e chama. Três testes chamam diretamente e dois casos de apply substituem a função por mock. `recalcular_fechamento.py:104–105` · `test_guarda_10_b.py:47–67`, `:84–85`, `:125–129` | [nao verificado]
| `_vigia_guarda()` | Método de `recalcular_fechamento.Command`. `recalcular_fechamento.py:99–107` | `handle` chama no DRY. Os testes chegam ao método via `call_command`. `recalcular_fechamento.py:120–125` · `test_guarda_10_b.py:69–74`, `:107–112` | [nao verificado]
| `processados=`, `foto=`, `fechamentos_mexidos=` | O comando imprime o retorno do serviço, o caminho do JSON gravado e a contagem de fechamentos que cruzam o limiar em alguma rubrica. `recalcular_fechamento.py:129–146`, `:157–162` | Os selos novos leem os marcadores. O selo O208 lê `foto()` e `RUBRICAS` diretamente. O wrapper delega sem capturar ou interpretar a saída do filho. `test_guarda_10_b.py:90–105` · `test_o208_contador_do_recalculo.py:25`, `:60–96` · `aplicar_janela_he_total.py:144–149` |
| `FOTO_DIR` | Global do comando, `/app/logs/recalculo`. `recalcular_fechamento.py:38` | O próprio apply resolve a global ao criar a pasta e montar o caminho. Os testes a substituem, e o relato referencia o JSON para reversão manual. `recalcular_fechamento.py:138–146` · `test_guarda_10_b.py:80–87`, `:122–129` · `app/docs/RELATO-ARQUIVO.md:18999–19001` |
| `aplicar_janela_he_total.PASTA` | Global do wrapper, `logs/janela_he`. `aplicar_janela_he_total.py:33` | O wrapper cria a pasta, grava carimbos e usa o caminho no aviso. O teste substitui a global. O relato aponta os carimbos para reversão. `aplicar_janela_he_total.py:72–84`, `:156–159` · `test_guarda_10_b.py:122–129` · `app/docs/RELATO-ARQUIVO.md:2455–2458` |

**Nenhum leitor executável desatualizado encontrado na cópia.** O teste antigo era o único leitor do literal no apply identificado nos anexos e foi substituído. Os dois comandos constam de `FORA_DE_PIPELINE`. O script `bin/diff_janela_he_total.sh` usa o mesmo nome de diretório para sua própria saída de diff. A instrução em `aplicar_col900` só imprime um comando manual. Crontab, selo externo e consumo pela sonda [nao verificado]. `diff_vigia_seco.diff:29` · `app/config/crons.py:769`, `:998–1003`, `:1148–1155` · `bin/diff_janela_he_total.sh:15`, `:36–42` · `aplicar_col900.py:250–252` · `fechamento.py:62`

### 3. Comentário e mensagem do commit [nao verificado]

- **Correto.** “Roda SO no DRY” e “O --apply nao o chama” descrevem a cópia. “O vigia so alarma, nunca para escrita” descreve sua ausência da cadeia de escrita. O DRY pode falhar se o vigia levantar. A antiga frase “ULTIMA linha” era falsa e saiu. `recalcular_fechamento.py:99–107`, `:120–129`, `:156–170` · `diff_vigia_seco.diff:5`, `:17` · `opus_final.md:3–4`
- **Impreciso.** “Linha da guarda sem exportacao vigente em nenhuma empresa ativa nao age” vale para o universo medido pelo vigia. Como afirmação geral, tem contraexemplo. `--apply --empresa <inativa>` consulta essa empresa explicitamente, sem filtro `ativa=True`. Se ela tiver exportação vigente e colaboradores elegíveis numa competência guardada, o serviço pode barrá-los enquanto o vigia acusa a linha. O teste de empresa inativa confirma apenas o lado do contador. [fechamento.py:1532](/home/ronald/codex_ws/026b415726b43c67766388fcbd7facce452d9fc1/app/ponto/services/fechamento.py:1532) · `fechamento.py:459–465`, `:559–560`, `:578–584` · `recalcular_fechamento.py:100`, `:129–131` · `test_guarda_10_b.py:60–67`
- **Mensagem do commit novo [nao verificado].** O anexo atribui a frase falsa ao commit antigo `401c3d52d`. A cópia veio sem metadados Git e o diff fornecido contém os patches, sem a mensagem nova. `opus_final.md:3` · `diff_vigia_seco.diff:1–5`

### 4. Mordida dos testes [nao verificado]

| Teste novo | Se `_vigia_guarda()` voltar a um ponto executado do apply | [nao verificado]
|---|---|
| `test_MORDE_no_apply_o_vigia_acusando_nao_roda` | **Cai.** A guarda sintética produz uma ocorrência real. `assertNotIn` vê a linha em qualquer posição do stdout. `test_guarda_10_b.py:43–45`, `:80–96` |
| `test_MORDE_no_apply_o_vigia_levantando_nao_interrompe` | **Cai.** O mock levanta na consulta. Uma chamada que execute antes, no meio ou depois dos contadores finais faz `call_command` falhar, inclusive se o texto final já tiver saído. `test_guarda_10_b.py:80–85`, `:98–105` · `recalcular_fechamento.py:104–105` |
| `test_no_dry_o_vigia_segue_acusando` | **Segue passando** se a regressão só acrescentar a chamada ao apply. Cai se o DRY perder a emissão. `test_guarda_10_b.py:107–112` |
| `test_MORDE_aplicar_janela_he_total_recalcula_todas_as_empresas_com_o_vigia_levantando` | **Cai.** A exceção no apply delegado sobe antes de o wrapper concluir. O teste exige as duas chamadas reais ao serviço, em ordem, e o carimbo `depois` da segunda empresa. `test_guarda_10_b.py:114–131` · `aplicar_janela_he_total.py:144–149` | [nao verificado]

**Os quatro alvos dos patches estão corretos.** `guarda_sem_export` e `recalcular_fechamento_mes` são importados dentro dos métodos, com o patch já ativo. O `wraps` pega a função real antes da substituição. `FOTO_DIR` e `PASTA` são lidas como globais dos respectivos módulos quando os arquivos são gravados. Os mocks de exceção cobrem a propagação. Uma chamada restrita a uma condição que as fixtures nunca atingem, ou uma consulta silenciosa com exceção engolida, poderia escapar. `recalcular_fechamento.py:104–110`, `:129–142` · `aplicar_janela_he_total.py:72–81` · `test_guarda_10_b.py:80–88`, `:119–131`

**As fotos e os carimbos dos testes novos ficam em diretórios temporários.** Há escritas no banco de teste. O teste de duas empresas usa a mesma pasta com relógio congelado. Como o nome de `foto=` tem precisão de segundos e omite a empresa, a segunda foto sobrescreve a primeira nessa fixture, dentro da pasta temporária. O selo prova as chamadas e o carimbo do wrapper. Uma execução com settings SaaS também pode abrir logs fora da pasta temporária. A suíte da casa usa `config.settings.ci`. `test_guarda_10_b.py:35–36`, `:80–88`, `:119–131` · `recalcular_fechamento.py:138–146` · `app/config/settings/saas.py:118–119`, `:136–142`, `:160–163` · `bin/suite.sh:373`

### 5. Checklist de reprovação

| Classe | Resultado no delta |
|---|---|
| Segundo juiz | **Não.** O vigia continua lendo `empresas_exportadas_no_escopo` → `marcos_da_competencia` → K6, registrado em `core/juizes.py:342`. `fechamento.py:64`, `:1520–1539`, `:1553–1561` |
| Segundo escritor fora da porta | **Não.** O wrapper delega e o comando continua chamando a porta existente. `aplicar_janela_he_total.py:144–146` · `recalcular_fechamento.py:129–131` |
| Relógio fora da borda | **Não introduz.** A leitura de relógio para nomear a foto já existia. `recalcular_fechamento.py:140–142` · `diff_vigia_seco.diff:17` |
| Derivação paralela da fonte única | **Não.** O contador usa K6 e `RUBRICAS` continua derivada de `CAMPOS`. `fechamento.py:64`, `:1539`, `:1561` · `recalcular_fechamento.py:34`, `:54` |
| Selo ou RED sem mordida | **Não para a regressão pedida.** Três selos pegam a chamada reintroduzida nos applies exercitados. O quarto protege a emissão no DRY. `test_guarda_10_b.py:90–131` |
| Ordem de trava | **Não muda.** A guarda efetiva continua antes do recálculo, e o wrapper mantém cadastro → recálculos → carimbos posteriores. `fechamento.py:559–595` · `aplicar_janela_he_total.py:116–149` |
| Pouso antes do pré-requisito | **[nao verificado].** O anexo põe a correção documental no commit de pouso. As linhas da reprova apontam para outros textos nesta cópia. Suíte verde, pouso e correção no merge [nao verificado]. `opus_final.md:5` · `CLAUDE.md:389–393`, `:419` · `app/docs/LEIS.md:156` |
| Campo com dois significados | **Não introduz.** `processados` é o retorno do escritor, `fechamentos_mexidos` vem da comparação das fotos e `guarda_sem_export` conta chaves da guarda. `fechamento.py:64`, `:1326–1335` · `recalcular_fechamento.py:105–107`, `:129–146`, `:157–163` |
| `except` novo | **0.** `RuntimeError` é injetado nos testes. O `except OSError` da foto é preexistente. `diff_vigia_seco.diff:5`, `:17`, `:29` · `recalcular_fechamento.py:147–150` · `test_guarda_10_b.py:100`, `:125` | [nao verificado]
| Fallback | **Nenhum novo.** O delta apaga a chamada do apply e deixa a consulta no DRY. `diff_vigia_seco.diff:17` · `recalcular_fechamento.py:120–125` |
| Constante de produção copiada para teste | **Não.** As chaves da guarda são sintéticas e os caminhos vêm de `TemporaryDirectory`. `test_guarda_10_b.py:30–45`, `:80–82`, `:122–124` · `fechamento.py:36–42` | [nao verificado]
| Aval incompleto ou excesso de escopo | **O comportamento pedido está coberto.** O delta contém a retirada da chamada do apply, o comentário de fluxo e os selos. A ressalva de exatidão é o escopo das empresas ativas no comentário. O delta fornecido não inclui novo diff de lista. Mensagem do commit, cobertura da emp4 pela v2 e a premissa de zero exportações em outubro [nao verificado]. `diff_vigia_seco.diff:1–29` · `recalcular_fechamento.py:100–103` · `opus_final.md:8–9` | [nao verificado]

## 25/11 19:21 · sessao 866b387ef747 · GUARDA-10-B · censo · origem=Code

# GUARDA-10-B · Censo VIGIA-SECO

Censo estático da árvore fornecida, em somente leitura. A árvore está no diretório nomeado `f226e77c1716ed2d2c0c4e78fad9cf209dfca727`. A correspondência com o commit está [não verificado], pois a cópia veio sem metadados Git. Execuções, crontab do host e scripts externos estão [não verificado].

## 1. Chamadores

| Alvo | Chamadores encontrados |
|---|---|
| `_vigia_guarda()` | Dois pontos de `recalcular_fechamento.Command.handle`: no dry, depois do resumo, e no `--apply`, depois da tabela de rubricas e antes dos contadores finais. `app/ponto/management/commands/recalcular_fechamento.py:123`, `:156`, `:162` |
| `guarda_sem_export()` | Em produção, `_vigia_guarda()`. Os três usos diretos em teste estão em `:51`, `:58` e `:67`. `app/ponto/management/commands/recalcular_fechamento.py:103`, `app/ponto/tests/test_guarda_10_b.py:51`, `:58`, `:67` |
| Command `recalcular_fechamento` com apply | O único chamador executável encontrado fora dos testes é `aplicar_janela_he_total --aplicar`, que faz `call_command(..., apply=True)` uma vez por empresa. O teste usa `apply=True` em `:80`. A entrada manual via `manage.py tenant_command` está documentada e o relato registra uso em produção. `app/ponto/management/commands/aplicar_janela_he_total.py:108`, `:145`, `app/ponto/tests/test_guarda_10_b.py:80`, `app/ponto/management/commands/recalcular_fechamento.py:3`, `app/docs/RELATO-ARQUIVO.md:18949` |

A busca em `bin/`, scripts, views e agendamentos da árvore não encontrou outra invocação executável do command com apply. Os dois commands estão em `FORA_DE_PIPELINE`. `aplicar_col900` imprime uma instrução de execução manual. O botão **Recalcular** e scripts como `bin/aplicar_o263_comp10.py` chamam o serviço `recalcular_fechamento_mes` diretamente. `app/config/crons.py:769`, `:998`, `:1148`, `app/ponto/management/commands/aplicar_col900.py:250`, `app/ponto/views.py:1280`, `bin/aplicar_o263_comp10.py:51`

## 2. Exceções e propagação

- **Leitura do vigia.** `guarda_sem_export()` percorre as chaves da guarda e, para cada competência, consulta todas as empresas ativas do schema. O caminho avalia `FechamentoMensal`, `ExportacaoDominio`, `PeriodoFechado` e `Holerite`, apesar de usar apenas o resultado `exportada`. Falhas de consulta ou conexão, como `OperationalError` ou `ProgrammingError`, podem subir de qualquer uma dessas leituras. A leitura de vigência faz uma consulta adicional quando encontra exportações. `app/ponto/services/fechamento.py:64`, `:1504`, `:1538`, `:1556`, `app/folha/export.py:588`, `app/folha/models.py:207`. [Django 5.2.14, `django/db/utils.py:70`](https://github.com/django/django/blob/5.2.14/django/db/utils.py#L70)
- **Outras falhas possíveis.** Chaves malformadas introduzidas na guarda podem causar `TypeError` ou `IndexError` na indexação e `TypeError` ou `ValueError` na conversão de mês e ano. A chave declarada é `(9, 2026)`. O `ValueError` explícito de exportações de outra tripla exige o argumento `exportacoes`, ausente nesta chamada. A escrita no stdout também pode falhar, por exemplo com `BrokenPipeError`. Uma contagem positiva só muda o texto emitido. `app/ponto/services/fechamento.py:36`, `:64`, `:1561`, `app/folha/export.py:581`, `app/ponto/management/commands/recalcular_fechamento.py:103`
- **Dry e apply na CLI.** Ambos deixam a falha subir de `handle`. O `except OSError` do apply cobre a foto. Na CLI, `tenant_command` encaminha para o `run_from_argv` do command, e `manage.py` delega ao Django. O Django trata `CommandError` na CLI, salvo com `--traceback`, e deixa subir as exceções acima, com traceback e saída de erro. `app/ponto/management/commands/recalcular_fechamento.py:123`, `:137`, `:146`, `:156`, `app/manage.py:18`. [django-tenants 3.9.0, `tenant_command.py:48`](https://github.com/django-tenants/django-tenants/blob/v3.9.0/django_tenants/management/commands/tenant_command.py#L48) · [Django 5.2.14, `base.py:403`](https://github.com/django/django/blob/5.2.14/django/core/management/base.py#L403)
- **`call_command`, inclusive via wrapper Python.** O Django chama `execute` diretamente e propaga a exceção ao chamador. `aplicar_janela_he_total` deixa a falha sair do laço. Os cadastros e suas trilhas já passaram antes do laço. O recálculo da empresa corrente já retornou e sua foto já foi tentada. A falha interrompe as empresas seguintes, os carimbos posteriores do comando pai e a prova da competência intacta. Na CLI SaaS, as gravações já concluídas ficam persistidas sob o autocommit padrão. `app/ponto/management/commands/aplicar_janela_he_total.py:116`, `:144`, `:148`, `:151`, `app/ponto/management/commands/recalcular_fechamento.py:128`, `:137`, `:156`, `app/config/settings/saas.py:83`. [Django 5.2.14, `management/__init__.py:194`](https://github.com/django/django/blob/5.2.14/django/core/management/__init__.py#L194) · [django-tenants 3.9.0, `tenant_command.py:61`](https://github.com/django-tenants/django-tenants/blob/v3.9.0/django_tenants/management/commands/tenant_command.py#L61) · [Django 5.2.14, `db/utils.py:161`](https://github.com/django/django/blob/5.2.14/django/db/utils.py#L161)

## 3. Leitores de `guarda_sem_export=` fora dos testes [nao verificado]

**Nenhum leitor automatizado encontrado na árvore.** O command escreve a linha no stdout. `aplicar_janela_he_total` chama sem capturar stdout e segue para seus próprios carimbos. Os leitores do literal encontrados são as asserções do teste. O código menciona uma sonda de produção, cuja implementação e eventual consumo dessa linha estão [não verificado]. `app/ponto/management/commands/recalcular_fechamento.py:105`, `app/ponto/management/commands/aplicar_janela_he_total.py:145`, `:148`, `app/ponto/tests/test_guarda_10_b.py:74`, `:82`, `:84`, `:85`, `app/ponto/services/fechamento.py:62`

## 4. Escritores dos insumos

| Insumo | Escritores encontrados |
|---|---|
| `GUARDA_COMPETENCIA` | O literal do módulo define a única entrada, `09/2026`. A mutação localizada é o `mock.patch.dict` do teste. `app/ponto/services/fechamento.py:36`, `app/ponto/tests/test_guarda_10_b.py:45` |
| `ExportacaoDominio` | `registrar()` cria o registro, chamado pela view `gerar_dominio_txt` e por `regerar_txt_dominio --aplicar`. Esse command também chama `invalidar()` no anterior. O vigia considera o registro mais recente por `(gerado_em, pk)` quando `invalidada_em` é nulo. `app/folha/models.py:158`, `:182`, `:220`, `app/folha/views.py:299`, `app/folha/management/commands/regerar_txt_dominio.py:116`, `:118`, `:122`, `app/folha/export.py:591` |
| `Empresa.ativa` | O formulário lê o checkbox e passa `ativa` na criação e na edição. `EmpresaAdmin` permite editar o campo. O bootstrap cria a empresa com o default `True`. `app/colaboradores/views.py:512`, `:540`, `:551`, `app/colaboradores/services/estrutura.py:13`, `:18`, `app/colaboradores/admin.py:7`, `:19`, `app/tenants/bootstrap.py:27`, `app/colaboradores/models.py:44` |

A porta `atualizar_empresa` usa `save()` da instância inteira. Ao gravar os campos da janela de HE, `aplicar_janela_he_total` também pode regravar o valor carregado de `ativa`. Os dois commands de diff fazem esses saves dentro de ensaios com rollback. `app/colaboradores/services/estrutura.py:18`, `app/ponto/management/commands/aplicar_janela_he_total.py:116`, `app/ponto/management/commands/diff_janela_he.py:74`, `:101`, `:120`, `app/ponto/management/commands/diff_janela_he_total.py:106`, `app/ponto/services/diff_frota.py:72`, `:89`

## 5. Efeito de deixar o vigia só no dry

- **Apply bem-sucedido.** Some `guarda_sem_export=` do stdout, inclusive dos applies delegados por `aplicar_janela_he_total`. O dry continua emitindo a linha. Nenhum consumidor automatizado dessa linha foi encontrado na árvore. `app/ponto/management/commands/recalcular_fechamento.py:105`, `:123`, `:156`, `app/ponto/management/commands/aplicar_janela_he_total.py:145`
- **Apply que falharia no vigia.** A retirada de `:156` elimina esse ponto de falha tardio. O command passa a alcançar `fechamentos_mexidos=`, `fechamentos_novos=` e o detalhamento por colaborador. O comando pai pode prosseguir para as empresas seguintes e para seus carimbos e prova final. `app/ponto/management/commands/recalcular_fechamento.py:156`, `:162`, `:164`, `app/ponto/management/commands/aplicar_janela_he_total.py:144`, `:148`, `:151`
- **Guarda efetiva.** O bloqueio de escrita consulta a exportação dentro do serviço, antes do recálculo. O apply também tem sua própria linha `GUARDA_COMPETENCIA ... barrado`, baseada na trilha de auditoria. Esses caminhos são independentes do vigia de stdout. Uma falha na consulta de exportação feita pelo próprio escritor ainda pode abortar o apply antes da escrita. `app/ponto/services/fechamento.py:67`, `:559`, `:580`, `app/ponto/management/commands/recalcular_fechamento.py:128`, `:133`

## 25/11 19:14 · sessao fe0e826240c7 · A-TELA-DO-DIA-LE-UM-JUIZ · verificador · origem=Code

1. **NAO.** Na cópia examinada, `_julgar_colab_corpo` é o único chamador direto de `_dia_novo_no_ato`. Os três carregadores que executam o corpo real carregam também o helper. Em `bin/`, a busca encontrou inspeção de AST, como a de `leitores_de_chave.py`, e nenhum carregador adicional desse corpo. [cartorio.py:1297](/home/ronald/codex_ws/cfab64bc1b871ad9ebf870e7ca8a20a901966d00/app/ponto/services/cartorio.py:1297), [cartorio.py:1931](/home/ronald/codex_ws/cfab64bc1b871ad9ebf870e7ca8a20a901966d00/app/ponto/services/cartorio.py:1931), [bin/leitores_de_chave.py:67](/home/ronald/codex_ws/cfab64bc1b871ad9ebf870e7ca8a20a901966d00/bin/leitores_de_chave.py:67).

   | Extrator executável | Conjunto extraído de `cartorio.py` |
   |---|---|
   | `app/ponto/tests/test_e3_nb_impressao_memoria.py:173` | `_julgar_colab_corpo`, `_dia_novo_no_ato`, `impressao_insumos`, `sem_adesao_no_horizonte`, `_cob_parcial_da_impressao`, `dispensar_por_ausencia`, `HORIZONTE_SEM_ADESAO`, `DISPENSA_POR_JUSTIFICATIVA`. |
   | `app/ponto/tests/x1_mutantes.py:23`, `:100` | `_julgar_colab_corpo`, `_dia_novo_no_ato`, `impressao_insumos`, `sem_adesao_no_horizonte`, `HORIZONTE_SEM_ADESAO`. O modo `--sem-banco` usa o carregador da linha anterior (`:77`). |
   | `app/chamados/tests/test_e3_pos_suite_sem_banco.py:135`, `:153`, `:174` | `impressao_insumos`, `sem_adesao_no_horizonte`, `_dia_novo_no_ato`, `_julgar_colab_corpo`. `literais()` carrega seis atribuições e o teste retém `HORIZONTE_SEM_ADESAO`. As outras cinco são `_MOTIVO_REJEICAO_NAO_PROTESTO`, `TEXTO_REJEICAO_NO_FECHO`, `PISO_EMISSAO`, `DISPENSA_POR_JUSTIFICATIVA` e `TETO_MARCO_MIN` (`app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:55`, `app/ponto/services/cartorio.py:42`, `:45`, `:298`, `:316`, `:796`, `:880`). |
   | `app/chamados/tests/test_e4b_c4_tranca_memoria.py:34`, `:138`, `:142` | `em_julgamento`, `colaborador_deste_julgamento`, `julgar_colab`. O invólucro recebe um corpo dublado que encerra a execução com `RuntimeError`. |
   | `app/chamados/tests/e4c_f5_voltaA_mutantes.py:17`, `:64` | `chamados_que_falam_de_batida`, recompilada no mutante `k24` com cópia dos globais originais. |
   | `app/ponto/tests/test_o219_versao_da_regra.py:246` | O laço de validação de `REGRA_VERSOES`. A inspeção de `_julgar_colab_corpo` no mesmo arquivo só percorre a AST (`:217`). |

   Com a importação de justificativas dublada, a E3 do ponto passou **24/24** e o caso S2/S3 de chamados passou **1/1**. Retirar somente o helper do carregador do ponto reproduziu **22 `NameError`**. Em execução direta, as duas suítes E3 ainda encontraram erros de importação de Django idênticos com o delta e com sua reversão em memória. O caso da tranca também passou com a borda dublada. `app/ponto/tests/test_e3_nb_impressao_memoria.py:173`, `app/chamados/tests/test_e3_pos_suite_sem_banco.py:174`, `app/ponto/services/cartorio.py:982`, `:1160`, `app/core/fmt.py:8`, `app/chamados/services/trava_do_colaborador.py:9`, `app/chamados/tests/test_e4b_c4_tranca_memoria.py:142`.

2. **PROCEDENTE.** O spy intercepta a B5.3b no mesmo global de módulo que `julgar_colab` usa na primeira passada. `_julga()` chama `julgar_celula` com `forcar=True`, e `julgar_celula` repassa esse argumento a `julgar_colab`. O `wraps` executa o corpo original em cada chamada registrada. `app/ponto/tests/test_a_tela_v18_protestos_gravado.py:245`, `app/ponto/tests/test_o261_cobrado_le_o_fio.py:294`, `app/ponto/services/cartorio.py:1015`, `:1048`, `:2113`.

   - **O congelamento cobre a fixture de classe.** O RED6 deriva diretamente de `TestCase`, instala a função emprestada como `classmethod` da própria classe e aplica `@freeze_time` à classe inteira. O decorador envolve `setUpClass`, que chama `setUpTestData`; é ali que a fixture lê `timezone.localdate()`. O instante literal coincide com o da O261. `app/ponto/tests/test_a_tela_v18_protestos_gravado.py:232`, `:238`, `app/ponto/tests/test_o261_cobrado_le_o_fio.py:217`, `:243`, `:258`, `app/colaboradores/tests/test_ui_painel_mostra_o_app_orm.py:56`, `app/requirements.txt:43`. Execução do RED6 com Django, Freezegun e ORM [nao verificado].
   - **O corte `[1:]` distingue as passadas.** A primeira já recebe `True`; depois dela, a B5.3b recebe `True` com o `out` principal e a O222 recebe `False` com `_conv`. Na sonda com o invólucro real e o corpo dublado, as duas rotas juntas registraram `[True, True, False]`. Com apenas a O222 após a primeira, registraram `[True, False]`, e a asserção rejeitou essa sequência. `app/ponto/services/cartorio.py:1002`, `:1015`, `:1048`, `:1105`, `app/ponto/tests/test_a_tela_v18_protestos_gravado.py:249`.

3. **NAO.** O delta muda o que os testes carregam e observam. Os escritores e os significados em produção seguem os do código abaixo. A autoridade do julgamento do dia continua em `julgar_celula`, e a lavra segue pela porta da célula. `app/docs/MAPA.md:26`, `app/docs/JUIZES.md:123`, `:133`, `app/ponto/portas/celula.py:1069`.

   | Campo ou valor | Quem escreve e quem lê |
   |---|---|
   | `out['reconciliados']` | `julgar_colab` soma os fechos adiados e o corpo soma os fechos de seus três ramos. O RED6 exige pelo menos um e o vigia agrega o total. `app/ponto/services/cartorio.py:1040`, `:1594`, `:1613`, `:1628`, `app/ponto/tests/test_a_tela_v18_protestos_gravado.py:247`, `app/ponto/services/vigia_de_hora.py:183`. |
   | `out['julgadas']`, `out['carimbadas']`, `out['nunca_bateu']` | `contadores()` começa em zero. O corpo conta por data após classificar, após `carimbar` devolver campos, ou após as guardas do ramo sem adesão, respectivamente. O RED6 lê os dois primeiros. `processar_cartorio` e o script O118 leem os três, e o vigia agrega `julgadas`. `app/ponto/services/cartorio.py:2022`, `:1517`, `:1350`, `:1931`, `:1297`, `app/ponto/tests/test_a_tela_v18_protestos_gravado.py:251`, `app/ponto/management/commands/processar_cartorio.py:67`, `bin/rejulgar_o118_col221.py:26`, `app/ponto/services/vigia_de_hora.py:183`. |
   | `_dias_do_ato`, `cods`, `out['protestos']` | `_dia_novo_no_ato` mantém os conjuntos por contador. O corpo escreve `cods`; a fábrica lê suas datas. No fim, `julgar_colab` une essas datas às de `_dias_do_ato['nunca_bateu']`, conta as células com coluna `protestos` cheia e limpa o estado do ato. O comando e o vigia leem o total. `app/ponto/services/cartorio.py:1938`, `:1519`, `:1061`, `:1124`, `:1129`, `app/ponto/management/commands/processar_cartorio.py:67`, `app/ponto/services/vigia_de_hora.py:183`. |
   | `CelulaDia.protestos`, `veredito`, `veredito_via` | `lavrar_veredito` escreve os campos e preserva a coluna quando prevalece a decisão do admin. `julgar_colab` lê a coluna para a contagem. A folha lê o veredito e a coluna para a retenção. `app/ponto/portas/celula.py:1097`, `:1111`, `:1129`, `app/ponto/services/cartorio.py:1125`, `app/folha/export.py:1273`, `:1286`. |
   | `forcar`, `call_count`, `call_args_list`, instante congelado | A fixture e o invólucro fornecem `forcar`; o corpo o lê na guarda de impressão. O spy registra as chamadas e o RED6 lê o registro. O decorador fornece o instante que a fixture consulta pelo relógio do Django. `app/ponto/tests/test_o261_cobrado_le_o_fio.py:258`, `:296`, `app/ponto/services/cartorio.py:1048`, `:1106`, `:1294`, `app/ponto/tests/test_a_tela_v18_protestos_gravado.py:232`, `:245`, `:249`. |

   **Fila preexistente.** A O222 mescla `cods` e soma `_conv['julgadas']` em `relidas_pos_emissao`; `julgadas` e `carimbadas` da releitura ficam fora dos totais principais. Continuam pendentes os mutantes do RED5 que incluem dias sem adesão pulados e dos DRY que contam coluna cheia apenas via admin. O RED5 zera os carimbos e força o ato; os DRY cobrem admin com coluna cheia e cartório com coluna vazia. Execução desses mutantes [nao verificado]. `app/ponto/services/cartorio.py:1105`, `app/ponto/tests/test_a_tela_v18_protestos_gravado.py:172`, `:187`, `:205`, `:217`, `app/ponto/tests/test_bateria_lavratura.py:1140`.

4. **NAO.** O delta fornecido se limita aos três carregadores de teste e ao RED6. As adições carregam o helper existente, fixam o instante e observam as chamadas. Nenhuma introduz `except`, fallback, juiz novo ou relógio solto, e nenhuma toca `ponto/motor_calculo_v2.py`. Os quatro blobs locais conferem com os pós-delta do anexo. `app/chamados/tests/test_e3_pos_suite_sem_banco.py:174`, `app/ponto/tests/test_e3_nb_impressao_memoria.py:173`, `app/ponto/tests/x1_mutantes.py:100`, `app/ponto/tests/test_a_tela_v18_protestos_gravado.py:232`, `:245`, `app/docs/JUIZES.md:29`. Identidade Git do commit na cópia e efeitos em produção [nao verificado].

VEREDITO: APROVA

## 25/11 18:49 · sessao e2a9e6323440 · A-TELA-DO-DIA-LE-UM-JUIZ · verificador · origem=Code

1. **PROCEDENTE — o delta quebra a suíte E3 de memória.** O corpo passou a chamar `_dia_novo_no_ato`, mas o carregador por AST da suíte deixa o helper fora. Com a borda de justificativas dublada para isolar a falha, reproduzi 22 `NameError` em 24 testes. Ao incluir apenas o helper real na extração em memória, os 24 passaram. O modo `--sem-banco` do verificador X1 usa essa suíte. `app/ponto/services/cartorio.py:1297`, `app/ponto/services/cartorio.py:1935`, `app/ponto/tests/test_e3_nb_impressao_memoria.py:56`, `app/ponto/tests/test_e3_nb_impressao_memoria.py:173`, `app/ponto/tests/x1_mutantes.py:77`.

2. **NAO — nenhum consumidor encontrado exige contagem por passada.** `contadores()` inicializa os quatro totais. O corpo incrementa `julgadas` após classificar, `nunca_bateu` após as guardas do ramo sem adesão e `carimbadas` nos dois ramos após retorno positivo da porta. `_dia_novo_no_ato` mantém um conjunto por contador. `julgar_colab` lê o conjunto de `nunca_bateu` para compor o universo e é o único acumulador de `out['protestos']`. `app/ponto/services/cartorio.py:1124`, `app/ponto/services/cartorio.py:1291`, `app/ponto/services/cartorio.py:1347`, `app/ponto/services/cartorio.py:1517`, `app/ponto/services/cartorio.py:1930`, `app/ponto/services/cartorio.py:1935`, `app/ponto/services/cartorio.py:2021`.

   `processar_cartorio` e o script O118 imprimem os quatro totais. O vigia soma e imprime `julgadas` **e** `protestos`. Seu teto usa `marcos_vencidos`, e o alarme do encadeado usa arquivos de conclusão e tempo de espera. `lavrar_status_competencia` lê contadores de status e de filas descartadas. O teste de férias exige pelo menos um julgamento ou carimbo por data e confere `julgada_em`. X1 cobra um carimbo no primeiro ato e zero no seguinte. Leitores externos dos logs [nao verificado]. `app/ponto/management/commands/processar_cartorio.py:43`, `app/ponto/management/commands/processar_cartorio.py:65`, `bin/rejulgar_o118_col221.py:24`, `app/ponto/services/vigia_de_hora.py:172`, `app/ponto/services/vigia_de_hora.py:182`, `app/ponto/management/commands/vigia_de_hora.py:28`, `bin/encadeado.sh:84`, `app/ponto/management/commands/lavrar_status_competencia.py:56`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:488`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:493`, `app/ponto/tests/test_x1_folga_sem_adesao.py:305`.

3. **PROCEDENTE — o selo de `out` passa. A O222 tem uma limitação preexistente.** O censo AST encontrou as cinco chaves declaradas, todas drenadas, com zero apelidos, chaves dinâmicas ou leituras indevidas do lote. `chave` é dinâmica apenas no dicionário interno de `_dias_do_ato`. Em memória, os 17 testes do selo e os 35 da catraca passaram com as bordas dubladas. O `MAPA.md` lista as mesmas cinco chaves na ordem do gerador. `app/ponto/services/cartorio.py:1127`, `app/ponto/services/cartorio.py:1938`, `app/ponto/services/cartorio.py:2018`, `app/ponto/tests/test_out_do_lote.py:195`, `app/ponto/tests/test_out_do_lote.py:266`, `app/core/tests/test_medida_turno_do_par.py:321`, `app/core/tests/test_medida_turno_do_par.py:562`, `app/core/management/commands/gerar_diagrama.py:891`, `app/docs/MAPA.md:27`.

   A O222 usa `_conv` próprio, mescla `cods` e soma `relidas_pos_emissao`. O escritor de `protestos` usa a coluna final e conta cada data elegível uma vez. `julgadas` e `carimbadas` de `_conv` ficam fora dos totais principais. Numa sonda com corpo dublado, um segundo dia julgado e carimbado só na releitura entrou em `cods` e `protestos`, enquanto `julgadas` e `carimbadas` ficaram em `1`. A ocorrência desse cenário com ORM ou em produção fica [nao verificado]. `app/ponto/services/cartorio.py:1091`, `app/ponto/services/cartorio.py:1100`, `app/ponto/services/cartorio.py:1124`.

4. **NAO — o ramo sem adesão exclui os dias pulados e o conjunto fica no ato.** `completar_status` e o pulo por impressão dão `continue` antes de registrar a data. O modo de complemento também retorna antes do escritor de `protestos`. Em dry, uma data que passa pelas guardas entra pelo conjunto de `nunca_bateu`, e o escritor lê a coluna vigente. A fábrica consulta apenas `cods` do ato. Confirmei com o invólucro real e corpo dublado que, mesmo com a mesma data em atos sucessivos, `_dias_do_ato` sai após retorno normal, complemento e exceção. `app/ponto/services/cartorio.py:1000`, `app/ponto/services/cartorio.py:1015`, `app/ponto/services/cartorio.py:1061`, `app/ponto/services/cartorio.py:1124`, `app/ponto/services/cartorio.py:1129`, `app/ponto/services/cartorio.py:1290`.

5. **PROCEDENTE — os quatro testes novos cobrem as regressões centrais, com lacunas.** Por inspeção, o RED5 derruba a retirada dos dias sem adesão do universo. O DRY K3 derruba zerar o total em todo dry, e o DRY cartório derruba contar `cods` cheios com a coluna vazia. Ambos passariam com um mutante que, em dry, só conta coluna cheia de admin. O RED5 também aceitaria incluir todos os dias sem adesão assim que um deles fosse julgado, pois força o ato após zerar os carimbos. `app/ponto/tests/test_a_tela_v18_protestos_gravado.py:171`, `app/ponto/tests/test_a_tela_v18_protestos_gravado.py:196`, `app/ponto/tests/test_a_tela_v18_protestos_gravado.py:204`, `app/ponto/tests/test_a_tela_v18_protestos_gravado.py:216`, `app/ponto/tests/test_bateria_lavratura.py:1140`.

   O RED6 cobra `julgadas=1` e `carimbadas=1`, mas `reconciliados` sobe antes da segunda chamada. Suprimir só essa chamada pode deixar o RED6 verde. O K4 anterior exige explicitamente as duas chamadas e derruba esse mutante. O RED6 também toma emprestados métodos da O261 sem o `freeze_time` da classe original, e a fixture calcula a data pelo relógio real. Execução dos quatro testes novos com ORM [nao verificado]. `app/ponto/tests/test_a_tela_v18_protestos_gravado.py:231`, `app/ponto/tests/test_a_tela_v18_protestos_gravado.py:291`, `app/ponto/services/cartorio.py:1044`, `app/ponto/services/cartorio.py:1626`, `app/ponto/tests/test_o261_cobrado_le_o_fio.py:217`, `app/ponto/tests/test_o261_cobrado_le_o_fio.py:258`.

6. **NAO — no delta fornecido, a mudança de produção é nos contadores.** As duas chamadas a `carimbar` continuam antes da deduplicação, com os mesmos argumentos. A lavra mantém a decisão do admin e sua coluna congelada. A retenção da folha lê o veredito e `CelulaDia.protestos`. A falha dupla do ato e do `write` segue no `finally` preexistente. Identidade Git da cópia e efeitos medidos em dinheiro ou estado de produção [nao verificado]. `app/ponto/services/cartorio.py:1323`, `app/ponto/services/cartorio.py:1347`, `app/ponto/services/cartorio.py:1908`, `app/ponto/services/cartorio.py:1930`, `app/ponto/services/cartorio.py:2112`, `app/ponto/portas/celula.py:1097`, `app/folha/export.py:1273`, `app/folha/export.py:1286`.

VEREDITO: REPROVA

## 25/11 18:26 · sessao 09ded850d4c4 · A-TELA-DO-DIA-LE-UM-JUIZ · verificador · origem=Code

1. **NAO — o delta não introduz escritor, `except` ou quebra no selo.** A única mudança de produção desde `3acc7452b` troca a soma por entrada em `cels` pela cardinalidade do conjunto de datas elegíveis. A data repetida de (B) passa de dois para um. O escritor continua na forma `out[K] = out.get(K, 0) + n`, e os incrementos de `carimbadas` continuam na forma `+=`. O `except` da precedência já existia no `main`. Em memória, os 17 testes do selo passaram com as bordas dubladas, e os 35 da catraca de linhas passaram. Identidade Git da cópia e execução dos casos com ORM [nao verificado]. `app/ponto/services/cartorio.py:1123`, `:1345`, `:1924`, `:2109`, `160173bb1:app/ponto/services/cartorio.py:2097`, `app/ponto/tests/test_out_do_lote.py:123`, `:195`, `:305`, `app/core/tests/test_medida_turno_do_par.py:321`, `:562`.

2. **PROCEDENTE — os cinco testes novos mordem as regressões abaixo.** Os sete testes das classes sem ORM passaram em memória no HEAD. As duas linhas marcadas como inspeção descrevem a mordida esperada dos testes com ORM. `app/ponto/tests/test_a_tela_v18_protestos_gravado.py:142`, `:156`, `:179`, `:256`.

   | Teste novo | Mutação que o derruba | [nao verificado]
   |---|---|
   | K1 positivo | Zerar a contribuição dos dias lavrados pelo cartório faz o contador divergir da contagem positiva consultada no banco. Inspeção. `app/ponto/tests/test_a_tela_v18_protestos_gravado.py:142`, `:151`, `app/ponto/services/cartorio.py:1123` |
   | K5 sem adesão | Restaurar `carimbadas += 1` incondicional após `carimbar` derruba o zero da segunda passada forçada. Na sonda com corpo e porta reais e persistência dublada, `1, 0` virou `1, 1`. `app/ponto/tests/test_a_tela_v18_protestos_gravado.py:156`, `:165`, `app/ponto/services/cartorio.py:1342`, `app/ponto/portas/celula.py:1227` |
   | P2 com dois dias | `int(any(...))` devolve `1` onde o teste exige `2`. Confirmado em memória. `app/ponto/tests/test_a_tela_v18_protestos_gravado.py:236`, `:244`, `app/ponto/services/cartorio.py:1123` |
   | P2 com data repetida | Voltar ao `sum(1 for ...)` devolve `2` onde o teste exige `1`. Confirmado em memória. `app/ponto/tests/test_a_tela_v18_protestos_gravado.py:246`, `:253`, `app/ponto/services/cartorio.py:1123` |
   | K6 com ato que cai | Suprimir a entrega do buffer na falha mantém o `ValueError`, mas deixa a lista de linhas vazia. Confirmado em memória. `app/ponto/tests/test_a_tela_v18_protestos_gravado.py:287`, `:305`, `app/ponto/services/cartorio.py:2099` |

   Retirar o filtro por data julgada cai no **P4 preexistente**, que inclui um dia pulado de coluna cheia. Um mutante que soma zero em todo `apply_=False` passa pelos cinco casos novos, que exercitam `apply_=True`. Confirmei a sobrevivência nos casos sem ORM. O K6 novo usa `linhas.append` e deixa descoberta a falha dupla. `app/ponto/tests/test_a_tela_v18_protestos_gravado.py:226`, `:192`, `:306`, `app/ponto/tests/test_bateria_lavratura.py:1140`, `app/ponto/services/cartorio.py:1121`.

3. **PROCEDENTE — (A) segue aberto como pergunta de lei.** Na sonda em memória, uma célula admin sem batida apurável saiu com `carimbadas=1`, `protestos=0` e sem `cods`, preservando o veredito `trabalhou` e a coluna `['FURO_PARCIAL']`. O ramo pode gravar ata, status e carimbo. A omissão do contador ocorre no fechamento e não altera essas gravações nem alimenta a retenção da folha, que lê o veredito e a coluna. O K5 novo parte de coluna vazia. A íntegra do contrato que resolveria a precedência entre K5 e a seção 9 fica [nao verificado]. `app/ponto/services/cartorio.py:1230`, `:1294`, `:1319`, `:1342`, `:1123`, `app/ponto/portas/celula.py:1097`, `app/folha/export.py:1273`, `:1286`, `app/ponto/tests/test_a_tela_v18_protestos_gravado.py:3`, `:156`, `app/ponto/tests/test_bateria_lavratura.py:1162`.

4. **PROCEDENTE — (C) mascara a falha do ato.** Na sonda, o ato bufferizou duas linhas e levantou `ValueError`. O `write` levantou `OSError` na primeira linha. Saiu o `OSError`, o `ValueError` ficou em `__context__`, a segunda linha se perdeu e a precedência não rodou. Nesse caminho, o `finally` chama o callback e não grava a célula. A falha também interrompe o chamador, inclusive antes da emissão de eventos de prazo no vigia. Na outra situação, em que o ato termina e só o `write` falha, a v18 já pode ter gravado `ata.precedencia`. Efeito efetivo em dinheiro ou estado em produção [nao verificado]. `app/ponto/services/cartorio.py:2094`, `:2099`, `:2106`, `:2114`, `app/ponto/portas/celula.py:1242`, `app/ponto/services/vigia_de_hora.py:122`, `:182`, `:185`.

5. **PROCEDENTE — (E) é contagem por passada no B5.3b.** O rejulgamento forçado usa o mesmo `out`. `julgadas` sobe a cada classificação e `carimbadas` pode subir duas vezes se ambas as chamadas à porta devolverem campos. O222 usa `_conv` e mantém os dois subtotais lá. A duplicação dos números não causa nova gravação ou mudança no dinheiro. A segunda passada em si pode mudar a coluna e o veredito final, pois relê o efeito da reconciliação e volta a lavrar. O teste K4 de B5.3b exige duas chamadas e só verifica a deduplicação de `protestos`. `app/ponto/services/cartorio.py:1044`, `:1105`, `:1511`, `:1619`, `:1901`, `:1922`, `app/ponto/tests/test_a_tela_v18_protestos_gravado.py:213`, `app/ponto/tests/test_o261_cobrado_le_o_fio.py:405`.

6. **NAO — nenhum leitor verificado quebra teto ou alarme por causa do delta.** `contadores()` inicializa as duas chaves. `julgar_colab` é o único acumulador de `protestos` e lê o saldo anterior na própria atribuição. O corpo escreve `carimbadas` nos dois ramos, condicionado ao retorno da porta. `processar_cartorio` acumula por empresa e imprime os dois valores. O script O118 também imprime os dois. Ambos carregam células de um colaborador, com unicidade por data. O vigia julga uma célula por chamada, agrega só `protestos` dessas duas chaves e imprime o total. O teto vem de `marcos_vencidos`, e o alarme do encadeado usa a presença de arquivos de conclusão e o tempo de espera. Entre os testes, O222 e v18 leem `protestos`, X1 e v18 leem `carimbadas`, e férias soma `julgadas + carimbadas` e confere a gravação de `julgada_em`. Consumidores externos dos logs [nao verificado]. `app/ponto/services/cartorio.py:1123`, `:1345`, `:1924`, `:2004`, `:2095`, `app/ponto/management/commands/processar_cartorio.py:43`, `:61`, `:67`, `bin/rejulgar_o118_col221.py:21`, `:27`, `app/escala/models.py:1846`, `app/ponto/services/vigia_de_hora.py:172`, `:182`, `app/ponto/management/commands/vigia_de_hora.py:28`, `bin/encadeado.sh:84`, `app/ponto/tests/test_bateria_lavratura.py:1224`, `app/ponto/tests/test_x1_folga_sem_adesao.py:311`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:488`, `:497`, `app/ponto/tests/test_a_tela_v18_protestos_gravado.py:154`, `:167`.

VEREDITO: REPROVA

## 25/11 18:19 · sessao b894452c553d · A-TELA-DO-DIA-LE-UM-JUIZ · verificador · origem=Code

**A v18 não cumpre integralmente a opção B.** O ramo sem adesão pode lavrar uma célula admin com `protestos` congelado e cheio, mas deixa a data fora de `cods`. O contador devolve zero para esse dia. Reproduzi o caminho em memória, com o corpo e a porta da cópia e a persistência dublada. `app/ponto/services/cartorio.py:1122`, `app/ponto/services/cartorio.py:1294`, `app/ponto/services/cartorio.py:1319`, `app/ponto/portas/celula.py:1097`.

Comparei a árvore fornecida com a cópia local de `160173bb1`. As duas alterações e o teste adicionado coincidem com os blobs do diff. A identidade do commit por metadados Git, a execução da suíte com ORM e a incidência dos achados em produção ficam **[nao verificado]**. `app/ponto/services/cartorio.py:1123`, `app/escala/models.py:1761`, `app/ponto/tests/test_a_tela_v18_protestos_gravado.py:1`.

## 1. Quem escreve e quem lê

| Chave | Escrita e leitura no HEAD |
|---|---|
| `out['protestos']` | `contadores()` inicializa em zero. `julgar_colab` tem o único incremento e lê o acumulado via `.get('protestos', 0)` nessa mesma expressão. O lote da empresa reutiliza o `out` entre colaboradores. O vigia soma os retornos avulsos via `r.get(k)`. A linha `fim` e o script O118 imprimem o valor. O command do vigia imprime o agregado. `app/ponto/services/cartorio.py:2004`, `app/ponto/services/cartorio.py:1123`, `app/ponto/management/commands/processar_cartorio.py:43`, `app/ponto/management/commands/processar_cartorio.py:65`, `app/ponto/services/vigia_de_hora.py:182`, `app/ponto/management/commands/vigia_de_hora.py:28`, `bin/rejulgar_o118_col221.py:26`. |
| `out['carimbadas']` | `contadores()` inicializa. Os dois ramos do corpo incrementam quando `carimbar` retorna campos. B5.3b usa o mesmo `out`. O222 usa `_conv` e deixa esse subtotal nele. Os leitores operacionais são a linha `fim` e o script O118. Os leitores de teste estão no item 5. `app/ponto/services/cartorio.py:2004`, `app/ponto/services/cartorio.py:1342`, `app/ponto/services/cartorio.py:1922`, `app/ponto/services/cartorio.py:1048`, `app/ponto/services/cartorio.py:1105`, `app/ponto/management/commands/processar_cartorio.py:66`, `bin/rejulgar_o118_col221.py:26`. |
| `out['cods']` | O ramo normal grava `data → lista da classificação`, inclusive `[]`, junto ao incremento de `julgadas`. B5.3b pode sobrescrever a data. O222 mescla as chaves de `_conv`. Durante o ato, a presença de chaves aciona o cluster e seleciona as células da fábrica e do contador. No `finally`, o mapa volta ao lote por data, com a última passada por cima. Assim, após o retorno, o mapa compartilhado pode conter datas de atos anteriores. `app/ponto/services/cartorio.py:1511`, `app/ponto/services/cartorio.py:1048`, `app/ponto/services/cartorio.py:1061`, `app/ponto/services/cartorio.py:1070`, `app/ponto/services/cartorio.py:1107`, `app/ponto/services/cartorio.py:1122`, `app/ponto/services/cartorio.py:1142`. |

Os leitores externos de `cods` que encontrei são testes. Em `app/ponto/tests/`: `test_cods_do_ato.py:81` e `:110`, `test_fnd_pergunta_a_juiza.py:64`, `test_c1_cartorio_pergunta_a_juiza.py:33`, `test_x1_folga_sem_adesao.py:117`, `:159`, `:167`, `:205`, `:214`, `:237`, `test_o261_cobrado_le_o_fio.py:130`, `:312`, `:339`, `:352`, `test_cobparc_marco_no_periodo.py:262`, `:269`, `:329`, `:343`, `:356`, `test_e3_status_indefinida_a2_na_lavra.py:67`, `test_veredito_celula.py:132` e `:156`, `test_hx_cartorio_ressuscita.py:62`, `test_dst_l2_cartorio_pergunta_ao_juiz.py:99` e `:118`, e `test_a_tela_v18_protestos_gravado.py:84`, `:91`, `:206`. Em `app/chamados/tests/`: `test_cobparc5_parcial_sem_periodo_so_lista.py:141` e `test_cobertura_parcial_sem_periodo_hoje_nao_emite.py:78` e `:83`. A sombra registra o uso antigo de `cods` no docstring e hoje espia a porta. `app/ponto/management/commands/celula_veredito_velho.py:13`, `app/ponto/management/commands/celula_veredito_velho.py:118`.

**Coluna `CelulaDia.protestos`.** A atribuição operacional é da porta `lavrar_veredito`. No fluxo do cartório, `carimbar` persiste os campos recebidos da lavra `save=False`. A lavra admin conserva os códigos existentes e a lavra do cartório sobre admin só pode gravar ata e status. O desfazimento de julgamento futuro é outro ponto de escrita **na mesma porta** e zera a coluna para via não admin. Os testes também escrevem diretamente para montar fixtures. `app/ponto/portas/celula.py:1097`, `app/ponto/portas/celula.py:1129`, `app/ponto/portas/celula.py:1236`, `app/ponto/portas/celula.py:1149`, `app/ponto/portas/celula.py:1192`, `app/ponto/portas/celula.py:1202`, `app/ponto/tests/test_bateria_lavratura.py:1162`.

- **Leitores da coluna:** a própria porta, para comparação e trilha, e o cartório, para completar status e contar. `app/ponto/portas/celula.py:1082`, `app/ponto/portas/celula.py:1113`, `app/ponto/portas/celula.py:1137`, `app/ponto/services/cartorio.py:952`, `app/ponto/services/cartorio.py:1123`.
- **Folha e vigias de passivo:** folha lê os códigos na retenção e na exceção da pausa. Furos sem cobrança exige `veredito='furo'`. Cadastro lê os códigos junto de `indefinida` ou `furo`. O c2 lê a coluna sem filtro de via nesse predicado, aplicado para nascer ou renascer fora da costura com chamado vivo. Admin fica fora de `VEREDITOS_ACUSAM` na retenção. Impacto do congelado no c2 em produção **[nao verificado]**. `app/folha/export.py:1173`, `app/folha/export.py:1273`, `app/folha/export.py:1286`, `app/folha/services/vigia_pausa.py:156`, `app/ponto/services/furos_sem_cobranca.py:55`, `app/escala/services/cadastro_realidade.py:514`, `app/escala/services/cadastro_realidade.py:654`, `app/escala/services/cadastro_realidade.py:660`, `app/chamados/reconciliador.py:989`, `app/escala/models.py:1772`, `app/escala/models.py:1780`.
- **Diagnóstico e fotos:** o command `supra_juiz` lê a lavra e tira dela o código do texto da retratação. Seu placar e alarme usam a classificação própria. A sombra de veredito inclui a coluna na foto de reversão, e a foto E4B inclui todos os campos concretos da célula. `app/ponto/management/commands/supra_juiz.py:165`, `app/ponto/management/commands/supra_juiz.py:235`, `app/ponto/management/commands/supra_juiz.py:243`, `app/ponto/management/commands/supra_juiz.py:250`, `app/ponto/management/commands/celula_veredito_velho.py:108`, `app/chamados/services/foto_e4b.py:251`, `app/chamados/services/foto_e4b.py:269`.

**Efeito nos leitores do contador.** `processar_cartorio` e O118 exibem o número. O vigia soma e exibe. Nesses leitores, o número muda, mas o código não o usa para decidir teto, emissão ou alarme. O rótulo `protestos` continua sem dizer a unidade, inclusive no `[DRY]`. O texto que descreve o significado antigo consta como registro histórico. A bateria O222 já exige zero quando o protesto sai da coluna, e o selo `out_do_lote` verifica a forma da acumulação e o isolamento das chaves, sem exigir a semântica antiga. Nenhum leitor operacional da coluna acima usa o contador. Consumidores externos dos logs **[nao verificado]**. `app/ponto/management/commands/processar_cartorio.py:66`, `bin/rejulgar_o118_col221.py:26`, `app/ponto/services/vigia_de_hora.py:172`, `app/ponto/services/vigia_de_hora.py:183`, `app/ponto/management/commands/vigia_de_hora.py:29`, `app/docs/RELATO-ARQUIVO.md:18076`, `app/ponto/tests/test_bateria_lavratura.py:1224`, `app/ponto/tests/test_bateria_lavratura.py:1247`, `app/ponto/tests/test_out_do_lote.py:123`, `app/ponto/tests/test_out_do_lote.py:195`.

## 2. Universo, identidade e retornos

- **Fluxo normal.** Os dois ramos lavram o `cel` do próprio laço. Os produtores de `rejulgar` e de adiados guardam esse objeto. O E2 pode carimbar perguntas do colaborador, mas o wrapper só transfere para B5.3b o `_cel_a` já presente na fila. O222 recebe a lista original. Os signals de chamado e de ponto consultados barram novo julgamento durante o ato. `app/ponto/services/cartorio.py:1239`, `app/ponto/services/cartorio.py:1319`, `app/ponto/services/cartorio.py:1381`, `app/ponto/services/cartorio.py:1571`, `app/ponto/services/cartorio.py:1589`, `app/ponto/services/cartorio.py:1608`, `app/ponto/services/cartorio.py:1622`, `app/ponto/services/cartorio.py:1701`, `app/ponto/services/cartorio.py:1717`, `app/ponto/services/cartorio.py:1746`, `app/ponto/services/cartorio.py:1901`, `app/ponto/services/cartorio.py:1025`, `app/ponto/services/cartorio.py:1042`, `app/ponto/services/cartorio.py:1106`, `app/chamados/reconciliador.py:696`, `app/chamados/signals.py:210`, `app/ponto/signals.py:85`.
- **Há entradas que quebram a equivalência.** A unicidade no banco é por `(colaborador, data)`, e o command carrega um queryset dessa pessoa. `julgar_colab` aceita uma lista com o mesmo objeto repetido ou duas instâncias da mesma linha. O mapa colapsa a data e a soma percorre as duas entradas. Com duas entradas cheias obtive `protestos=2` para uma chave. Um `out` já preenchido com `rejulgar` ou com adiados também é aceito na entrada. A fila pode levar um objeto externo a B5.3b, inclusive outra instância da mesma data, enquanto a soma lê o objeto original. Reproduzi a passagem de uma data externa pré-enfileirada com corpo dublado. Ocorrência dessas entradas em produção **[nao verificado]**. `app/escala/models.py:1846`, `app/ponto/management/commands/processar_cartorio.py:61`, `app/ponto/services/cartorio.py:1000`, `app/ponto/services/cartorio.py:1018`, `app/ponto/services/cartorio.py:1044`, `app/ponto/services/cartorio.py:1123`, `app/ponto/services/cartorio.py:1512`.
- **Chaves e saídas.** No caminho normal, cada chave local veio de uma classificação nesta chamada. O mapa antigo sai antes do corpo. A exceção pode deixar `cods` parcial no `out` e saltar o incremento, pois a classificação entra antes da lavra e o `finally` devolve as chaves já registradas. O único retorno antecipado do `try` é `completar_status`. O `return` do ramo sem adesão sai do corpo e o wrapper chega ao escritor, com a data ausente. `app/ponto/services/cartorio.py:1000`, `app/ponto/services/cartorio.py:1015`, `app/ponto/services/cartorio.py:1123`, `app/ponto/services/cartorio.py:1142`, `app/ponto/services/cartorio.py:1346`, `app/ponto/services/cartorio.py:1512`, `app/ponto/services/cartorio.py:1901`.

## 3. `apply_=False`

Antes, o ramo normal somava classificações não vazias. Agora, entre as datas classificadas, soma as que **já tinham a coluna cheia no objeto recebido**. Um furo novo com coluna vazia passa de `1` para `0`. Uma classificação vazia com coluna anterior cheia passa de `0` para `1`. `cods` continua expondo a simulação. O ramo sem adesão continua fora do contador. `160173bb1, app/ponto/services/cartorio.py:1748`, `app/ponto/services/cartorio.py:1121`, `app/ponto/services/cartorio.py:1294`, `app/ponto/services/cartorio.py:1512`, `app/ponto/services/cartorio.py:1898`.

Leem esse número em dry-run `processar_cartorio` sem `--apply`, `vigia_de_hora` sem `--apply` e O118 com `APLICAR=False`. A sombra `celula_veredito_velho` chama com `apply_=True` dentro de transação, intercepta a lavra e reverte o ensaio. `lavrar_status_competencia` lê os contadores de status, e os testes que usam o dry-run para provar classificação leem `cods`. `app/ponto/management/commands/processar_cartorio.py:27`, `:40`, `:65`, `app/ponto/management/commands/vigia_de_hora.py:19`, `:28`, `app/ponto/services/vigia_de_hora.py:182`, `bin/rejulgar_o118_col221.py:24`, `app/ponto/management/commands/celula_veredito_velho.py:121`, `:133`, `:144`, `:156`, `app/ponto/management/commands/lavrar_status_competencia.py:56`, `:60`, `app/ponto/tests/test_o261_cobrado_le_o_fio.py:411`.

## 4. `write` e precedência

No vigia, as linhas de um julgamento saem antes da soma daquele retorno e do resumo do command. O callback operacional é `self.stdout.write`. Em `furos_sem_cobranca.cobrar`, o callback opcional entrega as linhas antes da consulta de cobrança viva. O command desse serviço chama `cobrar` sem callback. Os dois chamadores de teste com `linhas.append` consultam a lista depois do retorno. Nesses chamadores, o código não usa a linha intermediária para dirigir o ato. Um callback que levanta continua abortando o chamador, agora depois da precedência quando o julgamento termina normalmente. `app/ponto/services/vigia_de_hora.py:182`, `app/ponto/management/commands/vigia_de_hora.py:28`, `app/ponto/services/furos_sem_cobranca.py:117`, `app/ponto/services/furos_sem_cobranca.py:128`, `app/ponto/management/commands/cobrar_furos_sem_cobranca.py:30`, `app/ponto/tests/test_dst_l2_cartorio_pergunta_ao_juiz.py:84`, `app/ponto/tests/test_dst_l2_jz1_rejeita_justificativa.py:132`, `app/ponto/services/cartorio.py:2106`, `app/ponto/services/cartorio.py:2114`.

**A falha dupla muda a exceção visível.** Reproduzi em memória um ato que acumula duas linhas e levanta `ValueError`, seguido de um `write` que levanta `OSError` na primeira linha. Sai o `OSError`, o `ValueError` fica em `__context__`, e a segunda linha não é entregue. A precedência fica fora desse caminho de falha do ato. `app/ponto/services/cartorio.py:2094`, `app/ponto/services/cartorio.py:2099`, `app/ponto/services/cartorio.py:2106`.

## 5. `carimbadas` e `julgadas`

Os leitores operacionais encontrados exibem os dois números. O teste de férias **usa** `julgadas + carimbadas >= 1` como prova de que um dia foi processado, especificamente no ramo sem adesão, e também exige um `julgada_em` novo. Como exige escrita real, o novo critério preserva o sentido dessa asserção. O selo X1 espera `1` na primeira lavra e `0` na passada pulada. O teste v18 exige explicitamente `julgadas=3`, `carimbadas=0` na segunda passagem forçada. `app/ponto/management/commands/processar_cartorio.py:66`, `bin/rejulgar_o118_col221.py:26`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:485`, `:493`, `:497`, `app/ponto/tests/test_x1_folga_sem_adesao.py:305`, `:311`, `:316`, `app/ponto/tests/test_a_tela_v18_protestos_gravado.py:137`.

A unidade efetiva de `carimbadas` é o retorno não vazio de `carimbar` nas passadas que usam o `out` principal. Se B5.3b volta ao mesmo dia e muda a impressão ou a lavra, ambas as passadas podem contar. O subtotal de O222 fica em `_conv`, e `carimbar_precedencia` também fica fora desse contador. `app/ponto/services/cartorio.py:1048`, `:1105`, `:1922`, `:2108`, `app/ponto/portas/celula.py:1226`, `:1236`, `:1242`.

## 6. K1–K6, P1–P4 e mordida

| Caso | O que a cobertura deixa passar |
|---|---|
| **K1** | O teste com esse nome verifica o furo que termina com coluna vazia. Falta o caso de cartório que termina realmente com `['FURO_PARCIAL']` e deve somar `1`. O corpo pode lavrar esse código mesmo quando a emissão está calada pelo piso. `app/ponto/tests/test_a_tela_v18_protestos_gravado.py:107`, `app/ponto/services/cartorio.py:1775`, `:1861`, `:1901`. |
| **K2/K3** | Os testes exigem `cods` divergente da coluna no admin e exercitam o ato com e sem O222. A asserção `relidas_pos_emissao > 0` vale para o lote. Ela passa mesmo quando só o dia 23 é relido, pois a data admin pode vir da primeira passada. Também não há asserção de status ou tela no caso admin. `app/ponto/tests/test_a_tela_v18_protestos_gravado.py:41`, `:65`, `:80`, `:88`, `app/ponto/services/cartorio.py:1106`, `:1441`. |
| **K4/P4** | Os dublês de RED-2 classificam `d22` em todas as chamadas e ignoram a identidade da célula passada ao corpo. O teste de B5.3b passa com duas chamadas e `julgadas=2` para um dia, porque só exige `protestos=1`. No corpo real, B5.3b incrementa `julgadas` de novo e pode voltar a carimbar. A asserção intitulada P4 mede `protestos <= len(cods)`, sem medir os dois contadores pedidos por P4. `app/ponto/tests/test_a_tela_v18_protestos_gravado.py:168`, `:176`, `:189`, `:206`, `app/ponto/services/cartorio.py:1048`, `:1511`, `:1922`. |
| **K5/P2** | RED-4 cria batidas para todos os dias, portanto exercita o ramo com adesão. Passa com o incremento incondicional restaurado só no ramo sem adesão. Também fica de fora o contraexemplo admin congelado e cheio sem batidas apuráveis. Nesse cenário, obtive `carimbadas=1`, `protestos=0` e ausência de `cods` em memória. A porta de retratação permite que a última batida deixe o universo da adesão. `app/ponto/tests/test_a_tela_v18_protestos_gravado.py:129`, `app/ponto/services/cartorio.py:1230`, `:1319`, `:1342`, `:1123`, `app/ponto/portas/celula.py:1097`, `app/ponto/registro_batida.py:287`. |
| **K6/P1** | RED-3 prova a ordem das chamadas no caminho feliz do ato com saída quebrada. A precedência é dublada, e o teste passa sem provar a persistência ou a entrega de linhas quando o ato falha. O teste P1 verifica somente os dois contadores na repetição. Passaria com efeito lateral ou trilha nova que deixasse esses contadores iguais. `app/ponto/tests/test_a_tela_v18_protestos_gravado.py:119`, `:212`, `:220`, `:230`, `app/ponto/services/cartorio.py:2099`. |
| **P3** | Os testes novos não chamam tela, PDF, folha ou vigia. A tela lê D4. O PDF lê o miolo compartilhado do espelho, que dispensa D4. A folha consulta a lavra na retenção. O vigia de hora literalmente chama o cartório e soma o retorno. Assim, a leitura literal de P3, com todos lendo a mesma coluna e ninguém julgando, não descreve esses caminhos. `app/ponto/tests/test_a_tela_v18_protestos_gravado.py:65`, `app/colaboradores/services/calendario.py:639`, `app/ponto/services/espelho.py:159`, `app/relatorios/pdf_espelho.py:596`, `app/folha/export.py:1273`, `app/ponto/services/vigia_de_hora.py:182`. |

**Mutantes.** Trocar a soma por `int(any(...))` preserva todos os valores exigidos nas asserções de `protestos` encontradas, que são `0` ou `1`, mas limita cada ato a um dia contado. Esse mutante e o que suprime a entrega no `finally` passaram nos quatro testes novos sem ORM que executei em memória. O m3, que antecipa a entrega, caiu no RED-3. Por inspeção, m1, contar pela classificação, cai nos casos K2/K3. O m2 literal incide sobre a base, onde havia `_prot0`. Na v18, a regressão equivalente de fechar a contagem antes da O222 é coberta pelo caso de furo que termina com coluna vazia. Execução dos casos com ORM **[nao verificado]**. `app/ponto/tests/test_a_tela_v18_protestos_gravado.py:84`, `:91`, `:115`, `:125`, `:183`, `:196`, `:205`, `:234`, `app/ponto/tests/test_bateria_lavratura.py:1224`, `:1247`, `160173bb1, app/ponto/services/cartorio.py:1111`, `160173bb1, app/ponto/services/cartorio.py:1116`.

## 7. Classes de reprovação

- **Segundo juiz e derivação paralela.** A v18 reaproveita o corpo em B5.3b e O222. A classificação continua vindo de `classificar_dia`, o par previsto para a lavra vem da porta, e a precedência pergunta a `fatos_do_dia`. A soma lê a coluna uma vez no fechamento do ato. O defeito está na delimitação por `cods`, que perde o ramo sem adesão. `app/ponto/services/cartorio.py:1048`, `:1106`, `:1123`, `:1499`, `:1542`, `:2048`, `app/docs/JUIZES.md:127`, `app/core/juizes.py:207`.
- **Segundo escritor e ordem de trava.** A coluna segue pelos pontos da porta descritos no item 1. O diff conserva o `atomic` do fecho, os saves da porta e a precedência após `julgar_colab`. A mudança de ordem é a entrega do callback, que no vigia continua dentro do advisory lock. `app/ponto/services/cartorio.py:1032`, `:2094`, `:2106`, `:2114`, `app/ponto/portas/celula.py:994`, `:1238`, `app/ponto/services/vigia_de_hora.py:146`, `:182`, `:186`.
- **Relógio.** A v18 conserva as leituras de `localdate()` e `now()` da entrada avulsa, inclusive o `now()` da recusa do dia corrente quando `agora` foi injetado. O escritor do contador e o buffer não consultam o relógio. `160173bb1, app/ponto/services/cartorio.py:2064`, `:2085`, `:2088`, `app/ponto/services/cartorio.py:1123`, `:2065`, `:2086`, `:2095`.
- **ESMERIL-SEMÂNTICA.** `CelulaDia.protestos` é uma lista de códigos. `out['protestos']` é uma contagem de dias e, em dry-run, lê a coluna anterior. `out['cods']` guarda a classificação produzida. O comentário do modelo identifica a distinção. As linhas operacionais exibem só `protestos`, e o texto histórico usa a acepção de classificação. `app/escala/models.py:1761`, 

[truncado em 20000 de 20752 caracteres]

## 25/11 17:08 · sessao 3745ac9f17f3 · GUARDA-10-B · sonda · origem=Code

Sim. A empresa 4 tem FechamentoMensal e DiaPago motor na competência 10/2026. (sonda/anexos:3, sonda/anexos:4)  
Empresa 1 — FechamentoMensal 4, DiaPago motor 49. (sonda/anexos:3, sonda/anexos:4)  
Empresa 3 — FechamentoMensal 119, DiaPago motor 2102. (sonda/anexos:3, sonda/anexos:4)  
Empresa 4 — FechamentoMensal 23, DiaPago motor 450. (sonda/anexos:3, sonda/anexos:4)  
Empresa 2 — FechamentoMensal 478, DiaPago motor 8817. (sonda/anexos:3, sonda/anexos:4)

_8253 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
