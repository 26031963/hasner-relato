# CODEX-ESTADO

> Ultima resposta processada: **28/11/2026 15:51** · publicado em **28/11/2026 15:52** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 28/11 15:51 · sessao 82560315f659 · CUSTO-DE-COORDENACAO-OBRA-PRONTA-ACORDA · leitura · origem=Code

A espera atual acorda por um `.rc` sem marca na entrada ou por qualquer `.rc` novo durante o laço. O filtro precisa valer nos dois pontos. `bin/disjuntor.py:2016–2022`  
A lease prova uma chamada admitida do Codex. Para opus, há declaração de raia e notificação de término, mas o vínculo exato entre um `.rc` intermediário e seu agente ficou [nao verificado]. `bin/codex_admissao.py:2–30`, `bin/raias_estado.py:13–21`, `bin/disjuntor.py:474–490`  
O marcador da cópia aponta para o plano `ORDEM-VIVA-2811-1450`. O hook resolve esse plano, e a próxima obra executável vem do adendo. `app/docs/BACKLOG.md:8,1932–1933`, `bin/hook_stop_fila1.py:246–284,332–343`  
O encaixe é no leitor e na espera existentes. O prazo de dez minutos depende de um início de ociosidade por obra, cuja fonte ficou [nao verificado]. `app/docs/BACKLOG.md:2264`, `bin/rc_sem_colher.py:50–83`  
O RELATO registra o pouso `824500282` e põe esta fatia em seguida na raia instrumento. A construção integral ainda tem três perguntas de corte. `app/docs/RELATO.md:15396–15399`

## 1. Mapa da espera

- **`RC SEM COLHER`.** `sem_colher` percorre pastas `logs/<OBRA>` com ID em caixa alta, dígitos e hífen e recência de 24 h. Considera o `.rc` regular mais novo da obra, exige idade estritamente maior que `--min-idade` e o exclui se houver arquivo não `.rc` mais de 5 s posterior ou marca `colhido|superado` com **obra, caminho relativo e HH:MM local do mtime**. O CLI devolve `3` quando lista e `0` quando vazio. Erro de leitura sobe. `bin/rc_sem_colher.py:13–14,21–36,46–83,86–107`
- **`ESPERA_UNICA`.** Cria a referência de tempo antes da consulta. O `3` do leitor pula o laço e o comando completo sai com sucesso. Com `0`, o laço procura `.rc` por `ctime` a cada 5 s por até 300 s, inclusive publicação por rename. A listagem final repete o `find` bruto. Assim, filtrar somente a entrada deixa um intermediário novo acordar pelo laço. Deixar o `3` bruto na entrada faz sucessivas esperas voltarem imediatamente pelo mesmo intermediário. `bin/disjuntor.py:2015–2022`, `bin/tests/test_rc_sem_colher.sh:75–103`
- **Demais leitores.** `vercodex` é documentado com `--min-idade 900`, que exclui idades de até 900 s. O RELATO registra uma versão de `~/vercodex.py` que contava todo o stdout, portanto a saída padrão merece compatibilidade. O executável de `vercodex` e o corpo de `logs/espera_unica.sh` ficaram [nao verificado]. A cópia documenta esse shell como espera de `.rc` ou 5 min. O fluxo atual do Stop consulta painel, prova de espera e fila, sem chamada ao leitor de RC nesse caminho. `bin/rc_sem_colher.py:2–4,76,96–107`, `app/docs/RELATO.md:13886,14467–14471,14493`, `bin/hook_stop_fila1.py:1700–1747`

## 2. Censo dos fatos

| Fato | Escritor e leitores | Significado que a espera pode usar |
|---|---|---|
| **RC e marca** | O produtor publica o `.rc`. No Codex construtor, o trap do wrapper publica o sidecar com `chamada=`, `rc=` e `fim=` por rename. A sessão registra a colheita no RELATO após ler o RC individualmente. `sem_colher` lê arquivo e marca. `bin/codex_pergunta.sh:566–584`, `bin/rc_sem_colher.py:26–43,71–82`, `app/docs/RELATO.md:15396` | O sidecar do wrapper é **fim daquela chamada**, mesmo durante a breve janela em que a lease segue presa. Um `.rc` genérico fornece o resultado numérico, sem identidade de produtor ou etapa no contrato do leitor. `bin/codex_pergunta.sh:577–584`, `bin/rc_sem_colher.py:39–43` |
| **Codex construtor** | O titular `codex_admissao.py` toma os flocks da vaga e do diretório da wt e passa os descritores ao executor. `codex_pergunta.sh` escreve a projeção `vaga.k.json` com obra, wt e ID de chamada e a trilha `vagas.log`, lida por `codex_vagas_estado.py` e pelo ESTADO. `bin/codex_admissao.py:188–223,260–271,296–308`, `bin/codex_pergunta.sh:295–314,552–558`, `bin/codex_vagas_estado.py:4–10`, `bin/gerar_estado.py:278–286` | O kernel prova ocupação. A projeção dá identidade diagnóstica e a trilha dá histórico. Um `abriu` sem `fechou` ou um JSON antigo junto de uma vaga retomada não prova a mesma chamada. O flock da wt prova admissão naquela wt pelo titular, com a associação à obra a conferir. O ramo Codex de leitura usa slots, sem essa lease de construtor. `bin/codex_admissao.py:4–7,20–30`, `bin/codex_pergunta.sh:510–555,679–703,781–800` |
| **Opus construtor** | `raia_agente.sh` é o escritor canônico de `OBRA`, `QUEM`, `DESDE` e `FALTA`, com baixa explícita. `raias_estado.py` lê para o ESTADO. O harness escreve lançamento e término no transcript principal. `disjuntor.agentes_sem_entrega` lê esses eventos, e o Stop o usa para CLEAR e B1. A admissão já resolve a obra pelo prompt ou pela cópia. `bin/raia_agente.sh:40–81`, `bin/raias_estado.py:13–21,51–70,125–146`, `bin/gerar_estado.py:213–215`, `bin/disjuntor.py:474–490,1959–1972`, `bin/hook_stop_fila1.py:1138–1148,1515–1519` | Na raia, “vivo” é **declarado e ainda não baixado**. No transcript, é **lançado e ainda sem notificação terminal**. A associação do `.rc` ao `agent_id` e a cobertura de morte sem notificação ficaram [nao verificado]. `bin/raias_estado.py:16–21`, `bin/disjuntor.py:474–490`, `bin/rc_sem_colher.py:39–43` |
| **VM suite** | `trava_teste.sh` toma o flock global e grava `.quem`. `suite.sh`, régua e pre-push usam a trava. `--quem` lê o texto, e o handoff testa o flock. `bin/trava_teste.sh:21–42`, `bin/suite.sh:539–564`, `bin/regua.sh:162–165`, `bin/pre-push.sh:142`, `bin/handoff_sessao.sh:261–279` | O flock prova que o recurso está tomado. `.quem` é rótulo e PID do chamador, atualizado na saída normal. A trava sozinha não identifica obra nem pedido que aguarda vez. `bin/trava_teste.sh:21–42`, `bin/suite.sh:559–564` |
| **Pista3** | O despacho documentado vincula obra, job e caminho do resultado. `pede` aceito dá ID, `estado` distingue fila, execução e fim, e o lançador remoto escreve o recibo final que `pista_certifica.sh` confere. O código da cópia ainda consulta o host `pista` fixo. O escritor remoto do `estado` e um índice uniforme obra → `(host, id, sha)` ficaram [nao verificado]. `app/docs/RELATO.md:14875–14882,15318`, `app/docs/BACKLOG.md:1931,2202`, `bin/suite.sh:379–390,442–461`, `bin/pista_certifica.sh:9–17` | Para evitar despacho duplicado, a evidência seria o **job exato** aceito e em fila ou rodando no host de origem. O recibo com `fim=` prova término. Ausência de recibo, por si só, deixa o estado [nao verificado]. `bin/suite.sh:383–390,446–461`, `bin/pista_certifica.sh:13–17` |
| **`ESPERANDO` no RELATO** | A sessão declara no painel. O wrapper construtor fornece a frase no stderr. O Stop verifica produtor/PID por `/proc`, executável, argv e starttime, e exige prova de cada campo declarado. A prova por arquivo compara avanço entre leituras e usa memória própria do hook. `CLAUDE.md:275–279`, `bin/codex_pergunta.sh:741–746`, `bin/hook_stop_fila1.py:631–680,693–739,837–887` | Prova pontual de produtor para devolver o turno. O painel cobre uma espera declarada e o formato não traz vínculo geral obra → agente → `.rc`. `bin/hook_stop_fila1.py:500–510,861–885` |

## 3. ORDEM VIVA, `PAREI` e encaixe

O marcador é mantido à mão e preservado pelo gerador. Stop, handoff, `backlog_estado`, `interrompidas_estado` e o selo O158 reutilizam o juiz do hook. Na cópia, `1450` substitui `1405` e herda sua raia instrumento. `_proximo_da_fila` devolve o ID do plano e texto truncado. `_itens_andaveis` enumera a tabela em ordem física. Para avisar “obra e próximo passo”, a fonte é o adendo apontado pelo marcador, com seus pré-requisitos. Um leitor já existente da sequência executável por raia ficou [nao verificado]. `bin/gerar_backlog.py:113–121`, `bin/gerar_estado.py:233–234`, `bin/handoff_sessao.sh:58–86`, `bin/backlog_estado.py:56–62`, `bin/interrompidas_estado.py:85–94`, `bin/tests/test_hook_nao_cobra_congelado.sh:105–151`, `app/docs/BACKLOG.md:8,1932–1933`, `bin/hook_stop_fila1.py:246–309,332–343,368–378`, `CLAUDE.md:451`

O `PAREI` comum é declaração da sessão no topo do RELATO. O hook julga se o `PAREI: !` trava o ID inteiro do item em curso, com guardas para CLEAR, COTA e SONDA. `_relato_parou` responde à pergunta mais ampla de devolver o turno. Há escritor próprio para `PAREI: censo-<OBRA>`, via `esteira_vigia.no_topo`, com prova do wrapper de censo compartilhada por Stop, handoff e alarme. O alarme mede idade da **sessão** e busca `PAREI` numa janela maior. Para o prazo desta fatia, o início de um intervalo contínuo sem job e sem trava daquela obra ficou [nao verificado]. `CLAUDE.md:275–285`, `bin/hook_stop_fila1.py:462–470,516–538,779–834,1533–1616`, `bin/disjuntor.py:311–338`, `bin/handoff_sessao.sh:133–152`, `bin/alarme_sessao_ociosa.py:119–155`

**Desenho proposto.** Dar ao leitor existente um modo explícito para a espera e preservar o inventário padrão. Aplicar a mesma decisão na entrada, aos candidatos novos por `ctime` e à saída. Adiar somente um intermediário positivamente vinculado àquela construção ainda em curso. Um final identificado acorda, inclusive com outro agente da mesma obra vivo. Sem prova do vínculo, o RC conserva a elegibilidade atual. O aval reserva `colhido` à leitura individual e o RELATO registra a reversão da marca em lote. `app/docs/BACKLOG.md:2264`, `app/docs/RELATO.md:15396`, `bin/rc_sem_colher.py:76–107`, `bin/disjuntor.py:2016–2022`, `bin/codex_pergunta.sh:577–584`

O mesmo intermediário pode ficar no conjunto de candidatos dentro da espera, sem renovar o prazo nem produzir `3` a cada passagem. Reavaliar nas publicações novas e na borda do teto existente cobre a baixa observável sem novo `.rc`. Para o alerta de ociosidade, quando houver início comprovado, limitar a espera ao menor prazo entre o teto atual e a borda dos 600 s. Isso evita cruzar os dez minutos e ainda esperar outro ciclo inteiro. A fonte que sustenta essa garantia ponta a ponta permanece [nao verificado]. `bin/disjuntor.py:2015–2022,2225–2242`, `app/docs/BACKLOG.md:2264`

## 4. REDs descritos

| Caso | Resultado esperado e mutante que deve falhar |
|---|---|
| **Obra ociosa** | Com obra elegível, próxima etapa conhecida e ausência de job e `PAREI` comprovada desde `t0`, ainda espera em 9:59 e aponta obra e passo na borda de 10:00, se confirmado `>= 600 s`. Começar a espera em 9:59 pega o mutante que espera os 300 s inteiros. `app/docs/BACKLOG.md:2264`, `bin/disjuntor.py:2015–2022` |
| **Impedimentos e ordem** | Job exato vivo ou `PAREI` válido da obra impede o alerta. Job ou `PAREI` de outra obra não impede. Obra fora da ordem não dispara e marcador apontando para ordem superada deve ser detectado pelo selo. Mordem os mutantes “qualquer job”, “qualquer `PAREI`” e “primeira linha física”. `bin/hook_stop_fila1.py:246–309,1600–1616`, `app/docs/PROMPTS.md:3233`, `app/docs/RELATO.md:15270` |
| **Intermediário com opus vivo** | Testar o mesmo RC já presente na entrada e publicado por rename durante a espera. Nenhum dos dois acorda. O RELATO continua sem marca e o inventário mantém o candidato quando ele satisfaz seus critérios. Mordem o filtro só na entrada e a marca automática. `bin/disjuntor.py:2016–2022`, `bin/tests/test_rc_sem_colher.sh:86–93`, `app/docs/RELATO.md:15396` |
| **Morte ou entrega** | Com término observável e o mesmo RC ainda elegível, a decisão volta a expô-lo sem depender de publicar outro `.rc`. Testar também entrega gravada mais de 5 s depois e final mais novo. Hoje ambos podem retirar o intermediário da lista mesmo sem marca. Esse caso distingue retirar o filtro de ampliar o inventário. `bin/disjuntor.py:474–490`, `bin/rc_sem_colher.py:61–83`, `app/docs/RELATO.md:15390–15396` |
| **Final na mesma obra** | Um final publicado enquanto há agente ou lease viva na mesma obra acorda na primeira observação. O intermediário ao lado segue sem provocar despertar. Morde o mutante que filtra toda a obra pela vaga ou pelo agente. `bin/codex_pergunta.sh:570–584`, `bin/codex_admissao.py:23–30`, `bin/disjuntor.py:2020–2022` |

## 5. Arquivos, sobreposição e perguntas de lei

- **Núcleo do desenho:** `bin/rc_sem_colher.py`, `bin/disjuntor.py` e o selo existente `bin/tests/test_rc_sem_colher.sh`. Se mudar a forma do comando, a validação, a transcrição em `CLAUDE.md` e seu selo precisam acompanhar. Para o marcador, estender `test_hook_nao_cobra_congelado.sh`, que hoje prova existência e estado do alvo. `bin/disjuntor.py:2020–2035,2225–2242`, `bin/tests/test_rc_sem_colher.sh:75–103`, `CLAUDE.md:337–351`, `bin/tests/test_agente_no_modelo.sh:285–297,365–370`, `bin/tests/test_hook_nao_cobra_congelado.sh:105–151`
- **Colisões documentadas:** CUSTO F1 v8 tem pouso registrado em `824500282`. `RC-SEM-PROXIMA` disputa o mesmo leitor com uma regra distinta de RC **já colhido** e mais de 15 min. Sua c5 foi registrada como parada. `V6-FURO-OUTPUT` declara `disjuntor.py`, e o censo de `PISTA3-NA-ESTEIRA` também aponta a guarda da espera. Diffs em voo e árvore viva atual ficaram [nao verificado]. O aval inicial da fatia cita teto 1 e a cópia registra o corte posterior de dois opus, um por raia sem arquivo comum. A espera pode consumir os fatos de trabalho sem decidir admissão. `app/docs/RELATO.md:15399,14550,14635,14885`, `app/docs/BACKLOG.md:1930,2202,2258,2260,2264`, `CLAUDE.md:401–404`

O `0` do censo anterior está registrado às 12:24. Para fechar o desenho com o item 3 das 15:35, ficam estas perguntas de corte. `app/docs/RELATO.md:15270,15396`

1. **Ordem e passo.** Qual registro existente autoriza obter mecanicamente o ID exato e a próxima etapa executável por raia quando o marcador aponta para um plano que herda outro em prosa? `app/docs/BACKLOG.md:8,1932–1933`, `bin/hook_stop_fila1.py:265–284` [nao verificado]
2. **Relógio e trava.** Qual evento existente inicia ou reinicia os 600 s por obra ao entrar na ordem, terminar o job ou retirar um `PAREI`? O disparo é em `>= 600 s`, conciliando “mais de 10 min” com o RED “em até 10 min”? E a trava considerada é o `PAREI` vigente daquela obra? `app/docs/BACKLOG.md:2264`, `bin/hook_stop_fila1.py:1533–1547,1600–1616` [nao verificado]
3. **Identidade e entrega opus.** Qual evidência existente liga o RC à tarefa construtora exata e cobre sua morte ou entrega? Se a entrega produzir um arquivo posterior ou um RC final, o intermediário deve continuar listado mesmo depois de perder a elegibilidade pela regra atual? `bin/disjuntor.py:474–490,1959–1972`, `bin/rc_sem_colher.py:61–83`, `app/docs/RELATO.md:15390–15396` [nao verificado]

## 28/11 15:51 · sessao 5968d1f54f90 · - · - · origem=Ronald (tmux)

- O marcador `ORDEM-VIVA-TOPO` aponta `ORDEM-VIVA-2811-1450`, uma linha de plano com raias que herda a ordem de instrumentos da 1405. O juiz do hook devolve esse ID e a descrição truncada. O handoff imprime o mesmo ID como próximo passo. A lista de itens andáveis segue a ordem física da tabela. A fonte estruturada de obra executável e próximo passo por raia permanece [nao verificado]. `app/docs/BACKLOG.md:8,1932-1933` `bin/hook_stop_fila1.py:246-309,332-343,368-378` `bin/handoff_sessao.sh:324-339`
- O marcador tem edição manual. O gerador preserva o bloco, `backlog_estado.py` escreve a célula de estado, e Stop, handoff, interrupções e selo perguntam ao mesmo juiz. O selo atual aceita um marcador que aponte à antiga 1405, já substituída. Confirmei esse `ok` com a troca em memória e a função real. `bin/gerar_backlog.py:113-121` `bin/backlog_estado.py:56-67` `bin/hook_stop_fila1.py:1710-1722` `bin/handoff_sessao.sh:60-80` `bin/interrompidas_estado.py:93` `bin/tests/test_hook_nao_cobra_congelado.sh:105-151` `app/docs/BACKLOG.md:1932`
- `PAREI` do item é julgado na primeira linha útil do painel, após a faixa de censo, com prefixo `PAREI:`, `!`, identificação inteira e exclusões para CLEAR, COTA e SONDA. `PAREI` da fila inteira exige que os restantes estejam parados. `PAREI: censo-…` é outro caso e vale com wrapper de leitura/censo comprovado. O disjuntor escreve pela porta `no_topo` e relê com o Stop. O alarme global procura a palavra nas primeiras 40 linhas e mede idade do transcript. Esse alarme tem outra semântica. `bin/hook_stop_fila1.py:462-470,516-538,779-835,1533-1616` `bin/disjuntor.py:311-338` `app/core/esteira_vigia.py:865-892` `bin/alarme_sessao_ociosa.py:119-155`
- Para Opus, o harness já escreve lançamento e notificação de estado no transcript principal. `agentes_sem_entrega` lê os IDs sem fim, e Stop usa o leitor em CLEAR e B1. Existe vínculo de ID a obra para verificador final, mas vínculo canônico de cada `.rc` ao agente construtor, com término confiável mesmo sem notificação, permanece [nao verificado]. `raia_agente.sh` escreve declaração com obra, quem, desde e o que falta para pousar. Seu medidor chama de vivo o agente declarado e ainda não baixado. `bin/disjuntor.py:443-490,612-637,654-676,844-858` `bin/hook_stop_fila1.py:1138-1148,1515-1519` `bin/raia_agente.sh:41-78` `bin/raias_estado.py:13-21,51-70,125-150`
- O relato da cópia registra pouso da CUSTO v8 `824500282` às 15:47 e OBRA-PRONTA-ACORDA em seguida. O teto documentado na cópia é dois Opus construtores, um por raia sem arquivo comum. A fatia deve preservar a admissão. `RC-SEM-PROXIMA` disputa `bin/rc_sem_colher.py`, com outro universo e prazo, e aparece pausada no relato. A matriz real de worktrees em voo permanece [nao verificado]. `app/docs/RELATO.md:15399,14228,14550,14635` `app/docs/BACKLOG.md:1930,1932-1933,2260,2264` `CLAUDE.md:323-325,357-360,403-404`
- O RED de recuperação precisa cobrir o intermediário cujo agente morre e o intermediário seguido de `entrega.md` ou `.rc` final mais novo. O leitor atual aplica “só o mais novo” e a folga de 5 segundos para arquivos posteriores. Uma filtragem apenas na saída pode preservar a ausência de marca e ainda deixar o intermediário fora do contador. `bin/rc_sem_colher.py:61-82` `app/docs/RELATO.md:15390-15393`
- O registro anterior de `lei 0` precede o aval de 15:35. A consulta atual pede duas decisões numeradas se o código e o aval não as resolverem. **1.** Qual campo existente fixa obra, próximo passo e início da ociosidade por raia, e qual borda concilia `>10 min` com `até 10 min`? **2.** Qual fato existente vincula o intermediário ao agente e permite recuperá-lo após morte ou entrega quando outro arquivo já o tornou invisível ao leitor atual? `app/docs/RELATO.md:15270,15396` `app/docs/BACKLOG.md:2264` `bin/rc_sem_colher.py:61-82` `CLAUDE.md:282-287`

## 28/11 15:51 · sessao 3249df36c0a2 · - · - · origem=Ronald (tmux)

- **Codex construtor.** O kernel decide a ocupação pelo `flock` da vaga e do diretório da worktree. `codex_admissao.py` segura os descritores, passa ao executor e mantém a lease até o wrapper encerrar. `codex_pergunta.sh` escreve a projeção com obra, vaga, worktree e ID de chamada. A projeção pode sobreviver à morte, ou pertencer ao dono anterior durante a admissão seguinte. A trilha `abriu/fechou`, lida pelo Estado, mede histórico. O `.rc` produzido pelo wrapper tem `chamada=` e é explicitamente final, publicado antes de soltar a lease. Uma regra que suprimisse todo `.rc` com vaga presa ocultaria esse final. `bin/codex_admissao.py:2-7,188-218,221-271,296-308`; `bin/codex_pergunta.sh:295-314,488-510,552-559,570-587`; `bin/codex_vagas_estado.py:4-12`; `bin/gerar_estado.py:279-285`.
- **Opus.** O harness escreve os eventos de lançamento e de fim do Agent no transcript principal. `agentes_sem_entrega()` identifica agentes de fundo sem notificação de fim, para o `/clear` e o B1. O censo já identifica obra pelo prompt ou worktree, e o B3 mapeia ID para obra via primeiro prompt e handback do verificador final. O leitor de `.rc` não recebe `transcript_path`. Vínculo universal entre RC intermediário e Agent, inclusive após morte sem notificação, [nao verificado]. `bin/disjuntor.py:443-447,474-490,521-535,612-637,654-676,844-857,1959-1972`; `bin/hook_stop_fila1.py:1138-1148,1515-1519,1645-1648`; `bin/rc_sem_colher.py:86-104`.
- **`ESPERANDO`.** A sessão declara o produtor no topo do RELATO. O wrapper construtor oferece a frase no stderr. O hook aceita uma palavra de produtor, exige todas as provas declaradas e verifica identidade/executável e PID vivo, ou avanço de mtime desde o Stop anterior. A prova por arquivo grava memória. A frase `ESPERANDO: agente <id>` sugerida pelo próprio hook tem duas palavras e falha nesse parser. A prova específica de `PAREI: censo-...` usa UID, wrapper da raiz e obra exata, e cobre leitura/censo. `CLAUDE.md:275-279`; `bin/codex_pergunta.sh:741-746`; `bin/hook_stop_fila1.py:631-680,775-887,893-902,1372-1373`.
- **VM.** `trava_teste.sh` é a porta canônica do `flock` global `/tmp/juliani_db_test.lock` e escreve `.quem` com chamador/PID na tomada e `livre` em `EXIT`. `suite.sh`, régua e pre-push usam a trava. `--quem` lê o texto e o handoff sonda o lock. O recurso ocupado não identifica a obra, e uma suite aguardando a vez ainda não segura o recurso. `bin/trava_teste.sh:21-42`; `bin/suite.sh:560-570`; `bin/regua.sh:165`; `bin/pre-push.sh:142`; `bin/handoff_sessao.sh:261-279`; `CLAUDE.md:135-137`.
- **Pistas.** O lançador remoto produz estado e recibo. Em `suite.sh`, `ssh pista estado` distingue `rodando`, `na fila` e `fim`; o aceite publica job/sha, e o `espera` traz o recibo final validado. A pista3 recebe `pede` direto no despacho atual registrado. O leitor `pista_certifica.sh` recebe arquivo de recibo e log e pode servir aos dois hosts. Ausência de recibo não prova job vivo e índice canônico obra↔host/job [nao verificado]. `bin/suite.sh:379-391,441-479`; `bin/pista_certifica.sh:9-17,22-30`; `app/docs/BACKLOG.md:1931-1933,2202`; `app/docs/RELATO.md:14875-14881,15358`.
- **Relógio e retorno à colheita.** A idade do alarme é do transcript da sessão, e a pasta recente do `rc_sem_colher` é janela de varredura. O relógio por obra permanece [nao verificado]. A regra atual também só mostra o `.rc` mais novo e o descarta se outro arquivo da obra for mais de 5 s posterior. Entrega `.md` posterior pode, portanto, tirar um intermediário sem marca da saída. Um RED para morte sem outro arquivo é diferente do RED de entrega. `bin/alarme_sessao_ociosa.py:48-63,148-150`; `bin/rc_sem_colher.py:50-83`; `app/docs/RELATO.md:15387-15398`.
- **Convivência.** O RELATO registra pouso `824500282`, fechamento da CUSTO F1 e OBRA-PRONTA seguinte na raia de instrumento. `RC-SEM-PROXIMA` toca o mesmo juiz e está registrada como pausada com cópia preservada. A triagem dessa obra registrou que `~/vercodex.py` contava todo stdout como SEM COLHER, e o patch proposto não foi instalado. A opção de espera deve preservar a interface default de colheita. `app/docs/RELATO.md:14467,14471,14493,14547-14550,14635,15399`; `app/docs/BACKLOG.md:2260`.

## 28/11 15:50 · sessao 3bc4953f162f · - · - · origem=Ronald (tmux)

- `RC SEM COLHER` lista o `.rc` mais recente de cada pasta de obra elegível, com atividade em até 24 h, idade estritamente maior que `--min-idade`, nenhum outro arquivo mais de 5 s posterior e nenhuma marca `colhido|superado` que case por obra, caminho relativo e hora local do mtime. O leitor retorna 3 para lista, 0 para vazio e falha com status diferente de zero se a leitura levanta erro. `bin/rc_sem_colher.py:21-36,39-83,86-111`
- A espera cria o marco antes da consulta. `rc 3` pula o laço e o comando termina com sucesso. `rc 0` inicia o `find -cnewer` a cada 5 s, limitado a 300 s. A lista final também vem do `find` cru. Filtrar só a consulta inicial deixaria o intermediário novo acordar a espera pelo laço. `bin/disjuntor.py:2015-2022`
- O `vercodex` usa `--min-idade 900` segundo o contrato. O binário está fora da cópia [nao verificado]. O `logs/espera_unica.sh` é citado no RELATO e sua forma ativa é [nao verificado]. O Stop registrado decide por PAREI, ESPERANDO e aviso de fila, sem chamar o leitor atual. `bin/rc_sem_colher.py:2-4,76`, `bin/tests/test_rc_sem_colher.sh:65`, `app/docs/RELATO.md:13886,13927-13931`, `.claude/settings.json:3-13`, `bin/hook_stop_fila1.py:1693-1748`
- Desenho proposto mantém pendências no modo atual e usa a mesma decisão de acionabilidade no começo, dentro do laço e na saída. Só adia o `.rc` comprovadamente intermediário de um construtor ainda em curso. A condição deve cair no fim/morte do agente mesmo que o `.rc` seja anterior ao marco. A publicação final precisa prevalecer mesmo com outra atividade da mesma obra, ou com lease ainda presa depois de publicar o `.rc` final. `bin/disjuntor.py:2015-2022,474-490`, `bin/codex_pergunta.sh:566-584`, `bin/codex_admissao.py:23-30`
- O harness escreve lançamento e término, e `_obra_de` e `_obra_do_caminho` já resolvem a identidade do pedido. O `.rc` genérico tem leitura de status e sua atribuição ao agente ou natureza intermediária não está provada. O painel de raias lê declaração manual ativa, cujo significado é “declarado e não baixado”. `bin/disjuntor.py:474-490,612-637,1835-1858,2322-2344`, `bin/rc_sem_colher.py:39-43`, `bin/raia_agente.sh:40-60,70-81`, `bin/raias_estado.py:13-21`
- O marcador atual aponta o adendo `ORDEM-VIVA-2811-1450`, que herda a ordem anterior. O hook devolve seu ID e até 110 caracteres da descrição da linha, enquanto `_itens_andaveis` enumera em ordem física. Os dados não provam uma fonte estruturada de obra e próximo passo de cada raia nem um instante canônico de início do intervalo contínuo sem job e sem PAREI [nao verificado]. `app/docs/BACKLOG.md:8,1932-1933`, `bin/hook_stop_fila1.py:241-309,1550-1616`
- REDs descritos: obra elegível ociosa nos limites 9/10/11 min; PAREI do próprio item e job correlacionado impedem só o alarme de ociosidade; fora da ordem fica fora; marcador antigo ainda aberto deve reprovar o novo selo; intermediário já presente e publicado durante a espera ficam sem marca e sem despertar; morte/entrega torna o mesmo arquivo acionável se os outros critérios permanecem; `.rc` final por rename desperta com intermediário da mesma obra no mesmo lote. A regra existente só mostra o `.rc` mais novo e deixa de listar quando existe um arquivo posterior mais de 5 s, portanto preservar intermediários mesmo diante desses fatos seria mudança adicional [nao verificado]. `app/docs/BACKLOG.md:2264`, `app/docs/RELATO.md:15270,15396`, `bin/rc_sem_colher.py:61-83`, `bin/tests/test_rc_sem_colher.sh:75-103`, `bin/tests/test_hook_nao_cobra_congelado.sh:105-151`
- Mudança de forma cruza a constante, o validador, a cópia em `CLAUDE.md` e seus selos. O RELATO registra o pouso próprio `824500282` às 15:47 e dá OBRA-PRONTA-ACORDA como próxima da raia instrumento. O teto não precisa ser alterado pela obra. O anexo fala em 1 e o texto da cópia traz corte posterior para 2 por raia sem arquivo em comum. `bin/disjuntor.py:2015-2035,2225-2242`, `CLAUDE.md:337-351,403-404`, `bin/tests/test_agente_no_modelo.sh:291-297,370`, `app/docs/RELATO.md:15399`, `app/docs/BACKLOG.md:2264`

## 28/11 15:33 · sessao 6319acbc4d36 · FERIADO-SEM-LIMITE-DO-SUPRIMIDO · censo · origem=Code

**1. Dinheiro — (c)**. O pacote de impacto da cura c5+c6 estava fora do escopo da pergunta c6. `verificador.md:3`, `c6_qr.txt:1,22–24`.  
**2. Quem mais lê — (a)**. A pergunta pediu o censo de leitores e seus significados. A resposta deixou de fora o leitor em `regras.py:443`. `c6_qr.txt:22–24,32–38`, `app/ponto/calculador/regras.py:433–448`.  
**3. Motor fora do escopo — (e)**. Faltou pedir a comparação com `S=0` e `N>T`. `c6_qr.txt:5–8,15–24`, `verificador.md:5`.  
**4. Silêncio sem fila — (c)**. O tratamento dos `expectedFailure` e sua entrada na fila ficaram fora da entrega c6. `c6_qr.txt:1,17–18,24,28`, `verificador.md:6`.  
**Alcance**. As classes valem para a pergunta e a resposta c6 fornecidas. A atribuição nas demais voltas e eventuais descartes na triagem desta reprovação ficam [nao verificado]. `c6_qr.txt:1–45`.

As referências aos anexos contam a partir da primeira linha de cada trecho colado.

1. **Dinheiro.** A pergunta abre com «so codigo» e pede «diff aplicado na wt». A resposta relata «`git diff --check` limpo». Esse é o diff de código. O parecer cobra o DIFF de frota da comp 11 por rubrica na árvore da cura, reversão nova e hash da exportada, com linha própria para 24x48. O RELATO registra um despacho c4c sobre «dinheiro», mas o resultado dessa revisão e sua triagem ficam [nao verificado]. Na comp 10, a cópia confirma a ausência de linha na `GUARDA`. A condição de exportada vem do parecer, e a situação em produção fica [nao verificado]. `c6_qr.txt:1,22–27`, `verificador.md:3`, `app/docs/PROMPTS.md:3248`, `app/docs/RELATO.md:345`, `app/ponto/services/fechamento.py:37–51,604–629`, `app/docs/LEIS.md:155–156`.

2. **Quem mais lê.** A pergunta pede «cada leitor do campo» e «se todos supoem o mesmo significado». A resposta diz que «a porta devolve minutos de dobra e do subconjunto noturno» e cita `regras.do_dia`. Ficou de fora `particionar_ft`, que lê `minutos_extra_100_feriado` e prepara `FolgaEmFeriado.minutos_noturnos`. Na cópia, quando `T>0` e a HE100 de feriado difere de `T`, o `elif` recalcula o split pelas pontas efetivas, se o ramo anterior não tiver sido tomado. Assim, baixar a HE100 abaixo de `T` pode mudar a origem do split entregue à projeção e ao mensal. A escolha entre pontas efetivas e cruas já constava como questão aberta. `c6_qr.txt:22–24,32–38`, `app/ponto/calculador/regras.py:380–383,433–448`, `app/ponto/calculador/alimentacao.py:365–377`, `app/ponto/services/fechamento.py:995–1012`, `app/docs/RELATO.md:5386`.

3. **Motor fora do escopo.** A pergunta manda «leia a docstring» e proíbe tocar o motor. A resposta afirma «Único arquivo alterado» e descreve plantões que passam de 26 h para 25 h. A comparação da noturna quando `S=0` ficou sem pedido explícito. Pelo clamp citado no parecer, `N>T` devolve `T` mesmo com `S=0`. A cópia confirma o clamp em `regras.py:448`. Nas linhas citadas para o motor, a cópia contém a validação de `regua_excedente`. O clamp do motor na árvore julgada fica [nao verificado] diretamente na cópia. `c6_qr.txt:5–8,15–24,27,43–45`, `verificador.md:5`, `app/ponto/calculador/regras.py:448`, `app/ponto/motor_calculo_v2.py:765–770`.

4. **Silêncio sem fila.** A pergunta diz «teste e do opus» e pede «qual teste de hoje, alem dos 3, pode mudar de valor». A resposta aponta `test_RED_par_26h_split_nao_supera_he100` e remete os RED ao Opus. O parecer registra FN2/FD6 como `expectedFailure` sem item de fila nem prazo. Os dois casos já aparecem como falhas na c2. A cópia local termina na linha 6051, então as marcações nas linhas 6207 e 6236 da árvore julgada ficam [nao verificado] diretamente na cópia. `c6_qr.txt:1,17–18,24,28,39`, `verificador.md:6`, `app/docs/RELATO.md:408`, `app/ponto/tests/test_o280_bateria_da_lei.py:6051`.

**Pergunta que faltou no (e)** — “Com `S=0` e `N>T` num par de mais de 17 h, qual era a HE100 noturna antes da cura e qual é depois, no motor e na promoção?” `verificador.md:5`, `c6_qr.txt:5–8,15–24`.

## 28/11 15:32 · sessao 9a466d69eea1 · C7-TELA-ADMIN-ENCERRA · verificador · origem=Code

**Bloqueia.** O encerramento usa a mesma porta e preserva as irmãs vivas fora da seleção, mas a tela pode anunciar que encerrou todas as perguntas marcadas. `app/chamados/models.py:2186`, `app/chamados/models.py:2250`, `app/chamados/views_disputa_admin.py:27`  
**Bloqueia.** O reenvio dito idêntico ignora os IDs escolhidos, que passaram a fazer parte da entrada. `app/chamados/models.py:2127`, `app/chamados/models.py:2141`  
**Bloqueia.** O novo julgamento C7 após os carimbos continua sem receber o instante da borda. `app/chamados/models.py:2273`, `app/docs/PLANO-PADROES.md:97`  
**Prova parcial.** A cópia tem 19 testes na classe nova. Os selos AST conferidos passaram, e o Q4b falhou ao retirar o `pop` em memória. Suíte Django e demais mutantes [nao verificado]. `app/chamados/tests/test_c7_tela_admin_encerra.py:131`, `app/chamados/tests/test_c7_tela_admin_encerra.py:452`, `app/chamados/tests/test_f5_v4_guarda_sob_trava.py:417`  
**Folha 11.** O parcial pode mudar a ação exibida na folha e adiar a retomada de regularizações confirmadas. Efeito em rubricas, valores ou TXT da comp. 11 [nao verificado]. `app/chamados/services/tarefa_do_dia.py:182`, `app/folha/fora_da_folha.py:225`, `app/chamados/signals.py:122`

1. **Bloqueia — sucesso parcial enganoso.** O formulário permite marcar qualquer pergunta listada. Com uma viva e uma já não viva, desde que a segunda esteja livre da guarda de resposta sem veredito, a porta carimba só a viva. Se a disputa continua aberta, a tela afirma que *as perguntas escolhidas* foram encerradas. Se a disputa fecha, a tela comunica o fechamento, mas também omite o resultado da segunda seleção. A devolutiva precisa dizer quais IDs foram carimbados e quais ficaram sem ação, a partir do resultado da porta. O caminho distinto, de uma escolhida com resposta sem veredito, recusa o pedido inteiro antes dos carimbos. `app/templates/chamados/disputa_admin.html:19`, `app/chamados/models.py:2173`, `app/chamados/models.py:2246`, `app/chamados/models.py:2264`, `app/chamados/views_disputa_admin.py:23`, `app/chamados/views_disputa_admin.py:76`

2. **Bloqueia — reenvio com alvo diferente.** `_ja_fechada` compara autor, resolução e porta. Com a disputa fechada, as seleções `[11]`, `[12]` e `[11, 12]`, todas da disputa e com o mesmo autor e motivo, devolveram `False/reenvio_identico` na sonda em memória. Exemplo do erro: o admin encerra A com motivo X, depois B com motivo Y, fechando a disputa. Um POST de A com Y recebe a mensagem de reenvio idêntico. O teste atual repete o mesmo alvo. `app/chamados/models.py:2127`, `app/chamados/models.py:2141`, `app/chamados/views_disputa_admin.py:16`, `app/chamados/tests/test_c7_tela_admin_encerra.py:338`

3. **Bloqueia — relógio do novo julgamento.** A seleção consulta `_pv(q)` sem `agora`. A nova releitura de C7 usa `fio_vivo`, que também chama C6 sem instante, e a janela pode consultar `timezone.localdate()` em cada passagem. O carimbo de cada pergunta lê `now()` na porta e o fechamento da disputa tem outro `now()`. `SILENCIO` recebe o próprio instante do carimbo, corretamente. A lacuna é o horizonte do julgamento, que pode mudar entre a seleção e o C7 final na virada do dia. A conversão de exibição para São Paulo é independente e está presente. `app/docs/PLANO-PADROES.md:97`, `app/chamados/models.py:2186`, `app/chamados/models.py:2255`, `app/chamados/models.py:2273`, `app/chamados/models.py:3466`, `app/chamados/juizes.py:633`, `app/chamados/juizes.py:1609`, `app/config/settings/base.py:94`

4. **Fila — escritor, juiz e trava.** O diff realiza a opção do aval na escrita direta. O POST passa os IDs a `admin_encerra`, que delega a `fechar`. Os carimbos passam por `fechar_por_admin` e `_escrever_carimbo`, e o fechamento final pergunta ao `pode_fechar` registrado. A tela lê o resultado e `fechada_em`. A composição das guardas do modo inteiro já existia no trecho de base do diff. A ordem de escrita é Colaborador → Disputa → Chamado → Pergunta. Há leituras **antes** da trava, inclusive de pertencimento, do estado da instância e das guardas, com possibilidade de recusa antecipada. Para autorizar a escrita, a porta trava, relê a disputa e reavalia as perguntas. No fechamento final, o S144 existente ainda pode carimbar uma irmã sem carimbo cujo fato já esteja aprovado, pela via `juiz_aprovou`. `aval.txt:1`, `app/chamados/views_disputa_admin.py:64`, `app/chamados/models.py:2127`, `app/chamados/models.py:2201`, `app/chamados/models.py:2211`, `app/chamados/models.py:2231`, `app/chamados/models.py:2273`, `app/chamados/models.py:2287`, `app/chamados/models.py:3057`, `app/chamados/models.py:3508`, `app/core/juizes.py:299`, `app/chamados/signals.py:149`, `app/chamados/services/materializacao.py:2342`

5. **Fila — censo de leitores, escritores e chamadores.**

   - **`perguntas=` e prefetch.** O formulário produz os IDs e o POST os lê com `getlist`. `admin_encerra` encaminha e `fechar` valida e consome a seleção. `None` conserva o modo inteiro. O prefetch da tela pertence à consulta de renderização, enquanto o POST busca a disputa separadamente. O `pop` em `fechar` invalida a foto recebida de chamadores que prefetcharam perguntas, como o comando dos becos. As guardas e `pode_fechar()` sem lista passam a ler pelo gerente relacionado. `app/templates/chamados/disputa_admin.html:21`, `app/chamados/views_disputa_admin.py:38`, `app/chamados/views_disputa_admin.py:64`, `app/chamados/views_disputa_admin.py:72`, `app/chamados/models.py:2064`, `app/chamados/models.py:2124`, `app/chamados/models.py:2172`, `app/chamados/models.py:2297`, `app/chamados/management/commands/fechar_becos_disputa.py:33`, `app/chamados/services/becos.py:59`
   - **`recusa_do_fechamento` e `RecusaDaPorta`.** `fechar` zera e escreve a causa em memória. A view é a leitora de produção do atributo e traduz quatro causas. A quinta, `RECUSA_COMPETENCIA`, pertence à porta de competência e seu chamador conta pelo `bool`. `fechar` e `admin_encerra` levantam `RecusaDaPorta`, subclasse de `ValueError`. A nova view captura só esse tipo. Nos vizinhos, o comando de fatos continua capturando `ValueError`, e o reconciliador que serializa `type(exc).__name__` passa a registrar `RecusaDaPorta`. A mensagem de disputa já fechada usa `localtime` com precisão de minuto. `app/chamados/models.py:1884`, `app/chamados/models.py:1978`, `app/chamados/models.py:2120`, `app/chamados/models.py:2141`, `app/chamados/models.py:2233`, `app/chamados/models.py:2269`, `app/chamados/models.py:2294`, `app/chamados/views_disputa_admin.py:16`, `app/chamados/views_disputa_admin.py:73`, `app/chamados/services/competencia_trancada.py:232`, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:93`, `app/chamados/reconciliador.py:1316`
   - **Demais chamadas diretas fora do diff.** Nenhuma passa `perguntas=`. Os usos estão em `app/chamados/reconciliador.py:415`, `app/chamados/reconciliador.py:719`, `app/chamados/reconciliador.py:1314`, `app/chamados/services/disputa_emissao.py:1298`, `app/chamados/services/disputa_emissao.py:2224`, `app/chamados/services/materializacao.py:1159`, `app/chamados/services/materializacao.py:2160`, `app/api/foto_recebida.py:126`, `app/ponto/services/entrada_sincronizada.py:60`, `app/colaboradores/services/desligamento.py:167`, `app/chamados/models.py:2422`, além de becos, competência e comando de fatos acima. A compatibilidade da exceção vale pelo tipo-base. A correção de todos os resultados já tinha ressalvas. Há chamadores que anunciam ou contam fechamento ignorando `False`, inclusive na materialização, no S128, no upload de foto, na remoção de motivo e no comando de fatos. `app/chamados/models.py:2223`, `app/chamados/services/materializacao.py:2160`, `app/chamados/services/disputa_emissao.py:2224`, `app/api/foto_recebida.py:126`, `app/chamados/models.py:2422`, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:89`

6. **Fila — semântica, exceções e alcance dos testes.** `True` passa a significar que houve carimbo **ou** fechamento da disputa. A view distingue pelo `fechada_em`, mas esse par perde os IDs do resultado, como no achado 1. `admin_decidiu` já servia ao silêncio do fechamento inteiro e ao veredito da célula. O rótulo da via ainda descreve só o segundo, enquanto a sequência grava `SILENCIO` e o leitor de mérito também exige resposta. A captura HTTP é estreita. Os `except Exception` já existentes dos receivers e a auditoria best-effort continuam no caminho. O teste de erro interno prova a propagação na primeira tentativa de carimbo, sem exercitar falha depois de uma primeira escrita. O Q4b morde a retirada do `pop`. Os testes de irmã viva, resposta sem veredito e recusa de todos os carimbos têm asserções sobre estado e trilha, mas não cobrem a seleção mista nem o reenvio com alvo diferente. `app/chamados/models.py:2255`, `app/chamados/models.py:2273`, `app/chamados/models.py:2923`, `app/chamados/services/fio.py:1399`, `app/chamados/views_disputa_admin.py:73`, `app/chamados/signals.py:143`, `app/chamados/signals.py:166`, `app/core/observ.py:94`, `app/chamados/tests/test_c7_tela_admin_encerra.py:143`, `app/chamados/tests/test_c7_tela_admin_encerra.py:225`, `app/chamados/tests/test_c7_tela_admin_encerra.py:251`, `app/chamados/tests/test_c7_tela_admin_encerra.py:338`, `app/chamados/tests/test_c7_tela_admin_encerra.py:423`, `app/chamados/tests/test_f5_v4_guarda_sob_trava.py:417`

7. **Fila — efeito possível na folha.** O carimbo parcial altera o fio. `SILENCIO` retira a ação daquela pergunta, e a tela da folha lê a porta publicada. A retenção do TXT consulta a célula. O efeito financeiro indireto está no fechamento total: o `on_commit` retoma regularizações confirmadas, que podem criar ou alterar batidas quando já não há disputa aberta. O parcial mantém essa guarda. O aval cita 420 irmãs vivas em sete disputas e descreve o feriado da comp. 11 em outro item. Delta de pessoas, rubricas, minutos, valores ou TXT atribuível ao C7 na comp. 11 [nao verificado]. `aval.txt:1`, `app/chamados/services/tarefa_do_dia.py:182`, `app/folha/fora_da_folha.py:225`, `app/folha/export.py:999`, `app/folha/export.py:1342`, `app/chamados/signals.py:122`, `app/chamados/services/regularizacao_ext.py:57`, `app/chamados/services/regularizacao_ext.py:77`, `app/chamados/services/regularizacao_ext.py:122`

## 28/11 15:31 · sessao 2336b65785f5 · FERIADO-L097-MOTOR · leitura · origem=Code

- A c3 libera o feriado em folga com escala certa quando o dia do turno coincide com o dia julgado e o ciclo está fora de `CICLOS_FERIADO_SIMPLES`. [motor_calculo_v2.py:1579](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/motor_calculo_v2.py:1579)
- A c4 pode consultar o `nao` do mesmo dia no mapa já carregado por `DecisaoHE.objects.vigentes()`, sem consulta adicional. [motor_calculo_v2.py:1530](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/motor_calculo_v2.py:1530) [motor_calculo_v2.py:1569](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/motor_calculo_v2.py:1569)
- A Gestão de HE já mostra o motivo genérico do bloqueio e a ciência da recusa quando o dia consta no retrato lavrado. [views.py:948](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/views.py:948) [gestao_he.html:309](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/templates/ponto/gestao_he.html:309)
- Para pagar após a recusa, o admin pode usar **Autorizar** no dia. A porta troca o estado para `autorizado` e encaminha o recálculo. [gestao_he.html:259](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/templates/ponto/gestao_he.html:259) [he.py:195](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/portas/he.py:195) [he.py:229](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/portas/he.py:229)
- O aval fornecido registra **17 dias recusados, 118,85 h** — 13 na emp2 comp. 11 e 4 na emp3 — e **26 dias g1 sem recusa**. Origem das recusas, presença na tela e totais em produção [nao verificado].

## 1. Guarda da c3

`MotorBase._dia_sem_previsto_fora_da_janela` recebe o período já calculado. Para chegar à exceção da c3, o par precisa ter entrada e minutos trabalhados, ser folga ou feriado, ter janela de HE vigente e ter previsto igual a zero. Uma decisão `autorizado` já faz a função retornar antes. [motor_calculo_v2.py:1554](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/motor_calculo_v2.py:1554) [motor_calculo_v2.py:1503](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/motor_calculo_v2.py:1503)

O `return` da c3 está em `:1601`, dentro do ramo de `_por_folga`, com colaborador conhecido e `escala_certa_no_dia` verdadeira. O predicado final exige `_dia_do_turno_do_par(entrada)[0] == dia`, `eh_feriado(dia)` e ciclo fora de `CICLOS_FERIADO_SIMPLES`, cujo único membro é `12x36`. A folga com escala incerta tem um retorno anterior. O ponto da c4 é esse `return` da c3. Com `nao` vigente para `dia`, o par cai no bloqueio existente, que transfere os minutos para fora da janela, zera as rubricas do período, colapsa o par efetivo e publica a pendência. [motor_calculo_v2.py:1579](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/motor_calculo_v2.py:1579) [motor_calculo_v2.py:1599](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/motor_calculo_v2.py:1599) [motor_calculo_v2.py:3757](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/motor_calculo_v2.py:3757)

## 2. Mapa e data da decisão

A chave persistida é `(colaborador, data)`. Na instância do motor, o mapa de um colaborador é `{date: estado}`. `_carregar_decisoes_he()` faz a leitura sob demanda e a guarda do bloqueio total já o chama antes da exceção. Os estados graváveis são `autorizado` e `nao`. Ausência de chave significa ausência de decisão vigente. [models.py:1287](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/models.py:1287) [models.py:1354](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/models.py:1354) [motor_calculo_v2.py:1511](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/motor_calculo_v2.py:1511)

A guarda recebe `data_jornada`. O laço parte do dia atribuído pelo juiz, usa a data civil da entrada como fallback e pode herdar a jornada anterior por um vão de até seis horas. A c3 só libera quando esse `dia` coincide com `_dia_do_turno_do_par(entrada)[0]`. Assim, para um turno de D com segundo par civil em D+1 que o juiz atribui a D, a recusa relevante é a de D. O outro leitor direto do mapa é `_janela_do_dia`, que usa o dia do turno e entrega o booleano `autorizado` aos clipadores de entrada e saída. [motor_calculo_v2.py:963](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/motor_calculo_v2.py:963) [motor_calculo_v2.py:2530](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/motor_calculo_v2.py:2530) [motor_calculo_v2.py:2591](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/motor_calculo_v2.py:2591) [motor_calculo_v2.py:1663](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/motor_calculo_v2.py:1663) [motor_calculo_v2.py:1781](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/motor_calculo_v2.py:1781) [motor_calculo_v2.py:1825](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/motor_calculo_v2.py:1825) [motor_calculo_v2.py:1869](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/motor_calculo_v2.py:1869)

## 3. Juiz de vigência

`DecisaoHEQuerySet.vigentes()` é o juiz único e aplica `revogada_em__isnull=True`. O manager expõe o queryset. O motor usa `objects.vigentes()` diretamente e a porta `_vigente()` consulta esse mesmo queryset pelo PK da linha travada. A revogação automática exige recusa de origem `sistema` e passagem do limite. Para a recusa humana, a operação disponível é a troca de estado pela porta `decidir_he`. [models.py:1251](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/models.py:1251) [models.py:1348](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/models.py:1348) [motor_calculo_v2.py:1530](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/motor_calculo_v2.py:1530) [he.py:130](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/portas/he.py:130) [he.py:195](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/portas/he.py:195) [he.py:497](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/portas/he.py:497)

O C6 registrado em `core/juizes.py` aponta para `chamados/juizes.py::pergunta_viva`. Ali, a decisão explícita do admin sobre o mesmo dia prevalece sobre a resposta do colaborador. É o precedente citado no aval. Para HE, a autoridade executável é `DecisaoHE.objects.vigentes()`. `JUIZES.md` descreve o papel do registro e situa C6 no fluxo de perguntas. [core/juizes.py:298](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/core/juizes.py:298) [chamados/juizes.py:1516](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/chamados/juizes.py:1516) [JUIZES.md:7](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/docs/JUIZES.md:7) [JUIZES.md:97](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/docs/JUIZES.md:97)

## 4. Lista do admin e motivo

A rota **Gestão de HE** lê o retrato da competência e sobrepõe o estado vigente por colaborador e dia. Na c3, o `return` do feriado pula a publicação do bloqueio total. Na c4, ao cair nesse bloco, o motor publica `dia/minutos/hora`, o espelho converte em `sentido='dia'` e o lavrador inclui a ocorrência no retrato. Com o retrato atualizado e filtros compatíveis, a tela existente já mostra o dia como recusado. O dia fica fora da contagem **sem decisão**. [urls.py:17](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/urls.py:17) [motor_calculo_v2.py:1599](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/motor_calculo_v2.py:1599) [motor_calculo_v2.py:1615](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/motor_calculo_v2.py:1615) [espelho.py:757](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/services/espelho.py:757) [he_pendente_lavrado.py:138](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/services/he_pendente_lavrado.py:138) [gestao_he.py:302](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/services/gestao_he.py:302) [gestao_he.py:357](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/services/gestao_he.py:357)

O texto de `:1616` vai para `periodo.alertas`. Na Gestão de HE, a tabela de apoio mostra **“dia sem jornada prevista”** e **“ciência dada”**, e o calendário mostra **“com ciência: segue bloqueado”**. Isso cobre o motivo genérico. Para exibir explicitamente **“recusa vigente”** como causa, cabe ajustar o rótulo da tela. Para exibir a justificativa digitada, seria preciso trazer `DecisaoHE.motivo` ao leitor, que hoje busca apenas colaborador, data e estado. O motivo da recusa pode estar vazio. [motor_calculo_v2.py:1615](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/motor_calculo_v2.py:1615) [gestao_he.html:262](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/templates/ponto/gestao_he.html:262) [gestao_he.html:309](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/templates/ponto/gestao_he.html:309) [gestao_he.py:92](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/services/gestao_he.py:92) [he.py:124](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/portas/he.py:124)

## 5. Casos RED propostos no papel

- **Dia vizinho.** Cadastre D como feriado que vira folga certa em 6x1, com janela vigente. Batidas em D das 07h às 11h. Recusa vigente apenas no dia vizinho → D paga 4 h de HE100-feriado. Mova a recusa para D → zero pago e 240 min bloqueados em D. [test_feriado_um_juiz_porta.py:586](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/tests/test_feriado_um_juiz_porta.py:586) [motor_calculo_v2.py:1569](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/motor_calculo_v2.py:1569)
- **D+1 civil.** Cadastre D como feriado em folga certa, com turno partido 21h–07h. Bata 21h em D, 00h01, 03h01 e 07h01 em D+1. Com os dois pares atribuídos pelo juiz a D, recusa só em D+1 → 7 h de HE100-feriado e nenhuma pendência em D. Recusa em D → os dois pares ficam bloqueados em D. [test_feriado_um_juiz_porta.py:672](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/tests/test_feriado_um_juiz_porta.py:672) [motor_calculo_v2.py:1599](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/motor_calculo_v2.py:1599)
- **Recusa desfeita e “sim” vigente.** No 6x1 de D, com batidas 07h–11h, parta de `nao` vigente e 240 min bloqueados. O admin usa **Autorizar** no dia com motivo. A mesma linha passa a `autorizado` vigente e o recálculo conta 4 h. Para testar a outra transição, use uma recusa de sistema que perdeu vigência pela porta própria. No cálculo seguinte, a linha fica fora do mapa e a c3 volta a liberar D. O valor literal `sim` é inválido na porta. [he.py:75](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/portas/he.py:75) [he.py:175](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/portas/he.py:175) [he.py:229](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/portas/he.py:229) [he.py:497](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/portas/he.py:497) [motor_calculo_v2.py:1530](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/motor_calculo_v2.py:1530) [motor_calculo_v2.py:1570](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/motor_calculo_v2.py:1570)
- **Recusa com foto menor.** No mesmo cadastro, a recusa humana em D guarda 30 min quando as batidas eram 07h–07h30. Corrija a saída para 11h e mantenha a recusa vigente → zero pago, 240 min bloqueados na lista e 30 min na foto da decisão. A decisão tem escopo de dia e o bloqueio usa os minutos calculados do período. [models.py:1278](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/models.py:1278) [he.py:497](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/portas/he.py:497) [motor_calculo_v2.py:1602](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/motor_calculo_v2.py:1602)

## 6. Selos existentes e impacto

Os métodos `*l097*` estão em `test_feriado_um_juiz_porta.py`. Eles fixam 4 h de folga trabalhada ou de HE100-feriado conforme o cadastro, o mesmo total com escala certa ou incerta e os controles de folga comum, 12x36 e feriado com previsto zero em dia de trabalho. Os selos da virada fixam o dia do turno, e os de publicação conferem motor, espelho e segmento do oráculo. A c4 acrescenta a variante com `nao` vigente via queryset real, com zero pago e classificação de recusado após a lavratura do retrato. O selo de `nao` na janela com marcos continua aplicável àquele caminho. [test_feriado_um_juiz_porta.py:549](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/tests/test_feriado_um_juiz_porta.py:549) [test_feriado_um_juiz_porta.py:579](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/tests/test_feriado_um_juiz_porta.py:579) [test_feriado_um_juiz_porta.py:607](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/tests/test_feriado_um_juiz_porta.py:607) [test_feriado_um_juiz_porta.py:651](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/tests/test_feriado_um_juiz_porta.py:651) [test_feriado_um_juiz_porta.py:942](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/tests/test_feriado_um_juiz_porta.py:942) [test_janela_he_saida.py:262](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/ponto/tests/test_janela_he_saida.py:262)

A **catraca de linhas** compara os números exatos do censo AST com `LINHAS_REGISTRADAS`. A cópia bateu com o registro. Simulei em memória uma linha adicional na guarda. O censo conservou as mesmas ocorrências e deslocou em `+1` os endereços registrados depois da guarda, incluindo a herança por vão de `2538` para `2539`. Se a edição deslocar essas linhas, o registro acompanha no mesmo commit. [test_medida_turno_do_par.py:306](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/core/tests/test_medida_turno_do_par.py:306) [test_medida_turno_do_par.py:352](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/core/tests/test_medida_turno_do_par.py:352) [test_medida_turno_do_par.py:565](/home/ronald/codex_ws/fe8f70a428f22f5c56a4adff2a2cf758a74d2cd0/app/core/tests/test_medida_turno_do_par.py:565)

A leitura e a simulação foram feitas sem gravar arquivos. Validação do hash pelo Git e execução da suíte Django [nao verificado]. [nao verificado]

## 28/11 15:21 · sessao 3bd917403db0 · E4C-F5-INSTANTE · construtor · origem=Code

1. `CargaDaTarefa.tarefa::ler` publica `chamados_do_colab` consultando a escada uma vez por PK e seguindo a ordem do mapa do dia. Com a tabela vigente, isso preserva o conjunto e a ordem do filtro anterior. `app/chamados/services/tarefa_do_dia.py:697`, `app/chamados/services/tarefa_do_dia.py:759`, `app/chamados/juizes.py:1787`
2. `fora_da_folha::_destinos_da_retencao` lê a chave da carga, inclusive quando a porta é `None`. A chave integra o retorno da carga, e os contratos de chaves de `porta_da_tarefa` e `_portas_da_tarefa` seguem iguais. `app/folha/fora_da_folha.py:125`, `app/chamados/services/tarefa_do_dia.py:817`, `app/chamados/services/tarefa_do_dia.py:821`, `app/chamados/tests/test_e3_portas_tarefa.py:257`, `app/chamados/tests/test_e3_c8b_sem_banco.py:95`
3. **Censo do código de aplicação.** Só `_destinos_da_retencao` usa a chave. Os demais leitores preservam seu resultado: `_porta`, `o_que_segura` e a pauta indireta (`app/folha/fora_da_folha.py:73`, `app/folha/fora_da_folha.py:309`, `app/ponto/services/pautas_dos_discordantes.py:87`), `censo` e `censo_alcance` (`app/chamados/services/vigia_fio.py:391`, `app/chamados/services/vigia_fio.py:634`), `medir` (`app/chamados/services/furo_sem_canal.py:97`), `beco_em_lote` (`app/escala/services/cadastro_realidade.py:557`) e o backfill (`app/chamados/management/commands/backfill_fio_morto_e4b.py:90`). A fachada `tarefa_do_dia` repassa a chave, e `diff_e3_sombra` continua lendo a projeção, inclusive `verbos`. `app/chamados/services/tarefa_do_dia.py:852`, `app/chamados/management/commands/diff_e3_sombra.py:77`, `app/chamados/management/commands/diff_e3_sombra.py:145`
4. **RED para o Opus.** Na carga, com `{41: 'cobrar', 20: 'validar', 7: 'decidir', 3: None}`, remapeie `cobrar` para `admin` e `validar` e `decidir` para `colab`. Exija `(7, 20)` na ordem canônica. Na folha, injete uma tarefa sem rota com a chave `(20, 7)` e `verbos` apontando para o PK `41`. Exija `[('chamado', 20), ('chamado', 7)]`. Cubra também a chave vazia e reaplique os selos Haiku e L-129. `app/chamados/services/tarefa_do_dia.py:697`, `app/chamados/services/tarefa_do_dia.py:759`, `app/folha/fora_da_folha.py:131`, `app/folha/tests/test_haiku_fora_da_folha.py:227`, `app/folha/tests/test_l129_fora_da_folha_so_le_a_tarefa.py:79`
5. **Mutantes para o Opus.** Fixar `cobrar` na derivação, aplicar a escada ao mapa inteiro, selecionar `admin`, parar no primeiro PK, reordenar na folha ou devolver a tradução à folha. Sintaxe e `git diff --check` conferidos. Suíte, RED, mutantes executados e produção [nao verificado]. `app/chamados/services/tarefa_do_dia.py:697`, `app/folha/fora_da_folha.py:131`

## 28/11 15:18 · sessao fc4926ac4efb · E4C-F5-INSTANTE · construtor · origem=Code

- `app/folha/fora_da_folha.py::_destinos_da_retencao` abre os chamados de cobrança indevida cujo verbo tem fio `colab`. `app/folha/fora_da_folha.py:125`
- O filtro chama `fio_pelos_verbos({pk: verbo})` para cada chamado, usando a função pública do juiz. `app/folha/fora_da_folha.py:135`, `app/chamados/juizes.py:1792`
- Pela tabela vigente, só `cobrar` tem fio `colab`, então o filtro preserva os mesmos PKs e a ordem da carga. `app/chamados/juizes.py:1787`, `app/folha/fora_da_folha.py:133`
- Na busca em produção, esse foi o único acesso direto a `carga.tarefa(...)['verbos']`. O comando `diff_e3_sombra` lê o campo pela projeção `porta_da_tarefa`. `app/folha/fora_da_folha.py:133`, `app/chamados/management/commands/diff_e3_sombra.py:77`, `app/chamados/management/commands/diff_e3_sombra.py:145`
- A entrega altera só o leitor, e `git diff --check` passou. O selo L-129 ainda proíbe a chamada. A execução da suíte e do RED fica `[nao verificado]`. `app/folha/fora_da_folha.py:131`, `app/folha/tests/test_l129_fora_da_folha_so_le_a_tarefa.py:16`

**RED para o Opus**

- **Delegação ao juiz.** Com carga falsa, forneça `{41: 'cobrar', 20: 'validar', 7: 'decidir', 3: None}`. No teste, substitua temporariamente `FIO_DO_VERBO` por uma tabela em que `cobrar` tenha fio `admin` e `validar` e `decidir` tenham fio `colab`. Espere `[('chamado', 20), ('chamado', 7)]`. A base retornaria apenas o PK `41`. `5d183096b:app/folha/fora_da_folha.py:133`, `app/chamados/juizes.py:1799`
- **Selo AST.** Reaplique o selo Haiku à base para capturar o literal `cobrar`. No L-129, permita `fio_pelos_verbos` apenas no recorte de cobrança indevida com um chamado por consulta. Preserve a proibição do C17 e do acesso direto a `FIO_DO_VERBO`. `app/folha/tests/test_haiku_fora_da_folha.py:227`, `app/folha/tests/test_l129_fora_da_folha_so_le_a_tarefa.py:16`, `app/folha/tests/test_l129_fora_da_folha_so_le_a_tarefa.py:104`
- **Controle de paridade.** Com a tabela vigente e a tarefa sem rota, use `{41: 'cobrar', 20: 'validar', 7: 'cobrar', 3: None}` e espere `[('chamado', 41), ('chamado', 7)]`. Com mapa vazio, espere `[]`. `app/chamados/juizes.py:1787`, `app/folha/fora_da_folha.py:133`, `app/folha/fora_da_folha.py:247`

**Mutantes para o Opus**

- Restaurar `verbo == 'cobrar'` ou copiar a tabela para a folha. O RED de delegação e o selo Haiku devem capturar essas regressões. `app/folha/fora_da_folha.py:135`, `app/folha/tests/test_haiku_fora_da_folha.py:227`
- Consultar `fio_pelos_verbos(verbos)` com o mapa inteiro, trocar `colab` por `admin` ou parar no primeiro PK elegível. O caso misto com dois destinos deve capturar os três mutantes. `app/folha/fora_da_folha.py:134`, `app/chamados/juizes.py:1799`

## 28/11 15:05 · sessao 27b144c710dd · E4C-F5-INSTANTE · leitura · origem=Code

## RESUMO

- **3 causas distintas em 32 ocorrências.** Duas vêm do diff `b82a5ef08..5d183096b` e uma já estava na base. [vigiar_fio_morto.py:241](/home/ronald/codex_ws/5d183096b6dc873eb141669d7161f289aab6dadb/app/chamados/management/commands/vigiar_fio_morto.py:241), [fora_da_folha.py:133](/home/ronald/codex_ws/5d183096b6dc873eb141669d7161f289aab6dadb/app/folha/fora_da_folha.py:133), [RELATO.md na base:302](/home/ronald/codex_ws/b82a5ef08988438256b90c5d0b9d94f4552156e5/app/docs/RELATO.md:302)
- **Do diff, 30 erros.** O vigia passou a exigir `indeterminado`, mas os dois dublês do contador fornecem `estoque`. [vigiar_fio_morto.py:241](/home/ronald/codex_ws/5d183096b6dc873eb141669d7161f289aab6dadb/app/chamados/management/commands/vigiar_fio_morto.py:241), [test_fio_mudo_placar.py:128](/home/ronald/codex_ws/5d183096b6dc873eb141669d7161f289aab6dadb/app/core/tests/test_fio_mudo_placar.py:128), [test_e4a2_c5_memoria.py:132](/home/ronald/codex_ws/5d183096b6dc873eb141669d7161f289aab6dadb/app/chamados/tests/test_e4a2_c5_memoria.py:132)
- **Do diff, 1 falha.** O novo literal `'cobrar'` em `fora_da_folha` viola a asserção por AST. [fora_da_folha.py:133](/home/ronald/codex_ws/5d183096b6dc873eb141669d7161f289aab6dadb/app/folha/fora_da_folha.py:133), [test_haiku_fora_da_folha.py:235](/home/ronald/codex_ws/5d183096b6dc873eb141669d7161f289aab6dadb/app/folha/tests/test_haiku_fora_da_folha.py:235)
- **Preexistente, 1 falha.** A regra de vocabulário encontra `recado` nas mesmas duas linhas de `RELATO.md` em ambas as cópias. [test_contract_vocabulario.py:46](/home/ronald/codex_ws/5d183096b6dc873eb141669d7161f289aab6dadb/app/pautas/tests/test_contract_vocabulario.py:46), [RELATO.md na base:302](/home/ronald/codex_ws/b82a5ef08988438256b90c5d0b9d94f4552156e5/app/docs/RELATO.md:302), [RELATO.md na base:15041](/home/ronald/codex_ws/b82a5ef08988438256b90c5d0b9d94f4552156e5/app/docs/RELATO.md:15041)

### 1. Contrato do contador `ato_x_coluna` — 30 erros

Todos os 30 tracebacks terminam em **`KeyError: 'indeterminado'`** em `Command.handle`, ao montar a linha `VFM|ATO_X_COLUNA`. O hunk `@@ -231,8 +235,11 @@` de `vigiar_fio_morto.py` acrescenta o acesso a `ato_x_coluna['indeterminado']`. O contador real devolve essa chave no hunk `@@ -6,20 +10,27 @@` de `vigia_ato_x_coluna.py`. Os hunks dos dois testes acrescentam `estoque` e `marco` aos dublês, deixando `indeterminado` ausente. [vigiar_fio_morto.py:241](/home/ronald/codex_ws/5d183096b6dc873eb141669d7161f289aab6dadb/app/chamados/management/commands/vigiar_fio_morto.py:241), [vigia_ato_x_coluna.py:33](/home/ronald/codex_ws/5d183096b6dc873eb141669d7161f289aab6dadb/app/chamados/services/vigia_ato_x_coluna.py:33), [test_fio_mudo_placar.py:128](/home/ronald/codex_ws/5d183096b6dc873eb141669d7161f289aab6dadb/app/core/tests/test_fio_mudo_placar.py:128), [test_e4a2_c5_memoria.py:132](/home/ronald/codex_ws/5d183096b6dc873eb141669d7161f289aab6dadb/app/chamados/tests/test_e4a2_c5_memoria.py:132)

**`core.tests.test_fio_mudo_placar` — 20 erros do diff.** O hunk `@@ -125,7 +125,7 @@` altera o dublê que `_vigiar` instala antes de chamar o comando. [test_fio_mudo_placar.py:128](/home/ronald/codex_ws/5d183096b6dc873eb141669d7161f289aab6dadb/app/core/tests/test_fio_mudo_placar.py:128), [test_fio_mudo_placar.py:144](/home/ronald/codex_ws/5d183096b6dc873eb141669d7161f289aab6dadb/app/core/tests/test_fio_mudo_placar.py:144)

| Ocorrências | Casos que chegam ao mesmo `KeyError` |
|---:|---|
| 5 | `test_RED_alvo_um_alarma_e_sai_dois_com_chave`, `test_zero_nao_alarma`, `test_RED_totais_c5_informativos_na_linha_e_no_carimbo_sem_alarme`, `test_hoje_do_carimbo_e_dia_civil_local_nao_data_utc_nem_competencia` e `test_mesma_funcao_alimenta_a_linha_e_o_carimbo`. [test_fio_mudo_placar.py:152](/home/ronald/codex_ws/5d183096b6dc873eb141669d7161f289aab6dadb/app/core/tests/test_fio_mudo_placar.py:152), [test_fio_mudo_placar.py:156](/home/ronald/codex_ws/5d183096b6dc873eb141669d7161f289aab6dadb/app/core/tests/test_fio_mudo_placar.py:156), [test_fio_mudo_placar.py:174](/home/ronald/codex_ws/5d183096b6dc873eb141669d7161f289aab6dadb/app/core/tests/test_fio_mudo_placar.py:174), [test_fio_mudo_placar.py:255](/home/ronald/codex_ws/5d183096b6dc873eb141669d7161f289aab6dadb/app/core/tests/test_fio_mudo_placar.py:255), [test_fio_mudo_placar.py:274](/home/ronald/codex_ws/5d183096b6dc873eb141669d7161f289aab6dadb/app/core/tests/test_fio_mudo_placar.py:274) |
| 3 | `test_RED_c5_publica_esperado_real_e_placar_alarma_divergencia_com_dono`, com `(numero, esperado)` igual a `(1,1)`, `(1,0)` e `(0,1)`. [test_fio_mudo_placar.py:160](/home/ronald/codex_ws/5d183096b6dc873eb141669d7161f289aab6dadb/app/core/tests/test_fio_mudo_placar.py:160) |
| 3 | `test_RED_carimbo_publica_esperado_do_contador_da_rodada`, com `(7,7)`, `(0,7)` e `(1,0)`. [test_fio_mudo_placar.py:241](/home/ronald/codex_ws/5d183096b6dc873eb141669d7161f289aab6dadb/app/core/tests/test_fio_mudo_placar.py:241) |
| 9 | `test_mutantes_em_memoria_sao_rejeitados_pela_assercao_pretendida` falha na verificação anterior à aplicação de cada mutante listado no anexo. `leitor_ignora_divergencia`, `carimbo_esperado_literal`, `placar_rc_zero`, `carimbo_numero_literal`, `placar_sem_leitor`, `vigia_sem_contador_c5`, `vigia_sem_esperado_c5`, `placar_sem_comparar_c5` e `carimbo_sem_informativos_c5`. [test_fio_mudo_placar.py:316](/home/ronald/codex_ws/5d183096b6dc873eb141669d7161f289aab6dadb/app/core/tests/test_fio_mudo_placar.py:316), [test_fio_mudo_placar.py:333](/home/ronald/codex_ws/5d183096b6dc873eb141669d7161f289aab6dadb/app/core/tests/test_fio_mudo_placar.py:333) |

**`chamados.tests.test_e4a2_c5_memoria` — 8 erros do diff.** O hunk `@@ -129,7 +129,7 @@` altera o dublê injetado em `_vigiar`, que executa o `handle` com esse retorno. [test_e4a2_c5_memoria.py:132](/home/ronald/codex_ws/5d183096b6dc873eb141669d7161f289aab6dadb/app/chamados/tests/test_e4a2_c5_memoria.py:132), [test_e4a2_c5_memoria.py:143](/home/ronald/codex_ws/5d183096b6dc873eb141669d7161f289aab6dadb/app/chamados/tests/test_e4a2_c5_memoria.py:143)

- **2 diretos.** `test_RED_publicacao_e_comparacao_c5` e `test_RED_totais_c5_publicados_sem_virar_esperado`. [test_e4a2_c5_memoria.py:158](/home/ronald/codex_ws/5d183096b6dc873eb141669d7161f289aab6dadb/app/chamados/tests/test_e4a2_c5_memoria.py:158), [test_e4a2_c5_memoria.py:180](/home/ronald/codex_ws/5d183096b6dc873eb141669d7161f289aab6dadb/app/chamados/tests/test_e4a2_c5_memoria.py:180)
- **6 subtestes.** `test_mutantes_em_memoria_mordem_na_assercao_pretendida` falha em `verificar()` antes de aplicar `vigia_sem_contador_c5`, `vigia_sem_esperado_c5`, `placar_sem_comparar_c5`, `vigia_sem_informativos_c5`, `disparar_sem_informativos` ou `vigia_sem_rotulo_informativo`. [test_e4a2_c5_memoria.py:41](/home/ronald/codex_ws/5d183096b6dc873eb141669d7161f289aab6dadb/app/chamados/tests/test_e4a2_c5_memoria.py:41), [test_e4a2_c5_memoria.py:223](/home/ronald/codex_ws/5d183096b6dc873eb141669d7161f289aab6dadb/app/chamados/tests/test_e4a2_c5_memoria.py:223)

**`chamados.tests.test_e4a2_matriz_memoria` — 2 erros do diff por dependência.** O arquivo da matriz é idêntico nas duas cópias. `test_RED_alvo_um_no_estado_deixa_placar_vermelho` chama `C5MemoriaTest._vigiar(0, 0)` e herda o dublê alterado pelo hunk de C5. O subteste `placar_ignora_fio_mudo` de `test_mutantes_mordem_na_assercao_pretendida` chama o mesmo teste em `verificar()`, antes de aplicar o mutante. [test_e4a2_matriz_memoria.py:38](/home/ronald/codex_ws/5d183096b6dc873eb141669d7161f289aab6dadb/app/chamados/tests/test_e4a2_matriz_memoria.py:38), [test_e4a2_matriz_memoria.py:287](/home/ronald/codex_ws/5d183096b6dc873eb141669d7161f289aab6dadb/app/chamados/tests/test_e4a2_matriz_memoria.py:287), [test_e4a2_matriz_memoria.py:377](/home/ronald/codex_ws/5d183096b6dc873eb141669d7161f289aab6dadb/app/chamados/tests/test_e4a2_matriz_memoria.py:377), [test_e4a2_c5_memoria.py:132](/home/ronald/codex_ws/5d183096b6dc873eb141669d7161f289aab6dadb/app/chamados/tests/test_e4a2_c5_memoria.py:132)

### 2. Literal de verbo no leitor — 1 falha do diff

**`folha.tests.test_haiku_fora_da_folha`.** `test_MORDE_a_folha_nao_tem_tabela_de_verbo` levanta `AssertionError` na linha 235. A asserção espera uma interseção vazia entre os literais de string da AST e `{'cobrar', 'validar', 'decidir'}`. O hunk `@@ -122,6 +122,24 @@` de `fora_da_folha.py` introduz `_destinos_da_retencao`, cujo filtro `if verbo == 'cobrar'` acrescenta o único literal encontrado na interseção. A mesma extração na cópia da base devolve o conjunto vazio. [test_haiku_fora_da_folha.py:233](/home/ronald/codex_ws/5d183096b6dc873eb141669d7161f289aab6dadb/app/folha/tests/test_haiku_fora_da_folha.py:233), [test_haiku_fora_da_folha.py:235](/home/ronald/codex_ws/5d183096b6dc873eb141669d7161f289aab6dadb/app/folha/tests/test_haiku_fora_da_folha.py:235), [fora_da_folha.py:133](/home/ronald/codex_ws/5d183096b6dc873eb141669d7161f289aab6dadb/app/folha/fora_da_folha.py:133)

### 3. Vocabulário proibido em `RELATO.md` — 1 falha preexistente

**`pautas.tests.test_contract_vocabulario`.** `test_MORDE_o_produto_tem_UM_nome` levanta `AssertionError` na linha 46, porque `ruins` tem duas entradas e a asserção espera `[]`. A regra encontra a palavra `recado` em `RELATO.md:302` e `RELATO.md:15041`. O teste e o documento são idênticos entre as duas cópias, e a aplicação estática da regra encontra as mesmas duas ocorrências na base. [test_contract_vocabulario.py:19](/home/ronald/codex_ws/5d183096b6dc873eb141669d7161f289aab6dadb/app/pautas/tests/test_contract_vocabulario.py:19), [test_contract_vocabulario.py:46](/home/ronald/codex_ws/5d183096b6dc873eb141669d7161f289aab6dadb/app/pautas/tests/test_contract_vocabulario.py:46), [RELATO.md:302](/home/ronald/codex_ws/5d183096b6dc873eb141669d7161f289aab6dadb/app/docs/RELATO.md:302), [RELATO.md:15041](/home/ronald/codex_ws/5d183096b6dc873eb141669d7161f289aab6dadb/app/docs/RELATO.md:15041)

A atribuição usa a comparação das duas cópias locais, a leitura dos tracebacks e as verificações estáticas. Uma nova execução da suíte na base ou no canário fica **[nao verificado]**. [nao verificado]

## 28/11 14:56 · sessao a93a8f4909b4 · FERIADO-L097-MOTOR · leitura · origem=Code

**RESUMO**  
**Feito.** Conferi o corpo inteiro do motor na cópia e o hunk c3. A sonda está abaixo. `app/ponto/motor_calculo_v2.py:1536–1602`  
**Veredito.** Totais A/B, universos, erros e `g4=0` na sombra [nao verificado].  
**Próximo.** Rodar `bin/sonda_leitura.sh --sombra <arquivo.py>` e conferir a última linha `IMPACTO|rc=...`. `bin/sonda_leitura.sh:54–59,235–251`

O corpo de `motor_c3` vem das linhas **1536–1602** da cópia indicada como HEAD, com as 17 linhas do anexo inseridas após **1584**. O blob local começa com `891199180` e a aplicação em memória reproduziu o prefixo `65f7046b2` do anexo. Hash do commit pela metadata Git [nao verificado]. `app/ponto/motor_calculo_v2.py:1536–1602`

A sonda usa o universo real e faz duas passadas na mesma transação de leitura. As 24 somas mensais usam os campos numéricos que o escritor persiste. As somas incluem os valores conhecidos e `mudou_nulo` mostra transições de disponibilidade. Para o detalhe diário, a sonda usa a partição efetiva e intercepta em memória as duas operações finais de `DiaPago`. `g3` pode acompanhar outro grupo no mesmo dia e informa se a recusa registrada está vigente. `g4` inclui o resíduo diário e diferenças mensais sem delta diário comparável. `app/ponto/services/fechamento.py:619–622,995–1021,1124–1136,1152–1173,1199–1222,1237–1267` · `app/ponto/services/dia_pago.py:344–373` · `app/ponto/models.py:1265–1288,1335–1339`

O Python opera em leitura. O launcher seleciona a sombra e o schema `juliani` e tem operações próprias de credencial, temporário e trilha no host. Execução pelo launcher nesta sessão [nao verificado]. `bin/sonda_leitura.sh:93–96,126–143,216–218,235–250`

### Quem regrava a competência 10 por evento

- **Batida.** `registrar_batida` chama `_fechamento_por_evento` após criar a batida. A retratação usa a mesma porta, e o ajuste retroativo que materializa uma saída entra por `registrar_batida`. A aprovação de justificativa por si só chama `invalidar_fechamento`. `app/ponto/registro_batida.py:170–198,287–302` · `app/ponto/services/justificativa.py:187–216,478–488`
- **Ajuste de célula ou validação.** A edição e a regeneração da célula passam por `_o_chao_do_dia_mudou`, que aciona o recálculo por competência atingida. A validação de pergunta chama a porta com `data_evento`, e o aceite de folga usa o dia da folga. `app/ponto/portas/celula.py:392,490–503,682` · `app/chamados/services/validacao.py:627–628,787–799`
- **Decisão de HE.** `decidir_he` grava a decisão e chama `recalcular_por_evento` pela data decidida. O caminho idempotente retorna antes dessa chamada. `app/ponto/portas/he.py:198–208,225–231`
- **Escrita mensal.** `recalcular_por_evento` agenda em `on_commit`, resolve a competência pela janela da empresa que contém a data do fato e chama `recalcular_fechamento_mes` para um PK em modo de escrita. Se o fato cair na competência 10, o escritor usa `save()` ou `update()` conforme o status. A guarda da cópia lista apenas 09/2026. Quais eventos ocorreram e quais valores já foram regravados em produção [nao verificado]. `app/ponto/services/fechamento.py:37–51,510–513,623–630,1174–1227,1634–1644,1838–1864` · `app/ponto/services/dia_pago.py:714–731`

```python
if getattr(__import__("django.conf", fromlist=["settings"]).settings, "HASNER_SOMBRA", False) is not True:
    print("IMPACTO|rc=1|erros=1|escopos=0|motivo=settings_sombra", flush=True)
    raise SystemExit(1)

import inspect
import logging
from collections import Counter
from contextlib import redirect_stderr, redirect_stdout
from decimal import Decimal
from hashlib import sha256
from io import StringIO
from sys import _getframe
from unittest.mock import patch

from ponto.motor_calculo_v2 import CICLOS_FERIADO_SIMPLES, MotorBase, localizar


def motor_c3(self, dia, entrada, periodo):
    """L-097 BLOQUEIO TOTAL NO DIA SEM PREVISTO (aval `!` Ronald 26/10 22:1x -- zona inviolavel, UMA mudanca). -> None

    A REGRA, nas palavras dele: *"dia sem previsto inteiro = ponta fora da janela"*. No dia cujo previsto e 0
    PELO JUIZ (`_previsto_do_dia`: folga, feriado, sem escala vigente), com a empresa na janela, o par inteiro
    e minuto fora do marco -- nao ha marco nenhum dentro do qual contar --, e pela L-097 *"nenhuma HE nasce
    sozinha"*: conta ZERO e vira pendencia ate o admin Autorizar. Caso nominal: col841 17/10, folga, 12h02
    pagas como HE sem decisao nenhuma (cartao 12h02 x Gestao de HE 0,00 h).

    INVERTE O O263-FT (aval 13/10 19:0x: "na folga trabalhada nao ha marco, entao a janela nao recorta"):
    la a ausencia de marco SOLTAVA o par; aqui ela o PRENDE inteiro. O selo de la troca a assercao.

    O PAR SE CALCULA INTEIRO ANTES (duracao, intervalo, noturno), e o que ele PAGARIA muda de lugar: e esse o
    numero que vira pendencia, o mesmo que Autorizar paga -- nunca o vao bruto com o intervalo dentro.
    Autorizado (B2), previsto que nao e 0, `None` (sem juiz) ou empresa fora da janela: nada muda.
    ALCANCE (aval Ronald 27/10 01:3x HE100-ALCANCE-A-PREMISSA (a); L-007, `ponto/calculador/regras.py`): folga SEM
    escala certa (`escala_certa_no_dia` False) fica fora -- a hora segue em trabalhadas, sem o 100%.
    """
    if entrada is None or not float(periodo.minutos_trabalhados or 0):
        return
    # O DIA SEM PREVISTO DO AVAL E A FOLGA E O FERIADO -- os dois casos nominais (col841 17/10 folga, col43 12/10
    # feriado) e o universo do laudo (35 dias, 34 em feriado). `previsto 0` SOZINHO e mais largo e foi MEDIDO
    # (DIFF da 10 na sombra, 26/10 22:5x): pegava o INTERMITENTE (previsto DERIVA do realizado -- excecao declarada)
    # e o dia de trabalho que o cadastro nao descreve, -1.693,83 h de trabalhadas em 32 colabs. A pergunta "e folga?"
    # e o MESMO juizo que classifica a FT neste laco (`_folga_do_par`); "e feriado?" e `eh_feriado`, o juiz de
    # feriado deste motor. Nenhum predicado novo: so os dois que ja decidem o dia.
    _por_folga = bool(getattr(self, '_folga_do_par', {}).get(localizar(entrada)))
    if not (_por_folga or self.eh_feriado(dia)):
        return
    if self._empresa_da_janela_do_dia(dia) is None:
        return
    if self._previsto_do_dia(dia) != 0:
        return
    self._carregar_decisoes_he()
    if (getattr(self, '_he_decisoes', None) or {}).get(dia) == 'autorizado':
        return
    # ALCANCE (aval Ronald 27/10 01:3x HE100-ALCANCE-A-PREMISSA (a)): a folga SEM escala certa fica FORA do
    # HE100 (nao zera, nao vira pendencia) -- o MESMO juiz `escala_certa_no_dia` que a L-007 (`ponto/calculador/regras.py`) ja usa para essa
    # hora: entra em trabalhadas sem o 100%. Vale quando o par e folga (mesmo se tambem for feriado: ele esta em
    # `periodos_ft` e `fechamento.py` aplica a `_certa` a todo `periodos_ft` antes de separar o feriado). Feriado
    # SO por `eh_feriado` (nao-folga) segue bloqueado como antes. O juiz e o caro, por isso vai por ultimo; o
    # cache e por calculo (nasce onde `_sem_previsto_fora` nasce). Sem colaborador conhecido (None) a regra se
    # aplica como antes. `assinaturas_do_colab(so_escala=True)` volta antes de chamar o motor: sem recursao.
    if _por_folga:
        _cid = getattr(self, '_colab_do_calculo', None) or getattr(getattr(self, '_ec', None), 'colaborador_id', None)
        if _cid is not None:
            from escala.services.escala_certa import escala_certa_no_dia
            if not escala_certa_no_dia(_cid, dia, getattr(self, '_escala_certa_cache', None)):
                return
            # FERIADO-L097-MOTOR (aval `!` Ronald 27/11 21:06, corte: *"feriado trabalhado e lei, paga direto pelo
            # ciclo, nao entra no filtro de autorizar HE"*): com escala CERTA, a folga que e FERIADO sai daqui com
            # o par intacto -- a rubrica e da particao de FT (`calculador/regras.py::particionar_ft`), que ja le a
            # certeza e a dobra do ciclo; o mesmo total da incerta, que retorna logo acima. Folga comum segue
            # pendente. O 12x36 segue pelos cortes vigentes (bloqueado): a pergunta e o CICLO, a mesma constante da
            # fabrica (`CICLOS_FERIADO_SIMPLES`), nunca `feriado_em_dobra`, que a CCT pode ligar no 12x36. Feriado
            # que nao e folga (so `eh_feriado`) e o colab desconhecido seguem bloqueados: leitura mais restritiva,
            # perguntas (0) e (1) no topo do RELATO.
            # c2 (reprovacao ver1 #1): o dia e o da PARTICAO que paga o par. `particionar_ft` pergunta
            # `_dia_do_turno_do_par` e, quando ele difere do dia HERDADO que chega aqui (vao de 6 h do laco),
            # leva o par ao dia do turno e ao vinculo DELE (`fechamento.py::contexto_por_dia`, de outra fatia),
            # que esta guarda nao ve. So se libera o par cujo dia do turno E este dia: ai a particao nao remapeia
            # e paga aqui, com o vinculo desta fatia. O par herdado segue bloqueado (`!` ao Ronald, sitio: a
            # heranca do dia no laco do MotorBase).
            if (self._dia_do_turno_do_par(entrada)[0] == dia and self.eh_feriado(dia)
                    and getattr(self.escala, 'tipo_ciclo', None) not in CICLOS_FERIADO_SIMPLES):
                return
    _m = float(periodo.minutos_trabalhados)
    periodo.minutos_fora_janela = float(periodo.minutos_fora_janela or 0) + _m
    for _f in ('minutos_trabalhados', 'minutos_noturnos_relogio', 'minutos_noturnos_legais', 'minutos_extra',
               'minutos_extra_50', 'minutos_extra_100', 'minutos_extra_100_feriado', 'minutos_extra_50_noturna',
               'minutos_extra_100_noturna', 'minutos_intrajornada_indenizada'):
        setattr(periodo, _f, 0.0)
    # A TESTEMUNHA LE O MESMO PAR (L-099, LEI-AKITA 2): o oraculo (`alimentacao.py::segmentos_do_motor`) monta os
    # segmentos de `efetivo_entrada/efetivo_saida`, nao de `minutos_*` -- MEDIDO na sombra: ele pagava o dia que
    # o motor bloqueou (col300 22/09 folga 12,0 h; col365 12/10 feriado 12,0 h). O par bloqueado sai COLAPSADO
    # (`efetivo_saida = efetivo_entrada`): a mesma familia do par invertido pela janela, que o leitor ja conta em
    # `par_invertido_pela_janela` e nao paga. A decisao segue AQUI; o calculador nao ganha regra.
    if periodo.efetivo_entrada is not None:
        periodo.efetivo_saida = periodo.efetivo_entrada
    periodo.alertas.append(
        'DIA SEM JORNADA PREVISTA -- %d min fora da janela de HE (L-097, bloqueio total): pendencia na Gestao '
        'de HE ate Autorizar.' % round(_m))
    self._sem_previsto_fora.append({'dia': dia, 'minutos': int(round(_m)),
                                    'hora': localizar(entrada).strftime('%H:%M')})


ZERO = Decimal(0)
EPS = Decimal("0.000000001")
FERIADO = "horas_extras_100_feriado"
FOLGA = "horas_folga_trabalhada"
METODO = "_dia_sem_previsto_fora_da_janela"
BASE = "a95c3432bc77f1094433e6aac3947460cb85b6d08419a15319a863c06821ebab"


def decimal(valor):
    return None if valor is None else Decimal(str(valor))


def fmt(valor):
    return "NA" if valor is None else format(valor, "f")


def detalhe(valores):
    return ",".join(f"{campo}:{valor:.9f}" for campo, valor in sorted(valores.items()))


def delta_dia(a, b):
    deltas = {campo: b.get(campo, ZERO) - a.get(campo, ZERO) for campo in a.keys() | b.keys()}
    return {campo: delta for campo, delta in deltas.items() if abs(delta) > EPS}


def parar_em_log_de_erro(_logger, registro):
    if registro.levelno >= logging.ERROR:
        raise SystemExit(1)


def medir(estado):
    from django.db import connection, transaction
    from django.utils import timezone

    from colaboradores.models import Empresa
    from escala.servico_jornada import escala_vigente
    from folha.export import _FERIADO_CLASSIFICADO_NO_MENSAL
    from ponto.janelas import janela_fechamento
    from ponto.management.commands.aplicar_09_corte_b import CAMPOS
    from ponto.models import DecisaoHE, DiaPago
    from ponto.services import dia_pago, fechamento

    campos = tuple(c for c in CAMPOS if c not in ("mes", "ano"))
    recalcular = fechamento.recalcular_fechamento_mes
    original = getattr(MotorBase, METODO)

    def simular_lavra(colab, mes, ano, **dados):
        # app/ponto/services/dia_pago.py:370-373
        class Memoria:
            def filter(self, **filtro):
                return self

            def delete(self):
                return None

            def bulk_create(self, novas):
                linhas.extend(novas)

        linhas = []
        with patch.object(DiaPago, "objects", Memoria()):
            quantidade = dia_pago.lavrar(colab, mes, ano, **dados)
        if quantidade != len(linhas) or not linhas or len({linha.data for linha in linhas}) != quantidade:
            raise RuntimeError("captura_dia_pago")
        por_dia = {}
        for linha in linhas:
            valores = {c: decimal(getattr(linha, c)) for c in dia_pago.CAMPOS}
            valores["horas_extras"] = valores["horas_extras_50"] + valores["horas_extras_100"]
            por_dia[linha.data] = valores
        return por_dia

    def passada(empresa_id, mes, ano, janela, ids, metodo, anterior=None):
        marcas = {}

        def observar(motor, dia, entrada, periodo):
            antes = len(motor._sem_previsto_fora)
            metodo(motor, dia, entrada, periodo)
            marcas[id(periodo)] = (periodo, dia, len(motor._sem_previsto_fora) > antes)

        class Coleta(dict):
            def __setitem__(self, pk, furos):
                # app/ponto/services/fechamento.py:1152-1173,1237-1267
                chamador = _getframe(1)
                if chamador.f_code is not recalcular.__code__ or pk in self:
                    raise RuntimeError("origem_da_coleta")
                v = chamador.f_locals
                del chamador
                colab = v["colab"]
                if (colab.pk != pk or colab.empresa_id != empresa_id
                        or (v["_data_ini"], v["_data_fim"]) != janela):
                    raise RuntimeError("escopo_da_coleta")
                r = v["resultado"]
                certo, feriado, comum = v["_particao_ft"]
                dias = simular_lavra(
                    colab, mes, ano, periodos=r.periodos,
                    ft_certa=certo, ft_feriado=feriado, ft_sem_escala=comum,
                    orfaos_por_dia=v["_orfaos_por_dia"], faltas_por_dia=v["_faltas_por_dia"],
                    atraso_por_dia=v["_atrasos_dia"], credito_por_dia=v["_creditos_dia"],
                    reflexo_dsr=v["fechamento"].horas_reflexo_dsr,
                    banco=v["fechamento"].saldo_banco_horas, abono_por_dia=v["_abono_dia"],
                    previsto_por_dia=v["_prev_dia_m"], realizado_por_dia=v["_real_dia_m"],
                    dia_da_jornada=r.dia_da_jornada, furos_do_motor=furos,
                    versao_motor=v["_carimbo_regua"], regua_por_dia=v["_regua_por_dia"],
                    evento_origem="recalcular_fechamento_mes",
                )
                contexto = v["_ap"].contexto_por_periodo_ft
                bloqueios, pares, ocorrencias = set(), {}, Counter()
                for p in r.periodos_ft:
                    _, dia, bloqueado = marcas[id(p)]
                    _, vinculo = contexto[id(p)]
                    chave = (vinculo.pk, dia, p.entrada, p.saida)
                    ocorrencias[chave] += 1
                    chave += (ocorrencias[chave],)
                    pares[id(p)] = (chave, dia, bloqueado)
                    if bloqueado:
                        bloqueios.add(chave)
                marcas.clear()

                esperado = {}
                if anterior is not None:
                    bloqueios_a = anterior[pk][1]

                    def liberado(p):
                        chave, dia, bloqueado = pares[id(p)]
                        if (bloqueado or chave not in bloqueios_a or p.minutos_trabalhados <= 0
                                or dia_pago._dia_de(p, r.dia_da_jornada) != dia):
                            return False
                        motor, _ = contexto[id(p)]
                        return motor.eh_feriado(dia)

                    def folga_comum(p):
                        if not liberado(p):
                            return False
                        _, dia, _ = pares[id(p)]
                        _, vinculo = contexto[id(p)]
                        if (ano, mes) < _FERIADO_CLASSIFICADO_NO_MENSAL:
                            vinculo = escala_vigente(colab, dia)
                        return vinculo is not None and vinculo.feriado_vira_folga(dia) is False

                    liberados_feriado = [f for f in feriado if liberado(f.periodo)]
                    liberados_comum = [p for p in certo if folga_comum(p)]
                    if liberados_feriado or liberados_comum:
                        esperado = simular_lavra(
                            colab, mes, ano, ft_certa=liberados_comum,
                            ft_feriado=liberados_feriado, dia_da_jornada=r.dia_da_jornada,
                        )
                super().__setitem__(pk, (dias, bloqueios, esperado))

        coleta = Coleta()
        with patch.object(MotorBase, METODO, observar):
            lidos = recalcular(
                mes, ano, empresa_id=empresa_id, somente_leitura=True,
                lavrar_dias_pagos=False, coletar_furos=coleta,
            )
        if lidos.keys() != ids or coleta.keys() != ids:
            raise RuntimeError("universo_incompleto")
        mensal = {pk: {c: decimal(valores[c]) for c in campos} for pk, valores in lidos.items()}
        return mensal, coleta

    def comparar(mensal_a, mensal_b, coleta_a, coleta_b, escopo):
        linhas, grupos = [], Counter()
        recusa_candidatos = {}
        for pk in sorted(mensal_a):
            dias_a = coleta_a[pk][0]
            dias_b, _, esperados = coleta_b[pk]
            com_delta = set()
            for dia in sorted(dias_a.keys() | dias_b.keys() | esperados.keys(), key=lambda d: (d is None, d)):
                a, b = dias_a.get(dia, {}), dias_b.get(dia, {})
                mudou = delta_dia(a, b)
                esperado = esperados.get(dia, {})
                imprevisto = delta_dia(esperado, mudou)
                if not mudou and not imprevisto:
                    continue
                com_delta.update(mudou)
                onde = f"{escopo}|colab={pk}|dia={dia.isoformat() if dia is not None else 'NA'}"
                for grupo, rubrica in (("g1", FERIADO), ("g2", FOLGA)):
                    horas = esperado.get(rubrica, ZERO)
                    if horas > EPS:
                        grupos[grupo] += 1
                        linhas.append(
                            f"IMPACTO|GRUPO|{onde}|grupo={grupo}|rubrica={rubrica}"
                            f"|A={a.get(rubrica, ZERO):.9f}|B={b.get(rubrica, ZERO):.9f}"
                            f"|delta={mudou.get(rubrica, ZERO):.9f}|horas_liberadas={horas:.9f}"
                        )
                if imprevisto:
                    grupos["g4"] += 1
                    linhas.append(f"IMPACTO|GRUPO|{onde}|grupo=g4|delta={detalhe(mudou)}"
                                  f"|delta_sem_explicacao={detalhe(imprevisto)}")
                pagas = {c: mudou[c] for c in (FOLGA, "horas_extras_50", "horas_extras_100",
                                               "horas_trabalhadas") if mudou.get(c, ZERO) > EPS}
                if dia is not None and pagas:
                    recusa_candidatos[pk, dia] = detalhe(pagas)

            for campo in campos:
                a, b = mensal_a[pk][campo], mensal_b[pk][campo]
                if a != b and (a is None or b is None or campo not in com_delta):
                    delta = None if a is None or b is None else b - a
                    grupos["g4"] += 1
                    linhas.append(f"IMPACTO|GRUPO|{escopo}|colab={pk}|dia=NA|grupo=g4"
                                  f"|rubrica={campo}|A={fmt(a)}|B={fmt(b)}|delta={fmt(delta)}"
                                  "|motivo=mensal_sem_delta_diario_comparavel")

        if recusa_candidatos:
         

[truncado em 20000 de 24892 caracteres]

## 28/11 14:52 · sessao 2f786fab1487 · FERIADO-BOTAO-FAIXA · construtor · origem=Code

**Resumo**  
Apliquei o diff em um único template. No layout `mes`, a faixa fica acima da rolagem, com ou sem botão. [_calendario_grade.html:99](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/templates/colaboradores/partials/_calendario_grade.html:99)   [nao verificado]
`git diff --check` passou. Geometria no navegador, suíte e RED [nao verificado].   [nao verificado]
Próximo passo da obra é o Opus escrever o RED de geometria e o Code rodar a suíte. [Smoke existente:588](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:588) [nao verificado]

### Alcance nas duas cascas

- `calendario_partial.html` inclui a grade com `layout='mes'`. A ficha inteira estende `base.html` e carrega esse calendário por HTMX. A aba **Espelho** do drawer carrega o mesmo calendário em `base.html` e `base_app.html`. [calendario_partial.html:67](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/templates/colaboradores/calendario_partial.html:67) · [detalhe.html:1](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/templates/colaboradores/detalhe.html:1) · [detalhe.html:215](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/templates/colaboradores/detalhe.html:215) · [base.html:399](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/templates/base.html:399) · [base_app.html:154](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/templates/base_app.html:154) · [_drawer_generico.html:134](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/templates/core/_drawer_generico.html:134)
- Os outros includes diretos usam `layout='fita'` no diagnóstico e o layout padrão na auditoria. Ambos renderizam a lista completa de badges no lugar original. [_drawer_diagnostico.html:35](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/templates/colaboradores/partials/_drawer_diagnostico.html:35) · [furos.html:42](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/templates/relatorios/furos.html:42) · [_calendario_grade.html:148](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/templates/colaboradores/partials/_calendario_grade.html:148)

Nas duas cascas, a faixa fica logo abaixo do botão quando ele aparece e logo abaixo do cabeçalho do dia nos demais feriados. Batidas, detalhes e outros badges ficam acessíveis pela rolagem da célula. Meses de cinco e seis semanas mantêm sete colunas e células de 132 px em todas as semanas. A seleção usa `dia.feriado` e a posição do primeiro badge, conforme a ordem do serviço. O texto e a cor vêm desse badge. [_calendario_grade.html:65](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/templates/colaboradores/partials/_calendario_grade.html:65) · [_calendario_grade.html:96](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/templates/colaboradores/partials/_calendario_grade.html:96) · [_calendario_grade.html:136](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/templates/colaboradores/partials/_calendario_grade.html:136) · [_calendario_grade.html:148](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/templates/colaboradores/partials/_calendario_grade.html:148) · [calendario.py:711](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/colaboradores/services/calendario.py:711)

### RED para o Opus

O smoke `test_clica_no_feriado_na_pagina_e_no_drawer_das_duas_cascas` é o teste de tela afetado. A faixa passa a ocupar espaço acima da região cujo tamanho o smoke exercita ao clicar no botão, abrir o popover e rolar até a última batida. [Smoke:588](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:588) · [Rolagem:469](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:469) [nao verificado]

Reusar o feriado de 20/11/2026 com quatro batidas e acrescentar um segundo badge. Rodar na ficha real e no Espelho das duas cascas, com e sem botão, em meses de cinco e seis semanas. Forçar o transbordamento com conteúdo antes da posição antiga da faixa, por exemplo aumentando o bloco de ícone e detalhes. Localizar a faixa pelo texto na célula e exigir exatamente uma ocorrência, com os quatro lados visíveis em `scrollTop=0` e após rolar até a última batida. Conferir também que o outro badge é alcançável, que o botão abre o popover quando oferecido e que as células continuam com 132 px. [Fixture:26](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:26) · [Sonda de geometria:329](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:329) · [_calendario_grade.html:68](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/templates/colaboradores/partials/_calendario_grade.html:68)

**Mutantes propostos**

- Devolver a faixa à posição antiga dentro do conteúdo rolável, mantendo texto e cor. A checagem dos quatro lados deve acusar o recorte. [_calendario_grade.html:105](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/templates/colaboradores/partials/_calendario_grade.html:105) · [_calendario_grade.html:148](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/templates/colaboradores/partials/_calendario_grade.html:148)
- Condicionar a rolagem à presença do botão. O feriado sem ação e com conteúdo extra deve perder acesso à última batida ou ao outro badge. [_calendario_grade.html:96](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/templates/colaboradores/partials/_calendario_grade.html:96) · [_calendario_grade.html:105](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/templates/colaboradores/partials/_calendario_grade.html:105)
- Manter a faixa acima da rolagem e também renderizar o primeiro badge no laço. A checagem de ocorrência única deve falhar. [_calendario_grade.html:100](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/templates/colaboradores/partials/_calendario_grade.html:100) · [_calendario_grade.html:149](/home/ronald/wt-codex-feriado-botao-faixa-144822/app/templates/colaboradores/partials/_calendario_grade.html:149)

## 28/11 14:49 · sessao a40e931c5c8a · FERIADO-SEM-LIMITE-DO-SUPRIMIDO · construtor · origem=Code

**Resumo**  
Cura aplicada em [alimentacao.py](/home/ronald/wt-codex-feriado-sem-limite-do-suprimido-144715/app/ponto/calculador/alimentacao.py:365). A projeção do `oraculo` usa a mesma porta da promoção.   [nao verificado]
Veredito estático: sintaxe válida, `git diff --check` limpo e 0 ocorrências de `except` no diff. Único arquivo alterado: `ponto/calculador/alimentacao.py:353`.  
Próximo: Opus relê a expectativa adicional de 26 h e escreve os RED e mutantes. Certificação pelo Code e pelo Opus [não verificado].

### Leitores e unidades

- A projeção usa `resultado.dia_da_jornada` com `_dia_de`, como o mensal e a lavra `motor`. A porta devolve minutos de dobra e do subconjunto noturno. Os três sítios convertem para horas em `horas_extras_100`, `horas_extras_100_feriado` e `horas_extras_100_noturna`. `horas_noturnas` continua vindo dos minutos noturnos legais. `ponto/calculador/alimentacao.py:360–384`, `ponto/services/dia_pago.py:139–156,261–269`, `ponto/services/fechamento.py:1003–1015`.
- `rubricas_por_dia` passa as rubricas em horas, pela chave do dia da jornada, a `regras.do_dia`. A regra incorpora os valores e a lavra `oraculo` os grava na linha desse dia. `soma_da_competencia` soma os mesmos campos em horas para a versão solicitada. `ponto/calculador/alimentacao.py:525,578–596`, `ponto/calculador/regras.py:788,818,958`, `ponto/services/fechamento.py:1295–1320`, `ponto/services/dia_pago.py:354–379,585–598`.
- O vigia usa a mesma conta para o teto em **minutos** e agrupa pela data de `entrada`. A diferença de chave pode aparecer na virada da meia-noite. `folha/porta_export.py:542–576`, `ponto/services/dia_pago.py:126–132,149–155`.

### Outros usos encontrados pelo grep

- `diff_calculador` aplica a projeção diretamente no lado `MOTOR`. O lado calculador também a recebe via `rubricas_por_dia` quando usa os pares da autoridade e a partição classificada. A comparação usa horas por dia. No lado `MOTOR`, a chamada direta também alcança a partição histórica se ela contiver promoção. `ponto/management/commands/diff_calculador.py:70,255–257,406–415,518–546`, `ponto/calculador/regras.py:393–404`.
- Os usos diretos nos testes estão em `ponto/tests/test_feriado_um_juiz.py:836,854,875,967,1074,1097`. Além dos três casos de porta, `test_RED_par_26h_split_nao_supera_he100` ainda exige **26 h** em `horas_extras_100`. Pela porta comum, a expectativa passa a **25 h**. A asserção de **1.500 min** no período do motor já está escrita. `ponto/tests/test_feriado_um_juiz.py:826–842`, `ponto/services/dia_pago.py:149–156`. Os alvos textuais do catálogo de mutantes continuam presentes. `ponto/tests/feriado_um_juiz_mutantes.py:294,404,408,484`, `ponto/calculador/alimentacao.py:360,378,386`, `ponto/management/commands/diff_calculador.py:255`.

### RED e mutantes para o Opus

- **Três RED existentes.** No 6x1 de 21h a 7h, a lavra deve dar **9 h** de HE100 feriado, **9 h** de HE100 noturna e **10,29 h** de noturno legal. No plantão de 26 h iniciado às 22h, as três rubricas de HE100 devem dar **25 h**. No plantão de 21h a 23h do dia seguinte, as três devem dar **25 h**, com **28,57 h** de noturno legal. `ponto/tests/test_feriado_um_juiz_porta.py:138–148,350–357,611–632`.
- **RED de agrupamento.** Dois pares promovidos com entradas em datas civis distintas e o mesmo dia no mapa. O primeiro tem 120 min todos noturnos e 60 min suprimidos. O segundo tem 120 min todos diurnos e zero suprimido. Cobrar uma linha no dia do juiz com **3 h** de HE100 total e feriado, das quais **2 h** noturnas. `ponto/services/dia_pago.py:126–132,149–156`, `ponto/motor_calculo_v2.py:753–785`.
- **Mutantes propostos.** Restaurar o trabalhado bruto nas duas HE100 totais deve cair nos três RED existentes. Restaurar só o split cheio deve cair no plantão iniciado às 22h, com **26 h** de HE100 noturna em vez de **25 h**. Chamar a porta par a par deve cair no RED de agrupamento, com **1 h** noturna em vez de **2 h**. Agrupar por data civil deve cair na chave da linha desse mesmo RED. Execução dos mutantes [não verificado]. `ponto/calculador/alimentacao.py:360–384`, `ponto/services/dia_pago.py:149–156`, `ponto/tests/test_feriado_um_juiz_porta.py:350–357,611–632`.

## 28/11 14:49 · sessao 1b673b01adf2 · - · - · origem=Ronald (tmux)

A partição efetivamente consumida pelo mensal está em `app/ponto/services/fechamento.py:995–1021`. Outubro escolhe o vínculo via `escala_vigente` e novembro usa o contexto do período ou do turno. Para g2, observe a rubrica comum retornada, `eh_feriado=True` e `feriado_vira_folga=False` no vínculo efetivo. A recusa vigente é um marcador adicional para g3, inclusive quando o dia já pertence a g1 ou g2. `app/ponto/calculador/regras.py:371–453`, `app/ponto/models.py:1265–1288`, `app/ponto/motor_calculo_v2.py:1511–1532`.

O gancho `coletar_furos` recebe a apuração já derivada no caminho de leitura. O cálculo diário canônico pode ser reaproveitado em memória interceptando as duas operações finais de persistência de `DiaPago`. Comparar os deltas diários completos permite apontar movimentos adicionais dentro de um dia que também recebeu g1 ou g2. `app/ponto/services/fechamento.py:1152–1173`, `app/ponto/services/fechamento.py:1237–1273`, `app/ponto/services/dia_pago.py:344–373`.

O método-base está em `app/ponto/motor_calculo_v2.py:1536–1602` e o acréscimo vai após a linha 1584. O prefixo do blob local é `891199180`, correspondente ao anexo. O launcher seleciona `juliani` e realiza manutenção de credencial e trilha próprias. A execução e os resultados na sombra estão [nao verificado]. `bin/sonda_leitura.sh:115–143`, `bin/sonda_leitura.sh:235–251`.

## 28/11 14:49 · sessao 06e8e544ee84 · - · - · origem=Ronald (tmux)

Os 24 campos numéricos persistidos estão em `app/ponto/services/fechamento.py:1199–1222`. O retorno de leitura contém os mesmos campos, mais mês e ano, em `app/ponto/services/fechamento.py:1165–1173` e `app/ponto/management/commands/aplicar_09_corte_b.py:67–74`. Cinco campos podem ser nulos por contrato da grade (`app/ponto/services/fechamento.py:1124–1136`).

A execução em lote nativa já visita cada colaborador do universo. Duas passadas independentes por empresa e competência mantêm a seleção real e evitam reutilizar objetos de batida que o motor muta (`app/ponto/services/fechamento.py:490–507,619–622,685–699,865–874`).

O método pré-hunk ocupa `app/ponto/motor_calculo_v2.py:1536–1602`. A inserção em memória após a linha 1584 reproduziu o hash pós-hunk `65f7046b2a1690e6673cc20e611fdaeb24abb00d` do anexo.

O launcher monta `juliani` na sombra, porém mantém arquivos operacionais próprios. A transação e o Python da sonda podem ser estritamente de leitura (`bin/sonda_leitura.sh:93–96,126–135,216–218,235–250`).

## 28/11 14:49 · sessao 62020b2ac988 · - · - · origem=Ronald (tmux)

A batida criada e a retratação chamam `_fechamento_por_evento`, que delega a `recalcular_por_evento`. O ajuste retroativo que materializa saída passa pela mesma porta da batida. A aprovação simples de justificativa invalida o fechamento. `app/ponto/registro_batida.py:170–198,287–302`, `app/ponto/services/justificativa.py:187–216,478–488`.

A edição ou regeneração de célula chama `_o_chao_do_dia_mudou`, que agenda um recálculo por competência afetada. A resposta validada usa `data_evento`, o aceite de folga usa o dia da folga, e `decidir_he` chama a porta pela data da decisão após gravá-la. `app/ponto/portas/celula.py:392,490–503,682`, `app/chamados/services/validacao.py:627–628,787–799`, `app/ponto/portas/he.py:206–230`.

O `on_commit` resolve a competência pela janela da empresa e chama `recalcular_fechamento_mes` com um PK em modo de escrita. Se o fato cai na competência 10, o escritor usa `save` ou `update` conforme o status. A guarda da cópia lista apenas 09/2026. `app/ponto/services/fechamento.py:37–51,510–513,623–630,1174–1227,1634–1644,1847–1864`, `app/ponto/services/dia_pago.py:714–731`. Ocorrências e valores efetivos em produção [nao verificado].

## 28/11 14:41 · sessao e1f21a73d56d · FERIADO-SEM-LIMITE-DO-SUPRIMIDO · leitura · origem=Code

**RESUMO**  
O “oráculo intocado” do corte é o calculador v3 de rubricas, `ponto/calculador/regras.py`. [PROMPTS.md:2956](/home/ronald/codex_ws/0f16e9470b5430e70fda70ef291fffc1f6c503af/app/docs/PROMPTS.md:2956) · [RELATO.md:430](/home/ronald/codex_ws/0f16e9470b5430e70fda70ef291fffc1f6c503af/app/docs/RELATO.md:430)  
A lavra `oraculo` usa esse calculador e lhe passa a projeção de folga trabalhada. Essa saída grava `DiaPago` de sombra. [app/ponto/services/fechamento.py:1292] · [app/ponto/calculador/alimentacao.py:571] · [app/ponto/services/fechamento.py:1311]  
Na folga promovida a feriado, a projeção usa `minutos_trabalhados / 60` para a HE100. O juiz F3 registrado responde se a dobra cabe. [app/ponto/calculador/alimentacao.py:363] · [app/core/juizes.py:172] · [app/core/juizes.py:320]

### 1. A que código o corte se refere?

O item 5 do RELATO identifica o código pelo nome e registra a comparação da c1: “oraculo v3 calculador/regras.py da 540, o v2 agora 480”. A resposta “oraculo intocado” está em `PROMPTS.md`. No código da cópia, `regras.do_dia` usa `dobra_de_feriado_do_dia` quando a dobra normal ainda não veio informada e o dia não recebeu rubricas da partição de folga. Esse helper devolve o trabalhado inteiro se `MotorBase._feriado_dobra_no_dia` der verdadeiro. [app/docs/RELATO.md:430] · [app/docs/PROMPTS.md:2956] · [app/ponto/calculador/regras.py:361] · [app/ponto/calculador/regras.py:855]

O BACKLOG registra o defeito da HE100 pelo trabalhado inteiro e liga a bateria v3 à L-110. LEIS e CORTES registram que os casos devem ter a resposta pela regra e que a comparação com o cálculo anterior mede impacto. A inclusão efetiva de todos os casos `S1–S5/C1–C7` na bateria desta cópia fica [nao verificado]. [app/docs/BACKLOG.md:2022] · [app/docs/BACKLOG.md:1707] · [app/docs/LEIS.md:136] · [app/docs/CORTES.md:383] · [app/docs/PROMPTS.md:2956]

### 2. Quem chama `_rubricas_da_particao_ft` e o que sai?

É **o mesmo calculador de rubricas**, com duas peças no fluxo. `rubricas_por_dia` chama `_rubricas_da_particao_ft` quando recebe a partição e entrega o mapa do dia a `regras.do_dia` via `rubricas_ft`. No ramo de folga trabalhada, `do_dia` usa os valores desse mapa. O teste de porta consulta explicitamente as versões `motor` e `oraculo` de `DiaPago` e confere o mensal e os eventos de exportação em separado. [app/ponto/calculador/alimentacao.py:518] · [app/ponto/calculador/alimentacao.py:571] · [app/ponto/calculador/regras.py:767] · [app/ponto/calculador/regras.py:788] · [app/ponto/tests/test_feriado_um_juiz_porta.py:67] [nao verificado]

- **Recalcular o fechamento.** A partir da comp. 11/2026, o caminho de escrita passa a partição para `rubricas_por_dia` e entrega o resultado a `lavrar(..., versao='oraculo')`. A lavra atribui as rubricas aos campos do dia e substitui as linhas `DiaPago` daquela versão, incluindo a linha de ajuste da competência. [app/folha/export.py:385] · [app/ponto/services/fechamento.py:603] · [app/ponto/services/fechamento.py:1228] · [app/ponto/services/fechamento.py:1292] · [app/ponto/services/fechamento.py:1311] · [app/ponto/services/dia_pago.py:331] · [app/ponto/services/dia_pago.py:354] · [app/ponto/services/dia_pago.py:370]
- **Comando `diff_calculador`.** Chama o helper diretamente para montar o lado motor nos dias com folga trabalhada e também pode alcançá-lo via `rubricas_por_dia` no lado calculador. Imprime a comparação por rubrica e, com `--csv`, escreve as divergências em arquivo. O comando é de diagnóstico e roda por ato. [app/ponto/management/commands/diff_calculador.py:255] · [app/ponto/management/commands/diff_calculador.py:406] · [app/ponto/management/commands/diff_calculador.py:541] · [app/ponto/management/commands/diff_calculador.py:646] · [app/config/crons.py:1040]

**Folha, TXT e holerite.** O fechamento mensal e a lavra `motor` recebem a mesma partição e fazem suas próprias somas da folga promovida. O TXT da tela lê `horas_extras_100_feriado` do `FechamentoMensal` calculado em memória. O espelho lê `DiaPago` na versão padrão `motor`. O módulo de holerite armazena e entrega o PDF recebido. O valor efetivamente pago ou impresso em produção fica [nao verificado]. [app/ponto/services/fechamento.py:995] · [app/ponto/services/fechamento.py:1237] · [app/ponto/services/fechamento.py:1263] · [app/ponto/services/dia_pago.py:238] · [app/folha/views.py:215] · [app/folha/views.py:276] · [app/folha/export.py:434] · [app/ponto/services/espelho.py:420] · [app/ponto/services/dia_pago.py:845] · [app/holerite/motor.py:473] · [app/holerite/views.py:1445]

### 3. Quem decide a dobra e quem calcula o valor?

**A classificação vem da autoridade.** `particionar_ft` consulta o ciclo pela constante registrada, chama `motor_turno._feriado_dobra_no_dia(dia_turno)` e usa o resultado na escolha da rubrica. O registro vivo `_F3` aponta para `CICLOS_FERIADO_SIMPLES`, que contém `12x36`. `JUIZES.md` confirma o endereço. O predicado diário combina o flag `feriado_em_dobra` com `eh_feriado(dia)`. [app/ponto/calculador/regras.py:314] · [app/ponto/calculador/regras.py:410] · [app/ponto/calculador/regras.py:428] · [app/core/juizes.py:172] · [app/core/juizes.py:320] · [app/docs/JUIZES.md:114] · [app/ponto/motor_calculo_v2.py:3740] · [app/ponto/motor_calculo_v2.py:961]

**O valor da folga promovida é calculado no helper.** Para essa parcela, ele lança `periodo.minutos_trabalhados / 60` em `horas_extras_100_feriado` e `horas_extras_100`, sem descontar o intra suprimido nessa conta. Para os períodos normais que caem no mesmo dia, ele soma o `minutos_extra_100_feriado` já apurado pelo motor. A diferença está na origem do **valor**, depois de decidida a **rubrica**. [app/ponto/calculador/alimentacao.py:363] · [app/ponto/calculador/alimentacao.py:370] · [app/ponto/calculador/alimentacao.py:379]

## 28/11 14:30 · sessao 7d83c1dc9dbb · C7-TELA-ADMIN-ENCERRA · sonda · origem=Code

**RESUMO — dado raspado fornecido [nao verificado em nova execução]**  
88 perguntas no alvo, em 7 disputas.  
7 disputas com irmãs vivas fora do alvo, 420 irmãs vivas fora do alvo e 0 perguntas em disputa já fechada.  
Motivos: `saida_sem_entrada` 122, `orfao_14h` 106, `intervalo_volta` 95, `intervalo_saida` 91, `sem_posto` 1, `batida_fora_escala_grave` 5.  
Distribuição: `{53: 1, 21: 1, 46: 1, 70: 1, 17: 1, 210: 1, 3: 1}`. As perguntas `61520` e `61521` estão no alvo, na disputa `7979`, com 3 irmãs vivas fora do alvo cada.

**Saída raspada fornecida, na íntegra** [nao verificado em nova execução]

```text
sonda_leitura: sonda.py [20704dd2b53e1772] no cpuset 4-7, lendo sombra como leitor (LEITURA)

alvo 88 disputas 7
disputas_com_irma_viva_fora_do_alvo 7 irmas_vivas_fora_do_alvo 420 perguntas_em_disputa_ja_fechada 0
motivos_das_irmas {'saida_sem_entrada': 122, 'orfao_14h': 106, 'intervalo_volta': 95, 'intervalo_saida': 91, 'sem_posto': 1, 'batida_fora_escala_grave': 5}
distribuicao_irmas_por_disputa {53: 1, 21: 1, 46: 1, 70: 1, 17: 1, 210: 1, 3: 1}
pergunta 61520 no_alvo True disputa 7979 irmas_vivas_fora_do_alvo 3
pergunta 61521 no_alvo True disputa 7979 irmas_vivas_fora_do_alvo 3
```

A tentativa local parou antes de executar a sonda, com código de saída `1` e erro `sonda_leitura: ./sonda.py nao existe`. A tentativa não produziu contagens. `bin/sonda_leitura.sh:71`

**Conferência do critério.** Pelo critério descrito no pedido, sim: o predicado de vida é o mesmo. `admin_encerra` chama `fechar` com `FECHOU_ADMIN_ENCERRA`. `fechar` aplica `chamados.juizes.pergunta_viva(q)` a todas as perguntas da disputa, reavalia a lista sob a trava e chama `fechar_por_admin` para cada viva, inclusive as que estão fora do alvo da sonda. Respostas sem veredito podem bloquear o fechamento antes desse laço. `app/chamados/models.py:2217`, `app/chamados/models.py:2078`, `app/chamados/models.py:2122`, `app/chamados/models.py:2123`, `app/chamados/models.py:2132`, `app/chamados/models.py:2175`, `app/chamados/models.py:2184`, `app/chamados/juizes.py:911`

A implementação do recorte e os argumentos usados em `sonda.py` ficam [nao verificado].

## 28/11 14:23 · sessao d8f55c0f4fab · E4C-F5-INSTANTE · leitura · origem=Code

Validei em memória o delta que recupera os vínculos aninhados e faz o gate acusar `projecao=?`. `app/core/censo_escritas.py:284` · `app/chamados/tests/test_chokepoint_chamado_gate.py:322` no delta.  
**5 REDs** deram 6 falhas de asserção e zero erros na cópia. A cura passou **22/22** métodos com `unittest.TestCase` e os analisadores reais. `app/chamados/tests/test_e4c_f5_c4_red.py:1551` · `app/chamados/tests/test_chokepoint_chamado_gate.py:347` na cópia.  
**4/4 mutantes mortos** por asserção. Censo **543 → 543**, com igualdade das tuplas completas. `app/core/censo_escritas.py:611` na cópia.  
Próximo passo do aceite é a conferência pela sessão. Sessão, suíte Django e execução da base `c9fded10a` [nao verificado].

### Vínculos e REDs

A leitura única dos registros de `ligacoes` fica em `_Ligacao.ler`, com a iteração em `app/core/censo_escritas.py:284` no delta. A inferência em `:469` e a projeção em `:590` passam por ela. Para atribuições simples, anotadas e nomeadas, ambas chegam ao mesmo RHS armazenado em `vinculo.valor`. `app/core/censo_escritas.py:253,420,430,442,609` no delta.

O trecho da cópia em `:340` passa a propagar os ramos em `:416` do delta, e o de `:363` passa a registrar o RHS sem condicioná-lo à profundidade em `:440`. A trava de `:519` dá lugar à seleção das escritas anteriores ao uso, da mais recente para a mais antiga. A primeira escrita garantida encerra a busca. Candidatos que levam a RHS distintos geram `projecao=?`. O controle verifica `p` e `q` nos respectivos ramos e a indeterminação após a junção. `app/core/censo_escritas.py:340,363,519` na cópia · `app/core/censo_escritas.py:595,609,630,641` · `app/chamados/tests/test_e4c_f5_c4_red.py:1663,1682` no delta.

Cada hunk abaixo teve **1** ocorrência do trecho original na cópia. O conjunto passou em `git apply --check -p1`. `app/core/censo_escritas.py:58,204,311,359,376,498` · `app/chamados/tests/test_e4c_f5_c4_red.py:1660` · `app/chamados/tests/test_chokepoint_chamado_gate.py:318,424` na cópia.

```diff
--- a/app/core/censo_escritas.py
+++ b/app/core/censo_escritas.py
@@ -58,6 +58,7 @@
 import ast
 import os
 from collections import deque
+from typing import NamedTuple
 
 CRIAR = frozenset({'create', 'get_or_create', 'update_or_create', 'bulk_create'})
 ALTERAR = frozenset({'update', 'bulk_update'})
@@ -204,11 +205,83 @@
 )
 
 
+_NOS_TRY = (ast.Try, ast.TryStar)
+_NOS_COM_RAMOS = (
+    ast.If, ast.IfExp, ast.Match, ast.With, ast.AsyncWith,
+    ast.For, ast.AsyncFor, ast.While, *_NOS_TRY,
+)
+
+
+def _inicio(no):
+    return no.lineno, no.col_offset
+
+
+def _fim(no):
+    return no.end_lineno, no.end_col_offset
+
+
+def _contem(bloco, ponto):
+    return bloco and _inicio(bloco[0]) <= ponto < _fim(bloco[-1])
+
+
+def _filhos_com_ramos(no):
+    for campo, filhos in ast.iter_fields(no):
+        if not isinstance(filhos, list):
+            filhos = (filhos,)
+        for filho in filhos:
+            if not isinstance(filho, ast.AST):
+                continue
+            if isinstance(no, ast.BoolOp) and campo == 'values' and filho is not filhos[0]:
+                yield filho, ((no, campo, (filho,)),)
+            elif (isinstance(no, _NOS_COM_RAMOS)
+                  and campo in ('body', 'orelse', 'handlers', 'finalbody', 'cases')):
+                bloco = (filho,) if campo in ('handlers', 'cases') else filhos
+                yield filho, ((no, campo, bloco),)
+            else:
+                yield filho, ()
+
+
+class _Vinculo(NamedTuple):
+    ordem: int
+    local: bool
+    valor: ast.AST | None
+    ponto: tuple[int, int]
+    ramos: tuple
+    outra_evidencia: ast.AST | None = None
+    valor_e_evidencia: bool = True
+
+    def evidencia(self):
+        return self.valor if self.valor_e_evidencia else self.outra_evidencia
+
+    def alcance(self, uso):
+        # True = executado ao chegar ao uso; False = possivel; None = outro ramo.
+        garantido = True
+        for no, campo, bloco in self.ramos:
+            if _contem(bloco, uso) or campo == 'finalbody':
+                continue
+            if uso < _fim(no):
+                if isinstance(no, (ast.If, ast.IfExp, ast.Match)):
+                    return None
+                if isinstance(no, _NOS_TRY):
+                    if campo == 'body' and _contem(no.orelse, uso):
+                        continue
+                    if campo == 'handlers' and (
+                        _contem(no.orelse, uso)
+                        or (isinstance(no, ast.Try) and not _contem(no.finalbody, uso))
+                    ):
+                        return None
+            garantido = False
+        return garantido
+
+
 class _Ligacao:
     def __init__(self):
-        self.vinculos = []  # (ordem, local, valor da projecao, evidencia do modelo)
+        self.vinculos = []
         self.campos = set()
         self.origem = None
+
+    def ler(self, locais=False):
+        return (v for v in self.vinculos if v.local or not locais)
 
 
 class _Varredor(ast.NodeVisitor):
@@ -311,46 +384,50 @@
                 self.modulos.add(a.asname or a.name)
 
     # -- escopos
-    def _ligar_alvo(self, ligacoes, alvo, valor, origem, local, ordem):
+    def _ligar_alvo(self, ligacoes, alvo, vinculo):
         if isinstance(alvo, ast.Name):
-            ligacoes.setdefault(alvo.id, _Ligacao()).vinculos.append((ordem, local, valor, origem))
+            ligacoes.setdefault(alvo.id, _Ligacao()).vinculos.append(vinculo)
         elif isinstance(alvo, ast.Starred):
-            self._ligar_alvo(ligacoes, alvo.value, None, None, local, ordem)
+            self._ligar_alvo(ligacoes, alvo.value, vinculo._replace(valor=None, outra_evidencia=None))
         elif isinstance(alvo, (ast.Tuple, ast.List)):
+            valor = vinculo.valor
             elts = ()
             if (isinstance(valor, (ast.Tuple, ast.List)) and len(alvo.elts) == len(valor.elts)
                     and not any(isinstance(e, ast.Starred) for e in (*alvo.elts, *valor.elts))):
                 elts = valor.elts
             for i, item in enumerate(alvo.elts):
-                evid = origem if i == 0 and isinstance(alvo, ast.Tuple) and isinstance(item, ast.Name) else None
-                self._ligar_alvo(ligacoes, item, elts[i] if elts else None, evid, local, ordem)
+                evidencia = (vinculo.evidencia()
+                             if i == 0 and isinstance(alvo, ast.Tuple) and isinstance(item, ast.Name)
+                             else None)
+                self._ligar_alvo(ligacoes, item, vinculo._replace(
+                    valor=elts[i] if elts else None, outra_evidencia=evidencia, valor_e_evidencia=False))
 
     def _colher_ligacoes(self, no, inferir):
-        # A origem do modelo usa a ordem em largura e pode ler escopos filhos,
-        # como antes. A tupla usa apenas os vinculos lexicais do escopo.
+        # A ordem em largura preserva a precedencia da inferencia de modelos.
         ligacoes = {}
-        fila = deque((sub, True, True) for sub in ast.iter_child_nodes(no))
+        fila = deque((sub, True, ramos) for sub, ramos in _filhos_com_ramos(no))
         ordem = 0
         while fila:
-            sub, local, direta = fila.popleft()
+            sub, local, ramos = fila.popleft()
             ordem += 1
             if not local and not inferir:
                 continue
             filhos_locais = local and not isinstance(sub, _NOS_DE_ESCOPO)
-            fila.extend((filho, filhos_locais, direta and isinstance(sub, ast.Expr))
-                        for filho in ast.iter_child_nodes(sub))
+            fila.extend((filho, filhos_locais, ramos + novos_ramos)
+                        for filho, novos_ramos in _filhos_com_ramos(sub))
             alvos, valor, origem = (), None, None
             if isinstance(sub, ast.Assign):
-                alvos, valor, origem = sub.targets, sub.value, sub.value
+                alvos, valor = sub.targets, sub.value
                 for alvo in alvos:
                     if (inferir and isinstance(alvo, ast.Attribute) and isinstance(alvo.value, ast.Name)
                             and alvo.attr in self.unicos):
                         ligacoes.setdefault(alvo.value.id, _Ligacao()).campos.add(self.unicos[alvo.attr])
             elif isinstance(sub, ast.AnnAssign) and sub.value is None:
                 if local and isinstance(sub.target, ast.Name):
-                    self._ligar_alvo(ligacoes, sub.target, sub, None, local, ordem)
+                    self._ligar_alvo(ligacoes, sub.target, _Vinculo(
+                        ordem, local, sub, _fim(sub), ramos, valor_e_evidencia=False))
             elif isinstance(sub, (ast.AnnAssign, ast.NamedExpr)):
-                alvos, valor, origem = (sub.target,), sub.value, sub.value
+                alvos, valor = (sub.target,), sub.value
             elif isinstance(sub, ast.AugAssign):
                 alvos, origem = (sub.target,), sub.value
             elif isinstance(sub, (ast.For, ast.AsyncFor, ast.comprehension)):
@@ -359,8 +436,13 @@
                 alvos, origem = (sub.optional_vars,), sub.context_expr
             elif isinstance(sub, (ast.Delete, ast.TypeAlias)):
                 alvos = sub.targets if isinstance(sub, ast.Delete) else (sub.name,)
-            for alvo in alvos:
-                self._ligar_alvo(ligacoes, alvo, valor if direta else None, origem, local, ordem)
+            if alvos:
+                vinculo = _Vinculo(
+                    ordem, local, valor, _fim(valor or origem or sub), ramos,
+                    outra_evidencia=origem, valor_e_evidencia=origem is None,
+                )
+                for alvo in alvos:
+                    self._ligar_alvo(ligacoes, alvo, vinculo)
             if not local:
                 continue
             nomes = ()
@@ -376,14 +458,15 @@
                                   ast.MatchAs, ast.MatchStar, ast.TypeVar, ast.ParamSpec, ast.TypeVarTuple)):
                 nomes = (sub.name,) if sub.name is not None else ()
             for nome in nomes:
-                ligacoes.setdefault(nome, _Ligacao()).vinculos.append((ordem, True, None, None))
+                ligacoes.setdefault(nome, _Ligacao()).vinculos.append(
+                    _Vinculo(ordem, local, None, _inicio(sub), ramos))
         return ligacoes
 
     def _inferir_amarras(self, ligacoes):
         # Ponto fixo sobre os mesmos vinculos: qs = X.objects.filter(); for b in qs: b.save().
         evidencias = sorted(
-            ((ordem, nome, valor) for nome, ligacao in ligacoes.items()
-             for ordem, _, _, valor in ligacao.vinculos if valor is not None),
+            ((v.ordem, nome, evidencia) for nome, ligacao in ligacoes.items()
+             for v in ligacao.ler() if (evidencia := v.evidencia()) is not None),
             key=lambda item: item[0],
         )
         mudou = True
@@ -498,31 +581,57 @@
         return (isinstance(classe, ast.ClassDef) and classe.name == 'DecisaoPergunta'
                 and isinstance(metodo, ast.FunctionDef) and metodo.name == 'registrar')
 
+    def _candidatos_da_projecao(self, nome, limite):
+        for i in range(limite, -1, -1):
+            escopo, ligacoes = self._escopos[i]
+            if isinstance(escopo, ast.ClassDef) and i != limite:
+                continue
+            ligacao = ligacoes.get(nome.id)
+            locais = list(ligacao.ler(locais=True)) if ligacao is not None else []
+            if locais:
+                break
+        else:
+            return []
+
+        uso = _inicio(nome)
+        escritas = [v for v in locais if not isinstance(v.valor, ast.AnnAssign)]
+        # Um fechamento pode ser chamado depois de uma escrita textual posterior ao seu corpo.
+        if i != limite and any(v.ponto > uso for v in escritas):
+            return []
+        anteriores = sorted(
+            (v for v in escritas if v.ponto <= uso),
+            key=lambda v: (v.ponto, v.ordem), reverse=True,
+        )
+        candidatos = []
+        for vinculo in anteriores:
+            alcance = vinculo.alcance(uso)
+            if alcance is not None:
+                candidatos.append((vinculo.valor, i))
+                if alcance:
+                    break
+        return candidatos
+
     def _valor_da_projecao(self, valor):
-        limite = len(self._escopos) - 1
-        while isinstance(valor, (ast.Name, ast.NamedExpr)):
+        pendentes = [(valor, len(self._escopos) - 1)]
+        visitados = set()
+        encontrado = None
+        while pendentes:
+            valor, limite = item = pendentes.pop()
+            if item in visitados:
+                continue
+            visitados.add(item)
             if isinstance(valor, ast.NamedExpr):
-                valor = valor.value
-                continue
-            for i in range(limite, -1, -1):
-                escopo, ligacoes = self._escopos[i]
-                if isinstance(escopo, ast.ClassDef) and i != limite:
-                    continue
-                ligacao = ligacoes.get(valor.id)
-                if ligacao is not None:
-                    locais = [v for _, local, v, _ in ligacao.vinculos if local]
-                    if locais:
-                        break
+                pendentes.append((valor.value, limite))
+            elif isinstance(valor, ast.Name):
+                candidatos = self._candidatos_da_projecao(valor, limite)
+                if not candidatos:
+                    return None
+                pendentes.extend(candidatos)
+            elif valor is None or (encontrado is not None and valor is not encontrado):
+                return None
             else:
-                return None
-            locais = [v for v in locais if not isinstance(v, ast.AnnAssign)]
-            if len(locais) != 1 or locais[0] is None:
-                return None
-            ligado = locais[0]
-            if (ligado.end_lineno, ligado.end_col_offset) > (valor.lineno, valor.col_offset):
-                return None
-            valor, limite = ligado, i
-        return valor
+                encontrado = valor
+        return encontrado
 
     def _projecao(self, no, valor):
         """`projecao=(inst, campos)` = `inst.save(update_fields=campos)` neste sitio (`ESCRITA_POR_PROJECAO`)."""
--- a/app/chamados/tests/test_e4c_f5_c4_red.py
+++ b/app/chamados/tests/test_e4c_f5_c4_red.py
@@ -1660,6 +1660,77 @@
                     (linha, None, 'save', 'DP.registrar(projecao=?) [projecao]'),
                 ])
 
+    def test_RED_projecao_ligada_em_if_elif_else(self):
+        fonte = (
+            'from chamados.models import DecisaoPergunta as DP\n'
+            'def f(p, q, linhas, a, b):\n'
+            '    if a:\n'
+            "        proj = (p, ['respondida_em'])\n"
+            '        DP.registrar(linhas, projecao=proj)\n'
+            '    elif b:\n'
+            "        proj = (p, ['respondida_em'])\n"
+            '        DP.registrar(linhas, projecao=proj)\n'
+            '    else:\n'
+            "        proj = (q, ['respondida_em'])\n"
+            '        DP.registrar(linhas, projecao=proj)\n'
+            '    DP.registrar(linhas, projecao=proj)\n'
+        )
+        self.assertEqual(self._varrer(fonte), [
+            (5, 'PerguntaDisputa', 'save', 'p.save() [update_fields] [projecao]'),
+            (8, 'PerguntaDisputa', 'save', 'p.save() [update_fields] [projecao]'),
+            (11, 'PerguntaDisputa', 'save', 'q.save() [update_fields] [projecao]'),
+            (12, None, 'save', 'DP.registrar(projecao=?) [projecao]'),
+        ])
+
+    def test_RED_projecao_ligada_em_try_except_else_finally(self):
+        fonte = (
+            'from chamados.models import DecisaoPergunta as DP\n'
+            'def f(p, q, r, linhas):\n'
+            '    try:\n'
+            "        proj = (p, ['respondida_em'])\n"
+            '        DP.registrar(linhas, projecao=proj)\n'
+            '    except ValueError:\n'
+            "        proj = (q, ['validada_em'])\n"
+            '        DP.registrar(linhas, projecao=proj)\n'
+            '    else:\n'
+            '        DP.registrar(linhas, projecao=proj)\n'
+            "        proj = (r, ['respondida_em'])\n"
+            '        DP.registrar(linhas, projecao=proj)\n'
+            '    finally:\n'
+            "        proj = (p, ['respondida_em'])\n"
+            '        DP.registrar(linhas, projecao=proj)\n'
+            '    DP.registrar(linhas, projecao=proj)\n'
+        )
+        self.assertEqual(self._varrer(fonte), [
+            (linha, 'PerguntaDisputa', 'save', f'{inst}.save() [update_fields] [projecao]')
+            for linha, inst in ((5, 'p'), (8, 'q'), (10, 'p'), (12, 'r'), (15, 'p'), (16, 'p'))
+        ])
+
+    def test_RED_projecao_ligada_em_with(self):
+        fonte = (
+            'from chamados.models import DecisaoPergunta as DP\n'
+            'def f(p, linhas):\n'
+            '    with contexto():\n'
+            "        proj = (p, ['respondida_em'])\n"
+            '        DP.registrar(linhas, projecao=proj)\n'
+        )
+        self.assertEqual(self._varrer(fonte), [
+            (5, 'PerguntaDisputa', 'save', 'p.save() [update_fields] [projecao]'),
+        ])
+
+    def test_RED_projecao_reatribuida_duas_vezes(self):
+        fonte = (
+            'from chamados.models import DecisaoPergunta as DP\n'
+            'def f(p, linhas):\n'
+            '    proj = obter()\n'
+            "    proj = (outro, ['respondida_em'])\n"
+            "    proj = (p, ['respondida_em'])\n"
+            '    DP.registrar(linhas, projecao=proj)\n'
+        )
+        self.assertEqual(self._varrer(fonte), [
+            (6, 'PerguntaDisputa', 'save', 'p.save() [update_fields] [projecao]'),
+        ])
+
     def test_os_dois_saves_da_porta_ficam_visiveis(self):
         import ast
 
```

### Balde

Na cópia, `tabela` recolhe `entidade=None` e o comando imprime esses achados com `--nao-atribuiveis`. O gate de chamado pula `None` em `:321`. O delta faz o gate consumir qualquer `None` com sufixo `[projecao]` antes do filtro de portas. O RED exige a acusação para ramos divergentes e para tupla cujo receptor ficou sem entidade, no próprio `chamados/models.py`. O `p.save()` comum da fixture continua sujeito ao filtro de `None`. `app/core/censo_escritas.py:649` · `app/core/management/commands/censo_escritas.py:37` · `app/chamados/tests/test_chokepoint_chamado_gate.py:321` · `app/core/portas.py:396` na cópia · `app/chamados/tests/test_chokepoint_chamado_gate.py:322,430,447` no delta.

```diff
--- a/app/chamados/tests/test_chokepoint_chamado_gate.py
+++ b/app/chamados/tests/test_chokepoint_chamado_gate.py
@@ -318,6 +318,9 @@
     ruins = []
     for x in achados:
         ent = x['entidade']
+        if ent is None and x['expr'].endswith(' [projecao]'):
+            ruins.append('%s:%d projecao=? %s' % (x['arquivo'], x['linha'], x['expr']))
+            continue
         if ent is None or portas.familia_de(ent) != FAMILIA:
             continue
         if x['arquivo'] in (portas.porta_de(ent) or ()):
@@ -424,6 +427,28 @@
         self.assertEqual(len(fora_da_porta(
             _achados(fonte, 'chamados/management/commands/x.py'))), 1)
 
+    def test_RED_o_gate_acusa_projecao_sem_entidade(self):
+        inicio = 'from chamados.models import DecisaoPergunta as DP\ndef f(p, q, linhas, flag):\n'
+        casos = (
+            ('ramos distintos',
+             '    if flag:\n'
+             "        proj = (p, ['respondida_em'])\n"
+             '    else:\n'
+             "        proj = (q, ['respondida_em'])\n"
+             '    DP.registrar(linhas, projecao=proj)\n',
+             7, 'DP.registrar(projecao=?) [projecao]'),
+            ('tupla sem entidade',
+             '    DP.registrar(linhas, projecao=(p, campos))\n',
+             3, 'p.save() [projecao]'),
+        )
+        arquivo = 'chamados/models.py'
+        for caso, corpo, linha, expr in casos:
+            with self.subTest(caso=caso):
+                fo

[truncado em 20000 de 24782 caracteres]

## 28/11 14:12 · sessao d7103919d556 · C7-TELA-ADMIN-ENCERRA · leitura · origem=Code

**Resumo** — Módulo completo abaixo, com 7 testes e 6 mutantes de um hunk. `app/chamados/urls.py:22`, `app/chamados/models.py:2210`  
Os seis diffs passaram em `git apply --check`. O selo AST passou na cópia e acusou o pai, a chamada removida e a escrita direta. `app/chamados/views_disputa_admin.py:49`, `app/chamados/views_disputa_admin.py:57`  
Próximo na sessão: rodar o módulo em `b82a5ef08`, em `d04973b65` e com cada mutante isolado. HTTP, ORM e trilha em execução seguem [nao verificado].

## 1. `app/chamados/tests/test_c7_tela_admin_encerra.py`

Copiei o molde de `TestCase` com `Empresa`, `Praca`, `Posto` e `Colaborador` da base C7, o usuário `sistema` do teste de reabertura vizinho e o relógio de `2026-10-22 15:00:00` congelado com `freezegun` da C7. A sessão `via_app` segue o teste das cascas. `app/chamados/tests/test_c7_red_minimos.py:88`, `app/chamados/tests/test_c7_red_minimos.py:92`, `app/chamados/tests/test_c7_red_minimos.py:127`, `app/chamados/tests/test_reabrir_disputa_admin_sem_aberta.py:9`, `app/core/tests/test_casca_telas_colab.py:35`

A recusa de `fechar` e o POST válido exercitam a mesma disputa muda. No pai, a recusa já existe e o RED esperado é a asserção de que o POST delegou à porta. As URLs literais deixam o teste do link chegar ao HTML da ficha. No teste de permissão, o controle positivo com staff impede que o 404 de rota ausente satisfaça o teste inteiro. `b82a5ef08988438256b90c5d0b9d94f4552156e5/app/chamados/models.py:2141`, `b82a5ef08988438256b90c5d0b9d94f4552156e5/app/chamados/urls.py:21`, `b82a5ef08988438256b90c5d0b9d94f4552156e5/app/templates/chamados/fio_colaborador.html:9`, `app/chamados/views_disputa_admin.py:36`

```python
import ast
import re
from datetime import date, datetime, timezone
from pathlib import Path
from unittest.mock import patch

from django.conf import settings
from django.contrib.auth.models import Group, User
from django.test import TestCase
from freezegun import freeze_time

from chamados.models import DecisaoPergunta, DisputaSupervisao, PerguntaDisputa
from colaboradores.models import Colaborador, Empresa, Posto, Praca
from core.models import LogAuditoria


RELOGIO = '2026-10-22 15:00:00'
INSTANTE = datetime.fromisoformat(RELOGIO).replace(tzinfo=timezone.utc)
MOTIVO = 'Plantão conferido com a supervisão'
SUCESSO = 'A disputa foi encerrada e o motivo ficou registrado.'


@freeze_time(RELOGIO)
class C7TelaAdminEncerraTest(TestCase):
    @classmethod
    def setUpTestData(cls):
        User.objects.get_or_create(username='sistema')
        empresa = Empresa.objects.create(
            razao_social='C7 TELA LTDA', cnpj='19131243000197', ativa=True,
        )
        praca = Praca.objects.create(nome='Praça C7', cidade='Londrina', estado='PR')
        posto = Posto.objects.create(nome='Posto C7', empresa=empresa, praca=praca)
        cls.staff = User.objects.create_user('c7_tela_staff', is_staff=True)
        cls.admin = User.objects.create_user('c7_tela_admin')
        cls.gestor = User.objects.create_user('c7_tela_gestor')
        cls.admin.groups.add(Group.objects.get_or_create(name='admin_ti')[0])
        cls.gestor.groups.add(Group.objects.get_or_create(name='gestor')[0])
        cls.colab = Colaborador.objects.create(
            nome='Colaborador C7 Tela', cpf='[cpf]', empresa=empresa,
            posto=posto, situacao='ativo', data_admissao=date(2026, 1, 1),
        )

    def setUp(self):
        self.ficha_url = f'/chamados/colaborador/{self.colab.pk}/'
        self.lista_url = f'{self.ficha_url}disputas/'
        self.encerrar_url = f'{self.lista_url}encerrar/'
        self.disputa = DisputaSupervisao.objects.create(
            colaborador=self.colab, motivos=['orfao_14h', 'intervalo_volta'],
        )
        self.pergunta = PerguntaDisputa.objects.create(
            disputa=self.disputa, motivo='orfao_14h', data_evento=date(2026, 10, 20),
            pergunta='A que horas terminou o plantão?', tipo_resposta='hora',
        )

    def _estado(self):
        self.disputa.refresh_from_db()
        return (
            self.disputa.fechada_em, self.disputa.fechada_por_id,
            self.disputa.resolucao, self.disputa.motivo_fechamento,
        )

    def _trilha(self):
        return LogAuditoria.objects.filter(
            modelo='DisputaSupervisao', objeto_id=self.disputa.pk, acao='disputa_fechada',
        )

    def _assert_aberta_sem_trilha(self):
        self.assertEqual(self._estado(), (None, None, '', ''))
        self.pergunta.refresh_from_db()
        self.assertIsNone(self.pergunta.materializada_em)
        self.assertIsNone(self.pergunta.materializada_por_id)
        self.assertEqual(self.pergunta.via_resolucao, '')
        self.assertFalse(self.pergunta.decisoes.exists())
        self.assertFalse(self._trilha().exists())

    def _login_app(self, ator):
        self.client.force_login(ator)
        sessao = self.client.session
        sessao['via_app'] = True
        sessao['plataforma_sessao'] = 'app'
        sessao.save()

    def test_ficha_mostra_link_so_para_staff_ou_admin_ti(self):
        link = rf'<a\b[^>]*\bhref="{re.escape(self.lista_url)}"[^>]*>\s*Disputas\s*</a>'
        for ator in (self.staff, self.admin):
            with self.subTest(ator=ator.username):
                self.client.force_login(ator)
                resposta = self.client.get(self.ficha_url)
                self.assertEqual(resposta.status_code, 200)
                self.assertRegex(resposta.content.decode(), link)
        self.client.force_login(self.gestor)
        resposta = self.client.get(self.ficha_url)
        self.assertEqual(resposta.status_code, 200)
        self.assertNotRegex(resposta.content.decode(), link)

    def test_lista_todas_as_perguntas_antes_do_form(self):
        irma = PerguntaDisputa.objects.create(
            disputa=self.disputa, motivo='intervalo_volta', data_evento=date(2026, 10, 19),
            pergunta='A que horas voltou do intervalo?', tipo_resposta='hora',
            resposta_colab='13:00', respondida_em=INSTANTE,
        )
        esperadas = ((self.pergunta, 'Pendente'), (irma, 'Respondida'))
        self._login_app(self.admin)
        resposta = self.client.get(self.lista_url)
        self.assertContains(resposta, f'id="disputa-{self.disputa.pk}"')
        self.assertTemplateUsed(resposta, 'base_app.html')

        antes, inicio_form, formulario = resposta.content.decode().partition(
            f'<form method="post" action="{self.encerrar_url}">',
        )
        self.assertTrue(inicio_form, 'O formulário precisa apontar para a porta HTTP.')
        secao = antes.split(f'id="disputa-{self.disputa.pk}"', 1)[1]
        itens = re.findall(
            r'<li\b[^>]*>\s*<strong>Pergunta #(\d+)</strong>(.*?)</li>', secao, re.S,
        )
        self.assertCountEqual([pk for pk, _ in itens], [str(p.pk) for p, _ in esperadas])
        por_pk = dict(itens)
        for pergunta, estado in esperadas:
            item = por_pk[str(pergunta.pk)]
            self.assertIn(estado, item)
            self.assertIn(f'Marco: {pergunta.instancia_marco}', item)
            self.assertIn(f'Dia: {pergunta.data_evento:%d/%m/%Y}', item)
            self.assertIn(pergunta.pergunta, item)

        formulario = formulario.split('</form>', 1)[0]
        self.assertIn(f'name="disputa_pk" value="{self.disputa.pk}"', formulario)
        self.assertIn('name="csrfmiddlewaretoken"', formulario)
        self.assertRegex(formulario, r'<textarea\b[^>]*\bname="motivo"[^>]*\brequired\b')
        self.assertIn('Encerrar disputa com motivo', formulario)

    def test_fechar_recusa_muda_e_post_com_motivo_delega(self):
        with self.assertRaisesRegex(ValueError, 'Encerrar com motivo'):
            self.disputa.fechar(self.staff)
        self._assert_aberta_sem_trilha()

        self.client.force_login(self.staff)
        motivo_postado = f'  {MOTIVO}  '
        porta_real = DisputaSupervisao.admin_encerra
        with patch.object(DisputaSupervisao, 'admin_encerra', autospec=True, side_effect=porta_real) as porta:
            resposta = self.client.post(self.encerrar_url, {
                'disputa_pk': self.disputa.pk, 'motivo': motivo_postado,
            })
        porta.assert_called_once_with(self.disputa, self.staff, motivo_postado)
        self.assertContains(resposta, SUCESSO)
        self.assertTemplateUsed(resposta, 'base.html')
        self.assertEqual(
            self._estado(),
            (INSTANTE, self.staff.pk, MOTIVO, DisputaSupervisao.FECHOU_ADMIN_ENCERRA),
        )
        self.pergunta.refresh_from_db()
        self.assertEqual(self.pergunta.via_resolucao, PerguntaDisputa.VIA_ADMIN_DECIDIU)
        self.assertEqual(self.pergunta.materializada_por_id, self.staff.pk)
        self.assertEqual(self.pergunta.materializada_em, INSTANTE)
        self.assertEqual(list(self.pergunta.decisoes.values_list('ato', 'ato_por_id', 'ato_motivo')), [
            (DecisaoPergunta.SILENCIO, self.staff.pk, f'admin_encerra: {MOTIVO}'),
        ])
        self.assertEqual(list(self._trilha().values_list('usuario_id', 'valor_depois')), [
            (self.staff.pk, {'motivo_fechamento': 'admin_encerra', 'resolucao': MOTIVO}),
        ])

    def test_post_sem_motivo_mostra_recusa_e_preserva_disputa(self):
        self.client.force_login(self.staff)
        porta_real = DisputaSupervisao.admin_encerra
        for vazio in (None, '', ' \t  '):
            with self.subTest(motivo=vazio):
                dados = {'disputa_pk': self.disputa.pk}
                if vazio is not None:
                    dados['motivo'] = vazio
                with patch.object(
                    DisputaSupervisao, 'admin_encerra', autospec=True, side_effect=porta_real,
                ) as porta:
                    resposta = self.client.post(self.encerrar_url, dados)
                porta.assert_called_once_with(self.disputa, self.staff, vazio)
                self._assert_aberta_sem_trilha()
                self.assertContains(resposta, 'exige o MOTIVO escrito')

    def test_reenvio_delega_e_preserva_uma_linha_de_trilha(self):
        self.assertIs(self.disputa.admin_encerra(self.admin, MOTIVO), True)
        estado = self._estado()
        trilha = list(self._trilha().values_list('pk', flat=True))
        decisoes = self.pergunta.decisoes.values_list('pk', 'ato', 'ato_em', 'ato_motivo')
        antes = list(decisoes.all())
        self.assertEqual(len(trilha), 1)
        self.assertEqual([linha[1] for linha in antes], [DecisaoPergunta.SILENCIO])

        retornos = []
        porta_real = DisputaSupervisao.admin_encerra

        def observar(disputa, ator, motivo):
            retorno = porta_real(disputa, ator, motivo)
            retornos.append(retorno)
            return retorno

        self._login_app(self.admin)
        motivo_postado = f'  {MOTIVO}  '
        with freeze_time('2026-10-22 16:00:00'), patch.object(
            DisputaSupervisao, 'admin_encerra', autospec=True, side_effect=observar,
        ) as porta:
            resposta = self.client.post(self.encerrar_url, {
                'disputa_pk': self.disputa.pk, 'motivo': motivo_postado,
            })
        porta.assert_called_once_with(self.disputa, self.admin, motivo_postado)
        self.assertEqual(retornos, [False])
        self.assertContains(resposta, 'Nada mudou: a disputa já estava encerrada com este motivo.')
        self.assertTemplateUsed(resposta, 'base_app.html')
        self.assertEqual(self._estado(), estado)
        self.assertEqual(list(self._trilha().values_list('pk', flat=True)), trilha)
        self.assertEqual(list(decisoes.all()), antes)
        self.pergunta.refresh_from_db()
        self.assertEqual(self.pergunta.materializada_em, INSTANTE)

    def test_sem_permissao_recusa_get_e_post_sem_tocar_na_disputa(self):
        dados = {'disputa_pk': self.disputa.pk, 'motivo': MOTIVO}
        self.client.force_login(self.gestor)
        porta_real = DisputaSupervisao.admin_encerra
        with patch.object(DisputaSupervisao, 'admin_encerra', autospec=True, side_effect=porta_real) as porta:
            lista = self.client.get(self.lista_url)
            escrita = self.client.post(self.encerrar_url, dados)
        self.assertIn(lista.status_code, (403, 404))
        self.assertIn(escrita.status_code, (403, 404))
        porta.assert_not_called()
        self._assert_aberta_sem_trilha()

        self.client.force_login(self.staff)
        self.assertEqual(self.client.get(self.encerrar_url).status_code, 405)
        self.assertEqual(
            self.client.post(self.encerrar_url, dados).status_code, 200,
            'O mesmo POST precisa estar disponível ao staff.',
        )

    def test_ast_view_so_delega_a_decisao_de_fechamento(self):
        fonte = Path(settings.BASE_DIR) / 'chamados' / 'views_disputa_admin.py'
        self.assertTrue(fonte.is_file(), 'Falta a view que delega o encerramento à porta.')
        nos = list(ast.walk(ast.parse(fonte.read_text(encoding='utf-8'))))
        chamadas = [no for no in nos if isinstance(no, ast.Call)]
        self.assertEqual([
            ast.unparse(no) for no in chamadas
            if isinstance(no.func, ast.Attribute) and no.func.attr == 'admin_encerra'
        ], ['disputa.admin_encerra(request.user, motivo)'])

        funcoes_http = {'_exigir_admin', '_renderizar_disputas', 'get_object_or_404', 'render', 'str'}
        self.assertEqual([
            (no.lineno, no.func.id) for no in chamadas
            if isinstance(no.func, ast.Name) and no.func.id not in funcoes_http
        ], [])
        atributos_http = {
            'user.is_staff', 'user.groups', "user.groups.filter(name='admin_ti').exists",
            'user.groups.filter', 'colaborador.disputas', 'colaborador.disputas.filter',
            'colaborador.disputas.filter(fechada_em__isnull=True).prefetch_related',
            'request.user', 'request.POST', 'request.POST.get', 're.fullmatch',
            'DisputaSupervisao.objects', 'DisputaSupervisao.objects.select_related',
            'disputa.admin_encerra', 'disputa.colaborador',
        }
        self.assertEqual([
            (no.lineno, ast.unparse(no)) for no in nos if isinstance(no, ast.Attribute)
            if not isinstance(no.ctx, ast.Load) or ast.unparse(no) not in atributos_http
        ], [])
        self.assertCountEqual(
            [ast.unparse(no.test) for no in nos if isinstance(no, ast.If)],
            [
                "not (user.is_staff or user.groups.filter(name='admin_ti').exists())",
                "disputa_pk is None or not re.fullmatch('[0-9]{1,19}', disputa_pk)",
                'encerrou',
            ],
        )
        self.assertEqual(
            [no.lineno for no in nos if isinstance(no, (ast.IfExp, ast.Match, ast.While))], [],
        )
```

## 2. Mutantes

**M1 — Remover a chamada à porta.** Devem falhar `test_fechar_recusa_muda_e_post_com_motivo_delega`, `test_post_sem_motivo_mostra_recusa_e_preserva_disputa`, `test_reenvio_delega_e_preserva_uma_linha_de_trilha` e `test_ast_view_so_delega_a_decisao_de_fechamento`. As três falhas HTTP são esperadas [nao verificado]. `app/chamados/views_disputa_admin.py:52`

```diff
--- a/app/chamados/views_disputa_admin.py
+++ b/app/chamados/views_disputa_admin.py
@@ -49,7 +49,7 @@
     motivo = request.POST.get('motivo')
     # A recusa de negócio da porta precisa chegar ao admin com o motivo original.
     try:
-        encerrou = disputa.admin_encerra(request.user, motivo)
+        encerrou = True
     except ValueError as e:
         tipo_mensagem = 'error'
         mensagem = str(e)
```

**M2 — Aceitar motivo ausente, vazio ou em branco.** Deve falhar `test_post_sem_motivo_mostra_recusa_e_preserva_disputa` na asserção de disputa aberta [nao verificado]. `app/chamados/models.py:2214`

```diff
--- a/app/chamados/models.py
+++ b/app/chamados/models.py
@@ -2212,8 +2212,6 @@
         OBRIGATORIO -- *"a opcao coerente que o admin nao tinha"*. As mudas saem com `admin_decidiu` e trilha
         (quem, quando, motivo). Resposta sem veredito continua recusando: a porta e para o silencio. -> bool"""
         texto = (motivo or '').strip()
-        if not texto:
-            raise ValueError('Encerrar a disputa #%s exige o MOTIVO escrito (auditoria CLT).' % self.pk)
         return self.fechar(user, resolucao=texto, motivo=self.FECHOU_ADMIN_ENCERRA)
 
     def aparar_motivo_respondido(self, motivo):
```

**M3 — Abrir a permissão ao usuário autenticado.** Deve falhar `test_sem_permissao_recusa_get_e_post_sem_tocar_na_disputa` no status da lista do gestor [nao verificado]. `app/chamados/views_disputa_admin.py:13`, `app/chamados/views_disputa_admin.py:28`, `app/chamados/views_disputa_admin.py:36`

```diff
--- a/app/chamados/views_disputa_admin.py
+++ b/app/chamados/views_disputa_admin.py
@@ -12,7 +12,7 @@
 
 def _exigir_admin(user):
     if not (user.is_staff or user.groups.filter(name='admin_ti').exists()):
-        raise PermissionDenied
+        return
 
 
 def _renderizar_disputas(request, colaborador, *, mensagem=None, tipo_mensagem=None):
```

**M4 — Remover o link da ficha.** Deve falhar `test_ficha_mostra_link_so_para_staff_ou_admin_ti` na asserção do link renderizado [nao verificado]. `app/templates/chamados/fio_colaborador.html:11`

```diff
--- a/app/templates/chamados/fio_colaborador.html
+++ b/app/templates/chamados/fio_colaborador.html
@@ -8,9 +8,6 @@
 
 <div class="mb-4">
   {% include "core/_voltar.html" with voltar_url=voltar_url voltar_rotulo="Voltar" %}
-  {% if is_admin_ti %}
-  <a class="hx-btn" href="{% url 'chamados:disputas_admin_colaborador' colaborador.pk %}{% if request.GET.app == '1' %}?app=1{% endif %}">Disputas</a>
-  {% endif %}
 </div>
 <div class="painel-acompanhamento" style="border:1px solid #e0e6ef;border-radius:10px;padding:16px;margin-bottom:16px;background:#fafcff;">
   <div style="display:flex;justify-content:space-between;align-items:center;gap:12px;">
```

**M5 — Mostrar só a primeira pergunta.** Deve falhar `test_lista_todas_as_perguntas_antes_do_form` na comparação dos PKs visíveis antes do formulário [nao verificado]. `app/templates/chamados/disputa_admin.html:18`

```diff
--- a/app/templates/chamados/disputa_admin.html
+++ b/app/templates/chamados/disputa_admin.html
@@ -15,7 +15,7 @@
     <h2 id="disputa-{{ disputa.pk|unlocalize }}">Disputa #{{ disputa.pk|unlocalize }}</h2>
     <p>O encerramento vale para a disputa inteira, incluindo todas as perguntas abaixo.</p>
     <ul>
-      {% for pergunta in disputa.perguntas.all %}
+      {% for pergunta in disputa.perguntas.all|slice:":1" %}
       <li class="mb-2">
         <strong>Pergunta #{{ pergunta.pk|unlocalize }}</strong> · {{ pergunta.estado|capfirst }}<br>
         Marco: {% if pergunta.instancia_marco %}{{ pergunta.instancia_marco }}{% else %}Sem marco registrado{% endif %} ·
```

**M6 — Gravar estado diretamente na view.** `test_ast_view_so_delega_a_decisao_de_fechamento` falhou na checagem em memória. O mutante mantém a chamada à porta e repete uma gravação com o mesmo valor para exercitar o selo de escrita independentemente do valor final. `app/chamados/views_disputa_admin.py:52`, `app/chamados/views_disputa_admin.py:57`, `app/chamados/models.py:2199`

```diff
--- a/app/chamados/views_disputa_admin.py
+++ b/app/chamados/views_disputa_admin.py
@@ -55,6 +55,8 @@
         mensagem = str(e)
     else:
         if encerrou:
+            disputa.motivo_fechamento = DisputaSupervisao.FECHOU_ADMIN_ENCERRA
+            disputa.save(update_fields=['motivo_fechamento'])
             tipo_mensagem = 'success'
             mensagem = (
                 'A disputa foi encerrada e o motivo ficou registrado. '
```

## 3. Quem escreve e quem lê

O registro de autoridades aponta `DisputaSupervisao.pode_fechar` para C7 e `pergunta_respondida` para C21. Na tela, a view lê `fechada_em` para listar abertas e consome o retorno de `admin_encerra` no POST. A escrita persistente do fechamento está em `DisputaSupervisao.fechar`. `app/docs/JUIZES.md:62`, `app/docs/JUIZES.md:102`

[truncado em 20000 de 26212 caracteres]

_8946 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
