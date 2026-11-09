# CODEX-ESTADO

> Ultima resposta processada: **08/11/2026 23:36** · publicado em **08/11/2026 23:38** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 08/11 23:36 · sessao 1256d4b89cb3 · BUG-DEPLOY-PORTAO · verificador · origem=Code

A volta 3 elimina a **recusa por cron após um apply bem-sucedido do core**, mas passa a permitir o restart sobre um cron recém-nascido. A afirmação “morto e refeito” não está sustentada por mecanismo de repetição; além disso, o selo de configuração continua podendo reprovar depois da publicação. (`bin/deploy.sh:300`, `bin/deploy.sh:325`, `bin/deploy.sh:296`, `bin/cron_run.sh:11`, `bin/cron_run.sh:38`, `bin/deploy.sh:409`)

A análise abaixo é estática. RED/VERDE, execução dos mutantes e efeitos reais em produção: **[nao verificado]**. Não executei o selo porque ele cria e remove arquivos. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:44`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:107`)

1) É preciso separar **passar pela posição da seção 1c** de **ter executado seu apply com sucesso**. O apply é condicional; sua falha encerra o script imediatamente. (`bin/deploy.sh:298`, `bin/deploy.sh:300`, `bin/deploy.sh:92`)

| Modo | Apply da 1c e caminhos que podem terminar sem completar a publicação das três cascas |
|---|---|
| Normal | Com pendência, faz apply; pode terminar por falha do próprio migrate, do HUP ou da troca dos workers. Sem pendência, pula apply e permanece sujeito às falhas de HUP/troca. (`bin/deploy.sh:223`, `bin/deploy.sh:300`, `bin/deploy.sh:343`, `bin/deploy.sh:356`) |
| `--sem-migrate` | Com pendência, recusa **antes** da 1c. Se chega à seção 2, não fez apply e pode falhar no HUP/troca. (`bin/deploy.sh:215`, `bin/deploy.sh:216`, `bin/deploy.sh:61`, `bin/deploy.sh:343`, `bin/deploy.sh:356`) |
| `--sem-sombra` | Pode migrar normalmente; depois disso, tem os mesmos caminhos de falha de migrate/HUP/troca do normal. O modo dispensa sombra por motivo, não migration nem auth. (`bin/deploy.sh:87`, `bin/deploy.sh:89`, `bin/deploy.sh:143`, `bin/deploy.sh:152`, `bin/deploy.sh:300`, `bin/deploy.sh:343`, `bin/deploy.sh:356`) |
| `--reload-agendado` | Com pendência, recusa antes da 1c. Sem pendência, não aplica migrate; pode falhar no HUP/troca. (`bin/deploy.sh:65`, `bin/deploy.sh:211`, `bin/deploy.sh:212`, `bin/deploy.sh:343`, `bin/deploy.sh:356`) |
| `--reload-copia` | Com pendência, recusa antes da 1c. Sem pendência, não aplica migrate; pode falhar no HUP/troca. (`bin/deploy.sh:82`, `bin/deploy.sh:208`, `bin/deploy.sh:209`, `bin/deploy.sh:343`, `bin/deploy.sh:356`) |
| `--restart` | Com apply bem-sucedido, pula a recusa de cron na seção 2 e tenta restart; pode terminar por falha do migrate ou do restart. **Sem apply**, pode terminar por **RECUSA de cron** na seção 2, antes de chamar restart. (`bin/deploy.sh:300`, `bin/deploy.sh:325`, `bin/deploy.sh:326`, `bin/deploy.sh:112`) |
| `--conferir` | Não alcança a 1c: termina antes, sem publicação. (`bin/deploy.sh:227`, `bin/deploy.sh:229`) |

Depois de uma chamada de HUP/restart, ainda existem estas saídas explícitas:

| Saída | O que permite concluir |
|---|---|
| Timeout de disponibilidade de qualquer casca | A publicação foi tentada; não se confirmou que as três ficaram disponíveis. (`bin/deploy.sh:365`, `bin/deploy.sh:366`, `bin/deploy.sh:368`) |
| Uma ou mais rotas reprovadas | A publicação foi tentada; a prova funcional falhou. Não demonstra, por si só, ausência de recarga. (`bin/deploy.sh:399`, `bin/deploy.sh:400`, `bin/deploy.sh:401`, `bin/deploy.sh:402`) |
| Selo de reciclagem vermelho | Reprova depois da tentativa de publicação por configuração/cmdline/compose/crontab. É uma guarda de conformidade posterior, não apenas erro operacional de HUP/restart. (`bin/deploy.sh:409`, `bin/selo_reciclagem.sh:46`, `bin/selo_reciclagem.sh:54`, `bin/selo_reciclagem.sh:64`, `bin/selo_reciclagem.sh:72`, `bin/selo_reciclagem.sh:78`) |
| Final “OK” sem PIDs anteriores conhecidos | O código admite continuar apenas com aviso quando não consegue listar os workers anteriores. Nesse caminho, não exige prova da troca deles. Recarga efetiva: **[nao verificado]**. (`bin/deploy.sh:341`, `bin/deploy.sh:346`, `bin/deploy.sh:431`) |

Portanto: **há RECUSA depois da posição da 1c**, no `--restart` **sem apply**, pela marca de cron. **Não há essa recusa depois de apply bem-sucedido**; o curto-circuito a desabilita. (`bin/deploy.sh:325`, `bin/deploy.sh:112`)

Falhas do apply, HUP ou restart não garantem ausência de efeitos parciais; o script encerra sem executar recuperação nesses ramos. Efeito parcial real: **[nao verificado]**. (`bin/deploy.sh:300`, `bin/deploy.sh:326`, `bin/deploy.sh:343`, `bin/deploy.sh:92`)

A frase literal “todo portão antes do migrate” continua mais forte que o código: o selo de reciclagem permanece depois dele, embora esse selo já não impeça a tentativa de publicação. (`bin/deploy.sh:300`, `bin/deploy.sh:409`)

2) O censo de escritores e leitores é este:

| Estado | Quem escreve | Quem lê |
|---|---|---|
| `APLICAR_MIGRATE` | Inicialização `0`; seleção elegível `1`. São dois pontos de atribuição no mesmo script. (`bin/deploy.sh:61`, `bin/deploy.sh:223`) | Decide o apply e decide dispensar a recusa posterior de cron. Portanto são **dois leitores**, contrariando o comentário “UM leitor”. (`bin/deploy.sh:298`, `bin/deploy.sh:325`, `bin/deploy.sh:59`) |
| `RESTART` | Inicialização `0`; somente o primeiro argumento `--restart` seleciona `1`. (`bin/deploy.sh:70`, `bin/deploy.sh:72`, `bin/deploy.sh:73`) | Recusa inicial de cron; exceção da pendência da mensageria; recusa pré-apply; seleção restart/HUP; mensagem final. (`bin/deploy.sh:115`, `bin/deploy.sh:200`, `bin/deploy.sh:299`, `bin/deploy.sh:322`, `bin/deploy.sh:428`) |
| Marcas de produção | O envelope cria diretório, escreve `<nome>_<empresa>.<PID>` e remove no `EXIT`. **Deploy também escreve no ciclo de vida: apaga marcas consideradas mortas.** (`bin/cron_run.sh:9`, `bin/cron_run.sh:10`, `bin/deploy.sh:109`) | Deploy enumera arquivos, testa `/proc/<PID>`, extrai o nome e usa o resultado para recusar. Não lê o timestamp do conteúdo. (`bin/deploy.sh:108`, `bin/deploy.sh:109`, `bin/deploy.sh:111`, `bin/deploy.sh:112`) |

Fora do deploy: [nao verificado]

- O selo desta obra remove `APLICAR_MIGRATE` herdado e o injeta novamente quando `FAKE_HERDA_APLICAR` está presente; o caso 10 seleciona essa injeção. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:90`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:146`)
- O contrato estrutural lê o **texto** do teste de `RESTART`, não o valor da variável. Também procura textualmente a presença do mecanismo de marcas nos dois scripts. (`bin/contrato_deploy.sh:41`, `bin/contrato_deploy.sh:81`, `bin/contrato_deploy.sh:82`)
- O selo gracioso escreve marca com PID do processo falso; o selo desta obra escreve `cron_falso.1`. As respectivas limpezas removem as árvores falsas. (`bin/tests/test_deploy_reload_gracioso.sh:90`, `bin/tests/test_deploy_reload_gracioso.sh:91`, `bin/tests/test_deploy_reload_gracioso.sh:104`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:68`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:107`)

**Os valores originais de `APLICAR_MIGRATE` e `RESTART` herdados do ambiente não alcançam seus leitores:** ambos são sobrescritos primeiro. (`bin/deploy.sh:61`, `bin/deploy.sh:70`, `bin/deploy.sh:298`, `bin/deploy.sh:115`)

Isso não significa ambiente neutralizado em geral. Continuam sendo entradas deliberadas: `DEPLOY_SEM_SOMBRA`, `SEM_JANELA_AUTH_MOTIVO`, `SEM_PROVA_DE_CASCA`, `HOST_TENANT` e as costuras `JANELA_AUTH_STAMP/DOW/HHMM`. Elas alcançam leitores e podem mudar o comportamento correspondente. (`bin/deploy.sh:85`, `bin/deploy.sh:143`, `bin/deploy.sh:282`, `bin/deploy.sh:376`, `bin/janela_auth.sh:36`, `bin/janela_auth.sh:49`, `bin/janela_auth.sh:50`)

O escritor das marcas usa caminho absoluto, enquanto deploy lê relativo à sua `RAIZ`. A equivalência desses diretórios em produção é **[nao verificado]**. (`bin/cron_run.sh:9`, `bin/deploy.sh:53`, `bin/deploy.sh:108`)

3) No cenário perguntado, com apply do core bem-sucedido:

- **Volta 2:** consultava novamente as marcas e podia recusar antes do restart, deixando o apply executado sem publicar as cascas. A chamada incondicional removida estava em `bin/deploy.sh` da volta 2:318, conforme o hunk `-315/+321` do anexo `codex_deployportao3.diff`.
- **Volta 3:** `APLICAR_MIGRATE=1` dispensa essa consulta; o restart é chamado apesar do cron recém-nascido. (`bin/deploy.sh:325`, `bin/deploy.sh:326`)
- Se esse cron estiver executando por `docker exec` numa casca reiniciada, ele fica exposto à interrupção que o próprio script documenta. Morte concreta e código de saída nesta produção: **[nao verificado]**. (`bin/deploy.sh:311`, `bin/deploy.sh:314`)

“Idempotente” não implica “automaticamente reexecutado”. O envelope executa o comando uma vez, carimba conclusão apenas para rc `0/2`, tenta alarmar nas outras falhas e sai; não contém retry. A idempotência universal é requisito textual, não prova apresentada. (`bin/cron_run.sh:11`, `bin/cron_run.sh:22`, `bin/cron_run.sh:32`, `bin/cron_run.sh:38`, `CLAUDE.md:246`)

A interrupção pode ainda deixar dependentes sem carimbo de conclusão: eles aguardam o arquivo e alarmam ao atingir o teto. A recuperação efetiva da corrente é **[nao verificado]**. (`bin/cron_run.sh:25`, `bin/encadeado.sh:89`, `bin/encadeado.sh:92`, `bin/encadeado.sh:94`)

Uma ordem concebível seria **bloquear novas admissões de cron → drenar os existentes → manter o bloqueio durante migrate/publicação → liberar**. Isso exigiria uma exclusão compartilhada entre envelope e deploy, ausente na sequência atual; sua implementação e viabilidade aqui são **[nao verificado]**. (`bin/cron_run.sh:9`, `bin/cron_run.sh:11`, `bin/deploy.sh:299`, `bin/deploy.sh:300`, `bin/deploy.sh:326`)

Para prometer também ausência absoluta de atendimento com Python velho sobre schema novo, seria necessário tratar o tráfego durante migrate/publicação, por pausa/drenagem ou compatibilidade das migrations. Essa alternativa não está sendo proposta como correta para esta aplicação: **[nao verificado]**.

4) Estes são os mutantes mortos **por inspeção das asserções**, não por execução:

| Caso | Mutante que distingue |
|---|---|
| **1** | Migrar antes da recusa de auth, ou ignorá-la no normal: exige falha, zero apply e zero HUP. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:110`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:111`) |
| **2** | Migrar antes da recusa de sombra, ou ignorá-la: mesmas exigências. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:114`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:115`) |
| **3** | Omitir/duplicar apply; aplicar antes da primeira auth/sombra/casca; trocar `sombra --conferir` por outra operação; omitir HUP. Exige contagens e primeira passagem dos três portões anterior ao apply. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:95`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:97`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:103`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:119`) |
| **4** | Isentar o agendado da **chamada** à auth. Ignorar apenas seu retorno pode escapar deste caso, porque a pendência já oferece outra recusa. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:122`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:123`, `bin/deploy.sh:211`) |
| **5** | Perguntar auth/sombra, aplicar migrate ou enviar HUP em `--conferir`. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:126`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:127`) |
| **6** | Aplicar antes da prova de casca recusar, ou ignorar sua recusa. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:130`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:131`) |
| **7** | Fazer `--sem-sombra` também dispensar auth, ou migrar antes da recusa dela. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:134`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:135`) |
| **8** | Aplicar sem pendência; deixar de enviar HUP no caminho sem pendência. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:138`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:139`) |
| **9** | Consultar auth no agendado e ignorar seu retorno: não há pendência para mascarar o erro. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:142`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:143`) |
| **10** | Deixar `APLICAR_MIGRATE=1` herdado provocar apply em `--sem-migrate`. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:90`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:146`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:147`) |
| **11** | Restaurar a recusa incondicional da volta 2 depois do apply. A mordida depende de `/proc/1` existir: a marca falsa usa PID fixo `1`, e o leitor consulta `/proc`. Essa precondição ambiental é **[nao verificado]**. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:68`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:150`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:151`, `bin/deploy.sh:109`) |

Mutantes plausíveis que **ainda sobrevivem aos 11 por inspeção**; execução: **[nao verificado]**:

- **Mover o bloco inteiro da 1c para depois da seção 2.** `ORDEM_OK` compara portões com apply, mas não compara apply com HUP/restart. No caso 11, o restart passa a acontecer antes de a marca nascer, e as contagens continuam aceitas. É o sobrevivente mais diretamente contrário ao pré-requisito schema → publicação. (`bin/deploy.sh:298`, `bin/deploy.sh:358`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:103`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:119`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:151`)
- **Remover as recusas prévias de cron.** Nenhum cenário instala cron antes delas; a única marca deste selo nasce durante o apply falso. (`bin/deploy.sh:117`, `bin/deploy.sh:299`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:45`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:68`)
- **Remover a guarda de pendência da mensageria.** O falso responde sempre sucesso ao check dela. (`bin/deploy.sh:200`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:69`)
- **Remover a recusa de pendência do agendado ou de `--sem-migrate`.** O agendado com pendência já encontra auth vermelha; `--sem-migrate` só é exercitado sem pendência. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:122`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:142`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:146`, `bin/deploy.sh:211`, `bin/deploy.sh:215`)
- **Dispensar auth/sombra no `--reload-copia`.** Nenhum dos onze casos seleciona esse modo. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:110`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:118`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:122`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:126`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:130`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:134`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:138`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:142`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:146`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:150`)
- **Acrescentar uma segunda consulta de auth/sombra/casca depois do apply.** Os falsos mantêm a mesma resposta durante toda a corrida; os vermelhos já encerraram antes, e os verdes continuam verdes. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:54`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:55`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:56`)
- **Reiniciar somente `saas_core`.** `N_RST` conta chamadas, não alvos; restart e rotas falsos continuam verdes. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:70`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:76`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:101`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:151`)
- **Retirar provas de workers/readiness/rotas, ignorar falha de apply/restart ou retirar o selo de reciclagem.** As asserções não exigem essas provas, os comandos relevantes não oferecem falha na fixture, e o selo é substituído por sucesso constante. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:51`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:68`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:70`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:76`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:119`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:151`)

5) Para as classes históricas, distinguindo alteração nova de problema já presente:

| Classe | Sim/não e prova |
|---|---|
| Segundo juiz que duplica juiz registrado | **Não no diff.** Deploy continua perguntando à auth existente; o selo substitui respostas, sem reproduzir a regra de horário. (`bin/deploy.sh:145`, `bin/janela_auth.sh:53`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:54`) |
| Segundo escritor fora da porta | **Sim no ciclo de vida das marcas, já existente:** envelope escreve/remove e deploy também remove. **Não aparece novo aplicador/publicador no diff.** (`bin/cron_run.sh:10`, `bin/deploy.sh:109`, `bin/deploy.sh:300`, `bin/deploy.sh:326`, `bin/deploy.sh:343`) |
| Relógio lido fora da borda | **Não introduzido pelo diff.** A decisão temporal de auth continua usando suas costuras declaradas; o novo cron falso não consulta relógio. (`bin/janela_auth.sh:49`, `bin/janela_auth.sh:50`, `bin/janela_auth.sh:55`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:68`) |
| Derivação paralela de fonte única | **Não no diff.** A elegibilidade do apply é decidida no ramo da seção 1; os dois pontos seguintes consomem a mesma variável. (`bin/deploy.sh:204`, `bin/deploy.sh:223`, `bin/deploy.sh:298`, `bin/deploy.sh:325`) |
| Selo/RED sem mordida | **Sim para partes do contrato, não para o selo inteiro.** Há mordida contra a recusa pós-apply, mas falta contra publicação antes do apply e contra a promessa de cron refeito. RED realmente demonstrado: **[nao verificado]**. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:103`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:151`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:76`) |
| Ordem de trava | **Sim: falta reservar o intervalo.** Observar marcas antes de migrar não impede novos crons entre observação e restart; o envelope marca/executa sem exclusão compartilhada. (`bin/deploy.sh:299`, `bin/deploy.sh:300`, `bin/deploy.sh:325`, `bin/deploy.sh:326`, `bin/cron_run.sh:9`, `bin/cron_run.sh:11`) |
| Pouso antes do pré-requisito | **Sim quanto ao pré-requisito ainda escrito “restart recusa com cron vivo”.** A nova exceção publica sem essa condição no cenário 11. Quanto à ordem core migrate → publicação, **não no código atual**, embora o selo aceite sua inversão. (`CLAUDE.md:90`, `bin/deploy.sh:325`, `bin/deploy.sh:326`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:150`, `bin/deploy.sh:300`) |
| Campo com dois significados | **Não demonstrado nos flags alterados.** O novo uso de `APLICAR_MIGRATE` depende corretamente do fluxo anterior; os limites das marcas e dos contadores estão discriminados abaixo. (`bin/deploy.sh:223`, `bin/deploy.sh:298`, `bin/deploy.sh:300`, `bin/deploy.sh:325`) |

O censo semântico das variáveis lidas/escritas pelas linhas alteradas é:

| Campos/variáveis | Concordância entre escritores e leitores |
|---|---|
| `APLICAR_MIGRATE` | `1` nasce como intenção elegível. Ao alcançar o leitor da seção 2, o comando já retornou sucesso, pois falha termina o script. Logo a inferência nesse leitor é válida; não prova alteração material do banco. O comentário de “UM leitor” está errado. (`bin/deploy.sh:223`, `bin/deploy.sh:300`, `bin/deploy.sh:92`, `bin/deploy.sh:325`, `bin/deploy.sh:59`) |
| `RESTART`, `RESTART_MOTIVO` | Significam seleção manual e justificativa textual. Escrita e leituras concordam; o motivo não prova necessidade técnica do boot. (`bin/deploy.sh:70`, `bin/deploy.sh:73`, `bin/deploy.sh:74`, `bin/deploy.sh:75`, `bin/deploy.sh:322`, `bin/deploy.sh:429`) |
| `FAKE_CRON_NO_MIGRATE`, `FAKE_HERDA_APLICAR` | São habilitadores por **presença não vazia**, não booleanos numéricos gerais. Os casos fornecidos usam `1`, coerentemente. (`bin/tests/test_deploy_portoes_antes_do_migrate.s

[truncado em 20000 de 22901 caracteres]

## 08/11 23:35 · sessao 4ff4663f72a0 · FIO-E0-V4-E1-FILA · verificador · origem=Code

**A captura está no lugar correto para isolar a recusa da migração, mas não torna atômico o ato maior dos chamadores. Há retornos `False` ignorados e contadores que anunciam trabalho recusado.** (`app/chamados/models.py:1414-1425`; `app/chamados/services/adesao.py:92`; `app/ponto/management/commands/reconciliar_grade.py:254-259`, `:529-531`.)

Análise estática da cópia. Execução dos testes, comportamento efetivo do banco e ocorrências em produção: **[nao verificado]**. [nao verificado]

**1. Quem chama `ChamadoColaborador.reabrir`**

No quadro, “sem atomic” significa sem envoltório explícito no caminho identificado. Os dicionários de banco da cópia não configuram `ATOMIC_REQUESTS`; configuração efetiva de produção: **[nao verificado]**. (`app/config/settings/prod.py:9-18`; `app/config/settings/saas.py:83-92`.)

| Chamador produtivo direto | Atomic externo à migração | Tratamento de `False` |
|---|---|---|
| `_renascer_por_premissa_viva` — `app/chamados/models.py:1019` | **Sim via `abrir`**, que abre atomic antes de chamar o renascimento. **Sem atomic identificado via resposta tardia**. (`app/chamados/models.py:902`, `:922`; `app/chamados/services/resposta_tardia.py:31-45`.) | Propaga `False`. `abrir` descarta esse resultado e retorna `(existente, False)`: esse segundo booleano significa **criado**, não reaberto. Resposta tardia registra nota/log e devolve `tardia`. (`app/chamados/models.py:1019-1024`, `:922-924`; `app/chamados/services/resposta_tardia.py:45-67`.) |
| Desarquivar — `app/chamados/views_arquivar.py:70` | Sem atomic identificado. (`app/chamados/views_arquivar.py:51-70`.) | Retorna `ok=False`, mas atribui **qualquer recusa a irmão vivo**. Para universo alterado, apresenta razão incorreta. (`app/chamados/views_arquivar.py:70-72`.) |
| Adesão — `app/chamados/services/adesao.py:92` | Sem atomic identificado no serviço/comando. (`app/chamados/services/adesao.py:44-92`; `app/escala/management/commands/apurar_furos_diarios.py:6-21`.) | **Ignora e incrementa `pais_reabertos` incondicionalmente**; o comando apresenta esse contador. (`app/chamados/services/adesao.py:92`; `app/escala/management/commands/apurar_furos_diarios.py:21`.) |
| Reabrir disputa administrativamente — `app/chamados/services/acoes_disputa.py:267` | Sem atomic identificado no serviço, dispatcher ou views de entrada. (`app/chamados/services/acoes_disputa.py:232-296`; `app/chamados/services/fio.py:583`; `app/chamados/views.py:658-674`, `:868-954`.) | **Ignora**; segue com ocorrência, pushes, auditoria e sucesso. As mensagens anunciam a **disputa**, efetivamente reaberta antes; não revelam que o chamado recusou. (`app/chamados/services/acoes_disputa.py:256-296`.) |
| Reabrir competência — `app/chamados/services/competencia_trancada.py:265` | **Sim**, atomic por chamado. (`app/chamados/services/competencia_trancada.py:264`.) | Só escreve nota/evento e incrementa se `True`; `False` pula esses efeitos. (`app/chamados/services/competencia_trancada.py:265-272`.) |
| Resolver perguntas de chamado resolvido — `app/chamados/services/disputa_emissao.py:1812` | **Sim**: o wrapper aproveita atomic existente ou abre um. O desvio da competência trancada retorna antes de alcançar essa chamada. (`app/chamados/services/disputa_emissao.py:1715-1722`, `:1742-1743`.) | Só anexa nota de reabertura se `True`; segue o processamento depois da recusa. (`app/chamados/services/disputa_emissao.py:1812-1830`.) |
| Reabrir órfãs — `app/chamados/management/commands/reabrir_orfas_enterradas.py:172` | **Sim**, atomic por pergunta. (`app/chamados/management/commands/reabrir_orfas_enterradas.py:146`.) | Descarta o booleano, **relê o estado** e registra `pai_falhou` se continua encerrado; imprime falhas. (`app/chamados/management/commands/reabrir_orfas_enterradas.py:172-189`.) |
| Ausência ainda aberta — `app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:49` | Sem atomic identificado. (`app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:22-55`.) | Só nota e incremento se `True`; nenhuma explicação própria para `False`. (`app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:49-55`.) |
| Reconciliar grade — `app/ponto/management/commands/reconciliar_grade.py:530` | Sem atomic identificado nesse caminho. (`app/ponto/management/commands/reconciliar_grade.py:488-531`.) | **Ignora e retorna `None`**; o chamador entende ausência de veto e incrementa `CREATE`, incluindo a entrada em `creates`. (`app/ponto/management/commands/reconciliar_grade.py:529-531`, `:254-259`.) |
| Detectar ausências/limbo — `app/ponto/management/commands/detectar_ausencias.py:301` | Sem atomic identificado nesse caminho. (`app/ponto/management/commands/detectar_ausencias.py:271-304`.) | Ignora; não anuncia reabertura nem registra explicação própria nesse trecho. (`app/ponto/management/commands/detectar_ausencias.py:300-304`.) |

Para a **nova exceção**, esses chamadores não eliminam a tentativa de trilha: a própria porta chama `evento` antes de devolver `False`. A persistência é best-effort, portanto não há garantia estática de linha gravada. Outras recusas, como `agrupado` e `transicao`, continuam retornando sem evento próprio nesses ramos. (`app/chamados/models.py:1421-1425`, `:1406-1407`, `:1437-1438`; `app/core/observ.py:105-139`.)

**2. Savepoint, conexão utilizável e escritas anteriores**

**Sob atomic externo saudável, sim, por desenho:** a migração entra em `transaction.atomic()` padrão; a exceção sai desse bloco antes de ser capturada por `reabrir`. O rollback pertence ao savepoint interno, preservando o atomic externo. O repositório documenta o mesmo mecanismo de isolamento na implementação de `evento`. Verificação efetiva da conexão após essa sequência: **[nao verificado]**. (`app/chamados/services/fio.py:2074-2094`; `app/chamados/models.py:1414-1417`; `app/core/observ.py:124-133`.)

Isso cobre os caminhos com atomic da tabela: `abrir`, competência, resolver perguntas e reabrir órfãs. Nos demais caminhos, sob autocommit normal, o atomic da migração é a **transação externa**, não savepoint; sua saída excepcional encerra essa transação antes da captura. Configuração operacional de autocommit: **[nao verificado]**. (`app/chamados/models.py:902`; `app/chamados/services/competencia_trancada.py:264`; `app/chamados/services/disputa_emissao.py:1720`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:146`; `app/chamados/services/fio.py:2074`; `app/chamados/models.py:1417`.)

**Não há migração parcialmente escrita nesse ramo:** o `raise` ocorre antes de `_migrar_sob_trava`; até ali há leituras e aquisição de travas, sem alteração de dados de negócio pela migração. (`app/chamados/services/fio.py:2084-2094`; `app/chamados/models.py:2674-2701`.)

**Há atos maiores que permanecem parcialmente realizados:**

- **Admin, sem atomic abrangente:** a disputa é reaberta e nasce pergunta de conversa **antes** da tentativa sobre o chamado; `False` não desfaz essas escritas e não impede os efeitos posteriores. (`app/chamados/services/acoes_disputa.py:256-296`.)
- **Órfãs, mesmo com atomic:** pergunta e disputa já foram reabertas; a recusa do pai vira `pai_falhou`, sem rollback dessas etapas anteriores. (`app/chamados/management/commands/reabrir_orfas_enterradas.py:157-177`.)
- **Competência:** `PeriodoFechado` é salvo antes do gancho best-effort que devolve a fila; os fechamentos mensais são atualizados depois. Não há transação nova para o conjunto. (`app/ponto/services/fechamento.py:1435-1452`.)

Logo, o comentário “o ato inteiro desfez” vale para **a migração**, não para todo trabalho anteriormente executado pelo chamador. (`app/chamados/services/fio.py:2026-2029`, `:2074-2094`; `app/chamados/services/acoes_disputa.py:256-267`.)

**3. Escritores e leitores de `reabertura_recusada` / `depois['motivo']`**

Os emissores produtivos encontrados são:

| Emissor | Conteúdo de `depois` |
|---|---|
| `_trilha_do_veto`, para renascimento/reabertura | `modulo_origem`, `dia`, **`veto`**, `via`, `colaborador`. (`app/chamados/models.py:1166-1188`.) |
| Admin sem fato novo | `status_local`, `via_encerramento`. (`app/chamados/models.py:1396-1398`.) |
| Premissa regularizada | `status_local`. (`app/chamados/models.py:1402-1404`.) |
| Universo alterado sob trava | `irmao`, **`motivo='universo_mudou_sob_trava'`**, `erro`. (`app/chamados/models.py:1421-1424`.) |

O leitor específico encontrado é a **deduplicação do veto**, que exige também `modulo_origem`, `dia`, `veto` e `via`. O novo evento não satisfaz essa chave e não suprime aquela trilha. (`app/chamados/models.py:1172-1183`, `:1424`.)

Não localizei leitor produtivo específico de `motivo` para essa ação; o teste lê explicitamente esse campo. Completude diante de acesso dinâmico/reflexão: **[nao verificado]**. (`app/chamados/tests/test_e1_fila.py:517`.)

Os leitores genéricos relevantes têm estes comportamentos:

- A auditoria consulta todas as ações, filtra ação literal e conta registros, sem classificar motivos. Seu seletor usa `ACOES`, que **não oferece** `reabertura_recusada`; o filtro por GET continua aceitando a ação. (`app/core/views.py:705-738`; `app/core/models.py:286-294`.)
- O vigia de escritas por cron conta **linhas de auditoria**, independentemente da ação. A recusa entra nessa contagem, coerente com o rótulo declarado; não significa reabertura realizada. (`app/chamados/services/vigia_loop.py:68-96`.)
- Os vigias de transição e confronto trilha/banco, e o histórico de estados, filtram `estado_chamado`; não interpretam essa recusa como mudança de estado. (`app/chamados/services/vigia_loop.py:28-34`, `:114-121`; `app/chamados/services/historico.py:44`, `:86-95`.)

Não há prova encontrada de leitor contando errado por presumir um conjunto antigo de motivos. Há **payloads heterogêneos**, sem `motivo` obrigatório em todas as recusas; o sentido comum do evento continua sendo recusa de reabertura. (`app/chamados/models.py:1187`, `:1398`, `:1404`, `:1424`.)

**4. Mordida dos testes novos e mutantes sobreviventes** [nao verificado]

**`A3AplicarDeclaracaoTest`: morde a retirada simples da chamada naquele cenário.** A prova exige presença da pergunta canônica e do destino na sequência, e ausência de inversão; o mutante remove a chamada real e exige inversão. O cenário usa disputa canônica fechada e destino encerrado de PK menor. (`app/chamados/tests/test_e1_fila.py:182-191`, `:436-454`.)

O alcance é menor que uma prova funcional completa: a proposta é mockada, `_r` é ignorado, linhas nascidas durante o ato são excluídas e o rastreador não cobre `executemany` nem travas de FK do INSERT. (`app/chamados/tests/test_e1_fila.py:63-71`, `:438-447`; `app/chamados/tests/test_e1_v9.py:46-53`.)

**`A2UniversoMudouSobATravaTest`: morde a retirada da conferência e as alterações simples no tratamento da recusa.** Retirar o `try/except`, trocar `False`, trocar o motivo ou o irmão viola as asserções explícitas. (`app/chamados/tests/test_e1_fila.py:479-497`, `:508-518`.)

Entretanto, a “corrida” é sintética; o teste de `reabrir` substitui tanto a migração quanto `evento`. Não verifica concorrência real, rollback seguido de escrita no chamador, nem INSERT real da trilha. (`app/chamados/tests/test_e1_fila.py:466-477`, `:499-505`.)

Mutantes que as asserções dessas classes deixam escapar **por inspeção**; sobrevivência executada: **[nao verificado]**:

| Mutante de produção | Por que escapa |
|---|---|
| Executar `_trava_do_ato` em `aplicar_declaracao` **somente para `saida_sem_entrada`**, omitindo outros motivos | A proposta testada sempre usa esse motivo; a retirada simples ainda seria detectada. (`app/chamados/tests/test_e1_fila.py:180`, `:438`, `:451-454`; `app/chamados/services/declaracao_texto.py:650`.) |
| Trocar o retorno final de `aplicar_declaracao` por resultado incorreto, preservando as operações | O resultado `_r` não é conferido. (`app/chamados/services/declaracao_texto.py:690-691`; `app/chamados/tests/test_e1_fila.py:444-447`.) |
| Trocar `if _fora[0] or _fora[1]` por **`and`** | O objeto extra introduz simultaneamente colaborador **e** PK novos; não há casos separados para cada diferença. (`app/chamados/services/fio.py:2089-2090`; `app/chamados/tests/test_e1_fila.py:469-476`.) |
| Suprimir `_migrar_sob_trava`, preservando conferência e travas | A classe não exige migração nominal bem-sucedida nem conteúdo de `out`; os testes herdados verificam aquisição/ordem das travas. (`app/chamados/services/fio.py:2094`; `app/chamados/tests/test_e1_fila.py:158-171`, `:479-497`.) |
| Emitir a recusa com **modelo ou objeto_id errados** | O mock confere ação, quantidade, motivo e irmão; não confere modelo/objeto. (`app/chamados/models.py:1421`; `app/chamados/tests/test_e1_fila.py:515-518`.) |
| Ampliar a captura para `RuntimeError`, engolindo falhas adicionais | Só a exceção específica é injetada. (`app/chamados/models.py:1417`; `app/chamados/tests/test_e1_fila.py:501-504`.) |
| Retirar a implementação real de persistência de `evento`, mantendo a chamada | Nesse teste, `evento` é mockado. (`app/chamados/tests/test_e1_fila.py:504`; `app/core/observ.py:133-138`.) |

A conferência também **não compara mudanças de `disputa_id` em perguntas já conhecidas**, embora esse ID componha a trava inicial. Possibilidade de escritor concorrente produtivo realizar essa mudança: **[nao verificado]**. (`app/chamados/services/fio.py:2086-2089`.)

**5. Outra porta capaz de levantar `UniversoMudouSobATrava`**

**Nenhuma outra porta produtiva nominal encontrada fora de `reabrir`.** O `raise` está em `migrar_resolvedoras_vivas`; a chamada produtiva identificada está no ramo `irmao_vivo`, com captura específica. Os caminhos indiretos são os chamadores da pergunta 1. Chamadas dinâmicas em execução: **[nao verificado]**. (`app/chamados/services/fio.py:2091`; `app/chamados/models.py:1411-1417`.)

**6. Classes de reprovação**

O quadro distingue o delta desta volta dos resíduos relacionados na cópia; ausência universal na árvore ou no ambiente não está provada. **[nao verificado]**

| Classe | Constatação com prova |
|---|---|
| **Segundo juiz duplicando juiz registrado** | **Não identificado no delta.** A captura traduz falha técnica em recusa; a migração continua consultando `pergunta_viva`, autoridade registrada. (`app/chamados/models.py:1417-1425`; `app/chamados/services/fio.py:2108-2119`; `app/core/juizes.py:266`.) |
| **Segundo escritor fora da porta** | **Não identificado no delta.** O novo evento passa pelo escritor de auditoria `evento`; não acrescenta escrita direta de estado. (`app/chamados/models.py:1421-1425`; `app/core/observ.py:94-138`.) |
| **Relógio fora da borda** | **Sim, residual na cadeia do juiz:** `_dentro_da_janela_viva` ainda lê `date.today()`. A cura de `expira_em` usa `timezone.now`, mas não elimina essa leitura do juiz. (`app/chamados/juizes.py:569-575`; `app/chamados/veredito_lavrado.py:95-102`.) |
| **Derivação paralela de fonte única** | **Sim, fórmula temporal reproduzida:** `expira_em` reimplementa o intervalo da janela para derivar sua expiração. Divergência observada entre as duas implementações: **[nao verificado]**. (`app/chamados/veredito_lavrado.py:117-143`; `app/chamados/juizes.py:575-601`.) |
| **Selo/RED sem mordida** | **Não para os alvos estreitos declarados; insuficiente para garantias amplas.** Há mutantes explícitos de remoção, mas integração transacional/persistência é substituída por mocks e sobrevivem os mutantes listados acima. (`app/chamados/tests/test_e1_fila.py:449-454`, `:489-518`.) |
| **Ordem de trava** | **Ordem local implementada; ordem de toda chamada composta [nao verificado].** Há chamadores que já seguram C ou C/D/Ch/P antes de a migração adquirir seu conjunto, inclusive colaboradores adicionais. A prova nova não percorre essas composições. (`app/chamados/models.py:905`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:153-156`; `app/chamados/services/disputa_emissao.py:1721`, `:1787`, `:1812`; `app/chamados/services/fio.py:2084-2087`; `app/chamados/tests/test_e1_fila.py:499-505`.) |
| **Pouso antes do pré-requisito** | **Pouso/deploy [nao verificado].** Na ordem do ato administrativo, há efeitos anteriores à confirmação de reabertura do chamado, e o fluxo continua após recusa. (`app/chamados/services/acoes_disputa.py:256-296`.) |
| **Campo com dois significados** | **Sim nos contadores:** `pais_reabertos` pode contar tentativa recusada; `CREATE` pode incluir emissão cujo chamado permaneceu encerrado. **Não demonstrado em `depois['motivo']`**, que tem um único valor emitido identificado. (`app/chamados/services/adesao.py:92`; `app/ponto/management/commands/reconciliar_grade.py:254-259`, `:529-531`; `app/chamados/models.py:1424`.) |

## 08/11 23:34 · sessao 7f2da60deb0c · FIO-E0-V4-E1-FILA · censo · origem=Code

Nos anexos, conto as linhas desde a primeira linha de cada bloco, incluindo linhas vazias e excluindo o cabeçalho `== anexo: ... ==`.

| Achado | Classe | Trecho literal da pergunta | Trecho literal da resposta | Linha de triagem |
|---|---|---|---|---|
| **1 — Falta de RED próprio para `aplicar_declaracao`** | **(a), na leitura de cobertura:** o tema dos testes estava perguntado; a resposta reconheceu a porta, mas não apontou sua falta de teste. (`codex_e1fila.q:7`; `codex_e1fila.r:72`) | “Os testes novos (test_e1_fila.py) mordem o que dizem morder?” (`codex_e1fila.q:7`). Na volta 2: “selo/RED sem mordida” (`codex_e1fila_v2.q:9`). | Sobre `aplicar_declaracao`: “Mesmo prefixo no atomic da emissão/resposta.” (`codex_e1fila.r:72`). A avaliação de A3 aponta outra lacuna: “Não exige a Disputa fechada na sequência.” (`codex_e1fila.r:31`). **Esses trechos não denunciam a ausência de caso para a porta irmã.** | Não há descarte específico dessa ausência. O registro próximo diz: “o ramo do rechaveamento por dia diferente continua sem caso proprio (fila)” — outro problema, não a cobertura de `aplicar_declaracao`. (`triagem_e1_fila.txt:29`, `triagem_e1_fila.txt:30`) | [nao verificado]
| **2 — `UniversoMudouSobATrava`: sem teste, observabilidade e tratamento produtivo** | **(b), pelo critério solicitado de algum trecho tocar o achado:** Codex viu a falta de teste e a propagação da exceção; a triagem colocou a lacuna de teste em FILA. Isso não demonstra que tenha identificado todos os componentes do bloqueante. (`codex_e1fila.r:30`; `codex_e1fila.r:90`; `triagem_e1_fila.txt:27`) | “Os testes novos [...] mordem o que dizem morder?” (`codex_e1fila.q:7`); “ou regra de negocio nova?” (`codex_e1fila.q:8`). Na volta 2: “Os descartes e as linhas de FILA da triagem: algum deles e procedente para ESTA obra” (`codex_e1fila_v2.q:7`). | A2: “Não testa crescimento do universo entre leitura e trava, nem entrada por chamador já travado.” (`codex_e1fila.r:30`). Item 8: “crescimento detectado aborta o atomic por exceção, propagada ao chamador, sem retry nessa porta.” (`codex_e1fila.r:90`). | “A2 nao testa crescimento do universo nem chamador ja travado -> FILA (UniversoMudouSobATrava sem teste de corrida; o chamador travado e o item 5).” **Não apresenta justificativa própria para adiar o teste de crescimento.** (`triagem_e1_fila.txt:27`, `triagem_e1_fila.txt:28`) | [nao verificado]

**Limites da classificação:**

- **Achado 1:** adoto “tema presente” como cobertura dos testes nas portas tocadas. Mencionar a trava de `aplicar_declaracao` não equivale a detectar seu RED ausente; também não autoriza atribuir à triagem um descarte que ela não registrou. A afirmação experimental “apagar a chamada sobrevive à suíte inteira” é **(c)** quanto à confirmação por execução nesta auditoria: **[nao verificado]**. (`codex_e1fila.q:5`; `codex_e1fila.q:7`; `codex_e1fila.r:72`; `triagem_e1_fila.txt:29`) [nao verificado]
- **Achado 2:** o **(b) é parcial**. A resposta registra propagação, mas não denuncia literalmente “sem trilha nem contador”; tampouco há descarte específico desses componentes. Os motivos “sem ocorrência verificada” e mudança de desenho pertencem ao problema do **chamador já travado**, não constituem justificativa expressa para deixar a exceção crua ou sem observabilidade. (`codex_e1fila.r:90`; `triagem_e1_fila.txt:27`; `triagem_e1_fila.txt:59`)
- A cópia atual já contém alterações identificadas como curas posteriores desses bloqueantes; elas não provam o que foi detectado nas duas verificações anteriores. ([test_e1_fila.py](/home/ronald/codex_ws/602a7d739b34f2a8524a4210bb4423a807f40331/app/chamados/tests/test_e1_fila.py:430); [models.py](/home/ronald/codex_ws/602a7d739b34f2a8524a4210bb4423a807f40331/app/chamados/models.py:1418))

## 08/11 23:33 · sessao 0f34790ec36d · - · - · origem=Ronald (tmux)

Confirmado por inspeção: **mover todo o bloco 1c para depois da seção 2 sobrevive aos 11 casos**; execução [nao verificado]. (`bin/deploy.sh:298-301`, `:358`; `bin/tests/test_deploy_portoes_antes_do_migrate.sh:110-152`)

- `ORDEM_OK` exige somente auth/sombra/casca **antes do apply**; não compara apply com HUP/restart. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:95-104`)
- O caso 3 aceita migrate=1 e HUP=1, independentemente da ordem entre eles. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:119`)
- No caso 11, `APLICAR_MIGRATE=1` já dispensa a recusa da seção 2; o restart falso acontece primeiro, depois o migrate cria a marca. A asserção aceita as mesmas contagens e rc=0. (`bin/deploy.sh:223`, `:325-326`; `bin/tests/test_deploy_portoes_antes_do_migrate.sh:68`, `:76`, `:150-152`)

Assim, o mutante publica antes do pré-requisito de schema e transforma “cron nascido durante migrate antes do restart” em “marca criada depois do restart”, sem ser denunciado pelas asserções. (`bin/deploy.sh:24-28`; `bin/tests/test_deploy_portoes_antes_do_migrate.sh:95-104`, `:151`)

## 08/11 23:32 · sessao 3dbe97426882 · - · - · origem=Ronald (tmux)

Censo estático: **10 chamadas produtivas diretas** a `ChamadoColaborador.reabrir`; não inclui `ponto.services.fechamento.reabrir`, que opera `FechamentoMensal` (`app/ponto/services/fechamento.py:1349`).

| Chamador | Atomic externo à migração | O que faz com False |
|---|---|---|
| `_renascer_por_premissa_viva` | Via `abrir`, sim: atomic `app/chamados/models.py:902`, chamada transitiva `:922`. Via resposta tardia, nenhum atomic local: `app/chamados/services/resposta_tardia.py:31`, `:45`; view `app/chamados/views.py:1039`, `:1053`. | Propaga False em `app/chamados/models.py:1019`. `abrir` descarta esse bool e retorna `(existente, False)`, cujo segundo elemento significa “criado”, não “reaberto” (`:922–924`). Resposta tardia faz nota/log/Pauta e devolve estado `tardia`, sem anunciar reabertura (`app/chamados/services/resposta_tardia.py:45`, `:52`, `:54`, `:58`, `:67`). |
| Desarquivar | Sem atomic local, view apenas `login_required` (`app/chamados/views_arquivar.py:51–70`). | Devolve JSON `ok=False`, mas texto atribui **qualquer recusa** a irmão vivo (`:70–72`); para universo mudou, razão exibida é errada. |
| Sincronizar adesão | Sem atomic local; comando chamador também não envolve atomic (`app/chamados/services/adesao.py:44`, `app/escala/management/commands/apurar_furos_diarios.py:6–21`). | **Conta como reaberto mesmo False**: `pai.reabrir(); r['pais_reabertos'] += 1` (`app/chamados/services/adesao.py:92`), depois imprime contador (`app/escala/management/commands/apurar_furos_diarios.py:21`). |
| Reabrir disputa admin | Sem atomic no serviço (`app/chamados/services/acoes_disputa.py:232–296`), no dispatcher (`app/chamados/services/fio.py:485`, `:583`) ou nas duas views de entrada (`app/chamados/views.py:658–674`, `:868–954`). | **Ignora False** (`app/chamados/services/acoes_disputa.py:267`); anexa ocorrência, envia pushes e responde sucesso (`:271`, `:275`, `:287`, `:296`). A mensagem fala da disputa, efetivamente reaberta, mas o chamado pode continuar encerrado. |
| Reabrir competência | Sim, atomic por chamado (`app/chamados/services/competencia_trancada.py:264`). | Só nota/log/incremento se True (`:265–272`); False pula efeitos, não levanta erro. |
| Resolver perguntas por chamado resolvido | Sim: wrapper usa atomic existente ou abre um (`app/chamados/services/disputa_emissao.py:1720–1722`). Ramo da tranca sai antes de chegar à chamada (`:1715–1716`, `:1742–1743`). | Só anexa nota de reabertura se True (`:1812–1814`). |
| Comando reabrir órfãs | Sim, atomic por pergunta (`app/chamados/management/commands/reabrir_orfas_enterradas.py:146`). | Ignora bool, mas relê estado, registra `pai_falhou` ou incrementa reabertos (`:172–177`), imprime falhas (`:185–189`). **Não desfaz** pergunta/disputa já reabertas (`:157–167`). |
| Comando ausência aberta | Sem atomic local (`app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:22–55`). | Só nota e incremento se True (`:49–54`). |
| Reconciliar grade | Sem atomic no caminho `_emitir_furo` (`app/ponto/management/commands/reconciliar_grade.py:488–531`); atomic `:472` é de outro método e termina antes. | **Ignora False**, retorna None (`:530–531`). Chamador interpreta None como ausência de veto e incrementa `plan['CREATE']` / `creates` (`:254–259`). |
| Detectar ausências, limbo | Sem atomic no caminho do comando (`app/ponto/management/commands/detectar_ausencias.py:61`, `:271–303`). | Ignora False (`:301`), sem contador específico nem aviso da recusa nesse chamador (`:300–304`). |

**Fronteira transacional:** `migrar_resolvedoras_vivas` abre `transaction.atomic()` com configuração padrão (`app/chamados/services/fio.py:2074`); levanta em `:2091` **antes** de executar `_migrar_sob_trava` em `:2094`. Antes da recusa há leituras e `select_for_update`; a função de trava não grava dados de negócio (`app/chamados/models.py:2674`, `:2683`, `:2696`, `:2700`). `reabrir` captura **depois da saída** desse atomic (`app/chamados/models.py:1415–1417`). Sob atomic externo saudável, a conclusão contratual é rollback ao savepoint e conexão utilizável; o próprio repositório documenta esse mecanismo em `app/core/observ.py:124–133`. Sem atomic externo, esse bloco abre uma transação e a encerra com rollback; a ocorrência da recusa entra depois em outro atomic próprio (`app/chamados/models.py:1421`, `app/core/observ.py:133`). Validação efetiva da conexão/Django em execução: **[nao verificado]**.

Configurações da cópia não ativam `ATOMIC_REQUESTS` nem desativam autocommit nos dicionários de produção/on-prem e SaaS (`app/config/settings/prod.py:9–18`, `app/config/settings/saas.py:83–92`); configuração efetiva carregada em produção: **[nao verificado]**.

**Há escrita anterior que fica:** no fluxo admin, `d_fec.reabrir_admin` salva a disputa (`app/chamados/services/acoes_disputa.py:256`, `app/chamados/models.py:2169`), abre Pergunta de conversa (`app/chamados/services/acoes_disputa.py:260`) e só depois tenta reabrir o chamado (`:267`); a recusa não desfaz essas escritas, nem bloqueia os efeitos posteriores (`:271–296`). No modal ainda há marcação de notas lidas antes do dispatcher (`app/chamados/views.py:879–880`, `:954`). Isso é parcialidade do **ato maior**, não migração pela metade (`app/chamados/services/fio.py:2091–2094`).

Mesmo com savepoint externo há parcialidade de negócio intencional/concreta: órfãs já alteraram Pergunta/Disputa quando pai recusa (`app/chamados/management/commands/reabrir_orfas_enterradas.py:157–175`); competência grava `PeriodoFechado` antes de tentar devolver chamados, em gancho best-effort, e depois reabre fechamentos mensais (`app/ponto/services/fechamento.py:1435–1452`).

A trilha de `universo_mudou_sob_trava` é tentada pela própria porta (`app/chamados/models.py:1421–1425`), inclusive nos chamadores que ignoram False. Persistência não é garantida: `evento` é best-effort e retorna None se falhar (`app/core/observ.py:94–105`, `:133–139`); sucesso real do INSERT sem banco/log: **[nao verificado]**.

**Lacuna dos testes para esta pergunta:** o teste de `reabrir` substitui migração por `side_effect` e substitui `evento` por mock (`app/chamados/tests/test_e1_fila.py:499–505`); verifica False e argumentos (`:514–518`), mas não executa a integração rollback/savepoint → INSERT da trilha → escrita posterior do chamador. Essa garantia integrada permanece **[nao verificado]**.

**Possível ordem de trava transitiva a investigar:** atomic de `abrir` já segura um Colaborador (`app/chamados/models.py:905`), órfãs já seguram C/D/Ch/P (`app/chamados/management/commands/reabrir_orfas_enterradas.py:153–156`), resolver já trava universo e pode ter escrito Pergunta antes de reabrir pai (`app/chamados/services/disputa_emissao.py:1721`, `:1787`, `:1812`). A migração então pode adquirir conjunto adicional de Colaboradores (`app/chamados/services/fio.py:2084–2087`), inclusive de pergunta alheia, caso expressamente admitido em comentário (`:2080–2081`). A ordem local ordenada não prova a ordem da transação inteira; existência/alcançabilidade de dados que realizem esse caminho: **[nao verificado]**.

## 08/11 23:31 · sessao ec542bc3f363 · - · - · origem=Ronald (tmux)

No escopo do diff, com distinção entre defeito novo e limite já existente:

| Classe | Veredito e prova |
|---|---|
| Segundo juiz duplicando juiz registrado | **Não.** O novo teste substitui auth/sombra/casca por respostas falsas, sem reimplementar suas regras; deploy continua chamando `janela_auth.sh`. O juiz de horário permanece `proibido()`. `bin/tests/test_deploy_portoes_antes_do_migrate.sh:54-56`; `bin/deploy.sh:145`; `bin/janela_auth.sh:53-61`. |
| Segundo escritor fora da porta | **Não introduzido.** `APLICAR_MIGRATE=0` inicializa estado local e `=1` decide apply; não há publicador novo: seguem um restart e um HUP. A exclusão de marca morta por deploy é escritor de limpeza **já existente**, além do envelope produtor/removedor. `bin/deploy.sh:61,108-109,223,326,343`; `bin/cron_run.sh:9-10`. |
| Relógio fora da borda | **Não introduzido.** O diff não acrescenta leitura de relógio; auth concentra `date` nas costuras `_dow/_hhmm`, com substituição explícita de teste. O caso 11 usa `/proc/1`, não relógio. `bin/janela_auth.sh:45-56`; `bin/tests/test_deploy_portoes_antes_do_migrate.sh:68`; `bin/deploy.sh:109`. |
| Derivação paralela de fonte única | **Não no patch.** A decisão de aplicar continua derivada do `--check` e modo, em um ramo; a seção 2 consome a mesma variável. Não nasce regra auth paralela. `bin/deploy.sh:184-188,204-224,298-300,325`; `bin/deploy.sh:145`. |
| Selo/RED sem mordida | **Não é inteiramente sem mordida**, mas **sim, há lacunas relevantes**. Caso 11 mata a recusa incondicional pós-apply, porém só cria `cron_falso.1`; o Docker falso não mata nem reexecuta cron. `bin/tests/test_deploy_portoes_antes_do_migrate.sh:68,76,150-152`. RED efetivamente executado nesta volta: **[nao verificado]**. |
| Ordem de trava | **Sim.** Checar cron antes de migrate e depois dispensar a checagem não impede admissão concorrente: envelope marca e executa sem coordenação com deploy. `bin/deploy.sh:299-300,325-326`; `bin/cron_run.sh:9-11`. Ausência de cron no instante observado não reserva o intervalo. |
| Pouso antes do pré-requisito | **Sim, pela regra ainda escrita “restart recusa com cron vivo”.** Depois de migrate, o novo ramo permite restart apesar da marca recém-criada, contrariando a promessa vigente. `CLAUDE.md:90`; `bin/deploy.sh:325-326`; `bin/tests/test_deploy_portoes_antes_do_migrate.sh:150-152`. **Outro resto do contrato literal “todo portão antes de migrate”:** selo de reciclagem continua posterior a migrate/reload e pode reprovar por configuração/crontab. `bin/deploy.sh:300,326,343,409`; `bin/selo_reciclagem.sh:46-78`. |
| Campo com dois significados | **Não demonstrado em `APLICAR_MIGRATE`, apesar da prosa incorreta.** Na escrita `=1` significa “apply planejado”; no leitor `:325`, o controle de fluxo permite inferir “apply retornou 0”, pois falha em `:300` termina por `falhou`, definido em `:92`. Portanto o novo leitor tem pré-condição válida; não lê intenção como sucesso antes de executar. `bin/deploy.sh:92,223,298-300,325`. O comentário “UM leitor” é falso: há dois, `:298` e `:325`, além de dois pontos de atribuição, `:61` e `:223`. |

Outras observações úteis para consolidar:

- O selo estrutural antigo só busca o texto `recusa_restart_com_cron` antes do restart; a linha nova mantém esse texto mesmo quando o curto-circuito dispensa sua execução. Assim, esse selo aceita a perda da garantia universal. `bin/contrato_deploy.sh:45-47`; `bin/deploy.sh:325`.
- O teste de reload gracioso verifica cron existente **no começo**; não verifica o nascido durante migrate. `bin/tests/test_deploy_reload_gracioso.sh:125-131`.
- “Todo cron é idempotente” é requisito escrito, não prova de reexecução: o envelope executa uma vez, mede RC, carimba somente 0/2 e alerta falha; não contém repetição automática do comando interrompido. `CLAUDE.md:246`; `bin/cron_run.sh:11-13,22-25,29-38`.
- Semântica dos campos novos do teste: `FAKE_CRON_NO_MIGRATE` é presença habilitadora, `FAKE_HERDA_APLICAR` injeta ambiente, `N_RST` conta linhas de chamadas restart. Seus escritores e leitores concordam nos usos fornecidos, mas `N_RST=1` não prova três cascas reiniciadas nem cron preservado. `bin/tests/test_deploy_portoes_antes_do_migrate.sh:68,90,101,146,150-151`.
- A marca de cron registra PID do **envelope do host**, não identidade do processo `docker exec`; deploy usa apenas existência de `/proc/<pid>`, sem conferir identidade ou conteúdo do arquivo. Isso admite falso “vivo” por PID reutilizado e não é novidade do diff. `bin/cron_run.sh:9-11`; `bin/deploy.sh:108-109`.

## 08/11 23:31 · sessao 73a57bb44d60 · - · - · origem=Ronald (tmux)

- **`APLICAR_MIGRATE`:** escritores executáveis em `bin/deploy.sh:61` (`0`) e `bin/deploy.sh:223` (`1`); leitores em `bin/deploy.sh:298` e `bin/deploy.sh:325`. O comentário “UM leitor” ficou falso com o segundo leitor; há também duas atribuições, embora ambas pertençam ao mesmo script (`bin/deploy.sh:59`). Fora dele, o selo remove a variável herdada e opcionalmente injeta `APLICAR_MIGRATE=1` (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:90`); o caso 10 ativa a injeção e exige zero migrations (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:146`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:147`). O valor herdado é sobrescrito antes dos leitores (`bin/deploy.sh:61`, `bin/deploy.sh:298`, `bin/deploy.sh:325`).

- **`RESTART`:** escritores em `bin/deploy.sh:70` (`0`) e `bin/deploy.sh:73` (`1`, somente pelo argumento `--restart`); leitores em `bin/deploy.sh:115`, `bin/deploy.sh:200`, `bin/deploy.sh:299`, `bin/deploy.sh:322` e `bin/deploy.sh:428`. O valor herdado é sobrescrito (`bin/deploy.sh:70`). Fora dele, `bin/contrato_deploy.sh:41` lê o **texto** do `if` para conferir a posição do restart; não consome o valor da variável.

- **Marcas de produção:** `bin/cron_run.sh:9` cria o diretório e deriva `<nome>_<empresa>.<PID do envelope>`; `bin/cron_run.sh:10` escreve a marca e instala sua remoção no `EXIT`. `bin/deploy.sh:108` enumera as marcas; `bin/deploy.sh:109` lê existência/PID e também **apaga** a marca cujo PID não existe. A pergunta é consumida em `bin/deploy.sh:111` e pode recusar em `bin/deploy.sh:112`; chamadas em `bin/deploy.sh:117`, `bin/deploy.sh:299` e `bin/deploy.sh:325`.

- **Marcas fora de produção:** o selo gracioso escreve uma marca com PID de processo real falso (`bin/tests/test_deploy_reload_gracioso.sh:89`, `bin/tests/test_deploy_reload_gracioso.sh:91`); o selo desta obra escreve `cron_falso.1` durante o migrate falso (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:68`). Os diretórios falsos são criados em `bin/tests/test_deploy_reload_gracioso.sh:34` e `bin/tests/test_deploy_portoes_antes_do_migrate.sh:45`. `bin/contrato_deploy.sh:81` e `bin/contrato_deploy.sh:82` procuram a presença textual de `crons_em_curso`; não leem marcas.

- **Limites semânticos das marcas:** o escritor registra o PID do envelope no host e um timestamp (`bin/cron_run.sh:7`, `bin/cron_run.sh:9`, `bin/cron_run.sh:10`); o leitor ignora o conteúdo e confere somente `/proc/<PID>` (`bin/deploy.sh:109`). Isso não identifica a execução corrente dentro de uma casca nem protege contra reutilização de PID (`bin/cron_run.sh:9`, `bin/deploy.sh:109`). O escritor usa caminho absoluto (`bin/cron_run.sh:9`), o leitor usa `RAIZ` calculada da cópia (`bin/deploy.sh:53`, `bin/deploy.sh:108`); equivalência desses caminhos em produção: **[nao verificado]**.

- **Corrida do restart:** não existe exclusão comum entre o nascimento da marca (`bin/cron_run.sh:9`, `bin/cron_run.sh:10`) e a sequência observar → migrar → reiniciar (`bin/deploy.sh:299`, `bin/deploy.sh:300`, `bin/deploy.sh:325`, `bin/deploy.sh:326`). Com `APLICAR_MIGRATE=1`, um cron novo deixa de recusar na seção 2 (`bin/deploy.sh:325`). Se estiver executando dentro das cascas reiniciadas, o risco de morte é o que o próprio deploy documenta (`bin/deploy.sh:312`, `bin/deploy.sh:314`); ocorrência e exit concretos em produção: **[nao verificado]**.

- **“Morto e refeito” não está demonstrado:** a frase está em `bin/deploy.sh:296`; idempotência é requisito textual em `CLAUDE.md:246`. O envelope executa uma vez (`bin/cron_run.sh:11`), não carimba término em rc diferente de 0/2 (`bin/cron_run.sh:22`), tenta registrar alarme (`bin/cron_run.sh:32`, `bin/cron_run.sh:33`) e sai (`bin/cron_run.sh:38`). Não implementa retry. A corrente segue adiante registrando erro (`bin/encadeado.sh:64`, `bin/encadeado.sh:66`); seus dependentes podem esperar o carimbo até alarmar (`bin/encadeado.sh:89`, `bin/encadeado.sh:92`, `bin/encadeado.sh:94`). Reexecução efetiva e idempotência de **todos** os comandos: **[nao verificado]**.

- **Outra ordenação concebível:** impedir admissões novas de cron sob uma trava também obedecida pelo envelope, drenar os existentes e manter essa exclusão durante migrate e publicação eliminaria esta corrida específica; o código atual não tem esse protocolo (`bin/cron_run.sh:9`, `bin/cron_run.sh:11`, `bin/deploy.sh:299`, `bin/deploy.sh:326`). Para garantir ausência absoluta de código velho atendendo sobre schema novo, seria preciso ainda tratar o atendimento durante o intervalo migrate → publicação; viabilidade concreta de pausa ou compatibilidade de migrations nesta aplicação: **[nao verificado]**.

## 08/11 23:31 · sessao 73e022226ffe · - · - · origem=Ronald (tmux)

**Q3 — escritores e leitores**

- Escritores produtivos do evento: `_trilha_do_veto`, com `via='renascer'` ou `'reabrir'` e campos `veto`, `dia`, `modulo_origem`, `colaborador` (`app/chamados/models.py:1166`, `:1184`); `reabrir` recusado por admin sem fato novo (`:1396`), premissa regularizada (`:1402`) e universo alterado (`:1421`). Só o último escreve `depois['motivo']='universo_mudou_sob_trava'` (`:1424`); os outros dois usam `status_local`/`via_encerramento` (`:1398`, `:1404`).
- Leitor específico identificado: deduplicação de `_trilha_do_veto`, que filtra ação/modelo/objeto e **também** `modulo_origem`, `dia`, `veto`, `via` (`app/chamados/models.py:1172`). O novo evento não satisfaz essa chave; portanto não impede a trilha do veto (`:1172`, `:1424`).
- Não identifiquei leitor produtivo de `depois['motivo']` para essa ação; único leitor explícito desse valor encontrado está no teste (`app/chamados/tests/test_e1_fila.py:517`). Ausência absoluta de leitores dinâmicos: [nao verificado].
- A auditoria genérica consulta todas as ações, aceita filtro literal `acao` e conta registros, sem classificar motivos (`app/core/views.py:705`, `:709`, `:727`). O template exibe ação/descrição, sem `valor_depois` (`app/templates/core/log_auditoria.html:55`, `:68`, `:73`). Limitação preexistente: dropdown usa `LogAuditoria.ACOES`, que contém só sete ações genéricas e não oferece `reabertura_recusada` (`app/core/views.py:738`; `app/core/models.py:286`; `app/templates/core/log_auditoria.html:23`); GET com `acao=reabertura_recusada` continua aceito (`app/core/views.py:709`).
- `vigia_loop.escritas_por_cron` inclui a nova recusa quando o ator corresponde ao cron: conta todas as linhas por ator/modelo/objeto, sem filtro de ação ou motivo (`app/chamados/services/vigia_loop.py:86`). Seu rótulo já declara “escritas com trilha LogAuditoria”; portanto não demonstrou pressuposto incompatível de motivos (`:68`, `:96`).
- Os vigias de trocas de estado e trilha versus banco filtram exclusivamente `estado_chamado`, excluindo recusas (`app/chamados/services/vigia_loop.py:29`, `:114`). O histórico do colaborador também só inclui essa ação de estado (`app/chamados/services/historico.py:44`, `:86`).
- Há várias causas sob o mesmo evento, mas o sentido comum permanece “reabertura recusada”; **não há contrato homogêneo de `motivo` obrigatório** nos emissores (`app/chamados/models.py:1187`, `:1398`, `:1404`, `:1424`). Não encontrei prova de contador lendo conjunto antigo de motivos; comportamento em produção: [nao verificado].
- A tentativa de trilha é best-effort: erro na gravação retorna `None`, com log/contador de engolidos, em vez de assegurar linha persistida (`app/core/observ.py:105`, `:133`, `:139`, `:73`). Logo “recusa falante” prova chamada do emissor; persistência real: [nao verificado].

**Q5 — outra porta capaz de levantar a exceção**

- Único `raise UniversoMudouSobATrava` identificado no código produtivo é a conferência de `migrar_resolvedoras_vivas` (`app/chamados/services/fio.py:2088`, `:2091`).
- Único chamador produtivo nominal identificado de `migrar_resolvedoras_vivas` é o ramo `irmao_vivo` de `ChamadoColaborador.reabrir`, que captura essa exceção (`app/chamados/models.py:1411`, `:1415`, `:1417`).
- Não há outra porta direta identificada fora de `reabrir`; rotas por alias dinâmico/reflexão em execução: [nao verificado]. Chamadores indiretos que usam `reabrir` pertencem ao inventário da Q1 (`app/chamados/models.py:1415`).

**Observação útil à conferência de travas:** o conjunto inicial inclui `disputa_id` nas travas, mas a conferência posterior só compara colaboradores e PKs das perguntas. Mudança de `disputa_id` de uma pergunta já conhecida não aciona esse `raise`; se uma escrita concorrente produtiva pode realizar isso nas condições do ato: [nao verificado] (`app/chamados/services/fio.py:2086`, `:2089`).

## 08/11 23:30 · sessao 0f34790ec36d · - · - · origem=Ronald (tmux)

Mapa estático, sem execução: o selo cria e remove arquivos, o que conflita com só leitura (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:44`, `:107`); resultados reais de RED/VERDE: [nao verificado].

| Caso | Mutante que a asserção mata |
|---|---|
| 1 | Aplicar migrate antes da auth recusar, ou ignorar a recusa no normal: exige rc≠0, migrate=0 e HUP=0 (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:110-112`; porta real `bin/deploy.sh:145-147`). |
| 2 | Aplicar migrate antes da sombra recusar, ou ignorar a recusa: mesmas três exigências (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:114-116`; `bin/deploy.sh:162-165`). |
| 3 | Eliminar migrate; aplicá-lo duas vezes; colocá-lo antes de auth/sombra/casca; chamar sombra com operação diferente de `--conferir`; eliminar HUP: exige migrate=1, HUP=1 e primeira passagem dos três portões anterior ao primeiro apply (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:95-104`, `:118-120`). |
| 4 | Isentar o agendado da chamada à auth: exige N_JAN≥1. **Ignorar o retorno**, mantendo a chamada, pode sobreviver a este caso porque há pendência core e a guarda de agendado ainda recusa (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:122-124`; `bin/deploy.sh:211-214`). |
| 5 | Consultar auth/sombra em `--conferir`, migrar ou enviar HUP nesse modo: exige rc=0 e todas essas contagens zeradas (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:126-128`). |
| 6 | Adiar a prova de casca até depois do apply, ou ignorar sua recusa: exige falha sem migrate/HUP (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:130-132`; `bin/deploy.sh:286-287`). |
| 7 | Fazer `--sem-sombra` também dispensar auth ou migrar antes dela: exige falha sem migrate/HUP (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:134-136`). |
| 8 | Aplicar migrate incondicionalmente, inclusive sem pendência; eliminar o HUP no caminho sem pendência (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:138-140`). |
| 9 | Chamar auth no agendado e ignorar sua recusa: agora não há pendência para esconder a falta de mordida (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:142-144`). |
| 10 | Permitir que `APLICAR_MIGRATE=1` herdado determine apply em `--sem-migrate`; a fixture injeta explicitamente esse valor (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:90`, `:146-148`; inicialização real `bin/deploy.sh:61`). |
| 11 | Restaurar a recusa incondicional por cron na seção 2 depois do apply, como na volta anterior: exige rc=0, migrate=1 e restart=1 (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:150-152`; decisão atual `bin/deploy.sh:325`). |

Mutantes plausíveis sobreviventes, por inspeção das fixtures e asserções; execução dos mutantes: [nao verificado].

- **Remover as recusas por cron antes do migrate**, nas linhas 117/299: nenhum caso instala cron previamente; o único cron falso nasce durante o apply (`bin/deploy.sh:117`, `:299`; `bin/tests/test_deploy_portoes_antes_do_migrate.sh:68`, `:150`).
- **Remover a guarda de migration pendente da mensageria**: o falso sempre responde sucesso a esse check (`bin/deploy.sh:200-202`; `bin/tests/test_deploy_portoes_antes_do_migrate.sh:69`).
- **Dispensar sombra/auth no `--reload-copia`**: nenhum dos 11 cenários usa esse modo (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:110-152`; condições reais `bin/deploy.sh:126`, `:151`).
- **Adicionar uma segunda consulta a auth/sombra/casca depois do migrate**: os retornos dos falsos não mudam durante uma corrida; casos vermelhos já encerraram antes, casos verdes continuam verdes. Portanto não detectam um portão que muda de resposta com o tempo (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:54-56`, `:90`).
- **Reiniciar somente `saas_core`**, omitindo as outras cascas: N_RST conta chamadas, não os alvos; restart falso sempre retorna 0, e readiness/rotas também ficam verdes (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:70-76`, `:101`, `:151`; comando real `bin/deploy.sh:326`).
- **Eliminar provas de troca dos workers, readiness ou rotas**: nenhuma asserção exige essas chamadas; as fixtures já produzem troca de PID e STATUS 200 no caminho verde (`bin/deploy.sh:344-356`, `:368-370`, `:399-402`; `bin/tests/test_deploy_portoes_antes_do_migrate.sh:70`, `:73-75`, `:110-152`).
- **Ignorar falha do apply ou do restart**: o docker falso não oferece retorno de falha nessas operações, logo os respectivos `|| falhou` não são exercitados (`bin/deploy.sh:300`, `:326`; `bin/tests/test_deploy_portoes_antes_do_migrate.sh:68`, `:76`).
- **Eliminar ou deslocar selo_reciclagem**: a fixture o substitui por `exit 0`; este selo não verifica a posição nem a mordida do selo real (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:51-53`; `bin/deploy.sh:409`).

O caso 11 **não prova cron morto nem cron refeito**: apenas cria `cron_falso.1`, conta uma chamada de restart e exige rc=0; o falso não executa nem reexecuta cron (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:68`, `:76`, `:101`, `:150-152`).

## 08/11 23:18 · sessao 6fa4aad98c43 · deployportao · verificador · origem=Code

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

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

_2932 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
