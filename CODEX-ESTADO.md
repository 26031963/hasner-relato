# CODEX-ESTADO

> Ultima resposta processada: **12/11/2026 18:10** · publicado em **12/11/2026 18:11** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 12/11 18:10 · sessao 2f0114aa1e5a · TIPO-NA-FONTE · verificador · origem=Code

**Parcial; mantenho a não aprovação como instrumento completo de “todo leitor e escritor”.** O delta cura defeitos concretos, mas introduz perdas silenciosas por sementes de laço e atribuições indevidas de proveniência. A medida também continua sem excluir acessor ou aplicar teto. (`bin/leitores_de_chave.py:169`, `bin/leitores_de_chave.py:286`, `bin/leitores_de_chave.py:695`)

Execução integral do selo, publicação do censo, identidade Git do pouso e comportamento em produção: **[não verificado]**. O selo contém gravações e remoção de temporários; seus casos foram examinados sem executá-lo. (`bin/tests/test_leitores_de_chave.sh:27`, `bin/tests/test_leitores_de_chave.sh:29`, `bin/tests/test_leitores_de_chave.sh:82`)

**1. Destino dos achados da volta 1.**

| Achado anterior | Veredito nesta volta e lugar no delta |
|---|---|
| Briefs chamarem obrigatoriamente a ferramenta; entrega ao construtor/preverificador | **Não curado demonstravelmente.** O selo chama a ferramenta; o disjuntor continua exigindo referências textuais, sem conferir sua saída. Entrega efetiva: **[não verificado]**. (`bin/tests/test_leitores_de_chave.sh:35`; `bin/disjuntor.py:1565`) |
| Universo Python, separação produção/testes, parse inválido, chave ausente, uso errado | **Garantias anteriores mantidas.** Parse e declaração falham antes da saída; consulta sem resultado continua rc 1 nomeado. Isso não prova completude da proveniência. (`bin/leitores_de_chave.py:623`, `bin/leitores_de_chave.py:635`, `bin/leitores_de_chave.py:683`, `bin/leitores_de_chave.py:736`) |
| Apelido de dois ou mais saltos | **Curado para cadeias acíclicas simples; parcial no geral.** `alias()` substitui o corte de um salto, mas ciclos de atribuições válidas podem perder proveniência. (`bin/leitores_de_chave.py:257`, `bin/leitores_de_chave.py:263`, `bin/leitores_de_chave.py:270`, `bin/leitores_de_chave.py:288`) |
| Laço perdia `lamp` e as escritas de subchaves | **Curado no sítio apontado.** Agora o laço transmite `lampadas.[]`, recuperando as escritas de `acesa`, `luz` e `dispensada`. A solução geral é **parcial**, pelos defeitos de escopo abaixo. (`bin/leitores_de_chave.py:168`; `app/ponto/services/cartorio.py:805`, `app/ponto/services/cartorio.py:808`, `app/ponto/services/cartorio.py:809`, `app/ponto/services/cartorio.py:810`) |
| Renomear somente o parâmetro declarado encolhia o censo silenciosamente | **Curado para o mutante exato; parcial como guarda de declaração.** Agora confere o nome, mas aceita também qualquer variável local atribuída com aquele nome. (`bin/leitores_de_chave.py:185`, `bin/leitores_de_chave.py:188`, `bin/leitores_de_chave.py:608`; `bin/tests/test_leitores_de_chave.sh:82`) |
| Reatribuição mantinha proveniência indevida | **Parcial.** Apelidos comuns ambíguos viram lacuna e deixam de contar; isso também apaga acessos válidos anteriores. Nomes semeados continuam ignorando a reatribuição. (`bin/leitores_de_chave.py:267`, `bin/leitores_de_chave.py:273`, `bin/leitores_de_chave.py:286`) |
| Dependência manual de `PARAMS/RETURNS` | **Não curada como fonte paralela de proveniência.** Os mapas continuam alimentando sementes e retornos presumidos; a guarda ficou mais forte, mas não verifica que o valor seja o JSON declarado. (`bin/leitores_de_chave.py:48`, `bin/leitores_de_chave.py:75`, `bin/leitores_de_chave.py:138`, `bin/leitores_de_chave.py:330`, `bin/leitores_de_chave.py:608`) |
| `.ata/.dna`, `getattr` e argumentos nomeados aceitos sem provar classe | **Não curado; agora mais claramente declarado.** Permanecem candidatos sintáticos. (`bin/leitores_de_chave.py:44`, `bin/leitores_de_chave.py:282`, `bin/leitores_de_chave.py:309`, `bin/leitores_de_chave.py:554`) |
| Comprehension sobre lista literal/chamada, chamadas indiretas e consultas ORM | **Não curado.** Lista literal/chamada e ORM continuam explicitamente fora; a resolução de chamadas segue reconhecendo formas específicas. (`bin/leitores_de_chave.py:43`, `bin/leitores_de_chave.py:44`, `bin/leitores_de_chave.py:307`) |
| Contador confundido com persistência/catraca; sem exclusão do acessor ou teto | **Parcial na comunicação; não curado na medida contratada.** O delta explica a unidade e a escrita em memória, mas a seleção continua sendo eventos reconhecidos de topo. (`bin/leitores_de_chave.py:26`, `bin/leitores_de_chave.py:38`, `bin/leitores_de_chave.py:695`) |
| Meta zero retornava erro | **Curado.** Quando consegue medir, imprime inclusive zero e retorna rc 0. Parse, raiz e declaração ainda podem produzir rc 2 antes disso. (`bin/leitores_de_chave.py:675`, `bin/leitores_de_chave.py:683`, `bin/leitores_de_chave.py:687`, `bin/leitores_de_chave.py:697`) |
| Selo ignorava papéis, DNA e rc do contador sintético | **Curados nos casos exercitados.** Agora compara papéis, confere rc e exercita DNA com controle, mutante e vizinha. (`bin/tests/test_leitores_de_chave.sh:130`, `bin/tests/test_leitores_de_chave.sh:131`, `bin/tests/test_leitores_de_chave.sh:145`, `bin/tests/test_leitores_de_chave.sh:164`) |
| Falha de `mktemp` permitia caminhos fora do temporário | **Curado para a falha apontada.** Aborta por falha, vazio ou ausência do diretório antes dos redirecionamentos. (`bin/tests/test_leitores_de_chave.sh:27`, `bin/tests/test_leitores_de_chave.sh:28`, `bin/tests/test_leitores_de_chave.sh:35`) |
| Árvore real só exigia presença mínima; raiz só provava presença | **Não curados.** Continuam mínimo de leitores/escritores, número positivo e existência de três arquivos; não há conferência de completude ou identidade do hash. (`bin/tests/test_leitores_de_chave.sh:23`, `bin/tests/test_leitores_de_chave.sh:46`, `bin/tests/test_leitores_de_chave.sh:54`) |

As constatações anteriores sobre ausência de escrita nos dados da aplicação e relógio apenas de duração permanecem: o instrumento lê fontes; o selo escreve cópias e usa `date` para duração. Acessor, migração e lei dos instantes não são implementados por esses caminhos. (`bin/leitores_de_chave.py:629`, `bin/leitores_de_chave.py:710`; `bin/tests/test_leitores_de_chave.sh:58`, `bin/tests/test_leitores_de_chave.sh:183`; `aval.txt:5`, anexo)

**2. Novos riscos do delta.**

| Mudança | Falso verde / falso vermelho | [nao verificado]
|---|---|
| Guarda `PARAMS` | **Pode aceitar declaração semanticamente falsa:** renomear o parâmetro e deixar um local `ata={}` satisfaz `nomes`; esse local recebe a semente declarada. Presença do nome não prova sua origem. (`bin/leitores_de_chave.py:138`, `bin/leitores_de_chave.py:188`, `bin/leitores_de_chave.py:608`) |
| Guarda `RETURNS` | **Pode aceitar retorno sem JSON:** `return None` e `return 42` satisfazem `retorna`, porque ambos têm expressão AST. Chamadas continuam recebendo raiz presumida pelo mapa. A guarda detecta ausência de função ou de expressão de retorno. (`bin/leitores_de_chave.py:190`, `bin/leitores_de_chave.py:330`, `bin/leitores_de_chave.py:613`) |
| Semeadura de laço | **Produz falsos positivos e negativos.** A semente vale para todo o escopo; até uma semente vazia tem prioridade sobre aliases. Também contorna a verificação de reatribuição. (`bin/leitores_de_chave.py:165`, `bin/leitores_de_chave.py:169`, `bin/leitores_de_chave.py:286`) |
| Alias memoizado | **Resolve a cadeia simples, mas pode perder ciclos válidos:** `a=c.ata; b=a; a=b; a["k"]; b["j"]` perde ambos na reprodução em memória. O vazio devolvido por `busy` participa do resultado memoizado e da classificação como reatribuído. (`bin/leitores_de_chave.py:263`, `bin/leitores_de_chave.py:267`, `bin/leitores_de_chave.py:270`) |
| Reatribuição excluída | **Pode dar falso verde na medida:** `a=c.ata; a["k"]; a={}` apaga o acesso válido anterior. A consulta de `k` pode dar falso negativo; o contador pode diminuir com rc 0. A lacuna não acompanha a saída do contador. (`bin/leitores_de_chave.py:273`, `bin/leitores_de_chave.py:274`, `bin/leitores_de_chave.py:697`, `bin/leitores_de_chave.py:736`) |
| rc 0 quando mede | **Correto para permitir a meta zero; insuficiente como certificado de completude.** Lacunas não impedem rc 0 e o modo não informa sua quantidade. (`bin/leitores_de_chave.py:697`, `bin/leitores_de_chave.py:721`; `aval.txt:5`, anexo) |

O sítio real pedido é **`app/chamados/services/materializacao.py:177` e `:178`**. Os três cortes de string da lambda `_h_ok` passam a aparecer como `orfas.[].<dinamica>` por causa do `for h` **posterior**, em `:181`. O indexador não separa o escopo de lambda/comprehension, e a semeadura aplica o nome à função inteira. Isso é atribuição indevida de proveniência, não mudança física da linha do arquivo. (`app/chamados/services/materializacao.py:177`, `app/chamados/services/materializacao.py:178`, `app/chamados/services/materializacao.py:181`; `bin/leitores_de_chave.py:169`, `bin/leitores_de_chave.py:199`, `bin/leitores_de_chave.py:286`)

Há outro exemplo real: o `x["entrada"]` que percorre `turnos` passa a ser atribuído a `lampadas.[].entrada`, contaminado pelo `x` da comprehension anterior de lâmpadas. (`app/ponto/services/diagnostico_escala.py:154`, `app/ponto/services/diagnostico_escala.py:156`, `app/ponto/services/diagnostico_escala.py:171`; `bin/leitores_de_chave.py:286`)

Dois contraexemplos adicionais reproduzidos em memória delimitam a falha:

- `a=c.ata; a["k"]; for a in []: pass` perde `k` **sem lacuna**: o laço instala semente vazia e impede a resolução do alias. (`bin/leitores_de_chave.py:168`, `bin/leitores_de_chave.py:169`, `bin/leitores_de_chave.py:286`)
- `for a in c.ata["xs"]: pass; a={}; a["k"]` emite indevidamente `xs.[].k`, **sem lacuna**: a semente prevalece sobre os bindings. (`bin/leitores_de_chave.py:169`, `bin/leitores_de_chave.py:267`, `bin/leitores_de_chave.py:286`)

Esses falsos positivos podem tornar vermelha uma checagem por presença de acesso; as perdas podem tornar verde uma checagem por diminuição/zero. Neste contador específico, eventos aninhados falsos ficam fora de `top`; portanto não se deve atribuir automaticamente aumento numérico aos exemplos aninhados. (`bin/leitores_de_chave.py:695`, `bin/leitores_de_chave.py:699`, `bin/leitores_de_chave.py:736`)

**3. Mordida e vacuidade do selo.**

| Caso | Avaliação |
|---|---|
| **f2** | **Morde o defeito exato e não é vazio:** exige âncora antes da mutação, renomeia o parâmetro e cobra rc 2, nome declarado e stdout vazio. Não demonstra verdade da proveniência nem resistência a um local homônimo. (`bin/tests/test_leitores_de_chave.sh:80`, `bin/tests/test_leitores_de_chave.sh:82`, `bin/tests/test_leitores_de_chave.sh:84`; `bin/leitores_de_chave.py:188`) |
| **h** | **Morde a regressão zero→rc 1.** Universo sem acesso é o objeto do teste, não vacuidade: exige exatamente zero, rc 0 e uma ressalva do rótulo. Os outros casos exigem contagens positivas/exatas. (`bin/tests/test_leitores_de_chave.sh:155`, `bin/tests/test_leitores_de_chave.sh:157`, `bin/tests/test_leitores_de_chave.sh:54`, `bin/tests/test_leitores_de_chave.sh:145`) |
| **i** | **Morde DNA ausente e desaparecimento da chave:** exige medida positiva, controle conhecido, mutação, rc 1 nomeado e vizinha preservada. Não prova cobertura completa de DNA. (`bin/tests/test_leitores_de_chave.sh:164`, `bin/tests/test_leitores_de_chave.sh:168`, `bin/tests/test_leitores_de_chave.sh:171`, `bin/tests/test_leitores_de_chave.sh:175`) |
| **g ampliado** | **Morde dois saltos simples, laço simples e papéis.** A reatribuição testada contém somente acesso posterior ao valor não JSON; não testa acesso válido anterior, ciclos ou sementes vazias. Lacunas são comparadas por conjunto caminho/forma, sem localização ou multiplicidade. (`bin/tests/test_leitores_de_chave.sh:113`, `bin/tests/test_leitores_de_chave.sh:119`, `bin/tests/test_leitores_de_chave.sh:130`, `bin/tests/test_leitores_de_chave.sh:134`) |

**Falta mutante de `RETURNS`.** f/f2 alteram uma função de `PARAMS`; os sintéticos desligam declarações. Retirar somente a verificação de `RETURNS` não contraria nenhuma dessas asserções. Logo, “todos os casos mordem” não demonstra mordida desse ramo. (`bin/leitores_de_chave.py:61`, `bin/leitores_de_chave.py:610`; `bin/tests/test_leitores_de_chave.sh:82`, `bin/tests/test_leitores_de_chave.sh:91`, `bin/tests/test_leitores_de_chave.sh:129`, `bin/tests/test_leitores_de_chave.sh:184`)

**É furo de cobertura da garantia adicionada**, mas o contrato fornecido exige explicitamente o mutante de chave conhecida sem resultado, não um mutante para cada ramo de declaração. O furo semântico de aceitar qualquer expressão retornada existe independentemente dessa ausência de teste. (`contrato.txt:6`, anexo; `bin/leitores_de_chave.py:190`, `bin/leitores_de_chave.py:613`)

**4. ESMERIL-SEMÂNTICA: as saídas compartilham eventos, mas seus números não são intercambiáveis.**

| Saída | Quem escreve; quem lê; significado |
|---|---|
| **Texto** | `run()` imprime eventos selecionados por seção, incluindo descendentes e `*`. O selo consulta principalmente rc/stderr dessas chamadas; não valida o conteúdo da listagem textual. (`bin/leitores_de_chave.py:702`, `bin/leitores_de_chave.py:727`, `bin/leitores_de_chave.py:730`; `bin/tests/test_leitores_de_chave.sh:62`, `bin/tests/test_leitores_de_chave.sh:67`) |
| **`--json`** | `run()` publica listas filtradas, lacunas globais e resumo global. O selo lê sítios, papéis, formas, separação e lacunas. `operacoes_topo` é o mesmo total usado pelo contador; tamanho da lista `producao` não é esse total. (`bin/leitores_de_chave.py:713`, `bin/leitores_de_chave.py:719`, `bin/leitores_de_chave.py:721`; `bin/tests/test_leitores_de_chave.sh:46`, `bin/tests/test_leitores_de_chave.sh:130`, `bin/tests/test_leitores_de_chave.sh:134`) |
| **`--contar-crus`** | Imprime `len(top)`: eventos reconhecidos de produção cujo caminho é exatamente uma chave literal de topo. O selo lê o número e rc; h também lê parte do rótulo. Descendentes, `*`, caminhos dinâmicos e aliases descartados não compõem esse total. (`bin/leitores_de_chave.py:694`, `bin/leitores_de_chave.py:695`, `bin/leitores_de_chave.py:699`; `bin/tests/test_leitores_de_chave.sh:145`, `bin/tests/test_leitores_de_chave.sh:157`) |

A unidade continua sendo evento deduplicado por posição/caminho/papéis/forma. Uma operação com LEITOR e ESCRITOR vale um evento no contador, mas participa das duas contagens por papel no JSON. (`bin/leitores_de_chave.py:361`, `bin/leitores_de_chave.py:715`, `bin/leitores_de_chave.py:716`)

**O rótulo stderr melhorou e descreve o recorte principal**, inclusive escrita em memória e ausência de exclusão do acessor. Entretanto, não explicita “reconhecidos”, exclusão de reatribuídos ou lacunas. O selo ainda chama a medida de “catraca”, enquanto o instrumento declara que teto/catraca não mora nele; o contrato exige acessos fora do acessor. Portanto a concordância de significado continua parcial. (`bin/leitores_de_chave.py:29`, `bin/leitores_de_chave.py:38`, `bin/leitores_de_chave.py:274`; `bin/tests/test_leitores_de_chave.sh:4`, `bin/tests/test_leitores_de_chave.sh:145`; `contrato.txt:5`, anexo)

Há ainda rótulo textual desatualizado: “lacunas ... chave dinamica/copia inteira” agora também totaliza `apelido`. (`bin/leitores_de_chave.py:273`, `bin/leitores_de_chave.py:732`)

Consumidores efetivos em briefs, construtor ou HAIKU, além do selo demonstrado: **[não verificado]**. (`bin/tests/test_leitores_de_chave.sh:35`, `bin/tests/test_leitores_de_chave.sh:38`)

**5. QUEM ESCREVE e QUEM LÊ os valores alterados pelo diff.**

| Valor | Quem escreve | Quem lê |
|---|---|---|
| `ROTULO_CRUS` | Literal. (`bin/leitores_de_chave.py:38`) | `run()`→stderr; h confere substring. (`bin/leitores_de_chave.py:698`; `bin/tests/test_leitores_de_chave.sh:157`) |
| `CONTA_REATRIBUIDO` | Constante `False`. (`bin/leitores_de_chave.py:40`) | Retorno de `alias()`. (`bin/leitores_de_chave.py:274`) |
| `NAO_COBERTOS` | Enumeração de limites. (`bin/leitores_de_chave.py:41`) | JSON e texto. (`bin/leitores_de_chave.py:722`, `bin/leitores_de_chave.py:734`) |
| Bindings em `env` | Indexação de atribuições→`bind()`. (`bin/leitores_de_chave.py:142`, `bin/leitores_de_chave.py:212`) | Novo resolvedor de aliases. (`bin/leitores_de_chave.py:267`) |
| `busy` | Inicialização; inclusão/remoção durante resolução. (`bin/leitores_de_chave.py:117`, `bin/leitores_de_chave.py:266`, `bin/leitores_de_chave.py:268`) | Guarda de ciclo. (`bin/leitores_de_chave.py:263`) |
| `memo[(escopo,nome)] = (out, reatribuido)` | Resolução dos bindings; limpeza por rodada e ao terminar semeadura. (`bin/leitores_de_chave.py:163`, `bin/leitores_de_chave.py:175`, `bin/leitores_de_chave.py:270`) | Consulta/cache de `alias()`. (`bin/leitores_de_chave.py:265`, `bin/leitores_de_chave.py:271`) |
| `ps`, `atual`, `novo`; sementes `<chave>.[]` | Laço de até quatro rodadas; união em `seeds`. (`bin/leitores_de_chave.py:162`, `bin/leitores_de_chave.py:168`, `bin/leitores_de_chave.py:171`) | Controle de nova rodada e `paths(Name)`; depois emissão de eventos. (`bin/leitores_de_chave.py:173`, `bin/leitores_de_chave.py:286`, `bin/leitores_de_chave.py:447`, `bin/leitores_de_chave.py:482`) |
| Lacuna `reatribuido:<nome>` / `apelido` | `alias()`; lacunas preliminares são apagadas após semeadura. (`bin/leitores_de_chave.py:176`, `bin/leitores_de_chave.py:273`) | `censo()`→JSON/total textual; g lê caminho/forma. (`bin/leitores_de_chave.py:639`, `bin/leitores_de_chave.py:721`, `bin/leitores_de_chave.py:732`; `bin/tests/test_leitores_de_chave.sh:134`) |
| Assinatura `nomes` | Parâmetros e nomes locais `Store`. (`bin/leitores_de_chave.py:185`, `bin/leitores_de_chave.py:188`) | Guarda `PARAMS`. (`bin/leitores_de_chave.py:608`) |
| Assinatura `retorna` | Qualquer `Return` com expressão. (`bin/leitores_de_chave.py:190`) | Guarda `RETURNS`. (`bin/leitores_de_chave.py:613`) |
| `defs[f.rel]`, `faltam` | `censo()` guarda assinaturas; guard acrescenta faltantes. (`bin/leitores_de_chave.py:634`, `bin/leitores_de_chave.py:605`, `bin/leitores_de_chave.py:609`, `bin/leitores_de_chave.py:614`) | `run()` imprime diagnóstico e retorna rc 2; f/f2 conferem. (`bin/leitores_de_chave.py:687`, `bin/leitores_de_chave.py:690`; `bin/tests/test_leitores_de_chave.sh:84`, `bin/tests/test_leitores_de_chave.sh:93`) |
| `len(top)` / rc 0 | Seleção produção→topo; impressão/retorno. (`bin/leitores_de_chave.py:693`, `bin/leitores_de_chave.py:699`) | JSON e selo real/e/h/i. (`bin/leitores_de_chave.py:713`; `bin/tests/test_leitores_de_chave.sh:52`, `bin/tests/test_leitores_de_chave.sh:145`, `bin/tests/test_leitores_de_chave.sh:157`, `bin/tests/test_leitores_de_chave.sh:165`) |
| `_tmp` | `mktemp`, agora validado. (`bin/tests/test_leitores_de_chave.sh:27`, `bin/tests/test_leitores_de_chave.sh:28`) | Redirecionamentos, cópias, fixtures e limpeza. (`bin/tests/test_leitores_de_chave.sh:29`, `bin/tests/test_leitores_de_chave.sh:35`, `bin/tests/test_leitores_de_chave.sh:57`, `bin/tests/test_leitores_de_chave.sh:155`, `bin/tests/test_leitores_de_chave.sh:161`) |
| `LC` / parâmetro f2 | Caminho e `sed` de renomeação/restauração. (`bin/tests/test_leitores_de_chave.sh:79`, `bin/tests/test_leitores_de_chave.sh:82`, `bin/tests/test_leitores_de_chave.sh:87`) | Âncora, ferramenta e asserção. (`bin/tests/test_leitores_de_chave.sh:80`, `bin/tests/test_leitores_de_chave.sh:83`, `bin/tests/test_leitores_de_chave.sh:84`) |
| Fixture g; `G`, `ESP`, `A` | Heredoc; extração JSON e expectativa literal. (`bin/tests/test_leitores_de_chave.sh:113`, `bin/tests/test_leitores_de_chave.sh:130`, `bin/tests/test_leitores_de_chave.sh:131`, `bin/tests/test_leitores_de_chave.sh:134`) | Comparações de eventos/lacunas e contador esperado 10. (`bin/tests/test_leitores_de_chave.sh:132`, `bin/tests/test_leitores_de_chave.sh:135`, `bin/tests/test_leitores_de_chave.sh:145`) |
| `Z` / fonte sem acesso | Fixture zero. (`bin/tests/test_leitores_de_chave.sh:155`) | Ferramenta e asserção h. (`bin/tests/test_leitores_de_chave.sh:156`, `bin/tests/test_leitores_de_chave.sh:157`) |
| `D`, `N_DNA`, chave mutada | Cópia, captura do contador e substituição de literal. (`bin/tests/test_leitores_de_chave.sh:161`, `bin/test

[truncado em 20000 de 21407 caracteres]

## 12/11 18:08 · sessao f61fb656431c · - · - · origem=Ronald (tmux)

Achados de proveniência, com reprodução somente em memória:

- **Apelido de 2+ saltos: curado para cadeia acíclica simples.** `a=c.ata; b=a; b["k"]` agora aparece; resolução passa por `alias()` com memo e guarda de ciclo. (`bin/leitores_de_chave.py:257`, `bin/leitores_de_chave.py:288`)
- **Perda de `lamp` em `dispensar_por_ausencia`: curada no sítio real.** O laço transmite `lampadas.[]` e agora aparecem leitura em `:807`, escritas em `:808–810` e leitura em `:812`. (`bin/leitores_de_chave.py:167`, `app/ponto/services/cartorio.py:805`, `app/ponto/services/cartorio.py:807`, `app/ponto/services/cartorio.py:808`, `app/ponto/services/cartorio.py:809`, `app/ponto/services/cartorio.py:810`, `app/ponto/services/cartorio.py:812`)
- **Guarda de nome declarado: parcial.** Renomear exclusivamente o parâmetro de `_orfas_da_ata` agora falha; porém um local `ata={}` mantém a assinatura válida e recebe proveniência declarada, mesmo com parâmetro renomeado. O guard aceita parâmetros **ou qualquer `Name(Store)`**. (`bin/leitores_de_chave.py:138`, `bin/leitores_de_chave.py:185`, `bin/leitores_de_chave.py:188`, `bin/leitores_de_chave.py:608`, `app/escala/services/leitor_celula.py:180`)
- **Guarda de `RETURNS`: parcial.** Detecta função ausente e `return` sem expressão; aceita `return None` e `return 42`, porque apenas testa `n.value is not None`. Logo prova presença sintática de retorno, sem provar que retorna JSON. (`bin/leitores_de_chave.py:190`, `bin/leitores_de_chave.py:613`, `bin/leitores_de_chave.py:330`)

Falsos resultados novos confirmados:

1. **Semeadura retroativa real:** `h[2]`, `h[:2]`, `h[3:]` da lambda `_h_ok`, definidos em `app/chamados/services/materializacao.py:177` e `app/chamados/services/materializacao.py:178`, passam a sair como `orfas.[].<dinamica>`, devido ao `for h in ...orfas` **posterior**, em `app/chamados/services/materializacao.py:181`. Lambda e comprehension não criam escopo no indexador; sementes valem para a função inteira. (`bin/leitores_de_chave.py:167`, `bin/leitores_de_chave.py:169`, `bin/leitores_de_chave.py:199`, `bin/leitores_de_chave.py:285`)

2. **Outro falso positivo real:** `x['entrada']`, cujo `x` percorre `turnos`, vira `lampadas.[].entrada`; o `x` usado antes na comprehension de lâmpadas contamina o restante da função. O mesmo atinge `x` percorrendo `dias` em `:209–235`. (`app/ponto/services/diagnostico_escala.py:154`, `app/ponto/services/diagnostico_escala.py:156`, `app/ponto/services/diagnostico_escala.py:171`, `app/ponto/services/diagnostico_escala.py:208`, `app/ponto/services/diagnostico_escala.py:209`)

3. **Seed vazio apaga alias silenciosamente:** sintético `a=c.ata; a["k"]; for a in []: pass; a["j"]` perde os dois acessos, **sem lacuna**. O bloco sempre cria entrada em `seeds`, mesmo com `ps=set()`, e `paths(Name)` retorna essa entrada antes de tentar o alias. (`bin/leitores_de_chave.py:168`, `bin/leitores_de_chave.py:169`, `bin/leitores_de_chave.py:286`)

4. **Seed também burla reatribuição:** sintético `for a in c.ata["xs"]: pass; a={}; a["k"]` emite `xs.[].k` sem lacuna. A proveniência semeada ignora os bindings e não consulta `CONTA_REATRIBUIDO`. (`bin/leitores_de_chave.py:169`, `bin/leitores_de_chave.py:267`, `bin/leitores_de_chave.py:274`, `bin/leitores_de_chave.py:286`)

5. **Reatribuição reduz medida apesar de acesso válido anterior:** sintético `a=c.ata; a["k"]; a={}` remove `k`, deixando lacunas. A lacuna é visível no JSON e contada no texto, porém `--contar-crus` retorna número menor com rc 0 sem informar a lacuna. Isso permite falso verde se o número for usado como catraca/completude. (`bin/leitores_de_chave.py:267`, `bin/leitores_de_chave.py:273`, `bin/leitores_de_chave.py:695`, `bin/leitores_de_chave.py:697`, `bin/leitores_de_chave.py:721`, `bin/leitores_de_chave.py:732`)

6. **Ciclo válido de atribuições perde proveniência:** sintético `a=c.ata; b=a; a=b; a["k"]; b["j"]` termina, mas perde ambos. `busy` devolve conjunto vazio; esse resultado parcial participa do memo e da detecção de reatribuição. (`bin/leitores_de_chave.py:263`, `bin/leitores_de_chave.py:267`, `bin/leitores_de_chave.py:270`)

Comparação v1 reconstruída em memória versus atual: eventos **de topo** permanecem 22→22 em `cartorio.py`, 20→20 em `leitor_celula.py`, 10→10 em `juizes.py`; nenhum topo entra/sai nesses três arquivos. Unidade/filtro: `bin/leitores_de_chave.py:695`; emissão e deduplicação: `bin/leitores_de_chave.py:347`, `bin/leitores_de_chave.py:361`.

No trecho `_ata_ate_o_teto`, entram eventos aninhados nas linhas `914`, `915` e `917`: leitura de `acesa/tipo/hora`, cópia de `lampadas.[].*`, escritas `lampadas.[].acesa`/`pendente` e leitura final de `acesa`. (`app/ponto/services/cartorio.py:913`, `app/ponto/services/cartorio.py:914`, `app/ponto/services/cartorio.py:915`, `app/ponto/services/cartorio.py:917`)

## 12/11 18:07 · sessao 0f18aeb186a9 · - · - · origem=Ronald (tmux)

| Valor do delta | Quem escreve | Quem lê / efeito |
|---|---|---|
| `ROTULO_CRUS` | Literal em `bin/leitores_de_chave.py:38` | `run` imprime no stderr (`bin/leitores_de_chave.py:698`); h confere apenas substring (`bin/tests/test_leitores_de_chave.sh:157`). |
| `CONTA_REATRIBUIDO=False` | `bin/leitores_de_chave.py:40` | `alias` descarta proveniência ambígua (`bin/leitores_de_chave.py:274`). |
| `NAO_COBERTOS` | `bin/leitores_de_chave.py:41` | JSON (`bin/leitores_de_chave.py:722`) e texto (`bin/leitores_de_chave.py:734`); g confere aviso de `--sem-declaracoes`, não essa enumeração (`bin/tests/test_leitores_de_chave.sh:132`). |
| `env` / bindings usados pelo novo alias | Atribuições indexadas e `bind` (`bin/leitores_de_chave.py:142`, `bin/leitores_de_chave.py:212`) | `alias` resolve cada binding (`bin/leitores_de_chave.py:267`). |
| `memo` | Inicialização (`bin/leitores_de_chave.py:117`), limpeza por rodada/final (`bin/leitores_de_chave.py:163`, `bin/leitores_de_chave.py:175`), preenchimento `(out, reatribuido)` (`bin/leitores_de_chave.py:270`) | Teste de presença e recuperação (`bin/leitores_de_chave.py:265`, `bin/leitores_de_chave.py:271`). |
| `busy` | Inicialização (`bin/leitores_de_chave.py:117`), inclusão/remoção (`bin/leitores_de_chave.py:266`, `bin/leitores_de_chave.py:268`) | Guarda de ciclo retorna conjunto vazio (`bin/leitores_de_chave.py:263`). |
| `seeds` / caminho `<chave>.[]` | PARAMS (`bin/leitores_de_chave.py:138`), atribuição a atributo (`bin/leitores_de_chave.py:155`), `values_list` (`bin/leitores_de_chave.py:246`), novo laço (`bin/leitores_de_chave.py:168`, `bin/leitores_de_chave.py:171`) | `paths(Name)` dá precedência à seed (`bin/leitores_de_chave.py:286`), alimentando novos eventos de `scan` (`bin/leitores_de_chave.py:447`, `bin/leitores_de_chave.py:482`). |
| `ps`, `atual`, `novo` | `bin/leitores_de_chave.py:164`, `bin/leitores_de_chave.py:168`, `bin/leitores_de_chave.py:169`, `bin/leitores_de_chave.py:172` | União de caminhos e encerramento das até quatro rodadas (`bin/leitores_de_chave.py:170`, `bin/leitores_de_chave.py:173`, `bin/leitores_de_chave.py:162`). |
| Lacuna `reatribuido:<nome>`, forma `apelido` | `alias` (`bin/leitores_de_chave.py:273`); lacunas preliminares apagadas ao fim da semeadura (`bin/leitores_de_chave.py:176`) | `censo` serializa (`bin/leitores_de_chave.py:639`); JSON publica (`bin/leitores_de_chave.py:721`); texto só totaliza (`bin/leitores_de_chave.py:732`); g lê conjunto caminho/forma (`bin/tests/test_leitores_de_chave.sh:134`). |
| Assinatura `nomes` | Parâmetros e nomes `Store` (`bin/leitores_de_chave.py:185`, `bin/leitores_de_chave.py:188`) | Guard PARAMS compara existência do nome (`bin/leitores_de_chave.py:608`). |
| Assinatura `retorna` | Existência de qualquer `Return` com expressão AST (`bin/leitores_de_chave.py:190`) | Guard RETURNS (`bin/leitores_de_chave.py:613`). |
| `defs[f.rel]` | `censo` recebe `f.assinaturas()` (`bin/leitores_de_chave.py:634`) | `run` entrega ao guard (`bin/leitores_de_chave.py:687`), que consulta arquivo/função (`bin/leitores_de_chave.py:603`, `bin/leitores_de_chave.py:612`). |
| `faltam` | Guard acrescenta função/nome/retorno ausente (`bin/leitores_de_chave.py:605`, `bin/leitores_de_chave.py:609`, `bin/leitores_de_chave.py:614`) | `run` emite diagnóstico e rc 2 (`bin/leitores_de_chave.py:688`); f/f2 conferem rc, diagnóstico e stdout vazio (`bin/tests/test_leitores_de_chave.sh:84`, `bin/tests/test_leitores_de_chave.sh:93`). |
| Número `len(top)` / rc 0 | Seleção produção→chaves literais de topo (`bin/leitores_de_chave.py:693`); impressão e retorno (`bin/leitores_de_chave.py:699`) | Selo real (`bin/tests/test_leitores_de_chave.sh:52`), e (`bin/tests/test_leitores_de_chave.sh:145`), h (`bin/tests/test_leitores_de_chave.sh:157`), DNA (`bin/tests/test_leitores_de_chave.sh:165`). JSON publica o mesmo total (`bin/leitores_de_chave.py:713`). |
| `_tmp` | `mktemp`, agora conferido (`bin/tests/test_leitores_de_chave.sh:27`) | Guarda de diretório (`bin/tests/test_leitores_de_chave.sh:28`), redirecionamentos (`bin/tests/test_leitores_de_chave.sh:35`), cópias/fixtures (`bin/tests/test_leitores_de_chave.sh:57`, `bin/tests/test_leitores_de_chave.sh:97`, `bin/tests/test_leitores_de_chave.sh:155`, `bin/tests/test_leitores_de_chave.sh:161`), limpeza (`bin/tests/test_leitores_de_chave.sh:29`). |
| `LC` / parâmetro mutado f2 | Caminho (`bin/tests/test_leitores_de_chave.sh:79`), `sed` de renomeação (`bin/tests/test_leitores_de_chave.sh:82`) | Âncora (`bin/tests/test_leitores_de_chave.sh:80`), ferramenta (`bin/tests/test_leitores_de_chave.sh:83`), asserção (`bin/tests/test_leitores_de_chave.sh:84`); restauração (`bin/tests/test_leitores_de_chave.sh:87`). |
| Fixture g: laço, dois saltos, reatribuição | Heredoc (`bin/tests/test_leitores_de_chave.sh:113`) | AST da ferramenta (`bin/tests/test_leitores_de_chave.sh:129`); `G/ESP` com papéis (`bin/tests/test_leitores_de_chave.sh:130`); `A` para lacunas (`bin/tests/test_leitores_de_chave.sh:134`); contador esperado 10 (`bin/tests/test_leitores_de_chave.sh:145`). |
| `Z` / universo zero | Caminho e fonte sem acesso (`bin/tests/test_leitores_de_chave.sh:155`) | Ferramenta (`bin/tests/test_leitores_de_chave.sh:156`); h exige zero, rc 0 e substring (`bin/tests/test_leitores_de_chave.sh:157`). |
| `D`, `N_DNA`, chave mutada | Cópia (`bin/tests/test_leitores_de_chave.sh:161`), captura (`bin/tests/test_leitores_de_chave.sh:165`), mutação literal (`bin/tests/test_leitores_de_chave.sh:171`) | Contagem positiva (`bin/tests/test_leitores_de_chave.sh:167`), controle (`bin/tests/test_leitores_de_chave.sh:168`), mutante (`bin/tests/test_leitores_de_chave.sh:172`), vizinha (`bin/tests/test_leitores_de_chave.sh:175`), mensagem final (`bin/tests/test_leitores_de_chave.sh:184`). |
| stdout, stderr, `rc` entre chamadas | `_lc` sobrescreve os três (`bin/tests/test_leitores_de_chave.sh:35`) | `_jq` lê stdout JSON (`bin/tests/test_leitores_de_chave.sh:38`); asserções leem número, diagnóstico e status (`bin/tests/test_leitores_de_chave.sh:84`, `bin/tests/test_leitores_de_chave.sh:145`, `bin/tests/test_leitores_de_chave.sh:157`, `bin/tests/test_leitores_de_chave.sh:173`). |

ESMERIL: o número do contador e `resumo_producao.operacoes_topo` compartilham `len(top)`, mas o texto e as listas JSON incluem eventos selecionados com descendentes e `*`; portanto seus tamanhos não são intercambiáveis. (`bin/leitores_de_chave.py:695`, `bin/leitores_de_chave.py:703`, `bin/leitores_de_chave.py:713`, `bin/leitores_de_chave.py:719`, `bin/leitores_de_chave.py:727`)

A linguagem do selo ainda chama essa medida de “catraca”, enquanto o instrumento declara que teto/catraca não mora ali. O texto também continua descrevendo lacunas apenas como dinâmica/cópia inteira, embora agora inclua `apelido`. (`bin/tests/test_leitores_de_chave.sh:4`, `bin/tests/test_leitores_de_chave.sh:145`; `bin/leitores_de_chave.py:29`, `bin/leitores_de_chave.py:273`, `bin/leitores_de_chave.py:732`)

Consumidor executável demonstrado dessas saídas: o selo; entrega efetiva a briefs, construtor, preverificador ou HAIKU: **[não verificado]**. (`bin/tests/test_leitores_de_chave.sh:35`, `bin/tests/test_leitores_de_chave.sh:38`)

## 12/11 18:06 · sessao 9e5e15709a4a · - · - · origem=Ronald (tmux)

- **f2 morde e não é vazio:** exige âncora real antes de renomear; cobra rc 2, nome declarado e stdout vazio. O guard anterior passaria indevidamente esse caso. (`bin/tests/test_leitores_de_chave.sh:80`, `bin/tests/test_leitores_de_chave.sh:82`, `bin/tests/test_leitores_de_chave.sh:84`; `app/escala/services/leitor_celula.py:180`)
- **h morde a regressão zero→rc 1:** cobra exatamente `0`, rc 0 e a ressalva sobre acessor. Seu universo é vazio por construção; isso não torna o conjunto vacuamente verde, porque a/e/i exigem medidas positivas/exatas. (`bin/tests/test_leitores_de_chave.sh:155`, `bin/tests/test_leitores_de_chave.sh:157`, `bin/tests/test_leitores_de_chave.sh:54`, `bin/tests/test_leitores_de_chave.sh:145`, `bin/tests/test_leitores_de_chave.sh:167`)
- **i morde DNA quebrado e desaparecimento:** medida positiva, controle conhecido rc 0, mutação, rc 1 nomeado e vizinha preservada. Não demonstra completude de DNA. (`bin/tests/test_leitores_de_chave.sh:164`, `bin/tests/test_leitores_de_chave.sh:168`, `bin/tests/test_leitores_de_chave.sh:171`, `bin/tests/test_leitores_de_chave.sh:173`, `bin/tests/test_leitores_de_chave.sh:175`)
- **RETURNS continua sem mutante próprio:** f/f2 mexem exclusivamente em `_orfas_da_ata`, declarada em PARAMS; os sintéticos seguintes desligam declarações. Retirar só a verificação RETURNS pode preservar todas as asserções do selo atual. É lacuna de cobertura da promessa “todos os casos mordem”, sem estabelecer obrigação contratual explícita de um mutante para cada ramo. (`bin/tests/test_leitores_de_chave.sh:82`, `bin/tests/test_leitores_de_chave.sh:91`, `bin/tests/test_leitores_de_chave.sh:129`, `bin/leitores_de_chave.py:61`, `bin/leitores_de_chave.py:610`, `bin/tests/test_leitores_de_chave.sh:184`)
- **Guarda curada parcialmente:** agora verifica nome declarado e existência de `return <expressão>`, mas nomes locais `Store` satisfazem PARAMS, e `return None`/`return 0` satisfazem RETURNS. Reprodução em memória confirmou essas três situações; a guarda prova presença sintática, sem provar JSON retornado/recebido. (`bin/leitores_de_chave.py:188`, `bin/leitores_de_chave.py:190`, `bin/leitores_de_chave.py:608`, `bin/leitores_de_chave.py:613`)
- **Achados v1 curados no selo:** papéis de setdefault/pop agora conferidos; rc do contador sintético conferido; DNA exercitado; falha de mktemp aborta antes das gravações. (`bin/tests/test_leitores_de_chave.sh:130`, `bin/tests/test_leitores_de_chave.sh:131`, `bin/tests/test_leitores_de_chave.sh:145`, `bin/tests/test_leitores_de_chave.sh:164`, `bin/tests/test_leitores_de_chave.sh:27`)
- **Achados v1 persistentes:** árvore real exige somente presenças mínimas e total positivo; raiz comprova presença, não identidade do hash; não há asserção de teto anterior ou exclusão de acessor. (`bin/tests/test_leitores_de_chave.sh:23`, `bin/tests/test_leitores_de_chave.sh:46`, `bin/tests/test_leitores_de_chave.sh:54`; `bin/leitores_de_chave.py:695`)
- **Semântica ainda divergente:** instrumento declara que teto/catraca não mora nele, mas selo continua nomeando medida “catraca”; texto rotula lacunas apenas como “chave dinamica/copia inteira”, omitindo novo `apelido`. h verifica apenas uma substring do rótulo. (`bin/leitores_de_chave.py:29`; `bin/tests/test_leitores_de_chave.sh:4`, `bin/tests/test_leitores_de_chave.sh:145`, `bin/tests/test_leitores_de_chave.sh:157`; `bin/leitores_de_chave.py:732`)
- **Limite novo de g:** lacunas são verificadas por conjunto de caminho/forma, sem posição, função ou multiplicidade. (`bin/tests/test_leitores_de_chave.sh:134`, `bin/tests/test_leitores_de_chave.sh:135`)

## 12/11 17:46 · sessao d118260d8f2a · TRANCA-VIVAS-08 · sonda · origem=Code

O envelope executa a sonda antes do Codex e entrega a saída raspada no prompt ([bin/codex_pergunta.sh:326](bin/codex_pergunta.sh:326), [bin/codex_pergunta.sh:357](bin/codex_pergunta.sh:357)). Segue a transcrição integral recebida; reexecução independente: [nao verificado].

```text
sonda_leitura: sonda_v4.py [b406e34784086e43] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

log#747607 acao=veredito_celula modelo=CelulaDia obj=10153 usuario=657 ator=None em=2026-10-22T15:35:25.946330-03:00 via_depois=cartorio via_antes=cartorio veredito_antes=cobrado veredito_depois=cobrado chaves_depois=['protestos', 'veredito', 'via'] acoes=None desc_ini='veredito cobrado -> cobrado via cartorio'
log#747612 acao=veredito_celula modelo=CelulaDia obj=10153 usuario=657 ator=None em=2026-10-22T15:35:27.552330-03:00 via_depois=cartorio via_antes=cartorio veredito_antes=cobrado veredito_depois=discordante chaves_depois=['protestos', 'veredito', 'via'] acoes=None desc_ini='veredito cobrado -> discordante via cartorio'
log#747617 acao=veredito_celula modelo=CelulaDia obj=10152 usuario=657 ator=None em=2026-10-22T15:35:29.134502-03:00 via_depois=cartorio via_antes=cartorio veredito_antes=cobrado veredito_depois=cobrado chaves_depois=['protestos', 'veredito', 'via'] acoes=None desc_ini='veredito cobrado -> cobrado via cartorio'
log#747622 acao=veredito_celula modelo=CelulaDia obj=10152 usuario=657 ator=None em=2026-10-22T15:35:30.735412-03:00 via_depois=cartorio via_antes=cartorio veredito_antes=cobrado veredito_depois=discordante chaves_depois=['protestos', 'veredito', 'via'] acoes=None desc_ini='veredito cobrado -> discordante via cartorio'
log#747627 acao=veredito_celula modelo=CelulaDia obj=10151 usuario=657 ator=None em=2026-10-22T15:35:32.359774-03:00 via_depois=cartorio via_antes=cartorio veredito_antes=cobrado veredito_depois=cobrado chaves_depois=['protestos', 'veredito', 'via'] acoes=None desc_ini='veredito cobrado -> cobrado via cartorio'
log#769000 acao=veredito_celula modelo=CelulaDia obj=4538 usuario=653 ator=None em=2026-10-26T11:05:49.557493-03:00 via_depois=cartorio via_antes=cartorio veredito_antes=concorde veredito_depois=furo chaves_depois=['protestos', 'veredito', 'via'] acoes=None desc_ini='veredito concorde -> furo via cartorio'
log#933557 acao=veredito_celula modelo=CelulaDia obj=10151 usuario=657 ator=None em=2026-11-11T11:48:54.050593-03:00 via_depois=cartorio via_antes=cartorio veredito_antes=cobrado veredito_depois=furo chaves_depois=['protestos', 'status_dia', 'veredito', 'via'] acoes=None desc_ini='veredito cobrado -> furo via cartorio'
cel#4538 colab367 data=2026-08-01 veredito=furo via=cartorio veredito_em=2026-10-26T11:05:49.511359-03:00
   logs veredito_celula com usuario: 1; com acoes (servico Resolver dia): 0
cel#10151 colab444 data=2026-08-03 veredito=furo via=cartorio veredito_em=2026-11-11T11:48:53.970330-03:00
   logs veredito_celula com usuario: 2; com acoes (servico Resolver dia): 0
cel#10152 colab444 data=2026-08-04 veredito=furo via=cartorio veredito_em=2026-11-12T06:27:03.004867-03:00
   logs veredito_celula com usuario: 2; com acoes (servico Resolver dia): 0
cel#10153 colab444 data=2026-08-05 veredito=discordante via=cartorio veredito_em=2026-10-22T15:35:27.512146-03:00
   logs veredito_celula com usuario: 2; com acoes (servico Resolver dia): 0
total_erros=0
== anexo: retira_acervo_tranca_2.txt ==
aval Ronald: RETIRA ACERVO-TRANCA-2 (publicado 16:3x, contraria CLAUDE.md 7b DINHEIRO item 3: nenhuma trava por exportada/trancada/paga; correcao provada entra em qualquer competencia). Nao executar os itens 1, 2, 3 e 5; nenhuma lei nova. Fica SO o item 4, que e lei existente (TRANCA-DE-VERDADE): as 4 perguntas vivas na comp 08 emp2 trancada -> listar pk e o degrau de chamados/juizes.py::pergunta_viva que as deixou vivas; curar na origem se for defeito; contador vivas_apesar_da_tranca na frota, esperado 0. Segue a fila; PAREI so em lei ou !
```

1. Pela assinatura recebida, **747607, 747612, 747617, 747622 e 747627** correspondem à trilha da lavra em **`app/ponto/portas/celula.py:1042`**, todos via **`cartorio`**; a descrição coincide com `app/ponto/portas/celula.py:1043`.
2. **769000** corresponde à mesma lavra, via **`cartorio`** (`app/ponto/portas/celula.py:1042`, `app/ponto/portas/celula.py:1043`).
3. **933557** corresponde à mesma lavra, via **`cartorio`**, incluindo `status_dia` no payload (`app/ponto/portas/celula.py:1044`).
4. Nenhum tem a assinatura de “Resolver dia”, que grava `acoes` (`app/ponto/services/veredito_celula.py:356`, `app/ponto/services/veredito_celula.py:358`), nem a descrição exclusiva de alteração de status do ramo `app/ponto/portas/celula.py:1028`.
5. **`_toque_humano_nos_dias` confunde as assinaturas:** usa a ação definida em `app/chamados/juizes.py:1088`, exige usuário não nulo/diferente do sistema (`app/chamados/juizes.py:1253`) e aceita ação+célula+horário, sem conferir `via`, `acoes` ou `ator` (`app/chamados/juizes.py:1287`, `app/chamados/juizes.py:1292`).
6. Esse toque passa por `_admin_tocou_os_dias_depois` (`app/chamados/juizes.py:1225`) e afasta a tranca em `dias_trancados` (`app/chamados/juizes.py:1079`), influenciando o degrau de `pergunta_viva` em `app/chamados/juizes.py:1395`.
7. Outro leitor direto é **`decisao_do_dia`**: restringe previamente a célula à via humana `admin` (`app/ponto/services/veredito_celula.py:76`, `app/ponto/services/veredito_celula.py:110`) e consulta a ação para completar autoria (`app/ponto/services/veredito_celula.py:115`, `app/ponto/services/veredito_celula.py:120`).
8. A lavra pode herdar o usuário autenticado da requisição (`app/core/observ.py:113`); portanto, usuário preenchido não distingue “Resolver dia” de cartório. Identidade do usuário de sistema, horário da tranca, PKs das perguntas e efeito efetivo desses logs na produção: [nao verificado].

## 12/11 17:45 · sessao a73b5ea5ea07 · - · - · origem=Ronald (tmux)

Encontrei dois leitores diretos de `LogAuditoria.acao='veredito_celula'`:

- `_toque_humano_nos_dias`: constante em `app/chamados/juizes.py:1088`; exige usuário não nulo/diferente do sistema em `app/chamados/juizes.py:1253`; consulta ação, modelo e célula em `app/chamados/juizes.py:1287`; marca toque pelo horário em `app/chamados/juizes.py:1292`, sem conferir `acoes`, `via` ou `ator`. Esse toque afasta a tranca em `app/chamados/juizes.py:1079`.
- `decisao_do_dia`: primeiro restringe as células a `veredito_via='admin'` em `app/ponto/services/veredito_celula.py:110`; depois consulta essa ação em `app/ponto/services/veredito_celula.py:115` para obter o autor da última trilha com nome em `app/ponto/services/veredito_celula.py:118`. Consumidores: calendário em `app/colaboradores/services/calendario.py:416` e API em `app/api/views.py:1411`.

Não encontrei outro leitor direto na busca da cópia; exaustividade fora dos arquivos pesquisados: [não verificado].

## 12/11 17:43 · sessao 3d62cbc2dfb9 · TRANCA-VIVAS-08 · sonda · origem=Code

Não houve nova execução: foi fornecida a saída raspada, sem o código de `sonda_v3.py`; execução independente [nao verificado]. Transcrição integral da saída fornecida [nao verificado]:

```text
sonda_leitura: sonda_v3.py [a35d1b44e7abf2fc] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

agora 2026-11-12T17:42:11.101502-03:00
PF#3 emp2 08/2026 fechado_em=2026-09-16T09:11:43.807307-03:00 reaberto_em=None janela_fechamento=(datetime.date(2026, 7, 21), datetime.date(2026, 8, 20))
#20798 col367 emp2 data_evento=2026-08-01 dia_turno=2026-08-01 janela=2026-07-21..2026-08-20 chave=(2,8,2026) pf=3 pf_fechado=2026-09-16T09:11:43.807307-03:00 tocou=True C20=False
   proj: validada=False respondida=False resposta_nao_vazia=False materializada=True via=sem_turno_aberto | celula=4538 veredito=furo via_cel=cartorio admin_decidiu=False concorda=n/a | viva=True
   toque cobrar: n=0 []
   toque resolver: n=1 [(769000, 653, '2026-10-26T11:05:49.557493-03:00')]
   toque desarquivar: n=0 []
#47879 col444 emp2 data_evento=2026-08-03 dia_turno=2026-08-03 janela=2026-07-21..2026-08-20 chave=(2,8,2026) pf=3 pf_fechado=2026-09-16T09:11:43.807307-03:00 tocou=True C20=False
   proj: validada=False respondida=True resposta_nao_vazia=True materializada=False via= | celula=10151 veredito=furo via_cel=cartorio admin_decidiu=False concorda=None | viva=True
   toque cobrar: n=0 []
   toque resolver: n=2 [(747627, 657, '2026-10-22T15:35:32.359774-03:00'), (933557, 657, '2026-11-11T11:48:54.050593-03:00')]
   toque desarquivar: n=0 []
#47881 col444 emp2 data_evento=2026-08-04 dia_turno=2026-08-04 janela=2026-07-21..2026-08-20 chave=(2,8,2026) pf=3 pf_fechado=2026-09-16T09:11:43.807307-03:00 tocou=True C20=False
   proj: validada=False respondida=True resposta_nao_vazia=True materializada=False via= | celula=10152 veredito=furo via_cel=cartorio admin_decidiu=False concorda=None | viva=True
   toque cobrar: n=0 []
   toque resolver: n=2 [(747617, 657, '2026-10-22T15:35:29.134502-03:00'), (747622, 657, '2026-10-22T15:35:30.735412-03:00')]
   toque desarquivar: n=0 []
#47884 col444 emp2 data_evento=2026-08-05 dia_turno=2026-08-05 janela=2026-07-21..2026-08-20 chave=(2,8,2026) pf=3 pf_fechado=2026-09-16T09:11:43.807307-03:00 tocou=True C20=False
   proj: validada=False respondida=True resposta_nao_vazia=True materializada=False via= | celula=10153 veredito=discordante via_cel=cartorio admin_decidiu=False concorda=None | viva=True
   toque cobrar: n=0 []
   toque resolver: n=2 [(747607, 657, '2026-10-22T15:35:25.946330-03:00'), (747612, 657, '2026-10-22T15:35:27.552330-03:00')]
   toque desarquivar: n=0 []
total_erros=0
```

- Pelo dado fornecido, nas quatro perguntas a chave `(2,8,2026)`, derivada do fim da janela em `20/08/2026`, encontrou PF#3; portanto a causa não é período ausente. O juiz deriva e consulta essa chave em `app/chamados/juizes.py:1062`–`1068`. Dados de produção [nao verificado].
- **#20798:** chega ao degrau **célula acusadora**, `furo`, e retorna `True` em `app/chamados/juizes.py:1440`–`1441`, apesar do carimbo `sem_turno_aberto`. C20=False por **Resolver dia**, log 769000, usuário 653, em 26/10, depois de `fechado_em` de 16/09; comparação em `app/chamados/juizes.py:1292`, inversão em `app/chamados/juizes.py:1079`. Dados de produção [nao verificado].
- **#47879:** retorna no degrau **resposta preenchida, sem decisão administrativa atual, concordância não comprovada**: `concorda=None` faz `is not True` retornar `True` em `app/chamados/juizes.py:1407`–`1419`. C20=False por **Resolver dia**, logs 747627 e 933557, usuário 657, em 22/10 e 11/11, posteriores ao fechamento; `app/chamados/juizes.py:1292`, `app/chamados/juizes.py:1079`. Dados de produção [nao verificado].
- **#47881:** mesmo degrau da resposta: `admin_decidiu=False`, `concorda=None`, retorno `True` em `app/chamados/juizes.py:1415`–`1419`. C20=False por **Resolver dia**, logs 747617 e 747622, usuário 657, em 22/10, posteriores ao fechamento; `app/chamados/juizes.py:1292`, `app/chamados/juizes.py:1079`. Dados de produção [nao verificado].
- **#47884:** mesmo degrau da resposta, antes de examinar o veredito `discordante`: `admin_decidiu=False`, `concorda=None`, retorno `True` em `app/chamados/juizes.py:1415`–`1419`. C20=False por **Resolver dia**, logs 747607 e 747612, usuário 657, em 22/10, posteriores ao fechamento; `app/chamados/juizes.py:1292`, `app/chamados/juizes.py:1079`. Dados de produção [nao verificado].
- **Resolver dia** corresponde a `veredito_celula` em `app/chamados/juizes.py:1088`; o filtro exige usuário preenchido e diferente do sistema em `app/chamados/juizes.py:1253`–`1254`. O dado fornecido mostra zero ocorrências de cobrar e desarquivar nas quatro perguntas [nao verificado].

## 12/11 17:37 · sessao d45010bd76f3 · E3-LEITORES · censo · origem=Code

**PERGUNTA DE DESENHO — o código tem várias operações que persistem a resposta.** O core humano, a resposta automática, o carimbo pela trilha e `PerguntaDisputa.carimbar` escrevem independentemente. Portanto, o censo não certifica **UMA porta de escrita**. (`app/chamados/services/disputa_emissao.py:1467`, `:1657`, `:1679`; `app/chamados/models.py:3157`)

A função denominada “porta do carimbo de resposta”, `recusa_respondida_sem_texto`, é uma **guarda**, sem persistência: constrói um `SimpleNamespace`, consulta o predicado e pode levantar exceção. (`app/chamados/services/fio.py:1578`, `:1585`, `:1587`)

**HEAD**

Critério: sítio de acesso ao campo, incluindo payloads encaminhados, projeções ORM e acessos dinâmicos; chamadas transitivas não são contadas novamente como acesso direto. A identificação Git do snapshot, execução dos testes e estado efetivo de produção ficam **[nao verificado]**. [nao verificado]

Nas tabelas, **E = `respondida_em`**, **T = `resposta_colab`**. Caminhos sem prefixo são relativos a **`app/chamados/`**. Linhas agrupadas pertencem à mesma função.

**Escritores e enquadramento nas portas**

A porta declarada para resposta web/API é `responder_pergunta_disputa_core`; ela recusa texto vazio e persiste o par. A guarda comum é `recusa_respondida_sem_texto`. São funções diferentes. (`services/disputa_emissao.py:1425`, `:1432`, `:1508`; `services/fio.py:1578`)

| arquivo:linha:função | ESCRITOR/LEITOR | forma | campo | Como decidiu o modelo; porta |
|---|---|---|---|---|
| `models.py:1509:ChamadoColaborador._universo_da_ressuscitacao` | ESCRITOR — memória | `_q.respondida_em = None` | E | Cópia de `p` em `:1495`, vindo de `self.perguntas_que_resolvem` em `:1484`; relação de PD em `:3415`. **Fora do core**; simulação sem salvar, usada pelo juiz em `:1510`. |
| `models.py:1567:ChamadoColaborador.ressuscitar_perguntas` | ESCRITOR — delegação | `descarimbar_em_lote(..., respondida_em=None)` | E | `alvo` da mesma relação, `:1551`. **Fora do core**; passa pelo descarimbo em lote. |
| `models.py:2809:PerguntaDisputa.abrir` | ESCRITOR — encaminhamento possível | `**campos` para `_sob_contrato` | E/T recebidos | Método da classe PD. Caminho de nascimento; **fora do core**, chega à guarda em `:2864`. |
| `models.py:2821:PerguntaDisputa.abrir_conversa` | ESCRITOR — encaminhamento possível | `**campos` para `_sob_contrato` | E/T recebidos | Classe PD; mesmo enquadramento. |
| `models.py:2840:PerguntaDisputa._sob_contrato` | ESCRITOR — encaminhamento possível | `_nascer(**kw)` | E/T recebidos | `cls` da classe PD; chamadas em `:2809`, `:2821`. |
| `models.py:3039,3042:PerguntaDisputa._nascer` | ESCRITOR | `cls(..., **campos)`; `save(force_insert=True)` | E/T recebidos | `cls` de PD. **Fora do core; chama a guarda** em `:2864`, `:2865`. |
| `models.py:3157,3158,3177:PerguntaDisputa.carimbar` | ESCRITOR | atribuição; acrescenta E a `campos`; `save(update_fields=campos)` | E | `self` de PD. **Fora do core e da guarda comum**; exige texto em `:3155`. |
| `models.py:3188:PerguntaDisputa._desfazer_em_memoria` | ESCRITOR — memória | restaura `self.respondida_em` | E | `self` de PD; desfaz `carimbar`, chamadas em `:3166`, `:3169`, `:3180`. **Fora do core**, sem persistência própria. |
| `models.py:3289,3291:PerguntaDisputa.descarimbar_em_lote` | ESCRITOR | `PerguntaDisputa.objects...update(..., **outros)` | E/T recebidos | Modelo explícito. **Fora do core; HEAD sem guarda do par**. Payloads comprovados em `models.py:1567` e `services/disputa_emissao.py:1630`. |
| `models.py:3448:PerguntaDisputa.save` | ESCRITOR — persistência genérica | `super().save(*args, **kwargs)` | E/T conforme save | `self` de PD. Infraestrutura comum dos saves; **não concentra os `QuerySet.update`**, existentes em `:3291` e `services/disputa_emissao.py:1632`. |
| `services/disputa_emissao.py:1467,1468,1469,1508:responder_pergunta_disputa_core` | ESCRITOR | atribuições T/E; `save(update_fields=_campos)` | E/T | PD consultada em `views.py:1239`, passada ao core em `:1283`. **É o core**; exige texto em `:1432`, `:1434`; não chama a guarda comum. |
| `services/disputa_emissao.py:1593,1630,1632:reabrir_para_correcao` | ESCRITOR | `dict(E=None,T='')`; `descarimbar_em_lote(**campos)` ou `qs.update(**campos)` | E/T | Querysets PD fornecidos em `services/validacao.py:593`, `services/materializacao.py:688`. **Fora do core**; porta de reabertura. |
| `services/disputa_emissao.py:1657,1658,1660:responder_automaticamente` | ESCRITOR | atribuições T/E; `save(update_fields=[T,E])` | E/T | Perguntas do upload, chamada em `app/api/foto_recebida.py:48`. **Fora do core; chama a guarda** em `:1655`. |
| `services/disputa_emissao.py:1679,1681:carimbar_resposta_pela_trilha` | ESCRITOR | atribuição E; `save(update_fields=[E])` | E | PD consultadas pelo comando em `management/commands/curar_perguntas_em_limbo.py:55`, chamada em `:67`, `:68`. **Fora do core e da guarda comum**; guarda própria em `:1675`. |
| `bin/sombra_mascara_pessoal.py:57:<módulo>` | ESCRITOR — sombra | `setattr(o,c,novo)` | T | `apps.get_model(app,mod)` em `:46`; configuração identifica PD/T em `bin/sombra_regras_pessoais.py:85`. **Fora das portas**. |
| `bin/sombra_mascara_pessoal.py:62,64:<módulo>` | ESCRITOR — sombra | `bulk_update(lote,list(campos))` | T | Mesma resolução de modelo/campo. **Fora das portas**; o script exige banco `sombra` em `:10`. |

O nascimento aceita o par por `**campos`, independentemente de haver chamada atual passando essas chaves; sua aceitação e persistência estão no construtor e no save. (`models.py:2864`, `:3039`, `:3042`) Chamador de produção passando explicitamente o par ao nascimento: **[nao verificado]**.

**Leitores no modelo e nos juízes**

| arquivo:linha:função | ESCRITOR/LEITOR | forma | campo | Como decidiu o modelo |
|---|---|---|---|---|
| `models.py:1487:ChamadoColaborador._universo_da_ressuscitacao` | LEITOR | `Q(T__isnull=True)`, `Q(T='')` | T | Relação `perguntas_que_resolvem`, `:1484`; FK PD em `:3415`. |
| `models.py:1565:ChamadoColaborador.ressuscitar_perguntas` | LEITOR | `only(E,T)` | E/T | Mesmo queryset relacionado. |
| `models.py:1849,1850:DisputaSupervisao.respostas_sem_veredito` | LEITOR | atributos | E/T | `self.perguntas` em `:1847`; relação PD em `:2436`, `:2439`. |
| `models.py:1933:DisputaSupervisao.fechar` | LEITOR | atributo T para trilha | T | `self.perguntas` → `vivas`, `:1902`, `:1903`. |
| `models.py:2065:DisputaSupervisao.remover_motivo` | LEITOR | atributo T para trilha | T | Iteração de `self.perguntas.filter`, `:2061`. |
| `models.py:2864,2865:PerguntaDisputa._nascer` | LEITOR — payload | `campos.get(E/T)` | E/T | Método PD; valores destinados ao construtor em `:3039`. |
| `models.py:3070,3073:PerguntaDisputa.estado` | LEITOR | comparação de atributo | E | `self` da classe PD. |
| `models.py:3152,3155,3156:PerguntaDisputa.carimbar` | LEITOR | foto anterior; teste de texto; comparação | E/T | `self` da classe PD. | [nao verificado]
| `models.py:3373,3374:PerguntaDisputa.q_vivas` | LEITOR | `Q(E__isnull=False)`, exclusão de T vazio/nulo | E/T | Método da classe PD. |
| `models.py:3523:PerguntaDisputa.respondida` | LEITOR | atributo `is not None` | E | `self` da classe PD. |
| `models.py:3546,3551,3555,3559,3563,3566,3567,3568:PerguntaDisputa.resposta_display` | LEITOR | atributos; conversão; comparação; retorno | T | `self` da classe PD. |
| `models.py:3949,3956,3957,3958,3959,3963:DecisaoPergunta.linhas_da_projecao_sem_linha` | LEITOR | atributos para projeção da trilha | E/T | Recebe PD em chamadas como `models.py:1564` e `services/disputa_emissao.py:1466`. |
| `juizes.py:348,360:_slot_esperado` | LEITOR | atributos | E/T | Pergunta da cadeia do C6/materialização; chamada em `:752`. |
| `juizes.py:852,853:_chave_memo_pv` | LEITOR | `getattr` literal | E/T | Chamado por `pergunta_viva`, `:890`, `:902`; admite também sondadores, conforme `:844`. |
| `juizes.py:1346:_resposta_concorda_com_a_luz` | LEITOR | atributo | T | Recebe a pergunta do C6 em `:1419`. |
| `juizes.py:1375:projecao_temporal_da_pergunta` | LEITOR | atributos | E/T | Recebe a pergunta do C6 em `:1390`. |
| `juizes.py:1518:respondida_sem_texto` | LEITOR | atributos; `texto_vazio(T)` | E/T | Predicado aplicado às perguntas, inclusive em `models.py:1851`; também recebe sonda em `services/fio.py:1585`. |
| `juizes.py:1665:perguntas_corrigiveis` | LEITOR | filtro ORM | E | `PerguntaDisputa.objects` explícito. |
| `juizes.py:1672,1673,1675:perguntas_respondidas_a_validar` | LEITOR | filtro, exclusão, `order_by(E)` | E/T | `PerguntaDisputa.objects` explícito. |
| `juizes.py:1701:declarou_hora` | LEITOR | atributo | T | Perguntas da materialização, chamada em `services/materializacao.py:555`. |
| `juizes.py:1848:delta_da_resposta` | LEITOR | atributo | T | Pergunta da validação, chamada em `services/validacao.py:864`. |
| `juizes.py:1952,1957:classificar_resposta_validacao` | LEITOR | atributo | T | Fila PD em `app/api/views_mensageria.py:540`, `:544`. |
| `juizes.py:1974:motivo_implausivel_validacao` | LEITOR | atributo | T | Pergunta carregada em `services/validacao.py:389`, chamada em `:404`. |
| `juizes.py:2022,2026:irmas_respondidas_sem_consumo` | LEITOR | filtro ORM; atributo | E/T | `PerguntaDisputa.objects`, `:2020`. |

**Demais leitores de `app/chamados/`**

| arquivo:linha:função | ESCRITOR/LEITOR | forma | campo | Como decidiu o modelo |
|---|---|---|---|---|
| `relatorios_disputa.py:21:classificar` | LEITOR | atributo `is None` | E | `disputa.perguntas.all()` em `:12`. |
| `views.py:1280:responder_pergunta_disputa` | LEITOR | atributo `is not None` | E | Consulta PD em `:1239`. |
| `views.py:1416:reabrir_disputa_supervisao` | LEITOR | filtro ORM | E | `PerguntaDisputa.objects` explícito. |
| `views_cobrar.py:169,176:validar_inline` | LEITOR | atributo; filtro ORM | E | `_vivas_de(d)` e consulta PD em `:175`. |
| `views_cobrar.py:291:proposta_lote` | LEITOR | `x['pergunta'].T` | T | Itens classificados pela fila, `services/validacao.py:921`, `:924`. |
| `canal.py:65:perguntas_mudas_com_resposta` | LEITOR | filtro ORM | T | Consulta PD em `:58`, `:64`. |
| `reconciliador.py:365:lavrar_sem_turno_aberto` | LEITOR | atributo | T | Pergunta recebida pela cadeia de reatar, chamada em `:1620`. |
| `reconciliador.py:1083:veredito_orfa` | LEITOR | atributo | T | Universo PD em `:1048`, `:1049`; chamada em `:1136`. |
| `veredito_lavrado.py:207,208:marcos_do_lote` | LEITOR | filtro E; `**_ate(E)`; `Max(E)` | E | Alias `_PD` em `:201`; `_ate` gera limite em `:163`. |
| `services/acoes_disputa.py:95:acao_reabrir_disputa` | LEITOR | filtro ORM | E | Consulta PD explícita. |
| `services/acoes_disputa.py:130:acao_regularizar_disputa` | LEITOR | atributo | E | Perguntas de `_vivas_de(d)`. |
| `services/acoes_disputa.py:353:acao_corrigir_resposta` | LEITOR | atributo | T | Consulta PD em `:341`. |
| `services/becos.py:30:classificar` | LEITOR | atributo | E | `d.perguntas.all()` em `:29`. |
| `services/cobranca.py:229:_calados` | LEITOR | filtro ORM | E | `PerguntaDisputa.objects` em `:228`. |
| `services/declaracao_texto.py:417,419:declarar_hora_colab` | LEITOR | atributos; `bool(E)` | E/T | Retorno de `PerguntaDisputa.abrir`, `:376`. |
| `services/declaracao_texto.py:687:aplicar_declaracao` | LEITOR | atributo | E | Retorno de `PerguntaDisputa.abrir`, `:666`. |
| `services/disputa_emissao.py:1619,1627:reabrir_para_correcao` | LEITOR | `values_list(T,E)`; nomes `_txt`, `_resp` | E/T | Queryset de perguntas; usa portas PD em `:1609`, `:1630`. |
| `services/disputa_emissao.py:1647:responder_automaticamente` | LEITOR | atributo | T | Perguntas do upload, `app/api/foto_recebida.py:48`. |
| `services/disputa_emissao.py:1675,1683:carimbar_resposta_pela_trilha` | LEITOR | atributos | E/T | Queryset PD do comando, `management/commands/curar_perguntas_em_limbo.py:55`, `:68`. |
| `services/disputa_emissao.py:1718:recobrar_questionario` | LEITOR | filtro ORM | E | Consulta PD em `:1717`. |
| `services/disputa_emissao.py:1826,1844:reabrir_credor_de_pergunta_viva` | LEITOR | atributos | E/T | Relação `perguntas_que_resolvem`, `:1803`, `:1805`. |
| `services/disputa_emissao.py:1984,2057,2062,2074:_resolver_perguntas_por_chamado_resolvido` | LEITOR | atributos; serialização | E/T | Perguntas do chamado tratadas pela função, `:1984`. |
| `services/explicador.py:32:_explicar_validada` | LEITOR | atributo | T | Chamado por `explicar_pergunta`, `:97`; consulta PD em `:122`. |
| `services/explicador.py:79,80:explicar_pergunta` | LEITOR | atributos em textos | T | Consulta PD em `:122`, aplicação em `:133`. |
| `services/fio.py:174:aguarda_o_colaborador` | LEITOR | atributo; predicado do par | E | Coleção PD do fio, carregada em `:242`, encaminhada em `:361`. |
| `services/fio.py:681:processar_post_fio` | LEITOR | atributo | E | `perguntas_vivas_de`, importada em `:680`. |
| `services/fio.py:1254:canonica_do_grupo` | LEITOR | atributo | T | Grupo PD fornecido, por exemplo, por `management/commands/dedup_perguntas_fato.py:74`, `:76`. |
| `services/fio.py:1266:merito_perdedor` | LEITOR | atributo | T | Perguntas do mesmo grupo; constantes PD em `:1265`. |
| `services/foto_e4b.py:180:nascimentos` | LEITOR | `a[c]`, `d[c]`, com `c=E` | E | Ramo `perguntas:` em `:179`; foto PD em `:272`. |
| `services/foto_e4b.py:251:capturar.guardar` | LEITOR | `f.value_from_object(obj)` para todos os campos concretos | E/T | `guardar('perguntas', PerguntaDisputa.objects...)` em `:272`. |
| `services/historico.py:99,105,109:_eventos_dos_chamados` | LEITOR | `getattr(p,campo)` variável; atributo T | E/T | Queryset PD em `:96`; E integra a tupla em `:99`. |
| `services/materializacao.py:280,308:_tipo_pelo_marco` | LEITOR | atributos | T | Cadeia da materialização PD; queryset em `:1375` e carregamento em `services/validacao.py:389`. |
| `services/materializacao.py:359,405,412:_ts_tipo_do_plantio` | LEITOR | atributos | T | Mesma cadeia da materialização. |
| `services/materializacao.py:438:estado_aplicacao_pergunta` | LEITOR | atributo | T | Perguntas carregadas pelo fio em `services/fio.py:242`, chamada em `:357`. |
| `services/materializacao.py:474:_declarar_ausencia_de_pergunta` | LEITOR | atributo | T | Cadeia da materialização PD. |
| `services/materializacao.py:571:_materializar_ausencia_de_resposta` | LEITOR | atributo de `q` | T | `disputa.perguntas.filter`, `:538`. |
| `services/materializacao.py:633:perguntas_absorvidas_por` | LEITOR | filtro `T__in` | T | Base PD em `:629`. |
| `services/materializacao.py:674,675:devolver_absorvidas_da_ausencia_rejeitada` | LEITOR | atributos; foto em dict | E/T | `perguntas_absorvidas_por`, `:671`. |
| `services/materializacao.py:835,876,977:_materializar_resposta_hora` | LEITOR | atributos | T | Cadeia da materialização PD. |
| `services/materializacao.py:1001,1010,1062:_materializar_resposta_data_hora` | LEITOR | atributos | T | Cadeia da materialização PD. |
| `services/materializacao.py:1376:materializar_perguntas_validadas_da_disputa` | LEITOR | filtro ORM | E | `disputa.perguntas.filter`, `:1375`. |
| `services/painel.py:67:marcador_do_placar` | LEITOR | `Max(E)` | E | Agregação PD em `:66`. |
| `services/passivo_validado.py:83:_reversao_de` | LEITOR | `getattr(p,c,None)` | E | `CAMPOS_REVERSAO` em `:64`, `:66`; pergunta recebida em `:367`. |
| `services/pergunta_no_app.py:71:consulta_do_app` | LEITOR | atributo | E | `perguntas_do_app`, `:70`; consulta PD em `:49`. |
| `services/raiox.py:138,143,144:creditos_perguntas` | LEITOR | `values_list(E,T)`; índices `p[3]`, `p[4]` | E/T | `_pq=PerguntaDisputa.objects`, `:56`; seleção em `:135`, `:138`. |
| `services/validacao.py:141:_carimbar_validacao` | LEITOR | atributo | T | Pergunta carregada em `:389`. |
| `services/validacao.py:189,207:devolver_validacao_recusada` | LEITOR | `refresh_from_db(fields=[T,...])`; atributo | T | Instância carregada em `:389`, encaminhada em `:464`. |
| `services/validacao.py:236:_e_aceite_de_folga` | LEITOR | atributo | T | Helper da validação PD. |
| `services/validacao.py:393:validar_pergunta` | LEITOR | atributo | E | Consulta PD em `:389`. |
| `services/validacao.py:576,580,581,600:recusar_pergunta` | LEITOR | atributos; leitura `antes[T]` | E/T | Consulta PD em `:569`, `:570`. |
| `services/validacao.py:851,853,855,860,861,862,863,866:classe_do_lote` | LEITOR | atributos | T | Perguntas da fila, chamadas em `:921`, `:922`. |
| `services/validacao.py:924:fila_validar_classificada` | LEITOR | atributo; idade da resposta | E | Fila padrão em `:918`, `:921`; argumento alternativo `qs` sem validação de tipo. |
| `services/verbos.py:335,336,342,345:placar_aceite` | LEITOR | filtro; `values(E)`; `pg[E]` | E | Consulta PD em `:333`. |
| `services/vigia_fio.py:681:censo_alcance` | LEITOR | atributo | E | Universo PD em `:615`. |
| `services/vigia_fio.py:789,792:respondida_sem_resposta` | LEITOR | filtro E; `values_list(T)` | E/T | Consulta PD explícita em `:789`. |
| `templatetags/chamados_tags.py:142:leitura_da_resposta` | LEITOR | `getattr(perg,T,'')` | T | Recebe `perg` dos templates: `modal_fio.html:250`, `_pergunta_card.html:43`. |

**Leitores nos management commands**

Todos os caminhos desta tabela ficam em `app/chamados/management/commands/`.

| arquivo:linha:função | ESCRITOR/LEITOR | forma | campo | Como decidiu o modelo |
|---|---|---|---|---|
| `curar_perguntas_em_limbo.py:32:nota_da_resposta` | LEITOR | atributo | T | Perguntas PD selecionadas em `:46`. |
| `curar_perguntas_em_limbo.py:46,69:Command.handle` | LEITOR | filtros ORM | E/T | `PerguntaDisputa.objects` explícito. |
| `validar_classe_a.py:80:Command.handle` | LEITOR | atributo | T | `d.respostas_sem_veredito()` em `:54`. |
| `auditar_invariantes_chamados.py:177:_contraprova_i21a` | LEITOR | atributo | E | Subconjunto PD de `:1572`, encaminhado em `:1585`. |
| `auditar_invariantes_chamados.py:592,1105,1106,1572,1575,1603,1655:Command.handle` | LEITOR | atributos; filtros ORM | E/T | Alias `PD.objects`; em `:1655`, relação de perguntas carregada em `:1652`. |
| `backfill_decisoes_pergunta.py:56,60,61:linhas_a_criar` | LEITOR | filtro; `only(E,T)` | E/T | Consulta PD explícita. |
| `backfill_decisoes_pergunta.py:326:aplicar` | LEITOR | atributo para trilha | T | Perguntas do backfill. |
| `backfill_decisoes_pergunta.py:504:Command.handle` | LEITOR | `only(T)` | T | Consulta PD explícita. |
| `dedup_perguntas_fato.py:75:Command.handle` | LEITOR | atributo | T | Queryset PD em `:74`. |

**Leitores fora de `app/chamados/`**

| arquivo:linha:função | ESCRITOR/LEITOR | forma | campo | Como decidiu o modelo |
|---|---|---|---|---|
| `bin/censo_folha.py:28,29:<módulo>` | LEITOR | filtros ORM | E | `PerguntaDisputa.objects`; import em `:12`. |
| `app/folha/previa.py:142:_material` | LEITOR | atributo | T | Mapa PD em `:64`, `:67`, encaminhado em `:95`. |
| `app/ponto/supra_juiz.py:545,548:pode_executar_csf` | LEITOR | atributos | E | Relações `d.perguntas` e `ch.perguntas_que_resolvem`, `:543`, `:548`; FKs em `app/chamados/models.py:2439`, `:3415`. |
| `app/api/views.py:1159,1161:api_chamados_v2` | LEITOR | atributos; guarda e serialização E | E/T | Perguntas de `consulta`, mapa em `:1029`, `:1035`; fonte PD em `services/pergunta_no_app.py:49`. |
| `app/api/views.py:2557,2559:api_disputa_perguntas._item` | LEITOR | atributos; guarda e serialização E | E/T | `consulta_do_app` em `:2532`, `:2535`; chamadas em `:2562`, `:2572`. |
| `app/api/views.py:2573:api_disputa_perguntas` | LEITOR | atributo | E | `consulta['historico']`; mesma origem. |
| `app/api/views.py:2616,2633:api_disputa_responder` | LEITOR | comparação; serialização | E | Consulta PD em `:2607`, `:2609`. |
| `app/api/views_mensageria.py:567,569,570:_item_validar` | LEITOR | atributos; serialização; diferença de datas | E/T | Fila em `:540`, `:546`; consulta PD em `app/chamados/juizes.py:1672`. |
| `app/colaboradores/services/calendario.py:474:contexto_calendario` | LEITOR | `exclude(T='')` | T | Alias PD em `:469`; queryset em `:473`. |
| `app/colaboradores/services/ficha.

[truncado em 20000 de 31100 caracteres]

## 12/11 17:33 · sessao aa3d7d6986fe · TIPO-NA-FONTE · verificador · origem=Code

**Não aprovo como cumprimento integral.** O instrumento entrega listagens e falhas explícitas, mas não garante “todo leitor e escritor”: há perdas silenciosas de proveniência e a medida chamada `acessos_crus_ata` ainda não significa “fora do acessor”. O selo tem mordida parcial e admite ferramentas quebradas em aspectos contratados. (`bin/leitores_de_chave.py:218`, `bin/leitores_de_chave.py:224`, `bin/leitores_de_chave.py:538`, `bin/leitores_de_chave.py:627`; `bin/tests/test_leitores_de_chave.sh:100`)

As referências a `aval.txt` e `contrato.txt` abaixo são aos anexos fornecidos, contando o título como linha 1. Execução histórica do selo, publicação prévia do contrato e pouso: **[não verificado]**. [nao verificado]

**1. As cinco linhas do contrato têm atendimento parcial.**

| Linha | Onde está e onde há furo |
|---|---|
| **Quem chama** | O contrato determina brief com saída anexa. O chamador executável demonstrado é o selo; o juiz existente do CENSO aceita referências textuais, sem conferir saída deste instrumento. Automatização para todo brief que toca ata/dna: **[não verificado]**. (`contrato.txt:2`, anexo; `bin/tests/test_leitores_de_chave.sh:26`; `bin/disjuntor.py:1565`) |
| **Quem espera** | Texto e JSON entregam arquivo, linha, função e papéis. Entrega efetiva ao construtor/preverificador e espera condicionada ao resultado: **[não verificado]**. (`contrato.txt:3`, anexo; `bin/leitores_de_chave.py:291`, `bin/leitores_de_chave.py:663`) |
| **Falha/limite** | Há universo Python, separação produção/testes, declaração de HTML/JS não cobertos, rc 1 nomeando chave sem resultado e rc 2 por erro de parse. O furo é tratar essas garantias como completude do censo: várias perdas de proveniência não geram erro nem lacuna. (`bin/leitores_de_chave.py:34`, `bin/leitores_de_chave.py:224`, `bin/leitores_de_chave.py:555`, `bin/leitores_de_chave.py:569`, `bin/leitores_de_chave.py:617`, `bin/leitores_de_chave.py:669`) |
| **Medida** | Existem resumo por chave e `--contar-crus`. Publicação do censo das 72 chaves no RELATO: **[não verificado]**. O contador não exclui acessor nem compara com teto anterior; portanto fornece uma medida, ainda não a catraca contratada. (`contrato.txt:5`, anexo; `bin/leitores_de_chave.py:627`, `bin/leitores_de_chave.py:631`, `bin/leitores_de_chave.py:648`) |
| **Quem desliga / selo** | Ronald está declarado como autoridade no contrato. O mutante de chave desaparecida tem controle antes e vizinha preservada depois; não é vazio nesse aspecto. A mordida não cobre toda a semântica prometida. (`contrato.txt:6`, anexo; `bin/tests/test_leitores_de_chave.sh:55`, `bin/tests/test_leitores_de_chave.sh:60`, `bin/tests/test_leitores_de_chave.sh:62`, `bin/tests/test_leitores_de_chave.sh:100`) |

O conteúdo dos dois arquivos permanece no instrumento e no selo de host: não implementa acessor, migração de `orfas` ou persistência de instantes. Os modos adicionais servem à inspeção e à medida; chamá-los de catraca pronta antecipa uma garantia ausente. (`bin/leitores_de_chave.py:5`, `bin/leitores_de_chave.py:24`, `bin/leitores_de_chave.py:552`, `bin/leitores_de_chave.py:631`; `bin/tests/test_leitores_de_chave.sh:4`; `aval.txt:5`, anexo)

**2. O AST reconhece candidatos sintáticos; não prova `CelulaDia`.**

Não há regra global “variável chamada `ata` é ata”. Há, porém, dependência de **nomes declarados** por arquivo e função em `PARAMS`, e de nomes de funções em `RETURNS`. Além disso, qualquer atributo `.ata`/`.dna`, qualquer `getattr` com esse literal e qualquer argumento nomeado `ata=`/`dna=` com dicionário são aceitos sem prova da classe ou do destinatário. (`bin/leitores_de_chave.py:39`, `bin/leitores_de_chave.py:66`, `bin/leitores_de_chave.py:128`, `bin/leitores_de_chave.py:218`, `bin/leitores_de_chave.py:249`, `bin/leitores_de_chave.py:493`)

| Forma | Resultado e risco |
|---|---|
| `a=c.ata; a["orfas"]` | Reconhecida por um salto. (`bin/leitores_de_chave.py:166`, `bin/leitores_de_chave.py:221`) |
| `a=c.ata; b=a; b["orfas"]` | Perde o acesso no caso comum: o limite de profundidade interrompe o segundo salto. Ocorrência real de raiz perdida exclusivamente por isso: **[não verificado]**. (`bin/leitores_de_chave.py:224`) |
| `a=b=c.ata` | Reconhece ambos: cada alvo recebe diretamente o mesmo valor. Não equivale ao caso anterior. (`bin/leitores_de_chave.py:134`, `bin/leitores_de_chave.py:138`) |
| Comprehension | Acesso direto à origem é visto; `values_list` recebe tratamento específico. Um caso sintético como `[a["orfas"] for a in [c.ata]]` perde a ligação do elemento. (`bin/leitores_de_chave.py:176`, `bin/leitores_de_chave.py:203`, `bin/leitores_de_chave.py:218`) |
| `getattr(c,"ata")["orfas"]` | Reconhecido. Campo obtido de variável ou chamada indireta `getattr(c.ata,"get")("orfas")` não recebe a mesma resolução. (`bin/leitores_de_chave.py:249`, `bin/leitores_de_chave.py:256`) |
| Reatribuição / ordem | O ambiente agrega atribuições da função inteira, sem invalidar proveniência. Em exemplo sintético `a=c.ata; a={...}; a["orfas"]`, o acesso continua atribuído à ata; uma atribuição posterior também pode contaminar acesso anterior. (`bin/leitores_de_chave.py:127`, `bin/leitores_de_chave.py:168`, `bin/leitores_de_chave.py:227`) |
| Outro objeto com `.ata` | Entra como candidato. Já outro objeto apenas com `.orfas` não entra por esse motivo. (`bin/leitores_de_chave.py:218`, `bin/leitores_de_chave.py:377`) |

**Sítios reais incluídos:** `getattr` em `app/chamados/juizes.py:966`, `app/chamados/reconciliador.py:701` e `app/ponto/services/lastro.py:89`, `app/ponto/services/lastro.py:113`, `app/ponto/services/lastro.py:128`, `app/ponto/services/lastro.py:149`; comprehensions em `app/ponto/juiz_batida.py:161` e `app/relatorios/cartao_pela_celula.py:288`; desempacotamento de `values_list` em `app/ponto/services/vigia_pareador_celula.py:71`, com leituras em `app/ponto/services/vigia_pareador_celula.py:74` e `app/ponto/services/vigia_pareador_celula.py:78`.

**Sítios corretamente excluídos:** a variável chamada `ata` em `app/relatorios/furos.py:132` é mapa de dias, consumido em `app/relatorios/furos.py:134`. As leituras de `orfas` em dias da grade ficam fora em `app/escala/services/furos_diarios.py:22`, `app/folha/causas.py:43`, `app/folha/causas.py:100`, `app/ponto/services/bordas_realizado.py:49`, `app/relatorios/pdf_espelho.py:477` e `app/relatorios/pdf_espelho.py:552`. `FuroDiario.orfas` é um inteiro próprio, não a chave do JSON. (`app/escala/models.py:1883`)

**Perdas reais:** a leitura de `ata["lampadas"]` é vista, mas o laço não transmite proveniência a `lamp`; as escritas de `acesa`, `luz` e `dispensada` somem do censo dessas subchaves. Também ficam fora consultas ORM, conforme limite declarado. (`app/ponto/services/cartorio.py:805`, `app/ponto/services/cartorio.py:808`, `app/ponto/services/cartorio.py:809`, `app/ponto/services/cartorio.py:810`; `app/escala/management/commands/celulas_contra_o_template.py:49`; `app/chamados/models.py:3356`; `bin/leitores_de_chave.py:176`, `bin/leitores_de_chave.py:34`)

**O 95 depende das declarações.** Na reprodução em memória, o total foi 95; sem apenas `PARAMS`, 73; sem `PARAMS` e `RETURNS`, 62. A fórmula conta eventos de topo reconhecidos pelos mapas e pelo rastreamento. (`bin/leitores_de_chave.py:39`, `bin/leitores_de_chave.py:66`, `bin/leitores_de_chave.py:627`)

As 33 operações que desaparecem desligando ambos os mapas estão nestes sítios; multiplicidade indica mais de um evento na mesma linha:

- `app/chamados/juizes.py:438` ×2.
- `app/escala/services/leitor_celula.py:68`, `app/escala/services/leitor_celula.py:82`, `app/escala/services/leitor_celula.py:91`, `app/escala/services/leitor_celula.py:103`, `app/escala/services/leitor_celula.py:105` ×2, `app/escala/services/leitor_celula.py:115`, `app/escala/services/leitor_celula.py:175`, `app/escala/services/leitor_celula.py:187`, `app/escala/services/leitor_celula.py:190`.
- `app/ponto/services/cartorio.py:424`, `app/ponto/services/cartorio.py:426`, `app/ponto/services/cartorio.py:738` ×2, `app/ponto/services/cartorio.py:739`, `app/ponto/services/cartorio.py:740`, `app/ponto/services/cartorio.py:741`, `app/ponto/services/cartorio.py:742`, `app/ponto/services/cartorio.py:745`, `app/ponto/services/cartorio.py:746`, `app/ponto/services/cartorio.py:762`, `app/ponto/services/cartorio.py:767`, `app/ponto/services/cartorio.py:771`, `app/ponto/services/cartorio.py:805`, `app/ponto/services/cartorio.py:812`, `app/ponto/services/cartorio.py:834`, `app/ponto/services/cartorio.py:839`, `app/ponto/services/cartorio.py:913`, `app/ponto/services/cartorio.py:917` ×2.
- `app/relatorios/dia_da_frota.py:21`.

Há uma perda silenciosa concreta por nome declarado: renomear somente o parâmetro `ata` e seus usos em `_orfas_da_ata`, mantendo a função, elimina duas operações e leva 95 a 93. O guard verifica a função, não a existência do parâmetro declarado. (`app/escala/services/leitor_celula.py:180`, `app/escala/services/leitor_celula.py:187`, `app/escala/services/leitor_celula.py:190`; `bin/leitores_de_chave.py:128`, `bin/leitores_de_chave.py:541`)

**3. O selo morde alguns defeitos, mas não sustenta “todos os casos mordem”.**

| Caso | Mordida / vacuidade |
|---|---|
| **a — árvore real** | Exige presença mínima de leitor/escritor nos arquivos esperados e número positivo. Não verifica completude nem valor/delta da medida. (`bin/tests/test_leitores_de_chave.sh:39`, `bin/tests/test_leitores_de_chave.sh:44`) |
| **b — mutante** | Não vazio para desaparecimento: exige controle rc 0, mutante rc 1 nomeado e vizinha rc 0. Uma ferramenta sempre “sem resultado” reprova. (`bin/tests/test_leitores_de_chave.sh:55`, `bin/tests/test_leitores_de_chave.sh:60`, `bin/tests/test_leitores_de_chave.sh:62`) |
| **c — inexistente** | Exige rc 1 nomeado; isoladamente seria vazio para ferramenta sempre negativa, mas b fornece controle positivo. (`bin/tests/test_leitores_de_chave.sh:67`, `bin/tests/test_leitores_de_chave.sh:55`) |
| **d — parse** | Introduz arquivo inválido depois do universo válido e exige rc 2, arquivo nomeado e stdout vazio. (`bin/tests/test_leitores_de_chave.sh:99`, `bin/tests/test_leitores_de_chave.sh:116`, `bin/tests/test_leitores_de_chave.sh:118`) |
| **e — testes** | Confere separação e chave exclusiva de teste. A asserção do contador aceita stdout `8` sem verificar rc. (`bin/tests/test_leitores_de_chave.sh:106`, `bin/tests/test_leitores_de_chave.sh:110`, `bin/tests/test_leitores_de_chave.sh:113`) |
| **f — declaração** | Morde função removida/renomeada; não morde parâmetro renomeado com função preservada. (`bin/tests/test_leitores_de_chave.sh:72`; `bin/leitores_de_chave.py:541`) |
| **g — formas** | Confere exatamente caminhos e formas, mas ignora papéis. Retirar ESCRITOR de `setdefault` e ESCRITOR/EXCLUI de `pop` preserva a comparação. (`bin/tests/test_leitores_de_chave.sh:100`; `bin/leitores_de_chave.py:427`, `bin/leitores_de_chave.py:429`) |
| **uso** | Exige rc 64 por argumento faltando. (`bin/tests/test_leitores_de_chave.sh:122`) |

Todas as invocações do selo usam `CelulaDia.ata`; uma implementação de DNA quebrada pode passar. (`bin/tests/test_leitores_de_chave.sh:36`, `bin/tests/test_leitores_de_chave.sh:55`, `bin/tests/test_leitores_de_chave.sh:99`, `bin/tests/test_leitores_de_chave.sh:122`)

A raiz é calculada por `BASH_SOURCE/../..` e três arquivos são conferidos **antes** de `mktemp`. Isso permite cwd externo; transportar o script para fora da posição `bin/tests` altera a raiz calculada. A prova é de presença, não de identidade do hash. (`bin/tests/test_leitores_de_chave.sh:16`, `bin/tests/test_leitores_de_chave.sh:18`, `bin/tests/test_leitores_de_chave.sh:22`)

No caminho normal, gravações ficam sob `$_tmp`. **Se `mktemp` falhar**, seu retorno não é conferido e `_tmp=''` produz tentativas em `/out`, `/err`, `/copia` e `/sint`. Portanto não há confinamento garantido em todos os caminhos. (`bin/tests/test_leitores_de_chave.sh:15`, `bin/tests/test_leitores_de_chave.sh:22`, `bin/tests/test_leitores_de_chave.sh:28`, `bin/tests/test_leitores_de_chave.sh:50`, `bin/tests/test_leitores_de_chave.sh:78`)

**4. As três saídas não oferecem números intercambiáveis.**

| Saída | Escritor, leitor demonstrado e significado |
|---|---|
| **Texto** | `run()` escreve eventos selecionados por seção; inclui `*` e descendentes compatíveis. O selo usa rc/stderr nas consultas textuais. (`bin/leitores_de_chave.py:635`, `bin/leitores_de_chave.py:659`; `bin/tests/test_leitores_de_chave.sh:55`, `bin/tests/test_leitores_de_chave.sh:60`) |
| **JSON** | `run()` escreve listas filtradas, mas o resumo é global, mesmo consultando uma chave. O selo lê sítios, formas e separação produção/testes. (`bin/leitores_de_chave.py:645`, `bin/leitores_de_chave.py:652`; `bin/tests/test_leitores_de_chave.sh:30`, `bin/tests/test_leitores_de_chave.sh:39`, `bin/tests/test_leitores_de_chave.sh:100`) |
| **`--contar-crus`** | Imprime eventos de produção cujo caminho é exatamente uma chave literal de topo; exclui `*` e caminhos dinâmicos. O selo lê o número real e o sintético. (`bin/leitores_de_chave.py:627`, `bin/leitores_de_chave.py:631`; `bin/tests/test_leitores_de_chave.sh:44`, `bin/tests/test_leitores_de_chave.sh:112`) |

A unidade efetiva é **evento emitido e deduplicado**, com posição, caminho, papéis e forma; não linha nem arquivo. Uma operação com dois papéis continua um evento, portanto somar leitores e escritores pode contar duas vezes. (`bin/leitores_de_chave.py:288`, `bin/leitores_de_chave.py:302`, `bin/leitores_de_chave.py:385`, `bin/leitores_de_chave.py:424`)

Também entram escritas apenas em memória, como `_cj.ata`; o contador não mede exclusivamente persistência. (`app/escala/utils.py:1886`, `app/escala/utils.py:1887`, `app/escala/utils.py:1895`)

Os principais conflitos futuros são: nenhum filtro “fora do acessor”; nenhuma comparação com teto; e total zero retorna rc 1. Assim, a meta zero poderá produzir erro, enquanto acessos internos ao acessor continuarão contados. (`bin/leitores_de_chave.py:627`, `bin/leitores_de_chave.py:633`; `aval.txt:5`, anexo)

Não há evidência suficiente para atribuir a briefs futuros ou à linha HAIKU uma suposição de unidade “linha” ou “arquivo”: **[não verificado]**. Tampouco é legítimo interpretar 174→95 como redução: o aval descreve grep em outro HEAD, e o instrumento define outra seleção por AST. (`aval.txt:1`, anexo; `bin/leitores_de_chave.py:24`)

**5. Classes que já reprovaram obra.**

| Classe | Situação neste recorte |
|---|---|
| **Segundo juiz** | **Não demonstrado como juiz de negócio.** Há classificador sintático sem prova de classe, insuficiente como veredito completo sobre proveniência. (`bin/leitores_de_chave.py:218`, `bin/leitores_de_chave.py:249`) |
| **Segundo escritor** | **Não no instrumento para dados da aplicação:** lê fonte e emite saída. O selo escreve fixtures/cópias, com o furo de `mktemp` descrito. (`bin/leitores_de_chave.py:563`, `bin/leitores_de_chave.py:643`; `bin/tests/test_leitores_de_chave.sh:22`, `bin/tests/test_leitores_de_chave.sh:58`) |
| **Relógio fora da borda** | **Não demonstrado no julgamento.** `date` mede apenas duração do selo. (`bin/tests/test_leitores_de_chave.sh:33`, `bin/tests/test_leitores_de_chave.sh:125`) |
| **Derivação paralela de fonte única** | **Presente na proveniência manual:** `PARAMS/RETURNS` precisam acompanhar o código; o guard incompleto permite divergência silenciosa. (`bin/leitores_de_chave.py:39`, `bin/leitores_de_chave.py:66`, `bin/leitores_de_chave.py:538`) |
| **Selo sem mordida** | **Presente parcialmente:** papéis não conferidos, DNA sem exercício e rc ignorado numa asserção do contador. (`bin/tests/test_leitores_de_chave.sh:100`, `bin/tests/test_leitores_de_chave.sh:113`, `bin/tests/test_leitores_de_chave.sh:122`) |
| **Ordem de trava** | **Não há operação de banco neste instrumento.** A ordem local raiz→temporário está correta, mas falta abortar na falha do temporário. (`bin/leitores_de_chave.py:552`; `bin/tests/test_leitores_de_chave.sh:18`, `bin/tests/test_leitores_de_chave.sh:22`) |
| **Pouso antes do pré-requisito** | **[não verificado]**: anexos e código não provam cronologia de verificação, publicação ou pouso. | [nao verificado]
| **Campo com dois significados** | **Presente no nome da medida:** `acessos_crus_ata` designa todos os eventos reconhecidos de topo, enquanto o contrato exige acessos fora do acessor. (`bin/leitores_de_chave.py:24`, `bin/leitores_de_chave.py:627`; `contrato.txt:5`, anexo) |

## 12/11 17:32 · sessao d7de2a47bc6a · - · - · origem=Ronald (tmux)

Censo de escritores do HEAD por AST, sem execução do app e sem gravações. A árvore não contém `.git`; a identidade do HEAD informado não pôde ser verificada independentemente `[não verificado]`.

`R = respondida_em`; `T = resposta_colab`.

| arquivo:linha:função | papel | forma/campo | prova do modelo e porta |
|---|---|---|---|
| `app/chamados/models.py:1509:ChamadoColaborador._universo_da_ressuscitacao` | ESCRITOR, memória | `_q.respondida_em = None` — R | `_q = copy.copy(p)` em `:1495`; `p` vem de `self.perguntas_que_resolvem` em `:1484`, reverse FK de PerguntaDisputa declarada em `:3415`. Não salva a cópia; simula estado para `pergunta_viva` em `:1510`. |
| `app/chamados/models.py:1567:ChamadoColaborador.ressuscitar_perguntas` | ESCRITOR, delegação | `PerguntaDisputa.descarimbar_em_lote(..., respondida_em=None)` — R | `alvo` vem da mesma reverse FK em `:1551`. Passa pela porta de descarimbo em lote, não pelo core de resposta. |
| `app/chamados/models.py:2809:PerguntaDisputa.abrir` | encaminhamento de payload | `**campos` → `_sob_contrato` — R/T se recebidos | Método da classe PerguntaDisputa, encaminha sem selecionar campos. |
| `app/chamados/models.py:2821:PerguntaDisputa.abrir_conversa` | encaminhamento de payload | `**campos` → `_sob_contrato` — R/T se recebidos | Mesmo modelo; nascimento de conversa. |
| `app/chamados/models.py:2840:PerguntaDisputa._sob_contrato` | encaminhamento de payload | `cls._nascer(**kw)` — R/T se recebidos | `cls` de PerguntaDisputa; os dois chamadores estão em `:2809/:2821`. |
| `app/chamados/models.py:3039:PerguntaDisputa._nascer` | ESCRITOR | `p = cls(..., **campos)`; `p.save(force_insert=True)` em `:3042` — R/T | `cls` de PerguntaDisputa. Guarda `recusa_respondida_sem_texto` em `:2864/:2865`. Fora do core de resposta. O código aceita o payload; chamador de produção passando explicitamente R/T no nascimento não foi encontrado. |
| `app/chamados/models.py:3157:PerguntaDisputa.carimbar` | ESCRITOR | atribuição R; `campos.append('respondida_em')` em `:3158`; `save(update_fields=campos)` em `:3177` | `self` de PerguntaDisputa. Guarda própria exige texto em `:3155`. Fora do core e da função `recusa_respondida_sem_texto`. |
| `app/chamados/models.py:3188:PerguntaDisputa._desfazer_em_memoria` | ESCRITOR, memória | restaura R a partir de `antes` | `self` de PerguntaDisputa; restauração na recusa/exceção de `carimbar`, chamadas em `:3166/:3169/:3180`. Sem persistência própria. |
| `app/chamados/models.py:3289:PerguntaDisputa.descarimbar_em_lote` | ESCRITOR | queryset `.update(..., **outros)` termina em `:3291` — R/T conforme payload | Queryset explicitamente `PerguntaDisputa.objects`; recebe R em `:1567` e R/T no caminho `disputa_emissao.py:1630`. Fora do core; HEAD não valida o par aqui. |
| `app/chamados/models.py:3448:PerguntaDisputa.save` | persistência genérica | `super().save(*args, **kwargs)` — pode persistir R/T | `self` de PerguntaDisputa. HEAD só envolve a persistência em atomic, sem guarda do par (`:3438/:3448`). É infraestrutura comum dos saves, não reúne o caminho `QuerySet.update`. |
| `app/chamados/services/disputa_emissao.py:1467:responder_pergunta_disputa_core` | ESCRITOR | T em `:1467`, R em `:1468`; `_campos` em `:1469`; `save(update_fields=_campos)` em `:1508` | `p` vem comprovadamente de `PerguntaDisputa.objects` em `chamados/views.py:1239`, passado ao core em `:1283`. É a porta de resposta web/API declarada em `disputa_emissao.py:1425/:1429`; recusa texto vazio em `:1432/:1434`. |
| `app/chamados/services/disputa_emissao.py:1593:reabrir_para_correcao` | ESCRITOR | `dict(respondida_em=None, ..., resposta_colab='')`; `descarimbar_em_lote(qs, **campos)` em `:1630` ou `qs.update(**campos)` em `:1632` | Chamadores dão queryset explícito de PerguntaDisputa em `validacao.py:593`, `materializacao.py:688`, `curar_perguntas_em_limbo.py:69`. Fora do core; porta de reabertura. |
| `app/chamados/services/disputa_emissao.py:1657:responder_automaticamente` | ESCRITOR | T em `:1657`, R em `:1658`; `save(update_fields=['resposta_colab','respondida_em'])` em `:1660` | Modelo determinado pelo chamador de foto e seu conjunto de perguntas, não só pelo nome `p`; chamada `api/foto_recebida.py:48`. Guarda comum em `disputa_emissao.py:1655`. Fora do core de resposta web/API. |
| `app/chamados/services/disputa_emissao.py:1679:carimbar_resposta_pela_trilha` | ESCRITOR | atribui R; `save(update_fields=['respondida_em'])` em `:1681` | O comando obtém os objetos com `PerguntaDisputa.objects.filter(...)` em `curar_perguntas_em_limbo.py:55` e chama em `:67/:68`. Guarda própria em `disputa_emissao.py:1675`. Fora do core e da guarda comum. |

Há um escritor operacional adicional, explicitamente restrito à sombra:

| arquivo:linha:função | papel | forma/campo | prova do modelo |
|---|---|---|---|
| `bin/sombra_mascara_pessoal.py:57:<módulo>` | ESCRITOR | `setattr(o, c, novo)` — T | `M=apps.get_model(app,mod)` em `:46`; `o` vem de `M._base_manager` em `:51`; `TEXTO[('chamados','PerguntaDisputa')]` inclui T em `bin/sombra_regras_pessoais.py:85`. |
| `bin/sombra_mascara_pessoal.py:62:<módulo>` | ESCRITOR | `M._base_manager.bulk_update(lote,list(campos))` — T; lote cheio | Mesma origem acima. |
| `bin/sombra_mascara_pessoal.py:64:<módulo>` | ESCRITOR | mesmo `bulk_update` — T; sobra | Mesma origem acima. |

O script exige `connection.settings_dict['NAME'] == 'sombra'` em `bin/sombra_mascara_pessoal.py:10`; portanto não prova uma segunda porta viva no banco de produção. A presença do escritor de T no código está comprovada pelas linhas acima.

A função `app/chamados/services/fio.py:1578:recusa_respondida_sem_texto` **não é um escritor de PerguntaDisputa**: instancia apenas `SimpleNamespace` em `:1585`, consulta o predicado e pode levantar em `:1587`. No HEAD, os escritores que a chamam são `_nascer` (`models.py:2864`) e `responder_automaticamente` (`disputa_emissao.py:1655`). O core humano, a trilha, o carimbo e a reabertura têm caminhos distintos nos pontos da tabela. Assim, não há UMA porta que concentre todas as escritas físicas enumeradas.

Casos excluídos após prova:

- `app/chamados/services/explicador.py:78`: `base.update(...)` atualiza dict, não queryset; utiliza T apenas na construção dos textos em `:79/:80`.
- `app/chamados/veredito_lavrado.py:250`: `PerguntaDisputa.objects.bulk_update(...)` usa `CAMPOS_DA_LAVRA`, definido em `:237` sem R/T.
- `app/core/reexibir.py:49`: `setattr(obj,campo,valor)` genérico, sem persistência; os chamadores encontrados são `colaboradores/views.py:1097/:1262` e trabalham com Colaborador, não PerguntaDisputa.
- `app/chamados/services/passivo_validado.py:83`: `getattr(p,c,None)` percorre `CAMPOS_REVERSAO` (`:64/:66`) para um snapshot colocado na saída em `:367`; não há restauração desse snapshot no arquivo.
- `app/chamados/services/foto_e4b.py:180`: leitura dos dicts antes/depois, não atribuição.

Não houve escritor selecionado somente pelo nome da variável: os escritores da tabela foram atribuídos por classe, queryset/relação, chamador ou registro `apps.get_model`.

Sobre escritores que mudam o desenho:

- `responder_automaticamente` é outra operação persistente de resposta comprovada, chamada por upload de foto em `api/foto_recebida.py:48`; não passa pelo core humano (`disputa_emissao.py:1657/:1660`).
- `carimbar_resposta_pela_trilha` é outra operação persistente comprovada, chamada por management command com `--apply` em `curar_perguntas_em_limbo.py:64/:68`; não passa pelo core humano.
- `PerguntaDisputa.carimbar` pode também persistir R (`models.py:3157/:3177`), além do carimbo de materialização.
- Migrações que obtêm o modelo histórico: `0051_decisao_pergunta.py:23` atualiza somente `colaborador_id` em `:25` e `instancia_marco` em `:27`; `0053_pergunta_unica_por_familia.py:37` somente consulta colisões em `:39/:46`. Os campos R/T em `0014_perguntadisputa.py:24/:25` são declaração de schema. Não foi encontrado escritor de dados R/T nas migrations examinadas.
- Receivers de PerguntaDisputa fecham beco (`signals.py:229/:240`) ou emitem eventos (`:252/:259`), sem escrita direta do par nesses corpos.

No WIP fornecido, as atribuições e updates listados permanecem; acrescentam-se validações no `save` e no lote. Isso acrescenta uma guarda compartilhada a mais caminhos, mas não transfere as escritas para uma única função — prova no anexo `wip_1b.diff`, hunks de `models.py` iniciados em `@@ -3279` e `@@ -3442`. A execução dos testes e o estado efetivo de produção são `[não verificado]`. [nao verificado]

## 12/11 17:31 · sessao 1cf1df545dd1 · - · - · origem=Ronald (tmux)

Censo AST somente leitura concluído para `app/chamados/**/*.py`, excluindo `tests/`, `migrations/`, `models.py` e `juizes.py` (estes ficaram com o agente principal). Atributos `Load`, kwargs ORM, literais de projeção, `getattr`, índices e nomes provenientes de tuplas foram conferidos no código.

Legenda: `E=respondida_em`, `T=resposta_colab`; todas as linhas abaixo são **LEITOR**. Caminhos relativos a `app/chamados/`.

| arquivo:linha:função | forma/campo | origem/modelo |
|---|---|---|
| `canal.py:65:perguntas_mudas_com_resposta` | filtro T | `PerguntaDisputa.objects`, 58,64 |
| `reconciliador.py:365:lavrar_sem_turno_aberto` | atributo T | recebe pergunta; chamada em 1620 a partir de `reatar_pergunta_orfa`; origem por encadeamento, não anotação |
| `reconciliador.py:1083:veredito_orfa` | atributo T | universo `_orfas_abertas`, `PerguntaDisputa.objects` em 1048-1049; chamada 1136 |
| `views_cobrar.py:169:validar_inline` | atributo E | `_vivas_de(d)`/perguntas vivas; origem `PerguntaDisputa` |
| `views_cobrar.py:176:validar_inline` | filtro E | `PerguntaDisputa.objects` 175 |
| `views_cobrar.py:291:proposta_lote` | `x['pergunta'].resposta_colab` T | itens da fila/classificação de perguntas |
| `veredito_lavrado.py:207-208:marcos_do_lote` | filtro E, `**_ate('respondida_em')`, `Max(E)` | alias `_PD=PerguntaDisputa`, 201; `_ate` 163 gera filtro de corte |
| `views.py:1280:responder_pergunta_disputa` | atributo E | `PerguntaDisputa.objects.filter` 1239 |
| `views.py:1416:reabrir_disputa_supervisao` | filtro E | `PerguntaDisputa.objects` na própria linha |
| `relatorios_disputa.py:21:classificar` | atributo E | `disputa.perguntas.all()` 12; `vivas` 18 |
| `services/cobranca.py:229:_calados` | filtro E | `PerguntaDisputa.objects` 228 |
| `services/vigia_fio.py:681:censo_alcance` | atributo E | universo `PerguntaDisputa.objects` 615 |
| `services/vigia_fio.py:789,792:respondida_sem_resposta` | filtro E / `values_list(T)` | `PerguntaDisputa.objects` 789 |
| `services/passivo_validado.py:83:_reversao_de` | `getattr(p,c)` E | `CAMPOS_REVERSAO` 64-66; chamada 367, universo `classificar(empresas)` 324 |
| `services/materializacao.py:280,308:_tipo_pelo_marco` | atributos T | parâmetro `p` da cadeia de materialização; chamadas originam do queryset de perguntas 1375-1382 e da validação `PerguntaDisputa.objects.get` em `validacao.py:389,463` |
| `services/materializacao.py:359,405,412:_ts_tipo_do_plantio` | atributos T | mesma cadeia |
| `services/materializacao.py:438:estado_aplicacao_pergunta` | atributo T | chamada por `fio.py:356`, sobre perguntas do chamado; usa também constantes `PerguntaDisputa` 444 |
| `services/materializacao.py:474:_declarar_ausencia_de_pergunta` | atributo T | mesma cadeia de materialização |
| `services/materializacao.py:571:_materializar_ausencia_de_resposta` | atributo T de `q` | `disputa.perguntas.filter` 538 |
| `services/materializacao.py:633:perguntas_absorvidas_por` | filtro T | `base=PerguntaDisputa.objects.filter` 629 |
| `services/materializacao.py:674-675:devolver_absorvidas_da_ausencia_rejeitada` | atributos T/E; serialização para dict | `ps=perguntas_absorvidas_por` 671; origem PD 629 |
| `services/materializacao.py:835,876,977:_materializar_resposta_hora` | atributos T; dict de saída | mesma cadeia de materialização |
| `services/materializacao.py:1001,1010,1062:_materializar_resposta_data_hora` | atributos T; dict de saída | mesma cadeia |
| `services/materializacao.py:1376:materializar_perguntas_validadas_da_disputa` | filtro E | `disputa.perguntas.filter` 1375 |
| `services/validacao.py:141:_carimbar_validacao` | atributo T | `perg` carregada por `validar_pergunta`, `PerguntaDisputa.objects.get` 389 |
| `services/validacao.py:189,207:devolver_validacao_recusada` | `refresh_from_db(fields=[T])`; atributo T | chamada 464 com a instância carregada em 389 |
| `services/validacao.py:236:_e_aceite_de_folga` | atributo T | helper da validação PD |
| `services/validacao.py:393:validar_pergunta` | atributo E | `PerguntaDisputa.objects.get` 389 |
| `services/validacao.py:576,580-581,600:recusar_pergunta` | atributos E/T; leitura `antes[T]` | `PerguntaDisputa.objects...get` 569-570 |
| `services/validacao.py:851,853,855,860-863,866:classe_do_lote` | atributos T | fila de `perguntas_respondidas_a_validar`, chamada 921-922 |
| `services/validacao.py:924:fila_validar_classificada` | atributo E | `perguntas_respondidas_a_validar()` 918,921; parâmetro `qs` alternativo não é validado por tipo |
| `services/pergunta_no_app.py:71:consulta_do_app` | atributo E | `perguntas_do_app` 70, cuja consulta é `PerguntaDisputa.objects` 49 |
| `services/declaracao_texto.py:417,419:declarar_hora_colab` | atributos E/T | retorno `PerguntaDisputa.abrir` 376 |
| `services/declaracao_texto.py:687:aplicar_declaracao` | atributo E | retorno `PerguntaDisputa.abrir` 666 |
| `services/verbos.py:335-336,342,345:placar_aceite` | filtro E; `values(E)`; `pg[E]` | `PerguntaDisputa.objects` 333 |
| `services/disputa_emissao.py:1619,1627:reabrir_para_correcao` | `values_list(T,E)`; nomes `_txt`,`_resp` na condição | queryset de PD; método chama portas PD em 1609,1630 |
| `services/disputa_emissao.py:1647:responder_automaticamente` | atributo T | parâmetro `p`, contrato e chamadas de resposta PD |
| `services/disputa_emissao.py:1675,1683:carimbar_resposta_pela_trilha` | atributos E/T | parâmetro `p`; chamada no command `curar_perguntas_em_limbo` sobre PD |
| `services/disputa_emissao.py:1718:recobrar_questionario` | filtro E | `PerguntaDisputa.objects` 1717 |
| `services/disputa_emissao.py:1826,1844:reabrir_credor_de_pergunta_viva` | atributos E/T | `p` pergunta; `carga` de relações `perguntas_que_resolvem`, 1803-1805 |
| `services/disputa_emissao.py:1984,2057,2062,2074:_resolver_perguntas_por_chamado_resolvido` | atributos E/T | universo perguntas do chamado, PD importada 17 |
| `services/explicador.py:32:_explicar_validada` | atributo T | chamada 97 por `explicar_pergunta`; origem queryset PD 122,133 |
| `services/explicador.py:79-80:explicar_pergunta` | atributo T | `PerguntaDisputa.objects.filter` 122; chamada 133 |
| `services/foto_e4b.py:180:nascimentos` | `a[c]`,`d[c]`, c=E | somente ramo `chave.startswith('perguntas:')` 179; foto associa perguntas a PD 254,272,311 |
| `services/raiox.py:138,143-144:creditos_perguntas` | `values_list(E,T)`, `p[3]`,`p[4]` | `_pq=PerguntaDisputa.objects` 56, `_vivas` 74,135-138 |
| `services/painel.py:67:marcador_do_placar` | `Max(E)` | `PerguntaDisputa.objects.aggregate` 66 |
| `services/becos.py:30:classificar` | atributo E | `d.perguntas.all()` 29 |
| `services/acoes_disputa.py:95:acao_reabrir_disputa` | filtro E | `PerguntaDisputa.objects` na linha |
| `services/acoes_disputa.py:130:acao_regularizar_disputa` | atributo E | `_vivas_de(d)` |
| `services/acoes_disputa.py:353:acao_corrigir_resposta` | atributo T | `PerguntaDisputa.objects...get` 341 |
| `services/fio.py:174:aguarda_o_colaborador` | atributo E | coleção de perguntas recebida; chamadores e contrato PD |
| `services/fio.py:681:processar_post_fio` | atributo E | `perguntas_vivas_de` importada 680 |
| `services/fio.py:1254:canonica_do_grupo` | atributo T | contrato retorno PD 1248; grupos carregados de PD, por exemplo command dedup 74,76 |
| `services/fio.py:1266:merito_perdedor` | atributo T | perguntas do mesmo grupo; constante PD 1265 |
| `services/historico.py:99,105,109:_eventos_dos_chamados` | campo variável `getattr(p,campo)` E; atributo T | `PerguntaDisputa.objects` 96, laço 97-105 |
| `templatetags/chamados_tags.py:142:leitura_da_resposta` | `getattr(perg,T,'')` | parâmetro de filtro de template; passagem por juiz de resposta 144-147; tipo não validado em runtime |
| `management/commands/curar_perguntas_em_limbo.py:32:nota_da_resposta` | atributo T | `p` de queryset PD em `handle`, 46 |
| `management/commands/curar_perguntas_em_limbo.py:46,69:Command.handle` | filtros E/T | `PerguntaDisputa.objects` |
| `management/commands/validar_classe_a.py:80:Command.handle` | atributo T | `d.respostas_sem_veredito()` 54, guardadas em `pendentes` e `classe_a` |
| `management/commands/auditar_invariantes_chamados.py:177:_contraprova_i21a` | atributo E | chamada 1585 sobre subconjunto de `_i21_cru`, queryset PD 1572 |
| `management/commands/auditar_invariantes_chamados.py:592,1105-1106,1572,1575,1603,1655:Command.handle` | atributos E/T e filtros E/T | alias `PD.objects`; 1655 relação `perguntas_que_resolvem` 1652 |
| `management/commands/backfill_decisoes_pergunta.py:56,60-61:linhas_a_criar` | filtro E; `only(E,T)` | `PerguntaDisputa.objects` 56 |
| `management/commands/backfill_decisoes_pergunta.py:326:aplicar` | atributo T | itens de perguntas do backfill |
| `management/commands/backfill_decisoes_pergunta.py:504:Command.handle` | `only(T)` | `PerguntaDisputa.objects` na linha |
| `management/commands/dedup_perguntas_fato.py:75:Command.handle` | atributo T | `ps=PerguntaDisputa.objects...` 74 |

Casos que **não** devem entrar como escritor/leitor do modelo diretamente:

- `services/fio.py:1585:recusa_respondida_sem_texto`: kwargs E/T são de **`types.SimpleNamespace`**, importado em 1583; o código cria um sondador para o predicado, não `PerguntaDisputa`. É guarda do payload, não porta que persiste. `fio.py:1578-1585`.
- `services/validacao.py:605`: dict `depois` enviado ao observador `_evento`, não escrita ORM. A limpeza real passa por `reabrir_para_correcao` em 593-594.
- `services/disputa_emissao.py:1686`: chave E em dict de observabilidade, não escrita de modelo.
- `services/materializacao.py:674-678`: dict `_foto` guarda valores lidos, não os persiste de volta.
- `services/passivo_validado.py:66`: declaração de nomes; a leitura real é 83.
- `services/disputa_emissao.py:1469,1660,1681`: literais dos campos em `save`, portanto pertencem à tabela de **escritores**, não projeções leitoras.
- `services/disputa_emissao.py:1593`: dict de limpeza E/T, portanto payload de **escritor**; persistência em 1630 ou ramo subsequente.

Pontos de cautela de tipo: as funções auxiliares acima recebem parâmetros sem anotação; a evidência é o encadeamento de chamadas e querysets indicado. As funções `aguarda_o_colaborador`, `fila_validar_classificada(qs=...)`, filtros de template e helpers públicos não verificam em runtime que todo argumento é PD; afirmar isso para qualquer chamador externo seria `[nao verificado]`. Nenhum homônimo de outro modelo apareceu neste recorte da varredura AST; a exceção concreta é o `SimpleNamespace` em `fio.py:1585`.

## 12/11 17:31 · sessao 144396724f9c · - · - · origem=Ronald (tmux)

Censo AST de `.py` fora de `app/chamados`, excluindo diretórios `tests/` e `migrations/`; templates por expressões ativas, excluindo comentários. `E = respondida_em`; `T = resposta_colab`.

| arquivo:linha:função | papel | forma/campo | prova do modelo |
|---|---|---|---|
| `bin/censo_folha.py:28,29:<módulo>` | LEITOR | filtros ORM `E__isnull` | `PerguntaDisputa.objects` explícito; import `:12` |
| `app/folha/previa.py:142:_material` | LEITOR | atributo T | Chamador `:95`; mapa alimentado de `PerguntaDisputa.objects` em `:64-67` |
| `app/ponto/supra_juiz.py:545,548:pode_executar_csf` | LEITOR | atributo E, `is not None` | Itera `d.perguntas` e `ch.perguntas_que_resolvem` em `:543-548`; relações declaradas por `PerguntaDisputa` em `app/chamados/models.py:2436-2439,3410-3415` |
| `app/api/views.py:1159,1161:api_chamados_v2` | LEITOR | T; E duas vezes, serialização e guarda | `perguntas_por_chamado` alimentado de `consulta['pendentes'] + consulta['historico']` em `:1029-1035`; fonte `app/chamados/services/pergunta_no_app.py:49-58,70-75` |
| `app/api/views.py:2557,2559:api_disputa_perguntas._item` | LEITOR | T; E duas vezes, serialização e guarda | `consulta_do_app` em `:2532-2535`; chamadas `_item(p)` `:2562,2572`; mesma fonte acima |
| `app/api/views.py:2573:api_disputa_perguntas` | LEITOR | atributo E, `is not None` | `consulta['historico']`; fonte acima |
| `app/api/views.py:2616,2633:api_disputa_responder` | LEITOR | E comparação e serialização | Objeto consultado explicitamente por `PerguntaDisputa.objects` em `:2607-2609` |
| `app/api/views_mensageria.py:567,569,570:_item_validar` | LEITOR | T; E serialização e diferença de datas | Chamador `:540-546` usa `perguntas_respondidas_a_validar()`; origem explícita `PerguntaDisputa.objects` em `app/chamados/juizes.py:1672-1676` |
| `app/colaboradores/services/calendario.py:474:contexto_calendario` | LEITOR | `.exclude(T='')` | `_PD = PerguntaDisputa`, import `:469`; queryset `:473` |
| `app/colaboradores/services/ficha.py:171:ficha_do_colab` | LEITOR | atributo T | `perguntas_do_app` em `:166`; fonte explícita em `app/chamados/services/pergunta_no_app.py:49-58` |
| `app/ponto/services/cartorio.py:1641:_julgar_colab_corpo._tem_resposta_do_colab` | LEITOR | atributo T | Chamador fornece `x.perguntas_que_resolvem.all()` em `:1645`; relação em `app/chamados/models.py:3410-3415` |
| `app/ponto/services/dia_do_colab.py:70:dia_do_colab` | LEITOR | atributo T | `for p in PerguntaDisputa.objects.filter(...)` em `:64-65` |
| `app/ponto/services/veredito_celula.py:197:fechar_o_fio_do_dia` | LEITOR | atributo T | `vivas` de `PerguntaDisputa.objects` em `:181-182`; iteração `:189` |
| `app/ponto/management/commands/reconciliar_grade.py:461:Command._relavrar_orfa` | LEITOR | filtro ORM E | `PerguntaDisputa.objects.filter` em `:460-461` |
| `bin/sombra_mascara_pessoal.py:51,54:<módulo>` | LEITOR | `.only('pk', *campos)`; `getattr(o,c)` variável T | `R.TEXTO` → `apps.get_model(app,mod)` em `:44-46`; `bin/sombra_regras_pessoais.py:85` contém `('chamados','PerguntaDisputa')` e `resposta_colab` |
| `bin/sombra_selo_pessoal.py:77:<módulo>` | LEITOR | SQL dinâmico lê T em `coalesce` e regex | Itera `R.TEXTO` `:74-76`, tabela resolvida por `apps.get_model` `:24`; configuração `bin/sombra_regras_pessoais.py:85` |

O mascarador também é **ESCRITOR fora da porta**: `setattr(o,c,novo)` em `bin/sombra_mascara_pessoal.py:57`, persistido por `bulk_update(lote,list(campos))` em `:62,64`. Ele exige banco chamado `sombra` por `assert` em `:10`; não atribuir esse escritor a produção operacional sem delimitar o universo. O código prova a escrita na cópia da sombra; execução efetiva [não verificado].

Metadados, sem leitura do valor do campo: `bin/sombra_regras_pessoais.py:85` declara T; `bin/inventario_pessoal_dos_modelos.py:53-60` e `bin/sombra_selo_pessoal.py:26-29` usam essa declaração para inventariar colunas.

Templates:

| arquivo:linha:contexto | papel | forma/campo | prova do modelo |
|---|---|---|---|
| `app/templates/chamados/partials/_card_atendimento.html:94,155,384` | LEITOR | condições E | Loops sobre `chamado.perguntas_disputa` `:76,384`; fonte `app/chamados/services/fio.py:242,261,271` |
| mesmo `:100` | LEITOR | filtro `default:T`; `E|date` | mesma origem |
| mesmo `:108,135,148,176` | LEITOR | condições/comparações T | mesma origem |
| mesmo `:137` | LEITOR | `default:T`, condição E, `E|date` | mesma origem |
| mesmo `:142,177` | LEITOR | interpolação T com `default` | mesma origem |
| mesmo `:385` | LEITOR | `default:T`, `E|date` | mesma origem |
| `app/templates/chamados/partials/modal_fio.html:185,217` | LEITOR | condições E | Loop `:158` sobre mesma `chamado.perguntas_disputa`; fonte acima |
| mesmo `:219,220` | LEITOR | interpolação T; filtro `E|date` | mesma origem |
| `app/templates/chamados/partials/_pergunta_card.html:24,29,32` | LEITOR | condições E; `E|date` | Contexto explícito `{'perg': _p}` de `_PD.objects.filter` em `app/chamados/views.py:398-405` |
| `app/templates/chamados/partials/_lote_preview.html:24` | LEITOR | interpolação `x.pergunta.T` | `views_cobrar.py:361-363` fornece `f['por_classe'][LOTE_A]`; `services/validacao.py:918-924` põe `pergunta=p` da fila `perguntas_respondidas_a_validar()`; queryset em `juizes.py:1672-1676` |

Nenhum caso dessa tabela foi decidido **somente pelo nome**: todos têm origem rastreável em queryset, chamada ou relação declarada, conforme a última coluna.

Delta dos templates no anexo: o wip retira as condições E das linhas HEAD `94,137,155,384`, substituindo por `resposta_enviada`; preserva a exibição E e as leituras T. No resultado projetado, E continua nas linhas wip `106,143,391`; T nas linhas `106,114,141,143,148,154,182,183,391` — linhas calculadas pelos hunks fornecidos, não por arquivo wip local. A atribuição `resposta_enviada` é outro atributo, não escrita dos dois campos censitados (`wip_1b.diff`, hunk `fio.py`, nova linha `358`).

## 12/11 17:31 · sessao bc99985d1006 · - · - · origem=Ronald (tmux)

- **Não há classificação global pelo nome `ata`/`dna`.** `Name` só ganha proveniência por declaração exata arquivo+função+nome, atribuição, `values_list` ou alias; qualquer `.ata`/`.dna` e `getattr(x, 'ata'/'dna')` é aceito sem comprovar a classe. (`bin/leitores_de_chave.py:127`, `bin/leitores_de_chave.py:140`, `bin/leitores_de_chave.py:176`, `bin/leitores_de_chave.py:218`, `bin/leitores_de_chave.py:249`)

- **O 95 depende das declarações.** Desligar somente `PARAMS` produz 73; desligar `PARAMS+RETURNS` produz 62 em reprodução em memória. A formação do número está em `bin/leitores_de_chave.py:627` e `bin/leitores_de_chave.py:631`; os mapas estão em `bin/leitores_de_chave.py:39` e `bin/leitores_de_chave.py:66`.

- **Os 33 acessos de topo dependentes desses mapas**, agrupados por sítio, são: `app/chamados/juizes.py:438` (2); `app/escala/services/leitor_celula.py:68`, `:82`, `:91`, `:103`, `:105` (2), `:115`, `:175`, `:187`, `:190`; `app/ponto/services/cartorio.py:424`, `:426`, `:738` (2), `:739`, `:740`, `:741`, `:742`, `:745`, `:746`, `:762`, `:767`, `:771`, `:805`, `:812`, `:834`, `:839`, `:913`, `:917` (2); `app/relatorios/dia_da_frota.py:21`. As declarações correspondentes estão em `bin/leitores_de_chave.py:41` e `bin/leitores_de_chave.py:68`.

- **Há furo na validação das declarações:** ela confere existência da função, sem conferir existência do nome declarado. Renomear o parâmetro `ata` e suas referências em `_orfas_da_ata`, mantendo a função, faz desaparecerem os acessos `orfas`/`n_orfas`; a contagem cai de 95 para 93 sem violar essa validação. (`bin/leitores_de_chave.py:128`, `bin/leitores_de_chave.py:541`, `app/escala/services/leitor_celula.py:180`, `app/escala/services/leitor_celula.py:187`, `app/escala/services/leitor_celula.py:190`)

- **Sítios corretamente fora:** a variável chamada `ata` em `app/relatorios/furos.py:132` é mapa da grade indexado por data, consumido em `app/relatorios/furos.py:134`; `dia.get('orfas')` em `app/escala/services/furos_diarios.py:22` recebe dia da grade por `app/escala/services/furos_diarios.py:43` e `app/escala/services/furos_diarios.py:54`; outras leituras da grade ficam em `app/folha/causas.py:43`, `app/folha/causas.py:100`, `app/ponto/services/bordas_realizado.py:49`, `app/relatorios/pdf_espelho.py:477` e `app/relatorios/pdf_espelho.py:552`. `FuroDiario.orfas` é inteiro, separado do JSON da célula. (`app/escala/models.py:1883`)

- **Sítios reais incluídos:** `getattr` em `app/chamados/juizes.py:966`, `app/chamados/reconciliador.py:701`, `app/ponto/services/lastro.py:89`, `:113`, `:128`, `:149`; comprehension lendo a raiz em `app/ponto/juiz_batida.py:161` e `app/relatorios/cartao_pela_celula.py:288`; desempacotamento e alias `a` em `app/ponto/services/vigia_pareador_celula.py:69`, `:73`, `:74`, `:78`. A cópia apenas em memória `_cj.ata` também conta como escrita de código. (`app/escala/utils.py:1886`, `app/escala/utils.py:1887`, `app/escala/utils.py:1895`)

- **Faltas reais declaradas:** leitura ORM de `dna__marcos` em `app/escala/management/commands/celulas_contra_o_template.py:49`; filtro ORM de `ata.lampadas` em `app/chamados/models.py:3356`. A exclusão é declarada em `bin/leitores_de_chave.py:19` e `bin/leitores_de_chave.py:34`.

- **Subchaves em laço comum não propagam proveniência:** `ata.get('lampadas')` é visto, mas `lamp['acesa']`, `lamp['luz']` e `lamp['dispensada']` não são ligados à ata. Isso limita censo de subchaves, embora a operação de topo já seja contada. (`app/ponto/services/cartorio.py:805`, `app/ponto/services/cartorio.py:806`, `app/ponto/services/cartorio.py:808`, `app/ponto/services/cartorio.py:809`, `app/ponto/services/cartorio.py:810`, `bin/leitores_de_chave.py:176`)

- **Alias de dois saltos:** o corte é explícito em `bin/leitores_de_chave.py:224`. Exemplo real de acesso de topo perdido exclusivamente por esse corte: [não verificado]. Atribuição encadeada `a=b=c.ata` liga ambos os alvos ao mesmo valor; não constitui automaticamente dois saltos. (`bin/leitores_de_chave.py:134`, `bin/leitores_de_chave.py:138`, `bin/leitores_de_chave.py:166`)

## 12/11 17:29 · sessao 56a1fc7d5d3d · - · - · origem=Ronald (tmux)

Achados sobre consumidores e semântica:

- O emissor é `run()`: `--contar-crus` imprime `len(top)`; JSON imprime resumo e listas; texto imprime seções e seus comprimentos. São projeções do mesmo censo, mas seus números têm unidades diferentes: operações de topo, arquivos distintos e eventos selecionados. `bin/leitores_de_chave.py:627`, `bin/leitores_de_chave.py:631`, `bin/leitores_de_chave.py:645`, `bin/leitores_de_chave.py:659`.
- No texto, `len(sel)` inclui eventos `*`, descendentes e dinâmicos que casem com a seleção; no JSON, `resumo_producao` é global mesmo numa consulta de uma chave; `producao`/`testes` são filtrados. Não são números intercambiáveis. `bin/leitores_de_chave.py:635`, `bin/leitores_de_chave.py:645`, `bin/leitores_de_chave.py:652`, `bin/leitores_de_chave.py:660`.
- Cada evento guarda posição, caminho, papéis e forma; a deduplicação usa também coluna, papéis e forma. Não é uma unidade “linha”, “arquivo” ou necessariamente “nó AST único”. Um `setdefault`, `pop` ou `+=` conta um evento com dois papéis; somar leitores e escritores superconta as operações. `bin/leitores_de_chave.py:288`, `bin/leitores_de_chave.py:302`, `bin/leitores_de_chave.py:385`, `bin/leitores_de_chave.py:424`.
- Execução somente leitura confirmou `--contar-crus CelulaDia.ata` = 95; JSON produziu 12 chaves, 95 operações de topo, 27 arquivos, 103 eventos de produção e 89 pares arquivo:linha. Os totais são resultado da execução, sem arquivo de resultado persistido: [não verificado]. O cálculo correspondente está em `bin/leitores_de_chave.py:627`.
- Os quatro eventos `*` vistos foram cópias de JSON em `app/ponto/portas/celula.py:945`, `app/ponto/portas/celula.py:995`, `app/ponto/portas/celula.py:1083`, `app/ponto/services/cartorio.py:917`; entram na listagem por chave, mas não no contador. `bin/leitores_de_chave.py:629`, `bin/leitores_de_chave.py:635`.
- O consumidor executável encontrado é o selo: chama a ferramenta, captura stdout/stderr e lê JSON, número e códigos de retorno. O JSON é lido para conferir sítios, formas e separação produção/testes; o número é lido para os casos real e sintético. `bin/tests/test_leitores_de_chave.sh:26`, `bin/tests/test_leitores_de_chave.sh:30`, `bin/tests/test_leitores_de_chave.sh:36`, `bin/tests/test_leitores_de_chave.sh:44`, `bin/tests/test_leitores_de_chave.sh:99`, `bin/tests/test_leitores_de_chave.sh:106`, `bin/tests/test_leitores_de_chave.sh:112`.
- A implementação chama esse número de catraca `acessos_crus_ata`, mas conta todos os eventos de topo de produção; não existe exclusão do futuro acessor nem comparação com teto anterior. Portanto ainda não implementa “acessos crus fora do acessor, só encolhe”. `bin/leitores_de_chave.py:24`, `bin/leitores_de_chave.py:627`, `bin/tests/test_leitores_de_chave.sh:44`.
- Não encontrei consumidor da ferramenta em `gerar_estado`, disjuntor ou briefs disponíveis: integração futura e unidade pressuposta por esses leitores são [não verificado]. O juiz existente do CENSO exige referências textuais de leitores/escritores e decisões, sem conferir saída desse instrumento. `bin/disjuntor.py:1522`, `bin/disjuntor.py:1565`.
- `PARAMS`/`RETURNS` são fonte manual adicional de proveniência. O guard verifica somente existência da função; não verifica nome do parâmetro, tipo nem fluxo. Renomear só o parâmetro pode reduzir o censo sem disparar rc 2. `bin/leitores_de_chave.py:39`, `bin/leitores_de_chave.py:66`, `bin/leitores_de_chave.py:538`.
- O censo publicado, o envio prévio do contrato, a execução histórica do selo, o pouso e o aval em arquivos de `logs/` não estão demonstrados pelos arquivos disponíveis: [não verificado]. A referência local ao contrato é apenas comentário/docstring. `bin/leitores_de_chave.py:10`, `bin/tests/test_leitores_de_chave.sh:2`.

## 12/11 17:28 · sessao 6c4410570fab · - · - · origem=Ronald (tmux)

Auditoria estática; selo não executado, portanto resultado da execução é [não verificado].

- **(a) Morde presença mínima**, exigindo leitor e escritor de `orfas` em dois arquivos e contador numérico positivo; não verifica completude, funções/linhas exatas, valor esperado nem incremento por novo acesso (`bin/tests/test_leitores_de_chave.sh:36`, `bin/tests/test_leitores_de_chave.sh:39`, `bin/tests/test_leitores_de_chave.sh:44`).
- **(b) Mutante não é vazio para ausência global:** primeiro exige `sem_ancora` presente, remove a chave, exige rc 1 nomeado e preserva `tipo_dia` rc 0. Ferramenta que sempre responde “sem resultado” falha no controle (`bin/tests/test_leitores_de_chave.sh:55`, `bin/tests/test_leitores_de_chave.sh:58`, `bin/tests/test_leitores_de_chave.sh:60`, `bin/tests/test_leitores_de_chave.sh:62`).
- **(c) Morde rc 1 nomeado**, mas isoladamente aceitaria uma ferramenta que sempre devolvesse rc 1; o controle de (b) cobre essa quebra global (`bin/tests/test_leitores_de_chave.sh:55`, `bin/tests/test_leitores_de_chave.sh:67`).
- **(f) Morde remoção da função declarada**, exige rc 2, nome e stdout vazio; não comprova validade do parâmetro declarado ou origem real do objeto (`bin/tests/test_leitores_de_chave.sh:72`, `bin/tests/test_leitores_de_chave.sh:74`; `bin/leitores_de_chave.py:541`).
- **(g) Morde desaparecimento/duplicação das oito formas**, pela lista exata de `(caminho, forma)`, mas **não testa papéis**. Retirar `ESCRITOR` de `setdefault` e `ESCRITOR/EXCLUI` de `pop` preserva o observado por (g); o escritor literal real de `orfas` continua satisfazendo (a) (`bin/tests/test_leitores_de_chave.sh:100`, `bin/tests/test_leitores_de_chave.sh:101`; `bin/leitores_de_chave.py:427`, `bin/leitores_de_chave.py:429`; `app/ponto/services/cartorio.py:745`).
- **(e) Morde separação produção/testes e chave exclusiva de teste**, mas o teste de contador apenas compara stdout com `8`: ignora rc. Um contador sintético que imprime `8` e retorna erro passa essa asserção (`bin/tests/test_leitores_de_chave.sh:106`, `bin/tests/test_leitores_de_chave.sh:110`, `bin/tests/test_leitores_de_chave.sh:112`).
- **(d) Morde parse inválido:** o universo antes válido recebe arquivo inválido, e o teste exige rc 2, arquivo nomeado e ausência de número (`bin/tests/test_leitores_de_chave.sh:99`, `bin/tests/test_leitores_de_chave.sh:116`, `bin/tests/test_leitores_de_chave.sh:118`).
- **Uso:** exige rc 64 por argumento faltando (`bin/tests/test_leitores_de_chave.sh:122`).
- **DNA não tem cobertura executável:** todas as invocações usam `CelulaDia.ata`; a ferramenta inteira para DNA pode quebrar sem esse selo perceber (`bin/tests/test_leitores_de_chave.sh:36`, `bin/tests/test_leitores_de_chave.sh:44`, `bin/tests/test_leitores_de_chave.sh:55`, `bin/tests/test_leitores_de_chave.sh:99`, `bin/tests/test_leitores_de_chave.sh:122`).
- **Raiz:** resolve relativamente a `BASH_SOURCE[0]/../..` e prova presença de três arquivos antes de criar temporários. Funciona com cwd externo; transportar o selo para fora da posição `bin/tests` muda a raiz calculada. Prova apenas presença desses arquivos, não identidade do checkout/hash (`bin/tests/test_leitores_de_chave.sh:16`, `bin/tests/test_leitores_de_chave.sh:18`, `bin/tests/test_leitores_de_chave.sh:22`).
- **Gravações:** no caminho normal, saídas, cópias, mutações e fixtures ficam sob `$_tmp`, com Python `-B` (`bin/tests/test_leitores_de_chave.sh:28`, `bin/tests/test_leitores_de_chave.sh:50`, `bin/tests/test_leitores_de_chave.sh:58`, `bin/tests/test_leitores_de_chave.sh:78`). **Há furo se `mktemp` falhar:** seu retorno não é verificado, e há somente `set -u`; com `_tmp=''`, o selo tenta gravar `/out`, `/err`, `/copia` e `/sint`, fora do temporário (`bin/tests/test_leitores_de_chave.sh:15`, `bin/tests/test_leitores_de_chave.sh:22`, `bin/tests/test_leitores_de_chave.sh:28`, `bin/tests/test_leitores_de_chave.sh:50`, `bin/tests/test_leitores_de_chave.sh:78`).

## 12/11 17:23 · sessao b18576745da1 · TRANCA-VIVAS-08 · sonda · origem=Code

1. **Transcrição integral do dado fornecido.** A execução e o hash da sonda não foram conferidos independentemente nesta cópia. [nao verificado]

```text
sonda_leitura: sonda_v2.py [8c23d857275b021a] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

agora 2026-11-12T17:13:38.126957-03:00
PF#7 emp2 04/2026 universo=0 vivas_juiz=0 contador_medir=0
PF#8 emp2 05/2026 universo=24 vivas_juiz=0 contador_medir=0
PF#9 emp2 06/2026 universo=37 vivas_juiz=0 contador_medir=0
PF#10 emp2 07/2026 universo=152 vivas_juiz=0 contador_medir=0
PF#3 emp2 08/2026 universo=12898 vivas_juiz=4 contador_medir=0
  VIVA #20798 col367 disputa#2502 data_evento=2026-08-01 dia_turno=2026-08-01 conversa=False C20=False no_medir=False respondida=False validada=False materializada=True motivo=saida_sem_entrada
  VIVA #47879 col444 disputa#3309 data_evento=2026-08-03 dia_turno=2026-08-03 conversa=False C20=False no_medir=False respondida=True validada=False materializada=False motivo=orfao_14h
  VIVA #47881 col444 disputa#3309 data_evento=2026-08-04 dia_turno=2026-08-04 conversa=False C20=False no_medir=False respondida=True validada=False materializada=False motivo=intervalo_volta
  VIVA #47884 col444 disputa#3309 data_evento=2026-08-05 dia_turno=2026-08-05 conversa=False C20=False no_medir=False respondida=True validada=False materializada=False motivo=intervalo_volta
PF#11 emp3 04/2026 universo=0 vivas_juiz=0 contador_medir=0
PF#12 emp3 05/2026 universo=7 vivas_juiz=0 contador_medir=0
PF#13 emp3 06/2026 universo=28 vivas_juiz=0 contador_medir=0
PF#4 emp3 07/2026 universo=1165 vivas_juiz=0 contador_medir=0
PF#5 emp3 08/2026 universo=2048 vivas_juiz=0 contador_medir=0
PF#14 emp4 04/2026 universo=2 vivas_juiz=0 contador_medir=0
PF#15 emp4 05/2026 universo=63 vivas_juiz=0 contador_medir=0
PF#16 emp4 06/2026 universo=616 vivas_juiz=0 contador_medir=0
PF#2 emp4 07/2026 universo=493 vivas_juiz=0 contador_medir=0
PF#6 emp4 08/2026 universo=399 vivas_juiz=0 contador_medir=0
TOTAL {'universo': 17932, 'vivas': 4, 'medir': 0, 'classe:trancado_False': 4}
total_erros=0
== anexo: retira_acervo_tranca_2.txt ==
aval Ronald: RETIRA ACERVO-TRANCA-2 (publicado 16:3x, contraria CLAUDE.md 7b DINHEIRO item 3: nenhuma trava por exportada/trancada/paga; correcao provada entra em qualquer competencia). Nao executar os itens 1, 2, 3 e 5; nenhuma lei nova. Fica SO o item 4, que e lei existente (TRANCA-DE-VERDADE): as 4 perguntas vivas na comp 08 emp2 trancada -> listar pk e o degrau de chamados/juizes.py::pergunta_viva que as deixou vivas; curar na origem se for defeito; contador vivas_apesar_da_tranca na frota, esperado 0. Segue a fila; PAREI so em lei ou !
```

2. **Por PF, conforme a saída fornecida.** [nao verificado]

| PF | Empresa | Competência | Universo | Vivas juiz | Contador medir | Evidência |
|---|---|---|---:|---:|---:|---|
| 7 | 2 | 04/2026 | 0 | 0 | 0 | [nao verificado] |
| 8 | 2 | 05/2026 | 24 | 0 | 0 | [nao verificado] |
| 9 | 2 | 06/2026 | 37 | 0 | 0 | [nao verificado] |
| 10 | 2 | 07/2026 | 152 | 0 | 0 | [nao verificado] |
| 3 | 2 | 08/2026 | 12898 | 4 | 0 | [nao verificado] |
| 11 | 3 | 04/2026 | 0 | 0 | 0 | [nao verificado] |
| 12 | 3 | 05/2026 | 7 | 0 | 0 | [nao verificado] |
| 13 | 3 | 06/2026 | 28 | 0 | 0 | [nao verificado] |
| 4 | 3 | 07/2026 | 1165 | 0 | 0 | [nao verificado] |
| 5 | 3 | 08/2026 | 2048 | 0 | 0 | [nao verificado] |
| 14 | 4 | 04/2026 | 2 | 0 | 0 | [nao verificado] |
| 15 | 4 | 05/2026 | 63 | 0 | 0 | [nao verificado] |
| 16 | 4 | 06/2026 | 616 | 0 | 0 | [nao verificado] |
| 2 | 4 | 07/2026 | 493 | 0 | 0 | [nao verificado] |
| 6 | 4 | 08/2026 | 399 | 0 | 0 | [nao verificado] |

**Há defeito de UNIVERSO em `vivas_apesar_da_tranca`, quando usado para contar todas as vivas da competência.** A lista nasce vazia e só recebe perguntas no ramo em que pai ou destino pertence a `resgatados`; no ramo comum, apenas perguntas mortas são registradas, e as vivas ficam sem inclusão. Isso contraria o comentário “contador tem que ver o mesmo que quem valida” e a LEI-AKITA 8. (`app/chamados/services/competencia_trancada.py:81`, `app/chamados/services/competencia_trancada.py:119–134`, `CLAUDE.md:35–36`)

Esse recorte é mais estreito que a exceção humana do juiz: `resgatados` exige chamado vivo elegível e `resgatado_em > pf.fechado_em`; o juiz também reconhece “Cobrar este dia” e “Resolver dia”. (`app/chamados/services/competencia_trancada.py:89–106`, `app/chamados/juizes.py:1083–1089`)

Já `r['perguntas']` vazio tem semântica intencional: representa perguntas que seriam carimbadas pela operação, retirada após a mudança para tranca por leitura. Isso não justifica omitir vivas de `vivas_apesar_da_tranca`. (`app/chamados/services/competencia_trancada.py:126–134`)

O significado operacional de `no_medir=False` dentro da sonda, inclusive se representa ausência do filtro SQL ou ausência da lista final, é **[nao verificado]**.

3. **Degrau e motivo de sobrevivência de cada VIVA.**

`pergunta_viva()` delega a `_pergunta_viva_calc()`; a tranca é testada antes da resposta do colaborador. Portanto resposta não ultrapassa uma tranca que tenha retornado `True`. (`app/chamados/juizes.py:888–894`, `app/chamados/juizes.py:1391–1407`)

| VIVA | O que os campos publicados permitem identificar | [nao verificado]
|---|---|
| **#20798** | Com `respondida=False` e `materializada=True`, são compatíveis o retorno por **célula acusando** ou por **materialização sem testemunha suficiente**. O degrau efetivamente tomado é **[nao verificado]**. (`app/chamados/juizes.py:1438–1441`, `app/chamados/juizes.py:1473–1475`) |
| **#47879** | São compatíveis o ramo de **resposta com texto sem concordância comprovada com a luz**, **célula acusando** ou **ausência de materialização**. O degrau efetivamente tomado é **[nao verificado]**. (`app/chamados/juizes.py:1407–1419`, `app/chamados/juizes.py:1438–1441`, `app/chamados/juizes.py:1466–1467`) |
| **#47881** | Mesmas possibilidades: resposta nas condições do juiz, célula acusando ou ausência de materialização. O degrau efetivamente tomado é **[nao verificado]**. (`app/chamados/juizes.py:1407–1419`, `app/chamados/juizes.py:1438–1441`, `app/chamados/juizes.py:1466–1467`) |
| **#47884** | Mesmas possibilidades: resposta nas condições do juiz, célula acusando ou ausência de materialização. O degrau efetivamente tomado é **[nao verificado]**. (`app/chamados/juizes.py:1407–1419`, `app/chamados/juizes.py:1438–1441`, `app/chamados/juizes.py:1466–1467`) |

`respondida=True` sozinho não prova passagem pelo ramo da resposta: esse ramo também exige texto não vazio e ausência de decisão administrativa do dia. Esses campos não foram publicados. (`app/chamados/juizes.py:1405–1419`; valores individuais: [nao verificado])

Existe ainda o retorno por falta dentro da janela viva. Com `hoje=2026-11-12`, a janela calculada pelo código é 20/09–20/11, excluindo os quatro dias de agosto; o argumento efetivamente usado pela sonda é **[nao verificado]**. (`app/chamados/juizes.py:592–618`, `app/chamados/juizes.py:1454–1462`, `app/ponto/janelas.py:10–33`, `app/ponto/janelas.py:36–63`)

**Por que C20 não as calou:** nas quatro linhas aparecem `conversa=False`, dia preenchido e `C20=False`; logo, o dado não sustenta “conversa” nem “dia None” como explicação individual. [nao verificado]

O código transforma conversa em dia de turno `None`; para as demais perguntas usa `data_evento`. O juiz começa em `False`, exige dia/colaborador/empresa, procura o PF da competência calculada e, encontrando-o, retorna `not tocou`. (`app/chamados/juizes.py:334–338`, `app/chamados/juizes.py:991–992`, `app/chamados/juizes.py:1023–1027`, `app/chamados/juizes.py:1062–1079`)

Assim, **`C20=False` não identifica sozinho “sem PF” ou “toque posterior”**. O PF efetivamente encontrado, seu `fechado_em` e a trilha humana de cada pergunta são **[nao verificado]**.

Se o juiz encontrou PF vivo para aquele colaborador/dia, seu resultado `False` corresponde a toque reconhecido. Os atos admitidos são `cobrar_dia_manual`, `veredito_celula` e `desarquivar_chamado`, com usuário identificado diferente do sistema e instante estritamente posterior à tranca. **Qual ato ocorreu para #20798, #47879, #47881 e #47884: [nao verificado].** (`app/chamados/juizes.py:1078–1089`, `app/chamados/juizes.py:1252–1254`, `app/chamados/juizes.py:1268`, `app/chamados/juizes.py:1292`, `app/chamados/juizes.py:1315`)

4. **Escritores, leitores e significado de dia.**

| Campo | Quem escreve | Quem lê / significado |
|---|---|---|
| `conversa` | `abrir()` passa `False`; `abrir_conversa()` passa `True`; nascimento instancia e salva. (`app/chamados/models.py:2790`, `app/chamados/models.py:2802`, `app/chamados/models.py:3015–3018`) | O juiz usa o discriminador para retirar o dia civil da leitura de turno e da tranca. (`app/chamados/juizes.py:334–338`, `app/chamados/juizes.py:991–992`) |
| `data_evento` | A atribuição central é `gravar_dia_do_fato`; nascimento, costura de linha sem dia e rechaveamento passam por ela. (`app/chamados/services/fio.py:791–802`, `app/chamados/models.py:3017`, `app/chamados/services/fio.py:880`, `app/chamados/services/fio.py:1038`) | Fato recebe **dia do turno**; conversa recebe **dia civil local do pedido**, conservado pelo derivador. (`app/chamados/models.py:2784–2803`, `app/chamados/services/fio.py:1631–1636`) |
| `vivas_apesar_da_tranca` | `medir()` inicializa e acrescenta somente no ramo dos resgatados. (`app/chamados/services/competencia_trancada.py:81`, `app/chamados/services/competencia_trancada.py:119–124`) | `medir_frota()` lê seu tamanho, expõe a contagem por competência e a soma em `ficam_com_o_admin`. (`app/chamados/services/competencia_trancada.py:313–320`) |

**Os leitores não aplicam todos a mesma distinção de dia.** O juiz distingue conversa de turno; `medir()` seleciona pela faixa de `data_evento` cru, sem discriminar conversa, e `reabrir()` também usa esse campo cru. Portanto esses filtros podem incluir o dia civil de uma conversa no universo da competência. (`app/chamados/juizes.py:334–338`, `app/chamados/services/competencia_trancada.py:107–109`, `app/chamados/services/competencia_trancada.py:252–253`)

Outro leitor cru é a lavra, que passa `p.data_evento` a `expira_em`; a documentação dessa função ainda associa conversa a dia `None`, enquanto `abrir_conversa()` grava `localdate()`. (`app/chamados/veredito_lavrado.py:131`, `app/chamados/veredito_lavrado.py:273`, `app/chamados/models.py:2802`)

Na exposição do contador, o comando imprime `ficam_com_o_admin` como `decidir_pelo_dp`, mas seu total principal soma somente chamados, perguntas e disputas; a API entrega `por_competencia`, onde está a contagem própria das vivas. (`app/chamados/management/commands/chamados_em_competencia_trancada.py:20–23`, `app/api/views_mensageria.py:1573–1574`)

A saída informa **`total_erros=0`**. [nao verificado]

## 12/11 17:21 · sessao d3492b1ae160 · - · - · origem=Ronald (tmux)

- **Defeito de universo do contador, quando apresentado como todas as perguntas vivas apesar da tranca.** `medir()` inicializa `vivas_apesar_da_tranca=[]` e só acrescenta perguntas quando o pai ou destino pertence a `resgatados`; o ramo geral acrescenta somente perguntas mortas a `perguntas_lidas_trancadas`, deixando vivas sem inclusão no contador. O próprio comentário exige contador igual à fila; LEI-AKITA 8 exige “contador == universo”. (`app/chamados/services/competencia_trancada.py:77`, `:119`, `:123`, `:124`, `:130`, `:133`; `CLAUDE.md:35`)
- A diferença de universo é mais estreita que “houve toque humano”: `resgatados` contém somente chamados com status VIVO, competência do chamado igual à medida, módulo sujeito à tranca e `resgatado_em > pf.fechado_em`. A exceção do juiz abrange também Cobrar dia e Resolver dia. (`app/chamados/services/competencia_trancada.py:89`, `:93`, `:97`, `:101`, `:105`; `app/chamados/juizes.py:1071`, `:1083`)
- **Não confundir o significado operacional de `perguntas=[]` com o defeito de `vivas_apesar_da_tranca`.** A primeira lista é vazia por construção porque as perguntas saem da fila por leitura e não são mais carimbadas; isso é semântica explicitamente documentada. A segunda pretende enxergar vivas que permanecem na fila, mas só tem o append restrito aos resgatados. (`app/chamados/services/competencia_trancada.py:126`, `:128`, `:120`, `:124`)
- A causa específica de cada `no_medir=False`, além do que literalmente diz a saída fornecida, é **[não verificado]**: não localizei fonte `sonda_v2.py` nesta exploração, e o código da sonda não foi fornecido.

**Escritor e leitores do contador:**

| Papel | Evidência |
|---|---|
| Inicialização e único append da lista | `app/chamados/services/competencia_trancada.py:81`, `:124` |
| `medir_frota()` lê a lista, acrescenta seu tamanho a `ficam_com_o_admin` e expõe `vivas_apesar_da_tranca` por competência | `app/chamados/services/competencia_trancada.py:313`, `:316`, `:317` |
| A soma de frota não inclui chave própria para total `vivas_apesar_da_tranca`; a informação fica dentro de `por_competencia` | `app/chamados/services/competencia_trancada.py:311`, `:318`, `:320` |
| Command lê `medir_frota`, imprime `ficam_com_o_admin` como `decidir_pelo_dp`; total principal soma só chamados + perguntas + disputas | `app/chamados/management/commands/chamados_em_competencia_trancada.py:13`, `:20`, `:23` |
| API lê `medir_frota` e devolve `ficam_com_o_admin` e `por_competencia` | `app/api/views_mensageria.py:1562`, `:1573`, `:1574` |
| Cliente HTTP e ferramenta propagam resposta da API | `mensageria/nucleo/core_client.py:239`, `:241`; `mensageria/nucleo/ferramentas.py:2035`, `:2036` |
| Placar lê somente total principal do command, sem campo próprio de vivas | `bin/placar_code.sh:287`, `:289` |
| Command de encerramento enumera várias listas, mas omite a lista `vivas_apesar_da_tranca` dessa impressão | `app/chamados/management/commands/encerrar_por_competencia_trancada.py:36`, `:38`, `:39` |

O `medir()` usa a janela de fechamento e filtro direto em `data_evento`; para chamados usa `competencia_do_chamado`. O juiz da tranca usa dia do turno e competência calculada por `janela_atual`; assim os leitores não usam todos a mesma expressão de dia, embora uma igualdade entre os dois dias seja mostrada nas quatro linhas da saída fornecida. (`app/chamados/services/competencia_trancada.py:87`, `:93`, `:108`; `app/chamados/juizes.py:984`, `:992`, `:1062`)

## 12/11 17:21 · sessao 2b245d6a8a87 · - · - · origem=Ronald (tmux)

Evidências dos campos:

- `conversa`: `abrir()` passa `False` em `app/chamados/models.py:2790`; `abrir_conversa()` passa `True` e `localdate()` em `app/chamados/models.py:2802`; `_nascer()` instancia e salva em `app/chamados/models.py:3015` e `app/chamados/models.py:3018`. O default é `False` em `app/chamados/models.py:2436`.
- Chamadores de conversa: reabertura administrativa em `app/chamados/services/acoes_disputa.py:263`; emissão sem dia de turno em `app/chamados/services/disputa_emissao.py:431` e `app/chamados/services/disputa_emissao.py:439`; `ancora_ausente` em `app/chamados/services/disputa_emissao.py:1371`.
- `data_evento`: atribuição centralizada em `app/chamados/services/fio.py:801`; nascimento chama e salva em `app/chamados/models.py:3017`; costura de linha sem dia chama em `app/chamados/services/fio.py:880` e salva em `app/chamados/services/fio.py:928`; rechaveamento chama em `app/chamados/services/fio.py:1038` e salva em `app/chamados/services/fio.py:1044`. A costura que data recusa conversa em `app/chamados/services/fio.py:876`; o rechaveamento também em `app/chamados/services/fio.py:1019`.
- Semântica do escritor: fatos recebem dia do turno; conversa recebe dia civil. As duas portas provam essa distinção em `app/chamados/models.py:2790` e `app/chamados/models.py:2802`; o derivador conserva o pedido/gravado da conversa em `app/chamados/services/fio.py:1631`.
- Leitor juiz: `_dia_de_turno_da_pergunta()` transforma conversa em `None` e lê `data_evento` nos demais casos em `app/chamados/juizes.py:334`; a tranca usa esse helper em `app/chamados/juizes.py:991`. `resolvedora_valida()` também reconhece conversa e retorna sem conferir turno em `app/chamados/juizes.py:2260`.
- Leitor `medir()`: filtra `data_evento` cru entre `ini/fim`, sem excluir `conversa`, em `app/chamados/services/competencia_trancada.py:107`. Portanto seu universo pode incluir dia civil de conversa; o universo do juiz distingue conversa. `reabrir()` também filtra o campo cru em `app/chamados/services/competencia_trancada.py:252`.
- Outro leitor cru: `lavrar()` calcula `expira_em(p.data_evento, ...)` em `app/chamados/veredito_lavrado.py:273`; a docstring de `expira_em` ainda associa conversa a dia `None` em `app/chamados/veredito_lavrado.py:131`, enquanto a porta grava `localdate()` em `app/chamados/models.py:2802`. Não afirmar que todos os leitores adotam a mesma interpretação.
- Contador `vivas_apesar_da_tranca`: lista inicializada em `app/chamados/services/competencia_trancada.py:81`; único `append` de produção encontrado no ramo dos chamados resgatados, condicionado ao juiz, em `app/chamados/services/competencia_trancada.py:119` e `app/chamados/services/competencia_trancada.py:124`. No ramo comum, pergunta viva não recebe append; só mortas entram em `perguntas_lidas_trancadas`, em `app/chamados/services/competencia_trancada.py:133`.
- `medir_frota()` lê `len` da lista tanto para `ficam_com_o_admin` quanto para a linha própria do contador em `app/chamados/services/competencia_trancada.py:316`; agrega apenas `ficam_com_o_admin` e os outros três totais em `app/chamados/services/competencia_trancada.py:318`.
- Comando da frota imprime `ficam_com_o_admin` como `decidir_pelo_dp` em `app/chamados/management/commands/chamados_em_competencia_trancada.py:20`; o total final soma somente chamados/perguntas/disputas, omitindo vivas/ficam_com_o_admin, em `app/chamados/management/commands/chamados_em_competencia_trancada.py:23`.

## 12/11 17:21 · sessao c74d56b50639 · - · - · origem=Ronald (tmux)

`pergunta_viva()` delega à lei em `_pergunta_viva_calc()`; com carga usa diretamente o cálculo, sem carga pode reutilizar memo por instância. (`app/chamados/juizes.py:862`, `app/chamados/juizes.py:888`, `app/chamados/juizes.py:890`, `app/chamados/juizes.py:894`)

A saída fornecida **não identifica o degrau exato nem o toque humano específico de nenhuma das quatro VIVAs**. [não verificado]

| Pergunta | Degraus compatíveis com os campos publicados, sem horizonte temporal diferente | [nao verificado]
|---|---|
| #20798: respondida=False, materializada=True | Célula acusa → `True` em `app/chamados/juizes.py:1441`; falta na janela viva → `True` em `app/chamados/juizes.py:1462`; carimbo sem materialização de fato → retorno em `app/chamados/juizes.py:1473`. Qual ocorreu: [não verificado]. |
| #47879, #47881, #47884: respondida=True, materializada=False | Resposta **com texto**, sem decisão admin e sem concordância comprovada com a luz → retorno em `app/chamados/juizes.py:1419`; célula acusa → `app/chamados/juizes.py:1441`; falta na janela → `app/chamados/juizes.py:1462`; ausência de materialização → `app/chamados/juizes.py:1467`. Qual ocorreu: [não verificado]. |

O carimbo `respondida=True` sozinho não prova que o ramo da resposta foi tomado: ele exige também texto não vazio e ausência da decisão admin descrita no código. (`app/chamados/juizes.py:1405`, `app/chamados/juizes.py:1407`, `app/chamados/juizes.py:1415`)

O ramo da falta depende da janela anterior/corrente, calculada com `hoje` recebido ou `date.today()`; não é lícito assumir que toda VIVA chegou nesse ramo. (`app/chamados/juizes.py:584`, `app/chamados/juizes.py:592`, `app/chamados/juizes.py:612`, `app/chamados/juizes.py:618`, `app/chamados/juizes.py:1455`)

`C20=False` explica apenas que a tranca não encerrou a pergunta no teste de `app/chamados/juizes.py:1395`; a causa individual não está publicada. [não verificado]

O leitor de dia retorna `None` para conversa e `data_evento` para as demais; `dia_da_pergunta_trancado()` passa esse dia ao juiz em lote. (`app/chamados/juizes.py:334`, `app/chamados/juizes.py:338`, `app/chamados/juizes.py:991`)

As quatro linhas dizem `conversa=False` e exibem dia não nulo, portanto não sustentam “era conversa” ou “dia None” como causa. [não verificado]

O juiz começa em `False`; só considera pares com dia, colaborador e empresa. Calcula a competência pela janela e procura PF vivo; apenas PF encontrado vai ao teste de toque, cujo resultado final é `not tocou`. (`app/chamados/juizes.py:1023`, `app/chamados/juizes.py:1025`, `app/chamados/juizes.py:1062`, `app/chamados/juizes.py:1066`, `app/chamados/juizes.py:1069`, `app/chamados/juizes.py:1078`)

PF vivo significa empresa/mês/ano correspondentes e `reaberto_em IS NULL`. (`app/ponto/services/fechamento.py:1464`, `app/ponto/services/fechamento.py:1470`)

Assim, `C20=False` sozinho não separa ausência de empresa/PF resolvido de toque posterior. O PF correspondente efetivamente lido e seu `fechado_em` não foram publicados. [não verificado] [nao verificado]

Os atos admissíveis são `cobrar_dia_manual`, `veredito_celula` e `desarquivar_chamado`; a trilha deve ter usuário diferente do sistema e instante estritamente posterior à tranca. (`app/chamados/juizes.py:1083`, `app/chamados/juizes.py:1087`, `app/chamados/juizes.py:1252`, `app/chamados/juizes.py:1268`, `app/chamados/juizes.py:1292`, `app/chamados/juizes.py:1315`)

Qual desses atos ocorreu para cada VIVA: **[não verificado]**.

## 12/11 17:16 · sessao 4d8764033314 · BUG-CADASTRO-NAO-ALCANCA-O-FUTURO · verificador · origem=Code

**REPROVO a pré-verificação:** há ampliação de escrita em **HOJE** pelo cron e D1 permite apagar julgamento sem garantir a trilha obrigatória. Também permanece a classe “relógio fora da borda”. Evidências: [cron](/home/ronald/codex_ws/4c2e280349fa07be8d1c423ebf46f1df56a1255d/app/ponto/management/commands/gerar_celulas.py:321), [limpeza D1](/home/ronald/codex_ws/4c2e280349fa07be8d1c423ebf46f1df56a1255d/app/ponto/portas/celula.py:1148), [trilha best-effort](/home/ronald/codex_ws/4c2e280349fa07be8d1c423ebf46f1df56a1255d/app/core/observ.py:105), [relógio](/home/ronald/codex_ws/4c2e280349fa07be8d1c423ebf46f1df56a1255d/app/ponto/services/cadastro_futuro.py:22).

**1. Portas e alcance no Salvar**

O suplemento comum seleciona somente células **geradas, existentes, posteriores a hoje, dentro da janela corrente**, respeitando `desde`/`datas`; chama a mesma porta com horizonte desligado. (`app/ponto/services/cadastro_futuro.py:22`, `:24`, `:28`, `:41`)

| Porta | Função/caminho | Alcança futura existente? |
|---|---|---|
| Estrutura: município | `atualizar_posto` / `definir_municipio_posto` → `_evento_do_municipio` → `reavaliar_pela_troca_de_municipio` | Sim. (`app/colaboradores/services/estrutura.py:59`, `:65`, `:88`; `app/colaboradores/services/vinculo.py:947`) |
| Criar/alterar vínculo | `executar_vinculo` → suplemento explícito | Sim. (`app/colaboradores/services/vinculo.py:330`) |
| Alterar vigência por `save` | `_ec_vigencia_regenera_celulas` | Sim. (`app/escala/signals.py:80`) |
| Fechar/encerrar vínculos | `fechar_vigencia`; `encerrar_vinculo` também provoca o signal | Sim. (`app/colaboradores/services/vinculo.py:649`, `:829`; `app/escala/signals.py:81`) |
| Reabrir vigência impossível | `reabrir_vigencia_impossivel` | Sim, no mesmo atomic. (`app/colaboradores/services/vinculo.py:716`, `:737`) |
| Restaurar snapshot | `reverter_para_snapshot` | Sim. (`app/colaboradores/services/vinculo.py:1016`, `:1020`) |
| Trabalhar em feriado | `marcar_trabalha_em_feriado` | Sim. (`app/colaboradores/services/vinculo.py:897`) |
| Âncora | `_ec_ancora_regenera_celulas` | Sim. (`app/escala/signals.py:132`) |
| Tipo pela tela | `salvar_tipo` → `_propagar_futuras_template` | Sim, incluindo vínculos encerrados. (`app/escala/services/cadastro_tipo.py:326`, `:373`) |
| Tipo pelo admin | `save_model` → `salvar_tipo_no_admin` | Sim; acrescenta somente propagação futura. (`app/escala/admin.py:11`; `app/escala/services/cadastro_tipo.py:380`) |
| Folga manual/foto | `lancar_folga`, `alterar_folga`, lote/reparentamento → `relavrar_celulas_da_foto` | Sim. (`app/escala/folgas.py:298`; `app/ponto/tests/test_cadastro_futuro_c3_folgas.py:51`, `:57`, `:71`, `:79`, `:86`) |
| Calendário de folga | Salvar calendário/trocar calendário → `sincronizar_foto_futura_do_calendario` | Sim. (`app/escala/folgas.py:598`, `:638`; `app/colaboradores/services/vinculo.py:459`) |
| Configuração de feriado | Formulário/importação/exclusão → signals → `regenerar_futuras_por_feriado` | Sim, nas datas afetadas, incluindo a data antiga quando movido. (`app/core/views_config.py:70`, `:83`, `:144`, `:201`; `app/core/signals.py:19`, `:33`, `:39`) |
| Escala automática | `aplicar_no_vinculo` / `desfazer` → regeneração explícita | Sim; aqui cria ausentes e estende horizonte por ato declarado. (`app/escala/services/escala_auto_executor.py:107`, `:147`) |
| Correção retroativa | `_aplicar` → trecho histórico + suplemento | Sim. (`app/colaboradores/management/commands/corrigir_escala_retroativa.py:231`, `:243`) |

Nos trechos históricos de cadastro examinados, o novo suplemento não amplia o alcance: vínculo mantém início→hoje; município mantém somente hoje quando coberto; template mantém janela anterior→hoje; folgas mantêm o ramo histórico; correção mantém piso/topo anteriores. (`app/colaboradores/services/vinculo.py:319`, `:323`, `:941`; `app/escala/services/cadastro_tipo.py:337`, `:339`, `:351`; `app/escala/folgas.py:290`; `app/colaboradores/management/commands/corrigir_escala_retroativa.py:225`, `:228`)

**Há, porém, ampliação proibida na obra:** o cron antigo enumerava somente `ativa=True`; o novo `_regenerar_hoje_existentes` regenera células de **HOJE de vínculo inativo** respondido pela juíza. O teste exige precisamente essa reescrita. (`app/ponto/portas/celula.py:829`, versão anterior no diff fornecido; `app/ponto/management/commands/gerar_celulas.py:316`, `:321`, `:323`; `app/ponto/tests/test_cadastro_futuro_c4.py:165`)

**2. D1: limpeza, trilha e fato humano**

A limpeza ocorre após reescrever DNA no ramo normal, no ramo sem vínculo e na reversão. Anula os 13 campos de julgamento e usa `salvar_e_emitir`, preservando a emissão do evento da célula. (`app/ponto/portas/celula.py:262`, `:307`, `:832`, `:1103`, `:1146`; `:948`)

**Defeito: “com trilha” não está garantido.** A função ignora o retorno de `evento`; esse registrador engole falhas, desfaz apenas a tentativa de trilha e retorna `None`. Assim, a limpeza pode confirmar sem o registro antes/depois. A própria `editar_dia` já contém o tratamento contrário: exige trilha e levanta erro se ela faltar. (`app/ponto/portas/celula.py:1149`; `app/core/observ.py:105`, `:124`, `:139`; `app/ponto/portas/celula.py:643`)

O único detector de decisão humana nessa limpeza é `veredito_via == 'admin'`: conserva o julgamento, registra “conservado” e retorna normalmente **depois de o DNA ter sido escrito**. Não inspeciona o conteúdo da ata nem interrompe o Salvar. Portanto, o comentário que chama isso de “PAREI” descreve conservação/contagem, não uma parada operacional. (`app/ponto/portas/celula.py:301`, `:1128`, `:1136`, `:1140`)

Ausência/folga/atestado externos permanecem intactos: a limpeza altera somente atributos da célula, incluindo sua FK `ausencia`. O teste preserva o objeto externo. Existência de ata contendo fato humano em produção: **[não verificado]**. (`app/ponto/portas/celula.py:1103`, `:1146`; `app/ponto/tests/test_cadastro_futuro_c5.py:165`)

| Escritor que não passa pela limpeza | Constatação | Escopo |
|---|---|---|
| `editar_dia` | Pode escrever DNA futuro e marcar origem editada; não chama a limpeza. (`app/ponto/portas/celula.py:617`, `:633`) | Fora de “Salvar regenerando cadastro”; é edição humana. |
| `julgar_colab` em lote | Os laços recebem células sem a guarda futura do avulso e podem lavrar/carimbar. (`app/ponto/services/cartorio.py:1212`, `:1353`, `:1851`; guarda avulsa `:2025`) | Fora da regeneração de cadastro; continua caminho de julgamento futuro. |
| `veredito_celula` | Pode criar ausência e lavrar julgamento admin sem guarda de data futura. (`app/ponto/services/veredito_celula.py:211`, `:332`, `:351`) | Fora de “Salvar regenerando”; continua capaz de produzir julgamento futuro. |

Esses caminhos não são omissões da chamada D1 no **Salvar regenerando**; são limites da promessa global “futura com ata/carimbo = 0”, que o vigia também observa. (`app/ponto/services/vigia_cadastro_futuro.py:29`, `:45`)

**3. D2 e `substitui_dono`**

Nos eventos de atributo, o suplemento pergunta `vinculos_dos_dias` e permite somente dono igual ao vínculo ou resposta `None`; não calcula outro dono localmente. Portanto, `None` permite seguir para a porta, que verifica cobertura. (`app/ponto/services/cadastro_futuro.py:34`, `:37`; `app/ponto/portas/celula.py:224`)

As chamadas com `substitui_dono=True` encontradas são somente atos de linha do tempo: executar vínculo, fechar/reabrir vigência, restaurar snapshot, correção retroativa e signal de vigência. (`app/colaboradores/services/vinculo.py:335`, `:651`, `:739`, `:1020`; `app/colaboradores/management/commands/corrigir_escala_retroativa.py:245`; `app/escala/signals.py:83`)

**Município sem o antigo filtro não é defeito D2.** A chamada de hoje continua condicionada à cobertura hoje; a chamada futura passa pelo suplemento com `substitui_dono=False` e seu filtro de dono. (`app/colaboradores/services/vinculo.py:941`, `:947`; `app/ponto/services/cadastro_futuro.py:34`)

**4. D3, D5, D6, D7 e feriado**

| Item | Verificação |
|---|---|
| D3 | Realizado: futura sem sucessor recebe geradora nula, `montar_dna(None, ...)`, DNA anterior e carimbos de regeneração; a célula permanece. (`app/ponto/portas/celula.py:246`, `:255`, `:256`, `:257`) |
| D5 | Realizado: reabertura e suplemento compartilham o atomic; suplemento só seleciona futuras existentes. (`app/colaboradores/services/vinculo.py:716`, `:737`; `app/ponto/services/cadastro_futuro.py:24`, `:28`) |
| D6 | Realizado: aplicar/desfazer declaram `estender_horizonte=True`; início/volta são futuros. (`app/escala/services/escala_auto_executor.py:100`, `:108`, `:143`, `:148`) |
| D7, corpo do gerador | Realizado: inativo somente em `d > hoje`, escolhido pela juíza; cria ausente e preserva futura existente. (`app/ponto/portas/celula.py:899`, `:904`, `:921`) |
| D7, cron completo | **Não fica restrito a `d > hoje`**: a casca adicional reescreve hoje de inativo, conforme achado 1. (`app/ponto/management/commands/gerar_celulas.py:323`; `app/ponto/tests/test_cadastro_futuro_c4.py:165`) |
| Feriado | Realizado: `DateField.to_python` canoniza na fronteira; salvar entrega datas nova/antiga, excluir entrega a excluída; serviço recebe datas e consulta a juíza. (`app/core/signals.py:19`, `:25`, `:33`, `:39`; `app/ponto/services/cadastro_futuro.py:57`, `:67`) |

**5. Vigia e linha de CADASTRO**

O vigia lê `vinculo_do_dia` com a geradora como alimentação; sem geradora, usa o carregador da mesma autoridade. A candidata (a) não foi implementada: o ramo da geradora continua sem exigir `ativa`. (`app/ponto/services/vigia_cadastro_futuro.py:41`, `:58`; `app/escala/alimentacao.py:269`, `:282`, `:284`)

Vínculo inativo sem fim respondido pela juíza entra em `na_lista_do_admin_c6`; a porta recusa regenerá-lo. A lista administrativa C6 usa o predicado canônico de vínculo sem vigência. Isso classifica CADASTRO, sem cura automática. (`app/ponto/services/vigia_cadastro_futuro.py:69`; `app/ponto/portas/celula.py:148`; `app/escala/services/cadastro_realidade.py:320`, `:829`)

Os quatro indicadores têm esperado zero; o cron mede antes da geração, lavra depois, e a API lê snapshot. Ausência de medição retorna `None`, não zero. O **N de produção exigido antes de construir**, comprovado pelo dado raspado fornecido: **[não verificado]**. (`app/ponto/services/vigia_cadastro_futuro.py:86`, `:99`, `:119`; `app/ponto/management/commands/gerar_celulas.py:346`, `:353`; `app/api/views_mensageria.py:647`)

**6. Quem escreve e quem lê os campos**

| Campo | Escritores persistentes | Leitores identificados |
|---|---|---|
| `CelulaDia.ata` | Lavra, carimbo de precedência e limpeza D1, todos na porta. (`app/ponto/portas/celula.py:1055`, `:1082`, `:1187`, `:1146`) | Grade/folha, cartório, chamados, relatórios e vigia. (`app/escala/services/leitor_celula.py:379`; `app/folha/previa.py:83`; `app/ponto/services/cartorio.py:451`; `app/chamados/juizes.py:450`; `app/relatorios/cartao_pela_celula.py:287`; `app/ponto/services/vigia_cadastro_futuro.py:45`) |
| `dna` | Conteúdo produzido por `montar_dna`; persistido por geração, regeneração, edição humana e reversão. (`app/ponto/management/commands/gerar_celulas.py:73`; `app/ponto/portas/celula.py:256`, `:301`, `:633`, `:819`, `:904`, `:915`) | Marcos/pausas da escala, grade, cartório, julgamento admin, chamados e vigia. (`app/escala/models.py:1124`, `:1189`, `:1235`; `app/escala/utils.py:46`; `app/ponto/services/cartorio.py:1851`; `app/ponto/services/veredito_celula.py:31`; `app/chamados/juizes.py:948`; `app/ponto/services/vigia_cadastro_futuro.py:60`, `:79`) |
| `regenerada_em` | Regeneração e reversão. (`app/ponto/portas/celula.py:258`, `:303`, `:826`) | Cobrança órfã/carimbos de reescrita, fase, lavra e cartório. (`app/escala/services/regeneracao.py:84`, `:101`, `:119`; `app/escala/services/fase_12x36.py:189`; `app/chamados/veredito_lavrado.py:193`; `app/ponto/services/cartorio.py:393`) |
| `regeneracoes` | Incrementos na regeneração e reversão. (`app/ponto/portas/celula.py:259`, `:304`, `:827`) | Os próprios incrementos leem o valor anterior; outro consumidor de produção: **[não verificado]**. |
| `dna_anterior` | Regeneração guarda; reversão limpa. (`app/ponto/portas/celula.py:257`, `:302`, `:820`) | Reversão e comando operacional. (`app/ponto/portas/celula.py:810`, `:816`; `app/ponto/management/commands/aplicar_col900.py:173`) |
| `EscalaColaborador.data_fim` | Porta de vínculo: reativar, fechar, reabrir, encerrar, corrigir/criar trecho, snapshot e desfazer automático. (`app/colaboradores/services/vinculo.py:271`, `:629`, `:717`, `:831`, `:958`, `:968`, `:1018`, `:1088`) | Cobertura, vínculo do dia, candidatura, signals e cadastro. (`app/escala/alimentacao.py:150`, `:189`, `:205`, `:228`; `app/escala/signals.py:25`, `:57`; `app/escala/services/cadastro_realidade.py:306`) |
| `EscalaColaborador.ativa` | Porta de vínculo, incluindo criação, absorção e restauração. (`app/colaboradores/services/vinculo.py:270`, `:286`, `:629`, `:717`, `:786`, `:830`, `:959`, `:1018`, `:1075`, `:1088`, `:1094`) | Candidatura/cobertura, vínculo do dia, D7 e cadastro. (`app/escala/alimentacao.py:150`, `:189`, `:205`, `:228`; `app/ponto/portas/celula.py:899`; `app/escala/services/cadastro_realidade.py:256`, `:306`) |

A tabela identifica escritores e leitores concretos; **censo global exaustivo de todos os consumidores de `ata`/`dna` e, especialmente, `data_fim`/`ativa`: [não verificado]**.

O JUIZES confirma autoridades distintas para vínculo do dia e cobertura, além de grade/precedência/ausência/feriado. Não autoriza substituir uma pergunta pela outra. (`app/docs/JUIZES.md:103`, `:105`, `:107`, `:108`, `:121`, `:123`)

**7. Classes de reprovação**

| Classe | Presença/ausência demonstrada |
|---|---|
| Segundo juiz | Ausente nos dois serviços novos: usam autoridades existentes de vínculo, cobertura e produtor de DNA. (`app/ponto/services/cadastro_futuro.py:35`, `:41`, `:67`; `app/ponto/services/vigia_cadastro_futuro.py:26`, `:58`, `:79`) |
| Segundo escritor fora da porta | Ausente nesses serviços: suplemento chama a porta; vigia não salva células. (`app/ponto/services/cadastro_futuro.py:41`; `app/ponto/services/vigia_cadastro_futuro.py:43`, `:83`) |
| Relógio fora da borda | **Presente:** defaults leem `localdate()` no interior; há chamadores reais sem `hoje`. É exatamente a armadilha descrita no plano. (`app/ponto/services/cadastro_futuro.py:22`, `:53`; `app/core/signals.py:33`; `app/escala/services/cadastro_tipo.py:386`; `app/docs/PLANO-PADROES.md:92`, `:94`) |
| Derivação paralela da fonte única | Ausente na decisão de vínculo dos serviços novos; comparação de cadastro reutiliza `montar_dna`. (`app/ponto/services/cadastro_futuro.py:36`; `app/ponto/services/vigia_cadastro_futuro.py:58`, `:79`) |
| Selo/RED sem mordida | Existem mutantes executáveis D1/D3 e D2/D5/D6/D7/feriado; **não cobrem a exigência de abortar quando falta trilha D1**. O teste de admin exige conservação com selo 1. (`app/ponto/tests/test_cadastro_futuro_c5.py:27`, `:151`, `:199`; `app/ponto/tests/test_cadastro_futuro_c5b.py:30`; `app/ponto/portas/celula.py:1149`) |
| Ordem de trava | Snapshot executa vínculo→célula; há teste dessa sequência. Concorrência real PostgreSQL: **[não verificado]**. (`app/colaboradores/services/vinculo.py:1005`, `:1011`; `app/ponto/tests/test_cadastro_futuro_c3.py:228`) |
| Pouso antes do pré-requisito | Pouso, ordem efetiva E3/E4B e certificação completa: **[não verificado]** pelo material autorizado. | [nao verificado]
| Campo com dois significados | `ativa` e vigência têm usos distintos explícitos; a exceção da geradora inativa sem fim segue o aval (b), com C6. Não demonstrado novo conflito semântico nos carimbos de regeneração. (`app/escala/alimentacao.py:189`, `:205`, `:282`; `app/ponto/services/vigia_cadastro_futuro.py:69`; `app/escala/models.py:1714`) |

**8. Leis dos sítios**

- **L-007: violação demonstrada na garantia de trilha D1.** O ato crítico pode confirmar sem antes/depois persistidos. (`CLAUDE.md:32`; `app/ponto/portas/celula.py:1149`; `app/core/observ.py:139`)
- **Proibição expressa do aval de ampliar `d <= hoje`: violada pela casca do cron**, conforme o caso de vínculo inativo descrito no item 1. (`app/ponto/management/commands/gerar_celulas.py:323`; `app/ponto/tests/test_cadastro_futuro_c4.py:165`)
- **L-020:** denúncia da reescrita futura está presente. (`app/ponto/portas/celula.py:257`, `:258`, `:259`, `:302`)
- **L-002:** vigia e suplemento consultam as autoridades existentes. (`app/ponto/services/vigia_cadastro_futuro.py:58`, `:79`; `app/ponto/services/cadastro_futuro.py:36`)
- **L-052:** guardas de lavra e célula humana permanecem na porta; abertura continua exigindo o ato `apesar_da_lavra`. (`app/ponto/portas/celula.py:213`, `:248`, `:279`, `:292`)
- **L-090:** janela vem do juiz de competência; mês civil da foto de folga serve à fase mensal, explicitamente. (`app/ponto/management/commands/gerar_celulas.py:290`; `app/escala/folgas.py:629`)
- Violação nova concreta de **L-006, L-009, L-023, L-050, L-071, L-086 ou L-091** nesta leitura: **[não verificado]**.

_4376 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
