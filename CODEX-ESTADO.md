# CODEX-ESTADO

> Ultima resposta processada: **09/11/2026 06:43** · publicado em **09/11/2026 06:44** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

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

## 09/11 05:22 · sessao 11864eaa6042 · - · - · origem=Ronald (tmux)

- **Toque numa ponta:** o filtro v4 excluiria a pausa, pois usa interseção inclusiva (`app/escala/utils.py:456-457`). O pareador de batidas persistidas válidas não produz esse toque: declarações consomem identidades (`app/ponto/turnos.py:811-822`), o guloso consome dois vizinhos (`app/ponto/turnos.py:819`, `app/ponto/turnos.py:883-885`), e duas batidas do mesmo colaborador não podem ter o mesmo timestamp (`app/ponto/models.py:105-106`).

- **Pausa contida:** existe no pareador real: intervalo externo declarado `Xi`, pausa interna de duas batidas residuais vizinhas (`app/ponto/turnos.py:1316-1325`, `app/ponto/turnos.py:811-816`, `app/ponto/turnos.py:819-883`). As consultas separam intervalo `>=900s` e pausa `<900s` (`app/ponto/turnos.py:544-546`, `app/ponto/turnos.py:565-566`). A v4 elimina a pausa e transporta o externo; o teste real cobra papéis e alinhamento (`app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:182-194`).

- **Pausa cruzando estritamente:** não é saída desse produtor com timestamps únicos: declarados são pares consecutivos ordenados; o guloso só casa vizinhos e não pode consumir uma ponta já declarada (`app/ponto/turnos.py:811-822`, `app/ponto/turnos.py:883-885`). O teste cruzado fornece as consultas por mock e declara essa limitação (`app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:229-233`, `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:287-310`).

- **Dois intervalos sobrepostos existem:** externo declarado `12:00→13:00`, interno guloso adjacente `12:10→12:30`; a fixture usa o pareador real e verifica ambos (`app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:166-176`). A v4 veta o externo, mantém o interno adjacente e preserva o alinhamento caracterizado do HEAD (`app/escala/utils.py:462-476`, `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:175-180`).

- **Ambos descartados:** ocorre nos mocks de dois intervalos não adjacentes; os testes caracterizam que o HEAD também não tinha nenhum desses pares (`app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:266-285`). No produtor real, um intervalo aninhado residual é fisicamente adjacente e passa pela exceção de `_conflitos` (`app/ponto/turnos.py:819`, `app/ponto/turnos.py:883-885`, `app/escala/utils.py:464-465`).

- **Perda versus HEAD por `_conflitos`, com timestamps únicos:** não. Um par aceito pelo HEAD tinha fechamento em `i+1` (trecho HEAD do anexo `diff_v4.diff`, sem arquivo numerado: [não verificado]); portanto passa pela exceção adjacente e tem faixa interna vazia na v4 (`app/escala/utils.py:464-476`). Existência desses arranjos na frota: [não verificado].

- **Mesma batida como ponta de dois pares:** não no produtor: os declarados são retirados da passada gulosa, que avança dois elementos após emitir (`app/ponto/turnos.py:815-822`, `app/ponto/turnos.py:883-885`). No transcritor, `_fins` pula a ponta final quando a varredura chega nela (`app/escala/utils.py:468-478`). A fixture de timestamps repetidos reutiliza `k`, mas não contém pares (`app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:249-255`).

Executei também entradas sintéticas com timestamps exatamente repetidos, por extração AST e shim de timezone; elas ficam fora da unicidade do modelo (`app/ponto/models.py:106`). Na entrada `E08,S12,E13,S13,E13:02,S17`, o HEAD reconstruído conserva a pausa e a v4 descarta ambos; essa comparação é execução exploratória sem Django instalado, não prova de regressão em produção [não verificado].

## 09/11 05:21 · sessao fae75912abde · - · - · origem=Ronald (tmux)

6. Verificação em memória: os 17 testes das classes `ParNaoAdjacente` e `PapeisDoTurnoLimites` passaram com os corpos reais extraídos e adaptação mínima de timezone; execução Django dos 36 testes completos: [não verificado]. As duas classes estão em `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:50` e `:218`; os testes restantes dependem de `TestCase` e fixtures ORM em `:313`, `:317`, `:325`.

| Mutante | Resultado nos 17 | Teste que morde | [nao verificado]
|---|---|---|
| Filtro `_pausas`: ambos `<=` → `<` | Sobrevive | Nenhuma fixture de toque; comparador em `app/escala/utils.py:456` |
| `_conflitos`: ambos `<=` → `<` | Sobrevive | Nenhuma fixture de toque; comparador em `app/escala/utils.py:462` |
| Remover filtro `_pausas` | Morre | `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:182`, `:287` |
| Remover filtro de `_conflitos` | Morre | `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:266` |
| `_n += 1` → `_n = 1` | Morre | `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:142` |

A inspeção estática dos demais testes também não apresenta toque entre pares: as fixtures usuais têm no máximo quatro batidas no dia, deixando no máximo duas internas depois da exclusão das bordas; o anexo declara `21:01:59→22:00` e suas duas extras ficam soltas. Isso sustenta sobrevivência por análise, sem afirmar execução dos 36. `app/ponto/turnos.py:802`, `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:379`, `:401`, `:405`, `:416`, `:441`, `:455`, `:466`, `:477`, `:524`, `:535`, `:619`, `:635`, `:670`.

A sobrevivência dos comparadores estritos não basta para concluir falta de cobertura no domínio persistido: o produtor não reutiliza a mesma batida em dois pares, e a unicidade impede duas batidas do colaborador no mesmo timestamp. `app/ponto/turnos.py:811`, `:820`, `:883`; `app/ponto/models.py:105`.

7. **Sim: os +8 do delta preservam os textos das ocorrências.** Os nove destinos atuais correspondem às mesmas expressões, deslocadas de v3 para v4; contra a base do diff total, o deslocamento é **+9**, não +8. `app/core/tests/test_medida_turno_do_par.py:247`, `:248`, `:249`, `:281`, `:282`; `delta_v4.diff` e `diff_v4.diff` fornecidos [não verificado por git].

| v3 → v4 | Texto atual |
|---|---|
| 497 → 505 | `for _n, (_ks_s, _ks_e) in enumerate(zip(...))` |
| 521 → 529 | `_pausas = list(range(1, _M - 2, 2))` |
| 797 → 805 | `_idx = _idx_por_data.get(locs[_k][0].date())` |
| 1481 → 1489 | `_d_min_locs = min(_l[0].date() ...)` |
| 1501 → 1509 | `_ix_janela = next(...date() >= floor)` |
| 1511 → 1519 | `_mudos = (...date() - _pre_ini).days` |
| 1524 → 1532 | `_d_anc = _locs_i[_anc_ix][0].date() ...` |
| 1535 → 1543 | `if (not _cons_i[k]) and ...date() == d]` |
| 1821 → 1829 | `_idx = _idx_por_data.get(locs[_k][0].date())` |

Destinos: `app/escala/utils.py:505`, `:529`, `:805`, `:1489`, `:1509`, `:1519`, `:1532`, `:1543`, `:1829`. O teste real da igualdade de `LINHAS_REGISTRADAS` passou em unittest puro, substituindo somente a importação de `SimpleTestCase`. `app/core/tests/test_medida_turno_do_par.py:534`.

## 09/11 05:19 · sessao 81ffe87c376b · - · - · origem=Ronald (tmux)

**4. Escritores e leitores dos valores**

| Valor | Quem escreve | Quem lê |
|---|---|---|
| `Batida.timestamp`, `tipo` | `app/ponto/registro_batida.py:152`, `:157` | pareador; construção de `locs` em `app/escala/utils.py:1332`; transcrição ordena timestamps em `:451` |
| `papel_por_minuto` (`E/S/X/Xi`) | `app/ponto/turnos.py:1804`–`:1806`; declaração `Xi` no final desse bloco | `_papel_da_ata`, `app/ponto/turnos.py:904`–`:909` |
| marcas `_papel_ata`, `_intra_dur`, `_intra_ata`, `_intra_declarado` | `app/ponto/turnos.py:1315`–`:1325` | `_pares_marcados`: declarados primeiro em `:811`–`:816`; restantes em `:819`–`:884` |
| `intervalos_do_turno` / `pausas_abaixo_do_piso` | `_pares_marcados`, dividido pelo piso: `app/ponto/turnos.py:545`–`:546`, `:565`–`:566` | `_papeis_do_turno`, `app/escala/utils.py:455`–`:458`; dinheiro lê intervalos em `app/ponto/turnos.py:756`–`:759` |
| `turno` | builder consulta `turnos_do_colab` em `app/escala/utils.py:1590`; indexa pelo `data_turno` em `:1591`–`:1592`; seleciona em `:1602`–`:1605` | `_match_marcos(turno=...)`, `app/escala/utils.py:1710`–`:1712`, depois `_papeis_do_turno` em `:605` |
| `locs` | `app/escala/utils.py:1332`, deduplicação `:1335`–`:1348` | índice timestamp→primeiro `k` em `:448`–`:452`; alinhamento em `:531`–`:540` |
| `consumed` | inicializado `False` em `app/escala/utils.py:1349`; escrito pelo matcher em `:631`–`:632`; outros caminhos `:673`, `:1584`, `:1844` | guarda de transcrição em `:453`; seleção e conservação em `:638`, `:1827` |
| `_intervalos`, `_pausas`, `_pares`, `_pontas_par`, `_pos`, `_conflitos`, `_ini_par` | exclusivamente `app/escala/utils.py:455`–`:465` | reconhecimento de ponta/impedimento no laço `:469`–`:478` |
| `_n`, `_fins`, `out`, `(k,papel)` | `app/escala/utils.py:466`–`:483` | leitores descritos abaixo |
| `_ass` (marco→`k`) | `_alinhar_por_papel`, `app/escala/utils.py:567`–`:591` | células e órfãs em `:625`–`:630` |
| células `tipo/papel/hora/delta/status/tipo_real` | `_celula_do_marco`, `app/escala/utils.py:325`–`:327`; chamada `:628` | projeção por marco em `:1714`–`:1717`; montagem do dia em `:1816`–`:1820`; lâmpadas `app/ponto/services/cartorio.py:677`–`:679` |
| ata e minuto | ata de trabalho em memória `app/escala/utils.py:1896`–`:1907`; minuto pedido ao juiz em `:2054`–`:2067`; `ata_do_dia` escreve campos em `app/ponto/services/cartorio.py:701`–`:711` | cartório encaminha à lavra em `:1800`–`:1802`; porta atribui/persiste em `app/ponto/portas/celula.py:1027`–`:1035` |

Há **um chamador produtivo direto**, `app/escala/utils.py:605`, e três chamadas de teste, `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:76`, `:233`, `:407`. Os **três leitores lógicos do retorno** são: guarda L‑084 (`app/escala/utils.py:614`–`:620`), alinhador (`:622`, decodificação de `k/papel/n` em `:533`–`:540`) e conservação de órfãs/consumo (`:629`–`:632`). 

Ata, Zona 5 e `cumpriu_a_volta` não são três chamadas diretas dessa função: Zona 5 chama `_match_marcos` **sem turno** em `app/ponto/motor_calculo_v2.py:3062`; `marco_ocupado_pela_batida` faz o mesmo em `app/escala/utils.py:427`; nesse formato o matcher tenta `_papeis_por_alternancia`, `app/escala/utils.py:605`–`:607`.

**5. Classes de defeitos**

- **Seleção local adicional:** `_papeis_do_turno` não transcreve incondicionalmente: elimina pausas sobrepostas e veta intervalos em conflito não adjacentes (`app/escala/utils.py:455`–`:465`, `:475`–`:476`). O registro atribui “o vão foi intervalo?” a `intervalos_do_turno` e “qual marco ocupa?” a `_match_marcos` (`app/core/juizes.py:200`–`:202`). A regra nova cabe no alinhamento apenas se entendida como preservação do HEAD; como decisão sobre existência do intervalo, seria segundo julgamento. A autorização para decidir conflito acima do piso permanece aberta no anexo `[não verificado no arquivo da cópia]`.
- **Derivação de dinheiro:** o trecho alterado não soma minutos; o realizado continua vindo do juiz (`app/escala/utils.py:2054`–`:2067`), que desconta `intervalos_do_turno` (`app/ponto/turnos.py:756`–`:770`). Consequência indireta pela próxima ata existe no fluxo `app/escala/utils.py:1896`–`:1907`; efeito em produção `[não verificado]`.
- **Relógio:** o trecho modificado usa conversão de timestamps, sem novo `now()`/`localdate()` (`app/escala/utils.py:437`–`:483`). O builder recebe `agora` e deriva `ref` dele (`:1286`, `:1299`).
- **Fronteiras:** veto de toque inclusivo em `app/escala/utils.py:457`, `:463`; interior estrito em `:475`–`:476`. São critérios diferentes explícitos, mas a justificativa de lei específica para toque permanece `[não verificado]`.
- **Dia civil/dia do turno:** a transcrição usa instantes absolutos e não introduz `.date()` (`app/escala/utils.py:437`–`:483`); seleção usa `data_turno` (`:1591`–`:1592`), marcos encadeiam virada (`:345`–`:348`). A conservação já arquiva sobras não consumidas por dia civil (`:1829`); esse texto não foi alterado, apenas sua linha registrada.

## 09/11 05:15 · sessao c7cd8e1c6f6d · PAR-NAO-ADJACENTE · construtor · origem=Code

Corrigido: pausas abaixo do piso que disputam pontas com intervalos são retiradas do pareamento; suas batidas ficam `sol`. O conflito entre dois intervalos conserva o casamento adjacente atribuído ao b57ce0a1. [escala/utils.py:455](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:455)

Hunks entregues:

- Docstring e transcrição da prioridade, sem a afirmação falsa sobre aninhados. [utils.py:438](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:438)
- Teste aninhado reescrito: exige o par 20:00/22:00 e verifica `_alinhar_por_papel`, incluindo [nome]. [teste:182](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:182) [nao verificado]
- Cruzamento testado nos dois sentidos, com consultas mockadas explicitamente. [teste:287](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:287) [nao verificado]
- Antigo teste:230 **reescrito**, caracterizando ambos os intervalos não adjacentes como soltos. [teste:266](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:266) [nao verificado]
- `LINHAS_REGISTRADAS`: somente deslocamento de **+8 linhas**, com motivo registrado. [core/tests/test_medida_turno_do_par.py:247](/home/ronald/wt-codex-par-nao-adjacente-031500/app/core/tests/test_medida_turno_do_par.py:247)

Para cada teste novo ou alterado, estes são os resultados **esperados** no b57ce0a1; execução nessa revisão: **[nao verificado]**.

| Teste/hunk | Esperado | Por quê | [nao verificado]
|---|---|---|
| [Duas soltas:89](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:89), comentário corrigido | Falha | Exige par não adjacente. |
| [Três pares:142](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:142) | Falha | Exige três pares não adjacentes, numerados 0/1/2. |
| [Dois intervalos, interno adjacente:166](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:166), docstring corrigida | Passa | Mantém o interno adjacente. |
| [Aninhado abaixo do piso:182](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:182) | Falha | Exige o externo, anteriormente perdido. |
| [Pausa curta separada:196](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:196) | Passa | Ambos os pares são adjacentes e separados. |
| [Sem par:235](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:235) | Passa | Caracterização; removido `RED`. |
| [Par adjacente:243](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:243) | Passa | Caracterização; removido `RED`. |
| [Timestamp repetido:249](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:249) | Passa | Caracterização; removido `RED`. |
| [Dois intervalos aninhados:266](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:266) | Passa | Nenhum par é adjacente. |
| [Dois intervalos cruzados:277](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:277) | Passa | Nenhum par é adjacente. |
| [Pausa curta cruzada:287](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:287) | Falha | Exige preservar e alinhar o intervalo atravessado. |

Quem escreve e quem lê:

| Papel | Escritor | Leitor |
|---|---|---|
| `('par', n, S/E)` | `_papeis_do_turno` [477](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:477); `_papeis_por_alternancia` [506](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:506) | `_alinhar_por_papel`: agrupa por número/lado [538](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:538), senta o par [570](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:570). |
| `sol` | `_papeis_do_turno` [481](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:481) | `_alinhar_por_papel` [540](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:540), encaixe posterior [573](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:573). |
| `ent` / `sai` | `_papeis_do_turno` [466](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:466), [482](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:482); alternância [504](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:504), [507](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:507) | `_alinhar_por_papel` [533](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:533), [534](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:534). |

`_match_marcos` encaminha os papéis ao alinhador e percorre todos para órfãs/consumo. [utils.py:622](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:622), [630](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/utils.py:630)

Ensaio reduzido: **17/17**; os oito mutantes terminaram com **rc=1 por asserção**. Comandos e limitações estão no [ensaio:3](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/ensaio_par_nao_adjacente.py:3):

```bash
export PYTHONDONTWRITEBYTECODE=1
ensaio_par=escala/tests/ensaio_par_nao_adjacente.py
python3 -B "$ensaio_par"
MUTANTE=sem_prioridade python3 -B "$ensaio_par"
MUTANTE=so_aninhado python3 -B "$ensaio_par"
MUTANTE=so_inicio python3 -B "$ensaio_par"
MUTANTE=so_fim python3 -B "$ensaio_par"
MUTANTE=apaga_pausas python3 -B "$ensaio_par"
MUTANTE=lei_nova_dois_intervalos python3 -B "$ensaio_par"
MUTANTE=numero_zero python3 -B "$ensaio_par"
MUTANTE=numero_um python3 -B "$ensaio_par"
python3 -B "$ensaio_par" --py-compile
```

`py_compile` passou nos quatro arquivos, interceptando escrita de bytecode. [ensaio:37](/home/ronald/wt-codex-par-nao-adjacente-031500/app/escala/tests/ensaio_par_nao_adjacente.py:37) Suite Django e builder: **[nao verificado]**; esta entrega não declara certificação. [nao verificado]

## 09/11 05:10 · sessao 781c071dafd8 · PAR-NAO-ADJACENTE · censo · origem=Code

Conto as linhas dos anexos a partir da primeira linha após `== anexo: ... ==`, incluindo linhas em branco. No `relato_par.txt`, uso os números já impressos.

1. **(a) — O tema estava presente; o pre-verificador descreveu o mecanismo, mas não apontou a contradição ata × intervalo descontado.** (`preverif_v1.q:11-15`; `preverif_v1.r:11`; `preverif_v2.r:31`)

   **Pergunta:** “QUEM ESCREVE e QUEM LE: papel 'par'/'sol' produzido aqui; `minutos_realizados` da ata; intervalos transportados ao motor. Todos supoem o mesmo significado”. (`preverif_v1.q:11-12`)

   **Resposta:** “O externo continua recusado; o interno continua aceito. Os testes fixam exatamente isso.” Depois, tratou como mutação reprovável: “Retirar `pausas_abaixo_do_piso` da composição de `_pares`”. (`preverif_v1.r:11`; `preverif_v2.r:31`) [nao verificado]

   A distinção também foi percebida: “o papel `par` inclui pausas abaixo do piso; o adaptador transporta somente intervalos de pelo menos 900 segundos”. Faltou relacioná-la ao veto do intervalo externo naquele teste. (`preverif_v1.r:48`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:162-169`)

   O descarte de “builder não relido da persistência” não documenta descarte dessa contradição específica. (`relato_par.txt:12`; `relato_par.txt:15`)

2. **(c) — Fora do alcance quanto à medição dos sete turnos de produção e ao impacto em dinheiro/ata.** O registro fornecido dizia expressamente “quantos sao aninhados: nao medido”. (`relato_par.txt:17`)

   **Pergunta:** o brief forneceu “Frota desde 21/09: 7 turnos de 12.498”; a pergunta ao pre-verificador foi estrutural: “para todo arranjo de batidas e pares do juiz [...] Liste os arranjos que mudam e os que nao mudam”. (`construtor_v1.q:8`; `preverif_v1.q:5-8`)

   **Resposta:** “O arranjo pode mudar: o externo permanece vetado, mas o interno passa se atravessar somente soltas.” Sobre dinheiro, delimitou: “Isso não demonstra igualdade com o resultado financeiro do motor.” Nenhum desses trechos constitui classificação ou medição financeira dos sete turnos. (`preverif_v1.r:12`; `preverif_v1.r:53`; `relato_par.txt:17`)

3. **(e) — NÃO PERGUNTADO: alcance no deploy e declaração da rota do apply.** Os três questionários do pre-verificador não pedem examinar `REGRA_VERSOES`, `--forcar` ou escolher/declarar essa rota. (`preverif_v1.q:5-18`; `preverif_v2.q:3-9`; `preverif_v3.q:3-7`) [nao verificado]

   **Pergunta mais próxima:** “Ha outros sitios com a MESMA exigencia de adjacencia que transcrevem o par do juiz para a ata/motor”. Isso pergunta sobre caminhos do defeito, sem perguntar sobre alcance de implantação. (`preverif_v1.q:17-18`)

   **Resposta específica sobre deploy/apply:** não há trecho correspondente a transcrever nos anexos de resposta. A falta de declaração aparece no laudo: “Rota do apply nao declarada”; depois, o RELATO manda “declarar rota”. (`opus_final_1.txt:4`; `relato_par.txt:3`) [nao verificado]

4. **O achado reúne duas classes: (a) para o rótulo RED falso; (c) para a execução contra o HEAD no Django.** (`preverif_v1.q:14-15`; `preverif_v2.r:19`; `preverif_v2.r:23`; `preverif_v2.r:38`)

   **Pergunta sobre RED:** “selo/RED sem mordida (cada teste novo falha no HEAD? qual mutante simples sobrevive?)”; na v2: “Os testes novos mordem? Para cada um, qual mutacao do codigo o reprova.” (`preverif_v1.q:14-15`; `preverif_v2.q:6`) [nao verificado]

   **Resposta sobre os três testes:** “Os três testes novos fixam as listas antigas para ausência de par, par adjacente e timestamps repetidos”. Reconheceu a preservação, mas não apontou o prefixo RED indevido, presente nos três nomes da cópia. (`preverif_v2.r:19`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:199`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:207`; `app/escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:213`)

   **Resposta sobre execução:** “Isso é um ensaio reduzido; execução integral Django e integração com banco: **[nao verificado]**”; para o builder: “execução desse teste: **[nao verificado]**”. Essa limitação foi declarada, não passou despercebida. (`preverif_v2.r:23`; `preverif_v2.r:38`) [nao verificado]

5. **(b) quanto à objeção aos testes como caracterização; com ressalva: a descoberta da mudança do interno foi acolhida como PROCEDENTE, não descartada.** (`relato_par.txt:15`) [nao verificado]

   **Pergunta:** “o diff muda algum resultado ALEM do caso [...]? Liste os arranjos [...] (adjacente, aninhado, cruzado [...] )”. (`preverif_v1.q:5-8`)

   **Resposta:** “Portanto ‘aninhado não muda’ não vale sem qualificar o interior.” (`preverif_v1.r:12`)

   **Triagem e motivo:** “PROCEDENTES [...] aninhado com interno nao adjacente muda (interno vira par) -> teste + docstring”; separadamente, descartou “aninhados sao caracterizacao (nao RED)”. (`relato_par.txt:15`) [nao verificado]

   O encaminhamento tornou essa distinção explícita: “Isso e a cura aplicada ao par interno, nao o caso aninhado; mantenha o comportamento, mas escreva o teste que o fixa”. A resposta seguinte considerou “Contrato corrigido e caso acrescentado”. Assim, o comportamento questionado pelo opus foi visto e deliberadamente fixado após a triagem. (`construtor_v2.q:14-17`; `preverif_v2.r:11`) [nao verificado]

## 09/11 04:08 · sessao e275d0649b92 · SELOS-CANON-I1 · verificador · origem=Code

1. **OBSERVACAO — a volta 3 tem curas reais, mas não encerra todos os PROCEDENTES da triagem v2.**

| PROCEDENTE | Resultado |
|---|---|
| **P1/achado 4: PAREI no lugar errado** | **Curado localmente:** publica na primeira linha útil e relê pelo Stop. **Integração incompleta:** desloca o painel anterior e pode esconder a linha B3. `bin/disjuntor.py:299`; `bin/disjuntor.py:301`; `app/core/esteira_vigia.py:888`; `bin/hook_stop_fila1.py:442`. |
| **P2: existência/idempotência/trava** | **Curado dentro da porta:** leitura, comparação com painel vigente e gravação ficam sob a mesma trava; histórico inferior não impede republicação. Escritores externos continuam podendo perder essa atualização. `app/core/esteira_vigia.py:878`; `app/core/esteira_vigia.py:883`; `app/core/esteira_vigia.py:891`; `bin/gerar_cortes.py:136`. |
| **P3: briefs diferentes entre bocas** | **Parcial:** há coletor compartilhado, mas a CLI lê somente o conteúdo da pergunta; dos anexos recebe apenas caminhos. Referências internas aos anexos ficam ausentes do julgamento. `bin/disjuntor.py:1573`; `bin/disjuntor.py:2152`; `bin/disjuntor.py:2154`; `bin/codex_pergunta.sh:315`. |
| **P4: raiz errada** | **Curado para BACKLOG/briefs do julgamento:** wrapper passa a raiz viva. **Costura da medida incompleta:** aprovação e `ini` podem nascer em raízes diferentes. `bin/codex_pergunta.sh:234`; `bin/disjuntor.py:2157`; `bin/codex_pergunta.sh:88`; `bin/codex_pergunta.sh:293`. |
| **P5: chave ignorada pela CLI** | **Curado:** consulta `desligado(raiz)`, registra DESLIGADO e retorna zero. `bin/disjuntor.py:2147`; `bin/disjuntor.py:2148`; `bin/disjuntor.py:2150`. |
| **P6/achado 3: dois julgamentos, IDs e fail-closed** | **Parcial:** divisão por via implementada; IDs anteriores do wrapper aceitos; BACKLOG sem marcador e JSON truncado literal negados. A guarda ainda depende da representação textual do JSON e não cobre `SyntaxError` anterior à instalação. `bin/disjuntor.py:1731`; `bin/disjuntor.py:1752`; `bin/disjuntor.py:2144`; `bin/disjuntor.py:1586`; `bin/disjuntor.py:1657`; `bin/disjuntor.py:82`. |
| **Achado 6: quatro mutantes anteriores** | **Casos discriminantes acrescentados:** ruim antes do bom, último do glob, painel neutro e rc 2. Há novos sobreviventes no item 8. Verde integral: **[nao verificado]**. `bin/tests/test_censo_antes_da_vaga.sh:138`; `bin/tests/test_censo_antes_da_vaga.sh:147`; `bin/tests/test_censo_antes_da_vaga.sh:303`; `bin/tests/test_codex_pergunta.sh:230`. |
| **Achado 7: aprovação confundida com abertura** | **Parcial:** separa APROVADO e correlaciona obra+PID+hash12 em 600 s. Entretanto, `ini` ainda precede recusas que impedem chamar o Codex; “briefs aprovados” também conta eventos, não arquivos. `bin/disjuntor.py:2026`; `bin/disjuntor.py:2028`; `bin/codex_pergunta.sh:292`; `bin/codex_pergunta.sh:399`; `bin/disjuntor.py:2007`. |

2. **PROCEDENTE-candidato — a cura do topo mascara contratos anteriores de posição.**

`no_topo` acrescenta uma linha acima do painel vigente, preservando todas as anteriores. Contraprova em memória com as funções reais: `[ESPERANDO, ERRO-BOBO B3]` torna-se `[PAREI censo, ESPERANDO, ERRO-BOBO B3]`. O Stop passa a julgar o censo; a liberação B3 cai para a terceira linha útil e desaparece do seu recorte. `app/core/esteira_vigia.py:888`; `bin/hook_stop_fila1.py:411`; `bin/hook_stop_fila1.py:442`; `bin/disjuntor.py:759`; `bin/disjuntor.py:1003`.

A segunda linha **não é alternativa de ESPERANDO/PAREI para o Stop**: ela existe para B3. Assim, uma espera anteriormente comprovável pode deixar de liberar o Stop quando o novo censo não tiver wrapper vivo. `bin/hook_stop_fila1.py:425`; `bin/hook_stop_fila1.py:746`; `bin/hook_stop_fila1.py:749`; `bin/hook_stop_fila1.py:751`.

| Leitor | Posição e pressuposto |
|---|---|
| `_linha_do_topo` | Primeira útil representa o painel vigente. `bin/hook_stop_fila1.py:411`. |
| `_esperando_com_prova` | Primeira útil deve declarar ESPERANDO ou PAREI censo comprovado; não busca a segunda. `bin/hook_stop_fila1.py:746`; `bin/hook_stop_fila1.py:751`. |
| `_parei_do_em_curso` → `_relato_parou` | Primeira útil deve declarar PAREI com `!`, respeitar exclusões e referir o item pertinente. `bin/hook_stop_fila1.py:1198`; `bin/hook_stop_fila1.py:1213`; `bin/hook_stop_fila1.py:1218`; `bin/hook_stop_fila1.py:1249`. |
| Orçamento/CLEAR e recuperação | Consultam a primeira útil para reconhecer CLEAR. `bin/hook_stop_fila1.py:1002`; `bin/hook_stop_fila1.py:1119`. |
| B4/COTA | Recebe a primeira útil para decidir COTA. `bin/hook_stop_fila1.py:1138`. |
| B3 | Fotografa as duas primeiras úteis e procura nelas DESENHO/ERRO-BOBO. `bin/disjuntor.py:759`; `bin/disjuntor.py:1003`. |
| Confirmação da publicação censo | Compara somente a primeira útil com o PAREI recém-escrito. `bin/disjuntor.py:301`; `bin/disjuntor.py:302`. |
| Handoff | Prova censo pela primeira útil; seu inventário textual percorre até o primeiro `##` e exibe quatro perguntas. `bin/handoff_sessao.sh:102`; `bin/handoff_sessao.sh:110`; `bin/handoff_sessao.sh:128`. |
| Alarme | Procura PAREI nas primeiras 40 linhas físicas; exige prova somente para censo. Pode reconhecer uma parada deslocada que o Stop ignora. `bin/alarme_sessao_ociosa.py:124`; `bin/alarme_sessao_ociosa.py:137`; `bin/alarme_sessao_ociosa.py:140`. |

O selo exige que o painel neutro desça à segunda posição, mas não combina ESPERANDO/PAREI anterior com linha B3. Portanto, prova a inserção e aceita o deslocamento defeituoso. `bin/tests/test_censo_antes_da_vaga.sh:299`; `bin/tests/test_censo_antes_da_vaga.sh:307`; `bin/tests/test_censo_antes_da_vaga.sh:309`.

3. **PROCEDENTE-candidato — permanecem escritores do RELATO fora da trava compartilhada.**

O censo escreve por `no_topo`; alarmes do disjuntor e do vigia usam `no_relato`. Essas portas compartilham trava e gravação por temporário/replace. Stop e B3 consomem o painel; o Stop preserva explicitamente a retirada da antiga escrita automática de PAREI. `bin/disjuntor.py:299`; `bin/disjuntor.py:269`; `app/core/esteira_vigia.py:1603`; `app/core/esteira_vigia.py:843`; `app/core/esteira_vigia.py:891`; `app/core/esteira_vigia.py:926`; `bin/hook_stop_fila1.py:1379`.

Porém, `gerar_cortes` e `deploy_agendado` leem e regravam o RELATO inteiro diretamente. O intercalamento “leitura antiga → publicação censo → regravação antiga” perde o novo PAREI, mesmo quando o escritor externo modifica outra seção. É residual declarado, não curado pela trava nova. Ocorrência produtiva: **[nao verificado]**. `bin/gerar_cortes.py:117`; `bin/gerar_cortes.py:136`; `bin/deploy_agendado.sh:37`; `bin/deploy_agendado.sh:44`; `app/core/esteira_vigia.py:839`.

4. **PROCEDENTE-candidato — o coletor compartilhado ainda recebe universos diferentes e aplica confinamentos diferentes.**

Na CLI, `partes = list(anexos)` acrescenta os **nomes** dos anexos; somente `texto_de` é aberto. No despacho, o wrapper concatena o **conteúdo** dos anexos. Um envelope contendo referência a outro brief ruim pode chegar ao construtor sem essa referência passar pelo censo. O selo cobre anexo cujo próprio caminho é brief, não essa referência interna. `bin/disjuntor.py:2152`; `bin/disjuntor.py:2154`; `bin/disjuntor.py:2156`; `bin/codex_pergunta.sh:315`; `bin/codex_pergunta.sh:344`; `bin/tests/test_censo_antes_da_vaga.sh:138`.

Além disso, referências citadas são resolvidas e conferidas contra `RAIZ/logs`; o glob da obra é resolvido por `realpath` e unido sem a mesma conferência. Um link no brief ou na pasta da obra pode entregar arquivo externo ao juiz, embora a mesma referência externa citada diretamente seja recusada. Presença desses links em produção: **[nao verificado]**. `bin/disjuntor.py:1549`; `bin/disjuntor.py:1554`; `bin/disjuntor.py:1566`; `bin/disjuntor.py:1574`.

5. **OBSERVACAO — um julgamento por via existe no caminho normal; a rota externa continua aberta tecnicamente e proibida por conduta.**

Agent construtor reconhecido passa pelo PreToolUse; Codex construtor pelo wrapper. O hook deixou de julgar o censo da chamada Bash ao wrapper. Não aparece, nesses caminhos normais, a mesma abertura julgada pelos dois mecanismos; a recuperação excepcional pode chamar novamente `_censo_nega` para Agent. `bin/disjuntor.py:1748`; `bin/disjuntor.py:1752`; `bin/disjuntor.py:1731`; `bin/codex_pergunta.sh:234`; `bin/disjuntor.py:1662`.

Uma chamada direta a `codex exec`, sem wrapper, não alcança essas portas de censo: na sessão principal, um Bash sem `codex_pergunta` sai antecipadamente. Essa rota permanece proibida pela conduta, conforme o descarte P7; ocorrência produtiva: **[nao verificado]**. `bin/disjuntor.py:1716`; `CLAUDE.md:348`.

A sessão principal **não tem isenção** ao despachar Agent ou wrapper. `CODEX_PAPEL=leitura`, sem `--construtor`, fica fora do censo novo; leitura com essa flag recebe a recusa de incompatibilidade papel/flag. Uma negativa espúria nova para a chamada concreta desta sessão: **[nao verificado]**. `bin/disjuntor.py:1716`; `bin/codex_pergunta.sh:184`; `bin/codex_pergunta.sh:185`.

6. **PROCEDENTE-candidato — fail-closed ainda depende da grafia do JSON; identidade do Agent não cabe integralmente nas portas seguintes.**

A guarda de import e o fallback cru procuram literalmente `"Agent"` e `/wt-`. JSON equivalente com `"\u0041gent"` e `\/wt-` não satisfaz essas substrings. Em memória, a guarda emitiu `deny` para a representação literal e nenhum `deny` para a equivalente escapada. Que o consumidor real abra após traceback/rc sem negativa é **[nao verificado]**: a configuração apresentada apenas executa Python. `bin/disjuntor.py:72`; `bin/disjuntor.py:1657`; `.claude/settings.json:68`.

`SyntaxError` no próprio módulo precede a instalação da guarda. Já no wrapper, todo rc não zero do Python impede chegar à vaga. `bin/disjuntor.py:62`; `bin/disjuntor.py:82`; `bin/codex_pergunta.sh:234`; `bin/codex_pergunta.sh:240`.

IDs como `ABC` foram compatibilizados na CLI, mas continuam ilegíveis para o extrator do Agent; isso já fazia parte da definição anterior dessa via, portanto regressão nova aí é **[nao verificado]**. Há também incompatibilidade interna demonstrável: `_obra_de` admite `O9 (C)`, enquanto glob e publicação PAREI rejeitam espaço/parênteses. Uma negativa dessa identidade pode não publicar PAREI. Obra atual assim na fila: **[nao verificado]**. `bin/disjuntor.py:132`; `bin/disjuntor.py:594`; `bin/disjuntor.py:602`; `bin/disjuntor.py:1563`; `bin/disjuntor.py:285`; `bin/disjuntor.py:2144`.

7. **PROCEDENTE-candidato — a medida ainda mistura tentativa e abertura, pode separar suas fontes e não mede ocupação da vaga.**

O wrapper grava `ini` antes da raspagem, da trava da resposta e da revalidação. Essas etapas podem terminar sem chamar Codex; mesmo assim, `_censo_24h` conta o `ini` casado como abertura. A chamada efetiva só ocorre depois. Portanto, o antigo defeito semântico não desapareceu integralmente. `bin/codex_pergunta.sh:292`; `bin/codex_pergunta.sh:309`; `bin/codex_pergunta.sh:318`; `bin/codex_pergunta.sh:376`; `bin/codex_pergunta.sh:399`; `bin/codex_pergunta.sh:401`; `bin/disjuntor.py:2035`.

O censo grava na raiz viva; `ini` usa `LOGD`, por padrão sob a raiz do script, e aceita outros diretórios sob `R/logs`. O leitor procura exclusivamente `<raiz>/logs/codex/chamadas.log`. Wrapper de cópia ou `CODEX_LOGD` alternativo pode separar aprovação e `ini`, fazendo a abertura desaparecer ou parecer sem aprovação. Ocorrência produtiva: **[nao verificado]**. `bin/codex_pergunta.sh:88`; `bin/codex_pergunta.sh:193`; `bin/codex_pergunta.sh:234`; `bin/codex_pergunta.sh:293`; `bin/codex_limite_estado.py:22`; `bin/codex_limite_estado.py:52`.

O novo rótulo das horas descreve corretamente NEGOU→APROVADO, mas o cálculo não consulta ocupação da vaga: a medida literal de “vaga ociosa” continua sem prova. Também falta limitar eventos a `t <= agora`; um APROVADO Agent uma hora no futuro contou em memória como aprovado e abertura. `bin/disjuntor.py:2003`; `bin/disjuntor.py:2007`; `bin/disjuntor.py:2009`; `bin/disjuntor.py:2038`; `bin/disjuntor.py:2041`.

8. **PROCEDENTE-candidato — há novos mutantes simples não distinguidos pelos casos.**

| Alvo/mutante | Lacuna e contraprova |
|---|---|
| **`_briefs_do_censo`: `return todos, fora` → `return todos, []`** | O caso externo existente continua negado por ausência de qualquer brief. Falta **bom local + referência externa**: original nega; mutante libera. `bin/disjuntor.py:1575`; `bin/disjuntor.py:1614`; `bin/tests/test_censo_antes_da_vaga.sh:214`. |
| **`_censo_24h`: retirar `k not in usados and`** | Preserva todos os números da fixture: a reutilização nela ocorre fora dos 600 s. Com uma aprovação e dois `ini` da mesma identidade em 10 s, original dá com=1/sem=1; mutante com=2/sem=0. `bin/disjuntor.py:2027`; `bin/disjuntor.py:2033`; `bin/tests/test_censo_antes_da_vaga.sh:394`; `bin/tests/test_censo_antes_da_vaga.sh:397`. |
| **`no_topo`: retornar `False` quando `i is None`** | Casos usam painel neutro existente. Com RELATO contendo apenas cabeçalho, original publica PAREI; mutante não publica. Contraprova executada em memória. `app/core/esteira_vigia.py:885`; `bin/tests/test_censo_antes_da_vaga.sh:299`; `bin/tests/test_censo_antes_da_vaga.sh:322`. |
| **`_guarda_import`: retirar a condição da chave desligada** | Os casos de import não combinam import quebrado com chave desligada. O mutante passaria esses cenários, mas negaria quando deveria respeitar a chave. `bin/disjuntor.py:72`; `bin/tests/test_censo_antes_da_vaga.sh:345`; `bin/tests/test_censo_antes_da_vaga.sh:352`. |

As contraprovas acima foram verificadas em memória; sobrevivência à **execução integral** dos selos: **[nao verificado]**. Os mutantes anteriores de último citado/glob, neutro e rc 1 exclusivo agora têm casos específicos. `bin/tests/test_censo_antes_da_vaga.sh:457`; `bin/tests/test_censo_antes_da_vaga.sh:458`; `bin/tests/test_censo_antes_da_vaga.sh:459`; `bin/tests/test_codex_pergunta.sh:545`.

O positivo do PreToolUse ainda verifica somente stdout vazio, sem exigir rc zero. Assim, o selo continua podendo chamar de passagem limpa uma falha silenciosa. `bin/tests/test_censo_antes_da_vaga.sh:186`; `bin/tests/test_censo_antes_da_vaga.sh:201`.

9. **OBSERVACAO — escritores, leitores e unidades das trilhas.**

| Linha/fato | Quem escreve | Quem lê e significado |
|---|---|---|
| **CENSO APROVADO** | `_censo_nega`; Agent publica depois de `_pretool` permitir. `bin/disjuntor.py:1623`; `bin/disjuntor.py:1707`. | `_censo_24h` conta aprovações, fecha espera e correlaciona wrapper com `ini`; Agent é tratado como abertura. `bin/disjuntor.py:2004`; `bin/disjuntor.py:2007`; `bin/disjuntor.py:2009`; `bin/disjuntor.py:2035`. |
| **CENSO FORA** | `main_censo`. `bin/disjuntor.py:2162`. | Casamento isenta o `ini` de com/sem; não encerra espera anterior. `bin/disjuntor.py:2006`; `bin/disjuntor.py:2034`. |
| **NEGA: real `NEGOU DISJUNTOR CENSO`** | PreToolUse normal, recuperação e CLI. `bin/disjuntor.py:1801`; `bin/disjuntor.py:1812`; `bin/disjuntor.py:2167`. | Conta negativas; abre intervalo por obra somente quando reconhece `construtor de … sem CENSO`. `bin/disjuntor.py:1995`; `bin/disjuntor.py:1998`. |
| **CENSO DESLIGADO** | Agent e CLI. `bin/disjuntor.py:1779`; `bin/disjuntor.py:2148`. | Agent soma diretamente em sem; wrapper precisa casar com `ini`. `bin/disjuntor.py:2005`; `bin/disjuntor.py:2036`. |
| **PAREI censo** | `_parei_censo_no_relato` → `no_topo`. `bin/disjuntor.py:299`. | Stop, handoff e alarme compartilham reconhecedor, porém selecionam recortes diferentes. `bin/hook_stop_fila1.py:746`; `bin/handoff_sessao.sh:128`; `bin/alarme_sessao_ociosa.py:124`. |
| **`chamadas.log` / `ini`** | Wrapper, antes da chamada efetiva. `bin/codex_pergunta.sh:292`. | Parser cria chave **(obra,hash,PID)**; censo reordena corretamente para **(obra,PID,hash)**. Também alimenta contagem por papel, últimas chamadas e procedência. `bin/codex_limite_estado.py:63`; `bin/disjuntor.py:2026`; `bin/codex_limite_estado.py:71`; `bin/codex_limite_estado.py:82`; `bin/codex_limite_estado.py:92`. |

A correlação atual usa hash de 12 caracteres, PID, obra, consumo único e janela anterior de 600 segundos; as horas são união dos intervalos dividida por 3600. Não encontrei troca de PID por hash nessa indexação. `bin/disjuntor.py:2002`; `bin/disjuntor.py:2026`; `bin/disjuntor.py:2028`; `bin/disjuntor.py:2033`; `bin/disjuntor.py:2038`.

Persistem duas diferenças de significado: **“briefs aprovados” conta linhas APROVADO**, embora uma linha possa listar vários arquivos; **`ini` é tentativa iniciada no wrapper**, mas os leitores o apresentam como chamada/abertura acontecida. `bin/disjuntor.py:1621`; `bin/disjuntor.py:2007`; `bin/disjuntor.py:2054`; `bin/codex_limite_estado.py:72`; `bin/codex_pergunta.sh:399`.

`CENSO OK` legado deixou de ser reconhecido: um `ini` recente coberto apenas por OK antigo aparece sem aprovação. Existência dessas linhas na janela produtiva: **[nao verificado]**. `bin/disjuntor.py:1946`; `bin/disjuntor.py:2029`.

10. **OBSERVACAO — classes estruturais ainda presentes.**

| Classe | Situação |
|---|---|
| **Segundo juiz** | Não apareceu clone do juiz de forma; divisão normal por via está implementada. Há seleção paralela do que representa parada vigente. `bin/disjuntor.py:1617`; `bin/disjuntor.py:1731`; `bin/handoff_sessao.sh:102`; `bin/alarme_sessao_ociosa.py:124`. |
| **Segundo escritor fora da porta** | Presente e capaz de sobrescrever publicação concorrente. `bin/gerar_cortes.py:136`; `bin/deploy_agendado.sh:44`. |
| **Relógio fora da borda** | Janela não exclui futuro; escritor da trilha captura seu próprio `datetime.now()`. `bin/disjuntor.py:173`; `bin/disjuntor.py:2003`; `bin/disjuntor.py:2024`. |
| **Derivação paralela** | Stop: primeira; B3: duas; handoff: bloco; alarme: 40 físicas. `bin/hook_stop_fila1.py:411`; `bin/hook_stop_fila1.py:442`; `bin/handoff_sessao.sh:102`; `bin/alarme_sessao_ociosa.py:124`. |
| **Selo sem mordida** | Interação painel/B3 e mutantes do item 8 não são distinguidos. `bin/tests/test_censo_antes_da_vaga.sh:299`; `bin/tests/test_censo_antes_da_vaga.sh:394`. |
| **Ordem de trava** | Censo antes da vaga e idempotência sob trava estão corretos; escritores externos não participam. `bin/codex_pergunta.sh:234`; `bin/codex_pergunta.sh:240`; `app/core/esteira_vigia.py:891`; `bin/gerar_cortes.py:136`. |
| **Campo com dois significados** | Continua em `ini`/abertura e evento APROVADO/quantidade de briefs. `bin/codex_pergunta.sh:292`; `bin/disjuntor.py:1972`; `bin/disjuntor.py:2007`; `bin/disjuntor.py:2054`. |

## 09/11 04:05 · sessao 992b267357f7 · - · - · origem=Ronald (tmux)

1. **PROCEDENTE-candidato — aprovação + `ini` ainda não prova abertura.** O wrapper escreve `ini` antes de raspagem, espera por slot, trava da resposta e revalidação; essas etapas podem recusar sem chamar Codex. O leitor conta esse `ini` como abertura de fato. `bin/codex_pergunta.sh:292`; `bin/codex_pergunta.sh:309`; `bin/codex_pergunta.sh:318`; `bin/codex_pergunta.sh:376`; `bin/codex_pergunta.sh:399`; `bin/codex_pergunta.sh:401`; `bin/disjuntor.py:1972`; `bin/disjuntor.py:2023`. Ocorrência produtiva: **[nao verificado]**.

2. **PROCEDENTE-candidato — trilha e `ini` podem nascer em raízes diferentes e desaparecer da medida.** O censo recebe a raiz viva; `ini` usa `CODEX_LOGD`, cujo padrão é a raiz do script e cuja guarda permite outros diretórios sob `R/logs`. A medida lê exclusivamente `<raiz>/logs/codex/chamadas.log`. `bin/codex_pergunta.sh:88`; `bin/codex_pergunta.sh:193`; `bin/codex_pergunta.sh:234`; `bin/codex_pergunta.sh:293`; `bin/codex_limite_estado.py:22`; `bin/codex_limite_estado.py:52`; `bin/disjuntor.py:2018`. O selo verifica argv e `ini` separadamente da árvore da medida. `bin/tests/test_codex_pergunta.sh:222`; `bin/tests/test_censo_antes_da_vaga.sh:369`.

3. **PROCEDENTE-candidato — sobrevivente de consumo único.** Remover `k not in usados and` de `bin/disjuntor.py:2027` preservou, em memória, todos os números da fixture: sem=7, com=2, aprovados=4, negados=2, horas=1,9972 → texto `2.0`. A fixture reutiliza a identidade somente 55 minutos depois, quando a janela de 600 s já exclui a aprovação. `bin/tests/test_censo_antes_da_vaga.sh:383`; `bin/tests/test_censo_antes_da_vaga.sh:394`; `bin/tests/test_censo_antes_da_vaga.sh:397`. Na contraprova com uma aprovação e dois `ini` da mesma identidade em 10 s: original com=1/sem=1; mutante com=2/sem=0. Código: `bin/disjuntor.py:2027`; `bin/disjuntor.py:2033`. Execução integral dos selos: **[nao verificado]**.

4. **PROCEDENTE-candidato — sobrevivente do coletor.** Alterar `return todos, fora` para `return todos, []` preserva a negativa do único caso externo: ele não possui brief local e continua negando por ausência de briefs. `bin/disjuntor.py:1575`; `bin/disjuntor.py:1614`; `bin/tests/test_censo_antes_da_vaga.sh:214`. Em memória, bom local + referência externa: original nega; mutante libera. Não há essa combinação nos casos mostrados. `bin/tests/test_censo_antes_da_vaga.sh:208`; `bin/tests/test_censo_antes_da_vaga.sh:216`. Execução integral: **[nao verificado]**.

5. **PROCEDENTE-candidato — guarda de import depende da representação literal do JSON.** JSON válido com `tool_name:"\u0041gent"` e `/wt-` representado como `\/wt-` decodifica para o mesmo payload construtor, mas não satisfaz as substrings da guarda. Em memória, payload literal emitiu `deny`; equivalente escapado não emitiu. `bin/disjuntor.py:72`. O fallback de JSON ilegível repete a dependência literal. `bin/disjuntor.py:1662`. Que o harness abra após traceback/rc sem deny: **[nao verificado]**; a configuração apenas chama Python diretamente. `.claude/settings.json:68`.

6. **OBSERVACAO — outro mutante da guarda não está distinguido.** Remover a condição da chave desligada em `bin/disjuntor.py:72` mantém os casos de import apresentados, mas negaria Agent com import quebrado mesmo com a chave desligada. Os casos cobrem Agent quebrado, Bash quebrado e Agent conforme; não chave desligada + import quebrado. `bin/tests/test_censo_antes_da_vaga.sh:345`; `bin/tests/test_censo_antes_da_vaga.sh:352`. Sobrevivência integral: **[nao verificado]**.

7. **PROCEDENTE-candidato — janela não limita o futuro.** A medida usa apenas `t >= corte`, sem `t <= agora`, tanto nos eventos quanto nos `ini`. Um APROVADO Agent uma hora no futuro foi contado em memória como aprovado=1/com=1. `bin/disjuntor.py:1996`; `bin/disjuntor.py:2003`; `bin/disjuntor.py:2007`; `bin/disjuntor.py:2024`. Existência de carimbos futuros produtivos: **[nao verificado]**.

8. **OBSERVACAO — unidade de “briefs aprovados” é evento.** Uma linha pode listar vários briefs, mas a medida soma um por linha APROVADO; o rótulo diz briefs aprovados. `bin/disjuntor.py:1621`; `bin/disjuntor.py:2007`; `bin/disjuntor.py:2054`. Duas aprovações do mesmo arquivo somam duas; uma aprovação de dois arquivos soma uma. `bin/disjuntor.py:1621`; `bin/disjuntor.py:2007`.

9. **OBSERVACAO — compatibilidade da trilha antiga perdida.** `CENSO OK` não é reconhecido pela regex nova. Um `ini` recente coberto apenas por OK legado conta sem aprovação; negativas antigas não fecham por esse OK. `bin/disjuntor.py:1946`; `bin/disjuntor.py:2006`; `bin/disjuntor.py:2029`. Existência de linhas antigas na janela produtiva: **[nao verificado]**.

10. **OBSERVACAO — inventário de leitores de `ini`.** O escritor é o wrapper; o parser único constrói chave `(obra,hash,pid)`. `bin/codex_pergunta.sh:292`; `bin/codex_limite_estado.py:49`; `bin/codex_limite_estado.py:63`. Seus consumidores são contagem por papel, últimas chamadas, procedência por `fim.thread_id`, tokens por papel e censo; os últimos dois cruzam fatos diferentes. `bin/codex_limite_estado.py:71`; `bin/codex_limite_estado.py:82`; `bin/codex_limite_estado.py:92`; `bin/codex_limite_estado.py:185`; `bin/disjuntor.py:2018`. `gerar_codex_estado` usa a procedência para rotular origem=Code. `bin/gerar_codex_estado.py:153`.

11. **OBSERVACAO — trilhas atuais.** APROVADO: `_censo_nega`, com publicação adiada no Agent; FORA: `main_censo`; DESLIGADO: `main_censo` e `_censo_desligado`; negativa real chama-se `NEGOU DISJUNTOR CENSO`, sem evento literal CENSO NEGA. `bin/disjuntor.py:1623`; `bin/disjuntor.py:1707`; `bin/disjuntor.py:2162`; `bin/disjuntor.py:2148`; `bin/disjuntor.py:1779`; `bin/disjuntor.py:1801`; `bin/disjuntor.py:1812`; `bin/disjuntor.py:2167`. O escritor comum carimba `datetime.now()` e trunca o detalhe em 300 caracteres; a medida interpreta carimbo local, conta linhas, correlaciona obra+pid+hash12 em até 600 s e calcula união NEGOU→APROVADO dividida por 3600. `bin/disjuntor.py:168`; `bin/disjuntor.py:173`; `bin/disjuntor.py:1949`; `bin/disjuntor.py:1990`; `bin/disjuntor.py:2026`; `bin/disjuntor.py:2038`.

_3036 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
