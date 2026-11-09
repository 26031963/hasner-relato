# CODEX-ESTADO

> Ultima resposta processada: **09/11/2026 14:36** · publicado em **09/11/2026 14:39** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 09/11 14:36 · sessao 10d859872129 · - · - · origem=Ronald (tmux)

| Item | Veredito e contraexemplo |
|---|---|
| A1 — identidade normal | **NAO PROCEDE** colisão original: UUID nasce uma vez, valida antes da vaga, segue ao censo e ao ini; casamento exige obra/pid/hash/tent. (`bin/codex_pergunta.sh:92`, `bin/codex_pergunta.sh:94`, `bin/codex_pergunta.sh:240`, `bin/codex_pergunta.sh:246`, `bin/codex_pergunta.sh:300`, `bin/disjuntor.py:989`, `bin/disjuntor.py:990`) |
| A1 — persistência na classe | **PROCEDENTE.** `CODEX_LOGD=$R/logs/alternativo` é permitido; o censo grava na raiz viva, mas o ini vai ao LOGD alternativo, enquanto B3 procura `logs/codex/chamadas.log` fixo. Chamada real com UUID nunca efetiva fotografia; pode virar SEM-FOTO. (`bin/codex_pergunta.sh:88`, `bin/codex_pergunta.sh:200`, `bin/codex_pergunta.sh:240`, `bin/codex_pergunta.sh:300`, `bin/codex_limite_estado.py:22`, `bin/codex_limite_estado.py:53`, `bin/disjuntor.py:1026`, `bin/disjuntor.py:1143`) |
| A1 — ini impossível de gravar | **PROCEDENTE.** `printf` falha e `INI=1` executa mesmo assim; não existe `set -e`. Ex.: chamadas.log regular sem permissão de escrita, demais saídas graváveis. O wrapper pode chamar Codex sem ini persistido. (`bin/codex_pergunta.sh:53`, `bin/codex_pergunta.sh:299`, `bin/codex_pergunta.sh:300`, `bin/codex_pergunta.sh:408`) |
| A1 — truncamento | **PROCEDENTE.** Obra de 210 caracteres (`OBRA-` + 205 letras A) passa a forma e cabe no nome da worktree; o detalhe SNAPSHOT ultrapassa 300 caracteres. O escritor corta o UUID, `_SNAP_RE` deixa de reconhecer a foto, mas a porta retorna sucesso. Demonstrado usando AST extraída em memória, sem arquivos. (`bin/disjuntor.py:183`, `bin/disjuntor.py:923`, `bin/disjuntor.py:925`, `bin/disjuntor.py:225`, `bin/disjuntor.py:899`, `bin/disjuntor.py:2128`, `bin/disjuntor.py:2133`, `bin/disjuntor.py:2138`; nome: `bin/codex_pergunta.sh:214`) |
| Retry/rc75/sonda | **NAO PROCEDE** vaga sem UUID na variável: geração/validação precedem todo ramo. Pausa sai antes da vaga; limite sai depois da chamada; sonda é recusada no construtor. Não há retry interno entre as duas chamadas únicas do Codex. Persistência tem as falhas acima. (`bin/codex_pergunta.sh:92`, `bin/codex_pergunta.sh:193`, `bin/codex_pergunta.sh:197`, `bin/codex_pergunta.sh:408`, `bin/codex_pergunta.sh:415`, `bin/codex_pergunta.sh:449`, `bin/codex_pergunta.sh:462`) |
| A2 | **NAO PROCEDE** desempate por N: índice da linha é preservado; ordenação usa `(t,i)` e consulta escolhe a última. (`bin/disjuntor.py:1010`, `bin/disjuntor.py:1018`, `bin/disjuntor.py:1020`, `bin/disjuntor.py:1031`, `bin/disjuntor.py:1039`) |
| A4 | **Cura segura parcial.** CLI exige inteiro; B3 não empresta foto da identidade nua. Porém B3 preserva `O9 (C)`, enquanto a porta/consulta recusam essa identidade: não existe representação comum que permita fotografar a obra qualificada. (`bin/disjuntor.py:659`, `bin/disjuntor.py:923`, `bin/disjuntor.py:1127`, `bin/disjuntor.py:1128`, `bin/disjuntor.py:2100`, `bin/disjuntor.py:2750`, `bin/disjuntor.py:2801`) |
| A5 — ordem original | **NAO PROCEDE** perda original evento após memo: escrita estrita precede memo. (`bin/disjuntor.py:1158`, `bin/disjuntor.py:1164`) |
| A5 — repetição incoerente | **PROCEDENTE.** Primeiro evento PROVADAS grava; `os.replace` do estado falha. RELATO perde uma medida antes da próxima chamada. Sem memo persistido, o mesmo aid vira METADE, incorporado pelo B3; contador mantém PROVADAS porque escolhe primeiro evento. (`bin/disjuntor.py:404`, `bin/disjuntor.py:411`, `bin/disjuntor.py:1123`, `bin/disjuntor.py:1146`, `bin/disjuntor.py:1158`, `bin/disjuntor.py:1320`, `bin/disjuntor.py:1328`, `bin/disjuntor.py:2608`) |
| A5 — futuro | **PROCEDENTE.** Linha futura PROVADAS aid=a1 seguida de linha presente METADE aid=a1 resulta em `finais=0`, `metade=[]`: dedup ocorre antes do filtro temporal. Demonstrado pela função real extraída por AST com StringIO. (`bin/disjuntor.py:2605`, `bin/disjuntor.py:2608`, `bin/disjuntor.py:2610`, `bin/disjuntor.py:2611`) |
| Fotografia de conteúdo | **PROCEDENTE.** CENSO, ETAPAS e SHA leem arquivo separadamente. Troca do brief entre N3 e hash do conteúdo N5 produz `sha(N5):N3`; não existe revalidação comum. Ocorrência real **[nao verificado]**. (`bin/disjuntor.py:1888`, `bin/disjuntor.py:1945`, `bin/disjuntor.py:2116`, `bin/disjuntor.py:2119`, `bin/disjuntor.py:2120`, `bin/disjuntor.py:1186`, `bin/disjuntor.py:2128`) |

Lei relevante: contador deve corresponder ao universo e leitura deve consumir a mesma autoridade; estado tem escritor canônico. (`CLAUDE.md:20`, `CLAUDE.md:32`, `CLAUDE.md:35`)

| Arquivo/campo | Escritor | Leitor |
|---|---|---|
| chamadas.log ini: hora/obra/papel/hash/pid/pergunta/tent | `bin/codex_pergunta.sh:299`, `bin/codex_pergunta.sh:300` | Parser `bin/codex_limite_estado.py:56`, `bin/codex_limite_estado.py:64`, `bin/codex_limite_estado.py:66`; foto `bin/disjuntor.py:1026`; primeira abertura `bin/disjuntor.py:1086`; censo `bin/disjuntor.py:2565` |
| chamadas.log fim: sessão/rc/duração | `bin/codex_pergunta.sh:266`, `bin/codex_pergunta.sh:267` | `bin/codex_limite_estado.py:68`; últimas chamadas ainda casam por obra/hash/pid, ignorando tent: `bin/codex_limite_estado.py:89`; origem Code: `bin/codex_limite_estado.py:97`, `bin/codex_limite_estado.py:98` |
| disjuntor.log instante/evento/detalhe | Único append `bin/disjuntor.py:225`; adaptador `bin/disjuntor.py:233` | Leitores específicos abaixo |
| SNAPSHOT obra/N/via/pid/hash/tent/briefs | Montagem `bin/disjuntor.py:2128`; CLI `bin/disjuntor.py:2133`; Agent `bin/disjuntor.py:2137`, `bin/disjuntor.py:2232` | Parser `bin/disjuntor.py:899`; ingestão `bin/disjuntor.py:1011`; casamento `bin/disjuntor.py:1026`; N único B3 `bin/disjuntor.py:1128`, CLI `bin/disjuntor.py:2805` |
| ETAPAS finais + aid | `bin/disjuntor.py:1152`, `bin/disjuntor.py:1158` | `bin/disjuntor.py:2597`, `bin/disjuntor.py:2606` |
| disjuntor_estado.json memo/filas | Memo `bin/disjuntor.py:1164`; B3 `bin/disjuntor.py:1326`, `bin/disjuntor.py:1328`; gravação única `bin/disjuntor.py:398`, `bin/disjuntor.py:404` | Memo `bin/disjuntor.py:1123`; estado `bin/disjuntor.py:424` |
| RELATO e RELATO-ARQUIVO medidas K | Autoria real **[nao verificado]** | Helper `bin/disjuntor.py:1042`; contador `bin/disjuntor.py:1055`; cruzamento B3 `bin/disjuntor.py:1146`, `bin/disjuntor.py:1149`; verificador humano/modelo `.claude/agents/verificador.md:18` |
| ETAPAS n/N do verificador | Instrução `.claude/agents/verificador.md:18`; execução **[nao verificado]** | `bin/disjuntor.py:1147`, `bin/disjuntor.py:1148` |
| ESTADO linha exibida | Composição `bin/disjuntor.py:2734`; publicação `bin/gerar_estado.py:281` | Gerador consome `ler/linha`: `bin/gerar_estado.py:280` |

Segundo escritor introduzido: **NAO PROCEDE** nos sítios auditados; append, estado e N convergem nas funções citadas. Duplicação de juiz **registrado** não demonstrada; N tem autoridade compartilhada, enquanto `n_v` e `n_rel` são duas evidências cruzadas explicitamente pelo mínimo. (`bin/disjuntor.py:225`, `bin/disjuntor.py:404`, `bin/disjuntor.py:1128`, `bin/disjuntor.py:2805`, `bin/disjuntor.py:1149`)

## 09/11 14:36 · sessao 87856602ee3a · - · - · origem=Ronald (tmux)

| Item | Conclusão | Evidência / contraexemplo |
|---|---|---|
| A10 — positivo Agent normal | **NAO PROCEDE a ausência original desse positivo.** | Abre Agent, exige PROVADAS pelo B3 e consulta `--etapas`: `bin/tests/test_etapas_no_brief.sh:529`, `:532`, `:535`; mutante remove ingestão Agent em `:698`. Execução/morte efetivas **[nao verificado]**. |
| A10 — classe de exceção | **PROCEDENTE.** | O teste injeta `RuntimeError` e exige somente stdout vazio: `bin/tests/test_etapas_no_brief.sh:540`, `:546`. Entretanto `_censo_na_excecao` passa `oks=[]` descartável, retorna `None`, e o hook libera: `bin/disjuntor.py:2187`, `:2190`, `:2365`. Uma abertura conforme na exceção passa sem SNAPSHOT/APROVADO; o teste fica verde sem provar a fotografia. |
| A10 — concorrência | **PROCEDENTE quanto à força da prova.** | O teste inicia oito processos, dorme 0,6 s e só conta linhas: `bin/tests/test_etapas_no_brief.sh:470`, `:473`, `:475`. Não há barreira confirmando que todos leram antes da soltura nem verificação dos oito rc. Um carimbo com outros processos falhando ainda satisfaz `:476`. |
| A11 | **NAO PROCEDE.** | Âncora de `bin/tests/test_censo_antes_da_vaga.sh:755` casa uma vez em `bin/disjuntor.py:2779`. Troca pertencimento canônico por célula crua. A fixture qualificada exige negativa e depois aprovação com brief conforme: `bin/tests/test_censo_antes_da_vaga.sh:537`, `:543`, `:545`. Morte executada **[nao verificado]**. |
| Guarda `#G:tentativa`, exit 2 | **PROCEDENTE: falta mordida própria.** | Guarda em `bin/codex_pergunta.sh:93`. Os mutantes alteram ini ou geram UUID fixo **válido**: `bin/tests/test_codex_pergunta.sh:560`, `:561`. Nenhum fornece vazio/UUID inválido removendo a recusa. Catálogo só cobra presença da tag, não cada guarda: `:563`. |
| Mutante `abertura-sem-hash` | **PROCEDENTE: morte por propriedade diferente.** | `bin/tests/test_censo_antes_da_vaga.sh:726` substitui o hash do **lado ini** por `''`; o confronto continua sendo igualdade integral (`bin/disjuntor.py:989`, `:990`). Portanto rejeita casamento válido, em vez de permitir hash divergente. A válido: fixture `:474`, `:481`; J divergente: `:476`, `:483`. O contador “sem” vira 9, em vez de 7, pelos matches válidos perdidos (`:488`), não pelo casamento ilegal de J. |
| Mutante `obra-fora-da-forma-passa` | **PROCEDENTE: não produz a abertura anunciada.** | Remove só o ramo `nc == 'forma'`: `bin/tests/test_censo_antes_da_vaga.sh:747`. Continua negando no `if nc`, agora por ETAPAS: `bin/disjuntor.py:2104`. Caso `O9 (C)` exige também texto “fora da forma do censo”: `bin/tests/test_censo_antes_da_vaga.sh:304`, `:306`. Morre pela mensagem diferente, enquanto a abertura continua negada. |
| Mutante `t2-carimbo-a-cada-final` | **PROCEDENTE: causa contaminada pela releitura.** | Mutação faz `_instalado_lido` ignorar todo carimbo (`bin/tests/test_etapas_no_brief.sh:641`). A releitura após append permanece sem resultado, causando `RuntimeError` em `bin/disjuntor.py:973`. O caso conta carimbos em `bin/tests/test_etapas_no_brief.sh:353`, mas a sequência já negou os finais; não isola “finais normalmente conferidos criam carimbos repetidos”. Execução **[nao verificado]**. |
| Harness ETAPAS | **NAO PROCEDE a alegação de aceitar qualquer rc.** | Exige rc≠0 e nome do caso em `bin/tests/test_etapas_no_brief.sh:588`. Isso não garante a causa interna correta, como o mutante anterior mostra. |
| Harness censo vizinho | **PROCEDENTE.** | Aceita qualquer rc≠0 em `bin/tests/test_censo_antes_da_vaga.sh:687`. |
| Harness wrapper | **PROCEDENTE quanto à declaração genérica “rc≠0 e mensagem específica”.** | Não verifica rc nem mensagem específica; procura qualquer `FALHA` emitida por `c_$caso`: `bin/tests/test_codex_pergunta.sh:579`, `:580`. |
| RED/GREEN e mortes executadas | **[nao verificado].** | A cópia não contém `.git`; os arquivos de RED/GREEN descritos na entrega não foram disponibilizados como artefatos independentes. Não usei o relato do construtor como comprovação. |

A exigência de RED evidenciado está em `CLAUDE.md:28`; a exigência de contraexemplo além do golden, em `CLAUDE.md:29`; contador e rótulo devem corresponder ao universo real, em `CLAUDE.md:35`.

## 09/11 14:36 · sessao c9cc3705ce2d · - · - · origem=Ronald (tmux)

| Item | Conclusão | Evidência e contraexemplo |
|---|---|---|
| A3 — unicidade concorrente | **PROCEDENTE: classe permanece aberta.** | `_instalado_lido` ignora carimbos posteriores ao instante particular do chamador (`bin/disjuntor.py:939`); essa filtragem é repetida sob o flock (`bin/disjuntor.py:967`). P1 lê borda 10:00:00, atrasa; P2 lê 10:00:01, adquire primeiro e carimba 10:00:01; P1 adquire depois, ignora esse carimbo e apenda 10:00:00 (`bin/disjuntor.py:969`). Ficam duas linhas mesmo com exclusão correta. |
| A3 — leitura do hook bloqueada pelo flock | **NAO PROCEDE para a leitura direta.** | O trecho citado do hook faz `stat` e comparação de inode, sem adquirir flock no disjuntor.log (`bin/hook_stop_fila1.py:659`, `bin/hook_stop_fila1.py:666`, `bin/hook_stop_fila1.py:679`). O helper de abertura também não adquire flock (`bin/orcamento_contexto.py:73`). |
| A3 — hook pode esperar indiretamente | **PROCEDENTE, com teto nominal de aproximadamente 1 s.** | Stop chama `julga_b3` (`bin/hook_stop_fila1.py:1204`); B3 detém a trava de estado (`bin/disjuntor.py:383`) ao chamar `_confere_etapas` (`bin/disjuntor.py:1320`), que pode disputar a trava do carimbo (`bin/disjuntor.py:1126`, `bin/disjuntor.py:961`). Há 20 tentativas e sleeps de 50 ms (`bin/disjuntor.py:911`, `bin/disjuntor.py:964`). |
| Ordem de travas | **NAO PROCEDE inversão demonstrada.** | B3 adquire estado→carimbo; o carimbo não chama escritor do estado e libera seu `with` antes de retornar (`bin/disjuntor.py:398`, `bin/disjuntor.py:958`, `bin/disjuntor.py:974`). Não demonstrado ciclo reverso neste código. |
| A6 — DESLIGADO | **PROCEDENTE: preservação incompleta.** | Com carimbo ocupado e chave desligada, Agent abre, grava somente FALHA e pula DESLIGADO (`bin/disjuntor.py:2313`, `bin/disjuntor.py:2315`, `bin/disjuntor.py:2334`). Retire a obra da fila antes do próximo B3: `_sujeita` não reconhece essa FALHA (`bin/disjuntor.py:907`, `bin/disjuntor.py:1103`), e o final recebe FORA (`bin/disjuntor.py:1130`). |
| A7 — carimbo antes de dispensar FORA | **NAO PROCEDE a diferença original.** | Agent carimba antes do pertencimento (`bin/disjuntor.py:2094`, `bin/disjuntor.py:2097`); wrapper também (`bin/disjuntor.py:2754`, `bin/disjuntor.py:2775`). |
| A8 — todo caminho novo usa borda | **PROCEDENTE: não fecha a classe.** | Negativas ETAPAS seguem por `trilha()` no wrapper e PreTool (`bin/disjuntor.py:2785`, `bin/disjuntor.py:2338`); esse helper omite `agora` (`bin/disjuntor.py:233`), provocando `datetime.now()` interno (`bin/disjuntor.py:219`). Essas negativas são insumo histórico de sujeição (`bin/disjuntor.py:908`, `bin/disjuntor.py:1104`). FALHA nova de DESLIGADO também usa esse caminho (`bin/disjuntor.py:2313`). Isso fere a classe explicitamente enumerada em `CLAUDE.md:371`. |
| A8 — dois caminhos declarados antigos | **Existem; antiguidade pelo diff fornecido, execução histórica [nao verificado].** | `trilha()` omite instante (`bin/disjuntor.py:233`); `decide_b3` lê relógio se recebe `None` (`bin/disjuntor.py:1298`). Há chamador real que omite: `--b3-parada`→`b3_parada`→`estado_b3` (`bin/disjuntor.py:2855`, `bin/disjuntor.py:1474`, `bin/disjuntor.py:1421`). Stop e PreTool passam instante (`bin/hook_stop_fila1.py:1198`, `bin/hook_stop_fila1.py:1204`, `bin/disjuntor.py:2202`). |
| A9 — wrapper pré-vaga isentando sem ini | **NAO PROCEDE o defeito original.** | Parser de primeira abertura aceita apenas Agent na trilha (`bin/disjuntor.py:904`); wrapper precisa de ini construtor (`bin/disjuntor.py:1086`). |
| Nova falha no ramo de exceção | **PROCEDENTE: abre sem fotografia.** | `_censo_na_excecao` fornece lista temporária `oks=[]` à porta e a descarta (`bin/disjuntor.py:2187`); nenhum escritor publica essa foto nesse ramo. Sendo conforme, retorna `None` e `main_pretool` libera a ferramenta (`bin/disjuntor.py:2190`, `bin/disjuntor.py:2344`, `bin/disjuntor.py:2365`). O novo teste exige apenas stdout vazio, sem conferir fotografia (`bin/tests/test_etapas_no_brief.sh:546`). |

Decisão aberta 1 tem efeito no B3, além da inflação do contador:

- **Falsa reprovação da fotografia antiga:** uma abertura wrapper legítima da v2, posterior ao carimbo I3 já existente, tem SNAPSHOT e ini sem tent. O novo `_SNAP_RE` exige tent (`bin/disjuntor.py:896`, `bin/disjuntor.py:898`), portanto descarta a fotografia. Como o ini é posterior ao carimbo, o final completo recebe SEM-FOTO (`bin/disjuntor.py:1086`, `bin/disjuntor.py:1135`, `bin/disjuntor.py:1143`).
- **Isenção não expira em 24 horas:** `_primeira_abertura` varre todos os ini, sem corte de janela (`bin/disjuntor.py:1086`, `bin/disjuntor.py:1089`). Ini anterior ao carimbo permite SEM-SNAPSHOT para final futuro sem fotografia (`bin/disjuntor.py:1135`). Uma reabertura DESLIGADA posterior também pode herdar essa isenção. Se isenção vitalícia por identidade da obra foi autorizada: **[nao verificado]**.
- **Mesmo segundo perde ordem real:** ini efetivamente anterior ao carimbo, mas persistido no mesmo segundo, falha no `< inst` e recebe SEM-FOTO (`bin/disjuntor.py:1135`, `bin/disjuntor.py:1143`). Persistência reduz instante a segundos (`bin/disjuntor.py:220`).
- O contador legado fica “sem censo” porque casamento exige tent (`bin/disjuntor.py:990`) e falta de casamento soma `sem` (`bin/disjuntor.py:2566`). A limitação de 24 h existe somente nesse medidor (`bin/disjuntor.py:2520`, `bin/disjuntor.py:2565`).

O selo concorrente não garante o cenário entre segundos nem sincroniza os oito leitores: usa relógios individuais, sleep fixo e conta linhas, descartando stderr e sem conferir rc individual (`bin/tests/test_etapas_no_brief.sh:471`, `bin/tests/test_etapas_no_brief.sh:473`, `bin/tests/test_etapas_no_brief.sh:474`, `bin/tests/test_etapas_no_brief.sh:475`). RED/GREEN executados e ocorrência dos contraexemplos em produção: **[nao verificado]**.

## 09/11 14:13 · sessao c07d957d350d · BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS · verificador · origem=Code

As curas específicas de 4, 7a e 7b estão na árvore. Porém, há um **contracaso W no cruzamento entre pendência preservada e irmã retratada**: a metade aceita deixa de ser reconhecida como espera, e C7 permite fechar sem aplicação (`app/chamados/juizes.py:2137`; `app/chamados/services/materializacao.py:1434`, `:112`; `app/chamados/models.py:1863`, `:1953`). Ocorrência em produção e execução desse contracaso: **[nao verificado]**.

| Item | Veredito | Classe | Evidência e contraexemplo concreto |
|---|---|---|---|
| **0 — triagem 4** | **NAO PROCEDE que continue aberto** | W corrigido | S127 agora consulta C7, preservando disputa vazia. O teste exige a recusa específica na transição real, antes da escrita (`app/chamados/juizes.py:2137`; `app/chamados/tests/test_bug_par_intervalo.py:468`). |
| **0 — triagem 7a** | **NAO PROCEDE que continue aberto, nos dois defeitos apontados** | W corrigido | Limpeza exclusiva de ESPERA e preservação do motivo com pendência estão implementadas. O cruzamento restante está no item 3 abaixo (`app/chamados/services/materializacao.py:1376`, `:1434`). |
| **0 — triagem 7b** | **NAO PROCEDE que falte o rótulo** | F | M38 está explicitamente rotulado estrutural; a diferença cobrada é chamada de método (`app/chamados/tests/test_bug_par_intervalo.py:815`, `:842`). |
| **1 — delegação S127** | **NAO PROCEDE duplicação restante em S127** | F | Consome C7 inteiro. Em snapshot estável, C7 não pode liberar quando o antigo `fio_vivo` travava, pois incorpora `not fio_vivo` (`app/chamados/juizes.py:2137`; `app/chamados/models.py:1863`). |
| **2 — recusa residual após aplicação** | **PROCEDENTE** | F | Falha conjunta grava recusa nas duas; reaceitar primeiro uma grava ESPERA nessa; reaceitar a segunda aplica ambas, mas conserva nela a recusa anterior (`app/chamados/services/materializacao.py:1392`, `:1435`, `:1376`). |
| **3 — pendência impede reconhecer espera** | **PROCEDENTE** | **W** | Pendência impede gravar ESPERA; motivo técnico impede o juiz de reconhecer espera. Retratada a irmã, C7 libera fechamento apesar da metade aceita sem aplicação (`app/chamados/services/materializacao.py:1434`, `:112`; `app/chamados/models.py:1863`, `:1953`). |
| **3 — limpeza posterior da pendência** | **NAO PROCEDE como caminho localizado** | F | O escritor encontrado grava valor não vazio; não localizei limpador runtime. Limpeza externa/dinâmica: **[nao verificado]** (`app/chamados/services/materializacao.py:937`). |
| **4 — outra recomposição de I19** | **NAO PROCEDE no censo da cópia** | F | Consulta única compartilhada pelo comando e contraprova; I18 e I6 são outras perguntas, com filtros diferentes (`app/chamados/management/commands/auditar_invariantes_chamados.py:549`, `:1118`, `:1081`, `:536`; `app/chamados/tests/test_bug_par_intervalo.py:503`). |
| **5 — campos com significados incompatíveis** | **PROCEDENTE** | W/F | A incompatibilidade interfere na retenção do fechamento acima; também produz apresentação “não plantou” quando o escritor efetivamente plantou pelo motivo (`app/chamados/services/materializacao.py:476`, `:1014`; `app/chamados/services/explicador.py:93`). |
| **6 — M38 estrutural** | **PROCEDENTE; rótulo verdadeiro** | F | Com três campos vazios, `descarimbar` retorna sem alterar estado ou salvar. M38 pula essa chamada; o teste diferencia pelas chamadas, não pelo resultado funcional (`app/chamados/models.py:3218`; `app/chamados/tests/mutantes_bug_par_intervalo.py:131`; `app/chamados/tests/test_bug_par_intervalo.py:842`). |

O **contracaso W** não precisa tomar a fixture como dado: os escritores permitem esta sequência condicional; sua ocorrência em produção permanece **[nao verificado]**.

1. Uma saída ainda avulsa não encontra tipo pelo marco. A derivação produz pendência e o ramo grava `pendencia_admin` junto do motivo (`app/chamados/services/materializacao.py:79`, `:388`, `:476`, `:937`).
2. Uma recusa posterior, por exemplo competência aprovada, retorna `None`; a saída normal do `atomic` preserva a pendência, enquanto a validação é devolvida. Isso dispensa competência exportada (`app/ponto/janelas.py:87`; `app/chamados/services/materializacao.py:1022`, `:1482`, `:1514`; `app/chamados/services/validacao.py:202`).
3. Nasce a irmã pela emissão; revalidar a primeira com a irmã ainda sem aceite conserva sua validação, pendência e recusa não-ESPERA, sem aplicar a batida (`app/chamados/services/disputa_emissao.py:834`; `app/chamados/services/materializacao.py:1434`; `app/chamados/services/validacao.py:473`).
4. A irmã sai do fato por retratação. A primeira morre no C6 por estar validada; a irmã morre pela identidade. A primeira também fica fora de `respostas_sem_veredito` e do juiz de espera (`app/chamados/models.py:2078`; `app/chamados/juizes.py:1153`, `:1164`; `app/chamados/models.py:1851`; `app/chamados/services/materializacao.py:112`).
5. **C7 libera.** `Disputa.fechar` passa pela consulta sob trava e salva o fechamento. `transicionar('fechado')` também passa: a guarda adicional busca somente perguntas não validadas, e a guarda de FURO é exclusiva de `resolvido` (`app/chamados/models.py:1863`, `:1927`, `:1953`, `:344`, `:181`, `:199`).

A diferença introduzida pela preservação é a **perda de retenção em C7**: gravar ESPERA conservaria a retenção depois da saída da irmã; preservar motivo técnico faz o juiz rejeitá-la. Não atribuo à troca S127 uma nova permissividade lógica: sua composição ficou mais restritiva (`app/chamados/services/materializacao.py:116`, `:112`, `:1434`; `app/chamados/models.py:1863`).

Os sete testes V5 têm a seguinte discriminação, por leitura das asserções; mortes executadas dos mutantes: **[nao verificado]**. [nao verificado]

| Teste em `app/chamados/tests/test_bug_par_intervalo.py` | Passaria sem a cura correspondente? | Onde morde |
|---|---|---|
| Fechado retido, `:472` | **Não** | Exige mensagem específica de S127 em `:468`; não basta qualquer bloqueio. M45 substitui justamente essa delegação (`app/chamados/tests/mutantes_bug_par_intervalo.py:157`). |
| Resolvido retido, `:475` | **Não** | A eventual recusa posterior por FURO não satisfaz a mensagem exigida em `:468` (`app/chamados/models.py:192`). M48: `app/chamados/tests/mutantes_bug_par_intervalo.py:167`. |
| Disputa vazia, `:478` | **Sim** | Controle de preservação: retorno da disputa e transição bloqueada em `:482`, `:484`. |
| Todas mortas, `:488` | **Sim** | Controle positivo: `None` em `:496` e status persistido em `:499`. |
| Pendência anterior, `:506` | **Não** | Sobrescrita antiga falha imediatamente na comparação do motivo em `:517`, antes de depender do I19. |
| Pendência nascida no plantio, `:527` | **Não** | Observa os campos produzidos no ramo e compara depois da aplicação em `:542`, `:552`. |
| Limpeza exclusiva, `:556` | **Não** | Limpeza ampla falha em `:565`; nunca limpar falha em `:566`. |

Esses testes cobrem separadamente pendência preservada e irmã retratada com ESPERA; não combinam pendência preservada com irmã retratada (`app/chamados/tests/test_bug_par_intervalo.py:459`, `:506`).

O censo AST, considerando importações com apelido e atribuições simples, encontrou estes quatro sítios de chamada executável; reflexão arbitrária e nomes montados dinamicamente: **[nao verificado]**.

| Chamador de `disputa_viva_do_chamado` | Uso do retorno |
|---|---|
| `app/chamados/models.py:154`, apelido `_disp_viva` importado em `:153` | Disputa retornada provoca `ValueError`; ausência permite prosseguir até `_aplicar` (`:155`, `:199`). `Chamado.fechar` chega por `transicionar` (`:454`). |
| `app/chamados/tests/test_disputa_s127.py:45` | Devolve resultado ao teste e cobra uma consulta C7 em `:47`. |
| `app/chamados/tests/test_bug_par_intervalo.py:482` | Cobra retorno da disputa vazia. |
| `app/chamados/tests/test_bug_par_intervalo.py:496` | Cobra `None`, seguido da transição real em `:497`. |

Inspeção de fonte e alvos de mutação não são chamadas ao juiz (`app/chamados/tests/test_pergunta_viva_no_guarda.py:37`; `app/chamados/tests/mutantes_bug_par_intervalo.py:157`, `:167`). Os caminhos automáticos localizados consultam C7 diretamente, inclusive reconciliador, materializador e beco (`app/chamados/reconciliador.py:411`, `:1170`; `app/chamados/services/materializacao.py:1534`; `app/chamados/services/becos.py:56`).

**C7 não é inteiramente puro nem barato por construção.** Carrega perguntas duas vezes; para cada pergunta elegível à espera, o identificador consulta irmãs e escala, formando padrão N+1. Contagem e latência reais: **[nao verificado]** (`app/chamados/models.py:1862`, `:1850`; `app/chamados/services/materializacao.py:114`, `:70`, `:87`). A cadeia C6 pode ler `date.today()`, já alcançável pelo `fio_vivo` antigo (`app/chamados/juizes.py:1217`, `:575`, `:1262`). Há memos em instância e um efeito persistente condicional: a leitura de toque humano chama `get_user_sistema`, implementado com `get_or_create`; ausência do usuário em produção: **[nao verificado]** (`app/chamados/juizes.py:862`, `:1087`; `app/core/utils.py:18`). Esses pontos são **F**, sem escrita errada ou efeito financeiro demonstrado pelo dado fornecido **[nao verificado]**.

Os valores de `recusa_motivo` não formam enumeração fechada: o campo aceita texto livre, e os escritores abaixo permitem resíduos anteriores à aplicação (`app/chamados/models.py:2577`).

| Valor/origem | Escritor e elegibilidade |
|---|---|
| Motivo técnico de tipo não conferido | Produzido na derivação e gravado com pendência (`app/chamados/services/materializacao.py:479`, `:937`). Deve sobreviver enquanto representar essa pendência; apagá-lo viola I19 (`app/chamados/management/commands/auditar_invariantes_chamados.py:549`). |
| Recusa de paridade, data, competência ou falha conjunta | `_carimbar_recusa` grava o texto; produtores incluem guarda de paridade, data e falha do par (`app/chamados/services/materializacao.py:1190`, `:1000`, `:1324`, `:1392`). |
| `Recusada por ...` | Recusa humana grava; resposta posterior normal não limpa automaticamente esse campo (`app/chamados/services/validacao.py:605`; `app/chamados/services/disputa_emissao.py:1483`, `:1505`). |
| `Colaborador respondeu ... exige decisao...` | Escape grava; resposta normal posterior pode preservar o texto (`app/chamados/services/disputa_emissao.py:1494`, `:1483`). |
| Descrição de duplicata | Porta de identidade grava, mas essa linha fica fora do universo do par (`app/chamados/services/fio.py:1350`; `app/chamados/services/materializacao.py:72`). |
| ESPERA | Envelope usa o escritor existente; aplicação limpa exclusivamente esse valor (`app/chamados/services/materializacao.py:1435`, `:1376`). |

Uma recusa histórica sem pendência pode, portanto, ficar obsoleta após sucesso; **não encontrei escrita posterior errada causada apenas por esse resíduo** nos leitores pedidos. Devolução ignora materializadas; validação usa erros do ato; vigia e passivo reconhecem lastro; flip exige a via `paridade_recusada`, diferente da aplicação normal; I18 exige não materializada (`app/chamados/services/validacao.py:190`, `:491`, `:511`; `app/chamados/services/vigia_carimbo.py:112`; `app/chamados/services/passivo_validado.py:329`; `app/ponto/services/flip_auto.py:128`; `app/chamados/services/materializacao.py:1283`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1081`). **F** para esse resíduo isolado; efeito adicional em produção: **[nao verificado]**.

Na apresentação, a metade técnica validada some da lista do detalhe porque não é C6 viva nem espera; o fio de disputa aberta carrega todas e mostra pendência/motivo no modal (`app/chamados/services/acoes_chamado.py:349`; `app/chamados/juizes.py:1153`; `app/chamados/services/fio.py:221`, `:225`; `app/templates/chamados/partials/modal_fio.html:222`). O explicador afirma “NAO plantou” antes de consultar lastro, embora o escritor possa plantar e conservar pendência; o cartão também diz conferir “antes de lançar”. Essa divergência é **F** (`app/chamados/services/explicador.py:93`, `:101`; `app/chamados/services/materializacao.py:1014`; `app/templates/chamados/partials/_card_atendimento.html:183`).

`o_que_segura` consulta exportação, dias retidos e a porta do dia; não lê diretamente pendência/motivo. A alimentação dessa porta usa C6, que exclui a metade validada. A frase resultante depende das demais portas daquele dia e é **[nao verificado]** para o dado fornecido (`app/folha/fora_da_folha.py:215`, `:221`, `:226`; `app/chamados/juizes.py:1322`, `:1153`).

Quanto às demais classes estruturais, as batidas continuam pela porta, os carimbos pela porta do modelo e as travas pelo adquirente comum em ordem Colaborador → Disputa → Chamado → Pergunta; não identifiquei outro escritor introduzido fora dessas portas nos caminhos examinados. Prova concorrente com duas conexões: **[nao verificado]** (`app/chamados/services/materializacao.py:1014`, `:1283`, `:134`; `app/chamados/models.py:2683`, `:2692`, `:2705`, `:2709`). A identidade e os instantes continuam consumindo suas fontes existentes, sem nova derivação paralela nesses trechos (`app/chamados/services/materializacao.py:87`, `:88`, `:1349`).

M38 satisfaz exatamente a alternativa estrutural solicitada: o rótulo existe, e o executável confirma a limitação. As asserções de estado/trilha desse cenário não constituem contracaso comportamental à guarda antiga (`app/chamados/tests/test_bug_par_intervalo.py:815`, `:842`, `:864`; `app/chamados/models.py:3218`).

## 09/11 14:11 · sessao d1463be4513a · - · - · origem=Ronald (tmux)

**Cruzamento PROCEDENTE, classe W, demonstrável estaticamente; ocorrência em produção [nao verificado].**

O estado `validada_em != None`, `materializada_em == None`, `pendencia_admin != ''`, `recusa_motivo` técnico pode nascer por portas reais:

1. Antes de existir a irmã, saída responde `12:00`; destino não tem marco do motivo, e escala 19–07 com pausa 01–02 deixa todos os marcos a mais de 240 minutos da resposta. `_tipo_pelo_marco` não acha tipo pela tolerância (`app/chamados/services/materializacao.py:374`–`:388`); `_ts_tipo_do_plantio` anexa `PEND_TIPO_NAO_CONFERIDO`, mas continua derivação (`:476`, `:493`).
2. O ramo grava os dois campos (`app/chamados/services/materializacao.py:937`–`:938`). Com competência **aprovada, sem precisar ser exportada**, `registrar_batida` devolve recusa (`app/ponto/janelas.py:87`, `app/ponto/registro_batida.py:139`).
3. O ramo retorna `None` (`app/chamados/services/materializacao.py:1022`–`:1027`); `_materializar_avulsa` retorna de dentro do `atomic` sem levantar (`:1482`–`:1483`), preservando a pendência. O `finally` grava o motivo da recusa (`:1514`–`:1515`); competência não é `COLISAO`, logo não ganha carimbo materializado (`:1181`, `:1189`). A validação é devolvida pela porta (`app/chamados/services/validacao.py:470`, `:202`–`:205`).
4. Nasce a irmã e o admin revalida a primeira com confirmação de implausibilidade. Com par incompleto, V5 pula o carimbo de espera por haver pendência (`app/chamados/services/materializacao.py:1434`); o ramo de par da validação não chama `devolver_validacao_recusada` (`app/chamados/services/validacao.py:473`–`:479`). Ficam aceite, pendência e recusa não-ESPERA, sem plantio.
5. A irmã sai por `remover_motivo`, que carimba identidade de retratação (`app/chamados/models.py:2071`–`:2079`). A primeira morre no degrau `validada_em` (`app/chamados/juizes.py:1153`); a irmã morre pela identidade (`:1164`). `respostas_sem_veredito` exclui ambas (`app/chamados/models.py:1851`), e o juiz de espera exclui a primeira por sua recusa técnica (`app/chamados/services/materializacao.py:112`). **C7 passa a devolver True** (`app/chamados/models.py:1863`).

O fechamento humano real passa S127 e chega à escrita: `transicionar('fechado')` chama S127 (`app/chamados/models.py:154`), a guarda adicional busca somente perguntas **não validadas** (`:344`), e aplica em `:199`, `:450`, salvando estado em `:296`. A primeira técnica não entra; a irmã retratada não está viva.

Há ainda efeito real de escrita da **disputa**: `Disputa.fechar` consulta C7 sob trava (`app/chamados/models.py:1927`) e salva fechamento em `:1953`. Se o pai estiver terminal, o beco classifica por C7 (`app/chamados/services/becos.py:31`–`:33`) e chama `d.fechar` em `:56`–`:57`; saves de pergunta agendam essa entrada em `app/chamados/signals.py:225`.

**Comparação com ac3/v4:** a mudança V5 de `if q.validada_em` para `if q.validada_em and not q.pendencia_admin` introduz a perda de retenção de C7: antes a primeira ganhava ESPERA, que continuava retendo após retratação da irmã (`app/chamados/services/materializacao.py:116`); agora preserva motivo técnico, que o mesmo juiz rejeita em `:112`. O S127 antigo já permitia o fechamento **humano** nesse cenário, pois ignorava toda espera; portanto esse ato humano não deve ser apresentado como regressão nova da troca S127. A liberação nova comprovável é **C7 permitir fechamento da disputa**, e V5 também deixa passar o mesmo cruzamento no S127 atual (`app/chamados/juizes.py:2137`).

Os testes existentes cobrem separadamente preservação técnica (`app/chamados/tests/test_bug_par_intervalo.py:508`) e irmã retratada com ESPERA (`:459`–`:465`); não combinam as condições. Seu GREEN não demonstra que esse cruzamento esteja fechado.

## 09/11 14:10 · sessao f88b05a1eeb9 · - · - · origem=Ronald (tmux)

Itens 2/3/5 — provas consolidadas:

| Ponto | Veredito/classe | Prova e contraexemplo |
|---|---|---|
| Recusa técnica deve sobreviver à aplicação | NÃO PROCEDE apagar indiscriminadamente, F | `_ts_tipo_do_plantio` sem marco acrescenta `tipo_nao_conferido` e texto dizendo **“batida plantada ... pelo MOTIVO”** (`app/chamados/services/materializacao.py:476`); ramo hora grava ambos (`:937`), pode plantar (`:1014`) e o par limpa exclusivamente ESPERA (`:1376`). Limpar motivo técnico deixaria `pendencia_admin` sem motivo, precisamente consulta I19 (`app/chamados/management/commands/auditar_invariantes_chamados.py:549`). |
| Recusa não técnica pode sobrar num par aplicado | PROCEDENTE, F | Falha escreve recusa nas duas e devolve validação (`materializacao.py:1392`). Após corrigir a causa, primeiro reaceite grava ESPERA só na primeira (`:1434`); segundo reaceite aplica ambas e limpa apenas ESPERA (`:1376`). A segunda conserva “intervalo recusado em conjunto”, embora aplicada. Ocorrência no dado prod: [nao verificado]. | [nao verificado]
| Leitores provocam escrita errada por essa recusa residual | NÃO PROCEDE demonstrado, F | Devolução retorna antes de ler motivo quando materializada (`app/chamados/services/validacao.py:190`); validação usa erros do ato (`:491`, `:511`) e retorna sucesso (`:538`). Vigia dá `com_registro` pelo lastro (`app/chamados/services/vigia_carimbo.py:112`); reparador pula essa classe (`app/chamados/services/passivo_validado.py:329`). Flip exige `via_resolucao=VIA_PARIDADE_RECUSADA` (`app/ponto/services/flip_auto.py:128`), enquanto aplicação usa a via pelo fato/declarou (`materializacao.py:1276`, `:1285`). I18 exige não materializada (`auditar_invariantes_chamados.py:1081`), I19 exige pendência (`:549`). |
| Pendência anterior tira metade da apresentação de espera | PROCEDENTE, F; há desdobramento W abaixo | Espera não é gravada quando há pendência (`materializacao.py:1434`) e juiz rejeita motivo técnico (`:112`). Detalhe inclui somente C6 viva ou espera (`app/chamados/services/acoes_chamado.py:349`), enquanto C6 mata validada (`app/chamados/juizes.py:1153`): a metade validada com pendência fica fora dessa lista. Fio aberto carrega todas as perguntas (`app/chamados/services/fio.py:221`, `:225`), portanto nela permanece visível com badge “Pendencia do admin” e motivo (`app/templates/chamados/partials/modal_fio.html:222`). |
| Pendência pode ser limpa depois sem regravar espera | NÃO PROCEDE na cópia, F | Único escritor runtime literal encontrado grava valor não vazio em `materializacao.py:937`; não há limpeza runtime localizada. Limpeza por SQL externo/dinâmico ou dado prod: [nao verificado]. |
| Semântica “não plantou” versus “plantada sem tipo confirmado” | PROCEDENTE, F | Escritor planta pelo motivo e acende pendência (`materializacao.py:476`, `:937`, `:1014`); display do modelo diz “plantada pelo motivo” (`app/chamados/models.py:2571`). Explicador prioriza pendência e afirma “sistema NAO plantou” antes de consultar lastro (`app/chamados/services/explicador.py:93`, `:101`); cartão diz conferir “antes de lançar” (`app/templates/chamados/partials/_card_atendimento.html:183`). Isso pode ocorrer inclusive após sucesso do par (`materializacao.py:1376`). |
| Pendência + irmã retratada libera fechamento com metade não aplicada | **PROCEDENTE condicional pelo código, W** | Há rota runtime abaixo; não foi executada e ocorrência em prod é [nao verificado]. | [nao verificado]

Contraexemplo W, sem depender de fixture:

1. Pergunta `intervalo_saida` está avulsa, sem irmã (`materializacao.py:79`); sem tipo identificável, ramo gera pendência (`:467`, `:476`) e grava `pendencia_admin`+motivo (`:937`).
2. Vizinha S posterior aciona paridade (`:962`); marco não aceso leva a `_carimbar_recusa` e `return None` (`:987`, `:1000`). `_materializar_avulsa` retorna normalmente do atomic (`:1482`), preservando pendência; validação devolve `validada_em` a nulo (`validacao.py:470`, `:202`).
3. Irmã passa a existir pela porta de emissão (`app/chamados/services/disputa_emissao.py:829`, `:834`, `:878`). Reaceitar a saída com irmã sem aceite chega a `materializacao.py:1434`, conserva pendência/recusa e deixa saída validada sem materialização.
4. Remoção do motivo da irmã retrata-a pela porta (`app/chamados/models.py:2071`, `:2078`); sobrando o motivo da saída, retorna sem fechar (`:2083`).
5. A saída morre no C6 por estar validada (`juizes.py:1153`); irmã retratada morre por identidade (`:1164`). Saída não entra em respostas sem veredito (`models.py:1851`), nem em espera por ter motivo técnico (`materializacao.py:112`). Logo C7 libera (`models.py:1863`).
6. S127 libera (`juizes.py:2137`); `transicionar('fechado')` supera guarda C6 ancorada (`models.py:165`), evita guarda de FURO que só vale para `resolvido` (`:181`) e chama `_aplicar` (`:199`). A guarda final também só retém C6 viva (`:224`), antes de gravar estado (`:228`). **Fecha chamado com metade aceita e não aplicada**. Na versão anterior do envelope, o primeiro reaceite sobrescreveria o motivo por ESPERA; após retratar irmã, fallback `materializacao.py:116` reteria C7. [nao verificado]

Escritores de `recusa_motivo` e valores alcançáveis:

- Motivo técnico: `materializacao.py:937`, derivado em `:479`.
- Espera, recusas de paridade/data/competência e falha conjunta: `_carimbar_recusa` em `:1190`; produtores em `:1000`, `:1312`, `:1392`, `:1417`, `:1435`, `:1515`.
- Recusa humana “Recusada por ...”: `validacao.py:605`; reabertura não limpa motivo (`disputa_emissao.py:1590`, `:1629`), resposta normal também não o limpa (`:1483`, `:1505`).
- Escapes “Colaborador respondeu ... exige decisao”: `disputa_emissao.py:1494`; resposta normal posterior pode preservar esse texto (`:1483`).
- Duplicata escreve descrição apontando canônica (`fio.py:1350`), mas linha fica fora do fato e é excluída da identidade do par (`materializacao.py:72`).
- Porta do modelo grava motivo fornecido (`models.py:3196`); limpeza do par está em `materializacao.py:1377`.

`o_que_segura` não lê diretamente esses campos: usa exportação e dias retidos (`app/folha/fora_da_folha.py:215`), lê `porta_alcancavel_do_dia` (`:65`, `:221`) e traduz fio por DESTRAVA (`:226`). Alimentação da porta considera C6 (`app/chamados/juizes.py:1322`), não espera do par; metade validada não participa desse conjunto (`:1153`). Resultado concreto do dado prod nessa ferramenta: [nao verificado].

## 09/11 14:08 · sessao d1463be4513a · - · - · origem=Ronald (tmux)

**Item 1 — NÃO PROCEDE que S127 ainda duplique C7; PROCEDENTE custo/leitura indireta de relógio e efeito colateral condicional, classe F.** S127 consulta C7 e preserva exceção vazia em `app/chamados/juizes.py:2134` e `:2137`; C7 incorpora `respostas_sem_veredito`, `fio_vivo` e espera em `app/chamados/models.py:1862`.

Censo AST de todos os `.py`, com importações `as` e propagação de atribuição simples de função:

| Sítio executável | Consumo do retorno |
|---|---|
| `app/chamados/models.py:153`, chamada `:154`, alias `_disp_viva` | `transicionar` levanta `ValueError` se recebeu disputa (`:155`); sem bloqueio chega a `_aplicar` em `:199`, que chama `_gravar_estado` em `:450`; escreve estado em `:228` e salva no corpo seguinte. |
| `app/chamados/tests/test_disputa_s127.py:45` | Guarda resultado e o devolve após cobrar consulta C7 em `:47`. |
| `app/chamados/tests/test_bug_par_intervalo.py:482` | Cobra disputa sem perguntas retornada. |
| `app/chamados/tests/test_bug_par_intervalo.py:496` | Cobra `None` quando todas morreram sem resposta pendente; depois transiciona realmente em `:497`. |

Referências não executáveis à função: `inspect.getsource` em `app/chamados/tests/test_pergunta_viva_no_guarda.py:37`; nomes dos alvos mutantes em `app/chamados/tests/mutantes_bug_par_intervalo.py:157` e `:167`; referência histórica em docstring de `app/chamados/tests/test_o261_fio_tem_uma_porta.py:25`. Acessos construídos dinamicamente por strings e reflexão arbitrária: **[nao verificado]**.

**Comparação de liberação:** em um mesmo snapshot, o novo bloqueio é mais restritivo: C7 só é verdadeiro com `not fio_vivo(perguntas)`, além das demais condições (`app/chamados/models.py:1863`). Portanto não há caso lógico estável em que C7 libere com o S127 antigo travando; concorrência entre consultas distintas: **[nao verificado]**. O contrário existe: saída validada em espera e volta retratada fazem ambas morrerem pelo C6, mas C7 retém a espera; cenário e asserções em `app/chamados/tests/test_bug_par_intervalo.py:459`, `:463`, `:465`. Isso alcança escrita por `transicionar('fechado'/'resolvido')`, bloqueada pela mensagem específica de S127 em `:468`; os wrappers estão em `:472` e `:475`.

**Disputa vazia continua bloqueando:** curto-circuito `not disputa.perguntas.exists()` em `app/chamados/juizes.py:2137`; teste chama a transição real e verifica estado/trilha intactos em `app/chamados/tests/test_bug_par_intervalo.py:480`, `:484`, `:486`. Este contracaso passaria também com a regra antiga; protege preservação, não demonstra a cura.

**Custo:** há consulta adicional `exists()` em `app/chamados/juizes.py:2137`, e C7 lê perguntas duas vezes, uma por `list(self.perguntas.all())` e outra em `respostas_sem_veredito()` (`app/chamados/models.py:1862`, `:1850`). Além disso, para cada pergunta elegível, a espera chama identificador (`app/chamados/services/materializacao.py:114`), que consulta irmãs em `:70` e escala em `:87`; o carregador consulta célula em `app/escala/servico_jornada.py:46`, e `pausas_do_dia` pode buscar novamente em `app/escala/models.py:1187`. Existe padrão N+1; quantidade real de queries/latência em produção: **[nao verificado]**.

**Efeitos colaterais:** C7 não contém `save/update`, mas sua cadeia não é inteiramente pura: C6 grava memo na instância em `app/chamados/juizes.py:862`; tranca grava memo em `:996` e `:1066`. `respostas_sem_veredito` pode alcançar `dia_da_pergunta_trancado` (`app/chamados/models.py:1854`), `_admin_tocou_o_dia_depois` (`app/chamados/juizes.py:971`), `_toque_humano_no_dia` (`:1064`) e `get_user_sistema` (`:1087`), cujo código é `User.objects.get_or_create` em `app/core/utils.py:18`. Pode criar usuário ausente; ausência desse usuário em produção e escrita errada: **[nao verificado]**. Em falhas engolidas, há contador em memória e log em `app/core/observ.py:75` e `:79`.

**Relógio:** C7 pode alcançar o C6 e `_dentro_da_janela_viva` (`app/chamados/juizes.py:1217`), que chama `date.today()` em `:575`. A cadeia já existia no S127 antigo por `fio_vivo`; não é leitura nova demonstrada da volta 5 (`app/chamados/juizes.py:1262`). Não afirmar “zero relógio”.

**Cron:** não apareceu chamada direta de cron à função no censo AST; os sítios automáticos relevantes consultam C7 diretamente em `app/chamados/reconciliador.py:411`, `:611`, `:1170` e `app/chamados/services/materializacao.py:672`, `:1534`. O único chamador de produção de S127 é `transicionar`; `Chamado.fechar` delega a ele em `app/chamados/models.py:454`. Escrita automática que passa por `_gravar_estado` sem `transicionar` não ganha automaticamente esse bloqueio S127, pois o gate mostrado é local a `transicionar` (`app/chamados/models.py:148`).

**Item 5 — juiz duplicado:** a recomposição antiga foi eliminada de S127 em favor de C7 (`app/chamados/juizes.py:2137`); fio continua reduzindo exclusivamente por C6 em `:1262`. Não há novo escritor de status nesta cura: o gate lança erro e a escrita continua em `_aplicar`/`_gravar_estado` (`app/chamados/models.py:155`, `:199`, `:450`). O risco de custo e relógio acima é **F**; ocorrência concreta de estado errado ou efeito exportada/dinheiro por essa troca: **[nao verificado]**.

## 09/11 14:08 · sessao 724a79eb995d · - · - · origem=Ronald (tmux)

| Item | Veredito | Classe | Evidência e contracaso |
|---|---|---|---|
| 0 / triagem 4 | **NAO PROCEDE que continue aberto.** | W corrigido | S127 consulta `not disputa.pode_fechar()` e preserva vazio em `app/chamados/juizes.py:2137`; `transicionar` aborta antes de `_aplicar` em `app/chamados/models.py:154` e `:199`. Contracaso discriminante: metade validada + irmã retratada, C6 sem vivas e C7 falso, em `app/chamados/tests/test_bug_par_intervalo.py:459`. |
| 0 / triagem 7a | **NAO PROCEDE que continue aberto.** | W corrigido | Limpa somente espera em `app/chamados/services/materializacao.py:1376`; não sobrescreve motivo com pendência em `:1434`. Motivo técnico nasce conjuntamente com pendência em `:937`. |
| 0 / triagem 7b | **NAO PROCEDE que falte o rótulo.** | F | O próprio teste rotula M38 estrutural em `app/chamados/tests/test_bug_par_intervalo.py:815`; a limitação é confirmada pelas operações executáveis, abaixo. |
| 4 | **NAO PROCEDE derivação paralela de I19 encontrada.** | F | Consulta única `app/chamados/management/commands/auditar_invariantes_chamados.py:549`, consumida pelo comando em `:1118` e contraprova em `app/chamados/tests/test_bug_par_intervalo.py:503`. Censo AST de palavras-chave `pendencia_admin` em chamadas Python encontrou apenas essa consulta e a fixture `:513`; acessos dinâmicos [nao verificado]. |
| 6 | **PROCEDENTE: M38 segue estrutural e o rótulo é verdadeiro.** | F | Fixture exige três campos vazios em `app/chamados/tests/test_bug_par_intervalo.py:827`; nessa condição `descarimbar` retorna antes de alterar campos, cache ou salvar, em `app/chamados/models.py:3218`. M38 acrescenta o curto-circuito pelo carimbo em `app/chamados/tests/mutantes_bug_par_intervalo.py:134`. A diferença cobrada é a lista de chamadas em `app/chamados/tests/test_bug_par_intervalo.py:842`, repetida em `:859`; asserções de estado/trilha não distinguem as implementações nesse cenário. |

Discriminação dos sete testes V5, inferida do código; mortes executadas dos mutantes **[nao verificado]**: [nao verificado]

| Teste em `app/chamados/tests/test_bug_par_intervalo.py` | Passaria sem a cura correspondente? | Asserção discriminante |
|---|---|---|
| `:472`, fechado retido | **Não.** | `assertRaisesRegex` em `:468`: S127 antigo libera o caminho sem vivas; novo aborta com mensagem de disputa. Guarda real em `app/chamados/models.py:155`. M45 em `app/chamados/tests/mutantes_bug_par_intervalo.py:157`. |
| `:475`, resolvido retido | **Não.** | Mesma asserção `:468`; eventual recusa posterior por FURO (`app/chamados/models.py:181`, `:192`) não satisfaz o texto exigido. M48 em `app/chamados/tests/mutantes_bug_par_intervalo.py:167`. |
| `:478`, disputa vazia | **Sim**, relativamente à troca S127. É controle de preservação. | C7 verdadeiro em `:481`, S127 retorna disputa em `:482`, transição bloqueada em `:484`. |
| `:488`, todas mortas | **Sim**, relativamente à troca S127. É controle positivo de liberação. | Retorno `None` em `:496` e status persistido fechado em `:499`. |
| `:506`, pendência anterior | **Não.** | A sobrescrita antiga falha imediatamente na comparação do motivo em `:517`; portanto não depende apenas de I19. Preservação após aplicação também cobrada em `:523`. M47 em `app/chamados/tests/mutantes_bug_par_intervalo.py:164`. |
| `:527`, pendência nascida no plantio | **Não.** | Captura os dois campos reais produzidos durante o plantio em `:542`, comprova origem em `:549`/`:550`, compara após aplicação em `:552`. Limpeza ampla apaga o motivo e falha nessa comparação. M46 em `app/chamados/tests/mutantes_bug_par_intervalo.py:161`. |
| `:556`, limpeza exclusiva | **Não** com limpeza ampla antiga; também não com limpeza desativada. | Limpeza ampla falha em `:565` (outro motivo preservado); nunca limpar falha em `:566` (espera vazia). M49 cobre a segunda mutação em `app/chamados/tests/mutantes_bug_par_intervalo.py:171`. |

O selo estrutural S127 exige chamada a `pode_fechar`, proíbe recomposição nominal de C6/espera e confere a cadeia C7→C6 em `app/chamados/tests/test_pergunta_viva_no_guarda.py:39`. O teste com dublê cobra efetivamente uma chamada C7 em `app/chamados/tests/test_disputa_s127.py:47`; os casos funcionais V5 acima sustentam o comportamento real em `app/chamados/tests/test_bug_par_intervalo.py:469`.

## 09/11 14:06 · sessao 8e2f0f5eb602 · CODEX-2-RAIAS-PORTAO-CPU · verificador · origem=Code

**Há ressalvas PROCEDENTES sobre limpeza após SIGKILL, semântica de `--toca` e medição da ocupação da trava.** A união dos testes não apresenta perda aparente nem nomes duplicados; a morte efetiva dos mutantes permanece **[nao verificado]**. Evidências abaixo. [nao verificado]

“PROCEDENTE” indica achado; “NAO PROCEDE” descarta a acusação no trecho examinado.

| Item | Parecer | Evidência |
|---|---|---|
| **1. Corrida na admissão** | **NAO PROCEDE entre wrappers novos.** Ordem: meta → prender própria vaga → consultar outra → portões → publicar estado → soltar meta. | `bin/codex_pergunta.sh:378`, `bin/codex_pergunta.sh:381`, `bin/codex_pergunta.sh:388`, `bin/codex_pergunta.sh:390` |
| Limpeza em rc3/timeout | **NAO PROCEDE falha no caminho normal:** retorno do `timeout` segue pelo inventário e pelo EXIT; inventário irregular vence com rc3. | `bin/codex_pergunta.sh:549`, `bin/codex_pergunta.sh:558`, `bin/codex_pergunta.sh:585`, `bin/codex_pergunta.sh:607` |
| Limpeza após SIGKILL | **PROCEDENTE:** remoção depende exclusivamente de EXIT; `.estado` pode ficar órfão. Estado sozinho não prende vaga: vida é julgada pelo flock. FD6 segue aberto na chamada dos filhos; duração efetiva dessa retenção **[nao verificado]**. | `bin/codex_pergunta.sh:236`, `bin/codex_pergunta.sh:240`, `bin/codex_pergunta.sh:254`, `bin/codex_pergunta.sh:539` |
| Limpeza sob meta | **PROCEDENTE como recusa conservadora:** remoção ocorre fora da meta; outra admissão pode encontrar flock ainda ocupado e estado já ausente, recusando. | `bin/codex_pergunta.sh:236`, `bin/codex_pergunta.sh:294` |
| **2. Fonte e limite do load5** | **NAO PROCEDE fonte diferente:** `/proc/loadavg`, segundo campo, exige estritamente `< nproc-2`. | `bin/codex_pergunta.sh:118`, `bin/codex_pergunta.sh:295`, `bin/codex_pergunta.sh:296` |
| “Na abertura” | Medido durante a admissão. **PROCEDENTE se significa início efetivo do CLI:** depois existem desligador, fila de slots e trava da resposta; a revalidação não relê load/hora. | `bin/codex_pergunta.sh:301`, `bin/codex_pergunta.sh:483`, `bin/codex_pergunta.sh:510`, `bin/codex_pergunta.sh:528` |
| Janela | Implementa **[06:00,07:30)**: 06:00 recusa; 07:30 aceita. Selo cobra esses extremos. Inclusividade pretendida pelo aval: **[nao verificado]**. | `bin/codex_pergunta.sh:298`, `bin/codex_pergunta.sh:299`, `bin/tests/test_codex_pergunta.sh:524` |
| Desligador ilegível | **NAO PROCEDE abertura:** exceção retorna2; wrapper recusa qualquer nãozero. Média `>7` ou uso `>85` com reset futuro desliga; igualdade passa. | `bin/codex_vagas_medida.py:85`, `bin/codex_vagas_medida.py:91`, `bin/codex_vagas_medida.py:94`, `bin/codex_pergunta.sh:301` |
| Terceira chamada | **NAO PROCEDE ultrapassar teto:** ambos flocks ocupados causam rc2/VAGA2. | `bin/codex_pergunta.sh:381`, `bin/codex_pergunta.sh:384`, `bin/codex_pergunta.sh:250` |
| **3. Obrigatoriedade e cobrança de `--toca`** | **NAO PROCEDE ausência de guarda:** obrigatório no construtor; TOCADO fora do próprio alcance ou cruzando o da outra viva produz rc3. | `bin/codex_pergunta.sh:319`, `bin/codex_pergunta.sh:282`, `bin/codex_pergunta.sh:285`, `bin/codex_pergunta.sh:572` |
| Arquivo × prefixo | **PROCEDENTE se sem `/` significa arquivo exato:** `rstrip("/")` apaga a distinção; `app/folha` e `app/folha/` cobrem descendentes igualmente, embora a mensagem diga “pasta termina em /”. **NAO PROCEDE se todo argumento é prefixo por componente:** nessa interpretação o comparador é consistente e não confunde `folha` com `folhas`. | `bin/codex_pergunta.sh:262`, `bin/codex_pergunta.sh:263`, `bin/codex_pergunta.sh:319` |
| **4. Protegidos e cabeçalho** | **NAO PROCEDE segundo juiz/lista operacional:** PROTEGIDOS alimenta o inventário; cabeçalho extrai texto dessa mesma fonte. | `bin/codex_pergunta.sh:157`, `bin/codex_pergunta.sh:162`, `bin/codex_pergunta.sh:242` |
| Writable roots e Claude | **NAO PROCEDE liberação geral:** root extra `<wt>/bin` somente com `--toca bin/...`; `.claude/` é proibido. Entradas executáveis configuradas estão na lista protegida; fecho de imports foi deliberadamente excluído. | `bin/codex_pergunta.sh:537`, `bin/codex_pergunta.sh:541`, `bin/codex_pergunta.sh:147`, `bin/codex_pergunta.sh:156`; `.claude/settings.json:9`, `.claude/settings.json:20`, `.claude/settings.json:44`, `.claude/settings.json:68`, `.claude/settings.json:76` |
| **6. Histórico e crescimento** | **NAO PROCEDE segundo escritor operacional:** append centralizado após flock. Crescimento sem limite nesse código; leitor percorre tudo. Rotação externa **[nao verificado]**. | `bin/trava_teste.sh:27`, `bin/trava_teste.sh:40`, `bin/trava_teste.sh:45`, `bin/codex_vagas_medida.py:107` |
| Ocupação medida | **PROCEDENTE:** trava é global, histórico é por raiz da cópia; outras worktrees podem ficar fora da medida. `ini` órfão conta até agora, mesmo sem confirmação de flock; falha de append é ignorada. | `bin/trava_teste.sh:21`, `bin/trava_teste.sh:26`, `bin/trava_teste.sh:27`, `bin/codex_vagas_medida.py:103`, `bin/codex_vagas_medida.py:124` |

**5. Leitores de `chamadas.log`: NAO PROCEDE interpretar `vaga=N` como rc/duração nos leitores encontrados.**

| Leitor | Posição usada / evidência |
|---|---|
| `codex_limite_estado.py` | Sessão=`p[6]`, rc=`p[7]`, duração=`p[8]`; aceita campos adicionais. `bin/codex_limite_estado.py:66`, `bin/codex_limite_estado.py:67` |
| `gerar_codex_estado.py` / `gerar_estado.py` | Delegam ao leitor anterior. `bin/gerar_codex_estado.py:176`, `bin/gerar_estado.py:269` |
| `disjuntor.py` | Delega contagem, tokens e inícios à mesma autoridade. `bin/disjuntor.py:2023`, `bin/disjuntor.py:2031`, `bin/disjuntor.py:2121` |
| Selos posicionais | Sonda usa `$8`/`$9`; pergunta usa `$7`/`$8`. Último campo é usado explicitamente como vaga. `bin/tests/test_codex_sonda.sh:98`, `bin/tests/test_codex_pergunta.sh:335`, `bin/tests/test_codex_pergunta.sh:495` |
| Nova medida | Usa `p[-1]` explicitamente como `vaga=2`. Portanto o comentário antigo “leitor único” ficou desatualizado. `bin/codex_vagas_medida.py:139`, `bin/gerar_codex_estado.py:41` |

| **7. União e selos** | Parecer |
|---|---|
| Perdeu caso / duplicou nome | **NAO PROCEDE na comparação estática com o diff fornecido:** 90 casos/funções, sem duplicação; fake `--censo`, casos I1 e novos v2/toca/bin coexistem. Antigo `p_bin` foi substituído pela permissão e proteção seletivas. Comparação integral com ancestrais **[nao verificado]**. `bin/tests/test_codex_pergunta.sh:82`, `bin/tests/test_codex_pergunta.sh:248`, `bin/tests/test_codex_pergunta.sh:575`, `bin/tests/test_codex_pergunta.sh:601` |
| Mutante sem mordida | **NAO PROCEDE ausência estática:** 93 entradas, sem linha duplicada, com casos existentes. Há cobrança específica de rc/motivo/não chamada; PROIBIDO; VAGA2/log; protegidos individuais. `bin/tests/test_codex_pergunta.sh:163`, `bin/tests/test_codex_pergunta.sh:169`, `bin/tests/test_codex_pergunta.sh:498`, `bin/tests/test_codex_pergunta.sh:580`, `bin/tests/test_codex_pergunta.sh:630` |
| Cada morte foi específica? | **PROCEDENTE a ressalva:** controlador aceita qualquer texto `FALHA`, sem exigir a asserção esperada. Execução e morte efetiva dos 93: **[nao verificado]**. `bin/tests/test_codex_pergunta.sh:740` |
| `CODEX_TESTE_*` fora do selo | **NAO PROCEDE uso dentro de Git ou com vivo de produção:** recusa antes de consumir fontes falsas. Porém a guarda reconhece topologia, não identidade do selo; qualquer cópia fora de Git com vivo fora de produção satisfaz essa condição. Selo testa apenas o ramo Git. `bin/codex_pergunta.sh:112`, `bin/codex_pergunta.sh:118`, `bin/tests/test_codex_pergunta.sh:588` |

| **8. Estado novo** | Quem escreve | Quem lê |
|---|---|---|
| `construtor.vaga2` | Wrapper abre/prende; `_viva` também abre para sondagem. `bin/codex_pergunta.sh:383`, `bin/codex_pergunta.sh:254` | flock na admissão/fim. `bin/codex_pergunta.sh:388`, `bin/codex_pergunta.sh:564` |
| `construtor.vaga{1,2}.estado` | Wrapper publica por rename e remove no EXIT; selo fabrica estados isolados. `bin/codex_pergunta.sh:309`, `bin/codex_pergunta.sh:236`, `bin/tests/test_codex_pergunta.sh:484` | `_toca_de`, abertura/fim; selo verifica ciclo. `bin/codex_pergunta.sh:289`, `bin/codex_pergunta.sh:293`, `bin/codex_pergunta.sh:565`, `bin/tests/test_codex_pergunta.sh:510` |
| `vaga2.log` | `_recusa_v2`. `bin/codex_pergunta.sh:249` | Medida e selo. `bin/codex_vagas_medida.py:141`, `bin/tests/test_codex_pergunta.sh:491` |
| `trava_teste.hist` | `_hist`; selo escreve somente histórico isolado. `bin/trava_teste.sh:27`, `bin/tests/test_trava_teste.sh:103`, `bin/tests/test_trava_teste.sh:142` | Medida e selo isolado. `bin/codex_vagas_medida.py:107`, `bin/tests/test_trava_teste.sh:145` |

| Classe anteriormente reprovadora | Resultado |
|---|---|
| Segundo juiz que duplica registrado | **NAO PROCEDE nos sítios examinados:** inventário único; leitura semanal reutilizada. `bin/codex_pergunta.sh:136`, `bin/codex_vagas_medida.py:75` |
| Segundo escritor fora da porta | **NAO PROCEDE nos estados novos:** escritores identificados acima; nova medida apenas lê cota. `bin/codex_vagas_medida.py:148`, `bin/statusline_cota.py:54` |
| Relógio fora da borda | **NAO PROCEDE na função do desligador:** recebe `agora` do CLI. Ressalva de envelhecimento antes da execução permanece. `bin/codex_vagas_medida.py:83`, `bin/codex_vagas_medida.py:201`, `bin/codex_pergunta.sh:298` |
| Derivação paralela de fonte única | **NAO PROCEDE para quota/protegidos:** delegação e extração da fonte original. **PROCEDENTE fragmentação da medida global da trava.** `bin/codex_vagas_medida.py:76`, `bin/codex_pergunta.sh:242`, `bin/trava_teste.sh:26` |
| Selo sem mordida | **PROCEDENTE a fragilidade do critério de morte**, sem prova de mutante sobrevivente. `bin/tests/test_codex_pergunta.sh:740` |
| Ordem de trava | **NAO PROCEDE deadlock demonstrado:** admissão usa tentativas não bloqueantes das vagas; espera pela meta é limitada. `bin/codex_pergunta.sh:379`, `bin/codex_pergunta.sh:381`, `bin/codex_pergunta.sh:384`, `bin/codex_pergunta.sh:563` |
| Campo com dois significados | **PROCEDENTE sob interpretação arquivo exato; NAO PROCEDE sob prefixo por componente.** `bin/codex_pergunta.sh:262`, `bin/codex_pergunta.sh:319` |

## 09/11 14:04 · sessao 8eb5063e6871 · - · - · origem=Ronald (tmux)

| Item | Resultado fundamentado |
|---|---|
| Corrida na admissão | **NAO PROCEDE entre wrappers novos**: META é presa antes das vagas, cobre leitura da outra, portões e publicação; solta depois. Ordem: META → vaga; nenhuma espera bloqueante pela vaga. `bin/codex_pergunta.sh:378`, `:381`, `:388`, `:390`, `:391`. |
| Limpeza universal / SIGKILL | **PROCEDENTE**: só há `trap EXIT`; remoção depende dele. SIGKILL deixa `.estado` órfão; não existe recuperação explícita, apenas substituição na publicação seguinte. `bin/codex_pergunta.sh:236`, `:240`, `:309`. FD6 permanece aberto na chamada de timeout/Codex; sobrevivência efetiva em descendentes [nao verificado]. `:380`, `:539`. |
| Limpeza concorrente | **PROCEDENTE como recusa conservadora**: `_saida` remove estado fora da META; outra chamada pode observar flock ocupado e estado ausente, recusando. `bin/codex_pergunta.sh:236`, `:254`, `:294`. Não demonstra terceira vaga. `:384`. |
| rc3 / timeout | Caminhos normais passam pelo inventário e EXIT; inventário irregular vence o rc original com rc3. `bin/codex_pergunta.sh:549`, `:558`, `:585`, `:607`, `:240`. Não cobre matar o próprio wrapper. `:240`. |
| Load / relógio | Fonte real `/proc/loadavg`, campo 2; exige `< nproc-2`. `bin/codex_pergunta.sh:118`, `:295`, `:296`. **PROCEDENTE quanto a envelhecimento antes do início efetivo**: leitura ocorre antes de desligador, fila global e trava da resposta; revalidação não relê portões. `:298`, `:301`, `:483`, `:510`, `:528`. Se “abertura” significa a admissão da vaga, a leitura está nessa borda. `:388`. |
| Janela | Implementa **[06:00,07:30)**: 06:00 proibida; 07:30 permitida. `bin/codex_pergunta.sh:299`. Selo cobra 0600/0729 contra 0559/0730. `bin/tests/test_codex_pergunta.sh:524`. Inclusividade exigida pelo aval [nao verificado]. |
| Desligador | Fonte ilegível retorna2 e wrapper recusa qualquer nãozero. `bin/codex_vagas_medida.py:85`, `:88`; `bin/codex_pergunta.sh:301`. Limites: média `>7`, used `>85` e reset futuro; igualdade aceita. `bin/codex_vagas_medida.py:91`, `:94`. |
| Teto | Terceira chamada recusa quando ambos flocks ocupados, rc2/VAGA2. `bin/codex_pergunta.sh:381`, `:384`, `:250`. |
| `--toca` obrigatório / rc3 | Obrigatório e validado no construtor. `bin/codex_pergunta.sh:319`, `:321`. Fora do próprio alcance ou cruzando o da outra viva gera PROIBIDO e rc3. `:282`, `:285`, `:572`, `:585`. |
| Campo com dois significados | **PROCEDENTE se ausência de `/` significa arquivo exato**: mensagem diz “pasta termina em /”, porém `rstrip("/")` torna `app/folha` e `app/folha/` equivalentes; ambos cobrem descendentes. `bin/codex_pergunta.sh:319`, `:262`, `:263`. **NAO PROCEDE se todo argumento significa prefixo por componente**: implementação consistente nessa interpretação, sem prefixo textual indevido `folha`→`folhas`. `:263`, `:265`. |
| Protegidos / cabeçalho | **NAO PROCEDE duplicação do juiz**: lista em PROTEGIDOS alimenta o inventário; cabeçalho extrai texto da própria fonte. `bin/codex_pergunta.sh:157`, `:162`, `:242`, `:465`. |
| Writable roots / hooks | `bin` extra só com argumento começando `bin/`; diretório de trabalho continua `app`. `bin/codex_pergunta.sh:537`, `:541`, `:543`. `.claude/` é proibido pelo inventário; raiz `.claude` não é writable root. `:147`, `:537`. Entradas configuradas estão cobertas pela lista: `.claude/settings.json:9`, `:20`, `:44`, `:68`, `:76`; `bin/codex_pergunta.sh:158`. Fecho de imports deliberadamente excluído; proteção integral das dependências [nao verificado]. `:156`. |

Ressalva: a META só serializa participantes que a respeitam. O comentário admite wrapper velho (`bin/codex_pergunta.sh:87`), cujo trecho anterior fornecido no diff não toma META; no misto, um velho pode ocupar vaga1 depois de o novo, na vaga2, observar vaga1 livre. A segurança do portão nesse interleaving [nao verificado]; o teto de dois flocks permanece. `bin/codex_pergunta.sh:388`, `:384`.

## 09/11 14:04 · sessao 6f45d6f77543 · - · - · origem=Ronald (tmux)

| Item | Resultado e prova |
|---|---|
| 5 — `vaga=N` quebra leitor? | **NAO PROCEDE.** `codex_limite_estado.py:56,63-67` lê thread/rc/duração por `p[6]/p[7]/p[8]`; `gerar_codex_estado.py:176` delega a ele; `disjuntor.py:2023,2031,2121` também delega. `gerar_estado.py:269-270` usa a mesma autoridade. |
| 5 — selos posicionais | **NAO PROCEDE.** `test_codex_sonda.sh:97-98` usa `$8` como rc e `$9` como duração; `test_codex_pergunta.sh:261,335` usa posições fixas. O último campo é interpretado explicitamente como vaga em `test_codex_pergunta.sh:495` e `codex_vagas_medida.py:139`. Nenhum desses o interpreta como rc/duração. |
| 6 — escritor único | **NAO PROCEDE** a acusação de segundo escritor de produção: `trava_teste.sh:27` concentra append; chamadas em `:45-46`, depois do flock `:40`. O selo escreve histórico isolado: `test_trava_teste.sh:103,142-145`. |
| 6 — ocupação global incompleta | **PROCEDENTE.** Trava global fixa em `/tmp` (`trava_teste.sh:21`), histórico por raiz da cópia (`:26`), leitor só da própria raiz (`codex_vagas_medida.py:103`). Runs por outras worktrees podem ficar fora dessa medida. |
| 6 — histórico órfão infla ocupação | **PROCEDENTE.** `ini` sem `fim` conta até agora (`codex_vagas_medida.py:124-128`); append falho é ignorado (`trava_teste.sh:27`) e limpeza depende de EXIT (`:46`). Não há reconciliação com flock nesse leitor (`codex_vagas_medida.py:101-129`). |
| 6 — rotação/crescimento | Append sem limite em `trava_teste.sh:27`; leitor percorre tudo em `codex_vagas_medida.py:107-108`. Rotação externa: **[nao verificado]**. |
| Fonte semanal paralela | **NAO PROCEDE.** A nova medida chama `codex_limite_estado.leituras_semanais` (`codex_vagas_medida.py:75-79`), também usada pelo orçamento (`codex_limite_estado.py:169`; `disjuntor.py:1451-1452`). Claude é somente leitura de fotos (`codex_vagas_medida.py:148-154`); escritor segue `statusline_cota.py:49-54`. |

| Estado novo | Quem escreve | Quem lê |
|---|---|---|
| `construtor.vaga2` | Wrapper abre em append, prende flock: `codex_pergunta.sh:383-384`; `_viva` também abre para sondar `:254`. | `_viva`: `:255`; admissão: `:388`; fim: `:564`. Selo: `test_codex_pergunta.sh:487`. |
| `construtor.vaga{1,2}.estado` | Wrapper publica por temporário+rename: `codex_pergunta.sh:306-310`; apaga no EXIT `:236`. Selo fabrica estados: `test_codex_pergunta.sh:484,488`. | `_toca_de`: `codex_pergunta.sh:289`, chamado na abertura `:293` e fim `:565`; selo verifica publicação/remoção `test_codex_pergunta.sh:510-517`. |
| `vaga2.log` | `codex_pergunta.sh:249`. Diretório já criado antes da admissão: `:361`. | Medida: `codex_vagas_medida.py:141-143`; selo: `test_codex_pergunta.sh:491,503-504,521`. |
| `trava_teste.hist` | `trava_teste.sh:27,45-46`. | Medida: `codex_vagas_medida.py:103-129`; selo isolado: `test_trava_teste.sh:145`. |

Observação: a nova medida faz leitura própria de `chamadas.log` (`codex_vagas_medida.py:135-140`), portanto o comentário “leitor único” de `gerar_codex_estado.py:41` deixou de descrever todos os leitores. Ela conta a pergunta nova `vaga=2`; não encontrei duplicação do julgamento semanal, que continua delegado (`codex_vagas_medida.py:75-79`).

## 09/11 14:03 · sessao a17614b1120f · - · - · origem=Ronald (tmux)

| Verificação | Resultado e evidência |
|---|---|
| União I1 + v2/toca/bin | **NAO PROCEDE** perda aparente no diff fornecido: fake `--censo` preservado (`bin/tests/test_codex_pergunta.sh:82`); casos I1 (`:248`, `:264`, `:606`) e mutantes (`:698`) coexistem com novos casos (`:506`, `:607`) e mutantes (`:704`). Comparação integral com ancestrais: **[nao verificado]**. |
| Nomes duplicados | **NAO PROCEDE**: 90 nomes em `CASOS`, 90 funções correspondentes, sem repetição; nenhuma das 93 linhas de mutantes é duplicada (`bin/tests/test_codex_pergunta.sh:601`, `:630`). Repetições de tags servem cenários distintos, por exemplo `portao` carga/toca (`:706`). |
| Mutante sem caso | **NAO PROCEDE**, estaticamente: toda tag `#G` tem mutante e todo mutante aponta a função existente; cobertura também é cobrada pelo selo (`bin/tests/test_codex_pergunta.sh:724`). |
| Mordida dos novos casos | Há asserções específicas: validação cobra rc, motivo e ausência de chamada (`bin/tests/test_codex_pergunta.sh:163`); inventário cobra motivo em `PROIBIDO`, rc3 e sidecars (`:169`); segunda vaga cobra `VAGA2`, incremento unitário e motivo no log (`:498`); protegidos são exercitados individualmente (`:580`). |
| “Cada mutante morreu pela asserção específica” | **PROCEDENTE a ressalva**: o controlador aceita qualquer texto `FALHA` como morte, sem conferir qual asserção falhou (`bin/tests/test_codex_pergunta.sh:740`). Resultado efetivo dos 93 mutantes: **[nao verificado]**. |
| Fontes falsas | Guarda real recusa wrapper dentro de Git ou vivo de produção (`bin/codex_pergunta.sh:112`); teste cobre somente wrapper dentro de Git (`bin/tests/test_codex_pergunta.sh:588`). Não prova exclusividade do selo: qualquer wrapper fora de Git com vivo fora de produção satisfaz a condição (`bin/codex_pergunta.sh:113`). |
| Sintaxe dos mutantes | Os 93 textos mutados passaram por `bash -n` via stdin e compilação Python do inventário em memória; execução comportamental não realizada **[nao verificado]**. O selo completo cria/remove arquivos (`bin/tests/test_codex_pergunta.sh:22`, `:78`). |

## 09/11 14:02 · sessao 94effc8c3aed · BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS · construtor · origem=Code

Implementei a cura de S127 e a preservação do motivo administrativo; M38 ficou explicitamente **ESTRUTURAL** no teste (`chamados/juizes.py:2137`; `chamados/services/materializacao.py:1376`, `:1434`; `chamados/tests/test_bug_par_intervalo.py:815`). **RED/GREEN executados, mortes dos mutantes e resultado em produção: [nao verificado].**

Entrega por hunk:

| Arquivo:linha | Alteração |
|---|---|
| [juizes.py:2126](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-105046/app/chamados/juizes.py:2126) | S127 preserva a exceção vazia e consome `not disputa.pode_fechar()`, sem recompor C6 ou espera. |
| [materializacao.py:1338](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-105046/app/chamados/services/materializacao.py:1338) | Importa o motivo canônico; limpeza em `:1376` retira exclusivamente `ESPERA_PAR_INTERVALO`. Espera em `:1434` preserva metades com `pendencia_admin`. |
| [auditar_invariantes_chamados.py:546](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-105046/app/chamados/management/commands/auditar_invariantes_chamados.py:546) | Extrai a consulta existente para `perguntas_i19()`; o comando passa a consumi-la em `:1118`. |
| [test_bug_par_intervalo.py:457](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-105046/app/chamados/tests/test_bug_par_intervalo.py:457) | REDs escritos antes da cura: `transicionar` real para fechado/resolvido; contracasos vazio e todas mortas; pendência anterior, pendência criada no plantio e limpeza exclusiva da espera. Cobrança do I19 real em `:503`. |
| `chamados/tests/test_bug_par_intervalo.py:815` | Docstring explica que M38 morde a chamada; com carimbo vazio, a guarda antiga conserva o mesmo estado/trilha. |
| [test_disputa_s127.py:24](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-105046/app/chamados/tests/test_disputa_s127.py:24) | Dublê suporta `exists()`; controle e cobrança da consulta C7 em `:37`. |
| [test_pergunta_viva_no_guarda.py:35](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-105046/app/chamados/tests/test_pergunta_viva_no_guarda.py:35) | Selo estrutural exige S127 → C7 → C6 e proíbe recomposição local. |
| [mutantes_bug_par_intervalo.py:157](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-105046/app/chamados/tests/mutantes_bug_par_intervalo.py:157) | M45–M49; compilação dos tocados em `:223`; comandos individuais v5 em `:245`. |

Os testes novos pertencem ao `TestCase` com `freeze_time` e cadastro sintético `col960` (`chamados/tests/test_bug_par_intervalo.py:32`, `:45`).

Censo de leitores de `disputa_viva_do_chamado`:

| Leitor | Efeito |
|---|---|
| `chamados/models.py:153`, `:154` | Único chamador de produção encontrado: `transicionar` passa a reter também os impedimentos exclusivos de C7, mantendo a exceção vazia (`chamados/juizes.py:2137`). |
| `chamados/tests/test_disputa_s127.py:45` | Testa o adaptador controlando C7; cobra a consulta em `:47`. |
| `chamados/tests/test_pergunta_viva_no_guarda.py:37` | Inspeciona o fonte e exige delegação integral. |
| `chamados/tests/test_bug_par_intervalo.py:482`, `:496` | Contracasos consultam a função real; os dois REDs principais chegam indiretamente por `transicionar` em `:469`. |
| `chamados/tests/mutantes_bug_par_intervalo.py:157`, `:167` | M45/M48 recompilam S127 substituindo C7 pela recomposição antiga. |
| `chamados/tests/test_o261_fio_tem_uma_porta.py:25` | Referência documental histórica; não é chamada executável. |

Exaustividade de acessos dinâmicos: **[nao verificado]**.

Quem escreve e quem lê, inclusive fora do diff:

| Campo/função | ESCREVE / dono | LÊ / chama |
|---|---|---|
| `pendencia_admin` + motivo técnico | Escritor conjunto permanece em `materializacao.py:937`; fixtures em `test_bug_par_intervalo.py:513`. | Espera `materializacao.py:1434`; explicador `explicador.py:93`; I19 `auditar_invariantes_chamados.py:549`; modal `templates/chamados/partials/modal_fio.html:222`; cartão `_card_atendimento.html:183`. |
| `recusa_motivo` | `materializacao.py:937`, `:1190`, `:1377`; `validacao.py:605`; `disputa_emissao.py:1494`; `chamados/models.py:3197`, chamado também por `fio.py:1351`. | Espera `materializacao.py:112`, `:116`; devolução/trilha `validacao.py:147`, `:195`, `:211`; explicador `explicador.py:98`; seleção O330 `passivo_validado.py:66`; auditor `auditar_invariantes_chamados.py:536`, `:549`, `:1081`; `ponto/services/flip_auto.py:132`; modal `modal_fio.html:226`; cartão `_card_atendimento.html:186`. |
| `_materializar_par` | Definição `materializacao.py:1329`; limpeza de motivo em `:1377`. | `_materializar_uma`, `materializacao.py:1438`; testes `test_bug_par_intervalo.py:843`, `:862`. | [nao verificado]
| `_materializar_uma` | Definição `materializacao.py:1402`; espera usa o escritor existente `_carimbar_recusa` em `:1435`. | Validação `validacao.py:469`, `:474`; varredura `materializacao.py:1595`; O330 `passivo_validado.py:532`. |
| `perguntas_i19` | Consulta sem escrita, `auditar_invariantes_chamados.py:546`. | Comando `:1118`; teste `test_bug_par_intervalo.py:503`. | [nao verificado]
| `MUTANTES` e fontes | Definições novas `mutantes_bug_par_intervalo.py:157`. | Mutação `:196`; conferência estática `:237`; emissão de comandos `:261`. |

Mutantes e asserções discriminantes previstas — **mortes executadas: [nao verificado]**:

| Mutante | Regressão | Asserção que deve matar |
|---|---|---|
| M45 / M48 | S127 recompõe sem C7 (`mutantes_bug_par_intervalo.py:157`, `:167`). | Exige erro específico de S127 na chamada real para fechado/resolvido (`test_bug_par_intervalo.py:468`). |
| M46 | Limpeza de qualquer motivo (`mutantes_bug_par_intervalo.py:161`). | Par pendência/motivo permanece igual ao observado durante o plantio (`test_bug_par_intervalo.py:552`). |
| M47 | Espera sobrescreve pendência (`mutantes_bug_par_intervalo.py:164`). | Motivo anterior preservado imediatamente após a espera (`test_bug_par_intervalo.py:517`). |
| M49 | Nunca limpa espera (`mutantes_bug_par_intervalo.py:171`). | Espera antiga deve ficar vazia após aplicar (`test_bug_par_intervalo.py:566`). |
| M38 | Guarda antiga pula descarimbo vazio (`mutantes_bug_par_intervalo.py:131`). | Cobra ambas as chamadas; selo estrutural (`test_bug_par_intervalo.py:842`, `:815`). |

Comandos individuais reservados ao Code; correspondem ao emissor `--comandos-v5` (`chamados/tests/mutantes_bug_par_intervalo.py:245`):

```bash
export DJANGO_SETTINGS_MODULE=config.settings.ci
export PYTHONDONTWRITEBYTECODE=1

teste() { [nao verificado]
  python3 manage.py test "chamados.tests.test_bug_par_intervalo.BugParIntervaloCol960.$1" --noinput
}
mutante() {
  python3 -m chamados.tests.mutantes_bug_par_intervalo "$1"
}

teste test_V5_S127_transicionar_fechado_retem_espera_com_irma_retratada [nao verificado]
teste test_V5_S127_transicionar_resolvido_retem_espera_com_irma_retratada [nao verificado]
teste test_V5_S127_sem_pergunta_segue_travando [nao verificado]
teste test_V5_S127_todas_mortas_sem_resposta_pendente_libera [nao verificado]
teste test_V5_pendencia_anterior_preserva_motivo_na_espera_e_no_par [nao verificado]
teste test_V5_pendencia_nascida_no_plantio_preserva_motivo_e_I19 [nao verificado]
teste test_V5_limpeza_retira_so_espera_preserva_outro_motivo [nao verificado]
teste test_V4_P6_falha_com_carimbo_vazio_duas_vezes_mesmo_estado_e_trilha [nao verificado]

mutante M38
mutante M45
mutante M46
mutante M47
mutante M48
mutante M49

python3 manage.py test \
  chamados.tests.test_disputa_s127 \
  chamados.tests.test_pergunta_viva_no_guarda --noinput
```

Executei a conferência abaixo: **rc 0**, `py_compile` dos sete arquivos tocados sem persistência; âncoras únicas, sintaxe e existência dos alvos M1–M49 conferidas (`chamados/tests/mutantes_bug_par_intervalo.py:212`, `:231`, `:237`):

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.mutantes_bug_par_intervalo --sintaxe
```

**DECISAO-ABERTA:** M38 permanece prova estrutural, conforme a alternativa solicitada; contracaso comportamental: **[nao verificado]** (`chamados/tests/test_bug_par_intervalo.py:815`). Execução dos REDs, GREEN e certificação pelo Code/opus: **[nao verificado]**.

## 09/11 13:52 · sessao 3b20bcb6caff · SONDA-SEM-GRANT · leitura · origem=Code

**A matriz está ABERTA.** Existe juiz único da conferência de catálogo, mas falta uma transição completa com contrato único de prontidão, falha e idempotência. A porta confirma alterações em duas transações e publica a credencial depois delas. (`bin/sonda_leitor_comum.sh:158`; `bin/sonda_provisionar.sh:118`; `bin/sonda_provisionar.sh:168`; `bin/sonda_provisionar.sh:196`)

Referências abreviadas abaixo são **arquivo:linha**, com esta expansão:

| Sigla | Arquivo |
|---|---|
| C | `bin/sonda_leitor_comum.sh` |
| P | `bin/sonda_provisionar.sh` |
| L | `bin/sonda_leitura.sh` |
| T | `bin/tests/test_sonda_sem_grant.sh` |
| G | `bin/tests/test_sonda_sem_grant_pg.sh` |
| S | `bin/sombra.sh` |

Execuções, RED/GREEN, mortes efetivas dos mutantes e estado atual de PROD: **[nao verificado]**. A coluna PROVA identifica casos e asserções existentes; não homologa os relatos dos anexos.

**1. Estados e fechamento**

O estado deve ser tratado como vetor: destino × existência/atributos do leitor × credencial × ACL vigente × defaults × autenticação × fase da transição. Esses componentes são consultados ou alterados separadamente no vivo. (`C:26`; `C:79`; `C:132`; `C:191`; `P:105`; `P:127`; `P:193`)

| Dimensão | Estados encontrados/contemplados no vivo |
|---|---|
| Destino | PROD=`saas_db/saas_hasner`; sombra=`sombra_db/sombra`; credencial compartilhada. (`C:22`; `C:28`; `C:29`) |
| Role | Ausente; sem LOGIN; super/poderes; membro; dono; configuração global ou por banco divergente. (`C:80`; `C:87`; `C:146`; `C:147`) |
| Credencial | Ausente; sem senha; modo diferente de 600; formato antigo com HOST; senha divergente; senha conhecida apenas pelo cluster. (`L:173`; `L:178`; `L:194`; `P:79`) |
| Relações | Leitura faltante; ACL aberta em tabela/view/partição/foreign table, coluna ou sequência; GRANT OPTION. (`C:97`; `C:112`; `C:122`) |
| Fora de relação | LO nomeados; SECURITY DEFINER não-sistema; CONNECT em outros bancos; TEMP/CREATE; CREATE/GRANT OPTION em schema. (`C:126`; `C:129`; `C:136`; `C:139`; `C:143`) |
| Defaults | De `saas_user`; do leitor; de outro concedente; destinados ao leitor ou PUBLIC, nos tipos consultados `r/S/n`. (`C:105`; `C:132`; `P:133`; `P:150`) |
| Alteração pelo leitor | `read_only` global/por banco, senha e defaults próprios alterados com sucesso antes da detecção seguinte. (`G:365`; `G:376`; `G:379`) |
| Instrumento/transição | Conferência muda, incompleta ou com erro; login recusado; cluster inacessível; trava ocupada; falha após cada COMMIT, na publicação ou na trilha. (`L:185`; `C:160`; `P:69`; `P:118`; `P:168`; `P:196`; `P:55`) | [nao verificado]

**Não está FECHADA por catálogo.** Há enumeração de relações/ACL, mas funções são limitadas por nomes e `prosecdef`; defaults consultados excluem tipos diferentes de `r/S/n`. A leitura de tabelas/sequências usa `information_schema`, sem prova fornecida de visibilidade integral do executor real. (`C:98`; `C:102`; `C:112`; `C:132`; `C:136`; `C:139`)

| Prova necessária de fechamento | Situação |
|---|---|
| Inventário completo dos objetos, ACL, defaults e capacidades, incluindo o complemento das consultas atuais | **FALTA**; fronteiras atuais em `C:112`, `C:132`, `C:136`, `C:139`. |
| Cada item inventariado classificado como permitido, recusado ou fora do contrato, sem resto sem classificação | **FALTA**; lista LO nominal e defaults filtrados. (`C:19`; `C:134`) |
| Cobertura de combinações, concorrência e sessões anteriores | **FALTA**; conferir, autenticar e executar são etapas separadas; CONNECT é revogado sem operação de encerramento de sessões. (`L:187`; `L:194`; `L:233`; `P:117`) |

**2. Matriz estado × role**

Convenções com expansão contratual:

| Símbolo | Significado existente |
|---|---|
| **J(k)** | `_q_conferencia` mede a chave; `_conferencia_pronta` decide. Porta e leitura delegam ao mesmo predicado. (`C:76`; `C:158`; `P:173`; `L:187`) |
| **A** | `_login_leitor` mede autenticação/sessão; a comparação `leitor\|on` está repetida nos dois chamadores. (`C:191`; `P:187`; `L:196`) |
| **E+T** | Em sucesso, promoção de `.env_leitor` e tentativa de trilha da porta; em falha, vale a matriz de fases adiante. (`P:193`; `P:196`; `P:60`) |
| **R(k)** | Leitura recusa rc **5**, nomeando a falta e a porta; registra trilha após instalação do trap. (`L:126`; `L:129`; `L:187`) |
| **✓** | Porta rc **0**, mensagem `pronto`, após conferência, login e publicação; leitura executa a sonda e devolve seu rc, remapeando 5→1. (`P:173`; `P:187`; `P:199`; `L:233`; `L:244`) |
| **F** | Porta rc **1**, sem anúncio regular de pronto, por falha SQL, conferência ou login. (`P:93`; `P:174`; `P:184`) |
| **N/A** | Dimensão inaplicável: PUBLIC é representado como grantee `0`, não como a role de LOGIN examinada. (`C:74`; `C:149`) |

“Role” abaixo identifica o titular afetado ou concedente. `saas_user` também é o executor administrativo fixo; seus próprios atributos/capacidades não são objeto do juiz atual. (`C:10`; `C:109`; `P:51`; `L:166`)

| Estado | Role | JUIZ → OPERAÇÃO | PERSISTIDO | RETORNO | PROVA / mutante |
|---|---|---|---|---|---|
| Role ausente | leitor | J(role) → CREATE/ALTER. (`C:80`; `P:107`) | Role, senha/configuração; E+T. (`P:108`; `P:109`) | R(role); porta ✓ se completar. | Recusa falsa `T:139`; criação `G:222`. Mutante isolante da ausência: **FALTA**. |
| Role ausente | saas_user | **FALTA** bootstrap/juiz; conexão já exige esse usuário. (`P:51`; `L:166`) | Sem restauração prevista desse executor. (`P:107`) | Falha SQL F / conferência R. (`P:93`; `L:185`) | **FALTA**. |
| Role ausente | PUBLIC | N/A: grantee `0`. (`C:74`) | N/A. | N/A. | N/A. |
| Presente, senha desconhecida/divergente | leitor | A; porta usa senha do arquivo ou gera outra e altera role. (`C:191`; `P:77`; `P:79`; `P:108`) | Senha do destino; E+T. | R(login); porta ✓/F. (`L:194`; `P:184`) | Divergência real `G:287`; ignorar login morderia `G:289`. Caso “role existente + arquivo ausente”: **FALTA**. |
| Presente, senha desconhecida/divergente | saas_user | **FALTA** juiz da sua senha; conexão administrativa usa socket sem senha no script. (`C:10`; `P:51`) | Nenhuma rotação desse usuário prevista. (`P:108`) | Dependente da conexão administrativa. (`P:93`) | **FALTA**. |
| Presente, senha desconhecida/divergente | PUBLIC | N/A. (`C:74`) | N/A. | N/A. | N/A. |
| `.env_leitor` ausente/desatualizado/inválido | leitor | Guardas locais + A; porta normaliza arquivo ao concluir. (`L:173`; `L:179`; `P:194`) | E+T; cluster pode mudar antes do arquivo. (`P:108`; `P:196`) | R(credencial/HOST/login); ✓/F. | Ausência/HOST/sem senha `T:147`, `T:150`, `T:252`; falhas mktemp/chmod/mv: **FALTA**. |
| `.env_leitor` ausente/desatualizado/inválido | saas_user | N/A como credencial desse usuário: arquivo é consumido pelo login leitor. (`L:194`) | Arquivo não publica senha de `saas_user`. (`P:194`) | **FALTA** garantia de disponibilidade do executor. (`P:51`) | **FALTA**. |
| `.env_leitor` ausente/desatualizado/inválido | PUBLIC | N/A. (`C:74`; `C:22`) | N/A. | N/A. | N/A. |
| ACL de relação/coluna/sequência aberta | leitor | J(esc_tab/esc_seq) → REVOKE relações, depois SELECT. (`C:112`; `C:122`; `P:129`; `P:165`) | ACL; E+T. Revogação explícita de ACL de coluna: **FALTA** no bloco. (`P:128`) | R(chave); ✓ se zerar, senão F. (`P:173`) | Estragos `G:250`–`G:255`; mutante específico por subtipo: **FALTA**. |
| ACL de relação/coluna/sequência aberta | saas_user | **FALTA** juiz de seus privilégios; consulta filtra leitor/PUBLIC. (`C:116`; `C:125`) | Porta concede leitura ao leitor, sem contrato de preservação do executor. (`P:165`) | **FALTA** retorno sobre esse sujeito. | **FALTA**. |
| ACL de relação/coluna/sequência aberta | PUBLIC | J detecta; operação de revogação relacional de PUBLIC: **FALTA**. (`C:116`; `P:129`) | ACL aberta pode permanecer; fases anteriores já confirmadas. (`P:118`; `P:173`) | R(chave); porta F. | Só cobrança textual `T:217`; estrago PG PUBLIC relacional e mutante: **FALTA**. |
| ACL LO aberta | leitor | J(esc_lo) → REVOKE das famílias nomeadas. (`C:136`; `P:156`) | `pg_proc.proacl`; E+T. | R(esc_lo); ✓/F. | Tentativas `G:327`–`G:329`; M1 atingiria primeiro `G:223`. Sete famílias sem tentativa: **FALTA** frente a `C:19`. |
| ACL LO aberta | saas_user | **FALTA** juiz de capacidades/impacto; filtro não o inclui. (`C:138`) | REVOKE não o nomeia. (`P:156`) | **FALTA** garantia de acesso preservado. | **FALTA**. |
| ACL LO aberta | PUBLIC | J(esc_lo) → REVOKE EXECUTE. (`C:136`; `P:156`) | `proacl`; E+T. | R(esc_lo); ✓/F. | Estrago `G:267`; retirar chave, M4, morderia `G:242`. |
| SECURITY DEFINER aberta/nova | leitor | J(esc_func) → REVOKE não-sistema. (`C:139`; `P:157`) | `proacl`; E+T. | R(esc_func); ✓/F. | Escrita autenticada `G:336`; M2 atingiria primeiro `G:223`. |
| SECURITY DEFINER aberta/nova | saas_user | **FALTA** juiz de impacto no dono/app. (`C:142`; `P:157`) | ACL do objeto é alterada, sem prova de clientes preservados. (`P:157`) | **FALTA** retorno sobre impacto. | **FALTA**. |
| SECURITY DEFINER aberta/nova | PUBLIC | J(esc_func) → REVOKE EXECUTE. (`C:139`; `P:157`) | `proacl`; E+T. | R(esc_func); ✓/F. | Função nova `G:266`; retirar chave, M5, morderia `G:242`. Concorrência: **FALTA**. |
| CONNECT em outro banco | leitor | J(outros_bancos) → REVOKE ALL nos outros bancos conectáveis. (`C:143`; `P:117`) | `pg_database.datacl`; E+T. | R(outros_bancos); ✓/F. | Novas conexões `G:345`; M3 atingiria primeiro `G:223`. Sessão anterior: **FALTA**. |
| CONNECT em outro banco | saas_user | **FALTA** juiz de continuidade de acesso; não é sujeito da contagem. (`C:145`) | Efeito indireto via PUBLIC depende de seus privilégios reais. (`P:117`) | **FALTA** retorno de impacto. | Superuser real/acesso preservado: **[nao verificado]**. |
| CONNECT em outro banco | PUBLIC | J(outros_bancos) → REVOKE CONNECT em todos os outros `datallowconn`. (`C:143`; `P:117`) | `datacl`; E+T. | R(outros_bancos); ✓/F. | Estrago `G:268`; retirar chave, M6, morderia `G:242`. |
| TEMP/CREATE/GRANT OPTION em banco/schema | leitor | J(esc_banco/esc_schema) → REVOKE ALL; repõe CONNECT/USAGE. (`C:126`; `P:131`; `P:158`; `P:162`) | ACL banco/schema; E+T. | R(chave); ✓/F. | Estragos `G:256`–`G:258`; tentativas `G:333`–`G:335`; mutante específico: **FALTA**. |
| TEMP/CREATE/GRANT OPTION em banco/schema | saas_user | **FALTA** juiz de impacto; excluído do filtro. (`C:128`; `C:131`) | Possível perda do que dependia de PUBLIC. (`P:132`; `P:159`) | **FALTA** retorno de impacto. | **FALTA**. |
| TEMP/CREATE/GRANT OPTION em banco/schema | PUBLIC | J → revoga CREATE schema e TEMP/CREATE banco; não toda ACL grantable restante. (`C:128`; `C:131`; `P:132`; `P:159`) | ACL; eventual resíduo permanece detectável. (`P:173`) | R(chave); ✓ ou F. | TEMP `G:259`; demais variantes PUBLIC e mutantes: **FALTA**. |
| Defaults do leitor abertos | leitor | J(default_escrita/dono) → revoga defaults próprios destinados a PUBLIC. (`C:132`; `C:147`; `P:136`; `P:150`) | `pg_default_acl`; E+T. | R(chave); ✓/F. | ALTER próprio `G:379`; M7 morderia reprovisionamento `G:372`. |
| Defaults do leitor abertos | saas_user | N/A como concedente dessa célula; operação distingue os dois donos. (`P:133`; `P:136`) | Nenhuma garantia própria adicional. | N/A. | **FALTA** prova de impacto combinado. |
| Defaults do leitor abertos | PUBLIC | J detecta destinatário; porta revoga tabelas/sequências por schema e `r/S/n` globais. (`C:133`; `P:136`; `P:150`) | `pg_default_acl`; E+T. | R(default_escrita); ✓/F. | Tabela global `G:379`; sequência/schema/por-schema autenticados: **FALTA**. |
| Defaults de `saas_user` abertos | leitor | J(default_escrita/default_falta) → remove grants e repõe SELECT. (`C:105`; `C:132`; `P:133`; `P:163`) | `pg_default_acl`; E+T. | R(chave); ✓/F. | Estragos `G:261`, `G:262`, `G:265`; mutantes específicos: **FALTA**. |
| Defaults de `saas_user` abertos | saas_user | J examina seus defaults **para leitor**, não seus poderes gerais. (`C:109`; `C:133`) | Defaults alterados como concedente. (`P:133`; `P:143`; `P:163`) | ✓/F segundo prontidão do leitor. (`P:173`) | Tabela futura legível `G:230`; preservação de outros destinatários: **FALTA**. |
| Defaults de `saas_user` abertos | PUBLIC | J detecta `r/S/n`; operação para revogar esses grants de PUBLIC: **FALTA**. (`C:132`; `P:133`; `P:150`) | Default aberto pode permanecer. (`P:175`) | R(default_escrita); porta F. | Caso PG e mutante: **FALTA**. |
| Defaults de outro concedente abertos | leitor | J detecta; cura automática: **FALTA**. (`C:132`; `P:175`) | Default permanece; outras fases podem estar confirmadas. (`P:118`; `P:168`) | R(default_escrita); F. | Caso PG/mutante: **FALTA**. |
| Defaults de outro concedente abertos | saas_user | **FALTA** avaliação de dependências desse executor. (`C:133`) | Sem operação específica. (`P:133`; `P:150`) | **FALTA** retorno próprio. | **FALTA**. |
| Defaults de outro concedente abertos | PUBLIC | J detecta; cura automática: **FALTA**. (`C:133`; `P:175`) | Default permanece. | R(default_escrita); F. | Caso PG/mutante: **FALTA**. |
| Leitor altera `read_only`/senha | leitor | J(ro/ro_banco)+A → próxima leitura recusa; porta realinha. (`C:84`; `C:87`; `P:108`; `P:111`) | Alteração do leitor persiste até cura; depois E+T. (`G:365`; `G:371`) | ALTER deve dar 0 no selo; leitura seguinte R; porta ✓/F. (`G:366`; `G:368`) | `G:376`–`G:378`; mutante de **impedimento**: **FALTA**, pois selo exige aceitação. |
| Leitor altera `read_only`/senha | saas_user | N/A como titular; atua administrativamente na cura. (`P:51`; `P:108`) | Estado alterado é do leitor. (`P:109`) | N/A. | Prova de preservação do executor: **FALTA**. |
| Leitor altera `read_only`/senha | PUBLIC | N/A; default próprio para PUBLIC está na célula anterior de defaults. (`G:379`) | N/A nesta célula. | N/A. | N/A. |
| Super/poderes/membership/ownership | leitor | J; cura atributos/membros; reatribuição de propriedade: **FALTA** na porta. (`C:81`; `C:146`; `P:108`; `P:112`; `P:175`) | Atributos/membros alterados; propriedade pode permanecer. | R(chave); ✓ ou F. (`P:173`) | Membership/super `G:260`, `G:264`; dono falso `T:195`; ownership real/mutante: **FALTA**. |
| Super/poderes/membership/ownership | saas_user | **FALTA** juiz de seus atributos/heranças. (`C:149`) | Não normalizados pela porta. (`P:108`) | **FALTA** retorno próprio. | **FALTA**. |
| Super/poderes/membership/ownership | PUBLIC | N/A como role proprietária/membro examinada. (`C:146`; `C:147`) | ACL PUBLIC continua avaliada nas células próprias. (`C:116`) | N/A. | N/A. |
| Leitura/default de leitura faltante | leitor | J(usage/tab/seq/default_falta) → CONNECT, USAGE, SELECT e defaults. (`C:92`; `C:105`; `P:160`; `P:166`) | ACL/defaults; E+T. | R(chave); ✓/F. | Falta falsa `T:137`; tabela futura `G:230`; mutantes por dimensão: **FALTA**. |
| Leitura/default de leitura faltante | saas_user | Seu OID é o concedente exigido; visibilidade integral do inventário: **FALTA** prova. (`C:98`; `C:109`) | Defaults desse concedente. (`P:163`) | ✓/F segundo leitor. (`P:173`) | Fixture `G:145`; equivalência ao executor real: **[nao verificado]**. |
| Leitura/default de leitura faltante | PUBLIC | SELECT/USAGE/CONNECT simples são admitidos, sem exigência de que PUBLIC os possua. (`C:116`; `C:128`; `C:131`) | Porta não concede leitura a PUBLIC. (`P:160`; `P:165`) | Não decide presença de leitura por PUBLIC. (`C:92`) | Caso isolante: **FALTA**. |
| Conferência ilegível/erro | leitor | J rejeita prefixo/chaves; falha SQL recusa. Porta tolera erro **antes**, rejeita **depois**. (`C:160`; `C:164`; `L:185`; `P:72`; `P:173`) | Leitura: trilha; porta: alterações já confirmadas podem ficar. (`L:121`; `P:168`) | L5; P1 após conferência final. | Mudo/erro/chave ausente `T:141`, `T:143`, `T:199`; duplicata/valor contraditório: **FALTA**. |
| Conferência ilegível/erro | saas_user | Executor da consulta; diagnóstico próprio: **FALTA**. (`L:166`; `P:51`) | Trilha tenta registrar erro genérico. (`P:72`; `P:60`) | L5/P1; sem distinguir causa administrativa. (`L:185`; `P:174`) | Caso separado por causa: **FALTA**. |
| Conferência ilegível/erro | PUBLIC | A conferência inteira é recusada; sem decisão parcial confiável de suas ACL. (`C:158`; `L:187`) | ACL não é curada pela leitura. (`L:166`) | L5/P1. (`L:185`; `P:174`) | Caso específico PUBLIC: **FALTA**. |
| Cluster inacessível | leitor | PROD: consulta recusa; sombra tenta subir cluster primeiro. (`L:143`; `L:185`) | Sombra pode alterar estado Docker; porta tenta trilha. (`L:144`; `P:60`) | PROD L5; sombra pode L1; porta falha SQL P1. (`L:144`; `P:93`) | Simulação de consulta `T:143`; indisponibilidade real/mutante: **FALTA**. |
| Cluster inacessível | saas_user | Conexão administrativa não disponível; reparo: **FALTA**. (`P:51`) | Estado do banco não conferido. (`P:72`) | Falha SQL P1. (`P:93`) | **FALTA**. |
| Cluster inacessível | PUBLIC | Estado ACL não observável nessa execução. (`C:143`; `L:185`) | **[nao verificado]**. | Recusa por conferência, não por ACL medida. (`L:185`) | **FALTA**. |
| Pronto; PROD × sombra | leitor | J+A separados; destino fixo; garantia conjunta dos dois clusters: **FALTA**. (`C:28`; `C:29`; `P:173`; `P:187`) | ACL do destino + E+T; sombra só carimba depois da porta. (`P:169`; `S:340`; `S:341`) | ✓ no destino escolhido. (`P:199`) | Ambos `G:220`, `G:309`; comparação de estado 2×: **FALTA**. |
| Pronto; PROD × sombra | saas_user | **FALTA** juiz da disponibilidade/capacidade do app nos dois clusters. (`C:149`) | Defaults alterados; impacto via PUBLIC não certificado. (`P:117`; `P:163`) | Nenhuma pós-condição própria. (`P:173`) | **FALTA**. |
| Pronto; PROD × sombra | PUBLIC | J considera ACL do destino e CONNECT dos demais bancos do cluster escolhido. (`C:129`; `C:143`) | Revogações amplas, inclusive em outros bancos. (`P:117`; `P:157`; `P:159`) | ✓ não certifica clientes legítimos dependentes de PUBLIC. (`C:145`) | `postgres/template1` em `G:345`; inventário real/clientes: **[nao verificado]**. |

**3. Contrato de saída e falha**

O predicado de catálogo existente é:

```text
role = login = ro = connect = 1
todas as chaves de CONF_AUSENCIA = 0
```

Ele é definido em `C:154`–`C:168`. Além dele, a porta cobra login `leitor|on` e arquivo promovido com modo 600; esses últimos não são verificáveis apenas por catálogo. (`P:183`; `P:187`; `P:196`; `P:198`)

| Saída/fase | Garantia atual e estado que pode ficar |
|---|---|
| **rc 0** | Conferência aprovada, depois login aprovado, depois arquivo publicado. **FALTA** garantia de que o catálogo continua igual no instante do retorno, diante de concorrência entre essas etapas. (`P:172`; `P:183`; `P:196`) | [nao verificado]
| Falha antes da transação da role | Sem alterações SQL enviadas por essa execução; lock/log podem existir. Argumento/sudo: rc2; trava/senha inválida: rc1. (`P:45`; `P:47`; `P:68`; `P:70`; `P:84`) |
| Falha dentro da transação da role | BEGIN/COMMIT com `ON_ERROR_STOP`; não há COMMIT intermediário do laço de bancos. Resultado concreto após falha: **[nao verificado]**. (`P:51`; `P:105`; `P:117`; `P:118`) | [nao verificado]
| Falha após primeiro COMMIT | Role/senha/configuração/membership/CONNECT podem estar novos; ACL e arquivo podem continuar antigos. **Não há atomicidade global.** (`P:118`; `P:127`; `P:196`) | [nao verificado]
| Falha após segundo COMMIT | Role e ACL podem estar novos, arquivo ainda antigo; conferência/login recusam a publicação. (`P:168`; `P:174`; `P:184`) | [nao verificado]
| Falha depois do `mv` | Arquivo já pode ser novo: verificação final ocorre após a promoção. (`P:196`; `P:198`) |
| Falha na trilha | `pronto` pode já ter sido emitido; o trap tenta gravar depois, sem tratamento específico da falha de append. **FALTA** contrato/selo de coerência mensagem–persistência–rc. (`P:199`; `P:55`; `P:61`) |
| Falha da porta dentro de `--refazer` | Carimbo já invalidado; falha impede publicação final. Esse caminho não mantém o verde anterior. (`S:288`; `S:265`; `S:340`; `S:341`) | [nao verificado]

**Idempotência:** `2× = mesmo estado total

[truncado em 20000 de 23724 caracteres]

## 09/11 13:48 · sessao f59a012f0887 · - · - · origem=Ronald (tmux)

| Contrato/célula | Evidência no vivo |
|---|---|
| Juiz único existente de prontidão de catálogo | `_conferencia_pronta`, `bin/sonda_leitor_comum.sh:158`, exige presenças=1/ausências=0 (`:154-168`); chamado pela porta `bin/sonda_provisionar.sh:173` e leitura `bin/sonda_leitura.sh:187`. |
| Juiz de autenticação distinto, pergunta distinta | `_login_leitor`, `bin/sonda_leitor_comum.sh:191`; porta compara `leitor\|on` em `bin/sonda_provisionar.sh:187`; leitura em `bin/sonda_leitura.sh:196`. Não é duplicação da pergunta de catálogo. |
| `saas_user` | É executor administrativo fixo (`bin/sonda_leitor_comum.sh:10`; `bin/sonda_provisionar.sh:51`; `bin/sonda_leitura.sh:166`) e concedente dos defaults (`bin/sonda_leitor_comum.sh:109`). **FALTA juiz de seus atributos, disponibilidade e capacidades**; ser superuser real: **[nao verificado]**. |
| rc 0 da porta | Conferência pós-alteração aprovada, login `leitor\|on`, envfile promovido e modo 600 verificado (`bin/sonda_provisionar.sh:172-199`). Predicado é instantâneo; não certifica estabilidade depois das consultas. |
| rc !=0 antes da transação 1 | Argumento/sudo recusam rc2 (`bin/sonda_provisionar.sh:45-47`); trava/senha inválida rc1 (`:69-84`). Sem SQL de alteração antes disso, mas lock/log podem ter sido criados (`:59-68`). |
| rc !=0 na transação 1 | Role/atributos/senha/CONNECT dos outros bancos estão num único BEGIN/COMMIT, com ON_ERROR_STOP (`bin/sonda_provisionar.sh:51,105,118`). Estado final executado: **[nao verificado]**. |
| rc !=0 depois da transação 1 | **Pode ficar parcialmente aplicado**: COMMIT role/senha/CONNECT em `bin/sonda_provisionar.sh:118`; ACL só começa `:127`; falhas de ACL, conferência/login/publicação não desfazem a primeira transação (`:169-196`). |
| rc !=0 depois da transação 2 | Role e ACL já confirmados, env velho ainda intacto até `mv` (`bin/sonda_provisionar.sh:168,174,184,196`). **Não garante rollback global**. |
| rc !=0 depois do `mv` | Env já pode ser novo: verificação de modo ocorre depois de sua promoção (`bin/sonda_provisionar.sh:196-198`). |
| Trilha falha | `_fim` tenta log sem guarda e está sob `set -e`; falha de mkdir/append pode interromper antes do `exit "$rc"` (`bin/sonda_provisionar.sh:40,55-62`). O `pronto` já pode ter sido emitido (`:199`). **FALTA contrato/selo de coerência dessa célula**. |
| `.env_leitor` ausente/sem senha | Porta gera senha (`bin/sonda_provisionar.sh:76-79`), alinha cluster (`:108`), só promove após prova (`:193-196`); leitura recusa rc5 (`bin/sonda_leitura.sh:173,179`; `_recusa :129-135`). |
| `.env_leitor` antigo/inválido | Leitura recusa modo ≠600/HOST/sem senha (`bin/sonda_leitura.sh:174-179`); porta reaproveita somente primeira linha da senha, admite alfanumérica, regrava só senha (`bin/sonda_provisionar.sh:77,83-84,194`). |
| Prod × sombra | Destinos fixos (`bin/sonda_leitor_comum.sh:28-29`), **um env compartilhado** (`:22`). Porta lê/gera uma senha (`bin/sonda_provisionar.sh:77-79`) e altera apenas o destino escolhido (`:108`). **FALTA garantia conjunta dos dois clusters após uma execução**. |
| Sombra/carimbo | `--refazer` invalida antes do drop (`bin/sombra.sh:288-289`), chama porta antes do carimbo final (`:340-344`). A própria porta não escreve stamp: sua publicação é `.env_leitor` + log (`bin/sonda_provisionar.sh:60-61,193-199`). |
| Idempotência | Mesmo segredo é reutilizado (`bin/sonda_provisionar.sh:77-79`), mas cada execução reaplica ALTER/REVOKE/GRANT e promove novo arquivo (`:108-168,193-196`), acrescentando trilha com data (`:60-61`). **2x = mesmo estado total é falso pelo log**; equivalência semântica ACL/configuração e caso de selo completo **FALTA**. |
| Conferência ilegível | Leitura recusa consulta falha e parser inválido (`bin/sonda_leitura.sh:185-187`; `bin/sonda_leitor_comum.sh:160`). Porta registra erro na conferência inicial e continua tentando provisionar (`bin/sonda_provisionar.sh:72`); pós-conferência inválida recusa rc1 (`:172-176`). |
| Cluster inacessível | Porta não trata conferência inicial inválida como parada (`bin/sonda_provisionar.sh:72`), mas escrita falha leva rc1 mascarado (`:93-97`). Leitura prod recusa consulta rc5 (`bin/sonda_leitura.sh:185`); sombra tenta subir cluster e pode sair rc1 antes (`:143-145`). |

**Lista não fechada**, pelas seguintes fronteiras do catálogo:

| Fronteira | Referência e consequência |
|---|---|
| Privilégios de relação cobrem todas as ACLs previstas, porém não todo objeto/capacidade PostgreSQL | Relações `r,p,v,m,f`, colunas e sequências não-sistema em `bin/sonda_leitor_comum.sh:112-125`; funções só LO nomeadas/SECURITY DEFINER não-sistema em `:136-141`; **FALTA enumeração das capacidades restantes**. |
| Defaults `f` ignorados | `default_escrita` só examina tipos `r,S,n` (`bin/sonda_leitor_comum.sh:132-135`); default EXECUTE de funções **FALTA célula/juiz**, embora futuras SECURITY DEFINER sejam detectadas depois de criadas (`:139-142`). |
| Inventário de leitura filtrado pelo executor | Tabelas/sequências vêm de `information_schema` (`bin/sonda_leitor_comum.sh:98-104`); comprovação de que saas_user enxerga universo inteiro **[nao verificado]**. |
| PUBLIC DML detectado, não curado | Juiz conta leitor/PUBLIC (`bin/sonda_leitor_comum.sh:112-125`); REVOKE em relações revoga só leitor (`bin/sonda_provisionar.sh:129-130`). Porta pode terminar rc1 sem pronto (`:173-176`). |
| Outros concedentes/defaults | Juiz cobre qualquer dono para `r/S/n` (`bin/sonda_leitor_comum.sh:132-135`); porta cura defaults de saas_user→leitor e leitor→PUBLIC (`bin/sonda_provisionar.sh:133-150`). **FALTA operação para demais concedentes/PUBLIC**, com recusa explícita (`:175`). |
| Membro/dono | Juiz mede membership e ownership do cluster (`bin/sonda_leitor_comum.sh:146-148`); porta revoga memberships (`bin/sonda_provisionar.sh:112`), não reatribui objetos; informa REASSIGN/REVOKE manual (`:175`). |
| Mutação concorrente e sessões anteriores | A porta só executa REVOKE CONNECT, sem encerrar sessões (`bin/sonda_provisionar.sh:117`); leitura confere, faz login e executa depois (`bin/sonda_leitura.sh:187,194,233`). **FALTA predicado de garantia contínua/caso concorrente**. |

**Separação Ronald × técnico:**

- **Decisão já estabelecida:** leitura não pode alterar role; só porta escreve (`bin/sonda_leitura.sh:28-33`); nada sem raspar/credencial no argv/prompt, escrita prod ou segundo envelope (`app/docs/CORTES.md:375`).
- **`!` operacional prod:** comentário reserva porta prod ao Ronald; único bloqueio técnico local é `SUDO_USER`, não identidade Ronald (`bin/sonda_provisionar.sh:5,45-48`). Impacto de REVOKE PUBLIC em clientes externos **[nao verificado]**; operações amplas estão em `:117,156-159`.
- **Lei já decide, sem novo corte:** um escritor com tripwire/trilha (`CLAUDE.md:32-34`), leitor sem regra própria (`:20-22`), risco medido antes (`:23-24`), lei existente antes de novo corte (`:25-27`).
- **Se quiser enfraquecer garantia para aceitar alterações próprias detectadas depois:** isso altera promessa de “impossibilidade de acidente” (`bin/sonda_leitura.sh:8-15`) e escritor único (`CLAUDE.md:32`); não é mero ajuste técnico. Existência desses ALTER próprios está no selo (`bin/tests/test_sonda_sem_grant_pg.sh:365-376`).
- **Técnico:** preencher enumeração, retornos por fase, atomicidade observável, projeção explícita de idempotência, cobertura das três identidades e mutantes específicos; não depende de inventar lei (`CLAUDE.md:20-32`).

## 09/11 13:47 · sessao 9ad35765ebd1 · - · - · origem=Ronald (tmux)

Execuções, RED/GREEN e mortes reais de mutantes: **[nao verificado]**. “Morderia” abaixo identifica asserção, sem execução.

| Estado / role | Caso do selo e mordida identificável | FALTA |
|---|---|---|
| Limpo / leitor | Dois destinos, rc0, SELECT e env600: `bin/tests/test_sonda_sem_grant_pg.sh:219`; autenticação efetiva: `:301`, `:311`. | Comparação de catálogo/env após **duas** portas: FALTA (`:219`, `:308`). |
| Role ausente / leitor | Conferência falsa `role=0`; rc5, mensagem, sem sonda, sem SQL escrito e trilha: `bin/tests/test_sonda_sem_grant.sh:127`, `:139`. | Recusa com role ausente em PG real: FALTA; primeira criação real é caso positivo (`bin/tests/test_sonda_sem_grant_pg.sh:222`). |
| Senha desconhecida/divergente / leitor | Login falso falho: `bin/tests/test_sonda_sem_grant.sh:145`; senha errada real: `bin/tests/test_sonda_sem_grant_pg.sh:285`. | Role existente + env ausente + senha desconhecida explicitamente ensaiados: FALTA. |
| Env ausente/antigo/sem senha / leitor | Recusa rc5 nomeada: `bin/tests/test_sonda_sem_grant.sh:147`, `:150`, `:252`. | Falhas de promoção `mktemp/chmod/mv` do env: FALTA nos casos da porta (`:255`, `:370`). |
| Conferência muda/erro / leitor | `FAKE_CONF=''` e rc2; rc5 e sonda impedida: `bin/tests/test_sonda_sem_grant.sh:141`, `:143`, `:127`. | Cluster indisponível real: FALTA; rc2 é simulação de consulta (`:143`). |
| Relação/coluna/sequência / leitor | Estragos de INSERT, UPDATE coluna, GRANT OPTION e USAGE: `bin/tests/test_sonda_sem_grant_pg.sh:250`; arnês exige chave positiva, rc5, cura e nova leitura: `:238`. | `nextval/setval` autenticados: FALTA na lista `:327`–`:341`. |
| Relação/coluna/sequência / PUBLIC | Consulta textual exige `e.grantee=0`: `bin/tests/test_sonda_sem_grant.sh:217`. | Estrago PG concedido a PUBLIC: FALTA na lista `bin/tests/test_sonda_sem_grant_pg.sh:250`–`:268`; casos relacionais concedem ao leitor. |
| TEMP/CREATE / leitor e PUBLIC | TEMP PUBLIC: `bin/tests/test_sonda_sem_grant_pg.sh:259`; negativas autenticadas TEMP/CREATE: `:333`–`:335`. | Negativa autenticada após retirar especificamente REVOKE PUBLIC: mutante executado [nao verificado]. |
| LO / leitor e PUBLIC | Estrago PUBLIC: `bin/tests/test_sonda_sem_grant_pg.sh:267`; tentativas autenticadas: `:327`–`:329`. | Sete famílias de `bin/sonda_leitor_comum.sh:19` sem tentativa autenticada. |
| SECURITY DEFINER / PUBLIC → leitor | Função nova: `bin/tests/test_sonda_sem_grant_pg.sh:266`; execução que escreve recusada: `:336`. | Concorrência criação após conferência: FALTA; tentativa é sequencial (`:238`, `:266`). |
| Outros bancos / PUBLIC → leitor | Estrago CONNECT PUBLIC: `bin/tests/test_sonda_sem_grant_pg.sh:268`; negativas em postgres/template1, efeito LO zero: `:345`–`:354`. | Sessões já abertas e outros bancos enumerados dinamicamente no selo: FALTA; lista é postgres/template1 (`:345`). |
| Defaults / saas_user → leitor | INSERT, SELECT grantable, schema grantable: `bin/tests/test_sonda_sem_grant_pg.sh:261`, `:262`, `:265`. | saas_user→PUBLIC e outro concedente→leitor/PUBLIC: FALTA na lista `:250`–`:268`. |
| Defaults / leitor → PUBLIC | O leitor executa ALTER DEFAULT PRIVILEGES; exige sucesso, recusa posterior e cura: `bin/tests/test_sonda_sem_grant_pg.sh:365`–`:379`. | Sequência/schema e variante por schema autenticadas: FALTA; caso é tabela global (`:379`). |
| ALTER próprio / leitor | Sucesso obrigatório do ALTER, rc5 posterior e cura: `bin/tests/test_sonda_sem_grant_pg.sh:365`–`:378`. | “Nenhuma escrita persistente pelo leitor” é incompatível com a asserção de sucesso (`:366`). |
| Login barrado / leitor | PG_hba reject; porta não anuncia pronto, env preservado por SHA, sem temporário: `bin/tests/test_sonda_sem_grant_pg.sh:275`–`:281`. | Snapshot do catálogo após falha: FALTA nessas asserções. |
| Impacto / saas_user | Role usada como administrador das fixtures: `bin/tests/test_sonda_sem_grant_pg.sh:162`; bootstrap é POSTGRES_USER: `:145`. | Prova de preservação de acesso legítimo **não-superuser**, ACL/defaults e estado de saas_user: FALTA; fixture não representa esse inventário. |

Mutantes e limites de mordida:

| Mutante | Asserção que morderia |
|---|---|
| Retirar REVOKE LO/definer/CONNECT (M1–M3) | Conferência pós-porta falha antes da credencial (`bin/sonda_provisionar.sh:172`–`:176`); selo acusa provisionamento/env (`bin/tests/test_sonda_sem_grant_pg.sh:223`–`:225`). A morte específica posterior “ESCRITA ACEITA” exige arnês adicional **[nao verificado]**. |
| Retirar chaves esc_lo/esc_func/outros_bancos de CONF_AUSENCIA (M4–M6) | Estragos deixam de ser recusados: cobrança rc5 e ausência de sonda (`bin/tests/test_sonda_sem_grant_pg.sh:242`–`:244`, `:266`–`:268`). |
| Retirar ADP leitor→PUBLIC (M7) | Reprovisionamento após default próprio precisa voltar rc0 (`bin/tests/test_sonda_sem_grant_pg.sh:372`, `:379`). |
| Reintroduzir SQL escrito na leitura | Injeta GRANT, SELECT INTO, CTE DELETE; cobra rc5, recusa falante e SQL impedido antes do psql (`bin/tests/test_sonda_sem_grant.sh:237`–`:248`). |
| Segredo em externo fora da lista/PATH absoluto | **FALTA** mordida universal; espiação enumera nomes e delega pelo PATH (`bin/tests/test_sonda_sem_grant.sh:306`–`:312`). |

Atomicidade global e idempotência de catálogo não são provadas pelos controles de env preservado: a role/CONNECT confirma em uma transação e o ACL em outra (`bin/sonda_provisionar.sh:118`, `:127`, `:168`); o selo preserva SHA do env, sem comparar o catálogo anterior (`bin/tests/test_sonda_sem_grant.sh:281`–`:300`).

## 09/11 13:41 · sessao b63b62a74363 · BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS · verificador · origem=Code

**P1–P5 da triagem v3 estão corrigidos nos sítios apontados, mas restam dois PROCEDENTES funcionais: fechamento do chamado por S127 e perda do motivo de `pendencia_admin`. M38 tem mordida estrutural, sem demonstrar diferença comportamental no cenário vazio.** Referências abaixo relativas a `app/`. (`chamados/models.py:1927`; `chamados/juizes.py:2138`; `chamados/services/materializacao.py:1375`; `chamados/tests/test_bug_par_intervalo.py:724`.)

**0. P1–P5 v3 — NAO PROCEDE que continuem abertos nesses pontos.**

| Item | Cura efetiva | Asserção discriminante |
|---|---|---|
| P1: retratadas | Vizinhança usa universo apurável. `chamados/services/materializacao.py:178`; `ponto/turnos.py:2866` | Exige aplicação e sequência canônica preservando retratada. `chamados/tests/test_bug_par_intervalo.py:176` |
| P2: descarimbo vazio | Compara os três campos antes de salvar. `chamados/models.py:3218` | Zero saves e agendamentos, duas vezes. `chamados/tests/test_bug_par_intervalo.py:492` |
| P3: fechamento | Conserva modalidade original e consulta C7 após trava. `chamados/models.py:1908`; `chamados/models.py:1927` | Chama `fechar`, exige consulta após trava e estado intacto. `chamados/tests/test_bug_par_intervalo.py:517`; `chamados/tests/test_bug_par_intervalo.py:523` |
| P4: espera prematura | Exige validação própria. `chamados/services/materializacao.py:109` | Cobra predicado, declaração e validação pelo administrador. `chamados/tests/test_bug_par_intervalo.py:533`; `chamados/tests/test_bug_par_intervalo.py:550`; `chamados/tests/test_bug_par_intervalo.py:566` |
| P5: desaparecimento | Detalhe inclui C6 viva **ou espera**. `chamados/services/acoes_chamado.py:349` | GET real cobra pergunta no contexto e aviso HTML. `chamados/tests/test_bug_par_intervalo.py:636`; `chamados/tests/test_bug_par_intervalo.py:641` |

Execução independente dos testes e causa efetiva das 44 mortes informadas: **[nao verificado]**. [nao verificado]

**1. Ato único — NAO PROCEDE para par identificado; a garantia não abrange todo motivo de intervalo.**

Validação, varredura e O330 convergem em `_materializar_uma`. O par entra inteiro em voo e no mesmo savepoint; ambas as aplicações precedem julgamento temporal, vizinhança, lastro, carimbos e reconciliação. (`chamados/services/validacao.py:474`; `chamados/services/materializacao.py:1593`; `chamados/services/passivo_validado.py:532`; `chamados/services/materializacao.py:1341`; `chamados/services/materializacao.py:1358`; `chamados/services/materializacao.py:1361`; `chamados/services/materializacao.py:1378`.)

O rastreamento AST com imports, apelidos e atribuições simples encontrou os plantadores de perguntas nos ramos hora/data-hora; hora é chamada pelo par ou pela avulsa. Chamadas dinâmicas por reflexão: **[nao verificado]**. (`chamados/services/materializacao.py:1014`; `chamados/services/materializacao.py:1095`; `chamados/services/materializacao.py:1359`; `chamados/services/materializacao.py:1475`; `chamados/models.py:4091`.)

**Há plantio individual:** `sem_par`, `par_ambiguo` e `marcos_sem_par` seguem pela avulsa; o teste exige plantio da metade cuja irmã já foi aplicada. Isso preserva esse comportamento explícito, não prova equivalência irrestrita com o HEAD anterior. (`chamados/services/materializacao.py:1428`; `chamados/tests/test_bug_par_intervalo.py:940`.)

**FILA:** regularização externa e veredito da célula admitem aplicação parcial em unidades próprias; não são o aceite deste par de perguntas. (`chamados/services/regularizacao_ext.py:172`; `chamados/services/regularizacao_ext.py:176`; `ponto/services/veredito_celula.py:322`; `ponto/services/veredito_celula.py:342`.)

**2. Rollback — comportamento conferido, com limite de prova após commit.** [nao verificado]

Após desfazer o savepoint, cada pergunta é relida. O resultado depende do lastro restaurado. (`chamados/services/materializacao.py:1384`; `chamados/services/materializacao.py:1388`.)

| Condição após rollback | Estado resultante |
|---|---|
| Sem lastro, dentro do fato | `materializada_em=None`, autor `None`, via `''`; recusa conjunta gravada; devolução limpa validação/autor para resposta de hora elegível. `chamados/models.py:3222`; `chamados/services/materializacao.py:1392`; `chamados/services/validacao.py:190`; `chamados/services/validacao.py:202` |
| Com lastro | Carimbo anterior preservado; recusa conjunta ainda é registrada; materialização preenchida impede devolução. `chamados/services/materializacao.py:1388`; `chamados/services/materializacao.py:1392`; `chamados/services/validacao.py:190` |
| Fora do fato | Excluída da identificação e protegida novamente no descarimbo. `chamados/services/materializacao.py:72`; `chamados/models.py:3215` |

Descarimbo vazio agora não salva. Descarimbo efetivo salva somente os três campos: agenda beco e emite evento da célula, mas não relavra veredito nem cria diretamente auditoria; a devolução acrescenta decisão e evento quando muda o estado. (`chamados/models.py:3224`; `chamados/signals.py:225`; `chamados/signals.py:244`; `ponto/portas/celula.py:891`; `chamados/services/validacao.py:208`; `chamados/services/validacao.py:224`.)

O censo estático continua vendo o sítio `save`; ausência de execução desse save é outra medida. (`core/censo_escritas.py:364`; `chamados/models.py:3226`.)

O teste cobra primeira tentativa com quatro saves/dois eventos e segunda com zero saves e foto idêntica. Entretanto, captura callbacks com `execute=False`: idempotência dos efeitos após commit real permanece **[nao verificado]**. (`chamados/tests/test_bug_par_intervalo.py:721`; `chamados/tests/test_bug_par_intervalo.py:726`; `chamados/tests/test_bug_par_intervalo.py:738`; `chamados/tests/test_bug_par_intervalo.py:745`.)

**FILA:** descarimbo/devolução não relavram o veredito persistido; remover timestamps não garante avanço do marco de staleness. Consequência concreta neste par e perda de carimbo legítimo: **[nao verificado]**. (`chamados/models.py:3226`; `chamados/services/validacao.py:205`; `chamados/veredito_lavrado.py:187`; `chamados/veredito_lavrado.py:212`.)

**3. Travas — NAO PROCEDE inversão entre os dois validadores pelo fluxo conferido.**

Ambos começam pelo Colaborador; depois travam Disputas, Chamados e Perguntas em ordem de PK. O330 inclui previamente as disputas fechadas dos itens. **FILA:** demonstração concorrente com duas conexões, **[nao verificado]**. (`chamados/services/materializacao.py:125`; `chamados/services/materializacao.py:134`; `chamados/models.py:2683`; `chamados/models.py:2692`; `chamados/models.py:2705`; `chamados/models.py:2709`; `chamados/services/passivo_validado.py:435`.)

**4. Leitores — PROCEDENTE: S127 libera o chamado enquanto C7 retém a disputa.**

Com metade aceita em espera e irmã fora do fato, C6 considera ambas mortas; C7 retém a disputa. Porém `disputa_viva_do_chamado` só consulta vazio/C6, e `transicionar` usa essa resposta. As guardas seguintes também excluem a validada. Assim, o caminho para `fechado` pode passar; a guarda adicional de premissa vale apenas para `resolvido`. (`chamados/juizes.py:1153`; `chamados/models.py:1864`; `chamados/juizes.py:2138`; `chamados/models.py:154`; `chamados/models.py:344`; `chamados/models.py:181`; `chamados/catalogo/motor.py:100`.)

**Cura:** S127 deve consumir C7, preservando sua exceção de disputa vazia; RED pela chamada real a `transicionar`, usando o cenário da irmã retratada. (`chamados/juizes.py:2138`; `chamados/tests/test_bug_par_intervalo.py:449`.)

Os demais leitores conferidos:

- Explicador, declaração, devolução, vigia de carimbo e O330 distinguem espera de falha. (`chamados/services/explicador.py:74`; `chamados/services/declaracao_texto.py:213`; `chamados/services/validacao.py:193`; `chamados/services/vigia_carimbo.py:109`; `chamados/services/passivo_validado.py:326`.)
- Painel/badge não creditam a metade validada; a irmã determina Validar/Cobrar, ou sobra Decidir no chamado acionável. (`chamados/services/raiox.py:72`; `chamados/services/raiox.py:137`; `chamados/catalogo/natureza.py:48`; `chamados/catalogo/natureza.py:61`.)
- Fio conserva perguntas da disputa aberta; beco consulta C7. (`chamados/services/fio.py:221`; `chamados/services/fio.py:253`; `chamados/services/becos.py:56`.)
- `o_que_segura` lê a porta alcançável. Vigia de fio continua julgando vitalidade por C6 e só depois abre `porta_aguardando_par`; portanto essa classe não elimina, por si, eventual alarme de fio morto. Contracaso executado: **[nao verificado]**. (`folha/fora_da_folha.py:77`; `chamados/services/vigia_fio.py:416`; `chamados/services/vigia_fio.py:430`; `chamados/services/vigia_fio.py:448`; `chamados/services/vigia_fio.py:462`.)

**5. Identidade — NAO PROCEDE derivação paralela.**

O identificador concentra elegibilidade, multiplicidade e abstenção por duas pausas; lê pausas da autoridade existente. A tag apenas delega ao predicado de espera. `par_ambiguo` segue avulsa; o teste cobra irmãs intactas nesse cenário. (`chamados/services/materializacao.py:70`; `chamados/services/materializacao.py:77`; `chamados/services/materializacao.py:87`; `escala/models.py:1189`; `escala/models.py:1197`; `chamados/templatetags/chamados_tags.py:19`; `chamados/tests/test_bug_par_intervalo.py:329`.)

**6. Texto/tela — NAO PROCEDE nos caminhos conferidos.**

“Colada” exige testemunha viva de mesmo tipo em ±60 segundos inclusivos, pela mesma função usada na porta. Espera tem entrada própria de catálogo. Detalhe e modal incluem o mesmo partial; os testes agora cobram GET real das duas telas. Renderização executada nesta revisão: **[nao verificado]**. (`ponto/registro_batida.py:94`; `ponto/registro_batida.py:115`; `chamados/services/materializacao.py:1354`; `chamados/catalogo/recusa.py:25`; `templates/chamados/partials/_pergunta_card.html:29`; `templates/chamados/partials/modal_fio.html:160`; `chamados/tests/test_bug_par_intervalo.py:623`.)

**7. Classes estruturais e semântica.**

**PROCEDENTE — espera e limpeza destroem motivo de pendência técnica.** O ramo hora grava `pendencia_admin` junto de seu motivo e pode continuar aplicando. Depois, o par apaga qualquer motivo, deixando exatamente o estado acusado por I19. A primeira espera também pode sobrescrever motivo anterior. Ocorrência em produção: **[nao verificado]**. (`chamados/services/materializacao.py:477`; `chamados/services/materializacao.py:937`; `chamados/services/materializacao.py:1375`; `chamados/services/materializacao.py:1433`; `chamados/services/materializacao.py:1190`; `chamados/management/commands/auditar_invariantes_chamados.py:1112`.)

**Cura:** preservar o par pendência/motivo tanto ao registrar quanto ao retirar espera; não zerar pendência automaticamente. RED deve aplicar par com pendência anterior e com pendência produzida durante plantio, cobrando motivo preservado e I19 vazio. (`chamados/services/materializacao.py:937`; `chamados/services/materializacao.py:1375`; `chamados/services/materializacao.py:1433`.)

M7 agora isola multiplicidade com DNA de uma pausa; M35 cobra o HTML específico do include. **NAO PROCEDE** a vacuidade anterior desses dois. (`chamados/tests/test_bug_par_intervalo.py:307`; `chamados/tests/test_bug_par_intervalo.py:315`; `chamados/tests/test_bug_par_intervalo.py:664`.)

**PROCEDENTE como limitação de prova:** M38 morre pela chamada observada a `descarimbar`. Com os três campos vazios, a guarda antiga produz o mesmo estado/trilha após P2. Para sustentar cura funcional, falta contracaso em que a guarda antiga deixe estado errado; alternativamente, rotular esse selo como estrutural. (`chamados/tests/mutantes_bug_par_intervalo.py:134`; `chamados/tests/test_bug_par_intervalo.py:724`; `chamados/models.py:3218`.)

**FILA:** relógio continua lido por metade na aprovação e no carimbo; não há instante único propagado pelo ato. Também permanece a diferença entre competência pelo dia do turno no O330 e pelo dia civil na porta. (`chamados/services/materializacao.py:1020`; `chamados/services/materializacao.py:1250`; `chamados/services/passivo_validado.py:113`; `ponto/janelas.py:84`.)

**8. Mapa adicional de escritores/leitores.**

| Campo/valor | Escritores conferidos | Leitores conferidos |
|---|---|---|
| Resposta/instante | `chamados/services/disputa_emissao.py:1464`; `chamados/models.py:3141` | `chamados/services/materializacao.py:110`; `chamados/services/raiox.py:136` |
| Validação/autor | `chamados/services/validacao.py:134`; `chamados/services/validacao.py:202`; `chamados/services/disputa_emissao.py:1477`; `chamados/models.py:3144` | `chamados/juizes.py:1153`; `chamados/models.py:1851`; `chamados/services/painel.py:67` |
| Materialização/autor/via | `chamados/models.py:3108`; `chamados/models.py:3194`; `chamados/models.py:3222`; `chamados/models.py:3280`; `chamados/services/relavra_via.py:125` | `chamados/juizes.py:660`; `chamados/services/validacao.py:190`; `chamados/veredito_lavrado.py:187` |
| Recusa/pendência | `chamados/services/materializacao.py:937`; `chamados/services/materializacao.py:1190`; `chamados/services/materializacao.py:1375`; `chamados/services/validacao.py:605`; `chamados/services/disputa_emissao.py:1494`; `chamados/models.py:3197` | `chamados/services/materializacao.py:112`; `chamados/services/validacao.py:195`; `chamados/services/explicador.py:98`; `chamados/management/commands/auditar_invariantes_chamados.py:1112` |
| Dia/contexto/marcos | `chamados/services/fio.py:800`; `chamados/models.py:1123` | `chamados/services/materializacao.py:425`; `chamados/services/materializacao.py:428`; `chamados/services/materializacao.py:490`; `chamados/services/declaracao_texto.py:468` |
| Batida: instante/tipo/origem/vínculo/retração | `ponto/registro_batida.py:159`; `chamados/services/materializacao.py:1035`; `ponto/registro_batida.py:276` | `ponto/registro_batida.py:99`; `ponto/turnos.py:2866`; `chamados/juizes.py:734` |
| Espera/projeção | `chamados/services/materializacao.py:1433`; `chamados/services/declaracao_texto.py:485` | `chamados/models.py:1864`; `chamados/templatetags/chamados_tags.py:19`; `api/views.py:1126`; `templates/chamados/partials/_card_atendimento.html:215` |
| Lavra/trilha/eventos | `chamados/veredito_lavrado.py:250`; `chamados/services/validacao.py:208`; `ponto/portas/celula.py:891` | `chamados/veredito_lavrado.py:204`; `chamados/services/raiox.py:133`; `chamados/signals.py:244` |

Exaustividade transitiva de todos os acessos dinâmicos, suite completa, concorrência real e efeitos em produção: **[nao verificado]**. [nao verificado]

## 09/11 13:39 · sessao f2e59b370272 · - · - · origem=Ronald (tmux)

**PROCEDENTE — limpeza do par deixa pendência sem motivo.** O ramo hora pode gravar `pendencia_admin` e `recusa_motivo` juntos e continuar aplicando; após sucesso, o par apaga qualquer `recusa_motivo`, sem limpar nem resolver a pendência. O resultado satisfaz exatamente o filtro de violação I19. (`app/chamados/services/materializacao.py:468`; `:477`; `:503`; `:934`; `:937`; `:1359`; `:1374`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1111`.)

Também alcança pendência anterior: identidade e prontidão não a excluem. Além disso, a primeira metade em espera já pode sobrescrever seu motivo com `ESPERA_PAR_INTERVALO`. (`app/chamados/services/materializacao.py:67`; `:77`; `:1412`; `:1431`; `app/chamados/services/materializacao.py:1180`; `:1190`.)

**Cura sugerida:** preservar o motivo pertencente a `pendencia_admin` tanto ao registrar espera quanto ao limpar espera após aplicação; retirar apenas o marcador de espera sem apagar pendência legítima. RED: par com pendência/motivo anteriores ou produzidos durante plantio; exigir preservação do motivo e ausência de I19 após aplicar. (`app/chamados/services/materializacao.py:937`; `:1374`; `:1433`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1112`.)

Ocorrência em produção: **[nao verificado]**.

## 09/11 13:39 · sessao ae987bd8c0fe · SONDA-SEM-GRANT · verificador · origem=Code

**Não fecharia V7 como garantia de nenhuma escrita, nem V7b como preservação integral de ordem e origem.** O selo aceita alterações persistentes feitas pelo próprio leitor; o transporte ainda separa stdout direto de stderr processado. (`bin/tests/test_sonda_sem_grant_pg.sh:365`; `bin/tests/test_sonda_sem_grant_pg.sh:376`; `bin/codex_pergunta.sh:322`)

Resultados executados dos selos, RED/GREEN e mortes reais dos mutantes: **[nao verificado]**. Abaixo avalio código e asserções, sem usar entrega ou triagem como prova.

1. **V7a: capacidades e mordida do selo**

O selo agora constrói uma conexão TCP autenticada como `leitor`, com `ON_ERROR_STOP`, e exige leitura bem-sucedida nos dois destinos antes das negativas. Isso fecha a lacuna anterior de testar apenas como administrador. (`bin/tests/test_sonda_sem_grant_pg.sh:301`; `bin/tests/test_sonda_sem_grant_pg.sh:305`; `bin/tests/test_sonda_sem_grant_pg.sh:309`; `bin/tests/test_sonda_sem_grant_pg.sh:312`)

| Via | O que o código e o selo sustentam |
|---|---|
| Large objects | A porta revoga todas as assinaturas das **dez famílias nomeadas**. O selo tenta apenas `lo_create`, `lo_from_bytea` e `lo_creat`; faltam tentativas das outras sete famílias. (`bin/sonda_leitor_comum.sh:19`; `bin/sonda_provisionar.sh:156`; `bin/tests/test_sonda_sem_grant_pg.sh:327`) |
| SECURITY DEFINER | Revogação e conferência cobrem rotinas fora dos schemas de sistema. Há tentativa autenticada de função que insere. Isso não certifica funções de sistema nem qualquer função com efeito colateral. (`bin/sonda_provisionar.sh:157`; `bin/sonda_leitor_comum.sh:139`; `bin/tests/test_sonda_sem_grant_pg.sh:336`) |
| Função volátil invoker | Há uma tentativa de INSERT por função invoker. A conferência não classifica rotinas por `provolatile`; esse caso não prova ausência de outros efeitos de funções voláteis. (`bin/tests/test_sonda_sem_grant_pg.sh:177`; `bin/tests/test_sonda_sem_grant_pg.sh:337`; `bin/sonda_leitor_comum.sh:136`; `bin/sonda_leitor_comum.sh:139`) |
| DML, TEMP e CREATE | Existem tentativas autenticadas de INSERT/UPDATE/DELETE, TEMP e criação nos dois schemas. (`bin/tests/test_sonda_sem_grant_pg.sh:330`; `bin/tests/test_sonda_sem_grant_pg.sh:333`; `bin/tests/test_sonda_sem_grant_pg.sh:335`) |
| COPY e arquivo do servidor | Há casos de COPY TO PROGRAM, COPY FROM arquivo e `pg_read_file`; não é cobertura de todas as formas de COPY. (`bin/tests/test_sonda_sem_grant_pg.sh:339`; `bin/tests/test_sonda_sem_grant_pg.sh:341`) |
| `set_config` / ALTER próprio | O caso `set_config` cobra a recusa do **INSERT posterior**, não a recusa da configuração. A seção seguinte exige sucesso de ALTER próprio e só depois detecção/cura. (`bin/tests/test_sonda_sem_grant_pg.sh:338`; `bin/tests/test_sonda_sem_grant_pg.sh:365`; `bin/tests/test_sonda_sem_grant_pg.sh:368`) |
| Outros bancos | Há teste de recusa de nova conexão em `postgres` e `template1`, nos dois clusters. O provisionamento revoga privilégios, sem encerrar sessões anteriores; existência dessas sessões: **[nao verificado]**. (`bin/tests/test_sonda_sem_grant_pg.sh:345`; `bin/tests/test_sonda_sem_grant_pg.sh:350`; `bin/sonda_provisionar.sh:117`) |
| Sequências | A conferência rejeita privilégios diferentes de SELECT; há estrago administrativo concedendo USAGE. Não há tentativa autenticada de `nextval` ou `setval`. (`bin/sonda_leitor_comum.sh:122`; `bin/tests/test_sonda_sem_grant_pg.sh:255`; `bin/tests/test_sonda_sem_grant_pg.sh:327`) |
| NOTIFY, LISTEN, advisory lock, prepared transaction, dblink/extensões, vacuum/analyze | Não aparecem nas tentativas autenticadas. Aceitação, recusa e configuração dessas vias nos clusters fornecidos: **[nao verificado]**. (`bin/tests/test_sonda_sem_grant_pg.sh:327`; `bin/tests/test_sonda_sem_grant_pg.sh:341`; `bin/tests/test_sonda_sem_grant_pg.sh:376`) |

As negativas existentes cobram rc diferente de zero, ausência de `ESCREVEU`, mensagem `permission denied` e efeito persistente zero quando há consulta de efeito. É mordida útil para esses casos, não prova das vias omitidas. (`bin/tests/test_sonda_sem_grant_pg.sh:317`; `bin/tests/test_sonda_sem_grant_pg.sh:319`; `bin/tests/test_sonda_sem_grant_pg.sh:324`)

O classificador `_sql` não fecha essas omissões: ele classifica o SQL administrativo do envelope; o Python da sonda executa separadamente, com conexão própria. (`bin/sonda_leitura.sh:158`; `bin/sonda_leitura.sh:166`; `bin/sonda_leitura.sh:233`; `bin/sonda_leitura.sh:239`)

| Mutantes relatados | Asserção identificável; morte executada **[nao verificado]** |
|---|---|
| M1–M3: retirar REVOKE LO/definer/CONNECT | A conferência da porta já deve reprovar **antes de publicar a credencial**. No selo fresco, isso atinge primeiro as cobranças de provisionamento e arquivo. Não comprova a morte específica posterior por “ESCRITA ACEITA”; seria necessário demonstrar o arnês que permite chegar ali autenticado. (`bin/sonda_provisionar.sh:173`; `bin/sonda_provisionar.sh:196`; `bin/tests/test_sonda_sem_grant_pg.sh:223`; `bin/tests/test_sonda_sem_grant_pg.sh:225`; `bin/tests/test_sonda_sem_grant_pg.sh:303`) |
| M4–M6: retirar chaves de `CONF_AUSENCIA` | Os estragos correspondentes cobram rc 5, chave positiva e ausência de `SONDA_RODOU`. (`bin/tests/test_sonda_sem_grant_pg.sh:242`; `bin/tests/test_sonda_sem_grant_pg.sh:244`; `bin/tests/test_sonda_sem_grant_pg.sh:266`) |
| M7: retirar cura global do default próprio | Há caso que concede INSERT a PUBLIC e cobra sucesso do reprovisionamento. Um mutante semanticamente válido deve atingir essa cobrança. (`bin/tests/test_sonda_sem_grant_pg.sh:372`; `bin/tests/test_sonda_sem_grant_pg.sh:379`; `bin/sonda_provisionar.sh:150`) |
| M8: stderr novamente colado no fim | A comparação da sequência e a exigência de `[stderr]` atingem essa regressão. (`bin/tests/test_codex_sonda.sh:105`; `bin/tests/test_codex_sonda.sh:107`) |

2. **Conferência: escritores, leitores e significado**

É preciso distinguir **quem produz a chave textual** de **quem modifica o estado consultado**. (`bin/sonda_leitor_comum.sh:79`; `bin/sonda_provisionar.sh:117`; `bin/sonda_provisionar.sh:156`)

| Elemento | Quem escreve/define | Quem lê |
|---|---|---|
| `LO_ESCRITA` | Definição única na biblioteca. (`bin/sonda_leitor_comum.sh:19`) | Consulta e revogação. (`bin/sonda_leitor_comum.sh:77`; `bin/sonda_provisionar.sh:156`) |
| `esc_lo` | `_q_conferencia` produz o valor; a porta revoga o ACL correspondente. (`bin/sonda_leitor_comum.sh:136`; `bin/sonda_provisionar.sh:156`) | `CONF_AUSENCIA` → `_conferencia_pronta`. (`bin/sonda_leitor_comum.sh:155`; `bin/sonda_leitor_comum.sh:161`) |
| `esc_func` | `_q_conferencia` produz o valor; a porta revoga EXECUTE. (`bin/sonda_leitor_comum.sh:139`; `bin/sonda_provisionar.sh:157`) | Mesmo parser/predicado. (`bin/sonda_leitor_comum.sh:155`; `bin/sonda_leitor_comum.sh:168`) |
| `outros_bancos` | `_q_conferencia` produz o valor; a porta revoga CONNECT de PUBLIC e privilégios do leitor. (`bin/sonda_leitor_comum.sh:143`; `bin/sonda_provisionar.sh:117`) | Mesmo parser/predicado. (`bin/sonda_leitor_comum.sh:155`; `bin/sonda_leitor_comum.sh:168`) |
| `CONF_AUSENCIA` | Uma definição. (`bin/sonda_leitor_comum.sh:155`) | `_conferencia_pronta`, exigindo cada chave presente e igual a zero. (`bin/sonda_leitor_comum.sh:161`; `bin/sonda_leitor_comum.sh:164`; `bin/sonda_leitor_comum.sh:168`) |

Envelope e porta importam a mesma biblioteca e usam a mesma consulta; a leitura exige prontidão antes da execução, e a porta exige prontidão depois das alterações. (`bin/sonda_leitura.sh:48`; `bin/sonda_leitura.sh:184`; `bin/sonda_leitura.sh:187`; `bin/sonda_provisionar.sh:42`; `bin/sonda_provisionar.sh:172`; `bin/sonda_provisionar.sh:173`)

O parser próprio localizado é `_chave`, **instrumentação do selo**; o predicado de prontidão desse selo delega à biblioteca. Não identifiquei outro julgador operacional dessa conferência. (`bin/tests/test_sonda_sem_grant_pg.sh:216`; `bin/tests/test_sonda_sem_grant_pg.sh:217`)

O significado é igual nos dois destinos: `_destino` troca cluster/banco, e as consultas usam `current_database()`. As três novas chaves contam **itens ACL**, podendo contar PUBLIC e leitor separadamente; não são números distintos de funções ou bancos. (`bin/sonda_leitor_comum.sh:28`; `bin/sonda_leitor_comum.sh:29`; `bin/sonda_leitor_comum.sh:136`; `bin/sonda_leitor_comum.sh:143`)

A frase “só a porta escreve todo esse estado” seria excessiva: o próprio selo exige que o leitor altere senha, configurações e default privileges. (`bin/tests/test_sonda_sem_grant_pg.sh:365`; `bin/tests/test_sonda_sem_grant_pg.sh:376`)

3. **Laço noturno e estado parcialmente aplicado** [nao verificado]

Pelo censo fornecido, o laço alcança `juliani_ref_1810`, `postgres` e `template1`, excluindo `sombra`; não há outro LOGIN não-superuser informado além do leitor. Assim, **dano atual a outro login legítimo não está demonstrado**. Censo independentemente conferido nesta cópia: **[nao verificado]**. (`bin/sonda_provisionar.sh:117`)

O laço não contém exclusão de `juliani_ref_*` nem preservação de CONNECT herdado de PUBLIC para outros usuários. Portanto, em outro inventário, pode retirar acesso legítimo dependente de PUBLIC; uso de roles NOLOGIN por `SET ROLE` e clientes externos: **[nao verificado]**. (`bin/sonda_provisionar.sh:117`)

Há duas granularidades de atomicidade:

- **Dentro do laço:** DO inteiro em BEGIN/COMMIT, com `ON_ERROR_STOP`; falha não deixa metade desses REVOKEs confirmada. (`bin/sonda_provisionar.sh:51`; `bin/sonda_provisionar.sh:105`; `bin/sonda_provisionar.sh:117`; `bin/sonda_provisionar.sh:118`)
- **No provisionamento completo:** role/CONNECT confirmam antes da transação de ACL. Falha posterior pode conservar as primeiras alterações, sem promoção do envfile ou anúncio de pronto. (`bin/sonda_provisionar.sh:118`; `bin/sonda_provisionar.sh:127`; `bin/sonda_provisionar.sh:169`; `bin/sonda_provisionar.sh:183`; `bin/sonda_provisionar.sh:196`)

O cron chama `--refazer && --bloco`. O refazer invalida o carimbo antes do drop, e erro da porta interrompe antes do carimbo final: **pode deixar a sombra vermelha e impedir o bloco, mas esse caminho não conserva o verde anterior**. (`app/config/crons.py:351`; `bin/sombra.sh:288`; `bin/sombra.sh:265`; `bin/sombra.sh:340`; `bin/sombra.sh:341`)

Conferência verde também não certifica disponibilidade de outros usuários: suas contagens verificam privilégios de leitor/PUBLIC. (`bin/sonda_leitor_comum.sh:136`; `bin/sonda_leitor_comum.sh:143`)

4. **V7b: ordem, origem, rc e segredo**

**Ordem integral: não.** A sincronização garante que a pipeline termine antes da raspagem, mas stdout escreve diretamente enquanto stderr atravessa `tee` e `sed`. A ordem entre esses caminhos pode inverter; linhas parciais também podem se concatenar. O selo apenas intercala linhas completas com pausas de 0,3 segundo. (`bin/codex_pergunta.sh:322`; `bin/codex_pergunta.sh:333`; `bin/tests/test_codex_sonda.sh:53`; `bin/tests/test_codex_sonda.sh:54`)

**Origem integral: não.** O próprio envelope já mistura stdout/stderr do processo da sonda com `2>&1`. Ambos chegam ao wrapper como stdout; `[stderr]` identifica apenas stderr recebido na fronteira do envelope. (`bin/sonda_leitura.sh:240`; `bin/codex_pergunta.sh:322`)

**Rc do envelope: sim, nas execuções normais da pipeline.** A captura imediata de `PIPESTATUS[0]` preserva 0, 5 e demais retornos; o wrapper não usa `errexit`. Porém, ignora os retornos de `tee` e `sed`: envelope rc 0 pode continuar após falha desses estágios. (`bin/codex_pergunta.sh:54`; `bin/codex_pergunta.sh:322`; `bin/codex_pergunta.sh:326`)

**Rc 5 continua impedindo o bruto de chegar ao Codex:** sai antes da raspagem e da inclusão do dado no prompt. A sonda real que retorna 5 é remapeada para 1 pelo envelope. (`bin/codex_pergunta.sh:325`; `bin/codex_pergunta.sh:333`; `bin/codex_pergunta.sh:359`; `bin/sonda_leitura.sh:244`)

**Segredo não tem garantia geral de retenção:**

- `tee` grava stderr cru em `envelope.err`; o diretório temporário recebe modo 700 e há tentativa de limpeza no EXIT. (`bin/codex_pergunta.sh:322`; `bin/codex_pergunta.sh:273`; `bin/codex_pergunta.sh:267`)
- No rc 5, qualquer linha iniciada por `sonda_leitura:` sai **sem raspagem**. Um segredo nessa linha passaria pelo filtro; ocorrência real: **[nao verificado]**. (`bin/codex_pergunta.sh:324`)
- No rc 0, a máscara trata nomes/CPF, não senhas arbitrárias. O selo de recusa só coloca os segredos fora das linhas prefixadas. (`bin/nome_no_texto.py:87`; `bin/nome_no_texto.py:110`; `bin/tests/test_codex_sonda.sh:42`; `bin/tests/test_codex_sonda.sh:44`)

5. **As quatro decisões abertas**

| Decisão | Classificação |
|---|---|
| **1 — aceitar ALTER próprio e detectar depois** | **PROCEDENTE; bloqueia a garantia absoluta.** Conserva escritor do estado fora da porta; detecção posterior não é impedimento. O selo exige essa escrita bem-sucedida, enquanto o envelope promete impossibilidade de acidente. (`CLAUDE.md:32`; `bin/tests/test_sonda_sem_grant_pg.sh:366`; `bin/sonda_leitura.sh:8`) |
| **2 — SECURITY DEFINER criada depois** | **DESCARTE como falso pronto apenas por nascer entre provisionamento e próxima conferência:** há caso exigindo recusa. **Permanece lacuna de garantia contínua:** conferir antes de executar não impede mudança depois da conferência; essa concorrência não é ensaiada. (`bin/tests/test_sonda_sem_grant_pg.sh:242`; `bin/tests/test_sonda_sem_grant_pg.sh:266`; `bin/sonda_leitura.sh:187`; `bin/sonda_leitura.sh:233`) |
| **3 — lista LO dependente da versão** | **DESCARTE como defeito futuro já demonstrado.** Função futura omitida: **[nao verificado]**. **PROCEDENTE a cobertura atual incompleta:** sete famílias já revogadas não têm tentativa autenticada. (`bin/sonda_leitor_comum.sh:19`; `bin/tests/test_sonda_sem_grant_pg.sh:327`) |
| **4 — CONNECT amplo na sombra** | **DESCARTE do dano atual alegado a outro LOGIN**, pelo censo fornecido; dano real: **[nao verificado]**. **PROCEDENTE o efeito amplo e a aplicação global não atômica**, sem demonstração de falso verde nesse caminho. (`bin/sonda_provisionar.sh:117`; `bin/sonda_provisionar.sh:118`; `bin/sonda_provisionar.sh:127`; `bin/sombra.sh:288`) |

Não identifiquei nessas quatro decisões derivação paralela ou relógio fora da borda. Os problemas demonstráveis pertencem à escrita fora da porta, cobertura insuficiente e fidelidade incompleta do instrumento; essas classes exigem tratamento, não mera aceitação como decisão aberta. (`CLAUDE.md:28`; `CLAUDE.md:32`; `CLAUDE.md:370`; `CLAUDE.md:373`)

6. **Lista final**

| Item | Veredito |
|---|---|
| “Leitor impossibilitado de qualquer escrita/efeito” | **PROCEDENTE manter aberto:** ALTER próprio é aceito pelo contrato do selo e várias vias não são medidas. (`bin/tests/test_sonda_sem_grant_pg.sh:366`; `bin/tests/test_sonda_sem_grant_pg.sh:327`) |
| “Cada via fechada tem tentativa e mutante específico comprovado” | **DESCARTE:** cobertura LO parcial; M1–M3 falham antes da credencial; execuções **[nao verificado]**. (`bin/tests/test_sonda_sem_grant_pg.sh:223`; `bin/tests/test_sonda_sem_grant_pg.sh:303`; `bin/tests/test_sonda_sem_grant_pg.sh:327`) |
| “Há outro juiz operacional da conferência” | **DESCARTE no código localizado:** consumidores delegam ao predicado comum; `_chave` é instrumentação. (`bin/sonda_leitura.sh:187`; `bin/sonda_provisionar.sh:173`; `bin/tests/test_sonda_sem_grant_pg.sh:217`) |
| “Laço pode deixar metade dos bancos confirmada” | **DESCARTE dentro da mesma transação; PROCEDENTE para o provisionamento completo em duas transações.** (`bin/sonda_provisionar.sh:105`; `bin/sonda_provisionar.sh:118`; `bin/sonda_provisionar.sh:127`) |
| “Falha da porta mantém verde anterior da sombra” | **DESCARTE nesse caminho:** invalidação precede a mudança e publicação final depende da porta. (`bin/sombra.sh:288`; `bin/sombra.sh:340`; `bin/sombra.sh:341`) |
| “V7b preserva ordem e origem de cada linha” | **PROCEDENTE a reprovação:** caminhos concorrentes e mistura anterior no envelope. (`bin/codex_pergunta.sh:322`; `bin/sonda_leitura.sh:240`) |
| “Rc 5 entrega bruto ao Codex” | **DESCARTE:** saída anterior à raspagem/prompt. (`bin/codex_pergunta.sh:325`; `bin/codex_pergunta.sh:333`) |
| “Falha de transporte ou segredo sempre fecha a passagem” | **PROCEDENTE a lacuna:** rc de filtros ignorado e linhas prefixadas emitidas cruas. Vazamento ocorrido: **[nao verificado]**. (`bin/codex_pergunta.sh:322`; `bin/codex_pergunta.sh:324`) |

## 09/11 13:37 · sessao 80205edd1ecf · - · - · origem=Ronald (tmux)

**PROCEDENTE — S127 ainda usa C6 e permite encerrar o chamado-pai com intervalo em espera.** `ChamadoColaborador.transicionar` chama `disputa_viva_do_chamado`; esse juiz só retém disputa vazia ou com `fio_vivo`, enquanto C6 mata imediatamente a pergunta validada. Com a irmã fora do fato, C7 retém a disputa, mas S127 devolve `None` e o gate seguinte de ancoradas também só procura C6 vivas. Cura: S127 consultar a condição canônica C7, preservando o caso da disputa vazia; RED chamar `transicionar` sobre o cenário da irmã retratada. `app/chamados/models.py:148`, `app/chamados/models.py:165`, `app/chamados/models.py:344`, `app/chamados/models.py:368`, `app/chamados/juizes.py:1153`, `app/chamados/juizes.py:2134`, `app/chamados/juizes.py:2138`, `app/chamados/models.py:1863`.

**NAO PROCEDE — leitores principais migram a espera antes de diagnosticar falha/devolver.** Explicador retorna `aguardando_par`; declaração retorna o mesmo estado antes da guarda de validação; devolução sai sem apagar aceite; vigia separa espera de dívida; O330 exclui espera do alvo. `app/chamados/services/explicador.py:74`, `app/chamados/services/declaracao_texto.py:213`, `app/chamados/services/declaracao_texto.py:382`, `app/chamados/services/declaracao_texto.py:678`, `app/chamados/services/validacao.py:193`, `app/chamados/services/vigia_carimbo.py:109`, `app/chamados/services/vigia_carimbo.py:133`, `app/chamados/services/passivo_validado.py:327`.

**Leitores restantes, alcance preciso.**

- C6 continua dizendo “morta”; propriedade `estado` diz `VALIDADA` enquanto não materializada; C7 acrescenta a retenção da espera. `app/chamados/juizes.py:1153`, `app/chamados/models.py:3050`, `app/chamados/models.py:1864`.
- Painel/badge creditam apenas `q_vivas`, portanto a metade aceita não conta: irmã respondida gera Validar; irmã sem resposta gera Cobrar; sem outra viva, chamado acionável vira Decidir. Isso preserva uma ação no chamado vivo, mas não produz um contador específico de espera. `app/chamados/services/raiox.py:72`, `app/chamados/services/raiox.py:133`, `app/chamados/services/raiox.py:137`, `app/chamados/services/verbos.py:34`, `app/chamados/catalogo/natureza.py:48`, `app/chamados/catalogo/natureza.py:54`, `app/chamados/catalogo/natureza.py:61`.
- Fio carrega todas as perguntas de disputas abertas, inclusive aceita; de disputa fechada carrega só C6 vivas. Não reabre nesse carregamento. `app/chamados/services/fio.py:221`, `app/chamados/services/fio.py:224`, `app/chamados/services/fio.py:238`, `app/chamados/services/fio.py:253`.
- `o_que_segura` lê a porta alcançável e seu `fio`; não lê C7 nem espera diretamente. Vigia antigo usa C6 para fio vivo; pode classificar dia sem C6 viva como `fio_morto`, mesmo tendo `porta_aguardando_par`, se faltarem as outras saídas (resolver dia/ausência/lista admin). Não é contraprova de produção: depende desses insumos. `app/folha/fora_da_folha.py:221`, `app/folha/fora_da_folha.py:77`, `app/chamados/services/vigia_fio.py:416`, `app/chamados/services/vigia_fio.py:430`, `app/chamados/services/vigia_fio.py:448`, `app/chamados/services/vigia_fio.py:462`, `app/chamados/services/vigia_fio.py:305`.
- Beco/evento e cron respeitam C7; vigias de fio/carimbo estão declarados como vigias. `app/chamados/services/becos.py:33`, `app/chamados/services/becos.py:56`, `app/config/crons.py:2044`, `app/config/crons.py:2213`, `app/config/crons.py:2226`.

**NAO PROCEDE — duas cascas administrativas recebem o aviso.** Detalhe conserva a aceita por `C6 ou espera`; modal carrega perguntas da disputa aberta; ambos incluem o mesmo partial que chama diretamente o predicado. GETs reais estão nos testes; execução nesta revisão: [nao verificado]. `app/chamados/services/acoes_chamado.py:349`, `app/chamados/views.py:549`, `app/templates/chamados/detalhe_local.html:310`, `app/chamados/services/fio.py:253`, `app/templates/chamados/partials/modal_fio.html:160`, `app/templates/chamados/partials/_pergunta_card.html:29`, `app/templates/chamados/partials/_espera_par_intervalo.html:2`, `app/chamados/templatetags/chamados_tags.py:19`, `app/chamados/tests/test_bug_par_intervalo.py:623`.

**Mapa adicional de escrita/leitura fora do diff.**

| Campo | Escritores conferidos | Leitores conferidos |
|---|---|---|
| `resposta_colab/respondida_em` | resposta core `disputa_emissao.py:1464`; complemento de carimbo `models.py:3140`; reabertura `disputa_emissao.py:1590` | C6 `juizes.py:1169`; estado `models.py:3053`; espera `materializacao.py:110`; crédito `raiox.py:136` |
| `validada_em/por` | aceite `validacao.py:134`; devolução `validacao.py:202`; nova resposta `disputa_emissao.py:1477`; complemento `models.py:3144`; descarimbo em lote `models.py:1565` | C6 `juizes.py:1153`; C7 `models.py:1851`; estado `models.py:3050`; espera `materializacao.py:109`; vigia `vigia_carimbo.py:100`; placar `painel.py:67` |
| materialização/via | `models.py:3108`; identidade `models.py:3192`; descarimbo `models.py:3222`; lote `models.py:3280`; relavra via `relavra_via.py:125` | lastro `juizes.py:660`; C6 `juizes.py:1164`; estado `models.py:3052`; devolução `validacao.py:190`; apresentação `materializacao.py:515` |
| `recusa_motivo` | `materializacao.py:1190`; limpeza do par `materializacao.py:1375`; recusa humana `validacao.py:605`; escape `disputa_emissao.py:1494`; identidade `models.py:3197` | espera `materializacao.py:112`; devolução `validacao.py:195`; explicador `explicador.py:98`; auditor I18 `auditar_invariantes_chamados.py:1075` |
| `contexto_json/data_evento/timestamp` | contexto nascimento `models.py:1123`; batida porta `registro_batida.py:155` | data plantio `materializacao.py:425`; fallback contexto disputa `materializacao.py:428`; datetime aware `materializacao.py:495`; ajuste overnight `materializacao.py:502`; tolerância `registro_batida.py:99` |

Caminhos acima são os conferidos; exaustividade transitiva de todos os escritores/leitores da árvore: **[nao verificado]**.

_3167 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
