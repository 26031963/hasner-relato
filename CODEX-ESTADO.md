# CODEX-ESTADO

> Ultima resposta processada: **09/11/2026 02:49** · publicado em **09/11/2026 02:51** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 09/11 02:49 · sessao 07ae55071934 · E1-FILA-V2-2-TRAVA · construtor · origem=Code

Implementei A1 com **captura única fora do `atomic`**: o agrupador registra `repasse_recusado` e devolve `REPASSE_RECUSADO`, distinto de `None`. O savepoint permite preservar o ato externo; a verificação PostgreSQL dessa preservação está **[nao verificado]**. [disputa_emissao.py:1048](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/services/disputa_emissao.py:1048), [captura:1290](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/services/disputa_emissao.py:1290)

Quem escreve o evento/retorno é o agrupador, via `core.observ.evento`. Quem lê o retorno são `acao_iniciar_disputa_orfaos` e `cobrar_dia`, com mensagens próprias; o evento é consultado pelo RED SQL. Leitura operacional da trilha: **[nao verificado]**. [ação:225](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/services/acoes_disputa.py:225), [cobrar_dia:50](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/services/cobrar_dia.py:50), [RED SQL:262](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/tests/test_e1_fila_v2_2_integracao.py:262)

Também alterei:

- **S1:** `_caso_b` usa destino independente para a P materializada, preservando `ch2` sem P e o assert da cauda. [integração:118](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/tests/test_e1_fila_v2_2_integracao.py:118)
- **S2:** os três closures capturam as variáveis por defaults. [trava:254](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/tests/test_e1_fila_v2_2_trava.py:254)
- **S3:** o registro acompanha a linha `1196`. [catraca:268](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/core/tests/test_medida_turno_do_par.py:268)

Os REDs de A1 exercitam agrupador e telas; os mutantes retiram captura, evento, retorno distinguível e leitores. Passaram **23 testes sem banco**. A catraca passou **35 testes**; reaplicar `[1131]` em memória fez seu RED falhar. Os sete Python tocados passaram por `py_compile` em memória, sem gravar bytecode. [resultados e comandos:4](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/tests/e1_fila_v2_2_verificacao.txt:4) [nao verificado]

**A2:** acrescentei agrupador real com P estrangeira e detector exigindo C estrangeiro antes da primeira D. O mutante insere a lambda antes do `with` e exige falha nesse assert. Consegui compilá-lo; **não consegui executar o caso PostgreSQL [nao verificado]**. [integração:190](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/tests/test_e1_fila_v2_2_integracao.py:190)

A relação completa `arquivo::funcao` e o comando de cada RED/mutante estão no [registro de entrega:137](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/tests/e1_fila_v2_2_verificacao.txt:137). PostgreSQL A1/A2/S1, Ruff e suíte inteira permanecem **[nao verificado]**; certificação reservada ao Code e opus final. [nao verificado]

## 09/11 02:40 · sessao 26af97663c42 · - · - · origem=Ronald (tmux)

S2: capturar por defaults as variáveis dos laços:

- `crescer(*args, original=original, lidas=lidas, classe=classe, pk=pk)` nos dois pontos ([test_e1_fila_v2_2_trava.py:254](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/tests/test_e1_fila_v2_2_trava.py:254), [mesmo arquivo:367](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/tests/test_e1_fila_v2_2_trava.py:367)).
- `lambda c, o, m=m: _fecho_antigo(m, c, o)` ([mesmo arquivo:270](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/tests/test_e1_fila_v2_2_trava.py:270)).

S3: atualizar o registro da linha B1 do agrupador após a cura A1; hoje `[1131]`, enquanto o censo atual calcula `[1188]` ([test_medida_turno_do_par.py:268](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/core/tests/test_medida_turno_do_par.py:268)). A regra exige registro no mesmo diff e compara sem consultar Git ([mesmo arquivo:17](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/core/tests/test_medida_turno_do_par.py:17), [mesmo arquivo:533](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/core/tests/test_medida_turno_do_par.py:533)). Preserve a identidade da função: extrair o corpo para outra função exigiria também rever `REGISTRO` ([mesmo arquivo:526](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/core/tests/test_medida_turno_do_par.py:526)).

Comandos executados, sem editar:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B -m unittest chamados.tests.test_e1_fila_v2_2_trava
```

Resultado baseline: 15 testes OK [nao verificado por arquivo persistido]. [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B - <<'PY'
from core.medida_turno_do_par import censo
c = censo('.')
print([(i['chave'], i['linhas'])
       for i in c['derivacoes_de_dia']['itens']
       if 'disputa_emissao.py' in i['chave']])
PY
```

O ambiente tem `python3`, sem Django ou Ruff; execução desses verificadores reais fica [nao verificado]. O runner da catraca sem Django está documentado em [test_medida_turno_do_par.py:20](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/core/tests/test_medida_turno_do_par.py:20).

## 09/11 02:40 · sessao 75dba09147d1 · - · - · origem=Ronald (tmux)

S1: a fixture ocupa `self.ch2` com P materializada ([integracao.py:118](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/tests/test_e1_fila_v2_2_integracao.py:118)), embora ele exista para atravessar o filtro como segundo órfão ([test_e1_fila.py:260](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/tests/test_e1_fila.py:260)). A regra pode excluí-lo por P materializada sem célula acusando ([disputa_emissao.py:759](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/services/disputa_emissao.py:759)); então o agrupador retorna antes da cauda ([disputa_emissao.py:1082](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/services/disputa_emissao.py:1082)). Cura sugerida: criar chamado independente como destino da P materializada e ressuscitá-lo na cauda, preservando `self.ch2` sem P; execução PostgreSQL [nao verificado].

A2: acrescentar teste que execute o agrupador real com D/P estrangeiras apontando o chamado local, sem wrapper da helper e sem cauda artificial; inspecionar `linhas_travadas` e exigir C estrangeiro por `for_update` antes da primeira D. O detector distingue aquisição e atualização ([test_e1_v9.py:71](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/tests/test_e1_v9.py:71)). O fecho encontra o estrangeiro por P reversa ao destino ([disputa_emissao.py:965](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/services/disputa_emissao.py:965)). [nao verificado]

MORDE sugerido: gerar função mutante usando `fila.mutante_ou_falha` ([test_e1_fila.py:48](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/tests/test_e1_fila.py:48)), inserir `_trava_do_repasse = lambda *a, **k: None` imediatamente antes do `with` e reutilizar o teste, exigindo falha no assert do C estrangeiro. Sem execução [nao verificado]. [nao verificado]

## 09/11 02:24 · sessao 69b9fc6d95cb · SELOS-CANON-I1 · verificador · origem=Code

**Parecer: o diff realiza parte do I-1, mas não sustenta aprovação integral.** Há escapes de abertura, a negativa não escreve o PAREI exigido e as medidas contam decisões intermediárias como abertura/ociosidade. (`bin/disjuntor.py:1332`, `bin/disjuntor.py:1521`, `bin/disjuntor.py:1588`, `bin/disjuntor.py:1607`, `bin/disjuntor.py:1777`)

As referências abaixo são da cópia. Identidade Git com o hash/base informados e resultados em produção: **[nao verificado]**. A execução integral dos selos também fica **[nao verificado]**; seus scripts criam arquivos e cópias. (`bin/tests/test_censo_antes_da_vaga.sh:21`, `bin/tests/test_censo_antes_da_vaga.sh:160`)

1. **O diff realiza o aval literal? Parcialmente.**

   Na tabela, “aval I-1” refere-se ao parágrafo fornecido no anexo `aval_selos_do_canon_0811.txt`; a numeração do arquivo original é **[nao verificado]**.

   | Exigência do aval I-1 | Realização ou falta |
   |---|---|
   | Interceptar abertura de construtor ESTRUTURA/fila 1 | Há chamadas no ramo Agent e no ramo Bash/Codex, condicionadas a reconhecedores textuais. A cobertura não alcança todas as chamadas aceitas pelo wrapper; detalhes na resposta 3. (`bin/disjuntor.py:1583`, `bin/disjuntor.py:1603`) |
   | Ler `logs/<obra>/brief*.md` | Lê os caminhos **citados na abertura**, com um nível de diretório sob `RAIZ/logs`. Não descobre automaticamente o brief da obra nem verifica que o diretório/conteúdo corresponde à obra declarada. (`bin/disjuntor.py:1413`, `bin/disjuntor.py:1475`, `bin/disjuntor.py:1513`) |
   | CENSO com leitores/escritores por AST e arquivo:linha | Verifica título, campos e existência de **alguma referência por regex em cada categoria**. Não verifica AST, existência dos sítios, completude nem referência em cada item. Isso não exige necessariamente executar AST na abertura, mas o check não comprova a procedência exigida. (`bin/disjuntor.py:1409`, `bin/disjuntor.py:1454`, `bin/disjuntor.py:1460`) |
   | DECISOES listadas | Exige algum conteúdo não vazio em `decisoes`; aceita também texto na própria linha, sem lista. (`bin/disjuntor.py:1452`, `bin/disjuntor.py:1463`) |
   | Linha literal `perguntas de lei: N` | Implementada, com recusa de repetição e conferência da quantidade de itens reconhecidos abaixo. (`bin/disjuntor.py:1408`, `bin/disjuntor.py:1445`, `bin/disjuntor.py:1465`) |
   | N>0: corte em cada pergunta | Percorre cada pergunta reconhecida e exige a substring `cortada por Ronald` seguida de token. Não verifica autenticidade do aval nem negação textual: `NAO cortada por Ronald X` satisfaz a regex. (`bin/disjuntor.py:1411`, `bin/disjuntor.py:1469`) |
   | Falha = rc 1; sessão só abre com rc 0 | A CLI `--censo` retorna 0/1. A abertura interceptada usa JSON `deny`, com retorno zero do hook; não há chamada obrigatória da CLI pelo wrapper. Há equivalência de bloqueio nas chamadas reconhecidas, mas não implementação literal do protocolo rc na abertura. (`bin/disjuntor.py:1532`, `bin/disjuntor.py:1654`, `bin/disjuntor.py:1905`, `bin/codex_pergunta.sh:394`) |
   | Linha PAREI no RELATO | **Falta escrita automática:** a negativa manda a sessão escrevê-la. A escrita automática existente produz outra linha, `(NEGOU CENSO)`, na seção de alarmes. (`bin/disjuntor.py:1521`, `bin/disjuntor.py:221`, `bin/disjuntor.py:236`) |
   | PAREI censo vale como ESPERANDO PROVADO | Implementado com prova adicional de wrapper vivo, obra e papel, compartilhando o caminho de espera do Stop e do orçamento. (`bin/hook_stop_fila1.py:703`, `bin/hook_stop_fila1.py:740`, `bin/hook_stop_fila1.py:1040`, `bin/hook_stop_fila1.py:1283`) |
   | Duas medidas no ESTADO | Os campos chegam ao gerador, mas seus significados não comprovam abertura efetiva nem vaga ociosa; resposta 8. (`bin/disjuntor.py:1777`, `bin/disjuntor.py:1817`, `bin/gerar_estado.py:280`) |
   | Allowlist vazia; desligamento só Ronald | Não aparece allowlist específica no novo check. Continua existindo a chave global, com proteção contra sua manipulação pelo Code. Ausência de obra reconhecida ou BACKLOG, porém, libera sem constituir allowlist explícita. (`bin/disjuntor.py:151`, `bin/disjuntor.py:156`, `bin/disjuntor.py:1496`, `bin/disjuntor.py:1507`) |
   | Selo com mutante de linha ausente | Presente e com asserção que o atinge; cobertura adicional tem lacunas, resposta 7. (`bin/tests/test_censo_antes_da_vaga.sh:70`, `bin/tests/test_censo_antes_da_vaga.sh:175`) |

   Pouso próprio, código somente depois da E1 e cumprimento da ordem I-1/I-3/I-2: **[nao verificado]**. O registro disponível ainda descreve a condição “código só depois do pouso da E1”. (`app/docs/PROMPTS.md:1983`, `app/docs/BACKLOG.md:728`)

2. **Há outro juiz de “brief tem censo”?**

   Nos caminhos examinados, há **um reconhecedor do conteúdo**: `censo_do_brief`, chamado tanto por `_censo_nega` quanto por `--censo`. `_prova_censo` decide existência de produtor vivo; `gerar_estado` apresenta medidas; o handoff consulta fila/PAREI. Eles não repetem a validação do bloco. (`bin/disjuntor.py:1417`, `bin/disjuntor.py:1514`, `bin/disjuntor.py:1906`, `bin/hook_stop_fila1.py:649`, `bin/gerar_estado.py:280`, `bin/handoff_sessao.sh:120`)

   Não localizei autoridade anterior dessa pergunta no registro ou no lookup examinados. `JUIZES.md` declara que é consulta e aponta `app/core/juizes.py` como registro vivo; a existência de equivalente não identificado fica **[nao verificado]**. (`app/docs/JUIZES.md:7`, `app/core/juizes.py:170`, `app/core/juizes.py:706`, `app/core/juizes.py:1025`, `app/core/juizes.py:1173`, `app/core/juizes.py:1285`)

   Portanto, há cobertura incompleta da porta; não há evidência suficiente para acusar **segundo juiz que duplica juiz registrado**. (`bin/disjuntor.py:1514`, `bin/disjuntor.py:1906`; autoridade anterior equivalente: **[nao verificado]**)

3. **Formas de abrir construtor e escapes.**

   | Forma demonstrável | Trava e classificação |
   |---|---|
   | Agent fora de `SO_LEITURA`, com caminho absoluto `wt-*` e obra reconhecida/andável | Passa pela trava. (`bin/disjuntor.py:98`, `bin/disjuntor.py:100`, `bin/disjuntor.py:1603`) |
   | Mesmo Agent, mas sem âncora reconhecível de obra — por exemplo, `Construa OBRA-E3...` | Obra pode virar `None`, que libera. **Escape real se continua sendo construtor estrutural da fila 1.** (`bin/disjuntor.py:111`, `bin/disjuntor.py:511`, `bin/disjuntor.py:1507`) |
   | Agent com cópia sem nome `wt-*`, caminho relativo ou cópia não citada | Não entra no ramo. **Escape real quando o trabalho pertence à classe do aval**; ausência desse padrão textual não prova ausência de construção. (`bin/disjuntor.py:100`, `bin/disjuntor.py:1603`) |
   | Bash com atribuições imediatamente antes do wrapper: `CODEX_OBRA=X CODEX_PAPEL=construtor bin/codex_pergunta.sh --construtor ...` | Passa pela trava. (`bin/disjuntor.py:1332`, `bin/disjuntor.py:1583`) |
   | `export CODEX_OBRA=X CODEX_PAPEL=construtor; bin/codex_pergunta.sh --construtor ...` | Hook lê papel `?` e obra vazia; wrapper aceita o ambiente herdado. **Escape real na própria via autorizada.** (`bin/disjuntor.py:1333`, `bin/disjuntor.py:1343`, `bin/disjuntor.py:1584`, `bin/codex_pergunta.sh:81`) |
   | `env CODEX_OBRA=X CODEX_PAPEL=construtor bin/codex_pergunta.sh ...` | A regex admite prefixos, mas não essas atribuições depois de `env`. **Escape real.** (`bin/disjuntor.py:1250`, `bin/disjuntor.py:1338`, `bin/codex_pergunta.sh:81`) |
   | Wrapper dentro de `bash -c`, script externo ou encapsulamentos residuais | Limitação expressamente declarada; o wrapper não impõe CENSO na sua entrada. **Escape real se abre construtor abrangido.** (`bin/disjuntor.py:1247`, `bin/codex_pergunta.sh:233`, `bin/codex_pergunta.sh:394`) |
   | Wrapper executado fora do PreToolUse | Não tem guarda própria de CENSO; toma vaga e executa o construtor. Uso concreto dessa via em produção: **[nao verificado]**. (`bin/codex_pergunta.sh:233`, `bin/codex_pergunta.sh:370`, `bin/codex_pergunta.sh:394`) |
   | `codex exec` direto | Escape técnico declarado, mas via proibida pela regra do wrapper; não demonstra uma segunda abertura autorizada. (`bin/disjuntor.py:1248`, `CLAUDE.md:347`) |
   | `bin/fabricante.sh`, que inicia `claude -p` | A abertura da sessão não é Agent e não recebe essa trava. Enquadramento de cada fabricação como obra fila 1 do aval: **[nao verificado]**. (`bin/fabricante.sh:2`, `bin/fabricante.sh:150`, `bin/fabricante.sh:162`) |
   | Leitura/censo/verificador legítimos, obra fechada ou fila 2 | Fora da classe construtor estrutural fila 1. (`bin/disjuntor.py:1507`, `bin/tests/test_censo_antes_da_vaga.sh:123`, `bin/tests/test_censo_antes_da_vaga.sh:125`, `bin/tests/test_censo_antes_da_vaga.sh:127`, `bin/tests/test_censo_antes_da_vaga.sh:135`) |

   Outras formas não localizadas: **[nao verificado]**. A configuração fornecida chama um caminho absoluto da árvore viva; instalação deste diff naquele caminho também é **[nao verificado]**. (`.claude/settings.json:68`)

4. **Fail-open contradiz lei ou contrato?**

   **O contrato existente declara fail-open explicitamente:** “toda excecao vira linha FALHA na trilha e a ferramenta passa”. O tratamento externo preserva isso, com exceção expressa do B3. O novo CENSO herda a política e retorna `None` após `ABRIU-SEM`. (`bin/disjuntor.py:34`, `bin/disjuntor.py:1643`, `bin/disjuntor.py:1645`, `bin/disjuntor.py:1525`)

   Logo, não contradiz essa cláusula do disjuntor. Isso, porém, não demonstra autorização para excepcionar o aval específico de abertura somente com censo fechado; essa compatibilidade permanece **[nao verificado]**. (`bin/disjuntor.py:44`, `bin/disjuntor.py:1504`, `bin/disjuntor.py:1525`)

   LEI-AKITA 1 não permite concluir que qualquer tratamento de exceção seja automaticamente “fallback”: seu texto associa o termo à sobrevivência de derivador paralelo. Existe tensão mais direta com “exceção [...] nunca destravando a trava” e com a literalidade do aval. (`CLAUDE.md:17`, `CLAUDE.md:19`, `CLAUDE.md:32`, `CLAUDE.md:34`, `CLAUDE.md:37`)

   A trilha também não é garantida: `trilha` engole falhas de escrita. Assim, uma liberação excepcional pode ficar sem `ABRIU-SEM` observável. (`bin/disjuntor.py:142`, `bin/disjuntor.py:1526`)

5. **“Fila 1” é definição paralela?**

   **Nesta costura, não.** `_fila1_viva` chama diretamente `_itens_andaveis`, que aplica `_item_parado`; o hook usa esse mesmo predicado para decidir quais linhas andam. (`bin/disjuntor.py:1489`, `bin/disjuntor.py:1498`, `bin/hook_stop_fila1.py:158`, `bin/hook_stop_fila1.py:212`, `bin/hook_stop_fila1.py:218`)

   “Todos os andáveis” e “item em curso” são perguntas diferentes: o conjunto vem de OBRAS; o item em curso vem de `ORDEM-VIVA-TOPO`. Não selecionar apenas o item do marcador não constitui divergência de fila. (`bin/hook_stop_fila1.py:179`, `bin/hook_stop_fila1.py:218`, `bin/hook_stop_fila1.py:243`, `bin/hook_stop_fila1.py:1292`)

   B3 decide parada/liberação por reprovações e triagens da obra; não é autoridade de pertencimento à fila 1. (`bin/disjuntor.py:903`, `bin/disjuntor.py:909`, `bin/disjuntor.py:993`)

   Há outro defeito: BACKLOG ausente vira conjunto vazio, liberando sem `ABRIU-SEM`. É falha de fonte interpretada como “fora do universo”, não derivação paralela. (`bin/disjuntor.py:1493`, `bin/disjuntor.py:1496`, `bin/disjuntor.py:1507`)

6. **A prova de ESPERANDO é a mesma? Quais mutantes passam?**

   A vivacidade usa a autoridade existente `_prova_pid`: identidade de executável/script, ancestralidade, estabilidade do processo e rejeição de Z/X. A nova função a chama, em vez de copiar esses testes. (`bin/hook_stop_fila1.py:523`, `bin/hook_stop_fila1.py:529`, `bin/hook_stop_fila1.py:535`, `bin/hook_stop_fila1.py:546`, `bin/hook_stop_fila1.py:682`)

   O predicado completo é diferente: CENSO descobre o wrapper por `/proc` e exige raiz, UID, obra exata e papel censo/leitura; ESPERANDO genérico recebe produtor e PID/arquivo declarados. É uma composição específica nova, não uma segunda implementação da vivacidade. Ambos compartilham decisão, memoização, orçamento e liberação final. (`bin/hook_stop_fila1.py:661`, `bin/hook_stop_fila1.py:671`, `bin/hook_stop_fila1.py:682`, `bin/hook_stop_fila1.py:715`, `bin/hook_stop_fila1.py:740`, `bin/hook_stop_fila1.py:1040`, `bin/hook_stop_fila1.py:1283`)

   Os exemplos “aceitar construtor”, “casar prefixo” e “aceitar ausência após morte” têm casos negativos específicos. Não cabe declará-los sobreviventes por simples leitura. (`bin/tests/test_hook_esperando_com_prova.sh:206`, `bin/tests/test_hook_esperando_com_prova.sh:209`, `bin/tests/test_hook_esperando_com_prova.sh:214`)

   Remover a guarda de UID ou a comparação adicional de starttime são candidatos sem caso dedicado no bloco novo; sobrevivência executada desses mutantes: **[nao verificado]**. O teste de morte faz `wait`, não simula reciclagem do PID. (`bin/hook_stop_fila1.py:671`, `bin/hook_stop_fila1.py:685`, `bin/tests/test_hook_esperando_com_prova.sh:31`, `bin/tests/test_hook_esperando_com_prova.sh:194`)

7. **Os quatro mutantes mordem? Há sobreviventes?**

   **Os quatro têm mordida nas asserções:**

   | Mutante | Caso que o atinge |
   |---|---|
   | Linha ausente vale zero | Exige rc 1 sem a linha. (`bin/tests/test_censo_antes_da_vaga.sh:70`, `bin/tests/test_censo_antes_da_vaga.sh:175`) |
   | Não exigir corte | Casos retiram o corte da última pergunta. (`bin/tests/test_censo_antes_da_vaga.sh:71`, `bin/tests/test_censo_antes_da_vaga.sh:72`, `bin/tests/test_censo_antes_da_vaga.sh:176`) |
   | Agent sem porta | Negativos exigem `deny`, motivo CENSO e PAREI específico. (`bin/tests/test_censo_antes_da_vaga.sh:103`, `bin/tests/test_censo_antes_da_vaga.sh:112`, `bin/tests/test_censo_antes_da_vaga.sh:177`) |
   | Codex sem porta | A mesma exigência no construtor sem brief. (`bin/tests/test_censo_antes_da_vaga.sh:103`, `bin/tests/test_censo_antes_da_vaga.sh:131`, `bin/tests/test_censo_antes_da_vaga.sh:178`) |

   A harness verifica âncora única e exige que a função de casos falhe após a mutação. Resultado da rodada integral: **[nao verificado]**. (`bin/tests/test_censo_antes_da_vaga.sh:165`, `bin/tests/test_censo_antes_da_vaga.sh:170`, `bin/tests/test_censo_antes_da_vaga.sh:173`)

   **Sobrevivente concreto por análise e reprodução em memória:** trocar `for q in perguntas:` por `for q in perguntas[-1:]:`. Todos os negativos de corte alteram a última pergunta; a primeira poderia ficar sem corte. A costura não acrescenta caso com N>0. (`bin/disjuntor.py:1469`, `bin/tests/test_censo_antes_da_vaga.sh:71`, `bin/tests/test_censo_antes_da_vaga.sh:72`, `bin/tests/test_censo_antes_da_vaga.sh:99`, `bin/tests/test_censo_antes_da_vaga.sh:100`)

   Outro candidato em `_censo_nega` é retirar a trilha da exceção: não há cenário que provoque essa exceção e confira `ABRIU-SEM`. A medida só exige rótulos e número decimal, sem duração ou contagem esperada. (`bin/disjuntor.py:1525`, `bin/tests/test_censo_antes_da_vaga.sh:141`, `bin/tests/test_censo_antes_da_vaga.sh:147`)

   A asserção positiva também é insuficiente: `_passa_censo` aceita qualquer saída sem “CENSO”, inclusive negativa B4 ou saída vazia; o status de `_pre` não é conferido. (`bin/tests/test_censo_antes_da_vaga.sh:97`, `bin/tests/test_censo_antes_da_vaga.sh:106`)

8. **Quem escreve, quem lê e onde muda o significado?**

   | Objeto | Escritor | Leitor |
   |---|---|---|
   | Brief: CENSO, leitores, escritores, decisões e perguntas | Escritor real/procedência AST: **[nao verificado]** | `censo_do_brief`. (`bin/disjuntor.py:1420`, `bin/disjuntor.py:1438`) |
   | BACKLOG/OBRAS usado no universo | Escritor real desta evidência: **[nao verificado]** | `_fila1_viva` → `_itens_andaveis`. (`bin/disjuntor.py:1494`, `bin/disjuntor.py:1498`) |
   | Ambiente `CODEX_OBRA`/`CODEX_PAPEL` do produtor | Fornecido pelo lançador; o wrapper o recebe. (`bin/codex_pergunta.sh:81`) | `_prova_censo` lê `/proc/<pid>/environ`. (`bin/hook_stop_fila1.py:678`, `bin/hook_stop_fila1.py:682`) |
   | `disjuntor.log` | Porta `trilha`, com carimbo e detalhe limitado a 300 caracteres. (`bin/disjuntor.py:135`, `bin/disjuntor.py:140`) | Leitor geral e `_censo_24h`; hook o proíbe como arquivo de prova de progresso próprio. (`bin/disjuntor.py:1717`, `bin/disjuntor.py:1784`, `bin/hook_stop_fila1.py:585`) |
   | `CENSO OK`, com obra/via/brief | `_censo_nega`. (`bin/disjuntor.py:1518`) | Conta `ok` e encerra intervalo da obra. (`bin/disjuntor.py:1797`) |
   | `CENSO ABRIU-SEM`, com obra/via/falha | Exceção de `_censo_nega`. (`bin/disjuntor.py:1525`) | Conta `abertos_sem`. (`bin/disjuntor.py:1801`) |
   | `NEGOU DISJUNTOR CENSO...` | `main_pretool`, antes do JSON deny. (`bin/disjuntor.py:1636`, `bin/disjuntor.py:1639`) | Inicia intervalo e conta `negados`. (`bin/disjuntor.py:1794`) |
   | `abertos_sem`, `negados`, `ok`, `ociosa_h`, `esperando` | `_censo_24h`, em memória. (`bin/disjuntor.py:1807`) | `_censo_txt` → `linha` → `gerar_estado`. (`bin/disjuntor.py:1812`, `bin/disjuntor.py:1895`, `bin/gerar_estado.py:280`) |
   | ESTADO publicado | Gerador grava destino; `relato.sh` entrega o temporário à raspagem. (`bin/gerar_estado.py:390`, `bin/relato.sh:38`, `bin/relato.sh:105`) | Consumo posterior efetivo: **[nao verificado]** |
   | PAREI censo no topo | O diff instrui a sessão; escritor concreto: **[nao verificado]**. (`bin/disjuntor.py:1521`, `bin/disjuntor.py:1908`) | Stop, handoff e alarme, com sentidos divergentes abaixo. (`bin/hook_stop_fila1.py:703`, `bin/handoff_sessao.sh:108`, `bin/alarme_sessao_ociosa.py:127`) |
   | Linha automática `(NEGOU CENSO)` no RELATO | `relato_negacao` pela porta `esteira_vigia.no_relato`, preservando o topo. (`bin/disjuntor.py:221`, `bin/disjuntor.py:236`, `app/core/esteira_vigia.py:829`) | Não substitui a linha literal consumida pelo reconhecedor de espera. (`bin/hook_stop_fila1.py:705`) |

   **ESMERIL-SEMANTICA presente nas medidas:** `OK` e `ABRIU-SEM` acontecem antes de B4/`codex_decide` e das validações do wrapper. Provam resultado do check, não abertura efetiva. (`bin/disjuntor.py:1518`, `bin/disjuntor.py:1526`, `bin/disjuntor.py:1588`, `bin/disjuntor.py:1607`, `bin/codex_pergunta.sh:387`)

   `ociosa_h` soma intervalos por obra entre NEGOU e OK/agora; não consulta vaga. Pode somar esperas simultâneas e encerrar em OK seguido de outra negativa. Correspondência com “horas de vaga ociosa”: **[nao verificado]**. Trilha ausente vira zero, embora falha de escrita seja silenciada. (`bin/disjuntor.py:1795`, `bin/disjuntor.py:1800`, `bin/disjuntor.py:1803`, `bin/disjuntor.py:1807`, `bin/disjuntor.py:142`)

   **Divergência no PAREI:** com produtor vivo, Stop libera por espera provada; handoff consulta somente PAREI genérico, que exige `!`, e pode informar “não devolve”. Com produtor morto, o alarme ainda desarma pela substring PAREI, enquanto Stop acusa falta de prova. (`bin/hook_stop_fila1.py:1171`, `bin/hook_stop_fila1.py:1283`, `bin/handoff_sessao.sh:120`, `bin/handoff_sessao.sh:123`, `bin/alarme_sessao_ociosa.py:42`, `bin/alarme_sessao_ociosa.py:127`)

9. **Classes de reprovação presentes.**

   | Classe | Conclusão |
   |---|---|
   | Segundo juiz duplicando registrado | **Não demonstrado** para conteúdo do brief; as duas entradas chamam a mesma função. (`bin/disjuntor.py:1514`, `bin/disjuntor.py:1906`) |
   | Segundo escritor fora da porta | **Não demonstrado** nas novas escritas: trilha e linha automática reutilizam portas existentes. O PAREI obrigatório, contudo, não é escrito pelo diff. (`bin/disjuntor.py:1518`, `bin/disjuntor.py:1639`, `bin/disjuntor.py:236`, `bin/disjuntor.py:1521`) |
   | Relógio novo fora da borda | **Não demonstrado**: `_censo_24h` recebe `agora`; o carimbo próprio pertence ao escritor já existente. (`bin/disjuntor.py:140`, `bin/disjuntor.py:1774`, `bin/disjuntor.py:1903`) |
   | Derivação paralela da fila | **Não**, nesta costura: chama o juiz do hook. (`bin/disjuntor.py:1498`) |
   | Selo/RED sem mordida | Os quatro declarados têm mordida; há sobrevivente de “cada pergunta”, positivos fracos e medida sem valor esperado. RED integral evidenciado: **[n

[truncado em 20000 de 20824 caracteres]

## 09/11 02:23 · sessao 4474b71ff4e2 · DEPLOY-EXCLUSAO-ENVELOPE · leitura · origem=Code

O código confirma a janela entre consultar a marca e reiniciar; ele próprio a nomeia como residual (ii). **A exclusão proposta ainda não está implementada no envelope desta cópia**, que executa o comando diretamente (`bin/deploy.sh:307-314`; `bin/cron_run.sh:9-13`). Estado efetivo de produção, crontab instalado e timers ativos: **[nao verificado]**.

**1. Quem lê e escreve `logs/crons_em_curso/`**

| Componente | Comportamento comprovado |
|---|---|
| `cron_run.sh` | Cria uma marca por corrida, com PID na extensão; remove no `EXIT`. A marca cobre comando, gravação do placar, carimbo de fim e tentativa de aviso ao vigia (`bin/cron_run.sh:9-37`). |
| `deploy.sh` | Lê as marcas, verifica `/proc/PID`, apaga as consideradas órfãs e recusa `--restart`. Consulta antes dos demais portões e novamente imediatamente antes do restart (`bin/deploy.sh:108-117`; `bin/deploy.sh:338-342`). |
| `contrato_deploy.sh` | Inspeciona o **texto dos scripts**, exigindo referência à marca; não consulta o diretório operacional (`bin/contrato_deploy.sh:79-82`). |
| Testes | Criam marcas em ambientes falsos e exigem recusa baseada nelas (`bin/tests/test_deploy_reload_gracioso.sh:91`; `bin/tests/test_deploy_reload_gracioso.sh:125-131`; `bin/tests/test_deploy_portoes_antes_do_migrate.sh:192-198`). |

Não localizei outro acesso operacional ao diretório em hook, vigias ou `gerar_estado`; acessos indiretos ou externos à cópia: **[nao verificado]**. O hook possui uma guarda diferente, baseada em `logs/sombra.lock` (`bin/hook_stop_fila1.py:785-794`).

Há um consumidor escrito do **significado** da marca: esperar o término da sombra pela ausência de `sombra.sh_-.*` (`CLAUDE.md:92`; `app/docs/INFRA.md:59-60`). O contrato não determina quando criar a marca em relação à aquisição do lock: se criada antes, também representará espera; se depois, não mostrará quem aguarda. Preservação desse significado na implementação: **[nao verificado]**.

Há ainda registro histórico de um gate que escrevia marca própria durante um apply, fora do envelope. O relato cita `logs/gate10/gate10_condicao1.sh`; presença e uso atual desse escritor, assim como sua futura adesão ao lock: **[nao verificado]** (`app/docs/RELATO.md:4472-4489`).

**2. Reiniciar ou recriar cascas: cobertura e escapes**

| Via | Resultado da leitura |
|---|---|
| `deploy.sh --restart "motivo"` | É o ramo que o contrato propõe proteger. Hoje exige motivo e chama `docker restart` das três cascas após consultar marcas (`bin/deploy.sh:72-75`; `bin/deploy.sh:338-342`). Implementação do exclusivo: **[nao verificado]**. |
| Normal, `--sem-migrate`, `--sem-sombra`, `--reload-copia` | Usam HUP; não têm fallback para restart. O código explica que o HUP vai ao master e preserva o comando executado por `docker exec` (`bin/deploy.sh:322-359`). |
| Reload das 03:30 | Está declarado como `--reload-agendado`, passa pelo envelope e chega ao mesmo HUP. Teria o compartilhado proposto, sem adquirir o exclusivo do ramo `--restart` (`app/config/crons.py:326-327`; `app/config/crons.py:1861-1863`; `bin/deploy.sh:346-359`). |
| `docker compose up -d`, rebuild/recreate | São explicitamente atos de infra externos ao deploy; escapam do exclusivo descrito (`bin/deploy.sh:20-22`; `bin/deploy.sh:66-69`). O README também contém uma sequência direta de remoção/recriação de `saas_web`; uso atual: **[nao verificado]** (`README.md:35-38`). |
| Restart automático | Mensageria declara `restart: unless-stopped`, sem participação no lock proposto. Política efetivamente instalada: **[nao verificado]** (`docker-compose.mensageria.yml:9`). |
| Outros caminhos examinados | Os moldes chamam `--reload-copia`; o vigia de login chama `--sem-migrate`, portanto chegam ao HUP. `recursos.sh` aplica `docker update` (`bin/molde_fatia/cadeia.sh:32`; `bin/molde_fatia/cadeia.sh:48`; `bin/vigia_logins.sh:87`; `bin/recursos.sh:140-145`). |

Reinício manual externo, host, daemon, OOM e configuração efetiva de core/ui: **[nao verificado]**. O compose principal é declarado específico do ambiente e ignorado (`.gitignore:40-41`).

Existe também uma ambiguidade no término do exclusivo: o contrato fala em “prova de casca depois do restart”, mas hoje `prova_de_casca.sh` roda **antes**; depois do restart há espera das cascas e provas de rota (`bin/deploy.sh:292-298`; `bin/deploy.sh:380-418`).

**3. Disparos fora de `cron_run.sh`**

**`RAW` não significa sem envelope:** todas as entradas de `CRONS`, inclusive `RAW`, passam por `_run`; cada elo executado também entra em `cron_run.sh` (`app/config/crons.py:1483-1486`; `app/config/crons.py:1860-1874`; `bin/encadeado.sh:102-104`).

Existem, porém, caminhos externos:

- **Despachante da corrente:** nasce depois que o envelope da raiz terminou e consulta `saas_core` para obter os elos antes de entrar em qualquer envelope. Essa consulta escapa do compartilhado; se falhar, registra ALARME e sai 2 sem passar pelo vigia do envelope (`app/config/crons.py:1884-1885`; `bin/encadeado.sh:40-52`).
- **Cron avulso em `/etc/cron.d`:** `deploy_agendado.sh` gera chamada direta a si próprio e executa as esteiras diretamente. Não ganha compartilhado automaticamente (`bin/deploy_agendado.sh:54-57`; `bin/deploy_agendado.sh:63-73`).
- **Timers:** esteira, fabricante, integrador, relato e teto dia/noite têm `ExecStart` direto, sem envelope (`bin/hasner-esteira.service:15`; `bin/hasner-fabricante.service:14`; `bin/hasner-integrador.service:13`; `bin/hasner-relato.service:15`; `bin/hasner-teto-dia.service:7`; `bin/hasner-teto-noite.service:7`).
- **Exemplo concreto de timer usando casca:** relato chama `relato_nomes.sh`, que faz `docker exec saas_core` diretamente (`bin/relato.sh:32`; `bin/relato_nomes.sh:17-21`).
- **Chamadas manuais:** `lavrar_deslogados.sh` mantém uma porta manual com `docker exec` direto (`bin/lavrar_deslogados.sh:11-18`).

Crontab cru, cron.d e timers efetivamente instalados em produção: **[nao verificado]**. Portanto, “todo cron” não está demonstrado como cobertura global; o código demonstra cobertura dos comandos envelopados e dos elos que chegam ao envelope (`app/config/crons.py:1860-1885`; `bin/encadeado.sh:102-104`).

**4. Crons longos e recusa prolongada**

Não localizei `placar.jsonl` na cópia; durações típicas atuais: **[nao verificado]**. Existe um snapshot datado de **31/10**, calculado como **p95 × 1,5, piso 30 s**, preservando medidas antigas quando faltam novas; os números abaixo não são duração típica bruta nem limite de execução (`app/config/crons_duracao.json:106-110`; `bin/crons_duracao.py:69-95`).

| Cron | Medida com folga | Fonte |
|---|---:|---|
| Sombra | 5.188 s — 86min28s | `app/config/crons_duracao.json:78` |
| Noturno | 1.800 s — 30min | `app/config/crons_duracao.json:56` |
| `lavrar_he_pendente` | 1.274 s — 21min14s | `app/config/crons_duracao.json:43` |
| Cartório | 760 s — 12min40s | `app/config/crons_duracao.json:63` |
| Auditoria de invariantes | 420 s — 7min | `app/config/crons_duracao.json:11` |
| Esmeril / scores | 340 s / 310 s | `app/config/crons_duracao.json:28`; `app/config/crons_duracao.json:14` |

A sombra é um único envelope para `--refazer && --bloco`. A corrente não mantém um envelope contínuo: cada elo espera carimbos antes de entrar no seu próprio envelope (`app/config/crons.py:350-352`; `app/config/crons.py:1884-1885`; `bin/encadeado.sh:84-104`).

O backup percorre schemas e faz dumps sem timeout declarado nesses scripts; seu valor de 30 s coincide com o piso e não prova rapidez nem duração de horas (`scripts/backup_all.sh:5-8`; `scripts/backup_tenant.sh:10-12`; `app/config/crons_duracao.json:12`).

O `vigia_arvore` pode permanecer bastante tempo: espera pelo retrato, pode aguardar até 3.600 s pela trava da suíte e executa suítes/repetições em primeiro plano. Horas são possíveis pelo fluxo; frequência e duração típica: **[nao verificado]** (`bin/vigia_arvore.sh:38-43`; `bin/vigia_arvore.sh:83-106`; `bin/trava_teste.sh:22-38`).

**Isso já recusa restart hoje:** a marca permanece durante esses comandos e o deploy recusa enquanto encontra seu PID vivo. Mantido o escopo, a duração longa não é uma restrição nova do compartilhado; a mudança fecha a janela entre consulta e restart. “Sempre recusado” não é demonstrável sem dados de execução (`bin/cron_run.sh:9-11`; `bin/deploy.sh:108-113`; `bin/deploy.sh:341-342`).

**5. Esperar 600 s, sair 75, corrente e TI**

**Rc 75 é falha para o envelope atual**, se chegar ao seu epílogo: grava `exit:75`, não escreve novo carimbo de fim, tenta avisar `cron_quebrado` e termina 75. Só 0 e 2 são aceitos como conclusão (`bin/cron_run.sh:11-38`). O serviço destina a pauta à TI (`app/core/services/crons_quebrados.py:57-73`).

Há quatro ressalvas:

- Um **`exit 75` antecipado** na aquisição do futuro lock pularia placar e aviso. A implementação precisa encaminhar essa falha ao epílogo; isso ainda está **[nao verificado]** (`bin/cron_run.sh:11-35`).
- A pauta é uma **tentativa** via `docker exec`, com saída ocultada e flag criada somente após sucesso. Pauta efetivamente criada: **[nao verificado]** (`bin/cron_run.sh:30-35`).
- A corrente é chamada mesmo se a raiz falhar, por causa do `;`. Sem carimbo, o dependente espera, alarma e sai 2 **antes do envelope**; esse timeout não gera automaticamente pauta daquele elo. O percurso continua nos elos seguintes (`app/config/crons.py:1884-1885`; `bin/encadeado.sh:64-68`; `bin/encadeado.sh:84-104`).
- Os carimbos são por dia. Um sucesso anterior no mesmo dia não é apagado por rc 75 posterior e pode satisfazer o dependente (`bin/cron_run.sh:22-25`; `bin/encadeado.sh:81-89`).

O contador atual considera a última corrida em **26 h**; não substitui a nova contagem contratual de ocorrências de rc 75 por lock em **24 h** (`app/core/services/crons_quebrados.py:17-47`).

Também falta definir `dur` versus `espera_lock`: conservar o relógio inicial antes da espera faria essa espera entrar no p95, pois o medidor não desconta `espera_lock` nem filtra falhas (`bin/cron_run.sh:7-13`; `bin/crons_duracao.py:69-76`).

**6. Confronto com a lei escrita**

- **“Recusa, não espera”: não há contradição demonstrada.** A regra é dirigida ao deploy `--restart`; o contrato faz o cron esperar e mantém o deploy não bloqueante. O mecanismo proibido é esperar e depois reiniciar por cima (`bin/deploy.sh:103-117`; `bin/contrato_deploy.sh:50-51`).
- **Seção 2:** permanece compatível se conservar HUP automático, restart manual e os demais portões; essa preservação futura está **[nao verificado]** (`CLAUDE.md:90`).
- **Seção 7 / lap-67:** o lock não exige mudar a fonte declarativa dos crons; afirmar equivalência com o instalado continua **[nao verificado]** (`CLAUDE.md:236-239`; `app/docs/LAPIDES.md:736-741`).
- **Lap-68:** a corrente deve continuar dependendo dos arquivos de fim. Rc 75 sem novo carimbo respeita essa condição, com a ressalva dos carimbos anteriores do mesmo dia (`CLAUDE.md:240-242`; `app/docs/LAPIDES.md:745-751`; `bin/encadeado.sh:81-94`).
- **GATE TEMPORAL:** esperar no processo do cron por exclusão em arquivo não demonstra retorno de `pgrep` nem de esperador de sessão do Code (`CLAUDE.md:204`; implementação proposta **[nao verificado]**).
- **Rc 75 “não é vermelho”:** essa exceção está no capítulo da suíte; o envelope dos crons aceita somente 0/2. Generalizá-la aos crons contradiz o tratamento atual (`CLAUDE.md:95-104`; `bin/cron_run.sh:27-35`).

Os selos atuais precisam acompanhar a troca de autoridade: hoje exigem referência à marca e simulam cron vivo criando apenas arquivo, sem adquirir o futuro compartilhado (`bin/contrato_deploy.sh:79-82`; `bin/tests/test_deploy_reload_gracioso.sh:125-131`; `bin/tests/test_deploy_portoes_antes_do_migrate.sh:192-198`).

Por fim, o contrato não remove expressamente a recusa residual de `--restart` com migration pendente no core. Essa recusa continua escrita e ocorre antes da aquisição final proposta; sua retirada ou alteração não está comprovada (`bin/deploy.sh:219-228`).

## 09/11 02:22 · sessao edca7285fe1d · - · - · origem=Ronald (tmux)

**Q4 — crons longos e efeito no restart**

Não há duração atual de produção comprovada pelo anexo; `placar.jsonl` de produção está [nao verificado]. A cópia contém um snapshot declarado como gerado em **31/10**, janela de sete dias, `p95 × 1,5`, piso de 30 segundos — não é duração típica bruta nem teto de execução (`app/config/crons_duracao.json:106-110`; `bin/crons_duracao.py:69-76`). Medições antigas são preservadas quando faltam novas (`bin/crons_duracao.py:83-95`).

| Cron | Medida versionada com folga | Fonte |
|---|---:|---|
| `sombra.sh` | 5188 s ≈ 86 min | `app/config/crons_duracao.json:78` |
| `noturno.py` | 1800 s = 30 min | `app/config/crons_duracao.json:56` |
| `lavrar_he_pendente` | 1274 s ≈ 21 min | `app/config/crons_duracao.json:43` |
| `processar_cartorio` | 760 s ≈ 13 min | `app/config/crons_duracao.json:63` |
| `auditar_invariantes_chamados` | 420 s = 7 min | `app/config/crons_duracao.json:11` |
| `esmeril_espelho` | 340 s | `app/config/crons_duracao.json:28` |
| `calcular_scores` | 310 s | `app/config/crons_duracao.json:14` |
| `backup_all.sh` | 30 s, sujeito ao piso | `app/config/crons_duracao.json:12,110` |

O backup percorre os schemas e executa um dump por schema; não há timeout declarado nesses scripts. Isso permite demora, mas não prova backup de horas (`scripts/backup_all.sh:5-8`; `scripts/backup_tenant.sh:10-12`).

A sombra é **um único envelope** para `--refazer && --bloco`; portanto, o compartilhado cobriria ambos e suas esperas internas (`app/config/crons.py:350-352,1861-1863`). A trava própria da sombra pode esperar 1800 s antes do trabalho (`bin/sombra_trava.sh:18,26-28`).

A corrente **não é um envelope contínuo**: o render coloca `; encadeado.sh --corrente ...` depois do `cron_run` da raiz (`app/config/crons.py:1873-1885`). A espera dos carimbos acontece antes de entrar no envelope de cada elo; o compartilhado seria adquirido apenas ao executar cada elo (`bin/encadeado.sh:84-104`). Portanto, somar todos os elos não prova um compartilhado contínuo de horas.

Há um caso adicional: `vigia_arvore`. O fallback declarado de 3300 s é **sobrescrito**, não combinado por máximo, pelo JSON que registra 30 s (`app/config/crons.py:139,159-160`; `app/config/crons_duracao.json:89`). O código pode esperar 600 s pelo retrato, esperar até 3600 s pela trava da suíte e executar suítes em primeiro plano, incluindo repetições; não há timeout global para a suíte (`bin/vigia_arvore.sh:38-43,83-88,106,131-135`; `bin/trava_teste.sh:22,35-38`). Horas são possíveis pelo código; duração típica de horas e restart “sempre recusado” permanecem [nao verificado].

**Esses tempos já bloqueiam o restart por marca:** ela nasce antes do comando e fica até a saída; o deploy recusa qualquer marca cujo PID exista (`bin/cron_run.sh:9-11`; `bin/deploy.sh:108-113`). Mantido o mesmo escopo, o compartilhado não acrescenta a duração do cron à recusa; acrescenta exclusão atômica na janela que hoje existe entre verificar e reiniciar (`bin/deploy.sh:341-342`).

**Q5 — rc 75, corrente e TI**

Se **o comando devolve 75 ao epílogo atual**, o envelope grava `exit:75`, não escreve `logs/fim/`, tenta chamar `cron_quebrado` e sai 75 (`bin/cron_run.sh:11-13,22-25,29-38`). O serviço classifica qualquer saída diferente de 0 e 2 como quebrado e escreve pauta destinada a TI (`app/core/services/crons_quebrados.py:16,45-47,57-73`).

Mas o contrato ainda precisa determinar o caminho de falha da aquisição: um **`exit 75` antecipado**, antes desse epílogo, não grava placar nem chama vigia. A aquisição proposta não está implementada em `cron_run.sh`; o fluxo atual executa diretamente o comando (`bin/cron_run.sh:7-13`). Garantia futura de passar RC=75 pelo epílogo: [nao verificado].

A pauta não é garantida pelo retorno 75 sozinho: o envelope usa `docker exec`, oculta sua saída e cria a flag apenas se a chamada der certo; com flag já existente, não repete a chamada (`bin/cron_run.sh:30-35`). Pauta efetivamente criada sem acesso ao banco/log: [nao verificado].

A raiz usa `;`, portanto a corrente é chamada mesmo depois de rc75 (`app/config/crons.py:1884-1885`). Sem carimbo do gatilho, cada dependente espera, escreve ALARME e sai 2 sem executar seu comando (`bin/encadeado.sh:84-97`). Essa expiração acontece **fora do cron_run**, logo não gera automaticamente placar/vigia daquele elo; o percorrimento continua nos elos seguintes (`bin/encadeado.sh:64-68,102-104`).

Há uma ressalva: carimbos são por **dia**, não por tentativa. O envelope não remove carimbo de sucesso anterior; o encadeado consulta apenas sua existência. Uma repetição com rc75 pode deixar disponível um carimbo bem-sucedido anterior do mesmo dia e permitir o dependente (`bin/cron_run.sh:22-25`; `bin/encadeado.sh:81-89`). Isso já é propriedade do protocolo atual.

O contador atual de quebrados usa a corrida mais recente nas últimas **26 horas**, não o contador contratual de rc75 por lock em 24 h (`app/core/services/crons_quebrados.py:17,24-47`).

**Ressalva de medição:** se o futuro envelope conservar `T0` antes da espera e `dur=T1-T0`, a espera entrará na duração usada pelo medidor; ele não filtra por `exit` nem desconta `espera_lock`. Assim, até timeouts rc75 podem inflar o p95 e alterar horários derivados (`bin/cron_run.sh:7,13`; `bin/crons_duracao.py:69-76`). A definição futura de `dur` versus `espera_lock` está [nao verificado].

## 09/11 02:22 · sessao 00b70fe1c898 · - · - · origem=Ronald (tmux)

**Q2 — Cobertura dos reinícios**

| Via | O que a cópia implementa | Cobertura pelo contrato proposto |
|---|---|---|
| `deploy.sh --restart "motivo"` | Modo manual, exige motivo; verifica marcas antes dos portões e novamente imediatamente antes de `docker restart saas_core saas_ui mensageria`. A própria cópia documenta a janela entre amostragem e restart. (`bin/deploy.sh:72-75,108-117,310-314,338-342`) | É a via nomeada pelo contrato fornecido; sua implementação com flock ainda é [nao verificado]. |
| Deploy normal, `--sem-migrate`, `--sem-sombra`, `--reload-copia` | Convergem para HUP das três cascas, com troca dos workers; falha do HUP não provoca fallback para restart. (`bin/deploy.sh:322-337,343-372`) | Ficam fora do exclusivo descrito, mas o código explica que `docker exec` não recebe esse HUP, destinado ao master PID 1. (`bin/deploy.sh:327-329`) Comportamento real das cascas de produção: [nao verificado]. |
| Reload diário 03:30 | `RAW('30 3 * * *', ...deploy.sh --reload-agendado)`; RAW também recebe `cron_run.sh`. (`app/config/crons.py:326-327,1475,1483-1486,1860-1864`) O modo usa o ramo HUP. (`bin/deploy.sh:65,346-359`) | Pelo contrato, teria o compartilhado do envelope, mas não o exclusivo do `--restart`. A premissa de que esse reload mata o cron não é sustentada pelo código: ele descreve preservação do exec. (`bin/deploy.sh:327-329`) |
| `docker compose up -d`, rebuild/recreate | Explicitamente fora de `deploy.sh`; imagem, command e Env novos precisam de ato de infra. (`bin/deploy.sh:20-22,66-69`) README ainda ensina `docker rm -f saas_web && docker-compose up -d --no-deps web`. (`README.md:35-38`) | Escapam: não chamam o exclusivo descrito no contrato. Execução atual desses comandos: [nao verificado]. |
| Restart automático da casca | Mensageria declara `restart: unless-stopped`. (`docker-compose.mensageria.yml:9`) | Não há aquisição de flock nessa definição; política/comportamento vivo: [nao verificado]. |
| Outros scripts examinados | `recursos.sh` aplica `docker update`, não restart; `db_teste.sh` e `sombra_db.sh` criam ou dão start nos próprios bancos auxiliares. (`bin/recursos.sh:140-145`; `bin/db_teste.sh:49-70`; `bin/sombra_db.sh:86-100`) | Esses trechos não reiniciam core/ui/mensageria. Outras vias externas, reinício de host/daemon, OOM e ações manuais: [nao verificado]. |

O `docker-compose.yml` principal não está na cópia e é declarado específico de produção/dev e ignorado; não se pode provar daqui a configuração core/ui instalada. (`.gitignore:40-41`; ausência na busca da cópia: [nao verificado])

As cadeias de fatia e auto-cura chamam `--reload-copia` e `--sem-sombra`, portanto chegam ao HUP, não a um restart adicional. (`bin/molde_fatia/cadeia.sh:32,48,89`; `bin/molde_autocura/cadeia.sh:31,45,86`; `bin/deploy.sh:338-359`) O auto-revert do vigia de login chama `--sem-migrate`, igualmente HUP. (`bin/vigia_logins.sh:87`; `bin/deploy.sh:338-359`)

**Q3 — Existem disparos fora do envelope**

- **Todos os `CRONS` declarados, inclusive `RAW`, são envelopados** por `_run`; os elos também entram por `cron_run.sh`. (`app/config/crons.py:1475,1483-1486,1860-1874`; `bin/encadeado.sh:102-104`) Crontab instalado de produção idêntico ao render: [nao verificado].
- **O despachante da corrente escapa antes dos elos:** a linha gerada executa `encadeado.sh --corrente` depois do wrapper da raiz; esse despachante faz `docker exec saas_core ... crons corrente` antes de entrar no wrapper dos elos. (`app/config/crons.py:1884-1885`; `bin/encadeado.sh:40-45,64,102-104`) Se esse render falhar, apenas escreve ALARME e sai 2: não chega a `cron_run.sh`/`cron_quebrado`. (`bin/encadeado.sh:47-52`) Portanto, o contrato de lock apenas no envelope **não protege o próprio nascimento da corrente** desse restart.
- **Cron avulso em `/etc/cron.d`:** `deploy_agendado.sh agendar` grava chamada direta a `deploy_agendado.sh rodar`, que executa as esteiras diretamente. (`bin/deploy_agendado.sh:54-57,63-73`) Esse disparo não passa pelo envelope; se a esteira chama `deploy.sh --restart`, terá a proteção do lado deploy prevista pelo contrato, mas seus demais comandos não ganham compartilhado automaticamente. Conteúdo e instalação dos cron.d vivos: [nao verificado].
- **Timers systemd diretos:** esteira, fabricante, integrador, relato e teto dia/noite chamam scripts em `ExecStart` diretamente, sem `cron_run.sh`. (`bin/hasner-esteira.service:15`; `bin/hasner-fabricante.service:14`; `bin/hasner-integrador.service:13`; `bin/hasner-relato.service:15`; `bin/hasner-teto-dia.service:7`; `bin/hasner-teto-noite.service:7`) Os respectivos disparos estão declarados nos timers. (`bin/hasner-esteira.timer:7`; `bin/hasner-fabricante.timer:8`; `bin/hasner-integrador.timer:6`; `bin/hasner-relato.timer:5-6`; `bin/hasner-teto-dia.timer:5`; `bin/hasner-teto-noite.timer:5`) Instalação/ativação viva: [nao verificado].
- **Exemplo concreto de timer que usa uma casca:** `hasner-relato.service` chama `relato.sh`; este chama `relato_nomes.sh`, que executa Django no core diretamente. (`bin/hasner-relato.service:15`; `bin/relato.sh:32`; `bin/relato_nomes.sh:17-21`) Esse trabalho escapa do compartilhado proposto; não é correto concluir que todo timer listado executa cron de negócio.
- **Chamadas manuais existentes também escapam:** `lavrar_deslogados.sh` documenta sua chamada manual e executa diretamente `docker exec ... lavrar_deslogados`; o agendamento normal desse comando pertence à corrente. (`bin/lavrar_deslogados.sh:11-18`) `gate_cobranca.sh` executa sondas e `cartao_x_txt_divergentes` diretamente; `placar_code.sh` executa diversos management commands diretamente. (`bin/gate_cobranca.sh:36-40`; `bin/placar_code.sh:215-218,238-259`) Uso efetivo dessas portas durante restart: [nao verificado].

Não há prova de crontab cru atualmente instalado; os exemplos antigos de linhas cruas em docstrings de commands são instruções, não evidência de instalação. (`app/ponto/management/commands/detectar_ausencias.py:9`; `app/ponto/management/commands/processar_alertas_avancados.py:7`; situação instalada: [nao verificado])

## 09/11 02:21 · sessao 2bd182e82d5b · - · - · origem=Ronald (tmux)

**1. Censo da marca na cópia**

- Escritor operacional localizado: `bin/cron_run.sh:9-10` cria `logs/crons_em_curso/${N}_${E}.$$`, grava início e remove no `EXIT`. A marca nasce antes do comando (`bin/cron_run.sh:11`) e permanece durante placar, carimbo e chamada ao vigia (`bin/cron_run.sh:13-37`).
- Leitor operacional localizado: `bin/deploy.sh:108-113`. Enumera arquivos, considera vivo o PID da extensão quando `/proc/PID` existe, apaga marcas sem esse PID e recusa restart. Chamado antes dos portões (`bin/deploy.sh:115-117`) e imediatamente antes do restart (`bin/deploy.sh:338-342`).
- Escritor secundário: o próprio deploy apaga marcas julgadas órfãs (`bin/deploy.sh:109`).
- `bin/contrato_deploy.sh:79-82` não lê o diretório em execução: inspeciona texto dos scripts e exige que o deploy e o envelope contenham `crons_em_curso`.
- Testes escrevem marcas em fixtures: `bin/tests/test_deploy_reload_gracioso.sh:34,91`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:63,76,88,110`. Em particular, o selo de reload exige recusa mediante marca, sem pegar o futuro lock (`bin/tests/test_deploy_reload_gracioso.sh:125-131`); os casos 15/16 também injetam marca durante a prova de casca (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:192-198`).

Não localizei leitores do diretório em hook, vigias ou `gerar_estado` mediante busca integral da cópia; outros acessos indiretos/dinâmicos são **[nao verificado]**. O hook mede trabalho por processos de teste/push (`bin/hook_stop_fila1.py:373-397`) e bloqueia clear por outro lock, `logs/sombra.lock` (`bin/hook_stop_fila1.py:785-794`). O vigia da esteira usa trava própria e delega ao Python (`bin/vigia_esteira.sh:8-11`); vigia da árvore também tem trava própria (`bin/vigia_arvore.sh:25`).

Existe consumidor **documental/humano** do significado da marca: esperar a conclusão da sombra pela ausência de `sombra.sh_-.*`, nunca por pgrep (`CLAUDE.md:92`; `app/docs/INFRA.md:59-60`). O contrato não define se a marca será criada antes ou depois da espera no lock; preservação exata desse significado é **[nao verificado]**. Se criada antes, passará a incluir cron esperando; se criada depois, não mostrará a fila — inferência de `bin/cron_run.sh:9-11` e do contrato fornecido **[nao verificado: implementação proposta ausente]**.

Há um escritor histórico fora do envelope mencionado no relato: `logs/gate10/gate10_condicao1.sh`, com marca própria durante apply de aproximadamente 45 minutos (`app/docs/RELATO.md:4472-4489`). Não está comprovado como código presente/ativo; existência atual e migração para lock são **[nao verificado]**.

**6. Confronto com a lei escrita**

- **Sem contradição demonstrada com seção 2:** ela manda publicar via deploy, usa HUP automático e reserva `--restart` manual com recusa por cron vivo (`CLAUDE.md:90`). O lock exclusivo não bloqueante conserva recusa; HUP permanece fora do problema do exec (`bin/deploy.sh:322-337`). Preservar todos os demais portões é **[nao verificado]**, pois implementação ausente.
- **Seção 7 / lap-67:** a autoridade declarativa é `config/crons.py`, não uma lista nova no contrato (`CLAUDE.md:236-239`; `app/docs/LAPIDES.md:736-741`). Introduzir lock no envelope não contradiz essa fonte. “Todo cron” continuar passando pelo envelope é **[nao verificado]** e depende do censo do outro explorer.
- **Seção 7 / lap-68:** corrente continua dependendo do arquivo do anterior, não do minuto (`CLAUDE.md:240-242`; `app/docs/LAPIDES.md:745-751`). Rc75 não carimba fim hoje, pois só 0/2 carimbam (`bin/cron_run.sh:19-25`); portanto dependente não recebe conclusão bem sucedida inventada.
- **GATE TEMPORAL:** o texto exige cron/arquivo para espera longa, proíbe sleep do Code e `pgrep -f` (`CLAUDE.md:204`). O contrato põe espera limitada no processo do cron, via lock em arquivo, e não propõe pgrep. Não demonstrada contradição com essa formulação; comportamento real é **[nao verificado]**.
- **“Recusa, não espera”:** a regra atual é dirigida ao deploy `--restart`, não ao cron (`bin/deploy.sh:103-117`). O selo condena explicitamente `ESPERA_CRON_S` e o retorno do “espera-e-reinicia” (`bin/contrato_deploy.sh:50-51`). Cron esperar deploy com `-w 600`, enquanto deploy usa `-n`, não reintroduz esse fallback; implementação é **[nao verificado]**.
- **Incompatibilidade mecânica a tratar:** substituir a decisão da marca pelo lock deixa os selos atuais presos à autoridade antiga: exigência textual (`bin/contrato_deploy.sh:79-82`) e fixtures que só criam marca (`bin/tests/test_deploy_reload_gracioso.sh:125-131`; `bin/tests/test_deploy_portoes_antes_do_migrate.sh:192-198`). Isso é incompatibilidade dos testes com a migração proposta, não prova de contradição com a lei.
- A frase “rc75 não é vermelho” existe no capítulo da **suite** (`CLAUDE.md:95-104`), enquanto o envelope de cron classifica qualquer saída diferente de 0/2 como falha (`bin/cron_run.sh:27-35`). Generalizar a exceção da suite a todos os crons não é sustentado pelo texto.

## 09/11 02:21 · sessao c79c9d113d29 · - · - · origem=Ronald (tmux)

Q4 — **O fail-open está declarado no contrato existente desta cópia:** “FALHA ABERTO: toda excecao vira linha FALHA na trilha e a ferramenta passa; medida ilegivel = ‘nao sei’, nunca age.” (`bin/disjuntor.py:34`). O tratamento externo mantém essa regra, exceto B3, que nega quando não sabe; CENSO ganha seu próprio `except Exception`, grava `CENSO ABRIU-SEM` e retorna `None`. (`bin/disjuntor.py:1643`, `bin/disjuntor.py:1645`, `bin/disjuntor.py:1653`, `bin/disjuntor.py:1504`, `bin/disjuntor.py:1525`).

**Isso não basta para realizar o aval específico:** o I-1 fornecido exige recusa de brief sem CENSO, enquanto essa exceção permite continuar sem obter a decisão do juiz. A compatibilidade autorizada dessa exceção com o aval é **[nao verificado]**. (`bin/disjuntor.py:1504`, `bin/disjuntor.py:1525`; aval literal fornecido, sem arquivo numerado na cópia).

**LEI-AKITA 1 não equivale simplesmente a “qualquer tratamento de exceção é proibido”:** ela caracteriza fallback como sobrevivência de derivador paralelo. Portanto, apenas chamar este ramo de fail-open não demonstra violação da linha 1; há, porém, conflito verificável com “exceção [...] nunca destravando a trava” e com a literalidade do aval. (`CLAUDE.md:17`, `CLAUDE.md:19`, `CLAUDE.md:32`, `CLAUDE.md:34`, `CLAUDE.md:37`).

Q8 — mapa dos escritores e leitores:

| Objeto | Quem escreve | Quem lê / significado |
|---|---|---|
| `logs/sessao/disjuntor.log` | Porta física `trilha`, append com carimbo próprio e detalhe limitado a 300 caracteres; exceção de escrita é engolida. (`bin/disjuntor.py:68`, `bin/disjuntor.py:135`, `bin/disjuntor.py:139`, `bin/disjuntor.py:140`, `bin/disjuntor.py:142`) | `ler` conta eventos gerais; `_censo_24h` interpreta eventos CENSO. O hook também reconhece o arquivo como trilha própria, proibida para provar espera por avanço de arquivo. (`bin/disjuntor.py:1717`, `bin/disjuntor.py:1725`, `bin/disjuntor.py:1784`, `bin/hook_stop_fila1.py:583`, `bin/hook_stop_fila1.py:585`, `bin/hook_stop_fila1.py:598`) |
| `CENSO OK <obra> :: via=... brief=...` | `_censo_nega`, depois de validar os briefs citados. (`bin/disjuntor.py:1513`, `bin/disjuntor.py:1517`) | `_censo_24h` incrementa `ok` e encerra o intervalo daquela obra. (`bin/disjuntor.py:1771`, `bin/disjuntor.py:1797`) |
| `CENSO ABRIU-SEM <obra> :: ...` | `_censo_nega`, se uma exceção escapar do check. (`bin/disjuntor.py:1525`) | `_censo_24h` incrementa `abertos_sem`. (`bin/disjuntor.py:1777`, `bin/disjuntor.py:1801`) |
| `NEGOU DISJUNTOR CENSO: construtor de <obra> sem CENSO...` | `main_pretool`, antes de enviar JSON deny. (`bin/disjuntor.py:1636`, `bin/disjuntor.py:1639`) | `_censo_24h` inicia intervalo por obra e incrementa `negados`. (`bin/disjuntor.py:1771`, `bin/disjuntor.py:1794`) |
| Medidas `abertos_sem`, `negados`, `ok`, `ociosa_h`, `esperando` | `_censo_24h` calcula o dicionário, consumido por `ler`. (`bin/disjuntor.py:1767`, `bin/disjuntor.py:1807`) | `_censo_txt` apresenta as medidas; `linha` a inclui; `gerar_estado` importa `ler/linha` e grava o destino recebido. (`bin/disjuntor.py:1812`, `bin/disjuntor.py:1820`, `bin/disjuntor.py:1895`, `bin/gerar_estado.py:280`, `bin/gerar_estado.py:281`, `bin/gerar_estado.py:390`) |
| `ESTADO.md` publicado | `relato.sh` pede `$TMP/ESTADO.md` ao gerador e o entrega à raspagem. (`bin/relato.sh:38`, `bin/relato.sh:103`, `bin/relato.sh:105`) | O destino público recebe esse artefato raspado; consumo posterior em produção **[nao verificado]**. (`bin/relato.sh:105`) |
| Linha literal `PAREI: censo-<obra> \| espera Codex` no topo | O diff **manda a sessão escrever**, não grava essa linha: é texto da negativa; `--censo` também só imprime instrução. Escritor concreto da linha em produção **[nao verificado]**. (`bin/disjuntor.py:1414`, `bin/disjuntor.py:1521`, `bin/disjuntor.py:1908`) | `_esperando_com_prova` lê o primeiro painel via `_linha_do_topo`, exige forma literal e chama `_prova_censo`. (`bin/hook_stop_fila1.py:400`, `bin/hook_stop_fila1.py:702`, `bin/hook_stop_fila1.py:705`) |
| Linha automática `(NEGOU CENSO)` no RELATO | `relato_negacao` monta linha de máquina e usa `core.esteira_vigia.no_relato`; **não é** a linha PAREI do topo. (`bin/disjuntor.py:221`, `bin/disjuntor.py:234`, `bin/disjuntor.py:236`) | A porta insere em `## ALARMES DA ESTEIRA`, preservando o topo; a escrita usa flock no diretório e temporário único. (`app/core/esteira_vigia.py:815`, `app/core/esteira_vigia.py:829`, `app/core/esteira_vigia.py:834`, `app/core/esteira_vigia.py:879`, `app/core/esteira_vigia.py:884`) |

**Há divergência de leitores da linha PAREI censo.** O Stop a trata como ESPERANDO provado e compartilha a mesma resposta memoizada com o orçamento; a liberação ocorre por essa prova. (`bin/hook_stop_fila1.py:703`, `bin/hook_stop_fila1.py:740`, `bin/hook_stop_fila1.py:1040`, `bin/hook_stop_fila1.py:1283`).

O handoff exibe `PAREI`, mas decide “trava o item?”/“devolve o turno?” somente pelos predicados genéricos; esses exigem `!`, ausente na linha censo. Assim, com censo vivo, o Stop pode liberar enquanto o handoff imprime “item em curso ANDA” e “não [...] devolve”. (`bin/handoff_sessao.sh:108`, `bin/handoff_sessao.sh:120`, `bin/handoff_sessao.sh:123`, `bin/hook_stop_fila1.py:1171`, `bin/hook_stop_fila1.py:1284`).

O alarme aceita qualquer substring `PAREI:` nas primeiras 40 linhas, sem prova; portanto desarma também com o produtor morto, situação em que o Stop registra falta de prova. (`bin/alarme_sessao_ociosa.py:119`, `bin/alarme_sessao_ociosa.py:124`, `bin/alarme_sessao_ociosa.py:127`, `bin/alarme_sessao_ociosa.py:42`, `bin/hook_stop_fila1.py:1286`).

**Achado material de semântica das medidas:** `OK` e `ABRIU-SEM` nascem durante a avaliação CENSO, antes de outras travas ainda capazes de negar a abertura. No Agent, CENSO antecede B4; no Codex, antecede `codex_decide`. Logo esses eventos provam passagem/falha do check, não abertura efetiva de construtor. (`bin/disjuntor.py:1517`, `bin/disjuntor.py:1525`, `bin/disjuntor.py:1583`, `bin/disjuntor.py:1588`, `bin/disjuntor.py:1603`, `bin/disjuntor.py:1607`, `bin/disjuntor.py:1621`).

`ociosa_h` soma, por obra, do primeiro NEGOU ao próximo OK ou até agora; não consulta ocupação da vaga nem processo censo. É duração de intervalos de negativas, e pode somar obras simultâneas ou encerrar num OK cuja abertura depois foi negada. A correspondência com “horas de vaga ociosa” é **[nao verificado]**. (`bin/disjuntor.py:1778`, `bin/disjuntor.py:1795`, `bin/disjuntor.py:1800`, `bin/disjuntor.py:1807`).

Além disso, trilha inexistente produz zero, enquanto falha ao gravar trilha é silenciada; logo zero nesse contador não prova zero aberturas reais sem censo. (`bin/disjuntor.py:142`, `bin/disjuntor.py:1803`).

**Relógio/ordem:** as funções novas recebem `agora` na medida e não introduzem leitura direta nova do relógio; o escritor existente `trilha` já usa `datetime.now`, e as bordas `main_pretool`/CLI obtêm `time.time`. Não há prova de novo relógio fora da borda no diff; permanece o carimbo existente da trilha. (`bin/disjuntor.py:140`, `bin/disjuntor.py:1632`, `bin/disjuntor.py:1774`, `bin/disjuntor.py:1780`, `bin/disjuntor.py:1903`).

Não há nova ordem entre locks demonstrada nesses trechos: CENSO usa a porta de append existente, declarada sem trava; a linha de máquina no RELATO continua usando a porta e a trava existentes. A ordem **entre verificações** afeta o significado dos eventos de abertura, conforme acima. (`bin/disjuntor.py:1637`, `bin/disjuntor.py:1642`, `app/core/esteira_vigia.py:834`, `bin/disjuntor.py:1603`).

Observação de fonte: `app/docs/CORTES.md` desta cópia termina na linha 425; as referências 428/430 pertencem ao raspado fornecido e não podem ser tratadas como linhas existentes desta cópia. (`app/docs/CORTES.md:425`; conteúdo integral das linhas raspadas: **[nao verificado]**).

## 09/11 02:21 · sessao 33daae8acd7e · - · - · origem=Ronald (tmux)

Q6 — Há uma só autoridade de vivacidade de processo: `_prova_censo` chama `_prova_pid`, reutilizando exclusão de ancestral, comparação do executável/script, estabilidade do PID e rejeição de Z/X (`bin/hook_stop_fila1.py:682`, `bin/hook_stop_fila1.py:523`, `bin/hook_stop_fila1.py:529`, `bin/hook_stop_fila1.py:535`, `bin/hook_stop_fila1.py:546`).

A regra de vínculo com o censo é nova: procura wrapper desta raiz, mesmo UID, `CODEX_OBRA` exato e papel censo/leitura; o ESPERANDO genérico aceita produtor e pid/arquivo declarados, sem exigir obra/papel. Portanto, não são predicados idênticos; há composição com o juiz de processo existente e uma regra específica nova de identificação do produtor (`bin/hook_stop_fila1.py:661`, `bin/hook_stop_fila1.py:663`, `bin/hook_stop_fila1.py:671`, `bin/hook_stop_fila1.py:682`, `bin/hook_stop_fila1.py:715`, `bin/hook_stop_fila1.py:729`).

Ambos entram na mesma decisão `_esperando_com_prova`, memoização, orçamento e liberação do Stop; não foi criada liberação final independente (`bin/hook_stop_fila1.py:703`, `bin/hook_stop_fila1.py:707`, `bin/hook_stop_fila1.py:740`, `bin/hook_stop_fila1.py:1040`, `bin/hook_stop_fila1.py:1283`).

Os mutantes simples “aceitar construtor”, “casar prefixo de obra” e “liberar quando o processo já foi removido” encontram casos negativos específicos, com exigência de block/siga e motivo NOPROVA (`bin/tests/test_hook_esperando_com_prova.sh:80`, `bin/tests/test_hook_esperando_com_prova.sh:84`, `bin/tests/test_hook_esperando_com_prova.sh:87`, `bin/tests/test_hook_esperando_com_prova.sh:206`, `bin/tests/test_hook_esperando_com_prova.sh:209`, `bin/tests/test_hook_esperando_com_prova.sh:214`).

Sobreviventes plausíveis por leitura: remover proteção contra zumbi, UID ou troca de starttime. A fixture mata e faz `wait`, removendo o processo; não produz Z/X, outra identidade de usuário ou reciclagem de PID durante leitura. Execução desses mutantes: [nao verificado]. (`bin/hook_stop_fila1.py:546`, `bin/hook_stop_fila1.py:671`, `bin/hook_stop_fila1.py:685`, `bin/tests/test_hook_esperando_com_prova.sh:31`, `bin/tests/test_hook_esperando_com_prova.sh:57`, `bin/tests/test_hook_esperando_com_prova.sh:194`, `bin/tests/test_hook_esperando_com_prova.sh:213`).

Q7 — Os quatro mutantes declarados têm mordida demonstrável por leitura das asserções:

- Linha ausente passa a zero: altera o resultado esperado do caso sem-linha, rc 1. (`bin/tests/test_censo_antes_da_vaga.sh:70`, `bin/tests/test_censo_antes_da_vaga.sh:175`).
- Remover corte: altera os dois casos que retiram o corte da última pergunta. (`bin/tests/test_censo_antes_da_vaga.sh:71`, `bin/tests/test_censo_antes_da_vaga.sh:72`, `bin/tests/test_censo_antes_da_vaga.sh:176`).
- Tirar porta Agent: deixa de produzir a negativa específica CENSO exigida nos casos Agent negativos. (`bin/tests/test_censo_antes_da_vaga.sh:103`, `bin/tests/test_censo_antes_da_vaga.sh:112`, `bin/tests/test_censo_antes_da_vaga.sh:177`).
- Tirar porta Codex: deixa de produzir aquela negativa no caso construtor sem brief. (`bin/tests/test_censo_antes_da_vaga.sh:103`, `bin/tests/test_censo_antes_da_vaga.sh:131`, `bin/tests/test_censo_antes_da_vaga.sh:178`).

A harness exige retorno não zero da função de casos depois da mutação e verifica âncora única; portanto, não basta declarar o mutante. Rodada integral dos selos: [nao verificado], pois criam arquivos (`bin/tests/test_censo_antes_da_vaga.sh:160`, `bin/tests/test_censo_antes_da_vaga.sh:165`, `bin/tests/test_censo_antes_da_vaga.sh:170`, `bin/tests/test_censo_antes_da_vaga.sh:173`).

**Mutante sobrevivente concreto:** trocar `for q in perguntas:` por `for q in perguntas[-1:]:`. Verifica somente a última pergunta; os dois casos que retiram corte mexem sempre na última. Reproduzi todos os 15 resultados de `casos_rc` em memória, extraindo AST e usando StringIO, sem gravar arquivos; também confirmei aceitação da primeira pergunta sem corte com a última cortada. A costura usa apenas briefs conforme/sem-linha, sem casos N>0 adicionais. (`bin/disjuntor.py:1469`, `bin/tests/test_censo_antes_da_vaga.sh:68`, `bin/tests/test_censo_antes_da_vaga.sh:71`, `bin/tests/test_censo_antes_da_vaga.sh:72`, `bin/tests/test_censo_antes_da_vaga.sh:99`, `bin/tests/test_censo_antes_da_vaga.sh:100`, `bin/tests/test_censo_antes_da_vaga.sh:112`).

A asserção positiva `_passa_censo` é fraca: qualquer saída sem `CENSO` passa, inclusive deny de outra trava ou saída vazia por erro/timeout; o status de `_pre` não é conferido. Já as negativas exigem deny+CENSO+PAREI específico. (`bin/tests/test_censo_antes_da_vaga.sh:97`, `bin/tests/test_censo_antes_da_vaga.sh:103`, `bin/tests/test_censo_antes_da_vaga.sh:106`).

A medida verifica rótulos e presença de número decimal, sem valor esperado; não há caso que provoque exceção e valide `ABRIU-SEM`, nem duração conhecida para ociosidade. Assim, retirar a trilha do ramo de exceção de `_censo_nega` permanece sem cobertura demonstrada. (`bin/tests/test_censo_antes_da_vaga.sh:141`, `bin/tests/test_censo_antes_da_vaga.sh:144`, `bin/tests/test_censo_antes_da_vaga.sh:147`, `bin/disjuntor.py:1525`, `bin/disjuntor.py:1526`).

## 09/11 02:21 · sessao 341d3f8bce55 · - · - · origem=Ronald (tmux)

2. **Um juiz de conteúdo localizado, mas a cobertura da porta é incompleta.** `censo_do_brief` decide a forma do bloco; `_censo_nega` o chama para os briefs reconhecidos; a CLI `--censo` chama a mesma função. O Stop prova produtor vivo, não o conteúdo do brief. `gerar_estado` apenas consome `disjuntor.ler/linha`; handoff consome predicados de fila/PAREI. Não localizei outro reconhecedor de “brief tem CENSO” nesses leitores nem autoridade anterior dessa pergunta em `app/core/juizes.py`/`app/docs/JUIZES.md`; a ausência global de qualquer implementação equivalente fica **[nao verificado]**. (`bin/disjuntor.py:1417`, `bin/disjuntor.py:1514`, `bin/disjuntor.py:1906`, `bin/hook_stop_fila1.py:649`, `bin/gerar_estado.py:280`, `bin/handoff_sessao.sh:120`, `app/core/juizes.py:170`, `app/core/juizes.py:706`, `app/core/juizes.py:1025`, `app/core/juizes.py:1173`, `app/core/juizes.py:1285`, `app/docs/JUIZES.md:19`)

3. **Vias e escapes demonstráveis pela cópia:**

| Forma | Trava nova / classificação |
|---|---|
| `Agent`, tipo fora de `SO_LEITURA`, prompt com caminho absoluto terminado `wt-*`, obra reconhecida e presente nos andáveis | Passa em `_censo_nega`. (`bin/disjuntor.py:98`, `bin/disjuntor.py:100`, `bin/disjuntor.py:1603`, `bin/disjuntor.py:1507`) |
| Mesmo `Agent`, mas `Construa OBRA-E3 ...` sem âncora `obra X`/`leitura ... da X` | `_obra_de` retorna `None`; `_censo_nega` libera. Escape real se o trabalho é construtor estrutural da fila 1. (`bin/disjuntor.py:108`, `bin/disjuntor.py:511`, `bin/disjuntor.py:1507`) |
| `Agent` construtor com cópia cujo nome não casa `wt-*`, caminho relativo ou sem cópia citada | Não entra no novo ramo. É escape real se ainda é construtor da classe avalizada; o teste define “sem cópia” como não construtor, mas não prova que escrever seja impossível. (`bin/disjuntor.py:100`, `bin/disjuntor.py:1603`, `bin/tests/test_censo_antes_da_vaga.sh:129`) |
| `Bash` com `CODEX_OBRA=X CODEX_PAPEL=construtor bin/codex_pergunta.sh --construtor ...` | Interceptado pelo parser e `_censo_nega`. (`bin/disjuntor.py:1332`, `bin/disjuntor.py:1583`) |
| Wrapper com papel/obra herdados: `export CODEX_OBRA=X CODEX_PAPEL=construtor; bin/codex_pergunta.sh --construtor ...` | Parser lê `papel='?'`, obra vazia; novo ramo pula. Wrapper aceita ambiente herdado. **Escape real de uma via autorizada.** (`bin/disjuntor.py:1333`, `bin/disjuntor.py:1343`, `bin/disjuntor.py:1584`, `bin/codex_pergunta.sh:81`) |
| `env CODEX_OBRA=X CODEX_PAPEL=construtor bin/codex_pergunta.sh ...` | `_INVOC_RE` admite `env` como prefixo, mas as atribuições só antes dos prefixos; não reconhece esta chamada. **Escape real**, wrapper lê o ambiente normalmente. (`bin/disjuntor.py:1250`, `bin/disjuntor.py:1342`, `bin/codex_pergunta.sh:81`) |
| `bash -c "..."`, invocação encapsulada em script externo | Residual expressamente declarado; wrapper não consulta CENSO antes da vaga nem do `codex exec`. Escape real se abre construtor abrangido pelo aval. (`bin/disjuntor.py:1247`, `bin/codex_pergunta.sh:233`, `bin/codex_pergunta.sh:394`) |
| Wrapper chamado diretamente no terminal/fora do `PreToolUse` | Nenhuma guarda CENSO no wrapper; B3 já tem consulta própria para verificador. A abertura de construtor continua possível sem consulta CENSO. (`bin/codex_pergunta.sh:370`, `bin/codex_pergunta.sh:384`, `bin/codex_pergunta.sh:394`) |
| `codex exec` direto | Residual declarado, porém proibido pela regra de uso do wrapper; via irregular, não evidência de uma segunda abertura autorizada. (`bin/disjuntor.py:1248`, `CLAUDE.md:347`) |
| Construtor de obra fechada/fila 2, leitura/censo/verificador | Fora da classe construtor estrutural fila 1. (`bin/disjuntor.py:1507`, `bin/tests/test_censo_antes_da_vaga.sh:123`, `bin/tests/test_censo_antes_da_vaga.sh:125`, `bin/tests/test_censo_antes_da_vaga.sh:127`, `bin/tests/test_censo_antes_da_vaga.sh:135`) |
| `bin/fabricante.sh` abre `claude -p` para fabricar fatias | A abertura dessa sessão não é `Agent`; não passa pela trava nova. O próprio fabricante se declara esteira estrutural e distingue fabricar em `.esteira/` da aplicação por `construir.py`; enquadramento de cada fatia como “obra ESTRUTURA (fila 1)” do aval **[nao verificado]**. (`bin/fabricante.sh:2`, `bin/fabricante.sh:6`, `bin/fabricante.sh:150`, `bin/fabricante.sh:162`) |

As chamadas do hook configuradas na cópia apontam o caminho absoluto `/home/ronald/saas-hasner/bin/disjuntor.py`; que o arquivo vivo nesse caminho contenha este diff é **[nao verificado]**. (`.claude/settings.json:68`)

As respostas dos parsers acima foram confirmadas por chamadas puras em memória, com bytecode desabilitado; não executei wrapper nem teste com criação de arquivos. Referências da implementação: `bin/disjuntor.py:1250`, `bin/disjuntor.py:1332`, `bin/disjuntor.py:511`.

5. **A trava não inventa outra definição de andável.** `_fila1_viva` lê BACKLOG e chama diretamente `H._itens_andaveis`; este usa `_item_parado`, o mesmo predicado de `_item_da_ordem_viva` e `_ids_que_nao_andam`. Não há derivação paralela nova de pertencimento à fila nessa costura. (`bin/disjuntor.py:1489`, `bin/disjuntor.py:1498`, `bin/hook_stop_fila1.py:158`, `bin/hook_stop_fila1.py:212`, `bin/hook_stop_fila1.py:218`, `bin/hook_stop_fila1.py:239`, `bin/hook_stop_fila1.py:333`)

**Andáveis e item em curso são perguntas distintas:** os andáveis vêm do bloco OBRAS; o item em curso vem do marcador `ORDEM-VIVA-TOPO`, sem fallback pela ordem física. A própria documentação da função explicita essa distinção. (`bin/hook_stop_fila1.py:180`, `bin/hook_stop_fila1.py:196`, `bin/hook_stop_fila1.py:221`, `bin/hook_stop_fila1.py:245`, `bin/hook_stop_fila1.py:1292`)

**B3 não é juiz de fila 1.** Seu universo vem de reprovações/triagens por obra e o juiz `estado_b3` devolve parada/livre/não sei; não consulta BACKLOG para decidir pertencimento à fila. Compartilha com I-1 o reconhecedor de identidade `_obra_de`, o que não transforma B3 em outro juiz da fila. (`bin/disjuntor.py:903`, `bin/disjuntor.py:909`, `bin/disjuntor.py:993`, `bin/disjuntor.py:511`, `bin/disjuntor.py:1604`)

**Fragilidade separada:** BACKLOG ausente vira texto vazio, logo conjunto de fila vazio e liberação silenciosa sem `CENSO ABRIU-SEM`; não é divergência de predicado, mas erro de fonte interpretado como “não abrangido”. (`bin/disjuntor.py:1493`, `bin/disjuntor.py:1496`, `bin/disjuntor.py:1498`, `bin/disjuntor.py:1507`)

## 09/11 02:20 · sessao d2c217883709 · FIO-E0-V4-E2 · construtor · origem=Code

Porte aplicado, com main preservado nos conflitos de contrato. `_C19` e `_C20` já estavam registrados em `core/juizes.py:291`. O horizonte de decisão/efeito permanece em `chamados/services/tarefa_do_dia.py:334` e `:357`.

Cada hunk E2, identificado pela linha antiga do anexo:

| Arquivo | Hunk | Resultado |
|---|---:|---|
| `chamados/juizes.py` | 926 | Portado em [juizes.py:948](/home/ronald/wt-codex-fio-e0-v4-e2-015740/app/chamados/juizes.py:948): avulso chama lote; Empresa, memo e tranca em lote em `:952` e `:961`. Dia preserva main; DECISAO abaixo. |
| mesmo | 1028 | Portado em `chamados/juizes.py:1118`: toque em lote. A entrada avulsa main permanece em `:1113`, também delegando ao lote. |
| mesmo | 1037 | Portado em `chamados/juizes.py:1130` e `:1157`: memo só do SIM e consultas por etapa, com corte por item. |
| `chamados/veredito_lavrado.py` | 87 | Imports portados em [veredito_lavrado.py:90](/home/ronald/wt-codex-fio-e0-v4-e2-015740/app/chamados/veredito_lavrado.py:90); assinatura de `expira_em`: DECISAO. |
| mesmo | 105 | DECISAO em `chamados/veredito_lavrado.py:137`; conversão local main mantida. |
| mesmo | 119 | Portado em `chamados/veredito_lavrado.py:147`: parâmetro `ate` e auxiliares. |
| mesmo | 134 | Portado em `chamados/veredito_lavrado.py:173` e `:176`: nascimento e retratação da batida. |
| mesmo | 150 | Portado em `chamados/veredito_lavrado.py:189`: corte individual dos cinco timestamps da célula. |
| mesmo | 160 | Portado em `chamados/veredito_lavrado.py:198`, `:203` e `:218`: materialização, resposta e rejeição de lavra futura. Relógio local já presente por `relogio_virou`, em `:222`; assinatura: DECISAO. |
| mesmo | 225 | Já presente em `chamados/veredito_lavrado.py:265`: lavra e expiração usam o mesmo instante, pela API main. |
| `chamados/services/raiox.py` | 32 | Portado em [raiox.py:35](/home/ronald/wt-codex-fio-e0-v4-e2-015740/app/chamados/services/raiox.py:35): horizonte compartilhado e lavra futura. Dia local já presente pelo helper main em `:41`. |
| `chamados/management/commands/backfill_veredito_lavrado.py` | 108 | DECISAO em [backfill_veredito_lavrado.py:111](/home/ronald/wt-codex-fio-e0-v4-e2-015740/app/chamados/management/commands/backfill_veredito_lavrado.py:111); arquivo mantido. |
| `escala/tests/test_signal_ancora.py` | 47 | Portado em [test_signal_ancora.py:50](/home/ronald/wt-codex-fio-e0-v4-e2-015740/app/escala/tests/test_signal_ancora.py:50): nenhuma pergunta viva, uma retratada e uma decisão `RETRATACAO`. |
| mesmo | 61 | Portado em `escala/tests/test_signal_ancora.py:71`: reutiliza a mesma verificação. |

Leitores diretos para orientar a suíte: [nao verificado]

| Função/campo | Leitores |
|---|---|
| `dia_da_pergunta_trancado` | `chamados/juizes.py:1297`; `chamados/models.py:1854`; `chamados/reconciliador.py:667` e `:747`; `chamados/services/disputa_emissao.py:1771`. |
| `dias_trancados` | `chamados/juizes.py:948`; `chamados/services/tarefa_do_dia.py:367`. |
| Auxiliares de Empresa/toque | `_empresa_ja_carregada`: `chamados/juizes.py:998`; toque avulso: `:1033`; toque em lote: `:1034` e `:1115`; trilha em lote: `:1149`. A catraca main nomeia o avulso em `chamados/tests/test_e1_v9.py:429`. |
| `marcos_do_lote` | `chamados/services/raiox.py:36`; `chamados/services/tarefa_do_dia.py:347`; `chamados/services/fio.py:249`; `chamados/management/commands/auditar_invariantes_chamados.py:1163`. |
| `esta_stale` / `relogio_virou` | `chamados/services/tarefa_do_dia.py:349`; auditor em `chamados/management/commands/auditar_invariantes_chamados.py:1167`; helper em `chamados/veredito_lavrado.py:222` e `chamados/services/raiox.py:41`. |
| `expira_em` | `chamados/veredito_lavrado.py:267`; `chamados/management/commands/backfill_veredito_lavrado.py:111`. |
| `lavrar` | `chamados/models.py:312`, `:3007`, `:3150`; `chamados/services/fio.py:923`, `:1033`; `chamados/services/relavra_via.py:126`; `chamados/management/commands/lavrar_perguntas_stale.py:87`. |
| `pks_stale_por_varredura` | `chamados/management/commands/lavrar_perguntas_stale.py:58`. |
| `julgado_em` / `janela_expira_em` | `chamados/veredito_lavrado.py:218`, `:222`, `:275`; `chamados/services/raiox.py:31`; backfill em `chamados/management/commands/backfill_veredito_lavrado.py:121`; captura de reversão em `chamados/services/passivo_validado.py:65`. |
| `veredito_lavrado` | `chamados/services/raiox.py:134`, `:183`; `chamados/services/tarefa_do_dia.py:348`; `chamados/services/relavra_via.py:130`; auditor em `chamados/management/commands/auditar_invariantes_chamados.py:1173`; backfill em `chamados/management/commands/backfill_veredito_lavrado.py:120`. |
| Retratação da âncora | Escritor E1 em `chamados/models.py:2065`; leitura temporal em `chamados/services/tarefa_do_dia.py:225` e `:338`; sinal em `escala/signals.py:161`; asserção em `escala/tests/test_signal_ancora.py:59`. |

Acrescentei seis testes RED e oito métodos de mutação, cobrindo 13 variantes, em `escala/tests/test_signal_ancora.py:101`. As mutações usam o fonte real e compilam somente em memória (`:82`). **Execução dos testes e morte dos mutantes: [nao verificado]**; não rodei a suíte.

Comandos individuais para o Code:

```bash
r() {
  PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
    --settings=config.settings_test \
    "escala.tests.test_signal_ancora.RebaseE2LeitoresTest.$1"
}

r test_RED_lavra_futura
r test_RED_varredura_no_mesmo_horizonte
r test_RED_avulso_em_lote_preserva_conversa_da_E1
r test_RED_toque_em_lote_memo_so_do_sim
r test_RED_marcos_cortam_cada_insumo
r test_RED_trilha_corta_cada_item_nas_tres_etapas

r test_MORDE_lavra_futura
r test_MORDE_varredura_sem_corte
r test_MORDE_varredura_aceita_lavra_futura
r test_MORDE_dia_civil_da_conversa
r test_MORDE_toque_inventado
r test_MORDE_marcos_sem_corte
r test_MORDE_celula_sem_corte_por_campo
r test_MORDE_toque_igual_a_tranca
```

A compilação dos cinco arquivos concluiu sem erro. Usei a API `py_compile` com escrita bloqueada para respeitar a proibição de bytecode; comando reproduzível:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B - <<'PY'
import py_compile
from unittest.mock import patch

arquivos = (
    'chamados/juizes.py',
    'chamados/veredito_lavrado.py',
    'chamados/services/raiox.py',
    'chamados/management/commands/backfill_veredito_lavrado.py',
    'escala/tests/test_signal_ancora.py',
)
with patch('py_compile.os.makedirs'), \
     patch('py_compile.importlib._bootstrap_external._write_atomic'):
    for arquivo in arquivos:
        py_compile.compile(arquivo, doraise=True)
        print('OK:', arquivo)
PY
```

As DECISOES de API abaixo continuam colidindo com expectativas E2 de argumento obrigatório e ausência total de relógio interno (`chamados/tests/test_e2_tarefa_do_dia.py:692`, `:1009`) e com chamadas que passam `date` (`:686`; `chamados/tests/test_selo_lavrado_orfaos.py:60`). Resultado dessas catracas: [nao verificado].

DECISAO chamados/juizes.py:949: lado main dia por `_dia_de_turno_da_pergunta`, conversa sem turno / lado E2 `data_evento` cru; mantido o dia main dentro do lote de um.

DECISAO chamados/veredito_lavrado.py:117: lado main `expira_em(data_evento, agora=None)`, recebendo instante / lado E2 `expira_em(data_evento, hoje)`, recebendo data obrigatória; mantido main.

DECISAO chamados/veredito_lavrado.py:137: lado main `_hoje(agora)`, com relógio da borda quando omitido / lado E2 `hoje` obrigatório fornecido pelo chamador; mantido main.

DECISAO chamados/veredito_lavrado.py:209: lado main argumentos opcionais e relógio compartilhado / lado E2 `marcos` e `agora` obrigatórios; mantido o contrato main, portando a rejeição de lavra futura quando `agora` é explícito.

DECISAO chamados/management/commands/backfill_veredito_lavrado.py:111: lado main `expira_em(p.data_evento, agora=agora)` / lado E2 `expira_em(p.data_evento, hoje)`; mantido main.

## 09/11 02:16 · sessao 4f480d1d7e3f · E1-FILA-V2-2-TRAVA · verificador · origem=Code

**Não aprovaria o aval completo nesta volta:** o fecho cobre as relações preexistentes examinadas, mas existe caminho por signal que chega à helper **depois de travar e escrever Ch**; além disso, a recusa escapa das views sem tratamento específico. (`app/chamados/services/disputa_emissao.py:958`, `app/chamados/models.py:290`, `app/chamados/models.py:296`, `app/chamados/signals.py:203`, `app/chamados/services/acoes_disputa.py:221`)

1. **TETO das escritas**

   Para **linhas preexistentes e cascatas executadas dentro do atomic**, não encontrei escrita fora das relações fechadas pelo algoritmo: dono de Ch; P por seu dono, dono da D, destino ou D alcançada; D por PK, pai alcançado ou aberta do dono. (`app/chamados/services/disputa_emissao.py:959`, `app/chamados/services/disputa_emissao.py:964`, `app/chamados/services/disputa_emissao.py:974`)

   | Escrita alcançável | Escritor efetivo | Por que pertence ao fecho |
   |---|---|---|
   | D: motivos/contexto da disputa existente | `app/chamados/services/disputa_emissao.py:1073`, `:1121`, `:1150`, `:1245`, `:296` | D aberta do dono ou D de P alcançada. `app/chamados/services/disputa_emissao.py:971`, `:975` |
   | Ch órfão: analisar/status | `app/chamados/services/disputa_emissao.py:1074`, `:1148`, `:1167`, `:1212`, `:1241`; persistência em `app/chamados/models.py:296` | Órfãos são sementes explícitas. `app/chamados/services/disputa_emissao.py:956` |
   | Ch pai reutilizado: contexto agregado pela nota | `app/chamados/services/disputa_emissao.py:1111` → `app/chamados/services/contexto.py:163` | Pai procurado entre VIVOS do colaborador. `app/chamados/services/disputa_emissao.py:1093`, `:1098`, `:960` |
   | P reutilizada: destino, célula, dia e lavra | `app/chamados/models.py:2960`, `:2985`, `:3009` | Todas as P dos donos alcançados entram, com suas D e destinos. `app/chamados/services/disputa_emissao.py:965`, `:971`, `:973` |
   | D: fechamento | `app/chamados/services/disputa_emissao.py:1273` → `app/chamados/models.py:1944` | D já alcançada; suas P entram pelo reverso da D. `app/chamados/services/disputa_emissao.py:966` |
   | S88: reconciliação do pai | `app/chamados/signals.py:42` → `app/chamados/models.py:296` | Pai de toda D alcançada entra, independentemente do status. `app/chamados/services/disputa_emissao.py:979` |
   | S143: carimbar P por destino e irmãs; reabrir pai; fechar D | `app/chamados/services/disputa_emissao.py:1844`, `:1869`, `:1918`, `:1945` | Reverso por destino, P por D, D abertas dos donos e respectivos pais. `app/chamados/services/disputa_emissao.py:965`, `:966`, `:975`, `:980` |
   | Relavra após mudança de status do Ch | `app/chamados/models.py:309` → `app/chamados/veredito_lavrado.py:255` | P por destino entra **sem filtro de dono**. `app/chamados/services/disputa_emissao.py:966` |
   | E2: carimbar P absolvida e fechar D | `app/chamados/reconciliador.py:595`, `:612` | E2 consulta pelo dono da D; esse predicado está incluído expressamente. `app/chamados/reconciliador.py:627`; `app/chamados/services/disputa_emissao.py:965` |
   | Ressuscitar: limpar P e reabrir D fechada | `app/chamados/models.py:1565`, `:1572`, `:2185` | P por destino, sua D, ambos os donos e pai entram transitivamente. `app/chamados/services/disputa_emissao.py:966`, `:969`, `:971`, `:978`, `:980` |
   | HIST: costurar em encerrado independente | `app/chamados/reconciliador.py:1582`, `:1638` → `app/chamados/services/fio.py:924` | ENCERRADOS de cada dono são sementes. `app/chamados/services/disputa_emissao.py:960` |
   | Gate de fechamento: reapontar âncora sem destino | `app/chamados/models.py:344`, `:437` → `app/chamados/services/fio.py:924` | D pelo pai alcançado, depois P pela D; sobrevivente VIVO também entra. `app/chamados/services/disputa_emissao.py:975`, `:966`, `:960` |
   | Cartório: reconciliar/renascer Ch, cluster e espelho do contexto | `app/ponto/services/cartorio.py:963`, `:994`; `app/ponto/services/cluster_espurio.py:124`, `:130`; `app/chamados/services/contexto.py:104` | Ch VIVOS/ENCERRADOS dos donos alcançados. `app/chamados/services/disputa_emissao.py:960` |
   | Fábrica: descarimbar P, reabrir D e gerar perguntas | `app/chamados/services/fabrica_da_celula.py:176`, `:149`, `:151`; `app/chamados/services/relavra_via.py:246`, `:261` | Consulta P pelo dono da D, coberto pelo fecho. `app/chamados/services/fabrica_da_celula.py:127`; `app/chamados/services/disputa_emissao.py:965` |
   | S144: carimbar pendentes aprovadas da D | `app/chamados/services/materializacao.py:1418`, `:1431` | Todas as P da D alcançada entram. `app/chamados/services/disputa_emissao.py:966` |

   Não identifiquei escrita de **C** nessas continuações; os pontos examinados travam C e persistem D/Ch/P. (`app/chamados/models.py:2674`, `app/chamados/models.py:296`, `app/chamados/models.py:1944`, `app/chamados/models.py:3264`)

   **Literalmente “tudo que escreve” exige duas ressalvas:** Ch/D/P novos não estão no conjunto prévio de PKs — existem INSERTs no ato — e a helper declara excluir `on_commit`. Há callbacks que fecham becos e retomam regularizações após commit, quando as travas anteriores já terminaram. (`app/chamados/services/disputa_emissao.py:950`, `:951`, `:1112`; `app/chamados/models.py:971`, `:3019`; `app/chamados/signals.py:132`, `:225`)

2. **Ordem das travas**

   **Dentro da helper**, a sequência é C ordenados; depois C reentrantes, D, Ch e P ordenados; crescimento posterior provoca recusa. As aquisições continuam concentradas em `trava_na_ordem`. (`app/chamados/services/disputa_emissao.py:1001`, `:1007`, `:1012`; `app/chamados/models.py:2674`, `:2683`, `:2696`, `:2700`)

   **No caminho completo, há pré-trava.** `_gravar_estado` abre atomic, trava C/D abertas/Ch e salva; o post_save chama cartório, cujo emissor pode chamar o repasse. (`app/chamados/models.py:280`, `:290`, `:296`; `app/chamados/signals.py:203`; `app/ponto/services/cartorio.py:1964`, `:1740`, `:1778`; `app/ponto/management/commands/detectar_ausencias.py:801`)

   **Contraexemplo construído:** C20 tem Ch101 vivo, de módulo diferente de `batida_ausente`, cancelável e com dia ainda furado. Há P301 de C20 em D201 fechada, cujo pai Ch102 pertence a C10. Cancelar Ch101 segura C20/Ch101; o signal emite a cobrança do dia e chega ao repasse; o fecho então descobre C10/D201. A sequência admite **C20 → Ch101 → C10 → D201**, sem crescimento entre as releituras da helper. Isso viola ordem por transação, embora cada aquisição use a autoridade correta. (`app/chamados/models.py:781`, `:782`, `:788`, `:711`; `app/chamados/services/disputa_emissao.py:965`, `:975`, `:1001`, `:1007`)

   Ocorrência desse cenário em produção: **[nao verificado]**. Aquisição posterior de **linha preexistente fora do fecho**, nas continuações internas examinadas: não encontrei; a falha demonstrada é a aquisição anterior ao topo pretendido. (`app/chamados/services/disputa_emissao.py:958`, `:1048`; `app/chamados/models.py:290`)

3. **Recusa e atomicidade**

   A recusa da helper acontece dentro do atomic próprio do repasse, antes das escritas de seu corpo. Portanto desfaz esse repasse/savepoint; não implica desfazer operações anteriores do chamador. (`app/chamados/services/disputa_emissao.py:1041`, `:1048`, `:1073`; `app/chamados/services/cobrar_dia.py:37`, `:39`, `:47`)

   | Chamador | Destino de `UniversoMudouSobATrava` |
   |---|---|
   | Fio/modal → ação de iniciar disputa | Escapa sem catch específico; pode resultar em 500. HTTP observado: **[nao verificado]**. `app/chamados/views.py:674`, `:954`; `app/chamados/services/fio.py:551`; `app/chamados/services/acoes_disputa.py:221` |
   | View “cobrar dia” | Escapa do serviço; log/toque e julgamento ocorreram antes. `app/chamados/views_cobrar.py:325`; `app/chamados/services/cobrar_dia.py:37`, `:39`, `:47` |
   | Comando de passivo | Interrompe o comando; não há retry local. Colaboradores anteriores foram processados em chamadas separadas. `app/chamados/management/commands/perguntar_chamados_sem_pergunta.py:29`; `app/chamados/services/pergunta_no_app.py:184`, `:186` |
   | Cron detectar ausências | Catch genérico escreve stderr e continua, sem retry. A cobrança já foi aberta. `app/ponto/management/commands/detectar_ausencias.py:693`, `:801`, `:802`, `:803` |
   | Cron alertas turno | Catch genérico escreve stdout, sem retry. `app/ponto/management/commands/processar_alertas_turno.py:328`, `:384`, `:385` |
   | Cron alertas avançados | Catch genérico escreve stdout, sem retry. `app/ponto/management/commands/processar_alertas_avancados.py:121`, `:158`, `:159` |
   | Signal indireto via cartório/emissor | O catch do emissor consome a recusa; o ato externo pode prosseguir. `app/ponto/management/commands/detectar_ausencias.py:802`; `app/chamados/signals.py:195` |

   Não é silêncio absoluto no código: há stderr/stdout/log. Coleta e visibilidade operacional dessas mensagens: **[nao verificado]**. Outras falhas de cascata também são consumidas por S88, S143 e pelo cartório; logo “ato mais todas as continuações completam ou desfazem juntos” não é garantia existente. (`app/chamados/signals.py:46`, `:116`, `:195`)

4. **Custo e ligação entre colaboradores**

   **Sim, pode alcançar grande parte do tenant:** basta uma cadeia de referências cruzadas legadas; cada novo dono acrescenta todos os seus Ch VIVOS+ENCERRADOS, todas as P pelo dono próprio ou da D e todas as D abertas. Não há recorte de dia/empresa nesses predicados. Tamanho real do componente em produção: **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:960`, `:965`, `:975`)

   O predicado exato é o ponto fixo destas relações:

   - Ch entra por `pk ∈ chids` **ou** `colaborador ∈ cids AND status ∈ VIVOS+ENCERRADOS`; acrescenta seu dono. (`app/chamados/services/disputa_emissao.py:960`, `:963`)
   - P entra por dono próprio, dono da D, destino alcançado, D alcançada ou PK já alcançada; acrescenta dono, D e destino. (`app/chamados/services/disputa_emissao.py:965`, `:966`, `:969`, `:971`, `:973`)
   - D entra por PK, pai alcançado ou aberta de dono alcançado; acrescenta dono e pai. (`app/chamados/services/disputa_emissao.py:975`, `:978`, `:980`)

   Assim, dois colaboradores se ligam por P com donos P/D diferentes, P apontando Ch estrangeiro, ou D com pai estrangeiro, inclusive transitivamente. **Compartilhar apenas dia ou empresa não cria aresta.** (`app/chamados/services/disputa_emissao.py:960`, `:965`, `:975`)

   Cada fecho faz três consultas por passada; a helper calcula o fecho três vezes. O limite de 128 passadas limita iterações, não cardinalidade nem duração das travas. Latência e plano SQL: **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:957`, `:959`, `:964`, `:974`, `:1000`, `:1002`, `:1009`)

5. **Classes de reprovação e mordida dos testes** [nao verificado]

   - **Segundo juiz/derivação paralela:** não identifiquei no diff. O cálculo decide alcance relacional; não reimplementa `decidir`, `pergunta_viva` ou `pode_fechar`, registrados nas autoridades existentes. (`app/chamados/services/disputa_emissao.py:958`; `app/core/juizes.py:261`, `:262`, `:266`, `:267`)
   - **Segundo escritor de trava fora da porta:** não identifiquei no diff; há, porém, a inversão por pré-travas descrita no item 2. (`app/chamados/services/disputa_emissao.py:1001`, `:1007`; `app/chamados/models.py:290`)
   - **Relógio fora da borda:** as funções acrescentadas não consultam relógio; 128 é limite operacional com recusa. (`app/chamados/services/disputa_emissao.py:942`, `:957`, `:985`)
   - **Os testes isolados mordem parte da cura:** executam fecho/helper reais e matam o predicado antigo e remoções de arestas. Mas as continuações a–c são reproduções manuais de aquisições/escritas. (`app/chamados/tests/test_e1_fila_v2_2_trava.py:111`, `:169`, `:265`, `:332`)
   - **Mutante sobrevivente concreto:** inserir `_trava_do_repasse = lambda *args: None` antes do `with` do agrupador desliga a proteção real e preserva a chamada nominal aceita pelo AST. O loader isolado não executa o agrupador; o selo verifica nome/posição, sem resolver sombreamento. (`app/chamados/services/disputa_emissao.py:1041`, `:1048`; `app/chamados/tests/test_e1_fila_v2_2_trava.py:111`; `app/chamados/tests/test_e1_volta2.py:184`)
   - Baseline e esse mutante deram 15/15 no ensaio em memória desta revisão; não há relatório persistido: **[nao verificado]**. A integração usa agrupador e escritores reais, mas começa na chamada direta do repasse, sem o prelúdio por signal; execução PostgreSQL: **[nao verificado]**. (`app/chamados/tests/test_e1_fila_v2_2_integracao.py:61`, `:75`, `:99`, `:123`, `:149`)
   - **VIVOS+ENCERRADOS não significa TERMINAIS.** Cancelado/superado ficam fora da semente, coerentemente com HIST e renascimento, que consultam ENCERRADOS. Se já forem órfão/pai/destino alcançado, entram por referência explícita. Portanto essa exclusão não demonstra quebra do aval de incluir terminal **reatável**. (`app/chamados/catalogo/motor.py:24`, `:30`, `:31`; `app/chamados/reconciliador.py:1582`; `app/chamados/models.py:921`, `:991`; `app/chamados/services/disputa_emissao.py:956`, `:960`, `:973`, `:980`)

6. **Quem escreve e quem lê os campos**

   Censo dos produtores e consumidores do caminho examinado; exaustividade de todos os leitores da aplicação: **[nao verificado]**.

   | Campo | Quem escreve/persiste | Quem lê no alcance auditado |
   |---|---|---|
   | Ch.`status_local` | Transição: `app/chamados/models.py:228`, `:296`; nascimento: `:938`, `:971`, `:1095` | Fecho: `app/chamados/services/disputa_emissao.py:960`; derivação: `:199`; HIST: `app/chamados/reconciliador.py:1583`; signals: `app/chamados/signals.py:70`, `:96`, `:192`; trava: `app/chamados/models.py:2691` |
   | D.`fechada_em` | Fechar: `app/chamados/models.py:1940`, `:1944`; reabrir admin: `:2157`, `:2169`; sistema: `:2181`, `:2185` | Fecho/repasse: `app/chamados/services/disputa_emissao.py:975`, `:1086`; S143: `:1807`; signals: `app/chamados/signals.py:28`, `:153`; trava: `app/chamados/models.py:2681` |
   | P.`chamado_destino` | Nascimento: `app/chamados/models.py:3016`, `:3019`; reuso: `:2985`, `:3009`; reapontamento: `app/chamados/services/fio.py:922`, `:924` | Fecho/repasse/S143: `app/chamados/services/disputa_emissao.py:966`, `:1059`, `:1805`; âncoras: `app/chamados/models.py:345`; fio exibido: `app/chamados/services/fio.py:117` |
   | P.`disputa` | Nascimento: `app/chamados/models.py:3016`, `:3019`; reuso retorna a canônica mantendo sua D: `:3010` | Fecho: `app/chamados/services/disputa_emissao.py:965`, `:967`; reuso/trava: `app/chamados/models.py:2957`; identidade: `:3430`; S144: `app/chamados/services/materializacao.py:1418` |
   | Ch.`colaborador` | Nascimento: `app/chamados/models.py:971`, `:1095` | Fecho: `app/chamados/services/disputa_emissao.py:960`, `:961`; HIST: `app/chamados/reconciliador.py:1583`; trava: `app/chamados/models.py:2691`; signal/cartório: `app/chamados/signals.py:203` |
   | D.`colaborador` | Fábricas: `app/chamados/services/disputa_emissao.py:587`, `:1112` | Fecho: `app/chamados/services/disputa_emissao.py:965`, `:975`, `:976`; E2: `app/chamados/reconciliador.py:627`; identidade P: `app/chamados/models.py:3430` |
   | P.`colaborador` | Inserção deriva da D e recusa divergência: `app/chamados/models.py:3420`, `:3430`, `:3432`; preenchimento histórico: `app/chamados/migrations/0051_decisao_pergunta.py:25` | Fecho: `app/chamados/services/disputa_emissao.py:965`, `:967`; costura/trava: `app/chamados/services/fio.py:863`, `:888` |

   As divergências legadas entre donos são tratadas pelo fecho sem presumir igualdade; os testes constroem essas divergências por `QuerySet.update`. Presença delas no dado real fornecido: **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:965`, `:969`, `:978`; `app/chamados/tests/test_e1_fila_v2_2_integracao.py:115`, `:144`)

## 09/11 02:14 · sessao aa869752624a · - · - · origem=Ronald (tmux)

Achados entregues ao pai:

- Os testes isolados mordem a implementação do fecho: executam as funções reais por AST e detectam a substituição pelo predicado antigo e a retirada de arestas específicas. Porém as continuações são simuladas, sem executar `reatar_pergunta_orfa`, `ressuscitar_perguntas` ou S143 reais. `app/chamados/tests/test_e1_fila_v2_2_trava.py:111`, `app/chamados/tests/test_e1_fila_v2_2_trava.py:169`, `app/chamados/tests/test_e1_fila_v2_2_trava.py:265`, `app/chamados/tests/test_e1_fila_v2_2_trava.py:332`.
- Mutante forte sobrevivente: inserir `_trava_do_repasse = lambda *args: None` imediatamente antes do `with` do repasse desliga a proteção real, preservando a chamada nominal examinada pelo selo. O teste AST não verifica resolução do nome ou sombreamento; os testes isolados carregam somente fecho/helper. `app/chamados/services/disputa_emissao.py:1041`, `app/chamados/services/disputa_emissao.py:1048`, `app/chamados/tests/test_e1_volta2.py:184`, `app/chamados/tests/test_e1_fila_v2_2_trava.py:111`.
- Executei baseline e esse mutante apenas em memória: ambos passaram nos 15 testes, sem falhas ou erros; não há relatório persistido dessa execução. [nao verificado] [nao verificado]
- Os testes de integração executam o agrupador e continuações reais, com comparação entre todas as linhas observadas e o prefixo da helper. Há substituição de gates para atingir os ramos. Resultado de execução PostgreSQL: [nao verificado]. `app/chamados/tests/test_e1_fila_v2_2_integracao.py:61`, `app/chamados/tests/test_e1_fila_v2_2_integracao.py:99`, `app/chamados/tests/test_e1_fila_v2_2_integracao.py:123`, `app/chamados/tests/test_e1_fila_v2_2_integracao.py:149`.

O uso de `VIVOS + ENCERRADOS` não troca o significado de `status_local`: são subconjuntos declarados pela fonte única. O HIST de `reatar_pergunta_orfa` consulta precisamente `ENCERRADOS`; a derivação consulta `VIVOS`. Cancelado/superado são terminais absolutos. Referências explícitas por órfão, destino ou pai continuam entrando independentemente do status. `app/chamados/catalogo/motor.py:24`, `app/chamados/catalogo/motor.py:103`, `app/chamados/reconciliador.py:1582`, `app/chamados/services/disputa_emissao.py:199`, `app/chamados/services/disputa_emissao.py:956`, `app/chamados/services/disputa_emissao.py:973`, `app/chamados/services/disputa_emissao.py:980`.

Censo dos campos no caminho examinado, sem alegar exaustividade global:

| Campo | Escritores | Leitores relevantes |
|---|---|---|
| Ch.`status_local` | Transição: `app/chamados/models.py:228`, persistida em `app/chamados/models.py:296`; nascimento: `app/chamados/models.py:971`, `app/chamados/models.py:1095` | Fecho: `app/chamados/services/disputa_emissao.py:960`; derivação: `app/chamados/services/disputa_emissao.py:199`; HIST: `app/chamados/reconciliador.py:1583`; universo da trava: `app/chamados/models.py:2691` |
| D.`fechada_em` | Fechar: `app/chamados/models.py:1940`; reabrir admin: `app/chamados/models.py:2157`; reabrir sistema: `app/chamados/models.py:2181` | Fecho: `app/chamados/services/disputa_emissao.py:975`; repasse: `app/chamados/services/disputa_emissao.py:1085`; trava: `app/chamados/models.py:2681` |
| P.`chamado_destino` | Nascimento: `app/chamados/models.py:3016`; reuso: `app/chamados/models.py:2985`; reapontamento: `app/chamados/services/fio.py:922` | Fecho: `app/chamados/services/disputa_emissao.py:966`; repasse: `app/chamados/services/disputa_emissao.py:1059`; gate: `app/chamados/models.py:345`; S143: `app/chamados/services/disputa_emissao.py:1805` |
| P.`disputa` | Nascimento: `app/chamados/models.py:3016`; reuso retorna a pergunta canônica, mantendo sua D: `app/chamados/models.py:3010` | Fecho lê FK e dono: `app/chamados/services/disputa_emissao.py:965`; trava de reuso: `app/chamados/models.py:2957`; chave: `app/chamados/models.py:3430` |
| Ch.`colaborador` | Nascimento: `app/chamados/models.py:971`, `app/chamados/models.py:1095` | Fecho: `app/chamados/services/disputa_emissao.py:960`; universo da trava: `app/chamados/models.py:2691` |
| D.`colaborador` | Nascimento no repasse: `app/chamados/services/disputa_emissao.py:1112`; outra fábrica: `app/chamados/services/disputa_emissao.py:587` | Fecho: `app/chamados/services/disputa_emissao.py:965`, `app/chamados/services/disputa_emissao.py:975`; chave da P: `app/chamados/models.py:3430` |
| P.`colaborador` | Save de inserção deriva de D e recusa divergência: `app/chamados/models.py:3420`, `app/chamados/models.py:3430`; migration histórica: `app/chamados/migrations/0051_decisao_pergunta.py:25` | Fecho: `app/chamados/services/disputa_emissao.py:965`; reapontamento: `app/chamados/services/fio.py:863`, `app/chamados/services/fio.py:888` |

O diff não cria outro juiz de estado, relógio ou escritor de campo de negócio: o fecho consulta relações; a helper delega aquisições à autoridade existente. O registro aponta os juízes atuais e `trava_na_ordem` concentra C/D/Ch/P. `app/chamados/services/disputa_emissao.py:958`, `app/chamados/services/disputa_emissao.py:1001`, `app/chamados/services/disputa_emissao.py:1007`, `app/core/juizes.py:260`, `app/chamados/models.py:2674`, `app/chamados/models.py:2683`, `app/chamados/models.py:2696`, `app/chamados/models.py:2700`.

## 09/11 02:14 · sessao 2518909c1faa · - · - · origem=Ronald (tmux)

No conjunto preexistente, não encontrei escrita fora das relações do fecho; a ressalva são linhas novas, excluídas expressamente do cálculo porque ainda não têm PK (`app/chamados/services/disputa_emissao.py:952`).

| Escrita alcançável | Ponto efetivo | Inclusão no fecho |
|---|---|---|
| D: motivos da disputa reusada/complementada | `app/chamados/services/disputa_emissao.py:1073`, `:1121`, `:1150`, `:1245` | D aberta do dono ou D de P apontando para órfão; regras em `:970`, `:983`. |
| D nova e Ch pai novo | `app/chamados/services/disputa_emissao.py:1093`, `:1112`; INSERT Ch em `app/chamados/models.py:971` | Novos; não possuem PK no topo (`disputa_emissao.py:952`). |
| Ch órfão: analisar/status | `app/chamados/services/disputa_emissao.py:1074`, `:1148`, `:1167`, `:1212`, `:1241`; save em `app/chamados/models.py:296` | Semente órfãos e universo VIVOS (`disputa_emissao.py:958`, `:962`). |
| Ch pai reusado: contexto pela nota | `app/chamados/services/disputa_emissao.py:1111` → `app/chamados/services/nota.py:110` → `app/chamados/services/contexto.py:163` | Pai selecionado VIVO do C (`disputa_emissao.py:1093`, `:1098`); semente VIVOS em `:962`. |
| P nova/reuso, celula/destino/lavra/rechaveamento | `app/chamados/models.py:2960`, `:2985`, `:3009`, `:3019` | P por dono e D respectiva entram em `disputa_emissao.py:967`; destinos em `:976`. |
| D: fechamento | `app/chamados/services/disputa_emissao.py:1273` → `app/chamados/models.py:1940`, `:1944` | D aberta/P alcançada; inclui suas irmãs via `disputa_emissao.py:968`. |
| S88: Ch pai reconciliado | `app/chamados/signals.py:42` → `app/chamados/models.py:654`, `:296` | Pai de D em `disputa_emissao.py:989`; dono do pai em `:965`. |
| S143: P por destino, irmãs, D e pai reaberto | `app/chamados/services/disputa_emissao.py:1844`, `:1869`, `:1918`, `:1945` | Reverso por destino `:968`, D de P `:974`, irmãs `:968`, pai `:989`; ambos donos `:972`, `:987`. |
| Relavra após mudar Ch | `app/chamados/models.py:309` → `app/chamados/veredito_lavrado.py:255` | Busca P por destino **sem filtro de dono** (`models.py:309`); coberta pelo reverso `disputa_emissao.py:968`. |
| E2: P absolvidas + D fechada | `app/chamados/reconciliador.py:595`, `:612` | Universo E2 por `disputa__colaborador` (`:627`), coberto explicitamente em `disputa_emissao.py:967`. |
| Ressuscitação: lote P + D reaberta + nota pai | `app/chamados/models.py:1565`, `:1572`, `:2185`, `:2188`; UPDATE P em `:3264` | P destino self sem filtro dono/D (`:1483`); reverso por destino, D, ambos donos e pai cobrem (`disputa_emissao.py:968`, `:974`, `:987`, `:989`). |
| HIST: costura em Ch encerrado independente | `app/chamados/reconciliador.py:1582`, `:1638` → `app/chamados/services/fio.py:922`, `:924` | Ch ENCERRADOS de cada dono (`disputa_emissao.py:962`). |
| Reata por gate antes do fechamento | `app/chamados/models.py:437` → `app/chamados/services/fio.py:924` | Âncoras sem destino por pai estão em `models.py:344`; fecho inclui D pelo pai (`disputa_emissao.py:983`), depois P por D (`:968`); sobrevivente é VIVO do dono (`models.py:390`). |
| Cartório: cluster fecha/abre Ch | `app/ponto/services/cartorio.py:994` → `app/ponto/services/cluster_espurio.py:124`, `:130` | Fecho de cluster escreve só Ch VIVOS do C (`cluster_espurio.py:66`); abertura só renasce ENCERRADOS (`models.py:921`), ambos sementes em `disputa_emissao.py:962`. |
| Cartório/emissor: contexto de marcos do Ch | `app/ponto/management/commands/detectar_ausencias.py:744` → `:558` → `app/chamados/services/contexto.py:104` | Apenas Ch vivo/renascido (`detectar_ausencias.py:743`), coberto por VIVOS+ENCERRADOS (`disputa_emissao.py:962`). |
| Fábrica: desfazer carimbo/reabrir D/gerar perguntas | `app/chamados/services/fabrica_da_celula.py:176`, `:149`, `:151` → `app/chamados/services/relavra_via.py:246`, `:261` | Filtro P por dono da D (`fabrica_da_celula.py:127`); ambos donos entram (`disputa_emissao.py:967`, `:972`, `:987`). |
| S144: carimbar P pendente de D | `app/chamados/services/materializacao.py:1418`, `:1431` | P por D em `disputa_emissao.py:968`; esse ramo não planta Batida (`materializacao.py:1408`). |

Não identifiquei escrita de Colaborador nessas cascatas; os pontos examinados adquirem a trava de C e escrevem D/Ch/P (`app/chamados/models.py:2674`, `:296`, `:1944`, `:3152`, `:3264`; `app/chamados/services/contexto.py:163`).

`cancelado`/`superado` não são sementes, mas isso coincide com os candidatos reais do HIST: `ENCERRADOS=('resolvido','fechado')`, enquanto TERMINAIS acrescenta aqueles dois (`app/chamados/catalogo/motor.py:30`). O HIST filtra **ENCERRADOS** (`app/chamados/reconciliador.py:1582`); `abrir` só tenta renascer ENCERRADOS (`app/chamados/models.py:921`), e cancelado/superado são absolutos pelo motor (`:991`). Se cancelado/superado já for órfão, pai ou destino alcançado, entra pelo PK sem filtro de status (`app/chamados/services/disputa_emissao.py:961`, `:976`, `:989`).

S88, S143 e cartório engolem erros com registro, portanto uma falha interna pode preservar o ato externo sem completar sua cascata (`app/chamados/signals.py:46`, `:116`, `:195`). Isso não constitui escrita fora do fecho, mas impede afirmar que o ato e todas suas continuações sempre completam juntos (`app/chamados/signals.py:41`, `:114`, `:195`).

## 09/11 02:13 · sessao b5e742ad675a · - · - · origem=Ronald (tmux)

- **O repasse tem atomic próprio e a helper antecede as escritas dele:** `app/chamados/services/disputa_emissao.py:1041`, `app/chamados/services/disputa_emissao.py:1048`. As três recusas saem da helper sem tratamento local: `app/chamados/services/disputa_emissao.py:985`, `app/chamados/services/disputa_emissao.py:1004`, `app/chamados/services/disputa_emissao.py:1013`.

- **Chamadores produtivos diretos e tratamento:**
  - Fio/modal: `app/chamados/views.py:674` / `app/chamados/views.py:954` → `app/chamados/services/fio.py:551` → `app/chamados/services/acoes_disputa.py:221`. Não há catch em torno da chamada; a exceção escapa da view. HTTP 500 é a consequência esperada do código; resposta observada em produção [nao verificado]. O middleware de mensagens apenas transforma respostas JSON já produzidas, sem tratar exceção: `app/core/middleware.py:24`.
  - Cobrar dia: view chama serviço em `app/chamados/views_cobrar.py:325`; serviço chama repasse em `app/chamados/services/cobrar_dia.py:47`, sem catch. Já registrou log e executou cartório antes: `app/chamados/services/cobrar_dia.py:37`, `app/chamados/services/cobrar_dia.py:39`; não há atomic englobante nesse serviço.
  - Passivo: `app/chamados/services/pergunta_no_app.py:186`, chamado pelo comando em `app/chamados/management/commands/perguntar_chamados_sem_pergunta.py:29`. Sem catch/retry; uma recusa interrompe o comando, e colaboradores processados anteriormente têm seus atos separados: `app/chamados/services/pergunta_no_app.py:184`.
  - Cron detectar ausências: chamada `app/ponto/management/commands/detectar_ausencias.py:801`; catch genérico escreve stderr em `app/ponto/management/commands/detectar_ausencias.py:802`.
  - Cron alertas turno: chamada `app/ponto/management/commands/processar_alertas_turno.py:384`; catch escreve stdout em `app/ponto/management/commands/processar_alertas_turno.py:385`.
  - Cron alertas avançados: chamada `app/ponto/management/commands/processar_alertas_avancados.py:158`; catch escreve stdout em `app/ponto/management/commands/processar_alertas_avancados.py:159`.
  - Esses catches não repetem a leitura/tentativa. Não são silenciosos no código; coleta operacional dessas saídas [nao verificado]. As cobranças já abertas antes do repasse não pertencem ao savepoint dele: `app/ponto/management/commands/detectar_ausencias.py:693`, `app/ponto/management/commands/processar_alertas_turno.py:328`, `app/ponto/management/commands/processar_alertas_avancados.py:121`.

- **Há pré-trava no caminho indireto por signal, portanto helper não é necessariamente o topo da transação.** `_gravar_estado` abre atomic e trava universo C/D/Ch antes de salvar: `app/chamados/models.py:280`, `app/chamados/models.py:290`, `app/chamados/models.py:296`. O post_save do Chamado chama cartório síncrono: `app/chamados/signals.py:188`, `app/chamados/signals.py:203`. Cartório usa detectar_ausencias como emissor: `app/ponto/services/cartorio.py:1964`; chama `_abrir_chamado`: `app/ponto/services/cartorio.py:1740` / `app/ponto/services/cartorio.py:1778`; emissor chama repasse: `app/ponto/management/commands/detectar_ausencias.py:801`.

- **Contraexemplo de ordem construído pelo código:** C20 tem Ch101 vivo de módulo diferente de `batida_ausente`, dia passado ainda furado e nenhuma P viva impedindo cancelamento; há uma D fechada/P antiga desse colaborador ou ligação legada com C10. `cancelar()` entra no atomic, recarrega sob C/D/Ch e chama `_gravar_estado`: `app/chamados/models.py:781`, `app/chamados/models.py:782`, `app/chamados/models.py:788`; recarregamento trava universo em `app/chamados/models.py:711`. O signal julga o dia, cria cobrança `batida_ausente`, chega ao repasse e sua helper adquire D fechada ou C10 após Ch já adquirido: `app/chamados/services/disputa_emissao.py:1001`, `app/chamados/services/disputa_emissao.py:1007`. Ocorrência desse estado em produção [nao verificado]. Se o cancelado fosse do próprio módulo deduplicado pelo emissor, ele seria devolvido sem renascimento e a função sairia antes do repasse: `app/chamados/models.py:921`, `app/ponto/management/commands/detectar_ausencias.py:739`.

- **Esse problema é ordem por transação; as aquisições continuam pela autoridade única.** Os quatro `select_for_update` estão em `app/chamados/models.py:2674`, `app/chamados/models.py:2683`, `app/chamados/models.py:2696`, `app/chamados/models.py:2700`; a helper só chama essa autoridade: `app/chamados/services/disputa_emissao.py:1001`, `app/chamados/services/disputa_emissao.py:1007`.

- **Signals e recusa:** no caminho indireto acima, o catch do emissor consome a recusa do repasse (`app/ponto/management/commands/detectar_ausencias.py:802`) antes de atingir o `engolir` exterior (`app/chamados/signals.py:195`). Se outra exceção escapar do cartório, `engolir` registra e suprime: `app/core/observ.py:85`, `app/core/observ.py:90`. Signals de Defesa 1 e S143 também têm catch genérico com log e atomic próprio: `app/chamados/signals.py:41`, `app/chamados/signals.py:46`, `app/chamados/signals.py:114`, `app/chamados/signals.py:116`.

## 09/11 02:09 · sessao 50eb9329224b · E1-FILA-V2-2-TRAVA · construtor · origem=Code

Implementei o fecho conservador e a recusa de crescimento, preservando `PerguntaDisputa.trava_na_ordem` como autoridade de aquisição. A certificação fica com Code/suite e opus final. ([cura](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/services/disputa_emissao.py:942)) [nao verificado]

Alterações:

- `disputa_emissao.py::fecho_do_repasse`: fecha C/D/Ch/P, incluindo ENCERRADOS, pais de D fechadas, destinos reversos, irmãs e donos; recusa após 128 passadas sem estabilizar. ([fonte](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/services/disputa_emissao.py:942))
- `disputa_emissao.py::_trava_do_repasse`: primeira aquisição só de C; recalcula, adquire D/Ch/P e relê antes de devolver. Crescimento gera `UniversoMudouSobATrava`. ([fonte](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/services/disputa_emissao.py:990))
- Acrescentei testes sem banco, integração PostgreSQL e a [entrega detalhada](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/docs/E1-FILA-V2-2-TRAVA.md:1). [nao verificado]

Dois desvios conservadores da matriz: expandir também os universos dos donos alcançados, porque E2 usa o dono da disputa; incluir D pelo pai alcançado, cobrindo âncoras sem destino. Os motivos e referências estão na [entrega:16](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/docs/E1-FILA-V2-2-TRAVA.md:16).

Os RED a–d falharam antes da cura no ensaio em memória:

- **a:** terminal independente adquirido depois de P. ([teste:221](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/tests/test_e1_fila_v2_2_trava.py:221)) [nao verificado]
- **b:** pai terminal de D fechada fora do topo. ([teste:224](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/tests/test_e1_fila_v2_2_trava.py:224)) [nao verificado]
- **c:** C/D/P estrangeiros adquiridos depois do topo. ([teste:227](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/tests/test_e1_fila_v2_2_trava.py:227)) [nao verificado]
- **d:** crescimento sem recusa; agora verifica C antes de D e crescimento final nas quatro classes. ([teste:231](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/tests/test_e1_fila_v2_2_trava.py:231)) [nao verificado]
- **e/MORDE:** substituir o fecho pelo antigo faz a–c acusarem; há também mutantes sem terminais, pais, reversos e conferência final. ([teste:265](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/tests/test_e1_fila_v2_2_trava.py:265)) [nao verificado]

Após a cura, passaram **15 testes sem banco**, **9 da catraca existente isolada de Django** e `py_compile` nos três arquivos Python, com gravação de cache desativada. Comandos individuais dos RED/mutantes e da compilação estão na [entrega:48](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/docs/E1-FILA-V2-2-TRAVA.md:48). Comando conjunto: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest chamados.tests.test_e1_fila_v2_2_trava -v
```

O mapa **QUEM ESCREVE / QUEM LÊ** cobre cada campo persistente consumido pelo diff:

| Campos | Escritores | Leitor no diff |
|---|---|---|
| C.pk | `criar_colaborador` | semente C |
| D.pk, colaborador, chamado | emissão, agrupador, âncora; FK do pai | expansão D/donos/pais |
| D.fechada_em | nascimento, fechar, reaberturas | seleção das abertas |
| Ch.pk, colaborador | `abrir`/`criar` | sementes e donos |
| Ch.status_local | nascimento e `_gravar_estado` | VIVOS + ENCERRADOS |
| P.pk, disputa | `_nascer` | irmandade e alcance D |
| P.colaborador | `_chave_de_identidade` | seleção direta e donos |
| P.chamado_destino | `_nascer`, reapontamento, FK | destinos e relação reversa |

Cada célula tem os endereços de escritor e leitor conferidos na [matriz arquivo:linha](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/docs/E1-FILA-V2-2-TRAVA.md:134).

Outras portas devolvidas à fila, sem implementação neste diff:

- Declaração/aplicação: expansão da validação posterior. ([fila:151](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/docs/E1-FILA-V2-2-TRAVA.md:151))
- `validar_pergunta`: cascatas da materialização. ([fila:152](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/docs/E1-FILA-V2-2-TRAVA.md:152))
- `reabrir_orfas_enterradas`: outras P ressuscitadas. ([fila:153](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/docs/E1-FILA-V2-2-TRAVA.md:153))
- Portas de emissão/agregação/âncora: terminais descobertos pelo reatamento. ([fila:154](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/docs/E1-FILA-V2-2-TRAVA.md:154))
- Fechar/S143/ressuscitar: conjuntos próprios e relações reversas. ([fila:155](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/docs/E1-FILA-V2-2-TRAVA.md:155))
- `migrar_resolvedoras_vivas`: preservar recusa; mesma lacuna não demonstrada. ([fila:156](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/docs/E1-FILA-V2-2-TRAVA.md:156))
- `reapontar_resolvedora`: atomic do chamador. ([fila:157](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/docs/E1-FILA-V2-2-TRAVA.md:157))
- Rechaveamento/retratação: separar efeitos posteriores. ([fila:158](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/docs/E1-FILA-V2-2-TRAVA.md:158))
- Migração/reversão de `data_evento`: mesma análise do rechaveamento. ([fila:159](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/docs/E1-FILA-V2-2-TRAVA.md:159))

Integração PostgreSQL, regressões com banco, concorrência e custo: **[nao verificado]**. Callbacks `on_commit`, modelos auxiliares, FCM e linhas novas ficam fora deste teto. ([limites](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/docs/E1-FILA-V2-2-TRAVA.md:161))

## 09/11 01:10 · sessao 62a6fe9645f5 · E1-FILA-V2-2-TRAVA · censo · origem=Code

**O conjunto inicial não cobre todo o alcance possível do ato.** Há caminhos para travar/escrever chamados terminais que não eram órfãos nem destinos previamente coletados, e para alcançar perguntas por relações sem filtro de colaborador. Isso é demonstrável no código; ocorrência concreta, quantidade e colaboradores afetados em produção: **[nao verificado]**. (`chamados/reconciliador.py:910–954`, `:1582–1644`; `ponto/services/cluster_espurio.py:127–147`; `chamados/services/disputa_emissao.py:1747–1751`)

As referências abaixo são relativas a `app/`. O anexo manda consultar o código vivo quando houver divergência. (`docs/JUIZES.md:14`)

**1. Escritores e travas depois de `_trava_do_repasse`**

Chamando o colaborador do ato de `c` e os órfãos recebidos de `O`, o conjunto efetivamente travado hoje é:

| Classe | Predicado inicial |
|---|---|
| Colaborador | Somente `pk=c`. (`chamados/services/disputa_emissao.py:954`, `:958`) |
| Pergunta | `colaborador_id=c OR chamado_destino_id IN O`, sem restrição de dia, disputa ou materialização. (`chamados/services/disputa_emissao.py:956–959`) |
| Disputa | Disputas dessas perguntas, abertas ou fechadas, mais todas as abertas de `c`. (`chamados/services/disputa_emissao.py:958`; `chamados/models.py:2679–2684`) |
| Chamado | `O`, destinos dessas perguntas, todos os vivos de `c` e pais das disputas travadas **que estejam abertas**. Uma disputa fechada explicitada não acrescenta automaticamente seu pai. (`chamados/services/disputa_emissao.py:959`; `chamados/models.py:2685–2696`) |

A autoridade ordena cada tabela por PK, mas não verifica se os IDs recebidos pertencem ao colaborador informado. (`chamados/models.py:2674`, `:2683`, `:2696`, `:2700–2701`)

O censo do corpo e das cascatas síncronas é:

| Linha/modelo alcançado | Cadeia e operação | Está no conjunto inicial? |
|---|---|---|
| **Disputa da pergunta já costurada ao órfão** | Busca por destino → complementa perguntas → grava `motivos`. (`chamados/services/disputa_emissao.py:1002–1016`) | A disputa encontrada entra pela mesma relação de destino usada no topo. Seu colaborador não é acrescentado por isso. (`chamados/services/disputa_emissao.py:956–959`) |
| **Disputa aberta de `c`** | Busca aberta → grava motivos; também pode gravar contexto na cauda de entradas pendentes. (`chamados/services/disputa_emissao.py:1028–1030`, `:1063–1064`, `:1186–1202`, `:293–296`) | Sim, se preexistente e aberta no topo. (`chamados/models.py:2681–2684`) |
| **Disputa nova e chamado-pai novo/reutilizado** | `Chamado.abrir` → eventual nota/fusão de contexto → `Disputa.objects.create`. (`chamados/services/disputa_emissao.py:1036–1060`) | Pai reutilizado vivo de `c`: sim. Linhas novas ainda não existiam para serem travadas. (`chamados/models.py:2691`, `:971`; `chamados/services/disputa_emissao.py:1055`) |
| **Órfãos recebidos** | `analisar()` nos ramos complemento, intervalo, ata, episódio e fallback → `_gravar_estado` → `save`. (`chamados/services/disputa_emissao.py:1017`, `:1091`, `:1110`, `:1155`, `:1184`; `chamados/models.py:800–806`, `:290–296`) | Seus IDs estão explicitados no topo. (`chamados/services/disputa_emissao.py:955`, `:959`) |
| **Pergunta nova ou canônica reutilizada** | `_criar_pergunta_se_couber`/`gerar_perguntas_disputa` → `Pergunta.abrir` → `_nascer`; insere nova ou grava célula, destino e lavra da existente. (`chamados/services/disputa_emissao.py:834–843`, `:439–452`; `chamados/models.py:2985–3019`) | Existente com coluna `colaborador_id=c`: sim. Nascimento com disputa de outro dono pode tomar outro Colaborador depois do topo. (`chamados/services/disputa_emissao.py:956`; `chamados/models.py:2853`) |
| **Pergunta reutilizada com dia incorreto; sua disputa e destinos** | `_nascer` → trava C/D/Ch/P → `rechavear_no_reuso` → `rechavear_data_evento` → grava dia/célula/lavra; acorda célula antiga. (`chamados/models.py:2948–2965`; `chamados/services/fio.py:1001–1003`, `:1033–1039`) | Cobertos quando a pergunta entrou no topo: este já inclui sua disputa e destino atual; o destino pedido é órfão/vivo. Isso não cobre automaticamente donos divergentes. (`chamados/services/disputa_emissao.py:956–959`) |
| **Pergunta órfã criada pela cauda** | `perguntar_entradas_pendentes` → `gerar_perguntas_disputa` → `reatar_pergunta_orfa`. (`chamados/services/disputa_emissao.py:294`, `:459–464`, `:1202`) | A pergunta é nova; os chamados que o reatamento descobrir não estão necessariamente no topo. (`chamados/reconciliador.py:1582–1585`, `:910–921`) |
| **Destino vivo derivado; pergunta reapontada** | Derivador busca vivos do dono da disputa → `reapontar_resolvedora` → trava C/Ch, eventualmente D/P da colisão → grava pergunta. (`chamados/services/disputa_emissao.py:198–201`; `chamados/reconciliador.py:1552–1553`; `chamados/services/fio.py:889–925`) | Vivo de `c`: sim. Se o dono da disputa divergir de `c`, a cobertura não decorre do topo. (`chamados/models.py:2691`; `chamados/services/disputa_emissao.py:958`) |
| **Destino histórico encerrado** | Reatamento busca histórico → costura no histórico único → chama resolver. A costura trava o chamado mesmo sem alterar seu status. (`chamados/reconciliador.py:1582–1585`, `:1638–1644`; `chamados/services/fio.py:768`) | **Não necessariamente:** pode não ser órfão, vivo, pai aberto nem destino anterior de pergunta coletada. (`chamados/services/disputa_emissao.py:956–959`; `chamados/models.py:2687–2691`) |
| **Chamado do dia renascido pelo c2** | Reatamento sem histórico → c1 ou c2 → `abrir(colaborador, módulo, data)` sem status → renascimento → costura. (`chamados/reconciliador.py:1615–1618`, `:910–954`; `chamados/models.py:906`, `:921–923`) | **Não necessariamente**, pela mesma ausência dos terminais independentes no predicado inicial. (`chamados/models.py:2687–2691`) |
| **Disputa fechada pelo próprio agrupador/c1** | `pode_fechar` → `fechar` → trava universo C/D/Ch → grava fechamento. (`chamados/services/disputa_emissao.py:1210–1219`; `chamados/reconciliador.py:405–413`; `chamados/models.py:1918–1920`, `:1940–1944`) | Disputa aberta preexistente de `c`: sim; disputas novas não existiam no topo. Pais/relações de outro dono exigem análise relacional adicional. (`chamados/models.py:2681–2687`) |
| **Pai pelo S88** | `post_save(Disputa)` → busca pai só por PK → `reconciliar` → nota. O gate pode travar novamente C, abertas e chamados. (`chamados/signals.py:18–45`; `chamados/models.py:711`) | Pai de disputa aberta no topo: sim. FK para outro dono ou disputa que só abriu posteriormente: cobertura não garantida. (`chamados/models.py:2685–2691`) |
| **Perguntas pelo S143/S128** | Chamado encerrado → resolver → busca por destino → carimba P → pode reabrir pai, carimbar irmãs e fechar disputas. (`chamados/signals.py:90–115`; `chamados/services/disputa_emissao.py:1721`, `:1747–1751`, `:1787`, `:1804–1814`, `:1827–1888`) | P de `c`: sim. P estrangeira ligada a chamado alcançado fora de `O`, suas irmãs e sua disputa: **não necessariamente**. (`chamados/services/disputa_emissao.py:956`, `:1747–1751`, `:1836–1861`) |
| **Perguntas pelo S144** | `post_save(Disputa fechada)` → todas as pendentes daquela disputa → `carimbar` → `save`. Esse receiver não planta batida. (`chamados/signals.py:147–163`; `chamados/services/materializacao.py:1408–1431`; `chamados/models.py:3152`) | Sim para as P coletadas pelo topo; não há filtro pela coluna colaborador na consulta do S144. (`chamados/services/materializacao.py:1418–1419`) |
| **Perguntas relavradas pela mudança de estado** | `_gravar_estado`, cruzando a fronteira de `ENCERRADOS`, busca P por destino sem dono → `lavrar` → salva lavra. (`chamados/models.py:273`, `:303–312`; `chamados/veredito_lavrado.py:234–255`) | P de `c`: sim. P estrangeira em chamado alcançado fora de `O`: **não necessariamente**. (`chamados/services/disputa_emissao.py:956–959`) |
| **Urgência do próprio chamado** | `post_save(Chamado encerrado)` → `_rebaixar_sem_post_save`. (`chamados/signals.py:53–72`) | A cobertura é a do próprio chamado; não amplia por si só o conjunto de chamados. (`chamados/signals.py:72`) |

O cartório amplia o alcance acima:

| Caminho do cartório | Escrita/trava e cobertura |
|---|---|
| **Entrada síncrona** | Nascimento/mudança de status → `julgar_celula(colaborador, dia)` → julgamento da célula. (`chamados/signals.py:187–203`; `ponto/services/cartorio.py:1976–1991`) |
| **E2, colaborador inteiro** | Busca P por `disputa__colaborador`, inclusive disputas fechadas → carimba → eventualmente fecha disputa. Se a coluna P.colaborador divergir da disputa, o topo pode não ter coletado a P. (`ponto/services/cartorio.py:958–963`; `chamados/reconciliador.py:593–614`, `:627–641`; `chamados/services/disputa_emissao.py:956`) |
| **Fecho e reatamento dos vivos** | Reconcilia chamados vivos, podendo passar novamente por S143 e pela costura anterior ao fechamento. Vivos de `c` estão no topo; suas perguntas estrangeiras não são coletadas automaticamente. (`ponto/services/cartorio.py:1488`, `:1501`, `:1522`; `chamados/models.py:435–437`, `:2691`) |
| **Ressuscitação de perguntas** | Chamado → relação reversa `perguntas_que_resolvem`, sem dono → trava suas D/P → registra decisões antigas → descarimba em lote → reabre disputas fechadas. A guarda do lote pode recusar dono não travado; ela não demonstra cobertura do conjunto inicial. (`chamados/models.py:1483–1489`, `:1550–1572`, `:3258–3266`) |
| **Pai de disputa reaberta** | Depois da ressuscitação, uma nova chamada com `abertas_do_colaborador=True` passa a incluir o pai dessa disputa, mesmo terminal. Ele podia faltar no topo quando a disputa estava fechada. (`chamados/models.py:1571–1572`, `:2685–2687`; `chamados/services/disputa_emissao.py:530`) |
| **Fábrica após lavra** | Desfaz carimbos, trava C/D/P, cria/agrega disputa e gera perguntas. No julgamento, o primeiro descarimbo usa `reabrir_fechada=False`; o helper `perguntar` também chama o descarimbador por motivo. (`ponto/services/cartorio.py:985–997`; `chamados/services/fabrica_da_celula.py:145–151`, `:174–186`; `chamados/services/relavra_via.py:230–246`) |
| **Emissor do chamado do dia** | `abrir` por colaborador/módulo/data **sem filtro de status**; pode renascer terminal não coletado e depois atualizar seus marcos/contexto. (`ponto/services/cartorio.py:1740–1742`, `:1778–1789`; `ponto/management/commands/detectar_ausencias.py:693–713`, `:743–744`) |
| **Cluster de outros dias** | Julgamento → cluster → janela de 14 dias → `abrir` sem status; fecha vivos ou tenta renascer encerrados daquelas chaves. Terminais independentes das relações iniciais faltam no topo. (`ponto/services/cartorio.py:990–994`; `ponto/services/cluster_espurio.py:12`, `:40–49`, `:123–147`) |
| **Limbo** | Emissor → `agrupar_furo` → trava C/D abertas/Ch furo+pai → contexto, reapontamento, reconciliação e nota. (`ponto/management/commands/detectar_ausencias.py:760–761`; `chamados/services/limbo_folgas.py:274–292`) |

Há ainda escritas em modelos fora das quatro tabelas da autoridade:

| Modelo | Onde é escrito; cobertura pelo topo |
|---|---|
| `CelulaDia` | Lavra/carimbo/precedência do cartório → `save`; **não está no topo**. (`ponto/services/cartorio.py:1802`, `:1822`, `:1997`; `ponto/portas/celula.py:900`, `:1067`, `:1082`) |
| `EventoCelula` | INSERT pelo save da célula, nascimento de chamado e mudanças de pergunta; **não está no topo**. Emitir o evento não equivale a consumir o reconciliador naquela chamada. (`ponto/portas/celula.py:882–902`; `chamados/signals.py:237–244`; `chamados/models.py:3239–3248`) |
| `DecisaoPergunta` | Ressuscitação preserva a projeção anterior antes do descarimbo; **não está no topo**. (`chamados/models.py:1563–1565`) |
| `FollowupChamado` | Notas de agrupamento, renascimento, fechamento e reabertura → INSERT. Só há escrita do Chamado pela nota quando existe `contexto_novo`. (`chamados/services/nota.py:102–123`) |
| `LogAuditoria` | Eventos/trilhas convergem ao INSERT; **não está no topo**. (`core/observ.py:134–137`) |
| `User` sistema | `get_user_sistema` pode inserir por `get_or_create`; **não está no topo**. (`core/utils.py:18`) |
| `Pauta` e `ContadorPauta` | Limbo exportado → pauta → contador departamental, com criação/UPDATE; **não estão no topo**. (`chamados/services/fabrica_da_celula.py:181–182`, `:245–251`; `pautas/services.py:180–188`, `:352–357`) |
| `PushSubscription` | Push pode apagar inscrição expirada; **não está no topo**. (`chamados/push_utils.py:24`, `:49–51`) |
| `Colaborador`, campos FCM | Push pode limpar token em lote, inclusive de outro colaborador; cobertura não garantida. (`chamados/fcm_utils.py:41`, `:89–94`; `colaboradores/fcm.py:60–62`; `colaboradores/services/colaborador.py:88`) |

Os callbacks precisam de uma fronteira separada: são agendados por `on_commit`, portanto as travas da transação original não protegem suas transações posteriores. (`chamados/signals.py:144`, `:225`, `:293`, `:356`)

| Callback | Alcance posterior |
|---|---|
| **Beco da pergunta** | Reabre uma transação, trava universo D/Ch, resolve destinos terminais e fecha D. (`chamados/signals.py:214–225`; `chamados/services/becos.py:41–57`, `:72–78`) |
| **Isento** | Percorre todos os vivos do colaborador e escreve o véu de silêncio, com trava/save por chamado. (`chamados/signals.py:269–293`; `chamados/services/silencio_isentos.py:24–31`, `:41–58`; `chamados/services/arquivo.py:170–173`) |
| **Filho morto** | Chama o reconciliador completo: E2, reatamento e retratação dos vivos. A retratação também pode rejeitar a `Justificativa` relacionada. (`chamados/signals.py:337–356`; `chamados/reconciliador.py:1428–1437`, `:1449–1460`, `:529–540`) |
| **Regularizações confirmadas** | Pode gravar `RegularizacaoItemBatida`/`RegularizacaoExterna`, criar/corrigir/apagar Batida e encerrar seu chamado. Batida dispara novamente núcleo, cartório e reconciliador. (`chamados/signals.py:129–144`; `chamados/services/regularizacao_ext.py:127–205`, `:218–224`; `ponto/signals.py:26–50`, `:135–140`, `:215`) |
| **Derivações da Batida retomada** | Reconstrói `TurnoMaterializado`, emite eventos e agenda recalcular fechamento; este grava `FechamentoMensal` e substitui `DiaPago`. Outros efeitos internos da derivação financeira: **[nao verificado]**. (`ponto/nucleo.py:59–93`; `ponto/services/fechamento.py:898`, `:1144`, `:1160`, `:1191–1201`, `:1810–1824`; `ponto/services/dia_pago.py:359–360`) |

**2. Dono: pode alcançar outro colaborador?**

**Sim, condicionalmente às relações e argumentos existentes.** Os caminhos abaixo não permitem concluir que isso ocorreu em produção: **[nao verificado]**.

| Acesso sem filtro suficiente de dono | Consequência possível |
|---|---|
| Complemento: `filter(chamado_destino=ch, materializada_em__isnull=True)` | Obtém a disputa pela pergunta encontrada, sem exigir que seja de `c`. O topo inclui essa D/P, mas não seu Colaborador. (`chamados/services/disputa_emissao.py:1002–1016`, `:956–959`) |
| S143: `filter(chamado_destino=chamado, …)` | Pode carimbar P estrangeira, alcançar sua disputa, irmãs e pai. (`chamados/services/disputa_emissao.py:1747–1751`, `:1787`, `:1804–1814`, `:1827–1861`) |
| Relavra: `filter(chamado_destino_id=self.pk, validada_em__isnull=True)` | Pode salvar lavra de P estrangeira ao mudar o status do chamado. (`chamados/models.py:309–312`; `chamados/veredito_lavrado.py:255`) |
| Ressuscitação: `self.perguntas_que_resolvem` | Pode alcançar D/P de outro dono. O lote tem guarda de colaboradores travados e pode recusar; não há filtro de proprietário na seleção reversa. (`chamados/models.py:1483–1489`, `:1550–1565`, `:3258–3260`) |
| S88: `filter(pk=instance.chamado_id)` | Pode entrar no gate do pai de outro colaborador se a FK estiver assim. (`chamados/signals.py:32–42`) |
| FCM: `Colaborador.objects.filter(fcm_token=fcm_token)` | Pode atualizar outros colaboradores com o mesmo token, sem filtro por `c`. (`chamados/fcm_utils.py:41`; `colaboradores/fcm.py:55–62`) |
| Regularização pós-commit: `item.batida_alvo` | A operação usa a Batida relacionada sem conferir ali o colaborador; uma relação cruzada pode disparar cascatas do outro dono. (`chamados/services/regularizacao_ext.py:127–155`) |

As portas atuais recusam **nova costura** para destino de outro dono, mas isso não valida universalmente relações já gravadas nem os argumentos da porta externa. (`chamados/models.py:2848`; `chamados/services/fio.py:863–865`, `:1575–1579`)

O próprio nascimento de P verifica igualdade entre sua coluna colaborador e a disputa; essa verificação só roda quando `_state.adding`. Integridade de todo o acervo existente: **[nao verificado]**. (`chamados/models.py:3420–3423`, `:3430–3435`)

**3. Chamado terminal pode voltar a ser escrito? Entra hoje?**

**Sim, especialmente `resolvido`/`fechado`.** Estes são `ENCERRADOS`; `cancelado`/`superado` também são terminais, mas o renascimento não os reabre. (`chamados/catalogo/motor.py:24–32`; `chamados/models.py:991–993`, `:1019`)

Há três cadeias independentes:

- **Reatar histórico:** cauda do agrupador → geração → reatamento → busca histórico encerrado → costura → trava do chamado → resolver. Aqui ele pode apenas ser travado, sem renascer. (`chamados/services/disputa_emissao.py:1202`, `:294`, `:459–464`; `chamados/reconciliador.py:1582–1585`, `:1638–1644`; `chamados/services/fio.py:768`)
- **Renascer chamado do dia:** reatamento → c2 → `abrir` sem filtro de status → `_renascer_por_premissa_viva` → `reabrir` → grava estado → ressuscita perguntas. (`chamados/reconciliador.py:1617`, `:910–921`; `chamados/models.py:921–923`, `:1019`, `:1442–1451`)
- **Renascer pela cascata do cartório:** mudança de status → cartório → emissor do dia ou cluster de outros dias → `abrir` sem status → mesmo renascimento. (`chamados/signals.py:203`; `ponto/services/cartorio.py:990–994`, `:1778–1780`; `ponto/management/commands/detectar_ausencias.py:693–713`; `ponto/services/cluster_espurio.py:130–147`)

**Ele só entra hoje se coincidir com alguma relação inicial:** órfão explícito, destino coletado ou pai de disputa aberta travada. O ramo “todos do colaborador” inclui somente `VIVOS`; não cobre todos os terminais dessas chaves. (`chamados/services/disputa_emissao.py:955–959`; `chamados/models.py:2685–2696`)

O filtro direto da fábrica que rejeita `ENCERRADOS` não elimina esses caminhos indiretos. (`chamados/services/disputa_emissao.py:755–756`; `chamados/reconciliador.py:910–921`)

**4. Fecho mínimo e computabilidade**

Para as **linhas preexistentes das quatro entidades**, o mínimo precisa ser um **fecho relacional dos caminhos efetivamente alcançáveis**, não apenas “abertas/vivos de `c`”. As relações necessárias são:

1. Colaboradores donos de cada Chamado, Disputa e Pergunta alcançada — considerando tanto a coluna da P quanto sua disputa, sem presumir igualdade no legado. (`chamados/models.py:2853`; `chamados/services/fio.py:889`, `:1001`; `chamados/reconciliador.py:628`)
2. Disputas das perguntas alcançadas e abertas dos colaboradores cujo universo for solicitado; também as fechadas que a ressuscitação pode reabrir. (`chamados/models.py:1562–1572`, `:2681–2684`)
3. Chamados recebidos, destinos, pais das disputas alcançadas **inclusive as inicialmente fechadas que podem abrir**, vivos dos universos solicitados e candidatos históricos/terminais dos lookups do reatamento, emissor e cluster. (`chamados/models.py:2685–2691`; `chamados/reconciliador.py:1582–1585`, `:910–921`; `ponto/services/cluster_espurio.py:130–147`)
4. Perguntas alcançadas por coluna colaborador, por dono da disputa, por destino reverso, por âncora no pai sem destino e por irmandade dentro de disputa; repetir as relações enquanto surgirem novos membros. (`chamados/services/disputa_emissao.py:956`, `:1747–1751`, `:1836`; `chamados/reconciliador.py:628`; `chamados/models.py:345`, `:1483–1489`)
5. Se a notificação fizer parte do ato, colaboradores selecionáveis pela mesma chave FCM, além dos modelos auxiliares efetivamente escritos. (`chamados/fcm_utils.py:41`; `colaboradores/fcm.py:60–62`)

Esse é o predicado de **menor fecho por a

[truncado em 20000 de 33469 caracteres]

## 09/11 01:04 · sessao 35a2cddeaac6 · DISPARAR-PERGUNTAS-NO-CONTRATO-20 · censo · origem=Code

## (1) Conceito de fio mudo e comparação com os juízes

**O comando usa os juízes de vitalidade existentes, mas seu universo emitível não é simplesmente “todo chamado vivo sem pergunta viva”.** Ele acrescenta janela, célula, motivo vencido, situação do colaborador e dedup por identidade do fato. (`app/chamados/management/commands/disparar_perguntas_competencia.py:141`, `app/chamados/management/commands/disparar_perguntas_competencia.py:154`, `app/chamados/management/commands/disparar_perguntas_competencia.py:215`)

| Pergunta | Autoridade registrada | Uso no comando |
|---|---|---|
| Chamado vivo? | `_C1 → VIVOS`. `VIVOS = aberto, em_analise, registrado`; `COBRANCA = aberto, em_analise`. (`app/core/juizes.py:261`; `app/chamados/catalogo/motor.py:24`) | `status_local__in=VIVOS`, com célula dentro de `ini/fim`. (`app/chamados/management/commands/disparar_perguntas_competencia.py:141`) |
| Pergunta viva? | `_C6 → chamados.juizes.pergunta_viva`. (`app/core/juizes.py:266`) | Importa a reexportação de `models` e aplica a cada pergunta ligada por `chamado_destino`. (`app/chamados/models.py:4062`; `app/chamados/management/commands/disparar_perguntas_competencia.py:133`, `app/chamados/management/commands/disparar_perguntas_competencia.py:148`) |
| Alguma pergunta da coleção viva? | `_C15 → fio_vivo`. (`app/core/juizes.py:275`) | Repete `any(pergunta_viva(...))`, equivalente ao corpo do juiz, sobre a coleção escolhida pelo comando. (`app/chamados/juizes.py:1253`; `app/chamados/management/commands/disparar_perguntas_competencia.py:148`) |

O anexo também distingue os conjuntos do motor; o registro vivo explica expressamente que `VIVOS` responde existência do chamado, não existência de cobrança/fio. (`app/docs/JUIZES.md:54`; `app/core/juizes.py:100`)

O C6 não se reduz a `materializada_em IS NULL`: considera validação, tranca, vias de identidade/retratação, resposta textual, decisão administrativa, marco no espelho, célula provisória, falta e lastro. Uma materializada pode continuar viva; uma não materializada pode estar morta. (`app/chamados/juizes.py:1152`, `app/chamados/juizes.py:1164`, `app/chamados/juizes.py:1189`, `app/chamados/juizes.py:1196`, `app/chamados/juizes.py:1216`, `app/chamados/juizes.py:1227`)

**DECISÕES encontradas na comparação:**

1. **Universo de chamados:** o comando exige célula/janela, inclui `registrado` e não restringe módulo. Isso é mais específico que `_C1` e mais largo que “cobrança de batida”. (`app/chamados/management/commands/disparar_perguntas_competencia.py:141`; `app/chamados/catalogo/motor.py:24`)

2. **Universo de perguntas:** o primeiro teste considera todas as perguntas destinadas àquele chamado, sem restringir motivo, dia ou `conversa=False`. Uma conversa viva também satisfaz esse teste; o dedup posterior do fato exclui conversas. (`app/chamados/management/commands/disparar_perguntas_competencia.py:147`; `app/chamados/models.py:2752`)

3. **Coleção diferente da do vigia:** o comando olha perguntas do chamado; `vigia_fio.censo` olha todas as perguntas da célula. Pergunta viva sob outro destino pode produzir respostas diferentes entre os leitores. (`app/chamados/management/commands/disparar_perguntas_competencia.py:148`; `app/chamados/services/vigia_fio.py:398`)

4. **Vitalidade versus identidade:** `_ja_perguntou` consulta motivo/data, sem conversa, dentro de uma disputa; `linhas_dos_fatos` consulta colaborador/motivo/dia/instância, independentemente de vitalidade. O comando usa ambas, nessa ordem. (`app/chamados/services/disputa_emissao.py:1316`; `app/chamados/models.py:2751`; `app/chamados/management/commands/disparar_perguntas_competencia.py:172`, `app/chamados/management/commands/disparar_perguntas_competencia.py:215`)

5. **Retratada/duplicata versus encerrada:** `linhas_dos_fatos` exclui `duplicata` e `motivo_retratado`; não exclui uma linha apenas por validada, materializada ou morta. Portanto o rótulo **FIO DECIDIDO** significa, nesse ponto, “existe linha do fato”; não comprova decisão humana. (`app/chamados/models.py:2391`, `app/chamados/models.py:2753`; `app/chamados/management/commands/disparar_perguntas_competencia.py:215`, `app/chamados/management/commands/disparar_perguntas_competencia.py:225`)

6. **Guardas da fábrica:** o caminho célula consulta `dia_sem_trabalho`, futuro e veredito acusador; o fio mudo chama diretamente `motivos_pela_ata`, além de parcial/teto/ativo. O texto “dia ACUSADO pela lei” não corresponde a um teste explícito de `VEREDITOS_ACUSAM` nesse ramo. (`app/chamados/services/fabrica_da_celula.py:89`; `app/chamados/management/commands/disparar_perguntas_competencia.py:154`, `app/chamados/management/commands/disparar_perguntas_competencia.py:158`, `app/chamados/management/commands/disparar_perguntas_competencia.py:221`)

7. **Competência entregue no limbo:** o evento da célula manda `trabalha=None` em competência entregue à Pauta DP; o fio mudo chama diretamente `perguntar`, sem essa composição. (`app/chamados/services/fabrica_da_celula.py:181`; `app/chamados/management/commands/disparar_perguntas_competencia.py:282`)

8. **Tempo:** o fio usa dia do turno da célula e teto por instante, podendo incluir hoje; o vigia do ramo célula exclui hoje e ontem enquanto ontem pertence à janela do vigia de hora. Passado sem marcos continua autorizado por comparação de data. (`app/chamados/management/commands/disparar_perguntas_competencia.py:102`, `app/chamados/management/commands/disparar_perguntas_competencia.py:110`, `app/chamados/management/commands/disparar_perguntas_competencia.py:150`; `app/chamados/services/fabrica_da_celula.py:29`)

## (2) Escritores — inventário AST e leitura dos efeitos

A classificação abaixo distingue **escrita do carimbo**, **mudança de vitalidade C6** e **perda da pergunta daquele chamado por reapontamento**. O censo nativo identifica escritas por modelo, campos e relações; as chamadas às portas foram acrescentadas por AST e classificadas pela leitura dos corpos. Ocorrências e transições concretas em produção: **[nao verificado]**. (`app/core/censo_escritas.py:6`, `app/core/censo_escritas.py:20`, `app/core/censo_escritas.py:469`)

Nas tabelas, **C/D/Ch/P** significam Colaborador/Disputa/Chamado/Pergunta, a ordem declarada em `trava_na_ordem`. “Sem trava própria” não exclui trava herdada do chamador. (`app/chamados/models.py:2627`)

**Evento comum das perguntas:** os `save()` relevantes acionam outbox síncrono e beco `on_commit`; save e outbox compartilham atomic. O `save()` não adquire genericamente C nem exige seu registro. (`app/chamados/signals.py:214`, `app/chamados/signals.py:237`; `app/chamados/models.py:3403`)

### (2a) Portas que escrevem a pergunta sem escrever diretamente seu chamado

| Função/sítio | Escrita e transação/trava | Signal/evento atual |
|---|---|---|
| `PerguntaDisputa.carimbar` | Carimbo/via, opcionalmente resposta/validação/lavra; só grava se C6 aceitar a morte resultante. Sem trava própria; atomic do `save`. (`app/chamados/models.py:3093`, `app/chamados/models.py:3128`, `app/chamados/models.py:3142`) | Evento comum de P. (`app/chamados/signals.py:237`) |
| `carimbar_identidade` | Carimbo/via/recusa; não consulta C6; sem trava própria. As vias fora do fato vencem inclusive resposta. (`app/chamados/models.py:3158`, `app/chamados/models.py:3172`) | Evento comum de P. (`app/chamados/models.py:3180`) |
| `fechar_por_admin`; `encerrar_fantasma` | Delegam às portas anteriores; não acrescentam trava. (`app/chamados/models.py:3054`, `app/chamados/models.py:3277`) | Herdado do `save`. (`app/chamados/signals.py:237`) |
| `encerrar_por_fato_no_espelho` | Troca a via de pergunta já materializada; não prova nova morte C6. Sem trava própria. (`app/chamados/models.py:3258`) | `save` da via acorda outbox. (`app/chamados/models.py:3266`; `app/chamados/models.py:3210`) |
| `_carimbar_validacao` | `validada_em`, autor e `DecisaoPergunta`, juntos em atomic; trava herdada. Validação mata no primeiro degrau C6. (`app/chamados/services/validacao.py:127`, `app/chamados/services/validacao.py:136`; `app/chamados/juizes.py:1153`) | Evento comum de P e sequência de decisão. (`app/chamados/services/validacao.py:137`) |
| `fio.reapontar_resolvedora` | Destino e, condicionalmente, data; atomic sem savepoint; C, destino pela guarda de costura e D/Ch/P adicionais em colisão. Pode retirar a pergunta do chamado antigo sem alterar o status dele. (`app/chamados/services/fio.py:873`, `app/chamados/services/fio.py:881`, `app/chamados/services/fio.py:896`, `app/chamados/services/fio.py:914`) | Outbox vê o estado posterior; não há emissão explícita para a âncora antiga nesse corpo. (`app/chamados/services/fio.py:916`; `app/chamados/models.py:3229`) |
| `PerguntaDisputa._nascer`, reuso | Pode substituir célula/destino de linha existente; atomic e C antes do dedup. (`app/chamados/models.py:2839`, `app/chamados/models.py:2843`, `app/chamados/models.py:2957`) | `ex.save`, logo evento comum de P. (`app/chamados/models.py:2999`) |
| `fio.rechavear_data_evento` | Dia/célula/lavra; atomic e C→D→P. (`app/chamados/services/fio.py:989`, `app/chamados/services/fio.py:993`) | Além do `save`, acorda explicitamente a célula antiga. (`app/chamados/services/fio.py:1026`) |
| `retratar_para_canonica → retratar_duplicata` | Atomic; C/D/P das perdedoras e canônica ordenados; carimbo `duplicata`. (`app/chamados/services/fio.py:1319`, `app/chamados/services/fio.py:1321`, `app/chamados/services/fio.py:1342`) | Evento comum de P. (`app/chamados/models.py:3180`) |
| `DisputaSupervisao.remover_motivo` | Atomic C/D/Ch/P; decisão de retratação e `motivo_retratado`. Com motivos restantes, retorna sem fechar disputa/pai. (`app/chamados/models.py:2023`, `app/chamados/models.py:2026`, `app/chamados/models.py:2054`, `app/chamados/models.py:2064`) | Evento comum de P; a disputa também é salva. (`app/chamados/models.py:2059`, `app/chamados/models.py:2063`) |
| `relavra_via.relavrar` | Troca via/lavra, sem atomic ou trava próprios além do `save`; pode mudar insumo C6. (`app/chamados/services/relavra_via.py:106`, `app/chamados/services/relavra_via.py:125`) | Outbox e auditoria da relavra. (`app/chamados/services/relavra_via.py:127`, `app/chamados/services/relavra_via.py:132`) |
| `responder_pergunta_disputa_core` | Resposta/carimbo, desfaz validação anterior; atomic, C e `exigir_colaboradores_travados`. A concordância resposta/luz pode fazer C6 morrer. (`app/chamados/services/disputa_emissao.py:1368`, `app/chamados/services/disputa_emissao.py:1404`; `app/chamados/juizes.py:1181`) | Outbox e decisão de resposta. (`app/chamados/services/disputa_emissao.py:1409`) |
| `responder_automaticamente`; `carimbar_resposta_pela_trilha` | Resposta/carimbo em atomic, sem trava própria de C; efeito C6 depende da pergunta. (`app/chamados/services/disputa_emissao.py:1539`, `app/chamados/services/disputa_emissao.py:1555`, `app/chamados/services/disputa_emissao.py:1562`, `app/chamados/services/disputa_emissao.py:1576`) | Outbox e sequência de decisão; trilha também registra auditoria. (`app/chamados/services/disputa_emissao.py:1557`, `app/chamados/services/disputa_emissao.py:1578`) |

### Chamadores semânticos e casos em que o chamado pode sobreviver

| Função | Atomicidade/trava e efeito |
|---|---|
| `DisputaSupervisao.fechar` | Atomic C/D/Ch antes de carimbar mudas; salva disputa depois. S88 tenta fechar o pai pelo gate, em best-effort; fechamento do pai não é garantido pelo carimbo. (`app/chamados/models.py:1903`, `app/chamados/models.py:1908`, `app/chamados/models.py:1919`, `app/chamados/models.py:1934`; `app/chamados/signals.py:41`) |
| `lavrar_sem_turno_aberto` | Só carimba após C6 já dizer morta; sem atomic/trava próprios nesse ramo além do `save`. Pode fechar disputa depois. (`app/chamados/reconciliador.py:397`, `app/chamados/reconciliador.py:405`, `app/chamados/reconciliador.py:411`) |
| `retratar_perguntas_absolvidas_por_celula` | Atomic por pergunta, C e exigência do registro de trava; carimba pergunta já absolvida pelo juiz; tenta fechar disputa depois. (`app/chamados/reconciliador.py:584`, `app/chamados/reconciliador.py:592`, `app/chamados/reconciliador.py:594`, `app/chamados/reconciliador.py:604`) |
| `encerrar_orfas_pela_lampada` | Atomic por pergunta; `encerrar_fantasma` e auditoria; sem aquisição própria de C nesse bloco. (`app/chamados/reconciliador.py:1146`) |
| `_materializar_ausencia_de_resposta` | Atomic por irmã, C, carimbo e decisão de absorção. Ausência aberta mantém destino vivo; ausência decidida pede `reconciliar`. (`app/chamados/services/materializacao.py:557`, `app/chamados/services/materializacao.py:560`, `app/chamados/services/materializacao.py:578`, `app/chamados/services/materializacao.py:581`) |
| `_materializar_resposta_hora` | Paridade recusada usa identidade; herda atomic de `_materializar_uma`. (`app/chamados/services/materializacao.py:924`, `app/chamados/services/materializacao.py:1220`) |
| `_carimbar_recusa` | Colisão usa `fato_ocupado`; pode anexar ocorrência ao destino sem mudar seu status. Sem trava própria; contexto depende do chamador. (`app/chamados/services/materializacao.py:1093`, `app/chamados/services/materializacao.py:1099`, `app/chamados/services/materializacao.py:1105`) |
| `_materializar_uma` | Atomic de plantio/carimbo; pode tocar destino/clones. Portanto somente seus ramos condicionais entram em “sem tocar o chamado”. (`app/chamados/services/materializacao.py:1115`, `app/chamados/services/materializacao.py:1220`, `app/chamados/services/materializacao.py:1277`) |
| `materializar_pendentes_aprovadas` | Carimbo pelo lastro, sem atomic/trava C próprios além do `save`; acionada pelo signal de disputa. (`app/chamados/services/materializacao.py:1418`, `app/chamados/services/materializacao.py:1431`; `app/chamados/signals.py:147`) |
| `_resolver_perguntas_por_chamado_resolvido` | Wrapper abre/preserva atomic e trava C/D/Ch; pode reabrir pai ou fechar disputa. O segundo laço só carimba perguntas já mortas pelo C6. (`app/chamados/services/disputa_emissao.py:1697`, `app/chamados/services/disputa_emissao.py:1764`, `app/chamados/services/disputa_emissao.py:1789`, `app/chamados/services/disputa_emissao.py:1825`) |
| `validar_pergunta` | Atomic C/D/Ch; valida e tenta materialização; pode devolver a validação antes do commit. Morte durável depende do desfecho. (`app/chamados/services/validacao.py:425`, `app/chamados/services/validacao.py:431`, `app/chamados/services/validacao.py:435`, `app/chamados/services/validacao.py:464`) |
| `_aceitar_a_folga` | Os dois ramos têm atomic e C/D/Ch; validam/encerram pergunta e pedem fechamento do fio. (`app/chamados/services/validacao.py:281`, `app/chamados/services/validacao.py:285`, `app/chamados/services/validacao.py:314`, `app/chamados/services/validacao.py:329`) |
| `fechar_o_fio_do_dia` | C/D/Ch; fecha perguntas e depois tenta reconciliar chamados. A recusa do gate pode preservar o chamado. (`app/ponto/services/veredito_celula.py:175`, `app/ponto/services/veredito_celula.py:184`, `app/ponto/services/veredito_celula.py:201`) |
| `encerrar_perguntas_com_fato.handle` | Chama a porta que troca a via de pergunta já materializada. (`app/chamados/management/commands/encerrar_perguntas_com_fato.py:68`; `app/chamados/models.py:3258`) |

Os gatilhos de `remover_motivo` encontrados são reativação de posto e recalibração da âncora. (`app/colaboradores/signals.py:77`; `app/escala/signals.py:161`)

Os chamadores de reapontamento encontrados são `_gate_pergunta_viva`, reemissão, dois ramos de reata órfã, declaração, migração, reata/agrupamento no limbo e reversão administrativa; todos passam pela porta de costura descrita acima. (`app/chamados/models.py:437`; `app/chamados/reconciliador.py:954`, `app/chamados/reconciliador.py:1638`, `app/chamados/reconciliador.py:1651`; `app/chamados/services/declaracao_texto.py:342`; `app/chamados/services/fio.py:2129`; `app/chamados/services/limbo_folgas.py:244`, `app/chamados/services/limbo_folgas.py:282`; `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:127`)

Rechaveamento também é chamado no reuso e na migração/reversão de `data_evento`. (`app/chamados/services/fio.py:1057`, `app/chamados/services/fio.py:1061`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:400`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:492`)

### Morte por leitura, sem salvar PerguntaDisputa

Esses caminhos não disparam signal de P por si; mudam entradas que C6 lê. (`app/chamados/juizes.py:1155`, `app/chamados/juizes.py:1166`, `app/chamados/juizes.py:1196`, `app/chamados/juizes.py:1216`)

| Escritor da entrada | Transação/trava/evento |
|---|---|
| `celula.lavrar_veredito → salvar_e_emitir` | Veredito/via/ata; atomic de célula+outbox, sem trava própria de C. Com `salvar=False`, depende do salvamento posterior. (`app/ponto/portas/celula.py:969`, `app/ponto/portas/celula.py:1033`, `app/ponto/portas/celula.py:894`) |
| `editar_dia`; regeneração | Edição usa atomic e trava da célula, seguida de rejulgamento; regeneração tem atomic. (`app/ponto/portas/celula.py:592`, `app/ponto/portas/celula.py:627`, `app/ponto/portas/celula.py:218`) |
| `reverter_regeneracao_dia` | Grava DNA/trilha; o corpo termina sem o rabo de rejulgamento da edição. (`app/ponto/portas/celula.py:785`, `app/ponto/portas/celula.py:808`) |
| `trancar_periodo` | Grava `PeriodoFechado`, sem atomic/trava próprios nessa função, e tenta fechamento no ato. A pergunta morre por leitura da tranca; não recebe novo carimbo dessa via. (`app/ponto/services/fechamento.py:1408`, `app/ponto/services/fechamento.py:1419`, `app/ponto/services/fechamento.py:1426`; `app/chamados/services/competencia_trancada.py:177`) |
| `registrar_batida`, `anotar`, `retratar_batida` | Sem atomic/trava C próprios nessas portas; saves despertam cartório/reconciliador pelos signals de Batida. (`app/ponto/registro_batida.py:152`, `app/ponto/registro_batida.py:254`, `app/ponto/registro_batida.py:274`; `app/ponto/signals.py:131`, `app/ponto/signals.py:206`) |
| Alterações de Ausência/Justificativa | Seus signals também julgam o dia, podendo mudar a leitura C6 sem salvar P. (`app/ponto/signals.py:143`, `app/ponto/signals.py:161`; `app/chamados/juizes.py:1200`) |

O instante também participa do fallback da janela viva; incidência produtiva de mudança de vitalidade apenas pela passagem do tempo: **[nao verificado]**. (`app/chamados/juizes.py:1217`)

### (2b) Nascimento/reabertura de chamado sem garantia de pergunta viva no mesmo ato

| Porta | Transação/trava e eventos |
|---|---|
| **A — `ChamadoColaborador.abrir`** | Atomic e C antes do dedup. Pode nascer ou renascer. Emite `EventoCelula` para módulo de turno com célula; não garante pergunta viva. (`app/chamados/models.py:902`, `app/chamados/models.py:905`, `app/chamados/models.py:922`, `app/chamados/models.py:971`, `app/chamados/models.py:977`) |
| **C — `criar`** | Atomic sem savepoint para INSERT+evento condicional; não adquire C por conta própria. (`app/chamados/models.py:1093`) |
| **R — `reabrir`** | `_gravar_estado` usa atomic C/D/Ch; depois tenta ressuscitar perguntas em outro atomic. O conjunto pode estar vazio; não cria pergunta nova. (`app/chamados/models.py:280`, `app/chamados/models.py:1432`, `app/chamados/models.py:1441`, `app/chamados/models.py:1533`, `app/chamados/models.py:1547`) |
| **T — transição humana** | O motor permite `resolvido → em_analise`; `_aplicar` só delega ao núcleo de estado, sem ressuscitação explícita. (`app/chamados/catalogo/motor.py:102`; `app/chamados/models.py:447`) |

Todas essas entradas salvam Chamado e podem acionar o cartório; isso é condicionado por nascimento/mudança de status, `raw` e `em_julgamento`. (`app/chamados/signals.py:187`)

**Entradas de furo/turno:**

| Função/sítio | Porta e tentativa de pergunta |
|---|---|
| `detectar_ausencias._abrir_chamado` | A; tentativa posterior de fio em best-effort, cuja falha é engolida. (`app/ponto/management/commands/detectar_ausencias.py:693`, `app/ponto/management/commands/detectar_ausencias.py:790`, `app/

[truncado em 20000 de 44226 caracteres]

## 09/11 01:04 · sessao 604cd303beb9 · - · - · origem=Ronald (tmux)

Consolidado do item 6 — referências abaixo usam o prefixo `app/`; é mapa de funções e usos sem repetir cada ocorrência textual. Os escritores persistentes identificados ficam em `chamados/models.py:971`, `chamados/models.py:1095`, `chamados/models.py:296`, `chamados/models.py:3009`, `chamados/models.py:3019` e `chamados/services/fio.py:924`.

**`PerguntaDisputa.chamado_destino`**

- Campo significa “Chamado órfão que esta pergunta resolve quando materializada”; a lista `chamados_relacionados_ids` representa clones resolvidos junto. `chamados/models.py:3385-3391`, `chamados/models.py:3404-3410`.
- Escritor de nascimento: `_nascer` constrói a pergunta com destino e insere; escritor de reapontamento no reuso: `_nascer` atribui `ex.chamado_destino`, lavra e salva; segunda porta de reapontamento: `fio.reapontar_resolvedora`, atribuição e save com lavra. `chamados/models.py:2985-3009`, `chamados/models.py:3016-3019`, `chamados/services/fio.py:922-925`.
- `_costura_nao_mata_o_fio` atribui temporariamente o destino para julgar e o restaura; `_vivas_entre` faz operação semelhante em memória. Essas atribuições não são novos escritores persistentes. `chamados/services/fio.py:778-783`, `chamados/models.py:356-371`.
- Leitores de trava/encadeamento: `_trava_do_repasse`, `_trava_do_ato`, `DisputaSupervisao.fechar`, `remover_motivo`, validação e fechamento do dia usam o destino para compor conjuntos. `chamados/services/disputa_emissao.py:956-959`, `chamados/services/declaracao_texto.py:295-297`, `chamados/models.py:1919`, `chamados/models.py:2035`, `chamados/services/validacao.py:432`, `ponto/services/veredito_celula.py:186`.
- Leitores de resolução/ciclo: `_gravar_estado`, `_ancoradas`, `_gate_pergunta_viva`, `_resolver_perguntas_por_chamado_resolvido`, `_pergunta_materializada_de_fato`, `irmas_respondidas_sem_consumo`, `via_de_hoje`, materializadores e `becos`. `chamados/models.py:309-312`, `chamados/models.py:345`, `chamados/models.py:436-444`, `chamados/services/disputa_emissao.py:1748`, `chamados/juizes.py:757-758`, `chamados/juizes.py:1770-1773`, `chamados/services/relavra_via.py:101-103`, `chamados/services/materializacao.py:339`, `chamados/services/materializacao.py:527`, `chamados/services/materializacao.py:581-582`, `chamados/services/materializacao.py:711`, `chamados/services/materializacao.py:794`, `chamados/services/materializacao.py:994`, `chamados/services/materializacao.py:1076`, `chamados/services/becos.py:30`, `chamados/services/becos.py:46-47`.
- Leitores de apresentação/identificação: `chamado_que_lista`, `_disputa_do_chamado`, `montar_chamados_fio`, `api_chamados_v2`, `api_disputa_perguntas`, `creditos_perguntas`, histórico, ações de chamado/disputa, janela do colaborador, pergunta no app e explicador. `chamados/services/fio.py:116-137`, `chamados/services/fio.py:225-227`, `api/views.py:1026`, `api/views.py:1059-1065`, `api/views.py:2590`, `api/views.py:2606-2608`, `chamados/services/raiox.py:133-149`, `chamados/services/historico.py:96`, `chamados/services/acoes_chamado.py:357`, `chamados/services/acoes_disputa.py:351`, `chamados/services/janela_colab.py:64-65`, `chamados/services/pergunta_no_app.py:143`, `chamados/services/pergunta_no_app.py:166-169`, `chamados/services/explicador.py:122`.
- Leitores de diagnóstico/manutenção: `vigia_carimbo.classificar`, `competencia_trancada.medir`, `limbo_folgas`, `lavrar_perguntas_stale`, `auditar_disputas`, `auditar_invariantes_chamados`, `reconciliar_fantasmas`, `reconciliar_grade`, commands de relavra/backfill/migração/reabertura. `chamados/services/vigia_carimbo.py:103`, `chamados/services/competencia_trancada.py:108-140`, `chamados/services/limbo_folgas.py:204`, `chamados/services/limbo_folgas.py:239-240`, `chamados/management/commands/lavrar_perguntas_stale.py:57`, `chamados/management/commands/auditar_disputas.py:151`, `chamados/management/commands/auditar_invariantes_chamados.py:588-595`, `ponto/management/commands/reconciliar_fantasmas.py:210-215`, `ponto/management/commands/reconciliar_grade.py:460`, `chamados/management/commands/relavrar_via_i6.py:56`, `chamados/management/commands/backfill_veredito_lavrado.py:101`, `chamados/management/commands/migrar_data_evento_dia_do_turno.py:377`, `chamados/management/commands/reabrir_orfas_enterradas.py:69`.

**Divergência semântica comprovada:** `chamado_que_lista` devolve destino sempre que houver; API e crédito do raio-X usam destino somente se vivo, caindo no pai quando morto; `irmas_respondidas_sem_consumo` segue a regra do crédito. Portanto, “destino”, “âncora de listagem” e “chamado creditado” não são universalmente intercambiáveis. `chamados/services/fio.py:116-117`, `api/views.py:1062-1065`, `chamados/services/raiox.py:140-147`, `chamados/juizes.py:1770-1773`.

**Queries por destino sem filtro de dono:** `_disputa_do_chamado`, histórico, resolução S143, busca/reuso na porta órfã, migração de resolvedoras e relavra do estado. A ausência de filtro é verificável; existência de dados cruzados em produção não veio no anexo. `chamados/services/fio.py:134-136`, `chamados/services/historico.py:96`, `chamados/services/disputa_emissao.py:1748`, `chamados/services/disputa_emissao.py:1002`, `chamados/services/disputa_emissao.py:1022`, `chamados/services/fio.py:2037`, `chamados/models.py:309-311`; dados cruzados em produção: [não verificado].

**`PerguntaDisputa.colaborador`**

- Escritor runtime identificado: `_chave_de_identidade` preenche com `self.disputa.colaborador_id`, ou recusa valor diferente; só é chamado por `save` quando `_state.adding`. Logo, há guarda de nascimento, sem prova de igualdade para qualquer UPDATE/acervo legado. `chamados/models.py:3420-3423`, `chamados/models.py:3430-3435`.
- Escritor histórico: migration 0051 atualiza a coluna por subquery da disputa. `chamados/migrations/0051_decisao_pergunta.py:25`.
- Leitores diretos de identidade/dedup/trava: `q_dos_fatos`, `fato_livre_para`, `fio.chave_do_dia`, `_colab_da_linha`, `reapontar_resolvedora`, `rechavear_data_evento`, `retratar_para_canonica`, `grupos_de_colisao`, migração de resolvedoras e conjunto de repasse. `chamados/models.py:2762-2763`, `chamados/models.py:2774-2781`, `chamados/services/fio.py:1450-1451`, `chamados/services/fio.py:1646-1653`, `chamados/services/fio.py:863`, `chamados/services/fio.py:888-889`, `chamados/services/fio.py:1001`, `chamados/services/fio.py:1022-1025`, `chamados/services/fio.py:1329-1330`, `chamados/services/fio.py:1946`, `chamados/services/fio.py:2085-2092`, `chamados/services/disputa_emissao.py:956`.
- Leitores de resolução/censo: `carimbar_pergunta_c1` usa `p.colaborador` como fallback; contador de destino cruzado compara a coluna ao dono do chamado; backfill de decisões usa a coluna para competência. `chamados/reconciliador.py:368`, `chamados/services/fio.py:1186-1193`, `chamados/management/commands/backfill_decisoes_pergunta.py:69`, `chamados/management/commands/backfill_decisoes_pergunta.py:114-120`.
- Outros leitores continuam derivando o dono pela disputa: `qs_perguntas_absolviveis`, `_orfas_abertas`, `perguntas_orfas_reatavies`, juiz de tranca, validação, autorização da resposta no fio e staleness da lavra. Se coluna e disputa divergirem, esses universos não são necessariamente iguais aos anteriores. `chamados/reconciliador.py:627-628`, `chamados/reconciliador.py:1052-1053`, `chamados/reconciliador.py:1496-1501`, `chamados/juizes.py:949`, `chamados/services/validacao.py:268`, `chamados/services/validacao.py:431`, `chamados/services/fio.py:598`, `chamados/veredito_lavrado.py:210-213`.
- A costura atual recusa destino de outro dono tanto no nascimento quanto no reapontamento; essa recusa não é validação global das relações já gravadas. `chamados/models.py:2848`, `chamados/services/fio.py:863-865`, `chamados/services/fio.py:1575-1579`.

**`ChamadoColaborador.status_local`**

- Escritores runtime persistentes: nascimento `abrir` define `_def['status_local']` e passa ao `get_or_create`; `criar` passa estado ao `create`; transições atribuem em `_gravar_estado` e salvam. `chamados/models.py:938-940`, `chamados/models.py:971`, `chamados/models.py:1095`, `chamados/models.py:228`, `chamados/models.py:296`.
- Chamadores diretos do escritor de transição: `_aplicar`, `reconciliar`, `cancelar`, `analisar`, `reabrir`, `encerrar`, `encerrar_por_competencia_trancada`. `chamados/models.py:450`, `chamados/models.py:654`, `chamados/models.py:788`, `chamados/models.py:806`, `chamados/models.py:1442`, `chamados/models.py:1616`, `chamados/models.py:1636`.
- Principais leitores do ato: conjuntos de trava; dedup/renascimento; guards de fechamento/reabertura; receivers de Chamado; reconciliador; cartório; C6; materializador; fio; crédito/API. `chamados/models.py:2691`, `chamados/models.py:921`, `chamados/models.py:927`, `chamados/models.py:673`, `chamados/models.py:1337-1358`, `chamados/signals.py:33`, `chamados/signals.py:70`, `chamados/signals.py:86-96`, `chamados/signals.py:184-192`, `chamados/signals.py:272-280`, `chamados/signals.py:341-345`, `chamados/reconciliador.py:839`, `chamados/reconciliador.py:936-938`, `chamados/reconciliador.py:997`, `chamados/reconciliador.py:1204`, `chamados/reconciliador.py:1450`, `chamados/reconciliador.py:1583`, `ponto/services/cartorio.py:1334`, `ponto/services/cartorio.py:1592`, `ponto/services/cartorio.py:1611`, `chamados/juizes.py:758`, `chamados/services/materializacao.py:773-779`, `chamados/services/fio.py:770`, `api/views.py:1063`.
- Leitores de fila/tela/diagnóstico incluem painel, fila por verbo, `na_fila`, `pergunta_no_app`, `vigia_fio`, calendário/ficha/drawer, relatórios, trabalho do DP, regeneração, cobertura, triagem, ausência, competência trancada, SLA e commands emissores/reconciliadores. Exemplos concretos: `chamados/services/painel.py:155`, `chamados/services/painel.py:187`, `chamados/services/verbos.py:230`, `chamados/services/na_fila.py:17`, `chamados/services/pergunta_no_app.py:129`, `chamados/services/vigia_fio.py:607-618`, `colaboradores/services/calendario.py:437-444`, `colaboradores/services/ficha.py:163`, `colaboradores/services/drawer.py:89`, `relatorios/views.py:745`, `ponto/worklist.py:40`, `escala/services/regeneracao.py:170`, `ponto/services/cobertura.py:226`, `ponto/services/triagem_batida.py:290`, `ponto/services/ausencia.py:709`, `chamados/services/competencia_trancada.py:88`, `chamados/services/sla.py:37`, `chamados/management/commands/reconciliar_chamados.py:59`, `ponto/management/commands/detectar_ausencias.py:739`.

A coluna representa estado do ciclo, mas os leitores fazem perguntas diferentes: `VIVOS` inclui `registrado`, enquanto `COBRANCA` o exclui; `ENCERRADOS` exclui `cancelado/superado`, que pertencem a `TERMINAIS`; portanto “vivo”, “cobra”, “encerrado” e “terminal” não são sinônimos. `chamados/catalogo/motor.py:24-32`. O véu de silêncio também é campo separado do estado. `chamados/models.py:87-98`.

**Leitura do juiz versus escrita de lavra**

`pergunta_viva` calcula e guarda memo apenas na instância; `celula_do_dia_agora` faz SELECT e projeção da ata/veredito em memória; `fato_do_marco_no_espelho` lê a ata. Não encontrei escrita de P/célula nesses corpos. `chamados/juizes.py:850-863`, `chamados/juizes.py:388-410`, `chamados/juizes.py:904-931`. Já `veredito_lavrado.lavrar` salva Pergunta por padrão, salvo `salvar=False`; o backfill persiste por `gravar_lavras_em_lote`. `chamados/veredito_lavrado.py:234-255`, `chamados/veredito_lavrado.py:220-230`.

_2979 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
