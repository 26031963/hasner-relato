# CODEX-ESTADO

> Ultima resposta processada: **01/11/2026 03:52** · publicado em **01/11/2026 03:54** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 01/11 03:52 · sessao a060f571054d · V3-DOBRA-SEM-NOTURNA · verificador · origem=Code

**1. Os esperados estão corretos; a cobertura dos mutantes exige uma ressalva em `extra_min`.** O texto exato dos mutantes da volta 1 não foi fornecido: sua identidade literal está [não verificado].

As contas abaixo usam o cadastro explícito do teste: intervalo de 60 minutos, previsto de 480, prorrogação após 05h ligada e hora reduzida preservada (`app/ponto/tests/test_v3_dobra_sem_noturna.py:40`, `app/ponto/tests/test_v3_dobra_sem_noturna.py:46`).

| Caso | Conta pela regra declarada | Esperados |
|---|---|---|
| 22h–06h | Trabalho: 480−60=420 min. Art.73 + Súm.60 II: noite 22h–06h menos intervalo =420 min; hora reduzida: 420÷52,5=8 h. A dobra coloca os 420 min em HE100; todos são noturnos. | HE100=HE100-feriado=7 h; HE100-noturna=7 h; noturnas legais=8 h; HE50 e HE50-noturna=0. Corretos (`app/ponto/tests/test_v3_dobra_sem_noturna.py:12`, `:64`, `:74`). |
| Gêmeo sem dobra, 22h–06h | Mesmo trabalho e mesma noite; 420<480, portanto excedente zero. | Noturnas=8 h; HE100 e HE100-noturna=0. Corretos (`app/ponto/tests/test_v3_dobra_sem_noturna.py:78`, `:81`). |
| 21h–07h | Trabalho: 600−60=540 min. Excedente: 540−480=60 min. Noite prorrogada: 22h–07h=540, menos intervalo=480 min. Hora reduzida: 480÷52,5=9,142857 h, ou 548,571429 min legais. | HE100=HE100-feriado=9 h; HE50=0; HE100-noturna=8 h; HE50-noturna=0; `extra_min`=60. Gêmeo sem dobra: HE50=1 h. Todos corretos (`app/ponto/tests/test_v3_dobra_sem_noturna.py:88`, `:92`, `:96`, `:99`, `:102`). |
| Entrada 22h20, marco 22h | Trabalho: 460−60=400 min. L-084: desvios 20 e 0, portanto não recusa os marcos. Tolerância da casa: 20>10, cobra 20 integralmente. L-093: teto 480−400=80, não corta. | Atraso=20÷60 h; antecipada=0. Corretos (`app/ponto/tests/test_v3_dobra_sem_noturna.py:108`, `:111`, `:114`; `app/ponto/motor_calculo_v2.py:1934`, `:1994`, `:1176`). |
| Saída 05h30, marco 06h | Trabalho: 450−60=390 min. L-084: desvios 0 e 30, portanto julga. Tolerância: 30>10, cobra 30 integralmente. L-093: teto 480−390=90, não corta. | Antecipada=30÷60=0,5 h; atraso=0. Corretos (`app/ponto/tests/test_v3_dobra_sem_noturna.py:119`, `:122`, `:126`; `app/ponto/motor_calculo_v2.py:1934`, `:1996`, `:1176`). |

A Súm.264 aparece no cadastro como **split em horas de relógio**, subconjunto da HE; o fator reduzido é aplicado à rubrica `horas_noturnas`. Por isso 8 h de HE100-noturna e 9,142857 h noturnas legais no caso longo são compatíveis (`app/ponto/motor_calculo_v2.py:342`, `app/ponto/calculador/regras.py:835`, `app/ponto/calculador/regras.py:88`).

Sobre os três reforços:

- **HE100-feriado=9:** detecta um mutante que altere essa parcela no caso com excedente, mantendo eventualmente HE100 total correta (`app/ponto/tests/test_v3_dobra_sem_noturna.py:93`).
- **`extra_min`=60:** detecta resultado final errado, mas **não mata uma mutação restrita à atribuição do ramo de dobra**: a escrita em `regras.py:787` agora é sobrescrita pela escrita em `regras.py:813`. Esse mutante sobrevive sem alterar a saída observável (`app/ponto/tests/test_v3_dobra_sem_noturna.py:96`; `app/ponto/calculador/regras.py:787`, `:813`).
- **Antecipada de 30 minutos:** detecta zeramento apenas dessa rubrica na dobra; o gêmeo precisa valer 0,5 h e a dobra precisa igualá-lo (`app/ponto/tests/test_v3_dobra_sem_noturna.py:122`, `:123`).

**2. As triagens estão corretas quanto à origem, mas “preexistente” não basta para justificar enfileiramento pelas regras locais.** `CLAUDE.md:30` determina curar bug comprovado no caminho, sem enfileirar.

- **Tolerância:** comentário corrigido para os 10 minutos efetivamente usados; não altera os esperados de 20 e 30 minutos (`app/ponto/tests/test_v3_dobra_sem_noturna.py:106`; `app/ponto/motor_calculo_v2.py:60`, `:1994`).
- **Intervalos:** origem preexistente em `alimentacao.py:514`, mas o diff passa a expô-la também no split da dobra. Para o mesmo caso longo, omitindo o intervalo, o recuo de 540 minutos desde 07h chega a 22h e devolve **9 h**, contra **8 h** com o intervalo informado. Portanto, não foi criado pelo diff, mas compromete o resultado desse caminho integrado (`app/ponto/calculador/alimentacao.py:514`; `app/ponto/calculador/regras.py:828`; `app/ponto/motor_calculo_v2.py:395`, `:440`).
- **Crédito:** a substituição já existia. O crédito é abatido em `dia_pago.py:190`, mas a rubrica do calculador substitui o valor em `:328`; a regra do dia deliberadamente não abate crédito. Após a cura, chega atraso bruto onde antes chegava zero. Origem preexistente; descartá-lo apenas por isso contraria a regra local acima (`app/ponto/services/dia_pago.py:190`, `:328`; `app/ponto/calculador/regras.py:184`, `:855`).
- **Folga-feriado:** triagem correta. Esse ramo afirma HE100/feriado, declara os splits em `nao_decide` e retorna antes do trecho alterado; não é o mesmo zero afirmado (`app/ponto/calculador/regras.py:682`, `:704`, `:709`, `:735`).

**3. Escritores e leitores depois do diff:**

| Campo | Escrita | Leitura |
|---|---|---|
| `horas_saida_antecipada` | Alimentador passa pontualidade; regra aplica tolerância/teto e grava horas em `valores` (`app/ponto/calculador/alimentacao.py:496`, `:518`; `app/ponto/calculador/regras.py:264`, `:271`, `:856`). | Testes (`app/ponto/tests/test_v3_dobra_sem_noturna.py:122`); diagnóstico/comparação (`app/ponto/management/commands/diff_calculador.py:476`, `:501`, `:535`); lavratura recebe as rubricas e substitui o campo do `DiaPago` (`app/ponto/services/fechamento.py:1256`; `app/ponto/services/dia_pago.py:322`, `:328`, `:343`). A apresentação lê a linha lavrada em `app/ponto/services/dia_pago.py:916`. |
| `detalhe['extra_min']` | Excedente calculado em `regras.py:769`, sujeito à tolerância em `:770`; escrito na dobra em `:787` e novamente em `:813` (`app/ponto/calculador/regras.py:769`, `:787`, `:813`). | Assert direto (`app/ponto/tests/test_v3_dobra_sem_noturna.py:96`); diagnóstico HE50 (`app/ponto/management/commands/diff_calculador.py:497`). **Esse leitor explícito não executa na dobra**, pois exige HE50>0, enquanto a dobra fixa HE50=0 (`app/ponto/management/commands/diff_calculador.py:480`; `app/ponto/calculador/regras.py:786`). O detalhe inteiro ainda pode aparecer na impressão de divergências (`app/ponto/management/commands/diff_calculador.py:546`). |

A lavratura não copia `Rubricas.detalhe`: copia valores e registra nomes das recusas. Logo, `extra_min` não é persistido por esse bloco (`app/ponto/services/dia_pago.py:322`, `:329`).

**4. Não identifiquei outro defeito funcional nas linhas alteradas:** o `elif` preserva a dobra e libera split, noturno e pontualidade. A ressalva nova de cobertura é a atribuição intermediária de `extra_min` mascarada pela escrita posterior (`app/ponto/calculador/regras.py:795`, `:813`, `:820`, `:840`, `:851`).

Execução da suíte, morte dos mutantes originais e impacto em produção: **[não verificado]**. [nao verificado]

## 01/11 03:50 · sessao 66a13e78b458 · - · - · origem=Ronald (tmux)

Fluxo após diff, com referências locais:

- `horas_saida_antecipada`: `do_dia` inicializa zero apenas se decidível (`app/ponto/calculador/regras.py:641`); a dobra continua adiante (`:779-795`); `pontualidade_do_dia` aplica tolerância (`:264`), teto (`:271-272`) e soma minutos (`:274`); `do_dia` grava horas em `valores` (`:856`) e minutos no detalhe (`:858`). Sem dados/lei, declara `nao_decide` (`:619-625`); sem `dia`, remove o valor (`:863-868`).
- Alimentação: organiza pontualidade (`app/ponto/calculador/alimentacao.py:496-500`) e passa `dia`, marcos e pares para `do_dia` (`:501-519`).
- Leitura para gravar: fechamento monta rubricas (`app/ponto/services/fechamento.py:1239-1244`) e entrega a `lavrar` (`:1245-1264`). A lavratura primeiro carrega antecipada do motor (`app/ponto/services/dia_pago.py:185`), depois lê genericamente `valores` e substitui pelo calculador (`:320-328`), construindo `DiaPago` (`:343-345`). Não há `do_motor` nessa chamada (`app/ponto/services/fechamento.py:1245-1264`).
- Leitura no diff: imprime antecipada explicitamente (`app/ponto/management/commands/diff_calculador.py:474-479`), põe `valores` em `c_dia` (`:501`) e compara cada rubrica decidida (`:527-542`). Os testes leem antecipada em `app/ponto/tests/test_v3_dobra_sem_noturna.py:114` e `:122-124`.
- Leitor de apresentação: `rubricas_da_linha` lê `linha.horas_saida_antecipada` (`app/ponto/services/dia_pago.py:916`). Atingimento efetivo dessa versão pelo leitor em produção: [não verificado].

`detalhe['extra_min']`:

- A fórmula é excedente, `trab - limite`, sujeito à tolerância (`app/ponto/calculador/regras.py:767-771`). A dobra escreve o detalhe em `:787`; depois do diff, `:813` escreve novamente **o mesmo valor**. Para o caso longo: 540 − 480 = 60 (`app/ponto/tests/test_v3_dobra_sem_noturna.py:86-96`).
- Leitor explícito: `app/ponto/management/commands/diff_calculador.py:497`, mas está sob `--abrir-he50` **e HE50 > 0** (`:480`). Como a dobra fixa HE50=0 (`app/ponto/calculador/regras.py:786`), esse leitor explícito **não executa para dobra**.
- Na dobra, o detalhe inteiro ainda pode ser impresso quando há divergência e `--colab` (`app/ponto/management/commands/diff_calculador.py:543-548`).
- O teste lê diretamente a chave (`app/ponto/tests/test_v3_dobra_sem_noturna.py:96`). `dia_pago.lavrar` não copia `Rubricas.detalhe`; copia valores e nomes de recusas (`app/ponto/services/dia_pago.py:320-331`).

Nuance dos mutantes: o assert `extra_min == 60` verifica o resultado final, mas **não mata um mutante restrito à atribuição em `regras.py:787`**, porque `:813` a sobrescreve. Se o mutante altera a variável `extra` ou a escrita final, o assert pode mordê-lo; quais eram exatamente os mutantes da volta 1: [não verificado].

## 01/11 03:50 · sessao aca67ccff62d · - · - · origem=Ronald (tmux)

A triagem técnica dos três defeitos está correta quanto à origem:

- **Intervalos:** `alimentacao.py:514` já passa `intervalos=()`. O diff torna o split alcançável na dobra (`regras.py:779`, `regras.py:820`), expondo essa alimentação defeituosa também nesse ramo. No caso longo, sem intervalo informado, o recuo de 540 minutos desde 07:00 chega a 22:00: 540 minutos noturnos = **9 h**. Com intervalo, recua até 21:00; janela 22–07 menos 02–03 = 480 minutos = **8 h** (`motor_calculo_v2.py:378`, `motor_calculo_v2.py:440`, `regras.py:828`). É origem preexistente, embora agora observável na dobra (`app/ponto/calculador/alimentacao.py:514`).

- **Crédito de atraso:** o abatimento ocorre em `dia_pago.py:190`, seguido pela substituição da rubrica em `dia_pago.py:328`; a regra do dia expressamente não abate crédito (`regras.py:184`). Antes, o retorno antigo já entregava atraso zero afirmado; agora entrega atraso bruto, portanto o problema permanece e passa a aparecer com esse valor (`regras.py:641`, `regras.py:855`; retorno antigo: anexo `cura2.diff`). O overwrite não foi criado pelo diff (`app/ponto/services/dia_pago.py:328`).

- **Folga-feriado:** paga HE100 e HE100-feriado em `regras.py:682`, declara os splits em `nao_decide` em `regras.py:704`–`regras.py:711` e retorna antes do ramo alterado (`regras.py:735`). Portanto, não apresenta o mesmo zero afirmado nem é alcançado pela cura (`app/ponto/calculador/regras.py:735`).

Há uma distinção de governança: a regra local manda curar bug comprovado no caminho, mesmo preexistente, e proíbe enfileirá-lo (`CLAUDE.md:30`). Logo, “não foi introduzido pelo diff” é correto; “pode necessariamente ficar para fila” não decorre disso (`CLAUDE.md:30`). Nesta revisão, prevalece a instrução expressa do usuário de somente leitura [não verificado: instrução sem arquivo].

Não identifiquei defeito novo nas linhas alteradas: o `elif` preserva a substituição da HE normal pela dobra e permite executar os blocos independentes de split, noturno e pontualidade (`app/ponto/calculador/regras.py:779`, `app/ponto/calculador/regras.py:795`, `app/ponto/calculador/regras.py:820`, `app/ponto/calculador/regras.py:840`, `app/ponto/calculador/regras.py:851`).

## 01/11 03:42 · sessao 5e70cae2048d · - · - · origem=Ronald (tmux)

**O `if/elif` corrige o fluxo local, mas a cura ainda deixa uma divergência na porta real:** no caso 21h–07h, o teste calcula **8h de HE100 noturna**, enquanto a alimentação existente, que entrega `intervalos=()`, produz **9h**. O teste novo contorna essa alimentação. (`app/ponto/calculador/alimentacao.py:96`, `app/ponto/calculador/alimentacao.py:514`; `app/ponto/motor_calculo_v2.py:440`; anexo `teste_novo.py:46`.)

As referências abaixo usam as linhas da **base**, antes do diff; as linhas do teste anexo são contadas desde seu cabeçalho. (`app/ponto/calculador/regras.py:789`; anexo `teste_novo.py:1`.)

**1. Resultado final de `do_dia` com `_dobra > 0`.**

`_dobra > 0` não determina sozinho noturnas e pontualidade: elas dependem dos respectivos insumos e das recusas montadas antes de `val`. Rubrica recusada fica **ausente de `valores`**, com causa em `nao_decide`. (`app/ponto/calculador/regras.py:602`, `app/ponto/calculador/regras.py:609`, `app/ponto/calculador/regras.py:618`, `app/ponto/calculador/regras.py:627`, `app/ponto/calculador/regras.py:641`.)

Na tabela, `T` é o trabalhado; `S`, o intervalo suprimido após piso/cadastro/tolerância; `N`, os minutos noturnos legais; `H50n/H100n`, as somas retornadas por `he_noturna`; `A/B`, atraso/antecipada após o julgamento e teto. (`app/ponto/calculador/regras.py:600`, `app/ponto/calculador/regras.py:756`, `app/ponto/calculador/regras.py:823`, `app/ponto/calculador/regras.py:836`, `app/ponto/calculador/regras.py:847`.)

| Rubrica de `RUBRICAS` | Resultado geral após o diff | Caso anexo 22h–06h, dobra |
|---|---|---|
| `horas_trabalhadas` | `T/60` | **7h** |
| `horas_intra_indenizada` | `S/60` | **0** |
| `horas_extras_50` | **0** | **0** |
| `horas_extras_100` | `_dobra/60 = T/60` | **7h** |
| `horas_extras_100_feriado` | `_dobra/60` | **7h** |
| `horas_extras_50_noturna` | `H50n/60`, ou `nao_decide` sem pontas/prorrogação | **0** |
| `horas_extras_100_noturna` | `H100n/60`, ou a mesma recusa | **7h** |
| `horas_folga_trabalhada` | **0** neste caminho; `nao_decide` se `escala_certa=None` | **0** |
| `horas_noturnas` | `N/60`, ou `nao_decide` sem pares/CCT completa | **8h** |
| `horas_atraso` | `A/60`, ou `nao_decide` por lei/insumo ausente | **`nao_decide`** |
| `horas_saida_antecipada` | `B/60`, ou a mesma recusa | **`nao_decide`** |
| `horas_falta` | `nao_decide`: fonte Ausência | `nao_decide` |
| `horas_reflexo_dsr` | `nao_decide`: apuração mensal | `nao_decide` |
| `saldo_banco_horas` | `nao_decide`: apuração mensal | `nao_decide` |

Fontes da tabela: enumeração e recusas permanentes em `app/ponto/calculador/regras.py:400`, `app/ponto/calculador/regras.py:426`; valores e guardas em `app/ponto/calculador/regras.py:609`, `app/ponto/calculador/regras.py:740`, `app/ponto/calculador/regras.py:763`, `app/ponto/calculador/regras.py:779`, `app/ponto/calculador/regras.py:815`, `app/ponto/calculador/regras.py:835`, `app/ponto/calculador/regras.py:846`. O caso padrão do teste não fornece pontualidade; somente o último teste a fornece. (Anexo `teste_novo.py:40`, `teste_novo.py:55`, `teste_novo.py:106`.)

**Não há sobrescrita indevida de HE50/HE100/HE100_feriado abaixo do antigo retorno:** o `elif` impede executar o excedente na dobra; os blocos seguintes escrevem splits, noturno e pontualidade. `det['extra_min']` é sobrescrito em `:808`, mas pela **mesma expressão** de `:787`: continua representando o excedente, não a jornada dobrada — **0** no caso padrão e **60min** no caso longo. (`app/ponto/calculador/regras.py:786`, `app/ponto/calculador/regras.py:787`, `app/ponto/calculador/regras.py:790`, `app/ponto/calculador/regras.py:808`, `app/ponto/calculador/regras.py:815`.)

Há, porém, erro alcançado no **split** quando os intervalos não chegam: o bloco usa `he_pontas` como recebido, sem reconstruir pausas nem obrigar HE50 noturna a zero por reconhecer dobra. A coerência dessas pontas é responsabilidade da alimentação. (`app/ponto/calculador/regras.py:818`, `app/ponto/calculador/regras.py:823`; `app/ponto/calculador/alimentacao.py:91`.)

**2. Quem escreve e quem lê os campos alterados.**

| Campo | Escritor agora alcançado na dobra | Fonte da regra |
|---|---|---|
| `horas_noturnas` | `do_dia`, legais/60 | `noturnas_do_dia`, janela e desconto do motor |
| `horas_extras_50_noturna` e `horas_extras_100_noturna` | `do_dia`, soma por período/60 | `motor_calculo_v2.he_noturna` |
| `horas_atraso` e `horas_saida_antecipada` | `do_dia`, minutos/60 | `pontualidade_do_dia`, métodos emprestados do motor |
| `noturno_relogio_min`, `noturno_legais_min` | `do_dia`, valores arredondados no detalhe | Resultado de `noturnas_do_dia` |
| `he_pontas_periodos` | `do_dia`, quantidade de pontas | `len(he_pontas)` |
| `atraso_min`, `saida_antecipada_min` | `do_dia`, minutos no detalhe | Resultado de pontualidade |
| `cadastro_x_realidade` | `do_dia`, somente se houver acusações | Lista produzida pela tolerância/L-084 |

Escritas: `app/ponto/calculador/regras.py:828`, `app/ponto/calculador/regras.py:831`, `app/ponto/calculador/regras.py:840`, `app/ponto/calculador/regras.py:850`, `app/ponto/calculador/regras.py:857`. Fontes: `app/ponto/calculador/regras.py:85`, `app/ponto/calculador/regras.py:112`, `app/ponto/calculador/regras.py:264`, `app/ponto/calculador/regras.py:271`, `app/ponto/calculador/regras.py:823`.

Os consumidores relevantes são:

- **`dia_pago.lavrar`:** primeiro monta valores dos períodos do motor; depois substitui os campos presentes em `Rubricas.valores`, exceto os explicitamente reservados em `do_motor`. Grava pela mesma porta `delete/bulk_create`. (`app/ponto/services/dia_pago.py:173`, `app/ponto/services/dia_pago.py:320`, `app/ponto/services/dia_pago.py:327`, `app/ponto/services/dia_pago.py:357`.)
- **Detalhes não persistem automaticamente:** `lavrar` lê `valores` e os nomes de `nao_decide`, mas **não copia `Rubricas.detalhe`**. Assim, esses novos `noturno_*`, `he_pontas_periodos`, `atraso_min` e `cadastro_x_realidade` não chegam à linha por essa passagem. (`app/ponto/services/dia_pago.py:320`, `app/ponto/services/dia_pago.py:329`, `app/ponto/services/dia_pago.py:339`.)
- **`fechamento`:** o fechamento principal continua recebendo noturnas/splits e pontualidade do resultado do motor. O calculador alimenta a lavra **`versao='oraculo'`**, desde competência 10/2026. (`app/ponto/services/fechamento.py:893`, `app/ponto/services/fechamento.py:969`, `app/ponto/services/fechamento.py:1233`, `app/ponto/services/fechamento.py:1256`, `app/ponto/services/fechamento.py:1262`.)
- **`diff_calculador`:** lê valores, exclui rubricas declaradas cegas da comparação e imprime o detalhe inteiro no diagnóstico por colaborador divergente. Também lê especificamente `extra_min` para explicar HE50. (`app/ponto/management/commands/diff_calculador.py:497`, `app/ponto/management/commands/diff_calculador.py:501`, `app/ponto/management/commands/diff_calculador.py:517`, `app/ponto/management/commands/diff_calculador.py:532`, `app/ponto/management/commands/diff_calculador.py:546`.)
- **`porta_export` e testemunhas:** a soma mensal chamada pela porta usa o padrão **`motor`**, comparando seus campos com o fechamento; a testemunha diária citada compara horas trabalhadas, sem conferir individualmente os cinco campos novos da dobra. Portanto, esses contadores não demonstram a correção da versão `oraculo`. (`app/ponto/services/dia_pago.py:548`, `app/folha/porta_export.py:388`, `app/folha/porta_export.py:451`, `app/folha/porta_export.py:101`, `app/folha/porta_export.py:115`.)
- **Tela/papel:** `rubricas_do_dia` lê noturnas, atraso e antecipada da linha lavrada. A lista de produção CADASTRO × REALIDADE lê **`resultado.dias_cadastro_x_realidade` do motor**, não o novo detalhe da `Rubricas`. (`app/ponto/services/dia_pago.py:915`, `app/ponto/services/dia_pago.py:922`; `app/ponto/services/espelho.py:745`, `app/ponto/services/espelho.py:752`.)

**Outro problema na ordem das escritas:** `lavrar` abate crédito parcial do atraso primeiro e depois pode substituir esse resultado pelo atraso bruto do calculador; `pontualidade_do_dia` deliberadamente não abate crédito. Com atraso de 20min e crédito de 20min no mesmo dia, essa sequência pode lavrar novamente **20min no oráculo**, após ter calculado zero. O teste novo não fornece crédito; ocorrência em produção **[nao verificado]**. (`app/ponto/services/dia_pago.py:190`, `app/ponto/services/dia_pago.py:193`, `app/ponto/services/dia_pago.py:328`; `app/ponto/calculador/regras.py:184`; anexo `teste_novo.py:106`.)

**3. Checklist das classes de reprovação.**

| Classe | Avaliação do diff |
|---|---|
| **(a) Segundo juiz** | **Não introduz.** Continua usando a tolerância registrada e a autoridade de dobra existente; os métodos são importados/emprestados. (`app/core/juizes.py:198`, `app/core/juizes.py:277`; `app/ponto/calculador/regras.py:112`, `app/ponto/calculador/regras.py:290`.) |
| **(b) Segundo escritor fora da porta** | **Não introduz persistência nova.** A montagem continua em `do_dia`; a gravação continua em `dia_pago.lavrar`. Há a sobrescrita do crédito descrita acima, dentro dessa porta. (`app/ponto/calculador/regras.py:865`; `app/ponto/services/dia_pago.py:328`, `app/ponto/services/dia_pago.py:357`.) |
| **(c) Relógio fora da borda** | **Não identificado no diff/teste.** `do_dia` recebe os instantes; o teste constrói setembro/2026 fixo. `make_aware` não consulta “agora”, embora dependa do fuso ativo. (`app/ponto/calculador/regras.py:572`; anexo `teste_novo.py:33`, `teste_novo.py:37`; lei em `app/docs/CORTES.md:369`.) |
| **(d) Derivação paralela** | **Não acrescenta fórmula paralela.** Reutiliza os três blocos existentes. Contudo, a porta não entrega os mesmos intervalos usados na chamada do teste. (`app/ponto/calculador/regras.py:823`, `app/ponto/calculador/regras.py:836`, `app/ponto/calculador/regras.py:847`; `app/ponto/calculador/alimentacao.py:514`.) |
| **(e) RED sem mordida** | **Morde o retorno antigo e a troca indevida de `elif` por `if`.** Não cobre toda a integração nem todos os campos; mutantes abaixo. (`app/ponto/calculador/regras.py:789`, `app/ponto/calculador/regras.py:791`; anexo `teste_novo.py:68`, `teste_novo.py:74`, `teste_novo.py:93`, `teste_novo.py:110`.) |
| **(f) Ordem de trava** | **Não altera aquisição de locks.** Preserva as recusas antes de inicializar valores e mantém dobra antes do excedente. A ordem problemática de crédito/substituição está na lavra existente. (`app/ponto/calculador/regras.py:618`, `app/ponto/calculador/regras.py:641`, `app/ponto/calculador/regras.py:779`; `app/ponto/services/dia_pago.py:193`, `app/ponto/services/dia_pago.py:328`.) |
| **(g) Pouso antes do pré-requisito** | Suite inteira verde, árvore certificada e leitura final exigidas; comprovação desses atos **[nao verificado]**. Este laudo não certifica pouso. (`CLAUDE.md:505`, `CLAUDE.md:557`, `CLAUDE.md:560`.) | [nao verificado]

Mutantes que **o teste novo não pega**: [nao verificado]

- Trocar a escrita comum por `det['extra_min'] = 0` — nenhum teste afirma esse detalhe. (`app/ponto/calculador/regras.py:808`; anexo `teste_novo.py:62`, `teste_novo.py:86`, `teste_novo.py:102`.)
- Fazer `horas_saida_antecipada` sempre zero — o único caso de pontualidade termina no marco e exige justamente zero. (`app/ponto/calculador/regras.py:851`; anexo `teste_novo.py:106`, `teste_novo.py:112`.)
- Corromper `horas_extras_100_feriado` somente quando houver excedente — o teste longo não verifica essa rubrica. (`app/ponto/calculador/regras.py:785`; anexo `teste_novo.py:92`.)
- Manter a perda dos intervalos na alimentação — o teste fornece o intervalo diretamente a `do_dia`. (`app/ponto/calculador/alimentacao.py:514`; anexo `teste_novo.py:46`.)

RED/GREEN efetivamente executados e suite completa: **[nao verificado]**. [nao verificado]

**4. Contas dos números esperados.**

Para as chamadas isoladas do anexo, os números conferem com a REGRA fornecida e com as funções: janela prorrogada, desconto do intervalo, fator somente nas noturnas legais e split da HE em minutos de relógio. (`app/ponto/motor_calculo_v2.py:46`, `app/ponto/motor_calculo_v2.py:110`, `app/ponto/motor_calculo_v2.py:440`, `app/ponto/motor_calculo_v2.py:448`; `app/ponto/calculador/regras.py:85`, `app/ponto/calculador/regras.py:88`.)

| Caso | Trabalhado | Noturno de relógio | Noturnas legais | HE100 / feriado | HE100 noturna |
|---|---:|---:|---:|---:|---:|
| 22h–06h; pausa 02h–03h | `480−60=420min` | `420+60−60=420min` | `420×60/52,5÷60 = 8h` | **7h / 7h** | **7h** |
| 21h–07h; mesma pausa | `600−60=540min` | `420+120−60=480min` | `480×60/52,5÷60 = 9,142857h` | **9h / 9h** | **8h** |
| 22h20–06h; mesma pausa | `460−60=400min` | `400min` | **7,619048h** | **6,666667h / 6,666667h** | **6,666667h** |

Fontes: cadastro do anexo `teste_novo.py:40`, `teste_novo.py:42`, `teste_novo.py:43`; dobra integral em `app/ponto/calculador/regras.py:330`, `app/ponto/calculador/regras.py:347`; contas noturnas e split em `app/ponto/calculador/regras.py:85`, `app/ponto/motor_calculo_v2.py:440`.

O recuo por trabalho explica o split: no primeiro caso, recua **180min de 06h até 03h**, pula a pausa e recua **240min até 22h**; no segundo, recua **240min até 03h**, pula a pausa e recua **300min até 21h**. (`app/ponto/motor_calculo_v2.py:392`, `app/ponto/motor_calculo_v2.py:400`, `app/ponto/motor_calculo_v2.py:404`.)

**Na porta real**, o segundo recuo recebe pausa vazia: `07h−540min=22h`; a janela 22h–07h produz **540min = 9h de HE100 noturna**. Isso demonstra a divergência com as 8h do teste; ocorrência desse cadastro/batidas em produção **[nao verificado]**. (`app/ponto/calculador/alimentacao.py:96`, `app/ponto/calculador/alimentacao.py:514`; `app/ponto/motor_calculo_v2.py:405`, `app/ponto/motor_calculo_v2.py:441`.)

No atraso, `22h20−22h=20min`; saída no marco dá zero; teto `480−400=80min`, portanto preserva os vinte minutos: **atraso = 1/3h**. (`app/ponto/motor_calculo_v2.py:1961`, `app/ponto/motor_calculo_v2.py:1994`; `app/ponto/calculador/regras.py:264`, `app/ponto/calculador/regras.py:271`; anexo `teste_novo.py:106`.)

**A justificativa de tolerância precisa ser corrigida:** o comentário do teste fala em 5min do art.58, mas o código aplica **10min por marcação**, por mandato de negócio explicitamente documentado como dobro do legal. O caso de 20min passa nos dois critérios; não distingue 5 de 10. (`app/ponto/motor_calculo_v2.py:52`, `app/ponto/motor_calculo_v2.py:60`, `app/ponto/motor_calculo_v2.py:1994`; anexo `teste_novo.py:104`.)

**5. Outros retornos e testes antigos.** [nao verificado]

Há um irmão relevante no próprio `do_dia`: **folga que é feriado**, com `_hora_feriado=True`. Esse subramo escreve HE100 e HE100_feriado, mantém os splits noturnos declarados em `nao_decide` e retorna antes do split comum. O comentário “se o pai não viaja” não descreve esse subramo, pois ali o pai HE100 viaja. (`app/ponto/calculador/regras.py:680`, `app/ponto/calculador/regras.py:682`, `app/ponto/calculador/regras.py:700`, `app/ponto/calculador/regras.py:704`, `app/ponto/calculador/regras.py:735`.)

Nesse retorno, **noturno geral já é calculado**; pontualidade fica excluída expressamente por ser folga sem marco. Há ainda o retorno da folga sem os insumos exigidos. Portanto, não é uma repetição exata dos cinco zeros afirmados da dobra. (`app/ponto/calculador/regras.py:697`, `app/ponto/calculador/regras.py:712`, `app/ponto/calculador/regras.py:737`.)

Em `periodo.py`, o retorno cedo de `periodo_do_dia` atende entrada/saída ausente, declarando turno aberto; não é um ramo de dobra que interrompe rubricas. (`app/ponto/calculador/periodo.py:351`.)

**Não encontrei teste que exigisse os cinco zeros antigos.** O teste de dobra existente verifica HE100, HE50 e a flag; o teste de feriado/split usa par diurno e verifica as metades da HE100. Ausência absoluta de qualquer outro teste desse tipo: **[nao verificado]**. (`app/ponto/tests/test_s5b_regra_pontualidade.py:309`; `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:207`.)

Há, contudo, **registro explícito do defeito no teste P5**: ele explica que retirou o feriado da fixture porque o retorno antecipado zerava noturnas/split e estava fora daquele aval. O teste passou a usar dia comum e cobrar noturnas nele. Isso é contorno documentado da lacuna. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:25`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:66`, `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:81`.)

**6. Lei que exclua pontualidade no feriado trabalhado em dobra.**

**Não localizei essa exclusão escrita.** As leis aplicáveis encontradas são L-084, julgamento contra marcos válidos, e L-093, teto pelo tempo efetivamente faltante. As exclusões explícitas encontradas são por família sem marcos e por folga trabalhada; elas não autorizam excluir todo feriado em dobra. Inexistência absoluta em todo o acervo: **[nao verificado]**. (`app/docs/LEIS.md:116`, `app/docs/LEIS.md:111`; `app/ponto/calculador/regras.py:487`, `app/ponto/calculador/regras.py:189`.)

Os quatro ramos de dobra do motor alteram HE/alertas, sem estabelecer isenção de pontualidade; no comercial, o fluxo segue ao split e ao julgamento de pontualidade. (`app/ponto/motor_calculo_v2.py:2797`, `app/ponto/motor_calculo_v2.py:3080`, `app/ponto/motor_calculo_v2.py:3243`, `app/ponto/motor_calculo_v2.py:3325`, `app/ponto/motor_calculo_v2.py:3269`, `app/ponto/motor_calculo_v2.py:3281`.)

## 01/11 03:40 · sessao 4e5de46601be · - · - · origem=Ronald (tmux)

Achados para consolidar:

- **Números do teste novo conferem no helper isolado.** `noturnas_do_dia` soma janela, desconta intervalo e aplica fator importado: `app/ponto/calculador/regras.py:80-89`; fator `60/52,5`: `app/ponto/motor_calculo_v2.py:46`. Para 22–06: `480−60=420` trabalhados/noturnos de relógio; `420×60/52,5=480` minutos legais, **8 h**; HE100 = **7 h**, split100 noturno = **7 h**. Para 21–07: `600−60=540` trabalhados; noturno `540−60=480`; legal `548,571428…` minutos = **9,142857 h**; HE100 **9 h**, split100 noturno **8 h**. A janela prorrogada é implementada em `app/ponto/motor_calculo_v2.py:99-116`; o split não aplica fator reduzido, apenas rateia minutos noturnos: `app/ponto/motor_calculo_v2.py:440-451`.

- **O recuo precisa considerar trabalho.** No primeiro caso, `_inicio_do_trecho_extra` recua 180 minutos de 06 até 03, pula intervalo 02–03 e recua os 240 restantes até 22; no segundo, recua 240 de 07 até 03, pula intervalo e recua 300 até 21. Código: `app/ponto/motor_calculo_v2.py:392-406`.

- **Atraso de 20 minutos confere, mas a justificativa “tolerância de 5” não descreve o código vivo.** O motor usa **10 minutos por marcação**, com mandato de negócio explicitamente registrado como dobro dos limites legais 5/10: `app/ponto/motor_calculo_v2.py:52-61`; comparação efetiva `>10`: `app/ponto/motor_calculo_v2.py:1994-1997`. A docstring ainda diz cinco: `app/ponto/motor_calculo_v2.py:1940`. Neste caso, 20 ultrapassa ambos, conta integralmente, saída tem diferença zero e teto `480−400=80` não corta os vinte: `app/ponto/calculador/regras.py:264-275`; teto descrito em `app/ponto/motor_calculo_v2.py:1012-1037`. O teste novo não distingue tolerância 5 de 10 [nao verificado: anexo sem linhas locais].

- **Há irmão relevante no ramo de folga que vira HE100 de feriado.** `regras.py` escreve HE100 e HE100_feriado quando `_hora_feriado`: `app/ponto/calculador/regras.py:680-684`; mantém os splits noturnos em `nao_decide`, pois a exceção do laço protege apenas os dois pais: `app/ponto/calculador/regras.py:704-711`; retorna antes do split comum: `app/ponto/calculador/regras.py:735`, `app/ponto/calculador/regras.py:815`. O comentário “se o pai não viaja” contradiz esse subramo onde HE100 viaja: `app/ponto/calculador/regras.py:700-703`. Ali o noturno geral é calculado antes do retorno: `app/ponto/calculador/regras.py:712-718`; pontualidade é excluída expressamente por ser folga sem marco: `app/ponto/calculador/regras.py:697-699`. Outro retorno de folga sem insumos: `app/ponto/calculador/regras.py:736-737`. Os demais retornos examinados não reproduzem o formato de dobra interrompendo rubricas; `periodo_do_dia` retorna para par aberto: `app/ponto/calculador/periodo.py:351-352`.

- **Não achei teste que exija os cinco zeros antigos da dobra.** O teste existente `RegraDobraDeFeriadoTest.test_MORDE_no_do_dia_a_dobra_SUBSTITUI_a_HE50_e_nao_soma` só verifica HE100=10, HE50=0 e flag, sem pares nem marcos: `app/ponto/tests/test_s5b_regra_pontualidade.py:309-318`. `FeriadoEOutraMetadeDaHE100Test.test_MORDE_feriado_em_dobra_x_excedente_dao_METADES_DIFERENTES` usa par diurno 08–17 e verifica somente HE100/feriado: `app/ponto/tests/test_s5b_split_noturno_tem_um_dono.py:207-233`. Ausência global de qualquer outro assert: [nao verificado].

- **Um teste antigo já registra o defeito e o contorna explicitamente.** O P5 explica que removeu o feriado da fixture porque o retorno antecipado produzia zero afirmado e aquele arquivo estava fora do aval: `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:25-28`; usa calendário vazio: `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:61-66`; cobra noturnas=8 no dia comum: `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:81-83`. Portanto há lacuna/contorno documentado, não evidência de selo que mande preservar os zeros.

- **Mutantes que o teste novo deixa escapar:** mudar `det['extra_min']` para `_dobra` na escrita comum (`app/ponto/calculador/regras.py:808`); zerar sempre saída antecipada (`app/ponto/calculador/regras.py:851`), pois o único caso verifica saída no marco; corromper HE100_feriado apenas quando houver excedente, pois o caso longo não confere essa rubrica [nao verificado: anexo sem linhas locais]. O caso longo pega a remoção do `elif`, e o caso de atraso pega retorno após noturnas que continue pulando pontualidade [nao verificado: anexo sem linhas locais].

- **Limite de integração:** o teste fornece par inteiro e intervalo explícito; o caminho real entrega `pares=_pares_por_dia` e `intervalos=()` em `app/ponto/calculador/alimentacao.py:512-515`. Já existe comentário explicando que segmentos após 05 podem perder prorrogação por essa forma de alimentação: `app/ponto/calculador/periodo.py:501-505`. O teste prova sua chamada isolada; equivalência com uma lavratura real continua [nao verificado].

## 01/11 03:39 · sessao 2f1486725273 · - · - · origem=Ronald (tmux)

Checklist pela leitura do diff:

- **(a) Segundo juiz: não introduz.** Noturnas já usam funções do motor (`app/ponto/calculador/regras.py:33`, `:63`, `:85`); pontualidade empresta os métodos registrados de `MotorBase` (`:112`–`:115`; registro em `app/core/juizes.py:198`); dobra usa a autoridade existente (`app/core/juizes.py:277`).
- **(b) Segundo escritor fora da porta: não introduz no diff.** O patch altera o fluxo da mesma montagem de `Rubricas`, permitindo alcançar as atribuições existentes; o retorno continua em `app/ponto/calculador/regras.py:865`. Persistência e leitores foram delegados ao outro explorador. [nao verificado]
- **(c) Relógio fora da borda: não identificado.** `do_dia` recebe data, pares e parâmetros nos argumentos (`app/ponto/calculador/regras.py:572`). O teste anexo constrói datas fixas; `make_aware` não é leitura do instante atual. A lei está em `app/docs/CORTES.md:369`. Execução do teste [nao verificado].
- **(d) Derivação paralela: o diff não acrescenta conta.** Reaproveita os blocos existentes e as funções importadas (`app/ponto/calculador/regras.py:815`, `:835`, `:846`).
- **(e) RED morde, por inspeção.** O `return` antigo (`app/ponto/calculador/regras.py:789`) impede obter os 8 h noturnos, 7 h de HE100 noturna e 20 min de atraso exigidos pelo anexo. O caso com excedente de 60 min também detecta usar `if` em lugar do `elif`: escreveria HE50=1 (`:790`–`:791`). RED/GREEN efetivamente executados [nao verificado]. Há mutantes sobreviventes: escrever sempre zero em `horas_saida_antecipada` (`:851`), ou em `det['extra_min']` (`:808`); o teste novo não exige antecipada positiva nem afirma esse detalhe. [nao verificado: teste disponível apenas no anexo]
- **(f) Ordem de trava: o patch não introduz transação nem aquisição de lock; altera a continuidade entre atribuições locais** (`app/ponto/calculador/regras.py:779`, `:815`, `:846`, `:865`).
- **(g) Pouso antes do pré-requisito: não pode ser certificado.** Exige suite inteira verde, depois opus final, sem alteração posterior da árvore certificada (`CLAUDE.md:505`, `:557`–`:561`). Evidências desses atos não foram fornecidas [nao verificado]. [nao verificado]

Sobre pontualidade em feriado com dobra: **não localizei exclusão escrita por esse motivo** nas leis consultadas; a inexistência absoluta é [nao verificado]. As regras escritas aplicáveis são L-084 e L-093 (`app/docs/LEIS.md:116`, `:111`). As isenções explícitas são por classe sem marcos (`app/ponto/calculador/regras.py:487`–`:498`) e por **folga trabalhada** (`:189`–`:192`, `:697`–`:711`), que não equivale automaticamente a feriado trabalhado em dobra.

Os quatro ramos de dobra do motor escrevem HE/alertas, sem zerar pontualidade (`app/ponto/motor_calculo_v2.py:2797`, `:3080`, `:3243`, `:3325`); o comercial segue ao split e à pontualidade depois da decisão de dobra (`:3269`, `:3281`).

## 01/11 03:38 · sessao 062333bad220 · - · - · origem=Ronald (tmux)

Mapa de escritores e leitores, com linhas da base:

| Campo | Escritor após a cura | Leitores e persistência |
|---|---|---|
| `horas_noturnas` | `do_dia` chama `noturnas_do_dia` e escreve legais/60; grava `noturno_relogio_min` e `noturno_legais_min` (`app/ponto/calculador/regras.py:835-842`). Sem pares ou CCT completa, fica em `nao_decide` (`:609-611`). | `dia_pago.lavrar` substitui valor genericamente (`app/ponto/services/dia_pago.py:320-328`); DIFF compara valores e respeita cegueira (`app/ponto/management/commands/diff_calculador.py:501-535`); tela/papel leem `rubricas_do_dia` (`app/ponto/services/dia_pago.py:899-924`). |
| `horas_extras_50_noturna`, `horas_extras_100_noturna` | `do_dia` soma `he_noturna` por `he_pontas`, escreve rubricas e `he_pontas_periodos` (`app/ponto/calculador/regras.py:815-831`). Sem pontas/prorrogação ficam em `nao_decide` (`:627-632`). | Mesma substituição genérica na lavra (`app/ponto/services/dia_pago.py:320-328`) e comparação do DIFF (`app/ponto/management/commands/diff_calculador.py:501-535`). São campos de `CAMPOS_DIA`, somados na competência (`app/ponto/services/dia_pago.py:35-37`, `:548-561`). |
| `horas_atraso`, `horas_saida_antecipada` | `do_dia` chama `pontualidade_do_dia` e escreve minutos/60 (`app/ponto/calculador/regras.py:846-853`). Lei da família ou falta de insumos mantém `nao_decide` (`:618-625`); sem dia remove valores e declara (`:858-863`). | Mesma lavra e DIFF; tela/papel leem atraso/antecipada (`app/ponto/services/dia_pago.py:915-916`). O export soma as duas pela função `rubrica_atraso_parcial` (`app/folha/export.py:372-388`, `:440-441`). |
| `noturno_*`, `he_pontas_periodos`, `atraso_min`, `saida_antecipada_min`, `cadastro_x_realidade` em `Rubricas.detalhe` | `do_dia` escreve nos blocos agora alcançados (`app/ponto/calculador/regras.py:831`, `:841-842`, `:852-857`). | DIFF imprime **detalhe inteiro** para colaborador divergente (`app/ponto/management/commands/diff_calculador.py:543-548`). A lavra **não copia `_r.detalhe`**: copia valores e apenas nomes de `nao_decide` (`app/ponto/services/dia_pago.py:320-331`); o detalhe persistido vem de `L['_detalhe']` (`:339-345`). Portanto não ligar automaticamente esse novo `cadastro_x_realidade` à lista de produção. |

A alimentação real vem de `alimentacao.rubricas_por_dia`, que fornece pares, pontas de HE, flags de CCT e pontualidade (`app/ponto/calculador/alimentacao.py:501-519`). As pontas de HE carregam extras **do período do motor**, deliberadamente (`:48-54`, `:91-96`).

O fechamento principal continua escrito pelo resultado do motor: noturnas/splits em `app/ponto/services/fechamento.py:893-899`, atraso com crédito e antecipada em `:962-970`. A versão `motor` de DiaPago recebe os períodos em `:1168-1205`. O calculador só alimenta a versão `oraculo`, desde competência 10/2026, pela mesma função `lavrar` (`:1233-1264`). Não nasce escritor novo: a persistência comum continua no delete/bulk_create (`app/ponto/services/dia_pago.py:357-358`).

As testemunhas da porta continuam lendo `motor`: `soma_da_competencia` tem esse default (`app/ponto/services/dia_pago.py:548-561`); `porta_export` a chama sem versão (`app/folha/porta_export.py:388`), compara todos os campos somados com fechamento (`:451-467`), compara badge de noturnas/atraso (`:245-276`) e consulta linhas com versão literal `motor` (`:588-589`). A testemunha diária `dias_fora_do_dia_pago` mede **horas trabalhadas**, por `pago_do_dia`, sem comparação individual de noturno/splits/pontualidade (`:101-118`).

A lista de produção CADASTRO x REALIDADE é `resultado.dias_cadastro_x_realidade`, escrita pelo motor (`app/ponto/motor_calculo_v2.py:2707`), lida no espelho (`app/ponto/services/espelho.py:745-752`, `:1264`) e pela testemunha de janela de HE (`app/ponto/janela_he.py:245-254`, `:277-282`). Ela tem forma/proveniência diferente do novo `Rubricas.detalhe['cadastro_x_realidade']` (`app/ponto/calculador/regras.py:854-857`).

**Achado fora do diff que o teste novo não cobre:** a porta real entrega `intervalos=()` (`app/ponto/calculador/alimentacao.py:514`), embora `he_pontas` guarde o envelope efetivo do período (`:73`, `:91-96`). Apenas os pares comuns são segmentados descontando pausa (`:115-146`). O teste anexo passa envelope e intervalo explícito, contornando essa alimentação [nao verificado: arquivo anexo não materializado].

Consequência demonstrável para o caso 21–07/trabalho 540/pausa 02–03: pela porta real, `he_noturna` recebe x100=540 e nenhum intervalo, recua de 07 para 22 (`app/ponto/motor_calculo_v2.py:392-406`), conta 540 minutos noturnos e devolve **9 h**, enquanto com intervalo recua até 21, conta 540 noturnos e desconta 60, devolvendo **8 h** (`:440-450`; janela em `:99-118`). Isso é um problema preexistente de alimentação que a cura passa a alcançar no ramo dobra; ocorrência em produção permanece [nao verificado].

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

_1350 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
