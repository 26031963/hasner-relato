# CODEX-ESTADO

> Ultima resposta processada: **01/11/2026 01:42** · publicado em **01/11/2026 01:45** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

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

## 01/11 01:38 · sessao a855cc84a62c · - · - · origem=Ronald (tmux)

Achados para consolidar, todos por leitura; os dois checks de certificado usaram funções em memória com I/O substituído.

- **bloqueia — item 9, certificado forjável:** `bin/disjuntor.py:2229` expõe `--cert-suite`; `:1226–1242` confia no rc, recorte, hashes e log informados pelo chamador. Um log sintético `Ran 1 tests...\nOK` emite certificado sem executar suite. `guarda_chave` não guarda certificados (`:182`); Write/Bash da sessão principal passa pelo retorno de `pretool` em `:1935`. O leitor aceita somente `arvore=<h>` e `rc=0`, sem exigir Ran/OK (`:1264`). Confirmei ambas as funções em memória: certificado de duas linhas passa; log sintético emite certificado.

- **bloqueia — segundo escritor e selo que normaliza certificado inválido:** `bin/tests/test_disjuntor.sh:73` escreve `.ok` diretamente com `Ran=1`, sem OK; `i9_cert` considera esse arquivo verde em `:1631`. O escritor normal é `bin/disjuntor.py:1247`, acionado por `bin/suite.sh:112`; o leitor é `_cert_ok` em `bin/disjuntor.py:1260`, chamado pela porta em `:1468`. Os testes também sobrescrevem certificado em `bin/tests/test_disjuntor.sh:1634` e chamam a CLI usando log inventado em `:1642–1649`. Não há integração de `suite.sh` real nesse caso.

- **bloqueia — item 9, hash parcial pode virar identidade válida:** `bin/arvore_hash.sh:31` ignora erro de `git add --ignore-errors`; `:32` pode terminar com sucesso e emitir hash da parte legível. O gate só testa rc final e sintaxe do hash (`bin/disjuntor.py:1220`). Portanto árvore não integralmente lida pode casar com certificado da árvore parcial, em vez de NAO_SEI. Erros também são ocultados em `bin/arvore_hash.sh:31` e `bin/suite.sh:100`.

- **bloqueia — item 10 exclui merges sem amparo no REVISADO:** `bin/contrato_do_push.py:37` usa `--no-merges`. Uma resolução de merge que acrescente/mude instrumento em `bin/` não é julgada. O contrato `app/docs/contratos/CONTENCAO-QUE-MORDE-V2.txt:20` não exclui merge. O selo só cria commits lineares (`bin/tests/test_contrato_do_push.sh:29–43`).

- **bloqueia — item 10 usa presença do próprio juiz como marcador de era:** `bin/contrato_do_push.py:41` pula qualquer commit cuja árvore não contenha o portão. Um commit novo que apague o arquivo do portão, inclusive junto de outro instrumento, passa pelo ramo de “não retroage”. Além disso, `_existe` colapsa qualquer erro de `cat-file` para ausência (`:24–25`), que nesse ponto libera. O selo cobre commit anterior legítimo, não remoção posterior (`bin/tests/test_contrato_do_push.sh:30`, `:43`).

- **bloqueia — entrada ilegível/malformada pode virar push sem julgamento:** `bin/pre-push.sh:7` não verifica erro do `cat`; `set -e` só aparece em `:79`. Entrada vazia devolve rc 0 pelo loop vazio (`bin/contrato_do_push.py:31`, `:55`); linhas malformadas são silenciosamente ignoradas (`:33–34`). Com SKIP_TESTS, o hook sai 0 em `bin/pre-push.sh:11`.

- **Item 5, bloqueia — allowlist evita B1 novo:** `bin/disjuntor.py:466` libera tipos SO_LEITURA antes de avaliar cópias e job antigo, embora `:460` já tenha encontrado job. Verificador/Explore/Plan com job próprio e cópia parada escapa. O contrato `app/docs/contratos/CONTENCAO-QUE-MORDE-V2.txt:15` não prevê essa exceção.

- **Item 5, bloqueia — cópia ilegível desaparece:** `bin/disjuntor.py:468` filtra caminhos citados por `os.path.isdir`. Se há uma cópia ilegível/inacessível e outra legível com arquivo novo, a primeira some e a segunda libera em `:505–506`. Transcript ilegível também devolve `disparo=False` em `:458–459`; hora ilegível do lançamento é ignorada em `:530–531`.

- **Item 5, bloqueia — fim/job derivados de texto livre:** `bin/disjuntor.py:521` aceita `<task-id>…<status>[a-z]+</status>` em qualquer texto, sem exigir task-notification nem estado terminal; uma citação ou `running` cancela o job. `:525` reconhece job pela frase de saída, sem conferir chamada Bash com `run_in_background`. O selo só fornece frase de saída (`bin/tests/test_disjuntor.sh:1666`) e fim completed; não exercita autenticidade.

- **Resposta direta à pergunta 5:** em `bin/pre-push.sh` a chamada é `:10`, SKIP_TESTS é `:11`, e não há exit anterior. A instalação declarada delega a esse arquivo em `bin/hooks.sh:56`; instalação efetiva [nao verificado]. Existe cópia divergente `app/core/hooks/pre-push.sh:5`, com SKIP_TESTS no início e sem portão; uso efetivo [nao verificado], portanto **fila** para remover ambiguidade.

- **fila — corrida ABA na suite:** H0 é calculado antes de adquirir trava (`bin/suite.sh:100`, trava em `:102`); H1 é calculado em `:111`. A cópia pode ser A em H0, B durante testes, A em H1 e receber certificado A. Não há snapshot ou imobilização da árvore. O escritor satisfaz a igualdade literal antes/depois, mas ela não prova a árvore efetivamente testada.

Escritores/leitores de contratos: contrato versionado presente em `app/docs/contratos/CONTENCAO-QUE-MORDE-V2.txt:1`; o leitor operacional é `bin/contrato_do_push.py:51` via `cat-file` em `:24`. Nenhum escritor operacional de contratos foi encontrado; o selo cria contratos sintéticos por `_commit` (`bin/tests/test_contrato_do_push.sh:20–21`, chamadas `:32`, `:37`). autoria/materialização externa [nao verificado].

## 01/11 01:38 · sessao 8e68217c962e · - · - · origem=Ronald (tmux)

Achados dos selos, por leitura e sondas em memória, sem escrever arquivos:

- **bloqueia — mutante morto por exceção, item 4.** `bin/tests/test_disjuntor.sh:2025` remove a guarda de obra única, mas `i4_universo` começa pelo prompt sem obra (`:1621`): o mutante chega a `obras[0]` em `bin/disjuntor.py:1470`, causa `IndexError` e continua negando via `:1996`. O teste cai porque mudou a mensagem, não porque liberou. O caso de duas obras (`test_disjuntor.sh:1622`), que mostraria liberação indevida, nem é alcançado. `IndexError` confirmado em execução isolada em memória.

- **bloqueia — mutante morto por exceção, item 9.** `bin/tests/test_disjuntor.sh:2033` remove a guarda de cópia. O caso sem cópia (`:1640`) passa a acessar `copias[0]` em `bin/disjuntor.py:1443`; recebe `IndexError` e continua negando em `:1996`. O harness aceita qualquer retorno não zero do caso como mutante morto (`test_disjuntor.sh:1705`). `IndexError` confirmado em memória.

- **bloqueia — certificado artificial aceito pelo próprio selo.** `_arvore` escreve diretamente `arvore=…`, `rc=0`, `Ran=1`, sem `OK`, em `bin/tests/test_disjuntor.sh:73`; `i9_cert` exige autorização com esse arquivo em `:1631`. Isso demonstra aceitação de arquivo que não tem o formato produzido pelo escritor (`bin/disjuntor.py:1243`). O teste da emissão chama a CLI diretamente (`test_disjuntor.sh:1643`), sem exercitar `suite.sh → --cert-suite` (`bin/suite.sh:112`); retirar essa chamada da suite preserva o caso i9.

- **bloqueia — identidade exercitada só enquanto a reserva existe.** `i11_vivos` repete o mesmo ID antes do resultado (`bin/tests/test_disjuntor.sh:1556`), sem obra (`:1552`). Não repete uma autorização da mesma obra após poda. `_ag` gera ID novo e já acrescenta seu resultado antes da autorização (`:335`, `:339`), portanto os testes de voltas não mordem idempotência após poda nem o lançamento real.

- **bloqueia — sincronismo e concorrência não são medidos por i11.** As seis primeiras vagas são apenas reservas sem resultado (`bin/tests/test_disjuntor.sh:1552`); o teste não cria agentes síncronos rodando nem disputa simultânea entre autorizações. Assim, o caso permanece verde se o leitor de agentes lançados retornar sempre vazio, desde que a contagem de reservas permaneça (`bin/disjuntor.py:1475`).

- **bloqueia — cláusulas sem mutante correspondente.** O bloco P1 (`bin/tests/test_disjuntor.sh:2003`) não retira persistência da autorização, semeadura retroativa/parcial, idempotência por `chaves`, migração de ERRO-BOBO antigo/ilegível, timestamps/janela de `b3_sem_obra`, custo 24h nem tratamento restritivo de meta/cópia ilegível no B1. Os respectivos casos limitam-se a contagem nova (`:1566`), ERRO-BOBO novo (`:1586`), universo (`:1621`), modelos/papéis (`:1533`) e três cenários legíveis de job (`:1676`). Retirar essas costuras não é mordido por seus casos P1.

- **fila — ordem do pre-push aferida apenas por texto.** `bin/tests/test_contrato_do_push.sh:48` procura a primeira ocorrência do nome do arquivo, inclusive comentário; `:49` procura o primeiro teste de `SKIP_TESTS`. Não executa o hook. Uma menção anterior em comentário e uma chamada efetiva depois do skip podem passar. O mutante específico `skip-antes` (`:70`) é detectado por esse grep.

Verificação dos mutantes:

- Todos os **32 mutantes P1** de `bin/tests/test_disjuntor.sh:2003` têm âncora única; todas as mutações Python resultantes compilam, conferidas em memória. Os dois problemas de mordida são os `IndexError` descritos acima, não erros de sintaxe.
- Os demais removem os ramos declarados. `i11-sem-poda`, `i11-nao-idempotente`, `i1-reabre-nada` e `i2-redesenha-nao-tira` são mortos por negar uma autorização esperada, não por liberar algo proibido (`:2012`, `:2013`, `:2016`, `:2020`).
- Os cinco mutantes do push têm âncoras únicas e removem as costuras declaradas: contrato obrigatório, não retroatividade, exclusão de testes, existência do contrato e posição do skip (`bin/tests/test_contrato_do_push.sh:66`).
- Execução integral dos selos: **[nao verificado]**, porque criam e apagam arquivos (`test_disjuntor.sh:39`, `test_contrato_do_push.sh:14`).

## 01/11 00:58 · sessao 45f7099e218f · COBERTURA-PARCIAL-UM-JUIZ · verificador · origem=Code

**Parecer: bloqueia.** Há cobrança de marcos cobertos no noturno, âncora com data errada e leitores que ainda calam o resto da parcial. (`app/escala/utils.py:1335`, `app/ponto/services/cartorio.py:1719`, `app/chamados/services/materializacao.py:74`, `app/chamados/reconciliador.py:825`)

Os casos abaixo são contraexemplos do código; ocorrência e quantidade em produção, execução da suíte Django e cumprimento dos atos de pouso ficam **[nao verificado]**. [nao verificado]

**1. Bloqueia — noturno com intervalo dispensa os marcos errados.**

Caso: turno **D19 → D+1 02 → D+1 03 → D+1 07**, parcial **23–07**. A régua escolhe a menor entrada, **03**, e entrega `[E03,S07,E19,S02]`; essa régua chega diretamente à marcação das ausências. (`app/escala/utils.py:1335`, `app/escala/utils.py:1343`, `app/escala/utils.py:1362`, `app/escala/utils.py:1805`)

`deslocamentos_dos_marcos` encadeia essa ordem como se fosse a sequência do turno: produz **180, 420, 1140, 1560** minutos. Portanto, dispensa S02, mas deixa E03 e S07 faltantes, embora ambos pertençam à madrugada coberta. Pela regra do período noturno, os deslocamentos corretos seriam **1140, 1560, 1620, 1860**. (`app/ponto/turnos.py:2652`, `app/ponto/turnos.py:2661`, `app/escala/utils.py:345`, `app/ponto/turnos.py:2636`, `app/escala/utils.py:1209`)

A falha está na alimentação do juiz com a régua de apresentação. O teste noturno fornece diretamente apenas E19/S07 e não exercita essa costura. (`app/ponto/tests/test_cobparc_marco_no_periodo.py:53`, `app/ponto/tests/test_cobparc_marco_no_periodo.py:62`)

**2. Bloqueia — a nova âncora parcial perde a data da madrugada.**

Caso: turno **D19–D+1 07**, parcial **D19–23**, sem batidas. E19 é dispensada; sobra S07. O ramo novo adota a âncora de `_marcos_do_furo_parcial`, mas esse auxiliar ordena `HH:MM` e combina a primeira hora com **D**: resulta **D07**, 24 horas antes do marco devido. (`app/ponto/services/cartorio.py:1578`, `app/ponto/services/cartorio.py:1717`, `app/ponto/services/cartorio.py:1719`)

Isso acrescenta **1.440 minutos** ao atraso e grava/apresenta `horario_previsto` errado no contexto e no push. O SLA dessa porta usa `agora` e janela; não atribuo à âncora alteração direta desse prazo. (`app/ponto/management/commands/detectar_ausencias.py:581`, `app/ponto/management/commands/detectar_ausencias.py:702`, `app/ponto/management/commands/detectar_ausencias.py:776`, `app/ponto/management/commands/detectar_ausencias.py:593`)

**3. Bloqueia — duas ausências fazem materialização e grade escolherem coberturas diferentes.**

Caso: ausência integral antiga e parcial com período mais recente, ambas aprovadas, com marco faltante fora da parcial. O juiz de cobertura escolhe a maior `(data, pk)`; a grade segue essa vencedora e cobra o resto. A materialização usa `any()` sobre todas as ausências não-falta e cala a pergunta pela integral perdedora. (`app/ponto/turnos.py:2552`, `app/ponto/turnos.py:2557`, `app/ponto/turnos.py:2784`, `app/escala/utils.py:1149`, `app/chamados/services/materializacao.py:74`)

`pergunta_no_app` e `disputa_emissao` propagam exatamente esse retorno. Assim, `_celula_furada` pode responder “furada” e a fábrica impedir a pergunta para o mesmo dia. (`app/chamados/services/pergunta_no_app.py:74`, `app/chamados/services/disputa_emissao.py:763`, `app/chamados/juizes.py:446`, `app/chamados/juizes.py:478`)

O argumento vazio **não é o defeito**: com os dois horários preenchidos, `periodo_da_parcial_no_dia(a, ())` retorna `fora`; faltando um horário, retorna `sem_periodo`. Logo, a comparação exclusiva com `sem_periodo` continua detectando ausência de cadastro. (`app/ponto/turnos.py:2632`, `app/ponto/turnos.py:2638`, `app/ponto/turnos.py:2641`)

**4. Bloqueia — ainda há leitores não migrados.**

| Leitor | Caso concreto e resposta divergente |
|---|---|
| Reconciliação c2 | Parcial com período e marco externo apagado: qualquer ausência vencedora veta criação, reabertura e até costura no chamado vivo. (`app/chamados/reconciliador.py:811`, `app/chamados/reconciliador.py:825`) |
| Calendário | Parcial sem batida, com resto cobrado: retorna `ausencia` antes de consultar o veredito; o status lavrado pode ser `irregular`. (`app/colaboradores/services/calendario.py:109`, `app/ponto/status_dia.py:98`, `app/ponto/status_dia.py:112`) |
| Relatório de furos | O mesmo dia parcial é classificado como `folga` antes de `estado_do_dia`; também desaparece das funções usadas pelo Raio-X. (`app/relatorios/furos.py:151`, `app/relatorios/furos.py:218`, `app/relatorios/furos.py:237`) |

No calendário, isso também exclui o dia do universo de “Resolver dia”, que exige `falta`, `irregular`, `aberto` ou `sem_certidao`. (`app/colaboradores/services/calendario.py:181`)

**5. Bloqueia a equivalência universal declarada — `falta` é uma exceção explícita.**

Pela definição fornecida, toda falta é parcial: `ausencia_explica_o_dia_inteiro` devolve `False` para `tipo='falta'`. Entretanto, a grade apaga suas células e órfãs, não grava o carimbo parcial e o supra exclui falta de `dia_de_cobranca`, declaradamente para evitar cobrança dobrada sobre dia já descontado. (`app/ponto/turnos.py:2690`, `app/escala/utils.py:1174`, `app/escala/utils.py:1178`, `app/ponto/supra_juiz.py:302`)

Portanto, o código não satisfaz literalmente “todo retorno `False` cobra o resto”. Isso tampouco é uma combinação real alterada por `_resto_cobrado`, pois a grade não produz o carimbo nessa falta. O catálogo padrão não aceita falta por minutos; existência de registro excepcional em produção: **[nao verificado]**. (`app/escala/utils.py:1191`, `app/ponto/catalogo/ausencias.py:88`)

**6. Matriz da alteração em `COBRANCA_EM_DIA_COBERTO`.**

Excluído o retorno inicial por isenção, o código anteriormente emitido é retirado exatamente nestas combinações; nenhuma das demais combinações muda por essa condição. (`app/ponto/supra_juiz.py:220`, `app/ponto/supra_juiz.py:530`)

| Insumo | Combinações que retiram o código |
|---|---|
| `cobertura_status` | `aprovada` |
| `tipo_dia` | Qualquer valor formal: o ramo não verifica o tipo |
| `cobertura_parcial` | Verdadeiro |
| `periodo_parcial` | `dentro`, `fora`, `nao_sei`; também `None`/ausente ou qualquer valor diferente de `sem_periodo` |
| `_n_missing` | Maior que zero |
| `chamados_vivos` | Maior que zero |
| `cobrancas_vivas` | Tanto zero quanto maior que zero |

Fonte da matriz: `app/ponto/supra_juiz.py:255`, `app/ponto/supra_juiz.py:268`, `app/ponto/supra_juiz.py:306`, `app/ponto/supra_juiz.py:530`.

**Retirar o código não equivale sempre a mudar `coberta` para `cobrado`:**

- Se já havia código de `furo` ou `cobranca_indevida`, ele já vencia `coberta`; o veredito permanece. (`app/escala/models.py:1580`)
- Se `coberta` estava vencendo códigos de cadastro, discordância ou fato, esses códigos passam a determinar o veredito. (`app/escala/models.py:1582`)
- Sem código restante, a derivação devolve `concorde`. Somente o cartório substitui esse resultado por `cobrado`, exigindo fio vivo **e protestante**; decisão administrativa continua prevalecendo. (`app/escala/models.py:1595`, `app/ponto/services/cartorio.py:1294`, `app/ponto/portas/celula.py:655`)

**Existe vazio sem `cobrado`.** Contraexemplo puro: ausência parcial, período `fora`, um missing, realizado 240, previsto 480, `sem_ancora=True`, turno encerrado, chamado vivo e nenhuma cobrança com fio. O ramo do fato não emite furo por `sem_ancora`, o novo ramo retira cobertura e o cartório não tem fio para escrever `cobrado`: sobra `concorde`. Ocorrência produtiva desses insumos: **[nao verificado]**. (`app/ponto/supra_juiz.py:392`, `app/ponto/supra_juiz.py:530`, `app/ponto/services/cartorio.py:1294`)

No caso ordinário **encerrado, previsto positivo, sem fato e com fio morto**, não há esse vazio: sai `FURO_COM_COBRANCA_MORTA`. (`app/ponto/supra_juiz.py:313`, `app/ponto/supra_juiz.py:360`)

A retirada não cria, sozinha, acusação de marco coberto quando a grade está correta; porém o contraexemplo noturno do achado 1 produz missing dentro da cobertura e derruba essa premissa. (`app/escala/utils.py:1209`, `app/ponto/supra_juiz.py:530`)

**7. Consumidores de `coberta` e efeito da mudança.**

| Consumidor | Efeito |
|---|---|
| Pergunta viva | `coberta` não acusa; `cobrado`, `furo` e demais acusadores mantêm a pergunta, ressalvada sua resolução própria. (`app/escala/models.py:1735`, `app/chamados/juizes.py:1102`) |
| Retratação | Veredito não acusador permite absolvição; marcos pendentes impedem essa conclusão antecipada. (`app/chamados/reconciliador.py:973`, `app/chamados/reconciliador.py:982`) |
| Emissão/reabertura | `coberta` integra os vereditos que vetam cobrança; `cobrado` não integra essa lista. (`app/ponto/marcos.py:119`, `app/ponto/marcos.py:154`) |
| Status lavrado | Sem batida, `coberta` dá ausência; parcial acusadora dá irregular. Com batida, `coberta` dá ok e acusação dá irregular. (`app/ponto/status_dia.py:98`, `app/ponto/status_dia.py:110`, `app/ponto/status_dia.py:117`) |
| Calendário | O mapa distingue `coberta→ok` e `cobrado→irregular`, mas o retorno antecipado continua escondendo a mudança na ausência sem batida. (`app/colaboradores/services/calendario.py:54`, `app/colaboradores/services/calendario.py:109`) |
| Folha/retenção | Acusadores passam pelo retentor, com exceções de protestos não cobráveis e furo exclusivamente de pausa; `coberta` fica fora desse ramo. (`app/folha/export.py:1215`, `app/folha/export.py:1235`) |
| Relatórios | `estado_do_dia` mapeia `coberta→folga` e `cobrado→furo`, mas o dataset pula antes os dias de ausência. Dia da frota distingue os rótulos e seleciona acusadores. (`app/relatorios/furos.py:24`, `app/relatorios/furos.py:151`, `app/relatorios/dia_da_frota.py:10`, `app/relatorios/dia_da_frota.py:54`) |

O executor do comando supra retrata diretamente por `COBRANCA_SEM_FURO` e `COBRANCA_EM_DIA_SEM_TRABALHO`; a consequência de `COBRANCA_EM_DIA_COBERTO` passa pela derivação e pelos consumidores do veredito. (`app/ponto/management/commands/supra_juiz.py:218`, `app/escala/models.py:1582`)

**8. Censo de escritores e leitores, inclusive fora do diff.**

| Campo/valor | Quem escreve | Quem lê |
|---|---|---|
| `cobertura_parcial` | Builder consulta integralidade e carimba; leitor da ata também carimba pela cobertura vigente; cartório propaga para a ata. (`app/escala/utils.py:1151`, `app/escala/utils.py:1191`, `app/escala/services/leitor_celula.py:409`, `app/ponto/services/cartorio.py:628`) | Supra; impressão; dispensa integral; contagem de emissão/espelho; status; predicado usado por `dia_sem_trabalho` e declaração textual. (`app/ponto/supra_juiz.py:268`, `app/ponto/services/cartorio.py:345`, `app/ponto/services/cartorio.py:653`, `app/ponto/services/cartorio.py:1547`, `app/ponto/services/cartorio.py:1631`, `app/ponto/status_dia.py:150`, `app/chamados/juizes.py:408`, `app/chamados/services/declaracao_texto.py:98`) |
| `periodo_parcial` | Builder e leitor da ata chamam o juiz de período. Não é persistido por `ata_do_dia`; é recalculado pelo leitor. (`app/escala/utils.py:1195`, `app/escala/services/leitor_celula.py:412`, `app/ponto/services/cartorio.py:600`) | Dispensa por marco, impressão e ramos do supra. (`app/escala/utils.py:1204`, `app/ponto/services/cartorio.py:345`, `app/ponto/supra_juiz.py:381`, `app/ponto/supra_juiz.py:439`, `app/ponto/supra_juiz.py:530`) |
| Célula `dispensada` | Grade dispensa missing dentro do período; leitor reconstrói o marcador da lâmpada. (`app/escala/utils.py:1206`, `app/escala/services/leitor_celula.py:127`) | Impressão, propagação à lâmpada, exclusão de batida no cartório/status e ausência de resto no supra. (`app/ponto/services/cartorio.py:346`, `app/ponto/services/cartorio.py:457`, `app/ponto/services/cartorio.py:1217`, `app/ponto/status_dia.py:145`, `app/ponto/supra_juiz.py:311`) |
| Lâmpada `dispensada` | Propagação da célula; dispensa integral/justificativa no cartório. (`app/ponto/services/cartorio.py:458`, `app/ponto/services/cartorio.py:662`) | Reconstrução da célula e `_n_dispensadas`; juízes de lastro/furo leem o efeito `acesa`, sem exigir luz. (`app/escala/services/leitor_celula.py:68`, `app/escala/services/leitor_celula.py:127`, `app/ponto/services/lastro.py:179`, `app/chamados/juizes.py:478`) |
| `n_ok_lavrado` / `n_casadas` | Derivados de contagens da ata menos dispensadas; transportados no dia. (`app/escala/services/leitor_celula.py:91`, `app/escala/services/leitor_celula.py:97`, `app/escala/services/leitor_celula.py:369`) | Supra prefere os valores lavrados; contador de bordas usa `n_ok` como prova de batida. (`app/ponto/supra_juiz.py:229`, `app/ponto/supra_juiz.py:464`, `app/ponto/services/bordas_realizado.py:51`) |
| `_teve_fato` | Variável local derivada de realizado ou contagem positiva. (`app/escala/services/leitor_celula.py:343`) | `_fora`, que decide exclusão antes da admissão e no dia da demissão. (`app/escala/services/leitor_celula.py:347`) |
| `parcial_na_lista_sem_push` | `_contar_na_lista`, chamado nos dois portões de emissão. (`app/ponto/services/cartorio.py:1673`, `app/ponto/services/cartorio.py:1564`, `app/ponto/services/cartorio.py:1616`) | Incremento e testes; o resumo do comando não imprime essa chave. (`app/ponto/services/cartorio.py:1673`, `app/ponto/tests/test_cobparc_marco_no_periodo.py:451`, `app/ponto/management/commands/processar_cartorio.py:66`) |
| `_parcial_na_lista_vistos` | `setdefault` e `add`; chave `(colab,data)`. (`app/ponto/services/cartorio.py:1551`, `app/ponto/services/cartorio.py:1670`) | Teste de pertinência; `finally` remove o conjunto da saída. (`app/ponto/services/cartorio.py:1671`, `app/ponto/services/cartorio.py:922`) |
| Âncora parcial | `_marcos_do_furo_parcial`. (`app/ponto/services/cartorio.py:1719`) | Ambos os ramos de emissão; emissor calcula atraso, contexto e push. (`app/ponto/services/cartorio.py:1580`, `app/ponto/services/cartorio.py:1619`, `app/ponto/management/commands/detectar_ausencias.py:581`, `app/ponto/management/commands/detectar_ausencias.py:702`, `app/ponto/management/commands/detectar_ausencias.py:776`) |
| `REGRA_VERSOES` | Declaração no cartório. (`app/ponto/services/cartorio.py:111`) | Validação, seleção da maior versão aplicável, impressão e testes de alcance. (`app/ponto/services/cartorio.py:179`, `app/ponto/services/cartorio.py:208`, `app/ponto/services/cartorio.py:380`, `app/ponto/tests/test_o219_versao_da_regra.py:77`) |
| Código / `coberta` / `cobrado` | Código nasce no supra; mapa deriva `coberta`; cartório propõe `cobrado`; porta decide e persiste. (`app/ponto/supra_juiz.py:532`, `app/escala/models.py:1582`, `app/ponto/services/cartorio.py:1295`, `app/ponto/portas/celula.py:658`, `app/ponto/portas/celula.py:745`) | Consumidores discriminados no achado 7. (`app/chamados/juizes.py:1102`, `app/folha/export.py:1215`, `app/relatorios/furos.py:24`) |
| Retorno `_coberto_por_ausencia` | Predicado de materialização; reexportado por models e importado por disputa. (`app/chamados/services/materializacao.py:55`, `app/chamados/models.py:3141`, `app/chamados/services/disputa_emissao.py:8`) | Retenção da pergunta no app e guarda antes da abertura da pergunta. (`app/chamados/services/pergunta_no_app.py:74`, `app/chamados/services/disputa_emissao.py:763`) |

**9. Contraexemplos restantes e limites dos testes.** [nao verificado]

- **Todos os marcos dispensados, chamado vivo:** não há furo; o supra puro mantém `COBRANCA_EM_DIA_COBERTO`. No cartório, chamado com marcos nomeados todos acesos deixa de protestar e pode ser fechado pelo fluxo por marco. Isso respeita “cobra o resto”: não sobrou marco. (`app/ponto/supra_juiz.py:311`, `app/ponto/supra_juiz.py:530`, `app/ponto/services/lastro.py:170`, `app/ponto/services/cartorio.py:1250`)
- **Hoje/VIGIA:** missing futuro sai do dia julgado e vira lâmpada `acesa=None, pendente=True`. Se nenhum missing vencido sobra, o código de cobertura pode aparecer; as guardas de pergunta e retratação impedem tratar a ata provisória como absolvição definitiva. (`app/ponto/services/cartorio.py:737`, `app/ponto/services/cartorio.py:755`, `app/chamados/juizes.py:1115`, `app/chamados/reconciliador.py:984`)
- **Fila — contagem ainda diverge com VIGIA:** duas dispensadas e duas pendentes, todas sem luz, dão `n_ok_lavrado=2`, mas a recontagem por hora dá zero. Isso também faz `_teve_fato=True`; subtrair apenas dispensadas não garante a igualdade anunciada pelos testes. (`app/escala/services/leitor_celula.py:91`, `app/ponto/supra_juiz.py:83`, `app/escala/services/leitor_celula.py:344`, `app/ponto/tests/test_cobparc_marco_no_periodo.py:370`)

Os testes atravessam grade→ata→supra, julgamento→pergunta viva, edição→impressão→rejulgamento, declaração textual, espelho e duas passadas do contador. (`app/ponto/tests/test_cobparc_marco_no_periodo.py:175`, `app/ponto/tests/test_cobparc_marco_no_periodo.py:304`, `app/ponto/tests/test_cobparc_marco_no_periodo.py:354`, `app/ponto/tests/test_cobparc_marco_no_periodo.py:397`, `app/ponto/tests/test_cobparc_marco_no_periodo.py:426`, `app/ponto/tests/test_cobparc_marco_no_periodo.py:441`)

**Sim, testes passam com cura revertida:** os três controles finais preservam exatamente dia inteiro, parcial sem período e parcial sem resto. O RED de retirada do código distingue a cura; os dois testes de contagem distinguem a subtração das dispensadas. (`app/ponto/tests/test_cobparc_marco_no_periodo.py:476`, `app/ponto/tests/test_cobparc_marco_no_periodo.py:480`, `app/ponto/tests/test_cobparc_marco_no_periodo.py:484`, `app/ponto/tests/test_cobparc_marco_no_periodo.py:473`, `app/ponto/tests/test_cobparc_marco_no_periodo.py:380`, `app/ponto/tests/test_cobparc_marco_no_periodo.py:385`)

A lacuna bloqueante é concreta: teste noturno pula a construção da régua; teste de âncora é diurno e verifica somente `.time()`, não a data. (`app/ponto/tests/test_cobparc_marco_no_periodo.py:43`, `app/ponto/tests/test_cobparc_marco_no_periodo.py:423`)

**10. Checklist estrutural e pouso.** [nao verificado]

| Classe | Conclusão |
|---|---|
| Segundo juiz | O novo helper apenas lê o carimbo; não recalcula integralidade. Porém o `any()` da materialização conserva seleção paralela de cobertura, demonstrada no achado 3. (`app/chamados/juizes.py:408`, `app/docs/JUIZES.md:60`, `app/chamados/services/materializacao.py:74`) |
| Segundo escritor | Os ramos novos reutilizam `_espelhar_marcos`→`renovar_contexto`; ata e veredito continuam pelas portas. Segundo escritor persistente nesses ramos: não demonstrado. (`app/ponto/services/cartorio.py:1639`, `app/ponto/management/commands/detectar_ausencias.py:553`, `app/chamados/services/contexto.py:96`, `app/ponto/services/cartorio.py:1643`) |
| Relógio fora da borda | **Fila, preexistente:** `julgar_celula` aceita `agora`, mas consulta data e instante reais antes de repassá-lo. Caso: instante injetado diferente do host altera o portão temporal. (`app/ponto/services/cartorio.py:1778`, `app/ponto/services/cartorio.py:1783`, `app/ponto/services/cartorio.py:1804`) |
| Fonte única | **Bloqueia:** cobertura por `any()` diverge do desempate canônico; cadeia noturna recebe régua inadequada. (`app/chamados/services/materializacao.py:74`, `app/ponto/turnos.py:2784`, `app/escala/utils.py:1335`) |
| Ordem de trava | Abertura mantém trava do colaborador; atualização de contexto usa sua porta existente. Inversão/deadlock causado pela obra: **[nao verificado]**. (`app/chamados/models.py:861`, `app/chamados/services/contexto.py:96`) |
| Versão 3 / competência 09 | O horizonte alcança **toda competência ≥09**, não apenas cinco dias, e invalida impressões inclusive sem mudança de marcos dispensados. Não seleciona uma implementação histórica isolada. (`app/ponto/services/cartorio.py:208`, `app/ponto/services/cartorio.py:380`, `app/ponto/services/cartorio.py:1192`) |

A 09 exportada, por si só, não impede rejulgar ata: o aval escrito autoriza isso e exige fechamento intacto com hashes antes/depois e destino da diferença financeira. A guarda existente exclui do recálculo os colaboradores das empresas exportadas da 09. (`a

[truncado em 20000 de 20455 caracteres]

## 01/11 00:54 · sessao 8e53981aa4d4 · - · - · origem=Ronald (tmux)

Achados e matriz, somente código:

1. **Bloqueia — âncora noturna errada, agora usada também no ramo de parcial sem batida.** `_marcos_do_furo_parcial` ordena `HH:MM` lexicograficamente e combina a hora com a data inicial, sem encadear madrugada (`app/ponto/services/cartorio.py:1717-1720`). O ramo novo adota essa âncora (`app/ponto/services/cartorio.py:1578-1582`). Caso: turno D 19h–D+1 07h, parcial 19h–23h dispensa E19; S07 segue apagada. A cobrança fica ancorada **D 07h**, 24h antes de seu marco. Com mais marcos apagados, a ordenação pode escolher a madrugada antes dos marcos anteriores à meia-noite. A âncora alimenta `atraso_min`, contexto e texto do push (`app/ponto/management/commands/detectar_ausencias.py:581`, `:702-703`, `:776`). **Corrigindo minha mensagem anterior:** o SLA usa `agora`, portanto não demonstrei mudança direta do prazo por essa âncora (`app/ponto/management/commands/detectar_ausencias.py:593-597`).

2. **Bloqueia — calendário ainda absolve visualmente o dia parcial sem batida.** A classificação retorna `ausencia` antes de ler o veredito quando há faixa de ausência e nenhuma batida (`app/colaboradores/services/calendario.py:109-110`). `declaracao` cai na faixa padrão `atestado` (`app/ponto/precedencia.py:76-79`). O chamador não fornece cobertura parcial (`app/colaboradores/services/calendario.py:799-819`). Mesmo caso parcial com resto cobrado: status da lavra fica `irregular` (`app/ponto/status_dia.py:98-113`), folha retém `cobrado` (`app/escala/models.py:1735-1736`; `app/folha/export.py:1215`, `:1235-1236`), mas calendário continua `ausencia`. Isso também mantém o dia fora de “Resolver dia”, cujo universo exige `falta|irregular|aberto|sem_certidao` (`app/colaboradores/services/calendario.py:181`).

3. **Matriz exata da retirada do código.** A nova condição retira `COBRANCA_EM_DIA_COBERTO` se, e somente se, cobertura aprovada, `chamados_vivos` verdadeiro, `cobertura_parcial` verdadeira, `_n_missing>0`, `periodo_parcial != 'sem_periodo'` (`app/ponto/supra_juiz.py:255`, `:268`, `:306`, `:530-534`). Assim:

   | Campo | Combinações alteradas |
   |---|---|
   | `tipo_dia` | Qualquer valor formal; não há guarda de tipo nesse ramo |
   | `cobertura_parcial` | Verdadeiro |
   | `periodo_parcial` | `dentro`, `fora`, `nao_sei`, `None`/ausente e qualquer valor diferente de `sem_periodo` |
   | `_n_missing` | >0 |
   | `chamados_vivos` | >0 |
   | `cobrancas_vivas` | Tanto 0 como >0 |

   Fonte da tabela: `app/ponto/supra_juiz.py:530-534`. Falta real não produz essa combinação: a grade apaga suas células e não carimba parcial (`app/escala/utils.py:1174-1179`); o outro produtor só carimba `tipo_dia='ausencia'`, excluindo falta (`app/escala/services/leitor_celula.py:399-412`).

4. **Não é verdadeiro que retirar esse código sempre produz `cobrado`.** Se sobraram códigos de furo ou cobrança indevida, a precedência já fazia esses vereditos vencerem: nesses casos não há mudança de veredito (`app/escala/models.py:1580-1582`). Se antes `coberta` vencia códigos de cadastro, discordância ou fato em ausência, esses códigos passam a determinar o veredito (`app/escala/models.py:1582-1599`). Se não sobra código, `derivar_veredito([])` é `concorde` (`app/escala/models.py:1593-1596`); somente o cartório injeta `cobrado` quando existe fio vivo **e protestante** (`app/ponto/services/cartorio.py:1271-1295`). Decisão administrativa continua prevalecendo (`app/ponto/portas/celula.py:655-658`).

   No caso padrão de ausência parcial, passado encerrado, previsto>0, sem fato e fio morto, **não** encontrei vazio: sai `FURO_COM_COBRANCA_MORTA` (`app/ponto/supra_juiz.py:304-316`, `:360-362`). Vazio sem `cobrado` existe formalmente com flag espúrio em tipo não cobravel, ou com turno não encerrado/previsto zero; ocorrência real desses insumos na frota `[nao verificado]`. Não vender esse caso sintético como regressão produtiva.

5. **Todos dispensados com chamado vivo:** o supra puro continua emitindo `COBRANCA_EM_DIA_COBERTO`, porque `_n_missing=0` (`app/ponto/supra_juiz.py:530-534`), e não emite furo porque `_parcial_sem_resto` o bloqueia (`:311-316`). No cartório, chamado com marcos nomeados todos dispensados não protesta: `marcos_acesos` aceita `acesa=True`, sem exigir luz (`app/ponto/services/lastro.py:170-181`), e `_protesta` o exclui (`app/ponto/services/cartorio.py:1250-1258`). Assim pode sair `concorde` diretamente e fechar pelo fluxo de marcos; chamado sem marcos nomeados permanece indeterminado nesse juiz (`app/ponto/services/lastro.py:170-172`), permitindo `coberta` e fechamento por absolvição. Não há resto a cobrar nesse contraexemplo (`app/ponto/supra_juiz.py:307-316`).

6. **Hoje/VIGIA:** o corte remove somente células `missing` pendentes (`app/ponto/services/cartorio.py:737-745`) e deixa a lâmpada correspondente `acesa=None, pendente=True` (`:748-758`). O supra recebe o dia cortado e encerrado até esse teto (`:1221-1222`, `:1264-1266`). Se todo missing remanescente estava no futuro, `_n_missing=0` permite novamente `COBRANCA_EM_DIA_COBERTO`; isso não deve matar o fio dos marcos pendentes: a guarda existe tanto na pergunta (`app/chamados/juizes.py:1115-1117`) quanto na retratação do chamado (`app/chamados/reconciliador.py:982-985`). Depois do vencimento, o missing volta ao julgamento pela alteração de teto na impressão (`app/ponto/services/cartorio.py:1192-1197`). Não demonstrei falha nova nessa costura.

Consumidores de `coberta` e efeito da mudança:

| Consumidor | Leitura e efeito |
|---|---|
| Derivação/escrita | Código nasce no supra `:532`; vira `coberta` em `app/escala/models.py:1582`; porta decide em `app/ponto/portas/celula.py:647-661` e lavra em `:745-746`. |
| Pergunta/retratação | `coberta` está fora de `VEREDITOS_ACUSAM` (`app/escala/models.py:1735-1736`): pergunta sem resposta morre pelo degrau da célula, salvo teto (`app/chamados/juizes.py:1102-1117`); chamado pode ser retratado por absolvição (`app/chamados/reconciliador.py:973-986`). `cobrado/furo/...` mantêm acusação. |
| Veto de emissão/reabertura | `coberta` veta em `app/ponto/marcos.py:119`, `:154-162`; `cobrado` não. Chamadores incluem detectar ausências `:625-626`, reconciliar grade `:502`, reconciliador `:768`. |
| Status lavrado | Sem batida: `coberta` dá ausência; parcial com `cobrado/furo` dá irregular (`app/ponto/status_dia.py:98-113`). Com batida: `coberta` dá ok, `cobrado` dá irregular (`:117-120`). |
| Calendário | Mapa `coberta→ok`, `cobrado→irregular` (`app/colaboradores/services/calendario.py:54-58`), mas o retorno antecipado citado acima continua ocultando a mudança para ausência sem batida (`:109-110`). |
| Folha/retenção | `coberta` não entra em `VEREDITOS_ACUSAM`; novo `cobrado/furo/...` retém, ressalvadas exceções de protestos não cobraveis e pausa (`app/folha/export.py:1215-1236`). Não demonstrado recálculo do FechamentoMensal por essa mudança isolada `[nao verificado]`. |
| Relatório de furos | `coberta→folga`, elimina faltantes; `cobrado→furo` mantém faltantes (`app/relatorios/furos.py:24-52`). |
| Dia da frota | `coberta` aparece como “coberto por ausência”; `cobrado` como “cobrado, aguardando resposta” (`app/relatorios/dia_da_frota.py:10-15`), e só os acusadores entram na lista de cobrança (`:54-61`). |

Executei em memória `classificar_dia` com `python3 -B`, sem Django/banco/arquivos de saída; os resultados confirmaram os ramos descritos. Execução da suíte Django e ocorrências produtivas `[nao verificado]`. [nao verificado]

## 01/11 00:54 · sessao dc02185ca894 · - · - · origem=Ronald (tmux)

Achado **bloqueia — âncora noturna ainda usa data civil errada**. O ramo novo passa a âncora de `_marcos_do_furo_parcial` ao emissor (`app/ponto/services/cartorio.py:1578-1583`), mas o auxiliar ordena por `HH:MM` e combina com a data inicial, sem encadear virada (`app/ponto/services/cartorio.py:1717-1720`). Caso concreto: turno D 19:00→D+1 07:00, parcial D 19:00→23:00, nenhuma batida, E19 dispensada e S07 missing; a cobrança ancora em **D 07:00**, 24 horas antes do marco real. Isso altera `atraso_min`, `horario_previsto` e texto do push (`app/ponto/management/commands/detectar_ausencias.py:581`, `:702`, `:776`); SLA usa `agora`/janela, portanto não atribuir à âncora mudança do SLA (`:591-597`). A autoridade correta da virada já existe em `deslocamentos_dos_marcos`, que chama `encadear_marcos` (`app/ponto/turnos.py:2644-2661`).

**Testes / mordida.** [nao verificado]

- A costura principal tem testes reais de grade→ata→supra (`app/ponto/tests/test_cobparc_marco_no_periodo.py:175-202`), julgamento→emissão→pergunta viva (`:304-318`), edição de período→impressão→rejulgamento (`:347-362`), declaração textual (`:397-404`), espelho de chamado vivo (`:426-439`) e contador em duas passadas (`:441-452`). Isso é cobertura estática observada; execução RED/GREEN e suite permanecem **[nao verificado]**.
- A lacuna que deixa passar o achado noturno é concreta: testes noturnos só chamam `_grade_pura` (`:64-80`); o teste de âncora é diurno e confere apenas `.time()`, sem data (`:416-424`). **bloqueia**, junto ao bug da âncora. [nao verificado]
- `_grade_pura` força `cobre_parte_do_dia=True` (`:49-51`), portanto esses testes não verificam a ligação ausência real→juiz de integralidade→carimbo. **fila** para cobertura dessa costura. [nao verificado]
- Sim, alguns testes passam com a cura revertida: os controles `test_MORDE_dia_inteiro_segue_coberta`, `...parcial_sem_periodo...` e `...parcial_sem_resto...` afirmam exatamente o comportamento anterior do código (`:476-487`; comparação com diff dado). Isso é adequado como controles, mas não prova a cura. O RED que a distingue exige ausência do código para parcial com resto (`:473-474`). [nao verificado]
- `LavradoNaoContaDispensadaTest` morde a subtração das dispensadas: antigo `4−2=2` viola esperado zero (`:374-383`); o controle com uma luz exige 1, também falhando com cálculo antigo (`:385-389`). Nenhuma reversão foi executada **[nao verificado]**.
- As combinações tipo `falta`, duas ausências, hoje cortado pelo vigia e âncora noturna não aparecem como integração no arquivo de teste inspecionado (`:40-487`); confirmação de execução adversarial dessas classes é **[nao verificado]**. [nao verificado]

**Escritores/leitores.**

| Valor | Escrita | Leitura |
|---|---|---|
| `REGRA_VERSOES` | declaração única em `app/ponto/services/cartorio.py:111` | validação de formato `:179-187`; `versao_para` escolhe maior versão aplicável `:203-209`; cache do julgamento `:988-991`; impressão inclui `regra:` `:380-381`, usada no julgamento `:1192-1197`; testes de declaração/alcance `app/ponto/tests/test_o219_versao_da_regra.py:77`, `:87-93` |
| âncora parcial | `_marcos_do_furo_parcial` `app/ponto/services/cartorio.py:1717-1721` | novo ramo de furo `:1578-1583`; parcial com fato `:1619-1621`; emissor escreve contexto/atraso `app/ponto/management/commands/detectar_ausencias.py:581`, `:702-703`; push `:776` |
| `out['parcial_na_lista_sem_push']` | `_contar_na_lista` `app/ponto/services/cartorio.py:1673` | incremento lê valor anterior na mesma linha; testes `app/ponto/tests/test_cobparc_marco_no_periodo.py:326`, `:451`; resumo do command não o imprime (`app/ponto/management/commands/processar_cartorio.py:66-68`) |
| `_parcial_na_lista_vistos` | `setdefault` + `.add` `app/ponto/services/cartorio.py:1670-1672` | pertinência `:1671`; limpo pelo `finally` `:922`; teste garante ausência na saída `app/ponto/tests/test_cobparc_marco_no_periodo.py:452` |
| invocação do contador | gates de emissão `app/ponto/services/cartorio.py:1564-1565`, `:1616-1617` | segunda passada reutiliza `out` em `:843`; chave `(c.pk,data)` em `:1551` |

**fila — contador sem consumidor explícito de produção encontrado**. A chave nova não nasce em `contadores` (`app/ponto/services/cartorio.py:1724-1737`), só é criada quando conta (`:1673`), e o resumo de produção omite-a (`app/ponto/management/commands/processar_cartorio.py:66-68`). A lista administrativa alegada deve ser demonstrada pelo veredito/consumidor, não pelo contador; visibilidade operacional do número é **[nao verificado]**.

**Checklist estrutural.**

- **Segundo escritor:** espelho novo reutiliza `_espelhar_marcos` (`app/ponto/services/cartorio.py:1635-1639`), que chama a porta `renovar_contexto` (`app/ponto/management/commands/detectar_ausencias.py:553-559`), cujo save é canônico (`app/chamados/services/contexto.py:96-102`). Lavra/ata continuam por `lavrar_veredito` e `carimbar` (`app/ponto/services/cartorio.py:1643`, `:1663`); não há segundo escritor persistente demonstrado nesses novos ramos.
- **Relógio:** novas operações da âncora usam `make_aware/combine`, sem consultar relógio (`app/ponto/services/cartorio.py:1568`, `:1719-1720`). `julgar_colab` recebe `hoje/agora` (`:781-782`); command lê relógio na borda (`app/ponto/management/commands/processar_cartorio.py:38-39`). Há relógios preexistentes em `julgar_celula` apesar de `agora` injetado (`app/ponto/services/cartorio.py:1783`, `:1804`, `:1807`): **fila**, caso teste injete instante diferente do host.
- **Ordem de trava:** novos ramos não introduzem aquisição explícita; contexto usa `atomic` apenas na escrita e lê instância sem `select_for_update` (`app/chamados/services/contexto.py:96-102`, documentação `:25-30`). Abertura trava Colaborador antes do dedup (`app/chamados/models.py:861-865`); fechamento existente trava chamado dentro do atomic (`:666-673`). Deadlock concreto causado pela obra: **[nao verificado]**.
- **Pré-requisito 09:** versão 3 alcança toda competência ≥09, não só cinco dias (`app/ponto/services/cartorio.py:111`, `:205-209`); hash muda para todos esses julgamentos (`:380-381`). Comentário reconhece a chegada conjunta da regra 2 à 09 (`:174-176`). O dado fornecido não contém medição nominal, hashes antes/depois, diferença financeira ou Pauta DP: **[nao verificado]**; não tratar a docstring `:169-176` como execução comprovada.
- O código protege escrita do fechamento da 09 exportada por guarda existente (`app/ponto/services/fechamento.py:36-42`, `:544-548`). Rejulgar ata da 09 não equivale automaticamente a regravar FechamentoMensal (`app/ponto/services/cartorio.py:1641-1664`). **Não há bloqueio técnico demonstrado só por a 09 estar EXPORTADA**; confirmação do pré-requisito documental do aval fica **[nao verificado]**.
- O teste da versão morde declaração e janela (`app/ponto/tests/test_o219_versao_da_regra.py:77`, `:87-93`), mas não confirma hashes de FechamentoMensal/Pauta DP; essas garantias permanecem **[nao verificado]**.

## 01/11 00:54 · sessao e754a970d942 · - · - · origem=Ronald (tmux)

**Bloqueia — a fábrica escolhe cobertura diferente da grade quando há duas ausências.** `materializacao._coberto_por_ausencia` usa `any()` sobre **todas** as ausências não rejeitadas, excluindo falta; a grade e `_celula_furada` usam a vencedora por maior `(data, pk)`. Caso: atestado integral antigo + declaração parcial com período mais recente, aprovada, com marco faltante fora do período. A grade conserva o resto cobrável; `_celula_furada` responde `True`; a fábrica retorna cobertura por causa do atestado antigo, impedindo perguntas. Fontes: `app/chamados/services/materializacao.py:74`, `app/ponto/turnos.py:2552`, `app/ponto/turnos.py:2772`, `app/ponto/turnos.py:2784`, `app/escala/utils.py:1149`, `app/escala/utils.py:1191`, `app/chamados/juizes.py:446`, `app/chamados/juizes.py:478`. A propagação ocorre em `app/chamados/services/disputa_emissao.py:763` e `app/chamados/services/pergunta_no_app.py:74`.

**Bloqueia — leitor de cobrança não migrado em c2.** `reconciliador.py` continua vetando qualquer ausência retornada por `ausencia_cobre`, inclusive a parcial com período que deixa marco apagado. Caso: pergunta órfã de um marco externo à parcial, cuja religação precisaria criar, reabrir ou costurar o chamado do dia: c2 retorna `False` pela mera existência da ausência. Fontes: `app/chamados/reconciliador.py:825`, `app/chamados/reconciliador.py:826`; a costura no chamado já vivo também passa por esse veto (`app/chamados/reconciliador.py:811`, `app/chamados/reconciliador.py:809`).

**Bloqueia — noturno com intervalo usa ordem da régua de apresentação como cadeia do turno.** Para turno E19/S02/E03/S07, a régua união escolhe a menor entrada E03 como âncora e produz `[E03,S07,E19,S02]`. `deslocamentos_dos_marcos` encadeia essa ordem e obtém 180/420/1140/1560 minutos; parcial 23–07 abrange [1380,1860], dispensando S02, mas deixando E03 e S07 como faltantes, embora ambos pertençam à madrugada D+1 dentro do período. Fontes: `app/escala/utils.py:1335`, `app/escala/utils.py:1343`, `app/escala/utils.py:1362`, `app/escala/utils.py:1805`, `app/ponto/turnos.py:2652`, `app/ponto/turnos.py:2661`, `app/escala/utils.py:345`, `app/ponto/turnos.py:2636`, `app/escala/utils.py:1209`. O teste noturno usa somente E19/S07, portanto não atravessa essa combinação da régua união com a cadeia (`app/ponto/tests/test_cobparc_marco_no_periodo.py:53`, `app/ponto/tests/test_cobparc_marco_no_periodo.py:62`).

**Bloqueia — âncora do resto noturno perde a data da madrugada.** Mesmo no noturno simples corretamente dispensado, `_marcos_do_furo_parcial` ordena `HH:MM` e combina o primeiro horário com **D**, sem a cadeia temporal. Caso: turno D19–D+1 07, parcial D19–23 dispensa E19, sobra S07; a nova emissão ancora em D07, 24 horas cedo. Fontes: `app/ponto/services/cartorio.py:1717`, `app/ponto/services/cartorio.py:1719`, `app/ponto/services/cartorio.py:1580`, `app/ponto/services/cartorio.py:1584`. Isso soma 1.440 minutos ao atraso, grava `horario_previsto` errado e aparece no push (`app/ponto/management/commands/detectar_ausencias.py:581`, `app/ponto/management/commands/detectar_ausencias.py:702`, `app/ponto/management/commands/detectar_ausencias.py:776`). **O SLA nessa porta usa `agora`, não a âncora** (`app/ponto/management/commands/detectar_ausencias.py:593`, `app/ponto/management/commands/detectar_ausencias.py:597`).

**Fila — falta expõe limite explícito da definição declarada de parcial.** `ausencia_explica_o_dia_inteiro` retorna `False` para qualquer falta, independentemente de minutos; contudo a grade trata falta em ramo próprio, apaga células e órfãs, não grava cobertura parcial, e o supra exclui falta de `dia_de_cobranca`. Logo “juiz retorna False” não equivale universalmente a “grade grava parcial e cobra resto”. Fontes: `app/ponto/turnos.py:2690`, `app/escala/utils.py:1174`, `app/escala/utils.py:1178`, `app/ponto/supra_juiz.py:302`, `app/ponto/supra_juiz.py:304`. O teste preserva deliberadamente `dia_sem_trabalho=True` para falta mesmo com carimbo parcial (`app/chamados/tests/test_emis_ausencia_parcial_cobra.py:50`). O catálogo padrão declara falta não lançável por minutos e a porta valida pelo catálogo vivo; existência de falta parcial concreta em produção: `[nao verificado]` (`app/ponto/catalogo/ausencias.py:88`, `app/ponto/services/ausencia.py:206`).

**Sem defeito no argumento vazio de período.** Com ambos os horários declarados, `periodo_da_parcial_no_dia(a, ())` retorna `fora`; sem um deles, retorna `sem_periodo`, independentemente das batidas. Portanto a comparação exclusiva com `'sem_periodo'` detecta ausência de cadastro e não muda por esvaziar o segundo argumento (`app/ponto/turnos.py:2632`, `app/ponto/turnos.py:2638`, `app/ponto/turnos.py:2641`). Ela não resolve o defeito do `any()` sobre ausências perdedoras (`app/chamados/services/materializacao.py:74`).

| Sítio | Predicado efetivamente lido |
|---|---|
| `_coberto_por_ausencia` | Qualquer ausência não falta, não rejeitada, que explique dia inteiro **ou** esteja sem período (`app/chamados/services/materializacao.py:74`). |
| `disputa_emissao` | Mesmo retorno, por importação e guarda antes de `PerguntaDisputa.abrir` (`app/chamados/services/disputa_emissao.py:8`, `app/chamados/services/disputa_emissao.py:763`, `app/chamados/services/disputa_emissao.py:770`). |
| `pergunta_no_app.retencao` | Mesmo retorno, para causa `coberto_por_ausencia` (`app/chamados/services/pergunta_no_app.py:60`, `app/chamados/services/pergunta_no_app.py:74`). |
| `dia_sem_trabalho` | Ata parcial de tipo ausência só cala se `trabalha=False`; outros tipos preenchidos diferentes de trabalho calam (`app/chamados/juizes.py:408`, `app/chamados/juizes.py:424`, `app/chamados/juizes.py:426`). |
| `_celula_furada` | Primeiro: ausência **vencedora** integral cala; depois lê ata, dia sem trabalho, lâmpadas apagadas e órfãs (`app/chamados/juizes.py:446`, `app/chamados/juizes.py:464`, `app/chamados/juizes.py:478`, `app/chamados/juizes.py:480`). |
| c2 | Qualquer ausência vencedora cala, sem ler integralidade ou período (`app/chamados/reconciliador.py:825`). |

**Diferença intencional a preservar:** parcial sem período e com lâmpada apagada pode dar `dia_sem_trabalho=False`, `_celula_furada=True`, mas `_coberto_por_ausencia=True`, porque a fábrica bloqueia a pergunta ao colaborador e remete o problema ao cadastro; não são respostas à mesma pergunta operacional (`app/chamados/juizes.py:424`, `app/chamados/juizes.py:478`, `app/chamados/services/materializacao.py:69`, `app/chamados/services/materializacao.py:74`).

**Outro leitor residual, de metadado:** `via_pelo_fato` ainda chama qualquer ausência aprovada de `VIA_AUSENCIA_DIA`, inclusive parcial com resto; esse valor significa “dia resolvido pela ausência”. Isso pode rotular erroneamente a testemunha de uma resolução; morte automática da pergunta por esse caminho, isoladamente, não demonstrada, porque o juiz de pergunta prioriza o veredito acusador da célula (`app/chamados/juizes.py:1381`, `app/chamados/juizes.py:1383`, `app/chamados/models.py:2261`, `app/chamados/juizes.py:1102`, `app/chamados/juizes.py:1104`). Gravidade: **fila**.

## 01/11 00:47 · sessao 8ff112be9bdd · FASE-CONFLITANTE-VINCULO-DO-DIA · censo · origem=Code

**O contador e a autoridade usam universos diferentes:** `medir` seleciona vínculos por `ativa=True` e os percorre pela janela inteira; a autoridade considera a geradora da célula e a vigência. Isso demonstra diferença de seleção, mas ocorrência e impacto no contador de produção são **[nao verificado]**. Fontes: [fase_conflitante.py:71](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/services/fase_conflitante.py:71), [alimentacao.py:237](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/alimentacao.py:237).

1. **Sítios em `fase_conflitante.py`**

   Há **uma seleção de universo de vínculos**, sem uma escolha individual de “vínculo do dia”:

   | Sítio | Decisão e fonte |
   |---|---|
   | [fase_conflitante.py:71](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/services/fase_conflitante.py:71) | Query direta de `EscalaColaborador`: inclui somente `ativa=True`. Não filtra colaborador, início ou fim. |
   | [fase_conflitante.py:73](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/services/fase_conflitante.py:73), [fase_conflitante.py:78](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/services/fase_conflitante.py:78) | Propagação dessa seleção: cada `ec` é usado para **todos** os dias da janela, sem testar se cobre aquele dia e sem consultar célula ou autoridade. |

   Os demais pontos não escolhem vínculo: a foto vem de query de **todas as `FolgaDia` daquele `ec`**, sem recorte de datas; `_ancora_diz` lê os campos `data_ancora_colaborador` e `tipo_escala.tipo_ciclo`; `_foto_diz` recebe o conjunto de datas. A ausência de foto elimina o vínculo da medição, não declara ausência de vínculo no dia. Fontes: [fase_conflitante.py:74](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/services/fase_conflitante.py:74), [fase_conflitante.py:40](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/services/fase_conflitante.py:40), [fase_conflitante.py:51](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/services/fase_conflitante.py:51).

2. **Autoridade registrada e predicado**

   É `escala/alimentacao.py::vinculo_do_dia`, definida em [alimentacao.py:196](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/alimentacao.py:196), registrada em [juizes.py:1270](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/core/juizes.py:1270) e indicada em [JUIZES.md:85](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/docs/JUIZES.md:85).

   - **Com geradora:** lê `escala_geradora_id` da célula e procura esse vínculo na lista recebida. Se faltar ou estiver fora da vigência, retorna `None`, sem escolher outro. As bordas são inclusivas; nesse ramo não exige vínculo ativo. Fonte: [alimentacao.py:237](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/alimentacao.py:237).
   - **Sem geradora:** usa `vinculo_cobre_a_data`: início não nulo, `início <= dia`, fim nulo ou `fim >= dia`, e `ativa or data_fim is not None`. Entre os elegíveis, escolhe o máximo por `(ativa, data_inicio)`. Fontes: [alimentacao.py:164](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/alimentacao.py:164), [alimentacao.py:182](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/alimentacao.py:182), [alimentacao.py:253](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/alimentacao.py:253).

   Portanto, encerrado pode responder dentro da vigência; futuro não responde antes do início; inativo sem fim é recusado no fallback, embora possa responder como geradora dentro dos limites declarados. Fontes: [alimentacao.py:182](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/alimentacao.py:182), [alimentacao.py:247](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/alimentacao.py:247).

3. **Comparação e casos de divergência**

   **Não são iguais.** O único seletor do contador exige atividade atual; sua aplicação por dia não verifica vigência nem geradora. A autoridade verifica esses elementos e retorna um vínculo ou `None`. Fontes: [fase_conflitante.py:71](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/services/fase_conflitante.py:71), [fase_conflitante.py:78](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/services/fase_conflitante.py:78), [alimentacao.py:237](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/alimentacao.py:237).

   Há uma contraprova **já escrita como fixture**, do colaborador `E4 C`: vínculo encerrado inativo de **01/09/2026 a 30/09/2026**, ativo desde **01/10/2026**, futuro ativo desde **01/11/2026**, além de inativo sem fim desde agosto. Fonte: [test_e4_vinculo_cobre_a_data.py:24](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/tests/test_e4_vinculo_cobre_a_data.py:24).

   | Dia da fixture | Autoridade | Universo da query do contador |
   |---|---|---|
   | 15/09/2026 | Escolhe o encerrado de setembro. | Exclui o encerrado e inclui os dois ativos, ainda futuros nesse dia. |
   | 10/10/2026 | Escolhe o ativo iniciado em outubro. | Inclui também o futuro iniciado em novembro. |

   As respostas da autoridade estão afirmadas em [test_e4_vinculo_cobre_a_data.py:54](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/tests/test_e4_vinculo_cobre_a_data.py:54); a seleção diferente decorre de [fase_conflitante.py:71](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/services/fase_conflitante.py:71).

   Isso comprova **diferença de universo**, não contagem indevida: essa fixture usa 5x2, que `_ancora_diz` não responde. Fontes: [test_e4_vinculo_cobre_a_data.py:22](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/tests/test_e4_vinculo_cobre_a_data.py:22), [fase_conflitante.py:43](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/services/fase_conflitante.py:43). Caso concreto de produção com colaborador, datas e dia: **[nao verificado]**.

4. **Chamadores, leitores e efeitos**

   | Caminho | Efeito confirmado |
   |---|---|
   | Command → `medir` | Chama em [lavrar_fase_conflitante.py:31](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/management/commands/lavrar_fase_conflitante.py:31). Sem `--apply`, imprime; com a flag, escreve `MetricaSnapshot` com total, quantidade de vínculos e detalhe ([linha 32](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/management/commands/lavrar_fase_conflitante.py:32)). |
   | Cron → command | Declarado às 07:14, com `--apply`, dependente de `reconciliar_vinculo` ([crons.py:479](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/config/crons.py:479)). Instalação/execução em produção: **[nao verificado]**. |
   | `ultimo_lavrado` → snapshot | Lê o último snapshot; sem medida retorna `total=None` e `sem_medida=True`; com medida acrescenta `medido_em` ([fase_conflitante.py:105](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/services/fase_conflitante.py:105)). |
   | API → `ultimo_lavrado` | Entrega o resultado em `contadores.fase_conflitante`, como JSON ([views_mensageria.py:632](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/api/views_mensageria.py:632), [linha 643](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/api/views_mensageria.py:643)). |
   | Mensageria → API | O cliente recebe o JSON inteiro; `saude_da_operacao` o cacheia, mas processa somente `metricas` e devolve apenas essas medidas. **Esse caminho não repassa o contador ao contexto do chat.** Fontes: [core_client.py:108](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/mensageria/nucleo/core_client.py:108), [ferramentas.py:259](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/mensageria/nucleo/ferramentas.py:259), [linha 294](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/mensageria/nucleo/ferramentas.py:294), [linha 380](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/mensageria/nucleo/ferramentas.py:380). |

   Nesses caminhos, as escritas identificadas são **snapshot e cache**. Emissão de chamado, alteração de estado operacional ou retenção de pouso causada por esse contador: **[nao verificado]**. [nao verificado]

5. **Selos e testes existentes** [nao verificado]

   | Referência | O que afirma |
   |---|---|
   | [test_fase_conflitante.py:60](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/tests/test_fase_conflitante.py:60) | Foto nos pares + âncora 20/07: um vínculo acusado, total **≥25**, esperado zero e vínculo na amostra. |
   | [test_fase_conflitante.py:72](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/tests/test_fase_conflitante.py:72) | Mesma foto, âncora 21/07: total zero. |
   | [test_fase_conflitante.py:80](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/tests/test_fase_conflitante.py:80) | Sem foto: zero. |
   | [test_fase_conflitante.py:84](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/tests/test_fase_conflitante.py:84) | Âncora `None`: zero. |
   | [test_fase_conflitante.py:88](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/tests/test_fase_conflitante.py:88) | Ciclo 6x1: zero. |
   | [test_fase_conflitante.py:93](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/tests/test_fase_conflitante.py:93) | Foto diz folga no dia fotografado e trabalho no seguinte. |
   | [test_semselo5_ativo_nao_responde_por_dia.py:128](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/tests/test_semselo5_ativo_nao_responde_por_dia.py:128) | Classifica `medir` em `HOJE`, como cadastro; a catraca cobra ausência de sítios novos/listas velhas, não equivalência com a autoridade ([linha 365](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/tests/test_semselo5_ativo_nao_responde_por_dia.py:365)). |
   | [test_api_mensageria_saude.py:106](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/api/tests/test_api_mensageria_saude.py:106) | Cobra nominalmente `fase_conflitante`: sem lavra, `sem_medida=True` e `total=None`. |
   | [test_api_mensageria_saude.py:79](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/api/tests/test_api_mensageria_saude.py:79) | GET não altera a quantidade de snapshots. |

   Os seis testes do contador usam vínculo ativo iniciado em 01/01/2026 e janela de setembro; não testam troca, encerramento, vínculo futuro ou geradora. Fontes: [test_fase_conflitante.py:30](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/tests/test_fase_conflitante.py:30), [linha 49](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/tests/test_fase_conflitante.py:49), [linha 60](/home/ronald/codex_ws/e0bdbc8e902ce92f39ac7652c5e7f2b6548fb95e/app/escala/tests/test_fase_conflitante.py:60). Resultado de execução dos testes nesta auditoria: **[nao verificado]**.

## 01/11 00:47 · sessao 709103adf3b6 · - · - · origem=Ronald (tmux)

A autoridade registrada é `escala/alimentacao.py::vinculo_do_dia`, definida em `app/escala/alimentacao.py:196`, registrada em `app/core/juizes.py:1262-1271` e documentada em `app/docs/JUIZES.md:84-89`.

- Com geradora: lê `CelulaDia.escala_geradora_id`, procura esse vínculo na lista recebida e devolve `None` se faltar ou se o dia estiver fora da vigência; limites inclusivos. Nesse ramo não exige `ativa=True`, nem exige `data_fim` para vínculo inativo — `app/escala/alimentacao.py:237-252`.
- Sem geradora: aceita início não nulo e `início <= dia <= fim`, sendo fim nulo ilimitado, e exige `ativa or data_fim is not None`; desempata por `(ativa, data_inicio)` — `app/escala/alimentacao.py:164`, `app/escala/alimentacao.py:182-184`, `app/escala/alimentacao.py:253-254`.
- Vínculo futuro não responde antes do início; encerrado responde dentro da vigência; inativo sem fim não responde no fallback — `app/escala/alimentacao.py:182-184`, `app/escala/alimentacao.py:250-254`.

Em `fase_conflitante`, há um sítio de seleção do universo: query direta `EscalaColaborador.objects.filter(ativa=True)`; cada vínculo retornado é aplicado a todos os dias da janela, sem verificar início, fim, célula ou juiz — `app/escala/services/fase_conflitante.py:71-80`. `_ancora_diz` lê âncora/ciclo; `_foto_diz` lê datas de folga; nenhum seleciona vínculo — `app/escala/services/fase_conflitante.py:40-58`.

Casos já presentes na cópia, como **fixtures**, não produção:

| Fixture | Datas/dia | Diferença de seleção |
|---|---|---|
| `E4 C` | Encerrada inativa 01/09..30/09; ativa desde 01/10; futura ativa desde 01/11; dia 15/09 | Juiz escolhe encerrada; filtro de fase inclui as duas ativas e exclui encerrada — `app/escala/tests/test_e4_vinculo_cobre_a_data.py:24-29`, `app/escala/tests/test_e4_vinculo_cobre_a_data.py:54`, `app/escala/services/fase_conflitante.py:71`. |
| `E4 C` | Mesmos vínculos; dia 10/10 | Juiz escolhe ativa desde 01/10; filtro inclui também futura desde 01/11 — `app/escala/tests/test_e4_vinculo_cobre_a_data.py:14`, `app/escala/tests/test_e4_vinculo_cobre_a_data.py:26-29`, `app/escala/tests/test_e4_vinculo_cobre_a_data.py:55`, `app/escala/services/fase_conflitante.py:71`. |
| `Colab EV` | Dia 30/09; velho inativo desde 31/08 até 05/10; novo ativo desde 30/09; célula nomeia velho | Juiz devolve velho; filtro de fase exclui velho e inclui novo — `app/escala/tests/test_escala_vigente_pergunta_a_celula.py:45-55`, `app/escala/tests/test_escala_vigente_pergunta_a_celula.py:88-96`, `app/escala/services/fase_conflitante.py:71`. |

Essas fixtures usam ciclos 5x2 e 6x1, respectivamente; provam diferença de universo pela leitura do código, não um total indevido do contador — `app/escala/tests/test_e4_vinculo_cobre_a_data.py:22-23`, `app/escala/tests/test_escala_vigente_pergunta_a_celula.py:40-42`, `app/escala/services/fase_conflitante.py:43-48`.

Caso de produção com colaborador, datas de vínculo e dia: **[nao verificado]**; o anexo recebido contém somente `JUIZES.md`, sem registros de colaboradores **[nao verificado]**.

## 01/11 00:46 · sessao 2e1e842e5343 · - · - · origem=Ronald (tmux)

**Chamadores e leitores**

- `medir` é importada e chamada pelo command em `app/escala/management/commands/lavrar_fase_conflitante.py:14` e `:31`. Sem `--apply`, só imprime e retorna (`:32-34`); com `--apply`, grava/atualiza exclusivamente o snapshot mostrado no trecho, usando `total` como numerador, `vinculos` como denominador e o dict como detalhe (`:35-38`); depois imprime (`:39-41`).
- O cron está **declarado** para 07:14, `--apply`, dependente de `reconciliar_vinculo`: `app/config/crons.py:479-480`. Instalação/execução em produção: **[nao verificado]**.
- `ultimo_lavrado` busca o último `MetricaSnapshot` de `chave=CHAVE`, `escopo='frota'`, por `-data_ref,-calculado_em`; sem snapshot, retorna `total=None, esperado=0, sem_medida=True`; com snapshot, copia detalhe e acrescenta `medido_em`: `app/escala/services/fase_conflitante.py:105-114`.
- A API importa essa leitora (`app/api/views_mensageria.py:632`), a põe em `saida['contadores']['fase_conflitante']` (`:643`) e devolve JSON (`:647`); rota em `app/api/urls.py:16`.
- `mensageria/nucleo/core_client.py:108-120` faz GET de `/api/mensageria/saude/` e devolve o JSON inteiro. Entretanto `saude_da_operacao` cacheia esse retorno (`mensageria/nucleo/ferramentas.py:259-262`), percorre **somente `dados['metricas']`** (`:265`) e retorna só `{'saude_da_operacao': medidas}` (`:294`). Portanto, nesse caminho, `contadores.fase_conflitante` **não chega ao contexto do chat**, que recebe `saude_da_operacao` em `:380`.
- Nos caminhos acima, a escrita ligada ao contador é snapshot (`app/escala/management/commands/lavrar_fase_conflitante.py:35-38`) e cache do payload (`mensageria/nucleo/ferramentas.py:262`); a API só devolve o contador (`app/api/views_mensageria.py:643-647`). Chamado emitido, mudança de estado operacional ou pouso retido **por esse contador**: **[nao verificado]**.

**Testes/selos** [nao verificado]

| Teste | O que efetivamente afirma | [nao verificado]
|---|---|
| `app/escala/tests/test_fase_conflitante.py:60` | Foto pares + âncora 20/07 gera 1 vínculo acusado, total ≥25, `esperado=0` e amostra com o vínculo (`:64-70`). |
| `app/escala/tests/test_fase_conflitante.py:72` | Mesma foto, âncora 21/07: total zero (`:74-78`). |
| `app/escala/tests/test_fase_conflitante.py:80` | Sem foto: total zero (`:81-82`). |
| `app/escala/tests/test_fase_conflitante.py:84` | Âncora `None`: total zero (`:85-86`). |
| `app/escala/tests/test_fase_conflitante.py:88` | Ciclo 6x1: total zero (`:90-91`). |
| `app/escala/tests/test_fase_conflitante.py:93` | `_foto_diz` devolve `False` no dia fotografado e `True` no seguinte (`:95-97`). |
| `app/escala/tests/test_semselo5_ativo_nao_responde_por_dia.py:128` | Declara `medir` em **HOJE**, motivo “cadastro: vinculos ativos em fase conflitante” (`:128-129`); o teste cobra ausência de sítios novos/listas velhas (`:365-371`) e motivos/listas disjuntas (`:373-377`). |
| `app/api/tests/test_api_mensageria_saude.py:106` | Inclui nominalmente `fase_conflitante`: sem lavra, `sem_medida=True` e `total=None` (`:108-112`). |
| `app/api/tests/test_api_mensageria_saude.py:79` | GET da API não altera a quantidade de snapshots (`:80-83`). |

Os seis testes funcionais usam fixture com `ativa=True`, início fixo em 01/01/2026 (`app/escala/tests/test_fase_conflitante.py:49-51`) e janela de setembro (`:30-31`); os corpos acima não verificam troca de vínculo, vínculo encerrado, futuro ou geradora de célula (`:60-97`). O teste da API que injeta snapshot e confere conteúdo é de **`previsto_cego`**, não de `fase_conflitante` (`app/api/tests/test_api_mensageria_saude.py:114-124`). Resultado de execução destes testes: **[nao verificado]**.

## 01/11 00:25 · sessao ab9069c1e335 · V3-P5-PRORROGACAO-POR-INTERVALO · verificador · origem=Code

**O comentário foi corrigido; o delta desta volta não muda valores calculados.** As duas alterações são “HE100” → “HE50”, coerentes com o teste central. (app/ponto/motor_calculo_v2.py:145; app/ponto/motor_calculo_v2.py:146; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:70)

**1. Situação dos achados da b1**

| Achado | Estado em 5394309c |
|---|---|
| **P1 — excesso microscópico de `float` sobre a união** | **Superado pelo descarte da triagem; sem cura numérica.** A acumulação continua em `float`, e as propriedades continuam usando tolerância `1e-9`. (app/ponto/motor_calculo_v2.py:193; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:203; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:205) |
| **P2 — overflow no extremo de datetime** | **Superado pelo descarte da triagem; possibilidade técnica permanece.** O retorno consulta `minutos_noturnos` mesmo sem pausas; essa função continua avançando datas sem guarda de overflow. (app/ponto/motor_calculo_v2.py:196; app/ponto/motor_calculo_v2.py:104; app/ponto/motor_calculo_v2.py:117) |
| **P3 — `feriados=set()` permite fallback ORM** | **Superado como impeditivo pela triagem; mecanismo permanece.** Data ausente chama `is_feriado`, que consulta o juiz e converte exceções em `False`. (app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:66; app/ponto/motor_calculo_v2.py:953; app/ponto/motor_calculo_v2.py:955; app/ponto/motor_calculo_v2.py:39; app/ponto/motor_calculo_v2.py:43) |
| **P4 — comentário atribuindo o caso à HE100** | **Resolvido.** Agora diz HE50, como as expectativas HE50=2h, HE100=0 e HE50 noturna=2h. (app/ponto/motor_calculo_v2.py:145; app/ponto/motor_calculo_v2.py:146; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:70; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:72) |

As demais conclusões e ressalvas da b1 permanecem:

- **Caso central corrigido anteriormente, preservado:** desconto esperado de 60 minutos, HE50 noturna de 2h e noturnas de 8h. Isso está nas asserções; execução nesta árvore é **[não verificado]**. (app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:112; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:72; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:82)
- **Laço parado, resultado negativo ou acima do todo seguem afastados pela estrutura**, ressalvadas exceções: o cursor avança, somente durações positivas entram na soma e o retorno aplica o teto do todo. (app/ponto/motor_calculo_v2.py:179; app/ponto/motor_calculo_v2.py:188; app/ponto/motor_calculo_v2.py:193; app/ponto/motor_calculo_v2.py:196)
- **Normalização e limites temporais permanecem:** aware converte de fuso; naive usa `localize` sem política explícita para ambiguidade/inexistência; o cursor relê o instante normalizado. Há casos específicos de mudança de offset, verão e segundos. (app/ponto/motor_calculo_v2.py:73; app/ponto/motor_calculo_v2.py:186; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:415; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:420; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:480)
- **Import, helper, assinatura e alcance do patch seguem sem defeito identificado:** `_aw` é local, Django é declarado, `he_pontas` é desempacotado em quatro campos e o wrapper resolve a função substituída pelos mutantes. (app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:44; app/requirements.txt:13; app/ponto/calculador/regras.py:818; app/ponto/motor_calculo_v2.py:1915)
- **Limites das expectativas e do oráculo permanecem:** a grade reutiliza `localizar`, trabalha com minutos inteiros e reconhece políticas da casa; os casos históricos e com segundos usam interseções próprias. (app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:157; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:160; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:167; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:410; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:473)
- **Alcance produtivo continua delimitado:** a alimentação diária passa `intervalos=()`, enquanto o adaptador por par fornece as pausas. (app/ponto/calculador/alimentacao.py:514; app/ponto/calculador/periodo.py:507)
- **Nova suíte após a correção, suíte completa, impacto produtivo, ordem de travas e pouso:** **[não verificado]**. O GREEN anterior mencionado na triagem não demonstra execução após este delta. **[não verificado]** [nao verificado]

**2. Linhas alteradas fora do corpo**

**Nenhuma no `diff2.txt` fornecido.** As únicas linhas alteradas são os comentários **145 e 146**, dentro da função delimitada por **121–196**. A comparação sintática em memória, revertendo apenas essas duas substituições, resultou em AST idêntica. (diff2.txt:5; app/ponto/motor_calculo_v2.py:121; app/ponto/motor_calculo_v2.py:145; app/ponto/motor_calculo_v2.py:146; app/ponto/motor_calculo_v2.py:196)

A completude histórica do anexo perante os commits não pôde ser conferida: a cópia não contém `.git`. **[não verificado]** [nao verificado]

**3. Escritores, leitores e mudança de valores**

O corpo devolve **um único escalar: desconto noturno em minutos**, produzido pela acumulação em `total` e limitado pelo noturno do recorte. Não devolve diretamente HE50, HE100 ou minutos legais. (app/ponto/motor_calculo_v2.py:176; app/ponto/motor_calculo_v2.py:193; app/ponto/motor_calculo_v2.py:196)

| Caminho | Quem lê e quem escreve o resultado derivado |
|---|---|
| **HE noturna** | `he_noturna` subtrai o escalar e reparte o saldo entre HE50/HE100. `_aplicar_he_noturna` grava os campos do período; `regras.do_dia` grava as rubricas em horas. (app/ponto/motor_calculo_v2.py:442; app/ponto/motor_calculo_v2.py:448; app/ponto/motor_calculo_v2.py:450; app/ponto/motor_calculo_v2.py:1008; app/ponto/motor_calculo_v2.py:1010; app/ponto/calculador/regras.py:828; app/ponto/calculador/regras.py:830) |
| **Noturnas pelo motor** | `_noturno_dos_intervalos` encaminha o escalar; `calcular_noturnas` subtrai e aplica o fator; `_escrever_noturnas` grava relógio/legais no período. (app/ponto/motor_calculo_v2.py:1915; app/ponto/motor_calculo_v2.py:973; app/ponto/motor_calculo_v2.py:974; app/ponto/motor_calculo_v2.py:992; app/ponto/motor_calculo_v2.py:993) |
| **Noturnas pelo calculador** | `_ContextoNoturno` empresta o mesmo wrapper; `noturnas_do_dia` subtrai o desconto. `do_dia` escreve horas/detalhes; o adaptador por par escreve relógio/legais no período. (app/ponto/calculador/regras.py:63; app/ponto/calculador/regras.py:86; app/ponto/calculador/regras.py:840; app/ponto/calculador/regras.py:841; app/ponto/calculador/periodo.py:507; app/ponto/calculador/periodo.py:752; app/ponto/calculador/periodo.py:753) |
| **Persistência e apresentação posteriores** | `lavrar` agrega e persiste `DiaPago`; `ResultadoMes` agrega para o fechamento mensal. Apresentação diária e exportação leem esses resultados. (app/ponto/services/dia_pago.py:179; app/ponto/services/dia_pago.py:322; app/ponto/services/dia_pago.py:358; app/ponto/motor_calculo_v2.py:321; app/ponto/motor_calculo_v2.py:340; app/ponto/motor_calculo_v2.py:344; app/ponto/services/fechamento.py:1128; app/ponto/services/dia_pago.py:922; app/folha/export.py:436) |

Os outros cinco locais de chamada direta encontrados estão no teste P5: `_f`, caso com segundos, `_celula` e os dois `_caso`. (app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:108; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:142; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:197; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:409; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:472)

**Nenhum desses leitores muda de valor pelo delta2**, que altera somente comentários. As mudanças numéricas descritas na b1 pertencem à cura anterior, preservada nesta cópia. (diff2.txt:9; diff2.txt:11; app/ponto/motor_calculo_v2.py:145; app/ponto/motor_calculo_v2.py:196)

**4. Classes que já reprovaram obra**

**Não é possível certificar todo o diff `e0bdbc8e..5394309c`: esse diff não foi fornecido e não há histórico Git na cópia. [não verificado]** No corpo P5, consumidores e testes examinados:

| Classe | Resultado delimitado ao que foi lido |
|---|---|
| **Segundo juiz** | **Não encontrado.** O corpo consulta a mesma `minutos_noturnos`; a autoridade do valor do par permanece registrada. (app/ponto/motor_calculo_v2.py:191; app/ponto/motor_calculo_v2.py:196; app/core/juizes.py:1160) |
| **Segundo escritor introduzido pela cura** | **Não encontrado.** O corpo retorna o escalar; os campos derivados passam pelos escritores existentes. (app/ponto/motor_calculo_v2.py:196; app/ponto/motor_calculo_v2.py:992; app/ponto/motor_calculo_v2.py:1008; app/ponto/calculador/periodo.py:752) |
| **Relógio fora da borda** | **Não encontrado.** O cursor deriva das entradas e os testes fixam datas, sem leitura de “agora” nesses trechos. (app/ponto/motor_calculo_v2.py:162; app/ponto/motor_calculo_v2.py:186; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:52; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:187) |
| **Derivação paralela de fonte única** | **Não encontrada no corpo produtivo.** A presença noturna é consultada em `minutos_noturnos`; a regra independente está no teste como oráculo. (app/ponto/motor_calculo_v2.py:191; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:151) |
| **RED sem mordida** | **Não encontrado por inspeção.** O patch alcança o símbolo chamado, e há testemunhas para os cinco mutantes. Execução nesta árvore: **[não verificado]**. (app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:347; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:356; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:359; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:365; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:454; app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:531) |

## 01/11 00:24 · sessao 19d5e5096e49 · - · - · origem=Ronald (tmux)

Classificação dos achados da b1, considerando a triagem fornecida:

| Achado | Estado nesta volta | Conferência atual |
|---|---|---|
| P1: arredondamento acima da união | **Superado pelo descarte; sem alteração numérica.** A soma continua em `float`; a propriedade permite `1e-9`. | `app/ponto/motor_calculo_v2.py:193`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:201`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:203`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:205` |
| P2: overflow no limite de datetime | **Superado pelo descarte; possibilidade técnica permanece.** O retorno chama `minutos_noturnos` mesmo sem pausas; essa função incrementa dia sem guarda. | `app/ponto/motor_calculo_v2.py:196`; `app/ponto/motor_calculo_v2.py:104`; `app/ponto/motor_calculo_v2.py:117` |
| P3: `feriados=set()` não fecha a fixture | **Superado pelo descarte; fallback permanece.** Data ausente consulta `is_feriado`; esta chama o juiz e converte exceção em `False`. A triagem relata GREEN anterior, mas não fornece prova de nova execução nesta árvore. | `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:66`; `app/ponto/motor_calculo_v2.py:953`; `app/ponto/motor_calculo_v2.py:955`; `app/ponto/motor_calculo_v2.py:39`; `app/ponto/motor_calculo_v2.py:43`; nova execução: **[não verificado]** |
| P4: comentário HE100 em caso HE50 | **Resolvido.** As duas linhas agora dizem HE50, coerente com HE50=2h e HE100=0 do teste. | `app/ponto/motor_calculo_v2.py:145`; `app/ponto/motor_calculo_v2.py:146`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:70`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:71`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:72` |

Demais ressalvas da b1:

- **Alcance da alimentação produtiva segue delimitado:** o caminho diário entrega `intervalos=()`; o adaptador por par entrega as pausas. O delta apenas de comentário não altera isso. (`app/ponto/calculador/alimentacao.py:512`; `app/ponto/calculador/alimentacao.py:514`; `app/ponto/calculador/periodo.py:507`)
- **Origem das expectativas segue explícita:** o teste declara a regra e reconhece o gatilho por toque em 00–05 e o teto de 26h como escolhas da casa; a aproximação da HE está documentada na própria função. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:14`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:157`; `app/ponto/motor_calculo_v2.py:413`)
- **Limites do oráculo permanecem:** reutiliza `localizar`, opera minuto inteiro e declara grade sem verão em 2026; segundos e história têm expectativas próprias de interseção. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:160`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:164`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:167`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:410`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:473`)
- **Import/helper/assinatura/alias permanecem sem defeito identificado:** `_aw` é local, Django é declarado, `do_dia` aceita os argumentos e desempacota quatro campos; os imports dos helpers ocorrem durante a chamada e o contexto empresta o método do motor. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:44`; `app/requirements.txt:13`; `app/ponto/calculador/regras.py:572`; `app/ponto/calculador/regras.py:818`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:105`; `app/ponto/calculador/regras.py:63`; `app/ponto/motor_calculo_v2.py:1915`)
- **Laço parado segue afastado pela estrutura:** `_f > a`, o próximo candidato avança ou recebe avanço subsidiário, e `a=b`; exceções continuam fora dessa garantia. (`app/ponto/motor_calculo_v2.py:179`; `app/ponto/motor_calculo_v2.py:188`; `app/ponto/motor_calculo_v2.py:190`; `app/ponto/motor_calculo_v2.py:194`)
- **RED sem mordida não encontrado no teste lido:** patch alcança a função e exige falha do caso; há testemunhas para os cinco mutantes. Isso é evidência estrutural, sem alegar execução nesta árvore. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:347`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:348`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:356`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:359`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:365`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:454`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:531`)
- **Relógio externo não encontrado no corpo nem teste lidos:** o corpo deriva cortes das entradas e o teste usa datas fixadas. (`app/ponto/motor_calculo_v2.py:162`; `app/ponto/motor_calculo_v2.py:186`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:52`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:187`)
- **Suíte inteira da árvore 5394309c, produção, ordem de travas e pouso seguem [não verificado].** A triagem apenas relata GREEN de 34 testes da situação anterior; não há execução feita por este explorer. **[não verificado]**

## 01/11 00:23 · sessao c5483f4142a7 · - · - · origem=Ronald (tmux)

Não encontrei essas classes no corpo atual e no teste P5 lidos; **o diff total `e0bdbc8e..5394309c` não pôde ser conferido**, pois a cópia não contém `.git` nem foi fornecido esse diff completo. [não verificado]

| Classe | Resultado e evidência |
|---|---|
| Segundo juiz | **Não encontrado no corpo lido**: a janela continua sendo consultada em `minutos_noturnos`; o corpo não repete os limites 22/05 ou o gatilho da prorrogação. (`app/ponto/motor_calculo_v2.py:191`; `app/ponto/motor_calculo_v2.py:196`) A autoridade do valor do par permanece registrada em `app/core/juizes.py:1160`. |
| Segundo escritor | **Não encontrado no corpo lido**: produz um escalar por `return`; os consumidores usam as atribuições existentes dos campos noturnos e HE noturna. (`app/ponto/motor_calculo_v2.py:196`; `app/ponto/motor_calculo_v2.py:992`; `app/ponto/motor_calculo_v2.py:993`; `app/ponto/motor_calculo_v2.py:1008`; `app/ponto/motor_calculo_v2.py:1010`) |
| Relógio fora da borda | **Não encontrado no corpo/teste lidos**: o cursor deriva do instante recebido, normalizado no fuso; os testes fixam datas. Não há consulta a “agora” nesses trechos. (`app/ponto/motor_calculo_v2.py:162`; `app/ponto/motor_calculo_v2.py:186`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:52`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:187`) |
| Derivação paralela de fonte única | **Não encontrada no corpo produtivo lido**: mede presença pela mesma `minutos_noturnos`, unindo intervalos antes da soma. (`app/ponto/motor_calculo_v2.py:164`; `app/ponto/motor_calculo_v2.py:169`; `app/ponto/motor_calculo_v2.py:191`) A regra independente está no teste, como oráculo, e não em outro leitor produtivo. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:151`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:177`) |
| RED sem mordida | **Não encontrado por inspeção dos caminhos**: os testes substituem o símbolo realmente chamado e exigem falha dos casos ou divergência da grade para cinco mutantes. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:347`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:356`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:359`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:365`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:454`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:531`) Execução nesta árvore: [não verificado]. |

O delta fornecido nesta volta troca somente “HE 100” por “HE 50” em duas linhas de comentário, agora coerentes com o teste central. (`app/ponto/motor_calculo_v2.py:145`; `app/ponto/motor_calculo_v2.py:146`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:70`; `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:72`)

## 01/11 00:23 · sessao 5e6f299b7c4a · - · - · origem=Ronald (tmux)

O corpo devolve **um único valor**, o desconto noturno em minutos (`float`), produzido por `total = 0.0`, acumulação e teto final; não devolve HE50/HE100 nem minutos legais. (`app/ponto/motor_calculo_v2.py:121`, `:176`, `:193`, `:196`)

| Leitor direto produtivo | Uso do escalar e leitores seguintes |
|---|---|
| `he_noturna` | Subtrai o desconto do noturno da cauda extra; reparte o saldo entre HE50/HE100 e devolve o par. (`app/ponto/motor_calculo_v2.py:442`, `:448`, `:450`, `:451`) |
| `MotorBase._noturno_dos_intervalos` | Encaminha o escalar com a prorrogação cadastrada. (`app/ponto/motor_calculo_v2.py:1915`, `:1916`) |

O wrapper tem dois consumidores: `calcular_noturnas` subtrai o desconto, aplica o fator e retorna relógio/legais; `_escrever_noturnas` grava esses resultados no período. (`app/ponto/motor_calculo_v2.py:973`, `:974`, `:975`, `:991`, `:992`, `:993`)

O outro consumidor usa o mesmo método emprestado por `_ContextoNoturno`; `noturnas_do_dia` subtrai o desconto e retorna relógio/legais. `do_dia` converte legais para horas e registra detalhes; `noturno_no_par` devolve `NoturnoV3`, lido pelo adaptador que preenche os dois campos do período. (`app/ponto/calculador/regras.py:63`, `:86`, `:89`, `:836`, `:840`, `:841`, `:842`; `app/ponto/calculador/periodo.py:507`, `:510`, `:751`, `:752`, `:753`)

O split de HE é lido por `_aplicar_he_noturna`, que preenche os campos do período, e por `do_dia`, que soma os splits e escreve as rubricas diárias em horas. (`app/ponto/motor_calculo_v2.py:1002`, `:1008`, `:1010`; `app/ponto/calculador/regras.py:823`, `:825`, `:826`, `:828`, `:830`)

Na busca textual em `app/**/*.py`, os outros cinco locais de chamada direta são todos no teste P5: helper `_f`, caso com segundos, `_celula` e os dois helpers `_caso`; seus resultados alimentam asserções de valor/propriedades. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:108`, `:142`, `:143`, `:197`, `:199`, `:201`, `:203`, `:205`, `:409`, `:411`, `:472`, `:475`)

Os escritores/leitores posteriores relevantes encontrados são:

- `lavrar` soma os campos dos períodos em horas; pode receber rubricas do calculador conforme a proveniência, constrói `DiaPago` e persiste por `bulk_create`. (`app/ponto/services/dia_pago.py:179`, `:181`, `:184`, `:224`, `:322`, `:327`, `:328`, `:343`, `:358`)
- `ResultadoMes` soma minutos legais e HE noturna em horas; fechamento arredonda e grava os três campos mensais. (`app/ponto/motor_calculo_v2.py:321`, `:340`, `:344`; `app/ponto/services/fechamento.py:893`, `:896`, `:899`, `:1128`, `:1131`, `:1134`, `:1137`)
- Apresentação diária lê `DiaPago.horas_noturnas`; exportação lê `FechamentoMensal.horas_noturnas` como adicional noturno. (`app/ponto/services/dia_pago.py:922`; `app/folha/export.py:436`)

**Nenhum leitor muda de valor pelo delta2 fornecido:** a alteração é somente “HE 100” → “HE 50” nos comentários; o retorno executável continua em `:196`. (`app/ponto/motor_calculo_v2.py:145`, `:146`, `:196`; completude histórica do anexo: [não verificado])

As mudanças `0→60`, HE50 noturna `3→2h` e noturnas `9→8h` pertencem à volta1; o mutante preserva o cálculo anterior por peça, e o teste declara o caso e as expectativas atuais. (`app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:57`, `:60`, `:62`, `:72`, `:82`, `:86`, `:94`, `:112`)

O alcance produtivo permanece delimitado: a alimentação diária passa `intervalos=()`, enquanto o adaptador por par fornece intervalos julgados; não se deve atribuir à alimentação diária o mesmo desconto não vazio do RED. (`app/ponto/calculador/alimentacao.py:512`, `:513`, `:514`; `app/ponto/calculador/periodo.py:501`, `:507`)

## 01/11 00:16 · sessao 37fb4120dc78 · CONTENCAO-QUE-MORDE · leitura · origem=Code

A v2 nomeia vários pontos de encaixe corretamente, mas eles ainda executam o contrato anterior: B3 conta reprovações, B1 depende de tokens, e o Stop não cobra gerações de contadores. Os gates de modelo, suite antes do opus, seis Agents vivos e contrato no push não aparecem nas respectivas portas. (`bin/disjuntor.py:449`, `bin/disjuntor.py:912`, `bin/disjuntor.py:1451`, `bin/hook_stop_fila1.py:1184`, `bin/pre-push.sh:200`)

A identificação desta cópia com o HASH informado não pôde ser confirmada por Git: ela não contém `.git`. **[não verificado]** Nos casos abaixo, separo contraexemplo dedutível do código de ocorrência em produção; a sonda fornecida não contém decisões dos hooks suficientes para demonstrar doze ocorrências reais. **[não verificado]**

Para evitar repetir proprietários em cada item, este é o mapa compartilhado de escrita e leitura:

| Estado/campos | Quem escreve | Quem lê |
|---|---|---|
| `disjuntor_estado.json` | Escrita física por `_com_estado`, sob flock e substituição de temporário. Ausente começa como `{}`; ilegível levanta. (`bin/disjuntor.py:237`, `bin/disjuntor.py:262`, `bin/disjuntor.py:274`) | `_com_estado` e `ler_estado`; decisões B1/B3/B4 e painel. (`bin/disjuntor.py:260`, `bin/disjuntor.py:291`, `bin/disjuntor.py:1626`) |
| `b3[obra]` | `decide_b3`: `reprovados`, `codex`, `disparado`, fotografia/instante do disparo, `liberado`, `consumidas`; rearme altera `rearmado` e limpa listas correntes. Não existe `voltas` nesses fluxos. (`bin/disjuntor.py:713`, `bin/disjuntor.py:891`, `bin/disjuntor.py:936`, `bin/disjuntor.py:1018`) | A mesma decisão, chamada pelas portas B3; painel lê liberações pela trilha. (`bin/disjuntor.py:980`, `bin/disjuntor.py:1042`, `bin/disjuntor.py:1574`) |
| `b3_sem_obra` | `decide_b3` acrescenta IDs dos verificadores finais sem obra encontrados posteriormente nos transcripts. (`bin/disjuntor.py:892`) | Painel conta o comprimento acumulado, sem janela temporal. (`bin/disjuntor.py:1624`) |
| `b1[id].{checado,disparado,motivo}` | `b1_do_agente`, via `_com_estado`. (`bin/disjuntor.py:481`, `bin/disjuntor.py:486`) | Próximo PreToolUse do agente e Stop, neste último apenas para disparos já gravados. (`bin/disjuntor.py:476`, `bin/hook_stop_fila1.py:1054`) |
| `b4.negou_desde`, `b4_para.desde` | PreToolUse/B4 Stop, via `_com_estado`. (`bin/disjuntor.py:1465`, `bin/disjuntor.py:1215`) | Transições B4; não há sequência persistida de Stops com cota desconhecida. (`bin/disjuntor.py:1206`, `bin/disjuntor.py:1461`) |
| `estado_contadores.json` | **Nenhum escritor atual no gerador contratado**: ele termina gravando o Markdown em `dst`. (`bin/gerar_estado.py:376`) | **Nenhum leitor atual no Stop contratado**: o fluxo não consulta esse sidecar. (`bin/hook_stop_fila1.py:1184`, `bin/hook_stop_fila1.py:1300`) |
| `triagem.log`: hora, obra, volta, veredito | CLI `--triagem` → `_triagem_sob_trava`, sob a trava do estado; atualiza também `b3_triagem_linhas`. (`bin/disjuntor.py:804`, `bin/disjuntor.py:812`, `bin/disjuntor.py:815`) | `_triagens`, consumido por decisão, triagem e rearme; `_procedentes` seleciona as voltas procedentes. (`bin/disjuntor.py:823`, `bin/disjuntor.py:857`, `bin/disjuntor.py:1007`) |
| `<resposta>.id`: obra, papel, hash, pid, fim, sha | Wrapper remove o anterior e grava outro somente com rc 0 e resposta desta chamada. Reutilizar o caminho substitui o histórico anterior. (`bin/codex_pergunta.sh:171`, `bin/codex_pergunta.sh:198`) | `_le_id` durante triagem; valida campos e SHA da resposta. Não há hoje semeadura geral desses arquivos. (`bin/disjuntor.py:753`, `bin/disjuntor.py:795`, `bin/disjuntor.py:800`) |
| `cota.json` | Statusline grava percentual/reset, hora, contexto e session_id; sem os dois números válidos não grava. (`bin/statusline_cota.py:45`, `bin/statusline_cota.py:51`) | `ler_cota` decide por percentual/reset; B4 e painel o chamam. Hora/session_id não participam da validade. (`bin/disjuntor.py:1069`, `bin/disjuntor.py:1111`, `bin/disjuntor.py:1619`) |
| Transcripts/meta de subagent | O leitor atribui a escrita ao harness; implementação desse escritor fora da cópia. (`bin/disjuntor.py:336`, `bin/disjuntor.py:369`) **[não verificado]** | B1 lê meta, tokens, cópias citadas e mtimes; B3 lê tipo, prompt e Handback; painel lê cache e modelo. (`bin/disjuntor.py:415`, `bin/disjuntor.py:449`, `bin/disjuntor.py:546`, `bin/disjuntor.py:742`, `bin/disjuntor.py:1512`, `bin/disjuntor.py:1631`) |

Nas fichas, **C/E/F/M/D** significam lacunas em quem chama / quem espera / falha-limite / medida / quem desliga.

1. **Teto de seis voltas por obra**

   **(a) Sítio:** `decide_b3` é o ponto comum correto, mas hoje mede reprovações finais e PROCEDENTES Codex, não toda abertura de Agent ou chamada de verificador. Construtor não consulta B3. O Stop continua sendo chamador da decisão, embora a v2 o enumere apenas nos itens 6/12. (`bin/disjuntor.py:878`, `bin/disjuntor.py:912`, `bin/disjuntor.py:1454`, `bin/hook_stop_fila1.py:1048`)

   **(b) Estado:** usa B3, transcripts e triagem do mapa; `b3[obra].voltas` e sua semeadura ainda não têm escritor/leitor implementados. O `.id` existente só nasce ao término bem-sucedido, depois da autorização. (`bin/disjuntor.py:891`, `bin/disjuntor.py:936`, `bin/codex_pergunta.sh:198`)

   **(c) Contraexemplo estático:** sete construtores da mesma obra podem passar quando B4 permite: a porta não incrementa nem consulta teto de voltas. Sete chamadas Codex sem PROCEDENTES suficientes também não atingem o B3 atual. Ocorrência real, inclusive a contagem 9 de GRADE-FOLGA-JUIZ: **[não verificado]**. (`bin/disjuntor.py:1451`, `bin/disjuntor.py:1460`, `bin/disjuntor.py:912`)

   **(d) Lacunas:** **C:** consulta da decisão versus registro de abertura; **E:** se N reabre para total absoluto ou concede N adicionais; **F:** identidade/idempotência, soma das fontes, chamadas falhadas e reserva de voo; **M:** a semente não pode presumir `.id` imutável; **D:** as CLIs reabre/redesenha não existem no despacho atual. Locks por resposta e sidecar posterior não reservam uma volta entre respostas distintas. (`bin/codex_pergunta.sh:162`, `bin/codex_pergunta.sh:171`, `bin/codex_pergunta.sh:198`, `bin/disjuntor.py:1699`)

2. **Segundo ERRO-BOBO vira DESENHO**

   **(a) Sítio:** `linhas_libera_b3` reconhece a linha; `decide_b3` a consome. O contrato acerta os sítios, mas o reconhecedor não consulta histórico por tipo. (`bin/disjuntor.py:626`, `bin/disjuntor.py:650`, `bin/disjuntor.py:930`)

   **(b) Estado:** `consumidas` guarda SHA da linha; `liberado` recebe reprovações/triagens anteriores. O tipo aparece no anúncio da trilha, sem acompanhar cada entrada de `consumidas`. (`bin/disjuntor.py:936`, `bin/disjuntor.py:939`, `bin/disjuntor.py:940`)

   **(c) Contraexemplo estático:** ERRO-BOBO A libera; nova reprovação dispara; ERRO-BOBO B, diferente, posterior e fora da fotografia, libera novamente. Não há teste de “segundo por obra” nessa transição. Ocorrência real: **[não verificado]**. (`bin/disjuntor.py:930`, `bin/disjuntor.py:932`, `bin/disjuntor.py:938`)

   **(d) Lacunas:** **C:** o reconhecedor sozinho não possui o histórico; **E:** esclarecer DESENHO automático versus liberação exclusiva de Ronald; **F:** migração dos SHAs antigos sem tipo; **M:** painel de liberações em 24 h não equivale ao histórico vitalício da obra; **D:** hoje uma linha válida do Code libera, enquanto a guarda reserva apenas `--rearma-b3`. (`bin/disjuntor.py:626`, `bin/disjuntor.py:939`, `bin/disjuntor.py:1552`, `bin/disjuntor.py:1568`, `bin/disjuntor.py:162`)

3. **Opus final com PROCEDENTE sem commit de cura** [nao verificado]

   **(a) Sítio:** `_procedentes` identifica os PROCEDENTES corretamente, mas não julga cura, cópia ou HEAD. A porta final só pergunta ao B3 atual. (`bin/disjuntor.py:857`, `bin/disjuntor.py:1457`)

   **(b) Estado:** `.id` contém hash completo; a triagem transforma a identidade em `hash12@pid@fim@sha12`. Seu log não preserva caminho da cópia nem vínculo achado→cura. (`bin/codex_pergunta.sh:199`, `bin/disjuntor.py:802`, `bin/disjuntor.py:813`)

   **(c) Contraexemplo estático atual:** um ou dois PROCEDENTES, sem cura, ainda permitem outro final: o limiar Codex é três. O teste atual exige passagem com dois. (`bin/disjuntor.py:77`, `bin/disjuntor.py:912`, `bin/tests/test_disjuntor.sh:1153`) **Contraexemplo do critério contratado:** HEAD diferente pode ser commit alheio ao achado; desigualdade de hash não demonstra cura. Ocorrência real: **[não verificado]**.

   **(d) Lacunas:** **C:** qual cópia é autoridade; **E:** quais PROCEDENTES permanecem pendentes; **F:** posterioridade/ancestralidade e diferença entre mudança e cura; **M:** prova com commit diferente que não cura; **D:** alcance de rearme/DESENHO sobre esse impedimento. Hoje o rearme retira todas as triagens anteriores da contagem corrente. (`bin/disjuntor.py:802`, `bin/disjuntor.py:1018`)

4. **Verificador final sem obra reconhecida**

   **(a) Sítio:** `_obra_de` é o reconhecedor correto; a porta atual usa ausência de obra para pular a negativa. O wrapper já exige `CODEX_OBRA`, mas somente não vazio. (`bin/disjuntor.py:504`, `bin/disjuntor.py:1457`, `bin/codex_pergunta.sh:45`)

   **(b) Estado:** `b3_sem_obra` é preenchido depois, lendo transcripts de verificadores finais; o painel publica total acumulado, sem timestamps para 24 h. (`bin/disjuntor.py:736`, `bin/disjuntor.py:892`, `bin/disjuntor.py:1624`)

   **(c) Contraexemplo estático:** “LEITURA FINAL antes do pouso, sem o nome dela” passa se B4 permitir. Esse prompt existe no teste, que verifica apenas a contagem posterior. Ocorrência real: **[não verificado]**. (`bin/disjuntor.py:1457`, `bin/tests/test_disjuntor.sh:427`)

   **(d) Lacunas:** **C:** universo continua `subagent_type=verificador` mais final ou passa a incluir outros tipos/modelos; **E:** reconhecimento do rótulo não prova leitura da obra; **F:** primeiro ID reconhecido pode ser de outra obra; **M:** abertura negada versus contagem posterior e janela 24 h; **D:** aplicação da chave a essa nova negativa. O reconhecedor atual toma a primeira ocorrência válida. (`bin/disjuntor.py:94`, `bin/disjuntor.py:504`, `bin/disjuntor.py:1454`, `bin/disjuntor.py:1487`)

5. **B1 com job próprio em fundo e nenhuma escrita por 15 min**

   **(a) Sítio:** `medir_b1` é correto. “PreToolUse Agent/Bash” descreve estreitamente o chamador: o matcher é geral, e B1 age nas ferramentas do agente com `agent_id`, exceto Handback. Stop apenas consulta disparos persistidos. (`.claude/settings.json:64`, `bin/disjuntor.py:1435`, `bin/hook_stop_fila1.py:1054`)

   **(b) Estado:** B1 lê meta/transcript/cópias citadas/mtimes; escreve os campos B1 do mapa. Não interpreta lançamento e conclusão de Bash em fundo. (`bin/disjuntor.py:415`, `bin/disjuntor.py:425`, `bin/disjuntor.py:428`, `bin/disjuntor.py:481`)

   **(c) Contraexemplo estático:** tokens zero causam não-disparo antes de verificar arquivos, mesmo havendo job em fundo. (`bin/disjuntor.py:452`) A sonda das 17 esperas não prova esse caso: não fornece mtimes nem decisões do hook. B1 exclui `cache_read`, mas os retornos informados também têm tokens das categorias que ele soma; portanto “esperou três minutos” não demonstra ausência de tokens para B1. Ocorrência real de liberação/negação: **[não verificado]**. (`bin/disjuntor.py:449`)

   **(d) Lacunas:** **C:** sem próxima ferramenta não há nova medição; **E:** negar ferramenta não encerra o job; **F:** ownership por caminho citado, isenções por tipo e fontes ilegíveis; **M:** job sem tokens, término do job e arquivos escritos por terceiro; **D:** chave hoje basta existir, sem RELATO obrigatório. Meta/cópia ilegível hoje não dispara B1. (`bin/disjuntor.py:1435`, `bin/disjuntor.py:1438`, `bin/disjuntor.py:418`, `bin/disjuntor.py:425`, `bin/disjuntor.py:457`, `bin/disjuntor.py:146`)

6. **Contador fora em duas gerações consecutivas**

   **(a) Sítio:** nenhum mecanismo atual. O contrato nomeia corretamente o futuro produtor `gerar_estado` e leitor Stop, mas o produtor atual só grava Markdown. (`bin/gerar_estado.py:376`, `bin/hook_stop_fila1.py:1184`)

   **(b) Estado:** sidecar e histórico de gerações não têm proprietário implementado. Os esperados atuais são heterogêneos: prosa de raias/interrompidas, mapa do carimbo do fio e esperado ao fim do programa dos leitores. (`bin/gerar_estado.py:222`, `bin/gerar_estado.py:240`, `bin/fio_estado.py:123`, `bin/leitor_dia_estado.py:46`)

   **(c) Contraexemplo estático:** PAREI ou ESPERANDO com prova podem liberar o Stop sem qualquer consulta a contador fora. Os valores dos “≥4 hoje” não vieram discriminados na sonda. Ocorrência real dessa liberação: **[não verificado]**. (`bin/hook_stop_fila1.py:1226`, `bin/hook_stop_fila1.py:1231`)

   **(d) Lacunas:** **C:** quando uma publicação está concluída e o que ocorre se o gerador falha; **E:** reconhecimento de cada FORA e validade da justificativa; **F:** geração repetida, saltos, sidecar velho, reinício, contador desaparecido e “sem esperado”; **M:** duas gerações distintas, atualização parcial e comparação estruturada; **D:** precedência da chave e da linha de desligamento. Hoje o gerador escreve o destino diretamente e a chave só testa existência. (`bin/gerar_estado.py:376`, `bin/disjuntor.py:146`)

7. **Raias velhas, tar e preservação de commits não pousados** [nao verificado]

   **(a) Sítio:** `raias_estado::ler` mede; `gerar_estado` publica. **Nenhum sítio de arquivamento/remoção** nesses fluxos. O contrato nomeia a publicação, mas não o executor da limpeza. (`bin/raias_estado.py:89`, `bin/raias_estado.py:125`, `bin/gerar_estado.py:215`)

   **(b) Estado:** `raia_agente.sh` escreve declarações em `logs/raias/*.agente`; `raias_estado` lê esses campos e Git; o gerador lê seu resultado. (`bin/raia_agente.sh:54`, `bin/raia_agente.sh:66`, `bin/raias_estado.py:62`, `bin/gerar_estado.py:216`)

   **(c) Contraexemplos do dado:** `tmp-ui`, com dois patches pendentes e sem worktree, fica fora do universo Git enumerado; entradas B sob `.claude/worktrees` são excluídas; `wt-comboio`, `ahead=1/plus=0`, pode ser contado velho apesar de patch já pousado. Esses registros são da sonda, sem reprodução no HEAD: **[não verificado]**. O código exclui `.claude/worktrees` e mede `rev-list`, não equivalência por `cherry`. (`bin/raias_estado.py:85`, `bin/raias_estado.py:96`, `bin/raias_estado.py:101`, `bin/raias_estado.py:123`)

   **(d) Lacunas:** **C:** executor e gatilho; **E:** destino da lista ao Ronald; **F:** definição de velha, detached, A* suja, C em uso e sucesso verificável do tar; **M:** universo completo versus contador atual; **D:** chave/reversão do instrumento não restaura worktree já removida. A medição atual define velha por último commit à frente há mais de seis horas. (`bin/raias_estado.py:39`, `bin/raias_estado.py:103`, `bin/raias_estado.py:123`) Política de restauração: **[não verificado]**.

8. **Modelo explícito e papel do Agent**

   **(a) Sítio:** nenhum gate atual; a porta Agent lê tipo/prompt e consulta B3/B4. `model` e `description` não são critérios de autorização ali. (`bin/disjuntor.py:1451`, `bin/disjuntor.py:1460`)

   **(b) Estado:** o painel lê modelo do primeiro `api-request` e tipo da meta, depois do lançamento; conta agentes por modelo/tipo. Não calcula custo por modelo em 24 h. (`bin/disjuntor.py:1631`, `bin/disjuntor.py:1643`, `bin/disjuntor.py:1647`)

   **(c) Contraexemplo estático:** Agent sem `model`, ou opus descrito como contagem, passa quando B1/B3/B4 permitem. Os fixtures de verificador já omitem `model`. Ocorrência real: **[não verificado]**. (`bin/disjuntor.py:1451`, `bin/tests/test_disjuntor.sh:322`)

   **(d) Lacunas:** **C:** campo autoritativo e sintaxe dos papéis; **E:** classificação de espera de suite; **F:** aliases, modelos desconhecidos e descrições com múltiplos papéis; **M:** custo precisa de tokens/preço e janela, além da contagem existente; **D:** alcance da chave sobre a nova porta. O custo aceito “mal rotulado custa uma volta” exige decisão conjunta com item 1: negar antes de registrar custa zero; registrar sem identidade pode cobrar repetição. (`bin/disjuntor.py:1451`, `bin/disjuntor.py:1631`) Regra dessa cobrança: **[não verificado]**.

9. **Opus final somente com suite inteira verde da árvore** [nao verificado]

   **(a) Sítio:** nenhum gate na abertura do Agent. A regra textual existe; o ramo não lê certificado de suite. (`CLAUDE.md:557`, `bin/disjuntor.py:1454`, `bin/disjuntor.py:1460`)

   **(b) Estado:** há prova adjacente: `regua.sh` escreve `.regua_stamp` com árvore/status/hora; pre-push lê. O contrato não estabelece se esse é o certificado do item 9, nem seu vínculo com a cópia da obra. (`bin/regua.sh:196`, `bin/pre-push.sh:53`, `bin/pre-push.sh:66`) Vínculo contratado: **[não verificado]**.

   **(c) Contraexemplo estático:** final de obra livre pode abrir sem suite registrada. Além disso, o atalho da própria régua compara somente impressão de Python/HTML do app e status: mudança só em `bin/` pode retornar “JA VERDE” sem executar os selos novos. Ocorrência real: **[não verificado]**. (`bin/disjuntor.py:1457`, `bin/regua.sh:33`, `bin/regua.sh:86`, `bin/regua.sh:93`, `bin/regua.sh:138`)

   **(d) Lacunas:** **C:** produtor autorizado do certificado; **E:** definição de inteira; **F:** hash de commit versus árvore, sujeira e alteração durante execução; **M:** prova parcial nunca valer como inteira — a régua já distingue `--only`; **D:** exceção/reabertura e chave. Sem produtor acordado, o gate negaria todo final sem certificado. (`bin/arvore_hash.sh:4`, `bin/regua.sh:181`, `bin/disjuntor.py:1487`) Comportamento do novo gate: **[não verificado]**.

10. **Instrumento commitado exige contrato no push** [nao verificado]

    **(a) Sítio:** nenhum gate atual; `bin/pre-push.sh` é o encaixe correto. O instalador gera wrapper em `.git/hooks/pre-push`; instalação efetiva no host: **[não verificado]**. (`bin/pre-push.sh:33`, `bin/pre-push.sh:200`, `bin/hooks.sh:26`, `bin/hooks.sh:51`)

    **(b) Estado:** falta escritor/leitor do vínculo instrumento→obra→contrato. O pre-push já resolve `PUSH_SHA`, que pode diferir do HEAD; conferir contrato apenas no disco certificaria outra árvore. (`bin/pre-push.sh:33`, `bin/pre-push.sh:37`, `bin/pre-push.sh:39`)

    **(c) Contraexemplo estático:** instrumento sem contrato pode atingir “push liberado” se os demais checks passam. `SKIP_TESTS=1` libera antes de todos os gates. Push concreto dessa classe: **[não verificado]**. (`bin/pre-push.sh:5`, `bin/pre-push.sh:200`)

    **(d) Lacunas:** **C:** classificação de instrumento/hook/cron/selo e alcance do push; **E:** relação entre múltiplas obras e contratos substituídos; **F:** existência versus contrato efetivamente commitado — `logs/` é ignorado; **M:** selo obrigatório em cada push, fora do arsenal pulável; **D:** relação de Ronald/chave/reversão com `SKIP_TESTS`. (`.gitignore:53`, `bin/pre-push.sh:14`, `bin/pre-push.sh:66`, `bin/pre-push.sh:159`, `bin/pre-push.sh:5`)

11. **Sétimo agente vivo negado**

    **(a) Sítio:** nenhum teto Agent atual. `agentes_sem_entrega` reconhece agentes assíncronos lançados sem notificação final; a porta Agent não usa esse leitor. (`bin/disjuntor.py:366`, `bin/disjuntor.py:376`, `bin/disjuntor.py:1451`)

    **(b) Estado:** o helper lê o transcript principal e deduplica IDs; não reserva slots nem persiste registro de todos os vivos. O teto de slots do wrapper é de processos Codex, com `CODEX_TETO` configurável, outra medida. (`bin/disjuntor.py:376`, `bin/disjuntor.py:379`, `bin/codex_pergunta.sh:47`, `bin/codex_pergunta.sh:143`)

    **(c) Contraexemplo estático:** sete Agents podem passar se os outros disjuntores permitirem; não há consulta à quantidade de vivos nessa porta. Ocorrência real: **[não verificado]**. (`bin/disjuntor.py:1451`, `bin/disjuntor.py:1475`)

    **(d) Lacunas:** **C:** sessão, host ou árvore de agentes; **E:** fila/recusa e liberação da vaga; **F:** síncronos, filhos, falha de lançamento e notificação perdida; **M:** disputa simultânea pela sexta vag

[truncado em 20000 de 24510 caracteres]

## 01/11 00:14 · sessao 0398ef9dc32b · - · - · origem=Ronald (tmux)

Leitura concluída só no código. Os casos abaixo são contraexemplos deduzidos do HEAD; ocorrência em produção, salvo o que consta expressamente no dado raspado, é **[não verificado]**.

**Chamadores e fronteira comum**

- O settings aponta PreToolUse sem filtro para `disjuntor.py pretool` e Stop para `hook_stop_fila1.py`; o contrato acerta esses nomes. `.claude/settings.json:3`, `.claude/settings.json:9`, `.claude/settings.json:62`, `.claude/settings.json:68`.
- PreToolUse só consulta B3 no Bash para wrapper identificado como `papel=verificador` e obra presente; no Agent, só para `subagent_type=verificador`, prompt final e obra reconhecida. O Agent comum/construtor não consulta nem registra volta. `bin/disjuntor.py:1440`, `bin/disjuntor.py:1443`, `bin/disjuntor.py:1451`, `bin/disjuntor.py:1454`, `bin/disjuntor.py:1457`.
- A porta do wrapper existe depois de adquirir slot e trava da resposta; chama `--b3-parada` para papel verificador. `bin/codex_pergunta.sh:137`, `bin/codex_pergunta.sh:162`, `bin/codex_pergunta.sh:164`.
- A chave hoje basta por existência, inclusive symlink; PreToolUse a consulta, a porta `b3_parada` não consulta. Nenhum desses dois pontos exige linha no RELATO. `bin/disjuntor.py:144`, `bin/disjuntor.py:1486`, `bin/disjuntor.py:1036`, `bin/disjuntor.py:1042`.

**Item 1 — teto seis/voltas**

(a) O local de encaixe é `decide_b3`, mas o mecanismo atual conta reprovações finais opus e voltas Codex triadas PROCEDENTE; não conta aberturas Agent nem toda chamada Codex. O contrato nomeia o local certo como destino, não como capacidade já existente. `bin/disjuntor.py:878`, `bin/disjuntor.py:896`, `bin/disjuntor.py:902`, `bin/disjuntor.py:910`.

(b) Escritor atual: `_com_estado` grava o JSON inteiro sob flock; `decide_b3` modifica `b3`, `reprovados`, `codex`, `disparado`, `liberado`, `consumidas`, `topo_no_disparo`, `disparado_em`, `b3_sem_obra`; `rearma_b3` modifica `rearmado` e zera listas correntes. Não há `b3[obra].voltas`. `bin/disjuntor.py:237`, `bin/disjuntor.py:271`, `bin/disjuntor.py:891`, `bin/disjuntor.py:936`, `bin/disjuntor.py:939`, `bin/disjuntor.py:1018`.

(c) Sete Agents construtores com obra reconhecida e cota disponível passam: esse ramo não consulta B3 nem incrementa volta. O mesmo vale para sete chamadas Codex sem triagem PROCEDENTE suficiente. Ocorrência real com sete disparos: **[não verificado]**. O teste atual exige que três chamadas não triadas não parem. `bin/disjuntor.py:1451`, `bin/disjuntor.py:1460`, `bin/tests/test_disjuntor.sh:1144`.

(d) Faltam identidade idempotente da abertura, unidade exata de volta entre fontes, reserva entre autorização e execução, significado de N adicional/absoluto na reabertura e alcance da semeadura. A falta de reserva é demonstrável: locks do wrapper são por resposta e sidecar só nasce ao fim rc0; respostas distintas podem autorizar antes que qualquer sidecar apareça. `bin/codex_pergunta.sh:162`, `bin/codex_pergunta.sh:198`. Há CLI atual só de triagem/consulta/rearme, sem reabre/redesenha. `bin/disjuntor.py:1699`.

**Item 2 — segundo ERRO-BOBO**

(a) O sítio certo é `linhas_libera_b3` + consumo em `decide_b3`, mas hoje qualquer ERRO-BOBO novo com motivo alfanumérico pode liberar. `bin/disjuntor.py:626`, `bin/disjuntor.py:650`, `bin/disjuntor.py:930`.

(b) `consumidas` contém apenas hashes das linhas, sem tipo; a linha aceita movimenta todas as reprovações e triagens anteriores para `liberado`. `bin/disjuntor.py:932`, `bin/disjuntor.py:936`, `bin/disjuntor.py:939`.

(c) Primeiro ERRO-BOBO A libera; nova reprovação dispara; segundo ERRO-BOBO B, diferente de A, fora da fotografia e posterior ao disparo, também libera. O código não conta liberações por tipo. Produção: **[não verificado]**. `bin/disjuntor.py:930`, `bin/disjuntor.py:932`, `bin/disjuntor.py:934`, `bin/disjuntor.py:940`.

(d) A migração do histórico só com sha para histórico com tipo não está definida; a trilha possui anúncios com tipo que o painel já conta em janela 24h, mas isso difere de histórico vitalício da obra. `bin/disjuntor.py:939`, `bin/disjuntor.py:1552`, `bin/disjuntor.py:1568`, `bin/disjuntor.py:1574`. Também permanece ambíguo se DESENHO válido libera automaticamente ou exige Ronald: o contrato mantém ambas as formulações **[não verificado]**.

**Item 3 — PROCEDENTE sem cura**

(a) `_procedentes` é fonte certa para identificar procedentes, mas não certifica cura; apenas seleciona IDs das voltas cujo último veredito é procedente. Não consulta git/HEAD/cópia. `bin/disjuntor.py:857`.

(b) Wrapper escreve `.id` com obra/papel/hash/pid/fim/sha da resposta; triagem lê, valida sha da resposta, transforma em identificador `hash12@pid@fim@sha12` e grava `hora,obra,volta,veredito` no log. O caminho da cópia não é preservado nesse log. `bin/codex_pergunta.sh:196`, `bin/codex_pergunta.sh:199`, `bin/disjuntor.py:795`, `bin/disjuntor.py:800`, `bin/disjuntor.py:802`, `bin/disjuntor.py:812`.

(c) Um ou dois PROCEDENTES sem cura ainda permitem opus final: limiar atual Codex é três, independentemente de HEAD posterior. O teste explicitamente pede passagem com dois. Produção: **[não verificado]**. `bin/disjuntor.py:77`, `bin/disjuntor.py:912`, `bin/tests/test_disjuntor.sh:1153`.

(d) O contrato precisa distinguir “HEAD diferente” de “commit de cura posterior”: commit alheio ao achado muda HEAD sem curar; comparação de desigualdade não prova ancestralidade, autoria nem correção. Isso é limite lógico da medida proposta, ocorrência real **[não verificado]**. O identificador atual ainda trunca hash a 12 e perde caminho; resolver identidade/cópia antecede esse item. `bin/disjuntor.py:802`, `bin/disjuntor.py:813`.

**Item 4 — verificador final sem obra**

(a) `_obra_de` é o reconhecedor certo, mas hoje ausência pula a negativa. `bin/disjuntor.py:504`, `bin/disjuntor.py:1455`, `bin/disjuntor.py:1457`.

(b) Após o agente existir, `_reprovacoes_finais` lê meta `agentType=verificador`, prompt e primeira linha do SubagentHandback; `decide_b3` acumula IDs sem obra em `b3_sem_obra`. O painel retorna comprimento acumulado, sem timestamps/janela 24h. `bin/disjuntor.py:546`, `bin/disjuntor.py:561`, `bin/disjuntor.py:736`, `bin/disjuntor.py:892`, `bin/disjuntor.py:1624`.

(c) Agent verificador com “LEITURA FINAL antes do pouso, sem o nome dela” passa a porta quando B4 não nega; depois pode entrar no contador. O fixture atual contém justamente esse prompt e só verifica contagem posterior. Produção: **[não verificado]**. `bin/disjuntor.py:1457`, `bin/tests/test_disjuntor.sh:427`.

(d) Escopo “verificador final” precisa permanecer ligado ao tipo/final ou declarar novo universo; modelo opus não é critério atual. O contrato aceita rótulo como medida, não prova leitura efetiva; prompt com ID formalmente válido mas de obra errada satisfaz o parser. `bin/disjuntor.py:1454`, `bin/disjuntor.py:504`.

**Item 8 — modelo/papel**

(a) **Nenhum mecanismo atual de negativa por model/description/papel.** O ramo Agent lê tipo/prompt e B4; a contagem do modelo é posterior, por meta + primeiro `api-request`. `bin/disjuntor.py:1451`, `bin/disjuntor.py:1460`, `bin/disjuntor.py:1631`.

(b) Transcript/meta são escritos pelo harness segundo o contrato local do leitor; o escritor executável do harness não está nesta cópia **[não verificado]**. O leitor atual usa `params.model` do primeiro api-request e `agentType` da meta. `bin/disjuntor.py:1632`, `bin/disjuntor.py:1641`.

(c) Agent sem `model`, ou opus descrito como contagem, passa se B1/B3/B4 não negarem; fixtures atuais de Agent verificador nem enviam `model`. Produção: **[não verificado]**. `bin/tests/test_disjuntor.sh:322`, `bin/disjuntor.py:1451`.

(d) Lacunas: sintaxe/caixa/acentos dos papéis; campo description versus prompt; vocabulário aliases do modelo; ordem com item1; modelos desconhecidos; definição de “espera de suite”. “Agente mal rotulado custa uma volta” exige contar antes de negar; negar antes custa zero; sem identidade da tentativa, reapresentação pode contar repetidamente. `bin/disjuntor.py:1451`; detalhes de política **[não verificado]**.

**Item 9 — suite inteira verde por árvore/hash** [nao verificado]

(a) **Nenhum gate atual na abertura do opus.** Existe regra textual suite antes do opus, sem leitura de selo no ramo Agent. `CLAUDE.md:557`, `bin/disjuntor.py:1451`.

(b) Writer/reader/schema/local do certificado pretendido não estão nomeados no contrato nem nesse ramo **[não verificado]**; ramo atual só lê B3 e B4. `bin/disjuntor.py:1454`, `bin/disjuntor.py:1460`.

(c) Opus final de obra livre passa sem qualquer suite registrada. Produção: **[não verificado]**. `bin/disjuntor.py:1457`, `bin/disjuntor.py:1475`.

(d) Falta definir “inteira”, origem autêntica do verde, suites necessárias, validade temporal, hash commit versus árvore/estado sujo e relação com cópia citada. Identidade do item3/4 e escritor do selo antecedem gate; gate sem produtor causaria negação permanente **[não verificado]**. A regra textual pede suite inteira da árvore, não fornece formato de prova. `CLAUDE.md:557`. [nao verificado]

**Item 11 — sétimo agente vivo**

(a) **Nenhum teto de seis na porta Agent.** Existe leitor de agentes de fundo sem entrega, mas a porta não o usa. `bin/disjuntor.py:366`, `bin/disjuntor.py:1451`.

(b) `agentes_sem_entrega` lê transcript principal, reconhece strings de lançamento assíncrono e notificações com task-id/status; não é registro de todos agentes vivos nem usa processos. `bin/disjuntor.py:367`, `bin/disjuntor.py:376`.

(c) Sete Agents novos passam enquanto B4/B3/B1 permitem; não há contagem de vivos nesse caminho. Produção: **[não verificado]**. `bin/disjuntor.py:1451`, `bin/disjuntor.py:1475`.

(d) Falta universo (sessão/host; fundo/síncrono; agentes filhos), tratamento de transcript ausente/notification perdida, slot/reserva/liberação e CLI/chave de desligar. O helper só vê o resultado do lançamento depois que ele foi registrado, logo não resolve corrida entre autorizações simultâneas. `bin/disjuntor.py:369`, `bin/disjuntor.py:376`.

**Matriz de escritores/leitores pedidos**

- `disjuntor_estado.json`: escrita física única `_com_estado`, JSON temporário + replace sob flock; leitura `_com_estado`, `ler_estado`; consumidores B1/B3/B4 e painel. Estado ausente é `{}`, diferente de ilegível que levanta — isso conflita com “estado/sidecar ilegível=NAO_SEI” apenas se o contrato pretender incluir ausência de estado já conhecido. `bin/disjuntor.py:237`, `bin/disjuntor.py:262`, `bin/disjuntor.py:291`.
- `triagem.log`: escritor `--triagem` → `_triagem_sob_trava`; leitor `_triagens` sob trava do estado, consumido por decide/rearme. Perda/truncamento detectados por contador de linhas e presença das voltas conhecidas. `bin/disjuntor.py:804`, `bin/disjuntor.py:812`, `bin/disjuntor.py:823`, `bin/disjuntor.py:1007`.
- `.id`: wrapper remove anterior antes da chamada e grava novo só rc0 com resposta desta chamada; disjuntor lê para triagem. Portanto não é histórico completo e “durável entre sessões” não significa imutável: reutilizar resposta remove o anterior. `bin/codex_pergunta.sh:171`, `bin/codex_pergunta.sh:198`, `bin/disjuntor.py:753`.
- Transcripts subagent: escritor harness não disponível **[não verificado]**; leitores locais `_meta`, B1, `_veredito_e_obra`, `_reprovacoes_finais`, cache/modelos do painel. Linhas JSON inválidas são puladas, meta ilegível no leitor de reprovações vira `{}` e agente pode deixar de ser classificado como verificador, sem NAO_SEI nesse percurso. `bin/disjuntor.py:342`, `bin/disjuntor.py:353`, `bin/disjuntor.py:742`, `bin/disjuntor.py:1513`, `bin/disjuntor.py:1631`.

**Ordem que o código exige**

- Antes dos gates 1/3/9/11: identidade da obra/cópia, esquema durável, escritores, idempotência e reserva; os pontos atuais são consulta prévia e sidecar posterior. `bin/disjuntor.py:1451`, `bin/codex_pergunta.sh:198`.
- Item8 e item1 precisam pousar juntos ou declarar qual negativa consome volta; o custo declarado de rótulo errado não resulta automaticamente da ordem proposta **[não verificado]**.
- O tratamento comum NAO_SEI precisa preceder extensão dos ramos: exceções hoje falham fechado só no ramo B3 final/verificador; Agent construtor falha aberto. `bin/disjuntor.py:1410`, `bin/disjuntor.py:1496`.
- Desligamento deve alcançar wrapper antes de depender dele para novos gates; hoje chave libera PreToolUse, mas wrapper continua recusando obra parada. `bin/disjuntor.py:1487`, `bin/codex_pergunta.sh:165`.
- O selo de testes atual cobre B3 anterior (três PROCEDENTES, consumo por sha, contagem posterior sem obra), não prova os novos contratos; suite não foi executada nesta leitura. `bin/tests/test_disjuntor.sh:1125`, `bin/tests/test_disjuntor.sh:427`, `bin/tests/test_disjuntor.sh:1468`.

## 01/11 00:14 · sessao dc80f0305892 · - · - · origem=Ronald (tmux)

Leitura sem executar testes ou geradores. As constatações abaixo são da cópia; a ocorrência em produção de entradas hipotéticas permanece [não verificado]. [nao verificado]

**Item 5 — B1, job próprio em fundo, sem escrita por 15 min**

- **Sítio atual e nome:** `medir_b1` é o medidor (`bin/disjuntor.py:413`), chamado por `b1_do_agente` (`bin/disjuntor.py:480`), somente quando o próprio agente passa no PreToolUse (`bin/disjuntor.py:1435`). O contrato acerta o B1, mas “PreToolUse Agent/Bash” é estreito: o matcher cobre todas as ferramentas (`.claude/settings.json:64`) e o B1 roda para `agent_id`, exceto `SubagentHandback` (`bin/disjuntor.py:1435`). Stop não mede B1: lê IDs já disparados e ainda sem entrega (`bin/hook_stop_fila1.py:1053`).
- **Escritor/leitor:** o harness é indicado como escritor dos transcripts e notificações (`bin/disjuntor.py:336`, `bin/disjuntor.py:369`); sua implementação não está nesta cópia [não verificado]. `medir_b1` lê meta/transcript, caminhos citados e mtimes (`bin/disjuntor.py:415`, `bin/disjuntor.py:421`, `bin/disjuntor.py:425`, `bin/disjuntor.py:454`). `b1_do_agente` escreve `b1[id].{checado,disparado,motivo}` via `_com_estado` (`bin/disjuntor.py:481`, `bin/disjuntor.py:486`); `b1_disparados` lê para Stop (`bin/disjuntor.py:489`).
- **Contraexemplo demonstrável:** tokens zero fazem não-disparo antes de qualquer leitura de arquivos (`bin/disjuntor.py:452`), mesmo que exista Bash em fundo: o medidor não interpreta jobs (`bin/disjuntor.py:428`). Meta/transcript/cópia ilegíveis também não disparam (`bin/disjuntor.py:417`, `bin/disjuntor.py:424`, `bin/disjuntor.py:457`), contrariando o NAO_SEI genérico do v2.
- **Limite do caso real:** a sonda informa 17 esperas, gaps máximos de 184,5 s e tokens nas mensagens; não informa mtimes nem as chamadas de hook. Não prova uma liberação ou negação concreta do B1 [não verificado]. B1 soma input/output/cache_creation, **não** cache_read (`bin/disjuntor.py:449`); inferir disparo só dos ~2,49 Mtok/h de cache_read seria incorreto (`bin/disjuntor.py:449`).
- **Lacunas nos cinco campos:** quem chama precisa incluir todas as ferramentas e reconhecer que não há relógio autônomo (`bin/disjuntor.py:1435`); quem espera precisa distinguir impedir próxima ferramenta de matar job (`bin/disjuntor.py:1438`); falha/limite precisa resolver isenções `SO_LEITURA`, ausência de cópia e ownership por caminho citado (`bin/disjuntor.py:91`, `bin/disjuntor.py:425`); medida precisa incluir jobs sem tokens e erro de leitura (`bin/disjuntor.py:452`); desligar hoje só testa existência da chave, sem exigir RELATO (`bin/disjuntor.py:146`).

**Item 6 — contadores fora em duas gerações**

- **Sítio atual:** nenhum mecanismo de sidecar/gerações no gerador ou Stop; `gerar_estado` escreve apenas seu `dst` Markdown (`bin/gerar_estado.py:376`), e Stop extrai apenas `transcript_path` do payload (`bin/hook_stop_fila1.py:1187`). O contrato nomeia corretamente o futuro produtor/leitor, mas não descreve comportamento já existente (`bin/gerar_estado.py:376`, `bin/hook_stop_fila1.py:1034`).
- **Esperado atual heterogêneo:** raias e interrompidas o expressam em prosa (`bin/gerar_estado.py:222`, `bin/gerar_estado.py:240`); fio compara valores contra mapa vindo do carimbo (`bin/fio_estado.py:123`); leitores do dia têm `esperado_no_fim` (`app/core/medida_leitor_do_dia.py:409`, `bin/leitor_dia_estado.py:46`); placar usa `meta`, inclusive textual (`bin/gerar_estado.py:97`, `bin/gerar_estado.py:104`).
- **Escritor/leitor:** atualmente não há escritor/leitor de `estado_contadores.json` nos sítios contratados (`bin/gerar_estado.py:376`, `bin/hook_stop_fila1.py:1034`). `_com_estado` é o ponto existente adequado para a história compartilhada: lê/muta/grava sob flock e substituição atômica (`bin/disjuntor.py:237`, `bin/disjuntor.py:253`, `bin/disjuntor.py:268`, `bin/disjuntor.py:274`).
- **Contraexemplo demonstrável:** PAREI legítimo encerra antes de qualquer cobrança de contadores (`bin/hook_stop_fila1.py:1226`); ESPERANDO com prova também encerra (`bin/hook_stop_fila1.py:1231`). Contadores fora duas vezes não alteram esses ramos porque não são lidos (`bin/hook_stop_fila1.py:1184`). Valores reais dos “≥4 fora hoje” não foram fornecidos [não verificado].
- **Lacunas:** chamador precisa definir publicação concluída e falha antes de publicar; esperado/valor precisam definir comparação e “sem esperado”; duas gerações precisa definir reinício, sidecar velho, saltos, geração repetida e mudança do conjunto; medida precisa demonstrar histórico e publicação parcial; desligar não possui exigência atual de RELATO (`bin/disjuntor.py:146`). Esses casos não têm tratamento do novo sidecar no código atual (`bin/gerar_estado.py:376`, `bin/hook_stop_fila1.py:1034`).

**Item 12 — cota desconhecida por mais de um Stop**

- **Sítio atual e nome:** `ler_cota` existe (`bin/disjuntor.py:1064`), B4 Stop existe (`bin/disjuntor.py:1202`), mas contar Stops de desconhecido e cobrar FORA não existe nessa transição: cota desconhecida não produz `escada`, e `b4_stop` apaga `b4_para` quando não há escada (`bin/disjuntor.py:1111`, `bin/disjuntor.py:1208`).
- **Escritor:** `statusline_cota.py` está configurado (`.claude/settings.json:76`); valida os dois números, grava `{seven_day_pct,seven_day_resets_at,hora,contexto_tokens,contexto_pct,session_id}` e troca atomicamente (`bin/statusline_cota.py:45`, `bin/statusline_cota.py:51`, `bin/statusline_cota.py:54`). Sem ambos, não escreve (`bin/statusline_cota.py:47`); erro é silenciado (`bin/statusline_cota.py:55`). Não há escritor manual de “escada N” aqui: só reconhece prefixo `escada`, sem extrair N (`bin/disjuntor.py:87`, `bin/disjuntor.py:1115`).
- **Leitor:** `ler_cota` lê percentual/reset e ignora hora/session_id (`bin/disjuntor.py:1069`); `escada` e `b4_decide` o usam (`bin/disjuntor.py:1111`, `bin/disjuntor.py:1126`); painel também (`bin/disjuntor.py:1619`). Transição B4 escreve `b4_para.desde` via `_com_estado` (`bin/disjuntor.py:1215`, `bin/disjuntor.py:1222`).
- **Contraexemplo:** cota ausente/ilegível retorna None (`bin/disjuntor.py:1075`), e Agent passa B4 sem escada (`bin/disjuntor.py:1127`). A sonda relata cota ausente, mas não traz uma chamada e sua decisão: a ocorrência concreta de liberação é [não verificado]. Causa do payload ausente e execução efetiva da statusline também são [não verificado].
- **Lacunas:** definir N/unidade/intervalo, hora/validade e precedência entre fonte manual e statusline; definir se o Stop seguinte à linha FORA reinicia sequência; definir persistência entre sessões; separar desconhecido de escada real; resolver Stop excepcional que hoje falha aberto (`bin/hook_stop_fila1.py:1305`). Quem desliga hoje é existência de chave (`bin/hook_stop_fila1.py:1039`); RELATO obrigatório é novo (`bin/disjuntor.py:146`).

**Dependências e medidas que afetam a consolidação**

- Antes de item 6 cobrar: contratos estruturados dos medidores → sidecar e geração concluída → histórico sob trava → decisão Stop. As fontes atuais não compartilham esquema (`bin/gerar_estado.py:222`, `bin/fio_estado.py:123`, `app/core/medida_leitor_do_dia.py:409`).
- Cobranças de 6/12 têm de preceder **todos** os atalhos que liberam; há B4, orçamento, PAREI, ESPERANDO, teto e B2 (`bin/hook_stop_fila1.py:1207`, `bin/hook_stop_fila1.py:1218`, `bin/hook_stop_fila1.py:1226`, `bin/hook_stop_fila1.py:1231`, `bin/hook_stop_fila1.py:1293`, `bin/hook_stop_fila1.py:1295`). Inserir só nos “recados” finais deixa os atalhos passarem (`bin/hook_stop_fila1.py:1300`).
- Exceções do disjuntor no Stop retornam None (`bin/hook_stop_fila1.py:1059`); exceção geral libera (`bin/hook_stop_fila1.py:1305`). Isso precisa ser conciliado com “nunca verde calado” antes da cobrança (`bin/hook_stop_fila1.py:1305`).
- `gerar_estado` é chamado depois de `relato_nomes.sh`, cuja falha encerra o publicador (`bin/relato.sh:32`, `bin/relato.sh:38`). Sidecar pode ficar velho se essa dependência falhar; ocorrência real [não verificado].
- “Custo por modelo 24 h” não é a medida atual: `_agentes_24h` conta agentes por modelo/tipo, usando mtime do meta (`bin/disjuntor.py:1631`, `bin/disjuntor.py:1637`, `bin/disjuntor.py:1647`); cache é soma de cache_read por **dia civil**, não custo nem janela móvel (`bin/disjuntor.py:1512`, `bin/disjuntor.py:1536`, `bin/disjuntor.py:1543`).
- As suítes atuais têm casos B1/B4 e escada-palavra (`bin/tests/test_disjuntor.sh:1468`); `escada_palavra` explicitamente encerra escada com a mensagem humana seguinte (`bin/tests/test_disjuntor.sh:500`). Resultado de execução atual dos 12 mutantes prometidos é [não verificado].

## 01/11 00:14 · sessao 63ce410ff0b6 · - · - · origem=Ronald (tmux)

**Item 7 — raias velhas**

- **Sítio:** `bin/raias_estado.py::ler`; contrato aponta o ESTADO corretamente como publicador, mas não nomeia chamador/escritor de limpeza. O gerador importa o medidor e publica suas linhas; o medidor somente consulta Git/declarações e retorna dados, sem arquivar ou remover worktrees. `bin/gerar_estado.py:215`, `bin/gerar_estado.py:222`, `bin/raias_estado.py:89`, `bin/raias_estado.py:125`.
- **Escritor/leitor:** `bin/raia_agente.sh` grava `logs/raias/<raia>.agente` com `RAIA`, `OBRA`, `QUEM`, `DESDE`, `FALTA`; atualiza `FALTA`, registra histórico e remove a declaração no fechamento. `bin/raia_agente.sh:54`, `bin/raia_agente.sh:58`, `bin/raia_agente.sh:66`, `bin/raia_agente.sh:76`, `bin/raia_agente.sh:77`. O único leitor declarado é `raias_estado`, que lê os pares `chave=valor`; `gerar_estado` lê o resultado. `bin/raia_agente.sh:10`, `bin/raias_estado.py:62`, `bin/gerar_estado.py:216`.
- **Universo incorreto para o dado raspado:** o medidor exclui `.claude/worktrees`, só enumera worktrees e declarações, portanto não alcança branches sem worktree; conta `rev-list main..HEAD`, sem avaliar equivalência de patches por `git cherry`. `bin/raias_estado.py:85`, `bin/raias_estado.py:93`, `bin/raias_estado.py:96`, `bin/raias_estado.py:101`. No dado fornecido, `tmp-ui` tem dois patches não pousados sem worktree; existem entradas B sob `.claude/worktrees`; `wt-comboio` tem `ahead=1`, `plus=0`, idade 451,8 h. As duas primeiras classes escapam e a terceira é contada velha pelo critério atual, embora não tenha patch pendente; ocorrência fornecida, sem arquivo disponível no HEAD [não verificado]. A regra que marca velha usa apenas idade do último commit à frente, sem descontar patch já pousado. `bin/raias_estado.py:103`, `bin/raias_estado.py:123`.
- **Outro caso estático:** falha de Git retorna `''`, que vira contagem zero; nenhuma declaração e nenhum resultado Git permitem publicar “nenhuma raia viva”. `bin/raias_estado.py:42`, `bin/raias_estado.py:48`, `bin/raias_estado.py:101`, `bin/gerar_estado.py:229`. Ocorrência concreta dessa falha [não verificado].
- **Lacunas:** falta definir quem chama/espera a limpeza; universo inclui branch sem worktree, detached e `.claude/worktrees`? “velha” continua sendo último commit >6h, nascimento, inatividade ou classe A/B/C da sonda? O mecanismo atual usa >6h do último commit à frente e “agente vivo” significa somente declaração não baixada. `bin/raias_estado.py:20`, `bin/raias_estado.py:39`, `bin/raias_estado.py:103`, `bin/raias_estado.py:123`. Tar sem verificação de sucesso/restauração, preservação de sujeira/A*, exclusão de C, destino da lista B e comprovação de “motivo” continuam sem sítio implementado [não verificado]. A chave do disjuntor não é consultada por esse medidor. `bin/raias_estado.py:89`, `bin/raias_estado.py:135`.
- **Medida existente não cobre limpeza:** o teste de raias verifica publicação, diferença com/sem declaração e unicidade de acesso a `logs/raias`; seu caso real sintético é uma única worktree um commit à frente. Não prova tar, remoção segura, branches sem worktree ou equivalência de patches. `bin/tests/test_raia_declarada_no_estado.sh:5`, `bin/tests/test_raia_declarada_no_estado.sh:38`, `bin/tests/test_raia_declarada_no_estado.sh:53`, `bin/tests/test_raia_declarada_no_estado.sh:68`.

**Item 10 — contrato exigido no push** [nao verificado]

- **Sítio:** nenhum gate de contrato implementado em `bin/pre-push.sh`; o arquivo faz gates específicos, resolve `PUSH_SHA`, testa a árvore e termina liberando. O nome do ponto de encaixe está correto, a regra é nova. `bin/pre-push.sh:10`, `bin/pre-push.sh:19`, `bin/pre-push.sh:33`, `bin/pre-push.sh:38`, `bin/pre-push.sh:198`, `bin/pre-push.sh:200`.
- **Chamador efetivo:** `hooks.sh install` escreve `.git/hooks/pre-push` como wrapper delegando para `$(git rev-parse --show-toplevel)/bin/pre-push.sh`; `hooks.sh check` verifica delegação. Instalação real no host desta sonda [não verificado]. `bin/hooks.sh:26`, `bin/hooks.sh:51`, `bin/hooks.sh:56`.
- **Leitores/escritores adjacentes:** a régua escreve `.regua_stamp` com árvore/status/hora; o pre-push lê esse arquivo e pode pular o arsenal. `bin/regua.sh:196`, `bin/regua.sh:198`, `bin/regua.sh:199`, `bin/pre-push.sh:53`, `bin/pre-push.sh:66`, `bin/pre-push.sh:70`. Nenhum escritor/leitor de contrato exigido está estabelecido por esses sítios [não verificado].
- **Contraexemplo estático:** um push só de instrumento pode chegar a `OK — push liberado` sem leitura de contrato; além disso, `SKIP_TESTS=1` retorna sucesso antes de todos os gates. Um push concreto dessa classe na produção [não verificado]. `bin/pre-push.sh:5`, `bin/pre-push.sh:200`.
- **Lacunas nos cinco campos:** falta vínculo determinístico arquivo/commit→obra→contrato, universo de mudanças de “hook/cron/selo de host”, regra para vários instrumentos/obras no mesmo push, qual contrato substitui qual e se deve estar no mesmo commit, no alcance do push ou apenas na árvore. O arquivo já distingue `PUSH_SHA` de HEAD; contrato conferido só no disco pode certificar outra árvore. `bin/pre-push.sh:33`, `bin/pre-push.sh:37`, `bin/pre-push.sh:39`. O destino pedido fica sob `logs/`, ignorado pelo Git; `.claude/` também é ignorada. ` .gitignore:53`, `.gitignore:66`. Quem desliga especificamente o gate e como interage com `SKIP_TESTS` não foi definido; a linha atual libera por variável de ambiente. `bin/pre-push.sh:5`.

**Medida, documentação e dependências**

- Os testes de host são chamados pela régua, antes da suite, no laço `bin/tests/test_*.sh`; o próprio pre-push registra que esse laço não roda ali e pode haver régua “JÁ VERDE”. Logo o gate 10 precisa estar na parte incondicional do push se pretende julgar cada push; colocar apenas um selo no laço da régua não cumpre essa função. `bin/regua.sh:138`, `bin/regua.sh:140`, `bin/pre-push.sh:14`, `bin/pre-push.sh:15`, `bin/pre-push.sh:66`.
- O teste atual do disjuntor já tem mutantes com controle da cópia íntegra e falha se a costura removida não for detectada. Isso fornece estrutura de medida, sem provar os 12 RED novos nem resultado vermelho/verde desta obra. `bin/tests/test_disjuntor.sh:1474`, `bin/tests/test_disjuntor.sh:1481`, `bin/tests/test_disjuntor.sh:1489`. Execução dos 12 novos [não verificado].
- `test_hook_parei_so_lei.sh` cobre PAREI técnico/lei, `!` e item só registrado; não há nesses casos a nova regra de contadores/FORA. `bin/tests/test_hook_parei_so_lei.sh:40`, `bin/tests/test_hook_parei_so_lei.sh:48`, `bin/tests/test_hook_parei_so_lei.sh:68`, `bin/tests/test_hook_parei_so_lei.sh:78`.
- A seção 7b já declara separação do pouso de instrumento e produto, raias velhas esperado zero e contrato de cinco linhas mandado uma vez ao Codex; não equivale ao novo enforcement do push. `CLAUDE.md:512`, `CLAUDE.md:514`, `CLAUDE.md:574`. A revisão de linhas de mecanismo no mesmo commit é coerente com a documentação no marco, cuja regra está em `CLAUDE.md:517`. [nao verificado]
- A ordem do item 7 depende primeiro de resolver o universo/identidade de “não pousado” e só depois de medir zero/justificar ou remover: o produtor atual não alcança o universo da sonda e confunde ahead com patches pendentes. `bin/raias_estado.py:85`, `bin/raias_estado.py:96`, `bin/raias_estado.py:101`. A ordem do item 10 depende de tornar inequívoco contrato commitado/obra e o alcance `PUSH_SHA` antes de endurecer a recusa; `logs/` é ignorado hoje. `.gitignore:53`, `bin/pre-push.sh:33`. O contrato prevê gate10 apenas no segundo pouso; a proteção dos instrumentos do primeiro pouso pelo gate10 ainda inexistente [não verificado].

_1326 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
