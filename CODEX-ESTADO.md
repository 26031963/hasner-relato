# CODEX-ESTADO

> Ultima resposta processada: **31/10/2026 10:55** · publicado em **31/10/2026 10:56** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 31/10 10:55 · sessao 9839af8cbf61 · V3-RETOMA · leitura · origem=Code

**Não: o `motor_h` do diff não reproduz a partição final do fechamento.** Para folga, soma todo `periodos_ft`; para HE100 feriado, não preenche a rubrica e lê zero na chave ausente. O fechamento filtra escala certa, separa feriado e devolve o restante a trabalhadas. (`app/ponto/management/commands/diff_calculador.py:245–268`, `:535`; `app/ponto/services/fechamento.py:908–953`).

**(a) Origem das duas colunas**

O resultado v2 vem de `autoridade_do_periodo(c, ini, fim)`. A autoridade usa `apurar_por_fatia` quando há escalas e devolve o resultado em `Autoridade`. (`app/ponto/management/commands/diff_calculador.py:147`; `app/ponto/services/espelho.py:1166–1170`, `:1195`).

| Rubrica | Origem de `motor_h` |
|---|---|
| `horas_extras_100_feriado` | **Não há atribuição dessa chave em `m_dia`.** O loop lê `p.minutos_extra_100` para **`horas_extras_100`**, sem separar feriado. Depois, `a = float(mm.get(r) or 0)` transforma a chave ausente em **0**, nas linhas efetivamente comparadas. (`app/ponto/management/commands/diff_calculador.py:245–268`, `:535`) |
| `horas_folga_trabalhada` | Soma `(p.minutos_trabalhados or 0) / 60` de **todos** os `aut.resultado.periodos_ft` com entrada, agrupados por `_dia_do(p.entrada)` e dentro da janela. (`app/ponto/management/commands/diff_calculador.py:263–268`) |

No lado calculador, a trilha é:

1. Com `--pares-da-autoridade`, chama `insumos_do_calculador(c, aut.resultado, ...)`; essa função chama `segmentos_do_motor`, que lê conjuntamente `periodos` e `periodos_ft`, usa `resultado.dia_da_jornada` e as pontas efetivas dos períodos. (`app/ponto/management/commands/diff_calculador.py:287–313`; `app/ponto/calculador/alimentacao.py:302–305`, `:63–73`; `app/ponto/calculador/nucleo.py:393–400`).
2. `rubricas_por_dia` soma os pares pelo núcleo; o núcleo acumula `(saida - entrada).total_seconds() / 60` por dia. A alimentação entrega esses minutos a `regras.do_dia`, junto com escala certa, calendário e `folga_e_feriado`. (`app/ponto/calculador/alimentacao.py:437`, `:501–512`; `app/ponto/calculador/nucleo.py:134–155`).
3. As regras que produzem as duas rubricas são:

| Rubrica | Regra que produz `calculador_h` |
|---|---|
| `horas_extras_100_feriado`, no caminho normal | `do_dia` chama `dobra_de_feriado_do_dia`; havendo dobra, escreve `_dobra / 60`. A função devolve todo o trabalhado quando `_feriado_dobra_no_dia(dia)` é verdadeiro; esse método é emprestado de `MotorBase` e exige `feriado_em_dobra` e feriado. (`app/ponto/calculador/regras.py:287–295`, `:344–347`, `:774–785`; `app/ponto/motor_calculo_v2.py:879`) |
| `horas_extras_100_feriado`, no caminho de folga | Com folga, escala certa e `folga_e_feriado`, escreve `_certa / 60` em HE100 e HE100 feriado. (`app/ponto/calculador/regras.py:646–664`, `:675–683`) |
| `horas_folga_trabalhada` | `folga_trabalhada_do_dia` divide o trabalhado entre certa e sem escala certa. `do_dia` escreve `_certa / 60` em folga somente quando essa parcela não foi destinada ao feriado. (`app/ponto/calculador/regras.py:386–395`, `:660–690`) |

O comando coloca `_r_dia.valores` em `c_dia`; lê `a` de `m_dia` e `b` de `c_dia`; publica `round(a, 2)` e `round(b, 2)` nas colunas `motor_h` e `calculador_h`. Rubricas presentes em `nao_decide` são puladas. (`app/ponto/management/commands/diff_calculador.py:501`, `:517`, `:527–542`, `:655–658`).

**Condição do comando:** sem `--pares-da-autoridade`, `_ins_porta=None`, `_rub_do_dia={}` e os dias sem `_r_dia` são pulados antes de preencher `c_dia`; portanto essa execução não percorre as regras acima, e a consulta posterior ao calculador ausente também usa zero. (`app/ponto/management/commands/diff_calculador.py:325–337`, `:417–424`, `:469–472`, `:535`).

**(b) Passos do fechamento e reprodução no diff**

O fechamento compõe `periodos` e `periodos_ft` das fatias de vínculo antes de calcular os campos mensais. (`app/ponto/services/fechamento.py:96–97`, `:270–281`).

| Passo do fechamento v2 | Lado motor do diff | Lado calculador v3, pela trilha acima |
|---|---|---|
| **1. Base de HE100 feriado:** inicializa `horas_extras_100_feriado` com `round(resultado.horas_extra_100_feriado, 2)`. Essa property soma o subset feriado exclusivamente em `resultado.periodos`. (`app/ponto/services/fechamento.py:898`; `app/ponto/motor_calculo_v2.py:280–282`) | **Não reproduz:** não lê o subset nem preenche a rubrica. (`app/ponto/management/commands/diff_calculador.py:245–268`) | Calcula a dobra do dia e a identifica em campo próprio, em vez de copiar a property v2. (`app/ponto/calculador/regras.py:774–785`) |
| **2. Escala certa:** parte de todo `periodos_ft` e mantém em `_ft_certa` somente quem passa em `_certa(colab.id, data_local_da_entrada, cache)`. (`app/ponto/services/fechamento.py:903–910`) | **Não reproduz esse filtro final:** soma todos os `periodos_ft`. (`app/ponto/management/commands/diff_calculador.py:263–268`) | **Reproduz a decisão certa/sem certa:** recebe `_certa(c.pk, k, cache)` e divide os minutos nos dois retornos de `folga_trabalhada_do_dia`. (`app/ponto/calculador/alimentacao.py:511`; `app/ponto/calculador/regras.py:395`, `:660–664`) |
| **3. `feriado_vira_folga`:** somente dentro de `_ft_certa`, resolve o vínculo; se `feriado_vira_folga` for verdadeiro, move para `_ft_feriado`, retira de `_ft_certa` por identidade e acrescenta seus minutos/60 à base de HE100 feriado. (`app/ponto/services/fechamento.py:919–932`) | **Não reproduz:** esses minutos continuam somados como folga; não são transferidos para HE100 feriado. (`app/ponto/management/commands/diff_calculador.py:263–268`) | **Reproduz a transferência**, com a condição recebida `folga_e_feriado`: mantém folga zero e escreve `_certa/60` em HE100 feriado. (`app/ponto/calculador/alimentacao.py:508`; `app/ponto/calculador/regras.py:675–686`) |
| **4. Folga final:** grava `round(sum(minutos_trabalhados de _ft_certa)/60, 2)`, após retirar os feriados. (`app/ponto/services/fechamento.py:926–934`) | **Não reproduz a seleção:** sua soma continua sendo a totalidade de `periodos_ft`. (`app/ponto/management/commands/diff_calculador.py:263–268`) | Escreve folga apenas no ramo `_certa` que não caiu em `_hora_feriado`. (`app/ponto/calculador/regras.py:680–686`) |
| **5. Sem escala certa:** calcula o complemento por identidade, excluindo **tanto** `_ft_certa` **quanto** `_ft_feriado`; soma os minutos restantes a `horas_trabalhadas`. (`app/ponto/services/fechamento.py:948–953`) | **Não reproduz:** mantém esses minutos em folga; trabalhadas é somada somente do loop de `periodos`. (`app/ponto/management/commands/diff_calculador.py:247–253`, `:263–268`) | **Reproduz o destino em trabalhadas:** `_sem/60` vai para `horas_trabalhadas`, e folga fica zero. Nesse ramo, HE100 feriado é declarada em `nao_decide`, salvo a exceção `_hora_feriado`; não é afirmada como zero. (`app/ponto/calculador/regras.py:675–711`) |

O juiz de escala certa retorna falso para vínculo `LIMBO` ou assinaturas `A2`/`A8` na competência do dia. O predicado `feriado_vira_folga` exige vínculo que não trabalha em feriado, ciclo permitido e feriado conforme o lugar do colaborador. (`app/escala/services/escala_certa.py:16–51`; `app/escala/models.py:1357–1363`).

**A reprodução v3 não é literalmente idêntica em todos os insumos:** o fechamento consulta escala certa e feriado pela **data local de `p.entrada`**; o calculador usa **`k`, dia da jornada**, e sua alimentação exige adicionalmente `k in _feriados` para `folga_e_feriado`. (`app/ponto/services/fechamento.py:910`, `:922–924`; `app/ponto/calculador/alimentacao.py:63–73`, `:459–460`, `:508–512`).

Há também um bloqueio anterior no próprio motor que pode zerar o período e colapsar suas pontas efetivas; ambos os lados recebem esses efeitos. Isso não executa a partição final acima. (`app/ponto/motor_calculo_v2.py:2396–2399`, `:1409–1421`; `app/ponto/management/commands/diff_calculador.py:263–268`; `app/ponto/calculador/alimentacao.py:73`, `:102–111`).

Os valores mensais assim montados são persistidos por `fechamento.save()` no status aberto, ou por `QuerySet.update`, que inclui explicitamente as duas rubricas. (`app/ponto/services/fechamento.py:1096–1112`, `:1128–1138`).

Mesmo `--periodos-do-fechamento` substitui apenas `aut.resultado`; o somatório posterior continua lendo todos os `periodos_ft`, sem consumir `_ft_certa`, `_ft_feriado` ou `_ft_sem_escala`. (`app/ponto/management/commands/diff_calculador.py:154–162`, `:263–268`).

**(c) Campo feriado existente e omitido pelo diff**

**Sim:**

- `PeriodoCalculo.minutos_extra_100_feriado`, declarado como subset de `minutos_extra_100`. (`app/ponto/motor_calculo_v2.py:168–169`).
- `ResultadoMes.horas_extra_100_feriado`, que soma esse campo em `self.periodos` e divide por 60; existe também `horas_extra_100_normal`, calculada pela diferença entre HE100 total e feriado. (`app/ponto/motor_calculo_v2.py:278–286`).

Os escritores da dobra preenchem o subset com toda a duração ou todo o trabalhado do dia. O diff lê apenas `minutos_extra_100` para HE100 total e omite o subset feriado. (`app/ponto/motor_calculo_v2.py:2747–2750`, `:3030–3033`, `:3193–3196`, `:3275–3279`; `app/ponto/management/commands/diff_calculador.py:255–256`).

Ler essa property recuperaria a **base** de feriado dos períodos normais; o acréscimo vindo de `_ft_feriado` é calculado separadamente pelo fechamento. (`app/ponto/motor_calculo_v2.py:280–282`; `app/ponto/services/fechamento.py:929–930`).

## 31/10 10:53 · sessao 5d59139bfe2d · V3-RETOMA · leitura · origem=Code

**O v3 dobra o feriado previsto do 6x1; no 12x36 padrão, não dobra. Isso diverge do texto inicial da L-086, mas concorda com a ressalva de revogação escrita na própria L-086.** (`app/ponto/calculador/regras.py:344–347`; `app/ponto/motor_calculo_v2.py:3537`; `app/docs/LEIS.md:118`)

Considere **H = minutos trabalhados computados ÷ 60**, com calendário e cadastro disponíveis, plantão iniciado no feriado e sem excedente da jornada. Nessas condições: (`app/ponto/calculador/regras.py:633–641`, `767–806`; `app/docs/LEIS.md:117`)

| Caso previsto na escala | `horas_extras_100` | `horas_extras_100_feriado` | Fundamento do código |
|---|---:|---:|---|
| 6x1 comercial | H | H | O ciclo dobra; a função devolve todo o trabalhado, e `do_dia` atribui o mesmo valor às duas rubricas. (`app/ponto/motor_calculo_v2.py:3537`, `3607–3608`; `app/ponto/calculador/regras.py:344–347`, `775–789`) |
| 12x36 padrão | 0 | 0 | O ciclo está em `CICLOS_FERIADO_SIMPLES`; sem dobra nem excedente, permanecem os zeros iniciais. (`app/ponto/motor_calculo_v2.py:3537`, `3607–3608`; `app/ponto/calculador/regras.py:641`, `775–806`) |

As duas colunas identificam o total de HE100 e sua parcela de feriado: **não são duas dobras independentes**. (`app/ponto/calculador/regras.py:401–409`, `780–785`)

A previsão de trabalho **não bloqueia a dobra de feriado**: `rubricas_por_dia` passa separadamente a previsão, o calendário e `feriado_em_dobra`; o predicado da dobra verifica apenas esse flag e se o dia é feriado. Já `folga_trabalhada_do_dia`, quando recebe `eh_dia_trabalho=True`, retorna zero de folga trabalhada. (`app/ponto/calculador/alimentacao.py:422–433`, `501–511`; `app/ponto/motor_calculo_v2.py:879`; `app/ponto/calculador/regras.py:389–395`)

O texto literal inicial da **L-086** diz:

> **PLANTAO PREVISTO NA ESCALA NUNCA E HE 100%**: domingo ou feriado so pagam dobra em dia NAO previsto (folga, ou feriado trabalhado fora da escala)

Fonte: [`app/docs/LEIS.md:118`](/home/ronald/codex_ws/5579d1763beacdcb2e26090a352e03dfcad049d6/app/docs/LEIS.md:118).

**Isoladamente, esse texto mandaria não lançar dobra por feriado nos dois casos previstos.** Porém, a mesma linha registra literalmente a correção: (`app/docs/LEIS.md:118`)

> TEXTO SUPERADO EM PARTE por corte de 27/09 03:xx citado em ponto/motor_calculo_v2.py::MotorBase._feriado_dobra_no_dia: a clausula de FERIADO do texto esta REVOGADA -- feriado previsto DOBRA conforme feriado_em_dobra do ciclo (12x36 nao dobra), e ha selo que exige isso.

Fonte: [`app/docs/LEIS.md:118`](/home/ronald/codex_ws/5579d1763beacdcb2e26090a352e03dfcad049d6/app/docs/LEIS.md:118). O corte também está transcrito no código: “FERIADO NAO entra na L-086: segue a regra de feriado que ja existe no cadastro do ciclo”. (`app/ponto/motor_calculo_v2.py:862–865`)

A **L-085** determina literalmente:

> **A JORNADA PERTENCE AO DIA DE INICIO**, inclusive para domingo, feriado, contagem de semana e DSR: hora depois da meia-noite herda o dia em que o plantao COMECOU, e nunca cria trabalho no dia seguinte

Fonte: [`app/docs/LEIS.md:117`](/home/ronald/codex_ws/5579d1763beacdcb2e26090a352e03dfcad049d6/app/docs/LEIS.md:117).

A **L-084** trata de “PONTUALIDADE SO CONTRA O DIA DECLARADO” e determina que “as horas trabalhadas contam normalmente”; não estabelece outra regra de dobra de feriado. (`app/docs/LEIS.md:116`) Não localizei em CORTES.md outra determinação de valor que altere estes dois cenários. [não verificado]

**Portanto: há divergência com a redação inicial da L-086 para o 6x1; não há divergência com a regra vigente registrada na própria linha.** Para o 12x36 padrão, ambos os textos afastam a dobra nesse plantão previsto. (`app/docs/LEIS.md:118`; `app/ponto/calculador/regras.py:775–806`)

Duas ressalvas do código: uma CCT cadastrada pode ativar a dobra no 12x36, produzindo H nas duas rubricas; e, sem dobra, excedente superior a 120 minutos pode gerar `horas_extras_100`, mantendo `horas_extras_100_feriado=0`. (`app/core/regua_cct.py:473–478`; `app/ponto/calculador/regras.py:641`, `790–806`)

## 31/10 10:53 · sessao aa825b389d88 · - · - · origem=Ronald (tmux)

- O diff obtém `aut = autoridade_do_periodo(c, ini, fim)` em `app/ponto/management/commands/diff_calculador.py:147`. A autoridade monta `get_motor_cct` por fatia com feriados em `app/ponto/services/espelho.py:1115-1119`, chama `apurar_por_fatia` em `app/ponto/services/espelho.py:1166-1170` e retorna `Autoridade(resultado, ..., feriados, motor, ...)` em `app/ponto/services/espelho.py:1195`. Sem escalas na janela, calcula diretamente por `motor.calcular_mes(...)` em `app/ponto/services/espelho.py:1182-1186`.

- No motor v2, a classificação em FT é feita por `escala_colaborador.eh_dia_trabalho(data_jornada, **_alimentacao) is False` ou pela folga semanal em `app/ponto/motor_calculo_v2.py:2383-2395`; calcula o período e aplica o bloqueio do dia sem previsto em `app/ponto/motor_calculo_v2.py:2396-2399`; marca `periodo.folga_trabalhada = True` em `app/ponto/motor_calculo_v2.py:2406-2407`; acrescenta FT em `resultado.periodos_ft`, restantes em `resultado.periodos`, em `app/ponto/motor_calculo_v2.py:2430-2432`.

- `motor_h` para `horas_folga_trabalhada` soma **todos** `aut.resultado.periodos_ft[].minutos_trabalhados / 60`, agrupados por `_dia_do(p.entrada)` e limitados à janela, em `app/ponto/management/commands/diff_calculador.py:263-268`. Não aplica ali a seleção final de escala certa, separação `feriado_vira_folga` e transferência da FT sem escala para trabalhadas que estão em `app/ponto/services/fechamento.py:908-953`.

- `motor_h` para `horas_extras_100_feriado` não recebe atribuição no bloco motor: os períodos normais alimentam `horas_extras_100` com `minutos_extra_100` em `app/ponto/management/commands/diff_calculador.py:247-262`; FT alimenta somente `horas_folga_trabalhada` em `app/ponto/management/commands/diff_calculador.py:263-268`. A leitura final converte chave ausente em zero por `float(mm.get(r) or 0)` em `app/ponto/management/commands/diff_calculador.py:535`; o CSV publica esse `a` em `app/ponto/management/commands/diff_calculador.py:542`, com cabeçalho `motor_h` em `app/ponto/management/commands/diff_calculador.py:655-658`.

- Existe campo específico omitido: `PeriodoCalculo.minutos_extra_100_feriado`, declarado como subset de `extra_100` originado em feriado, em `app/ponto/motor_calculo_v2.py:168-169`; `ResultadoMes.horas_extra_100_feriado` soma esse campo apenas em `self.periodos` e divide por 60 em `app/ponto/motor_calculo_v2.py:280-282`; `horas_extra_100_normal = horas_extra_100 - horas_extra_100_feriado` em `app/ponto/motor_calculo_v2.py:283-286`.

- A regra v2 efetiva de dobra é `bool(self.feriado_em_dobra) and self.eh_feriado(dia)` em `app/ponto/motor_calculo_v2.py:859-879`; `eh_feriado` consulta conjunto injetado e depois `is_feriado` em `app/ponto/motor_calculo_v2.py:901-905`. O flag padrão é resolvido pelo ciclo em `app/ponto/motor_calculo_v2.py:3607-3608` (salvo override recebido).

- Os escritores do subset feriado atribuem toda a duração/trabalhado a `minutos_extra`, `minutos_extra_100` e `minutos_extra_100_feriado`, condicionados à mesma `_feriado_dobra_no_dia`, em `app/ponto/motor_calculo_v2.py:2747-2751` (12x36), `app/ponto/motor_calculo_v2.py:3030-3034` (partido por dia), `app/ponto/motor_calculo_v2.py:3193-3197` (comercial por período), `app/ponto/motor_calculo_v2.py:3275-3280` (comercial por dia). O ramo diário comercial zera antes os campos individuais inclusive subset feriado em `app/ponto/motor_calculo_v2.py:3254-3263`.

- O bloqueio anterior à partição FT já pode zerar valores que o diff lê: exige folga ou feriado, empresa com janela, previsto zero e ausência de autorização em `app/ponto/motor_calculo_v2.py:1386-1395`; para folga, retorna sem bloquear quando `escala_certa_no_dia` é falsa em `app/ponto/motor_calculo_v2.py:1403-1408`; nos restantes move minutos para `minutos_fora_janela`, zera trabalhado/HE/subset feriado/noturno/intra em `app/ponto/motor_calculo_v2.py:1409-1414` e colapsa o segmento efetivo em `app/ponto/motor_calculo_v2.py:1420-1421`. Isso não substitui a partição final do fechamento, que seleciona FT certa em `app/ponto/services/fechamento.py:908-910`, separa feriado em `app/ponto/services/fechamento.py:919-930`, soma só FT certa restante em `app/ponto/services/fechamento.py:933-934` e transfere o complemento disjunto para trabalhadas em `app/ponto/services/fechamento.py:948-953`.

## 31/10 10:53 · sessao d42e577b733a · - · - · origem=Ronald (tmux)

O fechamento **não usa a mesma partição que `motor_h`**: parte de `resultado.periodos_ft`, filtra escala certa, retira feriado e classifica o restante sem escala certa; o diff soma integralmente `periodos_ft` como folga trabalhada. (`app/ponto/services/fechamento.py:908-949`; `app/ponto/management/commands/diff_calculador.py:263-268`).

1. **Resultado mensal:** `apurar_por_fatia` cria `ResultadoMes`, chama motor por fatia de vigência e concatena `periodos`/`periodos_ft` das fatias. O fechamento usa esse composto em `resultado`. (`app/ponto/services/fechamento.py:182`; `app/ponto/services/fechamento.py:203-205`; `app/ponto/services/fechamento.py:270-284`; `app/ponto/services/fechamento.py:96`; `app/ponto/services/fechamento.py:729-731`).

2. **Base de HE100 feriado:** `fechamento.horas_extras_100_feriado = round(resultado.horas_extra_100_feriado, 2)`; a property soma `p.minutos_extra_100_feriado` exclusivamente em `self.periodos`, dividido por 60. (`app/ponto/services/fechamento.py:898`; `app/ponto/motor_calculo_v2.py:280-282`).

3. **Escala certa:** `_ft = list(resultado.periodos_ft or ())`; `_ft_certa` mantém somente os períodos cujo `escala_certa_no_dia(colab.id, timezone.localtime(p.entrada).date(), cache)` é verdadeiro. O juiz retorna `motivo_escala_errada(...) is None`. (`app/ponto/services/fechamento.py:903-910`; `app/escala/services/escala_certa.py:49-51`).

   **Diff motor:** não reproduz: soma todos os `periodos_ft` com entrada e data de jornada dentro da janela, sem chamar o juiz nesse somatório. (`app/ponto/management/commands/diff_calculador.py:263-268`).

   **Calculador:** reproduz o filtro como regra diária: alimentação passa `escala_certa=bool(_certa(...))`; `folga_trabalhada_do_dia` devolve `(_trab, 0)` com escala certa e `(0, _trab)` sem ela. (`app/ponto/calculador/alimentacao.py:511`; `app/ponto/calculador/regras.py:386-395`; `app/ponto/calculador/regras.py:660-664`).

4. **Feriado que vira folga:** apenas sobre `_ft_certa`, resolve `_ev_fer(colab, data local da entrada)`; se houver vínculo e `feriado_vira_folga(data)`, move o período para `_ft_feriado`, retira-o de `_ft_certa` por identidade e soma seus minutos/60 à base `resultado.horas_extra_100_feriado`. Também acrescenta a HE100 total e horas extras totais. (`app/ponto/services/fechamento.py:919-932`).

   O predicado exige `trabalha_em_feriado=False`, tipo existente, ciclo em `CICLOS_FERIADO_VIRA_FOLGA` e feriado conforme `eh_feriado(data, lugar_do_colab(...))`. (`app/escala/models.py:1357-1363`).

   **Diff motor:** não reproduz: continua contando esses períodos em folga (`periodos_ft` integral), e seu loop de períodos normais não atribui `horas_extras_100_feriado` — só `horas_extras_100` de `p.minutos_extra_100`; na consulta à chave ausente, usa zero. (`app/ponto/management/commands/diff_calculador.py:247-268`; `app/ponto/management/commands/diff_calculador.py:535`).

   **Calculador:** reproduz a transferência por dia, com detalhe da alimentação: `folga_e_feriado` exige vínculo do dia, `k in _feriados` **e** `vínculo.feriado_vira_folga(k)`. Com `_certa > 0`, grava `_certa/60` em HE100 e HE100 feriado, mantendo folga zero; sem feriado, grava `_certa/60` em folga. (`app/ponto/calculador/alimentacao.py:479-486`; `app/ponto/calculador/alimentacao.py:501-511`; `app/ponto/calculador/regras.py:675-686`).

5. **Folga final:** fechamento grava somente `sum(p.minutos_trabalhados for p in _ft_certa)/60`, arredondado, **depois** de retirar feriados. (`app/ponto/services/fechamento.py:926-934`).

6. **Sem escala certa:** `_ids_fora` é a união por identidade de `_ft_certa` e `_ft_feriado`; `_ft_sem_escala = _ft - _ids_fora`. Soma esses minutos exclusivamente a `fechamento.horas_trabalhadas`; não acrescenta às duas rubricas perguntadas. (`app/ponto/services/fechamento.py:948-953`).

   **Diff motor:** não reproduz: seus minutos continuam em `horas_folga_trabalhada` e não entram no somatório normal de `horas_trabalhadas`, que percorre `periodos`. (`app/ponto/management/commands/diff_calculador.py:247-253`; `app/ponto/management/commands/diff_calculador.py:263-268`).

   **Calculador:** reproduz a devolução à rubrica trabalhada: `_sem/60` vai a `horas_trabalhadas` e registra `folga_sem_escala_min`; folga fica zero. Para HE100 feriado nesse caso, declara a rubrica em `nao_decide`, por ser rubrica que não viaja com a folga, salvo quando `_hora_feriado` é verdadeira. (`app/ponto/calculador/regras.py:675`; `app/ponto/calculador/regras.py:687-711`).

7. **Persistência:** no ramo não leitura, status aberto chama `fechamento.save()`; os demais status fazem `QuerySet.update` com as duas rubricas. (`app/ponto/services/fechamento.py:1096-1112`; `app/ponto/services/fechamento.py:1128-1138`).

**Cuidado de equivalência literal:** fechamento filtra por `timezone.localtime(p.entrada).date()`; diff agrupa pela função `_dia_do(p.entrada)`, lendo o mapa de dia da jornada; alimentação v3 recebe `k` como dia e usa esse `k` para escala certa/feriado. Portanto as regras de partição v3 existem, mas não se deve afirmar igualdade por execução só pelo código. (`app/ponto/services/fechamento.py:910`; `app/ponto/services/fechamento.py:922`; `app/ponto/management/commands/diff_calculador.py:235-244`; `app/ponto/management/commands/diff_calculador.py:250`; `app/ponto/management/commands/diff_calculador.py:266`; `app/ponto/calculador/alimentacao.py:459-460`; `app/ponto/calculador/alimentacao.py:508-512`).

## 31/10 10:46 · sessao f59b24da8285 · F1C4-3-EMISSORES · verificador · origem=Code

As quatro guardas passam a consultar o predicado existente, e os testes novos **mordem as quatro reversões isoladas por inspeção**. Isso ainda não demonstra “limbo cobra” em todos os caminhos: sobrevivem seletores próprios, a fábrica tem filtros posteriores e o intervalo exige DNA que a fixture fornece artificialmente. (`app/chamados/services/fabrica_da_celula.py:80`; `app/chamados/services/materializacao.py:160`; `app/ponto/services/cluster_espurio.py:26`; `app/ponto/management/commands/detectar_intervalo_ausente.py:49`; `app/chamados/tests/test_f1c4_3_emissores.py:58`)

Execução de testes/mutantes, respeito aos tetos após a mudança e incidência por colaborador em produção: **[não verificado]**. [nao verificado]

1) A mudança exata de universo é maior que admitir `None`.

Considere `A = bool(ata.tipo_dia) and ata.tipo_dia != 'trabalho'`, `S = bool(ata.sem_ancora)` e `L = existência de pelo menos uma lâmpada-dicionário`. O juiz exclui `trabalha is False` ou `A`; **tipo ausente, `None` ou vazio não satisfaz `A`**. (`app/chamados/juizes.py:407`; `app/chamados/juizes.py:408`; `app/chamados/services/materializacao.py:158`)

A tabela compara **passagem pela guarda alterada**, mantendo satisfeitos os demais requisitos; “antes” vem do diff fornecido. (`app/chamados/services/fabrica_da_celula.py:80`; `app/chamados/services/materializacao.py:160`; `app/ponto/services/cluster_espurio.py:26`; `app/ponto/management/commands/detectar_intervalo_ausente.py:49`)

| Sítio | Passava antes | Passa agora | Passa a entrar | Deixa de entrar |
|---|---|---|---|---|
| Fábrica | `trabalha is True` | `trabalha is not False and not A` | `None` com `not A` | `True` com `A` |
| Materialização | `True and L` | `trabalha is not False and not A and L` | `None`, `not A`, `L` | `True`, `A`, `L` |
| Cluster | `True and not S` | `trabalha is not False and not A` | `None/not A`; também `True/not A/S` | `True/A/not S` |
| Intervalo | `True and not S` | `trabalha is not False and not A` | `None/not A`; também `True/not A/S` | `True/A/not S` |

Para contraexemplos concretos, use:

- `L2`: entrada `08:00` acesa, luz `08:03`; saída `17:00` apagada. (`app/chamados/tests/test_f1c4_3_emissores.py:34`; `app/chamados/tests/test_f1c4_3_emissores.py:35`)
- `L4`: entrada `08:00` acesa; saída de intervalo `12:00` apagada; retorno `13:00` apagado; saída `17:00` acesa, luz `17:02`. (`app/chamados/tests/test_f1c4_3_emissores.py:34`; `app/chamados/tests/test_f1c4_3_emissores.py:36`; `app/chamados/tests/test_f1c4_3_emissores.py:37`; `app/chamados/tests/test_f1c4_3_emissores.py:38`)

| Sítio | Contraexemplo que entra agora | Resultado esperado pelo teste | [nao verificado]
|---|---|---|
| Fábrica | `(None, 'trabalho', True, L2)` | Motivos não vazios, com colaborador ativo, data passada e veredito acusador. (`app/chamados/tests/test_f1c4_3_emissores.py:55`; `app/chamados/tests/test_f1c4_3_emissores.py:60`; `app/chamados/tests/test_f1c4_3_emissores.py:70`) |
| Materialização | `(None, 'trabalho', True, L2)`, plantio às `17:00` | `True`: o plantio preenche o furo. (`app/chamados/tests/test_f1c4_3_emissores.py:81`; `app/chamados/tests/test_f1c4_3_emissores.py:82`; `app/chamados/tests/test_f1c4_3_emissores.py:86`) |
| Cluster | `(None, 'trabalho', True, L2)`, órfãs `18:56/18:58` | Dia detectado: duas órfãs próximas e uma lâmpada apagada. (`app/chamados/tests/test_f1c4_3_emissores.py:97`; `app/chamados/tests/test_f1c4_3_emissores.py:101`; `app/ponto/services/cluster_espurio.py:30`) |
| Intervalo | `(None, 'trabalho', True, L4)`, DNA `08:00–17:00`, pausa `12:00–13:00` | Furos `12:00/13:00`. (`app/chamados/tests/test_f1c4_3_emissores.py:112`; `app/chamados/tests/test_f1c4_3_emissores.py:117`) |

O contraexemplo de **perda de passagem pela guarda** é `(True, 'folga', False, L2)` nos três primeiros e `(True, 'folga', False, L4)` no intervalo: antes passava; agora o juiz exclui. Com órfãs e DNA acima, materialização/cluster/intervalo também perdem o resultado positivo dos respectivos helpers. (`app/chamados/juizes.py:408`; `app/chamados/services/materializacao.py:160`; `app/ponto/services/cluster_espurio.py:26`; `app/ponto/management/commands/detectar_intervalo_ausente.py:49`)

Na fábrica, porém, `True/'folga'` **já podia terminar em `[]` antes**, porque a coleta posterior descarta dias diferentes de `trabalho`. Portanto, perder passagem pela primeira guarda não prova perda de pergunta emitida. (`app/chamados/services/fabrica_da_celula.py:49`; `app/chamados/services/disputa_emissao.py:827`; `app/chamados/services/declaracao_texto.py:95`)

Outros limites concretos:

- `(True, 'trabalho', True, L2/L4)` passa a entrar em cluster/intervalo pela retirada de `sem_ancora`, embora não seja `trabalha=None`. A ocorrência dessa combinação em produção é **[não verificado]**. (`app/ponto/services/cluster_espurio.py:26`; `app/ponto/management/commands/detectar_intervalo_ausente.py:49`)
- `(None, tipo ausente, True, L2)` passa pela guarda nova, mas a fábrica recebe `tipo_dia='indefinido'` do leitor e continua sem cobrar esses marcos. (`app/chamados/juizes.py:408`; `app/escala/services/leitor_celula.py:344`; `app/chamados/services/declaracao_texto.py:95`)
- `False` continua excluído, mesmo com ata de trabalho e lâmpadas apagadas. (`app/chamados/juizes.py:408`)
- O intervalo continua exigindo entrada, saída e pausa no DNA. A fixture cria DNA completo e depois troca `trabalha` para `None`; o produtor normal só preenche marcos quando recebe `True`. Assim, um limbo gerado com `dna.marcos=None` continua sem furo de intervalo retornado. (`app/chamados/tests/test_f1c4_3_emissores.py:58`; `app/chamados/tests/test_f1c4_3_emissores.py:60`; `app/ponto/management/commands/gerar_celulas.py:77`; `app/ponto/management/commands/gerar_celulas.py:79`; `app/ponto/management/commands/detectar_intervalo_ausente.py:60`)

O comentário “`sem_ancora` é o limbo com outro nome” vale para a derivação normal, mas **não universalmente**: no intermitente ele deriva da falta de âncora na sequência de batidas. (`app/escala/utils.py:729`; `app/escala/utils.py:1701`; `app/escala/utils.py:1456`; `app/escala/utils.py:1514`)

2) Quem escreve e quem lê os quatro campos.

| Campo | Produção/escrita |
|---|---|
| `cel.trabalha` | Geração/regeneração calculam pela autoridade do vínculo e escrevem dentro da porta: `app/ponto/portas/celula.py:243`, `:247`, `:260`, `:590`, `:595`, `:612`. Fora da vigência escreve `False` em `:231`; reversão reconstrói pelo DNA em `:532`. O campo é triestado: `app/escala/models.py:1654`. |
| `ata.tipo_dia` | A grade produz trabalho/folga em `app/escala/utils.py:727`, `:752`, `:1743`, `:1749`; overlays escrevem folga/falta/ausência em `:1118`, `:1174`, `:1180`. O cartório copia para a ata em `app/ponto/services/cartorio.py:566`. |
| `ata.sem_ancora` | Grade normal: `app/escala/utils.py:729`, `:1701`; intermitente: `:1456`, `:1514`; transporte em `:1752`. Cartório copia/coage em `app/ponto/services/cartorio.py:569`. |
| `ata.lampadas` | Produtor `lampadas_do_dia`: `app/ponto/services/cartorio.py:427`, com montagem em `:539` e `:550`; `ata_do_dia` chama e incorpora em `:564`, `:566`. Dispensa modifica as luzes em `:614`; teto temporal projeta `acesa=None` em `:711`. |

Os três campos da ata persistem pela lavra: método da entidade delega à porta; a porta atribui a ata e salva com emissão do evento no mesmo `atomic`. O cartório também usa `save=False` e entrega os campos lavrados ao carimbo. (`app/escala/models.py:1781`; `app/ponto/portas/celula.py:711`; `app/ponto/portas/celula.py:725`; `app/ponto/portas/celula.py:732`; `app/ponto/portas/celula.py:639`; `app/ponto/services/cartorio.py:1520`; `app/ponto/services/cartorio.py:1535`)

| Campo | Leitores no caminho dos quatro sítios |
|---|---|
| `cel.trabalha` | Agora lido pelo juiz, chamado pelos quatro sítios. Também alimenta a autoridade do vínculo e o leitor genérico. (`app/chamados/juizes.py:408`; `app/chamados/services/fabrica_da_celula.py:80`; `app/chamados/services/materializacao.py:160`; `app/ponto/services/cluster_espurio.py:26`; `app/ponto/management/commands/detectar_intervalo_ausente.py:49`; `app/escala/models.py:1072`; `app/escala/services/leitor_celula.py:345`) |
| `ata.tipo_dia` | Juiz e leitor genérico; na fábrica, também a coleta de marcos faltantes. (`app/chamados/juizes.py:408`; `app/escala/services/leitor_celula.py:344`; `app/chamados/services/declaracao_texto.py:90`; `app/chamados/services/declaracao_texto.py:95`) |
| `ata.sem_ancora` | Após o diff, nenhum dos quatro mantém esse veto direto; o leitor genérico continua transportando o campo. (`app/ponto/services/cluster_espurio.py:26`; `app/ponto/management/commands/detectar_intervalo_ausente.py:49`; `app/escala/services/leitor_celula.py:362`) |
| `ata.lampadas` | Direto em materialização, cluster e intervalo. Na fábrica: `motivos_pela_ata → _ata_do_dia → _marcos_faltantes → grade_da_celula → _celulas_da_ata`. (`app/chamados/services/materializacao.py:158`; `app/ponto/services/cluster_espurio.py:29`; `app/ponto/management/commands/detectar_intervalo_ausente.py:62`; `app/chamados/services/fabrica_da_celula.py:49`; `app/chamados/services/disputa_emissao.py:827`; `app/chamados/services/declaracao_texto.py:82`; `app/escala/services/leitor_celula.py:93`) |

Quem mais lê, além desses caminhos:

- **`trabalha`:** precedência, veto de emissão, flip, passivo, descrição do dia, relatório da frota e seletores ORM relacionados abaixo. (`app/ponto/precedencia.py:290`; `app/ponto/marcos.py:155`; `app/ponto/services/flip_auto.py:208`; `app/ponto/services/passivo_s84.py:96`; `app/ponto/services/dia_do_colab.py:121`; `app/relatorios/dia_da_frota.py:47`)
- **`tipo_dia`:** suprajuiz, worklist, furos diários, propositor, simulação, causas e cálculo/fechamento. (`app/ponto/supra_juiz.py:199`; `app/ponto/worklist.py:19`; `app/escala/services/furos_diarios.py:39`; `app/ponto/propositor.py:36`; `app/ponto/services/simular_dia.py:12`; `app/folha/causas.py:41`; `app/ponto/services/dia_pago.py:386`; `app/ponto/services/fechamento.py:1015`; `app/folha/export.py:1071`)
- **`sem_ancora`:** suprajuiz, causas da folha, relatórios de furos, indicadores e fechamento. (`app/ponto/supra_juiz.py:214`; `app/folha/causas.py:96`; `app/relatorios/furos.py:157`; `app/relatorios/furos.py:218`; `app/relatorios/furos.py:237`; `app/inteligencia/calculadores.py:48`; `app/inteligencia/calculadores.py:143`; `app/ponto/services/fechamento.py:1018`)
- **`lampadas`:** demais juízes de chamados, juiz de batida, lastro, reconciliador, vigias, prévia/controle da folha, pautas e cartão. (`app/chamados/juizes.py:457`; `app/chamados/juizes.py:888`; `app/chamados/juizes.py:1008`; `app/ponto/juiz_batida.py:160`; `app/ponto/services/lastro.py:89`; `app/chamados/reconciliador.py:673`; `app/ponto/services/vigia_entrada.py:80`; `app/ponto/services/vigia_pareador_celula.py:78`; `app/folha/previa.py:84`; `app/folha/services/sem_furo.py:22`; `app/pautas/services.py:414`; `app/relatorios/cartao_pela_celula.py:289`)

A atribuição de ata em `escala/utils.py` é uma cópia em memória que reutiliza o mesmo produtor de lâmpadas; `carimbar_precedencia` preserva os campos e altera apenas `precedencia`. Esses pontos não constituem, por si, outro produtor dos quatro valores. (`app/escala/utils.py:1826`; `app/escala/utils.py:1832`; `app/ponto/portas/celula.py:774`)

3) Sim: sobrevivem outros sítios com predicados próprios.

| Sítio | Resposta própria |
|---|---|
| `app/chamados/management/commands/disparar_perguntas_competencia.py:76` | Lote seleciona somente `trabalha=True` e chama `motivos_pela_ata` diretamente, sem a guarda modificada de `motivos_a_perguntar`. (`:111`) |
| `app/ponto/services/vigia_de_hora.py:21` | Vigia seleciona somente `trabalha=True`; limbo fica fora antes do julgamento. |
| `app/chamados/services/fila_por_causa.py:192` | Constrói dias previstos por `trabalha=True`. |
| `app/ponto/services/vigia_entrada.py:79`, `:120` | Estatística e leitura do dia restringem a `trabalha=True`. |
| `app/relatorios/furos.py:151`, `:157`, `:218`, `:237` | Exclui tipos diferentes de trabalho e/ou `sem_ancora`; limbo fica fora da contagem de furos. |
| `app/inteligencia/calculadores.py:46`, `:48`, `:141`, `:143` | Exige trabalho e separa `sem_ancora` da contagem de furos. |
| `app/ponto/services/dia_do_colab.py:121` | `not cel.trabalha`: descreve `None` como dia sem trabalho previsto. |
| `app/relatorios/dia_da_frota.py:47` | `not cel.trabalha`: conta `None` como folga. |
| `app/ponto/services/passivo_s84.py:96` | `TRABALHO if cel.trabalha else FOLGA`: classifica `None` como folga. |
| `app/chamados/services/declaracao_texto.py:95`; `app/chamados/services/disputa_emissao.py:68` | Só tipo `trabalho` fornece marcos/data de furo. |
| `app/ponto/worklist.py:19`; `app/escala/services/furos_diarios.py:39` | Só tipo `trabalho` entra na classificação de gap/furo. |
| `app/ponto/marcos.py:159`; `app/escala/servico_jornada.py:303` | Veto por `trabalha is False`, sem a mesma cláusula de tipo da ata. |
| `app/ponto/services/flip_auto.py:208`, `:212` | Folga e ancoragem na véspera decididas por `is False`/`is not True`. |
| `app/ponto/services/cartorio.py:1167`, `:1664` | Protesto e espera pelo fim do turno usam `trabalha is not False`. |
| `app/ponto/supra_juiz.py:281`, `:443` | Define dia de cobrança e cobrança em dia sem trabalho com regras próprias. |

Há uma divergência concreta de política: **o suprajuiz considera ausência parcial como dia de cobrança**, enquanto `dia_sem_trabalho` exclui qualquer `tipo_dia='ausencia'`. Essa combinação não exige ata incoerente: o overlay registra ausência parcial e preserva as células. (`app/ponto/supra_juiz.py:281`; `app/chamados/juizes.py:408`; `app/escala/utils.py:1180`; `app/escala/utils.py:1190`; `app/escala/utils.py:1195`)

Isso não torna toda comparação de `tipo_dia` automaticamente um juiz duplicado: o registro separa trabalho **para o vínculo**, precedência, cobertura de ausência e furo. A autoridade registrada do vínculo retorna o triestado; o helper adotado combina esse estado com a ata para excluir cobrança. (`app/core/juizes.py:158`; `app/core/juizes.py:160`; `app/core/juizes.py:164`; `app/core/juizes.py:166`; `app/escala/models.py:1072`; `app/chamados/juizes.py:408`)

4) Os testes mordem as reversões pedidas, com limites. [nao verificado]

| Mutação isolada | Efeito previsto | Assert que morde |
|---|---|---|
| Reverter fábrica | Limbo retorna `[]` | `app/chamados/tests/test_f1c4_3_emissores.py:70` |
| Reverter materialização | Limbo retorna `False` | `app/chamados/tests/test_f1c4_3_emissores.py:86` |
| Reverter cluster | Limbo retorna `[]` | `app/chamados/tests/test_f1c4_3_emissores.py:101` |
| Reverter intervalo | Limbo retorna `[]` | `app/chamados/tests/test_f1c4_3_emissores.py:117` |
| Manter `or ata.get('sem_ancora')` no cluster | Exclui a fixture limbo | `app/chamados/tests/test_f1c4_3_emissores.py:55`, `:101` |
| Manter essa cláusula no intervalo | Exclui a fixture limbo | `app/chamados/tests/test_f1c4_3_emissores.py:55`, `:117` |

Os controles positivos `True/'trabalho'` demonstram que as fixtures podem produzir resultado. Contudo, os testes novos não cobrem por sítio `False`, tipo ausente/vazio, `True/'folga'`, `True/sem_ancora=True` ou ausência parcial; também não exigem estruturalmente delegação ao juiz — um clone equivalente passa nos asserts de retorno. (`app/chamados/tests/test_f1c4_3_emissores.py:54`; `app/chamados/tests/test_f1c4_3_emissores.py:76`; `app/chamados/tests/test_f1c4_3_emissores.py:92`; `app/chamados/tests/test_f1c4_3_emissores.py:107`; `app/chamados/tests/test_f1c4_3_emissores.py:123`)

Os testes anteriores do juiz cobrem `False`, tipo ausente e `True/'folga'`, mas isso não substitui exercício desses casos em cada consumidor. (`app/chamados/tests/test_f1c4_3_none_cobra.py:42`; `app/chamados/tests/test_f1c4_3_none_cobra.py:45`; `app/chamados/tests/test_f1c4_3_none_cobra.py:50`)

Além disso, os novos testes exercitam **retornos dos helpers**, não nascimento final de chamados: a fábrica só escreve depois; cluster só abre em `reconciliar_colab`; materialização responde uma segunda opinião de paridade; o command de intervalo está com emissão aposentada e apenas reconcilia legado. (`app/chamados/services/fabrica_da_celula.py:151`; `app/ponto/services/cluster_espurio.py:86`; `app/chamados/services/materializacao.py:128`; `app/ponto/management/commands/detectar_intervalo_ausente.py:147`; `app/ponto/management/commands/detectar_intervalo_ausente.py:154`)

5) Checklist das classes de reprovação.

| Classe | Resultado da inspeção |
|---|---|
| **Segundo juiz duplicando registrado** | Os quatro sítios delegam ao helper existente; não passam a implementar seu predicado. A classe global permanece aberta pelos sítios próprios listados, especialmente lote/vigia e tratamento de ausência parcial. (`app/chamados/services/fabrica_da_celula.py:80`; `app/chamados/services/materializacao.py:160`; `app/ponto/services/cluster_espurio.py:26`; `app/ponto/management/commands/detectar_intervalo_ausente.py:49`; `app/chamados/management/commands/disparar_perguntas_competencia.py:76`; `app/ponto/supra_juiz.py:281`) |
| **Segundo escritor fora da porta** | As guardas não escrevem os campos; a persistência encontrada permanece na porta. O `.update()` novo é fixture de teste. Existe selo AST da porta, mas sua execução nesta cópia é **[não verificado]**. (`app/ponto/portas/celula.py:260`; `app/ponto/portas/celula.py:725`; `app/chamados/tests/test_f1c4_3_emissores.py:60`; `app/ponto/tests/test_chokepoint_celula_gate.py:49`) |
| **Relógio fora da borda** | O juiz novo chamado não lê relógio. Permanecem leituras na fábrica quando faltam parâmetros e no cartório, inclusive `_tz.now()` apesar de aceitar `agora`; são anteriores à troca. (`app/chamados/juizes.py:407`; `app/chamados/services/fabrica_da_celula.py:84`; `app/chamados/services/fabrica_da_celula.py:136`; `app/ponto/services/cartorio.py:1668`; `app/ponto/services/cartorio.py:1671`) |
| **Derivação paralela de fonte única** | A troca remove quatro derivações locais, mas permanecem os predicados do item 3. Há ainda perda de informação na reversão de regeneração, detalhada abaixo. (`app/chamados/juizes.py:408`; `app/relatorios/dia_da_frota.py:47`; `app/ponto/portas/celula.py:532`) |
| **Selo/RED sem mordida** | Os casos limbo mordem as reversões solicitadas; falta prova executada e cobertura de delegação, integração e custo no universo novo. (`app/chamados/tests/test_f1c4_3_emissores.py:70`; `:86`; `:101`; `:117`; execução: **[não verificado]**) |
| **Import circular / query nova na batida** | Os imports adicionados são locais; o corpo do juiz não contém ORM. Já existe ciclo entre `juizes` e `models`; ausência de erro de import em execução é **[não verificado]**. (`app/chamados/services/fabrica_da_celula.py:78`; `app/chamados/services/materializacao.py:141`; `app/ponto/services/cluster_espurio.py:20`; `app/ponto/management/commands/detectar_intervalo_ausente.py:37`; `app/chamados/juizes.py:6`; `app/chamados/models.py:3135`) |

**Achado anterior ao diff na linhagem de escrita:** `reverter_regeneracao_dia` restaura `trabalha` como “DNA tem marcos?”. Mas `montar_dna` produz `marcos=None` tanto para `False` quanto para `None`. Assim, um DNA anterior produzido para limbo pode voltar como `trabalha=False`: a reversão não preserva o triestado. Não é segundo escritor fora da porta; é reconstrução com informação insuficiente dentro dela. (`app/ponto/portas/celula.py:491`; `app/ponto/portas/celula.py:532`; `app/ponto/management/commands/gerar_celulas.py:77`; `app/ponto/management/commands/gerar_celulas.py:79`; `app/ponto/management/commands/gerar_celulas.py:100`)

**Quanto ao teto da batida:** o juiz isolado não adiciona consulta explícita, mas admitir células antes puladas permite alcançar consultas e escritas posteriores. A fábrica consulta perguntas e pode criá-las; cluster consulta deduplicação e pode abrir chamados. Ambos são alcançáveis pelo julgamento síncrono disparado pela batida. Porta

[truncado em 20000 de 20847 caracteres]

## 31/10 10:41 · sessao 13795f00ab78 · - · - · origem=Ronald (tmux)

Censo de predicados próprios fora dos quatro sítios:

| Sítio | Predicado e efeito |
|---|---|
| `app/chamados/management/commands/disparar_perguntas_competencia.py:76` | SQL `trabalha=True`; exclui `None` do lote. Importa e chama `motivos_pela_ata` diretamente, sem passar por `motivos_a_perguntar` (`:30`, `:111`). |
| `app/ponto/services/vigia_de_hora.py:21` | SQL `trabalha=True`; exclui limbo antes do julgamento do vigia (`:113-115`). |
| `app/relatorios/furos.py:151`, `:157` | `tipo_dia != trabalho` vira folga; `sem_ancora` vira indefinido fora da contagem. |
| `app/relatorios/furos.py:218`, `:237` | `tipo_dia != trabalho or sem_ancora` elimina dia de `dias_100_em_branco`/`furos_por_dia`. |
| `app/ponto/services/dia_do_colab.py:121` | `not cel.trabalha` imprime “Dia sem trabalho previsto: nenhuma marca era esperada”; portanto inclui `None`. |
| `app/relatorios/dia_da_frota.py:47` | `not cel.trabalha` conta `None` como folga. |
| `app/ponto/worklist.py:19` | `tipo_dia != trabalho` impede classificação de gap. |
| `app/chamados/services/declaracao_texto.py:95` | `tipo_dia != trabalho` impede coleta de marcos faltantes. |
| `app/chamados/services/disputa_emissao.py:68` | Só reconhece data de furo se grade diz `tipo_dia == trabalho`. |
| `app/escala/services/furos_diarios.py:39` | Só mede furo em `tipo_dia == trabalho`. |
| `app/chamados/services/fila_por_causa.py:192` | SQL `trabalha=True` monta dias previstos usados para classificação da fila (`:199`). |
| `app/ponto/services/vigia_entrada.py:79`, `:120` | SQL `trabalha=True` restringe estatística e leitura de entradas faltantes. |
| `app/ponto/supra_juiz.py:281` | Define dia de cobrança por `trabalho` ou `ausencia` parcial; não equivale ao juiz que cala qualquer ata `ausencia`. |
| `app/ponto/supra_juiz.py:443` | Deriva `COBRANCA_EM_DIA_SEM_TRABALHO` de tipo diferente de trabalho, excluída ausência de jurisdição, previsto zero, chamados vivos e ausência sem cobertura. |
| `app/ponto/services/cartorio.py:1167` | `_protesta` usa `_cel.trabalha is not False`, além de marcos acesos; ignora a cláusula `ata.tipo_dia` do juiz. |
| `app/ponto/services/cartorio.py:1664` | Decide se aguarda fim do turno por `cel.trabalha is not False`. |
| `app/ponto/marcos.py:159` | Veta como `FOLGA` somente `trabalha is False`, além de vetos por veredito (`:161`). |
| `app/escala/servico_jornada.py:303` | `classificar_falta` cala quando célula diz `False`; segue regras próprias de ausência (`:308`). |
| `app/ponto/services/flip_auto.py:208`, `:212` | Folga e vespera que deve ser estritamente `True` influenciam proposta de flip. |
| `app/ponto/services/veredito_celula.py:221` | Não materializa se grade simulada não diz trabalho. |

Não são todos a mesma pergunta: juiz registrado para **vínculo** retorna o triestado da célula, enquanto `dia_sem_trabalho` decide exclusão para cobrança combinando `trabalha` e ata; chamar este helper existente não cria novo juiz (`app/core/juizes.py:158`; `app/escala/models.py:1048-1073`; `app/chamados/juizes.py:398-408`). Classificadores de geometria também filtram tipo trabalho (`app/ponto/propositor.py:36`, `:210`; `app/ponto/services/simular_dia.py:12`; `app/escala/orfa_classe.py:65`).

**Cuidado na análise da fábrica:** alteração da guarda não garante alteração do retorno. O caminho é `motivos_a_perguntar` → `motivos_pela_ata` → `_ata_do_dia` → `_marcos_faltantes` → leitor da célula (`app/chamados/services/fabrica_da_celula.py:111`, `:49`; `app/chamados/services/disputa_emissao.py:827`; `app/chamados/services/declaracao_texto.py:82`).

- `True + ata folga`: guarda antiga permitia, mas `_marcos_faltantes` já descartava; retorno normalmente já `[]` (`app/chamados/services/declaracao_texto.py:95`).
- `None + ata trabalho + sem_ancora=True`: a coleta não veta `sem_ancora`; leitor transporta o flag e mantém tipo da ata (`app/escala/services/leitor_celula.py:344`, `:362`; `app/chamados/services/declaracao_texto.py:95`).
- `None + ata sem tipo`: nova guarda permite, mas leitor produz `indefinido`, que coletor descarta; não implica pergunta emitida (`app/escala/services/leitor_celula.py:344-345`; `app/chamados/services/declaracao_texto.py:95`).
- Falha de leitura ou ausência de escala pode devolver fallback `(True, None)`, tratado pela lei de falta; não se pode universalizar o retorno anterior sem essas condições (`app/chamados/services/disputa_emissao.py:832`, `:839-841`; `app/chamados/services/fabrica_da_celula.py:52-57`).

Relógio: a nova chamada ao helper não lê relógio nem executa query explícita (`app/chamados/juizes.py:407-408`). Já havia fallback `timezone.localtime()` na fábrica quando não se passa `agora` e `localdate()` quando não se passa `hoje` (`app/chamados/services/fabrica_da_celula.py:84-86`, `:136`). Há leitura `_tz.now()` no caminho `julgar_celula`, apesar do parâmetro `agora`; é código fora dos sítios alterados (`app/ponto/services/cartorio.py:1668`, `:1671`).

## 31/10 10:40 · sessao 275443782328 · - · - · origem=Ronald (tmux)

Mapa de linhagem, somente por inspeção de código.

| Campo | Quem produz/escreve | Quem lê no caminho dos quatro sítios |
|---|---|---|
| `cel.trabalha` | Campo triestado: `app/escala/models.py:1654`. Geração/regeneração obtêm `valor` de `eh_dia_trabalho_calculado`: `app/ponto/portas/celula.py:243`, `:590`; escrevem em `:247`, `:260`, `:595`, `:612`. Fora da vigência vira `False`: `:231`; reversão deriva do DNA anterior: `:532`. | Juiz único lê em `app/chamados/juizes.py:408`; os quatro emissores o chamam em `app/chamados/services/fabrica_da_celula.py:80`, `app/chamados/services/materializacao.py:160`, `app/ponto/services/cluster_espurio.py:26`, `app/ponto/management/commands/detectar_intervalo_ausente.py:49`. |
| `ata.tipo_dia` | Grade normal produz trabalho/folga em `app/escala/utils.py:727`, `:752`; grade por turno em `:1743`, `:1749`; overlays escrevem folga/falta/ausência em `:1118`, `:1174`, `:1180`. Cartório copia para ata em `app/ponto/services/cartorio.py:566`. | Juiz lê em `app/chamados/juizes.py:408`; leitor genérico lê em `app/escala/services/leitor_celula.py:344`; fábrica também o consulta transitivamente em `app/chamados/services/declaracao_texto.py:90`, `:95`. |
| `ata.sem_ancora` | Grade normal: `work is None` em `app/escala/utils.py:729`, saída em `:752`; grade por turno deriva `eh_dia_trabalho(...) is None` em `:1701`; ramo intermitente deriva falta de âncora de batidas em `:1456`, repassa em `:1514`. Cartório copia/coage em `app/ponto/services/cartorio.py:569`. | Nenhum dos quatro emissores mantém leitura direta após a cura; leitor genérico repassa em `app/escala/services/leitor_celula.py:362`. |
| `ata.lampadas` | Produtor é `lampadas_do_dia` em `app/ponto/services/cartorio.py:427`, com `append` em `:500`, `:504`, `:506`, `:543`, `:554`; `ata_do_dia` chama-o em `:564` e inclui resultado em `:566`. Dispensa modifica luzes em `:614`; teto temporal modifica em `:711`. | Diretos: `app/chamados/services/materializacao.py:158`, `app/ponto/services/cluster_espurio.py:29`, `app/ponto/management/commands/detectar_intervalo_ausente.py:62`. Fábrica: `app/chamados/services/fabrica_da_celula.py:49` → `app/chamados/services/disputa_emissao.py:827` → `app/chamados/services/declaracao_texto.py:82` → `app/escala/services/leitor_celula.py:93`. |

Os três campos da ata persistem pela mesma porta: `CelulaDia.lavrar_veredito` delega em `app/escala/models.py:1781`; a porta recebe/copia a ata em `app/ponto/portas/celula.py:697`, atribui em `:711` ou `:725` e salva por `salvar_e_emitir` em `:732`, cujo `save` e evento estão no mesmo `atomic` em `:639`. O julgamento em lote usa `save=False` e entrega os campos a `carimbar`: `app/ponto/services/cartorio.py:1520`, `:1535`.

Há atribuições de ata em memória fora da lavra, mas sem nova derivação de lâmpadas: `app/escala/utils.py:1826` copia a célula e chama o mesmo `lampadas_do_dia` em `:1832`; `app/ponto/portas/celula.py:678` também copia para leitura. `carimbar_precedencia` só modifica a chave `precedencia`, preservando o restante: `app/ponto/portas/celula.py:774`.

Cuidado com a equivalência declarada no comentário do diff: `sem_ancora` não é universalmente apenas `trabalha=None`; no ramo intermitente vem de `bool(_locs_i) and _anc_ix is None`: `app/escala/utils.py:1456`.

Outros predicados próprios relevantes:

- `app/chamados/services/declaracao_texto.py:95`: só `tipo_dia == 'trabalho'` tem marcos cobrados; a fábrica chega aqui por `app/chamados/services/fabrica_da_celula.py:49`. Para `trabalha=None` com tipo ausente, juiz libera (`app/chamados/juizes.py:408`), leitor deriva `indefinido` (`app/escala/services/leitor_celula.py:345`) e essa cláusula cala os marcos (`app/chamados/services/declaracao_texto.py:95`).
- `app/ponto/services/dia_do_colab.py:121`: `not cel.trabalha` descreve dia sem trabalho, incluindo `None`.
- `app/relatorios/dia_da_frota.py:47`: `not cel.trabalha` conta folga, incluindo `None`.
- `app/ponto/services/passivo_s84.py:96`: `TRABALHO if cel.trabalha else FOLGA`, incluindo `None` em folga.
- `app/ponto/marcos.py:155`, `:159`: veto próprio lê `trabalha`/veredito e veta `False`, sem ler `tipo_dia`.
- `app/ponto/services/flip_auto.py:208`, `:212`: decide folga/âncora com `is False` e `is not True`.
- Recortes ORM que excluem `None`: `app/chamados/management/commands/disparar_perguntas_competencia.py:76`, `app/chamados/services/fila_por_causa.py:192`, `app/ponto/services/vigia_entrada.py:79`, `:120`, `app/ponto/services/vigia_de_hora.py:21`.

Leitores adicionais dos campos incluem `app/ponto/juiz_batida.py:160`, `app/chamados/juizes.py:457`, `:888`, `:1008`, `app/ponto/services/lastro.py:89`, `app/ponto/worklist.py:22`, `app/folha/previa.py:84`, `app/folha/services/sem_furo.py:22`, `app/pautas/services.py:414`, `app/relatorios/cartao_pela_celula.py:289`; leitores de `sem_ancora` derivado incluem `app/folha/causas.py:96`, `app/relatorios/furos.py:157`, `:218`, `:237`, `app/inteligencia/calculadores.py:48`, `:143`, `app/ponto/services/fechamento.py:1018`, `app/ponto/supra_juiz.py:214`.

## 31/10 10:40 · sessao 98123c160ec5 · - · - · origem=Ronald (tmux)

**Mordida dos testes — previsão estática; execução e RED efetivo [não verificado].** [nao verificado]

| Reversão isolada | Resultado antigo para fixture limbo | Assert que a rejeita |
|---|---|---|
| Fábrica | `[]`, pois `None is not True` | `app/chamados/tests/test_f1c4_3_emissores.py:70`; guarda atual `app/chamados/services/fabrica_da_celula.py:80` |
| Materialização | `False`, pois pula a única célula | `app/chamados/tests/test_f1c4_3_emissores.py:86`; loop/retorno `app/chamados/services/materializacao.py:156`, `:169` |
| Cluster | `[]`, pois pula a única célula | `app/chamados/tests/test_f1c4_3_emissores.py:101`; loop/retorno `app/ponto/services/cluster_espurio.py:23`, `:34` |
| Intervalo | `[]`, pois pula a única célula | `app/chamados/tests/test_f1c4_3_emissores.py:117`; loop/retorno `app/ponto/management/commands/detectar_intervalo_ausente.py:46`, `:86` |

A reversão parcial para `dia_sem_trabalho(cel) or ata.get('sem_ancora')` também é rejeitada nos sítios cluster/intervalo: a fixture grava `sem_ancora=True` quando `trabalha=None`, e os asserts exigem o dia retornado. `app/chamados/tests/test_f1c4_3_emissores.py:55`, `:101`, `:117`.

Há controles positivos `True/'trabalho'` nos quatro helpers; os controles de folga usam **None/'folga'**, que a guarda velha já descartava. Logo estes controles não isolam a nova exclusão de **True/'folga'**. `app/chamados/tests/test_f1c4_3_emissores.py:73`, `:76`, `:89`, `:92`, `:104`, `:107`, `:120`, `:123`.

Os novos testes não incluem `False`, tipo ausente/vazio, `True + sem_ancora=True`, nem verificam a chamada ao juiz mediante spy/patch: chamam funções reais e verificam somente resultados. Um clone equivalente do predicado poderia satisfazer esses asserts. `app/chamados/tests/test_f1c4_3_emissores.py:54`, `:64`, `:79`, `:95`, `:110`.

**Escopo real:** fábrica retorna motivos, e só `perguntar_pela_celula` chama o escritor; o teste chama apenas `motivos_a_perguntar`. Materialização testa uma segunda opinião de paridade. Cluster testa detecção, não `reconciliar_colab`. Intervalo já tem emissão aposentada: reconcilia legado e depois executa `continue`. `app/chamados/services/fabrica_da_celula.py:111`, `:146`, `:151`; `app/chamados/services/materializacao.py:128`, `:438`, `:818`; `app/ponto/services/cluster_espurio.py:69`, `:86`; `app/ponto/management/commands/detectar_intervalo_ausente.py:147`, `:154`.

**Imports/query/chokepoint:**

- Os quatro novos imports são internos às funções, não imports novos de módulo; o juiz apenas lê `cel.ata` e `cel.trabalha`, sem ORM nem relógio em seu corpo. `app/chamados/services/fabrica_da_celula.py:78`; `app/chamados/services/materializacao.py:141`; `app/ponto/services/cluster_espurio.py:20`; `app/ponto/management/commands/detectar_intervalo_ausente.py:37`; `app/chamados/juizes.py:407`.
- Há dependência circular **preexistente** entre `chamados.juizes` e `chamados.models`: juiz importa models no topo; models reexporta juiz e materialização ao final. Os imports locais evitam introduzir a aresta durante definição dos quatro módulos; ausência de erro em execução [não verificado]. `app/chamados/juizes.py:6`; `app/chamados/models.py:3135`, `:3140`.
- O caminho quente existe: signal síncrono da batida chama julgamento; cartório chama cluster e fábrica. `app/ponto/signals.py:85`, `:93`, `:131`, `:135`; `app/ponto/services/cartorio.py:793`, `:797`, `:798`, `:800`.
- Embora o juiz não acrescente consulta explícita, a mudança passa células antes puladas para consultas e escritas posteriores: fábrica consulta perguntas e pode gerar perguntas; cluster consulta dedup e abre chamados. Portanto “delta total zero na batida” não está demonstrado. `app/chamados/services/fabrica_da_celula.py:103`, `:111`, `:151`; `app/ponto/services/cluster_espurio.py:56`, `:62`, `:65`, `:86`.
- O selo existente cobra tetos e quantidade de `on_commit`; tetos dos cenários de batida com furos são 191/241 quando fecham e 134/194 quando ficam. As respectivas fixtures criam `trabalha=True`; não demonstram o custo do novo universo limbo. `app/ponto/tests/test_selo_chokepoint_escrita.py:157`, `:158`, `:163`, `:164`, `:212`, `:218`, `:340`, `:380`.
- Os novos testes não medem consultas ou imports: o arquivo inteiro contém fixtures e asserts de retorno. O teto após a alteração, custo real de produção e import completo da aplicação estão [não verificado]. `app/chamados/tests/test_f1c4_3_emissores.py:21`, `:123`.

Nenhum comando de teste foi executado e nenhum arquivo foi escrito. [não verificado] [nao verificado]

## 31/10 10:19 · sessao 026ee072896a · E3-STATUS · censo · origem=Code

Censo estático da cópia fornecida. Estado do banco, execução dos testes e correspondência com um commit Git: **[não verificado]**. [nao verificado]

**1. Cadastro × realidade — L-084**

A lista completa **fica em memória no motor e no resultado; o fechamento não a persiste**. `dias_cadastro_x_realidade` é atributo do `@dataclass ResultadoMes`, nasce vazia a cada cálculo e recebe a lista acumulada no retorno. O comando de diff confirma essa ausência de persistência e captura as instâncias do motor para obter o relato. Referências: `app/ponto/motor_calculo_v2.py:218`, `app/ponto/motor_calculo_v2.py:239`, `app/ponto/motor_calculo_v2.py:2221`, `app/ponto/motor_calculo_v2.py:2657`, `app/ponto/management/commands/diff_reclassificar_partido.py:169`, `app/ponto/management/commands/diff_reclassificar_partido.py:188`.

Os escritores encontrados são:

- `_janela_he`: acrescenta a acusação `l084_do_dia`, deduplicada por data — `app/ponto/motor_calculo_v2.py:1552`, `app/ponto/motor_calculo_v2.py:1560`.
- `_acusa_cadastro_x_realidade`: acrescenta as pontas fora da janela, chamada para entrada e saída — `app/ponto/motor_calculo_v2.py:1608`, `app/ponto/motor_calculo_v2.py:1648`, `app/ponto/motor_calculo_v2.py:1686`.
- `aplicar_tolerancia`: acrescenta o relato quando o cadastro não descreve as duas pontas — `app/ponto/motor_calculo_v2.py:1926`, `app/ponto/motor_calculo_v2.py:1930`.
- O fechamento concatena os relatos das fatias no resultado composto, ainda em memória — `app/ponto/services/fechamento.py:96`, `app/ponto/services/fechamento.py:278`.

Os leitores atuais encontrados são o espelho, que traduz o relato em pendências e expõe o subconjunto acima do teto; o oráculo de dono, que extrai as datas; e os diagnósticos/diffs. Referências: `app/ponto/services/espelho.py:751`, `app/ponto/services/espelho.py:1263`, `app/ponto/management/commands/e6_oraculo.py:150`, `app/ponto/management/commands/e6_oraculo.py:178`, `app/ponto/management/commands/pautas_discordantes.py:102`, `app/ponto/management/commands/diff_janela_he.py:70`, `app/ponto/management/commands/diff_reclassificar_partido.py:188`.

Também existem `Censo.cxr` e `Rubricas.detalhe['cadastro_x_realidade']`, em memória. Esse detalhe **não é copiado para `DiaPago.detalhe`**: o lavrador lê das rubricas apenas `valores` e `nao_decide`. Referências: `app/ponto/calculador/periodo.py:286`, `app/ponto/calculador/periodo.py:790`, `app/ponto/calculador/regras.py:857`, `app/ponto/services/dia_pago.py:320`, `app/ponto/services/dia_pago.py:329`, `app/ponto/services/dia_pago.py:343`.

Há persistências relacionadas, mas de outras estruturas: a tela Cadastro × Realidade lê assinaturas do esmeril, gravadas em `MetricaSnapshot.detalhe`; a C9 guarda por item o `dono` derivado do oráculo. Nenhuma dessas escritas conserva a lista completa do motor com todos os relatos L-084. Referências: `app/escala/services/cadastro_realidade.py:635`, `app/ponto/services/esmeril_espelho.py:324`, `app/ponto/services/esmeril_espelho.py:330`, `app/ponto/services/teto_na_lista.py:86`, `app/ponto/services/teto_na_lista.py:113`.

**2. Feriado no caminho do status**

`status_da_lavra` é pura: recebe fatos e não consulta calendário diretamente. Entretanto, o caminho **combina grade/célula com consulta dinâmica à precedência A2**; não depende exclusivamente do DNA. Referências: [app/ponto/status_dia.py:57](/home/ronald/codex_ws/ae0502e59fd6dc6cc647154e864643a7158a30b3/app/ponto/status_dia.py:57), `app/ponto/status_dia.py:122`.

Os ramos encontrados são:

- **Grade que alimenta o status:** o cartório monta a grade; ela consulta `eh_dia_trabalho` e usa marcos do DNA. Havendo célula, `eh_dia_trabalho` devolve `cel.trabalha`; sem célula, calcula novamente. Nesse fallback, trabalho pode virar folga por feriado se `trabalha_em_feriado=False` e o ciclo for `6x1`, `5x2` ou `semanal`. O builder legacy consulta o mesmo método. Referências: `app/ponto/services/cartorio.py:931`, `app/escala/utils.py:1548`, `app/escala/utils.py:1550`, `app/escala/utils.py:725`, `app/escala/models.py:32`, `app/escala/models.py:1071`, `app/escala/models.py:1073`, `app/escala/models.py:1340`, `app/escala/models.py:1358`, `app/escala/models.py:1362`.

- **Consulta explícita dos fatos do status:** somente quando `veredito == 'indefinida'`, `fatos_do_status` chama `fatos_do_dia` e `folga_humana_ou_feriado`. A2 consulta os feriados aplicáveis, testa a data e aplica precedência. Se o vínculo trabalha em feriado e o dia é previsto, o feriado fica apenas no pagamento; caso contrário, participa como candidato à natureza do dia. Referências: `app/ponto/status_dia.py:122`, `app/ponto/status_dia.py:126`, `app/ponto/precedencia.py:209`, `app/ponto/precedencia.py:245`, `app/ponto/precedencia.py:299`, `app/ponto/precedencia.py:330`, `app/ponto/precedencia.py:335`, `app/ponto/precedencia.py:349`.

- **Fonte territorial:** essas consultas usam `core.models.Feriado`, com alcance nacional, estadual ou municipal. O lugar é `colaborador.posto.municipio`; sem município, só o nacional alcança. A2 também consulta o nome do feriado. Referências: `app/core/feriados.py:27`, `app/core/feriados.py:60`, `app/core/feriados.py:62`, `app/core/feriados.py:64`, `app/core/feriados.py:91`, `app/core/feriados.py:106`, `app/ponto/precedencia.py:364`.

- **Depois do julgamento unitário:** `julgar_celula` também chama `aplicar_precedencia_na_celula`, que consulta A2 e carimba a precedência na ata. Essa chamada acontece depois do julgamento e não recalcula o status. Referências: `app/ponto/services/cartorio.py:1674`, `app/ponto/services/cartorio.py:1675`, `app/ponto/services/cartorio.py:1718`, `app/ponto/services/cartorio.py:1723`.

O resultado de `indefinida + folga_humana` é `folga` sem batida ou `folga_trabalhada` com batida; sem natureza humana vencedora, é `escala_suspeita`. Nos demais vereditos, a classificação lê `tipo_dia`/previsto da grade. Decisão do admin tem prioridade própria. Referências: `app/ponto/status_dia.py:83`, `app/ponto/status_dia.py:88`, `app/ponto/status_dia.py:93`, `app/ponto/status_dia.py:102`, `app/ponto/portas/celula.py:694`.

**3. `isento_ponto`, hash e rejulgamento**

A isenção é lida nestes pontos da lavra:

- A grade consulta `core.constants.isento_ponto(colab)` e aplica o preenchimento presumido dos marcos — `app/escala/utils.py:865`, `app/escala/utils.py:872`, `app/escala/utils.py:1790`.
- O cartório passa `c.isento_ponto` ao classificador nos dois ramos; o classificador retorna sem acusações para isento — `app/ponto/services/cartorio.py:1029`, `app/ponto/services/cartorio.py:1212`, `app/ponto/supra_juiz.py:195`.
- O cartório passa a flag aos fatos do status no ramo normal, no nunca-bateu e na complementação D5 — `app/ponto/services/cartorio.py:734`, `app/ponto/services/cartorio.py:1046`, `app/ponto/services/cartorio.py:1563`.
- Os fatos excluem marcos presumidos `isencao` da contagem de batidas; a regra usa a isenção na cobertura parcial, no `ok` sem batida e no rótulo — `app/ponto/status_dia.py:85`, `app/ponto/status_dia.py:95`, `app/ponto/status_dia.py:107`, `app/ponto/status_dia.py:130`.

Hoje, `impressao_insumos` calcula SHA-256 das partes concatenadas com `|`, contendo:

- Batidas: `pk:tipo:timestamp.isoformat()`;
- Status da cobertura;
- Chamados: `pk:status_local`;
- Somente `dna_versao` e `marcos` do DNA;
- Condicionalmente: `veto`, `teto`, versão `regra`, `enc`, quantidade de `fio` e cobertura parcial, inclusive seu estado de período quando fornecido.

Referências: [app/ponto/services/cartorio.py:324](/home/ronald/codex_ws/ae0502e59fd6dc6cc647154e864643a7158a30b3/app/ponto/services/cartorio.py:324), `app/ponto/services/cartorio.py:333`, `app/ponto/services/cartorio.py:339`, `app/ponto/services/cartorio.py:344`, `app/ponto/services/cartorio.py:352`, `app/ponto/services/cartorio.py:362`, `app/ponto/services/cartorio.py:377`, `app/ponto/services/cartorio.py:385`, `app/ponto/services/cartorio.py:390`.

**`isento_ponto`, feriado e `CelulaDia.trabalha` não entram diretamente no hash.** Há testes `expectedFailure` específicos para essas lacunas. A versão `regra` representa versão do código, não a régua CCT. Referências: `app/ponto/tests/test_d3_impressao_cobre_insumos.py:167`, `app/ponto/tests/test_d3_impressao_cobre_insumos.py:201`, `app/ponto/tests/test_d3_impressao_cobre_insumos.py:213`, `app/ponto/tests/test_d3_impressao_cobre_insumos.py:255`.

O hash persistido é `CelulaDia.impressao`. O cartório compara-o ao hash atual nos dois ramos: igualdade, julgamento anterior e veredito permitem pular; divergência conduz ao julgamento normal. Com `apply_`, lavra veredito/status/ata e grava o novo hash e carimbos. Isso não regenera o DNA. Referências: `app/escala/models.py:1713`, `app/ponto/services/cartorio.py:1000`, `app/ponto/services/cartorio.py:1157`, `app/ponto/services/cartorio.py:1559`, `app/ponto/services/cartorio.py:1579`, `app/ponto/portas/celula.py:800`.

Na operação D5, hash divergente **não provoca rejulgamento naquela execução**: a célula entra em `status_fila_cartorio`. Hash em dia e status ausente permitem completar só o status. Referências: `app/ponto/services/cartorio.py:719`, `app/ponto/services/cartorio.py:729`, `app/ponto/services/cartorio.py:1158`.

A porta `carimbar` também lê o hash para evitar gravação repetida; `celula_veredito_velho` o lê para snapshot diagnóstico. Referências: `app/ponto/portas/celula.py:796`, `app/ponto/management/commands/celula_veredito_velho.py:110`.

Os **goldens literais** estão em `test_o219_versao_da_regra.py`: cinco casos — vazio, uma batida, duas batidas + chamado, veto e teto. Dois testes exigem os valores exatos, inclusive com `regra=None`; outro preserva o golden fora do horizonte. Referências: `app/ponto/tests/test_o219_versao_da_regra.py:39`, `app/ponto/tests/test_o219_versao_da_regra.py:47`, `app/ponto/tests/test_o219_versao_da_regra.py:94`, `app/ponto/tests/test_o219_versao_da_regra.py:99`, `app/ponto/tests/test_o219_versao_da_regra.py:141`.

**4. Competência exportada**

A recusa fica no **command `lavrar_status_competencia`**: `if competencia_entregue(emp.pk, mes, ano)`, imprime `ENTREGUE` e retorna antes de chamar `julgar_colab(... completar_status=True)`. Referências: [app/ponto/management/commands/lavrar_status_competencia.py:40](/home/ronald/codex_ws/ae0502e59fd6dc6cc647154e864643a7158a30b3/app/ponto/management/commands/lavrar_status_competencia.py:40), `app/ponto/management/commands/lavrar_status_competencia.py:55`.

O predicado exige **`ExportacaoDominio` vigente e não invalidada para empresa/mês/ano**. Vigente é o registro mais recente por `(gerado_em, pk)`; não basta existir uma exportação histórica. Referências: `app/folha/export.py:569`, `app/folha/export.py:572`, `app/folha/models.py:209`, `app/folha/models.py:220`.

Essa guarda não é universal no serviço genérico: `julgar_colab` e `julgar_celula` não consultam `competencia_entregue` nessas entradas. O teste D5 verifica especificamente a recusa do command. Referências: `app/ponto/services/cartorio.py:739`, `app/ponto/services/cartorio.py:776`, `app/ponto/services/cartorio.py:1700`, `app/ponto/services/cartorio.py:1715`, `app/ponto/tests/test_e3_status_lavrado.py:287`.

Outros consumidores diretos do predicado:

| Consumidor | Uso e referência |
|---|---|
| `competencia_fechada_para_celula` | Compõe entrega **OU** holerite publicado — `app/folha/export.py:599`. |
| `txt_vigente_x_gravado` | Obtém o TXT vigente — `app/folha/export.py:636`. |
| Preview de exportação | Lê o registro da entrega — `app/folha/views.py:354`. |
| Ranking HE | Define carimbo lavrado — `app/folha/services/ranking_he.py:89`. |
| Esmeril | Limita a busca retroativa — `app/ponto/services/esmeril_espelho.py:58`. |
| Marcos da competência | Define `exportada` — `app/ponto/services/fechamento.py:1505`. |
| HE pendente lavrado | Seleciona competência anterior ainda não entregue — `app/ponto/services/he_pendente_lavrado.py:223`. |
| Vigia do fio | Consulta entrega da competência do dia — `app/chamados/services/vigia_fio.py:787`. |

O gerador usa indiretamente o predicado composto `competencia_fechada_para_celula`; essa proteção de geração/regeneração é distinta da guarda do command D5. Referências: `app/ponto/management/commands/gerar_celulas.py:173`, `app/ponto/management/commands/gerar_celulas.py:203`.

## 31/10 10:17 · sessao 2e5aa3a351d1 · - · - · origem=Ronald (tmux)

Questão 2 — caminho verificado, sem execução de testes: [nao verificado]

- **Não é exclusivamente DNA:** `status_da_lavra` é pura, recebe `tipo_dia`, batida, previsto, isenção, veredito e `folga_humana`; `fatos_do_status` extrai `tipo_dia`/minutos/batidas da grade, mas consulta a lei A2 **quando `veredito == 'indefinida'`**. Referências: `app/ponto/status_dia.py:57`, `:122`, `:125`, `:126`, `:130`, `:133`.

- **Ramo direto de feriado na composição dos fatos:** `fatos_do_status → fatos_do_dia → fatos_do_periodo`. A2 busca município do posto e feriados aplicáveis; pergunta `data in feriados`; usa `CelulaDia.trabalha` para o previsto (fallback aritmético se ausente); mantém trabalho quando `feriado and previsto is True and ec.trabalha_em_feriado`; nos demais casos inclui feriado e decide pela precedência. Referências: `app/ponto/precedencia.py:209`, `:239`, `:242`, `:245`, `:299`, `:305`, `:316`, `:322`, `:325`, `:330`, `:335`, `:349`. `folga_humana_ou_feriado` retorna verdadeiro quando a faixa vencedora é `feriado`: `app/ponto/precedencia.py:189`.

- **Efeito no status:** `indefinida + folga_humana` resulta em `folga` sem batida e `folga_trabalhada` com batida; sem esse fato resulta em `escala_suspeita`. Ausência integral e falta sem batida são avaliadas antes. Para veredito diferente de `indefinida`, a consulta A2 não ocorre; `tipo_dia='folga'` produz `folga` ou `folga_trabalhada`. Referências: `app/ponto/status_dia.py:83`, `:85`, `:88`, `:90`, `:91`, `:93`, `:102`.

- **Ramo upstream de feriado na grade:** cartório monta a grade (`app/ponto/services/cartorio.py:931`); o builder pergunta `eh_dia_trabalho` e usa marcos congelados (`app/escala/utils.py:1548`, `:1550`; leitura de DNA em `:84`, `:103`). `eh_dia_trabalho` devolve `cel.trabalha` quando existe célula; **só sem célula** chama cálculo vivo (`app/escala/models.py:1066`, `:1071`, `:1073`). Nesse fallback, `base is True` pode virar folga por feriado (`:1340`): exige vínculo que não trabalha em feriado e ciclo `6x1/5x2/semanal`, e consulta `core.feriados.eh_feriado` pelo município do posto (`:1357`, `:1358`, `:1360`, `:1362`, `:1363`; lista de ciclos em `:32`). A grade escreve `tipo_dia='folga'` ou `'trabalho'`: `app/escala/utils.py:1743`, `:1749`. O dispatcher também tem legacy: `app/escala/utils.py:2026`, `:2029`; legacy pergunta o mesmo método `eh_dia_trabalho`: `:725`.

- **Fonte do calendário de feriados:** consulta `core.models.Feriado`, nacional para todos, estadual pela UF e municipal por UF+município; município vem de `colaborador.posto.municipio`. Sem município, apenas nacional passa pelos predicados. Referências: `app/core/feriados.py:27`, `:29`, `:57`, `:60`, `:62`, `:64`, `:70`, `:82`, `:83`, `:91`, `:95`, `:106`, `:113`, `:114`. A2 também consulta o nome do feriado para o seu resultado: `app/ponto/precedencia.py:364` → `app/core/feriados.py:98`.

- **Porta/admin:** `_status_alvo` prioriza decisão do admin e chama `status_do_admin`, sem avaliar feriado; para cartório chama `status_da_lavra` com os fatos. Referências: `app/ponto/portas/celula.py:693`, `:694`, `:695`, `:698`; mapeamento admin em `app/ponto/status_dia.py:39`, `:51`.

- **Testes que documentam a distinção:** teste puro mostra feriado sobre célula de trabalho vencendo `indefinida` (`app/ponto/tests/test_e3_status_lavrado.py:110`, `:111`); teste de costura assegura A2 chamada para `indefinida`, sem nova chamada para `concorde` (`:134`, `:135`, `:137`, `:138`); integração cria feriado nacional e exige `(indefinida, folga)` (`app/ponto/tests/test_e3_status_indefinida_a2_na_lavra.py:74`, `:76`, `:79`). Testes apenas lidos; resultado de execução [não verificado].

- **Limite relacionado ao hash:** há teste `expectedFailure` que registra que feriado novo não move a impressão, porque não entra diretamente nem pelo DNA congelado: `app/ponto/tests/test_d3_impressao_cobre_insumos.py:201`, `:203`, `:206`.

## 31/10 10:17 · sessao a200f0ac5780 · - · - · origem=Ronald (tmux)

**Q1 — a lista completa da L-084 fica em memória; não encontrei persistência dela em campo/tabela.** Ela é atributo do `@dataclass ResultadoMes`, não modelo ORM; nasce vazia por cálculo e é entregue no resultado. Referências: `app/ponto/motor_calculo_v2.py:218`, `app/ponto/motor_calculo_v2.py:239`, `app/ponto/motor_calculo_v2.py:2221`, `app/ponto/motor_calculo_v2.py:2657`. O comentário e a captura de instâncias no comando de diff explicitam que `recalcular_fechamento_mes` não a persiste: `app/ponto/management/commands/diff_reclassificar_partido.py:169`, `app/ponto/management/commands/diff_reclassificar_partido.py:188`.

Quem escreve na lista do motor:

- `_janela_he` acusa a L-084 do dia, com deduplicação por data e `causa='l084_do_dia'`: `app/ponto/motor_calculo_v2.py:1544`, `app/ponto/motor_calculo_v2.py:1552`, `app/ponto/motor_calculo_v2.py:1560`.
- `_acusa_cadastro_x_realidade` faz `append` das pontas fora da janela; os chamadores acusam chegada e saída: `app/ponto/motor_calculo_v2.py:1593`, `app/ponto/motor_calculo_v2.py:1608`, `app/ponto/motor_calculo_v2.py:1648`, `app/ponto/motor_calculo_v2.py:1686`.
- `aplicar_tolerancia` acusa as duas pontas distantes, incluindo o dia do juiz e deltas, e devolve `(0, 0)`: `app/ponto/motor_calculo_v2.py:1926`, `app/ponto/motor_calculo_v2.py:1930`, `app/ponto/motor_calculo_v2.py:1935`, `app/ponto/motor_calculo_v2.py:1943`.
- O fechamento apenas concatena as listas das fatias no resultado composto: `app/ponto/services/fechamento.py:96`, `app/ponto/services/fechamento.py:278`.

Quem lê hoje:

- O espelho traduz o relato do motor em pendências de HE e expõe um subconjunto das chegadas acima do teto: `app/ponto/services/espelho.py:751`, `app/ponto/services/espelho.py:1263`.
- `e6_oraculo.Insumos.cxr()` transforma a lista em conjunto de datas; `dono()` passa a pertinência da data ao juiz de dono: `app/ponto/management/commands/e6_oraculo.py:150`, `app/ponto/management/commands/e6_oraculo.py:156`, `app/ponto/management/commands/e6_oraculo.py:178`.
- Os comandos de diagnóstico/diff contam ou capturam a lista: `app/ponto/management/commands/diff_janela_he.py:70`, `app/ponto/management/commands/diff_reclassificar_partido.py:188`. `pautas_discordantes` também chama `Ins...cxr()`: `app/ponto/management/commands/pautas_discordantes.py:102`.

Há equivalentes **também em memória** no calculador: `Censo.cxr` recebe as acusações de pontualidade (`app/ponto/calculador/periodo.py:286`, `app/ponto/calculador/periodo.py:790`); `Rubricas.detalhe['cadastro_x_realidade']` recebe a lista (`app/ponto/calculador/regras.py:857`). Apesar do JSONField `DiaPago.detalhe` existir (`app/ponto/models.py:378`), o lavrador de `DiaPago` copia apenas `Rubricas.valores` e `Rubricas.nao_decide`, não `Rubricas.detalhe`: `app/ponto/services/dia_pago.py:320`, `app/ponto/services/dia_pago.py:329`, `app/ponto/services/dia_pago.py:339`. Portanto esse caminho não persiste a CxR.

**A tela “Cadastro x Realidade” não é consumidora direta da lista completa do motor.** Sua `lista()` lê assinaturas do esmeril por `ler_lavra()` e combina fontes de cadastro: `app/escala/services/cadastro_realidade.py:560`, `app/escala/services/cadastro_realidade.py:635`, `app/escala/services/cadastro_realidade.py:639`. O esmeril grava outra estrutura em `MetricaSnapshot.detalhe` e lê o snapshot mais recente: `app/ponto/services/esmeril_espelho.py:324`, `app/ponto/services/esmeril_espelho.py:330`, `app/ponto/services/esmeril_espelho.py:345`.

Existem persistências **derivadas**, sem guardar a lista completa: a C9 do teto armazena por item o `dono` obtido de `Ins...dono(d)` (`app/ponto/services/teto_na_lista.py:86`) e grava o documento em `MetricaSnapshot.detalhe` (`app/ponto/services/teto_na_lista.py:113`). Isso conserva classificação do juiz para certos dias, não a lista `dias_cadastro_x_realidade` com todos os relatos.

Cuidado com comentário envelhecido em `app/ponto/calculador/periodo.py:281`: ele diz que o campo de produção carrega “só causas de janela de HE”, mas o código atual também escreve a acusação de `aplicar_tolerancia` e a causa `l084_do_dia`: `app/ponto/motor_calculo_v2.py:1930`, `app/ponto/motor_calculo_v2.py:1561`.

## 31/10 10:17 · sessao ee46650b7bc1 · - · - · origem=Ronald (tmux)

4. A recusa fica no **command** `lavrar_status_competencia`: após contar as células, faz `if competencia_entregue(emp.pk, mes, ano)`, imprime `ENTREGUE` e retorna. Só depois dessa guarda chama `julgar_colab(... completar_status=True)`. Referências: `app/ponto/management/commands/lavrar_status_competencia.py:39`, `:40`, `:41`, `:42`, `:55`.

O predicado é **existe ExportacaoDominio vigente e não invalidada para empresa+mês+ano**: filtra a tripla, passa por `anotar_vigencia` e `so_vigentes`, rejeita o vigente com `invalidada_em` preenchido e retorna booleano; `registro=True` devolve o registro ou `None`. Referência: `app/folha/export.py:569–575`. O vigente é o maior `(gerado_em, pk)` de cada tripla, não simplesmente qualquer exportação existente: `app/folha/models.py:209–225`; `so_vigentes` lê `e_vigente`: `app/folha/models.py:192–200`.

Essa guarda **não está no serviço genérico do cartório**: `julgar_colab` entra em `_julgar_colab_corpo` e trata `completar_status` sem consultar `competencia_entregue` (`app/ponto/services/cartorio.py:739–778`); a própria documentação adverte que uma chamada com janela ampla/`--forcar` alcançaria competência exportada (`app/ponto/services/cartorio.py:756–759`). `julgar_celula` guarda futuro, inexistência e turno ainda aberto e depois chama `julgar_colab`, sem guarda de exportação nessa entrada (`app/ponto/services/cartorio.py:1700–1717`).

O teste específico do bloqueio é `test_D5_competencia_entregue_nao_se_toca`: simula `competencia_entregue=True`, executa o lote com `--apply`, exige `ENTREGUE` e que nenhum `status_dia` tenha sido preenchido (`app/ponto/tests/test_e3_status_lavrado.py:287–294`). Os testes do predicado cobrem sem TXT, vigente, vigente revogado, substituição e empate de carimbo (`app/folha/tests/test_k6_competencia_entregue.py:60–91`). Execução desses testes: [não verificado].

Outros consumidores diretos de `competencia_entregue` encontrados:

- `competencia_fechada_para_celula`: compõe entrega **OU** holerite do colaborador com lote publicado; `app/folha/export.py:598–609`. O gerador lê esse composto em `_competencias_exportadas` e `fonte_da_lavra`: `app/ponto/management/commands/gerar_celulas.py:173`, `:203`.
- `txt_vigente_x_gravado`: obtém o TXT vigente para comparar com o gravado atual; `app/folha/export.py:636`, `:648–658`.
- Preview da exportação: mostra quem/quando exportou; `app/folha/views.py:354`, `:360–364`.
- Ranking HE `_carimbo`: devolve `lavrado` quando todas as empresas representadas foram entregues; `app/folha/services/ranking_he.py:77–91`.
- `esmeril_espelho.janela`: encerra a busca retroativa na primeira competência entregue; `app/ponto/services/esmeril_espelho.py:55–63`.
- `marcos_da_competencia`: calcula o marco `exportada`; `app/ponto/services/fechamento.py:1500–1512`.
- `he_pendente_lavrado.competencias_a_lavrar`: inclui a anterior apenas enquanto não entregue; `app/ponto/services/he_pendente_lavrado.py:220–225`.
- `vigia_fio.competencia_do_dia_entregue`: converte o dia na competência pela janela da empresa e consulta o predicado, com memo opcional; `app/chamados/services/vigia_fio.py:787–797`. Seus leitores são o censo por dono (`app/chamados/services/vigia_fio.py:769`), a cerca do reconciliador (`app/chamados/reconciliador.py:715–720`), a seleção de perguntas vivas no pós-estado do chamado (`app/chamados/models.py:332–350`) e a guarda da costura para destino terminal (`app/chamados/services/fio.py:765–771`).

## 31/10 09:43 · sessao 60db60690637 · DISJUNTOR-B3-TOPO · verificador · origem=Code

O delta corrige os exemplos procedentes da triagem, mas **não garante equivalência entre a identificação no contador e no topo**: os reconhecedores continuam diferentes (`bin/disjuntor.py:84`, `bin/disjuntor.py:475`, `bin/disjuntor.py:494`).

1. **Resposta aos procedentes**

| Procedente | Resultado no código atual |
|---|---|
| Branco depois da primeira linha encerrava o bloco | **Resolvido:** branco agora apenas avança; não encerra (`bin/hook_stop_fila1.py:433`). |
| `O9 (C)` e `O9 NOTURNO-FOO` calavam B3 de `O9` | **Resolvido nesses exemplos:** cada token incorpora sua cauda pela `_identidade`; a comparação exige pertinência exata ao conjunto (`bin/disjuntor.py:488`, `bin/disjuntor.py:505`, `bin/hook_stop_fila1.py:417`). |
| Falta de caso para lookbehind | **Coberto:** testa `PRE-OBRA-X` e `PRE_OBRA-X`; há mutante retirando lookbehind (`bin/tests/test_disjuntor.sh:349`, `bin/tests/test_disjuntor.sh:648`, `bin/tests/test_disjuntor.sh:779`). |
| Falta de caso para DESENHO e ID em linhas cruzadas | **Coberto:** cenário cruzado e mutante correspondente (`bin/tests/test_disjuntor.sh:351`, `bin/tests/test_disjuntor.sh:781`). |
| Falta de positivo na primeira linha | **Coberto:** pergunta na linha 1; mutante restringe a leitura à linha 2 (`bin/tests/test_disjuntor.sh:345`, `bin/tests/test_disjuntor.sh:782`). |
| Falta de cobertura para leitura parcial após `OSError` | **Coberto:** preserva o trecho lido, verifica também `_linha_do_topo` e testa erro antes de qualquer linha; há mutante (`bin/hook_stop_fila1.py:443`, `bin/tests/test_disjuntor.sh:665`, `bin/tests/test_disjuntor.sh:788`). |
| Positivo aceitava apenas ausência do recado | **Resolvido no selo:** exige JSON com `decision=block`, motivo iniciado por `siga: `, ausência do recado e nenhuma nova `FALHA`; há mutante que lança exceção (`bin/tests/test_disjuntor.sh:337`, `bin/tests/test_disjuntor.sh:791`). |

2. **Onde a identidade ainda falha**

**A composição usa a mesma função; o reconhecimento não é equivalente.** O contador chama `_identidade` em `bin/disjuntor.py:482`; o topo chama em `bin/disjuntor.py:506`. Entretanto, o contador aceita markdown com `_`, enquanto o lookbehind do topo impede um ID imediatamente precedido por `_` (`bin/disjuntor.py:84`, `bin/disjuntor.py:494`).

Contraexemplos confirmados em memória:

| Entrada | Contador | Topo |
|---|---|---|
| Prompt `Leitura final da obra _OBRA-X_`; pergunta `PERGUNTA DE DESENHO _OBRA-X_` | Produz `OBRA-X` | Produz conjunto vazio; **cobra pergunta já escrita nessa forma** (`bin/disjuntor.py:84`, `bin/disjuntor.py:494`, `bin/hook_stop_fila1.py:417`). |
| Prompt `obra O9a`; pergunta `DESENHO O9a`, para contador disparado de `O9` | Rejeita `O9a`: captura com `re.I`, depois reprova no `fullmatch` | Extrai `O9`; **cala seu recado** (`bin/disjuntor.py:84`, `bin/disjuntor.py:475`, `bin/disjuntor.py:494`, `bin/hook_stop_fila1.py:417`). |

Portanto, compartilhar `_identidade` não demonstra a promessa de “mesmas bordas”: há tanto rejeição indevida quanto aceitação divergente (`bin/disjuntor.py:491`, `bin/disjuntor.py:494`). Ocorrência desses exemplos em produção: **[nao verificado]**.

3. **Quem escreve, quem lê e a primeira linha**

Nos caminhos auditados, o construtor recebe a instrução de escrever a pergunta; o PreToolUse apenas emite essa instrução, e o Stop consulta o topo (`bin/disjuntor.py:941`, `bin/hook_stop_fila1.py:1049`). A escrita efetiva pelo construtor em produção é **[nao verificado]**.

O contador recebe a identidade de `_obra_de`, acumula por chave de obra e persiste pelo `_com_estado` em `logs/sessao/disjuntor_estado.json`; Stop e PreToolUse chamam a varredura (`bin/disjuntor.py:54`, `bin/disjuntor.py:521`, `bin/disjuntor.py:556`, `bin/disjuntor.py:244`, `bin/hook_stop_fila1.py:1050`, `bin/disjuntor.py:940`).

**O delta não muda qual primeira linha é devolvida para um mesmo conteúdo:** `_linha_do_topo` continua retornando `b[0]`; pular brancos passa a encontrar a segunda linha útil, preservando a primeira capturada (`bin/hook_stop_fila1.py:407`, `bin/hook_stop_fila1.py:433`, `bin/hook_stop_fila1.py:439`). O ramo atual do teto libera sem escrever PAREI no RELATO (`bin/hook_stop_fila1.py:1287`, `bin/hook_stop_fila1.py:1295`).

**No selo há reescrita de arquivos de fixture**, inclusive colocando a pergunta na primeira linha: `_topo` sobrescreve o RELATO de teste; o cenário com branco também o sobrescreve diretamente (`bin/tests/test_disjuntor.sh:72`, `bin/tests/test_disjuntor.sh:345`, `bin/tests/test_disjuntor.sh:347`).

Os leitores diretos são:

| Função lida | Chamadores |
|---|---|
| `_bloco_do_topo` | `_linha_do_topo`; `_disjuntor_contido`, para recados B3 (`bin/hook_stop_fila1.py:407`, `bin/hook_stop_fila1.py:1049`). |
| `_linha_do_topo` | `_esperando_com_prova`; `_orcamento_travado`; `_orcamento_contido`, na recuperação de erro; `_disjuntor_contido`, para B4; `_parei_do_em_curso` (`bin/hook_stop_fila1.py:656`, `bin/hook_stop_fila1.py:909`, `bin/hook_stop_fila1.py:1026`, `bin/hook_stop_fila1.py:1045`, `bin/hook_stop_fila1.py:1106`). |

4. **Casos, mutantes e lacunas**

Os procedentes têm casos e mutantes correspondentes, conforme a tabela acima; branco e cauda estão ligados aos mutantes específicos em `bin/tests/test_disjuntor.sh:783`, `bin/tests/test_disjuntor.sh:789` e `bin/tests/test_disjuntor.sh:790`.

Executei em memória o corpo Python de `b3_topo`: a base passa; os mutantes declarados `sem-lookbehind`, `parcial-vira-none`, `sem-cauda` e `regex-paralela` caem, respectivamente, nos casos 7, 10, 2 e 3 (`bin/tests/test_disjuntor.sh:648`, `bin/tests/test_disjuntor.sh:666`, `bin/tests/test_disjuntor.sh:643`, `bin/tests/test_disjuntor.sh:644`). A queda dos demais no selo integral é **[nao verificado]**.

**Nem todo ramo novo está protegido.** Mutar `_identidade` de `if cauda and '-' not in oid` para `if cauda` faz `obra OBRA-X (C)` virar `OBRA-X (C)`, contrariando o comportamento atual; mesmo assim, `b3_topo` passa em memória porque falta o caso de ID hifenado com cauda válida (`bin/disjuntor.py:488`, `bin/tests/test_disjuntor.sh:642`). Sobrevivência ao selo completo: **[nao verificado]**.

Também falta caso para normalização de cauda minúscula: remover `.upper()` não é distinguido pelos exemplos de cauda usados, todos já maiúsculos (`bin/disjuntor.py:488`, `bin/tests/test_disjuntor.sh:384`, `bin/tests/test_disjuntor.sh:642`, `bin/tests/test_disjuntor.sh:645`). Sobrevivência ao selo completo: **[nao verificado]**.

Não executei o selo integral: ele cria, altera e remove arquivos, contrariando a restrição desta verificação (`bin/tests/test_disjuntor.sh:37`, `bin/tests/test_disjuntor.sh:49`, `bin/tests/test_disjuntor.sh:697`, `bin/tests/test_disjuntor.sh:706`).

## 31/10 09:42 · sessao fbd435b3bfa8 · E3-STATUS · verificador · origem=Code

**1. Sim: o delta resolve o achado A por inspeção do código.** O cartório obtém `_par_da_lavra` pela função da própria porta e passa seu veredito efetivo a `fatos_do_status`. Assim, `indefinida` derivada dos códigos ativa a consulta A2 que antes era omitida. (`app/ponto/services/cartorio.py:1252`, `app/ponto/services/cartorio.py:1564`; `app/ponto/status_dia.py:122`, `app/ponto/status_dia.py:126`.)

**2. Quem produz, quem lê e igualdade com a lavra:**

| Chamador | Quem produz o argumento | É o veredito gravado/preservado? |
|---|---|---|
| Lavra normal | `classificar_dia` produz os códigos; o cartório define `_vd`; **`veredito_que_a_lavra_escreve`** resolve o par efetivo, incluindo preservação de decisão admin. (`app/ponto/services/cartorio.py:1207`, `app/ponto/services/cartorio.py:1220`, `app/ponto/services/cartorio.py:1245`, `app/ponto/services/cartorio.py:1252`; `app/ponto/portas/celula.py:655`.) | **Sim, no fluxo sequencial inspecionado:** o adaptador recebe `_par_da_lavra[0]`; a porta usa novamente a mesma função com `_vd/cods`. (`app/ponto/services/cartorio.py:1559`, `app/ponto/services/cartorio.py:1564`; `app/ponto/portas/celula.py:745`.) |
| Nunca-bateu | `classificar_dia` → `derivar_veredito`; o cartório substitui por `nunca_bateu` quando falta cobertura integral. (`app/ponto/services/cartorio.py:1026`, `app/ponto/services/cartorio.py:1031`, `app/ponto/services/cartorio.py:1041`.) | **Sim para célula sem decisão admin:** `_vd_nb` alimenta ambos. **Com admin, pode diferir:** o adaptador recebe `_vd_nb`, mas a porta preserva `cel.veredito` e calcula o status humano, ignorando esses fatos. (`app/ponto/services/cartorio.py:1042`, `app/ponto/services/cartorio.py:1047`; `app/ponto/portas/celula.py:739`, `app/ponto/portas/celula.py:694`.) |
| `_completar_status` | Lê **`cel.veredito`**, escrito pela lavra anterior; passa-o à porta e ao adaptador. (`app/ponto/services/cartorio.py:733`, `app/ponto/services/cartorio.py:735`; `app/ponto/portas/celula.py:770`.) | **Sim:** seus chamadores exigem veredito preenchido para fornecer um dia elegível, evitando derivação alternativa por valor vazio. (`app/ponto/services/cartorio.py:1000`, `app/ponto/services/cartorio.py:1002`, `app/ponto/services/cartorio.py:1157`, `app/ponto/services/cartorio.py:1159`.) |

O **leitor do argumento do adaptador** é `fatos_do_status`: usa-o para decidir consultar A2 e não o inclui no dicionário retornado. O **leitor que decide o status** é `status_da_lavra`, chamado pela porta com seu próprio `v` efetivo; esse mesmo `v` é atribuído à célula. Não identifiquei, nesses fluxos sequenciais, status decidido com outro veredito; a ressalva admin acima muda o argumento do adaptador, mas seus fatos são ignorados. (`app/ponto/status_dia.py:122`, `app/ponto/status_dia.py:133`; `app/ponto/portas/celula.py:694`, `app/ponto/portas/celula.py:698`, `app/ponto/portas/celula.py:752`, `app/ponto/portas/celula.py:770`.)

**3. O teste novo morde a reversão, por análise estática, e atravessa a implementação real.** Chama `julgar_colab(... apply_=True, forcar=True)` e relê a célula persistida; usa o classificador real e a porta real. Há congelamento do relógio, sem mock dessas funções no teste. (`app/ponto/tests/test_e3_status_indefinida_a2_na_lavra.py:29`, `app/ponto/tests/test_e3_status_indefinida_a2_na_lavra.py:65`, `app/ponto/tests/test_e3_status_indefinida_a2_na_lavra.py:66`; `app/ponto/services/cartorio.py:767`; `app/escala/models.py:1794`.)

Os dois RED exigem `PREVISTO_COM_DIA_INDEFINIDO` e `('indefinida', 'folga')`. Com esses códigos, `_vd=None`; reverter a linha para `_vd` deixa `folga_humana=False`, enquanto a porta continua derivando `indefinida`. A regra então retorna `escala_suspeita`, contrariando ambos RED; o controle permanece compatível. (`app/ponto/tests/test_e3_status_indefinida_a2_na_lavra.py:78`, `app/ponto/tests/test_e3_status_indefinida_a2_na_lavra.py:79`, `app/ponto/tests/test_e3_status_indefinida_a2_na_lavra.py:86`, `app/ponto/tests/test_e3_status_indefinida_a2_na_lavra.py:87`, `app/ponto/tests/test_e3_status_indefinida_a2_na_lavra.py:72`; `app/ponto/services/cartorio.py:1220`, `app/ponto/services/cartorio.py:1221`; `app/escala/models.py:1583`; `app/ponto/status_dia.py:121`, `app/ponto/status_dia.py:91`.)

**4. Nenhuma das cinco classes aparece como defeito novo deste delta:**

| Classe | Avaliação |
|---|---|
| Segundo juiz | Reutiliza a autoridade da lavra e consulta A2 pelo adaptador existente; A2 está registrada. (`app/ponto/services/cartorio.py:1252`; `app/ponto/portas/celula.py:745`; `app/ponto/status_dia.py:126`; `app/core/juizes.py:161`.) |
| Segundo escritor fora da porta | Guarda apenas o par local/transitório; veredito e status continuam escritos na porta. (`app/ponto/services/cartorio.py:1252`, `app/ponto/services/cartorio.py:1253`; `app/ponto/portas/celula.py:705`, `app/ponto/portas/celula.py:770`.) |
| Relógio fora da borda | Os trechos alterados não acrescentam leitura do relógio; a lavra recebe `agora`. (`app/ponto/services/cartorio.py:1252`, `app/ponto/services/cartorio.py:1559`, `app/ponto/services/cartorio.py:1564`.) |
| Derivação paralela | Calcular novamente pela **mesma função** mantém uma autoridade; o delta não copia a regra de derivação. (`app/ponto/services/cartorio.py:1252`; `app/ponto/portas/celula.py:745`.) |
| Selo/RED sem mordida | O selo AST continua aceitando qualquer `fatos_do_status=f`; o teste novo cobre semanticamente a reversão específica. (`app/ponto/tests/test_selo_veredito_so_pela_porta.py:287`; `app/ponto/tests/test_e3_status_indefinida_a2_na_lavra.py:79`, `app/ponto/tests/test_e3_status_indefinida_a2_na_lavra.py:87`.) |

Execução GREEN, execução do mutante, efeitos concorrentes, ocorrência/quantidade em produção e confirmação independente do hash Git: **[não verificado]**.

## 31/10 09:42 · sessao 2d7c58b7082a · - · - · origem=Ronald (tmux)

As duas rotas chamam `_identidade`: contador em `bin/disjuntor.py:482`; topo em `bin/disjuntor.py:506`; composição em `bin/disjuntor.py:488`. Porém, isso não torna os reconhecedores equivalentes: o contador aceita `_` como markdown e usa `re.I`, seguido de `fullmatch`; o topo usa lookbehind sem `re.I` (`bin/disjuntor.py:84`, `bin/disjuntor.py:475`, `bin/disjuntor.py:494`).

Contraexemplos confirmados com Python `-B`, somente funções puras:

- `_obra_de('Leitura final da obra _OBRA-X_') == 'OBRA-X'`, mas `obras_da_linha('PERGUNTA DE DESENHO _OBRA-X_') == set()`; a pergunta é rejeitada. O `_` é permitido no contador e proibido imediatamente antes do ID no topo (`bin/disjuntor.py:84`, `bin/disjuntor.py:494`, `bin/hook_stop_fila1.py:417`).
- `_obra_de('obra O9a') is None`, enquanto `obras_da_linha('DESENHO O9a') == {'O9'}`; a pergunta cala B3 de `O9`. O contador captura `O9a` com `re.I` e rejeita no `fullmatch`; o topo captura somente `O9`, sem borda direita (`bin/disjuntor.py:84`, `bin/disjuntor.py:475`, `bin/disjuntor.py:494`).

Não encontrei identidade válida emitida pelo contador cujo texto cru, sem markdown/contexto adicional, não faça roundtrip pelo topo; demonstração exaustiva: [nao verificado].

Mutante plausível: remover `.upper()` de `_identidade`. O selo usa somente caudas já maiúsculas, `(C)` e `NOTURNO-FOO`; portanto não verifica a normalização de `(c)` nem de sufixo minúsculo (`bin/disjuntor.py:488`, `bin/tests/test_disjuntor.sh:384`, `bin/tests/test_disjuntor.sh:642`, `bin/tests/test_disjuntor.sh:645`). Hoje `P('O9 (C)', ['DESENHO O9 (c)'])` é verdadeiro e esse mutante o tornaria falso; sobrevivência no selo completo: [nao verificado].

## 31/10 09:42 · sessao 661999cfe410 · - · - · origem=Ronald (tmux)

No delta auditado, nenhuma das cinco classes se sustenta como defeito novo, por inspeção estática; execução dos testes e mutante **[não verificado]**. [nao verificado]

| Classe | Parecer e prova |
|---|---|
| Segundo juiz registrado | **Não.** A costura pede o veredito à autoridade já usada pela lavra: `app/ponto/services/cartorio.py:1252`; `app/ponto/portas/celula.py:745`. A2 continua chamada pelo adaptador (`app/ponto/status_dia.py:126`) e registrada em `app/core/juizes.py:161`. |
| Segundo escritor fora da porta | **Não.** O delta guarda uma tupla local e no mapa transitório (`app/ponto/services/cartorio.py:1252`, `app/ponto/services/cartorio.py:1253`); a porta escreve status e veredito (`app/ponto/portas/celula.py:705`, `app/ponto/portas/celula.py:770`). |
| Relógio fora da borda | **Não acrescenta leitura.** A lavra recebe `agora` (`app/ponto/services/cartorio.py:1559`); o teste fixa o relógio (`app/ponto/tests/test_e3_status_indefinida_a2_na_lavra.py:29`). As leituras em `julgar_celula` são preexistentes ao delta fornecido (`app/ponto/services/cartorio.py:1691`, `app/ponto/services/cartorio.py:1712`, `app/ponto/services/cartorio.py:1715`). |
| Derivação paralela da fonte única | **Não.** O resultado de `veredito_que_a_lavra_escreve` alimenta os fatos (`app/ponto/services/cartorio.py:1252`, `app/ponto/services/cartorio.py:1564`); a porta chama a mesma regra (`app/ponto/portas/celula.py:745`). Continua existindo o predicado inline da tela, anterior ao delta (`app/colaboradores/services/calendario.py:801`), com migração E4 explicitada (`app/ponto/precedencia.py:185`). |
| Selo/RED sem mordida | **O selo AST continua limitado:** exige apenas presença do argumento (`app/ponto/tests/test_selo_veredito_so_pela_porta.py:166`) e aceita `fatos_do_status=f` (`app/ponto/tests/test_selo_veredito_so_pela_porta.py:287`). **O teste novo tem mordida estática sobre a reversão:** usa julgamento real com apply e releitura da célula (`app/ponto/tests/test_e3_status_indefinida_a2_na_lavra.py:65`, `app/ponto/tests/test_e3_status_indefinida_a2_na_lavra.py:66`), exigindo `indefinida/folga` em ambos RED (`app/ponto/tests/test_e3_status_indefinida_a2_na_lavra.py:79`, `app/ponto/tests/test_e3_status_indefinida_a2_na_lavra.py:87`). Reverter a `_vd` suprime A2, pois `_vd` é `None` nesse cenário com códigos (`app/ponto/services/cartorio.py:1220`, `app/ponto/services/cartorio.py:1221`), e o adaptador só pergunta com `indefinida` (`app/ponto/status_dia.py:122`); sem esse fato, a regra retorna `escala_suspeita` (`app/ponto/status_dia.py:91`). |

Entre cálculo do par e lavra, o corpo não reatribui `_vd`, `cods`, `cel.veredito` ou `cel.veredito_via`; as entradas da lavra continuam `_vd/cods` (`app/ponto/services/cartorio.py:1252`, `app/ponto/services/cartorio.py:1559`). Existem chamadas intermediárias a reconciliação, ressuscitação e emissão (`app/ponto/services/cartorio.py:1281`, `app/ponto/services/cartorio.py:1415`, `app/ponto/services/cartorio.py:1508`); a ausência de efeitos transitivos em todos os sinais/concorrência **[não verificado]**.

## 31/10 09:41 · sessao 3f3aa9587f73 · - · - · origem=Ronald (tmux)

Os procedentes têm cenário e mutante correspondente:

| Achado | Caso | Mutante |
|---|---|---|
| Branco entre linhas úteis | `bin/tests/test_disjuntor.sh:347` | `bin/tests/test_disjuntor.sh:783` |
| Cauda distingue obra | `bin/tests/test_disjuntor.sh:642`–`:647` | `bin/tests/test_disjuntor.sh:789`–`:790` |
| Lookbehind | `bin/tests/test_disjuntor.sh:648` | `bin/tests/test_disjuntor.sh:779` |
| DESENHO e ID em linhas cruzadas | `bin/tests/test_disjuntor.sh:351` | `bin/tests/test_disjuntor.sh:781` |
| Positivo na primeira linha | `bin/tests/test_disjuntor.sh:345` | `bin/tests/test_disjuntor.sh:782` |
| Leitura parcial após OSError | `bin/tests/test_disjuntor.sh:665`–`:669` | `bin/tests/test_disjuntor.sh:788` |
| Exceção não vale como positivo | `_b3_cala` verifica JSON, recado e FALHA, `bin/tests/test_disjuntor.sh:337`–`:341` | `bin/tests/test_disjuntor.sh:791` |

Executei em memória o heredoc exato de `b3_topo`: base passa; os mutantes `sem-lookbehind`, `parcial-vira-none`, `sem-cauda` e `regex-paralela` caem respectivamente nos casos 7, 10, 2 e 3 (`bin/tests/test_disjuntor.sh:643`, `:644`, `:648`, `:666`, `:779`, `:788`–`:790`). Os 13 mutantes B3-topo têm âncora única e substituição sintaticamente válida (`bin/tests/test_disjuntor.sh:775`–`:794`).

**Há mutante plausível sobrevivente ao novo `b3_topo`:** trocar `if cauda and '-' not in oid` por `if cauda` em `bin/disjuntor.py:488`. Verificado em memória: `b3_topo` continua rc=0, mas `_obra_de('obra OBRA-X (C)')` muda de `OBRA-X` para `OBRA-X (C)`; os casos não incluem ID hifenado com cauda válida (`bin/tests/test_disjuntor.sh:642`–`:650`). Sobrevivência ao selo completo: **[nao verificado]**.

Não executei o selo integral: cria/remove árvore e grava mutantes (`bin/tests/test_disjuntor.sh:37`–`:49`, `:697`–`:706`).

## 31/10 09:41 · sessao 0cd79261025c · - · - · origem=Ronald (tmux)

| Chamador | Quem produz o argumento de `fatos_do_status(veredito=...)` | Relação com a célula |
|---|---|---|
| Lavra normal | `classificar_dia` produz `cods`; `_vd` começa `None`, podendo virar `cobrado`; `veredito_que_a_lavra_escreve` resolve `_par_da_lavra`, cujo primeiro elemento vai ao adaptador. (`app/ponto/services/cartorio.py:1207`, `app/ponto/services/cartorio.py:1220`, `app/ponto/services/cartorio.py:1245`, `app/ponto/services/cartorio.py:1252`, `app/ponto/services/cartorio.py:1564`.) | Mesma autoridade e mesmas entradas da lavra: esta chama novamente `veredito_que_a_lavra_escreve`; sua regra preserva admin ou resolve `veredito or derivar_veredito(codigos)`. (`app/ponto/services/cartorio.py:1559`; `app/ponto/portas/celula.py:655`, `app/ponto/portas/celula.py:658`, `app/ponto/portas/celula.py:745`.) |
| Nunca-bateu | `classificar_dia` produz `_cods_nb`; `derivar_veredito` traduz; o cartório substitui por `nunca_bateu` quando falta cobertura integral. (`app/ponto/services/cartorio.py:1026`, `app/ponto/services/cartorio.py:1031`, `app/ponto/services/cartorio.py:1032`, `app/ponto/services/cartorio.py:1041`.) | O mesmo `_vd_nb` explícito alimenta lavra e adaptador. **Ressalva admin:** o adaptador ainda recebe `_vd_nb`, mas a porta preserva o veredito humano e deriva o status por `status_do_admin`, ignorando os fatos recebidos. (`app/ponto/services/cartorio.py:1042`, `app/ponto/services/cartorio.py:1047`; `app/ponto/portas/celula.py:738`, `app/ponto/portas/celula.py:739`, `app/ponto/portas/celula.py:694`.) |
| `_completar_status` | Lê `cel.veredito`, resultado de uma lavra anterior; passa esse valor tanto à porta quanto ao adaptador. (`app/ponto/services/cartorio.py:733`, `app/ponto/services/cartorio.py:735`; escritor: `app/ponto/portas/celula.py:770`.) | Os chamadores só fornecem `dia` elegível quando existe veredito preenchido; portanto, nesse fluxo não ocorre fallback de um veredito vazio para a derivação pelos protestos. (`app/ponto/services/cartorio.py:1000`, `app/ponto/services/cartorio.py:1002`, `app/ponto/services/cartorio.py:1157`, `app/ponto/services/cartorio.py:1159`, `app/ponto/services/cartorio.py:729`.) |

O argumento `veredito` do adaptador **apenas controla a consulta A2**: ele não integra o dicionário retornado. O leitor que decide o status, `status_da_lavra`, recebe da própria porta o `v` efetivo; esse mesmo `v` é atribuído a `cel.veredito`. Assim, nos caminhos sequenciais inspecionados, não aparece status decidido com um veredito diferente do gravado/preservado. A ressalva do ramo nunca-bateu/admin é diferença no argumento do adaptador, sem divergência do status, porque o caminho admin ignora esses fatos. (`app/ponto/status_dia.py:122`, `app/ponto/status_dia.py:133`; `app/ponto/portas/celula.py:694`, `app/ponto/portas/celula.py:698`, `app/ponto/portas/celula.py:739`, `app/ponto/portas/celula.py:752`, `app/ponto/portas/celula.py:770`.)

Concorrência, valores históricos concretos e ocorrência em produção: **[não verificado]**.

## 31/10 09:41 · sessao 5e906c1afdbc · - · - · origem=Ronald (tmux)

**Morde a reversão por análise estática.** Os dois RED exigem o código `PREVISTO_COM_DIA_INDEFINIDO` e a tupla `('indefinida', 'folga')`; o controle exige `('indefinida', 'escala_suspeita')`. (`app/ponto/tests/test_e3_status_indefinida_a2_na_lavra.py:71`, `:72`, `:78`, `:79`, `:86`, `:87`.)

Com aquele código, `_vd` permanece `None`, pois só muda para `cobrado` quando não há códigos. Reverter a chamada para `_vd` deixa `folga_humana=False`; a porta continua derivando `indefinida` pelos códigos e entrega esse veredito à regra de status, que devolve `escala_suspeita`. Logo, os dois RED contradizem a saída do mutante; o controle permanece compatível. (`app/ponto/services/cartorio.py:1220`, `:1221`, `:1245`, `:1564`; `app/ponto/status_dia.py:121`, `:122`, `:88`, `:91`; `app/ponto/portas/celula.py:658`, `:745`, `:698`; `app/escala/models.py:1583`, `:1597`.)

**Exercita a implementação real.** `_lavrar` cria a célula, chama `julgar_colab(... apply_=True, forcar=True)` e relê a persistência com `refresh_from_db`; `julgar_colab` importa o classificador real e chama o corpo, que monta a grade real. O método de modelo delega à porta; a porta chama `status_da_lavra`. (`app/ponto/tests/test_e3_status_indefinida_a2_na_lavra.py:61`, `:65`, `:66`; `app/ponto/services/cartorio.py:767`, `:776`, `:931`; `app/escala/models.py:1794`; `app/ponto/portas/celula.py:698`.)

Não há mock de julgamento, A2, adaptador ou regra no teste; há congelamento do relógio com `freezegun`. (`app/ponto/tests/test_e3_status_indefinida_a2_na_lavra.py:17`, `:29`, `:32`, `:57`, `:74`, `:82`.)

Fixture coerente com o caminho: vínculo termina sete dias antes, célula mantém `trabalha=True` e geradora vencida, batida anterior garante adesão; a autoridade recusa a geradora fora da vigência, e o supra acusa cadastro sem exigir previsto positivo. (`app/ponto/tests/test_e3_status_indefinida_a2_na_lavra.py:49`, `:53`, `:62`; `app/ponto/services/cartorio.py:956`, `:964`; `app/escala/alimentacao.py:250`; `app/ponto/supra_juiz.py:168`, `:405`.)

Execução GREEN e execução do mutante: **[não verificado]**.

## 31/10 09:27 · sessao f29f24b0d110 · DISJUNTOR-B3-TOPO · verificador · origem=Code

**REPROVADO.** Há caminhos que liberam o Stop antes da cobrança B3 e uma colisão entre IDs admitidos pelo próprio contador (`bin/hook_stop_fila1.py:1224`, `bin/hook_stop_fila1.py:1231`, `bin/hook_stop_fila1.py:1298`; `bin/disjuntor.py:483`).

**(1) Onde o desenho falha**

- **ESPERANDO verdadeiro contorna a cobrança.** A ordem relevante é PAREI → ESPERANDO com prova → teto → B2 → B3/recados. Portanto, pergunta ausente ou na terceira linha não é cobrada quando uma saída anterior libera. `_libera()` encerra o processo (`bin/hook_stop_fila1.py:1224`, `:1229`, `:1282`, `:1293`, `:1298`, `:133`).
- **O selo não exercita essa espera verdadeira.** Os quatro casos novos usam `pid=1`; esse PID está sempre entre os ancestrais e é recusado como prova. Eles chegam à cobrança B3 pelo caminho “ESPERANDO sem prova” (`bin/tests/test_disjuntor.sh:335`, `:337`, `:339`, `:341`; `bin/hook_stop_fila1.py:477`, `:529`).
- **IDs hifenados semelhantes estão protegidos, mas IDs com cauda não.** A regex rejeita `PRE-OBRA-X` e `OBRA-X-OUTRA` para `OBRA-X`. Contudo, aceita `DESENHO O9 (C)` ou `DESENHO O9 NOTURNO-FOO` como pergunta de `O9`, embora `_obra_de()` produza três identidades distintas. Confirmei esses resultados em memória (`bin/hook_stop_fila1.py:414`–`:415`; `bin/disjuntor.py:81`–`:85`, `:483`–`:484`).
- **“Duas primeiras úteis” tem uma restrição adicional:** depois da primeira linha capturada, uma linha vazia encerra o bloco. `ESPERANDO`, branco, pergunta resulta em somente uma linha lida; pelo contrato literal de duas linhas úteis, a segunda pergunta deveria contar (`bin/hook_stop_fila1.py:430`–`:436`).
- **A validação é apenas lexical:** exige a substring maiúscula `DESENHO` e o ID na mesma linha. Aceita `REDESENHO OBRA-X` e `OBRA-X NAO TEM PERGUNTA DE DESENHO`, sem verificar QUAL PADRÃO/POR QUÊ (`bin/hook_stop_fila1.py:415`, `:1048`–`:1050`).

**(2) Quem escreve e quem lê**

| Dado | Escritor | Leitor |
|---|---|---|
| Pergunta no topo | O construtor, conforme instrução emitida pelo PreToolUse; no selo, `_topo()` grava a fixture (`bin/disjuntor.py:919`–`:921`; `bin/tests/test_disjuntor.sh:72`). | B3 lê `_bloco_do_topo()` e aplica `_pergunta_b3_escrita()` (`bin/hook_stop_fila1.py:1045`–`:1047`). |
| Primeira linha ESPERANDO/PAREI | Declaração manual; o teto atual apenas registra/libera, tendo abandonado a escrita automática de PAREI (`bin/hook_stop_fila1.py:1283`–`:1291`). | Ambos usam `_linha_do_topo()`, derivada do mesmo bloco (`bin/hook_stop_fila1.py:407`, `:652`, `:1102`). |
| Contador B3 | `varrer_b3()` acumula agentes reprovados por obra, deduplica e dispara no limiar; `_com_estado()` persiste em `logs/sessao/disjuntor_estado.json` (`bin/disjuntor.py:54`, `:533`–`:550`, `:242`–`:245`). | Stop e PreToolUse chamam a varredura; `b3_disparados()` consulta o estado (`bin/hook_stop_fila1.py:1046`; `bin/disjuntor.py:918`, `:566`). |
| Rearme B3 | CLI chama `rearma_b3()`, preserva agentes antigos em `rearmado` e zera contagem/disparo (`bin/disjuntor.py:1138`, `:560`–`:563`). | A varredura ignora esses agentes antigos (`bin/disjuntor.py:535`). |

Escrever a pergunta **cala o recado**, sem rearmar o B3: o quarto verificador continua negado pelo PreToolUse, que decide pela chave da obra sem consultar o topo (`bin/hook_stop_fila1.py:1047`; `bin/disjuntor.py:915`–`:922`).

`_rteto()` restaura outro contador, `logs/hook_stop_fila1.json`; ele não restaura o estado B3 (`bin/tests/test_disjuntor.sh:333`–`:334`; `bin/hook_stop_fila1.py:35`; `bin/disjuntor.py:54`).

**(3) O selo morde cada ramo novo? Não.**

| Comportamento | Cobertura existente |
|---|---|
| Aceitar segunda linha | Caso `:335`–`:336`; mutante `:715`. |
| Rejeitar terceira linha | Caso `:337`–`:338`; mutante `:718`. |
| Rejeitar `OBRA-X-OUTRA` | Caso `:339`–`:340`; mutante `:716`. |
| Exigir `DESENHO` | Caso `:341`–`:342`; mutante `:717`. |

Todas as referências dessa tabela são de `bin/tests/test_disjuntor.sh`.

Há mutantes que preservam os resultados desses casos:

- Retirar **somente o lookbehind** da regex aceita `PRE-OBRA-X`; o selo testa apenas a fronteira direita (`bin/hook_stop_fila1.py:414`; `bin/tests/test_disjuntor.sh:339`).
- Separar os testes em `any(DESENHO)` e `any(id)` aceita `['DESENHO OBRA-Y', 'OBRA-X']`; falta esse caso cruzado (`bin/hook_stop_fila1.py:415`; `bin/tests/test_disjuntor.sh:335`–`:342`).
- Limitar B3 à última linha lida perde a pergunta válida na primeira linha; todos os positivos novos a colocam na segunda (`bin/hook_stop_fila1.py:1045`; `bin/tests/test_disjuntor.sh:335`–`:342`).
- Reverter `return out or None` para `return None` perde a leitura parcial após `OSError`. Esse ramo novo não tem cenário nem mutante declarado; também afeta ESPERANDO/PAREI pelo leitor compartilhado (`bin/hook_stop_fila1.py:439`, `:407`; `bin/tests/test_disjuntor.sh:72`, `:715`–`:718`).

Além disso, o positivo exige somente ausência do recado: uma exceção convertida em `FALHA + None` pode satisfazê-lo. Falta conferir a decisão esperada e ausência de falha (`bin/tests/test_disjuntor.sh:336`; `bin/hook_stop_fila1.py:1057`–`:1062`).

Validei funções isoladas em memória. Não executei o selo, pois ele cria, altera e apaga arquivos; sobrevivência no selo completo: **[nao verificado]** (`bin/tests/test_disjuntor.sh:37`–`:49`, `:637`–`:646`).

_947 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
