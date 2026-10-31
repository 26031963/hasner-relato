# CODEX-ESTADO

> Ultima resposta processada: **31/10/2026 13:43** · publicado em **31/10/2026 13:43** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 31/10 13:43 · sessao 01543f2c5b50 · - · - · origem=Ronald (tmux)

Auditoria de mutantes em só leitura concluída. **Nenhum dos 129 mutantes tem âncora ausente ou repetida; todos os 129 códigos mutados compilam.** Extraí as chamadas via Bash substituindo `_mutante` apenas por `printf`, contei as ocorrências e usei `compile()` em memória; o critério exigido pelo próprio script é uma ocorrência exata (`bin/tests/test_disjuntor.sh:934-939`). A lista dos 129 está em `bin/tests/test_disjuntor.sh:945-1128`.

**Nenhum sobrevivente identificado.** Executei em memória os 27 mutantes de `b3_topo` e os cinco de `b3_atomico`, com controle passando. Os demais 97 têm caso que os derruba por inspeção estática; a execução integrada deles está **[nao verificado]**. Não executei a suíte shell porque ela cria, altera e apaga arquivos (`bin/tests/test_disjuntor.sh:39-56`, `bin/tests/test_disjuntor.sh:930-939`).

Mapa das verificações em memória:

| Mutante | Assert que caiu |
|---|---|
| B3-sem-lei | 17 — `bin/tests/test_disjuntor.sh:698` |
| B3-sem-lei-juiz | 2 e outros — `bin/tests/test_disjuntor.sh:684` |
| B3-sem-lei-escritor-topo | 31 — `bin/tests/test_disjuntor.sh:708` |
| B3-sem-lei-celula | 19/32 — `bin/tests/test_disjuntor.sh:700`, `bin/tests/test_disjuntor.sh:709` |
| B3-sem-lei-lampada | 20/33 — `bin/tests/test_disjuntor.sh:701`, `bin/tests/test_disjuntor.sh:710` |
| B3-sem-arquivo-linha | 18/62 — `bin/tests/test_disjuntor.sh:699`, `bin/tests/test_disjuntor.sh:721` |
| B3-acento-conta | 19 — `bin/tests/test_disjuntor.sh:700` |
| B3-bobo-qualquer / B3-bobo-pontuacao | 21 — `bin/tests/test_disjuntor.sh:704` |
| B3-sitio-nao-confere | 34/35/36/38/39/60/61 — `bin/tests/test_disjuntor.sh:712-720` |
| B3-sitio-so-o-1o | 61 — `bin/tests/test_disjuntor.sh:720` |
| B3-linha-zero | 36 — `bin/tests/test_disjuntor.sh:714` |
| B3-linha-alem-do-fim | 38/39 — `bin/tests/test_disjuntor.sh:716-718` |
| B3-ultima-sem-quebra | 39 — `bin/tests/test_disjuntor.sh:717-718` |
| B3-fora-da-raiz | 60 — `bin/tests/test_disjuntor.sh:719` |
| B3-desenho-no-meio | 24 — `bin/tests/test_disjuntor.sh:722` |
| B3-id-substring | 3 e outros — `bin/tests/test_disjuntor.sh:685` |
| B3-obra-fora-do-cabecalho | 25/27 — `bin/tests/test_disjuntor.sh:723`, `bin/tests/test_disjuntor.sh:725` |
| B3-pergunta-sem-texto | 26 — `bin/tests/test_disjuntor.sh:724` |
| B3-topo-sem-lookbehind | 7 e controles — `bin/tests/test_disjuntor.sh:689`, `bin/tests/test_disjuntor.sh:728-732` |
| B3-topo-sem-sublinhado | 13 — `bin/tests/test_disjuntor.sh:693` |
| B3-topo-sem-re-I | 14 e controles — `bin/tests/test_disjuntor.sh:694`, `bin/tests/test_disjuntor.sh:728-732` |
| B3-topo-sem-validacao | controles 42/43/49/52/59 — `bin/tests/test_disjuntor.sh:728-732` |
| B3-identidade-sem-hifen | 15 — `bin/tests/test_disjuntor.sh:695` |
| B3-identidade-sem-upper | 16 — `bin/tests/test_disjuntor.sh:696` |
| B3-topo-parcial-vira-none | 10/11 — `bin/tests/test_disjuntor.sh:747-748` |
| B3-topo-sem-cauda | 2 e outros — `bin/tests/test_disjuntor.sh:684` |
| B3-sem-mtime | c5 — `bin/tests/test_disjuntor.sh:879-882` |
| B3-mtime-estrito | c4 — `bin/tests/test_disjuntor.sh:875-878` |
| B3-topo-fora-da-trava / B3-duas-travas | c1/c2 — `bin/tests/test_disjuntor.sh:853-867` |
| B3-observacao-sem-restat | c6 — `bin/tests/test_disjuntor.sh:884-901` |

Para consolidar os outros B3 por inspeção:

- `B3-limiar-3`, `B3-sem-lei-escritor`, `B3-foto-ilegivel-vira-vazia`, `B3-sem-foto-tardia`, `B3-foto-ilegivel-calada`, `B3-anuncia-antes-de-gravar`, `B3-gravou-mente`, `B3-libera-sem-gravar`, `B3-migra-sobrescreve`: casos próprios em `b3_limiar` (`bin/tests/test_disjuntor.sh:763-800`).
- `B3-stop-cala`: presença do recado no Stop (`bin/tests/test_disjuntor.sh:344-348`).
- `P1-b3-so-no-stop`: reprovação sem Stop anterior (`bin/tests/test_disjuntor.sh:401-403`).
- `P2-rearme-ignorado`: verificador passa após rearme e Stop (`bin/tests/test_disjuntor.sh:398-400`).
- `B3-liberado-volta`: reprovações já consumidas permanecem liberadas (`bin/tests/test_disjuntor.sh:379-381`).
- `B3-sitio-nao-confere-integrado`: arquivo inexistente mantém bloqueio (`bin/tests/test_disjuntor.sh:364`).
- `B3-so-a-1a-candidata`: primeira candidata consumida não esconde ERRO-BOBO seguinte (`bin/tests/test_disjuntor.sh:387-389`).
- `B3-topo-so-linha-1`: DESENHO na segunda linha útil libera (`bin/tests/test_disjuntor.sh:373-381`).
- `B3-sem-consumo`: reescrever linha consumida mantém bloqueio (`bin/tests/test_disjuntor.sh:382-385`).
- `B3-sem-topo-no-disparo`: linha anterior ao disparo seguida de touch mantém bloqueio (`bin/tests/test_disjuntor.sh:390-393`).
- `B3-sem-migracao`: reprovações antigas arquivadas não disparam (`bin/tests/test_disjuntor.sh:328-335`).
- `B3-topo-branco-encerra`: branco entre as duas linhas úteis (`bin/tests/test_disjuntor.sh:373-379`).
- `B3-topo-bloco-inteiro`: DESENHO na terceira linha útil não libera (`bin/tests/test_disjuntor.sh:367`).
- `B-sem-contador`: final sem obra aparece no contador (`bin/tests/test_disjuntor.sh:427-431`).
- Demais reconhecedores de final/obra/markdown/cauda: exemplos positivos e negativos (`bin/tests/test_disjuntor.sh:409-426`).

Limite material: `b3_atomico` mede a entrada no callback de `_com_estado`, não observa diretamente `fcntl.flock`; portanto comprova uma chamada e leitura dentro do callback, enquanto a aquisição efetiva da trava depende da implementação de `_com_estado` (`bin/tests/test_disjuntor.sh:829-850`, `bin/disjuntor.py:230-281`).

## 31/10 13:42 · sessao 42bd65f14083 · - · - · origem=Ronald (tmux)

Achados das perguntas 1–3:

1. A transição do estado está unificada: `_fn` lê o topo, acumula, dispara e libera sob uma chamada de `_com_estado`; os dois hooks usam `decide_b3`. A varredura dos transcripts ocorre antes da trava, sem modificar o estado. (`bin/disjuntor.py:690`, `bin/disjuntor.py:693`, `bin/disjuntor.py:744`, `bin/disjuntor.py:1136`, `bin/hook_stop_fila1.py:1044`)

2. **Há dois caminhos confirmados em memória que liberam linha anterior ao disparo:**

   - **Leitura parcialmente ilegível:** `_bloco_do_topo` devolve a primeira linha já lida quando ocorre `OSError` antes da segunda; `_topo_consistente` aceita essa leitura como completa. Se a segunda linha já continha `ERRO-BOBO`, a fotografia não a guarda. Restaurar a leitura e executar `touch` permite que ela libere. Reproduzido com iterador que falha antes da segunda linha útil: primeira decisão `{'OBRA-X': 1}`, segunda `{}`, eventos `DISJUNTOR`, `LIBERA`. (`bin/hook_stop_fila1.py:437`, `bin/disjuntor.py:635`, `bin/disjuntor.py:695`, `bin/disjuntor.py:731`, `bin/disjuntor.py:733`)
   - **Janela fotografia → disparo:** depois que `_topo_consistente` retorna, um escritor pode colocar a linha no RELATO antes da atribuição `disparado=True`. Ela não estará na fotografia e seu mtime será posterior ao `agora` capturado na borda. A próxima chamada libera. Reproduzido inserindo a linha no início de `_migra_limiar`, com a obra ainda não disparada. (`bin/disjuntor.py:689`, `bin/disjuntor.py:694`, `bin/disjuntor.py:696`, `bin/disjuntor.py:712`, `bin/disjuntor.py:733`)

   Com leitura completa e sem concorrência, `touch`, reescrita preservando a linha e mtime igual não vencem a fotografia: o SHA antigo é recusado antes da comparação temporal. Ilegibilidade total também recebe fotografia tardia sem liberação naquela chamada. (`bin/disjuntor.py:724`, `bin/disjuntor.py:726`, `bin/disjuntor.py:731`)

3. Não encontrei evento formal `DISJUNTOR`/`LIBERA` B3 anunciado sem gravação concluída: `gravou=True` só sucede `os.replace`; o anúncio depende desse resultado, e liberação não persistida volta ao conjunto bloqueado. (`bin/disjuntor.py:265`, `bin/disjuntor.py:274`, `bin/disjuntor.py:745`, `bin/disjuntor.py:751`)

   Uma negativa `NEGOU DISJUNTOR B3 ...` pode aparecer mesmo sem persistência, por decisão explícita de manter a negativa daquela chamada; o contador distingue `NEGOU` de `DISJUNTOR`. A publicação dos eventos ocorre depois de soltar a trava, permitindo ordem histórica invertida sob concorrência, embora ambos tenham sido gravados em algum instante. (`bin/disjuntor.py:277`, `bin/disjuntor.py:747`, `bin/disjuntor.py:749`, `bin/disjuntor.py:1136`, `bin/disjuntor.py:1168`, `bin/disjuntor.py:1226`)

## 31/10 13:42 · sessao eda389dede8f · CLAUDE-MD-RACIONAL · verificador · origem=Code

**1) Nenhuma das dez linhas removidas ficou sem correspondente no 7b atual**, considerando o bloco completo. (`CLAUDE.md:481`, `CLAUDE.md:483`, `CLAUDE.md:518`, `CLAUDE.md:519`, `CLAUDE.md:567`, `CLAUDE.md:568`, `CLAUDE.md:570`, `CLAUDE.md:575`, `CLAUDE.md:576`)

| Linha removida, identificada pelo conteúdo | Onde ficou |
|---|---|
| Cinco âncoras RAIAS/POUSO | Todas mantidas, apenas concatenadas: `CLAUDE.md:481`. | [nao verificado]
| Contador de raias (5), selo no pre-push, fila 2 depois de CELULA-TURNO-FECHA **e** O145, conduta provisória, HANDOFF HEAD/origin/status | Preservados em `CLAUDE.md:483`; continuação do HANDOFF em `CLAUDE.md:484`. | [nao verificado]
| Quatro primeiras âncoras CODEX | Todas mantidas, concatenadas: `CLAUDE.md:518`. |
| Seis demais âncoras CODEX | Todas mantidas, concatenadas: `CLAUDE.md:519`. |
| S3 EXERCÍCIO, não CAMINHO; zero com os dois verdes | Preservado; “dois” virou “2”: `CLAUDE.md:567`. |
| Corte 23/09: sem fechamento, mostrar número do motor rotulado; morre “quem CHAMA o motor” | Corte em `CLAUDE.md:570`; objeto “numero de motor” e rótulo em `CLAUDE.md:568`. |
| L-095 não cede; histórico lap-18 | Ambos em `CLAUDE.md:570`. |
| L-009/L-082: item 1 retira `!` somente na competência corrente | Redação equivalente pela negativa: “fora do item 1 (competencia CORRENTE) o `!` segue”: `CLAUDE.md:575`. |
| Paga, vínculo/escala retroativo, atalho; sem critério escrito vale L-009; introdução do DIFF | Casos em `CLAUDE.md:575`; cláusula residual e DIFF em `CLAUDE.md:576`. |
| DIFF ANTECIPADO; backup/renome do TXT pelo admin | Ambos em `CLAUDE.md:576`. |

**2) Os resultados foram transcritos, mas a referência “(2)” perdeu precisão.** NOTURNO conserva exatamente `(a) 1,5 / (b) 2,3 / (b) parcial 4,6`; no aval original, “item (2)” significa classificação pelo Codex. No bloco atual, o item 2 significa DESPACHO; a classificação está no item 6. Portanto, não certifico equivalência inequívoca da linha inteira sem explicitar o destino dessa referência. (`app/docs/PROMPTS.md:1671`; `CLAUDE.md:559`; `CLAUDE.md:528`; `CLAUDE.md:553`; `app/docs/PISTA.md:124`)

Há também uma ressalva de vigência: o aval de **10:4x substitui expressamente o de 10:3x**, passando NOTURNO 1,5 para **(e)**, com falha do Codex zero. A linha adicionada reproduz o resultado histórico de 10:3x, mas não informa essa substituição. (`app/docs/PROMPTS.md:1672`; `CLAUDE.md:559`)

**3) O corte do PLACAR está preservado em relação ao texto removido do anexo.** Na leitura conjunta, continuam: sem fechamento → MOSTRAR número do motor com `'sem apuracao ainda'`; eliminação de “quem CHAMA o motor”; L-095 intacta. A frase isolada ficou menos explícita, mas seu referente permanece no bloco. (`CLAUDE.md:568`; `CLAUDE.md:570`)

O cotejo direto com **`41b4d89d:CLAUDE.md`**, além do texto apresentado no anexo, é **[nao verificado]**.

**4) A frase conserva a cláusula residual de AVAL-DE-CRITERIO; sozinha, não expressa a regra inteira.** O registro L-082 também determina “sem critério escrito → L-009”. As condições restantes aparecem no item 2 de DINHEIRO: `!` de critério, somente campos-alvo, demais campos de todos os colaboradores zerados contra o GRAVADO, total na faixa aprovada e violação → PAREI com tabela. (`CLAUDE.md:576`; `app/docs/LEIS.md:109`; `CLAUDE.md:586–588`)

No conjunto atual, essas condições estão sob competência exportada; a não exportada tem a preaprovação condicionada do item 1. A equivalência integral de redação e escopo com **41b4d89d** permanece **[nao verificado]**. (`CLAUDE.md:577–588`)

**5) Bytes medidos com `wc -c`:**

| Recorte | Bytes |
|---|---:|
| 7b: título incluído, até imediatamente antes de `## 8.`, incluindo a linha vazia final | **20.997** (`CLAUDE.md:436–627`) |
| CLAUDE.md inteiro | **57.798** (`CLAUDE.md:1–631`) |

## 31/10 13:42 · sessao 342dfc199d5b · - · - · origem=Ronald (tmux)

Q4 tem falhas concretas:

- **Motivo só pontuação passa:** `ERRO-BOBO OBRA-X: _` e `...: ___` são aceitos, pois `\w` inclui underscore. Reproduzi em memória, sem escrita. `...` e `!!!` são recusados. Fonte: `bin/disjuntor.py:615-617`; testes cobrem apenas `...` como pontuação em `bin/tests/test_disjuntor.sh:704`.
- **Link interno passa:** `_sitio_existe` resolve `realpath` antes de abrir; se `link.py` aponta para um arquivo dentro da raiz, `_abre` recebe o destino e aceita. O `O_NOFOLLOW` deixa de detectar o link original. Confirmei por simulação de `realpath` em memória. Fontes: `bin/disjuntor.py:575-581`, `bin/orcamento_contexto.py:74`.
- **Referência fora da raiz pode ser ignorada:** a linha `DESENHO OBRA-X: lei=juiz unico pergunta=quem? bin/disjuntor.py:1 /etc/passwd:1` é aceita; a regex exige extensão, portanto só valida `bin/disjuntor.py:1`. Também passa referência válida seguida de `bin/disjuntor.py:-9`, cuja segunda referência não casa. Fontes: `bin/disjuntor.py:77`, `bin/disjuntor.py:608-612`. Contradiz “todo arquivo:linha” em `bin/disjuntor.py:596`; teste correspondente só usa referências com extensão em `bin/tests/test_disjuntor.sh:720`.
- **Número malformado passa:** `bin/disjuntor.py:1.5` e `bin/disjuntor.py:1xyz` são aceitos, validando apenas o prefixo `:1`. Reproduzido em memória. Fontes: `bin/disjuntor.py:77`, `bin/disjuntor.py:609-610`.
- Para referências reconhecidas integralmente, absoluto, resolução para fora da raiz, linha zero e além do fim são recusados; última linha sem quebra é contada. Fontes: `bin/disjuntor.py:573-588`. Casos existentes: `bin/tests/test_disjuntor.sh:714-719`.

Q6:

- AST do pai contra o novo identifica seis funções removidas: `linha_libera_b3`, `libera_b3`, `_topo_shas`, `varrer_b3`, `_topo_e_mtime`, `b3_disparados`; nenhuma removida de `hook_stop_fila1.py`. Evidência de remoção via diff/AST, sem arquivo persistido: `[nao verificado]` para citar diretamente uma linha inexistente no novo.
- `rg -n -uuu` em `bin/` e `app/` não encontrou chamadas residuais aos seis nomes; o único resultado foi documentação antiga mencionando `linha_libera_b3` em `app/docs/RELATO.md:11`.
- Os dois chamadores produtivos agora usam `decide_b3`: `bin/disjuntor.py:1136`, `bin/hook_stop_fila1.py:1044`.

## 31/10 13:42 · sessao eabc6a4aa035 · - · - · origem=Ronald (tmux)

Questão 2: a lista de resultados foi copiada de `app/docs/PROMPTS.md:1671` para `CLAUDE.md:559`: NOTURNO `(a) 1,5 / (b) 2,3 / (b) parcial 4,6`, V-FASE refeita pelo `(2)`. Entretanto, a referência perdeu precisão: no aval original `(2)` é classificação pelo Codex; no bloco consolidado o item 2 é DESPACHO (`CLAUDE.md:528`), e a classificação está no item 6 (`CLAUDE.md:553`). A PISTA ainda tem o item `(2) CLASSIFICACAO` original (`app/docs/PISTA.md:124`). Portanto: conteúdo classificatório transcrito, referência cruzada sem equivalência inequívoca (`CLAUDE.md:559`; `app/docs/PROMPTS.md:1671`; `app/docs/PISTA.md:124`).

Há ainda conflito com o aval seguinte: 10:4x substitui expressamente o de 10:3x e classifica NOTURNO 1,5 como `(e)`, falha do Codex zero (`app/docs/PROMPTS.md:1672`). A própria consolidação distingue `(a)` tema presente de `(e)` não perguntado (`CLAUDE.md:556`; `CLAUDE.md:557`). Assim, a linha 559 pode registrar o estado histórico de 10:3x, mas não reproduz a classificação posterior vigente; sem explicitar a substituição, pode reintroduzir a atribuição antiga (`CLAUDE.md:559`; `app/docs/PROMPTS.md:1672`). A PISTA confirma que sua versão é a de 10:4x, que substitui 10:3x (`app/docs/PISTA.md:120`).

Questão 4: a frase isolada não contém toda a AVAL-DE-CRITERIO; é a cláusula residual “sem critério escrito → L-009” (`CLAUDE.md:576`; `app/docs/LEIS.md:109`). As condições materiais continuam no item 2 de DINHEIRO: `!` de critério, somente campos-alvo, zero nos demais campos/colaboradores, medição contra o GRAVADO, total na faixa, violação → PAREI com tabela (`CLAUDE.md:586`; `CLAUDE.md:587`; `CLAUDE.md:588`). A L-082 registra exatamente esses critérios e o residual sem critério (`app/docs/LEIS.md:109`).

O conjunto atual mantém essa estrutura para competência exportada (`CLAUDE.md:582`; `CLAUDE.md:586`), com exceção de competência não exportada no item 1 (`CLAUDE.md:577`) e referência explícita aos casos em que o `!` segue (`CLAUDE.md:575`). Comparação literal e de escopo com `41b4d89d:CLAUDE.md`: [nao verificado], pois esse conteúdo histórico não foi fornecido nem encontrado como arquivo nesta cópia.

## 31/10 13:42 · sessao d727f053cc5f · - · - · origem=Ronald (tmux)

As 10 linhas removidas têm correspondentes no 7b novo; não identifiquei regra, cláusula, nome, script ou condição desse delta sem lugar. (CLAUDE.md:481; CLAUDE.md:483; CLAUDE.md:518; CLAUDE.md:519; CLAUDE.md:567; CLAUDE.md:568; CLAUDE.md:570; CLAUDE.md:575; CLAUDE.md:576)

| Linha removida, pelo conteúdo | Correspondente atual |
|---|---|
| Cinco âncoras RAIAS/POUSO | As cinco permanecem, apenas sem espaços entre parênteses. CLAUDE.md:481 | [nao verificado]
| Contador de raias, selo no pre-push, fila 2 depois da CELULA-TURNO-FECHA **e** O145, conduta provisória, começo do HANDOFF | Todos permanecem; “DEPOIS” virou “apos”, “e selo” virou “+ selo”. CLAUDE.md:483 | [nao verificado]
| Quatro primeiras âncoras CODEX | Permanecem concatenadas. CLAUDE.md:518 |
| Seis demais âncoras CODEX | Permanecem concatenadas. CLAUDE.md:519 |
| S3 exercício, não caminho; zero com os dois verdes | Preservado; “dois” virou “2”. CLAUDE.md:567 |
| Corte 23/09 19:2x: sem fechamento mostrar número do motor rotulado; morre “quem CHAMA o motor” | Preservado como “sem fechamento, MOSTRAR rotulado”, com referente “numero de motor” na condição (a). CLAUDE.md:568; CLAUDE.md:570 |
| L-095 não cede; histórico lap-18 | Ambos preservados na linha consolidada. CLAUDE.md:570 |
| L-009/L-082: exceção do item 1 apenas para competência corrente; demais situações conservam `!` | A redação inversa “fora do item 1 (competencia CORRENTE) o `!` segue” conserva a delimitação. CLAUDE.md:575 |
| Paga, vínculo/escala retroativo, atalho; sem critério escrito vale L-009; introdução do DIFF | Situações em CLAUDE.md:575; regra subsidiária e DIFF em CLAUDE.md:576 |
| DIFF antecipado; backup/renome do TXT por admin | Ambos preservados. CLAUDE.md:576 |

O corte do PLACAR conserva o efeito **na leitura do bloco completo**: “MOSTRAR rotulado” recupera “numero de motor” da condição (a), cujo rótulo continua literalmente `'sem apuracao ainda'`; mantém a condição “sem fechamento”, a morte da pergunta “quem CHAMA o motor” e a ressalva da L-095. (CLAUDE.md:568; CLAUDE.md:570)

A frase isolada ficou menos explícita quanto ao objeto mostrado; isso não elimina o referente no bloco. (CLAUDE.md:568; CLAUDE.md:570)

A PISTA estabelece que os blocos consolidados da 7b prevalecem sobre os textos anteriores nela guardados. (app/docs/PISTA.md:10; app/docs/PISTA.md:11; app/docs/PISTA.md:12)

## 31/10 13:39 · sessao 82f392df9084 · F1C4-3-EMISSORES · verificador · origem=Code

**O texto gravado nunca passa de 500 caracteres; o corpo só fica inteiro quando tem até 500. O corte final `texto[:500]` continua existindo.** (`app/pautas/services.py:182`)

**1. Escritores e leitores**

A escrita física de `Pauta.texto` está em `escrever`; `responder` cria outra Pauta pela mesma porta, herdando a âncora. (`app/pautas/services.py:177–182`; `app/pautas/services.py:189–197`)

Os sítios de produção que alimentam essa escrita são:

| Origem | Chamada |
|---|---|
| View de escrita e resposta | `app/pautas/views.py:76,112` |
| Resposta pelo CLI de suporte | `app/pautas/management/commands/suporte_hasner.py:133` |
| Pauta semanal | `app/pautas/semanal.py:276` |
| Dia em aberto: cabeça e filhas | `app/pautas/management/commands/pauta_dia_em_aberto.py:127,139,142` |
| Pré-fechamento | `app/folha/management/commands/pre_fechamento.py:110` |
| Aparelhos compartilhados | `app/colaboradores/management/commands/pauta_aparelhos_compartilhados.py:107` |
| Cobertura | `app/ponto/services/cobertura.py:333` |
| Documento vencido e férias recusadas | `app/ponto/services/ausencia.py:1039,1229` |
| Discordantes | `app/ponto/services/pautas_dos_discordantes.py:177` |
| Afastado que bateu | `app/ponto/services/afastado_avisa.py:74` |
| Deslogados | `app/ponto/services/pautas_dos_deslogados.py:77` |
| Autorização de HE | `app/ponto/services/autorizacao_he_periodo.py:244` |
| Esmeril | `app/ponto/services/pautas_do_esmeril.py:76` |
| Férias | `app/ferias/management/commands/pautas_de_ferias.py:167` |
| Cron quebrado | `app/core/services/crons_quebrados.py:71` |
| Alarme da esteira | `app/core/management/commands/alarme_esteira.py:54` |
| Alarme da árvore | `app/core/management/commands/alarme_arvore.py:50` |
| Limbo exportado | `app/chamados/services/fabrica_da_celula.py:185` |
| Passivo validado: cabeça, filhas e quem saiu | `app/chamados/services/passivo_validado.py:229,233,266` |
| Resposta tardia | `app/chamados/services/resposta_tardia.py:62` |
| Competência trancada | `app/chamados/services/competencia_trancada.py:224` |
| Órfãs enterradas | `app/chamados/management/commands/reabrir_orfas_enterradas.py:197` |

Os leitores que interpretam conteúdo são:

| Leitor | O que procura no texto |
|---|---|
| Limbo exportado | Prefixo `LIMBO EM COMPETENCIA EXPORTADA` e, depois, substring exata `[motivo:<m>]` em todo o texto. (`app/chamados/services/fabrica_da_celula.py:180–182`) |
| Semanal: existência da semana | Prefixo `<marcador> <semana ISO>`. (`app/pautas/semanal.py:195–199`) |
| Semanal: fechamento da família | Prefixo `<marcador>`. (`app/pautas/semanal.py:226–230`) |
| Semanal: números iguais | Substring `n=<contadores separados por vírgula>` na pauta anterior. (`app/pautas/semanal.py:245–248`) |
| Dia em aberto | Prefixo `[DIA EM ABERTO]`. (`app/pautas/management/commands/pauta_dia_em_aberto.py:16,108`) |
| Férias | Prefixo de `MARCADORES[codigo]`: PA vencido, vencendo ou cadastro. (`app/ferias/management/commands/pautas_de_ferias.py:30–33,149–156`) |
| Pré-fechamento | Prefixo `[PRE-FECHAMENTO]`; para deduplicar o dia, prefixo incluindo a data de hoje. (`app/folha/management/commands/pre_fechamento.py:29,91–98`) |
| Competência trancada | Prefixo `[TRANCA]`. (`app/chamados/services/competencia_trancada.py:211`) |
| Passivo validado | Prefixo `[PASSIVO-VALIDADO] col<id> AAAA-MM`. (`app/chamados/services/passivo_validado.py:44,159–164`) |
| Passivo validado: quem saiu | Prefixo `[PASSIVO-VALIDADO-FORA] col<id> AAAA-MM`. (`app/chamados/services/passivo_validado.py:48,254–256`) |
| Órfãs enterradas | Prefixo `[ORFA-ENTERRADA]`. (`app/chamados/management/commands/reabrir_orfas_enterradas.py:32,190–191`) |
| Alarme da árvore | Prefixo `ARVORE VERMELHA`. (`app/core/management/commands/alarme_arvore.py:11–16`) |
| Alarme da esteira | Prefixo `ESTEIRA <chave>:`. (`app/core/management/commands/alarme_esteira.py:10–15`) |
| Cron quebrado | Prefixo `[CRON QUEBRADO] <nome> (empresa <empresa ou ->)`. (`app/core/services/crons_quebrados.py:15,51–54`) |
| HE em exportada | Prefixo `[HE-AUTORIZADA-EM-EXPORTADA]`. (`app/ponto/services/autorizacao_he_periodo.py:124,240–242`) |
| Deslogados | Prefixo `[APP-401]`; a identidade restante vem da âncora. (`app/ponto/services/pautas_dos_deslogados.py:26,65–68`) |
| Afastado que bateu | Prefixo `TEXTO[:40]`. (`app/ponto/services/afastado_avisa.py:62–65`) |
| Documento vencido | Prefixo `[A4/SEM-DOCUMENTO]`. (`app/ponto/services/ausencia.py:1022–1025`) |
| Férias recusadas | Prefixo `[A1-FERIAS/AG#<id>]`. (`app/ponto/services/ausencia.py:1212–1215`) |
| Cobertura a confirmar | Prefixo `[COBERTURA-A-CONFIRMAR]`. (`app/ponto/services/cobertura.py:207,241–243`) |
| Esmeril | Prefixo `[ESMERIL ` e extração da ação até o primeiro `]`. (`app/ponto/services/pautas_do_esmeril.py:63–65`) |
| Discordantes | Prefixo `[DISCORDANTE]`; chave formada pelo trecho anterior ao primeiro `:`. (`app/ponto/services/pautas_dos_discordantes.py:37,96–100,165–167`) |

Os demais leitores apresentam ou transportam o conteúdo:

| Leitor | Tratamento |
|---|---|
| Endpoint das abas | Serializa integralmente cabeça e respostas. (`app/pautas/views.py:40,47`) |
| API da mensageria | Serializa integralmente o texto. (`app/api/views_mensageria.py:1324–1328`) |
| Painel do copiloto | Mostra cabeça e respostas por `textContent`. (`app/templates/core/_copiloto.html:405,424`) |
| Detalhe do colaborador | Mostra com `truncatechars:180`; aqui o comprimento afeta a apresentação. (`app/templates/colaboradores/detalhe.html:119`) |
| CLI de suporte | Imprime integralmente cabeça e respostas. (`app/pautas/management/commands/suporte_hasner.py:111,113`) |

A procura por **`[DIA `** ocorre na entrada de `escrever`, para decidir se acrescenta contexto; não é interpretação de uma Pauta persistida. O contexto produzido contém data, pessoa, matrícula, marcos, batidas, acusação e URL. (`app/pautas/services.py:163–169,415–425`)

**Há mudança de resultado na guarda do limbo:** preservar marcador e tags faz a segunda passagem reconhecer motivos que o corte antigo poderia apagar. O contexto completo não participa dessa decisão. A guarda da cobertura também depende de preservar seu prefixo. (`app/chamados/services/fabrica_da_celula.py:180–183`; `app/ponto/services/cobertura.py:241–243`)

As telas e saídas passam a receber mais corpo e menos contexto. Nenhum dos leitores de negócio acima extrai marcos, batidas ou URL do contexto; o limite de 180 no detalhe continua podendo esconder parte do texto. (`app/pautas/views.py:40,47`; `app/templates/core/_copiloto.html:405,424`; `app/templates/colaboradores/detalhe.html:119`)

**2. Resultado exato de `escrever`**

Considere `B = (texto or '').strip()` e **T = comprimento de B**. Se T significar comprimento antes do `strip`, T e C sozinhos não determinam o resultado; uma entrada só de espaços é recusada. (`app/pautas/services.py:163–165`)

Para âncora `dia`, sem `[DIA ` em B, e contexto K de comprimento C:

| Condição | Texto gravado |
|---|---|
| `1 ≤ T ≤ 495` e `C > 0` | `B + ' -- ' + K[:496-T]` |
| `C = 0`, qualquer T | `B[:500]` |
| `496 ≤ T ≤ 500`, qualquer C | B inteiro, sem contexto |
| `T > 500`, qualquer C | Primeiros 500 caracteres de B, sem contexto |

Esses casos seguem de `_sobra = 500-T-4`, da condição `_sobra > 0` e do corte final. (`app/pautas/services.py:174–182`)

Quando há contexto anexado, o comprimento é:

`T + 4 + min(C, 496-T) ≤ 500`.

Quando não há, é `min(T,500)`. Portanto, **não ultrapassa 500**; em particular, T=495 permite um único caractere de contexto, T=496 já não permite nenhum e T>500 corta o corpo. (`app/pautas/services.py:174–182`)

Para outra âncora, ou se B já contém `[DIA `, o bloco de contexto é pulado e grava-se `B[:500]`. (`app/pautas/services.py:168,182`)

**3. Quatro testes novos e suas mutações** [nao verificado]

Uma mutação local, na linha 176, derruba os quatro: trocar somente o primeiro `texto` da tupla por **`texto[:500 - len(_ctx) - 4]`**, reintroduzindo o corte antigo do corpo. (`app/pautas/services.py:176`)

| Teste novo | Falha prevista com essa mutação | [nao verificado]
|---|---|
| `test_RED_contexto_460_nao_come_a_tag` | Sobram 36 caracteres do corpo: marcador sobrevive, tag completa não. A segunda passada escreve outra pauta; `(p,)` encontra duas. (`app/chamados/tests/test_f1c4_3_emissores.py:274–285`; `app/chamados/services/fabrica_da_celula.py:180–190`) |
| `test_RED_contexto_480_nao_come_o_marcador` | Sobram 16 caracteres: o marcador completo desaparece. `_pautas()` não encontra nenhuma e `(p,)` falha. (`app/chamados/tests/test_f1c4_3_emissores.py:189–193,278,287–288`; `app/pautas/services.py:176`) |
| `test_RED_contexto_700_indice_negativo_come_o_corpo` | O corte vira `texto[:-204]`, esvaziando esse corpo; perde-se o marcador e falha o desempacotamento. (`app/chamados/tests/test_f1c4_3_emissores.py:278,290–293`; `app/chamados/services/fabrica_da_celula.py:189–190`) |
| `test_porta_escrever_corta_o_contexto_e_nunca_o_corpo` | O corpo tem 78 caracteres: n=100 passa; n=460 deixa só 36 e falha o `startswith(corpo + ' -- [DIA x]')`. (`app/chamados/tests/test_f1c4_3_emissores.py:299–307`; `app/pautas/services.py:176`) |

Esses são vereditos derivados do código; execução da suíte Django com as mutações: **[não verificado]**. [nao verificado]

No código atual, os quatro entram no ramo de anexação: usam contexto não vazio e corpo menor que 496. Entretanto, **os três testes de integração passam se a condição da linha 175 virar `if False:`**: eles exigem marcador, tag, tamanho e unicidade, mas não exigem contexto. O teste direto detecta esse desvio pela asserção de prefixo. (`app/pautas/services.py:174–176`; `app/chamados/tests/test_f1c4_3_emissores.py:274–282,299–306`)

Há outras lacunas:

- Os quatro aceitam `_ctx[:7]` no lugar de `_ctx[:_sobra]`: basta anexar `[DIA x]`; nenhum exige o contexto disponível inteiro. (`app/pautas/services.py:176`; `app/chamados/tests/test_f1c4_3_emissores.py:279–280,306–307`)
- Nenhum cobre C=0, T≥496, corpo contendo `[DIA ` ou T>500; retirar o corte final `[:500]` não derruba esses quatro. (`app/chamados/tests/test_f1c4_3_emissores.py:274,299–301`; `app/pautas/services.py:168,174–182`)
- O nome “nunca o corpo” excede o contrato demonstrado: o próprio service corta corpos acima de 500. (`app/chamados/tests/test_f1c4_3_emissores.py:295`; `app/pautas/services.py:182`)

**Outros defeitos no delta**

- **A exceção do limbo exportado pode ser contornada pelo lote.** O evento envia à Pauta e retorna sem pergunta, mas o caminho FIO MUDO seleciona chamados vivos sem filtrar `trabalha` ou competência, consulta os motivos da ata e chama `perguntar` diretamente. Para limbo exportado com chamado vivo, motivo faltante e sem pergunta existente, a Pauta não bloqueia esse caminho. O cron está configurado com `--apply`. Ocorrência desses candidatos em produção: **[não verificado]**. (`app/chamados/services/fabrica_da_celula.py:153–157`; `app/chamados/management/commands/disparar_perguntas_competencia.py:155–168,178–224,277–301`; `app/config/crons.py:1464`)
- **O fallback anunciado para contexto ausente não funciona pelo clique do painel.** O comentário diz que o DP reabre pela âncora, mas `ROTA` só contempla `colab`, `chamado` e `posto`; `dia` termina no aviso de ausência de tela. O URL fica no fim do contexto, justamente a parte agora cortável. A ausência de rota é preexistente, mas a nova estratégia depende dela. (`app/pautas/services.py:173–176,423–425`; `app/templates/core/_copiloto.html:564–574`)
- **A idempotência nova não protege concorrência localmente:** consulta e criação são separadas, sem trava nessa função, e o modelo não declara unicidade para marcador/âncora. Duas chamadas podem observar ausência antes de escrever. Serialização externa ou duplicação ocorrida: **[não verificado]**. (`app/chamados/services/fabrica_da_celula.py:180–190`; `app/pautas/models.py:123–133`)
- O comentário junto ao emissor continua dizendo que `escrever` corta o corpo para caber contexto, contradizendo a implementação nova. (`app/chamados/services/fabrica_da_celula.py:187–188`; `app/pautas/services.py:174–176`)

## 31/10 13:37 · sessao 608be0c6532d · CLAUDE-MD-RACIONAL · verificador · origem=Code

1. **Sim, com o mesmo efeito.** Data e obrigação de citar o aval estão em `CLAUDE.md:444-445`. As condições conjuntas — função existente, autoridade única medida, mesmo predicado e universo igual, medidos na sessão — continuam em `CLAUDE.md:460-464`; predicado novo ou duas candidatas exigem pergunta em `CLAUDE.md:453-454`. Correspondem a `41b4d89d:CLAUDE.md:478-486`.

   `app/docs/CORTES.md:64` e `app/docs/CORTES.md:283` continuam encontrando o aval pelo nome e pela data. **A numeração é histórica:** o antigo item 3 corresponde agora ao item 5 de PARADA, identificado pela ponte em `CLAUDE.md:444`.

2. **Não encontrei perda de cláusula, nome, script ou condição nos cortes desta volta.** A preservação aparece nestes pontos:

   | Trecho | Evidência atual |
   |---|---|
   | PARADA: RED, script e três sentidos | `CLAUDE.md:442-443` |
   | RAIAS: fonte do handoff e avais de hoje não cumpridos | `CLAUDE.md:483-484` |
   | CODEX: apelidos retirados da linha duplicada | `CODEX-SONDA-LEITURA` em `CLAUDE.md:524`; `SUITE-ANTES-DO-OPUS` em `CLAUDE.md:515` e `CLAUDE.md:549` | [nao verificado]
   | RELATO: mesma pergunta, frase de nascimento e declaração em `JUIZES_DO_PRAZO` | `CLAUDE.md:607`, `CLAUDE.md:624-627` |
   | PLACAR: exercício, dois zeros, rótulo, mostrar sem fechamento e L-095 | `CLAUDE.md:565-570` |
   | PONTUALIDADE: ponteiro e regra | `CLAUDE.md:601`, `app/docs/LEIS.md:116` |
   | DINHEIRO: backup/renome pelo admin | `CLAUDE.md:577` |

3. **Não identifiquei efeito adicional além da precedência dos cinco consolidados.** A exceção está expressamente limitada a eles em `app/docs/PISTA.md:10-12`. O selo extrai os blocos pelas âncoras, deixando o cabeçalho fora dos hashes; verifica nomes, hashes e ponteiros, sem comparar a frase antiga (`bin/tests/test_claude_md_regras_preservadas.sh:43-78`). Nenhuma dependência dessa frase foi encontrada nos leitores examinados; o leitor de `PISTA.md` identificado está em `bin/tests/test_claude_md_regras_preservadas.sh:35`.

4. **Contagem medida com `wc -c`**, incluindo as quebras de linha:

   | Conteúdo | Bytes | Localização |
   |---|---:|---|
   | 7b com título, até antes de `## 8.` | **20.995** | `CLAUDE.md:436-628` |
   | Mesmo trecho sem a linha do título | **20.911** | `CLAUDE.md:437-628` |
   | Linha do título | **84** | `CLAUDE.md:436` |
   | Arquivo inteiro | **57.796** | `CLAUDE.md:1-632` |

## 31/10 13:37 · sessao aa15e33c6c46 · - · - · origem=Ronald (tmux)

A escrita física encontrada é `Pauta.objects.create(..., texto=texto[:500])`; a porta normaliza com `strip`, rejeita vazio e corta a 500 no último passo. `responder` passa pela mesma porta. (`app/pautas/services.py:163-182`, `app/pautas/services.py:189-197`)

| Chamador de escrita | Referência |
|---|---|
| View escrever | `app/pautas/views.py:76` |
| View responder → service responder | `app/pautas/views.py:112` |
| CLI suporte responder → service responder | `app/pautas/management/commands/suporte_hasner.py:133` |
| Semanal | `app/pautas/semanal.py:276` |
| Dia em aberto: cabeça/filhas | `app/pautas/management/commands/pauta_dia_em_aberto.py:127`, `:139`, `:142` |
| Pré-fechamento | `app/folha/management/commands/pre_fechamento.py:110` |
| Aparelhos compartilhados | `app/colaboradores/management/commands/pauta_aparelhos_compartilhados.py:107` |
| Cobertura | `app/ponto/services/cobertura.py:333` |
| Ausência/documento vencido; férias recusadas | `app/ponto/services/ausencia.py:1039`, `:1229` |
| Discordantes | `app/ponto/services/pautas_dos_discordantes.py:177` |
| Afastado avisa | `app/ponto/services/afastado_avisa.py:74` |
| Deslogados | `app/ponto/services/pautas_dos_deslogados.py:77` |
| HE | `app/ponto/services/autorizacao_he_periodo.py:244` |
| Esmeril | `app/ponto/services/pautas_do_esmeril.py:76` |
| Férias | `app/ferias/management/commands/pautas_de_ferias.py:167` |
| Cron quebrado | `app/core/services/crons_quebrados.py:71` |
| Alarme esteira; árvore | `app/core/management/commands/alarme_esteira.py:54`, `app/core/management/commands/alarme_arvore.py:50` |
| Limbo exportado | `app/chamados/services/fabrica_da_celula.py:185` |
| Passivo validado: cabeça/filhas/quem saiu | `app/chamados/services/passivo_validado.py:229`, `:233`, `:266` |
| Resposta tardia | `app/chamados/services/resposta_tardia.py:62` |
| Competência trancada | `app/chamados/services/competencia_trancada.py:224` |
| Órfãs enterradas | `app/chamados/management/commands/reabrir_orfas_enterradas.py:197` |

Entre os emissores automáticos, fábrica e cobertura usam âncora `dia`; views aceitam a âncora enviada e respostas herdam a âncora original. (`app/chamados/services/fabrica_da_celula.py:185`, `app/ponto/services/cobertura.py:334`, `app/pautas/views.py:80`, `app/pautas/services.py:196`)

O campo declara `max_length=500`; a migration inicial apenas cria o schema. (`app/pautas/models.py:89`, `app/pautas/migrations/0001_initial.py:39`)

Defeitos adicionais:

- **A exceção limbo exportado pode ser contornada pelo lote.** O evento envia à Pauta e retorna sem perguntar, mas o universo FIO MUDO do lote seleciona chamado vivo sem filtrar `trabalha` ou competência, procura motivo pela ata e chama diretamente `perguntar`. Para chamado vivo de limbo exportado com motivo faltante e sem pergunta existente, a Pauta não barra esse caminho. O cron está configurado com `--apply`, e não apenas como vigia. Ocorrência desses candidatos em produção: **[não verificado]**. (`app/chamados/services/fabrica_da_celula.py:153-157`, `app/chamados/management/commands/disparar_perguntas_competencia.py:155-168`, `:178-224`, `:277-301`, `app/config/crons.py:1464-1466`)
- **Comentário novo contradiz a implementação:** diz que `escrever` corta o fim do corpo para acomodar contexto; agora o contexto cede, preservado o corte global de 500. (`app/chamados/services/fabrica_da_celula.py:187-188`, `app/pautas/services.py:174-182`)
- A guarda de idempotência faz consulta seguida de criação sem trava local; o modelo não tem unicidade marcador/âncora. Duas chamadas concorrentes podem ambas observar ausência e escrever. Serialização externa ou duplicação ocorrida: **[não verificado]**. (`app/chamados/services/fabrica_da_celula.py:180-190`, `app/pautas/models.py:123-131`)

## 31/10 13:37 · sessao c6c1828c7027 · - · - · origem=Ronald (tmux)

Não encontrei perda nos cortes **desta volta**:

- **PARADA:** RED e três sentidos continuam em `CLAUDE.md:442-443`, equivalentes a `41b4d89d:CLAUDE.md:76-84`.
- **RAIAS:** `CLAUDE.md:484` recupera exatamente as condições “não de leitor novo” e “avais de hoje não cumpridos” da base `41b4d89d:CLAUDE.md:611-612`.
- **CODEX:** eliminou apenas repetição dos apelidos: `CODEX-SONDA-LEITURA` permanece em `CLAUDE.md:524` e `app/docs/PISTA.md:98`; `SUITE-ANTES-DO-OPUS`, em `CLAUDE.md:515`, `CLAUDE.md:549` e `app/docs/PISTA.md:113-114`.
- **PLACAR:** exercício, duas condições de zero, rótulo, mostrar sem fechamento e manutenção da L-095 permanecem em `CLAUDE.md:565-570`, equivalentes à base `41b4d89d:CLAUDE.md:515-523`.
- **RELATO:** “mesma pergunta”, declaração em `JUIZES_DO_PRAZO` e frase citando a regra continuam conjuntamente em `CLAUDE.md:607` e `CLAUDE.md:624-627`, equivalentes à base `41b4d89d:CLAUDE.md:658-663`.
- **PONTUALIDADE:** nesta volta apenas juntou linhas; ponteiro em `CLAUDE.md:601`, conteúdo preservado em `app/docs/LEIS.md:116`, correspondente à base `41b4d89d:CLAUDE.md:593-600`.
- **DINHEIRO:** responsabilidade do admin pelo backup/renome permanece em `CLAUDE.md:577`, correspondente à base `41b4d89d:CLAUDE.md:535`.

## 31/10 13:37 · sessao 16a8af0f3457 · - · - · origem=Ronald (tmux)

- Os quatro testes novos são `test_RED_contexto_460...`, `test_RED_contexto_480...`, `test_RED_contexto_700...` e `test_porta_escrever...`; os três primeiros delegam ao mesmo helper, e o quarto chama `escrever` diretamente. (`app/chamados/tests/test_f1c4_3_emissores.py:265`, `:284`, `:287`, `:290`, `:295`)

- Uma mutação local que derruba os quatro é, em `app/pautas/services.py:176`, substituir apenas o primeiro argumento `texto` da tupla por `texto[:500 - len(_ctx) - 4]`. Isso reintroduz o corte antigo do corpo, mantendo o novo corte do contexto. (`app/pautas/services.py:174`, `:176`)

| Teste | Efeito dessa mutação | [nao verificado]
|---|---|
| Contexto 460 | Guarda somente 36 caracteres do corpo; preserva o marcador, mas mutila a tag completa, fazendo a segunda passada criar outra pauta. O desempacotamento `(p,)` encontra duas. (`app/pautas/services.py:176`; `app/chamados/services/fabrica_da_celula.py:181`, `:182`; `app/chamados/tests/test_f1c4_3_emissores.py:278`) |
| Contexto 480 | Guarda somente 16 caracteres do corpo; perde o marcador completo, e `_pautas()` não encontra as pautas criadas. (`app/pautas/services.py:176`; `app/chamados/tests/test_f1c4_3_emissores.py:193`, `:278`) |
| Contexto 700 | O corpo integrado tem 154 caracteres; `texto[:-204]` resulta vazio, perdendo o marcador e fazendo falhar o desempacotamento. (`app/chamados/services/fabrica_da_celula.py:164`, `:189`; `app/chamados/services/declaracao_texto.py:44`; `app/chamados/tests/test_f1c4_3_emissores.py:278`, `:291`) |
| Porta direta | O corpo tem 78 caracteres; a iteração 100 permanece válida, mas a iteração 460 reduz o corpo a 36 e falha no `startswith` exigido. (`app/chamados/tests/test_f1c4_3_emissores.py:299`, `:300`, `:306`; `app/pautas/services.py:176`) |

- Na execução normal, todos entram no bloco novo: o corpo integrado tem 154 caracteres, portanto `_sobra=342`; o direto tem 78, portanto `_sobra=418`; todos os contextos simulados são não vazios. O literal `'[DIA x] '` tem oito caracteres, então os contextos têm exatamente o comprimento anunciado. (`app/chamados/services/fabrica_da_celula.py:189`; `app/chamados/tests/test_f1c4_3_emissores.py:274`, `:299`, `:301`; `app/pautas/services.py:174`, `:175`)

- Entretanto, os três testes de integração continuam passando se `app/pautas/services.py:175` virar `if False:`: não exigem contexto nem `[DIA ` no resultado; exigem somente marcador, tag, tamanho, unicidade e ausência de perguntas. O teste direto detecta esse bypass. (`app/chamados/tests/test_f1c4_3_emissores.py:278`, `:279`, `:280`, `:281`, `:282`, `:306`)

- Os quatro também aceitam substituir `_ctx[:_sobra]` por `_ctx[:7]`, anexando apenas `[DIA x]`; o teste direto verifica somente esse prefixo, não a extensão correta do contexto. (`app/pautas/services.py:176`; `app/chamados/tests/test_f1c4_3_emissores.py:279`, `:306`)

- Nenhum dos quatro cobre contexto vazio, corpo contendo `[DIA `, `T>=496`, `_sobra<=0` ou corpo maior que 500; retirar o `[:500]` final não os derruba. O nome “nunca o corpo” não vale universalmente: o service continua cortando qualquer corpo acima de 500. (`app/chamados/tests/test_f1c4_3_emissores.py:274`, `:299`, `:300`, `:301`; `app/pautas/services.py:168`, `:174`, `:175`, `:182`)

- As falhas descritas foram derivadas por leitura e simulação isolada de strings; execução efetiva dos testes mutados: [não verificado]. [nao verificado]

## 31/10 13:36 · sessao 9315cc53a28b · LEI-AKITA-UNIVERSO-DO-PUSH · verificador · origem=Code

**O portão ainda tem brechas:** `origin/main` ausente produz verde vazio, e o `pre-push` não chama este selo. (`bin/tests/test_lei_akita.sh:93`, `bin/tests/test_lei_akita.sh:101`, `bin/tests/test_lei_akita.sh:153`; `bin/pre-push.sh:12–19`)

1. **O universo não é literalmente `origin/main..HEAD`.** É esse conjunto limitado à primeira parentalidade e ao piso do corte. (`bin/tests/test_lei_akita.sh:94–101`)

   - **`HEAD == origin/main`:** retorna vazio corretamente; significa ausência de commits pendentes, não prova de julgamento anterior. (`bin/tests/test_lei_akita.sh:101–107`)
   - **`origin/main` inexistente — falha:** o Git falha, mas o helper descarta status e stderr; stdout vazio vira `[]`, seguido de `PADRAO_FIM`, aceito pelo shell. Portanto cala commits que deveria julgar. (`bin/tests/test_lei_akita.sh:92–107`, `bin/tests/test_lei_akita.sh:143–153`)
   - **Referência desatualizada:** não há sincronização nessa função. Se estiver atrasada, poderá rejulgar commits já publicados; ela conhece a referência local, não o estado atual do servidor. (`bin/tests/test_lei_akita.sh:92–101`)
   - **Merge `--no-ff`:** só julga o merge; mensagens defeituosas dos commits da raia ficam fora. Essa política está declarada no comentário, mas difere do range literal completo. (`bin/tests/test_lei_akita.sh:97–104`)
   - **Outra brecha no merge:** `git show --name-only` pode omitir arquivos incorporados sem conflito; assim, `n/a` pode passar embora o pouso integre negócio. A decisão depende justamente dessa lista. Essa fragilidade já existia antes da mudança de universo. (`bin/tests/test_lei_akita.sh:83–85`, `bin/tests/test_lei_akita.sh:103–104`)
   - **Corte na raiz:** tratado corretamente: sem pai, `pisos=[]`, evitando revisão inválida. O próprio caso plantado usa essa configuração. (`bin/tests/test_lei_akita.sh:99`, `bin/tests/test_lei_akita.sh:127–130`)

2. **Quem chama e quando.** O chamador operacional encontrado é o glob de `bin/regua.sh`, que executa os selos e decide pelo exit code. Porém seu atalho `JA VERDE` termina antes desse glob e compara arquivos `.py`/`.html`, sem considerar mensagens dos commits ou posição de `origin/main`. (`bin/regua.sh:33–35`, `bin/regua.sh:84–94`, `bin/regua.sh:138–145`)

   **`bin/pre-push.sh` não chama `test_lei_akita.sh`.** Chama individualmente outros dois selos e declara expressamente que o loop de `bin/tests/` não roda ali. (`bin/pre-push.sh:12–19`)

   Há caminhos concretos que deixam o commit novo escapar: o molde roda a régua antes do commit; depois commita e chama novamente sem `--force`, permitindo o atalho sobre o verde anterior; então faz push. O molde de autocura repete a sequência. (`bin/molde_fatia/cadeia.sh:51`, `bin/molde_fatia/cadeia.sh:68–85`; `bin/molde_autocura/cadeia.sh:48`, `bin/molde_autocura/cadeia.sh:65–82`)

   O integrador também roda `regua.sh --force` antes de `_subir`, que cria os commits e depois empurra. (`app/core/integrador_lote.py:375`, `app/core/integrador_lote.py:701`, `app/core/integrador_lote.py:719–739`)

   **Não encontrei chamada automática do selo depois do push nesses fluxos.** O passo explicitamente posterior, `pos_push.sh`, faz fetch e atualiza TICKETS, sem chamar a régua. A brecha encontrada é julgamento pré-commit seguido de ausência no hook. (`bin/push.sh:6–8`; `bin/pos_push.sh:18–39`; `bin/pre-push.sh:12–19`)

   A atualidade efetiva de `origin/main` em cada execução histórica está **[nao verificado]**.

3. **O MORDE cobre uma história linear mínima:** corte-raiz → publicado ruim → novo ruim, exigindo exatamente o hash do novo. As conclusões abaixo são por análise da asserção; executar o caso integral criaria arquivos e repositório temporário. (`bin/tests/test_lei_akita.sh:122–140`)

   | Mutante | Resultado pelo caso plantado |
   |---|---|
   | Retirar `^origin/main` | **Morre:** acusa também o publicado. (`bin/tests/test_lei_akita.sh:101`, `bin/tests/test_lei_akita.sh:132–139`) |
   | Calar `julgar` ou devolver universo vazio | **Morre:** perde o novo obrigatório. (`bin/tests/test_lei_akita.sh:104–106`, `bin/tests/test_lei_akita.sh:139`) |
   | Aplicar `^corte^` mesmo na raiz | **Morre:** revisão inválida resulta em vazio. (`bin/tests/test_lei_akita.sh:93`, `bin/tests/test_lei_akita.sh:99`, `bin/tests/test_lei_akita.sh:139`) |
   | Julgar somente `HEAD` | **Sobrevive:** existe apenas um commit novo. (`bin/tests/test_lei_akita.sh:132–139`) |
   | Remover o piso | **Sobrevive:** o fixture já tem corte-raiz. (`bin/tests/test_lei_akita.sh:99`, `bin/tests/test_lei_akita.sh:129–130`) |
   | Remover `--first-parent` | **Sobrevive:** não há merge no fixture. (`bin/tests/test_lei_akita.sh:101`, `bin/tests/test_lei_akita.sh:127–136`) |
   | Continuar ignorando erros Git | **Sobrevive:** não se planta referência ausente nem erro de comando. (`bin/tests/test_lei_akita.sh:125–136`) |

   O juiz compartilhado rejeita o termo inventado quando recebe o commit; a garantia incompleta está na seleção e na execução pelo portão. (`bin/tests/test_lei_akita.sh:88–89`, `bin/tests/test_lei_akita.sh:104`, `bin/tests/test_lei_akita.sh:110–118`)

4. **A LÁPIDE 72 está correta nos termos e na correspondência principal com as curas.** (`app/docs/LAPIDES.md:785–786`)

   **`ac721607`: `Event-driven architecture` existe literalmente.** O encerramento passa pelo consumidor de eventos, enquanto o reconciliador periódico atua como vigia sem escrita; corresponde à descrição da lápide. (`app/docs/GLOSSARIO.md:16`; `app/chamados/reconciliador.py:1304–1309`; `app/ponto/management/commands/reconciliar_perguntas_orfas.py:21–35`; `app/docs/LAPIDES.md:785`)

   **`2cd634cb`: `Architectural Invariants + Fitness Functions` existe literalmente.** A cura move `pre_fechamento` de 05:15 para 05:25 e ajusta o vizinho declarado; preserva a recusa de cron que não cabe, fazendo a configuração satisfazer o invariante existente. (`app/docs/GLOSSARIO.md:14`; `app/config/crons.py:214–219`, `app/config/crons.py:252–258`, `app/config/crons.py:387`; `app/docs/LAPIDES.md:786`)

5. **Não há allowlist dos dois hashes nem juiz novo:** eles aparecem como história; a decisão continua em `julgar`, compartilhada pelo universo e pelo MORDE. As exigências de padrão conhecido e restrição de `n/a` já pertencem à L-123. (`bin/tests/test_lei_akita.sh:53–56`, `bin/tests/test_lei_akita.sh:73–90`, `bin/tests/test_lei_akita.sh:104`, `bin/tests/test_lei_akita.sh:118`; `app/docs/LEIS.md:144`)

   **`LEI_AKITA_R` passa a controlar toda a raiz auditada**, inclusive documentos, código e histórico; o default continua sendo `/home/ronald/saas-hasner`, independentemente do diretório de execução. A régua não define essa variável na chamada. (`bin/tests/test_lei_akita.sh:17`, `bin/tests/test_lei_akita.sh:28–46`, `bin/tests/test_lei_akita.sh:60–65`, `bin/tests/test_lei_akita.sh:143`; `bin/regua.sh:140`)

   **O contrato de saída permanece:** os marcadores Python são consumidos pelo próprio shell; a régua usa o exit code e imprime a saída na falha. O `lei_akita: OK` não informa quantos commits foram julgados, portanto não distingue julgamento efetivo de universo vazio. (`bin/tests/test_lei_akita.sh:145–163`; `bin/regua.sh:140–143`)

## 31/10 13:36 · sessao 9d8875003f72 · - · - · origem=Ronald (tmux)

3. **Não há efeito adicional identificado.** O cabeçalho mantém a precedência de `PISTA.md` e abre exceção expressa somente para CODEX, PARADA E AUTONOMIA, DINHEIRO, RAIAS E POUSO e RELATO consolidados (`app/docs/PISTA.md:8-12`; antes: `e8d0cb0b:app/docs/PISTA.md:8-10`).

Os 17 blocos conferem com os hashes registrados, incluindo suas condições e scripts; a mudança está fora dos blocos ancorados (`app/docs/PISTA.md:16`; `bin/tests/pista_blocos.ref:1-17`; extração em `bin/tests/test_claude_md_regras_preservadas.sh:47-52`).

**Nenhum leitor da frase antiga foi encontrado em `bin/` ou `.claude/`.** O leitor identificado verifica nomes, hashes e ponteiros; não interpreta nem compara a frase de precedência (`bin/tests/test_claude_md_regras_preservadas.sh:43-78`). Os hooks configurados executam outros arquivos (`.claude/settings.json:9`, `:20`, `:29`, `:40`, `:44`, `:53`, `:57`, `:68`).

O selo integral não foi executado porque cria arquivos temporários; os hashes foram conferidos em memória (`bin/tests/test_claude_md_regras_preservadas.sh:89-112`; `bin/tests/pista_blocos.ref:1-17`).

## 31/10 13:36 · sessao 09345b96188f · - · - · origem=Ronald (tmux)

Inventário dos leitores de `Pauta.texto` em produção na cópia:

| Sítio | O que lê/procura |
|---|---|
| `app/chamados/services/fabrica_da_celula.py:180-182` | Prefixo `LIMBO EM COMPETENCIA EXPORTADA`; depois substring exata `[motivo:<m>]` em todo texto, para deduplicar motivos. |
| `app/pautas/semanal.py:199` | Prefixo `<marcador> <semana ISO>`, âncora competência. |
| `app/pautas/semanal.py:228` | Prefixo do marcador para fechar pautas vivas da família. |
| `app/pautas/semanal.py:245-248` | Substring `n=<contadores separados por vírgula>` no texto da semana anterior. |
| `app/pautas/management/commands/pauta_dia_em_aberto.py:108` | Prefixo `[DIA EM ABERTO]`; âncora competência (`:104-108`). |
| `app/ferias/management/commands/pautas_de_ferias.py:149-156` | Prefixo de `MARCADORES[codigo]`: `[FERIAS/PA-VENCIDO]`, `[FERIAS/PA-VENCENDO]`, `[FERIAS/CADASTRO]` (`:30-33`). |
| `app/folha/management/commands/pre_fechamento.py:91-98` | Prefixo `[PRE-FECHAMENTO]` e prefixo mais específico com a data de hoje (`:29,93`). |
| `app/chamados/services/competencia_trancada.py:211` | Prefixo `[TRANCA]`, âncora competência. |
| `app/chamados/services/passivo_validado.py:159-164` | Prefixo `[PASSIVO-VALIDADO] col<id> AAAA-MM` (`:44`). |
| `app/chamados/services/passivo_validado.py:254-256` | Prefixo `[PASSIVO-VALIDADO-FORA] col<id> AAAA-MM` (`:48`). |
| `app/chamados/management/commands/reabrir_orfas_enterradas.py:190-191` | Prefixo `[ORFA-ENTERRADA]` (`:32`). |
| `app/core/management/commands/alarme_arvore.py:16` | Prefixo `ARVORE VERMELHA` (`:11`). |
| `app/core/management/commands/alarme_esteira.py:15` | Prefixo `ESTEIRA <chave>:` (`:10`). |
| `app/core/services/crons_quebrados.py:53-54` | Prefixo `[CRON QUEBRADO] <nome> (empresa <emp ou ->)` (`:15`). |
| `app/ponto/services/autorizacao_he_periodo.py:240-242` | Prefixo `[HE-AUTORIZADA-EM-EXPORTADA]` (`:124`), cabeças vivas em competência. |
| `app/ponto/services/pautas_dos_deslogados.py:65-66` | Prefixo `[APP-401]` (`:26`); chave restante vem da âncora, não do texto (`:68`). |
| `app/ponto/services/afastado_avisa.py:62-65` | Prefixo formado pelos primeiros 40 caracteres de `TEXTO`; âncora colaborador. |
| `app/ponto/services/ausencia.py:1022-1025` | Prefixo `[A4/SEM-DOCUMENTO]`; âncora colaborador. |
| `app/ponto/services/ausencia.py:1212-1215` | Prefixo `[A1-FERIAS/AG#<id>]`; âncora colaborador. |
| `app/ponto/services/cobertura.py:241-243` | Prefixo `[COBERTURA-A-CONFIRMAR]` (`:207`); âncora dia. |
| `app/ponto/services/pautas_do_esmeril.py:63-65` | Prefixo `[ESMERIL `; extrai a ação até o primeiro `]`; âncora posto. |
| `app/ponto/services/pautas_dos_discordantes.py:165-167` | Prefixo `[DISCORDANTE]` (`:37`), depois `_chave_do_texto` pega todo o trecho anterior ao primeiro `:` (`:96-100`); âncora competência (`:39`). |
| `app/pautas/views.py:40,47` | Serializa integralmente cabeça e respostas; sem interpretar conteúdo. |
| `app/api/views_mensageria.py:1324-1328` | Serializa integralmente texto da pauta na API de mensageria. |
| `app/templates/core/_copiloto.html:405,424` | Exibe cabeça e respostas por `textContent`; sem interpretar marcador/contexto. |
| `app/templates/colaboradores/detalhe.html:119` | Exibe texto com `truncatechars:180`: comprimento afeta somente a apresentação. |
| `app/pautas/management/commands/suporte_hasner.py:111,113` | Imprime integralmente cabeça e respostas no CLI. |

A única busca executável pelo trecho literal `[DIA ` está na **entrada** de `escrever`, para decidir se acrescenta contexto; não é um parser da pauta persistida (`app/pautas/services.py:163-169`). A construção do contexto coloca data/nome/matrícula/marcos/batidas/acusação/URL no corpo, mas não há consumidor de produção encontrado que extraia esses campos de `Pauta.texto`; os consumidores acima apenas os apresentam (`app/pautas/services.py:415-425`; `app/pautas/views.py:40`; `app/templates/core/_copiloto.html:405`; `app/templates/colaboradores/detalhe.html:119`).

A troca pode mudar a deduplicação do limbo porque preserva o prefixo e as tags que o corte anterior podia remover; o leitor busca ambos no corpo inteiro e não depende do contexto (`app/chamados/services/fabrica_da_celula.py:180-182`; `app/pautas/services.py:170-182`). O leitor de cobertura também recebe textos de dia, mas só procura seu prefixo; todos os demais leitores de negócio citados filtram outras âncoras, logo não passam pelo ramo de contexto alterado (`app/ponto/services/cobertura.py:241-243`; `app/pautas/services.py:168`).

Defeito associado: a nova justificativa “o dia o DP reabre pela ancora” não funciona pelo clique do painel: `ROTA` contempla somente `colab`, `chamado` e `posto`; `dia` cai no aviso de que não possui tela própria (`app/pautas/services.py:173`; `app/templates/core/_copiloto.html:564-574`). O URL textual do calendário fica no fim do contexto e pode desaparecer no novo corte; o painel também o renderiza como texto simples, sem link (`app/pautas/services.py:174-176,423-425`; `app/templates/core/_copiloto.html:405`). A ausência de rota é preexistente; a alteração passa a depender dessa rota ao permitir contexto ausente (`app/templates/core/_copiloto.html:564-574`; `app/pautas/services.py:173-176`).

Fora da aplicação de produção, a infraestrutura de sombra inventaria `Pauta.texto` para mascaramento: preserva marcador inicial entre colchetes e calcula hash do restante; não é leitor de negócio (`bin/sombra_regras_pessoais.py:24,29-33,81-84`; `bin/sombra_mascara_pessoal.py:44`).

## 31/10 13:35 · sessao 65da1b509851 · - · - · origem=Ronald (tmux)

Universo e MORDE, somente leitura:

- **Não é literalmente `origin/main..HEAD`**: usa `--first-parent` e exclui ancestrais do pai do corte; commits da segunda parentalidade e anteriores ao piso ficam fora. Isso é explicitamente descrito e implementado em `bin/tests/test_lei_akita.sh:94-101`. Ambos os filtros já existiam no pai; a extração da função e `^origin/main` são a mudança do commit. `bin/tests/test_lei_akita.sh:91-107` [comparação Git executada; diff sem arquivo de evidência].
- **HEAD igual a origin/main:** universo vazio esperado, sem falhas de padrão; a função retorna `[]` e imprime `PADRAO_FIM`, aceito pelo shell. `bin/tests/test_lei_akita.sh:101-107`, `143-153`.
- **Origin inexistente:** `rev-list` falha, mas o helper usa apenas stdout e ignora status/stderr; stdout vazio vira `[]`, depois `PADRAO_FIM`. Portanto o selo cala indevidamente o universo. `bin/tests/test_lei_akita.sh:92-93`, `101-107`, `143-153`.
- **Origin atrasado:** commits já publicados, mas posteriores à referência local, voltam ao julgamento; não há atualização da referência na função. `bin/tests/test_lei_akita.sh:92-101`.
- **Corte na raiz:** o teste de existência do pai deixa `pisos=[]`, evitando a revisão inválida `^<raiz>^`. O próprio fixture usa corte-raiz. `bin/tests/test_lei_akita.sh:99`, `127-130`.
- **Merge `--no-ff`:** só a mensagem do merge é julgada, por causa de `--first-parent`. Além disso, `git show --name-only` usa o diff combinado, podendo ocultar código de negócio integrado; `n/a` então pode passar com lista de arquivos só documentais. `bin/tests/test_lei_akita.sh:97-104`, `83-85`. Confirmei em leitura Git: `c7ea207a` mostra só BACKLOG/PROMPTS/RELATO nessa chamada, enquanto o diff contra primeiro pai inclui crons.py, juizes.py, models.py etc. [nao verificado por arquivo:linha para a saída Git]. Essa fragilidade já existe no pai.

**MORDE do universo:** o fixture tem apenas `corte-raiz → publicado ruim → novo ruim`, exigindo exatamente o hash do novo. `bin/tests/test_lei_akita.sh:127-139`.

Por análise do código, ele mata:

- Retirar `^origin/main`: acusa também o publicado. `bin/tests/test_lei_akita.sh:101`, `132-139`.
- Calar o juiz ou retornar universo vazio: perde o novo esperado. `bin/tests/test_lei_akita.sh:104-106`, `139`.
- Impor `^corte^` mesmo quando corte é raiz: Git falha, universo fica vazio e diverge do esperado. `bin/tests/test_lei_akita.sh:93`, `99-101`, `129-139`.

Sobrevivem:

- Remover `--first-parent`: fixture é linear. `bin/tests/test_lei_akita.sh:101`, `127-136`.
- Remover o piso: fixture já tem corte-raiz, portanto `pisos=[]`. `bin/tests/test_lei_akita.sh:99`, `129-130`.
- Julgar somente HEAD: há só um novo commit. `bin/tests/test_lei_akita.sh:132-139`.
- Escolher o último corte em vez do primeiro: há só uma ocorrência introduzida. `bin/tests/test_lei_akita.sh:94`, `129`.
- Ignorar falhas Git: fixture não planta referência ausente nem falhas de comando. `bin/tests/test_lei_akita.sh:125-136`.

Não executei o MORDE integral nem mutações em disco, pois ele cria arquivos e repositório temporário. `bin/tests/test_lei_akita.sh:122-136`.

## 31/10 13:34 · sessao 881fe507cad9 · - · - · origem=Ronald (tmux)

- **O selo não é chamado pelo pre-push.** O hook chama individualmente `test_lei_protege_sitio.sh` e `test_achado_por.sh`, e seu comentário registra que o loop de `bin/tests/` não roda ali; seu arsenal posterior roda testes Django (`bin/pre-push.sh:12`, `:16`, `:19`, `:155`, `:182`, `:194`). A cópia em `app/core/hooks/pre-push.sh` também só chama suas guardas próprias e a suíte (`app/core/hooks/pre-push.sh:7`, `:54`, `:57`, `:87`). O wrapper instalado delega ao `bin/pre-push.sh` (`bin/hooks.sh:56`).

- **Chamador real encontrado: o glob da régua**, que executa cada selo com Bash, consome seu exit code, imprime a saída somente na falha e aborta (`bin/regua.sh:138`, `:140`, `:142`, `:143`). **A régua pode sair antes desse glob** com `JA VERDE`, comparando apenas a impressão de arquivos `.py`/`.html` e status anterior; mensagens de commit e posição de `origin/main` não participam dessa condição (`bin/regua.sh:25`, `:33`, `:84`, `:86`, `:93`). Embora calcule `ARVORE_FP`, o atalho da própria régua não o compara (`bin/regua.sh:45`, `:86`).

- **Falha concreta do fluxo:** o molde roda a régua antes do commit, faz o commit e chama a régua mais duas vezes, todas sem `--force`; essas chamadas posteriores podem atalhar sobre o verde anterior. O push vem depois e seu hook não chama este selo (`bin/molde_fatia/cadeia.sh:51`, `:68`, `:70`, `:81`, `:85`; `bin/pre-push.sh:12`). O molde de autocura tem a mesma sequência (`bin/molde_autocura/cadeia.sh:48`, `:65`, `:67`, `:78`, `:82`).

- **Integrador também pode publicar commit que o selo ainda não viu:** sua suíte chama `bin/regua.sh --force`, porém `_subir` faz os commits depois e em seguida dá push (`app/core/integrador_lote.py:375`, `:691`, `:701`, `:719`, `:739`). Há ainda caminho de empurrar commits locais antes de rodar o lote, protegido somente pelo hook existente (`bin/integrador.sh:36`, `:44`, `:52`).

- **Estado de `origin/main`:** os chamadores acima usam a referência local existente; não há atualização acoplada ao selo, cuja consulta usa diretamente `^origin/main` (`bin/tests/test_lei_akita.sh:93`, `:101`). Sua atualidade efetiva no momento de cada execução histórica: **[nao verificado]**.

- **Não encontrei chamada automática do selo pós-push nos fluxos rastreados.** Nas cadeias todas as chamadas da régua antecedem o push; depois há deploy (`bin/molde_fatia/cadeia.sh:81`, `:85`, `:89`; `bin/molde_autocura/cadeia.sh:78`, `:82`, `:86`). O wrapper `push.sh` chama `pos_push.sh` após sucesso, mas esse script só atualiza `origin`, reescreve derivados de TICKETS e commita TICKETS (`bin/push.sh:6`, `:8`; `bin/pos_push.sh:18`, `:19`, `:20`, `:29`, `:30`). Uma régua posterior pode legitimamente encontrar universo vazio; isso não comprova que o commit foi julgado antes (`bin/tests/test_lei_akita.sh:101`, `:107`, `:148`).

- **Consumidores e contrato:** `LEI_AKITA_R` passa a controlar toda a raiz do selo, inclusive suas verificações iniciais e leitura dos termos; os chamadores encontrados não definem essa variável (`bin/tests/test_lei_akita.sh:17`, `:28`, `:35`, `:45`, `:60`, `:65`; `bin/regua.sh:140`). A régua depende do exit code, não do texto `lei_akita: OK` (`bin/regua.sh:140`). Os marcadores Python continuam internos, interpretados pelo shell do próprio selo: `PADRAO_FALHA`, `PADRAO_FIM`, `PADRAO_SEM_CORTE`, `MORDE_OK`, `MORDE_FALHOU` (`bin/tests/test_lei_akita.sh:145`, `:147`, `:148`, `:151`, `:152`, `:153`, `:156`). A saída pública final continua sendo `lei_akita: OK (...)` ou `lei_akita: N falha(s)` (`bin/tests/test_lei_akita.sh:160`, `:163`).

## 31/10 13:34 · sessao 4f29e9379c5b · - · - · origem=Ronald (tmux)

A LAPIDE 72 está correta quanto aos termos e à realização principal dos diffs históricos, conferidos via `git diff ac721607^1 ac721607` e `git show 2cd634cb`:

- **Event-driven architecture** existe literalmente em `app/docs/GLOSSARIO.md:16`. O diff de `ac721607` move o encerramento para o consumidor de eventos e transforma o cron em vigia: execução atual em `app/chamados/reconciliador.py:1307`, escrita pela porta em `app/chamados/reconciliador.py:1099`, vigia em `app/ponto/management/commands/reconciliar_perguntas_orfas.py:29` e troca para `--alarme` em `app/config/crons.py:1469`. A descrição de `app/docs/LAPIDES.md:785` corresponde.
- **Architectural Invariants + Fitness Functions** existe literalmente em `app/docs/GLOSSARIO.md:14`. O diff de `2cd634cb` muda `VIZINHO_SOMBRA` e o horário de `pre_fechamento`, ambos 05:15 → 05:25; o estado resultante está em `app/config/crons.py:219` e `app/config/crons.py:387`. A recusa existente permanece em `app/config/crons.py:252`; a alteração faz a configuração satisfazer o invariante. A descrição de `app/docs/LAPIDES.md:786` corresponde.
- O primeiro commit também realiza fonte única ao extrair `expiracao_da_aberta`, lida por `turno_aberto_vivo` e pelo motivo do dia encerrado; isso não torna falso o padrão principal nomeado na lápide (`app/ponto/turnos.py:1991`, `app/ponto/turnos.py:1997`, `app/ponto/turnos.py:2099`; termo existente em `app/docs/GLOSSARIO.md:11`).
- O diff de `d8912301` contra o pai altera somente o selo e acrescenta a lápide. Não modifica `julgar`: continua uma única função, chamada pelo universo e pelos casos plantados (`bin/tests/test_lei_akita.sh:73`, `bin/tests/test_lei_akita.sh:104`, `bin/tests/test_lei_akita.sh:118`). Os hashes publicados aparecem em comentários/documentação, sem condição de dispensa (`bin/tests/test_lei_akita.sh:53`, `app/docs/LAPIDES.md:785`, `app/docs/LAPIDES.md:786`).
- As exigências verificadas já pertencem à L-123: termo conhecido, restrição de `n/a` e conferência humana contra o diff (`app/docs/LEIS.md:144`, `app/docs/PLANO-PADROES.md:53`, `app/docs/PLANO-PADROES.md:58`). Não há novo termo no glossário nem nova lei no diff; o novo universo implementa o aval fornecido e está descrito em `bin/tests/test_lei_akita.sh:51`.

## 31/10 13:34 · sessao 7cfa3b05c897 · GRADE-FOLGA · verificador · origem=Code

A cura liga o `status` à precedência, mas não unifica todo o resultado do calendário. Restam cobertura incompleta do teste e comprovação pendente do pouso v3. (`app/colaboradores/services/calendario.py:803`, `:890`; `app/colaboradores/tests/test_calendario_folga_a_mao_e_da_pessoa.py:55`; `app/ponto/services/cartorio.py:173`.)

1. **Autoridade e caminho restante do calendário.** Sem o argumento, `fatos_do_periodo` carrega as folgas da pessoa, seleciona a válida e escreve `fatos['folga_lancada']`; também decide `faixa` pela precedência. O calendário lê ambos para classificar o dia. (`app/ponto/precedencia.py:281`, `:358`, `:392`, `:408`; `app/colaboradores/services/calendario.py:795`, `:803`.)

   Porém, `eh_trabalho` continua vindo do vínculo **ativo**, alimentado por `_folgas_mes`, e é devolvido sem o ajuste local de `_classificar`. No próprio cenário novo, pode sair `status='folga'` e `eh_trabalho=True`: a célula declara trabalho e o modelo a lê primeiro. (`app/colaboradores/services/calendario.py:772`, `:890`, `:116`; `app/colaboradores/tests/test_calendario_folga_a_mao_e_da_pessoa.py:40`; `app/escala/models.py:1071`.)

   Esse é o caminho restante por foto: sem célula, o fallback considera a data presente na foto como folga, sem distinguir origem. A diferença entre previsão bruta e status final, isoladamente, não demonstra regressão: `resolver_dia_oferecido` também exige um status que peça decisão, excluindo `folga`. (`app/escala/models.py:1073`, `:1252`, `:1280`; `app/colaboradores/services/calendario.py:181`, `:189`.)

   Os contadores acompanham o **status**; badges continuam lendo feriados, rubricas lavradas e chamados. Portanto, não passam todos a consumir `faixa`, nem isso demonstra por si um badge incorreto. (`app/colaboradores/services/calendario.py:937`, `:944`, `:838`, `:845`, `:898`.)

2. **Custo: sem N+1 novo de FolgaDia.** O carregador executa uma consulta antes do laço diário; a seleção acessa campos `_id` já carregados, sem buscar relações. (`app/ponto/precedencia.py:207`, `:281`, `:337`, `:238`.)

   Há duas ressalvas: o legacy recarrega por **segmento**, e o caminho completo já consulta o nome de cada feriado por data. Nenhuma dessas consultas nasce do seletor novo. (`app/escala/utils.py:985`, `:995`, `:770`, `:1119`; `app/ponto/precedencia.py:407`; `app/core/feriados.py:100`.)

3. **O RED tem mordida, mas a cobertura é parcial.** Pelo rastreio do código, a mão presa ao velho muda de `falta` para `folga`; a asserção nova distingue esses resultados. Execução integrada vermelho/verde: **[não verificado]**. (`app/colaboradores/tests/test_calendario_folga_a_mao_e_da_pessoa.py:54`, `:55`; `app/colaboradores/services/calendario.py:116`, `:133`.)

   O controle do calendário velho mede uma exclusão válida, mas fraca: `assertNotEqual(..., 'folga')` também aceita `sem_dados`, `previsto` ou outro status errado, embora a docstring prometa `falta`. Ele já passava com a foto restrita ao ativo. (`app/colaboradores/tests/test_calendario_folga_a_mao_e_da_pessoa.py:10`, `:60`, `:62`; `app/colaboradores/services/calendario.py:593`.)

   **Falta testar o outro defeito citado pela própria cura:** folga de calendário do ativo quando a geradora do dia é outro vínculo. A fixture atual sempre põe o ativo como geradora; também não cobra contadores, badges ou o contrato de `eh_trabalho`. (`app/colaboradores/services/calendario.py:608`; `app/colaboradores/tests/test_calendario_folga_a_mao_e_da_pessoa.py:41`, `:55`, `:62`.)

4. **Triagem da volta 2.** Não encontrei dependência nova que transforme `o_dia_ja_e_folga` ou `_folga_avulsa` em bloqueadores desta lavra: continuam na idempotência/veto da porta; o caminho alterado usa diretamente os helpers novos. (`app/ponto/services/declarar_dia.py:90`; `app/ponto/precedencia.py:501`; `app/escala/utils.py:1115`.)

   A troca de ordem alcança células/órfãs e a leitura da ausência parcial, portanto “nenhum `tipo_dia` mudou” não comprova ausência de efeito nos demais campos. Impacto financeiro adicional ao anexo: **[não verificado]**. (`app/escala/utils.py:1794`, `:1127`, `:1202`.)

5. **Classes estruturais e pouso.** O predicado novo está centralizado e é chamado pela grade e precedência; nesses trechos não aparece novo escritor persistente, relógio ou aquisição de trava. (`app/ponto/precedencia.py:236`, `:358`; `app/escala/utils.py:1115`, `:1124`.) O calendário conserva classificação própria, já reconhecida como migração pendente no registro; não é juiz criado por este diff. (`app/core/juizes.py:169`; `app/colaboradores/services/calendario.py:93`.)

   **Não daria o pouso como comprovado:** a v3 já está declarada, mas exige antes ensaio `--apply` com dias rejulgados, dias que movem minuto e `FORA-DO-HORIZONTE 0`. Esses resultados não constam do material fornecido: **[não verificado]**. Isso é pré-requisito sem comprovação, não prova de que um pouso indevido ocorreu. (`app/ponto/services/cartorio.py:110`, `:173`.)

## 31/10 13:33 · sessao 11088239546e · CLAUDE-MD-RACIONAL · leitura · origem=Code

1. **Não no histórico fornecido.** No commit **`8ce1e8c2`**, o texto removido de `CLAUDE.md` e adicionado a `app/docs/PISTA.md` passa diretamente de (4) para (6), literalmente:

   > escopo, mesmo fora do diff. (6) ALERTA no ESTADO

   Referências: `hist_triagem.txt:148` e `hist_triagem.txt:347`.

2. **Sim, no aval de 30/10 10:3x em `PROMPTS.md`**, consta literalmente:

   > (5) retroativo: NOTURNO (a) 1,5 / (b) 2,3 / (b) parcial 4,6; V-FASE refeita pelo item (2)

   Referência: [app/docs/PROMPTS.md:1671](/home/ronald/codex_ws/a7b66d14d0bcf92e6f71f940cef6b3fdf6063dd7/app/docs/PROMPTS.md:1671). Esse item não aparece nos blocos dos dois arquivos exibidos no commit `8ce1e8c2` (`hist_triagem.txt:137–149` e `hist_triagem.txt:336–348`). **Commit que registrou esse aval em `PROMPTS.md`: [nao verificado]**; o anexo não o identifica.

## 31/10 13:32 · sessao 6e629cbc1fa1 · - · - · origem=Ronald (tmux)

Não encontrei bloqueador novo no seletor ou nos builders, nestes pontos:

- **Carga de folgas sem N+1:** uma consulta traz todas as `FolgaDia` da pessoa na janela; o laço usa apenas campos já carregados (`app/ponto/precedencia.py:207-209`). Seletor e predicado são puros e acessam `calendario_origem_id`/`escala_colaborador_id`, sem acessar relações (`app/ponto/precedencia.py:213-240`). A precedência chama o carregador antes do laço diário (`app/ponto/precedencia.py:281-282,337-358`).
- **Nuance:** o caminho completo ainda consulta nomes de feriado por data: `app/ponto/precedencia.py:407` chama `nome_do_feriado`, que executa `_qs(data,data)` (`app/core/feriados.py:98-102,27-29`). É anterior ao diff fornecido; não decorre da carga de `FolgaDia`.
- **Builders:** ambos chamam o mesmo carregador e seletor (`app/escala/utils.py:770,1115-1128,1794`). No legacy, a carga acontece por segmento, pois cada segmento chama o builder individual (`app/escala/utils.py:985-995`); portanto não prometer “uma única consulta por calendário inteiro” para esse builder.
- **Autoridade:** o vínculo do dia continua vindo do juiz registrado (`app/core/juizes.py:1255`; `app/escala/utils.py:969-973,1256-1268`). A precedência consulta esse mesmo juiz (`app/ponto/precedencia.py:351`) e ambos usam o mesmo predicado de elegibilidade da folga (`app/ponto/precedencia.py:358`; `app/escala/utils.py:1124`). Não identifiquei segundo juiz introduzido nesses trechos.
- **Contrato antigo de `folgas_lancadas`:** não há outro chamador no código passando o argumento; a única ocorrência restante é a assinatura em `app/ponto/precedencia.py:261`. A mudança de valor unitário para lista não deixa chamador antigo demonstrável na cópia.
- **Fila da volta 2:** `o_dia_ja_e_folga` responde à idempotência da porta, escolhendo o vínculo e consultando sua foto (`app/ponto/services/declarar_dia.py:73-91`). `_folga_avulsa` alimenta veto de lançamento (`app/ponto/precedencia.py:161-166,501-509`). A lavra alterada não chama nenhum deles: usa diretamente os novos helpers (`app/ponto/precedencia.py:281-282,358`; `app/escala/utils.py:1115-1128`). Não encontrei dependência nova que torne esses achados bloqueadores desta cura.
- **Ordem:** folga agora precede todos os overlays de ausência no builder por turno (`app/escala/utils.py:1794-1797`). Ausência continua cobrindo folga e falta continua sem substituí-la (`app/escala/utils.py:1174-1178`). Há consequência possível sobre dados auxiliares: folga limpa células e órfãs (`app/escala/utils.py:1126-1128`), e ausência parcial mede seu período usando essas células (`app/escala/utils.py:1202`). O anexo já declara diferença não-tipo medida; outras vítimas ou efeito financeiro são [não verificado].
- **Classes estruturais:** os novos helpers e overlay não escrevem banco nem leem relógio (`app/ponto/precedencia.py:195-240`; `app/escala/utils.py:1103-1128`). Não introduzem ordem de trava nesses trechos. A repetição das leituras de relógio dos builders individuais é preexistente e recebe `ref` explícito no caminho por segmentos (`app/escala/utils.py:658,964,995`), enquanto o builder por turno deriva a data de `agora` recebido (`app/escala/utils.py:1242`).

Execução real, números de consultas em runtime e impacto além do dado anexado: [não verificado].

_1071 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
