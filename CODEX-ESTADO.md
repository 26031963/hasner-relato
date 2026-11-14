# CODEX-ESTADO

> Ultima resposta processada: **14/11/2026 17:38** · publicado em **14/11/2026 17:42** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 14/11 17:38 · sessao 190a94933cb1 · - · - · origem=Ronald (tmux)

Não encontrei defeito concreto que exija ajuste na fixture atual; execução ORM continua [nao verificado].

- O isolamento intercepta o nome importado dinamicamente pelo cartório e permanece ativo durante o `post_save` do órfão (`chamados/tests/test_hx_disputa_muda.py:28`, `ponto/services/cartorio.py:1033`, `:1045`, `chamados/signals.py:190-218`).
- O piso impede que o julgamento de 20/05 espelhe marcos ou abra cobranças pela emissão automática (`ponto/services/cartorio.py:298`, `:340-354`, `:1727`, `:1813`). A entrada real mantém a premissa acusadora; a guarda de nascimento só absolve diante de veredito conhecido fora de `VEREDITOS_ACUSAM` (`chamados/reconciliador.py:1027-1031`).
- O preparo exige resposta real `lido`; o novo caso exige `sem_celula` e origem ausente antes do repasse, depois verifica duas passadas abertas, sem pergunta, com luz e sem push (`chamados/tests/test_hx_disputa_muda.py:34-35`, `:80-94`).

Precisão para a entrega: esses dois casos não variam apenas a ata. O preparo com ata também cria batida-origem; o caso sem ata mantém origem ausente (`chamados/tests/test_hx_disputa_muda.py:29-31`, `:81`). Assim exercitam os dois ramos silenciosos distintos (`chamados/services/disputa_emissao.py:1420-1429`, `:1459-1471`).

## 14/11 17:31 · sessao 190a94933cb1 · - · - · origem=Ronald (tmux)

Censo por leitura da cópia; execução ORM e efeitos reais de signals: [nao verificado].

| Variável | Escrita | Leitura |
|---|---|---|
| `_absteve_por_ata` | Inicializada em `chamados/services/disputa_emissao.py:1289`; marcada nos complementos `:1304`, caminho de ata `:1392`, batida com classificador silencioso `:1428`, batida classificada `:1438` e sem origem `:1467`. | Veto de auto-fecho em `:1540`; acende em `:1542`, preservando a disputa aberta. |
| `complementadas` | Criada em `:1277`; única inclusão em `:1310`, seguida de atualização de motivos em `:1311`. Exige pergunta anterior `:1295`, ata disponível `:1302` e complemento admitido `:1307`. | Percorrida em `notificar_complementos`, `:1280`; push em `:1283`. Chamadas em `:1320`, `:1527`, `:1531`; esta última exclui a principal pela condição `:1281`. | [nao verificado]
| `motivos_atualizados` | Inicializada pelos motivos da principal em `:1327-1328`; recebe resultados de pergunta/complemento em `:1373`, `:1394`, `:1440`, `:1446`, `:1472`, `:1477`; seed `orfao_14h` em `:1486`. | Criação da disputa `:1354`; agregação `:1360`, `:1380`, `:1487`; insumo das mesmas portas de pergunta/complemento; entradas pendentes `:1501`; condição do push `:1550` e argumentos em `:1552`. Persistência dos motivos pertence a `_agregar_motivos_na_dona`, `:587`, `:601`, `:607`. | [nao verificado]

A fixture HX **não entra em `complementadas`**: o preparo cria disputa vazia com `motivos=['orfao_14h']`, sem perguntas (`chamados/tests/test_b2d_resposta_visivel.py:44-50`); a inclusão no dicionário exige pergunta anterior e complemento admitido (`chamados/services/disputa_emissao.py:1295`, `:1307`, `:1310`). O push `nova=False` do anexo corresponde ao push principal em `:1552`, após selecionar a disputa aberta existente em `:1323-1325` e copiar seus motivos em `:1327-1328`.

Também não há batida-origem criada pelo preparo (`chamados/tests/test_b2d_resposta_visivel.py:29-50`). Sem candidatas, `_achar_batida_origem` devolve `None` (`chamados/services/materializacao.py:140`); portanto a reordenação da c22 que alcança esse cenário é a consulta à ata antes do classificador no ramo sem origem (`chamados/services/disputa_emissao.py:1465-1471`). O ramo silencioso de batida existe separadamente em `:1420-1429`.

Os seis chamadores produtivos encontrados são:

- `ponto/management/commands/processar_alertas_turno.py:385`.
- `ponto/management/commands/processar_alertas_avancados.py:159`.
- `ponto/management/commands/detectar_ausencias.py:803`.
- `chamados/services/acoes_disputa.py:222`.
- `chamados/services/cobrar_dia.py:49`.
- `chamados/services/pergunta_no_app.py:246`, cujo parâmetro padrão é `notificar=False` em `:231`.

Os quatro primeiros importam pela reexportação de `chamados/models.py:4151`; os dois últimos importam diretamente do emissor (`cobrar_dia.py:20`, `pergunta_no_app.py:238`).

Adicionar existência de pergunta apenas à condição do push principal suprime, para esses chamadores, a notificação da principal aberta e vazia que passa pela condição atual (`chamados/services/disputa_emissao.py:1550-1552`). O retorno e a decisão de auto-fecho ficam nas linhas anteriores (`:1539-1549`); complementos continuam pelo caminho próprio (`:1279-1283`).

Há dois contratos de teste anteriores incompatíveis com essa supressão: ambos exigem push após afirmar zero perguntas sem ata (`chamados/tests/test_bo_disputa_volta5b_orfaos.py:191-199`, `chamados/tests/test_bo_disputa_volta5b_orfaos_memoria.py:187-193`). A resposta textual administrativa também afirma “Colaborador notificado” incondicionalmente após retorno de disputa (`chamados/services/acoes_disputa.py:231-232`); o efeito real dessa mensagem na execução corrigida é [nao verificado].

## 14/11 17:19 · sessao 7db8be5ab487 · JUSTIFICATIVA-PAGA · verificador · origem=Code

**Há curas efetivas, mas a classe ainda reprova: restam abono acima do previsto, divergência motor × gravado, prontidão paralela, `dias_abono` com significados diferentes e selo que aceita alimentação vazia indevida.** (`app/ponto/services/dia_pago.py:399`; `app/ponto/motor_calculo_v2.py:2611`; `app/folha/export.py:1100`; `app/ponto/services/dia_pago.py:701`; `app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:84`)

**1. Achados da v1.r: situação no hash**

| Achado da v1 | Resultado |
|---|---|
| Parcial não chegava ao gravado | **Curado para justificativa parcial em dia `trabalho`**: a grade transporta, `por_dia_da_grade` lê, fechamento soma e entrega o mesmo mapa ao DiaPago. A igualdade integral entre motor e gravado ainda falha com ABONA e trabalho, descrita em A2. (`app/escala/services/leitor_celula.py:436`; `app/ponto/services/dia_pago.py:398`; `app/ponto/services/fechamento.py:1067`; `app/ponto/services/fechamento.py:1217`) |
| Chamadores de sombra sem mapa | **Curados os três apontados**: diff espelho, extrato e esmeril recebem a fonte. A/B/C do diff continuam explicitamente sem crédito; D recebe a fonte completa. (`app/ponto/management/commands/diff_espelho_alimentacao.py:109`; `app/ponto/management/commands/diff_espelho_alimentacao.py:124`; `app/relatorios/management/commands/diff_extrato_parcial.py:62`; `app/ponto/services/esmeril_espelho.py:166`) |
| Segunda pausa perdida | **Curada para pausas regularmente ordenadas**, pela cadeia compartilhada e desconto de todos os pares. A classe semântica completa permanece aberta: pausas sobrepostas produzem virada indevida e abono excessivo, A1. (`app/ponto/services/efeito_ausencia.py:176`; `app/ponto/turnos.py:2792`; `app/escala/utils.py:346`) |
| Duas âncoras somavam a mesma cauda | **Curado por classe**: reúne intervalos por dia e mede sua união, incluindo sobreposição e trechos disjuntos. (`app/ponto/services/efeito_ausencia.py:137`; `app/ponto/services/efeito_ausencia.py:140`; `app/ponto/services/efeito_ausencia.py:150`) |
| ABONA inteira concorria com mapa parcial | **Curado o mapa**, retirando o parcial. **Não curada toda a conta**: com trabalho no dia, o motor não paga o restante inteiro, enquanto a grade paga. (`app/ponto/services/efeito_ausencia.py:63`; `app/ponto/motor_calculo_v2.py:2611`; `app/ponto/services/dia_pago.py:397`) |
| Cobertura vazia após cruzamento | **Permanece**: o filtro posterior ao juiz pode remover o marco; o efeito exclui esse dia das justificadas. Vazio, sozinho, continua não demonstrando falta inteira. (`app/ponto/services/efeito_ausencia.py:205`; `app/ponto/turnos.py:2764`; `app/ponto/services/efeito_ausencia.py:46`; `app/ponto/motor_calculo_v2.py:2605`) |
| Falha nova de relógio | **Não demonstrada** no caminho novo: cadeia usa data explícita, batidas e saída informada usam timestamps explícitos. (`app/escala/utils.py:345`; `app/ponto/services/efeito_ausencia.py:198`; `app/ponto/services/efeito_ausencia.py:202`) |
| Testes sem percurso até o gravado | **Melhorou, mas não fecha**: há teste ORM dos escritores reais, não executado nesta verificação; os isolados executam funções e blocos selecionados. (`app/ponto/tests/test_justificativa_paga_o_periodo.py:367`; `app/ponto/tests/test_justificativa_paga_o_periodo.py:398`; `app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:294`; execução ORM [nao verificado]) |
| Derivação paralela e campo com dois significados | **Parcialmente curado**: saiu a montagem manual de quatro marcos; continuam o fallback de prontidão e a divergência de `dias_abono`. (`app/ponto/services/efeito_ausencia.py:177`; `app/folha/export.py:1100`; `app/ponto/services/dia_pago.py:701`; `app/ponto/motor_calculo_v2.py:2630`) |
| Selo insuficiente | **Permanece**, embora agora detecte remoção da atribuição: atribuir `{}` indevidamente ainda passa, A5. (`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:84`; `app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:90`) |

**2. Fonte única: censo de produção**

A busca literal encontrou os seguintes escritores, transportadores e leitores; os testes são examinados no item 8. (`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:57`)

| Campo/função | Escrita/composição | Leitura/transporte |
|---|---|---|
| `periodos_justificados` | Consulta e agregação em `app/ponto/services/efeito_ausencia.py:99`, `app/ponto/services/efeito_ausencia.py:127`, `app/ponto/services/efeito_ausencia.py:140` | Efeito em `app/ponto/services/efeito_ausencia.py:45`; grade em `app/escala/utils.py:1198`, `app/escala/utils.py:1251`, `app/escala/utils.py:1257`, `app/escala/utils.py:1276` |
| `intervalos_do_periodo_justificado` | Trechos e subtração de pausas em `app/ponto/turnos.py:2783`, `app/ponto/turnos.py:2792`, `app/ponto/turnos.py:2805` | Medidor em `app/ponto/turnos.py:2780`; fonte em `app/ponto/services/efeito_ausencia.py:129`, `app/ponto/services/efeito_ausencia.py:206` |
| Mapa filtrado `min_justificados` | Cria em `app/ponto/services/efeito_ausencia.py:47`; remove ABONA em `app/ponto/services/efeito_ausencia.py:63`; devolve em `app/ponto/services/efeito_ausencia.py:68` | Fechamento em `app/ponto/services/fechamento.py:667`; grade em `app/escala/services/leitor_celula.py:365`; demais alimentadores abaixo |
| `min_justificados` na linha | `app/escala/utils.py:1276`; `app/escala/services/leitor_celula.py:436` | `app/ponto/services/dia_pago.py:398`, `app/ponto/services/dia_pago.py:399` |
| `minutos_justificados` no motor | Inicializa em `app/ponto/motor_calculo_v2.py:813`; escreve em `app/ponto/services/fechamento.py:755`, `app/ponto/services/fechamento.py:831`, `app/ponto/services/espelho.py:1134`, `app/relatorios/pdf_espelho.py:407`, `app/ponto/management/commands/diff_espelho_alimentacao.py:56`, `app/relatorios/management/commands/diff_extrato_parcial.py:62`, `app/ponto/services/esmeril_espelho.py:166` | Conta em `app/ponto/motor_calculo_v2.py:2628`, `app/ponto/motor_calculo_v2.py:2634`; contrato nominal em `app/ponto/calculador/nucleo.py:424` |
| `dias_com_abono` | Deriva datas pela grade e conta DiaPago em `app/ponto/services/dia_pago.py:700`, `app/ponto/services/dia_pago.py:701`, `app/ponto/services/dia_pago.py:702` | Cartão em `app/relatorios/cartao_pela_celula.py:188` |
| `minutos_abonados` materializado | Fechamento em `app/ponto/services/fechamento.py:1067`; lavratura em `app/ponto/services/dia_pago.py:288`, `app/ponto/services/dia_pago.py:292`; retrato em `app/ponto/services/dia_pago.py:467`, `app/ponto/services/dia_pago.py:479`, `app/ponto/services/dia_pago.py:484` | Cartão em `app/relatorios/cartao_pela_celula.py:118`, `app/relatorios/cartao_pela_celula.py:186`; prontidão em `app/folha/export.py:1078` |

Motor, fechamento, espelho, PDF, esmeril e os três caminhos laterais identificados recebem os minutos da fonte; a derivação paralela comprovada de prontidão continua fora dessa cadeia. (`app/ponto/services/fechamento.py:667`; `app/ponto/services/espelho.py:1134`; `app/relatorios/pdf_espelho.py:407`; `app/ponto/services/esmeril_espelho.py:166`; `app/folha/export.py:1100`)

**A3 — prontidão diverge conforme o ramo.** Com snapshot, soma realizado e `minutos_abonados`. Sem snapshot, soma como abono somente o previsto de `ausencia`, ignorando justificativa parcial em `trabalho`. No exemplo sintético previsto 660, realizado zero, parcial 630: snapshot **95%**, fallback **0%**. O fechamento grava 630. (`app/folha/export.py:1078`; `app/folha/export.py:1079`; `app/folha/export.py:1100`; `app/folha/export.py:1111`; `app/ponto/services/fechamento.py:1067`)

**8932 responde outra grandeza:** tipos do catálogo e datas de registros `Ausencia`; não lê minutos justificados nem lavrados. Uma justificativa parcial isolada não entra nessa contagem, embora gere minutos no fechamento. Isso é diferença de alcance e unidade, não prova de desconto no pagamento. Emissão efetiva da 8932 por essa função: **[nao verificado]**. (`app/folha/export.py:193`; `app/folha/export.py:198`; `app/folha/export.py:204`; `app/folha/export.py:160`; `app/ponto/services/dia_pago.py:399`)

**3. Semântica**

Não há significado uniforme em todos os campos e leitores: a fonte usa dia do turno; DSR Comercial usa data civil; o contador de dias usa classificação atual da grade; o motor conta dias justificados sem trabalho. (`app/ponto/services/efeito_ausencia.py:109`; `app/ponto/motor_calculo_v2.py:3477`; `app/ponto/services/dia_pago.py:701`; `app/ponto/motor_calculo_v2.py:2611`)

- **Dia e fuso:** a fonte usa `data_turno`, rejeita âncora atribuída a outro dia e encadeia marcos locais através da meia-noite. Converte timestamps por `localtime`; não encontrei nessa cadeia comparação intencional de hora local ingênua com UTC. (`app/ponto/services/efeito_ausencia.py:184`; `app/escala/utils.py:345`; `app/escala/utils.py:347`; `app/ponto/services/efeito_ausencia.py:198`)
- **Unidade:** fonte e lavratura usam minutos; motor e resumo convertem por `/60`. (`app/ponto/services/efeito_ausencia.py:154`; `app/ponto/services/dia_pago.py:288`; `app/ponto/motor_calculo_v2.py:2634`; `app/relatorios/cartao_pela_celula.py:118`)
- **Bordas:** seleção de datas inclui ambas; duração é diferença entre instantes, sem minuto adicional; contato entre extremidades não configura cruzamento. A classificação de `Ausencia` parcial, distintamente, inclui ambas as extremidades. (`app/ponto/services/efeito_ausencia.py:101`; `app/ponto/services/efeito_ausencia.py:154`; `app/ponto/turnos.py:2764`; `app/ponto/turnos.py:2657`)
- **Pausas:** a justificativa desconta todos os pares transportados pela cadeia; isso não garante coerência com o previsto para pausas sobrepostas ou fora de ordem, A1. (`app/ponto/services/efeito_ausencia.py:177`; `app/ponto/turnos.py:2792`; `app/escala/utils.py:2324`)

**22–06 com “justificativa 23–02”:** `Justificativa` não representa um intervalo arbitrário com início/fim; representa tipo, âncora e eventualmente `saida_informada`. Com `esqueci_saida`, âncora E23 e saída informada D+1 02, paga a cauda **02–06 = 240 minutos**, sem pausas. Sem saída informada, paga **23–06 = 420**. O intervalo declarado 23–02 pertence ao modelo `Ausencia`, cujo classificador desloca o fim para o dia seguinte. (`app/ponto/models.py:144`; `app/ponto/models.py:149`; `app/ponto/turnos.py:2738`; `app/ponto/services/efeito_ausencia.py:202`; `app/ponto/models.py:768`; `app/ponto/turnos.py:2654`)

**07–19, pausa 10–11, E07 e saída informada 07:30:** paga **630 minutos**. Se houver uma batida S07:30 fechando a ponta, conserva inteiro em vez de aplicar esse parcial. (`app/ponto/services/efeito_ausencia.py:202`; `app/ponto/turnos.py:2792`; `app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:172`; `app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:321`)

**4. Conta, teto, duplicidade, falta e DSR**

**A1 — parcial pode superar o previsto.** Reprodução sintética em memória, usando fonte e função de previsto reais: jornada 07–19, pausas **10–12 e 11–13**, E07 ancorada:

| Resultado | Minutos |
|---|---:|
| Previsto real | 480 |
| Fonte de justificativa | 1.920 |
| `por_dia_da_grade` | 1.920 |

A sequência mantém a ordem cadastrada; como 11h sucede 12h na lista, a cadeia empurra a segunda pausa para D+1 e também desloca o fim do turno. O previsto desconta durações sem essa cadeia. Não há teto final no parcial. O cadastro transporta o segundo par; o validador mostrado examina somente o primeiro. Ocorrência produtiva **[nao verificado]**. (`app/escala/utils.py:77`; `app/escala/utils.py:346`; `app/escala/utils.py:2328`; `app/ponto/services/efeito_ausencia.py:154`; `app/ponto/services/dia_pago.py:399`; `app/escala/services/cadastro_tipo.py:232`; `app/escala/models.py:656`)

**A2 — ABONA inteira com trabalho continua divergindo.** A retirada do parcial evita somar inteiro e parcial no mapa. Entretanto, com previsto 660 e realizado 180, o gravado recebe **480**, enquanto o motor recebe **zero** de abono: o dia trabalhado não integra `_abono`, e o mapa parcial foi esvaziado. Reproduzido executando os blocos reais em memória. (`app/ponto/services/efeito_ausencia.py:63`; `app/ponto/motor_calculo_v2.py:2611`; `app/ponto/motor_calculo_v2.py:2629`; `app/ponto/services/dia_pago.py:397`)

Duas justificativas sobrepostas agora pagam a união. Duas **Ausencia parciais**, porém, continuam tendo seus `a.minutos` somados sem união no efeito e no crédito diário; o PDF acrescenta a soma parcial ao abono do motor. É resíduo preexistente, não curado pela união de justificativas. (`app/ponto/services/efeito_ausencia.py:140`; `app/ponto/services/efeito_ausencia.py:52`; `app/ponto/services/credito_parcial.py:28`; `app/relatorios/pdf_espelho.py:480`)

**Falta inteira:** cobertura vazia retira a justificativa do conjunto, mas o Base também considera períodos abertos como trabalho; portanto, não se pode concluir “vazio ⇒ falta inteira”. A falta exige ausência simultânea de trabalho e justificativa. (`app/ponto/services/efeito_ausencia.py:46`; `app/ponto/motor_calculo_v2.py:2499`; `app/ponto/motor_calculo_v2.py:2583`; `app/ponto/motor_calculo_v2.py:2605`)

**DSR Comercial:** continua vulnerável à alimentação que perde a justificativa: refaz trabalho pela data civil, depois cobra dias obrigatórios ausentes de trabalho e justificadas. Se a fonte esvaziar a cobertura de D e a entrada civil estiver em D+1, esse predicado pode perder a semana mesmo havendo jornada atribuída a D no Base. Reprodução integrada desse cenário pelo pareador e ocorrência produtiva **[nao verificado]**. (`app/ponto/services/efeito_ausencia.py:205`; `app/ponto/motor_calculo_v2.py:2583`; `app/ponto/motor_calculo_v2.py:3477`; `app/ponto/motor_calculo_v2.py:3490`)

**A condição literal 0 falta inteira / 0 DSR adicional no universo 1.891 não está demonstrada pelos dados fornecidos: [nao verificado].**

**5. Âncora inválida conserva?**

**Sim, no predicado compartilhado**, para sem âncora, papel errado e ponta fechada: retorna motivo; fonte converte para `cobre=None`; efeito mantém o dia justificado sem crédito parcial. A grade também lê `None` como justificativa inteira. Não há tratamento por nome nesses ramos. (`app/ponto/turnos.py:2694`; `app/ponto/turnos.py:2700`; `app/ponto/turnos.py:2703`; `app/ponto/services/efeito_ausencia.py:131`; `app/ponto/services/efeito_ausencia.py:46`; `app/escala/utils.py:1251`)

**“Em todos os leitores” não está provado**: a garantia do predicado não elimina A2, A3 e A4. Reprodução individual dos três dias exigidos pelo aval e lista administrativa por motivo: **[nao verificado]**.

**6. Motor tocado?**

O **delta fornecido não altera** `motor_calculo_v2.py`. No hash existem a inicialização do memo e o bloco que substitui abono inteiro por minutos da fonte nos dias obrigatórios justificados. (`app/ponto/motor_calculo_v2.py:813`; `app/ponto/motor_calculo_v2.py:2628`; `app/ponto/motor_calculo_v2.py:2634`)

O anexo v1.r já identifica esses trechos como mudança da obra. O confronto independente de **todo** `14b61c124..29460c1d1` ficou **[nao verificado]**: a cópia não contém histórico Git; inclusive o carregamento histórico do teste depende de `git show`. (`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:28`)

**7. Idempotência e escritor**

**Estado integral não é idêntico:** fechamento renova carimbos e DiaPago é apagado/recriado, renovando identidade e `lavrado_em`. Igualdade dos valores financeiros em duas execuções completas, com entradas fixas: **[nao verificado]**; o teste novo executa o fechamento uma vez. (`app/ponto/services/fechamento.py:1091`; `app/ponto/services/fechamento.py:1148`; `app/ponto/services/dia_pago.py:359`; `app/ponto/models.py:380`; `app/ponto/tests/test_justificativa_paga_o_periodo.py:398`)

Não encontrei escritor novo de FechamentoMensal fora da porta no delta; o censo existente também não atribuiu escrita externa. A porta declarada permanece `ponto/services/fechamento.py`; escritas que o censo não consegue atribuir continuam limitação da análise. (`app/folha/tests/test_chokepoint_folha_gate.py:58`; `app/folha/tests/test_chokepoint_folha_gate.py:72`; `app/core/censo_escritas.py:43`)

**8. Testes e mutantes** [nao verificado]

Na execução local sem escrita: **15/15 testes passam; 14/14 mutantes declarados morrem por asserção**. Há mordida além do golden: união sobreposta/disjunta, duas pausas, duas âncoras, precedência e transporte à grade. (`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:186`; `app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:192`; `app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:201`; `app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:258`; `app/ponto/tests/mutantes_justificativa_periodo_v2.py:78`)

**A5 — mutante plausível sobrevivente comprovado:** trocar a atribuição do esmeril para `_m.minutos_justificados = {}`. Os 15 continuam verdes: o selo verifica que há uma escrita antes do uso, sem conferir sua origem ou valor. O mutante declarado remove a escrita, cobrindo uma classe menor. (`app/ponto/services/esmeril_espelho.py:166`; `app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:84`; `app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:90`; `app/ponto/tests/mutantes_justificativa_periodo_v2.py:42`)

Há testes de alcance limitado, que não devem ser apresentados como prova financeira integral: [nao verificado]

- O teste 06 usa motor falso que devolve o próprio atributo: prova argumento obrigatório e transporte, não cálculo. (`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:226`; `app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:235`)
- O teste 07 usa contador falso baseado em `data__in`: não prova os filtros ORM reais. (`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:243`)
- O teste 09 extrai cinco instruções e fornece `_abono`, obrigatórios e previsto artificialmente: não testa a seleção que produz A2 nem DSR. (`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:294`; `app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:297`; `app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:300`)

Esses testes têm mordida localizada; não são todos vacuamente verdes. A suíte, porém, não certifica a classe completa. ORM e RED histórico nesta cópia: **[nao verificado]**. (`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:6`; `app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:28`)

**9. Classes que já reprovaram obra**

| Classe | Resposta |
|---|---|
| Segundo juiz duplicando juiz registrado | **Não demonstrado especificamente.** Há decisão adicional de cobertura depois do juiz, pelo filtro de cruzamento; isso continua explícito. (`app/ponto/services/efeito_ausencia.py:195`; `app/ponto/services/efeito_ausencia.py:205`; `app/ponto/turnos.py:2751`) |
| Segundo escritor fora da porta | **Não encontrado no delta/censo atribuível.** (`app/folha/tests/test_chokepoint_folha_gate.py:58`; `app/folha/tests/test_chokepoint_folha_gate.py:72`) |
| Relógio lido fora da borda | **Não demonstrado como novidade desta volta.** A cadeia nova recebe instantes explícitos; fechamento mantém seu relógio da passada. (`app/ponto/services/efeito_ausencia.py:202`; `app/ponto/services/fechamento.py:616`) |
| Derivação paralela de fonte única | **Sim, A3**, fallback de prontidão. (`app/folha/export.py:1100`) |
| Selo/RED sem mordida suficiente | **Sim, A5**, atribuição vazia indevida sobrevive. RED histórico independente **[nao verificado]**. (`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:84`; `app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:28`) |
| Ordem de trava | **Não demonstrada falha nova.** O delta não acrescenta travas; concorrência completa **[nao verificado]**. (`app/ponto/services/fechamento.py:1149`; `app/ponto/services/fechamento.py:1165`) |
| Pouso antes do pré-requisito | **[nao verificado]** quanto a deploy/apply. O código permite recalcular 10 exportada; a guarda específica só lista 09. (`app/ponto/services/fechamento.py:36`; `app/ponto/services/fechamento.py:539`; `app/ponto

[truncado em 20000 de 21630 caracteres]

## 14/11 17:17 · sessao cc7cf9417762 · - · - · origem=Ronald (tmux)

1. **Teto ainda falha como classe — contracaso confirmado em memória.** Jornada 07–19, pausas 10–12 e 11–13, E07 ancorada: `minutos_previstos_do_dia` real devolve **480**; fonte real devolve **1.920**; `por_dia_da_grade` real entrega **1.920** de abono. A sequência conserva a ordem dos pares; ao encontrar 11h depois de 12h, empurra a segunda pausa e o fim para D+1 (`app/escala/utils.py:76`; `:345`; `:347`). O previsto desconta os dois comprimentos de pausa, sem encadear (`app/escala/utils.py:2291`; `:2324`; `:2332`). A fonte e os leitores não limitam o parcial ao previsto (`app/ponto/services/efeito_ausencia.py:154`; `app/ponto/services/dia_pago.py:398`; `app/ponto/motor_calculo_v2.py:2633`). A porta atribui o segundo par, enquanto o validador de intervalo dentro do turno só examina o primeiro (`app/escala/services/cadastro_tipo.py:232`; `app/escala/models.py:643`; `:656`). Ocorrência produtiva: [nao verificado].

2. **As duas pausas normais e a união de justificativas foram curadas por classe.** Fonte usa `pausas_do_dia` e `cadeia_do_dia`; medidor percorre todos os pares; união elimina sobreposições antes de somar (`app/ponto/services/efeito_ausencia.py:176`; `:177`; `:137`; `:144`; `:150`; `app/ponto/turnos.py:2792`).

3. **`dias_abono` continua com dois significados.** Novo contador anuncia “dias INTEIROS”, mas seleciona todo `tipo_dia == 'ausencia'` (`app/ponto/services/dia_pago.py:691`; `:701`). A grade também classifica ausência parcial como `ausencia`, mantendo `cobertura_parcial=True` (`app/escala/utils.py:1217`; `:1227`). O motor conta `_abono_inteiro` apenas entre dias sem trabalho (`app/ponto/motor_calculo_v2.py:2611`; `:2629`; `:2630`). Assim, não basta excluir justificativas parciais do contador para equiparar a semântica de todos os abonos.

4. **ABONA inteira precede o mapa parcial, mas sobra divergência entre motor e gravado com trabalho no dia.** Fonte remove o parcial (`app/ponto/services/efeito_ausencia.py:63`); motor não inclui dia trabalhado em `_abono`, portanto mapa vazio produz zero abono desse dia (`app/ponto/motor_calculo_v2.py:2611`; `:2628`; `:2634`). Grade classificada `ausencia` abona `previsto - realizado`, inclusive com trabalho (`app/ponto/services/dia_pago.py:394`; `:397`). Exemplo: previsto 660, realizado 180 → gravado 480; bloco do motor → zero. O teste isolado de precedência verifica mapa vazio e grade, sem exercitar o motor nessa composição (`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:207`).

5. **Âncora inválida conserva pela fonte, sem testes por nome.** Predicado único cobre ausência de âncora, papel errado e turno fechado (`app/ponto/turnos.py:2694`; `:2700`; `:2703`); fonte converte motivo em `cobre=None` (`app/ponto/services/efeito_ausencia.py:192`; `:194`; `:131`). Outra data de turno torna a âncora inexistente (`:184`). Porém cobertura inicialmente válida ainda pode virar vazia pelo filtro de cruzamento (`:205`; `app/ponto/turnos.py:2764`). Nesse caso sai de `justificadas` e `min_justificados` (`app/ponto/services/efeito_ausencia.py:46`; `:47`). Isso conserva a distinção anterior registrada em v1, não demonstra 0/0 de frota.

6. **DSR Comercial permanece risco condicional comprovado nos predicados, sem prova do pareamento completo.** Executei fonte e atribuições reais do Comercial em memória: turno de D22–06; âncora E em D+1 00:10 atribuída a D; outro trecho fechado D+1 02–04. Fonte retorna cobertura vazia e conjuntos vazios pelo cruzamento (`app/ponto/services/efeito_ausencia.py:205`; `app/ponto/turnos.py:2764`). Comercial toma entrada civil D+1, encontra D obrigatório sem trabalho/justificativa e dá `dsr_ok=False` (`app/ponto/motor_calculo_v2.py:3477`; `:3478`; `:3489`; `:3490`; `:3491`). O juiz admite herança da entrada noturna para a véspera (`app/ponto/turnos.py:299`; `:308`), mas **o pareador integral e a ocorrência produtiva desse cenário permanecem [nao verificado]**. Base usa dia da jornada e inclui períodos abertos; vazio não demonstra automaticamente falta inteira (`app/ponto/motor_calculo_v2.py:2499`; `:2501`; `:2583`).

7. **Exemplo 22–06 / “justificativa 23–02” exige distinguir os modelos.** `Justificativa` tem `data_turno`, `tipo` e `saida_informada`; não tem início/fim de intervalo arbitrário (`app/ponto/models.py:144`; `:146`; `:149`). Juiz nomeia exclusivamente `hi` ou `hf` (`app/ponto/turnos.py:2668`). `esqueci_saida` ancorada E23 sem saída informada mede **23–06 = 420**; com saída informada D+1 02 mede **02–06 = 240**, sem pausas (`app/ponto/services/efeito_ausencia.py:197`; `:202`; `app/ponto/turnos.py:2738`). Quem tem período 23–02 é `Ausencia.hora_ini/hora_fim` (`app/ponto/models.py:768`; `:770`): fim recebe +1440 e a classificação de batidas inclui ambas as bordas (`app/ponto/turnos.py:2652`; `:2655`; `:2657`). Duração paga usa diferença entre instantes, sem minuto adicional; toque de extremidades não é cruzamento (`:2779`; `:2764`).

8. **Exemplo 07–19 / pausa 10–11 / saída informada 07:30:** fonte mede **630 minutos** após aceitar âncora; saída informada desloca a ponta por `max` e pausa é subtraída (`app/ponto/services/efeito_ausencia.py:192`; `:198`; `:202`; `app/ponto/turnos.py:2792`). Com batida real S07:30, o turno fechado conserva inteiro antes da medição (`app/ponto/turnos.py:2703`; `app/ponto/services/efeito_ausencia.py:194`). Teste distingue essas duas situações (`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:172`; `:321`).

9. **Duas ausências:** cobertura escolhe uma por dia, sobrescrevendo em ordem data/pk, portanto não soma abono inteiro dessas ausências (`app/ponto/turnos.py:2916`; `:2944`). Caminho distinto continua somando minutos de todas as ausências parciais para crédito e resumo PDF (`app/ponto/services/efeito_ausencia.py:49`; `:52`; `app/ponto/services/credito_parcial.py:26`; `:28`; `app/relatorios/pdf_espelho.py:480`). Impacto dessa soma no pagamento efetivo: [nao verificado].

## 14/11 17:16 · sessao 9408059f510d · - · - · origem=Ronald (tmux)

Achados concretos:

1. **Prontidão conserva derivação paralela.** O caminho com snapshot soma `FechamentoMensal.minutos_abonados`; o fallback soma somente o previsto dos dias `ausencia`, ignorando `min_justificados` de dias `trabalho`. Exemplo sintético: previsto 660, realizado 0, justificativa parcial 630 → snapshot 95%; fallback 0%. (`app/folha/export.py:1078`, `:1079`, `:1095`, `:1100`, `:1106`, `:1111`; gravação dos 630: `app/ponto/services/dia_pago.py:398`, `app/ponto/services/fechamento.py:1067`)

2. **`dias_abono` continua com significados diferentes fora do caso parcial isolado.** O motor conta dias justificados sem trabalho, excluindo o mapa parcial. A lavratura agora conta qualquer linha `DiaPago` cuja grade atual seja `ausencia`, sem exigir minutos abonados positivos nem excluir ausência parcial. Uma ausência com par trabalhado pode dar motor 0 e lavratura 1; mudança posterior da grade altera o contador sem relavrar. (`app/ponto/motor_calculo_v2.py:2611`, `:2629`, `:2630`; `app/ponto/services/dia_pago.py:698`, `:701`, `:702`; transporte para resumo: `app/relatorios/cartao_pela_celula.py:188`)

3. **8932 não mede justificativa parcial.** `tipos_abonados_8932` consulta tipos do catálogo; `datas_abonadas` expande registros `Ausencia`, sem ler `Justificativa`, seu período ou minutos lavrados. Uma justificativa parcial isolada não entra nessa resposta. A pesquisa integral encontrou apenas testes chamando `datas_abonadas`; emissão efetiva da 8932 por esse caminho: **[nao verificado]**. (`app/folha/export.py:193`, `:198`, `:201`, `:204`, `:205`, `:146`, `:160`)

4. **Ausência parcial continua com outra aritmética.** A soma de `a.minutos` percorre todos os registros, sem união de períodos; `creditos_por_dia` também soma todos por dia. O PDF acrescenta essa soma ao abono do motor. Assim, a cura da união de justificativas não cura sobreposição de duas `Ausencia` parciais. (`app/ponto/services/efeito_ausencia.py:49`, `:52`; `app/ponto/services/credito_parcial.py:26`, `:28`; `app/relatorios/pdf_espelho.py:336`, `:480`)

Censo completo dos sítios de produção encontrados pela busca literal:

| Campo/função | Escritor/composição | Leitores/alimentadores |
|---|---|---|
| `periodos_justificados` | `app/ponto/services/efeito_ausencia.py:72`, `:127`, `:140` | efeito `:45`; `app/escala/utils.py:1198`, `:1251`, `:1257`, `:1266`, `:1268`, `:1275`, `:1276` |
| `intervalos_do_periodo_justificado` | `app/ponto/turnos.py:2783` | wrapper `:2780`; fonte `app/ponto/services/efeito_ausencia.py:129`, `:207` |
| União de intervalos | `app/ponto/services/efeito_ausencia.py:144` | `:140` |
| Mapa filtrado `min_justificados` | `app/ponto/services/efeito_ausencia.py:47`; retirada por ABONA inteira `:63`; retorno `:68` | fechamento `app/ponto/services/fechamento.py:667`; espelho `app/ponto/services/espelho.py:1134`; PDF `app/relatorios/pdf_espelho.py:407`; esmeril `app/ponto/services/esmeril_espelho.py:166`; extrato `app/relatorios/management/commands/diff_extrato_parcial.py:62`; diff espelho `app/ponto/management/commands/diff_espelho_alimentacao.py:124`; grade `app/escala/services/leitor_celula.py:365` |
| `min_justificados` na linha | `app/escala/utils.py:1276`; `app/escala/services/leitor_celula.py:436` | `app/ponto/services/dia_pago.py:398`, `:399` |
| `minutos_justificados` no motor | init `app/ponto/motor_calculo_v2.py:813`; fechamento normal/órfão `app/ponto/services/fechamento.py:755`, `:831`; espelho `app/ponto/services/espelho.py:1134`; PDF `app/relatorios/pdf_espelho.py:407`; diff `app/ponto/management/commands/diff_espelho_alimentacao.py:56`; extrato `app/relatorios/management/commands/diff_extrato_parcial.py:62`; esmeril `app/ponto/services/esmeril_espelho.py:166` | motor `app/ponto/motor_calculo_v2.py:2628`, `:2634`; contrato nominal `app/ponto/calculador/nucleo.py:424` |
| `dias_com_abono` | cálculo `app/ponto/services/dia_pago.py:690`, `:701`, `:702` | único chamador de produção localizado: `app/relatorios/cartao_pela_celula.py:188` |
| `minutos_abonados` | fechamento `app/ponto/services/fechamento.py:1067`; lavratura `app/ponto/services/dia_pago.py:288`, `:292`; retrato `:467`, `:479`, `:484` | cartão `app/relatorios/cartao_pela_celula.py:118`, `:186`; prontidão `app/folha/export.py:1078`; conferência pela mesma função `app/folha/porta_export.py:139` |

Os três chamadores laterais migraram para a fonte: diff espelho exige mapa explícito e o recebe no degrau D; extrato e esmeril alimentam justificadas e minutos antes de calcular. A/B/C do diff mantêm mapas explicitamente vazios. (`app/ponto/management/commands/diff_espelho_alimentacao.py:42`, `:56`, `:109`, `:111`, `:113`, `:117`, `:124`; `app/relatorios/management/commands/diff_extrato_parcial.py:41`, `:57`, `:62`; `app/ponto/services/esmeril_espelho.py:161`, `:162`, `:166`)

A cadeia até o gravado está conectada no código: grade transporta o número; `por_dia_da_grade` o lê; fechamento soma o mapa e fornece o mesmo mapa à lavratura. Execução ORM e igualdade produtiva: **[nao verificado]**. (`app/escala/services/leitor_celula.py:435`, `:436`; `app/ponto/services/dia_pago.py:398`; `app/ponto/services/fechamento.py:1066`, `:1067`, `:1217`)

## 14/11 17:16 · sessao 9cc2d627f055 · - · - · origem=Ronald (tmux)

Resultados do explorer, somente leitura:

- Os 15 testes sem banco passam; os 14 mutantes declarados morrem por asserção. A suíte carrega funções reais por AST, substituindo as bordas ORM; o runner exige falha sem erro (`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:38`; `app/ponto/tests/mutantes_justificativa_periodo_v2.py:76`). Execuções locais confirmadas, sem artefato escrito.
- Há mordida de classe na união de intervalos sobrepostos/disjuntos, duas pausas, duas âncoras, precedência ABONA e transporte à grade; não é apenas comparação com o golden (`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:186`, `:192`, `:201`, `:207`, `:258`).
- **Mutante plausível sobrevivente comprovado em memória:** trocar `ef['min_justificados']` por `{}` em `app/ponto/services/esmeril_espelho.py:166`. Todos os 15 continuam verdes quando a alteração antecede as mutações internas do teste 12. O selo só verifica Store e ordem textual, sem verificar RHS nem percurso (`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:83`, `:90`). O mutante declarado só apaga a atribuição, deixando essa classe descoberta (`app/ponto/tests/mutantes_justificativa_periodo_v2.py:42`).
- O teste 06 usa motor falso que devolve o próprio atributo; prova contrato de argumento/transporte, **não** cálculo do motor (`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:226`, `:235`). O teste 07 usa contador falso limitado a `data__in`; não prova filtros reais de colaborador, versão, tipo ou positividade (`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:243`). Esses testes têm mordida localizada, mas não sustentam certificação financeira integral.
- O teste 09 executa somente cinco instruções extraídas do bloco `_mj`, com previsto falso; não executa seleção dos dias obrigatórios, DSR ou motor completo (`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:293`, `:297`, `:299`). A suíte explicita não provar ORM/frota/DSR/TXT (`:6`).
- O teste ORM novo confere fechamento e DiaPago reais, mas só roda fechamento **uma vez**; não prova idempotência (`app/ponto/tests/test_justificativa_paga_o_periodo.py:398`, `:406`, `:408`). Não foi executado: import local de `django.core` confirmou `ModuleNotFoundError`; resultado ORM [nao verificado].
- **Estado integral duas vezes não é idêntico:** renovam-se timestamps de fechamento e DiaPago é apagado/recriado, mudando identidade/carimbo (`app/ponto/services/fechamento.py:1091`, `:1148`, `:1166`, `:1194`; `app/ponto/services/dia_pago.py:359`; `app/ponto/models.py:380`). Igualdade dos números sob entradas fixas no fechamento completo: [nao verificado].
- Executei o censo puro existente: zero escrita atribuída a FechamentoMensal fora da porta; o contrato e a porta estão em `app/folha/tests/test_chokepoint_folha_gate.py:58`, `:72`. O censo tem limites explícitos para escritas não atribuíveis (`app/core/censo_escritas.py:43`). No delta fornecido, nenhum novo escritor persistente.
- Não há trava nova no delta. Fechamento não possui `select_for_update` ou `atomic` próprio no percurso principal; acusar mudança de ordem de trava nesta volta não está demonstrado (`app/ponto/services/fechamento.py:448`; gravações `:1149`, `:1165`). Operações concorrentes completas [nao verificado].
- A guarda existente protege **somente 09/2026** (`app/ponto/services/fechamento.py:36`). 10 exportada recebe aviso, mas pode ser regravada se o chamador executar (`:539`, `:542`, `:1142`, `:1165`). Apply/deploy efetivo [nao verificado].
- Sem `.git` na cópia: o RED histórico, que exige `git show`, não pode ser reproduzido aqui (`app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:28`). Delta anexado não toca o motor; presença no diff inteiro e inventário exato histórico precisam ser sustentados pelo anexo v1 ou marcados [nao verificado]. No hash há init do memo e leitura parcial (`app/ponto/motor_calculo_v2.py:813`, `:2628`, `:2633`).

## 14/11 17:11 · sessao 750d1e2f857d · JUSTIFICATIVA-PAGA-O-PERIODO · construtor · origem=Code

Cura escrita na cópia. **Certificação pelo Code/opus permanece pendente.** O teste ORM está escrito, mas sua execução ficou bloqueada por `ModuleNotFoundError: No module named 'django.core'` em `manage.py:11`; resultado ORM [nao verificado]. [nao verificado]

O diff por arquivo::função:

| Arquivo::função | Mudança |
|---|---|
| `ponto/services/efeito_ausencia.py::periodos_justificados` | Une intervalos antes de medir (`:139`, `:144`). |
| `efeito_ausencia.py::_cobertura_de_uma` | Usa `cadeia_do_dia` com todas as pausas; considera a saída informada após aceitar a âncora (`:176`, `:201`). |
| `efeito_ausencia.py::efeitos_da_ausencia` | Ausência inteira ABONA retira o dia do mapa parcial (`:63`). |
| `ponto/turnos.py::intervalos_do_periodo_justificado` | Extrai os trechos sem pausas para a união na fonte (`:2783`). |
| `escala/services/leitor_celula.py::grade_da_celula` | Transporta o número da fonte até a grade usada pelo fechamento (`:365`, `:436`). |
| `ponto/services/dia_pago.py::por_dia_da_grade` | Abona também `trabalho`, lendo `min_justificados` da linha (`:397`). |
| `dia_pago.py::dias_com_abono` | Conta dias lavrados classificados como `ausencia` (`:701`). |
| `ponto/services/fechamento.py::recalcular_fechamento_mes` | Lê o mapa filtrado de `efeitos_da_ausencia`, eliminando a derivação própria (`:667`). |
| `diff_espelho_alimentacao.py::_motor/handle` | Exige mapa explícito e alimenta o degrau D pela fonte (`:42`, `:124`). |
| `diff_extrato_parcial.py::_antes` | Alimenta justificadas e mapa pela fonte (`:43`, `:62`). |
| `ponto/services/esmeril_espelho.py::assinaturas_do_colab` | Alimenta justificadas e mapa pela fonte (`:161`, `:166`). |
| `relatorios/cartao_pela_celula.py::totais_da_lavratura` | Lê a contagem compartilhada de dias inteiros (`:188`). |

Os casos e seus números estão registrados nos testes: [nao verificado]

| Caso | Resposta exigida |
|---|---|
| E07:00, saída **informada** 07:30, pausa 10–11 | **630 min** — `test_justificativa_periodo_v2_sem_banco.py:172` |
| 07–19, duas pausas de 85 min | **550**, contra 635 na base — `:186` |
| E07:30/S11/E12, duas âncoras | **420**, contra 840 na base — `:192` |
| ABONA inteira junto de parcial | Mapa parcial `{}`; abono **660−180=480** — `:207` |
| Inteiro junto de parcial | **720+630=1.350**, um dia inteiro — `:280` |
| Controle do motor completo | **720+690=1.410** — `test_justificativa_paga_o_periodo.py:268` |
| Fechamento real e DiaPago | Ambos devem gravar **630** — `test_justificativa_paga_o_periodo.py:367`; execução [nao verificado] |

O caso com **batida S07:30** continua conservando o inteiro, conforme o aval; ele é distinguido da saída apenas informada (`test_justificativa_periodo_v2_sem_banco.py:321`).

O selo mantém a distinção da casa: atributo ausente é cobrado por `exige_memos`; omissão do chamador é cobrada pela alimentação explícita antes do cálculo. `{}` informado continua legítimo (`ponto/calculador/nucleo.py:424`; `ponto/tests/test_s5b_porta_unica.py:985`; `test_justificativa_periodo_v2_sem_banco.py:220`).

Comandos executados e resultados locais:

```bash
# RED: lê os arquivos da base informada, sem restaurá-los no disco
PYTHONDONTWRITEBYTECODE=1 JPP_BASE=1 python3 -m unittest ponto.tests.test_justificativa_periodo_v2_sem_banco -q
# Ran 15 tests — FAILED (failures=15)

# GREEN isolado
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest ponto.tests.test_justificativa_periodo_v2_sem_banco -q
# Ran 15 tests — OK

# Mutantes em memória
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_justificativa_periodo_v2 --todos
# 14/14 mortos por assercao, sem escrever fontes
```

O carregamento da base está em `test_justificativa_periodo_v2_sem_banco.py:27`; os mutantes e seus testes estão em `mutantes_justificativa_periodo_v2.py:12`. Cada mutante também tem comando individual: [nao verificado]

```bash
for mutante in \
  ignora_saida_informada gravado_ignora_periodo \
  fonte_so_primeira_pausa medidor_so_primeira_pausa soma_ancoras \
  ausencia_nao_precede fechamento_rederiva_mapa \
  diff_sem_mapa extrato_sem_mapa esmeril_sem_mapa diff_aceita_omissao \
  contador_inclui_trabalho grade_perde_periodo motor_velho_em_memoria
do
  PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_justificativa_periodo_v2 "$mutante" || exit 1
done
```

O comando Django deixado para a sessão é:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  ponto.tests.test_justificativa_paga_o_periodo \
  ponto.tests.test_justificativa_periodo_v2_sem_banco \
  ponto.tests.test_s5b_porta_unica \
  relatorios.tests.test_s3_cartao_le_dia_pago \
  escala.tests.test_grade_alimentada \
  escala.tests.test_cobparc7_leitor_transporta_a_lampada \
  --settings=config.settings_test --noinput
```

Depois da cura, o censo fica:

| Campo | Quem escreve/alimenta | Quem lê |
|---|---|---|
| `min_justificados` | Fonte: `efeito_ausencia.py:47`, `:63`; grades: `escala/utils.py:1276`, `leitor_celula.py:436` | `dia_pago.py:397`; `fechamento.py:667`; alimentadores do motor |
| `minutos_justificados` | Init: `motor_calculo_v2.py:813`; fechamento `:755`, `:831`; espelho `:1134`; PDF `:407`; laterais acima | `motor_calculo_v2.py:2628` |
| `minutos_abonados` | Fechamento: `fechamento.py:1067`; DiaPago: `dia_pago.py:288`; retrato: `:484` | `cartao_pela_celula.py:118`, `:186`; prontidão: `folha/export.py:1078` |
| `dias_abono` | Motor: `motor_calculo_v2.py:2630`; lavratura: `dia_pago.py:701` | Espelho `:1253`; PDF `:479`, `:787`; cartão `:188` |
| `horas_abono` | Motor: `motor_calculo_v2.py:2631`, `:2633`; resumo por minutos gravados: `cartao_pela_celula.py:118`, `:186` | PDF: `pdf_espelho.py:480` |

`folha/export.py` permanece com os leitores existentes: prontidão usa `minutos_abonados`; seu fallback ainda soma o previsto inteiro dos dias `ausencia` (`:1078`, `:1100`). A rubrica TXT 8932 continua derivando dos tipos de Ausência (`:193`, `:202`). Igualdade dos hashes exportados e pagamento efetivo [nao verificado].

O DSR Comercial continua usando data civil da entrada e `self.datas_justificadas`; a cura alcança sua alimentação, sem corrigir essa rederivação (`motor_calculo_v2.py:3477`, `:3478`, `:3490`).

Não houve implementação em motor, porta da célula, juízes, models/migrations, export, crons ou docs; o diff protegido foi conferido vazio. Não executei banco, Docker, rede, commit, merge, push, deploy ou apply. **Frota 0/0, hashes das competências exportadas, multiplicidade dos 45 e contagens do admin por motivo: [nao verificado].** [nao verificado]

## 14/11 17:00 · sessao 8fb89e816b92 · E3-V10-DEGRAU-PUB · censo · origem=Code

**Nenhum B identificado nos 40 testes.** [não verificado: conclusão da triagem estática, sem reexecução] [nao verificado]

Referências relativas a `app/`. Nomes dos testes sem o prefixo `test_`. [nao verificado]

Regras usadas: **1** = responder + pergunta pendente no app, independentemente da âncora/prazo (`chamados/services/tarefa_do_dia.py:43`); **2** = ação administrativa/sistema alcançável (`:45`); **3** = residual sem `cobrar` (`:46`). **4*** = publicar rota somente com dono (`chamados/services/tarefa_do_dia.py:612`); sua numeração literal como item 4 é **[não verificado]**, pois o topo enumera apenas 1–3.

| teste | arquivo:linha do assert/erro | CLASSE | o que mudar e onde | [nao verificado]
|---|---|---|---|
| MORDE_o_universo_e_o_mesmo_do_fora_da_folha | chamados/tests/test_n1_alimentacao_unica.py:127 | F | Helper `_conta_pda`, em `:90`, precisa receber/repassar `em_lote` e `agora`; assinatura exigida em `chamados/services/pergunta_no_app.py:74`. Preservar igualdade dos universos. |
| RED_a_porta_e_alimentada_uma_vez_por_rodada | chamados/tests/test_n1_alimentacao_unica.py:99 | F | Mesmo helper `:90`: repassar os argumentos novos. Preservar alimentação única em `:101`; consulta do app é memorizada separadamente por colab (`chamados/services/tarefa_do_dia.py:551`). |
| RED_o_que_esta_no_app_se_pergunta_uma_vez_por_colaborador | chamados/tests/test_n1_alimentacao_unica.py:106 | F | Corrigir helper `:90`; depois, regra **1**: trocar expectativa zero de `:107` por uma consulta por colab utilizado na carga, constante em dias (`chamados/services/tarefa_do_dia.py:551`). |
| RED_o_que_segura_semeia_o_mapa_com_o_colaborador_pedido | chamados/tests/test_n1_alimentacao_unica.py:147 | F | `_le`, em `:141`, deve repassar kwargs. Regra **1**: atualizar zero leituras em `:149`; preservar proibição de busca individual em `:153` (`chamados/services/pergunta_no_app.py:59`). |
| a_saida_do_dia_nao_muda | chamados/tests/test_n1_alimentacao_unica.py:111 | F | Corrigir `_conta_pda` em `:90`. Regra **1** conserva a porta existente neste cenário pendente; manter verificações de saída (`chamados/tests/test_n1_alimentacao_unica.py:113`). |
| v8_R10_ancora_fora_do_fato_sem_acao | chamados/tests/test_e3_portas_tarefa.py:601 → chamados/services/tarefa_do_dia.py:551 | F | Inicializar `_pendentes_app` no objeto criado por `__new__`, `chamados/tests/e3_v8_apoio.py:127`; fornecer pendentes vazios ou mock de consulta. Inicialização real em `chamados/services/tarefa_do_dia.py:388`. |
| lote_responde_o_mesmo_que_o_avulso_e_com_queries_constantes | chamados/tests/test_e2_tarefa_do_dia.py:1180 | F | Separar custo do lote e nova consulta por colab; preservar igualdade lote/avulso. Parcela nova constante com dias, crescente com colabs; não fixar simplesmente 26 (`chamados/services/tarefa_do_dia.py:551`). |
| lote_tem_queries_constantes | chamados/tests/test_e2_tarefa_do_dia.py:938 | F | Comparação altera simultaneamente dias e colabs. Medir dias com colabs fixos e contabilizar consulta por colab; não substituir por teto 45 (`chamados/tests/test_e2_tarefa_do_dia.py:931`; `chamados/services/tarefa_do_dia.py:551`). |
| RED_8_pergunta_sob_ancora_REGISTRADA_nao_e_canal | chamados/tests/test_e3_bloqueantes.py:168 | L | **1**: novo `porta_app/colab`; atualizar também `:170` para excluir dos sem canal (`chamados/services/tarefa_do_dia.py:43`). |
| RED_8_pergunta_sob_ancora_SILENCIADA_nao_e_canal | chamados/tests/test_e3_bloqueantes.py:161 | L | **1**: novo `porta_app/colab`; atualizar também `:163` para excluir dos sem canal (`chamados/services/tarefa_do_dia.py:43`). |
| classe_e_viva_sob_terminal_reabre_pela_porta_com_nota | chamados/tests/test_e4_passivo.py:73 → chamados/tests/test_e4_backfill.py:112 | L | **1**: pendente sob terminal já tem `porta_app/colab`; esperar plano desse dia zero, sem reabertura/nota. Atualizar caracterização em `:73` (`chamados/services/tarefa_do_dia.py:43`; `chamados/services/vigia_fio.py:658`). |
| mutante_sem_porta_morde_caracterizacao_classe_e | chamados/tests/test_e4_passivo.py:90 | L | **1**: neste cenário, reabrir credor deixou de ser necessário. Reenquadrar teste de `:87` para verificar ausência dessa chamada; trocar apenas regex esconderia a mudança (`chamados/services/tarefa_do_dia.py:43`). |
| MORDE_dry_nao_escreve | chamados/tests/test_f3_beco1_reata_limbo.py:72 | L | **1**: novo zero candidatos/pais C11; preservar ausência de escrita. Para exercitar DRY com candidatos, montar cenário realmente sem porta (`escala/services/cadastro_realidade.py:469`). |
| MORDE_pai_cujo_silencio_ja_venceu_volta_a_fila_e_reata | chamados/tests/test_f3_beco1_reata_limbo.py:159 | L | **1**: pergunta já tem porta do app; novo zero reatas. Refazer cenário sem porta para continuar cobrindo silêncio vencido (`escala/services/cadastro_realidade.py:469`). |
| MORDE_sem_pai_fica_na_lista_e_nenhum_pai_nasce | chamados/tests/test_f3_beco1_reata_limbo.py:82 | L | **1**: novo sem linha `SEM PAI`, pois não entra em C11. Preservar teste de ausência de pai com cenário sem porta (`escala/services/cadastro_realidade.py:469`). |
| RED_apply_reata_ao_pai_e_o_dia_sai_do_beco | chamados/tests/test_f3_beco1_reata_limbo.py:53 | L | **1**: novo `_c11()==[]` antes do apply; zero reatas. Refazer cenário sem porta para testar costura (`escala/services/cadastro_realidade.py:469`). |
| RED_pai_silenciado_DEPOIS_da_selecao_nao_recebe_reata | chamados/tests/test_f3_beco1_reata_limbo.py:145 | L | **1**: cenário atual não seleciona C11, portanto não alcança a corrida. Criar candidato sem porta e preservar guarda contra reata após silêncio (`escala/services/cadastro_realidade.py:469`). |
| RED_pai_silenciado_fora_da_fila_nao_recebe_reata | chamados/tests/test_f3_beco1_reata_limbo.py:119 | L | **1**: novo sem linha `PAI FORA DA FILA`; há porta do app. Refazer cenário sem porta para conservar essa cobertura (`escala/services/cadastro_realidade.py:469`). |
| MORDE_F5_queries_nao_crescem_com_os_dias | escala/tests/test_f3_beco1_folga_na_lista.py:322 | L | **1**: novo `(d1,d5)=(0,0)`. Refazer cenário C11 sem porta antes de medir queries; consulta nova fica uma por colab, constante com dias (`escala/services/cadastro_realidade.py:469`; `chamados/services/tarefa_do_dia.py:551`). |
| MORDE_competencia_anterior_NAO_exportada_vai | escala/tests/test_f3_beco1_folga_na_lista.py:110 | L | **1**: novo C11 `[]`; competência anterior não elimina porta da pergunta pendente (`chamados/services/tarefa_do_dia.py:43`; `escala/services/cadastro_realidade.py:469`). |
| RED_F1_lista_le_o_instante_de_quem_chama | escala/tests/test_f3_beco1_folga_na_lista.py:219 | L | **1**: novo C11 `[]` nesse instante. Refazer cenário sem porta para preservar prova do relógio (`escala/tests/test_f3_beco1_folga_na_lista.py:215`; `escala/services/cadastro_realidade.py:469`). |
| RED_F6_C9_lavrado_divergente_nao_tira_o_dia_CADASTRO_da_mesa | escala/tests/test_f3_beco1_folga_na_lista.py:265 | L | **1** remove C11; novo conjunto `{('C9','BATIDA')}`, preservando dono lavrado. Refazer C11 sem porta para continuar testando reconciliação de donos (`escala/tests/test_f3_beco1_folga_na_lista.py:262`; `escala/services/cadastro_realidade.py:988`). |
| RED_contraex1_chamado_da_celula_na_fila_sem_pergunta_NAO_e_porta | escala/tests/test_f3_beco1_folga_na_lista.py:180 | L | **1**: manter rota/fila/verbos; novo `fio='colab'` pela pergunta pendente sob outra âncora. C11 continua vazio (`escala/tests/test_f3_beco1_folga_na_lista.py:173`; `chamados/services/tarefa_do_dia.py:43`). |
| RED_contraex2_ancora_viva_fora_da_fila_nao_e_porta | escala/tests/test_f3_beco1_folga_na_lista.py:204 | L | **1**: novo C11 `[]`; pendentes sob terminal/registrado têm porta do app (`escala/tests/test_f3_beco1_folga_na_lista.py:193`; `chamados/services/tarefa_do_dia.py:43`). |
| RED_dia_da_competencia_aberta_vai_a_lista_com_dono_CADASTRO | escala/tests/test_f3_beco1_folga_na_lista.py:88 | L | **1**: novo C11 `[]`, sem dono C11. Refazer cenário sem porta para testar destino CADASTRO (`escala/services/cadastro_realidade.py:469`). |
| MORDE_ancora_arquivada_nao_abre_a_porta_da_pergunta | chamados/tests/test_o261e_f5b_juiz_porta.py:272 | L | **1**: novo fio da folha `colab`, com `porta_app`; arquivamento da âncora não impede resposta pendente (`chamados/services/tarefa_do_dia.py:43`). |
| MORDE_a_porta_vem_da_TAREFA_e_nao_do_app | chamados/tests/test_o261e_f5_alcance.py:256 | L | **1, 4***: carga deve consultar app; mock vazio em `:253` implica `nada/None`. Atualizar também contadores em `:255`, além de zero→uma consulta (`chamados/services/tarefa_do_dia.py:551`; `:616`). |
| a_ancora_VIVA_fora_da_fila_tambem_e_veu | chamados/tests/test_o261e_f5_alcance.py:460 → helper :134 | L | **1**: novo `porta_app/colab`; retirar expectativa de causa sem porta `a/registrado` (`chamados/services/tarefa_do_dia.py:43`). |
| b_respondida_com_resposta_VAZIA_e_porta_do_COLABORADOR_pela_tarefa | chamados/tests/test_o261e_f5_alcance.py:196 | L | **1, 4***: carimbo respondido exclui dos pendentes; novo `nada/None`, não colab. A cópia já suprime a rota sem dono exibida no anexo (`chamados/services/pergunta_no_app.py:75`; `chamados/services/tarefa_do_dia.py:616`). |
| e_pergunta_viva_sob_chamado_TERMINAL_tem_nome | chamados/tests/test_o261e_f5_alcance.py:222 → helper :134 | L | **1**: novo `porta_app/colab`; não pertence à lista sem porta da causa `e` (`chamados/services/tarefa_do_dia.py:43`). |
| MORDE_a_frase_do_fio_nada_nao_afirma_o_que_o_juiz_nao_garante | folha/tests/test_haiku_fora_da_folha.py:173 | M | **4***: renovar substrings para texto `SEM PORTA`, preservando garantia de ação alcançável (`folha/fora_da_folha.py:54`; `chamados/services/tarefa_do_dia.py:612`). |
| o_dia_que_acusa_diz_o_veredito_e_o_que_DESTRAVA | folha/tests/test_haiku_fora_da_folha.py:81 | M | **4***: `SEM FIO`→`SEM PORTA`; preservar verificação `fio=nada` (`folha/fora_da_folha.py:54`; `chamados/services/tarefa_do_dia.py:612`). |
| endpoint_do_app_filtra_por_q_vivas | chamados/tests/test_contract_pergunta_viva.py:76 | M | Atualizar string para `perguntas_do_app(colab, em_lote=True, agora=agora)`; conservar lote/C6 e encaminhamento do relógio (`chamados/services/pergunta_no_app.py:74`). |
| todo_mutante_da_E3_tem_alvo_unico | chamados/tests/test_e3_mutantes_alvo.py:25 | M | Retargetar `consulta_vazia` e `consulta_sem_lote` para assinatura com `agora=agora`. **Já ajustados na cópia**; manter unicidade (`chamados/tests/e3_mutantes.py:34`; `:59`). |
| cobrar_precede_validar_e_decidir_sem_depender_dos_donos | chamados/tests/test_e3_portas_tarefa.py:478 | L | **2, 3**: novo `fio='admin'`; conservar fila/verbos. `cobrar` residual não dá dono colab (`chamados/services/tarefa_do_dia.py:45`; `:46`). |
| fio_normal_e_do_dono_da_acao_mesmo_com_verbo_validar | chamados/tests/test_e3_portas_tarefa.py:457 | L | **1, 4***: entrada não fornece pergunta pendente; novo `nada/None`. Esse `validar` está na porta da ação, não no residual (`chamados/services/tarefa_do_dia.py:584`; `:606`). |
| RED_A_na_fila_aparece_com_acao_sob_B | chamados/tests/test_e3_portas_tarefa.py:413 | L | **1, 3**: novo `fio='chamado'` pelo residual `decidir`; conservar rota/fila/verbos. Não há pendente fornecida (`chamados/tests/test_e3_portas_tarefa.py:411`; `chamados/services/tarefa_do_dia.py:46`). |
| v8_R1_colab_alcancavel_e_leitores | chamados/tests/test_e3_portas_tarefa.py:490 | L | **1, 4***: entrada atual dá `nada/None`. Para preservar cenário nominal colab, fornecer `pendentes_app={303}` em `:489`; então colab é justificado pela regra **1** (`chamados/services/tarefa_do_dia.py:43`). |
| v8_R2_validacao_e_resposta_alcancaveis_no_mesmo_dia — ambos subtestes | chamados/tests/test_e3_portas_tarefa.py:514 | L | **1, 2**: entrada atual dá `porta_fila/admin`, mantendo verbos/ações. Para provar simultaneidade com colab, fornecer identidade da pergunta e pendentes em `:509`/`:513` (`chamados/services/tarefa_do_dia.py:43`; `:45`). |
| alvos | chamados/tests/test_e3_volta7_sem_banco.py:92 → chamados/tests/test_e3_mutantes_alvo.py:25 | M | Mesmo selo delegado: conservar execução; alvos com relógio já atualizados na cópia (`chamados/tests/e3_mutantes.py:34`; `:59`). |

**B: nenhum identificado. Contagem: L=27; F=8; M=5; B=0; ?=0 — 40 testes distintos.** [não verificado por execução; consolidação das linhas acima] [nao verificado]

## 14/11 16:59 · sessao 47b7900a56ce · - · - · origem=Ronald (tmux)

| Teste | Assert/erro | Classe | Mudança justificada | [nao verificado]
|---|---|---|---|
| `test_v8_R10_ancora_fora_do_fato_sem_acao` | `app/chamados/tests/test_e3_portas_tarefa.py:601`; erro `app/chamados/services/tarefa_do_dia.py:551` | F | Inicializar `_pendentes_app={1: frozenset()}` no helper que usa `__new__`, `app/chamados/tests/e3_v8_apoio.py:127`. A carga real inicializa o memo em `app/chamados/services/tarefa_do_dia.py:388`. Não introduz queries no helper; na carga real, consulta uma vez por colab, constante com os dias, crescente com colabs (`app/chamados/services/tarefa_do_dia.py:551`). |
| `test_RED_8_pergunta_sob_ancora_SILENCIADA_nao_e_canal` | `app/chamados/tests/test_e3_bloqueantes.py:161` | L | Item 1: pergunta pendente com responder tem porta independente da âncora; novo `rota='porta_app'`, `fio='colab'` (`app/chamados/services/tarefa_do_dia.py:43`). Atualizar também a exclusão do furo: `assertNotIn` em `app/chamados/tests/test_e3_bloqueantes.py:163`; porta publicada sai dos sem canal (`app/chamados/services/furo_sem_canal.py:104`). |
| `test_RED_8_pergunta_sob_ancora_REGISTRADA_nao_e_canal` | `app/chamados/tests/test_e3_bloqueantes.py:168` | L | Item 1: novo `porta_app/colab`, independentemente da âncora registrada (`app/chamados/services/tarefa_do_dia.py:43`). Atualizar também para `assertNotIn` em `app/chamados/tests/test_e3_bloqueantes.py:170`, conforme consumo da porta em `app/chamados/services/furo_sem_canal.py:98`. |
| `test_classe_e_viva_sob_terminal_reabre_pela_porta_com_nota` | `app/chamados/tests/test_e4_passivo.py:73` → helper `app/chamados/tests/test_e4_backfill.py:112` | L | Item 1: responder pendente abre `porta_app/colab` sem reabrir terminal (`app/chamados/services/tarefa_do_dia.py:43`). Novo cenário: plano desse dia `0`, sem reabertura/nota; adaptar verificações em `app/chamados/tests/test_e4_passivo.py:73`. O vigia trata porta antes da classe sem porta (`app/chamados/services/vigia_fio.py:658`). |
| `test_mutante_sem_porta_morde_caracterizacao_classe_e` | `app/chamados/tests/test_e4_passivo.py:90` | L | Mesma mudança do item 1: reabrir credor deixou de ser necessário para esta pendente (`app/chamados/services/tarefa_do_dia.py:43`). Reenquadrar o selo em `app/chamados/tests/test_e4_passivo.py:87` para verificar que a porta de reabertura não é chamada; não basta trocar o regex, pois o alvo comportamental mudou (`app/chamados/services/vigia_fio.py:658`). |
| `test_RED_A_na_fila_aparece_com_acao_sob_B` | `app/chamados/tests/test_e3_portas_tarefa.py:413` | L | Itens 1 e 3: sem pergunta pendente fornecida, responder não dá colab; residual A `decidir` dá `chamado`. Novo `('porta_fila',(101,),{101:'decidir',202:'cobrar'},'chamado')`; modificar expectativa em `app/chamados/tests/test_e3_portas_tarefa.py:414`, pelas regras `app/chamados/services/tarefa_do_dia.py:43` e `:46`. |
| `test_fio_normal_e_do_dono_da_acao_mesmo_com_verbo_validar` | `app/chamados/tests/test_e3_portas_tarefa.py:457` | L | Itens 1 e 4: insumo atual não informa pergunta pendente; novo `fio='nada'`, `rota=None` (`app/chamados/services/tarefa_do_dia.py:43`, `:612`). `validar` pertence à porta da ação, portanto não é residual separado (`app/chamados/services/tarefa_do_dia.py:584`, `:606`). Alterar expectativa em `app/chamados/tests/test_e3_portas_tarefa.py:457`. |
| `test_cobrar_precede_validar_e_decidir_sem_depender_dos_donos` | `app/chamados/tests/test_e3_portas_tarefa.py:478` | L | Itens 2 e 3: validar/admin alcançável permanece; cobrar residual não cria colab. Novo mesmo `fila/verbos`, `fio='admin'`; modificar `app/chamados/tests/test_e3_portas_tarefa.py:479`, conforme `app/chamados/services/tarefa_do_dia.py:45`. |
| `test_v8_R1_colab_alcancavel_e_leitores` | `app/chamados/tests/test_e3_portas_tarefa.py:490` | L | Itens 1 e 4: embora tenha `pergunta_id=303`, não fornece pendentes; novo `fio='nada'`, `rota=None`, `porta_da_tarefa=None`, mantendo verbos (`app/chamados/services/tarefa_do_dia.py:43`, `:612`). Atualizar assert e projeções em `app/chamados/tests/test_e3_portas_tarefa.py:490`. Para preservar o cenário nominal alcançável, fornecer explicitamente `pendentes_app={303}` na chamada de `:489`. |
| `test_v8_R2_validacao_e_resposta_alcancaveis_no_mesmo_dia` — ambos subtestes | `app/chamados/tests/test_e3_portas_tarefa.py:514` | L | Itens 1 e 2: fixture não fornece pergunta identificada nem pendentes; `dia_plano` usa `pergunta_id=None` (`app/chamados/tests/e3_portas_apoio.py:29`). Novo `('porta_fila','admin',verbos)` em ambos, mantendo ações; mudar `app/chamados/tests/test_e3_portas_tarefa.py:514`, conforme `app/chamados/services/tarefa_do_dia.py:43`. Para continuar provando simultaneidade com colab pendente, fornecer identidade e `pendentes_app` em `:509`/`:513`. |

Nestes **10 testes distintos: L=9, F=1, B=0**; R2 aparece duas vezes por subtestes (`app/chamados/tests/test_e3_portas_tarefa.py:507`). A cópia já impede publicar rota com `fio=nada`, embora o anexo anterior mostre `porta_pergunta/nada` (`app/chamados/services/tarefa_do_dia.py:616`).

O topo enumera somente itens 1–3 (`app/chamados/services/tarefa_do_dia.py:43`); usei “item 4” como a regra explícita do aval 16:16 sobre publicação somente com dono, registrada em `app/chamados/services/tarefa_do_dia.py:612`. A numeração literal 1–4 no corte 16:16 permanece [não verificado].

## 14/11 16:58 · sessao af390de1e0e6 · - · - · origem=Ronald (tmux)

Na cópia, os itens numerados são apenas 1–3; a regra de publicação às 16:16 aparece separadamente, sem número (`app/chamados/services/tarefa_do_dia.py:43`, `:45`, `:46`, `:612`). O item 4 literal em CORTES é [não verificado].

| Teste | Assert/erro na cópia | Classe | Mudança justificada | [nao verificado]
|---|---|---|---|
| `test_MORDE_ancora_arquivada_nao_abre_a_porta_da_pergunta` | `app/chamados/tests/test_o261e_f5b_juiz_porta.py:272` | L | Item 1: pergunta pendente + responder dá `colab`, mesmo com âncora arquivada; sem fila, `porta_app` (`app/chamados/services/tarefa_do_dia.py:43`, `:621`). Atualizar expectativa da folha. |
| `test_MORDE_a_porta_vem_da_TAREFA_e_nao_do_app` | `app/chamados/tests/test_o261e_f5_alcance.py:256` | L | Item 1 torna a consulta necessária: carga consulta uma vez por colaborador (`app/chamados/services/tarefa_do_dia.py:551`). O mock deliberadamente vazio em `app/chamados/tests/test_o261e_f5_alcance.py:253` significa pergunta fora dos pendentes: esperar `nada`, sem porta, e não conservar `(fio_morto=0, porta_pergunta=1)` (`app/chamados/services/tarefa_do_dia.py:598`, `:616`). |
| `test_a_ancora_VIVA_fora_da_fila_tambem_e_veu` | `app/chamados/tests/test_o261e_f5_alcance.py:460`; helper `:134` | L | Item 1: trocar causa `a/registrado` por `porta_app/colab`; a fila da âncora não limita o colaborador (`app/chamados/services/tarefa_do_dia.py:43`). |
| `test_b_respondida_com_resposta_VAZIA_e_porta_do_COLABORADOR_pela_tarefa` | `app/chamados/tests/test_o261e_f5_alcance.py:196` | L | Item 1: carimbo de resposta vazia não integra pendentes (`app/chamados/tests/test_o261e_f5_alcance.py:109`; `app/chamados/services/pergunta_no_app.py:75`). Não esperar `colab`; sem outro dono, `fio=nada`, `rota=None` pela publicação (`app/chamados/services/tarefa_do_dia.py:612`). O anexo mostra `porta_pergunta/nada`, estado já eliminado na cópia por `:616`. |
| `test_e_pergunta_viva_sob_chamado_TERMINAL_tem_nome` | `app/chamados/tests/test_o261e_f5_alcance.py:222`; helper `:134` | L | Item 1: pendente a responder conserva `porta_app/colab` sob terminal; não é linha sem porta da causa `e` (`app/chamados/services/tarefa_do_dia.py:43`, `:621`). |
| `test_MORDE_a_frase_do_fio_nada_nao_afirma_o_que_o_juiz_nao_garante` | `app/folha/tests/test_haiku_fora_da_folha.py:173` | M | Renovar as três substrings antigas de `:172` para o contrato textual publicado, `SEM PORTA: a tarefa nao aponta acao alcancavel neste dia; alguem precisa olhar` (`app/folha/fora_da_folha.py:54`), coerente com publicação apenas com dono (`app/chamados/services/tarefa_do_dia.py:612`). |
| `test_o_dia_que_acusa_diz_o_veredito_e_o_que_DESTRAVA` | `app/folha/tests/test_haiku_fora_da_folha.py:81` | M | Selo textual mudou de `SEM FIO` para `SEM PORTA`; preservar assert `fio=nada` em `:80` e verificar o texto publicado (`app/folha/fora_da_folha.py:54`; `app/chamados/services/tarefa_do_dia.py:616`). |

Contagem deste subconjunto: **L=5, M=2, B=0** (classificações e fundamentos nas sete linhas acima).

## 14/11 16:58 · sessao 2b92adb14c68 · - · - · origem=Ronald (tmux)

Os **13 testes deste recorte são L**: `_beco` cria pergunta de marco sem resposta em disputa aberta (`app/escala/tests/test_f3_beco1_folga_na_lista.py:55-57`), pendente no app (`app/chamados/services/pergunta_no_app.py:74-76`). Pela lei **item 1**, responder+pendente dá `fio='colab'`, independentemente da âncora (`app/chamados/services/tarefa_do_dia.py:43-44`). C11 só admite tarefa sem porta (`app/escala/services/cadastro_realidade.py:469`).

| teste | assert/erro | CLASSE | mudança | [nao verificado]
|---|---|---|---|
| `test_MORDE_dry_nao_escreve` | `app/chamados/tests/test_f3_beco1_reata_limbo.py:72` | L | Item 1: cenário atual tem `porta_app/colab`; esperar zero candidatos/zero pais no DRY. Refazer cenário sem porta para conservar cobertura do command (`app/escala/services/cadastro_realidade.py:469`). |
| `test_MORDE_pai_cujo_silencio_ja_venceu_volta_a_fila_e_reata` | `app/chamados/tests/test_f3_beco1_reata_limbo.py:159` | L | Item 1: cenário atual tem porta do app; zero reatas. Refazer cenário sem porta para testar vencimento do silêncio (`app/escala/services/cadastro_realidade.py:469`). |
| `test_MORDE_sem_pai_fica_na_lista_e_nenhum_pai_nasce` | `app/chamados/tests/test_f3_beco1_reata_limbo.py:82` | L | Item 1: não entra em C11; sem linha `SEM PAI`, zero reatas. Refazer cenário sem porta para testar ausência de pai (`app/escala/services/cadastro_realidade.py:469`). |
| `test_RED_apply_reata_ao_pai_e_o_dia_sai_do_beco` | `app/chamados/tests/test_f3_beco1_reata_limbo.py:53` | L | Item 1: `_c11()` novo é `[]` antes do apply; zero reatas. Refazer cenário sem porta para cobrir reapontamento (`app/escala/services/cadastro_realidade.py:469`). |
| `test_RED_pai_silenciado_DEPOIS_da_selecao_nao_recebe_reata` | `app/chamados/tests/test_f3_beco1_reata_limbo.py:145` | L | Item 1: cenário atual nem seleciona C11; não alcança a disputa de concorrência. Refazer cenário sem porta, conservando a proibição de reatar após silêncio (`app/escala/services/cadastro_realidade.py:469`). |
| `test_RED_pai_silenciado_fora_da_fila_nao_recebe_reata` | `app/chamados/tests/test_f3_beco1_reata_limbo.py:119` | L | Item 1: cenário atual não entra em C11; não há linha `PAI FORA DA FILA`. Refazer cenário sem porta para testar a guarda (`app/escala/services/cadastro_realidade.py:469`). |
| `test_MORDE_F5_queries_nao_crescem_com_os_dias` | `app/escala/tests/test_f3_beco1_folga_na_lista.py:322` | L | Item 1: neste cenário `(d1,d5)` novo é `(0,0)`; para preservar o teste de custo, refazer cenário realmente sem porta e manter assert de queries constantes por dia (`app/escala/services/cadastro_realidade.py:429-432,469`). |
| `test_MORDE_competencia_anterior_NAO_exportada_vai` | `app/escala/tests/test_f3_beco1_folga_na_lista.py:110` | L | Item 1: aberta/anterior não suprime a porta do app; C11 novo `[]` (`app/chamados/services/tarefa_do_dia.py:43-44`; `app/escala/services/cadastro_realidade.py:469`). |
| `test_RED_F1_lista_le_o_instante_de_quem_chama` | `app/escala/tests/test_f3_beco1_folga_na_lista.py:219` | L | Item 1: no instante passado fornecido a pergunta está pendente; C11 novo `[]`. Refazer cenário sem porta para conservar a prova do relógio (`app/escala/tests/test_f3_beco1_folga_na_lista.py:215-218`; `app/escala/services/cadastro_realidade.py:469`). |
| `test_RED_F6_C9_lavrado_divergente_nao_tira_o_dia_CADASTRO_da_mesa` | `app/escala/tests/test_f3_beco1_folga_na_lista.py:265` | L | Item 1 remove C11; resta só C9 lavrado `BATIDA`, portanto novo conjunto `{('C9','BATIDA')}` e `dias[...]='BATIDA'`. Sem dois donos, não há reconciliação; refazer cenário C11 sem porta para continuar testando divergência (`app/escala/tests/test_f3_beco1_folga_na_lista.py:262`; `app/escala/services/cadastro_realidade.py:988-989`). |
| `test_RED_contraex1_chamado_da_celula_na_fila_sem_pergunta_NAO_e_porta` | `app/escala/tests/test_f3_beco1_folga_na_lista.py:180` | L | Item 1: manter rota/fila/verbos, mudar `fio` para `colab` pela pergunta pendente sob a outra âncora; C11 continua `[]` (`app/chamados/services/tarefa_do_dia.py:43-44`; `app/escala/tests/test_f3_beco1_folga_na_lista.py:173-175`). |
| `test_RED_contraex2_ancora_viva_fora_da_fila_nao_e_porta` | `app/escala/tests/test_f3_beco1_folga_na_lista.py:204` | L | Item 1: perguntas pendentes têm porta do app mesmo sob terminal/registrado; C11 novo `[]` (`app/escala/tests/test_f3_beco1_folga_na_lista.py:193-198`; `app/chamados/services/tarefa_do_dia.py:43-44`). |
| `test_RED_dia_da_competencia_aberta_vai_a_lista_com_dono_CADASTRO` | `app/escala/tests/test_f3_beco1_folga_na_lista.py:88` | L | Item 1: dia com porta do app não é C11; novo `c11=[]`, sem dono C11 publicado. Refazer cenário sem porta para testar `CADASTRO` (`app/escala/services/cadastro_realidade.py:469,512`). |

**B: nenhum neste recorte. Contagem: L=13, F=0, M=0, B=0, ?=0** — enumeração das linhas da tabela acima. [não verificado]

## 14/11 16:53 · sessao 870e29998817 · E3-V10-DEGRAU-PUB · verificador · origem=Code

**PROCEDENTE: há perda de informação de fila na Folha, rótulo administrativo insuficiente e uma exceção que C13 aceita apesar do “sse” anunciado.** Não encontrei quebra de vocabulário por `porta_acao` nos contadores alterados. (`app/folha/fora_da_folha.py:244`, `app/folha/fora_da_folha.py:46`, `app/chamados/tests/test_e3_degrau_dono_e_porta.py:125`, `app/chamados/services/furo_sem_canal.py:117`, `app/chamados/services/vigia_fio.py:577`)

A identidade criptográfica da cópia e a comparação Git contra `52110c313` ficaram **[nao verificado]**: a pasta entregue não contém `.git`. Contraexemplo nominal de produção também fica **[nao verificado]**; os anexos fornecidos não apresentam uma linha real com todos os insumos necessários.

**1. Escritores, leitores e mudança de baldes**

O escritor dos quatro campos **da tarefa** é `_portas_da_tarefa`: constrói `verbos`, `fila`, escolhe `fio` e publica `rota`. `porta_da_tarefa` apenas projeta esses campos quando a rota existe; caso contrário, devolve `None`, descartando a estrutura inteira na projeção. (`app/chamados/services/tarefa_do_dia.py:596`, `app/chamados/services/tarefa_do_dia.py:597`, `app/chamados/services/tarefa_do_dia.py:610`, `app/chamados/services/tarefa_do_dia.py:622`, `app/chamados/services/tarefa_do_dia.py:632`)

| Leitor executável | Com `porta_acao` | Com `None`, embora reste `cobrar` na fila |
|---|---|---|
| Folha: `_porta`, `_fio_do_dia`, `fora_da_folha` | Lê `fio=admin`; soma em admin/grupo A. | Converte em `nada`; soma em nada/grupo D. Não perde o dia do universo. (`app/folha/fora_da_folha.py:70`, `app/folha/fora_da_folha.py:85`, `app/folha/fora_da_folha.py:93`, `app/folha/fora_da_folha.py:195`) |
| Folha: `o_que_segura` | Usa fio admin e seu texto fixo. | Publica `chamado=[]`, mesmo que a tarefa conserve fila residual. **PROCEDENTE: perda do dado.** (`app/folha/fora_da_folha.py:239`, `app/folha/fora_da_folha.py:244`, `app/folha/fora_da_folha.py:247`) |
| Furo: `medir` | Soma em `com_porta_acao`; exclui de `total`. | Inclui em `sem_canal/total`; conserva a leitura de `nao_sei`. (`app/chamados/services/furo_sem_canal.py:98`, `app/chamados/services/furo_sem_canal.py:100`, `app/chamados/services/furo_sem_canal.py:104`, `app/chamados/services/furo_sem_canal.py:117`) |
| Vigia: `censo_alcance` | A chave dinâmica existe; soma rota, papel admin e contador de exigência do admin. | Passa ao teto temporal, aos quatro baldes legais ou às causas a–e de fio morto. **Não significa automaticamente `fio_morto_alcance`.** (`app/chamados/services/vigia_fio.py:577`, `app/chamados/services/vigia_fio.py:662`, `app/chamados/services/vigia_fio.py:666`, `app/chamados/services/vigia_fio.py:672`, `app/chamados/services/vigia_fio.py:714`) |
| Beco: `beco_em_lote` | Presença de porta exclui o dia. | Torna o dia candidato, ainda condicionado a perguntas vivas, âncoras e demais critérios do beco. (`app/escala/services/cadastro_realidade.py:468`, `app/escala/services/cadastro_realidade.py:469`, `app/escala/services/cadastro_realidade.py:474`) |
| Pauta: `dias_sem_fio` | Admin é excluído. | Um dia `discordante` passa a ser elegível ao universo de pauta. (`app/ponto/services/pautas_dos_discordantes.py:72`, `app/ponto/services/pautas_dos_discordantes.py:81`, `app/ponto/services/pautas_dos_discordantes.py:83`) |
| `diff_e3_sombra` | Compara valores sem catálogo fechado de rotas; lê os quatro campos. | Projeta fio nada, fila vazia e ausência de porta para seus consumidores simulados. (`app/chamados/management/commands/diff_e3_sombra.py:26`, `app/chamados/management/commands/diff_e3_sombra.py:49`, `app/chamados/management/commands/diff_e3_sombra.py:56`, `app/chamados/management/commands/diff_e3_sombra.py:143`) |
| `lavrar_furo_sem_canal` | Imprime o contador novo; pode gravar o resultado calculado pelo serviço. | Imprime/grava o novo `total`; não reinterpreta a tarefa. (`app/chamados/management/commands/lavrar_furo_sem_canal.py:33`, `app/chamados/management/commands/lavrar_furo_sem_canal.py:37`, `app/chamados/management/commands/lavrar_furo_sem_canal.py:50`) |

**NAO PROCEDENTE — `KeyError` provocado por `porta_acao` nesses leitores.** A Folha indexa pelo fio admin, não pela rota; o vigia inicializa `alcance_porta_acao`; o furo enumera essa rota. (`app/folha/fora_da_folha.py:85`, `app/folha/fora_da_folha.py:245`, `app/chamados/services/vigia_fio.py:577`, `app/chamados/services/furo_sem_canal.py:117`)

**PROCEDENTE — rótulo administrativo insuficiente.** A tarefa pode publicar `decidir_ausencia/admin` sem chamado, obtendo `porta_acao`, enquanto `DESTRAVA['admin']` fala somente em validar/recusar o que o painel põe em Validar. O retrato reutiliza esse texto. Ocorrência nominal: **[nao verificado]**. (`app/chamados/services/tarefa_do_dia.py:303`, `app/chamados/services/tarefa_do_dia.py:621`, `app/folha/fora_da_folha.py:46`, `app/folha/retrato_folha.py:1069`)

**2. Segundo escritor fora da porta**

**PROCEDENTE — existe outro escritor literal.** C17 publica seu próprio `{rota, fila, verbos, fio}`, limitado a `porta_fila/porta_pergunta`. Com residual `cobrar`, pode publicar porta e fio colab onde a tarefa atual publica `None`. (`app/chamados/juizes.py:1650`, `app/chamados/juizes.py:1699`, `app/chamados/juizes.py:1709`, `app/chamados/services/tarefa_do_dia.py:606`, `app/chamados/services/tarefa_do_dia.py:616`)

**NAO PROCEDENTE — leitor atual confundindo as duas fontes.** O chamador remanescente é o comparador: chama C17 como `atual`, a tarefa como candidata e mantém as assinaturas separadas. Essa fronteira está declarada no registro e cobrada pela catraca de chamadores. (`app/chamados/management/commands/diff_e3_sombra.py:127`, `app/chamados/management/commands/diff_e3_sombra.py:129`, `app/chamados/management/commands/diff_e3_sombra.py:141`, `app/core/juizes.py:136`, `app/chamados/tests/test_e3_pendentes_app.py:145`)

**3. Regras próprias de dono/porta nos leitores**

**PROCEDENTE — o vigia conserva uma classificação adicional.** Depois de receber `None`, consulta `dono_fora_do_fio`, que compõe Resolver dia, ausência aberta, folga validada e lista do admin. Portanto, Folha e vigia podem dar destinos diferentes ao mesmo resultado sem porta. Essa cadeia não republica `rota`, mas é uma decisão adicional de balde e papel. (`app/chamados/services/vigia_fio.py:274`, `app/chamados/services/vigia_fio.py:306`, `app/chamados/services/vigia_fio.py:308`, `app/chamados/services/vigia_fio.py:311`, `app/chamados/services/vigia_fio.py:313`, `app/chamados/services/vigia_fio.py:672`)

**NAO PROCEDENTE — retrato recalculando alcance por ações/verbos.** Ele recebe o grupo da Folha, grava o fio correspondente e aplica o agrupamento de apresentação `chamado/ninguem → admin`. Seus botões são mapas próprios de navegação por fio e tipo. (`app/folha/retrato_folha.py:177`, `app/folha/retrato_folha.py:192`, `app/folha/retrato_folha.py:1158`, `app/folha/retrato_folha.py:1190`, `app/folha/retrato_folha.py:1015`)

**NAO PROCEDENTE — template da Folha derivando a rota.** O partial imprime os dados e botões preparados em Python. A API consultada usa `consulta_do_app` para a própria lista de perguntas, não a rota publicada pela tarefa. (`app/templates/folha/_retrato_o_que_fazer.html:33`, `app/api/views.py:993`, `app/api/views.py:2533`)

**4. Vocabulário de rotas**

**NAO PROCEDENTE — falta de `porta_acao` no vocabulário executável alterado.** Ela consta no publicador, no contador do furo, na inicialização e impressão do vigia, no command e na soma contratual do alcance. (`app/chamados/services/tarefa_do_dia.py:621`, `app/chamados/services/furo_sem_canal.py:117`, `app/chamados/services/vigia_fio.py:577`, `app/chamados/services/vigia_fio.py:730`, `app/chamados/management/commands/lavrar_furo_sem_canal.py:39`, `app/chamados/tests/test_o261e_f5_alcance.py:124`)

**NAO PROCEDENTE — exigir `porta_acao` em `PORTA_DO_FIO`.** Esse mapa enumera fios gravados, não rotas da tarefa. Admin tem `None` explícito; seu botão depende do tipo. Assim, publicar `porta_acao` não garante que o drawer ofereça um botão específico para aquela ação. (`app/folha/retrato_folha.py:175`, `app/folha/retrato_folha.py:975`, `app/folha/retrato_folha.py:986`, `app/folha/retrato_folha.py:1015`)

**PROCEDENTE — contratos antigos permanecem incompatíveis.** Há testes esperando colab sem fornecer pendentes do app e outro esperando que `cobrar` residual prevaleça sobre validar/decidir. Essas expectativas conflitam com os predicados atuais. (`app/chamados/tests/test_e3_portas_tarefa.py:409`, `app/chamados/tests/test_e3_portas_tarefa.py:451`, `app/chamados/tests/test_e3_portas_tarefa.py:473`, `app/chamados/tests/test_e3_portas_tarefa.py:485`, `app/chamados/tests/test_e3_portas_tarefa.py:503`, `app/chamados/services/tarefa_do_dia.py:598`, `app/chamados/services/tarefa_do_dia.py:606`)

**5. ESMERIL-SEMANTICA: `None` versus existência de chamado**

**PROCEDENTE na saída detalhada da Folha.** A tarefa conserva `fila/verbos` mesmo com rota `None`; a projeção elimina tudo; `o_que_segura` transforma a projeção ausente em fila vazia. É uma inferência operacional de “sem porta” para “nenhum chamado a publicar”. Quebra de link numa tela consumidora desse campo: **[nao verificado]**. (`app/chamados/services/tarefa_do_dia.py:622`, `app/chamados/services/tarefa_do_dia.py:632`, `app/folha/fora_da_folha.py:244`)

**NAO PROCEDENTE no vigia, beco e furo como pressuposição de inexistência de chamado.** O vigia continua examinando chamados vivos; o beco examina perguntas e âncoras; o furo mede presença de porta publicada, sem testar inexistência de chamados. (`app/chamados/services/vigia_fio.py:681`, `app/chamados/services/vigia_fio.py:683`, `app/escala/services/cadastro_realidade.py:471`, `app/escala/services/cadastro_realidade.py:474`, `app/chamados/services/furo_sem_canal.py:104`)

**6. Classes de reprovação**

| Classe | Avaliação |
|---|---|
| Segundo juiz duplicando autoridade registrada | **NAO PROCEDENTE como clone novo nesta publicação.** O publicador continua dentro da tarefa registrada; C17 permanece isolado como comparação histórica. (`app/core/juizes.py:297`, `app/core/juizes.py:299`, `app/chamados/management/commands/diff_e3_sombra.py:127`) |
| Segundo escritor fora da porta | **PROCEDENTE quanto à existência; NAO PROCEDENTE quanto à contaminação dos leitores.** É o C17 descrito no item 2. (`app/chamados/juizes.py:1709`, `app/core/juizes.py:136`) |
| Relógio fora da borda | **NAO PROCEDENTE no caminho alterado.** A carga repassa o mesmo `agora` à fila e ao app. Isso não equivale a certificar todos os caminhos legados do repositório. (`app/chamados/services/tarefa_do_dia.py:549`, `app/chamados/services/tarefa_do_dia.py:556`, `app/chamados/services/vigia_fio.py:330`) |
| Derivação paralela | **PROCEDENTE como classificação adicional do vigia**, com os limites do item 3; a Folha lê a tarefa e tem selo AST restringindo suas dependências de chamados. (`app/chamados/services/vigia_fio.py:672`, `app/folha/tests/test_l129_fora_da_folha_so_le_a_tarefa.py:16`, `app/folha/tests/test_l129_fora_da_folha_so_le_a_tarefa.py:53`) |
| Ordem de trava | **NAO PROCEDENTE como inversão demonstrada pelo trecho publicado.** A tarefa recebe o julgamento da tranca antes da publicação dos campos aditivos. Travas operacionais da entrega: **[nao verificado]**. (`app/chamados/services/tarefa_do_dia.py:541`, `app/chamados/services/tarefa_do_dia.py:544`, `app/chamados/services/tarefa_do_dia.py:557`) |
| Pouso antes do pré-requisito | **[nao verificado].** A cópia registra condições de pouso e um relato anterior de parada; isso não comprova a execução dos pré-requisitos finais nem um pouso posterior. (`app/docs/CORTES.md:401`, `app/docs/RELATO.md:4`) |
| Campo com dois significados | **PROCEDENTE como inconsistência do contrato documentado:** `None = sem dono`, mas o código e C13 aceitam dono sistema com `None`. Além disso, fila existente deixa de implicar projeção existente. (`app/chamados/services/tarefa_do_dia.py:40`, `app/chamados/services/tarefa_do_dia.py:621`, `app/chamados/tests/test_e3_degrau_dono_e_porta.py:125`) |

**NAO PROCEDENTE — C11/C12, contador e vigia sem mordida nos alvos específicos.** C11 verifica publicação/projeção/Folha; C12 verifica nada e nao_sei sem rota; o contador verifica ação direta, residual cobrar e soma; o vigia verifica contador, papel e texto. (`app/chamados/tests/test_e3_degrau_dono_e_porta.py:92`, `app/chamados/tests/test_e3_degrau_dono_e_porta.py:103`, `app/chamados/tests/test_e3_degrau_dono_e_porta.py:151`, `app/chamados/tests/test_e3_degrau_dono_e_porta.py:256`)

**PROCEDENTE — C13 não prova o alcance nem o “sse” geral.** Compara dois resultados do próprio juiz em seis exemplos e depois exige a exceção sistema/None. Não há oráculo independente de ação alcançável. (`app/chamados/tests/test_e3_degrau_dono_e_porta.py:113`, `app/chamados/tests/test_e3_degrau_dono_e_porta.py:115`, `app/chamados/tests/test_e3_degrau_dono_e_porta.py:122`, `app/chamados/tests/test_e3_degrau_dono_e_porta.py:125`)

Os mutantes novos exigem falha de assert sem erros/skips, mas não fazem controle verde antes de cada mutação; o executor v6 faz esse controle e recusa carimbar um teste já vermelho. (`app/chamados/tests/e3_degrau_mutantes.py:77`, `app/chamados/tests/e3_degrau_mutantes.py:89`, `app/chamados/tests/e3_portas_v6_mutantes.py:92`)

Execuções em memória reportaram: degrau **27 OK**, mutantes novos **22/22 mortos**, v6 sem banco **10 OK**; bateria antiga **26 testes, 6 falhas e 1 erro**, e mutantes v6 interrompidos antes do terceiro mutante por controle já vermelho. Saída persistida em arquivo: **[nao verificado]**. A incompatibilidade é localizável nas expectativas antigas acima e no helper que cria a carga com `__new__` sem inicializar `_pendentes_app`. (`app/chamados/tests/e3_v8_apoio.py:127`, `app/chamados/tests/e3_v8_apoio.py:146`, `app/chamados/services/tarefa_do_dia.py:551`)

**7. Contraexemplo à lei 16:16**

**Dia real contraditório: [nao verificado].** Não transformo comentários “33 dias”, “31 dias” ou “0 dias” dos testes em linhas observadas de produção. Esses números aparecem como comentários, sem os respectivos insumos nominais no dado fornecido. (`app/chamados/tests/test_e3_degrau_dono_e_porta.py:94`, `app/chamados/tests/test_e3_degrau_dono_e_porta.py:105`, `app/chamados/tests/test_e3_degrau_dono_e_porta.py:123`)

- **Sistema sem chamado — PROCEDENTE como contraexemplo ao contrato universal de C13:** o corpo real pode produzir `perguntar/sistema/emissor`; o publicador escolhe sistema e devolve rota `None`. Isso prova a exceção no código, mas não prova um dia real nem uma porta humana alcançável contrariando a ordem colab > admin. (`app/chamados/services/tarefa_do_dia.py:316`, `app/chamados/services/tarefa_do_dia.py:602`, `app/chamados/services/tarefa_do_dia.py:610`, `app/chamados/services/tarefa_do_dia.py:621`)
- **Colab com fila somente `cobrar` — NAO PROCEDENTE, por si só, como contradição:** se há responder exigível e pergunta pendente no app, existe porta do colab. A rota fica `porta_fila` pela classificação da fila, embora o alcance do dono tenha sido estabelecido pelo app. (`app/chamados/services/tarefa_do_dia.py:598`, `app/chamados/services/tarefa_do_dia.py:604`, `app/chamados/services/tarefa_do_dia.py:619`)
- **Ação direta admin mais residual cobrar — lacuna concreta de cobertura:** `decidir_ausencia/admin/None` recebe dono sem fila, mas perde-o quando existe apenas residual cobrar, pois `not portas` deixa de valer e o residual é excluído. Alcance efetivo dessa ação na tela e ocorrência real: **[nao verificado]**. (`app/chamados/services/tarefa_do_dia.py:303`, `app/chamados/services/tarefa_do_dia.py:603`, `app/chamados/services/tarefa_do_dia.py:606`)
- **Entregar documento pelo colab — outra exclusão explícita:** a tarefa produz essa ação, mas o degrau colab aceita somente `responder`; C10 exige nada/None nesse caso. A existência de caminho alcançável para entregar o documento, num dia real fornecido, permanece **[nao verificado]**. (`app/chamados/services/tarefa_do_dia.py:300`, `app/chamados/services/tarefa_do_dia.py:599`, `app/chamados/tests/test_e3_degrau_dono_e_porta.py:87`)

## 14/11 16:51 · sessao c28e1f3a7ed3 · - · - · origem=Ronald (tmux)

- **PROCEDENTE — C13 não sela o “sse” anunciado.** A propriedade compara `rota` com `fio`, sem testar alcance independente; os casos omitem `sistema`, e depois o teste exige expressamente `('sistema', None)`. Portanto aceita a exceção que a descrição universal nega. `app/chamados/tests/test_e3_degrau_dono_e_porta.py:113`, `:115`, `:122`, `:123`, `:125`. A publicação produz essa exceção por `.get(fio)` sem entrada `sistema`. `app/chamados/services/tarefa_do_dia.py:602`, `:610`, `:621`. Dia real dessa classe: **[nao verificado]**.

- **NAO PROCEDENTE — novos C11/C12 e mutantes específicos sem mordida.** C11 verifica publicação, projeção e leitura pela Folha; C12 verifica os dois estados sem dono e retorno `None`; o contador verifica ação direta e residual cobrar, inclusive soma do universo. `app/chamados/tests/test_e3_degrau_dono_e_porta.py:92`, `:103`, `:151`. Vigia verifica contador, papel e texto publicado; command verifica ambos os números. `:231`, `:256`. Executei `PYTHONDONTWRITEBYTECODE=1 python3 -m unittest chamados.tests.test_e3_degrau_dono_e_porta`: **27 OK**; `python3 -m chamados.tests.e3_degrau_mutantes --todos`: **22/22 mortos por assert, zero erros, RC0**. O executor exige failures sem errors/skips. `app/chamados/tests/e3_degrau_mutantes.py:72`, `:77`, `:82`, `:106`.

- **PROCEDENTE — bateria antiga não acompanha o contrato atual.** `python3 -m chamados.tests.e3_portas_regra`: **26 testes, 6 failures e 1 error, RC2**. Failures: [nao verificado]
  - `ResidualComAcaoTest.test_RED_A_na_fila_aparece_com_acao_sob_B`. `app/chamados/tests/test_e3_portas_tarefa.py:409`.
  - `ContraprovasPortasTest.test_cobrar_precede_validar_e_decidir_sem_depender_dos_donos`. `:473`.
  - `ContraprovasPortasTest.test_fio_normal_e_do_dono_da_acao_mesmo_com_verbo_validar`. `:451`.
  - `MatrizV8Test.test_v8_R1_colab_alcancavel_e_leitores`. `:485`.
  - `MatrizV8Test.test_v8_R2_validacao_e_resposta_alcancaveis_no_mesmo_dia`, dois subtests `porta_colab=101/202`. `:503`.
  
  Esses casos esperam colab sem fornecer `pendentes_app`, cujo default é vazio e agora participa da atribuição. `app/chamados/services/tarefa_do_dia.py:564`, `:598`. Error: `MatrizV8Test.test_v8_R10_ancora_fora_do_fato_sem_acao`; o helper instancia com `__new__`, sem `_pendentes_app`, acessado pela carga. `app/chamados/tests/test_e3_portas_tarefa.py:593`, `app/chamados/tests/e3_v8_apoio.py:127`, `:146`, `app/chamados/services/tarefa_do_dia.py:551`.

- **PROCEDENTE — execução completa dos mutantes v6 interrompida por baseline vermelho.** `python3 -m chamados.tests.e3_portas_v6_mutantes --mutante todos` mata os dois primeiros e aborta em `residual_A_some` **antes da mutação**, porque seu teste já falha. O executor recusa carimbar isso como morto. `app/chamados/tests/e3_portas_v6_mutantes.py:30`, `:90`, `:92`, `:98`. Separadamente, `python3 -m unittest chamados.tests.test_e3_portas_v6_sem_banco`: **10 OK**.

- **Contraexemplos sintéticos confirmados, ocorrência real [nao verificado]:**
  - `perguntar/sistema/emissor`, fila vazia → `fio=sistema, rota=None`. O corpo real produz ações desse tipo. `app/chamados/services/tarefa_do_dia.py:312`, `:316`, `:621`.
  - Colab com responder pendente no app, única fila `cobrar` → `fio=colab, rota=porta_fila, fila=(101,), verbos={101:'cobrar'}`. Há dono alcançável pelo app; o nome da rota é determinado pela existência da fila, sem testar se ela conduz à ação vencedora. `:598`, `:604`, `:618`, `:619`.
  - `entregar_documento/colaborador/None`, fila vazia → `nada/None`: a ação existe no corpo real, mas o filtro de colab aceita somente responder. `:298`, `:300`, `:598`, `:599`; o teste C10 exige precisamente esse resultado. `app/chamados/tests/test_e3_degrau_dono_e_porta.py:87`.
  - `decidir_ausencia/admin/None` com residual cobrar → `nada/None`; sem fila → `admin/porta_acao`. A presença do residual bloqueia a ação direta pelo predicado de alcance; esse predicado antecede o diff de publicação. `app/chamados/services/tarefa_do_dia.py:302`, `:303`, `:602`, `:603`, `:606`, `:621`.

- **Pouso/pré-requisitos: [nao verificado].** A cópia encerra `CORTES.md` no aval de 15:37 e o topo do relato mantém “PAREI” com condições anteriores; não comprova matriz final conferida, remedida final, suite integral ou pouso. `app/docs/CORTES.md:401`, `app/docs/RELATO.md:4`. O anexo fornecido atualiza a lei aplicável; a documentação da cópia não prova sua execução.

## 14/11 16:51 · sessao d54d687beb51 · - · - · origem=Ronald (tmux)

- **PROCEDENTE: há segundo escritor literal de `rota`, preservado como comparação histórica.** C17 devolve `porta_fila/porta_pergunta` e fio próprio em `app/chamados/juizes.py:1709`. O único chamador encontrado em produção é o lado `atual` de `diff_e3_sombra`; a candidata vem separadamente da tarefa, e as assinaturas distinguem `C17`/`candidata`: `app/chamados/management/commands/diff_e3_sombra.py:127`, `:129`, `:141`. **NAO PROCEDENTE: contaminação dos leitores atuais pelo C17.** O registro declara essa fronteira em `app/core/juizes.py:132`; o selo exige um único chamador nomeado em `app/chamados/tests/test_e3_pendentes_app.py:145`.

- **NAO PROCEDENTE: vocabulário runtime esqueceu `porta_acao`.** Está no publicador `app/chamados/services/tarefa_do_dia.py:621`, no contador furo `app/chamados/services/furo_sem_canal.py:117`, na inicialização e impressão do vigia `app/chamados/services/vigia_fio.py:577`, `:730`, e na soma contratual `app/chamados/tests/test_o261e_f5_alcance.py:124`. O C17 mantém suas duas rotas por contrato histórico `app/chamados/tests/test_e3_pendentes_app.py:171`. `PORTA_DO_FIO` enumera fios, não rotas; adicionar `porta_acao` ali não corresponde à chave desse mapa: `app/folha/retrato_folha.py:974`.

- **NAO PROCEDENTE no caminho alterado: relógio novo fora da borda.** A carga repassa `self.agora` tanto à alimentação quanto ao app em `app/chamados/services/tarefa_do_dia.py:549`, `:556`; a alimentação repassa-o ao painel e à partição em `app/chamados/juizes.py:1570`, `:1573`. Defaults internos de relógio existem, mas são condicionais à ausência de `agora`: `app/chamados/services/verbos.py:35`, `app/chamados/services/painel.py:190`.

- **PROCEDENTE: C13 não sela a propriedade universal anunciada.** O laço verifica apenas seis exemplos, e depois afirma explicitamente o caso `('sistema', None)`: `app/chamados/tests/test_e3_degrau_dono_e_porta.py:113`, `:125`. Ações de sistema sem chamado podem nascer do corpo real em `app/chamados/services/tarefa_do_dia.py:312`, `:316`; sistema recebe dono por `:601`, mas não rota por `:621`. Dia nominal de produção dessa classe: **[nao verificado]**.

- **Possível rótulo incorreto já identificável por código:** `decidir_ausencia` é ação admin em `app/chamados/services/tarefa_do_dia.py:303`; pode publicar `porta_acao` por `:621`, enquanto a descrição admin da Folha fala apenas em validar/recusar o que o painel põe em Validar: `app/folha/fora_da_folha.py:46`. Ocorrência nominal de produção: **[nao verificado]**.

Verificação executada sem bytecode: CatracaC17Test passou 7 testes; selo AST da Folha passou 3 testes por extração dos corpos; suíte degrau passou 27 testes. Corpos respectivos: `app/chamados/tests/test_e3_pendentes_app.py:118`, `app/folha/tests/test_l129_fora_da_folha_so_le_a_tarefa.py:52`, `app/chamados/tests/test_e3_degrau_dono_e_porta.py:40`; saída persistida em arquivo: **[nao verificado]**.

## 14/11 16:50 · sessao 89574cb0d293 · - · - · origem=Ronald (tmux)

Inventário de leitores em código executável:

| Leitor | Campos/efeito de `porta_acao` | Efeito de `None` com fila residual |
|---|---|---|
| `folha/fora_da_folha.py:_porta/_fio_da_porta` | Lê a projeção e usa `fio='admin'`; a rota não indexa mapa local. `app/folha/fora_da_folha.py:67`, `app/folha/fora_da_folha.py:82` | Converte projeção ausente em `FIO_SEM_PORTA`; cai no fio `nada`, grupo D e contador `dias_por_fio['nada']`. `app/folha/fora_da_folha.py:85`, `app/folha/fora_da_folha.py:93`, `app/folha/fora_da_folha.py:195` |
| `folha/fora_da_folha.py:o_que_segura` | Usa fio admin e rótulo fixo de validar. `app/folha/fora_da_folha.py:239`, `app/folha/fora_da_folha.py:245`, `app/folha/fora_da_folha.py:46` | **Perde a fila preservada na tarefa:** lê fila exclusivamente da projeção; `None` produz `chamado=[]`. `app/folha/fora_da_folha.py:244`, `app/folha/fora_da_folha.py:247` |
| `furo_sem_canal.medir` | Inclui em `com_porta_acao`, retirando de `total`; partição contém as quatro rotas. `app/chamados/services/furo_sem_canal.py:97`, `app/chamados/services/furo_sem_canal.py:104`, `app/chamados/services/furo_sem_canal.py:114` | Vai para `sem_canal` mesmo que haja chamado na tarefa. `app/chamados/services/furo_sem_canal.py:98`, `app/chamados/services/furo_sem_canal.py:104` |
| `vigia_fio.censo_alcance` | Chave dinâmica existe; soma `alcance_porta_acao`, registra papel admin e publica log. `app/chamados/services/vigia_fio.py:577`, `app/chamados/services/vigia_fio.py:662`, `app/chamados/services/vigia_fio.py:730` | Vai para teto temporal, quatro baldes legais ou causas a–e; chamado vivo ainda é consultado independentemente da projeção. `app/chamados/services/vigia_fio.py:666`, `app/chamados/services/vigia_fio.py:672`, `app/chamados/services/vigia_fio.py:683` |
| `cadastro_realidade.beco_em_lote` | Presença da projeção exclui do beco. `app/escala/services/cadastro_realidade.py:469` | Pode entrar na investigação do beco, mas continua exigindo perguntas vivas e considera suas âncoras; não pressupõe ausência de chamados. `app/escala/services/cadastro_realidade.py:468`, `app/escala/services/cadastro_realidade.py:471`, `app/escala/services/cadastro_realidade.py:474` |
| `pautas_dos_discordantes.dias_sem_fio` | Usa `_fio_do_dia`; admin é excluído. `app/ponto/services/pautas_dos_discordantes.py:64`, `app/ponto/services/pautas_dos_discordantes.py:81` | Discordante com residual cobrar passa a ser elegível ao universo de pauta, pois o critério é fio sem porta. `app/ponto/services/pautas_dos_discordantes.py:72`, `app/ponto/services/pautas_dos_discordantes.py:81` |
| `diff_e3_sombra` | Lê todos os quatro campos, sem vocabulário fechado dos valores; compara C17/candidata separadamente. `app/chamados/management/commands/diff_e3_sombra.py:26`, `app/chamados/management/commands/diff_e3_sombra.py:46`, `app/chamados/management/commands/diff_e3_sombra.py:141` | Projeta fio nada, fila vazia, sem porta/beco/pauta; isso reflete suas próprias projeções, sem executar os consumidores. `app/chamados/management/commands/diff_e3_sombra.py:47`, `app/chamados/management/commands/diff_e3_sombra.py:49` |
| `lavrar_furo_sem_canal` | Imprime contador novo e grava todo detalhe no snapshot quando aplicado. `app/chamados/management/commands/lavrar_furo_sem_canal.py:37`, `app/chamados/management/commands/lavrar_furo_sem_canal.py:50` | Imprime/grava o `total` calculado pelo serviço. `app/chamados/management/commands/lavrar_furo_sem_canal.py:33`, `app/chamados/management/commands/lavrar_furo_sem_canal.py:52` |

Achados:

- **PROCEDENTE — outro escritor de rota existe:** C17 produz sempre `porta_fila/porta_pergunta` quando possui verbos e deriva fio dos verbos; pode divergir da tarefa nos residuais cobrar. Seu único chamador executável encontrado fora dos testes é o comparador, que distingue explicitamente C17 da candidata. Não constitui consumidor confundindo as fontes. `app/chamados/juizes.py:1699`, `app/chamados/juizes.py:1709`, `app/chamados/management/commands/diff_e3_sombra.py:126`, `app/core/juizes.py:136`.

- **PROCEDENTE — perda do dado de chamado no retorno Folha:** `porta_da_tarefa(None)` elimina a estrutura inteira, e `o_que_segura` tira `fila` dessa projeção, não da tarefa. Em residual cobrar, retorna `chamado=[]` apesar da fila original. Link quebrado numa tela consumidora desse campo: **[nao verificado]**. `app/chamados/services/tarefa_do_dia.py:622`, `app/chamados/services/tarefa_do_dia.py:632`, `app/folha/fora_da_folha.py:244`.

- **NÃO PROCEDENTE — `None` implica nenhum chamado no vigia/beco/furo:** vigia consulta os vivos da célula separadamente e distingue seu véu; beco consulta perguntas e âncoras separadamente; furo só pergunta presença de porta, sem afirmar inexistência de chamado. `app/chamados/services/vigia_fio.py:618`, `app/chamados/services/vigia_fio.py:681`, `app/escala/services/cadastro_realidade.py:468`, `app/chamados/services/furo_sem_canal.py:104`.

- **NÃO PROCEDENTE — `PORTA_DO_FIO` precisa enumerar `porta_acao`:** esse mapa enumera fios gravados, não rotas da tarefa. Admin possui `None` explícito e seu botão depende do tipo; o mapa não recebe `porta['rota']`. `app/folha/retrato_folha.py:175`, `app/folha/retrato_folha.py:974`, `app/folha/retrato_folha.py:1015`.

- **PROCEDENTE como limitação preexistente de apresentação:** novo `porta_acao` chega à Folha como admin, mas o texto admin promete validar/recusar o que o painel põe em Validar; o botão por fio admin é `None`. O drawer pode ficar sem botão conforme o tipo, embora o juiz publique porta. Não há evidência de dia real nessa combinação. `app/folha/fora_da_folha.py:46`, `app/folha/retrato_folha.py:975`, `app/folha/retrato_folha.py:990`, `app/folha/retrato_folha.py:1015`, **[nao verificado]**.

- **PROCEDENTE — regra de dono fora da rota publicada existe no vigia:** após projeção None, `dono_fora_do_fio` compõe Resolver dia, ausência, folga validada e lista do admin; são quatro baldes próprios, não republicação da rota. O nome dos baldes e a ordem estão materializados no código. `app/chamados/services/vigia_fio.py:274`, `app/chamados/services/vigia_fio.py:306`, `app/chamados/services/vigia_fio.py:672`.

- **NÃO PROCEDENTE — retrato recalcula dono por ações/verbos:** recebe o grupo de `fora_da_folha`, grava fio por tradução A/B/C/D, e a UI agrupa chamado/ninguém em admin e trata hoje. Há regra de apresentação, não nova leitura de ações alcançáveis. `app/folha/retrato_folha.py:177`, `app/folha/retrato_folha.py:192`, `app/folha/retrato_folha.py:368`, `app/folha/retrato_folha.py:477`, `app/folha/retrato_folha.py:1158`, `app/folha/retrato_folha.py:1190`.

- **NÃO PROCEDENTE — templates Folha derivam rota:** o partial imprime linhas e botões já produzidos pelo Python. API consulta o juiz do app diretamente para sua própria lista; não lê a rota da tarefa. `app/templates/folha/_retrato_o_que_fazer.html:33`, `app/api/views.py:993`, `app/api/views.py:2533`.

Limite: buscas foram com `rg`, pois a cópia não contém `.git`; identidade efetiva do hash entregue não foi verificável via Git. **[nao verificado]**

## 14/11 16:39 · sessao 5f078705d999 · JUSTIFICATIVA-PAGA · verificador · origem=Code

1. **PROCEDENTE — o período do motor não chega ao abono gravado.**

O motor soma `minutos_justificados`, mas o fechamento grava `minutos_abonados` por outra derivação: `por_dia_da_grade`. Ela só abona dias classificados como `ausencia`, por `previsto − realizado`; ignora `min_justificados`. A justificativa mantém o dia como `trabalho`. (`app/ponto/motor_calculo_v2.py:2633`; `app/ponto/services/fechamento.py:1065`; `app/ponto/services/dia_pago.py:395`; `app/escala/utils.py:1257`)

Contraexemplo sintético: dia `trabalho`, previsto 720, realizado 0, `min_justificados=690`: a função real `por_dia_da_grade`, executada isoladamente em memória, devolve **abono vazio**. Esse mapa alimenta tanto o fechamento quanto o `DiaPago`. (`app/ponto/services/dia_pago.py:387`; `app/ponto/services/fechamento.py:1067`; `app/ponto/services/fechamento.py:1217`)

Na competência, o resumo do PDF/espelho recebe `minutos_abonados/60` do fechamento ou da lavratura, sobrescrevendo o número apurado pelo motor. Portanto, podem existir **11,5 h no motor e 0 h no resumo**. (`app/relatorios/pdf_espelho.py:480`; `app/relatorios/cartao_pela_celula.py:118`; `app/relatorios/cartao_pela_celula.py:186`; `app/relatorios/cartao_pela_celula.py:346`)

| Campo | Quem escreve/alimenta | Quem lê |
|---|---|---|
| `minutos_justificados` | Inicialização: `app/ponto/motor_calculo_v2.py:813`; fechamento normal/órfão: `app/ponto/services/fechamento.py:755`, `:831`; espelho: `app/ponto/services/espelho.py:1134`; PDF: `app/relatorios/pdf_espelho.py:407` | Conta mensal: `app/ponto/motor_calculo_v2.py:2628`; contrato de memo: `app/ponto/calculador/nucleo.py:424` |
| `min_justificados` | Efeito: `app/ponto/services/efeito_ausencia.py:47`, `:67`; grade: `app/escala/utils.py:1276`; fechamento monta mapa equivalente: `app/ponto/services/fechamento.py:667` | Alimentação do espelho/PDF: `app/ponto/services/espelho.py:1134`; `app/relatorios/pdf_espelho.py:407`. A derivação do abono gravado não lê essa chave: `app/ponto/services/dia_pago.py:387` |
| `dias_com_ausencia_inteira` | Fonte: `app/ponto/turnos.py:2829`; inicialização: `app/ponto/motor_calculo_v2.py:808`; fechamento: `app/ponto/services/fechamento.py:754`, `:830`; espelho/PDF: `app/ponto/services/espelho.py:1133`; `app/relatorios/pdf_espelho.py:406` | Teto de pontualidade: `app/ponto/motor_calculo_v2.py:1090`; calculador: `app/ponto/calculador/regras.py:120`, `app/ponto/calculador/alimentacao.py:532`; envio ao oráculo: `app/ponto/services/fechamento.py:1281` |
| `horas_abono` | Inicialização: `app/ponto/motor_calculo_v2.py:292`; conta: `:2631`, `:2633`; composição de fatias: `app/ponto/services/fechamento.py:99` | PDF: `app/relatorios/pdf_espelho.py:480`; resumo substitui pela outra fonte: `app/relatorios/cartao_pela_celula.py:118`, `:186` |
| `dias_abono` | Inicialização: `app/ponto/motor_calculo_v2.py:284`; conta: `:2630`; composição: `app/ponto/services/fechamento.py:99`; resumos: `app/ponto/services/espelho.py:1253`, `app/relatorios/pdf_espelho.py:479` | Badge PDF: `app/relatorios/pdf_espelho.py:787`; ramo de lavratura substitui por contagem de dias com abono positivo: `app/relatorios/cartao_pela_celula.py:190` |
| `resumo_dias_abono` | Não localizado pela busca literal na cópia: [nao verificado] | [nao verificado] |

**PROCEDENTE — chamador de sombra ficou sem alimentação.** `diff_espelho_alimentacao` passa justificadas e ausência inteira, mas não passa o mapa novo, inclusive no degrau denominado “cura completa”. O memo permanece vazio: aplica o comportamento anterior — previsto inteiro quando o dia entra em `_abono`, nenhum crédito parcial quando não entra. (`app/ponto/management/commands/diff_espelho_alimentacao.py:41`; `:120`; `app/ponto/motor_calculo_v2.py:813`; `:2611`; `:2628`)

Outros caminhos fora do diff:

- `diff_extrato_parcial` consulta justificativas diretamente e calcula sem o memo novo; `esmeril_espelho` também não o alimenta, mas usa o resultado para assinaturas de pontualidade/HE. Divergência de **pagamento** nesses dois caminhos: [nao verificado]. (`app/relatorios/management/commands/diff_extrato_parcial.py:42`; `:67`; `app/ponto/services/esmeril_espelho.py:160`; `:170`)
- TXT Domínio monta eventos sem ler `horas_abono` ou o mapa novo; a rubrica 8932 deriva de tipos de `Ausencia`. Igualdade entre período justificado e pagamento no Domínio: [nao verificado]. (`app/folha/export.py:439`; `app/folha/export.py:201`)
- Prontidão lê `minutos_abonados`; seu fallback ainda soma o previsto inteiro dos dias de ausência, constituindo outra derivação de abono. (`app/folha/export.py:1078`; `app/folha/export.py:1100`)
- Tela e app recebem o resumo do espelho; Haiku consulta classificação de exportação e cópias dos TXT. O holerite é recebido por upload. Esses caminhos não comprovam que o período novo foi pago. (`app/ponto/views.py:161`; `app/api/views.py:1354`; `app/api/views_mensageria.py:283`; `app/folha/haiku_export.py:79`; `app/holerite/models.py:6`; pagamento efetivo [nao verificado])

2. **PROCEDENTE — a fonte perde a segunda pausa.**

A fonte desempacota quatro marcos e reconstrói apenas `hi/hii/hfi/hf`. O medidor desconta somente pausas presentes nesse mapa. O previsto, entretanto, consulta e desconta todas as pausas do dia. (`app/ponto/services/efeito_ausencia.py:156`; `app/ponto/services/efeito_ausencia.py:163`; `app/ponto/turnos.py:2786`; `app/escala/utils.py:2310`; `app/escala/utils.py:2324`)

Contraexemplo confirmado em memória: jornada 07–19, pausas 10–11:25 e 14–15:25, entrada 07:00 e saída justificada: **635 minutos pela cadeia construída pela fonte; 550 minutos com ambas as pausas**. O motor aceita o valor sem teto pelo previsto. (`app/ponto/services/efeito_ausencia.py:158`; `app/ponto/turnos.py:2791`; `app/ponto/motor_calculo_v2.py:2633`)

Quanto às demais perguntas semânticas:

- A chave é **dia do turno**, `Justificativa.data_turno`; a fonte recusa usar a âncora de turno com outra data. A cadeia encadeia a saída noturna no dia seguinte. (`app/ponto/services/efeito_ausencia.py:107`; `:165`; `:163`; `app/escala/utils.py:345`)
- O Base compara pelo dia da jornada, mas o Comercial refaz DSR pela **data civil da entrada**. Essa derivação paralela preexistente continua alcançável quando a justificativa deixa de integrar `datas_justificadas`. (`app/ponto/motor_calculo_v2.py:2581`; `:3473`; `:3477`; `:3490`)
- Os caminhos identificados convertem minutos para horas por `/60`; não encontrei erro de fator nesses sítios. (`app/ponto/motor_calculo_v2.py:2634`; `app/relatorios/pdf_espelho.py:480`; `app/relatorios/cartao_pela_celula.py:118`)
- A seleção de dias inclui as duas datas; a duração é diferença entre instantes, sem minuto adicional; toque entre extremidades não configura sobreposição, pois o teste usa `<`. (`app/ponto/services/efeito_ausencia.py:100`; `app/ponto/turnos.py:2764`; `app/ponto/turnos.py:2783`)

3. **PROCEDENTE — duas âncoras do mesmo turno podem pagar a mesma cauda duas vezes.**

A agregação une os rótulos cobertos, mas **soma os minutos**, sem deduplicar trechos. Ambas as justificativas usam a última entrada do turno para medir a saída coberta. (`app/ponto/services/efeito_ausencia.py:133`; `:134`; `:179`)

Contraexemplo: turno `E07:30/S11:00/E12:00`, fim previsto 19:00; uma justificativa aprovada ancorada na primeira entrada e outra na segunda: ambas medem 12–19, produzindo **420 + 420 = 840 minutos**. A unicidade impede repetir a mesma âncora, mas permite âncoras distintas; a unicidade por dia restringe apenas pendentes. (`app/ponto/services/efeito_ausencia.py:168`; `:179`; `app/ponto/turnos.py:2738`; `app/ponto/models.py:187`; `:189`)

O motor soma esses 840 sem deduplicação nem limite pelo previsto. Ocorrência produtiva: [nao verificado]. (`app/ponto/motor_calculo_v2.py:2633`)

Um dia em `_mj` fora de `_abono` por ter trabalho **não desaparece**: a soma nova independe de `_abono`. Já dia fora dos obrigatórios — inclusive folga corretamente excluída dos previstos — não recebe esse crédito. (`app/ponto/motor_calculo_v2.py:2611`; `:2601`; `:2634`)

**PROCEDENTE — precedência incompatível com ausência inteira.** O efeito mantém o mapa parcial mesmo quando uma ausência `ABONA` cobre o mesmo dia; a grade dá precedência à ausência, enquanto o motor continua usando `_mj` para excluir o abono inteiro. (`app/ponto/services/efeito_ausencia.py:47`; `:61`; `app/ponto/services/fechamento.py:667`; `:700`; `app/escala/utils.py:1207`; `app/ponto/motor_calculo_v2.py:2629`)

A troca não altera diretamente o predicado de DSR do Base; altera sua alimentação de justificadas. O Comercial conserva a rederivação por data civil descrita no item 2. **Zero falta inteira adicional e zero semana perdida no universo informado: [nao verificado].** (`app/ponto/services/efeito_ausencia.py:46`; `app/ponto/motor_calculo_v2.py:2605`; `:3477`)

4. **nada quanto a demonstrar que os dez dias viraram falta inteira.**

Sem âncora, papel errado e turno fechado retornam motivo de conservação; célula sem marcos também conserva. A fonte transforma esses motivos em `cobre=None`. (`app/ponto/turnos.py:2694`; `:2700`; `:2703`; `app/ponto/services/efeito_ausencia.py:154`; `:129`)

O caminho para **vazio sem `conserva`** vem depois: o juiz cobre o marco, o filtro encontra cruzamento com um turno fechado e remove a cobertura; a função retorna vazio com motivo `None`. O efeito então exclui o dia tanto de `justificadas` quanto de `min_justificados`. (`app/ponto/services/efeito_ausencia.py:176`; `:182`; `:183`; `app/ponto/turnos.py:2764`; `app/ponto/services/efeito_ausencia.py:46`)

Esse mecanismo tem caso sintético e referência a col451 no teste; a reprodução individual de col206 31/08 e col551 27/10 não está demonstrada pelo dado fornecido. (`app/ponto/tests/test_justificativa_paga_o_periodo.py:191`; `:383`; demais resultados [nao verificado])

Com vazio, o dia recebe **nenhum crédito da justificativa**; conserva o trabalho que o motor calcular. Vazio não implica automaticamente falta: o Base inclui períodos abertos em `datas_trabalho`, sem exigir minutos positivos. Os valores individuais dos dez dias permanecem [nao verificado]. (`app/ponto/motor_calculo_v2.py:2499`; `:2581`; `:2605`; `:2634`)

5. **nada de falha nova de relógio demonstrada.**

A fonte usa datas explícitas, ordenação por `data_turno, pk` e timestamps explícitos nas conversões locais. O previsto consulta escala, marcos e pausas, sem depender das batidas nesse cálculo. (`app/ponto/services/efeito_ausencia.py:98`; `:122`; `:179`; `app/escala/utils.py:2288`; `:2310`)

Os testes repetem o juiz puro e a consulta da fonte; não demonstram duas execuções completas do fechamento. **Idempotência integral do recálculo: [nao verificado].** (`app/ponto/tests/test_justificativa_paga_o_periodo.py:134`; `:320`)

6. **PROCEDENTE — há mordida localizada, mas falta prova do percurso até o gravado.**

O teste principal exige `horas_abono=11.5`; essa asserção distingue a conta nova da antiga. O controle de dia inteiro passaria com o motor velho, pois exige apenas um dia abonado e horas positivas. (`app/ponto/tests/test_justificativa_paga_o_periodo.py:255`; `:258`; `:262`; `:265`)

Os testes da fonte conferem seus próprios minutos e a grade; as propriedades somam trabalho com crédito da fonte, sem conferir o abono materializado que apresenta a falha do item 1. (`app/ponto/tests/test_justificativa_paga_o_periodo.py:320`; `:325`; `:338`; `:350`; `app/ponto/services/fechamento.py:1067`)

- `test_cobparc_marco_no_periodo.py`: **só atualiza a fixture**, acrescentando mock da fonte sem banco; mantém asserções de dispensa, ausência de hora fabricada e preservação da batida. (`app/ponto/tests/test_cobparc_marco_no_periodo.py:60`; `:75`)
- `test_s5b_porta_unica.py`: **só amplia lista e fixture** com o memo; preserva o teste que remove cada atributo e exige exceção nominal. Contudo, aceita `{}` e não detecta o chamador que esqueceu de alimentar o mapa. (`app/ponto/tests/test_s5b_porta_unica.py:989`; `:1000`; `:1006`; `:1014`)
- O caso de papel errado é sintético; reprodução dos três dias produtivos exigidos no aval: [nao verificado]. (`app/ponto/tests/test_justificativa_paga_o_periodo.py:429`)
- RED histórico e suíte verde efetivamente executada: [nao verificado]. [nao verificado]

7. **PROCEDENTE — derivação paralela, selo insuficiente e campo com dois significados.**

A cadeia manual de quatro marcos duplica uma composição já existente que recebe todas as pausas, causando os 85 minutos indevidos do item 2. (`app/ponto/services/efeito_ausencia.py:156`; `app/escala/utils.py:353`; `app/escala/utils.py:363`)

`dias_abono` também possui dois significados: no motor, exclui todo dia em `_mj`; no resumo da lavratura, conta qualquer dia com minutos abonados positivos. (`app/ponto/motor_calculo_v2.py:2629`; `app/ponto/services/dia_pago.py:698`; `app/relatorios/cartao_pela_celula.py:190`)

A cobertura ainda muda **depois** da resposta do juiz anunciado, pelo filtro de cruzamento. Isso demonstra decisão adicional de cobertura; duplicação de um juiz **já registrado**, especificamente: [nao verificado]. (`app/ponto/services/efeito_ausencia.py:176`; `:182`; `app/ponto/turnos.py:2751`; `app/core/juizes.py:207`; `:251`)

Segundo escritor persistente novo, relógio novo fora da borda ou toque adicional no motor além da inicialização e do bloco de abono apresentado: **nada demonstrado**. A falha comprovada é que essa leitura nova permanece desconectada do abono gravado. (`app/ponto/motor_calculo_v2.py:813`; `:2628`; `app/ponto/services/fechamento.py:1067`)

DIFF produtivo 0/0, hashes iguais dos TXT exportados e aplicação restrita à competência 11: **[nao verificado]**.

## 14/11 16:36 · sessao 2224a6af9587 · E3-V10-DEGRAU-DONO-E-PORTA · construtor · origem=Code

Cura implementada no juiz existente: COLAB exige `responder` ligado a pergunta pendente no app; `cobrar` residual não atribui dono; dia com porta apenas no app publica `porta_app`. A ordem permanece colab → admin → chamado → sistema. ([tarefa_do_dia.py:607](/home/ronald/wt-codex-e3-v10-degrau-dono-e-porta-162117/app/chamados/services/tarefa_do_dia.py:607))

Diff resumido:

- `tarefa_do_dia.py::CargaDaTarefa`: consulta do app uma vez por colaborador, memorizada na carga, com `self.agora`; `_portas_da_tarefa` decide o dono; `porta_da_tarefa` projeta a nova rota. (`chamados/services/tarefa_do_dia.py:389`, `:556`, `:573`, `:626`)
- `pergunta_no_app.py::consulta_do_app`: recebe `agora=None` e `em_lote=True`; conserva o predicado existente. (`chamados/services/pergunta_no_app.py:64`)
- `furo_sem_canal.py::medir`: acrescenta `com_porta_app`. (`chamados/services/furo_sem_canal.py:116`)
- `vigia_fio.py::censo_alcance/linhas_alcance`: acrescenta contador e saída de `porta_app`. (`chamados/services/vigia_fio.py:577`, `:730`)
- `lavrar_furo_sem_canal.py::handle`: exibe o contador do app. (`chamados/management/commands/lavrar_furo_sem_canal.py:37`)
- `fora_da_folha.py::DESTRAVA`: explicita “SEM PORTA” para `nada`, inclusive quando existe chamado residual na fila. (`folha/fora_da_folha.py:54`)
- Testes anteriores receberam a nova alimentação; alvos literais dos mutantes foram atualizados. (`chamados/tests/test_e3_portas_v6_sem_banco.py:19`, `test_e3_c8b_sem_banco.py:224`, `e3_portas_apoio.py:121`, `e3_volta7_apoio.py:146`, `e3_mutantes.py:34`, `e3_portas_v6_mutantes.py:58`)

Quem escreve e quem lê os campos:

| Campo | Escritor | Leitores |
|---|---|---|
| `rota`, `fila`, `verbos`, `fio` publicados | `_portas_da_tarefa`, `chamados/services/tarefa_do_dia.py:621` | Projeção `:633`; consumidores abaixo |
| Verbos de entrada | `alimentar_fila`, `chamados/juizes.py:1570` | Juiz da tarefa, `chamados/services/tarefa_do_dia.py:605` |
| `pendentes` do app | `consulta_do_app`, `chamados/services/pergunta_no_app.py:74` | Carga, `chamados/services/tarefa_do_dia.py:564` |
| Memo de pendentes | Carga, `chamados/services/tarefa_do_dia.py:563` | Mesma carga e juiz, `:556`, `:567`, `:607` |
| `pergunta_id`, ações | `_tarefa_pura`, `chamados/services/tarefa_do_dia.py:263` | Juiz das portas, `:607` |
| Identidade do sistema | Leitura em `chamados/services/tarefa_do_dia.py:392` | Contextos de tranca, `:444`, `:562` |

Censo dos consumidores:

| Leitor | Comportamento com `porta_app` |
|---|---|
| `tarefa_do_dia.py::porta_da_tarefa`, `:633` | Aceita rota preenchida, sem enumeração própria. |
| `folha/fora_da_folha.py:70`, `:85`, `:244` | Lê fio e fila publicados; fila vazia é aceita. |
| `folha/retrato_folha.py:183`, `:974`, `:1015` | Usa fio, sem consultar rota: `colab` conserva “Cobrar no fio”; `ninguem` corresponde a `nada`. |
| `chamados/services/furo_sem_canal.py:97`, `:116` | Reconhece a porta e agora a conta separadamente. |
| `chamados/services/vigia_fio.py:662`, `:730` | A indexação dinâmica precisava da nova chave; contador e apresentação adaptados. |
| `escala/services/cadastro_realidade.py:469` | Verifica presença de porta; o teste novo exclui **um dia sintético** do beco. (`chamados/tests/test_e3_degrau_dono_e_porta.py:267`) |
| `chamados/management/commands/diff_e3_sombra.py:43`, `:53`, `:143` | Projeta, compara e imprime a rota recebida. |
| `ponto/services/pautas_dos_discordantes.py:81` | Lê indiretamente o fio da Folha, comparando com `FIO_SEM_PORTA`. |
| `chamados/management/commands/backfill_fio_morto_e4b.py:87` | Consome instâncias e ações; não interpreta a rota. |

A saída completa do `git grep -n` está em [e3_degrau_censo.txt:1](/home/ronald/wt-codex-e3-v10-degrau-dono-e-porta-162117/app/chamados/tests/e3_degrau_censo.txt:1). Os consumidores diretos anteriores de `consulta_do_app` conservam os defaults; `chamados/models.py:1846` é uma lápide, não um leitor ativo. (`api/views.py:994`, `:2534`; `core/views.py:253`; `chamados/services/aviso_pendencia.py:9`; `chamados/services/cobranca.py:48`)

O `em_lote=True` abre `tranca_da_consulta`, carrega tranca/toque e restaura o contexto. Existe escrita indireta possível: o toque chama `get_user_sistema()`, cujo corpo usa `get_or_create`. Por isso a carga usa `em_lote=False` e fornece ao contexto existente a identidade obtida por `User.objects.get`. Usuário ausente provoca falha, sem criação. (`chamados/juizes.py:1128`, `:1152`, `:1225`; `core/utils.py:18`; `chamados/services/tarefa_do_dia.py:392`, `:562`)

Resultados locais, sem certificação:

- RED anterior à cura: **18 testes, 12 falhas de asserção**. ([e3_degrau_red.txt:161](/home/ronald/wt-codex-e3-v10-degrau-dono-e-porta-162117/app/chamados/tests/e3_degrau_red.txt:161)) [nao verificado]
- Casos novos: **24 verdes**; rodada com regressões: **88 verdes**, incluindo execução dos corpos originais do selo L-129. ([e3_degrau_green.txt:27](/home/ronald/wt-codex-e3-v10-degrau-dono-e-porta-162117/app/chamados/tests/e3_degrau_green.txt:27), [e3_degrau_regressoes.txt:92](/home/ronald/wt-codex-e3-v10-degrau-dono-e-porta-162117/app/chamados/tests/e3_degrau_regressoes.txt:92))
- **16 mutantes**, cada um vermelho por asserção, sem erro de execução. Os dois exigidos mordem C4 e C3. Cada comando individual está registrado junto ao resultado. ([e3_degrau_mutantes.txt:1](/home/ronald/wt-codex-e3-v10-degrau-dono-e-porta-162117/app/chamados/tests/e3_degrau_mutantes.txt:1), [:21](/home/ronald/wt-codex-e3-v10-degrau-dono-e-porta-162117/app/chamados/tests/e3_degrau_mutantes.txt:21), [:346](/home/ronald/wt-codex-e3-v10-degrau-dono-e-porta-162117/app/chamados/tests/e3_degrau_mutantes.txt:346))

Comandos de reprodução:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e3_degrau_mutantes --base
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest -v chamados.tests.test_e3_degrau_dono_e_porta
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e3_degrau_mutantes --todos
```

Não alterei arquivos proibidos, ignorados ou a allowlist L-129; não fiz commit, merge, push ou deploy. HEAD permanece na base e `git diff --check` passou. Não executei ORM nem remedida de produção. ([e3_degrau_auditoria.txt:1](/home/ronald/wt-codex-e3-v10-degrau-dono-e-porta-162117/app/chamados/tests/e3_degrau_auditoria.txt:1), [:27](/home/ronald/wt-codex-e3-v10-degrau-dono-e-porta-162117/app/chamados/tests/e3_degrau_auditoria.txt:27)) [nao verificado]

Registro solicitado: **PORTA-DO-ADMIN-NA-PERGUNTA-VENCIDA**, número recebido **2333**, pergunta “o que o admin pode fazer quando o colab não responde”; obra não construída nesta fatia. Remedida colab/admin/sem porta e número de produção: **[não verificado]**.

## 14/11 16:35 · sessao 6615c2d1b8dd · - · - · origem=Ronald (tmux)

5. **Nada de falha de relógio demonstrada no caminho novo.** A fonte ordena justificativas por `data_turno, pk`, mede com timestamps explícitos e chama o pareador com datas explícitas (`app/ponto/services/efeito_ausencia.py:98`, `:122`, `:179`, `:181`). O previsto consulta escala/marcos/pausas, sem batidas (`app/escala/utils.py:2275`, `:2285`, `:2288`, `:2310`). Os testes de idempotência repetem somente o juiz puro e a consulta da fonte; não repetem o fechamento completo (`app/ponto/tests/test_justificativa_paga_o_periodo.py:134`, `:320`). Idempotência integral de produção: **[nao verificado]**.

6. **O RED principal tem mordida localizada.** O teste exige `horas_abono == 11.5`, zero faltas e zero DSR perdido (`app/ponto/tests/test_justificativa_paga_o_periodo.py:255`). Reverter somente a conta nova do motor elimina o crédito explícito de 690 minutos que esse teste exige (`app/ponto/motor_calculo_v2.py:2628`). O controle de dia inteiro passaria com o motor anterior; exige apenas um dia abonado e horas positivas (`app/ponto/tests/test_justificativa_paga_o_periodo.py:262`). As propriedades de “pago + trabalhado” somam `minutos_trabalhados` dos períodos, não conferem `horas_abono` ou o fechamento gravado (`:338`, `:350`, `:380`, `:391`). Suíte executada/RED histórico: **[nao verificado]**.

As mudanças perguntadas preservam asserções: `test_cobparc` acrescenta mock da fonte de justificativas para manter a fixture sem banco (`app/ponto/tests/test_cobparc_marco_no_periodo.py:57`), preservando a cobrança de saída dispensada e hora não fabricada (`:75`). `test_s5b` amplia a lista e o motor fictício com o memo novo (`app/ponto/tests/test_s5b_porta_unica.py:988`, `:996`); continua removendo cada atributo e exigindo exceção nominal (`:1014`). Não há afrouxamento desses asserts.

7. **PROCEDENTE — derivação paralela de cadeia perde a segunda pausa.** A fonte reconstrói manualmente apenas `hi/hii/hfi/hf` (`app/ponto/services/efeito_ausencia.py:156`), embora a entidade declare também `hii2/hfi2` (`app/escala/models.py:1652`) e já exista `cadeia_do_dia`, que recebe todas as pausas (`app/escala/utils.py:353`). O medidor desconta somente pares presentes no dicionário recebido (`app/ponto/turnos.py:2786`), enquanto o previsto desconta todas as pausas (`app/escala/utils.py:2310`, `:2324`). Exemplo sintético confirmado em memória pelos métodos reais: 07–19, pausas 10–11:25 e 14–15:25, entrada 07:00: a cadeia produzida pela fonte paga **635 minutos**; completa, **550 minutos**. Nenhum arquivo foi escrito; ocorrência produtiva desse exemplo: **[nao verificado]**.

Não demonstrar “segundo juiz duplicando juiz registrado” apenas pela ausência de registro: o catálogo de turno e ausência não lista o juiz novo (`app/core/juizes.py:207`, `:251`), e o documento distingue lookup de registro vivo (`app/docs/JUIZES.md:7`). O corte fornecido autoriza expressamente seu nascimento. Há, porém, uma decisão posterior que ainda altera `cobre`: a fonte chama o juiz e depois remove cobertura por cruzamento (`app/ponto/services/efeito_ausencia.py:176`, `:182`; `app/ponto/turnos.py:2750`). Isso mostra cobertura decidida em duas etapas, não prova duplicação de autoridade já registrada.

## 14/11 16:35 · sessao 3c37fe314776 · - · - · origem=Ronald (tmux)

**PROCEDENTE — há derivação paralela e o período novo não chega ao abono materializado.**

- O motor lê `minutos_justificados` e soma o período em `horas_abono` (`app/ponto/motor_calculo_v2.py:2628-2634`), mas `FechamentoMensal.minutos_abonados` vem exclusivamente de `por_dia_da_grade` (`app/ponto/services/fechamento.py:1065-1067`).
- Essa função só abona `tipo_dia == 'ausencia'`, usando `previsto - realizado`; ignora `min_justificados` e `justificado` (`app/ponto/services/dia_pago.py:387-401`). A justificativa parcial mantém `tipo_dia='trabalho'` e apenas acrescenta o campo de minutos (`app/escala/utils.py:1257-1278`).
- A grade usada pelo fechamento lê a ATA (`app/folha/export.py:335-338`); o cartório não lavra `min_justificados` (`app/ponto/services/cartorio.py:738-739`) e o leitor não o transporta (`app/escala/services/leitor_celula.py:422-444`).
- O mesmo mapa inadequado alimenta a lavratura `DiaPago` (`app/ponto/services/fechamento.py:1217-1218`). Na competência, PDF/espelho recebem `horas_abono=minutos_abonados/60` do fechamento online (`app/relatorios/cartao_pela_celula.py:69-76,118`) ou da lavratura (`app/relatorios/cartao_pela_celula.py:186`), sobrescrevendo o resumo (`app/relatorios/cartao_pela_celula.py:323-346`).
- Contraexemplo sintético da função real, executada isolada por AST, sem banco e sem escrita: dia `trabalho`, previsto 720, realizado 0, `min_justificados=690`, `justificado=True` devolve `abono={}`. É consequência direta de `app/ponto/services/dia_pago.py:395-401`.

**PROCEDENTE — chamador de sombra continua com memo vazio.**

- `diff_espelho_alimentacao` monta motor, alimenta os demais memos e calcula sem alimentar `minutos_justificados` (`app/ponto/management/commands/diff_espelho_alimentacao.py:41-59`), inclusive no degrau anunciado como completo, que recebe `ef['justificadas']` e `ef['dia_inteiro']` (`app/ponto/management/commands/diff_espelho_alimentacao.py:113-122`).
- Como o memo nasce `{}` (`app/ponto/motor_calculo_v2.py:813`), esse degrau pode dar abono previsto inteiro enquanto os leitores migrados pagam o período (`app/ponto/motor_calculo_v2.py:2628-2634`).
- `diff_extrato_parcial` consulta as justificativas por conta própria e monta motor sem o novo memo (`app/relatorios/management/commands/diff_extrato_parcial.py:42-68`); seus campos retornados não incluem abono (`app/relatorios/management/commands/diff_extrato_parcial.py:70-76`).
- `esmeril_espelho` também não alimenta o memo e usa o resultado somente para assinaturas de pontualidade/HE (`app/ponto/services/esmeril_espelho.py:160-177`). `vigiar_motor_autoridade` não alimenta os efeitos, mas compara geometria/trabalhado (`app/ponto/management/commands/vigiar_motor_autoridade.py:57-68`). Esses dois caminhos não provam diferença de abono pago.

Mapa dos campos:

| Campo | Escritores/fontes | Leitores |
|---|---|---|
| `minutos_justificados` | Inicialização `app/ponto/motor_calculo_v2.py:813`; alimentação fechamento normal/órfão `app/ponto/services/fechamento.py:755,831`; espelho `app/ponto/services/espelho.py:1134`; PDF `app/relatorios/pdf_espelho.py:407` | Conta mensal `app/ponto/motor_calculo_v2.py:2628-2634`; declaração de memo `app/ponto/calculador/nucleo.py:424` |
| `min_justificados` | Efeito deriva de `periodos_justificados` (`app/ponto/services/efeito_ausencia.py:45-47,67`); grade escreve o valor (`app/escala/utils.py:1276`); fechamento deriva novamente o mapa (`app/ponto/services/fechamento.py:665-667`) | Espelho/PDF alimentam memo (`app/ponto/services/espelho.py:1134`; `app/relatorios/pdf_espelho.py:407`); nenhum leitor de dinheiro da grade usa essa chave (`app/ponto/services/dia_pago.py:387-401`) |
| `dias_com_ausencia_inteira` | Fonte `app/ponto/turnos.py:2829-2846`; efeito `app/ponto/services/efeito_ausencia.py:68`; fechamento `app/ponto/services/fechamento.py:688-689,754,830`; espelho `app/ponto/services/espelho.py:1133`; PDF `app/relatorios/pdf_espelho.py:406`; diagnósticos `app/ponto/services/esmeril_espelho.py:164`, `app/relatorios/management/commands/diff_extrato_parcial.py:67`, `app/ponto/management/commands/diff_espelho_alimentacao.py:57` | Teto de pontualidade `app/ponto/motor_calculo_v2.py:1090-1109`; contexto calculador `app/ponto/calculador/regras.py:117-120`; alimentação por dia `app/ponto/calculador/alimentacao.py:532`; fechamento envia ao oráculo `app/ponto/services/fechamento.py:1281` |
| `horas_abono` | Dataclass `app/ponto/motor_calculo_v2.py:292`; cálculo `app/ponto/motor_calculo_v2.py:2631-2634`; composição de fatias `app/ponto/services/fechamento.py:99` | PDF primeiro lê motor+parcial (`app/relatorios/pdf_espelho.py:480`), depois autoridade do resumo usa fechamento/lavratura (`app/relatorios/cartao_pela_celula.py:118,186,346`) |
| `dias_abono` | Dataclass `app/ponto/motor_calculo_v2.py:284`; motor `app/ponto/motor_calculo_v2.py:2630`; composição `app/ponto/services/fechamento.py:99`; resumo espelho/PDF `app/ponto/services/espelho.py:1253`, `app/relatorios/pdf_espelho.py:479` | PDF badge `app/relatorios/pdf_espelho.py:787`; ramo lavratura sobrescreve usando dias com minutos abonados positivos (`app/relatorios/cartao_pela_celula.py:190`; `app/ponto/services/dia_pago.py:687-699`) |
| `resumo_dias_abono` | Não localizado na cópia pela busca literal; existência/caminho [nao verificado] | [nao verificado] |

**PROCEDENTE — campo `dias_abono` possui dois significados conforme a fonte.** O motor exclui todo dia em `_mj`, mesmo com minutos pagos (`app/ponto/motor_calculo_v2.py:2629-2630`); o resumo da lavratura conta qualquer dia com `minutos_abonados>0` (`app/ponto/services/dia_pago.py:687-699`; `app/relatorios/cartao_pela_celula.py:190`).

**PROCEDENTE — ausência inteira coexistente não limpa o mapa parcial.** `efeitos_da_ausencia` conserva `min_justificados` ao acrescentar uma ausência ABONA (`app/ponto/services/efeito_ausencia.py:47,55-67`); fechamento faz o mesmo (`app/ponto/services/fechamento.py:667,695-701`). O motor então exclui esse dia do abono inteiro e paga apenas `_mj`, embora exista ausência inteira (`app/ponto/motor_calculo_v2.py:2628-2634`).

Fora do diff:

- TXT Domínio monta os eventos do fechamento, sem evento de abono/período na lista (`app/folha/export.py:439-461,730-732`). `datas_abonadas` usa `Ausencia`/catálogo para dias da rubrica 8932, não o mapa de justificativas (`app/folha/export.py:193-206`). Impacto financeiro efetivo no Domínio [nao verificado].
- Prontidão soma `minutos_abonados` do fechamento (`app/folha/export.py:1076-1079`), mas seu fallback rederiva abono pelo previsto inteiro de dias de ausência (`app/folha/export.py:1095-1111`).
- Haiku “fora da folha” lê `classificar_export` (`app/folha/fora_da_folha.py:115-119,216-222`); não lê os seis campos diretamente nesses sítios. Holerite nesta cópia recebe arquivos de folha, com lote de upload (`app/holerite/models.py:6-20`); equivalência de valores importados com os novos minutos [nao verificado].

## 14/11 16:35 · sessao 2a67c3406a0b · - · - · origem=Ronald (tmux)

2. **PROCEDENTE — segunda pausa perdida e crédito acima do previsto.** A fonte constrói somente `hi/hii/hfi/hf`, via quatro valores de `marcos_do_dia`; a segunda pausa nunca chega ao medidor (`app/ponto/services/efeito_ausencia.py:156`, `app/escala/models.py:1131`). O medidor só desconta pares presentes no mapa (`app/ponto/turnos.py:2786`). Já o previsto lê todas as pausas (`app/escala/models.py:1197`, `app/escala/utils.py:2310`, `app/escala/utils.py:2324`). Contraexemplo: jornada 07–19, duas pausas de 85 minutos, entrada 07:00 e saída justificada: crédito calculado 635 minutos, previsto 550; o motor soma o crédito sem teto (`app/ponto/turnos.py:2738`, `app/ponto/turnos.py:2791`, `app/ponto/motor_calculo_v2.py:2633`).

A chave é `Justificativa.data_turno`, e a fonte exige que o turno da âncora tenha essa mesma data (`app/ponto/services/efeito_ausencia.py:107`, `app/ponto/services/efeito_ausencia.py:165`). A cadeia dos marcos começa nessa data, permitindo saída no dia seguinte (`app/ponto/services/efeito_ausencia.py:163`). No Base, a comparação usa o dia da jornada; no Comercial, o DSR é refeito por data civil da entrada, constituindo derivação paralela preexistente (`app/ponto/motor_calculo_v2.py:2581`, `app/ponto/motor_calculo_v2.py:3473`, `app/ponto/motor_calculo_v2.py:3477`). Ocorrência desse desvio nos dias raspados: [nao verificado].

3. **PROCEDENTE — duas âncoras do mesmo turno podem duplicar minutos.** A cobertura une os rótulos, mas soma os minutos de cada justificativa (`app/ponto/services/efeito_ausencia.py:133`). Duas justificativas aprovadas, uma ancorada na entrada 07:30 e outra na volta 12:00 do mesmo turno `E07:30/S11:00/E12:00`, medem ambas a cauda 12–19: 420 + 420 = 840 minutos (`app/ponto/services/efeito_ausencia.py:168`, `app/ponto/services/efeito_ausencia.py:179`, `app/ponto/turnos.py:2738`). A unicidade impede repetir a mesma âncora, mas permite âncoras distintas; a restrição por dia vale apenas para pendentes (`app/ponto/models.py:187`, `app/ponto/models.py:189`). O motor soma os 840 sem deduplicar nem limitar ao previsto (`app/ponto/motor_calculo_v2.py:2633`). Ocorrência em produção: [nao verificado].

Um dia em `_mj` pode estar fora de `_abono` porque teve período de trabalho; a soma nova o inclui expressamente, independentemente de `_abono` (`app/ponto/motor_calculo_v2.py:2611`, `app/ponto/motor_calculo_v2.py:2633`). Dias fora dos obrigatórios não recebem esse crédito (`app/ponto/motor_calculo_v2.py:2601`, `app/ponto/motor_calculo_v2.py:2634`).

4. **PROCEDENTE — cobertura vazia tem caminho distinto de conserva.** Sem âncora, papel errado e turno fechado retornam motivo; célula sem marcos também conserva (`app/ponto/turnos.py:2694`, `app/ponto/turnos.py:2700`, `app/ponto/turnos.py:2703`, `app/ponto/services/efeito_ausencia.py:154`). Entretanto, depois desse predicado, o filtro de sobreposição pode remover o marco e retornar vazio sem motivo (`app/ponto/services/efeito_ausencia.py:182`, `app/ponto/services/efeito_ausencia.py:183`, `app/ponto/turnos.py:2764`). O efeito exclui esse dia de `justificadas` e de `min_justificados` (`app/ponto/services/efeito_ausencia.py:46`). Há caso sintético correspondente e referência nominal a col451 no teste (`app/ponto/tests/test_justificativa_paga_o_periodo.py:212`, `app/ponto/tests/test_justificativa_paga_o_periodo.py:383`).

Vazio não implica automaticamente falta: o Base considera todo período presente como dia trabalhado, inclusive aberto, sem testar minutos positivos (`app/ponto/motor_calculo_v2.py:2499`, `app/ponto/motor_calculo_v2.py:2581`). Portanto, nesses dias, paga o trabalho calculado e nenhum crédito de justificativa; falta/DSR dependem de o dia estar em `datas_trabalho` (`app/ponto/motor_calculo_v2.py:2605`, `app/ponto/motor_calculo_v2.py:2633`). Resultado individual de col206, col451 e col551, e condição global 0/0: [nao verificado].

O filtro ainda compara contra envelope bruto entrada–fim, sem retirar pausas, enquanto o motor calcula pares com intervalos; sobreposição detectada não prova sobreposição com minutos efetivamente pagos (`app/ponto/turnos.py:2745`, `app/ponto/turnos.py:2748`, `app/ponto/motor_calculo_v2.py:2551`, `app/ponto/motor_calculo_v2.py:2558`).

_5198 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
