# CODEX-ESTADO

> Ultima resposta processada: **02/11/2026 20:13** · publicado em **02/11/2026 20:47** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 02/11 20:13 · sessao 3d6058ec1077 · DEPLOY-[nome]-ELO-DA-CORRENTE · verificador · origem=Code

**1. Os seis furos não estão integralmente fechados.** Uso a enumeração resumida no contrato fornecido. (`contrato.txt:14`, anexo)

| Furo | Fechamento encontrado | Resultado |
|---|---|---|
| Teto → aviso → restart | No teto, retorna 75 sem reiniciar e nomeia o impedimento. Porém `_espera` soma apenas os sonos, excluindo varreduras e espera pelo flock; não é teto absoluto de tempo decorrido. (`bin/reinicio_sob_admissao.sh:29–39`) | **Parcial** |
| Agendador não repete | Existe repetição, mas perde falhas anteriores e associa a recusa somente por arquivo global/mtime. (`bin/deploy_agendado.sh:73–94`) | **Parcial** |
| Descoberta da corrente sem marca | A descoberta agora chama `adm_marcar` antes do Docker e desmarca depois. (`bin/encadeado.sh:46–53`) | **Fechado no fluxo normal** |
| RAW/híbridos dentro das cascas | Consulta descendentes vivos, mas declara expressamente que o intervalo entre dois exec permanece exposto. (`bin/admissao_cron.sh:18`, `63–68`, `87–92`) | **Aberto** |
| Corrida entre consulta e restart | Marca nova usa a mesma trava; deploy repete a varredura sob exclusão. Isso fecha o nascimento de marcas participantes, mas não a entrada posterior de um híbrido já admitido. (`bin/admissao_cron.sh:116–124`; `bin/reinicio_sob_admissao.sh:29–31`) | **Parcial** |
| Medida “137 no deploy” | Contador específico existe, mas fontes ausentes viram zero e nem todo restart tentado deixa registro. (`bin/rc137_no_restart.sh:26–50`; `bin/reinicio_sob_admissao.sh:43–49`) | **Parcial** |

**2. Sim, há caminho para restart com cron de casca vivo e marca válida.**

Um RAW nasce com `casca=0`; a última varredura ocorre entre seus execs e não encontra descendente Docker; o deploy conserva `.admissao`; o RAW inicia o exec seguinte sem passar novamente pela admissão. A sentinela oferece esse caminho concreto: é RAW e executa quatro comandos sucessivos em `mensageria`. A trava protege a criação da marca, não cada entrada posterior na casca. (`app/config/crons.py:669`; `mensageria/bin/sentinela_diario.sh:14–16`; `bin/admissao_cron.sh:117–124`; `bin/reinicio_sob_admissao.sh:30–43`)

Para um C/ENC com `casca=1`, marca criada antes da aquisição aparece na segunda varredura; um runner novo durante o restart espera antes de marcar e executar. Essa parte do protocolo fecha a corrida usual. (`bin/admissao_cron.sh:90`, `116–124`; `bin/reinicio_sob_admissao.sh:29–31`, `43–48`)

Marca antiga existente, contendo somente a data, é tratada conservadoramente: PID existente significa viva, e ausência de `casca` significa que segura. PID reutilizado pode causar recusa indevida; não autoriza restart. Migração de todos os escritores antigos para o protocolo novo: **[não verificado]**. (`bin/admissao_cron.sh:75–91`)

Há ainda uma entrada fora da porta: quando a admissão falha, o comando principal não roda, mas o vigia chama `docker exec ... cron_quebrado` sem admissão. (`bin/cron_run.sh:18–22`, `44–45`)

**3. Há caminhos de execução perdida e de espera sem limite garantido.**

- **Cron recusado não é reexecutado pelo runner:** vencidos os 900 segundos padrão, retorna 75 sem executar e sem carimbo de fim. O agendador novo repete deploy, não esse cron. (`bin/admissao_cron.sh:27`, `117`; `bin/cron_run.sh:18–25`, `34–38`; `bin/deploy_agendado.sh:90`)
- **Fechamento da admissão sem teto garantido:** `docker restart` não tem timeout e recebe o fd 8 aberto. Por inferência da herança do descritor, um filho sobrevivente pode conservar o flock após morrer o shell. Ocorrência em produção: **[não verificado]**. Saída normal sem filhos sobreviventes libera o descritor; ausência de trap, isoladamente, não prova vazamento. (`bin/reinicio_sob_admissao.sh:20`, `29`, `43–49`)
- **Espera infinita configurável:** com `ADM_PASSO_S=0` e marca impeditiva persistente, `_espera` nunca avança. Mesmo no padrão, o contador exclui o tempo das varreduras e dos flocks. (`bin/reinicio_sob_admissao.sh:17`, `27–39`)
- **Envelope morto com filho vivo:** a validade depende do PID do envelope; a varredura remove a marca se ele morreu, sem procurar seus filhos sobreviventes. O runner tem somente limpeza no `EXIT`, sem protocolo de término dos filhos. (`bin/admissao_cron.sh:75–76`, `99–102`; `bin/cron_run.sh:19–20`)
- **Marca retida após o trabalho:** o placar é escrito antes do vigia, mas a marca só sai no `EXIT`; um Docker de alarme/recuperação preso mantém a marca. (`bin/cron_run.sh:19`, `25`, `45–50`)

O RC do comando, quando o runner chega ao final, é preservado: captura em `RC`, grava e devolve esse valor. Entretanto, erro ao gravar o placar é ignorado; morte do envelope antes da gravação pode deixar a execução sem registro. Na descoberta, 70/75 ficam no CLOG e viram saída 2, sem linha própria no placar. (`bin/cron_run.sh:20–25`, `50`; `bin/encadeado.sh:52–61`)

**4. O repete não garante argv exato nem “pior tentativa”.**

Argumentos usuais, inclusive espaços e argumentos vazios explícitos, são preservados por array → NUL → `mapfile` → array. A exceção é **argv sem argumentos**: o `printf` grava um NUL, que volta como um argumento vazio; portanto a promessa de exatidão universal falha. (`bin/deploy.sh:48`, `278`; `bin/deploy_agendado.sh:87`, `90`)

A recusa é global, sobrescrita diretamente e identificada apenas por mtime em segundos. O loop não exige que a esteira tenha retornado 75; pode consumir recusa de outro deploy ou do mesmo segundo e repetir após falha em outro passo. (`bin/deploy.sh:278`; `bin/deploy_agendado.sh:73`, `81–90`)

**Pode executar entre 06:00 e 07:30:** o horário de agendamento é livre e a repetição não consulta essa proibição. A janela existente no deploy é de autenticação. Isso não cumpre o aval enquanto o instrumento ainda não pousou; cumprimento efetivo do aval em produção: **[não verificado]**. (`bin/deploy_agendado.sh:51–57`, `82–90`; `bin/deploy.sh:162–167`; `contrato.txt:16`, anexo)

`rc` é sobrescrito a cada repetição e só depois alimenta `PIOR`: 75 seguido de sucesso resulta em zero. Entre fatias, guarda o último não zero, sem comparação de gravidade. O próprio selo exige zero após recusa seguida de sucesso. (`bin/deploy_agendado.sh:83`, `91–94`, `103`; `bin/tests/test_deploy_nao_mata_elo.sh:162–165`)

**5. O contador tem mordida prevista, mas também verde sem testemunha.** [nao verificado]

O texto detalhado do placar corresponde ao cálculo `[ini−5, fim+60]`. “No minuto” é inexato: `fim` inclui a espera de subida das cascas. Conta qualquer linha com exit 137 nessa janela, sem filtrar casca ou demonstrar causalidade; não possui limite histórico de 26 horas. O recorte aos restarts registrados está declarado. (`bin/placar_code.sh:460`; `bin/rc137_no_restart.sh:6–8`, `27–48`; `bin/reinicio_sob_admissao.sh:42–48`)

**Fonte ausente devolve zero:** ignora `FileNotFoundError` de ambos os arquivos; sem janelas, nem abre o placar. Linhas inválidas também podem ser descartadas até restar zero. Outros erros de leitura não capturados produzem desconhecido, que o placar pesa como 1. (`bin/rc137_no_restart.sh:26–50`; `bin/placar_code.sh:459–461`)

Há lacuna no escritor: falha do próprio `docker restart` sai antes de registrar; falha no append final é seguida de `exit 0`. Assim, uma tentativa parcialmente destrutiva ou um registro perdido pode escapar da medida. (`bin/reinicio_sob_admissao.sh:43`, `48–49`)

A fixture exige 1/rc 2 para o caso casado e zero para janela distante; o contador entra no TOTAL. Execução integral do selo e dos mutantes: **[não verificado]**. (`bin/tests/test_deploy_nao_mata_elo.sh:170–178`; `bin/placar_code.sh:537`)

**6. Escritores, leitores e incompatibilidades.**

| Arquivo/campo | Quem escreve/remove | Quem lê |
|---|---|---|
| `crons_em_curso/*` — nome/empresa/PID, `ini,pid,starttime,exe,casca,cmd` | `adm_marcar`, chamado pelo runner e descoberta; limpeza pelo EXIT, chamada explícita ou varredura. (`bin/admissao_cron.sh:113–128`; `bin/cron_run.sh:16–19`; `bin/encadeado.sh:48–50`) | PID do nome, `starttime` e `exe` validam identidade; `casca` decide proteção ou consulta árvore. `ini`, `pid` interno e `cmd` não participam dessa decisão. (`bin/admissao_cron.sh:73–100`) |
| `.admissao` | Runner/descoberta e restart abrem o arquivo; serve de flock, sem campos de conteúdo. (`bin/admissao_cron.sh:116–124`; `bin/reinicio_sob_admissao.sh:20–32`) | Os mesmos participantes disputam a exclusão. (`bin/admissao_cron.sh:117`; `bin/reinicio_sob_admissao.sh:29`) |
| `recusa_cron.pendente` — argv NUL | Deploy grava na recusa; deploy normal concluído remove. (`bin/deploy.sh:278`, `353`) | Agendador consulta existência/mtime e lê argv. (`bin/deploy_agendado.sh:85–93`) |
| `restarts.log` — `ini fim`, epoch | Helper, após subida ou falha da subida. (`bin/reinicio_sob_admissao.sh:46–48`) | Contador temporal. (`bin/rc137_no_restart.sh:27–30`) |
| `placar.jsonl` — exit 70/75 | Runner registra recusa como linha do placar. (`bin/cron_run.sh:16–25`) | Vigia imediato, `crons_quebrados`, pipeline e duração. (`bin/cron_run.sh:44–45`; `app/core/services/crons_quebrados.py:33–47`; `app/core/management/commands/pipeline_placar.py:69–80`; `bin/crons_duracao.py:69–75`) |

**Sim, leitores passam a representar recusa como execução quebrada:** 70/75 entram em `crons_quebrados`; a pauta diz “terminou com erro”; o pipeline incrementa `rodou` e `erros`, embora o comando não tenha rodado. A duração também consome essas linhas sem filtrar exit. (`app/core/services/crons_quebrados.py:16`, `46–47`, `68`; `app/core/management/commands/pipeline_placar.py:73–80`; `bin/crons_duracao.py:69–75`; `bin/cron_run.sh:22`)

O handoff não interpreta o novo formato nem inclui `.admissao` na lista de recursos: consulta somente teste e sombra nesse trecho. (`bin/handoff_sessao.sh:223–230`)

**7. Classes de reprovação.**

- **Execução fora da porta:** confirmada nos execs posteriores dos híbridos e no Docker do vigia após recusa. (`mensageria/bin/sentinela_diario.sh:14–16`; `bin/cron_run.sh:44–45`)
- **Selo sem mordida suficiente:** confirmada para transição do híbrido e corrida antes da aquisição; os casos existentes exercitam híbrido já em exec e chegada após restart iniciado. (`bin/tests/test_deploy_nao_mata_elo.sh:90–98`, `114–126`)
- **Segundo juiz registrado / regra de negócio lendo relógio fora da borda:** **[não verificado]**. O defeito temporal demonstrado aqui é a atribuição da recusa por mtime, sem identidade da tentativa. (`bin/deploy_agendado.sh:81–93`)
- **Inversão de locks:** ciclo concreto **[não verificado]**. A cadeia toma árvore e depois régua; o retry toma régua, mas não foi demonstrado elo inverso que forme deadlock. (`bin/molde_fatia/cadeia.sh:11–14`; `bin/deploy_agendado.sh:90`)
- **Pouso antes dos pré requisitos:** ocorrência efetiva **[não verificado]**. Existe, porém, caminho para repetir deploy sem provar que a recusa pertence àquela esteira. (`bin/deploy_agendado.sh:83–90`)

**8. O selo não demonstra mordida para todos os furos.**

| Furo | Cobertura e mutante/caso faltante |
|---|---|
| Teto → restart | Exercita diretamente o helper. Não executa o deploy real; remover sua chamada em `deploy.sh` não é exercitado pelo selo comportamental. (`bin/tests/test_deploy_nao_mata_elo.sh:35`, `62`, `74`; `bin/deploy.sh:275`) |
| Repetição | Exercita recusa/sucesso e recusa persistente. Não pega perda da pior tentativa; exige o resultado que a mascara. Falta sequência de falhas diferentes entre tentativas/fatias. (`bin/tests/test_deploy_nao_mata_elo.sh:149–168`) |
| Descoberta | Testa espera pela admissão. Não verifica marca protegendo uma descoberta em andamento contra restart, nem sinais/filhos sobreviventes. (`bin/tests/test_deploy_nao_mata_elo.sh:138–146`) |
| RAW/híbrido | Falta híbrido entre dois execs e cobertura efetiva de RAW em `mensageria`/`saas_ui`; o caso positivo usa `saas_core`. (`bin/tests/test_deploy_nao_mata_elo.sh:90–98`, `203–211`) |
| Corrida | **Retirar a segunda varredura** não é provocado pelo caso existente: ele inicia o cron depois que o restart já começou. Falta injetar marca entre primeira varredura e flock. (`bin/reinicio_sob_admissao.sh:27–31`; `bin/tests/test_deploy_nao_mata_elo.sh:114–126`) |
| Medida | Falta mutante “fonte ausente → zero”, erro no registro, restart com falha e retirada da contribuição ao TOTAL. (`bin/tests/test_deploy_nao_mata_elo.sh:170–178`; `bin/placar_code.sh:537`) |

Também faltam marca antiga, expiração dos 900 segundos, sinais, flock herdado e teto medido por tempo decorrido. Sobrevivência efetivamente executada desses mutantes: **[não verificado]**. (`bin/tests/test_deploy_nao_mata_elo.sh:102–146`)

O selo está ligado à régua pelo glob; o placar continua chamando o contrato textual. (`bin/regua.sh:138–145`; `bin/placar_code.sh:130–132`)

**Veredito final: BLOQUEIA.**

- Híbrido válido ainda atravessa o restart; vigia executa Docker fora da admissão. (`bin/admissao_cron.sh:18`; `bin/cron_run.sh:45`)
- Teto não absoluto, restart sem timeout e crons descartados ao vencer a admissão. (`bin/reinicio_sob_admissao.sh:27–43`; `bin/admissao_cron.sh:117`)
- Repetição sem identidade da tentativa, perda do pior rc e ausência da guarda temporária de horário. (`bin/deploy_agendado.sh:81–94`; `contrato.txt:16`, anexo)
- Contador pode declarar zero sem fonte ou sem registro do restart; consumidores confundem recusa com execução quebrada. (`bin/rc137_no_restart.sh:31–50`; `bin/reinicio_sob_admissao.sh:43`; `app/core/management/commands/pipeline_placar.py:73–79`)
- Selo não cobre as interleavings e falhas acima. (`bin/tests/test_deploy_nao_mata_elo.sh:90–178`)

## 02/11 20:11 · sessao 5b5ae1ad4b7e · - · - · origem=Ronald (tmux)

- **argv usual preservado**, por array → NUL → `mapfile` → expansão de array. **argv vazio não é exato**: `printf '%s\0'` sem operandos grava um NUL, lido como um argumento vazio. Confirmei essa semântica somente em memória/pipe. (`bin/deploy.sh:48`; `bin/deploy.sh:278`; `bin/deploy_agendado.sh:87`; `bin/deploy_agendado.sh:90`)

- **Pode executar entre 06:00 e 07:30**: `agendar` aceita horário sem essa restrição; `rodar` executa esteira e repetição sem conferência de janela. A guarda de `deploy.sh` é de autenticação e não implementa o aval temporário. (`bin/deploy_agendado.sh:51`; `bin/deploy_agendado.sh:55`; `bin/deploy_agendado.sh:82`; `bin/deploy_agendado.sh:90`; `bin/deploy.sh:162`; aval: `avais.txt`, anexo, linha não fornecida)

- **`PIOR` não representa a pior tentativa**: o `rc` inicial é sobrescrito por cada repetição; a agregação ocorre somente depois. Uma recusa seguida de sucesso resulta em zero — comportamento explicitamente esperado pelo selo. Entre fatias, `PIOR` guarda o último resultado não zero, sem comparação de gravidade. (`bin/deploy_agendado.sh:83`; `bin/deploy_agendado.sh:91`; `bin/deploy_agendado.sh:94`; `bin/deploy_agendado.sh:103`; `bin/tests/test_deploy_nao_mata_elo.sh:162`)

- **Recusa sem identidade de tentativa/fatia**: é um arquivo global, associado apenas por `mtime` em segundos. A repetição não exige `rc=75`; pode consumir recusa de outro deploy ou uma recusa anterior criada no mesmo segundo. Logo, existe caminho lógico para repetir deploy após falha da esteira em outro passo. (`bin/deploy_agendado.sh:73`; `bin/deploy_agendado.sh:81`; `bin/deploy_agendado.sh:83`; `bin/deploy_agendado.sh:85`)

- **Admissão pode permanecer fechada sem teto**: `docker restart` não usa `timeout` e herda o fd 8; o script não fecha esse fd antes dos filhos. Por inferência, cliente pendurado mantém a trava; se o shell morrer e esse filho sobreviver, o filho continua segurando-a. Novos crons esgotam os 900 segundos e saem 75 sem executar. (`bin/reinicio_sob_admissao.sh:20`; `bin/reinicio_sob_admissao.sh:29`; `bin/reinicio_sob_admissao.sh:43`; `bin/admissao_cron.sh:27`; `bin/admissao_cron.sh:117`; `bin/cron_run.sh:17`)

- **Teto de espera não é absoluto no relógio**: `_espera` acumula somente os sonos, omitindo varreduras e cada espera de `flock -w "$TETO"`. Com `ADM_PASSO_S=0`, `_espera` não avança e cron vivo impede o término indefinidamente. (`bin/reinicio_sob_admissao.sh:17`; `bin/reinicio_sob_admissao.sh:27`; `bin/reinicio_sob_admissao.sh:29`; `bin/reinicio_sob_admissao.sh:34`; `bin/reinicio_sob_admissao.sh:39`)

- **Não há ciclo de locks demonstrado**: retry toma somente `regua.lock`; a cadeia toma árvore e depois régua. Não encontrei aquisição de árvore pelo deploy enquanto segura régua; portanto, deadlock por inversão e segundo escritor concorrente confirmado são **[não verificado]**. (`bin/deploy_agendado.sh:74`; `bin/deploy_agendado.sh:90`; `bin/molde_fatia/cadeia.sh:11`; `bin/molde_fatia/cadeia.sh:14`; `bin/trava.sh:64`)

- **Pouso efetivo antes de pré requisito: [não verificado]**. A cópia não possui histórico Git consultável; execução de produção e cumprimento do aval não foram demonstrados pelos anexos. **[não verificado]** [nao verificado]

## 02/11 20:11 · sessao 69891d8d3687 · - · - · origem=Ronald (tmux)

Achados para os itens 5 e 8:

- **Fonte ausente vira zero silencioso:** confirmei, em execução somente de leitura, que ambas as fontes inexistentes produzem `rc137_no_restart=0`, rc 0. `FileNotFoundError` é ignorado; sem janelas, nem tenta abrir o placar. (`bin/rc137_no_restart.sh:26–36`, `:49–55`)
- **Outros erros de leitura não viram zero:** não são capturados pelo Python; saída vazia vira `?` e pesa 1 no placar. JSON inválido e `fim` inválido são descartados silenciosamente. (`bin/rc137_no_restart.sh:37–46`; `bin/placar_code.sh:459–461`)
- **Rótulo:** a explicação detalhada do placar corresponde ao cálculo `[ini−5, fim+60]`; “no minuto” é abreviação inexata, porque `fim` vem depois das provas de subida. Não filtra cron de casca, apenas qualquer registro com exit 137; é correlação temporal, sem demonstração de causalidade. Universo histórico explicitamente limitado aos restarts registrados. (`bin/rc137_no_restart.sh:6–8`, `:29–48`; `bin/reinicio_sob_admissao.sh:42–48`; `bin/placar_code.sh:460`)
- **Buracos do registro:** falha de `docker restart` sai sem registrar janela; falha ao gravar a janela final é seguida de `exit 0`. Uma tentativa parcialmente destrutiva ou registro perdido pode escapar do contador. (`bin/reinicio_sob_admissao.sh:43`, `:48–49`)
- **Mordida positiva existe como fixture:** registro 137 casado deve contar 1/rc 2; janela distante deve contar 0. Não há caso de fonte ausente/corrompida ou de restart com erro. Execução do selo/mutantes: **[não verificado]**. (`bin/tests/test_deploy_nao_mata_elo.sh:170–178`)

Cobertura dos seis furos resumidos no contrato:

| Furo | Caso existente | Falta relevante |
|---|---|---|
| Teto → restart | Recusa/nome/nenhum restart no teto; espera término. (`bin/tests/test_deploy_nao_mata_elo.sh:59–79`) | O selo chama o helper diretamente; não copia/executa o deploy real. Mutante que desliga a chamada do helper no deploy não é exercitado. (`:35`, `:62`, `:74`; `bin/deploy.sh:275`) |
| Agendador não repete | Recusa seguida de sucesso; recusa persistente. (`:149–168`) | Expectativa explícita de rc 0 após uma recusa impede demonstrar “pior tentativa”; sem sequência de erros diferentes entre fatias. (`:162–168`) |
| Descoberta sem marca | Retém `.admissao` e verifica horário do `docker exec`. (`:138–146`) | Sinais, filhos sobreviventes e erro durante descoberta não são exercitados. (`:138–146`) |
| RAW/híbrido | Host puro e híbrido já dentro do exec. (`:83–98`) | Intervalo entre dois exec não é testado; tampouco RAW em `mensageria`/`saas_ui`. O censo positivo só renderiza C/saas_core e ENC/saas_core. (`:90–98`, `:203–211`) |
| Corrida | Cron chega depois de `docker restart` já ter sido chamado. (`:114–126`) | **Mutante retirar a segunda varredura** não é provocado: não injeta marca entre primeira varredura e aquisição da trava. (`bin/reinicio_sob_admissao.sh:27–31`; teste `:116–117`) |
| Medida 137 | Fixture positivo/negativo. (`:171–178`) | Ausência de fontes, erro do writer, restart com falha, integração do contador no TOTAL e universo de crons não são exercitados. (`:171–178`; `bin/placar_code.sh:537`) |

Também faltam casos de marca antiga, expiração de `ADM_ESPERA_ADMISSAO_S`, sinais, descritor herdado e teto de tempo efetivamente decorrido; as marcas do teste de órfãs são todas do formato novo e o teste de chegada tardia espera só três segundos. (`bin/tests/test_deploy_nao_mata_elo.sh:102–109`, `:114–126`; `bin/admissao_cron.sh:27`, `:77–91`)

O selo está conectado à régua pelo glob, que bloqueia quando falha; porém o atalho `JA VERDE` precede esse loop. O pre-push declara que não executa o loop de selos de host. O placar chama o contrato textual, sem executar este selo comportamental. (`bin/regua.sh:84–94`, `:138–145`; `bin/pre-push.sh:13–16`; `bin/placar_code.sh:130–132`)

O selo usa placar local ou fallback absoluto de produção para conferir duração; o dado fornecido não comprova os max/p95 declarados nos comentários. (`bin/tests/test_deploy_nao_mata_elo.sh:181–197`; `bin/admissao_cron.sh:20–23`)

## 02/11 20:11 · sessao eec33bfe3a9d · - · - · origem=Ronald (tmux)

- **Marcas `logs/crons_em_curso/*`:** `adm_marcar` escreve nome/empresa/PID no caminho e `ini,pid,starttime,exe,casca,cmd` no conteúdo, com temporário oculto e `mv`; é chamado pelo runner e pela descoberta da corrente. (`bin/admissao_cron.sh:113`; `bin/admissao_cron.sh:120`; `bin/cron_run.sh:16`; `bin/encadeado.sh:48`)
- **Leitores das marcas:** `adm_marca_viva` lê PID do nome, `starttime` e `exe`; `adm_segura` lê `casca` e, se zero, consulta descendentes vivos; `adm_varrer` remove órfãs. `ini`, `pid` do conteúdo e `cmd` são testemunhas sem uso decisório nesse leitor. (`bin/admissao_cron.sh:73`; `bin/admissao_cron.sh:87`; `bin/admissao_cron.sh:95`)
- **Quem apaga:** runner pelo `trap EXIT`; descoberta por chamada explícita após Docker; varredura apaga órfãs. (`bin/cron_run.sh:19`; `bin/encadeado.sh:50`; `bin/admissao_cron.sh:102`; `bin/admissao_cron.sh:128`)
- **`.admissao`:** runner/descoberta abrem fd8 e tomam exclusão antes de escrever a marca, fechando fd8 antes de rodar o comando. Restart abre o mesmo arquivo, toma exclusão, repete varredura e conserva fd8 durante Docker/subida. (`bin/admissao_cron.sh:116`; `bin/admissao_cron.sh:117`; `bin/admissao_cron.sh:124`; `bin/reinicio_sob_admissao.sh:20`; `bin/reinicio_sob_admissao.sh:29`; `bin/reinicio_sob_admissao.sh:30`; `bin/reinicio_sob_admissao.sh:43`)
- **`recusa_cron.pendente`:** deploy grava argv separado por NUL quando recebe 75; agendador lê por `mapfile -d ''`, consulta mtime e repete; deploy normal que chega ao final remove. (`bin/deploy.sh:278`; `bin/deploy_agendado.sh:85`; `bin/deploy_agendado.sh:87`; `bin/deploy_agendado.sh:90`; `bin/deploy.sh:353`)
- **`restarts.log`:** restart grava `ini fim` depois da subida ou de falha da subida; contador lê ambos como epoch. Falha do próprio `docker restart` sai antes da gravação. (`bin/reinicio_sob_admissao.sh:43`; `bin/reinicio_sob_admissao.sh:46`; `bin/reinicio_sob_admissao.sh:48`; `bin/rc137_no_restart.sh:27`)
- **`placar.jsonl`:** runner registra `RC` do comando ou 70/75 da admissão; RC do comando permanece em variável e é devolvido ao final. Falha ao anexar JSON não impede execução dos passos seguintes nem altera `RC`; não há verificação da gravação. (`bin/cron_run.sh:16`; `bin/cron_run.sh:20`; `bin/cron_run.sh:25`; `bin/cron_run.sh:50`)

**Consumidores incompatíveis com “não rodou”:** [nao verificado]

- 70/75 viram “cron quebrado” no vigia imediato e no serviço da última corrida, apesar de o comando não ter rodado. O serviço só exclui 0/2 e o texto da pauta diz “terminou com erro”. (`bin/cron_run.sh:18`; `bin/cron_run.sh:22`; `bin/cron_run.sh:44`; `app/core/services/crons_quebrados.py:16`; `app/core/services/crons_quebrados.py:46`; `app/core/services/crons_quebrados.py:68`)
- `pipeline_placar` conta ambos como `rodou += 1` e como erro; isso passa a contabilizar tentativa recusada como execução. (`app/core/management/commands/pipeline_placar.py:73`; `app/core/management/commands/pipeline_placar.py:74`; `app/core/management/commands/pipeline_placar.py:78`)
- `crons_duracao.py` também recebe duração da recusa: não filtra `exit`, e usa `dur` das linhas no horário. (`bin/crons_duracao.py:69`; `bin/crons_duracao.py:71`; `bin/crons_duracao.py:75`)
- Handoff não acompanha a admissão: sua relação de travas consultadas contém somente teste e sombra; não passa a interpretar o novo formato das marcas. (`bin/handoff_sessao.sh:223`; `bin/handoff_sessao.sh:224`; `bin/handoff_sessao.sh:230`)

**Segundo caminho Docker fora da porta:** depois de admissão recusada, o runner chama `docker exec ... cron_quebrado` mesmo sem marca; o comando principal fica bloqueado, mas o alarme entra na casca durante admissão fechada. O Docker do vigia tampouco tem timeout. (`bin/cron_run.sh:18`; `bin/cron_run.sh:22`; `bin/cron_run.sh:44`; `bin/cron_run.sh:45`)

**Prisão/atraso:** a marca permanece até terminar o vigia, porque limpeza está no EXIT; assim, Docker do alarme/recuperação preso mantém marca após o RC principal já estar no placar. (`bin/cron_run.sh:19`; `bin/cron_run.sh:25`; `bin/cron_run.sh:45`; `bin/cron_run.sh:47`; `bin/cron_run.sh:50`)

**Herança de trava:** fd8 fica aberto nos subprocessos do restart, sem fechamento nos filhos; `docker restart` não tem timeout. Um Docker que demore além de `ADM_ESPERA_ADMISSAO_S` faz o cron sair 75 sem execução. A possibilidade de filho sobrevivente reter o flock é inferência da herança de descritor; ensaio de sinais: **[não verificado]**. (`bin/reinicio_sob_admissao.sh:20`; `bin/reinicio_sob_admissao.sh:43`; `bin/reinicio_sob_admissao.sh:45`; `bin/admissao_cron.sh:26`; `bin/admissao_cron.sh:117`; `bin/cron_run.sh:18`)

## 02/11 19:35 · sessao 629e02d70665 · E4-CALENDARIO-L1 · censo · origem=Code

Uso a numeração dos anexos transcritos, incluindo linhas vazias; em `triagem.txt`, preservo os números fornecidos. Desdobro dois achados compostos para distinguir falha de revisão de ausência de evidência.

A cópia local conserva trechos anteriores ao delta; portanto, ela não confirma integralmente o estado final descrito pelo opus. (`app/templates/colaboradores/partials/_calendario_grade.html:103`; `app/colaboradores/services/calendario.py:619`; `app/core/juizes.py:839`)

1. **BLOQUEIA — D5 nas competências entregues: (e), NÃO PERGUNTADO no requisito necessário.**

   **Pergunta:** “pouso antes do pre-requisito”. (`v1_pergunta.txt:10`)   [nao verificado]
   **Resposta:** “sequência efetiva de pouso/apply: **[nao verificado]**”. (`v1_resposta.txt:46`) [nao verificado]

   O tema genérico estava presente, mas faltou o requisito específico: retirar a guarda e completar 06–09 antes do leitor. A triagem admite literalmente: “a pergunta ao Codex nao anexou AVAL_L1_COMPLEMENTO.md”. Isso sustenta **(e)**; não transforma a guarda visível em questão exclusivamente de produção. (`triagem.txt:5`; `AVAL_L1_COMPLEMENTO.md:3`; `AVAL_L1_COMPLEMENTO.md:9`; `app/ponto/management/commands/lavrar_status_competencia.py:40`)

   **Deveria levar:** COMPLEMENTO integral e cobrança explícita do PRONTO(1), separando comprovação pelo código da comprovação pelos dados. (`AVAL_L1_COMPLEMENTO.md:10`)

2. **BLOQUEIA — rodapé sem cards `sem_certidao`/`sem_dados`: (e), NÃO PERGUNTADO na exigência específica.**

   **Pergunta:** “ainda deriva status ou rotulo por regra propria”. (`v1_pergunta.txt:8`)  
   **Resposta:** “rótulo próprio no calendário — bloqueia”. (`v1_resposta.txt:17`)

   Revisar derivação textual não explicita a obrigação de representar essas duas contagens. A pergunta não identifica o REEQ entre seus anexos; a triagem identifica nele a exigência de rodapé. O conteúdo integral desse aval não foi fornecido aqui. (`v1_pergunta.txt:1`; `triagem.txt:62`; exigência literal completa: **[nao verificado]**)

   **Deveria levar:** REEQ integral e lista dos status que o rodapé deve representar, incluindo os dois ausentes. (`triagem.txt:5`; `triagem.txt:62`)

3. **BLOQUEIA — “Sem registro” sobre `resumo.faltas`: (a), tema presente e achado perdido.**

   **Pergunta:** “Algum leitor […] ainda deriva status ou rotulo por regra propria”. (`v1_pergunta.txt:8`)  
   **Resposta:** “o rodapé afirma que todo `aberto` ‘não teve saída’”. (`v1_resposta.txt:20`)

   O Codex examinou a fidelidade dos textos do rodapé, mas não apontou esse outro rótulo. A associação entre “Sem registro”, ausência de batidas e `resumo.faltas` está diretamente visível no template. (`app/templates/colaboradores/partials/_calendario_grade.html:145`; `app/templates/colaboradores/partials/_calendario_grade.html:146`; `app/templates/colaboradores/partials/_calendario_grade.html:147`; `opus_final.txt:2`)

   **Deveria levar:** conferir cada título e tooltip contra o predicado real do respectivo contador, inclusive falta lavrada ou decidida. (`opus_final.txt:2`)

4. **BLOQUEIA — publicação de `eh_trabalho`: (e), faltou a inclusão explícita na L1.**

   **Pergunta:** “passa a LER status e rotulo do dia”. (`v1_pergunta.txt:2`)  
   **Resposta:** “Ausente no classificador removido do calendário”. (`v1_resposta.txt:40`)

   O objeto declarado era status/rótulo. O BACKLOG disponível já registrava `eh_trabalho`, mas como fila da E4-CALENDARIO; a inclusão específica na L1 depende do DOMINIO MUDA 4 mencionado pelo opus, cujo texto integral não foi fornecido. (`app/docs/BACKLOG.md:827`; `opus_final.txt:2`)

   **Deveria levar:** DOMINIO MUDA 4 e pergunta explícita sobre quem publica `eh_trabalho` para o resolver e o vigia. (`opus_final.txt:2`; `triagem.txt:5`)

5. **FILA B — paridade mensal/unitária e `carga=`: (b), visto e encaminhado à fila.**

   **Pergunta:** “foi a fila como obra propria do pareador”. (`v2_pergunta.txt:1`)  
   **Resposta:** “fila; triagem procede pelos anexos, com ressalva”. (`v2_resposta.txt:34`)

   O Codex identificou o contraexemplo e depois aceitou a preexistência com ressalva. O motivo **procede para a divergência já existente**, mas não comprova a inocuidade do novo contrato com `carga=`: a própria resposta registra resultados de status diferentes entre base e atual. (`v1_resposta.txt:60`; `v2_resposta.txt:34`; `v2_resposta.txt:36`; `opus_final.txt:2`)

   **Linha da triagem que decidiu o encaminhamento:** **[nao verificado]**; essa decisão aparece na segunda pergunta, não nas linhas de triagem fornecidas.

   **Deveria levar:** separar preexistência do pareador, responsabilidade do D4 pela janela recebida e registro efetivo da fila no BACKLOG. (`v2_resposta.txt:36`; `opus_final.txt:2`)

6. **FILA C — constantes nos cards `escala_suspeita`/Abertos: (b), visto; rebaixamento com documentação incompleta.**

   **Pergunta:** “QUEM ESCREVE e QUEM LE […] ROTULO_EM_ANDAMENTO e o texto do card”. (`v2_pergunta.txt:2`)  
   **Resposta:** “Card `escala_suspeita` — bloqueia”; “fila para duplicação textual”. (`v2_resposta.txt:5`; `v2_resposta.txt:7`)

   Portanto, o Codex **viu os dois problemas**. O envio do bloqueio à fila consta do relato inicial do pedido; a linha da triagem e seu motivo específico são **[nao verificado]**. Uma justificativa baseada em precisar tocar a lavra não sustentaria a duplicação dos cards: a resposta identifica o próprio template como escritor e registra a constante central existente. (`v2_resposta.txt:15`; `v2_resposta.txt:16`)

   **Deveria levar:** decisão literal da triagem, distinguindo corrigir o vocabulário central de substituir cópias textuais nos leitores. (`v2_resposta.txt:5`; `v2_resposta.txt:7`)

7. **FILA A2 — propagação derrubando consumidores inteiros: (a), tema presente; alcance não examinado.**

   **Pergunta:** menciona “furos, vigia_fio” e cobra o teste de propagação da A2. (`v1_pergunta.txt:8`; `v2_pergunta.txt:2`)   [nao verificado]
   **Resposta:** “Retirar apenas o `raise` atual deixa de satisfazer esse contrato”. (`v2_resposta.txt:24`)

   O Codex verificou que o teste exige propagação, mas não discutiu sua consequência nos consumidores coletivos. O relatório de furos chama o calendário dentro do processamento dos colaboradores. (`v2_resposta.txt:24`; `app/relatorios/furos.py:187`; `app/relatorios/furos.py:195`; `opus_final.txt:2`)

   **Deveria levar:** rastrear uma exceção A2 pelos chamadores e explicitar qual unidade de processamento ela aborta. (`opus_final.txt:2`)

8. **FILA A2 — frequência e impacto real em produção: (c), fora do alcance evidenciado.**

   **Pergunta:** cobra se o teste “morde […] o defeito”. (`v2_pergunta.txt:2`)   [nao verificado]
   **Resposta:** “exige a exceção injetada e o log específico”. (`v2_resposta.txt:24`)

   Isso demonstra contrato de teste, não incidência operacional. O opus declara que não verificou logs de produção; os dados fornecidos não quantificam falhas A2. (`opus_final.txt:2`; frequência e impacto observado: **[nao verificado]**) [nao verificado]

   **Deveria levar:** medição raspada de falhas A2 e execuções afetadas; sem ela, manter o impacto observado como não verificado.

9. **FILA — pendente TT6 obsoleto: (a), tema documental presente.**

   **Pergunta:** anexa JUIZES e pede avaliação de baixa de pendente em `core/juizes.py`. (`v1_pergunta.txt:1`; `v1_pergunta.txt:12`)  
   **Resposta:** “A impressão removida pertencia ao classificador apagado”. (`v1_resposta.txt:58`)

   A resposta tratou o pendente nomeado, mas não identificou a incoerência residual do TT6 descrita pelo opus. A reancoragem final não é confirmada pela cópia local, que ainda conserva a âncora antiga. (`opus_final.txt:2`; `app/core/juizes.py:839`; `app/core/juizes.py:840`)

   **Deveria levar:** conferir todos os pendentes alterados ou tornados obsoletos pela migração ao D4, comparando âncora e motivo. (`opus_final.txt:2`)

10. **FILA — comentário antigo no `_dd`: (a), tema A2 presente.**

    **Pergunta:** cobra a propagação de falha da A2. (`v2_pergunta.txt:2`)  
    **Resposta:** “Retirar apenas o `raise` atual deixa de satisfazer esse contrato”. (`v2_resposta.txt:24`)

    O Codex reconheceu o novo fluxo de exceção, mas não apontou o comentário que descrevia continuidade após falha. Esse comentário existe na cópia; sua obsolescência na árvore final é sustentada pelo anexo do opus. (`app/colaboradores/services/calendario.py:869`; `app/colaboradores/services/calendario.py:870`; `opus_final.txt:2`)

    **Deveria levar:** revisar comentários que descrevem caminhos eliminados pela propagação obrigatória da A2. (`opus_final.txt:2`)

**Contagem das 10 unidades acima:** **(a) 4; (b) 2; (c) 1; (e) 3; (d) 0.** Os dois casos **(b)** têm evidência de que o Codex viu o problema; as linhas específicas das decisões históricas da triagem permanecem **[nao verificado]**. (`v2_resposta.txt:5`; `v2_resposta.txt:7`; `v2_resposta.txt:34`)

## 02/11 19:33 · sessao 9ae37b8be3a2 · - · - · origem=Ronald (tmux)

A cópia local não reproduz os trechos pós-delta descritos nos anexos: tooltip antigo permanece no template, A2 não propaga exceção, e TT6 conserva a âncora antiga. (`app/templates/colaboradores/partials/_calendario_grade.html:103`; `app/colaboradores/services/calendario.py:619`; `app/core/juizes.py:839`)

- **Rodapé:** não contém cards `sem_certidao`/`sem_dados`; `resumo.faltas` aparece entre tooltip “Dia previsto de trabalho sem nenhuma batida registrada.” e título “Sem registro”. (`app/templates/colaboradores/partials/_calendario_grade.html:138`; `:145`; `:146`; `:147`; `:173`)
- **Textos próprios:** card `escala_suspeita` literal em `:161`; card Abertos ainda usa “Turno começou e não teve saída registrada.” em `:169`. (`app/templates/colaboradores/partials/_calendario_grade.html:161`; `:169`)
- **`eh_trabalho`:** calendário calcula `_fora`, chama `ec.eh_dia_trabalho` e usa resultado no predicado que oferece resolução. (`app/colaboradores/services/calendario.py:777`; `:780`; `:197`)
- **A2:** cópia local captura e registra erro sem `raise`; não confirma o comportamento pós-delta. Furos chama calendário e documenta execução para aproximadamente 750 colaboradores. (`app/colaboradores/services/calendario.py:619`; `:620`; `app/relatorios/furos.py:187`; `:195`)
- **Comentário `_dd`:** localmente descreve fallback ainda presente; inexequibilidade na árvore final depende dos anexos. (`app/colaboradores/services/calendario.py:619`; `:869`; `:871`; `:872`)
- **TT6:** localmente continua ancorado em `def status_do_dia`, motivo “deveria ler idem”; reancoragem final não confirmada pela cópia. (`app/core/juizes.py:839`; `:840`)

Identidade da cópia com `15d4f10b`: **[nao verificado]**.

## 02/11 19:23 · sessao 94fd4804a524 · E4-CALENDARIO-L1 · verificador · origem=Code

**O delta cura o tooltip diário e dá mordida aos testes, mas não certifica cura integral dos textos na origem: resta um bloqueio no card `escala_suspeita`.** (`app/templates/colaboradores/partials/_calendario_grade.html:103`, `app/templates/colaboradores/partials/_calendario_grade.html:161`; `app/colaboradores/tests/test_e4cal_l1_r7_grade_le_o_rotulo.py:41`)

1. **Item (1), tooltip diário — não procede manter o bloqueio.** Agora lê `dia.status_rotulo`; o calendário apenas repassa o rótulo recebido do D4. A régua do vínculo ativo saiu desse caminho, sem novo classificador no leitor. (`app/templates/colaboradores/partials/_calendario_grade.html:103`; `app/colaboradores/services/calendario.py:573`, `app/colaboradores/services/calendario.py:654`, `app/colaboradores/services/calendario.py:719`)

   **Card `escala_suspeita` — bloqueia a cura integral na origem.** Continua escrevendo “Sem batida em dia previsto” no template. Entretanto, a regra devolve `escala_suspeita` para `indefinida` também **com batida**; há teste explícito desse contrato. O próprio rótulo central conserva “Sem batida”, portanto só trocar o card por essa constante não resolveria a inconsistência: a correção precisa começar no núcleo. Isso demonstra inconsistência no código, sem demonstrar ocorrência em produção. (`app/templates/colaboradores/partials/_calendario_grade.html:161`; `app/ponto/status_dia.py:38`, `app/ponto/status_dia.py:122`; `app/ponto/tests/test_e3_status_lavrado.py:43`, `app/ponto/tests/test_e3_status_lavrado.py:50`)

2. **Item (2), “Abertos” — não procede manter o bloqueio funcional; fila para duplicação textual.** O contador inclui todo status `aberto`; o título agora contempla turno sem saída e dia ainda em andamento com batidas fechadas. Contudo, escreve cópias literais dos dois rótulos: não os lê do núcleo. Não surgiu novo predicado de classificação, mas a origem textual continua duplicada. (`app/colaboradores/services/calendario.py:780`; `app/ponto/status_dia.py:259`, `app/ponto/status_dia.py:263`; `app/templates/colaboradores/partials/_calendario_grade.html:169`)

3. **Quem escreve e quem lê depois do delta:**

   | Objeto | Escritor | Leitor |
   |---|---|---|
   | `CelulaDia.status_rotulo` | A porta consulta `status_da_lavra`/`status_do_admin`, atribui e persiste. (`app/ponto/portas/celula.py:693`, `app/ponto/portas/celula.py:705`, `app/ponto/portas/celula.py:773`) | D4 lê a coluna, com fallback ao vocabulário se vazia. (`app/ponto/status_dia.py:268`) |
   | `dia.status_rotulo` do calendário | Contexto copia `_st['rotulo']`. (`app/colaboradores/services/calendario.py:719`) | Ícones da grade. (`app/templates/colaboradores/partials/_calendario_grade.html:99`, `app/templates/colaboradores/partials/_calendario_grade.html:107`) |
   | `ROTULO_EM_ANDAMENTO` | Constante do núcleo. (`app/ponto/status_dia.py:56`) | D4 e teste do card; o card contém uma cópia literal. (`app/ponto/status_dia.py:263`; `app/colaboradores/tests/test_e4cal_l1_r7_grade_le_o_rotulo.py:50`; `app/templates/colaboradores/partials/_calendario_grade.html:169`) |
   | Texto do card `escala_suspeita` | O próprio template. (`app/templates/colaboradores/partials/_calendario_grade.html:161`) | O `title` entregue ao navegador; do contexto, o card lê somente a contagem. (`app/templates/colaboradores/partials/_calendario_grade.html:162`) |

   Existe outro `status_rotulo`, de **previsão**, montado por `escala/views.py`; ele não escreve a coluna persistida. (`app/escala/views.py:54`, `app/escala/views.py:60`; `app/templates/escala/minha_escala.html:75`)

4. **Item (5), testes sem mordida — não procede para os defeitos específicos, por inspeção.** [nao verificado]

   - **Render:** restaurar o tooltip antigo elimina o `title="SENTINELA"` exigido; restaurar “Abertos” antigo elimina `ROTULO_EM_ANDAMENTO` do título. (`app/colaboradores/tests/test_e4cal_l1_r7_grade_le_o_rotulo.py:41`, `app/colaboradores/tests/test_e4cal_l1_r7_grade_le_o_rotulo.py:49`, `app/colaboradores/tests/test_e4cal_l1_r7_grade_le_o_rotulo.py:50`)
   - **Lavra absolve:** mantém trabalho previsto e nenhuma batida; alterna somente as colunas entre `ok`, `irregular` e `ok` com sentinela. Rejulgar pela grade ou reconstruir o rótulo contraria as asserções. É teste de leitura da coluna, não da escrita pela porta. (`app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:250`, `app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:256`, `app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:259`)
   - **Falha A2:** exige a exceção injetada e o log específico. Retirar apenas o `raise` atual deixa de satisfazer esse contrato. Confere classe, não identidade da exceção. (`app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:272`, `app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:274`, `app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:276`; `app/colaboradores/services/calendario.py:486`)

   **Limite:** os testes passam com os textos dos cards ainda escritos no template; não certificam origem única nem detectam o bloqueio descrito acima. O teste geral também procura a sentinela em qualquer lugar do HTML. (`app/colaboradores/tests/test_e4cal_l1_r7_grade_le_o_rotulo.py:35`, `app/colaboradores/tests/test_e4cal_l1_r7_grade_le_o_rotulo.py:41`, `app/colaboradores/tests/test_e4cal_l1_r7_grade_le_o_rotulo.py:49`)

5. **Aquecimento — não procede acusar ocultação de crescimento proporcional de queries.** Continua comparando 0, 1 e 8 dias sem fim na mesma janela de oito dias. Aquecer remove a assimetria inicial de um catálogo global cujo carregamento independe dessa quantidade. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:192`, `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:198`, `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:210`; `app/ponto/catalogo/ausencias.py:249`, `app/ponto/catalogo/ausencias.py:276`)

   Não mede tempo, CPU, primeira chamada fria nem crescimento da janela; “51 × 50” aparece somente no comentário. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:205`, `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:207`)

6. **Remoção de `regua_rotulo` — não procede apontar leitor direto quebrado.** O censo textual encontrou apenas comentários e asserções de ausência; os antigos consumidores usam rótulo recebido ou texto literal. Consumidores externos e chaves construídas dinamicamente: **[não verificado]**. (`app/colaboradores/services/calendario.py:264`; `app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:253`, `app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:262`; `app/templates/colaboradores/partials/_calendario_grade.html:103`, `app/templates/colaboradores/partials/_calendario_grade.html:161`)

7. **Item 7 — fila; triagem procede pelos anexos, com ressalva.** Ambos mostram a mesma divergência de atribuição do pareador. Porém, o RED do status não é idêntico: base apresenta `folga × folga_trabalhada`; atual, `folga × sem_certidao`. Vínculo dos logs ao commit `f60b7e0b`: **[não verificado]**.

   O mecanismo permanece na origem: mapa não vazio sem a véspera retorna `None`; mapa vazio usa `_bm_all`. A tela fornece carga mensal, enquanto o unitário abre carga diária. A divergência deve ser corrigida no pareador. (`app/ponto/turnos.py:1297`, `app/ponto/turnos.py:1300`; `app/colaboradores/services/calendario.py:573`; `app/ponto/status_dia.py:197`, `app/ponto/status_dia.py:249`)

Execução local dos testes e dos mutantes, GREEN atual e RED contra restaurações: **[não verificado]**. [nao verificado]

## 02/11 19:22 · sessao 63cc0a24b971 · - · - · origem=Ronald (tmux)

- **Item 5 — não procede, quanto aos defeitos específicos.** O render usa sentinelas distintas por status/layout; restaurar o tooltip antigo de `escala_suspeita` falharia no `title="SENTINELA"` exigido. Restaurar o card antigo falharia por ausência de `ROTULO_EM_ANDAMENTO` no título. (`app/colaboradores/tests/test_e4cal_l1_r7_grade_le_o_rotulo.py:29`, `:37`, `:44`; `app/templates/colaboradores/partials/_calendario_grade.html:103`, `:169`.)

- **Lavra — não procede.** A fixture confirma trabalho previsto, nenhuma batida e ausência de absolvição inicial; depois altera somente as colunas lavradas para `ok`, `irregular` e `ok` com sentinela. Exige resultados literais dessas colunas na tela e no D4: um leitor que volte a decidir pela grade ou reconstrua o rótulo falharia. (`app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:246`, `:250`, `:254`, `:259`; `app/ponto/status_dia.py:266`.)

- **Propagação A2 — não procede.** O teste injeta `Injetada` na função efetivamente importada pelo calendário, exige essa classe de exceção e o log específico; restaurar um `except` que engole a falha falharia. Limite: exige a classe, não identidade do objeto da exceção. (`app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:269`, `:272`, `:274`, `:276`; `app/colaboradores/services/calendario.py:468`, `:482`.)

- **Limites de cobertura — fila, se a promessa for universal.** O primeiro teste só procura a sentinela em qualquer lugar do HTML; as fixtures não exercitam `ausencia` nem `palavra_dia`, que precedem os ícones de status. Portanto não certifica todo caminho de apresentação. O teste específico de `escala_suspeita` prende a sentinela ao atributo `title`. (`app/colaboradores/tests/test_e4cal_l1_r7_grade_le_o_rotulo.py:22`, `:35`, `:41`; `app/templates/colaboradores/partials/_calendario_grade.html:97`, `:98`.)

- **Aquecimento — não procede como acusação de mascarar crescimento proporcional de consultas.** A janela permanece com oito dias; o teste mede `contexto_calendario` depois de trocar zero, um ou oito DNAs para sem fim, e continua exigindo igualdade. O catálogo tem cache global de 60 segundos e sua carga consulta tipos ativos independentemente dessa quantidade; aquecer torna as medições comparáveis. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:192`, `:198`, `:201`, `:210`; `app/ponto/catalogo/ausencias.py:249`, `:257`, `:275`.)

- **Limite do custo — fila somente para afirmação mais ampla.** O teste mede número de queries com cache aquecido, não tempo, CPU, quantidade de linhas, primeira chamada fria nem crescimento da própria janela. Os números “51 × 50” aparecem em comentário; execução que os confirme não foi fornecida. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:192`, `:205`, `:207`; medição independente: **[não verificado]**.)

Execução local dos testes e dos mutantes: **[não verificado]**. [nao verificado]

## 02/11 19:21 · sessao f2f080b25b73 · - · - · origem=Ronald (tmux)

- **Item 1, tooltip — não procede manter o bloqueio:** o ícone `escala_suspeita` lê diretamente `dia.status_rotulo`; o calendário repassa `_st['rotulo']` do D4, sem compor frase nem consultar régua ativa. (`app/templates/colaboradores/partials/_calendario_grade.html:103`; `app/colaboradores/services/calendario.py:654`; `app/colaboradores/services/calendario.py:719`)

- **Item 2, significado de Abertos — não procede manter o bloqueio funcional:** o contador conta todo `status == 'aberto'`; o D4 distingue turno vivo e dia com batidas fechadas ainda em andamento; o novo `title` menciona ambos. (`app/colaboradores/services/calendario.py:780`; `app/ponto/status_dia.py:259`; `app/ponto/status_dia.py:263`; `app/templates/colaboradores/partials/_calendario_grade.html:169`) **Ressalva de origem:** o template copia literalmente os dois rótulos; não lê `ROTULO_STATUS` nem `ROTULO_EM_ANDAMENTO`. É duplicação editorial, sem novo predicado de classificação. (`app/ponto/status_dia.py:40`; `app/ponto/status_dia.py:56`; `app/templates/colaboradores/partials/_calendario_grade.html:169`)

**Quem escreve e lê:**

| Objeto | Escritor/origem | Leitor |
|---|---|---|
| `CelulaDia.status_rotulo` persistido | `_status_alvo` consulta `status_da_lavra`/`status_do_admin`; `_aplicar_status` atribui o campo; `lavrar_veredito` grava pela porta. (`app/ponto/portas/celula.py:693`; `app/ponto/portas/celula.py:705`; `app/ponto/portas/celula.py:773`) | D4 lê coluna, com fallback ao vocabulário quando vazia. (`app/ponto/status_dia.py:266`) |
| `dia['status_rotulo']` no calendário | Contexto copia o rótulo do D4. (`app/colaboradores/services/calendario.py:719`) | Ícones da grade. (`app/templates/colaboradores/partials/_calendario_grade.html:99`; `app/templates/colaboradores/partials/_calendario_grade.html:107`) |
| `ROTULO_EM_ANDAMENTO` | Constante no núcleo. (`app/ponto/status_dia.py:56`) | D4 usa no ramo com batida e dia ainda em andamento; teste importa e compara ao título. (`app/ponto/status_dia.py:263`; `app/colaboradores/tests/test_e4cal_l1_r7_grade_le_o_rotulo.py:16`; `app/colaboradores/tests/test_e4cal_l1_r7_grade_le_o_rotulo.py:50`) |
| Texto do card `escala_suspeita` | Literal do próprio template. (`app/templates/colaboradores/partials/_calendario_grade.html:161`) | Navegador recebe esse `title`; template lê somente a contagem do resumo. (`app/templates/colaboradores/partials/_calendario_grade.html:161`; `app/templates/colaboradores/partials/_calendario_grade.html:162`) |

- **Remoção de `regua_rotulo` — não procede apontar leitor direto quebrado:** censo textual amplo encontrou somente comentário histórico e testes de ausência; nenhuma leitura executável de `regua_rotulo` em template, JS, PDF ou API. Os antigos locais agora leem `status_rotulo` ou texto literal. (`app/colaboradores/services/calendario.py:264`; `app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:253`; `app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:262`; `app/templates/colaboradores/partials/_calendario_grade.html:103`; `app/templates/colaboradores/partials/_calendario_grade.html:161`) Consumidor externo à cópia ou chave montada dinamicamente: **[não verificado]**.

- **Fila editorial preexistente:** o card `escala_suspeita` ainda afirma “Sem batida em dia previsto”; contudo, `status_da_lavra` retorna esse status para `veredito == 'indefinida'` antes de testar `tem_batida`, portanto a frase não descreve universalmente o domínio do status. (`app/templates/colaboradores/partials/_calendario_grade.html:161`; `app/ponto/status_dia.py:122`; `app/ponto/status_dia.py:126`) A frase já existia no delta fornecido; o delta apenas remove sua interpolação da régua. (`anexo delta.txt`, sem numeração: **[não verificado]** quanto à linhagem em Git)

- **Outro homônimo, fora do calendário:** `escala/views.py` escreve um `status_rotulo` de previsão usando `ROTULO_STATUS`; `minha_escala.html` o lê. Não é escrita da coluna persistida nem novo derivador introduzido no delta do calendário. (`app/escala/views.py:49`; `app/escala/views.py:54`; `app/escala/views.py:60`; `app/templates/escala/minha_escala.html:75`; `app/templates/escala/minha_escala.html:84`)

## 02/11 19:21 · sessao 7a6298e1627e · - · - · origem=Ronald (tmux)

**Item 7 — fila; triagem procede quanto à preexistência da divergência de atribuição, com limite de evidência.** Os anexos atual e base mostram a mesma falha do pareador: `[] != [(2026-09-09, False, True)]`; vínculo da execução ao commit `f60b7e0b` não pôde ser auditado nesta cópia, sem histórico Git disponível. [não verificado]

O mecanismo permanece: `_vespera` decide pelo mapa inteiro ser vazio/não vazio; mapa não vazio sem a véspera retorna `None`, enquanto mapa vazio usa `_bm_all`. Isso permite que DNA de outro dia altere o tratamento da mesma saída. `app/ponto/turnos.py:1297`, `app/ponto/turnos.py:1298`, `app/ponto/turnos.py:1299`, `app/ponto/turnos.py:1300`.

A alimentação também depende da janela: `marcos_dna_periodo` filtra as células entre `ini` e `fim`; `CargaDaJanela` entrega esse mapa convertido para `None` se vazio. `app/escala/utils.py:97`, `app/escala/utils.py:105`, `app/ponto/turnos.py:2993`.

**Não procede chamar o RED de D4 de resultado idêntico.** No anexo base, a diferença é `folga × folga_trabalhada`; no atual, `folga × sem_certidao`. A divergência existe em ambos, mas a palavra exibida mudou. [não verificado — anexos fornecidos]

O resultado atual é coerente com o código: com batida atribuída ao dia encerrado e sem lavra, D4 retorna `sem_certidao`; sem batida, a faixa `folga` retorna `folga`. `app/ponto/status_dia.py:271`, `app/ponto/status_dia.py:273`, `app/ponto/status_dia.py:291`.

A tela usa a carga da janela inteira, enquanto a porta unitária constrói carga `[data,data]`; portanto a paridade continua pendente na origem do pareamento, sem necessidade de criar correção no leitor. `app/colaboradores/services/calendario.py:573`, `app/ponto/status_dia.py:197`, `app/ponto/status_dia.py:249`.

## 02/11 18:27 · sessao ae4ec323f2f3 · DEPLOY-[nome]-ELO-DA-CORRENTE · leitura · origem=Code

A cópia ainda faz **“teto → aviso → restart”**, e o agendador **não repete automaticamente uma tentativa recusada**. São duas divergências diretas do contrato proposto. (`bin/deploy.sh:274–279`; `bin/deploy_agendado.sh:68–82`)

1. **Quem escreve, lê e quais execuções escapam da marca**

   `cron_run.sh` recebe nome/empresa/comando, cria `${N}_${E}.$$`, grava a data ISO e instala limpeza por `trap EXIT`. O PID é do envelope no host; a marca não identifica o container. (`bin/cron_run.sh:5–11`)

   O leitor operacional é `deploy.sh::em_curso()`: extrai o PID do nome, consulta `/proc/<pid>`, devolve nome/empresa quando encontra o processo e remove a marca quando não encontra. Não lê a data gravada no conteúdo. (`bin/deploy.sh:271–272`)

   Todos os comandos **C e RAW renderizados** passam por `_run()` → `cron_run.sh`; a execução final de cada elo também passa pelo runner. (`app/config/crons.py:1423`; `app/config/crons.py:1431–1434`; `app/config/crons.py:1789–1802`; `bin/encadeado.sh:93`)

   **Há furo comprovado:** `--corrente` é anexado depois do envelope, separado por `;`, e executa `docker exec saas_core ... crons corrente` antes de qualquer marca própria. A espera pelos carimbos também fica fora da marca; o comando final do elo recebe marca somente na linha 93. (`app/config/crons.py:1812–1813`; `bin/encadeado.sh:42`; `bin/encadeado.sh:77–93`)

   Existe ainda agendamento fora desse render: `deploy_agendado.sh` gera uma entrada em `/etc/cron.d` sem runner. A igualdade entre crontab instalado e configuração nesta produção é **[nao verificado]**. (`bin/deploy_agendado.sh:54–56`; mecanismo de comparação em `bin/crons.sh:17–22`)

2. **Como distinguir host de casca**

   Não há campo explícito `host/container/alvos` nos esquemas C, RAW ou ENC. `estagio`, `cwd` e `depende` não fornecem essa classificação. (`app/config/crons.py:29–33`; `app/config/crons.py:53–54`; `app/config/crons.py:102–109`)

   C usa `MANAGE`, definido como `docker exec saas_core`; os elos também lançam seu comando em `saas_core`. Para RAW, é necessário seguir o script chamado. (`app/config/crons.py:21`; `app/config/crons.py:1798`; `bin/encadeado.sh:93`; `app/config/crons.py:1791`)

   **RAW não significa “fora das cascas”:** a sentinela é RAW e executa em `mensageria`; `lavrar_abriu_nao_bateu.sh` coleta logs no host e executa uma etapa em `saas_core`. (`app/config/crons.py:669`; `mensageria/bin/sentinela_diario.sh:14–16`; `bin/lavrar_abriu_nao_bateu.sh:19–24`)

   **A sombra é híbrida:** o script parte do host, executa o ensaio em container separado por `docker run`, mas também chama `docker exec saas_core` para ler a lavra. Portanto, excluir a sombra inteira da proteção exige esclarecer o tratamento dessa fase interna; classificá-la apenas pelo nome ou por RAW deixa essa exposição. (`app/config/crons.py:331–333`; `bin/sombra.sh:138–144`; `bin/sombra.sh:182–191`)

3. **Marca órfã e espera eterna**

   Hoje há duas limpezas: o runner remove sua marca no `EXIT`; o deploy remove marcas cujo PID não existe em `/proc`. A limpeza de órfãs já está dentro da consulta usada pelo deploy, sem cron separado. (`bin/cron_run.sh:10`; `bin/deploy.sh:271–274`)

   A identidade é fraca: o deploy verifica somente a existência do PID, sem conferir início ou comando do processo. Por inferência, um PID reutilizado pode parecer cron vivo; também não há comprovação de que a morte do envelope encerrou todos os seus filhos. (`bin/deploy.sh:272`; `bin/cron_run.sh:9–11`)

   **Desenho proposto:** preservar a limpeza de órfãs confirmadas, fortalecer a identidade e, diante de execução ainda viva ou identidade inconclusiva, atingir um teto e **recusar sem restart**, nomeando o impedimento. Isso evita espera eterna sem transformar idade da marca em autorização para matar trabalho. Implementação desse comportamento: **[nao verificado]**; hoje o teto permite restart. (`bin/deploy.sh:274–279`)

4. **Corrida e ponto para impedir novo elo**

   A corrida permanece aberta: a última consulta está na linha 278 e o restart na 279, sem exclusão compartilhada com o nascimento da marca. Um elo pode alcançar o runner nesse intervalo. (`bin/deploy.sh:278–279`; `bin/encadeado.sh:93`; `bin/cron_run.sh:9–11`)

   Há uma **porta comum para a execução dos elos**: `cron_run.sh`, antes de registrar a marca e lançar o comando. Os C e RAW renderizados também passam ali. A trava existente em `encadeado.sh` é por comando (`/tmp/encadeado_$CMD.lock`), para deduplicar elos; o deploy não participa dela. (`bin/cron_run.sh:9–11`; `app/config/crons.py:1431–1434`; `bin/encadeado.sh:71–75`; `bin/deploy.sh:271–279`)

   **Proposta, sem cron novo:** admissão sincronizada nessa porta; o runner registra a execução sob a mesma exclusão usada pelo deploy. O deploy fecha novas admissões, espera as marcas existentes, reconsulta e mantém a exclusão até concluir o restart ou recusar. Um arquivo consultado sem exclusão atômica conservaria a corrida. Esse protocolo ainda é **[nao verificado]** na cópia. (`bin/cron_run.sh:9–11`; `bin/deploy.sh:271–279`)

   A porta precisa também abranger a descoberta hoje desmarcada em `encadeado.sh:42`. Não convém bloquear globalmente antes da espera pelos gatilhos, pois ela depende dos carimbos das outras empresas. (`bin/encadeado.sh:42`; `bin/encadeado.sh:77–90`)

5. **Outros consumidores e selos**

   No código encontrado, `ESPERA_CRON_S` é lido somente pelo ramo de restart do deploy, com padrão 600 e incrementos de cinco segundos. `deploy_agendado` consome o resultado da esteira, sem ler essa variável. (`bin/deploy.sh:274–278`; `bin/deploy_agendado.sh:73–77`)

   O selo específico atual apenas procura os textos `DEPLOY-ESPERA-CRON` e `crons_em_curso`; não demonstra recusa, filtragem host/casca ou ausência de restart. Ele é consumido pelo placar de código. (`bin/contrato_deploy.sh:47–49`; `bin/placar_code.sh:130–132`)

   Os outros testes preservam contratos distintos: conflitos/URLconfs, janela de autenticação, histórico após provas e ordem/teto dos carimbos encadeados. Esses trechos não exercitam a proteção contra restart com cron vivo. (`bin/tests/test_deploy_recusa_conflito.sh:52–60`; `bin/tests/test_janela_auth.sh:123–144`; `bin/tests/test_orcamento_contexto.sh:263–333`; `bin/tests/test_furo_encadeado_ao_cartorio.sh:24–68`)

   O molde de cadeia exige `deploy: OK` e sai com falha quando não encontra esse resultado; não contém repetição nesse trecho. Resultado de execução dos selos: **[nao verificado]**. (`bin/molde_fatia/cadeia.sh:89–90`)

6. **Furos restantes no contrato**

   **Quem chama/repete:** a promessa “recusa vira nova tentativa pelo agendador” não corresponde à implementação. Ele executa cada esteira uma vez, registra seu `rc`, gera `.done` e remove agenda e cron independentemente desse resultado. O `echo` final ainda pode fazer `rodar` terminar com sucesso após uma esteira falhar. (`contrato.txt:5`, anexo; `bin/deploy_agendado.sh:68–82`)

   **Quem espera:** “todo cron com marca” não cobre a descoberta da corrente; e excluir “cron do host” exige tratar scripts híbridos, inclusive a sombra. (`bin/encadeado.sh:42`; `bin/sombra.sh:138–144`; `mensageria/bin/sentinela_diario.sh:14–16`)

   **Falha/limite:** o contrato exige teto absoluto declarado; o número ainda está delegado ao construtor. Os 764 s do incidente não estabelecem duração máxima. Na fonte, `_espera` conta intervalos de sono, e o vencimento apenas avisa. (`contrato.txt:1` e `contrato.txt:3`, anexo; `bin/deploy.sh:273–279`)

   **Marca obrigatória:** falhas de `mkdir` ou gravação da marca não impedem explicitamente o lançamento do comando. Além disso, o runner usa caminho absoluto e o leitor usa `$RAIZ`; uma execução noutra raiz pode consultar diretórios diferentes. (`bin/cron_run.sh:9–11`; `bin/deploy.sh:45`; `bin/deploy.sh:271`)

   **Medida:** `crons_quebrados` considera a última corrida por nome/empresa, em 26 horas, e qualquer saída diferente de 0/2; não mede “137 no minuto de deploy”. Recuperação posterior pode retirar a morte desse contador. (`app/core/services/crons_quebrados.py:16–17`; `app/core/services/crons_quebrados.py:33–47`)

   O placar contém início/fim/duração/exit, mas o histórico do deploy é gravado após as provas, não no instante do restart. Falta definir a correlação temporal e o tratamento de registros ausentes; o contador atual retorna lista vazia quando não consegue abrir o placar. Valor da medida exigida e eficácia posterior à cura: **[nao verificado]**. (`bin/cron_run.sh:13`; `bin/deploy.sh:279`; `bin/deploy.sh:342–347`; `app/core/services/crons_quebrados.py:29–32`)

   **Quem desliga/modos:** `--reload-agendado` e `--reload-copia` usam HUP e não atravessam essa espera. O contrato deve delimitar a proteção ao restart destrutivo e exigir liberação automática da admissão ao terminar ou recusar, sem flag de bypass. O mutante “avisa e reinicia” precisa reprovar no selo novo; hoje ele corresponde ao comportamento da cópia. (`bin/deploy.sh:54–60`; `bin/deploy.sh:237–279`; `bin/contrato_deploy.sh:47–49`; `contrato.txt:5`, anexo)

## 02/11 18:25 · sessao bf48b9348077 · - · - · origem=Ronald (tmux)

- **Escritor, formato, conteúdo:** `bin/cron_run.sh:5` recebe nome/empresa/comando; `:9` monta `logs/crons_em_curso/${N}_${E}.$$`, com PID do envelope do host; `:10` grava instante ISO e instala `trap ... EXIT`; `:11` executa o comando. A marca não guarda alvo/container nem identidade adicional do processo (`bin/cron_run.sh:7-11`).
- **Leitor operacional e limpeza:** `bin/deploy.sh:271-272` enumera marcas, extrai o sufixo após o último ponto, considera vivo se `/proc/<pid>` existe, imprime nome/empresa e remove marca cujo PID não existe; `:274-278` usa essa lista para esperar e avisar. O conteúdo ISO da marca não é lido por essa função (`bin/deploy.sh:271-272`). O outro consumidor encontrado é estático: selo procura texto `crons_em_curso` no deploy e runner (`bin/contrato_deploy.sh:47-49`).
- **Cobertura normal:** tanto RAW quanto C renderizados passam `_run()` (`app/config/crons.py:1789-1802`), que chama runner (`:1423`, `:1431-1434`); os comandos finais dos elos também passam runner (`bin/encadeado.sh:93`). O crontab instalado de produção e sua igualdade com o render atual são **[nao verificado]**; há mecanismo para comparar, não evidência dessa execução (`bin/crons.sh:17-22`).
- **Furo sem marca comprovado:** o disparo `encadeado.sh --corrente` é anexado **após** o envelope, com `;` (`app/config/crons.py:1812-1813`). Esse caminho chama `docker exec saas_core ... crons corrente` sem runner (`bin/encadeado.sh:37-49`); portanto existe comando de casca da infraestrutura do cron sem marca. A espera de carimbos também fica sem marca e só entra no runner ao lançar efetivamente o elo (`bin/encadeado.sh:71-93`). Não significa que os management commands finais dos elos estejam sem marca (`bin/encadeado.sh:93`).
- **Fora do render:** `deploy_agendado.sh` escreve uma entrada `/etc/cron.d` que chama diretamente seu modo `rodar`, sem runner (`bin/deploy_agendado.sh:54-56`); este executa `esteira.sh` fornecido pelo chamador (`:68-74`). Quais comandos de casca essas esteiras executam: **[nao verificado]**.

- **Distinção por fonte:** C usa `MANAGE = docker exec saas_core ...` (`app/config/crons.py:21`, `:1798`); ENC lança `docker exec saas_core` (`bin/encadeado.sh:93`). Os esquemas C, RAW e ENC não declaram campo host/container/alvos (`app/config/crons.py:29-33`, `:53-54`, `:102-109`).
- **RAW não equivale a “fora das cascas”:** sentinela é RAW (`app/config/crons.py:669`) e executa dentro de `mensageria` (`mensageria/bin/sentinela_diario.sh:14-16`); negou_fogo também (`app/config/crons.py:686-687`; `mensageria/bin/negou_fogo_diario.sh:10-11`). `lavrar_abriu_nao_bateu.sh` mistura coleta no host e comando em `saas_core` (`bin/lavrar_abriu_nao_bateu.sh:19-24`).
- **Sombra é híbrida:** seu agendamento RAW chama script do host (`app/config/crons.py:331-333`), que lança container de ensaio separado por `docker run` (`bin/sombra.sh:182-191`), mas também executa uma fase de leitura de lavra dentro de `saas_core` (`bin/sombra.sh:138-144`). Para distinguir proteção efetiva pela fonte atual, é necessário seguir comando e scripts/fases; não basta RAW/C nem nome (`app/config/crons.py:102-109`, `:1789-1802`; `bin/sombra.sh:138-144`, `:182-191`).

- **Órfã:** limpeza normal é `trap EXIT` do runner (`bin/cron_run.sh:10`); limpeza de PID ausente é feita pelo deploy ao enumerar (`bin/deploy.sh:271-272`). Não há validação de identidade além da existência de `/proc/<pid>` nessa leitura, portanto reutilização de PID pode ser confundida com cron vivo — inferência direta do teste (`bin/deploy.sh:272`).
- **Tratamento proposto:** preservar a eliminação/ignorância de marca comprovadamente órfã; para marca considerada viva, a recusa com teto absoluto evita espera eterna. Validação mais forte de identidade exigiria desenho adicional: a marca atual só contém data e PID no nome (`bin/cron_run.sh:9-10`); implementação dessa cura **[nao verificado]**.

- **Ponto comum possível:** o runner é o ponto comum antes do lançamento efetivo dos C, RAW e elos (`app/config/crons.py:1431-1434`, `:1789-1802`; `bin/encadeado.sh:93`). Hoje não há coordenação com deploy nessa sequência marca→comando (`bin/cron_run.sh:9-11`; `bin/deploy.sh:271-279`). Um protocolo de exclusão compartilhado runner/deploy pode impedir novo lançamento enquanto deploy drena/reinicia; arquivo consultado sem exclusão atômica manteria a corrida — proposta, **[nao verificado]**.
- **Limites dessa proposta:** o `flock` atual é por elo (`/tmp/encadeado_$CMD.lock`), serve para deduplicar empresas e não é consultado pelo deploy (`bin/encadeado.sh:71-75`; `bin/deploy.sh:271-279`). Também seria necessário cobrir o `docker exec` de descoberta anterior ao runner (`bin/encadeado.sh:42`).

## 02/11 18:25 · sessao 8161155c5547 · - · - · origem=Ronald (tmux)

**Corrida e ponto de admissão**

- A corrida existe na fonte: deploy lê marcas em `em_curso()`, termina a espera e faz uma última leitura em `bin/deploy.sh:271-278`; o restart seguinte não está protegido por trava (`bin/deploy.sh:279`). Nesse intervalo, um elo pode terminar a espera pelos carimbos e entrar no runner (`bin/encadeado.sh:77-93`).
- A corrente executa cada elo sequencialmente, mas sem marca própria abrangendo toda a corrente (`bin/encadeado.sh:42`, `bin/encadeado.sh:53-60`). A marca de cada execução nasce apenas ao entrar em `cron_run.sh` (`bin/encadeado.sh:93`, `bin/cron_run.sh:9-11`).
- A trava existente é `/tmp/encadeado_$CMD.lock`, por comando, para impedir duplicata de elo; deploy não participa dela (`bin/encadeado.sh:71-72`, `bin/deploy.sh:271-279`).
- Há uma porta comum para **execução dos elos**: `bin/encadeado.sh:93` → `bin/cron_run.sh:9-11`. As linhas C e RAW geradas também usam esse runner (`app/config/crons.py:1423`, `app/config/crons.py:1431-1434`, `app/config/crons.py:1789-1802`).
- **Proposta, ainda não implementada:** protocolo de admissão nessa porta, antes de criar a marca e lançar o comando; deploy fecha a admissão sob a mesma trava, drena as execuções já marcadas, faz a rechecagem e mantém a admissão fechada até terminar o restart ou recusar. O ponto de inserção é `bin/cron_run.sh:9-11`; o trecho a proteger é `bin/deploy.sh:271-279`. Flag lida sem exclusão mútua não fecha a corrida; eficácia de implementação futura [nao verificado].
- Não colocar a admissão global antes da espera por gatilho: ali o elo depende de outras empresas chegarem aos carimbos (`bin/encadeado.sh:77-90`). O bloqueio deve atingir o nascimento da execução em `bin/encadeado.sh:93`.
- Limite dessa porta: o render da corrente já executa `docker exec saas_core` antes do runner (`bin/encadeado.sh:42`); logo “toda atividade dentro da casca passa pelo runner” não é verdade na fonte (`bin/encadeado.sh:42`, `bin/encadeado.sh:93`).

**Furos do contrato comprovados**

- A promessa de “recusa vira nova tentativa pelo deploy_agendado” não corresponde ao script: ele executa a esteira uma vez e registra seu rc (`bin/deploy_agendado.sh:73-77`); depois gera `.done` e remove agenda e cron independentemente desse rc (`bin/deploy_agendado.sh:79-82`). Há ainda risco de o `rodar` acabar com rc 0 pelo `echo` final mesmo após esteira falhar (`bin/deploy_agendado.sh:74`, `bin/deploy_agendado.sh:82`).
- O molde de cadeia reconhece falha de deploy e sai 1, mas não faz nova tentativa (`bin/molde_fatia/cadeia.sh:89-90`).
- O cron de higiene usa HUP, não restart, e não passa pela espera de marcas: ramificação `AGENDADO` em `bin/deploy.sh:237-265`; espera apenas no `else`, `bin/deploy.sh:266-279`. As flags que selecionam esse ramo estão em `bin/deploy.sh:54` e `bin/deploy.sh:60`.
- O selo atual do deploy só procura strings relacionadas à espera e à marca, sem demonstrar recusa no teto ou exclusão da corrida (`bin/contrato_deploy.sh:47-50`).
- A classificação RAW não significa execução exclusivamente fora das cascas: `lavrar_abriu_nao_bateu.sh` é RAW (`app/config/crons.py:271`), mas sua pipeline executa comando dentro de `saas_core` (`bin/lavrar_abriu_nao_bateu.sh:19-24`). Logo classificar todo RAW como host abriria furo (`app/config/crons.py:271`, `bin/lavrar_abriu_nao_bateu.sh:23-24`).
- O selo antigo do encadeado usa `CRON_RUN_FAKE=1` (`bin/tests/test_furo_encadeado_ao_cartorio.sh:55`), porém o runner lido não contém ramo para essa variável e executa `bash -c "$CMD"` (`bin/cron_run.sh:5-11`). Esse selo não deve ser tomado como prova de isolamento sem conferência adicional [nao verificado].

## 02/11 18:25 · sessao 6079d3f52882 · - · - · origem=Ronald (tmux)

Achados para consolidação, todos por leitura de fonte; execução dos selos [nao verificado].

1. **`deploy_agendado` não oferece a nova tentativa prometida no contrato.** Executa cada `esteira.sh` uma vez, captura e registra `rc`, segue o laço, copia resultado para `.done`, remove agenda e remove `/etc/cron.d/...`, independentemente do `rc` da fatia. Não há laço de repetição nessa implementação. `bin/deploy_agendado.sh:68-81`. Portanto, “recusa vira nova tentativa pelo deploy_agendado” é um furo concreto do contrato contra a cópia. `bin/deploy_agendado.sh:73-81`.

2. **Consumidor de `ESPERA_CRON_S`: somente o ramo de restart de `deploy.sh` no código encontrado.** Usa default 600, espera em passos de cinco segundos, imprime aviso e reinicia mesmo se o cron permanece vivo. `bin/deploy.sh:274-279`. O agendador não lê essa variável: chama uma esteira e lê seu `rc`. `bin/deploy_agendado.sh:73-77`. Há referências documentais em `app/docs/RELATO.md:3673`, `:3676`, `:3907`, `:4336`; seus relatos de produção adicionais não devem ser usados para ampliar o dado raspado autorizado.

3. **O selo de deploy atual é fraco para este defeito.** `contrato_deploy.sh` apenas exige os textos `DEPLOY-ESPERA-CRON` e `crons_em_curso`; não prova espera, recusa, filtragem host/casca nem ausência de restart com marca viva. `bin/contrato_deploy.sh:47-49`. O mutante “teto e reinicia” já corresponde ao comportamento da cópia. `bin/deploy.sh:274-279`. Esse contrato é consumido pelo placar de código. `bin/placar_code.sh:130-132`.

4. **Outros selos que leem deploy têm contratos diferentes.** `test_deploy_recusa_conflito.sh` cobra chamada à guarda de conflitos e importação dos URLconfs. `bin/tests/test_deploy_recusa_conflito.sh:52-60`. `test_orcamento_contexto.sh` cobra histórico depois das provas, seus modos e que reprovação da prova de casca continua sendo porta; seus mutantes não exercitam cron vivo/restart. `bin/tests/test_orcamento_contexto.sh:263-333`. `test_janela_auth.sh` verifica a chamada da guarda; `bin/tests/test_janela_auth.sh:64-67`. Nenhum desses trechos é prova do novo contrato de cron vivo. `bin/contrato_deploy.sh:47-49`; `bin/tests/test_deploy_recusa_conflito.sh:52-60`; `bin/tests/test_orcamento_contexto.sh:263-333`.

5. **Há modos sem docker restart e sem essa espera.** `--reload-agendado` e `--reload-copia` definem `AGENDADO=1`; esse ramo usa HUP nas três cascas. `bin/deploy.sh:54-60`, `:237-251`. A espera por marca está no `else` do ramo. `bin/deploy.sh:266-279`. Ao especificar “todo pouso”, delimitar o contrato para a operação destrutiva de restart evita confundir HUP com restart. `bin/deploy.sh:237-279`. A configuração instalada do reload é conferida por `selo_reciclagem.sh`; não é verificada aqui no host de produção. `bin/selo_reciclagem.sh:70-75`; situação instalada [nao verificado].

6. **O contador existente não é a medida “rc137 no minuto de deploy”.** `crons_quebrados` considera a última corrida por `(nome, emp)`, nas últimas 26h, dentre as últimas 20 mil linhas, e conta qualquer exit diferente de 0/2. `app/core/services/crons_quebrados.py:16-17`, `:33-47`. O placar chama esse contador. `bin/placar_code.sh:451-455`. Assim, ele pode deixar de contar uma morte após uma recuperação e não correlaciona cron com deploy. `app/core/services/crons_quebrados.py:41-47`.

7. **Fontes para construir a medida existem, mas falta definir a correlação.** O runner registra `nome`, `emp`, `ini`, `fim`, `dur`, `exit`. `bin/cron_run.sh:13`. O histórico de deploy registra hora/head/0 somente depois das provas, não no instante do restart. `bin/deploy.sh:279`, `:342-347`. Definir log de restart, janela temporal, denominador e tratamento de ausência de registro são requisitos ainda não comprovados por um contador implementado; valor atual da medida pedida [nao verificado].

8. **O vigia acusa depois da morte; não preserva o elo.** O runner cria flag/pauta quando RC é diferente de 0/2 e recupera quando volta bem. `bin/cron_run.sh:29-35`. O carimbo de fim só nasce para RC 0/2. `bin/cron_run.sh:22-25`. A guarda nova precisa impedir a morte antes do restart; manter o vigia não satisfaz por si o contrato. `bin/deploy.sh:278-279`; `bin/cron_run.sh:22-35`.

9. **Selos mínimos a cobrar no desenho:** marca viva de casca deve impedir restart e terminar por fim do cron ou recusa; marca de host não deve segurar; órfã não deve travar; nascimento concorrente deve usar a mesma exclusão do restart; mutante “avisa e reinicia” deve reprovar. São critérios propostos a partir dos furos do ramo atual, não testes já existentes. `bin/deploy.sh:271-279`; existência/resultado desses novos selos [nao verificado].

## 02/11 18:24 · sessao b1c5436d1ca9 · E4-CALENDARIO-L1 · verificador · origem=Code

**Veredito: bloqueia.** A leitura direta do D4 entrou, mas a janela mensal pode produzir status diferente da consulta unitária; o template também conserva rótulos próprios. (`app/colaboradores/services/calendario.py:572`; `app/ponto/status_dia.py:249`; `app/ponto/turnos.py:1297`; `app/templates/colaboradores/partials/_calendario_grade.html:103`)

**1. Quem escreve e quem lê — não procede a acusação de novo escritor persistente no diff.**

| Campo/valor | Quem produz/escreve | Quem lê |
|---|---|---|
| `CelulaDia.status_dia` | Regra `status_da_lavra`; porta calcula o alvo, atribui e salva junto do veredito. (`app/ponto/status_dia.py:92`; `app/ponto/portas/celula.py:686`; `:705`; `:773`) | D4 lê a coluna; calendário copia a resposta; resumo conta esse status. O cartório também verifica se a coluna já existe. (`app/ponto/status_dia.py:266`; `app/colaboradores/services/calendario.py:653`; `:766`; `app/ponto/services/cartorio.py:882`) |
| `CelulaDia.status_rotulo` | Mesma porta e mesmo alvo do status. (`app/ponto/portas/celula.py:705`; `:773`) | D4 lê, com fallback para `ROTULO_STATUS`; calendário publica como `status_rotulo`. (`app/ponto/status_dia.py:268`; `app/colaboradores/services/calendario.py:718`) |
| Resposta transitória `status`/`rotulo` | D4 produz por `_lido` ou pela coluna lavrada; não persiste o andamento. (`app/ponto/status_dia.py:174`; `:260`; `:267`) | Calendário e painel consomem a resposta. (`app/colaboradores/services/calendario.py:653`; `:718`; `app/core/services/painel_op.py:338`) |
| `ROTULO_EM_CURSO` | Constante do núcleo. (`app/ponto/status_dia.py:61`) | D4 escolhe a frase quando o resultado já é `previsto` e `data <= hoje`; calendário apenas repassa. (`app/ponto/status_dia.py:298`; `app/colaboradores/services/calendario.py:718`) |
| `faixa` | A2 seleciona o vencedor e monta o dicionário. (`app/ponto/precedencia.py:393`; `:400`) | D4 lê para natureza; calendário fornece os fatos previamente carregados. (`app/ponto/status_dia.py:287`; `app/colaboradores/services/calendario.py:473`; `:572`) |
| `dia['ausencia']` | Cobertura seleciona a ausência; calendário monta o chip, lendo efeito e catálogo. (`app/ponto/turnos.py:2753`; `:2769`; `app/colaboradores/services/calendario.py:451`; `:663`) | Template dá prioridade ao chip. (`app/templates/colaboradores/partials/_calendario_grade.html:97`) |
| `CelulaDia.ausencia` | É outro campo: escrito pelas operações `carimbar` e `carimbar_insumos_em_lote`, dentro da porta. (`app/ponto/portas/celula.py:802`; `:838`; `:843`) | Não é a fonte do chip carregado pelo calendário. (`app/colaboradores/services/calendario.py:451`; `:650`) |
| `sem_fim` | Booleano local do D4; nenhuma persistência. (`app/ponto/status_dia.py:256`) | Ramos de andamento e previsão. (`app/ponto/status_dia.py:262`; `:295`) |
| `motivo` | Juíza temporal retorna `turno_aberto*`, `sem_previsto`, `relogio` ou `None`; carregador publica por data. (`app/ponto/turnos.py:2114`; `:2117`; `:2121`; `:2122`; `:2166`) | D4 lê e decide andamento/lavra/disponibilidade. (`app/ponto/status_dia.py:253`; `:255`; `:259`; `:266`) |

**2. Leitores restantes — fila; rótulo próprio no calendário — bloqueia.**

- **Bloqueia:** `escala_suspeita` ainda recebe tooltip escrito no template, ignorando `dia.status_rotulo` e usando a régua do vínculo ativo. O D4, por sua vez, devolve o rótulo lavrado. (`app/templates/colaboradores/partials/_calendario_grade.html:103`; `app/colaboradores/services/calendario.py:265`; `:730`; `app/ponto/status_dia.py:268`)
- **Bloqueia a fidelidade do rótulo:** o rodapé afirma que todo `aberto` “não teve saída”, embora o novo D4 produza `aberto` para par já fechado antes do fim previsto. (`app/templates/colaboradores/partials/_calendario_grade.html:169`; `app/ponto/status_dia.py:262`; `:263`; `app/ponto/tests/test_e4cal_l1_d4_natureza.py:156`)
- **Fila, já declarada:** espelho, app e furos continuam classificando o dia por regras próprias. (`app/ponto/services/espelho.py:152`; `:183`; `app/api/views.py:1570`; `:1627`; `app/relatorios/furos.py:32`; `:162`; `app/core/medida_leitor_do_dia.py:323`)
- **Fila:** PDF recebe os dias desse espelho e acrescenta frases próprias de isenção e turno aberto. (`app/relatorios/pdf_espelho.py:592`; `:919`; `:921`; `:958`)
- **Não procede como duplicação do status diário:** drawer e view chamam o calendário; vigia chama a porta de oferecer resolução; painel guarda a resposta D4. (`app/colaboradores/services/drawer.py:120`; `app/colaboradores/views.py:1945`; `app/chamados/services/vigia_fio.py:280`; `:289`; `app/core/services/painel_op.py:338`)
- **Não procede como julgamento de dia encerrado:** `escala/views.py` deriva previsão e seu rótulo, papel expressamente declarado. (`app/escala/views.py:49`; `:54`; `:60`; `app/core/medida_leitor_do_dia.py:300`)
- **Não procede confundir TXT com status diário:** `classificar_export` responde entrada no TXT e consulta retenção por outra autoridade. (`app/folha/export.py:746`; `:942`; `:1212`)
- **Fila documental:** os ícones ainda citam `calendario.ROTULO_STATUS` em comentários; isso não executa import. (`app/templates/core/_icone_dia_extra.html:6`; `app/templates/core/_icone_dia_previsto.html:6`; `app/templates/core/_icone_dia_folga.html:5`) Nenhum import executável de nome morto apareceu na busca textual; garantia por importação/AST completa: **[nao verificado]**.

**3. Passado, relógio e meia-noite.**

- **Não procede:** passado com `MOTIVO_SEM_PREVISTO` não entra em `sem_fim`; com coluna lavrada, lê a coluna; sem lavra e com batida, retorna `sem_certidao`. Os dois casos possuem testes específicos. (`app/ponto/status_dia.py:256`; `:266`; `:271`; `app/ponto/tests/test_e4cal_l1_d4_natureza.py:410`; `:417`)
- **Não procede proibir todo `aberto` passado:** ele continua possível quando a própria juíza informa `turno_aberto*`; esse ramo não decide pela data. (`app/ponto/status_dia.py:259`; `app/ponto/turnos.py:2114`)
- **Não procede nova leitura de relógio no caminho do status:** calendário captura `_agora`, injeta no D4, que deriva `hoje` desse instante e o repassa à juíza. `data <= hoje` escolhe apenas o rótulo de um status já determinado. (`app/colaboradores/services/calendario.py:134`; `:572`; `app/ponto/status_dia.py:231`; `:233`; `:253`; `:298`)
- **Fila preexistente:** existe outra leitura de relógio no calendário, para permissão de desfazer retratação. Ela não alimenta o D4. (`app/colaboradores/services/calendario.py:221`; `:225`; `:572`)
- **Não procede teto somente por data no encerramento:** a juíza compara `agora >= fim` e considera turno vivo; o defeito de meia-noite encontrado está na atribuição da órfã conforme a janela, detalhado no item 7. (`app/ponto/turnos.py:2114`; `:2119`; `:2122`; `:1297`)

**4. Classes de reprovação.**

| Classe | Resultado |
|---|---|
| Segundo juiz | **Ausente no classificador removido do calendário; presente como fila nos demais leitores declarados.** (`app/colaboradores/services/calendario.py:78`; `app/core/medida_leitor_do_dia.py:323`) |
| Segundo escritor fora da porta | **Não identificado no diff de produção; não procede.** As atribuições persistentes examinadas continuam na porta. (`app/ponto/portas/celula.py:705`; `:773`) |
| Relógio fora da borda | **Ausente como introdução no status; presente preexistente para desfazer, fila.** (`app/colaboradores/services/calendario.py:134`; `:221`; `:572`) |
| Derivação paralela de fonte única | **Presente: bloqueia no rótulo do template; fila nos outros leitores.** (`app/templates/colaboradores/partials/_calendario_grade.html:103`; `app/core/medida_leitor_do_dia.py:323`) |
| Selo sem mordida suficiente | **Presente, fila:** paridade examina dicionários, sem render; propagação de erro A2 só exercita caminho feliz. (`app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:202`; `app/colaboradores/tests/test_calendario_um_juiz.py:92`) |
| Ordem de trava | **[nao verificado]** |
| Pouso antes do pré-requisito | A exigência está documentada, e a cópia contém a alteração de isenção; sequência efetiva de pouso/apply: **[nao verificado]**. (`app/docs/RELATO.md:19`; `app/ponto/services/cartorio.py:520`) |

**5. Testes apagados e asserções reescritas.** [nao verificado]

- **Fila:** perdeu-se a fixture adversária explícita “lavra absolve enquanto grade/motor acusam”, dos testes apagados. Os novos casos usam `ok` com batidas exatas e `irregular` sem batida; falta acrescentar essa oposição em `CalendarioLeODoD4Test`, alternando somente a coluna lavrada. (`app/colaboradores/tests/test_calendario_ata_decide.py:23`, arquivo apagado no diff fornecido; `app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:117`; `:126`)
- **Não procede perda da regra de isenção:** há cobertura funcional na regra, na porta e na tela, incluindo o rótulo; o teste apagado verificava apenas texto-fonte. (`app/ponto/tests/test_e3_status_lavrado.py:62`; `:220`; `app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:148`; `:192`; `app/colaboradores/tests/test_calendario_isencao.py:8`, arquivo apagado no diff)
- **Não procede vacuidade total do teste retargetado:** ele diferencia efetivamente `ok` de `irregular` e exige `None != ok`. O caso chamado “missing” agora injeta diretamente `irregular`; deixou de provar a antiga relação grade-missing → status, retirada do leitor. (`app/colaboradores/tests/test_t4_lei_status_none_alerta.py:114`; `:123`; `:129`; `:138`; `app/colaboradores/services/calendario.py:366`)
- **Fila:** falta injetar falha de `fatos_do_periodo` e exigir propagação; `assertNoLogs` no caminho feliz não protege o novo `raise`. (`app/colaboradores/tests/test_calendario_um_juiz.py:92`; `app/colaboradores/services/calendario.py:485`)
- **Bloqueia:** P14/P16 precisa ganhar o caso do item 7; também falta teste de render que detecte o tooltip próprio. Os testes atuais comparam apenas `status`/`status_rotulo` do contexto. (`app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:202`; `:211`; `app/templates/colaboradores/partials/_calendario_grade.html:103`)

**6. Baixa feriado/prazo — não procede a reprovação.**

A impressão removida pertencia ao classificador apagado, e o teste nomeado existe: distingue `trabalha_em_feriado=False → folga` de `True → previsto`, com cadastros separados e comparação dos resultados. A2 efetivamente conserva trabalho quando o feriado só afeta pagamento. (`app/core/juizes.py:456`; `app/colaboradores/services/calendario.py:78`; `app/ponto/tests/test_e4cal_l1_d4_natureza.py:323`; `:333`; `:343`; `app/ponto/precedencia.py:373`; `:378`) Execução verde desse teste: **[nao verificado]**.

**7. Contraexemplo além do golden — bloqueia.**

Caso sintético: vínculo 12x36 noturno 19–07, sem intervalo, âncora 08/09/2026; única batida próxima é `S` em 09/09 às 07h; sem células em 07–10/09; existe DNA 19–07 distante, em 02/09; sem ausência ou feriado em 09/09; instante posterior ao encerramento. A alternância faz 09/09 ser folga. (`app/escala/models.py:1260`; `app/ponto/precedencia.py:380`)

| Consulta | Caminho | Resultado |
|---|---|---|
| Calendário mensal | DNA distante torna o mapa não vazio; véspera ausente no mapa impede usar o template; órfã fica em 09/09. D4 vê batida e nenhuma lavra. (`app/escala/utils.py:103`; `app/ponto/turnos.py:1297`; `:285`; `app/ponto/status_dia.py:252`; `:271`) | **`sem_certidao`** |
| D4 unitário de 09/09 | Pad 07–10/09 não contém DNA; mapa vira `None`; template atribui a órfã a 08/09, filtrada fora da consulta. Sem batida em 09/09, natureza responde folga. (`app/ponto/turnos.py:1845`; `:2993`; `:1300`; `:295`; `:1993`; `app/ponto/status_dia.py:291`) | **`folga`** |

A sonda pura do pareador, executada em memória, confirmou as duas atribuições da mesma saída: **09/09 com mapa distante; 08/09 sem mapa**. A integração completa calendário/D4 e ocorrência desse cadastro em produção permanecem **[nao verificado]**. (`app/ponto/turnos.py:1297`; `:1300`; `:1494`)

## 02/11 18:22 · sessao 17d91a766eb9 · - · - · origem=Ronald (tmux)

**Bloqueia — janela muda o status do mesmo dia.** Contraexemplo estático: vínculo 12x36 noturno 19–07, sem intervalo, âncora 08/09; única batida na região: `S` em 09/09 às 07h; nenhuma célula em 07–10/09; célula com DNA 19–07 em 02/09; agora 20/09. A natureza de 09/09 é folga pela alternância (`app/escala/models.py:1260`).

- Calendário mensal: o DNA de 02/09 torna o mapa não vazio; como 08/09 falta nele, `_vespera` retorna `None`; a saída órfã fica em 09/09. D4 recebe `tem_batida=True` e devolve `sem_certidao` (`app/escala/utils.py:103`; `app/ponto/turnos.py:1297`; `app/ponto/turnos.py:285`; `app/ponto/turnos.py:1494`; `app/ponto/status_dia.py:252`; `app/ponto/status_dia.py:271`).
- D4 unitário de 09/09: pad 07–10/09 sem DNA faz o mapa virar `None`; `_vespera` usa template 19–07; a órfã é atribuída a 08/09 e descartada do resultado unitário. Sem batida em 09/09, D4 lê natureza folga e devolve `folga` (`app/ponto/turnos.py:1845`; `app/ponto/turnos.py:2993`; `app/ponto/turnos.py:1300`; `app/ponto/turnos.py:291`; `app/ponto/turnos.py:1990`; `app/ponto/status_dia.py:291`).
- É dedução dos ramos da cópia; reprodução executada e ocorrência em produção: **[nao verificado]**.

**Não procede — passado `sem_previsto` virar aberto por data.** O predicado exclui passado; nesse ramo, com lavra lê a coluna, sem lavra e com batida entrega `sem_certidao`. Há testes específicos para ambos (`app/ponto/status_dia.py:256`; `app/ponto/status_dia.py:266`; `app/ponto/status_dia.py:271`; `app/ponto/tests/test_e4cal_l1_d4_natureza.py:410`; `app/ponto/tests/test_e4cal_l1_d4_natureza.py:417`).

**Não procede — nova leitura temporal do status fora da borda.** Calendário captura `_agora`, passa-o ao D4, que obtém `hoje` desse instante e injeta o mesmo instante na juíza. O rótulo `data <= hoje` apenas escolhe a frase depois de `s == 'previsto'` (`app/colaboradores/services/calendario.py:134`; `app/colaboradores/services/calendario.py:572`; `app/ponto/status_dia.py:231`; `app/ponto/status_dia.py:253`; `app/ponto/status_dia.py:298`).

**Fila — segunda leitura de relógio preexistente no calendário.** `_agora_retr = timezone.now()` decide prazo de desfazer; não participa do status nem foi acrescentada pelo diff (`app/colaboradores/services/calendario.py:221`; `app/colaboradores/services/calendario.py:225`).

**Não procede — meia-noite tratada só por data no encerramento.** Marcos são encadeados em instantes, avançando dia quando necessário; a juíza compara `agora >= fim` e prioriza turno aberto vivo (`app/escala/servico_jornada.py:79`; `app/escala/servico_jornada.py:89`; `app/ponto/turnos.py:2114`; `app/ponto/turnos.py:2119`).

**Fila — custo de CPU não medido pelo teto de queries.** Para cada par, D4 recalcula `dias_encerrados` da carga inteira; esse método percorre todos os dias da janela. Com uma carga mensal, há repetição quadrática em dias, embora turnos e consultas estejam memoizados (`app/ponto/status_dia.py:248`; `app/ponto/status_dia.py:253`; `app/ponto/turnos.py:3004`; `app/ponto/turnos.py:2162`).

## 02/11 18:21 · sessao 4253b846026f · - · - · origem=Ronald (tmux)

- **Fila — três classificadores paralelos persistem**, já declarados: app próprio em `app/api/views.py:1570` e `app/api/views.py:1627`–`1653`; espelho próprio em `app/ponto/services/espelho.py:152`–`171`, gravado no dicionário em `:183`; furos por missing/veredito em `app/relatorios/furos.py:32`–`83`, usado em `:162`–`171`. O inventário restante os reconhece em `app/core/medida_leitor_do_dia.py:323`–`331`.

- **Bloqueia a alegação de rótulo único no render**: `_calendario_grade.html:103` mantém tooltip próprio para `escala_suspeita`, ignorando `dia.status_rotulo`; ele usa `regua_rotulo` do vínculo ativo (`app/colaboradores/services/calendario.py:265`, `:730`). D4 copia o rótulo lavrado (`app/ponto/status_dia.py:283`–`286`), portanto template e D4 podem imprimir frases diferentes. O medidor não vê templates (`app/core/medida_leitor_do_dia.py:338`).

- **Contraexemplo visual além do golden**: declaração parcial 13–17, jornada 08–12/13–17, sem batidas, resto cobrado, lavra `irregular`: o teste confirma esse status em `app/ponto/tests/test_cobparc_v4_procedentes.py:262`–`269`; calendário mantém `ausencia` no mesmo dicionário (`app/colaboradores/services/calendario.py:650`, `:657`–`667`, `:717`), e o template escolhe o chip de ausência antes de qualquer ramo de status (`app/templates/colaboradores/partials/_calendario_grade.html:97`–`100`). A confirmação por render executado é **[nao verificado]**. O teste confere só o dicionário (`app/ponto/tests/test_cobparc_v4_procedentes.py:259`–`260`).

- **Não procede — drawer, view e vigia como novos juízes de status**: drawer chama o calendário (`app/colaboradores/services/drawer.py:120`–`121`); view também (`app/colaboradores/views.py:1945`–`1946`); vigia importa e chama apenas a porta de `resolver_dia_oferecido` (`app/chamados/services/vigia_fio.py:280`, `:289`).

- **Não procede — painel recalcular status diário**: o painel guarda a resposta do lote diretamente em `linha['dia']` (`app/core/services/painel_op.py:334`–`338`). Seu campo `status` é presença, distinguida expressamente do status diário (`app/core/medida_leitor_do_dia.py:305`–`307`). Contudo, gate usa carga `[hoje-2,hoje]` e D4 é chamado sem essa carga (`app/core/services/painel_op.py:327`, `:334`, `:336`); paridade dessas janelas permanece **[nao verificado]**.

- **Não procede como novo juiz diário — escala/views**: ainda seleciona rótulo por trabalho/folga/indefinição (`app/escala/views.py:54`, `:60`), mas é tela de previsão, explicitamente declarada (`app/escala/views.py:49`–`53`; `app/core/medida_leitor_do_dia.py:300`–`302`). O relógio é `datetime.date.today()` em `app/escala/views.py:26`.

- **Fila — PDF herda a fonte antiga**: recebe os dias do espelho (`app/relatorios/pdf_espelho.py:592`–`598`), cujo classificador próprio permanece em `app/ponto/services/espelho.py:152`–`183`. Principal palavra impressa vem de `palavra_dia` (`app/relatorios/pdf_espelho.py:919`–`920`); agrega frases próprias de isenção (`:921`–`922`) e turno aberto (`:958`–`967`).

- **Não procede — confundir status do TXT com status diário**: `classificar_export` responde entrada no TXT (`app/folha/export.py:746`–`751`), lê retenção em `:942`–`944`; retenção responde por veredito/protestos/data (`:1212`–`1238`). Não lê as novas colunas de status nesses trechos (`app/folha/export.py:1188`, `:1212`–`1238`).

- **Fila documental — nomes mortos em comentários dos ícones**: `calendario.ROTULO_STATUS` permanece em `app/templates/core/_icone_dia_extra.html:6`, `_icone_dia_espuria.html:6`, `_icone_dia_previsto.html:6`, `_icone_dia_feriado.html:6`, `_icone_dia_folga.html:5`; são comentários, não imports executáveis. Os desenhos só contêm SVG (`app/templates/core/_icone_dia_previsto.html:7`–`9`; `_icone_dia_falta.html:8`–`14`). Nenhum import Python de nome morto foi encontrado na busca textual realizada **[nao verificado por AST/importação]**.

- **Fila temporal preexistente**: calendário captura o instante principal em `app/colaboradores/services/calendario.py:134`, mas lê outro relógio para permissão de desfazer retratação em `:221`–`225`. Esse segundo instante não alimenta o status D4, enviado com `_agora` em `:573`–`574`.

## 02/11 18:21 · sessao ae0f3405deb2 · - · - · origem=Ronald (tmux)

- **Não procede — baixa feriado/prazo.** A baixa nomeia a impressão eliminada e o teste correspondente em `app/core/juizes.py:456-460`. `D4NaturezaFeriadoTest` realmente distingue quem folga de quem trabalha no feriado: esperados em `app/ponto/tests/test_e4cal_l1_d4_natureza.py:322-324`, cadastro dos dois casos em `:329-335`, comparação com a regra em `:343-344`. Execução verde: [nao verificado].

- **Não procede — apagar teste de isenção deixou regra sem cobertura.** O teste apagado verificava texto-fonte; a propriedade funcional sobrevive na regra (`app/ponto/tests/test_e3_status_lavrado.py:62-66`), no escritor real (`:220-224`) e no calendário, incluindo o rótulo (`app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:148-150`, `:176`, `:192`). O novo teste da tabela exige a equivalência exata `ROTULO_ISENTO ⇔ status ok e isento`: `app/ponto/tests/test_e4cal_l1_tabela_da_lavra.py:84-94`.

- **Fila — perdeu-se fixture explicitamente adversária da soberania da ata perante grade/motor.** Os casos apagados `test_MORDE_ata_concorde_vence_a_grade_que_acusa` e `...vence_o_desvio_do_motor` estão no diff fornecido (antigo arquivo, linhas 23-27). Os substitutos de tela lavram `ok` com batidas exatas 07/19 e lavram `irregular` sem batidas: `app/colaboradores/tests/test_e4cal_l1_calendario_le_o_d4.py:117-126`. A paridade tela/D4 existe (`:202-217`), mas a fixture deveria acrescentar dia com coluna `ok`, pontas/atraso que acusam, e trocar apenas a coluna para `irregular`; assim morderia a volta da derivação do leitor. A regra pura permanece coberta para `concorde→ok` e `furo→irregular`: `app/ponto/tests/test_e3_status_lavrado.py:43-49`.

- **Fila — selo “sem engolir erro” não prova propagação da falha.** `app/colaboradores/tests/test_calendario_um_juiz.py:92-99` executa somente o caminho feliz com `assertNoLogs`; não injeta exceção A2. A nova obrigação é o `raise` em `app/colaboradores/services/calendario.py:485`; sua remoção não é observada por esse caso. Falta um caso que faça `fatos_do_periodo` falhar e exija a mesma exceção no calendário.

- **Não procede — retarget de `None` ficou totalmente vazio.** O teste reescrito altera efetivamente a coluna consumida (`app/colaboradores/tests/test_t4_lei_status_none_alerta.py:114-121`) e verifica que `ok` e `irregular` produzem respostas diferentes (`:123-127`), além de `None != ok` (`:129-132`). O nome `test_celula_missing_segue_irregular` tornou-se impreciso: agora injeta diretamente `irregular`, sem célula de grade missing (`:138-139`); a antiga propriedade de missing→irregular foi deliberadamente retirada do leitor, conforme `app/colaboradores/services/calendario.py:366-368`.

- **Não procede — L1 passado sem previsto ficou sem teste.** O teste confirma chegada real a `MOTIVO_SEM_PREVISTO` em `app/ponto/tests/test_e4cal_l1_d4_natureza.py:396-402`; depois exige passado sem lavra `sem_certidao` e passado lavrado `folga_trabalhada` em `:410-421`.

- **Ordem de trava / pouso antes de pré-requisito: [nao verificado].** O desenho descreve D5 em `app/docs/ADR/0002-celula-e-o-dia.md:69-71` e permite migração E4 um leitor por pouso em `:75-77`; isso não prova execução de backfill, suite, trava ou pouso. RED anterior à cura e GREEN dos bytes auditados: [nao verificado].

## 01/11 20:45 -- HISTORICO (mais de 24 h) · sessao 6bee4d54689e · COBERTURA-PARCIAL-UM-JUIZ · verificador · origem=Code

**Pré-verificação: bloqueada.** Há perda dos carimbos no legacy, emissores fora do predicado comum e perda do instante anterior em passadas do mesmo ato. Esses pontos impedem confirmar as intenções 1–3 nos caminhos completos (`app/escala/utils.py:1060`; `app/ponto/services/cluster_espurio.py:88`; `app/chamados/services/disputa_emissao.py:763`; `app/ponto/services/cartorio.py:914`).

A equivalência da cópia com o hash/base informados, a suite Django, o deploy, os hashes reais de produção e a incidência dos contracasos abaixo estão **[nao verificado]**. [nao verificado]

1. **BLOQUEIA — A/F/G: o legacy perde a identificação da ausência parcial e pode dispensar o resto indevidamente.**

   `_marcar_ausencias` escreve `cobertura_parcial` e `periodo_parcial`, mas o legacy remonta o resultado copiando apenas alguns campos: os dois carimbos ficam para trás, enquanto as células `dispensada` sobrevivem. O dispatcher continua oferecendo esse caminho quando a flag está falsa (`app/escala/utils.py:1247`; `app/escala/utils.py:1251`; `app/escala/utils.py:1058`; `app/escala/utils.py:1060`; `app/escala/utils.py:2106`).

   Contracaso: parcial 14–17, E13 ainda apagado e S17 dispensado. No resultado legacy falta `cobertura_parcial`; `dispensar_por_ausencia` interpreta essa ausência de chave como cobertura integral e acende também E13. A ata deixa de carregar os carimbos, `parcial_so_lista` retorna falso e `dia_sem_trabalho` volta a calar o dia inteiro (`app/ponto/services/cartorio.py:719`; `app/ponto/services/cartorio.py:726`; `app/ponto/services/cartorio.py:691`; `app/ponto/services/cartorio.py:368`; `app/chamados/juizes.py:424`).

   Os testes da volta 6 não mordem esse defeito: criam ausências com `minutos=0` e verificam férias, aviso, falta e rótulo da ata, sem testar parcial e preservação dos carimbos nas duas flags (`app/escala/tests/test_cobparc6_ausencia_sem_vinculo.py:55`; `app/escala/tests/test_cobparc6_ausencia_sem_vinculo.py:63`; `app/escala/tests/test_cobparc6_ausencia_sem_vinculo.py:83`; `app/escala/tests/test_cobparc6_ausencia_sem_vinculo.py:99`).

2. **BLOQUEIA — B/D/F: cluster nasce da célula e não lê `parcial_so_lista`.**

   O julgamento chama o reconciliador de cluster depois da lavra. Esse serviço lê a ata, passa por `dia_sem_trabalho`, procura duas órfãs próximas e alguma lâmpada apagada, e abre chamado diretamente. Não consulta o predicado comum (`app/ponto/services/cartorio.py:931`; `app/ponto/services/cluster_espurio.py:23`; `app/ponto/services/cluster_espurio.py:28`; `app/ponto/services/cluster_espurio.py:32`; `app/ponto/services/cluster_espurio.py:88`).

   Contracaso: colaborador ativo, vínculo ativo, ata de ausência parcial sem período, duas órfãs separadas por até 15 minutos e uma lâmpada apagada. A exceção nova faz `dia_sem_trabalho=False`; o cluster pode abrir chamado embora `parcial_so_lista=True`. A guarda global de absolvição não resolve isso: restringe-se a `MODULOS_DE_TURNO` (`app/ponto/services/cluster_espurio.py:77`; `app/ponto/services/cluster_espurio.py:79`; `app/chamados/juizes.py:424`; `app/ponto/services/cartorio.py:370`; `app/chamados/models.py:1061`).

   Isso viola a fronteira literal pedida — parcial sem período nunca vai a chamado — mesmo sendo um chamado de supervisão. Ocorrência nos quatro colaboradores apresentados: **[nao verificado]**.

3. **BLOQUEIA — B: o passivo também pergunta pelos marcos da ata sem aplicar a regra comum.**

   `pergunta_no_app.perguntar` encaminha o passivo para `criar_disputa_a_partir_de_chamados_orfaos`. Essa função abre o pai antes de consultar a ata do dia e complementar os marcos faltantes (`app/chamados/services/pergunta_no_app.py:148`; `app/chamados/services/pergunta_no_app.py:159`; `app/chamados/services/disputa_emissao.py:908`; `app/chamados/services/disputa_emissao.py:974`; `app/chamados/services/disputa_emissao.py:978`).

   A guarda da pergunta usa `_coberto_por_ausencia`, que passou a deixar passar parcial **com período**, sem consultar julgamento anterior ou fato novo. Portanto, um chamado preexistente pode ganhar perguntas para o resto de um dia que `parcial_so_lista` mandaria à lista. Na parcial **sem período**, a pergunta é barrada, mas o pai já pode ter nascido (`app/chamados/services/disputa_emissao.py:763`; `app/chamados/services/disputa_emissao.py:770`; `app/chamados/services/materializacao.py:78`; `app/chamados/services/materializacao.py:81`; `app/ponto/services/cartorio.py:372`).

   A exclusão do homônimo no censo não prova que esse fluxo esteja fora da obrigação: ele efetivamente lê os marcos da célula para criar perguntas (`app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:462`; `app/chamados/services/disputa_emissao.py:806`).

4. **BLOQUEIA — C: o instante anterior foi preservado na fábrica, mas não no corpo inteiro nem no c2.**

   | Leitor | Instante efetivamente usado |
   |---|---|
   | Primeira passada do corpo | `cel.julgada_em` anterior, antes do carimbo final (`app/ponto/services/cartorio.py:1618`; `app/ponto/services/cartorio.py:1733`). |
   | Fábrica dentro de `julgar_colab` | Snapshot anterior correto, inclusive quando seu valor é `None` (`app/ponto/services/cartorio.py:888`; `app/ponto/services/cartorio.py:938`; `app/chamados/services/fabrica_da_celula.py:102`). |
   | Segunda passada forçada | Mesmas instâncias, potencialmente já modificadas por `carimbar`; não recebe o snapshot (`app/ponto/services/cartorio.py:914`; `app/ponto/portas/celula.py:800`). |
   | Releitura pós-emissão | Também recebe as instâncias já carimbadas (`app/ponto/services/cartorio.py:979`). |
   | Lote | Último julgamento persistido antes da leitura do lote (`app/chamados/management/commands/disparar_perguntas_competencia.py:120`). |
   | c2 | Ata e `julgada_em` atuais do banco; pode ser o julgamento deste ato (`app/chamados/reconciliador.py:844`). |
   | Retroativo | Célula carregada antes do passe; o registro de cobertura, porém, vem do leitor que recalcula — achado 5 (`app/ponto/management/commands/emitir_furo_retroativo.py:69`; `app/ponto/management/commands/emitir_furo_retroativo.py:121`). |

   Quando o carimbo avança para `agora`, o intervalo de fato novo passa a começar nesse próprio ato. Para parcial com período, a segunda leitura pode mudar de “emite” para “só lista”; sem período, continua sempre “só lista” (`app/ponto/portas/celula.py:800`; `app/ponto/services/cartorio.py:370`; `app/ponto/services/cartorio.py:372`).

   Há um contracaso de fluxo: a ausência nova dispensa os marcos de uma cobrança antiga, deixando outro marco de fora ainda apagado. A cobrança antiga com fio é encerrada somente depois da primeira lavra; a segunda passada, que deveria poder cobrar o resto, já lê o carimbo novo. A fábrica conserva o snapshot anterior, mas o c2 volta a consultar o carimbo atual. Assim, fábrica e emissão podem decidir por instantes diferentes dentro do mesmo julgamento (`app/ponto/services/cartorio.py:1396`; `app/ponto/services/cartorio.py:899`; `app/ponto/services/cartorio.py:914`; `app/ponto/services/cartorio.py:938`; `app/chamados/reconciliador.py:845`). Reprodução integrada desse contracaso: **[nao verificado]**.

5. **BLOQUEIA — A/D: o leitor da célula continua derivando cobertura/período em vez de transportar a lâmpada lavrada.**

   A ata agora grava ambos os carimbos, mas `grade_da_celula` não os copia ao construir o dia. Depois consulta novamente a ausência atual e recalcula cobertura e período — inclusive no trecho alterado pelo diff (`app/ponto/services/cartorio.py:691`; `app/ponto/services/cartorio.py:696`; `app/escala/services/leitor_celula.py:370`; `app/escala/services/leitor_celula.py:385`; `app/escala/services/leitor_celula.py:404`; `app/escala/services/leitor_celula.py:409`; `app/escala/services/leitor_celula.py:420`).

   Não é um segundo algoritmo de período: chama o mesmo juiz. É uma **segunda derivação em outro momento**, misturando lâmpadas persistidas com cadastro atual. Ata `sem_periodo` e cadastro já corrigido podem produzir respostas diferentes na fábrica/lote/c2 e no retroativo. A propriedade que compara grade e ata recém-produzidas não testa essa divergência temporal (`app/ponto/services/cartorio.py:346`; `app/escala/services/leitor_celula.py:404`; `app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:247`). Incidência real dessa janela: **[nao verificado]**.

6. **FILA — B/D: a mordida dos mutantes AST é real, mas estreita; o universo não está fechado.**

   O censo reconhece as portas enumeradas, resolve `ImportFrom` e trata somente `_abrir_chamado` como chamada de método. Não reconhece emissão genérica por `ChamadoColaborador.abrir`, `PerguntaDisputa.abrir` nem chamada por módulo importado (`app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:311`; `app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:353`; `app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:388`; `app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:394`).

   Além disso, qualquer chamada sintática ao predicado absolve a função: não se exige que seu resultado proteja a emissão. `if False: parcial_so_lista(...)` seguido de emissão continua conforme segundo esse algoritmo (`app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:390`; `app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:403`).

   Os mutantes escritos verificam retirada da chamada, retirada da delegação e novo emissor pela porta conhecida. Isso sustenta essas propriedades sintáticas, não a afirmação universal sobre emissores e fluxo (`app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:455`; `app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:465`; `app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:472`).

   Outras portas exigem fronteira explícita:

   | Caminho | Situação |
   |---|---|
   | `detectar_ausencias` | Não lê o predicado; a exceção usa guarda mais restritiva que cala qualquer ausência (`app/chamados/tests/test_cobparc5_parcial_sem_periodo_so_lista.py:334`; `app/ponto/management/commands/detectar_ausencias.py:346`). |
   | `reconciliar_grade` | Emissão direta omitida; sua guarda cala ausência não-falta, portanto não demonstra vazamento da parcial por essa porta (`app/ponto/management/commands/reconciliar_grade.py:215`; `app/ponto/management/commands/reconciliar_grade.py:391`; `app/ponto/management/commands/reconciliar_grade.py:507`). |
   | Retroativo de turno aberto | Não lê o predicado, mas cala qualquer ausência antes da abertura (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:108`; `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:151`). |
   | Par relâmpago | Abre diretamente; origem é batida/par, exigindo distinguir essa fronteira da emissão pela ata (`app/ponto/management/commands/detectar_par_relampago.py:352`; `app/ponto/management/commands/detectar_par_relampago.py:368`). |
   | Régua degenerada | Chamado de cadastro; não é cobrança de marco (`app/escala/regua_defesa.py:36`; `app/escala/regua_defesa.py:51`). |
   | Declaração explícita | Também cria disputa/pergunta pelos marcos; há gesto humano, que precisa ser distinguido da emissão automática (`app/chamados/services/declaracao_texto.py:288`; `app/chamados/services/declaracao_texto.py:310`; `app/chamados/services/declaracao_texto.py:600`). |
   | Fio/reapontamento | O c2 dispensa a leitura de PSL ao costurar em chamado já vivo; isso é diferente de nascer/renascer (`app/chamados/reconciliador.py:805`; `app/chamados/reconciliador.py:842`). A ressuscitação de perguntas também ocorre antes do portão PSL do corpo (`app/ponto/services/cartorio.py:1536`; `app/ponto/services/cartorio.py:1618`). |

7. **FILA — A/D: ainda há leitores que descartam o resto parcial pelo rótulo `ausencia`.**

   O relatório alterado aceita ausência parcial, mas `furos_diarios` continua pulando todo dia que não seja `trabalho`; os calculadores de preenchimento e marcações fazem o mesmo. Portanto o resto pode aparecer no relatório e faltar no acompanhamento/indicadores (`app/relatorios/furos.py:154`; `app/escala/services/furos_diarios.py:39`; `app/inteligencia/calculadores.py:46`; `app/inteligencia/calculadores.py:141`).

   No acompanhamento persistido, essa seleção também alimenta a remoção de furos cuja premissa desapareceu. Não é apenas diferença visual (`app/escala/services/furos_diarios.py:89`; `app/escala/services/furos_diarios.py:98`).

8. **NOTA — E: a v4 não seleciona dias parciais; invalida impressões de toda a competência 09 em diante.**

   `versao_para` devolve a maior versão cujo horizonte alcança a competência empresarial. Assim, 09 passa de 1 para 4 e 10 passa de 3 para 4; não há filtro por ausência ou por colaborador nominal. A versão entra no hash dos dois ramos do cartório (`app/ponto/services/cartorio.py:111`; `app/ponto/services/cartorio.py:235`; `app/ponto/services/cartorio.py:238`; `app/ponto/services/cartorio.py:443`; `app/ponto/services/cartorio.py:1113`; `app/ponto/services/cartorio.py:1266`).

   Na próxima execução, células passadas com impressão antiga deixam de ser puladas sem `--forcar`. O comando percorre todos os colaboradores com célula passada da empresa, sem filtro de situação. A configuração confirma execução automática por cron `--apply`; disparo imediato pelo deploy está **[nao verificado]** (`app/ponto/management/commands/processar_cartorio.py:49`; `app/ponto/management/commands/processar_cartorio.py:61`; `app/ponto/services/cartorio.py:1272`; `app/config/crons.py:597`).

   O rejulgamento usa **todo o código atual**: pode mover folga/trabalho, ata, protestos, veredito/status, chamados, perguntas e fios além dos dias parciais. O próprio teste cobre folga→trabalho na 09 sem forçar (`app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:146`; `app/ponto/services/cartorio.py:899`; `app/ponto/services/cartorio.py:935`; `app/ponto/services/cartorio.py:938`; `app/ponto/services/cartorio.py:1713`).

   Mesmo mantendo o veredito, o hash novo pode avançar `julgada_em`. Fora do diff, isso invalida lavras de perguntas e torna janelas históricas novamente elegíveis no censo de fase 12x36 (`app/ponto/portas/celula.py:796`; `app/ponto/portas/celula.py:800`; `app/chamados/veredito_lavrado.py:151`; `app/chamados/veredito_lavrado.py:187`; `app/escala/services/fase_12x36.py:188`).

   **No percurso de rejulgamento inspecionado**, a porta grava célula/outbox, e o consumidor atua em chamados/perguntas. Há guarda explícita para impedir recalcular o gravado da 09 exportada; o teste verifica FM inteiro e exportações com premissas não vazias. Isso não prova hashes reais nem execução em produção (`app/ponto/portas/celula.py:634`; `app/chamados/reconciliador.py:1310`; `app/ponto/services/fechamento.py:36`; `app/ponto/services/fechamento.py:544`; `app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:118`). FM/TXT reais antes/depois: **[nao verificado]**.

9. **NOTA — A: mapa de escrita e leitura dos estados alterados.**

   | Campo/valor | Quem escreve | Quem lê e efeito relevante |
   |---|---|---|
   | `dia.cobertura_parcial` | Overlay da grade; também recalculado no leitor da célula (`app/escala/utils.py:1247`; `app/escala/services/leitor_celula.py:409`). | Supra, cartório, status, relatórios e predicado da lista (`app/ponto/supra_juiz.py:268`; `app/ponto/services/cartorio.py:719`; `app/ponto/status_dia.py:158`; `app/relatorios/furos.py:154`; `app/ponto/services/cartorio.py:368`). |
   | `dia.periodo_parcial` | Overlay pelo juiz de período; novamente derivado no leitor (`app/escala/utils.py:1251`; `app/escala/services/leitor_celula.py:420`). | Supra, impressão e PSL (`app/ponto/supra_juiz.py:381`; `app/ponto/services/cartorio.py:408`; `app/ponto/services/cartorio.py:370`). |
   | Carimbos correspondentes na ata | `ata_do_dia` copia somente quando há cobertura parcial; persistência pela lavra/carimbo (`app/ponto/services/cartorio.py:691`; `app/ponto/services/cartorio.py:696`; `app/ponto/services/cartorio.py:1713`; `app/ponto/services/cartorio.py:1733`). | Fábrica, lote, c2, `dia_sem_trabalho` e calendário (`app/chamados/services/fabrica_da_celula.py:102`; `app/chamados/management/commands/disparar_perguntas_competencia.py:120`; `app/chamados/reconciliador.py:845`; `app/chamados/juizes.py:424`; `app/colaboradores/services/calendario.py:828`). |
   | `celula.dispensada` / `lampada.dispensada` | Grade dispensa missing dentro do período; cartório propaga; dispensa integral/justificativa também escreve (`app/escala/utils.py:1262`; `app/ponto/services/cartorio.py:520`; `app/ponto/services/cartorio.py:729`). | Leitor reconstrói marcador; contagens, status e cartório excluem dispensa de batida real (`app/escala/services/leitor_celula.py:127`; `app/escala/services/leitor_celula.py:91`; `app/ponto/status_dia.py:153`; `app/ponto/services/cartorio.py:1288`). |
   | `missing`, `acesa`, `luz`, `n_missing`, `n_celulas` | Grade altera missing; cartório monta lâmpadas/contagens e pode dispensá-las (`app/escala/utils.py:1263`; `app/ponto/services/cartorio.py:636`; `app/ponto/services/cartorio.py:671`; `app/ponto/services/cartorio.py:685`; `app/ponto/services/cartorio.py:726`). | Supra, lastro, fábrica, cluster e relatórios (`app/ponto/supra_juiz.py:306`; `app/ponto/services/lastro.py:129`; `app/chamados/services/fabrica_da_celula.py:49`; `app/ponto/services/cluster_espurio.py:31`; `app/relatorios/furos.py:154`). |
   | `n_ok`, `n_casadas` | Derivados da ata pelo leitor; não são novo campo persistente (`app/escala/services/leitor_celula.py:91`; `app/escala/services/leitor_celula.py:97`; `app/escala/services/leitor_celula.py:371`). | Supra prefere o número lavrado; corte de admissão/demissão e diagnóstico também mudam (`app/ponto/supra_juiz.py:229`; `app/ponto/supra_juiz.py:464`; `app/escala/services/leitor_celula.py:343`; `app/ponto/services/bordas_realizado.py:51`). |
   | `CelulaDia.ata` | Porta `lavrar_veredito`; operação própria para a chave `precedencia` (`app/ponto/portas/celula.py:709`; `app/ponto/portas/celula.py:768`; `app/ponto/portas/celula.py:811`). | Leitor da célula, juízes, lastro, perguntas, calendário e cluster — incluindo os consumidores fora do diff dos achados 2, 3 e 7 (`app/escala/services/leitor_celula.py:342`; `app/chamados/juizes.py:463`; `app/ponto/services/lastro.py:118`; `app/ponto/services/cluster_espurio.py:24`). |
   | `CelulaDia.julgada_em` | `carimbar`, com guarda de idempotência (`app/ponto/portas/celula.py:796`; `app/ponto/portas/celula.py:800`). | Impressão, PSL e seus emissores; invalidação de perguntas e fase, descritas nos achados 4 e 8 (`app/ponto/services/cartorio.py:1272`; `app/ponto/services/cartorio.py:372`; `app/chamados/veredito_lavrado.py:154`; `app/escala/services/fase_12x36.py:189`). |
   | `REGRA_VERSOES` / `regra` | Declaração no módulo; validação na carga (`app/ponto/services/cartorio.py:111`; `app/ponto/services/cartorio.py:209`). | `versao_para` → `_regra_do_dia` → impressão dos dois ramos (`app/ponto/services/cartorio.py:238`; `app/ponto/services/cartorio.py:1061`; `app/ponto/services/cartorio.py:1113`; `app/ponto/services/cartorio.py:1266`). |
   | `julgada_antes` | Snapshot local por data; sentinela diferencia omissão de `None` explícito (`app/ponto/services/cartorio.py:888`; `app/chamados/services/fabrica_da_celula.py:71`). | `perguntar_pela_celula` repassa a `motivos_a_perguntar`; não alcança corpo/c2 (`app/chamados/services/fabrica_da_celula.py:164`; `app/chamados/services/fabrica_da_celula.py:103`). |
   | `impressao`, `ausencia`, `tem_entrada`, `tem_saida`, `insumos_em` | Continuam juntos em `carimbar`; entrada/saída vêm da ata transitória (`app/ponto/services/cartorio.py:1732`; `app/ponto/portas/celula.py:801`; `app/ponto/portas/celula.py:805`). | Pulo por impressão e leitores da célula; a obra muda a impressão por versão/período/índices dispensados (`app/ponto/services/cartorio.py:1272`; `app/ponto/services/cartorio.py:399`; `app/ponto/services/cartorio.py:443`). |
   | Veredito, prote

[truncado em 20000 de 30171 caracteres]

## 01/11 20:41 -- HISTORICO (mais de 24 h) · sessao 3d0aefa7323b · - · - · origem=Ronald (tmux)

1. **BLOQUEIA — cluster é emissor omitido e passa a alcançar parcial sem período.** `cartorio.py:931-935` chama `cluster_espurio.reconciliar_colab` após lavrar; `cluster_espurio.py:23-35` lê ATA, exige duas órfãs próximas e lâmpada apagada; `:88` abre chamado sem `parcial_so_lista`. A mudança em `chamados/juizes.py:423-426` faz `dia_sem_trabalho` devolver False para a parcial. Contracaso: colaborador ativo, EC ativa, parcial sem período, duas órfãs em até 15 min e missing → chamado de cluster apesar de PSL=True. As guardas globais não abrangem cluster: `chamados/models.py:1061`, `chamados/reconciliador.py:336`, `chamados/catalogo/furo_modulos.py:15`. Ocorrência em produção: [nao verificado].

2. **FILA/BLOQUEIA conforme fronteira exigida — passivo pergunta pela ATA sem PSL.** `pergunta_no_app.py:148-159` chama `disputa_emissao.criar_disputa_a_partir_de_chamados_orfaos`; esta cria pai em `:908-932`, lê ATA em `:974`, complementa marcos em `:978` e pergunta por `_criar_pergunta_se_couber:756-780`. A guarda `:763` usa `_coberto_por_ausencia`; parcial COM período agora devolve False (`materializacao.py:76-82`), independentemente de fato novo. Assim pode perguntar o resto de dia já julgado que PSL mandaria à lista. SEM período cala perguntas nessa guarda, mas pai pode ter sido criado antes dela. Esse caminho parte de chamado existente e foi excluído nominalmente pelo teste de homônimo (`test_cobparc5...py:454`); não está protegido pela propriedade geral anunciada. `declaracao_texto.declarar_hora_colab:288,310,325` e `aplicar_declaracao:600,608,614` também criam pai/pergunta a partir de marcos da célula sem PSL, porém mediante declaração/toque explícito. [nao verificado]

3. **FILA — censo prova presença sintática, não universo nem proteção do fluxo.** `_imports` reconhece apenas `ImportFrom` (`test_cobparc5...py:353`); chamadas por módulo não entram. `censo:394` só reconhece método `_abrir_chamado`, omitindo `ChamadoColaborador.abrir`, `PerguntaDisputa.abrir` e as portas de disputa. `violacoes:403` aceita qualquer chamada ao predicado, inclusive sob `if False`, sem exigir retorno ou domínio sobre emissão. Executei em memória os nove métodos AST reais: **9/9 OK**; os três contramutantes “via módulo”, “PSL inalcançável” e “porta genérica direta” também escaparam. Isso confirma mordida real e estreita dos mutantes escritos, sem equivaler à suite Django. Código das verificações: `test_cobparc5...py:374-413,430-484`. [nao verificado]

4. **C — snapshot correto só para fábrica; demais passadas podem ler julgamento do próprio ato.** `julgar_colab:888` captura anterior e passa-o à fábrica em `:938`; fábrica usa explicitamente esse argumento em `fabrica_da_celula.py:102-103`. Primeira passagem do corpo lê `cel.julgada_em` em `cartorio.py:1618`, antes de `carimbar:1733`; a porta escreve e modifica a instância em `portas/celula.py:800`. Segunda passagem forçada (`cartorio.py:914`) e releitura pós-emissão (`:979`) recebem as mesmas instâncias: se o primeiro carimbo mudou, PSL lê **agora**, não o anterior. COM período, sem fato posterior ao carimbo, passa a True; pode calar tentativa na segunda passagem. SEM período continua True independentemente do instante (`:370-372`). Efeito concreto em produção: [nao verificado].

5. **C — c2 lê o carimbo atual, inclusive dentro da fábrica do próprio julgamento.** `disputa_emissao.py:404-409` chama reapontamento após criar pergunta; `reconciliador.py:1531` chega ao c2, que consulta ATA e `julgada_em` atuais em `:844-845`, sem snapshot nem `agora`. Se fábrica rodou após carimbo, c2 vê o julgamento deste ato. COM período, um fato que habilitou fábrica pelo snapshot anterior pode já ter sido consumido pelo carimbo e não habilitar c2. Chamado do dia já vivo pula PSL por `_costura_no_vivo` (`:805-806,842`), o que preserva costura. O fallback temporal é relógio próprio: `cartorio.houve_fato_novo:317`. Lote lê julgamento anterior ao lote (`disparar_perguntas_competencia.py:120`); retroativo lê célula carregada antes do passe (`emitir_furo_retroativo.py:69,121`). [nao verificado]

6. **Portas adicionais, com alcance distinguido.**
   - `detectar_par_relampago.py:352-368`: abre chamado sem PSL; origem é batida/par completo, não ATA. Precisa fronteira explícita do universo.
   - `reconciliar_grade.py:250-254,488-530`: emissão direta omitida pelo AST; `_coberto:391-403`, consultado em `:216`, cala toda ausência não-falta, inclusive parcial. Não demonstra vazamento da parcial não-falta por esse caminho.
   - `reconciliar_grade.py:412-428`: pai de disputa por porta omitida.
   - `regua_defesa.py:36-78`: abre chamado de **cadastro** `regua_degenerada`, não cobrança de marco; exclusão legítima exige declarar essa fronteira.
   - `emitir_turno_aberto_retroativo.py:108-110`: não lê PSL, mas cala qualquer ausência antes de abrir em `:151`.
   - `detectar_ausencias.py:346-361`: allow tem fundamento na guarda de qualquer ausência; sua exceção enfraquece a formulação literal “todo emissor lê o mesmo predicado”, embora seja mais restritiva na emissão.

_1476 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
