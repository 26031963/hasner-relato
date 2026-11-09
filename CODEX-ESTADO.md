# CODEX-ESTADO

> Ultima resposta processada: **09/11/2026 03:14** · publicado em **09/11/2026 03:16** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 09/11 03:14 · sessao 8538307186c7 · SELOS-CANON-I1 · verificador · origem=Code

1. **OBSERVACAO — P1..P7: a volta 2 não cura todos integralmente.**

| Item | Resultado da conferência |
|---|---|
| **P1** | **Parcial.** Wrapper passa pelo censo antes da vaga; Agent sem obra é negado. Porém `export` herdado é negado mesmo com censo válido, pois o hook exige obra escrita na invocação. `bin/codex_pergunta.sh:230`; `bin/disjuntor.py:1691`; `bin/disjuntor.py:1713`. |
| **P2** | **Descoberta literal curada:** existe glob `logs/<OBRA>/brief*.md`, unido aos citados. A costura com briefs citados em pasta diferente continua quebrada no wrapper, que entrega texto vazio ao juiz. `bin/disjuntor.py:1536`; `bin/disjuntor.py:1570`; `bin/disjuntor.py:2087`. |
| **P3** | **Curado só na forma.** Escreve pela porta existente, mas em ALARMES; o Stop procura a primeira linha útil do painel. A idempotência também não é atômica. `bin/disjuntor.py:258`; `bin/disjuntor.py:269`; `app/core/esteira_vigia.py:863`; `bin/hook_stop_fila1.py:407`. |
| **P4** | **Parcial.** BACKLOG ausente e exceções dentro de `_censo_nega` negam; JSON ilegível ainda termina com rc 0 e sem `deny`. `bin/disjuntor.py:1544`; `bin/disjuntor.py:1590`; `bin/disjuntor.py:1751`; `bin/disjuntor.py:1790`. |
| **P5** | **Contraexemplos originais curados:** examina todas as perguntas e ancora o corte. O positivo exige stdout vazio, mas não verifica rc 0 do processo. `bin/disjuntor.py:1454`; `bin/disjuntor.py:1512`; `bin/tests/test_censo_antes_da_vaga.sh:124`; `bin/tests/test_censo_antes_da_vaga.sh:139`. |
| **P6** | **Parcial.** Há união dos intervalos e adiamento do OK do Agent. O OK do wrapper continua anterior à vaga e é contado como abertura efetiva. `bin/disjuntor.py:1924`; `bin/disjuntor.py:1662`; `bin/codex_pergunta.sh:230`; `bin/disjuntor.py:1988`. |
| **P7** | **Reconhecedor compartilhado; semântica incompleta.** Stop, handoff e alarme usam a mesma função, mas selecionam lugares diferentes do RELATO. `bin/hook_stop_fila1.py:716`; `bin/handoff_sessao.sh:128`; `bin/alarme_sessao_ociosa.py:124`; `bin/alarme_sessao_ociosa.py:137`. |

2. **PROCEDENTE-candidato — classificação dos pontos abertos 1..9.**

| Ponto | Classificação e fundamento |
|---|---|
| **1** | **Defeito:** PAREI entra abaixo de ALARMES, preservando o painel anterior; não satisfaz o leitor do Stop/handoff. Ausência de hora é comprovada; permanência pela dieta é **[nao verificado]**. `app/core/esteira_vigia.py:829`; `app/core/esteira_vigia.py:867`; `bin/disjuntor.py:1457`; `bin/hook_stop_fila1.py:408`. |
| **2** | **Defeito:** existência é consultada antes do lock; a inserção travada não deduplica. Dois chamadores podem inserir duas vezes. Além disso, uma linha histórica em qualquer lugar impede republicação posterior. `bin/disjuntor.py:258`; `bin/disjuntor.py:261`; `app/core/esteira_vigia.py:848`; `app/core/esteira_vigia.py:867`. |
| **3** | **Defeito de costura:** o PreToolUse aceita brief citado em pasta diferente; o wrapper perde essa citação e exige o glob literal. Portanto uma abertura pode passar no hook e ser recusada pela CLI com o mesmo brief conforme. Que as pastas atuais de produção sejam slugs é **[nao verificado]**. `bin/tests/test_censo_antes_da_vaga.sh:166`; `bin/disjuntor.py:1536`; `bin/disjuntor.py:2087`. |
| **4** | **Defeito de autoridade:** a vaga é do repositório vivo, mas o julgamento usa BACKLOG/briefs da raiz do script. Cópia desatualizada sem a obra retorna FORA/rc 0. Divergência efetiva em produção: **[nao verificado]**. `bin/codex_pergunta.sh:55`; `bin/codex_pergunta.sh:76`; `bin/codex_pergunta.sh:230`; `bin/disjuntor.py:1544`; `bin/disjuntor.py:2083`. |
| **5** | **Defeito no contrato anunciado da chave:** “existindo, nenhum disjuntor age” não corresponde à CLI, que não consulta `desligado()`. A diferença está declarada, mas aceite específico desse desvio é **[nao verificado]**. `bin/disjuntor.py:35`; `bin/disjuntor.py:1756`; `bin/disjuntor.py:2069`. |
| **6** | **Endurecimento declarado, com regressões concretas:** negar construtor sem obra é intencional; negar `export` com censo válido e IDs anteriormente admitidos é incompatibilidade adicional. `bin/disjuntor.py:1713`; `bin/disjuntor.py:1691`; `bin/codex_pergunta.sh:188`; `bin/disjuntor.py:2075`. |
| **7** | **Defeito na cobertura alegada:** abertura fora do wrapper não é automaticamente contada; sem `ini` no chamadas.log ela desaparece da medida. A rota direta também é proibida pela conduta existente. `bin/disjuntor.py:1972`; `bin/disjuntor.py:1975`; `bin/codex_pergunta.sh:288`; `CLAUDE.md:348`. |
| **8** | **Risco de compatibilidade declarado da forma restritiva:** exige `? cortada por Ronald <token>` no fim; texto posterior invalida. Não é outro escape; é redução das formas aceitas. Aceitação de formatos reais afetados: **[nao verificado]**. `bin/disjuntor.py:1454`; `bin/disjuntor.py:1513`. |
| **9** | **Fora do aval funcional de I1; ajuste coerente do selo vizinho.** A mutação continua retirando `bash|sh`; `env` passou a ramo separado. Verde efetivo nesta auditoria: **[nao verificado]**. `bin/disjuntor.py:1288`; `bin/tests/test_disjuntor.sh:1741`. |

**Chamadores expostos ao ponto 6:** agentes construtores em cópia, inclusive a rota documental `wt-ui`; wrapper com obra herdada por `export`; wrapper alcançado por script ou `bash -c`. Os dois primeiros podem receber negativa diretamente no hook; os últimos passam pela porta acrescentada ao wrapper. `CLAUDE.md:306`; `CLAUDE.md:309`; `bin/disjuntor.py:1691`; `bin/disjuntor.py:1711`; `bin/codex_pergunta.sh:230`.

**Lista de processos/prompts individualmente vivos: [nao verificado].** Os exemplos de `export` e Agent sem obra nos anexos são fixtures, não identificação de chamadores de produção. `bin/tests/test_censo_antes_da_vaga.sh:174`; `bin/tests/test_censo_antes_da_vaga.sh:178`.

O predicado exato do Agent é:

- `tool_name == 'Agent'`;
- tipo fora de `{'verificador','Explore','Plan','claude-code-guide','statusline-setup'}`;
- prompt casando `COPIA_RE = /[^\s"'\\:]*/wt-[A-Za-z0-9._-]+`;
- obra obtida pela primeira ocorrência válida entre as âncoras `obra[s] <ID>` e `leitura … da <ID>`, esta limitada a 80 caracteres sem quebra. `bin/disjuntor.py:102`; `bin/disjuntor.py:104`; `bin/disjuntor.py:115`; `bin/disjuntor.py:545`; `bin/disjuntor.py:1711`.

O ID validado é `(?:[A-Z][A-Z0-9]*(?:-[A-Za-z0-9]+)+|[A-Z]+\d+[A-Z0-9]*)`. A busca ignora caixa, mas a validação não; cauda opcional participa da identidade dos IDs sem hífen. Reconhecimento sintático não significa pertencer à fila: isso é consultado depois. `bin/disjuntor.py:108`; `bin/disjuntor.py:110`; `bin/disjuntor.py:562`; `bin/disjuntor.py:570`; `bin/disjuntor.py:1567`.

**A sessão principal não tem isenção ao despachar Agent ou wrapper:** a saída antecipada por ausência de `agent_id` exclui expressamente essas chamadas. Que esta sessão tenha emitido uma chamada afetada é **[nao verificado]**. `bin/disjuntor.py:1673`.

3. **PROCEDENTE-candidato — fail-closed ainda tem buracos; há negativas espúrias permanentes.**

JSON truncado levanta em `json.loads`, mantendo `payload={}`. O classificador devolve lista vazia; o fallback pelo texto cru só existe quando o próprio classificador levanta. O caminho termina com rc 0, stdout vazio e “ferramenta liberada”. Esse resultado foi reproduzido em memória. `bin/disjuntor.py:1748`; `bin/disjuntor.py:1751`; `bin/disjuntor.py:1594`; `bin/disjuntor.py:1611`; `bin/disjuntor.py:1790`.

BACKLOG legível, mas sem marcador OBRAS, também vira fila vazia: `_itens_andaveis` retorna `[]`, e o censo libera como fora da fila. Ausência de estrutura não é distinguida de universo realmente vazio. `bin/hook_stop_fila1.py:225`; `bin/disjuntor.py:1546`; `bin/disjuntor.py:1567`; `bin/disjuntor.py:2083`.

Falhas de importação anteriores a `main_pretool` não estão protegidas pelo seu `try`; settings chama Python diretamente. **Que o harness real abra após traceback/rc 1 é [nao verificado]**: seu consumidor de PreToolUse não está demonstrado pela cópia. `bin/disjuntor.py:59`; `bin/disjuntor.py:1747`; `.claude/settings.json:68`.

No inverso, há duas recusas independentes da qualidade do brief:

- `export CODEX_OBRA=… CODEX_PAPEL=construtor; wrapper --construtor …` continua sem obra para o hook, mesmo com censo conforme. `bin/disjuntor.py:1385`; `bin/disjuntor.py:1691`.
- IDs como `ABC` e `09-TURNO-ABERTO-EXPOSTA` passam no contrato anterior do wrapper e falham no novo `main_censo`. Não se resolvem corrigindo o brief. Chamadas atuais desses IDs: **[nao verificado]**. `bin/codex_pergunta.sh:188`; `bin/disjuntor.py:108`; `bin/disjuntor.py:2075`.

4. **PROCEDENTE-candidato — ESMERIL-SEMANTICA do PAREI: mesma porta, lugar incompatível.**

Sim: negativa comum e PAREI carregam `esteira_vigia.py` e chamam `no_relato`. Não foi acrescentado um escritor independente nesse caminho. `bin/disjuntor.py:195`; `bin/disjuntor.py:240`; `bin/disjuntor.py:267`; `bin/disjuntor.py:269`.

A string escrita corresponde à forma aceita pelo reconhecedor; o problema é onde ela fica. `bin/disjuntor.py:1457`; `bin/hook_stop_fila1.py:700`.

| Consumidor | Lugar efetivamente lido |
|---|---|
| **Escritor** | Insere dentro de ALARMES, preservando o painel anterior. `app/core/esteira_vigia.py:829`; `app/core/esteira_vigia.py:867`. |
| **Stop** | Primeira linha útil após o cabeçalho. `bin/hook_stop_fila1.py:407`; `bin/hook_stop_fila1.py:438`; `bin/hook_stop_fila1.py:715`. |
| **Handoff** | Coleta até o primeiro `##`; prova a linha devolvida pelo Stop. `bin/handoff_sessao.sh:102`; `bin/handoff_sessao.sh:128`. |
| **Alarme** | Procura nas primeiras 40 linhas. Pode encontrar ALARMES enquanto Stop/handoff não encontram. `bin/alarme_sessao_ociosa.py:124`; `bin/alarme_sessao_ociosa.py:137`. |

A contraprova em memória usando o fixture do próprio selo colocou PAREI na linha 7, enquanto `_linha_do_topo()` devolveu `texto neutro do painel`. O selo apenas conta a ocorrência no arquivo inteiro; o teste do Stop fabrica PAREI diretamente no topo. Falta a prova integrada negativa → escritor → leitor. `bin/tests/test_censo_antes_da_vaga.sh:145`; `bin/tests/test_censo_antes_da_vaga.sh:202`; `bin/tests/test_hook_esperando_com_prova.sh:62`.

5. **OBSERVACAO — `#G:censo` atual recusa qualquer rc não zero antes da vaga.**

A expressão é `python3 -I … --censo "$OBRA" || _recusa2 …`; `_recusa2` termina com rc 2. Portanto **erro Python com traceback/rc 1 recusa**, assim como rc 2, 127 ou outro não zero. O `flock` vem depois. `bin/codex_pergunta.sh:183`; `bin/codex_pergunta.sh:230`; `bin/codex_pergunta.sh:236`.

O selo mantém a vaga ocupada e exige que o motivo seja censo, exercitando essa ordem. `bin/tests/test_codex_pergunta.sh:210`.

6. **PROCEDENTE-candidato — existem mutantes simples não distinguidos pelos casos apresentados.**

As contraprovas abaixo foram exercitadas em memória; **sobrevivência à execução integral dos selos: [nao verificado]**. A previsão de sobrevivência decorre dos casos indicados. `bin/tests/test_censo_antes_da_vaga.sh:149`; `bin/tests/test_censo_antes_da_vaga.sh:186`; `bin/tests/test_hook_esperando_com_prova.sh:233`; `bin/tests/test_codex_pergunta.sh:85`.

| Alvo / mutação | Por que os casos não distinguem; defeito introduzido |
|---|---|
| **`_censo_nega`: `+ ok` → `+ ok[-1:]`** | O caso com vários citados põe o ruim por último. Continua negando ali, mas libera **ruim seguido de bom**, ignorando o primeiro. `bin/disjuntor.py:1570`; `bin/tests/test_censo_antes_da_vaga.sh:149`. |
| **`_briefs_da_obra`: glob ordenado → glob ordenado `[-1:]`** | O cenário múltiplo põe `brief_v2.md` ruim por último. Falta a inversão: primeiro ruim, último bom. `bin/disjuntor.py:1536`; `bin/tests/test_censo_antes_da_vaga.sh:181`; `bin/tests/test_censo_antes_da_vaga.sh:186`. |
| **`prova_parei_censo`: não reconhecido retorna `''`, em vez de `None`** | Os casos novos chamam diretamente a prova apenas com linhas de censo. O handoff passaria a declarar espera provada para painel comum, pois trata `''` como sucesso. `bin/hook_stop_fila1.py:698`; `bin/handoff_sessao.sh:128`; `bin/tests/test_hook_esperando_com_prova.sh:237`. |
| **`#G:censo`: recusar somente rc 1** | O fake recusa exclusivamente com rc 1. O mutante mantém esse caso e libera rc 2/127. `bin/tests/test_codex_pergunta.sh:85`; `bin/tests/test_codex_pergunta.sh:210`; `bin/tests/test_codex_pergunta.sh:525`. |

Também falta assert numérico do contador `ok`: os asserts numéricos cobrem abertos, negados e horas. `bin/tests/test_censo_antes_da_vaga.sh:246`.

7. **PROCEDENTE-candidato — trilhas e medidas ainda atribuem significados diferentes ao mesmo evento.**

| Linha | Quem escreve | Quem lê / significado |
|---|---|---|
| **CENSO OK** | CLI por `_censo_nega`; Agent após `_pretool` permitir. `bin/disjuntor.py:1581`; `bin/disjuntor.py:1664`. | `_censo_24h` aumenta `ok`, encerra espera e apresenta “aberturas que passaram”. `bin/disjuntor.py:1961`; `bin/disjuntor.py:1963`; `bin/disjuntor.py:1988`. |
| **CENSO FORA** | `main_censo`. `bin/disjuntor.py:2084`. | Registra par obra/PID como cobertura suficiente; não aumenta `ok` nem encerra espera anterior. `bin/disjuntor.py:1955`; `bin/disjuntor.py:1960`. |
| **NEGA: linha real `NEGOU DISJUNTOR CENSO`** | PreToolUse normal, recuperação de exceção e CLI. `bin/disjuntor.py:1763`; `bin/disjuntor.py:1774`; `bin/disjuntor.py:2089`. | Conta negativas; abre intervalo somente se reconhecer `construtor de <obra> sem CENSO`. Negativas sem obra não abrem intervalo. `bin/disjuntor.py:1949`. |
| **CENSO DESLIGADO** | `_censo_desligado`, para Agent. `bin/disjuntor.py:1742`. | Soma em `abertos_sem`. `bin/disjuntor.py:1958`; `bin/disjuntor.py:1977`. |
| **PAREI censo** | `_parei_censo_no_relato` pela porta do vigia. `bin/disjuntor.py:269`. | Stop, handoff e alarme, com os recortes divergentes do achado 4. `bin/hook_stop_fila1.py:716`; `bin/handoff_sessao.sh:128`; `bin/alarme_sessao_ociosa.py:137`. |
| **`chamadas.log` / `ini`** | Wrapper, antes da chamada efetiva. `bin/codex_pergunta.sh:288`. | `codex_limite_estado._linhas_chamadas`, depois `_censo_24h`. `bin/codex_limite_estado.py:49`; `bin/disjuntor.py:1972`. |

O **OK da CLI significa brief aprovado**, pois ocorre antes da vaga. O leitor o transforma em **construtor aberto**. CLI avulsa ou recusa posterior por vaga ocupada aumenta aberturas e encerra espera sem abertura. `bin/disjuntor.py:2087`; `bin/codex_pergunta.sh:230`; `bin/codex_pergunta.sh:236`; `bin/disjuntor.py:1961`.

A correlação aceita `(obra,pid)` de toda a história, sem contemporaneidade ou hash. Um PID reutilizado pode esconder abertura sem aprovação contemporânea; a chave original do chamadas.log dispõe também de hash. Ocorrência real: **[nao verificado]**. `bin/disjuntor.py:1957`; `bin/disjuntor.py:1975`; `bin/codex_limite_estado.py:63`.

A união corrige sobreposição, mas mede intervalos NEGOU→OK, sem consultar ocupação da vaga; não prova “horas de vaga ociosa”. `bin/disjuntor.py:1924`; `bin/disjuntor.py:1953`; `bin/disjuntor.py:1968`.

8. **OBSERVACAO — classes estruturais presentes nesta fatia.**

- **Segundo juiz:** não apareceu clone do juiz de forma; há divisão da autoridade por raiz entre censo e vaga. `bin/disjuntor.py:1575`; `bin/codex_pergunta.sh:76`; `bin/codex_pergunta.sh:230`.
- **Segundo escritor fora da porta:** não neste caminho; a violação é leitura de idempotência fora da trava. `bin/disjuntor.py:261`; `bin/disjuntor.py:269`; `app/core/esteira_vigia.py:848`.
- **Borda temporal:** correlação com PID histórico sem limite de tempo, comprovada no leitor; ocorrência produtiva **[nao verificado]**. `bin/disjuntor.py:1957`; `bin/disjuntor.py:1975`.
- **Derivação paralela:** recortes independentes decidem quais linhas representam a parada atual. `bin/hook_stop_fila1.py:407`; `bin/handoff_sessao.sh:102`; `bin/alarme_sessao_ociosa.py:124`.
- **Selo sem mordida:** falta ligação escritor→Stop e faltam as inversões/rcs dos mutantes acima. `bin/tests/test_censo_antes_da_vaga.sh:202`; `bin/tests/test_hook_esperando_com_prova.sh:62`; `bin/tests/test_codex_pergunta.sh:85`.
- **Ordem de trava:** censo antes da vaga está correto; deduplicação antes do lock permanece incorreta. `bin/codex_pergunta.sh:230`; `bin/codex_pergunta.sh:236`; `bin/disjuntor.py:261`.
- **Campo com dois significados:** **presente em CENSO OK**, aprovação do brief no escritor e abertura efetiva no leitor. `bin/disjuntor.py:1581`; `bin/disjuntor.py:1988`.

Os verdes relatados pelo construtor não foram reexecutados nesta auditoria; execução integral e comportamento de produção permanecem **[nao verificado]**.

## 09/11 03:13 · sessao 4f60167f1d5f · PAR-NAO-ADJACENTE · censo · origem=Code

**1. A adjacência não tem justificativa de lei demonstrada**

O contrato registrado manda transportar **o par do juiz inteiro**, acomodar a saída na última ponta e tratar soltas pelo marco mais próximo livre. Não exige pontas consecutivas entre as batidas; essa condição aparece na implementação. (`app/docs/RELATO.md:1710`, `app/escala/utils.py:467`, `app/escala/utils.py:507`, `app/escala/utils.py:511`)

O pouso `b3bf529e`, em 29/10, está registrado. A introdução precisa da condição em `9da200c8` e a razão histórica dessa escolha ficam **[nao verificado]**: esta cópia não contém o histórico Git consultável. (`app/docs/RELATO.md:1657`)

Os casos pedidos não demonstram dependência da adjacência:

| Caso | O que protege |
|---|---|
| **col345** | A entrada às 15:00 cumpriu a volta `hfi`; o período de trabalho é 15:00→18:56. Não apresenta pausa com solta interposta. (`app/ponto/tests/test_calculador_portoes_de_marco.py:15`, `app/ponto/tests/test_calculador_portoes_de_marco.py:56`) |
| **col60/O271** | E12:00→S19:06: cabeça atrasada em 300 minutos, mesmo estando sobre `hii`. As contraprovas distinguem entrada em `hfi`. (`app/ponto/tests/test_o271_pontualidade_no_comercial.py:177`, `app/ponto/tests/test_o271_pontualidade_no_comercial.py:192`) |
| **[nome]** | Cluster de quatro batidas não pode preencher marcos distantes; a proteção é o filtro final, independente de `_iz == _i + 1`. (`app/ponto/tests/test_pareador_cluster_marco.py:53`, `app/escala/utils.py:572`) |
| **Nominais da obra** | A, controle, convergência e noturno usam quatro batidas, com pontas internas adjacentes; B/C/D/E/F exercitam bordas e soltas. (`app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:104`, `:129`, `:143`, `:154`, `:165`, `:212`, `:223`, `:326`, `:358`) |

Por inspeção, esses exemplos preservam a elegibilidade dos pares ao trocar a comparação por `>`; resultado de executar a alteração contra a bateria: **[nao verificado]**. Há precedente explícito protegendo o **par declarado contra o adjacente guloso**, embora a sobra daquele teste esteja antes do par. (`app/ponto/tests/test_o68b_papel_da_ata_no_motor.py:701`)

**O contraexemplo à troca isolada é o aninhamento descrito abaixo**, permitido pela construção atual do extrator. (`app/ponto/turnos.py:811`, `app/ponto/turnos.py:818`)

**2. Arranjos que precisam ser distinguidos**

Os leitores públicos repartem os mesmos pares marcados pelo piso: `intervalos_do_turno` entrega os de pelo menos 15 minutos; `pausas_abaixo_do_piso`, o restante. A adjacência não é garantia conjunta deles: a passagem declarada casa suas pontas primeiro. (`app/ponto/turnos.py:545`, `app/ponto/turnos.py:565`, `app/ponto/turnos.py:811`)

| Arranjo | Resultado da leitura |
|---|---|
| **Uma solta entre duas pontas declaradas** | O par declarado permanece inteiro; a batida sem outro papel é `sol`, tratada depois. Rejeitá-lo por adjacência contradiz esse contrato. (`app/ponto/turnos.py:811`, `app/escala/utils.py:440`, `app/escala/utils.py:507`, `app/escala/utils.py:511`) |
| **Pares cruzados** | Com instantes distintos e lista cronológica, os dois laços atuais não os produzem: declarados são casados sucessivamente; os demais usam vizinhos sem pontas declaradas. (`app/ponto/turnos.py:1217`, `app/ponto/turnos.py:812`, `app/ponto/turnos.py:819`, `app/ponto/turnos.py:820`) |
| **Pares aninhados** | São possíveis: a primeira passagem reserva apenas as pontas declaradas; um par adjacente inteiramente dentro delas pode sobreviver à segunda passagem. (`app/ponto/turnos.py:815`, `app/ponto/turnos.py:820`, `app/ponto/turnos.py:880`) |
| **Volta igual à saída verdadeira** | As bordas reais são excluídas dos pares por identidade. Deve permanecer o limite interior, além de `iz > i`; a última batida continua `sai`. (`app/ponto/turnos.py:802`, `app/ponto/turnos.py:811`, `app/ponto/turnos.py:880`, `app/escala/utils.py:473`) |
| **“Solta” que é ponta de outro par** | Ela tem papel de par. Não se pode pular todo o trecho até a volta e apagar essas pontas. (`app/escala/utils.py:440`, `app/escala/utils.py:507`) |
| **Duas pausas disjuntas com solta entre elas** | Transportam-se os dois pares em ordem; depois a solta tenta seu marco mais próximo, livre e compatível com a cronologia. (`app/escala/utils.py:539`, `app/escala/utils.py:564`) |

**Contraexemplo concreto, deduzido do código:** ata/DNA `08/12/13/17`; batidas `E08, S12, S12:10, E12:30, E13, S17`. As extras podem receber `X`; 12/13 recebem `Xi`; o pareador absorve as internas. O extrator pode devolver **12→13 e 12:10→12:30**. Ocorrência desse arranjo na frota: **[nao verificado]**. (`app/ponto/turnos.py:1761`, `app/ponto/turnos.py:1787`, `app/ponto/turnos.py:1804`, `app/ponto/turnos.py:1827`, `app/ponto/turnos.py:1315`, `app/ponto/turnos.py:1371`, `app/ponto/turnos.py:811`, `app/ponto/turnos.py:880`)

Com apenas uma pausa cadastral, aceitar ambos faz o alinhador retornar `None` e seguir para a DP; com duas, ele pode distribuir externo e interno em pausas sucessivas, invertendo a cronologia entre os pares. (`app/escala/utils.py:530`, `app/escala/utils.py:535`, `app/escala/utils.py:561`, `app/escala/utils.py:613`, `app/escala/utils.py:625`)

A **L-085** conserva o dia de início da jornada; a **L-087** impede decidir papel pelo tipo gravado; **[nome]** ainda pode retirar associações no filtro final. Nenhuma prioridade explícita para pausas aninhadas foi demonstrada: **[nao verificado]**. (`app/docs/LEIS.md:117`, `app/docs/LEIS.md:119`, `app/escala/utils.py:578`)

**3. Cura mínima na fonte e derivações paralelas**

Para pares **internos, disjuntos e com pontas identificáveis**, a mudança local é reconhecer:

```python
0 < _i < _iz < len(_bats) - 1
```

em lugar da exigência de vizinhança. Conservam-se as pontas e a associação S/E entregues pelo juiz; não se procura outro parceiro. Isso é desenho proposto, ainda não implementado nem testado. (`app/escala/utils.py:454`, `app/escala/utils.py:467`; execução **[nao verificado]**)

O percurso deve continuar visitando as batidas entre as pontas. As que não pertencem a nenhum par recebem `sol`; as pontas de outros pares conservam seu papel. O alinhador já acomoda bordas e pares antes de examinar soltas, que não deslocam ocupantes; o filtro [nome] continua por último. (`app/escala/utils.py:461`, `app/escala/utils.py:468`, `app/escala/utils.py:472`, `app/escala/utils.py:558`, `app/escala/utils.py:564`, `app/escala/utils.py:578`)

**Não considero segura a cura geral como uma troca isolada de operador.** É preciso conferir o conjunto completo de pares antes de transcrevê-lo. Para sobreposição, o desenho sem fallback é explicitar o conflito, sem escolher vencedor no adaptador nem acrescentar queda para a DP. Esse tratamento é proposta; sua implementação e comportamento ficam **[nao verificado]**. A necessidade decorre do aninhamento permitido e da capacidade limitada do alinhador. (`app/ponto/turnos.py:811`, `app/ponto/turnos.py:818`, `app/escala/utils.py:535`)

Há outras adjacências a distinguir:

- **`_papeis_por_alternancia`** deriva pausas dos vizinhos S/E dos períodos. É a derivação paralela de papel usada sem `turno=`, exigindo pelo menos quatro batidas alternadas e duração até 26 horas. Não consulta os pares completos do turno. (`app/escala/utils.py:477`, `app/escala/utils.py:489`, `app/escala/utils.py:496`, `app/escala/utils.py:597`)
- **`_pares_marcados`** mantém adjacência própria para os pares residuais, depois dos declarados; essa é parte da autoridade existente, não deve ser refeita no adaptador. (`app/ponto/turnos.py:811`, `app/ponto/turnos.py:818`)
- **`_pausa_fechada_carimbada`** também percorre vizinhos, mas responde à figura da L-115, inclusive onde o extrator de pausas corretamente devolve vazio. Não é substituível mecanicamente por `intervalos_do_turno`. (`app/ponto/turnos.py:912`, `app/ponto/turnos.py:915`, `app/ponto/turnos.py:924`, `app/ponto/turnos.py:1005`)
- **`_borda_posicional`** procura um único vão interior adjacente somente quando nenhum turno tem pausa marcada; é outra pergunta, com exclusão explícita da autoridade já presente. (`app/ponto/turnos.py:654`, `app/ponto/turnos.py:679`, `app/ponto/turnos.py:697`)

**Pergunta de lei 1:** quando o extrator entrega uma pausa declarada externa e outra pausa inteiramente dentro dela, qual resultado deve valer: somente a declarada, ambas com tratamento explícito da sobreposição, ou turno conflitante? O código admite a figura; a regra de desempate permanece **[nao verificado]**. (`app/ponto/turnos.py:811`, `app/ponto/turnos.py:818`)

**4. Leitores e dinheiro**

| Caminho | Leitura e consequência |
|---|---|
| **Ata** | O builder passa `turno=` ao alinhador; monta cópias das células com as novas lâmpadas/órfãs, repareia e calcula o realizado. O cartório serializa esse número. É o caminho diretamente atingido por `_papeis_do_turno`. (`app/escala/utils.py:1701`, `app/escala/utils.py:1886`, `app/escala/utils.py:1898`, `app/escala/utils.py:2045`, `app/ponto/services/cartorio.py:706`) |
| **Zona 5** | Chama `_match_marcos` sem turno, sobre os períodos; identifica P1/P2 e aplica atraso/antecipada. Usa a derivação por alternância quando elegível. (`app/ponto/motor_calculo_v2.py:3060`, `app/ponto/motor_calculo_v2.py:3062`, `app/ponto/motor_calculo_v2.py:3077`, `app/escala/utils.py:597`) |
| **`marco_ocupado_pela_batida`** | Chama o mesmo alinhador sem turno e transcreve rótulo/delta; identifica a batida por HH:MM. (`app/escala/utils.py:427`, `app/escala/utils.py:429`) |
| **`regua_defesa.cumpriu_a_volta_do_intervalo`** | Consulta esse leitor com um período e exige `hfi` dentro da tolerância. Com duas batidas, permanece na DP; motor e calculador usam a resposta. (`app/escala/regua_defesa.py:489`, `app/escala/regua_defesa.py:493`, `app/escala/utils.py:489`, `app/ponto/motor_calculo_v2.py:2117`, `app/ponto/calculador/regras.py:251`) |
| **Outros chamadores diretos** | Builder single, passagem de diagnóstico e detector de anomalias desabilitam a derivação. (`app/escala/utils.py:776`, `app/escala/utils.py:1650`, `app/chamados/detector_anomalias.py:212`) |

Assim, mudar `_papeis_do_turno` não muda diretamente todos os chamadores. O efeito financeiro também pode chegar **pela ata**, cujo papel alimenta o pareador do motor e os intervalos transportados pelo adaptador. (`app/escala/utils.py:596`, `app/ponto/motor_calculo_v2.py:495`, `app/ponto/motor_calculo_v2.py:503`, `app/ponto/adaptador_motor.py:44`)

Os números envolvidos têm contas distintas:

- **Realizado da ata:** minutos HH:MM entre bordas, menos os intervalos reconhecidos. (`app/ponto/turnos.py:740`, `app/ponto/turnos.py:756`, `app/ponto/turnos.py:758`, `app/ponto/turnos.py:767`)
- **Pagamento e intrajornada:** no 12x36, o motor recorta a janela, desconta a pausa real e calcula supressão/tolerância; `DiaPago` recebe trabalhadas e intra indenizada convertidas em horas. (`app/ponto/motor_calculo_v2.py:2759`, `app/ponto/motor_calculo_v2.py:2764`, `app/ponto/motor_calculo_v2.py:2771`, `app/ponto/motor_calculo_v2.py:2784`, `app/ponto/services/dia_pago.py:178`, `app/ponto/services/dia_pago.py:186`)
- **Leitores derivados:** leitor da célula, PDF, causas, classificação, calendário e comparação ata/juiz consomem o realizado; fechamento e percentual ainda aplicam teto diário pelo previsto. (`app/escala/services/leitor_celula.py:406`, `app/folha/validacao_pdf.py:54`, `app/folha/causas.py:85`, `app/ponto/supra_juiz.py:224`, `app/colaboradores/services/calendario.py:359`, `app/ponto/services/bordas_realizado.py:87`, `app/ponto/services/dia_pago.py:394`, `app/ponto/services/fechamento.py:1064`, `app/folha/export.py:1096`)

**No dado raspado fornecido**, gravado→rejulgado:

| Campo | Antes → depois |
|---|---|
| Ata `minutos_realizados` | **660 → 599 min**, diferença −61. **[nao verificado]** |
| Motor `intra_descontada` | **58,0130167 → 119,6599667 min**, diferença +61,64695. **[nao verificado]** |
| Intrajornada indenizada | **0 → 0**. **[nao verificado]** |
| Pagamento lavrado exibido | **10,87 h → 10,87 h**, rubricas exibidas iguais. **[nao verificado]** |

Esses rótulos indicam ausência de arquivo:linha local para o anexo. O segundo `minutos_trabalhados` foi raspado; novo pagamento lavrado, impacto em reais e impacto financeiro da frota permanecem **[nao verificado]**. O espelho distingue realizado calculado de pagamento lavrado. (`app/ponto/services/espelho.py:783`, `app/ponto/services/espelho.py:799`, `app/ponto/services/espelho.py:811`)

**5. Sonda de impacto — somente texto**

A sonda usa `CargaDaJanela`, que carrega `batidas_apuraveis` e chama o mesmo corpo de `turnos_do_colab` usado pela ata. Conta turnos fechados com célula existente, sem julgar ou lavrar. (`app/ponto/turnos.py:3009`, `app/ponto/turnos.py:3022`, `app/escala/utils.py:1581`)

O agrupamento usa **21..20**, conforme solicitado, pela função existente; empresa significa o FK atual do colaborador, sem reconstrução histórica. (`app/ponto/janelas.py:24`, `app/colaboradores/models.py:321`) O banco sombra mantém o schema `juliani`. (`app/config/settings/sombra.py:13`, `app/config/settings/sombra.py:29`)

Texto para shell Django inicializado; execução e resultados: **[nao verificado]**.

```python
import json
from collections import defaultdict
from datetime import date

from django.db import connection, transaction
from django.utils import timezone
from django_tenants.utils import schema_context

from colaboradores.models import Colaborador
from ponto.janelas import periodo_apuracao_atual
from ponto.turnos import (
    CargaDaJanela,
    intervalos_do_turno,
    pausas_abaixo_do_piso,
)

NAME = connection.settings_dict["NAME"]
assert NAME == "sombra"

class _Rollback(Exception):
    pass

saida = None

try:
    with schema_context("juliani"):
        with transaction.atomic():
            with connection.cursor() as cursor:
                cursor.execute(
                    "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY"
                )
                cursor.execute("SELECT current_database()")
                assert cursor.fetchone()[0] == "sombra"

            inicio = date(2026, 9, 21)
            hoje = timezone.localdate()
            assert hoje >= inicio

            # Todos, sem filtro de situação.
            # app/colaboradores/models.py:321
            colabs = list(
                Colaborador.objects.all().only("id", "empresa_id").order_by("pk")
            )

            # Mesmo corpo do pareador, universo apurável e pad oficiais.
            # app/ponto/turnos.py:2953,3009,3022
            carga = CargaDaJanela(colabs, inicio, hoje)
            grupos = defaultdict(list)
            examinados = 0
            fechados_sem_celula = 0

            for colab in colabs:
                for turno in carga.turnos(colab):
                    if (
                        turno.aberto
                        or turno.entrada is None
                        or turno.saida is None
                    ):
                        continue

                    dia = turno.data_turno
                    assert dia is not None and inicio <= dia <= hoje

                    if carga.celula(colab.pk, dia) is None:
                        fechados_sem_celula += 1
                        continue

                    examinados += 1

                    # Os dois leitores repartem os pares do juiz pelo piso.
                    # app/ponto/turnos.py:545,565
                    pares = (
                        list(intervalos_do_turno(turno))
                        + list(pausas_abaixo_do_piso(turno))
                    )
                    if not pares:
                        continue

                    # Só mede posições; não procura nem escolhe parceiros.
                    # Mesma ordenação de app/escala/utils.py:450.
                    bats = sorted(turno.batidas, key=lambda b: b.timestamp)
                    pos = {b.timestamp: i for i, b in enumerate(bats)}

                    # Sem resultado parcial baseado em índice ambíguo.
                    if len(pos) != len(bats):
                        raise RuntimeError(
                            f"timestamp ambiguo colab_pk={colab.pk} data={dia}"
                        )
                    if any(a not in pos or z not in pos for a, z in pares):
                        raise RuntimeError(
                            f"ponta ausente colab_pk={colab.pk} data={dia}"
                        )

                    if not any(pos[z] > pos[a] + 1 for a, z in pares):
                        continue

                    # Corte 21 explicitamente solicitado.
                    # app/ponto/janelas.py:24
                    comp_ini, comp_fim = periodo_apuracao_atual(dia, 21)
                    grupos[(comp_ini, comp_fim, colab.empresa_id)].append(
                        (colab.pk, dia.isoformat())
                    )

            linhas = []
            for (ci, cf, empresa_pk), ocorrencias in sorted(grupos.items()):
                linhas.append({
                    "competencia": cf.strftime("%Y-%m"),
                    "janela": [ci.isoformat(), cf.isoformat()],
                    "empresa_pk": empresa_pk,
                    "n_turnos": len(ocorrencias),
                    "n_colabs": len({pk for pk, _ in ocorrencias}),
                    # Uma entrada por turno afetado; pode repetir pk/data.
                    "colab_pk_data": sorted(ocorrencias),
                })

            saida = {
                "inicio": inicio.isoformat(),
                "hoje": hoje.isoformat(),
                "turnos_fechados_com_celula": examinados,
                "turnos_fechados_sem_celula": fechados_sem_celula,
                "turnos_afetados": sum(len(v) for v in grupos.values()),
                "grupos_afetados": linhas,
            }

            # Rollback também no término normal.
            raise _Rollback()

except _Rollback:
    pass

print(json.dumps(saida, ensure_ascii=False, separators=(",", ":")))
```

A contagem mede presença de par não adjacente; não estima mudança de ata ou dinheiro, que também depende das guardas e do alinhamento subsequente. (`app/escala/utils.py:600`, `app/escala/utils.py:613`, `app/escala/utils.py:578`)

perguntas de lei: 1

## 09/11 03:10 · sessao 67cbec47612d · - · - · origem=Ronald (tmux)

Q2/Q3 — leitura concluída, sem execução.

- **A adjacência não é garantia do juiz.** `_pares_marcados` casa primeiro as pontas declaradas, podendo haver batidas entre elas; depois examina pares adjacentes remanescentes. Os dois leitores públicos apenas repartem esse resultado pelo piso. (`app/ponto/turnos.py:811`, `app/ponto/turnos.py:818`, `app/ponto/turnos.py:545`, `app/ponto/turnos.py:565`)

- **Aninhamento é estruturalmente alcançável.** Exemplo por inspeção: DNA/ata `08/12/13/17`; batidas `E08,S12,S12:10,E12:30,E13,S17`. As duas extras podem passar como `_resto` de duas batidas, virar `X`; 12/13 viram `Xi`. O pareador carimba e absorve todas as quatro internas, fechando às 17. O juiz devolve `(12,13)` e `(12:10,12:30)`: reserva apenas as pontas declaradas, deixando as duas extras disponíveis ao segundo laço. Ocorrência real desse exemplo: **[não verificado]**. (`app/ponto/turnos.py:1761`, `app/ponto/turnos.py:1787`, `app/ponto/turnos.py:1804`, `app/ponto/turnos.py:1827`, `app/ponto/turnos.py:1315`, `app/ponto/turnos.py:1371`, `app/ponto/turnos.py:1491`, `app/ponto/turnos.py:811`, `app/ponto/turnos.py:820`, `app/ponto/turnos.py:880`)

- **Só `==` → `>` não basta para aquele arranjo.** Com uma pausa cadastral, os dois pares fazem `_alinhar_por_papel` devolver `None`, caindo no alinhamento antigo; com duas pausas, o par externo pode ocupar a primeira e o interno a segunda, produzindo sequência temporal inversa entre a volta externa e a ida interna. O alinhador não valida a ordem cronológica entre pares; distribui-os pela ordem numérica recebida. (`app/escala/utils.py:530`, `app/escala/utils.py:535`, `app/escala/utils.py:551`, `app/escala/utils.py:561`, `app/escala/utils.py:613`, `app/escala/utils.py:625`)

- **Cruzamento não nasce dos dois laços sobre uma lista cronológica com instantes distintos:** declarados são emparelhados sucessivamente; os outros têm pontas adjacentes, sem usar pontas declaradas. Aninhamento permanece possível porque não se reserva o interior. O pareador ordena a entrada por timestamp. (`app/ponto/turnos.py:1217`, `app/ponto/turnos.py:812`, `app/ponto/turnos.py:819`, `app/ponto/turnos.py:820`)

- **Volta que é saída verdadeira não é pausa do juiz:** as bordas são excluídas por identidade na passagem declarada e na gulosa. Portanto a cura deve preservar o limite interior `0 < i < iz < len(bats)-1`; a última batida continua `sai`, sentada no último marco. (`app/ponto/turnos.py:802`, `app/ponto/turnos.py:811`, `app/ponto/turnos.py:880`, `app/escala/utils.py:467`, `app/escala/utils.py:473`, `app/escala/utils.py:559`)

- **Solta entre pontas:** só recebe `sol` se não pertence a nenhum par. Não se deve “pular até a volta”, pois isso apagaria os papéis de outras pontas interiores. O contrato manda transportar todos os pares e depois tratar as soltas, sem mover borda/par; cada solta tenta o marco mais próximo, livre e compatível com a ordem. O cluster guard [nome] ainda roda por último e pode retirar uma associação já feita. (`app/escala/utils.py:440`, `app/escala/utils.py:507`, `app/escala/utils.py:511`, `app/escala/utils.py:564`, `app/escala/utils.py:578`)

- **Duas pausas disjuntas com solta entre elas:** transcrever ambos os pares na ordem e manter a batida intermediária como `sol`; a distribuição busca a menor distância total entre pares e pausas disponíveis. L-085 mantém o dia da jornada; L-087 impede decidir o papel pelo tipo gravado; nenhuma das duas fornece prioridade para pares sobrepostos. (`app/escala/utils.py:539`, `app/escala/utils.py:553`, `app/escala/utils.py:564`, `app/docs/LEIS.md:117`, `app/docs/LEIS.md:119`; prioridade legal para sobrepostos **[não verificado]**)

- **Desenho mínimo:** em `_papeis_do_turno`, reconhecer pontas interiores não adjacentes por `_iz > _i`, conferir primeiro o conjunto completo de pontas e só então atribuir `sol` às demais. Preservar a associação S/E do juiz e o alinhador existente. A cura geral precisa tornar explícito o conflito de sobreposição, sem selecionar vencedor nem acrescentar queda para DP. O código atual não possui esse tratamento. (`app/escala/utils.py:454`, `app/escala/utils.py:467`, `app/escala/utils.py:472`, `app/escala/utils.py:613`; proposta de tratamento explícito **[não verificado]**)

- **Derivação paralela:** `_papeis_por_alternancia` reconstrói pausas por S/E adjacentes na forma `E S E S …`; só é usada sem `turno`, com `derivar=True`, ao menos quatro batidas, tipos alternados e duração até 26h. É a outra derivação de papel, pertinente à Zona 5. (`app/escala/utils.py:477`, `app/escala/utils.py:489`, `app/escala/utils.py:496`, `app/escala/utils.py:596`)

- **Outra adjacência, pergunta diferente:** `_pausa_fechada_carimbada` usa `zip(bats,bats[1:])`, duas pontas carimbadas e piso/teto; alimenta `_saida_pela_posicao_entre_turnos`. Sua docstring distingue figura de dinheiro e declara casos em que `_pares_marcados` corretamente não entrega pausa; substituir essa função por `intervalos_do_turno` apagaria essa diferença de contrato. (`app/ponto/turnos.py:912`, `app/ponto/turnos.py:915`, `app/ponto/turnos.py:924`, `app/ponto/turnos.py:1005`)

**Decisão de lei candidata:** quando o juiz devolve pausa declarada externa e outra pausa inteiramente dentro dela, qual é a pausa válida — conservar ambas, privilegiar a declarada ou considerar o turno conflitante? O código permite a figura; a regra localizada não resolve o conflito. (`app/ponto/turnos.py:811`, `app/ponto/turnos.py:818`, `app/escala/utils.py:507`; resposta legal **[não verificado]**)

## 09/11 03:10 · sessao cd0a18573d10 · - · - · origem=Ronald (tmux)

1. **PROCEDENTE-candidato — P6 curado parcialmente; `OK` continua com dois significados.** O escritor registra aprovação do brief antes da vaga, mas o leitor chama isso de “aberturas que passaram”. Uma CLI avulsa ou uma chamada posteriormente recusada por vaga ocupada aumenta `ok` e encerra a espera sem abrir construtor. Fontes: `bin/disjuntor.py:1579`, `bin/disjuntor.py:2087`, `bin/codex_pergunta.sh:230`, `bin/codex_pergunta.sh:236`, `bin/disjuntor.py:1961`, `bin/disjuntor.py:1963`, `bin/disjuntor.py:1988`. Prova em memória, executando funções extraídas por AST: `NEGOU 11:00 + OK 11:30`, sem nenhum `ini`, devolveu `ok=1`, `ociosa_h=0.5`, `esperando=[]`; execução integral dos selos [nao verificado].

2. **PROCEDENTE-candidato — correlação da medida admite PID reutilizado.** Os pares `(obra,pid)` são recolhidos de toda a história, sem verificar contemporaneidade ou hash; a chamada é dispensada apenas por encontrar esse par. Fontes: `bin/disjuntor.py:1955`, `bin/disjuntor.py:1957`, `bin/disjuntor.py:1975`. O registro original dispõe também de hash: `bin/codex_limite_estado.py:63`. Prova em memória: `OK` de 01/10, obra/pid111, mais `ini` de 09/11 com outro hash/pid111, sem aprovação contemporânea, produziu `abertos_sem=0`; incidência real [nao verificado].

3. **PROCEDENTE-candidato — ponto aberto 7 não garante cobertura fora do wrapper.** A medida lê somente `ini` de `logs/codex/chamadas.log`; o próprio wrapper é seu escritor. Uma abertura direta sem esse registro não entra na conta; a exceção implementada é `Agent` com chave desligada. Fontes: `bin/codex_pergunta.sh:249`, `bin/codex_pergunta.sh:288`, `bin/codex_limite_estado.py:52`, `bin/disjuntor.py:1972`, `bin/disjuntor.py:1975`, `bin/disjuntor.py:1742`. Aberturas reais fora do wrapper [nao verificado].

4. **PROCEDENTE-candidato — P2 tem contenção diferente entre glob e citados.** Citados exigem que o caminho real fique sob `RAIZ/logs/<dir>`; arquivos descobertos pelo glob são convertidos por `realpath` sem a mesma verificação. Assim, condicionalmente a um symlink em `logs/<OBRA>/brief*.md` apontando para fora, o juiz recebe o destino externo já resolvido. Fontes: `bin/disjuntor.py:1520`, `bin/disjuntor.py:1525`, `bin/disjuntor.py:1536`, `bin/disjuntor.py:1570`, `bin/disjuntor.py:1575`. Existência desses symlinks [nao verificado].

5. **PROCEDENTE-candidato — mutante simples do glob escapa à cobertura apresentada.** Alterar o retorno de `_briefs_da_obra` para `sorted(glob.glob(...))[-1:]` conserva o cenário testado, cujo último brief é ruim, mas deixa passar um primeiro brief ruim seguido de último bom. Fontes: `bin/disjuntor.py:1536`, `bin/tests/test_censo_antes_da_vaga.sh:181`, `bin/tests/test_censo_antes_da_vaga.sh:186`, `bin/tests/test_censo_antes_da_vaga.sh:188`. Prova em memória executada: original/mutante negam bom+ruim; original nega ruim+bom; mutante libera ruim+bom. Sobrevivência integral aos selos [nao verificado], prevista pela cobertura lida.

6. **PROCEDENTE-candidato — selo do wrapper só exerce rc1.** Mutante de `#G:censo`: substituir `python ... || _recusa2 ...` por `python ...; [ "$?" -ne 1 ] || _recusa2 ...`. Ele mantém a recusa exercida pelo fake, mas libera rc2/127. Fontes: `bin/codex_pergunta.sh:230`, `bin/tests/test_codex_pergunta.sh:76`, `bin/tests/test_codex_pergunta.sh:85`, `bin/tests/test_codex_pergunta.sh:210`, `bin/tests/test_codex_pergunta.sh:525`. Prova shell em memória: original e mutante retornam 2 para rc1; para rc2/127 original retorna 2 e mutante retorna 0. Sobrevivência integral [nao verificado]. **O código atual recusa qualquer rc diferente de zero, inclusive traceback Python com rc1:** `bin/codex_pergunta.sh:230`.

7. **OBSERVACAO — P5 cura os contraexemplos declarados.** Todas as perguntas são examinadas, a forma do corte termina em `? cortada por Ronald <aval>`, e o positivo exige saída vazia. Fontes: `bin/disjuntor.py:1454`, `bin/disjuntor.py:1512`, `bin/tests/test_censo_antes_da_vaga.sh:90`, `bin/tests/test_censo_antes_da_vaga.sh:139`. O selo não verifica numericamente o contador `ok`; seus asserts numéricos cobrem somente abertos, negados e horas: `bin/tests/test_censo_antes_da_vaga.sh:246`. Portanto, previsão por inspeção: mutante `oks += 0` em `bin/disjuntor.py:1961` não é distinguido; execução integral [nao verificado].

8. **OBSERVACAO — escritores e leitores da trilha.** Todos os eventos usam `trilha`, que escreve `logs/sessao/disjuntor.log` com timestamp local e detalhe limitado a 300 caracteres: `bin/disjuntor.py:72`, `bin/disjuntor.py:139`, `bin/disjuntor.py:144`.
   - `CENSO OK`: `_censo_nega` na CLI ou `pretool` após suas decisões; leitores de medida em `_censo_24h`. Fontes: `bin/disjuntor.py:1581`, `bin/disjuntor.py:1664`, `bin/disjuntor.py:1960`.
   - `CENSO FORA`: `main_censo`; leitor incorpora o PID como aprovação suficiente, mas não encerra espera nem aumenta `ok`. Fontes: `bin/disjuntor.py:2084`, `bin/disjuntor.py:1955`, `bin/disjuntor.py:1960`.
   - `NEGOU DISJUNTOR CENSO`: `main_pretool`, seu tratamento de exceção e `main_censo`; leitor conta negativas e inicia espera apenas quando reconhece `construtor de <obra> sem CENSO`. Fontes: `bin/disjuntor.py:1763`, `bin/disjuntor.py:1774`, `bin/disjuntor.py:2089`, `bin/disjuntor.py:1949`.
   - `CENSO DESLIGADO`: `_censo_desligado`; leitor soma em `abertos_sem`. Fontes: `bin/disjuntor.py:1742`, `bin/disjuntor.py:1958`, `bin/disjuntor.py:1977`.
   - `PAREI censo`: escritor `_parei_censo_no_relato`; leitores compartilham `prova_parei_censo`, mas a análise de lugar/formato ficou com o agente `parei`. Fontes: `bin/disjuntor.py:247`, `bin/hook_stop_fila1.py:692`, `bin/alarme_sessao_ociosa.py:137`, `bin/handoff_sessao.sh:128`.

## 09/11 03:10 · sessao ede921335bff · - · - · origem=Ronald (tmux)

1. **PROCEDENTE-candidato — P4 permanece parcial: JSON ilegível abre pelo protocolo do próprio hook.** Se `json.loads` levanta, `payload` continua `{}`; `_aberturas_construtor({})` devolve `[]`, portanto o fallback pelo texto cru nem é executado e `main_pretool` termina com rc 0, sem JSON `deny`. Reprodução em memória, sem escrita: entrada truncada `{"tool_name":"Agent","tool_input":{"prompt":"/home/ronald/wt-x"}` produziu rc 0 e stdout vazio. `bin/disjuntor.py:1748`, `bin/disjuntor.py:1751`, `bin/disjuntor.py:1594`, `bin/disjuntor.py:1611`, `bin/disjuntor.py:1614`, `bin/disjuntor.py:1790`.

2. **OBSERVACAO — exceção anterior ao julgamento também pode terminar sem negativa quando o censo é válido.** `_censo_na_excecao` apenas refaz o censo; se ele passa, retorna `None` e a exceção original termina em “ferramenta liberada”, rc 0. Isso é mais estreito que a promessa textual “exceção = nega”. `bin/disjuntor.py:1618`, `bin/disjuntor.py:1622`, `bin/disjuntor.py:1767`, `bin/disjuntor.py:1790`.

3. **OBSERVACAO — importações anteriores ao `try` não têm proteção local; abertura efetiva pelo harness é [nao verificado].** Os imports globais precedem `main_pretool`; a configuração chama Python diretamente, sem tradutor externo de rc para `deny`. Falha ali pode produzir traceback/rc 1 sem JSON de decisão, mas a implementação do harness que interpreta esse rc não está na cópia examinada. `bin/disjuntor.py:59`, `bin/disjuntor.py:1747`, `.claude/settings.json:68`; comportamento real do harness: **[nao verificado]**. O arquivo `eval/harness.py` executa perguntas HTTP do copiloto e não implementa o consumidor de PreToolUse. `eval/harness.py:14`, `eval/harness.py:52`.

4. **PROCEDENTE-candidato — export herdado é negado mesmo com censo conforme.** O classificador só lê atribuições da mesma invocação. Assim, `export CODEX_OBRA=OBRA-E3 CODEX_PAPEL=construtor; bin/codex_pergunta.sh --construtor ...` chega com `obra=''` e é recusado antes de consultar qualquer brief. O teste exercita essa forma somente sem brief; a restrição excede “export sem brief”. `bin/disjuntor.py:1371`, `bin/disjuntor.py:1385`, `bin/disjuntor.py:1690`, `bin/disjuntor.py:1692`, `bin/tests/test_censo_antes_da_vaga.sh:174`.

5. **PROCEDENTE-candidato — contrato de ID incompatível causa negativas permanentes para formas anteriormente aceitas pelo wrapper.** O wrapper admite `^[A-Z0-9-]+$`; `main_censo` exige `_OBRA_ID`, que começa por letra e exige hífen ou letras seguidas de dígitos. Exemplos `ABC` e `09-TURNO-ABERTO-EXPOSTA` passam no primeiro e falham no segundo independentemente do brief. `bin/codex_pergunta.sh:188`, `bin/disjuntor.py:108`, `bin/disjuntor.py:2075`. A existência de chamada atual dessas formas é **[nao verificado]**; o reconhecedor do BACKLOG admite IDs iniciados por dígito explicitamente. `bin/hook_stop_fila1.py:113`, `bin/hook_stop_fila1.py:120`.

6. **PROCEDENTE-candidato — ponto aberto 4: raiz de julgamento pode divergir da raiz da vaga.** `R` vem do arquivo do wrapper; `VIVO` é separado. O censo executa o disjuntor de `R`, cujo BACKLOG e glob também são de `R`, enquanto a vaga deriva do repositório vivo. Uma cópia com BACKLOG antigo sem aquela obra retorna `CENSO FORA`/rc 0, mesmo que o BACKLOG vivo exigisse censo. `bin/codex_pergunta.sh:55`, `bin/codex_pergunta.sh:56`, `bin/codex_pergunta.sh:76`, `bin/codex_pergunta.sh:230`, `bin/disjuntor.py:71`, `bin/disjuntor.py:1536`, `bin/disjuntor.py:1544`, `bin/disjuntor.py:2083`. Ocorrência dessa divergência em produção: **[nao verificado]**. O selo de cópia apenas fornece disjuntor falso e verifica a vaga ocupada; não confronta dois BACKLOGs. `bin/tests/test_codex_pergunta.sh:392`.

7. **OBSERVACAO — predicado exato de obra reconhecida no Agent.** A chamada entra no censo quando `tool_name == 'Agent'`, `subagent_type` não pertence a `{'verificador','Explore','Plan','claude-code-guide','statusline-setup'}` e o prompt casa `/[^\s"\'\\:]*/wt-[A-Za-z0-9._-]+`. `bin/disjuntor.py:102`, `bin/disjuntor.py:104`, `bin/disjuntor.py:1711`. `_obra_de` procura a primeira ocorrência válida entre `obra[s] <ID>` e `leitura ... da <ID>` — nesta segunda âncora, até 80 caracteres sem quebra —, com regex insensível à caixa, mas validação posterior sensível à caixa. O ID é `(?:[A-Z][A-Z0-9]*(?:-[A-Za-z0-9]+)+|[A-Z]+\d+[A-Z0-9]*)`; cauda opcional `(letra)` ou outro ID hifenizado integra a identidade somente quando o ID original não tem hífen. `bin/disjuntor.py:108`, `bin/disjuntor.py:110`, `bin/disjuntor.py:115`, `bin/disjuntor.py:545`, `bin/disjuntor.py:559`, `bin/disjuntor.py:567`.

8. **OBSERVACAO — chamadores expostos ao ponto 6, distinguindo rotas de ocorrências.**
   - Agentes construtores de cópia, inclusive a rota documental de UI em `wt-ui`, sem obra reconhecida no prompt: passam a negar. `CLAUDE.md:306`, `CLAUDE.md:309`, `bin/disjuntor.py:1711`.
   - Wrapper construtor com variáveis herdadas por `export`, sem `CODEX_OBRA` escrita na invocação: passa a negar no hook, independentemente do brief. `bin/disjuntor.py:1371`, `bin/disjuntor.py:1692`.
   - Wrapper chamado por script/`bash -c`: a via executada passa pelo novo `--censo`, ainda que o hook não consiga interpretar o shell. `bin/disjuntor.py:1281`, `bin/codex_pergunta.sh:230`.
   - A sessão principal **não é isenta** quando abre Agent ou chama wrapper: a saída antecipada por ausência de `agent_id` exclui expressamente essas ferramentas. `bin/disjuntor.py:1673`.
   - O tipo `verificador` declarado na configuração de agente é isento desse ramo de censo. `.claude/agents/verificador.md:2`, `bin/disjuntor.py:102`.
   
   Não há no dado fornecido nomes/transcripts de chamadas atualmente vivas que permitam identificar quais efetivamente cairão nessas condições: **[nao verificado]**. Os exemplos concretos de export e Agent sem obra são fixtures, não prova de produção. `bin/tests/test_censo_antes_da_vaga.sh:174`, `bin/tests/test_censo_antes_da_vaga.sh:178`.

9. **OBSERVACAO — Q5 está curada na expressão do wrapper.** `python3 -I ... --censo "$OBRA" || _recusa2` recusa qualquer rc diferente de zero; `_recusa2` termina com rc 2. Portanto Python com traceback/rc 1 recusa; o `flock` só ocorre depois. `bin/codex_pergunta.sh:183`, `bin/codex_pergunta.sh:230`, `bin/codex_pergunta.sh:236`. O selo mantém a vaga presa e exige recusa por censo, provando a ordem pretendida pelo teste. `bin/tests/test_codex_pergunta.sh:210`.

10. **OBSERVACAO — pontos abertos 5, 8 e 9.**
    - **5: risco declarado; desligamento comum não cobre a CLI.** `main_censo` não consulta `desligado`; o PreToolUse consulta. O texto da configuração enumera B1–B4/B4-CODEX/ANEXO-ENXUTO como escopo da chave, sem incluir censo. `bin/disjuntor.py:1756`, `bin/disjuntor.py:2069`, `bin/disjuntor_settings.json:3`. Exigência de que essa mesma chave desligue I1: **[nao verificado]**.
    - **8: restrição de formato declarada.** O juiz exige literalmente `? cortada por Ronald <token>` no fim; texto posterior deixa de contar, embora contenha a expressão de corte. `bin/disjuntor.py:1454`, `bin/disjuntor.py:1512`. A triagem fornecida pede ancoragem; autorização de todos os formatos rejeitados: **[nao verificado]**.
    - **9: ajuste coerente da âncora.** A mutação continua removendo `bash|sh` do ramo pertinente; `env` passou a ramo separado com atribuições. `bin/disjuntor.py:1288`, `bin/tests/test_disjuntor.sh:1741`. Execução verde do selo nesta auditoria: **[nao verificado]**.

## 09/11 03:10 · sessao 4358b47b3955 · FIO-E0-V4-E2 · construtor · origem=Code

Contrato E2 aplicado em `chamados/veredito_lavrado.py:119`, `:141`, `:213` e `chamados/management/commands/backfill_veredito_lavrado.py:111`. **Resta o ACHADO da catraca**, conforme a condição imposta: `instancias_do_turno` não monta universo pela porta (`chamados/services/tarefa_do_dia.py:88`). Suíte, testes RED e morte dos mutantes: **[nao verificado]**.

Alterações por hunk:

| Arquivo:linha atual | Alteração |
|---|---|
| [chamados/veredito_lavrado.py:95](/home/ronald/wt-codex-fio-e0-v4-e2-015740/app/chamados/veredito_lavrado.py:95) | `_hoje(agora)` obrigatório; somente converte o instante; rejeita `None`. |
| `chamados/veredito_lavrado.py:107` | `relogio_virou` exige `agora`; conserva as duas comparações em dia local. |
| `chamados/veredito_lavrado.py:119` | `expira_em(data_evento, hoje)` obrigatório, com `hoje` do tipo `date`; cálculo começa em `max(hoje, data_evento)` em `:141`. |
| `chamados/veredito_lavrado.py:213` | `esta_stale(p, marcos, agora)` obrigatório; conserva rejeição de lavra futura em `:224`. |
| `chamados/veredito_lavrado.py:273` | `lavrar` fornece o dia local do mesmo instante gravado em `julgado_em`. |
| [backfill_veredito_lavrado.py:87](/home/ronald/wt-codex-fio-e0-v4-e2-015740/app/chamados/management/commands/backfill_veredito_lavrado.py:87) | Importa o conversor puro; lê um instante em `:94`, converte em `:95` e passa `hoje` em `:111`. |
| [chamados/services/raiox.py:10](/home/ronald/wt-codex-fio-e0-v4-e2-015740/app/chamados/services/raiox.py:10) | Varredura exige `agora`; saiu a leitura interna do relógio. Marcos, lavra futura e comparação local usam esse horizonte em `:38`, `:41`, `:43`. |
| [lavrar_perguntas_stale.py:59](/home/ronald/wt-codex-fio-e0-v4-e2-015740/app/chamados/management/commands/lavrar_perguntas_stale.py:59) | Command lê o relógio e entrega o horizonte à varredura em `:60`. |
| [chamados/tests/test_e1_fila.py:352](/home/ronald/wt-codex-fio-e0-v4-e2-015740/app/chamados/tests/test_e1_fila.py:352) | Migração do chamador indireto de `expira_em`; mutantes atualizados em `:377`, `:384`; demais chamadas migradas em `:413`, `:427`. |
| [test_selo_concordancia_varredura_stale.py:56](/home/ronald/wt-codex-fio-e0-v4-e2-015740/app/chamados/tests/test_selo_concordancia_varredura_stale.py:56) | Um horizonte compartilhado pela varredura e pelo leitor da lavra. |
| [test_selo_get_nao_escreve.py:72](/home/ronald/wt-codex-fio-e0-v4-e2-015740/app/chamados/tests/test_selo_get_nao_escreve.py:72) | Três chamadas entregam o instante explicitamente: `:72`, `:102`, `:116`. |
| [escala/tests/test_signal_ancora.py:107](/home/ronald/wt-codex-fio-e0-v4-e2-015740/app/escala/tests/test_signal_ancora.py:107) | Atualizada a âncora do mutante de lavra futura; varredura recebe instante com relógio envenenado em `:119`. |
| `escala/tests/test_signal_ancora.py:281` | Acrescentados seis RED e seis métodos de mutação para contratos, conversão local e passagem dos argumentos pelos commands. |
| [core/tests/test_medida_turno_do_par.py:313](/home/ronald/wt-codex-fio-e0-v4-e2-015740/app/core/tests/test_medida_turno_do_par.py:313) | Dois motivos individuais e registro `[1167, 1319]`. |

Inventário dos chamadores alterados, antes → depois:

| Função | Arquivo:linha anterior → atual | Migração |
|---|---|---|
| `_hoje` | `chamados/veredito_lavrado.py:114 → :116` | Já passava `agora`; mantido. |
| `_hoje` | `chamados/veredito_lavrado.py:137 → removida` | `expira_em` usa diretamente `hoje`, agora em `:141`. |
| `_hoje` | nova em `chamados/veredito_lavrado.py:273` | Converte `_agora` da lavra. |
| `_hoje` | nova em `chamados/management/commands/backfill_veredito_lavrado.py:95` | Converte `agora` do command. |
| `expira_em` | `chamados/veredito_lavrado.py:267 → :273` | `agora=_agora` → `_hoje(_agora)`. |
| `expira_em` | `chamados/management/commands/backfill_veredito_lavrado.py:111 → :111` | `agora=agora` → `hoje`. |
| `expira_em`, indireto por `fn` | `chamados/tests/test_e1_fila.py:352 → :352` | `fn(DIA)` → `fn(DIA, timezone.localdate(timezone.now()))`. |
| `esta_stale` | `chamados/tests/test_e1_fila.py:412 → :413` | Acrescentado `marcos=None` explícito. |
| `relogio_virou` | `chamados/veredito_lavrado.py:222 → :228` | Já passava `agora`; mantido. |
| `relogio_virou` | `chamados/services/raiox.py:41 → :43` | Recebe agora o horizonte originado no command. |
| `pks_stale_por_varredura` | `chamados/management/commands/lavrar_perguntas_stale.py:58 → :60` | Acrescentado `agora`. |
| mesma | `chamados/tests/test_e1_fila.py:426 → :427` | Acrescentado `agora`. |
| mesma | `chamados/tests/test_selo_concordancia_varredura_stale.py:56 → :57` | Acrescentado o instante compartilhado. |
| mesma | `chamados/tests/test_selo_get_nao_escreve.py:72 → :72`, `:102 → :102`, `:116 → :116` | Relógio fornecido pelo teste chamador. |
| mesma, indireto por `fn` | `escala/tests/test_signal_ancora.py:121 → :121` | Acrescentado `self.AGORA`; relógio interno envenenado. |

Demais chamadas diretas, já compatíveis, antes → depois:

| Função | Chamadores preservados |
|---|---|
| `expira_em` | `chamados/tests/test_selo_lavrado_orfaos.py:60→60`, `:61→61`, `:85→85`, `:86→86`, `:97→97`; `chamados/tests/test_e2_tarefa_do_dia.py:686→686`. |
| `esta_stale` | `chamados/services/tarefa_do_dia.py:349→349`; `chamados/management/commands/auditar_invariantes_chamados.py:1167→1167`; `chamados/tests/test_e2_tarefa_do_dia.py:685→685`; `chamados/tests/test_porta_veredito_pergunta.py:270→270`, `:275→275`; `chamados/tests/test_selo_concordancia_varredura_stale.py:60→60`; `chamados/tests/test_selo_lavrado_orfaos.py:103→103`, `:115→115`, `:118→118`, `:128→128`, `:130→130`. |

As omissões em `chamados/tests/test_e2_tarefa_do_dia.py:696` e `:698` permanecem: são precisamente os testes que exigem `TypeError`. Os novos leitores indiretos dos contratos e das funções estão em `escala/tests/test_signal_ancora.py:294`, `:303`, `:317`, `:334`, `:353`, `:375`, `:408`.

Outros consumidores de `lavrar`: `chamados/models.py:312`, `:3007`, `:3150`; `chamados/services/fio.py:923`, `:1033`; `chamados/services/relavra_via.py:126`; `chamados/management/commands/lavrar_perguntas_stale.py:89`. Nos testes: `chamados/tests/test_e1_fila.py:372`; `chamados/tests/test_porta_veredito_pergunta.py:271`, `:286`, `:292`, `:318`, `:322`; `chamados/tests/test_selo_teto_placar.py:97`; `chamados/tests/test_profile_placar_rodinha.py:159`.

Consumidores dos commands: backfill em `chamados/tests/test_porta_veredito_pergunta.py:502`, `:509`; lavra stale em `chamados/tests/test_selo_cron_lavrar_stale.py:45` e `chamados/tests/test_selo_get_nao_escreve.py:115`; novos exercícios diretos em `escala/tests/test_signal_ancora.py:395`, `:423`.

Mantive `_dia_de_turno_da_pergunta` na tranca (`chamados/juizes.py:949`). Ele devolve `data_evento` para perguntas de fato e `None` para conversa (`chamados/juizes.py:332`). Os confrontos E2 consultados usam perguntas de fato (`chamados/tests/test_e2_tarefa_do_dia.py:1127`, `:1198`, `:1208`); passagem desses testes: **[nao verificado]**.

Os registros deslocados continuam apontando para `_julgar_colab_corpo`: conversão da adesão em `ponto/services/cartorio.py:1167` e recorte das batidas em `:1319`. Os motivos individuais ficaram em `core/tests/test_medida_turno_do_par.py:313` e `:314`. Igualdade textual com o fonte histórico indisponível de `2ea1f4c3`: **[nao verificado]**.

**ACHADO catraca: chamados/services/tarefa_do_dia.py:88.** A função recebe marcos prontos e converte a data civil em `:99`; não chama `parear_turnos` nem monta universo pela porta. Não acrescentei sua entrada a `CLASSIFICAR` (`core/tests/test_medida_turno_do_par.py:344`).

Os sete testes do anexo ficam assim; passagem efetiva de cada um: **[nao verificado]**. [nao verificado]

| Teste | Motivo tratado ou pendência | [nao verificado]
|---|---|
| `test_a_tarefa_e_a_lavra_lidas_sem_relogio` — `chamados/tests/test_e2_tarefa_do_dia.py:667` | `date` entra diretamente em `expira_em`; `_hoje` somente converte instante explícito (`chamados/veredito_lavrado.py:104`, `:141`). |
| `test_a_VESPERA_e_LEI_o_ultimo_dia_da_competencia_vive_UMA_a_mais` — `chamados/tests/test_selo_lavrado_orfaos.py:69` | Corrigido o contrato de entrada; preservado o limite com `- timedelta(days=1)` (`chamados/veredito_lavrado.py:145`). |
| `test_expira_em_devolve_FIM_DE_COMPETENCIA_nao_prazo_fixo` — `chamados/tests/test_selo_lavrado_orfaos.py:42` | Corrigido o contrato de entrada; preservado avanço pelo fim da competência (`chamados/veredito_lavrado.py:142`, `:147`). |
| `test_esta_stale_exige_agora` — `chamados/tests/test_e2_tarefa_do_dia.py:692` | Assinaturas obrigatórias em `chamados/veredito_lavrado.py:119`, `:213`. |
| `test_tarefa_e_leitores_da_lavra_nao_leem_o_relogio` — `chamados/tests/test_e2_tarefa_do_dia.py:1009` | Removida a leitura interna de `_hoje`; leitura da lavra permanece na borda declarada (`chamados/veredito_lavrado.py:104`, `:271`). |
| `test_CATRACA_linhas_reais_congeladas_exigem_registro_no_mesmo_commit` — agora `core/tests/test_medida_turno_do_par.py:535` | Registro ajustado em `core/tests/test_medida_turno_do_par.py:315`. |
| `test_a_classificar_publicado_sem_entrar_como_criterio_amplo` — agora `core/tests/test_medida_turno_do_par.py:541` | **Pendência mantida pelo ACHADO acima. Não afirmo que passa.** |

Comandos individuais para o Code, não executados aqui. Os seis RED e seis métodos MORDE novos estão em `escala/tests/test_signal_ancora.py:294`, `:303`, `:324`, `:328`, `:343`, `:347`, `:363`, `:367`, `:395`, `:399`, `:423`, `:427`:

```bash
r() {
  PYTHONDONTWRITEBYTECODE=1 python3 -B manage.py test \
    --settings=config.settings_test "$1"
}
c() { r "escala.tests.test_signal_ancora.RebaseE2ContratoRelogioTest.$1"; }

c test_RED_argumentos_obrigatorios
c test_MORDE_defaults_opcionais
c test_RED_hoje_so_converte_instante
c test_MORDE_hoje_le_relogio
c test_RED_expira_recebe_date_sem_relogio
c test_MORDE_expira_rele_relogio
c test_RED_relogio_virou_preserva_dois_dias_locais
c test_MORDE_dia_UTC_em_cada_lado
c test_RED_backfill_passa_date_do_mesmo_instante
c test_MORDE_backfill_instante_ou_segundo_relogio
c test_RED_command_stale_entrega_horizonte
c test_MORDE_command_omite_horizonte
```

Comandos dos exercícios existentes afetados pela migração (`escala/tests/test_signal_ancora.py:101`, `:105`, `:124`, `:128`, `:134`; `chamados/tests/test_e1_fila.py:354`, `:370`, `:375`, `:382`, `:409`, `:420`):

```bash
l() { r "escala.tests.test_signal_ancora.RebaseE2LeitoresTest.$1"; }

l test_RED_lavra_futura
l test_MORDE_lavra_futura
l test_RED_varredura_no_mesmo_horizonte
l test_MORDE_varredura_sem_corte
l test_MORDE_varredura_aceita_lavra_futura

r chamados.tests.test_e1_fila.F5RelogioDaBordaTest.test_RED_expira_em_le_o_relogio_da_borda
r chamados.tests.test_e1_fila.F5RelogioDaBordaTest.test_a_lavra_e_a_janela_leem_o_MESMO_instante
r chamados.tests.test_e1_fila.F5RelogioDaBordaTest.test_MORDE_duas_leituras_do_relogio_na_lavra
r chamados.tests.test_e1_fila.F5RelogioDaBordaTest.test_MORDE_a_volta_do_date_today
r chamados.tests.test_e1_fila.DiaLocalEstaStaleTest.test_RED_esta_stale_le_o_dia_local
r chamados.tests.test_e1_fila.DiaLocalVarreduraTest.test_RED_a_varredura_le_o_dia_local
```

`py_compile` concluiu com rc=0 nos nove arquivos alterados listados acima. Comando executado, com escrita de bytecode bloqueada:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B - <<'PY'
import py_compile
import subprocess
from unittest.mock import patch

arquivos = subprocess.check_output(
    ['git', 'diff', '--name-only', '--relative'], text=True
).splitlines()

with patch('py_compile.os.makedirs'), \
     patch('py_compile.importlib._bootstrap_external._write_atomic'):
    for arquivo in arquivos:
        if arquivo.endswith('.py'):
            py_compile.compile(arquivo, doraise=True)
            print('OK:', arquivo)
PY
```

## 09/11 03:09 · sessao 0c12bed4fd26 · - · - · origem=Ronald (tmux)

Q4 — cadeia e efeitos, somente leitura:

- `_match_marcos` é o ponto comum de alinhamento; recebe papel de `_papeis_do_turno` quando há `turno=`, ou de `_papeis_por_alternancia` sem turno. O ramo de papéis gera células, órfãs e marca as batidas consumidas. (`app/escala/utils.py:585`, `app/escala/utils.py:596`, `app/escala/utils.py:613`, `app/escala/utils.py:620`)
- O builder por turno passa `turno=`; o single e o dry-run chamam com `derivar=False`. Assim, corrigir apenas `_papeis_do_turno` incide diretamente no builder por turno, enquanto esses outros caminhos conservam a DP. (`app/escala/utils.py:776`, `app/escala/utils.py:1650`, `app/escala/utils.py:1701`)
- Para apurar o realizado da montagem atual, o builder cria cópias em memória das células, entrega as lâmpadas e órfãs recém-montadas, chama `turnos_do_colab` com elas e depois `realizado_do_dia`; grava seu resultado no dict da grade. Essa é a passagem pela qual um novo alinhamento altera o intervalo reconhecido e o número da ata. (`app/escala/utils.py:1886`, `app/escala/utils.py:1892`, `app/escala/utils.py:1898`, `app/escala/utils.py:2045`, `app/escala/utils.py:2058`)
- O cartório carrega `batidas_apuraveis`, chama o builder e serializa lâmpadas, órfãs e `minutos_realizados` em `ata_do_dia`. (`app/ponto/services/cartorio.py:1121`, `app/ponto/services/cartorio.py:1126`, `app/ponto/services/cartorio.py:696`, `app/ponto/services/cartorio.py:706`)
- A Zona 5 do motor chama `_match_marcos` **sem** `turno=` sobre entradas/saídas dos períodos do dia, associa períodos aos marcos `hi`/`hfi` e aplica atraso/antecipada. Sua leitura direta usa `_papeis_por_alternancia`, portanto a troca da adjacência de `_papeis_do_turno` não basta, isoladamente, para afirmar mudança direta neste caminho. (`app/ponto/motor_calculo_v2.py:3060`, `app/ponto/motor_calculo_v2.py:3062`, `app/ponto/motor_calculo_v2.py:3064`, `app/ponto/motor_calculo_v2.py:3077`, `app/escala/utils.py:597`)
- `marco_ocupado_pela_batida` transcreve `_match_marcos`, sem turno, devolvendo rótulo/delta; o casamento com a batida usa HH:MM. (`app/escala/utils.py:395`, `app/escala/utils.py:427`, `app/escala/utils.py:429`)
- `regua_defesa.cumpriu_a_volta_do_intervalo` entrega apenas um período a esse leitor e exige `hfi` com delta dentro da tolerância; esse único período fica na DP, porque a derivação por alternância exige pelo menos quatro batidas. Motor e calculador consultam esse predicado para retirar o previsto de entrada de quem cumpriu a volta. (`app/escala/regua_defesa.py:489`, `app/escala/regua_defesa.py:491`, `app/escala/regua_defesa.py:493`, `app/escala/utils.py:485`, `app/escala/utils.py:489`, `app/ponto/motor_calculo_v2.py:2117`, `app/ponto/calculador/regras.py:251`)

Dinheiro e leitores:

- O realizado líquido subtrai `intervalos_do_turno`; a conversão para minutos acontece antes da subtração. (`app/ponto/turnos.py:740`, `app/ponto/turnos.py:756`, `app/ponto/turnos.py:758`, `app/ponto/turnos.py:767`)
- O motor também lê o papel da ata quando recebe células alimentadas, chama o mesmo pareador e transporta os intervalos pelo adaptador. Portanto o alinhamento pode chegar ao cálculo financeiro **pela ata**, além dos chamadores diretos de `_match_marcos`. (`app/ponto/motor_calculo_v2.py:495`, `app/ponto/motor_calculo_v2.py:500`, `app/ponto/motor_calculo_v2.py:503`, `app/ponto/motor_calculo_v2.py:530`, `app/ponto/adaptador_motor.py:44`)
- No 12x36, o motor recorta os intervalos na janela, calcula pausa real, subtrai da duração, calcula supressão com tolerância e grava minutos trabalhados/indenização. A tolerância de intrajornada é dez minutos na cópia. (`app/ponto/motor_calculo_v2.py:2764`, `app/ponto/motor_calculo_v2.py:2771`, `app/ponto/motor_calculo_v2.py:2774`, `app/ponto/motor_calculo_v2.py:2776`, `app/ponto/motor_calculo_v2.py:2784`, `app/ponto/motor_calculo_v2.py:2785`, `app/ponto/motor_calculo_v2.py:69`)
- `DiaPago` recebe horas trabalhadas e intrajornada indenizada dividindo os campos do período por 60. O espelho lê `realizado_h` da autoridade do realizado e `pago_h`/rubricas da lavratura, portanto os números podem divergir. (`app/ponto/services/dia_pago.py:178`, `app/ponto/services/dia_pago.py:186`, `app/ponto/services/espelho.py:783`, `app/ponto/services/espelho.py:799`, `app/ponto/services/espelho.py:811`, `app/ponto/services/espelho.py:813`)
- O leitor da célula transporta o realizado da ata; o PDF de validação soma e divide por 60. Causas, classificação, comparação ata/juiz e calendário também o leem. (`app/escala/services/leitor_celula.py:369`, `app/escala/services/leitor_celula.py:406`, `app/folha/validacao_pdf.py:48`, `app/folha/validacao_pdf.py:54`, `app/folha/causas.py:85`, `app/ponto/supra_juiz.py:224`, `app/ponto/services/bordas_realizado.py:87`, `app/colaboradores/services/calendario.py:359`)
- Fechamento e percentual usam realizado limitado ao previsto de cada dia; a mesma diferença pode desaparecer no teto ou afetar cumprimento/abono. (`app/ponto/services/dia_pago.py:394`, `app/ponto/services/dia_pago.py:395`, `app/ponto/services/fechamento.py:1061`, `app/ponto/services/fechamento.py:1064`, `app/folha/export.py:1096`, `app/folha/export.py:1103`)

No dado raspado, antes→rejulgado: ata **660→599 min**, pausa descontada pelo motor **58,0130167→119,6599667 min**; indenização intrajornada **0→0**; pagamento já lavrado **10,87 h→10,87 h**, com rubricas exibidas idênticas. São evidências do texto fornecido, sem arquivo local/linha: **[não verificado]**. O segundo `minutos_trabalhados` está raspado, e o dado não prova novo pagamento lavrado nem valor em reais: **[não verificado]**.

## 09/11 03:09 · sessao 49739fa9eed9 · - · - · origem=Ronald (tmux)

A exigência de adjacência não tem motivação documentada nos trechos localizados. A declaração da obra manda transcrever o papel do juiz e acomodar “par numa pausa”, deixando as soltas para o marco livre mais próximo; não menciona exigir batidas consecutivas. (`app/docs/RELATO.md:1709`, `app/docs/RELATO.md:1710`, `app/escala/utils.py:440`, `app/escala/utils.py:507`)

O pouso registrado é `b3bf529e`, em 29/10 07:11; o texto identifica `_match_marcos`, a ata, Zona 5 e `cumpriu_a_volta` como consumidores da mesma resposta. (`app/docs/RELATO.md:1657`, `app/docs/RELATO.md:1659`) O diff que atribui a introdução precisamente a `9da200c8` não está disponível nesta cópia sem `.git`: **[não verificado]**. Não localizei justificativa adicional nos resultados de busca em LAPIDES/CORTES: **[não verificado]**.

Os casos nomeados não demonstram dependência da adjacência:

- **col345:** o caso é uma entrada de trabalho às 15:00 que corresponde à volta `hfi`; o par fechado é **15:00→18:56**, e o teste impede atraso falso. Não é pausa com batida interposta. (`app/ponto/tests/test_calculador_portoes_de_marco.py:15`, `app/ponto/tests/test_calculador_portoes_de_marco.py:56`)
- **col60/O271:** usa **E12:00→S19:06**, distingue a cabeça atrasada do turno da volta e cobra atraso cru de 300 minutos. As contraprovas usam entrada em `hfi` e `hfi+5`, sem atraso. São períodos sem pausa interna. (`app/ponto/tests/test_o271_pontualidade_no_comercial.py:177`, `app/ponto/tests/test_o271_pontualidade_no_comercial.py:192`, `app/ponto/tests/test_o271_pontualidade_no_comercial.py:200`)
- **[nome]:** o cluster tem quatro batidas — 18:56, 18:58, 19:04, 19:05 — e exige órfãs em vez de ocupar marcos distantes; qualquer pausa interna dessa sequência é adjacente. A proteção correspondente permanece no filtro final de `_alinhar_por_papel`, separado da condição de adjacência. (`app/ponto/tests/test_pareador_cluster_marco.py:53`, `app/ponto/tests/test_pareador_cluster_marco.py:59`, `app/escala/utils.py:572`)
- **Nominais da obra:** A, controle regular, convergência de tipo trocado e noturno apresentam quatro batidas, com as duas pontas internas consecutivas; B/F/C/D/E tratam entrada/saída ou batida solta, sem pausa não adjacente. (`app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:104`, `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:165`, `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:326`, `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:358`, `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:129`, `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:143`, `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:154`, `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:212`, `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:223`)

Por inspeção, retirar apenas `_iz == _i + 1` não altera a elegibilidade dos pares desses exemplos: os pares continuam adjacentes; os casos sem par continuam sem par. Execução dessa alteração contra os testes: **[não verificado]**. A afirmação universal de que “nenhum caso quebraria” também fica **[não verificado]**. [nao verificado]

Há precedente explícito contra redescobrir o par pela adjacência: o teste da O68b documenta que o par declarado deve vencer o adjacente guloso e compara a resposta com/sem uma sobra. Não é o mesmo cenário da solta *entre* as pontas, mas confirma a prioridade do par declarado. (`app/ponto/tests/test_o68b_papel_da_ata_no_motor.py:623`, `app/ponto/tests/test_o68b_papel_da_ata_no_motor.py:701`)

## 09/11 03:08 · sessao 9d4f10c661b8 · - · - · origem=Ronald (tmux)

1. **PROCEDENTE-candidato — P3/ponto aberto 1: cura apenas formal.** O PAREI passa pela porta existente, mas entra em `## ALARMES DA ESTEIRA`, enquanto o Stop julga somente a primeira linha útil do painel e o handoff encerra a coleta no primeiro `##`. O alarme procura apenas nas primeiras 40 linhas. Assim, compartilhar o reconhecedor não garante que leiam a linha escrita. (`bin/disjuntor.py:267`; `app/core/esteira_vigia.py:863`; `bin/hook_stop_fila1.py:407`; `bin/hook_stop_fila1.py:429`; `bin/handoff_sessao.sh:102`; `bin/handoff_sessao.sh:128`; `bin/alarme_sessao_ociosa.py:124`.)

   Prova executada somente em memória, com as funções extraídas por AST: o fixture do selo recebe o PAREI na linha 7, mas `_linha_do_topo()` devolve `texto neutro do painel`; o processamento foi interrompido antes de `mkstemp`. A divergência decorre diretamente da inserção e seleção existentes. (`bin/tests/test_censo_antes_da_vaga.sh:145`; `app/core/esteira_vigia.py:864`; `bin/hook_stop_fila1.py:438`.)

2. **PROCEDENTE-candidato — ponto aberto 2: idempotência sem atomicidade.** A consulta de existência precede a aquisição do lock pelo escritor; dois chamadores podem observar ausência e inserir duas vezes. A porta serializa cada inserção, mas não deduplica dentro da trava. (`bin/disjuntor.py:258`; `bin/disjuntor.py:261`; `bin/disjuntor.py:269`; `app/core/esteira_vigia.py:844`; `app/core/esteira_vigia.py:854`; `app/core/esteira_vigia.py:867`.)

   Além disso, a consulta percorre o arquivo inteiro: uma linha histórica impede republicação e promoção ao painel em negativa posterior da mesma obra. (`bin/disjuntor.py:259`; `bin/disjuntor.py:260`; `bin/disjuntor.py:261`.)

3. **OBSERVACAO — P7 parcialmente curado.** Stop, handoff e alarme chamam `prova_parei_censo`, e a string produzida coincide com a forma literal aceita. Entretanto, os recortes diferem: primeira linha útil, bloco anterior ao primeiro título e primeiras 40 linhas, respectivamente. Permanece a divergência semântica de localização descrita no achado 1. (`bin/disjuntor.py:1457`; `bin/hook_stop_fila1.py:696`; `bin/hook_stop_fila1.py:700`; `bin/hook_stop_fila1.py:716`; `bin/handoff_sessao.sh:102`; `bin/handoff_sessao.sh:128`; `bin/alarme_sessao_ociosa.py:124`; `bin/alarme_sessao_ociosa.py:137`.)

4. **PROCEDENTE-candidato — selo sem mordida na ligação escritor → leitor.** O selo do censo apenas conta a linha em todo o RELATO; o selo do Stop fabrica a linha diretamente no topo. Nenhum desses casos exige que o RELATO produzido pela negativa seja reconhecido pelo Stop. Isso permite os testes locais passarem preservando o defeito do achado 1. (`bin/tests/test_censo_antes_da_vaga.sh:201`; `bin/tests/test_censo_antes_da_vaga.sh:202`; `bin/tests/test_hook_esperando_com_prova.sh:62`; `bin/tests/test_hook_esperando_com_prova.sh:209`.)

5. **OBSERVACAO — mesma porta, sem segundo escritor acrescentado nessa alteração.** Tanto a negativa comum quanto o PAREI carregam `app/core/esteira_vigia.py` e chamam `no_relato`; lock e substituição atômica pertencem a essa porta. Isso comprova identidade do escritor, mas não adequação da seção escolhida para um PAREI. (`bin/disjuntor.py:195`; `bin/disjuntor.py:238`; `bin/disjuntor.py:240`; `bin/disjuntor.py:267`; `bin/disjuntor.py:269`; `app/core/esteira_vigia.py:848`; `app/core/esteira_vigia.py:884`.)

6. **OBSERVACAO — ausência de hora comprovada; efeito da dieta [nao verificado].** A string escrita não contém carimbo temporal. Não localizei, nesta auditoria, implementação da dieta que permita provar sua permanência por esse motivo. (`bin/disjuntor.py:1457`; `bin/disjuntor.py:255`; [nao verificado].)

## 09/11 03:06 · sessao b385027a2194 · COBPARC-ATA-DEFASADA-09 · leitura · origem=Code

1. **Cálculo pela regra, antes da comparação:** o campo mede minutos de relógio em **HH:MM, sem segundos**, atribuídos ao dia do turno. A madrugada de 04/09 pertence ao turno iniciado em 03/09, pela L-085. (`app/ponto/turnos.py:584`, `app/ponto/turnos.py:605`, `app/docs/LEIS.md:117`)

   A L-087 permite que a batida gravada como **E às 21:01** cumpra o marco **S21:00**: o papel vem do instante contra o marco. Nesse alinhamento, 20:00 e 21:02 sobram; a pausa declarada é **21:01→22:00**. Uma órfã interposta não desfaz esse par: a autoridade casa primeiro as duas pontas declaradas. (`app/docs/LEIS.md:119`, `app/escala/utils.py:230`, `app/ponto/turnos.py:811`, `app/ponto/turnos.py:1816`)

   Portanto: **14:51→21:01 = 370 min; 22:00→02:50 = 290 min; total = 660 min**, equivalentemente **719 − 59 = 660**. A pausa supera o piso de 15 minutos associado ao art. 71 §1 da CLT pela cópia; pagamento e indenização de intrajornada são outra apuração. (`app/ponto/turnos.py:495`, `app/ponto/turnos.py:710`, `app/ponto/turnos.py:717`)

2. **O seletor explica a diferença; a subtração final continua a mesma.** O alinhamento por custo, `app/escala/utils.py:_alinhar:216`, escolhe 21:01 para S21:00 e produz a pausa de 59 minutos; `app/ponto/turnos.py:realizado_dos_turnos:758` desconta essa pausa, resultando em **660**. (`app/escala/utils.py:230`, `app/ponto/turnos.py:767`)

   O caminho atual perde esse par em **`app/escala/utils.py:_papeis_do_turno:467`**: exige `_iz == _i + 1`. Como existe a órfã 21:02 entre 21:01 e 22:00, as pontas passam a ser `sol`. Depois, **`app/escala/utils.py:_alinhar_por_papel:564`** percorre as soltas cronologicamente: 20:00 ocupa primeiro S21:00; 21:01 encontra o marco ocupado. A pausa vira **20:00→22:00 = 120 min**, dando **719 − 120 = 599**, isto é, **−61 min**. (`app/escala/utils.py:472`, `app/escala/utils.py:566`, `app/escala/utils.py:571`, `app/ponto/turnos.py:758`)

   No `gitlog.txt` fornecido, `fba8aac8` identifica a obra **JUIZ-DO-PAR-CURA-NA-FONTE**, com declaração v2/10; atribuir-lhe precisamente a introdução da linha 467 exige o diff histórico **[não verificado]**. A cópia documenta o pouso dessa mudança como **`b3bf529e`, em 29/10**, nomeando `_papeis_do_turno` entre as funções alteradas. (`app/docs/RELATO.md:1657`, `app/docs/RELATO.md:1709`)

   **Falta fechar a proveniência histórica:** o anexo não fornece `lavrada_em/regra_versao` da ata **[não verificado]**. `julgada_em` não prova quando seu número foi escrito: `carimbar` atualiza esse campo, enquanto a ata só é substituída quando diferente. Setembro permanece atualmente em `regra:1`; isso não identifica o corpo histórico do seletor. (`app/ponto/portas/celula.py:1060`, `app/ponto/portas/celula.py:995`, `app/ponto/services/cartorio.py:188`)

3. **ESTRUTURA é o dono sustentável, pelo mecanismo localizado.** O contrato manda transportar cada par inteiro e impede que soltas movam o par; a exigência de adjacência descarta um intervalo que a autoridade reconhece. O sítio é **`_papeis_do_turno:467`**, com consequência em **`_alinhar_por_papel:566`**. (`app/escala/utils.py:507`, `app/escala/utils.py:511`, `app/ponto/turnos.py:811`)

   O e6 sozinho não prova isso: ESTRUTURA é seu resultado residual, e sua assinatura não recebe idade ou versão da ata. Aqui há evidência adicional no código; a conclusão não depende apenas do rótulo da sonda. (`app/ponto/management/commands/e6_oraculo.py:53`, `app/ponto/management/commands/e6_oraculo.py:69`, `app/ponto/management/commands/e6_oraculo.py:110`)

4. **Leitores e significado:**

   - **Diretos:** `leitor_celula` usa o número como evidência de trabalho e o transporta para a grade; o PDF de validação soma minutos e divide por 60. (`app/escala/services/leitor_celula.py:369`, `app/escala/services/leitor_celula.py:406`, `app/folha/validacao_pdf.py:48`, `app/folha/validacao_pdf.py:54`)
   - **Via grade:** causas da folha, classificação do dia, comparação ata/juiz, calendário e diagnóstico “sem turno pareado” do espelho. (`app/folha/causas.py:85`, `app/ponto/supra_juiz.py:224`, `app/ponto/services/bordas_realizado.py:87`, `app/colaboradores/services/calendario.py:359`, `app/ponto/services/espelho.py:892`)
   - **Fechamento e percentual:** transformam o valor, aplicando **teto pelo previsto de cada dia**; portanto seus números derivados têm significado mais restrito que o relógio líquido da ata. (`app/ponto/services/dia_pago.py:394`, `app/ponto/services/fechamento.py:1064`, `app/folha/export.py:1096`)
   - **Espelho:** seu campo homônimo não copia o número da ata; consulta `realizado_do_dia`. `realizado_h` converte para horas com uma casa decimal; `pago_h` vem do pagamento lavrado. (`app/ponto/services/espelho.py:290`, `app/ponto/services/espelho.py:783`, `app/ponto/services/espelho.py:799`, `app/ponto/services/espelho.py:801`)

   A referência temporal é **dia do turno**, e a unidade original é **minuto de relógio**. As transformações para horas, cumprimento limitado ao previsto e pagamento impedem tratar todos os campos homônimos como equivalentes. (`app/ponto/turnos.py:2930`, `app/ponto/turnos.py:719`, `app/ponto/services/dia_pago.py:382`, `app/ponto/services/espelho.py:801`)

dono: ESTRUTURA app/escala/utils.py:_papeis_do_turno:467

## 09/11 03:04 · sessao 5e96103287be · - · - · origem=Ronald (tmux)

A investigação corrige minha hipótese inicial: os 8 segundos são batidas internas absorvidas neste turno; o piso de par degenerado não decide esta diferença. O defeito está no transporte do intervalo conhecido para os papéis do alinhamento. (`app/ponto/turnos.py:1316`, `app/escala/utils.py:467`)

- **Regra: 660 minutos.** Papel pelo marco, independentemente do tipo gravado (L-087): a luz mais próxima de S21:00 é 21:01; 21:02 fica mais distante, e 20:00 fica a uma hora. A pausa fica 21:01→22:00, 59 minutos; os segmentos são 14:51→21:01 = 370 e 22:00→02:50 = 290, total **660**. A cauda é do turno de 03/09 (L-085); o campo usa HH:MM, truncando segundos. Os horários vêm do anexo fornecido, sem arquivo físico conferido [não verificado]. Bases: `app/docs/LEIS.md:119`, `app/docs/LEIS.md:117`, `app/ponto/turnos.py:584`, `app/ponto/turnos.py:605`, `app/ponto/turnos.py:710`.
- **A pausa declarada deve sobreviver à órfã interposta.** R2b declara: com hii/hfi acesos, o par é a pausa; `_pares_marcados` casa essas duas luzes primeiro, sem exigir adjacência. (`app/ponto/turnos.py:1816`, `app/ponto/turnos.py:811`)
- **Mecanismo exato de 599:** `_papeis_do_turno` recebe esse intervalo, mas exige `_iz == _i + 1`. Como a batida 21:02 está entre 21:01 e 22:00, o par é descartado e vira batidas `sol`. `_alinhar_por_papel` percorre as soltas cronologicamente; 20:00 ocupa S21:00 primeiro, 21:01 perde porque já ocupado e 22:00 ocupa E22:00. A pausa passa a 120 minutos: 719−120 = **599**, diferença −61. (`app/escala/utils.py:455`, `app/escala/utils.py:467`, `app/escala/utils.py:472`, `app/escala/utils.py:564`, `app/escala/utils.py:566`)
- **A ata anterior efetivamente alimenta o turno.** 21:02 vira `X` por estar a ≤15 minutos da luz 21:01; 20:00 passa pela exceção de perna solta próxima do intervalo, também vira `X`; 21:01 e 22:00 viram `Xi`. Assim, o turno chega com a pausa declarada não adjacente, que o transportador depois perde. (`app/escala/orfa_classe.py:28`, `app/ponto/turnos.py:1742`, `app/ponto/turnos.py:1763`, `app/ponto/turnos.py:1787`, `app/ponto/turnos.py:1804`, `app/ponto/turnos.py:1827`)
- **ESTRUTURA é sustentável**, com sítio em `_papeis_do_turno:467` e consequência em `_alinhar_por_papel:566`; o próprio contrato diz que cada par deve sentar inteiro e que soltas nunca movem o par. (`app/escala/utils.py:505`, `app/escala/utils.py:511`) O e6 sozinho apenas dá o resíduo, sem julgar frescor de ata ou versão. (`app/ponto/management/commands/e6_oraculo.py:69`, `app/ponto/management/commands/e6_oraculo.py:98`)
- **CLT e unidade:** o piso de intervalo de 15 minutos é atribuído ao art.71 §1 pela cópia, e o campo declara FATO entre batidas; pagamento, indenização, tolerâncias e janela HE pertencem ao motor. Não aplicar a L-097 para cortar os 9 minutos iniciais deste campo. (`app/ponto/turnos.py:495`, `app/ponto/turnos.py:710`, `app/ponto/turnos.py:717`, `app/escala/utils.py:299`)

## 09/11 03:03 · sessao a52cb32772c9 · - · - · origem=Ronald (tmux)

- **Não há `lavrada_em` nem `regra_versao` na definição da célula examinada:** existem `julgada_em` e `impressao` (`app/escala/models.py:1738-1746`), `veredito_em` (`:1772`), ata JSON (`:1778`) e `insumos_em` (`:1805`). A sonda fornecida imprime esses campos, mas não fornece os dois metadados pedidos; `lavrada_em/regra_versao` específicos desta ata permanecem **[não verificado]**.

- **`julgada_em` não deve ser chamado automaticamente de instante da lavratura da ata:** `carimbar` atualiza-o junto com insumos e impressão (`app/ponto/portas/celula.py:1047-1067`), enquanto `lavrar_veredito` altera ata quando diferente (`:995-996`, `:1027-1033`). No caso fornecido há `julgada_em=11/10` e `veredito_em=02/10`; são campos diferentes e não provam qual commit escreveu originalmente o número **[não verificado]**.

- **Versão declarada atual para setembro é 1**, mesmo com o seletor atual: `app/ponto/services/cartorio.py:132`, `:257-267`; a própria explicação de v2 registra “a 09 fica em regra:1 — FOTO” (`:183-188`). A versão vai para os componentes do hash (`:465-472`), chamado com `versao_para` (`:1143-1146`, `:1348-1353`); hash igual pula sem `forcar` (`:1358-1364`). Logo v1 **não identifica imutavelmente um corpo histórico do seletor**, nem recupera a versão histórica da ata.

- **Commit comprovado documentalmente para a mudança na fonte:** `app/docs/RELATO.md:1657-1660` identifica `b3bf529e`, pouso de **29/10 07:11**, merge da raia `7d028f1c`. A fonte declarada é `_match_marcos` (`:1659`); o detalhamento nomeia `_alinhar_por_papel`, `_papeis_do_turno` e `_papeis_por_alternancia` (`:1709`) e explica a mudança de assentamento pelo papel (`:1710`).

- **`fba8aac8` no anexo:** o título fornecido permite atribuir-lhe o bump `(2, '2026-10')`; atribuir precisamente o trecho seletor e o delta de 61 minutos a esse commit individual requer o diff histórico **[não verificado]**. O documento da cópia confirma o vínculo da v2 com essa obra (`app/ponto/services/cartorio.py:183-188`), mas o pouso nomeado documentalmente é `b3bf529e` (`app/docs/RELATO.md:1657-1660`), ausente do `gitlog.txt` fornecido **[não verificado]**.

- `.git` não existe na raiz da cópia, verificado por leitura do diretório; sem arquivo citável: **[não verificado]**. Portanto não foi possível conferir `git show`, ancestralidade ou diff de `fba8aac8`/`b3bf529e`; para fechar o item histórico faltam os diffs/objetos históricos e o metadado da lavratura deste registro **[não verificado]**.

## 09/11 03:02 · sessao a52cb32772c9 · - · - · origem=Ronald (tmux)

Leitores da **ata** e seus desdobramentos:

- **Diretos:** `app/escala/services/leitor_celula.py:369` usa o campo como evidência de fato no dia; `:406` transporta `int(ata.get('minutos_realizados') or 0)` para a grade, sob a data da célula (`:361`, `:373`). `app/folha/validacao_pdf.py:48` lê diretamente a ata, soma minutos e dias com valor não zero (`:50-52`) e converte para horas (`:54`).
- **Grade → folha:** `app/folha/export.py:329-338` obtém a grade da ata. `app/ponto/services/dia_pago.py:364-402` lê o realizado da grade, limita cada dia ao previsto (`:394`), restringe a trabalho/ausência (`:387-388`) e retorna os mapas por data. `app/ponto/services/fechamento.py:1060-1064` soma esse mapa para `FechamentoMensal.minutos_realizados`. São **minutos de cumprimento com teto por dia**, portanto campo homônimo com significado mais restrito que o relógio da ata (`app/ponto/services/dia_pago.py:367-384`).
- **Percentual e diagnóstico:** `app/folha/export.py:1096-1098` faz cap diário do realizado da grade e converte para horas; `app/folha/causas.py:85-88` verifica realizado zero; `app/ponto/supra_juiz.py:224` lê o realizado para classificação; `app/ponto/services/bordas_realizado.py:87-88` compara ata/grade com o juiz de turnos.
- **Calendário:** `app/colaboradores/services/calendario.py:359` armazena o número vindo da grade da célula; `:718-719` usa-o com a ausência de turno para produzir a palavra “Sem turno pareado”. A regra desse uso está em `app/ponto/services/dia_decidido.py:158-179`.
- **Espelho:** usa a ata para o mesmo diagnóstico `ata_sem_turno` em `app/ponto/services/espelho.py:891-894`. **O campo homônimo `espelho.minutos_realizados` é calculado pelo juiz**, não copiado do número da ata: `:675-676` → `_realizado_do_dia_tela` (`:267-294`) → `realizado_do_dia` → `realizado_dos_turnos` (`app/ponto/turnos.py:774-795`, `:707-771`). Publica o resultado em `app/ponto/services/espelho.py:783`; `realizado_h` divide por 60 e arredonda a uma casa em `:799-800`; o topo soma horas em `:910-912`.
- **Sondas que leem esse homônimo do espelho**, não diretamente o número da ata: `app/ponto/management/commands/e6_oraculo.py:263-268` (fallback para `pago_h × 60`), `app/ponto/management/commands/e6_piso_autopsia.py:117-123`, `app/relatorios/management/commands/vinculo_do_dia_divergentes.py:70-71`, `app/folha/porta_export.py:509-510`.

**Dia e unidade:** `app/ponto/turnos.py:2929-2933` declara explicitamente que o dia da célula/grade/ata é **dia do turno**; o cálculo filtra `t.data_turno == data` (`:719`), mede relógio em minutos (`:740-741`), desconta pausas (`:756-767`) e arredonda para inteiro (`:771`). Nos leitores diretos e da grade acima não aparece recorte do valor à meia-noite; a data usada é a da célula/grade (`app/escala/services/leitor_celula.py:361-406`, `app/ponto/services/dia_pago.py:390-401`).

**Não confundir significados:** ata/juiz = relógio líquido; fechamento/DiaPago homônimo = cumprimento limitado ao previsto por dia; `pago_h` = pagamento lavrado ou motor. O próprio espelho separa realizado e pago (`app/ponto/services/espelho.py:783-810`) e documenta o teto do fechamento (`:905-907`). A correção semântica de todo uso indireto fora dos sítios examinados permanece [não verificado].

## 09/11 02:59 · sessao adbb51c775ba · E1-FILA-V2-2-TRAVA · construtor · origem=Code

B1/B2 implementados:

| Arquivo::função | Alteração |
|---|---|
| [pergunta_no_app.py:175](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/services/pergunta_no_app.py:175)::`perguntar` | Conta chamados recusados em `recusados`, excluindo-os de `chamados`. |
| [perguntar_chamados_sem_pergunta.py:19](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/management/commands/perguntar_chamados_sem_pergunta.py:19)::`Command.handle` | Imprime `colabs`, `chamados` e `recusados` separadamente. |
| [detectar_ausencias.py:802](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/ponto/management/commands/detectar_ausencias.py:802)::`Command._abrir_chamado` | Recusa gera uma linha em **stderr**, com pk e mensagem oficial. |
| [processar_alertas_turno.py:385](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/ponto/management/commands/processar_alertas_turno.py:385)::`Command._abrir_chamado_critico` | Recusa gera uma linha em **stdout**, com pk e mensagem oficial. |
| [processar_alertas_avancados.py:159](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/ponto/management/commands/processar_alertas_avancados.py:159)::`Command.handle` | Recusa gera uma linha em **stdout**, com pk e mensagem oficial. |

Grep dos leitores do dict encontrou como consumidor de produção o comando acima, agora ajustado ([importação:20](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/management/commands/perguntar_chamados_sem_pergunta.py:20), [leitura:29](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/management/commands/perguntar_chamados_sem_pergunta.py:29)):

```bash
rg -n 'pergunta_no_app.*import.*perguntar|pergunta_no_app\.perguntar|\bperguntar\(alvo' --glob '*.py' --glob '!**/migrations/**' .
```

Testes novos em [test_e1_fila_v2_2_consumidores.py:131](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/tests/test_e1_fila_v2_2_consumidores.py:131)::`ConsumidoresRecusaSemBancoTest`. Os seis RED falharam antes da cura; após a cura, passaram ([RED:157](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/tests/test_e1_fila_v2_2_consumidores.py:157)). [nao verificado]

Comandos reproduzíveis:

```bash
export PYTHONDONTWRITEBYTECODE=1
T=chamados.tests.test_e1_fila_v2_2_consumidores

python3 -B -m unittest -v "$T" -k RED

for caso in \
  MORDE_passivo_sem_checagem_da_recusa \
  MORDE_passivo_conta_lotes_em_vez_de_chamados_recusados \
  MORDE_comando_omite_recusados \
  MORDE_ausencias_sem_checagem_da_recusa \
  MORDE_turno_sem_checagem_da_recusa \
  MORDE_avancados_sem_checagem_da_recusa
do
  python3 -B -m unittest -v "$T" -k "$caso"
done

python3 -B -m unittest -q "$T" \
  chamados.tests.test_e1_fila_v2_2_recusa \
  chamados.tests.test_e1_fila_v2_2_trava
```

Resultados: **6 RED verdes; seis mutantes detectados, cada MORDE individual OK; bateria de 40 testes OK** ([MORDE:222](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/tests/test_e1_fila_v2_2_consumidores.py:222)). Os testes executam os trechos reais por AST, sem banco; os crons completos ficam [nao verificado] ([limite do teste:1](/home/ronald/wt-codex-e1-fila-v2-2-trava-015419/app/chamados/tests/test_e1_fila_v2_2_consumidores.py:1)). [nao verificado]

Ruff indisponível no ambiente; lint: [nao verificado].

## 09/11 02:56 · sessao 996a00785e71 · C7-DISPUTA-FECHA-POR-UM-JUIZ · censo · origem=Code

**CENSO**

O núcleo C7 e o escritor de Chamado já estão implementados nos sítios do plano. A falta principal encontrada é **idade ainda absolvendo indiretamente dentro do C6**; também restam apresentações que anunciam “pode fechar” sem consultar toda a resposta do C7. (`app/chamados/models.py:1856`; `app/chamados/juizes.py:674`; `app/chamados/services/explicador.py:137`; `app/chamados/tests/test_chokepoint_chamado_gate.py:152`)

Execução de suíte, impacto atual em produção e correspondência independente da cópia ao HASH informado: **[não verificado]**. [nao verificado]

**leitores:**

| Sítio | Resultado da leitura |
|---|---|
| `app/core/juizes.py:266`, `app/core/juizes.py:267` | C6 e C7 já registrados: `pergunta_viva` e `DisputaSupervisao.pode_fechar`. |
| `app/chamados/models.py:1856` | C7 exige ausência de respostas sem veredito **e** ausência de `fio_vivo`. |
| `app/chamados/models.py:1894`, `app/chamados/models.py:1904` | `.fechar` reaplica a guarda de resposta e consulta C6; fechamento normal recusa pergunta viva. |
| `app/chamados/models.py:3375` | `p.viva` já delega diretamente a `pergunta_viva`; não mantém regra independente. |
| `app/chamados/models.py:3336` | `q_vivas` é pré-filtro deliberadamente amplo; não representa a resposta exata do juiz. |
| `app/chamados/services/materializacao.py:1314` | Contagem de pendentes apenas compõe retorno; quem libera fechamento é `pode_fechar`, em `:1316`. |
| `app/chamados/services/becos.py:29` | Visibilidade, resposta e destino identificam o **recorte de beco**; `FECHAVEL` depende de C7 em `:33`, reaplicado em `:56`. |
| `app/chamados/services/acoes_disputa.py:130`, `app/chamados/services/fio.py:660` | Ainda barram **regularização/materialização** quando existe viva sem resposta. São guardas da ação anterior ao fechamento; a autorização final volta a C7 em `app/chamados/services/materializacao.py:1316`. |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py:83` | Pré-checagem usa somente `fio_vivo`; tenta `.fechar` em `:88` e captura a recusa adicional da porta em `:92`. Não contorna a porta. |

**Falta efetiva: relógio dentro do juiz consumido pelo fechamento.**

- `_dentro_da_janela_viva` usa `date.today()` e a janela corrente/anterior. (`app/chamados/juizes.py:569`, `app/chamados/juizes.py:575`, `app/chamados/juizes.py:601`)
- Em `_pergunta_materializada_de_fato`, pergunta carimbada por via sem testemunha recebe `True` simplesmente por estar **fora da janela**, antes da consulta ao fato. (`app/chamados/juizes.py:660`, `app/chamados/juizes.py:674`, `app/chamados/juizes.py:679`)
- No fallback sem slot, destino encerrado só é confrontado com a falta quando o dia está dentro da janela; fora dela, `_dest_ok` pode resolver a pergunta. (`app/chamados/juizes.py:743`, `app/chamados/juizes.py:764`, `app/chamados/juizes.py:774`)
- C6 restringe a rechecagem da falta à janela e depois consulta esse lastro; portanto o relógio ainda pode retirar vitalidade e liberar C7. Trata-se de caminho estático identificado; quantidade atualmente afetada: **[não verificado]**. (`app/chamados/juizes.py:1217`, `app/chamados/juizes.py:1234`; `app/chamados/models.py:1861`)
- O caso C7 de silêncio testa disputa aberta há 45 dias com pergunta recente **sem carimbo**; não cobre esses dois caminhos carimbados. (`app/chamados/tests/test_c7_red_minimos.py:115`, `app/chamados/tests/test_c7_red_minimos.py:289`, `app/chamados/tests/test_c7_red_minimos.py:299`)

**Apresentação e outros usos, sem autorização de fechamento:**

| Sítio | O que decide |
|---|---|
| `app/chamados/services/explicador.py:137` | Produz `pode_fechar = not fio_vivo(ps)`, sem a guarda adicional de respostas sem veredito. O consumidor transforma isso em orientação “Fechar/regularizar”; há possibilidade de apresentação divergir da porta. (`app/chamados/services/desenlace.py:63`; `app/chamados/models.py:1861`) |
| `app/chamados/services/fio.py:312` | Carrega a explicação para apresentação; pode passar perguntas do chamado, enquanto C7 avalia a disputa inteira. (`app/chamados/models.py:1861`) |
| `app/chamados/relatorios_disputa.py:18` | Classifica `PRONTA_FECHAR` apenas pela ausência de vivas; não consulta a guarda adicional C7. (`app/chamados/relatorios_disputa.py:20`) |
| `app/chamados/services/explicador.py:93` | Lastro explica o estado individual; a decisão agregada está separada em `:137`. |
| `app/chamados/services/fio.py:283`, `app/chamados/services/raiox.py:185` | `.viva` e lastro/cache escolhem disponibilidade de ação na tela. |
| `app/chamados/services/raiox.py:72`, `app/chamados/services/raiox.py:133` | `q_vivas` alimenta crédito/badge. |
| `app/chamados/services/janela_colab.py:63`, `app/chamados/services/cobranca.py:229` | Visibilidade fora da janela e contagem para cobrança/SLA. |
| `app/chamados/services/aviso_pendencia.py:11`, `app/chamados/services/pergunta_no_app.py:47`, `app/chamados/services/vigia_fio.py:748` | Pré-filtro SQL seguido de C6 para aviso, app e vigia, respectivamente. |
| `app/chamados/management/commands/arquivar_competencia_encerrada.py:158` | `p.viva` impede arquivamento de **Chamado** com conversa viva; não fecha disputa. |
| `app/chamados/services/disputa_emissao.py:1781` | Lastro protege pergunta respondida contra carimbo S128; fechamento fica separado em `:1887`. |

A reaplicação estática do detector C7 não encontrou `q_vivas`, lastro ou `.viva` autorizando diretamente fechamento. Esse detector examina `if/while` cujo corpo fecha, com aliases e derivações locais; **não alcança toda dependência indireta**, como a janela acima. (`app/chamados/tests/test_c7_red_minimos.py:475`, `app/chamados/tests/test_c7_red_minimos.py:491`, `app/chamados/tests/test_c7_red_minimos.py:496`)

**escritores:**

O estado aberta/fechada da disputa é `fechada_em`; a propriedade `aberta` deriva desse campo. O único escritor material de **fechamento** encontrado em produção foi `DisputaSupervisao.fechar`, que salva data, autor, resolução e motivo juntos, com trilha. (`app/chamados/models.py:1754`, `app/chamados/models.py:1832`, `app/chamados/models.py:1940`, `app/chamados/models.py:1944`, `app/chamados/models.py:1945`)

Os **15 sítios de chamada direta** encontrados:

| Sítio | Porta e consulta anterior |
|---|---|
| `app/api/foto_recebida.py:80` | `.fechar` normal; valida antes em `:79`; C6 é consultado dentro da porta. |
| `app/chamados/models.py:1958` | `admin_encerra` → `.fechar`, exceção nomeada; exige motivo preenchido em `:1955`. |
| `app/chamados/models.py:2080` | `remover_motivo` → `.fechar` normal quando acabam motivos; C6 fica na porta. |
| `app/colaboradores/services/desligamento.py:142` | `.fechar`, exceção `desligamento`; consulta respostas sem veredito em `:140`. |
| `app/ponto/services/entrada_sincronizada.py:59` | `.fechar` normal; `pode_fechar` em `:58`. |
| `app/chamados/services/becos.py:57` | `.fechar` normal; `pode_fechar` em `:56`. |
| `app/chamados/services/competencia_trancada.py:204` | `.fechar`, exceção `competencia_trancada`; `pode_fechar` em `:202`. |
| `app/chamados/services/disputa_emissao.py:1216` | `.fechar` normal; `pode_fechar` em `:1210`; ausência de ata ainda impede autoencerramento em `:1211`. |
| `app/chamados/services/disputa_emissao.py:1888` | `.fechar` normal; `pode_fechar` em `:1887`; pergunta em voo adia fechamento em `:1885`. |
| `app/chamados/services/materializacao.py:586` | `.fechar` normal; `pode_fechar` em `:585`. |
| `app/chamados/services/materializacao.py:1333` | `.fechar` normal; `pode_fechar` em `:1316`. |
| `app/chamados/reconciliador.py:412` | `.fechar` normal; `pode_fechar` em `:411`. |
| `app/chamados/reconciliador.py:612` | `.fechar` normal; `pode_fechar` em `:611`. |
| `app/chamados/reconciliador.py:1171` | `.fechar` normal; respostas sem veredito em `:1163`, C7 em `:1170`. |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py:88` | `.fechar` normal; `fio_vivo` em `:83`; recusa da porta capturada em `:92`. |

Esses chamadores não calculam idade para fechar; a dependência de relógio identificada está **dentro do C6**, alcançado pela porta. (`app/chamados/juizes.py:674`, `app/chamados/juizes.py:764`, `app/chamados/juizes.py:1234`; `app/chamados/models.py:1904`)

Não foi encontrado `update/bulk_update` de produção fechando disputa fora da porta. As outras atribuições materiais de `fechada_em` encontradas são **reaberturas**, que escrevem `None`. (`app/chamados/models.py:1944`, `app/chamados/models.py:2157`, `app/chamados/models.py:2169`, `app/chamados/models.py:2181`, `app/chamados/models.py:2185`) Escritor dinâmico ou externo à cópia: **[não verificado]**.

Os caminhos indiretos relevantes também chegam às portas:

- Signals de posto/âncora → `remover_motivo` → `.fechar`. (`app/colaboradores/signals.py:77`; `app/escala/signals.py:161`; `app/chamados/models.py:2080`)
- Signal de pergunta → serviço de becos → C7 → `.fechar`. (`app/chamados/signals.py:223`; `app/chamados/services/becos.py:56`)
- Cron de becos está configurado com `--alarme`; o comando só aplica quando `--apply` e sem `--alarme`. (`app/config/crons.py:663`; `app/chamados/management/commands/fechar_becos_disputa.py:30`)
- S143 → resolver perguntas → C7 → `.fechar`. (`app/chamados/signals.py:115`; `app/chamados/services/disputa_emissao.py:1887`)
- Ato de trancar e comando de passivo usam o mesmo serviço de competência trancada. (`app/ponto/services/fechamento.py:1427`; `app/chamados/management/commands/encerrar_por_competencia_trancada.py:29`; `app/chamados/services/competencia_trancada.py:204`)
- Materialização comum e passivo compartilham a cauda de fechamento. (`app/chamados/services/materializacao.py:1378`; `app/chamados/services/passivo_validado.py:405`)
- O signal posterior ao fechamento da disputa reconcilia **Chamado**, não regrava fechamento da disputa. (`app/chamados/signals.py:42`)

**L-111 e desligamento:**

| Item | Situação na cópia |
|---|---|
| `reconciliar_disputa_zumbi` | Command ausente, sem chamada executável encontrada; lápide em `app/config/crons.py:835`; teste de ausência em `app/chamados/tests/test_c7_red_minimos.py:57`. |
| `heal_disputas_legado` | Command ausente, sem chamada executável encontrada; lápide em `app/config/crons.py:1444`; teste de ausência em `app/chamados/tests/test_c7_red_minimos.py:61`. |
| `veredito_do_explicador` | Definição e chamadas ausentes; lápide em `app/chamados/juizes.py:2115`; teste de ausência em `app/chamados/tests/test_c7_red_minimos.py:51`. |
| `disputa_fecha_na_cascata_de_desligamento` | Definição e chamadas ausentes; lápide em `app/chamados/juizes.py:2116`; teste de ausência em `app/chamados/tests/test_c7_red_minimos.py:54`. |
| Expiração por silêncio de 30 dias | Bloco removido, com lápide; teste cobra ausência das funções e da opção antiga. (`app/chamados/management/commands/arquivar_competencia_encerrada.py:61`; `app/chamados/tests/test_c7_red_minimos.py:307`) |
| `.update` do desligamento | **Já saiu**. Hoje há guarda em `app/colaboradores/services/desligamento.py:140` e `.fechar(..., motivo=FECHOU_DESLIGAMENTO)` em `:142`. |

Os testes remanescentes dos quatro apagados verificam ausência; não os invocam. Há escritas cruas de estado legado em fixtures, separadas do inventário de produção. (`app/chamados/tests/test_c7_red_minimos.py:51`; `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:141`; `app/chamados/tests/test_signals.py:268`)

**Escritor de Chamado — cinco mecânicos:**

| Origem | Escrita atual |
|---|---|
| `app/ponto/management/commands/reconciliar_grade.py:454` | `retirar_contexto`. |
| `app/ponto/management/commands/reconciliar_grade.py:473` | `renovar_contexto` e `retirar_contexto` em `:474`. |
| `app/ponto/management/commands/gerar_celulas.py:255` | `renovar_contexto`. |
| `app/chamados/management/commands/nomear_marco_por_lampada.py:109` | `renovar_contexto`; evento condicionado à escrita em `:111`. |
| `app/ponto/views.py:288` | `renovar_contexto`, preservando o restante do contexto. |

A persistência desses atos fica nas portas de contexto; **nenhum dos cinco sítios continua salvando cru**. (`app/chamados/services/contexto.py:104`, `app/chamados/services/contexto.py:127`)

**Escritor de Chamado — seis operações/sítios:**

| Origem | Campo | Porta atual |
|---|---|---|
| `app/chamados/services/cobranca.py:178` | `prazo_sla` | `definir_prazo_sla`; persistência em `app/chamados/models.py:591`. |
| `app/chamados/services/disputa_emissao.py:1642` | `prazo_sla` | Mesma porta, `app/chamados/models.py:573`. |
| `app/ponto/management/commands/escalonar_chamados_supervisao.py:103` | `escalonado_para_dp`, instante | `marcar_escalonado_dp`; persistência em `app/chamados/models.py:610`. |
| `app/chamados/services/abertura.py:115` | Nome real: `ticket_id_glpi` | `apontar_ticket_glpi`; persistência em `app/chamados/services/vinculos.py:117`. |
| `app/chamados/services/limbo_folgas.py:69` | `descricao` | `reescrever_texto`; persistência em `app/chamados/services/texto.py:36`. |
| `app/api/views.py:3160` | `titulo`, `assunto` | `reescrever_texto`, definida em `app/chamados/services/texto.py:21`. |

São seis **sítios**, com SLA em dois deles; todos já passam por porta. A enumeração está também no selo E1. (`app/chamados/tests/test_porta_chamado_e1.py:132`)

A matriz já declara `teste=` e `idempotencia=`. O censo AST atribuível retornou zero escritores de Chamado fora dos módulos declarados como porta; seu alcance exclui testes/migrations e admite escritas não atribuíveis. (`app/core/contratos_estruturais.py:497`, `app/core/contratos_estruturais.py:500`; `app/core/portas.py:196`; `app/core/censo_escritas.py:43`, `app/core/censo_escritas.py:54`)

Há documentação envelhecida: a nota que ainda acusa `.update` no desligamento e a que lista quatro vizinhos fora de porta não descrevem mais essas chamadas atuais. (`app/core/contratos_estruturais.py:730`, `app/core/contratos_estruturais.py:1015`; `app/colaboradores/services/desligamento.py:142`; `app/chamados/tests/test_chokepoint_chamado_gate.py:152`, `app/chamados/tests/test_chokepoint_chamado_gate.py:161`)

**DECISÕES:**

1. **Encaixe da retirada de idade:** corrigir os ramos temporais dentro do C6 existente, preservando C7 como consumidor; escolher a organização dos auxiliares e cobrir os caminhos carimbados e sem slot. A lei já proíbe absolvê-los por relógio; não exige juiz novo. (`app/docs/BACKLOG.md:1466`; `app/chamados/juizes.py:674`, `app/chamados/juizes.py:764`)

2. **Unificar a resposta apresentada:** decidir como explicador e relatório consumirão `d.pode_fechar()` e como receberão o conjunto completo da disputa. Hoje seus booleanos usam só C6, enquanto C7 acrescenta respostas sem veredito. (`app/chamados/services/explicador.py:137`; `app/chamados/services/fio.py:312`; `app/chamados/relatorios_disputa.py:20`; `app/chamados/models.py:1861`)

3. **Trava e releitura da porta:** definir o encaixe da avaliação sob trava. Atualmente respostas e vivas são lidas antes do `atomic` e da tomada de travas; possível divergência concorrente é inferência estática, reprodução **[não verificado]**. (`app/chamados/models.py:1894`, `app/chamados/models.py:1903`, `app/chamados/models.py:1913`, `app/chamados/models.py:1918`)

4. **Tela de encerramento:** falta oferecer a porta existente por uma ação fina com motivo obrigatório; o backlog a mantém como obra própria `C7-TELA-ADMIN-ENCERRA`. Nome da rota, botão e apresentação são desenho; a regra já está na porta. (`app/docs/BACKLOG.md:1469`; `app/chamados/models.py:1951`, `app/chamados/models.py:1955`)

5. **Não reconstruir E1/E2 do escritor de Chamado:** os onze sítios pedidos já migraram; resta reconciliar a descrição antiga do backlog/matriz com o código e manter os selos existentes. (`app/docs/BACKLOG.md:1466`; `app/chamados/tests/test_chokepoint_chamado_gate.py:152`, `app/chamados/tests/test_chokepoint_chamado_gate.py:161`; `app/core/contratos_estruturais.py:497`)

6. **Sobreposição documental com obras em curso:**

   | Arquivo | Sobreposição a coordenar |
   |---|---|
   | `chamados/services/disputa_emissao.py` | Autoencerramento/S128 do censo e fila `RETRATADA-CONVERSA-E-REABRIR`; a E4 também prevê reduzir escritores de estado da pergunta. (`app/chamados/services/disputa_emissao.py:1210`, `:1887`; `app/docs/BACKLOG.md:782`, `:1859`) |
   | `chamados/services/fio.py` | Explicação/regularização e migração dos leitores para `tarefa_do_dia` na E3, que absorve FIO-MUDO-B. (`app/chamados/services/fio.py:312`, `:660`; `app/docs/BACKLOG.md:1807`, `:1858`) |
   | `chamados/models.py` | Porta C7 já contém mudanças de trava/decisão da E1; E4 prevê encolher portas de estado da pergunta. (`app/chamados/models.py:1914`, `:1930`; `app/docs/BACKLOG.md:1859`) |
   | `chamados/juizes.py` | A retirada de idade toca C6, também nomeado pela obra E2 e pelo passivo O261-b. (`app/chamados/juizes.py:674`; `app/docs/BACKLOG.md:1857`, `:2237`) |

   Conflitos efetivos de merge entre branches/worktrees: **[não verificado]**.

As faltas identificadas acima são de implementação, apresentação e encaixe; a lei fornecida já determina juiz único, exceções por porta e proibição da absolvência por idade. Não identifiquei decisão normativa adicional necessária para essas correções. (`app/docs/BACKLOG.md:1466`)

perguntas de lei: 0

## 09/11 02:54 · sessao f4a9775a4f46 · - · - · origem=Ronald (tmux)

Censo por AST de 3.459 arquivos `.py` da cópia, sem falhas de parse; incluídos arquivos fora de `app`. Não há definição nem chamada AST dos quatro nomes L-111, inclusive `call_command('nome')`; referências restantes são lápides, documentação e testes de ausência (`app/chamados/juizes.py:2115`; `app/config/crons.py:835`; `app/config/crons.py:1444`; `app/chamados/tests/test_c7_red_minimos.py:45`). Execução de produção, Git/HEAD real e banco: [não verificado].

**Escritor material da disputa**

- `DisputaSupervisao.fechar`: atribui `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento` e salva somente esses campos; consulta respostas sem veredito e `pergunta_viva`, grava trilha; `app/chamados/models.py:1894`, `:1904`, `:1940`, `:1944`, `:1945`.
- `admin_encerra` exige motivo preenchido e delega à porta; `app/chamados/models.py:1951`, `:1955`, `:1958`.
- Reaberturas escrevem **abertura**, não fechamento: `reabrir_admin` zera campos e salva; `app/chamados/models.py:2157`, `:2169`. `reabrir_sistema` idem; `:2181`, `:2185`.
- Nenhum `update`/`bulk_update` de `fechada_em` de produção encontrado na varredura; contrato explícito e tripwire em `app/core/portas.py:424` e `app/chamados/tests/test_c7_red_minimos.py:574`. Limite: AST estático não prova inexistência de escritor dinâmico externo [não verificado].
- `DisputaSupervisao` usa `fechada_em` como estado aberta/fechada (`app/chamados/models.py:1754`, `:1832`); `status_local` refere-se a Chamado, não a esse campo da disputa (`app/chamados/signals.py:33`).

**Todos os 15 chamadores diretos de `.fechar` de disputa**

| Local | Consulta anterior / seleção | Porta |
|---|---|---|
| `app/api/foto_recebida.py:80` | Último motivo foto; valida respostas em `:79`; sem pré-`pode_fechar` | Normal; própria porta consulta C6 |
| `app/chamados/models.py:1958` | Motivo obrigatório `:1955` | `admin_encerra` |
| `app/chamados/models.py:2080` | Sem motivos restantes `:2074`; sem pré-`pode_fechar` | Normal; própria porta consulta C6 |
| `app/colaboradores/services/desligamento.py:142` | `respostas_sem_veredito` em `:140` | Exceção `desligamento` |
| `app/ponto/services/entrada_sincronizada.py:59` | `pode_fechar` em `:58`; seleção de pai com um único órfão `:54` | Normal |
| `app/chamados/services/becos.py:57` | `pode_fechar` em `:56` | Normal |
| `app/chamados/services/competencia_trancada.py:204` | `pode_fechar` em `:202` | Exceção `competencia_trancada` |
| `app/chamados/services/disputa_emissao.py:1216` | `pode_fechar` em `:1210`; sem ata impede fecho em `:1211` | Normal |
| `app/chamados/services/disputa_emissao.py:1888` | `pode_fechar` em `:1887`; pergunta em voo impede em `:1885` | Normal |
| `app/chamados/services/materializacao.py:586` | `pode_fechar` em `:585` | Normal |
| `app/chamados/services/materializacao.py:1333` | `pode_fechar` em `:1316`; contagem `perguntas_vivas_de` em `:1314` só compõe retorno | Normal |
| `app/chamados/reconciliador.py:412` | `pode_fechar` em `:411` | Normal |
| `app/chamados/reconciliador.py:612` | `pode_fechar` em `:611` | Normal |
| `app/chamados/reconciliador.py:1171` | Respostas sem veredito em `:1163`; `pode_fechar` em `:1170` | Normal |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py:88` | `fio_vivo` em `:83`; porta recusa respostas e comando captura `ValueError` em `:92` | Normal |

Nenhum desses **chamadores diretos** calcula idade para executar fechamento; porém o juiz consultado mantém dependência indireta de relógio: `app/chamados/juizes.py:674`–`:675`, `:764`, alcançados por C6 em `:1234` e pela porta em `app/chamados/models.py:1904`. Portanto não afirmar “idade totalmente eliminada”.

**Desligamento**

O update direto saiu: há comentário explícito em `app/colaboradores/services/desligamento.py:133`, guarda de respostas em `:140`, chamada `.fechar(... motivo=FECHOU_DESLIGAMENTO)` em `:142`. O teste busca especificamente `update(fechada_em=...)` em `app/chamados/tests/test_c7_red_minimos.py:34`.

**Signals, crons e commands indiretos**

- `app/escala/signals.py:161` e `app/colaboradores/signals.py:77` chamam `remover_motivo`, que fecha pela porta em `app/chamados/models.py:2080`; a seleção “acabaram motivos” continua em `:2074`, mas a porta valida C6.
- Evento de Pergunta chama fechamento de beco em `app/chamados/signals.py:223`–`:225` → `app/chamados/services/becos.py:74`, `:78` → `.fechar` em `:57`; classificação própria de beco usa visibilidade/resposta/status do destino em `:29`–`:33`, e decisão final consulta `pode_fechar`.
- Command beco aplica via serviço em `app/chamados/management/commands/fechar_becos_disputa.py:57`; sua agenda usa **`--alarme`**, que não aplica (`app/config/crons.py:663`; `app/chamados/management/commands/fechar_becos_disputa.py:30`).
- S143 chama resolver ao salvar status terminal de Chamado (`app/chamados/signals.py:104`, `:115`) → wrapper em `app/chamados/services/disputa_emissao.py:1706`, `:1722` → fecho em `:1888`.
- O post-save de disputa fecha **Chamado**, não disputa, por `ch.reconciliar()` em `app/chamados/signals.py:42`; os demais receivers de disputa retomam regularizações e materialização (`:134`, `:163`).
- Tranca dispara pelo ato em `app/ponto/services/fechamento.py:1427`; comando de passivo chama o mesmo serviço em `app/chamados/management/commands/encerrar_por_competencia_trancada.py:29`; está fora de rotina em `app/config/crons.py:1268`.
- `encerrar_perguntas_com_fato` é passivo declarado fora de rotina em `app/config/crons.py:1329`; escreve disputa exclusivamente pela chamada da tabela.
- Materialização comum e passivo compartilham `_fechar_disputa_se_quitada`: `app/chamados/services/materializacao.py:1378`; `app/chamados/services/passivo_validado.py:405`.
- Reconciliador de colaborador alcança fechamento por célula e órfãs em `app/chamados/reconciliador.py:1365`, `:1375`; lavra sem turno em `:1615`.
- Command `app/ponto/management/commands/fechar_chamados_meio_periodo.py:109` chama `.fechar` em **ChamadoColaborador** (universo em `:49`, `:55`), portanto não entra nos 15 de disputa.

**L-111**

- `veredito_do_explicador` e `disputa_fecha_na_cascata_de_desligamento`: apagados; lápide em `app/chamados/juizes.py:2115`–`:2119`; testes verificam ausência de definição em `app/chamados/tests/test_c7_red_minimos.py:51`, `:54`.
- `reconciliar_disputa_zumbi`: arquivo ausente; lápide de registro em `app/config/crons.py:835`; teste verifica ausência de arquivo em `app/chamados/tests/test_c7_red_minimos.py:57`.
- `heal_disputas_legado`: arquivo ausente; lápide em `app/config/crons.py:1444`; teste verifica ausência em `app/chamados/tests/test_c7_red_minimos.py:61`.
- Nenhum chamador executável produção/teste desses quatro nomes; testes remanescentes testam ausência, não invocam portas apagadas (`app/chamados/tests/test_c7_red_minimos.py:51`–`:63`). Documentação histórica ainda cita implementação antiga, por exemplo `app/docs/TICKETS.md:1070` e `app/chamados/tests/test_porta_disputa_ciclo.py:51`; não tratar prosa antiga como código vivo.
- Bloco de expiração de 30 dias foi apagado com lápide em `app/chamados/management/commands/arquivar_competencia_encerrada.py:61`–`:64`. Ainda há prosa antiga no cabeçalho, explicitamente marcada revogada em `:38`.

**Fixtures/migrations**

Migrations apenas declaram campos/choices (`app/chamados/migrations/0013_disputasupervisao.py:24`; `app/chamados/migrations/0049_disputa_motivo_fechamento.py:15`). Há escritores crus em **testes** que montam estado legado, por exemplo `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:141`, `:168`; `app/chamados/tests/test_reabrir_orfas_enterradas.py:81`; `app/chamados/tests/test_selo_universo_reconciliador.py:104`; `app/chamados/tests/test_signals.py:268`. Eles não são escritores de produção; nenhum teste foi executado [não verificado].

## 09/11 02:53 · sessao f5e8149ba135 · E1-FILA-V2-2-TRAVA · verificador · origem=Code

**Não daria o aval completo: A1 continua parcial.** As duas telas distinguem a recusa, mas quatro chamadores descartam o retorno e o passivo ainda conta chamados recusados como “APLICADO”. (`app/chamados/services/acoes_disputa.py:225`; `app/chamados/services/cobrar_dia.py:50`; `app/chamados/services/pergunta_no_app.py:186`; `app/chamados/management/commands/perguntar_chamados_sem_pergunta.py:29`)

1. **A1 — recusa falante e contagem**

   | Chamador | Comportamento diante de `REPASSE_RECUSADO` |
   |---|---|
   | `acoes_disputa` | Testa identidade antes de testar falsidade ou acessar `.pk`; devolve aviso explícito. `app/chamados/services/acoes_disputa.py:225`, `app/chamados/services/acoes_disputa.py:229` |
   | `cobrar_dia` | Testa identidade e devolve `ok=False`; a view apresenta aviso. `app/chamados/services/cobrar_dia.py:50`; `app/chamados/views_cobrar.py:326` |
   | `pergunta_no_app` | Descarta retorno, soma `len(chs)` e devolve os totais; o comando imprime **APLICADO**, inclusive para repasses recusados. `app/chamados/services/pergunta_no_app.py:186`; `app/chamados/services/pergunta_no_app.py:187`; `app/chamados/management/commands/perguntar_chamados_sem_pergunta.py:29` |
   | `detectar_ausencias` | Descarta retorno. O catch genérico não anuncia uma recusa que agora chegou como retorno normal. `app/ponto/management/commands/detectar_ausencias.py:801`; `app/ponto/management/commands/detectar_ausencias.py:802` |
   | `processar_alertas_turno` | Descarta retorno e continua devolvendo o chamado. `app/ponto/management/commands/processar_alertas_turno.py:384`; `app/ponto/management/commands/processar_alertas_turno.py:387` |
   | `processar_alertas_avancados` | Descarta retorno e continua. “Chamado criado” corresponde ao Ch criado antes; esse contador não demonstra repasse concluído. `app/ponto/management/commands/processar_alertas_avancados.py:121`; `app/ponto/management/commands/processar_alertas_avancados.py:158`; `app/ponto/management/commands/processar_alertas_avancados.py:162` |

   **Nenhum desses seis usa atualmente o objeto truthy como Disputa:** dois fazem comparação por identidade; quatro ignoram o resultado. A contagem enganosa concreta está no passivo. As referências de cada consumidor estão na tabela acima.

2. **Atomic externo, estado parcial e evento**

   O `except` está fora **somente do atomic próprio do repasse**. Existe caminho dentro de transação externa: `_gravar_estado` abre atomic, trava e salva Ch; o signal chama cartório, que usa o emissor de ausências e pode alcançar o repasse. (`app/chamados/services/disputa_emissao.py:1049`; `app/chamados/services/disputa_emissao.py:1290`; `app/chamados/models.py:280`; `app/chamados/models.py:290`; `app/chamados/models.py:296`; `app/chamados/signals.py:203`; `app/ponto/services/cartorio.py:1964`; `app/ponto/services/cartorio.py:1990`; `app/ponto/management/commands/detectar_ausencias.py:801`)

   **Sim: o repasse pode ser desfeito e o trabalho anterior do chamador permanecer.** Isso é inclusive exigido pelo teste do marcador anterior. Em `cobrar_dia`, toque e julgamento precedem a chamada recusada; portanto “nada feito” descreve o repasse, não todo o pedido. (`app/chamados/tests/test_e1_fila_v2_2_integracao.py:238`; `app/chamados/tests/test_e1_fila_v2_2_integracao.py:265`; `app/chamados/services/cobrar_dia.py:39`; `app/chamados/services/cobrar_dia.py:41`; `app/chamados/services/cobrar_dia.py:49`; `app/chamados/services/disputa_emissao.py:1021`)

   **O evento não sobrevive ao rollback externo.** Seu gravador abre atomic na mesma conexão, sem persistência independente. Além disso, é best-effort: pode retornar `None`, enquanto o agrupador devolve o sentinel sem conferir a gravação. Logo, `REPASSE_RECUSADO` **não garante evento persistido**. (`app/core/observ.py:103`; `app/core/observ.py:105`; `app/core/observ.py:133`; `app/core/observ.py:139`; `app/chamados/services/disputa_emissao.py:1293`; `app/chamados/services/disputa_emissao.py:1296`)

   Não encontrei segundo produtor nominal de `repasse_recusado`: a porta produz o evento e delega a persistência à autoridade genérica existente. (`app/chamados/services/disputa_emissao.py:1293`; `app/core/observ.py:134`)

3. **A2 e mutantes da volta 2**

   **A2 ganhou mordida no código do teste:** o novo cenário chama a porta real sem wrapper da helper nem cauda injetada, exige uma Disputa, exige C estrangeiro antes da primeira D e verifica a ordem. O MORDE injeta exatamente a lambda local antes do `with`. **Passagem efetiva no Postgres desta volta: [nao verificado].** (`app/chamados/tests/test_e1_fila_v2_2_integracao.py:190`; `app/chamados/tests/test_e1_fila_v2_2_integracao.py:199`; `app/chamados/tests/test_e1_fila_v2_2_integracao.py:202`; `app/chamados/tests/test_e1_fila_v2_2_integracao.py:204`; `app/chamados/tests/test_e1_fila_v2_2_integracao.py:211`)

   Para as remoções pedidas, há testes que rejeitam: ausência do catch, retorno `None`, ausência do evento e remoção das duas checagens das telas. **Não identifiquei sobrevivente dessas remoções diretas** nos oráculos examinados; isso não prova a suíte completa da volta 2 verde. (`app/chamados/tests/test_e1_fila_v2_2_recusa.py:164`; `app/chamados/tests/test_e1_fila_v2_2_recusa.py:173`; `app/chamados/tests/test_e1_fila_v2_2_recusa.py:179`; `app/chamados/tests/test_e1_fila_v2_2_recusa.py:188`)

   A lacuna permanece nos quatro consumidores ignorados: os testes de recusa exercitam agrupador, iniciar e cobrar, não exigem correção da contagem do passivo nem anúncio nos três crons. (`app/chamados/tests/test_e1_fila_v2_2_recusa.py:126`; `app/chamados/tests/test_e1_fila_v2_2_integracao.py:244`; `app/chamados/services/pergunta_no_app.py:187`)

4. **S1 / S2 / S3**

   - **S1:** a fixture move a P materializada para `alvo`, preservando `ch2` sem P; conserva o assert de chegada à cauda e o de reabertura da D. A causa anterior foi tratada sem afrouxar esses asserts; alcançar a cauda no Postgres permanece **[nao verificado]**. (`app/chamados/tests/test_e1_fila_v2_2_integracao.py:79`; `app/chamados/tests/test_e1_fila_v2_2_integracao.py:118`; `app/chamados/tests/test_e1_fila_v2_2_integracao.py:120`; `app/chamados/tests/test_e1_fila_v2_2_integracao.py:127`; `app/chamados/tests/test_e1_fila_v2_2_integracao.py:129`)
   - **S2:** B023 foi tratado na origem, capturando variáveis pelos argumentos default das closures. Ruff completo nesta revisão: **[nao verificado]**. (`app/chamados/tests/test_e1_fila_v2_2_trava.py:254`; `app/chamados/tests/test_e1_fila_v2_2_trava.py:270`; `app/chamados/tests/test_e1_fila_v2_2_trava.py:367`)
   - **S3:** **1196 é a linha real**, `data_entrada = _tz.localtime(batida_origem.timestamp).date()`. O registro corresponde ao sítio B1 e o selo compara as linhas reais por igualdade. (`app/chamados/services/disputa_emissao.py:1196`; `app/core/tests/test_medida_turno_do_par.py:268`; `app/core/tests/test_medida_turno_do_par.py:533`; `app/core/medida_turno_do_par.py:225`)

5. **Classes históricas de reprovação**

   **Segundo juiz / derivação paralela / relógio novo:** não identifiquei nas funções acrescentadas. O fecho percorre relações; o mérito do fechamento continua em `pode_fechar()`. (`app/chamados/services/disputa_emissao.py:957`; `app/chamados/services/disputa_emissao.py:959`; `app/chamados/services/disputa_emissao.py:974`; `app/chamados/services/disputa_emissao.py:1275`)

   **Ordem interna:** a helper pede C ordenados, depois D/Ch/P pela autoridade existente, e recusa crescimento. As pré-travas da porta externa continuam existindo; não foram curadas aqui. (`app/chamados/services/disputa_emissao.py:1001`; `app/chamados/services/disputa_emissao.py:1007`; `app/chamados/services/disputa_emissao.py:1012`; `app/chamados/models.py:2674`; `app/chamados/models.py:2683`; `app/chamados/models.py:2696`; `app/chamados/models.py:2700`; `app/chamados/models.py:290`)

   **Terminal reatável:** está incluído pela semente `VIVOS + ENCERRADOS`; HIST também procura `ENCERRADOS`. A exclusão de cancelado/superado dessa semente não demonstra quebra desse aval. (`app/chamados/services/disputa_emissao.py:960`; `app/chamados/reconciliador.py:1582`; `app/chamados/catalogo/motor.py:30`; `app/chamados/catalogo/motor.py:31`)

   **Retorno com três significados:** está documentado e separado por identidade nas telas, mas não consumido integralmente. Atenção adicional: `None` pode ocorrer **depois de complementar perguntas e gravar D/Ch**; não equivale a “zero escrita”. (`app/chamados/services/disputa_emissao.py:1029`; `app/chamados/services/disputa_emissao.py:1078`; `app/chamados/services/disputa_emissao.py:1081`; `app/chamados/services/disputa_emissao.py:1091`)

6. **Quem escreve e quem lê**

   | Objeto | Escritor | Leitores |
   |---|---|---|
   | Retorno | `None`: `app/chamados/services/disputa_emissao.py:1046`, `app/chamados/services/disputa_emissao.py:1091`; D: `app/chamados/services/disputa_emissao.py:1285`, `app/chamados/services/disputa_emissao.py:1288`; sentinel: `app/chamados/services/disputa_emissao.py:1296` | Identidade: `app/chamados/services/acoes_disputa.py:225`, `app/chamados/services/cobrar_dia.py:50`; quatro descartadores identificados no item 1. |
   | Evento `repasse_recusado` | Produção: `app/chamados/services/disputa_emissao.py:1293`; persistência: `app/core/observ.py:134` | Leitor específico encontrado: teste em `app/chamados/tests/test_e1_fila_v2_2_integracao.py:262`. Leitor genérico de produção: auditoria em `app/core/views.py:705`, com filtro livre de ação em `app/core/views.py:710`. |

   O histórico dos chamados não inclui esse evento de modelo `Colaborador`, pois filtra `Chamado`/`ChamadoColaborador`. Consumidor operacional específico, retry e visibilidade efetiva em produção: **[nao verificado]**. (`app/chamados/services/historico.py:86`; `app/chamados/services/disputa_emissao.py:1293`)

   Executei os **23 testes sem banco: OK**, usando `python3 -B`; relatório persistido dessa execução: **[nao verificado]**. Postgres e suíte completa da volta 2: **[nao verificado]**. (`app/chamados/tests/test_e1_fila_v2_2_trava.py:1`; `app/chamados/tests/test_e1_fila_v2_2_recusa.py:1`)

## 09/11 02:53 · sessao 5149a4a62a93 · - · - · origem=Ronald (tmux)

Censo de leitores por AST/leitura, sem alteração de arquivos:

- **Juiz C7 existente:** `app/chamados/models.py:1856` → `pode_fechar()` exige ausência de `respostas_sem_veredito()` e ausência de `fio_vivo`; `app/chamados/models.py:1834` define a primeira lista; `app/chamados/juizes.py:1253` define `fio_vivo` como `any(pergunta_viva(p))`. A porta reaplica a guarda de resposta e o C6 em `app/chamados/models.py:1894` e `app/chamados/models.py:1904`.
- **Registro vivo:** autoridades C6/C7 declaradas em `app/core/juizes.py:266` e `app/core/juizes.py:267`.
- **`p.viva` já não é cópia:** retorna diretamente `pergunta_viva(self)` em `app/chamados/models.py:3375`; `q_vivas` declara ser pré-filtro amplo, não valor exato, em `app/chamados/models.py:3336`.

**Leitores que efetivamente liberam fechamento e já consultam C7:**

| Local | Decisão |
|---|---|
| `app/chamados/services/disputa_emissao.py:1210` | Autoencerramento após emissão usa `pode_fechar`; ausência de ata ainda impede autoencerramento em `:1211`. |
| `app/chamados/services/disputa_emissao.py:1887` | S128 usa `pode_fechar`; pergunta em materialização impede reentrada em `:1885`. |
| `app/chamados/services/materializacao.py:585` | Ausência declarada: `pode_fechar`. |
| `app/chamados/services/materializacao.py:1316` | Quitação granular: `pode_fechar`. O contador `pendentes` de `:1314` apenas compõe retorno, não decide fechamento. |
| `app/chamados/reconciliador.py:411` | Reconciliação por pergunta: `pode_fechar`. |
| `app/chamados/reconciliador.py:611` | Retratação por célula: `pode_fechar`. |
| `app/chamados/reconciliador.py:1163` | Respostas sem veredito são relatadas; autorização final por `pode_fechar` em `:1170`. |
| `app/chamados/services/becos.py:33` | Classificação `FECHAVEL` vem de C7; `:56` reaplica antes da porta. O recorte `vis` de `:29` identifica beco, não libera fechamento. |
| `app/chamados/services/competencia_trancada.py:153` | Medição usa `pode_fechar`; aplicação reaplica em `:202` e chama porta com motivo em `:204`, sem carimbar perguntas. |
| `app/ponto/services/entrada_sincronizada.py:58` | Disputa existente usa `pode_fechar`. |
| `app/api/foto_recebida.py:79` | Valida antes e chama diretamente `.fechar` em `:80`; o juiz fica na porta. O recorte por motivos de `:70` escolhe aparar ou tentar fechar, não permite bypass. |

**Resíduos de decisão/apresentação a distinguir:**

- `app/chamados/services/acoes_disputa.py:130` e `app/chamados/services/fio.py:660`: recusam a ação de **regularização/materialização** se alguma pergunta viva está sem resposta. São guardas anteriores à materialização, não autorização de fechamento; fechamento real volta a C7 em `app/chamados/services/materializacao.py:1316`.
- `app/chamados/services/explicador.py:137`: rótulo `pode_fechar` usa apenas `not fio_vivo(ps)`, sem a guarda `respostas_sem_veredito` de C7. Seu consumidor de produção é apresentação: `app/chamados/services/fio.py:312`; `app/chamados/services/desenlace.py:63` transforma o booleano em texto “Fechar/regularizar”.
- `app/chamados/relatorios_disputa.py:18`: declara `PRONTA_FECHAR` quando não há vivas, também sem consultar a guarda adicional C7 (`:20`); é relatório, não escritor (`coletar` em `:26`).
- `app/chamados/services/explicador.py:93`: lastro continua usado para explicar estado individual; a decisão agregada já usa C6 em `:137`.
- `app/chamados/services/fio.py:283`: `.viva` + lastro/cache só escolhe presença de ação na tela.
- `app/chamados/services/raiox.py:72` e `:133`: `q_vivas` + veredito persistido constroem crédito/badge; não fecham disputa. Lastro direto é fallback de botão em `:185`.
- `app/chamados/services/janela_colab.py:63`: `q_vivas` amplia visibilidade de chamados fora da janela; não fecha disputa.
- `app/chamados/services/cobranca.py:229`: `q_vivas` alimenta contagem de pessoas caladas/SLA; não fecha disputa.
- `app/chamados/services/aviso_pendencia.py:11` e `:13`: pré-filtro SQL seguido de C6, só aviso.
- `app/chamados/services/pergunta_no_app.py:47` e `:53`: pré-filtro SQL seguido de C6, lista do app.
- `app/chamados/services/vigia_fio.py:748` e `:755`: pré-filtro SQL seguido de C6, conta vivas em disputa fechada.
- `app/chamados/management/commands/arquivar_competencia_encerrada.py:158`: `any(p.viva)` impede arquivamento de **chamado** com conversa viva, sem fechamento da disputa; expiração por silêncio foi removida, com lápide em `:61`.
- `app/ponto/services/veredito_celula.py:180`: `q_vivas` seleciona perguntas para a decisão humana do dia; carimbo pela porta da pergunta em `:189`, reconciliação de chamados em `:202`; não escreve fechamento da disputa diretamente.
- `app/chamados/services/disputa_emissao.py:1781`: lastro protege pergunta respondida contra o carimbo S128; autorização de fechamento está separada, em C7 (`:1887`).

**Idade ainda subsiste indiretamente no juiz C6 — lacuna relevante:**

1. `app/chamados/juizes.py:569` / `:575` / `:601`: `_dentro_da_janela_viva` depende de `date.today()` e competências corrente/anterior.
2. `app/chamados/juizes.py:660`: pergunta com via sem testemunha e carimbo chega ao retorno **True por estar fora da janela**, em `:674`–`:675`, dispensando a rechecagem do fato que existe em `:679`.
3. `app/chamados/juizes.py:764`: fallback de slot ausente só contradiz destino encerrado com o fato quando dentro da janela; fora dela aceita `_dest_ok` em `:774`.
4. `app/chamados/juizes.py:1217`: C6 só reconsulta falta dentro da janela; depois consulta esse lastro em `:1234`. Assim, há caminho de vitalidade/fechamento dependente do relógio mesmo com todos os chamadores passando por C7. Quantos registros de produção entram nele: **[não verificado]**.
5. O caso de silêncio em `app/chamados/tests/test_c7_red_minimos.py:289` usa pergunta muda criada pelo helper sem carimbo (`:115`); não cobre esses caminhos carimbados.

**Resultado AST específico:** reapliquei em memória os auxiliares de `app/chamados/tests/test_c7_red_minimos.py:452`–`:500` aos arquivos Python de `app`, excluindo testes/migrações/dependências: zero achados para `q_vivas`/lastro/`.viva` autorizando fechamento e zero por `disputa_so_de_falta`/`MOTIVOS_SO_DE_FALTA`. A abrangência é a do detector: `if`/`while` cujo corpo fecha (`:496`), inclusive aliases e derivações locais (`:485`–`:494`); não demonstra ausência de regras indiretas como a janela acima.

## 09/11 02:53 · sessao c85e4c4944fc · - · - · origem=Ronald (tmux)

Item 5 está implementado nos sítios nomeados pelo plano: E1 cobre seis sítios e E2 cobre os cinco mecânicos; as listas dos testes confirmam esse recorte. `app/chamados/tests/test_porta_chamado_e1.py:132`; `app/chamados/tests/test_porta_chamado_e2.py:80`.

| Sítio | Escrita atual | Porta |
|---|---|---|
| `app/chamados/services/cobranca.py:178` | `prazo_sla` do lote | `ChamadoColaborador.definir_prazo_sla`, persistência em `app/chamados/models.py:591` |
| `app/chamados/services/disputa_emissao.py:1642` | `prazo_sla` da recobrança | mesma porta, `app/chamados/models.py:573` |
| `app/ponto/management/commands/escalonar_chamados_supervisao.py:103` | `escalonado_para_dp` e instante | `marcar_escalonado_dp`, persistência em `app/chamados/models.py:610` |
| `app/chamados/services/abertura.py:115` | campo real `ticket_id_glpi` | `apontar_ticket_glpi`, persistência em `app/chamados/services/vinculos.py:117` |
| `app/chamados/services/limbo_folgas.py:69` | `descricao` | `reescrever_texto`, persistência em `app/chamados/services/texto.py:36` |
| `app/api/views.py:3160` | `titulo` e `assunto` | mesma porta, `app/chamados/services/texto.py:21` |

São **seis sítios**, contendo cinco nomes de campo do aval; `prazo_sla` aparece em dois sítios e o sítio do título também escreve `assunto`. `app/chamados/tests/test_porta_chamado_e1.py:1`; `app/chamados/tests/test_porta_chamado_e1.py:132`; `app/api/views.py:3160`.

| Cinco mecânicos | Escrita atual |
|---|---|
| `app/ponto/management/commands/reconciliar_grade.py:454` | `retirar_contexto(ch, 'orfa_regeneracao')` |
| `app/ponto/management/commands/reconciliar_grade.py:473` | `renovar_contexto` seguido de `retirar_contexto` em atomic, na linha 474 |
| `app/ponto/management/commands/gerar_celulas.py:255` | `renovar_contexto` |
| `app/chamados/management/commands/nomear_marco_por_lampada.py:109` | `renovar_contexto`; evento somente quando a porta escreveu, linha 111 |
| `app/ponto/views.py:288` | `renovar_contexto`, fundindo as chaves informadas |

As persistências do contexto ficam em `app/chamados/services/contexto.py:104` e `app/chamados/services/contexto.py:127`; o nascimento permanece tratado separadamente pela porta de abertura. `app/chamados/services/contexto.py:7`.

O `delete` vizinho também migrou: `app/ponto/management/commands/zerar_dados_operacionais.py:84` chama `ChamadoColaborador.apagar_em_ensaio`; a porta exige banco de ensaio antes de apagar. `app/chamados/models.py:563`; `app/chamados/models.py:569`.

A matriz já declara:

- `teste='chamados.tests.test_chokepoint_chamado_gate.ChokepointChamadoGate'`. `app/core/contratos_estruturais.py:497`.
- `idempotencia='chamados.tests.test_porta_chamado_e3.IdempotenciaDaFamiliaChamado'`. `app/core/contratos_estruturais.py:500`.
- `verde=True`. `app/core/contratos_estruturais.py:501`.

A frase “QUATRO vizinhos fora” em `app/core/contratos_estruturais.py:1015` é relato histórico da C1 e está desatualizada perante as chamadas efetivas acima; o próprio comentário posterior do gate registra E1/E2 e teto zero. `app/chamados/tests/test_chokepoint_chamado_gate.py:152`; `app/chamados/tests/test_chokepoint_chamado_gate.py:161`; `app/chamados/tests/test_chokepoint_chamado_gate.py:313`.

Executei somente o varredor AST puro, com bytecode desabilitado: retornou **16 escritas atribuíveis de ChamadoColaborador, todas em módulos declarados como porta; zero fora**. Esse resultado tem o alcance do varredor, que exclui testes/migrations e mantém balde de não atribuíveis; não prova ausência de escritores que ele não reconheça. `app/core/censo_escritas.py:43`; `app/core/censo_escritas.py:54`; `app/core/censo_escritas.py:469`; `app/core/portas.py:196`.

A leitura AST complementar das atribuições e `save/update` dos campos nomeados não encontrou escrita crua adicional de `ChamadoColaborador` fora dessas portas; os demais `descricao` encontrados pertencem a outras entidades. [não verificado por teste de execução] [nao verificado]

Os testes E1 possuem casos de idempotência para SLA, DP, GLPI e texto, e E2 possui caso de retirada ausente sem query; não executei testes nem consultei produção. `app/chamados/tests/test_porta_chamado_e1.py:57`; `app/chamados/tests/test_porta_chamado_e1.py:81`; `app/chamados/tests/test_porta_chamado_e1.py:94`; `app/chamados/tests/test_porta_chamado_e1.py:117`; `app/chamados/tests/test_porta_chamado_e2.py:45`.

## 09/11 02:52 · sessao 9e4f32c36ea4 · - · - · origem=Ronald (tmux)

- **A2 recebeu mordida por porta real:** `_agrupador_com_C_estrangeiro` chama o agrupador sem wrapper da helper nem cauda injetada, exige Disputa real, C estrangeiro antes da primeira D e ordem sem violações. O MORDE injeta exatamente a lambda local antes do `with` e exige falha por ausência do C estrangeiro. (`app/chamados/tests/test_e1_fila_v2_2_integracao.py:190`, `:199`, `:202`, `:204`, `:206`, `:211`) **Execução PostgreSQL: [nao verificado]**; não há Django/freezegun instalado neste ambiente **[nao verificado]**.

- **Os quatro mutantes da recusa têm oráculo que os rejeita:** identidade do retorno e mensagem nas telas, evento único depois do atomic e nenhuma D/P nova. Há MORDE para catch, evento, `None` e leitores das duas telas. (`app/chamados/tests/test_e1_fila_v2_2_recusa.py:126`, `:140`, `:164`, `:173`, `:179`, `:188`) Rodei os 23 testes sem banco de `trava` e `recusa`, com `python3 -B`: **23/23 OK**; resultado não persistido **[nao verificado]**.

- **S1: fixture corrigida sem afrouxar assert.** P materializada passa para `alvo`, preservando `ch2` sem P; ressuscitação usa `alvo`; permanecem assert de chegada à cauda e de D reaberta. Isso remove a causa identificada de vacuidade, mas passagem no Postgres da volta 2 segue **[nao verificado]**. (`app/chamados/tests/test_e1_fila_v2_2_integracao.py:118`, `:120`, `:121`, `:127`, `:129`, `:79`; origem de `ch2` sem P: `app/chamados/tests/test_e1_fila.py:260`)

- **S2: B023 curado na origem**, capturando variáveis pelos defaults das closures; configuração continua selecionando B e não ignora B023. Ruff completo: **[nao verificado]**. (`app/chamados/tests/test_e1_fila_v2_2_trava.py:254`, `:270`, `:367`; `app/ruff.toml:5`, `:6`)

- **S3: 1196 corresponde ao sítio real congelado**, `data_entrada = _tz.localtime(batida_origem.timestamp).date()`. O censo marca `.date()` de operando rastreado como B1; o selo compara todas as linhas registradas por igualdade. (`app/core/tests/test_medida_turno_do_par.py:268`, `:533`; `app/chamados/services/disputa_emissao.py:1196`; `app/core/medida_turno_do_par.py:217`, `:225`) Rodei esse teste isolado em memória, substituindo somente `SimpleTestCase` por `unittest.TestCase`: **OK**; resultado não persistido **[nao verificado]**.

- **O teste de savepoint não demonstra durabilidade diante do rollback externo.** Injeta recusa na helper antes das escritas do corpo; confirma marcador anterior ainda presente dentro de atomic externo e conjuntos D/P iguais. Não força rollback externo nem consulta o evento depois dele. (`app/chamados/tests/test_e1_fila_v2_2_integracao.py:238`, `:241`, `:262`, `:265`, `:266`; primeira escrita do corpo: `app/chamados/services/disputa_emissao.py:1081`) O nível zero exigido pelo teste sem banco refere-se apenas à transação simulada. (`app/chamados/tests/test_e1_fila_v2_2_recusa.py:68`, `:146`)

_3001 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
