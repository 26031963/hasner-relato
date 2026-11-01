# CODEX-ESTADO

> Ultima resposta processada: **01/11/2026 03:30** · publicado em **01/11/2026 03:32** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 01/11 03:30 · sessao 49226582207f · - · - · origem=Ronald (tmux)

Censo estático da cópia disponível. Correspondência com o commit Git `40988459`: **[nao verificado]**, pois não há metadados Git disponíveis.

**1. Chamadores e origem dos argumentos**

Encontrei **um chamador direto em produção**: `rubricas_por_dia`, que importa `regras` e chama `regras.do_dia`. (`app/ponto/calculador/alimentacao.py:367`, `app/ponto/calculador/alimentacao.py:501`)

| Argumento | Valor passado e origem |
|---|---|
| `feriados` | `_feriados`, construído com `set(feriados or ())` a partir do argumento da porta. (`app/ponto/calculador/alimentacao.py:396`, `app/ponto/calculador/alimentacao.py:509`) |
| `feriado_em_dobra` | `_fer_dobra`: `_cct.feriado_em_dobra`, ou `None` quando não há CCT lida. (`app/ponto/calculador/alimentacao.py:394`, `app/ponto/calculador/alimentacao.py:509`) |
| `prorrogacao_pos5h` | `_cct_pro`: `_cct.prorrogacao_pos5h`, ou `None`. (`app/ponto/calculador/alimentacao.py:392`, `app/ponto/calculador/alimentacao.py:515`) |
| `hora_reduzida_afastada` | `_cct_hr`: `_cct.hora_reduzida_afastada`, ou `None`. (`app/ponto/calculador/alimentacao.py:393`, `app/ponto/calculador/alimentacao.py:515`) |
| `he_pontas` | `_he_pontas.get(k)`, sendo `_he_pontas = insumos.get('he_pontas_por_dia') or {}`. (`app/ponto/calculador/alimentacao.py:444`, `app/ponto/calculador/alimentacao.py:513`) |
| `pares` | `_pares_por_dia.get(k)`, vindo de `insumos['pares_por_dia']`. (`app/ponto/calculador/alimentacao.py:438`, `app/ponto/calculador/alimentacao.py:512`) |
| `pontualidade` | `_pont`: cópia da lista diária de `pont_por_dia`, ou `None`; na primeira tupla, substitui trabalhados pelo total diário do núcleo e fora da janela pelo total diário correspondente. (`app/ponto/calculador/alimentacao.py:439`, `app/ponto/calculador/alimentacao.py:496–500`, `app/ponto/calculador/alimentacao.py:518`) |

A cadeia dos flags é `get_motor_cct(c, ini, tipo_escala=te)` → `cct_do_motor(_mcct)` → três campos da CCT. Exceção na fábrica deixa `_mcct=None`; `cct_do_motor` também devolve `None` se qualquer um dos quatro atributos exigidos estiver ausente ou nulo. Essa leitura acontece antes do laço diário. (`app/ponto/calculador/alimentacao.py:378–394`, `app/ponto/calculador/nucleo.py:317`, `app/ponto/calculador/nucleo.py:344–351`)

Na fábrica, com a régua ativa, a prorrogação vem de `regua_para`; o afastamento da hora reduzida exige a cláusula cadastrada, ciclo `12x36` e trava desativada; a dobra pode ser explicitamente ligada pela cláusula de feriado do `12x36`. Sem valor explícito de dobra, `get_motor` resolve pelo ciclo, usando `CICLOS_FERIADO_SIMPLES={'12x36'}`. (`app/core/regua_cct.py:458–479`, `app/ponto/motor_calculo_v2.py:3587`, `app/ponto/motor_calculo_v2.py:3657–3668`)

`insumos_do_calculador` monta os três mapas: segmentos do motor, marcos e pontualidade. Os pares usam `efetivo_entrada/efetivo_saida`, separados pelos intervalos descontados; a pontualidade usa entradas/saídas cruas em horário local e marcos do vínculo, ancorados no dia da jornada. (`app/ponto/calculador/alimentacao.py:302–304`, `app/ponto/calculador/alimentacao.py:73`, `app/ponto/calculador/alimentacao.py:115–146`, `app/ponto/calculador/alimentacao.py:184–189`, `app/ponto/calculador/alimentacao.py:251–289`)

Há dois chamadores de produção dessa porta:

- **Fechamento:** monta insumos e chama `rubricas_por_dia`; os feriados vêm de `feriados_de(lugar_do_colab(colab), _env_ini, _env_fim, carregados=_feriados_lote)`. (`app/ponto/services/fechamento.py:621–623`, `app/ponto/services/fechamento.py:1237–1244`)
- **DIFF:** chama a mesma porta com `aut.resultado`; passa `set(aut.feriados or ())`. A autoridade obtém esse calendário por `feriados_do_colab`, que delega a `feriados_de`. (`app/ponto/management/commands/diff_calculador.py:361`, `app/ponto/management/commands/diff_calculador.py:417–424`, `app/ponto/services/espelho.py:1034`, `app/ponto/services/espelho.py:146–147`)

**2. Fluxo de `do_dia`, de `_dobra` até o retorno**

Antes desse trecho, `valores` é inicializado com **`0.0` para toda rubrica que não esteja em `_nd`**. As cinco rubricas perguntadas pertencem a `RUBRICAS`. (`app/ponto/calculador/regras.py:420–421`, `app/ponto/calculador/regras.py:641`)

As condições iniciais dessas cinco são:

| Rubrica | Quando entra em `nao_decide`, ficando ausente de `valores` |
|---|---|
| `horas_noturnas` | `pares` vazio/ausente **ou** qualquer um dos flags `prorrogacao_pos5h` e `hora_reduzida_afastada` igual a `None`. (`app/ponto/calculador/regras.py:609–611`) |
| Ambas as HE noturnas | `he_pontas is None` **ou** `prorrogacao_pos5h is None`. Lista `[]` não aciona essa condição. (`app/ponto/calculador/regras.py:627–632`) |
| `horas_atraso` e `horas_saida_antecipada` | `pontualidade_fora_por_lei` verdadeiro: recebem essa causa e `pontualidade` vira `None`; senão, `pontualidade is None`: recebem `SEM_ENTRADA`. (`app/ponto/calculador/regras.py:618–625`) |

O fluxo posterior é:

1. `_dobra` começa em `0.0`. Só chama `dobra_de_feriado_do_dia` se calendário, flag de dobra e `dia` não forem `None`; registra `dobra_feriado_min`. O helper devolve o trabalhado integral quando positivo e o predicado do motor é verdadeiro; caso contrário, zero. (`app/ponto/calculador/regras.py:774–778`, `app/ponto/calculador/regras.py:344–347`)

2. **Se `_dobra > 0`:**
   - Escreve `horas_extras_100 = _dobra/60`, a parcela de feriado quando não declarada, `horas_extras_50 = 0.0`, os detalhes da substituição e **retorna imediatamente**. (`app/ponto/calculador/regras.py:779–789`)
   - Portanto, as cinco rubricas perguntadas conservam exatamente a inicialização: **`0.0` em `valores` quando seus insumos passaram pelas guardas; ausentes de `valores` e com causa em `nao_decide` quando não passaram**. Nenhuma das cinco é calculada nesse ramo. (`app/ponto/calculador/regras.py:609–641`, `app/ponto/calculador/regras.py:779–789`)

3. **Se `_dobra == 0`:**
   - Se `extra > 0`, escreve HE50 como `min(extra,120)/60`; escreve HE100 como `max(extra-120,0)/60` somente se essa rubrica não estiver em `_nd`. Registra os detalhes do excedente; sem extra positivo, esses valores conservam a inicialização. (`app/ponto/calculador/regras.py:790–808`)
   - Se `he_pontas` e prorrogação não forem `None`, chama `he_noturna` para cada `(entrada, saída, x50, x100)`, soma os resultados e escreve cada split dividido por 60. Com `he_pontas=[]`, escreve ambos como zero. Sem os insumos, conserva as causas iniciais. O flag de hora reduzida não participa dessa guarda. (`app/ponto/calculador/regras.py:815–831`)
   - Se existem pares e ambos os flags noturnos, escreve `horas_noturnas = _legais/60` e os detalhes relógio/legais. `_legais` é o noturno dos pares válidos, descontados os intervalos, limitado inferiormente a zero, multiplicado por `1.0` ou `FATOR_HORA_NOTURNA`. Sem esses insumos, conserva `nao_decide`. (`app/ponto/calculador/regras.py:80–89`, `app/ponto/calculador/regras.py:835–842`)
   - Se `pontualidade` e `dia` não forem `None`, chama `pontualidade_do_dia` e escreve atraso/antecipada, divididos por 60; registra detalhes e eventual `cadastro_x_realidade`. (`app/ponto/calculador/regras.py:846–857`)
   - Se existe `pontualidade`, mas falta `dia`, declara ambas em `nao_decide` com a causa específica de ausência de `dia=` e remove os zeros de `valores`. Se `pontualidade=None`, conserva as causas iniciais. (`app/ponto/calculador/regras.py:618–625`, `app/ponto/calculador/regras.py:858–863`)
   - Retorna `Rubricas(val, _nd, det)`. (`app/ponto/calculador/regras.py:865`)

**3. Motor v2 no feriado em dobra**

O predicado executável é `bool(self.feriado_em_dobra) and self.eh_feriado(dia)`; não contém teste de “dia previsto na escala”. (`app/ponto/motor_calculo_v2.py:929`)

Os **quatro sítios de dobra** têm estes comportamentos:

| Sítio | Noturnas ordinárias | Split de HE noturna |
|---|---|---|
| `Motor12x36.calcular_periodo` | Calcula antes da dobra; o ramo coloca a duração integral em HE100 sem alterar as noturnas. (`app/ponto/motor_calculo_v2.py:2780`, `app/ponto/motor_calculo_v2.py:2797–2805`) | Executa `_aplicar_he_noturna` depois do ramo, inclusive na dobra. (`app/ponto/motor_calculo_v2.py:2843`) |
| `MotorTurnoPartido.calcular_mes` | Os períodos já calcularam noturnas; a reinicialização mensal não as apaga. (`app/ponto/motor_calculo_v2.py:2884`, `app/ponto/motor_calculo_v2.py:2921–2927`) | Zera ambos os splits; atribui a HE100 integral ao último período e dá `continue`, pulando `_aplicar_he_noturna`. **Ambos ficam zero nesse caminho mensal.** (`app/ponto/motor_calculo_v2.py:2925–2926`, `app/ponto/motor_calculo_v2.py:3080–3089`, `app/ponto/motor_calculo_v2.py:3135`) |
| `MotorComercial.calcular_periodo` | Calcula antes da dobra, que não altera esses campos. (`app/ponto/motor_calculo_v2.py:3235`, `app/ponto/motor_calculo_v2.py:3243–3251`) | Executa `_aplicar_he_noturna` depois do ramo. (`app/ponto/motor_calculo_v2.py:3269`) |
| `MotorComercial.calcular_mes` | Preserva as noturnas ordinárias dos períodos; reinicializa extras e seus splits. (`app/ponto/motor_calculo_v2.py:3304–3313`) | Apaga os splits individuais; atribui HE100 integral ao último período e dá `continue`, pulando a nova aplicação. **Ambos ficam zero nesse caminho mensal.** (`app/ponto/motor_calculo_v2.py:3311–3312`, `app/ponto/motor_calculo_v2.py:3325–3340`, `app/ponto/motor_calculo_v2.py:3387`) |

Quando chamado, `_aplicar_he_noturna` entrega a `he_noturna` as pontas cruas do período, x50/x100, prorrogação e intervalos. A função mede o trecho extra no fim, desconta intervalos e reparte o noturno na proporção x50/x100; não aplica fator de hora reduzida. (`app/ponto/motor_calculo_v2.py:1000–1010`, `app/ponto/motor_calculo_v2.py:430–451`)

**Atraso/saída antecipada:** nenhum dos quatro ramos de dobra escreve ou zera esses campos. O comercial julga pontualidade após a HE noturna; o `12x36ComEscala` julga após o cálculo herdado; o partido julga antes do laço de HE/dobra. (`app/ponto/motor_calculo_v2.py:2797–2805`, `app/ponto/motor_calculo_v2.py:3080–3089`, `app/ponto/motor_calculo_v2.py:3243–3251`, `app/ponto/motor_calculo_v2.py:3325–3340`, `app/ponto/motor_calculo_v2.py:3281`, `app/ponto/motor_calculo_v2.py:3510–3521`, `app/ponto/motor_calculo_v2.py:3065–3068`)

A pontualidade ainda passa pelo teto diário: guardas de ausência inteira ou dia aberto/nulo podem zerar; com previsto conhecido, a soma fica limitada a `max(0, previsto − trabalhado_real)`, com rateio proporcional. (`app/ponto/motor_calculo_v2.py:1080–1093`, `app/ponto/motor_calculo_v2.py:1144–1184`)

Existe também um bloqueio separado da dobra: `_dia_sem_previsto_fora_da_janela` pode zerar trabalho, noturnas e HE/splits em folga/feriado sem previsto e sem autorização, conforme suas demais guardas. Portanto, a preservação das noturnas descrita na tabela é o comportamento **dos ramos de dobra**, não uma garantia contra esse bloqueio posterior. (`app/ponto/motor_calculo_v2.py:2446–2449`, `app/ponto/motor_calculo_v2.py:1436–1471`)

**4. Leitores das cinco rubricas no resultado de `do_dia`**

Em produção:

- **`dia_pago._DONOS_DECLARADOS`:** lê as chaves de `valores` e `nao_decide`; as cinco estão em `CAMPOS_DIA`. (`app/ponto/services/dia_pago.py:34–37`, `app/ponto/services/dia_pago.py:61–63`)
- **`dia_pago.lavrar`:** lê os valores e substitui o campo da linha diária, salvo proveniência explícita em `do_motor`; lê `nao_decide` para registrar `calculador_nao_decide`. O fechamento entrega a saída da porta em `rubricas=_rub_o`. (`app/ponto/services/dia_pago.py:320–331`, `app/ponto/services/fechamento.py:1239–1244`, `app/ponto/services/fechamento.py:1256`)
- **`diff_calculador`:** lê atraso e antecipada explicitamente para impressão; guarda o mapa completo, lê `nao_decide`, soma os valores e compara cada rubrica não declarada cega. O conjunto comparado inclui as rubricas fora do `NAO_DECIDE` do módulo. (`app/ponto/management/commands/diff_calculador.py:78`, `app/ponto/management/commands/diff_calculador.py:476–477`, `app/ponto/management/commands/diff_calculador.py:501`, `app/ponto/management/commands/diff_calculador.py:517–542`)

Em testes, encontrei estas leituras das cinco rubricas: [nao verificado]

| Arquivo | Leituras |
|---|---|
| `app/ponto/tests/test_calculador_regras.py` | Presença/ausência e causas: `:208–219`. |
| `app/ponto/tests/test_s5b_regra_pontualidade.py` | Atraso, antecipada e noturnas: `:211–253`; folga: `:400–412`; cobertura genérica: `:436–438`, `:452–454`; discriminador de pontualidade: `:485–493`, `:538–539`, `:551`, `:558`. |
| `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py` | HE noturna ausente/declarada: `:288–290`; valores: `:319–320`. |
| `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py` | HE50 noturna e noturnas: `:69–82`. |
| `app/ponto/tests/test_lote_de_celulas_nao_troca_de_colaborador.py` | Atraso: `:154–159`. |
| `app/ponto/tests/test_o280_bateria_da_lei.py` | Declaração de atraso/antecipada: `:2780–2781`. |

**5. O que os x50/x100 de `he_pontas` representam**

São lidos diretamente de `p.minutos_extra_50` e `p.minutos_extra_100`, com as pontas efetivas do período. Não são recalculados pelo excedente de `do_dia`. Dia visitado sem HE recebe `[]`; se existe x50 ou x100, acrescenta `(e,s,x50,x100)`. (`app/ponto/calculador/alimentacao.py:73`, `app/ponto/calculador/alimentacao.py:91–96`)

**Quando o motor aplicou a dobra, esses números já refletem a dobra:** nos cálculos individuais, x100 recebe a duração integral; nos consolidados de partido/comercial, as extras dos períodos são zeradas e o último recebe todo o trabalhado do dia em x100. Assim, nos consolidados, a tupla do último período pode carregar a HE100 do dia inteiro; não há distribuição da dobra por cada período. (`app/ponto/motor_calculo_v2.py:2797–2800`, `app/ponto/motor_calculo_v2.py:3243–3246`, `app/ponto/motor_calculo_v2.py:2921–2926`, `app/ponto/motor_calculo_v2.py:3080–3089`, `app/ponto/motor_calculo_v2.py:3307–3312`, `app/ponto/motor_calculo_v2.py:3325–3340`)

Essa origem é comum ao fechamento e ao DIFF, pois ambos constroem `insumos_do_calculador` e chamam a mesma porta. (`app/ponto/services/fechamento.py:1237–1244`, `app/ponto/management/commands/diff_calculador.py:311–312`, `app/ponto/management/commands/diff_calculador.py:417–424`)

**6. Testes de `do_dia` com feriado em dobra** [nao verificado]

Encontrei estes casos que alcançam o ramo da dobra:

- **`test_MORDE_no_do_dia_a_dobra_SUBSTITUI_a_HE50_e_nao_soma`:** passa feriado e dobra explicitamente; afirma HE100=10h, HE50=0 e detalhe de substituição. Não afirma noturnas nem HE noturna. (`app/ponto/tests/test_s5b_regra_pontualidade.py:309–318`)
- **`test_MORDE_feriado_em_dobra_x_excedente_dao_METADES_DIFERENTES`:** afirma HE100 positiva, parcela de feriado igual à HE100 e distinção do excedente normal. Não afirma noturnas nem HE noturna. (`app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:207–233`)
- **`test_C3b_flag_da_CCT_AUSENTE_nao_vira_o_default_do_leitor`:** alcança `do_dia` pela porta, injeta feriado e liga a dobra; verifica HE100, parcela de feriado e declaração quando falta cadastro. O resultado lido pelo teste não inclui noturnas ou HE noturna. (`app/ponto/tests/test_o280_bateria_da_lei.py:2858–2902`)

O teste de **folga/feriado** é outro caminho: passa `folga_e_feriado`, afirma folga=0 e HE100-feriado=8h, mas retorna antes de chegar a `_dobra`. (`app/ponto/tests/test_bob_b2_abono_feriado_guarda.py:225–233`, `app/ponto/calculador/regras.py:735`)

**Não encontrei asserção de `horas_noturnas` ou HE noturna numa execução de `_dobra > 0`.** As asserções encontradas de split usam dia comum; o P5 também executa dia comum e apenas documenta, em comentário, o retorno precoce da dobra. (`app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:312–320`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:25–28`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:61–82`)

## 01/11 03:29 · sessao eea9e4aa971d · - · - · origem=Ronald (tmux)

Leitores do resultado de `do_dia` em produção:

- `app/ponto/services/dia_pago.py:61-63`: `_DONOS_DECLARADOS` lê as chaves de `Rubricas.valores` e `nao_decide`; as cinco rubricas pertencem a `CAMPOS_DIA` em `app/ponto/services/dia_pago.py:34-37`.
- `app/ponto/services/dia_pago.py:320-328`: `lavrar` lê os valores e os copia para a linha do dia; lê `nao_decide` para registrar o detalhe em `app/ponto/services/dia_pago.py:329-331`. A ligação com a saída real da porta está em `app/ponto/services/fechamento.py:1239-1244` e `app/ponto/services/fechamento.py:1256`.
- `app/ponto/management/commands/diff_calculador.py:476-477`: leitura explícita de saída antecipada e atraso. O comando guarda o mapa de valores em `app/ponto/management/commands/diff_calculador.py:501`, lê as rubricas cegas em `app/ponto/management/commands/diff_calculador.py:517`, soma valores em `app/ponto/management/commands/diff_calculador.py:519-520` e cegueira em `app/ponto/management/commands/diff_calculador.py:525-526`; compara valores somente das rubricas não cegas em `app/ponto/management/commands/diff_calculador.py:527-542`. A seleção inclui todas as rubricas do calculador fora de `NAO_DECIDE`: `app/ponto/management/commands/diff_calculador.py:78`.

Leitores em testes das cinco rubricas: [nao verificado]

- `test_calculador_regras.py`: ausência em `valores` e presença/causa em `nao_decide`: `app/ponto/tests/test_calculador_regras.py:208-219`.
- `test_s5b_regra_pontualidade.py`: atraso, antecipada e noturnas em `app/ponto/tests/test_s5b_regra_pontualidade.py:211-253`; noturnas de folga em `app/ponto/tests/test_s5b_regra_pontualidade.py:411-412`; atraso/antecipada de folga em `app/ponto/tests/test_s5b_regra_pontualidade.py:400-402`; cobertura de todas as rubricas em `app/ponto/tests/test_s5b_regra_pontualidade.py:436-438` e `app/ponto/tests/test_s5b_regra_pontualidade.py:452-454`; testes do discriminador de pontualidade em `app/ponto/tests/test_s5b_regra_pontualidade.py:485-493`, `app/ponto/tests/test_s5b_regra_pontualidade.py:538-539`, `app/ponto/tests/test_s5b_regra_pontualidade.py:551` e `app/ponto/tests/test_s5b_regra_pontualidade.py:558`.
- HE noturna ausente/cega em `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:288-290`; HE noturna com valor em `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:319-320`.
- HE50 noturna e noturnas do P5 em `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:69-82`.
- Atraso em `app/ponto/tests/test_lote_de_celulas_nao_troca_de_colaborador.py:154-159`; cegueira de atraso/antecipada em `app/ponto/tests/test_o280_bateria_da_lei.py:2780-2781`.

Testes que chegam ao feriado em dobra: [nao verificado]

1. `test_MORDE_no_do_dia_a_dobra_SUBSTITUI_a_HE50_e_nao_soma`: chamada explícita em `app/ponto/tests/test_s5b_regra_pontualidade.py:312-314`; afirma HE100 de 10h, HE50 zero e detalhe da substituição em `app/ponto/tests/test_s5b_regra_pontualidade.py:315-318`. Não afirma noturnas ou HE noturna nesse caso: `app/ponto/tests/test_s5b_regra_pontualidade.py:309-318`.
2. `test_MORDE_feriado_em_dobra_x_excedente_dao_METADES_DIFERENTES`: chamada explícita em `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:209-214`; afirma HE100 positiva, igualdade da parcela de feriado ao total e distinção do dia normal em `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:222-233`. Não afirma noturnas nesse caso: `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:207-233`.
3. Indireto pela porta, `test_C3b_flag_da_CCT_AUSENTE_nao_vira_o_default_do_leitor`: passa feriado em `app/ponto/tests/test_o280_bateria_da_lei.py:2858` e `app/ponto/tests/test_o280_bateria_da_lei.py:2863`, liga a dobra em `app/ponto/tests/test_o280_bateria_da_lei.py:2875-2876` e executa em `app/ponto/tests/test_o280_bateria_da_lei.py:2883`; verifica HE100, parcela de feriado e declaração de ausência de cadastro em `app/ponto/tests/test_o280_bateria_da_lei.py:2887-2902`. Seu retorno lido não inclui noturnas: `app/ponto/tests/test_o280_bateria_da_lei.py:2864-2873`.
4. Caso relacionado, mas do ramo anterior de folga/feriado: `app/ponto/tests/test_bob_b2_abono_feriado_guarda.py:225-233`; usa `folga_e_feriado=True`, omite `feriado_em_dobra` e afirma folga zero/HE100 feriado 8h. Não é execução do ramo `_dobra > 0`, pois o ramo de folga retorna em `app/ponto/calculador/regras.py:735`.

Não encontrei asserção sobre `horas_noturnas` ou HE noturna **no ramo `_dobra > 0`**. Os testes que afirmam essas rubricas usam calendário vazio: `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:315-320` e `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:61-82`. O P5 documenta o retorno precoce do feriado em comentário em `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:25-28`, mas executa dia comum em `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:66`.

## 01/11 03:29 · sessao 7d15870ce6a1 · - · - · origem=Ronald (tmux)

No arquivo `app/ponto/motor_calculo_v2.py`, `_feriado_dobra_no_dia(dia)` é exatamente `bool(self.feriado_em_dobra) and self.eh_feriado(dia)`; não consulta “previsto na escala”, apesar dos comentários antigos em alguns sítios. `eh_feriado` consulta conjunto injetado e fallback `is_feriado`. Referências: `app/ponto/motor_calculo_v2.py:909-929`, `app/ponto/motor_calculo_v2.py:951-955`.

**Há quatro ramos que chamam `_feriado_dobra_no_dia`:**

| Sítio | Dobra | Adicional noturno ordinário | Split HE noturna |
|---|---|---|---|
| `Motor12x36.calcular_periodo` | Se duração positiva: `minutos_extra`, `minutos_extra_100`, `minutos_extra_100_feriado = duracao`; `hora_extra_100=True`. `app/ponto/motor_calculo_v2.py:2797-2805` | Calculado antes, por `_escrever_noturnas`, e não alterado pelo ramo. `app/ponto/motor_calculo_v2.py:2780`, `app/ponto/motor_calculo_v2.py:2797-2805` | `_aplicar_he_noturna(periodo)` é executado depois do if/elif/else, inclusive dobra. `app/ponto/motor_calculo_v2.py:2843` |
| `MotorTurnoPartido.calcular_mes` | HE integral do dia no último período: `minutos_extra`, `minutos_extra_100`, `minutos_extra_100_feriado=trab_dia`; flag verdadeira; `continue`. `app/ponto/motor_calculo_v2.py:3080-3089` | Cada período calcula noturnas antes, em `calcular_periodo`, e o recálculo mensal não as zera. `app/ponto/motor_calculo_v2.py:2884`, `app/ponto/motor_calculo_v2.py:2920-2927` | Mensal zera `minutos_extra_50_noturna` e `minutos_extra_100_noturna`; dobra dá `continue` antes da chamada em `:3135`. Portanto ambos permanecem zero nos `resultado.periodos` desse dia. `app/ponto/motor_calculo_v2.py:2925-2926`, `app/ponto/motor_calculo_v2.py:3089`, `app/ponto/motor_calculo_v2.py:3135`. Caminho semanal também exclui feriados: `app/ponto/motor_calculo_v2.py:3144-3146`. |
| `MotorComercial.calcular_periodo` | Duração inteira em HE100 e HE100-feriado; flag verdadeira. `app/ponto/motor_calculo_v2.py:3243-3251` | `_escrever_noturnas` antes do ramo; ramo não altera os campos. `app/ponto/motor_calculo_v2.py:3235`, `app/ponto/motor_calculo_v2.py:3243-3251` | Chama `_aplicar_he_noturna` após o ramo. `app/ponto/motor_calculo_v2.py:3269` |
| `MotorComercial.calcular_mes` | Último período recebe `trab_dia` nas três HE; flag verdadeira; `continue`. `app/ponto/motor_calculo_v2.py:3325-3340` | Mensal zera só extras e seus splits, preservando os campos noturnos ordinários calculados no período. `app/ponto/motor_calculo_v2.py:3304-3313` | O mensal apaga o split calculado individualmente (`:3311-3312`); dobra continua (`:3340`) antes de `_aplicar_he_noturna` (`:3387`). Resultado mensal desses períodos: ambos splits zero. |

**O cálculo ordinário de noturnas e o split são distintos.** `_escrever_noturnas` grava relógio e legais a partir de `calcular_noturnas`; esta calcula janela com `prorrogacao_noturna_pos5h`, desconta intervalos e multiplica pelo fator (1 se hora reduzida afastada; caso contrário `FATOR_HORA_NOTURNA`). Referências: `app/ponto/motor_calculo_v2.py:971-980`, `app/ponto/motor_calculo_v2.py:991-993`.

`_aplicar_he_noturna` passa entrada/saída brutas do período, extras 50/100, prorrogação e intervalos do motor para `he_noturna`, gravando somente resultados não zero. Referências: `app/ponto/motor_calculo_v2.py:1000-1010`. `he_noturna` recua da saída pelo total extra em minutos trabalhados, intersecta janela noturna, desconta intervalos e reparte em proporção de 50/100; não aplica fator de hora reduzida nessa função. Referências: `app/ponto/motor_calculo_v2.py:430-451`.

**Atraso/saída antecipada:** nenhum dos quatro ramos de dobra atribui/zera esses campos: somente escreve HE, flag e alerta; nos mensais o `continue` pula HE diária, depois de a pontualidade já ter sido julgada. Referências: `app/ponto/motor_calculo_v2.py:2797-2805`, `app/ponto/motor_calculo_v2.py:3080-3089`, `app/ponto/motor_calculo_v2.py:3243-3251`, `app/ponto/motor_calculo_v2.py:3325-3340`.

- Comercial chama `_julgar_pontualidade` após o cálculo da HE noturna, inclusive se passou pela dobra: `app/ponto/motor_calculo_v2.py:3269`, `app/ponto/motor_calculo_v2.py:3281`.
- `Motor12x36ComEscala` chama o cálculo do 12x36 e depois `_julgar_pontualidade`, exceto aberto ou sem escala; não há exceção feriado-dobra: `app/ponto/motor_calculo_v2.py:3510-3521`.
- Partido julga a Zona 5 antes do laço de HE/dobra, por `aplicar_tolerancia` e grava as duas rubricas: `app/ponto/motor_calculo_v2.py:3012-3023`, `app/ponto/motor_calculo_v2.py:3061-3068`.
- `_julgar_pontualidade` retorna sem escala/marco; havendo marcos, usa `aplicar_tolerancia` e grava atraso/antecipada. Referências: `app/ponto/motor_calculo_v2.py:2030-2055`, `app/ponto/motor_calculo_v2.py:2205-2214`.
- Teto mensal geral: ausência inteira zera (`:1080-1093`), dia aberto/nulo zera (`:1144-1157`), previsto inexistente (`None`) deixa como está (`:1158-1160`); caso contrário limita soma a `max(0, previsto - trabalhado_real)` e rateia proporcionalmente (`:1161-1184`). Chamadas: base `:2706`, partido `:3170`, comercial `:3472`, todas em `app/ponto/motor_calculo_v2.py`.

**Ressalva necessária para a frase “motor mantém noturnas”:** os ramos de dobra por si mantêm, mas existe bloqueio independente depois do cálculo do período. Base chama `_dia_sem_previsto_fora_da_janela` em `app/ponto/motor_calculo_v2.py:2446-2449`. Em folga ou feriado, se empresa tem janela, previsto é zero, HE não está autorizada e passam as guardas de escala certa, o método zera trabalhadas, noturnas e HE/splits, além de colapsar segmento. Referências: `app/ponto/motor_calculo_v2.py:1436-1458`, `app/ponto/motor_calculo_v2.py:1461-1471`.

**Leitura mensal:** `horas_noturnas` soma legais de `periodos + periodos_ft`; HE noturna, atraso e antecipada somam só `periodos`, divididos por 60. Referências: `app/ponto/motor_calculo_v2.py:321`, `app/ponto/motor_calculo_v2.py:338-360`. Base classifica cada período em `periodos_ft` ou `periodos` separadamente da dobra: `app/ponto/motor_calculo_v2.py:2456-2482`.

## 01/11 03:29 · sessao 08a395e9c86d · - · - · origem=Ronald (tmux)

Chamador direto único encontrado em produção: `rubricas_por_dia`, em `app/ponto/calculador/alimentacao.py:501`. Os outros `do_dia` encontrados em produção são de módulos distintos; a importação local deste chamador é `from ponto.calculador import regras` (`app/ponto/calculador/alimentacao.py:367`).

| Argumento | Valor passado e origem |
|---|---|
| `feriados` | `_feriados` (`app/ponto/calculador/alimentacao.py:509`), construído como `set(feriados or ())` sobre argumento de `rubricas_por_dia` (`app/ponto/calculador/alimentacao.py:396`). |
| `feriado_em_dobra` | `_fer_dobra` (`app/ponto/calculador/alimentacao.py:509`), `None` se `_cct` ausente, senão `_cct.feriado_em_dobra` (`app/ponto/calculador/alimentacao.py:394`). |
| `prorrogacao_pos5h` | `_cct_pro` (`app/ponto/calculador/alimentacao.py:515`), `None` se `_cct` ausente, senão `_cct.prorrogacao_pos5h` (`app/ponto/calculador/alimentacao.py:392`). |
| `hora_reduzida_afastada` | `_cct_hr` (`app/ponto/calculador/alimentacao.py:515`), `None` se `_cct` ausente, senão `_cct.hora_reduzida_afastada` (`app/ponto/calculador/alimentacao.py:393`). |
| `pares` | `_pares_por_dia.get(k)` (`app/ponto/calculador/alimentacao.py:512`), mapa vindo de `insumos['pares_por_dia']` (`app/ponto/calculador/alimentacao.py:438`). |
| `he_pontas` | `_he_pontas.get(k)` (`app/ponto/calculador/alimentacao.py:513`), mapa vindo de `insumos.get('he_pontas_por_dia') or {}` (`app/ponto/calculador/alimentacao.py:444`). |
| `pontualidade` | `_pont` (`app/ponto/calculador/alimentacao.py:518`), cópia de `pont_por_dia[k]` ou `None`; na primeira tupla substitui trabalhados por `mins` e fora da janela pelo total diário (`app/ponto/calculador/alimentacao.py:496`, `app/ponto/calculador/alimentacao.py:499`). |

Os três flags CCT vêm de `_mcct = get_motor_cct(c, ini, tipo_escala=te)`, com exceção produzindo `_mcct=None`; depois chama `cct_do_motor(_mcct)` (`app/ponto/calculador/alimentacao.py:378`, `app/ponto/calculador/alimentacao.py:380`, `app/ponto/calculador/alimentacao.py:391`). `cct_do_motor` exige os quatro atributos nomeados e não nulos, devolvendo `None` caso falte algum, e lê os flags do motor com `bool` (`app/ponto/calculador/nucleo.py:317`, `app/ponto/calculador/nucleo.py:344`, `app/ponto/calculador/nucleo.py:347`).

`insumos_do_calculador` monta segmentos, marcos e pontualidade (`app/ponto/calculador/alimentacao.py:302`, `app/ponto/calculador/alimentacao.py:303`, `app/ponto/calculador/alimentacao.py:304`). Os segmentos usam `p.efetivo_entrada/efetivo_saida`, descontam intervalos quando `intra_descontada>0` e publicam os pares por dia (`app/ponto/calculador/alimentacao.py:73`, `app/ponto/calculador/alimentacao.py:115`, `app/ponto/calculador/alimentacao.py:121`, `app/ponto/calculador/alimentacao.py:145`). Os períodos incluem `periodos` e `periodos_ft`, por `periodos_do_resultado(resultado)` (`app/ponto/calculador/alimentacao.py:66`, `app/ponto/calculador/nucleo.py:360`, `app/ponto/calculador/nucleo.py:393`, `app/ponto/calculador/nucleo.py:399`).

A pontualidade usa os períodos do resultado, suas entradas/saídas cruas convertidas para horário local, marcos do dia ancorados no dia da jornada e publica `(entrada, saida, marco_e, marco_s, 0.0, 0.0)` (`app/ponto/calculador/alimentacao.py:225`, `app/ponto/calculador/alimentacao.py:228`, `app/ponto/calculador/alimentacao.py:251`, `app/ponto/calculador/alimentacao.py:257`, `app/ponto/calculador/alimentacao.py:278`, `app/ponto/calculador/alimentacao.py:289`). Os marcos vêm de `vinculo_do_dia` e `ec.marcos_do_dia` (`app/ponto/calculador/alimentacao.py:184`, `app/ponto/calculador/alimentacao.py:189`).

Há dois chamadores indiretos de produção dessa porta:

- Fechamento: monta insumos, chama `rubricas_por_dia` e passa `feriados` (`app/ponto/services/fechamento.py:1237`, `app/ponto/services/fechamento.py:1239`, `app/ponto/services/fechamento.py:1242`). Esse conjunto vem de `core.feriados.feriados_de(lugar_do_colab(colab), _env_ini, _env_fim, carregados=_feriados_lote)` (`app/ponto/services/fechamento.py:621`, `app/ponto/services/fechamento.py:622`).
- Comando DIFF: monta insumos com `aut.resultado` e chama `rubricas_por_dia` (`app/ponto/management/commands/diff_calculador.py:312`, `app/ponto/management/commands/diff_calculador.py:417`). Os feriados são `set(aut.feriados or ())` (`app/ponto/management/commands/diff_calculador.py:361`, `app/ponto/management/commands/diff_calculador.py:420`); a autoridade os obtém por `feriados_do_colab`, que delega a `core.feriados.feriados_de(lugar_do_colab(...))` (`app/ponto/services/espelho.py:1034`, `app/ponto/services/espelho.py:146`, `app/ponto/services/espelho.py:147`).

Sobre os `he_pontas`: são leituras de `p.minutos_extra_50` e `p.minutos_extra_100`, com pontas efetivas `(e,s)`, adicionadas se algum extra é positivo; dia visitado sem HE recebe lista vazia (`app/ponto/calculador/alimentacao.py:73`, `app/ponto/calculador/alimentacao.py:91`, `app/ponto/calculador/alimentacao.py:92`, `app/ponto/calculador/alimentacao.py:94`, `app/ponto/calculador/alimentacao.py:96`). Portanto refletem a dobra já aplicada pelo motor, não o excedente normal: os ramos de dobra escrevem duração integral em HE100 (`app/ponto/motor_calculo_v2.py:2797`, `app/ponto/motor_calculo_v2.py:2799`, `app/ponto/motor_calculo_v2.py:3243`, `app/ponto/motor_calculo_v2.py:3245`). Nas consolidações por dia do turno partido e comercial, primeiro zeram extras de todos os períodos, depois escrevem o trabalhado do dia no último período e dão `continue` (`app/ponto/motor_calculo_v2.py:2921`, `app/ponto/motor_calculo_v2.py:2923`, `app/ponto/motor_calculo_v2.py:3080`, `app/ponto/motor_calculo_v2.py:3082`, `app/ponto/motor_calculo_v2.py:3089`, `app/ponto/motor_calculo_v2.py:3307`, `app/ponto/motor_calculo_v2.py:3309`, `app/ponto/motor_calculo_v2.py:3325`, `app/ponto/motor_calculo_v2.py:3328`, `app/ponto/motor_calculo_v2.py:3340`).

## 01/11 02:50 · sessao 7c3196dad0b1 · CONTENCAO-QUE-MORDE-V2 · verificador · origem=Code

**Veredito final: bloqueia.** As quatro curas fecham os caminhos específicos tratados no delta, mas permanecem dois caminhos de liberação: B1 com hora ilegível e perda do estado com agentes síncronos vivos (`bin/disjuntor.py:505`, `bin/disjuntor.py:301`, `bin/disjuntor.py:1544`).

A execução integral dos selos permanece **[nao verificado]**: eles criam arquivos, contrariando esta leitura. Fiz leitura do código e contraprovas sintéticas em memória (`bin/tests/test_disjuntor.sh:39`, `bin/tests/test_contrato_do_push.sh:14`).

1. **Bloqueia — o descarte de #3 parte a não cobre o ramo sem arquivo novo.**  
   Com lançamento sem hora legível, `_job_proprio` considera o job antigo. Entretanto, se nenhum outro registro tiver timestamp legível anterior à janela, `medir_b1` retorna `disparo=False` **antes de examinar as cópias** (`bin/disjuntor.py:542`, `bin/disjuntor.py:490`, `bin/disjuntor.py:505`, `bin/disjuntor.py:509`).

   Contraprova em memória: job reconhecido como `bg1`, outro timestamp de cinco minutos atrás e cópia sem arquivo novo resultaram em `disparo=False`, motivo “vivo ha menos de 15 min”. Esse booleano permite a ferramenta pelo caminho B1 (`bin/disjuntor.py:505`, `bin/disjuntor.py:570`, `bin/disjuntor.py:2038`).

   O descarte é correto **quando existe arquivo novo**; não resolve este outro ramo, no qual a hora ilegível deveria fazer o job contar como antigo e não há novidade nas cópias. Portanto, a cura de #3 continua parcial frente ao tratamento restritivo contratado (`bin/disjuntor.py:542`, `app/docs/contratos/CONTENCAO-QUE-MORDE-V2.txt:15`).

2. **Bloqueia — “estado ausente = {}” permite o sétimo síncrono após perda do estado.**  
   A ausência inicializa `{}` tanto no escritor quanto no leitor, sem distinguir primeiro uso de perda posterior. O contador do transcript recupera somente lançamentos de fundo; os síncronos dependem das reservas perdidas (`bin/disjuntor.py:300`, `bin/disjuntor.py:334`, `bin/disjuntor.py:405`, `bin/disjuntor.py:414`, `bin/disjuntor.py:1544`).

   Na contraprova em memória, com o mesmo transcript contendo seis aberturas síncronas autorizadas, ainda sem resultado: preservadas as seis reservas, a porta negou por VIVOS; substituído o estado por `{}`, autorizou a sétima e gravou apenas sua reserva (`bin/disjuntor.py:1521`, `bin/disjuntor.py:1541`, `bin/disjuntor.py:1545`, `bin/disjuntor.py:1554`).

   Isso ultrapassa o teto contratado, que inclui síncronos enquanto rodam. A escolha da recuperação pode exigir desenho; o comportamento atual já permite autorização indevida. Ocorrência dessa perda em produção: **[nao verificado]** (`app/docs/contratos/CONTENCAO-QUE-MORDE-V2.txt:21`).

3. **Fila — idempotência limitada a 500 autorizações.**  
   Continua divergindo da idempotência sem prazo declarada: após a poda, o replay deixa de ser reconhecido por `autorizados`, embora sua chave possa continuar em `voltas.chaves`. O caminho identificado reaplica os gates e pode consumir novamente ou antecipar negação; não demonstrou liberação acima do teto (`bin/disjuntor.py:1522`, `bin/disjuntor.py:1550`, `bin/disjuntor.py:1553`, `bin/disjuntor.py:1557`, `app/docs/contratos/CONTENCAO-QUE-MORDE-V2.txt:10`).

4. **Fila — os selos ainda têm limites de causalidade e permissões.**  
   `i11_persiste` pressupõe que `chmod 555` impede gravação. Já o ramo da cópia inacessível de `v2_b1` é explicitamente pulado quando `ls` consegue lê-la. Os harnesses aceitam qualquer retorno não zero, sem exigir o subcaso previsto (`bin/tests/test_disjuntor.sh:1576`, `bin/tests/test_disjuntor.sh:1777`, `bin/tests/test_disjuntor.sh:1811`, `bin/tests/test_contrato_do_push.sh:98`).

As quatro curas do delta ficam assim:

| Achado curado | Avaliação |
|---|---|
| **#1 — corrida da terceira PROCEDENTE** | **Fecha o caminho descrito.** Ambas as reservas consultam `_b3_revalida` sob `_com_estado`; a função lê a triagem atual, desconta voltas liberadas/rearmadas e aplica o limiar compartilhado. A triagem também escreve sob essa trava (`bin/disjuntor.py:890`, `bin/disjuntor.py:904`, `bin/disjuntor.py:973`, `bin/disjuntor.py:1439`, `bin/disjuntor.py:1454`, `bin/disjuntor.py:1525`, `bin/disjuntor.py:1560`). |
| **#2 — exceção de estado/flock no B1** | **Fecha o caminho descrito.** Estado ilegível e esgotamento da tentativa de flock levantam; agora o `pretool` transforma essas exceções em negativa B1, inclusive para Bash (`bin/disjuntor.py:296`, `bin/disjuntor.py:303`, `bin/disjuntor.py:2035`, `bin/disjuntor.py:2037`). |
| **#3 parte b — cópia inacessível eliminada pelo `isdir`** | **Fecha o cenário descrito.** O `None` de `stat` mantém a cópia na lista; persistindo a inacessibilidade, `_mais_novo` retorna `None`, e essa negativa tem prioridade sobre outra cópia com arquivo novo. Permanece o residual temporal do achado 1 acima (`bin/disjuntor.py:432`, `bin/disjuntor.py:479`, `bin/disjuntor.py:444`, `bin/disjuntor.py:458`, `bin/disjuntor.py:511`). |
| **#4 — commit legível com árvore ilegível** | **Fecha o caminho descrito.** Falha de `ls-tree` retorna `None`; o julgamento recusa com rc 2 antes de interpretar “nenhum portão”. O leitor do contrato no PUSH_SHA também distingue `None` de ausência (`bin/contrato_do_push.py:28`, `bin/contrato_do_push.py:30`, `bin/contrato_do_push.py:60`, `bin/contrato_do_push.py:79`). |

Quanto ao B3, **não encontrei limiar divergente introduzido pelo delta**: `_b3_no_limiar` concentra os dois tetos, e `_codex_vivas` concentra o desconto de liberadas/rearmadas; revalidação e decisão usam essas mesmas funções. A revalidação faz uma segunda avaliação para autorizar, enquanto `decide_b3` continua materializando disparo, fotografia e liberação (`bin/disjuntor.py:957`, `bin/disjuntor.py:963`, `bin/disjuntor.py:976`, `bin/disjuntor.py:1024`, `bin/disjuntor.py:1029`, `bin/disjuntor.py:1063`, `bin/disjuntor.py:1070`).

**Sim, a revalidação escreve indiretamente no estado:** `_triagens` atualiza `b3_triagem_linhas`, e `_com_estado` persiste o dicionário mesmo quando o callback retorna negativa. Isso muda a leitura seguinte: fonte ausente ou com menos linhas passa a ser NAO_SEI. Não funciona como cursor nem pula linhas; a decisão seguinte relê a fonte inteira (`bin/disjuntor.py:946`, `bin/disjuntor.py:306`, `bin/disjuntor.py:311`, `bin/disjuntor.py:923`, `bin/disjuntor.py:929`, `bin/disjuntor.py:932`, `bin/disjuntor.py:1001`).

O mapa de escritores e leitores é:

| Retorno/campo | Quem escreve | Quem lê |
|---|---|---|
| `_b3_revalida` | Um produtor, com retornos PARADA por estado, NAO_SEI por fonte e PARADA/LIVRE pelo limiar (`bin/disjuntor.py:972`, `bin/disjuntor.py:975`, `bin/disjuntor.py:977`). | Reserva Codex distingue NAO_SEI/PARADA; porta final faz o mesmo (`bin/disjuntor.py:1439`, `bin/disjuntor.py:1440`, `bin/disjuntor.py:1442`, `bin/disjuntor.py:1525`, `bin/disjuntor.py:1526`, `bin/disjuntor.py:1528`). |
| `est['b3_triagem_linhas']` | **Três sítios de atribuição:** triagem acrescenta 1; `_triagens` registra o tamanho lido; cura acrescenta 1 (`bin/disjuntor.py:901`, `bin/disjuntor.py:946`, `bin/disjuntor.py:1352`). | Leitura principal em `_triagens`; os dois incrementos também leem o valor anterior. `_com_estado` serializa e carrega o campo (`bin/disjuntor.py:915`, `bin/disjuntor.py:901`, `bin/disjuntor.py:1352`, `bin/disjuntor.py:299`, `bin/disjuntor.py:311`). O selo também confere o valor (`bin/tests/test_disjuntor.sh:1481`). |
| Escrita indireta pela revalidação | Novo caminho até o escritor `_triagens`, não um quarto sítio de atribuição. Outros chamadores desse escritor são triagem, decisão, rearme, conferência de cura e registro de cura (`bin/disjuntor.py:973`, `bin/disjuntor.py:891`, `bin/disjuntor.py:1001`, `bin/disjuntor.py:1132`, `bin/disjuntor.py:1315`, `bin/disjuntor.py:1341`). | A marca persistida restringe as leituras seguintes de `_triagens` (`bin/disjuntor.py:915`, `bin/disjuntor.py:923`, `bin/disjuntor.py:929`). |
| `_copia_citada` | Um produtor: `S_ISDIR` retorna True/False; ausência/NotADirectory retorna False; outro OSError retorna None. False também pode significar caminho existente que não é diretório (`bin/disjuntor.py:428`, `bin/disjuntor.py:430`, `bin/disjuntor.py:432`). | Um leitor direto: o filtro conserva tudo que não seja False. O processamento posterior usa os caminhos conservados (`bin/disjuntor.py:479`, `bin/disjuntor.py:509`). |
| `_existe` por `ls-tree` | Um produtor: erro retorna None; saída não vazia retorna True; saída vazia retorna False (`bin/contrato_do_push.py:28`, `bin/contrato_do_push.py:30`, `bin/contrato_do_push.py:31`). | Existência do portão no commit/pais: lista, teste de None e `any`. Existência do contrato: teste de None e condição booleana (`bin/contrato_do_push.py:59`, `bin/contrato_do_push.py:60`, `bin/contrato_do_push.py:62`, `bin/contrato_do_push.py:79`, `bin/contrato_do_push.py:80`, `bin/contrato_do_push.py:82`). |

Para os mutantes solicitados, as oito âncoras são únicas e as substituições compilam em memória; os pontos de substituição estão nos selos citados abaixo. A execução integral e os retornos efetivamente observados pelos harnesses permanecem **[nao verificado]**.

| Mutante | Costura e causalidade do caso |
|---|---|
| `v2-revalida-sem-fonte` | Zera somente a contagem fresca da revalidação. Em memória, a reserva com três PROCEDENTES passou de rc 3 para rc 0: explica a queda de `v2_corrida` no **return 2**. O caso reproduz estado atrasado frente à fonte, sem executar uma intercalação real entre processos; a metade Agent cobra separadamente a negativa B3 após registrar curas (`bin/tests/test_disjuntor.sh:1899`, `bin/tests/test_disjuntor.sh:1760`, `bin/tests/test_disjuntor.sh:1762`, `bin/tests/test_disjuntor.sh:1763`, `bin/tests/test_disjuntor.sh:1765`). |
| `v2-b1-excecao-libera` | Troca o booleano da exceção por False. Em memória, o mesmo Bash passou de negativa B1 para `None`; explica a queda de `v2_b1` no **return 1** (`bin/tests/test_disjuntor.sh:1900`, `bin/tests/test_disjuntor.sh:1772`, `bin/tests/test_disjuntor.sh:1773`). |
| `v2-copia-isdir` | Restaura o filtro que perde a cópia inacessível. **Condicionado à falha real de permissões**, a cópia restante tem arquivo novo e falta a negativa exigida no **return 2**. Se o ramo for pulado, esse caso não mata o mutante (`bin/tests/test_disjuntor.sh:1901`, `bin/tests/test_disjuntor.sh:1777`, `bin/tests/test_disjuntor.sh:1778`, `bin/tests/test_disjuntor.sh:1785`). |
| `v2-arvore-ilegivel` | Converte erro de consulta em False. No fixture, o pai não tem portão e a árvore `bin` do commit novo foi removida; ambas as respostas ficam falsas e o commit é pulado. Isso explica a queda de `caso_v2` no **return 22**, por receber 0 em vez de 2 (`bin/tests/test_contrato_do_push.sh:107`, `bin/tests/test_contrato_do_push.sh:69`, `bin/tests/test_contrato_do_push.sh:74`, `bin/tests/test_contrato_do_push.sh:75`, `bin/contrato_do_push.py:62`). |
| `b3-sem-codex` | Remove Codex do predicado compartilhado, afetando decisão e revalidação. A terceira PROCEDENTE deixa de produzir a negativa B3 cobrada no **return 16**; uma negativa CURA não satisfaz o reconhecedor do caso (`bin/tests/test_disjuntor.sh:1880`, `bin/tests/test_disjuntor.sh:1191`, `bin/tests/test_disjuntor.sh:1212`, `bin/tests/test_disjuntor.sh:1213`). |
| `b3-codex-acumula` | Restaura a união com a lista anterior na decisão. A volta reclassificada como descartada permanece acumulada; a terceira entrada dispara indevidamente, explicando a queda no **return 34** (`bin/tests/test_disjuntor.sh:1891`, `bin/tests/test_disjuntor.sh:1234`, `bin/tests/test_disjuntor.sh:1237`, `bin/tests/test_disjuntor.sh:1239`). |
| `cx1-porta-le-estado` | Substitui a consulta ao juiz por leitura de `disparado`. A revalidação ainda recusa a quarta chamada, mas não materializa nem anuncia o disparo. O **return 7** agora mata o mutante pela ausência do anúncio da decisão; não prova liberação indevida do Codex (`bin/tests/test_disjuntor.sh:1913`, `bin/tests/test_disjuntor.sh:1283`, `bin/tests/test_disjuntor.sh:1285`, `bin/disjuntor.py:1443`, `bin/disjuntor.py:1075`). |
| `v4-excecao-falha-aberto` | Elimina a classificação B3 do `except`. O subcaso 8 levanta deliberadamente dentro de `pretool` usando Bash/Codex, evitando o fechamento alternativo exclusivo de Agent. Em memória, o controle negou e o mutante deixou stdout sem negativa: causalidade correta para o **return 8** (`bin/tests/test_disjuntor.sh:2090`, `bin/tests/test_disjuntor.sh:1457`, `bin/tests/test_disjuntor.sh:1460`, `bin/disjuntor.py:2103`, `bin/disjuntor.py:2112`, `bin/disjuntor.py:2121`). |

O descarte de `linha-torta` pode permanecer em **fila**: removida a guarda, a entrada de dois campos alcança `c[3]`, e a exceção produz retorno não zero, ainda recusado pelo pre-push. O selo exige especificamente rc 2, portanto sua queda não demonstra autorização indevida (`bin/contrato_do_push.py:46`, `bin/contrato_do_push.py:50`, `bin/tests/test_contrato_do_push.sh:60`, `bin/tests/test_contrato_do_push.sh:104`, `bin/pre-push.sh:10`).

**Os descartes que devem bloquear são o residual temporal de #3 parte a, no ramo sem arquivo novo, e a perda do estado com síncronos vivos. Veredito final: bloqueia** (`bin/disjuntor.py:505`, `bin/disjuntor.py:301`, `bin/disjuntor.py:1544`, `app/docs/contratos/CONTENCAO-QUE-MORDE-V2.txt:15`, `app/docs/contratos/CONTENCAO-QUE-MORDE-V2.txt:21`).

## 01/11 02:23 · sessao 4c92d7457787 · CONTENCAO-QUE-MORDE-V2 · verificador · origem=Code

**Há caminhos que ainda liberam indevidamente; o P1 deve ser bloqueado** (`bin/disjuntor.py:1404`, `bin/disjuntor.py:2079`, `bin/contrato_do_push.py:60`).

A execução integral dos selos fica **[nao verificado]**: eles criam arquivos e cópias, contrariando a restrição desta leitura (`bin/tests/test_disjuntor.sh:39`, `bin/tests/test_contrato_do_push.sh:14`). As conclusões abaixo vêm do código e de reproduções isoladas em memória.

1. **Bloqueia — a revalidação B3 do Codex ainda admite corrida.**  
   `b3_parada` consulta B3 e reserva em duas transições distintas (`bin/disjuntor.py:1137`, `bin/disjuntor.py:1147`). Entre elas, a terceira triagem PROCEDENTE pode ser gravada sem atualizar `disparado` (`bin/disjuntor.py:879`, `bin/disjuntor.py:888`, `bin/disjuntor.py:890`). A reserva verifica somente os campos `disparado`/`pede_desenho`; não recalcula a triagem, e autoriza outra volta (`bin/disjuntor.py:1404`, `bin/disjuntor.py:1412`). Portanto, o flock serializa as gravações, mas não fecha esse caminho: todos os gates deveriam valer na autorização (`app/docs/contratos/CONTENCAO-QUE-MORDE-V2.txt:10`).

2. **Bloqueia — exceção de estado/flock ainda libera Bash sujeito ao B1.**  
   Bash com `agent_id` entra em `b1_do_agente`, que usa `_com_estado` (`bin/disjuntor.py:1994`, `bin/disjuntor.py:560`). Estado ilegível ou flock ocupado levantam exceção (`bin/disjuntor.py:302`, `bin/disjuntor.py:295`). O fechamento novo cobre `Agent`; Bash fora do ramo B3 chega a “ferramenta liberada” (`bin/disjuntor.py:2070`, `bin/disjuntor.py:2079`). Isso viola estado ilegível = nega no PreToolUse (`app/docs/contratos/CONTENCAO-QUE-MORDE-V2.txt:4`).

3. **Bloqueia — a cura parcial do B1 ainda perde condições restritivas.**  
   O delta trata timestamp ilegível do lançamento como job devido, mas `medir_b1` pode voltar a liberar porque os demais timestamps são recentes ou inexistentes (`bin/disjuntor.py:531`, `bin/disjuntor.py:494`). Além disso, uma cópia inacessível pode desaparecer no filtro `isdir`; outra cópia com arquivo recente então libera sem examinar aquela (`bin/disjuntor.py:468`, `bin/disjuntor.py:506`). A prioridade nova para cópia ilegível só alcança cópias que sobreviveram ao filtro e devolveram `None` (`bin/disjuntor.py:498`). Não fecha a exigência de cópia ilegível = dispara (`app/docs/contratos/CONTENCAO-QUE-MORDE-V2.txt:15`).

4. **Bloqueia — árvore ilegível pode ser confundida com ausência do portão no push.**   [nao verificado]
   `_existe` retorna `False` quando a consulta ao caminho falha, desde que o objeto commit exista; existência do commit não prova legibilidade da árvore (`bin/contrato_do_push.py:27`, `bin/contrato_do_push.py:29`). Num commit que introduz o portão, com árvore ilegível e pai sem portão, todas as consultas podem resultar falsas: o gate pula o commit e retorna 0 (`bin/contrato_do_push.py:57`, `bin/contrato_do_push.py:60`, `bin/contrato_do_push.py:85`). A cura cobre commit inexistente, mas não toda árvore ilegível.

5. **Fila — idempotência termina após 500 autorizações.**  
   O replay podado funciona enquanto o ID permanece em `autorizados`; a lista descarta entradas antigas, e o gate não consulta `voltas.chaves` para recuperá-las (`bin/disjuntor.py:1485`, `bin/disjuntor.py:1514`, `bin/disjuntor.py:1518`). Depois disso, a mesma chamada pode consumir novamente ou bater no teto. É divergência da idempotência sem prazo do REVISADO, embora o caminho identificado antecipe negação (`app/docs/contratos/CONTENCAO-QUE-MORDE-V2.txt:10`).

Quanto às curas da volta 1, o texto original de cada achado não foi fornecido; a associação exata dos números **#3 e #5** permanece **[nao verificado]**. Os demais números aparecem nos comentários dos casos ou das curas:

| Achado | Avaliação da cura |
|---|---|
| #1 | Fecha autorização sem persistência: Agent nega e Codex retorna 4 (`bin/disjuntor.py:1522`, `bin/disjuntor.py:1420`). |
| #2 | Fecha exceção na abertura de `Agent`; não fecha Bash/B1, conforme achado 2 acima (`bin/disjuntor.py:2070`, `bin/disjuntor.py:2079`). |
| #3 | A poda agora exige `tool_result` estruturado, não mera ocorrência textual do ID; associação ao número original **[nao verificado]** (`bin/disjuntor.py:1502`, `bin/disjuntor.py:1537`). |
| #4 | A fotografia dos vivos passou para dentro do callback executado sob flock (`bin/disjuntor.py:1494`, `bin/disjuntor.py:290`, `bin/disjuntor.py:305`). |
| #5 | Há revalidação do estado B3 sob trava, mas permanece a corrida do achado 1; associação ao número original **[nao verificado]** (`bin/disjuntor.py:1404`, `bin/disjuntor.py:1488`). |
| #6 | Fecha replay depois da poda dentro da janela de 500 autorizações; residual no achado 5 (`bin/disjuntor.py:1485`, `bin/disjuntor.py:1518`). |
| #7 | Marca sidecar ilegível como semente parcial e completa a parte Agent quando chega transcript após inicialização pelo wrapper (`bin/disjuntor.py:1357`, `bin/disjuntor.py:1384`). |
| #9 | `--rearma-b3` recusa `pede_desenho`; `--redesenha` retira o pedido antes de rearmar (`bin/disjuntor.py:1102`, `bin/disjuntor.py:1557`). |
| #10 | HEAD ilegível nega mesmo sem PROCEDENTE a conferir (`bin/disjuntor.py:1475`). |
| #11 | Coletor passa a reconhecer tipo **ou** papel verificador na descrição (`bin/disjuntor.py:816`). |
| #12 parcial | Não fecha os caminhos do achado 3 (`bin/disjuntor.py:468`, `bin/disjuntor.py:494`). |
| #13 | Fecha o certificado reduzido a árvore/rc: leitor exige também Ran e OK (`bin/disjuntor.py:1271`). |
| #14 | No modo `<dir>`, falha de `git add` impede emissão do hash; o leitor nega hash ausente (`bin/arvore_hash.sh:33`, `bin/disjuntor.py:1262`). |
| #15 | Inclui merges, examina o pai de quem remove o portão e retorna 2 para linha malformada; permanece o achado 4 (`bin/contrato_do_push.py:44`, `bin/contrato_do_push.py:50`, `bin/contrato_do_push.py:57`). |
| #16 | Modelo sem preço aparece como PARCIAL (`bin/disjuntor.py:1588`, `bin/disjuntor.py:2281`). |
| #17 | A janela usa `agora` recebido pela borda (`bin/disjuntor.py:2193`, `bin/disjuntor.py:2201`). |

**Estado ausente continua diferente de estado ilegível:** ausência inicializa `{}`, sem distinguir primeiro uso de perda posterior do arquivo (`bin/disjuntor.py:299`). Isso perde reservas que só existiam no estado; a recomposição de vivos lê agentes de fundo, enquanto os síncronos dependem das reservas (`bin/disjuntor.py:403`, `bin/disjuntor.py:1505`). Ocorrência de perda em produção e recuperação completa nesse cenário: **[nao verificado]**.

O mapa de escritores e leitores é:

| Campo/arquivo/retorno | Quem escreve | Quem lê; multiplicidade |
|---|---|---|
| `est['autorizados']` | `porta_agent` acrescenta e limita a lista; `_com_estado` persiste o JSON (`bin/disjuntor.py:1516`, `bin/disjuntor.py:310`). | `porta_agent` consulta para replay (`bin/disjuntor.py:1485`). Um escritor lógico na produção. |
| `reserva_volta_codex`, rc 4 | A própria função retorna 4 por exceção ou sucesso não persistido (`bin/disjuntor.py:1419`, `bin/disjuntor.py:1421`). | `b3_parada` repassa; CLI publica; wrapper recusa qualquer não zero (`bin/disjuntor.py:1147`, `bin/disjuntor.py:2314`, `bin/codex_pergunta.sh:168`). O selo exige 4 (`bin/tests/test_disjuntor.sh:1574`). |
| `out['sem_preco']` | `_custo_24h` acumula turnos por modelo (`bin/disjuntor.py:1588`). | `ler` transporta e `_v2_txt` lê numa cópia do dicionário (`bin/disjuntor.py:2194`, `bin/disjuntor.py:2278`). Um escritor lógico. |
| `logs/suite_cert/<h>.ok` | Produção: `cert_suite`, chamada pela CLI e por `suite.sh`, escreve temporário e troca o arquivo (`bin/disjuntor.py:1252`, `bin/disjuntor.py:1255`, `bin/disjuntor.py:2299`, `bin/suite.sh:112`). | `_cert_ok`, chamado pela porta final (`bin/disjuntor.py:1265`, `bin/disjuntor.py:1467`). Há escritores adicionais nos testes, diretamente nas fixtures (`bin/tests/test_disjuntor.sh:73`, `bin/tests/test_disjuntor.sh:1691`, `bin/tests/test_disjuntor.sh:1698`). |
| Modo `<dir>` de `arvore_hash.sh` | Implementação única seleciona diretório, executa add estrito e publica `write-tree` (`bin/arvore_hash.sh:25`, `bin/arvore_hash.sh:33`, `bin/arvore_hash.sh:37`). | `disjuntor.arvore_hash`, duas fotografias da suite e testes (`bin/disjuntor.py:1220`, `bin/suite.sh:100`, `bin/suite.sh:111`, `bin/tests/test_disjuntor.sh:1697`). |
| `contrato_do_push.py`, rc 2 | `julga`, em vários ramos de erro (`bin/contrato_do_push.py:45`, `bin/contrato_do_push.py:52`, `bin/contrato_do_push.py:59`, `bin/contrato_do_push.py:79`). | `main` repassa; pre-push converte qualquer não zero em recusa; selo exige 2 para linha torta (`bin/contrato_do_push.py:89`, `bin/contrato_do_push.py:96`, `bin/pre-push.sh:10`, `bin/tests/test_contrato_do_push.sh:61`). Um escritor lógico. |

Nos testes novos, a causalidade dos mutantes fica assim; execução integral permanece **[nao verificado]**: [nao verificado]

| Mutante/caso | A costura removida explica o vermelho? |
|---|---|
| `v1-replay-podado` | **Sim, por leitura:** sem consultar `autorizados`, replay após seis voltas bate no teto e derruba `i11_persiste`, caso 3 (`bin/tests/test_disjuntor.sh:2081`, `bin/tests/test_disjuntor.sh:1569`). |
| `v1-reserva-sem-gravar` | **Sim, condicionado à falha real de gravação:** falta a negativa exigida no caso 4 (`bin/tests/test_disjuntor.sh:2082`, `bin/tests/test_disjuntor.sh:1571`). |
| `v1-codex-sem-gravar` | **Sim, na mesma condição:** retorna 0 em vez de 4 no caso 5 (`bin/tests/test_disjuntor.sh:2083`, `bin/tests/test_disjuntor.sh:1574`). |
| `v1-rearme-com-desenho` | **Sim:** remove a recusa; caso 11 cobra a mensagem específica, e 12 consulta a parada (`bin/tests/test_disjuntor.sh:2084`, `bin/tests/test_disjuntor.sh:1611`, `bin/tests/test_disjuntor.sh:1612`). |
| `v1-coletor-so-tipo` | **Sim:** deixa de coletar o general-purpose com papel verificador; há controle positivo para construtor (`bin/tests/test_disjuntor.sh:2085`, `bin/tests/test_disjuntor.sh:1626`, `bin/tests/test_disjuntor.sh:1629`). |
| `v1-relogio-proprio` | **Sim:** ignora `agora+2 dias`, conservando eventos que deveriam sair da janela (`bin/tests/test_disjuntor.sh:2086`, `bin/tests/test_disjuntor.sh:1669`, `bin/tests/test_disjuntor.sh:1670`). |
| `v1-sem-preco-some` | **Sim:** mantém custo opus, mas perde PARCIAL; cai no caso 2 (`bin/tests/test_disjuntor.sh:2087`, `bin/tests/test_disjuntor.sh:1643`, `bin/tests/test_disjuntor.sh:1644`). |
| `v1-cert-sem-ran` | **Sim:** certificado reduzido passa; falta a negativa do caso 13 (`bin/tests/test_disjuntor.sh:2088`, `bin/tests/test_disjuntor.sh:1692`). |
| `v1-head-ilegivel` | **Sim:** fixture tem certificado e repo sem commit; removida a guarda, falta a negativa do caso 14 (`bin/tests/test_disjuntor.sh:2089`, `bin/tests/test_disjuntor.sh:1696`, `bin/tests/test_disjuntor.sh:1699`). |
| `v1-hash-parcial` | **Condicional:** remove o add estrito, mas o caso 15 só roda quando `cat` realmente falha (`bin/tests/test_disjuntor.sh:2092`, `bin/tests/test_disjuntor.sh:1702`). |
| `v1-suite-sem-cert` | **Só prova textual:** remove de fato a chamada, porém cai pelo grep do caso 16; a suite não é executada por esse caso (`bin/tests/test_disjuntor.sh:2093`, `bin/tests/test_disjuntor.sh:1707`). |
| `sem-merges` | **Sim, por leitura:** omite o merge que adiciona instrumento sem obra; cai no caso 11 (`bin/tests/test_contrato_do_push.sh:89`, `bin/tests/test_contrato_do_push.sh:54`, `bin/tests/test_contrato_do_push.sh:56`). |
| `tira-portao` | **Sim:** deixa de examinar o pai; a remoção passa e derruba o caso 12 (`bin/tests/test_contrato_do_push.sh:90`, `bin/tests/test_contrato_do_push.sh:59`). |
| `linha-torta` | **Não prova liberação indevida:** desativa a guarda, mas a entrada de dois campos causa `IndexError` em `c[3]`. O processo continua negando, com rc 1; o caso cai porque exige rc 2 (`bin/tests/test_contrato_do_push.sh:91`, `bin/contrato_do_push.py:48`, `bin/tests/test_contrato_do_push.sh:61`). **Fila.** |

Os casos i9 12–14 exercitam duas cópias, certificado incompleto e HEAD ilegível; 15 é condicional às permissões; 16 é textual; 17 apenas confirma que o fixture sem HEAD ainda produz hash (`bin/tests/test_disjuntor.sh:1688`, `bin/tests/test_disjuntor.sh:1692`, `bin/tests/test_disjuntor.sh:1697`, `bin/tests/test_disjuntor.sh:1699`, `bin/tests/test_disjuntor.sh:1702`, `bin/tests/test_disjuntor.sh:1707`).

**Fila — fragilidade dos selos:** `i11_persiste` pressupõe que chmod555 impede gravação, sem confirmar a condição; o harness aceita qualquer retorno não zero como morte do mutante, sem exigir o subcaso esperado (`bin/tests/test_disjuntor.sh:1570`, `bin/tests/test_disjuntor.sh:1772`). Isso impede afirmar causalidade universal desses dois mutantes de persistência.

Sobre os descartes: não há evidência nova suficiente para promover a guarda lexical, cura exclusiva do final, ABA da suite ou grep de ordem a bloqueadores adicionais; a correspondência com os descartes fornecidos permanece **[nao verificado]**. A cura exclusiva do final e o adiamento do atalho da régua estão expressos no REVISADO (`app/docs/contratos/CONTENCAO-QUE-MORDE-V2.txt:13`, `app/docs/contratos/CONTENCAO-QUE-MORDE-V2.txt:19`). A cópia divergente de `app/core/hooks/pre-push.sh` mantém SKIP antecipado, mas o instalador vigente gera delegação para `bin/pre-push.sh`; qual hook está instalado em produção é **[nao verificado]** (`app/core/hooks/pre-push.sh:5`, `bin/hooks.sh:56`). Os bloqueios B1 acima envolvem estado/flock e cópia ilegível, condições cobertas pelo P1 (`app/docs/contratos/CONTENCAO-QUE-MORDE-V2.txt:4`, `app/docs/contratos/CONTENCAO-QUE-MORDE-V2.txt:15`).

**Veredito final: bloqueia**, pelos caminhos de autorização residual nos achados 1–4 (`bin/disjuntor.py:1404`, `bin/disjuntor.py:2079`, `bin/disjuntor.py:494`, `bin/contrato_do_push.py:60`).

## 01/11 02:21 · sessao 99fe14057718 · - · - · origem=Ronald (tmux)

Achados para consolidação, todos por leitura e provas em memória com `python3 -B`, sem criar arquivos:

1. **Bloqueia — a revalidação Codex ainda usa B3 desatualizado.** `b3_parada` decide em uma transição e reserva em outra (`bin/disjuntor.py:1137`, `bin/disjuntor.py:1147`). Entre ambas, a terceira triagem PROCEDENTE pode gravar a trilha e `b3_triagem_linhas`, sem marcar `disparado` (`bin/disjuntor.py:879`, `bin/disjuntor.py:888`, `bin/disjuntor.py:890`). A reserva consulta apenas `disparado`/`pede_desenho`, sem reler/recalcular a triagem, e autoriza (`bin/disjuntor.py:1404`, `bin/disjuntor.py:1412`). Prova em memória com as funções reais produziu `rc=0`, três PROCEDENTES, `disparado=False` e quarta volta reservada. Viola a exigência de passar todos os gates antes da autorização (`app/docs/contratos/CONTENCAO-QUE-MORDE-V2.txt:10`; limiar em `bin/disjuntor.py:77`).

2. **Bloqueia — exceção de estado/flock ainda libera Bash do agente submetido a B1.** O caminho Bash com `agent_id` chama B1 (`bin/disjuntor.py:1994`), que usa `_com_estado` (`bin/disjuntor.py:560`). Estado ilegível e flock ocupado levantam exceção (`bin/disjuntor.py:302`, `bin/disjuntor.py:295`). O fechamento novo cobre `Agent`, mas Bash fora do ramo B3 continua em “ferramenta liberada” (`bin/disjuntor.py:2070`, `bin/disjuntor.py:2079`). Prova em memória produziu `rc=0`, stdout vazio e essa linha FALHA. Contraria estado ilegível = nega PreToolUse (`app/docs/contratos/CONTENCAO-QUE-MORDE-V2.txt:4`). O descarte fornecido de #12 cita transcript ilegível e reconhecimento textual de job/fim, sem cobrir estado/flock [nao verificado: anexo sem arquivo local numerado].

3. **Bloqueia — cura temporal de B1 não chega à decisão final.** `_job_proprio` agora considera timestamp ilegível como job devido (`bin/disjuntor.py:529`, `bin/disjuntor.py:532`), porém `medir_b1` retorna falso se os demais timestamps legíveis forem recentes ou não existirem (`bin/disjuntor.py:494`). Prova em memória: `_job_proprio → bgj1`; `medir_b1 → disparo=False, vivo ha menos de 15 min`, com cópia acessível sem arquivo novo. O resultado contradiz a regra restritiva declarada para esse caminho (`bin/disjuntor.py:451`; `app/docs/contratos/CONTENCAO-QUE-MORDE-V2.txt:15`).

4. **Fila/limite residual — cópias inacessíveis podem desaparecer da medição.** `os.path.isdir` filtra as cópias antes da verificação restritiva; uma cópia citada que não possa ser reconhecida como diretório não chega a `_mais_novo` (`bin/disjuntor.py:468`, `bin/disjuntor.py:498`). Uma segunda cópia acessível com arquivo recente encerra B1 sem disparo (`bin/disjuntor.py:506`). Além disso, `_mais_novo` retorna no primeiro arquivo recente, antes de examinar possíveis trechos ilegíveis da mesma cópia (`bin/disjuntor.py:442`). A precedência restritiva adicionada entre cópias não resolve esses casos (`bin/disjuntor.py:499`). Ocorrência real [nao verificado].

Curas efetivas nos caminhos examinados:

- Agent sem gravação retorna negativa e Codex sem gravação retorna 4 (`bin/disjuntor.py:1522`, `bin/disjuntor.py:1420`).
- Fotografia de vivos está dentro da transição sob flock (`bin/disjuntor.py:1494`; execução do callback sob trava em `bin/disjuntor.py:290`, `bin/disjuntor.py:305`).
- Poda exige `tool_result` estruturado, em vez de mera ocorrência textual do ID (`bin/disjuntor.py:1502`, `bin/disjuntor.py:1537`).
- Replay podado permanece idempotente enquanto o ID estiver nas últimas 500 autorizações; depois disso pode consumir novamente, pois `voltas.chaves` não participa desse teste (`bin/disjuntor.py:1485`, `bin/disjuntor.py:1514`, `bin/disjuntor.py:1518`). Esse residual tende a negar antes, sem caminho novo de liberação demonstrado [nao verificado].
- `--rearma-b3` recusa `pede_desenho`; `--redesenha` retira o pedido antes de rearmar (`bin/disjuntor.py:1102`, `bin/disjuntor.py:1557`).
- Coletor aceita tipo verificador ou papel verificador na descrição (`bin/disjuntor.py:816`).
- Semente marca sidecar ilegível como parcial e completa uma vez a parte Agent semeada sem transcript (`bin/disjuntor.py:1357`, `bin/disjuntor.py:1384`).

**Veredito: bloqueia**, pelos caminhos 1–3 (`bin/disjuntor.py:1404`, `bin/disjuntor.py:2079`, `bin/disjuntor.py:494`).

## 01/11 02:20 · sessao 0c693fff470a · - · - · origem=Ronald (tmux)

- **bloqueia — árvore ilegível ainda pode virar “anterior ao portão”.** `_existe` devolve `False` quando `sha:caminho` falha, desde que o objeto commit exista; não distingue caminho ausente de árvore faltante/corrompida. Se isso ocorrer no commit que introduz o portão e o pai não o contém, `tem` fica falso e o commit é ignorado, chegando a rc 0. Prova em memória com `_git` substituído: árvore faltante + objeto commit presente → `_existe=False`, `julga=(0,[])`. Isso contraria o “não sei” exigido. (`bin/contrato_do_push.py:27`, `bin/contrato_do_push.py:29`, `bin/contrato_do_push.py:57`, `bin/contrato_do_push.py:60`, `bin/contrato_do_push.py:85`; contrato `app/docs/contratos/CONTENCAO-QUE-MORDE-V2.txt:4`.)

- **fila — mutante `linha-torta` é morto por exceção, sem provar liberação indevida.** Desativar a guarda deixa a entrada de dois campos alcançar `c[3]`, causando `IndexError` e rc 1; o caso fica vermelho porque exige rc 2. O mutante remove a validação, mas permanece negando pelo processo que falhou. (`bin/tests/test_contrato_do_push.sh:60`, `bin/tests/test_contrato_do_push.sh:61`, `bin/tests/test_contrato_do_push.sh:91`; `bin/contrato_do_push.py:44`, `bin/contrato_do_push.py:48`.)

- **Mutantes causais por leitura:** `sem-merges` acrescenta `--no-merges`, fazendo desaparecer o único commit que escreve `bin/resolucao.sh` sem obra; o caso falha em `return 11`. `tira-portao` consulta apenas a árvore do próprio commit; a remoção do portão passa, causando `return 12`. A execução real desses repositórios sintéticos é **[nao verificado]**, pois escreveria arquivos. (`bin/tests/test_contrato_do_push.sh:51`, `bin/tests/test_contrato_do_push.sh:54`, `bin/tests/test_contrato_do_push.sh:56`, `bin/tests/test_contrato_do_push.sh:57`, `bin/tests/test_contrato_do_push.sh:59`, `bin/tests/test_contrato_do_push.sh:89`, `bin/tests/test_contrato_do_push.sh:90`.)

- **rc 2 — escritor/leitores:** `julga` escreve o resultado 2 nas falhas de forma, enumeração, pais, árvore, diff e mensagem; `main` o recebe e `sys.exit` o publica. O leitor de produção trata qualquer não-zero como recusa, convertendo para rc 1 antes de `SKIP_TESTS`. O selo exige exatamente 2 na linha torta e apenas não-zero para SHA inexistente. Há um escritor lógico desse código de retorno: `julga`. (`bin/contrato_do_push.py:45`, `bin/contrato_do_push.py:52`, `bin/contrato_do_push.py:56`, `bin/contrato_do_push.py:59`, `bin/contrato_do_push.py:67`, `bin/contrato_do_push.py:73`, `bin/contrato_do_push.py:79`, `bin/contrato_do_push.py:89`, `bin/contrato_do_push.py:96`; `bin/pre-push.sh:10`; `bin/tests/test_contrato_do_push.sh:61`, `bin/tests/test_contrato_do_push.sh:63`.)

## 01/11 02:20 · sessao 115a9ac88b85 · - · - · origem=Ronald (tmux)

Leitura estática concluída; execução dos casos/mutantes permanece [nao verificado], pois criam arquivos (`bin/tests/test_disjuntor.sh:39`, `:51`, `:1761`).

| Mutante | Costura removida e ponto esperado do vermelho |
|---|---|
| `v1-replay-podado` | Remove consulta a `autorizados`; depois de seis voltas, replay de `p1` passa a bater no teto, derrubando caso 3 (`bin/tests/test_disjuntor.sh:2081`, `:1567`, `:1569`; `bin/disjuntor.py:1485`, `:1511`). |
| `v1-reserva-sem-gravar` | Remove checagem positiva de persistência Agent; autorização sem gravar deixa de emitir negativa exigida pelo caso 4 (`bin/tests/test_disjuntor.sh:2082`, `:1571`; `bin/disjuntor.py:1522`). |
| `v1-codex-sem-gravar` | Remove conversão de sucesso sem gravação para rc 4; caso 5 encontra rc 0 (`bin/tests/test_disjuntor.sh:2083`, `:1572`, `:1574`; `bin/disjuntor.py:1420`). |
| `v1-rearme-com-desenho` | Remove recusa do rearme; caso 11 perde mensagem específica; caso 12 também consulta parada (`bin/tests/test_disjuntor.sh:2084`, `:1610`, `:1611`, `:1612`; `bin/disjuntor.py:1102`). |
| `v1-coletor-so-tipo` | Exclui general-purpose cujo papel é verificador; caso 1 de `i4_coletor` deixa de ver B3. Controle construtor consulta autorização positiva (`bin/tests/test_disjuntor.sh:2085`, `:1625`, `:1626`, `:1628`, `:1629`). |
| `v1-relogio-proprio` | Ignora relógio fornecido; consulta com agora+2 dias conserva os dois eventos, derrubando caso 5 (`bin/tests/test_disjuntor.sh:2086`, `:1668`, `:1669`, `:1670`). |
| `v1-sem-preco-some` | `continue` descarta turno desconhecido; custo opus continua e falta PARCIAL, derrubando caso 2 (`bin/tests/test_disjuntor.sh:2087`, `:1638`, `:1639`, `:1643`, `:1644`). |
| `v1-cert-sem-ran` | Desliga exigência conjunta Ran/OK; certificado reduzido é aceito, faltando negativa do caso 13 (`bin/tests/test_disjuntor.sh:2088`, `:1691`, `:1692`). |
| `v1-head-ilegivel` | Remove recusa de HEAD vazio; fixture é repo sem commit com certificado válido e sem PROCEDENTE, faltando negativa do caso 14 (`bin/tests/test_disjuntor.sh:2089`, `:1695`, `:1698`, `:1699`; `bin/disjuntor.py:1475`). |
| `v1-hash-parcial` | Troca `git add` estrito por `--ignore-errors`; caso 15 exige falha do hash real, mas somente quando `cat` não consegue ler (`bin/tests/test_disjuntor.sh:2092`, `:1701`, `:1702`, `:1703`). |
| `v1-suite-sem-cert` | Substitui chamada Python por `true`; cai exclusivamente porque desaparece a string procurada pelo grep do caso 16. A conexão funcional suite→certificado não é exercitada (`bin/tests/test_disjuntor.sh:2093`, `:1707`). |

Os casos i9 novos cobrem: duas cópias (12), cert sem Ran/OK (13), HEAD ilegível (14), hash com arquivo ilegível (15), presença textual do chamador (16) e pré-condição do fixture sem HEAD ainda produzir hash (17). O 17 é montagem do cenário, não verificação de uma negativa (`bin/tests/test_disjuntor.sh:1688`, `:1692`, `:1697`, `:1699`, `:1703`, `:1707`).

Achados de teste: [nao verificado]

- **Fila — dependência de permissões reais:** `i11_persiste` pressupõe que chmod555 impede gravação; não confirma essa condição. Sob usuário que ultrapasse permissões, o próprio controle falha no caso 4 antes de atingir o mutante Codex; execução nesse contexto [nao verificado] (`bin/tests/test_disjuntor.sh:1570`, `:1571`, `:1572`). O harness roda controle separado, mas considera qualquer saída não zero do caso mutante como mordida, sem exigir o subcaso esperado (`bin/tests/test_disjuntor.sh:1764`, `:1772`).
- **Fila — prova textual da suite:** `v1-suite-sem-cert` remove de fato a chamada, porém o vermelho demonstra alteração textual; não demonstra emissão real do certificado após uma suite inteira (`bin/tests/test_disjuntor.sh:1707`, `:2093`).
- **Fila — teste condicional de ilegibilidade:** se `cat` ler o arquivo chmod000, caso 15 é pulado. Nesse ambiente o mutante pode sobreviver; o harness o reportaria como falha, e não como sucesso falso (`bin/tests/test_disjuntor.sh:1702`, `:1703`, `:1772`).
- **Fila — curas sem caso novo correspondente:** os novos testes listados não injetam exceção de `main_pretool`, concorrência entre autorização/B3 ou troca de fotografia de vivos sob flock; seu comportamento em execução fica [nao verificado]. As adições concretas são apenas os casos listados em `bin/tests/test_disjuntor.sh:1750` e os mutantes de `:2081` a `:2093`.

Os controles em cópia agora incluem `suite.sh`, corrigindo a ausência dessa dependência para o grep i9 (`bin/tests/test_disjuntor.sh:1761`, `:1707`); todos os casos novos entram também no laço de controles sem mutação (`bin/tests/test_disjuntor.sh:1790`).

## 01/11 02:02 · sessao 99fd5c598699 · GRADE-FOLGA-JUIZ · verificador · origem=Code

**Os dois PROCEDENTES estão curados.** O teste novo morde as exclusões de `precedencia.py` pelo percurso, e a descrição de horizonte/`--forcar` corresponde ao código. **Não libero pouso incondicional:** o ensaio exigido antes dele continua **[não verificado]**. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:245`, `app/ponto/services/cartorio.py:178`, `app/ponto/services/cartorio.py:174`)

Referências abaixo são da cópia fornecida como **16298e59**. Exercitei a catraca em memória, substituindo Django por `unittest` e o diretório temporário por arquivos simulados; não executei a integração Django nem consultei banco ou logs: **[não verificado]**.

**1. Cura e mordida**

A execução em memória dos **12 testes da catraca passou**, com **22 achados e 22 entradas na ALLOWLIST**. O teste novo falhou, por asserção, em cada mutante abaixo. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:139`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:176`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:260`)

| Mutante aplicado somente em memória | Teste novo | [nao verificado]
|---|---|
| Excluir arquivos chamados `precedencia.py` no filtro de extensão | **RED**. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:128`) |
| Pular especificamente `ponto/precedencia.py` pelo caminho relativo | **RED**. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:129`) |
| Podar o diretório `ponto` em `dirs[:]` | **RED**. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:125`) |
| Fazer `andar` ignorar todas as funções de `ponto/precedencia.py` | **RED**. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:113`) |

Portanto, a lacuna da rodada anterior foi fechada para essas formas de regressão. Isso não transforma a AST em detector universal: `Q`, kwargs dinâmicos, reflexão e código no nível de módulo continuam limites declarados. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:17`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:245`)

**2. Quem escreve e quem lê**

| Valor | Escrita → leitura |
|---|---|
| Fonte usada pelo teste novo | O próprio teste escreve uma **fonte sintética literal** em `ponto/precedencia.py` temporário; `achados` percorre e abre esse arquivo, e `achados_da_fonte` analisa a AST. A asserção exige somente `outro/filtro`. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:251`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:130`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:260`) |
| Fonte real do juiz | **O teste novo não a lê nem executa o juiz.** Os testes anteriores de cópia literal é que usam `inspect.getsource` do predicado e do carregador reais. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:212`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:220`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:251`) |
| Versão e impressão | `REGRA_VERSOES` declara; `versao_para` escolhe a maior versão aplicável à competência; `_regra_do_dia` fornece o valor aos ramos; `impressao_insumos` incorpora `regra:N`. (`app/ponto/services/cartorio.py:111`, `app/ponto/services/cartorio.py:214`, `app/ponto/services/cartorio.py:963`, `app/ponto/services/cartorio.py:371`) |
| Ata e carimbos | O cartório monta a ata, chama a lavra existente e entrega os carimbos à porta; a porta salva célula e evento no `atomic` existente. (`app/ponto/services/cartorio.py:1580`, `app/ponto/services/cartorio.py:1602`, `app/ponto/portas/celula.py:639`) |

O texto novo está correto no alcance principal: a v3 altera a impressão da competência **10 em diante**; a 09 mantém versão 1. Sem `--forcar`, uma célula sem carimbo válido ou com impressão divergente continua elegível. (`app/ponto/services/cartorio.py:214`, `app/ponto/tests/test_o219_versao_da_regra.py:85`, `app/ponto/services/cartorio.py:1176`)

“Todas as células passadas” descreve o universo padrão do comando, **com `--apply` para persistir e sem `--limite`**. `forcar` remove o pulo por impressão atual; não elimina outros `continue`, como ausência do dia na grade. É uma ressalva de precisão, sem nova regressão demonstrada. (`app/ponto/management/commands/processar_cartorio.py:27`, `app/ponto/management/commands/processar_cartorio.py:49`, `app/ponto/management/commands/processar_cartorio.py:56`, `app/ponto/services/cartorio.py:1180`, `app/ponto/services/cartorio.py:1183`)

**3. Triagem: algum descarte errado?**

**Sim: o “NÃO PROCEDE” do pré-requisito precisa ser qualificado.** Não há prova de que faltou ensaio; também não há prova disponível de que foi cumprido. Atribuí-lo à sessão principal não dispensa a exigência explícita anterior ao pouso. Os resultados citados em logs, inclusive suficiência do ensaio da v9 para esta entrega, permanecem **[não verificado]**. (`app/ponto/services/cartorio.py:174`, `app/docs/BACKLOG.md:796`)

Os demais encaminhamentos são aceitáveis com estes limites:

- **Documentação:** BACKLOG ainda enumera fase como migrada; a linha seguinte registra o desmembramento. Pode ser corrigido no marco do pouso; não deve virar dívida posterior. A edição alegada na principal é **[não verificado]**. (`app/docs/BACKLOG.md:796`, `app/docs/BACKLOG.md:797`, `app/docs/LEIS.md:132`)
- **Juízes novos:** o corpo dos commits fornecido resolve a dúvida anterior: **um na obra, zero adicionais no delta**, compatível com registro e corte. (`app/core/juizes.py:192`, `app/docs/CORTES.md:416`)
- **C5, gerador e fase:** continuam filas declaradas; fase permanece fora desta obra pelo aval informado. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:21`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:160`, `app/docs/BACKLOG.md:797`)

**4. Classes de reprovação**

| Classe | Situação nesta verificação |
|---|---|
| Segundo juiz | Os sítios curados delegam; **C5 e gerador continuam abertos**, não curados pela obra. (`app/ponto/precedencia.py:505`, `app/ponto/services/declarar_dia.py:74`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1532`, `app/escala/models.py:1280`) |
| Segundo escritor fora da porta | Não localizado no trecho alterado: overlay escreve dict; persistência usa a porta existente. (`app/escala/utils.py:1126`, `app/ponto/services/cartorio.py:1582`, `app/ponto/portas/celula.py:807`) |
| Relógio fora da borda | Não localizado nas funções novas; o comando captura a data na borda. (`app/ponto/management/commands/rejulgar_ausencia_em_folga.py:52`, `app/ponto/precedencia.py:194`, `app/ponto/precedencia.py:222`) |
| Derivação paralela | Grade pergunta ao juiz; calendário compartilha a função entre classificação e publicação. (`app/escala/utils.py:1124`, `app/colaboradores/services/calendario.py:839`, `app/colaboradores/services/calendario.py:840`) |
| Selo sem mordida | **Curado para os mutantes exercitados acima.** (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:260`) |
| Ordem de trava | Nenhuma aquisição nova no delta; ausência de corrida operacional: **[não verificado]**. (`app/ponto/portas/celula.py:639`) |
| Pouso antes do pré-requisito | Condição permanece expressa; cumprimento: **[não verificado]**. (`app/ponto/services/cartorio.py:174`) |

**5. LEI-AKITA dos commits** [nao verificado]

A linha da **obra/303970f1** corresponde ao desenho: juiz, carregador, predicado, três sítios migrados e leitores indicados existem; `juiz unico` é padrão cadastrado. Resultados dos RED citados: **[não verificado]**. (`app/ponto/precedencia.py:194`, `app/ponto/precedencia.py:212`, `app/ponto/precedencia.py:505`, `app/ponto/services/declarar_dia.py:74`, `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:65`, `app/docs/GLOSSARIO.md:12`)

A linha do **delta/16298e59** é **parcialmente literal**: `achados`, `test_so_encolhe`, zero juízes adicionais e o padrão conferem. Porém, `testemunha=folga_lancada_do_dia` não descreve o que o teste novo lê: ele lê **o resultado de `achados` sobre fonte sintética**, sem chamar essa função do juiz. Cabe corrigir essa descrição na entrega; isso não desfaz a mordida demonstrada. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:177`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:251`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:260`)

**Bloqueia liberação incondicional:** comprovação do pré-requisito operacional ainda **[não verificado]**. **Fila:** C5, gerador e fase; documentação deve acompanhar o marco. **Não procede:** reabrir fase nesta obra, afirmar ausência do juiz em JUIZES.md, ou manter a acusação de catraca sem mordida para os quatro mutantes examinados. (`app/ponto/services/cartorio.py:174`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:21`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:160`, `app/docs/BACKLOG.md:797`, `app/docs/LEIS.md:132`, `app/docs/JUIZES.md:84`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:260`)

## 01/11 02:00 · sessao fcd1a1f10e20 · - · - · origem=Ronald (tmux)

Não localizei novo bloqueio material na cadeia de produção alterada; permanecem duas derivações conhecidas, declaradas como filas: C5 e gerador. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:21`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:160`)

- **Juiz e universo:** o carregador lê todas as FolgaDia da pessoa na janela, preserva todas por data e ordena por pk; o juiz percorre candidatas e aplica um único predicado de origem/vínculo. (`app/ponto/precedencia.py:206`, `app/ponto/precedencia.py:208`, `app/ponto/precedencia.py:216`, `app/ponto/precedencia.py:237`)
- **Sítios migrados:** veto, declaração e comando perguntam ao juiz; `escala_vigente` entrega o vínculo por `vinculo_do_dia`. (`app/ponto/precedencia.py:505`, `app/ponto/services/declarar_dia.py:74`, `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:65`, `app/escala/servico_jornada.py:57`)
- **Grade:** por turno passa o mapa julgado do vínculo do dia; legacy segmenta pelo mesmo juiz e conserva somente os dias do segmento. O overlay escreve apenas o dict em memória. (`app/escala/utils.py:1262`, `app/escala/utils.py:1794`, `app/escala/utils.py:972`, `app/escala/utils.py:995`, `app/escala/utils.py:999`, `app/escala/utils.py:1124`)
- **Calendário:** classificação e `eh_trabalho` publicado leem a mesma função sobre o mesmo dict; não há nova duplicação da regra nessa extração. (`app/colaboradores/services/calendario.py:103`, `app/colaboradores/services/calendario.py:126`, `app/colaboradores/services/calendario.py:839`)
- **Segundo escritor:** não apareceu no trecho alterado. FolgaDia mantém escritor registrado em `escala/folgas.py`; cartório passa a ata pela lavra existente e carimba pela porta, cujo save/evento compartilham `atomic`. (`app/core/portas.py:466`, `app/ponto/services/cartorio.py:1582`, `app/ponto/services/cartorio.py:1602`, `app/ponto/portas/celula.py:639`, `app/ponto/portas/celula.py:807`)
- **Relógio:** o comando captura `hoje` na borda e passa a janela; carregador, juiz, predicado e overlay não fazem nova leitura do relógio. (`app/ponto/management/commands/rejulgar_ausencia_em_folga.py:52`, `app/ponto/precedencia.py:194`, `app/ponto/precedencia.py:212`, `app/ponto/precedencia.py:222`, `app/escala/utils.py:1103`)
- **Ordem de trava:** o delta fornecido não introduz aquisição de trava; a persistência examinada conserva o `atomic` existente. Ausência de corrida operacional: **[não verificado]**. (`app/ponto/portas/celula.py:639`)
- **C5 continua errado para calendário alheio:** monta pares por cobertura de vínculo, sem consultar origem/juiz, e acusa qualquer célula `trabalha=True` fora de competência exportada. A triagem reconhece corretamente essa classe aberta como GRADE-FOLGA-C5. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1532`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1542`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:21`)
- **Gerador continua por vínculo:** consome `_datas_folga` do próprio vínculo ou consulta FolgaDia por `escala_colaborador=self`; não vê manual presa a vínculo anterior. Essa dívida está explicitamente marcada GRADE-FOLGA-GERADOR. (`app/escala/models.py:1251`, `app/escala/models.py:1280`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:160`)

Há resíduo documental pequeno: o predicado ainda diz “o calendário passa `True`”, embora o calendário já tenha retirado essa alimentação. Não é escritor/juiz novo nem bloqueio funcional localizado. (`app/ponto/precedencia.py:229`, `app/colaboradores/services/calendario.py:617`)

## 01/11 02:00 · sessao 320c4aa0025c · - · - · origem=Ronald (tmux)

- **A triagem não pode tratar o pré-requisito de ensaio como dispensado.** O cartório exige `--apply`, dias rejulgados, dias que movem minuto e `FORA-DO-HORIZONTE 0` **antes do pouso**; BACKLOG ainda exige `main + reversão + frota antes do opus`. Cumprimento operacional permanece **[não verificado]**; isso não demonstra que o ensaio faltou, mas impede aprovação incondicional. (`app/ponto/services/cartorio.py:174`, `app/docs/BACKLOG.md:796`)

- **FILA de fase é correta pelo aval explícito.** A cópia continua lendo a foto do vínculo ativo e comparando foto×âncora; o desmembramento está registrado e sua imprecisão declarada. (`app/escala/services/fase_conflitante.py:71`, `app/escala/services/fase_conflitante.py:74`, `app/escala/services/fase_conflitante.py:80`, `app/docs/BACKLOG.md:797`)

- **C5 e gerador permanecem abertos na obra inteira, declaradamente.** C5 considera FolgaDia por cobertura e acusa célula de trabalho sem distinguir origem; gerador lê foto do vínculo. São limites expressos da catraca, não curas concluídas. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1532`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1548`, `app/escala/models.py:1251`, `app/escala/models.py:1280`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:17`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:160`)

- **FILA dos documentos é defensável como atualização no marco, mas ainda não concluída.** BACKLOG ainda afirma que fase pergunta ao juiz; a linha seguinte supersede isso. L-106 manda docs no commit do marco. A edição alegadamente existente na principal é **[não verificado]**. CORTES registra o corte original, agora limitado pelo aval fornecido pelo usuário. (`app/docs/BACKLOG.md:796`, `app/docs/BACKLOG.md:797`, `app/docs/LEIS.md:132`, `app/docs/CORTES.md:416`)

- **“Juízes novos” está resolvido pelo corpo dos commits fornecido:** uma autoridade na obra, zero adicionais no delta, compatível com registro e corte. O padrão `juiz unico` existe no vocabulário autorizado. (`app/core/juizes.py:188`, `app/core/juizes.py:192`, `app/docs/CORTES.md:416`, `app/docs/GLOSSARIO.md:12`, `app/docs/PLANO-PADROES.md:49`)

- **Não encontrei nova ordem de travas ou segundo escritor no trecho examinado.** O veto novo apenas carrega e julga; a lavra continua pela porta, com save/evento no `atomic` existente. Ausência de corrida em execução é **[não verificado]**. (`app/ponto/precedencia.py:501`, `app/ponto/precedencia.py:505`, `app/ponto/services/cartorio.py:1582`, `app/ponto/services/cartorio.py:1602`, `app/ponto/portas/celula.py:639`, `app/ponto/portas/celula.py:807`)

- **Limite probatório das linhas LEI-AKITA:** os endereços e leitores batem com o desenho, mas nomear logs de RED não demonstra seus resultados nesta verificação. O RED de allowlist referido no primeiro commit não prova sozinho reconhecimento de `getattr` ou percurso de arquivos; essas mordidas exigem a execução dos mutantes apropriados. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:176`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:204`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:245`; resultados dos logs: **[não verificado]**)

## 01/11 02:00 · sessao 58e0cd444abd · - · - · origem=Ronald (tmux)

A descrição nova está **curada quanto ao alcance do horizonte e de `--forcar`**: a versão é a maior cujo horizonte alcança a competência; o cache alimenta a impressão, e `forcar` desativa o pulo por impressão atual. (`app/ponto/services/cartorio.py:196`, `app/ponto/services/cartorio.py:214`, `app/ponto/services/cartorio.py:963`, `app/ponto/services/cartorio.py:1170`, `app/ponto/services/cartorio.py:1180`)

O comando seleciona células passadas da empresa e repassa a flag. “Toda célula” descreve esse universo padrão: `--limite` pode recortar colaboradores, `--apply` controla persistência, e uma célula sem dia correspondente na grade ainda é pulada. Isso é uma ressalva de precisão, sem regressão concreta demonstrada no dado. (`app/ponto/management/commands/processar_cartorio.py:49`, `app/ponto/management/commands/processar_cartorio.py:56`, `app/ponto/management/commands/processar_cartorio.py:65`, `app/ponto/services/cartorio.py:1183`)

O teste sem `--forcar` **não estabelece explicitamente** carimbo/impressão/veredito antes da chamada. Entretanto, seria errado concluir que a fixture está necessariamente sem carimbo: as batidas criadas acionam o signal, que chama o cartório antes da criação da FolgaDia e da substituição da ata. A premissa depende implicitamente desse caminho. (`app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:62`, `app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:65`, `app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:74`, `app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:121`, `app/ponto/signals.py:85`, `app/ponto/signals.py:131`, `app/ponto/services/cartorio.py:1738`)

Execução Django, efetivo carimbo da fixture e resultado dos testes: **[não verificado]**. [nao verificado]

## 01/11 01:57 · sessao 296ef6213d0c · GRADE-FOLGA-JUIZ · verificador · origem=Code

**Não libero o pouso nesta pré-verificação: a cura da varredura está implementada, mas sua regressão ainda passa com todos os testes da catraca verdes.** Reintroduzir, somente em memória, o descarte de `precedencia.py` inteiro em `achados()` manteve os 11 casos verdes: o `MORDE_segundo_juiz_dentro_de_precedencia` exercita diretamente `achados_da_fonte`, sem testar o percurso dos arquivos. (`app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:122`, `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:236`)

As referências abaixo são relativas a `app/`. Os testes AST foram exercitados em memória, sem banco, logs ou gravação de arquivos; execução dos testes de integração Django: **[não verificado]**. [nao verificado]

**1. Bloqueios anteriores e mordida dos testes** [nao verificado]

| Item | Situação nesta cópia |
|---|---|
| **(a) `getattr` e cópia literal** | **Curado.** A catraca reconhece `getattr` literal e a cópia do predicado real é exigida pelo teste. Desativar esse reconhecimento deixou vermelhos os testes de cópia literal e leitura sem comparação. (`ponto/tests/test_catraca_folga_lancada_um_juiz.py:54`, `ponto/tests/test_catraca_folga_lancada_um_juiz.py:204`, `ponto/tests/test_catraca_folga_lancada_um_juiz.py:230`) |
| **(a) varrer `precedencia.py` fora do juiz** | **Parcialmente curado.** O código atual exclui somente as três funções autorizadas; porém excluir novamente o arquivo inteiro no percurso mantém os testes verdes. Falta a mordida nesse percurso. (`ponto/tests/test_catraca_folga_lancada_um_juiz.py:113`, `ponto/tests/test_catraca_folga_lancada_um_juiz.py:129`, `ponto/tests/test_catraca_folga_lancada_um_juiz.py:241`) |
| **(c) JUIZES.md** | **Curado.** Registra juiz, carregador, predicado e as duas filas; coincide com o registro vivo. (`docs/JUIZES.md:84`, `core/juizes.py:192`) |
| **Campo “juízes novos”** | O número correto desta obra é **1**, conforme a nova entrada e o corte. A alteração do campo na mensagem de entrega/commit não está disponível para conferência: **[não verificado]**. (`core/juizes.py:192`, `docs/CORTES.md:416`) |
| **Pauta DP** | **Corrigida no texto do cartório:** declara A0×C=0 e retira a conclusão de Pauta DP. O BACKLOG também cancela “comp09=8”, mas conserva textos anteriores sobre Pauta DP. Valores efetivos da comparação: **[não verificado]**. (`ponto/services/cartorio.py:180`, `docs/BACKLOG.md:796`, `docs/BACKLOG.md:795`, `docs/BACKLOG.md:860`) |
| **Integridade FM/TXT** | O teste agora compara a linha inteira de FM e todas as linhas de exportação, além do hash/conteúdo do TXT. Isso corrige a insuficiência da asserção; execução e hashes reais: **[não verificado]**. (`ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:106`, `ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:118`, `ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:119`) |

A mordida da catraca ficou assim:

| `MORDE_` | Resultado da conferência |
|---|---|
| `filtro_pessoa_e_data` | Vermelho ao desativar reconhecimento do filtro. (`ponto/tests/test_catraca_folga_lancada_um_juiz.py:189`) |
| `apelido_e_manager_relacionado` | Vermelho removendo apelidos de import ou reconhecimento dos managers relacionados. (`ponto/tests/test_catraca_folga_lancada_um_juiz.py:194`) |
| `predicado_da_origem` | Exige detectar o atributo direto; mutação específica executada: **[não verificado]**. (`ponto/tests/test_catraca_folga_lancada_um_juiz.py:200`) |
| `copia_literal_do_predicado_do_juiz` | Vermelho sem reconhecimento de `getattr`. (`ponto/tests/test_catraca_folga_lancada_um_juiz.py:204`) |
| `copia_literal_do_carregador_do_juiz` | Vermelho desativando reconhecimento do filtro. (`ponto/tests/test_catraca_folga_lancada_um_juiz.py:215`) |
| `apelido_de_modulo` | Vermelho retirando reconhecimento de `FolgaDia` no meio da cadeia. (`ponto/tests/test_catraca_folga_lancada_um_juiz.py:223`) |
| `leitura_da_origem_sem_comparar` | Vermelho sem reconhecimento de `getattr`. (`ponto/tests/test_catraca_folga_lancada_um_juiz.py:230`) |
| `segundo_juiz_dentro_de_precedencia` | Morde exclusão no analisador da fonte; **não morde exclusão no percurso real dos arquivos**. (`ponto/tests/test_catraca_folga_lancada_um_juiz.py:236`) |

Não cabe afirmar que cada `MORDE_` de integração ficará vermelho para qualquer reversão: os dois builders já erravam em sentidos diferentes. O próprio teste documenta que calendário alheio mordia o builder por turno, enquanto a manual no vínculo velho mordia o legacy; cada caso precisa ser associado à regressão específica. (`escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:20`)

Os testes adicionais cobram a retirada do portão na grade, inclusive `simular_dia`, e a relavra da ata da 09 com `--forcar`; suas execuções e mutações nesta cópia permanecem **[não verificado]**. (`escala/tests/test_grade_folga_vale_em_toda_competencia.py:74`, `escala/tests/test_grade_folga_vale_em_toda_competencia.py:79`, `escala/tests/test_grade_folga_vale_em_toda_competencia.py:84`, `ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:101`)

**2. Quem escreve e quem lê**

**FolgaDia e seus campos**

| Valor | Escritores e leitores localizados |
|---|---|
| Estrutura | `escala_colaborador` tem CASCADE; `data` identifica o dia; `calendario_origem` é nullable/SET_NULL. Unicidade é vínculo+data, não pessoa+data. (`escala/models.py:1553`, `escala/models.py:1556`, `escala/models.py:1557`, `escala/models.py:1571`) |
| Foto de calendário | `gerar_foto_folgas` lê manuais, apaga a foto de calendário selecionada e cria vínculo/data/origem. (`escala/folgas.py:165`, `escala/folgas.py:169`, `escala/folgas.py:173`) |
| Foto manual | `lancar_folga` consulta origem, passa pelo veto e cria por vínculo/data; `_foto_do_dia` consulta, apaga/cria ou altera apenas o tipo informativo. (`escala/folgas.py:254`, `escala/folgas.py:258`, `escala/folgas.py:259`, `escala/folgas.py:274`, `escala/folgas.py:280`, `escala/folgas.py:284`, `escala/folgas.py:289`) |
| Reparentamento | Lê origem, vínculo, data e ocupação; altera `escala_colaborador` pela porta existente. O comando delega à porta. (`escala/folgas.py:405`, `escala/folgas.py:418`, `escala/folgas.py:435`, `escala/folgas.py:453`, `escala/management/commands/reparentar_folgas.py:28`) |
| Universo da pessoa | O carregador consulta todos os vínculos da pessoa na janela e preserva todas as candidatas por data; o predicado lê origem e identidade do vínculo. (`ponto/precedencia.py:206`, `ponto/precedencia.py:208`, `ponto/precedencia.py:237`, `ponto/precedencia.py:239`) |
| Foto por vínculo/gerador | `folgas_do_periodo` carrega vínculo/data; `eh_dia_trabalho_calculado` consome essa foto. (`escala/alimentacao.py:60`, `escala/alimentacao.py:66`, `escala/models.py:1251`, `escala/models.py:1280`, `escala/models.py:1327`) |
| Cadastro e diagnóstico | Defeito mensal, divergência calendário×foto, Plano de Folgas, toggle, diagnóstico do vínculo e limbo leem a foto por vínculo. (`escala/models.py:1220`, `escala/folgas.py:101`, `escala/views_plano_folgas.py:60`, `escala/views_plano_folgas.py:112`, `ponto/services/diagnostico_escala.py:102`, `relatorios/furos.py:112`) |
| Vigias | Fase lê a foto do vínculo ativo; C5 itera toda FolgaDia e confronta cobertura/célula. (`escala/services/fase_conflitante.py:74`, `chamados/management/commands/auditar_invariantes_chamados.py:1532`) |

Não localizei novo escritor direto de FolgaDia fora da porta; o registro autoriza `escala/folgas.py`. CASCADE/SET_NULL continuam sendo efeitos dos relacionamentos existentes. (`core/portas.py:466`, `escala/models.py:1554`, `escala/models.py:1558`)

**Vínculo entregue ao juiz em cada sítio**

| Sítio | Feed |
|---|---|
| Precedência | `vinculo_do_dia`, com células e vínculos; completa geradoras faltantes antes da escolha. (`ponto/precedencia.py:326`, `ponto/precedencia.py:350`, `ponto/precedencia.py:357`) |
| Veto de lançamento | `escala_vigente(colab,d)`. (`ponto/precedencia.py:505`) |
| Declaração | `_ec_do_dia` → `escala_vigente`. (`ponto/services/declarar_dia.py:56`, `ponto/services/declarar_dia.py:74`) |
| Rejulgar ausência em folga | `escala_vigente(cid,d)`. (`ponto/management/commands/rejulgar_ausencia_em_folga.py:65`) |
| Grade por turno | Mapa produzido por `vinculo_do_dia`; overlay recebe `esc_por_dia.get`. (`escala/utils.py:1262`, `escala/utils.py:1268`, `escala/utils.py:1794`) |
| Grade legacy | Segmenta pelo juiz do vínculo; passa o vínculo do segmento ao builder e retém apenas seus dias. O `lambda _: esc` responde nesse recorte. (`escala/utils.py:972`, `escala/utils.py:995`, `escala/utils.py:999`, `escala/utils.py:770`) |
| Calendário | Chama `fatos_do_periodo` sem a antiga substituição `{d: True}`; usa o carregador da pessoa. (`colaboradores/services/calendario.py:617`, `ponto/precedencia.py:281`) |

`escala_vigente` carrega a célula e delega a `vinculo_do_dia`; a validade da geradora fica na autoridade existente. (`escala/servico_jornada.py:46`, `escala/servico_jornada.py:57`, `escala/alimentacao.py:237`)

**Tipo da grade, ata e célula**

O `tipo_dia` nasce nos builders; o overlay curado só escreve no dict em memória, depois da resposta do juiz. Ausências ainda podem substituir o tipo por `falta`/`ausencia`. (`escala/utils.py:727`, `escala/utils.py:1751`, `escala/utils.py:1124`, `escala/utils.py:1126`, `escala/utils.py:1182`, `escala/utils.py:1188`)

O cartório lê a grade, monta `ata.tipo_dia`, entrega a ata à lavra existente e carimba. A porta grava ata/veredito/status e salva junto com os carimbos; save e evento compartilham `atomic`. A precedência entra como chave da mesma ata pela sua operação existente. (`ponto/services/cartorio.py:947`, `ponto/services/cartorio.py:583`, `ponto/services/cartorio.py:1580`, `ponto/services/cartorio.py:1600`, `ponto/portas/celula.py:768`, `ponto/portas/celula.py:807`, `ponto/portas/celula.py:639`, `ponto/portas/celula.py:819`)

Leitores externos ao diff localizados:

| Origem | Leitores |
|---|---|
| Ata/célula | Juiz de cobrança, leitor da célula e tripwire. (`chamados/juizes.py:408`, `escala/services/leitor_celula.py:345`, `ponto/management/commands/tripwire_celulas.py:342`) |
| Grade — cobrança/ação | Materialização, declaração textual, emissão de disputa, furos diários, supra-juiz, worklist, propositor e veredito humano. (`chamados/services/materializacao.py:225`, `chamados/services/declaracao_texto.py:90`, `chamados/services/disputa_emissao.py:68`, `escala/services/furos_diarios.py:39`, `ponto/supra_juiz.py:222`, `ponto/worklist.py:19`, `ponto/propositor.py:210`, `ponto/services/veredito_celula.py:221`) |
| Grade — apuração/tela | Status, espelho, simulação, bordas, órfãs, furos e calculadores. (`ponto/status_dia.py:146`, `ponto/services/espelho.py:690`, `ponto/services/simular_dia.py:12`, `ponto/services/bordas_realizado.py:74`, `escala/orfa_classe.py:65`, `relatorios/furos.py:147`, `inteligencia/calculadores.py:141`) |
| Financeiros | Dia pago, fechamento, causas e exportação. (`ponto/services/dia_pago.py:386`, `ponto/services/fechamento.py:1011`, `folha/causas.py:100`, `folha/export.py:1071`) |
| Instrumentos | Autópsia e mapa de divergência. (`ponto/management/commands/e6_piso_autopsia.py:13`, `ponto/management/commands/mapa_divergencia.py:131`) |

A completude contra acessos dinâmicos não identificáveis pelo censo textual é **[não verificado]**.

**REGRA_VERSOES:** a declaração escreve a tupla; `versao_para` lê seus horizontes pela competência; `_regra_do_dia` alimenta os dois ramos do cartório; `impressao_insumos` incorpora a versão ao hash. (`ponto/services/cartorio.py:111`, `ponto/services/cartorio.py:194`, `ponto/services/cartorio.py:210`, `ponto/services/cartorio.py:963`, `ponto/services/cartorio.py:1015`, `ponto/services/cartorio.py:1168`, `ponto/services/cartorio.py:368`)

**3. ALLOWLIST e sítios fora dela**

Não encontrei entrada que oculte outro juiz pessoa-dia sob motivo falso: são escritores, fotos por vínculo, perguntas mensais ou escolha de frase sobre resultado já julgado. O gerador é uma exceção relevante, mas está explicitamente marcado como **INCOMPLETO / GRADE-FOLGA-GERADOR**. (`ponto/tests/test_catraca_folga_lancada_um_juiz.py:137`, `ponto/tests/test_catraca_folga_lancada_um_juiz.py:158`, `ponto/tests/test_catraca_folga_lancada_um_juiz.py:160`, `ponto/tests/test_catraca_folga_lancada_um_juiz.py:165`)

A entrada de fase é coerente: o arquivo lê a foto do vínculo ativo e compara foto×âncora; a entrada declara sua fila própria. `test_os_sitios_curados_nao_voltam` cobra comando, declaração e ausência de `_folga_avulsa`, sem exigir cura de fase. (`escala/services/fase_conflitante.py:74`, `escala/services/fase_conflitante.py:80`, `ponto/tests/test_catraca_folga_lancada_um_juiz.py:142`, `ponto/tests/test_catraca_folga_lancada_um_juiz.py:182`)

**Sim, há leitor fora do juiz e fora da ALLOWLIST:** C5 percorre FolgaDia, usa cobertura por vínculo e acusa célula de trabalho sem distinguir origem. É a fila nominal GRADE-FOLGA-C5, expressamente declarada como limite da catraca. (`chamados/management/commands/auditar_invariantes_chamados.py:1532`, `chamados/management/commands/auditar_invariantes_chamados.py:1535`, `chamados/management/commands/auditar_invariantes_chamados.py:1548`, `ponto/tests/test_catraca_folga_lancada_um_juiz.py:21`)

**4. Resíduos da retirada de fase**

Nos documentos nominalmente pedidos, não achei afirmação residual de migração: JUIZES declara foto/fila; MAPA-JUIZES, MAPA e `core/juizes.py` registram o juiz; ARQUITETURA mantém o cron de fase. O docstring dos testes da obra declara a retirada, e os testes próprios de fase continuam verificando foto×âncora. (`docs/JUIZES.md:89`, `docs/MAPA-JUIZES.md:52`, `docs/MAPA.md:107`, `core/juizes.py:192`, `docs/ARQUITETURA.mmd:62`, `ponto/tests/test_grade_folga_juiz_sitios.py:13`, `escala/tests/test_fase_conflitante.py:60`)

**Sobrou texto fora desse conjunto:** BACKLOG ainda afirma que `fase_conflitante:74` pergunta ao juiz; CORTES conserva a enumeração original. A linha seguinte do BACKLOG registra o desmembramento. Cabe atualizar o registro vivo e explicitar a supersessão, sem trazer a cura de fase de volta à obra. (`docs/BACKLOG.md:796`, `docs/BACKLOG.md:797`, `docs/CORTES.md:416`)

**5. Classes de reprovação**

| Classe | Veredito |
|---|---|
| Segundo juiz | Os sítios curados delegam ao juiz; C5 e gerador permanecem filas declaradas. (`ponto/precedencia.py:505`, `ponto/services/declarar_dia.py:74`, `ponto/management/commands/rejulgar_ausencia_em_folga.py:65`, `ponto/tests/test_catraca_folga_lancada_um_juiz.py:21`, `ponto/tests/test_catraca_folga_lancada_um_juiz.py:160`) |
| Segundo escritor | Não aparece novo escritor persistente fora da porta; overlay altera memória e cartório usa a lavra existente. (`escala/utils.py:1126`, `ponto/services/cartorio.py:1580`, `ponto/portas/celula.py:807`) |
| Relógio fora da borda | O trecho novo do comando lê a data na borda; carregador, predicado e overlay novos não consultam relógio. (`ponto/management/commands/rejulgar_ausencia_em_folga.py:52`, `ponto/precedencia.py:194`, `ponto/precedencia.py:222`, `escala/utils.py:1103`) |
| Derivação paralela | A grade passa a chamar o juiz; calendário compartilha `trabalho_do_dia` entre classificação e publicação. (`escala/utils.py:1124`, `colaboradores/services/calendario.py:126`, `colaboradores/services/calendario.py:840`) |
| Selo sem mordida | **Procedente e bloqueante:** exclusão integral de `precedencia.py` no percurso passa verde. (`ponto/tests/test_catraca_folga_lancada_um_juiz.py:129`, `ponto/tests/test_catraca_folga_lancada_um_juiz.py:236`) |
| Ordem de trava | Não há nova aquisição de trava no trecho alterado; a persistência continua pelo `atomic` existente. Ausência de corrida em execução: **[não verificado]**. (`ponto/services/cartorio.py:111`, `ponto/portas/celula.py:639`) |
| Pouso antes do pré-requisito | O próprio cartório exige ensaio `--apply` antes do pouso. Ensaio, reversão e resultado específico de 303970f1: **[não verificado]**. (`ponto/services/cartorio.py:174`) |

**6. Exportada, escala/vínculo, motor e alcance do rejulgamento**

O diff fornecido não introduz escritor de escala/vínculo nem alteração em `motor_calculo_v2.py`; a cura da grade consulta FolgaDia e modifica o resultado em memória. A cadeia examinada do cartório grava célula/ata, carimbos, veredito/status e evento pela porta existente. (`escala/utils.py:1115`, `escala/utils.py:1124`, `escala/utils.py:1126`, `ponto/services/cartorio.py:1580`, `ponto/services/cartorio.py:1600`, `ponto/portas/celula.py:640`)

**Ata/célula da exportada pode mudar, pelo desenho autorizado.** Não há nova escrita direta de FechamentoMensal/TXT nesse caminho; os testes cobram sua integridade completa. A confirmação operacional dessa integridade é **[não verificado]**. (`docs/ADR/0003-celula-tres-naturezas.md:25`, `ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:118`, `ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:119`)

A v3 muda a versão da competência **10 em diante**; 09 permanece na v1 e 08 sem versão. Isso altera a impressão e provoca nova análise das células elegíveis; não constitui portão de escrita. (`ponto/services/cartorio.py:111`, `ponto/services/cartorio.py:212`, `ponto/services/cartorio.py:1168`, `ponto/tests/test_o219_versao_da_regra.py:85`)

Com `--forcar`, o comando ignora o pulo por carimbo atual e seleciona **todas as células passadas da empresa**, inclusive abaixo do horizonte, sem limitar à 09. Sem `--forcar`, a 09 não muda de impressão **por esta v3**, mas células sem carimbo ou com outros insumos alterados continuam elegíveis. A frase “a 09 só se rejulga com --forcar” é ampla demais e deve ser corrigida. (`ponto/management/commands/processar_cartorio.py:49`, `ponto/management/commands/processar_cartorio.py:61`, `ponto/services/cartorio.py:1174`, `ponto/services/cartorio.py:1178`, `ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:122`)

**Bloqueia:** prender com teste a varredura real de `precedencia.py`. **Corrigir junto:** resíduos do BACKLOG e a descrição absoluta de `--forcar`. **Fila:** fase, C5 e gerador. **Não procede:** exigir cura de fase nesta obra, tratar relavra autorizada da ata da 09 como escrita proibida do gravado, ou afirmar que falta o juiz em JUIZES.md. (`ponto/tests/test_catraca_folga_lancada_um_juiz.py:129`, `docs/BACKLOG.md:796`, `ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:122`, `docs/BACKLOG.md:797`, `ponto/tests/test_catraca_folga_lancada_um_juiz.py:21`, `ponto/tests/test_catraca_folga_lancada_um_juiz.py:160`, `docs/ADR/0003-celula-tres-naturezas.md:25`, `docs/JUIZES.md:84`)

## 01/11 01:55 · sessao da6e67fc3dfd · - · - · origem=Ronald (tmux)

**Cadeia de escrita/leitura**

- `tipo_dia` nasce nos builders: single vínculo em `app/escala/utils.py:727` e `:752`; por turno em `:1751` e `:1757`; legacy recorta segmentos pelo `vinculo_do_dia` em `:972`, `:995` e `:999`, transportando o campo em `:1018`. O overlay curado escreve apenas o dict em memória (`:1126`), após consultar o juiz (`:1124`); ausências podem sobrescrever para `falta`/`ausencia` em `:1182`/`:1188`.
- O cartório lê a grade em `app/ponto/services/cartorio.py:947`, transporta `tipo_dia` para a ata em `:583`, entrega a ata a `CelulaDia.lavrar_veredito` em `:1059` e `:1580`, e carimba pelo escritor existente em `:1600`.
- O método do model só delega: `app/escala/models.py:1787`. A porta escreve `cel.ata` em `app/ponto/portas/celula.py:741`/`:768`; `carimbar` salva campos e lavra juntos em `:787`/`:807`; save e outbox compartilham atomic em `:639`. A precedência tem chave própria na mesma ata: preservação em `:732`; escritor `carimbar_precedencia` em `:811`/`:819`, chamado por `app/ponto/services/cartorio.py:1695`.
- Nenhuma nova ordem de locks foi introduzida pelo diff fornecido; o trecho novo do cartório é declaração de versão e documentação (`app/ponto/services/cartorio.py:111`, `:168`). Prova operacional de ausência de corrida: **[não verificado]**.

**Leitores externos ao diff**

- Ata diretamente: juiz de cobrança `app/chamados/juizes.py:408`; leitor da célula `app/escala/services/leitor_celula.py:332`/`:345`, que publica o campo em `:360`; vigia `app/ponto/management/commands/tripwire_celulas.py:342`.
- Grade/dict de dia: `app/escala/orfa_classe.py:65`; `app/relatorios/furos.py:147`/`:151`/`:218`/`:237`; `app/chamados/services/materializacao.py:225`; `app/chamados/services/declaracao_texto.py:90`; `app/chamados/services/disputa_emissao.py:68`; `app/escala/services/furos_diarios.py:39`; `app/ponto/status_dia.py:146`; `app/ponto/worklist.py:19`; `app/ponto/propositor.py:36`/`:210`; `app/inteligencia/calculadores.py:46`/`:141`; `app/ponto/services/veredito_celula.py:221`; `app/ponto/services/bordas_realizado.py:74`; `app/ponto/services/simular_dia.py:12`; `app/ponto/services/espelho.py:690`; `app/ponto/supra_juiz.py:222`; `app/folha/causas.py:40`/`:41`/`:100`.
- Leitores financeiros continuam existindo: `app/ponto/services/dia_pago.py:386`/`:393`/`:398`; `app/ponto/services/fechamento.py:1011`/`:1015`; `app/folha/export.py:1071`/`:1075`/`:1080`. Logo, alterar `tipo_dia` pode alterar resultados de uma futura projeção financeira; não equivale a escrever o gravado pelo cartório (`app/ponto/services/fechamento.py:1112`).
- Instrumentos adicionais leem o campo em `app/ponto/management/commands/e6_piso_autopsia.py:13` e `app/ponto/management/commands/mapa_divergencia.py:131`/`:143`/`:155`/`:229`. Ocorrências em templates não apareceram no censo textual; completude contra acesso dinâmico: **[não verificado]**.

**REGRA_VERSOES e horizonte**

- Escritor único é a declaração `app/ponto/services/cartorio.py:111`; validação em `:183`; leitor `versao_para` em `:194`/`:212`, usando a competência de `janela_atual` (`:210`).
- A versão integra a impressão em `:368`; `_regra_do_dia` fornece-a aos dois ramos (`:963`, `:1015`, `:1168`). A v3 alcança competência 10 em diante; 09 conserva v1 e 08 fica sem versão, conforme asserções de `app/ponto/tests/test_o219_versao_da_regra.py:85`.
- `--forcar` ignora o pulo por impressão igual (`app/ponto/services/cartorio.py:1020`, `:1178`). O comando seleciona **todas as células passadas da empresa**, sem recorte de competência (`app/ponto/management/commands/processar_cartorio.py:49`, `:61`, `:65`). Portanto, com `--forcar`, abaixo do horizonte não significa só 09: qualquer célula passada selecionada pode ser rejulgada.
- Sem `--forcar`, a 09 não recebe mudança de impressão **por esta v3**, mas células sem carimbo ou com outros insumos alterados continuam elegíveis (`app/ponto/services/cartorio.py:1174`). A frase absoluta “a 09 só se rejulga com --forcar” do teste é mais ampla que o código (`app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:122`).
- Não confirmei falha desse teste: as batidas da fixture (`app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:62`) disparam julgamento por signal (`app/ponto/signals.py:131`/`:135`, `:85`/`:93`), podendo inicializar o carimbo antes do update da ata (`app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:74`). Execução do teste e mutante: **[não verificado]**.

**Exportada, prerequisitos e “corrigir junto”**

- O caminho examinado reescreve célula/ata, carimbos, status, protestos e emite outbox; não chama o escritor de `FechamentoMensal` ou a porta REGEN (`app/ponto/services/cartorio.py:1578`, `:1600`; `app/ponto/portas/celula.py:787`). A fixture cobra linha inteira de FM e todas as exportações intactas (`app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:106`, `:118`, `:119`). Verde real e hashes de produção: **[não verificado]**.
- O diff fornecido não altera motor nem escritores de vínculo; a chamada nova da grade consulta o juiz e muda memória (`app/escala/utils.py:1115`, `:1124`, `:1126`). Identidade binária com main, sem repositório Git nesta cópia: **[não verificado]**.
- Ensaio `--apply` na sombra, reversão e impacto **antes do pouso** são prerequisito explícito da própria declaração (`app/ponto/services/cartorio.py:174`/`:175`). Os anexos narram evidência antiga; ensaio da cópia 303970f1, números e reversão: **[não verificado]**.
- Pauta DP foi corrigida no texto vivo para A0×C=0 (`app/ponto/services/cartorio.py:180`/`:181`); o valor real e hashes TXT não são demonstráveis pelos arquivos examinados: **[não verificado]**.
- Registro efetivamente conta juiz novo (`app/core/juizes.py:192`) e há corte (`app/docs/CORTES.md:416`). O campo LEI-AKITA da mensagem de entrega/commit não está acessível; sua correção é **[não verificado]**.
- Resíduos vivos fora da lista nominal pedida: `app/docs/BACKLOG.md:796` e o corte histórico `app/docs/CORTES.md:416` ainda dizem que `fase_conflitante` pergunta ao juiz. A retirada decidida pelo usuário prevalece, mas o BACKLOG precisa acompanhar o novo escopo (`app/ponto/tests/test_grade_folga_juiz_sitios.py:13`).

## 01/11 01:55 · sessao 201524adb215 · - · - · origem=Ronald (tmux)

Catraca implementada, porém **a exclusão do arquivo inteiro `precedencia.py` ainda pode voltar sem teste vermelho**. `test_MORDE_segundo_juiz_dentro_de_precedencia` chama diretamente `achados_da_fonte`; não exerce o percurso `achados`. Em mutação somente em memória, inserir `if rel == 'ponto/precedencia.py': continue` nesse percurso deixou os **11 testes verdes**. Referências: `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:122`, `:130`, `:236`, `:241`. Esse é um bloqueio de mordida, embora o código atual efetivamente varra o arquivo (`:113`, `:115`, `:131`).

Verificação em memória, com AST e `unittest.TestCase` substituindo somente `SimpleTestCase`, sem importar Django, banco ou logs:

| MORDE | Referência | Mutação que o deixou vermelho |
|---|---|---|
| filtro pessoa/data | catraca:189 | Desativar reconhecimento de filtro pessoa/data |
| apelido e manager | catraca:194 | Remover reconhecimento de import com apelido; ou esvaziar `REL` |
| predicado da origem | catraca:200 | A asserção exige detectar `fd.calendario_origem_id`; regressão específica não executada [não verificado] |
| cópia literal do predicado | catraca:204 | Desativar `_getattr_literal` |
| cópia literal do carregador | catraca:215 | Desativar filtro pessoa/data |
| apelido de módulo | catraca:223 | Remover reconhecimento do modelo no meio da cadeia |
| origem sem comparação | catraca:230 | Desativar `_getattr_literal` |
| segundo juiz dentro de precedência | catraca:236 | Excluir precedência de `achados_da_fonte`; **não** excluir do percurso `achados` |

As funções e asserções estão em `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:54`, `:62`, `:95`, `:105`, `:189`, `:194`, `:200`, `:204`, `:215`, `:223`, `:230`, `:236`. Os testes atuais passaram e `achados == ALLOWLIST == 22`; resultado da execução em memória [não verificado por artefato persistido].

A ALLOWLIST está coerente com a retirada de fase: `medir` lê a foto do vínculo ativo, compara foto e âncora e conta divergências; nenhuma chamada ao juiz aparece nesse fluxo. A entrada declara expressamente sua fila própria. `test_os_sitios_curados_nao_voltam` prende apenas comando, declaração e lápide `_folga_avulsa`, sem exigir cura de fase. Referências: `app/escala/services/fase_conflitante.py:71`, `:74`, `:80`, `:83`; `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:142`, `:182`.

Nas entradas inspecionadas, não achei outro juiz pessoa-dia oculto: há fotos por vínculo, contadores mensais e escritores. O gerador continua decidindo o dia pela foto do próprio vínculo, mas isso está explicitamente marcado como incompleto/fila. Referências: `app/escala/alimentacao.py:49`, `:66`; `app/escala/models.py:1212`, `:1220`, `:1280`; `app/escala/views_plano_folgas.py:60`, `:63`, `:70`; `app/ponto/services/diagnostico_escala.py:81`, `:102`; `app/relatorios/furos.py:112`; `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:160`.

C5 permanece fora do juiz e fora da ALLOWLIST: itera toda FolgaDia, verifica cobertura por algum vínculo e acusa célula de trabalho sem distinguir origem. O limite está declarado na catraca como GRADE-FOLGA-C5. Referências: `app/chamados/management/commands/auditar_invariantes_chamados.py:1532`, `:1534`, `:1535`, `:1540`, `:1542`; `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:19`.

Os documentos expressamente pedidos estão coerentes: JUIZES registra fase lendo foto em fila; MAPA-JUIZES/MAPA/core registram o juiz sem atribuir migração a fase; ARQUITETURA só mantém os nós do cron. Referências: `app/docs/JUIZES.md:84`, `:88`, `:90`; `app/docs/MAPA-JUIZES.md:52`; `app/docs/MAPA.md:107`; `app/core/juizes.py:188`, `:192`; `app/docs/ARQUITETURA.mmd:62`, `:112`.

**Sobrou resíduo fora desse conjunto:** BACKLOG ainda afirma `fase_conflitante:74` perguntando ao juiz; CORTES também enumera fase entre os migrados. A linha seguinte do BACKLOG registra o desmembramento, tornando necessário esclarecer o texto anterior. Referências: `app/docs/BACKLOG.md:796`, `:797`; `app/docs/CORTES.md:416`. O docstring dos testes da obra já declara corretamente a retirada; os testes antigos de fase verificam foto/âncora, sem depender dessa cura. Referências: `app/ponto/tests/test_grade_folga_juiz_sitios.py:13`; `app/escala/tests/test_fase_conflitante.py:60`, `:72`, `:93`.

## 01/11 01:54 · sessao 479951fb7264 · - · - · origem=Ronald (tmux)

Censo de `FolgaDia` e feeds do juiz:

| Valor / função | Quem escreve / quem lê |
|---|---|
| Modelo | `escala_colaborador` FK CASCADE, `data`, `calendario_origem` FK SET_NULL/nulo: `app/escala/models.py:1553-1559`; unicidade vínculo+data: `:1571`. |
| Foto do calendário | `gerar_foto_folgas`: lê manuais `app/escala/folgas.py:165-168`, apaga calendário `:169-172`, cria vínculo/data/origem `:173-175`. |
| Foto manual | `lancar_folga`: consulta/origem `app/escala/folgas.py:254-255`, veto `:258`, get_or_create vínculo/data `:259-260`; `_foto_do_dia`: consulta/origem `:274-275`, delete `:280`, create `:284`, mudança só de tipo `:288-289`. |
| Reparentamento | `app/escala/folgas.py:405-418` lê origem/vínculo/data/ocupação; único update de vínculo `:435-436`. Seleção em lote `:453-457`; command `app/escala/management/commands/reparentar_folgas.py:28-34` delega à porta. |
| Carregador da pessoa | `app/ponto/precedencia.py:206-208` lê vínculo pela pessoa e janela, preserva todas por data; juiz `:212-218`; único predicado origem/vínculo `:235-239`. |
| Foto por vínculo | `app/escala/alimentacao.py:66-70` lê vínculo/data, arredondando ao mês `:60-64`; consumidor gerador `app/escala/models.py:1251-1259`, `:1280-1282`, `:1327-1334`. |
| Defeito/cadastro | `app/escala/models.py:1220-1221`; divergência calendário/foto/origem `app/escala/folgas.py:101-104`; Plano de Folgas `app/escala/views_plano_folgas.py:60-76`; toggle lê origem para recusa e delega escrita `:112-137`; diagnóstico `app/ponto/services/diagnostico_escala.py:102-104`; limbo `app/relatorios/furos.py:112-115`. |
| Vigia fase | `app/escala/services/fase_conflitante.py:71-80` lê foto do vínculo ativo, compara `_foto_diz` e `_ancora_diz(ec,d)`; não chama o juiz. |
| Leitor fora juiz/ALLOWLIST | C5 em `app/chamados/management/commands/auditar_invariantes_chamados.py:1532-1536` itera toda FolgaDia, aceita cobertura por outro vínculo sem distinguir origem; acusa célula de trabalho `:1540-1550`. É segundo derivador já declarado como fila na catraca `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:21-24`. |

Não apareceu escritor direto novo fora de `escala/folgas.py` na busca dos símbolos FolgaDia/managers; o registro da porta permite só esse arquivo: `app/core/portas.py:466`. Exclusão de vínculo/calendário também pode alterar FolgaDia por CASCADE/SET_NULL do modelo, sem mudança desta obra: `app/escala/models.py:1553-1559`.

Feeds entregues ao juiz:

- Precedência: `vinculo_do_dia` em `app/ponto/precedencia.py:350`, aplicado `:357`; completa geradora ausente da lista `:326-330`.
- Veto: `escala_vigente(colab,d)` em `app/ponto/precedencia.py:505`.
- Declaração: `_ec_do_dia` → `escala_vigente` em `app/ponto/services/declarar_dia.py:56-58`; juiz `:74-75`; origem restante escolhe frase sobre folga já aceita `:112-118`.
- Command: `escala_vigente(cid,d)` em `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:64-65`.
- `escala_vigente` carrega célula e consulta o juiz do vínculo em `app/escala/servico_jornada.py:46-58`; validade da geradora e fallback ficam em `app/escala/alimentacao.py:237-254`.
- Grade por turno: `vinculo_do_dia` → mapa em `app/escala/utils.py:1262-1269`, overlay `:1794`.
- Grade legacy: segmenta pelo juiz `app/escala/utils.py:972-982`; chama builder com `e` `:995`; builder passa `lambda _: esc` `:770`; só retém dias do segmento escolhido `:997-1001`. Busca de chamadores de produção de `montar_grade_prevista` achou só esse legacy `:995`.
- Calendário usa `fatos_do_periodo` sem a antiga foto `{d: True}` em `app/colaboradores/services/calendario.py:617-619`; portanto recebe o mesmo juiz e carregador da pessoa `app/ponto/precedencia.py:281`.

As entradas da ALLOWLIST para foto, cadastro e escritor correspondem ao que os sítios fazem nas linhas acima; gerador é a dívida nominal `GRADE-FOLGA-GERADOR`, enquanto C5 permanece fora da cobertura e nominalmente declarado na própria catraca: `app/ponto/tests/test_catraca_folga_lancada_um_juiz.py:174-175`, `:21-24`.

## 01/11 01:42 · sessao 0c6764fddfce · CONTENCAO-QUE-MORDE-V2 · verificador · origem=Code

**Parecer: bloqueia o P1.** Há autorizações sem registro persistido, corridas entre consulta e reserva, semeadura incompleta e certificado que pode liberar sem comprovar a suite (`bin/disjuntor.py:318`, `bin/disjuntor.py:1142`, `bin/disjuntor.py:1372`, `bin/disjuntor.py:1264`). Execução integral dos selos, instalação efetiva dos hooks e ocorrência desses caminhos em produção: **[nao verificado]**.

1. **IDENTIDADE/RESERVA — bloqueia: autoriza mesmo se não conseguir gravar.** `_com_estado` executa a decisão, captura a falha de persistência e retorna o resultado original. As duas portas novas não conferem `gravou`: Agent pode abrir sem reservar vaga/volta; wrapper pode abrir sem consumir volta. A proteção já existente no B3 confere a gravação, mas não foi aplicada às novas autorizações (`bin/disjuntor.py:305`, `bin/disjuntor.py:318`, `bin/disjuntor.py:323`, `bin/disjuntor.py:1488`, `bin/disjuntor.py:1397`, `bin/disjuntor.py:1048`).

2. **FALHA RESTRITIVA — bloqueia: exceção anterior à porta libera Agent não final.** Um estado ilegível pode levantar na leitura de `pretool`, antes de `porta_agent`. O tratamento externo nega somente o ramo B3; para um construtor não final, termina liberando sem verificar modelo, papel, vivos ou voltas (`bin/disjuntor.py:1965`, `bin/disjuntor.py:1992`, `bin/disjuntor.py:1995`, `bin/disjuntor.py:2005`, `bin/disjuntor.py:2015`).

3. **IDENTIDADE/ORDEM DE TRAVA — bloqueia: B3 e autorização não são uma transição.** Agent consulta B3 numa transação e reserva noutra; o wrapper faz o mesmo. Entre ambas, outro processo pode parar a obra, e a reserva posterior não revalida essa parada. Usar o mesmo arquivo de flock em chamadas separadas não fecha essa corrida (`bin/disjuntor.py:1961`, `bin/disjuntor.py:1465`, `bin/disjuntor.py:1488`, `bin/disjuntor.py:1132`, `bin/disjuntor.py:1142`, `bin/disjuntor.py:1387`).

4. **(11) VIVOS — bloqueia: fotografia fora do flock permite subcontagem.** `txt` e `vivos_tx` são lidos antes da trava. Um processo pode fotografar cinco vivos; outro autorizar o sexto; uma transação seguinte podar a reserva desse sexto após seu lançamento. Quando o primeiro processo entra, sua fotografia ainda tem cinco e a reserva já desapareceu: pode autorizar o sétimo (`bin/disjuntor.py:1457`, `bin/disjuntor.py:1461`, `bin/disjuntor.py:1473`, `bin/disjuntor.py:1475`, `bin/disjuntor.py:1476`).

5. **(11) RESERVAS — bloqueia: poda por substring, sem reconhecer o resultado.** Qualquer objeto que contenha a sequência compacta `"tool_use_id":"<id>"` satisfaz a poda, sem exigir `type=tool_result`. Um resultado JSON válido com espaço após `:` não satisfaz a mesma busca. O leitor de lançamentos recupera apenas agentes assíncronos; síncronos anteriores, sem reserva registrada, não entram nessa recuperação (`bin/disjuntor.py:1473`, `bin/disjuntor.py:1474`, `bin/disjuntor.py:403`, `bin/disjuntor.py:413`).

6. **IDENTIDADE — bloqueia: idempotência acaba quando a reserva é podada.** Agent consulta somente `reservas`; acrescenta `tool_use_id` em `voltas.chaves`, mas não consulta essas chaves. Após a poda, repetir a mesma autorização incrementa novamente. A sequência isolada `a, b, a`, com resultado de `a` antes de `b`, termina com três voltas e chaves `[a,b,a]` (`bin/disjuntor.py:1467`, `bin/disjuntor.py:1474`, `bin/disjuntor.py:1484`, `bin/disjuntor.py:1485`).

7. **(1) SEMENTE — bloqueia: inicialização pelo wrapper congela Agent em zero.** Wrapper e `reabre` chamam `_voltas` sem transcript. A semente retorna `agent=0`, parcial; quando um Agent chega depois, o dicionário existente nunca é completado. Assim, seis voltas anteriores da sessão podem ser ignoradas (`bin/disjuntor.py:1388`, `bin/disjuntor.py:1494`, `bin/disjuntor.py:1346`, `bin/disjuntor.py:1357`, `bin/disjuntor.py:1372`).

   **Também bloqueia:** sidecar ilegível é simplesmente pulado, sem marcar semente parcial. A busca cobre somente `logs/`, embora o wrapper aceite resposta em caminho fornecido pelo chamador. Estado ausente vira `{}`, perdendo reservas e identidades que a semeadura não reconstrói (`bin/disjuntor.py:831`, `bin/disjuntor.py:1348`, `bin/disjuntor.py:1350`, `bin/codex_pergunta.sh:40`, `bin/disjuntor.py:299`).

8. **CHAVE / “só Ronald” — bloqueia: guarda apenas lexical.** A condição normal arquivo **e** linha está implementada, RELATO ilegível mantém ligado, e o wrapper consulta a chave. Porém, a exclusividade depende de encontrar nomes/literais no comando: importar `disjuntor` e chamar `reabre(...)` ou `redesenha(...)` passa sem os argumentos `--reabre`/`--redesenha`. Write direto no estado também não é guardado. As funções não verificam autoridade (`bin/disjuntor.py:176`, `bin/disjuntor.py:169`, `bin/disjuntor.py:1130`, `bin/disjuntor.py:190`, `bin/disjuntor.py:194`, `bin/disjuntor.py:201`, `bin/disjuntor.py:1491`, `bin/disjuntor.py:1502`).

9. **(2) SEGUNDO ERRO-BOBO — bloqueia: `--rearma-b3` contorna a exigência de `--redesenha`.** O rearme limpa `disparado` e arquiva as reprovações, preservando `pede_desenho`. Na decisão seguinte, a obra não disparada é ignorada **antes** de consultar esse pedido; volta a LIVRE sem `--redesenha` (`bin/disjuntor.py:1106`, `bin/disjuntor.py:1108`, `bin/disjuntor.py:1002`, `bin/disjuntor.py:1010`).

   **Fila — formato diferente do contratado:** `consumidas` continua sendo lista de SHA, com `consumidas_tipo` em mapa separado. A regra normal lê ambos, mas não grava `{sha,tipo}` como escrito no REVISADO (`app/docs/contratos/CONTENCAO-QUE-MORDE-V2.txt:12`, `bin/disjuntor.py:1023`, `bin/disjuntor.py:1027`, `bin/disjuntor.py:1162`).

10. **(3) CURA — bloqueia: HEAD ausente não nega incondicionalmente.** Falha ao obter HEAD vira `None`, mas sua ausência só é verificada dentro do laço de PROCEDENTES. Com triagem vazia, passa, apesar da ressalva contratual “cópia sem hash = nega” (`bin/disjuntor.py:1448`, `bin/disjuntor.py:1451`, `bin/disjuntor.py:1276`, `bin/disjuntor.py:1280`, `bin/disjuntor.py:1288`, `app/docs/contratos/CONTENCAO-QUE-MORDE-V2.txt:13`).

   **Fila — alcance mais restritivo:** o gate de cura também atinge verificadores finais sonnet/haiku; não verifica modelo, enquanto o item (3) diz “opus final” (`bin/disjuntor.py:1427`, `bin/disjuntor.py:1469`, `app/docs/contratos/CONTENCAO-QUE-MORDE-V2.txt:13`).

11. **(4) UNIVERSO — bloqueia: porta e coletor reconhecem universos diferentes.** A porta aceita `general-purpose` com papel verificador na descrição. O coletor de reprovações continua exigindo `meta.agentType == verificador`; a entrega REPROVADO daquele agente não alimenta B3 (`bin/disjuntor.py:1191`, `bin/disjuntor.py:1427`, `bin/disjuntor.py:815`, `bin/disjuntor.py:821`).

12. **(5) B1 — bloqueia: permanecem caminhos liberais.**

   - Um agente em `SO_LEITURA` passa mesmo quando já foi encontrado job próprio antigo. O REVISADO não concede essa isenção (`bin/disjuntor.py:460`, `bin/disjuntor.py:466`, `app/docs/contratos/CONTENCAO-QUE-MORDE-V2.txt:15`).
   - Transcript ilegível retorna `disparo=False`; lançamento sem hora legível é ignorado (`bin/disjuntor.py:458`, `bin/disjuntor.py:530`).
   - Cópias são filtradas por `isdir`: uma cópia inacessível pode desaparecer, deixando outra com arquivo novo liberar a avaliação (`bin/disjuntor.py:468`, `bin/disjuntor.py:505`).
   - Job e término são reconhecidos por texto livre. Não se exige chamada Bash `run_in_background`, nem `task-notification`, nem estado terminal; qualquer `<status>[a-z]+</status>` correspondente elimina o job (`bin/disjuntor.py:514`, `bin/disjuntor.py:521`, `bin/disjuntor.py:525`).

13. **(9) CERTIFICADO — bloqueia: emissão e leitura não comprovam a suite.** A CLI pública aceita hashes, rc, recorte e log declarados pelo chamador; um log sintético com `Ran` e `OK` emite certificado. A guarda não protege essa emissão nem Write direto no `.ok`. O leitor exige somente hash e `rc=0`, aceitando arquivo sem `Ran` e sem `OK` (`bin/disjuntor.py:2229`, `bin/disjuntor.py:1226`, `bin/disjuntor.py:1234`, `bin/disjuntor.py:1242`, `bin/disjuntor.py:182`, `bin/disjuntor.py:1935`, `bin/disjuntor.py:1264`).

14. **(9) HASH — bloqueia: leitura parcial pode produzir identidade válida.** `git add --ignore-errors` tem o resultado ignorado; o comando seguinte pode emitir árvore apenas com a parte legível. O gate aceita o sucesso final e o hash, permitindo casar certificado de árvore parcial em vez de negar NAO_SEI (`bin/arvore_hash.sh:31`, `bin/arvore_hash.sh:32`, `bin/disjuntor.py:1220`).

   **Fila — corrida durante a suite:** H0 é calculado antes da trava; H1 depois da execução. Sem snapshot da cópia, A→B→A pode certificar A após testar B; a igualdade das duas pontas não demonstra a árvore executada (`bin/suite.sh:100`, `bin/suite.sh:102`, `bin/suite.sh:111`).

15. **(10) PUSH — bloqueia: três exclusões liberam sem julgamento.** [nao verificado]

   - `--no-merges` exclui instrumento introduzido na resolução de merge, sem essa exceção no REVISADO (`bin/contrato_do_push.py:37`, `app/docs/contratos/CONTENCAO-QUE-MORDE-V2.txt:20`).
   - Um commit novo que remova o próprio portão é tratado como anterior ao portão. Erro de `cat-file` também vira “não existe”, liberando nesse ramo (`bin/contrato_do_push.py:24`, `bin/contrato_do_push.py:41`).
   - Linha malformada é ignorada; entrada vazia devolve sucesso. O pre-push não verifica falha do `cat`; com `SKIP_TESTS`, pode sair verde após julgamento vazio (`bin/contrato_do_push.py:31`, `bin/contrato_do_push.py:33`, `bin/contrato_do_push.py:55`, `bin/pre-push.sh:7`, `bin/pre-push.sh:11`).

16. **(8) CUSTO — fila: medição incompleta apresentada sem marcar parcial.** Modelo/papel têm os gates previstos, mas `fable`, aceito pela porta, não possui preço e é descartado da soma. Arquivos ilegíveis também são ignorados sem sinalizar custo parcial (`bin/disjuntor.py:1412`, `bin/disjuntor.py:1417`, `bin/disjuntor.py:118`, `bin/disjuntor.py:126`, `bin/disjuntor.py:1529`, `bin/disjuntor.py:1533`).

17. **(4) RELÓGIO — fila: janela lida fora da borda.** `ler` recebe `agora`, mas `_b3_sem_obra` consulta outro `time.time()`, podendo usar janela diferente da medição recebida (`bin/disjuntor.py:2130`, `bin/disjuntor.py:2137`).

**Escritores e leitores solicitados.** A escrita física dos objetos do estado converge para `_com_estado`; há várias funções que os alteram logicamente (`bin/disjuntor.py:305`, `bin/disjuntor.py:310`, `bin/disjuntor.py:311`).

| Campo/arquivo | Quem escreve | Quem lê / multiplicidade |
|---|---|---|
| `voltas` | Criação em `bin/disjuntor.py:1373`; semente em `bin/disjuntor.py:1346` | Retorno em `bin/disjuntor.py:1374`; painel em `bin/disjuntor.py:1549` |
| `voltas.agent` | Semente `bin/disjuntor.py:1365`; autorização `bin/disjuntor.py:1484` | Teto `bin/disjuntor.py:1482`; mensagem `bin/disjuntor.py:1380`; painel `bin/disjuntor.py:1550`. **Dois escritores lógicos.** |
| `voltas.codex` | Semente `bin/disjuntor.py:1353`; reserva `bin/disjuntor.py:1393` | Teto `bin/disjuntor.py:1391`; mensagem `bin/disjuntor.py:1380`; painel `bin/disjuntor.py:1550`. **Dois escritores lógicos.** |
| `voltas.extra` | Inicialização `bin/disjuntor.py:1346`; `reabre` em `bin/disjuntor.py:1495` | Tetos `bin/disjuntor.py:1391`, `bin/disjuntor.py:1482`; painel `bin/disjuntor.py:1551` |
| `voltas.chaves` | Inicialização `bin/disjuntor.py:1346`; Codex `bin/disjuntor.py:1394`; Agent `bin/disjuntor.py:1485` | Codex consulta em `bin/disjuntor.py:1389`; Agent consulta apenas reservas em `bin/disjuntor.py:1467`. **Mais de um escritor.** |
| `reservas` | Criação `bin/disjuntor.py:1466`; poda `bin/disjuntor.py:1474`; inclusão `bin/disjuntor.py:1486` | Identidade `bin/disjuntor.py:1467`; poda/contagem `bin/disjuntor.py:1473`, `bin/disjuntor.py:1475`. Uma porta escritora. |
| `consumidas` / tipo | SHA `bin/disjuntor.py:1027`; mapa de tipo `bin/disjuntor.py:1023` | Deduplicação `bin/disjuntor.py:1014`; classificação antiga/nova `bin/disjuntor.py:1162`, `bin/disjuntor.py:1174`. Uma transição escritora. |
| `pede_desenho` | Liga `bin/disjuntor.py:1019`; remove `bin/disjuntor.py:1505` | Gate `bin/disjuntor.py:1010`. **Dois escritores.** |
| Triagem `cura=` | `triagem_cura`, em `bin/disjuntor.py:1307`; a triagem comum também escreve no mesmo arquivo em `bin/disjuntor.py:885` | Parser `bin/disjuntor.py:921`; cura `bin/disjuntor.py:1273`; idempotência `bin/disjuntor.py:1299`. B3/rearme também leem a trilha em `bin/disjuntor.py:963`, `bin/disjuntor.py:1095`. **Dois escritores do arquivo.** |
| `b3_sem_obra` com `t` | Migração `bin/disjuntor.py:1154`; coleta `bin/disjuntor.py:975`; negativa `bin/disjuntor.py:1434` | Deduplicação `bin/disjuntor.py:974`; janela `bin/disjuntor.py:2136`; painel `bin/disjuntor.py:2207`. **Mais de um escritor.** |
| `logs/suite_cert/<h>.ok` | Emissor `bin/disjuntor.py:1247`, chamado por `bin/suite.sh:112`; selo escreve diretamente em `bin/tests/test_disjuntor.sh:73`, `bin/tests/test_disjuntor.sh:1634` | Leitor operacional `bin/disjuntor.py:1260`, chamado em `bin/disjuntor.py:1444`. **Há escritor adicional explícito no selo; escritor adicional efetivo em prod [nao verificado].** |
| `DJ_VOLTA_CODEX` | Wrapper `bin/codex_pergunta.sh:166` | Porta `bin/disjuntor.py:1139`, `bin/disjuntor.py:1142` |
| `app/docs/contratos/<obra>.txt` | Artefato apresentado em `app/docs/contratos/CONTENCAO-QUE-MORDE-V2.txt:1`; selo cria contratos sintéticos em `bin/tests/test_contrato_do_push.sh:20`, `bin/tests/test_contrato_do_push.sh:32` | Gate consulta existência no PUSH_SHA em `bin/contrato_do_push.py:51`, via `bin/contrato_do_push.py:24`. Escritor operacional/autoria externa **[nao verificado]**. |

O ESTADO consome essas medições por `disjuntor.ler/linha`; não é outro escritor desses campos (`bin/gerar_estado.py:280`, `bin/gerar_estado.py:281`).

**Selos e mutantes — achados.**

| Severidade | Mutante/caso | O que realmente acontece |
|---|---|---|
| **bloqueia** | `i4-sem-uma-obra`, `bin/tests/test_disjuntor.sh:2025` | O primeiro cenário tem zero obras; remover a guarda leva a `obras[0]`, causa `IndexError` e continua negando. O teste cai pela mensagem diferente antes de chegar ao cenário de duas obras (`bin/tests/test_disjuntor.sh:1621`, `bin/tests/test_disjuntor.sh:1622`, `bin/disjuntor.py:1470`, `bin/disjuntor.py:1996`). |
| **bloqueia** | `i9-uma-copia`, `bin/tests/test_disjuntor.sh:2033` | Sem cópia, remover a guarda leva a `copias[0]`; novamente há exceção e negativa, sem demonstrar autorização indevida (`bin/tests/test_disjuntor.sh:1640`, `bin/disjuntor.py:1443`, `bin/disjuntor.py:1996`). |
| **bloqueia** | `b3-codex-porta-sempre-livre`, `bin/tests/test_disjuntor.sh:1790` | Retira rc 3, mas a obra PARADA cai no ramo seguinte e recebe rc 4. Wrapper continua recusando; o caso morre porque exige exatamente 3 (`bin/disjuntor.py:1136`, `bin/tests/test_disjuntor.sh:1250`). |
| **bloqueia** | `cx8-ausente-sem-falha`, `bin/tests/test_disjuntor.sh:1818` | Permite alcançar `tri.get` com `tri=None`; a exceção vira NAO_SEI e mantém a recusa. O caso exige outro prefixo de negativa (`bin/disjuntor.py:1025`, `bin/disjuntor.py:1069`, `bin/tests/test_disjuntor.sh:1261`, `bin/tests/test_disjuntor.sh:1360`). |
| **fila** | `B3-duas-travas`, `bin/tests/test_disjuntor.sh:1902` | Acrescenta uma transação vazia; não divide a decisão existente. O teste o mata contando chamadas, sem reproduzir decisão partida (`bin/tests/test_disjuntor.sh:1904`, `bin/tests/test_disjuntor.sh:926`, `bin/tests/test_disjuntor.sh:935`). |
| **bloqueia** | `i9_cert` | O próprio fixture fabrica certificado sem `OK`, e o caso exige que passe. Testa emissão pela CLI com log sintético; retirar a chamada de certificação de `suite.sh` não é mordido (`bin/tests/test_disjuntor.sh:73`, `bin/tests/test_disjuntor.sh:1631`, `bin/tests/test_disjuntor.sh:1643`, `bin/suite.sh:112`). |
| **bloqueia** | `i11_vivos` / `i1_voltas` | Repetição de identidade ocorre somente antes da poda; vivos são simulados como reservas. `_ag` já escreve resultado antes da autorização. Não exercitam replay após poda, lançamento real ou disputa concorrente (`bin/tests/test_disjuntor.sh:1552`, `bin/tests/test_disjuntor.sh:1556`, `bin/tests/test_disjuntor.sh:339`, `bin/tests/test_disjuntor.sh:1566`). |
| **fila** | `test_contrato_do_push.sh::ordem` | Apenas compara primeiras ocorrências por grep; comentário contendo o nome do gate pode satisfazer a procura. Não executa pre-push com SKIP_TESTS (`bin/tests/test_contrato_do_push.sh:48`, `bin/tests/test_contrato_do_push.sh:49`, `bin/tests/test_contrato_do_push.sh:70`). |

Os demais mutantes P1 retiram os ramos declarados: CHAVE, modelo/papéis, tetos, reserva do wrapper, segundo ERRO-BOBO, cura/ancestralidade e condições do emissor de certificado (`bin/tests/test_disjuntor.sh:2003`, `bin/tests/test_disjuntor.sh:2006`, `bin/tests/test_disjuntor.sh:2011`, `bin/tests/test_disjuntor.sh:2014`, `bin/tests/test_disjuntor.sh:2018`, `bin/tests/test_disjuntor.sh:2021`, `bin/tests/test_disjuntor.sh:2027`, `bin/tests/test_disjuntor.sh:2034`). Os cinco mutantes do push alteram as costuras declaradas; o da ordem continua sendo prova textual (`bin/tests/test_contrato_do_push.sh:66`, `bin/tests/test_contrato_do_push.sh:70`).

Há mutantes mortos por ficarem **mais restritivos**, mediante casos positivos: sem poda, sem idempotência, reabre sem adicionar e redesenha sem remover pedido. Isso é verificável, mas não demonstra “liberou quando devia negar” (`bin/tests/test_disjuntor.sh:2012`, `bin/tests/test_disjuntor.sh:2013`, `bin/tests/test_disjuntor.sh:2016`, `bin/tests/test_disjuntor.sh:2020`). O harness aceita qualquer falha do caso como morte do mutante (`bin/tests/test_disjuntor.sh:1705`).

**Bloqueia — faltam mordidas específicas** para persistência da autorização, semeadura retroativa/parcial, idempotência após poda, migração de ERRO-BOBO antigo/ilegível, timestamps/janela, custo 24 h e tratamento restritivo de meta/cópia ilegível no B1. Os casos respectivos cobrem contagem nova, erros novos, universo, modelos/papéis e jobs legíveis; o bloco de mutantes P1 não retira essas outras costuras (`bin/tests/test_disjuntor.sh:1566`, `bin/tests/test_disjuntor.sh:1586`, `bin/tests/test_disjuntor.sh:1621`, `bin/tests/test_disjuntor.sh:1533`, `bin/tests/test_disjuntor.sh:1676`, `bin/tests/test_disjuntor.sh:2003`).

**Checklist da casa:** aplicam-se autorização antes da persistência, separação de decisão/reserva entre travas, universo derivado de duas formas, leitor de certificado com critério mais fraco que o emissor, relógio fora da borda e selos sem a mordida anunciada (`bin/disjuntor.py:318`, `bin/disjuntor.py:1142`, `bin/disjuntor.py:815`, `bin/disjuntor.py:1191`, `bin/disjuntor.py:1242`, `bin/disjuntor.py:1264`, `bin/disjuntor.py:2137`, `bin/tests/test_disjuntor.sh:1902`). B3 continua delegado ao juiz registrado; o defeito está na autorização posterior sem revalidação (`bin/disjuntor.py:1068`, `bin/disjuntor.py:1077`, `bin/disjuntor.py:1465`). Pouso efetivo antes de pré-requisito: **[nao verificado]**.

**Resposta direta à pergunta 5:** em `bin/pre-push.sh`, **sim**, o portão é chamado na linha 10, antes de `SKIP_TESTS` na linha 11; **não há saída normal anterior à chamada nesse arquivo** (`bin/pre-push.sh:7`, `bin/pre-push.sh:10`, `bin/pre-push.sh:11`). A instalação prevista delega para ele (`bin/hooks.sh:56`). Existe cópia divergente com SKIP no início, cuja utilização efetiva é **[nao verificado]** — **fila** (`app/core/hooks/pre-push.sh:5`). Mesmo no arquivo correto, permanecem os caminhos de julgamento vazio e exclusões descritos no achado 15 (`bin/contrato_do_push.py:33`, `bin/contrato_do_push.py:37`, `bin/contrato_do_push.py:41`).

## 01/11 01:40 · sessao 8e68217c962e · - · - · origem=Ronald (tmux)

Complemento do bloco antigo:

- **bloqueia — mutante não abre a porta.** `bin/tests/test_disjuntor.sh:1790`, `b3-codex-porta-sempre-livre`, remove só o retorno 3. A obra PARADA cai depois em `bin/disjuntor.py:1136` e retorna 4; o wrapper continua recusando. O caso mata o mutante porque exige exatamente 3 (`test_disjuntor.sh:1250`). Confirmado em memória.

- **bloqueia — mutante troca negativa por exceção.** `bin/tests/test_disjuntor.sh:1818`, `cx8-ausente-sem-falha`, deixa a execução alcançar `tri.get` com `tri=None` (`bin/disjuntor.py:1025`), causando `AttributeError`. `julga_b3` converte em NAO_SEI (`:1069`), mantendo a recusa. O teste falha em `test_disjuntor.sh:1360` porque `_xp` exige a mensagem “DISJUNTOR B3” (`:1261`), não “B3-NAO-SEI”. Confirmado em memória.

- **fila — mutante não parte a decisão entre travas.** `B3-duas-travas`, `bin/tests/test_disjuntor.sh:1902`, acrescenta uma transação vazia antes da decisão completa. A decisão continua inteira sob uma trava. O selo o mata contando chamadas (`:926`, `:935`), sem simular uma decisão repartida com corrida.

- Os **182 mutantes antigos** de `bin/tests/test_disjuntor.sh:1724` a `:2000` têm âncoras únicas; todos os Python resultantes passam AST e todos os Bash resultantes passam `bash -n`, verificados em memória.
- Os mutantes antigos da CHAVE retiram efetivamente o que declaram: `:1753`, `:1754`, `:1755`, `:1756`, `:1757`, `:1760`, `:1855`; os casos distinguem essas mudanças em `:616`, `:624`, `:630`, `:649`.
- Execução integral dos casos antigos: **[nao verificado]**.

## 01/11 01:38 · sessao 4e4238c0d392 · - · - · origem=Ronald (tmux)

Achados de `bin/disjuntor.py`, só leitura; reproduções mencionadas abaixo ocorreram em memória com I/O substituído, sem criar arquivos.

- **bloqueia — autorização sem persistência:** `bin/disjuntor.py:305`, `:313`, `:318`, `:323` deixam a decisão valer quando a gravação falha. `porta_agent` retorna essa autorização em `:1488`; `reserva_volta_codex` em `:1397`, sem conferir `gravou`. A chamada pode abrir sem registrar volta/reserva; chamadas seguintes reutilizam a vaga. B3 antigo confere persistência em `:1046`, mas as portas novas não repetem essa proteção.

- **bloqueia — exceção antes da porta libera Agent não final:** `pretool` lê estado em `bin/disjuntor.py:1965`; se ilegível, a execução pula `porta_agent` (`:1992`–`:1995`). A exceção fecha somente o ramo B3 (`:2005`–`:2014`); outro Agent termina liberado em `:2015`. Basta Agent construtor não final com estado ilegível: não passa pelos gates novos de modelo, papel, vivos ou volta.

- **bloqueia — B3 e autorização separados em duas travas:** Agent consulta B3 em `bin/disjuntor.py:1958`–`:1963`; reserva depois em `:1465`–`:1488`, sem revalidar B3. Wrapper consulta em `:1132`, reserva em `:1142`/`:1397`. Entre as duas transições, outro processo pode parar a obra; o primeiro ainda abre. O mesmo flock existe, mas não cobre a decisão completa.

- **bloqueia — corrida da contagem de vivos:** `txt`/`vivos_tx` são fotografados fora da trava (`bin/disjuntor.py:1455`–`:1463`); outro processo pode podar a reserva de um agente já lançado em `:1473`–`:1474`. Quando o processo com fotografia antiga entra na trava, o agente já não está em reservas e ainda não está na sua fotografia: `:1475` subconta e pode abrir o sétimo. A fotografia precisa corresponder ao estado sob o flock.

- **bloqueia — idempotência some depois da poda:** só `reservas` é consultado em `bin/disjuntor.py:1467`; `voltas.chaves` recebe a identidade em `:1485`, mas não é consultado pelo caminho Agent. Depois de `:1474` apagar a reserva, a mesma chamada incrementa de novo em `:1484`. Reproduzido em memória: `a,b,a`, com resultado de `a` antes de `b`, termina com `agent=3` e `chaves=['a','b','a']`. O teste só repete enquanto a reserva existe (`bin/tests/test_disjuntor.sh:1555`).

- **bloqueia — poda sem reconhecer resultado:** `bin/disjuntor.py:1473` procura uma substring compacta `"tool_use_id":"..."` em todo o transcript, sem verificar objeto `tool_result`. Texto citado pode eliminar reserva; JSON com espaço após `:` não a elimina. O primeiro caso permite subcontagem; o segundo conserva agentes já entregues.

- **bloqueia — semente Agent congelada em zero pelo wrapper/reabre:** `reserva_volta_codex` chama `_voltas(..., None)` em `bin/disjuntor.py:1388`; `reabre`, em `:1494`. `_semente` retorna `agent=0` e parcial quando não recebe transcript (`:1346`, `:1356`–`:1359`). `_voltas` nunca completa a semente existente (`:1372`–`:1374`). Assim, seis Agents anteriores podem ser ignorados se wrapper/reabre inicializar primeiro.

- **bloqueia — sidecar Codex ilegível não declara semente parcial:** `_le_id` devolve `None` em `bin/disjuntor.py:831`–`:833`; `_semente` simplesmente pula em `:1349`–`:1351`. Subconta voltas e não marca parcial, contrariando contrato `app/docs/contratos/CONTENCAO-QUE-MORDE-V2.txt:11`.

- **bloqueia — universo novo e coletor antigo divergem:** `_eh_verificador` admite papel na descrição (`bin/disjuntor.py:1191`); porta e PreToolUse o usam (`:1427`, `:1958`). Mas `_reprovacoes_finais` só coleta `meta.agentType == 'verificador'` (`:815`). Um `general-purpose` rotulado verificador pode entregar REPROVADO sem alimentar o B3. É derivação paralela do universo.

- **bloqueia — “só Ronald” é filtro lexical contornável:** `guarda_chave` bloqueia os literais CLI em `bin/disjuntor.py:192`–`:195`, mas não protege as funções `reabre`/`redesenha` (`:1491`–`:1507`) nem o estado contra Write (`:189`–`:212`). Verificado em memória: `guarda_chave` e `pretool` retornam `None` para Bash `python3 -c 'import sys; sys.path.insert(0,"bin"); import disjuntor; disjuntor.reabre("OBRA-X",3)'`; idem `redesenha`. Write direto em `logs/sessao/disjuntor_estado.json` também é admitido pela guarda. Não localizei segundo escritor operacional já existente fora dessas funções; a porta permite criar esse caminho.

- **bloqueia — HEAD ausente passa sem PROCEDENTE:** `head=None` é tolerado em `bin/disjuntor.py:1447`–`:1451`; só é negado dentro do loop dos PROCEDENTES em `:1276`–`:1281`. Com trilha vazia, retorna livre em `:1288`. A ressalva “cópia sem hash = nega” do contrato `:13` não é incondicional no código. Reproduzido em memória para `_cura_falta` com triagem vazia.

- **fila — formato `consumidas` diverge literalmente:** contrato pede `{sha,tipo}` (`app/docs/contratos/CONTENCAO-QUE-MORDE-V2.txt:12`); implementação mantém lista de sha (`bin/disjuntor.py:1027`) mais mapa paralelo `consumidas_tipo` (`:1023`). A regra normal é implementada pelo leitor `:1162`–`:1174`, mas o formato escrito não é o contratado.

- **fila — relógio fora da borda:** `ler(..., agora)` chama `_b3_sem_obra` em `bin/disjuntor.py:2129`, que usa novo `time.time()` em `:2137`. A janela pode divergir do instante da medição fornecido.

- **fila — custo aceito e omitido:** `fable` é modelo aceito (`bin/disjuntor.py:118`), mas não possui preço em `:126`; `_custo_24h` o descarta em `:1533`–`:1535`. Arquivos ilegíveis são silenciosamente ignorados em `:1529`–`:1530`, podendo produzir custo parcial apresentado como total.

Rastreio dos campos, com escritores múltiplos explicitados:

| Campo | Escritores | Leitores |
|---|---|---|
| `voltas`/`agent`/`codex`/`extra`/`chaves` | criação/semente `bin/disjuntor.py:1346`, `:1353`, `:1365`, `:1373`; Codex incrementa `:1393`–`:1394`; Agent `:1484`–`:1485`; reabre `:1495` | `_voltas` `:1372`–`:1374`; Codex `:1389`–`:1392`; Agent `:1482`; mensagem `:1380`; painel `:1549`–`:1553` |
| `reservas` | cria `bin/disjuntor.py:1466`; poda `:1474`; grava `:1486` | `:1467`, `:1473`, `:1475` |
| `consumidas` / `consumidas_tipo` | `bin/disjuntor.py:1023`, `:1027` | `:1014`, `_erros_bobos` `:1162`–`:1174` |
| `pede_desenho` | liga `bin/disjuntor.py:1019`; remove `:1505` | `:1010` |
| triagem `cura=` | novo escritor `triagem_cura`, `bin/disjuntor.py:1307`–`:1309`; triagem sem cura continua escrevendo no mesmo arquivo em `:885`–`:887` | `_triagens` `:921`–`:926`; `_cura_falta` `:1273`–`:1287`; `triagem_cura` `:1299`–`:1305`; demais consumidores `_triagens` recebem o veredito sem o campo |
| `b3_sem_obra` com `t` | migração `bin/disjuntor.py:1154`–`:1155`; coletor `:975`; negativa da porta `:1434`–`:1435` | deduplicação `:974`; medição `:2136`–`:2138`; painel `:2207` |
| `DJ_VOLTA_CODEX` | `bin/codex_pergunta.sh:166` | `bin/disjuntor.py:1139`–`:1142` |

Todos esses objetos do estado são serializados pela mesma escrita física em `bin/disjuntor.py:308`–`:311`, embora várias funções façam mutações lógicas. `consumidas`/tipo têm uma transição escritora; `reservas` uma porta escritora; `pede_desenho`, triagem, `b3_sem_obra` e `voltas` têm mais de uma função escritora nos pontos acima.

CHAVE normal confere arquivo e linha (`bin/disjuntor.py:176`); RELATO ilegível mantém ligado (`:169`–`:171`); wrapper efetivamente consulta (`:1130`). O parâmetro `raiz` da chave vale só para o arquivo: `_chave_no_relato` não recebe raiz (`:164`), e `_topo_consistente` usa o `H.RELATO` global (`:749`–`:759`), podendo medir raiz diferente em chamadas programáticas de `ler(raiz)`; ocorrência em produção [nao verificado].

_1343 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
