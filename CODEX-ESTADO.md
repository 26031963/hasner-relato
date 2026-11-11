# CODEX-ESTADO

> Ultima resposta processada: **11/11/2026 06:46** · publicado em **11/11/2026 06:48** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 11/11 06:46 · sessao f28b56e2711d · TRAVA-SELO · verificador · origem=Code

**1. NÃO PROCEDE que o delta feche todas as classes da volta 1.** A seleção continua sendo interpretada de forma diferente pela guarda e pelo comando executado. (`bin/suite.sh:81–87`, `bin/suite.sh:157–158`)

| Achado PROCEDE da v1 | Situação na v2 |
|---|---|
| Inteira disfarçada de recorte; `ONLY` com dois significados | **Parcialmente corrigido.** Branco e lista integral literal são recusados; permanecem as fugas abaixo. (`bin/suite.sh:83–98`) |
| Comando canônico recusado, selo antigo sem acusar | **Não corrigido.** A documentação mantém a chamada nua; o selo continua verificando strings e aprovando a fixture nua. (`CLAUDE.md:96`; `bin/tests/test_suite_sh.sh:31–32,50–56,72–81`) |
| Selo novo exercita arquivo real, mas tem cobertura insuficiente | **Parcialmente corrigido.** Há novos negativos e quatro mutantes; continuam faltando classes de seleção. (`bin/tests/test_suite_motivo.sh:31–43,47–66`) |
| Descoberta incompleta de vizinhos | **Não fecha a classe**, conforme pergunta 3. (`bin/vizinhos_da_volta.py:49–71`) |
| Rc 75 prevalecendo sobre vermelho | **Corrigido na composição dos rc**, conforme pergunta 4. (`bin/volta_intermediaria.sh:37–44`) |
| Identificação de escritores/leitores | Continua identificável; a mudança de ordem tem efeito na precedência dos erros, conforme pergunta 5. (`bin/suite.sh:66–67,86,157`) |

**PROCEDE: há fugas na guarda nova.** (`bin/suite.sh:81–87`)

| Caso solicitado | Resultado da análise |
|---|---|
| Labels pontuadas cuja união cobre todos os apps | **Escapa.** A guarda exige cada token literal de `LABELS`; não reconhece cobertura por `ponto.tests` ou pela união de módulos. A equivalência concreta desses módulos à inteira nesta obra é **[nao verificado]**. (`bin/suite.sh:86–87`) |
| Uma label repetida | **Não cria fuga por si.** Todas as labels mais uma duplicata continuam recusadas; repetir apenas uma label não cobre as demais. O algoritmo verifica pertinência, não quantidade. (`bin/suite.sh:86–89`) |
| `-- --tag x`, `-- -k padrao`, `-- --exclude-tag x` | **Escapa da autorização.** Descarta a flag, mas incorpora seu valor como suposta label. Com `ONLY` vazio, libera e depois encaminha **todas as `LABELS`** ao executor. Quantos testes os filtros efetivamente selecionariam: **[nao verificado]**. (`bin/suite.sh:84–87,157–158`) |
| `-- --exclude-tag`, sem valor | Não apresenta essa fuga: não sobra seleção e a guarda recusa sem motivo. Aceitação pelo executor com motivo válido: **[nao verificado]**. (`bin/suite.sh:84–85,91–98`) |
| `--volta` cujo helper devolve todas as `LABELS` | **Escapa.** `VOLTA` dispensa a guarda antes de executar o helper; não há nova avaliação depois de preencher `ONLY`. (`bin/suite.sh:81,103–108`) |
| Espaço duplo em `LABELS` ou entre labels de `ONLY` | **Não cria fuga por si.** O `for` e o `read -a` separam essas palavras; a comparação continua por label. (`bin/suite.sh:83,86–87`) |
| `--only ""` | Continua abortando no parser, antes da guarda; não recebeu tratamento próprio como seleção vazia. (`bin/suite.sh:51`) |

Há outra divergência: `read` considera somente a primeira linha de `ONLY`, enquanto a expansão final separa também as seguintes. Com a fixture, `ONLY=$'appA\nappB'` libera na guarda e encaminha ambos. (`bin/suite.sh:83,157`; `bin/tests/test_suite_motivo.sh:18`)

**2. PROCEDE: os quatro mutantes declarados morrem pela razão pretendida; NÃO PROCEDE cobertura completa.** O juiz distingue recusa, chegada à trava e custo antes da recusa. (`bin/tests/test_suite_motivo.sh:17–27,31–43`)

| Mutante | Razão da morte |
|---|---|
| `aceita_sem_motivo` | Negativos deixam de retornar `2 nao`. (`bin/tests/test_suite_motivo.sh:31–39,60`) |
| `selecao_pela_presenca_do_flag` | Libera branco, todas as labels e todas após `--`. (`bin/tests/test_suite_motivo.sh:37–39,61`) |
| `regex_frouxa` | Aceita espaço extra, `;` e quebra de linha nos motivos negativos. (`bin/tests/test_suite_motivo.sh:34–36,62`) |
| `guarda_depois_do_custo` | Insere `teste_envfile` na condição: a recusa continua rc 2, mas vira `2 custo`, contrariando `2 nao`. (`bin/tests/test_suite_motivo.sh:17,26,64–66`) |

**Há mutante plausível sobrevivente:** substituir a pertinência de cada app pela igualdade entre quantidade de labels selecionadas e quantidade de apps. Preservando o caso vazio, passa os 13 casos atuais; com a fixture, `--only 'appB appA appA'` libera a inteira porque três palavras diferem dos dois apps. O vivo recusa essa entrada. (`bin/suite.sh:85–89`; `bin/tests/test_suite_motivo.sh:18,31–43`)

Esses comportamentos foram confrontados em memória. Execução integral do selo: **[nao verificado]**; o próprio selo cria, copia e remove arquivos temporários. (`bin/tests/test_suite_motivo.sh:10–24`)

**3. PROCEDE: há falso-negativo novo no filtro relativo; NÃO PROCEDE atribuir uma regressão ao glob isoladamente.** (`bin/vizinhos_da_volta.py:32–33,49–57,68–69`)

Contraexemplo de classe: toque em `pkg/tests/sub/__init__.py`; teste consumidor em `pkg/tests/test_x.py` contendo `from .sub import algo`. A normalização produz `pkg.tests.sub`, a regex relativa casa, mas o filtro descarta porque `pkg/tests` não começa por `pkg/tests/sub`. No delta v1 anexado, o padrão relativo desse caso não tinha esse filtro. Ocorrência concreta no diff da obra: **[nao verificado]**. (`bin/vizinhos_da_volta.py:49–57,68–69`)

O prefixo textual tampouco resolve níveis de importação: não interpreta quantos pontos sobem no pacote nem verifica fronteiras entre componentes de pasta. (`bin/vizinhos_da_volta.py:56–57,68`)

| Classe da v1 | Fechamento |
|---|---|
| Relativos/reexportações | **Não fecha:** permanece correspondência textual direta, sem fechamento transitivo. (`bin/vizinhos_da_volta.py:49–71`) |
| `__init__.py` | Normaliza o módulo, mas sofre a regressão acima e não propaga automaticamente para todos os consumidores. (`bin/vizinhos_da_volta.py:49,57,68–71`) |
| Imports dinâmicos | **Não fecha.** Existe teste que monta o destino com f-string; o helper procura formas textuais fixas. (`app/ponto/tests/test_detectores_isencao.py:7–18`; `bin/vizinhos_da_volta.py:51–56`) |
| JSON/templates | Deixaram de ser descartados pela extensão, mas entram por caminho/basename; herança e dependência transitiva continuam sem resolução. (`bin/vizinhos_da_volta.py:58–71`) |
| Migration | Puxa o app tocado; não acrescenta regra de dependência de esquema entre apps. (`bin/vizinhos_da_volta.py:46–47,65–71`) |
| Selos que varrem diretório/AST | Continua lista fixa. Há varredor de templates fora dela; seleção por um diff concreto: **[nao verificado]**. (`bin/vizinhos_da_volta.py:72–73`; `bin/selos_globais_da_volta.txt:1–24`; `app/core/tests/test_contract_template_bem_formado.py:40,76–86`) |
| Remoções | Agora preserva o caminho e procura citações, mas não fecha dependências transitivas. (`bin/vizinhos_da_volta.py:19–21,44–71`) |

O glob novo amplia a profundidade, mas mantém a lacuna antiga fora da pasta `tests`: por exemplo, `app/comunicados/tests.py` contém `TestCase`, importa produção e fica fora dos candidatos e da obra direta. (`bin/vizinhos_da_volta.py:24–25,32–33`; `app/comunicados/tests.py:4–10`)

**NÃO SEI** se a descoberta coincide exatamente com o executor: o runner da cópia delega ao Django; essa implementação não foi fornecida. (`app/core/runner_quarentena.py:72–76`; comportamento completo: **[nao verificado]**)

**4. PROCEDE: a tabela fecha os casos pedidos quando os comandos retornam ao wrapper.** (`bin/volta_intermediaria.sh:37–44`)

| Ruff | Disjuntor | Testes | Marcador próprio | Rc final | [nao verificado]
|---|---|---|---|---|
| 0 | 0 | 0 | nenhum | 0 |
| vermelho | qualquer | 75 | `VOLTA_VERMELHA` | 1 |
| 0 | 0 | 75 | `VOLTA_SEM_VEZ` | 75 |
| 0 | 0 | 1 | `VOLTA_VERMELHA` | 1 |
| 0 | 0 | outro não zero, exceto 75 | `VOLTA_VERMELHA` | 1 |

As linhas da tabela decorrem do `if/elif` e da consolidação subsequente. (`bin/volta_intermediaria.sh:37–44`)

Timeout **da espera pela trava** retorna 75; vermelho anterior vence esse resultado. Um timeout externo que encerre o wrapper antes da consolidação não tem conclusão garantida por esse fluxo: **[nao verificado]**. (`bin/trava_teste.sh:36–38`; `bin/volta_intermediaria.sh:32–44`)

**5. NÃO PROCEDE dependência demonstrada da ordem antiga; PROCEDE mudança na precedência de falhas.** (`bin/suite.sh:66–67,91–114`)

| Quem escreve/lê | Papel |
|---|---|
| `bin/regua.sh:23` | Declara a lista literal. |
| `bin/suite.sh:66` | Lê a régua e escreve sua variável `LABELS`. |
| `bin/suite.sh:86` | A guarda lê `LABELS`. |
| `bin/suite.sh:157` | O comando usa `LABELS` quando `ONLY` está vazio. |
| `bin/suite.sh:113` | Imprime o texto literal `<LABELS da regua>`; não expande a variável. |
| `bin/tests/test_suite_motivo.sh:18` | Escreve a lista falsa da fixture. |
| `bin/tests/test_labels_fonte_unica.sh:20–34,103–105` | Lê a fonte e julga cobertura. |
| `bin/tests/test_suite_sh.sh:50–56` | Inspeciona textualmente a presença da leitura, sem exigir posição. |

Outras portas fazem sua própria leitura da régua: pre-push, hook versionado, relógio, calendário, vigia e autocura. Essas leituras não dependem da posição da atribuição dentro de `suite.sh`. (`bin/pre-push.sh:184–187`; `app/core/hooks/pre-push.sh:80–83`; `bin/regua_relogio.sh:27–29`; `bin/regua_calendario.sh:22,45`; `bin/vigia_arvore.sh:75,88`; `bin/molde_autocura/rodar.sh:25–26`)

Agora uma falha ao obter `LABELS` encerra com rc 1 **antes** da recusa por motivo e antes de `--seco`, inclusive para chamadas com `--volta`. O comentário que promete recusa antes do custo de ler labels ficou incompatível com a ordem atual. (`bin/suite.sh:66–67,73,91–114`)

Mensagem do commit, motivos declarados de descarte e resultados reais em produção: **[nao verificado]**. [nao verificado]

## 11/11 06:45 · sessao be835ce87e0b · - · - · origem=Ronald (tmux)

Confirmado: o mutante que troca a pertinência de cada app por **igualdade da quantidade de labels** sobrevive aos 13 casos atuais. (`bin/suite.sh:85–89`; `bin/tests/test_suite_motivo.sh:31–43`)

Com a fixture `LABELS="appA appB"`, `--only 'appB appA appA'` recebe **rc 2 no vivo e rc 0 no mutante**: a duplicata permite liberar a seleção integral sem motivo. Verificado por simulação em memória, sem arquivos. (`bin/tests/test_suite_motivo.sh:18`; `bin/suite.sh:85–98`)

## 11/11 06:45 · sessao 011136d78e46 · - · - · origem=Ronald (tmux)

**3. PROCEDE: a classe não fecha, e há falso-negativo novo plausível no filtro relativo.** O glob amplia profundidade, mas o filtro por prefixo de pasta perde consumidores de inicializadores de pacote. (`bin/vizinhos_da_volta.py:32`, `bin/vizinhos_da_volta.py:49`, `bin/vizinhos_da_volta.py:57`, `bin/vizinhos_da_volta.py:68`)

- **Regressão nova demonstrável:** tocado `pkg/tests/sub/__init__.py`; consumidor `pkg/tests/test_x.py` contém `from .sub import algo`. A normalização produz módulo `pkg.tests.sub`, a regex relativa casa, porém o filtro exige que `pkg/tests` comece por `pkg/tests/sub` e descarta o consumidor. Na v1 anexada, `pkg` era `pkg.tests`, satisfazia `pkg.endswith('tests')` e a regex relativa casava sem esse filtro. Simulação em memória confirmou `old_match=True`, `new_regex_match=True`, `new_package_filter_passes=False`; ocorrência concreta na obra: **[nao verificado]**. (`bin/vizinhos_da_volta.py:49`, `bin/vizinhos_da_volta.py:50`, `bin/vizinhos_da_volta.py:56`, `bin/vizinhos_da_volta.py:57`, `bin/vizinhos_da_volta.py:68`)
- **Glob:** `tests/**/test*.py` amplia o antigo `tests/*.py`; não demonstrou regressão por si. Permanece a lacuna antiga de testes fora da pasta `tests`: existem `TestCase` em `app/comunicados/tests.py` e testes em `app/ferias/test_cadastro_idempotente.py`, excluídos tanto da obra quanto dos candidatos. O runner delega a descoberta ao Django; a implementação exata do executor não está fornecida: **[nao verificado]**. (`bin/vizinhos_da_volta.py:24`, `bin/vizinhos_da_volta.py:32`; `app/comunicados/tests.py:4`, `app/comunicados/tests.py:10`; `app/ferias/test_cadastro_idempotente.py:2`, `app/ferias/test_cadastro_idempotente.py:6`; `app/ferias/tests.py:19`; `app/core/runner_quarentena.py:72`, `app/core/runner_quarentena.py:76`)

| Classe da v1 | Fechamento na v2 |
|---|---|
| Relativo/reexportação | **Não fecha:** referências textuais diretas; sem resolução dos níveis nem fechamento transitivo. (`bin/vizinhos_da_volta.py:49`, `bin/vizinhos_da_volta.py:56`, `bin/vizinhos_da_volta.py:65`) |
| `__init__.py` | Normaliza o nome, mas introduz a regressão de prefixo descrita acima; trânsito continua ausente. (`bin/vizinhos_da_volta.py:49`, `bin/vizinhos_da_volta.py:57`, `bin/vizinhos_da_volta.py:68`) |
| `importlib`/destino dinâmico | Nome literal pode casar; montagem dinâmica continua sem resolução. Existe teste que monta o destino com f-string. (`bin/vizinhos_da_volta.py:51`; `app/ponto/tests/test_detectores_isencao.py:7`, `app/ponto/tests/test_detectores_isencao.py:8`, `app/ponto/tests/test_detectores_isencao.py:12`) |
| JSON/template | Agora participa por caminho/basename; dependência transitiva, herança e destino montado não receberam resolução. (`bin/vizinhos_da_volta.py:58`, `bin/vizinhos_da_volta.py:63`, `bin/vizinhos_da_volta.py:65`) |
| Migration | Puxa o app tocado; não há regra nova para apps dependentes do esquema sem referência textual. (`bin/vizinhos_da_volta.py:46`, `bin/vizinhos_da_volta.py:47`, `bin/vizinhos_da_volta.py:65`) |
| Selo que varre diretório/AST | Continua lista fixa, sem declaração de escopo. Existe varredor de todos os templates fora dela; exclusão por um diff concreto: **[nao verificado]**. (`bin/vizinhos_da_volta.py:72`; `bin/selos_globais_da_volta.txt:1`; `bin/selos_globais_da_volta.txt:24`; `app/core/tests/test_contract_template_bem_formado.py:40`, `app/core/tests/test_contract_template_bem_formado.py:76`, `app/core/tests/test_contract_template_bem_formado.py:78`) |
| Remoções | Conserva agora o caminho removido e procura citações; não inclui como obra módulo inexistente nem fecha trânsito. (`bin/vizinhos_da_volta.py:19`, `bin/vizinhos_da_volta.py:21`, `bin/vizinhos_da_volta.py:44`, `bin/vizinhos_da_volta.py:48`, `bin/vizinhos_da_volta.py:65`) |

## 11/11 06:44 · sessao 90a0e384d5c4 · - · - · origem=Ronald (tmux)

**4. PROCEDE — a composição fecha os casos pedidos no fluxo normal.** (`bin/volta_intermediaria.sh:16,37–44`)

| ruff | disjuntor | testes | marcador próprio | rc final | [nao verificado]
|---|---|---|---|---|
| 0 | 0 | 0 | nenhum | 0 |
| vermelho | qualquer | 75 | `VOLTA_VERMELHA` | 1 |
| 0 | 0 | 75 | `VOLTA_SEM_VEZ` | 75 |
| 0 | 0 | 1 | `VOLTA_VERMELHA` | 1 |

A condição `if/elif` torna os marcadores próprios exclusivos; vermelho anterior vence testes 75. (`bin/volta_intermediaria.sh:37–44`)

O timeout **da espera pela trava** retorna 75; a suite repassa `--espera` e executa a trava. (`bin/trava_teste.sh:36–38`; `bin/suite.sh:53,151`; `bin/volta_intermediaria.sh:32`) Timeout externo que mate o wrapper antes da consolidação: **[nao verificado]**. Execução real desses casos: **[nao verificado]**.

**5. NÃO PROCEDE dependência demonstrada da ordem antiga; PROCEDE mudança na precedência dos erros.** (`bin/suite.sh:66–67,86,91–114,157`)

| Papel | Arquivo:linha |
|---|---|
| Declaração literal da lista | `bin/regua.sh:23` |
| Suite escreve sua variável `LABELS`, lendo a fonte | `bin/suite.sh:66` |
| Guarda lê a variável | `bin/suite.sh:86` |
| Comando lê como fallback de `ONLY` | `bin/suite.sh:157` |
| Saída seca imprime apenas texto literal `<LABELS da regua>`, sem ler `$LABELS` | `bin/suite.sh:113` |

As demais portas leem diretamente a régua e escrevem sua própria variável: pre-push (`bin/pre-push.sh:184–187`), hook versionado (`app/core/hooks/pre-push.sh:80–83`), relógio (`bin/regua_relogio.sh:27–29`), calendário (`bin/regua_calendario.sh:22,45`), vigia (`bin/vigia_arvore.sh:75,88`) e molde de autocura (`bin/molde_autocura/rodar.sh:25–26`). Essas leituras não dependem da posição da atribuição dentro da suite. (`bin/suite.sh:66`; referências anteriores)

O selo textual exige a presença da leitura, sem verificar sua ordem. (`bin/tests/test_suite_sh.sh:50–56`)

Agora falha na leitura de LABELS produz rc 1 **antes** da recusa e de `--seco`; o comentário “ANTES de qualquer custo (labels...)” contradiz a ordem atual. (`bin/suite.sh:66–67,73,91–114`)

**Pendência da volta 1 permanece: comando canônico e selo antigo.** A documentação ainda manda executar a suite nua, recusada pela guarda; o selo continua aprovando apenas a referência textual à porta, inclusive na fixture “boa” nua. (`CLAUDE.md:96`; `bin/suite.sh:85,91–98`; `bin/tests/test_suite_sh.sh:31–32,72–81`)

## 11/11 06:44 · sessao be835ce87e0b · - · - · origem=Ronald (tmux)

**2. PROCEDE quanto aos quatro mutantes declarados; NÃO PROCEDE como cobertura completa da guarda.** O juiz exige recusa `2 nao`, chegada `0 sim` e sinaliza custo antes da recusa como `2 custo`. (`bin/tests/test_suite_motivo.sh:26–43`)

| Mutante | Razão da morte |
|---|---|
| `aceita_sem_motivo` | Desliga a guarda; negativos deixam de retornar 2. (`bin/tests/test_suite_motivo.sh:31–39,60`) |
| `selecao_pela_presenca_do_flag` | Libera branco, todas as labels e todas após `--`; esses casos agora são negativos explícitos. (`bin/tests/test_suite_motivo.sh:37–39,61`) |
| `regex_frouxa` | Aceita obra com espaço extra, `;` ou quebra de linha, contrariando os três negativos novos. (`bin/tests/test_suite_motivo.sh:34–36,62`) |
| `guarda_depois_do_custo` | Executa `teste_envfile` antes de decidir; a recusa continua rc 2, mas recebe `2 custo`. (`bin/tests/test_suite_motivo.sh:17,26,64–66`) |

**Há mutante plausível sobrevivente:** substituir a verificação de pertinência de cada app por comparação da quantidade de palavras selecionadas com a quantidade de apps. Os casos atuais distinguem apenas zero, uma e duas labels; com a fixture `appA appB`, esse mutante passa todos, mas recusaria incorretamente `--only 'appA appA'`. (`bin/suite.sh:85–89`; `bin/tests/test_suite_motivo.sh:18,31–43`)

As variantes foram verificadas com `bash -n` por stdin e simulação em memória do parser/guarda real; as quatro são sintaticamente válidas e apresentam as falhas acima. Execução integral do selo: **[nao verificado]**, pois ele cria e remove arquivos temporários. (`bin/tests/test_suite_motivo.sh:10–24`)

## 11/11 06:40 · sessao bc1418e2b9d3 · TRAVA-SELO · verificador · origem=Code

**1. PROCEDE — há caminho para encaminhar todas as LABELS sem motivo válido.** A guarda verifica apenas se `ONLY`/`VOLTA` estão vazios; não verifica o alcance efetivo da seleção. Basta fornecer todas as labels em `--only`. (`bin/suite.sh:70`, `bin/suite.sh:142`; lista em `bin/regua.sh:23`)

| Entrada | Resultado |
|---|---|
| Sem argumentos | Recusa rc 2, oferecendo `--only`/`--volta`. (`bin/suite.sh:70–77`) |
| `--only ""` | Aborta na expansão `${2:?…}`, antes da guarda; execução direta retornou **rc 1**, não 2. (`bin/suite.sh:51`) |
| `--only " "` | Passa pela guarda; a expansão sem aspas desaparece e encaminha `manage.py test` **sem labels posicionais**. A equivalência exata dessa descoberta com todas as LABELS é **[nao verificado]**. (`bin/suite.sh:70,142`; `app/core/runner_quarentena.py:72–76`) |
| `--only` contendo todas as LABELS | Encaminha todas sem motivo. É a fuga suficiente para responder “sim”. (`bin/suite.sh:51,70,142`; `bin/regua.sh:23`) |
| `--volta` com diff vazio válido | Produz os 24 selos globais; não cai na suite inteira nem na recusa “diff vazio”. (`bin/vizinhos_da_volta.py:17,36–39`; `bin/selos_globais_da_volta.txt:1–24`; `bin/suite.sh:85–87`) |
| `-- <labels>` sem recorte/motivo | Recusa: `EXTRA` não participa da guarda. (`bin/suite.sh:57,70`) |
| `--only " " -- <todas as labels>` | Passa; todas as labels chegam pelo `EXTRA`, como único bloco posicional após as opções. (`bin/suite.sh:57,70,142–143`) |
| `--only X -- <outras labels>` | A guarda libera, mas a cópia registra recusa do argparse por duas levas posicionais. Não contar esse formato como suite executada. (`bin/suite.sh:142–143`; `app/docs/RELATO.md:1363`) |
| `--seco` | Sem motivo/recorte recebe rc 2; com entrada autorizada imprime e sai 0 antes de executar testes. (`bin/suite.sh:70–93`) |
| Ordem e repetição | Antes de `--`, prevalece a última atribuição; depois dele, os argumentos ficam em `EXTRA`. `--only` e `--volta` juntos são recusados, independentemente da ordem. (`bin/suite.sh:48–61,83`) |
| Motivo com quebra de linha | Na inteira nominal, não casa com a regex ancorada e é recusado; um `ONLY` não vazio continua dispensando essa validação. (`bin/suite.sh:69–70`) |

**2. PROCEDE — o comando canônico quebra, e o selo antigo não acusa.** (`CLAUDE.md:96`; `bin/tests/test_suite_sh.sh:31–58`)

| Chamador/instrução | Passa a ser recusado? |
|---|---|
| `CLAUDE.md:96`: chamada nua | **Sim, rc 2**, antes de env-file, montagem e trava. (`bin/suite.sh:70–77,104,117,136`) |
| `bin/volta_intermediaria.sh:32` | **Não pela guarda de motivo**: fornece `--volta`; ainda pode falhar na seleção/montagem/trava. (`bin/suite.sh:70,82–87,113–118,136`) |
| `bin/tests/test_suite_motivo.sh:24` | As chamadas negativas são recusadas deliberadamente; positivas e `--only` devem alcançar a trava falsa. (`bin/tests/test_suite_motivo.sh:29–34`) |
| Orientação de recorte em `app/core/placar_estrutural.py:109–110` | `--only` é admitido pela guarda; trata-se de texto publicado, não execução Python da suite. (`bin/suite.sh:70`) |
| Receitas com apenas `--dir` | **Sim**, se coladas sem motivo/recorte: aparecem em `app/docs/RELATO-ARQUIVO.md:37800`, `app/docs/RELATO.md:681,1137,1145` e na orientação de `app/docs/PROMPTS.md:1069`/`app/docs/BACKLOG.md:1433`. (`bin/suite.sh:50,70–77`) |

`regua.sh`, `pre-push.sh` e o hook versionado executam testes diretamente; esta guarda não os alcança. (`bin/regua.sh:165–169`; `bin/pre-push.sh:186–188`; `app/core/hooks/pre-push.sh:82–84`)

Outros chamadores em `.claude/` ou hooks não foram localizados na busca textual; ausência de chamadas construídas dinamicamente: **[nao verificado]**.

O `test_suite_sh.sh` permanece **logicamente verde**: procura strings, não executa o comando canônico. Sua fixture “boa” é justamente a chamada nua agora recusada. Verde efetivamente executado nesta revisão: **[nao verificado]**. (`bin/tests/test_suite_sh.sh:31–58,72–88`)

**3. PROCEDE quanto ao arquivo real e ao mutante; NÃO PROCEDE como prova completa da guarda.** O teste copia o `suite.sh` real e o executa com recursos, labels e trava falsos. Não replica a implementação da guarda, mas também não exercita a integração real. (`bin/tests/test_suite_motivo.sh:14–24,38–39`)

O `sed` corresponde à guarda atual e troca sua condição por `if false; then`; desliga efetivamente a recusa. O selo também acusa mutação que não se aplicou. (`bin/suite.sh:70`; `bin/tests/test_suite_motivo.sh:44–48`)

| Mutante plausível | Cobertura |
|---|---|
| Guarda desligada | **Morre**: as três entradas negativas chegam à trava falsa. (`bin/tests/test_suite_motivo.sh:29–31,44–48`) |
| Regex frouxa, como aceitar qualquer texto após `pre-opus ` | **Pode sobreviver**: faltam negativos com espaços, caracteres indevidos e quebra de linha. (`bin/tests/test_suite_motivo.sh:29–34`) |
| Guarda depois da preparação que pode chamar Docker | **Pode sobreviver**: `teste_envfile` falso é inócuo, e o juiz observa somente rc e chegada à trava. O `teste_envfile` real pode executar Docker. (`bin/tests/test_suite_motivo.sh:17,22–34`; `bin/recursos.sh:122`) |
| Guarda depois do `exec` final | **Morre**: seria inalcançável e a chamada nua chegaria à trava falsa. (`bin/suite.sh:136`; `bin/tests/test_suite_motivo.sh:29`) |
| Parser aceitando `--only ""` como seleção em branco | **Pode sobreviver**: nenhum caso testa vazio ou whitespace. (`bin/tests/test_suite_motivo.sh:29–34`) |

As seis expectativas atuais também foram confrontadas em memória: sobreviveram variantes com regex frouxa, guarda após `teste_envfile` e vazio convertido em espaço; isso confirma a limitação dessas expectativas, sem executar o selo original. (`bin/tests/test_suite_motivo.sh:29–34`; pontos alterados: `bin/suite.sh:51,69–78,104`)

**Sim, está integrado à pasta de selos da régua**, pelo glob `tests/test_*.sh`; usa subprocessos e arquivos temporários, com a trava falsa encerrando antes do Docker. Não exige banco/Docker para seus casos atuais. (`bin/regua.sh:138–144`; `bin/tests/test_suite_motivo.sh:10–24`)

**4. PROCEDE — existem lacunas de classe na descoberta.** O helper usa referências textuais diretas, não AST nem fechamento transitivo; considera apenas `.py` existentes. (`bin/vizinhos_da_volta.py:10–12,26–35`)

| Classe | Limitação | Regra mínima sugerida |
|---|---|---|
| Import relativo/reexportação | Não resolve pacote, nível ou trânsito; o padrão relativo só aparece sob condição especial. (`bin/vizinhos_da_volta.py:25–35`) | Resolver imports no contexto do pacote e fechar dependências transitivas. |
| `__init__.py` | Há normalização parcial do inicializador de produção; inicializador de testes não entra como obra nem produção. (`bin/vizinhos_da_volta.py:13,19–24`) | Propagar alterações do inicializador aos consumidores e ao pacote de testes correspondente. |
| `importlib`/string | Nome completo literal pode casar; destinos montados dinamicamente não são resolvidos. (`bin/vizinhos_da_volta.py:26–35`) | Resolver destinos estáticos e ampliar conservadoramente quando indeterminados. |
| Fixture JSON | Alterações não-Python são descartadas. (`bin/vizinhos_da_volta.py:12`) | Incluir dependências de recursos e seus consumidores. |
| Template | Também descartado pelo filtro de extensão. (`bin/vizinhos_da_volta.py:12`) | Incluir consumidores, herança e inclusões de templates. |
| Migration | Pode entrar como `.py`, mas não há propagação por dependência de esquema. (`bin/vizinhos_da_volta.py:12,21–35`) | Selecionar testes do app e de seus dependentes de esquema. |
| Selo que varre diretório/AST | Não relaciona escopo varrido com diff; a lista fixa cobre somente os selos nela declarados. (`bin/vizinhos_da_volta.py:31–37`) | Declarar escopos de varredura e selecionar por interseção com arquivos tocados. |

Há ainda descoberta incompleta de testes e exclusão de arquivos removidos: candidatos exigem `tests/*.py` com nome começando por `test`, e tocados precisam continuar existindo. A regra mínima é acompanhar a descoberta do executor e preservar dependências de remoções. (`bin/vizinhos_da_volta.py:12,15–16`)

Para **cada classe**, o risco é excluir um teste dependente e obter volta verde com vermelho fora do recorte; ocorrência concreta nesta obra: **[nao verificado]**. [nao verificado]

**5. NÃO PROCEDE que o rc final distinga os estados sem ambiguidade.** Existem 0, 1 e 75, mas **75 prevalece mesmo quando ruff ou disjuntor já estão vermelhos**. Além disso, timeout escreve tanto `VOLTA_VERMELHA` quanto `VOLTA_SEM_VEZ`. (`bin/volta_intermediaria.sh:35–39`)

- **`TESTE_DOCKER`: correto** na composição: vem de `recursos.sh` e sua expansão fornece as flags ao Docker. (`bin/volta_intermediaria.sh:15,23`; `bin/recursos.sh:81–82`)
- **`PIPESTATUS[0]`: correto** para capturar Docker/ruff imediatamente após o pipeline; falha exclusiva do `tail` não entra nesse valor. (`bin/volta_intermediaria.sh:24–25`)
- **`VOLTA_ESPERA`: chega**, convertido em `--espera`, armazenado e repassado ao `flock -w`. (`bin/volta_intermediaria.sh:32`; `bin/suite.sh:53,136`; `bin/trava_teste.sh:27,36`)
- **`VOLTA_PORQUE`: chega**, pelo ambiente, e determina o destino do stderr do helper. (`bin/volta_intermediaria.sh:32`; `bin/suite.sh:85`)

**6. PROCEDE — escritores e leitores identificados abaixo.** (`bin/suite.sh:47–56,70–93`; `bin/vizinhos_da_volta.py:36–42`)

| Valor | Quem escreve | Quem lê |
|---|---|---|
| `MOTIVO` | Inicialização e parser `--motivo`; sobrescreve eventual ambiente homônimo. (`bin/suite.sh:47,54`) | Guarda, recusa e saída seca. (`bin/suite.sh:70,72,91`) |
| `VOLTA` | Inicialização e parser; wrapper fornece `BASE`. (`bin/suite.sh:47,55`; `bin/volta_intermediaria.sh:13,32`) | Guarda, ativação do helper, argumento/erro e saída seca. (`bin/suite.sh:70,82,85–86,91`) |
| `SECO` | Inicialização 0 e parser escreve 1. (`bin/suite.sh:47,56`) | Condição que imprime e encerra. (`bin/suite.sh:90–93`) |
| `VOLTA_PORQUE` | Wrapper fornece o caminho no ambiente. (`bin/volta_intermediaria.sh:32`) | Suite lê o caminho e abre/trunca o destino; helper escreve justificativas no stderr; wrapper lê o arquivo para contar labels. (`bin/suite.sh:85`; `bin/vizinhos_da_volta.py:40–42`; `bin/volta_intermediaria.sh:34`) |
| `selos_globais_da_volta.txt` | Lista literal versionada; escritor automático não identificado: **[nao verificado]**. (`bin/selos_globais_da_volta.txt:1–24`) | Helper lê o arquivo **ao lado de si próprio**, não da cópia-alvo, e acrescenta suas entradas. (`bin/vizinhos_da_volta.py:36–37`) |

Sem `VOLTA_PORQUE`, a justificativa vai para `/dev/null`. O motivo aparece na recusa/saída seca; registro dele no RELATO: **[nao verificado]**. (`bin/suite.sh:72,85,91`)

**7. Checklist — há reprovações de cobertura e representação de estado; as demais classes não foram demonstradas no diff.** (`bin/suite.sh:70,142`; `bin/tests/test_suite_motivo.sh:29–34`; `bin/volta_intermediaria.sh:35–39`)

| Classe | Parecer |
|---|---|
| Segundo juiz duplicando registrado | **NÃO PROCEDE no código alterado examinado**: os novos julgamentos são de autorização/seleção de testes e consolidação do instrumento; duplicação de juiz de domínio não demonstrada. (`bin/suite.sh:69–87`; `bin/vizinhos_da_volta.py:18–37`; `bin/volta_intermediaria.sh:35–39`) |
| Segundo escritor fora da porta | **NÃO PROCEDE no fluxo examinado**: a suite mantém montagem delegada e execução pela trava; os novos destinos são saídas do instrumento. (`bin/suite.sh:117,136`; `bin/volta_intermediaria.sh:29,32,37–41`) |
| Relógio fora da borda | **NÃO PROCEDE como regra de domínio**: os novos `date` medem/publicam duração no wrapper de host. Estabilidade perante ajuste do relógio civil: **[nao verificado]**. (`bin/volta_intermediaria.sh:16,40`) |
| Derivação paralela da lista integral | **NÃO PROCEDE como cópia de LABELS**: a lista integral continua lida da régua; o TXT declara um subconjunto fixo de módulos. Porém, **o selo não detecta esse TXT como repetição**: só busca determinada forma inline de `manage.py test … colaboradores … ponto`. (`bin/suite.sh:99`; `bin/vizinhos_da_volta.py:36`; `bin/tests/test_labels_fonte_unica.sh:38–46`) |
| Selo sem mordida | **PROCEDE como cobertura insuficiente**: o novo mata o mutante declarado, mas omite as fugas identificadas; o antigo aprova textualmente o comando canônico recusado. (`bin/tests/test_suite_motivo.sh:29–34,44–48`; `bin/tests/test_suite_sh.sh:31–58,72–81`) |
| Ordem de trava | **NÃO PROCEDE inversão demonstrada**: a recusa antecede preparação, e os testes Django continuam sob a trava; ruff roda antes dela. (`bin/suite.sh:70–77,104,136–143`; `bin/volta_intermediaria.sh:23,32`) |
| Campo com dois significados | **PROCEDE**: `ONLY` “tem texto” é tratado como “é recorte”, embora possa representar toda a suite ou nenhum argumento; rc 75 também pode coexistir com vermelho anterior. (`bin/suite.sh:70,142`; `bin/volta_intermediaria.sh:35–39`) |

## 11/11 06:35 · sessao ac4da24cbf3b · - · - · origem=Ronald (tmux)

P4 — **PROCEDE: a seleção tem lacunas de classe.** O helper considera apenas `.py` existentes no diff e procura referências textuais diretas; não resolve AST, grafo transitivo ou dependências de recursos. (`bin/vizinhos_da_volta.py:10`, `bin/vizinhos_da_volta.py:12`, `bin/vizinhos_da_volta.py:26`, `bin/vizinhos_da_volta.py:31`)

| Classe | Limite e regra mínima sugerida |
|---|---|
| Import relativo | O padrão relativo só é acrescentado quando `pkg.endswith('tests')`; não resolve pacote nem nível. Resolver `ImportFrom` em contexto e dependências transitivas. (`bin/vizinhos_da_volta.py:25`, `bin/vizinhos_da_volta.py:29`) |
| `__init__.py` | Há normalização do inicializador de produção para pacote; portanto existe suporte parcial. O inicializador de testes não entra como obra nem produção. Propagar dependência do inicializador aos consumidores do pacote, incluindo o pacote de testes. (`bin/vizinhos_da_volta.py:13`, `bin/vizinhos_da_volta.py:19`, `bin/vizinhos_da_volta.py:21`, `bin/vizinhos_da_volta.py:24`) |
| `importlib`/strings | Uma string contendo o módulo completo pode casar; nomes montados dinamicamente não têm resolução. Resolver destinos estáticos e usar recorte conservador do pacote quando o destino permanecer indeterminado. (`bin/vizinhos_da_volta.py:26`, `bin/vizinhos_da_volta.py:34`) |
| Fixture JSON | Mudanças não-Python são descartadas. Incluir dependências de recursos carregados pelos testes; fallback por app consumidor. (`bin/vizinhos_da_volta.py:12`) |
| Template | Também descartado como arquivo tocado. Incluir dependências de templates, inclusões, herança e consumidores; fallback por app. (`bin/vizinhos_da_volta.py:12`) |
| Migration | Pode entrar como produção `.py`, mas só seleciona consumidores por citação literal. Propagar mudança de esquema aos testes do app e apps dependentes. (`bin/vizinhos_da_volta.py:12`, `bin/vizinhos_da_volta.py:21`, `bin/vizinhos_da_volta.py:26`) |
| Selo que varre diretório/AST | A descoberta não relaciona escopo varrido com arquivos alterados. Registrar escopos de varredura e selecionar pela interseção com o diff. Alguns scanners estão cobertos pela lista fixa, mas isso não constitui regra geral. (`bin/vizinhos_da_volta.py:31`, `bin/vizinhos_da_volta.py:36`, `bin/selos_globais_da_volta.txt:23`, `bin/selos_globais_da_volta.txt:24`) |

O risco de cada lacuna é excluir um teste dependente do recorte; a ocorrência concreta de volta verde com teste externo quebrado é **[nao verificado]**. A possibilidade de omissão decorre dos filtros e da procura textual. (`bin/vizinhos_da_volta.py:12`, `bin/vizinhos_da_volta.py:15`, `bin/vizinhos_da_volta.py:34`)

Há mais uma classe: descoberta de testes incompleta. Os candidatos exigem arquivo diretamente em diretório chamado `tests`, nome começando com `test`; isso deixa fora outras organizações e nomes de módulos. A regra mínima é usar a descoberta compatível com o executor, antes de calcular dependências. (`bin/vizinhos_da_volta.py:15`, `bin/vizinhos_da_volta.py:16`)

P7, fonte única — **NAO PROCEDE que o selo atual detecte o TXT como repetição.** Ele busca linhas contendo `manage.py test`, `colaboradores` e `ponto`, nessa ordem, em `bin/` e `app/core/hooks`; o TXT contém labels individuais, sem esse comando. (`bin/tests/test_labels_fonte_unica.sh:38`, `bin/tests/test_labels_fonte_unica.sh:39`, `bin/tests/test_labels_fonte_unica.sh:40`, `bin/selos_globais_da_volta.txt:1`)

A lista integral continua sendo lida da régua; o TXT acrescenta uma seleção global manual distinta. Classificá-lo como duplicação da lista integral não é sustentado por esse fluxo. (`bin/suite.sh:99`, `bin/vizinhos_da_volta.py:36`)

Scanners globais efetivamente existentes percorrem diretórios independentemente de importações: o contrato de escritores usa `os.walk`, e o de leitores de vitalidade percorre arquivos e analisa AST. Ambos constam da lista fixa. (`app/core/tests/test_contract_escritores_estado.py:188`, `app/core/tests/test_contract_escritores_estado.py:190`, `app/chamados/tests/test_contract_leitores_vitalidade.py:292`, `app/chamados/tests/test_contract_leitores_vitalidade.py:302`, `bin/selos_globais_da_volta.txt:23`, `bin/selos_globais_da_volta.txt:24`)

## 11/11 06:34 · sessao 2a37b2983b12 · - · - · origem=Ronald (tmux)

**5. PROCEDE parcialmente: os três resultados existem, mas 75 encobre vermelho anterior.** `VOLTA_VERMELHA` é escrito se qualquer etapa falhar; `VOLTA_SEM_VEZ` também é escrito quando a suite retorna 75. A consolidação dá prioridade a 75, mesmo se ruff ou disjuntor já falharam. Portanto “75 = apenas não conseguiu a vez” não é garantido. (`bin/volta_intermediaria.sh:35`, `bin/volta_intermediaria.sh:36`, `bin/volta_intermediaria.sh:38`, `bin/volta_intermediaria.sh:39`)

- **`$TESTE_DOCKER`: correto na leitura estática.** O wrapper carrega `recursos.sh`; ali as flags são construídas e exportadas; a expansão sem aspas produz os argumentos das flags utilizadas no Docker. (`bin/volta_intermediaria.sh:15`, `bin/recursos.sh:81`, `bin/recursos.sh:82`, `bin/volta_intermediaria.sh:23`)
- **`PIPESTATUS`: correto para capturar o Docker/ruff.** A posição zero é lida imediatamente após `docker … | tail -15`; assim recebe o código do Docker, não o do `tail`. A falha exclusiva do `tail` não é incorporada a `ruff_rc`. (`bin/volta_intermediaria.sh:24`, `bin/volta_intermediaria.sh:25`)
- **`VOLTA_ESPERA`: chega à trava.** O wrapper transforma o ambiente, com default 3600, em `--espera`; suite armazena em `ESPERA` e repassa à trava; a trava usa em `flock -w`. (`bin/volta_intermediaria.sh:32`, `bin/suite.sh:53`, `bin/suite.sh:136`, `bin/trava_teste.sh:27`, `bin/trava_teste.sh:36`)
- **`VOLTA_PORQUE`: chega ao suite.** O wrapper fornece a variável no ambiente da chamada e suite a usa como destino de stderr do helper. (`bin/volta_intermediaria.sh:32`, `bin/suite.sh:85`)
- **Limite adicional:** a coleta inicial de `.py` não verifica o rc do `git diff`, embora `pipefail` esteja ativo; sem `errexit`, a atribuição pode falhar e o processamento continuar com `PY` vazio e `ruff_rc=0`. A etapa posterior do helper usa `check=True`, portanto falha de diff nessa etapa normalmente produz suite rc1, não verde. (`bin/volta_intermediaria.sh:10`, `bin/volta_intermediaria.sh:18`, `bin/volta_intermediaria.sh:21`, `bin/vizinhos_da_volta.py:10`, `bin/vizinhos_da_volta.py:11`, `bin/suite.sh:86`)
- **Execução real e permissões de escrita dos logs:** [nao verificado].

**6. Mapa de escritores e leitores.**

| Valor novo | Quem escreve | Quem lê |
|---|---|---|
| `MOTIVO` | Inicialização vazia; parser `--motivo`. O ambiente homônimo é sobrescrito. (`bin/suite.sh:47`, `bin/suite.sh:54`) | Guarda de formato; mensagem de recusa; saída seca. (`bin/suite.sh:70`, `bin/suite.sh:72`, `bin/suite.sh:91`) |
| `VOLTA` | Inicialização vazia; parser `--volta`. O wrapper fornece o valor posicional `BASE` como argumento. (`bin/suite.sh:47`, `bin/suite.sh:55`, `bin/volta_intermediaria.sh:13`, `bin/volta_intermediaria.sh:32`) | Guarda; ativação do recorte; argumento do helper; mensagem de falha; saída seca. (`bin/suite.sh:70`, `bin/suite.sh:82`, `bin/suite.sh:85`, `bin/suite.sh:86`, `bin/suite.sh:91`) |
| `SECO` | Inicialização zero; parser `--seco` escreve 1. (`bin/suite.sh:47`, `bin/suite.sh:56`) | Condicional que imprime e encerra antes de labels/env-file/montagem/trava/Docker. (`bin/suite.sh:90`, `bin/suite.sh:93`, `bin/suite.sh:99`, `bin/suite.sh:104`, `bin/suite.sh:117`, `bin/suite.sh:136`) |
| `VOLTA_PORQUE` | Wrapper define o caminho `"$OUT.porque"` no ambiente do subprocesso. Chamador externo também pode defini-lo: o consumidor lê o ambiente. (`bin/volta_intermediaria.sh:32`, `bin/suite.sh:85`) | Suite lê o caminho e abre/trunca o arquivo para stderr; helper escreve justificativas, produção tocada e total nesse stderr; wrapper lê o arquivo para contar labels. (`bin/suite.sh:85`, `bin/vizinhos_da_volta.py:40`, `bin/vizinhos_da_volta.py:41`, `bin/vizinhos_da_volta.py:42`, `bin/volta_intermediaria.sh:34`) |
| `selos_globais_da_volta.txt` | Lista literal versionada, 24 entradas; escritor automático não identificado no fluxo analisado. (`bin/selos_globais_da_volta.txt:1`, `bin/selos_globais_da_volta.txt:24`) | Helper lê o arquivo **ao lado dele próprio**, não dentro de `wt`; acrescenta cada entrada ao conjunto de saída e à justificativa. (`bin/vizinhos_da_volta.py:36`, `bin/vizinhos_da_volta.py:37`) |

`VOLTA_PORQUE` tem default `/dev/null`, portanto justificativas não são preservadas quando suite é chamado diretamente sem essa variável. (`bin/suite.sh:85`)

## 11/11 06:34 · sessao 015a5f061c14 · - · - · origem=Ronald (tmux)

**Pergunta 2 — PROCEDE: a documentação canônica quebra e o selo não acusa.**

| Chamador/referência | Efeito da guarda |
|---|---|
| `CLAUDE.md:96`: `bash ~/saas-hasner/bin/suite.sh` | Recusa rc 2, oferecendo `--volta` e `--only`, antes do env-file/docker (`bin/suite.sh:69-77`). |
| `bin/volta_intermediaria.sh:32` | Passa `--volta`, `--dir` e `--espera`; não exige motivo nessa modalidade (`bin/suite.sh:70`, `82-87`). |
| `bin/tests/test_suite_motivo.sh:24` | Executa cópia da porta; chamadas nuas são deliberadamente recusadas e válidas/`--only` devem chegar à trava falsa (`bin/tests/test_suite_motivo.sh:29-34`). |
| `app/core/placar_estrutural.py:109-110` | Publica instruções com `--only`; a guarda não as recusa (`bin/suite.sh:70`). É texto, não execução Python da suite (`app/core/placar_estrutural.py:109`). |
| Exemplos de uso de `bin/suite.sh:23-32` | `--motivo` válido, `--only` e `--volta` passam pela guarda; exemplos isolados `--dir`, `--parallel`, `--espera`, `-- --failfast`, ou `--seco` sem motivo/recorte são recusados (`bin/suite.sh:50-77`, `90-94`). |
| Receita histórica `app/docs/RELATO-ARQUIVO.md:37800` | Se colada hoje sem complemento, `--dir` sozinho é recusado (`bin/suite.sh:50`, `69-77`). |
| Orientação histórica `app/docs/PROMPTS.md:1069` e `app/docs/BACKLOG.md:1433`: suite por `--dir` | Se usada apenas assim, é recusada pela mesma guarda (`bin/suite.sh:69-77`). |

Não localizei outros chamadores executáveis de `suite.sh` em `.claude/` ou hooks; completude diante de chamadas construídas dinamicamente: **[nao verificado]**. `regua.sh` e `pre-push.sh` executam sua própria chamada, portanto esta guarda não os alcança (`bin/regua.sh:168`; `bin/pre-push.sh:186`; `app/core/hooks/pre-push.sh:82`; explicação em `CLAUDE.md:101-103`).

**O selo `test_suite_sh.sh` permanece logicamente verde:** cobra apenas presença textual de `bin/suite.sh` no bloco (`bin/tests/test_suite_sh.sh:31-42`) e strings de trava, `manage.py test`, leitura de LABELS e ausência de DB_PASSWORD (`bin/tests/test_suite_sh.sh:50-58`). Seu fixture “bom” é exatamente a chamada nua que agora é recusada (`bin/tests/test_suite_sh.sh:72-81`). Não executa esse comando; verde medido nesta revisão: **[nao verificado]**. Executá-lo criaria arquivos temporários, contrariando a regra de leitura (`bin/tests/test_suite_sh.sh:63-76`).

**Pergunta 7, fonte única — PROCEDE a cegueira do selo; não confundir com uma cópia literal de LABELS.**

`test_labels_fonte_unica.sh` não reconhece o novo `.txt` como lista repetida: seu detector exige a expressão `manage.py test … colaboradores … ponto` (`bin/tests/test_labels_fonte_unica.sh:38-46`); o arquivo novo contém somente módulos por linha (`bin/selos_globais_da_volta.txt:1-24`). O helper lê essa lista diretamente (`bin/vizinhos_da_volta.py:36-37`), enquanto a lista de apps permanece em `bin/regua.sh:23`. A lista fixa tem autorização no anexo fornecido; classificação disso como violação ampla da fonte única, além da cegueira demonstrada do selo: **[nao verificado]**.

## 11/11 06:34 · sessao e94ad08110dc · FIO-E0-V4-E4B · construtor · origem=Code

Cura aplicada: um lote existente cobre S143/S128 e permanece até o retorno do C7. [Fonte](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/services/disputa_emissao.py:1923). [nao verificado]

Entregues o [diff](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/E4B_C4V5.patch) e o [recibo com comandos e RCs](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/E4B_C4V5_ENTREGA.txt:27):

- Sem banco: grupos de 50, 16 e 40 testes, todos rc=0. [nao verificado]
- RED sobre a base: rc=1, assertion `1 != 5`.
- Dois mutantes novos de tranca detectados; mutante do protocolo também falhou como esperado. [Evidências](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/E4B_C4V5_ENTREGA.txt:50).

Contagem esperada, **[nao verificado]**:

| Caso | Queries |
|---|---:|
| reconciliar | 60 |
| `_gravar_estado` sem pergunta | 28 |
| batida, 1 chamado fecha | 213 |
| batida, 3 chamados fecham | 269 |

Tetos 213/269 preservados; o custo `+N _ancoradas` continua como pergunta de desenho no [C8](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/ponto/tests/test_selo_chokepoint_escrita.py:189).

O [RED ORM pareado e seu mutante](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4_orm.py:34) estão preparados para o Code. Execução ORM e certificação final: **[nao verificado]**.

## 11/11 06:27 · sessao 8ca9ab311c9e · - · - · origem=Ronald (tmux)

Escopo recomendado: `tranca_da_consulta` deve envolver o restante inteiro do resolver, até o retorno final. O laço final chama `d.pode_fechar()` mesmo quando os anteriores pulam a disputa; esse juiz recarrega perguntas e consulta C6 (`chamados/services/disputa_emissao.py:2057`, `2069`; `chamados/models.py:1848–1853`). Assim, o memo existente também atende às instâncias posteriores (`chamados/juizes.py:1060–1062`).

Envolver somente S143/S128 deixa escapar essa reutilização: o contexto começa com memo vazio e o descarta na saída (`chamados/juizes.py:1093–1104`). A economia efetiva permanece [nao verificado].

Para provar o delta pedido, o melhor RED pareado reutiliza `_batida_com_furos_vivos(1/3)`, com rollback e memo frio por rodada, comparando contexto real versus `nullcontext`, e `wraps` sobre `periodo_trancado` (`ponto/tests/test_selo_chokepoint_escrita.py:403–427`, `481–494`). Exigir mesmos fechamentos, redução de uma chamada ao período e redução correspondente das queries; valores finais [nao verificado].

O caso existente de pergunta viva também pode servir com escopo completo: após S143/S128, `pode_fechar()` volta a ler C6 (`chamados/tests/test_e4b_c4_orm.py:172–198`; `chamados/services/disputa_emissao.py:2069`). Nele, o total economizado pode incluir leitura de Empresa; não deduzir delta total exatamente igual a um [nao verificado].

Antecipar a carga S128 muda seu instante em relação à ressuscitação: `reabrir()` pode descarimbar perguntas e reabrir disputas (`chamados/models.py:1451`, `1566–1575`). A consulta ampla posterior reencontra disputas abertas (`chamados/services/disputa_emissao.py:2010–2011`), mas equivalência integral desse adiantamento [nao verificado].

## 11/11 06:22 · sessao b6de4d034e52 · FIO-E0-V4-E3 · construtor · origem=Code

Cura em [ponto/services/cartorio.py:1233](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/services/cartorio.py:1233): primeiro argumento volta à chamada direta da porta. A classificação permanece depois da guarda (`cartorio.py:1227`); o marcador conserva `_insumo_nb` (`cartorio.py:1219–1222`).

```diff
@@ -1233 +1233 @@
-                    _insumo_nb, marcos, chamados_vivos=0, chamados_total=0, cobrancas_vivas=0,
+                    com_insumo_dia_sem_trabalho(_dia_nb, trabalha=cel.trabalha, ata=_ata_nb), marcos, chamados_vivos=0, chamados_total=0, cobrancas_vivas=0,
```

| Execução sem banco | Resultado | Testemunha |
|---|---|---|
| Catraca na base informada | RC 1; 1 FAIL, 0 ERROR | `ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:226` |
| Catraca na cura | RC 0; 3 testes | mesmo arquivo:200 | [nao verificado]
| Memória NB | RC 0; 15 testes | `ponto/tests/test_e3_nb_impressao_memoria.py:84` |
| Pós-suite | RC 0; 5 testes | `chamados/tests/test_e3_pos_suite_sem_banco.py:19` |
| Medida do turno | RC 0; 35 testes | `core/tests/test_medida_turno_do_par.py:478` |
| Mutante: argumento `_insumo_nb` | RC 1; 1 FAIL, 0 ERROR | catraca:226 |
| Mutante: argumento `_dia_nb` | RC 1; 1 FAIL, 0 ERROR | catraca:226 |
| Mutante: classificação antes da guarda | RC 1; 1 FAIL, 0 ERROR | memória NB:236 |

`LINHAS_REGISTRADAS` passou sem ajuste (`core/tests/test_medida_turno_do_par.py:539`). O alvo do marcador permanece igual (`ponto/tests/x1_mutantes.py:24`).

Comandos dos testes e do mutante de antecipação: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B -m unittest ponto.tests.test_e3_nb_impressao_memoria -v

PYTHONDONTWRITEBYTECODE=1 python3 -B -m unittest chamados.tests.test_e3_pos_suite_sem_banco -v

E3_NB_MUTANTE=classificacao_antes_guarda PYTHONDONTWRITEBYTECODE=1 python3 -B -m unittest ponto.tests.test_e3_nb_impressao_memoria.ImpressaoNuncaBateuMemoriaTest.test_F3_pulada_nao_classifica_nem_carrega -v
```

Comando da medida, conforme procedimento sem Django de `core/tests/test_medida_turno_do_par.py:20`:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B - <<'PY'
import pathlib, types, unittest
p = pathlib.Path('core/tests/test_medida_turno_do_par.py')
suite = types.ModuleType('suite_turno') [nao verificado]
suite.__file__ = str(p.resolve()) [nao verificado]
source = p.read_text().replace('from django.test import SimpleTestCase',
                              'from unittest import TestCase as SimpleTestCase')
exec(compile(source, str(p), 'exec'), suite.__dict__) [nao verificado]
unittest.main(module=suite, verbosity=2) [nao verificado]
PY
```

Para reproduzir catraca, RED e os dois mutantes, execute o bloco abaixo com `CASO=green`, `CASO=base`, `CASO=indireto` e `CASO=sem_porta`, respectivamente. As asserções originais são extraídas de `ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:200`; as mutações ficam em memória.

```bash
CASO=green PYTHONDONTWRITEBYTECODE=1 python3 -B - <<'PY'
import ast, os, subprocess, unittest
from pathlib import Path
from unittest.mock import patch

p = Path('ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py')
nomes = {'DECLARADOS', '_chamadas_do_supra', 'CatracaProdutoresDoDictTest'}
nodes = [n for n in ast.parse(p.read_text()).body
         if getattr(n, 'name', None) in nomes or isinstance(n, ast.Assign)
         and any(isinstance(t, ast.Name) and t.id in nomes for t in n.targets)]
ns = {'ast': ast, 'APP': Path.cwd(), 'SimpleTestCase': unittest.TestCase}
exec(compile(ast.Module(body=nodes, type_ignores=[]), str(p), 'exec'), ns)
caso = os.environ['CASO']
alvo = Path('ponto/services/cartorio.py').resolve()
ler = Path.read_text
fonte = ler(alvo)
if caso == 'base':
    fonte = subprocess.run(
        ['git', 'show', '769ed8cfb0cecf6de81bcfb5c7441a4668b87353:app/ponto/services/cartorio.py'],
        check=True, capture_output=True, text=True).stdout
elif caso != 'green':
    antes = '                    com_insumo_dia_sem_trabalho(_dia_nb, trabalha=cel.trabalha, ata=_ata_nb), marcos,'
    depois = {'indireto': '                    _insumo_nb, marcos,',
              'sem_porta': '                    _dia_nb, marcos,'}[caso]
    assert fonte.count(antes) == 1
    fonte = fonte.replace(antes, depois, 1)
with patch.object(Path, 'read_text',
                  lambda p, *a, **kw: fonte if p.resolve() == alvo else ler(p, *a, **kw)):
    r = unittest.TextTestRunner(verbosity=2).run(
        unittest.defaultTestLoader.loadTestsFromTestCase(ns['CatracaProdutoresDoDictTest']))
print(f'CASO={caso} TESTES={r.testsRun} FAIL={len(r.failures)} ERROR={len(r.errors)}') [nao verificado]
raise SystemExit(0 if r.wasSuccessful() else 1)
PY
```

Ruff: comando tentou executar, retornou RC 1 com `No module named ruff`; análise **[nao verificado]**.

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B -m ruff check --no-cache ponto/services/cartorio.py
```

Suite inteira e opus final nesta volta: **[nao verificado]**. [nao verificado]

## 11/11 06:04 · sessao cd87c28e1945 · FIO-E0-V4-E4B · construtor · origem=Code

Corrigi o enxerto dos mutantes, mas **o item 1 permanece incompleto**: não cortei o custo por S143 nem aumentei os tetos para encobri-lo. A pendência está registrada junto aos números. [test_selo_chokepoint_escrita.py:183](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/ponto/tests/test_selo_chokepoint_escrita.py:183)

Por sítio:

| Sítio | Delta: 1 / 3 chamados | Resultado |
|---|---:|---|
| `um_colaborador_por_identidade` | 0 / 0 diretamente | Troca instâncias; não consulta. [juizes.py:1121](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/juizes.py:1121) |
| `trava_na_ordem` no wrapper | 0 / 0 adicional | Há uma chamada por entrada; as linhas diferentes da base/head não representam duplicação. [disputa_emissao.py:1878](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/services/disputa_emissao.py:1878) |
| Nova carga `_ancoradas` | +2 / +4 | Continua uma consulta por S143. Sua lista não substitui S128: os universos diferem quanto a validadas, carimbadas e disputa fechada. [models.py:344](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/models.py:344), [disputa_emissao.py:1904](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/services/disputa_emissao.py:1904) |
| Residual | +1 / +1 | Atribuição à cadeia C6→tranca **[nao verificado]**. Acrescentei pareamento por função à sonda. [e4b_c4_medir_queries.py:58](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c4_medir_queries.py:58) |

`sem_cerca` e `sem_rollback` copiavam os globais antes da instalação do spy. O corpo mutado chamava `_executar_dia` antigo, sem executar a cascata adversarial instalada depois. Agora o enxerto conserva os globais vivos; mantive `call_count == 1` e as precondições de ressuscitação. [e4b_c4_base.py:12](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c4_base.py:12), [test_e4b_c4_orm.py:57](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4_orm.py:57), [test_e4b_c4_orm.py:133](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4_orm.py:133)

O mapa economizou **zero no chokepoint fornecido: 214/214**. Preparei outro ensaio, com pergunta viva nos dois laços e teto pareado medido durante o teste. Ele exige diferença antes de aceitar a morte do mutante; isso ainda não prova economia em `registrar_batida`. Execução ORM **[nao verificado]**. [test_e4b_c4_orm.py:156](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4_orm.py:156) [nao verificado]

O RED do spy passou; `globais_copiados` morreu na assertion-alvo. O protocolo passou; `mapa_falha_aceita` morreu por aceitar resultado sem diferença comprovada. [test_e4b_c4_base.py:17](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4_base.py:17), [test_e4b_c4_orm_protocolo.py:77](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4_orm_protocolo.py:77) [nao verificado]

```bash
export PYTHONDONTWRITEBYTECODE=1

git diff -- .

python3 -m chamados.tests.test_e4b_c4_base
python3 -m chamados.tests.test_e4b_c4_base --mutante globais_copiados

python3 -m chamados.tests.test_e4b_c4_orm_protocolo
python3 -m chamados.tests.test_e4b_c4_orm_protocolo --mutante mapa_falha_aceita
```

Comandos ORM para a sessão; o executor mantém GREEN como condição para contar mutantes. [e4b_c4_verificar.py:67](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c4_verificar.py:67)

```bash
export PYTHONDONTWRITEBYTECODE=1

python3 -m chamados.tests.e4b_c4_verificar --orm

c4_orm=chamados.tests.test_e4b_c4_orm.CercaC4ORMTest

for c4_mutante in sem_cerca sem_rollback sem_alvo; do
  E4B_C4_MUTANTE="$c4_mutante" python3 manage.py test \
    "$c4_orm.test_RED_Ch12_P90_D90_fora_da_competencia_K6_false_desfaz" --noinput
done

for c4_mutante in foto_so_dia foto_antes_C; do
  E4B_C4_MUTANTE="$c4_mutante" python3 manage.py test \
    "$c4_orm.test_RED_foto_inteira_sob_C_e_reversao_antes_do_primeiro_ato" --noinput
done

E4B_C4_QUERY_MUTANTE=tranca_sem_mapa python3 manage.py test \
  "$c4_orm.test_RED_mapa_comum_economiza_query_entre_dois_selects" --noinput

for c4_modo in base head; do
  E4B_C4_QUERIES="$c4_modo" python3 manage.py test \
    chamados.tests.e4b_c4_medir_queries.MedidaQueries.test_gate_da_premissa_fecha_com_dia_e_celula \
    chamados.tests.e4b_c4_medir_queries.MedidaQueries.test_gravar_estado_sem_pergunta \
    chamados.tests.e4b_c4_medir_queries.MedidaQueries.test_registrar_batida_com_1_furo_vivo_que_ela_fecha \
    chamados.tests.e4b_c4_medir_queries.MedidaQueries.test_registrar_batida_com_3_furos_vivos_que_ela_fecha \
    --noinput
done
```

## 11/11 06:02 · sessao 70e72c812f20 · - · - · origem=Ronald (tmux)

Não achei corte seguro das cargas `_ancoradas` com as restrições atuais:

- A API devolve uma `list` já avaliada, sem validadas e incluindo carimbadas/disputas fechadas; S128 tem universo diferente. Fundir perde casos; complemento mantém duas consultas. `chamados/models.py:336`, `chamados/services/disputa_emissao.py:1904`.
- A carga anterior do gate não chega ao serviço: `_f1_ancoradas` é limpa antes do `save` que dispara S143. `chamados/models.py:216`, `chamados/models.py:296`.
- O identity map não consulta diretamente; o custo pode vir do consumidor C6 e do memo por instância fora de request. `chamados/juizes.py:1134`, `chamados/juizes.py:1054`.
- O wrapper contém uma chamada de trava por entrada, seguida de uma chamada do corpo. O receiver contém uma chamada do wrapper. `chamados/services/disputa_emissao.py:1878`, `chamados/signals.py:115`.

Existe consulta por pergunta viva em `reabrir_credor_de_pergunta_viva`: recarrega o credor em `chamados/services/disputa_emissao.py:1826`. Passar o credor já conhecido poderia cortá-la, mas conservar essa instância entre perguntas exige provar frescor diante de reentrada por outro objeto; o próprio modelo documenta esse risco. `chamados/models.py:274`. Não recomendo esse corte sem RED adversarial.

Atribuir o residual +1 exclusivamente à tranca continua [nao verificado]. A cadeia nova alcança a tranca antes de julgar carimbo/célula, mas isso sozinho não demonstra o delta SQL observado. `chamados/services/disputa_emissao.py:1821`, `chamados/juizes.py:1329`.

## 11/11 05:50 · sessao 4cfcfad110c6 · FIO-E0-V4-E3 · construtor · origem=Code

Candidata implementada em cinco arquivos. **20 testes sem banco + 35 da medida passaram; sete mutantes morreram por asserção; Ruff limpo.** ORM, contagem SQL após a cura e certificação final: **[nao verificado]**. [nao verificado]

- **F1/F2:** o marcador deixou de consultar `_vd_nb`. Agora consulta o juiz existente de dia sem trabalho; `isento` continua entrando exclusivamente pela parte com horizonte `regra >= 4`. A classificação e `derivar_veredito` permanecem responsáveis pela lavra. [cartorio.py:1217](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/services/cartorio.py:1217), [cartorio.py:1232](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/services/cartorio.py:1232).
- **F3:** classificação, cadastro e carga de turnos ficaram depois da guarda, dentro de `apply_`. A impressão consulta somente a ata em memória e o juiz puro; isso elimina o ciclo impressão → classificação → impressão. `ponto/services/cartorio.py:1218`, `:1227`, `:1231`.
- **Registro:** somente a ocorrência movida passou de `1319` para `1321`. `core/tests/test_medida_turno_do_par.py:315`.
- **S2/S3:** adaptei o carregamento AST para fornecer os helpers reais novos; a asserção do hash legado e da cobertura cega permanece. `chamados/tests/test_e3_pos_suite_sem_banco.py:94`, `:139`.
- **X1:** atualizei somente o alvo textual de `sem_fonte_no_gancho`. `ponto/tests/x1_mutantes.py:24`.

Adotei “insumo já carregado pelo marcador” como o insumo de dia sem trabalho que o marcador anterior representava indiretamente. **A fonte agora distingue dia sem trabalho, em vez de todo veredito não acusador.** A equivalência do gancho distante para trabalho justificado, turno em curso e parcial sem resto fica **[nao verificado]**; o veredito continua passando pelo classificador e por `derivar_veredito`. `ponto/services/cartorio.py:1222`, `:1232`.

A hipótese causal procede no HEAD recebido: o veredito alimentava o marcador antes da guarda. A atribuição estática das seis queries adicionais é:

| Sítio | Queries inferidas |
|---|---:|
| Cadastro dos vínculos — `ponto/services/cartorio.py:1111` | 1 |
| Vínculos de `dias_encerrados` — `ponto/turnos.py:2149` | 1 |
| Batidas de `turnos_do_colab` — `ponto/turnos.py:1915` | 1 |
| Candidatos a vínculo — `escala/alimentacao.py:148` | 1 |
| Escala para ler a ata — `ponto/turnos.py:1559` | 1 |
| DNA da janela — `escala/utils.py:100` | 1 |

Isso é **inferência do código, não captura SQL**. `ata_do_dia`, `classificar_dia` e `derivar_veredito` são puros nesses sítios. `ponto/services/cartorio.py:701`, `ponto/supra_juiz.py:204`, `escala/models.py:1624`. Main=59 e HEAD recebido=65 vêm do raspado **[nao verificado nesta volta]**; resultado SQL após a cura: **[nao verificado]**. O teto 59 permanece em `ponto/tests/test_selo_chokepoint_escrita.py:118`.

O diff completo pode ser obtido sem escrita:

```bash
git diff -- ponto/services/cartorio.py ponto/tests/x1_mutantes.py core/tests/test_medida_turno_do_par.py chamados/tests/test_e3_pos_suite_sem_banco.py
git diff --no-index -- /dev/null ponto/tests/test_e3_nb_impressao_memoria.py
```

Os REDs e mutantes estão em [test_e3_nb_impressao_memoria.py:25](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/tests/test_e3_nb_impressao_memoria.py:25). Executei: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B -m unittest \
  ponto.tests.test_e3_nb_impressao_memoria \
  chamados.tests.test_e3_pos_suite_sem_banco -v
```

Resultado: 20 testes, RC=0. A execução da medida usa o procedimento sem Django documentado em `core/tests/test_medida_turno_do_par.py:20`; resultado: 35 testes, RC=0.

Comandos individuais reproduzíveis; base e mutantes abaixo retornaram RC=1 por asserção, sem ERROR:

```bash
E3_NB_TEST=ponto.tests.test_e3_nb_impressao_memoria.ImpressaoNuncaBateuMemoriaTest

PYTHONDONTWRITEBYTECODE=1 E3_NB_BASE=9710c1733f2b2e18f9a397b525875d631d94c5f0 python3 -B -m unittest \
  "$E3_NB_TEST.test_F1_costura_do_isento_e_unica" \
  "$E3_NB_TEST.test_F2_comp09_nao_rejulga" \
  "$E3_NB_TEST.test_F3_pulada_nao_classifica_nem_carrega"

PYTHONDONTWRITEBYTECODE=1 E3_NB_MUTANTE=isento_no_marcador python3 -B -m unittest "$E3_NB_TEST.test_F1_costura_do_isento_e_unica"
PYTHONDONTWRITEBYTECODE=1 E3_NB_MUTANTE=sem_fonte python3 -B -m unittest "$E3_NB_TEST.test_folga_vai_pelo_juiz_e_guarda_a_fonte"
PYTHONDONTWRITEBYTECODE=1 E3_NB_MUTANTE=literal_incondicional python3 -B -m unittest "$E3_NB_TEST.test_folga_vai_pelo_juiz_e_guarda_a_fonte"
PYTHONDONTWRITEBYTECODE=1 E3_NB_MUTANTE=sem_isento python3 -B -m unittest "$E3_NB_TEST.test_isento_no_horizonte_rejulga"
PYTHONDONTWRITEBYTECODE=1 E3_NB_MUTANTE=sem_horizonte python3 -B -m unittest "$E3_NB_TEST.test_F2_comp09_nao_rejulga"
PYTHONDONTWRITEBYTECODE=1 E3_NB_MUTANTE=cobertura_cega python3 -B -m unittest "$E3_NB_TEST.test_B5u_cobertura_visivel_rejulga"
PYTHONDONTWRITEBYTECODE=1 E3_NB_MUTANTE=classificacao_antes_guarda python3 -B -m unittest "$E3_NB_TEST.test_F3_pulada_nao_classifica_nem_carrega"
```

Labels ORM exatos para a sessão, **não executados aqui [nao verificado]**:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B manage.py test --settings=config.settings.ci \
  ponto.tests.test_e3_isento_na_impressao.RamoNuncaBateu.test_a_costura_MORDE \
  ponto.tests.test_e3_isento_na_impressao.RamoNuncaBateu09.test_isento_comp_09_nao_rejulga \
  ponto.tests.test_selo_chokepoint_escrita.SeloChokepointEscritaTest.test_gate_da_premissa_fecha_com_dia_e_celula

PYTHONDONTWRITEBYTECODE=1 python3 -B manage.py test --settings=config.settings.ci \
  ponto.tests.test_e3_isento_na_impressao \
  ponto.tests.test_selo_b5u_nunca_bateu_ve_ausencia \
  ponto.tests.test_x1_folga_sem_adesao \
  ponto.tests.test_selo_chokepoint_escrita \
  core.tests.test_medida_turno_do_par
```

X1 completo, base e dois mutantes ORM, **[nao verificado]**:

```bash
PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings.ci python3 -B -m ponto.tests.x1_mutantes --modo green
PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings.ci python3 -B -m ponto.tests.x1_mutantes --modo base
PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings.ci python3 -B -m ponto.tests.x1_mutantes --modo mutante --mutante literal_incondicional
PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings.ci python3 -B -m ponto.tests.x1_mutantes --modo mutante --mutante sem_fonte_no_gancho
```

Censo: os chamadores de produção de `impressao_insumos` continuam sendo os dois ramos do cartório, com reexportação pelo command. `ponto/services/cartorio.py:1220`, `:1361`; `ponto/management/commands/processar_cartorio.py:20`. `_vd_nb` é consumido localmente pela renomeação, lavra e fatos do status; `sem_adesao` é consumido pelo hash. `ponto/services/cartorio.py:525`, `:1252`, `:1255`, `:1260`.

Os demais leitores são testes; o censo integral, incluindo menções documentais, é reproduzível com: [nao verificado]

```bash
rg -n '\bimpressao_insumos\b|\b_vd_nb\b|sem_adesao=' \
  --glob '*.py' --glob '!**/migrations/**'
```

Ruff passou nos cinco arquivos alterados com `check --no-cache`; `git diff --check` também passou. A certificação da cura permanece com a suite e o opus final **[nao verificado]**. [nao verificado]

## 11/11 05:40 · sessao 0e7c4b4de1ab · - · - · origem=Ronald (tmux)

A cura com `dia_sem_trabalho` mantém F1/F2/F3 e a classe de folga, mas **estreita o gancho de adesão distante**: trabalho justificado, turno em curso e parcial sem resto também podem receber `concorde` pela classificação atual sem serem dia sem trabalho. Nesses casos o marcador anterior era verdadeiro; o juiz puro responderia falso (`ponto/supra_juiz.py:204`, `ponto/supra_juiz.py:369`, `chamados/juizes.py:441`).

Há colisão entre os argumentos já presentes no hash: X1 cria trabalho e folga com o mesmo DNA, sem batida/cobertura e na mesma regra; B5 exige impressão legada no trabalho, enquanto X1 exige fonte distinta na folga (`ponto/tests/test_x1_folga_sem_adesao.py:56`, `ponto/tests/test_x1_folga_sem_adesao.py:134`, `ponto/tests/test_selo_b5u_nunca_bateu_ve_ausencia.py:129`). Portanto um marcador exclusivamente desses argumentos não distingue os dois.

Preservar a classificação ampla sem queries exigiria expor carga e grade anterior à isenção: os vínculos já existem em `rows`, as células em `_cels_gr`, e a isenção é aplicada posteriormente (`escala/utils.py:1296`, `escala/utils.py:1308`, `escala/utils.py:1850`). Apenas passar `isento_ponto=False` não elimina o efeito prévio da isenção sobre as células da grade (`escala/utils.py:855`).

Não escrevi arquivos; resultados ORM e contagem real de queries: [nao verificado].

## 11/11 05:10 · sessao 7f992e0a1aee · FIO-E0-V4-E4B · construtor · origem=Code

Entregues o [diff](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4-v3.diff:1) e o [recibo com comandos de cada teste e mutante](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4-v3.txt:124). [nao verificado]

- **Fixture:** julho ficava abaixo do piso automático. A entrada passou para 21/09, competência 10, mantendo as precondições fortes. [Rastro da causa](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4-v3.txt:7). [nao verificado]
- **S143:** mantido `+1` justificado por chamado; fundir perderia validadas do S128. Os dois laços agora compartilham o mapa canônico do colaborador. [Justificativa por sítio](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4-v3.txt:53).
- **Sonda:** modo base usa o arquivo inteiro do main `fb9269ea9`, incluindo reexports em memória. [Implementação](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c4_base.py:9).

Observado localmente: 59+2 testes de memória e 40 selos passaram; os mutantes novos caíram nas assertions indicadas. [Registro](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4-v3.txt:124). [nao verificado]

Tetos `60/28/213/269` são **deduzidos, a remedir**. GREEN ORM, mortes ORM e opus: **[não verificado]**. [Comandos pela trava](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4-v3.txt:162).

## 11/11 05:00 · sessao c34bbb0cd360 · - · - · origem=Ronald (tmux)

A fusão fiel não cabe na porta atual:

- Main `fb9269ea9:app/chamados/services/disputa_emissao.py:1823` já consulta perguntas com destino no chamado, `materializada_em=NULL` e disputa aberta; inclui validadas.
- `_ancoradas` retorna `list`, exclui validadas e inclui outros universos (`chamados/models.py:336`, `:344`). Não fornece QuerySet para `union`.
- Perder validadas altera comportamento: C6 dá morta (`chamados/juizes.py:1325`), mas `carimbar` pode escrever materialização/via e salvar (`chamados/models.py:3126`, `:3152`). Também altera disputas candidatas (`chamados/services/disputa_emissao.py:1977`, `:2000`).
- Buscar complemento de validadas ou usar prefetch conserva duas cargas. Portanto permanece justificável **+1 por S143**, sem duplicar o predicado canônico (`chamados/models.py:344`).

Há reutilização canônica possível da tranca:

```python
colab = chamado._state.fields_cache.get('colaborador')
mapa = {chamado.colaborador_id: colab} if colab is not None else {}
```

Passar `mapa` aos **dois** `um_colaborador_por_identidade` evita criar donos separados do memo; a função aceita mapa sem consultar banco (`chamados/juizes.py:1121`, `:1134`). Seed apenas da FK quente evita adicionar consulta em chamados sem perguntas. Memo da tranca pertence ao request/instância (`chamados/juizes.py:1054`).

Antes de reconciliar na batida, cartório chama E2 (`ponto/services/cartorio.py:969`, `:974`), que usa C6 e tranca sobre o `colab` recebido (`chamados/reconciliador.py:650`, `:654`, `:661`). Contudo a consulta de chamados não aquece explicitamente `ch.colaborador` com essa mesma instância (`ponto/services/cartorio.py:117`). Compartilhar mapa entre os dois laços remove sua duplicação; aproveitamento do memo anterior e redução exata são **[não verificado]**.

Tetos previstos se cair o único `+1 periodo_trancado`: **60/28/213/269 [não verificado]**. Permanecem incrementos main→head **+1/+1/+2/+4**, compatíveis com os sítios S143 do `delta_main_head.txt` fornecido.

Ao ajustar assinatura do identity map nas chamadas, atualizar o double `lambda ps: ps` (`chamados/tests/test_e4b_classe_e_memoria.py:291`) para aceitar `mapa=None`; caso contrário a suíte de memória falha antes da assertion-alvo.

## 11/11 04:59 · sessao 919502c461f8 · - · - · origem=Ronald (tmux)

Causa da fixture: julho pertence à competência 07, abaixo do piso automático `2026-09`; `forcar=True` apenas força julgamento e não equivale a `toque=True`. O cartório cala a emissão do chamado, enquanto a fábrica ainda emite a pergunta (`ponto/services/cartorio.py:270`, `:312`, `:1687`, `:1695`, `:1707`, `:1773`, `:1008`).

Caminho real:

- `registrar_batida` cria a batida (`ponto/registro_batida.py:170`).
- O signal julga o dia anterior e o próprio dia (`ponto/signals.py:108`, `:131`).
- `julgar_celula` chama `julgar_colab`, que chama a fábrica (`ponto/services/cartorio.py:2000`, `:1008`).
- A fábrica chama o emissor (`chamados/services/fabrica_da_celula.py:172`).
- O emissor deriva o destino; zero candidatos retorna `(None, None)` (`chamados/services/disputa_emissao.py:411`, `:238`).
- O reatamento automático também não salva julho: passa por `reatar_pergunta_orfa` e pelo c2, que barra nascimento abaixo do mesmo piso (`chamados/services/disputa_emissao.py:459`; `chamados/reconciliador.py:1621`, `:828`, `:840`).

Entrada coerente proposta: construir o segundo fio em **21/09/2026**, competência **10/2026**, mantendo o alvo do backfill em 09/2026. Essa data continua passada no freeze de 25/09, fica fora do alvo e dentro do horizonte automático. Adaptar a exportação e a expectativa BANG para 10/2026 (`chamados/tests/test_e4_backfill.py:28`, `:49`; `ponto/services/cartorio.py:320`).

Isso preserva a precondição forte e evita criar ou alterar diretamente chamado, pergunta, veredito ou carimbo históricos. Alcance efetivo e execução ORM com essa entrada: [não verificado].

## 11/11 04:57 · sessao 18b1b34d5d9d · FIO-E0-V4-E3 · construtor · origem=Code

Entregues o [diff](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/docs/e3_pos_suite.diff:1) e o [relato S1–S7, com causas, cura e censo](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/docs/relato_fio_e0_v4_e3_pos_suite.md:7).

Verificação local: [46 testes sem banco verdes](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/docs/e3_pos_suite_vizinhos.txt:2), [selos AST verdes](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/docs/e3_pos_suite_green.txt:12), [mutantes mortos](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/docs/relato_fio_e0_v4_e3_pos_suite.md:35) e [Ruff limpo](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/docs/e3_pos_suite_ruff.txt:1). [nao verificado]

[Comandos de RED, mutantes e labels ORM](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/docs/e3_pos_suite_comandos.txt:1).

ORM, 11 testes X1, suíte inteira e opus final: **[não verificado]**. [nao verificado]

_3672 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
