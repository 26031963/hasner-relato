# CODEX-ESTADO

> Ultima resposta processada: **14/11/2026 20:20** · publicado em **14/11/2026 20:20** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 14/11 20:20 · sessao 090dd8640d32 · - · - · origem=Ronald (tmux)

**P2 — PROCEDENTE-candidato, severidade estrutura/texto: `dias_abono` passou a excluir também cobertura inteira sem trabalho.** A fonte agora coloca todos os dias cobertos no mapa, inclusive `cobre=None` (`app/ponto/services/efeito_ausencia.py:47`), mas o motor ainda usa **ausência no mapa** para contar dias abonados (`app/ponto/motor_calculo_v2.py:2629`, `app/ponto/motor_calculo_v2.py:2630`). Cenário sintético: justificativa `outro` sem batidas, previsto 480 → fonte paga 480 (`app/ponto/services/efeito_ausencia.py:122`, `app/ponto/services/efeito_ausencia.py:145`); motor produz **8 horas de abono e 0 dias**, embora o campo signifique “dias previstos sem trabalho mas justificados” (`app/ponto/motor_calculo_v2.py:284`, `app/ponto/motor_calculo_v2.py:2633`). Executei somente esse ramo real por AST em memória e obtive esse resultado; ocorrência produtiva [nao verificado].

O espelho transporta esse zero (`app/ponto/services/espelho.py:1253`); PDF também (`app/relatorios/pdf_espelho.py:479`), e oculta o badge quando zero (`app/relatorios/pdf_espelho.py:787`). Já o ramo de lavratura usa outra definição de `dias_abono`: conta linhas com `minutos_abonados>0` (`app/ponto/services/dia_pago.py:701`, `app/relatorios/cartao_pela_celula.py:190`). Assim, a mesma justificativa pode contar zero pelo motor e um pela lavratura; o ramo de escolha está em `app/relatorios/cartao_pela_celula.py:341`.

Mapa solicitado, para consolidar:

| Campo | Escritor/fornecedor | Leitores |
|---|---|---|
| `minutos` | Inicializado por dia de `j.data_turno`; valor final `max(0, previsto-trabalhado)` (`app/ponto/services/efeito_ausencia.py:118`, `app/ponto/services/efeito_ausencia.py:145`). | Feed `min_justificados` (`app/ponto/services/efeito_ausencia.py:47`); overlays da grade (`app/escala/utils.py:1257`, `app/escala/utils.py:1277`). |
| `cobre` | Resultado de `_cobertura_de_uma`, conservação e união (`app/ponto/services/efeito_ausencia.py:125`, `app/ponto/services/efeito_ausencia.py:129`, `app/ponto/services/efeito_ausencia.py:133`); juiz chamado em `app/ponto/services/efeito_ausencia.py:208`. | Inclusão em justificadas/mapa (`app/ponto/services/efeito_ausencia.py:46`); dispensa de marcos (`app/escala/utils.py:1251`, `app/escala/utils.py:1258`, `app/escala/utils.py:1267`). |
| `motivo` excedente | Comparação **bruto+trabalhado**, seguida do literal (`app/ponto/services/efeito_ausencia.py:146`, `app/ponto/services/efeito_ausencia.py:147`). | Projeção revisão (`app/ponto/services/efeito_ausencia.py:153`); Central (`app/ponto/views.py:1747`, `app/ponto/views.py:1750`); texto HE (`app/templates/ponto/fila_justificativas.html:116`). |
| `min_justificados` | Mapa dia→minutos (`app/ponto/services/efeito_ausencia.py:47`); ausência inteira ABONA remove a chave (`app/ponto/services/efeito_ausencia.py:63`). Linha da grade transporta o valor (`app/escala/services/leitor_celula.py:449`). | Fechamento (`app/ponto/services/fechamento.py:667`); espelho (`app/ponto/services/espelho.py:1134`); PDF (`app/relatorios/pdf_espelho.py:407`); esmeril (`app/ponto/services/esmeril_espelho.py:166`); comandos (`app/ponto/management/commands/diff_espelho_alimentacao.py:56`, `app/relatorios/management/commands/diff_extrato_parcial.py:62`). |
| `minutos_justificados` motor | Nasce `{}` (`app/ponto/motor_calculo_v2.py:813`); alimentado pelos leitores acima e pelos dois motores do fechamento (`app/ponto/services/fechamento.py:755`, `app/ponto/services/fechamento.py:831`). | Ramo de abono usa presença da chave e converte minutos para horas dividindo por 60 (`app/ponto/motor_calculo_v2.py:2628`, `app/ponto/motor_calculo_v2.py:2634`). |
| Abono gravado | `por_dia_da_grade` soma `min_justificados` na data da linha (`app/ponto/services/dia_pago.py:391`, `app/ponto/services/dia_pago.py:399`); fechamento soma mapa em `minutos_abonados` (`app/ponto/services/fechamento.py:1066`, `app/ponto/services/fechamento.py:1067`). | Resumo converte o gravado para horas (`app/relatorios/cartao_pela_celula.py:118`, `app/relatorios/cartao_pela_celula.py:186`). |

Os transportadores examinados mantêm **minutos por chave de data** até a conversão explícita `/60`; a comparação entre a atribuição do dia e o trabalho usado pelo motor ficou com você (`app/ponto/services/efeito_ausencia.py:118`, `app/ponto/services/dia_pago.py:391`, `app/ponto/motor_calculo_v2.py:2634`).

## 14/11 20:20 · sessao d24ad1288d7e · - · - · origem=Ronald (tmux)

- **PROCEDENTE-candidato — estrutura — segundo juiz residual:** `minutos_do_periodo_justificado` continua respondendo “minutos que […] paga” pela duração do trecho, enquanto a fonte paga saldo diário. Cenário: previsto480, E16 sem S, hf17: antigo60, fonte480. A função permanece sustentada por testes, contrariando L-111. (`app/ponto/turnos.py:2769`, `app/ponto/turnos.py:2779`, `app/ponto/services/efeito_ausencia.py:145`, `app/ponto/tests/test_justificativa_paga_o_periodo.py:49`, `app/docs/LEIS.md:137`).
- **PROCEDENTE-candidato — estrutura — previsto com duas autoridades:** registro aponta `minutos_previstos_do_dia`; novo helper prefere ata e só usa o registrado como fallback. Cenário sintético: ata440, marcos12:50–21:20 sem pausa: helper440, registrado510. A prioridade da ata já existia na grade; novidade demonstrada é a extração pública e seu consumo pela justificativa. (`app/core/juizes.py:185`, `app/escala/services/leitor_celula.py:354`, `app/escala/services/leitor_celula.py:361`, `app/escala/utils.py:2288`, `app/escala/utils.py:2292`, `app/ponto/services/efeito_ausencia.py:138`).
- **PROCEDENTE-candidato — texto — excedente hipotético apresentado como HE real:** `motivo` testa bruto+trabalhado, embora o pagamento já tenha virado saldo; a UI imprime “Hora além do turno”. Cenário sintético: justificativa `outro`, previsto480 e180 trabalhados inteiramente dentro do turno: bruto480 gera motivo, pago300, sem demonstrar qualquer trabalho além do turno. (`app/ponto/services/efeito_ausencia.py:122`, `app/ponto/services/efeito_ausencia.py:143`, `app/ponto/services/efeito_ausencia.py:145`, `app/ponto/services/efeito_ausencia.py:146`, `app/templates/ponto/fila_justificativas.html:116`).
- **Segundo escritor: não vi nos trechos lidos.** Fonte consulta e devolve estruturas; overlay altera grade em memória; fechamento recebe o mapa pela derivação existente e `lavrar` persiste `DiaPago`. Censo global de escritores [nao verificado]. (`app/ponto/services/efeito_ausencia.py:98`, `app/escala/utils.py:1251`, `app/escala/utils.py:1277`, `app/ponto/services/fechamento.py:1065`, `app/ponto/services/dia_pago.py:359`).

## 14/11 20:20 · sessao 850decff23c9 · - · - · origem=Ronald (tmux)

**P5 — PROCEDENTE-candidato, severidade estrutura:** `revisao` materializa **todas as aprovadas**, com filtros opcionais de empresa/nome, sem janela de competência nem paginação; para cada colaborador pergunta pelo intervalo entre a aprovação mais antiga e a mais recente. Cenário: duas aprovadas distantes fazem carregar e parear todo o histórico intermediário. (`app/ponto/views.py:1707`, `:1714`, `:1720`, `:1732`, `:1747`; `app/ponto/services/efeito_ausencia.py:98`, `:109`, `:114`.)

**Consultas por colaborador:** não há número universal; são **7 fixas + consultas condicionais + consultas por dia coberto**. No caminho usual com DNA de até um intervalo e célula com geradora em cada dia, a projeção faz **8 + 2D**, sendo D o número de dias cobertos; isso exclui a consulta inicial e os cards da UI. (`app/ponto/services/efeito_ausencia.py:98`, `:109`, `:110`; `app/ponto/turnos.py:1879`, `:1900`, `:1903`, `:1931`, `:1955`; `app/escala/alimentacao.py:148`; `app/ponto/turnos.py:1574`, `:1609`, `:1925`, `:2003`; `app/escala/utils.py:100`.)

O termo **2D** vem de chamar `realizado_do_dia` sem fornecer intervalo/modo: para cada dia ele busca novamente a célula com geradora e depois a pausa em outra consulta, embora `periodos_justificados` já tenha carregado células e vínculos. Cenário sintético: 100 colaboradores × 20 dias cobertos ⇒ **4.800 consultas na projeção usual**, além da UI. (`app/ponto/services/efeito_ausencia.py:109`, `:110`, `:136`, `:140`; `app/ponto/turnos.py:781`, `:788`, `:794`; `app/escala/servico_jornada.py:46`; `app/escala/models.py:1163`, `:1182`.)

Há consultas adicionais condicionais: geradoras ausentes da candidatura, agregação da última batida e fallbacks em dias sem célula/previsto. (`app/escala/alimentacao.py:155`; `app/ponto/turnos.py:1914`; `app/escala/servico_jornada.py:64`.)

A configuração UI declara `gthread`, quatro threads e `timeout = 30`; duração real e estouro em produção **[nao verificado]**. (`app/gunicorn.conf.py:19`, `:20`, `:21`.)

## 14/11 20:20 · sessao 513b12f5fdae · - · - · origem=Ronald (tmux)

1. **PROCEDENTE-candidato — P3 — texto/estrutura: falso “hora além do turno”.** O motivo nasce de `bruto + trabalhado > previsto`, embora o crédito final seja `max(0, previsto - trabalhado)` (`app/ponto/services/efeito_ausencia.py:143-147`). No cenário col788 do teste, há previsto 420, par trabalhado de 59 minutos e crédito de 361; ainda assim recebe esse motivo (`app/ponto/tests/test_justificativa_soma_sem_banco.py:178-188`). A Central imprime “Hora além do turno — revisar HE” mesmo sem trabalho excedendo o previsto (`app/templates/ponto/fila_justificativas.html:116`).

2. **PROCEDENTE-candidato — P3 — estrutura: segunda lista independente da decisão HE.** A Central seleciona motivos da justificativa sem consultar decisão HE (`app/ponto/views.py:1747-1754`); a Gestão de HE consulta decisões vigentes para determinar o estado do dia (`app/ponto/services/gestao_he.py:86-89`, `app/ponto/services/gestao_he.py:278-280`). Portanto, decidir HE não retira o motivo da Central: o predicado continua dependendo de bruto e trabalhado (`app/ponto/services/efeito_ausencia.py:143-147`). É segunda projeção/lista; não há novo escritor persistente nesse ramo (`app/ponto/views.py:1738-1754`).

3. **PROCEDENTE-candidato — P3 — estrutura: aviso não equivale à porta HE.** O caminho existente recebe exclusivamente `he_fora_da_janela`; dia sem essa lista é ignorado pelo retrato (`app/ponto/services/he_pendente_lavrado.py:132-150`). O espelho popula essa lista por pontas fora dos marcos ou dia sem previsto, condicionado à janela ativa e à vigência (`app/ponto/services/espelho.py:721-749`); trabalho maior que previsto não é seu critério geral de entrada (`app/ponto/janela_he.py:194-230`). Cenário sintético: janela desligada e trabalho acima do previsto deixam `he_fora_da_janela=[]`, enquanto a justificativa pode receber motivo e aparecer na Central (`app/ponto/services/espelho.py:721-722`, `app/ponto/services/efeito_ausencia.py:143-147`). Falta ligação desse aviso à pendência/ação existente: o filtro apenas monta a lista (`app/ponto/views.py:1747-1754`), e a porta real exige minutos positivos de pendência (`app/ponto/portas/he.py:138-172`).

Quando existem pontas reconhecidas, o dia já chega pelo caminho anterior: marcação das pontas → espelho → retrato → Gestão HE (`app/ponto/janela_he.py:218-230`, `app/ponto/services/espelho.py:727-742`, `app/ponto/services/he_pendente_lavrado.py:132-150`, `app/ponto/services/gestao_he.py:236-250`). Nessa classe, a Central acrescenta outra lista (`app/ponto/views.py:1738-1754`). Alcance produtivo e contagens: **[nao verificado]**.

## 14/11 20:19 · sessao 6ab72d5695ec · - · - · origem=Ronald (tmux)

P6 — não vi relógio novo no delta c5+c6: os corpos acrescentados/alterados em `app/ponto/services/efeito_ausencia.py:93`, `app/escala/services/leitor_celula.py:354`, `app/escala/utils.py:1257` e `app/ponto/views.py:1738` não consultam `now()`/`today()`. Novidade dos demais relógios desde `14b61c124`: [nao verificado].

**PROCEDENTE-candidato — estrutura: DIFF sensível ao instante de execução.** `recalcular_fechamento_mes` não recebe `agora` (`app/ponto/services/fechamento.py:448`), consulta `timezone.now()` em cada execução (`app/ponto/services/fechamento.py:616`) e usa esse instante para filtrar os furos entregues ao coletor (`app/ponto/services/fechamento.py:1120`). O filtro pergunta ao juiz com esse instante (`app/ponto/services/fechamento.py:377`), cujo resultado muda quando `agora >= fim` (`app/ponto/turnos.py:2140`). Cenário: dia sem par, fim previsto 19h, sem aberta viva; às 18:59 não afirma furo, às 19:01 afirma, sem alteração das batidas (`app/ponto/turnos.py:2132`). Isso pode contaminar a comparação entre rodadas; atribuição aos nove colaboradores fornecidos: [nao verificado].

Relógios fora da borda encontrados na cópia, com novidade [nao verificado]: fallbacks em `app/ponto/turnos.py:2113`, `app/ponto/turnos.py:2159`, `app/ponto/turnos.py:2307`, `app/ponto/turnos.py:2341`, `app/ponto/turnos.py:2475`, `app/ponto/turnos.py:2893`, `app/escala/utils.py:692` e `app/escala/utils.py:979`.

## 14/11 20:15 · sessao dfc90e576e7f · - · - · origem=Ronald (tmux)

**P4 — PROCEDENTE-candidato, dinheiro: há caminho concreto para falta inteira e DSR perdido com `minutos=0` e `cobre` vazio.** A reprodução em memória usou pareador real, trecho real de atribuição de dia do motor e ramo real de falta/DSR; execução integral com ORM permanece [nao verificado].

Cenário: template 22–06; DNA congelado D−1=22–06 e D=08–16, sem pausa, previsto de D=480. Batidas E22(D−1), S06(D), E08(D), S17(D), S18(D); `esqueci_entrada` aprovada na S18, `data_turno=D`. Ata somente com previsto, sem lâmpadas que substituam os papéis do pareador (`app/ponto/turnos.py:1217`; `app/ponto/motor_calculo_v2.py:488`).

O pareador real entrega `[22–06, D−1]`, `[08–17, D]` e `[S18 órfã, D]`; a órfã não produz período do motor (`app/ponto/turnos.py:1507`; `app/ponto/turnos.py:1512`; `app/ponto/adaptador_motor.py:30`).

O realizado de D é540; o prefixo justificado 08–16 cruza 08–17, portanto a fonte devolve `{cobre: frozenset(), minutos:0}` e exclui D de `justificadas` (`app/ponto/turnos.py:719`; `app/ponto/turnos.py:2739`; `app/ponto/turnos.py:2763`; `app/ponto/services/efeito_ausencia.py:46`).

O motor reatribui 08–17 a D−1 porque começa duas horas depois do fim anterior; a órfã S18 não deixa outro período em D (`app/ponto/motor_calculo_v2.py:2408`; `app/ponto/motor_calculo_v2.py:2412`; `app/ponto/adaptador_motor.py:36`). Assim, `datas_trabalho={D−1}`, D não está justificado, e o ramo real produziu `datas_falta=[D]`, `horas_falta=8` e `dsr_ok=False`; esse resultado incrementa semanas de DSR perdido (`app/ponto/motor_calculo_v2.py:2582`; `app/ponto/motor_calculo_v2.py:2605`; `app/ponto/motor_calculo_v2.py:2623`; `app/ponto/motor_calculo_v2.py:2666`).

A premissa do template importa: usando template08–16, o modo duração pode absorver S06/E08 como pausa antes da aplicação das bordas por dia; a reprodução confirmada usou template22–06, no qual S06 coincide com a saída prevista (`app/ponto/turnos.py:436`; `app/ponto/turnos.py:453`).

## 14/11 20:13 · sessao dfc90e576e7f · - · - · origem=Ronald (tmux)

Achados PROCEDENTE-candidato:

1. **Dinheiro/estrutura — a guarda de sobreposição ainda vence a lei do saldo.** Cadastro 07–19, previsto 720; batidas E01:10, E07:00, S11:00; `esqueci_saida` aprovada na E01:10. O pareador real forma `[01:10 aberta]` e `[07–11 fechado]` (`app/ponto/turnos.py:1433`, `:1507`). A regra pede **480 min**; a fonte devolve **`cobre=frozenset(), minutos=0`** porque a sobreposição remove `hf` (`app/ponto/turnos.py:2763`) e a fórmula só roda para cobertura None ou não vazia (`app/ponto/services/efeito_ausencia.py:137`). O dia sai também de `justificadas`, do mapa de minutos e da revisão (`app/ponto/services/efeito_ausencia.py:46`, `:47`, `:153`). Reprodução executada em memória com pareador e fonte reais; ORM/prod [nao verificado].

2. **Dinheiro/estrutura — realizado da justificativa e trabalhado do motor são contas distintas.** Cadastro 08–17, pausa fixa 12–13, previsto 480; E08, S16, E16:30, justificativa aprovada na última entrada. Pareador real: `[08–16 fechado]`, `[16:30 aberto]` (`app/ponto/turnos.py:1433`, `:1507`). `realizado_do_dia` desconta a pausa não batida: **420** (`app/ponto/turnos.py:760`); fonte paga **60** (`app/ponto/services/efeito_ausencia.py:145`). O corpo real de `Motor12x36.calcular_periodo`, com pontas dentro da janela e nenhuma pausa batida, produz **480 trabalhados**, porque desconta somente `_intra_real`, que vale zero (`app/ponto/motor_calculo_v2.py:2773`, `:2783`, `:2786`, `:2797`). Motor + justificativa = **540**, previsto 480; `horas_trabalhadas` soma esse trabalhado (`app/ponto/motor_calculo_v2.py:295`). A fonte nem marca revisão: bruto 30 + realizado 420 < 480 (`app/ponto/services/efeito_ausencia.py:146`). Executados pareador/fonte e corpo real do motor até a atribuição de trabalhado; execução integral ORM [nao verificado].

P1 — resultados concretos:

| Caso | Cadastro, batidas, justificativa | Regra / código |
|---|---|---|
| (a) | 08–16, previsto480, nenhuma batida; esqueci_saida aprovada sem âncora | **480 / 480**, `cobre=None`, `conserva=sem_ancora` (`app/ponto/turnos.py:2694`; `app/ponto/services/efeito_ausencia.py:129`, `:141`, `:145`). Divergência monetária nesse cenário isolado: não vi; `dias_abono` perde a contagem porque agora há chave no mapa (`app/ponto/motor_calculo_v2.py:2629`). |
| (b) | 06–13, previsto420; E03:54/S04:53, E06/S12:30, E12:45; esqueci_saida na última E | Trabalhado **449**, regra/código **0**, excedente29; fonte `hf`, minutos0, motivo de revisão (`app/ponto/turnos.py:719`, `:734`, `:770`; `app/ponto/services/efeito_ausencia.py:140`). Pareador real confirmou três turnos, todos do mesmo D (`app/ponto/turnos.py:1433`, `:1507`). |
| (c) | 08–16, previsto480, E08/S17, nenhuma justificativa | Crédito **0**; fonte devolve `{}` e não gera revisão, pois retorna antes de ler trabalho (`app/ponto/services/efeito_ausencia.py:98`, `:105`). Caminho HE examinado pelo outro explorer. |
| (d) | 22–06, previsto480; E20 solta, E22/S02(+1); esqueci_saida na E20 | Regra **240**; código **0**, `cobre` vazio, sem revisão: mesma guarda de sobreposição do achado1 (`app/ponto/turnos.py:2737`, `:2763`; `app/ponto/services/efeito_ausencia.py:137`). Pareador real confirmou aberta20 e fechada22–02, ambas D (`app/ponto/turnos.py:1433`, `:1507`). |

Noturno simples E22/S02(+1), E03(+1), justificativa na E03 com `data_turno=D`: fonte paga240; não vi divergência desse cenário isolado (`app/ponto/turnos.py:719`; `app/ponto/services/efeito_ausencia.py:196`, `:211`, `:145`).

P2 — dia/unidade: fonte usa `j.data_turno`, realizado filtra `t.data_turno==data` (`app/ponto/services/efeito_ausencia.py:118`; `app/ponto/turnos.py:719`). O motor lê o mapa do juiz, mas pode herdar o dia do período anterior por gap ≤6h (`app/ponto/motor_calculo_v2.py:2408`, `:2410`). Base usa dia da jornada no DSR; Comercial refaz o DSR por data civil (`app/ponto/motor_calculo_v2.py:2582`, `:3477`). Portanto identidade de dia em todos os leitores não está assegurada por esses caminhos; alcance produtivo [nao verificado].

P4 — distinções:

- `cobre=None` conserva cobertura; vazio exclui cobertura; rótulos conservam cobertura mesmo quando minutos0 (`app/ponto/services/efeito_ausencia.py:46`; `app/escala/utils.py:1251`, `:1258`).
- Grade conserva `min_justificados=0` nos ramos None/rótulos; com cobre vazio não entra em nenhum desses ramos (`app/escala/utils.py:1257`, `:1277`).
- Leitor da célula distingue presença da chave e transporta zero (`app/escala/services/leitor_celula.py:448`).
- Motor distingue `{D:0}` de `{}` por pertinência; `None` e `{}` são colapsados por `or {}` (`app/ponto/motor_calculo_v2.py:2628`, `:2629`, `:2634`).
- `dia_pago` usa truthiness e colapsa zero/None/ausente na formação do mapa de abono (`app/ponto/services/dia_pago.py:398`).
- **Não provei falta/DSR adicional causada apenas por cobre vazio em dia trabalhado acima do previsto**: Base inclui a data de todo período normal em `datas_trabalho`, independentemente dos minutos, e exige ausência dessa data e da justificativa para registrar falta (`app/ponto/motor_calculo_v2.py:2582`, `:2605`). A grade pode ficar sem `justificado` e manter furo, mas transformar esse furo em falta/DSR produtivo é [nao verificado].

## 14/11 20:09 · sessao b46ca7bab667 · - · - · origem=Ronald (tmux)

- **PROCEDENTE-candidato — estrutura/texto, P3:** “Revisar aprovadas” é segunda lista, sem ligação com a decisão de HE: calcula motivos em `app/ponto/views.py:1747`, acrescenta linhas em `:1753` e abre o painel da justificativa em `app/templates/ponto/fila_justificativas.html:154`; a porta de decisão efetiva permanece `app/ponto/portas/he.py:138`, com gravação em `:181` e recalculo em `:202`. Não vi segundo escritor de `DecisaoHE` nesses trechos.

  Cenário: escala 08–17, pausa12–13, previsto480, batidas E08/S18, trabalho540 e `esqueci_saida` aprovada na E08. A fonte conserva cobertura por turno fechado e devolve crédito0/motivo (`app/ponto/services/efeito_ausencia.py:128`, `:145`); o HE existente recebe a ponta60 (`app/ponto/services/espelho.py:722`, `:727`, `:776`; `app/ponto/services/he_pendente_lavrado.py:132`, `:148`). O mesmo dia entra também na revisão (`app/ponto/views.py:1747`). Sem justificativa, a revisão nem avalia o dia (`app/ponto/services/efeito_ausencia.py:98`, `:105`), embora o caminho existente de HE continue lendo a ponta (`app/ponto/services/he_pendente_lavrado.py:132`).

- **PROCEDENTE-candidato — texto/estrutura, P3:** o motivo/UI não significa necessariamente trabalho excedente: compara **bruto da justificativa + trabalho**, antes do limite, com previsto (`app/ponto/services/efeito_ausencia.py:143`, `:146`), mas a UI declara “Hora além do turno” (`app/templates/ponto/fila_justificativas.html:116`). O próprio caso420/59/pago361 exige esse motivo (`app/ponto/tests/test_justificativa_soma_sem_banco.py:178`, `:185`, `:188`): trabalho59 não excede previsto420.

- **PROCEDENTE-candidato — estrutura/desempenho, P5:** o request processa todas as aprovadas, sem competência/janela/paginação (`app/ponto/views.py:1707`, `:1714`, `:1732`), e cada colaborador recebe intervalo do menor ao maior dia histórico (`:1745`, `:1747`). A contagem estática típica é **8 + 2D consultas por colaborador**, com `D` dias cobertos, além de opcionais por cadastro:
  - 3 próprias: justificativas, células e vínculos (`app/ponto/services/efeito_ausencia.py:98`, `:109`, `:110`);
  - 4 no carregamento de turnos: batidas, células, candidatos e vínculo usado pela ata (`app/ponto/turnos.py:1879`, `:1902`, `:1903`, `:1955`; `app/escala/alimentacao.py:148`; `app/ponto/turnos.py:1609`, `:1574`);
  - normalmente1 DNA (`app/ponto/turnos.py:1928`, `:2005`; `app/escala/utils.py:100`);
  - 2 por dia com célula/geradora: `realizado_do_dia` recarrega vínculo/célula e pausa, pois o chamador fornece somente turnos/previsto (`app/ponto/services/efeito_ausencia.py:140`; `app/ponto/turnos.py:788`, `:794`; `app/escala/servico_jornada.py:46`; `app/escala/models.py:1187`).
  
  Há extras condicionais para geradora fora dos candidatos, agregação de última batida e dia sem geradora (`app/escala/alimentacao.py:157`; `app/ponto/turnos.py:1917`; `app/escala/servico_jornada.py:64`). Tempo real e timeout observado: **[nao verificado]**. A configuração é gthread/30s (`app/gunicorn.conf.py:19`, `:21`); o código registra que esse timeout não é teto rígido para request longo com gthread (`app/config/crons.py:1393`).

- **P6, não vi relógio novo no delta fornecido:** funções novas de cálculo/projeção/leitura não chamam `now()`/`today()` (`app/ponto/services/efeito_ausencia.py:98`, `:136`, `:151`; `app/escala/services/leitor_celula.py:354`). `tz.localdate()` da Central aparece como contexto preexistente no delta e alimenta SLA/cards (`app/ponto/views.py:1727`, `:1733`, `:1759`). O fechamento captura um instante vivo por rodada (`app/ponto/services/fechamento.py:616`), transmite-o ao filtro de furos (`:1120`) e consulta encerramento (`:377`; `app/ponto/turnos.py:2108`). Atribuir os nove furos de14/11 a isso, ou verificar novidade desde14b61c124: **[nao verificado]**.

## 14/11 20:08 · sessao 2dfc1fb51f5e · - · - · origem=Ronald (tmux)

P7/P8 — achados para consolidação:

1. **PROCEDENTE-candidato — texto: contrato antigo contradiz os testes atuais.** A docstring declara pagamento pelo período, C5=690, C7 sem justificativa e D1=420; as asserções atuais esperam respectivamente 720, cobertura inteira com 720 e 510. Referências: `app/ponto/tests/test_justificativa_paga_o_periodo.py:1`, `:17`, `:18`, `:20`, `:462`, `:492`, `:564`. D2/D3 dizem “nada se cobre”, porém esperam `cobre=None`, que representa cobertura inteira: `:496`, `:502`, `:509`, `:516`; semântica declarada em `app/ponto/services/efeito_ausencia.py:78`.

2. **PROCEDENTE-candidato — estrutura/texto: teste “sem perda DSR” não executa o cálculo semanal completo.** Ele extrai apenas o `If` que contém `_mj`, executa esse bloco e confere `dsr_ok`; não verifica `semanas_dsr_perdido`. Referências: `app/ponto/tests/test_justificativa_soma_sem_banco.py:262`, `:267`, `:273`, `:276`, `:277`. O contador real está depois do bloco e existe outro cálculo em `MotorComercial`: `app/ponto/motor_calculo_v2.py:2641`, `:2667`, `:3474`, `:3490`, `:3515`. Prova executada somente em memória: mudar `:3474` para iniciar `semanas_dsr_perdido=1` mantém os 24 testes `SomaDoDiaTest` verdes; resultado de execução sem arquivo persistido [nao verificado].

3. **PROCEDENTE-candidato — texto: “bordas ORM substituídas” omite substituição de autoridade de domínio.** A docstring diz isso em `app/ponto/tests/test_justificativa_soma_sem_banco.py:5`, mas `turnos_do_colab` inteiro é substituído por `self.ts` em `:43`, e os turnos são fabricados usando o tipo gravado para decidir entrada/saída em `:53`–`:58`. Assim, esses testes não exercitam o pareamento real, a atribuição do dia pelo envelope nem a aplicação real da L-087. Referências: `:43`, `:57`, `:58`.

4. **PROCEDENTE-candidato — texto: nome da propriedade promete uma desigualdade que o próprio teste permite violar.** `test_propriedade_coberto_implica_soma_dentro_do_previsto` só registra violação quando o crédito é positivo. Com previsto=100, trabalho=120 e crédito=0, a cobertura permanece e a soma=120 passa pelo teste — comportamento compatível com a lei atual, mas incompatível com o nome. Referências: `app/ponto/tests/test_justificativa_soma_sem_banco.py:163`, `:165`, `:168`, `:172`, `:174`.

**Mutantes declarados:** não vi mutante declarado que faça algo incompatível com seu nome. Li as 12 substituições em `app/ponto/tests/mutantes_justificativa_soma.py:19`–`:56`, o mecanismo de carregamento em `app/ponto/tests/test_justificativa_periodo_v2_sem_banco.py:27`–`:43` e o critério de morte em `app/ponto/tests/mutantes_justificativa_soma.py:94`. Executei `--todos`: controle com 24 testes; 12/12 mortos por asserção; retorno 0. Resultado de execução sem arquivo persistido [nao verificado]. Nenhum desses 12 muta o pareador, o motor semanal ou a porta de HE: `app/ponto/tests/mutantes_justificativa_soma.py:19`–`:56`.

Não executei a suíte ORM; seus resultados permanecem [nao verificado]. [nao verificado]

## 14/11 20:06 · sessao 93faa118c4dc · - · - · origem=Ronald (tmux)

Censo somente leitura; nenhuma edição nem execução com banco. O recorte contém **65 métodos únicos**, correspondendo às **70 ocorrências**: cinco subtests A/S acrescentam quatro ocorrências; dois subtests R2 acrescentam uma. Isso corresponde aos 69 failures + 1 error fornecidos. As linhas abaixo são da base inspecionada.

Classificação proposta: **60 métodos (a)**, **1 método (b), com cinco subtests**, e **4 métodos (c)**. A exclusão dos quatro métodos (c) depende da remoção efetiva do balde e do censo exigido por L-111.

| Arquivo | Método | Classe | Adaptação proposta |
|---|---|---|---|
| `chamados/tests/test_o261_vigia_fio.py:624` | `test_MORDE_a_lista_e_perguntada_no_UNIVERSO_DA_TELA` | a | Usar dia efetivamente sem porta; a fixture atual publica residual `decidir`, evitando a lista. |
| mesmo arquivo`:418` | `test_a_abertura_por_PERGUNTA_soma_o_universo_da_PORTA` | a | Adequar contagem aos dias que continuam sem porta após o juiz. |
| mesmo arquivo`:455` | `test_a_LISTA_com_dono_leva_a_CLASSE_de_cada_dia_sem_precedencia` | a | Preservar classificação da lista apenas para dias sem porta publicada. | [nao verificado]
| mesmo arquivo`:727` | `test_as_linhas_do_log_NAO_LEVAM_NOME_de_ninguem` | a | Preservar ausência de PII; ajustar `FIO_MORTO=1` à fixture sem porta. |
| mesmo arquivo`:680` | `test_as_PARTES_SOMAM_o_total_e_o_fino_e_subconjunto` | a | Tirar parcela do balde extinto; Resolver oferecido integra `fio_vivo`. |
| mesmo arquivo`:368` | `test_as_TRES_classes_de_DIA_somam_o_fio_morto` | a | Classes contam apenas dias sem porta, não residual `decidir`. |
| mesmo arquivo`:280` | `test_dia_SEM_pergunta_nenhuma_e_a_classe_mais_crua` | a | Sem pergunta com `decidir` tem porta; inverter consequência ou construir ausência real de porta. |
| mesmo arquivo`:557` | `test_MORDE_aguardando_DOCUMENTO_tambem_e_bola_do_DP` | a | Manter contador LU-04/R3, com fixture realmente sem porta. |
| mesmo arquivo`:224` | `test_MORDE_a_property_viva_LE_o_juiz_e_NAO_tem_regra_propria` | a | Preservar asserts C6/property; consequência no vigia agora considera residual com porta. |
| mesmo arquivo`:540` | `test_MORDE_ausencia_ABERTA_cobrindo_o_dia_AGUARDA_O_DP_e_nao_e_fio_morto` | a | Ausência decidível tem ação/porta; balde permanece somente para ausência sem porta. |
| mesmo arquivo`:513` | `test_MORDE_chamado_em_analise_SEM_resolver_dia_e_FIO_MORTO` | a | Manter teste vivo; `decidir` residual alcançável implica fio vivo; retirar só assert do balde apagado. | [nao verificado]
| mesmo arquivo`:586` | `test_MORDE_dono_BATIDA_ou_CADASTRO_NA_LISTA_e_fio_do_admin_e_o_resto_fica` | a | Manter subdivisão do sem-porta e testar com dias sem porta. |
| mesmo arquivo`:614` | `test_MORDE_falha_da_lista_e_UMA_chamada_por_rodada_e_todos_ficam` | a | Preservar custo/falha da lista com fixture que efetivamente chega à lista. |
| mesmo arquivo`:659` | `test_MORDE_folga_CONTESTADA_e_validada_e_ESCALA_NAO_DESCREVE_O_DIA_e_nao_fio_morto` | a | Preservar subdivisão; porta residual prevalece antes do contador. |
| mesmo arquivo`:789` | `test_MORDE_o_carimbo_guarda_os_CONTADORES_que_a_rodada_mediu` | a | Preservar comparação com censo; mudar mistura e contadores antigos, passar instante do carimbo também ao censo. |
| mesmo arquivo`:163` | `test_MORDE_o_dia_de_HOJE_fica_FORA_do_contador_alarmado` | a | Testar teto somente em dia sem porta, pois porta válida vence essa classificação. |
| mesmo arquivo`:485` | `test_MORDE_o_dia_que_OFERECE_resolver_dia_e_DECISAO_PENDENTE_e_nao_fio_morto` | c | Exclusivo do balde; remover com L-111; cobertura nova M4 prova porta administrativa. |
| mesmo arquivo`:389` | `test_MORDE_o_dia_SO_de_plantio_fora_do_slot_NAO_e_ALVO` | a | ALVO continua vivo; montar universo sem porta ou adequar contagem. |
| mesmo arquivo`:128` | `test_MORDE_o_fio_vivo_e_o_fio_morto_dao_CONTAS_DIFERENTES` | a | Pergunta morta não remove porta residual `decidir`; escolher par que difira na porta publicada. | [nao verificado]
| mesmo arquivo`:521` | `test_MORDE_o_passivo_ALVO_nao_some_dentro_da_decisao_pendente` | c | Exclusivo do balde e de seu contador ALVO; remover com balde. |
| mesmo arquivo`:197` | `test_MORDE_o_VOCABULARIO_do_cartorio_e_quem_diz_quais_dias_cobram` | a | Preservar vocabulário e recorte; adaptar fixture à porta válida. |
| mesmo arquivo`:145` | `test_MORDE_registrado_e_VIVO_mas_nao_COBRA_ninguem` | a | `registrado`/`historico` não dão porta LU-02; caso aberto com `decidir` dá porta. |
| mesmo arquivo`:404` | `test_o_dia_MISTO_conta_UMA_vez_e_no_ALVO` | a | Preservar unidade colab-dia/ALVO; contar apenas sem porta. |
| mesmo arquivo`:564` | `test_o_dia_que_OFERECE_resolver_dia_fica_na_DECISAO_DO_DIA_mesmo_com_ausencia_aberta` | c | Exclusivo da precedência do balde extinto. |
| mesmo arquivo`:495` | `test_o_juiz_REAL_oferece_o_dia_que_acusa_sem_lavra` | c | Assert exclusivo do balde; a integração real útil pode migrar para M4/M8. |
| mesmo arquivo`:606` | `test_sem_resposta_da_lista_o_dia_FICA_no_alarme` | a | Preservar falha explícita, com entrada sem porta. |
| `chamados/tests/test_e3_tarefa_admin_sem_banco.py:133` | `test_B_252_29_set_hipotese_ultima_resposta_textual` | a | Âncora existente fora da fila dá `admin/porta_acao/validar`. |
| mesmo arquivo`:136` | `test_B_841_10_out_hipotese_ultima_resposta_textual` | a | Mesma inversão LU-03. |
| `chamados/tests/test_e3_c8b_sem_banco.py:196` | `test_carga_da_tarefa_pergunta_ao_c6_avulso_lote_e_idempotencia` | a | Carga passa âncoras existentes: esperar `admin/porta_acao`, preservar custo e idempotência. |
| mesmo arquivo`:48` | `test_cinco_formas_acusadoras_sem_pergunta_viva` — cinco subtests | b | A/S sem oferta de Resolver não dá dono; manter `nada`, curar juiz e acrescentar controle oferecido. |
| mesmo arquivo`:166` | `test_grave_respondida_em_disputa_fechada_nao_e_filtrada_por_motivo` | a | Passar mapa de âncoras existente e esperar administração alcançável. |
| mesmo arquivo`:184` | `test_sem_trabalho_sem_lampadas_nao_esconde_resposta_viva` | a | Mesma inversão, preservando exposição da pergunta/marco. |
| `chamados/tests/test_e4_passivo.py:57` | `test_classe_c_passivo_entra_pela_porta_da_E1_e_2x_e_uma_linha` | a | Após registrar resposta E1, passa a `admin/porta_acao`; segunda rodada não seleciona mais sem-porta; preservar uma linha E1. |
| `chamados/tests/test_o261e_f5_alcance.py:484` | `test_a_ancora_VIVA_fora_da_fila_tambem_e_veu` | a | Resposta ancorada em registrado dá `porta_acao/admin`, sem causa sem-porta. |
| mesmo arquivo`:213` | `test_c_respondida_com_destino_FECHADO` | a | Âncora terminal existente também dá modal: `porta_acao/admin`. |
| `chamados/tests/test_e3_portas_tarefa.py:465` | `test_fio_normal_e_do_dono_da_acao_mesmo_com_verbo_validar` | a | Com app: `porta_app/colab`; sem app: preservar admin. |
| mesmo arquivo`:483` | `test_historico_tem_porta_mas_nao_e_nomeado_na_lista_padrao` | a | LU-02: projeção `None`, sem dono; manter verbo como evidência. |
| mesmo arquivo`:419` | `test_RED_A_na_fila_aparece_com_acao_sob_B` | a | Com app: `porta_app`; sem app: `porta_fila/chamado`; manter fila contextual. |
| mesmo arquivo`:115` | `test_tarefa_propaga_relogio_a_alimentacao` | a | A pergunta pendente no app continua dando `porta_app`; manter verificação do verbo/instante. |
| mesmo arquivo`:505` | `test_v8_R1_colab_alcancavel_e_leitores` | a | Trocar `porta_pergunta` por `porta_app`; todos os leitores seguem publicação. |
| mesmo arquivo`:231` | `test_v8_R2_alcance_admin_com_pergunta_colab` | a | `porta_app/colab`, preservando verbo administrativo contextual. |
| mesmo arquivo`:525` | `test_v8_R2_validacao_e_resposta_alcancaveis_no_mesmo_dia` — dois subtests | a | Ambos `porta_app/colab`, sem alterar ações originais. |
| mesmo arquivo`:265` | `test_v8_R7_membro_da_fila_sem_verbo` | a | LU-02: None não dá dono/porta. |
| mesmo arquivo`:572` | `test_v8_R7_membro_sem_verbo_conservado` | a | Alimentação conserva membro/None; projeção da tarefa é `None`. |
| `chamados/tests/test_o261e_f5b_juiz_porta.py:259` | `test_MORDE_ancora_arquivada_nao_abre_a_porta_da_pergunta` | a | C17 histórico pode continuar sem porta; tarefa/Folha dão `admin/porta_acao/validar`. |
| mesmo arquivo`:276` | `test_MORDE_membro_da_fila_que_a_particao_nao_viu_nao_abre_porta` | a | C17 histórico mantém assert próprio; tarefa/Folha passam a sem porta por LU-02. |
| mesmo arquivo`:135` | `test_MORDE_ricardo_a_folha_le_o_juiz_e_diz_nada` | a | Folha deve ler `admin`, pois há âncora existente para validar. |
| mesmo arquivo`:140` | `test_MORDE_ricardo_folha_e_vigia_dizem_a_MESMA_coisa` | a | Ambos leem `porta_acao/admin`, sem causa c. |
| `escala/tests/test_f3_beco1_folga_na_lista.py:137` | `test_MORDE_competencia_anterior_NAO_exportada_vai` | a | Resposta ancorada tem modal; C11 vazio. |
| mesmo arquivo`:351` | `test_MORDE_F5_queries_nao_crescem_com_os_dias` | a | Preservar custo; ajustar cardinalidade dos resultados à exclusão por porta. |
| mesmo arquivo`:199` | `test_RED_contraex1_chamado_da_celula_na_fila_sem_pergunta_NAO_e_porta` | a | Porta residual `decidir/chamado` permanece; atualizar projeção com `acao`, se publicada. | [nao verificado]
| mesmo arquivo`:219` | `test_RED_contraex2_ancora_viva_fora_da_fila_nao_e_porta` | a | Agora `admin/porta_acao`, C11 vazio. |
| mesmo arquivo`:100` | `test_RED_dia_da_competencia_aberta_vai_a_lista_com_dono_CADASTRO` | a | Fixture respondida/ancorada já tem modal; esperar porta e ausência no C11. |
| mesmo arquivo`:244` | `test_RED_F1_lista_le_o_instante_de_quem_chama` | a | Preservar instante explícito e sua propagação, sem exigir o C11 antigo. |
| mesmo arquivo`:292` | `test_RED_F6_C9_lavrado_divergente_nao_tira_o_dia_CADASTRO_da_mesa` | a | Preservar dono C9, retirar expectativa de C11 para dia que tem porta administrativa. |
| `chamados/tests/test_f3_beco1_reata_limbo.py:67` | `test_MORDE_dry_nao_escreve` | a | Dia já tem porta: zero candidatos, nenhuma escrita. |
| mesmo arquivo`:149` | `test_MORDE_pai_cujo_silencio_ja_venceu_volta_a_fila_e_reata` | a | Âncora atual já fornece modal; não há candidato a reata nesse cenário. |
| mesmo arquivo`:76` | `test_MORDE_sem_pai_fica_na_lista_e_nenhum_pai_nasce` | a | Não cria pai; dia com modal também não entra C11. |
| mesmo arquivo`:49` | `test_RED_apply_reata_ao_pai_e_o_dia_sai_do_beco` | a | Já sai pelo modal: zero reatas, destino preservado. |
| mesmo arquivo`:124` | `test_RED_pai_silenciado_DEPOIS_da_selecao_nao_recebe_reata` | a | Nenhuma seleção nesse cenário; preservar destino sem reata. |
| mesmo arquivo`:105` | `test_RED_pai_silenciado_fora_da_fila_nao_recebe_reata` | a | Sem seleção/reata; excluir expectativa de C11 para dia com modal. |
| `chamados/tests/test_e3_bloqueantes.py:179` | `test_RED_8_pergunta_sob_ancora_REGISTRADA_nao_e_canal` | a | Assert agora exige `admin/porta_acao`; furo não inclui dia. |
| mesmo arquivo`:170` | `test_RED_8_pergunta_sob_ancora_SILENCIADA_nao_e_canal` | a | Mesma inversão LU-03. |
| `chamados/tests/test_n1_perguntas_do_app.py:383` | `test_RED_censo_dono_fora_do_fio_le_uma_instancia_do_colab` | a | Fixture tem três `decidir`: não chega ao balde; verificar zero chamadas ou construir entrada sem porta para continuar testando identidade. |
| `chamados/tests/test_sombra_c1_resolver_em_lote.py:35` | `test_RED_censo_monta_um_calendario_por_colaborador` | a | Três dias com residual `decidir`: `fio_vivo=3`, zero calendários; acrescentar entrada sem dono para prova positiva uma vez por colaborador. |

Pontos adicionais fora do recorte:

- `chamados/tests/test_e3_c8b_sem_banco.py:81`: schema exato precisa incluir `acao`, caso publicada na tarefa.
- `chamados/tests/test_o261_vigia_fio.py:483`: stub plural `lambda colab, datas` precisa aceitar `agora` keyword.
- `chamados/tests/test_o261e_f5_alcance.py:293`: teste exclusivo do balde extinto, elegível a remoção L-111.
- `chamados/tests/test_o261e_f5_alcance.py:406`: teste misto continua vivo; oferta agora deve resultar `porta_acao/admin`, sem papel inventado `regularizar_batida`.
- `chamados/tests/test_sombra_c1_resolver_em_lote.py:49`: a prova de custo deve acompanhar a carga da tarefa; forçar `porta_da_tarefa=None` depois do julgamento já não força consulta ao Resolver.

Os B sem banco possuem âncoras existentes em `forma_b`, mas os testes diretos não passam o mapa ao juiz (`chamados/tests/test_e3_tarefa_admin_sem_banco.py:103`, `chamados/tests/test_e3_tarefa_admin_sem_banco.py:127`). Para provar LU-03, passar `{p.pk: chamado_que_lista(p)}`; `_pela_decisao` atualmente reduz a porta a None fora da fila (`chamados/services/tarefa_do_dia.py:140`).

Comentários V6: substituir a restrição “admin só chega por chamado na fila” por **LU-03: ação administrativa com tela dá porta, inclusive validação com âncora existente fora da fila**; None sem âncora não dá dono. Pontos: `test_e3_bloqueantes.py:97`, `test_o261e_f5_alcance.py:487`, `test_o261e_f5b_juiz_porta.py:264`, `folha/tests/test_haiku_fora_da_folha.py:160`.

Comando sem banco recomendado:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest chamados.tests.test_e3_tarefa_admin_sem_banco chamados.tests.test_e3_c8b_sem_banco chamados.tests.test_e3_degrau_dono_e_porta
```

Não executei esse comando para evitar disputar o RED/GREEN do construtor. Os demais arquivos listados importam `django.test.TestCase` e suas fixtures escrevem no banco; execução proibida nesta sessão (`chamados/tests/test_o261_vigia_fio.py:24`, `escala/tests/test_f3_beco1_folga_na_lista.py:18`, `chamados/tests/test_f3_beco1_reata_limbo.py:17`).

## 14/11 20:04 · sessao 0a6e6dad6330 · - · - · origem=Ronald (tmux)

Censo realizado somente por leitura.

- `vigia_fio.fio_do_dia` não tem chamador de produção encontrado: a busca encontra somente sua definição em `chamados/services/vigia_fio.py:156`. Os homônimos da Folha e do serviço de veredito são funções distintas. Comando:  
  `git grep -n -E '(vigia_fio import.*fio_do_dia|vigia_fio\.fio_do_dia|fio_do_dia\()' -- '*.py' ':!*/tests/*' ':!*/migrations/*'`

- `_resolver_da_rodada` tem somente os dois chamadores internos dos censos, em `chamados/services/vigia_fio.py:440` e `chamados/services/vigia_fio.py:661`; sua definição fica em `chamados/services/vigia_fio.py:258`. Depois de transferir a consulta para a carga da tarefa e remover essas chamadas, cabe apagá-lo com lápide L-111.

- O balde `decisao_do_dia_pendente` não tem consumidor de produção fora de `vigia_fio.py`: inicialização `:365`, classificação `:443`, passivo `:494`, enumeração `:547` e impressão `:906`. Comando:  
  `git grep -n 'decisao_do_dia_pendente' -- '*.py' ':!*/tests/*' ':!*/migrations/*'`

- `aguarda_decisao_de_ausencia`, `escala_nao_descreve_o_dia` e `fio_do_admin` são atualmente subdivisões dos dias sem porta: o censo já passou pela guarda da porta em `chamados/services/vigia_fio.py:428`; o alcance passou pela guarda em `chamados/services/vigia_fio.py:669`. As classificações em `chamados/services/vigia_fio.py:308` não escrevem campos da tarefa. Os comentários ainda atribuem “bola” e deveriam declarar explicitamente essa limitação LU-03.

- As duas construções avulsas de `ListaDoAdmin` perdem o instante em `chamados/services/vigia_fio.py:391` e `chamados/services/vigia_fio.py:645`. Devem receber `agora=agora`. A classe já encaminha `self.agora` em `chamados/services/vigia_fio.py:216`; o default de `escala/services/cadastro_realidade.py:775` deixa de ser acionado nesse caminho.

Leitores encontrados:

| Leitor | Uso atual |
|---|---|
| `folha/fora_da_folha.py:244` | Usa `fila` como números de chamados exibidos; a documentação `:241` declara chamados exigíveis da célula. |
| `folha/retrato_folha.py:1190` | Grava fio agregado como `dono`; `portas_da_linha`, `:1015`, consulta mapas de fio/tipo. |
| `chamados/services/vigia_fio.py:412` | Testa presença; o alcance `:673` conta rota e `:675` publica fio. |
| `chamados/services/furo_sem_canal.py:97` | Testa presença e conta rota em `:99`. |
| `escala/services/cadastro_realidade.py:469` | Exclui o beco quando existe porta. |
| `ponto/services/pautas_dos_discordantes.py:81` | Lê fio por projeção da Folha. |
| `chamados/management/commands/diff_e3_sombra.py:50` | Diagnóstico de `fila`; `:143` imprime rota, fio e tamanho. |

Nenhum deles interpreta `acao` atualmente, pela busca textual acima. Incidência dinâmica: `[não verificado]`.

Para M5, conservar `fila=(101,)` faz a Folha exibir 101 ao lado da porta administrativa da pergunta 202 (`folha/fora_da_folha.py:244`). Recomendo alinhar `fila` aos chamados da célula pertencentes às portas do dono e passar âncora/ação separadamente para produzir o link correto.

Consequências nos testes: [nao verificado]

| Teste/local | Tratamento sugerido | [nao verificado]
|---|---|
| `test_o261_vigia_fio.py:485` | Classe c: comportamento específico do balde removido; M4 passa a provar porta administrativa. |
| `test_o261_vigia_fio.py:521` | Classe c: contador `decisao_do_dia_pendente_com_alvo` removido. |
| `test_o261_vigia_fio.py:564` | Classe c quanto à precedência do balde; M4/M6 devem preservar a cobertura de porta. |
| `test_o261_vigia_fio.py:495` | Preservar: prova o juiz real; inverter resultado para `fio_vivo`, sem balde. |
| `test_o261_vigia_fio.py:513` | Preservar a contraprova sem oferta; retirar somente consulta à chave apagada. |
| `test_o261_vigia_fio.py:680` | Preservar soma; retirar termo do balde e contar o dia oferecido em `fio_vivo`. |
| `test_sombra_c1_resolver_em_lote.py:35` | Preservar desempenho; trocar termo do balde por `fio_vivo`. |
| `test_sombra_c1_resolver_em_lote.py:70` | Preservar paridade e fragmentação da plural, que continua viva. |
| `test_n1_perguntas_do_app.py:383` | Fixar oferta falsa para os dias chegarem ao subdivisor; a transferência da consulta para a tarefa muda o caminho. |

A fixture `_oferece` em `chamados/tests/test_o261_vigia_fio.py:483` precisa aceitar `**kw` para receber `agora`. A constante `PERMISSAO_DO_RESOLVER_DIA` tem teste de tela em `chamados/tests/test_o261e_f5_alcance.py:451`; não removê-la inadvertidamente junto do balde sem adaptar essa prova viva.

## 14/11 20:02 · sessao 941f6ed772c9 · JUSTIFICATIVA-PAGA-O-PERIODO · construtor · origem=Code

Cura escrita na fonte: `pago = max(0, previsto - trabalhado_do_dia)`, preservando `cobre` e nomeando o excedente para revisão. O bruto informa o motivo; não impõe outro teto (`ponto/services/efeito_ausencia.py:138`, `ponto/services/efeito_ausencia.py:145`).

**Q1.** O motor recebe o crédito pelo mapa alimentado em `ponto/services/fechamento.py:755`, `ponto/services/espelho.py:1134` e `relatorios/pdf_espelho.py:407`. Quando um dia justificado está ausente desse mapa, o motor usa o previsto inteiro (`ponto/motor_calculo_v2.py:2629`).

Com `cobre` vazio, a fonte exclui o dia de `justificadas`; portanto, os dados fornecidos não demonstram como os 420 chegaram ao motor. Além disso, o campo medido `minutos_abonados` é gravado da grade, por `por_dia_da_grade`, em `ponto/services/fechamento.py:1066`. A origem exata dos 420 produtivos permanece **[nao verificado]**. A cura utiliza o feed existente, sem editar o motor (`ponto/services/efeito_ausencia.py:47`).

**Q2.** A recusa integral podia retirar a justificativa usada pelo DSR. O leitor exige que o dia obrigatório tenha trabalho reconhecido ou esteja em `datas_justificadas`; não exige pagamento integral (`ponto/motor_calculo_v2.py:2605`, `ponto/motor_calculo_v2.py:3490`). O teste local executa o ramo real do motor com crédito 361, sem falta e com `dsr_ok=True` (`ponto/tests/test_justificativa_soma_sem_banco.py:262`). A explicação exata do 4→3 produtivo permanece **[nao verificado]**.

**Q3.** Existe `decidir_he`, destinada a minutos reais fora da janela (`ponto/portas/he.py:138`). A Gestão de HE lê essas pontas do retrato (`ponto/services/he_pendente_lavrado.py:19`). Para o excedente da justificativa, acrescentei a projeção `justificativas_para_revisao`, consumida pela Central no filtro “Revisar aprovadas”, uma linha por dia-colaborador (`ponto/services/efeito_ausencia.py:151`, `ponto/views.py:1738`, `templates/ponto/fila_justificativas.html:56`).

**Q4.** `cobre` mantém os marcos; somente o crédito muda. A grade dispensa os marcos cobertos e transporta o número; `dia_pago` soma esse número sem exigir a duração integral do período (`escala/utils.py:1266`, `escala/utils.py:1277`, `ponto/services/dia_pago.py:398`). Cobertura inteira também recebe mapa explícito, inclusive zero, evitando o fallback do motor (`ponto/services/efeito_ausencia.py:47`).

| Campo | Quem escreve/fornece | Quem lê |
|---|---|---|
| `previsto` | Ata: `ponto/services/cartorio.py:739`; leitor comum: `escala/services/leitor_celula.py:354` | `ponto/services/efeito_ausencia.py:138` |
| `trabalhado` | Autoridade `realizado_do_dia`: `ponto/turnos.py:774`; sem turno retorna `None`: `ponto/turnos.py:721` | Adaptação para zero e conta: `ponto/services/efeito_ausencia.py:141` |
| `cobre` | Cobertura e união existentes: `ponto/services/efeito_ausencia.py:125`, `ponto/services/efeito_ausencia.py:133` | Efeito: `ponto/services/efeito_ausencia.py:46`; grade: `escala/utils.py:1257` |
| `minutos` | Saldo diário: `ponto/services/efeito_ausencia.py:145` | Feed: `ponto/services/efeito_ausencia.py:47`; grade: `escala/utils.py:1277` |
| `motivo` | `ponto/services/efeito_ausencia.py:147` | Projeção: `ponto/services/efeito_ausencia.py:153`; Central: `ponto/views.py:1747`; apresentação: `templates/ponto/fila_justificativas.html:116` |
| Feed do motor | Três chamadores citados em Q1, incluindo órfão em `ponto/services/fechamento.py:831` | Abono: `ponto/motor_calculo_v2.py:2628` |

Outros alimentadores permanecem lendo o mesmo mapa: esmeril (`ponto/services/esmeril_espelho.py:166`), extrato (`relatorios/management/commands/diff_extrato_parcial.py:62`) e comparação de alimentação (`ponto/management/commands/diff_espelho_alimentacao.py:124`).

Diff por arquivo::função:

- `efeito_ausencia.py::periodos_justificados`: substitui recusa integral pelo saldo diário; trata ausência de turno (`ponto/services/efeito_ausencia.py:136`).
- `efeitos_da_ausencia` e `utils.py::_marcar_ausencias`: transportam crédito explícito para cobertura inteira (`ponto/services/efeito_ausencia.py:47`, `escala/utils.py:1257`).
- `views.py::fila_justificativas` e template: revisão pelo motivo da fonte (`ponto/views.py:1738`, `templates/ponto/fila_justificativas.html:116`).
- Testes: S1–S6 invertidos, T1/T7, propriedades, feed real, ramo do motor e Central (`ponto/tests/test_justificativa_soma_sem_banco.py:83`, `ponto/tests/test_justificativa_soma_sem_banco.py:163`, `ponto/tests/test_justificativa_soma_sem_banco.py:185`).
- Caracterizações anteriores ajustadas à fórmula nova; ORM escrito, com relógio fixado (`ponto/tests/test_justificativa_periodo_v2_sem_banco.py:209`, `ponto/tests/test_justificativa_paga_o_periodo.py:600`).

RED antes da implementação: **20 testes, 15 falhas por asserção**, preservado em [justificativa_soma_red.txt](/home/ronald/wt-codex-justificativa-paga-o-periodo-164913/app/ponto/tests/justificativa_soma_red.txt:179). [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest ponto.tests.test_justificativa_soma_sem_banco -v
```

GREEN local: **39 testes passando**, em [justificativa_soma_green.txt](/home/ronald/wt-codex-justificativa-paga-o-periodo-164913/app/ponto/tests/justificativa_soma_green.txt:42). [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest ponto.tests.test_justificativa_soma_sem_banco ponto.tests.test_justificativa_periodo_v2_sem_banco -v
```

Mutantes: **controle verde e 12/12 mortos por asserção**. Os comandos individuais e resultados estão em [justificativa_soma_mutantes.txt](/home/ronald/wt-codex-justificativa-paga-o-periodo-164913/app/ponto/tests/justificativa_soma_mutantes.txt:1). [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_justificativa_soma recusa_integral_c5
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_justificativa_soma sem_max_zero
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_justificativa_soma so_turno_da_ancora
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_justificativa_soma feed_motor_le_bruto
PYTHONDONTWRITEBYTECODE=1 python3 -m ponto.tests.mutantes_justificativa_soma --todos
```

ORM escrito para a sessão executar; resultado **[nao verificado]**:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test ponto.tests.test_justificativa_paga_o_periodo --settings=config.settings.ci
```

Não executei ORM, banco, rede, Docker, reconstrução, commit, merge, push, deploy ou crons. Não alterei motor, portas protegidas, modelos, migrations ou docs. Certificação independente dessas restrições: **[nao verificado]**. Frota dos 41, contagem produtiva por motivo, 9 colaboradores da competência 11 e exportadas: **[nao verificado]**. A certificação fica com Code/suíte e opus final. [nao verificado]

`LEI-AKITA: origem=Ronald 14/11 19:4x (ponto/services/efeito_ausencia.py:83); testemunha=feed real PDF e ramo real motor (ponto/tests/test_justificativa_soma_sem_banco.py:242, :262); RED=ponto/tests/justificativa_soma_red.txt:181; quem-mais-le=grade/fechamento/espelho/PDF/esmeril/extrato; juizes novos=0; padrao=single source of truth / um juiz por pergunta (docs/PLANO-PADROES.md:46).`

## 14/11 19:53 · sessao e037f87dec36 · E3-V10-DEGRAU-DONO-E-PORTA · verificador · origem=Code

**Parecer v2: há achados PROCEDENTES em LU-03 e LU-07.** A publicação de `porta_acao` aceita qualquer ação administrativa exigível, sem conferir sua tela; a classificação entre `porta_fila` e `porta_pergunta` ainda usa portas de outros chamados do dia. (`app/chamados/services/tarefa_do_dia.py:606`, `app/chamados/services/tarefa_do_dia.py:627`)

1. **Escritores, leitores e aceitação de `porta_acao`.**

   | Campo/caminho | Escritor ou leitor encontrado |
   |---|---|
   | `fio`, `rota`, `fila`, `verbos` da tarefa | Escrita final em `_portas_da_tarefa`; `porta_da_tarefa` apenas projeta esses campos. (`app/chamados/services/tarefa_do_dia.py:630`, `app/chamados/services/tarefa_do_dia.py:640`) |
   | `donos` | É variável local do juiz, não campo publicado da tarefa. (`app/chamados/services/tarefa_do_dia.py:608`, `app/chamados/services/tarefa_do_dia.py:630`) |
   | API do app | Lê `consulta_do_app`, sem consumir `rota`/`fio` da tarefa nesses endpoints. (`app/api/views.py:993`, `app/api/views.py:2533`) |
   | `fora_da_folha` | Lê a porta, extrai `fio` e usa `fila` para os números dos chamados. (`app/folha/fora_da_folha.py:67`, `app/folha/fora_da_folha.py:82`, `app/folha/fora_da_folha.py:244`) |
   | `retrato_folha` | Recebe a agregação da Folha e grava o fio na chave histórica `dono`; o leitor usa esse valor gravado. (`app/folha/retrato_folha.py:1158`, `app/folha/retrato_folha.py:1190`, `app/folha/retrato_folha.py:438`) |
   | `vigia_fio` | `censo` testa presença da porta; `censo_alcance` lê sua rota e seu fio. (`app/chamados/services/vigia_fio.py:428`, `app/chamados/services/vigia_fio.py:669`) |
   | `furo_sem_canal` / lavrador | Presença da porta define canal; os quatro contadores de rota são calculados e impressos. (`app/chamados/services/furo_sem_canal.py:97`, `app/chamados/services/furo_sem_canal.py:114`, `app/chamados/management/commands/lavrar_furo_sem_canal.py:37`) |
   | `cadastro_realidade` | Exclui o beco quando a tarefa publica qualquer porta. (`app/escala/services/cadastro_realidade.py:464`, `app/escala/services/cadastro_realidade.py:469`) |
   | Pauta dos discordantes | Usa o fio projetado pela Folha e compara com `FIO_SEM_PORTA`. (`app/ponto/services/pautas_dos_discordantes.py:64`, `app/ponto/services/pautas_dos_discordantes.py:81`) |
   | Diff sombra | Compara a projeção nova com o C17 histórico. (`app/chamados/management/commands/diff_e3_sombra.py:43`, `app/chamados/management/commands/diff_e3_sombra.py:127`) |
   | Relatórios e templates examinados | As telas de pendências usam suas fontes de classificação; o template do retrato recebe frases e links prontos, sem interpretar a enumeração da tarefa. (`app/relatorios/views_pend.py:23`, `app/relatorios/views_pend.py:91`, `app/templates/folha/_retrato_o_que_fazer.html:33`, `app/templates/folha/_retrato_o_que_fazer.html:72`) |

   **NÃO PROCEDENTE — `porta_acao` provocar KeyError ou desaparecer dos contadores identificados.** O alcance inicializa as quatro rotas antes do incremento dinâmico; furo e lavrador também incluem app e ação. (`app/chamados/services/vigia_fio.py:587`, `app/chamados/services/vigia_fio.py:673`, `app/chamados/services/vigia_fio.py:741`, `app/chamados/services/furo_sem_canal.py:114`)

   **PROCEDENTE — LU-07 ainda permite rota classificada por outro chamado.** Caso sintético reproduzido: ação `validar/admin/202`, pergunta ancorada em 202, chamado 101 da célula com `cobrar`. O juiz filtra a porta administrativa para 202, mas publica `rota='porta_fila'`, `fila=(101,)`, porque a escolha final consulta `da_celula` global. A porta do admin é a da pergunta; quem força “fila” é o cobrar sem dono. (`app/chamados/services/tarefa_do_dia.py:591`, `app/chamados/services/tarefa_do_dia.py:611`, `app/chamados/services/tarefa_do_dia.py:619`, `app/chamados/services/tarefa_do_dia.py:627`)

2. **LU-03: tela efetiva e apresentação de `porta_acao`.**

   Existem telas para validar pergunta no modal do fio, decidir ausência e Resolver dia; esta última depende da oferta do calendário e da permissão. (`app/templates/chamados/partials/modal_fio.html:279`, `app/templates/ponto/partials/painel_ausencia.html:323`, `app/templates/ponto/partials/painel_ausencia.html:344`, `app/templates/colaboradores/partials/_calendario_grade.html:131`)

   **PROCEDENTE — a publicação não prova alcançabilidade.** `_portas_da_tarefa` aceita toda ação `ADMIN`; não pergunta se existe tela correspondente. O contraexemplo sem âncora está detalhado no item 8. (`app/chamados/services/tarefa_do_dia.py:606`, `app/chamados/services/tarefa_do_dia.py:629`)

   **PROCEDENTE — a Folha pode mostrar “admin agir” sem link para essa ação.** `o_que_segura` descarta `rota`; o retrato tem `PORTA_DO_FIO['admin']=None`, e tipos `falta`, `nao_bate` e `ferias` também não fornecem link. Nessa combinação, `portas_da_linha` devolve lista vazia e o template omite os botões. (`app/folha/fora_da_folha.py:246`, `app/folha/retrato_folha.py:975`, `app/folha/retrato_folha.py:990`, `app/folha/retrato_folha.py:1015`, `app/templates/folha/_retrato_o_que_fazer.html:66`)

   **NÃO PROCEDENTE — âncora terminal ou silenciada, isoladamente, impedir validação.** O fio administrativo carrega todos os chamados da pessoa, e o POST de validar exige correspondência da âncora, mas não pertença à fila. (`app/chamados/services/fio.py:204`, `app/chamados/services/fio.py:600`, `app/chamados/services/fio.py:636`)

3. **Baldes do vigia e código morto.**

   **PROCEDENTE — ainda há decisão própria de dono fora da porta publicada.** Depois de receber porta `None`, `dono_fora_do_fio` classifica oferta de Resolver dia, ausência aberta, aceite de folga e dono da lista. Ambos os censos usam essa cadeia para retirar dias do fio morto. Portanto, os baldes ainda não se limitam a ler a porta da tarefa. (`app/chamados/services/vigia_fio.py:306`, `app/chamados/services/vigia_fio.py:308`, `app/chamados/services/vigia_fio.py:311`, `app/chamados/services/vigia_fio.py:313`, `app/chamados/services/vigia_fio.py:442`, `app/chamados/services/vigia_fio.py:683`)

   **NÃO PROCEDENTE — esses baldes ficaram sem chamador.** Permanecem chamados pelos dois censos. **PROCEDENTE — sobrou a função `vigia_fio.fio_do_dia` sem chamador de produção encontrado na busca estática**; os censos passaram a ler a porta. Chamador dinâmico: [não verificado]. (`app/chamados/services/vigia_fio.py:156`, `app/chamados/services/vigia_fio.py:428`, `app/chamados/services/vigia_fio.py:442`, `app/chamados/services/vigia_fio.py:683`)

4. **Segundo juiz/escritor e relógios.**

   **NÃO PROCEDENTE — segundo escritor dos campos da própria tarefa encontrado fora de `_portas_da_tarefa`.** A publicação final está concentrada no `out.update`; o C17 continua calculando outra projeção, mas seu consumidor operacional encontrado é o instrumento de comparação histórica. (`app/chamados/services/tarefa_do_dia.py:630`, `app/chamados/juizes.py:1709`, `app/chamados/management/commands/diff_e3_sombra.py:127`)

   **PROCEDENTE — relógio fora da borda no caminho dos baldes.** `_resolver_da_rodada` chama o calendário sem instante; a plural monta `contexto_calendario`, que lê `timezone.now()`. A oferta de Resolver dia usa o novo “hoje”, inclusive quando o command já forneceu seu instante. (`app/chamados/services/vigia_fio.py:270`, `app/colaboradores/services/calendario.py:132`, `app/colaboradores/services/calendario.py:163`, `app/colaboradores/services/calendario.py:103`)

   **PROCEDENTE — censos avulsos perdem `agora` ao criar `ListaDoAdmin`.** Ambos omitem o argumento; a lista repassa `None`, levando `cadastro_realidade.lista` a ler outro relógio. O command principal fornece a lista com `agora` corretamente. (`app/chamados/services/vigia_fio.py:391`, `app/chamados/services/vigia_fio.py:645`, `app/chamados/services/vigia_fio.py:216`, `app/escala/services/cadastro_realidade.py:775`, `app/chamados/management/commands/vigiar_fio_morto.py:175`)

5. **Selos, testes e mutantes.** [nao verificado]

   **NÃO PROCEDENTE — os mutantes novos dispensam controle verde.** O executor exige alvo textual único, teste original verde e morte por assertion sem erro ou skip; a substituição é removida em `finally`. (`app/chamados/tests/e3_degrau_mutantes.py:116`, `app/chamados/tests/e3_degrau_mutantes.py:130`, `app/chamados/tests/e3_degrau_mutantes.py:135`)

   Execuções em memória nesta sessão: suite nova **47 OK**; mutantes **34/34 mortos**; suites `test_e3_c8b_sem_banco` e `test_e3_tarefa_admin_sem_banco`, juntas, **32 testes, 10 falhas, zero erros**. Sem artefato persistido de resultado: [não verificado]. [nao verificado]

   **PROCEDENTE — permaneceram assertivas antigas contrárias ao comportamento LU atual.** Há testes exigindo `nada` para ações diretas do admin e outros exigindo rota de fila/pergunta para colab com app. (`app/chamados/tests/test_e3_c8b_sem_banco.py:56`, `app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:128`, `app/chamados/tests/test_e3_portas_tarefa.py:427`, `app/chamados/tests/test_e3_portas_tarefa.py:512`, `app/chamados/tests/test_e3_portas_tarefa.py:537`)

   **Limite de L1–L10/C1–C15:** exercitam o juiz real com ações e pendências sintéticas; não demonstram a existência da tela administrativa. C1/C2 realmente impedem consulta ao prazo, mas explicitamente não provam vencimento C9. (`app/chamados/tests/test_e3_degrau_dono_e_porta.py:100`, `app/chamados/tests/test_e3_degrau_dono_e_porta.py:105`, `app/chamados/tests/e3_portas_apoio.py:29`)

   **NÃO PROCEDENTE — lacuna para alias de import ou atributo simples no selo AST.** O resolvedor trata alias e atributos; a detecção bloqueia o nome resolvido e possui contraprovas para alias e `J.porta_alcancavel_do_dia`. Também exige chamadas efetivas da carga e projeção. (`app/folha/tests/test_l129_fora_da_folha_so_le_a_tarefa.py:33`, `app/folha/tests/test_l129_fora_da_folha_so_le_a_tarefa.py:42`, `app/folha/tests/test_l129_fora_da_folha_so_le_a_tarefa.py:73`, `app/folha/tests/test_l129_fora_da_folha_so_le_a_tarefa.py:83`, `app/folha/tests/test_l129_fora_da_folha_so_le_a_tarefa.py:107`)

   **Partição: prova uma por chamada de `censo_alcance`, não uma pelo command inteiro.** O teste agora exige `call_count==1` dentro de cada `_alc` e separa a tarefa avulsa. Porém o command chama `censo` e `censo_alcance`, cada qual com carga própria; com ambos os universos não vazios, cada carga executa sua partição. (`app/chamados/tests/test_o261e_f5_alcance.py:274`, `app/chamados/tests/test_o261e_f5_alcance.py:280`, `app/chamados/management/commands/vigiar_fio_morto.py:180`, `app/chamados/services/vigia_fio.py:410`, `app/chamados/services/vigia_fio.py:658`, `app/chamados/services/tarefa_do_dia.py:513`)

6. **Esmeril semântica.**

   | Valor | Significado implementado |
   |---|---|
   | `porta_app` | Colab com `RESPONDER` exigível e pergunta nos pendentes do app. (`app/chamados/services/tarefa_do_dia.py:603`, `app/chamados/services/tarefa_do_dia.py:624`) |
   | `porta_acao` | Admin vencedor sem porta administrativa reconhecida na fila. (`app/chamados/services/tarefa_do_dia.py:619`, `app/chamados/services/tarefa_do_dia.py:629`) |
   | `porta_fila` / `porta_pergunta` | Classificação ainda calculada pelo conjunto global das portas, causando o achado LU-07. (`app/chamados/services/tarefa_do_dia.py:627`) |
   | `fio='nada'` | Nenhum dono selecionado, com `nao_sei=False`; rota `None`. (`app/chamados/services/tarefa_do_dia.py:615`, `app/chamados/services/tarefa_do_dia.py:622`) |
   | `fio='nao_sei'` | Nenhum dono selecionado, com desconhecimento explícito; rota `None`. (`app/chamados/services/tarefa_do_dia.py:615`, `app/chamados/services/tarefa_do_dia.py:622`) |
   | `FIO_SEM_PORTA` | Constante `'nada'`; a Folha devolve-a quando a projeção é `None`, preservando `nao_sei` separadamente. (`app/chamados/juizes.py:1652`, `app/folha/fora_da_folha.py:85`, `app/folha/fora_da_folha.py:248`) |
   | `fio='sistema'`, rota `None` | Exceção existente: sistema pode conservar dono interno sem porta publicada; os leitores da projeção recebem `None`. (`app/chamados/services/tarefa_do_dia.py:607`, `app/chamados/services/tarefa_do_dia.py:629`, `app/chamados/services/tarefa_do_dia.py:640`) |

   **PROCEDENTE — os vocabulários não são idênticos em todas as camadas.** O retrato transforma o grupo em fio agregado, usa `ninguem` e agrupa esse valor em Admin; não publica o mesmo objeto colab-dia da tarefa. Esse é o comportamento legado relacionado ao LU-05 deixado fora deste pouso. (`app/folha/retrato_folha.py:368`, `app/folha/retrato_folha.py:192`, `app/folha/retrato_folha.py:1190`)

7. **Comentários/docstrings contraditórios do diff — PROCEDENTE.**

   Permanecem textos dizendo que ação administrativa sem chamado na fila não tem porta, contrariando a inclusão direta de `ADMIN` e a publicação de `porta_acao`. Exemplos: comentário da fixture bloqueante; comentários dos testes de alcance; docstring que inclui ação direta administrativa em `nada`. (`app/chamados/tests/test_e3_bloqueantes.py:97`, `app/chamados/tests/test_o261e_f5_alcance.py:487`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:264`, `app/folha/tests/test_haiku_fora_da_folha.py:160`, `app/chamados/services/tarefa_do_dia.py:606`, `app/chamados/services/tarefa_do_dia.py:629`)

8. **Contraexemplo pela lei — PROCEDENTE, sintético; incidência em produção [não verificado].**

   Considere dia passado da competência atual, marco vencido apagado e pergunta respondida com linha E1, sem chamado da disputa nem `chamado_destino`. Os dois vínculos admitem NULL. `_pela_decisao` produz `validar/admin/None`, e a publicação resulta em **`fio='admin'`, `rota='porta_acao'`, `fila=()`, `verbos={}`**. (`app/chamados/models.py:1796`, `app/chamados/models.py:3494`, `app/chamados/services/tarefa_do_dia.py:140`, `app/chamados/services/tarefa_do_dia.py:150`, `app/chamados/services/tarefa_do_dia.py:606`, `app/chamados/services/tarefa_do_dia.py:629`)

   Entretanto, o modal exclui disputa sem chamado e pergunta sem âncora; os POSTs de validação exigem chamado correspondente. Resolver dia não aparece na competência atual. Escolhendo resposta fora do envelope da classe A, o lote também não fornece saída: classifica como B e recusa validação pelo lote. Logo, publica dono administrativo sem ação alcançável nas telas identificadas, contrariando a condição de LU-03. (`app/chamados/services/fio.py:211`, `app/chamados/services/fio.py:271`, `app/chamados/services/fio.py:600`, `app/chamados/services/fio.py:636`, `app/chamados/views.py:499`, `app/colaboradores/services/calendario.py:738`, `app/chamados/services/validacao.py:902`, `app/chamados/services/validacao.py:770`)

9. **Pouso e dependências.** [nao verificado]

   **NÃO PROCEDENTE — necessidade de migration para os novos campos de saída.** São campos de dict e memo da carga, sem coluna persistente nova nesses trechos. A tarefa, porém, depende da sequência E1; projeção sem linha continua produzindo `passivo_sem_linha`. (`app/chamados/services/tarefa_do_dia.py:522`, `app/chamados/services/tarefa_do_dia.py:630`, `app/chamados/services/tarefa_do_dia.py:142`)

   O diff sombra está registrado como instrumento fora do pipeline, não como cron novo obrigatório. Aplicação das migrations, backfills prévios, equivalência do hash e existência de outra raia já pousada: **[não verificado]**. (`app/config/crons.py:769`, `app/config/crons.py:788`, `app/config/crons.py:798`)

## 14/11 19:51 · sessao a4657ec7ecfc · - · - · origem=Ronald (tmux)

**PROCEDENTE — suites antigas continuam vermelhas contra LU-02/03/07.** Executei com `PYTHONDONTWRITEBYTECODE=1`, sem settings, Django real, banco, logs ou arquivos de saída; resultados de execução não persistidos [não verificado]. [nao verificado]

- `test_e3_c8b_sem_banco` + `test_e3_tarefa_admin_sem_banco`: **32 testes, 10 falhas, zero erros**. Exigem `fio='nada'` apesar de ação administrativa direta, agora publicada como admin: `app/chamados/tests/test_e3_c8b_sem_banco.py:56`, `:172`, `:190`, `:256`; `app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:128`. A regra atual inclui toda ação ADMIN: `app/chamados/services/tarefa_do_dia.py:606`.
- Executei por AST, sem importar suas fixtures ORM, três classes originais de `test_e3_portas_tarefa`: **21 testes, sete falhas, zero erros**. Assertivas antigas de rota do colab em `app/chamados/tests/test_e3_portas_tarefa.py:427`, `:474`, `:512`, `:537`; histórico dando porta em `:487`; verbo None dando porta em `:582`. Contradizem a implementação em `app/chamados/services/tarefa_do_dia.py:611`, `:625`. Contagem de execução [não verificado].

**NÃO PROCEDENTE — mutantes novos mortos sem controle verde.** A suite nova passou **47 testes**; `--todos` concluiu **34/34 mortos**, exit 0 [não verificado]. O executor exige alvo único, verde antes de cada mutação, falha de assertion sem erro/skip e restaura a sobrescrita em memória: `app/chamados/tests/e3_degrau_mutantes.py:116`, `:127`, `:135`, `:154`. As fontes mutadas ficam em dict: `app/chamados/tests/e4_catracas.py:5`.

**Limite dos selos novos:** L1–L10/C1–C15 testam o juiz real com ações/perguntas sintéticas; não comprovam existência de tela administrativa. `julgar` injeta `dia_plano` e pendentes: `app/chamados/tests/test_e3_degrau_dono_e_porta.py:100`; formato sintético: `app/chamados/tests/e3_portas_apoio.py:29`. C1/C2 realmente armam traps de prazo local e no módulo motor e exigem ausência de chamada, mas explicitamente não provam vencimento C9: `app/chamados/tests/test_e3_degrau_dono_e_porta.py:105`.

**NÃO PROCEDENTE — selo AST ignora alias ou atributo simples.** Resolve `from ... import funcao as apelido`, imports de módulo e cadeias de atributos: `app/folha/tests/test_l129_fora_da_folha_so_le_a_tarefa.py:30`, `:38`. Bloqueia atributos e nomes resolvidos: `:69`, `:73`. Os contraprovas incluem alias e `J.porta_alcancavel_do_dia`: `:94`, `:106`, `:107`; exige chamadas efetivas da carga/projeção: `:83`.

**PROCEDENTE se “rodada” significa o command completo; NÃO PROCEDENTE para uma chamada de `censo_alcance`.** O teste passa a exigir exatamente uma partição dentro de cada `_alc`, separando depois a tarefa avulsa: `app/chamados/tests/test_o261e_f5_alcance.py:274`, `:280`, `:282`. Entretanto `_alc` chama apenas `censo_alcance`: `:116`. O command chama **censo e censo_alcance**: `app/chamados/management/commands/vigiar_fio_morto.py:180`. Ambos constroem carga própria: `app/chamados/services/vigia_fio.py:410`, `:658`. Cada carga executa sua partição: `app/chamados/services/tarefa_do_dia.py:513`. Assim, com ambos os universos não vazios, há duas partições no command, embora uma por carga. O comentário “por carga/rodada” amplia indevidamente o alcance: `app/chamados/tests/test_n1_alimentacao_unica.py:105`.

**PROCEDENTE — comentários/docstrings contraditórios remanescentes do diff:**

- “Sem porta da fila … dia não tem porta de ninguém”: `app/chamados/tests/test_e3_bloqueantes.py:97`; contraria ADMIN direto em `app/chamados/services/tarefa_do_dia.py:606`, `:629`.
- “admin só chega ao dia por chamado NA FILA”: `app/chamados/tests/test_o261e_f5_alcance.py:487` e `app/chamados/tests/test_o261e_f5b_juiz_porta.py:264`; mesma contradição.
- "`nada` inclui … ação direta do admin sem chamado na fila": `app/folha/tests/test_haiku_fora_da_folha.py:160`; mesma contradição.
- Comentários nas assertivas antigas que chamam validar direto de inalcançável: `app/chamados/tests/test_e3_c8b_sem_banco.py:56`; `app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:128`.

**Pouso:** não executei migrations, cron nem Django. Os novos campos são projeção em dict e memo de instância (`app/chamados/services/tarefa_do_dia.py:630`, `:522`), sem escrita persistente nesses trechos. Ausência absoluta de dependência de outra raia ou migration no hash não pode ser certificada sem histórico Git; esta cópia não tem `.git` [não verificado].

## 14/11 19:50 · sessao 6656d8ac9316 · - · - · origem=Ronald (tmux)

Achados para consolidar:

- **PROCEDENTE — rota do admin ainda pode ser escolhida pela porta de outro dono.** `_portas_da_tarefa` filtra `portas_vivas` para o admin, mas escolhe `porta_fila` usando `da_celula` e `portas` globais. Reproduzi, sem escrita, ação `validar/admin/202`, pergunta 303 ancorada em 202, chamado 101 da célula com `cobrar`: saída `fio=admin`, `rota=porta_fila`, `fila=(101,)`, `verbos={101:cobrar,202:validar}`. A porta do admin é 202, via pergunta; o chamado que força a classificação de fila é 101, cujo cobrar não dá dono. Não constatei link quebrado: a enumeração não é convertida diretamente em URL pelos leitores encontrados. `app/chamados/services/tarefa_do_dia.py:591`, `:599`, `:611`, `:619`, `:627`.

- **PROCEDENTE — os baldes do vigia continuam decidindo além da porta publicada.** Com porta ausente, `dono_fora_do_fio` ainda classifica “Resolver dia”, ausência aberta, aceite de folga e dono da lista; os dois censos chamam essa cadeia. Portanto, não são somente leitura de `porta_da_tarefa`. Os baldes têm chamadores; não ficaram mortos. `app/chamados/services/vigia_fio.py:306`, `:308`, `:311`, `:313`, `:440`, `:442`, `:661`, `:683`.

- **PROCEDENTE — relógios adicionais no caminho dos baldes.** `_resolver_da_rodada` chama o calendário sem instante; a plural monta `contexto_calendario`, que lê `timezone.now()`. O resultado usa o “hoje” desse novo instante para oferecer Resolver dia. Isso ocorre também no command principal. `app/chamados/services/vigia_fio.py:270`; `app/colaboradores/services/calendario.py:103`, `:117`, `:132`, `:159`, `:163`. Há outra leitura para desfazer batida em `app/colaboradores/services/calendario.py:250`.

- **PROCEDENTE — censos avulsos perdem o instante ao criar a lista.** Mesmo recebendo `agora`, ambos criam `ListaDoAdmin` sem ele; a lista repassa `None` e Cadastro × Realidade lê outro relógio. O command principal passa a lista com instante corretamente. `app/chamados/services/vigia_fio.py:391`, `:645`, `:216`; `app/escala/services/cadastro_realidade.py:775`; `app/chamados/management/commands/vigiar_fio_morto.py:175`, `:180`.

- **PROCEDENTE — função morta de produção.** `vigia_fio.fio_do_dia` permanece definida, mas a busca de chamadas em código de produção só encontrou a definição; os censos agora usam a porta publicada. `app/chamados/services/vigia_fio.py:156`, `:428`, `:669`. A ausência de outros chamadores é resultado da busca estática; execução dinâmica: **[não verificado]**.

- **NÃO PROCEDENTE — `porta_acao` causar KeyError ou desaparecer dos contadores identificados.** O alcance inicializa as quatro rotas, incrementa pela enumeração publicada e imprime app/ação; furo conta as quatro rotas e o command as imprime. `app/chamados/services/vigia_fio.py:587`, `:588`, `:673`, `:741`; `app/chamados/services/furo_sem_canal.py:114`; `app/chamados/management/commands/lavrar_furo_sem_canal.py:37`. Integração em produção: **[não verificado]**.

Mapa dos campos e consumidores:

| Elemento | Escritor/leitor |
|---|---|
| `fio`, `rota`, `fila`, `verbos` da tarefa | Escritor final único encontrado: `out.update`, `app/chamados/services/tarefa_do_dia.py:630`; projeção em `:640`. |
| `donos` | Variável local do juiz, não campo publicado: `app/chamados/services/tarefa_do_dia.py:608`, `:615`, `:630`. |
| Folha | Consome porta e fio; sem porta devolve `FIO_SEM_PORTA`; lê fila para números de chamados. `app/folha/fora_da_folha.py:67`, `:82`, `:193`, `:238`, `:244`. |
| Retrato | Recebe agregação da Folha, grava fio na chave histórica `dono`, lê esse valor; não lê enumeração `rota`. `app/folha/retrato_folha.py:1158`, `:1190`, `:438`. |
| Links do retrato | Deriva links dos mapas de fio/tipo; admin tem `None` no mapa de fio, e tipos falta/não bate/férias também não possuem link próprio. `app/folha/retrato_folha.py:974`, `:986`, `:1015`; template mostra os links fornecidos em `app/templates/folha/_retrato_o_que_fazer.html:66`. |
| Pauta dos discordantes | Usa o fio projetado e compara com `FIO_SEM_PORTA`. `app/ponto/services/pautas_dos_discordantes.py:64`, `:79`, `:81`. |
| Cadastro × Realidade | Exclui o beco quando existe porta, independentemente da enumeração. `app/escala/services/cadastro_realidade.py:464`, `:469`. |
| Furo sem canal | Presença da porta define canal; enumeração alimenta os quatro contadores. `app/chamados/services/furo_sem_canal.py:97`, `:104`, `:114`. |
| Vigia | Presença da porta define fio vivo; alcance lê rota/fio diretamente, depois aplica os próprios baldes se não houver porta. `app/chamados/services/vigia_fio.py:428`, `:669`, `:673`, `:683`. |
| API do app | Consulta `consulta_do_app`; não consome a tarefa publicada nesses endpoints. `app/api/views.py:993`, `:994`, `:2533`, `:2534`. |
| Diff sombra | Lê fio/fila/rota e presença da porta; conserva C17 como comparação histórica. `app/chamados/management/commands/diff_e3_sombra.py:49`, `:53`, `:56`, `:127`, `:143`. |

Semântica: tarefa `nada`/`nao_sei` publica rota `None`; `porta_da_tarefa` então devolve `None`, e a Folha reduz ambos a `FIO_SEM_PORTA='nada'`, preservando `nao_sei` separadamente. Sistema pode conservar fio `sistema` com rota `None`, sendo igualmente reduzido a sem porta pelos leitores. `app/chamados/services/tarefa_do_dia.py:615`, `:622`, `:629`, `:640`; `app/folha/fora_da_folha.py:85`, `:248`; `app/chamados/juizes.py:1652`.

C17 continua sendo segundo derivador no código, mas seu chamador de produção encontrado é o instrumento de comparação, não os leitores migrados. `app/chamados/juizes.py:1664`, `:1709`; `app/chamados/management/commands/diff_e3_sombra.py:127`. A agregação do retrato é outra unidade — colaborador/competência, com fallback legado — e conserva o agrupamento sem porta→Admin declarado fora deste pouso. `app/folha/retrato_folha.py:192`, `:343`, `:368`, `:441`, `:1190`.

## 14/11 19:50 · sessao 4923414193e6 · - · - · origem=Ronald (tmux)

**PROCEDENTE — `porta_acao` pode nascer sem tela capaz de executar a ação publicada.** Basta pergunta respondida com linha E1, marco vencido apagado, sem `chamado_destino` nem chamado da disputa, em dia da competência atual e resposta fora da classe A: ambos os vínculos aceitam NULL; `_pela_decisao` produz `validar/admin/None`; `_portas_da_tarefa` publica `admin/porta_acao`. (`app/chamados/models.py:1796`, `app/chamados/models.py:3494`, `app/chamados/services/tarefa_do_dia.py:140`, `app/chamados/services/tarefa_do_dia.py:150`, `app/chamados/services/tarefa_do_dia.py:606`, `app/chamados/services/tarefa_do_dia.py:629`.)

O modal carrega somente disputas com chamado e descarta perguntas sem âncora; o POST exige chamado real e correspondência da âncora. O detalhe impõe a mesma correspondência. (`app/chamados/services/fio.py:211`, `app/chamados/services/fio.py:271`, `app/chamados/services/fio.py:600`, `app/chamados/services/fio.py:636`, `app/chamados/views.py:493`, `app/chamados/views.py:499`.)

O lote não salva esse contraexemplo: resposta além do envelope é classe B; o escritor do lote recusa tudo fora de A, e a prévia apresenta as classes externas apenas por contagem. Resolver dia tampouco aparece na competência atual. (`app/chamados/services/validacao.py:902`, `app/chamados/services/validacao.py:770`, `app/templates/chamados/partials/_lote_preview.html:15`, `app/colaboradores/services/calendario.py:738`, `app/colaboradores/services/calendario.py:103`.) Incidência em produção: **[não verificado]**.

**NÃO PROCEDENTE — âncora terminal/silenciada, isoladamente, torna a ação inalcançável.** O fio administrativo carrega todos os chamados da pessoa, sem `qs_na_fila`; o POST de validar não exige fila. Havendo container e pergunta listada, existe botão no modal e execução pelo endpoint. (`app/chamados/services/fio.py:204`, `app/chamados/services/fio.py:600`, `app/chamados/services/fio.py:636`, `app/templates/chamados/partials/modal_fio.html:279`.)

**PROCEDENTE — a Folha pode dizer “admin agir” sem oferecer link para a ação.** `o_que_segura` conserva fio e frase, mas descarta rota; no retrato, `PORTA_DO_FIO['admin']` é `None`, assim como tipos `nao_bate`, `falta` e `ferias`; a combinação devolve lista vazia, e o template só mostra botões com `retrato_portas`. Não há KeyError nesse caminho: há omissão de link. (`app/folha/fora_da_folha.py:238`, `app/folha/fora_da_folha.py:246`, `app/folha/retrato_folha.py:975`, `app/folha/retrato_folha.py:990`, `app/folha/retrato_folha.py:1015`, `app/templates/folha/_retrato_o_que_fazer.html:66`.)

Telas existentes para ações legítimas: validar no modal do fio, decidir ausência no painel da ausência, Resolver dia no calendário condicionado ao juiz de oferta e à permissão. Nenhuma delas interpreta a string `porta_acao` para selecionar tela. (`app/templates/chamados/partials/modal_fio.html:279`, `app/templates/ponto/partials/painel_ausencia.html:323`, `app/templates/ponto/partials/painel_ausencia.html:344`, `app/templates/colaboradores/partials/_calendario_grade.html:131`, `app/colaboradores/services/calendario.py:97`.)

## 14/11 19:41 · sessao baf83260c678 · E3-V10-DEGRAU-DONO-E-PORTA · construtor · origem=Code

Cura implementada em [tarefa_do_dia.py:573](/home/ronald/wt-codex-e3-v10-degrau-dono-e-porta-192627/app/chamados/services/tarefa_do_dia.py:573): LU-02, LU-03 e LU-07 ficam no juiz único. A certificação pela suite/opus permanece pendente **[nao verificado]**. [nao verificado]

A execução local terminou com **76 testes OK**, **34/34 mutantes mortos**, controles verdes e nenhum erro nos alvos. Os dois mutantes legados adaptados também retornaram RC 1. Referências: `chamados/tests/test_e3_degrau_dono_e_porta.py:41`, `chamados/tests/e3_degrau_mutantes.py:127`, `chamados/tests/e3_portas_v6_mutantes.py:90`, `chamados/tests/e3_volta7_mutantes.py:123`.

O diff por arquivo e função ficou assim:

| Arquivo::função — referência | Alteração |
|---|---|
| `chamados/services/tarefa_do_dia.py:573::_portas_da_tarefa` | Exclui residual histórico/None; aceita ação direta administrativa; publica a rota do dono. |
| `chamados/services/tarefa_do_dia.py:633::porta_da_tarefa` | Documenta a projeção vigente; mantém leitura dos cinco campos. |
| `chamados/services/vigia_fio.py:318::censo` | Consome a porta da tarefa antes dos baldes. |
| `chamados/services/vigia_fio.py:553::censo_alcance` | Acrescenta `alcance_porta_acao`; publica o contador no log. |
| `chamados/services/furo_sem_canal.py:43::medir` | Acrescenta `com_porta_acao`. |
| `chamados/management/commands/lavrar_furo_sem_canal.py:29::handle` | Exibe o contador de ação direta. |
| `chamados/management/commands/vigiar_fio_morto.py:180::handle` | Encaminha o mesmo `agora` aos dois censos. |
| `folha/fora_da_folha.py:38::GRUPOS/DESTRAVA` | Rótulo administrativo passa a incluir ação direta. |
| `folha/tests/test_l129_fora_da_folha_so_le_a_tarefa.py:26::chamadas/violacoes` | Confere chamadas, inclusive aliases; exige chamadas à carga/projeção. |
| `chamados/tests/test_e3_degrau_dono_e_porta.py:41::DegrauDonoEPorta` | Matriz L1–L10, caracterizações invertidas, consumidores e traps de prazo. |
| `chamados/tests/e3_degrau_mutantes.py:17::MUTANTES` | Quatro mutantes pedidos, equivalentes vivos de P1 e cobertura dos leitores alterados. |
| `chamados/tests/test_o261e_f5_alcance.py:122::_soma` | Inclui `porta_acao` na soma L-110; P3 exige uma chamada à partição por rodada. |
| `chamados/tests/test_n1_alimentacao_unica.py:104` | Corrige docstrings para carga em lote, com os IDs LU. |
| `chamados/tests/test_e3_portas_tarefa.py:48` | Ajusta expectativas de rota do colab e residual histórico. |
| `chamados/tests/test_e3_portas_v6_sem_banco.py:45` | Ajusta a prioridade da ação administrativa direta. |
| `chamados/tests/test_o261e_f5b_juiz_porta.py:232` | Preserva fila/verbos e exige a nova porta administrativa. |
| `chamados/tests/test_e3_diff_sombra.py:170` | Exige a diferença de rota A/B prevista por LU-07. |
| `chamados/tests/e3_portas_v6_mutantes.py:63` e `chamados/tests/e3_volta7_mutantes.py:24` | Invertem os mutantes legados para negar a ação direta. |

A matriz foi executada **antes da alteração da regra**, com este comando:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest -v chamados.tests.test_e3_degrau_dono_e_porta.DegrauDonoEPorta -k test_L
```

Trecho da saída RED inicial:

```text
test_L1_historico_sem_porta ... FAIL
test_L2_verbo_nulo_sem_porta ... FAIL
test_L4_admin_direto_tem_porta_e_folha_le_admin ... FAIL
test_L5_admin_direto_com_cobrar ... FAIL
test_L6_rota_colab_com_decidir_na_celula ... FAIL
test_L7_rota_colab_com_acao_admin_na_fila ... FAIL
Ran 10 tests in 1.069s
FAILED (failures=6)
```

Os resultados antes/depois, em `(fio, rota)`, foram:

| Caso e referência no teste | Fase RED | GREEN | [nao verificado]
|---|---|---|
| L1 — `test_e3_degrau_dono_e_porta.py:41` | FAIL: `chamado, porta_fila` | `nada, None`; projeção None |
| L2 — `:47` | FAIL: `chamado, porta_fila` | `nada, None`; projeção None |
| L3 — `:53` | OK: `chamado, porta_fila` | Preservado |
| L4 — `:57` | FAIL: `nada, None` | `admin, porta_acao`; Folha lê admin |
| L5 — `:67` | FAIL: `nada, None` | `admin, porta_acao` |
| L6 — `:72` | FAIL: `colab, porta_fila` | `colab, porta_app` |
| L7 — `:76` | FAIL: `colab, porta_fila` | `colab, porta_app` |
| L8 — `:81` | OK: `admin, porta_fila` | Preservado |
| L9 — `:86` | OK: `admin, porta_fila` | Preservado |
| L10 — `:91` | OK: `nada, None` | Preservado |

L9 conserva `porta_fila`: o residual `validar` fornece uma porta administrativa na célula, que prevalece sobre a rota da ação direta. Já ação administrativa direta com residual `decidir` usa `porta_acao`, sem converter o residual em admin: `chamados/services/tarefa_do_dia.py:619`, `chamados/tests/test_e3_degrau_dono_e_porta.py:96`.

O censo de leitores foi feito antes da primeira gravação, usando `git grep -n` pelos termos solicitados. O tratamento de `porta_acao` é:

| Leitor — referência | Tratamento |
|---|---|
| `folha/fora_da_folha.py:37`, `:82`, `:238` | `FIOS` continua sendo vocabulário de fio; lê `admin` diretamente. Rótulo atualizado em `:45`; chamados continuam vindo de `fila`, em `:244`. |
| `folha/retrato_folha.py:192`, `:974`, `:1015`, `:1069` | Consome fio/grupo e rótulo. Não discrimina a rota da tarefa; links continuam pelo mapa de fio/tipo. `PORTA_DO_FIO['admin']` permanece None. |
| `chamados/services/furo_sem_canal.py:97`, `:117` | Reconhece a presença da porta e conta `com_porta_acao`. |
| `chamados/services/vigia_fio.py:412`, `:428` | `censo()` lê a projeção e conta em `fio_vivo`; não separa por rota. |
| `chamados/services/vigia_fio.py:588`, `:673`, `:740` | `censo_alcance()` inicializa, conta e imprime `alcance_porta_acao`. Soma cobrada em `chamados/tests/test_o261e_f5_alcance.py:124`. |
| `escala/services/cadastro_realidade.py:469` | Testa presença da projeção; qualquer porta publicada exclui o dia do beco. |
| `ponto/services/pautas_dos_discordantes.py:79` | Lê indiretamente o fio da Folha; admin deixa de ser sem fio. |
| `chamados/management/commands/diff_e3_sombra.py:46` | Projeta rota/fio/fila e testa presença, sem enumeração fechada de rotas. |
| `chamados/management/commands/lavrar_furo_sem_canal.py:37` | Exibe o contador novo. |

Os escritores e consumidores dos campos permanecem identificáveis:

| Campo | Escrita/fornecimento | Leitura |
|---|---|---|
| `rota`, `fio` | Seleção e publicação únicas em `chamados/services/tarefa_do_dia.py:615`, `:630` | Projeção `:641`; consumidores no censo acima. |
| `verbos` | Painel/alimentação em `chamados/juizes.py:1570`; seleção na tarefa em `tarefa_do_dia.py:601` | Residual em `:611`; projeção em `:641`; instrumento em `diff_e3_sombra.py:144`. |
| `fila` | Alimentação em `chamados/juizes.py:1573`; seleção publicada em `tarefa_do_dia.py:602` | Folha em `folha/fora_da_folha.py:244`; instrumento em `diff_e3_sombra.py:143`. |
| Projeção C17 histórica | `chamados/juizes.py:1709` | Lado anterior do comparador, `diff_e3_sombra.py:127`; não foi alterada. |

Nos baldes do vigia, os dias com ação administrativa já publicada pela tarefa passam antes dos baldes: `censo()` em `vigia_fio.py:428`; `censo_alcance()` em `:669`. [nao verificado]

- **Decidir ausência:** quando a tarefa expõe a ação, sai de `aguarda_decisao_de_ausencia` e ganha porta administrativa. Fontes: `tarefa_do_dia.py:174`, `:305`, `:606`.
- **A/S validar:** quando a tarefa publica `validar` sem chamado, ganha `porta_acao`. Fonte: `tarefa_do_dia.py:352`.
- **Resolver dia:** dias que também tenham ação administrativa na tarefa saem de `decisao_do_dia_pendente`. A oferta do calendário sozinha continua podendo alcançar o balde; não foi criada ação nova neste lote. Fontes: `vigia_fio.py:306`, `:442`, `:683`.
- **Aguardando documento:** continua sem dono por essa ação, preservando L10/LU-04 fora do lote. Fontes: `tarefa_do_dia.py:299`, `test_e3_degrau_dono_e_porta.py:91`.

Os dois baldes conservam chamadores condicionados à ausência de porta. Quantos dias restam em cada um, ou se algum fica vazio na frota: **[nao verificado]**.

Para reproduzir as verificações:

```bash
# Reproduz os seis REDs contra a base fornecida; RC esperado 1.
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e3_degrau_mutantes --base

# GREEN final: 76 testes OK. [nao verificado]
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest -q \
  chamados.tests.test_e3_degrau_dono_e_porta \
  chamados.tests.test_e3_pos_suite_sem_banco \
  chamados.tests.test_e3_portas_v6_sem_banco \
  chamados.tests.test_e3_volta7_sem_banco

# Concluiu: MORTOS=34 TOTAL=34; RC 0.
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e3_degrau_mutantes --todos
```

O runner imprime o comando individual antes de cada controle verde; falha do controle levanta `AssertionError`: `chamados/tests/e3_degrau_mutantes.py:132`. Os quatro pedidos e os equivalentes vivos de P1 são:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e3_degrau_mutantes --mutante historico_da_dono
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e3_degrau_mutantes --mutante acao_direta_sem_dono
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e3_degrau_mutantes --mutante rota_colab_pela_fila
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e3_degrau_mutantes --mutante porta_acao_some
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e3_degrau_mutantes --mutante sem_memo_por_colab
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e3_degrau_mutantes --mutante carga_sem_relogio

# Legados adaptados; ambos RC 1.
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e3_portas_v6_mutantes --mutante fio_acao_sem_porta_ignorada
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e3_volta7_mutantes --mutante fio_v6
```

P2 agora acusa chamadas proibidas inseridas em memória, inclusive por alias: `folha/tests/test_l129_fora_da_folha_so_le_a_tarefa.py:90`. P4 usa traps de prazo, sem alegar vencimento C9: `test_e3_degrau_dono_e_porta.py:105`. P3 exige `call_count == 1` por rodada, mas sua execução com fixture ORM permanece **[nao verificado]**: `test_o261e_f5_alcance.py:264`.

Ficaram fora LU-01, LU-04, LU-05 e LU-06: residual `decidir` continua `chamado` (`test_e3_degrau_dono_e_porta.py:53`); documento sem porta continua sem dono (`:91`); agrupamento `ninguem` permanece (`folha/retrato_folha.py:192`); universo da carga não foi alterado. Integração ORM, remedida e totais novos de produção: **[nao verificado]**. Registro sem construção: **PORTA-DO-ADMIN-NA-PERGUNTA-VENCIDA**, número informado no anexo **2.333**; remedida **[nao verificado]**.

## 14/11 19:27 · sessao c4cb8db0cbc8 · BO-DISPUTA-SEM-PERGUNTA · leitura · origem=Code

Referências relativas a `app/`, salvo indicação contrária. **Cópia `arquivo:+N`** significa a linha do lado novo do `diff_obra.patch` fornecido. “Push sim” significa tentativa de envio; entrega real e execução dos testes citados: **[nao verificado]**. Os testes novos da cópia foram apenas listados no anexo; sua cobertura: **[nao verificado]**. [nao verificado]

Estados: **a** ata lavrada; **b** sem ata; **c** sem marcos/lâmpadas na ata; **d** pergunta respondida; **e** pergunta morta pelo C6. Respondida pode continuar viva, mas deixa de ser pendência do colaborador no app (`chamados/juizes.py:1417`; `chamados/services/pergunta_no_app.py:71`).

**Porta 1 — criar ou atualizar disputa**

`chamados/services/disputa_emissao.py::criar_ou_atualizar_disputa:644` → `_abrir_ou_agregar_disputa:474`. Chamadores: views de batida `api/views.py:764`, `api/views_core.py:758`; triagem `ponto/services/triagem_batida.py:735`; command `marcar_foto_ausente_retro.py:96`; fábrica da célula `chamados/services/fabrica_da_celula.py:172`. Esta última é chamada pelo cartório, reconciliador e backfill (`ponto/services/cartorio.py:1045`; `chamados/reconciliador.py:1387`; `chamados/management/commands/backfill_fio_morto_e4b.py:143`).

| ID — entrada | Juiz consultado | O que persiste no hash | Push / destinatário | Prova; teste existente | Na cópia | [nao verificado]
|---|---|---|---|---|---|
| N0 — motivos vazios; qualquer estado | Nenhum; guarda local | Nada | Não | `disputa_emissao.py:656` | Guarda preservada. |
| N1 — a; DS inexistente ou somente fechadas | Nenhum para nascer; procura apenas DS aberta | Cria/reusa chamado-pai; cria `DS.motivos`, `contexto`, `chamado` **antes** da geração | Sim, ao colaborador, mesmo se o gerador não entregar pergunta local | `disputa_emissao.py:531`, `:568`, `:587`, `:669` | Nascimento passa por ensaio; sem pergunta local admitida, desfaz o savepoint. Reuso estrangeiro pode ser repetido na dona (`disputa_emissao.py:+435`). |
| N2 — b/c; DS inexistente ou somente fechadas | Nascimento não consulta ata; geração usa contexto/grade conforme insumo disponível | DS/pai já persistem. Pode criar pergunta com data do contexto; pode terminar sem pergunta | Sim, ao colaborador | `disputa_emissao.py:345`, `:350`, `:360`, `:371`, `:425`, `:669` | Resolvedor comum e ensaio evitam parte dos nascimentos vazios. **“Pergunta local admitida” não significa C6 viva**: `locais()` exclui vias, sem consultar C6 (`disputa_emissao.py:+435`). |
| N3 — a/b/c; DS aberta | Nenhum para agregar; união e merge locais | Atualiza `DS.motivos/contexto`; contexto registra `qtd/ultima_em` | Ao colaborador somente com motivo novo; outro dia do mesmo motivo pode gerar pergunta sem push | `disputa_emissao.py:535`, `:548`, `:565`, `:682`, `:685`, `:686` | Agrega motivos locais; encaminha estrangeiros à dona. Contexto continua podendo ser salvo antes da geração (`disputa_emissao.py:+387`, `:+408`, `:+435`). | [nao verificado]
| N4 — d/e na própria DS | Dedup local por existência; nenhum C6 nesse predicado | Pode atualizar DS como N3, sem criar pergunta nova | Regra N1/N3; resposta ou morte não são guarda desse push | `disputa_emissao.py:325`, `:398`, `:417`, `:669`, `:686` | Dedup por existência permanece; efeito passa pela pendência do app (`disputa_emissao.py:+213`, `:+1828`; `pergunta_no_app.py:+84`). | [nao verificado]
| N5 — pergunta canônica pertence a outra DS aberta/fechada | Identidade do fato; juiz da resolvedora e guarda da costura | Pode reutilizar/atualizar pergunta estrangeira e deixar a DS nova sem filhos; `_criada` não governa o retorno do gerador | Pode enviar ao colaborador mesmo com DS local vazia | `disputa_emissao.py:444`, `:470`; `chamados/models.py:2977`, `:3035`, `:3064` | Distingue `ReusadaDeOutraDS`; desfaz nascimento sem pergunta local; agrega na dona aberta. Na fechada, não salva motivo: registra descarte após commit (`disputa_emissao.py:+435`; `descarte_disputa.py:+5`). |

**Porta 2 — gerar/reaproveitar perguntas e entradas pendentes**

`chamados/services/disputa_emissao.py::gerar_perguntas_disputa:300`; `::perguntar_entradas_pendentes:257`. Chamadores internos: wrapper e agrupador (`disputa_emissao.py:670`, `:682`, `:685`, `:1270`). Há geração adicional pela fábrica e pelo command smoke (`chamados/services/fabrica_da_celula.py:174`; `chamados/management/commands/smoke_chamados.py:103`).

| ID — entrada | Juiz consultado | O que persiste no hash | Push / destinatário | Prova; teste existente | Na cópia | [nao verificado]
|---|---|---|---|---|---|
| G1 — a/b/c; texto/data resolvíveis | GEOMETRIA DE TURNO para batida; dados da célula/contexto; destino derivado separadamente | `PerguntaDisputa.abrir` cria ou retorna canônica; grava dia/destino/célula/lavra conforme nascimento ou reuso | Nenhum dentro do gerador; wrapper decide | `disputa_emissao.py:345`, `:350`, `:371`, `:391`, `:411`, `:444` | `_resolver_pergunta_por_motivo` resolve dia antes de texto/dedup/destino (`disputa_emissao.py:+213`). |
| G2 — sem dia para pergunta de hora | Nenhum; ausência local de insumo | Nenhuma pergunta; não desfaz DS já criada pelo wrapper | Wrapper ainda pode enviar | `disputa_emissao.py:424`, `:425`, `:430` | Causa `InsumoAusente`; núcleo pode desfazer o nascimento (`disputa_emissao.py:+213`, `:+435`). |
| G3 — d/e local, mesma identidade, fora das vias excluídas | Nenhum; existência local | Não cria; dedup não lê resposta nem C6 | Nenhum próprio | `disputa_emissao.py:325`, `:398`, `:417` | Predicado permanece no resolvedor (`disputa_emissao.py:+213`). |
| G4 — canônica de outra DS, aberta ou fechada | Porta do fato `PerguntaDisputa.abrir` | Retorna a pergunta estrangeira; não transfere sua `disputa` | Nenhum próprio | `chamados/models.py:2833`, `:2977`, `:3064`; `disputa_emissao.py:470` | Retorno recebe causa própria e argumentos para replay; wrapper trata propriedade da pergunta (`disputa_emissao.py:+369`, `:+435`). |
| G5 — lista de entradas pendentes | GEOMETRIA DE TURNO / `dia_do_turno_da_batida`; FURO DO DIA / `classificar_falta` | Injeta contexto por dia, chama gerador; salva `DS.contexto` quando há retorno | Nenhum próprio | `disputa_emissao.py:282`, `:288`, `:291`, `:295`, `:296` | Restaura contexto em memória, agrega conforme dona local/estrangeira e coleta criadas (`disputa_emissao.py:+136`). |
| G6 — fábrica da célula | CÉLULA/PRECEDÊNCIA; motivos pela ata | Desfaz carimbo sem lastro; chama wrapper e depois chama gerador novamente se recebeu DS | Silenciado no julgamento da célula | `chamados/services/fabrica_da_celula.py:139`, `:169`, `:172`, `:174`, `:218` | Arquivo não alterado no diff; chamada adicional permanece. |

O contrato final do dia já existe no hash: `_sob_contrato` exige saída com dia do juiz e desfaz a chamada se o contrato falhar; isso não uniformiza as decisões anteriores do gerador (`chamados/models.py:2862`; `chamados/services/fio.py:1140`). Testes dessa porta: `chamados/tests/test_e1_v15e_contrato_de_saida.py`, referenciado em `chamados/models.py:2872`; teste de derivação pelo dia da pergunta em `chamados/tests/test_selo_derivacao_pela_data_da_pergunta.py:80`.

**Porta 3 — órfãos, `orfao_14h` e complementação**

`chamados/services/disputa_emissao.py::criar_disputa_a_partir_de_chamados_orfaos`; auxiliares `::_criar_pergunta_se_couber:823`, `::_complementar_marcos_faltantes:850`, `::_ata_do_dia:887`.

Chamadores: commands `processar_alertas_turno.py:385`, `processar_alertas_avancados.py:159`, `detectar_ausencias.py:803`, agendados respectivamente em `*/5`, `*/30`, `*/5` (`config/crons.py:362`, `:363`, `:364`); ação de view `chamados/services/acoes_disputa.py:222`; cobrar-dia `chamados/services/cobrar_dia.py:49`; passivo `chamados/services/pergunta_no_app.py:246`, acionado pelo command `perguntar_chamados_sem_pergunta.py:20`.

| ID — entrada | Juiz consultado | O que persiste no hash | Push / destinatário | Prova; teste existente | Na cópia | [nao verificado]
|---|---|---|---|---|---|
| O1 — elegível; DS inexistente/somente fechadas | Nenhum para nascer; busca DS aberta | Cria pai e DS antes das guardas; motivo inicial `orfao_14h` | Depende dos ramos seguintes | `disputa_emissao.py:1096`, `:1101`, `:1123` | Nascimento antecipado permanece. Rollback específico: nova + retorno estrangeiro + nenhuma pergunta local (`disputa_emissao.py:+1146`). |
| O2 — a, marco apagado, rota por ata; DS aberta/nova | DIA DO CHAMADO / `data_do_chamado`; leitura dos marcos/lâmpadas | Cria/reusa pergunta por marco; agrega `DS.motivos`; chamado vai a análise | Sim ao colaborador, se não fechar e `notificar` | `disputa_emissao.py:1166`, `:1170`, `:1175`, `:873`, `:837`, `:1289` | Encaminha estrangeira à dona; push exige pendentes da DS e DS ainda aberta (`disputa_emissao.py:+1146`, `:+1363`). | [nao verificado]
| O3 — b, com escala vigente; DS aberta/nova | `_marcos_faltantes`; guarda local de `sem_celula` e escala vigente | DS/pai ficam; nenhuma pergunta; abstenção impede auto-fecho | **Sim ao colaborador no hash**, se `notificar` | `disputa_emissao.py:902`, `:908`, `:911`, `:1279`, `:1289`; teste espera DS aberta com zero perguntas: `chamados/tests/test_hx_disputa_muda_sem_ata.py:51` | **DS aberta vazia permanece.** Novo filtro suprime push sem pendentes (`disputa_emissao.py:+1363`). |
| O4 — c: ata de trabalho sem pares de marco/lâmpada | Leitor devolve `sem_celula` | Mesmo resultado de O3, havendo escala vigente | Mesmo resultado de O3 | `chamados/services/declaracao_texto.py:121`, `:126`; `disputa_emissao.py:902` | Predicado preservado; efeito filtrado como O3. |
| O5 — b, sem escala vigente | FURO DO DIA; exceção local da guarda de ata | Permite pergunta por batidas; não exige ata impossível dessa escala | Sim se permanece aberta | `disputa_emissao.py:908`, `:910`, `:1200`, `:1205`, `:1213` | Exceção preservada no hunk do agrupador (`disputa_emissao.py:+1146`). |
| O6 — falha ao ler ata | Nenhum; `except` local | `_ata_do_dia` devolve `True, None`; não bloqueia pergunta | Pode enviar se permanece aberta | `disputa_emissao.py:897`, `:901` | Ramo não alterado. |
| O7 — a, sem falta acusada; DS aberta/nova | FURO DO DIA; depois DISPUTA FECHA / C7 | Pode ter criado DS/pai; fecha DS se C7 permite e não houve abstenção | Não depois do auto-fecho | `disputa_emissao.py:1201`, `:1238`, `:1278`, `:1282`, `:1284`; `chamados/tests/test_hx_disputa_muda_sem_ata.py:63` | Conserva auto-fecho e exceção por falta de ata (`disputa_emissao.py:+1146`). |
| O8 — d, pergunta ainda viva; DS aberta | C6 por `fio_vivo` | `_ja_perguntou` bloqueia nova; resposta permanece | Hash não exige que o colaborador ainda deva resposta | `disputa_emissao.py:1415`, `:1422`, `:832`, `:1289` | Push exige pendência do app, que exclui respondida (`disputa_emissao.py:+1363`; `pergunta_no_app.py:71`). | [nao verificado]
| O9 — e, colaborador ativo | C6 por `fio_vivo` | Permite tentar `abrir`; canônica pode ser reutilizada em vez de nascer outra | Depende de permanecer aberta no hash | `disputa_emissao.py:1415`, `:1422`, `:837`; `chamados/models.py:2977` | Trata retorno estrangeiro e filtra push (`disputa_emissao.py:+840`, `:+1363`). |
| O10 — e, colaborador não ativo | Nenhum; guarda local de existência | Linha existente bloqueia geração antes de consultar C6 | Depende das demais perguntas/fluxo | `disputa_emissao.py:1416`, `:1420` | Predicado não alterado. |
| O11 — intervalo explícito; a/b/c | Nenhum juiz de ata nesse ramo; lê `contexto.marcos_faltantes` | Pergunta por marco com data do contexto e destino `_chi`; não passa por `_ata_do_dia` | Sim se permanece aberta | `disputa_emissao.py:1138`, `:1155`, `:1289` | Fotografa fatos em `_marcos_de_intervalo`; destino passa pelo eleitor comum (`disputa_emissao.py:+1146`; `fio.py:+752`). |
| O12 — chamado já tem pergunta não materializada; dona aberta **ou fechada** | Proxy SQL `materializada_em IS NULL`; depois leitor da ata | Complementa e pode salvar `DS.motivos` da dona fechada | Pode retornar antes do push principal | `disputa_emissao.py:1070`, `:1073`, `:1081`, `:1084`, `:1093` | Agregador não salva dona fechada; registra descarte. Complementos só notificam dona aberta (`disputa_emissao.py:+1092`, `:+408`; `descarte_disputa.py:+5`). | [nao verificado]
| O13 — sem origem e sem data/base | DIA DO CONTEXTO; GEOMETRIA DE TURNO ou DIA DE TRABALHO | Pergunta não nasce; DS/pai já existem; C7 pode fechar | Não se auto-fechar | `disputa_emissao.py:1232`, `:1235`, `:816`, `:820`, `:1282` | Guardas preservadas no agrupador (`disputa_emissao.py:+1146`). |

**Porta 4 — âncora, eleição e escrita do destino**

`chamados/services/disputa_emissao.py::criar_disputa_ancora_de_chamado:1302`, chamada pela ação de view `chamados/services/acoes_disputa.py::nucleo_pedir_confirmacao_ancora:178`, em `:189`.

| ID — entrada/porta | Juiz consultado | O que persiste no hash | Push / destinatário | Prova | Na cópia | [nao verificado]
|---|---|---|---|---|---|
| A1 — chamado não é `ancora_limbo` ou está encerrado | Nenhum; predicado local | Nada | Não | `disputa_emissao.py:1316`, `:1318` | Guarda preservada. |
| A2 — d/e, pergunta de âncora não materializada já existe | Nenhum; proxy SQL, sem C6 | Nada nessa chamada | Não | `disputa_emissao.py:1323` | Guarda preservada. |
| A3 — a/b/c; sem pergunta bloqueadora; DS aberta/nova | Nenhuma consulta à ata/marcos; busca DS aberta | Cria/agrega DS; cria **conversa**, destino `chamado_limbo`; chamado vai a análise | Sim ao colaborador | `disputa_emissao.py:1332`, `:1358`, `:1367`, `:1371`, `:1377`, `:1381` | Agregação central; push filtrado após commit. Conversa continua recebendo limbo diretamente (`disputa_emissao.py:+1411`, `:+1828`). | [nao verificado]
| A4 — eleição na emissão datada | CHAMADO: dia/motivos atendidos; desempate local por turno | Escolhe chamado vivo; exclui candidato já com pergunta não materializada do mesmo motivo; pode devolver nenhum | Nenhum próprio | `disputa_emissao.py:156`, `:202`, `:227`, `:241` | `_derivar_chamado_destino` delega ao novo eleitor; intervalo explícito precede chamado genérico; empate usa menor PK (`disputa_emissao.py:+98`; `fio.py:+752`). |
| A5 — eleição em outros caminhos | Órfãos escolhem primeiro do grupo; declaração pede seu chamado; âncora pede limbo | Destino recebido pela pergunta pode depender da porta de entrada | Nenhum próprio | `disputa_emissao.py:1197`, `:1377`; `chamados/services/declaracao_texto.py:376` | Emissores datados passam pelo eleitor; outros caminhos abaixo continuam escolhendo destino. |
| A6 — reatar histórico / substituir chamado terminal | CHAMADO: resolvedora válida; C8 para sobrevivente | Reaponta pergunta ao histórico único ou sobrevivente escolhido fora do emissor | Não próprio | `chamados/reconciliador.py:1586`, `:1624`, `:1643`; `chamados/models.py:374`, `:390`, `:437` | Esses eleitores permanecem; guarda nova preserva intervalo já eleito, sem substituir todas as eleições (`fio.py:+808`). |
| A7 — costura existente | `resolvedora_valida` + `_costura_nao_mata_o_fio` | Atualiza `PerguntaDisputa.chamado_destino`, eventualmente dia, e relavra | Não próprio | `chamados/services/fio.py::reapontar_resolvedora:804`, `:906`, `:926` | Atribuição/persistência passam por `gravar_destino_da_pergunta`; guarda protege intervalo eleito (`fio.py:+785`, `:+886`). |

O pai da DS também é escolhido por três nascimentos com argumentos distintos de `ChamadoColaborador.abrir`: núcleo comum, agrupador manual e âncora (`disputa_emissao.py:568`, `:1104`, `:1339`). O diff não os transforma em um único nascimento (`disputa_emissao.py:+435`, `:+1146`, `:+1411`).

**Porta 5 — declaração de horário**

`chamados/services/declaracao_texto.py::declarar_hora_colab:307`, `::aplicar_declaracao:631` → `disputa_emissao.py::disputa_para_declaracao:610`. São caminhos do colaborador e da aplicação administrativa da proposta, respectivamente (`declaracao_texto.py:313`, `:632`).

| ID — entrada | Juiz consultado | O que persiste no hash | Push / destinatário | Prova; teste existente | Na cópia | [nao verificado]
|---|---|---|---|---|---|
| D1 — sem dia; b/c; ou hora não casa com marco | DIA DO CHAMADO; leitor da ata; casamento hora→marco | Recusa antes de criar DS/pergunta | Não | `declaracao_texto.py:330`, `:334`, `:341` | Guardas anteriores preservadas. |
| D2 — a, marco faltante; DS aberta/nova | Núcleo da DS; canônica do fato; depois resposta/validação | Cria/agrega DS; cria/reusa pergunta com dia da célula e destino solicitado; grava resposta; admin valida | Não pela declaração | `declaracao_texto.py:360`, `:363`, `:376`, `:659`, `:666`, `:696`; ausência de push coberta em `chamados/tests/test_porta_disputa_emissao.py:223` | DS/pergunta ficam no mesmo ensaio; retorno estrangeiro desfaz ensaio local e repete na dona (`disputa_emissao.py:+614`). |
| D3 — d, mesma resposta; ou e por validação sem efeito no marco | Comparação local / `_resposta_validada` | Pode ter agregado/criado DS antes de retornar `ja_declarada` ou `validada_sem_registro` | Não | `declaracao_texto.py:378`, `:412`, `:417` | Recusas permanecem depois do novo ato conjunto (`declaracao_texto.py:+357`; `disputa_emissao.py:+614`). |
| D4 — canônica de outra DS aberta/fechada | Identidade do fato | Reuso conserva dona original; DS local pode ter sido criada anteriormente | Não | `chamados/models.py:2833`; `declaracao_texto.py:360`, `:376` | Replay na dona; fechada recebe descarte de motivo, sem reabertura pelo agregador (`disputa_emissao.py:+614`, `:+408`). |

**Porta 6 — demais atualizações de DisputaSupervisao**

O censo atribui os escritores a `chamados/models.py` e `chamados/services/disputa_emissao.py`; os atos são declarados nos selos (`chamados/tests/test_porta_disputa_ciclo.py:81`, `:86`; `chamados/tests/test_porta_disputa_emissao.py:84`). As portas abaixo não distinguem a/b/c salvo pelas perguntas/guardas indicadas.

| ID — porta / entrada | Juiz ou predicado | Persistência | Chamadores; push | Na cópia | [nao verificado]
|---|---|---|---|---|
| U1 — `DisputaSupervisao.fechar:1911`; aberta, inclusive d/e | C7: nenhuma viva e nenhuma resposta sem veredito; exceções nomeadas não dispensam resposta sem veredito | `fechada_em/por`, `resolucao`, `motivo_fechamento`; exceções podem encerrar perguntas mudas | Escrita `models.py:1992`; reconciliador `:413`, `:613`, `:1172`; materialização `materializacao.py:592`, `:1339`; tranca `competencia_trancada.py:229`; desligamento `colaboradores/services/desligamento.py:167`. Signal tenta resolver pai, sem push próprio (`chamados/signals.py:20`, `:43`). | C7 e signal não alterados no diff. |
| U2 — `aparar_motivo_respondido:2008`; motivo presente | Nenhum; fato respondido por outro caminho | Remove motivo e contexto; não fecha sozinha (`models.py:2024`) | Upload de foto (`api/foto_recebida.py:72`); sem push nessa porta | Sem mudança. |
| U3 — `remover_motivo:2027`; aberta e motivo presente | Predicados locais; C7 no eventual fechamento | Retrata perguntas; atualiza motivos/contexto; tenta fechar se não sobrou motivo (`models.py:2121`, `:2128`) | Signals de posto e âncora (`colaboradores/signals.py:77`; `escala/signals.py:174`); sem push próprio | Sem mudança. |
| U4 — `anotar_contexto`; a/b/c/d/e | Nenhum; primeira presença da chave | Atualiza contexto uma vez (`models.py:2157`, `:2161`) | View `chamados/views.py:1343`; command escalonamento `escalonar_chamados_supervisao.py:159`; efeitos ficam nos chamadores | Sem mudança. |
| U5 — `renovar_contexto:2164`; a/b/c/d/e | Nenhum; valor diferente | Atualiza contexto (`models.py:2175`, `:2179`) | Cron diário (`alertar_disputas_pendentes.py:48`), detalhado abaixo | Cópia desloca chamada do cron para callback condicionado a pendência (`alertar_disputas_pendentes.py:+41`). |
| U6 — `reabrir_admin:2182`; fechada | Nenhum; exige usuário e teto de duas | Limpa fechamento/resolução; incrementa contador; motivo opcional (`models.py:2194`, `:2217`) | Ação de view `acoes_disputa.py:259`, delegada pelo fio `fio.py:583`; depois cria conversa em `acoes_disputa.py:263`; push supervisão/colaborador em `:277`, `:288` | Só aviso ao cola

[truncado em 20000 de 34735 caracteres]

## 14/11 19:23 · sessao c50cfa1332dd · - · - · origem=Ronald (tmux)

Censo de escrita DS executado **sem Django, banco ou logs**, com `python3 -B`, usando o varredor puro existente; ele classifica construções, saves, gerentes e inferências, sem garantir resolução de todo alias dinâmico (`app/core/censo_escritas.py:1`, `app/core/censo_escritas.py:451`, `app/core/censo_escritas.py:469`).

**19 sítios atribuídos de escrita DS no hash**, todos nestes dois módulos (`app/core/portas.py:443`):

| Porta | Persistência | Chamadores identificados |
|---|---|---|
| `DisputaSupervisao.fechar` | `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento`; save `app/chamados/models.py:1992` | Reconciliador `app/chamados/reconciliador.py:413`, `:613`, `:1172`; materialização `app/chamados/services/materializacao.py:592`, `:1339`; tranca `app/chamados/services/competencia_trancada.py:229`; becos `app/chamados/services/becos.py:57`; upload foto `app/api/foto_recebida.py:80`; entrada sincronizada `app/ponto/services/entrada_sincronizada.py:60`; emissão `app/chamados/services/disputa_emissao.py:1284`, `:2092`; desligamento `app/colaboradores/services/desligamento.py:167`; comandos `app/chamados/management/commands/encerrar_perguntas_com_fato.py:88`. |
| `aparar_motivo_respondido` | `motivos`, `contexto`; save `app/chamados/models.py:2024` | Upload de foto, `app/api/foto_recebida.py:72`. |
| `remover_motivo` | retrata perguntas; `motivos`, `contexto`; save `app/chamados/models.py:2121`; chama `fechar` se nenhum motivo, `:2128` | Signals de posto `app/colaboradores/signals.py:77` e âncora `app/escala/signals.py:174`. |
| `anotar_contexto` | primeira anotação apenas; save `app/chamados/models.py:2161` | View `app/chamados/views.py:1343`; comando escalonamento `app/ponto/management/commands/escalonar_chamados_supervisao.py:159`. |
| `renovar_contexto` | se valor mudou; save `app/chamados/models.py:2179` | Alerta diário `app/ponto/management/commands/alertar_disputas_pendentes.py:48`. |
| `reabrir_admin` | limpa fechamento/resolução, incrementa teto administrativo, agrega motivo opcional; save `app/chamados/models.py:2217` | Ação de view `app/chamados/services/acoes_disputa.py:259`, delegada pelo fio `app/chamados/services/fio.py:583`. |
| `reabrir_sistema` | limpa fechamento/resolução; save `app/chamados/models.py:2233` | Ressurreição no chamado `app/chamados/models.py:1622`; relavra `app/chamados/services/relavra_via.py:261`; validação recusada `app/chamados/services/validacao.py:241`; materialização de ausência desfeita `app/chamados/services/materializacao.py:692`; comando `app/chamados/management/commands/reabrir_orfas_enterradas.py:165`. |
| `reabrir_por_colab` | questionário volta a pendente e incrementa `reaberturas_colab`; save `app/chamados/models.py:2296` | Views HTML `app/chamados/views.py:1334` e API `app/api/views.py:2679`. |
| `limpar_resolucao_residual` | apenas `resolucao` de aberta; save `app/chamados/models.py:2322` | Nenhum chamador produtivo encontrado; contrato declara a porta (`app/core/portas.py:405`); alcance de invocação externa [nao verificado]. |
| `perguntar_entradas_pendentes` | `contexto`; save `app/chamados/services/disputa_emissao.py:296` | Wrapper `app/chamados/services/disputa_emissao.py:685`; órfãos `:1270`. |
| `_abrir_ou_agregar_disputa` | atualiza motivos/contexto `app/chamados/services/disputa_emissao.py:565`; cria DS `:587` | Wrapper `:662`; declaração `:628`; comando smoke `app/chamados/management/commands/smoke_chamados.py:100`; helper sem chamador `_disputa` `app/ponto/management/commands/reconciliar_grade.py:424`. |
| Agrupador de órfãos | saves/criação `app/chamados/services/disputa_emissao.py:1084`, `:1123`, `:1132`, `:1161`, `:1256` | Fluxo auditado pelo outro explorer; cinco sítios também declarados em `app/chamados/tests/test_porta_disputa_emissao.py:86`. |
| Âncora | cria DS `app/chamados/services/disputa_emissao.py:1358`; atualiza motivos `:1367` | `nucleo_pedir_confirmacao_ancora`, `app/chamados/services/acoes_disputa.py:189`; cascas de detalhe e fio declaradas em `:179`. |

O comentário antigo que diz desligamento por `.update()` está ultrapassado: o código atual percorre as abertas e chama `fechar`, retendo resposta sem veredito (`app/colaboradores/services/desligamento.py:157`, `:164`, `:167`). Smoke e `_disputa` são **chamadores do núcleo**, não criações DS adicionais; smoke chama também o gerador, e `_disputa` é removido pelo diff (`app/chamados/management/commands/smoke_chamados.py:100`, `:103`; `app/ponto/management/commands/reconciliar_grade.py:424`; `diff_obra.patch`, hunk `@@ -409,23 +409,7 @@`).

**Portas adicionais de pergunta/âncora**

| Estado/porta | Juiz/predicado | Persistência e push | Cópia | [nao verificado]
|---|---|---|---|
| Declaração do colaborador sem dia | `dia_do_fato`; sem dia recusa | Nada, retorna `sem_data` (`app/chamados/services/declaracao_texto.py:330`). | Preserva guarda; altera operação posterior (`diff_obra.patch`, declaração, hunk `@@ -357,24 +357,13 @@`). |
| Sem ata | `_marcos_faltantes`; `sem_celula` | Recusa antes DS/pergunta; acende falta de ata (`app/chamados/services/declaracao_texto.py:334`). | Guarda anterior preservada pelo diff. |
| Com ata, sem marco que case | `_casar_hora` | Nada; `nao_casa` (`app/chamados/services/declaracao_texto.py:341`). | Guarda anterior preservada pelo diff. |
| Com marco faltante; DS aberta / inexistente | Núcleo DS; depois canônica do universo do fato, `fio.canonica_do_grupo` | DS criada/agregada; pergunta reusada localmente ou `abrir` devolve canônica até de outra DS; destino pedido é `ch`, dia é célula (`app/chamados/services/declaracao_texto.py:360`, `:363`, `:376`; `app/chamados/models.py:2833`). Declaração não manda push (`app/chamados/tests/test_porta_disputa_emissao.py:223`). | DS e pergunta entram no mesmo ensaio; retorno estrangeiro desfaz ensaio e repete na dona; destino passa `_destino_do_intervalo` (`diff_obra.patch`, `_pergunta_para_declaracao`, novas linhas a partir de `:614`). |
| Pergunta validada embora marco siga faltante | `_resposta_validada` | Retorna `validada_sem_registro`; **DS já foi criada/agregada antes** (`app/chamados/services/declaracao_texto.py:378`, `:412`). | Mantém a recusa, reorganiza nascimento (`diff_obra.patch`, declaração, hunk `@@ -357,24 +357,13 @@`). |
| Pergunta já respondida igual à hora | comparação local | `ja_declarada`; sem nova resposta (`app/chamados/services/declaracao_texto.py:417`). | Comparação posterior preservada pelo diff. |
| Declaração administrativa | proposta válida, mesma canônica; depois `validar_pergunta` | DS/pergunta como acima; responde se necessário e valida; sem proposta não escreve (`app/chamados/services/declaracao_texto.py:649`, `:659`, `:666`, `:687`, `:696`). | Passa argumentos da pergunta ao ato DS; replay em dona estrangeira (`diff_obra.patch`, hunk `@@ -656,15 +645,13 @@`). |
| Âncora: categoria diferente/encerrada | nenhum: predicados locais | Nada (`app/chamados/services/disputa_emissao.py:1316`). | Sem alteração desses predicados no diff. |
| Âncora: pergunta não materializada existente, respondida ou C6 morta | nenhum: `materializada_em IS NULL` | Nada, **não consulta C6** (`app/chamados/services/disputa_emissao.py:1323`). | Guarda preservada no diff. |
| Âncora sem tal pergunta; DS aberta / inexistente ou só fechada | nenhuma consulta à ata/marcos; busca DS aberta | Agrega ou cria DS; cria conversa `ancora_ausente`, destino literal `chamado_limbo`; chamado vai a análise; solicita push ao colab (`app/chamados/services/disputa_emissao.py:1332`, `:1358`, `:1371`, `:1377`, `:1379`, `:1381`). | Agregação passa `_agregar_motivos_na_dona`; push herda filtro de pendência pós-commit; eleição datada não entra porque conversa (`diff_obra.patch`, hunk `@@ -1332,7 +1411,7 @@`; `@@ -1755,38 +1828,36 @@`). |
| Reabertura administrativa de DS fechada | predicados locais: staff/grupo, motivo válido, justificativa, teto dois | Reabre DS primeiro; depois cria conversa nova, sem destino explícito; reabre chamado se encerrado; push supervisão e colab (`app/chamados/services/acoes_disputa.py:245`, `:255`, `:259`, `:263`, `:269`, `:276`, `:288`). | Filtro de pendência pós-commit no push ao colab; ordem DS→pergunta permanece (`diff_obra.patch`, hunk `@@ -285,18 +293,18 @@`). |
| Reabertura de questionário | `perguntas_corrigiveis` | Limpa texto/resposta/validação via `reabrir_para_correcao`; guarda foto na sequência; não gera pergunta nova (`app/chamados/services/disputa_emissao.py:1582`, `:1593`, `:1615`, `:1632`). View própria e ação fio mandam push (`app/chamados/views.py:1435`, `:1444`; `app/chamados/services/acoes_disputa.py:102`, `:109`). | Push ao colab filtrado depois do commit (`diff_obra.patch`, ações hunk `@@ -90,6 +90,7 @@`; views hunk `@@ -1444,12 +1444,13 @@`). |
| Fábrica de célula | motivos emitidos pela ata; remove carimbo sem lastro | Chama wrapper e **chama gerador novamente se DS retornou** (`app/chamados/services/fabrica_da_celula.py:160`, `:169`, `:172`, `:174`). Julgamento silencia push (`:218`). | Arquivo fora do diff; chamada duplicada preservada. |

Testes de existência/contrato, sem executá-los: declaração e ausência de push `app/chamados/tests/test_porta_disputa_emissao.py:185`, `:199`, `:211`, `:223`; reabertura colab e teto `app/chamados/tests/test_porta_disputa_ciclo.py:283`, `:297`, `:311`, `:322`; porta de dia `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:165`; cobertura dos novos testes listados no anexo [nao verificado].

**I3: NÃO FECHA globalmente, hash e cópia.**

No hash, eleição da emissão é `_derivar_chamado_destino`; órfãos escolhem primeiro chamado da ordem recebida; declaração pede seu chamado; âncora pede limbo (`app/chamados/services/disputa_emissao.py:156`, `:1197`, `:1377`; `app/chamados/services/declaracao_texto.py:376`). Escrita persistente está em dois corpos: `models._nascer` e `fio.reapontar_resolvedora`, explicitamente reconhecido (`app/chamados/services/fio.py:808`, `app/chamados/models.py:3038`, `:3070`, `app/chamados/services/fio.py:926`).

A cópia cria um eleitor comum aos emissores datados e um escritor de destino, mas conserva outros escolhendo destino: C8 escolhe sobrevivente no gate do chamado; reconciliador escolhe histórico único; limbo recebe pai imposto; migração recebe novo chamado (`app/chamados/models.py:374`, `:390`, `:437`; `app/chamados/reconciliador.py:1586`, `:1624`, `:1643`; `app/chamados/services/limbo_folgas.py:226`, `:245`; `app/chamados/services/fio.py:2175`). Esses sítios não são alterados no diff; guarda nova só preserva intervalo já eleito, não elege universalmente (`diff_obra.patch`, fio hunk `@@ -752,7 +808,32 @@`).

**I4: NÃO FECHA no ciclo inteiro, hash e cópia; o escritor final do campo já é único.**

`gravar_dia_do_fato` já é o ponto único de atribuição, e o contrato final exige dia do juiz para fato/conversa civil separadamente (`app/chamados/services/fio.py:791`, `:801`; `app/chamados/models.py:2862`, `:2868`, `:2875`). Isso não unifica as decisões anteriores: órfão com origem classifica o dia **civil** e pode sair antes de resolver dia **do turno** e ler sua ata (`app/chamados/services/disputa_emissao.py:1199`, `:1200`, `:1201`, `:1205`, `:1209`). A sequência permanece na cópia (`diff_obra.patch`, órfãos hunk `@@ -1098,176 +1146,202 @@`). Ocorrência/frequência em produção [nao verificado].

O gerador do hash também monta texto/dedup antes do dia final do modelo; a cópia resolve esses três insumos por `_resolver_pergunta_por_motivo`, melhoria local que não elimina a sequência civil→turno dos órfãos (`app/chamados/services/disputa_emissao.py:345`, `:372`, `:399`; `app/chamados/models.py:2931`; `diff_obra.patch`, `_resolver_pergunta_por_motivo`, novas linhas `:209` seguintes).

## 14/11 19:22 · sessao b468514477ca · E3-V10-DEGRAU-DONO-E-PORTA · verificador · origem=Code

**Veredito: REPROVADO nesta volta.** Há contraexemplo à L-129, divergência entre os universos da carga e do app, dois mutantes obsoletos e insuficiência do selo AST para provar consumo da tarefa. Referências: `app/chamados/services/tarefa_do_dia.py:277`, `app/chamados/services/pergunta_no_app.py:52`, `app/chamados/tests/e3_degrau_mutantes.py:41`, `app/folha/tests/test_l129_fora_da_folha_so_le_a_tarefa.py:57`.

**1. Escritores e leitores**

| Campo/valor | Quem fornece/escreve | Quem lê |
|---|---|---|
| `fio` | Seleção em `app/chamados/services/tarefa_do_dia.py:622`; publicação em `:635` | Projeção em `app/chamados/services/tarefa_do_dia.py:645`; Folha em `app/folha/fora_da_folha.py:85`; vigia em `app/chamados/services/vigia_fio.py:663`; instrumento em `app/chamados/management/commands/diff_e3_sombra.py:54` |
| `rota` | `app/chamados/services/tarefa_do_dia.py:629`; publicação em `:635` | Projeção em `app/chamados/services/tarefa_do_dia.py:645`; furo em `app/chamados/services/furo_sem_canal.py:99`; vigia em `app/chamados/services/vigia_fio.py:662`; instrumento em `app/chamados/management/commands/diff_e3_sombra.py:53` |
| `fila` | Calculada em `app/chamados/services/tarefa_do_dia.py:609`, a partir da alimentação de `app/chamados/juizes.py:1589` | Folha nomeia chamados por ela em `app/folha/fora_da_folha.py:244`; instrumento em `app/chamados/management/commands/diff_e3_sombra.py:143` |
| `verbos` | Painel fornece por `app/chamados/juizes.py:1570`; tarefa seleciona em `app/chamados/services/tarefa_do_dia.py:608` | Residual do próprio juiz em `app/chamados/services/tarefa_do_dia.py:618`; projeção em `:645`; instrumento em `app/chamados/management/commands/diff_e3_sombra.py:144` |
| `donos` | **Conjunto local, não campo publicado**: `app/chamados/services/tarefa_do_dia.py:615` | Seleção do fio em `app/chamados/services/tarefa_do_dia.py:622`; publicação não inclui `donos`, em `:635` |
| Retorno de `porta_da_tarefa` | Cópia dos cinco campos ou `None`: `app/chamados/services/tarefa_do_dia.py:645` | Folha `app/folha/fora_da_folha.py:70`, `:238`; vigia `app/chamados/services/vigia_fio.py:649`; furo `app/chamados/services/furo_sem_canal.py:97`; beco `app/escala/services/cadastro_realidade.py:469`; sombra `app/chamados/management/commands/diff_e3_sombra.py:43` |
| Pendentes/histórico | `particao_do_app`, em `app/chamados/services/pergunta_no_app.py:76`; consulta compõe em `:73` | API `app/api/views.py:994`, `:2534`; dashboard `app/core/views.py:253`; aviso `app/chamados/services/aviso_pendencia.py:9`; cobrança `app/chamados/services/cobranca.py:48`; projeção/badge `app/chamados/services/pergunta_no_app.py:100`; carga `app/chamados/services/tarefa_do_dia.py:514` |
| Vivas do app | Consulta ORM e C6 em `app/chamados/services/pergunta_no_app.py:52`, `:61` | Consulta em `app/chamados/services/pergunta_no_app.py:73`; ficha `app/colaboradores/services/ficha.py:166`; C17 `app/chamados/juizes.py:1642`; diagnósticos `app/chamados/services/pergunta_no_app.py:208`, `:231` |
| C6 na carga | Corpo único em `app/chamados/juizes.py:1399`; fachada em `:862`; alimentação em `app/chamados/services/carga_pergunta_viva.py:69` | Carga chama em `app/chamados/services/tarefa_do_dia.py:521`, `:525`; `viva_c6` alimenta exposição administrativa em `:260`, `:347`; `_pendentes_app` alimenta publicação em `:568`, `:610` |

A pauta lê indiretamente o fio da tarefa e compara com `FIO_SEM_PORTA`; o E4b lê instâncias e ações para escolher atos. Referências: `app/ponto/services/pautas_dos_discordantes.py:79`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:97`.

O retrato grava outro campo, `dono`, a partir do grupo agregado da Folha; depois prefere esse valor gravado. Templates renderizam o resultado e o PDF usa o rótulo do LED. Referências: `app/folha/retrato_folha.py:1158`, `:1190`, `:438`; `app/templates/folha/_retrato_lista.html:47`; `app/templates/folha/_retrato_o_que_fazer.html:33`; `app/folha/views_retrato.py:413`.

**Achado de segundo escritor dos quatro campos publicados: NÃO PROCEDENTE nos caminhos rastreados.** A montagem fica em `_portas_da_tarefa`; os leitores citados projetam, contam ou agregam sua saída. Referências: `app/chamados/services/tarefa_do_dia.py:635`, `app/folha/fora_da_folha.py:85`, `app/chamados/services/vigia_fio.py:662`.

**2. Uma definição de pendência, mas conjuntos diferentes — PROCEDENTE**

O predicado local é compartilhado: `respondida_em is None` e disputa aberta. Não há segunda cópia desse predicado na carga. Referências: `app/chamados/services/pergunta_no_app.py:81`, `app/chamados/services/tarefa_do_dia.py:514`.

O universo, porém, difere:

- A carga seleciona `colaborador_id`, `conversa=False` e `data_evento` dentro da janela. O app seleciona `disputa__colaborador` e `q_vivas`, sem esses recortes de conversa/data. Referências: `app/chamados/services/tarefa_do_dia.py:395`, `app/chamados/services/pergunta_no_app.py:52`.
- Mesmo dentro da janela, um `agora` histórico pode divergir: o app elimina previamente perguntas com `validada_em` preenchido; a carga admite a pergunta e C6 desconsidera validação posterior ao instante consultado. Referências: `app/chamados/models.py:3468`, `app/chamados/juizes.py:1387`, `app/chamados/services/tarefa_do_dia.py:395`.
- Resposta e fechamento da disputa são lidos **no estado atual** pela partição. C6 projeta resposta futura para inexistente, mas a partição continua vendo seu carimbo preenchido. Isso afeta ambas as composições. Referências: `app/chamados/services/pergunta_no_app.py:81`, `app/chamados/juizes.py:1390`.
- Os dois campos de colaborador devem coincidir pela porta de identidade. Divergência efetiva de colaborador/empresa em dados válidos: **[nao verificado]**. Referências: `app/chamados/models.py:3573`, `app/chamados/services/tarefa_do_dia.py:390`, `app/chamados/services/pergunta_no_app.py:59`.

A igualdade alegada nos comentários, inclusive os números de frota, não constitui prova disponível de equivalência: **[nao verificado]**. Referência da alegação: `app/chamados/services/tarefa_do_dia.py:44`.

**3. Juízes, escritores e relógios**

- **C6 duplicado na carga: NÃO PROCEDENTE.** A carga chama o corpo registrado e reaproveita o resultado quando a pergunta já foi julgada como candidata. Referências: `app/chamados/services/tarefa_do_dia.py:518`, `:521`, `:523`; `app/chamados/juizes.py:888`.
- **C17 paralelo nos leitores migrados: NÃO PROCEDENTE.** O chamado remanescente encontrado é o lado anterior do instrumento de comparação. Referências: `app/chamados/management/commands/diff_e3_sombra.py:127`, `app/core/juizes.py:136`.
- **Tabela paralela de verbo no novo juiz: NÃO PROCEDENTE.** Ele reutiliza `fio_pelos_verbos`, excluindo `cobrar` antes. Referências: `app/chamados/services/tarefa_do_dia.py:618`, `:620`; `app/chamados/juizes.py:1650`.
- **C9 substituído por `sla.vencido` na escolha do fio: NÃO PROCEDENTE.** A escolha não consulta prazo. Entretanto, o painel continua exibindo outro SLA, calculado por referência/urgência; esse valor não comprova vencimento C9. Referências: `app/chamados/services/tarefa_do_dia.py:610`, `app/chamados/catalogo/motor.py:54`, `app/chamados/services/painel.py:378`, `app/chamados/services/sla.py:26`.
- **Relógio novo dentro da tarefa: NÃO PROCEDENTE.** O instante segue à carga, ao C6 e à alimentação; a casca rejeita instante nulo ou carga incompatível. A API atual não fornece esse instante, e C6 conserva fallback de calendário. Referências: `app/chamados/services/tarefa_do_dia.py:519`, `:565`, `:657`; `app/api/views.py:2534`; `app/chamados/juizes.py:592`.

Há derivações de “dono” com **outra unidade**: a ficha traduz verbo do chamado; o relatório classifica disputa por `aguarda_o_colaborador`. Uma resposta vazia carimbada pode estar no histórico do app e ainda sustentar `AGUARDA_COLAB` no relatório. Isso é diferença semântica, não prova de outro juiz do colab-dia. Referências: `app/colaboradores/services/ficha.py:159`, `app/chamados/relatorios_disputa.py:26`, `app/chamados/services/fio.py:187`, `app/chamados/services/pergunta_no_app.py:81`.

**4. Residual `cobrar` e preservação do admin**

**Residual sozinho dar dono: NÃO PROCEDENTE.** O filtro exclui `cobrar`; sem outro dono, não publica rota. A porta `validar` do chamado ligado à ação do colab continua residual quando falta pendência no app, preservando o admin. Referências: `app/chamados/services/tarefa_do_dia.py:613`, `:618`, `:629`; testes em `app/chamados/tests/test_e3_degrau_dono_e_porta.py:58`, `:105`, `:113`.

**E4b esconder classe e como resolvida: NÃO PROCEDENTE por leitura.** O teste exige `segue_sem_porta`, reabertura, nota única, idempotência e manutenção de um dia sem porta no universo. Execução ORM: **[nao verificado]**. Referências: `app/chamados/tests/test_e4_passivo.py:76`, `:81`, `:86`, `:89`, `:117`, `:125`.

A cobertura denominada “B” exige cuidado: o agrupador produz grupo B a partir de fio colab; já `RedBRespostaTextual` usa perguntas respondidas e hipótese de resposta E1, esperando `nada`. Esse teste não certifica a classe B pendente da lei fornecida. Referências: `app/folha/fora_da_folha.py:97`, `app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:108`, `:120`, `:128`.

**Há caminho que perde o colab apesar da pendência: PROCEDENTE**, detalhado no item 8. Referências: `app/chamados/services/tarefa_do_dia.py:277`, `:610`.

**5. Mordida dos testes e mutantes** [nao verificado]

**PROCEDENTE — `--todos` não conclui.** `sem_memo_por_colab` e `carga_sem_relogio` ainda apontam para implementação/teste removidos na passagem para lote. A validação textual levanta antes do controle verde. Referências: `app/chamados/tests/e3_degrau_mutantes.py:41`, `:43`, `:87`, `:92`, `:112`.

**PROCEDENTE — selo AST insuficiente.** Proíbe imports/nomes, mas a anti-vacuidade exige apenas presença dos imports permitidos. Não exige chamadas nem consumo da resposta; preservar esses imports permite passar sem ler a tarefa. O degrau apenas executa esse selo. Referências: `app/folha/tests/test_l129_fora_da_folha_so_le_a_tarefa.py:24`, `:57`, `:61`; `app/chamados/tests/test_e3_degrau_dono_e_porta.py:293`.

| Teste alterado | Veredito sobre a mordida | [nao verificado]
|---|---|
| `n1` | **NÃO PROCEDENTE** a vacuidade “ninguém foi chamado”: junto ao mapa vazio exige uma chamada à partição. Não prova equivalência dos conjuntos. `app/chamados/tests/test_n1_alimentacao_unica.py:114`, `:115`, `:164` |
| `portas_tarefa` | **NÃO PROCEDENTE** ficar vazio: exige C6 exatamente para `[p.pk]`, alimentação única e repetição sem consultas. `app/chamados/tests/test_e3_portas_tarefa.py:139`, `:144`, `:146`, `:152` |
| `pos_suite` | **NÃO PROCEDENTE** perder toda mordida: agora protege a grade respondida, fora dos pendentes. O contrato ficou mais estreito. `app/chamados/tests/test_e3_pos_suite_sem_banco.py:50`, `:65`, `:67` |
| `e4_passivo` | **NÃO PROCEDENTE** mascarar ausência de efeito: exige que a classe e continue contada sem porta. `app/chamados/tests/test_e4_passivo.py:81`, `:122`, `:125` |
| `o261e_f5_alcance` | **PROCEDENTE, limitado:** prova influência da partição, mas deixou de provar ausência de consulta paralela pelo vigia; um leitor paralelo da mesma partição também poderia acompanhar o mock. `app/chamados/tests/test_o261e_f5_alcance.py:273`, `:274`, `:278`, `:280` |

Os testes C1/C2 fornecem um campo sintético `prazo_c9_estourou` que o juiz não lê; demonstram independência desse campo, não integração com C9 real. Referências: `app/chamados/tests/test_e3_degrau_dono_e_porta.py:44`, `:48`, `:300`.

Na execução sem banco, degrau+pós-suite+portas-v6 deram **50 testes OK**; o runner completo abortou no alvo obsoleto. Isoladamente, 19 mutantes tiveram controle verde e morreram; dois falharam por alvo inexistente. Fontes: `app/chamados/tests/test_e3_degrau_dono_e_porta.py:40`, `app/chamados/tests/test_e3_pos_suite_sem_banco.py:19`, `app/chamados/tests/test_e3_portas_v6_sem_banco.py:15`, `app/chamados/tests/e3_degrau_mutantes.py:84`. Registro persistente desses resultados: **[nao verificado]**.

**6. Semântica**

`rota` é classificação da composição de portas do dia; não identifica necessariamente a porta particular do dono vencedor. O fio é escolhido antes da rota. `porta_app` também não implica fila vazia: `cobrar` pode permanecer em `fila/verbos`. Referências: `app/chamados/services/tarefa_do_dia.py:608`, `:622`, `:632`, `:634`.

`nada` e `nao_sei` são distintos na tarefa, mas ambos ficam sem rota. A Folha os projeta para `FIO_SEM_PORTA='nada'`, preservando separadamente a flag de desconhecimento. Há ainda `sistema` sem rota. Referências: `app/chamados/services/tarefa_do_dia.py:623`, `:629`, `:634`; `app/chamados/juizes.py:1652`; `app/folha/fora_da_folha.py:85`, `:248`.

Os leitores diretos aceitam a presença da projeção: Folha lê fio, beco testa presença, furo reconhece as três rotas e vigia conta pela chave publicada. Referências: `app/folha/fora_da_folha.py:85`, `app/escala/services/cadastro_realidade.py:469`, `app/chamados/services/furo_sem_canal.py:114`, `app/chamados/services/vigia_fio.py:662`.

**PERGUNTA:** “sem porta explícito” também substitui o agrupamento do retrato/PDF? O código mantém `ninguem → Admin/Hoje`; “SEM PORTA” aparece no drawer, enquanto o PDF imprime o LED. Referências: `app/folha/retrato_folha.py:192`, `:477`, `:1069`; `app/folha/views_retrato.py:413`.

**7. Comentários/docstrings incompatíveis — PROCEDENTE**

- “Uma consulta do app por colab, memo da carga” descreve implementação abandonada; o corpo faz partição em lote. Referências: `app/chamados/services/tarefa_do_dia.py:362`, `:514`.
- “TODO chamado vivo na fila” publica porta e “CHAMADO VIVO NA FILA TEM PORTA” não ressalvam o residual `cobrar` agora excluído. Referências: `app/chamados/services/tarefa_do_dia.py:28`, `:580`, `:618`, `:629`.
- A promessa de conservar portas de chamados vivos também necessita dessa ressalva. Referências: `app/chamados/services/tarefa_do_dia.py:641`, `:618`.
- Docstrings dos testes `n1` ainda descrevem consulta/memo por colaborador, contrariando seus próprios comentários e asserções de lote. Referências: `app/chamados/tests/test_n1_alimentacao_unica.py:106`, `:112`, `:143`, `:163`.

**8. Contraexemplo à lei — PROCEDENTE**

Caso **sintético**, executado em memória com corpos reais: pergunta 303 de `orfao_14h`, sem resposta, validação ou materialização; disputa aberta; célula `furo`; marco `fim` aceso e vencido; sem tranca e fila vazia. O SQL admite a pergunta e C6 retorna viva porque a célula acusa. Referências: `app/chamados/models.py:3467`, `app/chamados/juizes.py:1444`, `:1450`; mecanismo de execução em `app/chamados/tests/e4_apoio.py:8`.

Resultado: pendentes `[303]`, mas tarefa com **`fio='nada', rota=None`**. A tarefa pula a lâmpada acesa antes de associar a pergunta; depois exige simultaneamente pendência e ação `RESPONDER` na instância. Referências: `app/chamados/services/pergunta_no_app.py:73`, `:81`; `app/chamados/services/tarefa_do_dia.py:277`, `:321`, `:610`, `:622`.

A API serve as pendentes e sua porta de resposta consulta C6, sem acrescentar guarda de lâmpada acesa. Portanto não basta alegar que essa pergunta estaria inacessível por estar acesa. Referências: `app/api/views.py:2535`, `:2572`, `:2608`, `:2611`.

Também existe contraexemplo histórico na direção inversa: validação posterior a `agora` exclui a pergunta do app pelo pré-filtro atual, mas C6 da carga pode considerá-la pendente e permitir fio colab. Referências: `app/chamados/models.py:3468`, `app/chamados/juizes.py:1387`, `app/chamados/services/tarefa_do_dia.py:523`.

Incidência desses casos em produção: **[nao verificado]**.

**9. Pouso** [nao verificado]

**Dependência nova de migration ou cron: NÃO PROCEDENTE nos caminhos alterados examinados.** Os novos valores são montados em memória; a saída do comando acrescenta o contador app ao fluxo existente. Referências: `app/chamados/services/tarefa_do_dia.py:391`, `:635`, `app/chamados/management/commands/lavrar_furo_sem_canal.py:37`.

Estado real de migrations, implantação conjunta, outras raias e cumprimento das condições de produção/exportação: **[nao verificado]**. A reprodução `--base` também permanece **[nao verificado]**, pois depende de `git show` da base. Referência: `app/chamados/tests/e3_degrau_mutantes.py:107`.

**PERGUNTA:** conversas pendentes do app pertencem a algum colab-dia para esta lei? O app as admite; a projeção declara que não têm dia de fato, e a carga as exclui. Referências: `app/chamados/services/pergunta_no_app.py:52`, `:114`, `app/chamados/services/tarefa_do_dia.py:396`.

_5257 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
