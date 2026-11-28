# CODEX-ESTADO

> Ultima resposta processada: **28/11/2026 13:37** · publicado em **28/11/2026 13:41** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 28/11 13:37 · sessao bfebdada828f · O30-F3-CONSTRUIR · leitura · origem=Code

**Candidato:** `ac4ad27ce` (c19b), na cópia `/home/ronald/wt-codex-o30-f3-construir-225036`. A cura de C está no código. `wts.txt:5–8`, `ac4ad27ce:app/relatorios/tests/test_atestados_lote_tela.py:213–235`  
**Veredito:** a obra é **mão única**. Aprovação, RC e horário da suíte inteira, da certificação da Pista e do verificador final **[nao verificado]**. `CLAUDE.md:256–264`, `ls.txt:1–9`   [nao verificado]
**Próximo:** preparar a árvore de pouso sobre o main, conferir os dois caminhos em comum e certificar essa árvore antes do Opus final. `CLAUDE.md:347–354,423–433` [nao verificado]

### 1. Candidato e provas

`ac4ad27ce`, de **26/11 às 23:49:25 −03**, é o HEAD que inclui a leitura dos seis campos do atestado no CSV e no PDF. O pai `c1027999c` ajusta o teste do PDF para aceitar a quebra de linha em `Dias do atestado: 0`. As outras três cópias listadas param na c17, no merge de pouso da c17 ou no merge intermediário da c18. `wts.txt:1–16`, `ac4ad27ce:app/ponto/services/atestados_lote.py:102–136,201–207`, `ac4ad27ce:app/relatorios/tests/test_atestados_lote_tela.py:213–235`

A listagem mostra `suite.rc`, `suite.log`, `espera.txt` e `certifica.txt` sob `suite_c19/`, e `diff.patch` sob `final/`. O vínculo desses artefatos com o SHA, os resultados e os horários estão **[nao verificado]**. O `rc=0` às **09:04 de 27/11** é da conta de smoke na sombra. `ls.txt:1–9`, `relato_o30.txt:14198,14203` [nao verificado]

### 2. Caminhos alterados dos dois lados

Comparei os diffs desde a base comum `59600163c` até `ac4ad27ce` e até o main arquivado `d97d10939f4ba861be231bc031dd856c22d8c803`. A interseção tem **dois caminhos**. `wts.txt:5–8,13–16`, referências abaixo.

| Caminho | Mudanças a preservar no merge |
|---|---|
| `app/core/tests/test_medida_turno_do_par.py` | A F3 atualiza as linhas esperadas de `regularizar_retorno` e `extrato_parcial`. O main atualiza outras entradas do mesmo teste, incluindo exportação e fechamento. `ac4ad27ce:app/core/tests/test_medida_turno_do_par.py:341–346`, `d97d10939:app/core/tests/test_medida_turno_do_par.py:289,329–334` |
| `app/relatorios/views.py` | A F3 altera `atestados_lote`. O main acrescenta o motivo `codigo_dominio_invalido` em `ranking_furos`. `ac4ad27ce:app/relatorios/views.py:373–401`, `d97d10939:app/relatorios/views.py:1722–1731` |

### 3. Classificação pelo princípio 7b

**Mão única para a obra.** O diff toca `ponto/`, inclui a migration `0079` e altera a exportação do lote. O aval classifica **o achado D** como mão dupla, com dinheiro zero, e o manda para a fila, sem varredor novo. A linha de D já consta no BACKLOG fornecido. `CLAUDE.md:256–264`, `ac4ad27ce:app/ponto/migrations/0079_ausencia_atestado_dados.py:4–24`, `ac4ad27ce:app/relatorios/views.py:389–401`, `respostas_2811_1335.txt:1 (5)`, `backlog.txt:2`

### 4. Passos mínimos para pousar

1. **Registrar a retomada.** Marcar a resposta como `RESPONDIDA` e escrever no topo do RELATO `ERRO-BOBO O30-F3-CONSTRUIR`, com motivo `D mão dupla, à fila`. Se a marca de pausa ainda existir **[nao verificado]**, soltá-la pela porta `bin/pausar.sh --soltar`, com trilha. `respostas_2811_1335.txt:1 (5)`, `relato_o30.txt:14627`, `CLAUDE.md:284–287,295–306`, `bin/pausar.sh:39–52`
2. **Montar a árvore em cópia nova do main e integrar `ac4ad27ce`.** Conferir os dois caminhos acima. Tratar `bin/sombra_regras_pessoais.py` em pouso próprio pela L-105 e conferir a máscara dos campos da `0079` no ensaio. Na conferência do escopo, o lote ainda usa tipos fixos e consulta por empresa, sem receber `q`, colaborador ou tipo como filtros. Dispensa desses filtros **[nao verificado]**. `CLAUDE.md:37–40,347–355`, `ac4ad27ce:bin/sombra_regras_pessoais.py:55–56,80,112`, `ac4ad27ce:app/relatorios/views.py:352–387`, `backlog.txt:1`
3. **Rodar a suíte inteira na árvore que vai pousar.** Na Pista 2, colher o log e o recibo de espera e obter `bin/pista_certifica.sh <sha> <log> <espera>` com **rc 0**. A prova exige camadas iguais às da VM, SHA exato, todas as labels, contagem reconciliada, rc 0 do run e `oom=0`. Se a Pista devolver rc 1 na certificação, a suíte de pouso roda na VM. Conferir revisão Codex, censo e mutantes e ter o **Opus final depois da inteira verde**. `CLAUDE.md:129,261–264,417–433`, `bin/pista_certifica.sh:4–17`
4. **Pousar a árvore certificada em um ato.** Conferir a sombra verde do dia. Fazer `merge --no-ff --no-commit`, comparar arquivo a arquivo com `cmp` contra a cópia certificada, registrar a comparação e `OBRA: O30-F3-CONSTRUIR` no commit, com a linha `LEI-AKITA`, e chamar imediatamente `bin/deploy.sh` **com a migration `0079`**. Registrar o resultado e o smoke no RELATO. `CLAUDE.md:62–64,90–92,284–287,347–355,477–491`, `relato_o30.txt:14055`

## 28/11 13:19 · sessao bfd44151da34 · FERIADO-SEM-LIMITE-DO-SUPRIMIDO · censo · origem=Code

**Feito.** Censo somente leitura da cópia `db9adafb21a643521da06ef6f2b2060a265b8fbe`. O diretório veio sem `.git`, portanto o hash por Git fica [nao verificado].  
**Veredito.** Refazer a diferença na cópia nova. O juiz do feriado já está no lugar, mas os quatro ramos regulares e a promoção ainda pagam o trabalhado inteiro. `app/ponto/motor_calculo_v2.py:2925,3208,3385,3467` · `app/ponto/services/fechamento.py:995–1008`  
**Números da regra.** Emp2, comp. 11. Col243 em 15/11 deve ter **420,0 min** de HE100 feriado. Col1046 em 20/11 deve ter **419,7 min**. `app/docs/RELATO.md:425` · `FERIADO-C1-LEIS-2511.txt:1`  
**Próximo.** Portar a conta diária para motor, promoção e vigia no mesmo ato. K1 tem RED marcado e aval condicional para a origem. DIFF e zero pós-apply [nao verificado]. `app/docs/BACKLOG.md:2014,2201` · `app/ponto/tests/test_feriado_um_juiz_porta.py:380–394`

## 1. Mapa curto

| Etapa | O que a cópia já faz e onde está a diferença |
|---|---|
| **Motor** | `_feriado_dobra_no_dia` lê o calendário e a política do ciclo pelo dia do turno. Os quatro ramos gravam o trabalhado inteiro em HE100, HE100 feriado e HE total. O motor separa `periodos` de `periodos_ft` depois de aplicar a janela de HE. `app/ponto/motor_calculo_v2.py:941–978,2558–2574,2581–2607,2925–2929,3213–3220,3385–3389,3471–3478` |
| **Fechamento e promoção** | O fechamento compõe a apuração por fatia com o mapa do dia e o contexto do motor/vínculo. Na via regular, lê os totais de `ResultadoMes`. Na FT, `particionar_ft` decide dia, rubrica e split noturno. A partição ainda entrega `periodo`, noturno de relógio e noturno legal. O mensal usa `f.periodo.minutos_trabalhados` para o valor promovido. `app/ponto/services/fechamento.py:344–383,988–1012` · `app/ponto/calculador/regras.py:371–448` |
| **DiaPago** | A lavra do motor recebe o mesmo resultado e a mesma partição do mensal. Soma os campos de HE dos pares regulares e usa o trabalhado inteiro da FT promovida. A intra entra em rubrica própria. A versão `oraculo` passa pela alimentação do v3 e pela mesma porta de lavra. `app/ponto/services/fechamento.py:1228–1254,1287–1317` · `app/ponto/services/dia_pago.py:176–189,217–219,238–251,331–339` |
| **Porta** | `medir` visita quem tem `status == 'entra'` no classificador do TXT. `minuto_em_duas_rubricas` lê `resultado.periodos` do espelho vivo pela data civil de entrada. Aplica a fórmula comum `max(0, T − previsto − min(S, declarado))` e acusa acima de **teto + 1 min**. O contador entra em `falhas_proprias`. A comparação mensal × soma do DiaPago é outra testemunha. `app/folha/porta_export.py:457–482,538–571,693–703` |
| **TXT** | A emissão passa pela porta e usa o `FechamentoMensal`. Desde a comp. 11, lê a classificação mensal por fatia. Separa HE100 comum como `HE100 total − HE100 feriado`, emite a parcela feriado e a intra em eventos próprios, e converte as horas em centésimos. HE100 noturna é subconjunto. O adicional noturno tem evento próprio. `app/folha/views.py:123–140` · `app/folha/export.py:34–45,383–394,434–462,747–760` |

## 2. Censo fechado dos quatro campos

`E` = `minutos_extra`, `H` = `minutos_extra_100`, `F` = `minutos_extra_100_feriado`, `N` = `minutos_extra_100_noturna`. O censo cobre os acessos de código aos quatro campos no caminho da dobra, incluindo resets, projeções e instrumentos que leem esses campos. `app/ponto/motor_calculo_v2.py:216–225` · `app/ponto/calculador/periodo.py:178–183` · `app/core/medida_leitor_do_dia.py:50–53`

| Escritor | Campos e efeito na dobra |
|---|---|
| **Motor por par, corpo `Motor12x36`** | Escreve `E/H/F = duracao`. `Motor12x36ComEscala` e plantão reutilizam esse corpo. O split chama `_aplicar_he_noturna`. `app/ponto/motor_calculo_v2.py:2925–2929,2971,3651–3665` |
| **Motor por mês, turno partido** | Zera `E/H/F/N` dos regulares e regrava `E/H/F = trabalhado de cada par`. Chama o split por par. O partido noturno herda o ramo. A via FT recebe mapa de dia, mas fica fora do laço que escreve o suprimido da pausa entre pares. `app/ponto/motor_calculo_v2.py:3045–3055,3067–3122,3197–3229,3677–3684` |
| **Motor comercial por par e por mês** | O par escreve `E/H/F = duracao` e chama o split. O mês zera `E/H/F/N` dos regulares, regrava `E/H/F = trabalhado de cada par` e chama o split novamente. `app/ponto/motor_calculo_v2.py:3385–3389,3411,3443–3455,3467–3478` |
| **Escritas transversais** | `PeriodoCalculo` inicializa os quatro em zero. `_aplicar_he_noturna` lê `H` e escreve `N` quando o split é positivo. O bloqueio L-097 pode zerar os quatro e o suprimido antes da partição. `app/ponto/motor_calculo_v2.py:216–225,1051–1066,1563–1594` |
| **Releitura v3** | `reler` zera `E/H/F` inclusive na FT, soma `F` regular quando há CCT e recoloca a dobra em `E/H/F` no último par regular. O reset de repetidos também cobre `E/H/F`. `N` fica fora dessas escritas. O ramo de dobra é condicionado a `_dobra > 0`, detalhe a observar no caso de dobra classificada com saldo zero. `app/ponto/calculador/periodo.py:178–183,731–739,755–775,819–843` |

| Leitor | Campos ou projeção |
|---|---|
| **Propriedades e contas do motor** | `PeriodoCalculo.horas_extra` lê `E`. `ResultadoMes` soma `H/F/N` dos regulares e deriva HE100 comum de `H−F`. O DSR usa `HE50+H−F`. O banco comercial deduz `HE50+H` como pago, portanto reduzir `H` sem carregar a parcela retirada faria o saldo subir. `app/ponto/motor_calculo_v2.py:264–265,328–344,2744–2753,3558–3572,3603–3608` |
| **Promoção, mensal e DiaPago** | A partição lê `N` e compara `F` com `T` para decidir se refaz o split da dobra cheia na FT. O mensal lê as propriedades; o DiaPago lê `H/F/N` dos regulares. Os dois projetam `H/F` promovidos a partir de `T` e o `N` a partir da partição. `detalhe['folga_feriado_min']` registra `T` para o trabalhado mostrado ao leitor. `app/ponto/calculador/regras.py:394–397,433–448` · `app/ponto/services/fechamento.py:988–1012` · `app/ponto/services/dia_pago.py:176–189,238–251,902–910` |
| **Alimentação v3** | Lê `H` para as pontas do split, `F` para a dobra regular do dia, e `H/F/N` regulares em dias com FT. Para a FT promovida, projeta `T` e o split recebido. As chamadas avulsas de `do_dia` também têm caminhos de dobra cheia. `app/ponto/calculador/alimentacao.py:101–108,353–387,518–523,571–585` · `app/ponto/calculador/regras.py:361–364,767–814,850–874,915–923,958–962` |
| **Espelho, PDF, calendário e comparação** | O resumo vivo do espelho lê `E` via `horas_extra` e lê `H` dos regulares. Na FT, soma `T` ao total vivo de extras. O PDF usa `H` bruto para presença da coluna e lê o valor de `DiaPago`; o calendário usa `minutos_extra` projetado de `HE50+HE100` lavrados. Cartão e TXT leem as rubricas em horas. `app/ponto/services/espelho.py:81–114,450–455` · `app/relatorios/pdf_espelho.py:832–862` · `app/ponto/services/dia_pago.py:922–940` · `app/colaboradores/services/calendario.py:698` · `app/relatorios/cartao_pela_celula.py:89–101,172–181` · `app/folha/export.py:434–462` |
| **Vigia e instrumentos** | `minuto_em_duas_rubricas` lê `E` dos regulares. `diff_calculador` lê `H/F/N`; `desvio_o68b` lê `H/N`; `esmeril_espelho` lê `E`. `medida_leitor_do_dia` enumera os quatro como vocabulário da inspeção AST. `app/folha/porta_export.py:541–571` · `app/ponto/management/commands/diff_calculador.py:246–248` · `app/ponto/management/commands/desvio_o68b.py:117,128` · `app/ponto/services/esmeril_espelho.py:185–186` · `app/core/medida_leitor_do_dia.py:50–83` |

## 3. Leis herdadas

- **Origem, leitor e prova.** L-001/L-002/L-007 põem a cura na fonte e a leitura na autoridade existente. L-095 exige uma lavra do dinheiro e o mesmo número nos leitores. L-099 exige coerência e idempotência. L-110 manda cobrar a regra na bateria. `app/docs/LEIS.md:59–65,113,122,136` · `app/docs/CORTES.md:383`
- **Dia e classificação.** L-085 atribui a jornada ao dia de início. A cláusula de feriado da L-086 foi revogada. O calendário e o ciclo decidem a dobra. O aval UM-JUIZ leva esse juiz à FT, usa a certeza da escala para a rubrica e mantém o 12×36 simples fora da promoção. O corte NOTURNO-A alinha a janela de HE ao dia do turno. O corte semanal do CADERNO trata a opção de cadastro “trabalha em feriado” nas escalas semanais. `app/docs/LEIS.md:117–118` · `app/docs/CORTES.md:616,646` · `FERIADO-UM-JUIZ-2711-1210.txt:1` · `app/ponto/calculador/regras.py:410–432`
- **Dinheiro da dobra.** O aval inicial retira da HE100 o minuto já indenizado como intra. C1 fixa `S` inteiro, diurnos primeiro, adicional noturno apurado pela régua vigente, banco comercial congelado no dia em dobra e cálculo por dia mesmo se `S` exceder um par. C1 reserva F10h à família INTERVALO, manda contar o padrão no DIFF e pede S1–S5/C1–C7 na bateria com o oráculo intocado. `feriado_sem_limite_do_suprimido_183x.txt:1` · `FERIADO-C1-LEIS-2511.txt:1` · `app/docs/BACKLOG.md:2013–2014` · `app/docs/RELATO.md:425`
- **K1 e L-097.** O `!` K1 escolheu a opção **(b)**, uma apuração de intervalo no motor para regular e `periodos_ft`, no trecho autorizado. O aval condiciona o pouso ao DIFF por rubrica e manda parar com número e lista se aparecer mudança fora de feriado de folga. O HEAD ainda carrega o bloqueio pré-partição da L-097, registrado para obras próprias. `FERIADO-K1-MOTOR-2611.txt:1` · `app/ponto/motor_calculo_v2.py:1563–1594,3067–3122` · `app/docs/LEIS.md:62,115` · `app/docs/BACKLOG.md:2190–2191,2201`
- **Saída.** DIFF de frota da comp. 11 por rubrica no RELATO antes do pouso, vigia zero nas emp2/3/4 e competência exportada sob a guarda da L-092. Se a cura perder o TXT, o aval destina os dois dias à Pauta DP. O anexo relata `emp2 = 2`. O zero pós-apply, a contagem isolada do vigia na emp4 e o DIFF desta volta ficam [nao verificado]. `app/docs/LEIS.md:62,125` · `app/docs/BACKLOG.md:2014` · `feriado_sem_limite_do_suprimido_183x.txt:1` · `estado1310.md:1`

## 4. Contrato de entrada do limite

| Dimensão | Contrato |
|---|---|
| **Fonte e unidade** | `T = Σ minutos_trabalhados` dos pares apurados da dobra. `S = Σ minutos_intrajornada_indenizada` desses pares, já julgados pela origem. Ambos em **minutos de relógio**, com frações. O intervalo gozado já compõe a apuração de `T`. O limite consome a indenização que o motor apurou, inclusive a de K1. `app/ponto/motor_calculo_v2.py:2872–2898,3078–3122,3340–3372` · `FERIADO-C1-LEIS-2511.txt:1` |
| **Universo** | Colaborador × dia da jornada × pares classificados em dobra, com o vínculo e a régua da fatia correta. A via regular e a FT promovida entregam as parcelas desse dia uma vez. A janela da competência vem do corte da empresa. Na porta, o universo de emissão vem de `classificar_export`. `app/docs/LEIS.md:76–77,117` · `app/ponto/janelas.py:36–57` · `app/ponto/services/fechamento.py:344–383` · `app/ponto/calculador/regras.py:410–448` · `app/folha/porta_export.py:475–482` |
| **Exclusões** | Dia comum, feriado simples, 12×36 simples, folga da rubrica própria e intermitente ficam fora da conta da dobra. Par sem trabalho apurado contribui zero. O 12×36 com cláusula cadastrada pode chegar em dobra no ramo regular do motor. A promoção do UM-JUIZ tem a guarda própria do ciclo simples. `app/ponto/motor_calculo_v2.py:1563–1594,3208–3218,3694–3698,3740,3810–3811` · `app/core/regua_cct.py:473–479` · `app/ponto/calculador/regras.py:371–377,410–432` |
| **Resposta** | `B = max(0, T − S)`. Na dobra, `E = H = F = B`, com HE50 zero; a intra continua `S`. Com `N₀` de relógio na dobra cheia, `N = min(N₀, B)` e a parcela diurna é `B − N`. A passagem distribui o corte pelo dia antes de atingir o noturno. O noturno legal continua vindo da apuração própria. Para o excedente comum, a fórmula vigente é `previsto + min(S, declarado)`. `FERIADO-C1-LEIS-2511.txt:1` · `app/ponto/motor_calculo_v2.py:1034–1049,1898–1916,3197–3229` · `app/folha/export.py:434–462` |
| **Escala de saída** | Somar em minutos antes de converter em horas. DiaPago armazena horas em `FloatField`; o mensal grava duas casas e o TXT emite centésimos. `folga_feriado_min` continua representando o trabalhado da FT para a leitura do dia. O vigia usa a classificação e o dia julgados para escolher o teto, e compara a HE observada com esse teto. `app/ponto/models.py:330–332,417–419` · `app/ponto/services/dia_pago.py:238–251,562–575,902–910` · `app/ponto/services/fechamento.py:988–1008` · `app/folha/export.py:34–45` · `app/folha/porta_export.py:541–571` |

## 5. Matriz dos REDs descritos

| Caso e entrada | Resposta da regra | Sítio que responde diferente ou controle a cobrar |
|---|---|---|
| **col243, 15/11, emp2.** `T=436,3`, `S=16,3`. | **420,0 min** de HE100 e HE100 feriado. **16,3 min** na intra. | O dado relata 436,3 na dobra cheia. Os quatro escritores e a promoção usam `T`. Ramo concreto, batidas, previsto, declarado e split do caso [nao verificado]. `app/docs/RELATO.md:425` · `app/docs/BACKLOG.md:2014` · `app/ponto/motor_calculo_v2.py:2925–2929,3213–3220,3385–3389,3471–3478` · `app/ponto/services/fechamento.py:1001–1008` |
| **col1046, 20/11, emp2.** `T=479,7`, `S=60`. | **419,7 min** de HE100 e HE100 feriado. **60 min** na intra. | O dado relata 479,7 na dobra cheia. A projeção promovida também usa `T`. Ramo concreto e parcelas do caso [nao verificado]. `app/docs/RELATO.md:425` · `app/docs/BACKLOG.md:2014` · `app/ponto/services/dia_pago.py:238–246` |
| **K1.** `10:00–14:00` + `14:30–18:20`. `T=470`, pausa julgada 30, mínimo 60. | **440 HE100 + 30 intra**, tanto regular quanto FT promovida. Com pausa julgada de 60 e o mesmo `T`, **470 HE100 + 0 intra**. | Na regular o código apura 30 e grava 470 na dobra. Na FT, o laço de intervalo fica fora e a promoção usa 470. O RED existente cobra `7,33 h / 0,50 h` na promoção. `app/ponto/motor_calculo_v2.py:3067–3122,3197–3220` · `app/ponto/services/fechamento.py:1001–1008` · `app/ponto/tests/test_feriado_um_juiz_porta.py:380–394` · `FERIADO-K1-MOTOR-2611.txt:1` |
| **Noturno e dois pares.** Entrada sintética já apurada com `T=510`, `N₀=420` em um par e 90 diurnos no outro, `S=60`. | **450 HE100 = 30 diurnos + 420 noturnos**. Controle de saturação com `T=450`, `N₀=420`, `S=60`: **390 HE100 noturnos**, com o adicional calculado sobre o noturno legal original. | Hoje os ramos gravam `T`. A promoção mede o split sobre a dobra cheia e o limita por `T`. O split comum se ancora no fim do par, razão para cobrar o corte diurno no conjunto do dia. `FERIADO-C1-LEIS-2511.txt:1` · `app/ponto/motor_calculo_v2.py:409–451,3213–3220,3471–3478` · `app/ponto/calculador/regras.py:433–448` |
| **Por dia, com `S` maior que um par.** Pares de 400 e 120 min, `S=420`. Segundo controle com `S≥520`. | **100 min** na primeira geometria e **0** na segunda. A classificação do dia continua sendo de dobra, inclusive quando o valor chega a zero. | Os escritores atuais gravam 520. Uma subtração truncada por par daria 120 na primeira geometria. O v3 usa positividade do valor para entrar no ramo da dobra. `FERIADO-C1-LEIS-2511.txt:1` · `app/docs/RELATO.md:425` · `app/ponto/motor_calculo_v2.py:3213–3220,3471–3478` · `app/ponto/calculador/periodo.py:819–843` |
| **12×36 simples, fora.** Feriado do ciclo sem cláusula, inclusive em folga. | **Zero HE100 feriado**. O controle de intervalo F10h só entra na contagem do padrão para o DIFF. | Controle negativo. O ciclo simples já fica fora e a promoção tem guarda explícita. F10h tem obra própria. `app/ponto/motor_calculo_v2.py:3740,3810–3811` · `app/ponto/calculador/regras.py:415–432` · `app/ponto/tests/test_feriado_um_juiz_porta.py:298–305` · `app/docs/BACKLOG.md:2013` |
| **Dia sem feriado.** Mesmas batidas, previsto, declarado e `S`, antes e depois. | Delta **zero por rubrica**, inclusive banco e intra. | Controle negativo. O ramo comum usa o teto próprio; há controle de competência sem feriado para o arredondamento do mensal. `app/ponto/motor_calculo_v2.py:1898–1916,3258–3274,3525–3538` · `app/ponto/tests/test_feriado_um_juiz_porta.py:421–433` · `FERIADO-K1-MOTOR-2611.txt:1` |
| **Vigia pela `medir` real.** Colaborador emitível, `T=480`, `S=60`, previsto 420, declarado 60. Rodar com HE 480 e com HE 420. Repetir com promoção e cauda de D+1. | Acusa **480** e aceita **420** na dobra. A cauda integra a jornada de início e a FT promovida entra na mesma pergunta. | Hoje o teto comum é **0** nessa entrada, portanto acusaria ambas na via regular. A leitura de `periodos` exclui a promoção e `.entrada.date()` pode separar a cauda. A outra testemunha da porta já tem a leitura do mapa de jornada. `app/folha/porta_export.py:100–121,475–482,541–571` · `app/docs/LEIS.md:117` |
| **Partes somam o total e idempotência.** Múltiplos pares, `S=0`, `0<S<T` e `S≥T`. Reapurar e relavrar duas vezes com os mesmos fatos. | `E=HE50+H`, `F=H` na dobra, `0≤N≤H` e `diurno+N=H`. Totais e linhas iguais nas duas rodadas. No comercial, o corte em HE100 produz **delta zero** no banco. | Hoje o total fica em `T`. O banco subtrai a HE gravada, ponto onde um corte isolado mudaria o saldo. A lavra substitui as linhas da versão em transação e já há controle de relavra fracionária. Prova da nova conta [nao verificado]. `app/ponto/motor_calculo_v2.py:3213–3220,3471–3478,3558–3572` · `app/ponto/services/dia_pago.py:349–373` · `app/ponto/tests/test_o280_bateria_da_lei.py:2880–2914` · `app/ponto/tests/test_feriado_um_juiz_porta.py:396–419` |
| **Todo leitor lê o mesmo número, incluindo K2.** Dobra regular e promovida, com segundos, comparada por dia, rubrica e competência. | Motor, partição, mensual, DiaPago, leitores e TXT concordam na unidade de cada saída. Conferir também o centésimo na soma da lavra × mensal. | A projeção FT do v3 e o resumo vivo ainda usam `T`. A porta atual tolera até `0,015 h` nessa comparação. O `1,07 × 1,06` está relatado na volta antiga; reprodução nesta cópia [nao verificado]. `app/ponto/calculador/alimentacao.py:353–387` · `app/ponto/services/espelho.py:95–114` · `app/ponto/services/dia_pago.py:562–575` · `app/folha/porta_export.py:245,457–464` · `app/docs/RELATO.md:343` |

## 6. Arquivos da diferença × obras citadas

Lista para a construção, com as menções conferidas nas linhas correspondentes do BACKLOG. Menção a arquivo é evidência de referência, e concorrência ativa de escrita fica [nao verificado].

| Arquivo | Obras que o BACKLOG cita |
|---|---|
| `app/ponto/motor_calculo_v2.py` | O214, FOLGA-TRABALHADA-TRES-NUMEROS, O251, O254, O263, O281, O296, RETIFICACAO-DA-10-8069-PRIMEIRO, INTERVALO-12X36-PAUSA-ENTRE-PARES e V2-I2-FILA. Na fila de feriado, NOTURNO-A, FERIADO-L097-MOTOR, FERIADO-L097-HERANCA-D1 e FERIADO-K1-MOTOR. `app/docs/BACKLOG.md:1308,1420,1623,1626,1688,1708,1723,1808,2002,2013,2190–2191,2201,2335` |
| `app/ponto/calculador/regras.py` | S5b-TROCA-NAO-CRIA-DESCONTO e O263. O FERIADO-L097-HERANCA-D1 cita a partição diretamente. `app/docs/BACKLOG.md:1425,1688,2191` |
| `app/ponto/services/fechamento.py` | V3-RETOMA-FILA, O-DIA-PAGO, FOLGA-TRABALHADA-TRES-NUMEROS, S5b-TROCA-NAO-CRIA-DESCONTO, O181, O184, FURO-APURADO-FALSO-EM-20378-LINHAS, REVERSAO-DA-10-NAO-COBRE-O-CARIMBO-DO-ESPELHO, EFEITO-NONE-UM-LEITOR, ESMERIL-SEM-INSTANTE, RETRATAR-COMP-DO-TURNO, FERIADO-UM-JUIZ e COMPETENCIA-PAGA-UM-JUIZ. O282 e REGUA-DO-PERIODO-COMPLEMENTO-19-10 citam o fechamento pelo caminho `folha/services/fechamento.py`. `app/docs/BACKLOG.md:1112,1267,1420,1425,1576,1579,1709,1766,1775,1782,1966,2144,2162,2189,2331` |
| `app/ponto/services/dia_pago.py` | O-DIA-PAGO S3, FURO-APURADO-FALSE-EM-VEZ-DE-NONE, V3-IMPACTO-COM-DONO, ABONO-LE-O-CATALOGO, RELAVRA-11-DOS-50 e JUSTIFICATIVA-PREVISTO-ATA-ANTIGA. O antecedente da lavra está em O-DIA-PAGO. `app/docs/BACKLOG.md:1260,1267,1794,1950,2170,2226,3108` |
| `app/ponto/calculador/alimentacao.py` | O281. O arquivo entra na diferença como projeção dos valores já julgados da FT. `app/docs/BACKLOG.md:1708` · `app/ponto/calculador/alimentacao.py:353–387` |
| `app/folha/porta_export.py` | O-DIA-PAGO S3, O214, O293, FURO-APURADO-FALSO-EM-20378-LINHAS, CONTADOR-ELEGIVEIS-DA-11, V2-I2-FILA e ATOMICIDADE-PORTA-NO-MAIN. `app/docs/BACKLOG.md:1260,1308,1720,1766,2228,2335,2458` |
| `app/ponto/tests/

[truncado em 20000 de 21538 caracteres]

## 28/11 13:12 · sessao 768f1e997dbb · FERIADO-SEM-LIMITE-DO-SUPRIMIDO · leitura · origem=Code

**Resumo**  
O HEAD ainda paga o trabalhado inteiro nos quatro ramos de dobra do motor e na promoção da folga-feriado. O FERIADO-UM-JUIZ resolveu a classificação, e o K1 segue aberto. `app/ponto/motor_calculo_v2.py:2925,3208,3385,3467` · `app/ponto/services/fechamento.py:995–1012` · `app/docs/BACKLOG.md:2201`  
Pela regra da obra, a HE100 do col243 em 15/11 é **420,0 min** e a do col1046 em 20/11 é **419,7 min**. `app/docs/BACKLOG.md:2014` · `app/docs/RELATO.md:425`  
**Veredito (a)** — refazer a diferença numa cópia nova do HEAD. Entregar motor, promoção e vigia com a mesma conta, depois medir a comp. 11. `app/ponto/calculador/regras.py:386–453` · `app/folha/porta_export.py:541–571` · `ordem_viva_2811_1310.txt:1`  
O selo fornecido registra **emp2 = 2**. Zero pós-apply e DIFF restrito aos dois dias **[nao verificado]**. `ordem_viva_2811_1310.txt:1`

## 1. Cobertura no HEAD

| Sítio | Onde o suprimido entra hoje |
|---|---|
| **Motor, 12x36 e plantão** | A dobra por par grava `duracao` inteira em `minutos_extra`, HE100 e HE100 feriado. O caminho de excedente usa `_limite_com_suprimido` quando há previsto. `Motor12x36ComEscala` e plantão passam pelo mesmo corpo. O ciclo 12x36 é simples por padrão e pode ter cláusula de dobra. O intermitente limpa o calendário do motor. `app/ponto/motor_calculo_v2.py:2925–2972,3651–3665,3694–3698,3740,3810–3811` · `app/core/regua_cct.py:473–479` |
| **Motor, turno partido** | Na via regular, o laço indeniza a pausa curta no par seguinte, mas a dobra grava o trabalhado inteiro de cada par e sai antes do limite diário. O excedente diário usa `_limite_com_suprimido`. A via semanal de 44 h exclui os feriados da soma. Para `periodos_ft` o método preenche o mapa de dia, e a apuração da pausa curta só percorre `resultado.periodos`. `app/ponto/motor_calculo_v2.py:3059–3122,3197–3239,3258–3260,3277–3287` |
| **Motor, comercial** | `calcular_periodo` grava `duracao` inteira na dobra. `calcular_mes` zera as extras regulares do par e grava de novo o trabalhado inteiro na dobra do dia. O limite com suprimido aparece nos dois caminhos de excedente. No mês, o feriado simples segue para esse limite. `app/ponto/motor_calculo_v2.py:3385–3411,3444–3499,3525–3526` |
| **`fechamento.py` e `dia_pago.py`** | Na via regular, o mensal e a lavra leem os campos de HE100 do motor. Na promoção, o mensal soma `f.periodo.minutos_trabalhados` e a lavra soma `p.minutos_trabalhados`. Ambos acrescentam o split noturno da partição. A intra viaja em rubrica própria. `app/ponto/services/fechamento.py:988–1012` · `app/ponto/services/dia_pago.py:176–189,217–219,238–251` · `app/ponto/motor_calculo_v2.py:328–344,362–370` |
| **`calculador/regras.py`** | O excedente usa `previsto + min(S, declarado)`. A dobra avulsa devolve o trabalhado inteiro e a folga classificada avulsamente também atribui o trabalhado inteiro à rubrica escolhida. Na comp. 11, o caminho alimentado recebe a dobra normal do motor e a partição da folga. Nessa projeção, a promoção ainda usa o trabalhado inteiro. `app/ponto/calculador/regras.py:361–364,767–788,843–874` · `app/ponto/calculador/alimentacao.py:353–387,518–523,571–585` · `app/folha/export.py:385` |
| **`calculador/periodo.py`** | `excesso_do_dia` tem o equivalente ao limite comum. `reler` soma a HE100 feriado já gravada pelos pares regulares e recoloca a soma no último par. O suprimido que calcula entra no excedente. A dobra é herdada do motor. `app/ponto/calculador/periodo.py:387–417,755–775,820–843` |
| **`folha/porta_export.py`** | O vigia aplica a fórmula do excedente comum também ao feriado em dobra. O teto específico `max(0, T − S)` está ausente. `app/folha/porta_export.py:541–571` |

O **FERIADO-UM-JUIZ** cobriu a decisão de **qual dia e qual rubrica** na promoção, com contexto por fatia, juiz do ciclo, escala certa e corte do intermitente. Também deixou o RED de K1 tagueado. O valor da promoção continua no trabalhado inteiro, e o bloqueio L-097 do motor continua em outro ponto. O pouso `a497c0cce` consta do RELATO. `app/ponto/calculador/regras.py:371–448` · `app/ponto/services/fechamento.py:350–351,995–1012` · `app/ponto/tests/test_feriado_um_juiz_porta.py:380–394` · `app/ponto/motor_calculo_v2.py:1563–1590` · `app/docs/RELATO.md:15239` · `app/docs/BACKLOG.md:2189–2190,2201`

## 2. Hunks de `obra.diff` contra a cópia

Os números `−N` são os inícios dos hunks no anexo. A classificação abaixo é por confronto textual com os sítios atuais. O arquivo veio sem `.git`, portanto hash, ancestralidade e resultado de `git apply --check` ficam **[nao verificado]**.

| Arquivo | Ainda encaixa | Conflita ou perdeu a função |
|---|---|---|
| `app/ponto/motor_calculo_v2.py` | **Os dez sítios** mantêm o contexto relevante. `−723` e `−1818` inserem os helpers junto a `:750–755` e `:1916–1918`. `−2578` entra antes do DSR em `:2674–2682`. `−2826/−2871` entram em `:2923–2972`. `−3112` entra em `:3208–3230`. `−3286/−3311` entram em `:3382–3414`. `−3358/−3370/−3461` entram em `:3455–3495,3558–3565`. | **K1 continua faltando ao diff**, mesmo que os dez hunks sejam reaproveitados. O laço atual de intra do partido só escreve na via regular. `app/ponto/motor_calculo_v2.py:3067–3122` |
| `app/folha/porta_export.py` | `−316` e `−544` encaixam na extração e chamada do vigia. `−335`, `−458` e `−644` também têm âncoras atuais. `app/folha/porta_export.py:316–319,332–341,457–464,544–572,644–650` | Os três últimos hunks citados são de **K2** — importação, tolerância e comentário sobre a soma. O HEAD ainda usa `_TOL_H` nessa comparação. O comentário novo depende da mudança de agregação, e esses hunks por si só deixam o teto do vigia como está. `app/folha/porta_export.py:245,338,457–464,566–568` · `app/ponto/services/dia_pago.py:562–575` |
| `app/folha/tests/test_porta_do_export.py` | `−131` encaixa no selo que inspeciona a fórmula dentro de `medir`. `app/folha/tests/test_porta_do_export.py:130–142` | A troca de âncora acompanha a extração. A prova dos dois comportamentos passa pela `medir` real. `app/folha/porta_export.py:475–482,541–571,693–701` |
| `app/ponto/services/dia_pago.py` | `−25`, `−141`, `−162`, `−170`, `−179`, `−291` e `−547` têm âncora. São imports/campos, preparação e soma de parcelas, e agregação de K2. `app/ponto/services/dia_pago.py:23–44,145–189,296–305,558–575` | `−132` conflita com `RecusaDeProcedencia` e introduz a assinatura antiga de promoção. `−241` conflita com a lavra atual de `(p, split_noturno, legais)`. Reaproveitar a conta na partição atual preserva os valores noturnos vindos da fatia. `app/ponto/services/dia_pago.py:132–145,217–219,235–251` |
| `app/ponto/services/fechamento.py` | `−69`, só a redação da docstring, ainda encaixa. `app/ponto/services/fechamento.py:82–85` | `−77/−207/−298/−327` colidem com os mapas de contexto vigentes. O mapa paralelo `prorrogacao_por_par` ficou obsoleto nesse desenho, pois a partição já recebe motor e vínculo. `−925/−943/−956` conflitam com o recuo e a nova partição, e o filtro antigo `_ft_certa → feriado_vira_folga` foi superado. `−1214/−1227/−1290/−1297` devem ser refeitos nas duas chamadas atuais, com a partição e a transação que o HEAD já usa. A soma do trabalhado inteiro em `:1002` segue sendo a diferença a corrigir. `app/ponto/services/fechamento.py:91–98,249–257,344–383,988–1012,1228–1254,1284–1317` |
| `app/core/tests/test_medida_turno_do_par.py` | No `−284`, a entrada da porta ainda aponta para `544`, mas o contexto ao redor mudou. `app/core/tests/test_medida_turno_do_par.py:287–293` | `−255/−300/−326/−345` conflitam com coordenadas novas. Os destinos antigos de **todos os cinco hunks** ficaram obsoletos. Exemplos do HEAD são `909/2521` no motor e `1609/257` no fechamento. O brief pede só o mapa de deslocamentos da futura alteração. `app/core/tests/test_medida_turno_do_par.py:253–262,304–315,329–352` · `brief_c4e.md` |
| `app/ponto/tests/test_motor.py` e `test_s6_intermitente_feriado.py` | Encaixam `−534/−544` e `−166`, respectivamente. Os controles ainda esperam **28 h** nos dois feriados e **12 h** no 12x36 com dobra ligada. `app/ponto/tests/test_motor.py:534–549` · `app/ponto/tests/test_s6_intermitente_feriado.py:168–177` | A diferença a cobrar nas mesmas geometrias é **26 h** e **11 h**, respectivamente, com a intra indenizada em rubrica própria. `app/docs/BACKLOG.md:2014` · `obra.diff` |
| `app/ponto/tests/test_o280_bateria_da_lei.py` | `−74/−210/−302/−330/−1592/−1726/−1752/−6048` mantêm as âncoras de import, matriz, casos, F10h, F11c e fim da classe. `app/ponto/tests/test_o280_bateria_da_lei.py:73–78,209–215,300–308,328–335,1593–1630,1720–1762,6045–6051` | A expectativa F11c deve ser conferida pela regra no caminho vigente. A fixture antiga usa outubro, enquanto a partição nova começa em novembro. F10h continua sendo caracterização de uma falha de intervalo com obra própria. `app/ponto/tests/test_o280_bateria_da_lei.py:1740–1753` · `app/ponto/calculador/regras.py:393–409` · `app/folha/export.py:385` · `app/docs/BACKLOG.md:2013` |
| Três arquivos novos em `app/ponto/tests/` | `test_feriado_sem_limite_do_suprimido.py`, `test_feriado_dobra_noturna_g0.py` e `mutantes_feriado_sem_limite_do_suprimido.py` entram como adições, sem arquivo homônimo visível na cópia. `obra.diff` · `app/ponto/tests/test_feriado_um_juiz_porta.py:380–394` | Reaproveitar os cenários. O K1 já tem entrada no HEAD, e os mutantes da cópia velha dependem de nomes, fontes e revisões daquele desenho. Execução desses mutantes no HEAD **[nao verificado]**. `app/ponto/tests/test_feriado_um_juiz_porta.py:380–394` · `commits.txt` · `obra.diff` |

## 3. Valores e REDs

A conta aprovada é **HE100 = HE100 feriado = `max(0, T − S)` no dia da jornada**. Deduz-se todo o `S` indenizado, começando pelos minutos diurnos de todos os pares. Preserva-se a HE100 noturna da dobra cheia até o limite da HE100 restante, e o adicional noturno conserva sua apuração. No comercial, o que sai da dobra continua contado como pago para o banco. O helper de excedente atual soma apenas `min(S, declarado)` ao previsto, portanto chamá-lo com previsto zero ainda produziria uma conta diferente. `app/docs/BACKLOG.md:2014` · `FERIADO-C1-LEIS-2511.txt:1` · `app/ponto/motor_calculo_v2.py:1898–1916,3558–3570`

| RED | Trabalhado no dado | Intra indenizada | HE100 esperada | HE100 inteira relatada |
|---|---:|---:|---:|---:|
| **col243 · 15/11** · pausa curta | 436,3 min | 16,3 min | **420,0 min** | 436,3 min |
| **col1046 · 20/11** · sem pausa | 479,7 min | 60,0 min | **419,7 min** | 479,7 min |

Os valores vêm do relato da obra. O primeiro caso também aparece no diff com 436,33 e 16,33, cuja diferença continua 420,0. O ramo exato, as batidas, o previsto, o declarado e o split noturno de cada um dos dois dias **[nao verificado]**. Se o selo rodou esse mesmo `medir`, a acusação veio de pares em `resultado.periodos`, que é a lista que o vigia percorre. `app/docs/RELATO.md:425` · `obra.diff` · `app/folha/porta_export.py:538–550`

- **Dois REDs do dinheiro.** Cobrar os totais da tabela na HE100 e HE100 feriado, com a intra em sua própria rubrica. Os quatro ramos de dobra do motor hoje escrevem o valor da última coluna. Exercitar também a promoção, que hoje soma o trabalhado inteiro no mensal e no DiaPago. No TXT, conferir as duas rubricas pelos campos do mensal. `app/ponto/motor_calculo_v2.py:2925–2929,3213–3220,3385–3389,3471–3478` · `app/ponto/services/fechamento.py:1001–1008` · `app/ponto/services/dia_pago.py:238–246` · `app/folha/export.py:434–462`
- **RED K1.** `10:00–14:00` + `14:30–18:20` dá **470 trabalhados**, **30 de intra** com mínimo de 60, e **440 HE100** em ambas as vias. O **440 + 30** da via regular descrito no brief é da c4d. No HEAD, a via regular já apura a intra, mas ainda grava a dobra inteira. A promoção tem o segundo defeito, pois o laço da pausa fica fora de `periodos_ft`. O RED existente cobra **7,33 h de HE100 / 0,50 h de intra**. Os controles do brief cobram gap suficiente com intra zero nas duas vias e dia fora de feriado sem mudança de rubrica. `app/ponto/motor_calculo_v2.py:3067–3122,3197–3220` · `app/ponto/tests/test_feriado_um_juiz_porta.py:380–394` · `app/docs/BACKLOG.md:2201` · `brief_c4e.md`
- **RED de dia e noturno.** Um par com 400 trabalhados e `S=420`, mais outro com 120, deve dar **100 min** no dia. Se houver diurnos suficientes em outro par, o desconto preserva o split noturno da dobra cheia. F10h tem dono próprio para a pausa entre pares, e o DIFF deve contar os dias desse padrão. `app/ponto/tests/test_o280_bateria_da_lei.py:1587–1630` · `app/docs/BACKLOG.md:2013` · `FERIADO-C1-LEIS-2511.txt:1` · `obra.diff`

## 4. Próxima volta e arquivos

**(a) Refazer no HEAD.** Os hunks do motor ainda servem de referência. A partição da folga, a identificação da fatia, o split de plantão longo e a lavra já têm contratos no HEAD que a cópia antiga antecede. A menor mudança é fazer esses leitores consumirem a conta comum da dobra no contrato atual. `app/ponto/services/fechamento.py:350–383,995–1012` · `app/ponto/calculador/regras.py:410–448` · `app/ponto/tests/test_feriado_um_juiz_porta.py:592–628`

**ARQUIVOS:**

- **Opus, motor e bateria.** `app/ponto/motor_calculo_v2.py` para os quatro escritores, a conta por dia, o congelamento do banco comercial e a extração K1 onde hoje está o laço. `app/ponto/tests/test_o280_bateria_da_lei.py` para S1–S5 e C1–C7, `app/ponto/tests/test_feriado_um_juiz_porta.py` para o RED K1 existente, e os dois controles que fixam a dobra cheia em `app/ponto/tests/test_motor.py` e `app/ponto/tests/test_s6_intermitente_feriado.py`. Entregar o mapa de deslocamentos da catraca para conferência. `app/ponto/motor_calculo_v2.py:2677–2678,2925,3078–3122,3208,3385,3467,3561–3570` · `app/ponto/tests/test_o280_bateria_da_lei.py:209–215` · `app/ponto/tests/test_feriado_um_juiz_porta.py:380–394` · `app/ponto/tests/test_motor.py:534–549` · `app/ponto/tests/test_s6_intermitente_feriado.py:168–177` · `brief_c4e.md`
- **Codex construtor, app e teste da porta.** `app/ponto/calculador/regras.py` no contrato de `particionar_ft`, para transportar a HE100 apurada junto do split, `app/ponto/services/fechamento.py` e `app/ponto/services/dia_pago.py` para consumir esse valor, e `app/folha/porta_export.py` com `app/folha/tests/test_porta_do_export.py` para o teto e o RED da porta. Para a lavra do oráculo da comp. 11 ler a HE100 **promovida** corrigida, há ainda `app/ponto/calculador/alimentacao.py:353–377`, que hoje projeta o trabalhado cru. Eu trataria esse ponto como adaptação da leitura da partição, com a regra independente do oráculo preservada conforme C1. Se “oráculo intocado” também abranger essa projeção, a paridade ali fica por resolver. `app/ponto/calculador/regras.py:380–448` · `app/ponto/services/fechamento.py:1000–1012,1292–1317` · `app/ponto/services/dia_pago.py:238–251,331–339` · `app/folha/tests/test_porta_do_export.py:130–142` · `FERIADO-C1-LEIS-2511.txt:1`
- **K2 na verificação.** O arredondamento fracionário já tem controles no FERIADO-UM-JUIZ, mas isso não comprova igualdade centesimal mensal × soma da lavra. O HEAD ainda soma horas gravadas e compara com tolerância de `0,015 h`. Cobrar o centésimo no RED e usar o DIFF para decidir se os hunks de parcelas do c4d são necessários. Resultado dessa prova **[nao verificado]**. `app/ponto/tests/test_feriado_um_juiz_porta.py:396–433` · `app/ponto/services/dia_pago.py:562–575` · `app/folha/porta_export.py:245,457–464`

## 5. Vigia e critério de saída

**Sim.** Para cada data civil de entrada em `resultado.periodos`, `medir` soma HE, intra e trabalhado. Quando HE e intra são positivas, consulta previsto e declarado e acusa se **`HE > max(0, T − previsto − min(S, declarado)) + 1 min`**. O resultado entra na contagem de falhas da porta. A fórmula pode acusar até a dobra já corrigida quando o teto comum fica abaixo de `T − S`. `app/folha/porta_export.py:538–571,693–701`

A volta deve entregar, **no mesmo ato**, a HE100 corrigida no motor e na promoção, e o teto **`max(0, T − S)`** para a dobra no vigia. O RED do vigia precisa rejeitar `HE=T` e aceitar `HE=T−S` com a mesma intra, pela `medir` real e com o colaborador no universo do TXT. Para os dias comuns, a referência segue sendo o teto atual. Se a validação incluir promoção ou cauda após meia-noite, o vigia precisa ler a classificação e o dia já julgados: hoje ele enxerga só `periodos` e agrupa por `_p.entrada.date()`. `ordem_viva_2811_1310.txt:1` · `app/folha/porta_export.py:475–482,538–571` · `app/ponto/calculador/regras.py:410–448`

O pronto é o DIFF da comp. 11 por rubrica no RELATO, limitado aos minutos da obra, e `selo_leitores_no_mesmo_numero --mes 11 --ano 2026` **pós-apply**, com emp2 em zero conforme a ordem e o vigia conferido nas emp2/3/4 conforme o BACKLOG. O `9` da emp4 no selo fornecido é o valor relatado da porta com HE pendente do admin. A contagem isolada desse vigia na emp4 **[nao verificado]**. Sem pouso antes do TXT, a saída registrada para os dois dias é **Pauta DP**. `app/docs/BACKLOG.md:2014` · `app/folha/porta_export.py:693–705` · `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:218–220` · `ordem_viva_2811_1310.txt:1`

## 28/11 12:59 · sessao b8bfa6617f2b · PUSH-2811 · verificador · origem=Code

- **L-085 — RESPEITA no diff.** A jornada pertence ao dia de início, inclusive após meia-noite. O pouso `e216b756a` acrescenta a reapresentação de uma batida histórica ao mesmo pareador. `app/docs/LEIS.md:117`, `app/docs/RELATO.md:256`, `app/ponto/turnos.py:1920`
- **L-113 — VIOLA.** Exportada fora da guarda recalcula com trilha, TXT anterior preservado, DIFF prévio para ato de frota e aviso ao DP com os dois valores se mudar. `a1377afb4` muda a partição de feriado, `5792a7bfe` torna a lavra atômica e `8639de6ca` habilita propagar falhas na revisão. A escrita genérica pode mudar valores sem emitir o aviso exigido. `app/docs/LEIS.md:155–156`, `commits.txt:4–8`, `app/ponto/services/fechamento.py:604–612`, `app/ponto/services/fechamento.py:1197–1206`
- **L-120 — RESPEITA no diff.** Faturar exige 20/20 e 15 invariantes medidas na frota aberta, com zero violações estruturais por uma sequência longa de rodadas. O pouso `a8623781e` acrescenta vagas Codex e `8ea3d525c` acrescenta a fotografia de disco ao ESTADO. `app/docs/LEIS.md:141`, `commits.txt:10`, `commits.txt:14`, `bin/gerar_estado.py:283–287`, `bin/gerar_estado.py:320–321`
- **GERAL — BARRA O PUSH.** Citar as três leis é honesto como registro dos toques. A citação satisfaz o teste textual, e a violação da cláusula de aviso da L-113 continua. `bin/tests/test_lei_protege_sitio.sh:175–189`, `app/docs/LEIS.md:156`, `app/ponto/services/fechamento.py:604–612`

- **L-085.** “Hora depois da meia-noite herda o dia em que o plantão começou.” O diff acrescenta `batidas.append(reapresentada)` e entrega a lista a `_turnos_do_colab_puro`. O pareador produz `data_turno` por `_data_do_turno`. `app/docs/LEIS.md:117`, `app/ponto/turnos.py:318`, `app/ponto/turnos.py:1920–1928`, `app/ponto/turnos.py:2005–2008`
- O selo escrito espera que a saída reapresentada às 00:02 pertença à véspera. O efeito de reapresentar uma só marca quando as duas foram retratadas está em aberto no backlog. Impacto em produção [nao verificado]. `app/ponto/tests/test_lavrar_estoque_par_relampago.py:261–270`, `app/docs/BACKLOG.md:2186`
- **L-113.** “Fora dela, exportada recalcula pelo caminho genérico” e “toda exportada que mudar gera aviso ao DP com os dois números”. O filtro genérico exige `_guarda and _exportadas`, e a única linha da guarda é 09/2026. O desvio do C13 entregue para Pauta DP tem aval próprio. `app/docs/LEIS.md:156`, `app/ponto/services/fechamento.py:37–51`, `app/ponto/services/fechamento.py:623–629`, `app/docs/PROMPTS.md:3008`
- O diff passa a usar `_particao_ft = particionar_ft(...)` e grava a nova rubrica via `.update(...)`. No caminho genérico acionado por evento, o `WARNING` de exportada antecede o cálculo e traz competência, empresas e motivo. A escrita pode terminar sem comparar os valores ou avisar o DP. Ocorrência em produção [nao verificado]. `app/ponto/services/fechamento.py:604–612`, `app/ponto/services/fechamento.py:995–1008`, `app/ponto/services/fechamento.py:1197–1206`, `app/ponto/services/fechamento.py:1847–1853`
- `a8eec39b3`, `a9be3ee84` e `0516d86be` tratam, respectivamente, da contagem após falha, do savepoint do oráculo e da distinção entre recusa de procedência e falha técnica. `commits.txt:1–5`, `app/ponto/services/fechamento.py:1274–1287`, `app/ponto/services/fechamento.py:1320–1339`
- **L-120.** A lei pede “N/15 em zero e a contagem de rodadas limpas seguidas”. As adições `_linha_cv(_ler_cv(raiz))` e `_linha_disco(raiz)` publicam telemetria operacional. O placar de faturamento segue registrado como trabalho a construir. Faturamento efetivo [nao verificado]. `app/docs/LEIS.md:141`, `app/docs/BACKLOG.md:1678–1679`, `bin/gerar_estado.py:283–285`, `bin/gerar_estado.py:320–321`
- Os três blobs da cópia batem com o diff fornecido. A cópia veio sem metadados Git. Contagem dos 178 commits, corpos das mensagens e atribuição linha a linha entre os commits de cada pouso [nao verificado]. `diff.txt` (cabeçalhos `index`) [nao verificado]

## 28/11 12:42 · sessao da9ef6c862e5 · CRON-15-SOMBRA-CABE · leitura · origem=Code

**Estado** — Medida inicial e dossiê colhidos. A construção aguarda resposta à pergunta de desenho. `relato.txt:15024,15027`  
**Trava** — Só os comandos em série têm p95 de 5.095 s, ante teto bruto de 2.200 s. Quatro trilhas ainda dão 3.997 s com as dependências e a ordem de escrita. `relato.txt:27,15024`  
**Próximo passo** — Ronald responder à pergunta de desenho. O aval exige manter todas as etapas e o cron de prod, com última prorrogação em 04/12. `app/config/crons.py:344–345`; `relato.txt:27,15027`; `backlog.txt:2248`  
**Lei aberta** — Zero perguntas de lei no dossiê. `relato.txt:15027`

- **Medida por etapa** — Faltam os recortes `.etapas` com início e fim datados por rodada, as duas linhas de espera e os TSVs dos comandos. Durações completas por rodada e hash do HEAD [nao verificado]. As linhas abaixo são da cópia. `logs_sombra.txt:1–3`; `bin/sombra.sh:380–398`; `bin/sombra_trava.sh:27–31`
- **Duas esperas pela trava** — Antes de `refazer` e de `bloco`. Cada espera vai de `t0` ao cálculo de `espera_s` em `sombra_espera.jsonl`. `bin/sombra.sh:438,451`; `bin/sombra_trava.sh:17,27–31`
- **Refazer e preparar dumps** — Refazer inteiro entre as travas ≈357 s no relato. Preparo, da garantia do banco até `sombra: dumps`, em `bin/sombra.sh:238–253`. `relato.txt:15024`
- **Recriar e restaurar** — Drop/criação `bin/sombra.sh:255–256`, restore `public` `bin/sombra.sh:257–258`, restore `juliani` `bin/sombra.sh:259–260`, resumo `bin/sombra.sh:263–264`.
- **Máscaras e selo pessoal** — Máscara 1 `bin/sombra.sh:266–268`, máscara 2 `bin/sombra.sh:272–274` (≈261 s no relato), selo pessoal `bin/sombra.sh:275–277`. `relato.txt:15024`
- **Divergência, completude e carimbo** — Das consultas ao resumo de divergência, seguido do carimbo. `bin/sombra.sh:286–301,302–305`
- **Bloco** — Preparo, lavra e lista `bin/sombra.sh:329–350`, execução `bin/sombra.sh:351–387`, fechamento `bin/sombra.sh:390–407`. P95 da soma dos comandos de 15–28/11 = 5.095 s. P95 ponta a ponta [nao verificado]. `relato.txt:15024`
- **Por comando** — O TSV mede de `t0` à gravação da duração. A reposição da lavra ocorre antes de `t0` e precisa de marcações próprias para ser medida. `bin/sombra.sh:378–385,398`
- **Três mais longos** — `selo_leitores` 1.222 s, `lavrar_he` 670 s, `lavrar_cxr` 634 s no p95. Na rodada reportada, 1.174 s, 670 s e 634 s, respectivamente. `relato.txt:15024,15132`
- **Hipótese de encaixe** — Testar leituras em lote na origem de `espelho_do_colab`, mantendo as três etapas, seus gatilhos e suas escritas. Na rodada reportada, mesmo zerando o custo das três na conta, sobram 2.617 s de comandos antes do refazer. O encaixe exige ganhos também no restante do bloco. Ganho efetivo [nao verificado]. `app/folha/porta_export.py:475–483`; `app/ponto/services/he_pendente_lavrado.py:80–81`; `app/ponto/services/cxr_na_lista.py:62–78`; `app/config/crons.py:1581–1583,1808–1820`; `relato.txt:15024,15132`

## 28/11 12:40 · sessao 88bb13529e50 · RAZAO-DA-SOBRA · censo · origem=Code

**Resumo · Perímetro da cura**, sob `app/` — `escala/{orfa_classe,utils}.py`, `escala/services/{leitor_celula,furos_diarios}.py`, `ponto/{turnos,juiz_batida,status_dia,supra_juiz}.py`, `ponto/services/{cartorio,fechamento,espelho,dia_pago,bordas_realizado,cluster_espurio,vigia_pareador_celula,vigia_de_hora,ponta_pequena}.py`, `ponto/portas/he.py` e `ponto/management/commands/{tripwire_celulas,recusar_ponta_pequena}.py`. `backlog.txt:2109`, `2ce5e49b5:app/escala/tests/test_razao_sobra_fonte.py:165`, `2ce5e49b5:app/ponto/tests/test_razao_sobra_leitores.py:30`, `app/docs/RELATO.md:14627`  
**Perímetro, continuação** — `chamados/{juizes,models,reconciliador,signals}.py`, `chamados/services/{declaracao_texto,materializacao,fabrica_da_celula,relavra_via,disputa_emissao,fio,cobrar_dia,vigia_fio}.py`, `folha/{causas,export}.py`, `relatorios/{cartao_pela_celula,pdf_espelho}.py`, `templates/ponto/espelho.html`, `core/juizes.py` e `docs/CORTES.{md,json}`. `2ce5e49b5:app/ponto/tests/test_razao_sobra_leitores.py:18`, `2ce5e49b5:app/ponto/tests/test_razao_sobra_leitores.py:155`, `2ce5e49b5:app/chamados/tests/test_razao_sobra_e2_fabrica.py:10`, `2ce5e49b5:app/chamados/tests/test_razao_sobra_repasse.py:11`, `a4a9b6bfe:app/core/juizes.py:1333`  
**Obras com alterações no perímetro fora do `main`** — PAR-PELO-TURNO, E1-PREVISAO-24H, ABONO-X-SAIDA-ANTECIPADA, DESLIGADO-PORTA-DO-CHAMADO, E4C-F5-INSTANTE, E4D-CONTRATO-ROTA-B, DIA-DA-BATIDA-JUIZ-UNICO, FERIADO-SEM-LIMITE-DO-SUPRIMIDO e O30-F3. Alterações sem commit [nao verificado]. `worktrees.txt:1`, `worktrees.txt:57`, `worktrees.txt:63–71`, `worktrees.txt:76`, `worktrees.txt:83–90`, `worktrees.txt:105`, `worktrees.txt:126`   [nao verificado]
**Interseção** — `turnos.py:PAR/ABONO/DIA`; `utils.py` e `espelho.py:PAR/E1`; `fechamento.py:PAR/E1/DIA/FERIADO`; `cartao_pela_celula.py` e `pdf_espelho.py:PAR`; `leitor_celula.py` e `cartorio.py:E1`; `dia_pago.py:E1/FERIADO`; `export.py:E1/E4C`; `core/juizes.py:E1/O30`; `CORTES.{md,json}:E1`; `chamados/{models,signals}.py:DESLIGADO/E4C`; `chamados/juizes.py`, `declaracao_texto.py` e `cluster_espurio.py:DESLIGADO`; `materializacao.py:DESLIGADO/E4C`; `disputa_emissao.py:DESLIGADO/E4C/E4D`; `fio.py:E4D`. `worktrees.txt:57`, `worktrees.txt:63–71`, `worktrees.txt:76`, `worktrees.txt:88`, `worktrees.txt:105`, `worktrees.txt:126`  
**Veredito — não.** A PAR ainda cruza a cura, e a ordem vigente põe a RAZAO-DA-SOBRA depois da PAR. `app/docs/RELATO.md:14627`, `app/docs/BACKLOG.md:1926`, `worktrees.txt:88`

- **PAR-PELO-TURNO** — c1v13 `84f8a88d4`, com c1v16 `aebeee441` no `wt-codex-par-pelo-turno-111717`. Nenhuma das duas pousou no `main`. `app/docs/RELATO.md:14630`, `worktrees.txt:88`
- **E1-PREVISAO-24H** — `8d81e64c5` no `wt-codex-e1-previsao-24h-083708`, E1a c2 `339368be7` e base c3 `8cc31f45c`. Esses commits seguem fora do `main`. `worktrees.txt:66–67`, `worktrees.txt:105`
- **ABONO-X-SAIDA-ANTECIPADA** — `6cc0c2a7e` e `f38300c20` alteram `turnos.py` e seguem fora do `main`, inclusive na cópia c1h `90b161ba3`. A c1d pousou por `19810f51c`. `worktrees.txt:6`, `worktrees.txt:11`, `worktrees.txt:17`, `worktrees.txt:57`
- **DESLIGADO-PORTA-DO-CHAMADO** — base `540ab73c0` e c5 `c28de9ee3`, ambas fora do `main`. `worktrees.txt:50`, `worktrees.txt:63`, `app/docs/RELATO.md:15149`
- **E4C-F5-INSTANTE** — c3 `330e159fb`, candidata a pouso `dbc5b8b4e`, cura5 `63680b06e`, cura6c `486a1ffc4` e OS6 `c9fded10a`, todos fora do `main`. `worktrees.txt:68–70`, `worktrees.txt:106–107`
- **E4D-CONTRATO-ROTA-B** — cura4 `f300431af`, fora do `main`. `worktrees.txt:71`
- **DIA-DA-BATIDA-JUIZ-UNICO** — F2 `a4d3675c7` e F4e `fb8194db0`, fora do `main`. `worktrees.txt:64–65`
- **FERIADO-SEM-LIMITE-DO-SUPRIMIDO** — c4d `28eb07f5d`, fora do `main`. **FERIADO-UM-JUIZ** pousou por `a497c0cce`, com sobreposição em folha, espelho e relatórios. `worktrees.txt:76`, `worktrees.txt:111`
- **O30-F3** — `ac4ad27ce` e candidata `e2955107b`, fora do `main`. **CODEX-CONSTROI-MOTOR** tinha alterações nos arquivos de cortes nas cópias c11/c12 `0b3939987`/`eea12c760`; a versão que pousou foi a c2, por `279aac944`. `worktrees.txt:81–84`, `worktrees.txt:126`, `app/docs/RELATO.md:14898`
- **Demais pousos com arquivos em comum** — PONTA-PEQUENA itens 2–3 `b8f12a692` e v5b `71efa33d7`; PAR-RELAMPAGO `e216b756a`; PORTA-CONFERE-TRAVA `eb76e7be7`; CODIGO-DOMINIO-VALIDO `3545ebc07`; RETRATO-PDF item 2 `b82a5ef08`. `backlog.txt:2109`, `backlog.txt:2440`, `app/docs/RELATO.md:108`, `app/docs/RELATO.md:495`, `worktrees.txt:61`, `worktrees.txt:96`

## 28/11 12:35 · sessao e4470d0b72a6 · RAZAO-DA-SOBRA · leitura · origem=Code

**Resumo**  
**Estado** — Obra descongelada. `reds1` commitada em `2ce5e49b5`, com seis módulos só de teste. Na VM, 52 testes, 30 falhas, 1 erro e 5 pulados (`relato.txt:14632`, `relato.txt:14654`).  
**Trava** — O perímetro da cura ainda cruzava PAR c1v13 e E1 na medição das 15:25. Cruzamento e vaga atuais [não verificado] (`relato.txt:14633`, `relato.txt:14645`, `relato.txt:14661`).  
**Próximo andável** — Codex leitura/censo reconfere o cruzamento antes do despacho. Com interseção zero e vaga, segue para Codex construtor (`relato.txt:14633`, `relato.txt:14645`, `relato.txt:14661`).  
**Lei aberta** — Zero no despacho, pelo aval RAZAO-LEIS-1-6-2611 (`relato.txt:14220`).

**Brief do Codex**

- Na sessão com as worktrees, contar e listar os arquivos em comum entre o perímetro do dossiê §8 e o `git diff --name-only <merge-base>..HEAD` de PAR e E1. Registrar o resultado por worktree e a disponibilidade de vaga (`relato.txt:14633`, `relato.txt:14645`, `relato.txt:14661`).
- Na cópia, seguir a sobra do pareador e da grade até o classificador e a ata. Conferir onde o instante vira `HH:MM` e onde dia de trabalho sem marco vazio recebe `INDEFINIDA` (`app/ponto/turnos.py:1192`, `app/escala/utils.py:1748`, `app/escala/utils.py:1883`, `app/escala/orfa_classe.py:53`, `app/escala/orfa_classe.py:68`, `app/ponto/services/cartorio.py:761`).
- No censo AST, localizar leituras cruas de `orfas` e `n_orfas`, incluindo ata antiga, espelho, PDF e efeito no fechamento. O aval de construção prevê o selo catraca e `sobras_sem_razao` (`app/escala/services/leitor_celula.py:180`, `app/ponto/services/espelho.py:703`, `app/relatorios/pdf_espelho.py:477`, `app/folha/causas.py:43`, `app/ponto/services/fechamento.py:1117`, `backlog.txt:2109`).
- **REDs registrados** — R1–R6, A1, A2, V1 e V2 (`relato.txt:14654`). Enunciados individuais e causa do erro [não verificado].

## 28/11 12:34 · sessao 4ff1449fdd7f · SONDA-ESPURIAS-RAMO2 · leitura · origem=Code

- **Estado.** Sonda s2 pronta com `rc 0`. Os quatro lados cobriram 963 colabs nas empresas 2/3/4, comp. 11, com 99 sobras dia a dia. RAMO2 mudou 0 dos 14 colabs do teto. (backlog.txt:2246, relato.txt:14229)
- **Trava.** A pausa da sonda foi retirada às 14:43. A c2u do RAMO2-CRUZA aguarda vaga do opus. (relato.txt:14632, 14447)
- **Próximo passo andável.** Ronald envia o aviso do CASO-LITERAL-14 para contraprova. A etapa do Codex na sonda já foi concluída. (backlog.txt:2111, relato.txt:14229)
- **Lei.** Nenhuma pergunta aberta registrada para a sonda. A regra do RAMO2 está decidida. A âncora preserva o trabalhado e L-097 só classifica a HE. (backlog.txt:2183, 2246)

## 28/11 12:28 · sessao 4b8be07b248d · TESTE-DO-ESTRANHO · leitura · origem=Code

**RESUMO — acertou 0 · parcial 5 · travou 0.** A nota considera todos os pontos explícitos de cada tarefa no gabarito. `gabarito.md:2`, `gabarito.md:6`, `gabarito.md:10`, `gabarito.md:12`, `gabarito.md:19`

- **T1 — parcial.** Acertou a ordem `ORDEM-VIVA-2811-1200`, as duas sequências e a meta. Faltou explicitar que o hook usa `ORDEM-VIVA-TOPO` como autoridade e que o marcador ficou na `1511-NOITE` até 28/11. Destino → glossário. `resposta.md:8`, `app/docs/BACKLOG.md:8`, `app/docs/BACKLOG.md:1926`, `app/docs/BACKLOG.md:1927`, `bin/hook_stop_fila1.py:188`
- **T2 — parcial.** A causa confere. As perguntas 61520/61521 foram emitidas em 23/11 às 12h36, quando a célula 122468 previa 11h/12h, e ficaram pendentes após a regeneração das 12h42 retirar o intervalo. É órfã de regeneração, com cura de classe no E2 `retirar_contexto`. Faltou identificar o 40588 como um dos seis da `CHAMADO-PRESO-CLASSE-PORTAS-2611` e dizer que a porta `admin_encerra` já estava no ar, sem tela que a oferecesse. Destino → diagrama. `resposta.md:18`, `resposta.md:20`, `app/docs/RELATO.md:15208`, `app/docs/BACKLOG.md:836`, `app/docs/BACKLOG.md:1640`
- **T3 — parcial.** Identificou o juiz da dobra, `MotorBase._feriado_dobra_no_dia`. Faltou explicar que a guarda L-097 soma os minutos a `minutos_fora_janela`, zera o trabalhado e colapsa o par. O `FERIADO-UM-JUIZ` pousou em `a497c0cce`. A cura da virada no 6x1, `dc876f5d1`, foi reprovada por perder 240 minutos e ficou fora do pouso. Destino → diagrama. `resposta.md:28`, `resposta.md:30`, `app/core/juizes.py:320`, `app/ponto/motor_calculo_v2.py:941`, `app/ponto/motor_calculo_v2.py:1585`, `app/docs/RELATO.md:14867`, `app/docs/RELATO.md:14872`, `app/docs/RELATO.md:15231`
- **T4 — parcial.** Faltaram o comando exato da suíte, a verificação na sequência geral, o arquivo de reversão em `logs/` e a linha `LEI-AKITA` no commit. Rito do gabarito → cópia `wt` → `bin/suite.sh --dir <copia> --motivo "pouso <OBRA>"` na árvore do pouso → verificador ou Opus final → reversão prévia em `logs/` → `git merge --no-ff --no-commit`, commit com `OBRA:` e `LEI-AKITA`, e `bin/deploy.sh` no mesmo ato. Reversão do merge → `git revert -m 1 <merge>` e `bin/deploy.sh`. `resposta.md:38`, `CLAUDE.md:318`, `CLAUDE.md:105`, `CLAUDE.md:347`, `CLAUDE.md:429`, `CLAUDE.md:456`, `CLAUDE.md:63`, `CLAUDE.md:91`, `bin/commit_msg.sh:29`, `app/docs/RELATO.md:15081`
- **T5 — parcial.** Começou pelo handoff e pulou a comparação com a ordem mais nova e a consulta explícita ao hook. A sequência é marcador × ordem mais nova → saída de `bin/hook_stop_fila1.py` → `PAREI` e `lei` no topo do RELATO → `python3 bin/rc_sem_colher.py` → processos e jobs vivos (`bin/trava_teste.sh --quem`, `bin/codex_vagas_estado.py` e pista) → `HANDOFF-SESSAO.md`. Destino → diagrama. `resposta.md:50`, `gabarito.md:20`, `app/docs/BACKLOG.md:1926`, `bin/hook_stop_fila1.py:188`, `CLAUDE.md:272`, `CLAUDE.md:278`, `CLAUDE.md:338`, `bin/rc_sem_colher.py:13`, `bin/trava_teste.sh:16`, `bin/codex_vagas_estado.py:2`, `app/docs/HANDOFF-SESSAO.md:3`

- TESTE-DO-ESTRANHO T1: explicitar a autoridade de `ORDEM-VIVA-TOPO` e registrar o marcador preso na ordem anterior -> glossário (`bin/hook_stop_fila1.py:188`, `app/docs/BACKLOG.md:1927`)
- TESTE-DO-ESTRANHO T2: ligar o 40588 à fatia dos seis chamados e mostrar a porta admin ainda sem tela -> diagrama (`app/docs/BACKLOG.md:836`, `app/docs/BACKLOG.md:1640`)
- TESTE-DO-ESTRANHO T3: mostrar onde a L-097 zera o par e registrar a reprovação da cura da virada 6x1 -> diagrama (`app/ponto/motor_calculo_v2.py:1585`, `app/docs/RELATO.md:14872`)
- TESTE-DO-ESTRANHO T4: registrar comando da suíte, verificação, arquivo de reversão e linha `LEI-AKITA` no rito de pouso -> inventário (`CLAUDE.md:105`, `CLAUDE.md:429`, `CLAUDE.md:456`, `CLAUDE.md:63`) [nao verificado]
- TESTE-DO-ESTRANHO T5: ordenar o diagnóstico da fila vazia, da comparação do marcador até o handoff -> diagrama (`gabarito.md:20`, `app/docs/BACKLOG.md:1927`, `app/docs/HANDOFF-SESSAO.md:3`)

## 28/11 12:27 · sessao 56360ec16f5a · FERIADO-L097-MOTOR · leitura · origem=Code

## Resumo

- A FERIADO-UM-JUIZ pousou em `a497c0cce` e o apply da 11 rodou. A L-097 segue fora do `main`, com a c2 `dc876f5d1` reprovada pelo ver2. `app/docs/RELATO.md:14867,14872,15264–15270`
- Seguem abertos os 240 min perdidos no feriado D+1 e a divergência de data entre autorização, pendência e oráculo. Faltam o DIFF da 10 e a lista final da J.A com reversão [nao verificado]. `app/docs/RELATO.md:14867,14872,14876`
- O `!` da herança já foi respondido no topo e o v2 manda tratar D+1 em volta própria **depois** do pouso da L-097. As perguntas de lei (0)–(2) têm decisões restritivas contestáveis. `app/docs/RELATO.md:16,29,14877`
- Já dá para transportar a cura do feriado do próprio turno, limitada à guarda, e revalidar pagamento, pendência e segmentos na base pós-FUJ. `app/docs/RELATO.md:40,14867,14877`
- Próximo passo na vaga opus é portar c1+c2, rodar RED e mutantes, inteira e opus final. Medir o DIFF da 10 e a lista da 11 na árvore candidata ao pouso. `app/docs/BACKLOG.md:1926,2190` · `app/docs/RELATO.md:14867,14872`

## BRIEF c3 — opus construtor

- **Base.** `main` HEAD `96e66c5f2`. Cherry-pick em ordem `05ec19a6f` → `dc876f5d1`, ou rebase dos dois desde `1d2eb496c`. A c2 isolada pressupõe a c1. A prévia de merge foi limpa em leitura. `app/docs/RELATO.md:14796–14797,14867,15270`
- **Guarda no HEAD.** Inserir o retorno no ramo de folga com escala certa, após `escala_certa_no_dia`. Exigir feriado, ciclo fora de `CICLOS_FERIADO_SIMPLES` e `_dia_do_turno_do_par(entrada)[0] == dia`. A partição usa o mesmo juiz e escolhe a rubrica. `app/ponto/motor_calculo_v2.py:963–978,1579–1590,3740` · `app/ponto/calculador/regras.py:410–432`
- **PROIBIDO.** Sem `except` novo, sem relógio solto, motor só no hunk da guarda. Fora do delta de produção ficam o laço de herança, data da autorização, publicação da pendência, partição, fechamento, guarda de competência e TXT. `app/docs/PROMPTS.md:3176` · `app/docs/RELATO.md:14867,14877` · `app/docs/PLANO-PADROES.md:97–103`
- **CENSO, escritores no HEAD.** Motor grava dia herdado e mapa por período, zera e colapsa o par, publica alerta e `{dia, hora, minutos}`. A porta de HE grava a decisão que o motor consulta. `app/ponto/motor_calculo_v2.py:2513–2522,2570–2574,1585–1602,2833,1569–1571` · `app/ponto/portas/he.py:44`
- **CENSO, escritores do pago.** Fechamento monta contexto por dia e por período, `particionar_ft` pode remapear dia/vínculo e escolhe rubrica, fechamento salva `FechamentoMensal` e chama as duas lavras de `DiaPago`. `app/ponto/services/fechamento.py:332–351,995–999,1181,1237,1300` · `app/ponto/calculador/regras.py:410–432` · `app/ponto/services/dia_pago.py:372`
- **CENSO, leitores da pendência.** Espelho traduz `sentido='dia'` e agrega dias. Retrato lê o espelho e grava `MetricaSnapshot`, Gestão de HE lê o retrato, portão do TXT lê pendência e decisão, PDF lê pendência e alertas. `app/ponto/services/espelho.py:757–763,939` · `app/ponto/services/he_pendente_lavrado.py:138,183–195` · `app/ponto/services/gestao_he.py:426–434` · `app/folha/porta_export.py:181–201,687–692` · `app/relatorios/pdf_espelho.py:935–945,968–979`
- **CENSO, leitores do par e do pago.** Oráculo cruza dia/hora da pendência com as pontas efetivas e consome a partição. TXT lê o mensal. Calendário lê `DiaPago`. `app/ponto/calculador/alimentacao.py:73–83,114–123,353–371,514–518` · `app/folha/export.py:434,460,734–742` · `app/colaboradores/services/calendario.py:684–689`
- **RED — feriado 6x1 no próprio turno, janela ativa.** Reativar o teste pulado. Certa e incerta totalizam 4 h com e sem autorização. A rubrica segue a partição: FT certa soma 4 h, ou HE100-feriado quando o feriado é a causa da folga. Conferir mensal e lavras. Mutante: retirar o retorno da guarda para a certa. `app/ponto/tests/test_feriado_um_juiz_porta.py:547–563` · `dc876f5d1:app/ponto/tests/test_feriado_um_juiz_porta.py:579–605`
- **RED — feriado em D, dois turnos.** D 18h–22h e D+1 03h–07h, vão de 5 h. Em 6x1→6x1 e 6x1→12x36, exigir 4 h HE100 em D e somente o par das 03h com 240 min pendentes. Mutante: retirar a igualdade do dia do turno e voltar a liberar pelo dia herdado. `dc876f5d1:app/ponto/tests/test_feriado_um_juiz_porta.py:810–841,859–862` · `dc876f5d1:app/ponto/motor_calculo_v2.py:1599`
- **RED — pendência isolada.** Feriado liberado sem `sentido='dia'`. Folga comum com uma pendência de 240 min às 07h no motor e no espelho. Mutante: suprimir só o `append` da pendência e manter o bloqueio. `dc876f5d1:app/ponto/tests/test_feriado_um_juiz_porta.py:903–923` · `app/ponto/motor_calculo_v2.py:1585–1602`
- **RED — segmentos.** Um segmento de 240 min no feriado. Folga comum sem segmento, com um `par_bloqueado_sem_previsto`. Mutante: omitir só o colapso de `efetivo_saida`, mantendo zeragem e pendência. `dc876f5d1:app/ponto/tests/test_feriado_um_juiz_porta.py:925–936` · `app/ponto/motor_calculo_v2.py:1591–1602`
- **Controles da guarda.** Folga comum sem autorização zera e com autorização paga. Feriado sem folga e previsto zero segue bloqueado. 12x36 segue bloqueado mesmo com dobra ligada pela CCT. Mutantes respectivos: dispensar feriado, dispensar folga, trocar o teste de ciclo por `feriado_em_dobra`. Reexecutar os controles de intermitente por base e ciclo. `app/docs/RELATO.md:29` · `dc876f5d1:app/ponto/tests/test_feriado_um_juiz_porta.py:607–651` · `app/ponto/tests/test_feriado_um_juiz_porta.py:358–378`
- **Controles da virada.** Em 6x1→12x36 com feriado D+1 e 12x36→6x1 com feriado D, ambos os pares ficam bloqueados. Um único turno do feriado com pares 21h–00h e 01h–05h paga 7 h. Mutantes: trocar o ciclo pelo vínculo vizinho e trocar o dia do turno pela data civil da entrada. `dc876f5d1:app/ponto/tests/test_feriado_um_juiz_porta.py:864–895` · `app/ponto/motor_calculo_v2.py:963–978`
- **D+1 na volta própria.** Preservar os dois `expectedFailure` de 4 h HE100, sem troca e em 12x36→6x1. O opus final decide se isola a asserção do pago para erro de fixture não contar como prova. Na volta D+1, o mutante é restaurar o bloqueio pelo dia herdado. `app/docs/RELATO.md:16,14877` · `dc876f5d1:app/ponto/tests/test_feriado_um_juiz_porta.py:850–857,880–884`
- **(0)** Feriado sem folga e com previsto zero entra na liberação? Corte atual mantém o bloqueio. `app/docs/RELATO.md:29`
- **(1)** 12x36 em folga real de feriado mantém o bloqueio integral ou só o corte da rubrica 59-A/CCT? Corte atual mantém o bloqueio integral. `app/docs/RELATO.md:29`
- **(2)** FT como única rubrica com trabalhadas zero entra no TXT? Classificar como INCOMPLETO na lista. O HEAD tem ressalva para HE100-feriado emitível. A classificação dos casos da frota está [nao verificado]. `app/docs/RELATO.md:29` · `app/folha/export.py:931–945,977–998`
- **Aceite.** Fixar janela, decisão e `agora` na fixture. Para cada RED, mostrar a asserção esperada na base sem a cura e a queda com seu mutante. Rodar inteira e anexar a triagem ver2 ao opus final. Execução na base c3 [nao verificado]. `app/docs/PROMPTS.md:3224–3225` · `dc876f5d1:app/ponto/tests/test_feriado_um_juiz_porta.py:903–923` · `app/docs/RELATO.md:14867–14872`
- **Rito de pouso.** DIFF da 10 pela L-113, publicado antes do pouso/ato de frota, com aviso ao DP. A triagem chama a 10 de exportada. O HEAD registra paga à mão, sem TXT e sem guarda. Situação atual em prod [nao verificado]. Preservar a 09 guardada. Lista J.A da 11 por colab, dia e minutos, com reversão antes do apply L-092 da L-097. `app/docs/RELATO.md:25,14872,14876` · `app/ponto/services/fechamento.py:37–50,603–629,1847–1853` · `app/docs/BACKLOG.md:2190`
- **Padrão.** `Single Writer Principle` / `juiz único`: a guarda decide a efetividade do par e a partição existente decide a rubrica. `app/docs/GLOSSARIO.md:12` · `app/ponto/motor_calculo_v2.py:1585–1602` · `app/ponto/calculador/regras.py:428–432`

## 28/11 12:25 · sessao ca3cc270be5e · TESTE-DO-ESTRANHO · leitura · origem=Code

## RESUMO

**5 resolvidas · 0 parciais · 0 travadas.** A ordem vigente está em `app/docs/BACKLOG.md:1926` e a causa do chamado 40588 em `app/docs/RELATO.md:15208`.  
**Principal limite:** o handoff é um retrato de 28/11 às 11h58. O estado ao vivo da sessão, das filas e da produção ficou [nao verificado]. `app/docs/HANDOFF-SESSAO.md:3`

## T1. O que está em construção agora, em que ordem e por quê?

**RESPOSTA** — Vale a **ORDEM-VIVA-2811-1200**. A meta é zerar os problemas atribuídos à Hasner na J.A, competência 11, até segunda, com uma sonda no domingo à noite. Codex, pista 2, pista 3 e Opus verificador devem receber trabalho sempre que houver item executável. Pré-requisito pendente herda a vez, e obras que disputam o mesmo arquivo precisam ser sequenciadas. `app/docs/BACKLOG.md:8` · `app/docs/BACKLOG.md:1926` · `CLAUDE.md:318` · `CLAUDE.md:444`

- **Opus construtor:** CUSTO F1 v7 → OBRA-PRONTA-ACORDA → FERIADO-L097-MOTOR c2 → DESLIGADO c4/c5 → CUSTO F2/F3. A F1 v7 já tinha commit em cópia e aguardava a coleta dos resultados de testes e mutantes. `app/docs/BACKLOG.md:1926` · `app/docs/RELATO.md:15212`
- **Codex e pistas:** canário E4C a3/c4 e E4D v4 → C7-TELA-ADMIN-ENCERRA, para os 88 casos → RETRATO-PORTA-PELO-MOTIVO, depois da OS6 → RAMO2-B e RAMO2-HERANCA-VAO → PAR → ORFAS-FOLHA-E-ALARME itens 2 e 3 → E4d → faixa do BOTAO → RAZAO-DA-SOBRA → CRON-15-SOMBRA-CABE → SONDA-ESPURIAS/CASO-LITERAL-14 → E1C. No último registro, a E4C a3 estava em aceite, a E4D v4 tinha entrega para aceite e a C7 tinha código em cópia, com testes, verificação e smoke pela frente. `app/docs/BACKLOG.md:1926` · `app/docs/RELATO.md:15234` · `app/docs/RELATO.md:15237` · `app/docs/RELATO.md:15241`
- **FERIADO-UM-JUIZ** já estava no ar às 11h36, com o apply da competência 11 registrado como concluído e o pacote ao Fernando preparado. O envio ficou com Ronald. `app/docs/RELATO.md:15231` · `app/docs/RELATO.md:15266` · `app/docs/RELATO.md:15270`

**AUTO-AVALIAÇÃO** — resolvi. **ONDE TRAVOU** — a ordem e o andamento estão no BACKLOG e no RELATO. A ocupação das vagas após o retrato ficou [nao verificado]. `app/docs/HANDOFF-SESSAO.md:18` · `app/docs/BACKLOG.md:1926` **TEMPO** — ~1 min e 30 s [nao verificado].

## T2. Chamado 40588, de 20/11

**RESPOSTA** — As perguntas **61520 e 61521** foram emitidas em **23/11 às 12h36**, quando a célula **122468** previa os marcos de **11h e 12h**. Às **12h42**, a célula foi regenerada para **07h–19h sem intervalo**, e as duas perguntas continuaram pendentes. A causa é **órfã de regeneração**. `app/docs/PROMPTS.md:3223` · `app/docs/RELATO.md:15208`

A correção da classe pertence à fatia **CHAMADO-PRESO** da obra **ESCRITOR-UNICO-DE-CHAMADO**, no **E2 `retirar_contexto`**, após o pouso da E4C. **INTERVALO-SEM-PREVISTO**, da RAIA-INTERVALO, encontrou a causa. **C7-TELA-ADMIN-ENCERRA** é a via manual para o admin encerrar esses casos com motivo e trilha. `app/docs/BACKLOG.md:835` · `app/docs/BACKLOG.md:2654` · `app/docs/PROMPTS.md:3229`

**AUTO-AVALIAÇÃO** — resolvi. **ONDE TRAVOU** — busquei `40588` no PROMPTS, RELATO e BACKLOG. O log bruto da sonda ficou [nao verificado]. `app/docs/RELATO.md:15208` **TEMPO** — ~1 min [nao verificado].

## T3. Feriado trabalhado em folga no 6x1

**RESPOSTA** — Na classificação mensal vigente desde a competência **11/2026**, os minutos apurados de feriado trabalhado em folga no **6x1** entram uma vez como **HE 100% de feriado**, ou seja, em dobro. O teste de 6x1 sem escala certa confirma **5h30 na rubrica de feriado e zero em horas comuns**. `app/folha/export.py:385` · `app/ponto/services/fechamento.py:1002` · `app/ponto/tests/test_he100_dia_sem_previsto.py:250`

O juiz da dobra é **`MotorBase._feriado_dobra_no_dia`**, que usa o feriado do **dia de início do plantão** e a regra do ciclo. Só o `12x36` consta da lista padrão de ciclos com feriado simples. **`rubrica_da_folga`** escolhe a rubrica, e **`feriado_vira_folga`** verifica se o feriado causou a folga daquele vínculo. `app/ponto/motor_calculo_v2.py:941` · `app/ponto/motor_calculo_v2.py:961` · `app/ponto/motor_calculo_v2.py:3740` · `app/ponto/calculador/regras.py:371` · `app/escala/models.py:1384`

A janela **L-097** ainda pode zerar o par antes da classificação. No apply registrado, os **13 dias de 10 colaboradores 6x1** do pacote seguiram com os mesmos valores. A cura para pagar o feriado diretamente pelo ciclo pertence à **FERIADO-L097-MOTOR**, que estava fora do HEAD registrado. `app/ponto/motor_calculo_v2.py:1562` · `app/ponto/motor_calculo_v2.py:1585` · `app/docs/PROMPTS.md:3176` · `app/docs/RELATO.md:15270`

**AUTO-AVALIAÇÃO** — resolvi. **ONDE TRAVOU** — conferi motor, calculador, fechamento e testes. O pagamento efetivo ficou [nao verificado]. `app/ponto/calculador/regras.py:428` · `app/docs/RELATO.md:15270` **TEMPO** — ~1 min [nao verificado].

## T4. Como pousar uma obra com reversão

**RESPOSTA**

1. **Prepare em cópia.** Confirme escopo, `!` e B3. Faça RED antes da cura. Para mão única, concorrência, schema ou multi-tenant, passe por censo, mutantes, revisão do Codex e Opus final após a suíte. `CLAUDE.md:28` · `CLAUDE.md:255` · `CLAUDE.md:274` · `CLAUDE.md:423` [nao verificado]
2. **Certifique a árvore que vai pousar.** Exija suíte inteira verde. Se rodar na pista, exija `bin/pista_certifica.sh` com rc 0 ou rode na VM. Confira os arquivos com `cmp` contra a cópia certificada e registre a prova no commit. `CLAUDE.md:104` · `CLAUDE.md:129` · `CLAUDE.md:347`
3. **Prepare a reversão antes do ato.** Registre no RELATO `git revert -m 1 <commit-do-merge>` seguido de `bin/deploy.sh`. Para apply de folha, grave antes o snapshot dos campos por colaborador e prove depois a integridade das competências exportadas por hash, com prova de frota no mesmo marco. Para alterar gravado de competência guardada, exija `!`, DIFF prévio e a porta staff. `app/docs/PROMPTS.md:3202` · `app/docs/RELATO.md:351` · `CLAUDE.md:455`
4. **Pouse e publique no mesmo ato.** Faça `git merge --no-ff --no-commit` → resolva → commit com `OBRA: <id>` → `bin/deploy.sh`. Produto pousa primeiro e instrumento tem pouso próprio. O deploy confere os portões, migra quando aplicável, recarrega as três cascas e prova as rotas. `CLAUDE.md:284` · `CLAUDE.md:352` · `app/docs/INFRA.md:38` · `bin/deploy.sh:416`
5. **Faça o smoke e registre o resultado.** Se reprovar, execute o revert previsto, republique e repita as provas. Se houve escrita de dados, siga também a reversão específica pela porta, restaurando só os campos-alvo do snapshot. `CLAUDE.md:477` · `app/docs/RELATO.md:4575` · `app/docs/RELATO-ARQUIVO.md:32867`

**AUTO-AVALIAÇÃO** — resolvi. **ONDE TRAVOU** — conferi o procedimento no CLAUDE, no deploy e em exemplos de reversão no RELATO. A execução ficou [nao verificado]. `CLAUDE.md:347` · `bin/deploy.sh:308` · `app/docs/RELATO.md:4575` **TEMPO** — ~2 min [nao verificado].

## T5. O Code parou e a esteira está vazia

**RESPOSTA** — Eu conferiria nesta ordem:

1. **Ordem e parada declarada.** Abra o `HANDOFF-SESSAO.md`, o topo do `RELATO.md` e o bloco `OBRAS` do BACKLOG. Confira se `ORDEM-VIVA-TOPO` aponta para um item aberto e a qual obra pertence o `PAREI: !` ou `ESPERANDO:`. Marcador ausente ou apontando para obra fechada pode dar a impressão de fila vazia. O hook de Stop hoje pode encerrar com item aberto e emitir só um aviso. `app/docs/HANDOFF-SESSAO.md:18` · `CLAUDE.md:272` · `bin/hook_stop_fila1.py:286` · `bin/hook_stop_fila1.py:1499`
2. **Sessão e trabalho em voo.** Veja o painel tmux e a última atividade do transcript. Para `ESPERANDO`, confira produtor, PID e arquivo de sinal. Confira também as travas de teste e sombra e os `.rc` concluídos ainda sem registro de coleta. Se houver recusa, leia o motivo na trilha do disjuntor ou do wrapper Codex. `bin/alarme_sessao_ociosa.py:18` · `CLAUDE.md:272` · `bin/handoff_sessao.sh:252` · `bin/rc_sem_colher.py:6` · `bin/disjuntor.py:4` · `bin/codex_pergunta.sh:517`
3. **Filas e portões da esteira.** Use o relatório `bin/esteira_status.sh --lista` para conferir a fila de fatias e a de integração. Veja se há fatia pronta e livre fora da integração, pronta com portão ou já no ar aguardando smoke. Confira `esteira.pausada` pelo dono, motivo e condição de saída, além de árvore vermelha, quarentena e controlador de teto. `bin/esteira_status.sh:3` · `bin/esteira_status.sh:41` · `bin/esteira_status.sh:63` · `bin/fabrica_estado.py:24` · `bin/esteira.sh:53`
4. **Descubra por que a vaga ficou vazia.** Se a fábrica estiver habilitada, confira o timer e o fim de `logs/fabricante.log`. A mensagem `registro sem item livre com alvo vivo` indica que o fabricante rodou e ficou sem alvo elegível. Se a sessão estiver tocando a fila, confira o próximo item da ordem, seu dossiê, pré-requisito e vaga. Uma trava de lei ou `!` de uma obra deixa seguir a próxima independente. `bin/hasner-fabricante.timer:7` · `bin/trava_a_vazia.sh:46` · `CLAUDE.md:278` · `CLAUDE.md:412` · `CLAUDE.md:444`

**AUTO-AVALIAÇÃO** — resolvi. **ONDE TRAVOU** — o handoff registra `esteira.pausada` na árvore do host e indica que havia item executável na fila 1. O motivo da pausa e o estado ao vivo do tmux e dos timers ficaram [nao verificado]. `app/docs/HANDOFF-SESSAO.md:14` · `app/docs/HANDOFF-SESSAO.md:27` **TEMPO** — ~2 min [nao verificado].

## 28/11 12:23 · sessao ff0ce7fa6409 · - · - · origem=Ronald (tmux)

**T3 — RESPOSTA.** Desde a competência 11/2026, o fechamento lança todos os minutos apurados de feriado trabalhado em folga no 6x1 como HE 100% de feriado, uma vez. O TXT lê essa rubrica do fechamento mensal. O teste do 6x1 sem escala certa mostra 5h30 na rubrica de feriado e zero em horas comuns. `app/ponto/services/fechamento.py:997`, `app/ponto/services/fechamento.py:1002`, `app/folha/export.py:385`, `app/folha/export.py:457`, `app/ponto/tests/test_he100_dia_sem_previsto.py:250`.

O juiz da dobra é `MotorBase._feriado_dobra_no_dia` em `app/ponto/motor_calculo_v2.py:941`. Ele consulta o calendário no dia de início do plantão e a regra do ciclo. `CICLOS_FERIADO_SIMPLES` contém apenas `12x36`. A escolha da rubrica passa por `rubrica_da_folga`, que consulta, quando há escala certa, se o feriado causou a folga do vínculo. `app/ponto/motor_calculo_v2.py:955`, `app/ponto/motor_calculo_v2.py:961`, `app/ponto/motor_calculo_v2.py:3740`, `app/ponto/calculador/regras.py:371`, `app/ponto/calculador/regras.py:428`, `app/escala/models.py:1384`.

O relato registra o apply da competência 11 em produção e informa que 13 dias de 10 colaboradores 6x1 seguiram sem alteração. A correção da janela de autorização nesses casos ficou com `FERIADO-L097-MOTOR`, fora do HEAD registrado. `app/docs/RELATO.md:15264`, `app/docs/RELATO.md:15270`, `app/docs/PROMPTS.md:3176`.

**AUTO-AVALIAÇÃO.** Resolvi. `app/ponto/motor_calculo_v2.py:941`, `app/ponto/tests/test_he100_dia_sem_previsto.py:250`.

**ONDE TRAVOU.** A regra e a ressalva de produção aparecem no calculador, nos testes e no relato. O estado atual de produção está [nao verificado]. `app/ponto/calculador/regras.py:371`, `app/ponto/tests/test_he100_dia_sem_previsto.py:250`, `app/docs/RELATO.md:15270`.

**TEMPO.** Cerca de 1 minuto [nao verificado].

## 28/11 12:23 · sessao a92def7fa5f7 · - · - · origem=Ronald (tmux)

### T4. Pousar uma obra com reversão

1. Preparar na cópia do HEAD, com RED antes da cura, escopo literal e linha LEI-AKITA no commit. Classificar mão única ou mão dupla e conferir `!` e B3. Mão única, concorrência, schema e multi-tenant exigem censo, mutante, revisão Codex e Opus final após a suíte. Mão dupla pode pousar com as pendências registradas. (`CLAUDE.md:37–40`, `:62–69`, `:255–265`, `:274–287`, `:423–433`) [nao verificado]
2. Antes do pouso, registrar a reversão no RELATO. Para um merge de código, preparar `git revert -m 1 <merge>` seguido de `bin/deploy.sh`. Se a obra aplicar dinheiro em competência não exportada, salvar em `logs/` o snapshot dos campos por colaborador, provar por hash que as exportadas ficaram intactas e publicar a prova de frota depois, no mesmo marco. Na competência guardada, exigir `!`, DIFF prévio e a porta staff. (`app/docs/PROMPTS.md:3202`, `app/docs/RELATO.md:351`, `CLAUDE.md:455–469`)
3. Obter suíte inteira verde na árvore que vai pousar. Na pista, exigir `bin/pista_certifica.sh` com rc 0, senão rodar na VM. Manter os bytes certificados e registrar o `cmp` no commit. (`CLAUDE.md:104–107`, `:129`, `:347–351`)
4. Pousar antes de abrir nova fatia. Na árvore principal, fazer `git merge --no-ff --no-commit`, resolver, commitar com `OBRA: <id>` e executar `bin/deploy.sh` no mesmo ato. `MERGE_HEAD` vazio após o merge indica fast-forward e deploy imediato. Instrumento segue em pouso próprio, depois do produto. (`CLAUDE.md:284–287`, `:352–356`, `app/docs/INFRA.md:65–78`)
5. Conferir os portões do deploy, a migração quando aplicável, o HUP conjunto e as três provas de rota. Fazer o smoke em produção e registrar no RELATO. Se o smoke exigido reprovar, executar o revert previsto, repetir deploy e provas. Para front abrangido pela L-072, o push espera o clique nas duas cascas. (`bin/deploy.sh:120–125`, `:308–325`, `:416–419`, `CLAUDE.md:477–479`, `app/docs/LEIS.md:99`, `app/docs/RELATO.md:4575`, `:4938`)
6. Se também houve escrita de dados, seguir a reversão específica. O exemplo da O211 primeiro reverte e republica o código, depois restaura apenas os campos-alvo pela porta `restaurar_fechamento`. (`app/docs/RELATO-ARQUIVO.md:32867–32874`)

**Auto-avaliação** — resolvi. **Onde travou** — nenhuma etapa documental após consultar `CLAUDE.md:255`, `app/docs/INFRA.md:38`, `bin/deploy.sh:308` e `app/docs/RELATO.md:4575`. Execução em produção [nao verificado]. **Tempo** — cerca de 2 min [nao verificado].

## 28/11 12:22 · sessao bd5d477b8c38 · - · - · origem=Ronald (tmux)

**T2 — Resolvi.** As perguntas 61520 e 61521 do chamado 40588 cobravam 11h e 12h do dia 20/11. Elas foram emitidas em 23/11 às 12h36, quando esses marcos constavam no DNA da célula 122468. Às 12h42, a célula foi regenerada com jornada das 07h às 19h e sem intervalo previsto. As duas perguntas continuaram pendentes. [app/docs/PROMPTS.md:3223](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/docs/PROMPTS.md:3223) · [app/docs/RELATO.md:15208](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/docs/RELATO.md:15208)

A causa é da classe `ORFA-DE-REGENERACAO`. A correção pertence à fatia `CHAMADO-PRESO-CLASSE` da obra `ESCRITOR-UNICO-DE-CHAMADO`, no E2, após o pouso da E4C. `INTERVALO-SEM-PREVISTO`, da `RAIA-INTERVALO`, foi a investigação. `C7-TELA-ADMIN-ENCERRA` é a via manual pelo admin. [app/docs/BACKLOG.md:835](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/docs/BACKLOG.md:835) · [app/docs/BACKLOG.md:2654](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/docs/BACKLOG.md:2654) · [app/docs/PROMPTS.md:3229](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/docs/PROMPTS.md:3229)

**Onde travou.** A busca por `40588` e pelos horários encontrou a conclusão em `PROMPTS.md`, `RELATO.md` e `BACKLOG.md`. O relato cita `logs/INTERVALO-SEM-PREVISTO/l1/sonda`, cujo conteúdo bruto está [não verificado]. [app/docs/RELATO.md:15208](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/docs/RELATO.md:15208)

**Tempo aproximado.** 1 min [não verificado].

## 28/11 12:09 · sessao 15877447f162 · CUSTO-DE-COORDENACAO-OBRA-PRONTA-ACORDA · leitura · origem=Code

**Resumo.** `ESPERA_UNICA` consulta `rc_sem_colher.py` na entrada. Um `.rc` sem colher já acorda a espera, com retorno `3` do leitor. O laço só detecta `.rc` novo. `bin/rc_sem_colher.py:86–107` `bin/disjuntor.py:1964–1971`  
**ORDEM VIVA.** `_item_da_ordem_viva` julga o item em curso pelo marcador, hoje `ORDEM-VIVA-2811-1200`. Lista estruturada de membros e próximo passo por obra [nao verificado]. `app/docs/BACKLOG.md:8,1926` `bin/hook_stop_fila1.py:188–249`  
**Job vivo e `PAREI`.** Há prova de PID e censo, lease via `flock`, estado da pista e leitura de agentes. Juiz unificado de job vivo por obra [nao verificado]. O Stop valida `PAREI` do item em curso. Validação para qualquer obra da ordem [nao verificado]. `bin/hook_stop_fila1.py:599–626,725–765,1358–1423` `bin/codex_admissao.py:188–193` `bin/suite.sh:383–391` `bin/disjuntor.py:469–485`  
**Última atividade e lei.** O leitor mede `mtime` para julgar colheita. Juiz de última atividade por obra [nao verificado]. **0 perguntas de lei identificadas**. Semântica conjunta do limiar e do prazo de despertar, ano e desempate da recência [nao verificado]. `bin/rc_sem_colher.py:25–36,50–83` `app/docs/BACKLOG.md:1926–1927,2249` `aval_ordem.txt:1`  
**Próximo passo do Opus.** Depois da F1 v7, conferir sobreposição (diff exato [nao verificado]), provar os REDs com relógio injetado e estender o leitor existente e o selo do marcador. `app/docs/BACKLOG.md:1926,2249` `app/docs/RELATO.md:15212` `app/docs/PROMPTS.md:3225,3233` `app/docs/PLANO-PADROES.md:97–103`

- **Encaixe** `bin/rc_sem_colher.py:main/sem_colher`, `bin/tests/test_rc_sem_colher.sh:caso_espera` e `bin/tests/test_hook_nao_cobra_congelado.sh:_julga`. Reusar o juiz do hook e conferir `bin/disjuntor.py:ESPERA_UNICA` se mudar a forma da espera. `app/docs/BACKLOG.md:2249` `bin/tests/test_rc_sem_colher.sh:75–106` `bin/tests/test_hook_nao_cobra_congelado.sh:105–152` `bin/tests/test_agente_no_modelo.sh:291–297`
- **9 min / 600 s / 11 min** `bin/rc_sem_colher.py:main/sem_colher`. Sem `.rc` que acorde a espera, silêncio aos 9 min e aos 600 s, aviso com ID e próximo passo aos 11 min. Mutantes `>= 600`, limite removido, só `.rc` novo e aviso sem próximo passo. `app/docs/BACKLOG.md:2249` `bin/disjuntor.py:1965–1971`
- **Limiar durante a espera** `bin/disjuntor.py:ESPERA_UNICA`. Entrar aos 9 min e cruzar o limiar sem `.rc` novo. Mutante que consulta o leitor só na entrada. Prazo exato [nao verificado]. `app/docs/BACKLOG.md:2249` `bin/disjuntor.py:1964–1971`
- **`PAREI`** `bin/hook_stop_fila1.py:_topo/_parei_do_em_curso`. Aos 11 min, `PAREI` válido da própria obra suprime o aviso e o de outra obra deixa o aviso sair. Mutantes sem guarda e guarda global. `app/docs/BACKLOG.md:2249` `bin/hook_stop_fila1.py:462–484,1358–1423`
- **Job vivo** `bin/rc_sem_colher.py:main`. Aos 11 min, prova viva da própria obra suprime o aviso e produtor morto restaura o aviso. Mutantes sem prova e aceitação de PID declarado ou JSON residual. `app/docs/BACKLOG.md:2249` `bin/hook_stop_fila1.py:599–626` `bin/codex_admissao.py:188–193` `bin/codex_pergunta.sh:52–56`
- **Fora da ORDEM VIVA** `bin/hook_stop_fila1.py:_item_da_ordem_viva`. Obra andável com pasta em `logs/`, ociosa há 11 min e fora da ordem declarada fica sem aviso. Mutantes que aceitam qualquer pasta viva ou qualquer item andável. `app/docs/BACKLOG.md:2249` `bin/rc_sem_colher.py:55–60` `bin/hook_stop_fila1.py:205–249`
- **Marcador na mais nova** `bin/tests/test_hook_nao_cobra_congelado.sh:_julga`. Duas ordens abertas, marcador na mais nova, selo verde. Mutantes `>=` na comparação e reprovação sempre que há duas ordens. `aval_ordem.txt:1` `bin/tests/test_hook_nao_cobra_congelado.sh:105–132`
- **Marcador na mais velha** `bin/tests/test_hook_nao_cobra_congelado.sh:_julga`. Selo vermelho identifica o alvo e a ordem mais nova, inclusive quando a mais nova está fisicamente abaixo. Mutantes sem comparação de recência, só existência/estado e escolha pela posição da linha. `aval_ordem.txt:1` `app/docs/BACKLOG.md:1936,2666` `bin/tests/test_hook_nao_cobra_congelado.sh:105–132`

## 28/11 12:07 · sessao 0ce98f2df43e · CUSTO-DE-COORDENACAO-OBRA-PRONTA-ACORDA · censo · origem=Code

## 1. Mapa de `rc_sem_colher.py`

O leitor abre `app/docs/RELATO.md` e percorre `logs/<OBRA>`. Aceita `--raiz`, `--relato` e `--min-idade`. Considera pastas com ID em maiúsculas, dígitos e hífen e `mtime` de até 24 h. Dentro delas, percorre subpastas e arquivos na mesma janela, ignora arquivos que sejam links simbólicos e escolhe o `.rc` de maior `mtime`. Um arquivo de outro tipo mais de 5 s posterior ao `.rc`, ou uma marca correspondente no RELATO, tira o resultado da lista. O leitor atual só abre essas duas fontes. `bin/rc_sem_colher.py:21–36,50–83,86–107`

| Saída do leitor | Significado |
|---|---|
| `0` | Nenhum resultado, saída vazia. `bin/rc_sem_colher.py:104–107` |
| `3` | Um ou mais resultados. Imprime `RC SEM COLHER <OBRA> <caminho relativo> <HH:MM> rc=<valor>`, dos mais antigos aos mais novos. O `rc=<valor>` vem do conteúdo do arquivo, com `?` se não houver número. `bin/rc_sem_colher.py:23,39–43,78–83,104–107` |
| `1` | Argumento desconhecido ou exceção de leitura que chega ao processo. Erro na caminhada de diretórios é propagado. `bin/rc_sem_colher.py:13–14,46–47,62,90–111` |

- **`disjuntor.ESPERA_UNICA`** chama o leitor uma vez, antes do laço. Com `0`, espera até 300 s pelo primeiro `.rc` com `ctime` posterior à marca, consultando a cada 5 s. Com `3`, pula o laço e a lista já está na saída. Em ambos, faz o `find` final e sai `0`, inclusive se o `timeout` venceu. Com `1` ou outro código, pula o laço, ainda faz o `find` final e sai `1`. O `-cnewer` permite acordar com publicação por `rename` mesmo quando o `mtime` do temporário é anterior à espera. `bin/disjuntor.py:1964–1971`
- **`logs/espera_unica.sh`** aparece no RELATO como forma da espera pelo primeiro `.rc` novo ou 5 min. O script, a chamada ao leitor e o tratamento de cada código estão **[nao verificado]**. `app/docs/RELATO.md:13865`
- **Hooks**. O disjuntor usa `ESPERA_UNICA` como comando sugerido nas recusas de espera e polling. O `Stop` executa `hook_stop_fila1.py`, que decide pelo painel do RELATO e pelo BACKLOG. Na cópia, esses caminhos do hook não consomem `0`/`3`/`1` do leitor. `.claude/settings.json:3–9` `bin/disjuntor.py:2174–2209,2243` `bin/hook_stop_fila1.py:1489–1528`
- **Vercodex** é citado como leitor com `--min-idade 900`. O filtro atual exige idade estritamente maior que 900 s quando recebe essa opção. O tratamento de cada código no executável está **[nao verificado]**. Há registro de que uma tentativa anterior fez o vercodex contar outra classe como `RC SEM COLHER`. `bin/rc_sem_colher.py:2–4,76,96–97` `app/docs/RELATO.md:13907,14450`

## 2. Autoridades que já existem

| Pergunta | Juiz e alcance |
|---|---|
| **Quais obras estão na ORDEM VIVA?** | `_item_da_ordem_viva(texto)` responde **qual é o item em curso**. Na cópia, o marcador aponta `ORDEM-VIVA-2811-1200`, cuja linha declara em prosa as sequências por vaga e pista. `_itens_andaveis` e `disjuntor._pertence_fila1` respondem perguntas sobre o bloco OBRAS e a fila 1 do censo. A lista estruturada dos membros e dos próximos passos da ORDEM VIVA está **[nao verificado]**. A tentativa anterior de arquivo de IDs está registrada como estacionada. `app/docs/BACKLOG.md:8,1926,3047–3049` `bin/hook_stop_fila1.py:188–249,272–283` `bin/disjuntor.py:1797–1808,1846–1869` |
| **`pid=` e `ESPERANDO`** | `hook_stop_fila1._esperando_com_prova` lê o painel e valida os campos declarados. `_prova_pid` confere `/proc`, identidade do produtor, ausência de estado morto/zumbi e estabilidade do processo durante a leitura. `arquivo=` prova avanço de `mtime` entre avaliações e grava memória. A forma de `ESPERANDO` carrega produtor e prova, sem campo de obra. Para **censo**, `_prova_censo(obra)` já casa o wrapper vivo com `CODEX_OBRA` exato e papel `censo` ou `leitura`. `bin/hook_stop_fila1.py:599–626,639–647,725–765,783–833` |
| **Lease de vaga** | O `flock` do kernel em `logs/vagas/vaga.k` decide ocupação de vaga Codex. `codex_admissao._ocupada` faz a sondagem. `vaga.k.json` contém a projeção da chamada, incluindo a obra. PID no JSON e trilha de abriu/fechou são diagnósticos. O executor pode continuar segurando a lease depois da morte do titular. `bin/codex_admissao.py:188–193` `bin/codex_pergunta.sh:52–70,507–510,552–555` `bin/codex_vagas_estado.py:4–9` |
| **Pista e agentes** | Na pista, `suite.sh` lê `pista estado` e distingue `rodando`, `na fila` e `fim`; o estado traz ID de job, e o despacho registra ID e SHA. Para agentes de fundo, `disjuntor.agentes_sem_entrega` cruza lançamento e notificação de fim no transcript da sessão. Um juiz único de **job vivo por obra** cobrindo essas fontes e as leases está **[nao verificado]**. `bin/suite.sh:379–391,447–461` `bin/disjuntor.py:469–485` |
| **`PAREI` no topo** | `hook_stop_fila1._topo` recorta a faixa do censo e a janela do painel. `_parei_do_em_curso` verifica `PAREI:`, `!` e ID inteiro do item em curso, com as exclusões do Stop. `_relato_parou` ainda exige que nenhum outro item ande para devolver o turno. O alarme de sessão tem um leitor próprio de 40 linhas, para a parada da sessão. Um juiz que devolva `PAREI` legítimo **por qualquer obra da ORDEM VIVA** está **[nao verificado]**. `bin/hook_stop_fila1.py:408–416,462–484,1342–1424` `bin/alarme_sessao_ociosa.py:119–145` |
| **Última atividade** | `rc_sem_colher.sem_colher` já mede o maior `mtime` de `.rc` e o maior `mtime` de outro arquivo da obra para decidir **colheita**. `marcas()` casa `(obra, caminho relativo do rc, HH:MM do mtime do rc)` quando `colhido` ou `superado` é a primeira palavra depois de `- HH:MM `. Esse conjunto guarda a identidade do `.rc`, sem guardar o horário da ação de colheita. Um juiz de **última atividade da obra** que reúna esses sinais, jobs e `PAREI`, inclusive para obra sem `.rc`, está **[nao verificado]**. `bin/rc_sem_colher.py:25–36,50–83` |

O `PROXIMO PASSO` do handoff já aponta o item do marcador e remete ao BACKLOG e ao RELATO. A fonte de um próximo passo executável para **cada** obra da lista está **[nao verificado]**. `bin/handoff_sessao.sh:313–326` `bin/hook_stop_fila1.py:272–283,308–318`

## 3. Censo do marcador e das linhas

- **Escrita da ordem.** O marcador e o bloco OBRAS são mantidos à mão. A linha `ORDEM-VIVA-2811-1200` registra que a inclusão da ordem e a troca do marcador ocorreram no mesmo commit. O item B exige esse mesmo ato para todo aval de ordem. `gerar_backlog.py` relê e repõe a janela inteira ao regenerar o arquivo, com uma guarda para impedir que o marcador desapareça. `backlog_estado.py` regrava a célula `c[3]` do ID solicitado e consulta o hook antes e depois. `app/docs/BACKLOG.md:8,1926` `aval_ordem.txt:1` `bin/gerar_backlog.py:102–140` `bin/backlog_estado.py:29–68`
- **Leitura semântica.** O parser do marcador é `_item_da_ordem_viva`. O Stop e `_proximo_da_fila` o chamam. O handoff, `backlog_estado.py` e `interrompidas_estado.py` chegam ao mesmo juiz, diretamente ou por `_proximo_da_fila`. `gerar_estado.py` publica a projeção de interrompidas. O disjuntor lê linhas andáveis do bloco para o censo. `relato.sh` publica o BACKLOG inteiro, e `gerar_backlog_topo.py` seleciona IDs `O` seguidos de dígitos. Um leitor que enumere `ORDEM-VIVA-*` para julgar sua recência está **[nao verificado]**. `bin/hook_stop_fila1.py:188–224,308–318,1499–1512` `bin/handoff_sessao.sh:54–78` `bin/backlog_estado.py:56–63` `bin/interrompidas_estado.py:85–100` `bin/gerar_estado.py:231–243` `bin/disjuntor.py:1797–1808` `bin/relato.sh:135–147` `bin/gerar_backlog_topo.py:11–21`
- **Selos de host.** `test_hook_nao_cobra_congelado.sh` já abre o BACKLOG da árvore, chama o juiz e fica vermelho para marcador ausente, alvo inexistente ou parado. `test_hook_le_o_marcador.sh` usa BACKLOGs de teste e passa pelo Stop para provar que o marcador escolhe o item e que fila vazia libera. `test_hook_estado_e_a_coluna.sh`, `test_hook_parei_com_andavel.sh`, `test_hook_parei_trava_o_item_em_curso.sh`, `test_handoff_sessao.sh` e `test_interrompidas_estado.sh` também exercitam esse caminho. `bin/tests/test_hook_nao_cobra_congelado.sh:105–152` `bin/tests/test_hook_le_o_marcador.sh:38–103` `bin/tests/test_hook_estado_e_a_coluna.sh:41–65` `bin/tests/test_hook_parei_com_andavel.sh:27–49` `bin/tests/test_hook_parei_trava_o_item_em_curso.sh:22–42` `bin/tests/test_handoff_sessao.sh:122–145` `bin/tests/test_interrompidas_estado.sh:37–44,60–65`
- **Encaixe do item B.** A extensão cabe em `test_hook_nao_cobra_congelado.sh`, junto à checagem do BACKLOG da árvore. Usa o alvo devolvido pelo juiz atual e o reconhecedor de linha do hook. A comparação de recência é a asserção nova do selo. A régua já transforma falha desse selo em VERMELHO. `bin/tests/test_hook_nao_cobra_congelado.sh:78–83,105–152` `bin/hook_stop_fila1.py:116–129,205–224` `bin/regua.sh:134–145` `aval_ordem.txt:1`

## 4. Esmeril de «mais nova»

| ID na cópia | Data e hora disponíveis |
|---|---|
| `ORDEM-VIVA-2811-1200` | `DDMM-HHMM` no ID, `28/11 12:00` no texto do aval e `28/11` na última célula. `app/docs/BACKLOG.md:1926` |
| `ORDEM-VIVA-1511-NOITE` | Dia no ID, `20:1x` no texto e `15/11` na última célula. `app/docs/BACKLOG.md:1927` |
| `ORDEM-VIVA-0811` | Dia no ID, `08:5x` no texto e `08/11` na última célula. `app/docs/BACKLOG.md:1936` |
| `ORDEM-VIVA-1811-MANHA` | Dia no ID, `18/11 08:55` na descrição e última célula com caminho de log. A linha de 18/11 aparece fisicamente depois da de 08/11. `app/docs/BACKLOG.md:1936,2666` |

O hook percorre o bloco OBRAS e procura a primeira célula **igual ao alvo do marcador**. Não compara datas. A ordem física é usada por `_itens_andaveis` para devolver a lista da tabela. No caso dado, a comparação de 28/11 12:00 com 15/11 é inequívoca, e a cópia já aponta 28/11 12:00. Um contrato geral de ano, empate e precedência entre ID, coluna e prosa está **[nao verificado]**. `bin/hook_stop_fila1.py:205–249` `app/docs/BACKLOG.md:8,1926–1927` `backlog.txt:1926` `aval_ordem.txt:1`

## 5. Casos RED descritos

Para o primeiro grupo, usar relógio injetado e uma árvore sem `.rc` novo ou `RC SEM COLHER` capaz de acordar a espera pelo caminho antigo. O positivo precisa falhar na cópia pela ausência do novo aviso. Os negativos protegem as exclusões e precisam derrubar seus mutantes. `app/docs/PLANO-PADROES.md:97–103` `app/docs/PROMPTS.md:3225` `bin/tests/test_rc_sem_colher.sh:71–103,124–137`

| Caso | Asserção e mutante que deve cair |
|---|---|
| **Ociosa há 9 min × 11 min** | Mesma obra da ordem, sem job e sem `PAREI`. Aos 9 min, nenhum aviso de ociosidade. Aos 11 min, a espera retorna com o ID da obra **e** o próximo passo declarado. Mata o mutante que elimina o limite, o que só espera `.rc` novo e o que omite o próximo passo. Um controle em 600 s distingue `> 600` de `>= 600`. `backlog.txt:2249` `bin/disjuntor.py:1965–1971` |
| **Com `PAREI`** | Aos 11 min, `PAREI` válido no topo para a própria obra suprime o aviso. Uma trava de outra obra deixa passar o aviso da primeira. Mata a remoção da guarda e a guarda global por qualquer `PAREI`. `backlog.txt:2249` `bin/hook_stop_fila1.py:1358–1424` |
| **Com job vivo** | Aos 11 min, prova do job da própria obra suprime o aviso. Com a mesma declaração e produtor morto, o aviso volta. Mata a remoção da prova e a aceitação de PID escrito ou JSON residual como prova suficiente. `backlog.txt:2249` `bin/hook_stop_fila1.py:599–626` `bin/codex_pergunta.sh:52–56` |
| **Fora da ORDEM VIVA** | Obra andável no bloco OBRAS, com pasta em `logs/`, ociosa há 11 min e ausente da ordem declarada. Nenhum aviso. Mata a troca da pertinência à ORDEM VIVA por «qualquer pasta viva» ou «qualquer item andável». `backlog.txt:2249` `bin/rc_sem_colher.py:55–60` `bin/hook_stop_fila1.py:227–249` |
| **Marcador igual à linha mais nova** | Duas ordens de teste abertas, marcador na mais nova. Selo verde. Mata `>=` no lugar de `>` e o selo que fica vermelho sempre que há duas ordens. `aval_ordem.txt:1` `bin/tests/test_hook_nao_cobra_congelado.sh:105–132` |
| **Marcador mais velho** | Mesmas ordens abertas, marcador na mais velha. Selo VERMELHO pela recência, indicando o alvo e a linha posterior. Repetir com a mais nova fisicamente abaixo da anterior. Mata a remoção da comparação, o selo que só valida existência/estado e a escolha pela posição da linha. `aval_ordem.txt:1` `app/docs/BACKLOG.md:1936,2666` `bin/tests/test_hook_nao_cobra_congelado.sh:105–132` |

Há uma diferença entre o limiar e o prazo de despertar. Com a forma atual, uma obra com 9 min de ociosidade na entrada pode chegar a 14 min antes de o comando devolver o controle. O `find` dentro do laço só vê `.rc` novo. Assim, o par 9/11 cobre a decisão **na entrada**, e o requisito de acordar durante a mesma espera exige um caso com passagem do limiar e nenhum `.rc` novo. O BACKLOG registra tanto «mais de 10 min» quanto «acordando em até 10 min»; a semântica conjunta desse prazo está **[nao verificado]**. `bin/disjuntor.py:1964–1971` `backlog.txt:2249`

## 6. Arquivos da cura e cruzamento com F1 v7

| Arquivo | Toque previsto pelo encaixe |
|---|---|
| `bin/rc_sem_colher.py` e `bin/tests/test_rc_sem_colher.sh` | Leitor existente para o caso novo e selo da entrada normal da espera. O teste atual já exerce retorno imediato, timeout, publicação por `rename` e falha do leitor. `backlog.txt:2249` `bin/tests/test_rc_sem_colher.sh:75–106` |
| `bin/tests/test_hook_nao_cobra_congelado.sh` | Selo de recência junto ao veredito do marcador. `aval_ordem.txt:1` `bin/tests/test_hook_nao_cobra_congelado.sh:105–152` |
| `bin/hook_stop_fila1.py` | Dependência de leitura do marcador e do painel. O selo B pode usar as funções existentes. Uma edição para generalizar a prova de `PAREI` por obra cruzaria a F1. `bin/hook_stop_fila1.py:188–224,462–484,1358–1424` `app/docs/RELATO.md:15074,15160,15212` |
| `bin/disjuntor.py` | Dependência da entrada `ESPERA_UNICA`. O `3` já acorda quando o caso é detectado ao entrar. Se a cura alterar a forma da espera para tratar a passagem do prazo durante o bloqueio, há cruzamento com F1 e com o selo da forma publicada em `CLAUDE.md`. `bin/disjuntor.py:1964–1971` `bin/tests/test_agente_no_modelo.sh:292–297` `app/docs/RELATO.md:15074,15155,15212` |
| `bin/backlog_estado.py` | O selo B pode consultar o juiz sem editar esse escritor de célula. A v5 registrou o arquivo fora de `ARQUIVOS`; a entrega v7 registra seis arquivos sem enumerá-los. A presença dele no diff exato da v7 está **[nao verificado]**. `bin/backlog_estado.py:56–68` `app/docs/RELATO.md:15047,15212` |

A fila das 12:00 põe **CUSTO F1 v7 → OBRA-PRONTA-ACORDA** na vaga de opus construtor. O RELATO registra commit da v7 **na cópia**. O hash informado e o diff exato da v7 estão **[nao verificado]**. `backlog.txt:1926,2249` `app/docs/RELATO.md:15212`

**Padrão.** `Single Source of Truth` para as autoridades reaproveitadas, `Clock Injection` para o limiar e `Fitness Function` para o selo de recência. `app/docs/PLANO-PADROES.md:53–62,97–103`

## 28/11 12:06 · sessao 33722f5a8a31 · FERIADO-BOTAO-FAIXA · leitura · origem=Code

## 1. Mapa da faixa

A faixa roxa é o `<div>` que imprime `badge.label` sobre `background:{{ badge.cor }}`. O serviço entrega `label='Feriado'` e `cor='#7c3aed'`. A faixa tem estilo inline e sai do laço `dia.badges`. O ícone de bandeira e a amostra da legenda são elementos separados. [calendario.py:711](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/colaboradores/services/calendario.py:711) [_calendario_grade.html:142](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/colaboradores/partials/_calendario_grade.html:142) [_calendario_grade.html:115](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/colaboradores/partials/_calendario_grade.html:115) [_calendario_legenda.html:17](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/colaboradores/partials/_calendario_legenda.html:17)

**O mecanismo de corte na cópia é o limite de altura combinado com `overflow`.** Em `layout='mes'`, a célula tem `height:132px`, `min-height:132px` e `overflow:hidden`. Quando a ação é oferecida a quem pode editar, a célula vira uma coluna flex. A faixa fica dentro da região `flex:1;min-height:0;overflow-y:auto`, que recebe o espaço restante. Se a faixa cruza a borda dessa região, só a parte dentro dela aparece até a rolagem. O `z-index` ordena a pintura do botão e da região. O smoke registra o corte. Qual borda cortou a faixa no navegador do smoke e quantos pixels ficaram ocultos estão [nao verificado]. [backlog.txt:2047] [_calendario_grade.html:68](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/colaboradores/partials/_calendario_grade.html:68) [_calendario_grade.html:95](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/colaboradores/partials/_calendario_grade.html:95) [_calendario_grade.html:142](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/colaboradores/partials/_calendario_grade.html:142)

| Casca | Caminho até a faixa |
|---|---|
| `base.html` | **A ficha de página inteira aparece aqui.** O bloco `content` recebe `detalhe.html`, que estende `base.html` literalmente. No `load`, a ficha busca `/colaboradores/<cid>/calendario/` por HTMX. A view devolve `calendario_partial.html`, que inclui `_calendario_grade.html` com `layout='mes'` e chega a `dia.badges`. [base.html:336](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/base.html:336) [detalhe.html:1](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/colaboradores/detalhe.html:1) [detalhe.html:215](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/colaboradores/detalhe.html:215) [views.py:2020](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/colaboradores/views.py:2020) [calendario_partial.html:67](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/colaboradores/calendario_partial.html:67) |
| `base_app.html` | **A ficha de página inteira é exclusiva de `base.html`.** Nas duas cascas, a ficha lateral pode carregar a mesma faixa. As duas incluem `_drawer_generico.html`, que busca `/colaboradores/<cid>/painel/`. A view renderiza `painel_colaborador.html`. A aba **Espelho** busca a mesma rota de calendário, que chega ao mesmo laço de badges. As rotas do painel e do calendário exigem `ver_cadastro`. [detalhe.html:1](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/colaboradores/detalhe.html:1) [base_app.html:154](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/base_app.html:154) [base.html:399](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/base.html:399) [_drawer_generico.html:91](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/core/_drawer_generico.html:91) [_drawer_generico.html:134](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/core/_drawer_generico.html:134) [painel_colaborador.html:182](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/colaboradores/partials/painel_colaborador.html:182) [views.py:935](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/colaboradores/views.py:935) [views.py:1010](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/colaboradores/views.py:1010) [views.py:1986](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/colaboradores/views.py:1986) |

## 2. Censo e alcance

| Include direto da grade | Alcance da mudança |
|---|---|
| `calendario_partial.html` | Ficha inteira, aba Espelho do drawer e abertura direta do calendário pelo **Resolver dia** do Retrato da Folha. Mês, Quinzena, Semana e Competência passam pelo mesmo `layout='mes'` do template. Uma cura limitada ao ramo mensal com oferta alcança esses caminhos. [calendario_partial.html:49](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/colaboradores/calendario_partial.html:49) [calendario_partial.html:67](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/colaboradores/calendario_partial.html:67) [retrato_folha.py:1053](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/folha/retrato_folha.py:1053) [_retrato_o_que_fazer.html:74](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/folha/_retrato_o_que_fazer.html:74) |
| `_drawer_diagnostico.html` | **Escala × batidas** do Painel Situacional usa `layout='fita'`. Consome o estilo comum dos badges e fica fora do ramo mensal. [_drawer_diagnostico.html:33](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/colaboradores/partials/_drawer_diagnostico.html:33) [_calendario_grade.html:68](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/colaboradores/partials/_calendario_grade.html:68) |
| `relatorios/furos.html` | **Auditoria de espelhos** em HTML passa apenas `dias` e `resumo`, com `only`. Consome o estilo comum dos badges e fica fora do ramo mensal. O PDF segue por um gerador próprio. [furos.html:42](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/relatorios/furos.html:42) [relatorios/views.py:1628](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/relatorios/views.py:1628) |

O CSS relevante está todo inline na grade. Uma edição geral no `<div>` do badge alcança também Extra 50%, Extra 100%, Atraso e Antecipada nos três includes. A classe `.icone-dia` do CSS compartilhado dimensiona a bandeira SVG. A legenda tem seu próprio estilo inline. [_calendario_grade.html:68](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/colaboradores/partials/_calendario_grade.html:68) [_calendario_grade.html:99](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/colaboradores/partials/_calendario_grade.html:99) [_calendario_grade.html:142](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/colaboradores/partials/_calendario_grade.html:142) [calendario.py:714](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/colaboradores/services/calendario.py:714) [hasner-ponto.css:578](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/static/css/hasner-ponto.css:578) [_calendario_legenda.html:17](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/colaboradores/partials/_calendario_legenda.html:17)

## 3. Sobreposição com FERIADO-BOTAO-CORTADO

**Sim, a sobreposição é na mesma célula da mesma grade.** O anexo localiza o corte antigo no `overflow:hidden` da célula e no include do botão, então na linha 131. Na cópia, o include do botão está na linha 97, acima da região rolável da linha 99. A faixa continua dentro dessa região, nas linhas 142–144. O diff exato do pouso `627303c71` está [nao verificado]. [backlog.txt:2046] [backlog.txt:2047] [_calendario_grade.html:68](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/colaboradores/partials/_calendario_grade.html:68) [_calendario_grade.html:97](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/colaboradores/partials/_calendario_grade.html:97) [_calendario_grade.html:142](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/colaboradores/partials/_calendario_grade.html:142)

## 4. Teste de tela e RED [nao verificado]

**Teste existente** — `SmokeCliqueFeriadoViraTrabalhoTest.test_clica_no_feriado_na_pagina_e_no_drawer_das_duas_cascas`. A fixture cria o feriado de 20/11/2026 com quatro batidas. O cenário de página `base.html` abre a rota real da ficha e carrega o calendário. A página `base_app.html` do teste é um hospedeiro montado para o ensaio. O drawer usa o painel real nas duas cascas. As asserções cobram botão, popover e acesso à última batida por rolagem. O teste estrutural `test_feriado_botao_cortado.py` usa `badges=[]`. [test_smoke_clique_feriado_vira_trabalho.py:26](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:26) [test_smoke_clique_feriado_vira_trabalho.py:536](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:536) [test_smoke_clique_feriado_vira_trabalho.py:588](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:588) [test_smoke_clique_feriado_vira_trabalho.py:669](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:669) [test_smoke_clique_feriado_vira_trabalho.py:700](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:700) [test_smoke_clique_feriado_vira_trabalho.py:731](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:731) [test_feriado_botao_cortado.py:39](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/colaboradores/tests/test_feriado_botao_cortado.py:39)

**RED proposto** — Reusar essa fixture no Chromium, na ficha real e na aba Espelho das duas cascas. Selecionar o badge textual dentro da célula do dia 20. Manter a célula em 132 px e preparar conteúdo suficiente **antes** da posição atual do badge para cruzar o limite da região rolável. As quatro batidas, sozinhas, podem caber no cenário existente. Cobrar exatamente uma faixa, com os quatro lados visíveis já com `scrollTop=0`, e repetir a medição após rolar até a última batida. Cobrar também que o botão siga visível e clicável. **Mutante a matar:** devolver a faixa ao laço dentro da região rolável, preservando texto, cor, botão e batidas. A falha esperada é de recorte da faixa. RED e mutante executados [nao verificado]. [_calendario_grade.html:99](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/colaboradores/partials/_calendario_grade.html:99) [_calendario_grade.html:142](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/colaboradores/partials/_calendario_grade.html:142) [test_smoke_clique_feriado_vira_trabalho.py:329](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:329) [test_smoke_clique_feriado_vira_trabalho.py:469](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:469)

## 5. Arquivos para conferir colisão

- **Cura proposta** — `app/templates/colaboradores/partials/_calendario_grade.html` para colocar a faixa acima da região rolável no ramo mensal com oferta. Para reaproveitar o mesmo HTML nos dois locais, extrair o `<div>` atual para um novo `_calendario_badge.html` na mesma pasta. A seleção pode usar `dia.feriado` e o primeiro badge. Isso depende do contrato atual do produtor, que insere Feriado primeiro quando `dia.feriado` é verdadeiro. [_calendario_grade.html:95](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/colaboradores/partials/_calendario_grade.html:95) [_calendario_grade.html:142](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/templates/colaboradores/partials/_calendario_grade.html:142) [calendario.py:711](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/colaboradores/services/calendario.py:711) [calendario.py:784](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/colaboradores/services/calendario.py:784)
- **Selo proposto** — `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py`, onde já ficam a sonda de geometria e os mutantes. A colisão com outras obras em voo está [nao verificado]. [test_smoke_clique_feriado_vira_trabalho.py:71](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:71) [test_smoke_clique_feriado_vira_trabalho.py:329](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:329)

**`padrao=fitness function / selo`** para a cura com o RED. O invariante é a faixa inteira à vista enquanto o conteúdo do dia continua rolável. A asserção é de geometria no navegador. [PLANO-PADROES.md:53](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/docs/PLANO-PADROES.md:53) [GLOSSARIO.md:14](/home/ronald/codex_ws/96e66c5f2be6f178128eab4fbd4434246ef71801/app/docs/GLOSSARIO.md:14)

## 28/11 11:46 · sessao 35e63e4b2e65 · E4D-CONTRATO-ROTA-B · leitura · origem=Code

**B é um defeito da view HTMX. Os casos 11 e 44 chegam às asserções de negócio e precisam de curas próprias.** No 44, o traceback mostra a lista **obtida** `[]` e a **esperada** `[('resposta', T0, '08:00')]`, na primeira chamada do comando. Nos três mutantes de C, a falha é um `AssertionError` do controle inicial. Nos seis de B, o `VariableDoesNotExist` do teste interno vira `FAIL` na contraprova, que exige `resultado.errors == []`. Em ambos os grupos, o executor para antes de instalar o mutante. `app/chamados/views.py:394–408` · `app/chamados/tests/e4d_matriz_mutantes.py:570–612` · `aceite_v3.txt`, tracebacks de `/app/chamados/tests/test_e4d_matriz_reds.py:603,834`.

## B · Cartão no GET e no POST HTMX

`chamados:detalhe_local` resolve para `detalhe_chamado_local`. No GET, a view chama `montar_contexto_detalhe`, que lista as perguntas vivas da disputa aberta, fornece `pode_agir` e cria `folha_julga_pontualidade` como `SimpleLazyObject`. O template da página inclui o cartão com esse contexto. No POST `recusar_pergunta`, a mesma view passa `_resp_card` à ação. A recusa sem motivo retorna antes de qualquer escrita, e `_resp_card` renderiza o cartão diretamente com `perg` e `pode_agir`. A variável vem da montagem do GET. A guarda fica na linha 43 e o argumento que o Django tenta resolver fica na 44. `app/chamados/urls.py:14` · `app/chamados/views.py:377–408,506–519,550–552` · `app/chamados/services/acoes_chamado.py:337–358,409–423` · `app/chamados/services/acoes_disputa.py:94–102` · `app/chamados/services/validacao.py:934–937` · `app/config/settings/base.py:70–88` · `app/templates/chamados/detalhe_local.html:307–311` · `app/templates/chamados/partials/_pergunta_card.html:43–44`.

A v3 já percorre essas duas entradas com `self.client.get(url)` e `self.client.post(url, ..., HTTP_HX_REQUEST='true')`. Pela ordem do teste, a primeira HTMX tem validação vigente e pula a guarda da linha 43. Depois da `DEVOLUCAO`, o GET tem a chave e a segunda HTMX entra na guarda com o contexto incompleto. O traceback confirma o contexto do fragmento. O ordinal do request no log é [nao verificado]. `r_v3.md`, B[13] · `app/chamados/views.py:394–408` · `app/templates/chamados/partials/_pergunta_card.html:20–44`.

### B.1 · Contexto de produção

Em `app/chamados/views.py:404–405`, substitua as duas linhas que começam em `_r = _render_card(request, 'chamados/partials/_pergunta_card.html',` e terminam em `{'perg': _p, 'pode_agir': True})`. A âncora composta tem contagem 1. A consulta imediatamente acima já traz `disputa__colaborador`. `app/chamados/views.py:399–405` · `app/chamados/services/acoes_chamado.py:420–422`.

```python
            from django.utils.functional import SimpleLazyObject
            from chamados.juizes import folha_julga_pontualidade

            _r = _render_card(request, 'chamados/partials/_pergunta_card.html', {
                'perg': _p,
                'pode_agir': True,
                'folha_julga_pontualidade': SimpleLazyObject(
                    lambda: folha_julga_pontualidade(_p.disputa.colaborador),
                ),
            })
```

### B.2 · Assertos do mesmo teste HTTP [nao verificado]

Em `app/chamados/tests/test_e4d_matriz_reds.py`, no método `test_cura4_cartao_exibe_composta_no_get_e_no_htmx` inserido por B[13], substitua a linha `            self.assertContains(selo, f'✓ validada {validacao_vigente:%d/%m %H:%M}</span>')` pelo primeiro bloco. Substitua de `                    self.assertNotIn('opacity:.55;', abertura)` até imediatamente antes de `        p.refresh_from_db()` **desse método**, usando como âncora final composta `        p.refresh_from_db()\n        self.assertEqual((p.respondida_em, p.validada_em), (None, validacao_residual))`. Cada âncora tem contagem 1 na v3 aplicada. `r_v3.md`, B[13] · `app/chamados/tests/test_e4d_matriz_reds.py:1616` · `app/templates/chamados/partials/_pergunta_card.html:20–44,118–127`.

```python
            self.assertContains(
                selo, f'✓ validada {validacao_vigente:%d/%m %H:%M}</span>', msg_prefix='C1 hora do selo',
            )
```

```python
                    self.assertNotIn('opacity:.55;', abertura, 'C1 opacidade')
                    self.assertNotContains(retorno, '✓ validada ', msg_prefix='C1 selo residual')
                    self.assertContains(retorno, '⚠ aguardando validacao', msg_prefix='C1 resposta vigente')
                    self.assertContains(
                        retorno, f'— {resposta_em:%d/%m %H:%M}</span>', msg_prefix='C1 hora da resposta',
                    )
                    self.assertContains(
                        retorno, 'name="acao" value="recusar_pergunta"', msg_prefix='C1 acao',
                    )

```

Em `app/chamados/tests/e4d_matriz_mutantes.py`, substitua de `MC1Opacidade = r(` até imediatamente antes de `MC3ApiRele = recibo(`. Cada delimitação tem contagem 1 após B[16]. Os seis continuam ligados a `C1_TESTE`, que é o teste HTTP acima. `falha_em` usa a checagem já inserida por B[12]. `r_v3.md`, B[11–12,15–16] · `app/chamados/tests/e4d_matriz_mutantes.py:602–624`.

```python
MC1Opacidade = r('MC1Opacidade', 'C1.1', C1_TESTE, falha_em='C1 opacidade')
MC1Selo = r('MC1Selo', 'C1.2', C1_TESTE, falha_em='C1 selo residual')
MC1HoraDoSelo = r('MC1HoraDoSelo', 'C1.3', C1_TESTE, falha_em='C1 hora do selo')
MC1HoraDaResposta = r('MC1HoraDaResposta', 'C1.4', C1_TESTE, falha_em='C1 hora da resposta')
MC1Acao = r('MC1Acao', 'C1.5', C1_TESTE, falha_em='C1 acao')
MX3RespondidaCrua = r('MX3RespondidaCrua', 'X3', C1_TESTE, falha_em='C1 resposta vigente')
```

| Mutante | Mordida esperada com o fragmento renderizável, por leitura do código |
|---|---|
| `MC1Opacidade` | A validação residual da coluna esmaece o cartão após a devolução. `app/templates/chamados/partials/_pergunta_card.html:21` |
| `MC1Selo` | A validação residual da coluna exibe um selo após a devolução. `app/templates/chamados/partials/_pergunta_card.html:24–26` |
| `MC1HoraDoSelo` | A primeira HTMX exibe a hora residual no selo. `app/templates/chamados/partials/_pergunta_card.html:24` |
| `MC1HoraDaResposta` | A hora fica vazia porque a coluna `respondida_em` está vazia. `app/templates/chamados/partials/_pergunta_card.html:30–33` |
| `MC1Acao` | A coluna `validada_em` residual esconde o formulário de recusa. `app/templates/chamados/partials/_pergunta_card.html:43,118–127` |
| `MX3RespondidaCrua` | A coluna `respondida_em` vazia muda o estado exibido para `aguardando colaborador`. A checagem de estado vem antes das de hora e ação. `app/templates/chamados/partials/_pergunta_card.html:20,24–33` · `r_v3.md`, B[13,15] |

**Escritores e leitores.** `cartao` grava o texto e os carimbos residuais na projeção. A fixture grava `RESPOSTA`, as duas `VALIDACAO` e `DEVOLUCAO` por `registrar_na_fixture` e `DecisaoPergunta.registrar`. Em produção, `responder_pergunta_disputa_core` grava texto, instante e ato de resposta, e `validacao` grava e devolve a validação. `DecisaoPergunta.leitura` ordena os atos, tira a validação na `DEVOLUCAO` e conserva a resposta. `composta` entrega os instantes dessa leitura e o texto da projeção. O template lê os instantes pelo filtro `resposta_composta`, lê o booleano por `PerguntaDisputa.respondida` e o texto exibido por `resposta_display`. O GET lê `perguntas_disputa` pelo juiz de vitalidade. A view HTMX fornece `perg`, `pode_agir` e `HX-Trigger`, com a mensagem da porta de recusa. `app/chamados/tests/test_e4d_matriz_reds.py:166–175,215–229` · `app/chamados/tests/_ato_da_fixture.py:36–44` · `app/chamados/services/disputa_emissao.py:1531–1546,1564–1570` · `app/chamados/services/validacao.py:164–178,304–316,934–937` · `app/chamados/models.py:3958–3965,3985–3991,4450–4456,4486–4515,4590–4623` · `app/chamados/templatetags/chamados_tags.py:144–182` · `app/chamados/services/acoes_chamado.py:350–358,409–423` · `app/chamados/juizes.py:1499–1533,1644–1653,2081–2102` · `app/chamados/views.py:399–408,418–421`.

## C11 · Pedido de reabertura em pergunta vazia

**A regra da v3 está aberta em `f300431af`.** A assinatura carece de `pedido_id`. A porta só monta o ato quando encontra texto, resposta, validação, materialização ou validação vigente só na sequência. A fixture deixa as duas perguntas realmente vazias, e o primeiro `registrar([])` volta cedo. Mesmo que se retire esse filtro, a porta do lote hoje projeta RVM e acorda células, e `registrar` também acorda células. B[5]/B[6] tratam da recusa na passagem da ausência. A cura do 11 entra na reabertura e na persistência da decisão. `app/chamados/tests/test_e4d_matriz_reds.py:166–179,570–606` · `app/chamados/services/disputa_emissao.py:1631,1674–1719` · `app/chamados/models.py:3594–3618,3657–3660,4590–4623` · `apv3.py`, ramo `--cura`.

A proposta usa uma chave opaca opcional de até 128 caracteres, única por pergunta, no ato imutável. A porta consulta essa chave sob a trava existente. O contrato novo começa na chamada do serviço com pedido identificado. Os chamadores atuais de recusa e de reabertura histórica usam a assinatura sem chave, que também é exercitada pelo controle de pendente vazia. A emissão e o transporte da chave pelas telas atuais são [nao verificado]. `app/chamados/services/disputa_emissao.py:1674–1685` · `app/chamados/models.py:4405–4411` · `app/chamados/services/validacao.py:997–1000` · `app/chamados/services/acoes_disputa.py:120–141` · `app/chamados/views.py:1416–1441` · `app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:816–820`.

### C11.1 · Campo, restrições e emissão compartilhada com C44

Em `app/chamados/models.py:4373`, insira logo após `    resposta_mantem_validacao = models.BooleanField(default=False, db_default=False)` (1).

```python
    pedido_id = models.CharField(max_length=128, null=True, blank=True)
```

Em `app/chamados/models.py:4395`, insira logo após `                name='decisao_mantem_validacao_so_na_resposta'),` (1).

```python
            models.CheckConstraint(
                condition=models.Q(pedido_id__isnull=True) | models.Q(ato='reabertura'),
                name='decisao_pedido_so_na_reabertura'),
            models.UniqueConstraint(
                fields=('pergunta', 'pedido_id'),
                condition=models.Q(pedido_id__isnull=False),
                name='decisao_reabertura_pedido_unico'),
```

Em `app/chamados/models.py:4590`, substitua `    def registrar(cls, linhas):` (1) pela assinatura abaixo. Em `:4622–4623`, substitua `            PerguntaDisputa.acordar_celulas(eventos)\n            return criadas` (1) pelo segundo bloco. A trava, o `bulk_create` e a invalidação do cache continuam na mesma porta. `app/chamados/models.py:4590–4623`.

```python
    def registrar(cls, linhas, *, acordar=True):
```

```python
            if acordar:
                PerguntaDisputa.acordar_celulas(eventos)
            return criadas
```

Texto integral da migração nova `app/chamados/migrations/0057_decisao_pedido_id.py`. O ponto de encadeamento na cópia é a `0056_decisao_resposta_mantem_validacao_historico`. O campo nasce anulável para as linhas existentes. `app/chamados/migrations/0056_decisao_resposta_mantem_validacao_historico.py:17–27` · `app/chamados/models.py:4344–4396`.

```python
from django.db import migrations, models


class Migration(migrations.Migration):
    dependencies = [
        ('chamados', '0056_decisao_resposta_mantem_validacao_historico'),
    ]

    operations = [
        migrations.AddField(
            model_name='decisaopergunta',
            name='pedido_id',
            field=models.CharField(max_length=128, null=True, blank=True),
        ),
        migrations.AddConstraint(
            model_name='decisaopergunta',
            constraint=models.CheckConstraint(
                condition=models.Q(pedido_id__isnull=True) | models.Q(ato='reabertura'),
                name='decisao_pedido_so_na_reabertura',
            ),
        ),
        migrations.AddConstraint(
            model_name='decisaopergunta',
            constraint=models.UniqueConstraint(
                fields=('pergunta', 'pedido_id'),
                condition=models.Q(pedido_id__isnull=False),
                name='decisao_reabertura_pedido_unico',
            ),
        ),
    ]
```

### C11.2 · Porta de reabertura

Em `app/chamados/services/disputa_emissao.py:1631`, substitua `def reabrir_para_correcao(qs, *, por=None, motivo='', ato='reabertura'):` (1). Em `:1665–1669`, substitua de `    # REABRIR-NAO-DESTROI (aval 21/10 RAIA D):` até antes de `    from django.db import transaction\n    from django.utils import timezone\n    from chamados.models import DecisaoPergunta, PerguntaDisputa\n    from chamados.services.trava_do_colaborador import exigir_colaboradores_travados\n    if ato not in DecisaoPergunta.ATOS_COM_FOTO:` (1 de cada) pelo segundo bloco. Em `:1675`, insira o terceiro bloco logo após `        raise ValueError('reabrir_para_correcao: ato %r nao leva foto (%s)' % (ato, DecisaoPergunta.ATOS_COM_FOTO))` (1). `app/chamados/services/disputa_emissao.py:1631,1664–1682`.

```python
def reabrir_para_correcao(qs, *, por=None, motivo='', ato='reabertura', pedido_id=None):
```

```python
    # A trilha guarda a foto anterior no mesmo atomic. Sem pedido identificado, só
    # o que há para compensar deixa linha. Um pedido novo identificado também
    # registra as perguntas vazias. A recusa tem seu próprio ato e a mesma foto.
```

```python
    com_pedido = pedido_id is not None
    if com_pedido and (ato != DecisaoPergunta.REABERTURA or not isinstance(pedido_id, str)
                       or not pedido_id.strip() or len(pedido_id) > 128):
        raise ValueError('pedido_id deve identificar uma reabertura (1 a 128 caracteres)')
```

No mesmo arquivo, substitua de `        # LASTRO-PARTE-3 (O-OUTBOX): o update do lote nao passa pelo post_save. Capture as linhas do evento sob a trava,` até antes de `def responder_automaticamente(p, texto, instante):` (1 de cada). O bloco aproveita `_agora` capturado sob a trava e mantém uma chamada a `DecisaoPergunta.registrar`. `app/chamados/services/disputa_emissao.py:1676–1723` · `app/chamados/models.py:3621–3660` · `app/chamados/tests/test_reabrir_nao_destroi.py:118–130`.

```python
        if com_pedido:
            ja_feitas = DecisaoPergunta.objects.filter(
                ato=DecisaoPergunta.REABERTURA, pedido_id=pedido_id,
            ).values('pergunta_id')
            qs = PerguntaDisputa.objects.filter(pk__in=qs.values('pk')).exclude(pk__in=ja_feitas)

        vazias = [
            pk for pk, texto, resposta, validada, materializada, via in qs.values_list(
                'pk', 'resposta_colab', 'respondida_em', 'validada_em', 'materializada_em', 'via_resolucao',
            )
            if via not in PerguntaDisputa.VIAS_FORA_DO_FATO
            and not ((texto or '').strip() or resposta is not None or validada is not None or materializada is not None)
        ]
        por_ato = {
            q.pk: DecisaoPergunta.composta(q)
            for q in (PerguntaDisputa.objects.filter(pk__in=vazias).prefetch_related('decisoes') if vazias else ())
        }
        origem = (
            'pk', 'resposta_colab', 'respondida_em', 'validada_em', 'validada_por_id', 'materializada_em', 'via_resolucao',
        )
        if com_pedido:
            origem += ('materializada_por_id',)
        atos = []
        projetar = set()
        for pk, texto, resposta, validada, autor, materializada, via, *autor_materializacao in qs.values_list(*origem):
            if via in PerguntaDisputa.VIAS_FORA_DO_FATO:
                continue
            lida = por_ato.get(pk)
            validada_no_ato = lida.validada_em if lida is not None else None
            tem_foto = any(((texto or '').strip(), resposta, validada, materializada, validada_no_ato))
            if not com_pedido and not tem_foto:
                continue
            linha = dict(
                pergunta_id=pk, ato=ato, ato_texto=texto or '', respondida_original_em=resposta,
                validada_original_em=validada if validada is not None else validada_no_ato,
                validada_original_por_id=autor, ato_em=_agora, ato_por=por, ato_motivo=(motivo or '')[:240],
            )
            if com_pedido:
                linha['pedido_id'] = pedido_id
                if (tem_foto or texto or via or autor is not None or autor_materializacao[0] is not None
                        or (lida is not None and lida.respondida_em is not None)):
                    projetar.add(pk)
            atos.append(linha)

        alvo_eventos = qs.filter(pk__in=projetar) if com_pedido else qs
        linhas_eventos = PerguntaDisputa.linhas_do_evento(alvo_eventos)
        DecisaoPergunta.registrar(atos, **({'acordar': False} if com_pedido else {}))
        if com_pedido and not projetar:
            return 0
        opcoes = {'escrever_so': projetar} if com_pedido else {}
        n = PerguntaDisputa.descarimbar_em_lote(qs, **opcoes, **campos)
        PerguntaDisputa.acordar_celulas(linhas_eventos)
        return n


```

### C11.3 · Contraprovas

O teste B[17] já distingue o primeiro pedido, o retry e outro pedido em `T2`. `M11DescartaReaberturaVazia` usa o par B[18], que exclui as vazias antes de `ATOS_COM_FOTO`. `M11RetryGanhaNovoPedido` usa o wrapper existente, que troca a chave na segunda chamada. Após a cura, o primeiro deve falhar na falta do ato inicial e o segundo na presença de uma linha adicional no retry. No aceite, ambos param na falta do primeiro ato do **controle inicial**. `r_v3.md`, B[17–18] · `app/chamados/services/disputa_emissao.py:1674` · `app/chamados/tests/e4d_matriz_mutantes.py:292–302,598–610,650–654` · `aceite_v3.txt`, falhas dos dois M11.

Em `app/chamados/tests/e4d_matriz_mutantes.py:650–654`, substitua de `M11DescartaReaberturaVazia = h(` até antes de `M12AutorDaFotoSetNull = h(` (1 de cada).

```python
M11DescartaReaberturaVazia = h(
    'M11DescartaReaberturaVazia', '11_descarta_reabertura_vazia',
    'test_caso_11_reabertura_vazia_grava_ato_sem_projetar',
    falha_em='Regra 11: o primeiro pedido deixa ato na pergunta vazia',
)
M11RetryGanhaNovoPedido = h(
    'M11RetryGanhaNovoPedido', '11_retry_ganha_novo_pedido',
    'test_caso_11_reabertura_vazia_grava_ato_sem_projetar',
    falha_em='Regra 11: o retry preserva a trilha',
)
```

**Escritores e leitores.** A porta de reabertura monta `ato`, `ato_em`, `ato_por`, `ato_motivo`, `ato_texto` e os três campos `*_original_*`; `registrar` aplica o relógio de compensação e insere a linha. A consulta `values_list(*campos_ato)` de B[17] lê esses valores, e `foto_dos_atos` lê a linha inteira, inclusive o PK. `cartao` monta a projeção de entrada. `descarimbar_em_lote` é quem escreve os sete campos RVM na saída, e `foto` os lê. `acordar_celulas` delega a criação de `EventoCelula` ao escritor do outbox, e `eventos` conta as linhas do colaborador. `app/chamados/services/disputa_emissao.py:1664,1682,1689–1719` · `app/chamados/models.py:3594–3618,3657–3660,4344–4353,4552–4586,4590–4623` · `app/chamados/tests/test_e4d_matriz_reds.py:51–54,166–175,208–229` · `app/ponto/portas/celula.py:981–991` · `r_v3.md`, B[17].

## C44 · Apply do estoque e identidade no retry

**A correspondência já existe. A escrita pedida pelo teste está aberta nessa entrada.** `linhas_a_criar` usa a correspondência por pergunta, ato e instante para medir as respostas faltantes. O `--aplicar` da cópia passa colisões e silêncios à porta, conforme o contrato explícito do comando. Por isso a resposta falta já no primeiro apply. A fixture usa dia dentro do horizonte. Seu `marco_do_estoque` tenta alterar o registro de uma migração ausente na cópia, e o comando examinado seleciona por dia do fato e projeção. O backfill de fio morto já registra respostas no recorte que seu juiz seleciona, por outra entrada. `app/chamados/management/commands/backfill_decisoes_pergunta.py:1–13,44–92,193–234,328–377,486–516,628–631` · `app/chamados/models.py:2714,4415–4448` · `app/chamados/tests/cortes_f2_f9_apoio.py:31–43` · `app/chamados/tests/test_e4d_matriz_re

[truncado em 20000 de 30480 caracteres]

## 28/11 11:42 · sessao dad8aafbdcba · - · - · origem=Ronald (tmux)

O caso 11 está vermelho por comportamento da porta. As perguntas vazias da fixture têm PKs selecionados, mas o predicado do escritor descarta ambas, e a assinatura atual não recebe identidade do pedido. O primeiro asserto da v3 é a falha observada, também nos controles iniciais de `M11DescartaReaberturaVazia` e `M11RetryGanhaNovoPedido`. A mutação só é instalada depois desse controle. `app/chamados/tests/test_e4d_matriz_reds.py:166–176,208–213,570–606` · `app/chamados/services/disputa_emissao.py:1631,1689–1719` · `app/chamados/tests/e4d_matriz_mutantes.py:570–612,650–653` · `aceite_v3.txt`, traceback do teste em `/app/chamados/tests/test_e4d_matriz_reds.py:603`.

A cura pertence a `reabrir_para_correcao` e à persistência de `DecisaoPergunta`. Enviei o bloco à raiz. A identidade opcional fica no ato, com unicidade por pergunta e chave. Sob a trava existente, a porta exclui pedidos já registrados, grava uma linha por pergunta elegível para um pedido novo e só projeta ou acorda células quando há estado para alterar. A supressão seletiva do outbox precisa alcançar `DecisaoPergunta.registrar`, que hoje emite mesmo quando o serviço evita a projeção. As chamadas sem chave conservam a semântica legada, coberta por testes vizinhos. `app/chamados/services/disputa_emissao.py:1674–1719` · `app/chamados/models.py:3621–3660,4344–4396,4590–4623` · `app/chamados/tests/test_e3_porta_A_reabrir.py:44–64` · `app/chamados/tests/test_e4_expand.py:205–227`.

Os marcadores de mordida são `Regra 11: o primeiro pedido deixa ato na pergunta vazia` para o descarte e `Regra 11: o retry preserva a trilha` para a troca de identidade. O mutante de descarte da v3 conserva âncora no teste de `ATOS_COM_FOTO`. O de retry já repassa a chave alterada na segunda chamada e preserva a assinatura inspecionada via `wraps`. `app/chamados/services/disputa_emissao.py:1674` · `app/chamados/tests/e4d_matriz_mutantes.py:292–302,650–653` · `r_v3.md`, bloco do caso 11 do anexo.

As âncoras entregues à raiz têm contagem 1 por leitura em memória. Execução ORM e migração [nao verificado]. `app/chamados/models.py:4373,4395,4590,4622` · `app/chamados/services/disputa_emissao.py:1631,1674–1675,1683,1723` · `app/chamados/tests/e4d_matriz_mutantes.py:650–654`.

## 28/11 11:42 · sessao 6746c6da5887 · - · - · origem=Ronald (tmux)

**Caso 44.** O log mostra `[] != [(RESPOSTA, T0, '08:00')]` na primeira importação. `M44DuplicaRetry` para no controle inicial, por uma asserção da regra. O wrapper duplicador ainda não entrou. `aceite_v3.txt` (anexo) · `app/chamados/tests/test_e4d_matriz_reds.py:809–830` · `app/chamados/tests/e4d_matriz_mutantes.py:248–266,585–605`

A fixture cai no horizonte, com `DIA=14/10/2026`, corte em 21/09 e competência iniciada no dia 21. A lacuna está no comando. `linhas_a_criar` mede as respostas faltantes e o `--aplicar` percorre colisões e silêncios. O `backfill_fio_morto_e4b` já preserva a identidade no seu recorte de fio morto, como mostra o teste com duas chamadas, e opera só na competência corrente. A importação geral de respostas do caso 44 exige cura própria. `app/chamados/tests/cortes_f2_f9_apoio.py:31–43` · `app/chamados/models.py:2714` · `app/chamados/management/commands/backfill_decisoes_pergunta.py:1–13,44–92,328–377,628–631` · `app/chamados/management/commands/backfill_fio_morto_e4b.py:161–164,297–311` · `app/chamados/tests/test_e4_passivo.py:57–72`

Enviei blocos para um modo público `--aplicar-respostas-da-projecao`, com escrita dentro de `aplicar` e de seu `atomic` existente. A modalidade limita a seleção ao horizonte datado, confere exportação e relê a correspondência sob a trava `C → D → P`. O payload vem de `linhas_da_projecao_sem_linha`; `registrar` insere com o instante original e com a emissão de eventos desativada expressamente para esse lote. O teste mantém o primeiro ato, compara todos os campos, inclusive PK, no retry e prova a entrada posterior de outra pergunta. A modalidade anterior conserva seus chamadores. Âncoras conferidas em memória com contagem 1. Execução Django/PostgreSQL da proposta [nao verificado]. `app/chamados/management/commands/backfill_decisoes_pergunta.py:193,240–264,455–463,486–489` · `app/chamados/tests/test_e1_v7_catracas.py:253–292,512–516` · `app/chamados/services/fio.py:1471–1481,1716–1747` · `app/chamados/models.py:2940–3006,4415–4448,4590–4623` · `app/chamados/tests/test_e4d_matriz_reds.py:228–229,809–830`

A fixture escreve o par legado com `cartao`. Em produção, a porta de resposta grava o texto e o carimbo em `disputa_emissao`; a validação grava autor e instante em `validacao`; o modelo grava o trio de materialização. O backfill lê o par legado, `linhas_da_projecao_sem_linha` produz `RESPOSTA`, instante, texto, motivo e marca de conservação da validação, e `registrar` faz o `INSERT`. O teste lê o par e os demais RVM por `foto`, o ato por `values_list` e sua identidade integral por `foto_dos_atos`. `eventos()` conta as linhas de `EventoCelula`, cuja escrita passa por `emitir_evento_celula`. `app/chamados/tests/test_e4d_matriz_reds.py:52–53,166–176,208–229,809–830` · `app/chamados/services/disputa_emissao.py:1531–1569` · `app/chamados/services/validacao.py:164–179` · `app/chamados/models.py:3428–3432,4344–4373,4438–4442,4606–4623` · `app/ponto/portas/celula.py:981–991`

_8926 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
