# CODEX-ESTADO

> Ultima resposta processada: **27/11/2026 17:10** · publicado em **27/11/2026 17:14** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 27/11 17:10 · sessao 85d4f254d751 · RAMO2-HERANCA-VAO · censo · origem=Code

# Dossiê RAMO2-HERANCA-VAO — censo de mão única

O aval cobre a herança do vão em `MotorBase.calcular_mes`, em fatia própria, com construtor depois do CCM, DIFF de frota da comp. 11 por rubrica antes do pouso e **PAREI se mudar dia fora da herança**. O pouso da PAR depende desta fatia. `aval.txt:1` · `LEIS-2711-1610.txt:1(B)` [nao verificado]

A referência é a cópia entregue como `59600163ccc9cffb8f3ae148beede7cd7866ae86`. A confirmação do SHA por Git, a medição pela porta de fechamento e o alcance na frota são [nao verificado]. O recibo fornece dois ensaios diretos de `Motor12x36.calcular_mes`, sem ID de colaborador ou cadastro completo. `recibo_c2v.md:25` · `recibo_c2v.md:28`

## 1. Mapa do cálculo atual

| Passo | O que a cópia faz |
|---|---|
| **1. Obter o dia do juiz** | Faz cópia das batidas. Antes de montar os períodos, pede os turnos ao juiz com marcos do template ou DNA do dia, conforme a alimentação. Mapeia cada entrada localizada para `Turno.data_turno` e disponibiliza esse mapa ao filtro da ata durante a montagem. `app/ponto/motor_calculo_v2.py:2381` · `app/ponto/motor_calculo_v2.py:894` · `app/ponto/motor_calculo_v2.py:911` · `app/ponto/motor_calculo_v2.py:2459` · `app/ponto/motor_calculo_v2.py:711` |
| **2. Aplicar a herança** | Para cada `(entrada, saída)`, `data_jornada` começa com o dia do juiz, com degradação para o dia civil local da entrada. Se houver saída real do período anterior e o vão até a entrada atual estiver entre **0 e 6 h inclusive**, `data_jornada` recebe `_prev_dj`. O estado guarda o dia já herdado, o que permite propagação em cadeia. Um período aberto deixa `_prev_fim = None`. `app/ponto/motor_calculo_v2.py:2494` · `app/ponto/motor_calculo_v2.py:2513` · `app/ponto/motor_calculo_v2.py:2518` · `app/ponto/motor_calculo_v2.py:2536` |
| **3. Recortar a janela** | Atualiza o estado anterior **antes** das duas guardas. Um par que ficou fora ainda pode transmitir seu dia ao próximo. Se o dia resultante cair antes de `data_ini` ou depois de `data_fim`, o par inteiro é pulado antes de calcular seus minutos. A meia-noite civil não divide o par nesse laço. `app/ponto/motor_calculo_v2.py:2522` · `app/ponto/motor_calculo_v2.py:2536` · `app/ponto/motor_calculo_v2.py:2542` · `app/ponto/motor_calculo_v2.py:2571` |
| **4. Calcular e atribuir** | Nos pares admitidos, o dia herdado orienta a consulta de folga, o tratamento de dia sem previsto e o mapa `id(periodo) → data_jornada`. O `calcular_periodo` escreve a duração apurada. A janela de HE e a dobra por par podem ler o dia original do juiz, enquanto a admissão e a folga já usaram o dia herdado. `app/ponto/motor_calculo_v2.py:2558` · `app/ponto/motor_calculo_v2.py:2570` · `app/ponto/motor_calculo_v2.py:2605` · `app/ponto/motor_calculo_v2.py:1646` · `app/ponto/motor_calculo_v2.py:2898` · `app/ponto/motor_calculo_v2.py:2911` |

**Caso A, 0 h no início.** Com `ini−1 18:00–22:00` e `ini 03:00–11:00`, o vão de 5 h faz o segundo par herdar `ini−1`. Na janela `[ini, fim]`, os dois pares ficam antes do início. O recibo obtém `{}`. O aval pede **480 min em `ini`**. `app/ponto/motor_calculo_v2.py:2518` · `app/ponto/motor_calculo_v2.py:2542` · `recibo_c2v.md:26` · `aval.txt:1`

**Caso B, 960 min em 20/11.** `20/11 22:00 → 21/11 06:00` tem 480 min e pertence ao dia 20. O par `21/11 11:00–19:00` começa 5 h depois e também recebe o dia 20. A janela do recibo que contém o dia 20 retém ambos e devolve `{20/11: 960}`. A janela seguinte os descarta e devolve `{}`. O aval pede **480 min em 20/11 e 480 min em 21/11**, cada par contado uma vez. `app/ponto/motor_calculo_v2.py:2516` · `app/ponto/motor_calculo_v2.py:2521` · `app/ponto/motor_calculo_v2.py:2542` · `recibo_c2v.md:27` · `aval.txt:1`

O sítio executável da herança nesta cópia começa em `app/ponto/motor_calculo_v2.py:2513`. A faixa aproximada `2610–2640` do aval cai aqui no BO-B, que identifica pares fechados para creditar trabalho em dia de turno aberto. O BO-B consome o mapa produzido pela herança e pode escrever minutos no primeiro período aberto do dia. `app/ponto/motor_calculo_v2.py:2608` · `app/ponto/motor_calculo_v2.py:2627` · `app/ponto/motor_calculo_v2.py:2672` · `aval.txt:1`

## 2. Censo de escritores e leitores

| Campo ou estrutura | Escritores | Leitores |
|---|---|---|
| **`Turno.data_turno` e mapa do juiz** | `_data_do_turno` responde pela entrada e pelo envelope da véspera. `_mk` grava no `Turno`. `_dia_do_turno` projeta `instante local → dia`. A base expõe o mapa temporário da ata, depois guarda mapa e turnos na instância. `app/ponto/turnos.py:248` · `app/ponto/turnos.py:312` · `app/ponto/motor_calculo_v2.py:911` · `app/ponto/motor_calculo_v2.py:2465` · `app/ponto/motor_calculo_v2.py:2509` | Filtro da ata, dia inicial do laço, janela L-097, consultas L-084, feriado por par, BO-B e os reagrupamentos de Partido e Comercial. `app/ponto/motor_calculo_v2.py:711` · `app/ponto/motor_calculo_v2.py:2516` · `app/ponto/motor_calculo_v2.py:963` · `app/ponto/motor_calculo_v2.py:1646` · `app/ponto/motor_calculo_v2.py:1696` · `app/ponto/motor_calculo_v2.py:2635` · `app/ponto/motor_calculo_v2.py:2911` · `app/ponto/motor_calculo_v2.py:3037` · `app/ponto/motor_calculo_v2.py:3380` · `app/ponto/motor_calculo_v2.py:3442` |
| **Pares, pausas e anomalias** | `periodos_de_turnos` projeta os turnos em pares e pausas por entrada. `_periodos_pelo_marco` pode substituir os pares e as pausas do dia pela ata e ordena a saída pela entrada. `app/ponto/adaptador_motor.py:13` · `app/ponto/adaptador_motor.py:28` · `app/ponto/motor_calculo_v2.py:711` · `app/ponto/motor_calculo_v2.py:748` | A base registra anomalias e entrega ao cálculo do par as pausas daquela entrada. As pontas efetivas e pausas usadas são carimbadas no período e lidas pelo calculador de segmentos. `app/ponto/motor_calculo_v2.py:2467` · `app/ponto/motor_calculo_v2.py:2470` · `app/ponto/motor_calculo_v2.py:2571` · `app/ponto/motor_calculo_v2.py:1947` · `app/ponto/calculador/alimentacao.py:73` |
| **Herança, `_folga_do_par` e listas de resultado** | `_prev_fim`, `_prev_dj` e `_gap_h` são locais ao cálculo. A base grava a folga por instante da entrada, o dia por identidade do período e põe o período em `periodos` ou `periodos_ft`. `app/ponto/motor_calculo_v2.py:2420` · `app/ponto/motor_calculo_v2.py:2494` · `app/ponto/motor_calculo_v2.py:2518` · `app/ponto/motor_calculo_v2.py:2570` · `app/ponto/motor_calculo_v2.py:2605` | As guardas de janela leem `data_jornada`. A janela e o bloqueio sem previsto leem `_folga_do_par`. BO-B, presença para falta/DSR, agrupamento O73 e teto de pontualidade leem o mapa local. O reflexo semanal de HE na base ainda consulta a data civil da entrada. `app/ponto/motor_calculo_v2.py:2542` · `app/ponto/motor_calculo_v2.py:1562` · `app/ponto/motor_calculo_v2.py:1640` · `app/ponto/motor_calculo_v2.py:2629` · `app/ponto/motor_calculo_v2.py:2689` · `app/ponto/motor_calculo_v2.py:2749` · `app/ponto/motor_calculo_v2.py:2807` · `app/ponto/motor_calculo_v2.py:2831` |
| **Dono do minuto apurado** | A atribuição é **um dia por período** em `resultado.dia_da_jornada[id(p)]`. O teto publica/mescla o mapa, Partido também o escreve diretamente e o fechamento mescla os mapas das fatias. O `_dono_do_dia` do fechamento é outro mapa, `dia → ID do vínculo`. `app/ponto/motor_calculo_v2.py:2573` · `app/ponto/motor_calculo_v2.py:1117` · `app/ponto/motor_calculo_v2.py:3071` · `app/ponto/services/fechamento.py:209` · `app/ponto/services/fechamento.py:331` | Classificação de FT no fechamento, lavra de `DiaPago`, porta de exportação, calculador e DIFF. `DiaPago._dia_de` usa o mapa quando há chave e recorre à data civil local da entrada quando falta. `app/ponto/services/fechamento.py:959` · `app/ponto/services/fechamento.py:1241` · `app/ponto/services/dia_pago.py:109` · `app/folha/porta_export.py:106` · `app/ponto/calculador/alimentacao.py:73` · `app/ponto/calculador/alimentacao.py:193` · `app/ponto/calculador/alimentacao.py:239` · `app/ponto/calculador/periodo.py:596` · `app/ponto/management/commands/diff_calculador.py:214` |
| **`minutos_trabalhados`** | Zero inicial em `PeriodoCalculo`. Escrevem os corpos de 12x36, Partido e Comercial. O bloqueio sem previsto pode transferir o valor para `minutos_fora_janela` e zerá-lo. BO-B pode creditar pares fechados ao aberto. A releitura V3 também escreve no período. `app/ponto/motor_calculo_v2.py:213` · `app/ponto/motor_calculo_v2.py:2898` · `app/ponto/motor_calculo_v2.py:3009` · `app/ponto/motor_calculo_v2.py:3372` · `app/ponto/motor_calculo_v2.py:1585` · `app/ponto/motor_calculo_v2.py:2672` · `app/ponto/calculador/periodo.py:745` | Totais normal e FT, teto L-093, BO-B, agregações de HE do Partido/Comercial, fechamento, `DiaPago`, espelho, exportação e instrumentos. `app/ponto/motor_calculo_v2.py:296` · `app/ponto/motor_calculo_v2.py:300` · `app/ponto/motor_calculo_v2.py:1276` · `app/ponto/motor_calculo_v2.py:2667` · `app/ponto/motor_calculo_v2.py:3061` · `app/ponto/motor_calculo_v2.py:3447` · `app/ponto/services/fechamento.py:862` · `app/ponto/services/fechamento.py:978` · `app/ponto/services/dia_pago.py:178` · `app/ponto/services/dia_pago.py:239` · `app/ponto/services/espelho.py:136` · `app/folha/porta_export.py:111` · `app/folha/porta_export.py:550` · `app/ponto/management/commands/diff_calculador.py:253` |

**Quem passa pelo mesmo laço mensal.** `Motor12x36`, `Motor12x36ComEscala`, `MotorPlantao` e `MotorIntermitente` herdam o método. Plantão delega o cálculo do par a 12x36. Intermitente, com vínculo identificado como tal, tem guarda própria na consulta de folga. Comercial e Partido chamam `super().calcular_mes` e só depois reagrupam pelo dia do juiz os períodos recebidos. PartidoNoturno herda Partido. Assim, o reagrupamento dos dois overrides não alcança um par já descartado pela base. `app/ponto/motor_calculo_v2.py:2843` · `app/ponto/motor_calculo_v2.py:3628` · `app/ponto/motor_calculo_v2.py:3652` · `app/ponto/motor_calculo_v2.py:3657` · `app/ponto/motor_calculo_v2.py:3677` · `app/ponto/motor_calculo_v2.py:3686` · `app/ponto/motor_calculo_v2.py:2478` · `app/ponto/motor_calculo_v2.py:2560` · `app/ponto/motor_calculo_v2.py:3024` · `app/ponto/motor_calculo_v2.py:3056` · `app/ponto/motor_calculo_v2.py:3431` · `app/ponto/motor_calculo_v2.py:3442`

## 3. Leis e cortes herdados

- **L-085, dia do turno.** A jornada e a cauda real pertencem ao dia de início, inclusive para feriado, domingo, semana e DSR. O corte de dia do turno usa o marco previsto de início, com regra de madrugada quando a véspera tem marco. O PDF lê o pertencimento dado pela célula/juiz e apresenta a batida não reclamada no dia civil. O corte NOTURNO-A põe a âncora de HE no dia do turno e protege a volta de pausa de atraso ou falta pelo marco de entrada. `app/docs/LEIS.md:117` · `app/docs/CORTES.md:328` · `app/docs/CORTES.md:329` · `app/docs/CORTES.md:646`
- **TETO TEMPORAL, L-073.** Julgar por instante ou por fato encerrado, inclusive na virada. O O93 exige saída real para a herança do período anterior. A L-115 dá a saída pela posição quando há pausa fechada e última batida depois dela. A L-093 limita o desconto de pontualidade ao tempo que faltou. `app/docs/LEIS.md:100` · `CLAUDE.md:192` · `app/ponto/motor_calculo_v2.py:2536` · `app/docs/LEIS.md:153` · `app/docs/LEIS.md:111`
- **L-097, janela de HE.** A emenda RAMO2 preserva trabalhado e noturno após o marco de saída. A ponta segue para classificação de HE, sujeita à decisão da pendência. No feriado, o anexo C fixa **240 min trabalhados** tanto com escala certa quanto incerta. A certeza escolhe a rubrica. Para domingo previsto vale a L-086, com o feriado regido pelo cadastro do ciclo e pelo dia de início. `app/docs/LEIS.md:115` · `app/docs/LEIS.md:118` · `LEIS-2711-1610.txt:1(C)`
- **Cadastro e fronteira com a PAR.** L-024 dá o previsto à grade, L-084 trata `CADASTRO x REALIDADE` quando as duas pontas se afastam mais de 3 h e L-087 distingue o instante da batida do papel julgado pela ata. A PAR V2 tem critérios próprios de ponta ancorada, teto cadastral de jornada e separação a partir de 11 h. Os 360 min encontrados no laço são um limiar do código desta cópia. `app/docs/LEIS.md:75` · `app/docs/LEIS.md:116` · `app/docs/LEIS.md:119` · `app/docs/CORTES.md:637` · `app/ponto/motor_calculo_v2.py:2520`
- **Prova e pouso.** A L-110 exige esperado pela regra aplicada ao cadastro e às batidas. A L-114 dá à bateria o papel de certificado e ao DIFF o de medição de impacto. A L-117 leva decisão nova ao topo como pergunta numerada. O aval desta obra fixa o alvo do DIFF nos dias de herança. `app/docs/LEIS.md:136` · `app/docs/LEIS.md:138` · `app/docs/LEIS.md:154` · `aval.txt:1`

## 4. Contrato de entrada

| Item | Contrato |
|---|---|
| **Fonte** | `Batida` por `colaborador_id`, ID, `timestamp` e papel apurável. Para a porta real entram também janela da empresa, vigência do vínculo, template, célula/DNA/ata, folgas, ausências, feriados, decisões de HE e régua da fatia. Encadeamento de referência `recalcular_fechamento_mes → apurar_por_fatia → motor.calcular_mes`. `app/ponto/models.py:22` · `app/ponto/turnos.py:2941` · `app/ponto/services/fechamento.py:639` · `app/ponto/services/fechamento.py:754` · `app/ponto/services/fechamento.py:774` · `app/ponto/services/fechamento.py:306` · `app/ponto/motor_calculo_v2.py:2403` · `app/ponto/motor_calculo_v2.py:1569` |
| **Unidade** | Instantes locais com segundos para as pontas, diferença em horas para o limiar do vão, minutos para o par e dia local para jornada, fatia e competência. Na lavra, `horas_*` em horas e os três campos `minutos_*` da grade em minutos. `app/ponto/motor_calculo_v2.py:73` · `app/ponto/motor_calculo_v2.py:2519` · `app/ponto/motor_calculo_v2.py:2874` · `app/ponto/services/dia_pago.py:34` |
| **Universo** | Todas as classes efetivamente escolhidas. A janela inclusiva vem do corte da empresa. A busca recebe um dia de contexto em cada ponta da competência e de cada fatia. A fatia respeita vigência, início da operação e dia do vínculo resolvido pela célula. `app/ponto/janelas.py:10` · `app/ponto/janelas.py:54` · `app/ponto/services/fechamento.py:209` · `app/ponto/services/fechamento.py:225` · `app/ponto/services/fechamento.py:264` · `app/ponto/services/fechamento.py:646` · `app/ponto/motor_calculo_v2.py:3743` |
| **Exclusões e bordas** | `batidas_apuraveis` exclui retratadas e inclui pendentes. O motor filtra tipos fora de `E/S` e duplicatas técnicas. Saída órfã vira anomalia e aberto carrega `saída=None`. Batidas de outro vínculo dentro da fatia são excluídas pelo filtro da fatia. Os dias de contexto entram para parear e só contribuem se a atribuição passar pelas guardas. `app/ponto/turnos.py:2941` · `app/ponto/motor_calculo_v2.py:924` · `app/ponto/adaptador_motor.py:24` · `app/ponto/services/fechamento.py:275` · `app/ponto/motor_calculo_v2.py:2542` |

## 5. Matriz de transições para RED

Os casos A e B vêm do recibo. As demais linhas definem controles com cadastro de teste explícito. Os esperados são de regra. Execução e estado RED/GREEN na porta real [nao verificado]. `recibo_c2v.md:25` · `app/docs/LEIS.md:136` · `app/docs/LEIS.md:154`

| Caso | Cadastro e batidas | Resposta da regra |
|---|---|---|
| **A. Novo turno na abertura da janela** | 12x36 do aval. Par anterior `D−1 18:00–22:00` e novo par `D 03:00–11:00`. Na fixture, os pares são independentes. | **480 min em D**, com o par anterior no próprio dono se apurado na janela anterior. O vão de 5 h fica fora do trabalhado. `aval.txt:1` · `recibo_c2v.md:26` · `app/docs/LEIS.md:117` |
| **B. Dois donos na virada** | 12x36 do aval. `20/11 22:00 → 21/11 06:00` e novo par `21/11 11:00–19:00`. Apurar as duas janelas e também uma janela que contenha os dois dias. | **480 min no dia 20 e 480 min no dia 21**, total 960 min, cada par uma vez. Com corte 21, o dia 20/11 pertence à comp. 11 e o dia 21/11 à comp. 12. `aval.txt:1` · `recibo_c2v.md:27` · `app/docs/LEIS.md:117` · `app/ponto/janelas.py:10` |
| **C. Retorno real de pausa** | Noturno `19:00–07:00`, pausa `04:30–05:30`, batidas nos quatro marcos. Feriado só em D+1. | **660 min em D**, incluindo os 90 min da volta, e zero de dobra por feriado de D+1. `app/docs/LEIS.md:117` · `app/docs/CORTES.md:646` · `app/ponto/tests/test_o280_bateria_da_lei.py:1531` · `app/ponto/tests/test_o280_bateria_da_lei.py:1580` |
| **D. Bordas do vão** | Variar o vão entre pares para `0`, `6h` e `6h00m01s`. Declarar na fixture se são do mesmo turno ou de jornadas distintas, inclusive em virada de fatia. | Quando o cadastro e as batidas estabelecem continuação, o trabalho fica no dia da jornada. Quando estabelecem novo turno, cada jornada fica no seu início. A expectativa para dados que não estabelecem essa distinção fica na pergunta 1. `app/docs/LEIS.md:117` · `app/docs/CORTES.md:637` · `app/ponto/motor_calculo_v2.py:2520` |
| **E. Período anterior aberto** | Entrada sem saída, seguida por par fechado de outro turno estabelecido pelo cadastro e pelas batidas. | O aberto não fornece saída para o vão. O par fechado cabe no próprio dia e a pontualidade do turno em aberto segue o teto por fato encerrado. `app/docs/LEIS.md:100` · `app/ponto/motor_calculo_v2.py:1200` · `app/ponto/tests/test_o93_dia_da_jornada.py:100` |
| **F. Um único plantão na borda** | 12x36 previsto `20/11 19:00 → 21/11 07:00`, um turno completo, corte 21. | Trabalho do plantão atribuído uma vez à comp. 11, no dia 20, com zero desse turno na comp. 12. `app/docs/LEIS.md:117` · `app/ponto/janelas.py:10` · `app/ponto/tests/test_dupla_contagem_dia20.py:30` |
| **G. Saída no marco ou após o marco** | Turno que cruza, saída no marco e variantes de saída posterior, com os mesmos marcos e decisões de HE declaradas. | O dono continua sendo o dia de início. A cauda trabalhada e noturna é preservada quando cabível. A HE da ponta segue a decisão L-097. O limite Comercial desta RAMO2-B é 480, e a pontualidade pelo dia do turno tem obra própria. `app/docs/LEIS.md:115` · `app/docs/LEIS.md:117` · `LEIS-2711-1610.txt:1(A–B)` · `recibo_c2v.md:18` |
| **H. Feriado e certeza da escala** | Mesmo par de 240 min em feriado nas duas fixtures, uma com escala certa e outra com escala incerta. | **240 min trabalhados em cada fixture**. A certeza só decide a rubrica. `LEIS-2711-1610.txt:1(C)` |

## 6. Padrão nomeado

**`padrao=fonte unica`**, *Single Source of Truth*. A pergunta é a pertença do par ao dia da jornada. A autoridade de turno está registrada em `core/juizes.py`, e a duplicidade observada está entre a resposta desse juiz e o dia que o laço mensal atribui por vão. O rótulo identifica o padrão para o dossiê. Sua realização no futuro DIFF é [nao verificado]. `app/docs/GLOSSARIO.md:11` · `app/docs/PLANO-PADROES.md:49` · `app/docs/PLANO-PADROES.md:61` · `app/core/juizes.py:216` · `app/ponto/motor_calculo_v2.py:2516`

## 7. Arquivos e cruzamento com obras em voo

| Arquivo | Cruzamento |
|---|---|
| **`app/ponto/motor_calculo_v2.py`** | É o arquivo comum com `RAMO2-B-EM-TURNO-QUE-CRUZA`, da cópia `wt-codex-ramo2-b-em-turno-que-cruza-221946`. A herança está no laço da base, antes de `_janela_do_par` e dos consumidores de trabalhado e base de HE. A RAMO2-B trata o recorte da saída e, segundo o c2v, também tocou limite mensal do Comercial, `_julgar_pontualidade` e a flag do Intermitente. O recibo registra mudanças anteriores no corpo do Partido e deixa a herança aberta no item 7. A colisão exata de linhas com o SHA c2v é [nao verificado]. `recibo_c2v.md:3` · `recibo_c2v.md:16` · `recibo_c2v.md:20` · `recibo_c2v.md:22` · `recibo_c2v.md:25` · `recibo_c2v.md:31` · `app/ponto/motor_calculo_v2.py:2513` · `app/ponto/motor_calculo_v2.py:2571` · `app/ponto/motor_calculo_v2.py:1865` · `app/ponto/motor_calculo_v2.py:3504` · `app/docs/RELATO.md:188` |
| **`app/core/tests/test_medida_turno_do_par.py`** | O c2v tocou `LINHAS_REGISTRADAS`. A cópia entregue registra `heranca_por_gap` na linha 2521 do motor e tem detector estrutural dedicado. É contato de verificação, além do contato no motor. `recibo_c2v.md:13` · `app/core/tests/test_medida_turno_do_par.py:250` ·

[truncado em 20000 de 25011 caracteres]

## 27/11 17:08 · sessao a0860a10b010 · - · - · origem=Ronald (tmux)

- A L-085 atribui a jornada inteira ao dia de início, inclusive a cauda legítima após a meia-noite. A L-073 exige instante ou fato encerrado. A L-097 preserva o trabalhado e o noturno após o marco de saída e reserva à janela a classificação de HE. O HEAD ainda recorta a saída quando o marco já cai na madrugada do turno que cruza. `app/docs/LEIS.md:100,115,117`, `CLAUDE.md:192`, `app/ponto/janela_he.py:181-200`.
- O corte de dia do turno consulta o marco inicial previsto e condiciona a madrugada à existência de marco na véspera. A PAR V2 distingue continuidade por âncora de 180 min e limite de jornada da empresa e determina separação com vão de 11 h ou mais. A cláusula de 6 h pertence à implementação atual. `app/docs/CORTES.md:329,637`, `app/ponto/motor_calculo_v2.py:2489-2545`.
- Os REDs literais são 480 min em `ini`, 480 min em 20/11 e 480 min em 21/11, cada par no respectivo dono. Controles pela regra incluem retorno verdadeiro de pausa com 660 min no dia de início, par anterior aberto sem fim presumido e turno que cruza 20→21 mas pertence a uma única competência. `aval.txt:1`, `recibo_c2v.md:25-28`, `app/ponto/tests/test_o280_bateria_da_lei.py:1531-1545,1568-1582`, `app/ponto/tests/test_o93_dia_da_jornada.py:100-117`, `app/ponto/tests/test_dupla_contagem_dia20.py:9-14,30-44`.
- Padrão nomeado **fonte única**. A obra adjacente MOTOR-LE-O-TURNO registra o padrão **juiz único** e o caso de continuidade ainda sujeito à evidência de cadastro e operação. `app/docs/GLOSSARIO.md:11-12`, `app/docs/PLANO-PADROES.md:61-67`, `app/docs/BACKLOG.md:1998,2265`.
- A sonda precisa usar o universo do escritor, janelas por empresa e bordas por fatia, batidas vigentes e as rubricas diárias enumeradas, com ajustes mensais separados. O comparador agregado existente só percorre fechamentos gravados e perde os ausentes de um dos lados. `app/ponto/services/fechamento.py:258-291,448-464,639-653`, `app/ponto/turnos.py:2941-2968`, `app/ponto/services/dia_pago.py:34-47`, `app/ponto/services/diff_frota.py:45-88,101-135`.
- Os resultados dos três REDs literais estão decididos. Perguntas residuais são a regra da classe quando juiz e cadastro divergem ou calam, o alcance por família de motor e a evidência exigida na competência contígua para o par de 21/11. O aval nomeia somente o DIFF da competência 11. `app/docs/BACKLOG.md:2265`, `app/docs/CORTES.md:637`, `aval.txt:1`, `recibo_c2v.md:22,25-28`.

## 27/11 17:08 · sessao 7ad913019bf2 · - · - · origem=Ronald (tmux)

**Cruzamento com RAMO2-B.** A herança está em `app/ponto/motor_calculo_v2.py:2489–2495,2513–2536`, seguida pela guarda da competência em `:2542–2545`. O intervalo aproximado do anexo, `:2610–2640`, cai no bloco de turno aberto nesta cópia. O anexo `recibo_c2v.md`, item 7, registra a medição e declara a herança fora das mudanças da c2v. O checkout não traz `.git`. O diff exato da outra raia e colisão de hunk são [nao verificado]. [app/ponto/motor_calculo_v2.py:2489](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/motor_calculo_v2.py:2489)

- A RAMO2-B passa pelo mesmo motor e pela chamada de cálculo do par. A decisão ali é recortar a saída com `_saida_efetiva` e `_janela_do_par`, que chamam `janela_he.saida_efetiva`. O ramo `marco_saida < virada <= saida` cobre turno que não cruza, com saída na madrugada, e deixa o turno que cruza no recorte seguinte. A herança opera antes, escolhendo o dono e passando ou barrando o período na janela. `app/docs/RELATO.md:188`, `app/docs/PROMPTS.md:3052`, `app/docs/BACKLOG.md:2159`, `app/ponto/janela_he.py:173–192`, `app/ponto/motor_calculo_v2.py:1827–1876,2513–2574`.
- O recibo c2v, itens 2–4 e Aberto, aponta ainda Comercial (`app/ponto/motor_calculo_v2.py:3425`), pontualidade (`:2125`), Intermitente (`:3686`) e Partido (`:3015`). O item 1 do recibo também toca `LINHAS_REGISTRADAS`. Esta cópia fixa a herança em `[2521]`, com 9 pareadores e 81 derivações no inventário local. `app/core/tests/test_medida_turno_do_par.py:108,250–259`, `app/core/medida_turno_do_par.py:261–333`. O contato conceitual da MOTOR-LE-O-TURNO-DIA-DO-PAR consta em `app/docs/BACKLOG.md:1998`.
- Controles já escritos para preservar a regra histórica estão em `app/ponto/tests/test_o93_dia_da_jornada.py:100–117` para período anterior aberto e `app/ponto/tests/test_o280_bateria_da_lei.py:1531–1575,1634–1650` para cauda legítima, feriado e prorrogação noturna.

**Universo do DIFF da 11.**

1. Apurar 11/2026 por empresa e pelo corte cadastrado, com 21/10–20/11 quando o corte é 21. Usar a mesma fotografia de insumos e o mesmo instante sob as duas versões. Medir todos os regimes do universo do escritor, mais as chaves que já têm `FechamentoMensal` ou `DiaPago` versão `motor`. O escritor inclui todos os estados que não sejam desligado e desligados com demissão a partir do início da janela da empresa, inclusive afastados/férias, e disputa não é filtro. A base traz também retidos no export e ausência de fechamento ou lavratura, com contagem explícita de não comparados. `app/ponto/janelas.py:10–21,36–57`, `app/ponto/services/fechamento.py:402–465,569–577,596–599,634–653,896–915`, `app/folha/porta_export.py:653–668`, `app/folha/management/commands/simular_folha.py:43–51`.
2. Ler as batidas apuráveis da abertura menos um dia ao fechamento mais um dia, em ordem de instantes, excluindo retratadas. Na fatia o filtro é por dia local, com margem de um dia e exclusão de batida de outro vínculo no interior da fatia. O caminho real chama o motor por vínculo e agrega a resposta. A segunda chamada de O85 para dias órfãos em vínculo parcial entra como controle, bem como quem não tem vínculo algum. `app/ponto/turnos.py:2941–2968`, `app/ponto/services/fechamento.py:259–311,314–336,639–678,744–776,801–863`.
3. Comparar união simétrica das chaves empresa, `colaborador_id`, competência, dia do dono e rubrica. Capturar para cada par o dono inicial e o dono após herança, a saída precedente, o intervalo e ambos os dias tocados. Manter dias da outra competência como testemunhas de borda. Contar chaves ausentes separadamente de zeros e exigir zero nas chaves de controle sem compensação entre dias e colabs. `app/ponto/motor_calculo_v2.py:2513–2545`, `app/ponto/services/dia_pago.py:109–132,285–292`, `app/ponto/models.py:317–323,382–390`, anexo `aval.txt:1`.
4. Usar todas as rubricas canônicas diárias de `DiaPago` versão `motor` e grade em minutos, mais a linha sem dia de DSR e banco e a testemunha mensal dos campos canônicos de `FechamentoMensal`. Períodos normais, folga certa, folga-feriado e folga sem escala podem guardar trabalho em campos distintos. Preservar os splits de HE como colunas sem somá-los novamente. `app/ponto/services/dia_pago.py:33–47,173–186,235–267,285–292,340–360,867–908`, `app/ponto/management/commands/aplicar_09_corte_b.py:67–74`.
5. Apurar o efeito `NOVO − HOJE` nos dois motores e declarar `HOJE − GRAVADO` e `NOVO − GRAVADO` no mensal. `diff_frota.medir` parte apenas de FM, `somar` omite chaves sem os dois lados e usa limiar de 0,01. Portanto sua saída mensal isolada não demonstra a condição diária. `recalcular_fechamento_mes(somente_leitura=True)` devolve o fechamento, e `lavrar_dias_pagos=True` entra em escrita. A lavra diária real recebe o mesmo `resultado` e `dia_da_jornada`. `app/ponto/services/diff_frota.py:7–22,45–89,101–135`, `app/ponto/services/fechamento.py:553–560,1152–1160,1215–1241`. Se a prova também contiver `DIFF_FOLHA`, validar TXT e retidos, observando que `simular_folha` escolhe a competência corrente pela data de referência da empresa. `app/folha/management/commands/simular_folha.py:43–51,113–131,202–225`. Frotas, deltas, status atual de exportação e resultado pela porta oficial são [nao verificado].

## 27/11 17:07 · sessao cf282caa47fc · - · - · origem=Ronald (tmux)

Na cópia, a herança está em `app/ponto/motor_calculo_v2.py:2489–2545`. A referência do aval a `~2610–2640` aponta, nesta cópia, para outra etapa, o crédito de pares fechados ao turno aberto. `app/ponto/motor_calculo_v2.py:2608–2640`

- O juiz produz `Turno.data_turno` pelo início e pelo envelope da véspera. O motor monta o mapa de entrada localizada para esse dia, entrega-o ao pareador e usa a data civil quando a chave falta. `app/ponto/turnos.py:248–324`, `app/ponto/motor_calculo_v2.py:894–922`, `app/ponto/motor_calculo_v2.py:2459–2469`, `app/ponto/motor_calculo_v2.py:2513–2517`
- Um vão de 0 a 6 h inclusive entre saída anterior e entrada atual sobrescreve esse dia com `_prev_dj`. O motor guarda o dia já herdado e a saída real, ou `None` se aberto, antes de excluir pares fora de `[data_ini, data_fim]`. A herança pode propagar em cadeia e atravessar a borda da janela pela ordem dessas operações. `app/ponto/motor_calculo_v2.py:2518–2545`
- No caso A, as 5 h entre 22:00 e 03:00 fazem o par de 480 min cair em D−1. Ambos os pares ficam fora da janela iniciada em D. No caso B, o par 20/11 22:00 → 21/11 06:00 vale 480 min no dia 20. As 5 h até 11:00 fazem o segundo par de 480 min cair também no dia 20. Out mantém ambos e nov descarta ambos. O recibo registra `{}`, `{20/11: 960}`, `{}` e o aval pede os 480 min no dono de cada par de 8 h. `app/ponto/motor_calculo_v2.py:2516–2545`, `recibo_c2v.md` item 7 fornecido, `aval.txt` fornecido.

| Estrutura | Escritores e leitores |
|---|---|
| Dia cru e turnos do juiz | `_turnos_do_juiz` e `_dia_do_turno` escrevem a resposta e a instância guarda o mapa e os turnos. Leitores incluem filtro da ata, janela L-097, bordas L-084, dobra por par e crédito BO-B. `app/ponto/motor_calculo_v2.py:711–714`, `:894–922`, `:2509–2510`, `:963–978`, `:1640–1654`, `:1696–1706`, `:2110`, `:2630–2637`, `:2911`, `:3380` |
| Herança e dia que leva o par | `_prev_fim`, `_prev_dj` são estado local. `data_jornada` filtra competência, julga folga, alimenta `_folga_do_par` e `_dia_jornada_por_periodo[id(periodo)]`. A janela e o bloqueio sem previsto leem `_folga_do_par`. O mapa local alimenta BO-B, presença para DSR/falta, O73 e teto de pontualidade. `app/ponto/motor_calculo_v2.py:2494–2545`, `:2558–2574`, `:1562`, `:1640`, `:2627–2637`, `:2689–2724`, `:2804–2831` |
| Dono para a soma | A chave é ID do objeto de período, com um dia para sua quantidade. O teto expõe o mapa no resultado, Partido também escreve diretamente e fechamento mescla as fatias. DiaPago, exportação, DIFF e calculador leem. Reflexo DSR no base ainda consulta data civil. `app/ponto/motor_calculo_v2.py:1117–1128`, `:3071–3075`, `:2749–2758`, `app/ponto/services/fechamento.py:314–336`, `app/ponto/services/dia_pago.py:109–132`, `:173–186`, `:234–267`, `app/folha/porta_export.py:106–117`, `app/ponto/calculador/alimentacao.py:73–92`, `:193–201`, `:239–260`, `app/ponto/calculador/periodo.py:596–615`, `app/ponto/management/commands/diff_calculador.py:214`, `:253`, `:268` |
| `minutos_trabalhados` | Inicializa em zero, três corpos calculam, bloqueio sem previsto move os minutos para fora e zera, BO-B pode creditar primeiro aberto, calculador V3 reescreve na releitura. Totais, teto, alerta FT, crédito, overrides, DiaPago, fechamento, espelho, exportação e DIFF leem. `app/ponto/motor_calculo_v2.py:213`, `:257`, `:296`, `:300`, `:1276`, `:1585–1590`, `:2602`, `:2666–2677`, `:2898`, `:3009`, `:3061`, `:3110`, `:3214–3218`, `:3372`, `:3447`, `:3472–3476`, `app/ponto/calculador/periodo.py:739–745`, `app/ponto/services/fechamento.py:922–923`, `:959–1009`, `app/ponto/services/espelho.py:136`, `app/ponto/services/dia_pago.py:178`, `:239`, `:249`, `:264`, `app/folha/porta_export.py:111`, `:550`, `app/ponto/management/commands/diff_calculador.py:253`, `:268` |

12x36, 12x36ComEscala, Plantão e Intermitente herdam o método mensal. Plantão reusa o cálculo por período do 12x36. Partido e Comercial chamam a base e reagrupam os pares sobreviventes pelo dia cru do juiz. PartidoNoturno herda Partido. `app/ponto/motor_calculo_v2.py:2843–2863`, `:2975`, `:3015–3061`, `:3315`, `:3425–3447`, `:3628–3652`, `:3657–3698`

Selos existentes cobrem jornada própria que excede o envelope da véspera, entrada atrasada depois da meia-noite, os dois lados da borda em segundos, volta real do intervalo e a guarda de saída anterior real. `app/ponto/tests/test_o93_dia_da_jornada.py:55–117`

## 27/11 17:01 · sessao 3c454d641058 · RAMO2-B-EM-TURNO-QUE-CRUZA · construtor · origem=Code

**O turno Partido está fora do aval.** A opção (a) limita a mudança ao 12x36 e ao Comercial. O recibo c2v também registra o selo do Partido vermelho. Na cópia, confirmei que o SHA de `MotorTurnoPartido` diverge do esperado (`1e159594…` contra `7970b507…`) e que o SHA de `MotorTurnoPartidoNoturno` confere. [docs/PROMPTS.md:3087](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/docs/PROMPTS.md:3087) · recibo c2v fornecido, seção «Aberto» · [ponto/tests/test_ramo2b_c2u_escopo.py:72](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2u_escopo.py:72)

**A restauração isolada quebra o cálculo de um par fechado.** O corpo do main desempacota quatro valores, e a janela atual devolve cinco. Reproduzi em memória `ValueError: too many values to unpack (expected 4)`. Restaurar o corpo também removeria a flag `False` do Partido, que hoje impede a inclusão da cauda e seria herdada como `True` de `MotorBase`. `MotorTurnoPartidoNoturno` herda o mesmo cálculo. `59600163c:app/ponto/motor_calculo_v2.py:3002` · [ponto/motor_calculo_v2.py:1939](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/motor_calculo_v2.py:1939) · [ponto/motor_calculo_v2.py:3100](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/motor_calculo_v2.py:3100) · [ponto/motor_calculo_v2.py:795](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/motor_calculo_v2.py:795) · [ponto/motor_calculo_v2.py:3820](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/motor_calculo_v2.py:3820)

**Para o Code levar ao Ronald:** restaurar o corpo do Partido e fazer `_janela_do_par` devolver quatro valores por padrão. Os dois chamadores de cinco valores, `Motor12x36` e `MotorComercial`, passam uma opção explícita. A saída só usa a cauda quando essa opção e a flag da classe estiverem ativas. Isso preserva as exclusões de Plantão e Intermitente, que reaproveitam o cálculo do 12x36. Proposta sem execução `[nao verificado]`. [ponto/motor_calculo_v2.py:1918](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/motor_calculo_v2.py:1918) · [ponto/motor_calculo_v2.py:2979](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/motor_calculo_v2.py:2979) · [ponto/motor_calculo_v2.py:3467](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/motor_calculo_v2.py:3467) · [ponto/motor_calculo_v2.py:3772](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/motor_calculo_v2.py:3772) · [ponto/motor_calculo_v2.py:3838](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/motor_calculo_v2.py:3838)

### Diff aplicado [nao verificado]

O B023 vem da captura de `previsto` pela lambda no laço. O argumento default fixa o valor de cada iteração. O diff é de uma linha, com as asserções e os SHAs preservados. `git diff --check` passou. A contagem AST de `ExceptHandler` ficou em `0 → 0` no teste e `18 → 18` no motor. [ponto/tests/test_ramo2b_c2u_escopo.py:56](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2u_escopo.py:56) · [ponto/tests/test_ramo2b_c2u_escopo.py:72](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2u_escopo.py:72) · [ponto/motor_calculo_v2.py:1](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/motor_calculo_v2.py:1) [nao verificado]

```diff
-                reler(resultado, lambda _dia: Regua(previsto_min=previsto), parciais_por_dia={})
+                reler(resultado, lambda _dia, previsto=previsto: Regua(previsto_min=previsto), parciais_por_dia={})
```

### RED e mutantes em memória

A sonda chama as duas lambdas depois do laço. Antes da correção, obteve `[720, 720]` e falhou. Depois, obteve `[300, 720]` e passou. O mutante `sem_default` repetiu a falha original. O mutante `default_zero` falhou com `[0, 0]`. [ponto/tests/test_ramo2b_c2u_escopo.py:41](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2u_escopo.py:41) · [ponto/tests/test_ramo2b_c2u_escopo.py:56](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2u_escopo.py:56) [nao verificado]

```bash
vinculo() {
    PYTHONDONTWRITEBYTECODE=1 python3 - "$1" <<'PY'
import ast
import sys
from pathlib import Path
from types import SimpleNamespace

path = Path("ponto/tests/test_ramo2b_c2u_escopo.py")
source = path.read_text()
variant = sys.argv[1]
if variant == "sem_default":
    source = source.replace("lambda _dia, previsto=previsto:", "lambda _dia:")
elif variant == "default_zero":
    source = source.replace("lambda _dia, previsto=previsto:", "lambda _dia, previsto=0:")

test = next(node for node in ast.walk(ast.parse(source))
            if isinstance(node, ast.FunctionDef)
            and node.name == "test_releitura_e_alimentacao_seguem_desconto_do_motor")
loop = next(node for node in ast.walk(test) if isinstance(node, ast.For))
callback = next(node for node in ast.walk(loop) if isinstance(node, ast.Lambda))
values = [ast.literal_eval(row.elts[2]) for row in loop.iter.elts]
make_rule = compile(ast.Expression(callback), str(path), "eval")
env = {"Regua": SimpleNamespace}
rules = []
for previsto in values:
    env["previsto"] = previsto
    rules.append(eval(make_rule, env))
actual = [rule(None).previsto_min for rule in rules]
assert actual == values, (variant, actual, values)
print(variant, actual)
PY
}

vinculo original
vinculo sem_default
vinculo default_zero
```

### Comandos para o Code

Suite Django e Ruff `[nao verificado]`. O selo do Partido continua divergente pela comparação de fonte descrita acima. [config/settings_test.py:1](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/config/settings_test.py:1) · [ruff.toml:4](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ruff.toml:4) · [ponto/tests/test_ramo2b_c2u_escopo.py:79](/home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946/app/ponto/tests/test_ramo2b_c2u_escopo.py:79) [nao verificado]

```bash
ruff check --no-cache ponto/tests/test_ramo2b_c2u_escopo.py

PYTHONDONTWRITEBYTECODE=1 python3 manage.py test --settings=config.settings_test \
  ponto.tests.test_ramo2b_c2u_escopo \
  ponto.tests.test_ramo2b_c2u_export \
  ponto.tests.test_ramo2b_c2v_borda \
  ponto.tests.test_ramo2b_motor_reds \
  ponto.tests.test_ramo2b_c2l_red_motor \
  ponto.tests.test_ramo2b_c2_base_da_he \
  ponto.tests.test_ramo2b_c2h_saida \
  ponto.tests.test_ramo2b_c2t_escopo \
  ponto.tests.test_ramo2b_c2t_motor \
  ponto.tests.test_ramo2b_turno_que_cruza \
  ponto.tests.test_ramo2b_porta_7a_testemunha \
  core.tests.test_medida_turno_do_par
```

## 27/11 17:00 · sessao b400b01c99fc · DESLIGADO-PORTA-DO-CHAMADO · censo · origem=Code

# Dossiê — DESLIGADO-PORTA-DO-CHAMADO

**Há uma terceira rota de reabertura de chamado.** Além de `abrir` e `reabrir`, a troca manual de status pode levar um chamado encerrado de volta a um estado vivo. O I5a, contador mais próximo do selo pedido, mede somente `COBRANCA` e deixa de fora o `registrado` do S89. A construção precisa cobrir a terceira rota e medir o estoque pelo universo `VIVOS`. `app/chamados/services/acoes_chamado.py:322`, `app/chamados/catalogo/motor.py:24`, `app/chamados/catalogo/motor.py:150`, `app/chamados/management/commands/auditar_invariantes_chamados.py:659`, `aval.txt:1`.

**Base e alcance.** A inspeção foi só de leitura. Identidade Git da cópia com `59600163ccc9cffb8f3ae148beede7cd7866ae86` [nao verificado]. O dado raspado de 27/11 informa 437 chamados abertos de 259 desligados, 301 nascidos depois da demissão, cerca de 50 por mês. Dos 301, são 230 S89, 49 `batida_ausente`, 10 `ancora_limbo`, 11 disputa e 1 `sem_previsao`. A distribuição por status canônico, o passivo de disputas e perguntas e a contagem após a cura ficam [nao verificado]. `aval.txt:1`.

## 1. Portas, chamadores e escritores

| Porta | Comportamento da cópia |
|---|---|
| `ChamadoColaborador.criar(cls, *, colaborador, status='aberto', **campos)` | Cria uma linha nova, sem dedup ou reabertura. Consulta `EMITIR` ou `REGISTRAR`, resolve a célula e consulta as guardas de absolvição e veto. As recusas existentes devolvem `None`. Na inserção grava a trilha e, quando aplicável, emite o evento da célula. `app/chamados/models.py:1113`, `app/chamados/models.py:1120`, `app/chamados/models.py:1137`, `app/chamados/models.py:1160`. |
| `ChamadoColaborador.abrir(cls, *, colaborador, defaults=None, **lookup)` | Trava o colaborador antes do dedup. Reusa o primeiro do lookup, tenta renascer se estiver em `ENCERRADOS` e devolve `(existente, False)` mesmo se a tentativa falhar. Também reutiliza filho sob pai vivo. Sem correspondência, consulta o motor e as guardas, então faz `get_or_create`. As recusas de nascimento existentes devolvem `(None, False)`. `app/chamados/models.py:899`, `app/chamados/models.py:968`, `app/chamados/models.py:987`, `app/chamados/models.py:993`, `app/chamados/models.py:1027`, `app/chamados/models.py:1037`. |

**Chamadores de `criar`.** A coluna de origem distingue pedidos humanos de efeitos automáticos disparados durante uma requisição. `app/chamados/services/abertura.py:19`, `app/ponto/services/triagem_batida.py:336`, `app/api/views_core.py:714`.

| Sítio | `modulo_origem` | Origem |
|---|---|---|
| `app/api/views.py:2189` | `app_colaborador` | Humana, solicitação no app |
| `app/chamados/services/abertura.py:47` | `modulo`, da categoria do catálogo quando mapeada, com `chamados` como padrão da view | Humana, abertura própria. `app/chamados/services/abertura.py:34`, `app/chamados/views.py:150`, `app/chamados/views.py:197` |
| `app/chamados/services/abertura.py:172` | `modulo=tipo`, padrão `mensagem_dp`, limitado pelo catálogo da porta | Humana, abertura para o colaborador. `app/chamados/views.py:101`, `app/chamados/views.py:136`, `app/chamados/services/abertura.py:149`, `app/chamados/catalogo/modulos.py:306` |
| `app/colaboradores/views.py:2026`, `app/colaboradores/views.py:2412` | `beneficio`, `agenda_dia` | Humana |
| `app/chamados/services/regularizacao_ext.py:247` | `regularizacao_externa` | Humana, regularização. `app/colaboradores/views.py:3032` |
| `app/colaboradores/services/vinculo.py:412` | `escala_colaborador` | Efeito do ato de vínculo, com entrada web e comando avulso. `app/colaboradores/views.py:442`, `app/ponto/management/commands/censo_fase_12x36.py:427` |
| `app/ponto/services/justificativa.py:411` | `justificativa` | Efeito da recusa humana pelo DP. `app/ponto/views.py:1956` |
| `app/ponto/services/triagem_batida.py:336` | `batida_fora_horario` | Humana, pedido de anuência. `app/ponto/services/triagem_batida.py:349` |
| `app/ponto/services/triagem_batida.py:107`, `app/ponto/services/triagem_batida.py:459` | `acesso_bloqueado`, `batida_fora_horario` | Sistema, triagem |
| `app/ponto/utils.py:40`, `app/ponto/utils.py:103`, `app/ponto/views.py:310` | `colaborador_sem_posto`, `colaborador_em_ferias`, `entrada_bloqueada_turno_aberto` | Sistema, tentativa de batida |
| `app/api/views.py:720`, `app/api/views_core.py:714` | `batida_fora_escala_leve` | Sistema, nas duas cascas da batida |
| `app/ponto/services/geofence.py:361` | `gps_ausente` ou `gps_fora_geofence` | Sistema, conforme o alerta. `app/ponto/services/geofence.py:344` |
| `app/ponto/management/commands/processar_alertas_avancados.py:121` | `turno_aberto_24h` | Cron. `app/config/crons.py:363` |
| `app/colaboradores/services/desligamento.py:195` | `desligamento_revisao` | Efeito do desligamento manual ou agendado. `app/colaboradores/views.py:1475`, `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:57` |
| `app/chamados/management/commands/smoke_chamados.py:124` | `smoke` | Comando diagnóstico na árvore de produção |

**Chamadores de `abrir`.** `app/chamados/models.py:899`.

| Sítio | `modulo_origem` | Origem |
|---|---|---|
| `app/chamados/views.py:974` | `mensagem_dp` | Humana, abertura rápida |
| `app/api/views.py:1985`, `app/ponto/services/ausencia.py:805` | `justificativa_colab`, `ausencia_colab` | Efeito de pedido humano. `app/api/views.py:1861`, `app/ponto/views.py:2718` |
| `app/chamados/services/disputa_emissao.py:582` | `disputa_supervisao` | Serviço compartilhado por declaração e emissão automática. `app/chamados/services/disputa_emissao.py:641`, `app/chamados/services/disputa_emissao.py:676`, `app/ponto/management/commands/reconciliar_grade.py:424` |
| `app/chamados/services/disputa_emissao.py:1118` | `disputa_supervisao_manual` | Humana ou cron, apesar do rótulo. `app/chamados/services/acoes_disputa.py:261`, `app/chamados/services/cobrar_dia.py:49`, `app/chamados/services/pergunta_no_app.py:263`, `app/ponto/management/commands/processar_alertas_turno.py:385`, `app/ponto/management/commands/processar_alertas_avancados.py:159`, `app/ponto/management/commands/detectar_ausencias.py:803` |
| `app/chamados/services/disputa_emissao.py:1353` | `disputa_supervisao_manual` | Humana, âncora. `app/chamados/services/acoes_disputa.py:228` |
| `app/ponto/views.py:3191`, `app/escala/regua_defesa.py:51` | `ping_geo_fora`, `regua_degenerada` | Sistema. Para a régua, `app/ponto/management/commands/detectar_ausencias.py:601`, `app/ponto/management/commands/reconciliar_grade.py:495` |
| `app/chamados/services/adesao.py:75` | `sem_previsao_escala` | Cron de apuração. `app/escala/management/commands/apurar_furos_diarios.py:17`, `app/config/crons.py:1536` |
| `app/ponto/management/commands/detectar_ausencias.py:191`, `app/ponto/management/commands/detectar_ausencias.py:272`, `app/ponto/management/commands/detectar_ausencias.py:511`, `app/ponto/management/commands/detectar_ausencias.py:694` | Respectivamente `sem_previsao_escala`, `ancora_limbo`, `sem_previsao_escala`, `batida_ausente` | Cron. `app/ponto/management/commands/detectar_ausencias.py:29`, `app/config/crons.py:364` |
| `app/ponto/management/commands/reconciliar_grade.py:507`, `app/ponto/management/commands/processar_alertas_turno.py:335` | `batida_ausente`, `turno_aberto` | Cron. `app/ponto/management/commands/reconciliar_grade.py:54`, `app/config/crons.py:362`, `app/config/crons.py:462` |
| `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:151` | `turno_aberto_24h` | Sistema, comando retroativo |
| `app/ponto/services/cluster_espurio.py:196` | `cluster_batida_espuria` | Sistema, detecção. `app/ponto/services/cluster_espurio.py:11`, `app/ponto/management/commands/detectar_cluster_espurio.py:81`, `app/config/crons.py:598` |
| `app/chamados/reconciliador.py:1032` | `batida_ausente`, na chave C2 | Sistema, reemissão. `app/chamados/reconciliador.py:925`, `app/chamados/reconciliador.py:957`, `app/chamados/reconciliador.py:1776` |

**Escrita de `status_local`.** O censo estático encontrou os dois `INSERT`s acima e um escritor de transições, `_gravar_estado`. Chegam a ele `_aplicar`/`transicionar`, `reconciliar`, `cancelar`, `analisar`, `reabrir`, `encerrar` e `encerrar_por_competencia_trancada`. As atribuições do ensaio pós-estado em `_vivas_entre` são em memória, restauradas no `finally`. O selo estático cobre atribuição, `save(update_fields=...)`, ORM de escrita e SQL literal. As migrações históricas alteram `aberto`/`registrado` e `superado`/`cancelado`; SQL externo e chamadas dinâmicas fora da árvore [nao verificado]. `app/chamados/models.py:213`, `app/chamados/models.py:240`, `app/chamados/models.py:308`, `app/chamados/models.py:375`, `app/chamados/models.py:390`, `app/chamados/models.py:469`, `app/chamados/models.py:674`, `app/chamados/models.py:854`, `app/chamados/models.py:872`, `app/chamados/models.py:1508`, `app/chamados/models.py:1793`, `app/chamados/models.py:1813`, `app/chamados/tests/test_selo_status_so_pelas_portas.py:227`, `app/chamados/tests/test_selo_status_so_pelas_portas.py:363`, `app/chamados/migrations/0038_a3_registrado_backfill.py:10`, `app/chamados/migrations/0039_a3_via_backfill.py:24`.

**Reaberturas fora das duas nascentes.** `reabrir(*, marcos=None, humano=False)` é chamado por adesão, repasse/S128, reabertura de ausência, reconciliação da grade e detector de ausências. O renascimento também é chamado por resposta tardia e cluster. As três entradas com `humano=True` e a rota paralela `transicionar` estão no item 5. `app/chamados/models.py:1085`, `app/chamados/models.py:1428`, `app/chamados/services/adesao.py:93`, `app/chamados/services/disputa_emissao.py:1971`, `app/chamados/services/disputa_emissao.py:2143`, `app/chamados/services/resposta_tardia.py:46`, `app/ponto/services/cluster_espurio.py:287`, `app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:49`, `app/ponto/management/commands/reconciliar_grade.py:530`, `app/ponto/management/commands/detectar_ausencias.py:302`.

## 2. Juiz e universos

O juiz registrado é `colaboradores.services.desligamento.esta_desligado(colaborador)`. Responde `getattr(colaborador, 'situacao', None) == 'desligado'`. Lê o estado que recebeu, sem argumento de data, sem consultar o relógio, `data_demissao` ou `vinculo_do_dia`. No aviso prévio o executor grava data e motivo e sai antes de mudar situação. A efetivação grava `'desligado'`. A reativação grava `'ativo'` e limpa data e motivo. `app/colaboradores/services/desligamento.py:23`, `app/colaboradores/services/desligamento.py:107`, `app/colaboradores/services/desligamento.py:129`, `app/colaboradores/services/desligamento.py:241`, `app/core/juizes.py:283`, `app/docs/CORTES.md:517`.

| Sítio | Predicado e relação com o juiz |
|---|---|
| `app/chamados/models.py:1384` | `recusa_da_reabertura` hoje consulta veto do dia para automático, fato novo depois de resolução administrativa, premissa regularizada, agrupamento, irmão vivo e motor. A situação do colaborador está ausente dessa lista. `app/chamados/models.py:1403`, `app/chamados/models.py:1423`. |
| `app/chamados/models.py:1070`, `app/chamados/reconciliador.py:942` | `situacao != 'ativo'` exclui mais estados que o juiz. No renascimento também há `via_encerramento == VIA_CASCADE`, tranca e veto. A via é propriedade do chamado e `encerrar` usa `cascade` genericamente. `app/chamados/models.py:1071`, `app/chamados/models.py:1793`, `app/chamados/catalogo/motor.py:95`. |
| `app/chamados/services/adesao.py:67`, `app/chamados/services/adesao.py:91` | Em `:67`, mesma igualdade crua do juiz dentro do universo de adesão. Em `:91`, o que barra é `VIA_CASCADE`, que pode continuar no chamado após reativação. `app/colaboradores/services/desligamento.py:241`. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:659`, `app/ponto/services/cartorio.py:1803`, `app/ponto/services/cartorio.py:1868`, `app/ponto/services/furos_vetados.py:62`, `app/ponto/services/vigia_de_hora.py:166` | Mesma igualdade ou complemento do juiz, com recortes próprios de cobrança, emissão, veto e isenção. |
| `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:38` | `data_demissao` preenchida, `<= hoje`, situação diferente de desligado. Identifica agendamento vencido. O juiz do cadastro é perguntado na revalidação. `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:51`, `app/colaboradores/services/desligamento.py:38`. |
| `app/api/views.py:403`, `app/api/views_core.py:389`, `app/api/views_core.py:172` | A batida aceita `ativo`/`afastado` na guarda de situação. A renovação de token exige `ativo`. São universos mais amplos de recusa que `esta_desligado`. |
| `app/escala/alimentacao.py:205`, `app/escala/alimentacao.py:228`, `app/ponto/supra_juiz.py:194`, `app/ponto/utils.py:177` | Vigência e vínculo perguntam por uma data. Sem vínculo no ramo do supra-juiz, `dia > data_demissao` corta a cobertura. A elegibilidade de `ponto/utils.py` compara com o fim da janela. |
| `app/folha/export.py:54`, `app/folha/export.py:1216`, `app/folha/export.py:1235`, `app/ponto/services/fechamento.py:452` | Rescisão e universo de competência têm recorte por data. Retenção e informativo da rescisão já chamam `esta_desligado`. |

Há outras leituras cruas da mesma situação em cadastro, recortes de listas, vínculo e relatórios. Elas têm universos próprios e não são novas portas de chamado. A situação exibida dá precedência ao desligado sobre as demais etiquetas. `app/colaboradores/views.py:1065`, `app/colaboradores/views.py:1449`, `app/colaboradores/views.py:2895`, `app/colaboradores/views.py:2945`, `app/colaboradores/views.py:2961`, `app/colaboradores/services/situacao_exibida.py:56`, `app/colaboradores/services/vinculo.py:91`, `app/colaboradores/management/commands/fechar_vinculo_de_desligado.py:35`, `app/colaboradores/management/commands/tripwire_vinculo_orfao.py:47`, `app/ponto/views.py:1148`, `app/ponto/services/vinculo_na_lista.py:71`, `app/ponto/services/vinculo_na_lista.py:152`, `app/escala/services/cadastro_realidade.py:670`, `app/ferias/views.py:655`, `app/relatorios/views.py:899`, `app/relatorios/views.py:914`, `app/relatorios/views.py:923`, `app/inteligencia/views.py:83`, `app/core/busca.py:104`.

## 3. S88/S89 e histórico

- S89 nasce **depois** da cascata, em `ChamadoColaborador.criar`, com `area='dp_rh'`, categoria `auditoria_clt`, subcategoria `registrar_resposta_oficial`, módulo `desligamento_revisao` e urgência `normal`. O JSON leva data, motivo, autor e instante. O catálogo o leva a `REGISTRAR`, que nasce `registrado`, dentro de `VIVOS` e `NAO_FECHADOS`. O cron de 06:06 usa `--apply`, revalida e chama o mesmo executor com `request.user=None`. A revisão S88 citada no emissor é a decisão substituída pelo aval. O signal também chamado S88, Defesa 1, é o que tenta reconciliar o pai após fechar a disputa. `app/colaboradores/services/desligamento.py:182`, `app/colaboradores/services/desligamento.py:191`, `app/colaboradores/services/desligamento.py:195`, `app/colaboradores/services/desligamento.py:205`, `app/chamados/catalogo/natureza.py:18`, `app/chamados/catalogo/motor.py:24`, `app/chamados/catalogo/motor.py:134`, `app/config/crons.py:440`, `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:46`, `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:57`, `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:66`, `app/chamados/signals.py:19`, `aval.txt:1`.
- **Leitura humana.** O push aponta ao detalhe local, que mostra descrição inteira, timeline e resposta livre com opção de status. Fio e modal gerenciais mostram descrição truncada e ocorrências. O painel lateral lê JSON e followups. A lista do colaborador exclui o módulo interno. O catálogo usa `data_demissao` como dia, dono DP, e o exclui da tranca. O desenlace o apresenta como registro, com o texto de conferência de RH. Entrega efetiva de push [nao verificado]. `app/colaboradores/services/desligamento.py:219`, `app/chamados/urls.py:14`, `app/templates/chamados/detalhe_local.html:97`, `app/templates/chamados/detalhe_local.html:112`, `app/chamados/services/acoes_chamado.py:292`, `app/templates/chamados/fio_colaborador.html:231`, `app/templates/chamados/partials/modal_fio.html:487`, `app/templates/chamados/partials/painel_chamado_turno.html:35`, `app/chamados/services/janela_colab.py:36`, `app/chamados/services/janela_colab.py:74`, `app/chamados/catalogo/modulos.py:141`, `app/chamados/catalogo/modulos.py:265`, `app/chamados/services/desenlace.py:85`, `app/chamados/services/verbos.py:176`.
- **Contadores e placares.** A lente Registros agrupa vivos por módulo e o painel aceita `status=registrado`. A fila padrão, os contadores da Inteligência e o I5a operam com cobrança. A API de mensageria expõe a pilha `registro_auditoria` e a visão individual lê vivos. O vigia do fio inclui vivos com célula no agregado e separa módulos fora do vocabulário de furo. O placar Equipe mede resoluções assinadas e validações. `app/chamados/views.py:1476`, `app/chamados/services/verbos.py:199`, `app/chamados/services/verbos.py:242`, `app/chamados/services/painel.py:212`, `app/chamados/services/painel.py:306`, `app/inteligencia/views.py:101`, `app/api/views_mensageria.py:452`, `app/api/views_mensageria.py:1043`, `app/chamados/services/vigia_fio.py:337`, `app/chamados/services/vigia_fio.py:401`, `app/chamados/management/commands/auditar_invariantes_chamados.py:659`.
- **Os dois históricos já recebem o ato.** `HistoricoVinculo` recebe agendamento e efetivação, com data, motivo, observação e autor. A observação tem limite de 400 caracteres e aparece no cadastro e no fio. `LogAuditoria` recebe o agendamento dentro do `atomic` e a efetivação via `registrar_log`, depois do bloco. Seu JSON comporta o informativo estruturado. A tela genérica exige `ver_saude_sistema`, semeada para TI. O cadastro com histórico tem `editar_cadastro`, semeada para DP. Portanto, gravar o JSON já é possível, e a entrega legível de dia, pergunta e resposta ao DP precisa de uma superfície com a permissão correspondente. `app/colaboradores/models.py:548`, `app/colaboradores/models.py:577`, `app/colaboradores/services/historico_vinculo.py:20`, `app/colaboradores/services/desligamento.py:112`, `app/colaboradores/services/desligamento.py:118`, `app/colaboradores/services/desligamento.py:138`, `app/colaboradores/services/desligamento.py:227`, `app/templates/colaboradores/editar.html:179`, `app/templates/chamados/fio_colaborador.html:88`, `app/core/models.py:285`, `app/core/views.py:686`, `app/colaboradores/mixins.py:363`, `app/colaboradores/mixins.py:375`, `aval.txt:1`.

## 4. Pergunta respondida sem veredito

**Hoje.** `respostas_sem_veredito` lê `DecisaoPergunta.composta`, exige instante **e** texto pelo juiz L-130, sem validação e sem materialização. Exclui dia trancado salvo `ignorar_tranca=True`. O executor pula a disputa se essa lista vier preenchida. `DisputaSupervisao.fechar` refaz a guarda sob trava e, no desligamento ou `admin_encerra`, carimba as vivas que passaram como `admin_decidiu`, registra `SILENCIO` e fecha a disputa. O signal tenta reconciliar o pai, depois o executor tenta `encerrar` nos `NAO_FECHADOS`. Uma pergunta ainda viva pode barrar o chamado, inclusive se estiver ancorada em disputa já fechada. `app/chamados/models.py:2000`, `app/chamados/models.py:2041`, `app/colaboradores/services/desligamento.py:164`, `app/chamados/models.py:2123`, `app/chamados/models.py:2175`, `app/chamados/models.py:2184`, `app/chamados/signals.py:43`, `app/colaboradores/services/desligamento.py:182`, `app/chamados/models.py:361`, `app/chamados/models.py:429`.

**Via que falta.** Via própria de desligamento para a respondida e, por LU-11, também para a muda. O molde é o **ramo legado** de `VIA_COMPETENCIA_TRANCADA`: via com carimbo vence o degrau da resposta no C6 e sai no espelho `q_vivas`. A tranca atual opera por leitura e gera Pauta DP. O valor novo precisa encerrar a pergunta, preservar a resposta como informação e continuar distinto 

[truncado em 20000 de 46253 caracteres]

## 27/11 16:58 · sessao e6c6f651b62f · PAR-PELO-TURNO · construtor · origem=Code

Apliquei a cura da c1v15 na cópia. As asserções e fixtures existentes foram preservadas. A contagem AST de `ExceptHandler` ficou em **1.394 → 1.394** no app e **2 → 2** nos arquivos do diff. `ponto/turnos.py:1930`, `relatorios/cartao_pela_celula.py:395`, `ponto/tests/test_par_pelo_turno_c1v15.py:13`.

### Causas e correções

- **C9.** A c1v14 passou a chamar `marcos_dna_periodo(..., celulas=...)`, mas `_mpd` da fixture aceita três argumentos. O `TypeError` acontece antes do juiz do dono. `medir` contabiliza o erro e descarta o colaborador inteiro. Isso explica o `SystemExit(2)`, a ausência da linha que gera o `KeyError` e as zero chamadas ao cadastro no teste que esperava falhar no segundo dia. O carregador voltou à chamada de três argumentos quando a carga implícita vem vazia e reaproveita o DNA quando a carga traz células. Na janela vazia, o provedor pode fazer uma consulta sob demanda. `a7174ec14:ponto/turnos.py:1958`, `ponto/tests/test_col129_teto_vira_lista.py:33`, `ponto/tests/test_col129_teto_vira_lista.py:135`, `ponto/tests/test_col129_teto_vira_lista.py:147`, `ponto/services/teto_na_lista.py:51`, `ponto/services/teto_na_lista.py:71`, `ponto/management/commands/lavrar_teto_do_dia.py:32`, `ponto/turnos.py:1934`, `ponto/turnos.py:1956`, `escala/utils.py:100`.
- **M8.** A c1v13 passou `ate=None` ao consultar turno vivo, embora o calendário só forneça `agora`. O dublê rejeita `ate`, o calendário publica oferta `None` e a tarefa devolve `nao_sei`. `folha_manda` agora encaminha `ate` apenas quando recebe um corte explícito. O PDF e o espelho continuam passando `ate=agora`. `a7174ec14:relatorios/cartao_pela_celula.py:398`, `colaboradores/services/calendario.py:615`, `colaboradores/services/calendario.py:812`, `chamados/tests/e3_lu3_apoio.py:72`, `chamados/services/tarefa_do_dia.py:732`, `relatorios/cartao_pela_celula.py:398`, `relatorios/pdf_espelho.py:255`, `ponto/services/espelho.py:1315`.
- **F401.** Removi o import sem uso de `MagicMock`. `ponto/tests/par_pelo_turno_c1v10_verificacao.py:14`.

### RED e mutantes

O RED novo reproduziu `erros=1` por `celulas=` na base. Com a cura, obteve `erros=0` e a linha C9 de 780 minutos com teto de 732. Em memória, os controles `dna`, `presenca` e `corte` deram `rc=0`. Cada mutante correspondente deu `rc=1` por asserção. `ponto/tests/test_par_pelo_turno_c1v15.py:50`, `ponto/tests/par_pelo_turno_c1v15_verificacao.py:40`, `ponto/tests/par_pelo_turno_c1v15_verificacao.py:73`.

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.par_pelo_turno_c1v15_verificacao dna --mutante
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.par_pelo_turno_c1v15_verificacao presenca --mutante
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.par_pelo_turno_c1v15_verificacao corte --mutante
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.par_pelo_turno_c1v15_verificacao import --mutante
```

O mutante do import ficou **[não verificado]**. O executável Ruff estava ausente no `PATH`. A certificação da suíte na pista fica com o Code e o Opus final. `ponto/tests/par_pelo_turno_c1v15_verificacao.py:65`.

### Comando para o Code

```bash
PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings.ci \
  python3 manage.py test --noinput \
  ponto.tests.test_col129_teto_vira_lista \
  chamados.tests.test_e3_degrau_dono_e_porta \
  core.tests.test_contract_esmeril.ContratoEsmerilTest.test_ruff_zero \
  ponto.tests.test_par_c1v8_reds \
  ponto.tests.test_par_pelo_turno_c1v10 \
  ponto.tests.test_par_pelo_turno_c1v11 \
  ponto.tests.test_par_pelo_turno_c1v12 \
  ponto.tests.test_par_pelo_turno_c1v13 \
  ponto.tests.test_par_pelo_turno_c1v14 \
  ponto.tests.test_par_pelo_turno_c1v15
```

### Diff na cópia

```bash
git diff -- ponto/turnos.py relatorios/cartao_pela_celula.py \
  ponto/tests/par_pelo_turno_c1v10_verificacao.py
git diff --no-index /dev/null ponto/tests/test_par_pelo_turno_c1v15.py
git diff --no-index /dev/null ponto/tests/par_pelo_turno_c1v15_verificacao.py
```

## 27/11 16:57 · sessao f39976049fa4 · FERIADO-UM-JUIZ · construtor · origem=Code

Implementei a c5 na cópia. O controle passou com **40 testes sem banco e 41 estruturais**. Os 10 mutantes da c5 passaram no controle e falharam como previsto quando aplicada a mutação. `ponto/tests/feriado_um_juiz_mutantes.py:47`, `:283`, `:337`

- **Achado 6 — selos.** Atualizei os cinco endereços da catraca e comparei as expressões B1 com as registradas na c3. A partição agora grava o dia corrigido no mapa e o relê com `_dia_de` antes de consultar a certeza da escala e a cláusula da rubrica. Carreguei as fontes da c4 em memória e reproduzi as três falhas por asserção. Os três selos passaram na c5. `core/tests/test_medida_turno_do_par.py:289`, `:329`, `:565`, `ponto/calculador/regras.py:425`, `ponto/tests/test_noturno_v8_ft_no_dia_da_lavra.py:58`
- **Achado 4 — competência e ciclo.** A borda consulta o juiz do feriado nos dois dias do corte. O RED mostrou que a c4 pôs as 4 h comuns do dia 21 na competência 11. Na c5, a 11 recebe as 4 h de feriado do dia 20 e a 12 recebe as 4 h comuns do dia 21. Na troca 6x1 → 12x36, o RED mostrou 4 h indevidas de HE100 feriado na véspera. Na c5, cada dia recebe 4 h comuns. Os dois casos rodaram com as mesmas batidas na c4 e na c5. `ponto/services/fechamento.py:155`, `ponto/calculador/regras.py:410`, `ponto/tests/test_feriado_um_juiz.py:607`, `:618`
- **Achado 5 — três gravações.** Um `atomic` por colaborador cobre a criação ou atualização do mensal e as duas lavras. As capturas existentes marcam rollback. Em memória, a falha injetada na segunda lavra deixou mensal e primeira lavra atualizados na c4. Na c5, os três voltaram aos valores anteriores. A falha na primeira lavra também foi testada. A competência sem pares consegue gravar o ajuste do oráculo com rubricas vazias. **`except` no diff lógico: 0 adicionados e 0 removidos**, conferidos por AST e `git diff -w`. `ponto/services/fechamento.py:928`, `:1276`, `:1340`, `ponto/calculador/alimentacao.py:360`, `ponto/services/dia_pago.py:311`, `ponto/tests/test_feriado_um_juiz.py:43`, `:147`

**Comandos em `app/`** — `base_c4` reproduz quatro falhas por asserção. O executor retorna `0` quando encontra o resultado esperado. `ponto/tests/feriado_um_juiz_mutantes.py:337`

```bash
prova() {
  PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.feriado_um_juiz_mutantes "$1"
}

prova base_c4
prova controle
prova estruturais

prova c5_endereco_folha_antigo
prova c5_dia_sem_leitura_da_lavra

prova c5_so_feriado_depois_do_corte
prova c5_dobra_pela_fatia_de_origem
prova c5_12x36_lavrado_na_vespera

prova c5_gravacoes_sem_atomic_comum
prova c5_motor_sem_sinal_de_rollback
prova c5_oraculo_sem_sinal_de_rollback
prova c5_exige_mapa_sem_pares
prova c5_recusa_lavra_sem_rubricas
```

**Recorte para a sessão na VM.** O teste Django cobre mensal aberto e aprovado, com falha no `bulk_create` do oráculo. Execução com banco, recorte na VM, suíte inteira e parecer do Opus [nao verificado]. `ponto/tests/test_feriado_um_juiz_porta.py:462`, `docs/RELATO.md:1841`, `docs/LEIS.md:147`

```bash
bin/suite.sh --dir /home/ronald/wt-codex-feriado-um-juiz-152427 \
  --only "ponto core folha relatorios"
```

## 27/11 16:57 · sessao f92e1d4a9b06 · - · - · origem=Ronald (tmux)

**Mapa.** Censo estático confirmou 20 chamadas a `criar`, incluindo um smoke manual, e 18 a `abrir`. O selo de escritores percorreu 774 arquivos em memória, com quatro verificações aprovadas e nenhum escritor de `status_local` em runtime fora das portas declaradas. O selo exclui testes e migrações. [test_selo_status_so_pelas_portas.py](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/tests/test_selo_status_so_pelas_portas.py:340)

- `criar(cls, *, colaborador, status='aberto', **campos)` insere sempre que passa motor, absolvição pela célula, veto e urgência. Quando `status='aberto'`, o motor pode escolher `registrado`. Sem dedup nem reabertura. Recusas preexistentes devolvem `None`. `app/chamados/models.py:1120-1166`.
- `abrir(cls, *, colaborador, defaults=None, **lookup)` serializa pelo colaborador, procura correspondente, tenta renascer se encerrado e devolve `(existente, False)` mesmo quando recusou renascer. Procura filho agrupado sob pai vivo. Se chegar ao nascimento passa motor, célula e veto, grava via `get_or_create` e devolve `(ch, criado)`. Recusas preexistentes de nascimento devolvem `(None, False)`. `app/chamados/models.py:899,968-1047`.
- Fora dos dois `INSERT`s (`app/chamados/models.py:1037,1161`), `_gravar_estado` é o escritor runtime da coluna (`:213,240,304-308`), chamado por `_aplicar` (`:469`), `reconciliar` (`:674`), `cancelar` (`:854`), `analisar` (`:872`), `reabrir` (`:1508`), `encerrar` (`:1793`) e `encerrar_por_competencia_trancada` (`:1813`). `:375,390` são simulação em memória. Migrações históricas alteram estado em `app/chamados/migrations/0038_a3_registrado_backfill.py:10,15` e `0039_a3_via_backfill.py:24,33`.
- Reabertura fora de nascer/renascer existe por `reabrir`, que leva `fechado`/`resolvido` a `em_analise`, e pelo POST humano → `transicionar_com_pauta` → `transicionar` → `_aplicar`. No segundo, o motor permite ao `admin_ti` sair de `fechado` inclusive para `aberto`, `em_analise` ou `registrado`, todos `VIVOS`. As telas do detalhe, fio e modal ocultam a troca usual de status em `fechado`, mas a aceitação no backend está no motor. `app/chamados/models.py:140,211,466-469,1428,1505-1518`; `app/chamados/services/fecho_runtime.py:175,181`; `app/chamados/catalogo/motor.py:24,98-106,113,139-156`; `app/templates/chamados/detalhe_local.html:375`, `fio_colaborador.html:254`, `partials/modal_fio.html:682`.

**Chamadores de `criar`.**

| Sítio | Módulo e origem |
|---|---|
| `app/api/views.py:2189` | `app_colaborador`, solicitação humana |
| `app/chamados/services/abertura.py:47` | `modulo`, humano, GET com default `chamados` e catálogo pode sobrescrever (`app/chamados/views.py:150,197`, `abertura.py:34`) |
| `app/chamados/services/abertura.py:172` | `modulo`, humano DP, `tipo` do formulário ou `mensagem_dp` default do helper (`app/chamados/views.py:136-140`, `abertura.py:124`) |
| `app/colaboradores/views.py:2026,2412` | `beneficio`, `agenda_dia`, ações humanas |
| `app/chamados/services/regularizacao_ext.py:247` | `regularizacao_externa`, ação humana (`app/colaboradores/views.py:3032`, `app/chamados/views_regularizacao.py:54` via `regularizacao_ext.py:319`) |
| `app/colaboradores/services/vinculo.py:412` | `escala_colaborador`, oferta de aceite em ação humana (`:388`) |
| `app/ponto/utils.py:40,103` | `colaborador_sem_posto`, `colaborador_em_ferias`, automatismos de ponto |
| `app/ponto/views.py:310` | `entrada_bloqueada_turno_aberto`, automatismo da batida |
| `app/api/views.py:720`, `app/api/views_core.py:714` | `batida_fora_escala_leve`, automatismo da batida |
| `app/ponto/services/triagem_batida.py:107,336,459` | `acesso_bloqueado`, `batida_fora_horario` nos dois últimos, automatismos da batida |
| `app/ponto/services/geofence.py:361` | `gps_ausente` ou `gps_fora_geofence` (`:344`), automatismo da batida |
| `app/ponto/services/justificativa.py:411` | `justificativa`, efeito de rejeição humana DP (`app/ponto/views.py:1956`) |
| `app/ponto/management/commands/processar_alertas_avancados.py:121` | `turno_aberto_24h`, cron (`app/config/crons.py:363`) |
| `app/colaboradores/services/desligamento.py:195` | `desligamento_revisao`, efeito de UI ou cron |
| `app/chamados/management/commands/smoke_chamados.py:124` | `smoke`, diagnóstico manual (`app/config/crons.py:1460`) |

**Chamadores de `abrir`.**

| Sítio | Módulo e origem |
|---|---|
| `app/chamados/views.py:974` | `mensagem_dp`, humano |
| `app/api/views.py:1985` | `justificativa_colab`, wrapper da solicitação humana |
| `app/ponto/services/ausencia.py:805` | `ausencia_colab`, efeito da solicitação humana (`app/api/views.py:1861`, `app/ponto/views.py:2718`) |
| `app/chamados/services/disputa_emissao.py:582` | `disputa_supervisao`, misto. Declaração humana `:620,641` e sistema `:648,676`, chamado por APIs de batida, fábrica e crons (`app/api/views.py:768`, `app/api/views_core.py:762`, `app/chamados/services/fabrica_da_celula.py:172`, `app/ponto/management/commands/reconciliar_grade.py:424`) |
| `app/chamados/services/disputa_emissao.py:1118,1353` | `disputa_supervisao_manual`. Órfãos misto (`app/chamados/services/acoes_disputa.py:261`, `app/chamados/services/cobrar_dia.py:49`, `app/chamados/services/pergunta_no_app.py:263`, crons `processar_alertas_turno.py:385`, `processar_alertas_avancados.py:159`, `detectar_ausencias.py:803`); âncora humana (`acoes_disputa.py:228`) |
| `app/chamados/services/adesao.py:75` | `sem_previsao_escala`, sistema (`app/escala/management/commands/apurar_furos_diarios.py:17`, `app/config/crons.py:1536`) |
| `app/ponto/views.py:3191` | `ping_geo_fora`, automatismo do ping |
| `app/escala/regua_defesa.py:51` | `regua_degenerada`, crons `app/ponto/management/commands/detectar_ausencias.py:601`, `reconciliar_grade.py:495` |
| `app/ponto/services/cluster_espurio.py:196` | `cluster_batida_espuria` (`:11`), cron `app/config/crons.py:598` |
| `app/ponto/management/commands/detectar_ausencias.py:191,511,272,694` | `sem_previsao_escala` nos dois primeiros, `ancora_limbo`, `batida_ausente` (`:29`), cron `app/config/crons.py:364` |
| `app/ponto/management/commands/reconciliar_grade.py:507` | `batida_ausente` (`:54`), cron `app/config/crons.py:462` |
| `app/ponto/management/commands/processar_alertas_turno.py:335` | `turno_aberto`, cron `app/config/crons.py:362` |
| `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:151` | `turno_aberto_24h`, comando retroativo |
| `app/chamados/reconciliador.py:1032` | `batida_ausente` (`:957`), reatar órfã automático (`:925,1776`) |

**Juiz e predicados.** `esta_desligado(colaborador)` compara somente `getattr(colaborador, 'situacao', None) == 'desligado'`. Situação do objeto, sem relógio, data ou vínculo. Agendamento muda data e motivo, efetivação grava situação, reativação muda para ativo e limpa a data. `app/colaboradores/services/desligamento.py:23-32,107-133,235-247`; `app/core/juizes.py:283`.

- `_renascer_por_premissa_viva` barra `situacao != 'ativo'`, incluindo afastado e férias, e barra `via_encerramento == VIA_CASCADE`, um fato do chamado que pode sobreviver à readmissão. Adesão compara a situação exata em `:67`, mas `:91` testa via de encerramento. `app/chamados/models.py:1070-1085`; `app/chamados/services/adesao.py:67,91-93`. Reemissão do reconciliador usa a versão mais ampla `!= 'ativo'`. `app/chamados/reconciliador.py:942`.
- `recusa_da_reabertura` testa veto de dia, admin sem fato novo, premissa regularizada, agrupamento, irmão e motor, sem predicado de desligado ou teste direto de `VIA_CASCADE`. O veto testa módulo, dia e vínculo do emissor. `app/chamados/models.py:1192-1211,1384-1426`.
- Estado exato, com filtros locais adicionais, aparece no Cartório `app/ponto/services/cartorio.py:1803,1868`, furos vetados `app/ponto/services/furos_vetados.py:62` e I5a `app/chamados/management/commands/auditar_invariantes_chamados.py:659-670`. O I5a conta `COBRANCA`, que exclui `registrado`, e roda pelo auditor agendado às 07:18. `app/chamados/catalogo/motor.py:24-32`; `app/config/crons.py:542`.
- Cron agendado usa data preenchida `<= hoje` mais situação diferente. Cadastro do dia no Supra Juiz usa `dia > data_demissao` quando `vinculo_do_dia` retornou `None`. Elegibilidade de turno usa data nula ou posterior ao fim. Rescisão do TXT usa data `<= fim_janela` e retenção/informativo leem o juiz canônico. São decisões com universos distintos. `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:38-40`; `app/ponto/supra_juiz.py:170-175,184-210`; `app/ponto/utils.py:177-180`; `app/folha/export.py:54-60,1215-1237`.

**Reabertura humana e trilha.**

- S93 admin sai do fio e valida POST `motivo_reabertura` e `justificativa_admin` de pelo menos 20 caracteres. Reabre a disputa, cria pergunta de conversa e então chama `ch.reabrir(humano=True)` sem entregar autor ou motivo e sem conferir o retorno. A nota recebe `request.user` e o log da disputa usa `registrar_log(request, ...)`. `app/chamados/views.py:675,955`; `app/chamados/services/fio.py:717-719`; `app/chamados/services/acoes_disputa.py:274-315,338`; `app/core/auditoria.py:8-25`.
- Desarquivar usa `reabrir(humano=True)` quando a via é tranca e registra o ator em resgate/ocorrência, sem campo de motivo. Reabrir período exige POST `motivo`, grava `PeriodoFechado.reaberto_motivo` e autor, mas o chamado recebe só `humano=True` e a trilha do lote passa o ator. `app/chamados/views_arquivar.py:52-88`; `app/ponto/views.py:1548-1575`; `app/ponto/services/fechamento.py:1456-1472`; `app/chamados/services/competencia_trancada.py:264,275,294-300`.
- O detalhe despacha `novo_status` mesmo sem `conteudo`. No fio, o `conteudo` é nota de resposta, exigida neste ramo. O `transicionar` grava `respondido_por` e o evento `estado_chamado` com o usuário. A própria `reabrir` não recebe motivo nem usuário e `_gravar_estado` é chamado sem `user`; em request o escritor do log capta `request.user`. `app/chamados/views.py:548`; `app/chamados/services/acoes_chamado.py:292-301,322-331`; `app/chamados/services/fio.py:862-876`; `app/chamados/models.py:242-244,304-308,469,1428,1508`; `app/core/observ.py:94-138`.
- Recusa cabe no `LogAuditoria`, JSON com porta, colaborador e módulo, por `core.observ.evento`. Já há ações de recusa de nascer/reabrir para outros motivos, com trio equivalente e dedup de ciclo, no veto de dia. `FollowupChamado` exige um chamado e serve apenas quando já existe objeto. `app/core/models.py:285-307`; `app/core/observ.py:94-138`; `app/chamados/models.py:1221-1254,1842`; `app/chamados/services/nota.py:75-89`.
- O retorno de recusa precisa ser consumido pelos chamadores. A abertura humana e criadores de disputa já desreferenciam ou registram followup sobre o resultado sem testar `None`. `app/chamados/views.py:141-143,196-210`; `app/chamados/services/abertura.py:194-202`; `app/chamados/services/disputa_emissao.py:598-606,1134-1142,1369-1376`; `app/chamados/services/nota.py:87-107`.

## 27/11 16:55 · sessao e83cdc28581f · - · - · origem=Ronald (tmux)

## Item 4 — pergunta respondida sem veredito

- O desligamento efetivo percorre apenas disputas abertas, pula as que têm resposta sem veredito, fecha as restantes pela porta `FECHOU_DESLIGAMENTO` e depois tenta `encerrar` cada chamado em `NAO_FECHADOS`. A disputa recusa resposta sem veredito antes de julgar as vivas, inclusive sob trava. Para as vivas que passam, a porta de desligamento compartilha com `admin_encerra` o carimbo `admin_decidiu`, a linha `DecisaoPergunta.SILENCIO` e a trilha. O signal tenta reconciliar o pai. `encerrar` ainda pode recusar pela pergunta viva ancorada. `app/colaboradores/services/desligamento.py:164`, `:182`; `app/chamados/models.py:2123`, `:2175`, `:2184`, `:2199`, `:1779`, `:429`; `app/chamados/signals.py:29`, `:43`.
- O censo de 13/11 ficou velho quanto à definição. A lista usa `DecisaoPergunta.composta`, exige **instante e texto** pelo juiz L-130, sem validação nem materialização, e exclui dia trancado salvo `ignorar_tranca=True`. A guarda do chamado busca perguntas ancoradas também em disputas fechadas, universo que o vigia conhece e que o laço atual do desligamento pula. `app/chamados/models.py:2000`, `:2020`, `:2041`, `:361`; `app/chamados/juizes.py:1644`; `app/chamados/services/vigia_fio.py:746`; `app/colaboradores/services/desligamento.py:164`.
- A via própria de desligamento precisa encerrar **respondidas e mudas**, sem validação de mérito. O registro de 13/11 já substitui expressamente o corte de 28/10 que mandava manter a respondida viva. LU-11 confirma as mudas, LU-12 diz que só o chamado reabre. O valor literal da nova via ainda não está na lista de choices, que ocupa campo de 24 caracteres. `app/docs/BACKLOG.md:732`, `:1211`; `app/docs/PROMPTS.md:2222`; `app/docs/CORTES.md:485`, `:545`, `:546`; `app/chamados/models.py:2836`, `:2872`.
- O molde é **o ramo legado** `VIA_COMPETENCIA_TRANCADA` com carimbo no degrau anterior à resposta do C6 e seu espelho SQL. A tranca atual mata por leitura e grava Pauta informativa ao DP. A via antiga continua legível e tem sua reabertura própria. `app/chamados/models.py:2835`, `:3774`, `:3388`, `:3703`; `app/chamados/juizes.py:1507`, `:1514`; `app/chamados/services/competencia_trancada.py:126`, `:133`, `:137`, `:202`, `:236`, `:241`, `:280`.
- A lista `VIAS_QUE_VENCEM_A_RESPOSTA` afeta C6 e SQL. Acrescentar apenas às identidades não vence o retorno antecipado da resposta. `carimbar` aplica o carimbo com `resposta=None` e `lavrar_veredito=False` por padrão e pode usar o C6 para uma via terminal fora de `vias_que_encerram_por_identidade`. `carimbar_identidade` exige pertinência à sua lista. O juízo de lastro é independente e, sem ramo explícito, entende escolha carimbada como fato, hora/data-hora exige prova, texto exige completude/retratação. Não confundir terminalidade com `VIAS_FORA_DO_FATO`, que rege identidade, dedup e índices. `app/chamados/juizes.py:729`, `:756`, `:758`, `:864`, `:1514`, `:1521`, `:1541`; `app/chamados/models.py:2821`, `:2835`, `:3414`, `:3434`, `:3462`, `:3484`, `:3511`, `:3544`, `:3036`, `:3067`, `:3947`, `:3953`; `app/chamados/veredito_lavrado.py:262`.
- A reabertura permitida de chamado hoje dispara `ressuscitar_perguntas`. Seu universo inclui perguntas mudas carimbadas, simula descarimbar, limpa os campos e pode chamar `DisputaSupervisao.reabrir_sistema`. Isso deve ser coberto para LU-12. `reabrir_admin` hoje pode abrir diretamente a disputa e plantar conversa antes de tentar o chamado. `app/chamados/models.py:1517`, `:1549`, `:1582`, `:1599`, `:1739`, `:1749`, `:2396`, `:2433`; `app/chamados/services/acoes_disputa.py:296`; `app/docs/CORTES.md:546`.
- Mostrar como informativa precisa contemplar `estado` (`RESOLVIDA` para morta carimbada), `estado_aplicacao_pergunta` (hora respondida carimbada cai em `aplicada` fora das duas vias da tabela atual), o card com fallback do choice e o explicador, que lê lastro e estados de recusa. A fila normal já lê C6 e SQL. `app/chamados/models.py:3361`; `app/chamados/services/materializacao.py:868`, `:890`, `:904`; `app/chamados/services/fio.py:466`; `app/templates/chamados/partials/modal_fio.html:230`; `app/templates/chamados/partials/_card_atendimento.html:195`; `app/chamados/services/explicador.py:123`, `:131`; `app/chamados/juizes.py:1863`.
- Validar e recusar têm guardas de resposta/validação e de disputa fechada quando há cartão esperado. Há validação sem cartão em API foto e comando. Aceitar folga ocorre antes do teste de `materializada_em`. Conferir que a via nova não vire decisão na rota direta. O `post_save` da disputa também agenda retomar regularizações já confirmadas, cujo materializador pode chamar `registrar_batida`. Incidência em produção [nao verificado]. `app/chamados/services/validacao.py:747`, `:756`, `:777`, `:813`, `:948`, `:980`, `:999`; `app/api/foto_recebida.py:93`; `app/chamados/management/commands/validar_classe_a.py:132`; `app/chamados/signals.py:122`, `:149`, `:252`; `app/chamados/services/regularizacao_ext.py:78`, `:219`.

## Demais ramos que leem a via

| Ramo atual | Efeito de uma via nova |
|---|---|
| Projeção e chave do memo | Memo incorpora o valor, projeção anterior ao carimbo limpa a via salvo `fora_do_fato`. `app/chamados/juizes.py:901`, `:1486`. |
| Ressuscitação, descarimbos e re-lavra | Portas gerais poupam `fora_do_fato`; uma via nova selecionada pode ser removida. Re-lavra só troca `declaracao`/`completude_turno`. `desfazer_carimbo_sem_lastro` admite candidata fora de identidades e só limpa se C6 der viva. `app/chamados/models.py:1554`, `:1678`, `:1723`, `:3570`, `:3653`, `:3659`; `app/chamados/services/disputa_emissao.py:1689`, `:1710`; `app/chamados/services/relavra_via.py:118`, `:155`, `:181`, `:242`. |
| Rótulos/escritores particulares | Admin e fato-espelho podem trocar uma via diferente. Fantasma tenta pelo C6. Nova resposta reabre apenas `VIAS_SEM_LASTRO`, competência reabre somente a via de tranca. A antiga inversa de fantasma saiu. `app/chamados/models.py:2325`, `:3377`, `:3667`, `:3678`, `:3683`, `:3699`, `:3706`. |
| Absorção, inversão e seleção por duplicata | Nova fica fora de absorção e fila de paridade, dentro da seleção que só exclui duplicata. O ramo do cartório exige que todas sejam duplicata. `app/chamados/models.py:4540`; `app/chamados/services/materializacao.py:275`, `:1202`; `app/chamados/services/fabrica_da_celula.py:149`; `app/ponto/services/flip_auto.py:129`; `app/ponto/services/cartorio.py:1705`. |
| Mérito, retratação, dedup e identidade | Proteção de mérito é só admin, no-op de retratação só duplicata. Se ficar fora de `VIAS_FORA_DO_FATO`, nova permanece nos filtros de identidade e dedup. `app/chamados/services/fio.py:1260`, `:1327`, `:1407`, `:1684`, `:1767`, `:2208`, `:2244`, `:2266`, `:2312`; `app/chamados/services/disputa_emissao.py:337`, `:409`, `:428`, `:446`; `app/chamados/services/tarefa_do_dia.py:262`; `app/api/foto_recebida.py:45`; `app/chamados/models.py:3036`, `:3067`, `:3947`, `:3953`. |
| Comandos | I6 acusa desconhecida se atingir `_i6_pares` e faltar no piso; F1t pode classificar como `carimbo/celula`; I9 só acusa via vazia. Relavra avulsa chama toda não derivada de “via de DECISAO”. Backfill só apanha `admin_decidiu` em seu filtro de decisão. Fecho-espelho só toca `SEM_HISTORIA`. `app/chamados/management/commands/auditar_invariantes_chamados.py:47`, `:204`, `:435`, `:462`, `:474`, `:546`, `:961`, `:1740`; `app/chamados/management/commands/relavrar_via_i6.py:58`, `:88`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:129`, `:364`, `:432`, `:540`; `app/chamados/management/commands/encerrar_perguntas_com_fato.py:34`, `:43`, `:67`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:70`. |
| Transporte e trilha | Composta, snapshots, refresh, histórico e contagens abertas aceitam o valor, sem branch próprio. `app/chamados/models.py:2186`, `:3589`, `:4507`; `app/chamados/reconciliador.py:1287`; `app/chamados/services/disputa_emissao.py:2182`; `app/chamados/services/materializacao.py:307`, `:1245`, `:1786`; `app/chamados/services/passivo_validado.py:69`, `:148`, `:578`; `app/chamados/services/relavra_via.py:123`, `:245`; `app/chamados/services/validacao.py:377`, `:471`; `app/chamados/management/commands/bug_par_103.py:67`; `app/chamados/management/commands/estoque_l130_instante_sem_texto.py:59`. |

`I5a` é o contador mais próximo de vivos de desligado. Percorre `VIVOS` e só conta `COBRANCA` com `situacao` crua, portanto `registrado` fica fora. O vigia roda às 07:18 e o placar depende dele. O contador literal `vivos_de_desligado` está no backlog. `app/chamados/management/commands/auditar_invariantes_chamados.py:643`, `:659`, `:670`; `app/chamados/catalogo/motor.py:24`, `:29`; `app/config/crons.py:542`, `:747`, `:2194`; `app/docs/BACKLOG.md:732`, `:733`.

## 27/11 16:54 · sessao aa0d1df7f86a · - · - · origem=Ronald (tmux)

**3. Auditoria S89**

- O nascimento está em `app/colaboradores/services/desligamento.py:195`, depois da cascata de disputas e chamados em `:164` e `:182`. Entra por `ChamadoColaborador.criar`, com `area='dp_rh'`, `categoria='auditoria_clt'`, `subcategoria='registrar_resposta_oficial'`, `modulo_origem='desligamento_revisao'`, urgência normal e JSON de data, motivo, autor e instante (`:205`, `:208`, `:209`, `:216`). O catálogo marca módulo de auditoria, e `criar` traduz o estado inicial para `registrado`. Esse estado pertence a `VIVOS` e `NAO_FECHADOS` e fica fora de `COBRANCA`. `app/chamados/catalogo/natureza.py:18`, `app/chamados/models.py:1113`, `app/chamados/models.py:1130`, `app/chamados/catalogo/motor.py:24`, `app/chamados/catalogo/motor.py:134`.
- A view `desligar_colaborador` julga e chama o executor. O cron seleciona demissão vencida de cadastro ainda não desligado, imprime em dry e, em apply, revalida e chama o mesmo executor com autor de request nulo. A agenda declara `--apply` às 06:06, papel `agenda`. `app/colaboradores/views.py:1464`, `app/colaboradores/views.py:1472`, `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:37`, `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:46`, `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:57`, `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:66`, `app/config/crons.py:440`, `app/config/crons.py:2309`.
- O push remete ao detalhe local, com descrição inteira, followups e ação regular de resposta/status. O fio e o modal gerenciais leem todos os chamados do colaborador, mostram descrição truncada e timeline. O painel lateral lê JSON e timeline. A lista do colaborador exclui o módulo interno. `app/colaboradores/services/desligamento.py:219`, `app/chamados/push_utils.py:64`, `app/chamados/urls.py:14`, `app/chamados/views.py:363`, `app/chamados/views.py:550`, `app/templates/chamados/detalhe_local.html:97`, `app/templates/chamados/detalhe_local.html:112`, `app/chamados/services/acoes_chamado.py:292`, `app/chamados/services/acoes_chamado.py:322`, `app/chamados/services/fio.py:228`, `app/chamados/views.py:678`, `app/chamados/views.py:890`, `app/templates/chamados/fio_colaborador.html:231`, `app/templates/chamados/partials/modal_fio.html:487`, `app/chamados/views.py:347`, `app/templates/chamados/partials/painel_chamado_turno.html:35`, `app/chamados/services/janela_colab.py:36`, `app/chamados/services/janela_colab.py:74`. Entrega do push em produção [nao verificado].
- Leitores de catálogo e contagem. `data_demissao`, escopo dia e dono DP em `app/chamados/catalogo/modulos.py:141`. Exceção da tranca em `:265`, consumida em `app/chamados/models.py:1107` e `app/chamados/services/competencia_trancada.py:113`. Rótulo e ação de registro no desenlace `app/chamados/services/desenlace.py:30`, `:85`, comentário RH `app/chamados/services/verbos.py:176`. Painel aceita drill `registrado` e conta a fila de cobrança em `app/chamados/services/painel.py:189`, `:212`, `:307`, `:316`. Modal ativos e Inteligência seguem a fila em `app/chamados/views.py:901` e `app/inteligencia/views.py:101`. Lente Registros agrupa e conta por módulo em `app/chamados/views.py:1482` e `app/chamados/services/verbos.py:199`, `:225`. Placar Equipe conta resolução assinada por staff e validação em `app/chamados/services/verbos.py:242`, `:253`, `:263`. API Mensageria lista o registro sem verbo e conta no balde `registro_auditoria`, e a visão do colaborador lê total de vivos. `app/api/views_mensageria.py:390`, `:452`, `:459`, `:1043`, `:1082`. Vigia do fio admite registro com célula no agregado de vivos e contabiliza célula acesa só de módulo fora da lista de furos como fora do vocabulário, sem alarmá-la. `app/chamados/services/vigia_fio.py:322`, `:337`, `:343`, `:401`.
- `HistoricoVinculo` já recebe o desligamento agendado e efetivo pelo escritor existente. Tem observação limitada a 400, data, motivo e autor. A edição do cadastro apresenta a observação, o painel de vínculo lê seis linhas e o fio completo a apresenta no tooltip do histórico. `app/colaboradores/models.py:548`, `:577`, `:579`, `app/colaboradores/services/historico_vinculo.py:20`, `app/colaboradores/services/desligamento.py:112`, `:138`, `app/templates/colaboradores/editar.html:179`, `app/colaboradores/views.py:306`, `:317`, `app/inteligencia/resumo.py:51`, `app/templates/chamados/fio_colaborador.html:88`.
- `LogAuditoria` também já recebe o agendado e o efetivo. O primeiro dentro do atomic, o segundo via `registrar_log` após o bloco. O modelo comporta ref do colaborador, JSON de antes/depois, usuário e ator de sistema. A tela genérica requer `ver_saude_sistema`, semeada para TI. Cadastro tem `editar_cadastro` semeada para DP e expõe o histórico, logo armazenar JSON sozinho não garante que o informativo chegue ao DP. `app/colaboradores/services/desligamento.py:118`, `:227`, `app/core/auditoria.py:8`, `:22`, `app/core/observ.py:94`, `:133`, `app/core/models.py:285`, `:300`, `app/core/views.py:686`, `app/colaboradores/mixins.py:363`, `:370`, `:375`, `app/colaboradores/views.py:1259`.
- O S88 mencionado pelo escritor é a revisão de roadmap que motivou o chamado S89. O signal também se chama S88 Defesa 1 e tenta resolver o pai após fechar a disputa pela porta do chamado. O aval item 2 substitui a decisão de nascimento e a exceção antiga de contar S89 à parte. `app/colaboradores/services/desligamento.py:191`, `app/chamados/signals.py:19`, `:43`, `app/docs/RELATO.md:4243`, `aval.txt:1` (anexo).
- Os testes atuais exigem S89 e o seu payload, preservação da respondida e via `admin_decidiu` para a muda. Precisam de revisão conforme aval e LU-11. `app/colaboradores/tests/test_desligar_guards.py:73`, `:93`, `:117`, `:120`, `app/docs/CORTES.md:545`.

**6. Medição**

- O nome `vivos_de_desligado` aparece como condição de pronto do backlog. O equivalente mais próximo em execução é I5a, que percorre vivos e conta apenas `COBRANCA` com comparação crua `situacao == 'desligado'`. Perde `registrado`, portanto o S89 normal, e não tem trilha de recusas no universo. É o `auditar_invariantes_chamados` das 07:18, papel `vigia`, imprime a medida em stdout e no HTML. O exit 2 atual só observa F1/F2. `app/docs/BACKLOG.md:732`, `:733`, `app/chamados/management/commands/auditar_invariantes_chamados.py:643`, `:659`, `:670`, `:1867`, `:1886`, `app/config/crons.py:542`, `:2194`.
- Encaixe de desenho para o contador solicitado é o mesmo vigia I5a, com uma medida de tentativas cujas trilhas vêm de `LogAuditoria` e a medida de estoque usando `VIVOS` e `esta_desligado`. Existe molde de trilha `nascimento_recusado`/`reabertura_recusada` com módulo, colaborador e porta, deduplicado por chave e ciclo. Essa deduplicação faz a quantidade de linhas diferir da quantidade de tentativas quando a chave se repete. `app/chamados/management/commands/auditar_invariantes_chamados.py:637`, `app/chamados/models.py:1221`, `:1232`, `:1254`, `app/colaboradores/services/desligamento.py:23`, `app/chamados/catalogo/motor.py:24`.
- O outro contador da família, `desligados_retidos`, conta colaborador × competência retido pelo export e passa ao `vigiar_fio_morto`/carimbo. É universo distinto do estoque de chamados e da trilha. Se o contador novo precisar de publicação no ESTADO, há o molde de `vigiar_fio_morto` com carimbo e leitor `fio_estado`, e o placar operacional já tem leitor do carimbo para duas chaves. `app/folha/fora_da_folha.py:166`, `app/chamados/services/vigia_fio.py:565`, `:580`, `app/chamados/management/commands/vigiar_fio_morto.py:243`, `:275`, `:289`, `bin/fio_estado.py:122`, `:192`, `app/core/management/commands/pipeline_placar.py:85`, `:135`. O vigia de esteira trabalha com arquivos de fila e estado de fatias. Os juízes por varredura são dívida que só encolhe. `app/core/esteira_vigia.py:1`, `app/config/crons.py:1985`.
- Lei a esclarecer para o selo de zero: uma reabertura humana autorizada entra em `em_analise`, logo conta como vivo de desligado. O aval fornece simultaneamente a exceção humana e o selo zero. Unidade, eventual exclusão e momento desse selo em relação à reabertura [nao verificado]. `aval.txt:1` (anexo), `app/chamados/models.py:1428`, `app/chamados/catalogo/motor.py:24`.

## 27/11 16:45 · sessao d019348cce5d · CODEX-CONSTROI-MOTOR · construtor · origem=Code

Implementei a cura14/P4 em `core/esteira_vigia.py` e `core/tests/test_vigia_ccm.py`. O mesmo juiz CCM atende a guarda prévia e o reteste. Se a guarda recusa, o vigia avisa Pauta e RELATO com a primeira linha do juiz e retorna antes de Git, deploy e gravação de `ultimo_revert`. [esteira_vigia.py:748](/home/ronald/wt-codex-codex-constroi-motor-163727/app/core/esteira_vigia.py:748) [esteira_vigia.py:1320](/home/ronald/wt-codex-codex-constroi-motor-163727/app/core/esteira_vigia.py:1320)

Na recusa tardia, o vigia tenta desfazer o revert. O deploy compensatório só roda se o Git retornar zero, e `revert desfeito` exige sucesso dos dois comandos. Uma falha em qualquer etapa aparece na Pauta e no RELATO, com teste para cada caso. A guarda depende de `_rodar_testes` devolver `None`, o que ocorre depois de escrever o log de recusa. [esteira_vigia.py:1339](/home/ronald/wt-codex-codex-constroi-motor-163727/app/core/esteira_vigia.py:1339) [test_vigia_ccm.py:189](/home/ronald/wt-codex-codex-constroi-motor-163727/app/core/tests/test_vigia_ccm.py:189) [esteira_vigia.py:753](/home/ronald/wt-codex-codex-constroi-motor-163727/app/core/esteira_vigia.py:753) [nao verificado]

**Pré-verificação.** O diff passou em `git diff --check`. A base, a cura e os três mutantes compilam, com 25 `ExceptHandler` e um único `argv` do CCM. A guarda usa o `agora` já recebido. `deve_reverter` lê `ultimo_revert` para aplicar o teto. `passada` regrava o estado que leu antes de chamar `auto_revert`, então preserva o valor carregado na recusa prévia e pode sobrescrever um carimbo novo se a tentativa chegou ao reteste. [esteira_vigia.py:749](/home/ronald/wt-codex-codex-constroi-motor-163727/app/core/esteira_vigia.py:749) [esteira_vigia.py:1076](/home/ronald/wt-codex-codex-constroi-motor-163727/app/core/esteira_vigia.py:1076) [esteira_vigia.py:1319](/home/ronald/wt-codex-codex-constroi-motor-163727/app/core/esteira_vigia.py:1319) [esteira_vigia.py:1548](/home/ronald/wt-codex-codex-constroi-motor-163727/app/core/esteira_vigia.py:1548) [esteira_vigia.py:1657](/home/ronald/wt-codex-codex-constroi-motor-163727/app/core/esteira_vigia.py:1657) [LEIS.md:97](/home/ronald/wt-codex-codex-constroi-motor-163727/app/docs/LEIS.md:97) [nao verificado]

**Pista pendente.** O ambiente disponível é a VM. A Pista exige rede e `logs/CODEX-CONSTROI-MOTOR/cura14/red_p4.log` está em caminho ignorado. O log ficou pendente. O RED na base, o verde na cura e a mordida dos mutantes seguem [nao verificado]. A certificação cabe ao Code e ao opus final. [/etc/hostname:1](/etc/hostname:1) [CLAUDE.md:129](/home/ronald/wt-codex-codex-constroi-motor-163727/CLAUDE.md:129) [.gitignore:53](/home/ronald/wt-codex-codex-constroi-motor-163727/.gitignore:53) [LEIS.md:147](/home/ronald/wt-codex-codex-constroi-motor-163727/app/docs/LEIS.md:147)

### Comandos para o Code na Pista

Rodar a partir de `app/`, no ambiente de teste da Pista. A função carrega a variante em memória e usa os testes da cópia. Os alvos dos mutantes correspondem às três asserções pedidas. [manage.py:7](/home/ronald/wt-codex-codex-constroi-motor-163727/app/manage.py:7) [test_vigia_ccm.py:146](/home/ronald/wt-codex-codex-constroi-motor-163727/app/core/tests/test_vigia_ccm.py:146) [test_vigia_ccm.py:162](/home/ronald/wt-codex-codex-constroi-motor-163727/app/core/tests/test_vigia_ccm.py:162) [test_vigia_ccm.py:172](/home/ronald/wt-codex-codex-constroi-motor-163727/app/core/tests/test_vigia_ccm.py:172) [nao verificado]

```bash
set -o pipefail
vigia=core/esteira_vigia.py
classe=core.tests.test_vigia_ccm.AutoRevertCCMTest
recusa="$classe.test_auto_revert_juiz_recusa_antes_do_revert"
ordem="$classe.test_auto_revert_ordem_juiz_antes_do_git"
desfaz="$classe.test_auto_revert_reteste_recusado_desfaz"

p4() {
    PYTHONDONTWRITEBYTECODE=1 python3 -c '
import sys
from core import esteira_vigia as vigia
from manage import main

exec(compile(sys.stdin.read(), vigia.__file__, "exec"), vars(vigia))
sys.argv[0] = "manage.py"
main()
' test --settings=config.settings.ci --noinput -v 2 "$@"
}

# RED na base. Capturar esta saída em logs/CODEX-CONSTROI-MOTOR/cura14/red_p4.log.
git show 0e0497a1ce54c3de18be24660cf8a35ec9de6680:app/core/esteira_vigia.py \
    | p4 "$recusa" "$ordem" "$desfaz"

# Cura.
p4 core.tests.test_vigia_ccm < "$vigia"

# juiz_depois_do_revert
sed -e '/^    # O revert restaura/,/^        return .sem revert: reteste seria recusado/{H;d;}' \
    -e '/^        return .revert nao aplicou./G' "$vigia" \
    | p4 "$recusa" "$ordem"

# recusa_segue
sed '/^def auto_revert(/,/^def explicar_vermelho(/s/^    if juiz.returncode != 0:$/    if False:/' \
    "$vigia" | p4 "$recusa"

# none_mantem_revert
sed "/^    desfecho_recusa = None$/,/^    memoria\[/s/^        if subprocess.run(.*'HEAD'.*returncode != 0:$/        if False:/" \
    "$vigia" | p4 "$desfaz"
```

## 27/11 16:44 · sessao a99d2197ef19 · - · - · origem=Ronald (tmux)

A recusa tardia apura o resultado da compensação, emite Pauta e RELATO e então grava `ultimo_revert`. [core/esteira_vigia.py:1339] [core/esteira_vigia.py:1347] [core/esteira_vigia.py:1350]

## 27/11 16:43 · sessao a99d2197ef19 · - · - · origem=Ronald (tmux)

A compensação agora precede a persistência, e o sucesso exige retorno zero de Git e deploy. O teste cobre separadamente os dois códigos de falha e os respectivos avisos. [core/esteira_vigia.py:1339] [core/tests/test_vigia_ccm.py:189] [nao verificado]

A gravação do estado ainda precede os avisos. Se ela lançar `OSError`, a compensação terá sido tentada, mas Pauta e RELATO podem ficar sem registro. [core/esteira_vigia.py:823] [core/esteira_vigia.py:1347]

## 27/11 16:42 · sessao a99d2197ef19 · - · - · origem=Ronald (tmux)

O pré-verificador confirmou por AST 25 `ExceptHandler` na base e na cura, um único argv do CCM e a leitura de relógio já existente. A guarda prévia retorna antes de Git, deploy e gravação. [core/esteira_vigia.py:749] [core/esteira_vigia.py:1304] [core/esteira_vigia.py:1322] [nao verificado]

`deve_reverter` lê `ultimo_revert` para aplicar o teto. A recusa prévia preserva o valor carregado. Quando a tentativa chega ao reteste, `passada` pode sobrescrever o carimbo novo ao regravar o estado lido antes de chamar `auto_revert`. [core/esteira_vigia.py:1076] [core/esteira_vigia.py:1338] [core/esteira_vigia.py:1542] [core/esteira_vigia.py:1630] [core/esteira_vigia.py:1651]

Na recusa tardia, uma falha na gravação do estado pode impedir o rollback. Os retornos do Git e do deploy compensatórios ficam sem conferência, permitindo anunciar `revert desfeito` se um dos comandos falhar. [core/esteira_vigia.py:823] [core/esteira_vigia.py:1339] [core/esteira_vigia.py:1341] [nao verificado]

O diff cobre os efeitos pedidos nos três selos. A execução na Pista e a mordida dinâmica seguem [nao verificado]. [core/tests/test_vigia_ccm.py:144] [core/tests/test_vigia_ccm.py:160] [core/tests/test_vigia_ccm.py:170]

## 27/11 16:23 · sessao df030164fcae · FERIADO-BOTAO-CORTADO · construtor · origem=Code

A c11 põe o gatilho do feriado acima da região rolável na ordem de pintura da célula. O template isola o empilhamento da célula mensal e usa `z-index:1` na ação e `z-index:0` na região. A célula segue com 132 px, e as marcações continuam no fim da região rolável, como na c10. A guarda B1 do histórico permanece no JS. [_calendario_grade.html:68](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/partials/_calendario_grade.html:68) · [Ação e região:95](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/partials/_calendario_grade.html:95) · [Ordem das marcações:132](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/partials/_calendario_grade.html:132) · [Guarda B1:27](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/static/js/popover-acao.js:27)

Os **104 px** vêm da sonda. Ela posiciona a ação sobre o conteúdo da célula para deixar pouco espaço vertical no drawer de 128 px. No s11, o retângulo cabe na célula e na viewport, mas a sonda também exige que o centro receba o clique. A correção trata a sobreposição com a grade rolável da c10. Qual elemento recebeu o clique no s11 é **[não verificado]**. [Posicionamento:277](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:277) · [Medição:291](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:291)

O **RED** é o próprio teste que acusou as quatro falhas do s11. O mutante novo `drawer-sem-empilhamento-da-acao` volta `isolation` e `z-index` a `auto` só no drawer, repondo o empilhamento da c10. A revisão estática confirmou que a sonda, as asserções, os esperados e os mutantes anteriores são idênticos aos da c10. Contagem no diff **0 `except`, 0 `catch`**. [Mutante:78](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:78) · [Teste RED:550](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:550) [nao verificado]

**Recorte para a sessão na VM**, com o smoke de clique e histórico, a renderização da grade e os selos do popover. [Uso de `--only`:13275](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/docs/RELATO.md:13275)

```bash
PYTHONDONTWRITEBYTECODE=1 bash bin/suite.sh --only "core.tests.test_smoke_clique_feriado_vira_trabalho colaboradores.tests.test_feriado_botao_cortado core.tests.test_selo_popover_acao"
```

**Comando do mutante** no ambiente de testes da VM. [Seleção do mutante:550](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:550) [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 SMOKE_FERIADO_MUTANTE=drawer-sem-empilhamento-da-acao \
  python manage.py test --settings=config.settings.ci --noinput \
  core.tests.test_smoke_clique_feriado_vira_trabalho.SmokeCliqueFeriadoViraTrabalhoTest.test_clica_no_feriado_na_pagina_e_no_drawer_das_duas_cascas
```

Resultado da c11 em Chromium e falha do mutante pelo motivo esperado **[não verificado]**. A certificação fica com a suite e o opus final. [nao verificado]

## 27/11 16:16 · sessao b845a7dd5be1 · FERIADO-UM-JUIZ · verificador · origem=Code

# REPROVADO

1. **K1 continua aberto.** O turno partido de `10:00–14:00 + 14:30–18:20` leva **470 min à HE100 feriado** na promoção. O corte do K1 exige **440 min de HE100 feriado e 30 min de intra indenizada**. O laço do motor que apura a supressão percorre `periodos`, e a promoção soma os minutos de `periodos_ft`. O teste continua como `expectedFailure`. (`app/ponto/motor_calculo_v2.py:3078–3118`, `app/ponto/services/fechamento.py:993–997`, `app/ponto/tests/test_feriado_um_juiz_porta.py:380–394`, `app/docs/RELATO.md:255`)
2. **A certeza da escala ainda pode decidir quantos minutos chegam à partição.** Com previsto zero, empresa sujeita à janela e sem autorização, a L-097 conserva o par de folga com escala incerta e zera o par com escala certa. O teste que cobra a mesma quantidade nas duas rubricas está em `skip`. A precedência entre esse bloqueio e o aval literal continua pendente. (`FERIADO-UM-JUIZ-2711-1210.txt:1`, `app/docs/LEIS.md:115`, `app/ponto/motor_calculo_v2.py:1562–1597`, `app/ponto/tests/test_feriado_um_juiz_porta.py:499–515`)
3. **A vigência protege a classificação antiga da 10, mas o aval também veda tocar exportada.** A guarda específica cobre a 09. Nas demais, o evento pode regravar o mensal e as duas lavras. Na 11 e seguintes, isso já usa a regra nova mesmo se houver TXT entregue. O POST pode gerar outra fotografia a partir do cálculo online. (`FERIADO-UM-JUIZ-2711-1210.txt:1`, `app/ponto/services/fechamento.py:36–50,600–639,1189–1223,1294–1328,1829–1855`, `app/folha/views.py:214–219,269–275,302–307`)
4. **A data e o vínculo ainda podem divergir.** Reproduzi em memória o turno do dia 21 cair na competência anterior quando o feriado é no dia 20. Também reproduzi na partição 4 h de HE100 feriado para um par cujo dia do turno pertence a 12x36, usando o contexto 6x1 que trouxe o par e a data herdada da véspera. (`app/ponto/services/fechamento.py:154–172,223–230`, `app/ponto/motor_calculo_v2.py:2513–2522,2537–2545`, `app/ponto/calculador/regras.py:410–431`)
5. **As três gravações ainda podem divergir.** O `atomic` protege o `delete` e o `bulk_create` de uma versão de `DiaPago`. O mensal é salvo antes, e as falhas das duas lavras são capturadas separadamente. Uma falha pode deixar o mensal novo com uma ou ambas as lavras antigas. (`app/ponto/services/dia_pago.py:355–363`, `app/ponto/services/fechamento.py:1166–1223,1267–1270,1328–1332`)
6. **Três testes estruturais falham na própria cópia.** Executei os testes de AST em memória. A catraca de endereços registra cinco sítios divergentes, incluindo `folha/export.py:505` onde o censo encontra `:514`. Os dois testes do dia da lavra rejeitam a atribuição `dia_jornada = dia_turno`, embora a linha seguinte atualize o mapa. (`app/core/tests/test_medida_turno_do_par.py:286–289,326–336,565–569`, `app/folha/export.py:514`, `app/ponto/tests/test_noturno_v8_ft_no_dia_da_lavra.py:46–71`, `app/ponto/calculador/regras.py:420–430`)
7. **Pouso e apply [nao verificado].** O aval exige medir antes os 75 dias de 42 colaboradores como hora comum, excluir os intermitentes e entregar reversão, antes × depois por colaborador e DIFF do TXT 11. A prova desses itens, a suíte inteira neste hash e o pouso de E1 antes do K1 estão [nao verificado]. O K1 tem aval separado e condicionado ao DIFF de frota por rubrica antes do pouso. (`FERIADO-UM-JUIZ-2711-1210.txt:1`, `app/docs/RELATO.md:252,255`, `app/docs/PROMPTS.md:3016`, `CLAUDE.md:448–467`)

## Ver3, achado por achado

| Achado do ver3 | Situação na c4 |
|---|---|
| **1. TXT perde a primeira fatia, `cadastro_zero` ou resumo perde a rubrica** | **Curado no caso reproduzido.** Emissão, homologação e resumo do cartão usam `eventos_do_fechamento_da_competencia`. A 11 admite FT promovida com `horas_trabalhadas=0` quando a rubrica está cadastrada e o colaborador está sujeito ao ponto. O comparador acusa a rubrica ausente com HE100 bruta igual. O PDF discrimina a parcela no resumo. (`app/folha/export.py:388–394,750–760,924–926,1023–1032`, `app/relatorios/cartao_pela_celula.py:89–101,250–277`, `app/relatorios/pdf_espelho.py:771–779`, `app/ponto/tests/test_feriado_um_juiz.py:56–99`) |
| **2. Exportada fora da guarda pode mudar** | **Curado para a promoção retroativa da FT incerta até a 10. Aberto para a proteção do gravado.** A fixture da 10 força `entra`, cadastra apenas a rubrica de feriado e chama `registrar` diretamente. Nesse caso, a fotografia anterior tem zero linhas de eventos. O teste compara seis campos do mensal, campos escolhidos das lavras e o registro anterior. A reemissão pelo POST e a 11 já entregue ficam sem essa cobertura. (`app/ponto/calculador/regras.py:393–409`, `app/ponto/tests/test_feriado_um_juiz_porta.py:151–160,239–296`, `app/folha/export.py:539–548,750–765`, `app/folha/views.py:214–219,269–275,302–307`) |
| **3. Oráculo e DIFF redecidem a dobra** | **Curado para a HE100 feriado normal alimentada a partir da 11.** O oráculo recebe os minutos dos períodos da fatia, inclusive zero com calendário injetado vazio. `periodo.reler` também passa a carregar os minutos do período. O DIFF usa a mesma projeção FT nos dois lados, então `4,00 = 4,00` prova a ligação da partição com o comando. O caminho do oráculo ainda calcula intra e, fora da projeção promovida, noturno e splits com a régua do recorte. (`app/ponto/calculador/alimentacao.py:429–446,518–523,571–597`, `app/ponto/calculador/periodo.py:764–769,824–843`, `app/ponto/calculador/regras.py:793–817,909–932`, `app/ponto/management/commands/diff_calculador.py:244–257,403–415`) |
| **4. Split bruto sobrescrito ou acima do total** | **Curado no recorte da FT promovida.** A c4 preserva o campo bruto, limita o split aos minutos trabalhados e usa a união da janela no plantão longo com prorrogação. O controle em memória de 21h a 23h do dia seguinte deu **1.500 min de split**, com adicional legal de **1.500 ÷ 0,875 min**. A correção do bruto em outros caminhos fica fora desse ramo. (`app/ponto/calculador/regras.py:432–447`, `app/ponto/motor_calculo_v2.py:82–118,151–161`, `app/ponto/tests/test_feriado_um_juiz.py:331–365`, `app/ponto/tests/test_feriado_um_juiz_porta.py:572–581`) |
| **5. K1 e dia do turno** | **K1 aberto. Data parcialmente curada.** Há asserções de data e rubrica nas duas lavras para dois turnos e troca de praça. A correção do mapa é condicional ao ciclo, ao feriado e à presença do dia na fatia. O corte de data e a classificação original do período já aconteceram quando ela roda. (`app/ponto/tests/test_feriado_um_juiz_porta.py:380–394,455–497`, `app/ponto/calculador/regras.py:410–431`, `app/ponto/motor_calculo_v2.py:2537–2574`) |
| **6. Soma, idempotência e falha da relavra** | **Curado o rollback de uma versão. Paridade parcial.** O teste sem promoção exige **8,04 h** no mensal para `241 + 241 min`. A soma diária desses minutos arredonda para **8,03 h**. Com uma FT promovida de 10 segundos, mesmo aparecendo como `0,00 h` de feriado no teste, o mensal passa para **8,03 h** nas horas comuns. Duas execuções bem-sucedidas podem repetir os valores, mas recriam linhas e carimbos. A prova de concorrência e de atomicidade entre mensal e versões está [nao verificado]. (`app/ponto/services/fechamento.py:1005–1013`, `app/ponto/services/dia_pago.py:344–363,552–565`, `app/ponto/models.py:373–387`, `app/ponto/tests/test_feriado_um_juiz_porta.py:396–453`) |

## Aval, vigência e alcance

- **A regra principal entrou.** Em 6x1, a FT já apurada consulta `_feriado_dobra_no_dia` pelo dia do turno. Com dobra devida, a FT incerta vira HE100 feriado. Com escala certa, `feriado_vira_folga` separa HE100 feriado de folga trabalhada a 100%. O diff fornecido preserva `motor_calculo_v2.py` e `CICLOS_FERIADO_SIMPLES`. O intermitente é excluído por base ou ciclo e seu motor injeta calendário vazio. Um período da própria fatia 12x36 fica fora da promoção. A emissão e a troca de vínculo ainda admitem os casos dos bloqueios 3 e 4. A cláusula cadastrada da CCT no caminho regular 12x36 continua no código. (`diff.txt:1`, `app/ponto/calculador/regras.py:314–322,367–377,410–431`, `app/ponto/motor_calculo_v2.py:941–978,3686–3698,3740`, `app/core/regua_cct.py:466–479`)
- **A partir da competência 11/2026**, mudam a promoção, o consumo da HE100 feriado no TXT e no cartão, a projeção nova para o oráculo, o ajuste noturno da FT promovida e o arredondamento em mensal com promoção positiva. Até a 10, a partição e a emissão históricas conservam o critério anterior. A borda final muda já na 11 e a inicial só na 12. O `atomic` da lavra, a comparação do cartão e a exibição no PDF valem para todas as chamadas. `periodo.reler` também mudou sem parâmetro de competência. (`app/folha/export.py:385–394,924–926`, `app/ponto/calculador/regras.py:386–447`, `app/ponto/services/fechamento.py:223–230,989–1013,1294–1305`, `app/ponto/services/dia_pago.py:358–362`, `app/ponto/calculador/periodo.py:539,764–769`, `app/relatorios/cartao_pela_celula.py:266–273`, `app/relatorios/pdf_espelho.py:771–779`)
- **Transportar a rubrica da fatia para o TXT cabe no aval.** Corrigir a data da rubrica preserva o objetivo enquanto usa o turno que já foi julgado. O filtro de batidas da borda atua **antes** do motor e pode alterar a apuração de um turno comum. A união altera também `horas_noturnas`, que sai como adicional no TXT. O arredondamento condicionado à promoção altera horas comuns de outros dias do mesmo mensal. Esses efeitos vão além da redistribuição entre HE100 feriado e folga trabalhada. A faixa de impacto na frota está [nao verificado]. (`FERIADO-UM-JUIZ-2711-1210.txt:1`, `app/ponto/services/fechamento.py:154–172,339–348,998–1013`, `app/folha/export.py:453–462`, `CLAUDE.md:274–277`)
- **A 10 fica igual ao gravado?** O corte preserva a regra para os mesmos insumos. Igualdade com o gravado real, com ambas as lavras e com o TXT vigente [nao verificado]. A própria cópia registra que a 10 foi paga à mão, sem TXT, e saiu da guarda. A condição “10 exportada” está na fixture e na pergunta. A L-113 permite recálculo genérico fora da guarda, enquanto o aval da obra exige preservar exportadas. (`app/ponto/services/fechamento.py:36–50,600–639`, `app/ponto/tests/test_feriado_um_juiz_porta.py:239–289`, `app/docs/LEIS.md:125,155`, `CLAUDE.md:458–470`)

## Escritores, leitores e invariantes

| Valor | Quem escreve e quem lê |
|---|---|
| **HE100 feriado, HE100 total e folga trabalhada** | O v2 escreve os minutos dos períodos normais. `particionar_ft` produz as três listas em memória. O fechamento soma e grava o mensal, e `lavrar` grava as linhas diárias. TXT e cartão leem o mensal passado a eles. A emissão calcula HE100 comum como HE100 total menos a parte de feriado. `horas_extras_100_feriado` agrega feriado normal e FT promovida, sem guardar a origem no mensal. No dia, `folga_feriado_min` identifica só a FT promovida. (`app/ponto/motor_calculo_v2.py:328–344`, `app/ponto/calculador/regras.py:392–452`, `app/ponto/services/fechamento.py:984–1006,1193–1199`, `app/ponto/services/dia_pago.py:173–186,235–257`, `app/folha/export.py:434–460`, `app/relatorios/cartao_pela_celula.py:90–101`) |
| **`horas_trabalhadas` e previsto** | O mensal e a lavra somam períodos normais, órfãos e FT comum. O campo exclui FT com rubrica própria. `pago_do_dia` acrescenta folga trabalhada e o detalhe da FT promovida, portanto responde a uma pergunta diferente. O export lê `horas_trabalhadas` para classificação e prontidão. O previsto do fechamento vem da grade da célula e o realizado limita o abono, sem entrar na soma do previsto. Há uma exceção preexistente na grade por turno do intermitente convocado, que copia o realizado para o previsto. (`app/ponto/services/fechamento.py:1005–1013,1075–1093`, `app/ponto/services/dia_pago.py:258–271,388–402,892–900`, `app/folha/export.py:329–338,853–858,924–926,1058–1074`, `app/escala/services/leitor_celula.py:399–423`, `app/escala/utils.py:2099–2124`) |
| **Split noturno e adicional** | O v2 escreve o split bruto e os minutos noturnos legais. A partição escreve `FolgaEmFeriado` com o split de relógio e o adicional legal da promoção. Mensal, lavra e projeção leem esses valores. O TXT emite o adicional legal em evento próprio. O split de HE100 é subconjunto da HE100. (`app/ponto/motor_calculo_v2.py:1013–1049`, `app/ponto/calculador/regras.py:380–383,432–447`, `app/ponto/services/fechamento.py:998–1004`, `app/ponto/services/dia_pago.py:244–252`, `app/ponto/calculador/alimentacao.py:369–388`, `app/folha/export.py:438–461`) |
| **Rubricas do oráculo e `lavrar(rubricas=...)`** | `alimentacao` projeta a partição e lê os valores da primeira fatia para a dobra normal. `do_dia` devolve `valores` e `nao_decide`. Desde a 10 existe a versão `oraculo`; da 11 em diante ela recebe a partição nova. A mesma `lavrar` valida donos declarados, monta a base com os períodos e substitui os campos decididos. A soma lê por versão. (`app/ponto/calculador/alimentacao.py:353–389,518–523,571–597`, `app/ponto/calculador/regras.py:787–817,954–961`, `app/ponto/services/fechamento.py:1294–1328`, `app/ponto/services/dia_pago.py:50–76,311–346,552–565`) |
| **Data, contexto da fatia e `_FERIADO_CLASSIFICADO_NO_MENSAL`** | O motor escreve o dia do turno por entrada crua e o mapa da jornada por período. A composição guarda `(motor, vínculo)` por identidade e por dia. A partição lê os dois e também escreve no mapa da jornada. Lavra, oráculo e DIFF leem esse mapa. A data civil é a resposta de contingência quando falta mapeamento. O literal `(2026, 11)` mora no export e é lido por emissão, classificação, partição, fechamento e DIFF. Competência é o rótulo da janela da empresa, vigência do vínculo é quem cobre cada data, e o corte da regra é um terceiro valor. Chamadas avulsas sem competência têm comportamento próprio. (`app/ponto/motor_calculo_v2.py:911–922,963–978,2513–2522`, `app/ponto/services/fechamento.py:223–243,335–350,377–382`, `app/ponto/calculador/regras.py:386–431,679–686`, `app/ponto/services/dia_pago.py:109–132`, `app/folha/export.py:385–394,924–926`, `app/ponto/management/commands/diff_calculador.py:112–152,203–215`, `app/ponto/janelas.py:10–21,54–63`, `app/escala/alimentacao.py:205–264`) |

**Juízes, portas e leitores.** F3 continua no motor, e o auxiliar do oráculo empresta seu método. O segundo escritor introduzido é **em memória**, no mapa `dia_da_jornada`. A borda chama o juiz de turnos para retirar batidas e depois entrega ao motor um conjunto diferente para novo pareamento. A partição é disjunta por ocorrência, e suas três listas somam a FT que chegou até ela. Isso não garante que todos os minutos tenham chegado à competência certa nem que mensal, dia e TXT usem a mesma fotografia. O cartão exige a existência do mensal gravado e então recalcula online. O PDF usa o mesmo resumo na janela de competência. O POST do TXT também calcula online, e o comparador `cartao_x_txt` usa o **mensal gravado** como referência. Já `diff_calculador` recalcula ou intercepta a apuração em memória. O DIFF entre arquivos entregues lê os conteúdos armazenados. (`app/core/juizes.py:318–320`, `app/ponto/calculador/regras.py:301–322,392–452`, `app/ponto/services/fechamento.py:339–348,1766–1792`, `app/relatorios/cartao_pela_celula.py:69–76,250–273,324–350`, `app/relatorios/pdf_espelho.py:628`, `app/folha/views.py:214–219,269–275`, `app/ponto/management/commands/diff_calculador.py:114–152`, `app/folha/haiku_export.py:79–107`)

**Relógio e travas.** O diff não adiciona leitura de relógio de parede. O corte usa `(ano, mes)` e o fechamento já captura `_agora_da_passada` uma vez. Não identifiquei inversão explícita nova. A entrega trava Empresa, e a revisão específica segue Empresa → Colaborador → Batida. O evento genérico roda em `on_commit` sem essas travas. A transação local da lavra não serializa entrega, mensal e as duas versões. (`diff.txt:1`, `app/ponto/services/fechamento.py:600–604,678–680,1829–1855`, `app/folha/views.py:102–104,132–135`, `app/folha/export.py:570–576`, `app/ponto/services/par_relampago_na_lista.py:172–187`, `app/ponto/services/dia_pago.py:358–362`)

## Contraexemplos e controles adicionais

- **20/21.** Em memória, com dois turnos `20/11 18–22h` e `21/11 03–07h` e feriado **no dia 20**, a 11 recebeu **4 h de feriado e 4 h comuns no dia 20**, e a 12 recebeu zero. Com feriado em **21/10**, a 10 recebeu as 8 h comuns e a 11 recebeu zero. A fixture de borda cobre feriado em **21/11**, quando os dois lados do filtro já atuam. Um único turno `20/11 22h → 21/11 07h` com feriado só no dia 21 ficou inteiro na 11, como hora comum. (`app/ponto/services/fechamento.py:154–172,223–230`, `app/ponto/motor_calculo_v2.py:2513–2545`, `app/ponto/tests/test_feriado_um_juiz.py:391–425`)
- **Municipal × estadual e troca no meio do mês.** Londrina e Cambé compartilham um feriado estadual do PR. Um municipal de Cambé alcança só o vínculo de Cambé. O carregador resolve a praça pelo vínculo de cada dia. Na troca **6x1 → 12x36**, a partição pode impedir a troca de contexto pelo ciclo de destino e conservar a dobra já decidida pelo 6x1 de origem. Reproduzi esse ramo com 240 min e confirmação de que a data lavrada ficou na véspera. Ocorrência na frota [nao verificado]. (`app/core/feriados.py:57–67,135–167`, `app/ponto/calculador/regras.py:410–431`, `app/ponto/tests/test_feriado_um_juiz_porta.py:470–497,595–615`)
- **Intermitente e desligado no dia 20.** Um FT intermitente por base ou ciclo fica em trabalhadas na partição. Para desligamento em 20/11 com corte 21, o colaborador ainda entra no universo de apuração da 11. A grade considera o fato lavrado no dia da demissão, e o classificador manda as verbas para a rescisão antes de avaliar o feriado emitível. Assim, 4 h no mensal podem coexistir com zero linha no TXT mensal por rescisão. O valor no módulo de rescisão está [nao verificado]. (`app/ponto/calculador/regras.py:367–377`, `app/ponto/services/fechamento.py:489–506`, `app/ponto/janelas.py:10–21`, `app/escala/services/leitor_celula.py:379–398`, `app/folha/export.py:54–60,878–893,924–926`)

## Selos e `except`

- **35 controles sem banco passaram. Os 33 mutantes sem banco mordem código de produção:** todos passaram no controle e provocaram falha de asserção no alvo mutado. Há ainda **11 mutantes de porta**, dez com alvo em produção e um de fixture que exige erro de unicidade. A execução dos 11 e os REDs contra os commits-base estão [nao verificado]. O mutante `endereco-c2` altera o registro do teste, e o controle correspondente já falha nesta cópia. (`app/ponto/tests/feriado_um_juiz_mutantes.py:42–219,273–318`, `app/core/tests/test_medida_turno_do_par.py:47–48,565–569`)
- **`except` no diff fornecido: 0 adicionados e 0 removidos, inclusive em testes.** Os `except` do comando de DIFF, da alimentação e das duas lavras já estão no contexto do diff. `assertRaisesRegex` e `finally` nos testes não são cláusulas `except`. (`diff.txt:1`, `app/ponto/management/commands/diff_calculador.py:130–140`, `app/ponto/calculador/alimentacao.py:429–455,552–560`, `app/ponto/services/fechamento.py:1267–1270,1328–1332`, `app/ponto/tests/test_feriado_um_juiz.py:20–40`)

## 27/11 16:15 · sessao 080e3483b92e · FERIADO-BOTAO-CORTADO · construtor · origem=Code

A c10 coloca as marcações no fim da área rolável dos feriados com ação oferecida no modo mês. A célula mantém os **132 px**, e o botão fica acima da região rolável. [Grade:68](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/partials/_calendario_grade.html:68) · [Grade:95](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/partials/_calendario_grade.html:95) · [Grade:132](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/partials/_calendario_grade.html:132)

**Causa provável.** Na ficha `420px 1fr`, o anexo mede uma célula de **102 × 132 px** e deixa **no máximo 70,41 px** abaixo do botão, descontados os 3 px de margem e os 6 px de padding e borda inferiores. A região da c9 podia ter ações e faixas depois de `17:00`. Chegar ao máximo (**145 de 145 px**) alinhava o fim de todo esse conteúdo, e `17:00` continuava invisível. O anexo traz o resultado agregado da sonda. A caixa de `17:00` e o ponto exato do recorte ficam **[nao verificado]**. `s10_falha.txt:4` · [Ficha:87](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/detalhe.html:87) · [Grade:138](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/partials/_calendario_grade.html:138) · [Sonda:428](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:428)

**RED e mutantes.** O RED é o smoke existente na c9. O s10 registrou **19 testes, 1 falha**, em `ultimaVisivelNoFim`. O diff de teste acrescenta duas entradas CSS e **0 `except`/`catch`**. A sonda, a asserção, os valores esperados e o teste B1 de histórico ficaram idênticos byte a byte. A execução da c10 e dos mutantes fica com o Code **[nao verificado]**. `s10_falha.txt:3–9` · [Mutantes:76](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:76) · [Smoke e B1:545](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:545) [nao verificado]

```bash
# RED na c9. Na c10, o mesmo comando exercita a cura.
alvo=core.tests.test_smoke_clique_feriado_vira_trabalho.SmokeCliqueFeriadoViraTrabalhoTest.test_clica_no_feriado_na_pagina_e_no_drawer_das_duas_cascas
PYTHONDONTWRITEBYTECODE=1 python manage.py test --settings=config.settings.ci "$alvo"

# M1 — restaura o fluxo em bloco da c9
PYTHONDONTWRITEBYTECODE=1 SMOKE_FERIADO_MUTANTE=rolagem-em-bloco python manage.py test --settings=config.settings.ci "$alvo"

# M2 — mantém o grid e devolve as marcações à ordem anterior
PYTHONDONTWRITEBYTECODE=1 SMOKE_FERIADO_MUTANTE=marcacoes-antes-dos-detalhes python manage.py test --settings=config.settings.ci "$alvo"
```

**Leitores.** A ficha e a aba Espelho do drawer carregam o calendário mensal. A fita de diagnóstico usa `layout='fita'`, `relatorios/furos.html` inclui a grade sem `layout`, e o Espelho de Ponto avulso tem grade semanal própria. A mudança de CSS vale somente para `layout='mes'` com a ação de feriado oferecida ao usuário. [Ficha:215](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/detalhe.html:215) · [Aba Espelho:134](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/core/_drawer_generico.html:134) · [Calendário:67](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/calendario_partial.html:67) · [Fita:35](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/partials/_drawer_diagnostico.html:35) · [Furos:42](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/relatorios/furos.html:42) · [Espelho avulso:97](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/ponto/espelho.html:97)

**Recorte.** O `--only` literal do s10 está **[nao verificado]**. O anexo traz a contagem, e este recorte foi reconstruído para 19 testes. `s10_falha.txt:7` [nao verificado]

```bash
--only "colaboradores.tests.test_feriado_botao_cortado colaboradores.tests.test_ui_pilulas_quebram_linha core.tests.test_selo_popover_acao core.tests.test_smoke_clique_feriado_vira_trabalho core.tests.test_smoke_retratar_1366"
```

## 27/11 16:05 · sessao 1a2ab4657839 · FERIADO-UM-JUIZ · construtor · origem=Code

A c4 está na cópia sobre `29488e3bb`. Em memória, os **9 REDs falharam na c3**, com 12 falhas de asserção contando os subcasos. Na c4, passaram **35 testes e 33 mutantes**. O diff acrescenta **0 `except`**. Os testes de porta com banco e a certificação pelo Code e pelo opus final ficam [nao verificado]. (`ponto/tests/feriado_um_juiz_mutantes.py:261`, `ponto/tests/feriado_um_juiz_mutantes.py:279`)

| Item | RED e prova na c3 | Cura e leitores |
|---|---|---|
| **A** | `ponto/tests/test_feriado_um_juiz.py::test_RED_c4_cartao_le_a_rubrica_emitida` e `::test_RED_c4_cartao_x_txt_acusa_feriado_ausente_com_he100_igual`. O TXT emitiu `400`, o cartão mostrou `0 h` e o contador devolveu `{}` ao perder a rubrica com HE100 bruta igual. (`ponto/tests/test_feriado_um_juiz.py:56`, `:72`) | Cartão, montagem do TXT e homologação leem os mesmos eventos do mensal. O PDF discrimina HE100 feriado e o contador confere a rubrica e o total. App e ficha leem o total. (`folha/export.py:388`, `:753`, `:1026`, `relatorios/cartao_pela_celula.py:89`, `:266`, `relatorios/pdf_espelho.py:773`, `api/views.py:1663`, `colaboradores/services/ficha.py:205`) |
| **B** | `ponto/tests/test_feriado_um_juiz.py::test_RED_c4_vigencia_conserva_ft_incerta_em_trabalhadas_ate_outubro`. Na c3, outubro classificou os 240 min como HE100 feriado. O teste de porta `ponto/tests/test_feriado_um_juiz_porta.py::test_RED_c4_evento_na_10_entregue_conserva_a_vigencia_e_a_11_aberta_promove` cobre mensal, duas lavras, TXT vigente e controle na 11 aberta. Execução da porta [nao verificado]. (`ponto/tests/test_feriado_um_juiz.py:300`, `ponto/tests/test_feriado_um_juiz_porta.py:239`) | Partição, lavras e DIFF usam a vigência do TXT. Até outubro, a partição segue a seleção por escala certa e a cláusula histórica da base. O recálculo genérico L-113 segue disponível nas competências entregues a partir da 11. A reemissão nessas competências está [nao verificado]. (`folha/export.py:385`, `ponto/calculador/regras.py:395`, `ponto/services/fechamento.py:989`, `:1305`, `:1829`, `ponto/management/commands/diff_calculador.py:152`, `59600163c:app/ponto/services/fechamento.py:958`) |
| **C** | `ponto/tests/test_feriado_um_juiz.py::test_RED_c4_alimentacao_le_a_dobra_normal_da_primeira_fatia` e `::test_RED_c4_periodo_normal_respeita_calendario_injetado_vazio`. Na c3, o oráculo respondeu `0`, `4` e `4 h` nos casos que pediam `4`, `0` e `0 h`. A releitura também inventou 240 min com calendário vazio. (`ponto/tests/test_feriado_um_juiz.py:216`, `:232`) | A alimentação da lavra `oraculo`, o DIFF e a releitura por período leem os minutos decididos pelo motor que apurou o turno. A prova integrada de mensal, lavras e TXT está [nao verificado]. (`ponto/calculador/alimentacao.py:519`, `:584`, `ponto/calculador/periodo.py:768`, `ponto/management/commands/diff_calculador.py:411`, `ponto/tests/test_feriado_um_juiz_porta.py:298`) |
| **D** | `ponto/tests/test_feriado_um_juiz.py::test_RED_c4_par_de_21h_a_23h_conta_a_uniao_do_noturno`. A c3 deu **1.560 min** de split. A c4 deu **1.500 min** e adicional legal de **1.500 ÷ 0,875 min**. (`ponto/tests/test_feriado_um_juiz.py:353`) | A promoção mede a união da janela com o medidor existente, desconta os intervalos e entrega split e adicional ao mensal, às duas lavras e ao DIFF. A prova integrada está [nao verificado]. (`ponto/calculador/regras.py:433`, `ponto/services/fechamento.py:998`, `ponto/services/dia_pago.py:244`, `ponto/calculador/alimentacao.py:353`, `ponto/tests/test_feriado_um_juiz_porta.py:572`) |
| **E** | `ponto/tests/test_feriado_um_juiz.py::test_RED_c4_data_e_certeza_seguem_o_turno_de_cada_par` e `::test_RED_c4_turnos_distintos_na_borda_20_21_entram_cada_um_na_sua_competencia`. A c3 pôs ambas as parcelas na véspera. Na borda, deixou os dois pares na 11 e nenhum na 12. (`ponto/tests/test_feriado_um_juiz.py:367`, `:391`) | A partição resolve vínculo, certeza, cláusula e data da lavra pelo turno. Entre competências já na vigência, a entrada do motor usa o pareador existente para retirar da janela ampliada o turno fechado e distinto que pertence à competência vizinha. As duas lavras e o DIFF leem o resultado. Os casos de porta com troca de praça estão [nao verificado]. (`ponto/calculador/regras.py:412`, `ponto/services/fechamento.py:154`, `:226`, `ponto/services/dia_pago.py:244`, `ponto/calculador/alimentacao.py:361`, `ponto/tests/test_feriado_um_juiz_porta.py:455`) |
| **F** | `ponto/tests/test_feriado_um_juiz_porta.py::test_RED_c4_competencia_sem_feriado_conserva_arredondamento_da_base`. Executadas as atribuições dos dois hashes com 241 + 241 min, a base deu **8,04 h** e a c3 **8,03 h**. `ponto/tests/test_feriado_um_juiz.py::test_RED_c4_delete_e_bulk_create_usam_a_mesma_transacao` mostrou a perda da linha anterior na c3. (`59600163c:app/ponto/services/fechamento.py:922`, `:1008`, `29488e3bb:app/ponto/services/fechamento.py:964`, `ponto/tests/test_feriado_um_juiz.py:13`) | Sem promoção, o mensal volta a **8,04 h**. Com promoção positiva, arredonda uma vez. O RED existente conserva as asserções e agora injeta uma promoção fracionária na fixture. As duas versões de `lavrar` executam `delete` e `bulk_create` na mesma transação. Rollback no banco [nao verificado]. (`ponto/services/fechamento.py:1008`, `ponto/tests/test_feriado_um_juiz_porta.py:396`, `:435`, `ponto/services/dia_pago.py:360`) |
| **G** | `ponto/tests/test_feriado_um_juiz_porta.py::test_RED_c4_l097_certeza_so_decide_rubrica_com_ou_sem_autorizacao` está com o `skip` literal. A chamada isolada ao motor deu **240/0 min** para incerta/certa sem autorização e **240/240 min** com autorização. (`ponto/tests/test_feriado_um_juiz_porta.py:499`, `ponto/motor_calculo_v2.py:1562`, `:1585`) | G fica pendente da decisão de lei. K1 conserva o `expectedFailure` e as asserções da c3. (`ponto/tests/test_feriado_um_juiz_porta.py:381`, `:499`) |

**Comandos a partir de `app/`**. `base_c3` carrega os fontes do hash em memória e exige falhas de asserção. Os cinco comandos com `--settings` dependem da pista com banco e ficam [nao verificado]. (`ponto/tests/feriado_um_juiz_mutantes.py:43`, `:169`, `:279`)

```bash
m() { PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.feriado_um_juiz_mutantes "$@"; }

m base_c3
m controle
m c4_cartao_rejulga_ciclo
m c4_contador_ignora_feriado
m c4_ignora_vigencia
m c4_alimentacao_rejulga_dobra
m c4_periodo_rejulga_dobra
m c4_split_bruto_do_noturno_longo
m c4_adicional_bruto_do_noturno_longo
m c4_lavra_na_data_herdada
m c4_certeza_no_dia_herdado
m c4_pad_conserva_turnos_externos
m c4_lavra_sem_atomic

m c4_fechamento_ignora_competencia --settings=config.settings.ci
m c4_mensal_le_noturno_bruto --settings=config.settings.ci
m c4_lavra_le_noturno_bruto --settings=config.settings.ci
m c4_troca_de_praca_le_vinculo_herdado --settings=config.settings.ci
m c4_arredonda_fora_da_promocao --settings=config.settings.ci
```

**Recorte para o Code** (`bin/suite.sh:295`)

```bash
bash ../bin/suite.sh --dir /home/ronald/wt-codex-feriado-um-juiz-152427 \
  --only "ponto folha relatorios escala core colaboradores api"
```

Medição dos 75 dias e 42 colaboradores, DIFF do TXT 11 de frota, reversão e apply L-092 [nao verificado].

## 27/11 16:04 · sessao ab2be411f222 · FERIADO-BOTAO-CORTADO · construtor · origem=Code

**B1 — Histórico HTMX.** O componente instala os listeners uma vez por documento e devolve o painel ao `details` antes do snapshot. O RED `core/tests/test_smoke_clique_feriado_vira_trabalho.py::test_historico_htmx_do_feriado_e_de_inverter_retratar` exercita Feriado e Inverter/Retratar, conta as inscrições após a segunda execução do script e verifica os snapshots e o DOM ao Voltar e Avançar. [popover-acao.js:28](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/static/js/popover-acao.js:28) [popover-acao.js:177](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/static/js/popover-acao.js:177) [teste:546](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:546)

**Prova antes da cura.** Li o blob de `c44771b4a` com `git show` e executei o JS duas vezes no Duktape, com DOM simulado. O base registrou **2 inscrições por evento e 1 órfão** no ponto do snapshot. A cura registrou **1 inscrição por evento e 0 órfãos**. Retirar a guarda voltou a duplicar as inscrições. Retirar o fechamento voltou a deixar o órfão. A reprodução em Chromium fica [nao verificado]. [popover-acao.js:160](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/static/js/popover-acao.js:160) [mutantes:77](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:77)

**B2 — Ficha real a 1366 px.** `core/tests/test_smoke_clique_feriado_vira_trabalho.py::test_clica_no_feriado_na_pagina_e_no_drawer_das_duas_cascas` agora abre a rota da ficha, exige `colaboradores/detalhe.html` e espera o swap HTMX. A sonda verifica a coluna de 420 px e o `overflow:hidden` antes de medir viewport, recorte, ancoragem e submit. O MORDE `core/tests/test_smoke_clique_feriado_vira_trabalho.py::test_MORDE_painel_recortado_na_ficha_real` injeta recorte e exige que a sonda o acuse mesmo com o painel dentro da viewport. A leitura do hash-base confirmou a lacuna do cenário sintético. A mordida e a geometria da ficha real ficam [nao verificado], portanto a cura geométrica condicional segue em aberto. [ficha:87](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/detalhe.html:87) [ficha:215](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/detalhe.html:215) [sonda:235](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:235) [MORDE:549](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:549) [rota:676](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:676)

A tentativa do RED por `manage.py` parou no import de `django.core`, antes de iniciar a suíte. O clique na tela de produção e a certificação pelo Code e pelo opus ficam [nao verificado]. `360 px` e `base_app` ficam na lista do pouso conforme o aval fornecido. [manage.py:11](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/manage.py:11) [cenários:641](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:641) [nao verificado]

**Inclusões e diff.** `base.html` e `base_app.html` são as duas inclusões diretas. Os consumidores são Feriado, Inverter/Retratar e Resolver dia. O diff tem **0 `except` e 0 `catch` adicionados ou removidos**. [base.html:440](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/base.html:440) [base_app.html:194](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/base_app.html:194) [grade:96](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/partials/_calendario_grade.html:96) [grade:133](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/partials/_calendario_grade.html:133) [grade:138](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/colaboradores/partials/_calendario_grade.html:138)

**Comandos dos mutantes para o Code**, a partir de `app/`, no ambiente de teste. Cada invocação abaixo deve falhar. [mutantes:72](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:72) [seleção:543](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:543) [nao verificado]

```bash
export PYTHONDONTWRITEBYTECODE=1
smoke=core.tests.test_smoke_clique_feriado_vira_trabalho.SmokeCliqueFeriadoViraTrabalhoTest
b1="$smoke.test_historico_htmx_do_feriado_e_de_inverter_retratar"
b2="$smoke.test_clica_no_feriado_na_pagina_e_no_drawer_das_duas_cascas"

SMOKE_FERIADO_MUTANTE=historico-sem-guarda python3 manage.py test --settings=config.settings.ci "$b1"
SMOKE_FERIADO_MUTANTE=historico-sem-fechar-painel python3 manage.py test --settings=config.settings.ci "$b1"
SMOKE_FERIADO_MUTANTE=pagina-sem-ficha python3 manage.py test --settings=config.settings.ci "$b2"
SMOKE_FERIADO_MUTANTE=painel-recortado python3 manage.py test --settings=config.settings.ci "$b2"
```

**Recorte `--only` para o Code.** [suite.sh:26](/home/ronald/wt-codex-feriado-botao-cortado-112417/bin/suite.sh:26) [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 ../bin/suite.sh \
  --dir /home/ronald/wt-codex-feriado-botao-cortado-112417 \
  --only "core.tests.test_smoke_clique_feriado_vira_trabalho core.tests.test_selo_popover_acao core.tests.test_smoke_retratar_1366"
```

_8742 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
