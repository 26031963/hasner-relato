# CODEX-ESTADO

> Ultima resposta processada: **31/10/2026 20:07** · publicado em **31/10/2026 20:09** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 31/10 20:07 · sessao 438619dd0837 · V3-P5-PRORROGACAO-POR-INTERVALO · verificador · origem=Code

**REPROVADO para a garantia geral.** O caso 22–07 com intervalo 05:30–06:30 foi corrigido, mas permanecem duas contraprovas: a HE parcial perde a janela do turno e plantões longos descontam minutos em dobro. A afirmação de monotonicidade também falha acima de 26h. (`app/ponto/motor_calculo_v2.py:94`, `app/ponto/motor_calculo_v2.py:110`, `app/ponto/motor_calculo_v2.py:156`, `app/ponto/motor_calculo_v2.py:402`)

**1. Chamadores, recortes e prorrogação**

| Leitor | Recorte e `prorrogacao` que chegam ao desconto |
|---|---|
| `he_noturna` | `[inicio_extra, saida]`; `inicio_extra` recua por minutos trabalhados, atravessando os intervalos. Total e desconto recebem esse recorte e o mesmo parâmetro `prorrogacao`. (`app/ponto/motor_calculo_v2.py:354`, `app/ponto/motor_calculo_v2.py:402`) |
| `_noturno_dos_intervalos` / `calcular_noturnas` | O wrapper encaminha entrada, saída, intervalos e `self.prorrogacao_noturna_pos5h`. Total e desconto usam o mesmo par; depois há piso zero. (`app/ponto/motor_calculo_v2.py:933`, `app/ponto/motor_calculo_v2.py:1877`) |
| `_ContextoNoturno` / `noturnas_do_dia` | Empresta o método do motor. Cada par válido é contado e descontado com `bool(prorrogacao_pos5h)`; pares nulos/invertidos são ignorados. (`app/ponto/calculador/regras.py:60`, `app/ponto/calculador/regras.py:82`) |
| HE por período no calculador | Distribui a HE do dia pelos períodos, do último para o primeiro, limitada ao trabalho de cada período; entrega suas pontas, seus intervalos e a prorrogação do dia a `he_noturna`. (`app/ponto/calculador/regras.py:875`, `app/ponto/calculador/regras.py:883`) |
| Releitura v3 do período | `noturno_no_par` entrega um único par, seus intervalos e o flag da CCT a `noturnas_do_dia`. (`app/ponto/calculador/periodo.py:508`) |

No motor, as noturnas recebem pontas **efetivas**, após a janela de HE e o corte dos intervalos. Já `_aplicar_he_noturna` entrega as pontas **cruas** do período, acompanhadas dos intervalos efetivos. No calculador, a alimentação usa as pontas efetivas carimbadas. Portanto os envelopes dos leitores podem divergir. (`app/ponto/motor_calculo_v2.py:2701`, `app/ponto/motor_calculo_v2.py:2709`, `app/ponto/motor_calculo_v2.py:2742`, `app/ponto/motor_calculo_v2.py:964`, `app/ponto/calculador/alimentacao.py:106`)

O flag vem do motor/CCT; a implementação do piso retorna `False` para `12x36`, enquanto a CCT pode fornecer outro valor. O cadastro efetivo de produção é **[nao verificado]**. (`app/core/regua_cct.py:182`, `app/core/regua_cct.py:466`, `app/ponto/calculador/alimentacao.py:572`)

Para recorte de até 26h e intervalos válidos e disjuntos, o desconto fica entre zero e o total contado pelo mesmo leitor, inclusive quando esse total contém duplicação. A função, porém, soma intervalos sem unir sobreposições nem rejeitar intervalo invertido; portanto **“sempre ≤ total” não é uma garantia irrestrita da API**. (`app/ponto/motor_calculo_v2.py:94`, `app/ponto/motor_calculo_v2.py:112`, `app/ponto/motor_calculo_v2.py:152`)

Resultados das funções reais, em minutos:

| Recorte; intervalo | Novo desconto | Avaliação |
|---|---:|---|
| 22–07; 05:30–06:30; `True` | 60 | Corrige P5. (`app/ponto/motor_calculo_v2.py:156`) |
| 19–07; 21:30–22:30; `True` | 30 | Cruza 22:00 corretamente. (`app/ponto/motor_calculo_v2.py:103`, `app/ponto/motor_calculo_v2.py:156`) |
| 22–07; 23–00; `True` | 60 | Meia-noite preservada. (`app/ponto/motor_calculo_v2.py:100`, `app/ponto/motor_calculo_v2.py:103`, `app/ponto/motor_calculo_v2.py:156`) |
| 22–07; 04:30–05:30; `True` / `False` | 60 / 30 | Respeita o flag na fronteira 05:00. (`app/ponto/motor_calculo_v2.py:110`, `app/ponto/motor_calculo_v2.py:156`) |
| 22–07; intervalo inteiramente fora | 0 | Fora é ignorado; interseção parcial é cortada. (`app/ponto/motor_calculo_v2.py:154`, `app/ponto/motor_calculo_v2.py:156`) |
| Recorte invertido | 0 | Os prefixos utilizados retornam zero; não apareceu desconto indevido nesse caso. (`app/ponto/motor_calculo_v2.py:91`, `app/ponto/motor_calculo_v2.py:156`) |

**Contraprova da janela do turno:** turno 22–07, intervalo 05:30–06:30, prorrogação ligada e **60 minutos de HE**. O recuo encontra 05:00; `N(05:00,07:00)=0`, porque a ativação exige início estritamente anterior às 05:00. O split devolve **zero**, embora os 60 minutos trabalhados da ponta pertençam à prorrogação do turno original. Com HE de 120 minutos, o início recua a 04:00 e o split devolve 120. É defeito preexistente do recorte do leitor; o diff não transporta a janela original. (`app/ponto/motor_calculo_v2.py:109`, `app/ponto/motor_calculo_v2.py:354`, `app/ponto/motor_calculo_v2.py:402`)

**2. Validade da diferença de prefixos**

A diferença preserva a medida acumulada de `N`; ela **não transforma uma medida duplicada em minutos físicos corretos**. O segmento 00–05 é estendido até `fim`, mas os segmentos seguintes continuam sendo somados. (`app/ponto/motor_calculo_v2.py:110`, `app/ponto/motor_calculo_v2.py:112`, `app/ponto/motor_calculo_v2.py:156`)

| Entrada → saída; intervalo; `True` | `N(total)` | Desconto novo | Quebra |
|---|---:|---:|---|
| 00:00 → 00:00 seguinte; 22:30–23:30 | 1.560 | 120 | Um intervalo físico de 60 minutos desconta 120. |
| 04:00 → 04:00 seguinte; 01–02 do dia seguinte | 1.800 | 120 | A extensão anterior sobrepõe a janela seguinte. |
| 22:00 → 00:01 dois dias depois, 26h01; intervalo 01–02 intermediário | 0 | 60 | Desconto maior que o total. |
| Mesmo recorte de 26h01; último intervalo 23:30–00:01 | 0 | −1.620 | Desconto negativo acrescenta noturnas. |

Esses resultados decorrem da sobreposição das janelas, do retorno abrupto a zero acima de 26h e da subtração dos prefixos. (`app/ponto/motor_calculo_v2.py:94`, `app/ponto/motor_calculo_v2.py:110`, `app/ponto/motor_calculo_v2.py:112`, `app/ponto/motor_calculo_v2.py:156`)

Logo, a justificativa “`N` não decresce” é falsa globalmente. A duplicação já ocorre **dentro de 24h**, sem atingir o teto; turnos regulares de 12h não alcançam essas sobreposições. (`app/ponto/motor_calculo_v2.py:94`, `app/ponto/motor_calculo_v2.py:100`, `app/ponto/motor_calculo_v2.py:103`, `app/ponto/motor_calculo_v2.py:149`)

O plantão 24h tem caminho concreto: `MotorPlantao` admite continuidade de 26h e reutiliza `Motor12x36.calcular_periodo`. O pareador também possui envelope de 26h. Existência desses exemplos em produção: **[nao verificado]**. (`app/ponto/motor_calculo_v2.py:3437`, `app/ponto/motor_calculo_v2.py:3460`, `app/ponto/turnos.py:1431`)

**3. Quem escreve e quem lê as três rubricas**

| Etapa | Escritor → leitor |
|---|---|
| Período do motor | `_escrever_noturnas` grava minutos noturnos; `_aplicar_he_noturna` grava os dois subsets de HE. (`app/ponto/motor_calculo_v2.py:953`, `app/ponto/motor_calculo_v2.py:964`) |
| Consolidação mensal | `ResultadoMes.horas_noturnas` soma períodos comuns e FT; os subsets de HE somam somente períodos comuns. (`app/ponto/motor_calculo_v2.py:283`, `app/ponto/motor_calculo_v2.py:300`) |
| `FechamentoMensal` | O serviço atribui `horas_noturnas`, `horas_extras_50_noturna` e `horas_extras_100_noturna`, arredondando a duas casas. Persiste por `save` ou `update`. (`app/ponto/services/fechamento.py:893`, `app/ponto/services/fechamento.py:896`, `app/ponto/services/fechamento.py:899`, `app/ponto/services/fechamento.py:1112`, `app/ponto/services/fechamento.py:1128`) |
| `DiaPago`, versão motor | `lavrar` lê os minutos do período e monta as três rubricas; persiste por `bulk_create`. (`app/ponto/services/dia_pago.py:179`, `app/ponto/services/dia_pago.py:181`, `app/ponto/services/dia_pago.py:184`, `app/ponto/services/dia_pago.py:358`) |
| Calculador v3 | `regras.do_dia` escreve os subsets e as noturnas; a mesma `lavrar` substitui os campos calculados. O fechamento grava essa versão como `oraculo`, sem substituir por ela os campos de `FechamentoMensal`. (`app/ponto/calculador/regras.py:896`, `app/ponto/calculador/regras.py:898`, `app/ponto/calculador/regras.py:912`, `app/ponto/services/dia_pago.py:320`, `app/ponto/services/fechamento.py:1245`) |
| Folha/TXT | `eventos_do_fechamento` lê `horas_noturnas` para `adicional_noturno`. Os dois subsets **não geram linhas próprias**: HE50/100 saem integrais. (`app/folha/export.py:413`, `app/folha/export.py:430`, `app/folha/export.py:436`) |
| Conferência, tela e relatórios | A soma de `DiaPago` agrega os campos; a porta do export consulta essa soma. `rubricas_do_dia` apresenta noturnas; ranking e simulação também leem os valores do fechamento. (`app/ponto/services/dia_pago.py:548`, `app/folha/porta_export.py:267`, `app/ponto/services/dia_pago.py:922`, `app/folha/services/ranking_he.py:225`, `app/folha/management/commands/simular_folha.py:84`) |

Há escritas excepcionais existentes: `restaurar_fechamento` aceita valores conhecidos e faz `update`; os comandos antigos de apply passam por essa porta. `retratar_exportada` preserva dinheiro existente, mas inicializa rubricas em zero ao criar dia ausente. Nenhuma dessas portas nasce neste diff. (`app/ponto/services/fechamento.py:1326`, `app/ponto/services/fechamento.py:1360`, `app/ponto/management/commands/aplicar_09_he_intra.py:149`, `app/ponto/management/commands/aplicar_09_corte_b.py:266`, `app/ponto/services/dia_pago.py:469`)

**4. Testes e mutante** [nao verificado]

Os testes exatos **mordem a regra do caso P5**: exigem 8h de HE100, 8h de HE100 noturna e 8h de noturnas. Os quatro mutantes restauram `_por_peca` e exigem falha de asserção; não inspecionam a forma da implementação. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:51`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:138`)

A grade **passa com o código antigo**: `desc >= _por_peca` admite igualdade. Portanto não detecta essa regressão, embora não seja genericamente vazia — descontos noturnos positivos impedem um mutante constante zero. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:128`)

O golden usa um período inteiro em HE100. Faltam HE parcial após 05:00, HE50, múltiplos intervalos, sobreposição, recortes externos/invertidos e limite de 26h; a grade exclui expressamente os turnos longos. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:43`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:115`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:120`)

Execução isolada em memória: 13 métodos verdes, incluindo os quatro mutantes, com substituições das dependências Django. Suite Django canônica: **[nao verificado]**. [nao verificado]

**5. Classes já reprovadas**

- **Segundo juiz, segundo escritor, derivação paralela:** o desconto continua na mesma função e usa `minutos_noturnos`; os wrappers permanecem delegadores. Não aparece nova autoridade nesses caminhos. (`app/ponto/motor_calculo_v2.py:156`, `app/ponto/motor_calculo_v2.py:1877`, `app/ponto/calculador/regras.py:63`)
- **Relógio fora da borda:** o cálculo usa os instantes recebidos e os localiza no fuso; a falha demonstrada é a troca da âncora para a ponta extra. (`app/ponto/motor_calculo_v2.py:151`, `app/ponto/motor_calculo_v2.py:402`)
- **Selo sem mordida:** há mordida focal nos quatro mutantes, mas a grade não distingue a regressão antiga e exclui a contraprova de 24h. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:115`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:132`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:138`)
- **Ordem de trava e pouso antes do pré-requisito:** execução de esteira, deploy, impacto financeiro e estado produtivo não constam dos anexos: **[nao verificado]**. [nao verificado]

## 31/10 20:05 · sessao 9e38a74b7ef3 · - · - · origem=Ronald (tmux)

Os testes mordem o caso P5, mas não provam a propriedade universal alegada: o golden fixa 22→07, intervalo05:30→06:30, prorrogação ligada e toda a jornada em HE100; cobra 8h no total, no split e nas noturnas. `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:39-65`.

Os quatro mutantes verificam falha de asserção ao restaurar `_por_peca`; usam resultados numéricos, não inspeção da forma do código. `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:138-156`.

A grade **passa também sob `_por_peca`**: suas condições admitem exatamente o desconto antigo; consequentemente não detecta a regressão P5. Ela não é genericamente vazia: o mutante constante zero falha em intervalos noturnos. `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:128-132`.

A grade usa somente entradas18/20/22/23, durações6/8/9/12h e um intervalo contido de1h; exclui explicitamente os turnos longos que contam em dobro. O golden não exercita HE50 nem HE parcial cujo início efetivo ocorre depois05. `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:115-126`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:43-49`, `app/ponto/motor_calculo_v2.py:402-405`.

`assertRaises(AssertionError)` aceita qualquer asserção interna quebrada, mas os testes numéricos normais coexistem; portanto não há fundamento para chamar os quatro mutantes atuais de vazios. `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:140-142`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:51-65`.

Execução isolada:13 métodos verdes, incluindo os4 mutantes, com motor/regras originais importados e AST original dos testes, `_aw` e `piso_intervalo_art71`; substituídas somente dependências Django por `unittest.TestCase`, settings.TIME_ZONE e `timezone.make_aware=TZ_LOCAL.localize`. Suite Django canônica: [nao verificado]. O helper original está em `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:26-27`; o piso original em `app/escala/models.py:6-21`.

## 31/10 20:05 · sessao b26384d06b53 · - · - · origem=Ronald (tmux)

**Censo dos chamadores e recortes (somente leitura).**

| Caminho | Recorte e `prorrogacao` recebidos |
|---|---|
| `he_noturna` | Calcula `inicio_extra` recuando minutos trabalhados desde `saida`; chama tanto `minutos_noturnos` quanto o desconto em `[inicio_extra, saida]`, com o mesmo parâmetro `prorrogacao`. `app/ponto/motor_calculo_v2.py:354`, `app/ponto/motor_calculo_v2.py:402` |
| `MotorBase._noturno_dos_intervalos` | Encaminha os argumentos recebidos à função e usa `self.prorrogacao_noturna_pos5h`. `app/ponto/motor_calculo_v2.py:1877` |
| `MotorBase.calcular_noturnas` | Total e desconto usam exatamente `[entrada, saida]` e o mesmo atributo de prorrogação; aplica piso zero após descontar. `app/ponto/motor_calculo_v2.py:933` |
| `_ContextoNoturno` / `noturnas_do_dia` | Empresta o método de `MotorBase`; para cada par válido usa o mesmo par e `bool(prorrogacao_pos5h)` no total e desconto. Ignora par nulo/invertido e aplica piso zero depois da soma. `app/ponto/calculador/regras.py:60`, `app/ponto/calculador/regras.py:82` |
| Calculador, HE por período | Ordena períodos por saída, distribui HE do dia em ordem reversa, limita cada parcela ao trabalho do período e chama `he_noturna` com suas pontas, seus intervalos e a prorrogação do dia. `app/ponto/calculador/regras.py:875`, `app/ponto/calculador/regras.py:883` |

Os únicos chamadores diretos produtivos de `minutos_noturnos_dos_intervalos` encontrados são `he_noturna` e o wrapper de `MotorBase`. `app/ponto/motor_calculo_v2.py:404`, `app/ponto/motor_calculo_v2.py:1877`

**Diferença importante entre janela do leitor e janela do turno.**

No motor, adicional noturno usa pontas **efetivas**, depois de `_janela_do_par` e do corte dos intervalos: 12x36 em `app/ponto/motor_calculo_v2.py:2709` e `app/ponto/motor_calculo_v2.py:2742`; partido em `app/ponto/motor_calculo_v2.py:2836` e `app/ponto/motor_calculo_v2.py:2846`; comercial em `app/ponto/motor_calculo_v2.py:3161` e `app/ponto/motor_calculo_v2.py:3197`.

Já `_aplicar_he_noturna` entrega `periodo.entrada/saida` **crus**, acompanhados dos intervalos efetivos carimbados. Assim, adicional e HE podem ter envelopes diferentes, embora usem o mesmo flag. `app/ponto/motor_calculo_v2.py:2701`, `app/ponto/motor_calculo_v2.py:1790`, `app/ponto/motor_calculo_v2.py:964`

Há contraprova simples da regra “janela do turno”: turno **22–07**, intervalo **05:30–06:30**, `prorrogacao=True`, **60 minutos de HE**. O recuo encontra **05:00**; a chamada `N(05:00,07:00)` não ativa prorrogação e `he_noturna` devolve **0**, embora os 60 minutos trabalhados da ponta estejam dentro da prorrogação do turno original. Com **120 minutos de HE**, o início fica **04:00** e a função devolve **120**. Verifiquei executando em memória as quatro funções extraídas por AST; a causa está no teste estrito `inter_s1_ini < s1_fim` e na troca da âncora para `inicio_extra`. `app/ponto/motor_calculo_v2.py:109`, `app/ponto/motor_calculo_v2.py:354`, `app/ponto/motor_calculo_v2.py:402`

Isso é limitação preexistente do chamador/minuendo; o diff faz desconto e total concordarem **no recorte da HE**, mas não transporta a janela do turno original. `app/ponto/motor_calculo_v2.py:402`, `app/ponto/motor_calculo_v2.py:156`

**Alimentação do calculador.**

`segmentos_do_motor` lê pontas efetivas e intervalos carimbados, recusa `s <= e`, entrega envelopes em `pares_por_dia`, intervalos em `intervalos_por_dia`, e uma ponta HE por período com teto calculado dos segmentos. `app/ponto/calculador/alimentacao.py:106`, `app/ponto/calculador/alimentacao.py:127`, `app/ponto/calculador/alimentacao.py:141`, `app/ponto/calculador/alimentacao.py:185`

A prorrogação da regra diária vem do motor CCT consultado na data `k`, e segue para `regras.do_dia`. `app/ponto/calculador/alimentacao.py:445`, `app/ponto/calculador/alimentacao.py:572`, `app/ponto/calculador/alimentacao.py:596`

A releitura v3 do período chama `noturno_no_par` com **um par e seus intervalos**; este chama `noturnas_do_dia` com o flag da CCT. Pares efetivos invertidos são contados e ignorados anteriormente. `app/ponto/calculador/periodo.py:508`, `app/ponto/calculador/periodo.py:697`, `app/ponto/calculador/periodo.py:752`

**Garantias e limites upstream.**

O motor corta intervalos à janela efetiva e mantém somente interseções positivas; a função de desconto alterada não faz essa validação internamente. `app/ponto/motor_calculo_v2.py:1833`, `app/ponto/motor_calculo_v2.py:152`

A autoridade filtra intervalos com duração mínima de 15 minutos; o adaptador entrega sua resposta ao motor. A passagem pela ata pode fornecer uma lista de pausas, posteriormente cortada à janela. `app/ponto/turnos.py:497`, `app/ponto/turnos.py:543`, `app/ponto/adaptador_motor.py:44`, `app/ponto/motor_calculo_v2.py:675`

O fechamento geométrico permite envelope de **26h**, sujeito ao teto do dia e sua exceção declarada; portanto 24h não é descartado universalmente antes do cálculo. `app/ponto/turnos.py:1431`, `app/ponto/turnos.py:1442`, `app/ponto/turnos.py:1445`

O default de prorrogação do motor consulta o cadastro do ciclo; o piso implementado devolve `False` para `12x36`, enquanto CCT pode fornecer outra resposta. Portanto “12x36 sofre o defeito com prorrogação ligada” depende do cadastro; ocorrência produtiva no material dado: **[nao verificado]**. `app/ponto/motor_calculo_v2.py:720`, `app/core/regua_cct.py:166`, `app/core/regua_cct.py:182`

## 31/10 20:04 · sessao d20f92fb7cbe · - · - · origem=Ronald (tmux)

Censo dos três campos, somente pelo código:

| Etapa | Escrita/leitura |
|---|---|
| Minutos noturnos do período | `MotorBase._escrever_noturnas` recebe `calcular_noturnas` e escreve `minutos_noturnos_relogio/legais`: `app/ponto/motor_calculo_v2.py:953-955`. Chamadores: 12x36 `:2742`, partido `:2846`, comercial `:3197`. |
| Minutos de HE noturna | `_aplicar_he_noturna` lê período, extras, flag e intervalos; chama `he_noturna` e escreve os subsets: `app/ponto/motor_calculo_v2.py:964-972`. Chamadores: `:2805`, `:3097`, `:3128`, `:3231`, `:3349`. |
| Zeros/reinicializações | Bloqueio de período zera os três grupos por `setattr`: `app/ponto/motor_calculo_v2.py:1423-1426`. Partido reinicializa HE noturna `:2887-2888`; comercial `:3273-3274`. |
| Total mensal | `ResultadoMes.horas_noturnas` soma períodos comuns **e FT**: `app/ponto/motor_calculo_v2.py:283`. `horas_extra_50_noturna/100_noturna` somam só comuns: `:300-306`. Noturna legal também entra na base de HE do partido quando `regua_excedente == 'legais'`: `:3069-3081`. |
| FechamentoMensal | Campos persistentes: `app/ponto/models.py:413`, `:416`, `:419`. `recalcular_fechamento_mes` atribui, arredondando total em duas casas: `app/ponto/services/fechamento.py:893`, `:896`, `:899`. Persiste aberto com `save` `:1112`; demais status pelo **mesmo módulo e mesmos valores**, `update` `:1128-1137`. Caminho somente leitura entrega objeto/dict sem persistir fechamento: `:1096-1104`. |
| DiaPago, versão motor | `lavrar` soma noturnas e subsets de cada período: `app/ponto/services/dia_pago.py:179`, `:181`, `:184`; FT leva noturnas `:224`. Campos `CAMPOS_DIA`: `:34-42`; modelos: `app/ponto/models.py:327-332`. Escrita efetiva: `delete`/`bulk_create`, `app/ponto/services/dia_pago.py:357-358`; chamada pelo fechamento com `versao='motor'`: `app/ponto/services/fechamento.py:1169-1195`. |
| Calculador v3 | `_ContextoNoturno` empresta método do motor `app/ponto/calculador/regras.py:63`; `noturnas_do_dia` soma total e subtrai desconto do motor `:85-89`. `do_dia` escreve `horas_noturnas` no ramo FT `:735-740` e comum `:907-912`. HE por período chama `he_noturna` `:880-888` e escreve os dois subsets `:895-898`. |
| Alimentação v3 | Fábrica do motor recebe **dia k** e template: `app/ponto/calculador/alimentacao.py:445-460`; flags vêm da CCT `:572-575`; pares, pontas, intervalos e flags chegam a `do_dia` `:582-600`. |
| Persistência v3 | Fechamento chama calculador e a **mesma** `lavrar`, com `rubricas=` e `versao='oraculo'`: `app/ponto/services/fechamento.py:1233-1264`. `lavrar` substitui campos declarados pelos valores calculados `app/ponto/services/dia_pago.py:320-328`; usa o mesmo `bulk_create` `:358`. Esse caminho não atribui esses valores v3 ao FechamentoMensal (`app/ponto/services/fechamento.py:1245-1264`). |
| Export TXT | `eventos_do_fechamento` lê `fech.horas_noturnas` em `('adicional_noturno', ...)`: `app/folha/export.py:436`. **Não lê os dois subsets de HE noturna no corpo executável**; emite HE50/100 integrais `:430-432`, explicação da não duplicação `:413-427`. TXT passa cada evento pelo cadastro de rubrica e monta linha: `:711-720`; logo código de rubrica efetivamente cadastrado em produção é [nao verificado]. |
| Conferência do export | Porta lê soma do DiaPago: `app/folha/porta_export.py:267`, `:332`. Soma agrega todos `CAMPOS`, inclusive três campos auditados: `app/ponto/services/dia_pago.py:548-561`. Consulta de dias da porta fixa `versao='motor'`: `app/folha/porta_export.py:284`, `:589`. |
| Tela/papel | `rubricas_do_dia` lê noturnas, sem adicionar subsets à HE: `app/ponto/services/dia_pago.py:912-926`. Calendário a chama `app/colaboradores/services/calendario.py:785`; espelho importa `app/ponto/services/espelho.py:419`. Totais de espelho também somam `PeriodoCalculo.horas_noturnas`: `:99`. Cartão lê mesma função de eventos da folha `app/relatorios/cartao_pela_celula.py:90` e soma DiaPago `:142-143`. Relatório por período agrega `CAMPOS_DIA` `app/ponto/services/dia_pago.py:782-787` e apresenta noturnas `app/relatorios/views.py:524`. |

Há porta adicional **existente**, `retratar_exportada`, explicitamente documentada como segunda porta (`app/ponto/services/dia_pago.py:403-425`). Ela mantém rubricas dos dias existentes, mas cria dias ausentes com dinheiro zero (`:469-477`) e ajuste ausente com todos `CAMPOS` zero antes de copiar DSR/banco (`:507-514`). Não recalcula as três rubricas noturnas nessa porta (`:459-481`, `:502-518`).

A ocorrência desses caminhos e os valores gravados na produção não constam no dado raspado fornecido: [nao verificado].

## 31/10 19:56 · sessao 87c7f078bee3 · TRAVA-COBERTURA · censo · origem=Code

A cobertura é parcial: os runners de suíte em `bin/` passam pela trava, mas há um hook com testes crus em `app/core/hooks/`; o scanner não examina arquivos shell de `app/` (`bin/pre-push.sh:142`, `app/core/hooks/pre-push.sh:54`, `bin/tests/test_trava_teste.sh:130`). A trava identifica o processo dono e rotula seu container, mas não registra a obra (`bin/trava_teste.sh:36`, `bin/trava_teste.sh:101`, `bin/trava_teste.sh:138`).

Censo estático. Execução dos selos, processos vivos, instalação efetiva dos hooks e correspondência Git com o HASH informado: **[não verificado]**.

**1. Onde nasce**

Nas tabelas, “trava direta” significa `bash bin/trava_teste.sh … -- docker run …`. Quando não há `--espera` explícito, vale **3600 segundos** (`bin/trava_teste.sh:35`, `bin/trava_teste.sh:87`). `$TESTE_DOCKER` fornece cpuset, CPUs e memória; os padrões são `4-7`, `4` e `2g` (`bin/recursos.sh:41`, `bin/recursos.sh:61`, `bin/recursos.sh:69`, `bin/recursos.sh:81`).

| Nascimento | Passa por qual porta / chamador | Argumentos relevantes |
|---|---|---|
| `bin/suite.sh:97` | Trava direta; `ESTEIRA_QUEM` recebido ou `suite`; repassa `--espera` (`bin/suite.sh:49`, `bin/suite.sh:96`). | `test ${ONLY:-$LABELS}`, CI, `--noinput`, paralelo padrão 4; extras após `--`; árvore viva ou `--dir`; repassa `REGUA_DB` (`bin/suite.sh:43`, `bin/suite.sh:74`, `bin/suite.sh:89`, `bin/suite.sh:102`). |
| `bin/regua.sh:166` | Trava direta; `regua` (`bin/regua.sh:165`). | `${ONLY:-$LABELS} --settings=config.settings.ci --noinput --parallel 4 $_ORDEM`; monta `$APP` (`bin/regua.sh:167`, `bin/regua.sh:168`). |
| `bin/pre-push.sh:143` | `_teste_irmao()` → trava direta; `pre-push` (`bin/pre-push.sh:122`, `bin/pre-push.sh:142`). | Duas chamadas: arsenal `$LABELS`, CI, paralelo 4; e `tenants`, `teste_motor`, sem paralelo. Monta `$ARVORE_PUSH/app` (`bin/pre-push.sh:144`, `bin/pre-push.sh:186`, `bin/pre-push.sh:198`). |
| `bin/placar_code.sh:127` | Trava direta; `placar_code`; `--espera 120` (`bin/placar_code.sh:126`). | DB `test_juliani_placar`; `core.tests.test_bug116_credencial_deixa_trilha.CContratoCredencialSemTrilhaTest`; CI, `--noinput` (`bin/placar_code.sh:128`, `bin/placar_code.sh:130`). |
| `bin/regua_relogio.sh:52` | Trava direta; `regua_relogio` (`bin/regua_relogio.sh:51`). | `$APPS --settings=cfg_tz --noinput`; `PROBE_TZ`, `PYTHONPATH` e shim montado (`bin/regua_relogio.sh:53`, `bin/regua_relogio.sh:55`). |
| `bin/regua_calendario.sh:47` | Trava direta; `regua_calendario` (`bin/regua_calendario.sh:46`). | `$LABELS`, `ci_calendario`, paralelo 4; três datas, `RELOGIO_FAKE` e DB `test_juliani_cal$nome` (`bin/regua_calendario.sh:44`, `bin/regua_calendario.sh:48`, `bin/regua_calendario.sh:49`). |
| `bin/vigia_arvore.sh:84` | Função local `suite()` → trava direta; `vigia_arvore` (`bin/vigia_arvore.sh:77`, `bin/vigia_arvore.sh:83`). | Snapshot, CI; primeiro `$LABELS`, DB `test_juliani_vigia`, paralelo 2; depois `$IDS`, DB `test_juliani_vigia_b`, sem paralelo (`bin/vigia_arvore.sh:85`, `bin/vigia_arvore.sh:88`, `bin/vigia_arvore.sh:106`). |
| `bin/vigia_arvore.sh:132` | Trava direta; `vigia_arvore` (`bin/vigia_arvore.sh:131`). | `$IDS2` na árvore viva; DB `test_juliani_vigia_viva`; CI, sem paralelo (`bin/vigia_arvore.sh:132`, `bin/vigia_arvore.sh:134`). |
| `bin/isolamento.sh:34` | `_iso_roda()` → trava direta; `isolamento` (`bin/isolamento.sh:33`). | `$ids`, CI, `--noinput`; snapshot e DB `$db`, chamado com `test_juliani_iso_$fatia` (`bin/isolamento.sh:34`, `bin/isolamento.sh:36`, `bin/isolamento.sh:49`). |
| `bin/molde_fatia/rodar.sh:31` | Trava direta; `fatia`; `--espera 5400` (`bin/molde_fatia/rodar.sh:30`). | `core.tests.test_selo_mapa_contrato`, CI; DB `test_juliani___FATIA__m`; app da cura (`bin/molde_fatia/rodar.sh:32`, `bin/molde_fatia/rodar.sh:33`). |
| `bin/molde_fatia/rodar.sh:45` | `teste()` → trava direta; `fatia`; `--espera 5400` (`bin/molde_fatia/rodar.sh:37`, `bin/molde_fatia/rodar.sh:44`). | RED: `__RED__`, DB terminado em `r`; GREEN: `__RED__ __GREEN__ __PACOTE__ --parallel 4`, DB terminado em `g`; CI (`bin/molde_fatia/rodar.sh:52`, `bin/molde_fatia/rodar.sh:54`). |
| `bin/molde_autocura/rodar.sh:23` | `teste()` → trava direta; `autocura`; `--espera 5400` (`bin/molde_autocura/rodar.sh:18`, `bin/molde_autocura/rodar.sh:22`). | RED `$IDS`, DB `test_juliani_esacred`; GREEN `$LABELS --parallel 4`, DB `test_juliani_esacfull`; CI (`bin/molde_autocura/rodar.sh:27`, `bin/molde_autocura/rodar.sh:30`). |
| **`app/core/hooks/pre-push.sh:54`** | **Não passa por trava de teste nem por `suite.sh`.** Usa outro flock, `/tmp/regua.lock` (`app/core/hooks/pre-push.sh:32`, `app/core/hooks/pre-push.sh:39`, `app/core/hooks/pre-push.sh:54`). | Arsenal `$LABELS`, CI, paralelo 2, `-e DB_PASSWORD`; depois `tenants`, `teste_motor`; monta árvore viva (`app/core/hooks/pre-push.sh:55`, `app/core/hooks/pre-push.sh:82`, `app/core/hooks/pre-push.sh:87`). |

A função `suite()` do vigia é local àquele arquivo; ela não chama `bin/suite.sh` (`bin/vigia_arvore.sh:77`, `bin/vigia_arvore.sh:83`).

Há ainda estes nascimentos para sonda, ensaio, preparação ou verificação, **fora das duas portas de suíte**: [nao verificado]

| Nascimento | Trabalho / controle |
|---|---|
| `bin/sonda_frota.sh:41` | Shell tenant `juliani`, executando `/sonda/sonda.py`; app somente leitura; sem trava de teste no lançamento (`bin/sonda_frota.sh:43`, `bin/sonda_frota.sh:45`). |
| `bin/sonda_leitura.sh:235` | Shell tenant, `$SETTINGS`, `$REDE`, `$ENVFILE`, `$BANCO`; app somente leitura; sem trava de teste no lançamento (`bin/sonda_leitura.sh:236`, `bin/sonda_leitura.sh:239`). |
| `bin/sombra.sh:183` | Função `container()`: settings sombra, DB `sombra`, cpuset de teste, `--cpus 1`, app somente leitura e proxies mortos. Os ramos `--refazer`, `--bloco` e `--rodar` tomam **a trava própria da sombra** (`bin/sombra.sh:184`, `bin/sombra.sh:189`, `bin/sombra.sh:438`, `bin/sombra.sh:451`, `bin/sombra.sh:487`). |
| `bin/simular_folha.sh:48` | Função `container()`, settings sombra e `--cpus 1`; foto/par tomam trava da sombra; comparar também usa essa função (`bin/simular_folha.sh:49`, `bin/simular_folha.sh:67`, `bin/simular_folha.sh:87`, `bin/simular_folha.sh:96`). |
| `bin/diff_janela_he_total.sh:30` | Toma trava da sombra; executa `diff_janela_he_total --schema=juliani --mes … --ano … --empresas … --piso … --json …` (`bin/diff_janela_he_total.sh:18`, `bin/diff_janela_he_total.sh:38`). |
| `bin/r5_idempotencia_frota.sh:16` | Executa `/r5/r5_idempotencia_frota.py`, settings sombra; lançamento cru, sem aquisição de trava no preâmbulo (`bin/r5_idempotencia_frota.sh:7`, `bin/r5_idempotencia_frota.sh:24`). |
| `bin/prova_de_casca.sh:30` | `--network none`, settings saas, imagem obtida de `saas_ui`, `python /prova.py`; lançamento cru (`bin/prova_de_casca.sh:27`, `bin/prova_de_casca.sh:31`, `bin/prova_de_casca.sh:34`). |
| `bin/esmeril_da_copia.sh:23` | Ruff/Vulture na cópia; lançamento cru (`bin/esmeril_da_copia.sh:24`). |
| `bin/molde_fatia/rodar.sh:19` | `gerar_diagrama --settings=config.settings.ci`; lançamento cru (`bin/molde_fatia/rodar.sh:19`). |
| `bin/molde_fatia/rodar.sh:21` | Ruff/Vulture; lançamento cru (`bin/molde_fatia/rodar.sh:21`). |
| `bin/molde_autocura/rodar.sh:13` | Ruff; lançamento cru (`bin/molde_autocura/rodar.sh:13`). |
| `bin/db_teste.sh:53` | Banco persistente `juliani_db_test`, `postgres:17-alpine`, `-d`, volume próprio, cpuset e restart `unless-stopped`; sem trava de suíte (`bin/db_teste.sh:21`, `bin/db_teste.sh:23`, `bin/db_teste.sh:53`, `bin/db_teste.sh:56`). |
| `bin/sombra_db.sh:91` | Banco persistente `sombra_db`, `postgres:17-alpine`, `-d`, volume próprio, cpuset, memória e restart `unless-stopped`; sem trava de suíte (`bin/sombra_db.sh:40`, `bin/sombra_db.sh:45`, `bin/sombra_db.sh:91`, `bin/sombra_db.sh:94`). |
| `bin/tests/test_inventario_pessoal_no_commit.sh:24` | `_rodar()` executa `inventario_pessoal_dos_modelos.py`, com `$TESTE_DOCKER`; lançamento cru (`bin/tests/test_inventario_pessoal_no_commit.sh:23`, `bin/tests/test_inventario_pessoal_no_commit.sh:28`). |
| `bin/tests/test_prepush_testa_o_commit.sh:97` e `bin/tests/test_prepush_testa_o_commit.sh:102` | Dois containers verificam staticfiles com/sem montagem auxiliar; crus, sem `$TESTE_DOCKER` nesses comandos (`bin/tests/test_prepush_testa_o_commit.sh:98`, `bin/tests/test_prepush_testa_o_commit.sh:103`). |
| `bin/tests/test_trava_dono_para_filho.sh:46`, `bin/tests/test_trava_dono_para_filho.sh:60`, `bin/tests/test_trava_dono_para_filho.sh:96` | Alpine pela **cópia isolada** da trava; esperas 5/5/2 segundos; `--init`, cpuset `4-7`, memória `64m`, nomes próprios (`bin/tests/test_trava_dono_para_filho.sh:28`, `bin/tests/test_trava_dono_para_filho.sh:46`, `bin/tests/test_trava_dono_para_filho.sh:96`). |
| `bin/tests/test_trava_dono_para_filho.sh:91` | Alpine deliberadamente cru e destacado, simulando suíte alheia; `manage.py test` é argumento de `:`, não execução de Django (`bin/tests/test_trava_dono_para_filho.sh:92`). |

O comando Docker reconstituído pela própria trava é o executor genérico dos lançamentos protegidos, não uma suíte adicional (`bin/trava_teste.sh:138`, `bin/trava_teste.sh:151`). Os Docker escritos em fixtures de scanner/documentação são texto gerado, não nascimentos executados por esses fixtures (`bin/tests/test_trava_teste.sh:158`, `bin/tests/test_trava_teste.sh:164`, `bin/tests/test_suite_sh.sh:65`).

Entradas indiretas e hooks:

- Cron horário → `vigia_arvore.sh`; cron da sombra → `sombra.sh --refazer && sombra.sh --bloco` (`app/config/crons.py:277`, `app/config/crons.py:331`, `app/config/crons.py:332`).
- O instalador corrente produz wrapper Git que faz `exec bash …/bin/pre-push.sh "$@"` (`bin/hooks.sh:10`, `bin/hooks.sh:56`).
- A fonte alternativa `hooks_fonte.py` ainda lê o hook cru de `app/core/hooks/pre-push.sh`; o outro instalador em `app/` copia `bin/pre-push.sh` (`app/core/hooks/hooks_fonte.py:17`, `app/core/hooks/hooks_fonte.py:23`, `app/core/hooks/hooks.sh:10`, `app/core/hooks/hooks.sh:35`).
- Em `scripts/`, o drill usa `$DC exec -T web python manage.py shell`; é ensaio fora das duas portas, mas não `compose run` nem `manage.py test` (`scripts/drill_restore.sh:4`, `scripts/drill_restore.sh:6`).
- Além do recorte pedido, o integrador em `bin/` chega ao runner Python `_rodar_testes()`: trava direta, espera padrão 5400, `ESTEIRA_QUEM=esteira_vigia:<banco>`, recursos comuns, DB informado e CI (`bin/integrador.sh:52`, `app/core/integrador_lote.py:373`, `app/core/esteira_vigia.py:767`, `app/core/esteira_vigia.py:781`, `app/core/esteira_vigia.py:790`).
- Ocorrências executáveis de `docker compose run`, `docker-compose run` ou `docker exec … manage.py test`: nenhuma encontrada no censo **[não verificado quanto à ausência absoluta]**.

**2. Lançamento, grupo/sessão e dono**

`bin/suite.sh` termina com **`exec env … bash trava_teste.sh …`**: substitui seu processo pelo executor da trava, preservando o PID através dos `exec`; não destaca o job nesse ponto (`bin/suite.sh:96`).

A trava mantém o flock no **FD 9** e lança o comando assim:

```bash
"$@" 9>&- <&0 &
[nome]=$!
wait "$[nome]"
```

Portanto, o comando é assíncrono, o pai espera por ele e o filho não herda o FD 9 (`bin/trava_teste.sh:96`, `bin/trava_teste.sh:97`, `bin/trava_teste.sh:151`, `bin/trava_teste.sh:153`).

Não há criação explícita de sessão ou grupo nesse lançamento. Em Bash sem job control, o processo lançado herda PGID/SID; o código não garante isolamento por `setsid` nem grava esses identificadores (`bin/suite.sh:96`, `bin/trava_teste.sh:151`). **PGID/SID numéricos e opções efetivas do shell: [não verificado].**

O container não integra a descendência do Bash como um subprocesso comum: a implementação declara que ele pertence ao lado do daemon Docker e usa label para alcançá-lo (`bin/trava_teste.sh:22`, `bin/trava_teste.sh:25`, `bin/trava_teste.sh:144`).

| Registro | Conteúdo / identidade |
|---|---|
| `/tmp/juliani_db_test.lock` | Arquivo usado pelo flock; posse mantida pelo FD 9 do processo da trava (`bin/trava_teste.sh:34`, `bin/trava_teste.sh:96`, `bin/trava_teste.sh:97`). |
| `/tmp/juliani_db_test.lock.quem` | Texto `<ESTEIRA_QUEM ou basename>:<PID> desde <dia/mês hora>`; no EXIT vira `livre desde …` (`bin/trava_teste.sh:36`, `bin/trava_teste.sh:102`, `bin/trava_teste.sh:103`). |
| Label do container | `hasner.trava.juliani_db_test.lock=<PID>-<starttime>`; o starttime vem do campo 22 de `/proc/<pid>/stat` (`bin/trava_teste.sh:37`, `bin/trava_teste.sh:39`, `bin/trava_teste.sh:45`, `bin/trava_teste.sh:101`, `bin/trava_teste.sh:138`). |
| Obra, PID do filho, PGID/SID | **Não são persistidos como campos nesse registro.** `[nome]=$!` fica em memória; o identificador persistido usa o PID da trava (`bin/trava_teste.sh:101`, `bin/trava_teste.sh:102`, `bin/trava_teste.sh:152`). |

Assim, o “dono” verificável é **o processo da trava**, com identidade PID+starttime; o nome humano é apenas o chamador informado em `ESTEIRA_QUEM`, sem vínculo obrigatório com uma obra (`bin/trava_teste.sh:36`, `bin/trava_teste.sh:47`, `bin/trava_teste.sh:101`).

Leitores e ações:

- **`--quem`** tenta flock não bloqueante. Presa: lê `.quem`, mas recusa dizer “livre” se o texto estiver vazio/mentiroso. Livre: consulta Docker e pode responder `ORFAO`, `FORA DA TRAVA`, `nao sei` ou `livre` (`bin/trava_teste.sh:60`, `bin/trava_teste.sh:63`, `bin/trava_teste.sh:65`, `bin/trava_teste.sh:71`, `bin/trava_teste.sh:75`, `bin/trava_teste.sh:80`).
- **Timeout da aquisição** lê `.quem` para informar quem está segurando e retorna 75 (`bin/trava_teste.sh:98`, `bin/trava_teste.sh:99`).
- **Nova aquisição** lê labels, valida PID+starttime e tenta parar órfãos de dono morto; dono ainda vivo sem flock provoca 75 (`bin/trava_teste.sh:115`, `bin/trava_teste.sh:121`, `bin/trava_teste.sh:126`).
- **TERM/INT/HUP** consultam a label do próprio dono, tentam parar containers, enviam TERM ao PID direto do filho, esperam e saem 143; não há kill de grupo nesse handler (`bin/trava_teste.sh:141`, `bin/trava_teste.sh:144`, `bin/trava_teste.sh:146`, `bin/trava_teste.sh:150`).
- **Handoff** consulta somente o flock para publicar LIVRE/TOMADA; não lê `.quem` nem consulta containers nessa seção (`bin/handoff_sessao.sh:223`, `bin/handoff_sessao.sh:230`).
- **Hook Stop** consulta `ps -eo args` e reconhece `manage.py test`/`git push`; esse caminho não verifica posse da trava (`bin/hook_stop_fila1.py:373`, `bin/hook_stop_fila1.py:386`, `bin/hook_stop_fila1.py:392`, `bin/hook_stop_fila1.py:395`). Para `ESPERANDO`, valida produtor/PID ou avanço de arquivo declarado no painel, por outro mecanismo (`bin/hook_stop_fila1.py:523`, `bin/hook_stop_fila1.py:645`, `bin/hook_stop_fila1.py:677`).

A sombra tem propriedade separada: flock em `logs/sombra.lock`, FD 8 herdável, e log JSONL com `ts`, `quem`, `espera_s`, `ok`, `timeout_s`, `pid`; também não há campo obra nesse registro (`bin/sombra_trava.sh:16`, `bin/sombra_trava.sh:26`, `bin/sombra_trava.sh:30`, `bin/sombra_trava.sh:38`).

**3. Existe recusa de Docker de teste fora da trava?** [nao verificado]

**Não há uma barreira geral demonstrada no código inspecionado.** Há recusa estática pelo selo e defesa posterior dentro da trava; o próprio teste cria um container cru e exige que ele permaneça vivo (`bin/tests/test_trava_teste.sh:145`, `bin/trava_teste.sh:129`, `bin/tests/test_trava_dono_para_filho.sh:91`, `bin/tests/test_trava_dono_para_filho.sh:99`).

- O scanner acusa comandos sem trava e faz a régua falhar antes da suíte; não intercepta o executável Docker (`bin/tests/test_trava_teste.sh:145`, `bin/regua.sh:138`, `bin/regua.sh:140`, `bin/regua.sh:143`).
- O PreToolUse configurado chama `disjuntor.py pretool`; Bash principal comum sai desse fluxo sem negativa logo na entrada (`.claude/settings.json:68`, `bin/disjuntor.py:1431`, `bin/disjuntor.py:1432`).
- O wrapper Git apenas delega ao pre-push (`bin/hooks.sh:56`).
- A label é adicionada somente quando os dois primeiros argumentos recebidos são exatamente `docker run`; não é uma credencial exigida pelo Docker (`bin/trava_teste.sh:109`, `bin/trava_teste.sh:138`).
- Suíte externa sem label é detectada pelo texto `manage.py test` no comando do container. Quem espera/recusa é **a nova suíte protegida**, não o lançamento externo (`bin/trava_teste.sh:55`, `bin/trava_teste.sh:56`, `bin/trava_teste.sh:111`, `bin/trava_teste.sh:130`).

A lacuna concreta é o hook cru em `app/`, excluído da análise shell do scanner; seu uso efetivo como hook instalado permanece **[não verificado]** (`app/core/hooks/pre-push.sh:54`, `bin/tests/test_trava_teste.sh:130`).

**4. Selos/testes e o que fixam** [nao verificado]

| Selo/teste | Afirmações | [nao verificado]
|---|---|
| `bin/tests/test_trava_teste.sh:20` | A trava existe e é executável. |
| `bin/tests/test_trava_teste.sh:48`, `bin/tests/test_trava_teste.sh:83`, `bin/tests/test_trava_teste.sh:87`, `bin/tests/test_trava_teste.sh:120`, `bin/tests/test_trava_teste.sh:145` | Scanner junta continuações; exige trava no comando/função shell ou na unidade AST Python; acusa infrações. |
| `bin/tests/test_trava_teste.sh:150`, `bin/tests/test_trava_teste.sh:183` | Scanner deve reconhecer os runners canônicos e acusar exatamente os quatro fixtures indevidos, incluindo `REGUA_DB` sem trava. |
| `bin/tests/test_trava_teste.sh:202`, `bin/tests/test_trava_teste.sh:207` | Testes dinâmicos usam cópia com lock próprio; isolamento inválido aborta. |
| `bin/tests/test_trava_teste.sh:218`, `bin/tests/test_trava_teste.sh:225` | Serialização: ordem `A A_fim B`. |
| `bin/tests/test_trava_teste.sh:231`, `bin/tests/test_trava_teste.sh:236` | Sem vez, retorno 75. |
| `bin/tests/test_trava_teste.sh:244`, `bin/tests/test_trava_teste.sh:251`, `bin/tests/test_trava_teste.sh:257` | `.quem` dizendo “livre” não pode vencer flock preso; depois de soltar, aceita livre ou suíte externa. |
| `bin/tests/test_trava_dono_para_filho.sh:46`, `bin/tests/test_trava_dono_para_filho.sh:54`, `bin/tests/test_trava_dono_para_filho.sh:55` | TERM no dono remove seu container e retorna 143. |
| `bin/tests/test_trava_dono_para_filho.sh:60`, `bin/tests/test_trava_dono_para_filho.sh:66`, `bin/tests/test_trava_dono_para_filho.sh:72` | KILL deixa container reconhecido como `ORFAO` por `--quem`. |
| `bin/tests/test_trava_dono_para_filho.sh:75`, `bin/tests/test_trava_dono_para_filho.sh:77`, `bin/tests/test_trava_dono_para_filho.sh:80` | Próxima aquisição para órfão, anuncia-o no stderr e executa com sucesso. |
| `bin/tests/test_trava_dono_para_filho.sh:91`, `bin/tests/test_trava_dono_para_filho.sh:98`, `bin/tests/test_trava_dono_para_filho.sh:99` | Suíte sem label continua viva; nova suíte protegida retorna 75. |
| `bin/tests/test_suite_sh.sh:37`, `bin/tests/test_suite_sh.sh:42`, `bin/tests/test_suite_sh.sh:51`, `bin/tests/test_suite_sh.sh:55`, `bin/tests/test_suite_sh.sh:57` | Comando canônico chama `suite.sh`; porta contém trava e teste, lê LABELS da régua e não contém `DB_PASSWORD`. |
| `bin/tests/test_suite_sh.sh:77`, `bin/tests/test_suite_sh.sh:80`, `bin/tests/test_suite_sh.sh:85` | Fixture crua reprova, porta aprova e recorte da documentação não invade prosa. |
| `app/core/tests/test_pista_ocupada_nao_e_veredito.py:51`, `app/core/tests/test_pista_ocupada_nao_e_veredito.py:60`, `app/core/tests/test_pista_ocupada_nao_e_veredito.py:69` | Runner Python transforma 75 em `PistaOcupada`; usa trava/cpuset e evita senha no argv; OK/FAILED continuam vereditos. |
| `app/core/tests/test_pista_ocupada_nao_e_veredito.py:73`, `app/core/tests/test_pista_ocupada_nao_e_veredito.py:82` | Ocupação não vira árvore vermelha; quarentena não libera teste sem run. |
| `bin/tests/test_handoff_sessao.sh:115`, `bin/tests/test_handoff_sessao.sh:116`, `bin/tests/test_handoff_sessao.sh:117` | Handoff consulta flock e inclui locks de teste/sombra. |
| `bin/tests/test_hook_nao_e_copia.sh:33`, `bin/tests/test_hook_nao_e_copia.sh:36`, `bin/tests/test_hook_nao_e_copia.sh:43` | Hook instalado delega ao pre-push, não carrega suíte/Docker/senha e tem até 12 linhas de código. |



[truncado em 20000 de 20437 caracteres]

## 31/10 19:54 · sessao 3a1532fe8ade · - · - · origem=Ronald (tmux)

Censo das entradas reais encontradas; `T` abaixo significa chamada direta a `bin/trava_teste.sh`, nunca indireta por `bin/suite.sh`, salvo a própria suíte (`bin/suite.sh:96`). Argumentos comuns `$TESTE_DOCKER` expandem para `--cpuset-cpus $TESTE_CPUSET --cpus $TESTE_CPUS --memory $TESTE_MEM`, padrões `4-7`, `4`, `2g` (`bin/recursos.sh:41`, `bin/recursos.sh:61`, `bin/recursos.sh:69`, `bin/recursos.sh:81`).

| Nascimento | Passagem/dono/espera | Comando e variantes |
|---|---|---|
| `bin/suite.sh:97` | `T`, `ESTEIRA_QUEM=${ESTEIRA_QUEM:-suite}`, espera repassada por `--espera` (`bin/suite.sh:49`, `bin/suite.sh:96`) | `manage.py test ${ONLY:-$LABELS} --settings=config.settings.ci --noinput`, paralelo padrão 4, extras após `--`; app vivo ou `--dir`, `REGUA_DB` repassado (`bin/suite.sh:43`, `bin/suite.sh:74`, `bin/suite.sh:89`, `bin/suite.sh:102`). |
| `bin/regua.sh:166` | `T`, `regua`, espera padrão (`bin/regua.sh:165`) | `${ONLY:-$LABELS}`, CI, `--noinput --parallel 4 $_ORDEM`, app `$APP` (`bin/regua.sh:167`). |
| `bin/pre-push.sh:143` | Função `_teste_irmao`; `T`, `pre-push`, espera padrão (`bin/pre-push.sh:122`, `bin/pre-push.sh:142`) | Arsenal `$LABELS`, CI, paralelo 4 (`bin/pre-push.sh:186`); control plane `tenants`, `teste_motor`, sem paralelo (`bin/pre-push.sh:198`). App do commit `$ARVORE_PUSH/app` (`bin/pre-push.sh:144`). |
| `bin/placar_code.sh:127` | `T`, `placar_code`, `--espera 120` (`bin/placar_code.sh:126`) | `REGUA_DB=test_juliani_placar`, `core.tests.test_bug116_credencial_deixa_trilha.CContratoCredencialSemTrilhaTest`, CI e `--noinput` (`bin/placar_code.sh:128`). |
| `bin/regua_relogio.sh:52` | `T`, `regua_relogio`, espera padrão (`bin/regua_relogio.sh:51`) | `$APPS --settings=cfg_tz --noinput`, shim montado `/var/tmp/probe`, `PROBE_TZ=$TZ_SONDA` (`bin/regua_relogio.sh:53`). |
| `bin/regua_calendario.sh:47` | `T`, `regua_calendario`, espera padrão (`bin/regua_calendario.sh:46`) | `$LABELS`, `ci_calendario`, paralelo 4; três datas `d20`, `d21`, `d01`, banco `test_juliani_cal$nome` e `RELOGIO_FAKE=$alvo` (`bin/regua_calendario.sh:44`, `bin/regua_calendario.sh:48`). |
| `bin/vigia_arvore.sh:84` | Função `suite`; `T`, `vigia_arvore`, espera padrão (`bin/vigia_arvore.sh:77`, `bin/vigia_arvore.sh:83`) | CI; primeiro `$LABELS`, DB `test_juliani_vigia`, paralelo 2 (`bin/vigia_arvore.sh:88`); depois `$IDS`, DB `test_juliani_vigia_b`, sem paralelo (`bin/vigia_arvore.sh:106`). Monta snapshot (`bin/vigia_arvore.sh:85`). |
| `bin/vigia_arvore.sh:132` | `T`, `vigia_arvore`, espera padrão (`bin/vigia_arvore.sh:131`) | Confirma `$IDS2` na árvore viva, DB `test_juliani_vigia_viva`, CI, sem paralelo (`bin/vigia_arvore.sh:132`). |
| `bin/isolamento.sh:34` | Função `_iso_roda`; `T`, `isolamento`, espera padrão (`bin/isolamento.sh:23`, `bin/isolamento.sh:33`) | `$ids`, CI, DB `$db`, snapshot; chamadas usam `test_juliani_iso_$fatia` (`bin/isolamento.sh:34`, `bin/isolamento.sh:49`, `bin/isolamento.sh:70`). |
| `bin/molde_fatia/rodar.sh:31` | `T`, `fatia`, `--espera 5400` (`bin/molde_fatia/rodar.sh:30`) | Selo `core.tests.test_selo_mapa_contrato`, CI, DB `test_juliani___FATIA__m`, app da cura (`bin/molde_fatia/rodar.sh:32`). |
| `bin/molde_fatia/rodar.sh:45` | Função `teste`; `T`, `fatia`, `--espera 5400` (`bin/molde_fatia/rodar.sh:37`, `bin/molde_fatia/rodar.sh:44`) | RED `__RED__`, DB `...r`; GREEN `__RED__ __GREEN__ __PACOTE__ --parallel 4`, DB `...g`; CI (`bin/molde_fatia/rodar.sh:52`, `bin/molde_fatia/rodar.sh:54`). |
| `bin/molde_autocura/rodar.sh:23` | Função `teste`; `T`, `autocura`, `--espera 5400` (`bin/molde_autocura/rodar.sh:18`, `bin/molde_autocura/rodar.sh:22`) | RED `$IDS`, DB `test_juliani_esacred`; GREEN `$LABELS --parallel 4`, DB `test_juliani_esacfull`; CI (`bin/molde_autocura/rodar.sh:27`, `bin/molde_autocura/rodar.sh:30`). |
| **`app/core/hooks/pre-push.sh:54`** | **Nenhuma trava de teste/suite**; função `_teste_irmao` roda Docker cru (`app/core/hooks/pre-push.sh:54`) | Arsenal `$LABELS`, CI, paralelo 2, credencial no `-e DB_PASSWORD` (`app/core/hooks/pre-push.sh:82`); `tenants`, `teste_motor` (`app/core/hooks/pre-push.sh:87`). Monta árvore viva (`app/core/hooks/pre-push.sh:55`). |

Sondas, ensaios e verificadores fora de `trava_teste.sh`/`suite.sh`: [nao verificado]

| Nascimento | Trabalho e controle próprio |
|---|---|
| `bin/sonda_frota.sh:41` | Shell tenant `juliani` que executa arquivo montado `/sonda/sonda.py`; app só leitura, env da casa, `$TESTE_DOCKER`; sem trava de teste no comando (`bin/sonda_frota.sh:41`). |
| `bin/sonda_leitura.sh:235` | Shell tenant `juliani`, settings `$SETTINGS`, rede `$REDE`, credencial `$ENVFILE`, `POSTGRES_DB=$BANCO`, código montado só leitura; sem trava de teste (`bin/sonda_leitura.sh:235`). |
| `bin/sombra.sh:183` | Função `container`, `config.settings.sombra`, `POSTGRES_DB=sombra`, cpuset de teste e `--cpus 1`, proxy morto, app só leitura (`bin/sombra.sh:182`). Ramas `--refazer`, `--bloco`, `--rodar` usam `trancar_sombra`, não trava de suíte (`bin/sombra.sh:438`, `bin/sombra.sh:451`, `bin/sombra.sh:487`). |
| `bin/simular_folha.sh:48` | Função `container`, sombra, cpuset de teste e `--cpus 1`; foto/par usam `trancar_sombra` (`bin/simular_folha.sh:47`, `bin/simular_folha.sh:67`, `bin/simular_folha.sh:96`). `comparar` também usa essa função (`bin/simular_folha.sh:87`). |
| `bin/diff_janela_he_total.sh:30` | Ensaio financeiro na sombra; chama `trancar_sombra "diff_janela_he_total $ROTULO"` (`bin/diff_janela_he_total.sh:18`); executa tenant command com mês/ano/empresas/piso/JSON (`bin/diff_janela_he_total.sh:38`). |
| `bin/r5_idempotencia_frota.sh:16` | Executa `/r5/r5_idempotencia_frota.py` em shell tenant, settings sombra, `$TESTE_DOCKER`; sem chamada a trava de teste/sombra no corpo anterior ao Docker (`bin/r5_idempotencia_frota.sh:7`, `bin/r5_idempotencia_frota.sh:24`). |
| `bin/prova_de_casca.sh:30` | `--network none`, settings saas, app só leitura e `/prova.py`; imagem obtida do `saas_ui`; sem trava de suíte (`bin/prova_de_casca.sh:27`, `bin/prova_de_casca.sh:30`). |
| `bin/esmeril_da_copia.sh:23` | Ruff/Vulture na cópia só leitura, `$TESTE_DOCKER`; sem trava de suíte (`bin/esmeril_da_copia.sh:23`). |
| `bin/molde_fatia/rodar.sh:19` | `gerar_diagrama --settings=config.settings.ci`, app da cura; Docker cru (`bin/molde_fatia/rodar.sh:19`). |
| `bin/molde_fatia/rodar.sh:21` | Ruff/Vulture; Docker cru (`bin/molde_fatia/rodar.sh:21`). |
| `bin/molde_autocura/rodar.sh:13` | Ruff; Docker cru (`bin/molde_autocura/rodar.sh:13`). |
| `bin/db_teste.sh:53` | Nasce persistente `juliani_db_test`, postgres 17 alpine, `-d`, restart `unless-stopped`, volume próprio e cpuset de teste; sem trava de suíte (`bin/db_teste.sh:21`, `bin/db_teste.sh:32`, `bin/db_teste.sh:53`). |
| `bin/sombra_db.sh:91` | Nasce persistente `sombra_db`, postgres 17 alpine, `-d`, restart `unless-stopped`, memória `$SOMBRA_DB_MEM`, cpuset de teste; sem trava de suíte (`bin/sombra_db.sh:40`, `bin/sombra_db.sh:48`, `bin/sombra_db.sh:91`). |

Os próprios selos também criam containers:

- `bin/tests/test_prepush_testa_o_commit.sh:97` e `bin/tests/test_prepush_testa_o_commit.sh:102`: duas verificações de existência de staticfiles dentro de container, com/sem montagem auxiliar; Docker cru e sem `$TESTE_DOCKER` (`bin/tests/test_prepush_testa_o_commit.sh:97`, `bin/tests/test_prepush_testa_o_commit.sh:102`).
- `bin/tests/test_inventario_pessoal_no_commit.sh:24`: função `_rodar` executa `inventario_pessoal_dos_modelos.py`, `$TESTE_DOCKER`, sem trava de suíte (`bin/tests/test_inventario_pessoal_no_commit.sh:23`).
- `bin/tests/test_trava_dono_para_filho.sh:46`, `bin/tests/test_trava_dono_para_filho.sh:60`, `bin/tests/test_trava_dono_para_filho.sh:96`: Alpine através de **cópia isolada** da trava, espera 5/5/2 s; `bin/tests/test_trava_dono_para_filho.sh:91` cria deliberadamente container sem trava/label, `-d`, simulação alheia `sleep 60; : python manage.py test alheia`. Os comandos “manage.py test” aqui são texto em `:`; não executam Django (`bin/tests/test_trava_dono_para_filho.sh:92`, `bin/tests/test_trava_dono_para_filho.sh:97`).
- `bin/tests/test_trava_teste.sh:160`, `bin/tests/test_suite_sh.sh:68` e demais `printf`/heredoc encontrados são fixtures textuais; não contei como nascimento real (`bin/tests/test_trava_teste.sh:158`, `bin/tests/test_suite_sh.sh:68`).

Entradas indiretas e hooks:

- Cron da suíte: `app/config/crons.py:277` agenda `bin/vigia_arvore.sh` às `5 * * * *`; cron sombra agenda `sombra.sh --refazer && sombra.sh --bloco` (`app/config/crons.py:331`).
- `.claude/settings.json:9` chama Stop hook; `.claude/settings.json:68` chama `disjuntor.py pretool`; não são chamadas Docker de teste nesses campos (`.claude/settings.json:9`, `.claude/settings.json:68`).
- O instalador corrente declara `FONTE=$R/bin/pre-push.sh` e grava wrapper `exec bash "$(git rev-parse --show-toplevel)/bin/pre-push.sh" "$@"` (`bin/hooks.sh:10`, `bin/hooks.sh:56`).
- Há fonte divergente: `app/core/hooks/hooks_fonte.py:17` aponta **a cópia local sem trava**, e `fonte_do_pre_push()` a lê (`app/core/hooks/hooks_fonte.py:20`). Já `app/core/hooks/hooks.sh:10` aponta `bin/pre-push.sh`, copiando-a na instalação (`app/core/hooks/hooks.sh:35`).
- Em `scripts/`, `drill_restore.sh` faz ensaio via `$DC exec -T web python manage.py shell`, fora das duas portas; não é `compose run` nem `manage.py test` (`scripts/drill_restore.sh:4`, `scripts/drill_restore.sh:6`).
- Não encontrei execução `docker compose run`, `docker-compose run` ou `docker exec ... manage.py test` no escopo inspecionado [não verificado quanto à exaustividade sem metadados Git]. Versionamento efetivo, hook instalado e processos/containers vivos [não verificado].

## 31/10 19:54 · sessao 8740280619f6 · - · - · origem=Ronald (tmux)

Auditoria estática dos selos e das recusas:

| Selo/teste | O que afirma | [nao verificado]
|---|---|
| `bin/tests/test_trava_teste.sh:20` | A trava existe e é executável. |
| `bin/tests/test_trava_teste.sh:48`, `:83`, `:87`, `:90`, `:125`, `:145` | Scanner junta continuações, descarta comentários/buscas de processo, exige trava no comando shell ou na função chamada; Python exige constante terminando em `trava_teste.sh` na mesma unidade AST. Varre `bin/` e `app/`, exclui diretórios `tests`; **shell somente em `bin/`**. `REGUA_DB` não dá isenção. |
| `bin/tests/test_trava_teste.sh:150` | Scanner tem de reconhecer suite, régua e pre-push como protegidos; evita universo vazio. |
| `bin/tests/test_trava_teste.sh:158`, `:183` | Fixtures exigem exatamente quatro infrações: função sem trava, continuação com Docker cru, `REGUA_DB` cru e execução Python sem trava; ignora comentário, busca, docstring e outro command `testar_pwa`. |
| `bin/tests/test_trava_teste.sh:202`, `:207` | Exercita cópia com lock próprio e aborta antes dos testes dinâmicos se a cópia não estiver isolada. |
| `bin/tests/test_trava_teste.sh:218`, `:225` | Dois chamadores produzem ordem `A A_fim B`: serialização. |
| `bin/tests/test_trava_teste.sh:231`, `:236` | Falta de vez termina com rc 75. |
| `bin/tests/test_trava_teste.sh:244`, `:251`, `:257` | `--quem` não pode anunciar livre com flock preso, mesmo com `.quem` mentiroso; após soltar aceita livre ou suite externa sem rótulo. |
| `bin/tests/test_trava_dono_para_filho.sh:28`, `:46`, `:54`, `:55` | Cópia com lock próprio; TERM no dono faz container desaparecer e devolve 143. |
| `bin/tests/test_trava_dono_para_filho.sh:60`, `:66`, `:69`, `:72` | KILL deixa container visível como `ORFAO` no `--quem`. |
| `bin/tests/test_trava_dono_para_filho.sh:75`, `:77`, `:80`, `:85` | Próxima aquisição para órfão, anuncia `ORFAO` no stderr, roda com rc 0 e depois deixa pista livre. |
| `bin/tests/test_trava_dono_para_filho.sh:91`, `:96`, `:98`, `:99` | Suite externa sem rótulo permanece viva; nova suite protegida espera e termina 75. |
| `bin/tests/test_suite_sh.sh:22`, `:32`, `:37`, `:42` | Bloco canônico da seção 3 de `CLAUDE.md` deve chamar `bin/suite.sh`; recorte pela estrutura do bloco. |
| `bin/tests/test_suite_sh.sh:46`, `:51`, `:53`, `:55`, `:57` | Porta existe, contém trava e `manage.py test`, lê LABELS de `regua.sh` e não contém `DB_PASSWORD`. São verificações textuais. |
| `bin/tests/test_suite_sh.sh:65`, `:77`, `:80`, `:85` | Fixture crua reprova, fixture pela porta aprova e recorte não invade prosa posterior. |
| `app/core/tests/test_pista_ocupada_nao_e_veredito.py:51`, `:60`, `:69`, `:73`, `:82` | rc 75 vira `PistaOcupada`, nunca False; runner passa pela trava, com cpuset, sem senha no argv; OK/FAILED continuam vereditos; isolamento não chama ocupação de árvore vermelha; quarentena não solta ninguém sem run. |
| `bin/tests/test_handoff_sessao.sh:115`, `:116`, `:117` | Handoff usa flock e inclui locks de teste e sombra. |
| `bin/tests/test_hook_nao_e_copia.sh:21`, `:33`, `:36`, `:43` | Hook instalado deve delegar para `bin/pre-push.sh`, conter até 12 linhas de código e não carregar suite/Docker/senha; fixture compara wrapper e cópia (`:47–51`). |

Os selos de host são executados pela régua antes da suite; o primeiro que falha impede o lançamento (`bin/regua.sh:138`, `:140`, `:143`). Não executei nenhum selo [não verificado: resultado de execução].

**Não há interceptador geral demonstrado que recuse o nascimento de todo Docker de teste fora da trava.** A recusa estática do scanner ocorre na régua (`bin/tests/test_trava_teste.sh:145`, `bin/regua.sh:140`). A defesa runtime recusa **a nova suite que entrou pela trava** quando encontra suite externa sem rótulo (`bin/trava_teste.sh:109`, `:111`, `:129`, `:133`); o teste deliberadamente cria essa suite crua e prova que permanece viva (`bin/tests/test_trava_dono_para_filho.sh:91`, `:99`).

O único PreToolUse declarado chama `disjuntor.py pretool` (`.claude/settings.json:68`); esse fluxo trata B1, agentes e Codex, e libera Bash principal comum já na entrada (`bin/disjuntor.py:1431`, `:1432`, `:1440`, `:1451`, `:1475`). O wrapper de Git gerado apenas delega ao pre-push (`bin/hooks.sh:56`); não envolve o executável Docker.

**Lacuna concreta:** `app/core/hooks/pre-push.sh:54` tem `_teste_irmao()` com Docker cru, chamado para arsenal em `:82` (`--parallel 2`, `:84`) e tenants em `:87` (`teste_motor`, `:88`). O scanner da trava não examina `.sh` de `app/` (`bin/tests/test_trava_teste.sh:130`). A fonte atualmente escolhida pelo instalador é `bin/pre-push.sh` (`bin/hooks.sh:10`), enquanto `app/core/hooks/hooks_fonte.py:17` aponta para a cópia de `app/`; instalação efetiva e versionamento nesta árvore sem `.git` [não verificado].

**Hook Stop:** está configurado em `.claude/settings.json:9`. Ele não consulta o arquivo de proprietário: `_tem_trabalho_em_curso()` executa `ps -eo args` e reconhece substring `manage.py test` ou `git push` (`bin/hook_stop_fila1.py:386`, `:392`, `:395`). Portanto detecta atividade por argv; não valida posse da trava (`bin/hook_stop_fila1.py:373`).

## 31/10 19:52 · sessao 4bd4d009df1b · PADRAO-DE-CIMA · verificador · origem=Code

**Não aprovaria o pre-pouso:** há um caminho de LIVRE sem prova sobre o histórico e um mutante aceito cuja morte vem da exceção artificial que ele próprio injeta. (`bin/disjuntor.py:1498`; `bin/disjuntor.py:1514`; `bin/tests/test_disjuntor.sh:1762`; `bin/tests/_mordida.sh:159`)

Execução integral dos selos, ocorrência em produção e confirmação Git do hash: **[nao verificado]**.

1. **LIVRE, julgamento e fontes ilegíveis**

   **Há uma lacuna no fallback.** Considere uma obra anteriormente disparada, com eventos ainda dentro das 24 horas, cujas linhas saíram do RELATO. O estado conserva esses eventos independentemente da permanência no texto. Se o estado estiver corrompido e o escopo ausente ou ilegível, `estado_pdc` aceita apenas a ausência da obra no RELATO atual e retorna LIVRE. Essa ausência não prova que inexista evento histórico na janela. (`bin/disjuntor.py:1386`; `bin/disjuntor.py:1288`; `bin/disjuntor.py:272`; `bin/disjuntor.py:1484`; `bin/disjuntor.py:1506`; `bin/disjuntor.py:1514`)

   Nesse caminho houve julgamento pelo fallback nesta chamada, mas **não houve decisão completa nem prova sobre o histórico**. O teste sem escopo mantém a obra com evento no RELATO; não exercita a combinação “evento histórico retirado do texto + estado corrupto + escopo ilegível”. (`bin/disjuntor.py:1494`; `bin/disjuntor.py:1495`; `bin/tests/test_padrao_de_cima.sh:253`; `bin/tests/test_padrao_de_cima.sh:259`; `bin/tests/test_padrao_de_cima.sh:260`)

   Para fontes efetivamente abertas, a proteção está correta: `OSError`, `SyntaxError` ou `UnicodeDecodeError` chegam ao tratamento que acrescenta a obra a `duvida` e pula a criação do evento novo. RELATO ausente/ilegível também não acrescenta eventos e produz NAO_SEI fora das disparadas. (`bin/disjuntor.py:1236`; `bin/disjuntor.py:1262`; `bin/disjuntor.py:1263`; `bin/disjuntor.py:1380`; `bin/disjuntor.py:1384`; `bin/disjuntor.py:712`; `bin/disjuntor.py:1373`; `bin/disjuntor.py:1454`; `bin/disjuntor.py:1518`)

   **Ressalva anterior à abertura:** `isfile=False` é tratado como ausência. Se um caminho existente, porém inacessível, produzir esse resultado, ele será pulado; o resolvedor poderá devolver `[]`, gravar `res=2` e permitir LIVRE. Reprodução dessa condição de permissões: **[nao verificado]**. (`bin/disjuntor.py:1233`; `bin/disjuntor.py:1259`; `bin/disjuntor.py:1284`; `bin/disjuntor.py:1386`; `bin/disjuntor.py:1520`)

   No fluxo normal de `main_pretool`, a marca é limpa na entrada e acrescentada somente depois de `_nega_pdc` confirmar LIVRE; não encontrei reaproveitamento serial de julgamento anterior. Já JSON inválido no payload pode liberar a ferramenta sem julgamento PDC: o envelope recebe `{}`, não reconhece obra e termina na falha aberta. Isso é liberação da ferramenta, não retorno literal `PDC_LIVRE`; aceitação desse payload pela ferramenta real: **[nao verificado]**. (`bin/disjuntor.py:2016`; `bin/disjuntor.py:1991`; `bin/disjuntor.py:1995`; `bin/disjuntor.py:1539`; `bin/disjuntor.py:2019`; `bin/disjuntor.py:2043`; `bin/disjuntor.py:2052`)

2. **Escritores, leitores e trava**

   | Dado | Quem escreve | Quem lê |
   |---|---|---|
   | Escopo: `disparadas`, `duvida`, `obras_com_evento` | `_pdc_grava_escopo` monta e instala o arquivo por `replace`. (`bin/disjuntor.py:1467`; `bin/disjuntor.py:1473`) | `_pdc_escopo_lido`; `estado_pdc` consulta os três campos. (`bin/disjuntor.py:1480`; `bin/disjuntor.py:1500`; `bin/disjuntor.py:1502`) |
   | `_PDC_LIVRE_NESTA` | Inicialização, limpeza na entrada e inclusão após julgamento. (`bin/disjuntor.py:1559`; `bin/disjuntor.py:2016`; `bin/disjuntor.py:1995`) | `_ramo_pdc`. (`bin/disjuntor.py:1571`) |
   | Eventos do resolvedor | `decide_pdc._fn` cria eventos e atualiza `entidades/res` herdados. (`bin/disjuntor.py:1386`; `bin/disjuntor.py:1389`) | Cache de resolução, janela/agrupamento, nomeação e painel. (`bin/disjuntor.py:1375`; `bin/disjuntor.py:1288`; `bin/disjuntor.py:1297`; `bin/disjuntor.py:1427`; `bin/disjuntor.py:1580`) |
   | `visto_em` | Atribuído na criação; a reclassificação preserva o campo. (`bin/disjuntor.py:1386`; `bin/disjuntor.py:1389`) | Filtro da janela. (`bin/disjuntor.py:1289`) |

   Não identifiquei segundo escritor persistente desses dados no fluxo auditado. A chamada ao escritor do escopo ocorre em `_anuncia`; `_com_estado` mantém `LOCK_EX` durante a decisão, a gravação do estado e o anúncio. A marca em memória é escrita fora dessa trava, mas pertence ao envelope da chamada. (`bin/disjuntor.py:1444`; `bin/disjuntor.py:260`; `bin/disjuntor.py:275`; `bin/disjuntor.py:281`; `bin/disjuntor.py:293`; `bin/disjuntor.py:296`; `bin/disjuntor.py:1995`)

3. **As três provas de `_mordida.sh` ainda admitem falsos positivos**

   | Prova | Contraprova |
   |---|---|
   | Marca do assert | Um caso sintético `(return 1); false` deixa a marca do `return` da subshell e termina pelo `false`. O harness aceita porque compara apenas última marca e rc. Para expressões contendo `$`, nem compara o valor nem comprova o “último ato” anunciado no comentário. (`bin/tests/_mordida.sh:67`; `bin/tests/_mordida.sh:114`; `bin/tests/_mordida.sh:116`; `bin/tests/_mordida.sh:118`) |
   | Exceções por `sys.monitoring` | Um mutante que execute `os._exit(1)` antes da decisão não levanta exceção Python. `i14_mesma` receberia rc 1 e executaria seu `return 1`: marca válida, nenhuma exceção nova, regra sequer executada. Execução desse mutante: **[nao verificado]**. (`bin/tests/test_padrao_de_cima.sh:90`; `bin/tests/test_padrao_de_cima.sh:116`; `bin/tests/_mordida.sh:113`; `bin/tests/_mordida.sh:161`) |
   | Declaração da guarda | Basta declarar exatamente uma exceção plantada pelo próprio patch. O harness verifica presença e depois a retira da comparação; não verifica que o patch removeu uma guarda. `P17-passa-por-excecao` é um exemplo existente. (`bin/tests/_mordida.sh:150`; `bin/tests/_mordida.sh:155`; `bin/tests/_mordida.sh:159`; `bin/tests/test_disjuntor.sh:1762`) |

   A sonda também perde **contexto e multiplicidade**: transforma ocorrências em conjuntos de tipo/arquivo/função/texto. Um mutante que redirecionasse toda leitura do estado para um caminho ausente produziria repetidamente o mesmo `FileNotFoundError` já esperado na primeira leitura do controle; reiniciaria o estado e cairia no contador de anúncios, mas a assinatura estaria autorizada pela base. Execução desse mutante: **[nao verificado]**. (`bin/tests/_mordida.sh:131`; `bin/tests/_mordida.sh:136`; `bin/tests/_mordida.sh:160`; `bin/orcamento_contexto.py:74`; `bin/disjuntor.py:269`; `bin/tests/test_padrao_de_cima.sh:120`)

   Os meta-casos cobrem exceções não declaradas, tipo errado, função errada no selo geral, prefixo, declaração ausente, `ValueError` e morte **sem marca alguma**. Não cobrem marca anterior de subshell, expressão sem último ato, repetição indevida de assinatura da base ou declaração exata de exceção artificial. (`bin/tests/test_disjuntor.sh:1799`; `bin/tests/test_disjuntor.sh:1803`; `bin/tests/test_disjuntor.sh:1807`; `bin/tests/test_disjuntor.sh:1809`; `bin/tests/test_disjuntor.sh:1812`; `bin/tests/test_padrao_de_cima.sh:687`; `bin/tests/test_padrao_de_cima.sh:705`)

4. **Mutantes reancorados e declarados**

   **Sim: `P17-passa-por-excecao` é vacuoso para a propriedade anunciada.** Ele substitui o cálculo por `1/0` e declara essa mesma `ZeroDivisionError`. O caso cai já na primeira exigência da mensagem orçamentária. Como a chamada é de verificador, o envelope pode responder B3-NAO-SEI e negar a ferramenta; portanto a mordida nem demonstra o anunciado “passa por exceção”. (`bin/tests/test_disjuntor.sh:1762`; `bin/tests/test_disjuntor.sh:525`; `bin/tests/test_disjuntor.sh:529`; `bin/disjuntor.py:1946`; `bin/disjuntor.py:2034`; `bin/disjuntor.py:2041`)

   Nos demais examinados há vínculo estático com a costura:

   | Mutantes | Evidência |
   |---|---|
   | `resolvedor-vira-vazio`, `resolvedor-sem-duvida`, `escopo-fora-da-trava` | Os casos exigem NAO_SEI, ausência de evento gravado e trava tomada. (`bin/tests/test_padrao_de_cima.sh:631`; `bin/tests/test_padrao_de_cima.sh:636`; `bin/tests/test_padrao_de_cima.sh:637`; `bin/tests/test_padrao_de_cima.sh:361`; `bin/tests/test_padrao_de_cima.sh:364`; `bin/tests/test_padrao_de_cima.sh:408`) |
   | `B3-ref-fora-da-pergunta`, `B3-symlink` | Há referência ruim fora da pergunta e link em diretório, respectivamente; o segundo conserva uma mordida comportamental além da exceção do link final. (`bin/tests/test_disjuntor.sh:1659`; `bin/tests/test_disjuntor.sh:1661`; `bin/tests/test_disjuntor.sh:740`; `bin/tests/test_disjuntor.sh:739`) |
   | `B3-foto-parcial`, `B3-foto-parcial-engolida` | Reintroduzem leitura textual parcial; o caso verifica que a linha anterior ao disparo não libere depois. (`bin/tests/test_disjuntor.sh:1663`; `bin/tests/test_disjuntor.sh:1664`; `bin/tests/test_disjuntor.sh:1023`; `bin/tests/test_disjuntor.sh:1031`) |
   | `P5`, `P7`, `P9`, `b3-sem-DESLIGADO` | Exercitam, respectivamente, B4 durante DIFF, entrada de tipo inválido, colisão do temporário fixo e chave desligada diante de estado corrupto. (`bin/tests/test_disjuntor.sh:475`; `bin/tests/test_disjuntor.sh:462`; `bin/tests/test_disjuntor.sh:653`; `bin/tests/test_padrao_de_cima.sh:326`; `bin/tests/test_padrao_de_cima.sh:327`) |
   | `P11-fifo-prende` | Prova preservação da decisão com FIFO presente; não prova que retirar o filtro necessariamente prende, pois a abertura ainda rejeita arquivo não regular. (`bin/tests/test_disjuntor.sh:1740`; `bin/tests/test_disjuntor.sh:580`; `bin/orcamento_contexto.py:77`) |

   Resultados executados desses mutantes e controles: **[nao verificado]**.

5. **Classes já reprovadas**

   | Classe | Conferência |
   |---|---|
   | Segundo juiz | As portas PDC convergem em `estado_pdc/decide_pdc`; o vocabulário é compartilhado com LEI-AKITA. Não identifiquei duplicação nesses consumidores. (`bin/disjuntor.py:1536`; `bin/disjuntor.py:1549`; `bin/disjuntor.py:1494`; `bin/disjuntor.py:1333`; `bin/tests/test_lei_akita.sh:89`) |
   | Segundo escritor fora da porta | Não identificado nos campos persistentes auditados; escopo e eventos passam pela transição sob trava. (`bin/disjuntor.py:1386`; `bin/disjuntor.py:1444`; `bin/disjuntor.py:293`) |
   | Relógio fora da borda | **Permanece:** o wrapper não injeta `agora`; `decide_pdc` lê `time.time()` internamente. O PreToolUse injeta o instante na entrada. (`bin/disjuntor.py:1549`; `bin/disjuntor.py:1353`; `bin/disjuntor.py:2022`; `bin/disjuntor.py:2025`) |
   | Derivação paralela | Agrupamento e reconhecimento são compartilhados; porém o fallback deriva LIVRE da ausência no RELATO, sem reconstruir a janela histórica — lacuna do item 1. (`bin/disjuntor.py:1292`; `bin/disjuntor.py:1181`; `bin/disjuntor.py:1508`; `bin/disjuntor.py:1514`) |
   | Selo sem mordida | **Reprovado pelas contraprovas do harness e pelo P17.** (`bin/tests/_mordida.sh:116`; `bin/tests/_mordida.sh:159`; `bin/tests/test_disjuntor.sh:1762`) |
   | Ordem de trava | Escopo sob trava do estado; wrapper adquire slot, depois trava da resposta, depois consulta B3/PDC, antes do Codex. (`bin/disjuntor.py:1444`; `bin/disjuntor.py:293`; `bin/codex_pergunta.sh:145`; `bin/codex_pergunta.sh:163`; `bin/codex_pergunta.sh:168`; `bin/codex_pergunta.sh:177`) |
   | Pouso antes do pré-requisito | Contrato aprovado, certificações e sequência real de pousos: **[nao verificado]**. | [nao verificado]

## 31/10 19:51 · sessao 35b9665cca65 · - · - · origem=Ronald (tmux)

Achados do harness, por leitura estática:

- **Marca não prova último ato.** `_mordida_marca` registra `return` dentro de subshell do corpo; `_mordida_morre` compara somente a última marca e o rc, sem verificar o comando final. Caso sintético `(return 1); false` deixa marca `return 1` e termina fora do assert com rc 1, aceito pelo ramo literal. Expressões com `$` são aceitas sem comparar sequer seu valor. (`bin/tests/_mordida.sh:67`; `bin/tests/_mordida.sh:114`; `bin/tests/_mordida.sh:118`)
  
  Reproduzi somente a trap em memória, sem arquivos: saída `marca=return 1` / `rc-final=1`. Execução integral do harness: [nao verificado].

- **Sonda não prova que a regra rodou.** Um mutante que execute `os._exit(1)` antes da decisão mata o processo sem exceção Python; `i14_mesma` recebe rc 1 e executa seu `return 1`, cuja marca satisfaz o harness. (`bin/tests/test_padrao_de_cima.sh:90`; `bin/tests/test_padrao_de_cima.sh:116`; `bin/tests/_mordida.sh:113`; `bin/tests/_mordida.sh:160`)

- **Linha de base perde contexto e multiplicidade.** As exceções viram conjuntos de tipo, arquivo, função e texto da linha; mensagem, argumento e quantidade desaparecem. (`bin/tests/_mordida.sh:131`; `bin/tests/_mordida.sh:136`; `bin/tests/_mordida.sh:160`)
  
  Contraprova estática concreta: inserir antes de `os.open` uma alteração de `caminho` para acrescentar `.ausente` quando termina em `/disjuntor_estado.json`, preservando o texto da linha do `os.open`. O controle já produz `FileNotFoundError` nessa linha na primeira leitura do estado ausente. O mutante produziria a mesma assinatura em todas as leituras, recriaria `{}` e reanunciaria os disparos; `i14_mesma` cairia no contador de anúncios. A sonda absorveria a repetição na base. (`bin/orcamento_contexto.py:74`; `bin/disjuntor.py:267`; `bin/disjuntor.py:269`; `bin/tests/test_padrao_de_cima.sh:38`; `bin/tests/test_padrao_de_cima.sh:120`) Execução desse mutante: [nao verificado].

- **Declaração prova presença, não legitimidade causal.** A exceção declarada é retirada de `vistas` antes da comparação com a base; nada verifica que o patch removeu uma guarda. O mutante existente `P17-passa-por-excecao` planta `o = 1 / 0` e declara precisamente sua própria `ZeroDivisionError`. Isso autoriza o mecanismo que deveria distinguir contaminação de mordida. (`bin/tests/_mordida.sh:142`; `bin/tests/_mordida.sh:155`; `bin/tests/_mordida.sh:159`; `bin/tests/test_disjuntor.sh:1762`)

Os meta-casos cobrem exceção não declarada, tipo errado, função errada no selo geral, declaração por prefixo, declaração ausente, exceção fora dos cinco tipos e morte sem marca alguma. Não cobrem marca anterior de subshell, expressão sem último ato, repetição de assinatura já presente na base ou declaração exata de exceção plantada. (`bin/tests/test_disjuntor.sh:1799`; `bin/tests/test_disjuntor.sh:1817`; `bin/tests/test_padrao_de_cima.sh:687`; `bin/tests/test_padrao_de_cima.sh:707`)

## 31/10 19:50 · sessao f390df4bc2f0 · - · - · origem=Ronald (tmux)

Achado principal: **há LIVRE sem prova sobre o histórico quando estado e escopo estão ilegíveis/ausentes**. `_com_estado` levanta para JSON corrupto (`bin/disjuntor.py:268`, `bin/disjuntor.py:272`); `_pdc_escopo_lido` transforma escopo ausente/corrupto em `None` (`bin/disjuntor.py:1480`, `bin/disjuntor.py:1484`, `bin/disjuntor.py:1486`); `estado_pdc` então ignora as provas de `duvida`/`obras_com_evento` e retorna LIVRE se a obra não aparece no RELATO atual (`bin/disjuntor.py:1498`, `bin/disjuntor.py:1502`, `bin/disjuntor.py:1506`, `bin/disjuntor.py:1514`). Um evento ainda na janela pode ter saído do RELATO: o estado retém eventos por SHA e usa `visto_em`, independentemente da permanência no texto (`bin/disjuntor.py:1375`, `bin/disjuntor.py:1386`, `bin/disjuntor.py:1288`). Portanto, obra anteriormente disparada + eventos retirados do RELATO + estado corrupto + escopo ilegível → LIVRE; ocorrência em produção [não verificado].

As fontes efetivamente abertas pelo resolvedor propagam OSError/SyntaxError/UnicodeDecodeError até o `except` que adiciona a obra a `duvida` e não cria evento novo (`bin/disjuntor.py:1236`, `bin/disjuntor.py:1262`, `bin/disjuntor.py:1263`, `bin/disjuntor.py:1379`, `bin/disjuntor.py:1381`, `bin/disjuntor.py:1384`). Porém, os testes `isfile` anteriores transformam `False` em ausência e pulam a abertura (`bin/disjuntor.py:1233`, `bin/disjuntor.py:1259`); se um caminho inacessível resultar em `False`, o resolvedor poderá gravar `entidades=[]`, `res=2` e liberar (`bin/disjuntor.py:1284`, `bin/disjuntor.py:1386`, `bin/disjuntor.py:1520`). Reprodução dessa condição de permissões [não verificado].

RELATO ausente ou com erro de leitura retorna `None` (`bin/disjuntor.py:707`, `bin/disjuntor.py:712`); encoding inválido também vira texto `None` (`bin/disjuntor.py:1364`, `bin/disjuntor.py:1365`); nesse caminho não entram eventos novos e o resultado fica NAO_SEI para todas as obras fora das disparadas (`bin/disjuntor.py:1373`, `bin/disjuntor.py:1453`, `bin/disjuntor.py:1518`).

Inventário:

- **Escopo:** escritor `_pdc_grava_escopo`, conteúdo em `bin/disjuntor.py:1467`, instalação atômica em `bin/disjuntor.py:1473`; única chamada em `bin/disjuntor.py:1444`. Leitor `_pdc_escopo_lido` em `bin/disjuntor.py:1480`; consumidores de disparadas/duvida/obras_com_evento em `bin/disjuntor.py:1500`, `bin/disjuntor.py:1502`.
- **Eventos/resolvedor:** escritor novo em `bin/disjuntor.py:1386`; atualização de entidades/res herdados em `bin/disjuntor.py:1389`. Leitores de evento já resolvido em `bin/disjuntor.py:1375`; agrupamento/janela em `bin/disjuntor.py:1288`, `bin/disjuntor.py:1297`; nomeação em `bin/disjuntor.py:1427`; painel em `bin/disjuntor.py:1580`.
- **`visto_em`:** única atribuição em `bin/disjuntor.py:1386`; leitura em `bin/disjuntor.py:1289`; reclassificação preserva o campo porque só atualiza entidades/res (`bin/disjuntor.py:1389`).
- **`_PDC_LIVRE_NESTA`:** criação em `bin/disjuntor.py:1559`; limpeza em `bin/disjuntor.py:2016`; escrita após `_nega_pdc` não negar em `bin/disjuntor.py:1991`, `bin/disjuntor.py:1995`; leitura em `bin/disjuntor.py:1571`. `_nega_pdc` só retorna `None` para LIVRE (`bin/disjuntor.py:1536`, `bin/disjuntor.py:1539`).

As escritas persistentes do PDC ocorrem no callback `_fn` ou no anúncio, ambos sob `_com_estado`: trava exclusiva em `bin/disjuntor.py:260`, callback em `bin/disjuntor.py:275`, gravação em `bin/disjuntor.py:281`, anúncio em `bin/disjuntor.py:293`, fechamento em `bin/disjuntor.py:296`. Não identifiquei segundo escritor desses campos no código pesquisado; uso externo não presente na cópia [não verificado].

No fluxo serial de `main_pretool`, a marca não libera obra julgada em chamada anterior porque a limpeza precede a leitura do payload (`bin/disjuntor.py:2016`, `bin/disjuntor.py:2018`); antes da prova, `_ramo_pdc` nega a obra reconhecida (`bin/disjuntor.py:1571`, `bin/disjuntor.py:1573`). Payload Agent malformado pode escapar da classificação: o `except` de `_ramo_pdc` retorna `None`, permitindo a liberação do envelope (`bin/disjuntor.py:1568`, `bin/disjuntor.py:1569`, `bin/disjuntor.py:1570`, `bin/disjuntor.py:2052`); aceitação desse payload pela ferramenta real [não verificado].

**Relógio fora da borda permanece:** o caminho CLI wrapper → `padrao_parada` → `estado_pdc` não injeta `agora` (`bin/disjuntor.py:2265`, `bin/disjuntor.py:1549`), e `decide_pdc` lê `time.time()` internamente (`bin/disjuntor.py:1353`). O PreToolUse injeta o relógio da entrada corretamente (`bin/disjuntor.py:2022`, `bin/disjuntor.py:2025`, `bin/disjuntor.py:1991`, `bin/disjuntor.py:1536`).

O gate do wrapper está após aquisição do slot e da trava de resposta, antes da chamada Codex (`bin/codex_pergunta.sh:140`, `bin/codex_pergunta.sh:145`, `bin/codex_pergunta.sh:163`, `bin/codex_pergunta.sh:165`, `bin/codex_pergunta.sh:168`).

Os casos novos cobrem estado corrupto com escopo legível e evento removido do RELATO (`bin/tests/test_padrao_de_cima.sh:500`, `bin/tests/test_padrao_de_cima.sh:502`), além de chave `duvida` ausente num escopo ainda legível (`bin/tests/test_padrao_de_cima.sh:505`, `bin/tests/test_padrao_de_cima.sh:507`). O caso sem escopo mantém no RELATO a obra com evento (`bin/tests/test_padrao_de_cima.sh:253`, `bin/tests/test_padrao_de_cima.sh:259`, `bin/tests/test_padrao_de_cima.sh:260`), deixando descoberta a combinação histórica descrita acima.

## 31/10 19:50 · sessao 4639519e8746 · - · - · origem=Ronald (tmux)

**Q4 — há um vacuamente aceito para a regra que o nome anuncia:** `P17-passa-por-excecao` injeta literalmente `o = 1 / 0` e declara esse `ZeroDivisionError`; o harness remove a exceção declarada antes de comparar com o controle. (`bin/tests/test_disjuntor.sh:1762`; `bin/tests/_mordida.sh:154`; `bin/tests/_mordida.sh:159`)

O caso `codex` exige a mensagem orçamentária já na primeira chamada; a exceção artificial impede essa mensagem e provoca `return 1`. Como a chamada tem `CODEX_OBRA=X CODEX_PAPEL=verificador`, o envelope produz **B3-NAO-SEI**, recusando a ferramenta; portanto o mutante pode “morder” sem demonstrar o anunciado “passa por exceção”. A declaração só autoriza a falha que o próprio patch fabricou. (`bin/tests/test_disjuntor.sh:525`; `bin/tests/test_disjuntor.sh:528`; `bin/tests/test_disjuntor.sh:529`; `bin/disjuntor.py:1946`; `bin/disjuntor.py:2034`; `bin/disjuntor.py:2041`)

Os demais examinados possuem ligação estática com a regra:

- `B3-ref-fora-da-pergunta` agora valida apenas `perg.group(1)`, deixando passar a referência ruim anterior à pergunta; o caso 76 contém precisamente `/etc/passwd:1` nessa posição. Evita a antiga quebra por `sv=None` porque continua validando a pergunta antes de acessar `sv[0]`. (`bin/tests/test_disjuntor.sh:1659`; `bin/tests/test_disjuntor.sh:740`; `bin/disjuntor.py:653`; `bin/disjuntor.py:655`)
- `B3-symlink`: a exceção declarada vem do link no componente final, ainda barrado por `O_NOFOLLOW`; a mordida comportamental é o **diretório** simbólico, também presente no caso 74, que passa quando se remove `realpath`. Não depende somente da exceção declarada. (`bin/tests/test_disjuntor.sh:1661`; `bin/tests/test_disjuntor.sh:738`; `bin/tests/test_disjuntor.sh:739`; `bin/orcamento_contexto.py:74`; `bin/disjuntor.py:599`)
- `B3-foto-parcial` e `B3-foto-parcial-engolida` voltam à leitura textual que tolera prefixo parcial; o caso diferencia leitura binária íntegra de leitura textual quebrada e verifica se a linha antiga libera indevidamente. As exceções declaradas são consequências dessa leitura, não um `raise` gratuito introduzido pelo patch. (`bin/tests/test_disjuntor.sh:1663`; `bin/tests/test_disjuntor.sh:1664`; `bin/tests/test_disjuntor.sh:1023`; `bin/tests/test_disjuntor.sh:1031`; `bin/hook_stop_fila1.py:429`; `bin/hook_stop_fila1.py:441`)
- `P5-diff-cala-tudo` consulta a trava e retorna antes do B4; o caso mantém a trava tomada e exige a negativa B4. O `BlockingIOError` declarado é a observação esperada da trava tomada. (`bin/tests/test_disjuntor.sh:1723`; `bin/tests/test_disjuntor.sh:471`; `bin/tests/test_disjuntor.sh:475`; `bin/disjuntor.py:326`; `bin/disjuntor.py:329`)
- `P7-tipo-solto` remove uma guarda de tipo; o caso passa `"rate_limits": "x"` e exige rc 0 e `-`. A exceção declarada corresponde à operação desprotegida. (`bin/tests/test_disjuntor.sh:1731`; `bin/tests/test_disjuntor.sh:461`; `bin/tests/test_disjuntor.sh:462`; `bin/statusline_cota.py:41`)
- `P9-novo-fixo` troca temporário único pelo nome ocupado propositalmente por diretório; o assert exige que a linha ainda seja gravada. O `IsADirectoryError` representa exatamente a colisão que a regra evita. (`bin/tests/test_disjuntor.sh:1735`; `bin/tests/test_disjuntor.sh:650`; `bin/tests/test_disjuntor.sh:653`)
- `P11-fifo-prende` tira o filtro de arquivo regular; a segunda proteção continua evitando bloqueio, mas levanta `OSError` e impede a decisão orçamentária exigida pelo caso. Assim prova a preservação da decisão com FIFO presente, não que remover esse filtro necessariamente prenda o processo. (`bin/tests/test_disjuntor.sh:1740`; `bin/tests/test_disjuntor.sh:577`; `bin/tests/test_disjuntor.sh:580`; `bin/codex_limite_estado.py:146`; `bin/orcamento_contexto.py:74`; `bin/orcamento_contexto.py:77`)
- `b3-sem-DESLIGADO` retira a saída antecipada e força julgamento de JSON sabidamente corrompido; o caso exige sucesso com a chave desligada. As exceções declaradas são o efeito esperado de atravessar indevidamente essa porta. (`bin/tests/test_padrao_de_cima.sh:608`; `bin/tests/test_padrao_de_cima.sh:611`; `bin/tests/test_padrao_de_cima.sh:326`; `bin/tests/test_padrao_de_cima.sh:327`)
- Os reancorados `resolvedor-vira-vazio`, `resolvedor-sem-duvida` e `escopo-fora-da-trava` preservam alvos concretos: evento não gravado, resultado NAO_SEI e gravação sob trava. (`bin/tests/test_padrao_de_cima.sh:631`; `bin/tests/test_padrao_de_cima.sh:636`; `bin/tests/test_padrao_de_cima.sh:637`; `bin/tests/test_padrao_de_cima.sh:361`; `bin/tests/test_padrao_de_cima.sh:364`; `bin/tests/test_padrao_de_cima.sh:408`)

Execução dos mutantes, controles e resultados dos selos: **[não verificado]**.

## 31/10 19:44 · sessao 7a3217b254f8 · GRADE-FOLGA-JUIZ · censo · origem=Code

A cópia contém uma regra compartilhada pela precedência e pela grade: **folga manual pertence à pessoa; folga de calendário só vale quando pertence ao vínculo do dia**. O carregamento preserva todas as candidatas da pessoa, inclusive quando há duas folgas na mesma data; o predicado decide qual vale. Outros leitores usam universos diferentes, descritos abaixo. (`app/ponto/precedencia.py:195`, `app/ponto/precedencia.py:213`, `app/ponto/precedencia.py:223`, `app/escala/utils.py:1119`.)

Este é um censo estático. Resultado de execução dos testes, correspondência Git do HASH e ocorrência dos casos em produção: **[nao verificado]**. As referências a testes abaixo indicam o comportamento escrito neles. [nao verificado]

**1. Precedência, seleção da folga e grade**

| Sítio | Universo, vínculo do dia e calendário de outro vínculo | Chamadores e decisão movida | Testes | [nao verificado]
|---|---|---|---|
| `_folga_avulsa` — `app/ponto/precedencia.py:161`; filtro em `app/ponto/precedencia.py:165` | **Pessoa + data**; retorna `.first()`. Não resolve EC nem lê origem. Portanto encontra também calendário de outro vínculo. | `veto_de_lancamento`, em `app/ponto/precedencia.py:501`: veta folga duplicada e outros lançamentos humanos; férias atravessam a folga. (`app/ponto/precedencia.py:492`, `app/ponto/precedencia.py:502`, `app/ponto/precedencia.py:505`.) | Atestado recusado: `app/ponto/tests/test_a2_precedencia.py:402`; férias permitidas: `app/ponto/tests/test_a2_precedencia.py:725`; duplicidade recusada: `app/ponto/tests/test_a2_precedencia.py:740`. Caso específico de calendário de outro EC no veto: **[nao verificado]**. |
| `folgas_lancadas_do_periodo` — `app/ponto/precedencia.py:195`; filtro em `app/ponto/precedencia.py:207` | **Pessoa + intervalo inclusivo**, qualquer vínculo, ordenado por PK; preserva uma lista por data. Carrega calendário alheio como **candidato**, sem decidir validade. | `fatos_do_periodo`, em `app/ponto/precedencia.py:282`; overlay da grade, em `app/escala/utils.py:1119`. Alimenta precedência e grade. | Duas candidatas, com inválida de PK menor: `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:85`; manual em vínculo fora da janela: `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:91`. |
| `folga_lancada_do_dia` — `app/ponto/precedencia.py:213` | Recebe candidatas e EC; retorna a primeira aceita pelo predicado. Não filtra pessoa/data por conta própria nem escolhe EC. (`app/ponto/precedencia.py:217`.) | `fatos_do_periodo`, em `app/ponto/precedencia.py:358`; overlay, em `app/escala/utils.py:1124`. Escolhe o fato FolgaDia usado no dia. | Calendário alheio, manual antigo, calendário próprio e duas candidatas: `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:70`, `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:75`, `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:80`, `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:85`. |
| `folga_lancada_vale_no_dia` — `app/ponto/precedencia.py:223` | `None` → falso; origem nula/atributo ausente → verdadeiro; calendário → exige `folga.escala_colaborador_id == ec.pk`. Não verifica pessoa/data. **Rejeita calendário de outro EC**, inclusive quando EC é `None`; manual continua aceita. (`app/ponto/precedencia.py:236`, `app/ponto/precedencia.py:238`, `app/ponto/precedencia.py:240`.) | Chamado pelo seletor em `app/ponto/precedencia.py:218`; afeta precedência e grade pelos dois consumidores acima. | Integração: `app/ponto/tests/test_precedencia_folga_de_calendario_e_do_vinculo.py:45`, `app/ponto/tests/test_precedencia_folga_de_calendario_e_do_vinculo.py:50`; cinco casos da grade nas referências acima. Teste unitário direto de `None`/objeto sem origem: **[nao verificado]**. |
| `fatos_do_periodo` — `app/ponto/precedencia.py:260` | **Pessoa/período**. Carrega candidatas da pessoa; EC vem de `vinculo_do_dia`, com células e vínculos da janela, completando geradoras faltantes. Aplica o seletor e registra `fatos.folga_lancada`. **Rejeita calendário alheio como fato lançado**. (`app/ponto/precedencia.py:282`, `app/ponto/precedencia.py:315`, `app/ponto/precedencia.py:319`, `app/ponto/precedencia.py:327`, `app/ponto/precedencia.py:351`, `app/ponto/precedencia.py:358`, `app/ponto/precedencia.py:408`.) | `fatos_do_dia`, em `app/ponto/precedencia.py:257`, e consumidores por período listados adiante. Folga entra na precedência junto com previsão falsa; outro fato pode vencê-la. (`app/ponto/precedencia.py:380`, `app/ponto/precedencia.py:392`.) | Origem/vínculo: `app/ponto/tests/test_precedencia_folga_de_calendario_e_do_vinculo.py:45`, `app/ponto/tests/test_precedencia_folga_de_calendario_e_do_vinculo.py:50`. Geradora versus ordem: `app/ponto/tests/test_precedencia_pela_celula.py:82`. Vínculo encerrado e período/dia: `app/ponto/tests/test_precedencia_vinculo_encerrado.py:74`, `app/ponto/tests/test_precedencia_vinculo_encerrado.py:84`. |
| `fatos_do_dia` — `app/ponto/precedencia.py:243` | É o período de um dia; herda integralmente universo, EC e regra de origem. (`app/ponto/precedencia.py:257`.) | Consumidores relacionados na tabela 5. | Igualdade dia/período: `app/ponto/tests/test_precedencia_vinculo_encerrado.py:84`; casos de calendário/manual: `app/ponto/tests/test_precedencia_folga_de_calendario_e_do_vinculo.py:45`, `app/ponto/tests/test_precedencia_folga_de_calendario_e_do_vinculo.py:50`. |
| `folga_humana_ou_feriado` — `app/ponto/precedencia.py:179` | Lê o resultado já julgado: feriado vencedor, ou faixa folga com FolgaDia lançada/subtipo de ausência. Não escolhe EC nem lê origem novamente. (`app/ponto/precedencia.py:187`.) | Status, em `app/ponto/status_dia.py:139`; calendário, em `app/colaboradores/services/calendario.py:817`. Move natureza/status do dia. | Manual vence dúvida de cadastro: `app/ponto/tests/test_e3_status_indefinida_a2_na_lavra.py:82`; calendário/manual da pessoa: `app/colaboradores/tests/test_calendario_folga_a_mao_e_da_pessoa.py:56`, `app/colaboradores/tests/test_calendario_folga_a_mao_e_da_pessoa.py:66`. |
| `veto_de_lancamento` — `app/ponto/precedencia.py:426` | Para FolgaDia usa `_folga_avulsa`: **pessoa/data, sem validade por origem/EC**. Assim, pode vetar por calendário alheio que o seletor da precedência rejeitaria. Para `falta`, também pergunta a previsão ao EC de `escala_vigente`. (`app/ponto/precedencia.py:501`, `app/ponto/precedencia.py:505`, `app/ponto/precedencia.py:528`.) | Ausência: `app/ponto/services/ausencia.py:380`; declaração: `app/ponto/services/declarar_dia.py:114`; folga: `app/escala/folgas.py:258`, `app/escala/folgas.py:283`; férias: `app/ferias/models.py:428`, `app/ferias/views.py:253`. Move **veto de escrita**. | `app/ponto/tests/test_a2_precedencia.py:392`, `app/ponto/tests/test_a2_precedencia.py:402`, `app/ponto/tests/test_a2_precedencia.py:725`, `app/ponto/tests/test_a2_precedencia.py:733`, `app/ponto/tests/test_a2_precedencia.py:740`. Divergência específica com calendário alheio: **[nao verificado]**. |
| `_aplicar_folgas_foto` — `app/escala/utils.py:1103` | **Pessoa**, carregada pela mesma função da precedência. EC recebido por `esc_do_dia(d)`. Aplica o mesmo seletor: calendário alheio rejeitado; manual antiga aceita. (`app/escala/utils.py:1119`, `app/escala/utils.py:1124`.) | Builder simples em `app/escala/utils.py:770` passa EC fixo; builder por turno em `app/escala/utils.py:1794` passa mapa diário. Marca folga e limpa células/órfãs. (`app/escala/utils.py:1126`.) | Ambos builders: `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:70`, `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:75`, `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:80`, `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:85`, `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:91`; classes/flags em `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:97`, `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:102`. |

O EC usado por esses consumidores segue esta implementação: havendo geradora, procura seu PK e exige que a data esteja dentro de início/fim; geradora ausente da lista ou fora desses limites produz `None`. Sem geradora, usa candidatos que cobrem a data, desempata por ativa/início. (`app/escala/alimentacao.py:237`, `app/escala/alimentacao.py:247`, `app/escala/alimentacao.py:250`, `app/escala/alimentacao.py:253`.)

Na grade por turno, o mapa diário chama essa autoridade; no legacy, a mesma autoridade determina os segmentos, e cada segmento chama o builder simples. (`app/escala/utils.py:1263`, `app/escala/utils.py:1268`, `app/escala/utils.py:973`, `app/escala/utils.py:975`, `app/escala/utils.py:995`, `app/escala/utils.py:999`.)

**2. Outros leitores diretos de FolgaDia e de `calendario_origem`**

“Somente o EC recebido” significa que o sítio não procura linhas dos demais vínculos; a seleção correta desse EC depende do chamador indicado na linha. (`app/ponto/services/declarar_dia.py:70`, `app/escala/models.py:1281`, `app/escala/folgas.py:254`.)

| Sítio | Universo e critério de EC/origem | Chamadores e decisão movida | Testes | [nao verificado]
|---|---|---|---|
| `_folga_do_dia` — `app/ponto/services/declarar_dia.py:61` | **EC recebido + data**; `.first()`. Não encontra manual nem calendário de outro EC. (`app/ponto/services/declarar_dia.py:70`.) | `o_dia_ja_e_folga`, em `app/ponto/services/declarar_dia.py:91`; `_recusa_do_dia`, em `app/ponto/services/declarar_dia.py:108`. Existência/idempotência e veto. | Manual: `app/ponto/tests/test_declarar_dia.py:79`; calendário: `app/ponto/tests/test_declarar_dia.py:87`. Outro EC: **[nao verificado]**. |
| `o_dia_ja_e_folga` / `_recusa_do_dia` — `app/ponto/services/declarar_dia.py:73`, `app/ponto/services/declarar_dia.py:94` | EC de `_ec_do_dia` → `escala_vigente` → `vinculo_do_dia`. Origem não nula da linha encontrada veta declaração manual; manual acusa duplicidade. (`app/ponto/services/declarar_dia.py:56`, `app/ponto/services/declarar_dia.py:90`, `app/ponto/services/declarar_dia.py:105`, `app/ponto/services/declarar_dia.py:109`, `app/escala/servico_jornada.py:57`.) | Aceite de folga: `app/chamados/services/validacao.py:261`, `app/chamados/services/validacao.py:316`; preview/escrita: `app/ponto/services/declarar_dia.py:141`, `app/ponto/services/declarar_dia.py:154`. Move aceite idempotente, veto e declaração. | `app/ponto/tests/test_declarar_dia.py:79`, `app/ponto/tests/test_declarar_dia.py:87`, `app/ponto/tests/test_declarar_dia.py:196`, `app/ponto/tests/test_declarar_dia.py:208`; aceite de quatro perguntas para um fato: `app/chamados/tests/test_o199_aceite_de_folga_decide_o_dia.py:167`. |
| `folgas_do_periodo` — `app/escala/alimentacao.py:49`; query em `app/escala/alimentacao.py:66` | **Lista de ECs recebida**, datas arredondadas para meses inteiros; saída por EC. Não lê origem nem escolhe EC do dia. Calendário de outro EC permanece no balde daquele EC. (`app/escala/alimentacao.py:60`, `app/escala/alimentacao.py:69`.) | Alimentações listadas na tabela 4; precedência em `app/ponto/precedencia.py:333`. Deriva previsão, não a regra pessoa/manual. | Mês inteiro: `app/escala/tests/test_selo_alimentacao_dias.py:96`; equivalência/custo alimentado: `app/escala/tests/test_selo_alimentacao_dias.py:73`. |
| `defeito_folga_nao_definida` — `app/escala/models.py:1212`; query em `app/escala/models.py:1220` | **Próprio EC + mês**. No 6x1 sem folga semanal, qualquer FD do próprio EC elimina o defeito mensal. Não distingue origem; outro EC não serve. | Detector: `app/ponto/management/commands/detectar_ausencias.py:144`, `app/ponto/management/commands/detectar_ausencias.py:187`; supra: `app/ponto/supra_juiz.py:200`; limbo: `app/chamados/services/limbo_folgas.py:199`, `app/chamados/services/limbo_folgas.py:281`; plano: `app/escala/views_plano_folgas.py:89`; faltas hoje: `app/ponto/services/faltas_de_hoje.py:39`; régua: `app/escala/regua_defesa.py:145`, `app/escala/regua_defesa.py:176`; diagnóstico: `app/ponto/services/diagnostico_escala.py:104`. Move **defeito/cobrança de cadastro**. | `app/escala/tests/test_t1_ligado_por_padrao.py:34`, `app/escala/tests/test_t1_ligado_por_padrao.py:39`, `app/escala/tests/test_t1_ligado_por_padrao.py:45`, `app/escala/tests/test_t1_ligado_por_padrao.py:55`; ligação ao EC do juiz: `app/ponto/tests/test_e6_janela_declarada_nao_e_divergencia.py:177`. |
| `eh_dia_trabalho_calculado` — `app/escala/models.py:1223` | **Próprio EC**: consulta foto mensal em `app/escala/models.py:1257`, data exata em `app/escala/models.py:1281`, anterior em `app/escala/models.py:1327` e posterior em `app/escala/models.py:1330`; também aceita mapa por EC. **Nenhuma leitura distingue origem**. Foto do próprio EC no dia gera folga; fora dela usa ciclo ou paridade da foto próxima. Outro EC não entra. (`app/escala/models.py:1251`, `app/escala/models.py:1282`, `app/escala/models.py:1290`, `app/escala/models.py:1335`.) | `eh_dia_trabalho`, em `app/escala/models.py:1073`, e chamadas diretas da tabela 4. Move geração de célula/previsão. | Foto vence âncora: `app/escala/tests/test_foto_vence_ancora.py:38`, `app/escala/tests/test_foto_vence_ancora.py:44`; preserva ciclo: `app/escala/tests/test_foto_nao_apaga_o_ciclo.py:50`, `app/escala/tests/test_foto_nao_apaga_o_ciclo.py:60`; foto parcial/paridade: `app/escala/tests/test_o37_foto_parcial.py:69`, `app/escala/tests/test_o37_foto_parcial.py:87`, `app/escala/tests/test_o37_foto_parcial.py:106`, `app/escala/tests/test_o37_foto_parcial.py:114`. |
| `medir` / `_foto_diz` — `app/escala/services/fase_conflitante.py:61`, `app/escala/services/fase_conflitante.py:51` | **Cada EC ativo**, todas as suas datas de FD, sem recorte na query; não escolhe EC diário nem lê origem. `_foto_diz` reconhece presença e extrapola paridade da foto próxima. (`app/escala/services/fase_conflitante.py:71`, `app/escala/services/fase_conflitante.py:74`, `app/escala/services/fase_conflitante.py:55`.) | Command em `app/escala/management/commands/lavrar_fase_conflitante.py:31`; snapshot em `app/escala/management/commands/lavrar_fase_conflitante.py:35`; cron em `app/config/crons.py:479`. Move contador de conflito foto/âncora. | `app/escala/tests/test_fase_conflitante.py:60`, `app/escala/tests/test_fase_conflitante.py:72`, `app/escala/tests/test_fase_conflitante.py:80`, `app/escala/tests/test_fase_conflitante.py:84`, `app/escala/tests/test_fase_conflitante.py:88`, `app/escala/tests/test_fase_conflitante.py:93`. |
| `divergencia_foto_x_calendario` — `app/escala/folgas.py:83` | **EC recebido + mês**; separa manual/calendário pela origem. Compara a foto com calendário atual do EC, sem exigir igualdade entre origem de cada FD e calendário atual. Outro EC não entra. (`app/escala/folgas.py:99`, `app/escala/folgas.py:101`, `app/escala/folgas.py:104`.) | Agregador em `app/escala/folgas.py:143`; command em `app/escala/management/commands/lavrar_foto_folgas.py:113`, `app/escala/management/commands/lavrar_foto_folgas.py:167`. `sobra` impede reconciliação do mês. (`app/escala/management/commands/lavrar_foto_folgas.py:168`.) | `app/escala/tests/test_foto_folga_continua.py:136`, `app/escala/tests/test_foto_folga_continua.py:158`, `app/escala/tests/test_foto_folga_continua.py:179`. |
| `gerar_foto_folgas` — `app/escala/folgas.py:150` | **EC recebido + mês**; lê manuais para preservá-las e seleciona calendário para reconciliar. Não procura outro EC. (`app/escala/folgas.py:165`, `app/escala/folgas.py:167`, `app/escala/folgas.py:169`, `app/escala/folgas.py:171`.) | Views em `app/escala/views.py:414`, `app/colaboradores/views.py:245`; serviço em `app/colaboradores/services/vinculo.py:163`; command em `app/escala/management/commands/lavrar_foto_folgas.py:170`. Materializa foto e chama relavra em `app/escala/folgas.py:192`. | `app/escala/tests/test_gerador_folgas.py:39`, `app/escala/tests/test_gerador_folgas.py:47`, `app/escala/tests/test_gerador_folgas.py:54`, `app/escala/tests/test_gerador_folgas.py:63`; preservação manual: `app/escala/tests/test_foto_folga_continua.py:179`; relavra: `app/escala/tests/test_torneira_folgas.py:87`. |
| `lancar_folga` — `app/escala/folgas.py:228` | **EC recebido + dia**; linha própria com calendário veta escrita manual. Depois consulta o veto por **pessoa**, que pode encontrar FD de outro EC. (`app/escala/folgas.py:254`, `app/escala/folgas.py:255`, `app/escala/folgas.py:258`.) | Plano em `app/escala/views_plano_folgas.py:131`; declaração em `app/ponto/services/declarar_dia.py:183`. Cria/obtém foto e relavra. (`app/escala/folgas.py:259`, `app/escala/folgas.py:261`.) | `app/escala/tests/test_torneira_folgas.py:68`; calendário/veto da declaração: `app/ponto/tests/test_declarar_dia.py:87`. |
| `_foto_do_dia` — `app/escala/folgas.py:267` | **EC recebido + dia**; origem não nula veta alteração/remoção. Não lê outro EC. (`app/escala/folgas.py:274`, `app/escala/folgas.py:275`.) | `alterar_folga`, em `app/escala/folgas.py:302`; lote, em `app/escala/folgas.py:334`; views em `app/escala/views_plano_folgas.py:136`, `app/escala/views_plano_folgas.py:185`. Move veto, alteração/remoção e relavra. | `app/escala/tests/test_bug136_plano_em_lote_relavra.py:68`, `app/escala/tests/test_bug136_plano_em_lote_relavra.py:75`, `app/escala/tests/test_bug136_plano_em_lote_relavra.py:93`, `app/escala/tests/test_bug136_plano_em_lote_relavra.py:114`, `app/escala/tests/test_bug136_plano_em_lote_relavra.py:124`. |
| `reparentar_folga` — `app/escala/folgas.py:392` | FD individual: calendário retorna sem migrar; manual fica se EC atual cobre a data; caso contrário, alvo vem de `vinculo_do_dia_carregado`. FD de qualquer origem já existente no alvo impede migração. (`app/escala/folgas.py:405`, `app/escala/folgas.py:408`, `app/escala/folgas.py:414`, `app/escala/folgas.py:418`.) | Coletor em `app/escala/folgas.py:459`; command em `app/escala/management/commands/reparentar_folgas.py:34`. Move titularidade; relavra da competência corrente em diante. (`app/escala/folgas.py:435`, `app/escala/folgas.py:441`.) | `app/escala/tests/test_folga_segue_a_pessoa.py:53`, `app/escala/tests/test_folga_segue_a_pessoa.py:66`, `app/escala/tests/test_folga_segue_a_pessoa.py:80`, `app/escala/tests/test_folga_segue_a_pessoa.py:92`, `app/escala/tests/test_folga_segue_a_pessoa.py:104`, `app/escala/tests/test_folga_segue_a_pessoa.py:113`. |
| `reparentar_folgas_do_colaborador` — `app/escala/folgas.py:447` | **Pessoa inteira, somente manuais**; seleciona vínculos que não cobrem a data e delega destino à porta acima. (`app/escala/folgas.py:453`, `app/escala/folgas.py:457`, `app/escala/folgas.py:459`.) | Signal em `app/escala/signals.py:190`; move a mesma migração/relavra. | Troca de escala leva folga ao novo vínculo: `app/escala/tests/test_folga_segue_a_pessoa.py:53`. |
| Command `reparentar_folgas.handle` — `app/escala/management/commands/reparentar_folgas.py:22` | **Todas as manuais**; ignora as cujo EC cobre a data e delega à porta individual. (`app/escala/management/commands/reparentar_folgas.py:28`, `app/escala/management/commands/reparentar_folgas.py:31`, `app/escala/management/commands/reparentar_folgas.py:34`.) | Entrada management command; `apply` controla aplicação. (`app/escala/management/commands/reparentar_folgas.py:26`.) | Porta individual: testes acima. Teste específico do command: **[nao verificado]**. |
| `_grid_ctx` — `app/escala/views_plano_folgas.py:51` | **ECs ativos da empresa/posto + mês**. Mapa por EC/data; não escolhe vínculo diário nem distingue origem. FD de outro EC não pinta a linha deste EC. FD presente precede a previsão. (`app/escala/views_plano_folgas.py:55`, `app/escala/views_plano_folgas.py:60`, `app/escala/views_plano_folgas.py:63`, `app/escala/views_plano_folgas.py:70`, `app/escala/views_plano_folgas.py:76`.) | View em `app/escala/views_plano_folgas.py:46`; template usa tipo pronto em `app/templates/escala/plano_folgas.html:81`. Move grade de cadastro. | `app/escala/tests/test_plano_folgas_filtro.py:53`, `app/escala/tests/test_plano_folgas_filtro.py:58`, `

[truncado em 20000 de 47238 caracteres]

## 31/10 19:42 · sessao 453b70466694 · V3-P5-PRORROGACAO-POR-INTERVALO · censo · origem=Code

**O booleano é o mesmo, mas a elegibilidade da prorrogação é recalculada por intervalo.** A função recebe `entrada/saida`, recorta cada pausa e chama `minutos_noturnos` com as pontas desse pedaço; esta só prorroga quando a própria chamada começa antes das 05:00 e termina depois. Assim, a pausa 05:30–06:30 não herda a prorrogação do turno 22:00–07:00. (`app/ponto/motor_calculo_v2.py:109-111`, `app/ponto/motor_calculo_v2.py:141-145`.)

**1. Chamadores da função alvo e destino do resultado**

O censo estático encontrou **dois sítios de chamada direta**. As ocorrências do nome em testes são texto explicativo e mensagem de asserção; não são chamadas. (`app/ponto/motor_calculo_v2.py:392`, `app/ponto/motor_calculo_v2.py:1865`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:18`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:54`.) Chamadas construídas por reflexão sem referência estática identificável: [nao verificado].

| Chamador direto | Valor de `prorrogacao` | Origem de `entrada/saida` | Destino imediato |
|---|---|---|---|
| `he_noturna` | Seu argumento `prorrogacao`, cujo padrão é `True`. | Passa `inicio_extra`, calculado por `_inicio_do_trecho_extra`, e `fim=saida`: é a cauda de HE, não necessariamente o turno inteiro. | Subtrai de `minutos_extra_not`; reparte proporcionalmente entre `not_50/not_100` e retorna a dupla. (`app/ponto/motor_calculo_v2.py:359`, `app/ponto/motor_calculo_v2.py:385-401`.) |
| `MotorBase._noturno_dos_intervalos` | `self.prorrogacao_noturna_pos5h`. | Repassa seus parâmetros `entrada/saida/intervalos` sem alterar. | Retorna o desconto aos leitores abaixo. (`app/ponto/motor_calculo_v2.py:1852`, `app/ponto/motor_calculo_v2.py:1865-1866`.) |

Os caminhos que chegam a esses dois sítios são:

| Caminho | Pontas, flag e destino |
|---|---|
| **Motor → adicional noturno** | `calcular_noturnas` calcula o bruto e desconta `_noturno_dos_intervalos` com o mesmo par e o mesmo flag; aplica o fator e retorna relógio/legais. `_escrever_noturnas` grava `minutos_noturnos_relogio/legais`. (`app/ponto/motor_calculo_v2.py:921-925`, `app/ponto/motor_calculo_v2.py:941-943`.) |
| **12x36 → `_escrever_noturnas`** | `e/s` vêm de `_janela_do_par(localizar(entrada), localizar(saida))`; os intervalos passam por `_intervalos_na_janela`. (`app/ponto/motor_calculo_v2.py:2697-2702`, `app/ponto/motor_calculo_v2.py:2730`.) |
| **Turno partido → `_escrever_noturnas`** | Mesma preparação de pontas efetivas e intervalos recortados. (`app/ponto/motor_calculo_v2.py:2824-2834`.) |
| **Comercial → `_escrever_noturnas`** | Mesma preparação de pontas efetivas e intervalos recortados. (`app/ponto/motor_calculo_v2.py:3149-3150`, `app/ponto/motor_calculo_v2.py:3162`, `app/ponto/motor_calculo_v2.py:3185`.) |
| **Motor → HE noturna** | `_aplicar_he_noturna` passa `periodo.entrada/saida`, extras 50/100, flag do motor e `intervalos_do_motor`; grava `minutos_extra_100_noturna` e `minutos_extra_50_noturna`. Aqui as pontas são as originais localizadas, diferentemente das pontas efetivas do adicional acima. (`app/ponto/motor_calculo_v2.py:952-960`, `app/ponto/motor_calculo_v2.py:2689`, `app/ponto/motor_calculo_v2.py:2816`, `app/ponto/motor_calculo_v2.py:3141`.) |
| **Calculador → adicional, por método emprestado** | `_ContextoNoturno` guarda `bool(prorrogacao_pos5h)` e atribui `MotorBase._noturno_dos_intervalos` como método da classe. `noturnas_do_dia` calcula cada par e desconta por esse método. (`app/ponto/calculador/regras.py:60-63`, `app/ponto/calculador/regras.py:80-89`.) |
| **Calculador → HE, por apelido de import** | Importa `he_noturna as _he_not`; chama por ponta de período, com `prorrogacao_pos5h` e os intervalos daquela ponta; soma e grava `horas_extras_50_noturna/horas_extras_100_noturna`. (`app/ponto/calculador/regras.py:868`, `app/ponto/calculador/regras.py:880-898`.) |
| **Releitura v3 → adicional por par** | `noturno_no_par` passa um par inteiro, intervalos e `cct.prorrogacao_pos5h` a `noturnas_do_dia`; `reler` grava os minutos no período. Esse caminho se declara de ensaio, não de produção. (`app/ponto/calculador/periodo.py:508-511`, `app/ponto/calculador/periodo.py:544`, `app/ponto/calculador/periodo.py:752-754`.) |

As cinco chamadas de `_aplicar_he_noturna` estão no 12x36 por período, no partido após HE diária e semanal, e no comercial por período e após HE diária mensal. (`app/ponto/motor_calculo_v2.py:2793`, `app/ponto/motor_calculo_v2.py:3085`, `app/ponto/motor_calculo_v2.py:3116`, `app/ponto/motor_calculo_v2.py:3219`, `app/ponto/motor_calculo_v2.py:3337`.)

Plantão reutiliza o cálculo do 12x36; `Motor12x36ComEscala` chama `super`; partido noturno e intermitente herdam os caminhos correspondentes. (`app/ponto/motor_calculo_v2.py:3448-3461`, `app/ponto/motor_calculo_v2.py:3474-3495`.)

Na alimentação de produção do calculador, as pontas vêm de `efetivo_entrada/saida`, e as pausas de `intervalos_do_motor`; são montados `he_pontas`, pares e intervalos por dia. O flag vem da CCT daquele dia e segue para `regras.do_dia`. (`app/ponto/calculador/alimentacao.py:106`, `app/ponto/calculador/alimentacao.py:127-135`, `app/ponto/calculador/alimentacao.py:185-190`, `app/ponto/calculador/alimentacao.py:572-596`.)

Os leitores seguintes dos minutos são as propriedades do período/mês, a lavratura diária e o fechamento mensal: `horas_noturnas` soma períodos comuns **e folgas**; `horas_extra_100_noturna` soma períodos comuns. (`app/ponto/motor_calculo_v2.py:210-211`, `app/ponto/motor_calculo_v2.py:271`, `app/ponto/motor_calculo_v2.py:292-294`, `app/ponto/services/dia_pago.py:179-184`, `app/ponto/services/dia_pago.py:224`, `app/ponto/services/fechamento.py:893-899`.)

Nos testes, as chamadas explícitas a `he_noturna` usam a grade de pontas com `True/False`, sem pausas, ou `True` com pausas anteriores às 05:00; o P5 alcança a função por `regras.do_dia`, com pausa pós05. (`app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:90-123`, `app/ponto/tests/test_noturno_desconta_intervalo.py:249-267`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:40-47`.)

**2. Mesmo valor do turno? Alguém já passa a janela do turno?**

- **Via `calcular_noturnas`: sim, o flag é igual** no bruto do par e no desconto das pausas. Também são passadas as pontas do par inteiro ao desconto. Entretanto, a função alvo usa essas pontas apenas para recortar cada pausa; não transporta a janela noturna já reconhecida para o par. (`app/ponto/motor_calculo_v2.py:921-923`, `app/ponto/motor_calculo_v2.py:141-145`, `app/ponto/motor_calculo_v2.py:1865-1866`.)
- **Via `he_noturna`: o flag também é igual**, mas as pontas passadas ao desconto são as da cauda extra. Tanto a elegibilidade do bruto da HE quanto a das pausas é avaliada sobre suas respectivas chamadas. (`app/ponto/motor_calculo_v2.py:390-393`.)
- **Via calculador: o mesmo flag da CCT do dia** alimenta bruto, desconto e split de HE. Isso não comprova igualdade com o flag de um motor montado para outra data/fatia. (`app/ponto/calculador/alimentacao.py:448`, `app/ponto/calculador/alimentacao.py:572-596`, `app/ponto/calculador/regras.py:80-86`, `app/ponto/calculador/regras.py:886-888`.)
- Portanto, **há caminhos que já passam o envelope inteiro do par**, mas **nenhum dos dois chamadores diretos passa uma janela noturna elegível previamente decidida pelo turno**; a assinatura só recebe pontas, intervalos e flag. (`app/ponto/calculador/periodo.py:508-510`, `app/ponto/motor_calculo_v2.py:121`, `app/ponto/motor_calculo_v2.py:392-393`, `app/ponto/motor_calculo_v2.py:1865-1866`.)

O flag do motor é recebido explicitamente ou resolvido pelo ciclo quando chega `None`; a fábrica CCT ativa pode substituí-lo pelo valor da régua. Valores cadastrados efetivos em produção: [nao verificado]. (`app/ponto/motor_calculo_v2.py:708-712`, `app/core/regua_cct.py:182`, `app/core/regua_cct.py:458-466`.)

**3. Outras funções: turno ou pedaço**

| Função | Como decide a prorrogação |
|---|---|
| `minutos_noturnos` | Decide pela geometria **da própria chamada**: início antes das 05:00 e fim depois. Não recebe contexto de turno externo. (`app/ponto/motor_calculo_v2.py:82`, `app/ponto/motor_calculo_v2.py:109-111`.) |
| `minutos_noturnos_dos_intervalos` | Mede **cada pausa recortada separadamente**, repassando o flag. (`app/ponto/motor_calculo_v2.py:141-145`.) |
| `MotorBase._noturno_dos_intervalos` | Só delega; portanto o desconto continua decidido por pedaço. (`app/ponto/motor_calculo_v2.py:1865-1866`.) |
| `MotorBase.calcular_noturnas` | Bruto pelo par recebido; desconto por pausa isolada. Usa o mesmo flag em ambos. (`app/ponto/motor_calculo_v2.py:921-925`.) |
| `he_noturna` | Bruto pela cauda extra, cujo início recua por minutos trabalhados; desconto por pausa recortada nessa cauda. (`app/ponto/motor_calculo_v2.py:328-356`, `app/ponto/motor_calculo_v2.py:390-393`.) |
| `regras._ContextoNoturno` | Não deriva uma janela: guarda o booleano e empresta o método do motor. (`app/ponto/calculador/regras.py:60-63`.) |
| `regras.noturnas_do_dia` | Bruto por par; desconto por pausa, via contexto emprestado; aplica fator após a soma. (`app/ponto/calculador/regras.py:80-89`.) |
| `periodo.noturno_no_par` | Encaminha um único par inteiro e suas pausas à porta acima; conserva a mesma assimetria entre bruto do par e desconto por pedaço. (`app/ponto/calculador/periodo.py:508-511`.) |

**4. Escritores e leitores de `horas_noturnas` e `horas_extras_100_noturna`**

Os dois campos existem em `DiaPago` e `FechamentoMensal`; o resultado do motor usa o nome singular `horas_extra_100_noturna`, convertido para o plural no fechamento. (`app/ponto/models.py:327`, `app/ponto/models.py:332`, `app/ponto/models.py:413`, `app/ponto/models.py:419`, `app/ponto/services/fechamento.py:899`.)

| Escritor/derivador | Campos e operação |
|---|---|
| Motor | `_escrever_noturnas` grava minutos noturnos relógio/legais; `_aplicar_he_noturna` grava minutos de HE100 noturna. As propriedades convertem e somam em horas. (`app/ponto/motor_calculo_v2.py:941-943`, `app/ponto/motor_calculo_v2.py:958`, `app/ponto/motor_calculo_v2.py:271`, `app/ponto/motor_calculo_v2.py:294`.) |
| Fechamento mensal | Atribui os dois campos a partir de `resultado`; persiste por `save` ou `update`. (`app/ponto/services/fechamento.py:893`, `app/ponto/services/fechamento.py:899`, `app/ponto/services/fechamento.py:1112`, `app/ponto/services/fechamento.py:1131`, `app/ponto/services/fechamento.py:1137`.) |
| Calculador v3 | Escreve noturnas nos ramos folga/comum e HE100 noturna no split. (`app/ponto/calculador/regras.py:739`, `app/ponto/calculador/regras.py:898`, `app/ponto/calculador/regras.py:912`.) |
| `dia_pago.lavrar` | Soma ambos dos períodos; acrescenta noturnas das folgas; substitui campos pelos `valores` do calculador quando cabível; persiste por `bulk_create`. (`app/ponto/services/dia_pago.py:179-184`, `app/ponto/services/dia_pago.py:224`, `app/ponto/services/dia_pago.py:320-328`, `app/ponto/services/dia_pago.py:343-358`.) |
| Integração do fechamento | Lavra a versão `motor`; calcula as rubricas v3 e lavra separadamente a versão `oraculo`. (`app/ponto/services/fechamento.py:1169-1195`, `app/ponto/services/fechamento.py:1239-1262`.) |
| Releitura v3 de ensaio | Reescreve minutos noturnos no período, posteriormente lidos pelas propriedades do resultado. (`app/ponto/calculador/periodo.py:544`, `app/ponto/calculador/periodo.py:752-754`, `app/ponto/motor_calculo_v2.py:271`.) |
| `restaurar_fechamento` | Escritor genérico: aceita campos existentes e os persiste por `**_upd`, podendo atingir ambos. (`app/ponto/services/fechamento.py:1342-1361`.) |
| `retratar_exportada` | Pode criar linhas diárias e ajuste com ambos zerados por compreensão sobre `CAMPOS`. (`app/ponto/services/dia_pago.py:470-477`, `app/ponto/services/dia_pago.py:508-515`, `app/ponto/services/dia_pago.py:543`.) |
| Restauros em `bin` | Reescrevem campos mensais por `setattr/save` e recriam `DiaPago`: `bin/restore_relavratura_10_2026.py:79-86`, `bin/restore_o263_comp10.py:48-56`. O restauro de badge recria os campos diários presentes no snapshot: `bin/restore_badge_linha_09_10.py:27`, `bin/restore_badge_linha_09_10.py:38-41`, `bin/restore_badge_linha_09_10.py:63`. |

Os leitores funcionais e de apresentação são:

| Leitor | Uso |
|---|---|
| Agregações de `DiaPago` | Ambos por listas de campos: soma da competência e soma do período. (`app/ponto/services/dia_pago.py:34-36`, `app/ponto/services/dia_pago.py:560-561`, `app/ponto/services/dia_pago.py:782-787`.) |
| `pago_do_dia` | Lê ambos para detectar existência de dinheiro lavrado. (`app/ponto/services/dia_pago.py:859-862`, `app/ponto/services/dia_pago.py:890`.) |
| `rubricas_do_dia` | Lê `horas_noturnas` para tela/papel; consumido por espelho, calendário e PDF. (`app/ponto/services/dia_pago.py:922`, `app/ponto/services/espelho.py:824`, `app/colaboradores/services/calendario.py:785`, `app/relatorios/pdf_espelho.py:840`, `app/relatorios/pdf_espelho.py:906`.) |
| `folha/porta_export` | Compara ambos genericamente, lendo cada campo da soma e do fechamento. (`app/folha/porta_export.py:451-458`.) |
| **`folha/export`** | Lê `horas_noturnas` → evento `adicional_noturno`. Não emite HE100 noturna como linha adicional no retorno de eventos. (`app/folha/export.py:413-450`.) |
| TXT e homologação | Consomem `eventos_do_fechamento`. O código da rubrica sai do cadastro, por `_rub_map`/`codigo_fopag`; “25” aparece no comentário, não como código fixo dessa emissão. Cadastro real: [nao verificado]. (`app/folha/export.py:419`, `app/folha/export.py:694`, `app/folha/export.py:711-719`, `app/folha/export.py:976`.) |
| Simulação de folha | Lê ambos por `CAMPOS_HORAS/getattr`. (`app/folha/management/commands/simular_folha.py:59-60`, `app/folha/management/commands/simular_folha.py:84`.) |
| Ranking | Lê efetivamente `horas_noturnas`. (`app/folha/services/ranking_he.py:225`.) |
| Relatórios e espelho | Leem noturnas para CSV, totais, extrato e cartão. (`app/relatorios/views.py:94`, `app/relatorios/views.py:108`, `app/relatorios/views.py:524`, `app/relatorios/cartao_pela_celula.py:176`, `app/ponto/services/espelho.py:99`.) |
| Templates | Leem noturnas. (`app/templates/relatorios/horas_mes.html:68`, `app/templates/ponto/_fech_linha.html:31`, `app/templates/ponto/partials/painel_fechamento.html:33`.) |

Também há acessos de manutenção, comparação e snapshot — inclusive dinâmicos, que uma busca apenas por atribuição literal perderia:

- `aplicar_09_he_intra` fotografa ambos e pode alterar HE100 noturna pela porta de restauração; `aplicar_09_corte_b` fotografa ambos, recalcula e restaura campos; `desvio_o68b` fotografa e restaura campos selecionados. (`app/ponto/management/commands/aplicar_09_he_intra.py:39-45`, `app/ponto/management/commands/aplicar_09_he_intra.py:128-152`, `app/ponto/management/commands/aplicar_09_corte_b.py:184`, `app/ponto/management/commands/aplicar_09_corte_b.py:212`, `app/ponto/management/commands/aplicar_09_corte_b.py:265-268`, `app/ponto/management/commands/desvio_o68b.py:149`, `app/ponto/management/commands/desvio_o68b.py:195-202`.)
- Hash/snapshot de ambos por introspecção ou lista canônica: `carimbo_gravado`, `lavrar_dias_pagos`, `recalcular_fechamento`, `regen_exportada` e a serialização do modo leitura do fechamento. (`app/ponto/management/commands/carimbo_gravado.py:36`, `app/ponto/management/commands/carimbo_gravado.py:54-55`, `app/ponto/management/commands/lavrar_dias_pagos.py:70-77`, `app/ponto/management/commands/recalcular_fechamento.py:54-67`, `app/ponto/portas/regen_exportada.py:72-84`, `app/ponto/services/fechamento.py:1102-1104`.)
- `diff_frota` lê os campos recebidos; seus chamadores passam a lista que contém ambos. `diff_calculador` também monta o valor diário de noturnas do motor. (`app/ponto/services/diff_frota.py:63`, `app/ponto/management/commands/diff_col900.py:45`, `app/ponto/management/commands/diff_passivo_s84.py:50`, `app/ponto/management/commands/diff_janela_he_total.py:101-115`, `app/ponto/management/commands/diff_calculador.py:260`.)
- Outros leitores de noturnas: `celula_veredito_velho`, `diff_reclassificar_partido` e a lista de rubricas de autorização de HE. (`app/ponto/management/commands/celula_veredito_velho.py:70-85`, `app/ponto/management/commands/celula_veredito_velho.py:155`, `app/ponto/management/commands/diff_reclassificar_partido.py:67`, `app/ponto/management/commands/diff_reclassificar_partido.py:146-156`, `app/ponto/services/autorizacao_he_periodo.py:48-50`.)
- Scripts de fotografia/diff leem ambos: `bin/snapshot_relavratura_10_2026.py:31-37`, `bin/snapshot_o263_comp10.py:57-63`, `bin/snapshot_badge_linha_09_10.py:50-60`, `bin/diff_o118_col221.py:23-27`, `bin/diff_o119_frota.py:27-46`, `bin/diff_o124_frota.py:31`, `bin/aplicar_o263_comp10.py:44-46`.
- Os scripts de relavratura também fotografam ambos genericamente antes de chamar o recálculo: `bin/relavrar_o118_col221.py:26-40`, `bin/relavrar_o119_col221.py:25-39`, `bin/relavrar_o120_col221_09.py:29-41`, `bin/relavrar_o120_col221_09.py:62-69`.

Fixtures e asserções dos campos aparecem, entre outros, nos testes de exportação, soma diária e v3; são usos de teste, não novos derivadores de produção. (`app/folha/tests/test_export_invariante_he.py:27`, `app/folha/tests/test_export_59a.py:14`, `app/folha/tests/test_export_dsr_reflexo.py:25`, `app/ponto/tests/test_dia_pago_soma.py:231`, `app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:69-74`, `app/ponto/tests/test_v3_periodos_da_celula.py:116-176`.)

**5. Testes do intervalo pós05 com prorrogação** [nao verificado]

**Existe o caso exato, mas ele exige a correção: não fixa 9 horas defeituosas como resultado esperado.** O módulo declara ser vermelho de propósito e sem `expectedFailure`; monta turno 22:00–07:00, pausa 05:30–06:30, flag `True` e fator 1. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:4-6`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:40-47`.)

| Teste | O que exige | [nao verificado]
|---|---|
| `test_P5_HE100_noturna_pela_regra` | HE100 = 8 h e HE100 noturna = 8 h. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:49-54`.) |
| `test_P5_PROPRIEDADE_parte_nao_passa_do_todo` | HE100 noturna ≤ HE100. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:56-59`.) |
| `test_P5_noturnas_pela_regra` | `horas_noturnas` = 8 h. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:61-63`.) |

O docstring registra 9 h nas duas parcelas como comportamento defeituoso atual; essa descrição é coerente com a avaliação por pedaço presente no código. Resultado de execução dos testes nesta auditoria: [nao verificado]. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:17-24`, `app/ponto/motor_calculo_v2.py:109-111`, `app/ponto/motor_calculo_v2.py:145`.)

Há guardas relacionadas, com geometria diferente:

- Pausa **04:30–05:30**, atravessando as 05:00: C3 exige 8 h e verifica que a porta entrega par inteiro e pausas. (`app/ponto/tests/test_v3_periodos_da_celula.py:123-148`.)
- F7f fixa dois pares e 390 minutos; F7g, marcado `expectedFailure`, pede um par e 480 minutos. Outra guarda compara explicitamente par inteiro = 480 versus segmentos = 390. (`app/ponto/tests/test_o280_bateria_da_lei.py:1562-1616`, `app/ponto/tests/test_o280_bateria_da_lei.py:4132-4171`.)
- As guardas de desconto da pausa 00:00–01:00 e de prorrogação do turno sem pausa são testes separados; não cobrem a pausa inteiramente pós05. (`app/ponto/tests/test_s5b_regra_noturnas.py:75-93`.)
- As guardas de HE descontam pausas 23:30–00:30 e 23:00–23:30; também não cobrem a pausa inteiramente pós05. (`app/ponto/tests/test_noturno_desconta_intervalo.py:245-269`.)

Quantidade de colaboradores atingidos, valores persistidos, impacto financeiro e estado efetivo de produção: **[nao verificado]**.

## 31/10 19:41 · sessao 9af78461416a · - · - · origem=Ronald (tmux)

Censo complementar de grade e consumidores indiretos, somente pelo código:

| Sítio | Universo / EC do dia / calendário de outro vínculo | Chamador e decisão |
|---|---|---|
| `app/escala/utils.py::_aplicar_folgas_foto:1103` | Carrega FolgaDia de toda a pessoa por `folgas_lancadas_do_periodo` em `:1119`; julga por `folga_lancada_do_dia(..., esc_do_dia(d))` em `:1124`. Portanto calendário de outro EC é recusado; manual de outro EC é aceito pela autoridade delegada. | `montar_grade_prevista:770` passa EC fixo recebido; `montar_grade_prevista_periodo_por_turno:1794` passa mapa do vínculo do dia. Marca `tipo_dia='folga'`, apaga células e órfãs em `:1126-1128`. |
| `app/escala/utils.py::_montar_grade_prevista_periodo_legacy:955` | Todas escalas da pessoa em `:966`; EC por `vinculo_do_dia` em `:973`, com células em `:970`; segmenta por EC em `:975-982`. | Chama builder simples em `:995`, retém só datas do segmento em `:999`; wrapper seleciona legacy em `:2038`. Herda overlay acima. |
| `app/escala/utils.py::montar_grade_prevista_periodo_por_turno:1229` | Todas escalas da pessoa em `:1248`; células carregadas em `:1260`; EC por `vinculo_do_dia:1263`, mapa em `:1268`. | Wrapper `montar_grade_prevista_periodo:2036`; overlay em `:1794`. Wrapper é chamado por cartório em `app/ponto/services/cartorio.py:946`, reconciliação em `app/ponto/management/commands/reconciliar_grade.py:118` e contrato de grade em `app/escala/utils.py:2286`. |
| `app/colaboradores/services/calendario.py::contexto_calendario:252` | Foto para previsão carrega só EC disponível em `:606`; fatos A2 usam `fatos_do_periodo` em `:617-619`, **sem** sobrescrever `folgas_lancadas`. Otimização passa `[ec]` se cobre as duas pontas, pelo juiz de cobertura em `:614-619`. Origem/vínculo válido são julgados pela precedência, com universo pessoa, conforme ligação em `:617-623`. | `folga_humana_ou_feriado` em `:817-820` alimenta `_classificar:839` e `trabalho_do_dia:840`; esta última força False para folga humana em `:103-104`. Chamadores: view `app/colaboradores/views.py:1946`, drawer `app/colaboradores/services/drawer.py:121`, relatório `app/relatorios/furos.py:184`, portas locais de status/resolução em `calendario.py:211,223,234`. |
| `app/ponto/status_dia.py::fatos_do_status:124` | Consulta A2 pela pessoa/data somente com veredito `indefinida`, em `:135-139`; herda regra de vínculo/origem. | Cartório `app/ponto/services/cartorio.py:749,1062,1583`; resultado `folga_humana` entra nos fatos do status em `status_dia.py:149`. |
| `app/ponto/services/cartorio.py::aplicar_precedencia_na_celula:1681` | Consulta `fatos_do_dia(cel.colaborador,cel.data)` em `:1694`, herdando universo e EC da precedência. | `julgar_celula:1743`; carimba faixa, tipo, suprimidos e previsto na ata em `:1695-1702`. |
| `app/ponto/services/espelho.py::montar_dias`, chamada A2 em `:659` | Pessoa/período, sem carga própria de FolgaDia/origem; herda regra da precedência. | Transporta fato em `:881`; `aplicar_palavra_do_dia` o lê em `:342`, chamada ao final em `:1295`. Move palavra/cor do espelho e cartão; PDF delega desenho e palavra à tela, ligação documentada em `app/relatorios/pdf_espelho.py:371-378`. |
| `app/folha/services/sem_furo.py::dias_com_furo:16` | Pessoa, intervalo mínimo/máximo das células com lâmpada apagada; A2 em `:25`; calendário de outro EC recusado por delegação. | Só mantém apagados cujo `tipo_do_dia == trabalho` em `:26`. `colabs_sem_furo:51` usa a resposta no contador; command `app/folha/management/commands/colabs_sem_furo.py:13`. |
| `app/ferias/situacao.py::_dia_de_descanso:72` | Consulta A2 em `:94`, mas lê `fatos.folga_lancada` em `:97`, preservando repouso mesmo com férias vencendo a precedência; também lê `era_dia_previsto` em `:99`. | `_vespera:110`, usada em `:246,320`; move aviso de início de férias junto de repouso/feriado, descrito em `:102-105`. |
| `app/escala/services/semana_do_posto.py::_fato_do_dia:168` | Pessoa/data pela A2 em `:177`; herda regra do EC e da origem. | `grade_do_posto:151-158` classifica previsto sem batida como explicado ou buraco. Grade chamada na API `app/api/views_mensageria.py:2106`, propostas em `semana_do_posto.py:190,234`. |
| `app/chamados/services/feriado_reavaliar.py::espurios:23` | Pessoa e dia do chamado pelo juiz `dia_do_fato:30`; A2 em `:33`. Exige previsão conhecida em `:34`, faixa feriado/folga em `:36` e premissa morta em `:38`. | `app/chamados/models.py:1253`, command `app/ponto/management/commands/reavaliar_ausencias_feriado.py:24`, serviço `reavaliar_dias:82`; move reconciliação em `feriado_reavaliar.py:47`, anotação em `:49-55`. Evento da porta da célula chama `reavaliar_dias` em `app/ponto/portas/celula.py:432`. |
| `app/ponto/services/dia_do_colab.py::dia_do_colab:6` | Pessoa/data pela A2 em `:14`; herda regra. | Ficha em `app/colaboradores/services/ficha.py:280`, API em `app/api/views_mensageria.py:1963`; publica fato vencedor em `dia_do_colab.py:84-89` e contradição com célula em `:130-136`. |
| `app/ponto/services/abriu_nao_bateu.py::com_nomes:94` | Pessoa/hoje pela A2 em `:123`; herda regra. | Command `app/ponto/management/commands/lavrar_abriu_nao_bateu.py:30`; exclui quem não tem faixa trabalho da lista em `abriu_nao_bateu.py:123-124`. |
| `app/ponto/services/deslogados.py::apurar`, consulta em `:166` | Pessoa/dias candidatos pela A2; remove dias cujo tipo não é trabalho/None em `:166-168`. | `app/ponto/management/commands/lavrar_deslogados.py:38` e pautas em `app/ponto/services/pautas_dos_deslogados.py:62`; move corrida de plantões e lista de deslogados em `deslogados.py:168-190`. |
| `app/ponto/services/faltas_de_hoje.py::faltas_de_hoje_sem_cobranca:13` | Pessoa/hoje pela A2 em `:61`; seleção inicial usa EC ativo recente em `:35-40`, antes da pergunta A2. | Command `app/ponto/management/commands/faltas_de_hoje_sem_cobranca.py:16`; A2 exclui dias sem trabalho em `faltas_de_hoje.py:61-62`, move contador em `:67-70`. |
| `app/ponto/management/commands/detectar_ausencias.py`, consulta em `:616` | Pessoa/data pela A2; trecho testa exclusivamente faixa feriado em `:616`. | Não é um novo filtro de FolgaDia: lê resultado amplo da A2, mas o ramo mostrado só suprime feriado, conforme `:610-616`. |

Todos os chamadores de `folgas_do_periodo` fora models/precedência encontrados:

| Local | Seleção e decisão |
|---|---|
| `app/escala/utils.py:1384` | Todos ECs da pessoa em `rows`; previsão por EC do dia, dado pelo juiz em `:1263,1268`. Overlay FolgaDia usa autoridade separada em `:1794`. |
| `app/colaboradores/services/calendario.py:606` | Foto só do `ec` disponível; `eh_dia_trabalho` em `:786`; fato humano amplo é consultado separadamente em `:617`. |
| `app/api/views.py::api_espelho_v2:1609` | `esc` vem da autoridade do espelho em `:1371`; `eh_dia_trabalho:1627`; move falta/previsto/neutro em `:1636-1655`. |
| `app/core/services/painel_op.py::montar_painel_operacional:105` | ECs do mapa `escalas:74`; `_previsto_agora:109` pergunta `ec.eh_dia_trabalho:118`; move previsão de turno agora em `:123`. |
| `app/ponto/management/commands/censo_fase_12x36.py:176` | ECs dos segmentos escolhidos por `vinculo_do_dia:161`; previsão por segmento em `:179`; paridade com realizado em `:184`. |
| `app/ponto/management/commands/diff_espelho_alimentacao.py:101` | ECs de `escalas_do_periodo:96`; compara previstas e motores com alimentações distintas em `:103-122`. |
| `app/relatorios/pdf_espelho.py::_coletar_dados_espelho:361` | Escalas do período e geradoras fora de vigência adicionadas em `:354-359`; alimenta `previstas_do_periodo:368-370` e motor por fatia em `:383`. |
| `app/ponto/services/espelho.py::autoridade_do_periodo:1071` | Escalas da janela mais geradoras em `:1057-1059`; EC do fim pelo juiz em `:1069`; alimenta previstas em `:1073`. |

Esses mapas preservam chave EC/data e não examinam `calendario_origem`; a decisão é delegada ao modelo e a `previstas_do_periodo` nas chamadas indicadas acima. Não estabelecem por si a regra “manual da pessoa / calendário do EC do dia”. (`app/escala/utils.py:1384`; `app/colaboradores/services/calendario.py:606`; `app/api/views.py:1609`; `app/core/services/painel_op.py:105`; `app/ponto/management/commands/censo_fase_12x36.py:176`; `app/ponto/management/commands/diff_espelho_alimentacao.py:101`; `app/relatorios/pdf_espelho.py:361`; `app/ponto/services/espelho.py:1071`.)

Chamadores diretos de `eh_dia_trabalho_calculado` fora models/precedência:

- `app/ponto/portas/celula.py::regenerar_celulas_vinculo:243`: EC recebido; monta DNA em `:244`, cria/atualiza célula em `:247,251`.
- `app/ponto/portas/celula.py::gerar_celulas_periodo:590`: EC ativa em `:568`, cobertura pelo juiz em `:588`; cria DNA/célula em `:593-596`, preserva editada em `:598`.
- `app/ponto/management/commands/censo_col900.py:115`: EC do dia por `vinculo_do_dia:111`, todas escalas em `:108`; compara DNA vivo com congelado em `:118-121`.
- `app/ponto/management/commands/tripwire_celulas.py::medir_stale:27`: EC do mapa por pessoa vindo de `ativa=True`/pessoa ativa em `:17-19`; não consulta geradora para selecionar esse EC; compara trabalha e DNA em `:29-33`.
- `app/colaboradores/management/commands/corrigir_escala_retroativa.py:120`: projeta EC de mesmo PK em `:107`, tipo novo em `:108` e âncora opcional em `:110`; compara previsão em `:118,128-130`.

Testes específicos que fixam os comportamentos principais: [nao verificado]

- Grade: calendário alheio recusado, manual antigo aceito, calendário próprio aceito, candidata alheia de PK menor não esconde própria, manual fora da janela: `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:70,75,80,85,91`.
- Overlay básico FolgaDia: `app/escala/tests/test_grade_folga_overlay.py:37,43`.
- Correção aplicável a competências antigas e simulação, mantendo manual: `app/escala/tests/test_grade_folga_vale_em_toda_competencia.py:74,79,84,90`.
- Rejulgar ata antiga com força, preservar fechamento/TXT, ausência de rejulgamento automático sem força: `app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:101,106,121`.
- Calendário: manual do antigo aceito e `eh_trabalho=False`, calendário antigo recusado, calendário ativo recusado quando geradora é outra: `app/colaboradores/tests/test_calendario_folga_a_mao_e_da_pessoa.py:56,66,102`.
- Status lavrado com previsão indefinida: FolgaDia manual produz status folga: `app/ponto/tests/test_e3_status_indefinida_a2_na_lavra.py:82`.
- Folha sem furo: mesma lâmpada apagada não conta em folga/ausência; contador: `app/folha/tests/test_sem_furo.py:67,75`.
- Foto carregada em lote cobre mês completo, mesmo folga fora da janela pedida: `app/escala/tests/test_selo_alimentacao_dias.py:96`; custo constante: `:73`.
- Regeneração de célula por FolgaDia e idempotência: `app/ponto/tests/test_porta_celula.py:46`; recorte só do dia: `app/ponto/tests/test_porta_celula_dia.py:79,85`; mesma fixture regenerada por vínculo toca mais dias: `:99`.
- Painel: 12x36 de folga não é previsto, DNA vence template, turno iniciado ontem permanece previsto: `app/core/tests/test_painel_previsto_celula.py:58,70,79`.
- Palavra única tela/cartão/calendário/template: `app/relatorios/tests/test_palavra_do_dia.py:205,228,307`; esses testes não fixam especificamente calendário de outro EC [nao verificado].
- Semana do posto: atestado remove buraco em `app/escala/tests/test_semana_do_posto.py:125`; teste específico de FolgaDia/origem nesse consumidor [nao verificado].
- Corrida de deslogados: cobertura não rompe nem estica em `app/ponto/tests/test_lista_viva_dos_deslogados.py:38`; teste específico de calendário de outro EC [nao verificado].
- Vigia de faltas de hoje e contador: `app/ponto/tests/test_vigia_falta_de_hoje.py:62,68,74`; teste específico de calendário de outro EC [nao verificado].

Templates/tags que consultem diretamente FolgaDia ou `calendario_origem`: [nao verificado]. O template de grade é fixado como consumidor de palavra pronta, pelo teste `app/relatorios/tests/test_palavra_do_dia.py:307`.

## 31/10 19:40 · sessao f567806d1e0f · - · - · origem=Ronald (tmux)

Censo de campos e v3, somente por leitura:

- **Definições dos campos persistidos:** `DiaPago.horas_noturnas` e `.horas_extras_100_noturna`: `app/ponto/models.py:327`, `app/ponto/models.py:332`; correspondentes do `FechamentoMensal`: `app/ponto/models.py:413`, `app/ponto/models.py:419`.
- **Origem no motor:** `PeriodoCalculo.horas_noturnas` converte minutos legais em horas: `app/ponto/motor_calculo_v2.py:210`; `ResultadoMes.horas_noturnas` soma `periodos + periodos_ft`: `app/ponto/motor_calculo_v2.py:271`; `ResultadoMes.horas_extra_100_noturna` soma apenas `periodos`: `app/ponto/motor_calculo_v2.py:292`.
- **Escritor mensal canônico:** atribui ambos do `resultado`: `app/ponto/services/fechamento.py:893`, `app/ponto/services/fechamento.py:899`; persiste com `queryset.update`: `app/ponto/services/fechamento.py:1131`, `app/ponto/services/fechamento.py:1137`.
- **Outro escritor mensal genérico:** `restaurar_fechamento` aceita campos existentes do modelo e persiste `**_upd`, portanto pode escrever ambos: `app/ponto/services/fechamento.py:1342`, `app/ponto/services/fechamento.py:1348`, `app/ponto/services/fechamento.py:1360`.

**DiaPago/v3**

- Ambos pertencem a `CAMPOS_DIA`: `app/ponto/services/dia_pago.py:34`.
- `lavrar` soma noturnas e HE100 noturna dos períodos comuns em `app/ponto/services/dia_pago.py:179`, `app/ponto/services/dia_pago.py:184`; folgas acrescentam somente noturnas na rotina compartilhada em `app/ponto/services/dia_pago.py:224`.
- Quando recebe rubricas v3, substitui genericamente os campos declarados em `valores`, exceto `do_motor`: `app/ponto/services/dia_pago.py:320`, `app/ponto/services/dia_pago.py:327`; monta e grava os objetos em `app/ponto/services/dia_pago.py:343`, `app/ponto/services/dia_pago.py:357`.
- `retratar_exportada` pode criar linhas e ajuste com ambos zerados por compreensão sobre `CAMPOS`: `app/ponto/services/dia_pago.py:470`, `app/ponto/services/dia_pago.py:476`, `app/ponto/services/dia_pago.py:508`, `app/ponto/services/dia_pago.py:513`; persiste em `app/ponto/services/dia_pago.py:543`.
- v3 escreve `horas_noturnas` no ramo folga em `app/ponto/calculador/regras.py:739` e no ramo comum em `app/ponto/calculador/regras.py:912`; escreve HE100 noturna em `app/ponto/calculador/regras.py:898`.
- Os dados v3 chegam de `rubricas_por_dia`; flags CCT por dia são extraídos em `app/ponto/calculador/alimentacao.py:572`, `app/ponto/calculador/alimentacao.py:573` e passados em `app/ponto/calculador/alimentacao.py:593`, `app/ponto/calculador/alimentacao.py:596`.
- Na integração do fechamento, o motor é lavrado como `versao='motor'`: `app/ponto/services/fechamento.py:1170`, `app/ponto/services/fechamento.py:1195`; rubricas v3 são calculadas e lavradas separadamente como `versao='oraculo'`: `app/ponto/services/fechamento.py:1239`, `app/ponto/services/fechamento.py:1245`, `app/ponto/services/fechamento.py:1256`, `app/ponto/services/fechamento.py:1262`.

**_ContextoNoturno**

- Converte o cadastro em `bool` e empresta o método do motor, por alias de atributo de classe: `app/ponto/calculador/regras.py:60`, `app/ponto/calculador/regras.py:63`.
- `noturnas_do_dia` passa o mesmo flag ao cálculo do par e ao contexto do desconto: `app/ponto/calculador/regras.py:80`, `app/ponto/calculador/regras.py:85`, `app/ponto/calculador/regras.py:86`.
- O desconto executa o método emprestado; a função final calcula cada intervalo recortado com seu próprio início/fim, sem transportar uma janela noturna previamente decidida pelo turno: `app/ponto/motor_calculo_v2.py:145`, `app/ponto/motor_calculo_v2.py:1865`.
- HE v3 chama o alias `_he_not` por período, com as pontas e intervalos desse período: `app/ponto/calculador/regras.py:868`, `app/ponto/calculador/regras.py:880`, `app/ponto/calculador/regras.py:886`.

**Leitores de produção, inclusive dinâmicos**

- `soma_da_competencia` agrega ambos via `CAMPOS`: `app/ponto/services/dia_pago.py:560`; `soma_do_periodo` via `CAMPOS_DIA`: `app/ponto/services/dia_pago.py:782`, `app/ponto/services/dia_pago.py:785`.
- `pago_do_dia` lê ambos genericamente para verificar existência de dinheiro: `app/ponto/services/dia_pago.py:859`, `app/ponto/services/dia_pago.py:890`.
- `rubricas_do_dia` lê noturnas para tela/papel: `app/ponto/services/dia_pago.py:922`; consumidores: `app/ponto/services/espelho.py:824`, `app/colaboradores/services/calendario.py:785`, PDF por chave `noturnas` em `app/relatorios/pdf_espelho.py:840`, `app/relatorios/pdf_espelho.py:906`.
- `folha/porta_export` compara todos os campos retornados pela soma com `getattr` do fechamento, abrangendo ambos: `app/folha/porta_export.py:451`, `app/folha/porta_export.py:452`.
- `folha/export` lê `horas_noturnas` e emite `adicional_noturno`: `app/folha/export.py:436`; não emite uma linha própria de HE100 noturna no retorno: `app/folha/export.py:428` a `:450`. A justificativa dos subsets está em `app/folha/export.py:413`. O TXT resolve o código pelo cadastro de rubricas, não por `25` cravado: `app/folha/export.py:694`, `app/folha/export.py:716`, `app/folha/export.py:719`; “rubrica 25” está em comentário: `app/folha/export.py:419`. Código cadastrado real de produção: [nao verificado].
- `eventos_do_fechamento` é consumido pelo TXT em `app/folha/export.py:711` e pela homologação em `app/folha/export.py:976`.
- Relatórios: CSV mensal `app/relatorios/views.py:94`, soma mensal `app/relatorios/views.py:108`, extrato `app/relatorios/views.py:524`, cartão pela soma diária `app/relatorios/cartao_pela_celula.py:176`, espelho pela property do período `app/ponto/services/espelho.py:99`.
- Templates leem noturnas: `app/templates/relatorios/horas_mes.html:68`, `app/templates/ponto/_fech_linha.html:31`, `app/templates/ponto/partials/painel_fechamento.html:33`.
- Ranking lê efetivamente noturnas em `app/folha/services/ranking_he.py:225`. A tupla `CAMPOS` inclui ambos em `app/folha/services/ranking_he.py:33`, mas não tem uso naquele módulo.
- Simulação de folha lê ambos via `CAMPOS_HORAS/getattr`: `app/folha/management/commands/simular_folha.py:59`, `app/folha/management/commands/simular_folha.py:84`.

**Comandos e scripts: não omitir do censo**

- `aplicar_09_he_intra`: lê ambos no snapshot em `app/ponto/management/commands/aplicar_09_he_intra.py:128`; HE100 noturna é alvo em `:39`–`:40`, e recebe gravado + delta pela porta em `:141`–`:152`; noturnas está só no snapshot em `:43`.
- `aplicar_09_corte_b`: lê ambos via `CAMPOS` em `app/ponto/management/commands/aplicar_09_corte_b.py:184`; recalcula em `:212`; pode restaurar ambos dentre todos os campos em `:265`–`:268`.
- `desvio_o68b`: lê snapshot em `app/ponto/management/commands/desvio_o68b.py:149`, `:202`; restaura todos os campos selecionados em `:195`–`:198`.
- `carimbo_gravado`: introspecção dos campos em `app/ponto/management/commands/carimbo_gravado.py:36`, leitura em `:54`; `lavrar_dias_pagos` faz hash por introspecção em `app/ponto/management/commands/lavrar_dias_pagos.py:70`, `:76`.
- `celula_veredito_velho` lê noturnas via tupla em `app/ponto/management/commands/celula_veredito_velho.py:70`, `:85`, `:155`.
- `diff_reclassificar_partido` lê noturnas em tupla `app/ponto/management/commands/diff_reclassificar_partido.py:67` e getters em `:146`, `:156`, `:212`; sua restauração explícita é só banco em `:224`–`:225`.
- `diff_frota` lê campos recebidos via getter em `app/ponto/services/diff_frota.py:63`; recebe a lista canônica que inclui ambos dos comandos `app/ponto/management/commands/diff_col900.py:45`, `app/ponto/management/commands/diff_passivo_s84.py:50`, `app/ponto/management/commands/diff_janela_he_total.py:101`, `:110`.
- `recalcular_fechamento` agrega ambos pela lista canônica em `app/ponto/management/commands/recalcular_fechamento.py:54`, `:67`.
- `regen_exportada` lê ambos por hash/snapshot em `app/ponto/portas/regen_exportada.py:72`, `:84`.
- **Escritores diretos fora da porta:** `bin/restore_relavratura_10_2026.py:79`–`:81` e `bin/restore_o263_comp10.py:48`–`:50` fazem `setattr/save` com `VALOR` derivado de `CAMPOS`, abrangendo ambos; recriam DiaPago em `bin/restore_relavratura_10_2026.py:86`, `bin/restore_o263_comp10.py:56`.
- Snapshots bin leem ambos: `bin/snapshot_relavratura_10_2026.py:31`, `:36`–`:37`; `bin/snapshot_o263_comp10.py:57`–`:58`, `:63`; diff pontual em `bin/diff_o118_col221.py:23`, `:27`.
- O modo leitura do fechamento também serializa ambos pela lista canônica: `app/ponto/services/fechamento.py:1102`–`:1104`.

Testes que usam os campos podem ser agrupados como fixtures e assertions; evidências úteis: `app/folha/tests/test_export_invariante_he.py:27`, `app/folha/tests/test_export_59a.py:14`, `app/folha/tests/test_export_dsr_reflexo.py:25`, `app/folha/tests/test_sm1_monster_export.py:92`, `app/ponto/tests/test_dia_pago_soma.py:231`, `app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:69`, `:74`, `:107`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:52`, `:58`, `:62`, `app/ponto/tests/test_v3_periodos_da_celula.py:116`, `:134`, `:170`, `:176`.

## 31/10 19:40 · sessao f9695340e72e · - · - · origem=Ronald (tmux)

Censo complementar dos leitores diretos, somente leitura:

| Sítio e filtro | Universo / ec / calendário de outro vínculo | Chamador e decisão | Testes | [nao verificado]
|---|---|---|---|
| `app/escala/alimentacao.py:49` `folgas_do_periodo`; query `:66` | Lista de vínculos fornecida; agrupa datas por `ec_id`; arredonda início/fim para meses inteiros (`:60`). Não escolhe ec do dia nem lê origem: calendário de outro vínculo fica no balde daquele outro vínculo. | Alimenta cálculo do vínculo: `app/ponto/precedencia.py:333`; grade `app/escala/utils.py:1384`; calendário `app/colaboradores/services/calendario.py:606`; painel `app/core/services/painel_op.py:105`; PDF `app/relatorios/pdf_espelho.py:361`; censos `app/ponto/management/commands/censo_fase_12x36.py:176` e `diff_espelho_alimentacao.py:101`. | `app/escala/tests/test_selo_alimentacao_dias.py:73` custo constante e `:96` mês inteiro; contrato lista de vínculos em `app/escala/tests/test_carregadores_contrato.py:6`. |
| `app/escala/models.py:1212` `defeito_folga_nao_definida`; query `:1220` | Próprio `self` e mês da data. Para 6x1 sem `folga_dia_semana`, ausência de qualquer FD no mês acusa defeito. Não lê origem; outro vínculo não serve. | Plano `app/escala/views_plano_folgas.py:89`; limbo `app/chamados/services/limbo_folgas.py:199`, `:281`; detector `app/ponto/management/commands/detectar_ausencias.py:144`, `:187`; supra juiz `app/ponto/supra_juiz.py:200`; faltas hoje `app/ponto/services/faltas_de_hoje.py:39`; régua `app/escala/regua_defesa.py:145`, `:176`; diagnóstico `app/ponto/services/diagnostico_escala.py:104`. Move defeito/cobrança de cadastro, visibilidade e guardas. | `app/escala/tests/test_t1_ligado_por_padrao.py:34`, `:39`, `:45`, `:55`; `app/escala/tests/test_plano_folgas_filtro.py:53`, `:58`, `:61`. |
| `app/escala/models.py:1223` `eh_dia_trabalho_calculado`: foto mensal `:1257`, existência no dia `:1281`, anterior `:1327`, posterior `:1330`; ou mapa alimentado `:1251` | Próprio vínculo (`self`); **nenhuma dessas leituras distingue manual/calendário**. Outro vínculo não entra. Foto no dia => `False` (`:1280`); fora da foto usa ciclo se ele sabe (`:1290`), senão paridade da foto mais próxima (`:1322`); sem foto mensal pode usar âncora (`:1260`). | `eh_dia_trabalho` retorna célula se existe e só cai aqui sem célula: `app/escala/models.py:1048`, `:1071`. Geradora/regeneradora chama diretamente em `app/ponto/portas/celula.py:243`, `:590` e grava `trabalha`/DNA (`:244`, `:594`); precedência em `app/ponto/precedencia.py:368`; projeção retroativa `app/colaboradores/management/commands/corrigir_escala_retroativa.py:120`; sentinelas `app/ponto/management/commands/tripwire_celulas.py:27`, `censo_col900.py:115`. Move célula, grade e decisões downstream. | `app/escala/tests/test_foto_vence_ancora.py:34`, `:38`, `:44`, `:52`; `app/escala/tests/test_foto_nao_apaga_o_ciclo.py:50`, `:60`, `:67`, `:73`; continuação da foto parcial em `app/escala/tests/test_o37_foto_parcial.py` [linhas de métodos não verificadas]; alimentação acima. |
| `app/escala/services/fase_conflitante.py:61` `medir`; query `:74`; derivador `_foto_diz` `:51` | Itera **cada ec ativo** (`:71`) e todas suas FD, sem recorte de data na query. Não seleciona ec por dia/vigência; não lê origem. Foto de outro ec não entra. `_foto_diz` marca data presente como folga e extrapola paridade a partir da mais próxima (`:55`). | `app/escala/management/commands/lavrar_fase_conflitante.py:31` mede e `:35` grava snapshot; cron `app/config/crons.py:479`; API lê snapshot em `app/api/views_mensageria.py:632`, `:643`. Conta conflito com âncora; não altera veredito (`app/escala/services/fase_conflitante.py:80`). | `app/escala/tests/test_fase_conflitante.py:60`, `:72`, `:80`, `:84`, `:88`, `:93`. |
| `app/escala/folgas.py:83` `divergencia_foto_x_calendario`; query `:101`, origem `:104` | Próprio ec + mês; separa FD manual e calendário; compara com calendário **atual** de `ec` (`:99`), sem exigir que a origem de cada FD seja esse calendário atual. Outro vínculo não entra. Não escolhe ec do dia. | Contador `foto_que_o_calendario_de_hoje_nao_manda` em `app/escala/folgas.py:133`, chamada `:143`; comando `app/escala/management/commands/lavrar_foto_folgas.py:113`, `:167`. `falta` permite lavra; `sobra` impede reconciliar o mês (`:123`, `:168`). | `app/escala/tests/test_foto_folga_continua.py:136` sobra bloqueia; `:158` vazio/contador; `:179` manual preservado. |
| `app/escala/folgas.py:150` `gerar_foto_folgas`; leitura manuais `:165`, filtro origem `:167`; filtro calendário `:169`/`:171` | Próprio ec + mês; manual preservada; FD com origem não nula é reconciliada com calendário atual. Outro vínculo não entra; ec é argumento. | `app/escala/views.py:414`, `:415`; `app/colaboradores/views.py:245`, `:246`; `app/colaboradores/services/vinculo.py:163`, `:164`; lavra `app/escala/management/commands/lavrar_foto_folgas.py:170`. Cria foto e chama re-lavra `app/escala/folgas.py:192`. | `app/escala/tests/test_gerador_folgas.py:39`, `:47`, `:54`, `:63`; `app/escala/tests/test_torneira_folgas.py:87`; `app/escala/tests/test_foto_folga_continua.py:179`; lista de chamadores é fixada por `:260`. |
| `app/escala/folgas.py:228` `lancar_folga`; query `:254`/origem `:255` | Próprio ec + dia; calendário no próprio vínculo veta lançamento manual. FD de outro vínculo não é procurada. Ec fornecido pelo chamador. | Plano `app/escala/views_plano_folgas.py:131`; declaração `app/ponto/services/declarar_dia.py:183`; veto precedência em `app/escala/folgas.py:258`, foto `:259`, re-lavra `:261`. | `app/escala/tests/test_torneira_folgas.py:68`; `app/escala/tests/test_bug136_plano_em_lote_relavra.py:114` para veto equivalente no helper; testes da porta declarar com outro agente. |
| `app/escala/folgas.py:267` `_foto_do_dia`; query `:274`/origem `:275` | Próprio ec + dia; origem não nula veta alteração/remoção; outro vínculo não entra. | `alterar_folga` `app/escala/folgas.py:302` e lote `:334`; alterar chama re-lavra `:303`; lote recusa dias anteriores à competência `:330`, calendário `:335`, re-lavra `:340`. Portas views `app/escala/views_plano_folgas.py:136`, `:185`. | `app/escala/tests/test_bug136_plano_em_lote_relavra.py:68`, `:75`, `:84`, `:93`, `:105`, `:114`, `:124`. |
| `app/escala/folgas.py:392` `reparentar_folga`; origem `:405`; ocupação destino `:418` | Folga individual; calendário retorna `'calendario'` sem migrar; manual do próprio ec é mantida se cobre o dia (`:408`), senão destino vem de `vinculo_do_dia_carregado` (`:414`). Se destino já tem FD de **qualquer origem**, não migra (`:418`). | `app/escala/folgas.py:459` e comando `app/escala/management/commands/reparentar_folgas.py:34`. Move titularidade e re-lavra só competência corrente em diante (`app/escala/folgas.py:435`, `:441`). | `app/escala/tests/test_folga_segue_a_pessoa.py:53`, `:66`, `:80`, `:92`, `:104`, `:113`. |
| `app/escala/folgas.py:447` `reparentar_folgas_do_colaborador`; query `:453` | Pessoa inteira, **somente manuais** (`:454`); filtra as cujo vínculo não cobre data (`:457`); destino delegado acima. Calendário outro vínculo excluído. | Signal `app/escala/signals.py:190`, `:191` após mudança do vínculo. Mesmo efeito da porta individual. | `app/escala/tests/test_folga_segue_a_pessoa.py:53`; casos individuais acima. |
| `app/escala/management/commands/reparentar_folgas.py:22` `handle`; query `:28` | Todas FD manuais, ordenadas por pessoa/data; não requer ec ativo; ignora as cujo ec cobre (`:31`); destino pela porta `:34`. | Entrada management command (`:16`); flag decide dry/apply `:26`; migração/re-lavra pela porta. | Teste específico do command [nao verificado]; porta fixada acima. |
| `app/escala/views_plano_folgas.py:51` `_grid_ctx`; query `:60` | Vínculos **ativos** da empresa/posto (`:55`), mês pedido; mapa por ec/dia (`:63`). Não escolhe vínculo por dia nem vigência. Não lê origem, outro vínculo não pinta linha deste ec. FD presente tem prioridade sobre previsão (`:70`); sem FD usa `ec.eh_dia_trabalho` (`:76`). | View `plano_folgas` chama em `app/escala/views_plano_folgas.py:46`; URL `app/escala/urls.py:8`. Pinta grid/defeito de cadastro. | `app/escala/tests/test_plano_folgas_filtro.py:53`, `:58`, `:61`, `:64`; `app/escala/tests/test_folga_cadastro.py:26`. |
| `app/escala/views_plano_folgas.py:98` `plano_folgas_toggle`; query `:112`/origem `:113` | Ec ativo pelo ID do POST (`:109`), dia do POST; calendário próprio gera 409. Não verifica ec do dia; outro vínculo não entra. | URL `app/escala/urls.py:9`; chama lançar/alterar `app/escala/views_plano_folgas.py:131`, `:136`. Veto ou gravação/re-lavra. | `app/escala/tests/test_bug136_plano_em_lote_relavra.py:124`; `app/escala/tests/test_torneira_folgas.py:68`. |
| `app/ponto/services/diagnostico_escala.py:81` `template_do_vinculo`; query `:102` | Próprio ec + mês; origem ignorada; outro vínculo não conta. **Ec aqui é `atual = vinculos[0]`** (`:182`), não escolhido por cada dia. | `diagnostico_escala` `app/ponto/services/diagnostico_escala.py:193`; sem folga cadastrada gera ação Plano (`:194`); API `app/api/views_mensageria.py:1995`. Diagnóstico/contador. | `app/api/tests/test_diagnostico_template.py:36`, `:45`; teste específico de count FD [nao verificado]. |
| `app/relatorios/furos.py:78` `escalas_em_limbo`; query `:112` | Cada ec ativo sem fim, colaborador ativo (`:86`), após dois dias sem previsão (`:93`); FD do mês atual e ec próprio. Não lê origem/outro vínculo; ec por queryset, não juiz do dia. | `app/relatorios/views.py:1552`; inclui peça explicativa “sem foto FolgaDia do mes” (`app/relatorios/furos.py:115`) na fila de cadastro. | `app/relatorios/tests/test_selo_ancora_do_vinculo.py:51` fixa diagnóstico da âncora; teste específico origem/FD [nao verificado]. |

Outros auxiliares relevantes da lavra: universo ativo/calendário/mês vem de `app/escala/folgas.py:59`, usando carregador de vínculos em `:78`; `app/escala/management/commands/lavrar_foto_folgas.py:101` mede falta/sobra, e `:144` handle reconsulta antes de gravar (`:167`). Testes em `app/escala/tests/test_foto_folga_continua.py:80`, `:99`, `:125`, `:136`, `:158`, `:199`, `:208`.

Atenção à escolha de ec no diagnóstico: `app/ponto/services/diagnostico_escala.py:49` `_vigente` pega o **primeiro vínculo que cobre a data** usando `vinculo_cobre_a_data` (`:53`), chamado em `:150`; não chama `vinculo_do_dia` nem toma a geradora da célula nessa escolha. Isso afeta previsão diagnóstica em `:152`, não a contagem mensal que usa `vinculos[0]` (`:182`, `:193`).

Não identifiquei uso operacional de reverse `folgas_dia` além da definição em `app/escala/models.py:1554`; ausência total além dos sítios pesquisados [nao verificado].

## 31/10 19:40 · sessao b38a17c024b7 · - · - · origem=Ronald (tmux)

Censo somente por leitura; nenhum teste executado. [nao verificado]

**Alvo: dois chamadores diretos, nenhum em teste ou por alias de import.** A varredura AST de todos os `.py` encontrou somente os dois `Load` abaixo; nenhuma ocorrência semântica como import, atributo, atribuição ou string exata para `getattr`. Os usos do nome em testes são texto explicativo/mensagem, em `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:18` e `:54`.

| Chamador direto | `prorrogacao` | `entrada` / `saida` recebidas pelo alvo | Destino |
|---|---|---|---|
| `he_noturna`, `app/ponto/motor_calculo_v2.py:392` | Mesmo argumento `prorrogacao` de `he_noturna`, padrão `True` (`:359`, `:393`) | `inicio_extra` calculado por `_inicio_do_trecho_extra(entrada,fim,total_extra,intervalos)` em `:390`; `fim=saida` em `:385`. Portanto recorte da cauda extra, não necessariamente turno inteiro. | Subtraído de `minutos_extra_not`; dividido proporcionalmente em `not_50/not_100` e retornado (`:394`, `:398`, `:400`, `:401`). |
| `MotorBase._noturno_dos_intervalos`, `app/ponto/motor_calculo_v2.py:1865` | `self.prorrogacao_noturna_pos5h` (`:1866`) | Encaminha seus próprios parâmetros sem mudar (`:1852`, `:1865`). | Retorna desconto para `calcular_noturnas` (`:923`) ou para o método emprestado pelo calculador (`app/ponto/calculador/regras.py:63`, `:86`). |

**Bool igual não equivale a janela igual.** O alvo encaminha o mesmo bool a cada intervalo, mas recorta cada pedaço e chama `minutos_noturnos` separadamente em `app/ponto/motor_calculo_v2.py:145`. A elegibilidade efetiva da prorrogação depende do início/fim da chamada: `inter_s1_ini=max(inicio,s1_ini)` e condição `inter_s1_ini < 05h` com `fim > 05h`, em `:109-111`. Logo, um intervalo iniciado às 05:30 não herda a elegibilidade de um turno iniciado às 22h. Nenhum dos dois chamadores fornece ao alvo uma janela noturna elegível ou o contexto temporal do turno além do recorte `entrada/saida`; assinatura e encaminhamentos em `:121`, `:392-393`, `:1865-1866`.

**Cadeias de produção para o desconto normal:**

- `MotorBase.calcular_noturnas` calcula o bruto com `self.prorrogacao_noturna_pos5h` e o mesmo par e desconta `_noturno_dos_intervalos(entrada,saida,intervalos)`, em `app/ponto/motor_calculo_v2.py:921-923`. O resultado multiplica pelo fator e retorna (`:924-925`); `_escrever_noturnas` grava `PeriodoCalculo.minutos_noturnos_relogio/legais` (`:941-943`).
- Há três chamadas de `_escrever_noturnas`: Motor12x36 `:2730`, MotorTurnoPartido `:2834`, MotorComercial `:3185`. Em cada uma, `e/s` vêm de `_janela_do_par(localizar(entrada),localizar(saida))` (`:2697-2698`, `:2824-2825`, `:3149-3150`), e os intervalos são recortados por `_intervalos_na_janela(e,s,intervalos)` (`:2702`, `:2829`, `:3162`).
- MotorPlantao reutiliza explicitamente o cálculo do Motor12x36 (`:3448-3449`); Motor12x36ComEscala chama `super().calcular_periodo` (`:3460-3461`); MotorTurnoPartidoNoturno herda sem implementação adicional (`:3474-3481`); MotorIntermitente herda Motor12x36 (`:3483`, `:3491-3495`).
- Os minutos legais são lidos pela property `PeriodoCalculo.horas_noturnas` (`:210-211`) e pela soma mensal `ResultadoMes.horas_noturnas`, que inclui `periodos + periodos_ft` (`:253`, `:271`). MotorTurnoPartido também lê o ganho `legais - relogio` para excedente em régua legal (`:3063-3066`).

**Cadeias do calculador, incluindo apelidos:**

- `_ContextoNoturno` armazena `bool(prorrogacao_pos5h)` (`app/ponto/calculador/regras.py:60-61`) e empresta exatamente `MotorBase._noturno_dos_intervalos` como atributo de classe (`:63`).
- `noturnas_do_dia` percorre cada par recebido, usa o mesmo bool no bruto e no contexto do desconto (`:80-86`), limita a soma a zero e aplica fator (`:87-89`): bruto por par, desconto efetivamente por intervalo.
- `regras.do_dia` chama `noturnas_do_dia` na folga trabalhada (`:736-738`) e grava `_vf['horas_noturnas']` (`:739`); no caminho normal chama em `:908-911` e grava `val['horas_noturnas']` (`:912`).
- `periodo.noturno_no_par` chama `noturnas_do_dia` com um único par inteiro e intervalos julgados; flag `cct.prorrogacao_pos5h` (`app/ponto/calculador/periodo.py:508-510`). Seu chamador lê o par `e/s_` em `:752` e grava `p.minutos_noturnos_relogio/legais` em `:753-754`.
- Para o split de HE, `regras.py` importa `he_noturna as _he_not` em `:868` e chama esse alias para cada ponta de período em `:886-888`, passando `prorrogacao_pos5h` e intervalos daquela ponta. Soma retornos (`:889-890`) e grava `horas_extras_50_noturna` / `horas_extras_100_noturna` (`:895-898`).
- A produção V3 obtém os pares dos carimbos `p.efetivo_entrada/saida` (`app/ponto/calculador/alimentacao.py:106`), lê `intervalos_do_motor` (`:127`), monta `he_pontas` (`:135`) e pares/intervalos por dia (`:185-186`); entrega tudo a `regras.do_dia` (`:593-596`). O bool vem da CCT daquele dia (`:572-574`), cujo motor é obtido por `get_motor_cct(c,_k,tipo_escala=_te,...)` (`:448`). `_cct_do_motor` transcreve o flag do motor (`app/ponto/calculador/nucleo.py:347-351`).

**Cadeias de produção para `he_noturna`:**

- `_aplicar_he_noturna` passa `periodo.entrada/saida`, extras 50/100, `self.prorrogacao_noturna_pos5h` e `periodo.intervalos_do_motor`, em `app/ponto/motor_calculo_v2.py:952-956`. Grava `minutos_extra_100_noturna` em `:958` e `minutos_extra_50_noturna` em `:960`; soma mensal lê esses campos em `:290` e `:294`.
- Os pares gravados em `periodo.entrada/saida` são localizações das pontas originais, em `:2689`, `:2816`, `:3141`; portanto esse caminho usa pontas cruas, enquanto o adicional normal recebe `e/s` efetivos, conforme referências anteriores.
- Chamadas exatas de `_aplicar_he_noturna`: Motor12x36 por período `:2793`; MotorTurnoPartido na HE diária `:3085` e semanal `:3116`; MotorComercial por período `:3219` e na HE diária mensal `:3337`. Na agregação diária o portador é `periodos_dia[-1]` (`:3021-3025`, `:3269-3273`); na semanal é `pds[-1]` (`:3106-3110`).

**Origem do bool do motor:** explicitamente recebido ou, quando `None`, lido de `prorrogacao_pos5h_legal(tipo_ciclo)` em `app/ponto/motor_calculo_v2.py:708-712`. A função legal retorna `tipo_ciclo != '12x36'` (`app/core/regua_cct.py:182`); a fábrica ativa injeta o valor da régua cadastrada em `:458`, `:466`, e no ramo desligado/CLT usa o ciclo em `:452-457`. O valor cadastrado de qualquer colaborador real é [nao verificado].

**Testes diretos de `he_noturna`, úteis para a cadeia:** `test_s5b_split_noturno_tem_um_dono.py:111-112` percorre bools `True/False` definidos em `:108`, sem intervalos; `:122-123` usa `True`, também sem intervalos. `test_noturno_desconta_intervalo.py:249-250`, `:263-267` usa `True` com intervalos noturnos antes das 05h. O teste P5 monta explicitamente turno 22–07, intervalo 05:30–06:30 e flag `True` (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:40-47`), exige 8h nas rubricas (`:51-54`, `:62-63`) e parte ≤ todo (`:58-59`); é teste da correção desejada, não aprovação do resultado atual descrito em seu docstring (`:4-6`, `:17-24`).

## 31/10 19:40 · sessao b04758375e64 · - · - · origem=Ronald (tmux)

Censo dos sítios sob minha responsabilidade, somente por leitura:

| Sítio | Universo e EC do dia | Calendário de outro vínculo | Chamadores e decisão | Testes | [nao verificado]
|---|---|---|---|---|
| `app/ponto/services/declarar_dia.py:61` `_folga_do_dia` | Filtra exatamente `escala_colaborador=ec,data=data`; EC fornecido pelo chamador. `:69–70`. | Não entra nessa consulta; também não entra a manual de outro vínculo. `:70`. | `o_dia_ja_e_folga` em `:91`; `_recusa_do_dia` em `:108`. | Existência manual e recusa: `app/ponto/tests/test_declarar_dia.py:79`; calendário impede declaração manual: `:87`. Caso de outro vínculo nessa porta: `[nao verificado]`. |
| `app/ponto/services/declarar_dia.py:73` `o_dia_ja_e_folga` | EC via `_ec_do_dia(:90)` → `escala_vigente(:56–58)`; esta carrega célula/geradora ou vínculos e chama `vinculo_do_dia`: `app/escala/servico_jornada.py:38`, `:46`, `:55–58`. | Só vê a linha do EC escolhido. `declarar_dia.py:91`, `:70`. | `app/chamados/services/validacao.py:261`, `:316`: aceite já satisfeito, carimbo, fechamento do fio sem declarar novamente (`:262–279`), ou repetição do ramo idempotente (`:316–319`). | Quatro perguntas dão uma folga/uma trilha/um chamado resolvido: `app/chamados/tests/test_o199_aceite_de_folga_decide_o_dia.py:167`. |
| `app/ponto/services/declarar_dia.py:94` `_recusa_do_dia` | Para tipo folga, escolhe EC pela porta acima `:105`; lê a linha em `:108`, origem em `:109`. | Não vê linha de outro EC; calendário do EC escolhido produz recusa “edite o calendario”; manual produz “ja e folga”. `:109–112`. | `checar_declaracao(:141)`; `declarar_dia` consulta novamente `:154`. Move veto/preview e escrita do lote `:159–168`; folga escrita por `lancar_folga(:183)` e fio fechado `:198`. Entrada IA registrada `app/core/intake.py:76–93`, resolvida `:481–494`, executada `app/core/views_ia.py:729`, `:775`. Aceite de resposta chama preview e escrita em `app/chamados/services/validacao.py:280`, `:291`; despacho para aceite em `:382–383`. | Recusas manual/calendário: `test_declarar_dia.py:79`, `:87`; precedência: `:99`; relavra: `:144`; fecha fio: `:157`; lote parcial: `:196`; rechecagem: `:208`. Aceite decide dia: `test_o199_aceite_de_folga_decide_o_dia.py:136`; fecha e deixa trilha: `:151`; recusa sem carimbar: `:194`. |
| `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:45` `Command.handle` | Todas as FolgaDia da pessoa na janela da empresa: filtro por empresa da pessoa e data, saída `(colaborador_id,data)`. `:53–57`. Não escolhe EC. | Sim, como candidato ao rejulgamento: não há filtro de EC/origem `:55–57`. O julgamento final é delegado a `julgar_celula(...,forcar=True)` `:69–71`. | Execução por command; classificado como ato único sob demanda em `app/config/crons.py:1400–1401`. `handle(:60)` chama `dias_de_ausencia_em_folga(:24)`; ausência não-falta aprovada cruzada com conjunto pessoa/data `:28–35`; dry-run `:63–65`; recusa se conjunto divergir de `MEDIDAS(:66–68)`. Move seleção de dias para nova lavra de ausência/folga. | `app/ponto/tests/test_rejulgar_ausencia_em_folga.py:23` ausência em folga; `:26` falta fica fora; `:29` aberta; `:58` dry-run; `:64` universo mudou; `:70` força porta. `app/ponto/tests/test_aus_f6_vencidas_pelo_juiz.py:27` usa expansão do juiz. Teste calendário de outro vínculo: `[nao verificado]`. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:1523` bloco C5 | Itera todas as FolgaDia, converte em pessoa/data; inclui se próprio vínculo cobre ou se `vinculo_que_cobre(...,exceto=...)` não retorna “nenhum” `:1530–1536`. Não escolhe EC soberano do dia. | Sim: não consulta origem nem compara dono da FolgaDia com vínculo soberano `:1532–1536`. | Corpo do command; compara célula da pessoa/data `:1537–1542`, acusa apenas `trabalha is True`, exclui competências exportadas `:1544–1547`, registra contador C5 `:1549–1550`. Move alarme de contradição folha/grade; não corrige aqui. | Teste específico desse C5: `[nao verificado]`. |

O gate solicitado **não lê FolgaDia diretamente nesta cópia**; ele lê a premissa do chamado e só chega à precedência num fallback. `app/chamados/models.py:699`, `:1200`, `:1252–1254`.

| Sítio indireto | Universo/EC e origem | Quem chama e efeito | Testes | [nao verificado]
|---|---|---|---|
| `app/chamados/models.py:678` `_gate_premissa_do_fechamento` | Pessoa e dia do fato pelo juiz `:690–692`; chama `premissa_regularizada(para='folha')` `:699`. Não escolhe EC nem lê `calendario_origem`. | `_decisao_do_fechamento(:646)`, usada por `reconciliar(:615)` e `reconciliaria(:625)`. Regularizada ou agrupada deixa fechar `:699–700`; dedup com sobrevivente também `:709–722`; caso contrário recusa `:723–732`. | `app/chamados/tests/test_reconciliar_pergunta_a_premissa.py:70`, `:80`, `:95`, `:130`, `:143`; folga: `:196`, `:212`; trabalho com furo recusa: `:217`. |
| `app/chamados/models.py:1200` `premissa_regularizada` | Primeiro célula/ata atual da pessoa/data `:1227–1228`; `dia_sem_trabalho` regulariza chamado que não é turno aberto `:1232–1233`. Depois `_celula_furada(:1247)`; só se resposta indeterminada chama `espurios(:1253)`. Origem FolgaDia não é insumo do ramo célula; portanto não verifica se a célula veio de calendário alheio `[nao verificado]` quanto à origem efetiva. | Gate acima `:699`; reabertura também consome o coletor (contrato descrito em `:1203–1208`, implementação em `recusa_da_reabertura` a partir de `:1266`). Move regularização, fechamento/reabertura de cobrança de furo. | `test_reconciliar_pergunta_a_premissa.py:212` célula folga com ata velha apagada fecha; `:244` gate e reabertura concordam. `app/chamados/tests/test_f1c4_3_none_cobra.py:105` None não regulariza; `:111` ata folga regulariza. |
| `app/chamados/juizes.py:398` `dia_sem_trabalho`; `:411` `_celula_furada` | Lê `trabalha is False` ou `ata.tipo_dia` preenchido diferente de trabalho `:407–408`. `_celula_furada` trabalha em pessoa/data `:417`, ausência `:426`, célula `:436`; dia sem trabalho recursa à véspera com mesmo `para` `:443–449`. EC só é consultado para confiabilidade de órfãs `:464–465`; não lê FolgaDia/origem. | `premissa_regularizada` em `models.py:1232`, `:1247`. Move cobrança/regularização do dia/turno. | `test_f1c4_3_none_cobra.py:39`, `:42`, `:45`, `:50`, `:93`, `:97`, `:101`, `:117`, `:133`. |
| `app/chamados/services/feriado_reavaliar.py:23` `espurios` | Pessoa e dia do chamado por `dia_do_fato(:30)`; chama `fatos_do_dia(:33)`. Herda EC e regra de origem desse juiz. Não aceita candidato de calendário alheio se o juiz o descartar; ainda exige `era_dia_previsto != None(:34–35)` e faixa feriado/folga `:36`. | `models.py:1253`; `reavaliar_dias(:82)`; `app/ponto/management/commands/reavaliar_ausencias_feriado.py:24`. `premissa_morta(:38)` seleciona falta espúria; `encerrar(:43)` chama `reconciliar(:47)`. Move retirada de cobrança/chamado. Evento feriado: `app/chamados/signals.py:274`; evento da porta de célula: `app/ponto/portas/celula.py:431`; cron `app/config/crons.py:589`. | `app/chamados/tests/test_varredura_feriado_evento.py:60` evento encerra; `:70` outro dia não muda; `:78` idempotência; `:87` cron e receiver mesma porta; `:94` cron registrado. Teste específico de FolgaDia/calendário alheio nesse serviço: `[nao verificado]`. |

Nenhum teste foi executado por este agente; resultados de execução são `[nao verificado]`. [nao verificado]

## 31/10 19:39 · sessao cf3a0f71ee37 · - · - · origem=Ronald (tmux)

O teste específico do intervalo **inteiramente depois das 05:00** existe, mas exige a correção; não preserva o resultado defeituoso como expectativa: `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:4` declara vermelho de propósito e sem `expectedFailure`; o cenário está em `:40-47`: período 22:00–07:00, intervalo 05:30–06:30, prorrogação `True`, hora reduzida afastada.

Seus três testes são: [nao verificado]

- `test_P5_HE100_noturna_pela_regra`: HE100 = 8 h, HE100 noturna = 8 h — `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:49-54`.
- `test_P5_PROPRIEDADE_parte_nao_passa_do_todo`: HE100 noturna ≤ HE100 — `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:56-59`.
- `test_P5_noturnas_pela_regra`: horas noturnas = 8 h — `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:61-63`.

O docstring registra 9 h no código atual para ambas as parcelas e atribui o erro ao desconto do intervalo isolado — `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:17-24`. Execução nesta auditoria: [nao verificado].

Testes relacionados, com pausa **atravessando** as 05:00: [nao verificado]

- C3 fixa 22:00–07:00, intervalo 04:30–05:30, prorrogação `True` e hora normal: `horas_noturnas = 8` — `app/ponto/tests/test_v3_periodos_da_celula.py:123-136`.
- A porta deve entregar o período inteiro e os intervalos, inclusive dentro de `he_pontas` — `app/ponto/tests/test_v3_periodos_da_celula.py:138-148`.
- F7f fixa o comportamento geométrico de dois pares, 390 minutos noturnos de relógio e 445,71 legais — `app/ponto/tests/test_o280_bateria_da_lei.py:1562-1592`.
- F7g está em `@unittest.expectedFailure` e pede um par, 480/548,57 — `app/ponto/tests/test_o280_bateria_da_lei.py:1594-1616`.
- v3 F7f compara expressamente par inteiro = 480 com segmentos = 390 — `app/ponto/tests/test_o280_bateria_da_lei.py:4132-4171`; asserção efetiva em `:4168-4171`.

Outras guardas não cobrem o intervalo inteiramente pós05:

- Desconto de pausa noturna com prorrogação desligada — `app/ponto/tests/test_noturno_desconta_intervalo.py:41-61`, `:123-126`, `:170-200`.
- HE noturna recua por trabalho e desconta pausa 23:30–00:30 — `app/ponto/tests/test_noturno_desconta_intervalo.py:245-257`; outra pausa 23:00–23:30 em `:259-269`.
- Testa prorrogação em turno inteiro, sem pausa — `app/ponto/tests/test_s5b_regra_noturnas.py:86-93`.
- Testa desconto de pausa 00:00–01:00 — `app/ponto/tests/test_s5b_regra_noturnas.py:75-84`.
- Grade de equivalência de `he_noturna` inclui atravessar05, mas sem intervalos passados — `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:90-115`.
- Selo estrutural verifica encanamento de intervalos, sem caso temporal pós05 — `app/ponto/tests/test_noturno_sitio_unico.py:130-155`.

A busca textual em todos os `.py`, inclusive ocultos, encontrou chamadas diretas à função alvo apenas em `app/ponto/motor_calculo_v2.py:392` e `:1865`; nos testes, as ocorrências do nome são docstring/mensagem de P5 (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:18`, `:54`), não import nem chamada direta. Nenhum apelido de import da própria função apareceu nessa busca; reflexão/import dinâmico fora dessas ocorrências: [nao verificado].

Também existe o wrapper `noturno_no_par`, que passa o par inteiro à porta comum com os intervalos e a CCT — `app/ponto/calculador/periodo.py:487`, `:508`; convém incluí-lo no item 3 do censo.

## 31/10 19:04 · sessao 6638e332e36a · GRADE-FOLGA-DO-VINCULO · censo · origem=Code

As referências abreviadas abaixo pertencem a **`obra_qr_triagem.txt`, blocos `logs/grade_folga/…`**. A numeração começa na primeira linha após cada separador `=====` e inclui linhas em branco.

**O item 8 não cabe nas cinco classes propostas:** o Codex viu os dois leitores, mas não há descarte documentado pela triagem. Forçar uma classe apagaria essa diferença. (`censo.r:85–86`; descarte: [nao verificado])

1. **Pré-requisito de ensaio — (c), quanto à comprovação material. O requisito foi visto.**

   **Pergunta:** “pouso antes do pre-requisito” (`v5/pv/v1.q:8`).

   **Resposta:** “**Não daria o pouso como comprovado:** a v3 já está declarada, mas exige antes ensaio `--apply` com dias rejulgados, dias que movem minuto e `FORA-DO-HORIZONTE 0`.” (`v3.r:27`). [nao verificado]

   A triagem registrou: “ensaio --apply v3 (rejulgados, minutos, FORA-DO-HORIZONTE 0) antes do pouso”. Portanto, houve alerta e reconhecimento; a suficiência do ensaio posterior depende da evidência da sombra. (`triagem_v3.txt:5`; `opus_final.txt:2`) [nao verificado]

2. **Reversão, prova de frota e hashes FM/TXT — (c).**

   **Pergunta:** “FechamentoMensal e TXT ficam intactos por construcao ou so no teste?” (`v5/pv/v1.q:5`).

   **Resposta:** “**FechamentoMensal e TXT ficam fora das escritas diretas desse caminho por construção**, com testemunha adicional no teste”, seguida da ressalva: “Isso não constitui imunidade universal”. (`v5/pv/v1.r:46`).

   A resposta também apontou que o teste observava menos que o registro inteiro. Isso é análise do caminho e da fixture; não comprova reversão em `logs/`, execução na frota ou hashes reais da 09. Esses são os artefatos ausentes relatados pelo opus. (`v5/pv/v1.r:48`; `opus_final.txt:3`; comprovação independente: [nao verificado])

3. **`delta_chamados=0` antigo não certifica a alteração posterior — (c).**

   **Pergunta:** “Quem le `ata['tipo_dia']` gravada na CelulaDia, e para que?” (`censo.q:6`).

   **Resposta:** `dia_sem_trabalho` “Retorna verdadeiro se `cel.trabalha is False` **ou** se a ata tem tipo preenchido diferente de `'trabalho'`.” (`censo.r:99`).

   Portanto, o consumidor foi visto; não cabe atribuir ao Codex desconhecimento desse elo. A validade do delta depois de `836c57b7` exige nova comparação material, que não é demonstrada por essa leitura estática. (`censo.r:99`; `opus_final.txt:4`; delta posterior: [nao verificado])

4. **“Sem `--forcar` não rejulga a 09” — (b), alerta conhecido afastado pela decisão posterior.**

   **Pergunta:** “E sem `--forcar` (o cron de prod), em que condicoes um dia de competencia 2026-09 e rejulgado (impressao muda? quais insumos compoem a impressao)?” (`v4_fora/q1.q:5`).

   **Resposta:** “**Sim, pelo componente `cobparcial`, mesmo mantendo `regra:1` e o mesmo `tipo_dia` final.**” E: “O cartório incorpora esse estado como `cobparcial:1:<estado>`, sem filtro por versão.” (`v4_fora/q1.r:48,53`).

   **Triagem e motivo:** “o pouso nao rejulga a 09 (horizonte 2026-10, provado por mutante)”. A decisão tomou o mutante como prova geral apesar do mecanismo anteriormente identificado. As sete células concretas são dado do opus, não reprodução do Codex. (`v5/pv/triagem_v1.txt:3`; `opus_final.txt:5`)

5. **Pauta DP da 09 — (b), explicitamente adiada.**

   **Pergunta:** “Quem gera a Pauta DP da diferenca pago x devido e onde?” (`v5/pv/v1.q:5`).

   **Resposta:** “Não encontrei chamada correspondente no rejulgamento desta cura; a declaração da v3 apenas promete a Pauta.” (`v5/pv/v1.r:50`).

   **Triagem e motivo:** “DECIDIDO PELA LEI (3, Pauta DP nao encadeada)”, invocando “a Pauta DP da 09 e ato proprio, depois”, e destinando a `BACKLOG GRADE-09-PAUTA`. Logo, a lacuna foi apontada e conscientemente adiada. (`v5/pv/triagem_v1.txt:3`)

6. **`_folga_avulsa` mantém outra regra de validade — (b), literalmente FILA.**

   **Pergunta:** “Algum outro sitio decide "esta FolgaDia vale neste dia" com regra diferente da de folga_lancada_vale_no_dia e alimenta a mesma lavra/ata/chamado?” (`v1.q:11`).

   **Resposta:** “o veto consulta **qualquer FolgaDia da pessoa/data**, sem validar proveniência/vínculo”, permitindo que calendário antigo rejeitado pelo predicado ainda impeça lançamento. (`v1.r:109`).

   **Triagem e motivo:** “veto de _folga_avulsa […] -> FILA: respondem "pode lancar?", nao a lavra”. O mesmo destino foi repetido na volta 2. (`triagem_v1.txt:7`; `triagem_v2.txt:5`)

7. **`_folga_do_dia` ignora manual do vínculo antigo — (b), literalmente FILA.**

   **Pergunta:** “Algum outro sitio decide "esta FolgaDia vale neste dia" com regra diferente da de folga_lancada_vale_no_dia e alimenta a mesma lavra/ata/chamado?” (`v1.q:11`).

   **Resposta:** “Uma manual presa ao velho vale pelo predicado proposto, mas não é reconhecida ali. Esse booleano decide o caminho que carimba/fecha a pergunta e o fio.” (`v1.r:107`).

   **Triagem e motivo:** `declarar_dia.o_dia_ja_e_folga` foi enviado à FILA porque responderia “pode lancar?”, “nao a lavra”. Portanto, o Codex apontou inclusive a consequência sobre chamado; a triagem afastou o bloqueio. (`triagem_v1.txt:7`; `triagem_v2.txt:5`)

8. **Leitores com regra velha — sem encaixe estrito nas classes.**

   **Pergunta:** “Que outros sitios do codigo decidem se uma `FolgaDia` vale como folga de um dia”; a pergunta exige listar cada sítio e seu tratamento de calendário e manual de outro vínculo. (`censo.q:5`).

   **Resposta sobre `fase_conflitante`:** “Lê fotos de cada vínculo ativo, sem filtro de origem/vigência por data”. (`censo.r:85`).

   **Resposta sobre `rejulgar_ausencia_em_folga`:** “Seleciona pares pessoa/data por foto da empresa/janela, sem distinguir origem ou vínculo diário.” (`censo.r:86`).

   Assim, **o Codex viu ambos**. Descarte pela triagem: **[nao verificado]**. A situação documentada é “visto no censo, sem destino demonstrado”; classificá-la como (a), (d) ou (e) negaria os trechos acima, e classificá-la como (b) inventaria uma decisão. (`censo.q:5`; `censo.r:85–86`)

9. **Predicado novo sem juiz/corte e completude indevida — (a), quanto à falha estrutural específica.**

   **Pergunta:** “com JUIZES.md anexo. Qual e o juiz unico de "este dia e folga?" e quantos sitios respondem essa pergunta depois do diff?” (`v5/pv/v1.q:3`).

   **Resposta:** “**Um predicado:** `folga_lancada_vale_no_dia`”; “**Um seletor**”; “**Dois sítios consumidores:** precedência e overlay da grade.” (`v5/pv/v1.r:26–28`).

   A resposta apresentou essa centralização sem apontar a falta específica de registro/corte cobrada pelo opus. O tema autoridade/juiz estava expressamente perguntado. Os resíduos 6 e 7, entretanto, já haviam sido vistos e enfileirados; essa parte do item composto não é descoberta nova. (`v5/pv/v1.q:3`; `v5/pv/v1.r:26–28`; `triagem_v2.txt:5`; `opus_final.txt:10`)

   A correspondência da LEI-AKITA ao commit `362c6d8a`: **[nao verificado]**.

10. **`red_aval.log` era verde; RED real em `red0_aval.log` — (c).** [nao verificado]

    **Pergunta:** “Os RED mordem? Que mutante simples […] cada teste novo deixaria passar?” (`v5/pv/v1.q:6`).

    **Resposta:** “A tabela descreve **mordida prevista por inspeção**, não execução de mutantes. Resultado RED/GREEN: **[não verificado]**.” (`v5/pv/v1.r:56`).

    A resposta delimitou a inspeção; não certificou os arquivos de execução. A troca concreta dos rótulos é informação do opus, cuja conferência independente permanece **[nao verificado]**. (`v5/pv/v1.r:56`; `opus_final.txt:11`)

Contagem das atribuições acima:

| Classe | Itens | Total |
|---|---|---:|
| **(a)** | 9, na falha estrutural específica | **1** |
| **(b)** | 4, 5, 6, 7 | **4** |
| **(c)** | 1, 2, 3, 10, na comprovação material | **4** |
| **(d)** | Nenhum identificado | **0** |
| **(e)** | Nenhum item integral identificado | **0** |
| **Sem encaixe nas definições** | 8: visto, sem descarte demonstrado | **1** |

A exceção que impede fechar os dez nas cinco classes está documentada no próprio censo. (`censo.r:85–86`; descarte correspondente: [nao verificado])

_1218 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
