# CODEX-ESTADO

> Ultima resposta processada: **17/11/2026 14:09** · publicado em **17/11/2026 14:09** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 17/11 14:09 · sessao 56328c467ec4 · - · - · origem=Ronald (tmux)

ITEM 2 — AVAL-DEVOLVIDO

1. Premissas: parcialmente. O POST só consulta a porta e devolve recusa (`app/folha/views.py:124-133`); o escritor existente lavra `MetricaSnapshot` por empresa/dia via `update_or_create`, sem tentativa individual (`app/folha/porta_export.py:768-796`). O comando aceita somente `--mes` e `--ano`, portanto a distinção “manual sem `--tentativa`” não existe nesta cópia (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:27-29`). A medição está explicitamente declarada como não agendada, contrariando a premissa de cron existente (`app/config/crons.py:887-897`). A exceção de `medir` imprime motivo e termina antes de `lavrar`; não deixa registro FALHOU (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:86-95`).

2. Lei vigente: escritor único está alinhado à L-007 e ao contrato estrutural (`app/docs/LEIS.md:65`; `CLAUDE.md:178`). A declaração atual de cron é “não agendar”; o aval precisa reconciliar isso para tornar R9 executável (`app/config/crons.py:887-897`). Não há estados FALHOU/DIVERGENTE demonstrados no escritor atual, cujo valor é número de falhas ou -1 (`app/folha/porta_export.py:772-776`).

3. Literal: “toda execução cria a tentativa” não define se a unidade é invocação ou empresa. O comando percorre empresas, lavra cada uma e aborta na primeira exceção; uma invocação pode ter empresas já lavradas e outras não medidas (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:86-95`). O carimbo atual sobrescreve a mesma empresa/dia inclusive quando muda competência, pois competência só está em `defaults` (`app/folha/porta_export.py:772-776`). A unidade do registro e o cron de R9 precisam ser identificados antes da construção.

4. Dinheiro/exportada/!: a escrita examinada é de métrica, sem alteração de `FechamentoMensal` (`app/folha/porta_export.py:768-796`). Tocar gravado exportado exigiria `!` e a porta específica, fora dessa escrita (`CLAUDE.md:411-417`). Impacto real em produção: [não verificado].

ITEM 2: PROCEDE — premissas de cron e `--tentativa` não correspondem à cópia, e “toda execução” deixa a unidade do registro indefinida (`app/config/crons.py:887-897`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:27-29`, `:86-95`).

## 17/11 14:00 · sessao 3f14516fb790 · PORTA-EXPORT · verificador · origem=Code

**G ainda tem uma regressão no reinício: a limpeza de `detalhe` feita por `pedir` deixou de valer. Também há lacunas nos testes novos e falhas de `assumir` sem registro persistido da falha original.** (`app/folha/porta_export.py:1105`, `app/folha/porta_export.py:1049`, `app/folha/tests/test_porta_sem_beco_item2.py:459`, `app/folha/tests/test_porta_sem_beco_item2.py:477`, `app/folha/porta_export.py:1209`)

1. **Chamadores, transações e travas**

   Há quatro chamadores diretos em produção e um em teste. O auxiliar abre `atomic`, busca com `select_for_update`, filtra estado/tentativa e deriva o detalhe da linha travada. (`app/folha/porta_export.py:1040`, `app/folha/porta_export.py:1049`)

   | Chamador direto | Contexto |
   |---|---|
   | `_falhar` | Sem `atomic` próprio; usa o do auxiliar e depois registra a recusa. (`app/folha/porta_export.py:1073`, `app/folha/porta_export.py:1076`) |
   | `pedir`, ao refazer | O `atomic` da tentativa de criação já terminou; a retomada usa o `atomic` do auxiliar, sem advisory local. (`app/folha/porta_export.py:1093`, `app/folha/porta_export.py:1098`, `app/folha/porta_export.py:1106`) |
   | `assumir`, com tentativa | Leitura inicial comum; no comando, entra sob a advisory já adquirida. (`app/folha/porta_export.py:1129`, `app/folha/porta_export.py:1131`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:138`) |
   | `concluir` | Tem `atomic` externo, já travou a mesma linha e já chamou `lavrar`; o auxiliar abre `atomic` aninhado e pede novamente a trava dessa linha. (`app/folha/porta_export.py:1179`, `app/folha/porta_export.py:1180`, `app/folha/porta_export.py:1185`, `app/folha/porta_export.py:1187`) |
   | Teste direto | Ignora o retorno, em classe `TestCase`, sem advisory explícita nesse teste. (`app/folha/tests/test_porta_sem_beco_item2.py:34`, `app/folha/tests/test_porta_sem_beco_item2.py:102`) |

   `_falhar` recebe chamadas de `encerrar` e dos dois ramos de `vigiar`. `encerrar` vem da recuperação do comando, de `falhar_ao_assumir` e da falha de disparo do subprocesso. Os dois primeiros caminhos do comando permanecem dentro da advisory; `disparar` não adquire essa trava. (`app/folha/porta_export.py:1199`, `app/folha/porta_export.py:1229`, `app/folha/porta_export.py:1238`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:152`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:168`, `app/folha/porta_export.py:1215`, `app/folha/porta_export.py:1340`)

   `vigiar` também roda pela leitura da porta, pelo censo global anterior à advisory e por `assumir` sem tentativa, este último sob a advisory do comando. O helper de testes chama `assumir`/`concluir` sem advisory local. (`app/folha/porta_export.py:1272`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:73`, `app/folha/porta_export.py:1252`, `app/folha/porta_export.py:1137`, `app/folha/porta_export.py:887`)

   **Não identifiquei inversão dessas travas formando ciclo nos caminhos de produção examinados.** A ordem do comando é advisory → pedido → carimbo; a segunda aquisição do pedido em `concluir` é da linha que a própria transação já travou. O censo global encerra cada `atomic` antes de chegar à advisory; esta usa `pg_try_advisory_lock`, e `medicao_viva` somente consulta `pg_locks`. Ausência de deadlock em execução real: **[nao verificado]**. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:138`, `app/folha/porta_export.py:1180`, `app/folha/porta_export.py:1185`, `app/folha/porta_export.py:1187`, `app/folha/porta_export.py:1040`, `app/folha/porta_export.py:982`, `app/folha/porta_export.py:1002`)

2. **Dependência da cópia do chamador: sim, em `pedir`**

   `pedir` fazia `linha.detalhe = {}` para reiniciar o registro. Agora essa limpeza é ignorada: `_mudar_estado` começa com `atual.detalhe`, vindo do banco. (`app/folha/porta_export.py:1105`, `app/folha/porta_export.py:1049`)

   Os extras da retomada substituem competência, origem, ator e tentativa, mas deixam sobreviver, quando existentes, **`resultado`, `n`, `motivo`, `pid`, `backend_pid`, `em_curso_em`, `concluido_em` e `falhou_em` da tentativa anterior**. Seus escritores são `assumir`, `_falhar`, `concluir` e o timestamp automático da transição. (`app/folha/porta_export.py:1106`, `app/folha/porta_export.py:1132`, `app/folha/porta_export.py:1141`, `app/folha/porta_export.py:1073`, `app/folha/porta_export.py:1187`, `app/folha/porta_export.py:1052`)

   **`pedido_em` é renovado**, porque `para=PEDIDO` o regrava dinamicamente; não procede concluir timeout imediato por timestamp antigo. A regressão comprovada é o registro da tentativa nova carregar campos da anterior. Liberação indevida do TXT por isso: **[nao verificado]**; o leitor decide por estado, identidade da tentativa e carimbo, e a adoção sobrescreve a sessão antes de entrar em curso. (`app/folha/porta_export.py:1052`, `app/folha/porta_export.py:1106`, `app/folha/porta_export.py:1275`, `app/folha/porta_export.py:1283`, `app/folha/porta_export.py:1131`)

3. **Mudança de retorno: os usos encontrados continuam corretos**

   | Uso | Avaliação |
   |---|---|
   | `_falhar` | Usa o estado anterior na trilha e devolve `bool(antes)`. (`app/folha/porta_export.py:1073`, `app/folha/porta_export.py:1077`) |
   | `pedir` | Converte explicitamente para bool; `abrir_a_porta` testa `if criado`. (`app/folha/porta_export.py:1106`, `app/folha/porta_export.py:1308`) |
   | `assumir` | Usa `not`; sucesso retorna `'pedido'`, string não vazia, e derrota retorna `None`. Preserva seu contrato linha/`None`. (`app/folha/porta_export.py:913`, `app/folha/porta_export.py:1047`, `app/folha/porta_export.py:1055`, `app/folha/porta_export.py:1131`) |
   | `concluir` | Ignora o retorno, mas já protege a mesma linha/estado/tentativa na transação externa. Seu retorno continua dict/`None`. (`app/folha/porta_export.py:1180`, `app/folha/porta_export.py:1187`, `app/folha/porta_export.py:1192`) |
   | Teste direto | Ignora o retorno. (`app/folha/tests/test_porta_sem_beco_item2.py:102`) |

   `encerrar` e `falhar_ao_assumir` continuam repassando bool; `_fechar` descarta esse retorno. O comando testa `linha is None` e `res is None`, não o estado anterior contra `True`. Não apareceu comparação incompatível nos consumidores encontrados. (`app/folha/porta_export.py:1199`, `app/folha/porta_export.py:1215`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:63`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:156`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:172`)

4. **Fechamento, leitores e falhas sem registro**

   `falhar_ao_assumir` delega a `encerrar` → `_falhar` → `_mudar_estado`: grava estado, resultado, motivo, instante da falha e `calculado_em`, condicionado à tentativa. Depois `_recusa` escreve a trilha pelo mecanismo existente. `_fechar` somente executa o callback e captura exceções com saída em stdout; não é outro escritor de estado. (`app/folha/porta_export.py:1215`, `app/folha/porta_export.py:1199`, `app/folha/porta_export.py:1073`, `app/folha/porta_export.py:1051`, `app/folha/porta_export.py:1066`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:59`)

   Os leitores relevantes são: `vigiar`, para estado/sessão/prazo; `estado_da_porta`, para estado/tentativa/carimbo; `conferir`, para o texto; `_recusa`, para identificação e autoria; e o contador de recusas, que lê a linha crítica. O stdout de `_fechar` não entra nesse contador. (`app/folha/porta_export.py:1225`, `app/folha/porta_export.py:1275`, `app/folha/porta_export.py:820`, `app/folha/porta_export.py:1063`, `app/core/recusa_critica.py:108`, `app/core/recusa_critica.py:133`, `bin/recusa_estado.py:68`)

   **Nos dois callsites novos, outro detentor vivo da mesma advisory impede a entrada no ramo de recuperação.** O CAS também impede encerrar uma tentativa sucessora com outro UUID. Essa proteção depende de conservar a sessão que adquiriu a trava; perda/substituição da conexão durante a recuperação: **[nao verificado]**. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:138`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:139`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:152`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:168`, `app/folha/porta_export.py:1042`)

   Há uma ressalva anterior a G, dentro do escopo dos chamadores: `disparar` inclui `Popen` e a saída do contexto do arquivo no mesmo `try`. **Se o fechamento do arquivo levantar `OSError` depois de o filho nascer**, chama `encerrar`, que aceita também `EM_CURSO`, sem consultar vivacidade. Esse caminho pode fechar o estado da mesma tentativa já adotada pelo filho vivo; ocorrência real: **[nao verificado]**. (`app/folha/porta_export.py:1335`, `app/folha/porta_export.py:1337`, `app/folha/porta_export.py:1339`, `app/folha/porta_export.py:1199`)

   **Sim, `assumir` pode levantar sem deixar a falha original no registro do job nem na recusa crítica:** cron/manual não passa tentativa, e `falhar_ao_assumir` retorna `False` imediatamente. Se a própria recuperação levantar, `_fechar` também fica apenas no stdout; uma tentativa aberta poderá receber depois o motivo genérico do vigia. Isso não prova ausência de toda saída: o comando imprime o erro; retenção dessa saída em produção é **[nao verificado]**. (`app/folha/porta_export.py:1125`, `app/folha/porta_export.py:1209`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:152`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:153`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:65`, `app/folha/porta_export.py:1229`, `app/folha/porta_export.py:1238`)

   A mensagem final “cada uma FALHOU com o seu motivo” não é garantida: `_fechar` não verifica sucesso e pode capturar a falha da gravação. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:63`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:65`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:270`)

5. **Os três testes de `VoltaTresTest`** [nao verificado]

   - **Primeiro: morde a regressão da cópia velha.** Lê PEDIDO, adota, encerra pela cópia antiga e exige trilha `antes=em_curso`, ator e metadados presentes. É sequencial; não prova concorrência/travas e não compara os valores exatos da sessão e do instante preservados. (`app/folha/tests/test_porta_sem_beco_item2.py:430`, `app/folha/tests/test_porta_sem_beco_item2.py:432`, `app/folha/tests/test_porta_sem_beco_item2.py:438`, `app/folha/tests/test_porta_sem_beco_item2.py:442`)
   - **Segundo: morde a persistência da falha do clique, mas não A→B no mesmo disparo.** Executa um comando exclusivo para A e outro para B. O mock também levanta antes de qualquer escrita real de `assumir`. (`app/folha/tests/test_porta_sem_beco_item2.py:453`, `app/folha/tests/test_porta_sem_beco_item2.py:459`, `app/folha/tests/test_porta_sem_beco_item2.py:460`, `app/folha/tests/test_porta_sem_beco_item2.py:462`)
   - **Terceiro: pode passar vazio quanto a “encerrar levantou”.** Não exige que o mock de `encerrar` seja chamado. Removendo a recuperação do handler, A ainda entra em `falharam`, B continua medindo e o comando termina com `SystemExit`; suas assertivas continuam satisfeitas. (`app/folha/tests/test_porta_sem_beco_item2.py:477`, `app/folha/tests/test_porta_sem_beco_item2.py:478`, `app/folha/tests/test_porta_sem_beco_item2.py:480`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:168`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:170`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:275`)

6. **Classes de reprovação**

   **Presentes:** selo sem mordida no terceiro teste; cobertura insuficiente do “mesmo disparo” no segundo; e campos misturando tentativas após o reinício — por exemplo, UUID novo com resultado/sessão/instantes anteriores. (`app/folha/tests/test_porta_sem_beco_item2.py:477`, `app/folha/tests/test_porta_sem_beco_item2.py:459`, `app/folha/porta_export.py:1105`, `app/folha/porta_export.py:1049`)

   **Não identifiquei introdução de segundo juiz, segundo escritor fora da porta ou derivação paralela:** os helpers novos delegam às funções existentes. Tampouco identifiquei inversão das travas examinadas. As leituras de relógio adicionadas são timestamps da transição; não há demonstração de divergência de dia causada por G. (`app/folha/porta_export.py:1215`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:63`, `app/folha/porta_export.py:1180`, `app/folha/porta_export.py:1187`, `app/folha/porta_export.py:1052`)

   Resultado de execução dos testes, concorrência real e ocorrência desses caminhos em produção: **[nao verificado]**. [nao verificado]

## 17/11 13:58 · sessao c06c9acddf9f · - · - · origem=Ronald (tmux)

**Q3 — retorno compatível nos usos encontrados.**

- `_falhar` usa o estado anterior na trilha e devolve `bool(antes)`; `encerrar` e `falhar_ao_assumir` repassam esse bool (`app/folha/porta_export.py:1073`, `:1075`, `:1077`, `:1199`, `:1215`).
- `pedir` converte explicitamente para bool; `abrir_a_porta` testa `if criado` (`app/folha/porta_export.py:1106`, `:1307`, `:1308`).
- `assumir` testa `not _mudar_estado`; o sucesso devolve `'pedido'`, string não vazia, e o fracasso devolve `None`. Depois retorna linha ou `None`, que o comando testa com `is None` (`app/folha/porta_export.py:913`, `:1047`, `:1055`, `:1131`, `:1134`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:156`).
- `concluir` ignora esse retorno, mantendo o contrato dict ou `None`; já travou a mesma linha com estado e tentativa antes da chamada interna. O comando testa `res is None` e depois `res['resultado']` (`app/folha/porta_export.py:1179`, `:1180`, `:1187`, `:1192`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:172`, `:179`).
- A quinta chamada direta fica no teste e ignora o retorno (`app/folha/tests/test_porta_sem_beco_item2.py:102`). Comparações desse retorno com `True`/`False` não foram encontradas na cópia [nao verificado fora da cópia].

**Q5 — duas limitações concretas nos testes novos.** [nao verificado]

1. O primeiro testa efetivamente cópia antiga, trilha `antes=em_curso`, ator e presença dos metadados da adoção. Porém é sequencial; chama `assumir` diretamente, sem advisory lock, e só exige que sessão e instante sejam verdadeiros, sem comparar os valores exatos (`app/folha/tests/test_porta_sem_beco_item2.py:428`, `:430`, `:432`, `:438`, `:439`, `:442`, `:443`).

2. O segundo testa que falha de adoção do clique persiste estado, motivo e uma recusa. **Não prova continuidade A→B no mesmo disparo:** executa um comando exclusivo para A e outro exclusivo para B. Além disso, o mock levanta antes de qualquer escrita real de `assumir`, cobrindo apenas essa posição da falha (`app/folha/tests/test_porta_sem_beco_item2.py:451`, `:453`, `:459`, `:460`, `:462`, `:463`, `:464`, `:465`).

3. O terceiro **pode passar vazio quanto a “encerrar levantou”**: instala o mock, mas não verifica que ele foi chamado. Se a chamada de recuperação for removida do comando, o handler ainda acrescenta A a `falharam`, continua para B e levanta `SystemExit` no final; as duas assertivas permanecem satisfeitas. Portanto ele prova B pronta após falha de concluir, mas não exige que a exceção de encerrar tenha ocorrido (`app/folha/tests/test_porta_sem_beco_item2.py:471`, `:477`, `:478`, `:480`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:168`, `:170`, `:171`, `:275`, `:276`).

Observação corrigida: **não há evidência de `pedido_em` velho no retry**, pois `_mudar_estado(..., para=PEDIDO)` regrava `pedido_em` dinamicamente. Os outros campos antigos continuam presentes porque a limpeza da cópia em memória deixou de ser a base da persistência (`app/folha/porta_export.py:1049`, `:1052`, `:1105`, `:1106`).

Execução dos testes e comportamento em produção: [nao verificado]. [nao verificado]

## 17/11 13:57 · sessao 0721ab2ad95a · - · - · origem=Ronald (tmux)

Q4: há caminho sem registro da falha original: cron/manual chama `assumir` sem tentativa; se levantar, `falhar_ao_assumir` retorna `False` sem gravar. O comando só imprime e acrescenta a empresa a `falharam`; depois afirma “cada uma FALHOU com seu motivo”, mesmo sem essa garantia. (`app/folha/porta_export.py:1125`, `app/folha/porta_export.py:1209`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:152`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:269`.)

`_fechar` não é escritor alternativo: executa o callback e captura qualquer `Exception`, imprimindo em stdout. Não verifica sucesso nem preserva o erro em outro registro. Se a recuperação falhar, a tentativa pode continuar aberta e o vigia posteriormente gravar apenas o motivo genérico de prazo/processo morto. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:59`, `app/folha/porta_export.py:1226`, `app/folha/porta_export.py:1237`.)

Nos dois callsites atuais, `_fechar` executa dentro da advisory lock adquirida para empresa/competência; outro detentor dessa mesma trava impede a entrada no ramo. A tentativa também é conferida novamente pelo CAS. Não encontrei nesse caminho fechamento de tentativa de outro detentor vivo da mesma trava; comportamento com perda/substituição de sessão de conexão [nao verificado]. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:138`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:152`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:168`, `app/folha/porta_export.py:982`, `app/folha/porta_export.py:1042`.)

Existe caminho anterior ao diff que merece ressalva: `disparar` põe `Popen` e saída do contexto de arquivo no mesmo `try`; um `OSError` no fechamento do arquivo depois de o filho nascer chama `encerrar`, que aceita também `EM_CURSO` e não consulta vivacidade. Pode, portanto, fechar o estado da tentativa já adotada pelo filho vivo; ocorrência real [nao verificado]. (`app/folha/porta_export.py:1335`, `app/folha/porta_export.py:1337`, `app/folha/porta_export.py:1339`, `app/folha/porta_export.py:1199`.)

Os campos gravados seguem pela porta existente: `_falhar` fornece `resultado` e `motivo`; `_mudar_estado` grava `estado`, `falhou_em` e `calculado_em`; `_recusa` escreve a trilha por `recusa_critica.registrar`. (`app/folha/porta_export.py:1073`, `app/folha/porta_export.py:1051`, `app/folha/porta_export.py:1063`, `app/core/recusa_critica.py:70`.) `vigiar` lê estado/sessão/prazo; `estado_da_porta` lê estado/tentativa; `conferir` transforma FALHA em texto fixo, sem mostrar o motivo. (`app/folha/porta_export.py:1225`, `app/folha/porta_export.py:1275`, `app/folha/porta_export.py:823`.) O contador lê exclusivamente as linhas de recusa; stdout de `_fechar` não alimenta esse mecanismo. (`app/core/recusa_critica.py:108`, `app/core/recusa_critica.py:133`, `bin/recusa_estado.py:68`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:65`.)

Q6: os helpers novos delegam ao escritor existente, sem introduzir segundo juiz ou segundo escritor de estado; permanece a lacuna de observabilidade acima. (`app/folha/porta_export.py:1215`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:63`.) Não executei testes nem acessei banco/log; efeitos reais [nao verificado].

## 17/11 13:57 · sessao e84a60e96239 · - · - · origem=Ronald (tmux)

Censo completo de `_mudar_estado`: quatro chamadores em produção e um direto em teste; o auxiliar sempre abre `atomic`, trava a linha filtrada por estado/ficha, deriva `detalhe` da linha travada e retorna estado anterior ou `None` (`app/folha/porta_export.py:1040–1055`).

| Chamador direto | Contexto e travas |
|---|---|
| `_falhar` | Sem `atomic` próprio; delega ao auxiliar e depois registra recusa (`app/folha/porta_export.py:1072–1077`). Vem de `encerrar` (`:1199`), de `vigiar` para EM_CURSO (`:1229`) ou PEDIDO (`:1238`). |
| `pedir`, retomada | O `atomic` da tentativa de criação já terminou antes de chamar o auxiliar; não retém trava explícita anterior nesse caminho (`app/folha/porta_export.py:1093–1107`). Vem de `abrir_a_porta` (`:1307`), acionada pela view sem `atomic` local (`app/folha/views.py:101–131`). |
| `assumir`, com tentativa | Sem `atomic` próprio; faz leitura comum e chama o auxiliar (`app/folha/porta_export.py:1129–1134`). No comando, executa dentro da advisory lock (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:138–150`). |
| `concluir` | Já está dentro de `atomic` externo e já travou a MESMA linha; chama `lavrar` antes do auxiliar, que abre `atomic` aninhado e pede novamente a mesma trava (`app/folha/porta_export.py:1179–1188`). No comando, permanece sob advisory lock (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:138–168`). |
| Teste `test_CONCLUIDO_sem_o_carimbo_DESTA_tentativa_nao_le_o_verde_antigo` | Chamada direta, sem uso do retorno, dentro de classe `TestCase`; nenhum advisory lock explícito nesse teste (`app/folha/tests/test_porta_sem_beco_item2.py:34`, `:96–104`). |

Entradas indiretas relevantes: `encerrar` é chamado pela recuperação do comando, ainda dentro da advisory lock (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:138`, `:168`), por `falhar_ao_assumir` nesse mesmo contexto (`:152`; `app/folha/porta_export.py:1215`), e por `disparar` quando o subprocesso não nasce, sem advisory lock local (`app/folha/porta_export.py:1331–1340`). `vigiar` vem da leitura da porta (`:1272`), do censo global antes da aquisição da advisory lock (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:73`, `:138`; `app/folha/porta_export.py:1252`) e de `assumir` sem tentativa, sob a advisory do comando (`app/folha/porta_export.py:1135–1138`). O helper de testes chama `assumir` e `concluir` sem advisory lock local (`app/folha/porta_export.py:887–890`).

Não encontrei ciclo de espera introduzido: comando adquire advisory → linha do pedido; `concluir` adquire pedido → carimbo → novamente o próprio pedido; a advisory usa `pg_try_advisory_lock`, sem espera, e `medicao_viva` apenas consulta `pg_locks` (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:138`; `app/folha/porta_export.py:982`, `:1002–1006`, `:1179–1188`, `:779`). O censo anterior ao comando encerra cada chamada do auxiliar antes de chegar à advisory lock (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:73`, `:138`; `app/folha/porta_export.py:1040–1055`, `:1249–1253`). Ausência de deadlock em execução real: [nao verificado].

Q2: **há dependência da cópia em memória**. `pedir` faz explicitamente `linha.detalhe = {}` antes de chamar o auxiliar; essa limpeza agora é ignorada porque a base vem de `atual.detalhe` (`app/folha/porta_export.py:1105`, `:1049`). Assim, retomada ganha nova `tentativa`, `pedido_em`, origem/ator/competência, mas conserva `resultado`, `n`, `motivo`, `pid`, `backend_pid`, `em_curso_em`, `concluido_em` e `falhou_em` anteriormente existentes, pois os extras não os removem (`app/folha/porta_export.py:1106–1107`, `:1050–1052`). Escritores desses campos: `assumir` (`:1132`, `:1141–1143`), `_falhar` (`:1073–1074`), `concluir` (`:1187–1188`) e carimbos automáticos de transição (`:1052`).

**Correção de hipótese preliminar:** `pedido_em` não permanece velho: `para=PEDIDO` faz `'%s_em' % para` renová-lo (`app/folha/porta_export.py:1052`, `:1106`). Portanto não há base para afirmar timeout imediato por esse campo. A mistura de campos de tentativas está demonstrada; liberação indevida do TXT por ela não está demonstrada, pois a porta decide por estado/tentativa e carimbo, e `assumir` substitui o `backend_pid` antes de EM_CURSO (`app/folha/porta_export.py:1275–1291`, `:1131–1132`). Os leitores encontrados de `resultado/n/motivo` do registro são sobretudo asserts dos testes (`app/folha/tests/test_porta_sem_beco_item2.py:121–130`); `vigiar` só consulta `backend_pid` quando estado é EM_CURSO (`app/folha/porta_export.py:1226–1228`).

## 17/11 13:38 · sessao 2977a8187bea · PORTA-EXPORT · verificador · origem=Code

**Não considero a obra fechada:** a exceção em `assumir` ainda não grava FALHOU/motivo/recusa; e o `antes` de `encerrar` pode diferir do estado vencido pelo CAS. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:143`; `app/folha/porta_export.py:1065`; `app/folha/porta_export.py:1192`)

Execução dos testes, reprodução das corridas e ocorrência em produção: **[nao verificado]**. [nao verificado]

1. **Quem escreve e quem lê**

   | Campo | Escritores e leitores encontrados |
   |---|---|
   | Pedido: `detalhe.tentativa` | `pedir` cria/renova UUID; `assumir` adota a recebida ou cria outra. `_mudar_estado` transporta o detalhe. Leem `_ficha`/CAS, adoção, conclusão, recusa, estado da porta e argv do subprocesso. (`app/folha/porta_export.py:1083`; `app/folha/porta_export.py:1100`; `app/folha/porta_export.py:1123`; `app/folha/porta_export.py:1135`; `app/folha/porta_export.py:1022`; `app/folha/porta_export.py:1040`; `app/folha/porta_export.py:1170`; `app/folha/porta_export.py:1060`; `app/folha/porta_export.py:1260`; `app/folha/porta_export.py:1307`) |
   | Pedido: `detalhe.ator` | `pedir` grava `getattr(ator, 'pk', None)`, recebendo `request.user` da view. A adoção preserva o campo; uma tentativa nova cron/manual substitui o detalhe sem ator. `_recusa` o lê. (`app/folha/views.py:131`; `app/folha/porta_export.py:1082`; `app/folha/porta_export.py:1100`; `app/folha/porta_export.py:1124`; `app/folha/porta_export.py:1134`; `app/folha/porta_export.py:1057`) |
   | Carimbo: `detalhe.tentativa` | `lavrar` grava o argumento, inclusive `None`; `concluir` fornece a tentativa da linha validada. O leitor decisório é `estado_da_porta`. (`app/folha/porta_export.py:768`; `app/folha/porta_export.py:784`; `app/folha/porta_export.py:1178`; `app/folha/porta_export.py:1260`) |
   | Motivo | `_falhar` persiste até 500 caracteres; `concluir` persiste `res['motivo']`. `_recusa` acrescenta a trilha; divergência recebe texto explicativo somente na linha. `registrar` normaliza espaços; o parser recupera o motivo, mas o contador usa data/porta, sem interpretar seu conteúdo. (`app/folha/porta_export.py:1067`; `app/folha/porta_export.py:1181`; `app/folha/porta_export.py:1056`; `app/folha/porta_export.py:1184`; `app/core/recusa_critica.py:84`; `app/core/recusa_critica.py:92`; `app/core/recusa_critica.py:117`) |

   **Há escrita fora de `concluir`:** `lavrar` continua escrevendo diretamente; os dois testes negativos novos a chamam; a fixture `_carimbo` escreve pedido e carimbo diretamente, com tentativa constante `fixture`. Portanto, exclusividade literal de `concluir` não existe. (`app/folha/porta_export.py:779`; `app/folha/tests/test_porta_sem_beco_item2.py:305`; `app/folha/tests/test_porta_sem_beco_item2.py:317`; `app/folha/tests/test_porta_do_export.py:42`; `app/folha/tests/test_porta_do_export.py:46`)

   Nos caminhos produtivos encontrados, o comando chama `concluir`; o helper corrigido usa `assumir → concluir`, inclusive para o teste de relatórios. Não encontrei outro chamador produtivo de escrita desse carimbo. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:157`; `app/folha/porta_export.py:887`; `app/relatorios/tests/test_cauda_trilha_do_cartao.py:109`)

   Não encontrei leitor decisório alternativo aceitando carimbo sozinho: `conferir`, `abrir_a_porta` e a view delegam à mesma porta. (`app/folha/porta_export.py:820`; `app/folha/porta_export.py:1282`; `app/folha/views.py:130`)

2. **PRONTA e caminhos legítimos**

   Nos objetos lidos pela função, **PRONTA exige pedido CONCLUIDO, tentativa não vazia e igual à do carimbo**, ambos consultados pelo mesmo dia. Os dois escapes da volta 2 estão fechados. (`app/folha/porta_export.py:1248`; `app/folha/porta_export.py:1258`; `app/folha/porta_export.py:1260`; `app/folha/porta_export.py:1268`)

   Isso não garante que o banco permaneça naquele estado até a resposta: entre as duas consultas, um cron pode substituir o pedido concluído por outra tentativa EM_CURSO, enquanto o leitor conserva o pedido antigo e encontra o carimbo antigo correspondente. É uma leitura válida do par anterior; não demonstra liberação sem jamais ter existido o par exigido. Corrida executada: **[nao verificado]**. (`app/folha/porta_export.py:1249`; `app/folha/porta_export.py:1250`; `app/folha/porta_export.py:1131`)

   Não encontrei beco estrutural novo para cron/manual/rollout: sem clique, `assumir` cria pedido EM_CURSO com UUID, e `concluir` grava o par. O rollout seleciona empresas ativas em rollout e a competência corrente; o cron está declarado com `--rollout --origem cron`. Instalação e funcionamento reais: **[nao verificado]**. (`app/folha/porta_export.py:1131`; `app/folha/porta_export.py:1178`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:46`; `app/config/crons.py:1803`)

3. **R8 alargado**

   **`assumir` continua incompleto.** Seu `except` apenas imprime, acrescenta a empresa a `falharam` e continua. Não persiste FALHOU, motivo nem linha de recusa. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:140`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:143`)

   Se a exceção ocorrer antes da criação e não houver pedido anterior, não nasce registro. Se houver pedido do clique e a adoção ainda não ocorreu, ele pode permanecer PEDIDO até o vigia aplicar o prazo de 300 segundos; o motivo posterior será o prazo, não a exceção original. (`app/folha/porta_export.py:1118`; `app/folha/porta_export.py:1122`; `app/folha/porta_export.py:920`; `app/folha/porta_export.py:1214`)

   **`concluir` entrou no `try` corretamente.** Exceções dentro do bloco que lavra e muda o estado desfazem ambas as escritas; depois o comando tenta `encerrar` a tentativa como FALHOU. Essa chamada de recuperação não tem proteção adicional: se ela levantar, a seguinte empresa não será alcançada. (`app/folha/porta_export.py:1172`; `app/folha/porta_export.py:1178`; `app/folha/porta_export.py:1180`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:155`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:159`)

   A frase “`concluir` levantou, nada ficou” é ampla demais: `_recusa` ocorre depois do `atomic`. Uma exceção ali não desfaz o par já confirmado. É o limite do log não transacional já descartado na triagem, não outro bloqueador por queda CAS/log. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:153`; `app/folha/porta_export.py:1182`; `app/folha/porta_export.py:1184`)

4. **Verdade da trilha**

   **Contraexemplo do `antes`:** `encerrar` conserva um objeto PEDIDO(t); outro chamador adota a mesma tentativa, mudando o banco para EM_CURSO(t); o encerramento aceita ambos os estados e vence de EM_CURSO, mas imprime `antes=pedido`. Além disso, escreve o JSON antigo inteiro, podendo apagar `pid`, `backend_pid` e o instante da adoção. Esse interleaving é permitido pelo código; reprodução: **[nao verificado]**. (`app/folha/porta_export.py:1065`; `app/folha/porta_export.py:1192`; `app/folha/porta_export.py:1124`; `app/folha/porta_export.py:1035`; `app/folha/porta_export.py:1039`; `app/folha/porta_export.py:1042`)

   Nos caminhos do vigia, o CAS aceita um único estado; em `concluir`, EM_CURSO é conferido sob trava da linha. Nesses sítios, o problema acima não se aplica. (`app/folha/porta_export.py:1206`; `app/folha/porta_export.py:1215`; `app/folha/porta_export.py:1173`)

   Há também uma ambiguidade em **`origem`**: nasce como origem do pedido e é sobrescrita pela origem do executor durante a adoção. O CLI aceita `--tentativa` com `--origem manual`; nessa combinação, a linha deixa de preservar literalmente a origem do pedido. (`app/folha/porta_export.py:1083`; `app/folha/porta_export.py:1125`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:39`)

   **Ator é pk nesse caminho**, nunca username: a porta grava `.pk` e o imprime no motivo. Cron/manual sem ator saem `ator=-`; o campo principal `usuario` também fica `-`, pois `_recusa` passa `request=None`. (`app/folha/porta_export.py:1082`; `app/folha/porta_export.py:1057`; `app/folha/porta_export.py:1058`; `app/core/recusa_critica.py:77`)

5. **Mordida dos testes** [nao verificado]

   - Os freezes existem nas duas classes; os negativos exercitam a porta real contra carimbo sozinho e tentativa ausente dos dois lados. (`app/folha/tests/test_porta_sem_beco_item2.py:19`; `:292`; `:302`; `:310`; `:387`)
   - M9 verifica retorno, ausência de carimbo/recusa e preservação da sucessora. O marcador de log não esvazia a verificação: o filtro seleciona a porta. (`app/folha/tests/test_porta_sem_beco_item2.py:328`; `:341`; `:29`)
   - M12 verifica o perdedor do CAS e uma linha, mas ambos os objetos começam EM_CURSO. Não cobre a adoção PEDIDO→EM_CURSO entre leitura e encerramento, nem verifica ator. (`app/folha/tests/test_porta_sem_beco_item2.py:350`; `:354`; `:358`)
   - O relógio simulado hoje/amanhã morde a antiga leitura dupla; o reclique exige um único subprocesso, impedindo que todos os POSTs passem inertes. (`app/folha/tests/test_porta_sem_beco_item2.py:366`; `:368`; `:384`)
   - **R8ConcluirTest prova tratamento na borda e continuação para B.** Para A, lança antes do corpo real; não prova rollback após lavrar, ausência de carimbo parcial, uma recusa ou tratamento de `assumir`. (`app/folha/tests/test_porta_sem_beco_item2.py:400`; `:405`; `:408`)
   - O acréscimo do shell exige ausência de arquivo também para a porta e verifica sua frase, com datas explícitas. (`bin/tests/test_recusa_estado.sh:30`; `:44`; `:48`)

6. **Classes históricas**

   Não encontrei segundo juiz nem derivação paralela nova: texto e clique continuam consultando `estado_da_porta`; a dupla leitura do dia foi removida. Não identifiquei inversão nova de travas no trecho: o comando mantém advisory lock antes da trava da linha em `concluir`. (`app/folha/porta_export.py:820`; `app/folha/porta_export.py:1282`; `app/folha/porta_export.py:1248`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:130`; `app/folha/porta_export.py:1173`)

   Permanecem a escrita direta de fixtures fora de `concluir`, a cobertura insuficiente do selo para rollback/R8 completo e o campo `origem` servindo ao pedido e ao executor. Os bloqueadores concretos desta revisão são a falha de `assumir` sem registro e a trilha `antes` potencialmente falsa. (`app/folha/tests/test_porta_do_export.py:42`; `app/folha/tests/test_porta_sem_beco_item2.py:400`; `app/folha/porta_export.py:1125`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:143`; `app/folha/porta_export.py:1065`)

## 17/11 13:36 · sessao d53c28693167 · - · - · origem=Ronald (tmux)

Achados para consolidar:

1. **R8 não cumpre “assumir levantou → FALHOU com motivo e uma recusa”.** O `except` de `assumir` apenas imprime, acrescenta a empresa a `falharam` e segue; não chama `encerrar`, não persiste motivo nem emite `_recusa`. Se falhar antes da criação, não nasce registro; se já existia pedido do clique, ele pode continuar `PEDIDO` até o vigia expirar os 300 segundos. A seguinte empresa é tentada, supondo que a saída e a liberação da trava não levantem. Fontes: `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:140`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:143`, `app/folha/porta_export.py:1118`, `app/folha/porta_export.py:1122`, `app/folha/porta_export.py:1214`, `app/folha/porta_export.py:920`. Ocorrência em produção: [nao verificado].

2. **`antes` não é necessariamente o estado vencido pelo CAS amplo de `encerrar`.** `_falhar` guarda `antes` do objeto em memória; `encerrar` aceita banco em `PEDIDO` **ou** `EM_CURSO`, com filtro somente da tentativa. Se outra chamada adotar o mesmo pedido entre essa leitura e o UPDATE, o banco pode transitar `PEDIDO→EM_CURSO` preservando a tentativa; o encerramento então vence de `EM_CURSO`, mas escreve `antes=pedido`. Como o JSON inteiro sai do objeto antigo, também pode apagar `pid`, `backend_pid` e o instante da adoção. Nos caminhos de `vigiar`, o estado permitido é unitário; no `concluir`, `EM_CURSO` está protegido pela linha travada. Fontes: `app/folha/porta_export.py:1065`, `app/folha/porta_export.py:1192`, `app/folha/porta_export.py:1124`, `app/folha/porta_export.py:1035`, `app/folha/porta_export.py:1039`, `app/folha/porta_export.py:1042`, `app/folha/porta_export.py:1206`, `app/folha/porta_export.py:1215`, `app/folha/porta_export.py:1173`. Interleaving reproduzido: [nao verificado].

3. **Rollback de concluir:** falhas dentro do bloco que trava o pedido, lavra e muda o estado ficam no mesmo `atomic`; o comando captura a exceção e chama `encerrar` sobre a tentativa. Entretanto, a afirmação comentada “`concluir` levantou, nada ficou” não cobre literalmente toda a função: a cópia do detalhe e `_recusa` são posteriores ao `atomic`. Isso é o limite já associado ao log não transacional, não proponho reabrir a queda CAS/log descartada. Fontes: `app/folha/porta_export.py:1172`, `app/folha/porta_export.py:1178`, `app/folha/porta_export.py:1180`, `app/folha/porta_export.py:1182`, `app/folha/porta_export.py:1184`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:153`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:158`.

4. **Limite de R8 se o próprio encerramento levantar:** a chamada a `encerrar` no tratamento da exceção não tem proteção adicional; uma exceção nela interrompe o laço, logo a continuação não é incondicional. Fonte: `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:159`. Ocorrência concreta: [nao verificado].

5. **R8ConcluirTest cobre a borda, não o rollback interno.** Há freeze na classe. Para A, o mock lança antes de executar qualquer linha do `concluir` real; somente B usa a implementação real. O teste verifica FALHA, motivo e B pronta, mas não verifica ausência de carimbo parcial nem quantidade de recusas. Fontes: `app/folha/tests/test_porta_sem_beco_item2.py:387`, `app/folha/tests/test_porta_sem_beco_item2.py:400`, `app/folha/tests/test_porta_sem_beco_item2.py:405`, `app/folha/tests/test_porta_sem_beco_item2.py:408`.

6. **M12 morde perdedor do CAS, mas não o `antes` concorrente acima.** Ambos os objetos começam `EM_CURSO`, o primeiro encerra e o segundo perde; não há adoção `PEDIDO→EM_CURSO` entre leitura e CAS. Fontes: `app/folha/tests/test_porta_sem_beco_item2.py:350`, `app/folha/tests/test_porta_sem_beco_item2.py:354`, `app/folha/tests/test_porta_sem_beco_item2.py:358`.

7. **PRONTA local está protegida; fotografia atual do banco não está garantida.** A função exige pedido local `CONCLUIDO`, tentativa verdadeira e igualdade com carimbo. Ela pode ler o pedido concluído antigo, um cron substituí-lo por nova tentativa em curso, e ainda ler o carimbo antigo correspondente. Isso continua linearizável ao instante inicial em que aquele par era válido; não demonstra liberação sem jamais ter existido o par exigido. Fontes: `app/folha/porta_export.py:1249`, `app/folha/porta_export.py:1250`, `app/folha/porta_export.py:1258`, `app/folha/porta_export.py:1260`, `app/folha/porta_export.py:1268`, `app/folha/porta_export.py:1131`. Corrida executada: [nao verificado].

8. **Ator:** `pedir` grava `getattr(ator, 'pk', None)`, a adoção preserva o campo e a recusa o imprime no motivo. Cron/manual criam detalhe sem `ator`, saindo `ator=-`; o campo principal `usuario` da linha também sai `-`, porque `_recusa` chama `registrar(None, ...)`. Nome de usuário não é introduzido por esse caminho da porta. Fontes: `app/folha/porta_export.py:1082`, `app/folha/porta_export.py:1083`, `app/folha/porta_export.py:1124`, `app/folha/porta_export.py:1134`, `app/folha/porta_export.py:1056`, `app/folha/porta_export.py:1058`, `app/core/recusa_critica.py:77`.

Nenhum teste foi executado; resultados de execução: [nao verificado]. [nao verificado]

## 17/11 13:35 · sessao 7dede955e4c0 · - · - · origem=Ronald (tmux)

- **Pedido — tentativa:** `pedir` cria UUID e o renova ao refazer (`app/folha/porta_export.py:1083`, `app/folha/porta_export.py:1100`); `assumir` adota a tentativa recebida ou cria outra para cron/manual (`app/folha/porta_export.py:1122`, `app/folha/porta_export.py:1135`). Leitores: `_ficha`/CAS, `assumir`, `concluir`, `_recusa`, `estado_da_porta` e argv do subprocesso (`app/folha/porta_export.py:1022`, `app/folha/porta_export.py:1040`, `app/folha/porta_export.py:1123`, `app/folha/porta_export.py:1170`, `app/folha/porta_export.py:1060`, `app/folha/porta_export.py:1260`, `app/folha/porta_export.py:1307`).

- **Pedido — ator:** escritor explícito é `pedir`, via `getattr(ator, 'pk', None)`; a view fornece `request.user`, e a recusa imprime esse pk (`app/folha/porta_export.py:1082`, `app/folha/porta_export.py:1100`, `app/folha/views.py:131`, `app/folha/porta_export.py:1057`). `assumir` com tentativa conserva ator mas sobrescreve origem; sem tentativa substitui todo detalhe e não inclui ator, como esperado para nova tentativa cron/manual (`app/folha/porta_export.py:1124`, `app/folha/porta_export.py:1131`).

- **Carimbo — tentativa:** `lavrar` grava o argumento recebido, incluindo `None`; `concluir` passa a tentativa validada da linha atual (`app/folha/porta_export.py:768`, `app/folha/porta_export.py:784`, `app/folha/porta_export.py:1178`). O leitor decisório exige pedido `CONCLUIDO`, tentativa verdadeira e igualdade com o carimbo (`app/folha/porta_export.py:1258`).

- **Ainda há escrita de carimbo fora de `concluir`:** `lavrar` continua função pública capaz de escrever diretamente; dois testes novos a chamam sem pedido ou sem tentativa (`app/folha/porta_export.py:779`, `app/folha/tests/test_porta_sem_beco_item2.py:305`, `app/folha/tests/test_porta_sem_beco_item2.py:317`). A fixture escreve diretamente tanto pedido quanto carimbo (`app/folha/tests/test_porta_do_export.py:42`, `app/folha/tests/test_porta_do_export.py:46`). Não encontrei outro chamador produtivo que escreva esse carimbo; o comando usa `concluir` e o helper passou a usar `assumir → concluir` (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:157`, `app/folha/porta_export.py:885`).

- **Testes de outros apps:** o uso encontrado em relatórios chama o helper corrigido (`app/relatorios/tests/test_cauda_trilha_do_cartao.py:109`). Os demais usos do helper estão em integração/fixtures de folha (`app/folha/tests/test_integracao.py:160`, `app/folha/tests/test_txt_le_a_celula_e_o_juiz.py:155`, `app/folha/tests/test_sm1_monster_export.py:124`).

- **Nenhum leitor decisório encontrado aceita carimbo sozinho:** `conferir` delega ao estado, o clique delega ao estado e a view delega ao clique; o script externo encontrado chama `conferir` (`app/folha/porta_export.py:820`, `app/folha/porta_export.py:1282`, `app/folha/views.py:130`, `bin/txt_antes_badge_linha.py:60`). PRONTA sem as condições do par não atravessa a cadeia de `elif` (`app/folha/porta_export.py:1254`).

- **Motivo da recusa:** `_falhar` grava no pedido o motivo truncado a 500 caracteres e envia o original à recusa; `concluir` grava `res['motivo']`, mas divergência grava vazio e usa fallback apenas na linha (`app/folha/porta_export.py:1067`, `app/folha/porta_export.py:1069`, `app/folha/porta_export.py:1181`, `app/folha/porta_export.py:1184`). `_recusa` acrescenta antes/depois/origem/ator; `registrar` normaliza espaços e grava WARNING (`app/folha/porta_export.py:1056`, `app/core/recusa_critica.py:80`, `app/core/recusa_critica.py:85`).

- **Leitores do motivo:** o parser o devolve como campo, mas o contador da porta ignora seu conteúdo e conta por data local da linha e porta; o wrapper de estado usa esse contador (`app/core/recusa_critica.py:49`, `app/core/recusa_critica.py:92`, `app/core/recusa_critica.py:113`, `app/core/recusa_critica.py:136`, `bin/recusa_estado.py:68`). A mensagem de FALHA na tela não lê o motivo persistido (`app/folha/porta_export.py:823`, `app/folha/porta_export.py:933`).

## 17/11 13:35 · sessao 267c99086cf9 · - · - · origem=Ronald (tmux)

Revisão estática; resultado da execução dos testes = [nao verificado]. [nao verificado]

- **Freeze existe e está importado:** ambas as classes novas têm `@freeze_time`, com import de `freezegun`; não encontrei ausência de congelamento nesses testes. `app/folha/tests/test_porta_sem_beco_item2.py:19`, `:292`, `:387`.
- **Os negativos da porta mordem os dois bugs declarados:** criam carimbo sem pedido e par sem tentativa, chamam `estado_da_porta` real e exigem recusa/FALHA. `app/folha/tests/test_porta_sem_beco_item2.py:302`, `:310`.
- **Helper verde:** exercita o helper real e exige PRONTA, pedido CONCLUIDO e tentativa preenchida; não espiona `assumir`/`concluir`, portanto prova o resultado, não a exclusividade desse caminho. `app/folha/tests/test_porta_sem_beco_item2.py:320`; implementação efetivamente usa ambos em `app/folha/porta_export.py:887`.
- **M9:** cria sucessora real, conserva detalhe da velha e verifica retorno `None`, ausência de carimbo, ausência de recusa e sucessora EM_CURSO; o marcador de log não torna a asserção vazia, pois `_linhas_da_porta` filtra só a porta. `app/folha/tests/test_porta_sem_beco_item2.py:328`, `:29`.
- **M12:** verifica CAS perdedor mediante duas cópias lidas antes do primeiro encerramento, contando exatamente uma recusa; porém é sequencial e cobre somente EM_CURSO→FALHOU, sem testar `PEDIDO→EM_CURSO` entre leitura e CAS de `encerrar`, nem ator. `app/folha/tests/test_porta_sem_beco_item2.py:346`.
- **Relógio:** o mock retorna hoje na primeira chamada e amanhã nas seguintes; ao exigir PRONTA com os registros de hoje, morde a antiga leitura dupla entre pedido/carimbo. Não é um contador literal de chamadas ignoradas, mas cobre o efeito relevante. `app/folha/tests/test_porta_sem_beco_item2.py:361`.
- **Reclique:** exercita POSTs e CAS reais; substitui apenas subprocesso e medição indevida no request, exigindo uma linha e um disparo. Não verifica respostas HTTP, mas `popen.call_count == 1` impede que todos os POSTs sejam inertes. `app/folha/tests/test_porta_sem_beco_item2.py:371`.
- **Lacuna principal: R8ConcluirTest não prova atomicidade de concluir.** Para A, o mock lança ANTES de chamar a implementação real; para B, chama a real. Isso morde tratamento da exceção no comando, motivo de A e continuação de B, mas não falha após lavrar, rollback do carimbo/estado, exceção pós-commit, nem uma linha de recusa. `app/folha/tests/test_porta_sem_beco_item2.py:398`, `:400`, `:408`.
- **Essa lacuna importa:** a `_recusa` real ocorre depois do `transaction.atomic`; uma exceção ali não é revertida pelo bloco, ao contrário do comentário do comando. `app/folha/porta_export.py:1172`, `:1182`, `:1184`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:153`.
- **Nenhum teste novo injeta falha em `assumir`**; o R8 novo injeta apenas `concluir`. `app/folha/tests/test_porta_sem_beco_item2.py:396`.
- **Fixtures alteradas:** `_carimbo` ainda escreve diretamente pedido+carimbo, com tentativa constante `fixture`, fora de `concluir`; assim continua adequada para testar o leitor/texto, mas não certifica escritor único nem protocolo do job. `app/folha/tests/test_porta_do_export.py:33`, `:42`, `:46`. Já `_pela_porta` passou a usar `assumir→concluir`. `app/folha/tests/test_porta_sem_beco.py:34`.
- **Shell:** o acréscimo realmente torna obrigatórios `sem_arquivo` da porta e texto “sem arquivo”; datas são explícitas, sem relógio solto nesse trecho. `bin/tests/test_recusa_estado.sh:29`, `:44`, `:48`.

## 17/11 12:42 · sessao 04f310b9fdf3 · PORTA-EXPORT · verificador · origem=Code

**Item 2 ainda não fecha integralmente.** Há liberação sem pedido/tentativa concluída, janela para término ruim ficar sem recusa e reemissão de recusa pelo POST. A proteção contra conclusão atrasada e contra FALHOU encoberto por verde antigo está implementada no caminho normal. (`app/folha/porta_export.py:1238–1257`, `:1056–1060`, `:1163–1174`; `app/folha/views.py:130–135`; `app/core/respostas.py:50–56`.)

Execução dos testes, comportamento em produção, banco, logs e correspondência histórica dos commits com o hash informado: **[nao verificado]**. [nao verificado]

**1. Quem escreve e quem lê**

As identidades de pedido e carimbo são empresa × competência × dia: competência na chave, empresa no escopo e dia em `data_ref`; a constraint inclui esses quatro campos. (`app/folha/porta_export.py:779–784`, `:933–941`, `:1077–1079`; `app/inteligencia/models.py:103–105`.)

| Campo | Escritor | Leitor |
|---|---|---|
| Pedido: `estado` | `pedir` cria PEDIDO; `assumir` cria EM_CURSO; `_mudar_estado` faz as demais transições, chamado por `pedir`, `assumir`, `_falhar` e `concluir`. (`app/folha/porta_export.py:1030–1039`, `:1056–1058`, `:1073–1079`, `:1089–1090`, `:1114–1126`, `:1170–1171`.) | CAS, repetição, adoção, conclusão, vigia, varredura e porta. (`app/folha/porta_export.py:1034`, `:1086`, `:1114–1119`, `:1164`, `:1193–1198`, `:1217`, `:1242–1249`.) |
| Pedido: `tentativa` | UUID novo em `pedir` e em `assumir` sem tentativa; adoção conserva o UUID recebido. (`app/folha/porta_export.py:1010–1012`, `:1073`, `:1090`, `:1112–1125`.) | Ficha do CAS, adoção, conclusão, objeto da recusa, argv do job e comparação com carimbo. (`app/folha/porta_export.py:1015–1017`, `:1035–1036`, `:1051–1052`, `:1113–1114`, `:1160–1168`, `:1248`, `:1296`.) |
| Pedido: `pid` | `assumir`, com `os.getpid()`. O PID retornado por `Popen` vai à auditoria, sem atualização do pedido naquele ponto. (`app/folha/porta_export.py:1115`, `:1125`, `:1303–1315`.) | O teste exige presença; o vigia operacional consulta `backend_pid`, não `pid`. (`app/folha/tests/test_porta_sem_beco_item2.py:60–64`; `app/folha/porta_export.py:1193–1196`.) |
| Pedido: `backend_pid` | `assumir` consulta `pg_backend_pid()` e grava a sessão. (`app/folha/porta_export.py:1108–1115`, `:1125`.) | `vigiar → medicao_viva`, comparando-o com `pg_locks.pid`. (`app/folha/porta_export.py:997–1001`, `:1194`.) |
| Pedido: `resultado`, `n` | `resultado_de` produz; `concluir` persiste ambos. `_falhar` grava `resultado='falhou'`, sem acrescentar `n`. (`app/folha/porta_export.py:1056–1058`, `:1136–1147`, `:1170–1171`.) | Os testes leem o registro; o command lê o retorno de `concluir`. A porta decide pelo estado e pelo carimbo, sem consultar esses dois campos persistidos. (`app/folha/tests/test_porta_sem_beco_item2.py:121–129`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:154–164`; `app/folha/porta_export.py:1241–1257`.) |
| Pedido: `motivo` | `_falhar`, limitado a 500 caracteres; `concluir`, com o motivo de `resultado_de`. (`app/folha/porta_export.py:1058`, `:1136–1144`, `:1171`.) | Testes conferem o motivo persistido. A mensagem pública deixou de lê-lo; a recusa recebe o motivo como argumento. (`app/folha/tests/test_porta_sem_beco_item2.py:56`, `:130`; `app/folha/porta_export.py:1050`, `:1174`, `:1261–1262`.) |
| Pedido: `pedido_em` | Criação em `pedir`; repetição por `_mudar_estado`, que gera `pedido_em` a partir do nome do estado. (`app/folha/porta_export.py:1033`, `:1074`, `:1089–1090`.) | `vigiar`, para os 300 segundos sem adoção. (`app/folha/porta_export.py:1200–1205`.) |
| Pedido: `em_curso_em`, `concluido_em`, `falhou_em` | `_mudar_estado`; `assumir` sem tentativa escreve diretamente `em_curso_em`. Esses timestamps não participam da decisão de vida atual, que usa sessão/trava ou `pedido_em`. (`app/folha/porta_export.py:1033`, `:1126`, `:1192–1205`.) | Leitor operacional específico desses timestamps: **[nao verificado]**. |
| Pedido/carimbo: `calculado_em` | `auto_now` do modelo; `_mudar_estado` também o atualiza explicitamente. (`app/inteligencia/models.py:96`; `app/folha/porta_export.py:1037`.) | Uso específico para liberar ou vigiar a porta: não aparece nos respectivos julgamentos. (`app/folha/porta_export.py:1192–1205`, `:1238–1257`.) |
| Carimbo: `tentativa` | Somente `lavrar`; `concluir` fornece a tentativa corrente. (`app/folha/porta_export.py:784`, `:1168`.) | `estado_da_porta`, quando o pedido está CONCLUIDO; testes conferem o vínculo. (`app/folha/porta_export.py:1247–1248`; `app/folha/tests/test_porta_sem_beco_item2.py:92`, `:268`.) |
| Carimbo: `erro_cartao_x_txt` | `medir` produz; `lavrar` persiste. (`app/folha/porta_export.py:671–675`, `:737`, `:790`.) | Teste verifica a persistência. O texto público não lê mais esse campo. (`app/folha/tests/test_porta_sem_beco_item2.py:132–133`; `app/folha/porta_export.py:1261–1262`.) |
| Carimbo: `valor` | `lavrar`: `falhas`, ou `-1` quando ausente. (`app/folha/porta_export.py:779–781`.) | `estado_da_porta` classifica o escalar; `conferir` imprime a quantidade divergente. (`app/folha/porta_export.py:871–872`, `:1252–1257`.) |

**Escritores externos existem nos testes:** `_carimbo` escreve diretamente o snapshot; o teste R7 altera diretamente `backend_pid`. Não são escritores operacionais novos. O helper verde chama o mesmo `lavrar`. (`app/folha/tests/test_porta_do_export.py:33–42`; `app/folha/tests/test_porta_sem_beco_item2.py:50–52`; `app/folha/porta_export.py:875–887`.)

O escritor genérico `inteligencia.metricas.calcular` exige chave registrada; os calculadores declarados usam outras chaves. Não há nesses registros um segundo escritor identificado para as chaves da porta. (`app/inteligencia/metricas.py:36–56`; `app/inteligencia/calculadores.py:9`, `:112`, `:183`, `:240`, `:302`.)

**Os dois PIDs têm significados separados**, mas “PID gravado e conferido” precisa dessa ressalva: o `pid` do SO fica como trilha e só nasce no pedido ao assumir; quem é conferido é o PID do backend. (`app/folha/porta_export.py:899–904`, `:1073–1079`, `:1115`, `:1194`.)

**2. Liberação e tentativa atrasada**

**Sim, há caminho para `conferir == ''` sem tentativa concluída:** pedido ausente + carimbo do dia com zero chega a PRONTA. `lavrar` aceita `tentativa=None`, e o helper verde produz exatamente esse estado. A exceção é executável e não está condicionada a ambiente de teste; utilização desse caminho em produção: **[nao verificado]**. (`app/folha/porta_export.py:768`, `:875–887`, `:1238–1257`, `:820–822`.)

Também não se exige tentativa **preenchida**: pedido CONCLUIDO e carimbo com tentativa ausente passam pela comparação `None == None`. Existência desses registros em produção: **[nao verificado]**. (`app/folha/porta_export.py:1247–1248`.)

**FALHOU de hoje com verde antigo não libera:** FALHOU prevalece antes da inspeção do escalar. CONCLUIDO com carimbo de outra tentativa também falha. (`app/folha/porta_export.py:1245–1249`.)

**No caminho do command, a tentativa atrasada é barrada:** adoção exige PEDIDO com o UUID recebido; `encerrar` usa CAS por tentativa; `concluir` trava e filtra a linha pela tentativa EM_CURSO antes de carimbar. (`app/folha/porta_export.py:1112–1116`, `:1056–1058`, `:1160–1168`, `:1178–1182`.)

Essa proteção pertence a `concluir`; uma chamada direta a `lavrar` continua sem verificar posse ou estado. Os chamadores diretos encontrados incluem o helper e testes. (`app/folha/porta_export.py:768–807`, `:885–887`; `app/folha/tests/test_porta_sem_beco.py:157–162`.)

**3. Pedido preso, morte e R7**

O vigia consulta a trava correta de dois inteiros: banco, `classid`, `objid`, `objsubid=2`, `granted` e o `pid` gravado do backend. Não basta outra sessão segurar a trava. (`app/folha/porta_export.py:995–1001`.)

Entretanto, a pergunta respondida é **“essa sessão segura essa trava?”**. Perder a sessão enquanto o processo Python continua vivo gera FALHOU; continuar segurando a trava enquanto o processo fica travado conserva EM_CURSO, sem prazo máximo nessa fase. (`app/folha/porta_export.py:1193–1205`.)

**Reuso de PID não é distinguido pela identidade da sessão:** não há `backend_start` nem outro identificador de encarnação no registro/consulta. Um backend reutilizado com o mesmo PID e a mesma trava satisfaria o predicado; ocorrência real dessa combinação: **[nao verificado]**. (`app/folha/porta_export.py:997–1000`, `:1108–1115`, `:1124–1126`.)

**R7 ganhou observador sem tela:** o command varre todos os pedidos abertos do schema, inclusive de dias anteriores, antes de selecionar os alvos. Isso fecha a dependência exclusiva da leitura humana. (`app/folha/porta_export.py:1209–1221`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:63–66`.)

**Não garante observação independente da execução seguinte:** o selo está no fim da corrente; o encadeador pode não executá-lo quando faltar o carimbo do predecessor. Uma morte depois da varredura inicial espera outra chamada. Portanto, basta para observação eventual com a corrente funcionando; prazo garantido e funcionamento instalado em produção: **[nao verificado]**. (`app/config/crons.py:1793–1805`; `bin/encadeado.sh:81–95`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:65`.)

FALHOU também não é histórico imutável no registro: `pedir` e `assumir` reutilizam a linha para outra tentativa. A memória diária da falha depende da recusa emitida antes dessa substituição. (`app/folha/porta_export.py:1085–1090`, `:1118–1126`, `:1056–1060`.)

**4. Uma recusa por tentativa e contador**

**Zero linhas é possível por interrupção:** `_falhar` persiste primeiro e registra depois; `concluir` termina a transação antes de registrar. Morte nesse intervalo deixa a tentativa terminal fora da varredura dos abertos, sem mecanismo mostrado de recuperação da recusa. (`app/folha/porta_export.py:1056–1060`, `:1162–1174`, `:1216–1217`.)

**A corrida normal entre vigia, encerrar e concluir não demonstra duplicação da linha `porta_export`:** o CAS só deixa o vencedor de `_falhar` emitir, e `concluir` filtra/trava a mesma tentativa. (`app/folha/porta_export.py:1034–1042`, `:1056–1060`, `:1163–1174`.)

**Mas há 2+ recusas críticas associadas ao fluxo da tentativa:** além da emissão do término sob `porta_export`, cada POST recusado passa pelo envelope sem `recusas=()`, que registra outra linha sob a porta da rota. O contador do export exclui essas linhas; isso evita inflação daquele N, mas não cumpre literalmente ausência de reemissão pelo clique. (`app/folha/porta_export.py:1050–1053`; `app/folha/views.py:130–135`; `app/core/respostas.py:45`, `:50–56`; `app/core/recusa_critica.py:119–121`.)

**O contador usa o leitor único e está integrado ao ESTADO**, com esperado zero e distinção explícita entre arquivo ausente e zero. (`app/core/recusa_critica.py:133–139`; `bin/recusa_estado.py:60–77`; `bin/gerar_estado.py:267–270`.)

**O dia contado é o prefixo temporal da linha**, não o campo `dia=` — este recebe o dia da tentativa. Assim, uma falha observada hoje para pedido de ontem conta pela data de emissão do log. (`app/folha/porta_export.py:1050`; `app/core/recusa_critica.py:46–50`, `:113–121`.)

A equivalência com o dia civil da aplicação continua **[nao verificado]**: a aplicação declara São Paulo; o ESTADO usa `datetime.date.today()` do host, e o formatter usa `asctime` sem conversão explícita de fuso nesses pontos. (`app/config/settings/base.py:93–95`; `bin/recusa_estado.py:64`; `app/config/settings/saas.py:122`.)

Há outra limitação de ausência de sinal: com o arquivo existente, erros de abertura são ignorados pelo leitor; isso pode terminar em zero, sem a distinção `sem_arquivo` feita anteriormente pelo wrapper. (`app/core/recusa_critica.py:95–105`, `:114–123`; `bin/recusa_estado.py:65–68`.)

**5. R8 e rc**

**Exceção em `medir` e retorno classificado como falhou deixam as seguintes medirem**, desde que persistência e emissão da falha terminem normalmente: ambos os ramos acrescentam a empresa a `falharam` e fazem `continue`. O fechamento retorna rc 2 quando houve essas falhas ou divergências acumuladas. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:147–165`, `:251–259`.)

O isolamento não cobre qualquer exceção da empresa: `assumir`, `concluir` e a persistência/emissão feita por `encerrar` ficam fora da proteção de `medir`. Uma falha nesses pontos pode abortar as seguintes. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:140–154`.)

**O rc não fica vermelho para toda falha descoberta na rodada.** Exemplo estático: `vigiar_abertos` marca FALHOU um PEDIDO expirado; o job com aquele `--tentativa` não consegue adotá-lo, entra em `compartilhadas` e termina pelo retorno normal. Essa falha não entra em `falharam`. (`app/folha/porta_export.py:1200–1205`, `:1112–1116`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:65`, `:128`, `:140–146`, `:258–265`.)

A parcela global `cartao_x_txt` foi preservada: continua medida para a competência inteira e integrada às falhas de cada empresa. R8 não a tornou artificialmente empresarial. (`app/folha/porta_export.py:667`, `:723–727`, `:753–764`.)

**6. Classes de reprovação e força dos testes** [nao verificado]

- **Clone de juiz registrado: não demonstrado.** A medição continua chamando `classificar_export`, autoridade registrada; `resultado_de` valida/classifica o retorno da medição. Existe, porém, classificação paralela do resultado na leitura: o registro guarda `resultado/n`, enquanto a porta volta a interpretar o escalar do carimbo. (`app/core/juizes.py:329–331`; `app/folha/porta_export.py:469`, `:1136–1147`, `:1171`, `:1252–1257`.)
- **Fallback presente:** carimbo sozinho responde quando falta pedido. **Segundo escritor operacional do carimbo: não identificado**; `concluir` passa por `lavrar`, e os escritores diretos externos apontados são fixtures. (`app/folha/porta_export.py:1168`, `:1235–1257`; `app/folha/tests/test_porta_do_export.py:33–42`.)
- **Relógio não é capturado uma vez para a leitura inteira:** pedido e carimbo consultam `_hoje()` separadamente. A gravação pelo job adotado conserva o dia do pedido, mas a leitura ainda pode atravessar a meia-noite entre as duas consultas. (`app/folha/porta_export.py:1004–1007`, `:1112–1117`, `:1168`, `:1238–1240`.)
- **Trilha incompleta permanece:** a recusa do término recebe `request=None`, produzindo usuário `-`, e não contém antes/depois. A auditoria do disparo tem ator/PID, mas não equivale à trilha das transições terminais exigida na rev2.r. (`app/folha/porta_export.py:1050–1053`, `:1311–1314`; `app/core/recusa_critica.py:77–85`; `CLAUDE.md:32–34`.)
- **Ordem de trava no caminho normal:** advisory lock → assumir/medir → trava da linha → carimbo/estado. Inversão concorrente demonstrada: **[nao verificado]**. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:129–154`; `app/folha/porta_export.py:1162–1171`.)

Os testes novos **têm mordida**, mas não provam tudo que seus nomes sugerem. (`app/folha/tests/test_porta_sem_beco_item2.py:42–57`, `:69–134`, `:168–190`.)

| Cobertura | O que efetivamente prova / o que falta |
|---|---|
| Identidade e atraso | Exercita persistência, CAS e carimbo com tentativa; as sucessões são sequenciais, sem corrida entre processos nesses testes. (`app/folha/tests/test_porta_sem_beco_item2.py:60–104`.) |
| R7 | Exercita `pg_locks` com backend gravado artificialmente inexistente e varredura de pedido de ontem; não mata um processo nem executa o encadeador. (`app/folha/tests/test_porta_sem_beco_item2.py:47–57`, `:137–146`.) |
| R8 | Mocka `medir`, mas usa o command e a persistência reais para os estados das duas empresas. É cobertura válida do isolamento dessas respostas; não prova recuperação de indisponibilidade real do banco. (`app/folha/tests/test_porta_sem_beco_item2.py:158–190`.) |
| Re-clique | O teste chamado “nenhuma por clique repetido” chama `conferir` três vezes, sem POST; o filtro ainda descarta linhas de outras portas. Esconde precisamente a costura do envelope. (`app/folha/tests/test_porta_sem_beco_item2.py:29–30`, `:214–223`.) |
| Meia-noite | Confere vínculo e mesmo `data_ref` na gravação; não confere uma leitura da porta atravessando a virada. (`app/folha/tests/test_porta_sem_beco_item2.py:249–268`.) |
| Contador | O teste novo conta linhas sintéticas já formatadas; apesar do nome, não exercita ausência de arquivo. O teste shell verifica ausência para o leitor anterior, sem chamar `ler_porta_export` nessa situação. (`app/folha/tests/test_porta_sem_beco_item2.py:273–289`; `bin/tests/test_recusa_estado.sh:29–43`.) |

O selo de soma em `relatorios` mocka conclusão, vigia, trava e adoção; serve para a agregação, sem comprovar essas costuras do item 2. Evidência de RED anterior e execução verde dos testes: **[nao verificado]**. (`app/relatorios/tests/test_selo_leitores_no_mesmo_numero.py:126–136`.)

**7. Quais pontos (ii) da rev2.r continuam abertos**

| Ponto da rev2.r | Situação nesta cópia |
|---|---|
| Posse da tentativa e conclusão atrasada | **Fechado no caminho normal**, pelo UUID/CAS e conclusão transacional; permanece a liberação excepcional sem pedido/tentativa preenchida. (`app/folha/porta_export.py:1056–1058`, `:1163–1171`, `:1247–1257`.) |
| PID gravado e conferido | **Parcial:** ambos gravados ao assumir; somente backend conferido; PID do processo ainda não é persistido no PEDIDO ao disparar. (`app/folha/porta_export.py:1073–1079`, `:1115`, `:1194`, `:1314`.) |
| Observador sem tela | **Implementado na declaração/caminho do cron**, condicionado à próxima execução do elo. Garantia operacional de periodicidade: **[nao verificado]**. (`app/config/crons.py:1803–1805`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:65`; `bin/encadeado.sh:84–95`.) |
| Resultado empresarial vinculado | **Persistência fechada; leitura parcial:** resultado/N estão no job, mas a porta continua classificando o carimbo. (`app/folha/porta_export.py:1170–1171`, `:1241–1257`.) |
| FALHOU × verde antigo | **Fechado.** (`app/folha/porta_export.py:1245–1249`.) |
| Dia e meia-noite | **Gravação vinculada fechada; leitura sem referência única permanece.** (`app/folha/porta_export.py:1168`, `:1007`, `:1238–1240`.) |
| Textos e lista | **Fechado no código:** literal de FALHA e fecho de DIVERGENTE, conservando a lista. (`app/folha/porta_export.py:828–872`, `:922–929`.) |
| Motivos, recusa única e trilha | **Ainda aberto:** motivo foi persistido, mas faltam garantia da emissão após interrupção, supressão da reemissão pelo POST e trilha terminal com ator/antes/depois. (`app/folha/porta_export.py:790`, `:1050–1060`, `:1170–1174`; `app/folha/views.py:135`; `app/core/respostas.py:54–56`; `CLAUDE.md:32–34`.) |
| Contador | **Integração fechada; dia operacional e completude do sinal não demonstrados integralmente.** (`bin/gerar_estado.py:267–270`; `bin/recusa_estado.py:64–68`; `app/core/recusa_critica.py:95–105`, `:117–121`.) |
| R8 | **Fechado para exceção de `medir` e resultado falhou com registro funcionando.** Não cobre falha na conclusão/persistência, nem torna vermelho o rc por falha descoberta apenas na varredura inicial. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:65`, `:140–165`, `:251–265`.) |

## 17/11 12:39 · sessao 5a018d6a0313 · E4C-F1-B2 · verificador · origem=Code

**A cerca ainda permite contorno e também recusa atos que não tocariam perguntas entregues.** Os dois problemas aparecem nos caminhos abaixo. (`app/ponto/management/commands/reconciliar_fantasmas.py:288`; `app/chamados/services/disputa_emissao.py:1891`)

Execução da suíte, RED contra o objeto Git `682ccefa4` e reprodução dinâmica dos contraexemplos: **[não verificado]**.

1. **A enumeração não coincide com a escrita efetiva.**

   Contraexemplo: A vivo; disputa D já fechada, cujo pai é A; pergunta Q entregue, materializada, não validada, **sem destino**, morta pelo C6. `_ancoradas()` inclui Q pelo pai, e a cerca a enumera. (`app/chamados/models.py:344`; `app/chamados/services/disputa_emissao.py:1942`)

   Nesse estado, Q não entra no direto, que exige destino A, materialização nula e disputa aberta; não provoca reabertura, pois está morta; D fica fora da varredura de abertas; a relavra S2 exige destino A. Portanto a enumeração inclui Q embora esses escritores não a escrevam. (`app/chamados/services/disputa_emissao.py:1830`; `app/chamados/services/disputa_emissao.py:1968`; `app/chamados/services/disputa_emissao.py:1839`; `app/chamados/models.py:309`)

   Mesmo assim, o dia entregue de Q produz veto e recusa A. Há outro sobrealcance, específico da previsão do fecho, no item 6. (`app/chamados/reconciliador.py:530`; `app/chamados/reconciliador.py:538`; `app/chamados/reconciliador.py:576`)

2. **A cerca não antecede a primeira escrita em todos os caminhos.**

   | Entrada | Constatação |
   |---|---|
   | `retratar` | Quando chega à consulta, o veto antecede a transição, o Fix A, o resolver explícito e a ocorrência. Porém a retenção anterior pode escrever trilha e retornar sem consultar competência. (`app/chamados/reconciliador.py:564`; `app/chamados/reconciliador.py:576`; `app/chamados/reconciliador.py:580`; `app/chamados/reconciliador.py:592`; `app/chamados/reconciliador.py:593`) |
   | `reconciliar_geofence` | Delega a `retratar`; separa retorno verdadeiro de falso e só registra sucesso quando verdadeiro. (`app/ponto/services/geofence.py:444`; `app/ponto/services/geofence.py:448`; `app/ponto/services/geofence.py:449`) |
   | `reconciliar_fantasmas` | Interrompe somente pelo motivo `competencia_entregue`. Uma recusa por `retencao` continua no resolver direto. (`app/ponto/management/commands/reconciliar_fantasmas.py:284`; `app/ponto/management/commands/reconciliar_fantasmas.py:288`) |
   | Ramo HIST, antigo ponto `1685` | A cópia chama primeiro `reapontar_resolvedora` em `1700`, que grava destino e lavra; depois chama o resolver em `1706`. A guarda da costura permite o destino terminal quando a competência é entregue. (`app/chamados/reconciliador.py:1700`; `app/chamados/services/fio.py:824`; `app/chamados/services/fio.py:973`; `app/chamados/reconciliador.py:1706`) |
   | `becos.py:55` | Chama diretamente o resolver depois das travas, sem consultar a cerca da retratação. O wrapper do resolver também só trata a via de tranca e adquire travas. (`app/chamados/services/becos.py:52`; `app/chamados/services/becos.py:55`; `app/chamados/services/disputa_emissao.py:2021`; `app/chamados/services/disputa_emissao.py:2027`) |
   | S143 | Pode chamar o resolver após qualquer save elegível de encerramento; sua entrada não exige passagem por `retratar_com_motivo`. (`app/chamados/signals.py:104`; `app/chamados/signals.py:112`; `app/chamados/signals.py:124`) |

   **Contorno concreto por retenção:** considere A elegível em fantasmas, com resposta completa sem veredito creditada a ele; e outra disputa aberta do mesmo colaborador, sem credor vivo, contendo Q entregue, morta, não validada nem materializada. A resposta retém A antes da cerca; o comando recebe `'retencao'` e chama o resolver. (`app/chamados/juizes.py:2115`; `app/chamados/reconciliador.py:449`; `app/chamados/reconciliador.py:564`; `app/ponto/management/commands/reconciliar_fantasmas.py:288`)

   O resolver varre todas as disputas abertas do colaborador, seleciona Q, passa pela guarda de vitalidade e a carimba. Esse caminho escreve a pergunta entregue sem consultar o veto; `recusados_competencia_entregue` permanece zero porque só incrementa na recusa explícita da cerca. (`app/chamados/services/disputa_emissao.py:2168`; `app/chamados/services/disputa_emissao.py:1867`; `app/chamados/services/disputa_emissao.py:2182`; `app/chamados/services/disputa_emissao.py:2194`; `app/ponto/management/commands/reconciliar_fantasmas.py:285`)

3. **Há classes reprovadoras presentes.**

   - **Derivação paralela do fecho/alcance:** `_disputa_fecha_no_ato` chama C7 sobre uma lista reduzida, descartando perguntas apenas potencialmente escritas. O escritor consulta C7 sobre o estado real restante. Usar o mesmo juiz com universos diferentes não estabelece equivalência; o contraexemplo do item 6 mostra a divergência. (`app/chamados/services/disputa_emissao.py:1890`; `app/chamados/services/disputa_emissao.py:1892`; `app/chamados/services/disputa_emissao.py:2225`; `app/chamados/models.py:1943`)
   - **Defesa 1:** não houve duplicação literal do predicado extraído: receptor e enumerador usam `defesa_1_reconcilia`. Entretanto, a previsão ignora o gate do pai, enquanto o receptor respeita sua recusa; isso amplia indevidamente o conjunto previsto como encerrado. (`app/chamados/signals.py:23`; `app/chamados/signals.py:41`; `app/chamados/services/disputa_emissao.py:1899`; `app/chamados/services/disputa_emissao.py:1909`; `app/chamados/signals.py:50`)
   - **Escritor sem cerca:** continuam existindo entradas diretas ao resolver compartilhado; o problema demonstrado é ausência da proteção nessas entradas, embora o carimbo ainda passe pela porta `carimbar`. (`app/chamados/services/becos.py:55`; `app/chamados/services/disputa_emissao.py:2028`; `app/chamados/services/disputa_emissao.py:2194`)
   - **Relógio:** a nova previsão consulta C7 sem fornecer um instante comum; C6 pode chegar a `date.today()`. Divergência efetiva numa virada temporal: **[não verificado]**. (`app/chamados/services/disputa_emissao.py:1892`; `app/chamados/models.py:1944`; `app/chamados/juizes.py:596`)
   - **Ordem de trava:** no caminho protegido, a cerca usa a porta global antes da transição; essa porta adquire Colaborador → Disputa → Chamado → Pergunta. Inversão nova nesse trecho: **[não verificado]**. (`app/chamados/reconciliador.py:575`; `app/chamados/models.py:2781`; `app/chamados/models.py:2790`; `app/chamados/models.py:2803`; `app/chamados/models.py:2807`)
   - **Valor com descrição incompleta:** `relatorio['recusados']` recebe qualquer retorno falso, incluindo `'transicao'`, embora a saída diga apenas “retenção ou competência ENTREGUE”. (`app/ponto/services/geofence.py:448`; `app/chamados/reconciliador.py:543`; `app/chamados/reconciliador.py:581`; `app/ponto/management/commands/reconciliar_geofence.py:46`)

4. **Escritores e leitores dos valores alterados e dos principais campos usados na composição.**

   | Campo/valor | Quem escreve/produz | Quem lê |
   |---|---|---|
   | `(bool, motivo)`: `'retencao'`, `'competencia_entregue'`, `'transicao'`, `None` | `retratar_com_motivo`, nos quatro retornos. (`app/chamados/reconciliador.py:567`; `:579`; `:581`; `:598`) | `_retratar` descarta o motivo; fantasmas o recebe e distingue somente competência. (`app/chamados/reconciliador.py:543`; `app/ponto/management/commands/reconciliar_fantasmas.py:281`; `:284`) |
   | Lista de empresas vetadas | Agrupamento por empresa e composição do veto. (`app/chamados/reconciliador.py:533`; `:538`) | Unidade de retratação e escritor da retenção. (`app/chamados/reconciliador.py:576`; `:578`; `:477`) |
   | Trilha `retratacao_retida_competencia_entregue`; `empresas_vetadas`; ação/modelo/objeto/descrição | Payload em `registrar_retencao`; persistência por `evento`. (`app/chamados/reconciliador.py:477`; `:491`; `app/core/observ.py:134`) | Deduplicação lê o último `valor_depois`; teste conta a trilha. (`app/chamados/reconciliador.py:480`; `:487`; `app/chamados/tests/test_e4c_b2_cerca_retratar.py:174`) |
   | `recusa_entregue` / `recusados_competencia_entregue` | Inicialização, incremento e publicação no comando. (`app/ponto/management/commands/reconciliar_fantasmas.py:169`; `:285`; `:321`) | Asserção do caso f. (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:377`) |
   | `relatorio['retratados'/'recusados']` | Serviço geofence acrescenta o PK conforme `ok`. (`app/ponto/services/geofence.py:448`) | Comando conta cada lista separadamente. (`app/ponto/management/commands/reconciliar_geofence.py:47`) |
   | `perdeu`, `acusam`, total de ocorrências | Reavaliação geofence calcula e acrescenta os chamados sem premissa. (`app/ponto/services/geofence.py:433`; `:437`) | Aplicação e apresentação do comando; continuam representando perda de premissa. (`app/ponto/services/geofence.py:443`; `app/ponto/management/commands/reconciliar_geofence.py:35`) |
   | `Chamado.status_local` | `_gravar_estado` atribui e salva. (`app/chamados/models.py:228`; `:296`) | Seleção HX e predicado da Defesa 1. (`app/chamados/services/disputa_emissao.py:1860`; `app/chamados/signals.py:23`) |
   | `Disputa.fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento` | Porta `fechar`. (`app/chamados/models.py:2023`; `:2027`) | Seleção HX e receptores do fecho. (`app/chamados/services/disputa_emissao.py:1856`; `app/chamados/signals.py:36`; `:172`) |
   | `materializada_em`, `materializada_por`, `via_resolucao` | `_escrever_carimbo`, persistido por `carimbar`; S128, HX e S144 chamam essa porta. (`app/chamados/models.py:3220`; `:3273`; `app/chamados/services/disputa_emissao.py:2129`; `:2194`; `app/chamados/services/materializacao.py:2167`) | Seleções direta/HX/FECHO e C6. (`app/chamados/services/disputa_emissao.py:1832`; `:1845`; `:1948`; `app/chamados/juizes.py:1404`) |
   | `respondida_em`, `resposta_colab`, `validada_em`, `validada_por` | `carimbar` pode completar instante/validação quando há texto; validação humana também grava seu par. (`app/chamados/models.py:3251`; `app/chamados/services/validacao.py:168`) | C21, C6 e seleções HX/âncoras. (`app/chamados/juizes.py:1552`; `:1405`; `app/chamados/services/disputa_emissao.py:1845`; `app/chamados/models.py:344`) |
   | `data_evento`, `conversa` | Dia atribuído por `gravar_dia_do_fato`; nascimento informa conversa. (`app/chamados/services/fio.py:848`; `app/chamados/models.py:3134`) | Juiz do dia distingue conversa; cerca exclui conversa e consulta esse juiz. (`app/chamados/juizes.py:342`; `app/chamados/reconciliador.py:531`; `:534`) |
   | `chamado_destino`, vínculo à disputa/célula | Nascimento da pergunta; costura pode trocar o destino. (`app/chamados/models.py:3134`; `app/chamados/services/fio.py:973`) | Direto, HX, previsão de fecho e âncoras. (`app/chamados/services/disputa_emissao.py:1831`; `:1865`; `:1891`; `app/chamados/models.py:345`) |
   | `veredito_lavrado`, `julgado_em`, `janela_expira_em` | `lavrar`; S2 chama após mudança de estado, e a costura salva a mesma lavra. (`app/chamados/veredito_lavrado.py:270`; `app/chamados/models.py:312`; `app/chamados/services/fio.py:975`) | Fotografia dos testes observa os campos persistidos da pergunta. (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:164`) |
   | Justificativa `status`/`descricao`; ocorrência da retratação | Fix A rejeita justificativa pendente; a ocorrência passa por `nota.anexar_ocorrencia`. (`app/chamados/reconciliador.py:588`; `:592`; `:595`) | Retenção lê o status; teste de controle confere a rejeição. (`app/chamados/reconciliador.py:454`; `app/chamados/tests/test_e4c_b2_cerca_retratar.py:300`) |

   Censo global exaustivo de todos os demais escritores desses campos fora dos caminhos examinados: **[não verificado]**.

5. **Os testes têm mordida parcial; não demonstram uniformemente o motivo anunciado.** [nao verificado]

   - **a/b/n:** `_pergunta` verifica C6 somente na instância recém-criada. O resolver recarrega perguntas e pula as vivas. Se a pergunta está morta apenas em memória, isso não invalida testar a recusa preventiva, mas invalida usar o caso como prova de que aquele S128/HX carimbaria a pergunta; a fotografia pode falhar só pela alteração do chamado ou justificativa. (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:135`; `:162`; `:196`; `app/chamados/services/disputa_emissao.py:2083`; `:2109`; `:2182`)
   - **b2:** a discrepância não elimina o caminho da relavra: S2 busca destino igual ao chamado e validação nula, sem exigir pergunta morta. (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:272`; `app/chamados/models.py:309`)
   - **c:** é controle positivo, esperado verde, não RED. (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:288`)
   - **h1/h2:** exercitam os pulos por pai/destino vivo, mas não cobrem a pergunta viva removida da projeção HX no contraexemplo do item 6. (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:302`; `:311`; `app/chamados/services/disputa_emissao.py:1890`)
   - **k/s:** exigem igualdade da própria pergunta entregue antes da fotografia geral, uma verificação mais específica. O caso **s**, porém, grava texto vazio com `respondida_em` preenchido: testa uma validada com par ilegal legado, e não uma resposta completa segundo C21. (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:205`; `:334`; `:347`; `:357`; `app/chamados/juizes.py:1552`)
   - **f:** corrige a célula no banco e verifica C6 por objeto fresco; **g** verifica especificamente a contagem de sucesso. O caso f não exercita o contorno por `'retencao'` demonstrado no item 2. (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:371`; `:372`; `:393`; `app/ponto/management/commands/reconciliar_fantasmas.py:284`)
   - **Anti-vacuidade:** o harness aceita mensagens amplas de `AssertionError`; portanto “morde” não significa necessariamente escrita da pergunta-alvo. As novas allowlists também dispensam a fiscalização dessas funções, sem provar equivalência com os escritores. (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:455`; `app/chamados/tests/test_contract_leitores_vitalidade.py:103`; `:356`)

   Pelo diff fornecido, a cerca já existia na base da volta; a/b/b2/n não constituem novos REDs dessa volta pela ausência da cerca. A nova exigência nesses casos inclui trilha/deduplicação. Execução contra a base e causa exata de cada falha: **[não verificado]**. (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:203`; `:257`)

6. **Sim: a superaproximação pode recusar retratação legítima em competência não entregue.**

   Caminho específico pelo **destino que encerra**:

   - A é cobrança duplicada de dia aberto; S é o sobrevivente válido do mesmo dia. Q viva aponta para A numa disputa dB cujo pai é B. O gate permite fechar A por dedup, reatando Q para S antes de gravar o encerramento. (`app/chamados/models.py:795`; `app/chamados/models.py:389`; `app/chamados/models.py:435`; `app/chamados/models.py:655`)
   - No ato real, Q continua viva com destino S vivo: HX pula a disputa e C7 impede seu fecho; portanto a Defesa 1 não encerra B por esse caminho. (`app/chamados/juizes.py:1456`; `app/chamados/services/disputa_emissao.py:1865`; `app/chamados/models.py:1944`; `app/chamados/services/disputa_emissao.py:2225`)
   - A previsão remove Q pelo destino **antigo A**, considera dB fechável e acrescenta B à cascata. Uma pergunta R entregue, de destino B, em disputa fechada, entra então no alcance previsto e veta A — embora B não fosse encerrado nem R relavrada pelo ato real. (`app/chamados/services/disputa_emissao.py:1891`; `:1911`; `:1938`; `:1942`; `app/chamados/reconciliador.py:538`)

   Há também a variante HX: pergunta Q viva de escolha em dia aberto, sem credor vivo, e irmã R validada não materializada em dia entregue. A previsão remove Q como candidata HX e prevê fecho; o escritor pula Q viva e não fecha. R é acrescentada indevidamente pelo FECHO. (`app/chamados/services/disputa_emissao.py:1845`; `:1890`; `:1948`; `:2182`; `app/chamados/juizes.py:1405`; `app/chamados/models.py:1944`)

   A recusa nesses exemplos depende de uma pergunta entregue indevidamente incluída. Se **todos** os dias enumerados forem conhecidos e não entregues, o veto retorna falso. (`app/chamados/services/fio.py:1365`)

VEREDITO: REPROVA -- achados: contorno por retenção (`app/ponto/management/commands/reconciliar_fantasmas.py:288`); resolver sem cerca (`app/chamados/services/disputa_emissao.py:2028`); sobrealcance (`app/chamados/services/disputa_emissao.py:1891`); mordida inespecífica (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:455`).

## 17/11 12:36 · sessao 09fa193ca3fa · - · - · origem=Ronald (tmux)

Achados confirmados por leitura:

1. **Enumeração e escrita efetiva diferem por sobrealcance.** `_ancoradas()` inclui pergunta sem destino ancorada pelo pai, mesmo materializada e em disputa fechada (`app/chamados/models.py:336`, `app/chamados/models.py:344`). O enumerador incorpora esse universo inteiro (`app/chamados/services/disputa_emissao.py:1942`). Contraexemplo: A vivo, d fechada com pai A, q entregue, materializada, não validada, sem destino e morta pelo C6. O resolver não a carimba diretamente, pois exige destino A, materialização nula e disputa aberta (`app/chamados/services/disputa_emissao.py:1830`); a tentativa de reabertura termina sem ato porque q está morta (`app/chamados/services/disputa_emissao.py:1968`); d fechada fica fora da varredura aberta (`app/chamados/services/disputa_emissao.py:1839`); a relavra S2 exige destino A (`app/chamados/models.py:309`). Mesmo assim, q veta A (`app/chamados/reconciliador.py:530`, `app/chamados/reconciliador.py:538`). Reprodução executada: [não verificado].

2. **A projeção do fecho também produz falso positivo.** dX aberta sem pai, q de escolha viva, não validada e sem destino em competência aberta; r validada, não materializada, sem destino em competência entregue. HX seleciona somente q (`app/chamados/services/disputa_emissao.py:1845`, `app/chamados/services/disputa_emissao.py:1867`). O resolver real pula q viva (`app/chamados/services/disputa_emissao.py:2182`) e C7 impede fechar dX (`app/chamados/models.py:1944`). A preditora remove q como se HX a tivesse escrito (`app/chamados/services/disputa_emissao.py:1890`); r validada está morta pelo C6 (`app/chamados/juizes.py:1405`), logo prevê fecho e incorpora r (`app/chamados/services/disputa_emissao.py:1947`). A acaba recusada embora r não fosse tocada (`app/chamados/reconciliador.py:576`). Reprodução executada: [não verificado].

3. **Variante pedida em Q6, pelo destino encerrando.** A é cobrança duplicada em competência aberta, sustentada por sobrevivente S do mesmo dia; q viva aponta inicialmente para A em dB com pai B. O gate permite dedup apoiado em S (`app/chamados/models.py:795`), planeja e aplica reaponte de q para S (`app/chamados/models.py:389`, `app/chamados/models.py:435`) antes de gravar A (`app/chamados/models.py:689`, `app/chamados/models.py:655`). Portanto, q permanece viva, destino S vivo impede HX e C7 segura dB (`app/chamados/juizes.py:1456`, `app/chamados/services/disputa_emissao.py:1865`, `app/chamados/models.py:1944`). A preditora, porém, elimina q pelo destino **antigo** A, presente em `encerrando` (`app/chamados/services/disputa_emissao.py:1891`), pode prever fecho de dB e incluir B na cascata (`app/chamados/services/disputa_emissao.py:1911`). Uma pergunta entregue r de destino B em disputa fechada passa a entrar no alcance projetado, embora B não fosse efetivamente encerrado (`app/chamados/services/disputa_emissao.py:1938`, `app/chamados/services/disputa_emissao.py:1942`). Assim pode recusar a retratação legítima aberta de A (`app/chamados/reconciliador.py:538`). Reprodução executada: [não verificado].

4. **Defesa 1 não ganhou cópia literal do seu predicado de seleção:** ambos usam `defesa_1_reconcilia` (`app/chamados/signals.py:19`, `app/chamados/signals.py:41`, `app/chamados/services/disputa_emissao.py:1909`). Entretanto, a predição ignora deliberadamente o gate real de reconciliação do pai (`app/chamados/services/disputa_emissao.py:1899`), enquanto o receiver respeita sua recusa (`app/chamados/signals.py:50`). Isso acrescenta sobreaproximação ao alcance, além da diferença no universo fornecido a C7 (`app/chamados/services/disputa_emissao.py:1891`, `app/chamados/models.py:1943`).

5. Ressuscitação pode reabrir disputas inicialmente fechadas (`app/chamados/models.py:1648`), enquanto `_disputas_do_resolver` parte das abertas e do direto anterior (`app/chamados/services/disputa_emissao.py:1873`). Um subalcance concreto decorrente dessa mutação não foi fechado nesta leitura: [não verificado].

A recusa de uma retratação aberta nos contraexemplos depende de uma pergunta entregue acrescentada indevidamente ao alcance; se todos os dias enumerados forem conhecidos e não entregues, o veto retorna falso (`app/chamados/services/fio.py:1365`).

**Conclusão do subexame: REPROVA por sobrealcance demonstrável no código; equivalência entre enumeração e escrita efetiva não se sustenta** (`app/chamados/services/disputa_emissao.py:1891`, `app/chamados/services/disputa_emissao.py:1942`, `app/chamados/services/disputa_emissao.py:2182`).

## 17/11 12:36 · sessao 433d50b4830a · - · - · origem=Ronald (tmux)

A validação dinâmica dos testes e do RED na base é **[não verificado]**; a leitura do código permite separar os seguintes pontos. [nao verificado]

| Casos | O que o teste demonstra / limite | [nao verificado]
|---|---|
| **a, b, n** | `_pergunta` consulta C6 somente na instância recém-criada, sem recarregar a célula (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:135-142`). O resolver carrega instâncias novas e pula perguntas VIVAS (`app/chamados/services/disputa_emissao.py:2083-2084`, `:2109-2110`, `:2181-2183`). Assim, a discrepância memória/banco não invalida testar a recusa preventiva, mas invalida afirmar que esses cenários provam o carimbo descrito; a fotografia pode falhar apenas pela alteração do chamado ou da justificativa (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:162-169`, `:196-203`). |
| **b2** | A discrepância não elimina a prova da relavra: `_gravar_estado` busca perguntas de destino igual ao chamado, não validadas, e as relavra ao cruzar ENCERRADOS, sem exigir pergunta MORTA (`app/chamados/models.py:303-311`). O teste coloca a pergunta materializada e a disputa fechada, mantendo-a não validada (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:267-278`). |
| **c** | É controle positivo e deve permanecer verde; não constitui RED da base (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:288-300`). |
| **h1, h2** | Exercitam o sobrealcance: pergunta entregue VIVA de disputa cujo pai ou destino de irmã permanece vivo (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:302-318`). A precondição de vitalidade usa a instância construída com célula explicitamente atualizada para `furo` (`:224-234`). |
| **k** | O teste exige que a própria pergunta entregue permaneça idêntica, além da fotografia geral; portanto o mutante não pode ser considerado morto somente pela alteração do chamado (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:205-211`, `:320-334`). |
| **s** | Também exige identidade da própria pergunta entregue e consulta C6 por `GET` fresco após corrigir a célula (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:347-357`). Porém não é “RESPONDIDA E VALIDADA” como anunciado: grava `resposta_colab=''` com `respondida_em` preenchido (`:347-348`), e C21 exige instante **e texto** (`app/chamados/juizes.py:1544-1552`). Prova S144 sobre pergunta validada com par ilegal legado. |
| **f** | Corrige o banco e confirma C6 com objeto recarregado antes de executar o comando; isso remove o problema memória/banco para demonstrar o fallback (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:366-377`). |
| **g** | Verifica que perder a premissa não equivale a reconciliar: exige zero reconciliados e uma recusa (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:379-394`). |

**Mordida incompleta:** o harness aceita qualquer `AssertionError` que contenha uma das mensagens amplas permitidas; alterar apenas chamado/justificativa já pode produzir “morde”, sem comprovar escrita da pergunta descrita (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:196-203`, `:403-404`, `:445-455`). Apenas **k/s** passam `entregue=p` e verificam primeiro a identidade da pergunta-alvo (`:205-211`, `:334`, `:357`).

**Base da volta:** a cerca já existia no trecho anterior do diff fornecido; logo **a/b/b2/n** não são novos REDs da volta pelo motivo anunciado. A nova exigência observável desses casos inclui a trilha da recusa e sua deduplicação (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:203`, `:245-257`); execução efetiva contra `682ccefa4` **[não verificado]**.

O novo harness de memória incorpora o corpo real do helper HX, sem substituir sua seleção por stub (`app/chamados/tests/test_e4b_classe_e_memoria.py:369-374`). As novas entradas da allowlist, por sua vez, dispensam a fiscalização AST dessas funções; não comprovam equivalência entre enumeração e escrita (`app/chamados/tests/test_contract_leitores_vitalidade.py:103-107`, `:354-358`).

## 17/11 12:36 · sessao b4b4c07052ef · - · - · origem=Ronald (tmux)

Achados de entrada/cerca:

- **Fora da cerca:** `resolver_perguntas_por_chamado_resolvido` só verifica via de tranca e adquire travas; não consulta `_retratacao_toca_entregue` nem `veto_de_competencia`. Seus chamadores diretos continuam chegando sem cerca: `becos.fechar_beco` e ramo HIST de `reatar_pergunta_orfa`. (`app/chamados/services/disputa_emissao.py:2012`, `app/chamados/services/disputa_emissao.py:2021`, `app/chamados/services/disputa_emissao.py:2027`, `app/chamados/services/becos.py:55`, `app/chamados/reconciliador.py:1706`)
- **Primeira escrita no HIST precede até o resolver:** `reatar_pergunta_orfa` chama `reapontar_resolvedora` em `:1700`; esta grava destino e lavra em `fio.py:973-976`, sem veto da unidade de retratação. `_costura_nao_mata_o_fio` considera competência entregue motivo para permitir a costura, não para recusá-la (`fio.py:824-825`). (`app/chamados/reconciliador.py:1700`, `app/chamados/services/fio.py:824`, `app/chamados/services/fio.py:973`)
- **Contorno sobrevivente em fantasmas:** `retencao_da_retratacao` retorna antes da cerca; `retratar_com_motivo` devolve `(False, 'retencao')`. O comando interrompe somente `competencia_entregue`; qualquer outra recusa cai no resolver direto. (`app/chamados/reconciliador.py:564`, `app/chamados/reconciliador.py:567`, `app/ponto/management/commands/reconciliar_fantasmas.py:284`, `app/ponto/management/commands/reconciliar_fantasmas.py:288`)
- Contraexemplo desse contorno: A é `turno_aberto` VIVO elegível no comando e tem resposta com texto/instante sem veredito creditada a A; isto produz retenção. Outra disputa D do mesmo colaborador, aberta, sem pai/destino vivo, possui Q ENTREGUE, MORTA, sem validação/materialização. O fallback do resolver inclui todas as disputas abertas do colaborador, seleciona Q e carimba Q sem consultar competência. (`app/ponto/management/commands/reconciliar_fantasmas.py:157`, `app/chamados/juizes.py:2115`, `app/chamados/reconciliador.py:449`, `app/chamados/services/disputa_emissao.py:2168`, `app/chamados/services/disputa_emissao.py:1856`, `app/chamados/services/disputa_emissao.py:2182`, `app/chamados/services/disputa_emissao.py:2194`)
- O contador desse caso fica zero, porque só incrementa na recusa explícita `competencia_entregue`; o contador não mede todas as tentativas que alcançam competência entregue. (`app/ponto/management/commands/reconciliar_fantasmas.py:169`, `app/ponto/management/commands/reconciliar_fantasmas.py:284`, `app/ponto/management/commands/reconciliar_fantasmas.py:321`)
- Em `retratar`, pelo caminho que chega à cerca, ela antecede a transição, Fix A, resolver explícito e ocorrência. A retenção anterior tem escrita de trilha antes dela. (`app/chamados/reconciliador.py:564`, `app/chamados/reconciliador.py:576`, `app/chamados/reconciliador.py:580`, `app/chamados/reconciliador.py:592`, `app/chamados/reconciliador.py:593`, `app/chamados/reconciliador.py:595`)
- Geofence delega a `retratar` e só registra sucesso se o retorno for verdadeiro; o comando conta listas distintas. (`app/ponto/services/geofence.py:444`, `app/ponto/services/geofence.py:447`, `app/ponto/services/geofence.py:449`, `app/ponto/management/commands/reconciliar_geofence.py:46`)
- Literalmente qualquer escrita: ambos os comandos podem criar User `sistema` antes da cerca via `get_or_create`; existência prévia do usuário [não verificado]. (`app/ponto/management/commands/reconciliar_fantasmas.py:165`, `app/ponto/services/geofence.py:442`, `app/core/utils.py:18`)

Mapa de valores novos/alterados:

| Campo/valor | Escritor | Leitor |
|---|---|---|
| Resultado `(bool, motivo)`; `None`, `'retencao'`, `'competencia_entregue'`, `'transicao'` | `app/chamados/reconciliador.py:567`, `:579`, `:581`, `:598` | Wrapper descarta motivo em `app/chamados/reconciliador.py:543`; fantasmas lê em `app/ponto/management/commands/reconciliar_fantasmas.py:281` e só distingue competência em `:284`. |
| Lista de empresas vetadas | `app/chamados/reconciliador.py:533`, `:538` | Unidade em `:576-578`; escritor de retenção em `:477`. |
| `LogAuditoria.acao='retratacao_retida_competencia_entregue'`, `modelo`, `objeto_id`, `descricao`, `valor_depois.empresas_vetadas` | Payload em `app/chamados/reconciliador.py:477`; persistência única em `app/core/observ.py:134` | Dedup em `app/chamados/reconciliador.py:480`, `:487`; teste conta em `app/chamados/tests/test_e4c_b2_cerca_retratar.py:174`. |
| `recusa_entregue` / saída `recusados_competencia_entregue` | Inicializa `app/ponto/management/commands/reconciliar_fantasmas.py:169`; incrementa `:285`; publica `:321` | Teste em `app/chamados/tests/test_e4c_b2_cerca_retratar.py:377`; outros leitores de produção [não verificado]. |
| `relatorio['retratados'/'recusados']` | `app/ponto/services/geofence.py:448` | `app/ponto/management/commands/reconciliar_geofence.py:47`. |
| `perdeu` | `app/ponto/services/geofence.py:423`, `:437` | Laço de aplicação `:443`; retorno `:454`; comando `app/ponto/management/commands/reconciliar_geofence.py:35`. Continua significando perda de premissa, separado de sucesso. |
| Predicado `defesa_1_reconcilia` | `app/chamados/signals.py:23` | Receptor em `:41`; enumerador da cascata em `app/chamados/services/disputa_emissao.py:1912`. |

Campos de estado lidos na enumeração e escritores efetivos do ato:

- `Chamado.status_local`: escritor `_gravar_estado` atribui em `models.py:228` e persiste em `:296`; Defesa 1 consulta helper em `signals.py:41`; HX consulta em `disputa_emissao.py:1860`. O diff não cria outro escritor de status. (`app/chamados/models.py:228`, `app/chamados/models.py:296`, `app/chamados/signals.py:41`, `app/chamados/services/disputa_emissao.py:1860`)
- `Disputa.fechada_em/fechada_por/resolucao/motivo_fechamento`: porta `Disputa.fechar` grava em `models.py:2023-2027`; helper HX lê `fechada_em` em `disputa_emissao.py:1856`; receptor lê em `signals.py:36`. (`app/chamados/models.py:2023`, `app/chamados/models.py:2027`, `app/chamados/services/disputa_emissao.py:1856`, `app/chamados/signals.py:36`)
- `Pergunta.materializada_em/materializada_por/via_resolucao`: atribuição única `_escrever_carimbo` em `models.py:3220-3222`, persistida por `carimbar` em `:3273`; seleção HX lê em `disputa_emissao.py:1845`, seleção direta em `:1830-1834`. (`app/chamados/models.py:3220`, `app/chamados/models.py:3273`, `app/chamados/services/disputa_emissao.py:1845`, `app/chamados/services/disputa_emissao.py:1830`)
- `validada_em/validada_por/respondida_em`: `carimbar` pode completar os campos em `models.py:3251-3258`; validação humana tem escritor em `validacao.py:168-171`; descarimbo restaura/limpa por outras portas. Enumeração usa `validada_em` como seleção em `disputa_emissao.py:1845`, `_ancoradas` em `models.py:344`. (`app/chamados/models.py:3251`, `app/chamados/services/validacao.py:168`, `app/chamados/models.py:344`, `app/chamados/services/disputa_emissao.py:1845`)
- `data_evento`: atribuição central em `fio.py:848`; `_dia_de_turno_da_pergunta` lê-o e distingue `conversa` em `juizes.py:342`; cerca aplica em `reconciliador.py:531-534`. (`app/chamados/services/fio.py:848`, `app/chamados/juizes.py:342`, `app/chamados/reconciliador.py:531`)
- `chamado_destino`: costura grava em `fio.py:973-976`; nascimento/reuso são outros escritores declarados em `fio.py:855-862`; seleção direta/HX/âncoras lê em `disputa_emissao.py:1831`, `:1865`, `models.py:345`. (`app/chamados/services/fio.py:973`, `app/chamados/services/fio.py:855`, `app/chamados/services/disputa_emissao.py:1831`, `app/chamados/models.py:345`)
- Dados geofence: emissão monta ocorrências em `geofence.py:345-351`, grava nascimento em `:375-392`, acrescenta ocorrência em `:359`; reavaliação lê contexto/GPS/posto em `:425-435`. (`app/ponto/services/geofence.py:345`, `app/ponto/services/geofence.py:359`, `app/ponto/services/geofence.py:375`, `app/ponto/services/geofence.py:425`)

Observação menor: `recusados` é qualquer `False`, incluindo `'transicao'` cujo motivo o wrapper descarta; o texto do comando restringe a descrição a “retenção ou competência ENTREGUE”. (`app/ponto/services/geofence.py:448`, `app/chamados/reconciliador.py:543`, `app/chamados/reconciliador.py:581`, `app/ponto/management/commands/reconciliar_geofence.py:46`)

## 17/11 12:16 · sessao caca8ade9cf4 · E4C-F2 · verificador · origem=Code

**REPROVADO.** A corrida original de duas ressuscitações foi fechada, mas permanecem duas falhas: a trilha e a reabertura de disputas podem incluir perguntas não ressuscitadas; uma resposta concorrente pode ser confirmada depois da ressuscitação e continuar apagada na leitura do ato. (`app/chamados/models.py:1642`, `app/chamados/models.py:1701`, `app/chamados/models.py:1719`; `app/chamados/services/disputa_emissao.py:1476`, `app/chamados/services/disputa_emissao.py:1520`; `app/chamados/models.py:4297`, `app/chamados/models.py:4307`)

1. **Concorrência, trilha e disputas**

**Sim, a revalidação fecha a corrida descrita na v2.** A primeira chamada limpa a materialização; a segunda, depois de adquirir as travas, retira esse PK antes das fotos e da compensadora. Foto, R e limpeza permanecem no mesmo `atomic`. (`app/chamados/models.py:1629`, `app/chamados/models.py:1634`, `app/chamados/models.py:1642`, `app/chamados/models.py:1644`, `app/chamados/models.py:1690`, `app/chamados/models.py:1692`)

**Não demonstrei outra interleaving que grave R marcada duplicada pelo mesmo estado ou R marcada sem o UPDATE correspondente, mantidas essas travas.** R e UPDATE excluem as mesmas vias fora do fato, sobre PKs fixados sob a trava. Rematerializar entre duas chamadas significa retirar outro estado, não duplicar necessariamente a compensação anterior. (`app/chamados/models.py:1642`, `app/chamados/models.py:1643`, `app/chamados/models.py:1676`, `app/chamados/models.py:2843`, `app/chamados/models.py:2869`, `app/chamados/models.py:3467`)

**A trilha `ids` e a lista `disputas`, entretanto, não continuam corretas em geral.** Esta interleaving usa uma porta existente:

| Passo | Consequência |
|---|---|
| A seleciona `p1/D1` e `p2/D2`, guardando ambas as disputas antes da trava. | O modelo permite perguntas de disputas distintas no mesmo destino. (`app/chamados/models.py:1622`, `app/chamados/models.py:1623`, `app/chamados/models.py:2593`, `app/chamados/models.py:3629`) |
| B remove o último motivo de D1. | `remover_motivo` retrata p1, conserva sua materialização e fecha D1. (`app/chamados/models.py:2214`, `app/chamados/models.py:2215`, `app/chamados/models.py:2225`, `app/chamados/models.py:3368`) |
| A adquire as travas. | p1 continua em `ids`, pois a revalidação só exige materialização; R e UPDATE a excluem pela via retratada. p2 volta, produzindo `n > 0`. (`app/chamados/models.py:1642`, `app/chamados/models.py:1676`, `app/chamados/models.py:1692`, `app/chamados/models.py:3467`) |
| A conclui. | O log declara p1 e p2 ressuscitadas; o laço também reabre D1, embora nenhuma pergunta dela tenha voltado. A reabertura apaga resolução e campos do fechamento. (`app/chamados/models.py:1700`, `app/chamados/models.py:1701`, `app/chamados/models.py:1719`, `app/chamados/models.py:1720`, `app/chamados/models.py:2326`) |

A fotografia inicial também recebe esse universo maior: pode gravar fotos históricas de p1, embora a compensadora e a limpeza depois a excluam. (`app/chamados/models.py:1644`, `app/chamados/models.py:1645`, `app/chamados/models.py:4243`, `app/chamados/models.py:4248`)

A correção necessária é fixar sob a trava o conjunto efetivamente gravável e usá-lo nas fotos, R, limpeza e auditoria; as disputas a reabrir devem vir desse conjunto, preservando a aquisição antecipada das travas necessárias. (`app/chamados/models.py:1634`, `app/chamados/models.py:1642`, `app/chamados/models.py:1676`, `app/chamados/models.py:1701`, `app/chamados/models.py:3467`)

**Há ainda uma inversão temporal com a resposta humana, sem depender de atos futuros:**

- B prepara uma resposta em T1, capturando relógio e foto antes de pedir a trava do colaborador. Pausa antes da aquisição. (`app/chamados/services/disputa_emissao.py:1476`, `app/chamados/services/disputa_emissao.py:1480`, `app/chamados/services/disputa_emissao.py:1520`)
- A ressuscita a pergunta ainda materializada: grava R em T2, com `T2 > T1`, limpa e conclui. (`app/chamados/models.py:1642`, `app/chamados/models.py:1673`, `app/chamados/models.py:1685`, `app/chamados/models.py:1692`)
- B passa pela trava e grava a projeção e RESPOSTA com T1. A leitura ordena RESPOSTA/T1 antes de R/T2, retornando resposta vazia; a projeção não fornece outra linha virtual porque RESPOSTA/T1 já existe. (`app/chamados/services/disputa_emissao.py:1522`, `app/chamados/services/disputa_emissao.py:1525`; `app/chamados/models.py:4243`, `app/chamados/models.py:4292`, `app/chamados/models.py:4297`, `app/chamados/models.py:4307`)

A entrada é viável: uma pergunta materializada, sem resposta, com marco apagado e célula acusadora pode estar viva; API e HTML aceitam essa pergunta sem adquirir previamente a trava. (`app/chamados/juizes.py:1450`, `app/chamados/juizes.py:1456`; `app/api/views.py:2613`, `app/api/views.py:2629`; `app/chamados/views.py:1239`, `app/chamados/views.py:1257`, `app/chamados/views.py:1284`)

A captura pré-trava já estava no escritor existente; a nova compensadora participa da divergência. O relógio da resposta precisa respeitar a ordem protegida da escrita. Ocorrência em produção e ensaio com duas conexões: **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:1476`, `app/chamados/services/disputa_emissao.py:1520`; `app/chamados/models.py:1685`)

2. **Mordida do RED_8**

Ele morde uma **subclasse**, não apenas o PK da fixture: seleção antiga cuja materialização já foi retirada. Injeta o universo velho, exige retorno zero e exige somente uma R; retirar a revalidação permite a segunda compensadora. (`app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:202`, `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:207`, `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:208`, `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:211`)

Não cobre seleção velha em toda a sua extensão: sobrevivência parcial, retratação mantendo materialização, auditoria dos IDs, disputas sem pergunta efetivamente tocada, alteração de resposta/marco/C6 ou inversão temporal. Também não testa espera real por locks: executa sequencialmente, substituindo apenas `_universo_da_ressuscitacao`. (`app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:192`, `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:205`, `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:207`, `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:213`)

3. **Tripwire ampliado e `LEITURAS`**

Não encontrei escritor concreto de RESPOSTA de produção omitindo o campo: os quatro montadores estão no mapa abaixo. As chamadas de RESPOSTA omitidas por `LEITURAS` nesta cópia são os `Q` da face SQL e da constraint; ambas são predicados, sem escritor escondido nesses sítios. (`app/chamados/services/disputa_emissao.py:1525`, `app/chamados/services/disputa_emissao.py:1679`, `app/chamados/services/disputa_emissao.py:1701`; `app/chamados/models.py:4245`, `app/chamados/models.py:3541`, `app/chamados/models.py:4200`)

A ampliação cobre chamadas com keyword reconhecível e dicionários literais, e a contraprova inclui construtor, `objects.create` e literal. Continua sem acompanhar fluxo de dados: ato em variável, `**kwargs`, atribuições posteriores ao objeto/dicionário e `setattr`. A exclusão de `get/filter/exclude/Q` considera apenas o nome, sem provar a identidade da função. (`app/chamados/tests/test_f2_resposta_mantem_validacao.py:29`, `app/chamados/tests/test_f2_resposta_mantem_validacao.py:42`, `app/chamados/tests/test_f2_resposta_mantem_validacao.py:46`, `app/chamados/tests/test_f2_resposta_mantem_validacao.py:79`)

O montador indireto encontrado, `reabrir_para_correcao`, restringe `ato` a REABERTURA/RECUSA; não constitui quinto escritor de RESPOSTA. A porta `registrar` continua aceitando omissão e aplicando o default `False`: o selo não é obrigatoriedade dinâmica. (`app/chamados/services/disputa_emissao.py:1620`, `app/chamados/services/disputa_emissao.py:1633`; `app/chamados/models.py:4149`, `app/chamados/models.py:4179`, `app/chamados/models.py:4322`)

4. **Triagem v2**

| Item | Conferência |
|---|---|
| Cura da idempotência | Procede para a corrida original; não demonstra correção dos efeitos laterais nem da ordenação temporal acima. (`app/chamados/models.py:1642`, `app/chamados/models.py:1701`, `app/chamados/models.py:1719`; `app/chamados/services/disputa_emissao.py:1476`) |
| Futuro: descarte por medida | Procede somente como ausência no retrato raspado. Conferência independente da sonda: **[nao verificado]**. A porta continua usando `now()` e a leitura ordena todos os atos; a medida também não afasta a inversão temporal descrita, que dispensa atos futuros. (`app/chamados/models.py:1673`, `app/chamados/models.py:4297`; `app/chamados/services/disputa_emissao.py:1476`) |
| Tripwire só reconhecia `dict` | Cura procede para os formatos acrescentados; permanece a limitação de montagem indireta declarada. (`app/chamados/tests/test_f2_resposta_mantem_validacao.py:7`, `app/chamados/tests/test_f2_resposta_mantem_validacao.py:42`, `app/chamados/tests/test_f2_resposta_mantem_validacao.py:46`, `app/chamados/tests/test_f2_resposta_mantem_validacao.py:79`) |
| `NULL` em motivo | Descarte procede no contrato do esquema: coluna sem `null=True`. Estoque fora desse contrato: **[nao verificado]**. (`app/chamados/migrations/0051_decisao_pergunta.py:48`, `app/chamados/models.py:4156`) |
| N+1 no explicador/vigia | Descarte como regressão deste diff procede: o patch fornecido acrescenta prefetch no app; os outros caminhos continuam lendo `estado` em laço. Comparação independente com ancestral: **[nao verificado]**. (`app/chamados/services/pergunta_no_app.py:58`, `app/chamados/services/explicador.py:95`, `app/chamados/services/explicador.py:171`, `app/chamados/services/vigia_fio.py:259`) |
| Dono descritivo | Descarte como mudança da tarefa oficial procede. O explicador continua atribuindo PENDENTE ao colaborador e declara sua lista descritiva; a tarefa oficial exclui a compensadora. (`app/chamados/services/explicador.py:96`, `app/chamados/services/explicador.py:174`, `app/chamados/services/tarefa_do_dia.py:426`) |
| C11 esvazia R | É limitação declarada, não defeito independente. Outros testes exigem efeito e quantidade de compensadoras; isso não amplia a cobertura concorrente deles. (`app/chamados/tests/test_e4_expand.py:316`, `app/chamados/tests/test_e4_expand.py:324`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:453`, `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:184`) |

5. **Mapa final de escrita e leitura**

| Campo/valor | Quem escreve | Quem lê |
|---|---|---|
| `resposta_mantem_validacao=False` | Resposta humana; default do modelo. (`app/chamados/services/disputa_emissao.py:1525`, `app/chamados/models.py:4179`) | Regra Python, por `leitura`, e face SQL da validação. (`app/chamados/models.py:4275`, `app/chamados/models.py:4293`, `app/chamados/models.py:3541`) |
| `resposta_mantem_validacao=True` | Automática, trilha e fábrica da projeção; migration 0055 preenche o histórico de RESPOSTA com motivo não vazio. (`app/chamados/services/disputa_emissao.py:1679`, `app/chamados/services/disputa_emissao.py:1701`, `app/chamados/models.py:4245`, `app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:12`) | Mesmos leitores; constraint permite `True` somente em RESPOSTA. (`app/chamados/models.py:4275`, `app/chamados/models.py:3541`, `app/chamados/models.py:4200`) |
| `da_ressuscitacao=True` | Montador da ressuscitação; persistência por `registrar`. (`app/chamados/models.py:1689`, `app/chamados/models.py:4322`) | `es_da_ressuscitacao`, consumido pela carga da tarefa; constraint prende a marca à REABERTURA. (`app/chamados/models.py:4208`, `app/chamados/services/tarefa_do_dia.py:426`, `app/chamados/models.py:4197`) |
| `da_ressuscitacao=False` | Default e histórico criado pela 0054. (`app/chamados/models.py:4172`, `app/chamados/migrations/0054_decisao_da_ressuscitacao.py:15`) | Mesmo predicado; REABERTURA comum permanece na sequência da tarefa. (`app/chamados/models.py:4208`, `app/chamados/services/tarefa_do_dia.py:426`, `app/chamados/services/tarefa_do_dia.py:163`) |
| `materializada_em`, na porta | Ressuscitação manda limpar; lote grava `None` junto com autor/via. Escritas individuais passam pelo carimbo/descarimbo. (`app/chamados/models.py:1692`, `app/chamados/models.py:3469`, `app/chamados/models.py:3315`, `app/chamados/models.py:3368`, `app/chamados/models.py:3394`) | Seleção inicial e revalidação exigem não nula; o lote exclui vias fora do fato. (`app/chamados/models.py:1543`, `app/chamados/models.py:1642`, `app/chamados/models.py:3467`) |

Os backfills encontrados reutilizam a fábrica comum de RESPOSTA; não acrescentam montador independente. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:67`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:324`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:141`)

A compensadora escreve os seguintes valores e fotos:

| Conteúdo | Origem e leitura |
|---|---|
| `pergunta_id`, `ato=REABERTURA`, `ato_em=now()`, `ato_por=None` | Montados na ressuscitação; persistidos pela porta única. Leitura Python usa instante/PK e ato; SQL usa o mesmo conjunto de limpeza da validação. (`app/chamados/models.py:1673`, `app/chamados/models.py:1685`, `app/chamados/models.py:4322`, `app/chamados/models.py:4297`, `app/chamados/models.py:4305`, `app/chamados/models.py:3541`) |
| `ato_texto`, `respondida_original_em`, `validada_original_em` | Vêm da leitura vigente anterior à limpeza. São fotos históricas; não restauram o par na leitura corrente. (`app/chamados/models.py:1678`, `app/chamados/models.py:1686`, `app/chamados/models.py:1687`, `app/chamados/models.py:4307`) |
| `validada_original_por_id` | Última VALIDACAO do instante vigente, segundo a ordem da relação; RED_7 exige o segundo autor no empate. (`app/chamados/models.py:4184`, `app/chamados/models.py:1681`, `app/chamados/models.py:1688`, `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:232`) |
| `ato_motivo` | Texto com chamado, truncado a 240; modal exibe como motivo. Não decide conservação da validação nem procedência da ressuscitação. (`app/chamados/models.py:1689`, `app/templates/chamados/partials/modal_fio.html:212`, `app/chamados/models.py:4275`, `app/chamados/models.py:4208`) |

Os leitores relevantes ficam assim:

| Leitor | Tratamento |
|---|---|
| Leitura corrente/SQL | R limpa resposta e validação, sem distinguir a marca. `estado`, `respondida` e `resposta_display` consomem a leitura. (`app/chamados/models.py:4305`, `app/chamados/models.py:4307`, `app/chamados/models.py:3541`, `app/chamados/models.py:3225`, `app/chamados/models.py:3776`, `app/chamados/models.py:3800`) |
| Tarefa oficial | Exclui a marcada antes de formar a sequência. (`app/chamados/services/tarefa_do_dia.py:426`) |
| Histórico/modal | Inclui R por `ATOS_COM_FOTO`, inclusive foto vazia; exibe texto, instante original da resposta, autor original da validação, ato e motivo. (`app/chamados/models.py:4149`, `app/chamados/models.py:3791`, `app/templates/chamados/partials/modal_fio.html:212`) |
| CSF | Consome `sem_veredito_entre`, que consulta a resposta vigente pelo juiz; não conta compensadoras. (`app/ponto/supra_juiz.py:573`, `app/ponto/supra_juiz.py:574`, `app/chamados/models.py:1994`) |
| Foto E4b | Enumera todos os campos concretos e todas as decisões: incorpora booleanos e fotos. (`app/chamados/services/foto_e4b.py:251`, `app/chamados/services/foto_e4b.py:280`) |
| Auditor I18/I20 e cura de limbo | Leem a projeção; não fiscalizam quantidade de R marcada. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1077`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1106`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:46`) |
| Materialização BUG-PAR | O reconhecimento textual exige DEVOLUCAO; não interpreta esta REABERTURA. (`app/chamados/services/materializacao.py:275`) |

6. **Classes anteriores de reprovação**

| Classe | Resultado nesta revisão |
|---|---|
| Segundo juiz | Não identificado como novo: tarefa conserva sua autoridade registrada; o predicado da marca informa procedência. (`app/core/juizes.py:304`, `app/chamados/models.py:4208`) |
| Segundo escritor fora da porta | Não identificado para os campos novos/RESPOSTA; persistência permanece em `registrar`. A abrangência estática tem os limites descritos no item 3. (`app/chamados/models.py:4322`, `app/chamados/tests/test_e1_entradas.py:196`, `app/chamados/tests/test_f2_resposta_mantem_validacao.py:42`) |
| Relógio fora da borda protegida | **Presente:** resposta captura instante antes da trava e pode entrar temporalmente antes da R, embora seja persistida depois. (`app/chamados/services/disputa_emissao.py:1476`, `app/chamados/services/disputa_emissao.py:1520`, `app/chamados/models.py:4297`) |
| Derivação paralela de fonte única | Python/SQL usam o mesmo booleano e conjunto de limpeza, com contraprova de motivo discordante. Há, porém, divergência entre universo dos IDs/disputas e universo efetivamente gravável. (`app/chamados/models.py:3541`, `app/chamados/models.py:4275`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:136`, `app/chamados/models.py:1642`, `app/chamados/models.py:1676`) |
| Selo/RED sem mordida | Não são vazios; a cobertura continua parcial. Golden obrigatório e contraprovas substantivas não cobrem as duas interleavings apontadas. (`app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:155`, `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:207`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:136`) |
| Ordem de trava | A aquisição mostrada conserva C → D → Ch → P; isso não corrige a seleção anterior nem o instante capturado antes dela. (`app/chamados/models.py:2843`, `app/chamados/models.py:2852`, `app/chamados/models.py:2865`, `app/chamados/models.py:2869`) |
| Pouso antes do pré-requisito | Roteiro oficial coloca selo, migration e depois HUP. Execução efetiva: **[nao verificado]**. (`bin/deploy.sh:306`, `bin/deploy.sh:317`, `bin/deploy.sh:360`) |
| Campo com dois significados | A sobrecarga do motivo para RESPOSTA foi retirada: booleano decide conservação; marca decide procedência. Permanece reconhecimento textual específico de DEVOLUCAO BUG-PAR. (`app/chamados/models.py:4275`, `app/chamados/models.py:4208`, `app/chamados/services/materializacao.py:275`) |

Execução da suíte Django, ensaio concorrente PostgreSQL e ocorrência em produção além do dado raspado: **[nao verificado]**. [nao verificado]

## 17/11 12:14 · sessao 02f9dcc79012 · - · - · origem=Ronald (tmux)

O segundo bloqueador é viável por construção e não depende de atos futuros previamente gravados. A porta pública aceita pergunta materializada que o C6 considera viva; uma célula acusadora com o marco apagado satisfaz isso. (`app/chamados/views.py:1239`, `app/chamados/views.py:1257`, `app/api/views.py:2613`, `app/chamados/juizes.py:1450`, `app/chamados/juizes.py:1456`)

Interleaving:

1. B prepara resposta `07:13`: captura T1 e a foto antes da trava; pode pausar antes de `trava_do_colaborador`. (`app/chamados/services/disputa_emissao.py:1476`, `app/chamados/services/disputa_emissao.py:1480`, `app/chamados/services/disputa_emissao.py:1520`)
2. A ressuscita essa pergunta ainda materializada e sem resposta: trava, grava R com T2 > T1, limpa e conclui. (`app/chamados/models.py:1542`, `app/chamados/models.py:1545`, `app/chamados/models.py:1634`, `app/chamados/models.py:1673`, `app/chamados/models.py:1685`, `app/chamados/models.py:1692`)
3. B passa pela trava e salva a resposta na projeção e na linha RESPOSTA com T1. Para via sem lastro, também descarimba pela porta existente. (`app/chamados/services/disputa_emissao.py:1499`, `app/chamados/services/disputa_emissao.py:1522`, `app/chamados/services/disputa_emissao.py:1525`, `app/chamados/models.py:3516`, `app/chamados/models.py:3518`)
4. A leitura ordena RESPOSTA/T1 antes de R/T2 e retorna resposta vazia. A projeção com T1 não vira linha virtual posterior, porque já existe RESPOSTA/T1 correspondente. Resultado: B confirmou uma resposta depois de A concluir, mas a resposta não fica vigente no leitor do ato. (`app/chamados/models.py:4292`, `app/chamados/models.py:4297`, `app/chamados/models.py:4301`, `app/chamados/models.py:4307`, `app/chamados/models.py:4243`)

A cauda do core não corrige esse resultado: para chamado sem silêncio e resposta HH:MM, não há levantamento de arquivo nem materialização de escape; depois há apenas nota e retorno de sucesso. (`app/chamados/services/materializacao.py:1197`, `app/chamados/services/disputa_emissao.py:1553`, `app/chamados/services/disputa_emissao.py:1558`, `app/chamados/services/disputa_emissao.py:1574`)

A classe “relógio fora da borda” está presente: a resposta captura o instante antes de adquirir a trava que determina a ordem efetiva das escritas. (`app/chamados/services/disputa_emissao.py:1476`, `app/chamados/services/disputa_emissao.py:1520`)

Para o achado anterior, duas disputas podem conter perguntas com o mesmo `chamado_destino`: as FKs de pergunta→disputa, pergunta→destino e disputa→chamado são independentes, e o modelo admite explicitamente perguntas resolvedoras de outra disputa. (`app/chamados/models.py:2593`, `app/chamados/models.py:3630`, `app/chamados/models.py:1876`, `app/chamados/models.py:718`)

Não encontrei escritor concreto de produção que migre `p.disputa`; não sustentaria a variante “nova disputa ausente da lista” apenas pela possibilidade do esquema. `[nao verificado]` A troca de destino tem escritor concreto e pode reduzir `ids` sem reduzir `disputas`: `reapontar_resolvedora` grava o novo destino, enquanto a lista de disputas já foi materializada antes da trava. (`app/chamados/services/fio.py:973`, `app/chamados/services/fio.py:975`, `app/chamados/models.py:1623`, `app/chamados/models.py:1642`)

## 17/11 12:12 · sessao 7b7ff8ad7c95 · - · - · origem=Ronald (tmux)

Não encontrei escritor concreto de produção de RESPOSTA omitindo `resposta_mantem_validacao`: os quatro montadores declaram `False` na resposta humana e `True` na automática, no carimbo pela trilha e na foto da projeção. Todos persistem pela porta `registrar`, cujo `bulk_create` constrói `cls(**ln)`. (`app/chamados/services/disputa_emissao.py:1525`, `app/chamados/services/disputa_emissao.py:1679`, `app/chamados/services/disputa_emissao.py:1701`, `app/chamados/models.py:4245`, `app/chamados/models.py:4322`)

Executei o varredor AST em memória, sem Django e sem escrita: o censo retornou exatamente esses quatro sítios. As únicas chamadas com `ato=RESPOSTA` omitidas por `LEITURAS` foram os dois `Q` da face SQL e da constraint; não há escritor concreto escondido nessa lista. (`app/chamados/tests/test_f2_resposta_mantem_validacao.py:34`, `app/chamados/models.py:3541`, `app/chamados/models.py:4200`)

O montador indireto encontrado, `reabrir_para_correcao`, recebe `ato` variável, porém recusa valores fora de `ATOS_COM_FOTO`, conjunto formado exclusivamente por REABERTURA e RECUSA; não é escritor adicional de RESPOSTA. (`app/chamados/services/disputa_emissao.py:1620`, `app/chamados/services/disputa_emissao.py:1633`, `app/chamados/models.py:4149`)

O tripwire ampliado cobre chamadas com keyword `ato` reconhecível e dicionários literais com chave constante; a prova sintética inclui construtor, `objects.create`, literal e exclusão de consultas. Não acompanha fluxo de dados, atribuição posterior `d.ato=...`, `ln['ato']=...`, `setattr`, valor de ato em variável ou kwargs indiretos. A exclusão por nome simples também aceitaria qualquer função chamada `get/filter/exclude/Q`, independentemente de sua identidade; escritor concreto nessas formas: `[nao verificado]`. (`app/chamados/tests/test_f2_resposta_mantem_validacao.py:29`, `app/chamados/tests/test_f2_resposta_mantem_validacao.py:42`, `app/chamados/tests/test_f2_resposta_mantem_validacao.py:79`)

A porta continua aceitando omissão e aplicando o default `False`; portanto o selo é fiscalização estática, não validação dinâmica de obrigatoriedade do campo. (`app/chamados/models.py:4179`, `app/chamados/models.py:4322`)

Mapa de escritores/leitores dos dois campos novos:

| Campo | Escritores | Leitores |
|---|---|---|
| `resposta_mantem_validacao` | Quatro montadores acima; default `False`; 0055 escreve `True` em RESPOSTA com motivo diferente de vazio. (`app/chamados/models.py:4179`, `app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:12`) | Regra Python `_resposta_desfaz_validacao`, chamada por `leitura`; face SQL `q_validada_pelo_ato`; constraint restringe `True` a RESPOSTA. (`app/chamados/models.py:4275`, `app/chamados/models.py:4293`, `app/chamados/models.py:3541`, `app/chamados/models.py:4200`) |
| `da_ressuscitacao` | Um montador de `True`, dentro de `ressuscitar_perguntas`; default/histórico `False`, criado pela 0054. (`app/chamados/models.py:1689`, `app/chamados/models.py:4172`, `app/chamados/migrations/0054_decisao_da_ressuscitacao.py:15`) | Predicado único `es_da_ressuscitacao`, usado pela carga da tarefa para excluir a compensadora antes de escolher o último ato; constraint restringe `True` a REABERTURA. (`app/chamados/models.py:4208`, `app/chamados/services/tarefa_do_dia.py:426`, `app/chamados/models.py:4197`) |

A foto E4b lê ambos os campos genericamente ao enumerar todos os campos concretos de cada decisão. (`app/chamados/services/foto_e4b.py:251`, `app/chamados/services/foto_e4b.py:280`)

A leitura normal continua tratando toda REABERTURA como limpeza de resposta e validação; o histórico continua incluindo a compensadora por ela pertencer a `ATOS_COM_FOTO`, sem consultar a marca. A tarefa oficial é o leitor que faz a distinção nominal pela marca. (`app/chamados/models.py:4260`, `app/chamados/models.py:4305`, `app/chamados/models.py:4307`, `app/chamados/models.py:3791`, `app/chamados/services/tarefa_do_dia.py:426`)

Os backfills não acrescentam montador independente de RESPOSTA: `backfill_fio_morto_e4b` registra as linhas da fábrica comum; `backfill_decisoes_pergunta` usa a fábrica para o inventário e sua escrita própria monta SILENCIO. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:141`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:67`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:324`)

O script de máscara possui escrita genérica sobre `DecisaoPergunta`, mas está explicitamente limitado ao banco chamado `sombra` e só mascara `ato_texto`/`ato_motivo`; não é escritor adicional de RESPOSTA de produção nem dos campos novos. (`bin/sombra_mascara_pessoal.py:10`, `bin/sombra_mascara_pessoal.py:51`, `bin/sombra_mascara_pessoal.py:62`, `bin/sombra_regras_pessoais.py:142`)

## 17/11 12:12 · sessao 21b79216f57e · - · - · origem=Ronald (tmux)

Triagem v2:

| Item | Conferência |
|---|---|
| Idempotência concorrente | Revalidação agora existe após trava, mas demais interleavings ficam com agente concorrência. (`app/chamados/models.py:1634`, `app/chamados/models.py:1642`) |
| Futuro | **Descarte limitado ao retrato raspado.** As contagens citadas em `triagem_v2.txt` só comprovam o estoque informado; conferência independente da sonda: **[nao verificado]**. A possibilidade temporal continua: compensadora recebe `now()`, enquanto a leitura ordena atos futuros sem corte; uma VALIDACAO/RESPOSTA futura pode ficar depois da compensadora. (`app/chamados/models.py:1673`, `app/chamados/models.py:1685`, `app/chamados/models.py:4291`, `app/chamados/models.py:4297`, `app/chamados/models.py:4300`) |
| NULL no motivo | **Descarte procede no contrato do esquema.** Coluna não permite NULL; não se pode estender para estoque ilegal não mostrado. (`app/chamados/migrations/0051_decisao_pergunta.py:48`, `app/chamados/models.py:4156`) |
| N+1 explicador/vigia | Caminhos atuais continuam lendo `estado` em laço, sem prefetch próprio no carregamento do explicador; o diff fornecido altera prefetch do app, não esses corpos. Descarte como regressão deste delta procede; confronto independente com ancestral: **[nao verificado]**. (`app/chamados/services/explicador.py:95`, `app/chamados/services/explicador.py:160`, `app/chamados/services/explicador.py:171`, `app/chamados/services/vigia_fio.py:259`, `app/chamados/services/pergunta_no_app.py:58`) |
| Dono descritivo | **Descarte como alteração da tarefa oficial procede.** Explicador transforma PENDENTE em dono colaborador e declara lista descritiva; tarefa oficial retira a compensadora. (`app/chamados/services/explicador.py:96`, `app/chamados/services/explicador.py:174`, `app/chamados/services/tarefa_do_dia.py:426`) |
| C11 esvazia R | **Limitação de prova, não defeito independente.** Teste declara escopo e retorna `[]` em `order_by`; outros testes exigem efeito real da compensadora, sua quantidade e conservação da tarefa. Execução: **[nao verificado]**. (`app/chamados/tests/test_e4_expand.py:316`, `app/chamados/tests/test_e4_expand.py:324`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:453`, `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:159`, `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:190`, `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:211`) |

Mapa dos leitores e valores da REABERTURA compensadora:

- Escritor monta `pergunta_id`, REABERTURA, `ato_em=now()`, `ato_por=None`, texto/instantes vigentes e último autor da validação por ordem da relação; motivo textual truncado e marca `True`. (`app/chamados/models.py:1673`, `app/chamados/models.py:1678`, `app/chamados/models.py:1681`, `app/chamados/models.py:1685`, `app/chamados/models.py:1689`)
- Persistência é pela porta `registrar`, que instancia a linha e faz `bulk_create`; não há unicidade de R na lista de constraints. (`app/chamados/models.py:4185`, `app/chamados/models.py:4312`, `app/chamados/models.py:4322`)
- Leitura Python trata qualquer REABERTURA como limpeza de resposta e validação, sem distinguir procedência; SQL inclui o mesmo conjunto `_LIMPAM_VALIDACAO`. (`app/chamados/models.py:4260`, `app/chamados/models.py:4261`, `app/chamados/models.py:4305`, `app/chamados/models.py:4307`, `app/chamados/models.py:3541`)
- Consumidores da leitura incluem `estado`, `respondida`, `resposta_display` e `sem_veredito_entre`; nesta última, materialização continua sendo coluna e resposta passa pelo juiz. (`app/chamados/models.py:3225`, `app/chamados/models.py:3776`, `app/chamados/models.py:3800`, `app/chamados/models.py:1994`, `app/chamados/models.py:1995`, `app/chamados/models.py:1996`)
- CSF consulta `sem_veredito_entre` para pai e perguntas resolvedoras. (`app/ponto/supra_juiz.py:573`, `app/ponto/supra_juiz.py:574`)
- Tarefa do dia usa predicado único de procedência e retira linha marcada antes da sequência; REABERTURA comum continua devolvendo ao colaborador. (`app/chamados/models.py:4208`, `app/chamados/services/tarefa_do_dia.py:426`, `app/chamados/services/tarefa_do_dia.py:428`, `app/chamados/services/tarefa_do_dia.py:163`)
- Histórico inclui toda R por `ATOS_COM_FOTO`, inclusive foto vazia; modal mostra texto ou “sem texto”, instante, autor original se houver, tipo e motivo. (`app/chamados/models.py:4149`, `app/chamados/models.py:3791`, `app/chamados/models.py:3792`, `app/templates/chamados/partials/modal_fio.html:212`)
- Foto auditável lê todos os campos concretos e todas as decisões, portanto incorpora marca, novo booleano e compensadora. (`app/chamados/services/foto_e4b.py:251`, `app/chamados/services/foto_e4b.py:280`)
- Auditor I18/I20 consulta projeção, não contagem de R; não fiscaliza duplicação da compensadora. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1077`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1106`)
- Cura de limbo seleciona meio-par projetado e conjunto nominal; não lê R. Usa porta de reabertura comum para a limpeza. (`app/chamados/management/commands/curar_perguntas_em_limbo.py:46`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:48`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:69`)
- Reconhecimento textual BUG-PAR permanece, mas exige DEVOLUCAO, portanto não interpreta a compensadora; ausência rejeitada também chama reabertura comum. (`app/chamados/services/materializacao.py:275`, `app/chamados/services/materializacao.py:1167`)

`materializada_em` na porta:

- Universo exige não nula antes da trava e volta a exigir após ela. (`app/chamados/models.py:1543`, `app/chamados/models.py:1642`)
- Escrita em memória centralizada `_escrever_carimbo`; `carimbar` e `carimbar_identidade` persistem o conjunto de três campos; `descarimbar` limpa o mesmo conjunto. (`app/chamados/models.py:3283`, `app/chamados/models.py:3285`, `app/chamados/models.py:3315`, `app/chamados/models.py:3338`, `app/chamados/models.py:3371`, `app/chamados/models.py:3376`, `app/chamados/models.py:3394`)
- Ressuscitação limpa materialização, autor e via pelo lote, com validação e par de resposta vazios; lote exclui vias fora do fato no UPDATE. (`app/chamados/models.py:1692`, `app/chamados/models.py:3467`, `app/chamados/models.py:3469`)

Classes já reprovadas:

- **Segundo juiz:** nenhum novo identificável nos trechos analisados. Tarefa permanece autoridade registrada; predicado de marca só informa procedência, leitura/SQL correspondem ao mesmo efeito. (`app/core/juizes.py:304`, `app/core/juizes.py:306`, `app/chamados/models.py:4208`, `app/chamados/models.py:3541`)
- **Derivação paralela de fonte única:** SQL usa conjunto de limpeza do mesmo modelo e mesmo booleano da leitura; a equivalência é cobrada com motivo/campo discordantes nos dois sentidos. (`app/chamados/models.py:3541`, `app/chamados/models.py:4275`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:136`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:145`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:152`)
- **Relógio fora da borda:** relógio novo está no escritor; leitura não consulta relógio. Problema de ordem perante atos futuros permanece potencial, mas ocorrência além do retrato: **[nao verificado]**. (`app/chamados/models.py:1673`, `app/chamados/models.py:4291`, `app/chamados/models.py:4300`)
- **Pouso antes do pré-requisito:** roteiro oficial executa selo, migration, depois HUP; execução efetiva **[nao verificado]**. (`bin/deploy.sh:306`, `bin/deploy.sh:317`, `bin/deploy.sh:360`)
- **Campo com dois significados:** dupla função de `ato_motivo` para RESPOSTA foi removida; booleano controla validação, marca controla procedência. Há leitor textual restrito a DEVOLUCAO BUG-PAR, fora dessa cura. (`app/chamados/models.py:4275`, `app/chamados/models.py:4208`, `app/chamados/services/materializacao.py:275`)
- **Selo sem mordida:** testes de campo têm contraprovas substantivas; golden é obrigatório, tarefas e verbos não vazios, CSF exige P4. Execução **[nao verificado]**. (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:136`, `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:150`, `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:153`, `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:155`)

## 17/11 12:12 · sessao 02f9dcc79012 · - · - · origem=Ronald (tmux)

A revalidação fecha a corrida original: depois da primeira chamada limpar `materializada_em`, a segunda descarta aquele PK antes de fotografar/gravar R; ambas passam pela trava de Colaborador e pela trava das perguntas, dentro do mesmo `atomic`. (`app/chamados/models.py:1629`, `app/chamados/models.py:1634`, `app/chamados/models.py:1642`, `app/chamados/models.py:2843`, `app/chamados/models.py:2869`)

Não identifiquei outra interleaving que produza **R marcada duplicada ou R marcada sem o UPDATE correspondente** enquanto essas travas estão mantidas: o compensador e o UPDATE excluem `VIAS_FORA_DO_FATO`, e o queryset restante tem PKs fixados sob a trava das perguntas. Uma nova materialização entre duas chamadas representa outro estado retirado, não prova de R duplicada pelo mesmo estado. (`app/chamados/models.py:1642`, `app/chamados/models.py:1643`, `app/chamados/models.py:1676`, `app/chamados/models.py:1690`, `app/chamados/models.py:1692`, `app/chamados/models.py:3467`) Ocorrência/ensaio concorrente real: `[nao verificado]`.

Entretanto, **ids da trilha e disputas reabertas continuam incorretos em uma interleaving concreta**:

1. A seleciona `p1/D1` e `p2/D2` e guarda ambas as disputas antes da trava. (`app/chamados/models.py:1615`, `app/chamados/models.py:1622`, `app/chamados/models.py:1623`)
2. B executa `D1.remover_motivo`: retrata `p1`, conserva seu carimbo existente por `manter_carimbo=True` e fecha D1 quando não sobram motivos. (`app/chamados/models.py:2214`, `app/chamados/models.py:2215`, `app/chamados/models.py:2225`, `app/chamados/models.py:3368`, `app/chamados/models.py:3372`)
3. A adquire as travas: `p1` ainda tem materialização, portanto permanece em `ids`; a revalidação não exclui a via retratada. O compensador e o UPDATE excluem `p1`, enquanto `p2` é ressuscitada e torna `n > 0`. (`app/chamados/models.py:1642`, `app/chamados/models.py:1676`, `app/chamados/models.py:1692`, `app/chamados/models.py:3467`)
4. A registra ambas como ressuscitadas e reabre D1 também, embora não tenha ressuscitado sua pergunta. A reabertura apaga os campos do fechamento e deixa ocorrência no chamado. (`app/chamados/models.py:1700`, `app/chamados/models.py:1701`, `app/chamados/models.py:1702`, `app/chamados/models.py:1719`, `app/chamados/models.py:1720`, `app/chamados/models.py:2326`, `app/chamados/models.py:2333`)

Além disso, a fotografia inicial não exclui a pergunta que saiu do fato: pode materializar linhas históricas nessa pergunta, embora R e limpeza a excluam depois. (`app/chamados/models.py:1644`, `app/chamados/models.py:1645`, `app/chamados/models.py:1676`, `app/chamados/models.py:4243`, `app/chamados/models.py:4248`)

O RED_8 morde a remoção do filtro de materialização: injeta um universo antigo de uma pergunta e exige retorno zero e apenas uma R. É representativo da subclasse **seleção velha + materialização já retirada**, independentemente de quem retirou o carimbo; não cobre todas as mudanças possíveis da seleção velha. (`app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:192`, `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:205`, `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:208`, `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:211`)

Ele não cobre subconjunto parcialmente sobrevivente, mudança para fora do fato mantendo materialização, ids da auditoria, disputa sem pergunta efetivamente tocada, mudança da resposta/marco/C6, nem disputa real por locks em duas conexões; a execução é sequencial com `mock.patch` apenas do universo. (`app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:202`, `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:207`, `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:208`, `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:213`)

Correção de escopo sugerida: fixar sob a trava o conjunto **efetivamente gravável**, usar esse conjunto para fotos/R/limpeza/log e derivar dele as disputas que serão reabertas; preservar a aquisição antecipada de todas as travas necessárias. O conjunto atual de escrita é definido pela exclusão de fora do fato na porta do lote. (`app/chamados/models.py:1634`, `app/chamados/models.py:1676`, `app/chamados/models.py:3467`)

## 17/11 12:03 · sessao 7594d387160a · RESPOSTAS-1150 · verificador · origem=Code

**CONFERE para executar o item 2.** As pendências restantes cabem em **(ii)**: o literal orienta o comportamento e o encaixe técnico pertence ao Code. Não identifiquei **(iii)** com duas consequências demonstradas para o cliente ou para o dinheiro. Isso ainda deixa correções concretas na implementação. (`CLAUDE.md:25–38`, `:276–281`; `app/folha/porta_export.py:1081–1090`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:121–139`.)

A análise abaixo é estática; comportamento em produção, banco, logs e execução dos testes: **[nao verificado]**. [nao verificado]

| Ponto da volta 1 | Classificação nesta árvore e leitura que fecha |
|---|---|
| **(a) Identidade do registro** | **(i).** Existem duas linhas de `MetricaSnapshot`: pedido e carimbo. Ambas distinguem **empresa × competência × dia**; competência integra `chave`, empresa integra `escopo_id`, e o dia integra `data_ref`. A constraint conserva competências diferentes no mesmo dia. (`app/folha/porta_export.py:775–799`, `:917–925`, `:1005–1011`; `app/inteligencia/models.py:103–105`.) |
| **(a) Escritor, simultaneidade e tentativa** | **(i), parcialmente:** as transições estão no serviço `porta_export`, com UPDATE condicionado ao estado; a medição usa trava por schema × empresa × competência. **(ii), restante:** a linha é reutilizada, sem identidade própria da tentativa; o UPDATE verifica estado, sem verificar posse daquela tentativa. A leitura que fecha é conservar o resultado da execução vigente e impedir conclusão/observação atrasada de encerrar sua sucessora. (`app/folha/porta_export.py:928–959`, `:981–994`, `:1017–1019`, `:1029–1040`; `CLAUDE.md:32–34`.) |
| **(a) PID gravado e conferido** | **(ii).** O registro novo não recebe PID; `disparar` envia PID à auditoria. A vida é consultada em `pg_locks`, expressamente por causa dos containers distintos. Logo, ainda não cumpre literalmente “PID gravado e conferido”; implementar essa identidade no registro é encaixe do requisito, sem nova escolha de resultado. (`app/folha/porta_export.py:887–891`, `:963–972`, `:1005–1011`, `:1029–1033`, `:1146–1149`; padrão: `app/relatorios/models.py:151–160`.) |
| **(a)/R7 — quem observa a morte, quando e sem tela** | **(i), parcialmente:** `estado_da_porta → vigiar` persiste FALHOU para EM_CURSO sem trava; PEDIDO sem trava vence após 300 segundos. **(ii), restante:** a observação demonstrada depende dessa leitura. A tela aberta, por si, não consulta o estado; o cron de medição chama `assumir`, que sobrescreve o pedido. Observador periódico autônomo: **[nao verificado]**. A leitura que fecha é observar pedidos abandonados também sem acesso humano e passar a transição pelo mesmo serviço; periodicidade e caminho são decisões técnicas. (`app/folha/porta_export.py:898–900`, `:1029–1033`, `:1043–1077`; `app/folha/views.py:83–98`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:121–128`; `CLAUDE.md:276`.) |
| **(b) Resultado empresarial e código de saída** | **(i), parcialmente:** o carimbo conserva zero/N/negativo por empresa e competência; a porta lê esses registros, sem consultar o exit code. **(ii), restante:** resultado e execução continuam separados: `pedido` guarda estado/motivo e `carimbo` guarda N/listas. A leitura que fecha é vinculá-los à tentativa vigente; “CONCLUIDO” sozinho não significa zero nem DIVERGENTE. (`app/folha/porta_export.py:775–799`, `:1037–1040`, `:1077–1091`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:128–139`.) |
| **(b) Tentativa falha × verde antigo** | **(ii).** Continua aberto: com carimbo verde existente, pedido FALHOU pode resultar em PRONTA, porque FALHOU só prevalece quando falta carimbo. A leitura que fecha é “resultado da tentativa vigente”: a falha atual impede liberação pelo verde anterior. (`app/folha/porta_export.py:1081–1090`.) |
| **(b) Dia e virada da meia-noite** | **(i):** gravação e leitura usam o dia local; carimbo de ontem não libera hoje. **(ii):** a associação entre pedido e resultado precisa continuar correta atravessando meia-noite, pois pedido e carimbo obtêm `_hoje()` separadamente. Conservar prova diária e vínculo da tentativa é encaixe dessa regra existente. (`app/folha/porta_export.py:14–23`, `:774–776`, `:977–978`, `:1004–1011`, `:1077–1079`.) |
| **(c) Textos e repetição** | **(ii).** FALHA ainda apresenta o texto técnico antigo; o comentário reserva a alteração textual ao item 2. O clique já pode repetir SEM_MEDICAO/FALHA. Aplicar os textos fornecidos é implementação direta. (`app/folha/porta_export.py:905–914`, `:1102–1113`.) |
| **(c) Lista de divergências** | **(i).** Continua em `conferir`, inclusive cartão/TXT e HE quando bloqueadora; chega à view, ao envelope e às mensagens do template. O teste continua exigindo a lista. (`app/folha/porta_export.py:819–863`; `app/folha/views.py:130–135`; `app/core/respostas.py:63–71`; `app/templates/folha/exportar.html:10`; `app/folha/tests/test_porta_do_export.py:53–63`.) |
| **(d) Motivo, recusa crítica e trilha** | **(i), parcialmente:** exceção de `medir` já persiste motivo no pedido. **(ii), restante:** `erro_cartao_x_txt` continua fora do JSON de `lavrar`, embora o texto tente lê-lo; o caminho do command grava estado e imprime, sem emitir ali a recusa crítica. A leitura que fecha é persistir cada motivo, emitir a ocorrência ruim pelo emissor existente e completar ator/antes/depois; o envelope já admite `recusas=()` para evitar reemissão. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:129–139`; `app/folha/porta_export.py:779–799`, `:1037–1040`, `:1095–1099`; `app/core/respostas.py:41–56`; `CLAUDE.md:32–34`.) |
| **R8 — falha antes da empresa que passaria** | **(ii).** Ainda aborta na primeira exceção ou cartão/TXT não medido. A leitura que fecha é registrar o resultado daquela empresa e continuar as seguintes, preservando estados individuais. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:121–139`.) |
| **R8 — divergência global `cartao_x_txt`** | **(ii).** “Outra passa” exige que sua medição satisfaça os critérios vigentes. `cartao_x_txt` continua global e integra `falhas` de cada empresa; R8 isola falhas de execução, sem autorizar retirar essa parcela ou localizar artificialmente seu universo. (`app/folha/porta_export.py:723–727`, `:753–764`; `CLAUDE.md:265–268`.) |

Quanto aos textos exatos: a frase **“O suporte Hasner ja foi avisado” não foi localizada** nesta cópia; sua apresentação atual é **[nao verificado]**. Hoje, DIVERGENTE devolve `TXT RECUSADO...` com a lista, e FALHA usa `_TEXTO_DO_ESTADO[FALHA]`. Os pontos de alteração estão, portanto, em `conferir` e nessa tabela de textos. (`app/folha/porta_export.py:815–863`, `:908–914`.)

A leitura do literal fornecido que fecha **(c)** é usar em FALHOU: “Nao foi possivel conferir os numeros agora. Tente de novo em alguns minutos. Se repetir, avise o suporte Hasner.”; e acrescentar/substituir em DIVERGENTE por “Avise o suporte Hasner.”, conservando a lista. Localização desse aval em arquivo da cópia: **[nao verificado]**. A obrigação existente de conservar a lista está documentada e testada. (`app/folha/porta_export.py:4`; `app/folha/tests/test_porta_do_export.py:53–63`.)

O contador de **(d)** também fica em **(ii)**. Hoje, o ESTADO integra somente “Resolver dia recusado hoje”, filtrado por `veredito_celula`; seu leitor soma linhas. Unidade e integração implementadas para o novo N: **[nao verificado]**. (`bin/gerar_estado.py:262–266`, `:333–335`; `app/core/recusa_critica.py:35–37`, `:104–125`.)

A leitura que fecha é **N = ocorrências de término ruim por tentativa empresarial, no dia local da ocorrência**: uma tentativa FALHOU ou DIVERGENTE conta uma vez; N divergências internas não contam N vezes; novos POSTs não multiplicam a ocorrência; repetição verde posterior não apaga que houve falha hoje. É uma decisão de encaixe coerente com “falhou ou divergiu hoje” e com o padrão de uma linha por ocorrência, a declarar explicitamente. (`app/core/recusa_critica.py:66–82`, `:104–119`; `CLAUDE.md:35–36`, `:276`.)

Contar pares atualmente ruins produziria outra métrica após recuperação. Entretanto, efeito distinto dessa escolha sobre cliente/dinheiro é **[nao verificado]**: o contador deve apenas alarmar, enquanto a liberação consulta o estado da porta. Portanto, essa diferença operacional não satisfaz o critério exigido para **(iii)**. “Hoje” deve ter referência explícita, e ausência de observação deve conservar a distinção de ausência de sinal versus zero. (`CLAUDE.md:172–173`; `app/folha/porta_export.py:1066–1091`; `app/core/recusa_critica.py:109–115`; `bin/recusa_estado.py:41–54`.)

**QUEM ESCREVE e QUEM LÊ o registro novo:**

| Campos | Quem escreve | Quem lê |
|---|---|---|
| `chave`, `escopo`, `escopo_id`, `data_ref` | `pedir` e `assumir`. (`app/folha/porta_export.py:1009–1011`, `:1029–1033`.) | `_linha_do_pedido` filtra identidade/dia. (`app/folha/porta_export.py:975–978`.) |
| `estado` | `pedir`, `assumir` e `_mudar_estado`, chamado por repetição, `encerrar` e `vigiar`. (`app/folha/porta_export.py:988–991`, `:1005`, `:1017–1019`, `:1032`, `:1040`, `:1054–1062`.) | `pedir`, `vigiar`, `estado_da_porta` e a condição SQL de `_mudar_estado`. (`app/folha/porta_export.py:990`, `:1017`, `:1050–1053`, `:1080–1084`.) |
| `pedido_em` | `pedir`; repetição usa o timestamp genérico de `_mudar_estado`. (`app/folha/porta_export.py:989`, `:1005–1006`, `:1019`.) | `vigiar`, para o prazo sem assumir. `assumir` substitui o detalhe anterior. (`app/folha/porta_export.py:1029–1033`, `:1056–1060`.) |
| `motivo` | `encerrar` e `vigiar`, por `_mudar_estado`. (`app/folha/porta_export.py:986–991`, `:1040`, `:1054–1062`.) | `_campos_do_texto`. (`app/folha/porta_export.py:1095–1099`.) |
| `mes`, `ano`, `origem`; `data_ini`, `valor=None`; demais timestamps | `pedir`, `assumir`, `_mudar_estado`; `calculado_em` também possui `auto_now`. (`app/folha/porta_export.py:989–991`, `:1005–1019`, `:1029–1033`; `app/inteligencia/models.py:96`.) | Leitores operacionais específicos desses valores do pedido: **[nao verificado]**. |
| PID, solicitante e identidade da tentativa | Não integram o pedido criado/reassumido; ator/PID são enviados à auditoria de disparo. (`app/folha/porta_export.py:1005–1011`, `:1029–1033`, `:1142–1149`.) | A observação de vida lê a trava em `pg_locks`. (`app/folha/porta_export.py:963–972`, `:1051`.) |
| Resultado `valor` e listas — **carimbo irmão** | `medir` produz; `lavrar` persiste; o command os chama. (`app/folha/porta_export.py:692–764`, `:775–799`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:130–136`.) | `estado_da_porta` lê o escalar; `conferir` lê as listas. (`app/folha/porta_export.py:1078–1090`, `:817–863`.) |

Não restou item em **(iii)** para repetir as quatro perguntas; as pendências acima têm direção de execução sob o literal e as regras existentes de encaixe técnico. (`CLAUDE.md:25–38`, `:276–281`.)

VEREDITO item 2: CONFERE -- itens em (iii): nenhum

_6562 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
