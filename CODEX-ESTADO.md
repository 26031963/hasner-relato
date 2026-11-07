# CODEX-ESTADO

> Ultima resposta processada: **07/11/2026 17:02** · publicado em **07/11/2026 17:02** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 07/11 17:02 · sessao 93157a80dbed · - · - · origem=Ronald (tmux)

**Item 3 — PROCEDENTE; cura fecha o mutante anterior, não a classe.** A comparação exata de A1/B1 cobre somente `novo.py` sintético, com **duas ocorrências por função**, nos deslocamentos 0/+40. Na medida real, o registro compara identidade sem linhas e só exige linhas não vazias. (`app/core/tests/test_medida_turno_do_par.py:432`, `app/core/tests/test_medida_turno_do_par.py:448`, `app/core/tests/test_medida_turno_do_par.py:454`, `app/core/tests/test_medida_turno_do_par.py:407`, `app/core/tests/test_medida_turno_do_par.py:412`)

Mutante sobrevivente no instrumento, substituindo a publicação em `app/core/medida_turno_do_par.py:351`:

```python
'criterio': criterion,
'linhas': lines if len(lines) > 1 else [node.lineno]
```

A fixture preserva suas duas ocorrências; uma função com uma ocorrência publica o `def`. Exemplos reais: `_alinhar_por_papel` publica 502 em vez do `range` em 520; `_dia_da_batida` publica 47 em vez de `localdate` em 50. (`app/core/tests/test_medida_turno_do_par.py:437`, `app/core/tests/test_medida_turno_do_par.py:441`, `app/escala/utils.py:502`, `app/escala/utils.py:520`, `app/chamados/juizes.py:47`, `app/chamados/juizes.py:50`)

Executei a suite inteira com o runner em memória, `python3 -B`, trocando somente `SimpleTestCase` por `unittest.TestCase`: baseline **22 testes OK**, mutante acima **22 testes OK**, sem arquivos persistidos; resultado desta execução não dispõe de arquivo:linha e deve ser marcado **[nao verificado]** se a exigência de referência for aplicada também à evidência de ferramenta. A infraestrutura Django oficial permanece **[nao verificado]**. [nao verificado]

**d — PROCEDENTE quanto a fatos que a suite não prova; falsidade não demonstrada.** O docstring afirma uma execução histórica “do construtor”, data, duração de 30.656s e quatro resultados externos RED. São declarações documentais; os testes não comprovam autoria, data, duração nem proveniência dessa execução. (`app/core/tests/test_medida_turno_do_par.py:58`, `app/core/tests/test_medida_turno_do_par.py:59`, `app/core/tests/test_medida_turno_do_par.py:60`, `app/core/tests/test_medida_turno_do_par.py:63`)

O runner é fornecido, e a certificação Code/opus e suite oficial são explicitamente `[nao verificado]`; portanto não há ali alegação de certificação oficial. (`app/core/tests/test_medida_turno_do_par.py:18`, `app/core/tests/test_medida_turno_do_par.py:64`)

“MESMO commit” é uma exigência textual: as asserções comparam valores e listas da árvore corrente; não consultam histórico nem demonstram que atualização e código pertencem ao mesmo commit. (`app/core/tests/test_medida_turno_do_par.py:364`, `app/core/tests/test_medida_turno_do_par.py:368`, `app/core/tests/test_medida_turno_do_par.py:407`, `app/core/tests/test_medida_turno_do_par.py:415`)

**f — linhas novas/endurecidas.** `_scan` escreve linhas das ocorrências A1 e B1; `censo_fontes` as publica; o teste real lê apenas presença e a fixture lê igualdade exata. (`app/core/medida_turno_do_par.py:191`, `app/core/medida_turno_do_par.py:214`, `app/core/medida_turno_do_par.py:350`, `app/core/tests/test_medida_turno_do_par.py:412`, `app/core/tests/test_medida_turno_do_par.py:454`)

## 07/11 17:01 · sessao b4fd11f2739c · - · - · origem=Ronald (tmux)

**Item 4: a cura fecha as formas diretas ensaiadas, mas não a classe inteira.** A guarda procura destino `Name/Attribute` chamado `data_jornada` e ocorrência literal `Name('_prev_dj')` na expressão; não acompanha aliases nem associa elementos de desempacotamento. (`app/core/medida_turno_do_par.py:253`, `app/core/medida_turno_do_par.py:256`, `app/core/medida_turno_do_par.py:261`)

- **NAO PROCEDE — falso na cópia atual:** `_gap` retorna `[2408]`; a atribuição real está dentro da comparação reconhecida de `_gap_h`. Isso verifica a fonte, não uma execução produtiva com banco/log. (`app/ponto/motor_calculo_v2.py:2405`, `app/ponto/motor_calculo_v2.py:2407`, `app/ponto/motor_calculo_v2.py:2408`; execução produtiva: [nao verificado])
- **NAO PROCEDE — escape das formas diretas testadas:** tupla, atributo de destino, walrus e augassign contendo `_prev_dj` literal lançam `LookupError`, conforme exigido pelos respectivos casos. (`app/core/tests/test_medida_turno_do_par.py:459`, `app/core/tests/test_medida_turno_do_par.py:462`, `app/core/tests/test_medida_turno_do_par.py:463`, `app/core/tests/test_medida_turno_do_par.py:464`, `app/core/tests/test_medida_turno_do_par.py:465`, `app/core/tests/test_medida_turno_do_par.py:475`)
- **PROCEDENTE — falso estrutural:** `data_jornada, outro = (data_entrada, _prev_dj)` dispara a guarda embora `data_jornada` receba `data_entrada`: qualquer destino compatível é cruzado com qualquer ocorrência de `_prev_dj` no RHS. (`app/core/medida_turno_do_par.py:255`, `app/core/medida_turno_do_par.py:259`, `app/core/medida_turno_do_par.py:261`)
- **PROCEDENTE — escapes restantes:** `data_jornada = self._prev_dj`, `dados['data_jornada'] = _prev_dj` e `anterior = _prev_dj; data_jornada = anterior` retornam `[]`: o primeiro perde o nome literal de origem, o segundo perde o tipo permitido de destino e o terceiro exige propagação inexistente nessa guarda. (`app/core/medida_turno_do_par.py:256`, `app/core/medida_turno_do_par.py:261`, `app/core/medida_turno_do_par.py:282`)

**Mutante sobrevivente do instrumento**, na guarda: (`app/core/medida_turno_do_par.py:278`)

```python
# antes
if inherited - recognized:
# mutante
if len(inherited - recognized) == 1:
```

Executei os **22 métodos**, em memória, com `SimpleTestCase` substituído por `unittest.TestCase`: **0 falhas, 0 erros**; resultado da execução sem artefato persistido: **[nao verificado]**. O runner corresponde à substituição documentada. (`app/core/tests/test_medida_turno_do_par.py:24`)

Com esse mutante, renomear `_gap_h` e duplicar a herança na fixture retorna normalmente **2/2**, retirando a parcela nomeada; resultado em memória: **[nao verificado]**. Os nove subcasos atuais têm exatamente uma atribuição não reconhecida, inclusive aqueles que acrescentam outra atribuição reconhecida; falta um caso com duas desconhecidas. (`app/core/tests/test_medida_turno_do_par.py:458`, `app/core/tests/test_medida_turno_do_par.py:468`, `app/core/tests/test_medida_turno_do_par.py:469`, `app/core/tests/test_medida_turno_do_par.py:475`; publicação da parcela: `app/core/medida_turno_do_par.py:361`, `app/core/medida_turno_do_par.py:363`)

## 07/11 17:01 · sessao 2a612d8fe253 · - · - · origem=Ronald (tmux)

- **2 — cura fecha a classe indicada na triagem:** o teste compara os 47 membros da lista inteira, por identidade e ordem; não compara só tamanho. Troca de um membro mantendo 47 foi rejeitada em memória por `_composicao`. A lista nasce em `app/core/tests/test_medida_turno_do_par.py:79`; igualdade em `app/core/tests/test_medida_turno_do_par.py:368`; aplicação à medida real em `app/core/tests/test_medida_turno_do_par.py:372`.
- **Limite semântico — FILA:** continua existindo exclusão integral de qualquer função alcançada pelo fechamento, inclusive por chamada morta. Congelar os membros detecta mudança da lista; não prova que cada corpo ou uso compartilhado pertence à autoridade. O fechamento está em `app/core/medida_turno_do_par.py:330`; exclusão em `app/core/medida_turno_do_par.py:343`; o teste deliberadamente aceita exclusão transitiva em `app/core/tests/test_medida_turno_do_par.py:597`.
- **b — NAO PROCEDE:** acusação de que troca de identidade com mesmo tamanho não seria cobrada. A igualdade integral cobra isso diretamente. `app/core/tests/test_medida_turno_do_par.py:368`.
- **5b — cura fecha a classe “leitor zero”:** `CLASSIFICAR` agora tem leitor que exige igualdade integral com a publicação calculada. Seu escritor permanece o registro estático em `app/core/tests/test_medida_turno_do_par.py:226`; leitor em `app/core/tests/test_medida_turno_do_par.py:415`. O cálculo é escrito por `difference`/`classify` em `app/core/medida_turno_do_par.py:373` e publicado em `app/core/medida_turno_do_par.py:381`.
- **FILA de nomenclatura:** o docstring chama `CLASSIFICAR` de “sem teto ou catraca”, embora sua igualdade possa reprovar alterações; as linhas seguintes explicam que é cobrança de registro, sem promover candidatos aos contadores. `app/core/tests/test_medida_turno_do_par.py:12`; `app/core/tests/test_medida_turno_do_par.py:14`; `app/core/tests/test_medida_turno_do_par.py:415`.
- **Quem escreve/lê composição:** fechamento escreve `composed` em `app/core/medida_turno_do_par.py:335`; exclusão lê em `app/core/medida_turno_do_par.py:343`; guarda de nomeado absorvido lê em `app/core/medida_turno_do_par.py:358`; publicação escreve `composicao` em `app/core/medida_turno_do_par.py:384`; teste lê em `app/core/tests/test_medida_turno_do_par.py:368`.
- **Quem escreve/lê expectativa:** `COMPOSICAO_REGISTRADA` é escrito em `app/core/tests/test_medida_turno_do_par.py:79` e lido em `app/core/tests/test_medida_turno_do_par.py:368`; o instrumento não importa esse registro, pois seus únicos imports são `ast` e `os`. `app/core/medida_turno_do_par.py:21`.

Mutante do instrumento sobrevivente específico dessas duas curas: **[nao verificado]**. Não executei a suite inteira; executei a prova direta da troca de identidade 47→47. **[nao verificado quanto à suite inteira]** [nao verificado]

## 07/11 16:58 · sessao d959e2a6a2ad · MOTOR-LE-O-TURNO-INSTRUMENTO · construtor · origem=Code

Entrega para certificação, com as quatro curas:

| Cura/teste | Mutante que derruba | [nao verificado]
|---|---|
| [Composição congelada + MORDE](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:423) | Chamada nova sob `if False` no juiz; exige registro no MESMO commit. | [nao verificado]
| [Linhas exatas A1/B1](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:432) | `'linhas': [node.lineno]`, publicando o `def`. |
| [Gap](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:456) | Renomear `_gap_h`; remover a guarda faz o teste falhar. | [nao verificado]
| [Registro CLASSIFICAR](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:414) | Trocar identidade mantendo a quantidade. |

Execução em memória: **22 testes OK; quatro mutantes externos RED, rc=1**. [Registro](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:58) [nao verificado]

[Runner da suite](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:18) e comandos individuais: [composição](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:52), [linhas](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:53), [gap](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:54), [CLASSIFICAR](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:55). [nao verificado]

Herança não reconhecida agora lança `LookupError` com “rever detector”; destino transitivo ignorado está declarado em `NAO_VE`. [Guarda](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/medida_turno_do_par.py:278), [limitação](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/medida_turno_do_par.py:39).

Certificação Code/opus e execução Django oficial: **[nao verificado]**.

## 07/11 16:49 · sessao bc9f57e9f38b · MOTOR-LE-O-TURNO-INSTRUMENTO · verificador · origem=Code

**Há achados PROCEDENTES na exclusão por composição e na cobertura dos testes.** A medição atual reproduz 9/85, mas isso não prova que toda derivação exigida continuará contada após mudanças de forma. (`app/core/medida_turno_do_par.py:324`, `app/core/medida_turno_do_par.py:350`; `app/core/tests/test_medida_turno_do_par.py:31`)

A comparação histórica `ebe80893..c2058690` está **[nao verificado]**: esta cópia não disponibiliza o histórico Git.

1. **Critério literal — contagem atual correta; garantia incompleta.**

   **NAO PROCEDE** a acusação de promover A2/B2 ao contador: a composição dos contadores é exatamente A1 + nomeados e B1 + nomeados; as diferenças amplas são publicadas separadamente. Reproduzi A1=6, A2=15, B1=82, B2=171; nomeados=3; contadores=9/85; diferenças=9/89. (`app/core/medida_turno_do_par.py:348`, `app/core/medida_turno_do_par.py:358`, `app/core/medida_turno_do_par.py:360`)

   Os três nomeados correspondem às chamadas em `app/ponto/motor_calculo_v2.py:503` e `app/ponto/motor_calculo_v2.py:882`, e à herança em `app/ponto/motor_calculo_v2.py:2408`; entram positivamente nos dois contadores. (`app/core/medida_turno_do_par.py:57`, `app/core/medida_turno_do_par.py:345`, `app/core/medida_turno_do_par.py:349`)

   **PROCEDENTE:** existem maneiras de deixar de contar sem remover a derivação: composição expansível, descrita no item 2, e reconhecimento frágil do gap, descrito no item 4. (`app/core/medida_turno_do_par.py:253`, `app/core/medida_turno_do_par.py:324`)

2. **Allowlist e composição — distinguir duas coisas.**

   **NAO PROCEDE** chamar `REGISTRO` ou `SITIOS_NOMEADOS` de allowlist de funções: o primeiro é esperado da suite; o segundo acrescenta parcelas. Nenhum deles dispensa nominalmente uma função produtiva da varredura. (`app/core/tests/test_medida_turno_do_par.py:296`; `app/core/medida_turno_do_par.py:335`, `app/core/medida_turno_do_par.py:349`)

   Existem exclusões nominais de **universo**: diretórios em `_FORA`, diretórios ocultos e arquivos `test_*.py`; elas correspondem ao molde. (`app/core/medida_turno_do_par.py:26`, `app/core/medida_turno_do_par.py:225`, `app/core/medida_turno_do_par.py:227`; `app/core/medida_leitor_do_dia.py:247`, `app/core/medida_leitor_do_dia.py:249`)

   **PROCEDENTE — a composição funciona como isenção estrutural expansível.** Qualquer chamada AST alcançada pelo fechamento inclui sua função em `composed`; depois essa função inteira fica fora dos quatro critérios. Não há verificação de execução, exclusividade de uso ou responsabilidade pela pergunta do juiz. (`app/core/medida_turno_do_par.py:311`, `app/core/medida_turno_do_par.py:318`, `app/core/medida_turno_do_par.py:324`)

   Prova em memória: uma função externa contendo `entrada.date()` conta B1=1; acrescentar ao juiz apenas `if False: leitor(None)` faz B1=0, sem retirar a derivação externa. O algoritmo percorre essa chamada morta e aplica a exclusão integral. (`app/core/medida_turno_do_par.py:65`, `app/core/medida_turno_do_par.py:318`, `app/core/medida_turno_do_par.py:324`)

   Excluir a implementação do próprio juiz é compatível com “fora do juiz”; equiparar **todo chamado transitivo**, inclusive morto e compartilhado, à implementação autorizada é uma extensão do critério. Autorização explícita dessa extensão no aval fornecido: **[nao verificado]**. A suite consagra a exclusão transitiva, mas não demonstra sua legitimidade semântica. (`app/core/tests/test_medida_turno_do_par.py:416`, `app/core/tests/test_medida_turno_do_par.py:429`)

3. **MORDE — há mordida local, mas existe mutante sobrevivente.**

   **NAO PROCEDE** dizer que todos são decorativos: os testes de pareador e dia novos verificam incremento e rejeição por `SUBIU`. (`app/core/tests/test_medida_turno_do_par.py:322`, `app/core/tests/test_medida_turno_do_par.py:325`, `app/core/tests/test_medida_turno_do_par.py:330`, `app/core/tests/test_medida_turno_do_par.py:333`)

   A cobertura efetiva dos 13 `test_MORDE` é esta:

   | Teste | O que efetivamente cobra | [nao verificado]
   |---|---|
   | Candidato amplo | Publicação de novos A2/B2 sem subir os contadores. `app/core/tests/test_medida_turno_do_par.py:310` |
   | Pareador novo | Um `range(..., 2)` novo sobe contador e dispara RED. `app/core/tests/test_medida_turno_do_par.py:318` |
   | Dia novo | Alias de `entrada` seguido de `.date()` sobe contador e dispara RED. `app/core/tests/test_medida_turno_do_par.py:328` |
   | Número curado | Diminui diretamente o resultado; testa `_catraca`, sem curar fonte nem exercitar detector. `app/core/tests/test_medida_turno_do_par.py:336` |
   | Âncora renomeada | Renomear cada função nomeada provoca exceção. `app/core/tests/test_medida_turno_do_par.py:344` |
   | Sintaxe inválida | Exige `SyntaxError` e identificação do arquivo. `app/core/tests/test_medida_turno_do_par.py:353` |
   | Leitura/travessia | Testa erro de `open` e raiz inexistente; não injeta falha intermediária de travessia ou encoding. `app/core/tests/test_medida_turno_do_par.py:358` |
   | Linhas/alias | Desloca fonte e verifica linhas **dos nomeados**; usa os aliases já presentes na fixture. `app/core/tests/test_medida_turno_do_par.py:374` |
   | Cura dos nomeados | Remove corpos, conserva âncoras e exige zero. `app/core/tests/test_medida_turno_do_par.py:383` |
   | Critérios lexicais | Exemplos de `zip`, `append`, `localdate` e `.date()` civil. `app/core/tests/test_medida_turno_do_par.py:394` |
   | Composição | Exige excluir auxiliar chamado e contar função independente do mesmo arquivo. `app/core/tests/test_medida_turno_do_par.py:416` |
   | Aninhamento | Exige atribuir B1 à função interna. `app/core/tests/test_medida_turno_do_par.py:433` |
   | Autoridade ausente | Remove arquivos inteiros e exige exceção. `app/core/tests/test_medida_turno_do_par.py:441` |

   **PROCEDENTE — mutante do instrumento que passa todos:** em `app/core/medida_turno_do_par.py:332`, substituir `'linhas': lines` por `'linhas': [node.lineno]`. Ele publica a linha do `def` em lugar da ocorrência: por exemplo, aponta `app/escala/utils.py:502`, embora o `range` esteja em `app/escala/utils.py:520`. A suite exige apenas linhas não vazias para A1/B1; a comparação precisa de deslocamento cobre os nomeados. (`app/core/tests/test_medida_turno_do_par.py:301`, `app/core/tests/test_medida_turno_do_par.py:380`)

   Esse mutante passou nos **18 métodos**, executados em memória com `SimpleTestCase` substituído por `unittest.TestCase`. A execução pela infraestrutura Django oficial está **[nao verificado]**. (`app/core/tests/test_medida_turno_do_par.py:26`, `app/core/tests/test_medida_turno_do_par.py:257`)

4. **Falha silenciosa — erros de arquivo propagam; detecção incompleta pode retornar normalmente.**

   **NAO PROCEDE** para erros reportados por `os.walk`, leitura/encoding, `SyntaxError`, autoridade ausente, raízes ausentes e funções âncora ausentes: os caminhos propagam exceção ou lançam `LookupError`. (`app/core/medida_turno_do_par.py:221`, `app/core/medida_turno_do_par.py:272`, `app/core/medida_turno_do_par.py:273`, `app/core/medida_turno_do_par.py:290`, `app/core/medida_turno_do_par.py:337`, `app/core/medida_turno_do_par.py:374`)

   **PROCEDENTE para contagem parcial por forma:** renomeei somente `_gap_h` para `_intervalo_h` na fixture, mantendo a mesma herança. O censo retornou normalmente e ambos os contadores caíram de 3 para 2. O detector exige aquele identificador e, sem correspondência, simplesmente não acrescenta o nomeado. (`app/core/medida_turno_do_par.py:253`, `app/core/medida_turno_do_par.py:344`; fixture em `app/core/tests/test_medida_turno_do_par.py:249`)

   A catraca de números atuais acusaria essa queda; portanto o resultado parcial do **instrumento** não equivale a passagem silenciosa pela **suite**. (`app/core/tests/test_medida_turno_do_par.py:269`)

   **FILA:** destinos transitivos sem registro são ignorados, sem distinguir dependência local ausente de chamada externa não resolvida; somente as raízes têm presença exigida. (`app/core/medida_turno_do_par.py:290`, `app/core/medida_turno_do_par.py:314`)

5. **ESMERIL-SEMANTICA — nome ambíguo, sem leitor mostrado usando outra unidade.**

   **NAO PROCEDE** uma divergência demonstrada entre escritor e leitor: `numero` mede parcelas; `itens` contém essas parcelas; `funcoes` projeta suas chaves **com repetição**; `linhas` contém ocorrências AST dentro da parcela. A suite compara essas mesmas relações. (`app/core/medida_turno_do_par.py:348`, `app/core/medida_turno_do_par.py:351`; `app/core/tests/test_medida_turno_do_par.py:294`, `app/core/tests/test_medida_turno_do_par.py:295`)

   **FILA de nomenclatura:** `funcoes` não é conjunto de funções únicas. Uma função B1 que também contém um nomeado aparece duas vezes, multiplicidade declarada pelo instrumento. Nenhum consumidor mostrado interpreta essa lista como conjunto único. (`app/core/medida_turno_do_par.py:10`, `app/core/medida_turno_do_par.py:351`; `app/core/tests/test_medida_turno_do_par.py:295`)

6. **Universo — nenhum outro desvio encontrado frente a `producao`.**

   **NAO PROCEDE:** reproduzi listas idênticas, com 755 arquivos, entre os dois `producao`. As regras de seleção correspondem e a suite compara as listas completas. (`app/core/medida_turno_do_par.py:224`; `app/core/medida_leitor_do_dia.py:246`; `app/core/tests/test_medida_turno_do_par.py:449`)

   `e6_piso_autopsia` não é excluído por `producao` do molde: sua exclusão pertence ao estágio `NAO_E_LEITOR` daquele outro detector. Portanto sua inclusão aqui não constitui desvio de `producao`. (`app/core/medida_leitor_do_dia.py:240`, `app/core/medida_leitor_do_dia.py:282`; `app/core/tests/test_medida_turno_do_par.py:452`)

   A atribuição histórica da diferença 83→82 exclusivamente a `bin/` consta da documentação da obra; recomposição independente do censo antigo: **[nao verificado]**. (`app/core/medida_turno_do_par.py:5`, `app/core/tests/test_medida_turno_do_par.py:4`)

7. **Juiz novo / segundo escritor / relógio / derivação paralela.**

   **NAO PROCEDE nos dois arquivos examinados:** o instrumento importa `ast`/`os`, lê fontes e devolve estruturas em memória; não escreve dados de negócio nem consulta relógio. (`app/core/medida_turno_do_par.py:21`, `app/core/medida_turno_do_par.py:348`, `app/core/medida_turno_do_par.py:374`)

   **FILA de negócio:** as portas do motor e a herança continuam presentes, agora medidas como RED nomeado. A receita `turnos_de_batidas` copia batidas, prepara parâmetros e chama o pareador; o gap continua escrevendo `data_jornada`. (`app/ponto/turnos.py:1515`, `app/ponto/turnos.py:1533`; `app/ponto/motor_calculo_v2.py:2408`; `app/core/medida_turno_do_par.py:57`)

8. **QUEM ESCREVE / QUEM LÊ os valores públicos e insumos.**

   A autoridade de negócio vem do registro vivo; `JUIZES.md` se declara lookup. O instrumento lê esse registro, não o Markdown. (`app/docs/JUIZES.md:7`, `app/docs/JUIZES.md:14`; `app/core/juizes.py:196`; `app/core/medida_turno_do_par.py:232`)

   | Valor | Quem escreve/fornece | Quem lê |
   |---|---|---|
   | Fontes e árvores AST | Leitura em `app/core/medida_turno_do_par.py:374`; parse em `app/core/medida_turno_do_par.py:272` | Indexação em `app/core/medida_turno_do_par.py:277` |
   | Autoridade | `JUIZES`, `app/core/juizes.py:196` | `_autoridade`, `app/core/medida_turno_do_par.py:232`; raízes em `app/core/medida_turno_do_par.py:275` |
   | Composição | Fechamento em `app/core/medida_turno_do_par.py:311`; publicação em `app/core/medida_turno_do_par.py:365` | Exclusão em `app/core/medida_turno_do_par.py:324`; teste em `app/core/tests/test_medida_turno_do_par.py:429` |
   | `chave`, `criterio`, `linhas` lexicais | `app/core/medida_turno_do_par.py:331` | Montagem em `app/core/medida_turno_do_par.py:349`; validação em `app/core/tests/test_medida_turno_do_par.py:296` |
   | `sitio`, nomeados e suas `linhas` | `app/core/medida_turno_do_par.py:342` | Ambos os contadores em `app/core/medida_turno_do_par.py:349`; testes em `app/core/tests/test_medida_turno_do_par.py:365` |
   | `numero` | `len(items)`, `app/core/medida_turno_do_par.py:350` | `_catraca`, `app/core/tests/test_medida_turno_do_par.py:266` |
   | `itens` | Lexicais + nomeados, `app/core/medida_turno_do_par.py:349` | Consistência/identidade em `app/core/tests/test_medida_turno_do_par.py:294` |
   | `funcoes` | Projeção com multiplicidade, `app/core/medida_turno_do_par.py:351` | Comparação em `app/core/tests/test_medida_turno_do_par.py:295` |
   | `a_classificar_pela_regra` | Diferença de chaves em `app/core/medida_turno_do_par.py:354` | Testes em `app/core/tests/test_medida_turno_do_par.py:303` |
   | `criterios` | Tamanhos das listas, `app/core/medida_turno_do_par.py:363` | Testes em `app/core/tests/test_medida_turno_do_par.py:288` |
   | `universo_arquivos`, `ilegiveis` | `len(trees)` e constante `[]`, `app/core/medida_turno_do_par.py:366` | Testes em `app/core/tests/test_medida_turno_do_par.py:290` |
   | `nao_ve` | Constante em `app/core/medida_turno_do_par.py:31` | Comparação em `app/core/tests/test_medida_turno_do_par.py:299` |
   | `esperado_no_fim` | Constante zero em `app/core/medida_turno_do_par.py:352` | Teste em `app/core/tests/test_medida_turno_do_par.py:300`; a catraca atual lê outro teto em `app/core/tests/test_medida_turno_do_par.py:265` |
   | Rótulo `contador` | `app/core/medida_turno_do_par.py:350`, `app/core/medida_turno_do_par.py:360` | Consumidor externo **[nao verificado]** |
   | Esperados numéricos e `REGISTRO` | `app/core/tests/test_medida_turno_do_par.py:31` | Cat­raca e identidade em `app/core/tests/test_medida_turno_do_par.py:265`, `app/core/tests/test_medida_turno_do_par.py:296` |
   | `CLASSIFICAR` estático | `app/core/tests/test_medida_turno_do_par.py:131` | Nenhum leitor localizado; os testes leem a classificação calculada em `app/core/tests/test_medida_turno_do_par.py:305` |

   Publicação efetiva desses valores em placar/HOST fora da suite: **[nao verificado]**. [nao verificado]

## 07/11 16:47 · sessao 2bf3f03528c4 · - · - · origem=Ronald (tmux)

Achados consolidados, somente leitura:

- **PROCEDENTE — exclusão estrutural por composição pode baixar contador sem curar derivação.** O fechamento percorre toda chamada AST, inclusive sob `if False`, e exclui inteiramente a função alcançada da medição: `app/core/medida_turno_do_par.py:311`, `:318`, `:324`. Prova em memória: `externo.py::leitor(entrada): return entrada.date()` contou B1=1; acrescentar somente `if False: leitor(None)` ao juiz produziu B1=0, mantendo a derivação. A insensibilidade a ramos está declarada em `:34`. O teste explicitamente consagra a exclusão transitiva de um auxiliar: `app/core/tests/test_medida_turno_do_par.py:416`, `:429`, `:430`. Autorização literal do aval para fechamento transitivo irrestrito: **[não verificado]**.

- **NÃO PROCEDE — outra allowlist nominal encontrada.** Os nomes positivos dos três sítios adicionam parcelas, não dispensam funções: `app/core/medida_turno_do_par.py:57`, `:335`, `:345`, `:349`. A exclusão de funções ocorre por composição em `:324`. As exclusões de diretórios e prefixo `test_` pertencem ao universo: `:26`, `:225`, `:227`.

- **NÃO PROCEDE — desvio adicional frente a `producao` do molde nesta cópia.** Execução em memória encontrou listas idênticas, com 755 arquivos. As regras correspondem: novo em `app/core/medida_turno_do_par.py:224`, `:225`, `:227`; molde em `app/core/medida_leitor_do_dia.py:246`, `:247`, `:249`. O teste compara as listas completas: `app/core/tests/test_medida_turno_do_par.py:449`. A exclusão de `e6_piso_autopsia` do molde pertence a outro estágio (`NAO_E_LEITOR`), não à sua `producao`: `app/core/medida_leitor_do_dia.py:278`, `:282`, `:377`.

- **NÃO PROCEDE — leitura, encoding, SyntaxError ou raízes ausentes virarem zero silencioso.** `os.walk` recebe callback que relança erro (`app/core/medida_turno_do_par.py:221`, `:224`); leitura UTF-8-SIG não captura exceção (`:374`); `ast.parse` também não captura (`:272`); autoridade, raízes e âncoras faltantes geram `LookupError` (`:273`, `:290`, `:337`). Os testes cobrem leitura, raiz inexistente, SyntaxError e autoridade/composição ausente: `app/core/tests/test_medida_turno_do_par.py:353`, `:358`, `:441`.

- **FILA — composição incompleta pode ser apresentada normalmente.** Destino de chamada sem registro é descartado silenciosamente: `app/core/medida_turno_do_par.py:314`. Prova em memória: `parear_turnos(): return sumida()` manteve resultado normal. Portanto exige as duas raízes, não todas as dependências transitivas: `:290`. Chamadas não resolvidas constam entre limitações declaradas em `:39`; integridade completa do grafo não é comprovada.

- Medição reproduzida: A1=6, A2=15, B1=82, B2=171; contadores 9/85, pelos produtores em `app/core/medida_turno_do_par.py:349`, `:350`, `:363`. Parcelas nomeadas apontam para `app/ponto/motor_calculo_v2.py:503`, `:882`, `:2408`. Os únicos consumidores localizados do novo instrumento são os testes, incluindo leitura dos campos em `app/core/tests/test_medida_turno_do_par.py:286`, `:303`, `:365`; integração externa/HOST publicada: **[não verificado]**.

## 07/11 16:47 · sessao 75130e94b46d · - · - · origem=Ronald (tmux)

Achados para consolidação, somente pela cópia:

- **PROCEDENTE: composição pode isentar código que não executa no juiz.** O fechamento percorre qualquer `ast.Call`, sem avaliar alcance, e exclui integralmente toda função alcançada (`app/core/medida_turno_do_par.py:311-325`). Confirmei em memória com `_mini` (`app/core/tests/test_medida_turno_do_par.py:235`): uma função `novo(entrada,xs)` com `range(0,len(xs),2)` e `entrada.date()` produz A1/A2/B1/B2=1/1/1/1; adicionar `from novo import novo; if False: novo()` a `turnos_do_colab` faz tudo virar zero. A suite exige a exclusão transitiva de helper, mas não testa alcance/semântica (`app/core/tests/test_medida_turno_do_par.py:416-431`). O aval colado não explicita autorização desse fechamento irrestrito [não verificado].

- **NÃO PROCEDE, para os dois arquivos novos: juiz de negócio, segundo escritor persistente ou relógio novo.** Instrumento importa apenas `ast`/`os` (`app/core/medida_turno_do_par.py:21-22`), lê arquivos (`:374-375`) e devolve dicionários em memória (`:348-367`). O teste restringe imports a esses dois módulos (`app/core/tests/test_medida_turno_do_par.py:454-464`). A comparação histórica do diff continua [não verificado], sem `.git`.

- **FILA: derivações paralelas já existentes são os RED nomeados.** `turnos_via_autoridade` chama `parear_turnos` com parâmetros próprios (`app/ponto/motor_calculo_v2.py:503-506`); `_turnos_do_juiz` chama `turnos_de_batidas` (`:882`), cuja receita independente copia batidas, marca ecos e chama o pareador (`app/ponto/turnos.py:1515-1533`). O gap escreve `data_jornada = _prev_dj` (`app/ponto/motor_calculo_v2.py:2405-2409`); essa data governa inclusão na competência (`:2429-2432`). São os três itens positivos declarados pelo instrumento (`app/core/medida_turno_do_par.py:57-60`).

- **Autoridade:** o registro vivo atribui a pergunta do turno da batida a `ponto/turnos.py::turnos_do_colab` (`app/core/juizes.py:195-196`). `JUIZES.md` se declara lookup e remete ao vivo (`app/docs/JUIZES.md:7-15`); lista geometria em `turnos.py` (`:20-25`) e cálculo no motor (`:29`). O escritor de `Turno.data_turno` usa `_data_do_turno` e constrói `Turno` (`app/ponto/turnos.py:310-322`).

Inventário de saída/consumo:

| Valor | Quem escreve | Quem lê |
|---|---|---|
| fontes / árvores | `censo` lê fonte, instrumento `:373-375`; parse `:272` | `censo_fontes`, instrumento `:277-289` |
| autoridade | `JUIZES`, `app/core/juizes.py:195-196` | `_autoridade`, instrumento `:232-244`, raiz `:275` |
| `criterio`, `chave`, `linhas` lexicais | instrumento `:329-332` | montagem `:349-351`; testes `:296-301`, `:307-308` | [nao verificado]
| `sitio`, itens nomeados, `linhas` | instrumento `:335-346` | ambos contadores `:349`; testes `:365-381` | [nao verificado]
| `numero` | `len(itens)`, instrumento `:350` | catraca testes `:264-270`; consistência `:294` | [nao verificado]
| `itens` | lexical + nomeados, instrumento `:349-350` | testes `:294-308` | [nao verificado]
| `funcoes` | lista de chaves com multiplicidade, instrumento `:351` | testes `:295`, `:324`, `:332`, `:379`, `:430-431` | [nao verificado]
| classificação | diferença por chave, instrumento `:354-358`, `:362` | testes `:303-316`, `:411-412` | [nao verificado]
| `criterios` | tamanho das listas, instrumento `:363` | testes `:288-289`, `:305`, `:410` | [nao verificado]
| `composicao` | instrumento `:365` | teste `:429`; exclusão interna usa conjunto `:324-325` | [nao verificado]
| `universo_arquivos` | `len(trees)`, instrumento `:366` | teste só exige `>100`, `:291` | [nao verificado]
| `ilegiveis` | constante `[]`, instrumento `:366` | teste `:290` | [nao verificado]
| `nao_ve` | constante instrumento `:31-53`, saída `:351`, `:366` | teste compara saída interna `:299`; leitor externo [não verificado] | [nao verificado]
| `esperado_no_fim` | constante 0, instrumento `:352` | teste `:300`; não é teto executado pela catraca `:264-270` | [nao verificado]
| `contador` | rótulos instrumento `:350`, `:360-361` | leitor externo [não verificado] |
| registro esperado | testes `:31-129` | testes `:264-270`, `:288-298` | [nao verificado]
| `CLASSIFICAR` | testes `:131-228` | nenhuma referência adicional localizada na cópia [não verificado] | [nao verificado]

`funcoes` tem significado de parcelas, não conjunto de funções únicas: a documentação declara multiplicidade (`app/core/medida_turno_do_par.py:10-13`) e o leitor compara essa mesma lista (`app/core/tests/test_medida_turno_do_par.py:295`). `linhas` mede sítios internos, enquanto `numero` mede funções por critério mais sítios nomeados (`app/core/medida_turno_do_par.py:48-49`, `:349-351`); não há consumidor mostrado confundindo essas unidades.

## 07/11 16:47 · sessao 321d2aab3083 · - · - · origem=Ronald (tmux)

**PROCEDENTE — há mutante sobrevivente na proveniência.** Em `app/core/medida_turno_do_par.py:332`, trocar:

```python
'criterio': criterion, 'linhas': lines
```

por:

```python
'criterio': criterion, 'linhas': [node.lineno]
```

mantém os totais e identidades, mas publica a linha da definição em lugar das ocorrências A1/B1. Exemplo: `_alinhar_por_papel` passa a apontar `app/escala/utils.py:502`, embora o `range(..., 2)` esteja em `app/escala/utils.py:520`. O teste exige somente lista não vazia (`app/core/tests/test_medida_turno_do_par.py:301`); a verificação de deslocamento das linhas cobre apenas os nomeados (`app/core/tests/test_medida_turno_do_par.py:380`).

Executei todos os **18 métodos** com esse mutante em memória: **18 passaram, zero falhas/erros**. Como Django está ausente, substituí somente `django.test.SimpleTestCase` por `unittest.TestCase`; execução pela infraestrutura Django oficial **[nao verificado]**. [nao verificado]

Os 13 `test_MORDE` têm estas mordidas delimitadas, sem provar completude do instrumento:

| Teste, linha no arquivo de testes | O que efetivamente cobra | [nao verificado]
|---|---|
| `:310` candidato amplo | Novos A2/B2 publicados e totais preservados (`:314–316`). | [nao verificado]
| `:318` pareador novo | Um `range` novo aumenta contador e provoca `SUBIU` (`:322–326`). |
| `:328` dia novo | `copia = entrada; copia.date()` aumenta contador e provoca `SUBIU` (`:330–334`). |
| `:336` número curado | **Altera diretamente o resultado**, diminuindo `numero`; cobra `_catraca`, sem curar fonte nem exercitar detector (`:339–342`). |
| `:344` renomear âncora | Renomeia cada função nomeada e exige `LookupError` (`:349–351`). |
| `:353` sintaxe inválida | Cobra `SyntaxError` e nome do arquivo (`:354–356`). |
| `:358` leitura/travessia | Cobra erro de `open` e raiz inexistente; não injeta `UnicodeDecodeError` nem erro no meio de `os.walk` (`:359–363`). |
| `:374` linhas/alias | Usa aliases já presentes no `_mini`; desloca fonte e verifica linhas **nomeadas** (`:375–381`; aliases em `:243`, `:247`). |
| `:383` cura dos nomeados | Preserva âncoras, remove seus corpos e exige zero (`:385–392`). |
| `:394` critérios lexicais | Cobre um `zip`, um `append`, um `localdate` e um `.date()` civil (`:397–414`). |
| `:416` composição | Cobra helper importado excluído e função independente do mesmo arquivo contada (`:419–431`). |
| `:433` aninhamento | Cobra que a função interna responda pelo B1 (`:435–439`). |
| `:441` autoridade ausente | Remove dois arquivos inteiros e exige erro (`:442–447`). |

**NÃO PROCEDE** dizer que os selos são inteiramente decorativos: há incremento e rejeição explícita em `app/core/tests/test_medida_turno_do_par.py:322` e `:325`, assim como em `:330` e `:333`. **PROCEDENTE** dizer que não asseguram a fidelidade das linhas publicadas, pelo sobrevivente acima (`app/core/medida_turno_do_par.py:332`; testes `:301`, `:380`).

## 07/11 16:34 · sessao 076a0a8f57fe · FIO-E0-V4-E2 · verificador · origem=Code

**Há dois bloqueios para a equivalência universal:** o delta passa a compartilhar uma Empresa mutável entre FKs e antecipa leituras que N chamadas unitárias podem dispensar. São consequências do código; incidência em produção e resultado da suíte completa: [nao verificado]. Referências: [juizes.py:977](app/chamados/juizes.py:977), [juizes.py:997](app/chamados/juizes.py:997).

1. **Empresa compartilhada no cache das FKs — bloqueante, introduzido no delta.**

   `in_bulk` fornece um objeto por PK, e `set_cached_value` instala **esse mesmo objeto** em cada colaborador frio daquela empresa, fora do request compartilhado. Portanto, depois do lote, dois colaboradores distintos podem ter `a.empresa is b.empresa`. Alterar somente `a.empresa.dia_inicio_competencia` também altera o que `b.empresa` responde. N chamadas unitárias com instâncias frias independentes não compartilham esse objeto entre chamadas. O próprio teste novo considera legítima a alteração do corte somente em memória. [juizes.py:987](app/chamados/juizes.py:987), [juizes.py:997](app/chamados/juizes.py:997), [test_e2_tarefa_do_dia.py:1224](app/chamados/tests/test_e2_tarefa_do_dia.py:1224).

   **Quem escreve:** o juiz escreve o cache da FK em `997` e o memo em `995`, `998` e `1000`. **Quem lê depois:** o teste lê diretamente `c.empresa`; existe também leitor de produção após `pergunta_viva`, em `vigia_fio`. Esse leitor de produção já usa `select_related`, portanto sua existência não prova incidência desse defeito ali. [juizes.py:995](app/chamados/juizes.py:995), [test_e2_tarefa_do_dia.py:1252](app/chamados/tests/test_e2_tarefa_do_dia.py:1252), [vigia_fio.py:807](app/chamados/services/vigia_fio.py:807), [vigia_fio.py:813](app/chamados/services/vigia_fio.py:813).

2. **Leitura antecipada quebra lote = N unitárias — bloqueante para a promessa “toda combinação”.**

   Contraexemplo com request inicialmente vazio: primeiro par com objeto não-modelo que possui `empresa`; segundo par com outro objeto não-modelo, mesmo `empresa_id`, mas sem atributo `empresa`. O lote lê ambos antes de preencher o memo e lança `AttributeError` no segundo. Nas unitárias, a primeira preenche o memo do request; a segunda encontra a chave e dispensa a leitura do atributo, podendo responder normalmente. Isso foi reproduzido em memória com as funções extraídas e dependências simuladas; execução com ORM real: [nao verificado]. O caminho está em [juizes.py:943](app/chamados/juizes.py:943), [juizes.py:977](app/chamados/juizes.py:977), [juizes.py:993](app/chamados/juizes.py:993), [juizes.py:1038](app/chamados/juizes.py:1038).

   Para objetos ordinários, atributos completos e estado estável, o delta resolve os casos apresentados de FK quente, FK fria e `empresa=None`; isso não estabelece equivalência universal de exceções nem dos objetos deixados nos caches. [juizes.py:942](app/chamados/juizes.py:942), [juizes.py:994](app/chamados/juizes.py:994).

3. **A Empresa efetiva tem precedência de memo — limite residual, não regressão nova de v2.**

   A seleção efetiva por par é esta. [juizes.py:978](app/chamados/juizes.py:978), [juizes.py:993](app/chamados/juizes.py:993):

   | Situação | Empresa que decide a janela |
   |---|---|
   | Chave já no memo | A Empresa do memo, mesmo que a FK carregue outra |
   | Sem memo, FK quente | A Empresa daquela FK |
   | Sem memo, não-modelo | Seu atributo `empresa`, inclusive `None` |
   | Sem memo, FK fria encontrada | O objeto retornado pelo `in_bulk` |
   | PK ausente do `in_bulk` | A leitura direta `c.empresa`, inclusive sua exceção |

   Referências dos ramos: [juizes.py:942](app/chamados/juizes.py:942), [juizes.py:943](app/chamados/juizes.py:943), [juizes.py:995](app/chamados/juizes.py:995), [juizes.py:997](app/chamados/juizes.py:997), [juizes.py:1000](app/chamados/juizes.py:1000).

   No request, o memo pertence ao request e a chave de Empresa contém somente `empresa_id`. Assim, dois colaboradores com cortes em memória 21/22 podem ser julgados pelo primeiro corte. **As unitárias no mesmo request também fazem isso**: não seria correto apresentar esse caso, sozinho, como divergência lote/unitárias. Empresas com IDs diferentes têm chaves distintas. [juizes.py:992](app/chamados/juizes.py:992), [juizes.py:1038](app/chamados/juizes.py:1038).

   Em `CargaDaTarefa`, os pares usam os colaboradores recarregados em `_cadastro`; a Empresa quente do objeto entregue pelo chamador não é necessariamente a usada pelo juiz, e o aquecimento ocorre nessa instância interna. [tarefa_do_dia.py:317](app/chamados/services/tarefa_do_dia.py:317), [tarefa_do_dia.py:366](app/chamados/services/tarefa_do_dia.py:366).

4. **Queries: constância condicionada — não é bloqueio novo por si só.**

   O desenho faz uma chamada `in_bulk`, uma consulta por chave nova **empresa × competência**, mais as etapas em lote do toque. Portanto, não é constante para qualquer crescimento de colaboradores/dias: novas empresas ou competências aumentam as consultas de tranca. Os testes comparam uma empresa e dias da mesma competência. [juizes.py:987](app/chamados/juizes.py:987), [juizes.py:1002](app/chamados/juizes.py:1002), [fechamento.py:1463](app/ponto/services/fechamento.py:1463), [test_e2_tarefa_do_dia.py:1146](app/chamados/tests/test_e2_tarefa_do_dia.py:1146).

   **Há caminhos de acesso individual:** se o `in_bulk` omitir a Empresa e a leitura direta conseguir resolvê-la, `1000` pode consultar por colaborador; se a FK estiver realmente quebrada, a chamada falha no primeiro acesso correspondente. Além disso, no request compartilhado, depois que o primeiro colaborador preenche o memo, os demais pulam `set_cached_value`: leituras posteriores dessas FKs podem custar uma consulta por colaborador ainda frio. Não atribuo esse N+1 à execução de `.tarefa()` sem demonstrar tal leitura nela. [juizes.py:993](app/chamados/juizes.py:993), [juizes.py:997](app/chamados/juizes.py:997), [juizes.py:1000](app/chamados/juizes.py:1000).

5. **Testes novos: não identifiquei vácuo — não bloqueante; cobertura incompleta.** [nao verificado]

   O teste dos cortes exige explicitamente `[True, False]`; o não-modelo exige ausência de SQL de Empresa; o aquecimento usa colaborador recém-buscado e exige zero consultas no acesso seguinte. Eles verificam efeitos relevantes, mas não cobrem compartilhamento entre duas FKs frias nem o memo compartilhado de request. [test_e2_tarefa_do_dia.py:1230](app/chamados/tests/test_e2_tarefa_do_dia.py:1230), [test_e2_tarefa_do_dia.py:1239](app/chamados/tests/test_e2_tarefa_do_dia.py:1239), [test_e2_tarefa_do_dia.py:1249](app/chamados/tests/test_e2_tarefa_do_dia.py:1249).

   Nos mutantes novos do Caso10, o detector encontra a infração no próprio mutante: `getattr` pela regra de chamada e `**dict` pela chave constante. A verificação AST isolada encontrou a fonte-base limpa e ambos os mutantes detectados; portanto, eles não passam apoiados numa infração prévia. [test_e2_tarefa_do_dia.py:1278](app/chamados/tests/test_e2_tarefa_do_dia.py:1278), [test_e2_tarefa_do_dia.py:1282](app/chamados/tests/test_e2_tarefa_do_dia.py:1282), [test_e2_tarefa_do_dia.py:1311](app/chamados/tests/test_e2_tarefa_do_dia.py:1311).

6. **Os três descartados — nenhum demonstra bloqueio novo do delta, mas duas justificativas precisam de ressalva.**

   - **Linhas de “Cobrar” versus `exists()`:** mesma contagem de consultas não significa mesmo volume. Não há `LIMIT`; o SQL usa o menor `desde` do lote e pode trazer outros dias e linhas anteriores à tranca específica de um item. A comparação individual preserva o resultado, mas a justificativa de volume estritamente limitado é excessiva. Impacto real de volume: [nao verificado]. [juizes.py:1151](app/chamados/juizes.py:1151), [juizes.py:1159](app/chamados/juizes.py:1159), [juizes.py:1166](app/chamados/juizes.py:1166).
   - **`self._trancado` como retrato da carga:** o descarte é coerente com a materialização na construção. Porém, “nunca entre requests” não é imposto pelo código: a reutilização verifica igualdade de `agora`, sem vínculo com request ou atualização da tranca. [tarefa_do_dia.py:367](app/chamados/services/tarefa_do_dia.py:367), [tarefa_do_dia.py:402](app/chamados/services/tarefa_do_dia.py:402), [tarefa_do_dia.py:420](app/chamados/services/tarefa_do_dia.py:420).
   - **Trancado sem célula:** descarte sustentado pelo código. A tranca retorna sem ações antes do ramo `sem_celula`, conforme a regra registrada no próprio teste. [tarefa_do_dia.py:187](app/chamados/services/tarefa_do_dia.py:187), [tarefa_do_dia.py:196](app/chamados/services/tarefa_do_dia.py:196), [test_e2_tarefa_do_dia.py:1048](app/chamados/tests/test_e2_tarefa_do_dia.py:1048).

## 07/11 16:32 · sessao bffeb73bbc60 · MOTOR-LE-O-TURNO-INSTRUMENTO · construtor · origem=Code

(a) Escritos:

- [Instrumento puro AST](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/medida_turno_do_par.py:1).
- [Selo SimpleTestCase e inventários](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:1).

(b) Medição registrada nesta cópia:

| Contador | Número | Lista integral `arquivo::funcao` |
|---|---:|---|
| Pareadores | **9 = A1(6) + 3 nomeados** | [REGISTRO](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:33) |
| Derivações de dia | **85 = B1(82) + 3 nomeados** | [REGISTRO](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:42) |

“A classificar pela regra”: **A2−A1=9; B2−B1=89**, com [listas integrais publicadas](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:131). Candidatos novos dessas listas não travam a catraca ([caso explícito](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:310)). [nao verificado]

Referência fornecida `ffbda473`: **6/15/83/172**; cópia medida no universo `app/`: **6/15/82/171**. A diferença corresponde ao recorte que exclui `bin/`; a base informada está registrada no [instrumento](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/medida_turno_do_par.py:4). HEAD independente: [nao verificado].

(c) [`NAO_VE` integral](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/medida_turno_do_par.py:31), publicado junto de cada número: homônimos e ausência de tipos; fluxo e origem interprocedural; chamadas dinâmicas, `getattr` e aliases; limites da composição estática; código fora do universo; formas sintáticas não reconhecidas; novos sítios dentro de função já contada; reescritas dos sítios nomeados. [nao verificado]

(d) Localização por função AST:

- `turnos_via_autoridade`: chamada resolvida para `ponto.turnos.parear_turnos`.
- `MotorBase._turnos_do_juiz`: chamada resolvida para `ponto.turnos.turnos_de_batidas`.
- `MotorBase.calcular_mes`: atribuição `data_jornada = _prev_dj` sob comparação de `_gap_h`.

As [âncoras](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/medida_turno_do_par.py:57) não dependem de linhas. Renomear uma função âncora provoca [erro explícito](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/medida_turno_do_par.py:334).

(e) Executei **18 casos no HOST**, substituindo `SimpleTestCase` por `unittest.TestCase` somente em memória, sem falhas. Desativar A1, B1, proteção das âncoras ou erro de parse produziu RED esperado. Certificação pela suite Django: **[nao verificado]**; a tentativa parou por ausência de `django`. [nao verificado]

(f) Comando da suite e comandos individuais dos quatro mutantes exigidos ([casos](/home/ronald/wt-codex-motor-le-o-turno-instrumento-162203/app/core/tests/test_medida_turno_do_par.py:318)): [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B manage.py test core.tests.test_medida_turno_do_par

turno_selo=core.tests.test_medida_turno_do_par.CatracaTurnoDoParTest

PYTHONDONTWRITEBYTECODE=1 python3 -B manage.py test "$turno_selo.test_MORDE_pareador_novo_sobe_contador_1_e_catraca_fica_RED"

PYTHONDONTWRITEBYTECODE=1 python3 -B manage.py test "$turno_selo.test_MORDE_dia_novo_sobe_contador_2_e_catraca_fica_RED"

PYTHONDONTWRITEBYTECODE=1 python3 -B manage.py test "$turno_selo.test_MORDE_renomear_cada_funcao_nomeada_falha_explicitamente"

PYTHONDONTWRITEBYTECODE=1 python3 -B manage.py test "$turno_selo.test_MORDE_arquivo_invalido_e_RED_mesmo_fora_do_escopo_do_motor"
```

## 07/11 16:20 · sessao 504cc4b15e21 · FIO-E0-V4-E1 · verificador · origem=Code

**Veredito: a cura permanece parcial. PROCEDENTES as classes (1), (2) e (4); NAO PROCEDE a permanência dos defeitos enumerados nas classes (3) e (5). Persistem falhas de alcance dos selos e uma cegueira nova no I22.** (`app/chamados/services/fio.py:1081`; `app/chamados/models.py:2019`; `app/chamados/juizes.py:932`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:202`; `app/chamados/services/fio.py:1172`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1046`)

Identidade independente do commit/pai, ocorrência dos contraexemplos em produção, reprodução em banco, RED→GREEN histórico e suíte inteira `rc=0`: **[nao verificado]**. [nao verificado]

**1. Classe (1) — PROCEDENTE: as portas ainda podem escolher canônicas diferentes, por divergência de universo.**

O contraexemplo anterior do horizonte foi corrigido: `aplicar` escolhe a canônica antes de recortar as perdedoras que pode escrever. **NAO PROCEDE sua permanência nessa forma.** (`app/chamados/management/commands/backfill_decisoes_pergunta.py:226`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:227`)

Entretanto, `universo_do_grupo` não é idempotente. Contraexemplo estático: mesmo colaborador, motivo e dia no horizonte; `L` sem episódio, muda e menor PK; `A` do episódio α, muda; `B` do episódio β, respondida. `do_fato(α)` entrega `[L,A]`, e `abrir(α)` segura `L`. Episódios distintos são fatos distintos. (`app/chamados/models.py:2565`; `app/chamados/models.py:2567`; `app/chamados/models.py:2691`; `app/chamados/models.py:2692`; `app/chamados/services/fio.py:1240`)

O agrupador também entrega `[L,A]`, mas `aplicar` expande esse grupo novamente. Ao visitar `L`, sem episódio, `universo_do_grupo` chama o ramo de `do_fato` por dia, que inclui `B`. O apply passa a escolher `B` e retrata `A`: **a expansão de uma linha legada uniu episódios diferentes**. (`app/chamados/services/fio.py:1120`; `app/chamados/services/fio.py:1126`; `app/chamados/services/fio.py:1079`; `app/chamados/services/fio.py:1081`; `app/chamados/models.py:2571`; `app/chamados/models.py:2598`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:221`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:265`)

Assim, DRY e apply podem divergir; abrir e migração usam diretamente `do_fato`, enquanto backfill/dedup aplicam a expansão adicional. **NAO PROCEDE segundo implementador da preferência resposta/menor PK localizado; PROCEDENTE derivação paralela do universo entregue ao juiz.** (`app/chamados/management/commands/backfill_decisoes_pergunta.py:359`; `app/chamados/management/commands/dedup_perguntas_fato.py:70`; `app/chamados/services/fio.py:1248`; `app/chamados/services/fio.py:1255`; `app/chamados/services/fio.py:984`)

**2. Classe (2) — PROCEDENTE pelas ordens residuais; NAO PROCEDE outro `select_for_update` explícito de produção localizado para as quatro entidades.**

As quatro aquisições explícitas estão em `trava_na_ordem`. Os dois caminhos apontados na volta anterior receberam antecipação das travas. (`app/chamados/models.py:2513`; `app/chamados/models.py:2520`; `app/chamados/models.py:2533`; `app/chamados/models.py:2537`; `app/chamados/models.py:290`; `app/chamados/services/materializacao.py:676`)

Persistem, porém:

- **Pergunta → Disputa:** `remover_motivo` abre atomic, carimba a Pergunta — com `save` real — e depois salva a Disputa, sem antecipar a ordem. Os receivers de reativação de Posto e calibração de âncora chegam diretamente a esse método. (`app/chamados/models.py:2011`; `app/chamados/models.py:2019`; `app/chamados/models.py:2911`; `app/chamados/models.py:2023`; `app/colaboradores/signals.py:71`; `app/colaboradores/signals.py:77`; `app/escala/signals.py:155`; `app/escala/signals.py:161`)
- **Chamado → Disputa por chamadas sucessivas ao próprio dono:** a aprovação de justificativa trava Colaborador e Chamados sem incluir as Disputas abertas. Depois, o fechamento da origem chama `retratar`, chega ao reconciliar e adquire as Disputas abertas. Um Chamado elegível ao fechamento e outra Disputa aberta da mesma pessoa bastam para alcançar essa ordem. (`app/ponto/services/justificativa.py:123`; `app/ponto/services/justificativa.py:134`; `app/ponto/services/justificativa.py:211`; `app/ponto/services/justificativa.py:39`; `app/ponto/services/justificativa.py:44`; `app/chamados/reconciliador.py:523`; `app/chamados/models.py:655`; `app/chamados/models.py:694`)

Portanto, centralizar a aquisição não garante a ordem global entre chamadas: uma chamada posterior pode adquirir linhas de tabela anterior ainda não travadas. (`app/ponto/services/justificativa.py:123`; `app/chamados/models.py:694`)

**3. Classe (3) — NAO PROCEDE a permanência dos defeitos anteriores enumerados.**

O atomic engloba o processamento; as travas precedem a conferência do horizonte empresarial, mérito e exportada. `_ent` agora é reiniciado por colaborador. O horizonte da escrita dos grupos e o veto dos `sem_ato` também são lidos nesse envelope. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:202`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:208`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:211`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:215`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:218`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:227`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:233`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:239`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:252`)

**NAO PROCEDE mistura de escrita persistida e recusa no retorno normal:** qualquer recusa levanta exceção dentro do atomic; o retorno posterior zera os contadores. `abaixo_do_horizonte` é informativo, e o dedup corretamente o exclui das recusas. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:278`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:281`; `app/chamados/management/commands/dedup_perguntas_fato.py:91`)

**NAO PROCEDE segundo escritor do backfill localizado:** os commands preparam o DRY e encaminham a escrita para `aplicar`. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:401`; `app/chamados/management/commands/dedup_perguntas_fato.py:89`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:256`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:265`)

Exclusão mútua contra entrega concorrente da competência: **[nao verificado]**; `competencia_entregue` faz leitura simples, o que não invalida, por si, a conferência literal exigida dentro do atomic. (`app/folha/export.py:581`)

**4. Classe (4) — PROCEDENTE: significado declarado, mas censo incompleto e leitores de decisão incompatíveis.**

Pergunta de fato usa **dia do turno**, com `conversa=False`; conversa usa **dia civil da emissão**, com `conversa=True`. O proxy e `resolvedora_valida` agora respeitam essa distinção. (`app/chamados/models.py:2631`; `app/chamados/models.py:2642`; `app/chamados/models.py:2673`; `app/chamados/juizes.py:1969`)

O censo lista `_dia_da_disputa`, `dias_do_fato`, prévia, copiloto e diagnóstico, mas conclui “Nenhum deles escreve”. Essa conclusão não considera os consumidores que decidem a escrita. (`app/chamados/services/fio.py:1034`)

Há provas concretas:

- **Leitor censado alimenta escrita:** `dias_do_fato` inclui datas das conversas; o arquivamento usa seu resultado para decidir pelo corte e depois silenciar o Chamado. Conversa encerrada não impede o caminho, pois a guarda procura pergunta viva. (`app/chamados/juizes.py:209`; `app/chamados/management/commands/arquivar_competencia_encerrada.py:102`; `app/chamados/management/commands/arquivar_competencia_encerrada.py:121`; `app/chamados/management/commands/arquivar_competencia_encerrada.py:158`; `app/chamados/management/commands/arquivar_competencia_encerrada.py:288`)
- **Escritor fora do censo:** `fabrica_da_celula.perguntar` seleciona por motivo/data sem distinguir conversa e chama a porta que pode descarimbar a pergunta. Os filtros posteriores também não excluem conversa. (`app/chamados/services/fabrica_da_celula.py:145`; `app/chamados/services/relavra_via.py:179`; `app/chamados/services/relavra_via.py:234`; `app/chamados/services/relavra_via.py:244`)
- **Decisores fora do censo:** `dia_da_pergunta_trancado` transforma essa data em competência e pode matar a pergunta antes da resposta; `_dias_das_perguntas_vivas` transforma a data civil em fato usado por `competencia_do_chamado`. (`app/chamados/juizes.py:932`; `app/chamados/juizes.py:945`; `app/chamados/juizes.py:1141`; `app/chamados/juizes.py:2037`; `app/chamados/juizes.py:2058`; `app/chamados/services/competencia_trancada.py:92`)

Também falta o leitor de classificação `pergunta_no_app.retencao`, que pode tomar a conversa civil como pergunta do dia do furo. (`app/chamados/services/pergunta_no_app.py:92`)

**5. Classe (5) — NAO PROCEDE no leitor de “novas perguntas” localizado.**

Ele lê `data_evento` da Pergunta e compara com o dia local da aplicação da 0052. Ausência do registro ou dia nulo alarma; o escape dos fatos anteriores está declarado. Não usa `Disputa.aberta_em`. (`app/chamados/services/fio.py:1158`; `app/chamados/services/fio.py:1165`; `app/chamados/services/fio.py:1168`; `app/chamados/services/fio.py:1172`)

**6. Selos — PROCEDENTE insuficiência de alcance; NAO PROCEDE ausência geral de mordida.**

| Selo | Burla que permanece |
|---|---|
| A — trava | O detector reconhece SQL literal `FOR UPDATE`, mas a varredura só o chama se houver `select_for_update` no arquivo. Um arquivo contendo somente SQL cru passa. O mutante testa o detector isolado, sem esse filtro. (`app/chamados/tests/test_e1_v7_catracas.py:120`; `app/chamados/tests/test_e1_v7_catracas.py:238`; `app/chamados/tests/test_e1_v7_catracas.py:277`) |
| A — trava | `lock = qs.select_for_update; lock()` não é chamada de atributo nem o `getattr` reconhecido. (`app/chamados/tests/test_e1_v7_catracas.py:100`; `app/chamados/tests/test_e1_v7_catracas.py:116`) |
| B — canônica | Argumento nomeado recortado escapa: o detector examina somente `n.args[0]`. `min`, `sorted()[0]` e QuerySet filtrado continuam no limite declarado. (`app/chamados/tests/test_e1_v7_catracas.py:185`; `app/chamados/tests/test_e1_v7_catracas.py:26`) |
| C — escrita | `gravar = p.save; gravar()` escapa: há resolução de alias de importação, mas não dessa atribuição. Helper externo segue no limite declarado. (`app/chamados/tests/test_e1_v7_catracas.py:208`; `app/chamados/tests/test_e1_v7_catracas.py:217`; `app/chamados/tests/test_e1_v7_catracas.py:28`) |

A/B percorrem `app` com exclusões; C examina os dois commands. Esse alcance não equivale a uma garantia semântica sobre todo o repositório. (`app/chamados/tests/test_e1_v7_catracas.py:37`; `app/chamados/tests/test_e1_v7_catracas.py:43`; `app/chamados/tests/test_e1_v7_catracas.py:194`)

Os mutantes têm expectativas que mordem as formas sintéticas escolhidas, inclusive homônimos e aliases. **Não demonstram o caminho completo**, como mostra o SQL cru filtrado antes do detector. (`app/chamados/tests/test_e1_v7_catracas.py:270`; `app/chamados/tests/test_e1_v7_catracas.py:277`; `app/chamados/tests/test_e1_v7_catracas.py:340`; `app/chamados/tests/test_e1_v7_catracas.py:383`)

Quanto aos REDs v8:

- Canônica exige duas retratações, preservação de p1, restauração do índice e idempotência. Porém usa uma única batida e não chama efetivamente abrir/migração; não cobre a legada que conecta episódios distintos. (`app/chamados/tests/test_e1_v8.py:50`; `app/chamados/tests/test_e1_v8.py:65`; `app/chamados/tests/test_e1_v8.py:72`; `app/chamados/tests/test_e1_v8.py:77`)
- Ordem exige presença das tabelas e ausência de violações, mas o observador registra somente a primeira operação por **tabela**. Uma aquisição posterior de outra PK desaparece; uma consulta registrada também não prova que encontrou linha. (`app/chamados/tests/test_e1_v8.py:108`; `app/chamados/tests/test_e1_v8.py:121`; `app/chamados/tests/test_e1_v5_ordem_trava.py:36`; `app/chamados/tests/test_e1_v5_ordem_trava.py:46`)
- Conversa verifica nascimento aceito e fato cruzado recusado isoladamente; não testa a composição do I22. (`app/chamados/tests/test_e1_v8.py:138`; `app/chamados/tests/test_e1_v8.py:148`)

Execução Django desses REDs e comprovação histórica da mordida: **[nao verificado]**.

**7. Escritores e leitores — PROCEDENTE incompatibilidade residual de datas e imprecisão de `abaixo_do_horizonte`.**

| Campo/estado | Quem escreve | Quem lê / significado |
|---|---|---|
| `via_resolucao` | `_escrever_carimbo`, pelas portas de carimbo/descarimbo; lote tem UPDATE; exceções existentes são `encerrar_por_fato_no_espelho` e `relavrar`. (`app/chamados/models.py:2818`; `app/chamados/models.py:2983`; `app/chamados/models.py:2992`; `app/chamados/services/relavra_via.py:123`) | C6, identidade e backfill leem via de resolução/identidade. Duplicata é terminal antes da resposta; a documentação corrigida concorda com isso. (`app/chamados/juizes.py:1148`; `app/chamados/models.py:2569`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:129`; `app/chamados/models.py:2893`) |
| “retratada” | É estado representado pela via: dedup grava `VIA_DUPLICATA`; remoção de motivo grava `VIA_MOTIVO_RETRATADO`. (`app/chamados/services/fio.py:1002`; `app/chamados/models.py:2019`) | `fora_do_fato` exclui da identidade; C6 torna terminal quando carimbada. A resposta permanece histórica. (`app/chamados/models.py:2922`; `app/chamados/juizes.py:1148`; `app/chamados/models.py:2894`) |
| `data_evento` | Nascimento; preenchimento no reapontamento; rechaveamento. (`app/chamados/models.py:2739`; `app/chamados/services/fio.py:840`; `app/chamados/services/fio.py:948`) | Identidade/horizonte usam turno; leitores do item 4 ainda recebem dia civil sem distinguir. **Significado incompatível entre consumidores.** (`app/chamados/models.py:2599`; `app/chamados/services/fio.py:1043`; `app/chamados/juizes.py:932`) |
| `conversa` | As portas fixam `False`/`True`; `_nascer` persiste e agora transmite ao proxy. (`app/chamados/models.py:2631`; `app/chamados/models.py:2642`; `app/chamados/models.py:2673`; `app/chamados/models.py:2740`) | Identidade, índice e resolvedora usam “sem fato”. Os leitores do item 4 ignoram o discriminador. (`app/chamados/models.py:2599`; `app/chamados/models.py:3207`; `app/chamados/juizes.py:1969`) |
| `abaixo_do_horizonte` | `aplicar` acumula perdedoras fora de `q_do_horizonte`; retorno deduplica PKs. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:230`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:284`) | CLI chama de perdedoras do acervo abaixo; dedup corretamente não interpreta como recusa. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:419`; `app/chamados/management/commands/dedup_perguntas_fato.py:92`) |

O último significado é mais amplo que o anunciado: uma BATIDA perdedora com `data_evento=None` também entra em `abaixo_do_horizonte`. O agrupador permite esse NULL, embora o censo o distinga como `batida_sem_dia`. **Não escreve a linha, mas classifica “sem dia” como “abaixo”.** (`app/chamados/services/fio.py:1021`; `app/chamados/services/fio.py:1108`; `app/chamados/services/fio.py:1111`; `app/chamados/services/fio.py:1148`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:230`)

**8. Reincidências — PROCEDENTES universo paralelo, ordem de trava, cobertura insuficiente e relógio não injetado; NAO PROCEDE segundo juiz de preferência ou segundo escritor do backfill demonstrado.** (`app/chamados/services/fio.py:1081`; `app/chamados/models.py:2023`; `app/chamados/tests/test_e1_v7_catracas.py:238`; `app/chamados/services/fio.py:978`; `app/chamados/management/commands/dedup_perguntas_fato.py:90`)

O relógio residual alcança **teste novo**: apesar de declarar todos os instantes literais/injetados, ele chama `ch.fechar` sem congelamento, chegando a `timezone.now()`. As novas fixtures também criam Disputa com `aberta_em=auto_now_add`. Portanto contar zero `now()` literal no diff não demonstra injeção. (`app/chamados/tests/test_e1_v8.py:12`; `app/chamados/tests/test_e1_v8.py:106`; `app/chamados/models.py:450`; `app/chamados/models.py:244`; `app/chamados/tests/test_e1_v8.py:104`; `app/chamados/models.py:1715`)

**NAO PROCEDE subida dos cinco TETOS sem frase C8:**

| Cenário | Teto / aumento | Referência |
|---|---:|---|
| Estado sem pergunta | 27 / +2 | `app/ponto/tests/test_selo_chokepoint_escrita.py:116` |
| Reconciliar fecha | 59 / +2 | `app/ponto/tests/test_selo_chokepoint_escrita.py:118` |
| Uma cobrança fecha | 208 / +4 | `app/ponto/tests/test_selo_chokepoint_escrita.py:161` |
| Três cobranças fecham | 262 / +8 | `app/ponto/tests/test_selo_chokepoint_escrita.py:162` |
| Três perguntas, sem cruzar | 10 / +2 | `app/ponto/tests/test_selo_chokepoint_escrita.py:193` |

A justificativa é compatível com **duas consultas adicionais por entrada**: Disputas e Chamados. Isso satisfaz a exigência documental da C8, mas não prova tempo ou contenção constantes: o conjunto de linhas bloqueadas cresce com Disputas abertas e Chamados vivos. Medições citadas nos comentários: **[nao verificado]**. (`PLANO_PISCADA.md:10`; `app/chamados/models.py:2518`; `app/chamados/models.py:2520`; `app/chamados/models.py:2528`; `app/chamados/models.py:2533`)

A diferença de mérito anteriormente levada a desenho permanece: migração retrata perdedoras sem o veto de `admin_decidiu` com resposta que o backfill aplica. (`app/chamados/services/fio.py:1265`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:129`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:233`)

**9. `resolvedora_valida` — PROCEDENTE cegueira do I22; NAO PROCEDE transformação automática da entrada factual em conversa ou cegueira equivalente no I8b.**

`abrir` força `conversa=False`, e o proxy preserva esse valor. A mudança não desvia essa porta para o retorno antecipado da conversa. (`app/chamados/models.py:2631`; `app/chamados/models.py:2673`; `app/chamados/juizes.py:1969`)

O contraexemplo do **I22 não exige flag corrompida**: Chamado vivo com perguntas de fato de outro turno e uma conversa legítima apontada para ele. A conversa agora devolve `True`; o `any(...)` fica verdadeiro e o I22 deixa de acusar que nenhuma pergunta **de fato** resolve aquele turno. (`app/chamados/models.py:2642`; `app/chamados/juizes.py:1969`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1043`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1046`)

O **I8b continua examinando cada pergunta viva individualmente**: a conversa irmã não impede que uma pergunta factual cruzada seja rejeitada pelo juiz. Os REDs novos cobrem a recusa individual, mas não a composição que mascara o I22. (`app/chamados/management/commands/auditar_invariantes_chamados.py:891`; `app/chamados/management/commands/auditar_invariantes_chamados.py:896`; `app/chamados/tests/test_e1_v8.py:148`)

## 07/11 16:17 · sessao f802bf7d346e · - · - · origem=Ronald (tmux)

**2 — PROCEDENTE ordem implícita residual; NAO PROCEDE outro `select_for_update` explícito de produção localizado.**

As quatro aquisições explícitas estão no dono: `app/chamados/models.py:2513`, `:2520`, `:2533`, `:2537`. Os dois caminhos antigos foram antecipados: `_gravar_estado` em `app/chamados/models.py:290` e ausência rejeitada em `app/chamados/services/materializacao.py:676`.

Permanecem dois contraexemplos de código:

1. **Pergunta → Disputa sem trava prévia:** `DisputaSupervisao.remover_motivo` abre atomic em `app/chamados/models.py:2011`, carimba a Pergunta em `:2019`, cujo save ocorre em `:2911`, e depois salva a Disputa em `:2023`. Não chama `trava_na_ordem`. Entradas reais: reativação de Posto em `app/colaboradores/signals.py:77` e calibração de âncora em `app/escala/signals.py:161`. Os receivers consultam as disputas e chamam diretamente, sem antecipar essas travas (`app/colaboradores/signals.py:71`; `app/escala/signals.py:155`).

2. **Chamado → Disputa por chamadas sucessivas ao próprio dono:** aprovação de justificativa chama `travar_origem_antes` dentro do atomic (`app/ponto/services/justificativa.py:134`, `:135`). O helper trava Colaborador e Chamados **sem** `abertas_do_colaborador=True` (`:123`). Depois `encerrar_chamado_de_origem` (`:211`) trava novamente somente Colaborador/origem (`:39`) e chama `retratar` (`:44`). Este alcança `reconciliar` (`app/chamados/reconciliador.py:523`), que chama `_decisao_do_fechamento` (`app/chamados/models.py:634`), `_recarregar_estado_se_mudou` (`:655`) e finalmente adquire as Disputas abertas em `:694`, depois dos Chamados já adquiridos. Premissa suficiente: origem viva sem retenção e outra Disputa aberta da mesma pessoa. Há variante no bloqueio de entrada (`app/ponto/services/justificativa.py:76`, `:83`). Reprodução/concorrência real: **[nao verificado]**.

**6 — PROCEDENTE alcance insuficiente; NAO PROCEDE ausência geral de mordida.**

Burla importante **não declarada**: A reconhece SQL literal com tabela e `FOR UPDATE` (`app/chamados/tests/test_e1_v7_catracas.py:120`), mas o pipeline só chama o detector se o arquivo contiver o texto `select_for_update` (`:238`). Um arquivo contendo apenas `cursor.execute('SELECT ... FROM "chamados_perguntadisputa" FOR UPDATE')` passa a catraca. O mutante chama diretamente `travas_fora`, sem passar pelo filtro que o excluiria (`:277`).

Outras burlas:

- A: método ligado, `lock = qs.select_for_update; lock()`, não é chamada de atributo nem `getattr` reconhecido (`app/chamados/tests/test_e1_v7_catracas.py:100`, `:116`).
- B: recorte passado por argumento nomeado não é examinado; só `n.args[0]` é lido (`:185`). `min`, `sorted()[0]` e QuerySet filtrado seguem no limite declarado (`:26`).
- C: método ligado, `gravar = p.save; gravar()`, não tem resolução transitiva da atribuição, só nome/alias de import/getattr (`:208`, `:217`). Helper externo também segue no limite declarado (`:28`).
- A/B só percorrem `app` e excluem diretórios/nome de testes; C só dois commands (`:35`, `:38`, `:364`). [nao verificado]

Executei em memória os 22 testes das três classes com `python3 -B`, substituindo apenas `django.test.SimpleTestCase` por `unittest.TestCase`, sem Django, banco ou arquivos escritos: 22 passaram, incluindo os três zeros e mutantes existentes. Essa execução não é a suíte Django nem prova PostgreSQL; por exigência de referência para afirmações, resultado sem arquivo persistido: **[nao verificado]**. Os próprios testes e suas expectativas estão em `app/chamados/tests/test_e1_v7_catracas.py:232`, `:297`, `:362`.

REDs v8 têm mordida direcionada por leitura do código:

- Canônica verifica p1, duas retratações, p1 intocada, restauração do índice e idempotência (`app/chamados/tests/test_e1_v8.py:63`, `:72`, `:75`, `:77`, `:78`). Porém chama `do_fato + canonica_do_grupo`, sem invocar efetivamente `abrir` nem migração (`:65`, `:66`).
- Ordem exige tabelas e ausência de violações (`:108`, `:109`, `:110`, `:121`, `:122`), mas herda observador que registra só a primeira operação por **tabela**, inclusive consulta sem provar linhas retornadas (`app/chamados/tests/test_e1_v5_ordem_trava.py:36`, `:46`). Não é prova de ordem por PK nem bloqueio real de todas as linhas.
- Conversa comprova nascimento com dia civil e destino antigo (`app/chamados/tests/test_e1_v8.py:138`, `:142`), com controle factual recusado (`:148`). Não testa todos os defeitos estruturais de I8b/I22.

**8 — TETOS: NAO PROCEDE subida sem C8; PROCEDENTE ressalva sobre “custo constante”.**

Cinco aumentos têm frase escrita ao lado: 27/+2 (`app/ponto/tests/test_selo_chokepoint_escrita.py:116`), 59/+2 (`:118`), 208/+4 (`:161`), 262/+8 (`:162`), 10/+2 (`:193`). C8 exige precisamente justificativa ao lado (`PLANO_PISCADA.md:10`).

A decomposição de +2 por entrada é compatível com uma consulta das Disputas e outra dos Chamados (`app/chamados/models.py:2519`, `:2532`). É número fixo de consultas por entrada; o número de linhas examinadas/travadas depende das Disputas abertas e Chamados vivos (`:2518`, `:2528`). Tempo/contenção e medições citadas nos comentários: **[nao verificado]**.

**Relógio residual também existe no arquivo novo**, não apenas nas linhas antigas descartadas pela triagem: `test_e1_v8.py:12` declara todos os instantes literais/injetados, mas o novo teste chama `ch.fechar` sem congelamento (`app/chamados/tests/test_e1_v8.py:106`), que alcança `_gravar_estado` (`app/chamados/models.py:454`, `:450`) e `timezone.now()` (`:244`). Fixtures novas criam Disputa (`app/chamados/tests/test_e1_v8.py:49`, `:104`, `:136`, `:147`) cujo `aberta_em` usa `auto_now_add=True` (`app/chamados/models.py:1715`). Efeito no resultado: **[nao verificado]**.

## 07/11 16:17 · sessao bb8915b7b890 · - · - · origem=Ronald (tmux)

1. **Classe (1) — PROCEDENTE: permanece divergência de universo, agora introduzida por `universo_do_grupo`.** A identidade BATIDA é por episódio, e dois episódios no mesmo dia são fatos distintos. (`app/chamados/catalogo/motivos.py:16`; `app/chamados/services/fio.py:1017`; `app/chamados/management/commands/dedup_perguntas_fato.py:54`)

   Contraexemplo estático: mesmo colaborador/motivo/dia acima do horizonte; `L` legada sem episódio, muda e menor PK; `A` do episódio α, muda; `B` do episódio β, respondida. `do_fato(α)` retorna `[L,A]`, porque seu OR acrescenta somente as legadas sem episódio daquele dia; `abrir(α)` escolhe `L`. (`app/chamados/models.py:2565`; `app/chamados/models.py:2567`; `app/chamados/models.py:2691`; `app/chamados/models.py:2692`; `app/chamados/services/fio.py:984`)

   `grupos_de_colisao` detecta α com a legada e entrega `[L,A]`. `aplicar` chama novamente `universo_do_grupo([L,A])`; a iteração sobre `L`, sem episódio, chama `do_fato(..., batida_origem_id=None)`, que consulta **todas** as perguntas daquele motivo/dia, incluindo `B`. Assim o apply escolhe `B` e retrata `A`, embora a porta α escolhesse `L`. `universo_do_grupo` não é idempotente, e reaplicar expansão alarga o fato. (`app/chamados/services/fio.py:1116`; `app/chamados/services/fio.py:1120`; `app/chamados/services/fio.py:1126`; `app/chamados/services/fio.py:1076`; `app/chamados/services/fio.py:1079`; `app/chamados/services/fio.py:1081`; `app/chamados/models.py:2571`; `app/chamados/models.py:2598`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:209`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:221`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:226`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:265`)

   O DRY escolhe diretamente sobre os PKs fornecidos pelo agrupador, enquanto o apply expande de novo: portanto DRY/apply também podem informar/escolher canônicas diferentes. A migração permanece com o universo direto de `do_fato`, que não faz essa segunda expansão. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:358`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:359`; `app/chamados/management/commands/dedup_perguntas_fato.py:68`; `app/chamados/management/commands/dedup_perguntas_fato.py:70`; `app/chamados/services/fio.py:1248`; `app/chamados/services/fio.py:1255`)

   Ocorrência de produção e reprodução em banco: **[nao verificado]**.

   **NAO PROCEDE o contraexemplo anterior do horizonte:** a canônica agora vem antes de restringir a escrita ao horizonte; tampouco localizei outro implementador de preferência por resposta/PK fora de `canonica_do_grupo`. Os consumidores centrais delegam a escolha. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:226`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:227`; `app/chamados/services/declaracao_texto.py:245`; `app/chamados/services/declaracao_texto.py:267`; `app/chamados/services/fio.py:891`; `app/chamados/services/fio.py:1255`; `app/chamados/models.py:2692`)

2. **Classe (3) — NAO PROCEDE a permanência dos defeitos anteriores enumerados.** O atomic engloba o laço; as travas precedem a conferência do horizonte, mérito e exportada; o cache `_ent` nasce novamente para cada colaborador. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:202`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:208`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:211`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:215`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:218`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:233`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:239`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:251`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:252`)

   Qualquer recusa provoca exceção dentro do atomic e retorno com contadores zerados; `abaixo_do_horizonte` é separado das recusas, e o dedup o exclui corretamente desse conjunto. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:278`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:280`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:284`; `app/chamados/management/commands/dedup_perguntas_fato.py:91`)

   **NAO PROCEDE segundo escritor do backfill localizado:** os dois commands encaminham a execução à mesma `aplicar`; as escritas do apply estão nela, por `DecisaoPergunta.registrar`, `retratar_duplicata` e `evento`. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:401`; `app/chamados/management/commands/dedup_perguntas_fato.py:89`; `app/chamados/management/commands/dedup_perguntas_fato.py:90`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:256`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:265`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:267`)

   A conferência de exportada é uma leitura simples da autoridade; exclusão mútua contra exportação concorrente não foi demonstrada. Isso é **[nao verificado]**, não prova de falha no requisito literal de reler dentro do atomic depois da trava. (`app/folha/export.py:581`; `app/folha/export.py:586`)

3. **Item 7 — PROCEDENTE imprecisão no significado de `abaixo_do_horizonte`.** O escritor coloca ali todo perdedor que **não satisfaz** `q_do_horizonte`, inclusive BATIDA com `data_evento=None`; não somente datas anteriores ao horizonte. O agrupador BATIDA admite data nula porque sua exclusão de NULL é por `batida_origem`, e basta outra linha do episódio estar no horizonte. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:227`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:230`; `app/chamados/services/fio.py:1021`; `app/chamados/services/fio.py:1043`; `app/chamados/services/fio.py:1108`; `app/chamados/services/fio.py:1110`; `app/chamados/services/fio.py:1111`)

   Exemplo estático: mesma batida/motivo; `p_null` muda com data nula, `p_datada` respondida no horizonte. `p_null` vai a `abaixo_do_horizonte`, enquanto o próprio censo a distingue como `batida_sem_dia`. A documentação e a saída do command chamam todas essas linhas de perdedoras abaixo do horizonte. A linha continua intocada, mas a classificação é mais ampla que o significado anunciado. (`app/chamados/services/fio.py:1148`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:164`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:230`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:419`)

4. **Item 8 — PROCEDENTE derivação paralela de universo; NAO PROCEDE segundo juiz de preferência ou segundo escritor de backfill demonstrado.** A preferência segue centralizada, mas o novo expansor de grupos produz universo diferente da porta/migração no contraexemplo acima. (`app/chamados/services/fio.py:978`; `app/chamados/services/fio.py:1070`; `app/chamados/models.py:2691`; `app/chamados/services/fio.py:1248`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:221`)

5. **Cobertura do RED de canônica — morde a regressão específica anterior; não cobre o novo contraexemplo.** A fixture usa uma única batida, compartilhada por todas as perguntas; não tem legada sem episódio nem segundo episódio. Ela exige retratar as duas perdedoras, preservar a canônica, reinstalar índice e obter zero retratações na segunda aplicação. Execução/RED→GREEN histórico: **[nao verificado]**. (`app/chamados/tests/test_e1_v8.py:50`; `app/chamados/tests/test_e1_v8.py:54`; `app/chamados/tests/test_e1_v8.py:63`; `app/chamados/tests/test_e1_v8.py:72`; `app/chamados/tests/test_e1_v8.py:75`; `app/chamados/tests/test_e1_v8.py:77`; `app/chamados/tests/test_e1_v8.py:78`)

6. **Questão de desenho da triagem anterior permanece:** `_migrar_sob_trava` retrata as perdedoras sem consultar `merito_perdedor`; o backfill veta expressamente `admin_decidiu` com resposta. A mudança atual não resolve essa diferença entre portas, embora ambas usem a mesma preferência de canônica. (`app/chamados/services/fio.py:1264`; `app/chamados/services/fio.py:1265`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:124`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:129`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:233`)

## 07/11 16:17 · sessao d0b6bdfa5b99 · - · - · origem=Ronald (tmux)

4. **PROCEDENTE: o censo está incompleto e há consumidores que decidem/escrevem com o dia civil da conversa.** O contrato atual separa turno (`abrir`, `conversa=False`) e emissão civil (`abrir_conversa`, `conversa=True`). (`app/chamados/models.py:2631`; `app/chamados/models.py:2635`; `app/chamados/models.py:2642`)

- `dias_do_fato`, já censado, alimenta uma escrita: o comando de arquivamento escolhe alvos pelo dia devolvido e pelo corte, chama `_aplicar` e silencia o chamado. Conversa encerrada não impede esse caminho: a guarda verifica existência de pergunta viva. Logo a frase “Nenhum deles escreve” não cobre a cadeia de decisão. (`app/chamados/juizes.py:209`; `app/chamados/management/commands/arquivar_competencia_encerrada.py:102`; `app/chamados/management/commands/arquivar_competencia_encerrada.py:113`; `app/chamados/management/commands/arquivar_competencia_encerrada.py:121`; `app/chamados/management/commands/arquivar_competencia_encerrada.py:141`; `app/chamados/management/commands/arquivar_competencia_encerrada.py:158`; `app/chamados/management/commands/arquivar_competencia_encerrada.py:288`; `app/chamados/services/fio.py:1040`)
- Outro leitor ausente do censo: `fabrica_da_celula.perguntar` seleciona perguntas pelo dia, sem excluir conversa, e chama `desfazer_carimbo_sem_lastro`. A porta relê pela elegibilidade/carimbo, consulta C6 e descarimba; esses filtros também não excluem conversa. Assim uma conversa carimbada pode entrar na manutenção do fato do turno cuja data coincida com sua emissão civil. (`app/chamados/services/fabrica_da_celula.py:145`; `app/chamados/services/relavra_via.py:179`; `app/chamados/services/relavra_via.py:234`; `app/chamados/services/relavra_via.py:240`; `app/chamados/services/relavra_via.py:244`)
- Também faltam no censo `dia_da_pergunta_trancado`, que calcula competência de `data_evento` sem discriminador e mata a pergunta antes da resposta; e `_dias_das_perguntas_vivas`, que transforma a data civil em fato usado por `competencia_do_chamado`. (`app/chamados/juizes.py:932`; `app/chamados/juizes.py:945`; `app/chamados/juizes.py:955`; `app/chamados/juizes.py:1141`; `app/chamados/juizes.py:2037`; `app/chamados/juizes.py:2040`; `app/chamados/juizes.py:2058`; `app/chamados/juizes.py:2062`)
- `pergunta_no_app.retencao` pode rotular uma conversa como pergunta viva do mesmo dia do furo. Esse consumidor não diferencia conversa no teste da data. (`app/chamados/services/pergunta_no_app.py:92`)

5. **NAO PROCEDE a reincidência no contador “nova pergunta”.** Ele lê `data_evento` da pergunta, compara com o dia local da aplicação da 0052 e declara o escape dos fatos anteriores. Não depende de `Disputa.aberta_em`. (`app/chamados/services/fio.py:1158`; `app/chamados/services/fio.py:1165`; `app/chamados/services/fio.py:1168`; `app/chamados/services/fio.py:1171`; `app/chamados/services/fio.py:1172`)

9. **PROCEDENTE para a cegueira do I22; NAO PROCEDE desvio automático das perguntas abertas pela porta de fato.** `abrir` força `conversa=False`, e `_nascer` passa esse discriminador ao proxy. A alteração não transforma essa entrada em conversa. (`app/chamados/models.py:2631`; `app/chamados/models.py:2673`)

O contraexemplo do I22 não exige dado malformado: chamado vivo com uma pergunta de fato de outro turno e uma conversa legítima apontada para ele. Antes, ambas podiam falhar na comparação; agora a conversa retorna `True`, e o `any(...)` do I22 torna-se verdadeiro, escondendo que nenhuma pergunta **de fato** resolve aquele turno. A conversa legítima pode receber destino pela própria porta pública. (`app/chamados/models.py:2635`; `app/chamados/models.py:2642`; `app/chamados/juizes.py:1969`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1043`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1047`)

**NAO PROCEDE a mesma cegueira individual no I8b:** ele examina cada pergunta viva separadamente; a pergunta de fato cruzada continua sendo rejeitada pelo juiz, independentemente da conversa irmã. (`app/chamados/management/commands/auditar_invariantes_chamados.py:891`; `app/chamados/management/commands/auditar_invariantes_chamados.py:895`; `app/chamados/management/commands/auditar_invariantes_chamados.py:897`; `app/chamados/juizes.py:2009`)

Os REDs novos verificam conversa aceita e fato cruzado recusado isoladamente; não verificam a composição de resolvedoras no I22. (`app/chamados/tests/test_e1_v8.py:135`; `app/chamados/tests/test_e1_v8.py:143`)

Ocorrência desses estados em produção, reprodução no banco e execução dos testes: **[nao verificado]**. [nao verificado]

## 07/11 15:47 · sessao d55aaa0b4dac · FIO-E0-V4-E2 · verificador · origem=Code

**Não atesto equivalência irrestrita.** O predicado ficou centralizado, mas há contraexemplos entre lote e chamadas separadas com Empresas divergentes em cache; também existe um caminho condicional de escrita pela tarefa. (`app/chamados/juizes.py:932`, `app/chamados/juizes.py:975`, `app/chamados/juizes.py:990`, `app/core/utils.py:18`)

Suíte, deltas efetivamente executados, RED anterior, estado de produção e sequência de pouso: **[nao verificado]**. [nao verificado]

**1) Quem escreve, quem lê e significado**

| Dado | Escritor → leitor; significado |
|---|---|
| `PeriodoFechado.fechado_em` | Nasce com `auto_now_add`; `trancar_periodo` renova ao retrancar uma competência reaberta. O juiz usa como **instante inferior exclusivo** do toque. Também é comparado com `resgatado_em` pelo encerrador de chamados. (`app/ponto/models.py:926`, `app/ponto/services/fechamento.py:1401`, `app/ponto/services/fechamento.py:1408`, `app/chamados/juizes.py:999`, `app/chamados/services/competencia_trancada.py:104`) |
| `PeriodoFechado.reaberto_em` | `reabrir_periodo` grava o instante; retrancar limpa. `periodo_trancado` e `competencia_trancada` leem **NULL versus não NULL**, não um horizonte temporal; `ativo` não decide. Outros leitores diretos usam o mesmo filtro. (`app/ponto/services/fechamento.py:1410`, `app/ponto/services/fechamento.py:1432`, `app/ponto/services/fechamento.py:1463`, `app/ponto/services/fechamento.py:1473`, `app/chamados/juizes.py:1894`, `app/chamados/services/competencia_trancada.py:311`) |
| `LogAuditoria.acao`, `modelo`, `objeto_id`, `descricao` | A porta `registrar_log` encaminha para `evento`, que cria a linha. São verbo/modelo textuais, ID inteiro sem FK e texto livre. Para cobrar: `cobrar_dia_manual/Colaborador/colab.pk`, com `dia=<data>`; resolver: `veredito_celula/CelulaDia/cel.pk`; desarquivar: `desarquivar_chamado/ChamadoColaborador/ch.pk`. O leitor reconhece exatamente essas combinações. (`app/core/auditoria.py:22`, `app/core/observ.py:134`, `app/core/models.py:296`, `app/chamados/services/cobrar_dia.py:37`, `app/ponto/services/veredito_celula.py:334`, `app/chamados/views_arquivar.py:77`, `app/chamados/services/arquivo.py:137`, `app/chamados/juizes.py:1150`, `app/chamados/juizes.py:1175`, `app/chamados/juizes.py:1199`) |
| `LogAuditoria.criado_em`, `usuario` | Instante automático e FK anulável; `evento` resolve o usuário pelo ator explícito/request. O juiz exige `criado_em > fechado_em`, usuário não NULL e diferente de sistema. **Não verifica papel ADMIN**: “humano” nesse predicado significa esses dois filtros. (`app/core/models.py:295`, `app/core/models.py:307`, `app/core/observ.py:108`, `app/core/observ.py:118`, `app/chamados/juizes.py:1141`) |
| `ChamadoColaborador.celula` | FK anulável da célula de origem. O nascimento obtém a célula pelo colaborador/dia do fato; `apontar_celula` preenche somente ponteiro ainda NULL. O juiz lê o vínculo **atual**, exigindo também que o chamado pertença ao colaborador consultado. (`app/chamados/models.py:48`, `app/chamados/models.py:1090`, `app/chamados/models.py:1119`, `app/chamados/models.py:1191`, `app/chamados/services/vinculos.py:92`, `app/chamados/juizes.py:1188`, `app/chamados/juizes.py:1192`) |
| `CelulaDia.colaborador`, `data` | FK da pessoa e `DateField`. Geração, regeneração e edição humana criam pela pessoa e pelo dia percorrido/recebido. O juiz consulta IDs/datas em lote e depois confere o **par exato**, eliminando os cruzamentos extras do `__in`. (`app/escala/models.py:1681`, `app/escala/models.py:1685`, `app/ponto/portas/celula.py:259`, `app/ponto/portas/celula.py:600`, `app/ponto/portas/celula.py:849`, `app/chamados/juizes.py:1164`, `app/chamados/juizes.py:1167`) |

Os instantes seguem a configuração `USE_TZ=True`, `America/Sao_Paulo`; o juiz não converte fuso nem escolhe o dia: recebe uma data pronta. A janela usa o corte empresarial, inclui início/fim e adota 21 quando falta Empresa, limitado a 2–28. Configuração efetiva de produção: [nao verificado]. (`app/config/settings/base.py:93`, `app/config/settings/base.py:95`, `app/chamados/juizes.py:991`, `app/ponto/janelas.py:10`, `app/ponto/janelas.py:24`, `app/ponto/janelas.py:36`)

| Chave/parâmetro | Escritor → leitor; significado |
|---|---|
| `('empresa', id)` | `dias_trancados` grava/lê a Empresa, inclusive `None`; alimenta o corte da janela. (`app/chamados/juizes.py:974`, `app/chamados/juizes.py:990`, `app/chamados/juizes.py:991`) |
| `('tranca', emp, mes, ano)` | O mesmo corpo grava/lê `PeriodoFechado` ou `None`; mês/ano vêm do **fim da competência**, não do mês civil do dia. (`app/chamados/juizes.py:992`, `app/chamados/juizes.py:995`, `app/chamados/juizes.py:997`) |
| `('toque', colab, dia, desde)` | `_admin_tocou_os_dias_depois` lê a chave e grava somente `True`; distingue pessoa, data e instante de tranca. (`app/chamados/juizes.py:1100`, `app/chamados/juizes.py:1101`, `app/chamados/juizes.py:1116`) |
| `('sistema',)` | O helper obtém o PK de sistema e o distribui pelos memos pendentes; a consulta da trilha o exclui. (`app/chamados/juizes.py:1107`, `app/chamados/juizes.py:1112`, `app/chamados/juizes.py:1142`) |
| `_tarefa_pura(trancado=)` | A carga fornece pertencimento a `self._trancado`; significa **competência trancada e ausência de toque humano posterior**, não apenas existência de `PeriodoFechado`. O corpo puro somente consome esse booleano. (`app/chamados/services/tarefa_do_dia.py:168`, `app/chamados/services/tarefa_do_dia.py:187`, `app/chamados/services/tarefa_do_dia.py:367`, `app/chamados/services/tarefa_do_dia.py:402`) |

**Não é correto afirmar que todo `data_evento` significa dia do turno.** Perguntas do fato exigem `DiaDoTurno`; conversas gravam o dia civil da criação. A tarefa seleciona `conversa=False`, enquanto C6/wrapper da tranca não fazem essa distinção. É uma dualidade anterior ao diff, mas o novo docstring generaliza demais ao chamar todo `data_evento` de dia do turno. (`app/chamados/models.py:2477`, `app/chamados/models.py:2494`, `app/chamados/services/tarefa_do_dia.py:321`, `app/chamados/juizes.py:930`, `app/chamados/juizes.py:932`, `app/chamados/juizes.py:1261`)

**2) Lote versus avulso**

O avulso novo é literalmente lote de um. Repetições preservam posições; empresas/competências possuem chaves distintas; células e chamados são reconduzidos ao par correto. Com entradas coerentes e estado estável durante a leitura, essas estruturas preservam o predicado. (`app/chamados/juizes.py:932`, `app/chamados/juizes.py:962`, `app/chamados/juizes.py:992`, `app/chamados/juizes.py:1167`, `app/chamados/juizes.py:1192`)

O menor `desde` está correto: a consulta traz um superconjunto, e **cada etapa** reaplica `> desde` do item. Um registro exatamente no instante da tranca não conta. (`app/chamados/juizes.py:1141`, `app/chamados/juizes.py:1156`, `app/chamados/juizes.py:1180`, `app/chamados/juizes.py:1203`)

A equivalência **para todo objeto aceito** falha nestas bordas:

- Dois colaboradores com mesmo `empresa_id`, memos separados e objetos Empresa com cortes 10/21: o lote reaproveita a primeira Empresa para ambos; chamadas separadas usam suas respectivas Empresas. Com dia 15/11 e tranca somente em novembro, reproduzi em memória **lote `[False, False]`; separados `[False, True]`**. Ocorrência em produção: [nao verificado]. (`app/chamados/juizes.py:975`, `app/chamados/juizes.py:979`, `app/chamados/juizes.py:990`, `app/ponto/janelas.py:24`)
- `SimpleNamespace(empresa_id=E, empresa=None)`: o corpo antigo anexado usava `None`, portanto corte default 21; o novo interpreta como Empresa faltante e busca E, podendo mudar competência e resultado. A comparação histórica depende do diff fornecido; execução de `e2dc08e2`: [nao verificado]. (`app/chamados/juizes.py:941`, `app/chamados/juizes.py:984`, `app/chamados/juizes.py:991`, `app/ponto/janelas.py:50`)
- NS sem atributo `empresa` agora pode ser atendido pela busca; Empresa inexistente é omitida por `in_bulk`, vira `None` e segue pela janela default. Isso substitui o acesso direto anterior ao atributo/FK, inclusive seus possíveis erros. Execução ORM dessas bordas: [nao verificado]. (`app/chamados/juizes.py:941`, `app/chamados/juizes.py:984`, `app/chamados/juizes.py:990`)

Os selos novos não cobrem essas bordas: usam uma Empresa, uma competência e colaboradores reais; o NS envolve pergunta/disputa. (`app/chamados/tests/test_e2_tarefa_do_dia.py:1146`, `app/chamados/tests/test_e2_tarefa_do_dia.py:1186`, `app/chamados/tests/test_e2_tarefa_do_dia.py:1198`)

**3) Quantidade e ordem de queries**

Para um colaborador válido, sistema existente e caches equivalentes, a sequência nominal permanece: Empresa → período → sistema → cobrar → células → resolver → chamados → desarquivar, interrompendo quando possível. Cada etapa necessária corresponde a uma consulta; o caminho frio completo chega a oito. A comparação com a base é estática pelo diff; deltas executados: [nao verificado]. (`app/chamados/juizes.py:984`, `app/chamados/juizes.py:995`, `app/chamados/juizes.py:1110`, `app/chamados/juizes.py:1149`, `app/chamados/juizes.py:1164`, `app/chamados/juizes.py:1175`, `app/chamados/juizes.py:1188`, `app/chamados/juizes.py:1198`)

**Não são as mesmas queries SQL:** saíram `contains/exists` do cobrar; agora são carregadas descrição/data e a seleção ocorre em Python. O custo de linhas/memória não é certificado pela contagem de queries. (`app/chamados/juizes.py:1149`, `app/chamados/juizes.py:1156`)

Há diferenças possíveis: NS com `empresa=None` ganha consulta de Empresa; o carregamento por `in_bulk` alimenta o memo, mas não aquece a FK `colab.empresa`, podendo deslocar uma consulta para um acesso posterior. (`app/chamados/juizes.py:940`, `app/chamados/juizes.py:984`, `app/chamados/juizes.py:990`)

`test_snc2` conta somente queries em `core_logauditoria` e `auth_user`; exige o delta de quatro consultas de trilha e igualdade do custo de sistema. **Não verifica total nem ordem de todas as queries.** (`app/chamados/tests/test_snc2_toque_em_lote.py:99`, `app/chamados/tests/test_snc2_toque_em_lote.py:107`, `app/chamados/tests/test_snc2_toque_em_lote.py:109`)

**4) Estado entre chamadas**

Empresa/tranca continuam com `lote=False`, no request ou colaborador; toque/sistema podem usar `LOTE_DA_TRANCA`. Os contextos são restaurados em `finally`. `da_vez` é local, mas seus objetos são copiados para os memos existentes, agora compartilhando referências entre colaboradores. (`app/chamados/juizes.py:970`, `app/chamados/juizes.py:971`, `app/chamados/juizes.py:990`, `app/chamados/juizes.py:996`, `app/chamados/juizes.py:1024`, `app/chamados/juizes.py:1055`, `app/core/middleware.py:78`, `app/core/middleware.py:82`)

**Não há NAO de toque gravado no `_tranca_memo`.** Só `True` entra nessa chave. (`app/chamados/juizes.py:1101`, `app/chamados/juizes.py:1115`, `app/chamados/juizes.py:1116`)

Há, porém, **estado novo na carga**: `self._trancado` conserva o resultado durante sua vida. Reutilizar a mesma carga após toque/reabertura mantém o resultado anterior; `.tarefa()` não consulta novamente o juiz. Isso é uma fotografia da carga, distinta do memo de toque. (`app/chamados/services/tarefa_do_dia.py:367`, `app/chamados/services/tarefa_do_dia.py:402`)

**5) Caminho da tarefa e posição do degrau**

No módulo inspecionado, a tranca entra por `dias_trancados`, vira `self._trancado` e chega ao corpo puro por booleano; não encontrei outro caminho direto de leitura da tranca. (`app/chamados/services/tarefa_do_dia.py:364`, `app/chamados/services/tarefa_do_dia.py:367`, `app/chamados/services/tarefa_do_dia.py:402`)

A ordem é `fora_vinculo → isento → trancado → sem_celula`. Portanto preserva as duas primeiras prioridades, mas muda casos existentes: **trancado sem célula passa de `sem_celula/nao_sei=True` para `competencia_trancada/nao_sei=False`**. Também antecede ata sem lâmpadas, teto, ausência e decisões. (`app/chamados/services/tarefa_do_dia.py:177`, `app/chamados/services/tarefa_do_dia.py:180`, `app/chamados/services/tarefa_do_dia.py:187`, `app/chamados/services/tarefa_do_dia.py:196`, `app/chamados/services/tarefa_do_dia.py:200`, `app/chamados/services/tarefa_do_dia.py:255`)

**6) Estado atual e horizonte**

Sim: o docstring declara expressamente tranca/toque como estado atual, sem corte por `agora`; a consulta compara a trilha somente com `fechado_em`. (`app/chamados/services/tarefa_do_dia.py:298`, `app/chamados/services/tarefa_do_dia.py:306`, `app/chamados/juizes.py:999`, `app/chamados/juizes.py:1141`)

O selo de horizonte continua mordendo decisões, efeitos, retratação e passivo posteriores; não ficou vazio. Sua lista textual não acrescentou nominalmente a tranca, embora remeta ao docstring completo da carga; também não cobre a interação entre horizonte e tranca. Logo ele não certifica uma reconstrução histórica integral da tarefa. (`app/chamados/tests/test_e2_tarefa_do_dia.py:709`, `app/chamados/tests/test_e2_tarefa_do_dia.py:714`, `app/chamados/tests/test_e2_tarefa_do_dia.py:732`, `app/chamados/tests/test_e2_tarefa_do_dia.py:750`, `app/chamados/tests/test_e2_tarefa_do_dia.py:785`)

**7) Classes de reprovação**

| Classe | Resultado |
|---|---|
| Segundo juiz/derivação paralela | Não apareceu um segundo corpo: C6 → avulso → lote; tarefa → lote. `_C20` está registrado e a frase da TRAVA existe. A equivalência universal exigida na declaração, contudo, tem as ressalvas do item 2. (`app/chamados/juizes.py:932`, `app/chamados/juizes.py:1261`, `app/chamados/services/tarefa_do_dia.py:364`, `app/core/juizes.py:292`, `app/docs/CORTES.md:425`, `CLAUDE.md:276`) |
| Escrita pelo leitor | **Caminho condicional encontrado:** toque pendente chama `get_user_sistema`, cujo `get_or_create` cria usuário se faltar. O helper é anterior, mas a tarefa agora o alcança. Migration semeia sistema; existência em produção: [nao verificado]. (`app/chamados/juizes.py:1107`, `app/chamados/juizes.py:1110`, `app/core/utils.py:18`, `app/core/migrations/0003_user_sistema.py:6`) |
| Relógio fora da borda | Os blocos novos recebem datas/instantes; não acrescentam leitura do relógio. A tarefa exige `agora` e confere compatibilidade da carga. (`app/chamados/juizes.py:999`, `app/chamados/juizes.py:1141`, `app/chamados/services/tarefa_do_dia.py:416`, `app/chamados/services/tarefa_do_dia.py:420`) |
| Caso14 sem mordida | Tem respostas fixas para trancado, O44, cobrar/resolver, sistema/nulo/antes/outro e reabertura. Não é apenas comparação entre implementações. (`app/chamados/tests/test_e2_tarefa_do_dia.py:1086`, `app/chamados/tests/test_e2_tarefa_do_dia.py:1092`, `app/chamados/tests/test_e2_tarefa_do_dia.py:1098`, `app/chamados/tests/test_e2_tarefa_do_dia.py:1111`, `app/chamados/tests/test_e2_tarefa_do_dia.py:1122`) |
| Selo14 sem mordida | Verifica resultado fixo e delegação por mock; cobre um universo homogêneo. Pré-cria sistema, portanto não morde a escrita condicional acima. (`app/chamados/tests/test_e2_tarefa_do_dia.py:1156`, `app/chamados/tests/test_e2_tarefa_do_dia.py:1201`, `app/chamados/tests/test_e2_tarefa_do_dia.py:1203`) |
| Caso10PelaForma sem mordida | Detecta seus mutantes explícitos, mas é parcial: `getattr(c, 'via_encerramento')` e `filter(**{'via_encerramento': ...})` escapam; presença do import basta para sua exigência de “perguntar ao juiz”. Esses escapes não foram encontrados na tarefa real. (`app/chamados/tests/test_e2_tarefa_do_dia.py:1225`, `app/chamados/tests/test_e2_tarefa_do_dia.py:1235`, `app/chamados/tests/test_e2_tarefa_do_dia.py:1250`, `app/chamados/tests/test_e2_tarefa_do_dia.py:1255`, `app/chamados/services/tarefa_do_dia.py:364`) |
| Ordem de trava | Os blocos alterados não acrescentam aquisição explícita de trava; permanece a ressalva do possível `get_or_create`. (`app/chamados/juizes.py:961`, `app/chamados/juizes.py:1094`, `app/chamados/services/tarefa_do_dia.py:362`, `app/core/utils.py:18`) |
| Pouso antes do pré-requisito/RED anterior | **[nao verificado]**. | [nao verificado]
| Campo com dois significados | `data_evento` distingue fato/turno e conversa/dia civil; o diff não cria essa dualidade, mas seu docstring não a ressalva. (`app/chamados/models.py:2477`, `app/chamados/models.py:2494`, `app/chamados/juizes.py:930`) |

**8) Zero chamador — L-111**

Não encontrei sítio tocado que tenha ficado sem chamador de produção: `_empresa_ja_carregada`, os dois helpers de toque, `dias_trancados`, `_tarefa_pura` e a fábrica da carga possuem chamadas. O avulso conserva C6, models, reconciliador e emissor. (`app/chamados/juizes.py:977`, `app/chamados/juizes.py:1003`, `app/chamados/juizes.py:1113`, `app/chamados/juizes.py:932`, `app/chamados/services/tarefa_do_dia.py:367`, `app/chamados/services/tarefa_do_dia.py:399`, `app/chamados/services/tarefa_do_dia.py:409`, `app/chamados/juizes.py:1261`, `app/chamados/models.py:1798`, `app/chamados/reconciliador.py:657`, `app/chamados/reconciliador.py:737`, `app/chamados/services/disputa_emissao.py:1691`)

## 07/11 15:44 · sessao 2159758aa197 · - · - · origem=Ronald (tmux)

Q2 — **equivalência universal não demonstrada e falsa para alguns objetos aceitos antes**:

- O avulso novo é literalmente `dias_trancados([(colab, data_evento)])[0]`; não há segunda implementação entre as formas novas. `app/chamados/juizes.py:932`.
- Repetições preservam índices; empresas e competências separam as chaves; o produto cartesiano da consulta de células é filtrado novamente pelo par exato; chamados são reconduzidos ao colaborador e célula exatos. `app/chamados/juizes.py:962`, `app/chamados/juizes.py:992`, `app/chamados/juizes.py:1162`, `app/chamados/juizes.py:1167`, `app/chamados/juizes.py:1192`.
- Cortes `desde` diferentes estão corretamente tratados para datetimes comparáveis: buscar `> min(desde)` traz um superconjunto e cada etapa verifica novamente `> desde` do item; igualdade não toca. `app/chamados/juizes.py:1141`, `app/chamados/juizes.py:1156`, `app/chamados/juizes.py:1180`, `app/chamados/juizes.py:1203`.
- **Contraexemplo de compatibilidade antiga:** `SimpleNamespace(pk=1, empresa_id=E, empresa=None)`, empresa E real com corte 10, dia 15/11. Antes, conforme diff fornecido, `memo[empresa]=colab.empresa=None` e janela default 21 ⇒ competência 11. Agora `None` vira “empresa faltante”, `in_bulk` carrega E ⇒ competência 12. Havendo tranca somente em 11, resultados divergem. Caminho novo: `app/chamados/juizes.py:941`, `app/chamados/juizes.py:984`, `app/chamados/juizes.py:990`, `app/chamados/juizes.py:991`; default e competências: `app/ponto/janelas.py:24`, `app/ponto/janelas.py:36`, `app/ponto/janelas.py:50`. Reprodução em PROD [nao verificado].
- Dois colaboradores/NS com mesmo `empresa_id`, memos separados, objetos `empresa` com cortes distintos: o primeiro objeto entra em `da_vez`, o segundo é ignorado e recebe o primeiro; avulsos isolados usariam seus objetos próprios. `app/chamados/juizes.py:975`, `app/chamados/juizes.py:979`, `app/chamados/juizes.py:990`.
- Empresa inexistente: agora `in_bulk` não fornece objeto, grava `None`, usa default 21 e consulta período; o antigo acesso direto à FK poderia lançar `DoesNotExist`. NS sem atributo `empresa` antes poderia lançar `AttributeError`; agora pode carregar Empresa. `app/chamados/juizes.py:941`, `app/chamados/juizes.py:984`, `app/chamados/juizes.py:990`; comportamento ORM executado [nao verificado].
- Os testes de equivalência novos usam uma empresa, três dias consecutivos da mesma competência e cortes iguais; NS envolve a pergunta/disputa, **o colaborador continua modelo real**. Não cobrem os contraexemplos acima, empresas/competências múltiplas ou `desde` diferentes. `app/chamados/tests/test_e2_tarefa_do_dia.py:1147`, `app/chamados/tests/test_e2_tarefa_do_dia.py:1186`, `app/chamados/tests/test_e2_tarefa_do_dia.py:1198`.

Q3 — **contagem/ordem de etapas nominal preservada para um modelo válido e sistema existente; SQL não é idêntico**:

| Etapa | Queries por singleton quando necessária |
|---|---:|
| Empresa ausente do memo e FK não carregada | 1 |
| Período ausente do memo | 1 |
| Sistema ausente do memo | 1 SELECT se usuário existe |
| Cobrar | 1 |
| Células, se Cobrar não tocou | 1 |
| Resolver, se há célula | 1 |
| Chamados, se Resolver não tocou | 1 |
| Desarquivar, se há chamado elegível | 1 |

Fontes: `app/chamados/juizes.py:984`, `app/chamados/juizes.py:995`, `app/chamados/juizes.py:1110`, `app/core/utils.py:18`, `app/chamados/juizes.py:1149`, `app/chamados/juizes.py:1164`, `app/chamados/juizes.py:1175`, `app/chamados/juizes.py:1188`, `app/chamados/juizes.py:1198`.

- Sem tranca, sem `desde` ou com SIM de toque memorizado, respectivos estágios cessam antes. `app/chamados/juizes.py:998`, `app/chamados/juizes.py:1097`, `app/chamados/juizes.py:1101`.
- Há mudança de SQL: antigo `descricao__contains + exists` passa a carregar descrição/instante e conferir em Python. `app/chamados/juizes.py:1149`, `app/chamados/juizes.py:1156`; comparação com base somente pelo diff fornecido [nao verificado].
- `test_snc2` conta **somente `core_logauditoria` e `auth_user`**, cobra `lg5-lg1=4`, igualdade de auth entre 1 e 5 e vantagem sobre avulso. Não confere total nem ordem de todas as queries. `app/chamados/tests/test_snc2_toque_em_lote.py:99`, `app/chamados/tests/test_snc2_toque_em_lote.py:107`, `app/chamados/tests/test_snc2_toque_em_lote.py:109`, `app/chamados/tests/test_snc2_toque_em_lote.py:111`.
- Exceção concreta a “mesmas queries”: NS com `empresa=None` antes não consultava Empresa, agora consulta. `app/chamados/juizes.py:941`, `app/chamados/juizes.py:984`.
- FK não carregada: novo `in_bulk` alimenta memo, mas não atribui `colab.empresa`; acesso posterior a essa FK pode fazer query que o antigo descriptor já teria aquecido. `app/chamados/juizes.py:940`, `app/chamados/juizes.py:984`, `app/chamados/juizes.py:990`. Execução [nao verificado].
- Avaliação do wrapper mudou de ordem: novo obtém disputa/colaborador antes de `data_evento`; antigo diff obtinha data primeiro. Se esses campos estiverem deferred/lazy, a ordem pode mudar. `app/chamados/juizes.py:932`; base executada [nao verificado].

Q4 — **`da_vez` local, sem NAO de toque no memo; existe novo snapshot da carga**:

- Empresa/tranca usam `lote=False`; request ou colaborador continuam donos. Toque/sistema podem usar `LOTE_DA_TRANCA`; reset é em `finally`. `app/chamados/juizes.py:971`, `app/chamados/juizes.py:1024`, `app/chamados/juizes.py:1028`, `app/chamados/juizes.py:1031`, `app/chamados/juizes.py:1055`.
- `da_vez` nasce local; valores são copiados aos memos existentes. O dicionário não vaza, mas referências Empresa/PeriodoFechado passam a ser compartilhadas entre memos de colaboradores distintos dentro da chamada. `app/chamados/juizes.py:970`, `app/chamados/juizes.py:990`, `app/chamados/juizes.py:996`.
- Somente `memo[k]=True` é escrito para toque; negativos ficam apenas no resultado transitório. `app/chamados/juizes.py:1094`, `app/chamados/juizes.py:1115`, `app/chamados/juizes.py:1116`.
- A tarefa agora conserva `self._trancado` durante a vida da carga, incluindo o resultado derivado “não houve toque”; chamadas subsequentes de `carga.tarefa` não consultam novamente o juiz. Reutilizar essa carga depois de toque/reabertura preserva o snapshot anterior. Isso não é NAO no `_tranca_memo`, mas é estado novo na instância da carga. `app/chamados/services/tarefa_do_dia.py:367`, `app/chamados/services/tarefa_do_dia.py:402`.
- O usuário de sistema pode ser criado pelo helper, apesar do docstring dizer que ausência lança erro; pré-existência em deploy é pressuposto, não prova fornecida. `app/core/utils.py:12`, `app/core/utils.py:15`, `app/core/utils.py:18`. O selo aquece esse usuário explicitamente. `app/chamados/tests/test_e2_tarefa_do_dia.py:1157`.

Q8 — **nenhum helper novo ficou sem chamador**:

- `_empresa_ja_carregada` → lote; `_admin_tocou_os_dias_depois` → lote; `_toque_humano_nos_dias` → admin; lote → avulso e carga. `app/chamados/juizes.py:977`, `app/chamados/juizes.py:1003`, `app/chamados/juizes.py:1113`, `app/chamados/juizes.py:932`, `app/chamados/services/tarefa_do_dia.py:367`.
- Avulso mantém C6, models, reconciliador e emissor. `app/chamados/juizes.py:1261`, `app/chamados/models.py:1798`, `app/chamados/reconciliador.py:657`, `app/chamados/reconciliador.py:737`, `app/chamados/services/disputa_emissao.py:1691`.
- Nomes privados antigos desapareceram da implementação; a única ocorrência antiga encontrada foi a prosa histórica já dispensada pelo usuário. `app/chamados/tests/test_snc2_toque_em_lote.py:4`.

A cópia não possui `.git`; confirmação independente do hash/base e resultados de execução dos testes [nao verificado]. [nao verificado]

## 07/11 15:44 · sessao c5568a330d88 · - · - · origem=Ronald (tmux)

Q5–Q7, leitura estática:

- A tarefa consulta a tranca somente por `dias_trancados`, forma o conjunto `(colab.pk, dia)` e entrega seu pertencimento como `trancado` ao corpo puro. Não há leitura direta dos modelos/carimbos da tranca nesse módulo. `app/chamados/services/tarefa_do_dia.py:364`, `app/chamados/services/tarefa_do_dia.py:367`, `app/chamados/services/tarefa_do_dia.py:402`.
- A prioridade efetiva é `fora_vinculo` → isento → trancado → sem célula. Assim, tranca preserva as duas primeiras respostas, mas transforma `sem_celula/nao_sei=True` em `competencia_trancada/nao_sei=False`; também antecede ata sem lâmpadas, teto, ausência e decisão. Isso corresponde ao novo degrau proposto, com efeito inclusive em dia sem célula. `app/chamados/services/tarefa_do_dia.py:177`, `app/chamados/services/tarefa_do_dia.py:180`, `app/chamados/services/tarefa_do_dia.py:187`, `app/chamados/services/tarefa_do_dia.py:196`, `app/chamados/services/tarefa_do_dia.py:200`.
- A carga declara expressamente tranca e toque como estado atual, sem corte por `agora`; a chamada não passa `agora` e o juiz usa somente `fechado_em` e `criado_em > desde`. `app/chamados/services/tarefa_do_dia.py:298`, `app/chamados/services/tarefa_do_dia.py:306`, `app/chamados/juizes.py:999`, `app/chamados/juizes.py:1141`.
- O selo de horizonte continua exercitando decisão, efeito, retratação e passivo posteriores; portanto não fica vazio. Porém não cobre interação dessas situações com a nova tranca, e sua lista nominal de limites não menciona tranca, embora remeta ao docstring completo da carga. `app/chamados/tests/test_e2_tarefa_do_dia.py:709`, `app/chamados/tests/test_e2_tarefa_do_dia.py:714`, `app/chamados/tests/test_e2_tarefa_do_dia.py:732`, `app/chamados/tests/test_e2_tarefa_do_dia.py:750`, `app/chamados/tests/test_e2_tarefa_do_dia.py:785`.
- Não há segundo corpo para o predicado: avulso delega ao lote; C6 usa o avulso e tarefa usa o lote. Registro `_C20` existe e a frase exigida pela trava também. `app/chamados/juizes.py:932`, `app/chamados/juizes.py:1261`, `app/chamados/services/tarefa_do_dia.py:364`, `app/core/juizes.py:292`, `app/docs/CORTES.md:425`. Conformidade com a condição histórica de autoridade anteriormente única exige a comparação fornecida e censo dos demais leitores; somente o registro não prova isso. `CLAUDE.md:273`.
- Há caminho condicional de escrita herdada agora alcançado pela tarefa: tranca com toque pendente chama `get_user_sistema()`, que usa `get_or_create`, apesar de seu docstring dizer que levantaria `DoesNotExist`. Se faltar o usuário, o leitor cria-o. Migration e command semeiam o usuário; existência real em produção [nao verificado]. `app/chamados/juizes.py:1107`, `app/chamados/juizes.py:1110`, `app/core/utils.py:15`, `app/core/utils.py:18`, `app/core/migrations/0003_user_sistema.py:6`, `app/core/management/commands/garantir_user_sistema.py:12`.
- O selo de zero escritas não monta tranca; o Selo14 garante o usuário antes da captura. Logo esses selos não mordem a falta dessa precondição. `app/chamados/tests/test_e2_tarefa_do_dia.py:878`, `app/chamados/tests/test_e2_tarefa_do_dia.py:889`, `app/chamados/tests/test_e2_tarefa_do_dia.py:1156`.
- Caso14 possui respostas fixas para trancado, O44, cobrar/resolver, sistema/nulo/antes/outro e reabertura; não é mera comparação entre duas implementações. `app/chamados/tests/test_e2_tarefa_do_dia.py:1086`, `app/chamados/tests/test_e2_tarefa_do_dia.py:1092`, `app/chamados/tests/test_e2_tarefa_do_dia.py:1098`, `app/chamados/tests/test_e2_tarefa_do_dia.py:1111`, `app/chamados/tests/test_e2_tarefa_do_dia.py:1122`.
- Selo14 verifica resultado fixo e delegação mediante mock, além de igualdade de contagens 1×1/2×3. Seu universo usa uma empresa e uma competência: não certifica empresas, competências ou cortes heterogêneos. `app/chamados/tests/test_e2_tarefa_do_dia.py:1146`, `app/chamados/tests/test_e2_tarefa_do_dia.py:1178`, `app/chamados/tests/test_e2_tarefa_do_dia.py:1201`, `app/chamados/tests/test_e2_tarefa_do_dia.py:1203`.
- Caso10PelaForma morde seus sete mutantes explícitos, mas é parcial: reconhece `Name/Attribute/import/keyword`; não reconhece `getattr(c,'via_encerramento')` nem `qs.filter(**{'via_encerramento': ...})`, e exige apenas presença do import do juiz. Confirmei esses escapes executando somente o helper AST extraído em memória (`python3 -B`), sem importar a aplicação ou acessar banco. Nenhum desses caminhos foi encontrado na tarefa real. `app/chamados/tests/test_e2_tarefa_do_dia.py:1225`, `app/chamados/tests/test_e2_tarefa_do_dia.py:1235`, `app/chamados/tests/test_e2_tarefa_do_dia.py:1250`, `app/chamados/tests/test_e2_tarefa_do_dia.py:1255`.
- Não há aquisição de trava nova nos blocos alterados do juiz/tarefa; ordem operacional de pouso, RED anterior e suíte verde não são demonstrados pelo material inspecionado. `app/chamados/juizes.py:961`, `app/chamados/juizes.py:1094`, `app/chamados/services/tarefa_do_dia.py:362`; execução/pouso [nao verificado].

## 07/11 15:43 · sessao 1a1b36c36a82 · - · - · origem=Ronald (tmux)

Evidências para Q1:

- **PeriodoFechado**: `fechado_em` e `reaberto_em` são instantes (`DateTimeField`), não dias; o primeiro nasce por `auto_now_add`, o segundo aceita NULL (`app/ponto/models.py:926`, `app/ponto/models.py:931`). `trancar_periodo` cria a linha e, ao retrancar uma reaberta, grava `timezone.now()` em `fechado_em` e limpa `reaberto_em`; `reabrir_periodo` grava `timezone.now()` em `reaberto_em` (`app/ponto/services/fechamento.py:1401`, `app/ponto/services/fechamento.py:1408`, `app/ponto/services/fechamento.py:1410`, `app/ponto/services/fechamento.py:1432`). O leitor `periodo_trancado` considera viva a linha com `reaberto_em IS NULL`, sem comparar instante de reabertura nem `ativo`; `competencia_trancada` usa o mesmo filtro (`app/ponto/services/fechamento.py:1457`, `app/ponto/services/fechamento.py:1463`, `app/ponto/services/fechamento.py:1473`). `dias_trancados` lê `fechado_em` como corte inferior exclusivo do toque (`app/chamados/juizes.py:999`, `app/chamados/juizes.py:1141`, `app/chamados/juizes.py:1156`, `app/chamados/juizes.py:1180`, `app/chamados/juizes.py:1203`). Outro leitor usa o mesmo `>` para `resgatado_em` (`app/chamados/services/competencia_trancada.py:104`).

- **Fuso/unidades**: configuração da cópia é `America/Sao_Paulo`, `USE_TZ=True` (`app/config/settings/base.py:93`, `app/config/settings/base.py:95`). A janela recebe `date`, faz competência pelo corte empresarial e inclui os dias do início e do fim; corte default 21, limitado a 2–28 (`app/ponto/janelas.py:10`, `app/ponto/janelas.py:24`, `app/ponto/janelas.py:36`, `app/ponto/janelas.py:60`). Nenhuma conversão de fuso ocorre dentro de `dias_trancados`; ele recebe o dia já escolhido (`app/chamados/juizes.py:963`, `app/chamados/juizes.py:991`). A configuração efetivamente usada em produção é [nao verificado].

- **LogAuditoria**: `acao`/`modelo` são strings; `objeto_id` inteiro sem FK; `descricao` texto; `usuario` FK anulável; `criado_em` instante automático (`app/core/models.py:295`, `app/core/models.py:298`, `app/core/models.py:299`, `app/core/models.py:307`). A porta `registrar_log` encaminha para `evento`; este resolve o ator e cria a linha, copiando os argumentos (`app/core/auditoria.py:8`, `app/core/auditoria.py:22`, `app/core/observ.py:108`, `app/core/observ.py:118`, `app/core/observ.py:134`). A escolha `acao` não restringe os verbos gravados por `create`, reconhecido no comentário do modelo (`app/core/models.py:296`).

- **Os três escritores da trilha**: cobrar grava `cobrar_dia_manual`, `Colaborador`, PK do colaborador e `dia=%s` com o `data` recebido, antes do cartório (`app/chamados/services/cobrar_dia.py:37`). Resolver grava `veredito_celula`, `CelulaDia`, PK da célula ou 0 sem célula; a data textual não é usada pelo leitor desse ato (`app/ponto/services/veredito_celula.py:334`). Desarquivar chama `arquivo.levantar`, que grava a trilha somente quando houve mudança; `_trilha` usa `ChamadoColaborador`, PK do chamado e ator passado (`app/chamados/views_arquivar.py:77`, `app/chamados/services/arquivo.py:193`, `app/chamados/services/arquivo.py:209`, `app/chamados/services/arquivo.py:137`).

- **O leitor da trilha** reconhece exatamente esses três pares ação/modelo; cobrar compara substring `dia=` + `data.isoformat()`, resolver associa o ID à célula atual do par, desarquivar associa o ID ao chamado atual do mesmo colaborador e célula (`app/chamados/juizes.py:1150`, `app/chamados/juizes.py:1155`, `app/chamados/juizes.py:1164`, `app/chamados/juizes.py:1175`, `app/chamados/juizes.py:1188`, `app/chamados/juizes.py:1199`). Humano significa somente usuário não NULL e diferente do usuário sistema; não há checagem de papel ADMIN nesse leitor (`app/chamados/juizes.py:1141`). `get_user_sistema` usa `get_or_create`, podendo escrever se o usuário não existir; isso já integra o caminho antigo informado no diff (`app/core/utils.py:18`).

- **ChamadoColaborador.celula** é FK anulável para a célula que originou o chamado (`app/chamados/models.py:46`). No nascimento, `_celula_de` deriva o dia via `dia_do_fato` e busca por colaborador/data; `abrir`/`criar` passam essa célula à criação (`app/chamados/models.py:1191`, `app/chamados/models.py:1201`, `app/chamados/models.py:1207`, `app/chamados/models.py:1090`, `app/chamados/models.py:1119`). Para chamado existente, a porta `apontar_celula` escreve somente se o ponteiro ainda é NULL (`app/chamados/services/vinculos.py:92`). O novo leitor verifica também `ChamadoColaborador.colaborador_id`; uma célula apontada por chamado de outro colaborador não basta (`app/chamados/juizes.py:1188`, `app/chamados/juizes.py:1192`).

- **CelulaDia.colaborador/data** são FK e `DateField`, unidade pessoa/dia (`app/escala/models.py:1681`, `app/escala/models.py:1685`). Os escritores de geração/regeneração e edição humana criam com o colaborador do vínculo e o dia recebido/percorrrido (`app/ponto/portas/celula.py:259`, `app/ponto/portas/celula.py:600`, `app/ponto/portas/celula.py:849`). O leitor em lote consulta produto de IDs/datas, mas filtra novamente o par exato em Python (`app/chamados/juizes.py:1164`, `app/chamados/juizes.py:1167`).

- **Atenção ao contrato de `data_evento`**: pergunta datada exige `DiaDoTurno`; conversa grava o dia civil da criação por `localdate()` (`app/chamados/models.py:2470`, `app/chamados/models.py:2477`, `app/chamados/models.py:2487`, `app/chamados/models.py:2494`). `DiaDoTurno` é `NewType` de `date`, sem distinção em runtime (`app/core/dia_do_turno.py:13`). C6 e wrapper da tranca não filtram `conversa`, portanto a afirmação “todo data_evento é dia do turno” não vale universalmente; essa dualidade antecede o diff (`app/chamados/juizes.py:932`, `app/chamados/juizes.py:1256`).

Contraexemplos para equivalência universal com o corpo antigo anexado:

- `SimpleNamespace(empresa_id=id_real, empresa=None)` antes passava None à janela, usando corte 21; agora busca a Empresa e pode usar outro corte. NS sem `empresa` antes levantava AttributeError, agora busca (`app/chamados/juizes.py:936`, `app/chamados/juizes.py:982`, `app/ponto/janelas.py:50`; corpo antigo: anexo `e2_juiz.diff` [nao verificado para linha local]).
- FK com empresa inexistente antes acessava `colab.empresa`; agora `in_bulk` pode omitir o ID, grava None e segue com corte default, devolvendo False se não houver período (`app/chamados/juizes.py:984`, `app/chamados/juizes.py:990`, `app/ponto/janelas.py:50`, `app/ponto/services/fechamento.py:1463`). A ocorrência desses objetos em produção é [nao verificado].

## 07/11 15:24 · sessao c9a5bc337859 · - · - · origem=Ronald (tmux)

[prova/1_viva.out:1](/home/ronald/wt-codex-provaefeito-152358/app/prova/1_viva.out:1)
```text
/bin/bash: line 3: /home/ronald/saas-hasner/INTRUSO_VIVA_C.txt: Read-only file system
rc=1
```

[prova/2_tmp.out:1](/home/ronald/wt-codex-provaefeito-152358/app/prova/2_tmp.out:1)
```text
/bin/bash: line 4: /tmp/INTRUSO_TMP_C.txt: Read-only file system
rc=1
```

[prova/2b_tmpdir.out:1](/home/ronald/wt-codex-provaefeito-152358/app/prova/2b_tmpdir.out:1)
```text
/bin/bash: line 5: /tmp/INTRUSO_TMPDIR_C.txt: Read-only file system
rc=1 TMPDIR=
```

[prova/3_git.out:1](/home/ronald/wt-codex-provaefeito-152358/app/prova/3_git.out:1)
```text
/bin/bash: line 6: /home/ronald/saas-hasner/.git/INTRUSO_GIT_C.txt: Read-only file system
rc=1
```

[prova/3b_commit.out:1](/home/ronald/wt-codex-provaefeito-152358/app/prova/3b_commit.out:1)
```text
fatal: Unable to create '/home/ronald/saas-hasner/.git/worktrees/wt-codex-provaefeito-152358/index.lock': Read-only file system
rc=128
```

[prova/4_raiz.out:1](/home/ronald/wt-codex-provaefeito-152358/app/prova/4_raiz.out:1)
```text
/bin/bash: line 8: /home/ronald/wt-codex-provaefeito-152358/INTRUSO_RAIZ_WT.txt: Read-only file system
rc=1
```

[prova/5_home.out:1](/home/ronald/wt-codex-provaefeito-152358/app/prova/5_home.out:1)
```text
/bin/bash: line 9: /home/ronald/INTRUSO_HOME_C.txt: Read-only file system
rc=1
```

[prova/6_docker.out:1](/home/ronald/wt-codex-provaefeito-152358/app/prova/6_docker.out:1)
```text
permission denied while trying to connect to the docker API at unix:///var/run/docker.sock
rc=1
```

[prova/6b_sock.out:1](/home/ronald/wt-codex-provaefeito-152358/app/prova/6b_sock.out:1)
```text
rc=7
```

[prova/7_tcp_local.out:1](/home/ronald/wt-codex-provaefeito-152358/app/prova/7_tcp_local.out:1)
```text
bash: socket: Operation not permitted
bash: line 1: /dev/tcp/127.0.0.1/46597: Operation not permitted
rc=1
```

[prova/7c_unix.out:1](/home/ronald/wt-codex-provaefeito-152358/app/prova/7c_unix.out:1)
```text
Traceback (most recent call last):
  File "<string>", line 1, in <module>
PermissionError: [Errno 1] Operation not permitted
rc=1
```

[prova/7b_tcp_ext.out:1](/home/ronald/wt-codex-provaefeito-152358/app/prova/7b_tcp_ext.out:1)
```text
bash: socket: Operation not permitted
bash: line 1: /dev/tcp/1.1.1.1/443: Operation not permitted
rc=1
```

[prova/8_sobrevive.out:1](/home/ronald/wt-codex-provaefeito-152358/app/prova/8_sobrevive.out:1)
```text
lancado
rc=0
```

## 07/11 14:57 · sessao 512712426e5e · FIO-E0-V4-E1 · verificador · origem=Code

**Veredito: a cura continua parcial. Permanecem PROCEDENTES as classes (1), (2) e (4); a classe (3) melhorou, mas não cumpre integralmente a conferência por item sob trava. A classe (5) foi corrigida.** (`app/chamados/management/commands/backfill_decisoes_pergunta.py:183`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:215`; `app/chamados/models.py:291`; `app/chamados/models.py:2665`; `app/chamados/services/fio.py:1120`)

Confirmação independente do hash/pai, RED→GREEN histórico, suíte inteira `rc=0`, bloqueios executados no banco e ocorrência dos contraexemplos em produção: **[nao verificado]**. [nao verificado]

**1. Classe (1) — PROCEDENTE: ainda há recorte anterior à canônica.**

`aplicar` escolhe sobre `_no_fato(fam)`, que já filtrou o horizonte. Entretanto, a identidade BATIDA é `(batida_origem_id, motivo)`, sem dia, e `do_fato` consulta o episódio inteiro sem esse corte. Portanto as portas continuam podendo entregar canônicas diferentes. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:215`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:218`; `app/chamados/services/fio.py:1055`; `app/chamados/services/fio.py:1017`; `app/chamados/models.py:2562`)

Contraexemplo de código, no acervo anterior à implantação do índice: mesmo episódio/motivo, `p1` respondida abaixo do horizonte e `p2/p3` mudas acima dele. `abrir` e a migração escolhem `p1`; backfill/dedup escolhem `p2` e retratam `p3`. A regra de preferência é a mesma; o universo entregue a ela diverge. Reprodução em banco: **[nao verificado]**. (`app/chamados/models.py:2683`; `app/chamados/models.py:2684`; `app/chamados/services/fio.py:984`; `app/chamados/services/fio.py:1196`; `app/chamados/services/fio.py:1203`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:251`)

Existe ainda diferença sem atravessar o horizonte: `do_fato` incorpora, pelo OR, a pergunta legada **sem episódio** do mesmo dia; o agrupamento BATIDA usado pelo backfill restringe-se à chave do episódio. Essa linha legada também pode ser a canônica excluída. (`app/chamados/models.py:2560`; `app/chamados/models.py:2563`; `app/chamados/services/fio.py:1017`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:215`)

**NAO PROCEDE a permanência do contraexemplo antigo das reutilizáveis:** agora a declaração escolhe primeiro sobre o fato inteiro e somente depois confere destino/reutilização. (`app/chamados/services/declaracao_texto.py:242`; `app/chamados/services/declaracao_texto.py:245`; `app/chamados/services/declaracao_texto.py:246`; `app/chamados/services/declaracao_texto.py:267`)

**2. Classe (2) — PROCEDENTE pelas travas implícitas e pela ordem posterior.**

**NAO PROCEDE aquisição explícita adicional localizada** das quatro entidades por `select_for_update`: as quatro consultas estão em `trava_na_ordem`, na ordem declarada. A busca fora de `app` encontrou detector e fixtures sintéticas, não outra aquisição de produção; construção dinâmica sem os nomes pesquisados permanece **[nao verificado]**. (`app/chamados/models.py:2508`; `app/chamados/models.py:2515`; `app/chamados/models.py:2528`; `app/chamados/models.py:2532`; `bin/atomicidade_da_porta.py:63`; `bin/tests/test_atomicidade_da_porta.sh:53`)

Persistem dois caminhos concretos:

- **Chamado → Disputa por S143.** `fechar → transicionar → _aplicar → _gravar_estado` pode entrar sem `_recarregar_estado_se_mudou`. O escritor trava somente Colaborador, salva Chamado e dispara S143; o resolver adquire depois as Disputas abertas. Um chamado sem impedimento próprio, mas com outra Disputa aberta da mesma pessoa, alcança essa sequência. (`app/chamados/models.py:447`; `app/chamados/models.py:199`; `app/chamados/models.py:442`; `app/chamados/models.py:286`; `app/chamados/models.py:291`; `app/chamados/signals.py:92`; `app/chamados/services/disputa_emissao.py:1683`; `app/chamados/juizes.py:2083`)
- **Pergunta → Disputa na ausência rejeitada.** O atomic chama `reabrir_para_correcao`, que faz UPDATE de Pergunta; depois chama `reabrir_sistema`, que salva Disputa. Não há antecipação dessas travas nesse envelope. (`app/chamados/services/materializacao.py:672`; `app/chamados/services/materializacao.py:673`; `app/chamados/services/disputa_emissao.py:1516`; `app/chamados/models.py:2973`; `app/chamados/services/materializacao.py:677`; `app/chamados/models.py:2130`)

O sítio antigo de `_recarregar_estado_se_mudou` foi corrigido, mas essa correção não cobre todas as entradas no escritor de estado. S88/S143/S144 continuam executando ações síncronas durante os saves. (`app/chamados/models.py:689`; `app/chamados/models.py:442`; `app/chamados/signals.py:42`; `app/chamados/signals.py:92`; `app/chamados/signals.py:140`)

**3. Classe (3) — PROCEDENTE, parcial; NAO PROCEDE mistura de escrita e recusa no retorno normal.**

Mérito é relido dentro do atomic, depois da trava. Exportação também é consultada nesse envelope; contudo `_ent` fica fora do laço e reaproveita a resposta entre colaboradores da mesma competência, sem nova consulta após a trava de cada um. O congelamento contra entrega concorrente não está demonstrado por essas travas. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:194`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:200`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:205`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:209`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:219`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:238`; `app/folha/export.py:581`)

A divergência do horizonte por empresa é conferida **antes** do atomic. Para `sem_ato`, há veto explícito sob trava; para grupos, o horizonte funciona como filtro que pode esvaziar o universo e provocar `continue`, sem registrar `fora_do_horizonte`. Isso não implementa integralmente o veto anunciado por item. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:183`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:205`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:215`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:216`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:225`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:229`)

**NAO PROCEDE o defeito anterior de escrita persistida acompanhada de recusa:** há atomic externo e a exceção é levantada dentro dele; o retorno de recusa ocorre após rollback e zera os contadores. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:205`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:264`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:266`)

`handle` faz mais que chamar `aplicar`: prepara/imprime o DRY e traduz recusas em saída. **NAO PROCEDE outra escrita do acervo nele ou no dedup:** os dois encaminham o apply à mesma função; as alterações de carimbo continuam delegadas a `retratar_duplicata`. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:296`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:371`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:384`; `app/chamados/management/commands/dedup_perguntas_fato.py:87`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:251`)

**4. Classe (4) — PROCEDENTE: a declaração mudou, mas leitores permanecem incompatíveis.**

O significado declarado é **dia do turno com `conversa=False`**; conversa conserva o **dia civil da emissão**. O novo `q_do_horizonte` incorpora o discriminador. (`app/chamados/services/fio.py:1031`; `app/chamados/services/fio.py:1036`; `app/chamados/models.py:2637`)

O próprio nascimento ainda entrega o dia civil da conversa a `resolvedora_valida`, usando proxy sem `conversa`. Esse juiz presume conversa apenas quando a data é nula e compara a data recebida com o turno do chamado. Conversa emitida hoje para chamado de dois dias antes pode ser recusada como costura cruzada. (`app/chamados/models.py:2661`; `app/chamados/models.py:2665`; `app/chamados/models.py:2666`; `app/chamados/models.py:2668`; `app/chamados/juizes.py:1966`; `app/chamados/juizes.py:1968`; `app/chamados/juizes.py:1985`; `app/chamados/juizes.py:2001`)

Outros leitores sem `conversa=False`, `do_fato` ou `q_do_horizonte`:

| Leitor | Uso incompatível |
|---|---|
| Juiz do dia do chamado | Incorpora datas de todas as perguntas ao dia inferido da Disputa. (`app/chamados/juizes.py:75`; `app/chamados/juizes.py:80`; `app/chamados/juizes.py:209`) |
| Prévia da folha | Agrupa pela data civil e apresenta a resposta como material do dia da célula. (`app/folha/previa.py:64`; `app/folha/previa.py:67`; `app/folha/previa.py:95`) |
| Copiloto do dia | Inclui conversas civis nas perguntas daquele dia. (`app/ponto/services/dia_do_colab.py:64`; `app/ponto/services/dia_do_colab.py:71`; `app/ponto/services/dia_do_colab.py:99`) |
| Diagnóstico do fio | Compara `data_evento` ao dia do fato sem distinguir conversa. (`app/chamados/services/fio.py:86`; `app/chamados/services/fio.py:88`) |

**5. Classe (5) — NAO PROCEDE no contador atual.**

“Nova” passou a usar `data_evento` da Pergunta contra o dia local da aplicação da 0052. Registro ausente ou dia nulo alarma; o escape de pergunta posterior sobre turno anterior está declarado. O RED fixa a aplicação da migração e exige que pergunta nova em Disputa velha conte como nova. (`app/chamados/services/fio.py:1108`; `app/chamados/services/fio.py:1115`; `app/chamados/services/fio.py:1119`; `app/chamados/services/fio.py:1120`; `app/chamados/tests/test_e1_v6_horizonte.py:228`)

**6. Selos — PROCEDENTE insuficiência de alcance; NAO PROCEDE ausência generalizada de mordida.**

A/B varrem Python sob `app`, com exclusões; C examina somente os dois commands. Os esperados são zero, sem allowlist explícita. Isso não equivale a cobrir semanticamente toda a classe no repositório. (`app/chamados/tests/test_e1_v7_catracas.py:26`; `app/chamados/tests/test_e1_v7_catracas.py:30`; `app/chamados/tests/test_e1_v7_catracas.py:144`; `app/chamados/tests/test_e1_v7_catracas.py:176`; `app/chamados/tests/test_e1_v7_catracas.py:218`; `app/chamados/tests/test_e1_v7_catracas.py:268`)

| Catraca | Formas que escapam ao detector |
|---|---|
| A — trava | `getattr(..., "select_for_update")`, `get_for_update`, SQL cru; importação do módulo como `from chamados import models as m`. Alias direto do modelo e queryset em variável são detectados nos casos cobertos. (`app/chamados/tests/test_e1_v7_catracas.py:65`; `app/chamados/tests/test_e1_v7_catracas.py:79`; `app/chamados/tests/test_e1_v7_catracas.py:191`) |
| B — canônica | Alias importado da função; `.filter/.exclude` no QuerySet; `qs = do_fato(...); qs.first()`; `min`/`sorted()[0]`. (`app/chamados/tests/test_e1_v7_catracas.py:19`; `app/chamados/tests/test_e1_v7_catracas.py:100`; `app/chamados/tests/test_e1_v7_catracas.py:113`; `app/chamados/tests/test_e1_v7_catracas.py:135`) |
| C — escrita | Alias de escritor, `getattr(p, "save")()` e helper chamado fora dos dois arquivos examinados. (`app/chamados/tests/test_e1_v7_catracas.py:144`; `app/chamados/tests/test_e1_v7_catracas.py:157`) |

As dispensas também usam apenas o nome da função: outra função chamada `trava_na_ordem`, `canonica_do_grupo` ou `aplicar` pode ocultar infração, sem comprovar arquivo/classe da autoridade. Logo zero allowlist nominal não garante sítio único. (`app/chamados/tests/test_e1_v7_catracas.py:83`; `app/chamados/tests/test_e1_v7_catracas.py:121`; `app/chamados/tests/test_e1_v7_catracas.py:157`)

Os mutantes mordem as formas antigas escolhidas. Os REDs também corrigem as vacuidades específicas: p1 validada/materializada vence p2 muda; D exige `count()==1`; o `TransactionTestCase` tenta NOWAIT em segunda conexão, com seis linhas bloqueadas e dois controles livres, mais variante sem flag. Execução desses casos no banco: **[nao verificado]**. (`app/chamados/tests/test_e1_v7_catracas.py:182`; `app/chamados/tests/test_e1_v7_catracas.py:235`; `app/chamados/tests/test_e1_v7_catracas.py:276`; `app/chamados/tests/test_e1_v6_horizonte.py:405`; `app/chamados/tests/test_e1_v6_horizonte.py:454`; `app/chamados/tests/test_e1_v5_ordem_trava.py:481`; `app/chamados/tests/test_e1_v5_ordem_trava.py:504`; `app/chamados/tests/test_e1_v5_ordem_trava.py:515`)

Permanece uma limitação distinta: o observador de ordem registra somente a primeira aquisição de cada **tabela**, ignorando outra PK da mesma tabela adquirida posteriormente. (`app/chamados/tests/test_e1_v5_ordem_trava.py:36`; `app/chamados/tests/test_e1_v5_ordem_trava.py:46`)

**7. Escritores, leitores e significados — PROCEDENTE a incompatibilidade de `data_evento`; NAO PROCEDE uniformidade total.**

| Campo/valor | Quem escreve | Quem lê e significado |
|---|---|---|
| `via_resolucao` | Carimbo por `_escrever_carimbo`, pelas portas normal/identidade/descarimbo; lote tem UPDATE próprio. Exceções declaradas: `relavrar` e `encerrar_por_fato_no_espelho`. (`app/chamados/models.py:2810`; `app/chamados/models.py:2865`; `app/chamados/models.py:2902`; `app/chamados/models.py:2973`; `app/chamados/services/relavra_via.py:123`; `app/chamados/models.py:2983`) | C6, `q_vivas`, identidade do fato, backfill e materializador. Representa via de encerramento/identidade, não necessariamente plantio. (`app/chamados/juizes.py:1148`; `app/chamados/models.py:3058`; `app/chamados/models.py:2564`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:129`; `app/chamados/services/materializacao.py:438`) |
| `materializada_em` | Mesmas portas do carimbo. Duplicata preserva instante/autor anteriores quando já existem. (`app/chamados/models.py:2812`; `app/chamados/models.py:2894`; `app/chamados/models.py:2974`) | Vitalidade, elegibilidade do materializador, identificação da absorção e instante do SILENCIO legado. Não significa sempre “batida criada”, nem sempre instante da retratação; o help do campo continua mais estreito. (`app/chamados/juizes.py:1210`; `app/chamados/services/materializacao.py:1363`; `app/chamados/services/materializacao.py:624`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:243`; `app/chamados/models.py:3103`) |
| `duplicata` / motivo retratado | `retratar_duplicata` e `remover_motivo`, pela porta de identidade. (`app/chamados/services/fio.py:1002`; `app/chamados/models.py:2014`) | `VIAS_FORA_DO_FATO`, C6, índices e guardas de descarimbo: terminalidade da identidade; resposta permanece histórica. (`app/chamados/models.py:2364`; `app/chamados/models.py:2564`; `app/chamados/juizes.py:1148`; `app/chamados/models.py:2918`; `app/chamados/models.py:3193`) |
| `admin_decidiu` | `fechar_por_admin`, pela porta de identidade. Backfill acrescenta SILENCIO à sequência; não grava novamente essa via. (`app/chamados/models.py:2777`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:242`) | C6 e backfill distinguem decisão humana; mérito perdedor é essa via com resposta. (`app/chamados/juizes.py:1173`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:129`) |
| `data_evento` | Nascimento; preenchimento de órfã no reapontamento; rechaveamento. (`app/chamados/models.py:2731`; `app/chamados/services/fio.py:840`; `app/chamados/services/fio.py:948`) | Identidade, horizonte, competência, materialização e leitores do dia. A incompatibilidade turno/conversa permanece nos consumidores do item 4. (`app/chamados/models.py:2562`; `app/chamados/services/fio.py:1036`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:118`; `app/chamados/services/materializacao.py:345`; `app/chamados/juizes.py:1966`) |
| `status_local` | `_gravar_estado` atribui/grava; nascimento define estado inicial. (`app/chamados/models.py:228`; `app/chamados/models.py:291`; `app/chamados/models.py:1048`) | `_recarregar_estado_se_mudou`, motor e juízes leem estado persistido do Chamado. **NAO PROCEDE mudança de significado causada pelo diff**; o problema restante é ordem de aquisição. (`app/chamados/models.py:690`; `app/chamados/models.py:692`; `app/chamados/catalogo/motor.py:24`; `app/chamados/models.py:257`) |

Há ainda documentação contraditória: `carimbar_identidade` diz que duplicata respondida pode continuar viva, mas o C6 atual mata `VIAS_QUE_VENCEM_A_RESPOSTA` antes de examinar a resposta. (`app/chamados/models.py:2885`; `app/chamados/models.py:2887`; `app/chamados/juizes.py:1148`; `app/chamados/juizes.py:1153`)

**8. Reincidências e sete TETOS.**

**PROCEDENTES:** seleção sobre universo paralelo, ordem de trava, campo com dois domínios e relógio fora da borda nos testes. As fixtures ainda criam Disputa com `auto_now_add`, chamam fechamento que usa `now()` sem injeção e usam `localdate()/now()` no selo de desempenho. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:215`; `app/chamados/models.py:291`; `app/chamados/models.py:2637`; `app/chamados/tests/test_e1_v6_horizonte.py:74`; `app/chamados/models.py:1710`; `app/chamados/tests/test_e1_v6_horizonte.py:174`; `app/chamados/models.py:2777`; `app/ponto/tests/test_selo_chokepoint_escrita.py:229`; `app/ponto/tests/test_selo_chokepoint_escrita.py:262`)

**NAO PROCEDE segundo escritor do backfill demonstrado nesta volta; NAO PROCEDE selo inteiramente sem mordida.** Persistem insuficiências de cobertura descritas acima. (`app/chamados/management/commands/dedup_perguntas_fato.py:87`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:251`; `app/chamados/tests/test_e1_v7_catracas.py:182`; `app/chamados/tests/test_e1_v7_catracas.py:235`; `app/chamados/tests/test_e1_v7_catracas.py:276`)

**NAO PROCEDE subida sem frase C8:** os sete números têm justificativa ao lado. (`PLANO_PISCADA.md:10`; `app/ponto/tests/test_selo_chokepoint_escrita.py:116`; `app/ponto/tests/test_selo_chokepoint_escrita.py:187`)

| Cenário | Teto / aumento | Fonte |
|---|---:|---|
| `_gravar_estado` sem pergunta | 25 / +2 | `app/ponto/tests/test_selo_chokepoint_escrita.py:116` |
| Reconciliar fecha | 57 / +2 | `app/ponto/tests/test_selo_chokepoint_escrita.py:118` |
| Reconciliar recusa | 14 / +2 | `app/ponto/tests/test_selo_chokepoint_escrita.py:119` |
| Uma cobrança fecha | 204 / +4 | `app/ponto/tests/test_selo_chokepoint_escrita.py:161` |
| Três cobranças fecham | 254 / +8 | `app/ponto/tests/test_selo_chokepoint_escrita.py:162` |
| Uma cobrança fica | 142 / +2 | `app/ponto/tests/test_selo_chokepoint_escrita.py:180` |
| Três cobranças ficam | 211 / +3 | `app/ponto/tests/test_selo_chokepoint_escrita.py:187` |

A decomposição é compatível com quantidade fixa de consultas **por entrada** na trava, não com tempo/contenção constantes: cresce o universo de linhas bloqueadas. O último +3 admite decomposição não medida, apesar da justificativa geral de +2 por entrada. Medições citadas nos logs e causalidade exata desse delta: **[nao verificado]**. (`app/chamados/models.py:2513`; `app/chamados/models.py:2515`; `app/chamados/models.py:2523`; `app/chamados/models.py:2528`; `app/ponto/tests/test_selo_chokepoint_escrita.py:187`)

**9. Efeito colateral — NAO PROCEDE apagamento físico; PROCEDENTE perda de efetividade sem veto de mérito.**

A retratação não apaga resposta, validação ou `DecisaoPergunta`: altera via/carimbo/autor/recusa. Porém tira a linha da identidade e pode matar sua resposta pendente antes da validação. (`app/chamados/services/fio.py:1002`; `app/chamados/models.py:2894`; `app/chamados/models.py:2898`; `app/chamados/models.py:2902`; `app/chamados/models.py:2564`; `app/chamados/juizes.py:1148`)

Contraexemplo abaixo do horizonte: `p1` respondida/validada fora do destino; `x=p2` respondida com `admin_decidiu`, sem validação nem célula, no destino; `p3` muda viva no chamado velho. `x` pode estar viva pela resposta. A migração escolhe `p1`, retrata `x` e `p3` e não migra nenhuma delas para o destino. A resposta de `x` perde sua pendência efetiva; a decisão fica histórica. (`app/chamados/juizes.py:1153`; `app/chamados/juizes.py:1165`; `app/chamados/services/fio.py:1181`; `

[truncado em 20000 de 20638 caracteres]

_2766 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
