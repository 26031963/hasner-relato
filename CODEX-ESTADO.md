# CODEX-ESTADO

> Ultima resposta processada: **04/11/2026 12:22** · publicado em **04/11/2026 12:22** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 04/11 12:22 · sessao c2861ff43f2f · - · - · origem=Ronald (tmux)

Mapa de escritores/leitores e falhas:

- **RESTART**: valor local inicializado em `0`, alterado para `1` apenas por `MODO=--restart`; motivo obrigatório (`bin/deploy.sh:67-72`). Leitores: primeira recusa (`bin/deploy.sh:112-114`), exceção da guarda mensageria (`bin/deploy.sh:147`), escolha restart/HUP (`bin/deploy.sh:295-315`), frase final (`bin/deploy.sh:400-403`). O contrato examina textualmente esse ramo (`bin/contrato_deploy.sh:41-49`).

- **PEND_MSG**: escrito pelo resultado de `docker exec mensageria … migrate --check`, `0` em sucesso e `1` em qualquer falha (`bin/deploy.sh:141-145`); lido na saída e na guarda (`bin/deploy.sh:146-149`). O fake do selo fornece o rc por `FAKE_PEND_MSG` (`bin/tests/test_deploy_reload_gracioso.sh:55`).

- **logs/crons_em_curso**: envelope cria diretório/marca `${N}_${E}.$$`, escreve horário e remove em `trap EXIT` (`bin/cron_run.sh:9-10`). Deploy percorre marcas e consulta `/proc/<pid>`; **também escreve**, removendo marcas consideradas mortas (`bin/deploy.sh:105-106`). Leitura para recusa usa `em_curso` (`bin/deploy.sh:107-110`). O selo cria marca falsa de PID vivo (`bin/tests/test_deploy_reload_gracioso.sh:85`).

- **logs/vigia_logins.lock**: `vigia_logins` abre fd 7 e tenta flock exclusivo não bloqueante (`bin/vigia_logins.sh:71-75`); o selo mantém a mesma trava ocupada no fixture (`bin/tests/test_vigia_logins_sem_sobreposicao.sh:58`). A trava cobre as leituras decisórias e o revert/deploy (`bin/vigia_logins.sh:76-87`).

- **“deploy: OK”**: escritores preservam prefixo nos dois modos (`bin/deploy.sh:401-403`); leitores usam apenas `^deploy: OK`, portanto compatíveis com nova redação (`bin/molde_fatia/cadeia.sh:89-90`, `bin/molde_autocura/cadeia.sh:86-87`). Ambos têm `set -e`, além da conferência textual (`bin/molde_fatia/cadeia.sh:4`, `bin/molde_autocura/cadeia.sh:3`).

- **rc em `_subir`**: `_rodar` retorna o `subprocess.run(...).returncode` real (`app/core/integrador_lote.py:242-248`); novo ramo lê o rc, grava `veredito`/`deploy_rc` e retorna antes de FIM/NO_AR (`app/core/integrador_lote.py:753-766`). `_fechar` persiste o registro em LOTES e no log (`app/core/integrador_lote.py:825-829`). Entretanto, `main rodar` sempre retorna `0`, portanto o shell ainda recebe sucesso mesmo com veredito `deploy falhou` (`app/core/integrador_lote.py:856-858`, `bin/integrador.sh:52`).

Falhas a destacar:

1. **Consumidor não migrado: reload da cópia do integrador ignora rc** e entra na suite imediatamente, mesmo quando a nova guarda recusou reload (`app/core/integrador_lote.py:638-640`).

2. **Consumidor não migrado: `esteira_vigia.auto_revert` ignora rc dos dois deploys**, inicial e revert do revert (`app/core/esteira_vigia.py:1267-1268`, `app/core/esteira_vigia.py:1275`). Pode concluir “a tela volta” porque testes passaram sem provar publicação (`app/core/esteira_vigia.py:1269-1273`, `app/core/esteira_vigia.py:1287-1291`).

3. **Alarme novo afirma estado de prod que o rc não prova.** Para qualquer rc não zero informa “PROD SEGUE no codigo de antes do lote” (`app/core/integrador_lote.py:754-760`), porém falhas podem acontecer depois de restart/HUP, na espera de workers, rotas ou selo (`bin/deploy.sh:298`, `bin/deploy.sh:315`, `bin/deploy.sh:328`, `bin/deploy.sh:374`, `bin/deploy.sh:381`). Estado efetivo nessas falhas: **[não verificado]**.

4. **Trava do vigia não estabelece Single Writer global.** Vigia usa trava própria e faz `git revert` (`bin/vigia_logins.sh:71-87`), enquanto a porta compartilhada da árvore é `/tmp/cadeia.lock` (`bin/trava.sh:57-70`), usada pelas cadeias (`bin/molde_fatia/cadeia.sh:11`, `bin/molde_autocura/cadeia.sh:10`). `auto_revert` também reverte sem adquirir essa porta dentro da função (`app/core/esteira_vigia.py:1242-1291`). Sobreposição real em prod: **[não verificado]**.

## 04/11 12:22 · sessao 00bdd655fe03 · - · - · origem=Ronald (tmux)

1. **Existe HUP sem `recusa_restart_com_cron`, por desenho:** normal, `--sem-migrate`, `--sem-sombra`, `--reload-copia` e `--reload-agendado` convergem no único `docker kill -s HUP`; a recusa só é chamada para `RESTART=1` (`bin/deploy.sh:112-114`, `bin/deploy.sh:295-315`). O único `docker restart` está imediatamente após a segunda recusa (`bin/deploy.sh:297-298`).

   **`--so-conferir` não existe no parser.** Só `--conferir` liga `SO_CONFERIR`; `--so-conferir` atravessa o fluxo normal, podendo migrar e emitir HUP (`bin/deploy.sh:56-62`, `bin/deploy.sh:167`, `bin/deploy.sh:171-174`, `bin/deploy.sh:315`). Não encontrei um modo válido silenciosamente esquecendo reload: as saídas anteriores são recusas e o modo de conferência (`bin/deploy.sh:89`, `bin/deploy.sh:147-174`, `bin/deploy.sh:213-235`, `bin/deploy.sh:267-273`).

2. **No deploy não há retry de sinal nem fallback restart:** HUP falho chama `falhou`; o laço apenas observa PIDs e também termina em falha (`bin/deploy.sh:315-328`). O único restart explícito é `bin/deploy.sh:298`; os outros comandos Docker de partida localizados em scripts operacionais são para os bancos de teste/sombra (`bin/db_teste.sh:70`, `bin/sombra_db.sh:100`). Não há operação compose no fluxo de deploy; só comentários indicando o ato separado de infraestrutura (`bin/deploy.sh:20-22`, `bin/deploy.sh:65-66`).

   **Limitação Single Writer:** as duas recusas não excluem um cron que comece entre checagem e restart; o envelope cria a marca sem trava compartilhada, e a segunda consulta é seguida pelo restart (`bin/cron_run.sh:9-11`, `bin/deploy.sh:105-110`, `bin/deploy.sh:297-298`). Há passagem pela recusa, mas não exclusão mútua com o início dos crons nesses pontos.

   **Prova de workers permite vazio:** PIDs anteriores vazios só avisam e pulam; consulta atual vazia satisfaz “nenhum antigo restou” e aprova (`bin/deploy.sh:318`, `bin/deploy.sh:321-324`). A prova de rota executa um processo Python novo por `docker exec`, sem consultar o worker HTTP (`bin/deploy.sh:353-356`).

3. **Guarda mensageria:** qualquer rc não zero, incluindo container indisponível e rc diferente de 1, define `PEND_MSG=1`; bloqueia antes da migration do core e do reload em todos os modos, salvo `--conferir` e `--restart` (`bin/deploy.sh:141-150`, `bin/deploy.sh:167`). Isso inclui agendado e cópia, pois os dois mantêm `SO_CONFERIR=0`, `RESTART=0` (`bin/deploy.sh:57-79`). Conferência pode sair zero mesmo sem resposta do mensageria, apenas imprimindo o estado (`bin/deploy.sh:146-147`, `bin/deploy.sh:171-174`).

   Recusar às 03:30 com migration pendente é coerente com o desenho registrado: o cron declara que não deve publicar fatia inacabada, o HUP publica a árvore viva, e a migration mensageria está no comando de boot (`app/config/crons.py:303-308`, `bin/deploy.sh:187-189`, `docker-compose.mensageria.yml:20`). O custo é deixar a higiene de memória sem ocorrer até correção; o próprio propósito desse cron é pagar o vazamento retirado da reciclagem por contagem (`app/config/crons.py:300-306`). Ocorrência real em produção: [não verificado].

**Nota para consolidação do item 4:** rc não zero não prova “PROD SEGUE no código de antes”: erros podem acontecer depois de emitir HUP, por rota ou selo (`bin/deploy.sh:315`, `bin/deploy.sh:374`, `bin/deploy.sh:381`); a mensagem do integrador é forte demais nesse aspecto (`app/core/integrador_lote.py:757-760`).

## 04/11 11:33 · sessao c5de69a10b63 · DEPLOY-[nome]-ELO · verificador · origem=Code

1. **Veredito: bloqueia. As curas da marca e da pauta são parciais.** Há caminho para restart com cliente vivo e caminho para pauta aberta sem fechamento automático (`bin/em_casca.sh:28-38`; `bin/admissao_cron.sh:202-208`; `bin/deploy.sh:368-375`). Execução da suíte, morte efetiva dos mutantes e ocorrência desses caminhos em produção: **[nao verificado]**.

   Para cada PROCEDENTE da seção `codex_v2.r`:

   | Item | Resultado em e8f72370 | Gravidade |
   |---|---|---|
   | SIGKILL com exec vivo | Cura quando a transferência **já terminou**. Continua descoberto entre lançar o filho e publicar sua marca, ou quando `sed/mv` falha. A transferência ainda introduz a corrida de varredura descrita no item 2 (`bin/em_casca.sh:28-38`). | **bloqueia** |
   | Pauta perdida; resolver falhando | Resolver não zero conserva a recusa. Entretanto, pauta pode nascer sem recusa ou depois de sua remoção; nesses casos `_tira_recusa` não a alcança (`bin/deploy.sh:368-375`; `app/core/integrador_lote.py:773-780`). | **bloqueia** |
   | CLI por AST aceitava `!=` | O caso agora executa a porta positiva e rejeita a inversão direta dessa condição. Falta testar outra porta; uma condição excessivamente abrangente ainda passa (`bin/tests/test_deploy_nao_mata_elo.sh:930-934`; `app/core/esteira_vigia.py:1652`). | Curado no alvo específico; lacuna em **fila** |
   | Falha da pauta do lote | O cenário injeta 70 e exige aviso no relato; a produção trata qualquer retorno verdadeiro de `_pauta` (`bin/tests/test_deploy_nao_mata_elo.sh:789-792`; `app/core/integrador_lote.py:780-781`). | Curado por leitura; execução **[nao verificado]** |
   | Justificativa da espera por env | A correção procede: `ADM_ESPERA_ADMISSAO_S` pode alongar a espera, pois não há limite superior. Isso não altera o teto declarado do restart (`bin/admissao_cron.sh:50`, `bin/admissao_cron.sh:62-68`, `bin/admissao_cron.sh:243`). | **fila** |

   A regressão mais direta é o segundo escritor de marcas, fora da trava usada pela porta original: publicar a marca nova antes de remover a velha **não garante** que uma varredura veja alguma delas (`bin/em_casca.sh:33-36`; `bin/admissao_cron.sh:202-203`).

2. **Caminhos de `bin/em_casca.sh`.**

   | Caminho | Marca, processo e retorno | Gravidade |
   |---|---|---|
   | Filho termina antes da leitura do starttime | Se `/proc` já sumiu, não transfere; `wait` recupera o resultado e EXIT normalmente remove a marca do wrapper. Não resta comando vivo nesse caminho normal (`bin/admissao_cron.sh:84-91`; `bin/em_casca.sh:23`, `bin/em_casca.sh:32`, `bin/em_casca.sh:41-42`). | Sem bloqueio nesse caminho |
   | `sed` ou `mv` falha | O comando **já começou**; permanece somente a marca do wrapper. Enquanto ele vive, protege. Se morrer antes do filho, o reaper remove essa marca pelo PID morto do wrapper, deixando o cliente vivo sem proteção (`bin/em_casca.sh:28`, `bin/em_casca.sh:34-38`; `bin/admissao_cron.sh:169-175`, `bin/admissao_cron.sh:208`). | **bloqueia** |
   | SIGTERM no wrapper | Há somente trap EXIT. Após transferência, preserva a marca do filho vivo; antes dela ou no caminho de falha, preserva um arquivo cuja identidade ainda é a do wrapper morto. O retorno observado é o término do wrapper, não o resultado futuro do comando (`bin/em_casca.sh:23`, `bin/em_casca.sh:28-38`). | **bloqueia** antes da transferência; retorno do comando **[nao verificado]** após o sinal |
   | SIGKILL no wrapper | Depois da transferência, a marca continua identificando o filho. Antes dela, ou após falha de transferência, a marca identifica o wrapper morto e pode ser recolhida enquanto o cliente vive (`bin/em_casca.sh:28-38`; `bin/admissao_cron.sh:169-177`). | **bloqueia** |
   | Filho acaba após SIGKILL do wrapper | Não há wrapper para desmarcar: fica arquivo órfão até a próxima varredura, que verifica identidade e o remove (`bin/em_casca.sh:23`, `bin/em_casca.sh:33-36`; `bin/admissao_cron.sh:199-208`). | **fila**, comportamento de reaper |
   | PID do filho reutilizado | EXIT usa apenas `kill -0`, podendo conservar arquivo por causa de outro processo. O reaper compara starttime; se a reutilização anteceder a captura, pode registrar a identidade substituta. `exe` é substring da cmdline, não identidade do executável (`bin/em_casca.sh:23`, `bin/em_casca.sh:32-34`; `bin/admissao_cron.sh:175-177`). | **fila** |
   | rc 64 | Uso/nome inválido sai antes de marcar. Alvo fora das cascas marca primeiro, mas sai antes de lançar o comando e desmarca pelo EXIT (`bin/em_casca.sh:12-13`, `bin/em_casca.sh:21-28`). | Sem execução escondida |
   | Admissão 75/70 | Retorna antes de lançar o comando. 75 vem da espera pela trava; 70, das falhas de preparação/publicação da marca (`bin/em_casca.sh:16-19`; `bin/admissao_cron.sh:241-248`). | Retornos previstos |
   | Execução normal | O resultado de `wait` é devolvido; o resultado da transferência não o substitui (`bin/em_casca.sh:41-42`). | Sem troca de rc nesse caminho |

   **A corrida existe mesmo sem sinal:** a varredura final expande o glob contendo apenas a marca velha; o wrapper publica a nova e remove a velha; o teste `-e` pula a velha desaparecida; a nova não pertence à enumeração. A varredura retorna vazio e o restart começa com o cliente vivo (`bin/admissao_cron.sh:202-203`; `bin/em_casca.sh:35-36`; `bin/reinicio_sob_admissao.sh:43-45`, `bin/reinicio_sob_admissao.sh:66`). **Bloqueia.**

   **Sim, a marca nova também pode ser publicada depois da verificação vazia:** se o wrapper morrer após lançar o processo externo `mv`, mas antes de ele publicar, a varredura pode recolher a marca do wrapper morto e concluir vazio; o `mv` remanescente publica sem consultar fd8 (`bin/em_casca.sh:35`; `bin/admissao_cron.sh:170`, `bin/admissao_cron.sh:208`; `bin/reinicio_sob_admissao.sh:44-45`). É uma interleaving deduzida do código; reprodução e ocorrência em prod **[nao verificado]**.

3. **Quem escreve, lê, remove e trata os retornos.**

   **Marcas `logs/crons_em_curso/*`:**

   | Papel | Implementação |
   |---|---|
   | Escritor original | `adm_marcar` publica `${nome}_${emp}.$$`, com `ini`, `pid`, `starttime`, `exe`, `casca` e `cmd`, usando temporário e `mv` sob fd8 (`bin/admissao_cron.sh:239-250`). |
   | Chamadores da porta | Comando e alarme do cron; descoberta da corrente; `em_casca` (`bin/cron_run.sh:16`, `bin/cron_run.sh:52`; `bin/encadeado.sh:48`; `bin/em_casca.sh:16`). |
   | Segundo escritor | `em_casca` publica `${N}_-.${filho}`, altera pid/starttime/exe, conserva os demais campos e remove a marca anterior, sem retomar fd8 (`bin/em_casca.sh:33-36`). |
   | Leitor da identidade | `adm_marca_viva` usa o PID **do nome do arquivo**; não usa `pid=`. Compara starttime e procura `exe` na cmdline (`bin/admissao_cron.sh:169-177`). |
   | Leitor da proteção | `adm_segura` lê `casca`; formato antigo segura; `casca=0` ainda consulta descendentes (`bin/admissao_cron.sh:181-186`). |
   | Varredor/reaper | `adm_varrer` verifica identidade, desconsidera ancestrais como bloqueadores e remove órfãs (`bin/admissao_cron.sh:199-208`). |
   | Consumidor do restart | `adm_seguram` filtra a varredura; o restart consulta antes e depois de tomar fd8 (`bin/admissao_cron.sh:213`; `bin/reinicio_sob_admissao.sh:40-45`). |
   | Remoção explícita | `adm_desmarcar`, chamado pelos envelopes e pelo EXIT (`bin/admissao_cron.sh:254`; `bin/cron_run.sh:22`, `bin/cron_run.sh:51`, `bin/cron_run.sh:58`; `bin/encadeado.sh:50`; `bin/em_casca.sh:23`). |

   O contrato verifica a presença textual dessa infraestrutura; os casos do selo também leem as marcas nas raízes falsas (`bin/contrato_deploy.sh:55-57`; `bin/tests/test_deploy_nao_mata_elo.sh:635`, `bin/tests/test_deploy_nao_mata_elo.sh:675`).

   **Recusas e pautas:**

   | Estado | Cria/abre | Lê | Apaga/fecha |
   |---|---|---|---|
   | `recusa_cron.<id>.pendente` | Deploy, somente no ramo restart75, com ID e argv separados por NUL (`bin/deploy.sh:284-287`). | Agendado valida ID e lê argv; retentar lê `auto_revert`; contador conta nomes; limpeza lê ID/mtime (`bin/deploy_agendado.sh:83-87`, `bin/deploy_agendado.sh:101`; `app/core/esteira_vigia.py:1344-1354`; `bin/deploy_recusado.sh:22-27`; `bin/deploy.sh:369`, `bin/deploy.sh:382`). | Deploy por `_tira_recusa`; **também agendado**, preparando IDs e substituindo a recusa anterior (`bin/deploy.sh:375-382`; `bin/deploy_agendado.sh:95`, `bin/deploy_agendado.sh:104`, `bin/deploy_agendado.sh:110`). |
   | `deploy_recusado:integrador` | `_deploy_do_lote` (`app/core/integrador_lote.py:780`). | `pauta_viva` (`app/core/management/commands/alarme_esteira.py:13-15`). | `_tira_recusa` chama o resolver (`bin/deploy.sh:370-375`). |
   | `deploy_recusado:auto_revert` | Primeiro deploy e retentativa (`app/core/esteira_vigia.py:1331`, `app/core/esteira_vigia.py:1356`). | Mesmo leitor (`app/core/management/commands/alarme_esteira.py:13-15`). | Mesmo resolvedor (`bin/deploy.sh:370-375`). |
   | `deploy_recusado:vigia_logins` | Falha do deploy do revert (`bin/vigia_logins.sh:91-95`). | Mesmo leitor (`app/core/management/commands/alarme_esteira.py:13-15`). | Mesmo resolvedor (`bin/deploy.sh:370-375`). |

   A escrita da pauta continua pela porta `escrever`; o fechamento passa por `marcar_feita_pelo_sistema`. Não surgiu escrita direta de `feito_em` nesse trecho (`app/core/management/commands/alarme_esteira.py:27-31`, `app/core/management/commands/alarme_esteira.py:54-56`; `app/pautas/services.py:269-275`).

   **Os três IDs que abrem pauta podem ficar sem fechamento automático**, por dois caminhos:

   - Todos abrem para rc diferente de zero, mas somente restart75 produz recusa. Falha sem arquivo deixa `_tira_recusa` sem entrada para resolver (`app/core/integrador_lote.py:773-780`; `app/core/esteira_vigia.py:1326-1331`; `bin/vigia_logins.sh:91-95`; `bin/deploy.sh:284-290`, `bin/deploy.sh:368`). **Bloqueia.**
   - Outro deploy pode resolver antes de o chamador abrir a pauta e apagar a recusa. Resolver pauta inexistente retorna normalmente; depois o chamador abre a pauta sem arquivo restante (`app/core/management/commands/alarme_esteira.py:28-33`; `bin/deploy.sh:370-375`; `app/core/esteira_vigia.py:1325-1331`). **Bloqueia.**

   `manual` e `agendado.*` também produzem recusas, mas os abridores acima usam os três IDs fixos; não há abertura correspondente a cada ID do agendado (`bin/deploy.sh:52-54`; `bin/deploy_agendado.sh:94-105`; `app/core/integrador_lote.py:780`; `bin/vigia_logins.sh:95`).

   **Retorno de `deploy.sh` nos chamadores:** [nao verificado]

   | Chamador | Tratamento |
   |---|---|
   | Integrador: reload inicial | Descarta rc e segue para a suíte (`app/core/integrador_lote.py:638-639`). **Fila.** |
   | Integrador: publicação final | Lê rc; falha impede FIM e retirada da fila (`app/core/integrador_lote.py:748-752`, `app/core/integrador_lote.py:771-782`). |
   | Auto-revert inicial | Lê rc e interrompe antes da conferência em falha (`app/core/esteira_vigia.py:1276-1284`). |
   | Revert do revert | Lê rc e tenta incluí-lo na pauta `auto_revert`; pauta já viva não é reescrita (`app/core/esteira_vigia.py:1289-1294`; `app/core/management/commands/alarme_esteira.py:38-41`). **Fila.** |
   | Retentar | Lê rc; CLI retorna 0 e vigia usa `tail`, sem decidir pelo status (`app/core/esteira_vigia.py:1354-1362`, `app/core/esteira_vigia.py:1652-1654`; `bin/vigia_arvore.sh:28`). |
   | Vigia de logins | Lê rc e abre pauta em falha; o próprio vigia termina em 2 nesse caminho de alarme (`bin/vigia_logins.sh:90-96`, `bin/vigia_logins.sh:105`). |
   | Agendado | Captura rc da esteira e da repetição; repetição bem-sucedida vira 3 pela cauda não executada; devolve `PIOR` (`bin/deploy_agendado.sh:96-106`, `bin/deploy_agendado.sh:123-135`). |
   | Dois moldes de cadeia | Reload inicial guardado; publicação final sujeita a `set -e`; rollback ignora rc sob `set +e` (`bin/molde_fatia/cadeia.sh:4`, `bin/molde_fatia/cadeia.sh:26-33`, `bin/molde_fatia/cadeia.sh:48`, `bin/molde_fatia/cadeia.sh:89`; `bin/molde_autocura/cadeia.sh:3`, `bin/molde_autocura/cadeia.sh:25-32`, `bin/molde_autocura/cadeia.sh:45`, `bin/molde_autocura/cadeia.sh:86`). Rollbacks: **fila**. |
   | `o135-teto20` | Conferência guardada; publicação captura e propaga rc (`fatias_agendadas/o135-teto20/esteira.sh:80`, `fatias_agendadas/o135-teto20/esteira.sh:116-124`). |
   | `abono-no-ar` | `pipefail` e condicional convertem falha do deploy em 1 (`fatias_agendadas/abono-no-ar/esteira.sh:21`, `fatias_agendadas/abono-no-ar/esteira.sh:37-38`). |
   | Reload diário RAW | Passa pelo envelope que registra e devolve rc (`app/config/crons.py:307`; `bin/cron_run.sh:24`, `bin/cron_run.sh:29`, `bin/cron_run.sh:70`). |

   **RC do resolver:** qualquer não zero é reduzido a 1 em `_tira_recusa`, conservando arquivo. Esse 1 é ignorado pelos chamadores internos, e o deploy termina em `echo`, devolvendo 0; portanto rc0 não prova pauta fechada nem recusa removida (`bin/deploy.sh:370-385`). **Fila**, pois o restart pode ter passado e a pendência continua visível.

4. **Os casos mordem alguns erros, mas deixam estas mutações plausíveis passar.** Sobrevivência experimental dos mutantes: **[nao verificado]**; abaixo, dedução por leitura.

   | Caso | Mordida e mutação sobrevivente |
   |---|---|
   | 19(a) | Cobra marca protetora, recusa75 e zero restart. Remover a transferência ainda passa esse caso isolado: o wrapper continua vivo; o laço não exige que encontrou PID diferente (`bin/tests/test_deploy_nao_mata_elo.sh:632-640`). |
   | 19(f) | Cobra SIGKILL **depois** de esperar a transferência. Trocar EXIT por `trap adm_desmarcar EXIT` passa (a)/(f), mas quebra TERM com filho vivo; SIGKILL não executa EXIT (`bin/tests/test_deploy_nao_mata_elo.sh:670-676`; `bin/em_casca.sh:23`). |
   | 21 limpeza | Executa o trecho real e verifica os resolvedores. Apagar a criação real do marcador passa: o teste fabrica `.restart_inicio.$$`; a extração do caso8 também começa depois da criação (`bin/tests/test_deploy_nao_mata_elo.sh:233`, `bin/tests/test_deploy_nao_mata_elo.sh:752-766`; `bin/deploy.sh:282`). |
   | 21 resolver | Injeta somente 0 e 1. Reter arquivo só para rc1, apagando para 70/75, passa esses cenários e perde a pendência em falha de admissão (`bin/tests/test_deploy_nao_mata_elo.sh:765-771`; `bin/deploy.sh:370-375`). |
   | 21 pauta70 | Trocar `if rp` por `if rp == 70` passa e omite aviso para 75; o teste também não exige o número correto no aviso (`app/core/integrador_lote.py:781`; `bin/tests/test_deploy_nao_mata_elo.sh:789-792`). |
   | 23 retentar | Trocar `if rc != 0` por `if rc == 75` passa os cenários 75/0 e anuncia sucesso para rc1 (`app/core/esteira_vigia.py:1355`; `bin/tests/test_deploy_nao_mata_elo.sh:907-917`). |
   | 23 CLI | Usar `if argv[1:2] != ['auto-revert']` aceita retentar e passa a sentinela, mas sequestra outras portas. Não há chamada negativa, apesar do comentário (`app/core/esteira_vigia.py:1652`; `bin/tests/test_deploy_nao_mata_elo.sh:925-934`). |

   As lacunas isoladas são **fila de mordida**; a falta de cobertura das janelas de transferência acompanha o **bloqueio** do item 2 (`bin/tests/test_deploy_nao_mata_elo.sh:670-671`; `bin/em_casca.sh:28-38`).

5. **Classes históricas no delta da volta 3:**

   | Classe | Resultado |
   |---|---|
   | (a) Segundo juiz registrado | Não identifiquei duplicação de juiz de domínio: o novo fechamento chama o comando existente, que usa as portas de pauta (`bin/deploy.sh:370-371`; `app/core/management/commands/alarme_esteira.py:27-31`). O registro vivo, e não a tabela de consulta, é a autoridade declarada (`app/docs/JUIZES.md:10-12`). |
   | (b) Segundo escritor | **Aparece:** transferência escreve marca fora de `adm_marcar` (`bin/em_casca.sh:33-36`; `bin/admissao_cron.sh:239-250`). **Bloqueia.** |
   | (c) Relógio fora da borda | Não identifiquei leitura nova de relógio no trecho de transferência ou limpeza acrescentado (`bin/em_casca.sh:32-38`; `bin/deploy.sh:366-383`). |
   | (d) Derivação paralela | A lista fixa foi retirada; o fechamento deriva ID do arquivo. Essa parte melhora a origem (`bin/deploy.sh:369-371`; `app/core/esteira_vigia.py:1359`). |
   | (e) Selo sem mordida | Há mordida parcial e lacunas concretas, enumeradas no item 4 (`bin/tests/test_deploy_nao_mata_elo.sh:759`, `bin/tests/test_deploy_nao_mata_elo.sh:789-792`, `bin/tests/test_deploy_nao_mata_elo.sh:930-934`). **Fila**, além da prova insuficiente do caminho bloqueante. |
   | (f) Trava/fd | **Aparece:** troca fora de fd8 quebra a garantia da segunda varredura. O fechamento original do fd antes dos filhos continua correto (`bin/admissao_cron.sh:250`; `bin/em_casca.sh:33-36`; `bin/reinicio_sob_admissao.sh:43-45`). **Bloqueia.** |
   | (g) Pouso antes do pré-requisito | Pouso e atendimento dos pré-requisitos operacionais: **[nao verificado]**. O aval os exige antes de PRONTO (`AVAL_DEPLOY-ELO-B3.txt:14`, anexo). | [nao verificado]
   | (h) Fallback/env solta | **Aparece fallback:** falha da transferência mantém a marca do wrapper, insuficiente após sua morte (`bin/em_casca.sh:37-38`). **Bloqueia.** A env sem limite superior permanece na admissão (`bin/admissao_cron.sh:50`, `bin/admissao_cron.sh:243`). **Fila.** |
   | (i) Erro engolido | Não há novo `try/except` nesse trecho; há equivalente em shell: erro de leitura pode virar ID vazio e remoção, e erro do resolver não altera o rc final do deploy (`bin/deploy.sh:369-385`). **Fila.** |
   | (j) Restart com exec vivo | **Aparece** pela troca de marca e permanece no caminho deploy × deploy (`bin/em_casca.sh:33-36`; `bin/reinicio_sob_admissao.sh:44-66`; `bin/deploy.sh:300`, `bin/deploy.sh:316`). **Bloqueia.** |

6. **Triagem para FILA: motivos sustentados e exceções.**

   - **Deploy × deploy: não sustenta como autorização de fechamento. Bloqueia.** A exclusão do vigia inteiro permite que ele lance o deploy automático sem marca permanente protetora; migrate/provas continuam fora da admissão. Podem começar depois da última varredura de outro restart. A existência anterior da corrida em outros chamadores não protege esse caminho, nem revoga a proibição escrita de reiniciar sobre exec vivo (`bin/admissao_cron.sh:36`, `bin/admissao_cron.sh:229-231`; `bin/vigia_arvore.sh:28`; `bin/deploy.sh:102`, `bin/deploy.sh:124`, `bin/deploy.sh:300`; `bin/reinicio_sob_admissao.sh:43-66`; `AVAL_DEPLOY-ELO-B3.txt:13`, anexo).

   - **SIGKILL não trapável: não sustenta a transferência incompleta. Bloqueia.** O problema não exige trapar KILL: exige que o comando lançado continue representado durante toda a janela. Hoje ele começa antes da transferência e há fallback para a identidade do wrapper (`bin/em_casca.sh:28-38`).

   - **Outros consumidores que ignoram `_pauta`: motivo de escopo sustenta fila.** `_pauta` anuncia falha em stderr, mas chamadas continuam sem tratar seu retorno; isso não prova persistência da pauta nesses usos (`app/core/esteira_vigia.py:900-905`, `app/core/esteira_vigia.py:1274`, `app/core/esteira_vigia.py:1291`, `app/core/esteira_vigia.py:1632-1637`). **Fila.**

   - **Revert do revert: fila, com justificativa imprecisa.** O rc pode ser engolido por pauta já viva. Também não há ID explícito: usa o ambiente do processo pai ou `manual`; o `env` local do primeiro deploy não altera esse ambiente (`app/core/esteira_vigia.py:1289-1294`, `app/core/esteira_vigia.py:1324-1325`; `bin/deploy.sh:52`; `app/core/management/commands/alarme_esteira.py:38-41`).

   - **Chave antiga sem sufixo: fila; existência em prod [nao verificado].** O fechamento atual deriva `deploy_recusado:<id>` do arquivo, e os abridores atuais usam sufixos (`bin/deploy.sh:369-371`; `app/core/esteira_vigia.py:1331`; `app/core/integrador_lote.py:780`; `bin/vigia_logins.sh:95`).

   - **Lacunas do caso8, marcador fabricado, segundo deploy só75 e universo derivado do classificador: fila de mordida sustentada.** Os testes cobram existência da recusa, fabricam o marcador, injetam75 no segundo deploy e derivam o universo da própria autoridade (`bin/tests/test_deploy_nao_mata_elo.sh:208`, `bin/tests/test_deploy_nao_mata_elo.sh:759`, `bin/tests/test_

[truncado em 20000 de 24144 caracteres]

## 04/11 11:27 · sessao 931f9f956368 · - · - · origem=Ronald (tmux)

Leitura estática concluída; execução/mutantes/suite verde: **[nao verificado]**. O selo cria e apaga arquivos, portanto não o executei (`bin/tests/test_deploy_nao_mata_elo.sh:44`).

1. **Os casos novos mordem alvos concretos, mas têm lacunas.**
   
   | Caso | O que efetivamente cobra | Mutação plausível que deixa passar |
   |---|---|---|
   | 19(a) | Marca `t19a_-|1`, recusa 75, zero restart, retorno final 0 e diretório vazio (`bin/tests/test_deploy_nao_mata_elo.sh:632-640`). | Remover transferência ao filho ainda passa **19(a)** isoladamente: a marca do wrapper vivo basta; o laço não afirma que encontrou PID diferente (`:634`). O 19(f) acrescentado cobre essa mutação (`:670-676`). |
   | 19(f) | Mata wrapper com SIGKILL depois de esperar a transferência, exige cliente vivo, recusa e limpeza posterior (`:668-676`). | Trocar `bin/em_casca.sh:23` por `trap adm_desmarcar EXIT`: passa (a), que termina normalmente, e (f), cujo SIGKILL não dispara EXIT; quebra SIGTERM com filho vivo. Esses testes não exercitam TERM nem falha de `sed`/`mv` (`:632-640`, `:668-676`; produção `bin/em_casca.sh:34-38`). |
   | 21 limpeza | Executa o trecho real, verifica os dois resolvedores e a sobrevivência apenas da recusa posterior (`:752-767`); com resolver 1 exige conservar as três (`:769-771`). | Apagar a criação real de `.restart_inicio.$$` em `bin/deploy.sh:282`: o teste fabrica o marcador (`:759`), e o caso8 começa a extração depois da criação (`:233`). |
   | 21 resolver | Distingue resolver 0 e 1 (`:765-771`). | Reter recusa somente quando resolver devolve 1, apagando para 70/75: passa esses cenários, mas perde recusa quando admissão falha; produção trata todos os não zero (`bin/deploy.sh:370-375`). |
   | 21 pauta70 | Executa `_deploy_do_lote` com `_pauta` devolvendo 70 e busca `PAUTA NAO ABRIU` (`:789-792`, `:844`). | Em `app/core/integrador_lote.py:781`, substituir `if rp` por `if rp == 70`: passa e omite falha 75. Hardcode de rc no texto também passa: o teste verifica só a expressão, não o número (`:792`). |
   | 23 retentar | Exercita ausência, recusa75 e sucesso0, argv/id e quantidade de relato/pautas (`:904-917`). | Em `app/core/esteira_vigia.py:1355`, substituir `if rc != 0` por `if rc == 75`: passa e anuncia sucesso para rc1. |
   | 23 CLI | Executa `main(['x','retentar-deploy'])` com sentinela e exige rc0 (`:930-934`). | Em `app/core/esteira_vigia.py:1652`, usar `if argv[1:2] != ['auto-revert']`: aceita retentar, passa o teste e sequestra zumbido/outras portas. O comentário promete “e outra porta nao”, mas não há chamada negativa (`:925-934`). |

   **Gravidade:** lacunas de mordida, **fila**, exceto quando deixam sem prova exigência explícita do aval; exigência/mutantes efetivamente mortos do anexo: **[nao verificado]**.

2. **Regressão de composição: o segundo apagador continua burlando o novo contrato de limpeza.** `_tira_recusa` conserva arquivo quando resolver falha (`bin/deploy.sh:370-373`), mas essa falha não vira o resultado do deploy, que termina com `echo` (`:377-385`). O agendado apaga `_rec_ant` sempre que recebe rc0 (`bin/deploy_agendado.sh:110`), podendo remover justamente a recusa que o resolvedor conservou. A explicação “só substitui a própria recusa” não sustenta a alegação geral “só tira se pauta fechou” (`bin/deploy.sh:363-365`). IDs próprios do agendado abrirem pauta: **[nao verificado]**; não afirmar pauta órfã automaticamente nesse cenário. **Gravidade: fila**, ou bloqueio se o contrato reivindicado for universal.

3. **FILA deploy × deploy: o fato está confirmado; a autorização para fechá-lo como fila não.** A admissão fecha somente dentro do reinício (`bin/reinicio_sob_admissao.sh:40-45`) e abre com a saída desse script (`:73`); a prova posterior usa exec sem marca (`bin/deploy.sh:300-305`). A chamada automática está antes da pausa do vigia (`bin/vigia_arvore.sh:24-29`), cujo run é excluído do universo inteiro pela declaração/classificação (`bin/admissao_cron.sh:34`, `:229-231`). Portanto outro restart pode alcançar esses execs. O aval fornecido proíbe reiniciar sobre exec vivo e condiciona PRONTO a provas; citar como **anexo AVAL_DEPLOY-ELO-B3.txt [nao verificado]** se não houver arquivo local. **Gravidade: bloqueia**, apesar de preexistir parcialmente; mudar o mecanismo necessário não prova que o risco esteja fora da condição escrita.

4. **Demais motivos de FILA que consegui conferir.**
   
   - `_pauta` já expõe stderr e devolve rc (`app/core/esteira_vigia.py:900-905`), mas vários chamadores descartam retorno (`:959`, `:1179`, `:1241`, `:1244`, `:1274`, `:1291`, `:1393`, `:1400`, `:1632`, `:1637`). Motivo de escopo dos três chamadores é coerente com o texto do aval fornecido **[nao verificado]**. **Fila**.
   - Revert do revert realmente usa outra pauta (`auto_revert`) e não informa ID no subprocess (`app/core/esteira_vigia.py:1289-1294`); o padrão do deploy é `manual` se não houver ID no ambiente (`bin/deploy.sh:52`). Pauta já viva não é reescrita (`app/core/management/commands/alarme_esteira.py:38-41`). Assim a alegação “rc chega à pauta” não vale sempre. **Fila**, mas considerar não comprovada a cobertura operacional.
   - Falta de teste da criação do marcador é real (`bin/tests/test_deploy_nao_mata_elo.sh:759` versus `bin/deploy.sh:282`). **Fila de mordida**.
   - Revert do revert só recebe segundo deploy75 no teste (`bin/tests/test_deploy_nao_mata_elo.sh:828-835`). **Fila de mordida**.
   - Mutante que remove a limpeza de `_rec_ant` não é morto pelo cenário persistente, que cobra existência, não quantidade (`bin/tests/test_deploy_nao_mata_elo.sh:208`); cenário rc1 cobra quantidade1, mas não percorre a substituição bem-sucedida (`:218-225`). **Fila de mordida**.
   - Remoção de `_rec_ant` antecede a validação do conteúdo do ID novo (`bin/deploy_agendado.sh:110`, `:115`); produtor escreve ID do ambiente (`bin/deploy.sh:286`). Premissa operacional de produtor íntegro sustenta fila, mas não torna a sequência atômica. **Fila**.
   - Reload inicial do integrador ignora rc (`app/core/integrador_lote.py:638`); rollback dos dois moldes também (`bin/molde_fatia/cadeia.sh:32`, `bin/molde_autocura/cadeia.sh:31`). Classificação como preexistentes depende do diff fornecido **[nao verificado]**. **Fila**.
   - `ADM_ESPERA_ADMISSAO_S` realmente não tem limite superior (`bin/admissao_cron.sh:50`, `:243`), enquanto teto de restart só pode baixar (`:62-68`). Correção da justificativa procede. **Fila**, sem confundir os dois tetos.
   - CLI imprime resultado e retorna0 (`app/core/esteira_vigia.py:1652-1654`); vigia passa por `tail` sem decidir (`bin/vigia_arvore.sh:28`). Motivo de fila procede para esse uso. **Fila**.
   - Campos argv vazios realmente são descartados (`app/core/esteira_vigia.py:1352`); argv produzido pelo auto-revert nessa chamada não contém campo vazio (`:1276`). Motivo específico procede. **Fila**.
   - Retentar não refaz conferência/marcação: rc0 somente relata e retorna (`app/core/esteira_vigia.py:1359-1362`), enquanto auto-revert original confere e marca (`:1283`, `:1299-1303`). **Fila**, sem apresentar a tentativa como prova de cura.
   - Corte por mtime ocorre antes da espera (`bin/deploy.sh:282-283`), conservando arquivos posteriores (`:382`). No caso declarado isso superconta; motivo de lado conservador procede. **Fila**.

5. **Classes históricas relevantes no delta3.**
   
   - **(b)/(f)/(h)/(j):** transferência escreve marca fora de `adm_marcar`/fd8 e recua à marca do wrapper se falha (`bin/em_casca.sh:32-38`), enquanto a porta canônica fecha/abre fd8 (`bin/admissao_cron.sh:242-250`); os detalhes do risco devem vir da análise do agente de marcas.
   - **(d):** a lista paralela `DEPLOY_RECUSA_IDS` não aparece no trecho atual; fecho deriva ID do arquivo (`bin/deploy.sh:369-371`; `app/core/esteira_vigia.py:1311-1315`). Remoção no delta: anexo **[nao verificado]**.
   - **(e):** lacunas acima; não confundir testes que mordem alguns mutantes com prova de todos os caminhos (`bin/tests/test_deploy_nao_mata_elo.sh:670-676`, `:759`, `:789-792`, `:930-934`).
   - **(a)/(c)/(g)/(i):** não identifiquei instância nova nos trechos examinados; conclusão global sobre delta3 **[nao verificado]**.
   - Conferência da hipótese de rede: o harness chama HTTP dentro de `_TIRO` (`eval/harness.py:14-24`), executado sob em_casca (`:53-56`); não é uma chamada HTTP direta do run no host nessa implementação. Linha correspondente no RELATO exigida pelo aval: **[nao verificado]**.

## 04/11 11:27 · sessao 953adbce7ef6 · - · - · origem=Ronald (tmux)

Achados principais, por leitura estática:

- **Bloqueia — cura da pauta incompleta e regressão da recuperação anterior.** Os três chamadores abrem `deploy_recusado:<id>` para qualquer rc diferente de zero: `app/core/integrador_lote.py:773-780`, `app/core/esteira_vigia.py:1326-1333`, `bin/vigia_logins.sh:91-96`. A recusa só nasce quando o restart devolve 75 (`bin/deploy.sh:284-290`). Logo, rc 1 antes/depois do restart pode abrir pauta sem arquivo; `_tira_recusa` retorna imediatamente sem arquivo (`bin/deploy.sh:368`), e o antigo fechamento dos IDs sem recusa foi removido, restando apenas relato após sucesso (`app/core/esteira_vigia.py:1359-1362`). Esses três IDs podem, portanto, ter pauta aberta sem caminho automático que a feche. A ocorrência em prod é [nao verificado].

- **Bloqueia — corrida abertura versus fechamento continua.** Após o deploy recusado retornar, o chamador ainda vai abrir a pauta (`app/core/esteira_vigia.py:1325-1331`; `app/core/integrador_lote.py:771-780`; `bin/vigia_logins.sh:90-95`). Outro deploy pode primeiro resolver uma pauta inexistente — operação que retorna normalmente (`app/core/management/commands/alarme_esteira.py:28-33`) — e apagar a recusa (`bin/deploy.sh:370-375`). O primeiro chamador então abre pauta depois do apagamento; não há arquivo que permita um fechamento seguinte (`bin/deploy.sh:368`). É interleaving permitido pelo código; ocorrência em prod [nao verificado].

- **Fila — erro de leitura vira apagamento.** `_tira_recusa` não verifica o resultado de `head|tr|sed` (`bin/deploy.sh:369`); ID vazio pula o resolver e chega ao `rm` (`bin/deploy.sh:370-375`). Arquivo ilegível/malformado pode perder a testemunha sem fechar pauta. O produtor normal escreve via `.novo` e `mv` (`bin/deploy.sh:286-287`), portanto corrupção real [nao verificado].

- **Fila — corrida no mesmo ID também alcança a nova limpeza.** `_tira_recusa` lê ID, executa um comando externo e depois remove o pathname sem confirmar que é o mesmo arquivo (`bin/deploy.sh:369-375`). Uma recusa nova do mesmo ID escrita nesse intervalo (`bin/deploy.sh:286-287`) pode ser apagada. Não há trava envolvendo essa sequência (`bin/deploy.sh:366-383`).

Mapa de recusas e pautas:

| Estado | Escreve/cria | Lê | Apaga/fecha |
|---|---|---|---|
| `recusa_cron.<id>.pendente` | `bin/deploy.sh:286-287`, exclusivamente no ramo restart rc 75 (`:284`) | agendado valida primeiro campo (`bin/deploy_agendado.sh:83-87`), lê argv (`:101`); auto-revert lê arquivo fixo (`app/core/esteira_vigia.py:1344-1354`); contador conta filenames (`bin/deploy_recusado.sh:22-27`); limpeza lê ID/mtime (`bin/deploy.sh:369`, `:382`) | deploy via `_tira_recusa` (`bin/deploy.sh:375`, `:377-382`); **também agendado**, antes de iniciar IDs próprios (`bin/deploy_agendado.sh:95`, `:104`) e substituindo recusa anterior (`:110`) |
| `deploy_recusado:integrador` | `app/core/integrador_lote.py:780` → `_pauta` em `app/core/esteira_vigia.py:900-905` | `pauta_viva`, `app/core/management/commands/alarme_esteira.py:13-15` | comando resolver disparado por `_tira_recusa`, `bin/deploy.sh:370-375` |
| `deploy_recusado:auto_revert` | `app/core/esteira_vigia.py:1331`, `:1356` | mesmo leitor acima | mesmo fechamento acima |
| `deploy_recusado:vigia_logins` | `bin/vigia_logins.sh:94-95` | mesmo leitor acima | mesmo fechamento acima |

O comando não reescreve pauta viva (`app/core/management/commands/alarme_esteira.py:38-41`), cria por `pautas.services.escrever` (`:54-56`) e fecha pela porta `marcar_feita_pelo_sistema` (`:27-31`; escrita efetiva em `app/pautas/services.py:269-275`). IDs `manual`/`agendado.*` podem gerar recusas (`bin/deploy.sh:52-54`; `bin/deploy_agendado.sh:94-105`), mas nenhum dos três abridores usa essas chaves (`app/core/integrador_lote.py:780`; `app/core/esteira_vigia.py:1331`, `:1356`; `bin/vigia_logins.sh:95`). A chave antiga sem sufixo não tem abridor/fechador automático específico no código atual; existência em prod [nao verificado].

RCs dos chamadores encontrados:

| Chamador | Tratamento |
|---|---|
| integrador, reload inicial | Resultado descartado; segue para suite (`app/core/integrador_lote.py:638-639`). |
| integrador, publicação final | Lê rc (`app/core/integrador_lote.py:771-782`); impede `FIM` em falha (`:748-752`). |
| auto-revert, primeiro deploy | Lê rc e para antes dos testes (`app/core/esteira_vigia.py:1276-1284`); pauta/relato em falha (`:1325-1334`). |
| auto-revert, desfazer revert | Lê rc e põe no texto da pauta `auto_revert` (`app/core/esteira_vigia.py:1289-1294`); pauta já viva pode engolir texto (`app/core/management/commands/alarme_esteira.py:38-41`). Não fornece explicitamente `DEPLOY_RECUSA_ID`, herdando ambiente/default `manual` (`app/core/esteira_vigia.py:1289-1290`; `bin/deploy.sh:52`). |
| retentar | Lê rc (`app/core/esteira_vigia.py:1354-1362`); CLI sempre retorna 0 (`:1652-1654`); `vigia_arvore` usa pipeline `tail -1` (`bin/vigia_arvore.sh:28`). |
| vigia_logins | Lê rc e abre pauta em falha (`bin/vigia_logins.sh:90-96`); seu próprio rc final é 2 (`:105`). |
| deploy_agendado | Primeira esteira rc capturado (`bin/deploy_agendado.sh:96-99`); repetição rc capturado/validado (`:105-120`); sucesso repetido vira 3 por cauda não executada (`:123-124`); retorna `PIOR` (`:126`, `:135`). |
| molde_fatia | Reload inicial guardado (`bin/molde_fatia/cadeia.sh:48`); rollback roda com `set +e` e ignora rc (`:26`, `:32-33`); deploy final sujeito a `set -e` (`:4`, `:89`), depois exige texto OK (`:90`). |
| molde_autocura | Equivalente: `bin/molde_autocura/cadeia.sh:3`, `:25`, `:31-32`, `:45`, `:86-87`. |
| fatia `o135-teto20` | `--conferir` guardado (`fatias_agendadas/o135-teto20/esteira.sh:80`); publicação captura e propaga rc (`:116-124`). |
| fatia `abono-no-ar` | Pipeline protegido por `pipefail` e condicional, traduz falha em 1 (`fatias_agendadas/abono-no-ar/esteira.sh:21`, `:37-38`). |
| reload diário RAW | Declarado em `app/config/crons.py:307-308`; execução genérica pelo cron wrapper registra/trata rc e sai com ele (`bin/cron_run.sh:64-70`). |

**RC do resolver:** qualquer não zero do `em_casca`/comando entra no `if !`, gera aviso, preserva arquivo e devolve 1 (`bin/deploy.sh:370-373`). Esse 1 **não é propagado como rc do deploy**: chamadas em `:377`/`:381` não condicionam o resultado final, e o último `echo` devolve 0 (`:385`). Assim `retentar_deploy` pode anunciar “no ar” e deixar recusa/pauta pendentes (`app/core/esteira_vigia.py:1354-1362`; `bin/deploy.sh:370-385`). O restart pode de fato ter passado; o rc 0 não prova que a limpeza fechou.

O segundo apagador `deploy_agendado` já pertence ao diff total fornecido, não ao delta3; sua existência contradiz o comentário “ninguém mais apaga” (`bin/deploy_recusado.sh:6-8` versus `bin/deploy_agendado.sh:110`). Não identifiquei novo escritor direto de `feito_em` na volta 3: fechamento continua pela porta `app/pautas/services.py:256-275`.

## 04/11 11:27 · sessao 9fcf87ff9ab8 · - · - · origem=Ronald (tmux)

**BLOQUEIA — transferência sem fd8 permite restart com exec vivo.** `adm_marcar` solta fd8 antes do comando (`bin/admissao_cron.sh:250`); a transferência publica outra marca e remove a anterior sem retomar a trava (`bin/em_casca.sh:32-36`). A varredura expande o glob uma vez (`bin/admissao_cron.sh:202`): se ele contém apenas a velha e a transferência ocorre antes do teste `-e`, a velha é pulada (`bin/admissao_cron.sh:203`) e a nova não pertence àquela enumeração. Isso também vale para a varredura final sob fd8 (`bin/reinicio_sob_admissao.sh:43-45`), permitindo alcançar o restart com cliente docker vivo (`bin/reinicio_sob_admissao.sh:66`). O comentário “nunca vê zero” não se sustenta (`bin/em_casca.sh:31`).

**Caminhos do item 2:**

- **Filho acaba antes da leitura do starttime:** se `/proc` já desapareceu, `adm_starttime` falha (`bin/admissao_cron.sh:84-91`); não há transferência, `wait` recolhe o rc e EXIT remove a marca caso aquele PID não exista (`bin/em_casca.sh:23`, `bin/em_casca.sh:32`, `bin/em_casca.sh:41-42`). O caminho normal devolve o rc do comando; não há exec vivo nesse intervalo.
- **Falha de sed/mv:** mantém somente a marca do wrapper (`bin/em_casca.sh:34-38`). Enquanto o wrapper vive, ela segura; se ele morrer antes do filho, o reaper a considera órfã pelo PID do wrapper e a apaga (`bin/admissao_cron.sh:169-175`, `bin/admissao_cron.sh:208`), embora o cliente ainda possa viver. **Bloqueia**, pois a cura de SIGKILL não cobre o caminho de falha.
- **SIGTERM no wrapper:** não há trap TERM, apenas EXIT (`bin/em_casca.sh:23`). Depois da transferência, EXIT preserva a marca enquanto `kill -0` do filho responde; antes dela ou após falha de transferência, preserva arquivo cujo PID ainda é do wrapper (`bin/em_casca.sh:23`, `bin/em_casca.sh:34-38`), e o reaper o remove após a morte. O status observado pelo chamador é o de término do wrapper, não o resultado futuro do comando; ocorrência de produção **[nao verificado]**.
- **SIGKILL no wrapper:** depois da transferência concluída, a marca identifica o filho e continua válida por PID/starttime/exe (`bin/em_casca.sh:33-36`, `bin/admissao_cron.sh:169-177`). Antes da transferência, ou no fallback sed/mv, a marca morre junto com o wrapper para o leitor, embora o filho tenha sido iniciado (`bin/em_casca.sh:28`, `bin/em_casca.sh:32-38`). **Bloqueia**.
- **PID do filho reutilizado:** o EXIT usa somente `kill -0`, sem comparar starttime (`bin/em_casca.sh:23`); pode deixar arquivo após o comando acabar. O reaper distingue reutilização comparando starttime (`bin/admissao_cron.sh:175`). Se a reutilização ocorrer *antes* da captura de starttime, pode registrar a identidade do processo substituto; `exe` é apenas substring da cmdline, e não `/proc/PID/exe` (`bin/em_casca.sh:32-34`, `bin/admissao_cron.sh:176-177`). **Fila**, para retenção espúria; ocorrência concreta **[nao verificado]**.
- **rc64 de uso/nome:** sai antes da admissão (`bin/em_casca.sh:12-13`); alvo fora das cascas sai depois de marcar, mas antes de iniciar o comando, e EXIT desmarca porque `_c` está vazio (`bin/em_casca.sh:21-28`). Sem execução escondida nesse caminho.
- **Admissão 75/70:** não chega ao lançamento do comando (`bin/em_casca.sh:16-19`, `bin/em_casca.sh:28`). O 75 nasce da espera pela trava e o 70 da criação/abertura/starttime/publicação (`bin/admissao_cron.sh:241-248`).
- **Marca nova literalmente depois de uma verificação vazia:** é uma interleaving possível se o wrapper for morto após lançar o `mv`, mas antes de o filho `mv` publicar: o reaper pode remover a marca velha pelo wrapper morto, enquanto `mv` ainda publica sem fd8 (`bin/em_casca.sh:35`, `bin/admissao_cron.sh:170`, `bin/admissao_cron.sh:208`). Dedução de concorrência; reprodução e ocorrência de produção **[nao verificado]**. A corrida do glob acima já prova o defeito sem sinais.

**Quem escreve/lê as marcas:**

- Porta original: `adm_marcar` escreve `${nome}_${emp}.$$`, com `ini/pid/starttime/exe/casca/cmd`, por temporário e `mv` sob fd8 (`bin/admissao_cron.sh:239-250`).
- Chamadores da porta: `cron_run.sh` para comando e alarme (`bin/cron_run.sh:16`, `bin/cron_run.sh:52`); descoberta da corrente (`bin/encadeado.sh:48`); `em_casca` (`bin/em_casca.sh:16`).
- **Segundo escritor novo:** `em_casca` escreve `${N}_-.${filho}`, altera pid/starttime/exe, conserva os demais campos e remove a marca anterior (`bin/em_casca.sh:33-36`).
- Leitor de identidade: `adm_marca_viva` toma PID **do nome do arquivo**, não do campo `pid=`, e lê starttime/exe (`bin/admissao_cron.sh:169-177`).
- Leitor de proteção: `adm_segura` lê `casca`; formato antigo também segura; casca=0 ainda consulta a árvore viva (`bin/admissao_cron.sh:181-186`).
- Leitor/reaper: `adm_varrer` aplica identidade, exclui ancestrais da proteção, chama `adm_segura` e apaga órfãs (`bin/admissao_cron.sh:199-209`).
- Consumidor efetivo: restart consulta `adm_seguram` antes e depois de tomar fd8 (`bin/reinicio_sob_admissao.sh:40-45`); `adm_seguram` filtra `adm_varrer` (`bin/admissao_cron.sh:213`).
- Remoção explícita: `adm_desmarcar` (`bin/admissao_cron.sh:254`), usada por EXIT/comando e alarme do cron (`bin/cron_run.sh:22`, `bin/cron_run.sh:51`, `bin/cron_run.sh:58`), descoberta da corrente (`bin/encadeado.sh:50`) e EXIT do `em_casca` (`bin/em_casca.sh:23`).
- Contrato apenas verifica presença textual da infraestrutura; não lê marcas de execução (`bin/contrato_deploy.sh:55-57`). Selos usam raízes de teste e leem `adm_varrer` (`bin/tests/test_deploy_nao_mata_elo.sh:635`, `bin/tests/test_deploy_nao_mata_elo.sh:675`).

A cura da volta 3 cobre o cenário **já transferido antes do SIGKILL**, mas introduz o segundo escritor sem fd8 e deixa janelas anteriores/fallback abertas (`bin/em_casca.sh:28-38`).

## 04/11 11:08 · sessao 74fc7acb1b74 · DEPLOY-[nome]-ELO · verificador · origem=Code

1. **Os PROCEDENTES tiveram cura parcial. Eu não daria fechamento: permanecem caminhos sem garantia contra restart sobre exec vivo, proibido pelo aval.** A serialização cobre o restart, enquanto outros execs do deploy ficam fora dela; o wrapper também pode perder sua marca após timeout (`bin/reinicio_sob_admissao.sh:43`, `bin/deploy.sh:124`, `bin/em_casca.sh:21`, `bin/admissao_cron.sh:208`; `AVAL_DEPLOY-ELO-B3.txt:13`, anexo).

   Referências aos anexos abaixo usam a numeração das linhas do texto fornecido. Suíte executada nesta cópia: **[não verificado]**. Não executei o selo porque ele cria e remove arquivos (`bin/tests/test_deploy_nao_mata_elo.sh:176`, `bin/tests/test_deploy_nao_mata_elo.sh:193`).

   | PROCEDENTE da triagem | Verificação em 4f7035a3 |
   |---|---|
   | Recusa apagada antes da nova tentativa | **Curado no cenário apontado:** `_rec_ant` preserva a anterior quando a tentativa dá rc1 sem arquivo novo; o caso 8 exige essa preservação. A autorização para substituir por recusa nova verifica apenas existência, ressalva de **fila** detalhada no item 6 (`bin/deploy_agendado.sh:102`, `bin/deploy_agendado.sh:110`, `bin/tests/test_deploy_nao_mata_elo.sh:224`). |
   | `_pauta` ignorava rc | **Parcial:** agora devolve rc e escreve diagnóstico; três chamadores consomem esse retorno. Os demais, inclusive a resolução e a pauta do revert do revert, continuam descartando-o. **Fila** para os consumidores restantes (`app/core/esteira_vigia.py:902`, `app/core/esteira_vigia.py:905`, `app/core/esteira_vigia.py:1291`, `app/core/esteira_vigia.py:1364`; censo no item 3). |
   | Revert do revert ignorava rc do deploy | **Leitura curada; comunicação parcial:** `rrd` entra no texto da pauta, mas seu envio pode falhar ou encontrar pauta já viva; o relato e o retorno não carregam o rc. **Fila** (`app/core/esteira_vigia.py:1289`, `app/core/esteira_vigia.py:1293`, `app/core/esteira_vigia.py:1295`, `app/core/esteira_vigia.py:1298`, `app/core/management/commands/alarme_esteira.py:38`). |
   | Chave única engolia pauta de outro chamador | **Curado entre os três novos motivos**, mas o fechamento continua condicionado ao sucesso de um retry do auto-revert. Há também **regressão de compatibilidade, fila:** a chave antiga perde seu resolvedor automático (`app/core/integrador_lote.py:780`, `bin/vigia_logins.sh:95`, `app/core/esteira_vigia.py:1332`, `app/core/esteira_vigia.py:1349`, `app/core/esteira_vigia.py:1364`). |
   | 19: `exit 0` sobrevivia | **Curado para esse mutante:** agora exige rc3 do comando e desmarcação (`bin/tests/test_deploy_nao_mata_elo.sh:660`, `bin/tests/test_deploy_nao_mata_elo.sh:661`). |
   | 21: limpeza provada só por grep | **Curado quanto ao bloco de limpeza:** ele é executado. Ainda não prova a criação real do marcador; lacuna de **fila** (`bin/tests/test_deploy_nao_mata_elo.sh:734`, `bin/tests/test_deploy_nao_mata_elo.sh:738`, `bin/tests/test_deploy_nao_mata_elo.sh:740`). |
   | 23: CLI não exercitada | **Parcial:** passou de grep para AST, mas continua sem executar a CLI e aceita comparação invertida. **Fila de mordida** (`bin/tests/test_deploy_nao_mata_elo.sh:904`, `bin/tests/test_deploy_nao_mata_elo.sh:905`; `app/core/esteira_vigia.py:1657`). |
   | 20: cron sem duração excluído calado | **Curado para a fonte viva considerada pelo selo:** informa ausentes e exige zero nesse caminho; lacunas da cópia são apenas informadas. O dado fornecido declara 13 lacunas na cópia e nenhuma no vivo; não houve nova medição de produção (`bin/tests/test_deploy_nao_mata_elo.sh:699`, `bin/tests/test_deploy_nao_mata_elo.sh:707`; `triagem_v1.txt:11`, anexo). |

2. **FILA/DESCARTADO: duas justificativas não bastam para fechamento diante do PROIBIDO escrito; outra contém afirmação factual incorreta.** O aval não escreve exceção para DEPLOY × DEPLOY ou morte do wrapper (`AVAL_DEPLOY-ELO-B3.txt:13`, anexo). [nao verificado]

   | Item triado | O motivo sustenta-se? Gravidade |
   |---|---|
   | Execs internos do deploy: “DEPLOY × DEPLOY, fora do aval” | O diagnóstico técnico sustenta-se; a dispensa, **não**. Migrate, collectstatic e prova de rota executam sem marca própria, enquanto o mutex cobre somente o restart. Dois deploys podem sobrepor esses trechos. **Bloqueia pelo PROIBIDO**, embora não seja introdução do delta da volta 2; ocorrência em produção **[não verificado]** (`bin/deploy.sh:124`, `bin/deploy.sh:205`, `bin/deploy.sh:316`, `bin/reinicio_sob_admissao.sh:43`, `bin/reinicio_sob_admissao.sh:73`). |
   | Timeout/SIGKILL deixa docker sem marca | “SIGKILL não se trapa” explica a limitação, mas não a cura. Há timeout sobre o wrapper; ele só trata EXIT, identifica sua própria vida e roda docker como filho. Morto o wrapper, o reaper pode apagar sua marca sem comprovar término do exec. **Bloqueia a garantia exigida por MUDA 1/PROIBIDO**; sobrevivência concreta em produção **[não verificado]** (`eval/harness.py:56`, `eval/juiz.py:245`, `bin/em_casca.sh:21`, `bin/em_casca.sh:26`, `bin/admissao_cron.sh:175`, `bin/admissao_cron.sh:208`; `AVAL_DEPLOY-ELO-B3.txt:6`, `:13`, anexo). |
   | Marca casca=0 antes do rc64 | Sustenta-se: a validação vem após marcar, mas antes de executar; EXIT desmarca. **Fila**, sem execução indevida demonstrada (`bin/em_casca.sh:16`, `bin/em_casca.sh:21`, `bin/em_casca.sh:22`, `bin/em_casca.sh:26`). |
   | Marca sobra após morte sem EXIT | **DESCARTA sustentado quanto à limpeza da marca:** PID/starttime/cmdline são conferidos e órfãs são removidas. Isso não resolve o filho órfão do item anterior (`bin/admissao_cron.sh:167`, `bin/admissao_cron.sh:175`, `bin/admissao_cron.sh:177`, `bin/admissao_cron.sh:208`). |
   | Reload inicial do integrador ignora rc | Sustenta-se: não guarda retorno e segue à suíte. **Fila**; não é o deploy final do lote tratado por MUDA 3 (`app/core/integrador_lote.py:638`, `app/core/integrador_lote.py:639`, `app/core/integrador_lote.py:748`). |
   | Rollback dos moldes ignora rc | Sustenta-se: executa reload e anuncia “recarregada” sem conferir sucesso. **Fila**, fora do delta fornecido (`bin/molde_fatia/cadeia.sh:32`, `bin/molde_fatia/cadeia.sh:33`, `bin/molde_autocura/cadeia.sh:31`, `bin/molde_autocura/cadeia.sh:32`). |
   | fd7 liberado cedo | **DESCARTA como defeito atual sustentado:** o pai conserva fd7; `7>&-` fecha nos filhos. **Fila** para ampliar mordida do teste (`bin/vigia_logins.sh:70`, `bin/vigia_logins.sh:71`, `bin/vigia_logins.sh:90`, `bin/vigia_logins.sh:95`). |
   | Corte por mtime nasce antes da espera | Sustenta-se para o cenário descrito: recusa produzida durante a espera pode sobreviver ao restart que a superou. É **supercontagem, fila** (`bin/deploy.sh:282`, `bin/deploy.sh:283`, `bin/deploy.sh:367`). |
   | Mesmo id concorrente disputa `.novo` | Sustenta-se: temporário e destino derivam só do id. As travas dos chamadores não constituem mutex comum de deploy inteiro. **Fila**, ligada ao primeiro bloqueio (`bin/deploy.sh:54`, `bin/deploy.sh:286`, `bin/integrador.sh:9`, `bin/vigia_arvore.sh:23`, `bin/vigia_logins.sh:70`). |
   | CLI retorna 0 / `tail` perde rc | Sustenta-se: a CLI imprime o texto e retorna 0; o vigia não decide pelo rc. **Fila** para esse contrato, sem provar sucesso da resolução (`app/core/esteira_vigia.py:1658`, `app/core/esteira_vigia.py:1659`, `bin/vigia_arvore.sh:28`). |
   | “Cascas não reiniciaram” para qualquer rc1 | A objeção procede: rc1 também pode acontecer depois do restart, na prova de rota ou selo. **Fila de comunicação**, pois o estado afirmado pode estar errado (`app/core/esteira_vigia.py:1329`, `bin/deploy.sh:283`, `bin/deploy.sh:337`, `bin/deploy.sh:344`). |
   | Retry não refaz conferência/marcação do auto-revert | Sustenta-se. O sucesso resolve pautas e relata; não volta aos testes nem à marcação das fatias. **Fila**: MUDA 5 exige retry automático, mas não explicita essa continuação (`app/core/esteira_vigia.py:1283`, `app/core/esteira_vigia.py:1303`, `app/core/esteira_vigia.py:1362`, `app/core/esteira_vigia.py:1367`; `AVAL_DEPLOY-ELO-B3.txt:10`, anexo). |
   | argv perde campo vazio | Sustenta-se; o produtor atual passa argumentos não vazios. **Fila** (`app/core/esteira_vigia.py:1353`, `app/core/esteira_vigia.py:1276`). |
   | Cabeçalho 3300 → 1586 | Correção conferida contra o JSON da cópia (`bin/admissao_cron.sh:31`, `app/config/crons_duracao.json:82`). |
   | Env de admissão “só encurta” | **Motivo parcialmente falso:** o valor é aceito sem limite superior e passado ao `flock`; também pode aumentar a espera. É verdadeiro que não altera o teto do restart. **Fila**, preexistente no delta (`bin/admissao_cron.sh:50`, `bin/admissao_cron.sh:243`, `bin/admissao_cron.sh:58`, `bin/reinicio_sob_admissao.sh:18`). |

   **CONFERIR:** no harness, o HTTP está dentro de `_TIRO`, executado pelo docker exec protegido; não é HTTP direto do host nessa função. O código atende à marca nesse trecho. Execução da conferência e eventual linha no RELATO: **[não verificado]** (`eval/harness.py:14`, `eval/harness.py:18`, `eval/harness.py:24`, `eval/harness.py:54`; `AVAL_DEPLOY-ELO-B3.txt:11`, anexo).

3. **Censo de escritores e leitores.**

   **Motivos e tupla:**

   | Elemento | Quem escreve | Quem lê |
   |---|---|---|
   | `deploy_recusado:auto_revert` | Abertura em `_deploy_do_revert` e na falha do retry (`app/core/esteira_vigia.py:1332`, `app/core/esteira_vigia.py:1357`). | Resolução pelo loop dos ids (`app/core/esteira_vigia.py:1364`). |
   | `deploy_recusado:integrador` | `_deploy_do_lote` (`app/core/integrador_lote.py:780`). | Mesmo loop (`app/core/esteira_vigia.py:1364`). |
   | `deploy_recusado:vigia_logins` | Shell do vigia (`bin/vigia_logins.sh:95`). | Mesmo loop (`app/core/esteira_vigia.py:1364`). |
   | `DEPLOY_RECUSA_IDS` | Uma definição literal (`app/core/esteira_vigia.py:1316`). | Um consumidor: universo do loop de resolução (`app/core/esteira_vigia.py:1362`). |
   | Estado efetivo da pauta | O comando delega abertura/fechamento aos serviços (`app/core/management/commands/alarme_esteira.py:27`, `app/core/management/commands/alarme_esteira.py:31`, `app/core/management/commands/alarme_esteira.py:54`). | A localização da pauta viva usa prefixo do texto; pauta já viva impede reescrever o conteúdo (`app/core/management/commands/alarme_esteira.py:15`, `app/core/management/commands/alarme_esteira.py:38`). |

   **Chave antiga — regressão, fila:** não resta produtor ou resolvedor automático específico de `deploy_recusado` sem sufixo. A porta genérica ainda aceita essa chave, mas os novos resolvedores procuram `ESTEIRA deploy_recusado:<id>:` e não encontram o texto antigo `ESTEIRA deploy_recusado: ...`. As ações genéricas atuais também não produzem essa resolução (`app/core/management/commands/alarme_esteira.py:15`, `app/core/management/commands/alarme_esteira.py:24`, `app/core/esteira_vigia.py:1364`, `app/core/esteira_vigia.py:490`, `app/core/esteira_vigia.py:504`, `app/core/esteira_vigia.py:530`, `app/core/esteira_vigia.py:597`). Pauta antiga viva em produção: **[não verificado]**.

   **Rc de `_pauta`:** nasce do subprocess, é diagnosticado centralmente e devolvido (`app/core/esteira_vigia.py:900`, `app/core/esteira_vigia.py:902`, `app/core/esteira_vigia.py:905`).

   | Chamador | Retorno lido? |
   |---|---|
   | `_deploy_do_lote` | **Sim:** acrescenta falha ao relato (`app/core/integrador_lote.py:780`, `app/core/integrador_lote.py:781`). |
   | `_deploy_do_revert` | **Sim:** acrescenta falha ao relato (`app/core/esteira_vigia.py:1332`, `app/core/esteira_vigia.py:1334`). |
   | `retentar_deploy`, abertura | **Sim:** acrescenta falha à string devolvida, impressa pela CLI (`app/core/esteira_vigia.py:1357`, `app/core/esteira_vigia.py:1359`, `app/core/esteira_vigia.py:1658`). |
   | `_relance_nao_pegou` | **Não** (`app/core/esteira_vigia.py:959`). |
   | `curar_zumbido` | **Não** (`app/core/esteira_vigia.py:1179`). |
   | `conferir_quarentena`, prazo e depósito | **Não**, nos dois pontos (`app/core/esteira_vigia.py:1241`, `app/core/esteira_vigia.py:1244`). |
   | `auto_revert`, revert que não aplicou e revert desfeito | **Não**, nos dois pontos (`app/core/esteira_vigia.py:1274`, `app/core/esteira_vigia.py:1291`). |
   | `retentar_deploy`, resolução | **Não** (`app/core/esteira_vigia.py:1364`). |
   | `curar_por_relogio`, quarentena e vermelho | **Não**, nos dois pontos (`app/core/esteira_vigia.py:1398`, `app/core/esteira_vigia.py:1405`). |
   | `passada`, ações genéricas de alarme e resolução | **Não**, nos dois pontos (`app/core/esteira_vigia.py:1637`, `app/core/esteira_vigia.py:1642`). |

   **Pendentes `recusa_cron.<id>.pendente`:**

   | Papel | Sítios |
   |---|---|
   | Criar/sobrescrever em produção | `deploy.sh` escreve `.novo` e faz `mv`, exclusivamente no ramo rc75 (`bin/deploy.sh:286`, `bin/deploy.sh:287`, `bin/deploy.sh:290`). |
   | Apagar em produção | Próprio id no fim do deploy; recusas anteriores pelo corte; nomes de tentativas do agendado antes de iniciar; recusa anterior substituída pelo agendado (`bin/deploy.sh:363`, `bin/deploy.sh:367`, `bin/deploy_agendado.sh:95`, `bin/deploy_agendado.sh:104`, `bin/deploy_agendado.sh:110`). |
   | `_rec_ant` | Recebe o caminho anterior; seu único consumidor é o `rm` condicionado a rc0 ou rc75 com arquivo novo (`bin/deploy_agendado.sh:102`, `bin/deploy_agendado.sh:110`). |
   | Ler conteúdo | Agendado valida id e recupera argv; retry lê somente o arquivo do auto-revert (`bin/deploy_agendado.sh:83`, `bin/deploy_agendado.sh:101`, `app/core/esteira_vigia.py:1345`, `app/core/esteira_vigia.py:1348`). |
   | Ler existência/nomes | Loop de resolução e contador; placar consome o contador e soma ao TOTAL (`app/core/esteira_vigia.py:1363`, `bin/deploy_recusado.sh:22`, `bin/deploy_recusado.sh:25`, `bin/placar_code.sh:464`, `bin/placar_code.sh:542`). |
   | Criar/apagar em testes | Fixtures dos casos 8, 21 e 23, além dos blocos extraídos de produção (`bin/tests/test_deploy_nao_mata_elo.sh:176`, `bin/tests/test_deploy_nao_mata_elo.sh:189`, `bin/tests/test_deploy_nao_mata_elo.sh:238`, `bin/tests/test_deploy_nao_mata_elo.sh:716`, `bin/tests/test_deploy_nao_mata_elo.sh:736`, `bin/tests/test_deploy_nao_mata_elo.sh:858`, `bin/tests/test_deploy_nao_mata_elo.sh:870`, `bin/tests/test_deploy_nao_mata_elo.sh:879`). |

   **Rc do deploy do revert do revert:** o único consumidor de `rrd` é a composição do texto enviado à pauta; não controla retry, relato ou retorno. Esse segundo deploy não fixa o id `auto_revert`: herda o ambiente ou usa `manual`, cujo pendente não é lido pelo retentador automático. **Fila** (`app/core/esteira_vigia.py:1289`, `app/core/esteira_vigia.py:1293`, `app/core/esteira_vigia.py:1295`, `app/core/esteira_vigia.py:1298`, `bin/deploy.sh:52`, `app/core/esteira_vigia.py:1345`).

4. **Os casos ganharam mordida, mas não provam toda a cura anunciada.**

   | Caso | Mordida e mutação sobrevivente |
   |---|---|
   | 8, rc1 | Mata a exclusão incondicional da recusa anterior. Entretanto, remover a exclusão de `_rec_ant` inteiramente deixa acumular recusas substituídas: a persistência exige existência, sem exigir quantidade. **Fila de cobertura** (`bin/deploy_agendado.sh:110`, `bin/tests/test_deploy_nao_mata_elo.sh:207`, `bin/tests/test_deploy_nao_mata_elo.sh:224`). |
   | 19(e) | Mata `exit 0`: exige rc3 e nenhuma marca restante. **Cura específica sustentada** (`bin/em_casca.sh:27`, `bin/tests/test_deploy_nao_mata_elo.sh:660`, `bin/tests/test_deploy_nao_mata_elo.sh:661`). |
   | 21, limpeza | Mata alterações do bloco que preservem a velha, apaguem a nova ou deixem marcador. **Sobrevive apagar a criação real do marcador:** o próprio teste o fabrica. **Fila** (`bin/deploy.sh:282`, `bin/deploy.sh:366`, `bin/tests/test_deploy_nao_mata_elo.sh:738`, `bin/tests/test_deploy_nao_mata_elo.sh:740`). |
   | 21, chave do lote | Mata voltar à chave única. Porém retirar “PAUTA NÃO ABRIU” do relato passa: `_pauta` foi substituída por coletor que retorna `None`, sem cenário de falha. **Fila** (`bin/tests/test_deploy_nao_mata_elo.sh:750`, `bin/tests/test_deploy_nao_mata_elo.sh:808`, `app/core/integrador_lote.py:781`). |
   | 21, revert do revert | Mata ignorar completamente o rc, mas só injeta 75. Alterar a composição para anunciar apenas rc75, silenciando rc1, passa. **Fila** (`bin/tests/test_deploy_nao_mata_elo.sh:796`, `bin/tests/test_deploy_nao_mata_elo.sh:813`, `app/core/esteira_vigia.py:1293`). |
   | 20, lacuna | Detecta ausência na fonte viva dentro do universo construído. Contudo, retirar um cron desse universo pelo classificador também o retira da checagem; só dois nomes têm presença explicitamente exigida. Mutante completo executado: **[não verificado]**. **Fila de independência da prova** (`bin/tests/test_deploy_nao_mata_elo.sh:689`, `bin/tests/test_deploy_nao_mata_elo.sh:699`, `bin/tests/test_deploy_nao_mata_elo.sh:707`, `bin/tests/test_deploy_nao_mata_elo.sh:711`). |
   | 23, CLI | **Sobrevivente confirmado em memória:** trocar `==` por `!=` mantém a aceitação da AST, mas impede atender o comando correto. O teste verifica string e chamada, sem verificar o operador. **Fila** (`app/core/esteira_vigia.py:1657`, `bin/tests/test_deploy_nao_mata_elo.sh:904`, `bin/tests/test_deploy_nao_mata_elo.sh:905`). |
   | 23, resolução por id | Mata resolver o integrador com sua recusa ainda presente. Não testa falha do resolvedor, cura por outro deploy nem migração da chave velha; `_pauta` é coletor. **Fila** (`bin/tests/test_deploy_nao_mata_elo.sh:865`, `bin/tests/test_deploy_nao_mata_elo.sh:878`, `bin/tests/test_deploy_nao_mata_elo.sh:888`). |

   Essas lacunas não tornam todos os REDs inúteis: existem mutantes específicos mortos. Impedem, porém, tratar AST como execução da CLI ou chamada ao coletor como resolução efetiva de pauta (`bin/tests/test_deploy_nao_mata_elo.sh:661`, `bin/tests/test_deploy_nao_mata_elo.sh:808`, `bin/tests/test_deploy_nao_mata_elo.sh:865`, `bin/tests/test_deploy_nao_mata_elo.sh:904`; exigência RED em `AVAL_DEPLOY-ELO-B3.txt:12`, anexo).

5. **Classes da casa no delta da volta 2:**

   | Classe | Resultado |
   |---|---|
   | (a) Segundo juiz registrado | **Não identificado no delta.** A pauta continua passando pelo comando e pelos serviços; o lookup aponta o registro vivo, não constitui outra autoridade (`app/core/management/commands/alarme_esteira.py:27`, `app/docs/JUIZES.md:10`). |
   | (b) Segundo escritor fora da porta | **Presente na gestão das recusas, fila:** o delta mantém no agendado uma exclusão de estado também gerido pelo deploy. A substituição pode ser correta, mas contradiz “ninguém mais apaga” do contador. Para estado ORM de pauta, a porta permanece canônica (`bin/deploy_agendado.sh:110`, `bin/deploy.sh:367`, `bin/deploy_recusado.sh:7`, `app/pautas/services.py:180`, `app/pautas/services.py:272`). |
   | (c) Relógio fora da borda | **Não identifiquei nova leitura de relógio no código de produção do delta.** O corte por mtime já está no diff total e tem a supercontagem descrita acima (`bin/deploy.sh:282`, `bin/deploy.sh:367`; `diff_v6_delta.patch`, anexo). |
   | (d) Derivação/lista paralela | **Sim, fila:** `DEPLOY_RECUSA_IDS` enumera separadamente os mesmos ids escritos pelos produtores; eles não consultam a tupla (`app/core/esteira_vigia.py:1316`, `app/core/esteira_vigia.py:1325`, `app/core/integrador_lote.py:771`, `bin/vigia_logins.sh:90`). |
   | (e) RED sem mordida | **Mordida parcial, fila**, especialmente CLI por AST e limpeza com marcador fabricado; não são provas integrais dos caminhos anunciados (`bin/tests/test_deploy_nao_mata_elo.sh:738`, `bin/tests/test_deploy_nao_mata_elo.sh:904`). |
   | (f) Ordem de trava | Não demonstrada nova inversão no delta; o fd7 continua pertencendo ao pai. **[não verificado]** quanto à ausência de toda corrida possível (`bin/vigia_logins.sh:70`, `bin/vigia_logins.sh:90`). |
   | (g) Pouso antes do pré-requisito | O FIM do lote está depois da guarda de sucesso do deploy. Pouso próprio e janela autorizada: **[não verificado]** (`app/core/integrador_lote.py:748`, `app/core/integrador_lote.py:751`; `AVAL_DEPLOY-ELO-B3.txt:14`, anexo). |
   | (h

[truncado em 20000 de 22609 caracteres]

## 04/11 11:02 · sessao c956083d529c · - · - · origem=Ronald (tmux)

Revisão só leitura das FILA/DESCARTADO:

| Triagem | Resultado e gravidade | Evidência |
|---|---|---|
| DEPLOY × DEPLOY | O diagnóstico técnico sustenta-se: execs do deploy ficam fora da admissão; só o restart é serializado. A justificativa “fora do aval” não elimina a proibição expressa de restart sobre exec vivo. **Bloqueia pelo PROIBIDO do aval**, se aplicado literalmente; ocorrência em produção [não verificado]. | `bin/deploy.sh:102`, `bin/deploy.sh:124`, `bin/deploy.sh:205`, `bin/deploy.sh:315`; `bin/reinicio_sob_admissao.sh:28`, `bin/reinicio_sob_admissao.sh:43`, `bin/reinicio_sob_admissao.sh:66`; novo caminho `app/core/esteira_vigia.py:1355`, `bin/vigia_arvore.sh:28`. |
| Wrapper morto por timeout/SIGKILL | Só há trap EXIT e a marca identifica o wrapper, enquanto docker roda como filho. Morto o wrapper, o reaper elimina a marca; falta garantia de proteção do filho órfão. “SIGKILL não se trapa” explica a falha, mas não satisfaz a proibição de restart sobre exec vivo. **Bloqueia pelo PROIBIDO do aval**; sobrevivência concreta em produção [não verificado]. | `bin/em_casca.sh:21`, `bin/em_casca.sh:26`; `bin/admissao_cron.sh:167`, `bin/admissao_cron.sh:175`, `bin/admissao_cron.sh:208`; timeouts em `eval/harness.py:56`, `eval/gerador.py:66`, `eval/juiz.py:245`. |
| Marca casca=0 antes de rc64 | Sustenta-se: marca nasce antes da validação; o comando não roda e EXIT desmarca. **Fila**, sem violação operacional demonstrada. | `bin/em_casca.sh:16`, `bin/em_casca.sh:21`, `bin/em_casca.sh:22`, `bin/em_casca.sh:24`, `bin/em_casca.sh:26`. |
| Marca órfã após morte sem EXIT | DESCARTA sustenta-se quanto à remoção: pid/starttime/cmdline são conferidos e a marca órfã é apagada. Isso não cura o filho órfão do item anterior. | `bin/admissao_cron.sh:167`, `bin/admissao_cron.sh:175`, `bin/admissao_cron.sh:177`, `bin/admissao_cron.sh:208`. |
| Reload inicial integrador ignora rc | Sustenta-se; segue imediatamente à suite. **Fila**, preexistência consta do diff fornecido. | `app/core/integrador_lote.py:638`, `app/core/integrador_lote.py:639`. |
| Rollback dos moldes ignora rc | Sustenta-se: comando de reload seguido de mensagem de sucesso sem guarda de retorno. **Fila**, fora do delta fornecido. | `bin/molde_fatia/cadeia.sh:25`, `bin/molde_fatia/cadeia.sh:32`, `bin/molde_fatia/cadeia.sh:33`; `bin/molde_autocura/cadeia.sh:24`, `bin/molde_autocura/cadeia.sh:31`, `bin/molde_autocura/cadeia.sh:32`. |
| fd7 liberado cedo | DESCARTA como defeito atual sustenta-se: pai mantém fd7; `7>&-` fecha apenas nos filhos. **Fila** para mordida do selo. | `bin/vigia_logins.sh:70`, `bin/vigia_logins.sh:71`, `bin/vigia_logins.sh:90`, `bin/vigia_logins.sh:95`. |
| Corte mtime antes da espera | Sustenta-se no cenário descrito: recusa produzida durante espera fica, embora o restart posterior possa tê-la superado; supercontagem. **Fila**. | `bin/deploy.sh:282`, `bin/deploy.sh:283`, `bin/deploy.sh:366`, `bin/deploy.sh:367`. |
| Mesmo id concorrente disputa `.novo` | Sustenta-se: destino e temporário derivam apenas do id. Chamadores normais possuem travas próprias, mas deploy inteiro não tem mutex comum. **Fila**, ligada à corrida DEPLOY × DEPLOY acima. | `bin/deploy.sh:52`, `bin/deploy.sh:54`, `bin/deploy.sh:286`, `bin/deploy.sh:287`; `bin/integrador.sh:9`, `bin/integrador.sh:10`; `bin/vigia_arvore.sh:23`, `bin/vigia_arvore.sh:24`; `bin/vigia_logins.sh:70`. |
| CLI retorna 0 / tail come rc | Sustenta-se: main retorna 0 e shell usa pipeline sem decidir pelo resultado. **Fila** quanto ao contrato atual; não prova resolução bem-sucedida. | `app/core/esteira_vigia.py:1657`, `app/core/esteira_vigia.py:1658`, `app/core/esteira_vigia.py:1659`; `bin/vigia_arvore.sh:28`. |
| Texto “não reiniciaram” para rc1 | Sustenta-se como defeito: rc1 pode vir depois do restart, inclusive prova de rota. **Fila de texto**, mas o relato pode descrever estado incorreto. | `app/core/esteira_vigia.py:1329`, `app/core/esteira_vigia.py:1331`; `bin/deploy.sh:283`, `bin/deploy.sh:337`, `bin/deploy.sh:344`. |
| Retentar não confere nem marca auto-revert | Sustenta-se: sucesso apenas resolve pautas, relata e retorna; não volta ao bloco de testes/marcação. **Fila**, salvo interpretação mais ampla do fechamento MUDA5. | `app/core/esteira_vigia.py:1283`, `app/core/esteira_vigia.py:1301`; `app/core/esteira_vigia.py:1362`, `app/core/esteira_vigia.py:1365`, `app/core/esteira_vigia.py:1367`. |
| argv perde campos vazios | Sustenta-se; produtor atual passa dois campos não vazios. **Fila**. | `app/core/esteira_vigia.py:1353`, `app/core/esteira_vigia.py:1276`. |
| Cabeçalho 1586 | Correção confere com JSON da cópia. Números vivos permanecem apenas no dado fornecido. | `bin/admissao_cron.sh:31`; `app/config/crons_duracao.json:82`. |
| ADM_ESPERA_ADMISSAO_S “só encurta” | **Motivo parcialmente falso**: env livre também aumenta acima de 900, sem clamp. Verdadeiro que não altera teto do restart. **Fila**; não é alteração do delta. | `bin/admissao_cron.sh:50`, `bin/admissao_cron.sh:243`; teto separado em `bin/admissao_cron.sh:55`, `bin/admissao_cron.sh:58`; `bin/reinicio_sob_admissao.sh:18`, `bin/reinicio_sob_admissao.sh:48`. |

CONFERIR: a rede do harness está dentro da string `_TIRO`, enviada ao `docker exec` que passa pelo wrapper; portanto o código cobre aquele trecho pela marca. Não há chamada HTTP direta do host nessa função. Prova de execução da conferência/linha no RELATO [não verificado]. `eval/harness.py:14`, `eval/harness.py:18`, `eval/harness.py:24`, `eval/harness.py:53`, `eval/harness.py:54`.

Outro detalhe do delta para consolidar: o deploy do revert do revert lê o rc, mas não fixa `DEPLOY_RECUSA_ID=auto_revert`; se recusado, usa id herdado ou `manual`, enquanto o retentador só lê `recusa_cron.auto_revert.pendente`. O rc apenas aparece em pauta `auto_revert`; o retorno dessa `_pauta` continua ignorado. `app/core/esteira_vigia.py:1289`, `app/core/esteira_vigia.py:1291`, `app/core/esteira_vigia.py:1311`, `app/core/esteira_vigia.py:1345`; `bin/deploy.sh:52`.

## 04/11 11:02 · sessao f949e30a8f81 · - · - · origem=Ronald (tmux)

Censo e achados, apenas por leitura:

1. **Pauta por chamador: cura parcial; fechamento incompleto — fila.** Os emissores passam as novas chaves em `app/core/esteira_vigia.py:1332`, `app/core/esteira_vigia.py:1357`, `app/core/integrador_lote.py:780` e `bin/vigia_logins.sh:95`. A resolução automática existe somente no caminho de sucesso de `retentar_deploy`, em `app/core/esteira_vigia.py:1362-1364`. Um deploy terceiro pode apagar a recusa de auto-revert em `bin/deploy.sh:367`; depois disso `retentar_deploy` retorna antes da resolução em `app/core/esteira_vigia.py:1349-1350`. Logo, “fecha quando a recusa já não está de pé” excede o comportamento implementado (`app/core/esteira_vigia.py:1314-1315`).

2. **Falha de resolução fica sem nova tentativa — fila.** `_pauta` devolve rc em `app/core/esteira_vigia.py:905`, mas o loop de resolver ignora esse retorno em `app/core/esteira_vigia.py:1364`. A recusa própria já foi apagada pelo deploy de sucesso (`bin/deploy.sh:363`); a próxima passada retorna “nada pendente” (`app/core/esteira_vigia.py:1349-1350`).

3. **Chave antiga órfã — fila; existência em produção [não verificado].** Não há chamada específica atual com `--resolver deploy_recusado` sem sufixo; a única resolução específica usa `deploy_recusado:%s` (`app/core/esteira_vigia.py:1364`). A porta genérica continua aceitando qualquer chave (`app/core/management/commands/alarme_esteira.py:21-24`), procurando `ESTEIRA <chave>:` (`app/core/management/commands/alarme_esteira.py:13-15`). As novas resoluções não encontram texto antigo `ESTEIRA deploy_recusado: ...` (`app/core/management/commands/alarme_esteira.py:15`). Ações genéricas de resolver em `app/core/esteira_vigia.py:1642` recebem as chaves produzidas por `decidir`; seus resolvedores concretos são `pausa_sem_dono`, `teto_morto`, `trava_a_vazia`, `vigia_sem_efeito` (`app/core/esteira_vigia.py:490`, `:504`, `:530`, `:597`).

4. **`DEPLOY_RECUSA_IDS`: lista paralela — fila, classe (d).** A tupla é definida em `app/core/esteira_vigia.py:1316` e lida somente pelo loop em `app/core/esteira_vigia.py:1362`. Os emissores repetem os IDs em literais independentes: auto-revert (`app/core/esteira_vigia.py:1325`, `:1332`, `:1354`, `:1357`), integrador (`app/core/integrador_lote.py:771`, `:780`) e login (`bin/vigia_logins.sh:90`, `:95`). Trocar/adicionar um produtor não atualiza automaticamente o universo de fechamento (`app/core/esteira_vigia.py:1362-1364`).

5. **Revert do revert: rc lido, comunicação parcial — fila.** `rrd` nasce do `.returncode` em `app/core/esteira_vigia.py:1289-1290` e seu único leitor é o ternário que acrescenta texto à pauta em `app/core/esteira_vigia.py:1293-1294`. O retorno dessa `_pauta` é ignorado (`app/core/esteira_vigia.py:1291`), o relato não inclui `rrd` (`app/core/esteira_vigia.py:1295-1297`) e o retorno continua “revert desfeito” (`app/core/esteira_vigia.py:1298`). Uma pauta `auto_revert` já viva impede reescrever o novo rc (`app/core/management/commands/alarme_esteira.py:38-41`); haver tal pauta em produção é [não verificado].

6. **Revert do revert não usa explicitamente o ID auto-revert — fila.** O segundo deploy não recebe `env=...DEPLOY_RECUSA_ID` (`app/core/esteira_vigia.py:1289-1290`); herda o ambiente, ou usa `manual` pela regra de `bin/deploy.sh:52`. Seu rc75 deixa recusa correspondente (`bin/deploy.sh:286-290`), enquanto a repetição automática só lê `recusa_cron.auto_revert.pendente` (`app/core/esteira_vigia.py:1311`, `:1345-1351`).

7. **Arquivos de recusa, produção:**
   - Criador/sobrescritor: `bin/deploy.sh:286-287`, formato `id` + argv NUL; nome definido em `bin/deploy.sh:54`.
   - Apagador próprio: `bin/deploy.sh:363`; apagador global das anteriores: `bin/deploy.sh:366-368`.
   - Agendado remove o nome da tentativa antes de iniciá-la (`bin/deploy_agendado.sh:95`, `:104`); substitui a recusa anterior sob condição (`bin/deploy_agendado.sh:102`, `:110`).
   - Agendado lê conteúdo/ID em `bin/deploy_agendado.sh:83-87` e argv em `bin/deploy_agendado.sh:101`.
   - `retentar_deploy` lê conteúdo da recusa auto-revert (`app/core/esteira_vigia.py:1345-1353`) e existência das três recusas (`app/core/esteira_vigia.py:1363`).
   - Contador lê nomes/existência, sem conteúdo (`bin/deploy_recusado.sh:22-25`); placar lê sua saída e soma (`bin/placar_code.sh:464-466`, `:542`).
   - Testes criam/apagam recusas nas fixtures do caso 8 (`bin/tests/test_deploy_nao_mata_elo.sh:181-190`), caso 21 (`:716-737`) e caso 23 (`:858`, `:869`, `:879`); executam os blocos extraídos de produção em `:238-240` e `:738-739`.

8. **`_rec_ant`: cura o rc1 sem recusa nova.** O caminho antigo fica preservado em `bin/deploy_agendado.sh:102` e só é removido com rc0 ou rc75 e arquivo novo em `bin/deploy_agendado.sh:110`. A validação de ID da recusa nova vem depois, em `bin/deploy_agendado.sh:115`; portanto o teste `-f` que autoriza apagar a anterior não valida seu conteúdo (`bin/deploy_agendado.sh:110`, `:83-87`). Isso é divergência pequena de contrato, não prova de colisão real [não verificado].

9. **Porta canônica de pautas preservada.** A abertura e fechamento passam por `escrever`/`marcar_feita_pelo_sistema` (`app/core/management/commands/alarme_esteira.py:27`, `:31`, `:54-56`); as escritas de estado ficam em `app/pautas/services.py:180` e `:272-273`. Os emissores do delta não escrevem diretamente estado ORM de pauta (`app/core/integrador_lote.py:780`, `app/core/esteira_vigia.py:1332`, `:1364`, `bin/vigia_logins.sh:94-95`).

## 04/11 11:02 · sessao b4e9371026c1 · - · - · origem=Ronald (tmux)

1. **Caso 8, rc 1 — morde a cura específica.** O falso deploy dá rc 1 na segunda chamada sem criar recusa nova; exige rc 1, duas chamadas e exatamente uma recusa preservada (`bin/tests/test_deploy_nao_mata_elo.sh:186`, `:217`, `:219`, `:224`). Reintroduzir o `rm` incondicional da recusa anterior em `bin/deploy_agendado.sh:102` mata essa asserção. **Limite/fila:** remover inteiramente a exclusão condicional em `bin/deploy_agendado.sh:110` deixa passar os casos citados: a persistência verifica existência, não quantidade, e o teste rc 1 continua tendo uma recusa (`bin/tests/test_deploy_nao_mata_elo.sh:207`, `:224`, `:730`). Resultado do mutante: recusas substituídas acumulam e supercontam.

2. **Caso 19(e) — morde `exit 0`.** O falso Docker devolve 3 e o caso exige simultaneamente rc 3 e ausência de marcas; portanto substituir o retorno real por `exit 0` morre (`bin/tests/test_deploy_nao_mata_elo.sh:60`, `:660`, `:661`; produção `bin/em_casca.sh:26`, `:27`). Não exercita todos os possíveis códigos, mas isso não torna sem mordida o RED específico.

3. **Caso 21, limpeza — melhora real, ainda incompleto.** Executa o bloco real e verifica recusa antiga apagada, nova preservada e marcador apagado (`bin/tests/test_deploy_nao_mata_elo.sh:734`, `:736`, `:737`, `:740`). **Sobrevivente concreto/fila de mordida:** apagar a criação real do marcador em `bin/deploy.sh:282` deixa o caso passar, pois o selo fabrica o marcador em `bin/tests/test_deploy_nao_mata_elo.sh:738`; em produção o bloco depende da existência dele (`bin/deploy.sh:366`). Esse mutante impede a limpeza real.

4. **Caso 21, chave do lote — morde a regressão da chave.** Executa `_deploy_do_lote` com rc 0/75/1 e exige duas pautas exatamente `deploy_recusado:integrador`; voltar à chave única mata a asserção (`bin/tests/test_deploy_nao_mata_elo.sh:753`, `:756`, `:758`, `:808`; produção `app/core/integrador_lote.py:780`). **Limite:** `_pauta` é substituída por função que só acumula e devolve `None`; nenhuma falha 70/75 da pauta é exercitada, logo a nova comunicação “PAUTA NAO ABRIU” pode ser retirada sem este caso perceber (`bin/tests/test_deploy_nao_mata_elo.sh:750`; produção `app/core/integrador_lote.py:781`).

5. **Caso 21, revert do revert — morde ignorar completamente o rc, mas só prova texto para 75.** Força primeiro deploy 0, segundo 75, testes vermelhos e exige a frase `deploy do desfazer deu rc=75` (`bin/tests/test_deploy_nao_mata_elo.sh:793`, `:796`, `:798`, `:813`). **Sobrevivente/fila:** substituir a leitura por `rrd = 75` em `app/core/esteira_vigia.py:1289` continua atendendo o caso; não há execução do desfazer com rc 0 ou 1 nesse teste (`bin/tests/test_deploy_nao_mata_elo.sh:796`). Também não testa falha da pauta, pois a mesma substituição de `_pauta` permanece (`bin/tests/test_deploy_nao_mata_elo.sh:750`; produção `app/core/esteira_vigia.py:1291`).

6. **Caso 20, lacuna — morde somente a lacuna no universo e na fonte viva considerados pelo selo.** A lista de ausentes vem de `DUR_MAX_S`; o código exige zero ausentes apenas no caminho `/home/ronald/saas-hasner/...`, enquanto a cópia apenas informa a lacuna (`bin/tests/test_deploy_nao_mata_elo.sh:698`, `:699`, `:707`, `:708`). Essa assimetria está expressa no teste, não é cura de completude da cópia (`bin/tests/test_deploy_nao_mata_elo.sh:705`, `:706`). **Limite/fila de mordida:** mudar `adm_classifica_casca` para devolver 0 para um cron C específico, exceto os dois nomes exigidos, retira-o do universo e da checagem de lacuna; o selo constrói seu universo pelo próprio classificador e só fixa presença de `processar_cartorio`/`lavrar_he_pendente` (`bin/tests/test_deploy_nao_mata_elo.sh:689`, `:690`, `:711`). A conferência anterior dos C consulta `adm_texto_entra_na_casca`, não `adm_classifica_casca` (`bin/tests/test_deploy_nao_mata_elo.sh:296`, `:304`). Não executei esse mutante [não verificado].

7. **Caso 23, CLI — cura parcial, sobrevivente comprovado em memória.** A AST exige um `print(retentar_deploy())` em algum `if` cujo teste contenha a string; não verifica o operador de comparação (`bin/tests/test_deploy_nao_mata_elo.sh:904`, `:905`, `:906`). Trocar `==` por `!=` em `app/core/esteira_vigia.py:1657` preserva `cli=True`, mas deixa de executar a função para o comando solicitado. Reproduzi a checagem original sobre as duas ASTs em memória, com `python3 -B`, sem escrever arquivo. **Bloqueia como prova RED da CLI:** o defeito que o caso declara curar continua possível sem vermelho; o aval fornecido exige um mutante morto por item [não verificado quanto à numeração do anexo].

8. **Caso 23, resolução por id — morde os caminhos previstos; não prova resolução efetiva.** Exige resolução de auto_revert/integrador quando ausentes e preservação do integrador quando sua recusa permanece (`bin/tests/test_deploy_nao_mata_elo.sh:878`, `:879`, `:887`, `:888`). Porém a pauta é substituída por coletor que devolve `None`, então `_pauta --resolver` falhar com 70/75 não é exercitado (`bin/tests/test_deploy_nao_mata_elo.sh:865`). A produção ignora o retorno da resolução e termina com “no ar” sem agenda de nova resolução (`app/core/esteira_vigia.py:1364`, `:1367`); na passada seguinte sem recusa de auto_revert, sai antes (`app/core/esteira_vigia.py:1349`, `:1350`). Também não há fixture da chave antiga nos cenários de resolução (`bin/tests/test_deploy_nao_mata_elo.sh:868`, `:870`, `:878`, `:879`).

9. **Limitação de validação:** não executei o selo porque ele cria arquivos temporários, copia scripts e remove diretórios, contrariando a regra de só leitura (`bin/tests/test_deploy_nao_mata_elo.sh:44`, `:45`, `:46`). Não se pode afirmar suite verde nesta cópia [não verificado].

## 04/11 10:37 · sessao b22fc9c7c9ec · DEPLOY-[nome]-ELO · verificador · origem=Code

1. **Veredito: bloqueia a admissão do deploy.** Há caminhos para reiniciar com exec vivo, além de perda da recusa que alimenta o contador (`bin/em_casca.sh:26`, `bin/admissao_cron.sh:175`, `bin/deploy_agendado.sh:102`).

   | Item do aval | Resultado |
   |---|---|
   | **MUDA 1** | **Parcial — bloqueia.** Os três envelopes passam a `casca=0`, e os execs diretamente alterados recebem wrapper (`bin/admissao_cron.sh:36`, `bin/admissao_cron.sh:230`, `bin/sombra.sh:139`, `bin/vigia_arvore.sh:177`, `eval/harness.py:54`). Porém o vigia alcança `deploy.sh`, cujos execs continuam sem marca própria; detalhes no item 3 (`app/core/esteira_vigia.py:1340`, `bin/deploy.sh:102`). |
   | **MUDA 2** | **Parcial — bloqueia a prova do máximo completo.** Entre as durações presentes na cópia, o maior valor do universo remanescente é `processar_cartorio=412`, abaixo de 1500 (`app/config/crons_duracao.json:58`, `bin/admissao_cron.sh:43`). Mas há integrantes sem duração, como `lavrar_retrato_folha`; o selo simplesmente os exclui (`app/config/crons.py:1585`, `bin/tests/test_deploy_nao_mata_elo.sh:687`). O máximo de produção alegado no cabeçalho é **[nao verificado]**. |
   | **MUDA 3** | **Parcial — bloqueia.** Os três caminhos alterados leem rc, solicitam pauta e deixam o contador depender da recusa; o integrador retorna antes de `FIM` (`app/core/integrador_lote.py:748`, `app/core/integrador_lote.py:780`, `app/core/esteira_vigia.py:1312`, `bin/vigia_logins.sh:90`). Entretanto o agendador ainda pode apagar a testemunha sem restart, e `_pauta` descarta falhas de abertura (`bin/deploy_agendado.sh:102`, `app/core/esteira_vigia.py:898`). |
   | **MUDA 4** | **Realizado no caminho examinado.** A trava antecede a leitura decisiva do commit, cobre revert/publicação, e a guarda impede desfazer um assunto iniciado por `Revert "` (`bin/vigia_logins.sh:70`, `bin/vigia_logins.sh:75`, `bin/vigia_logins.sh:79`, `bin/vigia_logins.sh:86`). Os filhos de deploy e alarme fecham fd7 (`bin/vigia_logins.sh:90`, `bin/vigia_logins.sh:95`). |
   | **MUDA 5** | **Parcial — bloqueia.** Rc75 interrompe a conferência imediata, grava relato e prepara repetição antes da pausa do vigia (`app/core/esteira_vigia.py:1270`, `app/core/esteira_vigia.py:1319`, `bin/vigia_arvore.sh:28`). Porém uma pauta `deploy_recusado` preexistente impede atualizar o texto para “commit quebrado”: a porta retorna sem alterar a pauta viva (`app/core/management/commands/alarme_esteira.py:38`). |
   | **CONFERIR** | Há HTTP no harness, dentro do trecho já protegido pelo wrapper (`eval/harness.py:18`, `eval/harness.py:24`, `eval/harness.py:54`). A linha exigida no RELATO sobre essa descoberta é **[nao verificado]**. |
   | **RED** | Existem casos 19–23, mas todos têm lacunas demonstráveis por leitura; ver item 5 (`bin/tests/test_deploy_nao_mata_elo.sh:576`, `bin/tests/test_deploy_nao_mata_elo.sh:817`). Mutantes efetivamente executados e mortos, suite verde desta cópia e demais condições de PRONTO: **[nao verificado]**. |

   **Fila — cabeçalho ainda impreciso:** ele atribui `3300` ao `DUR_MAX_S` do vigia na cópia, mas o JSON contém `1586`, que sobrescreve o declarado (`bin/admissao_cron.sh:31`, `app/config/crons_duracao.json:82`, `app/config/crons.py:160`).

2. **Escritores, leitores e chamadores.**

   | Estado/autoridade | Quem escreve ou altera | Quem lê |
   |---|---|---|
   | **Marcas e `casca`** | Porta única `adm_marcar`, incluindo nome, PID e conteúdo (`bin/admissao_cron.sh:239`, `bin/admissao_cron.sh:247`). Chamadores: cron, alarme do cron, descoberta da corrente e wrapper (`bin/cron_run.sh:16`, `bin/cron_run.sh:52`, `bin/encadeado.sh:48`, `bin/em_casca.sh:16`). Remoção pelo dono via `adm_desmarcar`, ou reaper de órfãs (`bin/admissao_cron.sh:254`, `bin/admissao_cron.sh:208`). | Validação de vida, classificação efetiva e varredura (`bin/admissao_cron.sh:167`, `bin/admissao_cron.sh:181`, `bin/admissao_cron.sh:199`). Restart consome a varredura duas vezes; wrapper confere o campo (`bin/reinicio_sob_admissao.sh:40`, `bin/reinicio_sob_admissao.sh:44`, `bin/em_casca.sh:22`). O selo também cria e lê marcas artificiais (`bin/tests/test_deploy_nao_mata_elo.sh:125`, `bin/tests/test_deploy_nao_mata_elo.sh:627`). |
   | **`recusa_cron.<id>.pendente`** | `deploy.sh` grava `id` e argv após rc75; apaga a própria recusa e as anteriores ao corte (`bin/deploy.sh:286`, `bin/deploy.sh:363`, `bin/deploy.sh:367`). **Também apaga o agendador**, antes da repetição (`bin/deploy_agendado.sh:102`). | Agendador valida id e recupera argv; retry lê a de `auto_revert`; contador enumera os nomes (`bin/deploy_agendado.sh:83`, `bin/deploy_agendado.sh:101`, `app/core/esteira_vigia.py:1332`, `bin/deploy_recusado.sh:22`). Fixtures escrevem recusas artificiais (`bin/tests/test_deploy_nao_mata_elo.sh:703`, `bin/tests/test_deploy_nao_mata_elo.sh:832`). |
   | **`.restart_inicio.<pid>`** | Criado e removido por `deploy.sh`; há remoções em recusa, falha de restart e sucesso (`bin/deploy.sh:282`, `bin/deploy.sh:289`, `bin/deploy.sh:292`, `bin/deploy.sh:368`). | O próprio deploy consulta existência e usa mtime como corte da limpeza (`bin/deploy.sh:366`, `bin/deploy.sh:367`). |
   | **`vigia_logins.lock`** | O vigia abre o arquivo e adquire flock; a fixture também o ocupa (`bin/vigia_logins.sh:70`, `bin/vigia_logins.sh:71`, `bin/tests/test_deploy_nao_mata_elo.sh:805`). | O próprio flock decide se outra passada pode continuar (`bin/vigia_logins.sh:71`). |
   | **Pauta `deploy_recusado`** | Pedida pelo lote, auto-revert, retry e logins (`app/core/integrador_lote.py:780`, `app/core/esteira_vigia.py:1318`, `app/core/esteira_vigia.py:1342`, `bin/vigia_logins.sh:94`). Escrita efetiva pela porta existente, via `pautas.services.escrever` (`app/core/management/commands/alarme_esteira.py:54`). | A porta consulta pauta viva por motivo; resolução passa por `marcar_feita_pelo_sistema` (`app/core/management/commands/alarme_esteira.py:15`, `app/core/management/commands/alarme_esteira.py:31`). O único pedido produtivo de `--resolver deploy_recusado` encontrado está no retry (`app/core/esteira_vigia.py:1347`). |
   | **`ADM_SO_O_EXEC`** | Declarada em um lugar (`bin/admissao_cron.sh:36`). | `adm_so_o_exec` e selo (`bin/admissao_cron.sh:219`, `bin/tests/test_deploy_nao_mata_elo.sh:582`). |
   | **`adm_so_o_exec` / `adm_classifica_casca`** | Definidos na admissão (`bin/admissao_cron.sh:215`, `bin/admissao_cron.sh:225`). | O classificador chama o primeiro; `adm_marcar` consome o classificador; casos 11/19/20 também o consultam (`bin/admissao_cron.sh:230`, `bin/admissao_cron.sh:244`, `bin/tests/test_deploy_nao_mata_elo.sh:339`, `bin/tests/test_deploy_nao_mata_elo.sh:578`, `bin/tests/test_deploy_nao_mata_elo.sh:679`). |
   | **`adm_texto_entra_na_casca`** | Definido na admissão, agora incluindo `docker cp` (`bin/admissao_cron.sh:96`, `bin/admissao_cron.sh:110`). | Fecho da fonte, árvore de processos, classificador e selo (`bin/admissao_cron.sh:149`, `bin/admissao_cron.sh:160`, `bin/admissao_cron.sh:229`, `bin/tests/test_deploy_nao_mata_elo.sh:661`). |
   | **Contador / `TOTAL`** | O contador deriva quantidade e ids dos arquivos pendentes; não mantém estado próprio (`bin/deploy_recusado.sh:22`, `bin/deploy_recusado.sh:27`). O placar transforma ausência de número em 1 e soma `_n_dr` (`bin/placar_code.sh:466`, `bin/placar_code.sh:542`). | Placar e retry consomem o contador; o `TOTAL` decide mensagem e rc0/rc2 do placar (`bin/placar_code.sh:464`, `app/core/esteira_vigia.py:1346`, `bin/placar_code.sh:544`, `bin/placar_code.sh:554`). |

   **Rc de `deploy.sh`, por chamador:** [nao verificado]

   | Chamador | Tratamento |
   |---|---|
   | Lote final | Lê rc e impede `FIM` quando diferente de zero (`app/core/integrador_lote.py:771`, `app/core/integrador_lote.py:748`). |
   | Reload inicial do integrador | **Ainda ignora rc — fila** (`app/core/integrador_lote.py:638`). |
   | Auto-revert e retry novos | Leem `.returncode` e tratam falha (`app/core/esteira_vigia.py:1312`, `app/core/esteira_vigia.py:1340`). |
   | Revert do revert | **Ainda ignora rc, inclusive 75 — bloqueia o fechamento desse caminho**; segue anunciando “desfeito” (`app/core/esteira_vigia.py:1283`, `app/core/esteira_vigia.py:1286`). |
   | Vigia de logins | Lê `_rd`, solicita pauta em falha; o script termina com rc2 de alarme, não com o rc do deploy (`bin/vigia_logins.sh:90`, `bin/vigia_logins.sh:91`, `bin/vigia_logins.sh:105`). |
   | Agendador | Lê rc da esteira e da repetição direta; combina arquivo/id com rc75; sucesso após repetição vira rc3 pela cauda não executada (`bin/deploy_agendado.sh:97`, `bin/deploy_agendado.sh:106`, `bin/deploy_agendado.sh:111`, `bin/deploy_agendado.sh:120`). |
   | Moldes de cadeia | Deploy normal aborta por `set -e`; reload inicial tem guarda explícita (`bin/molde_fatia/cadeia.sh:4`, `bin/molde_fatia/cadeia.sh:48`, `bin/molde_fatia/cadeia.sh:89`, `bin/molde_autocura/cadeia.sh:3`, `bin/molde_autocura/cadeia.sh:45`, `bin/molde_autocura/cadeia.sh:86`). **Rollback ignora rc — fila:** usa `set +e` e anuncia recarga mesmo com falha (`bin/molde_fatia/cadeia.sh:26`, `bin/molde_fatia/cadeia.sh:32`, `bin/molde_autocura/cadeia.sh:25`, `bin/molde_autocura/cadeia.sh:31`). |
   | Fatias agendadas existentes | O135 confere ou propaga rc; abono usa `pipefail` e converte falha para rc1 (`fatias_agendadas/o135-teto20/esteira.sh:80`, `fatias_agendadas/o135-teto20/esteira.sh:117`, `fatias_agendadas/o135-teto20/esteira.sh:124`, `fatias_agendadas/abono-no-ar/esteira.sh:21`, `fatias_agendadas/abono-no-ar/esteira.sh:37`). |
   | Cron de higiene | Declarado como RAW; o runner captura e devolve rc (`app/config/crons.py:307`, `bin/cron_run.sh:24`, `bin/cron_run.sh:70`). |

   **`DEPLOY_RECUSA_ID`:** produtores explícitos são integrador, auto-revert, retry, logins e agendador; o deploy aplica default `manual`, valida e usa no caminho/conteúdo (`app/core/integrador_lote.py:771`, `app/core/esteira_vigia.py:1311`, `app/core/esteira_vigia.py:1339`, `bin/vigia_logins.sh:90`, `bin/deploy_agendado.sh:94`, `bin/deploy_agendado.sh:104`, `bin/deploy.sh:52`, `bin/deploy.sh:54`, `bin/deploy.sh:286`).

   **Sim, há consumidores fora do diff com comportamento diferente:** `cron_run.sh` continua chamando a porta, mas os três envelopes agora recebem `casca=0`; o restart inalterado consome essa mudança. A árvore viva ainda pode fazer uma marca zero segurar durante um descendente Docker (`bin/cron_run.sh:16`, `bin/admissao_cron.sh:230`, `bin/admissao_cron.sh:186`, `bin/reinicio_sob_admissao.sh:40`).

3. **Sim: há execs transitivos sem `em_casca` — bloqueia.**

   O caminho novo é `vigia_arvore → retentar-deploy → deploy.sh`; o auto-revert também alcança o mesmo script (`bin/vigia_arvore.sh:28`, `app/core/esteira_vigia.py:1340`, `app/core/esteira_vigia.py:1312`).

   Dentro desse fecho ficam descobertos:

   - Check, plano e aplicação de migrations (`bin/deploy.sh:102`, `bin/deploy.sh:109`, `bin/deploy.sh:124`).
   - `collectstatic` no UI (`bin/deploy.sh:205`).
   - Espera e prova de rotas com alvo por parâmetro (`bin/deploy.sh:300`, `bin/deploy.sh:316`).

   A trava do restart não cobre esses preflights nem as provas posteriores; outro deploy pode reiniciar durante um desses execs. A descoberta por árvore de uma marca zero não fecha a corrida entre varredura e nascimento de um exec que não usa a admissão (`bin/deploy.sh:283`, `bin/reinicio_sob_admissao.sh:44`, `bin/reinicio_sob_admissao.sh:66`, `bin/admissao_cron.sh:186`).

   **Rede:** o noturno faz HTTP para `localhost:8010` dentro da mensageria; esse trecho passa pelo wrapper (`eval/harness.py:18`, `eval/harness.py:24`, `eval/harness.py:54`). A sombra consulta o PostgreSQL de produção por socket dentro de `saas_db`; os alvos variáveis ali são fixados como `saas_db` e `sombra_db`, distintos do conjunto reiniciado (`bin/sombra.sh:72`, `bin/sombra.sh:101`, `bin/sombra.sh:105`, `bin/admissao_cron.sh:42`). Há também teste TCP local em `sombra_db` (`bin/sombra_db.sh:40`, `bin/sombra_db.sh:73`).

   `saas_caddy` não pertence a `ADM_CASCAS`; o conjunto efetivo é core, UI e mensageria. Um caminho alcançável que execute especificamente em `saas_caddy` permanece **[nao verificado]** (`bin/admissao_cron.sh:42`).

4. **`em_casca.sh`: caminho normal correto; ciclo de vida incompleto.**

   - **Normal:** só executa após marca aceita e campo igual a 1; guarda o rc do comando e o devolve; o EXIT solicita remoção (`bin/em_casca.sh:16`, `bin/em_casca.sh:21`, `bin/em_casca.sh:22`, `bin/em_casca.sh:26`, `bin/em_casca.sh:27`).
   - **Bloqueia — wrapper pode morrer antes do exec.** O eval passou a aplicar timeout ao processo `bash em_casca.sh`, enquanto Docker roda como filho. Não há acompanhamento do filho após morte do wrapper; existe apenas trap EXIT (`eval/oraculo.py:122`, `eval/oraculo.py:124`, `bin/em_casca.sh:21`, `bin/em_casca.sh:26`). Se o wrapper é morto e o filho continua, a marca perde validade pelo PID/starttime e o reaper a remove, permitindo restart com exec ainda vivo (`bin/admissao_cron.sh:169`, `bin/admissao_cron.sh:175`, `bin/admissao_cron.sh:208`, `bin/reinicio_sob_admissao.sh:66`). Ocorrência real: **[nao verificado]**.
   - **Fila — marca pode sobrar fisicamente.** Morte sem execução do EXIT deixa o arquivo até uma varredura posterior; ele passa a ser órfão (`bin/em_casca.sh:21`, `bin/admissao_cron.sh:167`, `bin/admissao_cron.sh:208`).
   - **Fila — marca nasce zero.** Para alvo como `sombra_db`, `adm_marcar` grava `casca=0` antes de o wrapper verificar e sair 64. Logo a promessa literal “não nasce diferente de 1” não é cumprida, embora o comando seja barrado (`bin/em_casca.sh:16`, `bin/em_casca.sh:22`, `bin/admissao_cron.sh:244`, `bin/admissao_cron.sh:247`).

5. **Casos 19–23 mordem parcialmente. Os sobreviventes abaixo são deduções estáticas; execução dos mutantes é [nao verificado].**

   | Caso | Mordida existente | Mutação plausível que passa |
   |---|---|---|
   | **19** | Exercita espera, recusa, desmarca e serialização de chegada durante restart (`bin/tests/test_deploy_nao_mata_elo.sh:625`, `bin/tests/test_deploy_nao_mata_elo.sh:646`). | Trocar somente o `exit "$_r"` final por `exit 0`: todos os comandos válidos exercitados retornam zero; a rejeição 64 ocorre antes. **Fila: não prova preservação de rc não zero** (`bin/em_casca.sh:24`, `bin/em_casca.sh:27`, `bin/tests/test_deploy_nao_mata_elo.sh:632`). Outra lacuna **bloqueante**: exec cru com alvo Python variável escapa do scanner de constantes, e helper novo escapa da lista fixa (`bin/tests/test_deploy_nao_mata_elo.sh:591`, `bin/tests/test_deploy_nao_mata_elo.sh:605`). |
   | **20** | Compara teto com durações encontradas usando `DUR_MAX_S` (`bin/tests/test_deploy_nao_mata_elo.sh:687`, `bin/tests/test_deploy_nao_mata_elo.sh:692`). | Remover a duração de `lavrar_he_pendente` do JSON: continua cron, mas sai silenciosamente da medição por ausência da chave. **Bloqueia a prova de cobertura** (`app/config/crons_duracao.json:41`, `app/config/crons.py:1506`, `bin/tests/test_deploy_nao_mata_elo.sh:687`). |
   | **21** | Testa contador vazio/com arquivos, tratamento do lote e recusa inicial do auto-revert (`bin/tests/test_deploy_nao_mata_elo.sh:701`, `bin/tests/test_deploy_nao_mata_elo.sh:704`, `bin/tests/test_deploy_nao_mata_elo.sh:769`, `bin/tests/test_deploy_nao_mata_elo.sh:774`). | Remover o `find ... -delete`, deixando o literal `recusa_cron.*.pendente` em comentário: a “prova” da limpeza é apenas grep. **Bloqueia** (`bin/deploy.sh:367`, `bin/tests/test_deploy_nao_mata_elo.sh:717`). |
   | **22** | Testa guarda contra revert do revert e trava previamente ocupada (`bin/tests/test_deploy_nao_mata_elo.sh:802`, `bin/tests/test_deploy_nao_mata_elo.sh:805`). | Liberar fd7 imediatamente após adquirir a trava, antes de ler/reverter/publicar. A fixture ocupada continua barrando, mas não há duas passadas reais concorrentes para perceber a liberação prematura. **Bloqueia** (`bin/vigia_logins.sh:71`, `bin/vigia_logins.sh:75`, `bin/tests/test_deploy_nao_mata_elo.sh:807`). |
   | **23** | Exercita diretamente a função, argv/id e condição de resolução (`bin/tests/test_deploy_nao_mata_elo.sh:831`, `bin/tests/test_deploy_nao_mata_elo.sh:847`). | Na CLI, substituir `print(retentar_deploy())` por `print('nada pendente')`. Os testes diretos continuam verdes; a conferência da CLI só procura o literal da opção. A repetição automática real desaparece. **Bloqueia** (`app/core/esteira_vigia.py:1641`, `bin/tests/test_deploy_nao_mata_elo.sh:857`). |

6. **Classes de reprovação solicitadas.**

   | Classe | Avaliação |
   |---|---|
   | **(a) Segundo juiz registrado** | Não identifiquei duplicação de juiz de domínio: o novo classificador é consumido pela porta de marcas, e a pauta usa o serviço existente. O registro vivo, conforme JUIZES, é `core/juizes.py` (`app/docs/JUIZES.md:11`, `bin/admissao_cron.sh:244`, `app/core/management/commands/alarme_esteira.py:54`). |
   | **(b) Segundo escritor fora da porta** | **Bloqueia:** o apagador antigo do agendador continua alterando o estado que o novo contador declara pertencer ao deploy bem-sucedido (`bin/deploy_agendado.sh:102`, `bin/deploy_recusado.sh:7`). |
   | **(c) Relógio fora da borda** | **Fila:** há novo corte temporal por mtime para decidir quais recusas apagar; ele nasce antes da espera/restart, sem serialização com os escritores de recusa (`bin/deploy.sh:282`, `bin/deploy.sh:286`, `bin/deploy.sh:367`). As novas chamadas de `strftime` servem ao relato, não ao teto de espera (`app/core/esteira_vigia.py:1319`, `app/core/esteira_vigia.py:1349`). |
   | **(d) Derivação paralela** | **Bloqueia:** o fecho do selo é uma lista manual e omite o deploy realmente alcançado (`bin/tests/test_deploy_nao_mata_elo.sh:591`, `app/core/esteira_vigia.py:1340`). A duração usa a fonte correta, mas exclui suas lacunas (`bin/tests/test_deploy_nao_mata_elo.sh:687`). |
   | **(e) Selo/RED sem mordida suficiente** | **Bloqueia:** limpeza por grep e CLI não exercitada permitem os mutantes dos casos 21 e 23 (`bin/tests/test_deploy_nao_mata_elo.sh:717`, `bin/tests/test_deploy_nao_mata_elo.sh:857`). |
   | **(f) Ordem/herança de trava** | A admissão fecha fd8 antes do comando, e o restart fecha fd8 nos filhos longos (`bin/admissao_cron.sh:250`, `bin/reinicio_sob_admissao.sh:66`, `bin/reinicio_sob_admissao.sh:69`). O novo fd7 não vai aos filhos de deploy/alarme; o pai o segura durante a publicação conforme MUDA4 (`bin/vigia_logins.sh:68`, `bin/vigia_logins.sh:90`, `bin/vigia_logins.sh:95`). Deadlock novo demonstrado: **[nao verificado]**. |
   | **(g) Pouso antes do pré-requisito** | Pouso, horário e comprovação prévia da suite nesta volta: **[nao verificado]**. O selo exige escrita e consulta também JSON fora da cópia, portanto sua existência não prova execução válida sob este exame (`bin/tests/test_deploy_nao_mata_elo.sh:44`, `bin/tests/test_deploy_nao_mata_elo.sh:681`). |
   | **(h) Fallback/env solto** | O teto de cron continua declarado e o env só o reduz; ids novos são explícitos (`bin/admissao_cron.sh:43`, `bin/admissao_cron.sh:58`, `app/core/esteira_vigia.py:1311`). `ADM_ESPERA_ADMISSAO_S` permanece livre por ambiente, mas é trecho preexistente, não introdução deste diff (`bin/admissao_cron.sh:50`). |
   | **(i) Except que engole erro** | O except novo trata especificamente arquivo ausente; não engole erro geral (`app/core/esteira_vigia.py:1334`). O fallback amplo de duração já existia e continua relevante à cobertura incompleta (`app/config/crons.py:155`, `bin/tests/test_deploy_nao_mata_elo.sh:687`). |
   | **(j) Restart com exec vivo** | **Bloqueia:** presente pelos execs transitivos sem admissão e pela morte do wrapper antes do filho, descritos nos itens 3–4 (`bin/deploy.sh:102`, `bin/em_casca.sh:26`, `bin/admissao_cron.sh:175`). |

7. **Outros defeitos.**

   - **Bloqueia — contador pode zerar sem cura.** O agendador apaga a recusa antes da repetição. Se a repetição falhar numa guarda anterior ao restart, não nasce outra recusa e o contador vê zero com publicação ainda pendente (`bin/deploy_a

[truncado em 20000 de 22791 caracteres]

## 04/11 10:34 · sessao fefc8cd1ab3a · - · - · origem=Ronald (tmux)

Inventário e achados, somente por leitura:

- **Chamadores de deploy que leem rc:** `app/core/integrador_lote.py:771` obtém rc via `_rodar` (`:247`), retorna sem FIM em `:748`; `app/core/esteira_vigia.py:1312` e `:1340` leem `.returncode`; `bin/vigia_logins.sh:90` captura `_rd`, trata em `:91`; `bin/deploy_agendado.sh:96` captura rc da esteira em `:97`, e chamada direta `:105` captura em `:106`; `fatias_agendadas/o135-teto20/esteira.sh:116` captura em `:117`, devolve em `:124`; chamada `--conferir` desse arquivo em `:80` aborta por `parar`; `fatias_agendadas/abono-no-ar/esteira.sh:37` detecta falha com `pipefail` (`:21`) e converte para rc1 (`:38`).

- **Templates:** deploy normal de `bin/molde_fatia/cadeia.sh:89` e `bin/molde_autocura/cadeia.sh:86` aborta sob `set -e` (`:4` / `:3`), portanto o `tail` seguinte não mascara normalmente o 75. Os reloads iniciais são guardados com `|| exit 1` (`molde_fatia:48`; `molde_autocura:45`). **Ignoram rc no rollback:** `molde_fatia:26` e `molde_autocura:25` fazem `set +e`; os reloads `:32` / `:31` são seguidos de mensagem “recarregada” sem conferir. **Fila.**

- **Ignorados restantes:** `app/core/integrador_lote.py:638` joga fora rc do reload-copia; `app/core/esteira_vigia.py:1283` joga fora rc do deploy do revert do revert e em `:1286` anuncia “desfeito”. O segundo ainda aceita rc75 sem pauta `deploy_recusado` nem nova tentativa do id auto_revert. **Bloqueia o fechamento “rc nos chamadores” como exaustivo.** O cron `--reload-agendado` está em `app/config/crons.py:307`; usa RUNNER declarado em `:1423`/`:1443`, cujo `bin/cron_run.sh:24` captura rc e `:70` devolve.

- **DEPLOY_RECUSA_ID:** fonte externa aceita e validada em `bin/deploy.sh:52`/`:53`, default `manual`; nome do arquivo em `:54`, conteúdo `id=` em `:286`. Produtores: integrador (`app/core/integrador_lote.py:771`), auto-revert (`app/core/esteira_vigia.py:1311`), retentativa (`:1339`), vigia_logins (`bin/vigia_logins.sh:90`), agendado inicial (`bin/deploy_agendado.sh:94`/`:96`) e por repetição (`:104`/`:105`). Testes produzem/inspecionam id em `bin/tests/test_deploy_nao_mata_elo.sh:178`, `:204`, `:732`, `:750`, `:799`, `:820`.

- **Segundo apagador mantém buraco no contador:** `bin/deploy_agendado.sh:102` apaga recusa antes da tentativa seguinte. Se essa tentativa falha antes do restart (por exemplo guarda de auth em `bin/deploy.sh:170`) não nasce nova recusa, e o contador passa a zero embora a recusa nunca tenha sido superada por restart. Contradiz o novo comentário `bin/deploy_recusado.sh:7` e `bin/deploy_agendado.sh:113`. **Bloqueia.** Casos 8/21 só exercitam novas recusas ou sucesso, não falha de preflight depois do apagamento (`bin/tests/test_deploy_nao_mata_elo.sh:188`, `:714`).

- **Pauta abrir/resolver:** aberturas novas em `app/core/integrador_lote.py:780`, `app/core/esteira_vigia.py:1318`, `:1342`, `bin/vigia_logins.sh:94`. `_pauta` usa wrapper em `app/core/esteira_vigia.py:898` mas continua ignorando rc; agora 75/70 da admissão também pode perder o alarme sem registro. **Bloqueia MUDA3 como garantia.** Vigia_logins ao menos informa a falha (`bin/vigia_logins.sh:96`). A porta canônica consulta pauta em `app/core/management/commands/alarme_esteira.py:13`, escreve via `pautas.services.escrever` em `:54`, resolve via `marcar_feita_pelo_sistema` em `:31`.

- **Pauta pode permanecer aberta depois de cura:** único `--resolver deploy_recusado` em produção está em `app/core/esteira_vigia.py:1347`; só alcançável quando existe arquivo auto_revert (`:1332`/`:1335`). Se só integrador ou vigia_logins recusou e um deploy posterior passou, `bin/deploy.sh:367` limpa a recusa, mas nenhum caminho de sucesso deles resolve a pauta (`integrador_lote.py:773`; `vigia_logins.sh:90`). Retentar retorna “nada pendente” antes da conferência do contador. **Fila** (alarme falso persistente).

- **Pauta compartilhada pode omitir o texto exigido por MUDA5:** a porta existente retorna sem atualizar texto quando já há pauta desse motivo (`app/core/management/commands/alarme_esteira.py:38`/`:41`). Uma recusa anterior do integrador impede que a chamada do auto-revert (`app/core/esteira_vigia.py:1318`) passe a dizer “PROD SEGUE no commit quebrado”. **Bloqueia o item literal em todas as sequências.**

- **Fecho transitivo invalida “sem exec cru”:** `bin/vigia_arvore.sh:27` chama `retentar-deploy`; `app/core/esteira_vigia.py:1340` chama deploy; esse script faz `docker exec` direto sem `em_casca` em `bin/deploy.sh:102`, `:109`, `:124`, `:205`, `:300`, `:316`. Auto-revert também chega por `app/core/esteira_vigia.py:1312` e `:1283`. A admissão própria do restart só começa em `bin/deploy.sh:283`; preflight e provas ficam descobertos perante outro deploy concorrente. `_SCAN19` não inclui `deploy.sh` (`bin/tests/test_deploy_nao_mata_elo.sh:596`). **Bloqueia MUDA1 / proibição de restart sobre exec vivo.**

- **Vigia_logins:** trava canônica só escrita/lida pelo próprio script em `bin/vigia_logins.sh:70`/`:71` (e fixture do selo `bin/tests/test_deploy_nao_mata_elo.sh:805`); fd7 é fechado nos filhos de deploy e alarme em `:90`/`:95`; o pai mantém a trava cobrindo ler commit, revert e deploy. Guarda de revert do revert em `:79` precede `git revert` (`:86`). Não encontrei bypass entre duas passadas desse mesmo script; execução em produção é [nao verificado].

Nenhum teste foi executado nem arquivo gravado [nao verificado quanto a resultados executados]; afirmações de comportamento acima são deduções dos pontos citados. [nao verificado]

## 04/11 10:33 · sessao b088deebb57f · - · - · origem=Ronald (tmux)

Achados principais:

- **Bloqueia — timeout remove a proteção antes do fim do exec.** `eval/oraculo.py:121-124`, `eval/juiz.py:242-245`, `eval/harness.py:44-56`, `eval/gerador.py:65-66` e `eval/investigador.py:26-28` passaram a executar **bash em_casca** como processo direto, com timeout. O Docker roda como filho (`bin/em_casca.sh:26`); não há tratamento de sinais nem acompanhamento do filho após morte do wrapper, apenas trap EXIT (`bin/em_casca.sh:21`). Quando o wrapper morre, a marca deixa de ser viva pelo PID/starttime (`bin/admissao_cron.sh:169-175`) e é apagada pelo reaper (`bin/admissao_cron.sh:208`), permitindo restart (`bin/reinicio_sob_admissao.sh:40-45`, `:66`) enquanto o Docker/exec pode continuar. O filho órfão também deixa de pertencer à árvore do cron usada pelo fallback (`bin/admissao_cron.sh:157-160`, `:186`). A ocorrência em produção é [nao verificado]; o caminho de perda de proteção existe no código citado. A implementação local de `subprocess.run` confirma kill apenas do processo direto, mas fica fora das fontes autorizadas da cópia, portanto evitar usá-la como citação final.

- **Fila — marca nasce com casca=0 para alvo inválido.** `bin/em_casca.sh:16` grava antes de verificar casca em `:22-24`; o valor sai de `bin/admissao_cron.sh:244-247`. `docker exec sombra_db true` cria marca casca=0 e só depois sai 64 e desmarca via EXIT. Portanto não satisfaz literalmente “a marca nasce sempre casca=1”. O caso 19 afirma testar isso, mas observa apenas rc64 e ausência de exec (`bin/tests/test_deploy_nao_mata_elo.sh:652-655`).

- **Fila — selo 19 não testa rc não zero do comando nem morte/timeout.** As chamadas válidas usam fake Docker com sucesso; a única afirmação de preservação de rc verifica zero (`bin/tests/test_deploy_nao_mata_elo.sh:625-632`, `:646-650`). Mutação `exit 0` em `bin/em_casca.sh:27` sobrevive ao trecho 19 (a invalidade já sai 64 antes, em `:24`). Não há caso de sinal/timeout nas linhas `:623-656`.

- **Fila — scanner 19 tem fecho manual e ignora alvos variáveis.** Lista fixa `_SCAN19` em `bin/tests/test_deploy_nao_mata_elo.sh:591`; Python só analisa `ast.List` e constantes (`:600-608`), shell só compara tokens literais às cascas (`:614-616`). Mutação plausível: acrescentar helper novo chamado por noturno/sombra ou usar `docker exec "$CASCA"` sem wrapper; o scanner não deriva o fecho nem considera variável como casca. A autoridade produtiva expressamente considera variável como casca (`bin/admissao_cron.sh:100`, `:115`), divergindo da detecção do selo.

Censo das marcas e autoridades:

- Escritor de marcas/campo casca: único `adm_marcar`, caminho/arquivo em `bin/admissao_cron.sh:239-247`. Chamadores: `bin/cron_run.sh:16`, `:52`; `bin/encadeado.sh:48`; novo `bin/em_casca.sh:16`.
- Removedores: `adm_desmarcar` em `bin/admissao_cron.sh:254`, chamado pelos traps `bin/cron_run.sh:22` e `bin/em_casca.sh:21`, pelo alarme `bin/cron_run.sh:51`, `:58`, pela descoberta `bin/encadeado.sh:50`; reaper de órfãs em `bin/admissao_cron.sh:199-208`.
- Leitores: `adm_marca_viva` (`bin/admissao_cron.sh:167-178`), `adm_segura` (`:181-186`), `adm_varrer` (`:199-210`), `adm_seguram` (`:213`); restart consome em `bin/reinicio_sob_admissao.sh:40`, `:44`; wrapper lê casca em `bin/em_casca.sh:22`; testes leem/escrevem marcas falsas, por exemplo `bin/tests/test_deploy_nao_mata_elo.sh:125-128`, `:627`.
- `ADM_SO_O_EXEC`: declarada somente em `bin/admissao_cron.sh:36`; lida em `adm_so_o_exec` (`:219`) e selo (`bin/tests/test_deploy_nao_mata_elo.sh:582`).
- `adm_so_o_exec`: definição `bin/admissao_cron.sh:215`; leitor `adm_classifica_casca` em `:230`.
- `adm_classifica_casca`: definição `:225`; escritor canônico consome em `:244`; selo casos 11/19/20 consome em `bin/tests/test_deploy_nao_mata_elo.sh:339`, `:578`, `:679`.
- `adm_texto_entra_na_casca`: definição `bin/admissao_cron.sh:96`; leitores fecho estático `:149`, árvore viva `:160`, classificador `:229` e testes.
- **Leitores/escritores fora do diff mudam de comportamento:** `cron_run.sh:16` e `encadeado.sh:48` continuam chamando a mesma porta, agora com classificador novo; os três RAW host passam a gravar 0 por `bin/admissao_cron.sh:230`, enquanto antes o fecho os classificava 1. `adm_segura` ainda pode elevar uma marca casca=0 quando há descendente cujo texto parece Docker (`:186`, `:157-160`). `reinicio_sob_admissao.sh:40-44` consome essa mudança sem alteração própria.

Travas:

- A admissão fecha fd8 antes do comando (`bin/admissao_cron.sh:250`), e restart fecha fd8 em filhos longos (`bin/reinicio_sob_admissao.sh:66`, `:69`); não identifiquei herança nova dessa trava.
- A trava de logins é aberta/flocada em `bin/vigia_logins.sh:70-71`, cobre revert/publicação, e fd7 é fechado nas duas chamadas longas novas (`:90`, `:95`); o pai continua segurando-a enquanto publica, por intenção do MUDA4 (`:68-69`).
- `vigia_arvore` segura fd9 em `bin/vigia_arvore.sh:23-24` durante a nova tentativa (`:28`); existe herança para a pipeline nova. Não demonstrado filho persistente/desacoplado nesse trecho; não chamar de deadlock sem outra evidência.
- `ADM_ESPERA_ADMISSAO_S` continua vindo livre do ambiente (`bin/admissao_cron.sh:50`), mas não é introdução deste diff; ESPERA_CRON_S só reduz teto (`:55-59`). Não apontar teto por ambiente solto como novidade.

Nenhum AGENTS.md foi encontrado pelo `rg --files -g AGENTS.md` [nao verificado]. Nenhum teste que escreve foi executado [nao verificado]. [nao verificado]

## 04/11 10:33 · sessao ef02e1701314 · - · - · origem=Ronald (tmux)

Achados de leitura; mutantes abaixo são análise estática, sem execução [nao verificado].

1. **Bloqueia — fechamento transitivo incompleto.** `bin/vigia_arvore.sh:28` chama `retentar-deploy`, que executa `bin/deploy.sh` em `app/core/esteira_vigia.py:1340`; o mesmo vigia pode chamar auto-revert por `bin/vigia_arvore.sh:180` → `app/core/esteira_vigia.py:1270`. O deploy tem `docker exec` sem `em_casca` em `bin/deploy.sh:102`, `:109`, `:124`, `:205`, `:300` e `:316`. Os dois últimos recebem cascas por parâmetro, passadas em `bin/deploy.sh:303` e `:334`. A lista fixa do scanner não contém `deploy.sh`, em `bin/tests/test_deploy_nao_mata_elo.sh:591`.

2. **CONFERIR: há HTTP, já abrangido pela marca do trecho.** `eval/harness.py:18` cria requisição para `http://localhost:8010/api/perguntar/` e `:24` a executa. Esse código roda dentro do `docker exec mensageria` envelopado por `em_casca` em `eval/harness.py:54`. Os demais módulos noturnos chamados por `eval/noturno.py:32`, `:36`, `:43`, `:93` usam os wrappers registrados em `eval/gerador.py:65`, `:79`, `:84`; `eval/juiz.py:239`, `:243`; `eval/investigador.py:26`, `:206`; `eval/oraculo.py:122`. Registro da descoberta exigido pelo CONFERIR no RELATO: [nao verificado].

3. **Alvos variáveis da sombra identificados.** Os `docker exec` diretos de `bin/sombra.sh:101`, `:245`, `:256` usam os bancos fixados como `PG_PROD=saas_db` e `PG=sombra_db`, em `bin/sombra.sh:72`. O incremental fixa os mesmos alvos em `bin/sombra_incremental.sh:20`; seus exec/cp de banco não são os três processos reiniciados pelo deploy. `bin/sombra_db.sh:73` testa PostgreSQL por TCP local dentro de `sombra_db`; `bin/sombra_db.sh:40` fixa esse alvo. `bin/prova_de_casca.sh:30` usa `docker run --network none`, sem exec/cp em casca.

4. **Casos 19–23: mordida parcial e mutantes plausíveis sobreviventes.**

   - **19:** trocar um wrapper Python por alvo variável, por exemplo `alvo='mensageria'; subprocess.run(['docker','exec',alvo,...])` em `eval/harness.py:54`. O scanner só procura cascas em nós constantes da lista (`bin/tests/test_deploy_nao_mata_elo.sh:605`), portanto omite o alvo variável. Também não percorre dependências: a lista fixa está em `:591`. **Bloqueia.**
   - **20:** acrescentar um cron casca=1 sem entrada em `DUR_MAX_S`: o teste o exclui silenciosamente por `if n in c.DUR_MAX_S`, em `bin/tests/test_deploy_nao_mata_elo.sh:687`. A comparação com o teto pode continuar verde, em `:692`. **Bloqueia.**
   - **21:** retirar o `find ... -delete` de `bin/deploy.sh:368`, conservando o literal `recusa_cron.*.pendente` num comentário. A suposta prova da limpeza é somente um grep desse literal, em `bin/tests/test_deploy_nao_mata_elo.sh:717`; os testes Python substituem o subprocesso em `:730` e `:752`. **Bloqueia.**
   - **22:** liberar o fd7 imediatamente depois de adquirir o flock (`bin/vigia_logins.sh:69`), antes de ler/reverter/publicar. O teste comprova apenas que uma trava previamente ocupada impede a passada (`bin/tests/test_deploy_nao_mata_elo.sh:805`); não executa duas passadas sobrepostas nem observa a posse durante deploy (`:798`, `:801`, `:812`). Esse mutante passa e restaura a janela de corrida. **Bloqueia.**
   - **23:** mudar o despacho da CLI de `print(retentar_deploy())` para `print('nada pendente')` em `app/core/esteira_vigia.py:1641`. O teste exercita diretamente a função (`bin/tests/test_deploy_nao_mata_elo.sh:831`, `:834`, `:837`) e só exige que exista o literal `'retentar-deploy'` no módulo (`:857`); a tentativa automática real morre sem o selo acusar. **Bloqueia.**

5. **Fila — rc perdido em pipe.** `bin/vigia_arvore.sh:12` habilita somente `set -u`; `retentar-deploy`, alarmes e curas terminam em `tail`, em `:28`, `:177`, `:180`, `:183`, `:186`. Falhas desses comandos podem ficar representadas pelo rc do `tail`. Ademais, a CLI devolve 0 independentemente do resultado textual da tentativa em `app/core/esteira_vigia.py:1642`.

6. **Fora do diff, chamador ainda ignora rc.** O caminho “revert do revert” executa deploy sem ler `returncode` em `app/core/esteira_vigia.py:1283` e publica o relato de que o revert foi desfeito em `:1286`; não passa por `_deploy_do_revert` nem fixa `DEPLOY_RECUSA_ID=auto_revert`. **Bloqueia.**

## 04/11 08:09 · sessao 7c92567a8ce6 · DEPLOY-[nome]-ELO · verificador · origem=Code

**A volta 4 corrige os casos apontados, mas ainda não demonstra a garantia inteira.** Permanecem contraexemplos de admissão dinâmica e de isenção ancestral, além de um consumidor que pode registrar sucesso após repetir somente o deploy, deixando a cauda da esteira sem executar. Ocorrência desses caminhos em PROD: **[nao verificado]**. (`bin/admissao_cron.sh:174`; `bin/admissao_cron.sh:193`; `bin/deploy_agendado.sh:105`; `fatias_agendadas/abono-no-ar/esteira.sh:38`)

Executei a conferência de sintaxe Bash e o contrato textual, ambos com rc 0. Selo integral e mutantes executados nesta revisão: **[nao verificado]**; o selo cria e remove arquivos. (`bin/contrato_deploy.sh:60`; `bin/tests/test_deploy_nao_mata_elo.sh:44`)

**1. Escopo inteiro, segurança e progresso**

O protocolo fecha a corrida entre **publicação de uma marca reconhecida** e restart: o cron publica sob flock; o restarter adquire a mesma trava e varre novamente; bloqueador persistente provoca rc 75, sem restart. Isso protege marca válida, envelope vivo, `casca=1` e não ancestral. (`bin/admissao_cron.sh:211`; `bin/admissao_cron.sh:215`; `bin/reinicio_sob_admissao.sh:43`; `bin/reinicio_sob_admissao.sh:44`; `bin/reinicio_sob_admissao.sh:48`)

Ainda há estes limites:

| Caminho | Conclusão |
|---|---|
| **Comando dinâmico antes do exec** | Contraexemplo aceito pelo runner: `CMD='eval "$HCMD"'`, com `HCMD='sleep 2; docker exec saas_core sleep 30'`. A classificação recebe o texto sem expandir `HCMD`; durante o sleep, a árvore também não mostra exec. O restart pode começar e o exec futuro nasce sem nova admissão. RAW instalado com esse comportamento: **[nao verificado]**. (`bin/admissao_cron.sh:108`; `bin/admissao_cron.sh:130`; `bin/admissao_cron.sh:145`; `bin/admissao_cron.sh:212`; `bin/cron_run.sh:24`; `bin/reinicio_sob_admissao.sh:45`) |
| **Ancestral com outro filho vivo** | Contraexemplo: `docker exec saas_core sleep 30 & bash <raiz>/bin/reinicio_sob_admissao.sh; wait`. A marca ancestral inteira é dispensada antes de examinar o exec irmão. O vigia instalado chama deploy sincronamente; concorrência desse tipo nele: **[nao verificado]**. (`bin/admissao_cron.sh:193`; `bin/vigia_logins.sh:71`) |
| **Envelope morto, filho vivo — D4b** | O PID ausente torna a marca órfã e provoca remoção, sem procurar filhos sobreviventes. Portanto o descarte não refuta a classe. Morte isolada do envelope em PROD: **[nao verificado]**. (`bin/admissao_cron.sh:158`; `bin/admissao_cron.sh:196`; `bin/cron_run.sh:24`) |
| **Falha durante restart** | Timeout/falha registra fim e sai, liberando a trava sem demonstrar que a operação sobre as cascas terminou. Sobrevida da operação no daemon após morrer o cliente Docker: **[nao verificado]** pelo material permitido. (`bin/reinicio_sob_admissao.sh:66`; `bin/reinicio_sob_admissao.sh:67`; `bin/reinicio_sob_admissao.sh:70`) |

**O descarte de F3 é válido no sentido específico:** segunda varredura vazia sob a trava permite prosseguir, mesmo após consumir o tempo de espera. Isso não resolve a classificação incompleta dos contraexemplos acima. (`bin/reinicio_sob_admissao.sh:44`; `bin/reinicio_sob_admissao.sh:45`; `bin/admissao_cron.sh:174`)

**Não há garantia de progresso do deploy.** Enquanto espera bloqueadores, ele mantém a admissão aberta; crons sobrepostos podem impedir uma oportunidade livre em tentativas sucessivas. Marca legada também aceita mera existência do PID, podendo conservar um bloqueador após reuso. Recusa efetivamente permanente em PROD: **[nao verificado]**. (`bin/reinicio_sob_admissao.sh:29`; `bin/reinicio_sob_admissao.sh:46`; `bin/admissao_cron.sh:158`; `bin/admissao_cron.sh:160`)

O agendado limita-se, por padrão, a três novas tentativas e depois desagenda; entretanto, a aquisição da trava da régua não tem prazo. Assim há tanto término recusado sem nova agenda quanto possibilidade de espera indefinida nessa outra trava. (`bin/deploy_agendado.sh:99`; `bin/deploy_agendado.sh:105`; `bin/deploy_agendado.sh:113`; `bin/deploy_agendado.sh:120`; `bin/deploy_agendado.sh:121`)

O teto medido pelo selo também cobre um universo menor que o bloqueado: a comparação de duração considera C/ENC e exclui RAWs, embora o censo classifique `noturno.py=1`. O JSON versionado contém 2518 para ele, contra teto 1500; isso **não prova bloqueio eterno**, mas impede usar o caso 10 como prova de suficiência para todo bloqueador. Duração atual em PROD: **[nao verificado]**. (`bin/tests/test_deploy_nao_mata_elo.sh:271`; `bin/tests/test_deploy_nao_mata_elo.sh:325`; `app/config/crons_duracao.json:51`; `bin/admissao_cron.sh:35`)

**2. Quem escreve, quem lê e onde há concorrência**

| Estado | Escritores/mutadores | Leitores e decisões |
|---|---|---|
| **Marcas `logs/crons_em_curso/*`** | `adm_marcar` publica temporário por `mv`; runner, descoberta e alarme chamam essa porta. `adm_desmarcar` remove; varredura remove órfãs. (`bin/admissao_cron.sh:207`; `bin/admissao_cron.sh:215`; `bin/admissao_cron.sh:222`; `bin/admissao_cron.sh:196`; `bin/cron_run.sh:16`; `bin/cron_run.sh:52`; `bin/encadeado.sh:48`) | Identidade por PID/starttime/exe; bloqueio por `casca`, legado ou árvore; exceção ancestral; restarter consome `adm_seguram`. (`bin/admissao_cron.sh:155`; `bin/admissao_cron.sh:169`; `bin/admissao_cron.sh:193`; `bin/reinicio_sob_admissao.sh:40`) |
| **`.admissao`, flock e fd 8** | Cron abre/adquire/fecha antes do comando; restarter abre/adquire e conserva até sair. (`bin/admissao_cron.sh:210`; `bin/admissao_cron.sh:211`; `bin/admissao_cron.sh:218`; `bin/reinicio_sob_admissao.sh:28`; `bin/reinicio_sob_admissao.sh:43`; `bin/reinicio_sob_admissao.sh:73`) | Os mesmos participantes disputam a trava. Docker restart e provas de subida recebem fd 8 fechado. O comentário “filho nenhum” excede o código: `date` e o `tr` das pipelines não têm fechamento explícito. (`bin/reinicio_sob_admissao.sh:40`; `bin/reinicio_sob_admissao.sh:60`; `bin/reinicio_sob_admissao.sh:63`; `bin/reinicio_sob_admissao.sh:66`; `bin/reinicio_sob_admissao.sh:69`) |
| **`restarts.log`** | Restarter escreve `ini -` antes do ato e `ini fim` depois, inclusive falhas. (`bin/reinicio_sob_admissao.sh:64`; `bin/reinicio_sob_admissao.sh:65`; `bin/reinicio_sob_admissao.sh:67`) | Contador lê e monta janelas; selo inspeciona registros. (`bin/rc137_no_restart.sh:45`; `bin/rc137_no_restart.sh:60`; `bin/tests/test_deploy_nao_mata_elo.sh:359`) |
| **Placar: `exit`, `rodou`, `dur`, `ini`, `fim`** | Escritor produtivo localizado: runner. `rodou=false` antes da decisão; `true` antes de executar; publicação única depois. (`bin/cron_run.sh:20`; `bin/cron_run.sh:23`; `bin/cron_run.sh:29`) | Pipeline e medidor consultam `linha_nao_rodou`; vigia usa última corrida/exit; contador usa exit 137/fim. (`app/config/crons.py:1432`; `app/core/management/commands/pipeline_placar.py:72`; `bin/crons_duracao.py:54`; `app/core/services/crons_quebrados.py:42`; `app/core/services/crons_quebrados.py:46`; `bin/rc137_no_restart.sh:75`) |
| **`rc137_no_restart` e TOTAL** | Contador publica número ou `?`; placar host converte desconhecido em contador aceso e soma ao TOTAL. (`bin/rc137_no_restart.sh:88`; `bin/rc137_no_restart.sh:90`; `bin/placar_code.sh:461`; `bin/placar_code.sh:537`) | O placar host consome a saída do contador, sem recalcular a correlação. (`bin/placar_code.sh:459`) |
| **`crons_duracao.json`** | Medidor agrega amostras, preserva valores antigos sem amostra e escreve o JSON; `crons.sh check --medir` o dispara. (`bin/crons_duracao.py:90`; `bin/crons_duracao.py:107`; `bin/crons_duracao.py:111`; `bin/crons.sh:13`) | Configuração carrega `DUR_MAX_S`; agenda derivada e contratos usam esses valores. (`app/config/crons.py:154`; `app/config/crons.py:160`; `app/config/crons.py:241`; `app/chamados/tests/test_contract_crons.py:402`) |
| **`logs/fim/*`** | Runner carimba rc 0/2. **Existe outro escritor fora do diff:** selo antigo fabrica carimbos e também os apaga na raiz medida. (`bin/cron_run.sh:38`; `bin/cron_run.sh:41`; `bin/tests/test_furo_encadeado_ao_cartorio.sh:15`; `bin/tests/test_furo_encadeado_ao_cartorio.sh:18`; `bin/tests/test_furo_encadeado_ao_cartorio.sh:49`) | Encadeado decide idempotência e liberação dos dependentes pela existência. (`bin/encadeado.sh:87`; `bin/encadeado.sh:97`) |
| **Flags de cron quebrado** | Runner cria/remove após sucesso de `_alarme`; invocações do mesmo nome/empresa compartilham destino, sem trava específica dessa flag. (`bin/cron_run.sh:62`; `bin/cron_run.sh:65`; `bin/cron_run.sh:67`) | Runner usa a flag para suprimir novo aviso ou pedir recuperação; command delega a abertura/fechamento ao serviço. (`bin/cron_run.sh:65`; `bin/cron_run.sh:66`; `app/core/management/commands/cron_quebrado.py:35`; `app/core/management/commands/cron_quebrado.py:37`) |
| **Recusa 75 e arquivo por id** | Restarter retorna 75; deploy grava id/argv e propaga 75; sucesso remove sua recusa. Agendado também remove pendências durante consumo. (`bin/reinicio_sob_admissao.sh:43`; `bin/reinicio_sob_admissao.sh:50`; `bin/deploy.sh:285`; `bin/deploy.sh:288`; `bin/deploy.sh:361`; `bin/deploy_agendado.sh:102`) | Agendado valida id e recupera argv; nas novas chamadas confere também rc. (`bin/deploy_agendado.sh:83`; `bin/deploy_agendado.sh:101`; `bin/deploy_agendado.sh:107`; `bin/deploy_agendado.sh:111`) |

O segundo escritor de `logs/fim` é material para este escopo: a régua executa todos os selos de host, e esse selo não usa raiz temporária para os carimbos. Também apaga o arquivo da trava do encadeado; remover seu pathname enquanto outro processo mantém o inode aberto permite travas sobre arquivos distintos. Execução desse caminho na árvore viva: **[nao verificado]**. (`bin/regua.sh:138`; `bin/regua.sh:140`; `bin/tests/test_furo_encadeado_ao_cartorio.sh:15`; `bin/tests/test_furo_encadeado_ao_cartorio.sh:18`; `bin/encadeado.sh:83`)

**Régua e pós-push não consomem a recusa 75 diretamente.** A régua disputa sua trava e executa selos; pós-push deriva/commita TICKETS. A cadeia molde pode propagar a falha por `set -e`; `abono-no-ar` traduz falha do deploy para 1. (`bin/regua.sh:100`; `bin/regua.sh:138`; `bin/pos_push.sh:18`; `bin/pos_push.sh:30`; `bin/molde_fatia/cadeia.sh:4`; `bin/molde_fatia/cadeia.sh:89`; `fatias_agendadas/abono-no-ar/esteira.sh:38`)

**Há um defeito concreto na integração da repetição:** `abono-no-ar` interrompe no deploy recusado, antes do backfill e da medição. O agendado repete somente o deploy, substitui `rc` pelo novo resultado e publica seu `.done`. Portanto pode registrar rc 0 com essa cauda ainda não executada. Agendamento produtivo atual dessa esteira: **[nao verificado]**. (`fatias_agendadas/abono-no-ar/esteira.sh:37`; `fatias_agendadas/abono-no-ar/esteira.sh:45`; `fatias_agendadas/abono-no-ar/esteira.sh:52`; `bin/deploy_agendado.sh:105`; `bin/deploy_agendado.sh:106`; `bin/deploy_agendado.sh:119`)

Não encontrei regra produtiva paralela para “não rodou” nos dois consumidores dessa pergunta: ambos consultam o predicado compartilhado. Vigia e contador fazem perguntas diferentes por exit. O histórico sem `rodou` é deliberadamente contado como corrida; isso preserva compatibilidade, mas não recupera proveniência perdida. Quantidade histórica afetada: **[nao verificado]**. (`app/config/crons.py:1433`; `app/core/management/commands/pipeline_placar.py:72`; `bin/crons_duracao.py:85`; `app/core/services/crons_quebrados.py:46`; `bin/rc137_no_restart.sh:75`)

**3. Curas da volta 4: classe ou caso**

| Cura | Veredito |
|---|---|
| **F1 — admissão do alarme** | **Fecha os dois caminhos atuais do runner:** abertura e recuperação usam `_alarme`, que pede admissão própria antes do exec; recusa preserva a flag. Não fecha os limites dinâmico/ancestral. (`bin/cron_run.sh:52`; `bin/cron_run.sh:57`; `bin/cron_run.sh:65`; `bin/cron_run.sh:67`) |
| **F2 — valor decimal `10#`** | Fecha zero com zeros à esquerda e octal nesse teto: restart só recebe valor entre 1 e o declarado. Não é validação de inteiro arbitrário: overflow ainda altera `ESPERA_CRON_S`; por exemplo, `9223372036854775808` pode virar negativo e antecipar recusa. `ADM_PASSO_S` continua validado por grafia e usado em aritmética sem normalização decimal. (`bin/admissao_cron.sh:50`; `bin/admissao_cron.sh:59`; `bin/reinicio_sob_admissao.sh:21`; `bin/reinicio_sob_admissao.sh:54`) |
| **F4 — ilegível vira `?`** | Fecha JSON **sintaticamente inválido e não vazio**. Não valida o contrato inteiro: `{}`, `[]` ou `{"exit":"137"}` são descartados e podem resultar zero. Essas formas produzidas pelo runner atual: **[nao verificado]**; seu escritor publica exit numérico e campos completos. (`bin/rc137_no_restart.sh:69`; `bin/rc137_no_restart.sh:75`; `bin/cron_run.sh:29`) |
| **F6 — monotônico no encadeado** | Fecha o retrocesso civil na espera normal, mas cria `_mono` próprio em vez de consultar `adm_agora`; a leitura falha sem propagação explícita, e permanece o sleep fixo de 10 s. Assim não prova prazo absoluto de retorno nem fechamento da classe “relógio fora da fonte”. (`bin/encadeado.sh:93`; `bin/encadeado.sh:99`; `bin/encadeado.sh:104`; `bin/admissao_cron.sh:70`) |
| **Caso 17** | Morde alarme direto sem marca, zeros/octal, JSON truncado sem 137 e janela fixada em constante antiga. Contudo, não força recusa do alarme: o runner nasce antes, enquanto o prazo de 1 s é passado somente ao restarter. Tampouco exercita recuperação de um nome previamente quebrado. (`bin/tests/test_deploy_nao_mata_elo.sh:498`; `bin/tests/test_deploy_nao_mata_elo.sh:505`; `bin/tests/test_deploy_nao_mata_elo.sh:508`; `bin/tests/test_deploy_nao_mata_elo.sh:520`; `bin/tests/test_deploy_nao_mata_elo.sh:528`; `bin/tests/test_deploy_nao_mata_elo.sh:536`) |

**4. Mutantes sobreviventes e o vermelho do caso 11**

Há candidatos com independência estática do selo; sobrevivência integral executada permanece **[nao verificado]**:

| Mutante plausível | Lacuna |
|---|---|
| Remover o filtro de recusas de `pipeline_placar.medidos` | O selo executa o medidor, sem executar o pipeline. O teste Django separado o atacaria. (`app/core/management/commands/pipeline_placar.py:72`; `bin/tests/test_deploy_nao_mata_elo.sh:409`; `app/chamados/tests/test_contract_crons.py:199`) |
| Reverter `_mono` do encadeado para `date +%s` | O caso 7 usa `--corrente`; o fake não retorna elos e o fluxo termina antes da espera normal. Confirma o INCOMPLETO F6 declarado. (`bin/tests/test_deploy_nao_mata_elo.sh:58`; `bin/tests/test_deploy_nao_mata_elo.sh:164`; `bin/encadeado.sh:59`; `bin/encadeado.sh:93`) |
| Restaurar exec direto apenas em `--recuperou` | Caso 17 não faz corrida bem sucedida posterior do mesmo cron quebrado. (`bin/cron_run.sh:67`; `bin/tests/test_deploy_nao_mata_elo.sh:498`; `bin/tests/test_deploy_nao_mata_elo.sh:505`) |
| Descartar também corrida real com exit 70 no medidor | Caso 15 fornece corrida real 0 e 75, sem corrida real 70. (`bin/crons_duracao.py:85`; `bin/tests/test_deploy_nao_mata_elo.sh:397`; `bin/tests/test_deploy_nao_mata_elo.sh:398`) |

**O caso 11 depende comprovadamente de fora de `bin/`.** Importa a configuração e percorre RAWs na árvore medida, incluindo `eval/` e `mensageria/bin/`. Os execs relevantes estão nesses arquivos ou em filhos nomeados. (`bin/tests/test_deploy_nao_mata_elo.sh:329`; `bin/tests/test_deploy_nao_mata_elo.sh:331`; `bin/tests/test_deploy_nao_mata_elo.sh:335`; `eval/noturno.py:32`; `eval/gerador.py:65`; `mensageria/bin/sentinela_diario.sh:16`; `mensageria/bin/negou_fogo_diario.sh:10`)

Portanto, uma montagem incompleta pode explicar os três RAWs virando zero em todos os mutantes; **a causa concreta dessa montagem está [nao verificado]**. O log registra também falhas específicas do caso 17 para cada mutação, além do vermelho comum do censo; não é correto atribuir toda a mordida somente ao caso 11. (`v4_mutantes.log:3`; `v4_mutantes.log:4`; `v4_mutantes.log:9`; `v4_mutantes.log:10`; `v4_mutantes.log:17`; `v4_mutantes.log:22`, anexo)

O selo integral tem outra dependência externa: caso 10 recorre ao placar no caminho produtivo quando não encontra amostra na árvore medida. Seu resultado não é determinado apenas pelos arquivos versionados. (`bin/tests/test_deploy_nao_mata_elo.sh:267`; `bin/tests/test_deploy_nao_mata_elo.sh:273`)

**5. Classes que já reprovaram a obra**

| Classe | Presente ou ausente |
|---|---|
| **Segundo juiz duplicando registrado** | **Não identificado no delta.** A pergunta nova de proveniência tem uma definição compartilhada. Ausência global de duplicações: **[nao verificado]**. (`app/config/crons.py:1432`; `app/core/management/commands/pipeline_placar.py:72`; `bin/crons_duracao.py:54`) |
| **Segundo escritor fora da porta** | **Presente no escopo ampliado:** selo antigo fabrica/apaga `logs/fim`; não foi introduzido por este diff. No placar, escritor produtivo localizado permanece o runner. (`bin/tests/test_furo_encadeado_ao_cartorio.sh:18`; `bin/tests/test_furo_encadeado_ao_cartorio.sh:49`; `bin/cron_run.sh:29`) |
| **Relógio lido fora da borda** | **Presente:** `_mono` próprio; medidor consulta hoje em dois pontos; pipeline consulta hora e minuto separadamente. Isso não torna toda janela civil incorreta, mas o descarte não demonstra centralização temporal. (`bin/encadeado.sh:93`; `bin/crons_duracao.py:66`; `bin/crons_duracao.py:108`; `app/core/management/commands/pipeline_placar.py:124`) |
| **Derivação paralela da fonte única** | **Parcialmente presente:** pior caso do restart agora deriva corretamente da biblioteca; `_mono` duplica sua fonte e a lista literal do restart continua paralela a `ADM_CASCAS`, conciliada por selo. (`bin/rc137_no_restart.sh:25`; `bin/admissao_cron.sh:67`; `bin/encadeado.sh:93`; `bin/reinicio_sob_admissao.sh:66`; `bin/tests/test_deploy_nao_mata_elo.sh:285`) |
| **Selo/RED sem mordida suficiente** | **Presente:** mutantes acima; caso 14 presume a fase por sleep, sem confirmação. Um deploy atrasado pode enxergar a marca já na primeira varredura e deixar passar a remoção da segunda. (`bin/tests/test_deploy_nao_mata_elo.sh:382`; `bin/tests/test_deploy_nao_mata_elo.sh:384`; `bin/tests/test_deploy_nao_mata_elo.sh:387`) |
| **Ordem de trava** | **Correta entre participantes migrados**, com publicação e segunda varredura sob a mesma trava. Os contraexemplos restantes vêm de classificação/isênções; o `flock` da régua continua sem prazo. (`bin/admissao_cron.sh:211`; `bin/reinicio_sob_admissao.sh:43`; `bin/reinicio_sob_admissao.sh:44`; `bin/deploy_agendado.sh:105`) |
| **Pouso antes do pré-requisito** | Migration e registro precedem o restart. **Há violação no consumidor de repetição:** sucesso do deploy pode substituir conclusão da esteira sem backfill/medição. Pouso efetivo desta obra: **[nao verificado]**. (`bin/deploy.sh:124`; `bin/deploy.sh:282`; `bin/reinicio_sob_admissao.sh:64`; `fatias_agendadas/abono-no-ar/esteira.sh:45`; `bin/deploy_agendado.sh:119`) |

O descarte do contrato textual não o transforma em prova comportamental: uma linha que apenas imprima o texto esperado satisfaz sua busca. O comando atual existe, mas a verificação continua textual. (`bin/contrato_deploy.sh:25`; `bin/contrato_deploy.sh:28`; `bin/reinicio_sob_admissao.sh:66`)

O descarte de `manual` compartilhado também não refuta a colisão: chamadas sem id usam o mesmo arquivo e o mesmo `.novo`, e um sucesso remove a recusa desse id. Os ids próprios do agendado resolvem seu caminho específico. (`bin/deploy.sh:52`; `bin/deploy.sh:54`; `bin/deploy.sh:285`; `bin/deploy.sh:361`; `bin/deploy_agendado.sh:94`)

**6. Efeito imediato fora do deploy** [nao verificado]

**Sim, o comportamento dos próximos crons muda antes de qualquer restart.** O caminho instalado chama o runner, que passa a carregar a biblioteca e pedir admissão em cada invocação. (`app/config/crons.py:1423`; `app/config/crons.py:1443`; `app/config/crons.py:1800`; `bin/cron_run.sh:15`; `bin/cron_run.sh:16`)

- **Pode deixar de executar:** falha de marca retorna 70; espera excessiva na admissão retorna 75. O comando principal não roda, há `rodou=false`, stderr e nenhum carimbo de fim. Isso vale também para cron somente 

[truncado em 20000 de 21097 caracteres]

## 04/11 08:06 · sessao 6a17eb482ad9 · - · - · origem=Ronald (tmux)

Achados da admissão, somente por leitura; execução integral dos cenários e ocorrência produtiva permanecem **[nao verificado]**.

1. **A classe “cron dinâmico entra durante restart” continua aberta.** O runner aceita `CMD='eval "$HCMD"'`, com `HCMD='sleep 2; docker exec saas_core sleep 30'`. O classificador recebe apenas o texto do CMD e arquivos nomeados, sem expandir HCMD; a marca pode nascer `casca=0`. Enquanto o filho está em `sleep`, nenhuma varredura encontra `docker exec`, as duas passam e o restart começa. O futuro exec roda sem consultar novamente a admissão. O teste existente verifica o eval já dentro do exec, não antes dele. RAW instalado com esse comportamento: **[nao verificado]**. (`bin/admissao_cron.sh:108`, `bin/admissao_cron.sh:130`, `bin/admissao_cron.sh:145`, `bin/admissao_cron.sh:169`, `bin/admissao_cron.sh:212`; `bin/cron_run.sh:24`; `bin/reinicio_sob_admissao.sh:40`, `bin/reinicio_sob_admissao.sh:44`, `bin/reinicio_sob_admissao.sh:66`; `bin/tests/test_deploy_nao_mata_elo.sh:112`)

2. **A isenção ancestral ainda é mais ampla que o caso curado.** `adm_varrer` ignora toda marca ancestral antes de consultar seus filhos. Contraexemplo admitido pelo runner: `docker exec saas_core sleep 30 & bash <raiz>/bin/reinicio_sob_admissao.sh; wait`. O exec irmão vivo não segura o restart. O consumidor instalado examinado, vigia_logins, chama o deploy sincronamente; irmão concorrente nesse RAW permanece **[nao verificado]**. (`bin/admissao_cron.sh:193`; `bin/cron_run.sh:24`; `bin/vigia_logins.sh:71`)

3. **D4b não foi tecnicamente refutado.** A ausência do PID do envelope torna a marca órfã e leva à remoção, sem procurar filhos que sobreviveram e foram reparentados. A morte isolada do wrapper em produção continua **[nao verificado]**. (`bin/admissao_cron.sh:158`, `bin/admissao_cron.sh:196`; `bin/cron_run.sh:24`)

4. **O protocolo funciona para marca válida, `casca=1`, não ancestral, com envelope vivo.** A publicação acontece sob flock, a mesma trava é adquirida pelo restarter e a segunda varredura ocorre antes do restart; se ainda houver bloqueador no teto, sai 75. Isso valida o descarte de F3: segunda varredura vazia sob a trava permite restart, mesmo que a espera anterior tenha consumido o teto. (`bin/admissao_cron.sh:211`, `bin/admissao_cron.sh:215`, `bin/admissao_cron.sh:218`; `bin/reinicio_sob_admissao.sh:43`, `bin/reinicio_sob_admissao.sh:44`, `bin/reinicio_sob_admissao.sh:45`, `bin/reinicio_sob_admissao.sh:48`)

5. **F1 fecha o caminho específico do alarme do runner.** Tanto abertura quanto recuperação passam por `_alarme`, que remove a marca anterior, pede admissão para comando literal de casca, executa e remove a nova marca; recusa preserva a flag. Isso não corrige os contraexemplos dinâmico/ancestral nem transforma falha do alarme em linha própria do placar. (`bin/cron_run.sh:49`, `bin/cron_run.sh:51`, `bin/cron_run.sh:52`, `bin/cron_run.sh:57`, `bin/cron_run.sh:58`, `bin/cron_run.sh:65`, `bin/cron_run.sh:67`)

6. **F2 cura `00`, `000` e `08`; não valida inteiros arbitrários sem overflow.** Executei apenas funções em memória: `ADM_RESTART_TIMEOUT_S=00` retorna120, `08` retorna8; `ESPERA_CRON_S=18446744073709551617` retorna1 e `9223372036854775808` retorna número negativo por overflow. Isso pode provocar recusa prematura, embora o teste `>=1` impeça prazo negativo/zero no restart. `ADM_PASSO_S` continua validado por grafia: `00` passa e `08` alcança aritmética octal; o monotônico impede que passo zero sozinho torne o laço eterno. (`bin/admissao_cron.sh:50`, `bin/admissao_cron.sh:59`; `bin/reinicio_sob_admissao.sh:21`, `bin/reinicio_sob_admissao.sh:48`, `bin/reinicio_sob_admissao.sh:54`)

7. **F6 corrige o relógio civil da espera, mas duplica a leitura em `_mono`.** O normal de encadeado não usa `adm_agora`: lê `/proc/uptime` em função própria; essa função não propaga falha da leitura. O teto continua sujeito à passagem completa das verificações e a `sleep 10`; relógio monotônico não significa limite absoluto de retorno. Falha real de `/proc/uptime`: **[nao verificado]**. (`bin/encadeado.sh:93`, `bin/encadeado.sh:94`, `bin/encadeado.sh:99`, `bin/encadeado.sh:100`, `bin/encadeado.sh:104`; `bin/admissao_cron.sh:70`)

8. **Há ausência de pós-condição no caminho falho do restart.** Timeout/falha escreve fim e sai imediatamente, soltando a trava; falha da subida também sai. O código não verifica nesse ramo que as operações sobre as cascas terminaram. Sobrevida de operação no daemon depois da morte do cliente Docker é **[nao verificado]** pelo material permitido; não afirmar como fato de produção. (`bin/reinicio_sob_admissao.sh:66`, `bin/reinicio_sob_admissao.sh:67`, `bin/reinicio_sob_admissao.sh:69`, `bin/reinicio_sob_admissao.sh:70`)

9. **Não há garantia de progresso do deploy.** Novos crons continuam admitidos durante sua espera; bloqueadores sobrepostos podem manter a recusa em tentativas sucessivas. Marca legada sem starttime aceita qualquer PID existente, podendo sobreviver vinculada a PID reutilizado. Recusa efetivamente permanente em produção: **[nao verificado]**. (`bin/reinicio_sob_admissao.sh:29`, `bin/reinicio_sob_admissao.sh:46`, `bin/reinicio_sob_admissao.sh:48`; `bin/admissao_cron.sh:158`, `bin/admissao_cron.sh:160`, `bin/admissao_cron.sh:161`, `bin/admissao_cron.sh:211`)

10. **O comportamento fora do deploy muda imediatamente nas próximas invocações do runner.** Todo cron agora pode aguardar a trava ou não executar seu comando por rc70/75; isso gera `rodou=false`, nova linha do placar, stderr e nenhum carimbo fim. A descoberta recusada da corrente encerra com2 e não percorre os elos. A admissão do alarme pode adicionar espera após a publicação do placar/carimbo, preservando o RC original. Nenhum novo laço de repetição do comando principal aparece no runner; duplicação concreta produtiva permanece **[nao verificado]**. (`bin/cron_run.sh:15`, `bin/cron_run.sh:16`, `bin/cron_run.sh:20`, `bin/cron_run.sh:24`, `bin/cron_run.sh:26`, `bin/cron_run.sh:29`, `bin/cron_run.sh:38`, `bin/cron_run.sh:52`, `bin/cron_run.sh:70`; `bin/encadeado.sh:48`, `bin/encadeado.sh:59`, `bin/encadeado.sh:61`, `bin/encadeado.sh:65`; `app/config/crons.py:1423`, `app/config/crons.py:1443`, `app/config/crons.py:1800`)

11. **RAWs examinados:** sentinela tem quatro execs sequenciais e justifica `casca=1` pelo run inteiro; backup_all/backup_tenant usam serviço db, fora das três cascas; tickets_rodape executa Git e Python host. Vigia_logins tem deploy síncrono; noturno chama filhos Python que o fecho consegue alcançar pelos nomes literais. Não executei fecho que pudesse resolver fontes absolutas fora da cópia. (`mensageria/bin/sentinela_diario.sh:14`, `mensageria/bin/sentinela_diario.sh:16`; `scripts/backup_all.sh:5`, `scripts/backup_all.sh:7`; `scripts/backup_tenant.sh:10`, `scripts/backup_tenant.sh:12`; `bin/tickets_rodape.sh:20`, `bin/tickets_rodape.sh:54`; `bin/vigia_logins.sh:71`; `eval/noturno.py:32`, `eval/noturno.py:36`, `eval/noturno.py:43`, `eval/noturno.py:93`)

## 04/11 08:05 · sessao 323413e04117 · - · - · origem=Ronald (tmux)

Achados para consolidar, todos por leitura; execução integral de selo e mutantes **[nao verificado]**.

1. **Retry pode declarar fatia concluída sem executar sua cauda.** `abono-no-ar` usa `pipefail`, transforma qualquer falha do deploy em `exit 1` e para antes do backfill e do contador. O agendado reconhece o arquivo da recusa, repete somente `deploy.sh`, substitui `rc` pelo resultado dessa chamada e publica `.done`, sem retomar a esteira. Assim uma recusa seguida de deploy bem sucedido pode produzir resultado zero com o backfill ausente. Disparo atual dessa esteira em produção: **[nao verificado]**. (`fatias_agendadas/abono-no-ar/esteira.sh:21,37,38,45,48,52,67`; `bin/deploy_agendado.sh:99,105,106,116,119`)

2. **Consumidores da recusa e progresso.** O agendado confere o primeiro campo `id=...`, cria um id por tentativa, recupera o argv e valida arquivo/rc nas chamadas diretas. Faz três novas tentativas por padrão; ao esgotar, retorna 75, remove pendência e desagenda o cron. Entretanto, seu `flock` da régua não tem prazo e pode esperar indefinidamente. A ocorrência produtiva de bloqueio indefinido é **[nao verificado]**. (`bin/deploy_agendado.sh:83,87,94,99,101,104,105,107,110,111,113,120,121,123`)

3. **Régua e pós-push não consomem 75 diretamente.** A régua disputa `/tmp/regua.lock` e roda os selos; não chama deploy. O pós-push busca origin, deriva o TICKETS e commita; não chama deploy nem lê recusa. `push.sh` só chama pós-push depois de push zero. A cadeia molde, sob `set -e`, chama deploy e pode propagar sua falha; `abono-no-ar` a traduz explicitamente para 1. (`bin/regua.sh:16,97,100,138,140`; `bin/pos_push.sh:18,19,20,29,30`; `bin/push.sh:6,8,9`; `bin/molde_fatia/cadeia.sh:4,89`; `fatias_agendadas/abono-no-ar/esteira.sh:37,38`)

4. **Mutantes plausíveis sobreviventes ao selo específico:**
   - Reverter `_mono` de `encadeado.sh` para `date +%s`. O selo só executa o encadeado em `--corrente`; o fake Docker não emite elos, portanto sai antes do laço que usa `_mono`. O grep posterior só verifica o comando final. (`bin/encadeado.sh:59,61,93,99`; `bin/tests/test_deploy_nao_mata_elo.sh:54,58,59,164,295`)
   - Remover o filtro `linha_nao_rodou` do pipeline. O selo mede `crons_duracao.py`, sem importar ou executar `pipeline_placar`; o teste Django é uma proteção separada. (`app/core/management/commands/pipeline_placar.py:72`; `bin/tests/test_deploy_nao_mata_elo.sh:409,411,416`; `app/chamados/tests/test_contract_crons.py:199`)
   - Restabelecer `docker exec ... --recuperou` direto somente no ramo de recuperação do runner. O código atual protege falha e recuperação pela mesma `_alarme`, mas o caso 17 cria flags e não faz depois uma corrida bem sucedida do mesmo nome; o caso 15 também termina em recusa depois da falha real. A execução desses mutantes permanece **[nao verificado]**. (`bin/cron_run.sh:65,67`; `bin/tests/test_deploy_nao_mata_elo.sh:397,398,401,498,500,505,508`)

5. **Caso 17 não força a recusa do alarme.** O runner `morre17b` nasce antes, sem `ADM_ESPERA_ADMISSAO_S=1`; esse valor é passado somente ao restarter. A biblioteca do runner já capturou seu prazo padrão 900. Normalmente o teste exercita esperar a abertura e executar marcado; o ramo que exige alarme recusado não tem fixture determinística. (`bin/admissao_cron.sh:42`; `bin/tests/test_deploy_nao_mata_elo.sh:505,508,511,515,517`)

6. **Caso 11 depende comprovadamente de fora de `bin/`.** Importa `app/config/crons.py`, troca HOME pela árvore medida e percorre os arquivos nomeados. RAWs apontam para `mensageria/bin/sentinela_diario.sh`, `mensageria/bin/negou_fogo_diario.sh` e `eval/noturno.py`; seus execs estão nesses arquivos ou filhos. Logo uma montagem com apenas `bin/` pode alterar o censo. A montagem concreta que produziu `v4_mutantes.log` é **[nao verificado]**; o log sozinho não prova sua causa. As falhas específicas 17 registradas para m1–m4 continuam distinguindo as mutações além do ruído comum do caso 11. (`bin/tests/test_deploy_nao_mata_elo.sh:325,327,329,331,334,335`; `bin/admissao_cron.sh:114,115,123,124,125,138`; `app/config/crons.py:575,576,669,686`; `mensageria/bin/sentinela_diario.sh:16`; `mensageria/bin/negou_fogo_diario.sh:10,11`; `eval/noturno.py:32,36,43,93`; `eval/gerador.py:65,84`; `eval/harness.py:45,54`; `eval/juiz.py:241`; `eval/investigador.py:24`)

7. **Descartes sobre contrato e caso 14 não constituem refutação técnica.** O contrato procura texto contendo `docker restart`, não prova execução desse comando; o caso 14 presume por `sleep 0.5` que o deploy já chegou à espera da trava, sem handshake dessa fase. Um deploy atrasado pode ver a marca na primeira varredura e deixar passar um mutante sem a segunda. (`bin/contrato_deploy.sh:25,28`; `bin/tests/test_deploy_nao_mata_elo.sh:379,382,384,387,391`)

## 04/11 08:05 · sessao 82f428d196e5 · - · - · origem=Ronald (tmux)

- **Placar: escritor produtivo localizado único** `bin/cron_run.sh:29`; escreve `exit`, `dur`, `ini`, `fim`, `rodou`. Proveniência nasce `false`, vira `true` antes de executar: `bin/cron_run.sh:20`, `bin/cron_run.sh:23`, `bin/cron_run.sh:24`. Fixtures também escrevem placares, em arquivos temporários: `bin/tests/test_medicao_de_cron_nao_expira.sh:31`, `app/core/tests/test_crons_quebrados.py:33`.
- **Leitores de corrida/duração consultam a função compartilhada** `app/config/crons.py:1432`, `app/config/crons.py:1433`: pipeline em `app/core/management/commands/pipeline_placar.py:72`; medidor em `bin/crons_duracao.py:54`, `bin/crons_duracao.py:85`. Ausência histórica de `rodou` conta como corrida pelo próprio predicado: `app/config/crons.py:1433`. Quantidade histórica de recusas sem campo: **[nao verificado]**.
- **Outras perguntas sobre a linha:** vigia escolhe corrida mais recente por `ini` e alarma saída diferente de 0/2, incluindo recusa: `app/core/services/crons_quebrados.py:36`, `app/core/services/crons_quebrados.py:39`, `app/core/services/crons_quebrados.py:42`, `app/core/services/crons_quebrados.py:46`. Comando consumidor: `app/core/management/commands/cron_quebrado.py:25`, `app/core/management/commands/cron_quebrado.py:29`; host chama em `bin/placar_code.sh:453`. Contador de morte cruza `exit=137`/`fim` com restarts: `bin/rc137_no_restart.sh:75`, `bin/rc137_no_restart.sh:78`, `bin/rc137_no_restart.sh:81`. Esses leitores não duplicam a pergunta “rodou”: respondem quebra e correlação temporal, respectivamente: mesmos sítios.
- **`crons_duracao.json`:** medidor lê placar, filtra proveniência, agrega p95, preserva valores antigos sem amostra, escreve arquivo: `bin/crons_duracao.py:73`, `bin/crons_duracao.py:85`, `bin/crons_duracao.py:90`, `bin/crons_duracao.py:94`, `bin/crons_duracao.py:107`, `bin/crons_duracao.py:111`. Gatilho produtivo é `crons.sh check --medir`: `bin/crons.sh:13`, `bin/crons.sh:14`; erro apenas avisa e segue: `bin/crons.sh:15`. Configuração lê JSON e monta `DUR_MAX_S`: `app/config/crons.py:146`, `app/config/crons.py:154`, `app/config/crons.py:159`, `app/config/crons.py:160`; agenda derivada consome em `app/config/crons.py:241`, `app/config/crons.py:248`. Contratos também consomem: `app/chamados/tests/test_contract_crons.py:316`, `app/chamados/tests/test_contract_crons.py:342`, `app/chamados/tests/test_contract_crons.py:402`.
- **`logs/fim`: existe segundo escritor fora do runner.** Runner grava no rc0/2: `bin/cron_run.sh:38`, `bin/cron_run.sh:41`; encadeado decide idempotência/dependência pela existência: `bin/encadeado.sh:87`, `bin/encadeado.sh:97`. Entretanto, `bin/tests/test_furo_encadeado_ao_cartorio.sh:15` resolve a raiz da árvore, apaga carimbos reais dessa raiz e `/tmp/encadeado_apurar_furos_diarios.lock` em `bin/tests/test_furo_encadeado_ao_cartorio.sh:18`, inclusive via trap e na entrada: `bin/tests/test_furo_encadeado_ao_cartorio.sh:20`, `bin/tests/test_furo_encadeado_ao_cartorio.sh:22`; fabrica três carimbos em `bin/tests/test_furo_encadeado_ao_cartorio.sh:49`, `bin/tests/test_furo_encadeado_ao_cartorio.sh:50`, `bin/tests/test_furo_encadeado_ao_cartorio.sh:51`. Execução produtiva desse selo após a obra: **[nao verificado]**.
- **`restarts.log` e contador:** escritor é o restarter antes e depois do ato: `bin/reinicio_sob_admissao.sh:64`, `bin/reinicio_sob_admissao.sh:65`. Leitor é `bin/rc137_no_restart.sh:45`; janela sem fim vem da biblioteca em `bin/rc137_no_restart.sh:25`. Placar host transforma desconhecido em contador aceso e inclui no TOTAL: `bin/placar_code.sh:459`, `bin/placar_code.sh:461`, `bin/placar_code.sh:537`.
- **F4 fecha JSON sintaticamente ilegível não vazio, não valida todo o contrato da linha.** JSON válido sem `exit`, uma lista `[]`, ou `exit:"137"` são silenciosamente descartados e podem produzir zero: `bin/rc137_no_restart.sh:75`. Executei exclusivamente o heredoc real em memória, substituindo `open` por `StringIO`: `{}`, `[]` e `{"exit":"137","fim":"2026-11-02T06:48:47-03:00"}` imprimiram `0`. Essas formas produzidas pelo runner atual: **[nao verificado]**; seu `printf` publica schema completo com saída numérica em `bin/cron_run.sh:29`.
- **Relógio fora da borda permanece nos leitores:** medidor consulta hoje duas vezes: `bin/crons_duracao.py:66`, `bin/crons_duracao.py:108`; pipeline consulta hoje e duas vezes o instante: `app/core/management/commands/pipeline_placar.py:122`, `app/core/management/commands/pipeline_placar.py:124`; vigia recebe `agora` ou consulta relógio: `app/core/services/crons_quebrados.py:27`. Encadeado agora mede espera por uptime, mas copia implementação em `_mono`, sem consultar `adm_agora`: `bin/encadeado.sh:93`; fonte compartilhada original em `bin/admissao_cron.sh:70`.
- **Mutante plausível fora de `bin/`:** retirar filtro `linha_nao_rodou` de `app/core/management/commands/pipeline_placar.py:72` transforma recusa em corrida/erro. O selo shell testa medidor em `bin/tests/test_deploy_nao_mata_elo.sh:409`, `bin/tests/test_deploy_nao_mata_elo.sh:418`, mas não executa pipeline; teste Django próprio o ataca em `app/chamados/tests/test_contract_crons.py:199`. Sobrevivência integral executada: **[nao verificado]**. Outro candidato: acrescentar descarte por `exit==70` em `bin/crons_duracao.py:85`; caso15 só mede corridas reais 0/75: `bin/tests/test_deploy_nao_mata_elo.sh:396`, `bin/tests/test_deploy_nao_mata_elo.sh:397`, `bin/tests/test_deploy_nao_mata_elo.sh:408`.

## 03/11 00:05 -- HISTORICO (mais de 24 h) · sessao d1798f3d1359 · DEPLOY-[nome]-ELO · verificador · origem=Code

**A volta 3 melhora a proteção, mas ainda não fecha a classe.** Restam um caminho real de `docker exec` fora da admissão, um desvio do teto por `ADM_RESTART_TIMEOUT_S=00` e descarte de placar ilegível que pode produzir zero. (`bin/cron_run.sh:49`; `bin/admissao_cron.sh:55`; `bin/rc137_no_restart.sh:69`)

Executei o contrato em leitura: retornou `contrato_deploy: OK`. Não executei o selo integral, pois ele cria e remove arquivos; suíte inteira e mutantes executados permanecem **[nao verificado]**. (`bin/contrato_deploy.sh:60`; `bin/tests/test_deploy_nao_mata_elo.sh:44`)

**1. V3-1..V3-7: classe ou caso**

| Item | Conclusão |
|---|---|
| **V3-1 — fecho/reconhecimento** | **Cura os formatos nomeados, não toda a classe.** Agora há fecho transitivo com vistos, caminhos relativos, continuação por `\`, opções de compose e `$var`. Porém o classificador recebe somente o comando principal: deixa fora o próprio runner, que também executa `docker exec`. (`bin/admissao_cron.sh:89,97,99,119,128,131,208`; `bin/cron_run.sh:49,51`) |
| **V3-2 — ancestral** | **Cura o autoimpedimento síncrono do vigia; a exceção é mais ampla.** Qualquer marca ancestral recebe `segura=0`, sem examinar outros filhos ainda usando casca. O vigia real chama deploy sincronamente; um exec irmão concorrente nesse caminho atual é **[nao verificado]**. (`bin/admissao_cron.sh:189`; `bin/vigia_logins.sh:71`) |
| **V3-3 — relógio/passo** | **Cura retrocesso civil e sono do passo inteiro; não garante teto absoluto do procedimento.** As varreduras não têm prazo e, se a segunda vier vazia, o script prossegue antes de conferir o tempo esgotado. (`bin/admissao_cron.sh:66`; `bin/reinicio_sob_admissao.sh:40,44,45,48,54`) |
| **V3-4 — teto/escalada** | **Parcial: há contraexemplo concreto.** A validação rejeita apenas a grafia `0`; aceita `00` e `000`, menores que 120, e entrega esse prazo zero ao `timeout`. Conferi a função e um comando inofensivo em memória: o timeout interno permaneceu até ser interrompido pelo externo. A escalada `-k` existe, mas depende de o prazo disparar. (`bin/admissao_cron.sh:55`; `bin/reinicio_sob_admissao.sh:25,66`) |
| **V3-5 — contrato leitor** | **Cura o leitor para a linha atual com `timeout`.** Continua sendo verificação textual: uma linha executável que apenas imprima `"docker restart saas_core saas_ui mensageria"` também satisfaria essa busca. Não demonstra execução nem reunião das cascas em um único ato. (`bin/contrato_deploy.sh:25,28`) |
| **V3-6 — contador** | **Cura os casos nomeados, não toda a classe “ilegível não vira zero”.** Janela deriva da biblioteca; fim invertido e JSON quebrado contendo `137` viram desconhecido. JSON quebrado antes de aparecer `137` continua descartado e pode terminar em zero. (`bin/rc137_no_restart.sh:25,53,69,70,72,81`) |
| **V3-7 — proveniência** | **Cura a ambiguidade exit/proveniência nas novas linhas e nos dois leitores identificados.** Somente `rodou is False` exclui a corrida; saída real 70/75 continua sendo corrida com erro. Ausência do campo equivale a `true`. (`bin/cron_run.sh:20,23,29`; `app/config/crons.py:1433`; `app/core/management/commands/pipeline_placar.py:72,80`; `bin/crons_duracao.py:85`) |

O **contraexemplo com cron real** é `HOME + '/scripts/backup_all.sh'`, declarado e envelopado pelo runner. O censo o considera host. Se a admissão recusar, o comando principal não roda, mas o runner segue para `docker exec saas_core ... cron_quebrado`, sem marca e sem nova passagem pela trava. Também há esse caminho após falha do backup, cuja marca pode estar como `casca=0`. A árvore viva só pode reconhecer o exec depois que ele existir; não fecha a janela anterior ao seu nascimento. Ocorrência dessa corrida em produção: **[nao verificado]**. (`app/config/crons.py:263,1800`; `bin/tests/test_deploy_nao_mata_elo.sh:324`; `bin/cron_run.sh:16,25,48,49`; `bin/admissao_cron.sh:170,208`)

No sentido conservador, `vigia_logins.sh --auto` pode terminar pelo piso/percentual sem executar nada dentro da casca, embora nasça `casca=1` pelo ramo que contém deploy. Isso prova classificação além do caminho efetivamente executado; **recusa permanente causada por esse ramo não está demonstrada**. (`app/config/crons.py:284`; `bin/vigia_logins.sh:48,52,71`; `bin/tests/test_deploy_nao_mata_elo.sh:324`)

**2. Quem escreve e quem lê**

| Campo/artefato | Quem escreve | Quem lê |
|---|---|---|
| **`rodou` em `placar.jsonl`** | Runner inicia `false`, muda para `true` antes do comando e publica no JSON. (`bin/cron_run.sh:20,23,29`) | Predicado da configuração; pipeline e medidor perguntam a ele. Vigia e contador leem a mesma linha para outras perguntas. (`app/config/crons.py:1433`; `app/core/management/commands/pipeline_placar.py:72`; `bin/crons_duracao.py:85`; `app/core/services/crons_quebrados.py:46`; `bin/rc137_no_restart.sh:73`) |
| **`linha_nao_rodou`** | Definição única na configuração. (`app/config/crons.py:1432`) | Importação no pipeline e carregamento no medidor. (`app/core/management/commands/pipeline_placar.py:9`; `bin/crons_duracao.py:54`) |
| **`restarts.log`** | Restarter publica início antes do restart e fim nos caminhos de sucesso/falha. (`bin/reinicio_sob_admissao.sh:64,65,67,70,72`) | Contador; selo inspeciona registros. O placar de host consome a saída do contador. (`bin/rc137_no_restart.sh:45`; `bin/tests/test_deploy_nao_mata_elo.sh:359`; `bin/placar_code.sh:459`) |
| **Marca `<nome>_<emp>.<pid>`** | `adm_marcar`, chamado pelo runner e pela descoberta da corrente; publicação por temporário e `mv`. (`bin/admissao_cron.sh:203,210,211`; `bin/cron_run.sh:16`; `bin/encadeado.sh:48`) | Validação de identidade, classificação, ancestralidade e varredura. Remoção pelo desmarcador ou pela varredura de órfãs. (`bin/admissao_cron.sh:151,165,183,192,218`) |
| **`.admissao`, fd8 e flock** | Cron abre e trava antes da classificação/publicação; deploy abre e trava antes da segunda varredura e do restart. (`bin/admissao_cron.sh:206,207,208,211`; `bin/reinicio_sob_admissao.sh:28,43,44,66`) | Os mesmos participantes disputam a trava. Cron fecha fd8 antes do comando; deploy mantém a trava até terminar o restarter. (`bin/admissao_cron.sh:214`; `bin/reinicio_sob_admissao.sh:69,73`) |

**Não localizei leitor produtivo que ainda use 70/75 para decidir “não rodou”.** O vigia continua tratando saída diferente de 0/2 como quebra, deliberadamente; isso inclui recusas. O contador continua procurando 137. Nenhum desses dois distingue ausência de `rodou` de `rodou=true`. (`app/core/services/crons_quebrados.py:16,46`; `bin/rc137_no_restart.sh:73`)

**Único escritor produtivo do placar localizado na cópia: `bin/cron_run.sh`.** As outras escritas encontradas são fixtures; escritor externo à cópia ou dinâmico permanece **[nao verificado]**. (`bin/cron_run.sh:29`; `bin/tests/test_medicao_de_cron_nao_expira.sh:31`; `app/core/tests/test_crons_quebrados.py:33`)

A migração não recupera proveniência histórica: uma recusa antiga sem `rodou` será contada como corrida real, conforme a compatibilidade escolhida. Quantidade dessas linhas no dado produtivo: **[nao verificado]**. (`app/config/crons.py:1428,1433`)

**3. Relógio e ancestralidade**

O teto de espera do **restarter** agora usa `/proc/uptime`; seus `date` restantes registram início/fim civis. Entretanto, **a corrente ainda decide seu teto por `date +%s`**, podendo prolongar a espera se o relógio retroceder. (`bin/reinicio_sob_admissao.sh:37,38,63,65`; `bin/encadeado.sh:91,96,97`)

Ainda há janelas civis no medidor de sete dias, no vigia de 26 horas e no pipeline; este obtém hora e minuto em duas leituras distintas. O contador cruza timestamps civis, portanto o relógio monotônico do restarter não torna a correlação imune a ajustes civis. (`bin/crons_duracao.py:66`; `app/core/services/crons_quebrados.py:27,39`; `app/core/management/commands/pipeline_placar.py:122,124`; `bin/rc137_no_restart.sh:60,76,79`)

Quanto ao `/proc`:

- **`comm` contendo `) ` não demonstra defeito:** `${s##*) }` remove até a última ocorrência, preservando PPID e starttime. Conferi também strings com múltiplos `) ` em memória. (`bin/admissao_cron.sh:75,79,177`)
- **Reuso simples de PID não passa na marca moderna:** starttime é validado antes da isenção ancestral. Marca legada sem starttime aceita mera existência do PID e não prova identidade original. (`bin/admissao_cron.sh:154,156,159,188,189`)
- **A cadeia de ancestrais guarda somente PIDs**, sem identidade temporal nem retrato atômico com a validação posterior. Há uma lacuna contra mudanças entre essas leituras; isenção concreta de uma marca moderna não ancestral, pelo fluxo atual, permanece **[nao verificado]**. (`bin/admissao_cron.sh:173,178,185,188,189`)

**4. Mordida dos casos 13, 15 e 16**

| Caso | O que efetivamente distingue | Mutante plausível que ainda escapa |
|---|---|---|
| **13** | Docker herdando fd8; restart lento/falho; falha em gravar o início impedindo restart. A fixture de diretório no lugar de `restarts.log` agora ataca o pré-requisito. (`bin/tests/test_deploy_nao_mata_elo.sh:52,355,358,364,370,372`) | Retirar `8>&-` das varreduras/sono e preservá-lo nos Docker: o observador de fd8 está somente no fake Docker. Sobrevivência integral executada: **[nao verificado]**. (`bin/reinicio_sob_admissao.sh:40,44,54`; `bin/tests/test_deploy_nao_mata_elo.sh:52`) |
| **15** | Diferencia corrida real 75 de recusa 75, exige os três valores de proveniência e duas amostras. (`bin/tests/test_deploy_nao_mata_elo.sh:396,397,400,407,419`) | Acrescentar `or d.get('exit') == 70` ao filtro do medidor apagaria corridas reais 70, sem atingir as amostras 0/75. O teste Django novo também só usa corrida real 75. Execução do mutante: **[nao verificado]**. (`bin/crons_duracao.py:85`; `app/chamados/tests/test_contract_crons.py:234`) |
| **16** | Ataca os formatos novos, ciclo, ancestral sequencial, relógio parado, TERM ignorado, passo maior que teto, limite alto do ambiente, leitor do contrato e erros do contador. (`bin/tests/test_deploy_nao_mata_elo.sh:423,434,444,455,460,468,472,478,488,491`) | Fixar `SEM_FIM_S=670` preserva a fixture atual: ela mata o velho 660, mas não demonstra derivação da fonte. Também falta ambiente `00` e ancestral com exec irmão vivo. Execução dos mutantes: **[nao verificado]**. (`bin/rc137_no_restart.sh:25`; `bin/tests/test_deploy_nao_mata_elo.sh:444,472,484,486`) |

O teste de restart surdo **morde a ausência de `-k`**: o fake ignora TERM antes de virar `sleep`, e o caso exige término dentro da escalada. (`bin/tests/test_deploy_nao_mata_elo.sh:55,56,460,461`)

O antigo mutante `_rr=0` agora é atacado: o caso 8 extrai a chamada real, exige a captura `$?` e cobra saída 75 com arquivo/argv. (`bin/tests/test_deploy_nao_mata_elo.sh:223,224,233`)

**5. Classes recorrentes**

| Classe | Conferência |
|---|---|
| **Segundo juiz registrado** | Não identifiquei duplicação de juiz de negócio no delta. A pergunta nova de proveniência tem um predicado compartilhado pelos dois consumidores; o registro vivo continua sendo `core/juizes.py`. Ausência global de duplicações: **[nao verificado]**. (`app/config/crons.py:1432`; `app/core/management/commands/pipeline_placar.py:9`; `bin/crons_duracao.py:54`; `app/docs/JUIZES.md:11`) |
| **Segundo escritor fora da porta** | Não localizei segundo escritor produtivo do placar. Há, contudo, **execução fora da porta de admissão** no alarme/recuperação do runner. (`bin/cron_run.sh:29,49,51`) |
| **Relógio fora da borda** | Corrigido o relógio civil da espera do restarter; permanece na espera da corrente e nas janelas dos leitores. (`bin/reinicio_sob_admissao.sh:38`; `bin/encadeado.sh:96,97`; `bin/crons_duracao.py:66`) |
| **Derivação paralela** | A janela sem fim agora deriva das constantes compartilhadas. **670 é coerente:** a primeira subida que falha encerra o procedimento, portanto não se acumulam três escaladas de KILL. A comparação das cascas literais do restart com `ADM_CASCAS` ainda depende do selo. (`bin/admissao_cron.sh:63`; `bin/reinicio_sob_admissao.sh:66,69,70`; `bin/tests/test_deploy_nao_mata_elo.sh:283,284`) |
| **Selo sem mordida** | Persistem as lacunas de 70 real, prazo `00`, derivação por valor fixo e sincronização do caso 14. (`bin/tests/test_deploy_nao_mata_elo.sh:383,397,472,484`) |
| **Ordem de trava** | Entre participantes migrados, publicação sob flock e segunda varredura sob a mesma trava fecham a janela publicação→restart. O alarme do runner não participa desse protocolo. (`bin/admissao_cron.sh:207,211`; `bin/reinicio_sob_admissao.sh:43,44,66`; `bin/cron_run.sh:49`) |
| **Pouso antes do pré-requisito** | Registro precede restart; migration precede chamada ao restarter. Suíte inteira, leitura final e pouso efetivo: **[nao verificado]**. A conduta exige suíte antes do opus e proíbe aprovação de pouso só pelo Codex. (`bin/reinicio_sob_admissao.sh:64,66`; `bin/deploy.sh:124,282`; `CLAUDE.md:370,373`) |

O comentário “fd8 não vai a filho nenhum” ainda excede o código: `date` e o `tr` das pipelines não recebem fechamento explícito. O selo demonstra fechamento no Docker. (`bin/reinicio_sob_admissao.sh:44,60,63,65,66`; `bin/tests/test_deploy_nao_mata_elo.sh:52`)

**6. Descartes da triagem**

- **D4b — envelope morto com filho vivo:** não é refutado. PID ausente torna a marca órfã e ela é removida, sem busca por descendentes reparentados. “Nenhum caminho de produção mata só o runner” permanece **[nao verificado]**; a fila não transforma isso em garantia de proteção. (`bin/admissao_cron.sh:154,192`; `bin/cron_run.sh:22`)
- **`manual` compartilhado:** a pendência permanece. Chamadas sem id compartilham destino e `.novo`; sucesso remove o arquivo desse id. O agendado tem ids próprios, portanto sua correção específica não depende de resolver o manual. (`bin/deploy.sh:52,54,285,286,361`; `bin/deploy_agendado.sh:94,104`)
- **Caso 14 apoiado em sleep:** a crítica permanece. Se o deploy atrasar antes da primeira varredura, ela já verá a marca e um mutante sem segunda varredura poderá passar. Não há confirmação da fase presumida pelo comentário. (`bin/tests/test_deploy_nao_mata_elo.sh:381,383,386,390`)

Esses descartes podem ser decisões de fila pelo julgador final; **não são demonstrações de improcedência técnica**, e a própria conduta manda encaminhar descartes ao opus. (`CLAUDE.md:368,369`)

**7. Noturno, recusa 75 e progresso do deploy** [nao verificado]

A classificação `noturno.py=1` tem fundamento: o RAW chama o noturno, este chama filhos literais, e eles executam em `mensageria`. O teto é 1500; o valor 2518 consta no dado fornecido e na configuração versionada, mas duração atual em produção permanece **[nao verificado]**. (`app/config/crons.py:575,576`; `eval/noturno.py:32,36,43,93`; `eval/gerador.py:65`; `eval/investigador.py:24`; `bin/admissao_cron.sh:35`; `app/config/crons_duracao.json:51`)

**2518 > 1500, sozinho, não implica bloqueio eterno.** Num cenário com uma corrida de 2518s começando junto da tentativa, sem outros bloqueadores, a primeira recusa após 1500s e uma segunda pode esperar os aproximadamente 1018s restantes. As hipóteses desse cenário em produção são **[nao verificado]**. (`bin/reinicio_sob_admissao.sh:48,50`; `bin/deploy_agendado.sh:99,105`)

O agendado tem **três novas tentativas por padrão**, termina 75 se a recusa persistir e remove seu agendamento; não há repetição infinita por padrão nesse laço. Entretanto, o `flock` da régua não tem prazo: existe espera sem teto antes da tentativa. Além disso, novos crons continuam admitidos enquanto o deploy espera, permitindo recusas repetidas sem garantia de oportunidade livre. Ocorrência de bloqueio permanente: **[nao verificado]**. (`bin/deploy_agendado.sh:99,105,113,121,123`; `bin/reinicio_sob_admissao.sh:29,46`; `bin/admissao_cron.sh:207,214`)

**No caminho normal, marca moderna válida e não ancestral do noturno impede o restart; atingir 1500 não o mata.** Ainda não há garantia geral para envelope morto com filho vivo ou para a exceção ancestral ampla. Os timeouts internos do noturno também podem encerrar seus subprocessos independentemente do deploy. Morte produtiva do noturno após este delta: **[nao verificado]**. (`bin/admissao_cron.sh:154,168,189`; `bin/reinicio_sob_admissao.sh:48,50`; `eval/noturno.py:33,37,44,94`)

## 03/11 00:01 -- HISTORICO (mais de 24 h) · sessao 740c1f5f01c0 · - · - · origem=Ronald (tmux)

**V3-1 cura os formatos nomeados, mas não fecha o universo executado pelo cron.** O fecho segue arquivos transitivamente, aceita caminhos relativos e usa `vistos`; o parser junta `\`, pula opções do compose e considera `$var` como casca. Continua sendo reconhecimento lexical de arquivos nomeados, limitado a arquivos menores que 2 MB, sem interpretar imports, expansões ou controle de fluxo. (`bin/admissao_cron.sh:89`, `bin/admissao_cron.sh:97`, `bin/admissao_cron.sh:99`, `bin/admissao_cron.sh:119`, `bin/admissao_cron.sh:121`, `bin/admissao_cron.sh:128`, `bin/admissao_cron.sh:131`)

**Contraexemplo com cron real:** `HOME + '/scripts/backup_all.sh'`, declarado em `CRONS`, nasce classificado como host; porém seu envelope pode executar `docker exec saas_core ... cron_quebrado` após falha do backup ou após recusa da própria admissão. O classificador recebe somente `CMD`, deixando o código do envelope fora do fecho. Na recusa nem sequer há marca publicada; após falha do comando a marca pode existir com `casca=0`. A árvore viva só protege enquanto o Docker já estiver visível, deixando a janela anterior ao exec. (`app/config/crons.py:263`, `app/config/crons.py:1800`, `bin/admissao_cron.sh:208`, `bin/cron_run.sh:16`, `bin/cron_run.sh:25`, `bin/cron_run.sh:45`, `bin/cron_run.sh:49`, `bin/admissao_cron.sh:170`)

Verifiquei apenas os predicados em leitura: o fecho de `scripts/backup_all.sh` retornou 1; o texto de `bin/cron_run.sh` retornou classificação casca. A reprodução integral dessa corrida e sua ocorrência produtiva permanecem **[nao verificado]**. (`bin/admissao_cron.sh:126`, `bin/cron_run.sh:49`)

**Outro cron real sujeito ao mesmo caminho:** `tickets_rodape.sh --conferir` está pinado host; havendo flag anterior, seu sucesso ou rc2 pode executar a recuperação em `saas_core` pelo envelope. (`app/config/crons.py:296`, `bin/tests/test_deploy_nao_mata_elo.sh:324`, `bin/cron_run.sh:50`, `bin/cron_run.sh:51`)

**V3-2 resolve o autoimpedimento síncrono de `vigia_logins.sh --auto`, mas a exceção é mais ampla que esse caso:** toda marca cujo PID esteja na lista de ancestrais recebe `segura=0`, sem conferir se existem outros descendentes em Docker. O cron real chama deploy sincronamente; não encontrei nesse caminho um exec irmão concorrente que prove dano atual. (`app/config/crons.py:284`, `bin/vigia_logins.sh:71`, `bin/admissao_cron.sh:185`, `bin/admissao_cron.sh:189`)

**Não procede acusar o parser de quebrar por `comm` contendo `) `.** Tanto `adm_starttime` quanto `adm_ancestrais` removem até a **última** ocorrência de `) `, preservando os campos posteriores; a primeira função lê o campo 22 e a segunda o PPID. (`bin/admissao_cron.sh:75`, `bin/admissao_cron.sh:79`, `bin/admissao_cron.sh:177`)

**Reuso simples de PID não burla uma marca moderna:** a validação do starttime acontece antes da isenção ancestral. A ressalva é a marca legada sem starttime: basta existir `/proc/<pid>` para ela ser considerada viva, sem provar identidade do processo originalmente marcado. Uma corrida entre o retrato dos ancestrais e a conferência posterior não é protegida por identidade na lista, que contém somente PIDs; exploração concreta pelo fluxo atual permanece **[nao verificado]**. (`bin/admissao_cron.sh:154`, `bin/admissao_cron.sh:156`, `bin/admissao_cron.sh:159`, `bin/admissao_cron.sh:173`, `bin/admissao_cron.sh:188`, `bin/admissao_cron.sh:189`)

**Noturno:** o fecho agora encontra seus filhos literais `gerador.py`, `harness.py`, `juiz.py` e `investigador.py`; esses executam `docker exec mensageria`. A classificação `noturno.py=1` tem fundamento na cópia. (`eval/noturno.py:32`, `eval/noturno.py:36`, `eval/noturno.py:43`, `eval/noturno.py:93`, `eval/gerador.py:65`, `eval/harness.py:45`, `eval/juiz.py:241`, `eval/investigador.py:24`)

Não achei outro falso negativo no comando principal dos RAW atualmente pinados; ausência global de outros caminhos permanece **[nao verificado]**. O contraexemplo do envelope já basta para reprovar a alegação de classe fechada. (`bin/tests/test_deploy_nao_mata_elo.sh:324`, `bin/admissao_cron.sh:208`, `bin/cron_run.sh:49`)

_1516 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
