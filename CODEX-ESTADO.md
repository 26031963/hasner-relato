# CODEX-ESTADO

> Ultima resposta processada: **07/11/2026 06:01** · publicado em **07/11/2026 06:02** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 07/11 06:01 · sessao 761aee8c9215 · ESMERIL-ORACULO-NOTURNA · leitura · origem=Code

**O achado principal é: nesta cópia, calculador e motor usam o mesmo juiz `he_noturna`, mas entregam pontas diferentes: efetivas no calculador, cruas no motor.** Portanto, a diferença demonstrável não é “conta diária versus conta por período”. (`app/ponto/calculador/alimentacao.py:83`, `app/ponto/calculador/alimentacao.py:106`, `app/ponto/calculador/regras.py:827`, `app/ponto/motor_calculo_v2.py:1015`)

Os valores da sonda são o dado fornecido; sua reprodução com os insumos e a versão executada em produção permanece **[nao verificado]**.

**1. Quem decide o campo gravado no oráculo**

O fechamento monta `_rub_o = rubricas_por_dia(...)` e chama a mesma `lavrar`, passando `rubricas=_rub_o` e `versao='oraculo'`. Essa chamada **não passa `do_motor`**, cujo padrão é `()`. (`app/ponto/services/fechamento.py:1271`, `app/ponto/services/fechamento.py:1288`, `app/ponto/services/fechamento.py:1294`, `app/ponto/services/dia_pago.py:140`)

A sequência executada é:

- Primeiro, `lavrar` acumula o valor do motor: `p.minutos_extra_100_noturna / 60`. (`app/ponto/services/dia_pago.py:184`; também no ramo `ft_feriado`, `app/ponto/services/dia_pago.py:252`)
- `_DONOS_DECLARADOS` reúne campos presentes em `valores`, em `nao_decide`, nos mapas entregues e em `do_motor`. Isso verifica **cobertura de responsabilidade**; não escolhe precedência entre valores. (`app/ponto/services/dia_pago.py:60`, `app/ponto/services/dia_pago.py:64`, `app/ponto/services/dia_pago.py:75`)
- Com `rubricas`, campo sem dono provoca `ValueError`, antes da gravação. (`app/ponto/services/dia_pago.py:312`, `app/ponto/services/dia_pago.py:316`)
- O laço percorre `Rubricas.valores`: se o campo pertence a `CAMPOS_DIA` e não está em `do_motor`, executa **`_L[_campo] = float(_valor or 0)` — substitui, não soma**. (`app/ponto/services/dia_pago.py:324`, `app/ponto/services/dia_pago.py:329`)
- `nao_decide` apenas registra o motivo no detalhe; não substitui o valor previamente acumulado. (`app/ponto/services/dia_pago.py:331`)
- As linhas resultantes são gravadas na versão recebida. (`app/ponto/services/dia_pago.py:335`, `app/ponto/services/dia_pago.py:345`, `app/ponto/services/dia_pago.py:359`)

**Logo, quando `valores` contém `horas_extras_100_noturna`, quem decide o número final do oráculo é `regras.do_dia`, por essa substituição.** Se vier somente em `nao_decide`, permanece o acumulado anterior; `do_motor` protegeria explicitamente esse acumulado, mas não é passado nessa chamada. (`app/ponto/calculador/regras.py:838`, `app/ponto/services/dia_pago.py:329`, `app/ponto/services/dia_pago.py:331`, `app/ponto/services/fechamento.py:1277`)

**2. Onde e como o calculador calcula HE100 noturna diária**

`segmentos_do_motor` agrupa os períodos por `resultado.dia_da_jornada[id(p)]`; sem esse mapa, exclui o período e conta `sem_dia_da_jornada`. Não reparte automaticamente o turno entre os dias civis atravessados. (`app/ponto/calculador/alimentacao.py:73`, `app/ponto/calculador/alimentacao.py:78`, `app/ponto/calculador/alimentacao.py:80`)

Para cada período, monta `he_pontas_por_dia` com:

- `efetivo_entrada` e `efetivo_saida`;
- `minutos_extra_50` e `minutos_extra_100` **já calculados pelo motor**;
- os intervalos do motor, reunidos por dia. (`app/ponto/calculador/alimentacao.py:83`, `app/ponto/calculador/alimentacao.py:102`, `app/ponto/calculador/alimentacao.py:106`, `app/ponto/calculador/alimentacao.py:107`)

`rubricas_por_dia` passa esses insumos a `regras.do_dia`; esta chama `he_noturna` **uma vez por período**, soma os retornos de 100% e escreve `_s100 / 60` em `horas_extras_100_noturna`. A quantidade diária de HE100 que o calculador calcula separadamente não alimenta esse split. (`app/ponto/calculador/alimentacao.py:526`, `app/ponto/calculador/regras.py:784`, `app/ponto/calculador/regras.py:827`, `app/ponto/calculador/regras.py:832`, `app/ponto/calculador/regras.py:839`)

As regras do juiz compartilhado são:

| Aspecto | Regra executada |
|---|---|
| Ancoragem do trecho extra | Parte da saída de **cada período** e recua `HE50 + HE100` minutos trabalhados, saltando intervalos e limitado pela entrada. (`app/ponto/motor_calculo_v2.py:392`, `app/ponto/motor_calculo_v2.py:395`, `app/ponto/motor_calculo_v2.py:406`, `app/ponto/motor_calculo_v2.py:440`) |
| Janela | Horário local; interseções com **22–24 e 00–05** nas datas civis atravessadas pelo recorte. (`app/ponto/motor_calculo_v2.py:89`, `app/ponto/motor_calculo_v2.py:97`, `app/ponto/motor_calculo_v2.py:100`, `app/ponto/motor_calculo_v2.py:103`) |
| Após 05h | Com prorrogação ligada, estende 00–05 até o fim recebido se **o próprio recorte extra** tocar 00–05 e continuar após 05h. O início noturno do turno completo, sozinho, não basta. (`app/ponto/motor_calculo_v2.py:109`, `app/ponto/motor_calculo_v2.py:110`, `app/ponto/motor_calculo_v2.py:441`) |
| Intervalos | Desconta o noturno consumido pelos intervalos dentro do recorte. (`app/ponto/motor_calculo_v2.py:442`) |
| Parcela de 100% | Multiplica o noturno encontrado por `extra_100_min / total_extra`. (`app/ponto/motor_calculo_v2.py:447`, `app/ponto/motor_calculo_v2.py:448`) |
| Hora reduzida | **O split não aplica `60/52,5`**: retorna minutos de relógio. A redução pertence à rubrica distinta `horas_noturnas`. (`app/ponto/motor_calculo_v2.py:409`, `app/ponto/motor_calculo_v2.py:440`, `app/ponto/motor_calculo_v2.py:451`, `app/ponto/calculador/regras.py:88`, `app/ponto/calculador/regras.py:849`) |

O calculador obtém o flag de prorrogação uma vez, por `get_motor_cct(c, ini, tipo_escala=te)`, e o passa aos dias. A resolução posterior do vínculo diário não recalcula esse flag. (`app/ponto/calculador/alimentacao.py:390`, `app/ponto/calculador/alimentacao.py:404`, `app/ponto/calculador/alimentacao.py:492`, `app/ponto/calculador/alimentacao.py:528`)

**3. Onde e como o motor calcula no feriado em dobra**

O juiz da dobra é `_feriado_dobra_no_dia`: `feriado_em_dobra and eh_feriado(dia)`. Seu contrato atribui o plantão ao dia de início, incluindo a cauda após meia-noite. (`app/ponto/motor_calculo_v2.py:928`, `app/ponto/motor_calculo_v2.py:934`)

No ramo geométrico `Motor12x36`, o teste usa `e.date()` e coloca a duração líquida inteira em HE100/feriado; depois chama `_aplicar_he_noturna`. Isso depende do flag de dobra, não apenas do nome da classe. (`app/ponto/motor_calculo_v2.py:2785`, `app/ponto/motor_calculo_v2.py:2803`, `app/ponto/motor_calculo_v2.py:2809`, `app/ponto/motor_calculo_v2.py:2855`)

Nos recálculos mensais comercial e partido, a dobra é distribuída **em cada período**, com os minutos trabalhados daquele período, seguida de `_aplicar_he_noturna`. No comercial, o agrupamento usa `data_turno` do juiz, com fallback à data local da entrada. (`app/ponto/motor_calculo_v2.py:3097`, `app/ponto/motor_calculo_v2.py:3104`, `app/ponto/motor_calculo_v2.py:3325`, `app/ponto/motor_calculo_v2.py:3354`, `app/ponto/motor_calculo_v2.py:3361`)

`_aplicar_he_noturna` entrega ao juiz **`periodo.entrada/saida`**, as quotas HE50/100, o flag do motor e `intervalos_do_motor`; grava o retorno de 100% em `minutos_extra_100_noturna`. Portanto, janela, recuo, prorrogação e ausência de fator reduzido são os mesmos descritos em (2), mas aplicados às pontas cruas. (`app/ponto/motor_calculo_v2.py:1014`, `app/ponto/motor_calculo_v2.py:1020`)

As pontas efetivas ficam em campos separados: a janela calcula `e/s`, e `_carimbar_segmento` grava `efetivo_entrada/efetivo_saida` sem substituir as pontas originais. (`app/ponto/motor_calculo_v2.py:2751`, `app/ponto/motor_calculo_v2.py:2759`, `app/ponto/motor_calculo_v2.py:1840`)

**4. Diferenças candidatas para os dois turnos**

**Turno 22:50 → 07:04:** a diferença concreta é ancorar a mesma quota extra na saída efetiva ou na saída crua; isso desloca a interseção com 05h. Também pode haver diferença de flag: o motor é construído por fatia, enquanto o calculador consulta a CCT uma vez para sua chamada. A ocorrência dessas condições nos colaboradores é **[nao verificado]**. (`app/ponto/calculador/alimentacao.py:106`, `app/ponto/motor_calculo_v2.py:1015`, `app/ponto/services/fechamento.py:279`, `app/ponto/calculador/alimentacao.py:390`)

Exemplo **condicional**: se a saída efetiva fosse 05:00, a crua 07:04 e a prorrogação estivesse desligada, o motor gastaria aproximadamente 124 minutos da quota extra depois de 05h, fora da janela estrita; o oráculo ancoraria antes. Essa ordem de grandeza é compatível com a diferença fornecida de aproximadamente 2,06 h, mas **marco 05:00, flag, pausas e reprodução exata são [nao verificado]**. A mecânica está no recuo e na interseção. (`app/ponto/motor_calculo_v2.py:395`, `app/ponto/motor_calculo_v2.py:405`, `app/ponto/motor_calculo_v2.py:110`, `app/ponto/motor_calculo_v2.py:440`)

**Turno da tarde → cerca de 23h:** uma saída efetiva às 23:00 e crua às 23:06 permitiria 60 versus 66 minutos após 22h, compatíveis com 1,00 versus 1,10 h. Esses horários exatos são apenas exemplo: **[nao verificado]**. Prorrogação após 05h não explica isoladamente esse exemplo, pois a interseção relevante seria 22–24. (`app/ponto/calculador/alimentacao.py:83`, `app/ponto/motor_calculo_v2.py:1015`, `app/ponto/motor_calculo_v2.py:103`, `app/ponto/motor_calculo_v2.py:115`)

**Hora reduzida e “diário versus período” não são diferenças entre os dois splits nesta cópia:** ambos chamam a mesma função por período, sem multiplicador reduzido. O comentário de `dia_pago.py` que ainda descreve ancoragens distintas não corresponde às chamadas executáveis atuais. (`app/ponto/calculador/regras.py:827`, `app/ponto/motor_calculo_v2.py:1014`, `app/ponto/motor_calculo_v2.py:451`, `app/ponto/services/dia_pago.py:307`)

Não há elementos para escolher qual regra deveria prevalecer nem para fechar a causa dos dois números observados: **[nao verificado]**.

**5. Leitores e registro de juiz**

Nas rotas identificadas abaixo, os leitores usam **`motor`**, pelos defaults das APIs de leitura. A execução efetiva dessas rotas em produção é **[nao verificado]**. (`app/ponto/services/dia_pago.py:550`, `app/ponto/services/dia_pago.py:752`, `app/ponto/services/dia_pago.py:833`)

| Canal | Chamada encontrada |
|---|---|
| Espelho | `app/ponto/services/espelho.py:410` |
| Calendário | `app/colaboradores/services/calendario.py:290` |
| App/API | `app/api/views.py:1457` |
| Cartão | `app/relatorios/cartao_pela_celula.py:143` |
| Extrato parcial | `app/relatorios/views.py:487` |
| Gestão de HE | `app/ponto/services/gestao_he.py:256` |
| Autorização de HE | `app/ponto/services/autorizacao_he_periodo.py:174` |

O selo/porta de exportação usa soma com default `motor` e consultas explicitamente filtradas por `motor`. O TXT lê `FechamentoMensal`. (`app/folha/porta_export.py:388`, `app/folha/porta_export.py:284`, `app/folha/porta_export.py:589`, `app/folha/porta_export.py:651`, `app/folha/export.py:711`, `app/folha/export.py:777`)

Há, porém, leitores operacionais que **podem alcançar `oraculo`**; portanto, “ninguém lê” seria amplo demais:

| Comando/script | Leitura |
|---|---|
| `lavrar_dias_pagos` | Aceita `--versao`; conta linhas da versão pedida. (`app/ponto/management/commands/lavrar_dias_pagos.py:52`, `app/ponto/management/commands/lavrar_dias_pagos.py:140`) |
| `apurar_furo_da_exportada` | Consulta todas as versões e lê versão/colaborador/furo; sem seleção explícita, processa versões com NULL. (`app/ponto/management/commands/apurar_furo_da_exportada.py:59`, `app/ponto/management/commands/apurar_furo_da_exportada.py:61`, `app/ponto/management/commands/apurar_furo_da_exportada.py:99`) |
| Selo de idempotência | Lê todas as versões, incluindo a versão na chave. (`bin/r5_idempotencia_frota.py:65`, `bin/r5_idempotencia_frota.py:67`) |
| Snapshot de reversão | Fotografa todas as versões e campos selecionados. (`bin/snapshot_badge_linha_09_10.py:50`, `bin/snapshot_badge_linha_09_10.py:60`) |
| Relavraturas específicas | Capturam todas as versões para reversão. (`bin/relavrar_o118_col221.py:39`, `bin/relavrar_o119_col221.py:38`, `bin/relavrar_o120_col221_09.py:68`) |

A execução desses comandos/scripts em produção é **[nao verificado]**. `diff_calculador` compara cálculos vivos e, com `--contra-gravado`, `FechamentoMensal`; não é leitor do `DiaPago` oráculo gravado. (`app/ponto/management/commands/diff_calculador.py:45`, `app/ponto/management/commands/diff_calculador.py:62`, `app/ponto/management/commands/diff_calculador.py:245`)

**O `JUIZES.md` não registra um juiz específico para HE noturna por dia.** Registra cálculo genericamente em `motor_calculo_v2.py` e dobra de feriado em `CICLOS_FERIADO_SIMPLES`; declara-se lookup e aponta ao registro vivo. Isso é distinto da existência da função compartilhada `he_noturna` no código. (`app/docs/JUIZES.md:7`, `app/docs/JUIZES.md:29`, `app/docs/JUIZES.md:73`, `app/ponto/motor_calculo_v2.py:409`)

## 07/11 05:59 · sessao 939e50b04f0b · - · - · origem=Ronald (tmux)

Motor, com referências para consolidação:

- `_aplicar_he_noturna` passa **`periodo.entrada/saida` crus**, os totais HE50/100, o flag de prorrogação e `intervalos_do_motor` ao juiz compartilhado; atribui `minutos_extra_100_noturna` se o retorno for não zero. `app/ponto/motor_calculo_v2.py:1012-1022`.
- As pontas cruas nascem no `PeriodoCalculo`; a janela produz `e/s` e o carimbo guarda esses valores em **outros campos**, `efetivo_entrada/efetivo_saida`, sem substituir `entrada/saida`. `app/ponto/motor_calculo_v2.py:2751`, `:2759-2765`, `:1760-1763`, `:1840-1842`. Comercial segue o mesmo desenho em `:3216`, `:3224-3238`.

Regra efetiva do split:

1. Soma HE50+HE100; reconstrói a ponta extra **a partir da saída**, recuando por minutos trabalhados e pulando intervalos. `app/ponto/motor_calculo_v2.py:432-440`, `:392-406`.
2. Mede noturno nessa ponta e subtrai seus intervalos; reparte pela proporção HE50/HE100. `app/ponto/motor_calculo_v2.py:441-451`.
3. Janela local: 22–24 + 00–05, percorrendo os dias civis atravessados; não corta o cálculo ao dia civil do feriado. `app/ponto/motor_calculo_v2.py:89-117`.
4. Prorrogação: se o flag estiver ligado e **o recorte passado ao juiz** tocar 00–05 e continuar após05, estende até o fim. Como o recorte é `inicio_extra → fim`, a condição é da ponta extra, não automaticamente da jornada inteira. `app/ponto/motor_calculo_v2.py:109-111`, `:440-441`.
5. HE noturna retorna **minutos de relógio**, sem multiplicação por60/52,5. A redução aparece no adicional noturno (`calcular_noturnas`) e no fator1 quando afastada, em outro cálculo. `app/ponto/motor_calculo_v2.py:432-451`, `:983-992`. A soma mensal HE100noturna divide os minutos por60. `:342-344`.

Feriado em dobra:

- O juiz devolve `feriado_em_dobra and eh_feriado(dia)`; o contrato documenta pertencimento ao dia de início do plantão. `app/ponto/motor_calculo_v2.py:928-934`.
- No 12x36 geométrico, testa `e.date()`, atribui a duração líquida inteira a HE100/feriado e depois chama `_aplicar_he_noturna`. `app/ponto/motor_calculo_v2.py:2761-2785`, `:2809-2813`, `:2855`.
- No comercial e partido, o recálculo mensal usa `data_turno` do juiz, com fallback à data local da entrada; distribui a dobra em **cada período**, pelos minutos trabalhados daquele período, e faz split individual. `app/ponto/motor_calculo_v2.py:884-895`, `:2941-2945`, `:3092-3114`, `:3325-3330`, `:3350-3376`.
- A regra padrão da fábrica exclui `tipo_ciclo='12x36'` da dobra; CCT pode reativá-la por cláusula cadastrada. `app/ponto/motor_calculo_v2.py:3623`, `:3693-3694`; `app/core/regua_cct.py:477-478`.
- Prorrogação e afastamento da redução chegam da régua/cadastro; valores aplicados aos colaboradores concretos são `[nao verificado]`. Fluxo: `app/core/regua_cct.py:449-472`; default legal por ciclo em `:153-182`.

Diferença verificável frente ao calculador: este chama o mesmo juiz com `he_pontas`; o agente do calculador identificou que essas pontas vêm dos campos efetivos, enquanto o motor usa os crus. A chamada compartilhada e soma por período estão em `app/ponto/calculador/regras.py:824-839`; lado motor cru em `app/ponto/motor_calculo_v2.py:1015`.

Exemplos **condicionais**, sem afirmar os insumos reais:

- Se a saída efetiva fosse05:00, a crua07:04 e a prorrogação estivesse desligada, o motor gastaria124min da quota extra na cauda após05; esses minutos deslocariam o início do recorte e não entrariam na janela estrita. O oráculo ancorado05:00 teria outra interseção. Isso é compatível com uma diferença próxima de2,06h, pelo recuo e janela citados; saída efetiva05:00 e flag concretos são `[nao verificado]`. `app/ponto/motor_calculo_v2.py:392-406`, `:109-116`, `:440-448`.
- Se a saída efetiva fosse23:00 e a crua23:06, haveria60 contra66min após22, compatível com1,00 contra1,10h; esses dois horários exatos e os demais insumos são `[nao verificado]`. `app/ponto/motor_calculo_v2.py:103-116`, `:435-448`.

Não usar os comentários sobre correções anteriores como prova de qual árvore gerou a sonda; a versão executada e os insumos completos da medição são `[nao verificado]`.

## 07/11 05:59 · sessao e6f623b28100 · - · - · origem=Ronald (tmux)

Leitores encontrados na cópia; a execução efetiva em produção é `[nao verificado]`.

| Canal | O que lê |
|---|---|
| Espelho | `linhas_do_periodo` sem versão explícita: `app/ponto/services/espelho.py:410`. |
| Calendário | Mesma API sem versão explícita: `app/colaboradores/services/calendario.py:290`. |
| App/API | Mesma API sem versão explícita: `app/api/views.py:1457`. |
| Cartão | `soma_da_competencia` sem versão explícita: `app/relatorios/cartao_pela_celula.py:143`. |
| Extrato parcial | `soma_do_periodo` sem versão explícita: `app/relatorios/views.py:487`. |
| Gestão de HE | `soma_do_periodo` sem versão explícita: `app/ponto/services/gestao_he.py:256`. |
| Autorização de HE | `soma_do_periodo` sem versão explícita: `app/ponto/services/autorizacao_he_periodo.py:174`. |

Esses leitores selecionam **motor**, porque os defaults são `versao='motor'`: `app/ponto/services/dia_pago.py:550`, `app/ponto/services/dia_pago.py:752`, `app/ponto/services/dia_pago.py:833`; os filtros efetivos ficam em `app/ponto/services/dia_pago.py:559`, `app/ponto/services/dia_pago.py:784`, `app/ponto/services/dia_pago.py:856`.

O selo/porta de exportação também usa motor: soma default em `app/folha/porta_export.py:388`; filtros explícitos em `app/folha/porta_export.py:284`, `app/folha/porta_export.py:589`, `app/folha/porta_export.py:651`. `dias_fora_do_dia_pago` aceita versão, mas seu default é motor, e a chamada da porta recebe o mapa motor já carregado: `app/folha/porta_export.py:26`, `app/folha/porta_export.py:597`.

O TXT lê **FechamentoMensal**, em `app/folha/export.py:711` e `app/folha/export.py:777`. A função de eventos recebe um fechamento: `app/folha/export.py:402`.

Há, entretanto, comandos/scripts capazes de ler **oraculo**, portanto “ninguém lê” seria absoluto demais:

- `lavrar_dias_pagos`: aceita `--versao`, default motor, e conta linhas da versão pedida: `app/ponto/management/commands/lavrar_dias_pagos.py:52`, `app/ponto/management/commands/lavrar_dias_pagos.py:140`. É ferramenta por ato, conforme registro em `app/config/crons.py:1015`.
- `apurar_furo_da_exportada`: `_universo` consulta **todas** as versões e lê `versao`, colaborador e `furo_apurado`: `app/ponto/management/commands/apurar_furo_da_exportada.py:59`, `app/ponto/management/commands/apurar_furo_da_exportada.py:61`. Sem `--versao`, seleciona todas as versões com NULL: `app/ponto/management/commands/apurar_furo_da_exportada.py:78`, `app/ponto/management/commands/apurar_furo_da_exportada.py:99`; passa cada versão à porta em `app/ponto/management/commands/apurar_furo_da_exportada.py:131`.
- Selo operacional de idempotência lê todas as versões: `bin/r5_idempotencia_frota.py:65`; usa versão na chave e lê trabalhadas/realizados: `bin/r5_idempotencia_frota.py:67`.
- Snapshot de reversão fotografa todas as versões e campos: `bin/snapshot_badge_linha_09_10.py:50`, `bin/snapshot_badge_linha_09_10.py:60`.
- Snapshots incorporados às relavraturas capturam todas as versões: `bin/relavrar_o118_col221.py:39`, `bin/relavrar_o119_col221.py:38`, `bin/relavrar_o120_col221_09.py:68`.

`diff_calculador` compara cálculos vivos, e `--contra-gravado` usa **FechamentoMensal**; isso não constitui leitura da versão oraculo de DiaPago: `app/ponto/management/commands/diff_calculador.py:45`, `app/ponto/management/commands/diff_calculador.py:62`, `app/ponto/management/commands/diff_calculador.py:245`.

O **JUIZES.md não registra juiz específico para “HE noturna por dia”**: contém o endereço genérico de cálculo em `app/docs/JUIZES.md:29` e o de dobra de feriado em `app/docs/JUIZES.md:73`. A tabela declara ser lookup e aponta ao registro vivo: `app/docs/JUIZES.md:7`. No registro vivo, feriado/dobra aponta a `CICLOS_FERIADO_SIMPLES` (`app/core/juizes.py:280`), e a pergunta de totais de competência aponta a `folha/export.py::eventos_do_fechamento` (`app/core/juizes.py:698`, `app/core/juizes.py:711`); essas entradas não nomeiam um juiz de HE noturna diária.

## 07/11 05:59 · sessao a5699bf2ca46 · - · - · origem=Ronald (tmux)

Achado central: o calculador usa **a mesma função `he_noturna` do motor**, porém com pontas efetivas, enquanto `_aplicar_he_noturna` entrega pontas cruas. Não há base para atribuir os valores observados a regras independentes de janela ou hora reduzida. (`app/ponto/calculador/regras.py:824`; `app/ponto/calculador/alimentacao.py:83`; `app/ponto/calculador/alimentacao.py:106`; `app/ponto/motor_calculo_v2.py:1014`)

1. **Fluxo do calculador:** `rubricas_por_dia` recebe `insumos`; estes são montados por `insumos_do_calculador`, que chama `segmentos_do_motor`. O fechamento usa essa porta para construir `_rub_o`. (`app/ponto/calculador/alimentacao.py:305`; `app/ponto/calculador/alimentacao.py:314`; `app/ponto/services/fechamento.py:1269`; `app/ponto/services/fechamento.py:1271`)

2. **Dia de atribuição:** `segmentos_do_motor` obtém o dia por `resultado.dia_da_jornada[id(p)]`; sem esse mapa o período é excluído e contado como `sem_dia_da_jornada`. Portanto a agregação é por dia da jornada, não repartição automática no dia civil. (`app/ponto/calculador/alimentacao.py:73`; `app/ponto/calculador/alimentacao.py:78`; `app/ponto/calculador/alimentacao.py:80`)

3. **Pontas e quantidade de HE:** usa `p.efetivo_entrada/efetivo_saida` e copia `p.minutos_extra_50/minutos_extra_100` do motor. Não usa a quantidade diária de HE recalculada por `do_dia` para decidir o split. Copia também os intervalos do motor para o mapa diário. (`app/ponto/calculador/alimentacao.py:83`; `app/ponto/calculador/alimentacao.py:101`; `app/ponto/calculador/alimentacao.py:102`; `app/ponto/calculador/alimentacao.py:106`; `app/ponto/calculador/alimentacao.py:107`)

4. **Conta diária:** `rubricas_por_dia` passa as pontas, intervalos e flag de prorrogação a `regras.do_dia`. Esta chama `motor_calculo_v2.he_noturna` uma vez por período, soma `_b`, e escreve `val['horas_extras_100_noturna'] = _s100 / 60`. (`app/ponto/calculador/alimentacao.py:514`; `app/ponto/calculador/alimentacao.py:526`; `app/ponto/calculador/alimentacao.py:528`; `app/ponto/calculador/regras.py:824`; `app/ponto/calculador/regras.py:827`; `app/ponto/calculador/regras.py:832`; `app/ponto/calculador/regras.py:839`)

5. **Ancoragem temporal:** `he_noturna` toma `saida` como fim, recua `extra50 + extra100` minutos trabalhados, saltando intervalos e limitado pela entrada, e mede o trecho resultante. Portanto o trecho extra fica no fim de cada período; a janela de prorrogação é julgada a partir desse **trecho extra**, não necessariamente do turno completo. (`app/ponto/motor_calculo_v2.py:392`; `app/ponto/motor_calculo_v2.py:395`; `app/ponto/motor_calculo_v2.py:406`; `app/ponto/motor_calculo_v2.py:432`; `app/ponto/motor_calculo_v2.py:435`; `app/ponto/motor_calculo_v2.py:440`; `app/ponto/motor_calculo_v2.py:441`)

6. **Janela/prorrogação:** converte para horário local; percorre as datas civis do recorte e intersecta 00–05 e 22–24. Com prorrogação habilitada, se `max(inicio, 00h) < 05h` e o fim é posterior às 05h, estende a janela 00–05 até o fim recebido. Assim, um trecho extra que só começa depois das 05h não ganha prorrogação por o turno original ter começado às 22h. (`app/ponto/motor_calculo_v2.py:89`; `app/ponto/motor_calculo_v2.py:97`; `app/ponto/motor_calculo_v2.py:100`; `app/ponto/motor_calculo_v2.py:103`; `app/ponto/motor_calculo_v2.py:109`; `app/ponto/motor_calculo_v2.py:110`; `app/ponto/motor_calculo_v2.py:112`)

7. **Intervalos e repartição:** desconta o noturno consumido por intervalos e reparte o resultado na proporção `extra100 / total_extra`. Se a dobra deixou toda a HE em 100%, todo o noturno medido nesse trecho é HE100 noturna. (`app/ponto/motor_calculo_v2.py:442`; `app/ponto/motor_calculo_v2.py:447`; `app/ponto/motor_calculo_v2.py:448`)

8. **Hora reduzida:** `he_noturna` não recebe nem aplica `hora_reduzida_afastada`, tampouco multiplica por `60/52,5`; o resultado do split é em minutos de relógio. O fator aparece na rubrica distinta `horas_noturnas`: `noturnas_do_dia` multiplica o relógio por 1 ou `FATOR_HORA_NOTURNA`. (`app/ponto/motor_calculo_v2.py:409`; `app/ponto/motor_calculo_v2.py:440`; `app/ponto/motor_calculo_v2.py:448`; `app/ponto/motor_calculo_v2.py:451`; `app/ponto/calculador/regras.py:88`; `app/ponto/calculador/regras.py:89`; `app/ponto/calculador/regras.py:849`)

9. **Origem do flag no calculador:** chama `get_motor_cct(c, ini, tipo_escala=te)` uma vez e obtém `_cct_pro` por `cct_do_motor`; é esse flag que passa a todos os dias. A resolução do vínculo por dia muda tipo, intermitência, intervalo cadastrado e folgas, sem recalcular `_cct_pro`. O fechamento entrega como referência o vínculo do último dia coberto na janela. Portanto uma divergência de flag entre o motor que calculou uma fatia e o calculador é hipótese verificável no código, mas ocorrência nestes colaboradores é `[nao verificado]`. (`app/ponto/calculador/alimentacao.py:390`; `app/ponto/calculador/alimentacao.py:403`; `app/ponto/calculador/alimentacao.py:404`; `app/ponto/calculador/alimentacao.py:492`; `app/ponto/calculador/alimentacao.py:496`; `app/ponto/calculador/alimentacao.py:499`; `app/ponto/calculador/alimentacao.py:528`; `app/ponto/services/fechamento.py:851`; `app/ponto/services/fechamento.py:852`)

10. **Diferenças candidatas para os dois casos:** motor ancora o recuo em `periodo.saida` e limita por `periodo.entrada`; calculador usa `efetivo_saida/efetivo_entrada`. Uma saída efetiva diferente muda a quantidade de HE que cruza 22h, podendo explicar qualitativamente 1,10 versus 1,00; pode também deslocar o início do trecho em relação às 05h e mudar a prorrogação. Flag diferente pode afetar o turno que atravessa 05h, mas não explica isoladamente um turno que termina por volta de 23h. Pontas efetivas, pausas, flags e subdivisão em períodos destes casos não constam do dado: reconstruir exatamente 5,16/3,10 ou 1,00/1,10 é `[nao verificado]`. (`app/ponto/motor_calculo_v2.py:1015`; `app/ponto/calculador/alimentacao.py:83`; `app/ponto/calculador/alimentacao.py:106`; `app/ponto/motor_calculo_v2.py:440`; `app/ponto/motor_calculo_v2.py:110`)

11. **Dobra do calculador:** classifica pelo dia da jornada recebido, devolvendo todo o trabalhado quando `_feriado_dobra_no_dia(dia)` é verdadeiro. Porém esse total diário não alimenta o split: este continua usando as HE dos períodos do motor. (`app/ponto/calculador/regras.py:348`; `app/ponto/calculador/regras.py:351`; `app/ponto/calculador/regras.py:780`; `app/ponto/calculador/regras.py:784`; `app/ponto/calculador/regras.py:827`; `app/ponto/calculador/regras.py:832`)

Atenção à leitura dos comentários: afirmam que os dois caminhos têm as mesmas pontas e “a troca não muda número”, mas os argumentos executados diferem entre efetivos e crus; citar o código executável nesse ponto. (`app/ponto/calculador/alimentacao.py:52`; `app/ponto/calculador/alimentacao.py:54`; `app/ponto/calculador/alimentacao.py:83`; `app/ponto/motor_calculo_v2.py:1015`)

## 07/11 05:18 · sessao 2ab7f98e2983 · PERTENCA-DO-MINUTO-PELO-TURNO · censo · origem=Code

**A cópia usa três critérios diferentes:** dia do turno/jornada no agrupamento mensal; data civil da entrada em decisões por período e no fechamento; instantes do recorte para o noturno. Assim, um período pode receber a **linha diária correta**, mas carregar uma **rubrica escolhida pelo dia civil errado**. (`app/ponto/motor_calculo_v2.py:2403–2408`; `app/ponto/motor_calculo_v2.py:2809`; `app/ponto/services/fechamento.py:934–949`; `app/ponto/services/dia_pago.py:128–132`)

As referências abaixo são às linhas **da cópia consultada**. Resultados efetivamente pagos em produção, incidência por colaborador e execução dos cenários nesta auditoria: **[não verificado]**.

**O juiz existente e seu alcance.** `core/juizes.py` registra expressamente “a que turno (`data_turno`) pertence a batida?” em `turnos_do_colab`; a pergunta financeira distinta — “que horas do turno que cruza a meia-noite contam como feriado?” — permanece em `SEM_JUIZ`. (`app/core/juizes.py:196`; `app/core/juizes.py:153`; `app/core/juizes.py:329–331`)

| Função/contrato | Entrada, saída e relógio |
|---|---|
| `turnos_do_colab(colab, ini, fim, celulas=None)` | Recebe colaborador e datas; carrega batidas/células/vínculos e devolve turnos com `data_turno` em `[ini,fim]`. O objeto contém `data_turno`, entrada, saída, batidas, aberto e cruzamento de meia-noite. (`app/ponto/turnos.py:41`; `app/ponto/turnos.py:1848–1857`; `app/ponto/turnos.py:1913`) |
| `parear_turnos(...)` | Recebe batidas com timestamp **aware**, tipo E/S, marcos e eventual papel da ata; ordena pelo instante e forma turnos. É o pareador puro usado pelas portas superiores. (`app/ponto/turnos.py:1190–1215`) |
| `turnos_de_batidas(...)` | Porta usada pelo motor com batidas já carregadas: copia objetos, interpreta timestamp naive como horário local e chama `parear_turnos`. (`app/ponto/turnos.py:1515–1533`) |
| `_data_do_turno(entrada, vespera, saida, cont_max_s)` | Parte da data **local** da entrada, ou da saída órfã. Pode devolver a véspera quando os marcos anteriores cruzam meia-noite, a entrada está antes do fim e longe do início, e a saída cabe no envelope anterior. `_mk` publica isso em `Turno.data_turno`. (`app/ponto/turnos.py:283–322`) |
| Relógio da atribuição | Usa timestamps recebidos, `timezone.localtime` e marcos; não recebe `agora`. A pergunta separada sobre turno aberto vivo recebe `agora` e pode usar `timezone.now()`. (`app/ponto/turnos.py:43–46`; `app/ponto/turnos.py:246`; `app/ponto/turnos.py:1998`; `app/ponto/turnos.py:2024`) |

Portanto, existe juiz de **batida → turno/dia**, com associação pelo pareamento. Um contrato geral **instante arbitrário sem batida → turno** é **[não verificado]**; não é o domínio registrado dessa porta. (`app/core/juizes.py:196`; `app/ponto/turnos.py:1193`)

**Censo da pertença no motor.**

| Sítio/função | Qual “dia” usa e origem | Quem chama; resultado e leitor imediato |
|---|---|---|
| `_dia_do_turno` | Lê `Turno.data_turno` e monta `{instante localizado de cada E: data_turno}`. Consulta `_turnos_do_juiz`, que usa `turnos_de_batidas`. (`app/ponto/motor_calculo_v2.py:867–895`) | Chamado pelos cálculos mensais Base, Partido e Comercial; alimenta os agrupadores abaixo. (`app/ponto/motor_calculo_v2.py:2348`; `app/ponto/motor_calculo_v2.py:2922`; `app/ponto/motor_calculo_v2.py:3325`) |
| `MotorBase.calcular_mes`: escolha de `data_jornada` | Primeiro mapa do juiz; sem chave, **data civil local da entrada**. Depois aplica decisão própria: período iniciado de 0 a 6 horas após a saída anterior herda o dia anterior. Só saída real alimenta `_prev_fim`. (`app/ponto/motor_calculo_v2.py:2400–2423`) | O fechamento chega por `apurar_por_fatia → motor.calcular_mes`. `data_jornada` filtra a competência e alimenta FT e mapa por período. (`app/ponto/services/fechamento.py:286–291`; `app/ponto/motor_calculo_v2.py:2429–2432`; `app/ponto/motor_calculo_v2.py:2448–2461`) |
| `MotorBase.calcular_mes`: folga trabalhada | Pergunta `EscalaColaborador.eh_dia_trabalho(data_jornada)`; sem esse ramo, usa `_folga_pelo_dia_previsto(data_jornada, dias_folga)`. Grava somente o booleano `_folga_do_par[instante]` **antes** de chamar `calcular_periodo`; grava o mapa `id(periodo) → data_jornada` depois. (`app/ponto/motor_calculo_v2.py:2445–2461`) | Separa `resultado.periodos_ft` de `resultado.periodos`; a janela lê o memo de folga, e o fechamento lê as listas. (`app/ponto/motor_calculo_v2.py:2468–2494`; `app/ponto/motor_calculo_v2.py:1526`; `app/ponto/services/fechamento.py:933`) |
| `_folga_pelo_dia_previsto` | Recebe o dia escolhido pelo chamador; usa `weekday()` e a previsão injetada. Não consulta o juiz de turno. (`app/ponto/motor_calculo_v2.py:936–954`) | Chamado pelo laço Base para classificação e motivo da FT. O **texto** ainda extrai o dia da semana da entrada civil, podendo discordar do número. (`app/ponto/motor_calculo_v2.py:2453`; `app/ponto/motor_calculo_v2.py:2474–2484`) |
| `_feriado_dobra_no_dia` / `eh_feriado` | Não escolhem a pertença: recebem `dia`. O primeiro aplica `feriado_em_dobra && eh_feriado(dia)`; o segundo lê feriados injetados ou `is_feriado`. O contrato escrito exige o dia de início do plantão. (`app/ponto/motor_calculo_v2.py:928–967`) | Chamados pelos quatro escritores de dobra: 12x36 por período, Comercial por período e os dois agrupadores mensais. Cada chamador escolhe o dia que entrega. (`app/ponto/motor_calculo_v2.py:2809`; `app/ponto/motor_calculo_v2.py:3268`; `app/ponto/motor_calculo_v2.py:3092`; `app/ponto/motor_calculo_v2.py:3350`) |
| `MotorTurnoPartido.calcular_mes`: agrupamento | Reconsulta o juiz; normais usam `dia_do_turno.get(instante, entrada.date())`; FT recebe o mesmo critério. Esse mapa é mesclado em `resultado.dia_da_jornada`; para chaves regravadas, prevalece sobre a herança do Base. (`app/ponto/motor_calculo_v2.py:2922–2959`) | O agrupamento de trabalhado, dobra e HE deste override usa **somente períodos normais**. FT ganha mapa, mas não passa por essas contas. (`app/ponto/motor_calculo_v2.py:2944–2950`; `app/ponto/motor_calculo_v2.py:3081–3084`) |
| Partido: dobra e HE | Feriado/dobra pelo dia agrupado. A dobra já é escrita em **cada período**, com split em cada um. HE diária usa esse dia para previsto e limite; HE semanal usa esse dia para semana, exclusão de feriado e escolha da última jornada. (`app/ponto/motor_calculo_v2.py:3081–3114`; `app/ponto/motor_calculo_v2.py:3128–3160`; `app/ponto/motor_calculo_v2.py:3169–3191`) | Chamado pelo cálculo mensal; escreve campos dos períodos que as propriedades de `ResultadoMes`, o fechamento e DiaPago somam. (`app/ponto/motor_calculo_v2.py:324–344`; `app/ponto/services/fechamento.py:919–924`; `app/ponto/services/dia_pago.py:180–184`) |
| `MotorComercial.calcular_mes` | Reconsulta o juiz, com fallback civil, para **normais**; zera HE individual e recalcula dobra/HE pelo dia agrupado. FT fica fora desse reset. (`app/ponto/motor_calculo_v2.py:3325–3350`) | Dobra em cada período + split; HE diária pelos marcos de `dia`, lançada no último período. Leitores: mesmas propriedades, fechamento e DiaPago. (`app/ponto/motor_calculo_v2.py:3354–3361`; `app/ponto/motor_calculo_v2.py:3396–3423`; `app/ponto/services/fechamento.py:919–924`; `app/ponto/services/dia_pago.py:180–184`) |
| `_aplicar_teto_pontualidade` | Recebe mapa `id(periodo) → dia`; agrupa por ele e **expõe/mescla** `resultado.dia_da_jornada`. Não reconstrói o dia nesse agrupamento. (`app/ponto/motor_calculo_v2.py:1073–1084`) | Chamado ao final dos três cálculos mensais; o fechamento mescla os mapas das fatias e passa o resultado à lavratura. (`app/ponto/motor_calculo_v2.py:2718`; `app/ponto/motor_calculo_v2.py:3195`; `app/ponto/motor_calculo_v2.py:3508`; `app/ponto/services/fechamento.py:311–316`; `app/ponto/services/fechamento.py:1217`) |

**Decisões por período, recorte e noturno — inclusive os sítios mínimos pedidos.**

| Sítio/função | Critério efetivo na cópia; chamada e resultado |
|---|---|
| `Motor12x36.calcular_periodo` | `e` vem da **entrada efetiva** da janela. Feriado e dobra usam **`e.date()`**, sem ler `data_jornada`. HE comum consulta `_jornada_do_dia(e)` e o limite com `e`. O laço Base chama o método antes de associar o período ao mapa. (`app/ponto/motor_calculo_v2.py:2759–2760`; `app/ponto/motor_calculo_v2.py:2796–2841`; `app/ponto/motor_calculo_v2.py:2458–2460`) |
| `MotorComercial.calcular_periodo` | Também decide feriado/dobra por **`e.date()`**, da entrada efetiva. Escreve HE100/feriado e chama split. O mensal corrige os normais por agrupamento; os FT preservam esse julgamento individual. (`app/ponto/motor_calculo_v2.py:3224–3238`; `app/ponto/motor_calculo_v2.py:3264–3294`; `app/ponto/motor_calculo_v2.py:3327–3338`) |
| `_previsto_do_dia`, `_jornada_do_dia`, `_intervalo_descontado_no_dia` | Recebendo datetime, extraem **`quando.date()`**; não consultam mapa de turno. Recebendo date, usam a própria data. Por isso o ramo individual e o mensal podem perguntar por dias diferentes. (`app/ponto/motor_calculo_v2.py:1227–1253`; `app/ponto/motor_calculo_v2.py:1273–1277`; `app/ponto/motor_calculo_v2.py:1313`; `app/ponto/motor_calculo_v2.py:2835`; `app/ponto/motor_calculo_v2.py:3128`) |
| `_janela_do_dia` / `_janela_do_par` | Critério **misto**: folga pelo memo por instante; depois **`entrada.date()`** para empresa, marcos, extra declarada e autorização. A exceção L-084 do dia consulta `data_turno` no mapa e seus marcos. Os clipadores recebem essa decisão e produzem o par efetivo usado no trabalhado/noturno. (`app/ponto/motor_calculo_v2.py:1526–1536`; `app/ponto/motor_calculo_v2.py:1582–1594`; `app/ponto/motor_calculo_v2.py:1646–1653`; `app/ponto/motor_calculo_v2.py:1760–1763`) |
| `_dia_sem_previsto_fora_da_janela` | Recebe **`data_jornada`** do Base. Usa esse dia para feriado, empresa, previsto, autorização e escala certa; pode zerar trabalhado, noturno e HE e colapsar o segmento efetivo. Não deriva outro dia. (`app/ponto/motor_calculo_v2.py:2461`; `app/ponto/motor_calculo_v2.py:1448–1487`) |
| `minutos_noturnos`: janela, teto e prorrogação | Usa **instantes localizados**, percorrendo datas civis entre início e fim; cria 00–05 e 22–24. Recortes acima de 26h retornam zero. Havendo interseção com 00–05, a prorrogação vai até **`fim`**, sem teto em `s2_ini`; portanto os segmentos podem sobrepor. Não consulta juiz de turno. (`app/ponto/motor_calculo_v2.py:89–118`) |
| `minutos_noturnos_dos_intervalos` | Mede pausas dentro do recorte recebido: união de intervalos, cortes nas horas locais e presença noturna segundo prefixos de `minutos_noturnos`; limita o desconto ao noturno total. É chamado pelo total noturno e pelo split. (`app/ponto/motor_calculo_v2.py:162–196`; `app/ponto/motor_calculo_v2.py:985`; `app/ponto/motor_calculo_v2.py:442`) |
| `_inicio_do_trecho_extra` → `he_noturna` | Recua do **instante final** pela quantidade de minutos extras trabalhados, descontando pausas. Depois mede **`inicio_extra → fim` isoladamente**: esse trecho decide sua própria prorrogação, sem herdar a janela do par inteiro. Reparte o noturno entre HE50/100 proporcionalmente. (`app/ponto/motor_calculo_v2.py:392–406`; `app/ponto/motor_calculo_v2.py:430–451`) |
| `_aplicar_he_noturna`: split do par | Passa **`periodo.entrada/saida` brutos**, não `efetivo_entrada/saida`; lê intervalos e escreve os subconjuntos noturnos. Chamado pelos cálculos individuais 12x36/Comercial e pelas dobras/HE mensais Partido/Comercial. (`app/ponto/motor_calculo_v2.py:1012–1022`; `app/ponto/motor_calculo_v2.py:2855`; `app/ponto/motor_calculo_v2.py:3294`; `app/ponto/motor_calculo_v2.py:3104`; `app/ponto/motor_calculo_v2.py:3160`; `app/ponto/motor_calculo_v2.py:3191`; `app/ponto/motor_calculo_v2.py:3361`; `app/ponto/motor_calculo_v2.py:3423`) |
| `calcular_noturnas` / `_escrever_noturnas` | O total noturno usa o **par efetivo recebido**, desconta intervalos e aplica o fator. Os três cálculos de período escrevem por essa porta; isso difere do par bruto entregue ao split. (`app/ponto/motor_calculo_v2.py:983–1005`; `app/ponto/motor_calculo_v2.py:2792`; `app/ponto/motor_calculo_v2.py:2877–2897`; `app/ponto/motor_calculo_v2.py:3260`) |

Nos marcadores aproximados do pedido, a cópia contém: prorrogação/teto em `minutos_noturnos:93–112`, medição da cauda em `he_noturna:440–443`, split bruto em `_aplicar_he_noturna:1012–1018`, classificação FT em `2448–2457` e dobra mensal do partido em `3092–3104`. (`app/ponto/motor_calculo_v2.py:93–112`; `app/ponto/motor_calculo_v2.py:440–443`; `app/ponto/motor_calculo_v2.py:1012–1018`; `app/ponto/motor_calculo_v2.py:2448–2457`; `app/ponto/motor_calculo_v2.py:3092–3104`)

O helper **`_dia_da_jornada_do_par` do patch retirado não integra o caminho presente**: entre o predicado de dobra e o de folga não há esse método; os dois escritores individuais continuam passando `e.date()`. O armazenamento por instante feito pelo Base é `_folga_do_par`, um booleano, enquanto o dia por período é gravado depois da chamada. (`app/ponto/motor_calculo_v2.py:914–936`; `app/ponto/motor_calculo_v2.py:2809`; `app/ponto/motor_calculo_v2.py:3268`; `app/ponto/motor_calculo_v2.py:2457–2460`)

Há ainda duas atribuições complementares: o crédito de dia aberto agrupa pelo mapa e pelos turnos do juiz, mas transfere dos períodos calculados apenas trabalhado/fora da janela; banco e reflexo de DSR misturam agrupamento por jornada com seleções de extras por entrada civil. (`app/ponto/motor_calculo_v2.py:2514–2560`; `app/ponto/motor_calculo_v2.py:2577–2579`; `app/ponto/motor_calculo_v2.py:2636–2644`; `app/ponto/motor_calculo_v2.py:3442–3447`; `app/ponto/motor_calculo_v2.py:3465`; `app/ponto/motor_calculo_v2.py:3487–3495`)

**Fechamento e DiaPago.**

| Sítio/função | Dia usado, chamador e destino |
|---|---|
| `apurar_por_fatia` | Seleciona batidas por **data civil local**, lida de `turnos.data_local`, com contexto de ±1 dia. Chama o motor e mescla `dia_da_jornada` preservando os objetos/períodos. É chamado pelo recalculador e pelos leitores da autoridade do período. (`app/ponto/services/fechamento.py:203–204`; `app/ponto/services/fechamento.py:244–262`; `app/ponto/services/fechamento.py:286–316`; `app/ponto/services/fechamento.py:754–756`) |
| `recalcular_fechamento_mes`: filtro `_ft_certa` | Pergunta `escala_certa_no_dia` pela **data civil local de `p.entrada`**, sem mapa de jornada. A decisão determina se há adicional de FT ou apenas hora trabalhada. (`app/ponto/services/fechamento.py:928–935`; `app/ponto/services/fechamento.py:965–985`) |
| Mesmo recalculador: promoção `_ft_feriado` | Novamente usa **data civil local da entrada** para obter vínculo e perguntar `feriado_vira_folga`. Promove toda a hora a HE100/feriado; apenas lê o split noturno já escrito pelo motor. (`app/ponto/services/fechamento.py:944–964`) |
| `dia_pago._dia_de(p,mapa)` | Sem entrada: `None`. Com mapa e valor para `id(p)`: dia do mapa. Sem mapa, mapa vazio, chave ausente ou valor `None`: **data civil local da entrada**. Não chama diretamente o juiz. (`app/ponto/services/dia_pago.py:109–132`) |
| `dia_pago.lavrar` | Chama `_dia_de` nos quatro conjuntos: normais, FT certa, FT feriado e FT sem escala. Recebe listas já classificadas pelo fechamento **e** `resultado.dia_da_jornada`; grava a linha em `DiaPago.data`. Logo, mudar a chave diária aqui não reclassifica a rubrica anteriormente escolhida pelo civil. (`app/ponto/services/dia_pago.py:173–184`; `app/ponto/services/dia_pago.py:234–265`; `app/ponto/services/dia_pago.py:345–360`; `app/ponto/services/fechamento.py:1201–1217`) |
| O85, períodos sem vínculo | O fechamento seleciona dias órfãos por batidas civis e atribui cada período órfão à **data civil local da entrada**, sem ler seu mapa; entrega minutos já datados à lavratura. (`app/ponto/services/fechamento.py:793–796`; `app/ponto/services/fechamento.py:838–843`; `app/ponto/services/fechamento.py:1204`; `app/ponto/services/dia_pago.py:269–272`) |

**Quem lê esses resultados.**

| Leitor | Caminho e eventual nova decisão de dia |
|---|---|
| Fechamento | Soma propriedades de `ResultadoMes`: HE somente dos normais; noturno dos normais + FT. Depois aplica sua partição FT civil. (`app/ponto/motor_calculo_v2.py:321–344`; `app/ponto/services/fechamento.py:918–985`) |
| Espelho — dinheiro diário | Carrega `DiaPago` por `data` e expõe `pago_h`/rubricas. Essas rubricas já chegam com a pertença e classificação descritas acima. (`app/ponto/services/dia_pago.py:854–856`; `app/ponto/services/espelho.py:406–410`; `app/ponto/services/espelho.py:805–813`) |
| Espelho — geometria e resumo | Batidas/cards usam o juiz de batidas. Entretanto, `somar_periodos(..., dentro=...)` ainda recorta competência pela **entrada civil**, sem mapa de jornada. (`app/ponto/services/espelho.py:450–460`; `app/ponto/services/espelho.py:574–577`; `app/ponto/services/espelho.py:82–86`) |
| `porta_export` | `dias_fora_do_dia_pago` pede o resultado à autoridade, mas reagrupa trabalhado por **entrada civil**, comparando com `DiaPago.data`. Pode acusar divergência diária de uma cauda que DiaPago colocou no dia anterior. (`app/folha/porta_export.py:74–78`; `app/folha/porta_export.py:101–118`) |
| PDF do espelho | Chama `espelho_do_colab`; desenha `pago_h` e rubricas diárias. O total passa por `folha_manda`, que consulta o fechamento/soma lavrada. (`app/relatorios/pdf_espelho.py:596–602`; `app/relatorios/pdf_espelho.py:628`; `app/relatorios/pdf_espelho.py:901–914`; `app/relatorios/cartao_pela_celula.py:323–342`) |
| Exportação e PDF do TXT | Eventos leem os campos do fechamento; splits HE noturna não são eventos adicionais separados. O PDF do TXT traduz o conteúdo exportado e suas datas, sem atribuir novamente minutos a turnos. (`app/folha/export.py:420–447`; `app/folha/pdf_export.py:54–65`; `app/folha/pdf_export.py:111–137`; `app/folha/pdf_export.py:173–185`) |

**Contra-exemplos e sítios que discordam.** Os casos abaixo são fixtures/assertivas da cópia ou deduções estáticas explicitadas; sua execução nesta auditoria é **[não verificado]**.

| Caso | Pertença e sítio que erraria/divergiria |
|---|---|
| Madrugada sem entrada noturna anterior: escala 21–05, E **02/09 00:57**, saídas 02:29/05:00 | O teste põe batidas e card em **01/09**. Os escritores individuais por `e.date()`, o fechamento FT civil e o fallback de `_dia_de` perguntariam por **02/09**. (`app/ponto/tests/test_espelho_tela_le_o_juiz_do_dia.py:118–129`; `app/ponto/motor_calculo_v2.py:2796–2809`; `app/ponto/motor_calculo_v2.py:3264–3268`; `app/ponto/services/fechamento.py:935–949`; `app/ponto/services/dia_pago.py:132`) |
| 12x36: d19–d+1 04:30 / d+1 05:30–07; só **d+1 feriado** | Base herda **d**, mas `Motor12x36.calcular_periodo` dobra a cauda pelo civil **d+1**. O teste caracterizador espera **90 min** indevidos segundo a regra do teste vizinho, que exige zero e está `expectedFailure`. (`app/ponto/motor_calculo_v2.py:2405–2408`; `app/ponto/motor_calculo_v2.py:2809`; `app/ponto/tests/test_o280_bateria_da_lei.py:1549–1577`) |
| Mesmo 12x36, só **d feriado** | O caracterizador espera **570 min** de dobra, deixando a cauda de 90 fora. O dia herdado é d, mas o escritor individual usa d+1. O noturno da cauda também fica zero porque é medido separadamente. A exigência de prorrogação entre esses pares está expressamente condicionada à pergunta aberta do F7g; não cabe tratá-la como regra já resolvida. (`app/ponto/tests/test_o280_bateria_da_lei.py:1579–1626`; `app/ponto/motor_calculo_v2.py:2792–2809`) |
| Partido 21–00 / 03–07 | O segundo par entra civilmente em d+1, mas pertence a d. A dobra dos **normais já usa o juiz e alcança cada par**. Se os pares estiverem em FT, o fechamento civil pode promover o segundo ao feriado errado, ou deixar de promovê-lo quando o feriado é d; o filtro de escala certa pode igualmente consultar o dia errado. Essa última conclusão é dedução do fluxo. (`app/ponto/tests/test_o280_bateria_da_lei.py:1758–1781`; `app/ponto/motor_calculo_v2.py:2942–2955`; `app/ponto/motor_calculo_v2.py:3097–3104`; `app/ponto/services/fechamento.py:934–950`) |
| Partido com vão **7h**: d19–23 / d+1 06–10

[truncado em 20000 de 21998 caracteres]

## 07/11 05:16 · sessao 93764bc03c64 · - · - · origem=Ronald (tmux)

Censo do motor confirmado por leitura; numeração real desta cópia difere do prompt. Os pontos equivalentes são `he_noturna:440`, `_aplicar_he_noturna:1015`, FT `2448`, dobra partido `3092`. [app/ponto/motor_calculo_v2.py:440](/home/ronald/codex_ws/a23831c32f40a9291b760e99734b9854c246340e/app/ponto/motor_calculo_v2.py:440)

| Sítio | Dia/instante usado e origem | Chamador/leitor imediato |
|---|---|---|
| `localizar` | Naive interpretado em `settings.TIME_ZONE`; aware convertido ao mesmo fuso. | Todo recorte do motor. `motor_calculo_v2.py:28,73-79`. |
| `minutos_noturnos` | Datas civis locais de `inicio.date()` até `fim.date()`; segmentos 00–05 e 22–24; >26h zera; prorrogação vai até `fim`, sem teto às 22h. | `calcular_noturnas`, desconto de intervalos e `he_noturna`. `motor_calculo_v2.py:82-118,191-196,983-985,441`. |
| `minutos_noturnos_dos_intervalos` | Janela do recorte recebido; união de pausas; cortes nas horas cheias locais; presença medida por diferença de prefixos; teto do total. | Total noturno e split; nenhuma consulta a `data_turno`. `motor_calculo_v2.py:162-196,442,1914`. |
| `_inicio_do_trecho_extra` / `he_noturna` | Cauda de HE recuada desde o fim por minutos trabalhados; mede **a cauda isoladamente**, portanto a prorrogação depende do toque dela em 00–05. | `_aplicar_he_noturna`; escreve retorno proporcional HE50/100. `motor_calculo_v2.py:392-406,430-451`. |
| `_aplicar_he_noturna` | Passa `periodo.entrada/saida` **crus**, embora existam carimbos efetivos. | 12x36 período `2855`, comercial período `3294`, partido mês `3104,3160,3191`, comercial mês `3361,3423`; escreve campos split `1019-1022`. `motor_calculo_v2.py:1007-1022`. |
| `_escrever_noturnas` | Usa e/s recebidos, efetivos nos três ramos; desconto usa a mesma janela recebida. | Escreve minutos relógio/legais; leitores `ResultadoMes.horas_noturnas` e serviços. `motor_calculo_v2.py:983-987,1003-1005,2792,2896,3260,321`. |
| `_dia_do_turno` | Mapa `{instante local de cada E em t.batidas: t.data_turno}`; `_turnos_do_juiz` chama `turnos_de_batidas` com marcos DNA e cópias. | Base e overrides mensais. `motor_calculo_v2.py:867-895`. |
| `MotorBase.calcular_mes` | Primeiro mapa do juiz, fallback civil; depois **sobrepõe** com dia anterior se entrada ocorre 0–6h após saída anterior fechada. | Competência, FT, bloqueio HE e mapa final. `motor_calculo_v2.py:2346-2349,2396-2408,2423-2432`. |
| FT no Base | `eh_dia_trabalho(data_jornada)` ou `_folga_pelo_dia_previsto(data_jornada)`; memo por instante da entrada. | Janela lê memo; separa `periodos_ft`/`periodos`. Rótulo semanal, contudo, lê weekday civil. `motor_calculo_v2.py:2448-2461,2468-2494`. |
| `_dia_sem_previsto_fora_da_janela` | Recebe `data_jornada`; usa esse dia para feriado, empresa, previsto, decisão e escala certa; folga vem do memo por instante. | Base chama `2461`; zera rubricas/colapsa efetivo. `motor_calculo_v2.py:1448-1483`. |
| `_janela_do_dia` | Mistura: folga por memo do instante; empresa/marcos/extra/decisão pela **data civil da entrada**; exceção L084 consulta mapa `data_turno` e marcos daquele dia. | `_janela_do_par` → clipadores → todos os cálculos de período. `motor_calculo_v2.py:1526-1536,1582-1594,1646-1653,1760-1763`. |
| 12x36 `calcular_periodo` | Feriado/dobra por `e.date()`, sendo **e efetivo**, após janela; HE comum usa `_jornada_do_dia(e)`. | Base chama antes de registrar mapa do período; herdeiros reutilizam. Campos dobra/split lidos pelas propriedades mensais. `motor_calculo_v2.py:2759-2765,2796-2813,2835-2840,2855,2458-2460,3535,3547`. |
| Comercial `calcular_periodo` | Mesmo `e.date()` efetivo para feriado/dobra; HE comum também recebe e. | Base; mês recalcula apenas períodos normais, portanto FT conserva decisão por período. `motor_calculo_v2.py:3224-3238,3264-3272,3284-3294,3327-3338`. |
| Partido `calcular_mes` | Consulta juiz novamente; mapa normais e FT usa juiz ou data civil; pode substituir herança de 6h do Base. | Publica `resultado.dia_da_jornada`; agrega apenas normais; dobra por esse dia em **cada período** + split. FT só recebe mapa, sem bloco de dobra/split. `motor_calculo_v2.py:2921-2959,3081-3104`. |
| HE diária/semanal partido | Diária por `dia_da_jornada`; semanal seg–dom pelo mesmo dia, exclui feriados desse dia e lança HE no último período da última jornada. | Split do último período. `motor_calculo_v2.py:3128-3160,3169-3191`. |
| Comercial `calcular_mes` | Mapa juiz ou civil apenas normais; dobra diária por esse mapa; HE diária consulta marcos desse dia. | Dobra em cada período + split; publica mapa no teto. `motor_calculo_v2.py:3325-3338,3342-3361,3396-3423,3508`. |
| `_previsto_do_dia`, `_jornada_do_dia`, `_intervalo_descontado_no_dia` | Extraem `.date()` do argumento datetime; **não** traduzem pelo mapa do juiz. | Chamadores por período passam e civil efetivo; agregados passam dia já julgado. `motor_calculo_v2.py:1227-1253,1273-1277,1313-1327,1803,2835,3128`. |
| Reflexo HE/DSR Base | Presença usa mapa da jornada com fallback civil; **extras e contagem de dias do reflexo** usam civil da entrada. | `resultado.horas_reflexo_dsr`. `motor_calculo_v2.py:2577-2579,2636-2653`. |
| Banco/DSR comercial | Banco soma trabalho pelo mapa, mas subtrai extras por civil; DSR override usa presença, extras e quantidade de dias civis. | Saldo/reflexo mensais. `motor_calculo_v2.py:3442-3455,3465,3487-3501`. |
| Crédito de aberto | Agrupa abertos pelo mapa da jornada e seleciona turnos com `t.data_turno==dia`; calcula peças fechadas, mas transfere apenas trabalho/fora da janela. | Primeiro aberto do dia; noturno/HE calculados na peça não são transferidos. `motor_calculo_v2.py:2514-2524,2553-2560`. |
| Exposição do mapa | Mescla `{id(periodo): dia}` em `resultado.dia_da_jornada`. | Base `2718`, partido `3195`, comercial `3508`; leitores externos recebem mapa. `motor_calculo_v2.py:1073-1075`. |

`_feriado_dobra_no_dia` não escolhe o dia: faz `feriado_em_dobra and eh_feriado(dia)`; seu contrato exige dia de início, mas os ramos por período passam `e.date()`. `_folga_pelo_dia_previsto` usa weekday e previsão injetada. `motor_calculo_v2.py:928-954,2796-2809,3264-3268`.

`_dia_da_jornada_do_par` não está no HEAD: a classe passa diretamente de `_feriado_dobra_no_dia` a `_folga_pelo_dia_previsto`; não há o método nem os consumidores introduzidos pelo patch. `motor_calculo_v2.py:934-936,2796-2809,3264-3268`.

Contraexemplos úteis com lastro no HEAD:

- N12 `d19:00–d+1 04:30 / d+1 05:30–07:00`: cauda herda d no Base, mas dobra por `e.date()`; teste F10d caracteriza 90 minutos dobrados no feriado d+1, e F10e está `expectedFailure` esperando zero. `test_o280_bateria_da_lei.py:1533-1577`; `motor_calculo_v2.py:2405-2408,2796-2809`. [nao verificado]
- Mesmas batidas com feriado d: caracteriza 570 minutos de dobra, cauda noturna/split zero; expectativa de prorrogação pela jornada permanece pinada e explicitamente depende da pergunta F7g. `test_o280_bateria_da_lei.py:1579-1626`.
- Partido `21–00 / 03–07`: HEAD já distribui dobra por cada par normal; não atribuir ao HEAD o defeito antigo de concentrar tudo no último. `test_o280_bateria_da_lei.py:1759-1781`; `motor_calculo_v2.py:3097-3104`.
- Entrada civil `21/09 00:00` para marco `20/09 23:50`: comentário registra juiz respondendo dia 20; prova atual do mecanismo é mapa antes do filtro de competência, mas números de produção citados no comentário não foram revalidados. `motor_calculo_v2.py:2388-2403,2429-2432`.

## 07/11 05:15 · sessao 0bdb7021b06a · - · - · origem=Ronald (tmux)

O juiz existe para **batidas/turnos formados**, e seu registro distingue isso da pergunta financeira “que horas do turno cross-meia-noite contam como feriado?”, ainda listada em `SEM_JUIZ`. [`app/core/juizes.py:196`; `app/core/juizes.py:153`; `app/core/juizes.py:330`]

- `turnos_do_colab(colab, ini, fim, celulas=None)` retorna os `Turno` cujo `data_turno` pertence a `[ini,fim]`; `Turno` contém `data_turno, entrada, saida, batidas, aberto, cross_meianoite`. Carrega batidas não retratadas, células, vínculos e DNA, com pad de dois dias atrás e um à frente. [`app/ponto/turnos.py:41`; `app/ponto/turnos.py:1845`; `app/ponto/turnos.py:1848`; `app/ponto/turnos.py:1861`; `app/ponto/turnos.py:1882`; `app/ponto/turnos.py:1913`]
- O corpo agrupa por vínculo dono de cada dia, pareia com marcos e papel da ata, e filtra pelo `data_turno` produzido. [`app/ponto/turnos.py:1937`; `app/ponto/turnos.py:1954`; `app/ponto/turnos.py:1970`; `app/ponto/turnos.py:1990`]
- `parear_turnos` recebe objetos com `.timestamp` **aware** e `.tipo` E/S; sua ordenação usa o instante; marcos de intervalo e papel da ata influem na geometria. É puro, sem DB. [`app/ponto/turnos.py:1190`; `app/ponto/turnos.py:1193`; `app/ponto/turnos.py:1199`; `app/ponto/turnos.py:1215`]
- `turnos_de_batidas(batidas,tipo_escala,marcos_por_dia=None)` é a receita sem DB usada pelo motor: copia batidas, torna timestamps naive aware, marca ecos e chama o mesmo `parear_turnos`. [`app/ponto/turnos.py:1515`; `app/ponto/turnos.py:1523`; `app/ponto/turnos.py:1528`; `app/ponto/turnos.py:1531`]
- **Relógio:** `data_local` usa `timezone.localtime(ts).date()`; a atribuição de `data_turno` usa timestamps e marcos recebidos, sem argumento `agora`. `turno_aberto_de`, pergunta diferente sobre presença viva, recebe `agora` e usa `timezone.now()` por padrão; a expiração é fim previsto de `data_turno` + margem de quatro horas. [`app/ponto/turnos.py:43`; `app/ponto/turnos.py:246`; `app/ponto/turnos.py:1998`; `app/ponto/turnos.py:2024`; `app/ponto/turnos.py:2281`]

`_data_do_turno(entrada,vespera,saida=None,cont_max_s=14h)` é o atribuidor interno. Começa na data local da entrada (ou saída órfã); sem marcos válidos da véspera ou sem cruzar meia-noite, mantém essa data. Havendo véspera noturna, entrada antes do fim e distante mais de 90 min do início pode ser atribuída à véspera, desde que a saída caiba no envelope da jornada anterior. O envelope parte do **início previsto da véspera**, com duração prevista + quatro horas. [`app/ponto/turnos.py:53`; `app/ponto/turnos.py:71`; `app/ponto/turnos.py:214`; `app/ponto/turnos.py:227`; `app/ponto/turnos.py:283`; `app/ponto/turnos.py:299`]

`_vespera` impede usar a véspera quando já houve turno terminado nela e procura os marcos do DNA daquela data, ou do template quando não há mapa. `_mk` publica a decisão no `Turno`. [`app/ponto/turnos.py:1251`; `app/ponto/turnos.py:1293`; `app/ponto/turnos.py:1297`; `app/ponto/turnos.py:310`]

**Não há nesse contrato uma API genérica `instante arbitrário → turno/data_turno`:** o domínio declarado é a batida, e o resultado é produzido pelo pareamento; a pertença de instante sem batida é [não verificado]. [`app/core/juizes.py:196`; `app/ponto/turnos.py:1193`; `app/ponto/turnos.py:1848`]

Consulta pelo motor:

| Sítio | Como consulta |
|---|---|
| `MotorBase._turnos_do_juiz` | Chama `turnos_de_batidas`, com template, intervalo2 e DNA diário; declara que período/minutos continuam sendo os do motor. `app/ponto/motor_calculo_v2.py:867` |
| `MotorBase._dia_do_turno` | Produz `{localizar(timestamp de cada E): data_turno}` a partir de todas as batidas E dos turnos. `app/ponto/motor_calculo_v2.py:884` |
| `MotorBase.calcular_mes` | Guarda `_dia_turno_juiz_calc`, consulta o mapa, usa data civil quando ausente e depois **sobrepõe herança própria**: gap de 0–6 h após saída do período anterior herda `_prev_dj`. `app/ponto/motor_calculo_v2.py:2396`; `app/ponto/motor_calculo_v2.py:2400`; `app/ponto/motor_calculo_v2.py:2423` |
| Folga nesse laço | Pergunta `eh_dia_trabalho(data_jornada)` ou `_folga_pelo_dia_previsto(data_jornada)`, grava `_folga_do_par[instante]`, chama `calcular_periodo` e associa o período a `data_jornada`. `app/ponto/motor_calculo_v2.py:2445`; `app/ponto/motor_calculo_v2.py:2457` |
| `MotorTurnoPartido.calcular_mes` | Reconsulta juiz e agrupa normais e FT pelo mapa, com fallback civil; comentário explicita que as contas desse método não leem o mapa dos FT. `app/ponto/motor_calculo_v2.py:2921`; `app/ponto/motor_calculo_v2.py:2941`; `app/ponto/motor_calculo_v2.py:2946` |
| `MotorComercial.calcular_mes` | Reconsulta `_dia_do_turno` e agrupa normais pelo dia do juiz, com fallback civil. `app/ponto/motor_calculo_v2.py:3325` |
| `Motor12x36.calcular_periodo` | Independente do mapa: passa `e.date()` a `eh_feriado` e `_feriado_dobra_no_dia`; `e` vem da janela efetiva do par. `app/ponto/motor_calculo_v2.py:2759`; `app/ponto/motor_calculo_v2.py:2796`; `app/ponto/motor_calculo_v2.py:2809` |
| Fechamento FT | Independente do mapa: filtro de escala certa e promoção a feriado usam `timezone.localtime(p.entrada).date()`. `app/ponto/services/fechamento.py:934`; `app/ponto/services/fechamento.py:947` |

`_dia_da_jornada_do_par` não foi encontrado no HEAD pesquisado em `app/ponto` e `app/core`; logo o contrato e os efeitos descritos para esse método no patch fornecido não devem ser atribuídos ao HEAD. [não verificado: prova negativa por busca; o corpo imediatamente após `_feriado_dobra_no_dia` pode ser citado pelo agente do motor]

Contra-exemplos presentes em testes do HEAD, **sem executá-los**: [nao verificado]

1. **Entrada de madrugada sem entrada da noite:** cadastro 21–05; E 02/09 00:57, S 02:29 e S 05:00 pertencem ao turno de 01/09 segundo o teste. Quem usar entrada civil para feriado/folga verá 02/09, enquanto o juiz entrega 01/09. [`app/ponto/tests/test_espelho_tela_le_o_juiz_do_dia.py:118`; `app/ponto/tests/test_espelho_tela_le_o_juiz_do_dia.py:122`; `app/ponto/tests/test_espelho_tela_le_o_juiz_do_dia.py:126`; `app/ponto/motor_calculo_v2.py:2796`; `app/ponto/services/fechamento.py:947`]
2. **12x36 noturno com dois pares:** d19–d+1 04:30 / d+1 05:30–07; laço herda d, mas ramo por período usa d+1 na cauda. O teste caracterizador espera 90 min de dobra quando só d+1 é feriado, e o teste da regra “zero” está `expectedFailure`. [`app/ponto/tests/test_o280_bateria_da_lei.py:1543`; `app/ponto/tests/test_o280_bateria_da_lei.py:1549`; `app/ponto/tests/test_o280_bateria_da_lei.py:1561`; `app/ponto/tests/test_o280_bateria_da_lei.py:1564`; `app/ponto/motor_calculo_v2.py:2407`; `app/ponto/motor_calculo_v2.py:2809`]
3. **Inverso do anterior:** só d é feriado; o teste caracterizador registra 570 min dobrados, cauda de 90 min sem dobra, sem noturno e sem split. O teste que exige 660 de dobra e 480 de noturno é `expectedFailure`, condicionado à questão da prorrogação entre pares. [`app/ponto/tests/test_o280_bateria_da_lei.py:1581`; `app/ponto/tests/test_o280_bateria_da_lei.py:1591`; `app/ponto/tests/test_o280_bateria_da_lei.py:1602`; `app/ponto/tests/test_o280_bateria_da_lei.py:1611`; `app/ponto/tests/test_o280_bateria_da_lei.py:1624`]
4. **Partido com vão de sete horas:** d19–23 / d+1 06–10:30; marcos absorvem o vão, uma jornada de 510 min produz 30 de HE. Derivar pelo civil ou somente pelo limite de seis horas separaria o segundo bloco; o agrupador do partido já consulta o juiz. [`app/ponto/tests/test_motor_partido_dia_do_turno.py:10`; `app/ponto/tests/test_motor_partido_dia_do_turno.py:84`; `app/ponto/motor_calculo_v2.py:2921`; `app/ponto/motor_calculo_v2.py:2942`]
5. **Comercial com intervalo2 após meia-noite:** d18/21/22/d+1 00:30/01/04:30; o juiz põe a volta de 01h em d. Testes exigem 60 de HE, nenhuma dobra se feriado só d+1 e dobra integral se feriado d. O agrupador mensal usa o juiz; entrada civil isolada produziria outra atribuição. [`app/ponto/tests/test_motor_comercial_dia_do_turno.py:3`; `app/ponto/tests/test_motor_comercial_dia_do_turno.py:56`; `app/ponto/tests/test_motor_comercial_dia_do_turno.py:68`; `app/ponto/tests/test_motor_comercial_dia_do_turno.py:72`; `app/ponto/tests/test_motor_comercial_dia_do_turno.py:76`; `app/ponto/motor_calculo_v2.py:3325`]

Para apresentação de marcação há ainda `dia_das_batidas`: `{batida.pk:date}`, usando `data_turno` quando algum turno reclama a batida e data local quando nenhum reclama; não equivale a decidir feriado ou remuneração de minuto. [`app/ponto/turnos.py:2874`; `app/ponto/turnos.py:2911`]

## 07/11 05:15 · sessao bd8197184a8b · - · - · origem=Ronald (tmux)

Caminho dos leitores, pelo código da cópia:

- `recalcular_fechamento_mes` copia os totais do `resultado` para `FechamentoMensal`: noturnas, HE50/100, feriado e subconjuntos noturnos em `app/ponto/services/fechamento.py:918-924`. A partição dos `periodos_ft` ainda pergunta por **data civil local da entrada**: `_certa(...timezone.localtime(p.entrada).date())` em `:934-935`, e `escala_vigente`/`feriado_vira_folga` recebem esse mesmo civil em `:946-950`. A promoção soma toda a hora em HE100/feriado e apenas lê o subconjunto noturno já escrito no período em `:954-964`; FT restante vai para `horas_folga_trabalhada` em `:965-966`. As mesmas listas e o mapa `resultado.dia_da_jornada` são entregues a `dia_pago.lavrar` em `:1201-1217`.

- `dia_pago._dia_de(p,mapa)` devolve `None` sem entrada; havendo mapa e chave `id(p)`, lê o dia do mapa; caso contrário — inclusive mapa vazio ou chave ausente — deriva `timezone.localtime(p.entrada).date()`. **Não consulta diretamente juiz de turno**, apenas recebe o mapa do motor. `app/ponto/services/dia_pago.py:109-132`. `lavrar` usa essa função nos quatro ramos: períodos normais `:173-184`, FT certa `:234-240`, FT feriado `:244-253`, FT sem escala `:259-265`. Portanto a linha diária usa dia do motor, mas pode receber uma rubrica escolhida pelo civil no fechamento. `app/ponto/services/fechamento.py:934-950`; `app/ponto/services/dia_pago.py:235-252`.

- No espelho, o dinheiro diário é lido por `linhas_do_periodo` (consulta `{DiaPago.data: objeto}` em `app/ponto/services/dia_pago.py:833-856`), carregado em `app/ponto/services/espelho.py:406-410`, e exposto como `dia_pago`, `pago_h` e `rubricas` em `:805-813`. `pago_do_dia` soma normais + FT + `detalhe['folga_feriado_min']/60` em `app/ponto/services/dia_pago.py:890-898`; `rubricas_do_dia` lê HE50/100 e noturnas em `:914-928`. A geometria dos cards e batidas usa outro caminho: `dia_das_batidas`, ponte instante→pk→dia em `app/ponto/services/espelho.py:450-460`; batidas são agrupadas pelo mesmo mapa em `:574-577`.

- Há um derivador civil adicional no resumo: `somar_periodos(...dentro=...)` recorta os períodos pela data civil de `p.entrada`, sem `dia_da_jornada`, em `app/ponto/services/espelho.py:82-86`; é chamado por `montar_dias` em `:439-441`. O topo de competência depois passa por `folha_manda` em `:1283-1284`; essa função usa `totais_da_folha` em `app/relatorios/cartao_pela_celula.py:323`, ou soma lavrada em `:342`. `totais_da_folha` lê `fechamento_lido` em `:69-76`, mapeando o fechamento e eventos de export em `:89-116`.

- `porta_export.dias_fora_do_dia_pago` lê DiaPago por `data` em `app/folha/porta_export.py:74-76`, pede resultado a `autoridade_do_periodo` em `:78`, mas agrupa **novamente por civil** (`localtime(p.entrada).date()`) em `:101-105`, sem consultar mapa do motor; compara contra `pago_do_dia` em `:107-118`. `medir` chama essa testemunha em `:597`, e também compara soma de DiaPago ao fechamento em `_medir_dia_pago`, `:378-388`. Assim, uma cauda cuja entrada civil seja d+1, mas mapa seja d, pode gerar divergência diária nessa testemunha mesmo com dinheiro mensal igual. `app/folha/porta_export.py:101-118`; `app/ponto/services/dia_pago.py:128-132`.

- PDF do espelho: `_coletar_dados_espelho` chama `espelho_do_colab` e recorta seus `dias`, em `app/relatorios/pdf_espelho.py:596-602`; `_build_story` lê `pago_h` e `rubricas` em `:901-914`. O total do PDF passa por `folha_manda` em `:628`. As colunas são noturno, atraso, saída antecipada, HE50, HE100 e intra, definidas em `:844-847`; não há coluna própria do subconjunto HE100 noturna nesse trecho. `app/relatorios/pdf_espelho.py:844-847`.

- PDF do TXT é outro leitor: `folha.pdf_export.gerar_pdf_bytes` chama `traduzir_conteudo` em `app/folha/pdf_export.py:173-185`; este lê `reg.conteudo` em `:54-65`, e o parser extrai valores dos registros 10 e datas dos registros 11 em `:111-137`. Não decide pertença de minuto ao turno nesse caminho. `app/folha/pdf_export.py:111-137`. Para formar eventos de export, `folha.export.eventos_do_fechamento` lê HE100/feriado em `app/folha/export.py:420-423` e emite HE50, HE100 normal/feriado, FT e noturno em `:439-447`; os subconjuntos de HE noturna não são eventos separados nesse retorno. `app/folha/export.py:439-447`.

Não executei banco nem motor; afirmações de números históricos presentes nas docstrings não foram tratadas como medições desta auditoria. [não verificado] [nao verificado]

## 07/11 04:39 · sessao e0720c2a392d · FIO-E0-V4-E2 · verificador · origem=Code

**A cura do leitor está presente; a cura integral de `data_evento` não está fechada:** ainda existe caminho de nascimento que aceita dia civil do marco, e o migrador deixa partes do acervo nesse significado. (`app/chamados/services/tarefa_do_dia.py:176`; `app/chamados/services/disputa_emissao.py:1037`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:186`)

A cópia não contém histórico Git utilizável: o diff exato `0c0db89f → cdff0483 → 129754b9`, a atribuição histórica dos resíduos e os RED/GREEN citados ficam **[não verificado]**. Execução da bateria, custo em produção e ocorrência dos contraexemplos no acervo também ficam **[não verificado]**.

1. **Procedentes da volta 2**

   - **b — releitura civil/véspera: curada no leitor.** A pergunta entra exclusivamente por `p.data_evento == dia`; a carga filtra `[ini, fim]`, sem acrescentar D+1. Não achei a heurística anterior nesses caminhos. Isso depende de os escritores entregarem o dia correto — há resíduos descritos abaixo. (`app/chamados/services/tarefa_do_dia.py:176`; `app/chamados/services/tarefa_do_dia.py:252`; `app/chamados/services/tarefa_do_dia.py:292`)
   - **d — forma agregada: explicação coerente com o comportamento.** O leitor agregado devolve marcos vazios; a tarefa responde `nao_sei/ata_sem_lampadas` em dia de trabalho sem instâncias. Não achei contraexemplo nesses ramos. O teste E2 usa lâmpadas vazias, sem montar explicitamente a ata agregada com contagens. (`app/escala/services/leitor_celula.py:170`; `app/chamados/services/tarefa_do_dia.py:160`; `app/chamados/tests/test_e2_tarefa_do_dia.py:382`)
   - **f — limite inferior: curado.** A guarda é bilateral e o teste exige `ValueError` tanto em D−1 quanto em D+1; o mutante `dia > fim` seria alcançado pelo caso D−1. (`app/chamados/services/tarefa_do_dia.py:290`; `app/chamados/tests/test_e2_tarefa_do_dia.py:435`)
   - **Derivação paralela de posição: removida entre os dois consumidores.** Ambos chamam `posicao_do_marco`; o selo troca essa autoridade e observa os dois leitores. A cobertura dos argumentos tem uma lacuna, detalhada no item 5. (`app/chamados/services/declaracao_texto.py:134`; `app/chamados/services/tarefa_do_dia.py:58`; `app/chamados/tests/test_e2_tarefa_do_dia.py:453`)

2. **Quem escreve `PerguntaDisputa.data_evento`**

   O censo AST localizou três sítios de persistência: `_nascer`, `reapontar_resolvedora` e `rechavear_data_evento`; são as três portas declaradas pelo selo. O selo verifica o endereço da escrita, sem validar a semântica do valor recebido. (`app/chamados/models.py:2578`; `app/chamados/services/fio.py:838`; `app/chamados/services/fio.py:904`; `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:30`; `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:136`)

   | Porta/caminho | Origem efetiva e conclusão |
   |---|---|
   | `abrir → _nascer` | Exige data não nula e grava o argumento; não confere sua origem. A anotação usa um `NewType`, e o construtor de dia da célula apenas devolve o dia recebido. (`app/chamados/models.py:2481`; `app/chamados/models.py:2578`; `app/core/dia_do_turno.py:13`; `app/ponto/turnos.py:2943`) |
   | Emissão por batida e entradas pendentes | Consultam `dia_do_turno_da_batida`; sem resposta do juiz, não produzem a pergunta datada correspondente. (`app/escala/servico_jornada.py:485`; `app/chamados/services/disputa_emissao.py:1094`; `app/chamados/services/disputa_emissao.py:282`) |
   | Complementos e declarações | Usam o dia retornado pelo marco faltante da célula; o complemento exige igualdade com o dia solicitado. (`app/chamados/services/disputa_emissao.py:850`; `app/chamados/services/declaracao_texto.py:315`; `app/chamados/services/declaracao_texto.py:343`; `app/chamados/services/declaracao_texto.py:631`) |
   | Geração geral | Aceita dia da grade, depois contexto do motivo, depois batida; o dia resultante segue para `abrir`. Contexto antigo não recebe conversão semântica nesse percurso. (`app/chamados/services/disputa_emissao.py:350`; `app/chamados/services/disputa_emissao.py:367`; `app/chamados/services/disputa_emissao.py:385`; `app/chamados/services/disputa_emissao.py:433`) |
   | Sem batida de origem | Usa `data_evento_do_contexto`, que tenta cinco chaves, incluindo `horario_previsto`; pode receber o dia civil desse instante e passá-lo ao nascimento. (`app/chamados/data_evento.py:27`; `app/chamados/data_evento.py:39`; `app/chamados/services/disputa_emissao.py:1121`; `app/chamados/services/disputa_emissao.py:1133`) |
   | `abrir_conversa` | **Grava dia civil**, por `localdate()`. Inclui reabertura administrativa e âncora ausente; essas linhas novas ficam fora da E2 por `conversa=False`. (`app/chamados/models.py:2494`; `app/chamados/services/acoes_disputa.py:260`; `app/chamados/services/disputa_emissao.py:1251`; `app/chamados/services/tarefa_do_dia.py:253`) |
   | `reapontar_resolvedora` | Só preenche data ausente. Recebe dia do chamado/contexto no reconciliador ou dia do marco na declaração; não corrige data antiga já preenchida. (`app/chamados/services/fio.py:836`; `app/chamados/reconciliador.py:941`; `app/chamados/reconciliador.py:1625`; `app/chamados/reconciliador.py:1641`; `app/chamados/services/declaracao_texto.py:336`) |
   | Rechaveamento | Grava o argumento, após gates de colisão, célula e resolvedora. A migração normal calcula o novo dia pelo juiz; `--reverter` restaura a data antiga do JSON e pode reintroduzir dia de saída. (`app/chamados/services/fio.py:893`; `app/chamados/services/fio.py:898`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:142`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:336`) |

   **Escritor vivo ainda incorreto para pergunta de marco:** o ramo `intervalo_ausente` lê `marcos_faltantes[*].data`, passa por `dia_do_turno_da_celula` e cria a pergunta. Esse helper não converte datas; o formato do detector contém a data civil do instante encadeado do marco. A emissão do detector foi aposentada, mas o consumidor de chamados legados continua executável. (`app/chamados/services/disputa_emissao.py:1027`; `app/chamados/services/disputa_emissao.py:1037`; `app/chamados/services/disputa_emissao.py:1043`; `app/ponto/turnos.py:2939`; `app/ponto/management/commands/detectar_intervalo_ausente.py:82`; `app/ponto/management/commands/detectar_intervalo_ausente.py:149`)

   **Contraexemplo estático:** turno D, intervalo ausente às 01h de D+1, contexto com `data=D+1`: nasce pergunta de intervalo em D+1. O E4-c permite a diferença de um dia quando o turno cruza meia-noite, inclusive quando não sabe; portanto esse gate não garante a correção. Ocorrência produtiva: **[não verificado]**. (`app/chamados/services/disputa_emissao.py:1035`; `app/chamados/services/disputa_emissao.py:1043`; `app/chamados/juizes.py:1993`)

   **Absorção e reabertura não saneiam a data.** A absorção lê a data existente, cria a ausência nela e registra a decisão; a reabertura altera resposta/carimbos, preservando o dia. O backfill E1 de decisões é somente DRY, e a 0051 preenche colaborador/instância, preservando a indistinguibilidade das conversas antigas. (`app/chamados/services/materializacao.py:517`; `app/chamados/services/materializacao.py:522`; `app/chamados/services/materializacao.py:560`; `app/chamados/services/disputa_emissao.py:1462`; `app/chamados/services/disputa_emissao.py:1495`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:84`; `app/chamados/migrations/0051_decisao_pergunta.py:5`; `app/chamados/migrations/0051_decisao_pergunta.py:25`)

   **O migrador deixa de fora:**

   - Intervalos/outros motivos, tipos diferentes de `hora`, datas nulas e recortes fora de `--desde/--ate`. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:35`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:186`)
   - Absorções sem batida plantada viva, fontes ausentes/ambíguas, `orfao_14h` materializada, colisões e competências exportadas; a porta ainda pode recusar célula/destino. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:51`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:150`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:257`; `app/chamados/services/fio.py:895`)
   - Algumas linhas desaparecem da classificação sem registro `FORA`: retornos `None` de `_fonte_viva` e materializadas cuja saída não coincide com a data gravada. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:136`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:140`)

   Há um contraexemplo adicional: pergunta antiga do turno D21 gravada em D22, existindo outro turno aberto D22. `_fonte_viva` considera a pergunta coerente pela existência desse segundo turno antes de procurar a saída prevista de D21; a classificação a ignora. Ocorrência produtiva: **[não verificado]**. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:90`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:138`)

   **A tarefa não transforma essas exclusões automaticamente em `nao_sei`.** Ela atribui a pergunta ao dia gravado. Em D21, marco apagado/vencido, sem cobertura nem outra pergunta, responde `sem_pergunta → perguntar`; em D22 pode consumir a pergunta legada como pertencente a D22. (`app/chamados/services/tarefa_do_dia.py:176`; `app/chamados/services/tarefa_do_dia.py:220`; `app/chamados/services/tarefa_do_dia.py:257`)

3. **`posicao_do_marco`**

   A regra atual prioriza primeiro índice, depois último, depois tipo: `inicio`; `fim`; `int_ida` para `S`, senão `int_volta`. Para `i=0,n=1`, vence `inicio`. Os dois consumidores passam os três argumentos correspondentes. A identidade universal com o corpo histórico de `0c0db89f` permanece **[não verificado]**. (`app/chamados/services/declaracao_texto.py:79`; `app/chamados/services/declaracao_texto.py:134`; `app/chamados/services/tarefa_do_dia.py:58`)

   Não achei terceiro sítio da mesma regra na busca por `i == 0`, `n - 1`, `int_ida` e `int_volta`; os dois consumidores encontrados delegam ao helper. A tabela congelada da migração é motivo→instância, sem julgamento por índice. Completude além desses padrões: **[não verificado]**. (`app/chamados/services/declaracao_texto.py:134`; `app/chamados/services/tarefa_do_dia.py:58`; `app/chamados/migrations/0051_decisao_pergunta.py:14`)

4. **Carga e queries**

   Não achei leitura de `inst_por_dia` de outro dia: os dois acessos usam `dia`, e a carga entrega somente `{dia: instancias(...)}`. (`app/chamados/services/tarefa_do_dia.py:161`; `app/chamados/services/tarefa_do_dia.py:178`; `app/chamados/services/tarefa_do_dia.py:292`)

   O selo continua mordendo um N+1 genérico por colaborador ou dia: mede construção **e consumo**, comparando dois colaboradores × três dias com um × um. Porém sua fixture não acrescenta decisões; um SELECT por pergunta apenas no ramo de validação escaparia desse selo de custo. Execução: **[não verificado]**. (`app/chamados/tests/test_e2_tarefa_do_dia.py:537`; `app/chamados/tests/test_e2_tarefa_do_dia.py:543`; `app/chamados/tests/test_e2_tarefa_do_dia.py:550`; `app/chamados/services/tarefa_do_dia.py:126`)

5. **Mordida dos testes** [nao verificado]

   Os testes reescritos atingem diretamente a releitura D22→D21 e a guarda inferior; o selo novo atinge a substituição do helper por uma cópia local. (`app/chamados/tests/test_e2_tarefa_do_dia.py:200`; `app/chamados/tests/test_e2_tarefa_do_dia.py:206`; `app/chamados/tests/test_e2_tarefa_do_dia.py:435`; `app/chamados/tests/test_e2_tarefa_do_dia.py:453`)

   **Mutante sobrevivente à bateria E2, por inspeção:** trocar a chamada de `_marcos_faltantes` para `posicao_do_marco(i, n, 'S')`. O substituto do selo ignora `tipo`; no uso real, uma entrada intermediária passaria de `int_volta` para `int_ida`. A suíte mais ampla contém teste que deveria matá-lo, esperando explicitamente `int_volta/E`; execução **[não verificado]**. (`app/chamados/tests/test_e2_tarefa_do_dia.py:453`; `app/chamados/services/declaracao_texto.py:83`; `app/chamados/services/declaracao_texto.py:134`; `app/chamados/tests/test_hx_ata_nao_sabe.py:69`)

   Outro sobrevivente E2 por inspeção é inverter a prioridade dos dois `if`: altera o caso `i=0,n=1`, ausente das montagens de dois/quatro marcos dessa bateria. Execução: **[não verificado]**. (`app/chamados/services/declaracao_texto.py:79`; `app/chamados/tests/test_e2_tarefa_do_dia.py:65`; `app/chamados/tests/test_e2_tarefa_do_dia.py:176`)

6. **Checklist**

   - **Segundo juiz:** não achei clone novo de teto, vínculo ou cobertura; esses caminhos delegam às autoridades. Há derivador paralelo preexistente no caminho escritor: `data_evento_do_contexto` tenta chaves próprias, enquanto o juiz registrado lê a chave declarada no catálogo. (`app/ponto/turnos.py:3052`; `app/ponto/turnos.py:3059`; `app/ponto/turnos.py:3072`; `app/chamados/data_evento.py:27`; `app/chamados/catalogo/modulos.py:219`; `app/docs/JUIZES.md:56`)
   - **Segundo escritor:** não achei persistência fora das três portas declaradas no censo; o erro de intervalo atravessa uma porta permitida, portanto o selo P13 não o detecta. (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:30`; `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:136`; `app/chamados/services/disputa_emissao.py:1043`)
   - **Relógio fora da borda:** a E2 passa `agora` à lavra vigente, fila e teto. Permanece a questão externa já triada: o escritor da lavra usa `date.today()`/`timezone.now()`, enquanto a validade lê `agora.date()`. (`app/chamados/services/tarefa_do_dia.py:270`; `app/chamados/services/tarefa_do_dia.py:278`; `app/chamados/services/tarefa_do_dia.py:285`; `app/chamados/veredito_lavrado.py:112`; `app/chamados/veredito_lavrado.py:182`; `app/chamados/veredito_lavrado.py:228`)
   - **Fonte única e selo sem mordida:** posição centralizada; selo de delegação incompleto quanto aos argumentos; custo não cobre o ramo de validação. (`app/chamados/services/declaracao_texto.py:134`; `app/chamados/tests/test_e2_tarefa_do_dia.py:453`; `app/chamados/tests/test_e2_tarefa_do_dia.py:537`)
   - **ESMERIL-SEMÂNTICA:** o campo ainda comporta dia civil da conversa e dia do turno do fato; `conversa` distingue as linhas novas, mas o legado não foi distinguido pela 0051. Além disso, o caminho de intervalo pode introduzir dia civil dentro de pergunta do fato. (`app/chamados/models.py:2483`; `app/chamados/models.py:2494`; `app/chamados/migrations/0051_decisao_pergunta.py:5`; `app/chamados/services/disputa_emissao.py:1043`)

## 07/11 04:38 · sessao 2e3f23bb9008 · - · - · origem=Ronald (tmux)

Censo somente leitura de `app/`, migradores e chamadas às portas: encontrei estas escritas persistentes e origens.

- **Nascimento:** `PerguntaDisputa._nascer` grava `data_evento` no `create`; `abrir` exige apenas valor não nulo, sem conferir dinamicamente que seja `DiaDoTurno`. A anotação sozinha não converte nem valida datas. (`app/chamados/models.py:2470`, `app/chamados/models.py:2481`, `app/chamados/models.py:2578`)
- **Costura de pergunta sem data:** `reapontar_resolvedora` só grava o parâmetro quando `p.data_evento` está vazio; data entra antes do E4-c e da lavra. Chamadores com parâmetro estão no reconciliador e na declaração. (`app/chamados/services/fio.py:836`, `app/chamados/services/fio.py:851`, `app/chamados/reconciliador.py:941`, `app/chamados/reconciliador.py:1625`, `app/chamados/reconciliador.py:1641`, `app/chamados/services/declaracao_texto.py:336`)
- **Rechaveamento:** grava o valor trazido pelo chamador, recusa colisão/ausência de célula quando havia célula anterior/E4-c, relavra e salva. A função não consulta a origem para garantir que o novo valor é dia de turno. (`app/chamados/services/fio.py:884`, `app/chamados/services/fio.py:893`, `app/chamados/services/fio.py:898`, `app/chamados/services/fio.py:904`)
- **Conversa continua escrevendo dia civil:** `abrir_conversa` passa `timezone.localdate()` com `conversa=True`; isso inclui confirmação de reabertura administrativa e âncora ausente. A carga E2 exclui as conversas pela coluna. (`app/chamados/models.py:2487`, `app/chamados/models.py:2494`, `app/chamados/services/acoes_disputa.py:259`, `app/chamados/services/disputa_emissao.py:1251`, `app/chamados/services/tarefa_do_dia.py:252`)
- **Absorção não escreve `data_evento`:** lê a data da pergunta, cria ausência nessa data, carimba irmãs da mesma data e registra a decisão de absorção. Se o dia gravado já era civil errado, este caminho o propaga à ausência. (`app/chamados/services/materializacao.py:517`, `app/chamados/services/materializacao.py:521`, `app/chamados/services/materializacao.py:532`, `app/chamados/services/materializacao.py:558`)
- **Reaberturas de pergunta preservam o dia:** `reabrir_fantasma`, `reabrir_por_resposta`, `reabrir_por_competencia` delegam a descarimbo, sem nova atribuição de `data_evento` nesses métodos. (`app/chamados/models.py:2825`, `app/chamados/models.py:2834`, `app/chamados/models.py:2837`, `app/chamados/models.py:2851`, `app/chamados/models.py:2853`)
- **Backfill E1 não escreve datas:** a 0051 preenche colaborador e instância e deixa conversa antiga indistinguível, com default `False`; `backfill_decisoes_pergunta` é apenas DRY de linhas e colisões. (`app/chamados/migrations/0051_decisao_pergunta.py:4`, `app/chamados/migrations/0051_decisao_pergunta.py:22`, `app/chamados/migrations/0051_decisao_pergunta.py:99`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:3`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:26`)

**Portas cuja origem está correta:**

- Emissão agrupada por batida usa `resolver_data_evento`, que delega à autoridade `dia_do_turno_da_batida`; sem resposta não emite. (`app/chamados/services/disputa_emissao.py:1092`, `app/escala/servico_jornada.py:485`)
- Entradas pendentes são resolvidas pelos PKs das batidas, e não pela data civil do item. (`app/chamados/services/disputa_emissao.py:275`, `app/chamados/services/disputa_emissao.py:282`)
- Complementos leem o dia retornado por `_marcos_faltantes`, exigem igualdade com o dia pedido e o passam como dia da célula. (`app/chamados/services/disputa_emissao.py:836`, `app/chamados/services/disputa_emissao.py:850`)
- Declaração direta e aplicação de proposta usam o dia do marco faltante convertido nominalmente por `dia_do_turno_da_celula`. (`app/chamados/services/declaracao_texto.py:315`, `app/chamados/services/declaracao_texto.py:341`, `app/chamados/services/declaracao_texto.py:629`)

**Escritor vivo que ainda aceita dia civil do marco:**

O ramo dedicado a `intervalo_ausente` lê `contexto_json.marcos_faltantes[*]['data']`, passa esse mesmo dia por `_dtc_i` e cria pergunta de intervalo. Esse construtor **não converte**: retorna `DiaDoTurno(dia)`. O formato produzido por `furos_intervalo` contém explicitamente a data local do instante encadeado do marco — D+1 em intervalo após meia-noite. O emissor daquele command está aposentado, mas o consumidor que cria perguntas de chamados legados continua no código executável. (`app/chamados/services/disputa_emissao.py:1027`, `app/chamados/services/disputa_emissao.py:1037`, `app/chamados/services/disputa_emissao.py:1041`, `app/ponto/turnos.py:2939`, `app/ponto/management/commands/detectar_intervalo_ausente.py:77`, `app/ponto/management/commands/detectar_intervalo_ausente.py:82`, `app/ponto/management/commands/detectar_intervalo_ausente.py:149`)

Contraexemplo: chamado de intervalo do turno D com marco ausente às 01h de D+1 e `marcos_faltantes.data=D+1` → a pergunta nasce com D+1. O gate E4-c permite diferença simétrica de um dia se o turno cruza meia-noite, inclusive quando não sabe; portanto não bloqueia esta gravação. Existência de chamado produtivo nessa configuração: **[não verificado]**. (`app/chamados/services/disputa_emissao.py:1035`, `app/chamados/services/disputa_emissao.py:1043`, `app/chamados/juizes.py:1993`)

Há ainda fallback pelo derivador antigo `data_evento_do_contexto`: hierarquia por cinco chaves, terminando em `horario_previsto`, cujo dia pode ser o dia civil do marco. Esse valor alimenta tanto complementos de pergunta existente quanto novas perguntas sem origem. É derivação paralela ao juiz registrado do dia do chamado, sem validação da origem semântica da data. Ocorrência produtiva com fallback incorreto: **[não verificado]**. (`app/chamados/data_evento.py:16`, `app/chamados/data_evento.py:27`, `app/chamados/data_evento.py:39`, `app/chamados/services/disputa_emissao.py:935`, `app/chamados/services/disputa_emissao.py:963`, `app/chamados/services/disputa_emissao.py:1121`, `app/chamados/services/disputa_emissao.py:1133`, `app/chamados/catalogo/modulos.py:219`)

**Migração não cobre todo o acervo:**

- Consulta só `orfao_14h`/`saida_sem_entrada`, tipo `hora`, data não nula; intervalos e outros motivos ficam fora desde a consulta. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:35`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:186`)
- Materializada sem batida plantada viva — incluindo absorção — fica fora; duas fontes ou ausência de turno também. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:47`)
- `orfao_14h` materializada explicitamente não migra; colisão também não. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:150`)
- Materializada cuja saída não coincide com o dia gravado é ignorada; vivas sem candidato também podem sumir por `continue`, sem registro FORA. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:84`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:104`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:136`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:140`)
- Competência exportada em dia velho ou novo fica fora; no apply, recusas da porta não contam automaticamente como erro de rollback, embora erros e descarimbo contem. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:233`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:253`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:226`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:289`)
- `--reverter` pode reintroduzir o significado velho, pois lê a data anterior do JSON e a passa diretamente a `rechavear_data_evento`. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:336`)

Outro contraexemplo estático: pergunta antiga do noturno D21 gravada D22, com um segundo turno aberto D22. `_fonte_viva` a considera coerente pela existência do turno D22 antes de procurar a saída esperada de D21, devolve `None` e a classificação a ignora. Ocorrência em produção: **[não verificado]**. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:90`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:136`)

A E2 **não marca automaticamente essas exclusões como `nao_sei`**: carrega por `data_evento` literal e associa por igualdade literal. Assim, pergunta errada em D+1 é considerada em D+1; em D, se o marco está vencido/apagado, sem cobertura e sem outra pergunta, responde `sem_pergunta` com ação `PERGUNTAR`. Isso é consequência do contrato sem releitura, mas depende de a E1/migração entregar a data correta. (`app/chamados/services/tarefa_do_dia.py:252`, `app/chamados/services/tarefa_do_dia.py:176`, `app/chamados/services/tarefa_do_dia.py:220`)

O selo P13 prova endereço do escritor, não a semântica da origem que chega a ele: sua whitelist permite `_nascer`, `reapontar_resolvedora`, `rechavear_data_evento`; o ramo intervalo passa pela porta permitida e não é pego por ele. (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:30`, `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:136`)

## 07/11 04:37 · sessao 69241ecf9b7c · - · - · origem=Ronald (tmux)

- **Não achei acesso a `inst_por_dia` de outro dia.** Os únicos acessos são `.get(dia)` em `app/chamados/services/tarefa_do_dia.py:161` e `:178`; `CargaDaTarefa.tarefa` fornece exatamente `{dia: self.instancias(cid, dia)}` em `:292`. A carga de perguntas também usa o intervalo fechado `[ini, fim]`, sem `d+1`, em `:252-255`; a guarda recusa ambos os lados fora da janela em `:290-291`.

- **O selo ainda morde um N+1 genérico por colaborador ou dia.** A medição envolve construir a carga e percorrer cada colaborador/dia (`app/chamados/tests/test_e2_tarefa_do_dia.py:543-549`), comparando 2 colaboradores × 3 dias com 1 × 1 (`:550`). Uma query por colaborador adicionaria 2 contra 1; por célula, 6 contra 1. A execução e seu resultado são **[não verificado]**.

- **Limite do selo:** os três fatos da fixture são perguntas sem decisões explicitamente acrescentadas (`app/chamados/tests/test_e2_tarefa_do_dia.py:537-539`); a função só chega aos ramos de validação depois de haver decisão (`app/chamados/services/tarefa_do_dia.py:115-126`). Um mutante que acrescentasse um `SELECT` por chamada dentro do ramo `ult.ato == D.VALIDACAO` (`:126`) escaparia desse selo de queries. A passagem pela bateria inteira é **[não verificado]**, mas os testes de validação examinados não impõem contagem total de queries; os selos SQL específicos vetam `eventocelula` (`app/chamados/tests/test_e2_tarefa_do_dia.py:313`) ou `folha_` (`:491`), e o selo de pureza veta somente escrita/lock (`:505-508`).

- **Não achei segundo juiz para teto/vínculo/cobertura nos caminhos examinados.** O vínculo delega a `vinculo_do_dia` (`app/ponto/turnos.py:3049-3054`); o teto delega a `cartorio.marcos_com_teto` (`:3056-3062`), que chama `marcos_turno` (`app/ponto/services/cartorio.py:848-859`); a cobertura reaproveita `_pelo_efeito`, `_q_cobertura` e `_cobertura_pura` (`app/ponto/turnos.py:3064-3077`). A autoridade da tarefa consta do registro vivo (`app/core/juizes.py:150-155`, `:285`) e da tabela (`app/docs/JUIZES.md:73-80`).

- **Relógio da E2:** a carga entrega `agora` explicitamente a `esta_stale` (`app/chamados/services/tarefa_do_dia.py:267-270`), à fila (`:278-279`) e ao teto (`:285`). A casca recusa `None` e carga aberta em outro instante (`:309-314`). O relógio civil preexistente continua no escritor da lavra: `expira_em` usa `datetime.date.today()` (`app/chamados/veredito_lavrado.py:112`), e `lavrar` usa `timezone.now()` (`:228`); o leitor de validade compara `agora.date()` (`:181-185`). A E2 lê a lavra; não chama `lavrar` no caminho examinado (`app/chamados/services/tarefa_do_dia.py:266-270`).

- **Fontes únicas adicionais:** o passivo vem de `DecisaoPergunta.linhas_da_projecao_sem_linha` (`app/chamados/services/tarefa_do_dia.py:264-265`), cuja identificação é por ato e instante (`app/chamados/models.py:3409-3424`); o fechamento vem da lavra vigente (`app/chamados/services/tarefa_do_dia.py:126-134`, `:266-270`), sem rejulgar localmente o efeito.

- Não executei testes nem consultei banco/log; execução, RED/GREEN e custo em produção são **[não verificado]**. [nao verificado]

## 07/11 04:36 · sessao b74c85bf8bb9 · - · - · origem=Ronald (tmux)

- A extração está centralizada na cópia: `posicao_do_marco` prioriza `i == 0`, depois `i == n - 1`, depois `tipo == 'S'`; `_marcos_faltantes` passa `(i, n, tipo)` e `_posicoes` passa `(i, n, lp['tipo'])`. Para `i=0,n=1`, o resultado atual é `inicio`. Não certifiquei identidade universal com o predicado de `0c0db89f`, pois o histórico Git não está disponível. (`app/chamados/services/declaracao_texto.py:75-83`, `:129-134`; `app/chamados/services/tarefa_do_dia.py:55-58`; histórico: [não verificado].)

- Não achei terceiro sítio com essa regra na busca por `i == 0`, `n - 1`, `int_ida`, `int_volta` em arquivos Python. Os dois consumidores localizados usam o helper; a migração tem somente a tabela motivo→posição. (`app/chamados/services/declaracao_texto.py:134`; `app/chamados/services/tarefa_do_dia.py:58`; `app/chamados/migrations/0051_decisao_pergunta.py:15-19`; completude além dos padrões buscados: [não verificado].)

- O selo novo morde a reintrodução da cópia nos dois consumidores: substitui o helper por posições `P0…P3` e exige que ambos acompanhem. Mas o substituto ignora `n` e `tipo`; logo não demonstra preservação dos três argumentos. (`app/chamados/tests/test_e2_tarefa_do_dia.py:445-456`.)

- Mutante concreto sobrevivente à bateria E2 por inspeção: trocar, em `_marcos_faltantes`, `posicao_do_marco(i, n, tipo)` por `posicao_do_marco(i, n, 'S')`. O único exercício de `_marcos_faltantes` na bateria acontece sob o substituto que ignora `tipo`; em produção, uma entrada intermediária viraria `int_ida`. A suíte mais ampla contém teste que deveria matá-lo: espera explicitamente `int_volta/E` na terceira posição. (`app/chamados/services/declaracao_texto.py:83`, `:134`; `app/chamados/tests/test_e2_tarefa_do_dia.py:453-456`; `app/chamados/tests/test_hx_ata_nao_sabe.py:63-69`; execução: [não verificado].)

- Outro mutante sobrevivente à bateria E2 por inspeção: inverter a prioridade dos dois `if` do helper, testando fim antes de início. Só muda a interseção `i=0,n=1`, ausente das montagens da bateria, que usam quatro marcos ou os dois marcos do noturno. (`app/chamados/services/declaracao_texto.py:79-82`; `app/chamados/tests/test_e2_tarefa_do_dia.py:65-66`, `:176`, `:355`; execução: [não verificado].)

- Os testes reescritos atacam diretamente a releitura civil: pergunta de D22 não entra no turno D21, inclusive com D22 trabalhando; o teste de janela exige erro tanto em D−1 quanto em D+1. (`app/chamados/tests/test_e2_tarefa_do_dia.py:200-212`, `:431-437`.)

- A alteração documental sobre a forma agregada está coerente com o leitor: a tarefa retorna `ata_sem_lampadas` quando falta instância em dia de trabalho; o leitor agregado devolve marcos vazios, sem identificar quais faltaram. (`app/chamados/services/tarefa_do_dia.py:160-163`; `app/escala/services/leitor_celula.py:146-172`.)

- O selo de queries mede carga e consumo para 2 colaboradores × 3 dias contra 1 × 1, ambos com perguntas; assim, um SELECT adicional por colaborador ou dia nesse percurso aumentaria a contagem comparada. Não prova percursos que a montagem não ativa. (`app/chamados/tests/test_e2_tarefa_do_dia.py:531-550`.)

## 07/11 04:28 · sessao efc6cfc6aeb8 · FIO-E0-V4-E2 · verificador · origem=Code

**Não aprovo a volta 2 nesta cópia.** O principal problema é a nova ambiguidade aplicada a perguntas cujo nascimento já recebeu explicitamente o **dia do turno**. O teste novo exige essa ambiguidade e consolida o erro (`app/chamados/models.py:2477`; `app/chamados/tests/test_e2_tarefa_do_dia.py:351`; `app/chamados/tests/test_e2_tarefa_do_dia.py:355`).

A cópia não contém `.git`; comparação efetiva com `9bd00eca`, exclusividade dos três arquivos e execução da bateria/mutantes: **[nao verificado]**. Abaixo julgo o código disponível; não infiro ocorrências em produção.

**1. Procedentes a–g: cura no sítio e contraexemplo**

| Item | Julgamento |
|---|---|
| **a — conversa** | **Curado para `conversa=True`**: a query agora exclui essas perguntas; o teste verifica que o marco continua sem pergunta (`app/chamados/services/tarefa_do_dia.py:278`; `app/chamados/tests/test_e2_tarefa_do_dia.py:338`). **Residual:** conversa antiga recebe `False` na migração e pode continuar ocupando o marco; a própria migração declara essa indistinguibilidade (`app/chamados/migrations/0051_decisao_pergunta.py:5`; `app/chamados/migrations/0051_decisao_pergunta.py:100`). |
| **b — noturnos seguidos** | **A restrição proposta está implementada**, mas não constitui cura semântica completa: havendo posição nos dois dias, devolve `None` (`app/chamados/services/tarefa_do_dia.py:113`). **Contraexemplo:** dois noturnos 22–06, saídas apagadas, pergunta **nova** aberta para o turno d, sem atos: seu dono é conhecido pela porta de nascimento, mas a tarefa devolve `pergunta_de_turno_ambiguo`. O próprio teste fabrica esse caso e exige a resposta errada (`app/chamados/models.py:2477`; `app/chamados/tests/test_e2_tarefa_do_dia.py:346`; `app/chamados/services/tarefa_do_dia.py:232`). |
| **c — documento** | **Curado nos dois sítios:** cobertura e absorção entregam `entregar_documento` ao colaborador (`app/chamados/services/tarefa_do_dia.py:164`; `app/chamados/services/tarefa_do_dia.py:225`). Não achei contraexemplo nessa troca de dono; examinei estado inicial, transições, reabertura e os dois testes correspondentes (`app/ponto/services/ausencia.py:779`; `app/ponto/services/ausencia.py:767`; `app/ponto/services/ausencia.py:1119`; `app/chamados/tests/test_e2_tarefa_do_dia.py:357`; `app/chamados/tests/test_e2_tarefa_do_dia.py:366`). |
| **d — ata sem lâmpadas** | **Curado o “vazio conhecido”**: retorna `nao_sei=True` (`app/chamados/services/tarefa_do_dia.py:182`). Não achei contraexemplo ao resultado conservador; achei erro na explicação “ata não lavrada”: existe forma antiga **já lavrada**, agregada, sem lâmpadas (`app/escala/services/leitor_celula.py:146`; `app/chamados/services/tarefa_do_dia.py:183`). |
| **e — quatro mutantes** | Os testes têm asserções que distinguem fila vazia, expiração ignorada, decisões ordenadas por PK e supressão de folga removida (`app/chamados/tests/test_e2_tarefa_do_dia.py:392`; `app/chamados/tests/test_e2_tarefa_do_dia.py:409`; `app/chamados/tests/test_e2_tarefa_do_dia.py:415`; `app/chamados/tests/test_e2_tarefa_do_dia.py:424`). O relato de sete failures não foi reproduzido **[nao verificado]**. Há outros sobreviventes, no item 6. |
| **f — índice e janela** | **Implementados:** agrupamento por `(colaborador_id,data_evento)` e rejeição de dia fora dos dois limites (`app/chamados/services/tarefa_do_dia.py:282`; `app/chamados/services/tarefa_do_dia.py:315`). Não achei contraexemplo à guarda atual; o teste cobre somente o limite superior (`app/chamados/tests/test_e2_tarefa_do_dia.py:431`). |
| **g — caso 10** | **Curado o exagero textual:** a docstring admite que não fabrica competência exportada; o teste verifica ação exigível e ausência de consulta à folha (`app/chamados/tests/test_e2_tarefa_do_dia.py:27`; `app/chamados/tests/test_e2_tarefa_do_dia.py:459`). Exportação efetiva continua **[nao verificado]**. |

**2. Dono da pergunta: tabela completa da decisão**

Para `d = p.data_evento`, a função reduz todas as combinações a dois predicados:

- **V:** existe, na véspera, a posição pedida com `data_civil == d`.
- **D:** existe essa posição no turno d, em qualquer dia civil.

Essa é a decisão efetiva; não consulta batidas, atos, `trabalha`, tipo do dia nem a FK da célula da pergunta (`app/chamados/services/tarefa_do_dia.py:109`; `app/chamados/services/tarefa_do_dia.py:111`; `app/chamados/services/tarefa_do_dia.py:113`).

| Posição | V | D | Dono devolvido |
|---|---:|---:|---|
| início | qualquer | qualquer | d |
| pausa/retorno/fim | falso | qualquer | d |
| pausa/retorno/fim | verdadeiro | falso | d−1 |
| pausa/retorno/fim | verdadeiro | verdadeiro | `None` |

A tabela é exaustiva para essas posições, conforme os únicos retornos da função (`app/chamados/services/tarefa_do_dia.py:111`; `app/chamados/services/tarefa_do_dia.py:115`).

O cruzamento dos cadastros pedidos entra assim nessa tabela:

| Situação em cada dia | Como participa |
|---|---|
| **Diurno** | As posições ficam no próprio dia civil; portanto o diurno da véspera normalmente não satisfaz V (`app/escala/servico_jornada.py:80`). |
| **Noturno** | Fim e pausas satisfazem V somente quando seu instante encadeado caiu em d. Pausa anterior à meia-noite não satisfaz V (`app/escala/servico_jornada.py:75`; `app/escala/servico_jornada.py:82`). |
| **Folga** | Não possui tratamento especial no dono. Sem lâmpadas, não oferece posição; conservando lâmpadas antigas, participa como qualquer turno (`app/chamados/services/tarefa_do_dia.py:86`; `app/chamados/services/tarefa_do_dia.py:111`). O cadastro de folga com lâmpadas antigas existe na própria bateria (`app/chamados/tests/test_e2_tarefa_do_dia.py:421`). |
| **Sem célula** | Produz lista vazia de instâncias: V ou D fica falso, conforme o lado (`app/chamados/services/tarefa_do_dia.py:84`). |
| **Ata sem lâmpadas E/S** | Também produz lista vazia; isso é indistinguível de “posição inexistente” para o dono (`app/chamados/services/tarefa_do_dia.py:54`; `app/chamados/services/tarefa_do_dia.py:86`). |
| **Duas pausas** | As duas ocorrências têm a mesma `pos`, embora `instancia` receba sufixo `:2`; o dono compara `pos`, sem identificar qual pausa (`app/chamados/services/tarefa_do_dia.py:62`; `app/chamados/services/tarefa_do_dia.py:64`; `app/chamados/services/tarefa_do_dia.py:112`). |

Portanto, **noturno→diurno** e **noturno→noturno** ficam igualmente ambíguos para uma posição pós-meia-noite existente nos dois turnos; **noturno→folga sem lâmpadas**, **sem célula** ou **ata vazia** entrega à véspera. Os demais cruzamentos seguem diretamente V/D, inclusive folgas com lâmpadas residuais (`app/chamados/services/tarefa_do_dia.py:111`; `app/chamados/services/tarefa_do_dia.py:115`).

**A premissa das “duas leituras vivas” está desatualizada para o nascimento atual.** O emissor chama `resolver_data_evento`, que agora devolve o dia do turno pela autoridade da batida; complementos da célula também passam seu dia do turno (`app/chamados/services/disputa_emissao.py:1094`; `app/escala/servico_jornada.py:474`; `app/escala/servico_jornada.py:487`; `app/chamados/services/disputa_emissao.py:858`). Há código específico para migrar o significado antigo; sua aplicação no acervo é **[nao verificado]** (`app/chamados/services/fio.py:859`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:8`).

Assim:

- **Decidível tornado ambíguo:** a pergunta nova dos dois noturnos, já citada; `abrir` recebe dia do turno e vincula a célula desse dia, mas o dono ignora essa informação (`app/chamados/models.py:2477`; `app/chamados/models.py:2530`; `app/chamados/services/tarefa_do_dia.py:101`).
- **Atribuição errada pelo contrato atual:** véspera noturna, célula d trabalhando mas com ata vazia, pergunta nova de fim para d: o dono entrega a d−1. Uma decisão de silêncio dessa pergunta pode então encerrar a exigência da véspera (`app/chamados/services/tarefa_do_dia.py:115`; `app/chamados/services/tarefa_do_dia.py:156`; `app/chamados/services/tarefa_do_dia.py:198`).
- **Terceiro turno, fora de ambas as datas candidatas:** não achei; os únicos retornos são d, d−1 e `None`. Isso não prova pertencimento quando nenhuma posição foi identificada (`app/chamados/services/tarefa_do_dia.py:115`).
- **Segunda pausa:** continua incompleta antes mesmo do dono, pois `marcos_turno` monta somente o primeiro intervalo; a segunda pode ficar sem teto e sair como `marco_fora_do_turno` (`app/escala/servico_jornada.py:75`; `app/chamados/services/tarefa_do_dia.py:215`). Se a pergunta trouxer explicitamente `int_ida:2`, a comparação com `i['pos']` também não casa (`app/chamados/services/tarefa_do_dia.py:98`; `app/chamados/services/tarefa_do_dia.py:112`).

**3. Escritores e leitores dos dois campos**

**`PerguntaDisputa.conversa`:**

- **Escritores atuais:** `abrir` passa `False`; `abrir_conversa` passa `True`; `_nascer` persiste o valor (`app/chamados/models.py:2483`; `app/chamados/models.py:2494`; `app/chamados/models.py:2578`).
- **Portas de conversa encontradas:** reabertura administrativa, emissão sem dia resolvido e âncora ausente. Todas expressam conversa sem identidade de marco (`app/chamados/services/acoes_disputa.py:259`; `app/chamados/services/disputa_emissao.py:420`; `app/chamados/services/disputa_emissao.py:1251`).
- **Leitores explícitos encontrados:** identidade dos fatos, trava/deduplicação no nascimento, derivação da instância, censo de colisões e carga E2 (`app/chamados/models.py:2466`; `app/chamados/models.py:2508`; `app/chamados/models.py:2528`; `app/chamados/models.py:2972`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:74`; `app/chamados/services/tarefa_do_dia.py:278`).

**`True = sem fato` está coerente nesses escritores. O inverso `False = fato` não é garantido para o legado:** a migração atribui `False` às conversas antigas e deriva a instância somente pelo motivo (`app/chamados/migrations/0051_decisao_pergunta.py:5`; `app/chamados/migrations/0051_decisao_pergunta.py:26`). O novo teste só força `True` numa pergunta moderna; não cobre esse residual (`app/chamados/tests/test_e2_tarefa_do_dia.py:341`).

**`Ausencia.status='aguardando_documento'`:**

- **Decisão e escrita:** `estado_inicial` determina o estado; criação grava esse resultado; alteração de tipo e reabertura voltam ao mesmo juiz; `mudar_estado` efetiva as transições (`app/ponto/services/ausencia.py:795`; `app/ponto/services/ausencia.py:333`; `app/ponto/services/ausencia.py:344`; `app/ponto/services/ausencia.py:543`; `app/ponto/services/ausencia.py:1119`; `app/ponto/services/ausencia.py:824`). O backfill histórico também classifica pelo documento/tipo (`app/ponto/migrations/0057_a4_backfill_pendente.py:35`).
- **Leitores coerentes:** modelo, contadores, escalonador, cobrança ao colaborador, mensagens de tela, endpoint de anexo e os dois ramos E2 (`app/ponto/models.py:701`; `app/ponto/services/ausencia.py:965`; `app/ponto/services/ausencia.py:993`; `app/ponto/services/ausencia.py:1019`; `app/ponto/views.py:2591`; `app/api/views.py:3117`; `app/chamados/services/tarefa_do_dia.py:164`; `app/chamados/services/tarefa_do_dia.py:225`).

**Não estão todos os leitores alinhados:** o payload dos chamados do app oferece “Anexar comprovante” somente para `status == 'pendente'`, ausente do vocabulário atual. Uma ausência `aguardando_documento` não recebe esse bloco (`app/api/views.py:1087`; `app/api/views.py:1091`; `app/ponto/models.py:701`).

**Não achei outro status válido não final cuja obrigação seja do colaborador:** os únicos abertos são documento e decisão; este último pertence ao DP. Permitir anexar durante a decisão não cria outra obrigação de documento (`app/ponto/services/ausencia.py:765`; `app/ponto/models.py:702`; `app/api/views.py:3117`).

**4. Ata lavrada com zero lâmpada E/S**

**Sim, o formato antigo agregado é reconhecido explicitamente.** Ele sabe quantidades, mas não identifica os marcos; logo `nao_sei` é defensável, enquanto “não foi lavrada” é falso como explicação geral (`app/escala/services/leitor_celula.py:146`; `app/escala/services/leitor_celula.py:216`; `app/chamados/tests/test_e2_tarefa_do_dia.py:38`).

**Ausência total:** o caminho examinado transforma o dia em `ausencia` e limpa suas colunas; `dia_sem_trabalho` reconhece esse tipo antes da nova guarda. Não achei falso positivo causado somente por esse caminho (`app/escala/utils.py:1220`; `app/escala/utils.py:1251`; `app/chamados/juizes.py:429`; `app/chamados/services/tarefa_do_dia.py:181`).

**Somente lâmpadas de outro tipo:** o ramo alternativo aceita tipos vindos dos marcos/colunas e existem colunas `I`; não achei trajetória comprovada de trabalho legítimo com somente `I`, sem E/S. Ocorrência em produção: **[nao verificado]** (`app/ponto/services/cartorio.py:688`; `app/escala/utils.py:1383`).

A cadeia de escrita encontrada é:

- **Produção:** `lampadas_do_dia`, pelo DNA ou pelas colunas; `ata_do_dia` incorpora a lista. A forma individual contém `tipo`, `hora`, `acesa`, `luz` e `tipo_real`; `acesa` pode ser indeterminada (`app/ponto/services/cartorio.py:565`; `app/ponto/services/cartorio.py:638`; `app/ponto/services/cartorio.py:677`; `app/ponto/services/cartorio.py:688`; `app/ponto/services/cartorio.py:704`).
- **Modificadores:** dispensa por ausência/justificativa acrescenta a dispensa; teto converte apagadas ainda não exigíveis em indeterminadas (`app/ponto/services/cartorio.py:773`; `app/ponto/services/cartorio.py:873`).
- **Persistência:** a porta da célula recebe/copia a ata e a salva (`app/ponto/portas/celula.py:986`; `app/ponto/portas/celula.py:1027`). A montagem da grade também chama o mesmo produtor numa cópia em memória (`app/escala/utils.py:1886`).

**5. Índice, ordem e janela**

**A ordem atual é global por PK**, mesmo juntando perguntas de d e d+1: a query ordena, os buckets preservam seus elementos, a junção é novamente ordenada e a seleção usa `ps[-1]`. Não achei inversão introduzida pelo agrupamento (`app/chamados/services/tarefa_do_dia.py:280`; `app/chamados/services/tarefa_do_dia.py:282`; `app/chamados/services/tarefa_do_dia.py:319`; `app/chamados/services/tarefa_do_dia.py:249`). Igualdade textual/comportamental contra o pai: **[nao verificado]**.

Os consumidores de lote encontrados são os testes, com datas internas, além do caso deliberadamente inválido. A casca abre janela exatamente no dia solicitado (`app/chamados/tests/test_e2_tarefa_do_dia.py:427`; `app/chamados/tests/test_e2_tarefa_do_dia.py:503`; `app/chamados/tests/test_e2_tarefa_do_dia.py:520`; `app/chamados/services/tarefa_do_dia.py:338`). A alimentação adjacente interna chama **`instancias`**, não `tarefa`, portanto não é bloqueada pelo `ValueError` (`app/chamados/services/tarefa_do_dia.py:318`).

Chamadas legítimas da sonda externa ou de futuros leitores fora da janela: **[nao verificado]**.

**6. Os testes mordem? Mutante sobrevivente** [nao verificado]

**Mordem os resultados locais declarados**, com a ressalva de que o caso dos dois noturnos exige uma ambiguidade indevida para pergunta recém-criada (`app/chamados/tests/test_e2_tarefa_do_dia.py:338`; `app/chamados/tests/test_e2_tarefa_do_dia.py:355`; `app/chamados/models.py:2477`).

Dois sobreviventes por **análise estática da bateria deste arquivo**:

- Trocar a guarda por `if dia > self.fim`. Retira a proteção de `d < ini`, preservando o único teste externo à janela, que solicita D+1 (`app/chamados/services/tarefa_do_dia.py:315`; `app/chamados/tests/test_e2_tarefa_do_dia.py:431`).
- Trocar `pos != 'inicio'` por `pos == 'fim'`. Perde o tratamento de pausas noturnas; os cadastros noturnos da bateria têm somente entrada e saída (`app/chamados/services/tarefa_do_dia.py:111`; `app/chamados/tests/test_e2_tarefa_do_dia.py:169`; `app/chamados/tests/test_e2_tarefa_do_dia.py:347`).

Passagem efetiva da bateria inteira com esses mutantes: **[nao verificado]**.

**7. Checklist**

| Verificação | Resultado |
|---|---|
| **Segundo juiz** | O juiz agregado está autorizado/registrado. Entretanto, `dono_da_pergunta` volta a inferir o dia já entregue pela autoridade de turno, produzindo a divergência demonstrada (`app/core/juizes.py:285`; `app/chamados/services/tarefa_do_dia.py:101`; `app/escala/servico_jornada.py:487`). |
| **Segundo escritor** | Não achei escritor persistente na E2: a carga consulta e monta estruturas em memória (`app/chamados/services/tarefa_do_dia.py:277`; `app/chamados/services/tarefa_do_dia.py:307`; `app/chamados/services/tarefa_do_dia.py:320`). |
| **Relógio fora da borda** | A E2 passa `agora` explicitamente. Permanece a pendência conhecida: expiração produzida com `date.today()` e lida com `agora.date()`, sem conversão local nesses pontos (`app/chamados/services/tarefa_do_dia.py:295`; `app/chamados/services/tarefa_do_dia.py:303`; `app/chamados/veredito_lavrado.py:112`; `app/chamados/veredito_lavrado.py:182`). |
| **Derivação paralela** | A classificação primeiro/último/meio está repetida em `_posicoes` e `_marcos_faltantes`; sua introdução neste delta é **[nao verificado]** (`app/chamados/services/tarefa_do_dia.py:57`; `app/chamados/services/declaracao_texto.py:123`). |
| **Selo/RED sem mordida** | Limite inferior da janela e pausas noturnas ficam descobertos pelos mutantes acima; o selo dos noturnos também cristaliza a premissa antiga (`app/chamados/tests/test_e2_tarefa_do_dia.py:431`; `app/chamados/tests/test_e2_tarefa_do_dia.py:347`; `app/chamados/tests/test_e2_tarefa_do_dia.py:355`). |
| **ESMERIL-SEMÂNTICA** | `conversa=False` mistura fato moderno e conversa legada; `data_evento` tem contrato atual único, mas a E2 o trata como duplo; `aguardando_documento` tem dono definido, com leitor do app ainda preso ao status morto (`app/chamados/migrations/0051_decisao_pergunta.py:5`; `app/chamados/models.py:2477`; `app/chamados/services/tarefa_do_dia.py:104`; `app/ponto/models.py:701`; `app/api/views.py:1087`). |

## 07/11 04:24 · sessao 85f50560761b · - · - · origem=Ronald (tmux)

Achado principal: o teste novo de dois noturnos cristaliza a semântica antiga. Ele cria pergunta nova com `data_evento=D22` pela porta `abrir` e exige ambiguidade; essa porta já define o parâmetro como dia do turno e vincula a célula desse dia. Portanto a pergunta recém-criada é decidível pelo contrato atual, independentemente da existência de passivo antigo. (`app/chamados/tests/test_e2_tarefa_do_dia.py:107-111,346-355`; `app/chamados/models.py:2477-2484,2528-2530`.)

A premissa “disputa_emissao ainda grava dia civil” está desatualizada: o emissor chama `resolver_data_evento`, que entrega o dia do turno pela autoridade `dia_do_turno_da_batida`. (`app/chamados/services/disputa_emissao.py:1092-1104`; `app/escala/servico_jornada.py:474-488`.) O código reconhece acervo antigo e contém migração própria, com exclusões explícitas; isso demonstra a possibilidade de passivo, sem demonstrar sua quantidade nem aplicação em produção. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:8-20`; `app/chamados/services/fio.py:859-876`; estado de produção `[nao verificado]`.)

Mutantes sobreviventes por inspeção da bateria do arquivo:

- Trocar `if not (self.ini <= dia <= self.fim)` por `if dia > self.fim`: o novo teste só tenta `D+1`; as outras chamadas usam dias internos ou a casca que abre janela exata. O limite inferior fica sem mordida. (`app/chamados/services/tarefa_do_dia.py:315-316`; `app/chamados/tests/test_e2_tarefa_do_dia.py:427-432,491-525`.)
- Trocar `pos != 'inicio'` por `pos == 'fim'`: todos os cadastros noturnos da bateria têm apenas entrada e saída, sem pausa. Assim, perder o tratamento de intervalo noturno não altera esses resultados. (`app/chamados/services/tarefa_do_dia.py:111-115`; `app/chamados/tests/test_e2_tarefa_do_dia.py:168-177,346-355`.)

Execução desses mutantes e da suíte `[nao verificado]`; não executei testes que escrevem. [nao verificado]

Checklist:

- O juiz agregado está registrado; a convivência temporária com os cinco respondedores está declarada. (`app/core/juizes.py:150-155,285`; `app/docs/JUIZES.md:73-80`.)
- Há reinterpretacão paralela do dia já produzido pela autoridade: `dono_da_pergunta` volta a inferir o dono por geometria, enquanto o nascimento atual recebe dia do turno. (`app/chamados/services/tarefa_do_dia.py:101-115`; `app/escala/servico_jornada.py:474-488`; `app/chamados/models.py:2477-2484`.)
- Há também cópia da derivação de posição primeiro/último/meio, já presente em `_marcos_faltantes`; sem o pai, o nascimento dessa cópia no delta é `[nao verificado]`. (`app/chamados/services/tarefa_do_dia.py:54-64`; `app/chamados/services/declaracao_texto.py:110-128`.)
- Não achei escritor persistente em `tarefa_do_dia`: os trechos examinados fazem consultas e montam estruturas em memória. (`app/chamados/services/tarefa_do_dia.py:277-304,307-321`.)
- O relógio é passado explicitamente aos leitores usados pela carga; permanece a diferença entre `agora.date()` no consumidor e `date.today()` na produção da expiração, já deixada em fila pela triagem. (`app/chamados/services/tarefa_do_dia.py:295,303,310`; `app/chamados/veredito_lavrado.py:112,181-185`.)
- A documentação do teste ainda equipara ata sem lâmpadas a ata nunca lavrada; o leitor antigo reconhece expressamente ata agregada sem lâmpadas. (`app/chamados/tests/test_e2_tarefa_do_dia.py:38`; `app/chamados/services/declaracao_texto.py:112-115`.)

## 07/11 04:24 · sessao dca9a40ed118 · - · - · origem=Ronald (tmux)

Q4 — A frase “ata sem lâmpadas = ata não lavrada” é excessiva: o leitor reconhece explicitamente **ata antiga agregada**, já lavrada, sem identidade individual dos marcos. `nao_sei` continua coerente; a explicação causal é que está errada (`app/escala/services/leitor_celula.py:146-172`; `app/chamados/services/tarefa_do_dia.py:182-184`; `app/chamados/tests/test_e2_tarefa_do_dia.py:38`).

- Produtor das lâmpadas: `lampadas_do_dia`, ramo DNA e ramo alternativo; `ata_do_dia` incorpora a lista na ata (`app/ponto/services/cartorio.py:565`, `app/ponto/services/cartorio.py:622-704`). O vocabulário do DNA é E/S e contempla duas pausas (`app/escala/models.py:1652-1669`).
- O ramo alternativo aceita o tipo/hora do marco ou da coluna, portanto pode produzir outros tipos (`app/ponto/services/cartorio.py:682-692`). Existem colunas I para intervalo livre; não achei trajeto comprovado de trabalho com **somente** I e nenhuma E/S (`app/escala/utils.py:1383-1390`, `app/escala/utils.py:1823-1829`); ocorrência real `[nao verificado]`.
- Intermitente usa tipos E/S e horas posicionais `·I1`, `·I2`; não confundir esses rótulos com tipo I (`app/escala/utils.py:1392-1401`).
- Ausência total aprovada altera o dia para `ausencia` e limpa suas colunas; esse tipo satisfaz `dia_sem_trabalho`, então não cai no novo retorno antecipado (`app/escala/utils.py:1220`, `app/escala/utils.py:1251-1253`; `app/chamados/juizes.py:429`; `app/chamados/services/tarefa_do_dia.py:181-185`).
- Há modificadores da lista: dispensa por ausência/justificativa altera `acesa`, `luz`, `dispensada`; teto altera apagadas para `acesa=None, pendente=True` (`app/ponto/services/cartorio.py:766-779`, `app/ponto/services/cartorio.py:873-883`).
- Persistência passa por `lavrar_veredito`, que recebe e copia a ata (`app/ponto/portas/celula.py:986-1003`, `app/ponto/portas/celula.py:1027-1032`). A montagem na grade usa cópia em memória e o mesmo produtor, sem persistência (`app/escala/utils.py:1886-1897`). `carimbar_precedencia` preserva a lista e altera apenas `precedencia` (`app/ponto/portas/celula.py:1071-1082`).

Q5 — A implementação atual preserva ordem global por PK mesmo ao juntar perguntas de d e d+1: consulta ordenada, agrupamento por `(colaborador_id,data_evento)`, concatenação seguida de `sorted(key=pk)`; a escolha continua sendo `ps[-1]` (`app/chamados/services/tarefa_do_dia.py:277-282`, `app/chamados/services/tarefa_do_dia.py:319`, `app/chamados/services/tarefa_do_dia.py:249`). Igualdade com o pai `[nao verificado]`.

Os consumidores de lote encontrados estão nos testes: o caso explícito fora da janela e os dois selos com datas internas; a casca abre janela exatamente no dia solicitado (`app/chamados/tests/test_e2_tarefa_do_dia.py:427-432`, `app/chamados/tests/test_e2_tarefa_do_dia.py:491-505`, `app/chamados/tests/test_e2_tarefa_do_dia.py:507-525`; `app/chamados/services/tarefa_do_dia.py:337-341`). Sonda externa e futuros consumidores `[nao verificado]`.

A alimentação interna adjacente usa `instancias(d-1/d/d+1)`, não `tarefa(d-1/d+1)`, portanto não é barrada pelo novo `ValueError` (`app/chamados/services/tarefa_do_dia.py:307-318`).

Q6 — Mutante sobrevivente por inspeção: trocar a condição da janela para `if dia > self.fim`, deixando d<ini passar. O único teste dessa guarda solicita D+1; os demais lotes encontrados usam datas internas (`app/chamados/services/tarefa_do_dia.py:315-316`; `app/chamados/tests/test_e2_tarefa_do_dia.py:427-432`, `app/chamados/tests/test_e2_tarefa_do_dia.py:503-505`, `app/chamados/tests/test_e2_tarefa_do_dia.py:520-523`). Execução da bateria com esse mutante `[nao verificado]`.

## 07/11 04:24 · sessao 8cd90ff2efca · - · - · origem=Ronald (tmux)

Q3 — censo e conclusões, somente leitura:

- **Escritores atuais de `conversa` são coerentes com `True = sem identidade de fato`**: `abrir` passa `False`, `abrir_conversa` passa `True` e data civil de emissão; `_nascer` grava a coluna (`app/chamados/models.py:2483`, `app/chamados/models.py:2494`, `app/chamados/models.py:2578`). Chamadores de conversa: reabertura administrativa, até com motivo escolhido no catálogo (`app/chamados/services/acoes_disputa.py:247`, `app/chamados/services/acoes_disputa.py:260`); emissão sem dia resolvido (`app/chamados/services/disputa_emissao.py:420`); `ancora_ausente` (`app/chamados/services/disputa_emissao.py:1251`).
- **O inverso `False = pergunta do fato` não vale para o acervo**. A migration introduz a coluna com `default=False` para perguntas antigas (`app/chamados/migrations/0051_decisao_pergunta.py:99`), preenche instância apenas por motivo (`app/chamados/migrations/0051_decisao_pergunta.py:26`) e confessa que conversa antiga é indistinguível (`app/chamados/migrations/0051_decisao_pergunta.py:5`). Assim, conversa histórica com motivo `orfao_14h` e data coincidente ainda entra no filtro novo (`app/chamados/services/tarefa_do_dia.py:278`); pode ocupar a posição de pergunta do marco e gerar responder/validar segundo seus atos (`app/chamados/services/tarefa_do_dia.py:198`, `app/chamados/services/tarefa_do_dia.py:249`). A existência desse caso no dado de produção fornecido é `[nao verificado]`.
- **O teste novo não cobre esse legado**: cria pergunta pela porta moderna e força `conversa=True` (`app/chamados/tests/test_e2_tarefa_do_dia.py:341`, `app/chamados/tests/test_e2_tarefa_do_dia.py:342`). O backfill de decisões apenas lista colisões, igualmente filtrando `conversa=False`; não classifica conversas antigas (`app/chamados/management/commands/backfill_decisoes_pergunta.py:74`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:84`).
- **Leitores explícitos da coluna**: identidade `linhas_dos_fatos` (`app/chamados/models.py:2466`); trava/dedup no nascimento (`app/chamados/models.py:2508`, `app/chamados/models.py:2528`); derivação de instância na inserção (`app/chamados/models.py:2972`); censo de colisões (`app/chamados/management/commands/backfill_decisoes_pergunta.py:74`); carga E2 (`app/chamados/services/tarefa_do_dia.py:278`). O modelo documenta que a coluna identifica a porta que fez nascer a pergunta (`app/chamados/models.py:2247`).

`Ausencia`:

- **`aguardando_documento` corresponde à obrigação de documento do colaborador**, como a E2 agora supõe: choices nomeiam explicitamente os donos (`app/ponto/models.py:701`), `estado_inicial` retorna documento quando o tipo exige e não há anexo (`app/ponto/services/ausencia.py:779`, `app/ponto/services/ausencia.py:795`). Criação chama esse juiz e grava o estado (`app/ponto/services/ausencia.py:333`, `app/ponto/services/ausencia.py:344`); reabertura chama o mesmo juiz (`app/ponto/services/ausencia.py:1119`); alteração de tipo rejulga abertos (`app/ponto/services/ausencia.py:541`, `app/ponto/services/ausencia.py:543`).
- **Transição do status**: `mudar_estado` escreve em `ausencia.status` e salva (`app/ponto/services/ausencia.py:824`, `app/ponto/services/ausencia.py:835`). Anexo move documento→decisão (`app/ponto/services/ausencia.py:873`); escalonador vencido move documento→decisão do DP (`app/ponto/services/ausencia.py:1002`); `_reabrir` grava o estado calculado (`app/ponto/services/ausencia.py:1141`). Migration histórica classifica pendentes pelo anexo e tipo (`app/ponto/migrations/0057_a4_backfill_pendente.py:35`, `app/ponto/migrations/0057_a4_backfill_pendente.py:43`). Materialização ATESTADO deixa status vazio para o juiz, enquanto FALTA_DIA é aprovada (`app/chamados/services/materializacao.py:594`).
- **Leitores coerentes**: `pode_aprovar` aceita só `aguardando_decisao` (`app/ponto/models.py:741`); `esta_aberta` usa `ABERTAS` (`app/ponto/models.py:719`); contadores discriminam estados e vencimento (`app/ponto/services/ausencia.py:963`); cobrança faz push ao colaborador pedindo documento (`app/ponto/services/ausencia.py:1019`); view informa falta de documento (`app/ponto/views.py:2555`); reversão comunica explicitamente colaborador versus DP (`app/ponto/views.py:2591`); endpoint de anexo aceita ambos os estados abertos (`app/api/views.py:3117`). A E2 trata documento nos dois lugares: absorção (`app/chamados/services/tarefa_do_dia.py:164`) e cobertura (`app/chamados/services/tarefa_do_dia.py:225`).
- **Há leitor incoerente anterior ao delta**: o payload dos chamados do app só oferece “Anexar comprovante” quando `Ausencia.status == 'pendente'` (`app/api/views.py:1087`, `app/api/views.py:1091`), embora `pendente` não esteja nos choices atuais (`app/ponto/models.py:701`) nem em `ABERTAS` (`app/ponto/services/ausencia.py:765`). Uma ausência válida `aguardando_documento` não recebe esse bloco do payload; a própria intenção do bloco está descrita no comentário (`app/api/views.py:1071`). Isso não muda o dono semântico, mas impede afirmar alinhamento de todos os leitores.
- **Não achei outro status válido não final cuja bola seja do colaborador**: vocabulário tem documento, decisão, aprovada e rejeitada (`app/ponto/models.py:701`); `ABERTAS` contém somente documento e decisão (`app/ponto/services/ausencia.py:765`). Anexo também é permitido durante `aguardando_decisao`, mas possibilidade de anexar não equivale a obrigação pendente (`app/api/views.py:3117`).
- **Cuidado no “escritor único” absoluto**: além de nascimento e migrations, existe baixa de férias por `QuerySet.update(status='rejeitada')` (`app/ponto/services/ausencia.py:1283`); esse bypass não escreve `aguardando_documento`, nem demonstra um segundo escritor introduzido pela E2 (`app/ponto/services/ausencia.py:1283`).

## 07/11 04:12 · sessao 47a2809364aa · FIO-E0-V4-E2 · verificador · origem=Code

**Reprovo a E2 como concluída:** há atribuição de pergunta ao turno errado, desconhecimento devolvido como ausência de pendência e perda da distinção entre pendência de documento e decisão do DP. (`app/chamados/services/tarefa_do_dia.py:111`, `app/chamados/services/tarefa_do_dia.py:173`, `app/chamados/services/tarefa_do_dia.py:216`; `app/ponto/services/ausencia.py:779`)

A cópia não contém metadados Git acessíveis; identidade do HEAD e comparação com o pai: **[não verificado]**. Não executei a bateria Django nem consultei banco/logs; execução integral, RED anterior ao código, medida de frota e divergências de produção: **[não verificado]**. Executei somente corpos extraídos por AST, em memória, para conferir contraexemplos; nenhum arquivo foi criado, alterado ou apagado. **[não verificado por registro em arquivo]** [nao verificado]

**1. Procedência dos campos e compatibilidade de significado.**

O quadro registra escritores e leitores localizados e conferidos. A enumeração exaustiva de **todos** os leitores indiretos da árvore não ficou certificada: **[não verificado]**.

| Campo/valor | Quem escreve | Quem lê e significado encontrado |
|---|---|---|
| `CelulaDia.ata['lampadas']`: `acesa`, `tipo`, `hora` | `lampadas_do_dia` produz a lista; `ata_do_dia` a incorpora; a porta da célula persiste a ata. O teto pode transformar lâmpadas pendentes em `acesa=None`. (`app/ponto/services/cartorio.py:565`, `:704`, `:873`; `app/ponto/portas/celula.py:1027`) | E2 lê a ordem, tipo E/S, hora local e valor de `acesa`; lastro e leitor da célula também consomem a lista. **`acesa=True` não significa necessariamente batida ocorrida:** ausência/justificativa pode dispensar o marco, com `luz=None`. O docstring novo simplifica indevidamente isso para “aconteceu”; o comportamento de não cobrar o marco dispensado é compatível. (`app/chamados/services/tarefa_do_dia.py:10`, `:52`, `:89`, `:202`; `app/ponto/services/lastro.py:89`; `app/escala/services/leitor_celula.py:115`; `app/ponto/services/cartorio.py:752`, `:774`) |
| `ata['tipo_dia']` | A ata copia o tipo produzido pela grade; a porta da célula a grava. (`app/ponto/services/cartorio.py:704`; `app/ponto/portas/celula.py:1028`) | E2 chama `dia_sem_trabalho` e `ausencia_parcial_cobra_o_resto`; o leitor da célula transporta o tipo. Tipo preenchido diferente de `trabalho` cala, exceto ausência parcial; não é uma data nem um instante. (`app/chamados/services/tarefa_do_dia.py:178`; `app/chamados/juizes.py:411`, `:423`; `app/escala/services/leitor_celula.py:380`) |
| `CelulaDia.trabalha` | Geração, regeneração, edição humana e reversão passam pela porta da célula. (`app/ponto/portas/celula.py:259`, `:272`, `:603`, `:788`, `:851`, `:868`) | O juiz de dia sem trabalho distingue `False` de `None`: `False` é folga; `None` é indeterminado e cobra. E2 delega corretamente esse predicado. (`app/escala/models.py:1686`; `app/chamados/juizes.py:419`, `:429`; `app/chamados/services/tarefa_do_dia.py:178`) |
| `dna['marcos']` | `montar_dna` produz horários `HH:MM`; as operações da porta persistem o blob. (`app/ponto/management/commands/gerar_celulas.py:79`, `:83`, `:100`; `app/ponto/portas/celula.py:272`, `:604`, `:870`) | `EscalaColaborador.marcos_do_dia`, `marcos_turno`, cartório e E2 pelo teto. São horas de parede ancoradas no **dia do turno**; `marcos_turno` constrói instantes e avança a data quando necessário. Há incompatibilidade de cardinalidade: o DNA admite duas pausas, mas `marcos_turno` monta somente a primeira. (`app/escala/models.py:1652`, `:1124`; `app/escala/servico_jornada.py:69`, `:74`; `app/chamados/services/tarefa_do_dia.py:299`) |
| `PerguntaDisputa.data_evento` | Nascimento em `_nascer`; preenchimento no reapontamento; correção em `rechavear_data_evento`. A porta atual declara **dia do turno**; conversa grava o dia local em que foi aberta. (`app/chamados/models.py:2477`, `:2494`, `:2578`; `app/chamados/services/fio.py:838`, `:898`) | E2 usa em `dono_da_pergunta`; materialização e fio também o tratam como data do fato. **Não existe significado único efetivo:** o caminho de intervalo ainda entrega dia civil do marco, descrito abaixo. (`app/chamados/services/tarefa_do_dia.py:106`; `app/chamados/services/fio.py:45`; `app/chamados/services/materializacao.py:345`; `app/ponto/management/commands/detectar_intervalo_ausente.py:82`) |
| `.instancia_marco` e `.motivo` | O nascimento grava motivo e deriva a instância pelo catálogo quando vazia; a migração E1 também preenche instância. (`app/chamados/models.py:2578`, `:2972`; `app/chamados/migrations/0051_decisao_pergunta.py:27`) | E2 usa instância ou posição derivada do motivo. **Posição e ocorrência não coincidem:** o escritor usual produz `int_ida`, enquanto E2 cria `int_ida:2`; a associação posterior continua indexada pela posição. (`app/chamados/services/declaracao_texto.py:52`, `:55`; `app/chamados/services/tarefa_do_dia.py:62`, `:96`, `:192`, `:227`) |
| `.via_resolucao` | Porta do carimbo, descarimbo em lote, `encerrar_por_fato_no_espelho` e relavra de via. (`app/chamados/models.py:2662`, `:2801`, `:2811`; `app/chamados/services/relavra_via.py:123`) | E2 exclui vias fora do fato; o juiz de materialização interpreta vias com/sem testemunha. É vocabulário de resolução, não prova isolada de efeito físico. (`app/chamados/services/tarefa_do_dia.py:183`; `app/chamados/juizes.py:648`) |
| `.respondida_em` | Resposta normal, automática, recuperação pela trilha, reabertura/ressuscitação e carimbo automático; há restauração em memória. (`app/chamados/services/disputa_emissao.py:1346`, `:1462`, `:1516`, `:1537`; `app/chamados/models.py:1515`, `:2691`, `:2722`) | E1 verifica correspondência entre projeção e linha; `marcos_do_lote` agrega o instante; leitores antigos continuam usando a projeção. É instante de resposta, podendo ser retirado na projeção sem apagar a decisão histórica. (`app/chamados/models.py:2983`, `:3416`; `app/chamados/veredito_lavrado.py:166`; `app/chamados/services/disputa_emissao.py:1463`) |
| `.validada_em` | Validação, devolução, resposta nova que desfaz validação, reabertura/ressuscitação e carimbo automático. (`app/chamados/services/validacao.py:134`, `:199`; `app/chamados/services/disputa_emissao.py:1358`, `:1462`; `app/chamados/models.py:1515`, `:2694`) | E1 confere correspondência com o ato; leitores antigos filtram validação nula. E2 não toma esse carimbo como decisão vigente: usa a sequência. (`app/chamados/models.py:3420`, `:2895`; `app/chamados/services/tarefa_do_dia.py:137`, `:279`) |
| `.materializada_em` | `_escrever_carimbo` e descarimbo em lote, pelas portas do modelo. (`app/chamados/models.py:2660`, `:2801`) | **A premissa do pedido está errada:** a função de passivo E1 não lê esse campo; E2 o alcança por `marcos_do_lote`. Juiz e agrupamento legado de absorção também o leem. O carimbo registra processamento/resolução e pode existir sem batida. (`app/chamados/models.py:3409`, `:3424`; `app/chamados/services/tarefa_do_dia.py:282`; `app/chamados/veredito_lavrado.py:162`; `app/chamados/juizes.py:648`; `app/chamados/services/materializacao.py:624`) |
| `.chamado_destino_id` | Nascimento/dedup e reapontamento; há simulações temporárias em memória para julgar a costura. (`app/chamados/models.py:2550`, `:2578`, `:354`; `app/chamados/services/fio.py:777`, `:849`) | E2 chama `chamado_que_lista`, que usa destino ou chamado da disputa. É identidade da âncora de apresentação, sem unidade temporal. (`app/chamados/services/tarefa_do_dia.py:131`, `:290`; `app/chamados/services/fio.py:116`) |
| `DecisaoPergunta.ato/ato_em/ato_texto/absorcao_ausencia` | Porta física `registrar`; produtores: resposta, validação/devolução, reabertura/recusa, silêncio, retratação e absorção. (`app/chamados/models.py:3428`, `:1869`, `:1989`; `app/chamados/services/disputa_emissao.py:1369`, `:1478`, `:1519`, `:1540`; `app/chamados/services/validacao.py:139`, `:205`; `app/chamados/services/materializacao.py:560`) | E2 ordena por `(ato_em,pk)`, escolhe a última e lê o status atual da ausência vinculada. A tela lê respostas anteriores pela mesma sequência. `ato_em` é instante; `ato_texto` é o texto tocado pelo ato; absorção é ligação persistida, não agrupamento por coincidência de horário. (`app/chamados/services/tarefa_do_dia.py:137`, `:158`, `:275`; `app/chamados/models.py:2985`, `:3338`, `:3374`) |
| `Ausencia.status/data/data_fim/tipo` | Criação/edição/mudança de estado na porta de ausência; `Ausencia.save` também deriva `data_fim`. (`app/ponto/services/ausencia.py:341`, `:498`, `:824`; `app/ponto/models.py:907`) | Cobertura lê tipo, estado e intervalo **inclusivo**, com aberto somente para tipos próprios; E2 lê status para definir ação. **Há divergência de dono:** `aguardando_documento` pertence ao colaborador, mas E2 o transforma em `decidir_ausencia/admin`. (`app/ponto/turnos.py:2511`, `:2771`, `:2780`; `app/ponto/services/ausencia.py:779`; `app/chamados/services/tarefa_do_dia.py:158`, `:216`) |
| `Batida.pergunta_origem/retratada_em` | Registro/anotação pela porta; materialização vincula a pergunta; retratação carimba e desfazer limpa o instante. (`app/ponto/registro_batida.py:94`, `:235`, `:274`; `app/chamados/services/materializacao.py:937`; `app/ponto/services/retratar_humano.py:207`) | E2 lê origem dentro de `batidas_apuraveis`; juiz lê batidas plantadas; staleness agrega retratação. `retratada_em=NULL` inclui a batida na apuração; origem liga a pergunta, mas não prova sozinha o slot correto. (`app/chamados/services/tarefa_do_dia.py:287`; `app/ponto/turnos.py:2876`; `app/chamados/juizes.py:719`; `app/chamados/veredito_lavrado.py:140`) |
| `ChamadoColaborador.status_local/silenciado_ate` | Estado pela porta `_gravar_estado`; silêncio e levantamento por `arquivo`. (`app/chamados/models.py:199`, `:226`; `app/chamados/services/arquivo.py:166`, `:201`) | E2 delega à fila: estado de cobrança e exclusão quando silêncio é **estritamente posterior** a `agora`; igualdade já permite retorno. Silêncio é véu de apresentação, distinto do estado. (`app/chamados/services/tarefa_do_dia.py:292`; `app/chamados/services/painel.py:188`; `app/chamados/services/arquivo.py:154`) |

Os três campos da lavra estão discriminados no item 7. (`app/chamados/veredito_lavrado.py:194`)

**O erro concreto de `data_evento`:**

- O escritor atual `abrir` exige dia do turno, e `rechavear_data_evento` existe para normalizar o legado para esse significado. (`app/chamados/models.py:2477`; `app/chamados/services/fio.py:859`)
- Continua havendo alimentação civil: detector de intervalo grava `localtime(marco).date()`; emissão lê essa data e apenas a envolve em `dia_do_turno_da_celula`, que expressamente não converte. (`app/ponto/management/commands/detectar_intervalo_ausente.py:78`; `app/chamados/services/disputa_emissao.py:1037`; `app/ponto/turnos.py:2939`)
- Contraexemplo: turnos noturnos consecutivos de 21 e 22/10, ambos 22h–06h; pergunta de fim gravada corretamente com `data_evento=22/10`. Como o fim de 21 cai civilmente em 22 e o fim de 22 cai em 23, `dono_da_pergunta` devolve **21/10**. A heurística não consulta a célula/proveniência da pergunta; portanto **não cobre os dois significados sem atribuição errada**. (`app/chamados/services/tarefa_do_dia.py:106`, `:109`, `:114`)

Há ainda derivação paralela da identidade: E1 exclui `conversa=True` em `linhas_dos_fatos`; E2 recompõe o filtro sem essa exclusão e recupera posição pelo motivo. Uma conversa com motivo de marco pode ser tratada como pergunta factual. (`app/chamados/models.py:2465`, `:2487`; `app/chamados/services/tarefa_do_dia.py:94`, `:182`)

**2. Pureza, escrita e relógio.**

O **núcleo** `_tarefa_pura` recebe fatos carregados e não contém escrita de modelo. A função pública consulta banco; logo “pura” é defensável como decisão sem efeitos sobre fatos carregados, mas não como função matemática exclusivamente de `(colab,dia,agora)`. Banco e fuso ativo são entradas implícitas. (`app/chamados/services/tarefa_do_dia.py:167`, `:267`, `:318`, `:72`; `app/escala/servico_jornada.py:81`)

Não encontrei relógio corrente nem escrita persistente nos corpos alcançados de `vinculo_do_dia`, `marcos_turno`, `_pelo_efeito`, `batidas_apuraveis`, `chamado_que_lista`, `dia_sem_trabalho`, `ausencia_parcial_cobra_o_resto`, `texto_vazio` e `instancia_do_motivo`. (`app/escala/alimentacao.py:269`; `app/escala/servico_jornada.py:61`; `app/ponto/turnos.py:2511`, `:2856`; `app/chamados/services/fio.py:116`; `app/chamados/juizes.py:401`, `:414`, `:1251`; `app/chamados/services/declaracao_texto.py:55`)

`qs_na_fila` possui fallback de relógio, mas E2 fornece `agora`; `esta_stale` também possui fallback, igualmente evitado nesse caminho. (`app/chamados/services/painel.py:190`; `app/chamados/veredito_lavrado.py:181`; `app/chamados/services/tarefa_do_dia.py:284`, `:292`, `:322`)

Há memorização **por instância da carga**, em `_inst` e `_cob`; não localizei memorização global nesse caminho. Assim, a descrição literal “nenhum cache” omite essas memórias locais. (`app/chamados/services/tarefa_do_dia.py:8`, `:296`; `app/ponto/turnos.py:3068`)

**3. Equivalência da carga com cobertura e teto.**

**Cobertura:** dentro da mesma janela e sobre os mesmos fatos, os dois caminhos usam exatamente `_pelo_efeito`, `_q_cobertura` e `_cobertura_pura`. Preservam filtro, intervalo inclusivo e desempate pelo maior `(data,pk)`; a ordenação adicional por colaborador somente agrupa o lote. (`app/ponto/turnos.py:2753`, `:2761`, `:2780`, `:3072`)

**Teto:** compartilham o corpo, a ordem de `marcos_turno` e o limite inclusivo `marco + teto <= agora`. Entretanto, **a alimentação não é exatamente igual**: `marcos_vencidos` passa a célula recebida à escolha do vínculo, mas não a passa a `marcos_com_teto`; a carga passa seu snapshot de células. (`app/ponto/services/cartorio.py:841`, `:843`, `:858`; `app/ponto/turnos.py:3061`)

Contraexemplo construído: célula/carga em memória com DNA A; banco alterado para DNA B. A carga usa A; a casca pode escolher o vínculo com a célula A e buscar os marcos B no banco. Isso refuta equivalência incondicional da alimentação, sem demonstrar ocorrência em produção. (`app/escala/models.py:1119`, `:1122`, `:1124`; `app/ponto/services/cartorio.py:843`; `app/ponto/turnos.py:3062`)

O pad é `[ini−2,fim+1]`, mas a cobertura é expandida apenas em `[ini,fim]`. A API não impede consulta fora da janela: carga `[21,21]`, tarefa de 22 com célula no pad e ausência em 22 pode cobrar sem enxergar essa ausência. É uso fora do contrato documentado, aceito silenciosamente pela implementação. (`app/ponto/turnos.py:1847`, `:2977`, `:3073`; `app/chamados/services/tarefa_do_dia.py:302`, `:318`)

Queries constantes também não provam tempo linear no tamanho variável da janela: cada tarefa diária volta a percorrer todas as perguntas carregadas daquele colaborador. A meta de frota `≤1 query/colab-dia` não é assegurada pelo teste existente. (`app/chamados/services/tarefa_do_dia.py:306`; `app/chamados/tests/test_e2_tarefa_do_dia.py:397`)

**4. Checklist das classes de reprovação.**

| Classe | Resultado |
|---|---|
| Segundo juiz de autoridade registrada | Não achei duplicação do predicado de dia sem trabalho, cobertura, teto ou fila: E2 chama essas autoridades. O predicado de posição primeiro/último/meio é repetido; registro dele como juiz próprio: **[não verificado]**. (`app/chamados/services/tarefa_do_dia.py:49`, `:170`, `:292`, `:299`, `:307`; `app/chamados/services/declaracao_texto.py:118`) |
| Segundo escritor fora da porta | Não localizado no arquivo novo; o `out.update` é mutação do dict de saída. (`app/chamados/services/tarefa_do_dia.py:173`, `:176`) |
| Relógio fora da borda | Não localizado no caminho normal E2; existem fallbacks evitados pelo argumento obrigatório. (`app/chamados/services/tarefa_do_dia.py:322`; `app/chamados/services/painel.py:190`; `app/chamados/veredito_lavrado.py:181`) |
| Derivação paralela | Identidade factual é recomposta sem o filtro de conversa; dono do turno é inferido por heurística incompatível com dado normalizado. (`app/chamados/models.py:2466`; `app/chamados/services/tarefa_do_dia.py:99`, `:182`) |
| Campo com dois significados | `data_evento`: turno na porta atual; civil ainda vindo da emissão de intervalo. (`app/chamados/models.py:2477`; `app/chamados/services/disputa_emissao.py:1037`; `app/ponto/management/commands/detectar_intervalo_ausente.py:82`) |
| Desconhecimento tratado como resposta definitiva | Célula existente de trabalho com ata vazia/lâmpadas vazias retorna `instancias=[]`, `nao_sei=False`, razão vazia. O leitor anterior distingue ausência de lâmpadas como desconhecimento. (`app/chamados/services/tarefa_do_dia.py:84`, `:173`, `:193`; `app/chamados/services/declaracao_texto.py:82`, `:112`) |
| “Cada instância” incompleto | A segunda pausa recebe sufixo na saída, mas associação continua por posição; a geometria usada pelo teto também não inclui a segunda pausa do DNA. (`app/chamados/services/tarefa_do_dia.py:62`, `:192`, `:227`; `app/escala/models.py:1652`; `app/escala/servico_jornada.py:74`) |

**5. Bateria: o que responde pela regra e o que apenas acompanha a implementação.**

| Caso | Avaliação |
|---|---|
| 1 — dois marcos | Exercita independência entre marcos e ações distintas após resposta de um deles. (`app/chamados/tests/test_e2_tarefa_do_dia.py:129`) |
| 2 — noturno | Exercita teto por instante, mas aceita os dois significados de `data_evento` como equivalentes e sanciona a heurística. Não enfrenta dois noturnos consecutivos nem pergunta já normalizada. (`app/chamados/tests/test_e2_tarefa_do_dia.py:167`, `:174`, `:189`) |
| 3 — devolução | Exige retorno ao admin após ato de devolução. (`app/chamados/tests/test_e2_tarefa_do_dia.py:198`) |
| 4 — ausência | Exercita não rejeitada/aprovada/rejeitada, mas somente ausência de um dia e sem `aguardando_documento`. Não prova intervalos, desempate ou parcial. (`app/chamados/tests/test_e2_tarefa_do_dia.py:210`, `:215`, `:233`) |
| 5 — troca | A folga possui lâmpadas vazias; portanto não testa o juiz calando lâmpadas antigas apagadas. (`app/chamados/tests/test_e2_tarefa_do_dia.py:243`) |
| 6 — concorrência | Testa idempotência de atos duplicados inseridos sequencialmente; corrida real: **[não verificado]**. (`app/chamados/tests/test_e2_tarefa_do_dia.py:253`) |
| 7 — efeito | Distingue veredito NÃO, SIM com batida e lavra vencida por alteração de insumo; a lavra fresca é artificialmente futura. (`app/chamados/tests/test_e2_tarefa_do_dia.py:269`, `:108`) |
| 8 — passivo | Verifica projeção respondida sem linha, exigindo desconhecimento. (`app/chamados/tests/test_e2_tarefa_do_dia.py:325`) |
| 9 — desligamento | Verifica pendência histórica do admin até silêncio. (`app/chamados/tests/test_e2_tarefa_do_dia.py:336`) |
| 10 — exportada | Não cria competência exportada: avança quarenta dias e verifica ausência de SQL da folha. O título excede a evidência do caso. (`app/chamados/tests/test_e2_tarefa_do_dia.py:349`) |
| 11 — via sem testemunha | Protege contra rederivar materialização somente pela existência de batida. Não testa expiração civil da lavra. (`app/chamados/tests/test_e2_tarefa_do_dia.py:299`, `:108`) |

O erro de regra mais direto é o dono da ausência: o texto da bateria universaliza “DP decide” para ausência não rejeitada, enquanto o juiz de estado distingue documento devido pelo colaborador. E2 reproduz essa universalização nos dois ramos, cobertura e absorção. (`app/chamados/tests/test_e2_tarefa_do_dia.py:5`; `app/ponto/services/ausencia.py:779`; `app/chamados/services/tarefa_do_dia.py:158`, `:216`)

Mutantes que a inspeção indica deixarem as expectativas atuais intactas — execução desses mutantes: **[não verificado]**:

- `_fila = set()` sempre: fixtures não criam âncoras e o helper de comparação descarta `porta`. (`app/chamados/tests/test_e2_tarefa_do_dia.py:95`, `:124`; `app/chamados/services/tarefa_do_dia.py:292`)
- Remover verificação de `janela_expira_em`: o helper sempre a grava como `None`. (`app/chamados/tests/test_e2_tarefa_do_dia.py:108`; `app/chamados/veredito_l

[truncado em 20000 de 27153 caracteres]

## 07/11 04:08 · sessao dd6f157c1bd2 · - · - · origem=Ronald (tmux)

Achados para consolidação, todos obtidos só por leitura; executei apenas dois corpos extraídos por AST em `python3 -B`, sem Django, banco ou escrita. A comparação com o pai permanece [não verificado]. [nao verificado]

1. **`dono_da_pergunta` atribui perguntas normalizadas ao turno errado.** A porta atual recebe explicitamente dia do turno, e a porta de migração corrige o legado para esse significado. Mesmo assim, a heurística desloca para a véspera quando nela existe aquela posição no dia civil indicado. Para dois noturnos consecutivos, `p.data_evento=05/11`, fim de 04/11 em 05/11 e fim de 05/11 em 06/11, o corpo executado devolveu **04/11**, embora a pergunta atual seja de 05/11. (`app/chamados/models.py:2470`, `app/chamados/models.py:2483`, `app/chamados/services/fio.py:859`, `app/chamados/services/fio.py:898`, `app/chamados/services/tarefa_do_dia.py:106`, `app/chamados/services/tarefa_do_dia.py:111`.)

2. **Ainda há escritor atual de dia civil no campo.** O detector de intervalo escreve `localtime(marco).date()` em `marcos_faltantes[].data`; o emissor lê essa data e chama `_criar_pergunta_se_couber`, envolvendo-a em `dia_do_turno_da_celula`. Esse construtor declara expressamente que não converte nada. A gravação chega a `PerguntaDisputa.abrir(data_evento=...)`. (`app/ponto/management/commands/detectar_intervalo_ausente.py:78`, `app/ponto/management/commands/detectar_intervalo_ausente.py:82`, `app/chamados/services/disputa_emissao.py:1037`, `app/chamados/services/disputa_emissao.py:1043`, `app/ponto/turnos.py:2939`, `app/ponto/turnos.py:2943`, `app/chamados/services/disputa_emissao.py:814`, `app/chamados/services/disputa_emissao.py:821`.)

3. **O caso 2 sanciona a heurística, sem provar identificação correta.** Aceita D21 e D22 como a mesma pergunta de D21 e testa ambiguidade contra um turno diurno em D22. Falta o contraexemplo de dois noturnos consecutivos ou acervo já rechaveado. (`app/chamados/tests/test_e2_tarefa_do_dia.py:160`, `app/chamados/tests/test_e2_tarefa_do_dia.py:174`, `app/chamados/tests/test_e2_tarefa_do_dia.py:189`.)

4. **VALIDAÇÃO lê a lâmpada do juiz; não rederiva “materializou?”.** A carga aceita apenas `sim`/`nao` com `esta_stale(..., agora)==False`; o ramo usa esse valor, e a existência de batida apenas distingue `efeito_sem_lampada` dentro do SIM. Sem lâmpada confiável, retorna desconhecimento. (`app/chamados/services/tarefa_do_dia.py:146`, `app/chamados/services/tarefa_do_dia.py:152`, `app/chamados/services/tarefa_do_dia.py:281`.)

5. **Esse caminho não alcança `date.today()` do juiz.** `_lavra` não chama `_pergunta_materializada_de_fato`; `esta_stale` recebe o `agora` obrigatório. O fallback `timezone.now()` existe, mas não é usado nessa chamada normal. (`app/chamados/services/tarefa_do_dia.py:281`, `app/chamados/services/tarefa_do_dia.py:284`, `app/chamados/services/tarefa_do_dia.py:322`, `app/chamados/veredito_lavrado.py:181`.) O juiz direto usa `date.today()` em `_dentro_da_janela_viva`. (`app/chamados/juizes.py:563`.)

6. **`esta_stale` mistura calendários e depende da representação de fuso do mesmo instante.** Usa `agora.date()` e `julgado_em.date()` sem conversão local. Com julgado em 06/11 12Z, expiração 07/11 e agora 07/11 00:30Z, o corpo executado deu `True`; com o mesmo agora convertido para −03, deu `False`. O projeto configura São Paulo e `USE_TZ=True`; a expiração é uma `DateField` calculada a partir de `date.today()` do processo, não de `timezone.localdate()`. Fuso efetivo do processo de produção: [não verificado]. (`app/chamados/veredito_lavrado.py:112`, `app/chamados/veredito_lavrado.py:182`, `app/chamados/veredito_lavrado.py:185`, `app/config/settings/base.py:93`, `app/config/settings/base.py:95`, `app/chamados/models.py:2399`.)

7. **A invalidação não cobre todos os insumos do juiz.** O marco agrega criação/retratação de batida, cinco timestamps da célula, materialização e resposta. Não agrega decisões, via, logs de retratação, estado/destino. O juiz lê via e materialização, log de retratação e estado do destino. Para destino, existe relavra por evento na porta; isso não transforma o predicado de staleness em cobertura completa de insumos. (`app/chamados/veredito_lavrado.py:137`, `app/chamados/veredito_lavrado.py:151`, `app/chamados/veredito_lavrado.py:162`, `app/chamados/juizes.py:283`, `app/chamados/juizes.py:648`, `app/chamados/juizes.py:744`, `app/chamados/services/fio.py:849`.) Uma decisão posterior a `julgado_em` não invalida diretamente a lavra; o juiz antigo tampouco lê `DecisaoPergunta`, e a tarefa não compara `ult.ato_em` à data da lavra. (`app/chamados/veredito_lavrado.py:179`, `app/chamados/services/tarefa_do_dia.py:137`, `app/chamados/services/tarefa_do_dia.py:146`, `app/chamados/juizes.py:633`.)

8. **A premissa de leitura de `materializada_em` somente pelo passivo não corresponde ao código.** A autoridade E1 `linhas_da_projecao_sem_linha` lê resposta e validação, sem consultar materialização; a carga da tarefa consulta indiretamente materialização por `marcos_do_lote`. (`app/chamados/models.py:3409`, `app/chamados/models.py:3416`, `app/chamados/models.py:3420`, `app/chamados/services/tarefa_do_dia.py:279`, `app/chamados/services/tarefa_do_dia.py:282`, `app/chamados/veredito_lavrado.py:162`.)

9. **Teste da validade da lâmpada não prova suas fronteiras temporais.** O helper escreve `julgado_em` dez anos no futuro e `janela_expira_em=None`. O caso de staleness usa retratação posterior à lavra; nenhum caso do arquivo exercita expiração. Portanto, o mutante que remove a verificação de `janela_expira_em` de `esta_stale` passaria esses casos E2. (`app/chamados/tests/test_e2_tarefa_do_dia.py:108`, `app/chamados/tests/test_e2_tarefa_do_dia.py:112`, `app/chamados/tests/test_e2_tarefa_do_dia.py:284`, `app/chamados/veredito_lavrado.py:182`.)

Censo localizado dos três campos da lavra:

- **Atribuição de valores:** `lavrar`, restauração em memória e backfill; persistência normal por `save`, e lote pela porta `gravar_lavras_em_lote`. (`app/chamados/veredito_lavrado.py:227`, `app/chamados/veredito_lavrado.py:243`, `app/chamados/management/commands/backfill_veredito_lavrado.py:138`, `app/chamados/veredito_lavrado.py:207`.)
- **Persistidores da lavra já calculada:** modelo no reapontamento e carimbo, fio no reapontamento/rechaveamento, e relavra de via. (`app/chamados/models.py:2571`, `app/chamados/models.py:2708`, `app/chamados/services/fio.py:850`, `app/chamados/services/fio.py:903`, `app/chamados/services/relavra_via.py:124`.)
- **Leitores dos três campos:** `esta_stale`, foto da lavra, carga nova, varredura do raio X e backfill. (`app/chamados/veredito_lavrado.py:179`, `app/chamados/veredito_lavrado.py:237`, `app/chamados/services/tarefa_do_dia.py:283`, `app/chamados/services/raiox.py:31`, `app/chamados/management/commands/backfill_veredito_lavrado.py:120`.)
- **Leitores adicionais do veredito:** crédito/ausência de ação do raio X, auditoria de invariantes e comparação/trilha da relavra de via. (`app/chamados/services/raiox.py:133`, `app/chamados/services/raiox.py:182`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1170`, `app/chamados/services/relavra_via.py:121`.)

## 07/11 04:08 · sessao c6c0a6f64a84 · - · - · origem=Ronald (tmux)

Achados para itens 4–6, somente por leitura, sem executar a bateria Django:

- **O selo de pureza não cobre ramo com pergunta.** O teste de zero escritas cria apenas célula e chama dois instantes; nenhuma pergunta/decisão existe nesse teste (`app/chamados/tests/test_e2_tarefa_do_dia.py:363–374`). Uma escrita condicionada à entrada em `_pela_decisao` não seria observada por esse selo (`app/chamados/services/tarefa_do_dia.py:121`). Mutante passar na bateria inteira: [não verificado].
- **A fila e suas portas ficam sem mordida na bateria.** `pergunta()` cria disputa com `contexto={}`, sem chamado (`app/chamados/tests/test_e2_tarefa_do_dia.py:95–99`); `acoes()` descarta `porta` antes das comparações (`:124–126`). Por inspeção, substituir `_fila` por `set()` deixaria as expectativas da bateria inalteradas; execução do mutante [não verificado]. O código cuja integração fica desprotegida é `app/chamados/services/tarefa_do_dia.py:290–293`.
- **“Concorrência” não prova corrida.** O caso 6 registra validações e silêncios sequencialmente e compara saídas; não abre transações concorrentes (`app/chamados/tests/test_e2_tarefa_do_dia.py:253–265`).
- **“Competência exportada” não monta uma competência exportada.** O caso apenas avança `agora` quarenta dias, verifica ação e ausência de SQL da folha (`app/chamados/tests/test_e2_tarefa_do_dia.py:349–357`). A independência diante de um registro real de exportação fica [não verificado].
- **A ordem das decisões é exercitada somente com relógio e PK concordantes.** O helper aumenta `ato_em` junto da quantidade de linhas existentes (`app/chamados/tests/test_e2_tarefa_do_dia.py:103–106`). Não há caso retrodatado nem empate; portanto, trocar a carga de `order_by('pergunta_id','ato_em','pk')` por PK provavelmente passaria, por inspeção; mutante executado [não verificado]. A ordem do modelo é `ato_em,pk` (`app/chamados/models.py:3374`) e a carga a respeita (`app/chamados/services/tarefa_do_dia.py:275–277`).
- **A lavra “fresca” dos testes é artificialmente futura.** O helper fixa `julgado_em` em D+3650 e apaga `janela_expira_em` (`app/chamados/tests/test_e2_tarefa_do_dia.py:108–113`), deixando sem teste a expiração civil e enfraquecendo a integração real de staleness.
- **O custo medido no teste é constância, não a meta ≤1.** A única asserção compara 2 colaboradores × 3 dias com 1 × 1; não impõe teto por colab-dia, nem mede frota (`app/chamados/tests/test_e2_tarefa_do_dia.py:397–415`). Resultado 6 do aval: [não verificado].
- **Falta caso de célula existente sem lâmpadas.** Nesse cenário `instancias_do_turno` devolve `[]`, e `_tarefa_pura` termina com `nao_sei=False` (`app/chamados/services/tarefa_do_dia.py:79–90`, `:173–177`, `:193–248`). O leitor antigo distingue ata sem lâmpadas como `ESTADO_SEM_CELULA` (`app/chamados/services/declaracao_texto.py:82–83`, `:110–116`), e o registro do novo juiz promete “Não sabe -> nao_sei” (`app/docs/JUIZES.md:77`).

A bateria tem mordida para soberania das lâmpadas por instância (caso 1: `app/chamados/tests/test_e2_tarefa_do_dia.py:129–147`), teto por instante/noturno (`:167–172`) e fronteira 14:00/14:01 (`:363–370`), cobertura não rejeitada (`:215–240`), devolução ao admin (`:198–206`) e passivo sem inventar decisão (`:325–333`). Não encontrei caso de ausência parcial, `trabalha=None`, ata de folga com `trabalha=True`, duas pausas, empate/retrodatação de decisões, nem status/silêncio reais de chamado no arquivo completo (`app/chamados/tests/test_e2_tarefa_do_dia.py:1–415`). Aprovação 10/10, RED e cronologia “teste antes do código”: [não verificado].

Contratos:

| Contrato | Alcança o arquivo novo? | O que efetivamente garante |
|---|---|---|
| leitores_vitalidade | Sim: varre os `.py` de produção de toda a árvore (`app/chamados/tests/test_contract_leitores_vitalidade.py:282–307`). | Só compara `materializada_em/validada_em` com None ou `__isnull` (`:14`, `:298–305`). O novo arquivo não faz isso; lê passivo pela E1 (`app/chamados/services/tarefa_do_dia.py:279`). Não prova pureza, relógio ou validade de `_lavra`. |
| censo_escritas | Sim (`app/core/censo_escritas.py:469–491`). | Rodei apenas o analisador estático com `python3 -B`: ele devolve `(176,None,'update','out.update()')`, falso positivo do dict de retorno (`app/chamados/services/tarefa_do_dia.py:173–176`). O varredor documenta que `update` só kwargs desconhecido vai ao balde (`app/core/censo_escritas.py:49–50`); entidade None é ignorada na agregação de famílias (`:505–509`). Nenhuma escrita de modelo detectada nesse arquivo por esse analisador. |
| escritores_estado | Sim: app `chamados`, descida por diretórios (`app/core/tests/test_contract_escritores_estado.py:12–13`, `:184–197`). | Regex de atribuição/`update` para campos enumerados (`:177–181`, `:201–208`); não há atribuição desses campos no novo módulo. Execução do contrato: [não verificado]. |
| B5j universo de apuração | Não seleciona esse módulo: `_producao` exige texto `parear_turnos` ou `_turnos_do_colab_puro` (`app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:63`, `:135–148`). | O novo módulo usa a porta correta `batidas_apuraveis` (`app/chamados/services/tarefa_do_dia.py:287–289`), mas esse tripwire não protege seu universo. |
| bocais_apuracao | Não: lista fechada de seis arquivos, sem tarefa (`app/ponto/tests/test_tripwire_bocais_apuracao.py:13–24`, `:27–33`). | Nenhuma garantia sobre o arquivo novo. |
| registro de juízes | Sim: `_C19` declarado (`app/core/juizes.py:150–155`, `:285`), lookup atualizado (`app/docs/JUIZES.md:73–78`), corte escrito (`app/docs/CORTES.md:424`). | O teste de existência percorre toda família (`app/chamados/tests/test_contract_juiz_chamado.py:24–27`), mas a procura de juiz duplicado só casa regex de SLA e data crua (`app/core/juizes.py:1397–1404`; algoritmo em `app/ponto/tests/test_contract_juiz_celula.py:48–60`). Não detectaria duplicação semântica de `_C19`. |

Não achei duplicação dos juízes registrados de dia sem trabalho, cobertura, teto ou fila no corpo principal: ele chama `dia_sem_trabalho`/`ausencia_parcial_cobra_o_resto` (`app/chamados/services/tarefa_do_dia.py:170–179`), usa carga de cobertura/teto (`:299`, `:307`) e fila (`:292`). `_posicoes` repete a regra de primeiro/último/meio de `_marcos_faltantes` (`app/chamados/services/tarefa_do_dia.py:49–62`; `app/chamados/services/declaracao_texto.py:118–128`), mas não encontrei essa classificação registrada como juiz próprio; violação de autoridade registrada por essa repetição: [não verificado].

## 07/11 04:07 · sessao 867869b107c9 · - · - · origem=Ronald (tmux)

Itens 2/3/custo, somente leitura; nenhum teste executado. [não verificado] [nao verificado]

**Pureza e relógio**

- Não encontrei escrita no banco nem leitura do relógio corrente no caminho executado por `tarefa_do_dia`: o wrapper exige `agora`, a carga o repassa a `qs_na_fila` e `esta_stale`, e o corpo recebe os fatos carregados. `app/chamados/services/tarefa_do_dia.py:259`, `app/chamados/services/tarefa_do_dia.py:281`, `app/chamados/services/tarefa_do_dia.py:292`, `app/chamados/services/tarefa_do_dia.py:318`.
- `qs_na_fila` só executa `timezone.now()` quando `agora is None`; a tarefa fornece valor obrigatório. `app/chamados/services/painel.py:188`, `app/chamados/services/painel.py:190`, `app/chamados/services/tarefa_do_dia.py:292`, `app/chamados/services/tarefa_do_dia.py:322`.
- `esta_stale` possui fallback `timezone.now()`, mas recebe o `agora` obrigatório da carga; não chama `lavrar`, `expira_em` nem `_pergunta_materializada_de_fato`. Portanto o `date.today()` de `expira_em` e do juiz não é alcançado por este caminho. `app/chamados/veredito_lavrado.py:112`, `app/chamados/veredito_lavrado.py:173`, `app/chamados/veredito_lavrado.py:181`, `app/chamados/services/tarefa_do_dia.py:281`.
- `vinculo_do_dia` escolhe entre fatos carregados; `marcos_turno` encadeia datas recebidas; `_pelo_efeito` e `batidas_apuraveis` constroem queryset; `chamado_que_lista`, `dia_sem_trabalho`, `ausencia_parcial_cobra_o_resto`, `texto_vazio` e `instancia_do_motivo` não leem relógio nesses corpos. `app/escala/alimentacao.py:269`, `app/escala/servico_jornada.py:68`, `app/ponto/turnos.py:2516`, `app/ponto/turnos.py:2875`, `app/chamados/services/fio.py:116`, `app/chamados/juizes.py:411`, `app/chamados/juizes.py:423`, `app/chamados/juizes.py:1253`, `app/chamados/services/declaracao_texto.py:57`.
- Há memorização **na instância da carga**, em `_inst` e `_cob`; não encontrei mutação global no caminho. A descrição literal “nenhum cache” precisa distinguir essas memórias por carga. `app/chamados/services/tarefa_do_dia.py:8`, `app/chamados/services/tarefa_do_dia.py:296`, `app/ponto/turnos.py:3068`.
- A função pública consulta banco; somente `_tarefa_pura` recebe todos os fatos. A pureza matemática de apenas `(colab,dia,agora)` não existe: há banco e fuso ativo implícitos. `localtime` e `make_aware` não recebem fuso explícito. `app/chamados/services/tarefa_do_dia.py:72`, `app/chamados/services/tarefa_do_dia.py:90`, `app/chamados/services/tarefa_do_dia.py:267`, `app/escala/servico_jornada.py:81`.

**Equivalência**

- Cobertura: dentro da janela declarada, ambas usam exatamente `_pelo_efeito`, `_q_cobertura` e `_cobertura_pura`. A ordem extra `colaborador_id` só agrupa; dentro de cada pessoa preserva `(data,pk)`, e o último sobrescreve. O intervalo expandido é inclusivo. `app/ponto/turnos.py:2753`, `app/ponto/turnos.py:2761`, `app/ponto/turnos.py:2771`, `app/ponto/turnos.py:2780`, `app/ponto/turnos.py:3072`.
- Teto: ambas usam `cartorio.marcos_com_teto`, cujo limite é inclusivo `marco + teto <= agora`, na ordem de `marcos_turno`. `app/ponto/services/cartorio.py:843`, `app/ponto/services/cartorio.py:858`, `app/ponto/turnos.py:3061`, `app/escala/servico_jornada.py:74`.
- **Não são exatamente a mesma alimentação:** `marcos_vencidos` usa a `cel` recebida para escolher vínculo, mas não a passa ao corpo; `marcos_do_dia` então busca DNA no banco. A carga passa suas células já carregadas. Contraexemplo: carga/célula em memória com DNA A, banco depois alterado para DNA B; a carga calcula A e a casca calcula B, mesmo colab-dia/agora. É um contraexemplo de código, não ocorrência de produção. `app/ponto/services/cartorio.py:841`, `app/ponto/services/cartorio.py:843`, `app/ponto/turnos.py:3062`, `app/escala/models.py:1119`, `app/escala/models.py:1122`.
- O pad é `[ini−2,fim+1]`, mas cobertura só cobre `[ini,fim]`; não existe guarda contra consulta fora da janela. Exemplo: carga `[21,21]`, consulta dia22 com célula no pad e ausência no dia22: a tarefa não vê cobertura, ao contrário da casca aberta para22. Isso é uso fora do contrato documentado, permitido silenciosamente pela API atual. `app/ponto/turnos.py:1847`, `app/ponto/turnos.py:2977`, `app/ponto/turnos.py:3073`, `app/chamados/services/tarefa_do_dia.py:302`, `app/chamados/services/tarefa_do_dia.py:318`.

**Custo e testes** [nao verificado]

- A carga faz um número de queries independente do número de colaboradores nos corpos examinados; sua construção inclui carga de vínculos/ata não utilizados diretamente pela tarefa. Não há prova fornecida de `<=1 query/colab-dia` na frota. O teste só exige igualdade entre dois colaboradores×três dias e um colaborador×um dia, sem limite absoluto. `app/ponto/turnos.py:2986`, `app/ponto/turnos.py:2989`, `app/ponto/turnos.py:2992`, `app/chamados/tests/test_e2_tarefa_do_dia.py:397`, `app/chamados/tests/test_e2_tarefa_do_dia.py:415`.
- A tarefa percorre todas as perguntas daquele colaborador em cada dia consultado, após filtrar em memória; queries constantes não demonstram complexidade linear no tamanho variável da janela. `app/chamados/services/tarefa_do_dia.py:306`.
- Mutante que remove o tratamento `dia_sem_trabalho` pode passar a bateria: os casos de folga têm lâmpadas vazias, logo nenhuma instância cuja cobrança precise ser suprimida. Falta `trabalha=False` com lâmpadas antigas apagadas, e também ausência parcial. `app/chamados/tests/test_e2_tarefa_do_dia.py:165`, `app/chamados/tests/test_e2_tarefa_do_dia.py:245`, `app/chamados/services/tarefa_do_dia.py:199`.
- Mutantes de cobertura por intervalo aberto/fechado e desempate `(data,pk)` também não são atingidos pela ausência do caso4, que cria apenas ausência de um dia. `app/chamados/tests/test_e2_tarefa_do_dia.py:210`, `app/chamados/tests/test_e2_tarefa_do_dia.py:233`.

_2667 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
