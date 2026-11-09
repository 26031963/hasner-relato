# CODEX-ESTADO

> Ultima resposta processada: **08/11/2026 23:15** · publicado em **08/11/2026 23:16** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 08/11 23:15 · sessao 7f6b12750a43 · - · - · origem=Ronald (tmux)

Análise estática; não executei selos ou mutantes, pois criam/removem temporários. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:36`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:96`, `bin/tests/test_grep_q_sem_sigpipe.sh:30`)

| Caso | O que efetivamente morde e limite |
|---|---|
| 1 | Normal com pendência/auth recusando: exige falha, zero apply e zero HUP. Detecta apply anterior à auth nesse caminho. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:99`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:100`) |
| 2 | Mesma proteção contra sombra recusando no normal. O falso aceita qualquer argumento; quem exige `--conferir` é a medição de ordem do caso 3. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:47`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:87`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:103`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:104`) |
| 3 | Exige exatamente um apply, auth/sombra `--conferir`/casca anteriores a ele, sucesso e um HUP. Não exige ordem relativa entre os três portões. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:82`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:85`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:92`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:108`) |
| 4 | Agora confirma chamada à auth, zero apply/HUP e falha, mas **não prova respeito ao retorno da auth**: com core pendente, a guarda de agendado também recusa. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:111`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:112`, `bin/deploy.sh:208`) |
| 5 | Exige sucesso, zero apply/HUP e ausência de chamada à sombra. Não exige ausência de chamada à auth: uma chamada cujo resultado seja ignorado passaria. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:115`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:116`) |
| 6 | Detecta apply normal anterior à casca recusando, bem como simples remoção da casca que deixe o deploy prosseguir. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:119`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:120`) |
| 7 | Protege auth antes do apply em `--sem-sombra`; não testa casca nesse modo nem um deploy verde nele. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:123`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:124`) |

Mutantes que passam, por inferência estática; execução dos mutantes: [nao verificado].

- Repor apply na seção 1 **somente em `--sem-sombra`**, conservando apply tardio no normal: caso 7 recusa antes de alcançar a seção 1; os outros casos não exercitam esse apply antecipado. O mesmo vale para `--restart`, ausente da matriz. (`bin/deploy.sh:142`, `bin/deploy.sh:201`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:99`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:123`)
- Chamar auth em agendado e ignorar seu retorno: caso 4 continua vermelho pelo core pendente, com `N_JAN>=1`. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:111`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:112`, `bin/deploy.sh:208`)
- Remover o escritor `APLICAR_MIGRATE=1` e executar apply incondicional após a casca: todos os casos que chegam ao apply usam pendência; nenhum caso verde usa `PEND=0`. (`bin/deploy.sh:220`, `bin/deploy.sh:292`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:99`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:107`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:123`)
- Apenas remover o escritor normalmente falha no caso 3; **com `APLICAR_MIGRATE=1` herdado**, pode passar toda a matriz: o selo não limpa essa variável, e os casos negativos encerram antes do leitor tardio. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:80`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:108`, `bin/deploy.sh:292`)

Reproduzi somente a varredura textual de `achados`, sem temporários: retornou zero linhas no conjunto efetivamente selecionado. Assim, não encontrei falso positivo efetivo atual; a regex crua casa a string legítima da própria fixture, mas o selo exclui seu próprio basename. (`bin/tests/test_grep_q_sem_sigpipe.sh:15`, `bin/tests/test_grep_q_sem_sigpipe.sh:19`, `bin/tests/test_grep_q_sem_sigpipe.sh:21`, `bin/tests/test_grep_q_sem_sigpipe.sh:25`, `bin/tests/test_grep_q_sem_sigpipe.sh:31`)

A autoprova passou a exigir os dois resultados, mas não demonstra determinismo universal: gera aproximadamente 1,29 MB com `ALVO` no início e não mede capacidade do pipe nem comportamento de leitura do `grep`. Se capacidade mais leitura inicial forem menores que a saída, o produtor não consegue terminar antes do fechamento: agendamento, sozinho, não basta para fazê-lo acertar. Premissas das implementações/host real e intermitência em produção: [nao verificado]. (`bin/tests/test_grep_q_sem_sigpipe.sh:37`, `bin/tests/test_grep_q_sem_sigpipe.sh:39`, `bin/tests/test_grep_q_sem_sigpipe.sh:44`)

Variáveis relevantes da alteração nos selos:

| Variável | Escritor | Leitor |
|---|---|---|
| `FAKE_CASCA` | Caso 6. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:119`) | Script falso gerado. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:48`) |
| `lc` | Posição da casca no log. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:88`) | Cálculo de ordem. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:92`) |
| `N_JAN` | Contagem do log. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:89`) | Caso 4. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:112`) |
| `N_SOM` | Contagem do log. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:90`) | Caso 5. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:116`) |
| `ORDEM_OK` | Inicialização e comparação. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:91`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:93`) | Diagnóstico/caso 3. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:94`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:108`) |
| `RX` | Literal da regex. (`bin/tests/test_grep_q_sem_sigpipe.sh:15`) | `grep -nE`. (`bin/tests/test_grep_q_sem_sigpipe.sh:21`) |
| `N` | Contagem dos achados da fixture. (`bin/tests/test_grep_q_sem_sigpipe.sh:32`) | Expectativa de três variantes. (`bin/tests/test_grep_q_sem_sigpipe.sh:33`) |
| `X` | `printf` + `seq`. (`bin/tests/test_grep_q_sem_sigpipe.sh:37`) | Pipeline e here-string. (`bin/tests/test_grep_q_sem_sigpipe.sh:39`, `bin/tests/test_grep_q_sem_sigpipe.sh:40`) |
| `r_pipe`, `r_here` | Ramos dos respectivos testes. (`bin/tests/test_grep_q_sem_sigpipe.sh:39`, `bin/tests/test_grep_q_sem_sigpipe.sh:40`) | Veredito e diagnóstico. (`bin/tests/test_grep_q_sem_sigpipe.sh:44`, `bin/tests/test_grep_q_sem_sigpipe.sh:45`) |

## 08/11 23:14 · sessao eca57893509f · - · - · origem=Ronald (tmux)

**Q2.** `APLICAR_MIGRATE` tem um escritor interno, `APLICAR_MIGRATE=1`, no ramo core pendente e elegível, e um leitor, no bloco 1c; não é zerada junto com os demais flags do parser. Portanto, `export APLICAR_MIGRATE=1` chega ao apply quando `PEND=0`, inclusive em `--sem-migrate`, `--reload-agendado` e `--reload-copia`. Antes, sem pendência, esses caminhos não executavam o apply da seção 1. (`bin/deploy.sh:56`, `bin/deploy.sh:67`, `bin/deploy.sh:78`, `bin/deploy.sh:201`, `bin/deploy.sh:220`, `bin/deploy.sh:292`, `bin/deploy.sh:294`)

Isso **não contorna as recusas existentes com `PEND>0`**: cópia, agendado e sem-migrate continuam encerrando antes do bloco 1c, mesmo com a variável herdada. `--conferir` também encerra antes dele. Não há, por esse vazamento, modo que passe com a mesma pendência que antes provocava recusa; há apply indevido em caminhos antes aceitos **sem apply**. (`bin/deploy.sh:205`, `bin/deploy.sh:208`, `bin/deploy.sh:212`, `bin/deploy.sh:224`, `bin/deploy.sh:226`, `bin/deploy.sh:292`)

Com ambiente limpo e portões verdes, a elegibilidade permanece normal/sem-sombra/restart. Nesses modos, casca vermelha agora impede o apply que antes a antecedia, conforme o diff fornecido; no restart, a nova verificação de cron também pode impedir o apply. (`bin/deploy.sh:220`, `bin/deploy.sh:283`, `bin/deploy.sh:293`; anexo `codex_deployportao2.diff`, localização original removida **[nao verificado]**)

**Q4.** São **três** chamadas se o restart aplicar core: primeira em `114`, nova em `293`, e a preservada em `318`. Sem apply são duas. A chamada em `318` ainda pode recusar depois do apply em `294`, se um cron começar nesse intervalo. (`bin/deploy.sh:114`, `bin/deploy.sh:292`, `bin/deploy.sh:293`, `bin/deploy.sh:294`, `bin/deploy.sh:318`)

A rotina lista marcas com PID presente em `/proc` e remove marcas cujo PID não existe; `vivos` é local. Repetir a remoção de uma marca já ausente é inócuo por `rm -f`; não há contador nem append duplicado. Mas cada chamada observa novamente o estado e pode encontrar novos crons: o envelope cria a marca antes do comando e a remove por trap. Não existe trava que reserve a janela entre a verificação e o restart. Ocorrência dessa corrida em produção: **[nao verificado]**. (`bin/deploy.sh:105`, `bin/deploy.sh:106`, `bin/deploy.sh:108`, `bin/deploy.sh:109`, `bin/cron_run.sh:9`, `bin/cron_run.sh:10`, `bin/cron_run.sh:11`)

Mapa completo enviado nas mensagens anteriores, incluindo variáveis de ambos os selos e campos dos logs/fixtures.

## 08/11 23:14 · sessao 5594ec55ebdb · - · - · origem=Ronald (tmux)

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 08/11 23:08 · sessao 6ea0bd9fb4d7 · BUG-DEPLOY-PORTAO · verificador · origem=Code

**1. Auth e sombra agora antecedem o migrate, mas ainda há efeitos antes de portões e recusas depois de alterações no banco.** A sequência efetiva é esta. (`bin/deploy.sh:123`, `bin/deploy.sh:159`, `bin/deploy.sh:217`, `bin/deploy.sh:280`, `bin/deploy.sh:306`)

| Ordem | Passo e efeito | Posição dos portões |
|---|---|---|
| 1 | `arvore_sem_conflito`: lê índice e arquivos; pode encerrar qualquer modo. | Primeiro portão, antes dos demais passos abaixo. (`bin/deploy.sh:98`, `bin/arvore_sem_conflito.sh:19`, `bin/arvore_sem_conflito.sh:29`) |
| 2 | Só `--restart`: verifica crons e **apaga marcas de PID morto**. | Depois da árvore; antes de auth, sombra e pendências. A própria verificação já modifica arquivos. (`bin/deploy.sh:105`, `bin/deploy.sh:106`, `bin/deploy.sh:114`) |
| 3 | Auth: chama `janela_auth.sh` ou anuncia a dispensa por motivo. | Antes de sombra, consultas de pendências e migrate; omitido em `--conferir`. (`bin/deploy.sh:123`, `bin/deploy.sh:140`, `bin/deploy.sh:142`) |
| 4 | Sombra: chama `--conferir` ou **apenda `deploy_sem_sombra.log`**. O helper real executa também **`mkdir -p logs/sombra`**, antes de validar o carimbo. | Depois de auth; antes das recusas por mensageria/core pendentes. Agendado puro é isento; cópia não. (`bin/deploy.sh:148`, `bin/deploy.sh:157`, `bin/deploy.sh:159`, `bin/sombra.sh:84`, `bin/sombra.sh:410`) |
| 5 | Executa os dois `--check`; qualquer resposta não zero vira pendência. | Mensageria pendente recusa antes de aplicar migration, exceto em conferir/restart. (`bin/deploy.sh:181`, `bin/deploy.sh:191`, `bin/deploy.sh:197`) |
| 6 | Core pendente: executa `--plan`; conferir informa; cópia, agendado e sem-migrate recusam; demais **aplicam `migrate_schemas --tenant`**. | Árvore, primeira recusa do restart, auth, sombra e guarda de mensageria já foram tratados conforme o modo. (`bin/deploy.sh:202`, `bin/deploy.sh:203`, `bin/deploy.sh:205`, `bin/deploy.sh:208`, `bin/deploy.sh:212`, `bin/deploy.sh:217`) |
| 7 | Conferir encerra. Nos demais, `collectstatic` pode **reescrever estáticos/manifest**; falha apenas avisa. | Depois das pendências; antes da prova de casca. (`bin/deploy.sh:221`, `bin/deploy.sh:258`, `bin/deploy.sh:260`) |
| 8 | Prova de casca: **cria temporário, cria/remove container efêmero e apaga temporário**; pode recusar. App montado somente leitura, logs em tmpfs, rede desativada. | **Depois do migrate e collectstatic**, antes da publicação. Pode ser dispensada por `SEM_PROVA_DE_CASCA`. (`bin/deploy.sh:276`, `bin/deploy.sh:280`, `bin/prova_de_casca.sh:28`, `bin/prova_de_casca.sh:30`, `bin/prova_de_casca.sh:34`, `bin/prova_de_casca.sh:46`) |
| 9 | Restart: **repete a recusa por cron**, podendo novamente apagar marcas, e executa **`docker restart`**. Demais modos enviam **HUP aos três containers** e aguardam troca de workers. | A segunda recusa pode acontecer **depois de migrar**. Falha de HUP/troca também ocorre depois das escritas anteriores. (`bin/deploy.sh:306`, `bin/deploy.sh:307`, `bin/deploy.sh:324`, `bin/deploy.sh:337`) |
| 10 | O comando declarado para o boot do mensageria **aplica `migrate --noinput`**. | Efeito do ramo restart segundo o compose; configuração efetiva do container vivo: **[nao verificado]**. (`docker-compose.mensageria.yml:20`) |
| 11 | Espera os containers e executa `Client.get` em três rotas. Resposta inadequada encerra o deploy. | **Depois da publicação**. Escrita de negócio por essas requisições: **[nao verificado]**. (`bin/deploy.sh:346`, `bin/deploy.sh:362`, `bin/deploy.sh:380`, `bin/deploy.sh:383`) |
| 12 | Selo de reciclagem ainda pode recusar; contador de ImportError lê log. | Ambos depois da publicação; o contador não é portão. (`bin/deploy.sh:390`, `bin/deploy.sh:393`, `bin/importerror_na_janela.sh:13`, `bin/importerror_na_janela.sh:32`) |
| 13 | **Trunca/escreve `deploy.stamp.novo`, substitui `deploy.stamp`, cria diretório e apenda histórico**. | Depois das provas e selo. Histórico exclui agendado puro e prova de casca dispensada; stamp não tem essas exclusões. (`bin/deploy.sh:396`, `bin/deploy.sh:400`, `bin/deploy.sh:404`, `bin/deploy.sh:405`, `bin/deploy.sh:406`) |

Portanto, **sim: permanece alteração de schema/dados antes da prova de casca e da segunda recusa do restart**, sem compensação correspondente nesses ramos de falha. O HUP/restart também antecede os portões de rotas e reciclagem. A cura resolve especificamente a recusa tardia de auth/sombra; não estabelece atomicidade integral. (`bin/deploy.sh:217`, `bin/deploy.sh:281`, `bin/deploy.sh:306`, `bin/deploy.sh:324`, `bin/deploy.sh:383`, `bin/deploy.sh:390`)

“Antes de qualquer escrita” também é literalmente excessivo pelos arquivos acima. Além disso, comandos Django podem carregar settings que criam `/app/logs` e configuram arquivos de log; ocorrência efetiva dessas escritas neste ambiente: **[nao verificado]**. (`bin/deploy.sh:121`, `app/manage.py:9`, `app/config/settings/saas.py:118`, `app/config/settings/saas.py:139`, `app/config/settings/saas.py:163`)

**2. A política de inclusão dos modos foi preservada; os caminhos que recusam antecipadamente mudaram.** O parser considera somente `$1` como modo. (`bin/deploy.sh:56`, `bin/deploy.sh:60`, `bin/deploy.sh:79`, `bin/deploy.sh:83`)

| Modo | Auth, salvo motivo de dispensa | Sombra |
|---|---|---|
| Normal | Exigida | Exigida, salvo motivo herdado. (`bin/deploy.sh:140`, `bin/deploy.sh:148`) |
| `--conferir` | Não executa | Não executa. O fluxo anterior também encerrava antes do bloco removido, conforme o diff. (`bin/deploy.sh:123`, `bin/deploy.sh:221`) |
| `--sem-migrate` | Exigida | Exigida, salvo motivo herdado. (`bin/deploy.sh:61`, `bin/deploy.sh:140`, `bin/deploy.sh:148`) |
| `--sem-sombra` | Exigida | Dispensada com motivo obrigatório e tentativa de registro. (`bin/deploy.sh:83`, `bin/deploy.sh:85`, `bin/deploy.sh:140`, `bin/deploy.sh:157`) |
| `--reload-agendado` | Exigida | Isento. (`bin/deploy.sh:62`, `bin/deploy.sh:140`, `bin/deploy.sh:148`) |
| `--reload-copia` | Exigida | Exigida, salvo motivo herdado. (`bin/deploy.sh:79`, `bin/deploy.sh:140`, `bin/deploy.sh:148`) |
| `--restart` | Exigida | Exigida, salvo motivo herdado. (`bin/deploy.sh:69`, `bin/deploy.sh:140`, `bin/deploy.sh:148`) |

A resposta literal a “antes não passava e agora passa” é **sim, em caminhos recusados**: antes, mensageria pendente ou core pendente em sem-migrate/agendado/cópia encerravam antes de alcançar auth/sombra. Agora executam os portões aplicáveis primeiro. Isso altera a primeira causa/mensagem de falha e pode deixar registro de dispensa de sombra em uma tentativa posteriormente recusada ou cujo migrate falhe. (`bin/deploy.sh:140`, `bin/deploy.sh:158`, `bin/deploy.sh:197`, `bin/deploy.sh:205`, `bin/deploy.sh:208`, `bin/deploy.sh:212`, `bin/deploy.sh:217`)

**3. Os chamadores identificados são os seguintes; instalação e execução reais em produção permanecem [nao verificado].**

| Origem | Chamada e dependência observável |
|---|---|
| Cron declarado | 03:30, `--reload-agendado`, com saída em log. (`app/config/crons.py:326`) |
| Integrador | Wrapper chama o lote; lote faz `--reload-copia` após copiar, descartando seu retorno. O deploy final `--sem-sombra` tem retorno conferido antes de marcar FIM/NO_AR. (`bin/integrador.sh:52`, `app/core/integrador_lote.py:627`, `app/core/integrador_lote.py:638`, `app/core/integrador_lote.py:753`, `app/core/integrador_lote.py:754`, `app/core/integrador_lote.py:763`) |
| Molde de fatia | `--reload-copia` após desfazer e copiar; final `--sem-sombra`. Reconhece sucesso final pelo texto `^deploy: OK`. (`bin/molde_fatia/cadeia.sh:32`, `bin/molde_fatia/cadeia.sh:48`, `bin/molde_fatia/cadeia.sh:89`, `bin/molde_fatia/cadeia.sh:90`) |
| Molde autocura | Mesmas três chamadas e reconhecimento textual. (`bin/molde_autocura/cadeia.sh:31`, `bin/molde_autocura/cadeia.sh:45`, `bin/molde_autocura/cadeia.sh:86`, `bin/molde_autocura/cadeia.sh:87`) |
| Vigia de logins | Após revert, chama `--sem-migrate`; falha é anunciada. (`bin/vigia_logins.sh:87`) |
| Vigia da esteira | Auto-revert e revert do revert chamam `--sem-sombra`, descartando os retornos dos deploys. (`app/core/esteira_vigia.py:1267`, `app/core/esteira_vigia.py:1275`) |
| Fatia abono | Deploy normal, depois backfill; verifica falha do pipeline do deploy. (`fatias_agendadas/abono-no-ar/esteira.sh:37`, `fatias_agendadas/abono-no-ar/esteira.sh:45`) |
| Fatia o135 | Usa `--conferir`; após merge usa `--sem-migrate` e guarda seu retorno. (`fatias_agendadas/o135-teto20/esteira.sh:80`, `fatias_agendadas/o135-teto20/esteira.sh:116`, `fatias_agendadas/o135-teto20/esteira.sh:117`) |
| Agendador genérico | Delega à `esteira.sh` da fatia. (`bin/deploy_agendado.sh:73`) |
| Fabricante | Declara que não publica e proíbe deploy; comportamento indireto de conteúdo gerado: **[nao verificado]**. (`bin/fabricante.sh:8`, `bin/fabricante.sh:17`, `bin/fabricante_prompt.md:70`) |
| Selos | Os dois testes comportamentais executam uma cópia do deploy contra docker falso. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:34`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:74`, `bin/tests/test_deploy_reload_gracioso.sh:36`, `bin/tests/test_deploy_reload_gracioso.sh:95`) |

Os consumos observados são retorno final, mensagem final ou descarte do retorno; não há nesses pontos uma espera por “migrate antes do carimbo”. Dependência externa ou dinâmica dessa ordem: **[nao verificado]**. (`app/core/integrador_lote.py:754`, `bin/molde_fatia/cadeia.sh:90`, `app/core/esteira_vigia.py:1267`)

**4. O selo morde a regressão principal no modo normal, mas não demonstra sua promessa universal.** Não executei os selos: eles criam e removem arquivos temporários. Os resultados abaixo são análise estática, não execução de mutantes. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:31`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:34`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:86`)

| Caso | O que efetivamente cobra |
|---|---|
| 1 — auth recusa | Normal, core pendente: exige rc não zero, zero migrate contado e zero HUP. Mover auth para depois do migrate normal quebra essa expectativa. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:89`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:90`) |
| 2 — sombra recusa | Mesma cobrança para sombra no normal. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:93`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:94`) |
| 3 — verde | Exige um migrate, um HUP e registro dos dois portões antes do migrate. É a proteção contra passar apenas por falha antecipada. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:79`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:83`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:97`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:98`) |
| 4 — agendado | Exige apenas rc não zero e zero HUP. Não põe core pendente, não exige ausência de migrate nem confirma que auth causou a recusa. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:101`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:102`) |
| 5 — conferir | Exige sucesso sem migrate/HUP mesmo com auth recusando. Não verifica ausência de chamada à sombra. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:105`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:106`) |

Há mutantes que a matriz deixa escapar, por inferência estática:

- **Antecipar os portões somente no normal e manter o bloco antigo nos demais modos.** Os três primeiros casos passam; o agendado ainda recusa auth sem HUP; conferir continua saindo antes. A ordem continua desprotegida em sem-sombra/restart/cópia. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:89`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:93`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:97`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:101`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:105`)
- **Trocar `sombra --conferir` por outra operação.** O falso aceita qualquer argumento; a medição de ordem procura somente `PORTAO sombra`. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:42`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:81`)
- **Acrescentar outro efeito não contado**, como restart antes do portão: o falso aceita comandos desconhecidos, enquanto as asserções contam somente a forma específica de migrate e HUP. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:60`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:76`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:77`)

Há ainda contaminação possível pelo ambiente: o selo limpa `SEM_SOMBRA_MOTIVO`, mas o deploy imediatamente a sobrescreve usando `DEPLOY_SEM_SOMBRA`. Um valor herdado nessa segunda variável dispensa o falso portão e pode produzir vermelho com a cura correta. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:74`, `bin/deploy.sh:82`, `bin/deploy.sh:149`)

**5. O diff contém 19 substituições, em 18 linhas — não 17.**

| Grupo | Quantidade e valores |
|---|---|
| Atomicidade | 2: `M`, `C`, saídas do varredor sobre fixtures. (`bin/tests/test_atomicidade_da_porta.sh:55`, `bin/tests/test_atomicidade_da_porta.sh:56`, `bin/tests/test_atomicidade_da_porta.sh:83`, `bin/tests/test_atomicidade_da_porta.sh:84`) |
| Import tardio | 2: `OUT`, `MORDE`, stdout/stderr do Python. (`bin/tests/test_import_tardio_contra_o_ar.sh:23`, `bin/tests/test_import_tardio_contra_o_ar.sh:25`, `bin/tests/test_import_tardio_contra_o_ar.sh:37`, `bin/tests/test_import_tardio_contra_o_ar.sh:38`) |
| Inventário | 2: `OUT`, `MORDE`, saída de `_rodar`. (`bin/tests/test_inventario_pessoal_no_commit.sh:31`, `bin/tests/test_inventario_pessoal_no_commit.sh:33`, `bin/tests/test_inventario_pessoal_no_commit.sh:61`, `bin/tests/test_inventario_pessoal_no_commit.sh:62`) |
| Akita | 3: `PADRAO_OUT` e duas consultas a `MORDE_OUT` na mesma linha. (`bin/tests/test_lei_akita.sh:730`, `bin/tests/test_lei_akita.sh:734`) |
| Raia | 10: cinco consultas a `SEM`, cinco a `COM`, ambos JSON. (`bin/tests/test_raia_declarada_no_estado.sh:63`, `bin/tests/test_raia_declarada_no_estado.sh:69`, `bin/tests/test_raia_declarada_no_estado.sh:79`) |

Nas saídas normais declaradas pelos produtores, não identifiquei valor que seja uma opção isolada de `echo`, como `-n`/`-e`: há prefixos de diagnóstico, marcadores ou JSON. Sob `echo` sem interpretação de escapes, ambos os caminhos acrescentam a quebra final ao valor capturado; a mudança pretendida é eliminar o produtor que pode receber SIGPIPE. Configuração efetiva de `echo` no ambiente de produção: **[nao verificado]**. (`bin/atomicidade_da_porta.py:97`, `bin/import_tardio_contra_o_ar.py:204`, `bin/inventario_pessoal_dos_modelos.py:85`, `bin/tests/test_lei_akita.sh:719`, `bin/tests/test_lei_akita.sh:725`, `bin/tests/test_raia_declarada_no_estado.sh:63`)

**Não há equivalência universal de bytes:** `SEM`/`COM` contêm escapes JSON `\n`, pois `linha()` produz texto multilinha. Um `echo` configurado para expandir escapes transforma esse conteúdo; a here-string o preserva. Mudança adicional de veredito nos casos concretos fornecidos: **[nao verificado]**. (`bin/raias_estado.py:153`, `bin/tests/test_raia_declarada_no_estado.sh:65`, `bin/tests/test_raia_declarada_no_estado.sh:69`, `bin/tests/test_raia_declarada_no_estado.sh:79`)

**Sobraram pipelines da mesma classe sob `pipefail`:**

- `printf ... | grep -qxF` na régua de tickets. (`bin/regua_tickets.sh:15`, `bin/regua_tickets.sh:87`)
- Quatro `printf ... | grep -q...` no selo da suite. (`bin/tests/test_suite_sh.sh:14`, `bin/tests/test_suite_sh.sh:51`, `bin/tests/test_suite_sh.sh:53`, `bin/tests/test_suite_sh.sh:55`, `bin/tests/test_suite_sh.sh:57`)
- `printf | grep | grep -q` no selo da sonda. (`bin/tests/test_codex_sonda.sh:9`, `bin/tests/test_codex_sonda.sh:91`)
- Função produtora com `git show | grep -q`. (`bin/tests/test_codex_estado_evento.sh:9`, `bin/tests/test_codex_estado_evento.sh:57`, `bin/tests/test_codex_estado_evento.sh:62`)
- Função produtora com `cat | grep -q`; nesses ramos, falha do pipeline pode anunciar “negado” apesar de encontrar o marcador. Isso é inferência do encadeamento, não reprodução dinâmica. (`bin/tests/prova_codex_pergunta_real.sh:15`, `bin/tests/prova_codex_pergunta_real.sh:142`, `bin/tests/prova_codex_pergunta_real.sh:144`, `bin/tests/prova_codex_pergunta_real.sh:147`, `bin/tests/prova_codex_pergunta_real.sh:152`)

O `grep -c` remanescente de Akita consome toda a entrada: isoladamente não tem o encerramento antecipado de `-q`. O diagnóstico com `head -2` ainda pode cortar o produtor, mas está na mensagem de uma falha já decidida. (`bin/tests/test_lei_akita.sh:729`, `bin/tests/test_lei_akita.sh:734`, `bin/tests/test_lei_akita.sh:735`)

**6. Sim: o selo textual admite falsos positivos e negativos.** A regex exige a forma estreita `echo "$NOME" | grep -...q`; a seleção de arquivos exige somente que o texto contenha `pipefail`. (`bin/tests/test_grep_q_sem_sigpipe.sh:14`, `bin/tests/test_grep_q_sem_sigpipe.sh:19`)

- **Falsos negativos:** nomes com dígitos, `${OUT}`, espaçamento diferente, continuação de linha, caminho explícito para grep, `--quiet`, produtores `printf`/`cat`/funções, subdiretórios e pipefail herdado sem essa palavra no arquivo. Os pipelines reais da resposta 5 ilustram essa lacuna. (`bin/tests/test_grep_q_sem_sigpipe.sh:14`, `bin/tests/test_grep_q_sem_sigpipe.sh:19`, `bin/tests/test_grep_q_sem_sigpipe.sh:24`, `bin/regua_tickets.sh:87`, `bin/tests/prova_codex_pergunta_real.sh:144`)
- **Falsos positivos:** `pipefail` apenas em comentário ou desativado; forma canônica dentro de string/heredoc/comentário inline. O filtro exclui somente comentários de linha inteira e não interpreta shell. (`bin/tests/test_grep_q_sem_sigpipe.sh:19`, `bin/tests/test_grep_q_sem_sigpipe.sh:20`)
- **Autoprova incompleta:** MORDE 1 cobre apenas a forma canônica. MORDE 2 calcula `r_pipe`, mas não exige que seja zero: fica verde sempre que `r_here=1`, mesmo se o pipeline antigo também acertar. (`bin/tests/test_grep_q_sem_sigpipe.sh:30`, `bin/tests/test_grep_q_sem_sigpipe.sh:31`, `bin/tests/test_grep_q_sem_sigpipe.sh:37`, `bin/tests/test_grep_q_sem_sigpipe.sh:40`)

**7. Os leitores efetivos da chave são genéricos, pelo conjunto `ISENTOS`.** Inventário de modelos e selo SQL montam a cobertura com `set(R.ISENTOS)` e comparam `tabela.coluna`; portanto passam a reconhecer a chave nova. (`bin/sombra_regras_pessoais.py:230`, `bin/inventario_pessoal_dos_modelos.py:52`, `bin/inventario_pessoal_dos_modelos.py:78`, `bin/sombra_selo_pessoal.py:25`, `bin/sombra_selo_pessoal.py:34`)

Não encontrei consumidor literal da chave antiga em `bin/`/`app/`; consumidores por nomes construídos dinamicamente: **[nao verificado]**. A ocorrência literal remanescente de `chamados_respostaanterior` no instrumento é comentário sobre o incidente. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:9`)

O **modelo antigo** permanece na criação histórica, no `RenameModel` e em um teste AST que vigia os dois nomes; isso não equivale a código corrente consultando a tabela antiga. A troca da isenção acompanha o rename declarado. (`app/chamados/migrations/0050_resposta_anterior.py:17`, `app/chamados/migrations/0051_decisao_pergunta.py:41`, `app/chamados/tests/test_reabrir_nao_destroi.py:129`, `bin/sombra_regras_pessoais.py:230`)

**8. Escritores e leitores dos estados pedidos:**

| Estado | Quem escreve | Quem lê |
|---|---|---|
| `logs/deploy.stamp` | Deploy escreve `QUANDO`, `COMMIT`, `TITULO` em `.novo` e faz `mv`, usando caminho absoluto de produção. (`bin/deploy.sh:397`, `bin/deploy.sh:398`, `bin/deploy.sh:399`, `bin/deploy.sh:400`) | Janela lê `COMMIT`, com override de caminho; import tardio lê `COMMIT`, salvo override; handoff lê `QUANDO`/`COMMIT`; vigia lê os três campos para atribuição/quarentena, auto-revert e diagnóstico de relógio. (`bin/janela_auth.sh:36`, `bin/janela_auth.sh:39`, `bin/import_tardio_contra_o_ar.py:46`, `bin/import_tardio_contra_o_ar.py:50`, `bin/handoff_sessao.sh:254`, `bin/handoff_sessao.sh:255`, `app/core/esteira_vigia.py:990`, `app/core/esteira_vigia.py:1160`, `app/

[truncado em 20000 de 22621 caracteres]

## 08/11 23:03 · sessao 351fb2a58f19 · - · - · origem=Ronald (tmux)

Q5 — há **19 substituições em 18 linhas**, pelos sítios atuais: atomicidade 2; import tardio 2; inventário 2; lei Akita 3; raias 10. A linha 734 da lei Akita contém duas substituições. (`bin/tests/test_atomicidade_da_porta.sh:56,84`; `bin/tests/test_import_tardio_contra_o_ar.sh:25,38`; `bin/tests/test_inventario_pessoal_no_commit.sh:33,62`; `bin/tests/test_lei_akita.sh:730,734`; `bin/tests/test_raia_declarada_no_estado.sh:69-73,79-83`)

Proveniência e equivalência:

- `M` e `C` são saídas do varredor Python sobre fixtures controlados; a primeira linha tem prefixo literal `select_for_update:`. Não podem ser apenas opções `-n`/`-e` nesse caminho normal. (`bin/tests/test_atomicidade_da_porta.sh:43-55,64-83`; `bin/atomicidade_da_porta.py:97-103`)
- `OUT`/`MORDE` do import vêm do Python, com stderr incluído; os caminhos normais imprimem prefixos `IMPORT_TARDIO`. (`bin/tests/test_import_tardio_contra_o_ar.sh:23,37`; `bin/import_tardio_contra_o_ar.py:153,204-217`)
- `OUT`/`MORDE` do inventário vêm de `docker run`, juntando stderr e stdout; o programa imprime `INVENTARIO_MODELOS`. Conteúdo adicional emitido pelo Docker em produção: **[nao verificado]**. (`bin/tests/test_inventario_pessoal_no_commit.sh:23-31,61`; `bin/inventario_pessoal_dos_modelos.py:48,58,85-93`)
- `PADRAO_OUT`/`MORDE_OUT` vêm do Python embutido, que imprime marcadores literais `PADRAO_*`/`MORDE_*`. (`bin/tests/test_lei_akita.sh:719-734`)
- `SEM`/`COM` vêm de `json.dumps(...)` sobre a fixture controlada. **Esses valores contêm barras invertidas reais**, pois o campo `linha` recebe texto multilinha e JSON representa as quebras como `\n`. (`bin/tests/test_raia_declarada_no_estado.sh:57-68,76-78`; `bin/raias_estado.py:153-154`)

Com `echo` Bash usual, sem expansão de escapes, a troca preserva os bytes relevantes: os dois caminhos acrescentam uma quebra final ao valor já capturado por substituição de comando; a diferença intencional é eliminar o produtor sujeito a SIGPIPE. Nenhum desses arquivos configura `xpg_echo`; configuração herdada de produção: **[nao verificado]**. Se `xpg_echo` estiver habilitado, o `echo` antigo pode transformar `\n`, `\c` etc., enquanto a here-string preserva o conteúdo literal; logo equivalência absoluta de fluxo não está demonstrada, sobretudo nos JSONs. (`bin/tests/test_raia_declarada_no_estado.sh:63-73,79-83`; `bin/tests/test_atomicidade_da_porta.sh:15,55-56`; `bin/tests/test_import_tardio_contra_o_ar.sh:20,23-25`; `bin/tests/test_inventario_pessoal_no_commit.sh:17,31-33`; `bin/tests/test_lei_akita.sh:16,727-734`)

Sobraram membros reais da mesma classe:

| Local | Pipeline restante |
|---|---|
| `bin/regua_tickets.sh:15,87` | `printf "$DECLARADOS" \| grep -qxF` sob pipefail |
| `bin/tests/test_suite_sh.sh:14,51-57` | Quatro `printf "$CORPO" \| grep -q...` |
| `bin/tests/test_codex_sonda.sh:9,91` | `printf \| grep ... \| grep -q` |
| `bin/tests/test_codex_estado_evento.sh:9,57,62` | Função que executa `git show`, canalizada para `grep -q` |
| `bin/tests/prova_codex_pergunta_real.sh:15,142-152` | `_out(){ cat ...; }` canalizado para `grep -q` |

No último exemplo, SIGPIPE no `cat` pode fazer um padrão presente cair no ramo que anuncia acesso “negado”; é inferência do encadeamento `&& ... || ...`, sem reprodução dinâmica. (`bin/tests/prova_codex_pergunta_real.sh:142-152`)

O `echo "$PADRAO_OUT" | grep -c` restante consome a entrada inteira, portanto não é o mesmo problema de encerramento antecipado; o diagnóstico com `head -2` pode cortar o produtor, mas não decide o resultado do selo. (`bin/tests/test_lei_akita.sh:729,735`)

Q6 — o selo é uma busca textual estreita, não um detector geral de SIGPIPE. A expressão só reconhece `echo "$NOME"` com letras/sublinhado, espaçamento exato e `grep -...q`; procura apenas nos dois níveis listados. (`bin/tests/test_grep_q_sem_sigpipe.sh:14,24`)

**Falsos negativos:** `$OUT2`, `${OUT}`, espaçamento alternativo, continuação de linha, `/bin/grep`, `--quiet`, `printf`, `cat`, funções produtoras e scripts em subdiretórios escapam; pipefail herdado ou ativado por `source` sem a palavra no arquivo também escapa. Os exemplos reais acima passam pela regex. (`bin/tests/test_grep_q_sem_sigpipe.sh:14,19,24`; `bin/regua_tickets.sh:87`; `bin/tests/prova_codex_pergunta_real.sh:142-152`)

**Falsos positivos:** basta a palavra `pipefail` em comentário ou em comando que o desative; só comentários de linha inteira são retirados. Uma ocorrência canônica dentro de string, heredoc ou comentário inline pode ser acusada mesmo sem ser pipeline executado. (`bin/tests/test_grep_q_sem_sigpipe.sh:19-20`)

O MORDE 1 exercita somente a forma canônica, sem contraexemplos para precisão. O MORDE 2 calcula `r_pipe`, mas **não exige que seja zero**: fica verde sempre que `r_here=1`, mesmo se o pipeline antigo também acertar. Portanto ele não comprova a reprodução do SIGPIPE no ambiente em que roda. (`bin/tests/test_grep_q_sem_sigpipe.sh:30-32,35-41`)

Não executei os selos: eles criam e removem temporários, contrariando a restrição somente leitura. (`bin/tests/test_grep_q_sem_sigpipe.sh:29-30`; `bin/tests/test_atomicidade_da_porta.sh:41-43`; `bin/tests/test_raia_declarada_no_estado.sh:31-55`)

## 08/11 23:03 · sessao 6fde343d3c37 · - · - · origem=Ronald (tmux)

Q1 — sequência e efeitos:

| Ordem | Passo / efeito | Portões que já responderam |
|---|---|---|
| 1 | `arvore_sem_conflito` lê índice/disco; pode encerrar todos os modos. | Nenhum anterior. `bin/deploy.sh:98`; `bin/arvore_sem_conflito.sh:19-35`. |
| 2 | Só `--restart`: primeira recusa de cron; durante a checagem **apaga marcas de PID morto** com `rm -f`. | Árvore; auth, sombra e pendências ainda não. `bin/deploy.sh:105-114`. |
| 3 | Exceto conferir: auth ou motivo de dispensa; motivo só vai ao stdout deste script. | Árvore e primeira recusa do restart, quando aplicável. `bin/deploy.sh:123-145`. |
| 4 | Exceto conferir/agendado: sombra ou dispensa. Dispensa **apenda arquivo de produção** `logs/deploy_sem_sombra.log`; sombra real também executa **`mkdir -p logs/sombra`** antes de conferir. | Auth já respondeu; mensageria e migration pendente ainda não. `bin/deploy.sh:148-159`; `bin/sombra.sh:81-84`. |
| 5 | Checa migrations core e mensageria por subprocessos `docker exec`; mostra plano quando core pendente. A recusa de mensageria acontece antes do migrate aplicado; restart/conferir são dispensados dessa recusa. | Árvore/auth/sombra conforme modo. `bin/deploy.sh:181-202`. |
| 6 | Core pendente: conferir só informa; cópia/agendado/sem-migrate recusam; demais aplicam `migrate_schemas --tenant`. | Todos os portões pedidos, **exceto a segunda recusa de restart**, já responderam; recusa mensageria é dispensada no restart. `bin/deploy.sh:197-217`. |
| 7 | Conferir sai; demais podem escrever estáticos/manifest por `collectstatic`. | Pendências já tratadas. `bin/deploy.sh:221-224`; `bin/deploy.sh:257-261`. |
| 8 | Prova de casca: cria temporário, cria/remove container efêmero e remove temporário; reprovação encerra. App está montado RO e logs em tmpfs. | Migration/collectstatic podem já ter ocorrido. `bin/deploy.sh:276-282`; `bin/prova_de_casca.sh:28-46`. |
| 9 | Restart: **segunda recusa de cron**, novamente podendo apagar marcas mortas; depois `docker restart`. Demais: `docker kill -s HUP` nos três e checagem de troca dos workers. | Prova de casca já respondeu; segunda recusa pode impedir restart **depois** do migrate/collectstatic. `bin/deploy.sh:304-337`. |
| 10 | Restart do mensageria executa migration no boot segundo compose; as configurações efetivas do container não foram fornecidas. | Portões anteriores passaram. `docker-compose.mensageria.yml:20`; configuração viva [não verificado]. |
| 11 | Subprocessos esperam containers e fazem `Client.get` nas três rotas. Rotas ruins encerram **depois da publicação**. | Todos os portões pedidos anteriores já responderam. `bin/deploy.sh:345-383`. |
| 12 | Selo reciclagem ainda pode reprovar depois da publicação; contador ImportError só lê arquivo. | Portões pedidos anteriores passaram. `bin/deploy.sh:390-393`; `bin/importerror_na_janela.sh:27-32`. |
| 13 | Escreve/trunca `deploy.stamp.novo`, renomeia sobre `deploy.stamp`; depois cria diretório e apenda histórico, salvo agendado puro ou prova pulada. | Inclusive rotas e selo reciclagem já passaram. `bin/deploy.sh:396-407`. |

Efeitos incidentais adicionais: os comandos Django usam por padrão settings SaaS; importar essas settings executa `os.makedirs('/app/logs')` e configura handlers de `errors.log`/`ponto.log`. Portanto não há base para prometer “zero arquivo escrito” por `--check`, `--plan` ou `--conferir`; ambiente efetivo/arquivos existentes [não verificado]. `app/manage.py:9-18`; `app/config/settings/saas.py:118-139`; `app/config/settings/saas.py:160-166`.

Provas GET não demonstram escrita de negócio: UI sem login é protegida por `@login_required`; os dois healths apenas constroem JSON. Escrita de sessão/cache nessas chamadas [não verificado]. `bin/deploy.sh:362-365`; `app/colaboradores/urls.py:47`; `app/colaboradores/views.py:1008-1009`; `app/core/views.py:1302-1306`; `mensageria/config/urls.py:7-8`.

**Conclusão Q1:** a cura põe auth e sombra antes do migrate, mas não torna deploy atômico. Migration ainda precede prova de casca, segunda recusa do restart, prova de rotas e selo reciclagem; esses passos podem recusar quando o banco já mudou. Também existe escrita de log/diretório antes dos portões de pendências. `bin/deploy.sh:157-158`; `bin/deploy.sh:217`; `bin/deploy.sh:280`; `bin/deploy.sh:306`; `bin/deploy.sh:383`; `bin/deploy.sh:390`.

Q2 — política dos modos:

| Modo | Auth | Sombra |
|---|---|---|
| normal | Sim, salvo motivo | Sim, salvo motivo herdado |
| `--conferir` | Não | Não |
| `--sem-migrate` | Sim, salvo motivo | Sim, salvo motivo herdado |
| `--sem-sombra` | Sim, salvo motivo | Dispensa, motivo obrigatório/apendado |
| `--reload-agendado` | Sim, salvo motivo | Isento |
| `--reload-copia` | Sim, salvo motivo | Exigida, salvo motivo herdado |
| `--restart` | Sim, salvo motivo | Exigida, salvo motivo herdado |

As condições dessa tabela são `bin/deploy.sh:56-86` e `bin/deploy.sh:123-164`; pelo diff fornecido, as condições internas do bloco movido não mudaram. Não existe combinação de flags: só `$1` escolhe o modo e `$2` é lido para motivos. `bin/deploy.sh:56-85`.

**Há mudança observável além de “ordem” em caminhos recusados:** antes, mensageria pendente ou core pendente com sem-migrate/agendado/cópia encerravam antes do bloco; agora esses caminhos executam auth e, quando cabível, sombra antes da recusa de pendência. Assim muda a primeira causa/mensagem de falha e pode existir registro “sem sombra” mesmo quando a migration subsequente falha ou a pendência impede publicação. `bin/deploy.sh:123-164`; `bin/deploy.sh:197-217`.

`--conferir` continua fora dos dois portões; agendado continua isento só de sombra; cópia continua sujeito aos dois. A cura não introduz outra isenção nem torna agendado sujeito à sombra. `bin/deploy.sh:123`; `bin/deploy.sh:148`; `bin/deploy.sh:221-223`.

## 08/11 23:03 · sessao 6854c0837f27 · - · - · origem=Ronald (tmux)

Q3 — Chamadores encontrados na cópia:

| Origem | Chamada / tratamento |
|---|---|
| Cron declarado | 03:30, `--reload-agendado`, saída em `reload_diario.log` (`app/config/crons.py:326`). Instalação real do cron em produção: [nao verificado]. |
| Integrador | Wrapper executa `integrador_lote.py rodar` (`bin/integrador.sh:52`); lote chama `--reload-copia` após copiar e gerar documentos, sem examinar seu retorno (`app/core/integrador_lote.py:627`, `app/core/integrador_lote.py:638`); deploy final `--sem-sombra` tem retorno conferido antes de marcar FIM/NO_AR (`app/core/integrador_lote.py:753`, `app/core/integrador_lote.py:754`, `app/core/integrador_lote.py:763`). |
| Molde de fatia | `--reload-copia` ao desfazer e após copiar; final `--sem-sombra` (`bin/molde_fatia/cadeia.sh:32`, `bin/molde_fatia/cadeia.sh:48`, `bin/molde_fatia/cadeia.sh:89`). Reconhece sucesso pelo texto `^deploy: OK` (`bin/molde_fatia/cadeia.sh:90`). |
| Molde autocura | Mesmas três chamadas (`bin/molde_autocura/cadeia.sh:31`, `bin/molde_autocura/cadeia.sh:45`, `bin/molde_autocura/cadeia.sh:86`); reconhece sucesso pelo texto `^deploy: OK` (`bin/molde_autocura/cadeia.sh:87`). |
| Vigia de login | Depois de `git revert`, chama `--sem-migrate`, conferindo falha da cadeia `&&` (`bin/vigia_logins.sh:87`). |
| Vigia da esteira | Auto-revert e revert do revert chamam `--sem-sombra`; retornos dos deploys são descartados (`app/core/esteira_vigia.py:1267`, `app/core/esteira_vigia.py:1275`). |
| Fatia agendada abono | Confere sombra antes; deploy normal seguido de backfill; verifica pipeline com deploy (`fatias_agendadas/abono-no-ar/esteira.sh:30`, `fatias_agendadas/abono-no-ar/esteira.sh:37`, `fatias_agendadas/abono-no-ar/esteira.sh:45`). |
| Fatia agendada o135 | Chama `--conferir`, confere sombra separadamente; após merge chama `--sem-migrate` e guarda retorno (`fatias_agendadas/o135-teto20/esteira.sh:80`, `fatias_agendadas/o135-teto20/esteira.sh:81`, `fatias_agendadas/o135-teto20/esteira.sh:116`, `fatias_agendadas/o135-teto20/esteira.sh:117`). |
| Agendador genérico | Não chama deploy diretamente; chama a esteira de cada diretório (`bin/deploy_agendado.sh:73`). |
| Fabricante | Declara que não publica e proíbe deploy; única ocorrência de `deploy.sh` no script é comentário (`bin/fabricante.sh:8`, `bin/fabricante.sh:17`); prompt também proíbe a chamada (`bin/fabricante_prompt.md:70`). Ausência de execução indireta por conteúdo fabricado: [nao verificado]. |

Nos chamadores acima, não encontrei consumo de “migrate antes do carimbo” ou leitura intermediária do log para disparar outra ação; o consumo mostrado é retorno final, `deploy: OK`, ou descarte do retorno (`bin/molde_fatia/cadeia.sh:89`, `bin/molde_fatia/cadeia.sh:90`, `app/core/integrador_lote.py:753`, `app/core/integrador_lote.py:754`, `app/core/esteira_vigia.py:1267`). Dependência externa não incluída na cópia: [nao verificado].

Q7 — Chave e tabela:

- A chave corrente está em `ISENTOS` como `chamados_decisaopergunta.pergunta_id` (`bin/sombra_regras_pessoais.py:230`).
- Os consumidores efetivos do conjunto `ISENTOS` são o inventário de modelos e o selo SQL da sombra, ambos comparando nomes completos tabela.coluna; nenhum acessa essa chave individualmente (`bin/inventario_pessoal_dos_modelos.py:52`, `bin/inventario_pessoal_dos_modelos.py:78`, `bin/sombra_selo_pessoal.py:25`, `bin/sombra_selo_pessoal.py:34`).
- O mascarador importa as mesmas regras, mas percorre os dicionários de transformação; não percorre `ISENTOS` (`bin/sombra_mascara_pessoal.py:12`, `bin/sombra_mascara_pessoal.py:17`, `bin/sombra_mascara_pessoal.py:44`).
- Busca literal da chave antiga em `bin/` e `app/`: nenhum consumidor encontrado; ausência absoluta ou nomes construídos dinamicamente: [nao verificado]. A ocorrência literal remanescente da tabela antiga no código de instrumentos é comentário sobre o incidente (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:9`).
- O nome de modelo antigo permanece legitimamente na criação e no `RenameModel` histórico (`app/chamados/migrations/0050_resposta_anterior.py:17`, `app/chamados/migrations/0051_decisao_pergunta.py:41`); aparece ainda no tripwire AST que procura escritores dos dois nomes (`app/chamados/tests/test_reabrir_nao_destroi.py:129`) e em comentários/documentação (`app/chamados/models.py:3830`, `app/templates/chamados/partials/modal_fio.html:364`).
- A transformação textual já usa `DecisaoPergunta.ato_texto/ato_motivo` (`bin/sombra_regras_pessoais.py:139`); a alteração de `ISENTOS` alinha o FK ao rename histórico (`bin/sombra_regras_pessoais.py:230`, `app/chamados/migrations/0051_decisao_pergunta.py:41`).

Q8 — Escritores e leitores:

| Estado | Escrita | Leitura |
|---|---|---|
| `logs/deploy.stamp` | Deploy grava `QUANDO`, `COMMIT`, `TITULO` em `.novo`, depois `mv` para destino absoluto de produção (`bin/deploy.sh:396`, `bin/deploy.sh:400`). | Janela lê primeiro `COMMIT=`; caminho pode ser substituído por `JANELA_AUTH_STAMP` (`bin/janela_auth.sh:36`, `bin/janela_auth.sh:39`). Checador de import lê `COMMIT`, salvo override `COMMIT_NO_AR` (`bin/import_tardio_contra_o_ar.py:46`, `bin/import_tardio_contra_o_ar.py:50`). Handoff lê `QUANDO` e `COMMIT` (`bin/handoff_sessao.sh:253`, `bin/handoff_sessao.sh:254`, `bin/handoff_sessao.sh:255`). Vigia da esteira lê para atribuir dono/quarentena, auto-revert e diagnóstico de relógio (`app/core/esteira_vigia.py:64`, `app/core/esteira_vigia.py:1160`, `app/core/esteira_vigia.py:1255`, `app/core/esteira_vigia.py:1313`); parser usa os três campos (`app/core/esteira_vigia.py:990`). |
| `logs/deploy_sem_sombra.log` | Append somente pelo bloco de override, falha de gravação tolerada (`bin/deploy.sh:149`, `bin/deploy.sh:157`, `bin/deploy.sh:158`). | Nenhum leitor literal de execução encontrado em `bin/`/`app/`; leitores externos/dinâmicos: [nao verificado]. |
| `SEM_JANELA_AUTH_MOTIVO` | Deploy não atribui; vem do ambiente, produtor real de produção: [nao verificado]. Testes forçam vazio (`bin/tests/test_deploy_reload_gracioso.sh:94`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:74`). | Deploy testa não vazio, anuncia motivo e evita chamar guarda (`bin/deploy.sh:140`, `bin/deploy.sh:141`). Esteira o135 lê e anuncia o mesmo override (`fatias_agendadas/o135-teto20/esteira.sh:68`, `fatias_agendadas/o135-teto20/esteira.sh:73`). |
| `SEM_SOMBRA_MOTIVO` | Sobrescrita ao iniciar pelo valor de `DEPLOY_SEM_SOMBRA`; se primeiro argumento for `--sem-sombra`, recebe `$2` (`bin/deploy.sh:82`, `bin/deploy.sh:83`, `bin/deploy.sh:84`). Portanto exportar apenas `SEM_SOMBRA_MOTIVO` não estabelece o motivo (`bin/deploy.sh:82`). | Validação de motivo e bloco que anuncia/grava override em lugar de sombra (`bin/deploy.sh:85`, `bin/deploy.sh:149`, `bin/deploy.sh:156`, `bin/deploy.sh:157`). Teste novo força vazio no ambiente, mas essa atribuição é sobrescrita pela linha 82 (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:74`, `bin/deploy.sh:82`). |
| `SO_CONFERIR` | Inicializa 0; muda a 1 apenas se `$1` for `--conferir` (`bin/deploy.sh:56`, `bin/deploy.sh:57`, `bin/deploy.sh:60`). | Envoltório auth/sombra, recusa mensageria, ramo de migration e retorno antecipado (`bin/deploy.sh:123`, `bin/deploy.sh:197`, `bin/deploy.sh:203`, `bin/deploy.sh:221`). |
| `AGENDADO` | Inicializa 0; vira 1 para `--reload-agendado`; `--reload-copia` também o coloca em 1 (`bin/deploy.sh:59`, `bin/deploy.sh:62`, `bin/deploy.sh:79`). | Sombra, recusa de migration, rótulo do reload e elegibilidade do histórico (`bin/deploy.sh:148`, `bin/deploy.sh:208`, `bin/deploy.sh:311`, `bin/deploy.sh:404`). Teste de orçamento injeta valor ao avaliar a condição extraída (`bin/tests/test_orcamento_contexto.sh:311`); teste auth procura `AGENDADO` no texto dos `if` que cercam a chamada (`bin/tests/test_janela_auth.sh:139`). |
| `COPIA` | Inicializa 0; vira 1 só em `--reload-copia`, junto de `AGENDADO=1` (`bin/deploy.sh:78`, `bin/deploy.sh:79`). | Sombra, recusa de migration, rótulo do reload e elegibilidade do histórico (`bin/deploy.sh:148`, `bin/deploy.sh:205`, `bin/deploy.sh:309`, `bin/deploy.sh:404`). Teste de orçamento injeta valor (`bin/tests/test_orcamento_contexto.sh:311`). |

Produtores da entrada `DEPLOY_SEM_SOMBRA`: moldes e integrador na recarga de cópia (`bin/molde_fatia/cadeia.sh:32`, `bin/molde_fatia/cadeia.sh:48`, `bin/molde_autocura/cadeia.sh:31`, `bin/molde_autocura/cadeia.sh:45`, `app/core/integrador_lote.py:638`). O deploy usa só o primeiro argumento como modo, mantendo o risco já existente de flags posteriores ignoradas (`bin/deploy.sh:56`, `bin/deploy.sh:83`).

## 08/11 22:47 · sessao acbe2fb35c2e · FIO-E0-V4-E1-FILA · verificador · origem=Code

**O delta fecha os defeitos pontuais de dia local e dos três sítios A1, mas não fecha a garantia global do repasse:** ainda há Chamado terminal e relações com outro colaborador alcançáveis depois da trava inicial de P (`app/chamados/veredito_lavrado.py:114`; `app/chamados/services/fio.py:1022`; `app/chamados/services/disputa_emissao.py:956`; `app/chamados/reconciliador.py:1617`; `app/chamados/services/disputa_emissao.py:1747`).

A execução das suites, os resultados RED→verde citados nos anexos e a ocorrência desses caminhos em produção são **[nao verificado]**. [nao verificado]

**1. Achados marcados CURADO**

| Achado | Conferência do delta |
|---|---|
| **1 — teto do repasse** | **Fecha a omissão por dia:** seleciona toda P de C, além das costuradas nos órfãos. **Não fecha o teto transitivo:** as cascatas alcançam outras P/D e Chamados terminais fora dessa seleção; caminhos detalhados abaixo (`app/chamados/services/disputa_emissao.py:956`; `app/chamados/services/disputa_emissao.py:1747`; `app/chamados/reconciliador.py:1617`). |
| **2 — local × UTC** | **Fecha nos dois leitores apontados:** ambos chamam `relogio_virou`, que converte hoje e a lavra para dia local. Não significa que todo leitor temporal do sistema passe por ela: o backfill mantém conta própria (`app/chamados/veredito_lavrado.py:114`; `app/chamados/veredito_lavrado.py:208`; `app/chamados/services/raiox.py:41`; `app/chamados/management/commands/backfill_veredito_lavrado.py:113`). |
| **4 — A1** | **Fecha os três sítios apontados:** backfill e contador usam diretamente o FK; rechaveamento usa `p.colaborador_id` para célula e despertar. A regressão desse terceiro sítio continua fora do selo novo (`app/chamados/management/commands/backfill_decisoes_pergunta.py:199`; `app/chamados/services/fio.py:1845`; `app/chamados/services/fio.py:1022`; `app/chamados/tests/test_e1_fila.py:87`). |
| **6-F1** | **Fecha a saída vazia identificada:** acrescenta segundo órfão sem P e exige retorno não nulo. Isso não comprova todos os ramos das cascatas (`app/chamados/tests/test_e1_fila.py:261`; `app/chamados/tests/test_e1_fila.py:269`; `app/chamados/tests/test_e1_fila.py:275`). |
| **6-F5** | **Fecha a fraqueza do relógio constante:** cada leitura avança um dia e o mutante retira o instante compartilhado. O juiz real continua mockado nesse teste (`app/chamados/tests/test_e1_fila.py:359`; `app/chamados/tests/test_e1_fila.py:364`; `app/chamados/tests/test_e1_fila.py:376`). |
| **6-A3 parcial** | **Continua parcial:** agora exige a D fechada na sequência, mas pergunta e declaração permanecem no mesmo `DIA`; não exercita o reuso que entra no rechaveamento por dia divergente (`app/chamados/tests/test_e1_fila.py:185`; `app/chamados/tests/test_e1_fila.py:194`; `app/chamados/tests/test_e1_fila.py:204`; `app/chamados/models.py:2938`). |
| **8 — escritores de destino** | **A documentação passa a mencionar SET_NULL**, correspondente à política declarada no FK. A expressão “só o banco” não comprova execução por ação SQL/trigger: isso é **[nao verificado]**. A conferência da mensagem do commit, “seis” versus “sete”, também é **[nao verificado]** (`app/chamados/services/fio.py:808`; `app/chamados/services/fio.py:814`; `app/chamados/models.py:3377`). |

**2. Conjunto do repasse e caminhos que escapam**

Chamando de **C** o colaborador recebido, **O** os órfãos e **P₀** as perguntas selecionadas, o conjunto inicialmente travado é este; a chamada ocorre dentro do atomic, antes do complemento e das demais escritas (`app/chamados/services/disputa_emissao.py:984`; `app/chamados/services/disputa_emissao.py:991`):

| Classe | Conjunto inicial |
|---|---|
| **C** | Somente C; os donos das D/Ch fornecidas explicitamente não são automaticamente acrescentados (`app/chamados/services/disputa_emissao.py:954`; `app/chamados/models.py:2663`). |
| **P₀** | `P.colaborador=C OU P.chamado_destino∈O` (`app/chamados/services/disputa_emissao.py:956`). |
| **D₀** | Todas as D abertas de C, mais as D de P₀, abertas ou fechadas (`app/chamados/services/disputa_emissao.py:958`; `app/chamados/models.py:2669`). |
| **Ch₀** | O, destinos de P₀, todos os Ch vivos de C e pais das D selecionadas que estejam abertas. O recolhimento desses pais não exige que a D pertença a C (`app/chamados/services/disputa_emissao.py:959`; `app/chamados/models.py:2673`; `app/chamados/models.py:2677`; `app/chamados/models.py:2680`). |

O conjunto **alcançável** precisa incluir as expansões abaixo, repetidas enquanto outra cascata encontrar novas linhas; a seleção inicial não calcula esse fechamento (`app/chamados/services/disputa_emissao.py:956`; `app/chamados/services/disputa_emissao.py:1827`; `app/chamados/models.py:1908`):

- **Corpo e cauda:** D da P encontrada por destino, D aberta/criada, perguntas criadas ou reusadas, órfãos analisados e perguntas dos dias extraídos do contexto da D (`app/chamados/services/disputa_emissao.py:1002`; `app/chamados/services/disputa_emissao.py:1013`; `app/chamados/services/disputa_emissao.py:1028`; `app/chamados/services/disputa_emissao.py:1202`; `app/chamados/services/disputa_emissao.py:278`; `app/chamados/services/disputa_emissao.py:294`).
- **Cartório:** outros Ch vivos reconciliados, P absolvidas do colaborador inteiro e D que essas retratações podem fechar (`app/chamados/signals.py:203`; `app/ponto/services/cartorio.py:1488`; `app/ponto/services/cartorio.py:1522`; `app/ponto/services/cartorio.py:958`; `app/chamados/reconciliador.py:584`; `app/chamados/reconciliador.py:612`).
- **Resolver/S143:** P encontradas pelo destino, suas D, todas as irmãs dessas D e respectivos pais; ao fechar uma D, também o C e o universo aberto do dono dela (`app/chamados/services/disputa_emissao.py:1747`; `app/chamados/services/disputa_emissao.py:1804`; `app/chamados/services/disputa_emissao.py:1827`; `app/chamados/services/disputa_emissao.py:1836`; `app/chamados/models.py:1908`).
- **Fechamento:** reconciliação síncrona do pai e carimbo de pendentes aprovadas da D. Retomada de regularizações e fechamento de becos estão em `on_commit`, portanto ficam fora desta cascata síncrona (`app/chamados/signals.py:42`; `app/chamados/signals.py:163`; `app/chamados/services/materializacao.py:1418`; `app/chamados/signals.py:144`; `app/chamados/signals.py:225`).

Há dois contraexemplos relevantes ao teto:

**a) Chamado terminal existente, com todos os donos consistentes.** A cauda pode criar P sem destino; `gerar_perguntas_disputa` chama `reatar_pergunta_orfa`, que pode reemitir o Chamado do dia. A busca não restringe a vivos; `Chamado.abrir` pode renascer um terminal existente (`app/chamados/services/disputa_emissao.py:444`; `app/chamados/services/disputa_emissao.py:459`; `app/chamados/reconciliador.py:1617`; `app/chamados/reconciliador.py:838`; `app/chamados/reconciliador.py:910`; `app/chamados/models.py:906`; `app/chamados/models.py:921`).

Se esse terminal não era órfão, pai de D aberta nem destino de P preexistente, ficou fora de Ch₀; o renascimento adquire sua trava posteriormente, embora o topo já tenha travado P₀ e a cauda já tenha inserido a nova P. Seus gates podem recusar casos concretos; ocorrência no dado: **[nao verificado]** (`app/chamados/models.py:2677`; `app/chamados/models.py:2680`; `app/chamados/models.py:3009`; `app/chamados/models.py:1019`; `app/chamados/models.py:1432`; `app/chamados/models.py:290`).

**b) D/Ch de outro colaborador por costura preexistente.** Considere ChX vivo de C, fora de O, com P2 de C2 apontando para ele. ChX entra por “vivos de C”, mas P2/D2 não entram pelo predicado de P₀. O cartório pode reconciliar ChX; o S143 busca P2 pelo destino **sem filtro de colaborador**, pode carimbá-la e segue D2, suas irmãs e seu pai Ch2 (`app/chamados/services/disputa_emissao.py:956`; `app/chamados/models.py:2680`; `app/ponto/services/cartorio.py:1488`; `app/chamados/signals.py:115`; `app/chamados/services/disputa_emissao.py:1747`; `app/chamados/services/disputa_emissao.py:1787`; `app/chamados/services/disputa_emissao.py:1804`; `app/chamados/services/disputa_emissao.py:1836`).

A reabertura de Ch2 ou o fechamento de D2 adquire C2 e seu universo depois das P já presas. O nascimento atual recusa nova costura cruzada, mas isso não elimina a leitura do acervo existente; existência dessa configuração em produção: **[nao verificado]** (`app/chamados/services/disputa_emissao.py:1812`; `app/chamados/services/disputa_emissao.py:1888`; `app/chamados/models.py:1908`; `app/chamados/models.py:2838`).

Portanto, **há linhas alcançáveis fora do conjunto travado antes da primeira escrita**, inclusive D/Ch de outro colaborador; a ampliação para toda P de C não resolve essas expansões reversas e transitivas (`app/chamados/services/disputa_emissao.py:956`; `app/chamados/services/disputa_emissao.py:1747`; `app/chamados/models.py:1908`).

**3. `janela_expira_em`, `julgado_em` e relógio**

| Operação | Escritores/leitores |
|---|---|
| **Derivação normal** | `lavrar`: amostra `_agora`, atribui `julgado_em`, calcula a janela com o mesmo instante e salva ambos (`app/chamados/veredito_lavrado.py:251`). |
| **Outro derivador/escritor** | Backfill: calcula a janela e atribui ambos; persiste pela porta de lote. É exceção nominal no contrato (`app/chamados/management/commands/backfill_veredito_lavrado.py:111`; `app/chamados/management/commands/backfill_veredito_lavrado.py:139`; `app/chamados/management/commands/backfill_veredito_lavrado.py:143`; `app/core/tests/test_contract_escritores_estado.py:74`). |
| **Restauração** | `desfazer_lavra_em_memoria` restaura a fotografia, inclusive ambos os campos (`app/chamados/veredito_lavrado.py:261`; `app/chamados/veredito_lavrado.py:268`). |
| **Persistência sem nova derivação** | Reuso, carimbo, reapontamento, rechaveamento e relavra recebem os valores de `lavrar` (`app/chamados/models.py:2996`; `app/chamados/models.py:3139`; `app/chamados/services/fio.py:923`; `app/chamados/services/fio.py:1033`; `app/chamados/services/relavra_via.py:126`). |
| **Leitores temporais principais** | `esta_stale` e varredura chamam `relogio_virou`; ambos também comparam marco com `julgado_em` (`app/chamados/veredito_lavrado.py:208`; `app/chamados/veredito_lavrado.py:212`; `app/chamados/services/raiox.py:41`; `app/chamados/services/raiox.py:44`). |
| **Outros leitores** | Fotografia de rollback, serialização de reversão do passivo e comparação persistido/recalculado no backfill (`app/chamados/veredito_lavrado.py:261`; `app/chamados/services/passivo_validado.py:64`; `app/chamados/services/passivo_validado.py:83`; `app/chamados/management/commands/backfill_veredito_lavrado.py:121`; `app/chamados/management/commands/backfill_veredito_lavrado.py:135`). |

As unidades são distintas e coerentes nos dois leitores corrigidos: `julgado_em` é **instante**, `janela_expira_em` é **dia**; para o ramo do relógio, o instante é convertido para dia local. A regra é `expira <= hoje` e `dia_local(lavra) < expira`: expiração inclusiva, lavra estritamente anterior; marco usa `marco > julgado_em` (`app/chamados/models.py:2588`; `app/chamados/models.py:2594`; `app/chamados/veredito_lavrado.py:114`; `app/chamados/veredito_lavrado.py:212`).

A janela viva inclui os dois extremos; `expira_em` procura o primeiro dia fora dela, compatível com invalidar quando `hoje >= expira` (`app/chamados/juizes.py:601`; `app/chamados/veredito_lavrado.py:141`; `app/chamados/veredito_lavrado.py:143`).

**Não há um único relógio para todo o ato.** O juiz ainda lê `date.today()` e é chamado antes de `lavrar` amostrar `_agora`; o teste F5 o substitui por mock. A configuração é São Paulo com timezone habilitado, mas a igualdade efetiva entre dia do processo e dia local em produção é **[nao verificado]** (`app/chamados/juizes.py:575`; `app/chamados/veredito_lavrado.py:242`; `app/chamados/veredito_lavrado.py:251`; `app/chamados/tests/test_e1_fila.py:364`; `app/config/settings/base.py:93`).

**Há outro leitor com conta própria:** o backfill lê `localdate()` e `now()` separadamente e calcula `e <= hoje`. Portanto pode atravessar meia-noite entre as amostras; ocorrência real: **[nao verificado]** (`app/chamados/management/commands/backfill_veredito_lavrado.py:94`; `app/chamados/management/commands/backfill_veredito_lavrado.py:95`; `app/chamados/management/commands/backfill_veredito_lavrado.py:113`).

Também permanece a ambiguidade anterior do campo: a descrição promete valor determinístico por evento, mas, para evento já expirado, `expira_em` devolve o dia da consulta/lavra, porque começa em `max(hoje, data_evento)` (`app/chamados/models.py:2597`; `app/chamados/veredito_lavrado.py:137`; `app/chamados/veredito_lavrado.py:142`).

**4. FILA e descartes**

- **Item 5 / A2 com chamador já travado: procedente quanto à ordem global.** O filtro de irmão/irmãs ocorre antes do atomic. Um irmão pode surgir antes da aquisição de C; depois de C/D/Ch/P presos, `pai.reabrir()` consulta novamente e entra em migração. Esta tenta adquirir todos os C das perguntas, inclusive outro colaborador, antes da recusa posterior de costura. Ocorrência em produção: **[nao verificado]** (`app/chamados/management/commands/reabrir_orfas_enterradas.py:105`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:146`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:153`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:172`; `app/chamados/models.py:1411`; `app/chamados/services/fio.py:2085`; `app/chamados/services/fio.py:2086`; `app/chamados/services/fio.py:2160`).
- **`UniversoMudouSobATrava` não fecha esse caso:** a conferência de crescimento vem depois da aquisição interna dos C; ela não desfaz preventivamente a ordem já imposta pelo chamador (`app/chamados/services/fio.py:2084`; `app/chamados/services/fio.py:2086`; `app/chamados/services/fio.py:2088`; `app/chamados/services/fio.py:2090`).
- **Fila 3/f5 é dívida real no caminho lido pela obra:** a cura de `expira_em` não torna único o relógio do julgamento completo. Pode ser adiada como desenho, mas não sustenta a afirmação de mesmo instante entre juiz e lavra (`app/chamados/juizes.py:569`; `app/chamados/juizes.py:575`; `app/chamados/veredito_lavrado.py:242`; `app/chamados/veredito_lavrado.py:251`).
- **A3 por dia diferente e A2 por crescimento continuam lacunas de prova:** o caso A3 permanece no mesmo dia; o cenário A2 monta dois colaboradores estáticos (`app/chamados/tests/test_e1_fila.py:146`; `app/chamados/tests/test_e1_fila.py:157`; `app/chamados/tests/test_e1_fila.py:185`; `app/chamados/tests/test_e1_fila.py:194`).
- **Descarte P3/P6/P7 é defensável:** os mutantes introduzem explicitamente P antes de C e exigem sequência inválida; P7 exige também três migrações. Não é necessário exigir uma mensagem textual específica para demonstrar essa mutação (`app/chamados/tests/test_e1_fila.py:311`; `app/chamados/tests/test_e1_fila.py:320`; `app/chamados/tests/test_e1_fila.py:333`; `app/chamados/tests/test_e1_fila.py:337`).
- **Rastreador:** ignorar `executemany` e trava de FK é limitação real da prova, não comprovação de outra violação concreta. Os verdes desse instrumento não demonstram todas as aquisições possíveis; um contraexemplo adicional especificamente nesses mecanismos é **[nao verificado]** (`app/chamados/tests/test_e1_v9.py:51`; `app/chamados/tests/test_e1_v9.py:69`).

A ordem exigida é por transação, inclusive entre colaboradores, com todos os C antes da primeira D; por isso a ausência de ocorrência demonstrada não elimina o caminho estático do item 5 (`app/chamados/models.py:2629`; `app/chamados/models.py:2652`; `app/chamados/tests/test_e1_v15g_portas.py:8`).

**5. Os testes mordem? Passam vazios?** [nao verificado]

| Teste | Avaliação estática | [nao verificado]
|---|---|
| **A1 por `values_list`** | Morde os dois sítios antigos: o censo real acusa chamadas com ambos os campos. **Não morde** a regressão direta `colab_id = p.disputa.colaborador_id`, pois só reconhece `or` ou chamadas com ambos os nomes (`app/chamados/tests/test_e1_fila.py:87`; `app/chamados/tests/test_e1_fila.py:98`; `app/chamados/tests/test_e1_fila.py:118`; `app/chamados/services/fio.py:1022`). |
| **Teto do repasse** | Não é vazio: cria P a vinte dias e exige sua P e D na sequência; o recorte anterior ±1 não inclui essa P. Contudo testa apenas `_trava_do_repasse`, não as cascatas nem outros colaboradores (`app/chamados/tests/test_e1_fila.py:292`; `app/chamados/tests/test_e1_fila.py:295`; `app/chamados/tests/test_e1_fila.py:297`; `app/chamados/tests/test_e1_fila.py:299`). |
| **F1 com segundo órfão** | Retira a vacuidade conhecida pelo retorno não nulo. O mutante morde a retirada da trava ampla; não demonstra, sozinho, necessidade da ampliação para qualquer dia (`app/chamados/tests/test_e1_fila.py:261`; `app/chamados/tests/test_e1_fila.py:275`; `app/chamados/tests/test_e1_fila.py:281`; `app/chamados/tests/test_e1_fila.py:285`). |
| **F5 relógio que anda** | Com evento já expirado, duas amostras produzem dias diferentes; a asserção de igualdade falha se `expira_em` voltar a ler sozinho. O mutante exige justamente essa diferença. Não prova o relógio do juiz real, que está mockado (`app/chamados/tests/test_e1_fila.py:359`; `app/chamados/tests/test_e1_fila.py:364`; `app/chamados/tests/test_e1_fila.py:372`; `app/chamados/tests/test_e1_fila.py:376`). |
| **F5 `date.today`** | Borda simulada em março contra mundo congelado em novembro distingue as fontes de relógio; o mutante deve divergir do resultado esperado (`app/chamados/tests/test_e1_fila.py:342`; `app/chamados/tests/test_e1_fila.py:350`; `app/chamados/tests/test_e1_fila.py:355`; `app/chamados/tests/test_e1_fila.py:383`). |
| **DiaLocalEstaStale** | Dois casos efetivos: antes da meia-noite local espera False; depois, com lavra na noite anterior, True. As contas UTC anteriores dão os resultados contrários (`app/chamados/tests/test_e1_fila.py:393`; `app/chamados/tests/test_e1_fila.py:401`; `app/chamados/tests/test_e1_fila.py:410`). |
| **DiaLocalVarredura** | Cria uma P, atualiza os campos e consulta sua PK; o caso esperado True impede passar com queryset vazio (`app/chamados/tests/test_e1_fila.py:420`; `app/chamados/tests/test_e1_fila.py:422`; `app/chamados/tests/test_e1_fila.py:425`; `app/chamados/tests/test_e1_fila.py:426`). |
| **A3 alterado** | A nova asserção prova presença da D fechada; não prova execução do rechaveamento por dia divergente (`app/chamados/tests/test_e1_fila.py:194`; `app/chamados/tests/test_e1_fila.py:204`; `app/chamados/models.py:2938`). |

Os mutantes falham explicitamente quando sua âncora não existe, evitando montagem silenciosamente inoperante; execução das suites nesta auditoria: **[nao verificado]** (`app/chamados/tests/test_e1_fila.py:47`; `app/chamados/tests/test_e1_fila.py:50`).

**6. Classes que já reprovaram obra**

- **Ordem de trava:** permanece nos caminhos de terminal e de outro colaborador descritos acima; é a principal insuficiência do teto alterado (`app/chamados/reconciliador.py:1617`; `app/chamados/models.py:921`; `app/chamados/services/disputa_emissao.py:1747`; `app/chamados/models.py:1908`).
- **Selo sem cobertura da cura inteira:** o novo selo morde os aliases, mas deixa passar a reversão do rechaveamento para o colaborador da D (`app/chamados/tests/test_e1_fila.py:87`; `app/chamados/tests/test_e1_fila.py:98`; `app/chamados/services/fio.py:1022`).
- **Relógio fora da borda compartilhada:** continua no juiz alcançado por `lavrar`; o delta não introduz essa dívida, mas também não a elimina (`app/chamados/juizes.py:575`; `app/chamados/veredito_lavrado.py:242`; `app/chamados/veredito_lavrado.py:251`).
- **Derivação paralela:** os três sítios A1 apontados foram corrigidos; permanece a reprodução preexistente da regra de janela em `expira_em` e no juiz (`app/chamados/management/commands/backfill_decisoes_pergunta.py:199`; `app/chamados/services/fio.py:1022`; `app/chamados/services/fio.py:1847`; `app/chamados/veredito_lavrado.py:120`; `app/chamados/juizes.py:601`).
- **Segundo escritor:** o backfill continua atribuindo os campos fora de `lavrar`, com exc

[truncado em 20000 de 20745 caracteres]

## 08/11 22:44 · sessao 2858cbb65143 · - · - · origem=Ronald (tmux)

3. **Relógio e campos**

- `julgado_em` é `DateTimeField`; `janela_expira_em`, `DateField` (`app/chamados/models.py:2588`, `app/chamados/models.py:2594`). A porta `lavrar` atribui ambos a partir de `_agora=timezone.now()`, passando esse instante a `expira_em`; salva juntos (`app/chamados/veredito_lavrado.py:251-255`).
- Há outro atribuidor: o backfill obtém `hoje=timezone.localdate()` e `agora=timezone.now()` em leituras distintas, calcula `e=expira_em(..., agora=agora)` e atribui os dois campos; persiste por `gravar_lavras_em_lote` (`app/chamados/management/commands/backfill_veredito_lavrado.py:94-95`, `:111`, `:139-146`; `app/chamados/veredito_lavrado.py:220-230`). Essa exceção está registrada no contrato (`app/core/tests/test_contract_escritores_estado.py:74-84`).
- A reversão em memória também atribui ambos, restaurando a fotografia anterior; o chamador é o tratamento de falha de `carimbar` (`app/chamados/veredito_lavrado.py:259-269`; `app/chamados/models.py:3137-3145`).
- Persistências que recebem valores de `lavrar`, sem nova derivação: nascimento/reuso em `models.py:2996-2999`, carimbo em `models.py:3139-3142`, reapontamento em `services/fio.py:923-925`, rechaveamento em `services/fio.py:1033-1034`, relavra em `services/relavra_via.py:126-128` (todos sob `app/chamados/`).
- Os leitores temporais principais convergem: `esta_stale` chama `relogio_virou`; a varredura faz o mesmo, com um instante para o lote (`app/chamados/veredito_lavrado.py:204-212`; `app/chamados/services/raiox.py:31-44`). `relogio_virou` usa **dia local**, invalida com `expira <= hoje` e exige `dia_local(julgado_em) < expira`; portanto igualdade da lavra com expiração não invalida (`app/chamados/veredito_lavrado.py:112-114`). Os marcos usam comparação de instantes estrita `marco > julgado_em` (`app/chamados/veredito_lavrado.py:212`; `app/chamados/services/raiox.py:44`).
- Outros leitores diretos: fotografia para rollback (`app/chamados/veredito_lavrado.py:261`), serialização da reversão do passivo (`app/chamados/services/passivo_validado.py:64-66`, `:81-83`, `:367`), e backfill, que compara o campo persistido ao recalculado (`app/chamados/management/commands/backfill_veredito_lavrado.py:121`, `:135`).
- **Existe outra conta de dia no backfill:** `e <= hoje`, usando seu próprio `hoje`; não é a implementação completa de `relogio_virou`, mas impede afirmar “todo leitor temporal passa por ela” (`app/chamados/management/commands/backfill_veredito_lavrado.py:94-95`, `:111-113`). Cruzamento de meia-noite entre essas duas leituras é caminho de código; ocorrência real [nao verificado].
- A cura não alcança o relógio do juiz: `_dentro_da_janela_viva(data_evento)` não recebe instante e usa `date.today()` (`app/chamados/juizes.py:569`, `:575`). A lavra chama o juiz **antes** de amostrar seu `_agora` (`app/chamados/veredito_lavrado.py:242-253`); há ramos do juiz que consultam essa janela (`app/chamados/juizes.py:674`, `:764`, `:1217`). Igualdade do fuso efetivo do processo em produção [nao verificado]; configuração da cópia: `America/Sao_Paulo`, `USE_TZ=True` (`app/config/settings/base.py:93-95`).
- A janela viva mantém limites inclusivos, enquanto sua expiração é o primeiro dia fora dela (`app/chamados/juizes.py:601`; `app/chamados/veredito_lavrado.py:137-143`). `janela_expira_em` não é estritamente função fixa do evento: para evento já expirado retorna `hoje`; isso contradiz a descrição “Deterministico por data_evento”, mas precede o delta (`app/chamados/models.py:2597`; `app/chamados/veredito_lavrado.py:137-143`; `app/chamados/management/commands/backfill_veredito_lavrado.py:118`).

5. **Testes: mordida e limites** [nao verificado]

| Teste | Avaliação estática | [nao verificado]
|---|---|
| A1 por `values_list` | O detector agora acusa os dois campos na mesma chamada; o RED sobre fontes reais falha com os dois sítios antigos (`app/chamados/tests/test_e1_fila.py:97-119`). A mordida usa exemplo literal com os dois campos (`:129-138`). |
| Limite A1 | **A reversão de `rechavear` para `colab_id=p.disputa.colaborador_id` continua invisível ao selo:** não é `or` nem chamada `values` com os dois campos (`app/chamados/tests/test_e1_fila.py:87-101`; cura em `app/chamados/services/fio.py:1022`). Executei apenas os dois helpers extraídos por AST, em Python com `-B`: essa atribuição retorna `[]`; nenhuma suite Django executada. |
| Teto do repasse | Cria P vinte dias distante, exige P e sua D na sequência; falha com o recorte ±1 anterior (`app/chamados/tests/test_e1_fila.py:287-300`). Não é teste das cascatas nem de Ch/C de outro colaborador: chama somente `_trava_do_repasse` e verifica P/D (`:296-300`). |
| F1 segundo órfão | Introduz órfão sem P, passa os dois à porta e exige retorno não nulo, retirando a saída prematura que esvaziava os laços (`app/chamados/tests/test_e1_fila.py:261-277`). Seu mutante remove a trava ampla e exige violação (`:279-285`); o teste específico da ampliação por todos os dias é o do teto (`:287-300`). |
| F5 mesmo instante | Relógio avança um dia por leitura; juiz é mockado para `False`; exige expiração no dia local da lavra (`app/chamados/tests/test_e1_fila.py:357-372`). Mutante retira `agora=_agora`, e a desigualdade exigida demonstra a diferença entre duas amostragens (`:374-379`). Não exercita o relógio real do juiz, por causa do mock (`:364`). |
| F5 borda/date.today | Congela mundo em novembro, faz `timezone.now` devolver março e exige março; mutante devolve relógio `date.today()` e deve divergir (`app/chamados/tests/test_e1_fila.py:342-355`, `:381-384`). |
| DiaLocalEstaStale | Dois casos não vazios: antes da meia-noite local deve dar False; depois, com lavra na noite anterior, True. As comparações UTC antigas produzem os resultados opostos (`app/chamados/tests/test_e1_fila.py:393-411`). |
| DiaLocalVarredura | Cria P real, atualiza ambos os campos para cada caso, consulta queryset contendo sua PK e exige presença/ausência exata (`app/chamados/tests/test_e1_fila.py:420-426`). Não passa com queryset vazio no caso esperado True (`:426`). |
| A3 parcial | Exige agora D fechada na sequência, além da P e Ch (`app/chamados/tests/test_e1_fila.py:199-206`). Continua mesmo DIA: P nasce no DIA padrão e mocks devolvem DIA (`:185`, `:194-196`; padrão em `app/chamados/tests/test_e1_v9.py:145-147`); ramo de rechaveamento por dia diferente não ganhou caso (`app/chamados/services/fio.py:1012-1013`). |

Resultado de execução das suites, RED anterior e verde atual: [nao verificado]. O montador de mutantes falha explicitamente se a âncora da cura não existe, evitando erro silencioso de montagem (`app/chamados/tests/test_e1_fila.py:47-55`).

6. **Classes relacionadas ao delta**

- `relogio_virou` centraliza o predicado já duplicado nos dois leitores; não cria segundo juiz para decidir se a janela viva contém o evento, pois seu predicado é invalidação da lavra persistida (`app/chamados/veredito_lavrado.py:105-114`; `app/chamados/juizes.py:569-601`).
- Continuam, nos sítios lidos pelo delta, relógio independente no juiz, escritor excepcional do backfill e conta temporal própria desse backfill; todos já existiam, com referências acima (`app/chamados/juizes.py:575`; `app/chamados/management/commands/backfill_veredito_lavrado.py:94-113`, `:139-146`).
- O selo novo tem mordida para a regressão por `values_list`, porém não protege a terceira cura A1, a atribuição direta de `rechavear`; isso é limitação concreta de cobertura, sem inferir ocorrência de dados divergentes (`app/chamados/tests/test_e1_fila.py:87-101`; `app/chamados/services/fio.py:1022`).

## 08/11 22:44 · sessao 5d1428d82aec · - · - · origem=Ronald (tmux)

O teto mudou de “dias ±1” para `P.colaborador=C ∪ P.destino∈órfãos`; isso fecha a omissão temporal específica, mas não demonstra cobertura transitiva de todas as cascatas (`app/chamados/services/disputa_emissao.py:954-959`).

**Conjunto efetivamente travado antes da primeira escrita:**

- **C:** somente o colaborador recebido (`app/chamados/services/disputa_emissao.py:954`; `app/chamados/models.py:2660-2664`).
- **D:** todas as abertas de C, mais as disputas das P selecionadas, abertas ou fechadas (`app/chamados/services/disputa_emissao.py:956-959`; `app/chamados/models.py:2669-2674`).
- **Ch:** órfãos recebidos, destinos das P selecionadas, todos os vivos de C e pais das D selecionadas que estejam abertas; esse último recolhimento não verifica o dono da D (`app/chamados/services/disputa_emissao.py:958-959`; `app/chamados/models.py:2673-2687`).
- **P:** somente as selecionadas pelo OR acima; ordenar e travar seus próprios registros não expande irmãs nem destinos reversos (`app/chamados/services/disputa_emissao.py:956-959`; `app/chamados/models.py:2688-2691`).

**O conjunto alcançável é maior:**

- O corpo complementa D da primeira P encontrada por destino, cria ou atualiza D de C, cria/reusa P e analisa órfãos; a seleção inicial da P não exige que ela ou sua D pertençam a C (`app/chamados/services/disputa_emissao.py:1002-1017`, `1028-1064`, `1145-1155`, `1176-1188`).
- A cauda lê batidas nomeadas pelo contexto da D, gera perguntas de outros dias, modifica o contexto e pode fechar a D (`app/chamados/services/disputa_emissao.py:278-296`, `1202-1219`).
- `analisar` grava estado e dispara síncronamente o cartório; o cartório pode reconciliar outros chamados vivos do dia, retratar P de todo o colaborador e executar a fábrica (`app/chamados/models.py:800-806`, `289-296`; `app/chamados/signals.py:187-203`; `app/ponto/services/cartorio.py:1488`, `1501`, `1522`, `958-963`, `985-997`; `app/chamados/reconciliador.py:584-614`).
- O resolver por chamado segue todas as P ancoradas nele, sem filtro por dono; depois segue suas D, todas as P dessas D, seus pais e o universo aberto do dono da D quando `fechar` toma suas travas (`app/chamados/services/disputa_emissao.py:1747-1751`, `1787-1791`, `1804-1812`, `1827-1845`, `1861`, `1875-1883`; `app/chamados/models.py:1908-1910`).
- Fechar D reconcilia o pai pelo signal e materializa P aprovadas de toda a D; são chamadas síncronas. Retomar regularizações e fechar becos são `on_commit` e ficam fora desse conjunto síncrono (`app/chamados/signals.py:41-45`, `162-163`, `144`, `225`; `app/chamados/services/materializacao.py:1418-1432`).

**Caminhos que continuam fora do teto:**

1. **Ch terminal existente, mesmo colaborador e sem nenhuma divergência de dono.** Uma P nova da cauda pode nascer sem destino e chamar `reatar_pergunta_orfa`; este pode reemitir um chamado do dia já encerrado. `Chamado.abrir` busca todos os estados e renasce o encerrado, que não estava no teto se não era órfão, pai de D aberta nem destino de P preexistente (`app/chamados/services/disputa_emissao.py:444-464`; `app/chamados/reconciliador.py:1582-1591`, `1617`, `834-839`, `910-911`; `app/chamados/models.py:906-924`; teto: `2677-2687`). A aquisição de Ch ocorre depois da inserção da nova P; o renascimento chama `reabrir`, que grava estado e ressuscita perguntas (`app/chamados/models.py:3009`, `1019`, `1432-1441`, `289-291`). Os gates podem recusar um caso concreto; ocorrência no dado [nao verificado].

2. **D/Ch de outro colaborador por costura preexistente.** Considere ChX vivo de C, fora da lista de órfãos, com P2 de C2 apontando para ele: o teto inclui ChX, mas exclui P2/D2. O cartório despertado por outro órfão pode reconciliar ChX; S143 busca P2 pelo destino, carimba-a e percorre D2 e suas irmãs. Pode reabrir seu pai Ch2 e fechar D2, adquirindo C2 e seu universo depois das P já travadas/escritas (`app/chamados/services/disputa_emissao.py:956-959`; `app/chamados/models.py:2680-2687`; `app/chamados/signals.py:114-115`; `app/ponto/services/cartorio.py:1488`, `1501`, `1522`; `app/chamados/services/disputa_emissao.py:1747-1751`, `1787`, `1804-1812`, `1827-1861`, `1875-1883`; `app/chamados/models.py:1908-1910`). O nascimento atual recusa destino de outro colaborador, mas isso não elimina o caminho de leitura/escrita sobre acervo já costurado; existência dessa configuração no dado [nao verificado] (`app/chamados/models.py:2837-2838`; `app/chamados/services/disputa_emissao.py:1747-1751`).

3. **Entrada órfã de outro colaborador.** A porta não confere o dono dos chamados recebidos antes de travá-los explicitamente; um `analisar` posterior toma C e o universo do dono efetivo do chamado (`app/chamados/services/disputa_emissao.py:980-991`, `997-1017`; `app/chamados/models.py:289-291`, `800-806`). Ocorrência entre os chamadores/prod [nao verificado].

A checagem de igualdade entre `P.colaborador` e o dono da D roda apenas no nascimento; portanto ela não prova consistência de todo o acervo lido pelas cascatas (`app/chamados/models.py:3410-3425`). A cobertura exata exigiria o fechamento transitivo das relações efetivamente seguidas — destinos reversos, D das P alcançadas, irmãs, pais e universos dos novos donos — além dos terminais que a reemissão pode renascer; `_trava_do_repasse` faz somente uma expansão (`app/chamados/services/disputa_emissao.py:954-959`; caminhos: `1747-1883`; `app/chamados/reconciliador.py:910-911`; `app/chamados/models.py:906-924`).

## 08/11 22:44 · sessao 00b27372de1d · - · - · origem=Ronald (tmux)

**Descartes/FILA que merecem ressalva**

- **Item 5/A2, chamador já travado: caminho continua aberto.** O command filtra `irmao_vivo`/irmãs antes da transação; depois trava C/D/Ch/P e chama novamente `pai.reabrir()`. Se um irmão surgir entre filtro e trava, `reabrir()` entra em migração; esta coleta perguntas dos dois chamados e tenta travar seus colaboradores, inclusive outro colaborador, depois das linhas já presas pelo command. A recusa de destino cruzado na costura ocorre posteriormente e não impede essa aquisição de C. Ocorrência em produção: [nao verificado]. (`app/chamados/management/commands/reabrir_orfas_enterradas.py:105`, `:108`, `:146`, `:153`, `:172`; `app/chamados/models.py:1390`, `:1411`; `app/chamados/services/fio.py:2037`, `:2085`, `:2086`, `:2094`, `:2160`.)
- **A recusa `UniversoMudouSobATrava` não cura o chamador previamente travado:** ela verifica crescimento entre as duas leituras internas da migração; a primeira aquisição interna de C já aconteceu antes dessa conferência. (`app/chamados/services/fio.py:2084`, `:2086`, `:2088`, `:2090`.)
- **Fila 3/f5 é dívida real preexistente:** `_dentro_da_janela_viva` continua lendo `date.today()` sem parâmetro; `expira_em` reproduz sua guarda com um dia obtido por `timezone.localdate`. O novo `relogio_virou` centraliza somente invalidação do cache; não é novo juiz de mérito. A autoridade registrada continua `pergunta_viva`. (`app/chamados/juizes.py:569`, `:575`, `:601`; `app/chamados/veredito_lavrado.py:102`, `:105`, `:114`, `:120`, `:137`, `:141`; `app/core/juizes.py:266`.)
- **A3 parcial continua parcial:** o teste exige agora a D fechada, mas prepara a pergunta e a declaração para `DIA`; não exercita o ramo de reuso com data divergente que chama rechaveamento. (`app/chamados/tests/test_e1_fila.py:185`, `:194`, `:196`, `:204`; `app/chamados/models.py:2938`, `:2945`.)
- **Não confirmei cascata efetiva A3→cartório.** Existe a ligação genérica de `post_save` de Chamado ao cartório, mas depende de mudança de status e dia forte; o pai criado pela declaração recebe somente `origem`, sem dia forte. Assim, usar essa cascata específica como reprovação exige mais caminho: [nao verificado]. (`app/chamados/signals.py:191`, `:192`, `:201`, `:202`, `:203`; `app/chamados/services/disputa_emissao.py:630`; `app/chamados/juizes.py:98`, `:117`, `:127`.)

**Testes e instrumento** [nao verificado]

- O descarte de exigir identificação textual da violação P3/P6/P7 é defensável: os mutantes introduzem explicitamente trava de P antes de C, e o contrato exige sequência inválida; P7 também exige três migrações. Isso não prova execução verde nesta auditoria: [nao verificado]. (`app/chamados/tests/test_e1_fila.py:311`, `:314`, `:319`, `:324`, `:332`, `:337`, `:338`.)
- O rastreador realmente ignora `executemany` e travas de FK. Portanto seus verdes não provam a ordem de **todas** as aquisições possíveis; não encontrei nesta subtarefa um caminho concreto adicional que explore esses dois limites: [nao verificado]. (`app/chamados/tests/test_e1_v9.py:51`, `:69`, `:72`; `app/chamados/tests/test_e1_fila.py:62`.)

**Classes/f7**

- O contrato exige ordem por transação, conjunto completo no topo e cada classe por PK; logo o caminho concorrente do command acima pertence ao escopo, apesar de a ocorrência não estar demonstrada. (`app/chamados/models.py:2629`, `:2633`, `:2634`.)
- A documentação f7 agora enumera a escrita do nascimento/reuso e a limpeza por `SET_NULL`; o campo efetivamente declara essa política. A afirmação literal “só o banco” não demonstra trigger ou `ON DELETE SET NULL` no DDL: isso está [nao verificado]. (`app/chamados/services/fio.py:808`, `:811`, `:813`; `app/chamados/models.py:3375`, `:3377`.)
- O contrato de escritores continua listando `models.py` e `fio.py`; o delta de prosa não elimina nem acrescenta escritor próprio de aplicação. (`app/core/tests/test_contract_escritores_estado.py:61`; `app/chamados/services/fio.py:808`.)

## 08/11 22:27 · sessao ec61578336e4 · FIO-E0-V4-E1-FILA · verificador · origem=Code

**A ordem global ainda não está demonstrada para o atomic inteiro:** o repasse pode alcançar perguntas fora da pré-trava, e a migração pode receber travas já mantidas pelo chamador. (`app/chamados/services/disputa_emissao.py:955`, `:1206`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:153`, `:172`)

Resultados RED/GREEN, migrations aplicadas, contagens e ocorrência dos cenários em produção: **[nao verificado]**. [nao verificado]

1. **ALTA — O novo “teto” do repasse não cobre tudo que a mesma transação pode escrever.** A pré-trava inclui perguntas costuradas nos órfãos e perguntas do colaborador nos dias dos órfãos ±1 ou sem dia. A cauda chama `perguntar_entradas_pendentes`, que obtém outros dias do contexto da disputa e chama a geração de perguntas. (`app/chamados/services/disputa_emissao.py:955`, `:958`, `:1206`, `:278`, `:283`, `:294`)

   Um caminho permitido pelo código é: P de pk maior pré-travada no dia do órfão; entrada pendente de outro dia, fora do recorte; reuso de P canônica antiga desse outro dia; atribuição de célula/destino e **UPDATE dessa P de pk menor depois da maior**. O reuso procura a canônica globalmente e salva as alterações. Reprodução e ocorrência no acervo: [nao verificado]. (`app/chamados/services/disputa_emissao.py:443`; `app/chamados/models.py:2905`, `:2919`, `:2958`, `:2975`, `:2999`)

   Há outra expansão pela cascata síncrona: `analisar → _gravar_estado/save → post_save → cartório`; havendo adiamentos pela lâmpada, o cartório chama a retratação das perguntas absolvidas **do colaborador inteiro**, sem recorte de dia. Ela pode atualizar P antiga que não entrou no teto. (`app/chamados/services/disputa_emissao.py:1021`; `app/chamados/models.py:806`, `:296`; `app/chamados/signals.py:203`; `app/ponto/services/cartorio.py:951`, `:959`; `app/chamados/reconciliador.py:584`, `:595`, `:627`)

2. **ALTA — A data de expiração continua com contratos de fuso incompatíveis.** O escritor usa `localdate(agora)`; `esta_stale` compara `agora.date()` e `julgado_em.date()`, e a varredura repete essas comparações. A configuração declara São Paulo e `USE_TZ=True`. Portanto, com instantes UTC, escritor e leitores podem estar em dias diferentes. É problema preexistente mantido pelo diff. (`app/chamados/veredito_lavrado.py:103`, `:126`, `:196`, `:199`; `app/chamados/services/raiox.py:36`, `:41`; `app/config/settings/base.py:93`, `:95`)

   A consequência pode ultrapassar algumas horas: se a lavra ocorrer antes da meia-noite local, mas seu dia UTC já for a data de expiração, a condição `julgado_em.date() < janela_expira_em` fica falsa. Esse ramo deixa de invalidar a lavra quando o dia local alcançar a expiração. É uma consequência das condições; ocorrência real: [nao verificado]. (`app/chamados/veredito_lavrado.py:196`, `:199`)

3. **MÉDIA — f5 une o instante do carimbo ao da expiração, mas não ao julgamento.** `lavrar` chama o juiz antes de capturar `_agora`; o juiz ainda lê `date.today()` independentemente. O backfill captura `hoje` e `agora` separadamente e executa o juiz novamente por linha. Logo, “mesmo instante” vale para `julgado_em` e `expira_em`, não para a decisão inteira. (`app/chamados/veredito_lavrado.py:234`, `:242`, `:244`; `app/chamados/juizes.py:575`; `app/chamados/management/commands/backfill_veredito_lavrado.py:94`, `:95`, `:105`)

4. **MÉDIA — A1 elimina a sintaxe escolhida, não toda derivação paralela.** Permanecem os equivalentes com aliases: `c or dc` em `fecho_cruza_dias` e no backfill de decisões. O detector AST procura diretamente `.disputa.colaborador[_id]` como alternativa de `or`, sem resolver aliases. (`app/chamados/services/fio.py:1844`, `:1847`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:199`; `app/chamados/tests/test_e1_fila.py:76`, `:86`)

   Também permanece uma identidade dividida no rechaveamento: trava/dedup usam `p.colaborador_id`, mas célula nova e despertar usam `p.disputa.colaborador_id`. A igualdade é verificada no nascimento, sem nova conferência nesse `save` de linha existente. Divergência efetiva no acervo: [nao verificado]. (`app/chamados/services/fio.py:1000`, `:1021`, `:1022`, `:1024`, `:1035`; `app/chamados/models.py:3410`, `:3423`)

5. **MÉDIA — A2 ordena sua própria chamada, não o prefixo transacional de todos os chamadores.** `reabrir_orfas_enterradas` já mantém C/D/Ch/P quando chama `pai.reabrir`; o ramo `irmao_vivo` chama a migração, cujo conjunto pode acrescentar C de outra pergunta. Isso permite aquisição de C depois de D/Ch/P já seguros. (`app/chamados/management/commands/reabrir_orfas_enterradas.py:146`, `:153`, `:172`; `app/chamados/models.py:1411`, `:1414`; `app/chamados/services/fio.py:2037`, `:2085`, `:2086`)

   **A ressalva é necessária:** o pré-filtro desse comando já exclui `irmao_vivo`. O caminho exige mudança entre o pré-filtro e a trava — por exemplo, surgimento de irmão nesse intervalo — ou mudança posterior da decisão. Acervo cruzado sozinho não demonstra esse caminho; ocorrência: [nao verificado]. (`app/chamados/management/commands/reabrir_orfas_enterradas.py:105`, `:108`, `:153`)

6. **MÉDIA — Os testes novos têm cobertura útil, mas algumas alegações excedem o que verificam.** [nao verificado]

   | Teste | O que efetivamente demonstra | [nao verificado]
   |---|---|
   | **A1** | Varre produção e detecta os exemplos sintáticos declarados; pode passar com os aliases sobreviventes do achado 4. (`app/chamados/tests/test_e1_fila.py:99`, `:109`, `:122`; `app/chamados/services/fio.py:1847`) |
   | **A2** | Não é vazio: exige os dois colaboradores e detecta a aquisição adicional fora de ordem. Não testa crescimento do universo entre leitura e trava, nem entrada por chamador já travado. (`app/chamados/tests/test_e1_fila.py:147`, `:151`, `:154`; `app/chamados/services/fio.py:2088`) |
   | **A3** | **Morde por motivo diferente do anunciado para a Disputa fechada:** fixture e pedido usam o mesmo dia; isso pula o ramo que trava a Disputa existente por rechaveamento. O cenário exercita Ch destino de pk menor depois de Ch vivo maior. Não exige a Disputa fechada na sequência. (`app/chamados/tests/test_e1_fila.py:173`, `:175`, `:183`, `:191`, `:195`; `app/chamados/tests/test_e1_v9.py:145`; `app/chamados/models.py:2938`; `app/chamados/services/fio.py:768`) |
   | **F1** | Não é vazio: verifica o complemento inicial. Porém o único chamado já tem P pendente; após complementar, ele é eliminado pelo filtro e a porta retorna `None`. Não alcança os laços principais, entradas pendentes ou fecho. O retorno é descartado. (`app/chamados/tests/test_e1_fila.py:229`, `:237`, `:254`; `app/chamados/services/disputa_emissao.py:1006`, `:1022`, `:1029`) |
   | **P3/P6/P7** | Injetam P antes de C e exigem alguma violação; P7 também exige `migradas=3`. Mordem essa inversão, mas não identificam especificamente qual violação matou o mutante. (`app/chamados/tests/test_e1_fila.py:273`, `:282`, `:295`, `:302`) |
   | **F5** | Os testes com datas diferentes distinguem o relógio da borda de `date.today()`. O teste “MESMO instante” devolve uma constante de `now`; duas leituras independentes devolveriam os mesmos valores e passariam. (`app/chamados/tests/test_e1_fila.py:307`, `:319`, `:322`, `:330`) |

   O rastreador observa **UPDATE simples e FOR UPDATE**, elimina reacessos e verifica rank/pk. Não observa `executemany` nem travas de FK; verifica a presença de algum C anterior, sem comprovar que seja o dono de cada linha. `_travar` captura somente a chamada isolada e exclui pks nascidos durante ela. (`app/chamados/tests/test_e1_v9.py:47`, `:68`, `:86`, `:95`, `:109`; `app/chamados/tests/test_e1_fila.py:57`, `:65`)

7. **INFORMATIVA — `colaborador_id=None`: possível em memória; não demonstrado em linha persistida compatível com o esquema.** O FK atual é NOT NULL; a migration preenche o acervo pela Disputa antes de impor essa restrição. Aplicação real dessa migration e estado de produção: [nao verificado]. (`app/chamados/models.py:2431`; `app/chamados/migrations/0051_decisao_pergunta.py:25`, `:101`)

   `_nascer` monta a instância final sem colaborador explícito; o preenchimento acontece em `save → _chave_de_identidade`. Já o stub consultado antes do nascimento e o proxy do juiz recebem `colaborador_id` explicitamente. Não identifiquei nesse caminho uma leitura alterada que dependa do FK ainda ausente. (`app/chamados/models.py:3006`, `:3009`, `:3410`, `:3420`, `:3422`, `:2874`, `:2884`)

   `lavrar_sem_turno_aberto` passa a usar `p.colaborador` como alternativa, mas seu chamador produtivo localizado fornece `colab` explicitamente. (`app/chamados/reconciliador.py:368`, `:1615`)

O mapa dos campos mostra estes escritores, leitores e significados:

| Campo | Quem escreve | Quem lê; significado |
|---|---|---|
| `P.colaborador_id` | `_chave_de_identidade` no nascimento; preenchimento do acervo na migration. (`app/chamados/models.py:3410`, `:3422`; `app/chamados/migrations/0051_decisao_pergunta.py:25`) | Dedup/fato livre, chave, costura, migração e juiz de destino usam o FK da pergunta. É identidade por pk; os leitores pela Disputa e os aliases ainda sobrevivem. (`app/chamados/models.py:2752`, `:2764`; `app/chamados/services/fio.py:1449`, `:862`, `:2085`; `app/chamados/juizes.py:2003`) |
| `P.chamado_destino` | `_nascer` cria/reusa; `reapontar_resolvedora` recostura. `_costura_nao_mata_o_fio` atribui temporariamente e restaura. (`app/chamados/models.py:2975`, `:3006`; `app/chamados/services/fio.py:921`, `:779`, `:783`) | Materialização, crédito destino-ou-pai e guardas do fio. É FK opcional para o chamado resolvedor, sujeita ao juiz de dono/turno/estado. (`app/chamados/services/materializacao.py:339`; `app/chamados/services/raiox.py:54`, `:60`; `app/chamados/services/fio.py:906`, `:908`) |
| `P.julgado_em` | `lavrar`; backfill atribui e persiste pelo gravador em lote. (`app/chamados/veredito_lavrado.py:243`, `:221`; `app/chamados/management/commands/backfill_veredito_lavrado.py:139`) | Stale, varredura e foto da lavra. É **instante**, comparado estritamente com marcos posteriores; a conversão para dia causa a divergência do achado 2. (`app/chamados/veredito_lavrado.py:193`, `:199`, `:203`, `:252`; `app/chamados/services/raiox.py:41`, `:44`) |
| `P.janela_expira_em` | `lavrar` e backfill, mediante `expira_em`; restauração da foto apenas em memória. (`app/chamados/veredito_lavrado.py:244`, `:260`; `app/chamados/management/commands/backfill_veredito_lavrado.py:140`) | Stale, varredura e comparação do backfill. É **date**, primeiro dia inválido a partir do probe: validade termina exclusivamente nessa data; stale usa expiração inclusiva, `<= hoje`. Para fato já expirado, a função pode devolver o próprio dia do ato. (`app/chamados/veredito_lavrado.py:126`, `:130`, `:196`; `app/chamados/services/raiox.py:41`; `app/chamados/management/commands/backfill_veredito_lavrado.py:121`) |
| `P.data_evento` | Atribuição central em `gravar_dia_do_fato`; nascimento, costura que data e rechaveamento chamam esse ponto. (`app/chamados/services/fio.py:800`, `:878`, `:1027`; `app/chamados/models.py:3008`) | Dedup, identidade, materialização, juiz do chamado e expiração. **Já tem dois significados discriminados:** dia do turno para fato; dia civil local do pedido para conversa. (`app/chamados/models.py:2781`, `:2794`, `:2799`; `app/chamados/services/materializacao.py:345`; `app/chamados/juizes.py:75`; `app/chamados/veredito_lavrado.py:244`) |
| `D.fechada_em` | `fechar` carimba; reaberturas administrativa/sistêmica limpam. (`app/chamados/models.py:1930`, `:1934`, `:2147`, `:2171`) | Pré-trava, emissão e signals. É **instante de fechamento**; NULL significa aberta. (`app/chamados/models.py:2671`; `app/chamados/services/disputa_emissao.py:531`; `app/chamados/signals.py:28`, `:129`, `:153`) |
| `D.motivos` | Emissão agrega/cria; agrupador complementa; modelo apara/remove e reabertura administrativa recompõe. (`app/chamados/services/disputa_emissao.py:541`, `:589`, `:1019`, `:1191`; `app/chamados/models.py:1964`, `:2061`, `:2153`) | Emissão, complemento e remoção sob trava. É lista de identificadores de motivos; sua inclusão pode preceder as guardas que decidem se nasce pergunta. (`app/chamados/services/disputa_emissao.py:536`, `:1017`, `:825`; `app/chamados/models.py:2029`) |
| `Ch.status_local` | Nascimento e `_gravar_estado`, que salva e dispara signals. (`app/chamados/models.py:971`, `:1095`, `:228`, `:296`) | Catálogo/transições, pré-trava dos vivos, costura e signals. É estado categórico; vivo/encerrado/terminal vêm dos conjuntos do catálogo. (`app/chamados/models.py:803`, `:2681`; `app/chamados/services/fio.py:755`, `:770`; `app/chamados/signals.py:70`, `:96`) |

O mapa das funções também exige distinguir chave gravada, dia julgado e instante:

| Função | Contrato e consumidores |
|---|---|
| `PerguntaDisputa.trava_na_ordem` | Ignora None/repetidos; ordena cada tabela por pk; adquire C→D→Ch→P. Não ordena retrospectivamente as travas do chamador. É usada pelas portas e pelos dois helpers novos. (`app/chamados/models.py:2650`, `:2664`, `:2673`, `:2686`, `:2690`; `app/chamados/services/declaracao_texto.py:293`; `app/chamados/services/disputa_emissao.py:954`) |
| `fio.universo_do_fato` | Parte de `do_fato`, filtra pelo dia real do turno e fecha o componente; serve a `_nascer`, `_candidata_reusavel` e à nova pré-trava da declaração. Não é simplesmente igualdade da data gravada. (`app/chamados/services/fio.py:1784`, `:1800`, `:1822`; `app/chamados/models.py:2905`; `app/chamados/services/declaracao_texto.py:244`, `:294`) |
| `fio.chave_do_dia` | Retorna `(FK, motivo, data_evento gravada, instancia_marco)`. Seus sete usos internos alimentam identidade/cache/vizinhança; o dia real pode ser obtido depois pelo juiz. (`app/chamados/services/fio.py:1449`, `:1450`, `:1437`, `:1455`, `:1478`, `:1510`, `:1645`, `:1658`, `:1790`) |
| `chamados.juizes.dia_do_fato` | Escada: célula; catálogo/contexto; dia local da batida; menor data das perguntas não conversacionais; finalmente `.date()` do instante cru do contexto. Portanto os degraus fracos não garantem a mesma força semântica de “dia do turno”. O repasse novo usa a escada completa. (`app/chamados/juizes.py:53`, `:75`, `:98`, `:114`, `:129`, `:133`, `:166`; `app/chamados/services/disputa_emissao.py:956`) |
| `veredito_lavrado.expira_em` | Usa limites inclusivos da janela, como o juiz, e devolve o primeiro probe fora dela; consumidores são `lavrar` e backfill. Só distingue ausência de data, não conversa: conversas novas têm data civil não nula. (`app/chamados/veredito_lavrado.py:123`, `:130`, `:244`; `app/chamados/juizes.py:601`; `app/chamados/management/commands/backfill_veredito_lavrado.py:111`; `app/chamados/models.py:2799`) |

Para a sequência transacional, **C = Colaborador, D = DisputaSupervisao, Ch = ChamadoColaborador, P = PerguntaDisputa**. Toda aquisição agrupada abaixo usa pk crescente pelo adquirente; UPDATE de linha já pré-travada é reentrante. (`app/chamados/models.py:2650`, `:2664`, `:2673`, `:2686`, `:2690`)

| Porta/atomic | Sequência efetiva |
|---|---|
| `_trava_do_ato` / `declarar_hora_colab` | C; leitura do universo; D abertas + D do fato; Ch vivos/pais das abertas + declarado + destinos do fato; P do fato. Emissão readquire C/D/Ch e atualiza D ou insere Ch/D; depois costura/`abrir`, UPDATE P e resposta. A resposta pode ainda levantar arquivo de destino/pai pela porta de campo. (`app/chamados/services/declaracao_texto.py:293`, `:295`, `:342`, `:354`, `:364`, `:370`, `:413`; `app/chamados/services/disputa_emissao.py:530`, `:565`, `:1434`, `:1436`, `:1465`; `app/chamados/services/materializacao.py:711`, `:718`; `app/chamados/services/arquivo.py:209`) |
| `aplicar_declaracao` | Mesmo prefixo no atomic da emissão/resposta. **A validação está fora desse bloco**; abre outro atomic com C→D→Ch→UPDATE P, seguido da materialização. Sem atomic externo, são transações distintas. (`app/chamados/services/declaracao_texto.py:648`, `:650`, `:687`, `:690`; `app/chamados/services/validacao.py:425`, `:431`, `:435`) |
| `_trava_do_repasse` / agrupador | C; cálculo do teto; D explícitas+abertas; Ch órfãos/destinos/pais/vivos; P selecionadas. Depois complemento/`abrir`, UPDATE D, `analisar` e cascatas; corpo principal; entradas pendentes; possível `fechar`. O teto incompleto permite aquisições novas posteriores, conforme achado 1. (`app/chamados/services/disputa_emissao.py:954`, `:962`, `:988`, `:1017`, `:1020`, `:1021`, `:1206`, `:1220`) |
| `PerguntaDisputa.abrir/_nascer` | `_sob_contrato` abre atomic; `_nascer` readquire C. Reuso com rechaveamento toma C→D→Ch→P antes de rechavear; reuso normal pode tomar Ch pela costura e fazer UPDATE P; nascimento faz INSERT P. (`app/chamados/models.py:2817`, `:2843`, `:2938`, `:2945`, `:2973`, `:2999`, `:3009`) |
| `reapontar_resolvedora` | Sem colisão: C→Ch destino→UPDATE P. Com colisão: C→D→Ch→P do lote, costura, retratação e UPDATE P. O bloco usa `savepoint=False`; sob atomic externo conserva suas travas até o fim dele. (`app/chamados/services/fio.py:880`, `:888`, `:903`, `:908`, `:920`, `:923`) |
| `rechavear_data_evento` / `fato_livre_para` | C→D→P; `fato_livre_para` readquire C; valida destino por leitura; UPDATE P. Não adquire Ch por essa validação. (`app/chamados/services/fio.py:1000`, `:1022`, `:1029`, `:1033`; `app/chamados/models.py:2764`, `:2765`) |
| `retratar_para_canonica` | C→D→P de canônica+perdedoras; UPDATE P pela porta de identidade. Mesmo com perdedoras vazias ainda trava a canônica. (`app/chamados/services/fio.py:1326`, `:1328`, `:1331`; `app/chamados/models.py:3172`, `:3180`) |
| `migrar_resolvedoras_vivas` | Calcula conjunto; C de todos→D do conjunto→Ch velho/novo→P do conjunto; relê; recusa crescimento; reaponta para novo e retrata. A chamada isolada corrige a dupla aquisição de C; não corrige prefixos externos. (`app/chamados/services/fio.py:2084`, `:2086`, `:2088`, `:2091`, `:2160`, `:2166`) |
| Command `_migrar` / `_reverter` | `_migrar` herda atomic do `handle`; `_reverter` abre seu próprio. Ambos pré-travam o lote C→D→Ch→P; chamadas por linha e rechaveamento reencontram essas linhas. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:237`, `:374`, `:389`, `:400`, `:465`, `:470`, `:485`, `:492`) |
| `DisputaSupervisao.remover_motivo` | C; lote C→D→Ch→P; UPDATE P; UPDATE D. O eventual `fechar` ocorre depois do atomic próprio, mas continua na mesma transação se existir atomic externo. (`app/chamados/models.py:2023`, `:2026`, `:2052`, `:2061`, `:2064`) |
| `lavrar_sem_turno_aberto` | Não abre atomic global: carimba P e pode depois fechar D. Em autocommit, o atomic do save P termina antes do atomic de fechamento D; com atomic externo, depende do prefixo dele. (`app/chamados/reconciliador.py:405`, `:411`; `app/chamados/models.py:3412`, `:1903`, `:1908`) |
| `lavrar` / backfill de lavra | Fazem UPDATE P, individual ou em lote, sem pré-trava C/D/Ch própria. `expira_em`, `_hoje`, `chave_do_dia` e `_conjunto_da_migracao` não adquirem essas travas. (`app/chamados/veredito_lavrado.py:95`, `:106`, `:221`, `:246`; `app/chamados/services/fio.py:1442`, `:2032`) |

As cascatas relevantes são: Ch encerrado pode atualizar sua urgência e resolver perguntas; D fechada pode reconciliar o pai e materializar pendentes; mudança de estado de Ch pode chamar o cartório sincronamente. P agenda o fechamento do beco em `on_commit` e emite outbox sincronamente. Não se deve contar o callback pós-commit como se ainda mantivesse as travas da transação encerrada. (`app/chamados/signals.py:72`, `:115`, `:42`, `:163`, `:203`, `:225`, `:244`)

Os chamadores das declarações nas views fazem leitura comum antes da chamada, sem aquisição explícita prévia nesses corpos. A menção a `core/views_ia.py:150` é um homônimo: chama outro `_aplicar_declaracao`, que despacha declaração de dias pelo intake. Transação adicional de middleware/configuração de produção: [nao verificado]. (`app/chamados/views.py:363`, `:480`, `:1046`, `:1090`; `app/core/views_ia.py:150`, `:754`, `:775`)

Foram localizadas seis cha

[truncado em 20000 de 21940 caracteres]

## 08/11 22:24 · sessao 65246f94aba2 · - · - · origem=Ronald (tmux)

Complementos:

- `lavrar_sem_turno_aberto` não abre transação nem trava C/D/Ch/P: carimba P e depois pode fechar D (`app/chamados/reconciliador.py:405-413`). Em autocommit, o `save` de P encerra seu próprio `atomic` (`app/chamados/models.py:3412-3413`) antes do `atomic` de D (`:1903-1910`); portanto não há P→D mantidas nesse caso. Com transação externa, a ordem depende do prefixo do chamador. A emissão chama a porta dentro de `atomic` próprio após criar a pergunta (`app/chamados/services/disputa_emissao.py:459-465`); o reconciliador avulso não abre `atomic` em volta do laço (`app/chamados/reconciliador.py:1520-1525`, `:1615`).

- `remover_motivo` começa com C, depois trava C→D→Ch→P do conjunto (`app/chamados/models.py:2023-2028`); atualiza P (`:2052-2060`) e D (`:2061-2063`) já travadas. `fechar` é chamado após o `atomic` próprio (`:2064-2070`): sem transação externa é outro ato; com externa, conserva as travas anteriores.

- `_migrar` herda o `atomic` do comando (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:237-242`), trava o lote C→D→Ch→P (`:374-378`), e as travas por grupo/linha e o rechaveamento reencontram esse conjunto (`:381-400`). `_reverter` abre um `atomic`, trava o lote inteiro (`:465-473`) e depois retrava por linha/rechaveia (`:485-492`).

- Encontrei seis chamadas diretas de produção ao agrupador: `app/chamados/services/acoes_disputa.py:221`, `app/chamados/services/pergunta_no_app.py:186`, `app/chamados/services/cobrar_dia.py:47`, `app/ponto/management/commands/processar_alertas_avancados.py:158`, `app/ponto/management/commands/processar_alertas_turno.py:384`, `app/ponto/management/commands/detectar_ausencias.py:801`. `app/chamados/models.py:4072` é reexportação; as outras duas chamadas são testes (`app/tenants/tests.py:74`, `:85`). O “sétimo chamador” alegado no texto do commit é [nao verificado].

## 08/11 22:23 · sessao 1ebe59f1dec7 · - · - · origem=Ronald (tmux)

**Sequência das declarações.** Sem atomic externo, `declarar_hora_colab` abre o atomic em `app/chamados/services/declaracao_texto.py:342`; `_trava_do_ato` toma C, calcula o universo e toma D abertas + D do fato, Ch vivos/pais das abertas + destino declarado + destinos do fato, P do fato (`:293-297`). Cada grupo é ordenado por PK pelo adquirente (`app/chamados/models.py:2650-2691`). `disputa_para_declaracao` chama `_abrir_ou_agregar_disputa`, cuja aquisição C/D/Ch é reentrante para as linhas existentes; agrega motivos por UPDATE D ou cria Ch/D (`app/chamados/services/disputa_emissao.py:627-630,530-592`). Depois vêm `_candidata_reusavel`, eventual reapontamento ou `abrir`, resposta e nota (`app/chamados/services/declaracao_texto.py:357-371,413-419`). A resposta readquire C e faz UPDATE P (`app/chamados/services/disputa_emissao.py:1431-1436`); sua cauda pode levantar arquivo no destino/pai, o que toma C/D/Ch pela porta de campo e faz UPDATE Ch (`app/chamados/services/disputa_emissao.py:1464-1465`; `app/chamados/services/materializacao.py:710-722`; `app/chamados/services/arquivo.py:209-210`). O fim do atomic próprio é `app/chamados/services/declaracao_texto.py:419`; callbacks `on_commit` de P ficam para depois do commit externo (`app/chamados/signals.py:214-225`).

**`aplicar_declaracao` tem duas fases transacionais.** O primeiro atomic começa em `app/chamados/services/declaracao_texto.py:648`, toma o mesmo conjunto (`:650`), emite/reusa disputa e pergunta e responde (`:653-661,681-689`). `validar_pergunta` está **fora** desse bloco (`:690`). A validação abre outro atomic e toma C → D abertas + D da pergunta → Ch vivos/pais/destino/clones, depois UPDATE P pelo carimbo; a materialização individual ocorre dentro desse bloco (`app/chamados/services/validacao.py:425-466`). O lote da disputa é chamado **fora** desse atomic (`:469`) e percorre todas as perguntas prontas antes do fecho (`app/chamados/services/materializacao.py:1369-1379`). Se há atomic externo do chamador, os blocos continuam na mesma transação externa; sem ele, não se deve juntar essas aquisições numa única sequência contínua (`app/chamados/services/declaracao_texto.py:648-690`; `app/chamados/services/validacao.py:425-469`).

**Chamadores confirmados.** As views leem o Chamado com `get_object_or_404` e chamam diretamente as portas; não há aquisição prévia explícita nesses corpos (`app/chamados/views.py:359-363,477-481,1041-1046,1089-1091`). A referência `core/views_ia.py:150` é homônimo: despacha `declarar_folga`/`declarar_falta` para `_aplicar_declaracao(request, proposta)`, que chama `core.intake.escritor_de(acao)` com dias/tipo, não a porta de horário tocada (`app/core/views_ia.py:149-150,754-776`). Transação adicional introduzida por configuração/middleware em produção [nao verificado].

**Achados prioritários dos testes.** [nao verificado]

1. **Média — A3 acusa motivo mais amplo do que demonstra.** Com o mesmo `DIA` na fixture e no pedido, `_nascer` pula a aquisição de D existente condicionada à mudança de dia (`app/chamados/tests/test_e1_fila.py:175,183-185`; `app/chamados/tests/test_e1_v9.py:145-147`; `app/chamados/models.py:2938-2948`). O mutante morde o Ch destino de PK menor depois do Ch vivo maior, e não necessariamente D fechada depois de Ch (`app/chamados/tests/test_e1_fila.py:173-178,195-199`; `app/chamados/services/fio.py:762-769`).

2. **Média — F1 termina no complemento inicial.** A fixture fornece contexto sem `marcos_faltantes` e P viva costurada (`app/chamados/tests/test_e1_fila.py:229-246`); `cobra_marcos_da_ata` é falso (`app/chamados/services/disputa_emissao.py:732-735`; `app/ponto/services/lastro.py:74-79`). Após complementar, o filtro elimina o único Ch por já ter P não materializada e retorna `None` (`app/chamados/services/disputa_emissao.py:1004-1030`). Não cobre a cauda da porta (`:1032-1223`); `_r` é ignorado (`app/chamados/tests/test_e1_fila.py:254,264`).

3. **Alta — O teto do repasse é incompleto.** Pré-trava somente dias dos órfãos ±1/nulos e perguntas já costuradas (`app/chamados/services/disputa_emissao.py:955-963`), mas a cauda processa dias arbitrários de `entradas_pendentes` (`:1206,273-296`). Pode reusar e atualizar P canônica antiga de PK menor fora do teto, depois das P já pré-travadas (`:443-451`; `app/chamados/models.py:2905-2919,2958-2999`). Ocorrência em prod [nao verificado]; F1 não alcança essa cauda (`app/chamados/services/disputa_emissao.py:1029-1030`).

4. **Média — A1 passa com derivador equivalente sobrevivente.** O detector busca sintaxe `.disputa.colaborador` como alternativa de `or`, sem aliases (`app/chamados/tests/test_e1_fila.py:76-93`); `fecho_cruza_dias` ainda lê ambas as colunas e usa `c or dc` (`app/chamados/services/fio.py:1844-1847`).

5. **Baixa — F5 não prova uma única leitura de relógio.** O teste fixa `now` em constante e compara somente valores finais; duas leituras independentes passariam (`app/chamados/tests/test_e1_fila.py:322-328`). O código atual efetivamente repassa `_agora` (`app/chamados/veredito_lavrado.py:242-244`).

6. **Média — Cobertura de ordem não equivale à transação inteira.** `_travar` captura apenas a chamada isolada e filtra linhas novas (`app/chamados/tests/test_e1_fila.py:57-69`); A2/A3 não exercitam prefixos de chamadores, a porta irmã, nem universo crescendo entre leitura e trava (`:147-160,181-199`). O rastreador inclui UPDATE simples e FOR UPDATE, mas ignora `executemany` e travas de FK; identifica presença de algum C, sem conferir o dono da linha (`app/chamados/tests/test_e1_v9.py:47-53,68-90,95-116`).

Execução dos testes e resultado GREEN [nao verificado]. [nao verificado]

## 08/11 22:22 · sessao 1ebe59f1dec7 · - · - · origem=Ronald (tmux)

1. **Média — A1 prova só a forma sintática, não a ausência de derivação paralela.** O detector reconhece apenas `BoolOp Or` cuja alternativa contém diretamente `.disputa.colaborador[_id]`; aliases ficam fora (`app/chamados/tests/test_e1_fila.py:76-93`). Há derivação equivalente no código de produção: `values_list` entrega ambos os colaboradores e a chave usa `c or dc` (`app/chamados/services/fio.py:1844-1847`). Portanto o teste pode passar com derivador paralelo sobrevivente, embora efetivamente examine arquivos de produção (`app/chamados/tests/test_e1_fila.py:99-111`).

2. **Média — O mutante A3 morde ordem de PK de Chamado; não demonstra a aquisição tardia da Disputa fechada anunciada.** O cenário fornece `ex.data_evento=DIA` pela base e passa o mesmo `DIA` na declaração (`app/chamados/tests/test_e1_v9.py:145-147`; `app/chamados/tests/test_e1_fila.py:175,183-185`). O ramo de `_nascer` que trava a disputa existente exige diferença de dia (`app/chamados/models.py:2938-2948`). Com igualdade, a costura trava o Chamado por `_costura_nao_mata_o_fio` e salva a Pergunta (`app/chamados/services/fio.py:762-769`; `app/chamados/models.py:2996-2999`). A inversão exercitada é o destino de PK menor, criado antes do Chamado vivo (`app/chamados/tests/test_e1_fila.py:173-178`). O teste aceita qualquer violação e não exige a Disputa fechada na sequência (`app/chamados/tests/test_e1_fila.py:188-199`).

3. **Média — F1 cobre somente o complemento inicial do agrupador.** A fixture entrega a mesma instância `ch`, com contexto contendo apenas `data_turno` e uma Pergunta pendente já costurada (`app/chamados/tests/test_e1_fila.py:229-246`). `cobra_marcos_da_ata` resulta falso sem `marcos_faltantes` (`app/chamados/services/disputa_emissao.py:732-735`; `app/ponto/services/lastro.py:74-79`). O primeiro laço complementa essa disputa; depois o filtro elimina o único Chamado por já ter Pergunta não materializada e retorna `None` (`app/chamados/services/disputa_emissao.py:1004-1030`). Assim os testes não alcançam o corpo principal, os loops posteriores, `perguntar_entradas_pendentes` ou o fecho (`app/chamados/services/disputa_emissao.py:1032-1223`). O teste não é vazio: exige Perguntas costuradas e presença da Disputa na sequência; porém descarta o retorno e não verifica a porta inteira (`app/chamados/tests/test_e1_fila.py:252-265`).

4. **Alta — O “teto” do repasse não cobre a cauda dos dias pendentes da disputa.** O conjunto inicial considera somente Perguntas costuradas nos órfãos e datas dos órfãos ±1 ou nulas (`app/chamados/services/disputa_emissao.py:955-963`). A mesma transação termina chamando `perguntar_entradas_pendentes`, que extrai outros dias do contexto da disputa e chama `gerar_perguntas_disputa` (`app/chamados/services/disputa_emissao.py:1206`; `app/chamados/services/disputa_emissao.py:273-296`). Essa geração pode abrir/reusar uma Pergunta canônica de outra disputa para um dia fora daquele conjunto (`app/chamados/services/disputa_emissao.py:443-451`; `app/chamados/models.py:2905-2919`). Se essa Pergunta tem PK menor que alguma já pré-travada e recebe célula/destino, seu `UPDATE` ocorre posteriormente, quebrando a ordem de PK de Pergunta (`app/chamados/models.py:2958-2999`). É um caminho permitido pelo código; ocorrência no acervo/prod [nao verificado]. F1 não o cobre porque retorna antes, conforme item 3.

5. **Baixa — F5 “MESMO instante” não distingue uma leitura de duas leituras do relógio.** O teste fixa `timezone.now()` em uma constante e compara somente os dois valores finais (`app/chamados/tests/test_e1_fila.py:322-328`). Uma implementação com duas leituras independentes desse relógio produziria a mesma tupla e passaria; não há `call_count`, `side_effect` ou inspeção do argumento passado a `expira_em` (`app/chamados/tests/test_e1_fila.py:322-328`). O código atual passa explicitamente `_agora`, o que é verificável na implementação (`app/chamados/veredito_lavrado.py:242-244`). Os outros testes F5 realmente distinguem `_hoje(agora)` de `date.today()` usando duas datas diferentes (`app/chamados/tests/test_e1_fila.py:307-320,330-333`).

6. **Média — Os testes novos não verificam a transação inteira dos chamadores nem a recusa por universo alterado.** `_travar` inicia a captura imediatamente antes da chamada isolada e filtra os PKs nascidos durante ela (`app/chamados/tests/test_e1_fila.py:57-69`). A2 chama diretamente `migrar_resolvedoras_vivas`; A3 chama diretamente `declarar_hora_colab`; não exercitam entrada já travada por `reabrir`/views, nem `aplicar_declaracao` (`app/chamados/tests/test_e1_fila.py:147-160,181-199`). Os dois casos A2 verificam ordenação e o mutante de chamada duplicada; nenhum provoca o crescimento entre `_conjunto_da_migracao` e sua releitura (`app/chamados/tests/test_e1_fila.py:147-160`).

7. **Informativa — O rastreador inclui UPDATE, com limites específicos.** Ele observa `UPDATE` simples e `SELECT ... FOR UPDATE`, consulta os PKs e elimina reacessos à mesma linha (`app/chamados/tests/test_e1_v9.py:68-90`). Não observa `executemany`, INSERT ou travas de FK; o próprio helper declara parte desse limite (`app/chamados/tests/test_e1_v9.py:47-53,69-70`). A regra de violação identifica rank inferior posterior, PK decrescente e ausência de qualquer Colaborador anterior, mas não valida se esse Colaborador é o dono de cada linha (`app/chamados/tests/test_e1_v9.py:95-116`). Os testes P3/P6/P7 inserem deliberadamente a aquisição de Pergunta antes de Colaborador; suas assertions exigem qualquer violação, sem especificar qual (`app/chamados/tests/test_e1_fila.py:273-303`).

8. **Limite sobre acervo cruzado do repasse.** A seleção por destino pode trazer Pergunta de outro colaborador, enquanto a pré-trava de Colaborador continua restrita ao argumento da porta (`app/chamados/services/disputa_emissao.py:954-963`). Entretanto o caminho simples de complemento passa a disputa dessa Pergunta com o Chamado do argumento; `_nascer` recusa o destino cruzado antes de adquirir outro Colaborador (`app/chamados/services/disputa_emissao.py:1006-1017,834-841`; `app/chamados/models.py:2837-2843`). Esse caminho sozinho não basta para afirmar deadlock por segunda aquisição de Colaborador; ocorrência de outra cascata [nao verificado].

A análise foi estática; execução dos testes, resultado GREEN e comportamento concorrente efetivo [nao verificado]. [nao verificado]

## 08/11 22:21 · sessao 65246f94aba2 · - · - · origem=Ronald (tmux)

1. **Média — o novo “teto” do repasse não cobre toda a cascata síncrona do cartório.** `_trava_do_repasse` trava perguntas costuradas nos órfãos ou do colaborador nos dias ±1/sem dia (`app/chamados/services/disputa_emissao.py:954-963`). O corpo chama `ch.analisar()` (`:1021`, `:1095`, `:1114`, `:1159`), que passa por `_gravar_estado/save` (`app/chamados/models.py:800-806`, `:280-296`); o `post_save` chama o cartório (`app/chamados/signals.py:187-203`). Havendo adiamentos pela lâmpada (`app/ponto/services/cartorio.py:1482-1485`), o cartório chama `retratar_perguntas_absolvidas_por_celula(colab)` (`:951-959`). Essa função varre o colaborador inteiro, sem recorte de dia (`app/chamados/reconciliador.py:584`, `:627-642`), e atualiza perguntas via `carimbar/save` (`:592-595`; `app/chamados/models.py:3142`). Assim, uma pergunta antiga de outro dia, com pk menor que alguma já travada pelo repasse, pode ser adquirida por UPDATE depois dela. A ocorrência e a reprodução desse cenário são [nao verificado].

2. **Média — A2 não ordena travas já mantidas pelos chamadores.** `reabrir_orfas_enterradas` trava C/D/Ch/P no mesmo `atomic` (`app/chamados/management/commands/reabrir_orfas_enterradas.py:146-156`) e depois chama `pai.reabrir()` (`:168-172`); o ramo `irmao_vivo` de `reabrir` chama a migração (`app/chamados/models.py:1411-1415`), que acrescenta colaboradores das perguntas costuradas e do universo (`app/chamados/services/fio.py:2037-2038`, `:2084-2087`). O caminho exige mudança entre o pré-filtro e a trava: o pré-filtro já exclui `irmao_vivo` (`app/chamados/management/commands/reabrir_orfas_enterradas.py:105-112`). Um irmão que apareça depois dele pode levar a nova aquisição de C após D/Ch/P; ocorrência [nao verificado].

3. **Inventário da ordem efetiva das portas de fio.** `trava_na_ordem` ignora None e ordena pks (`app/chamados/models.py:2650-2664`); trava D explícitas/abertas (`:2667-2676`), Ch explícitos/pais das abertas/vivos (`:2677-2687`), depois P (`:2689-2691`).

   - `reapontar_resolvedora`, sem colisão: C (`app/chamados/services/fio.py:887-888`), Ch destino pela costura (`:906-908`, `:768`), UPDATE P (`:923`). Com colisão: lote C→D→Ch→P (`:903-905`), retratação reentrante (`:920`), UPDATE P (`:923`).
   - `rechavear_data_evento`: C→D→P (`app/chamados/services/fio.py:1000-1001`), C reentrante por `fato_livre_para` (`:1022`), UPDATE P (`:1033`).
   - `retratar_para_canonica`: C→D→P (`app/chamados/services/fio.py:1326-1331`), UPDATE P por `carimbar_identidade` (`app/chamados/models.py:3172-3180`). Mesmo sem perdedoras ainda trava a canônica (`app/chamados/services/fio.py:1326-1329`).
   - `migrar_resolvedoras_vivas`: C de todos os envolvidos→D do conjunto→Ch velho/novo→P do conjunto (`app/chamados/services/fio.py:2084-2087`); relê e recusa crescimento por C/P (`:2088-2093`); reaponta apenas para `novo` (`:2160`) e retrata usando a canônica do universo (`:2137-2141`, `:2166`). Não identifiquei aquisição adicional de Ch externo nesse corpo; a costura trava apenas o destino recebido (`:768`).

4. **Signals das perguntas não acrescentam C/D/Ch/P imediatamente.** O fechamento do beco fica em `on_commit` (`app/chamados/signals.py:214-225`); o outbox síncrono chama `acordar_celulas` (`:237-244`). O `save` da pergunta envolve UPDATE e signals num `atomic` próprio (`app/chamados/models.py:3403-3413`). Já o signal do chamado pode entrar sincronamente no cartório (`app/chamados/signals.py:187-203`), portanto precisa integrar a análise da transação inteira.

## 08/11 22:21 · sessao a5b4ba247a80 · - · - · origem=Ronald (tmux)

Auditoria dos campos e semântica, sem execução de testes. [nao verificado]

1. **[ALTA] Relógios continuam discordando.** `expira_em` agora obtém dia civil local de `agora` (`app/chamados/veredito_lavrado.py:103,126`), enquanto `esta_stale` usa `agora.date()` e `julgado_em.date()` (`app/chamados/veredito_lavrado.py:196,199`) e a varredura usa `timezone.now().date()`/`_jul.date()` (`app/chamados/services/raiox.py:36,41`). O projeto configura `USE_TZ=True` e São Paulo (`app/config/settings/base.py:93,95`). Portanto, nos instantes em que UTC e São Paulo estão em dias distintos, não há contrato temporal único entre escritor e leitores; ocorrência em produção [nao verificado].

2. **[MÉDIA] f5 unifica carimbo e expiração, mas não o julgamento.** `lavrar` executa o juiz antes de capturar `_agora`, depois passa esse instante somente a `expira_em` (`app/chamados/veredito_lavrado.py:233-244`). O juiz ainda lê `date.today()` por conta própria (`app/chamados/juizes.py:575`); o backfill captura `hoje=localdate()` e `agora=now()` separadamente e volta a executar o juiz por linha (`app/chamados/management/commands/backfill_veredito_lavrado.py:94-105`). A janela do juiz e da expiração usa extremos inclusivos (`app/chamados/juizes.py:601`; `app/chamados/veredito_lavrado.py:130`); a obsolescência dispara na data de expiração, inclusivamente (`app/chamados/veredito_lavrado.py:196`).

3. **[MÉDIA] A1 não elimina toda derivação alternativa.** Há o fallback equivalente em tuplas `c or dc` no contador `fecho_cruza_dias` (`app/chamados/services/fio.py:1844-1847`). Em `rechavear_data_evento`, trava/dedup usam o FK da pergunta (`app/chamados/services/fio.py:1000,1022`), mas busca e despertar da célula usam o colaborador da disputa (`app/chamados/services/fio.py:1021,1024,1035`). O `save` só confere/preenche igualdade na inserção (`app/chamados/models.py:3410-3425`); não existe essa conferência nesse `save` para objetos existentes. Divergência real do acervo [nao verificado].

4. **[INFORMATIVA] `None` é possível em instância não salva, mas não há evidência de um caminho de produção novo que dependa dele.** `_nascer` cria inicialmente a pergunta final sem informar colaborador (`app/chamados/models.py:3006`), e `save` chama `_chave_de_identidade`, preenchendo-o da disputa (`app/chamados/models.py:3410-3422`). O stub usado antes para perguntar pelo dia recebe `colaborador_id` explicitamente (`app/chamados/models.py:2874`), assim como o proxy do juiz (`app/chamados/models.py:2884`). O esquema atual é NOT NULL (`app/chamados/models.py:2431`); a migration primeiro preenche todo acervo pela disputa e depois aplica NOT NULL (`app/chamados/migrations/0051_decisao_pergunta.py:25,101-108`). Acervo persistido com NULL e migrations efetivamente aplicadas em produção [nao verificado].

5. **[INFORMATIVA] Dois significados de `data_evento` são preexistentes e discriminados por `conversa`.** Pergunta datada exige dia do turno (`app/chamados/models.py:2781-2788`); conversa recebe dia civil local do pedido (`app/chamados/models.py:2794-2800`). `gravar_dia_do_fato` é a atribuição única do campo (`app/chamados/services/fio.py:790-800`), usada pelo nascimento (`app/chamados/models.py:3008`), costura que data (`app/chamados/services/fio.py:878`) e rechaveamento (`app/chamados/services/fio.py:1027`). A escada `dia_do_fato` ignora conversas (`app/chamados/juizes.py:75-83`), mas `expira_em` e `lavrar` não recebem o discriminador, apenas `data_evento` (`app/chamados/veredito_lavrado.py:123-130,244`). Portanto, `expira_em(None)` não caracteriza todas as conversas: conversas novas têm data não nula (`app/chamados/models.py:2799`).

Mapa de escritores/leitores comprovados:

| Campo/função | Escritores/implementação | Leitores de produção comprovados |
|---|---|---|
| `P.colaborador_id` | Inserção, conferência e preenchimento: `models.py:3410-3425`; migração: `migrations/0051_decisao_pergunta.py:25` | Dedup `models.py:2752,2764-2771`; chave `services/fio.py:1449`; costura `services/fio.py:862,887`; migração `services/fio.py:2087-2091`; juiz de destino `juizes.py:2003` |
| `P.chamado_destino` | `_nascer`, criação/reuso `models.py:2975,2999,3006,3009`; costura `services/fio.py:921-924`; atribuição temporária sem save `services/fio.py:779-783` | Materializador `services/materializacao.py:339`; crédito destino-ou-pai `services/raiox.py:54-64`; juiz/costura `services/fio.py:778`, `juizes.py:1994-2003`; trava/migração `services/fio.py:2087-2091` |
| `P.julgado_em/janela_expira_em` | `lavrar`: `veredito_lavrado.py:242-246`; backfill calcula `management/commands/backfill_veredito_lavrado.py:139-140` e persiste via `veredito_lavrado.py:218-221`; restauração só em memória `veredito_lavrado.py:255-260` | `esta_stale`: `veredito_lavrado.py:193-203`; varredura `services/raiox.py:31-44`; foto `veredito_lavrado.py:252`; recorte/backfill `management/commands/backfill_veredito_lavrado.py:120-136` |
| `P.data_evento` | Atribuição `services/fio.py:800`, callers acima | Índice/dedup `models.py:2751,3473-3482`; materialização `services/materializacao.py:344-347,473-476`; escada `juizes.py:80-83`; chave/universo `services/fio.py:1450,1783-1807`; expiração `veredito_lavrado.py:244` |
| `D.fechada_em` | Fechar `models.py:1930-1934`; reabrir admin/sistema `models.py:2147-2159,2171-2175` | Universo de trava `models.py:2671-2675`; emissão `services/disputa_emissao.py:531-533`; signals `signals.py:28,129,153` |
| `D.motivos` | Emissão `services/disputa_emissao.py:541,589,1019-1020,1061,1067-1068,1096-1097,1191-1192,1288,1294-1295`; aparar `models.py:1964-1966`; remover `models.py:2061-2063`; reabrir admin `models.py:2153-2159` | Emissão `services/disputa_emissao.py:536-540,1017,1190`; remoção relê sob trava `models.py:2029-2030`; lista de IDs ativos conforme definição `models.py:1728-1730` |
| `Ch.status_local` | Nascimento `models.py:971,1095`; mudança de estado `models.py:228` | Universo de trava `models.py:2681`; costura `services/fio.py:770-771`; signals `signals.py:70,96,272,341`; gates usam `motor.decidir`, exemplo `models.py:1601` |
| `trava_na_ordem` | Implementação ordena pks, ignora None, adquire C,D,Ch,P: `models.py:2650-2691` | Helpers novos declaração `services/declaracao_texto.py:293-297`, repasse `services/disputa_emissao.py:954-963`; portas `services/fio.py:1000,1328,2087` |
| `universo_do_fato` | `services/fio.py:1774-1825`, funda-se no `do_fato` e admite pelo dia real do turno | `_nascer` `models.py:2905`; candidata reutilizável `services/declaracao_texto.py:244`; helper de trava `services/declaracao_texto.py:294` |
| `chave_do_dia` | Tupla FK,motivo,data gravada,instância: `services/fio.py:1449-1450` | Sete usos internos: `services/fio.py:1437,1455,1478,1510,1645,1658,1790` |
| `juizes.dia_do_fato` | Escada célula → catálogo/data_turno → data civil local de batida → menor data pergunta de disputa → data de instante cru de contexto: `juizes.py:50-53,73-84,98-138,159-166` | Repasse novo `services/disputa_emissao.py:956`; declaração `services/declaracao_texto.py:193,324,455,565,600`; reconciliador `reconciliador.py:139,200,286,1023,1290` |
| `expira_em` | `veredito_lavrado.py:106-133`; `None` sem dia, devolve primeiro probe fora da janela, com probe inicial `max(localdate(agora), data_evento)` | `lavrar` `veredito_lavrado.py:244`; backfill `management/commands/backfill_veredito_lavrado.py:111` |

A afirmação “DOIS escritores de destino” tem escopo de criação/costura intencional: o FK também declara `on_delete=SET_NULL`, que autoriza escrita implícita de NULL ao excluir o chamado (`app/chamados/models.py:3375-3380`). Não há caminho concreto de exclusão identificado nesta auditoria [nao verificado].

## 08/11 17:58 · sessao 3691d7efd094 · FIO-E0-V4-E1 · verificador · origem=Code

**NÃO PROCEDENTE a conformidade integral da 15f.** A inversão antiga do reuso isolado foi corrigida, mas permanecem composições que violam a ordem global, leitores sem discriminação da conversa e lacunas nas provas. (`app/chamados/models.py:2910`; `app/chamados/services/disputa_emissao.py:993`; `app/chamados/models.py:390`; `app/chamados/tests/test_e1_v9.py:94`.)

Execução das suítes, RED/GREEN históricos, ocorrência dos contraexemplos em produção e confirmação da ancestralidade Git declarada: **[nao verificado]**.

**1. Situação dos itens 3, 4, 6 e 7 da v15e**

| Item | Agora | Fundamentação |
|---|---|---|
| **3 — Matriz** | **Parcial** | Há provas novas dos ramos antes mal citados, inclusive escrita parcial seguida de rollback. A5 percorre o `raise`, porém provoca divergência de instância no mesmo dia; continuam ramos de classificação sem prova na extensão. (`app/chamados/tests/test_e1_v15f.py:284`; `app/chamados/tests/test_e1_v15f.py:250`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:78`, `:105`, `:120`, `:123`.) |
| **4 — Conversa** | **Curado localmente; parcial globalmente** | A ida exclui conversa e o contador inclui conversa NULL. A reversão continua selecionando por PK sem esse filtro; outros leitores continuam usando o dia civil como dia do fato. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:205`, `:474`; `app/chamados/services/fio.py:1118`; `app/chamados/models.py:390`.) |
| **6 — Catracas/mutantes** | **Parcial** | O wrapper agora exige atomic; o espião alcança o contrato real; M6 exige motivo específico. Permanecem os escapes descritos no item 4 abaixo. (`app/chamados/tests/test_e1_v15e_contrato_de_saida.py:113`, `:551`, `:567`; `app/chamados/tests/test_e1_v15f.py:169`.) |
| **7 — Ordem de trava** | **Parcial** | Corrigidos o reuso isolado com rechave/costura e a aquisição entre candidatos do migrar/reverter. Isso não garante ordem entre todas as chamadas da mesma transação, nem entre PKs adquiridas por chamadas diferentes. (`app/chamados/models.py:2910`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:375`, `:469`; `app/chamados/services/disputa_emissao.py:993`.) |

**2. Ordem de trava — PROCEDENTE a permanência da classe**

O helper ordena **cada conjunto recebido**, mas não mantém um controle da maior classe/PK já adquirida pela transação. Portanto, chamadas individualmente ordenadas podem compor uma sequência global invertida. (`app/chamados/models.py:2625`, `:2639`, `:2648`, `:2661`, `:2665`.)

| Caminho | Resultado da conferência estática |
|---|---|
| **Criação isolada, incluindo conversa** | Adquire Colaborador e insere Pergunta; não adquire explicitamente Disputa/Chamado nesse caminho. Os locks de FK do INSERT ficam fora do rastreador utilizado. (`app/chamados/models.py:2811`, `:2974`; `app/chamados/tests/test_e1_v9.py:51`.) |
| **Reuso sem rechave, mesmo dono, isolado** | Colaborador → Chamado, quando há costura → UPDATE Pergunta. Sem costura ou alteração, não há esse UPDATE. (`app/chamados/models.py:2811`, `:2926`, `:2938`, `:2955`, `:2964`; `app/chamados/services/fio.py:768`.) |
| **Reuso com rechave/costura, mesmo dono, isolado** | O bloco novo antecipa Disputa, destinos e Pergunta. Corrige o P→Chamado anterior. (`app/chamados/models.py:2910`; `app/chamados/services/fio.py:969`, `:768`.) |
| **`rechavear_no_reuso` isolado** | Consulta o veto e chama a porta que adquire C→D→P; o Colaborador consultado posteriormente por `fato_livre_para` já foi adquirido. O DRY executa essa porta e desfaz o ato. (`app/chamados/services/fio.py:1020`, `:1026`, `:1029`, `:969`; `app/chamados/models.py:2740`.) |
| **`_migrar` e `_reverter`** | Antecipam C→D→Chamado→P dos candidatos antes dos laços. Curam a inversão anterior entre grupos/linhas desse conjunto. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:374`, `:375`, `:468`, `:469`.) |
| **Reapontamento e chamadores com atomic aberto** | Permanecem contraexemplos entre classes e entre PKs, abaixo. (`app/chamados/services/fio.py:889`, `:892`; `app/chamados/services/disputa_emissao.py:964`, `:993`.) |

**Contraexemplo estático entre classes, mesmo colaborador:** o agrupador abre atomic, adquire somente C, encontra uma pergunta ligada ao chamado e complementa os marcos da disputa dela. Se o marco irmão tem canônica em outra disputa, `_ja_perguntou` não a encontra porque consulta apenas a disputa atual; `abrir`, porém, pode reusar essa canônica e costurá-la. (`app/chamados/services/disputa_emissao.py:964`, `:968`, `:979`, `:990`, `:1316`; `app/chamados/models.py:2871`, `:2938`, `:2964`.)

Depois desse reuso, `_criar_pergunta_se_couber` devolve `True` e o agrupador salva a Disputa atual, ainda sem pré-trava: **C→Chamado→Pergunta→Disputa**. A chamada seguinte a `ch.analisar()` também pode adquirir disputas abertas depois da Pergunta. (`app/chamados/services/disputa_emissao.py:834`, `:844`, `:991`, `:993`, `:994`; `app/chamados/models.py:806`, `:290`.) Execução e ocorrência em produção: **[nao verificado]**.

**Contraexemplo estático entre colaboradores:** `_nascer` adquire primeiro C da disputa; o bloco novo pode depois adquirir C do destino com PK menor. E4-c aceita dias iguais sem conferir igualdade de donos. (`app/chamados/models.py:2811`, `:2911`; `app/chamados/juizes.py:2027`.) Existência desse arranjo em produção: **[nao verificado]**.

**Contraexemplo estático entre perguntas:** no reapontamento que data uma canônica menor, as perdedoras retratáveis são atualizadas antes dela. Ordenar apenas as perdedoras não impede **P maior→P menor**, porque a canônica está excluída dessa ordenação. (`app/chamados/services/fio.py:889`, `:892`, `:1207`, `:1253`, `:1271`; `app/chamados/models.py:3145`.) Execução com esse acervo: **[nao verificado]**.

**Outro caso de PK:** a declaração antecipa os chamados vivos; uma costura posterior pode adquirir um destino terminal, excluído daquele conjunto, com PK menor. A decisão sobre o terminal acontece depois da aquisição da linha. (`app/chamados/services/declaracao_texto.py:324`, `:332`, `:348`; `app/chamados/services/disputa_emissao.py:530`; `app/chamados/models.py:2656`; `app/chamados/services/fio.py:768`, `:770`.) Execução: **[nao verificado]**.

Sobre o primeiro argumento de `trava_na_ordem`:

- **Lista ordenada:** aceita lista/tupla/set/frozenset; elimina repetidos, ignora `None` e instâncias com PK `None`, ordena os PKs e também ordena a consulta. `None` escalar resulta em conjunto vazio. (`app/chamados/models.py:2625`, `:2631`, `:2636`, `:2639`.)
- **Reentrância:** repete as consultas; para linhas já possuídas, a repetição é reentrante. Não impede aquisição posterior de uma linha nova fora da ordem. (`app/chamados/models.py:2609`, `:2639`, `:2648`, `:2661`, `:2665`.)
- **Modo universo:** `abertas_do_colaborador` admite no máximo um colaborador. (`app/chamados/models.py:2633`.)
- **`select_for_update` fora do helper:** não há chamada acrescentada pelo diff fornecido; nos arquivos produtivos alterados, as chamadas executáveis dessas quatro entidades estão no helper. (`app/chamados/models.py:2639`, `:2648`, `:2661`, `:2665`.) Correspondência histórica integral entre diff e commits: **[nao verificado]**.

**3. Conversa — cura local PROCEDENTE; esmeril global NÃO PROCEDENTE**

A conversa nasce com `localdate`; seu adaptador devolve pedido/gravado. O contador agora a inclui, mas compara o dia gravado consigo mesmo: acusa NULL, não determina independentemente se uma conversa datada conserva o dia civil original. (`app/chamados/models.py:2771`; `app/chamados/services/fio.py:1507`, `:1508`, `:1125`, `:1130`.)

Permanecem estes consumidores sem discriminação suficiente:

| Consumidor | Tratamento encontrado |
|---|---|
| **Reversão** | Desvia antes do filtro da ida, busca qualquer PK do JSON e passa o dia do arquivo à porta de rechaveamento. Conversa passa pelo dedup e pelo E4-c; a porta pode escrever esse dia e trocar a célula. Com veto favorável e célula compatível, o caminho aceita a alteração. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:199`, `:474`, `:478`, `:485`, `:489`, `:493`; `app/chamados/models.py:2741`; `app/chamados/juizes.py:1997`; `app/chamados/services/fio.py:989`, `:990`.) |
| **Gate de Chamado** | Inclui conversa nas ancoradas, consulta competência pelo campo cru e procura cobrança sobrevivente desse dia. (`app/chamados/models.py:344`, `:364`, `:390`.) |
| **Pauta de competência** | Seleciona perguntas pela janela de `data_evento`, sem excluir conversa. (`app/chamados/services/competencia_trancada.py:106`.) |
| **Lavra** | Calcula expiração com `p.data_evento`; a documentação ainda associa conversa a NULL. (`app/chamados/veredito_lavrado.py:229`, `:107`.) |
| **Comparação/recusa de data** | Compara `data_evento` com o dia do chamado sem testar conversa; a materialização pode registrar `DATA_EM_DISPUTA`. (`app/chamados/services/fio.py:85`, `:88`; `app/chamados/services/materializacao.py:1191`, `:1213`.) |
| **Materialização de hora** | A seleção não exclui conversa. Uma conversa-hora pode alcançar código que usa `data_evento` para marcos, combinação de hora e ajuste overnight, se passar pelas demais guardas. (`app/chamados/services/materializacao.py:1369`, `:1221`, `:344`, `:371`, `:414`, `:421`.) |
| **Folha e tela do dia** | Agrupam/selecionam pelo campo cru, admitindo conversa. (`app/folha/previa.py:65`, `:67`; `app/ponto/services/dia_do_colab.py:65`.) |

Arquivo de reversão contendo conversa, conversa-hora materializada e efeitos desses caminhos em produção: **[nao verificado]**.

Há uma correção necessária à narrativa da triagem: o emissor normal **já barra pergunta de hora sem dia antes de chegar à conversa**. A linha 439, isoladamente, não demonstra emissão produtiva de conversa-hora; a porta aceita esse tipo e o teste novo o constrói diretamente. (`app/chamados/services/disputa_emissao.py:425`, `:430`, `:439`; `app/chamados/models.py:2764`; `app/chamados/tests/test_e1_v15f.py:64`.)

Não identifiquei no delta um novo escritor que escolha dia civil para uma linha de fato. A classificação ainda lê o antigo dia civil da saída para reconhecer o acervo a migrar; essa leitura é explícita, e o valor novo vem do juiz. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:102`, `:158`, `:160`.)

**4. Catracas e mutantes — atingem produção, mas a prova continua parcial**

O arnês extrai a função real, recompila uma substituição e instala o objeto no módulo/classe utilizado pelo caminho produtivo. O novo espião chama o contrato real e observa PK, dia relido e dia esperado. (`app/chamados/tests/test_e1_v15e_contrato_de_saida.py:331`, `:342`, `:349`, `:356`; `app/chamados/tests/test_e1_v15f.py:169`, `:182`, `:185`.)

As correções específicas estão presentes: atomic exato no wrapper, mutantes de remoção/perda do savepoint e M6 condicionado ao motivo `pergunta_unica_por_episodio`. (`app/chamados/tests/test_e1_v15e_contrato_de_saida.py:113`, `:206`, `:551`, `:567`.)

Permanecem mutantes plausíveis:

| Mutante | Lacuna |
|---|---|
| **Inverter PK de Disputa/Chamado/Pergunta no helper** | `violacoes_linhas` verifica rank, mas não compara PKs. A exclusão é expressa; o teste antigo de SQL crescente cobre Colaborador. (`app/chamados/tests/test_e1_v9.py:51`, `:94`, `:106`; `app/chamados/tests/test_e1_v13_ordem.py:59`.) |
| **Migrar com `if ok and ln['p'].chamado_destino_id is None:`** | A catraca AST continua encontrando o contrato. Os espiões novos usam candidatos construídos sem destino, deixando o ramo com destino sem testemunha nesses cenários. (`app/chamados/tests/test_e1_v15e_contrato_de_saida.py:147`; `app/chamados/tests/test_e1_v15f.py:182`; `app/chamados/tests/test_plantio_noturno_mais1.py:64`.) |
| **Retirar o atomic de `_reverter`** | A prova nova fornece seu próprio atomic ao redor da chamada e percorre sucesso. A catraca inspeciona `_migrar`, não `_reverter`; a base também é `TestCase`. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:465`; `app/chamados/tests/test_e1_v15f.py:151`; `app/chamados/tests/test_e1_v15e_contrato_de_saida.py:141`; `app/chamados/tests/test_plantio_noturno_mais1.py:39`.) |

O detector de ordem aceita sequência descendente dentro da classe, e o detector AST do migrar aceita a guarda condicional descrita; isso decorre dos predicados efetivamente implementados. Sobrevivência desses mutantes à **suíte inteira**: **[nao verificado]**. (`app/chamados/tests/test_e1_v9.py:98`; `app/chamados/tests/test_e1_v15e_contrato_de_saida.py:147`.)

**5. Linhas da seção “Volta 15f”**

A conferência abaixo é do vínculo estático teste→ramo; execução dos testes: **[nao verificado]**. [nao verificado]

| Linha | O teste percorre o anunciado? | [nao verificado]
|---|---|
| **A5** | **Parcial:** percorre o `raise` exato, mas ambas as datas são DIA; quem causa a recusa é `instancia_marco='X'`. Não demonstra episódio realmente em outro dia. (`app/chamados/tests/test_e1_v15f.py:286`, `:287`, `:292`; `app/chamados/services/fio.py:1688`; `app/chamados/models.py:2881`.) |
| **A6** | **Sim, nas formas declaradas:** real preserva K; injeção acrescenta retratada à eleição e alcança a recusa da canônica. (`app/chamados/tests/test_e1_v15f.py:307`, `:315`, `:316`; `app/chamados/models.py:2890`.) |
| **A12** | **Sim, com distinção:** o real recusa na entrada; somente a injeção alcança o segundo portão, com espião identificando a canônica. (`app/chamados/tests/test_e1_v15f.py:327`, `:339`, `:343`; `app/chamados/models.py:2930`.) |
| **G6** | **Sim:** injeta exceção na porta, exige erros/PAREI e igualdade das fotografias/trilha; não provoca escrita parcial antes da exceção. (`app/chamados/tests/test_e1_v15f.py:215`, `:217`, `:225`.) |
| **G7** | **Sim:** remove índice, cria gêmea, confirma fato conjunto e verifica migração da primeira/recusa da segunda. (`app/chamados/tests/test_e1_v15f.py:228`, `:236`, `:240`, `:246`.) |
| **G8** | **Sim:** observa primeira escrita real, faz a gêmea mentir sucesso e verifica retorno dos dois dias/trilhas, enquanto outro grupo migra. (`app/chamados/tests/test_e1_v15f.py:250`, `:257`, `:263`, `:266`, `:268`.) |
| **C1** | Absorção: corresponde ao ramo. (`app/chamados/tests/test_e1_v15f.py:361`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:70`.) |
| **C2** | Materializada sem plantada, sem via de absorção: corresponde. (`app/chamados/tests/test_e1_v15f.py:365`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:72`.) |
| **C3** | Ambiguidade de **plantadas**: corresponde; não cobre ambiguidade de turnos vivos. (`app/chamados/tests/test_e1_v15f.py:371`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:73`.) |
| **C4** | Cobre plantada sem turno encontrado; não cobre plantada associada a dois turnos. (`app/chamados/tests/test_e1_v15f.py:375`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:78`.) |
| **C5** | Cobre exclusão da materializada antes do julgamento do novo dia. (`app/chamados/tests/test_e1_v15f.py:380`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:158`.) |
| **C6** | Juiz None injetado: corresponde. (`app/chamados/tests/test_e1_v15f.py:386`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:161`.) |
| **C7** | Divergência juiz×turno injetada: corresponde. (`app/chamados/tests/test_e1_v15f.py:392`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:165`.) |
| **C8** | Órfã materializada fora do aval: corresponde. (`app/chamados/tests/test_e1_v15f.py:399`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:168`.) |
| **C9** | DRY real: corresponde. “Sem escrita” significa aqui **sem persistência**: a porta roda e o atomic desfaz. (`app/chamados/tests/test_e1_v15f.py:405`; `app/chamados/services/fio.py:1029`, `:1030`, `:1032`.) |
| **Conversa/contadores** | Corresponde à exclusão da ida e à contagem de conversa NULL. O teste do rótulo exige somente substring. (`app/chamados/tests/test_e1_v15f.py:72`, `:78`, `:96`, `:109`.) |
| **Ordem A8/A11 + G4/G3** | **Parcial:** combina rechave+costura; o migrar usa cenário limpo, não G3 vetado. Não prova A11 isolado sem rechave nem ordem crescente de PK. (`app/chamados/tests/test_e1_v15f.py:117`, `:140`, `:147`; `app/chamados/tests/test_e1_v9.py:51`.) |
| **Catraca abrir** | Corresponde à exigência estrutural do atomic e aos três mutantes novos. (`app/chamados/tests/test_e1_v15e_contrato_de_saida.py:113`, `:206`.) |
| **Catraca migrar** | Corresponde aos três candidatos do cenário e aos mutantes de remoção/instância velha; não demonstra todos os ramos possíveis. (`app/chamados/tests/test_e1_v15f.py:182`, `:194`, `:199`, `:203`.) |
| **M6** | Agora exige motivo específico; morre pelo índice. Não demonstra tratamento de acervo previamente duplicado. (`app/chamados/tests/test_e1_v15e_contrato_de_saida.py:551`, `:567`.) |

**Caminhos produtivos sem prova na extensão nova:** plantada em dois turnos; dois turnos só de saída; múltiplos turnos abertos candidatos da órfã; órfã sem marcos. C3/C4 não substituem essas provas. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:78`, `:105`, `:120`, `:123`; `app/chamados/tests/test_e1_v15f.py:371`, `:375`.)

**6. Quem escreve e quem lê os valores envolvidos no delta**

| Campo/valor | Escritores e persistência | Leitores envolvidos |
|---|---|---|
| **`data_evento`** | Atribuidor: `gravar_dia_do_fato`. Persistem nascimento, costura que data e rechaveamento. O valor vem do juiz/adaptador; na reversão vem do arquivo. (`app/chamados/services/fio.py:800`, `:863`, `:892`, `:989`, `:995`; `app/chamados/models.py:2973`, `:2974`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:478`.) | Identidade/dedup, contrato, contadores, classificação, veto/reversão, materialização e lavra; leitores sem discriminador estão discriminados no item 3. (`app/chamados/models.py:2726`; `app/chamados/services/fio.py:1085`, `:1058`, `:1125`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:97`, `:391`, `:485`; `app/chamados/services/materializacao.py:344`; `app/chamados/veredito_lavrado.py:229`.) |
| **`celula`** | Nascimento escolhe a do dia do fato; reuso troca a FK; rechaveamento escolhe a do novo dia. (`app/chamados/models.py:2860`, `:2924`, `:2964`, `:2971`; `app/chamados/services/fio.py:986`, `:990`, `:995`.) | Rechaveamento lê célula velha; reversão fotografa/compara a FK; juiz e sinal/evento leem a célula vinculada. (`app/chamados/services/fio.py:972`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:355`, `:493`; `app/chamados/juizes.py:1121`; `app/chamados/signals.py:244`.) |
| **`chamado_destino`** | Nascimento, reuso na porta e `reapontar_resolvedora`. A guarda terminal também altera/restaura temporariamente a instância. (`app/chamados/models.py:2971`, `:2940`, `:2964`; `app/chamados/services/fio.py:890`, `:892`, `:779`, `:783`.) | Pré-travas novas, âncora do fio, E4-c, guarda terminal e materialização. (`app/chamados/models.py:2912`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:377`, `:471`; `app/chamados/services/fio.py:97`, `:991`, `:768`; `app/chamados/services/materializacao.py:339`.) |
| **`universo`** | Contadores locais, incrementados antes dos descartes e devolvidos no dicionário; não são campo persistido. (`app/chamados/services/fio.py:1054`, `:1056`, `:1066`, `:1121`, `:1123`, `:1133`.) | Backfill imprime ambos. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:544`, `:550`.) |
| **`universo_rotulo`** | Literais produzidos pelos próprios contadores, declarando contagem anterior aos descartes. (`app/chamados/services/fio.py:1067`, `:1134`.) | Backfill imprime literalmente; teste exige `"ANTES dos descartes"`. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:544`, `:550`; `app/chamados/tests/test_e1_v15f.py:109`.) |

A exclusividade literal reivindicada para `chamado_destino` na docstri

[truncado em 20000 de 22136 caracteres]

## 08/11 17:55 · sessao bdaaabc83e2f · - · - · origem=Ronald (tmux)

Correção: a cadeia anterior **declaração → rechave de episódio BATIDA** não se sustenta: os motivos da declaração são MARCO e o derivador de episódio exige BATIDA. (`app/chamados/services/declaracao_texto.py:42`, `app/chamados/catalogo/motivos.py:34`, `app/chamados/catalogo/motivos.py:35`, `app/chamados/catalogo/motivos.py:48`, `app/chamados/catalogo/motivos.py:49`, `app/chamados/models.py:3416`.)

**Contraexemplo real de composição, mesmo dono, sem rechave: C→Ch→P→D.** O agrupador abre atomic e trava somente C; encontra uma pergunta ligada ao chamado, obtém sua disputa e complementa os marcos. (`app/chamados/services/disputa_emissao.py:964`, `app/chamados/services/disputa_emissao.py:968`, `app/chamados/services/disputa_emissao.py:979`, `app/chamados/services/disputa_emissao.py:982`, `app/chamados/services/disputa_emissao.py:990`.)

Premissa estática: um marco irmão está ausente dessa disputa, mas tem canônica em outra disputa do mesmo colaborador, já no dia correto e com destino diferente. `_ja_perguntou` consulta apenas a disputa atual, enquanto `abrir` deduplica entre disputas; portanto o caminho pode costurar e salvar a canônica existente. (`app/chamados/services/disputa_emissao.py:1316`, `app/chamados/services/disputa_emissao.py:829`, `app/chamados/services/disputa_emissao.py:834`, `app/chamados/models.py:2871`, `app/chamados/models.py:2926`, `app/chamados/models.py:2938`, `app/chamados/models.py:2964`.)

O helper retorna `True` também depois desse reuso, levando o agrupador a salvar a disputa atual **depois do UPDATE da Pergunta**, embora não a tenha travado antes. A chamada seguinte a `ch.analisar()` também pode adquirir disputas abertas depois da Pergunta. (`app/chamados/services/disputa_emissao.py:844`, `app/chamados/services/disputa_emissao.py:991`, `app/chamados/services/disputa_emissao.py:993`, `app/chamados/services/disputa_emissao.py:994`, `app/chamados/models.py:806`, `app/chamados/models.py:290`.)

Execução desse contraexemplo, ocorrência em produção e existência de um chamador produtivo BATIDA com atomic externo que alcance especificamente o novo bloco `2910`: **[nao verificado]**.

## 08/11 17:53 · sessao 2663eef25924 · - · - · origem=Ronald (tmux)

**Item 3: cura substancial, mas não matriz integral. Item 6: parcial.** Os novos testes exercitam os alvos reais e corrigem as falhas específicas anteriores; permanecem caminhos sem cobertura e mutantes plausíveis que os selos novos não detectam. (`app/chamados/tests/test_e1_v15f.py:165`, `app/chamados/tests/test_e1_v15e_contrato_de_saida.py:342`; lacunas abaixo.)

Suítes Django, banco, RED/GREEN históricos e sobrevivência de uma suíte inteira: **[nao verificado]**. Rodei somente AST/funções detectoras em memória, com `python3 -B`, sem importar Django nem escrever arquivos. **[nao verificado quanto a evidência persistida; resultados observados na ferramenta]**. [nao verificado]

| Linha 15f | Conferência estática do caminho citado |
|---|---|
| A5 | Percorre a guarda e exige tipo exato `FatoEmOutroDia`; o dado tem **mesmo dia**, mas `instancia_marco='X'`, portanto prova a guarda por identidade diferente, não episódio realmente noutro dia. (`test_e1_v15f.py:284`, `:287`, `:292`; guarda `models.py:2873`, `:2881`; recusa da identidade `fio.py:1688`.) |
| A6 | Real elege K; injeção inclui retratada no universo entregue ao juiz canônico e percorre `recusa_batida_retratada`. Declaração da injeção está correta. (`test_e1_v15f.py:296`, `:307`, `:312`; produção `models.py:2885`, `:2890`.) |
| A12 | Primeiro teste recusa na entrada, antes do reuso. Segundo desativa essa guarda e espião exige chamada com canônica, atingindo o gate do reuso. Não prova ocorrência natural do segundo gate como primeira recusa. (`test_e1_v15f.py:327`, `:334`, `:339`, `:343`; produção `models.py:2930`.) | [nao verificado]
| G6 | Injeta erro na porta real utilizada pelo comando, exige erros=3, PAREI, rollback e fotos iguais. Não há escrita parcial antes do erro nessa prova. (`test_e1_v15f.py:215`, `:217`, `:225`; produção `migrar_data_evento_dia_do_turno.py:400`, `:411`.) |
| G7 | Remove índice, cria gêmea do fato, confirma universo conjunto; apply exige primeira migrada e segunda recusada por colisão. (`test_e1_v15f.py:228`, `:236`, `:240`, `:246`.) |
| G8 | Primeiro membro usa porta real e grava; gêmea mente sucesso. Exige observação da escrita parcial, rollback de ambos e migração de outro grupo. Prova efetiva do atomic por grupo. (`test_e1_v15f.py:250`, `:257`, `:263`, `:266`, `:268`; contrato `migrar...py:407`, captura `:408`.) |
| C1 | Absorção efetiva em `_fonte_materializada`. (`test_e1_v15f.py:361`; `migrar...py:70`.) |
| C2 | Materializada sem plantada e sem via de absorção. (`test_e1_v15f.py:365`; `migrar...py:72`.) |
| C3 | Duas **batidas plantadas**, não dois turnos vivos candidatos. (`test_e1_v15f.py:371`; `migrar...py:73`.) |
| C4 | Plantada fora do intervalo usado para carregar turnos, nenhuma associação; prova `not ts`, não o outro ramo `len(ts)>1`. (`test_e1_v15f.py:375`; `migrar...py:77`, `:145`.) |
| C5 | Remove saída e atinge exclusão da materializada antes do juiz do novo dia. (`test_e1_v15f.py:380`; `migrar...py:158`.) |
| C6 | Juiz None injetado, atinge `_Fora sem_fonte`. (`test_e1_v15f.py:386`; `migrar...py:161`.) |
| C7 | Dia divergente injetado, atinge juiz × turno da janela. (`test_e1_v15f.py:392`; `migrar...py:165`.) |
| C8 | Materializa órfã e planta saída, atinge fora do aval. (`test_e1_v15f.py:399`; `migrar...py:168`.) |
| C9 | Chama DRY real com sucesso e colisão. “Sem escrita” deve significar **sem persistência**: a porta escreve e o atomic desfaz. (`test_e1_v15f.py:405`; `fio.py:1029`, `:1030`, `:1032`.) |
| Conversa | Porta real cria conversa datada; DRY/apply verificam exclusão/não migração; contador injeta NULL e compara com conversa datada. Rótulo só exige substring, não prova numeração do universo. (`test_e1_v15f.py:64`, `:72`, `:78`, `:96`, `:106`.) |
| Ordem A8/A11 + G4/G3 | Um cenário combina rechave + costura; migrar usa base limpa, três migrantes, e reversão. **Não exercita G3 vetado**, nem prova A11 sem rechave; não verifica pk crescente. (`test_e1_v15f.py:117`, `:140`, `:147`; rastreador `test_e1_v9.py:51`, `:94`.) |
| Catraca abrir | Agora exige exatamente `transaction.atomic()` como dono do bloco `_nascer` + contrato; mutações if True/savepoint=False/nullcontext são verificadas. (`test_e1_v15e_contrato_de_saida.py:109`, `:113`, `:206`.) |
| Catraca migrar | Espião chama contrato real e verifica três PKs, dia relido e dia esperado; recompila `_migrar` real e muta chamada real. (`test_e1_v15f.py:165`, `:182`, `:185`, `:194`, `:199`, `:203`.) |
| M6 | Deixou de aceitar motivo arbitrário: agora exige substring `pergunta_unica_por_episodio`. Prova morte pelo índice, não tratamento de acervo já duplicado. (`test_e1_v15e_contrato_de_saida.py:471`, `:548`, `:561`, `:567`.) |

**Caminhos reais ainda sem linha/prova na extensão nova:** plantada associada a dois turnos; dois turnos só de saída; múltiplos turnos abertos candidatos da órfã; órfã sem marcos. São ramos distintos dos C3/C4 adicionados. (`migrar_data_evento_dia_do_turno.py:78`, `:105`, `:120`, `:123`; novos C3/C4 `test_e1_v15f.py:371`, `:375`.) A seção fornecida admite alguns, mas omite explicitamente os dois ramos adicionais `:78` e `:120` na lista final. **[nao verificado quanto à matriz não existente como arquivo na cópia]**.

**Mutantes plausíveis ainda descobertos:**

1. **Ordem invertida dentro da classe:** trocar apenas `order_by('pk')` por `order_by('-pk')` na query de Pergunta/Chamado/Disputa do helper. `violacoes_linhas` só acompanha rank, presença de algum Colaborador e PK desconhecida; não compara PKs. Seq descendente foi aceita em execução isolada do detector. (`test_e1_v9.py:94`, `:98`, `:106`, `:109`; exclusão explícita `:51`.) O teste antigo SQL ASC cobre **só Colaborador**, portanto escolher mutante em outra classe. (`test_e1_v13_ordem.py:59`, `:68`, `:71`.) [nao verificado]

2. **Contrato pulado num ramo do migrar:** trocar a guarda por `if ok and ln['p'].chamado_destino_id is None:`. O detector AST continua devolvendo `[]`: ele busca chamada posterior em qualquer descendente do corpo do try, sem testar condição. (`test_e1_v15e_contrato_de_saida.py:143`, `:147`, `:149`; guarda de produção `migrar...py:406`.) Novos espiões usam migrantes da `_Cenario`, produzidos sem destino: `_pergunta` chama abrir sem destino, e gêmea também não passa destino. Assim o ramo com destino fica sem testemunha nesses testes. (`test_plantio_noturno_mais1.py:62`, `:64`; `test_migrar_data_evento_dia_do_turno.py:55`, `:65`, `:68`; `test_e1_v15f.py:233`, `:182`.) Sobrevivência da suíte completa **[nao verificado]**; sobrevivência do detector AST confirmada em memória. [nao verificado]

3. **Atomic retirado da reversão:** substituir o `with transaction.atomic()` da reversão por `if True:`. A nova prova já fornece seu próprio atomic ao redor da chamada e só percorre sucesso; as bases são `django.test.TestCase`. Portanto não distingue transação da porta e transação fornecida pelo teste. (`migrar...py:465`; `test_e1_v15f.py:151`, `:153`; `_Cenario` em `test_migrar...py:48`; base `test_plantio_noturno_mais1.py:39`.) A catraca inspeciona somente `_migrar`, não `_reverter`. (`test_e1_v15e_contrato_de_saida.py:141`.) Suíte completa **[nao verificado]**. [nao verificado]

O arnês não é derivação paralela manual: extrai AST/trecho da função real, recompila com uma substituição e instala objeto no módulo/classe de produção. (`test_e1_v15e_contrato_de_saida.py:331`, `:342`, `:349`, `:356`; `test_e1_v15f.py:185`.)

_2922 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
