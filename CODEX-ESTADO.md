# CODEX-ESTADO

> Ultima resposta processada: **09/11/2026 07:42** · publicado em **09/11/2026 07:42** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 09/11 07:42 · sessao 693ce8822a20 · SELOS-CANON-I1 · censo · origem=Code

**A v5.q não enunciava os dois sentidos da pertença.** Explicitava o achado de igualdade com célula crua; “nas duas vias (agente e wrapper)” designava percursos de execução. Ainda assim, perguntava pela semântica de pertença, incluindo “id cru x id com sufixo”. Portanto, classifico o achado 1 como **(a), pelo tema solicitado**, sem afirmar que o espelho tivesse sido expressamente formulado. (`i1_preverif_v5.q:3–10`)

As referências aos anexos abaixo contam as linhas do texto colado, incluindo linhas vazias, começando pela primeira linha de cada anexo.

| achado | classe | trecho q | trecho r | linha da triagem |
|---|---|---|---|---|
| **1 — espelho: token da célula dentro da obra** | **(a)**: pertença e diferença entre ID cru/qualificado estavam perguntadas; o espelho passou despercebido. O código procura somente a obra dentro da célula em `nomeiam`. (`bin/disjuntor.py:1683–1689`) | “Os achados 1 e 4 do laudo estao curados em TODOS os sitios”; “nas duas vias (agente e wrapper)?”; “id cru x id com sufixo”. (`i1_preverif_v5.q:5,10`) | “**Achado 1, nos dois pontos alterados: curado.**”; “Pedir `O9` diante dessa última célula resulta em NAO SEI”. Examina obra `O9`/célula `O9 (C)`, enquanto o laudo 2 aponta obra `O9 (C)`/célula `O9`. (`i1_preverif_v5.r:5–6`; `i1_opus_final_2.md:2`) | **L2:** “CURADO nas duas vias [...] -- conferido no diff.” Foi acolhimento da conclusão de cura, **não descarte de um espelho identificado**. (`i1_triagem_v5.txt:2`) |
| **2 — minúsculas no wrapper** | **(b)**: incompatibilidade vista e descartada. (`i1_preverif_v5.r:34`; `i1_triagem_v5.txt:6`) | “Algum leitor supoe significado diferente”. (`i1_preverif_v5.q:9`) | “O wrapper shell aceita somente `[A-Z0-9-]+`; o CLI aceita também minúsculas.” (`i1_preverif_v5.r:34`) | **L6:** “wrapper [A-Z0-9-] x CLI minusculas [...] anteriores, fora do diff. FILA.” (`i1_triagem_v5.txt:6`) |
| **2 — O67b/O51b também rejeitados pelo Agent** | **(a), parcialmente visto**: a resposta identificou wrapper/CLI, mas não diagnosticou o impedimento nas duas vias para esses IDs. A validação do Agent exige a gramática sensível à caixa. (`i1_preverif_v5.r:34`; `bin/disjuntor.py:157,616–620`; `app/docs/BACKLOG.md:1325,1327`) | “nas duas vias (agente e wrapper)?” (`i1_preverif_v5.q:5`) | O trecho pertinente limita-se a “O wrapper shell aceita somente [...]”; não apresenta o diagnóstico específico do Agent para O67b/O51b. (`i1_preverif_v5.r:34`) | **L6** descarta wrapper/CLI; não registra descarte específico desse impacto no Agent. (`i1_triagem_v5.txt:6`) |
| **2 — FECHADA sem negrito** | **(e)**: reconhecimento das formas de fechamento não foi perguntado especificamente. O caso e a restrição do reconhecedor constam da cópia. (`i1_preverif_v5.q:1–16`; `app/docs/BACKLOG.md:768`; `bin/hook_stop_fila1.py:59,168`) | O pedido próximo é sobre funções “que o diff cria ou muda”; não nomeia `_FECHADO` nem `FECHADA:`. (`i1_preverif_v5.q:7–10`) | Não há diagnóstico desse caso; a resposta descreve a delegação a `_itens_andaveis`. (`i1_preverif_v5.r:19`) | Sem entrada específica. (`i1_triagem_v5.txt:1–8`) |
| **3 — célula nua com cauda nunca abre** | **(b), com ressalva**: a impossibilidade de aprovação foi vista; a triagem justificou o descarte pelo efeito na medida. (`i1_preverif_v5.r:53`; `i1_triagem_v5.txt:3`) | “negativa de obra fora da forma nunca fecha intervalo em _censo_24h”; “Quem e afetado hoje?” (`i1_preverif_v5.q:13–14`) | “não torna possível aprovar a mesma identidade fora da forma”; “isso não equivale a vaga aberta”. (`i1_preverif_v5.r:53`) | **L3:** “nao fecha intervalo de obra qualificada -> FILA (pre-existente, so medida [...]). Nao bloqueia.” Essa justificativa não trata expressamente a indisponibilidade da abertura. (`i1_triagem_v5.txt:3`; `bin/disjuntor.py:1704–1707`) |
| **4 — PARADA excluída e tratada como FORA** | **(a)**: o universo produzido por `_fila1_viva` estava perguntado; a resposta descreveu a coleta sem apontar essa consequência. (`i1_preverif_v5.q:7`; `i1_preverif_v5.r:19`) | “QUEM ESCREVE e QUEM LE [...] (_casa_id_inteiro, _fila1_viva, _token_da_celula”. (`i1_preverif_v5.q:7`) | “Lê BACKLOG, delega a `_itens_andaveis` e devolve **lista de células cruas**, preservando duplicidades”. (`i1_preverif_v5.r:19`) | Sem descarte correspondente. A exclusão ocorre em `_itens_andaveis`; a ausência conduz a FORA. (`i1_triagem_v5.txt:1–8`; `bin/hook_stop_fila1.py:250–252`; `bin/disjuntor.py:1637,1685–1686,2269–2272`) |
| **5 — descartes confirmados não bloqueantes** | **(b)**: o próprio achado confirma os descartes, sem apresentar nova omissão. (`i1_opus_final_2.md:6`) | “sao regressao desta volta ou estado anterior? Quem e afetado hoje?” (`i1_preverif_v5.q:14`) | “o defeito já existia”; “Quem é afetado é o consumidor da medida”. (`i1_preverif_v5.r:53`) | **L3–6:** motivos de anterioridade, efeito na medida e casos fora do diff; **L7:** lacunas encaminhadas ao opus. (`i1_triagem_v5.txt:3–7`) |
| **6 — papel leitura aceito como prova** | **(a)**: a semântica da prova estava perguntada; a resposta mapeou consumidores sem apontar essa aceitação. O conjunto admite `censo` e `leitura`. (`i1_preverif_v5.q:8–10`; `bin/hook_stop_fila1.py:726,763`) | “prova_parei_censo, _eh_faixa_censo”; “Algum leitor supoe significado diferente”. (`i1_preverif_v5.q:8–9`) | “Produz `None`, prova `''` ou motivo → Stop, alarme e handoff.” (`i1_preverif_v5.r:27`) | Sem entrada específica. (`i1_triagem_v5.txt:1–8`) |
| **6 — retirada do PAREI; `_cru_abre_agent_copia`; `~/wt-*`** | **(e)** para esses temas específicos: ciclo de retirada, guarda de importação e forma abreviada do caminho não foram formulados. (`i1_preverif_v5.q:1–16`) | O trecho próximo pergunta quem “escreve o literal do PAREI fora do ponto unico”; não pergunta quando retirá-lo. (`i1_preverif_v5.q:6`) | “A escrita material do PAREI continua passando por `no_topo`”; não diagnostica retirada nem os dois reconhecedores de cópia. (`i1_preverif_v5.r:30`; `bin/disjuntor.py:65–84,153`) | Sem entrada específica. O laudo os chama de “herdados opus 1”, portanto não sustentam **(d)** por si mesmos. (`i1_triagem_v5.txt:1–8`; `i1_opus_final_2.md:7`) |
| **6 — teor de `CORTES.md:428`** | **[não verificado]**: não há conteúdo suficiente para classificar a obrigação desse corte. A cópia termina na linha 425. (`app/docs/CORTES.md:425`) | “Leis dos sitios em anexo”, mas o conteúdo desse anexo não foi fornecido. (`i1_preverif_v5.q:2`) | Não apresenta o teor desse corte. (`i1_preverif_v5.r:1–70`) | Sem entrada específica. (`i1_triagem_v5.txt:1–8`) |

**(c) aplica-se à comprovação operacional que faltava**, como execução integral/RED e eventos reais de produção; não à possibilidade de examinar estaticamente o espelho. A resposta declarou essas limitações, enquanto o código permite identificar a comparação unilateral. (`i1_preverif_v5.r:46,54,68`; `bin/disjuntor.py:1684`) Atribuir **(d), modo comum**, aos casos restantes exigiria evidência adicional sobre quem os examinou: **[não verificado]**.

## 09/11 07:41 · sessao e9a3685d39c6 · - · - · origem=Ronald (tmux)

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 09/11 07:40 · sessao 44256442ce4b · - · - · origem=Ronald (tmux)

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 09/11 07:40 · sessao 9d8baeed0bd3 · - · - · origem=Ronald (tmux)

Auditoria estática concluída; execução dos selos nesta sessão e estado vivo dos privilégios: [nao verificado].

| Selo | O que o código cobra | Lacuna / efeito do contrato |
|---|---|---|
| `test_codex_sonda.sh` | Usa envelope e Codex falsos; injeta nomes/CPF e verifica raspagem; falha do envelope/lista vazia bloqueia Codex com **rc 4**; cobra chamada padrão a `sonda_leitura.sh`, ausência de `psql/manage.py/credencial/docker` no wrapper e sandbox `read-only`; confere trilha obra/papel/hash/rc. (`bin/tests/test_codex_sonda.sh:21`, `:28`, `:46`, `:57`, `:71`, `:79`, `:93`) | **Não executa o envelope real nem observa SQL**: injeta `CODEX_SONDA_ENVELOPE` nas chamadas. (`bin/tests/test_codex_sonda.sh:49`) Contrato rc5 do envelope conflita com rc externo se exigir propagação: o selo hoje exige rc4 do wrapper. (`bin/tests/test_codex_sonda.sh:73`) |
| `test_sondas_rollback.sh` | AST encontra `atomic`, exige último comando `raise _Rollback()`, `exec` dentro dele e referência a `_canario_existe` depois; mutante retira o `raise`; exige `sondas.sh` referir `sonda_frota.sh` e `sondas_rollback.py`, sem `docker run` próprio. (`bin/tests/test_sondas_rollback.sh:15`, `:23`, `:28`, `:31`, `:37`, `:49`, `:63`) | **É prova de forma AST, não execução/efeito no PostgreSQL**: lê e analisa o fonte. (`bin/tests/test_sondas_rollback.sh:42`) O envelope real faz `CREATE TEMP TABLE` antes de executar a sonda: contradiz contrato global “nenhum CREATE”. (`bin/sondas_rollback.py:49`) Tirar canário sem substituir desenho deixa teste vermelho por ausência da conferência; trocar roteamento de `sondas.sh` deixa vermelho por exigência de frota. (`bin/tests/test_sondas_rollback.sh:38`, `:69`) |
| `test_ensaio_fora_do_postgres_de_prod.sh` | Consulta `pg_database` de `saas_db`; cobra ausência de `sombra/juliani_ref_*`; busca `dropdb/createdb/pg_restore` em shell de `bin/` apontando para prod; cobra host literal `sombra_db`; pares sintéticos mordem os detectores. (`bin/tests/test_ensaio_fora_do_postgres_de_prod.sh:38`, `:58`, `:65`, `:79`, `:94`) | O detector de escrita só procura esses três utilitários; não fiscaliza GRANT, ALTER ROLE, CREATE TEMP ou Python da sonda. (`bin/tests/test_ensaio_fora_do_postgres_de_prod.sh:60`) A consulta de catálogo real impede executar esse selo sem banco, e ele cria fixtures temporárias. (`bin/tests/test_ensaio_fora_do_postgres_de_prod.sh:38`, `:99`) |
| `test_recursos_sourced.sh` | Cobra source imune aos argumentos, proíbe suite em `saas_core`, exige cpuset nas suites; para `sombra_db`, exige cpuset/restart/memória no nascimento/update e pertencimento a `TESTE_CONTAINERS`. (`bin/tests/test_recursos_sourced.sh:18`, `:31`, `:38`, `:88`) | Não pergunta por SQL nem role leitor; inspeciona recursos/forma dos scripts. (`bin/tests/test_recursos_sourced.sh:94`) |
| `test_cpuset_de_teste.sh` | Procura `--cpuset-cpus` à mão e confere containers vivos cujo comando contém `manage.py test`; pares sintéticos verificam detector. (`bin/tests/test_cpuset_de_teste.sh:26`, `:41`, `:55`) | A medição viva filtra **suite**, não sondas: não mede isolamento real do container de `sonda_leitura.sh`. (`bin/tests/test_cpuset_de_teste.sh:44`) |
| `test_codex_pergunta.sh` | Com Codex falso e repositórios temporários, cobra sandbox, quatro papéis leitores, guardas do construtor e mutantes; não é prova de PostgreSQL. (`bin/tests/test_codex_pergunta.sh:2`, `:24`, `:73`, `:86`) | Alterações no envelope não são exercitadas por esse cenário; ele substitui apenas o Codex e testa o wrapper. (`bin/tests/test_codex_pergunta.sh:75`, `:86`) |
| `prova_codex_pergunta_real.sh` | No modo leitor, pede criações na cópia/viva/tmp e compara hash/status/ausência de arquivos. (`bin/tests/prova_codex_pergunta_real.sh:174`, `:180`, `:184`) | Não é prova de role/SQL. O selo de sonda só o invoca com `CODEX_SELO_REAL=1`. (`bin/tests/test_codex_sonda.sh:107`) |

O próprio `sonda_leitura.sh` documenta que `test_sonda_leitura.sh` **ainda não pousou** e que a proteção restante era documental mais RED em log; esse log não foi consultado. (`bin/sonda_leitura.sh:16`; estado do RED: [nao verificado]).

Os selos de host são chamados pelo loop da régua; falha interrompe antes da suite. Isso não prova que tenham sido executados nem que estejam verdes nesta árvore. (`bin/regua.sh:138`; resultados: [nao verificado]). O pre-push documenta que esse loop não roda ali. (`bin/pre-push.sh:13`).

Pontos essenciais para a consolidação do selo novo:

- O canário de rollback já é DDL executado pelo envelope Python, independentemente do SQL enviado a `psql`; vem antes do `exec` da sonda. (`bin/sondas_rollback.py:50`)
- O contador de tentativas soma apenas `n_tup_ins + n_tup_upd + n_tup_del`; não conta comandos DDL/DCL nem exige que tentativas sejam zero para emitir `OK`. (`bin/sondas_rollback.py:35`, `:72`)
- `sondas.sh` aceita qualquer linha `SONDA_ROLLBACK|OK|*` para mover a sonda a “feitas”, inclusive contador de tentativas positivo. (`bin/sondas.sh:38`)
- A sonda Python é arbitrária e recebe execução direta; um docker falso que só registre `psql` não observa SQL emitido por Django/driver dentro desse processo. (`bin/sonda_leitura.sh:239`, `bin/sondas_rollback.py:53`)
- O próprio código reconhece que `SET default_transaction_read_only=off` desfaz o parâmetro de sessão; não basta provar default `on`. (`bin/sonda_leitura.sh:13`)

Proposta ainda não implementada [nao verificado]: selo do envelope real, em raiz descartável, com Docker falso que registre SQL por stdin/`-c`/`-f` e recuse operações desconhecidas; matriz prod/sombra, grants prontos/faltantes, role ausente, login divergente, erros antes/depois do Python, chamada direta e via wrapper. Deve verificar controle positivo de consulta, nenhum provisionamento, rc5 nomeando falta, trilha coerente e mutante que reinsere `_garantir_grants`. O contador precisa representar a observação feita; campo literal `sql_escrita=0` não acrescenta evidência [nao verificado].

Proposta adicional, não provada na cópia [nao verificado]: separar selo host do envelope de prova de capacidades efetivas em cluster descartável equivalente, incluindo CREATE em `public/juliani`, CREATE TEMP, DML, sequência, GRANT/ALTER, memberships/SET ROLE, propriedade, privilégios herdados de PUBLIC e funções SECURITY DEFINER. Um grep de verbos em chamadas `psql` não demonstra recusa a SQL originado no Python arbitrário, cujo ponto de execução está em `bin/sonda_leitura.sh:240`.

## 09/11 07:39 · sessao 55358bb0102a · - · - · origem=Ronald (tmux)

Para o **espelho específico**, recomendo **(e)**: a pergunta não escreve a condição “token da célula dentro da obra”. Ela contextualiza “pertenca a fila 1 por igualdade com celula crua” e pede cura “nas duas vias (agente e wrapper)”; essas duas vias são percursos de execução, não os dois sentidos da relação. (`i1_preverif_v5.q:3-6`, numeração do anexo colado)

A pergunta aborda o tema geral: “id cru x id com sufixo” e mordida de `casos_pertenca`. Isso permite discutir **(a)** numa classificação ampla do tema pertença, mas não sustenta afirmar que a pergunta exigiu explicitamente os dois sentidos. (`i1_preverif_v5.q:7-10,15`)

A resposta demonstra apenas o sentido inverso: “Pedir `O9` diante dessa última célula resulta em NAO SEI”. A triagem declara “CURADO nas duas vias”; não relata ter visto e descartado o espelho. Portanto **(b)** não cabe para esse defeito. (`i1_preverif_v5.r`, §1, segundo item; linha absoluta [não verificado]; `i1_triagem_v5.txt:2`)

## 09/11 07:39 · sessao a46b11001e13 · - · - · origem=Ronald (tmux)

| Achado | Classe sugerida | Trecho literal q | Trecho literal r | Triagem/motivo |
|---|---|---|---|---|
| 5 — descartes confirmados | **(b)** | “sao regressao desta volta ou estado anterior? Quem e afetado hoje?” (`i1_preverif_v5.q`, item 4) | “O defeito já existia”; “Quem é afetado é o consumidor da medida” (`i1_preverif_v5.r`, seção 4) | Linha 3: “FILA (pre-existente, so medida [...] Nao bloqueia”. Linhas 4–6: demais limitações anteriores. Linha 7: lacunas encaminhadas ao opus. |
| 6 — PAREI/faixa e papel leitura | **(a), com ressalva de escopo** | “prova_parei_censo, _eh_faixa_censo”; “Algum leitor supoe significado diferente” (`i1_preverif_v5.q`, item 2) | “Classifica a linha literal válida”; “Produz `None`, prova `''` ou motivo” (`i1_preverif_v5.r`, tabela seção 2) | Ausente da triagem. A pergunta trouxe as funções e sua semântica; a resposta mapeou leitores, mas não apontou esses resíduos. O ciclo de retirada do PAREI não foi expressamente pedido. |
| 6 — `_cru_abre_agent_copia` e `COPIA_RE`/`~/wt-*` | **(e)** para os temas específicos | Não menciona essas funções nem caminhos `~/wt-*`; pedido geral sobre achados 1/4 e funções alteradas (`i1_preverif_v5.q`, itens 1–2). | Não os discute (`i1_preverif_v5.r`, seções 1–5). | Ausentes da triagem. Não cabe afirmar “nenhum viu”: o próprio laudo os apresenta como “herdados opus 1” (`i1_opus_final_2.md`, achado 6). |
| 6 — `CORTES.md:428` | **Conteúdo não classificável: [não verificado]** | A pergunta menciona “Leis dos sitios em anexo”, mas esse anexo não foi fornecido (`i1_preverif_v5.q`, linha inicial de anexos). | Sem discussão desse corte (`i1_preverif_v5.r`). | Ausente; o teor da referência não está no dado fornecido. |

Evidência do código:

- Papel `leitura` conta como prova: `bin/hook_stop_fila1.py:726`; o reconhecimento usa o conjunto em `bin/hook_stop_fila1.py:763`.
- A faixa preserva linhas de censo: `bin/hook_stop_fila1.py:481`; o escritor adiciona/reconhece a linha sem apresentar remoção nesse percurso: `bin/disjuntor.py:301–326`. Uma linha provada libera: `bin/hook_stop_fila1.py:804–806`.
- `_cru_abre_agent_copia` decide pela presença textual de `wt-`: `bin/disjuntor.py:82–84`; `COPIA_RE` exige caminho com barras antes de `wt-`: `bin/disjuntor.py:153`, usado na classificação do Agent em `bin/disjuntor.py:1740` e `bin/disjuntor.py:1844`.
- A cópia de `app/docs/CORTES.md` termina na linha 425; o conteúdo citado como `:428` permanece **[não verificado]** (`app/docs/CORTES.md:425`).

## 09/11 07:39 · sessao c019cdd8aca8 · - · - · origem=Ronald (tmux)

Sugestão de classificação por subtema:

| Achado | Classe | Trecho literal q | Trecho literal r | Triagem / ressalva |
|---|---|---|---|---|
| 2 — wrapper/minúsculas | **b** | “Algum leitor supoe significado diferente” (`i1_preverif_v5.q:9`) | “O wrapper shell aceita somente `[A-Z0-9-]+`; o CLI aceita também minúsculas.” | `i1_triagem_v5.txt:6`: “wrapper [A-Z0-9-] x CLI minusculas [...] anteriores, fora do diff. FILA.” Código: `bin/codex_pergunta.sh:188`; `bin/disjuntor.py:162`. |
| 2 — impacto específico O67b/O51b e rejeição Agent | **a**, como extensão não diagnosticada da incompatibilidade já identificada | “nas duas vias (agente e wrapper)” (`i1_preverif_v5.q:5`) | A resposta menciona wrapper/CLI, sem nomear O67b/O51b ou diagnosticar a rejeição Agent. (`i1_preverif_v5.r`, linha exata [não verificado]) | Não há descarte específico do Agent. Código: `bin/disjuntor.py:616`; IDs: `app/docs/BACKLOG.md:1325`, `app/docs/BACKLOG.md:1327`. |
| 2 — FECHADA sem negrito | **e**, se “tema presente” exigir pergunta específica sobre fechamento | Não há pergunta específica sobre formas de FECHADA. (`i1_preverif_v5.q:1`, demais linhas [não verificado]) | Não há diagnóstico desse caso. [não verificado] | Sem descarte específico. Caso em `app/docs/BACKLOG.md:768`; regex em `bin/hook_stop_fila1.py:59`; aplicação em `bin/hook_stop_fila1.py:168`. |
| 3 — identidade fora da forma nunca aprova | **b**, com ressalva sobre o motivo da triagem | “negativa de obra fora da forma nunca fecha intervalo em _censo_24h” (`i1_preverif_v5.q:13`) | “a volta corrige a leitura da negativa, mas não torna possível aprovar a mesma identidade fora da forma” | `i1_triagem_v5.txt:3` descarta o residual como “pre-existente, so medida”. Essa linha não analisa expressamente a indisponibilidade de abertura; a recusa está em `bin/disjuntor.py:1704`. |
| 4 — PARADA excluída antes da pertença | **a** | “QUEM ESCREVE e QUEM LE cada funcao/constante [...] (_casa_id_inteiro, _fila1_viva, _token_da_celula” (`i1_preverif_v5.q:7`) | “Lê BACKLOG, delega a `_itens_andaveis` e devolve **lista de células cruas**, preservando duplicidades” | Sem descarte correspondente. A resposta descreve a coleta, mas não diagnostica PARADA→FORA: `bin/disjuntor.py:1637`; `bin/hook_stop_fila1.py:250`; `bin/disjuntor.py:2269`. |

A ausência atual de célula viva do achado 3 é informação do laudo fornecido, não conclusão obtida por acesso a produção (`i1_opus_final_2.md:4`).

## 09/11 06:43 · sessao 41bbd4564418 · DEPLOY-EXCLUSAO-ENVELOPE · verificador · origem=Code

**Parecer: não daria VERDE nesta volta.** O fluxo de sucesso fecha as duas janelas para envelopes desta versão, mas encontrei autoespera no `vigia_arvore`, execução auxiliar sem compartilhada, retenção sem teto global e lacunas nos leitores e no selo. (`bin/deploy.sh:316`, `bin/deploy.sh:442`, `app/core/esteira_vigia.py:1267`, `bin/cron_run.sh:50`, `app/core/management/commands/pipeline_placar.py:74`, `bin/tests/test_deploy_exclusao_cron.sh:168`)

Validação por execução, duração real dos deploys e comportamento observado em produção: **[nao verificado]**. [nao verificado]

**1. Fechamento das janelas, saídas e morte do deploy** [nao verificado]

No fluxo de sucesso, **sim, para quem passa pelo envelope novo e disputa o mesmo arquivo**: o único apply está depois da aquisição exclusiva; o único restart também, com a última consulta de marcas já dentro dela. O envelope adquire compartilhada antes de criar MARCA ou lançar `CMD`; o unlock explícito só vem depois das três provas de rota. (`bin/deploy.sh:319`, `bin/deploy.sh:337`, `bin/deploy.sh:363`, `bin/deploy.sh:364`, `bin/deploy.sh:437`, `bin/deploy.sh:442`, `bin/cron_run.sh:17`, `bin/cron_run.sh:22`, `bin/cron_run.sh:24`)

Não localizei ramo que aplique migration ou execute restart sem passar pela exclusiva. HUP sem migration fica deliberadamente fora dela; `--conferir` termina antes desses atos. (`bin/deploy.sh:237`, `bin/deploy.sh:316`, `bin/deploy.sh:336`, `bin/deploy.sh:359`, `bin/deploy.sh:381`)

**A promessa “segura até reload conferido” vale somente no sucesso.** `falhou()` executa `exit 1`; pode encerrar após falha do migrate, do HUP, da troca de workers ou das rotas, sem chegar ao unlock normal. Não há mecanismo nesses caminhos que mantenha a exclusão até uma recuperação posterior. (`bin/deploy.sh:92`, `bin/deploy.sh:337`, `bin/deploy.sh:381`, `bin/deploy.sh:394`, `bin/deploy.sh:440`, `bin/deploy.sh:442`)

O arquivo permanece; a posse está no fd 4. A saída fecha o descritor do processo, mas **não garante liberação imediata se algum descendente ainda o conservar**: a biblioteca declara herança, e o lançamento do comando não fecha o fd. Descendente efetivamente sobrevivendo à morte de um deploy nesta produção: **[nao verificado]**. (`bin/trava_deploy_exclusao.sh:10`, `bin/trava_deploy_exclusao.sh:32`, `bin/trava_deploy_exclusao.sh:42`, `bin/cron_run.sh:24`)

A transição de versão também merece distinção: o restart relê marcas para acomodar envelope antigo já em memória; o migrate normal não consulta essas marcas. Portanto a exclusiva, sozinha, não demonstra proteção contra um cron antigo que ainda não participa do protocolo. Existência desse cron durante a publicação: **[nao verificado]**. (`bin/deploy.sh:108`, `bin/deploy.sh:315`, `bin/deploy.sh:337`, `bin/deploy.sh:362`, `bin/deploy.sh:363`)

**2. Envelope, MARCA/FIM e rc 75**

O `CMD` só roda no ramo armado após aquisição compartilhada bem-sucedida. MARCA fica nesse ramo; FIM só é criado para rc 0 ou 2. Sem vez, o envelope grava placar com rc 75, não cria MARCA/FIM e sai pelo epílogo. (`bin/cron_run.sh:17`, `bin/cron_run.sh:20`, `bin/cron_run.sh:22`, `bin/cron_run.sh:24`, `bin/cron_run.sh:27`, `bin/cron_run.sh:30`, `bin/cron_run.sh:39`, `bin/cron_run.sh:55`)

**Há uma exceção à afirmação ampla “nenhum comando entra no core”:** sem compartilhada, o epílogo ainda tenta executar `docker exec ... cron_quebrado`. Esse alarme pode entrar no core enquanto a exclusiva responsável pela recusa continua tomada. Sua saída é descartada; a FLAG só nasce se a chamada funcionar. (`bin/cron_run.sh:49`, `bin/cron_run.sh:50`)

O 75 não identifica exclusivamente timeout de deploy: também cobre biblioteca ausente, falha de `mkdir`, abertura do arquivo ou aquisição; além disso, um `CMD` pode devolver 75 por conta própria. A linha do placar não registra uma causa separada. (`bin/cron_run.sh:17`, `bin/cron_run.sh:24`, `bin/cron_run.sh:27`, `bin/cron_run.sh:30`, `bin/trava_deploy_exclusao.sh:31`, `bin/trava_deploy_exclusao.sh:32`, `bin/trava_deploy_exclusao.sh:34`)

Há significados anteriores de 75: “sem vez” na trava de testes, tratado pelo pre-push como “não rodou”; limite/indisponibilidade e pausa no wrapper Codex. Isso não constitui colisão automática entre scripts, mas impede interpretar 75 como um significado global único. (`bin/trava_teste.sh:18`, `bin/trava_teste.sh:38`, `bin/pre-push.sh:149`, `bin/codex_pergunta.sh:22`, `bin/codex_pergunta.sh:41`)

Para os leitores de cron, 75 é erro: `EXITS_DE_ALARME=(0,2)`; o vigia aceita o inteiro e abre pauta sem tratamento particular para 75. A corrente registra esse retorno, continua os elos e não faz retry; um dependente sem FIM pode terminar no próprio teto com rc 2 e ALARME. (`app/core/services/crons_quebrados.py:16`, `app/core/services/crons_quebrados.py:46`, `app/core/management/commands/cron_quebrado.py:16`, `app/core/management/commands/cron_quebrado.py:37`, `bin/encadeado.sh:64`, `bin/encadeado.sh:66`, `bin/encadeado.sh:92`)

**3. Autoespera, aninhamentos e segundo plano**

O reload das 03:30 evita autoespera: é RAW envelopado, mas `--reload-agendado` recusa migration pendente e não arma exclusiva. (`app/config/crons.py:326`, `app/config/crons.py:1863`, `bin/deploy.sh:211`, `bin/deploy.sh:316`)

**Existe outro caminho cron → deploy:** `vigia_arvore.sh` chama `esteira_vigia.py curar`, que pode chamar deploy `--sem-sombra`, inclusive novamente ao desfazer o revert. Se esse deploy precisar aplicar migration, tentará exclusiva enquanto seu envelope ancestral conserva compartilhada: espera até 600 s e recusa contra si mesmo. A chamada Python também descarta o retorno do deploy e segue para os testes. (`app/config/crons.py:296`, `bin/vigia_arvore.sh:176`, `app/core/esteira_vigia.py:1267`, `app/core/esteira_vigia.py:1269`, `app/core/esteira_vigia.py:1275`, `bin/cron_run.sh:24`, `bin/deploy.sh:233`, `bin/deploy.sh:319`, `bin/trava_deploy_exclusao.sh:26`)

O outro cron encontrado chamando deploy, `vigia_logins --auto`, usa `--sem-migrate`; com pendência ele recusa, sem pendência não pede exclusiva. (`app/config/crons.py:303`, `bin/vigia_logins.sh:87`, `bin/deploy.sh:215`, `bin/deploy.sh:316`)

A corrente não aninha envelopes por sua construção: `; encadeado.sh` vem depois do envelope raiz, e cada elo faz `exec` de seu próprio envelope. `deploy_agendado` entra diretamente por cron.d e executa a esteira recebida, sem envelope próprio; as fatias realmente agendadas são **[nao verificado]**. (`app/config/crons.py:1885`, `bin/encadeado.sh:102`, `bin/encadeado.sh:104`, `bin/deploy_agendado.sh:55`, `bin/deploy_agendado.sh:73`)

A cadeia toma árvore e régua antes do deploy final. Já `sombra.sh --conferir`, chamado pelo deploy, não passa pelos ramos que adquirem a trava da sombra e não executa o bloco de crons. (`bin/molde_fatia/cadeia.sh:11`, `bin/molde_fatia/cadeia.sh:14`, `bin/molde_fatia/cadeia.sh:89`, `bin/deploy.sh:162`, `bin/sombra.sh:438`, `bin/sombra.sh:451`, `bin/sombra.sh:463`)

**Comandos efetivamente agendados em `crons.py` com lançamento explícito por `&`, `nohup` ou `setsid`: nenhum localizado.** O gerador executa RAW/Django sincronamente e acrescenta a corrente com `;`. Os jobs `Popen` citados no arquivo pertencem a `FORA_DE_PIPELINE`. (`app/config/crons.py:282`, `app/config/crons.py:350`, `app/config/crons.py:594`, `app/config/crons.py:688`, `app/config/crons.py:705`, `app/config/crons.py:769`, `app/config/crons.py:1369`, `app/config/crons.py:1863`, `app/config/crons.py:1885`)

Há assíncronos vizinhos: fabricante usa `setsid nohup ... &`; o observador de árvore fecha fds 6–9, mas deixa 4 aberto e vive enquanto o pai existir. Caminho demonstrado de um cron até esses lançamentos, ou filho de cron segurando compartilhada indefinidamente: **[nao verificado]**. (`bin/fabricante.sh:187`, `bin/trava.sh:68`, `bin/trava.sh:69`)

**4. Quem escreve e quem lê**

| Objeto | Escritores e consumidores localizados |
|---|---|
| Caminho da trava | Uma definição literal, derivada da raiz da biblioteca; usada para criar diretório e abrir o arquivo. (`bin/trava_deploy_exclusao.sh:16`, `bin/trava_deploy_exclusao.sh:31`, `bin/trava_deploy_exclusao.sh:32`) |
| fd 4 | A função comum abre; `flock` adquire; fecha na recusa; a soltura explícita faz unlock e close. (`bin/trava_deploy_exclusao.sh:32`, `bin/trava_deploy_exclusao.sh:34`, `bin/trava_deploy_exclusao.sh:37`, `bin/trava_deploy_exclusao.sh:42`) |
| Funções da biblioteca | Wrappers compartilhado/exclusivo chamam a implementação comum; consumidores são envelope e deploy; soltura explícita só no deploy. (`bin/trava_deploy_exclusao.sh:28`, `bin/trava_deploy_exclusao.sh:40`, `bin/trava_deploy_exclusao.sh:41`, `bin/cron_run.sh:17`, `bin/deploy.sh:319`, `bin/deploy.sh:442`) |
| `espera_deploy` | Envelope escreve; leitores explícitos localizados somente no selo novo. (`bin/cron_run.sh:30`, `bin/tests/test_deploy_exclusao_cron.sh:165`, `bin/tests/test_deploy_exclusao_cron.sh:182`, `bin/tests/test_deploy_exclusao_cron.sh:237`) |

Os leitores operacionais diretos do JSONL localizados foram estes; nenhum usa `espera_deploy`:

| Leitor | Interpretação e consequência |
|---|---|
| `crons_quebrados` | Usa chegada `ini` para escolher a última corrida em 26 h e classifica por exit. (`app/core/services/crons_quebrados.py:36`, `app/core/services/crons_quebrados.py:39`, `app/core/services/crons_quebrados.py:42`, `app/core/services/crons_quebrados.py:46`) |
| `pipeline_placar` | Toda linha incrementa `rodou`: uma tentativa sem comando vira `rodou=1, erros=1`, situação ERRO. (`app/core/management/commands/pipeline_placar.py:73`, `app/core/management/commands/pipeline_placar.py:74`, `app/core/management/commands/pipeline_placar.py:78`, `app/core/management/commands/pipeline_placar.py:92`) |
| `crons_duracao.py` | Inclui `dur` sem filtrar exit; tentativas sem comando entram no p95 e podem reduzir a medida. Efeito efetivo: **[nao verificado]**. (`bin/crons_duracao.py:69`, `bin/crons_duracao.py:75`, `bin/crons_duracao.py:76`) |
| `corrida_no_horario` | Recebe `ini` e filtra seu minuto, embora a documentação diga “começou no horário”. Agora isso mede chegada ao envelope, podendo anteceder o comando em até 300 s. (`app/config/crons.py:2305`, `app/config/crons.py:2328`, `app/config/crons.py:2332`, `bin/cron_run.sh:7`, `bin/cron_run.sh:24`) |

`dur` exclui a aquisição, mas inclui preparação da MARCA; `fim−ini` inclui a espera. Nenhum dos leitores diretos acima calcula essa diferença. Há, portanto, uma mudança real entre “chegou”, “rodou” e “começou o comando” que o comentário novo não resolve nos consumidores. (`bin/cron_run.sh:7`, `bin/cron_run.sh:19`, `bin/cron_run.sh:22`, `bin/cron_run.sh:29`, `bin/cron_run.sh:30`)

**5. Tetos de 300/600 e corrente**

**600 s limita aquisição, não retenção.** Depois de obter exclusiva, o deploy inclui migrate sem timeout, consultas `docker top` sem timeout, até 270 s nominais de sleeps dos workers, três esperas de disponibilidade de até 180 s cada e provas de rota sem timeout. Não existe teto global demonstrável para quanto segura. (`bin/trava_deploy_exclusao.sh:26`, `bin/trava_deploy_exclusao.sh:34`, `bin/deploy.sh:337`, `bin/deploy.sh:377`, `bin/deploy.sh:386`, `bin/deploy.sh:391`, `bin/deploy.sh:403`, `bin/deploy.sh:406`, `bin/deploy.sh:419`, `bin/deploy.sh:442`)

Assim, **um deploy com migration pode terminar com sucesso e ainda fazer um cron atingir 300 s**, sair 75 e alarmar sem FIM. A afirmação de que 300 “cobre” a troca e deixa 600 para a corrente não demonstra cobertura de migration, disponibilidade e rotas. (`bin/trava_deploy_exclusao.sh:17`, `bin/trava_deploy_exclusao.sh:21`, `bin/cron_run.sh:27`, `bin/cron_run.sh:39`, `bin/cron_run.sh:50`)

Os 900 s pertencem ao dependente e começam antes da consulta dos carimbos; não são folga integral garantida quando o deploy começa. HE espera deslogados por 1800 s, mas quem espera HE continua com 900 s. Tempo efetivo disponível e duração real do deploy: **[nao verificado]**. (`bin/encadeado.sh:86`, `bin/encadeado.sh:92`, `app/config/crons.py:1550`, `app/config/crons.py:1563`)

**6. Classes de reprovação anteriores**

- **Fonte do caminho:** há uma definição textual, mas ela é relativa à biblioteca carregada. Deploy carrega da própria raiz; envelope carrega da viva absoluta. Executar deploy de outra cópia pode disputar outro arquivo enquanto opera os mesmos containers. Ocorrência operacional: **[nao verificado]**. (`bin/trava_deploy_exclusao.sh:16`, `bin/deploy.sh:317`, `bin/cron_run.sh:17`, `bin/deploy.sh:337`)
- **Derivação paralela:** aquisição, tetos e descritor estão centralizados na biblioteca; a diferença de raiz acima continua possível. (`bin/trava_deploy_exclusao.sh:16`, `bin/trava_deploy_exclusao.sh:21`, `bin/trava_deploy_exclusao.sh:26`, `bin/trava_deploy_exclusao.sh:28`)
- **Ordem:** cadeia → árvore → régua → exclusiva está demonstrada. Não demonstrei ciclo inverso envolvendo árvore/régua; a autoespera concreta é compartilhada ancestral → exclusiva no auto-revert. (`bin/molde_fatia/cadeia.sh:11`, `bin/molde_fatia/cadeia.sh:14`, `bin/molde_fatia/cadeia.sh:89`, `app/core/esteira_vigia.py:1267`, `bin/deploy.sh:319`)
- **Relógio:** espera registrada e `dur` usam subtração de `date +%s`, sem proteção contra salto do relógio; podem divergir do tempo transcorrido ou ficar negativos. (`bin/trava_deploy_exclusao.sh:33`, `bin/trava_deploy_exclusao.sh:35`, `bin/cron_run.sh:19`, `bin/cron_run.sh:29`)
- **Campo com dois significados:** `rodou` passa a incluir tentativa recusada; 75 agrega causas diferentes; filtro de “início” usa chegada. (`app/core/management/commands/pipeline_placar.py:74`, `bin/cron_run.sh:17`, `bin/cron_run.sh:27`, `app/config/crons.py:2328`)

Os mutantes “sem exclusiva” e “exit 75 antes do epílogo” têm asserções capazes de acusá-los em a1/a2 e b2. Porém o arquivo apenas aceita fontes alternativas; execução e mordida observadas dos mutantes/RED: **[nao verificado]**. (`bin/tests/test_deploy_exclusao_cron.sh:32`, `bin/tests/test_deploy_exclusao_cron.sh:135`, `bin/tests/test_deploy_exclusao_cron.sh:152`, `bin/tests/test_deploy_exclusao_cron.sh:187`)

**Falta mordida para soltura prematura:** b1 exige comando após HUP; b3, após restart. O Docker falso troca workers e responde às provas imediatamente; esses casos não demonstram reprovar soltura antes da conferência final. d4 apenas verifica `30 < teto < 900`, sem validar orçamento temporal. (`bin/tests/test_deploy_exclusao_cron.sh:90`, `bin/tests/test_deploy_exclusao_cron.sh:96`, `bin/tests/test_deploy_exclusao_cron.sh:168`, `bin/tests/test_deploy_exclusao_cron.sh:205`, `bin/tests/test_deploy_exclusao_cron.sh:258`)

**7. Contrato e selos vizinhos**

Pela leitura, não vejo quebra das condições textuais de `contrato_deploy.sh`: HUP único e conjunto, restart manual único precedido de recusa e três provas de rota continuam presentes. Esse contrato não cobra a nova exclusiva; sua checagem de migration usa a primeira ocorrência de `migrate_schemas`, que é consulta, não apply. Resultado executado: **[nao verificado]**. (`bin/contrato_deploy.sh:24`, `bin/contrato_deploy.sh:33`, `bin/contrato_deploy.sh:47`, `bin/contrato_deploy.sh:54`, `bin/deploy.sh:184`, `bin/deploy.sh:337`, `bin/deploy.sh:437`)

Copiar a biblioteca sem substituir caminho nos vizinhos está correto nesta implementação: ela deriva o lock de seu próprio local. Entretanto, os vizinhos continuam plantando MARCA sem adquirir compartilhada; preservam a prova da recusa por marca, não comprovam o protocolo novo. O selo novo entra na régua pelo glob existente. (`bin/trava_deploy_exclusao.sh:16`, `bin/tests/test_deploy_reload_gracioso.sh:41`, `bin/tests/test_deploy_reload_gracioso.sh:92`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:71`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:78`, `bin/regua.sh:138`)

Há divergência do contrato anterior registrado: cron 600/deploy não bloqueante/`espera_lock`/dois contadores ESTADO versus cron 300/deploy esperando 600/`espera_deploy`, sem consumo operacional desse campo nos leitores encontrados. Aval posterior para essa mudança de desenho: **[nao verificado]**. (`app/docs/RELATO.md:25`, `bin/trava_deploy_exclusao.sh:21`, `bin/trava_deploy_exclusao.sh:26`, `bin/trava_deploy_exclusao.sh:34`, `bin/cron_run.sh:30`)

## 09/11 06:42 · sessao c9691c154609 · - · - · origem=Ronald (tmux)

1. **Caminho de sucesso fecha as duas janelas para envelopes desta versão.** O único apply está em `bin/deploy.sh:337`, e o único restart em `bin/deploy.sh:364`; ambos passam pela aquisição exclusiva em `bin/deploy.sh:316-325`. O envelope só cria MARCA e executa após adquirir compartilhada (`bin/cron_run.sh:17-24`). A lib mantém a trava no fd 4 (`bin/trava_deploy_exclusao.sh:32-36`).

2. **Falha não preserva a exclusão até uma prova concluída.** `falhou()` chama `exit 1` (`bin/deploy.sh:92`); pode ocorrer depois de começar migrate, depois do HUP, durante conferência de workers ou nas rotas (`bin/deploy.sh:337`, `bin/deploy.sh:381`, `bin/deploy.sh:394`, `bin/deploy.sh:440`). Só no sucesso há unlock explícito depois das três provas (`bin/deploy.sh:437-442`). Assim, um deploy abortado pode liberar a exclusão sem reload conferido; consistência resultante do banco/código depende do ponto da falha e das migrations [nao verificado].

3. **A morte do processo não garante liberação imediata se houver descendente vivo.** A própria lib declara herança pelos filhos (`bin/trava_deploy_exclusao.sh:10-11`); o fd é aberto sem fechamento no comando de cron (`bin/trava_deploy_exclusao.sh:32`, `bin/cron_run.sh:24`). O arquivo de lock permanece; exclusão depende dos descritores, não da existência do arquivo (`bin/trava_deploy_exclusao.sh:14-16`, `bin/trava_deploy_exclusao.sh:32-42`). Descendente concreto de cron que retenha indefinidamente: [nao verificado].

4. **Aninhamentos encontrados:**
   - Reload agendado é RAW envelopado → deploy `--reload-agendado` (`app/config/crons.py:326-327`, `app/config/crons.py:1861-1863`). Esse modo não arma migration pendente: recusa em `bin/deploy.sh:211-214`; sem restart/apply não pede exclusiva (`bin/deploy.sh:316`). Portanto esse caminho evita autoespera.
   - Outro caminho existe: cron `vigia_arvore.sh` (`app/config/crons.py:296`) → `core/esteira_vigia.py curar` (`bin/vigia_arvore.sh:176`) → deploy `--sem-sombra` (`app/core/esteira_vigia.py:1267-1268`, `app/core/esteira_vigia.py:1275`). Se esse deploy armar apply, tenta exclusiva enquanto o envelope ancestral segura compartilhada: autoespera até 600 s e recusa (`bin/cron_run.sh:17-24`, `bin/deploy.sh:233`, `bin/deploy.sh:316-323`, `bin/trava_deploy_exclusao.sh:26`).
   - `vigia_logins.sh --auto` é cron e chama deploy, mas usa `--sem-migrate`, que recusa migrations pendentes e evita exclusiva (`app/config/crons.py:303`, `bin/vigia_logins.sh:87`, `bin/deploy.sh:215-218`, `bin/deploy.sh:316`).
   - Corrente não aninha envelopes por construção: o render acrescenta `; encadeado.sh` depois do envelope raiz, e cada elo faz exec do seu envelope (`app/config/crons.py:1884-1885`, `bin/encadeado.sh:102-104`).
   - `deploy_agendado` entra diretamente por cron.d e executa a esteira fornecida; não cria envelope próprio (`bin/deploy_agendado.sh:54-56`, `bin/deploy_agendado.sh:68-73`). Fatias realmente agendadas [nao verificado].
   - Cadeia toma árvore e régua antes de deploy; o deploy final pode adquirir exclusiva nessa ordem (`bin/molde_fatia/cadeia.sh:11-14`, `bin/molde_fatia/cadeia.sh:88-89`, `bin/deploy.sh:316-325`). `sombra.sh --conferir`, usado pelo deploy, não adquire a trava da sombra (`bin/deploy.sh:162`, `bin/sombra.sh:438`, `bin/sombra.sh:451`, `bin/sombra.sh:463`).

5. **Segundo plano:** não há `&`, `nohup` ou `setsid` nos comandos efetivamente declarados em CRONS; o gerador lança RAW/Django pelo envelope e a corrente por `;` (`app/config/crons.py:282-352`, `app/config/crons.py:594-595`, `app/config/crons.py:688-706`, `app/config/crons.py:1861-1885`). Os RAW inspecionados usam execução síncrona, incluindo `noturno.py` (`eval/noturno.py:32-44`, `eval/noturno.py:93-94`), fábrica (`bin/fabrica_estado.py:63-66`), alarme de sessão (`bin/alarme_sessao_ociosa.py:142-150`) e backup (`scripts/backup_all.sh:6-8`). Efeitos transitivos arbitrários dos comandos Django [nao verificado].

   Há lançamentos assíncronos vizinhos, sem caminho demonstrado a partir dessas entradas: fabricante usa `setsid nohup ... &` (`bin/fabricante.sh:187`); controlador/esteiras usam `Popen` (`app/core/esteira_vigia.py:801-802`, `app/core/esteira_vigia.py:917-918`). A esteira está declarada como timer fora do cron (`app/config/crons.py:764-767`). O observador de árvore fecha fds 6–9, mas não 4, e dura enquanto o pai existir (`bin/trava.sh:68-69`); cron → esse observador [nao verificado]. Os três jobs `Popen` descritos em `crons.py` pertencem a FORA_DE_PIPELINE, não a CRONS (`app/config/crons.py:769`, `app/config/crons.py:1369-1371`).

6. **Posse não tem teto de 600 s.** Esse teto limita a aquisição (`bin/trava_deploy_exclusao.sh:26`, `bin/trava_deploy_exclusao.sh:34`). Depois dela, o deploy inclui migrate sem timeout (`bin/deploy.sh:337`), consultas `docker top` sem timeout (`bin/deploy.sh:377-379`), até 270 s nominais de sleeps dos workers (`bin/deploy.sh:382-395`), três esperas de disponibilidade de até 180 s cada (`bin/deploy.sh:402-408`) e provas de rota sem timeout (`bin/deploy.sh:419-439`). Portanto um deploy com migration pode superar 300 s mesmo terminando com sucesso, levando cron ao rc 75; duração efetivamente observada em produção [nao verificado].

## 09/11 06:41 · sessao 5299063743ea · - · - · origem=Ronald (tmux)

**Achados que merecem bloquear a confirmação:**

1. **O caminho “sem vez” ainda entra no core:** o `CMD` não roda e MARCA/FIM não são criados, mas o epílogo chama `docker exec ... cron_quebrado` sem ter conseguido a compartilhada. Isso permite executar o alarme no core durante a migration/exclusiva que impediu o cron. A ausência de execução real em produção é [nao verificado]. (`bin/cron_run.sh:17`, `bin/cron_run.sh:20`, `bin/cron_run.sh:27`, `bin/cron_run.sh:39`, `bin/cron_run.sh:46`, `bin/cron_run.sh:50`)

2. **O placar passa a chamar “rodou” uma tentativa que não executou o comando.** `pipeline_placar.medidos()` incrementa `rodou` para toda linha e conta `75` também como erro. Assim, o caso sem vez aparece como `rodou=1, erros=1`; não como `NAO_RODOU`. (`app/core/management/commands/pipeline_placar.py:69`, `app/core/management/commands/pipeline_placar.py:73`, `app/core/management/commands/pipeline_placar.py:78`, `app/core/management/commands/pipeline_placar.py:92`)

3. **Tentativas sem comando entram na duração medida.** `crons_duracao.py` aceita qualquer `dur` inteiro, filtra pelo `ini` e não verifica `exit`; portanto uma tentativa rc75 com `dur≈0` entra no p95. Muitas dessas tentativas podem diminuir a duração calculada. Diminuição efetiva em produção: [nao verificado]. (`bin/crons_duracao.py:69`, `bin/crons_duracao.py:71`, `bin/crons_duracao.py:73`, `bin/crons_duracao.py:75`, `bin/crons_duracao.py:76`)

4. **300/600 não limitam o tempo segurando a exclusiva.** São os tempos passados ao `flock -w`, isto é, limites de aquisição. Depois de obter a exclusiva, migrate não tem timeout; há até 270 segundos nominais de sleeps de troca de workers, até três esperas de disponibilidade com timeout180, e provas de rota sem timeout. O total de retenção não tem limite demonstrável no script. (`bin/trava_deploy_exclusao.sh:21`, `bin/trava_deploy_exclusao.sh:26`, `bin/trava_deploy_exclusao.sh:34`, `bin/deploy.sh:337`, `bin/deploy.sh:382`, `bin/deploy.sh:386`, `bin/deploy.sh:391`, `bin/deploy.sh:403`, `bin/deploy.sh:406`, `bin/deploy.sh:419`, `bin/deploy.sh:442`)

**Pergunta 2 — envelope e rc75**

O único lançamento do `CMD` está no ramo `VEZ=0`, que só é armado depois de source e aquisição compartilhada bem-sucedidos; MARCA está nesse mesmo ramo, e FIM só é escrito para rc0/2. Não localizei caminho normal que rode o `CMD` ou faça MARCA/FIM sem vez. A execução de `cron_quebrado` no epílogo é a exceção acima. (`bin/cron_run.sh:16`, `bin/cron_run.sh:17`, `bin/cron_run.sh:20`, `bin/cron_run.sh:22`, `bin/cron_run.sh:24`, `bin/cron_run.sh:39`, `bin/cron_run.sh:50`)

O `75` significa tanto timeout quanto falha ao carregar biblioteca, criar diretório, abrir arquivo ou executar/adquirir `flock`; o placar não contém uma causa separada. Portanto `exit75` isolado não prova “esperou deploy e atingiu teto”. (`bin/cron_run.sh:17`, `bin/cron_run.sh:27`, `bin/trava_deploy_exclusao.sh:31`, `bin/trava_deploy_exclusao.sh:32`, `bin/trava_deploy_exclusao.sh:34`, `bin/trava_deploy_exclusao.sh:38`)

Há usos preexistentes de75: `trava_teste.sh` usa “não conseguiu a vez”, e `pre-push.sh` distingue isso de vermelho de código; `codex_pergunta.sh` usa75 para limite/indisponibilidade e também pausa. São contextos distintos, não uma reserva global de significado. (`bin/trava_teste.sh:18`, `bin/trava_teste.sh:38`, `bin/pre-push.sh:149`, `bin/codex_pergunta.sh:22`, `bin/codex_pergunta.sh:41`, `bin/codex_pergunta.sh:54`)

Nos leitores de cron,75 é inequivocamente erro: `EXITS_DE_ALARME=(0,2)`, e todos os demais exits aparecem como cron quebrado; o comando `cron_quebrado` aceita o inteiro e abre pauta sem especializar75. (`app/core/services/crons_quebrados.py:16`, `app/core/services/crons_quebrados.py:46`, `app/core/management/commands/cron_quebrado.py:16`, `app/core/management/commands/cron_quebrado.py:37`)

A corrente não dá retry nem tratamento especial a75: registra o rc, continua os próximos elos e devolve o último rc não zero observado. Sem FIM do predecessor, um dependente espera seu teto e sai2 com ALARME. (`bin/encadeado.sh:64`, `bin/encadeado.sh:66`, `bin/encadeado.sh:68`, `bin/encadeado.sh:86`, `bin/encadeado.sh:92`, `bin/encadeado.sh:94`)

**Pergunta 4 — escritores e leitores**

| Objeto | Escritores/operadores | Leitores/consumidores |
|---|---|---|
| Caminho da trava | Única definição literal localizada; deriva da raiz da biblioteca. (`bin/trava_deploy_exclusao.sh:16`) | A função comum usa o caminho para mkdir/open. (`bin/trava_deploy_exclusao.sh:31`, `bin/trava_deploy_exclusao.sh:32`) |
| fd4 | Função comum abre em append, fecha na falha; soltura explícita faz unlock/close. (`bin/trava_deploy_exclusao.sh:32`, `bin/trava_deploy_exclusao.sh:37`, `bin/trava_deploy_exclusao.sh:42`) | `flock` opera no fd4; filhos o herdam segundo contrato declarado e ausência de close no lançamento. (`bin/trava_deploy_exclusao.sh:10`, `bin/trava_deploy_exclusao.sh:34`, `bin/cron_run.sh:24`) |
| Função interna | `_trava_deploy_exclusao_toma` implementa aquisição/medida. (`bin/trava_deploy_exclusao.sh:28`) | Wrappers compartilhado/exclusivo. (`bin/trava_deploy_exclusao.sh:40`, `bin/trava_deploy_exclusao.sh:41`) |
| Compartilhada | Wrapper chama interna com `-s`. (`bin/trava_deploy_exclusao.sh:40`) | Envelope. (`bin/cron_run.sh:17`) |
| Exclusiva/soltura | Wrapper chama interna com `-x`; soltura faz `-u`. (`bin/trava_deploy_exclusao.sh:41`, `bin/trava_deploy_exclusao.sh:42`) | Deploy. (`bin/deploy.sh:319`, `bin/deploy.sh:442`) |
| `espera_deploy` | Envelope escreve uma linha com o valor. (`bin/cron_run.sh:18`, `bin/cron_run.sh:30`) | Leitores explícitos localizados apenas no selo novo. (`bin/tests/test_deploy_exclusao_cron.sh:165`, `bin/tests/test_deploy_exclusao_cron.sh:182`, `bin/tests/test_deploy_exclusao_cron.sh:237`) |

Os leitores operacionais diretos de `placar.jsonl` localizados foram:

- `crons_quebrados()`: lê `ini`, nome/emp e exit; escolhe a última chegada por cron em26h; ignora duração e espera. (`app/core/services/crons_quebrados.py:30`, `app/core/services/crons_quebrados.py:36`, `app/core/services/crons_quebrados.py:39`, `app/core/services/crons_quebrados.py:42`, `app/core/services/crons_quebrados.py:46`)
- `pipeline_placar.medidos()`: agrupa pelo dia de `ini`, conta rodadas/erros/alarmes e máximo de `dur`; ignora espera. (`app/core/management/commands/pipeline_placar.py:65`, `app/core/management/commands/pipeline_placar.py:70`, `app/core/management/commands/pipeline_placar.py:73`, `app/core/management/commands/pipeline_placar.py:80`)
- `crons_duracao.py`: janela de sete dias por `ini`, filtro horário, p95 de `dur`; ignora espera e exit. (`bin/crons_duracao.py:55`, `bin/crons_duracao.py:61`, `bin/crons_duracao.py:69`, `bin/crons_duracao.py:73`, `bin/crons_duracao.py:76`)
- `corrida_no_horario()` recebe `ini` do leitor anterior e decide pelo minuto de chegada; sua documentação ainda diz “começou no horário agendado”. O comando pode começar até300s depois dessa chegada. (`app/config/crons.py:2304`, `app/config/crons.py:2305`, `app/config/crons.py:2328`, `app/config/crons.py:2332`, `bin/cron_run.sh:7`, `bin/cron_run.sh:19`, `bin/cron_run.sh:24`)

`ini` é chegada, `fim` é horário após a tentativa/comando, e `dur` é diferença dos tempos após aquisição. Logo agora `fim−ini` inclui espera enquanto `dur` a exclui; nenhum dos leitores diretos acima calcula `fim−ini`. “Só comando” é aproximado: `T0` antecede mkdir/echo/trap da MARCA. (`bin/cron_run.sh:7`, `bin/cron_run.sh:19`, `bin/cron_run.sh:22`, `bin/cron_run.sh:23`, `bin/cron_run.sh:29`, `bin/cron_run.sh:30`)

**Perguntas 5/6 — tetos, fonte e relógio**

Um cron que chegue durante retenção exclusiva superior300s termina75 sem FIM e tenta alarmar. Não é necessário que os900s da corrente acabem para esse alarme; e, se o dependente já consumiu parte do seu teto, não há folga universal de600s. Os1800s de HE são a espera de HE por deslogados; o sucessor de HE continua com900s. Ocorrência real num deploy normal: [nao verificado]. (`bin/trava_deploy_exclusao.sh:21`, `bin/cron_run.sh:27`, `bin/cron_run.sh:39`, `bin/cron_run.sh:50`, `bin/encadeado.sh:86`, `bin/encadeado.sh:92`, `app/config/crons.py:1549`, `app/config/crons.py:1550`, `app/config/crons.py:1562`, `app/config/crons.py:1563`)

Há fonte única textual do caminho, mas **não há caminho global independente da cópia**: deploy carrega sua `RAIZ/bin`, enquanto cron_run carrega a biblioteca absoluta da viva; a biblioteca deriva o lock de sua própria raiz. Um deploy executado de outra raiz pode disputar outro inode/arquivo e operar os mesmos containers. Se isso ocorre operacionalmente: [nao verificado]. (`bin/deploy.sh:317`, `bin/cron_run.sh:17`, `bin/trava_deploy_exclusao.sh:16`, `bin/deploy.sh:337`, `bin/deploy.sh:364`)

Espera registrada e duração usam `date +%s`, sem defesa/clamp para salto do relógio; portanto os números podem ficar negativos ou divergir do tempo efetivamente transcorrido. A biblioteca passa o teto a `flock -w`, mas a medição é uma subtração independente do relógio civil. (`bin/trava_deploy_exclusao.sh:33`, `bin/trava_deploy_exclusao.sh:34`, `bin/trava_deploy_exclusao.sh:35`, `bin/cron_run.sh:19`, `bin/cron_run.sh:29`, `bin/cron_run.sh:30`)

O contrato anterior anexado exige `espera_lock`, espera cron600/deploy não bloqueante e contadores ESTADO. A cópia implementa `espera_deploy`, cron300/deploy600 e não fornece consumidor operacional desse novo campo nos leitores encontrados; autorização posterior dessa mudança contratual é [nao verificado]. (`bin/trava_deploy_exclusao.sh:21`, `bin/trava_deploy_exclusao.sh:26`, `bin/trava_deploy_exclusao.sh:34`, `bin/cron_run.sh:30`; contrato_v2.txt no prompt, sem numeração: [nao verificado])

## 09/11 06:41 · sessao 2ef9d324e8ba · - · - · origem=Ronald (tmux)

Leitura estática concluída; não executei testes nem alterei arquivos. Resultados RED/GREEN e mutantes efetivamente executados: **[nao verificado]**. [nao verificado]

1. **Há lacuna de mordida na retenção até a prova de rota.** O código solta a exclusiva depois das provas (`bin/deploy.sh:437-442`), mas b1 apenas exige comando depois do HUP e b3 apenas depois do restart (`bin/tests/test_deploy_exclusao_cron.sh:165-168`, `bin/tests/test_deploy_exclusao_cron.sh:203-205`). O Docker falso avança imediatamente a geração no HUP e responde imediatamente às provas (`bin/tests/test_deploy_exclusao_cron.sh:90-98`). Esses cenários não demonstram reprovar um mutante que solte logo após HUP/restart, antes de conferir workers/rotas; isso exigiria atraso controlado e observação nessas etapas (`bin/tests/test_deploy_exclusao_cron.sh:93-98`, `bin/tests/test_deploy_exclusao_cron.sh:165-168`).

2. **Os dois mutantes requeridos têm asserções que deveriam mordê-los, mas faltam as execuções.** Tirar a exclusiva deve quebrar a1/a2: exigem recusa sem migrate/HUP enquanto cron vive e migrate posterior ao fim do comando (`bin/tests/test_deploy_exclusao_cron.sh:129-154`). `exit 75` antes do epílogo deve quebrar b2: exige linha no placar, espera, chamada do vigia e ausência de MARCA/FIM (`bin/tests/test_deploy_exclusao_cron.sh:175-192`). O arquivo disponibiliza `DEPLOY_SRC`/`CRON_RUN_SRC`; não fabrica nem executa automaticamente os mutantes (`bin/tests/test_deploy_exclusao_cron.sh:32-35`, `bin/tests/test_deploy_exclusao_cron.sh:264-265`). Mordida observada: **[nao verificado]**.

3. **O novo selo participa da régua.** `regua.sh` executa todos os `bin/tests/test_*.sh` e interrompe no primeiro vermelho (`bin/regua.sh:138-145`). A checagem d4, contudo, só exige `30 < teto_cron < 900`; não demonstra que migrate + conferência + rota cabem em 300 s nem que o dependente conserva 600 s (`bin/tests/test_deploy_exclusao_cron.sh:255-259`).

4. **Copiar a biblioteca sem substituir o caminho nos selos vizinhos é correto para esta implementação.** Ela deriva o arquivo de trava do seu próprio `BASH_SOURCE`, e os vizinhos a copiam para `$F/bin`; portanto usa `$F/logs/deploy_exclusao.lock` (`bin/trava_deploy_exclusao.sh:16`, `bin/tests/test_deploy_reload_gracioso.sh:41`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:71`). Não identifiquei segunda definição operacional do nome do arquivo na cópia; ambos os chamadores carregam a biblioteca (`bin/cron_run.sh:17`, `bin/deploy.sh:317`).

5. **Os vizinhos continuam simulando marca sem tomar a compartilhada.** O selo de reload cria `docker exec` em background e planta a marca; o de portões planta marcas antes do deploy, na casca ou no migrate (`bin/tests/test_deploy_reload_gracioso.sh:91-97`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:78`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:90`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:112-113`). Isso preserva a cobertura da recusa por marca, mas não implementa a conversão dos vizinhos para segurar o lock pedida no `contrato_v2.txt` anexado **[nao verificado: anexo sem arquivo:linha disponível]**.

6. **Não vejo quebra das condições textuais de `contrato_deploy.sh`.** O HUP permanece único e conjunto; restart permanece único, no ramo manual e depois da recusa; as três provas e a falha por rota continuam presentes (`bin/contrato_deploy.sh:24-51`, `bin/contrato_deploy.sh:69-82`; implementação: `bin/deploy.sh:359-381`, `bin/deploy.sh:437-440`). O contrato textual não cobra a nova exclusiva e sua checagem de migration usa a primeira ocorrência de `migrate_schemas`, que é consulta, não o apply; logo não prova a posição do apply (`bin/contrato_deploy.sh:54-59`, `bin/deploy.sh:184`, `bin/deploy.sh:337`). Aprovação executada do contrato: **[nao verificado]**.

7. **Há divergência explícita do contrato anterior registrado.** O registro promete cron esperando 600 s, deploy `-x -n`, campo `espera_lock` e dois contadores no ESTADO (`app/docs/RELATO.md:25`). A implementação usa cron 300 s, deploy esperando 600 s e campo `espera_deploy` (`bin/trava_deploy_exclusao.sh:21-26`, `bin/trava_deploy_exclusao.sh:34`, `bin/cron_run.sh:30`). Isso é mudança de desenho, não simples realização literal daquele contrato; aval posterior para essas diferenças: **[nao verificado]**.

## 09/11 06:21 · sessao 771c488d1489 · SELOS-CANON-I1 · verificador · origem=Code

**Não confirmo cura integral.** A igualdade com célula crua foi retirada das duas vias, e o formato da negativa e o literal completo do PAREI foram centralizados. Porém, a negativa de obra fora da forma continua abrindo um intervalo que `_censo_24h` não consegue fechar. Há também células descartadas antes de chegar à nova pertença. (`bin/disjuntor.py:1702,2042,2096,2109,2265`; `bin/hook_stop_fila1.py:133,245,434,783`)

**1. Achados 1 e 4**

- **Achado 1, nos dois pontos alterados: curado.** Agent entra por `_censo_nega`; wrapper consulta `_pertence_fila1` e entrega a mesma fotografia da fila a `_censo_nega`. A segunda consulta repete a mesma autoridade, sem reler o BACKLOG. (`bin/disjuntor.py:1702,1848,2264,2265,2273`)
- **A pertença agora distingue token, célula e ambiguidade.** `OBRA-Q (qualificação)` fornece `OBRA-Q`; `O9 (C)` conserva a identidade qualificada. Pedir `O9` diante dessa última célula resulta em NAO SEI, assim como dois candidatos ou uma ocorrência adicional incompatível. (`bin/disjuntor.py:624,1661,1663,1666,1683,1684,1687,1689`)
- **A coleta anterior ainda limita o universo.** O reconhecedor de linha não aceita `+` nem `/` na célula. Portanto, os exemplos `ESPURIA-TELA (+ESPELHO-DECIDIU-ICONE)` e `HE-DE-14H-NUM-DIA (col945 12/09)` nem chegam à pertença. Isso não demonstra abertura indevida hoje: suas próprias linhas dizem fila 2 e registro sem construção, respectivamente. (`bin/hook_stop_fila1.py:133,245`; `app/docs/BACKLOG.md:1810,1083`)
- **O caso vivo apontado de ESMERIL é alcançado:** sua célula passa pela coleta e fornece o token `ESMERIL-SEM-POLITICA`, exigindo censo nas duas vias. (`app/docs/BACKLOG.md:1825`; `bin/hook_stop_fila1.py:133,252`; `bin/disjuntor.py:1663,1666,1702,2265`)
- **Achado 4, centralização: curado nos pontos lidos.** As negativas com obra usam `NEG_CENSO`; os dois consumidores usam `_obra_da_negativa`; os dois pontos do hook usam `DJ.PAREI_CENSO`. (`bin/disjuntor.py:309,1511,1515,1519,1642,1705,1723,2096`; `bin/hook_stop_fila1.py:434,783`)
- **Achado 4, efeito na medida: permanece.** Para `O9 (C)`, o parser agora recupera o ID inteiro, mas `_CENSO_EV_RE` só reconhece uma palavra como obra do APROVADO. Aprovar `O9` também não fecha a chave `O9 (C)`. O intervalo segue até `agora` e continua em `esperando`. (`bin/disjuntor.py:1516,2042,2099,2109,2115,2139`)

Existem outras negativas fora de `NEG_CENSO`: obra ilegível, obra ausente e falhas de classificação/importação. Elas não têm o cabeçalho “construtor de `<obra>` sem CENSO”; não são um `%r` remanescente nesse cabeçalho. (`bin/disjuntor.py:101,1647,1754,1905,2247`)

**2. Quem produz e quem lê**

| Função/constante | Definição ou produção → leitores |
|---|---|
| `_casa_id_inteiro` | Define comparação sem caixa, com fronteira que exclui letra/dígito/hífen → `_pertence_fila1` e os dois testes de `_parei_do_em_curso`. O comentário cita `_relato_parou`, mas as chamadas diretas estão em `_parei_do_em_curso`. (`bin/hook_stop_fila1.py:110,113,117,1301,1313`; `bin/disjuntor.py:1684`) |
| `_fila1_viva` | Lê BACKLOG, delega a `_itens_andaveis` e devolve **lista de células cruas**, preservando duplicidades → `_censo_nega` e `main_censo`. (`bin/disjuntor.py:1626,1633,1637,1702,2264`) |
| `_token_da_celula` | Produz ID simples ou identidade com sufixo, conforme a gramática → `_pertence_fila1`. (`bin/disjuntor.py:1651,1661,1666,1683`) |
| `_pertence_fila1` | Produz dentro/fora ou levanta ambiguidade → `_censo_nega` e `main_censo`. (`bin/disjuntor.py:1669,1685,1687,1689,1702,2265`) |
| `NEG_CENSO` | Cabeçalho definido uma vez → `_censo_nao_sei` e os dois ramos de negativa de `_censo_nega`. (`bin/disjuntor.py:1515,1642,1705,1723`) |
| `_NEG_CENSO_RE` | Reconhecedor definido em um ponto → `_obra_da_negativa`. (`bin/disjuntor.py:1516,1521`) |
| `_obra_da_negativa` | Extrai a obra → escritor do PAREI e `_censo_24h`; também é chamado pelo roundtrip do selo. (`bin/disjuntor.py:1519,309,2096`; `bin/tests/test_censo_antes_da_vaga.sh:598`) |
| `PAREI_CENSO` | Define o literal completo → mensagens de negativa, montagem do PAREI, validação da faixa e prova da linha. (`bin/disjuntor.py:1511,312,1644,1707,1726`; `bin/hook_stop_fila1.py:434,783,784`) |
| `_censo_24h` | Produz contagens, intervalos e `esperando` → `ler` incorpora em `censo`; `_censo_txt` apresenta o resultado. (`bin/disjuntor.py:2064,2138,2038,2147,2158`) |
| `prova_parei_censo` | Produz `None`, prova `''` ou motivo → Stop, alarme e handoff. (`bin/hook_stop_fila1.py:773,804,809`; `bin/alarme_sessao_ociosa.py:137`; `bin/handoff_sessao.sh:130`) |
| `_eh_faixa_censo` | Classifica a linha literal válida → `_topo` e `painel_do_topo`, usados pelo leitor e pelo escritor. (`bin/hook_stop_fila1.py:424,481,520`; `app/core/esteira_vigia.py:883`) |

A escrita material do PAREI continua passando por `no_topo`, dentro da trava do escritor do RELATO. Agent e CLI acionam essa escrita por `relato_negacao`. (`bin/disjuntor.py:298,324,1900,2277`; `app/core/esteira_vigia.py:890,892`)

**Divergências de significado encontradas:**

- O wrapper shell aceita somente `[A-Z0-9-]+`; o CLI aceita também minúsculas. Assim, o caso `obra-q` do selo testa diretamente o CLI, não o percurso completo do wrapper. (`bin/codex_pergunta.sh:188`; `bin/disjuntor.py:162,2246`; `bin/tests/test_censo_antes_da_vaga.sh:73,550`)
- Alarme e handoff delegam a prova ao hook. Seus prefixos não substituem a comparação do literal inteiro; no handoff, o recorte por espaço serve ao rótulo. Entretanto, o alarme procura em 40 linhas e o handoff consulta faixa e primeira linha: os universos observados diferem. (`bin/alarme_sessao_ociosa.py:124,137,138`; `bin/handoff_sessao.sh:129,130,133`; `bin/hook_stop_fila1.py:783`)
- A negativa admite identidade qualificada, enquanto o APROVADO consumido pela medida admite apenas uma palavra: essa é a incompatibilidade restante. (`bin/disjuntor.py:1516,2042`)

**3. Classes que já reprovaram**

| Classe | Resultado da leitura |
|---|---|
| Segundo juiz duplicando juiz registrado | Não identifiquei duplicação nova: as duas vias usam a mesma pertença e o mesmo juiz de brief. Correspondência dessa família com juiz registrado: **[não verificado]**. (`bin/disjuntor.py:1702,1713,1848,2265,2273`) |
| Segundo escritor fora da porta | **Presente, anterior:** `gerar_cortes` e `deploy_agendado` escrevem RELATO diretamente; o escritor canônico declara esses resíduos. (`bin/gerar_cortes.py:136`; `bin/deploy_agendado.sh:44`; `app/core/esteira_vigia.py:839`) |
| Relógio fora da borda | Não apareceu relógio novo no trecho alterado: `_censo_24h` recebe `agora`, e a entrada CLI obtém o tempo. Certificação do restante do repositório: **[não verificado]**. (`bin/disjuntor.py:2064,2078,2304`) |
| Derivação paralela | **Presente no alarme:** `_fila` conta com regra própria, excluindo apenas feito/fechado; o hook também exclui congelada/fila 2 e outras paradas. Essa conta alimenta o alarme, não a pertença do censo. (`bin/alarme_sessao_ociosa.py:89,113,148`; `bin/hook_stop_fila1.py:95,180`) |
| Selo/RED sem mordida | Os nove vínculos novos têm contraprovas identificáveis; execução integral e RED efetivo: **[não verificado]**. Há lacunas descritas abaixo. (`bin/tests/test_censo_antes_da_vaga.sh:714,724`) |
| Ordem de trava | O wrapper verifica censo antes de tomar a vaga; `no_topo` decide e grava sob a trava do RELATO. Não identifiquei inversão nova nesses percursos. (`bin/codex_pergunta.sh:234,239,240`; `app/core/esteira_vigia.py:883,890,892`) |
| Campo com dois significados | **Permanece a incompatibilidade de domínio de `obra`** entre negativa qualificada e APROVADO de uma palavra. Célula crua e token agora estão separados explicitamente. (`bin/disjuntor.py:1637,1683,1516,2042`) |

**4. Regressão ou estado anterior; quem é afetado**

- **ID iniciado por dígito com sufixo:** a limitação do reconhecedor é anterior; `_OBRA_ID` exige letra inicial e não foi alterado pelo diff fornecido. A função nova reutiliza essa gramática. Para uma célula como `09-OBRA (C)`, o wrapper agora recebe NAO SEI em vez do antigo FORA por igualdade crua; mesmo um brief conforme não supera essa etapa. No Agent, a extração continua falhando e produz negativa sem obra. É uma restrição nova sobre uma limitação anterior, não uma nova passagem sem censo. (`bin/disjuntor.py:157,1663,1665,1689,1728,1845,1847,2265,2267`; comparação anterior: `bin/disjuntor.py:1647`, trecho removido no diff fornecido)
- **Intervalo de obra fora da forma:** o defeito já existia; antes havia ainda a captura truncada/com aspas indicada no laudo. A volta corrige a leitura da negativa, mas não torna possível aprovar a mesma identidade fora da forma. Quem é afetado é o consumidor da medida: horas e lista de espera permanecem abertas; isso não equivale a vaga aberta. (`bin/disjuntor.py:1704,2042,2099,2109,2115,2159`)
- Ocorrência atual desses dois casos em produção: **[não verificado]**. O dado apresentado não demonstra célula viva qualificada iniciada por dígito nem eventos reais suficientes para medir o intervalo. **[não verificado]**

**5. Mordida dos mutantes novos**

| Mutante | Contraprova que o distingue |
|---|---|
| `pertenca-celula-crua`, `pertenca-wrapper-propria` | OBRA-Q qualificada exige rc 1 e NEGOU, contrariando FORA. (`bin/tests/test_censo_antes_da_vaga.sh:535,537,714,717`) |
| `token-e-a-celula` | O positivo com brief conforme exige rc 0 e APROVADO; impede “nega tudo”. (`bin/tests/test_censo_antes_da_vaga.sh:540,542,543,718`) |
| `pertenca-exato-cala-a-outra` | O9 junto de O9 (C), com brief conforme, deve negar. (`bin/tests/test_censo_antes_da_vaga.sh:545,576,578,715`) |
| `pertenca-dois-candidatos` | OBRA-Q (A)/(B) deve negar; o brief conforme ainda existe. (`bin/tests/test_censo_antes_da_vaga.sh:540,569,573,584,716`) |
| `id-inteiro-por-substring` | OBRA deve continuar FORA diante de OBRA-E3/OBRA-Q. (`bin/tests/test_censo_antes_da_vaga.sh:553,555,721`) |
| `negativa-com-repr`, `parser-sem-espaco` | Roundtrip da negativa real de O9 (C) exige identidade inteira e sem aspas. (`bin/tests/test_censo_antes_da_vaga.sh:595,598,599,722,723`) |
| `literal-de-volta-no-hook` | Contagem AST exige zero literais completos no hook. É uma mordida estrutural. (`bin/tests/test_censo_antes_da_vaga.sh:601,603,605,724`) |

Não identifiquei caso vazio nesses nove vínculos. O controle inclui `casos_pertenca`, e a fábrica exige âncora única antes da mutação. Isso não comprova execução verde/vermelha nesta cópia. (`bin/tests/test_censo_antes_da_vaga.sh:628,640,645,649`; execução: **[não verificado]**)

**As lacunas são concretas:** o roundtrip não chama `_censo_24h`; não há célula qualificada iniciada por dígito na fixture. O caso anterior `09-TURNO-ABERTO-EXPOSTA` espera FORA e não cadastra essa obra: prova aceitação sintática do CLI, mas seria vazio como prova de pertença ou de censo obrigatório. (`bin/tests/test_censo_antes_da_vaga.sh:36,117,119,588,598`)

## 09/11 06:18 · sessao 4175ac87a3e8 · - · - · origem=Ronald (tmux)

Os nove mutantes novos têm âncora única e produzem Python sintaticamente válido, conferidos em memória; a execução e a mordida real continuam [não verificado]. A fábrica exige âncora única, roda `casos_pertenca` na cópia mutada e cobra retorno diferente de zero; o controle inclui a mesma função na cópia sem mutação. `bin/tests/test_censo_antes_da_vaga.sh:633`, `bin/tests/test_censo_antes_da_vaga.sh:640`, `bin/tests/test_censo_antes_da_vaga.sh:645`, `bin/tests/test_censo_antes_da_vaga.sh:649`, `bin/tests/test_censo_antes_da_vaga.sh:628`.

| Mutante | Caso que estaticamente o distingue |
|---|---|
| `pertenca-celula-crua` | OBRA-Q qualificada passa a FORA, contrariando rc 1 e NEGOU. `bin/tests/test_censo_antes_da_vaga.sh:535`, `bin/tests/test_censo_antes_da_vaga.sh:537`, `bin/tests/test_censo_antes_da_vaga.sh:714` |
| `pertenca-wrapper-propria` | A igualdade crua no wrapper desvia OBRA-Q qualificada para FORA. `bin/disjuntor.py:2269`, `bin/tests/test_censo_antes_da_vaga.sh:535`, `bin/tests/test_censo_antes_da_vaga.sh:717` |
| `token-e-a-celula` | A negativa inicial pode continuar correta por NAO SEI, mas o positivo conforme precisa rc 0 e APROVADO. `bin/tests/test_censo_antes_da_vaga.sh:540`, `bin/tests/test_censo_antes_da_vaga.sh:543`, `bin/tests/test_censo_antes_da_vaga.sh:718` |
| `pertenca-exato-cala-a-outra` | O9 junto de O9 (C) passa indevidamente com brief conforme, contrariando rc 1. `bin/tests/test_censo_antes_da_vaga.sh:545`, `bin/tests/test_censo_antes_da_vaga.sh:576`, `bin/tests/test_censo_antes_da_vaga.sh:578`, `bin/tests/test_censo_antes_da_vaga.sh:715` |
| `pertenca-dois-candidatos` | OBRA-Q (A)/(B) passa indevidamente; o brief conforme criado antes ainda existe. `bin/tests/test_censo_antes_da_vaga.sh:540`, `bin/tests/test_censo_antes_da_vaga.sh:569`, `bin/tests/test_censo_antes_da_vaga.sh:573`, `bin/tests/test_censo_antes_da_vaga.sh:584`, `bin/tests/test_censo_antes_da_vaga.sh:716` |
| `id-inteiro-por-substring` | OBRA passa a ser nomeada por OBRA-E3/OBRA-Q e deixa de retornar FORA rc 0. `bin/tests/test_censo_antes_da_vaga.sh:553`, `bin/tests/test_censo_antes_da_vaga.sh:555`, `bin/tests/test_censo_antes_da_vaga.sh:721` |
| `negativa-com-repr` | O roundtrip da negativa real de O9 (C) lê aspas e difere da obra esperada. `bin/tests/test_censo_antes_da_vaga.sh:595`, `bin/tests/test_censo_antes_da_vaga.sh:599`, `bin/tests/test_censo_antes_da_vaga.sh:722` |
| `parser-sem-espaco` | O roundtrip de O9 (C) deixa de recuperar a obra inteira. `bin/tests/test_censo_antes_da_vaga.sh:595`, `bin/tests/test_censo_antes_da_vaga.sh:599`, `bin/tests/test_censo_antes_da_vaga.sh:723` |
| `literal-de-volta-no-hook` | A contagem AST exige zero literais no hook e encontra um. `bin/tests/test_censo_antes_da_vaga.sh:601`, `bin/tests/test_censo_antes_da_vaga.sh:605`, `bin/tests/test_censo_antes_da_vaga.sh:724` |

Não identifiquei caso vazio nesses nove vínculos. Há limites relevantes: `casos_pertenca` testa o parser diretamente, sem demonstrar fechamento do intervalo por `_censo_24h`; também não cria célula qualificada começando por dígito. O teste anterior de `09-TURNO-ABERTO-EXPOSTA` espera FORA e não põe essa obra no BACKLOG da fixture. `bin/tests/test_censo_antes_da_vaga.sh:117`, `bin/tests/test_censo_antes_da_vaga.sh:119`, `bin/tests/test_censo_antes_da_vaga.sh:588`, `bin/tests/test_censo_antes_da_vaga.sh:598`, `bin/tests/test_censo_antes_da_vaga.sh:36`.

Retiro minha hipótese inicial sobre caixa: `obra-q` é aceita pelo regex do CLI e chega à pertença; o wrapper shell efetivo não é chamado por esse caso, que usa diretamente `disjuntor.py --censo`. `bin/disjuntor.py:162`, `bin/disjuntor.py:2246`, `bin/disjuntor.py:2265`, `bin/tests/test_censo_antes_da_vaga.sh:73`, `bin/tests/test_censo_antes_da_vaga.sh:550`.

## 09/11 06:17 · sessao db66642c6e5d · - · - · origem=Ronald (tmux)

Fora dos dois arquivos centrais, os leitores encontrados são:

| Sítio | Leitura e significado |
|---|---|
| `bin/alarme_sessao_ociosa.py:137` | Chama `H.prova_parei_censo(l.strip())`; só aceita como parada provada quando recebe `''`, descartando os demais resultados para linhas contendo o prefixo (`:138-140`). A substring encaminha ao reconhecedor; não substitui sua prova. |
| `bin/handoff_sessao.sh:129` | Percorre faixa e primeira linha do topo; chama `prova_parei_censo` em `:130` e decide pelo retorno em `:131-134`. O recorte pelo prefixo e primeiro espaço em `:133` serve apenas ao rótulo apresentado. |
| `bin/codex_pergunta.sh:234` | Invoca `disjuntor.py --censo`; todo retorno diferente de zero recusa. A gramática do construtor permite `[A-Z0-9-]+`, incluindo início por dígito, e não permite cauda (`:188`). O censo precede a aquisição da vaga (`:239-240`). |

Não encontrei escritor externo do literal completo ou leitor externo de `NEG_CENSO`/`_obra_da_negativa` no código Python/shell pesquisado; abrangência além desses arquivos e extensões: **[não verificado]**.

A implementação da prova continua aceitando processos com papel **censo ou leitura**, usando `CODEX_OBRA` inteiro e exato; isto não é alteração da volta 5: `bin/hook_stop_fila1.py:726`, `:742-745`, `:763-767`; o diff fornecido não altera esses trechos **[não verificado: linha do anexo]**.

Há classes anteriores ainda visíveis:

- **Derivação paralela da fila no alarme:** `_fila` lê seu próprio bloco e conta linhas, descartando somente `☑`, `FEITO` ou `FECHAD` (`bin/alarme_sessao_ociosa.py:89-116`). Assim, uma linha `CONGELADA`/`FILA 2` pode contar ali, embora o hook a classifique como não andável (`bin/hook_stop_fila1.py:95-97`). Essa contagem alimenta o alarme (`bin/alarme_sessao_ociosa.py:148-150`), não a pertença do censo.
- **Escritores do RELATO fora da porta:** `bin/gerar_cortes.py:117` lê e `:136` escreve diretamente; `bin/deploy_agendado.sh:37` lê e `:44` escreve diretamente. A autoridade da porta já declara esses resíduos (`app/core/esteira_vigia.py:839-840`).
- **A via do censo usa a porta existente:** `no_topo` chama `_grava_relato` dentro do callback (`app/core/esteira_vigia.py:878-890`) executado sob `_sob_trava_do_relato` (`:892`); esta trava o diretório (`:850-860`). Não há segundo escritor material introduzido nessa via nesses pontos.

O alarme também lê uma janela própria de 40 linhas (`bin/alarme_sessao_ociosa.py:120-124`), enquanto o handoff consulta a faixa e a primeira linha reconhecidas pelo hook (`bin/handoff_sessao.sh:129`). Portanto, compartilhar a prova não torna idêntico o conjunto de linhas observado.

Não há evidência de juiz **registrado** duplicado nesses achados: associação dessa família a uma entrada de `app/core/juizes.py`/`app/docs/JUIZES.md`: **[não verificado]**. Ocorrência, quantidade ou impacto em produção dessas divergências: **[não verificado]**.

## 09/11 06:15 · sessao 5f772a0de95b · DEPLOY-EXCLUSAO-ENVELOPE · censo · origem=Code

**A cópia já diverge da premissa inicial:** `--restart` recusa migration pendente no core antes do apply. O deploy normal ainda permite nascer cron durante o migrate; o `--restart` permitido ainda amostra marcas, deixando uma janela entre a última consulta e o restart. (`bin/deploy.sh:219-233`, `bin/deploy.sh:315-359`)

**1. Linha do tempo**

| Etapa | Onde acontece |
|---|---|
| Definição da leitura de marcas | `em_curso()` enumera os arquivos, consulta `/proc/<PID>` e remove marcas cujo PID não existe. `bin/deploy.sh:108-109`. |
| Primeira leitura efetiva | Somente em `--restart`: chamada em `:117`, passando por `recusa_restart_com_cron()` em `:111`. O deploy normal não passa por esse ramo. `bin/deploy.sh:110-118`. |
| Consulta de migrations | Core `--check` em `:184`; mensageria `--check` em `:194`; plano do core em `:205`. Isso ainda não é apply. `bin/deploy.sh:184-205`. |
| Decisão sobre apply | `--restart` com core pendente recusa; modo elegível arma `APLICAR_MIGRATE=1`. `bin/deploy.sh:219-233`. |
| Últimos portões | Prova de casca e selo de reciclagem antecedem o apply. `bin/deploy.sh:292-305`. |
| Migrate efetivo | `docker exec saas_core ... migrate_schemas --tenant`. `bin/deploy.sh:315-317`. |
| Outra leitura das marcas | Exclusiva de `--restart`, imediatamente antes do restart. `bin/deploy.sh:338-341`. |
| Restart ou HUP | Restart das três cascas em `:342`; nos demais modos, HUP das três em `:359`. `bin/deploy.sh:338-359`. |
| Espera após HUP | Confere desaparecimento dos workers antigos, com até 45 iterações e `sleep 2` por casca. `bin/deploy.sh:355-372`. |
| Prova de rota | Espera disponibilidade em `:380-386`; executa Django Client em `:394-405`; prova as três rotas em `:415-418`. `bin/deploy.sh:380-418`. |

As duas chamadas de leitura são, portanto, `:117` e `:341`, ambas exclusivas de `--restart`; não há espera por esvaziamento nesse fluxo: marca viva provoca recusa. (`bin/deploy.sh:110-118`, `bin/deploy.sh:338-342`)

**Janela aberta:** no normal, nenhum mecanismo desses impede nascimento desde o início do migrate até o HUP — nem durante a substituição dos workers. No `--restart`, cron nascido depois da primeira consulta pode ser detectado pela segunda; cron nascido depois da segunda amostragem pode alcançar o restart. O envelope escreve a marca e executa sem consultar o deploy. (`bin/deploy.sh:315-372`, `bin/cron_run.sh:9-11`)

**2. Escritores, leitores e entradas fora de envelope próprio**

| Caminho | Escrita, leitura ou remoção operacional localizada |
|---|---|
| `logs/crons_em_curso/` | Envelope cria diretório/nome `${N}_${E}.$$`, grava início e remove por `trap EXIT`. `bin/cron_run.sh:9-10`. |
| `logs/crons_em_curso/` | Deploy enumera marcas, consulta PID e remove as consideradas mortas; chamadas em `:117` e `:341`. `bin/deploy.sh:108-117`, `bin/deploy.sh:341`. |
| `logs/fim/` | Envelope cria diretório e grava `${N}.${E}.<dia>` somente para RC 0 ou 2. `bin/cron_run.sh:22-25`. |
| `logs/fim/` | Encadeado cria diretório, consulta carimbo próprio para não repetir e consulta carimbos do gatilho. `bin/encadeado.sh:74-89`. |
| Configuração | Define o runner, transmite gatilho/empresas/teto e gera chamadas; não faz esses acessos diretamente. `app/config/crons.py:1475-1486`, `app/config/crons.py:1845-1846`, `app/config/crons.py:1873-1885`. |

O carimbo FIM pode existir enquanto a marca “em curso” ainda existe: o envelope grava FIM antes dos comandos de alerta/recuperação de `cron_quebrado`, e só remove a marca ao sair. (`bin/cron_run.sh:10`, `bin/cron_run.sh:22-38`)

Nos testes, também há escrita/remoção desses caminhos: [nao verificado]

- Marcas em raiz falsa: `bin/tests/test_deploy_reload_gracioso.sh:34`, `bin/tests/test_deploy_reload_gracioso.sh:91`; `bin/tests/test_deploy_portoes_antes_do_migrate.sh:63`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:76`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:88`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:110`.
- Carimbos FIM na raiz calculada pelo teste, incluindo limpeza: `bin/tests/test_furo_encadeado_ao_cartorio.sh:17-22`, `bin/tests/test_furo_encadeado_ao_cartorio.sh:49-51`.
- Inspeções textuais, sem ler os diretórios operacionais: `bin/contrato_deploy.sh:81-82`, `bin/tests/test_furo_encadeado_ao_cartorio.sh:25`.

Uma ocorrência em `app/` descreve o carimbo, sem ser seu escritor: `app/chamados/management/commands/vigiar_fio_morto.py:18`. Outros acessos indiretos não identificados pelo censo textual: **[nao verificado]**.

**Entradas que executam no `saas_core` sem criar envelope próprio:**

| Entrada | Pontos de lançamento |
|---|---|
| Deploy: check, plano, apply, disponibilidade e rota | `bin/deploy.sh:184`, `bin/deploy.sh:205`, `bin/deploy.sh:316`, `bin/deploy.sh:381`, `bin/deploy.sh:397`. |
| Render da corrente, antes dos envelopes dos elos | `bin/encadeado.sh:45`; os elos passam pelo envelope em `bin/encadeado.sh:102-104`. |
| Render/install do crontab | `bin/crons.sh:24-27`. |
| Sombra: leitura da lavra e cálculo da janela | `bin/sombra.sh:138-144`, `bin/sombra.sh:466-473`. O executor principal da sombra usa outro container: `bin/sombra.sh:183`. |
| Lavradores auxiliares | `bin/lavrar_deslogados.sh:18`, `bin/lavrar_abriu_nao_bateu.sh:23`. |
| Gate de cobrança | `bin/gate_cobranca.sh:37-40`. |
| Alarmes da árvore e de ausência de fatia | `bin/vigia_arvore.sh:173-182`, `bin/alarme_sem_fatia.sh:90`. |
| Fábrica e sessão ociosa | `bin/fabrica_estado.py:64-66`, `bin/alarme_sessao_ociosa.py:140-149`. |
| Relato de nomes e diagrama | `bin/relato_nomes.sh:18`, `bin/gerar_diagrama.py:15`. |
| Node, usando core por padrão | `bin/node_check.sh:24`, `bin/node_check.sh:37`. |
| Selo de reciclagem, via container variável | `bin/selo_reciclagem.sh:34-45`. |
| Vigia da esteira → alarme | `bin/vigia_esteira.sh:11` → `app/core/esteira_vigia.py:894-896`. |

O placar contém vários lançamentos diretos: `bin/placar_code.sh:59`, `:116`, `:182`, `:190`, `:197`, `:204`, `:215`, `:218`, `:227`, `:233`, `:238`, `:243`, `:251`, `:259`, `:285`, `:290`, `:296`, `:301`, `:306`, `:311`, `:317`, `:323`, `:328`, `:373`, `:383`, `:387`, `:400`, `:408`, `:412`, `:416`, `:420`, `:424`, `:428`, `:461`, `:477`, `:504`, `:546`.

`deploy_agendado` agenda a si próprio em `/etc/cron.d`, sem `cron_run`, e chama a esteira da fatia recebida; o que essa fatia executa depende do argumento. (`bin/deploy_agendado.sh:54-56`, `bin/deploy_agendado.sh:68-73`; fatias efetivamente agendadas: **[nao verificado]**)

`keepalive` lê e envia texto ao tmux, sem lançamento direto no core; o efeito posterior dessa mensagem é **[nao verificado]**. (`bin/keepalive.sh:3-9`)

Um `docker exec` interno direto pode herdar um envelope externo: o render envolve tanto RAW quanto comandos Django. Assim, “sem envelope próprio” não demonstra execução real sem marca. (`app/config/crons.py:1861-1874`)

**3. Corrente e tetos**

O teto pertence ao **elo que espera o gatilho**: começa antes da consulta dos carimbos, verifica a cada 10 s e, faltando carimbo no teto, registra ALARME e retorna 2 sem executar o comando. O envelope só entra depois dessa espera. (`bin/encadeado.sh:86-104`)

| Elo que espera | Gatilho | Teto | Fonte |
|---|---|---:|---|
| `apurar_furos_diarios` | `processar_cartorio`, empresas 2,3,4 | 900 s | `app/config/crons.py:1504-1505`, `app/config/crons.py:1842` |
| `disparar_perguntas_competencia` | `apurar_furos_diarios` | 900 s | `app/config/crons.py:1516-1517` |
| `reconciliar_perguntas_orfas` | `disparar_perguntas_competencia` | 900 s | `app/config/crons.py:1521-1522` |
| `lavrar_deslogados` | `reconciliar_perguntas_orfas` | 900 s | `app/config/crons.py:1534-1535` |
| `lavrar_he_pendente` | `lavrar_deslogados` | **1800 s** | `app/config/crons.py:1549-1550` |
| `recusar_ponta_pequena` | `lavrar_he_pendente` | 900 s | `app/config/crons.py:1562-1563` |
| `vigiar_txt_dominio` | `recusar_ponta_pequena` | 900 s | `app/config/crons.py:1587-1588` |
| `lavrar_retrato_folha` | `vigiar_txt_dominio` | 900 s | `app/config/crons.py:1627-1628` |
| `vigiar_pausa_export` | `lavrar_retrato_folha` | 900 s | `app/config/crons.py:1651-1652` |
| `vigiar_retido_calado` | `vigiar_pausa_export` | 900 s | `app/config/crons.py:1657-1658` |
| `vigiar_fio_morto` | `vigiar_pausa_export` | 900 s | `app/config/crons.py:1713-1714` |
| `vigiar_carimbo_sem_registro` | `vigiar_fio_morto` | 900 s | `app/config/crons.py:1751-1752` |
| `vigiar_ausencia_x_veredito` | `vigiar_carimbo_sem_registro` | 900 s | `app/config/crons.py:1758-1759` |
| `vigia_pareador_celula` | `vigiar_ausencia_x_veredito` | 900 s | `app/config/crons.py:1764-1765` |
| `lavrar_cxr_do_motor` | `vigia_pareador_celula` | 900 s | `app/config/crons.py:1775-1776` |

**Não existe um atraso universal garantido para esperar deploy.** O menor orçamento nominal é 900 s, mas um dependente já esperando dispõe apenas do restante desse orçamento; o predecessor ainda precisa executar e produzir FIM. Os 1800 s de `lavrar_he_pendente` são sua espera por `lavrar_deslogados`; quem espera o término de HE tem 900 s. (`bin/encadeado.sh:86-96`, `bin/cron_run.sh:22-25`, `app/config/crons.py:1549-1563`; folga efetiva em produção: **[nao verificado]**)

A espera hipotética **dentro** do envelope não teria teto próprio no executor atual, pois ele já terminou sua espera antes de fazer `exec cron_run.sh`. (`bin/encadeado.sh:99-104`)

O documento chama `vigiar_retido_calado` de paralelo; ambos os vigias têm o mesmo gatilho, mas um runner percorre os elos sincronamente. Outros runners podem avançar quando encontram um elo ocupado, porque o lock por elo usa `-n`; assim, o código permite dependente esperando enquanto outro runner executa o predecessor. (`app/docs/CRONS.md:44-45`, `app/config/crons.py:1873-1885`, `bin/encadeado.sh:56-67`, `bin/encadeado.sh:78-79`)

Não há teto global da corrente: cada elo inicia seu relógio, e o runner continua após retorno não zero. (`bin/encadeado.sh:64-67`, `bin/encadeado.sh:86`)

**4. Travas existentes**

Deploy e envelope não adquirem uma trava compartilhada nesses caminhos: usam consulta de marcas e criação/remoção de marcas, respectivamente. (`bin/deploy.sh:108-118`, `bin/deploy.sh:338-342`, `bin/cron_run.sh:9-11`)

| Trava existente | Semântica |
|---|---|
| Testes: `/tmp/juliani_db_test.lock` | `flock -w`, padrão 3600 s, configurável; sem vez retorna 75. `bin/trava_teste.sh:21-38`. |
| Vaga: `logs/construtor.vaga` | `flock -n`, recusa imediatamente se ocupada. `bin/codex_pergunta.sh:231-233`. |
| Slots Codex | Gate bloqueante; tentativas de slot `-n`, depois `-w 5`; resposta usa `-w "$TIMEOUT"`. `bin/codex_pergunta.sh:341-369`. |
| Elo: `/tmp/encadeado_$CMD.lock` | `-n`; concorrente sai 0. `bin/encadeado.sh:78-79`. |
| Sombra: `logs/sombra.lock` | `-w`, padrão 1800 s; herança declarada evita reaquisição. `bin/sombra_trava.sh:16-28`, `bin/sombra_trava.sh:37-38`. |
| Raia e árvore | `flock` bloqueante, sem timeout declarado nessas aquisições. `bin/trava.sh:60-64`. |
| Régua | `-n`, salvo trava herdada; pre-push espera até 1800 s. `bin/regua.sh:97-103`, `bin/pre-push.sh:80-89`. |
| Cadeia do molde | Toma árvore e régua antes de chamar deploy; são travas do chamador. `bin/molde_fatia/cadeia.sh:11-14`, `bin/molde_fatia/cadeia.sh:48`. |
| Fabricante, integrador e vigias | Exclusões `-n`. `bin/fabricante.sh:28`, `bin/integrador.sh:9-10`, `bin/vigia_arvore.sh:24-25`, `bin/vigia_esteira.sh:9-10`, `bin/esteira_slots.sh:15`, `bin/vigia_logins.sh:71-72`. |
| Estado e relato | `-w 600`. `bin/codex_estado.sh:65-66`, `bin/relato.sh:27`. |
| `pista_lock.sh` | Consulta existência e depois escreve marcador; não é aquisição atômica por flock. `bin/pista_lock.sh:14-21`. |

O deploy chama `sombra.sh --conferir`, ramo que não adquire a trava da sombra; carregar a biblioteca não equivale a chamar `trancar_sombra`. (`bin/deploy.sh:162`, `bin/sombra.sh:87`, `bin/sombra.sh:438-463`)

**5. Cron dentro do deploy e possibilidade de deadlock** [nao verificado]

As chamadas diretas de `manage.py` do deploy são `migrate_schemas`, `migrate --check`, `collectstatic` e `shell` das provas; não foi identificada chamada direta a um elo de negócio da corrente. (`bin/deploy.sh:184`, `:194`, `:205`, `:276`, `:316`, `:397`; corrente declarada em `app/config/crons.py:1500-1778`)

O `sombra.sh --conferir` chamado pelo deploy consulta carimbo e existência de migrations na base da sombra; esse ramo não executa o bloco de crons. (`bin/deploy.sh:162`, `bin/sombra.sh:410-433`, `bin/sombra.sh:463`)

**O aninhamento explícito está no sentido inverso:** o reload das 03:30 é RAW envelopado → `cron_run.sh` cria marca → executa `deploy.sh --reload-agendado`. (`app/config/crons.py:326-327`, `app/config/crons.py:1475-1486`, `app/config/crons.py:1861-1863`, `bin/cron_run.sh:9-11`)

Se um futuro envelope mantiver trava durante o comando e esse deploy tentar adquirir uma trava incompatível, esse caminho permite autoespera. Da mesma forma, uma espera indiscriminada por todas as marcas incluiria a marca do próprio reload agendado. São consequências condicionais desse aninhamento; contrato da futura trava e efeitos internos das migrations efetivamente pendentes: **[nao verificado]**. (`bin/cron_run.sh:9-11`, `app/config/crons.py:326-327`)

**6. Testes de host e alcance** [nao verificado]

| Teste/selo | O que fixa | [nao verificado]
|---|---|
| `test_deploy_reload_gracioso.sh` | Normal/sem-migrate/sem-sombra/agendado usam HUP; exec simulado sobrevive; três cascas trocam workers; marca viva recusa restart; sem marca permite restart conjunto; motivo obrigatório; mensageria pendente recusa normal. `bin/tests/test_deploy_reload_gracioso.sh:107-156`. |
| `test_deploy_portoes_antes_do_migrate.sh` | Portões antes do apply, apply antes da publicação, modos sem apply e proteção contra variável herdada. `bin/tests/test_deploy_portoes_antes_do_migrate.sh:136-174`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:200-218`. |
| Mesmo teste, casos de cron/restart | Recusa com cron anterior ou surgido na casca; atualmente recusa core pendente antes de migrar; preserva restart só com mensageria pendente e explicita recusa quando ambos pendem. `bin/tests/test_deploy_portoes_antes_do_migrate.sh:176-198`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:220-230`. |
| `test_furo_encadeado_ao_cartorio.sh` | Presença textual de FIM no envelope, declaração sem minuto próprio, ordem dos três carimbos e teto retornando 2 com ALARME. `bin/tests/test_furo_encadeado_ao_cartorio.sh:25-42`, `bin/tests/test_furo_encadeado_ao_cartorio.sh:49-76`. |
| `contrato_deploy.sh` | Um HUP conjunto, restart manual com consulta de cron, rejeição da antiga espera seguida de restart, presença textual das marcas. `bin/contrato_deploy.sh:24-51`, `bin/contrato_deploy.sh:79-82`. |
| `test_trava_teste.sh` | Serialização e timeout 75 da trava de testes, sem demonstrar exclusão cron/deploy. `bin/tests/test_trava_teste.sh:117-137`. |

Os testes de deploy usam Docker falso e marcas plantadas pelo teste, sem executar o envelope real. No caso “cron nasceria no migrate”, o resultado exigido hoje é **zero migrate**; portanto esse nascimento não acontece no caso aprovado. (`bin/tests/test_deploy_reload_gracioso.sh:90-95`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:76-88`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:176-178`)

Há uma limitação adicional no teste do furo: ele passa `CRON_RUN_FAKE=1`, mas o executor atual chama o envelope absoluto sem consultar essa variável. (`bin/tests/test_furo_encadeado_ao_cartorio.sh:55`, `bin/encadeado.sh:102-104`)

Esses selos não demonstram impedir nascimento entre consulta e restart nem uma espera compartilhada entre deploy e envelope. Resultados atuais dos testes, crontab instalado e ocorrências reais em produção: **[nao verificado]**. (`bin/tests/test_deploy_reload_gracioso.sh:90-95`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:76-88`)

## 09/11 06:13 · sessao 74364eb50759 · - · - · origem=Ronald (tmux)

A corrente tem **15 elos** na declaração; o teto mínimo nominal de espera por dependência é **900 s (15 min)**; `lavrar_he_pendente` tem **1800 s (30 min)**. Esses tetos não são duração máxima do comando nem limite direto de espera dentro de `cron_run.sh`: o relógio começa em `bin/encadeado.sh:86`, espera carimbos em `bin/encadeado.sh:89`, alarma e sai 2 sem executar em `bin/encadeado.sh:92`, e somente depois chama o envelope em `bin/encadeado.sh:102`.

| Elo que espera | Gatilho | Teto | Fonte |
|---|---|---:|---|
| apurar_furos_diarios | processar_cartorio, empresas 2,3,4 | 900 s | `app/config/crons.py:1504`, `app/config/crons.py:1505`, `app/config/crons.py:1842` |
| disparar_perguntas_competencia | apurar_furos_diarios | 900 s | `app/config/crons.py:1516`, `app/config/crons.py:1517` |
| reconciliar_perguntas_orfas | disparar_perguntas_competencia | 900 s | `app/config/crons.py:1521`, `app/config/crons.py:1522` |
| lavrar_deslogados | reconciliar_perguntas_orfas | 900 s | `app/config/crons.py:1534`, `app/config/crons.py:1535` |
| lavrar_he_pendente | lavrar_deslogados | 1800 s | `app/config/crons.py:1549`, `app/config/crons.py:1550` |
| recusar_ponta_pequena | lavrar_he_pendente | 900 s | `app/config/crons.py:1562`, `app/config/crons.py:1563` |
| vigiar_txt_dominio | recusar_ponta_pequena | 900 s | `app/config/crons.py:1587`, `app/config/crons.py:1588` |
| lavrar_retrato_folha | vigiar_txt_dominio | 900 s | `app/config/crons.py:1627`, `app/config/crons.py:1628` |
| vigiar_pausa_export | lavrar_retrato_folha | 900 s | `app/config/crons.py:1651`, `app/config/crons.py:1652` |
| vigiar_retido_calado | vigiar_pausa_export | 900 s | `app/config/crons.py:1657`, `app/config/crons.py:1658` |
| vigiar_fio_morto | vigiar_pausa_export | 900 s | `app/config/crons.py:1713`, `app/config/crons.py:1714` |
| vigiar_carimbo_sem_registro | vigiar_fio_morto | 900 s | `app/config/crons.py:1751`, `app/config/crons.py:1752` |
| vigiar_ausencia_x_veredito | vigiar_carimbo_sem_registro | 900 s | `app/config/crons.py:1758`, `app/config/crons.py:1759` |
| vigia_pareador_celula | vigiar_ausencia_x_veredito | 900 s | `app/config/crons.py:1764`, `app/config/crons.py:1765` |
| lavrar_cxr_do_motor | vigia_pareador_celula | 900 s | `app/config/crons.py:1775`, `app/config/crons.py:1776` |

`corrente()` transmite **gatilho**, empresas e teto para o executor; `depende=` adicional é informação declarada, não uma lista extra de arquivos esperados pelo shell: `app/config/crons.py:1839`, `app/config/crons.py:1845`, `bin/encadeado.sh:71`, `bin/encadeado.sh:89`.

A folga disponível para um predecessor atrasado é o teto do dependente **menos o tempo já gasto esperando**, e deve incluir o tempo até terminar o comando e criar seu carimbo; somente rc 0 ou 2 produz esse carimbo. Um atraso de deploy de 900 s não tem garantia de caber, e a folga efetiva em produção é [nao verificado]. Fontes: `bin/encadeado.sh:86`, `bin/encadeado.sh:92`, `bin/cron_run.sh:22`, `bin/cron_run.sh:25`.

Há uma ressalva essencial à descrição de “paralelo” do documento: `CRONS.md` põe `vigiar_retido_calado` no mesmo gatilho que `vigiar_fio_morto`, mas o executor percorre a lista **sincronamente, um elo por vez**; o segundo não nasce em paralelo pelo mesmo runner. Fontes: `app/docs/CRONS.md:44`, `app/config/crons.py:1657`, `app/config/crons.py:1713`, `bin/encadeado.sh:56`, `bin/encadeado.sh:64`.

Entretanto, cada empresa do cartório dispara um runner; quando um elo já tem dono, o runner concorrente sai daquele elo pelo `flock -n` e avança ao seguinte. Assim, o código permite um dependente esperando carimbo enquanto outro runner executa — ou hipoteticamente aguarda deploy dentro do envelope do predecessor. Fontes: `app/config/crons.py:1873`, `app/config/crons.py:1885`, `bin/encadeado.sh:79`, `bin/encadeado.sh:64`, `bin/encadeado.sh:66`.

Não há um teto global da corrente: cada elo tem relógio próprio; após falha de um elo, o runner continua para os próximos, que podem esperar e alarmar pela ausência do carimbo. Fontes: `bin/encadeado.sh:64`, `bin/encadeado.sh:66`, `bin/encadeado.sh:86`.

Sobre deploy por dentro de cron e deadlock: [nao verificado]

- O deploy executa diretamente `migrate_schemas --check`, `--plan`, apply, `migrate --check` no mensageria, `collectstatic` no UI e `shell` nas provas de rota; não passa esses comandos por `cron_run.sh`. Fontes: `bin/deploy.sh:184`, `bin/deploy.sh:194`, `bin/deploy.sh:205`, `bin/deploy.sh:276`, `bin/deploy.sh:316`, `bin/deploy.sh:397`.
- O sentido inverso está explicitamente presente: **o deploy das 03:30 é um RAW envelopado**. `RAW(... deploy.sh --reload-agendado ...)` → `linhas()` → `_run()` → `RUNNER=cron_run.sh` → marca → execução do deploy. Fontes: `app/config/crons.py:326`, `app/config/crons.py:1863`, `app/config/crons.py:1486`, `app/config/crons.py:1475`, `bin/cron_run.sh:9`, `bin/cron_run.sh:11`.
- Portanto, **condicionalmente**, se um futuro envelope mantiver uma trava durante `CMD` e o deploy chamado por ele esperar uma trava incompatível, haverá autoespera/aninhamento; o contrato dessa futura trava é [nao verificado]. O encadeamento atual que torna essa condição relevante está em `app/config/crons.py:326`, `app/config/crons.py:1863`, `bin/cron_run.sh:11`.
- Igualmente, uma espera hipotética do deploy por todas as marcas incluiria sua própria marca no reload agendado, se não houvesse tratamento específico; hoje o modo agendado usa HUP e não chama a recusa exclusiva do `--restart`. Fontes: `bin/cron_run.sh:9`, `bin/deploy.sh:115`, `bin/deploy.sh:338`, `bin/deploy.sh:359`.
- `crons.sh check --medir` também é RAW envelopado e chama `manage.py crons render` diretamente; o deploy não o chama pelo selo: o selo apenas lê `crontab -l`. Fontes: `app/config/crons.py:318`, `bin/crons.sh:27`, `bin/selo_reciclagem.sh:72`.
- O `sombra.sh --conferir` invocado pelo deploy lê o carimbo e consulta a base da sombra, sem rodar o bloco; a invocação atual não contém cron de negócio aninhado. Fontes: `bin/deploy.sh:162`, `bin/sombra.sh:410`, `bin/sombra.sh:429`, `bin/sombra.sh:433`.

## 09/11 06:12 · sessao 6ea44879d18f · - · - · origem=Ronald (tmux)

Censo somente pela cópia; execução/instalação em produção [nao verificado].

**Diretórios operacionais**

| Caminho | Escritor/leitor efetivo |
|---|---|
| `logs/crons_em_curso/` | `bin/cron_run.sh:9` cria diretório e nome `${N}_${E}.$$`; `:10` grava início e instala `trap EXIT` que remove; `:11` executa o comando. |
| `logs/crons_em_curso/` | `bin/deploy.sh:108` enumera marcas; `:109` testa existência de `/proc/<sufixo>` e apaga marcas sem PID. `:111` recolhe vivos; chamadas em `:117` e `:341`, exclusivamente no ramo `RESTART=1` (`:115`, `:338`). |
| `logs/fim/` | `bin/cron_run.sh:22-25` cria diretório/grava `${N}.${E}.<dia>` somente quando RC é 0 ou 2. |
| `logs/fim/` | `bin/encadeado.sh:74-75` resolve/cria diretório; `:82` lê carimbo próprio para idempotência; `:89` lê carimbos de todos os gatilhos; `:92-96` aplica teto e polling de 10 s. |
| Declaração | `app/config/crons.py:1475-1486` define runner e gera envelope; `:1845-1846` exporta configuração dos elos; `:1873-1885` anexa corrente após envelope do gatilho. Não são acessos diretos aos diretórios. |

As outras ocorrências encontradas em Python de aplicação são documentação/comentários, por exemplo `app/chamados/management/commands/vigiar_fio_morto.py:18`; não acesso ao arquivo. A conclusão negativa fica restrita ao censo textual realizado [nao verificado].

**Escritores/leitores nos testes e selos** [nao verificado]

- `bin/tests/test_deploy_reload_gracioso.sh:34` cria diretório de fixture; `:91` grava marca viva fictícia.
- `bin/tests/test_deploy_portoes_antes_do_migrate.sh:63` cria fixture; `:76`, `:88`, `:110` plantam marcas simulando nascimento na prova de casca, migrate ou antes do deploy.
- `bin/tests/test_furo_encadeado_ao_cartorio.sh:17-22` usa `logs/fim` da raiz calculada e remove carimbos de cartório/furo na entrada e no `trap`; `:49-51` grava três carimbos. Não executei esse teste.
- `bin/contrato_deploy.sh:81-82` só procura os textos nos scripts; não lê o diretório operacional.
- `bin/tests/test_furo_encadeado_ao_cartorio.sh:25` só procura `FIM_DIR` no runner.

**Entradas que podem lançar dentro do core sem criar envelope próprio**

Há diferença entre um script conter `docker exec` direto e ser necessariamente executado sem envelope: a linha renderizada de cron envolve tanto RAW como comandos (`app/config/crons.py:1861-1874`); o mesmo script chamado manualmente não ganha essa marca automaticamente (`bin/cron_run.sh:9-11`).

| Entrada | Evidência |
|---|---|
| Deploy: checks, plano, migrate, espera e prova | `bin/deploy.sh:184`, `:205`, `:316`, `:381`, `:397`; prova do core em `:415`. |
| Render da corrente antes de entrar no runner dos elos | `bin/encadeado.sh:45`; os comandos dos elos efetivamente passam pelo runner em `:102`, `:104`. |
| Render/install do crontab | `bin/crons.sh:24`, `:27`. |
| Sombra: leitura da lavra de produção e cálculo da janela | `bin/sombra.sh:138-144`, `:466-473`. Ensaio principal usa outro container via `docker run` em `:183`, e não `saas_core`. |
| Lavrar deslogados | `bin/lavrar_deslogados.sh:18`. |
| Lavrar abriu/não bateu | `bin/lavrar_abriu_nao_bateu.sh:23`. |
| Gate de cobrança | `bin/gate_cobranca.sh:37`, `:40`. |
| Alarmes de árvore | `bin/vigia_arvore.sh:173`, `:182`. |
| Alarme sem fatia | `bin/alarme_sem_fatia.sh:90`. |
| Snapshot da fábrica | `bin/fabrica_estado.py:64-66`, acionado com `--lavrar` em `:71-72`. |
| Push de sessão ociosa | `bin/alarme_sessao_ociosa.py:140-149`. |
| Relato de nomes | `bin/relato_nomes.sh:18`. |
| Diagrama | `bin/gerar_diagrama.py:15`. |
| Verificador Node, com core por padrão | `bin/node_check.sh:24`, `:37`. |
| Selo de reciclagem, `gunicorn --print-config` | `bin/selo_reciclagem.sh:34-45`. |
| Placar | Lista detalhada abaixo. |
| Esteira, indiretamente | `bin/esteira.sh:16-19` importa `core.esteira_vigia`; sua `_pauta` faz `docker exec saas_core ... alarme_esteira` em `app/core/esteira_vigia.py:894-896`. `bin/vigia_esteira.sh:11` também executa essa biblioteca no host. |

`bin/placar_code.sh` faz execs diretos nas linhas **59, 116, 182, 190, 197, 204, 215, 218, 227, 233, 238, 243, 251, 259, 285, 290, 296, 301, 306, 311, 317, 323, 328, 373, 383, 387, 400, 408, 412, 416, 420, 424, 428, 461, 477, 504, 546**. A linha 116 chama `fechar_cobranca_com_lastro`; as outras incluem comandos e shells de contadores, devendo ser citadas como entradas diretas, sem presumir execução real nem ausência de efeitos de cada command.

`bin/deploy_agendado.sh` não faz exec direto no core: agenda sua própria chamada em `/etc/cron.d` sem `cron_run` (`:54-56`) e executa `bash "$f/esteira.sh"` (`:73`). O comportamento final depende da fatia passada [nao verificado].

`bin/keepalive.sh:3-9` lê/envia teclas pelo tmux; não contém lançamento direto de comando no core. Efeito posterior do texto enviado à sessão [nao verificado].

`bin/gerar_mapa_juizes.py:5-6` e `bin/censo_folha.py:2` têm exemplos documentados de exec no core; não devem ser contados como exec disparado pelo próprio Python. `bin/snapshot_o263_comp10.py:74` monta instrução textual de restauração.

**Observação relevante para exclusão**

O runner mantém a marca até o término de toda sua própria execução, inclusive alerta/recuperação de `cron_quebrado`, porque o `trap` é EXIT (`bin/cron_run.sh:10`) e os execs de alarme ocorrem em `:33`, `:35`; o carimbo FIM é escrito antes desses execs (`:22-25`). Portanto dependente pode ver FIM enquanto a marca do envelope anterior ainda existe (`bin/cron_run.sh:25`, `:33-38`).

## 09/11 06:12 · sessao 87c44fd999e1 · - · - · origem=Ronald (tmux)

4. **Travas existentes e relação com deploy/envelope** [nao verificado]

- `deploy.sh` consulta marcas e remove marca com PID ausente; não toma trava para essa consulta (`bin/deploy.sh:108-117`, `bin/deploy.sh:338-342`). O envelope cria marca e executa imediatamente, sem lock (`bin/cron_run.sh:9-11`).
- `trava_teste.sh`: `/tmp/juliani_db_test.lock`, espera limitada `flock -w "$ESPERA"`; padrão 3600 s, parametrizável `--espera`; falha de aquisição retorna 75 e não executa comando (`bin/trava_teste.sh:21-38`, `bin/trava_teste.sh:42`).
- Vaga do construtor: deriva de `<repo vivo>/logs/construtor.vaga`, abre append e tenta `flock -n 6`; ocupada causa recusa imediata (`bin/codex_pergunta.sh:76`, `bin/codex_pergunta.sh:231-233`).
- Slots Codex: gate bloqueante `flock 9`, slots por tentativa `-n`, espera em slot escolhido `-w 5`; gate solto após adquirir slot (`bin/codex_pergunta.sh:341-357`). Trava da resposta usa `-w "$TIMEOUT"` (`bin/codex_pergunta.sh:367-369`).
- Corrente: `/tmp/encadeado_$CMD.lock`, `flock -n`; concorrente sai 0, sem esperar (`bin/encadeado.sh:78-79`).
- Sombra: `logs/sombra.lock`, `-w`, padrão 1800 s; filhos herdam descritor e `SOMBRA_TRAVA_HERDADA=1` evita reaquisição (`bin/sombra_trava.sh:16-28`, `bin/sombra_trava.sh:37-38`). O deploy chama `sombra.sh --conferir`, enquanto os ramos que tomam essa trava são `--refazer`, `--bloco`, `--com-a-sombra`, `--rodar` (`bin/deploy.sh:162`, `bin/sombra.sh:438`, `bin/sombra.sh:451`, `bin/sombra.sh:456`, `bin/sombra.sh:487`).
- Árvore/raia: `/tmp/trava_$RAIA.lock` e `/tmp/cadeia.lock`, ambos bloqueantes, sem `-n/-w`; após esperar árvore confere portões novamente e pode devolver descritor (`bin/trava.sh:60-73`).
- Régua: aquisição `-n`, salvo `REGUA_TRAVA_HERDADA=1` (`bin/regua.sh:97-103`). Pre-push usa a mesma `/tmp/regua.lock`, com `-w 1800`, respeitando herança (`bin/pre-push.sh:80-89`). Cadeias dos moldes tomam bloqueante (`bin/molde_fatia/cadeia.sh:14`, `bin/molde_fatia/cadeia.sh:88`, `bin/molde_autocura/cadeia.sh:13`, `bin/molde_autocura/cadeia.sh:85`).
- Outras exclusões `-n`: fabricante, integrador, vigia árvore, vigia esteira, esteira slots, moldes esteira, vigia logins (`bin/fabricante.sh:28`, `bin/integrador.sh:9-10`, `bin/vigia_arvore.sh:24-25`, `bin/vigia_esteira.sh:9-10`, `bin/esteira_slots.sh:15`, `bin/molde_fatia/esteira.sh:7`, `bin/molde_autocura/esteira.sh:6`, `bin/vigia_logins.sh:71-72`). Relato/estado usam `-w 600` (`bin/relato.sh:27`, `bin/codex_estado.sh:65-66`).
- `pista_lock.sh` é marcador consultado com `-e`, seguido de escrita; não é flock nem aquisição atômica entre a consulta e a escrita (`bin/pista_lock.sh:14-21`).

6. **Testes de host encontrados — lidos, não executados** [nao verificado]

- `test_deploy_reload_gracioso.sh` roda cópia do deploy com Docker falso e exec `sleep` sobrevivente; marca de cron é criada pelo próprio teste, sem `cron_run.sh` (`bin/tests/test_deploy_reload_gracioso.sh:33-42`, `bin/tests/test_deploy_reload_gracioso.sh:89-95`). Fixa normal/sem-migrate/sem-sombra/agendado usando HUP, sobrevivência do exec e troca das três gerações de workers; normal com marca também usa HUP (`bin/tests/test_deploy_reload_gracioso.sh:107-123`). Fixa restart recusado com marca, restart conjunto sem marca, exigência de motivo e recusa de mensageria pendente no normal (`bin/tests/test_deploy_reload_gracioso.sh:125-156`).
- `test_deploy_portoes_antes_do_migrate.sh`: Docker/portões falsos podem criar marca antes do deploy, na prova de casca ou no migrate (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:76`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:88`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:110`). Caso 11 atual exige `--restart` com core pendente recusar **antes** do apply: zero migrate/restart; consequentemente o cron configurado para nascer no migrate não chega a nascer (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:176-178`). Casos 12–16 cobram recusas por cron anterior/intermediário; casos 22–24 preservam restart com apenas mensageria pendente, restart sem pendências e recusa explícita quando ambos pendem (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:180-198`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:220-230`).
- Esse teste também fixa migrate após auth/sombra/casca e antes do HUP; recusa de portões, inicialização contra ambiente herdado e modos sem apply (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:136-174`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:200-218`).
- `test_furo_encadeado_ao_cartorio.sh`: apenas verifica textualmente `FIM_DIR` no envelope; cobra declaração ENC sem minuto, chegada ordenada dos três carimbos, teto com retorno 2 e texto ALARME (`bin/tests/test_furo_encadeado_ao_cartorio.sh:25-42`, `bin/tests/test_furo_encadeado_ao_cartorio.sh:49-61`, `bin/tests/test_furo_encadeado_ao_cartorio.sh:66-76`). Atenção: passa `CRON_RUN_FAKE=1`, mas o encadeado atual executa o envelope absoluto sem consultar essa variável; portanto não há isolamento comprovado do envelope nesse caminho (`bin/tests/test_furo_encadeado_ao_cartorio.sh:55`, `bin/encadeado.sh:102-104`).
- `test_trava_teste.sh` cobre serialização A→A_fim→B e timeout retornando 75, para trava de testes isolada, não para cron/deploy (`bin/tests/test_trava_teste.sh:117-137`).
- Os testes de deploy encontrados não executam o envelope real nem demonstram impedir nascimento entre a última consulta de marcas e o restart; exercitam marcas sintéticas e recusa/amostragem (`bin/tests/test_deploy_reload_gracioso.sh:90-95`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:76`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:88`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:110-111`). Resultado atual desses testes e execução em produção: [nao verificado].

## 09/11 05:54 · sessao d3b19c436eee · SELOS-CANON-I1 · censo · origem=Code

Numeração dos anexos: primeira linha após cada marcador `== anexo: ... ==` = linha 1.

A última pergunta examina `eec4007f`; o RELATO registra a volta 4 “sem pre-verif v4”. Assim, **tema perguntado não significa código final examinado**. (`i1_preverif_v3.q:1`; `i1_relato.txt:3`.)

1. **Pertencimento à fila por célula crua — (a): tema presente; escape específico não apontado.**

   **Pergunta:** “Ha uma autoridade unica de ‘esta obra e fila 1’?” (`i1_preverif_v1.q:17`.)

   **Resposta:** “**Nesta costura, não.** `_fila1_viva` chama diretamente `_itens_andaveis`”. A resposta examinou compartilhamento da autoridade, sem apontar a incompatibilidade entre célula qualificada e identidade extraída. (`i1_preverif_v1.r:64`; `i1_opus_final.md:2`.)

   A v3 viu uma incompatibilidade próxima: “`_obra_de` admite `O9 (C)`, enquanto glob e publicação PAREI rejeitam espaço/parênteses”; a triagem a marcou **PROCEDENTE-menor**, sem registrar o escape por pertencimento. (`i1_preverif_v3.r:60`; `i1_triagem_v3.txt:4`.) Na cópia, a devolução da célula inteira e a comparação direta permanecem demonstráveis. (`bin/hook_stop_fila1.py:242`; `bin/disjuntor.py:1647`; `bin/disjuntor.py:2213`.)

2. **CORTES descreve outra forma de espera — (e): NÃO PERGUNTADO especificamente.**

   **Pergunta mais próxima:** “O diff realiza o aval literal anexo?” (`i1_preverif_v1.q:7`.)

   **Resposta correspondente:** “Na tabela, ‘aval I-1’ refere-se ao parágrafo fornecido no anexo `aval_selos_do_canon_0811.txt`”. Isso delimita uma conferência do aval; não relata comparação entre a redação de CORTES e a forma viva. (`i1_preverif_v1.r:7`.)

   As perguntas fornecidas não solicitam essa comparação documental; não há trecho específico de resposta identificando a divergência narrada pelo opus. (`i1_preverif_v1.q:7-27`; `i1_preverif_v2.q:7-22`; `i1_preverif_v3.q:7-21`; `i1_opus_final.md:3`.) A referência a `CORTES.md:428` fica como referência do laudo, sem confirmação independente dessa linha nesta cópia: **[nao verificado]**.

3. **PAREI persistente, handoff e leitura usada como prova — classificação composta.**

   **Retirar a linha quando o censo termina: (e).** A pergunta sobre o topo foi: “A linha inserida pode deslocar ou mascarar a linha 2 ESPERANDO/PAREI [...] ou a linha do B3?”. A resposta tratou exatamente do deslocamento: “`no_topo` acrescenta uma linha acima do painel vigente, preservando todas as anteriores”. Não houve pergunta específica sobre encerramento e retirada do PAREI. (`i1_preverif_v3.q:8-10`; `i1_preverif_v3.r:16`; perguntas completas em `i1_preverif_v1.q:7-27`, `i1_preverif_v2.q:7-22`, `i1_preverif_v3.q:7-21`.)

   **Papel leitura bastar para provar censo: (a).** A pergunta solicitou examinar “o predicado de ESPERANDO provado”. A resposta descreveu que ele exige “obra exata e papel censo/leitura”, sem apontar como defeito uma leitura viva substituir censo em curso. (`i1_preverif_v1.q:19-21`; `i1_preverif_v1.r:76`.) Essa aceitação está explícita na cópia. (`bin/hook_stop_fila1.py:714`; `bin/hook_stop_fila1.py:751`.)

   **Handoff após o encerramento: (a), quanto ao caso final.** O handoff estava expressamente incluído no censo dos leitores; a resposta descreveu sua prova pela primeira linha, sem identificar o caso final de PAREI remanescente. (`i1_preverif_v3.q:8-10`; `i1_preverif_v3.r:29`; `i1_opus_final.md:4`.)

4. **Literal repetido e `%r` contaminando a identidade da espera — (a), quanto ao defeito semântico específico.**

   **Pergunta:** “QUEM ESCREVE e QUEM LE cada linha de trilha [...] algum leitor supoe outro significado”. (`i1_preverif_v3.q:17-18`.)

   **Resposta:** “Conta negativas; abre intervalo por obra somente quando reconhece `construtor de … sem CENSO`”; sobre PAREI, afirmou que os leitores “compartilham reconhecedor, porém selecionam recortes diferentes”. Não apontou a aspa capturada nem a repetição final do literal. (`i1_preverif_v3.r:89`; `i1_preverif_v3.r:91`.)

   A costura final é verificável: a negativa usa `%r`, enquanto o leitor captura `(\S+)`; os dois literais citados pelo opus também existem. (`bin/disjuntor.py:1650`; `bin/disjuntor.py:2041`; `bin/hook_stop_fila1.py:420`; `bin/hook_stop_fila1.py:769`.) Essa implementação final sucedeu a última pre-verificação fornecida. (`i1_relato.txt:3`.)

5. **Segundo reconhecedor cru mais largo — (a) pelo tema; implementação introduzida depois da última pergunta.**

   **Pergunta:** “Fail-closed: excecao que deixa abrir? negacao espuria permanente de algo que hoje passa”. (`i1_preverif_v3.q:13-14`.)

   **Resposta:** “A guarda de import e o fallback cru procuram literalmente `"Agent"` e `/wt-`”. O problema então apontado era a representação escapada do JSON; não era o reconhecedor final mais largo e sua negativa sem trava nomeada. (`i1_preverif_v3.r:56`; `i1_opus_final.md:6`.)

   Na cópia, `_cru_abre_agent_copia` se declara da **volta 4** e, para texto ilegível, aceita qualquer `Agent` **ou** `wt-`; `_aberturas_construtor` exige outra combinação de condições. Portanto, há tema perguntado, mas não evidência de que o pre-verificador examinou essa implementação final. (`bin/disjuntor.py:67-84`; `bin/disjuntor.py:1683-1686`; `i1_relato.txt:3`.)

6. **“Descartados” — exige classificação por subitem; o histórico não documenta um descarte uniforme.** (`i1_opus_final.md:7`; `i1_triagem_v2.txt:6-12`; `i1_triagem_v3.txt:7-9`.)

   - **Escritores do RELATO fora da trava: (b).**  
     **Pergunta:** “QUEM MAIS escreve e le o topo do RELATO”. (`i1_preverif_v3.q:8`.)  
     **Resposta:** “`gerar_cortes` e `deploy_agendado` leem e regravam o RELATO inteiro diretamente”. (`i1_preverif_v3.r:38`.)   [nao verificado]
     **Motivo do descarte:** “escritores anteriores a obra, fora do aval I-1”. (`i1_triagem_v3.txt:7`.)

   - **`codex exec` direto: (b).**  
     **Pergunta:** “liste TODAS as formas de abrir um construtor [...] quais escapam”. (`i1_preverif_v1.q:11-13`.)  
     **Resposta:** “Uma chamada direta a `codex exec`, sem wrapper, não alcança essas portas de censo”. (`i1_preverif_v3.r:50`.)  
     **Motivo do descarte:** “abertura fora do wrapper e rota proibida por conduta (CODEX 1)”. (`i1_triagem_v2.txt:9`.)

   - **`SyntaxError` antes da guarda: visto e encaminhado como resíduo; corresponde a (b) com essa ressalva documental.**  
     **Pergunta:** “excecao fora do try de main_pretool, import que falha antes”. (`i1_preverif_v2.q:11-12`.)  
     **Resposta:** “`SyntaxError` no próprio módulo precede a instalação da guarda”. (`i1_preverif_v3.r:58`.)  
     **Decisão registrada:** “residuo declarado, sem cura possivel no proprio modulo. Vai ao opus.” A triagem não usa aqui a palavra **DESCARTADO**. (`i1_triagem_v3.txt:9`.)

   - **`COPIA_RE` não reconhecer `~/wt-*`: (a) para a grafia específica; antecedente genérico já visto.**  
     **Pergunta:** “liste TODAS as formas de abrir um construtor”. (`i1_preverif_v1.q:11`.)  
     **Resposta:** “Agent com cópia sem nome `wt-*`, caminho relativo ou cópia não citada [...] Não entra no ramo.” Portanto, a classe geral de caminhos já havia sido vista; `~/wt-*` não foi nomeado nessa resposta. (`i1_preverif_v1.r:40`.)  
     **Motivo de descarte pelo Code:** **[nao verificado]**. As triagens fornecidas não registram descarte nominal desse caso. (`i1_triagem_v1.txt:2-10`; `i1_triagem_v2.txt:2-12`; `i1_triagem_v3.txt:2-9`.)

   - **Agent com ID iniciado por dígito: (a) para esse caso específico; incompatibilidade de IDs já vista em termos próximos.**  
     **Pergunta:** pede “o predicado exato de ‘obra reconhecida’”. (`i1_preverif_v2.q:8-10`.)  
     **Resposta:** “O ID validado é `(?:[A-Z][A-Z0-9]*(?:-[A-Za-z0-9]+)+|[A-Z]+\d+[A-Z0-9]*)`”; a v3 observou que IDs como `ABC` continuam ilegíveis para o Agent. Nenhum desses trechos relata especificamente Agent com ID iniciado por dígito e censo fechado. (`i1_preverif_v2.r:38`; `i1_preverif_v3.r:60`.)  
     **Descarte específico pelo Code:** **[nao verificado]**. O registro sobre IDs foi **PROCEDENTE**, com alegação de cura, e não descarte. (`i1_triagem_v2.txt:7`.)

7. **Preservar os RED da worktree — (c): fora do alcance do material fornecido.**

   **Pergunta mais próxima:** “os 4 mutantes declarados [...] mordem de verdade (leia as assercoes)?”. Trata de mordida, sem pedir preservação dos arquivos RED. (`i1_preverif_v1.q:22-23`.)

   **Resposta:** “A execução integral dos selos também fica **[nao verificado]**; seus scripts criam arquivos e cópias.” (`i1_preverif_v1.r:3`.)

   O anexo final fornece a localização declarada dos dois logs e a orientação de copiá-los; não fornece seu conteúdo nem prova de preservação. Conteúdo e preservação dos RED: **[nao verificado]**. (`i1_opus_final.md:8`.)

## 09/11 05:24 · sessao bbeb3f354882 · PAR-NAO-ADJACENTE · verificador · origem=Code

**A v4 cura a inversão entre intervalo e pausa curta. A aplicação sobre atas já carimbadas e o RED integrado continuam sem certificação.** A cura está em [escala/utils.py:455](/home/ronald/codex_ws/d361dfee6b22a10d4dd4298b47e3912f3f91b4ae/app/escala/utils.py:455); o pulo por impressão igual continua em `app/ponto/services/cartorio.py:1358–1364`.

Executei em memória os 17 testes das duas classes sem banco, com adaptação de timezone: passaram. Isso não certifica os testes de integração; execução completa em Django: **[nao verificado]**. As classes estão em `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:50,218,313`.

1. **Achados do opus e cura na v4**

| Achado | Resultado |
|---|---|
| **1 — pausa curta veta o intervalo que desconta dinheiro** | **Curado na transcrição:** a pausa sobreposta sai antes da formação de `_pontas_par`, deixando o intervalo externo passar. Cura: `app/escala/utils.py:455–459,475–479`. Mordida real: teste aninhado curto em `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:182–194`; mordida por consultas simuladas, nos dois cruzamentos: `:287–310`. |
| **2 — sonda conta geometria, não impacto em ata/dinheiro** | **Continua aberto.** A v4 acrescenta asserções de papéis, alinhamento e minutos, mas elas não medem a frota: `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:88–105,182–194,397–410`. Os números da sonda e seu impacto efetivo: **[nao verificado]**. |
| **3 — alcance do deploy e rota de aplicação** | **Não curado pelo delta.** A declaração continua na versão 4, e célula em dia continua pulada sem `forcar`: `app/ponto/services/cartorio.py:132,1348–1364`. A porta aceita `--apply` e `--forcar`: `app/ponto/management/commands/processar_cartorio.py:27–29,65`. “Zero células” exige qualificar “já carimbadas, com os demais insumos iguais”; alcance efetivo e situação da competência: **[nao verificado]**. |
| **4 — RED falsamente rotulado e builder sem RED comprovado** | Os três prefixos indevidos foram retirados: `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:235,243,249`. O builder ainda tem o nome RED e asserções relevantes em `:397–410`; sua execução vermelha contra a base em Django permanece **[nao verificado]**. |
| **5 — “aninhado fica como hoje” e interno não adjacente** | A afirmação foi delimitada, e `_conflitos` restringe intervalos conflitantes ao casamento adjacente: `app/escala/utils.py:441–443,461–465`. O caso real preserva o interno adjacente (`teste:166–180`); os mocks agora preservam a resposta antiga de ambos soltos (`teste:266–285`). Esses testes caracterizam uma resposta; não resolvem a lei aberta entre dois intervalos. |

A cura **não recupera automaticamente a ata original de 660 minutos**. A ata já virada descreve o intervalo externo de duas horas; a transcrição passa a respeitar esse intervalo. O cenário externo produz 599 minutos, enquanto a fixture com intervalo `21:01→22:00` preserva 660: `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:88–105,367–370,389–410`. Uma relavra integrada da **ata já virada**, seguida de outra relavra para comprovar convergência desse cenário específico: **[nao verificado]**.

O mutante `_n = 1` ganhou mordida específica no terceiro par: `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:142–164`. O mérito restante das duas triagens resumidas no anexo, sem sua fundamentação completa: **[nao verificado]**.

2. **Fronteira inclusiva e casos de borda**

Os dois leitores dividem **a mesma lista de pares marcados** pelo piso: intervalo com duração `>= 900 s`; pausa com duração `< 900 s`. Eles próprios não excluem sobreposição: `app/ponto/turnos.py:499,544–546,565–566`.

Considerando intervalo `I=[20:00,22:00]`:

| Pausa curta | Pareador real | Transcrição v4 |
|---|---|---|
| **Só toca antes:** `[19:58,20:00]` | Os dois pares exigiriam uma ponta compartilhada ou dois objetos no mesmo timestamp. O produtor não reutiliza identidades; o modelo declara unicidade por colaborador/timestamp: `app/ponto/turnos.py:811–822,883–885`; `app/ponto/models.py:106`. | Se consultas simuladas fornecerem ambos, elimina a pausa pelo `ini <= z`: `app/escala/utils.py:456–457`. |
| **Só toca depois:** `[22:00,22:02]` | Mesma restrição de identidade/unicidade: `app/ponto/turnos.py:815–822,883–885`; `app/ponto/models.py:106`. | Se fornecidos ambos, elimina a pausa pelo `a <= fim`: `app/escala/utils.py:456–457`. |
| **Contida:** `[21:01,21:02]` | **Produzível:** externo declarado por `Xi`; interno formado pelos vizinhos residuais. As consultas devolvem, respectivamente, externo e interno: `app/ponto/turnos.py:811–816,819–883,545–546,565–566`. | Elimina a pausa curta e mantém o externo; caso real em `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:182–194`. |
| **Cruza uma ponta:** `[19:59,20:01]` ou `[21:59,22:01]` | Com timestamps únicos, **não nasce desse produtor**: declarados são pareados sequencialmente; o guloso só casa vizinhos físicos e não pode usar ponta declarada: `app/ponto/turnos.py:811–822,883–885`. | Se fornecidos por mock, elimina a pausa e mantém o intervalo: `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:287–310`. |

**A mesma batida não é ponta de dois pares produzidos:** as identidades declaradas são reservadas, e o guloso avança duas posições ao emitir. Na transcrição, `_fins` também impede reutilizar o fechamento como outra abertura: `app/ponto/turnos.py:815–822,883–885`; `app/escala/utils.py:468–478`.

O teste de timestamps repetidos demonstra reutilização de **índice `k`**, mas não contém pares; não demonstra uma batida em dois pares. Além disso, essa duplicação contraria a unicidade declarada: `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:249–255`; `app/ponto/models.py:106`. A constraint efetivamente instalada no banco de produção: **[nao verificado]**.

3. **Quem produz dois intervalos sobrepostos; há perda contra a base?**

**Existe entrada real:** intervalo externo declarado mais intervalo interno guloso, ambos acima do piso. O teste real produz `12:00→13:00` e `12:10→12:30`: `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:166–176`. A origem são as duas passadas de `_pares_marcados`: `app/ponto/turnos.py:811–816,819–885`.

Nesse arranjo:

- O externo encontra as pontas internas e é vetado: `app/escala/utils.py:475–476`.
- O interno guloso é fisicamente adjacente e passa por `_conflitos`: `app/escala/utils.py:464–465`; a conservação é verificada em `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:175–180`.

**Dois intervalos conflitantes podem ficar ambos soltos nos mocks**, mas esses exemplos também não tinham par adjacente na base reconstruída pelo diff fornecido: `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:266–285`. Comparação direta com o objeto Git `b57ce0a1`: **[nao verificado]**.

No domínio com timestamps únicos, **não há perda de intervalo adjacente causada por `_conflitos`**: um par adjacente antigo satisfaz `_pos(z)==_pos(a)+1`, e o teste de pontas internas percorre um intervalo vazio. São precisamente as guardas atuais de `app/escala/utils.py:465,475–476`.

Existência e frequência desses arranjos na frota: **[nao verificado]**.

4. **Quem escreve e quem lê**

| Valor | Escritor → leitor |
|---|---|
| `Batida.timestamp`, `tipo` | Registro em `app/ponto/registro_batida.py:152–158` → montagem de `locs` em `app/escala/utils.py:1332` e ordenação do turno em `:451`. |
| Papel da ata `E/S/X/Xi` | Mapa escrito em `app/ponto/turnos.py:1804–1806,1827–1828` → `_papel_da_ata` em `:904–909`; marcas aplicadas em `:1315–1325`. |
| Pares e classificação pelo piso | `_pares_marcados`, `app/ponto/turnos.py:811–885` → divisão em `:545–546,565–566` → transcrição em `app/escala/utils.py:455–458`; realizado lê intervalos em `app/ponto/turnos.py:756–759`. |
| `turno` | Consulta/indexação por `data_turno` em `app/escala/utils.py:1590–1592`; seleção em `:1602–1605` → matcher em `:1710–1712` → helper em `:605`. |
| `locs`, `consumed` | `locs` montado e deduplicado em `app/escala/utils.py:1332–1348`; consumo inicial em `:1349` → helper lê em `:448–453`; matcher escreve consumo em `:631–632`. |
| `_idx`, `_bats`, `_ks` | Construídos em `app/escala/utils.py:448–452` → guarda em `:453`, emissão em `:466–483`. `_idx` conserva o primeiro `k` de cada timestamp: `:450`. |
| `_intervalos`, `_pausas`, `_pares` | Construídos em `app/escala/utils.py:455–458` → pontas, conflitos e mapa inicial em `:459–465`. |
| `_pontas_par`, `_pos`, `_conflitos`, `_ini_par` | Escritos em `app/escala/utils.py:459–465` → consultados no reconhecimento dos pares em `:473–476`. |
| `_n`, `_fins`, `out`, `(k,papel)` | Inicializados e emitidos em `app/escala/utils.py:466–483` → três leitores lógicos abaixo. |
| Associação marco→`k` | Alinhador escreve em `app/escala/utils.py:567–591` → matcher transforma em células/órfãs em `:625–632`. |
| Células, órfãs e ata | Células em `app/escala/utils.py:628`; órfãs em `:630`; lâmpadas/ata em `app/ponto/services/cartorio.py:677–711` → lavra em `:1800–1802` → persistência em `app/ponto/portas/celula.py:1027–1035`. |
| Minutos realizados | Juiz calcula em `app/ponto/turnos.py:756–771` → builder consulta em `app/escala/utils.py:2054–2067`. |

Há **um chamador produtivo direto** de `_papeis_do_turno`: `app/escala/utils.py:605`. Seus **três leitores lógicos** são:

- Guarda L‑084: `app/escala/utils.py:614–620`.
- Alinhador, incluindo índice `k`, número e lado do par: `app/escala/utils.py:622,533–540`.
- Conservação de órfãs e consumo: `app/escala/utils.py:629–632`.

Há também três chamadas de teste: `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:76,233,407`.

Ata, Zona 5 e juiz da volta não são três chamadores diretos desse helper: os caminhos sem turno tentam `_papeis_por_alternancia`. As portas estão em `app/ponto/motor_calculo_v2.py:3062`, `app/escala/utils.py:427,605–607`.

5. **Classes que já reprovaram obras**

| Classe | Avaliação |
|---|---|
| **Segundo juiz** | Existe seleção local adicional, portanto “transcrição” não significa copiar todos os pares: `app/escala/utils.py:455–476`. O registro separa a autoridade de intervalo da autoridade de alinhamento: `app/core/juizes.py:200–202`. O diff altera esta última; não cria outro cálculo de minutos. |
| **Derivação paralela de fonte única** | A soma continua no juiz, consultando `intervalos_do_turno`: `app/ponto/turnos.py:756–771`; o builder lê essa resposta em `app/escala/utils.py:2054–2067`. No aninhado acima do piso, a projeção ainda não transporta ambos os intervalos: comportamento preservado em `teste:166–180`, enquanto o realizado soma os intervalos em `turnos.py:758–759`. |
| **Relógio fora da borda** | Não há nova leitura de `now()`/data corrente no trecho alterado; há conversão dos timestamps recebidos: `app/escala/utils.py:437–483`. |
| **RED/selo sem mordida** | Os três rótulos falsos foram retirados; os mutantes relevantes têm mordida descrita abaixo. RED integrado do builder permanece **[nao verificado]**. Referências: `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:142,182,235,243,249,266,287,397`. |
| **Campo com dois significados** | O veto de contato é inclusivo, enquanto o impedimento por pontas internas é estrito: são predicados distintos em `app/escala/utils.py:457,463,475–476`. O helper não deriva dia civil; o builder seleciona pelo `data_turno`: `:1591–1605`. A linha civil `:1829` já existia e apenas mudou de posição no diff fornecido. |

6. **Mutantes sobreviventes**

| Mutante | Ensaio dos 17 casos em memória | Mordida |
|---|---|---|
| `_pausas`: trocar ambos os `<=` por `<` | **Sobrevive** | Não há fixture de toque; expressão em `app/escala/utils.py:457`. |
| `_conflitos`: trocar ambos os `<=` por `<` | **Sobrevive** | Não há fixture de toque; expressão em `app/escala/utils.py:463`. |
| Remover filtro de `_pausas` | **Morre** | `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:182,287`. |
| Remover restrição de `_conflitos` | **Morre** | Somente o mock aninhado com interno não adjacente: `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:266`. |
| `_n += 1` → `_n = 1` | **Morre** | Terceiro par: `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:142–164`. |

Para **TODOS os testes do arquivo**, a sobrevivência dos comparadores estritos é sustentada pela inspeção das fixtures, sem execução completa: os casos adicionais não apresentam toque entre pares; o anexo declara `21:01:59→22:00`. Referências: `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:379–407,416,441,455,466,477,524,535,619,638,670`. Resultado completo em Django: **[nao verificado]**.

Esses sobreviventes podem ser **equivalentes no domínio que respeita a unicidade declarada**, pois contato exigiria compartilhar batida ou timestamp. Não constituem, isoladamente, prova de cobertura insuficiente nesse domínio: `app/ponto/turnos.py:811–822,883–885`; `app/ponto/models.py:106`.

7. **Catraca `LINHAS_REGISTRADAS`**

**Sim: os +8 do delta apontam às mesmas expressões.** São **+8 de v3 para v4**; o diff total fornecido indica **+9 contra a base**. Destinos registrados em `app/core/tests/test_medida_turno_do_par.py:247–249,281–282`; conferência direta pelo Git: **[nao verificado]**.

| v3 → v4 | Expressão preservada em `app/escala/utils.py` |
|---|---|
| 497 → **505** | `enumerate(zip(_ks[1:-1:2], _ks[2:-1:2]))` |
| 521 → **529** | `list(range(1, _M - 2, 2))` |
| 797 → **805** | `_idx_por_data.get(locs[_k][0].date())` |
| 1481 → **1489** | `min(_l[0].date() ...)` |
| 1501 → **1509** | busca com `_locs_i[_j][0].date() >= floor` |
| 1511 → **1519** | diferença `_locs_i[0][0].date() - _pre_ini` |
| 1524 → **1532** | `_locs_i[_anc_ix][0].date()` |
| 1535 → **1543** | filtro `_locs_i[k][0].date() == d` |
| 1821 → **1829** | `_idx_por_data.get(locs[_k][0].date())` |

A igualdade das linhas reais com `LINHAS_REGISTRADAS` passou no ensaio com `unittest`, substituindo a base Django; a asserção está em `app/core/tests/test_medida_turno_do_par.py:534–538`. Essa catraca verifica localização/identidade das ocorrências; não certifica a regra de conflito nem o impacto financeiro: `:528–538`.

_3043 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
