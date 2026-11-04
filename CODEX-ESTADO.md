# CODEX-ESTADO

> Ultima resposta processada: **04/11/2026 11:08** · publicado em **04/11/2026 11:13** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

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

## 03/11 00:01 -- HISTORICO (mais de 24 h) · sessao f9861db3b76e · - · - · origem=Ronald (tmux)

O selo melhorou e morde regressões específicas; ainda não prova as classes inteiras. A análise foi somente estática, pois ele cria e remove arquivos. Execução integral e sobrevivência efetivamente executada dos mutantes: **[nao verificado]**. (`bin/tests/test_deploy_nao_mata_elo.sh:44`)

| Caso | O que morde | Mutante plausível sobrevivente |
|---|---|---|
| **13** | fd8 herdado pelo Docker; restart lento; falha com registro; impossibilidade de registrar início impede qualquer restart. (`bin/tests/test_deploy_nao_mata_elo.sh:354`, `:358`, `:364`, `:370`) | Remover o fechamento de fd8 das varreduras/sono, mantendo-o no Docker: o caso13 só observa `/proc/$$/fd/8` no fake Docker. Sobrevivência integral **[nao verificado]**. (`bin/tests/test_deploy_nao_mata_elo.sh:52`, `:355`; `bin/reinicio_sob_admissao.sh:40`, `:44`, `:54`) |
| **15** | Distingue corrida real exit75 de recusa exit75; exige proveniência e exatamente duas amostras de duração. (`bin/tests/test_deploy_nao_mata_elo.sh:396`, `:397`, `:400`, `:407`, `:419`) | `linha_nao_rodou = rodou is False OR exit == 70`: continuaria apagando corridas reais exit70. O caso15 só produz exit0/75; o novo Django também só produz corrida real75. Sobrevivência prevista; execução **[nao verificado]**. (`bin/tests/test_deploy_nao_mata_elo.sh:396`, `:397`, `:400`; `app/chamados/tests/test_contract_crons.py:233`, `:235`, `:236`) |
| **16** | Casos positivos/negativos do parser; fecho relativo com ciclo; ancestral; relógio parado; TERM ignorado; passo acima do teto; limite do env; contrato com casca faltante; truncamento137 e fim invertido. (`bin/tests/test_deploy_nao_mata_elo.sh:423`, `:434`, `:444`, `:455`, `:460`, `:468`, `:472`, `:478`, `:488`, `:491`) | Fixar `SEM_FIM_S=670`: a fixture compara comportamento no número atual, sem alterar as constantes produtoras. Portanto ela morde o velho660, mas não a derivação paralela. Sobrevivência integral **[nao verificado]**. (`bin/tests/test_deploy_nao_mata_elo.sh:483`, `:484`, `:486`; `bin/admissao_cron.sh:59`, `:63`) |

O caso16 do ancestral usa um único fluxo sequencial, com `docker exec ... true` concluído antes do restart. Não testa ancestral com outro filho ainda usando a casca nem identidade temporal da cadeia de pais. (`bin/tests/test_deploy_nao_mata_elo.sh:444`; `bin/admissao_cron.sh:173`, `:189`)

O caso8 agora morde o antigo mutante `_rr=0`: extrai a própria chamada e captura do retorno e exige saída75, arquivo e argv; a extração também exige literalmente `_rr=$?`. (`bin/tests/test_deploy_nao_mata_elo.sh:223`, `:224`, `:233`)

**Descartes:**

- **Caso14:** permanece dependente de sleeps. Assume que o deploy já fez a primeira varredura antes da publicação; se o processo atrasar, a primeira varredura já verá a marca e o mutante sem segunda varredura poderá passar. A fila é decisão de escopo, não demonstração de cura. (`bin/tests/test_deploy_nao_mata_elo.sh:378`, `:380`, `:381`, `:383`, `:386`, `:390`)
- **Manual compartilhado:** continua literal `manual`, compartilhando arquivo e `.novo`; sucesso pode apagar a recusa daquele id. O agendado usa ids próprios, portanto isso não desfaz seu isolamento, mas não cura a classe concorrente manual. (`bin/deploy.sh:52`, `:54`, `:285`, `:286`, `:361`; `bin/deploy_agendado.sh:94`, `:104`)
- **D4b:** `/proc/<pid>` ausente declara marca órfã; a varredura remove-a sem procurar descendentes reparentados. A afirmação “nenhum caminho de produção mata só o envelope” permanece **[nao verificado]**; descartá-la da garantia geral de proteção não é provado pelo código. (`bin/admissao_cron.sh:154`, `:192`; `bin/cron_run.sh:22`)

**Noturno e retries:**

O RAW real chama `cd .../eval && /usr/bin/python3 noturno.py`; o comentário registra2518s, coincidindo com o dado fornecido. O teto é1500s. (`app/config/crons.py:559`, `:575`, `:576`; `bin/admissao_cron.sh:35`)

Isso sozinho não implica bloqueio eterno: sob a hipótese de uma corrida2518s iniciada junto à primeira tentativa, sem outro cron bloqueador nem atraso externo, a primeira tentativa recusa em1500s e a segunda dispõe de teto suficiente para o restante1018s. Hipóteses produtivas e duração atual: **[nao verificado]**. (`bin/reinicio_sob_admissao.sh:48`, `:50`; `bin/deploy_agendado.sh:99`, `:105`)

As novas tentativas têm limite padrão3; recusa persistente termina75 e o agendamento é removido. Entretanto, o `flock` de `/tmp/regua.lock` não tem `-w`: há caminho de espera sem prazo antes de uma tentativa de deploy. Bloqueio produtivo dessa trava: **[nao verificado]**. (`bin/deploy_agendado.sh:78`, `:99`, `:105`, `:113`, `:121`, `:123`)

Não há timeout externo sobre o RAW completo no envelope, e a marca permanece até a saída do runner; timeout1500 do deploy não mata o noturno no caminho de recusa. Há timeouts internos dos subprocessos do noturno, independentes do deploy. (`bin/cron_run.sh:22`, `:24`; `bin/reinicio_sob_admissao.sh:48`, `:50`; `eval/noturno.py:32`, `:36`, `:43`, `:93`)

## 03/11 00:01 -- HISTORICO (mais de 24 h) · sessao de5873872725 · - · - · origem=Ronald (tmux)

- **V3-7 fecha a ambiguidade para as novas linhas:** o runner grava `rodou=false` antes da admissão e só muda para `true` antes de executar; escreve o campo no placar. O predicado exclui exclusivamente `False`; ausência do campo e `true` são equivalentes. Ambos os leitores de corrida/duração consultam esse predicado. (`bin/cron_run.sh:20,23,24,29`; `app/config/crons.py:1432,1433`; `app/core/management/commands/pipeline_placar.py:9,72`; `bin/crons_duracao.py:54,85`)

- **Nenhum leitor produtivo localizado usa 70/75 para inferir “não rodou”.** O vigia continua julgando quebra por saída diferente de 0/2, enquanto o contador busca 137; são perguntas distintas. (`app/core/services/crons_quebrados.py:46`; `bin/rc137_no_restart.sh:73`)

- **Único escritor produtivo localizado do placar:** `cron_run.sh`; as outras escritas encontradas pertencem a fixtures. Escritor externo à cópia: **[nao verificado]**. (`bin/cron_run.sh:29`; `bin/tests/test_medicao_de_cron_nao_expira.sh:31`; `app/core/tests/test_crons_quebrados.py:33`; `bin/tests/test_deploy_nao_mata_elo.sh:240`)

- **V3-6 cura os casos nomeados, não toda a classe “ilegível não é zero”.** Placar truncado antes de conter `137` continua sendo ignorado e pode resultar em zero. A verificação depende da substring literal. (`bin/rc137_no_restart.sh:69,70,71,72,81`)

- **670s não está subestimado por faltar KILL em cada casca:** cada primeira subida que falha encerra o script. O limite é restart antes de120 + duas subidas antes de180 + última subida180 com escalada10 =670. A derivação efetiva usa as constantes compartilhadas. (`bin/admissao_cron.sh:39,40,41,59,63`; `bin/reinicio_sob_admissao.sh:66,67,69,70`)

- **Persistem relógios civis decidindo outras janelas/tetos:** corrente espera carimbos com `date +%s`; medidor recorta sete dias; vigia recorta26h; pipeline lê hora/minuto separadamente. A espera do restarter usa `/proc/uptime`; seus `date` restantes registram testemunhas civis. (`bin/encadeado.sh:91,96,97`; `bin/crons_duracao.py:66`; `app/core/services/crons_quebrados.py:27,39`; `app/core/management/commands/pipeline_placar.py:122,124`; `bin/admissao_cron.sh:66,68`; `bin/reinicio_sob_admissao.sh:37,38,63,65`)

- **Mutantes sobreviventes previstos, sem execução:** acrescentar ao filtro de duração `or d.get('exit') == 70` apagaria corrida real70; o caso15 só oferece0/75. Copiar literalmente o predicado em `_nao_rodou` preservaria os efeitos e violaria fonte única; não há mutação da autoridade nessa fixture. Fixar `SEM_FIM_S=670` também conserva o caso16: ele cobra o valor atual, não a derivação. (`bin/crons_duracao.py:54,85`; `bin/tests/test_deploy_nao_mata_elo.sh:396,397,400,414,419,484,485,486`; `bin/rc137_no_restart.sh:25`)

- **Caso15 testa ausência de `rodou` apenas indiretamente fora do selo shell:** o novo teste Django fornece uma linha com `true` e outra sem campo, ambas com75, exigindo duas corridas e dois erros. Não verifica corrida real70 no medidor. (`app/chamados/tests/test_contract_crons.py:226,233,235,236,241,242`; `bin/tests/test_deploy_nao_mata_elo.sh:396,397,400`)

## 02/11 23:42 -- HISTORICO (mais de 24 h) · sessao cf0aa3782303 · DEPLOY-[nome]-ELO · verificador · origem=Code

**A volta 2 ainda não fecha P1..P7 como classes:** permanecem falhas de cobertura da fonte, teto e interpretação do placar; há também um leitor não migrado. (`bin/admissao_cron.sh:58,87`; `bin/reinicio_sob_admissao.sh:25,51`; `bin/rc137_no_restart.sh:63`; `bin/contrato_deploy.sh:23`)

Comparação Git independente dos hashes, execução integral da suíte e execução integral dos mutantes: **[nao verificado]**. A análise abaixo usa o patch fornecido e a cópia; o selo não foi executado porque cria, altera e remove arquivos. (`bin/tests/test_deploy_nao_mata_elo.sh:40,42,169`)

1. **P1..P7: classe, caso e contraexemplo**

| Item | Conclusão |
|---|---|
| **P1 — híbrido entre execs** | **Cura a sentinela e uma família de scripts literais, não toda a classe.** A sentinela contém `docker exec mensageria` e agora nasce `casca=1`. Entretanto, o reconhecimento é lexical, por linha, e segue caminhos absolutos somente um nível. `docker exec \` seguido de quebra de linha, `docker compose -f compose.yml exec…`, chamadas relativas indiretas e container por variável continuam podendo escapar entre execs. (`mensageria/bin/sentinela_diario.sh:14,16`; `bin/admissao_cron.sh:58,64,66,87,150`) |
| **P2 — teto absoluto** | **Parcial.** Passou a contar tempo decorrido e rejeita `PASSO=0`, mas aceita `ADM_PASSO_S=3600` com `ESPERA_CRON_S=1` e dorme o passo inteiro. Varreduras não têm prazo; a segunda varredura vazia permite prosseguir antes da conferência do tempo. Usa relógio civil, sujeito a retrocesso. (`bin/reinicio_sob_admissao.sh:19,21,35,36,38,42,43,46,51`) |
| **P3 — restart preso/fd herdado** | **Cura o fd no Docker e o processo que responde ao timeout; não toda espera ilimitada.** Restart e provas fecham fd 8. Porém `ADM_RESTART_TIMEOUT_S` aceita `0` ou prazo arbitrariamente alto, e `timeout` não tem escalada para KILL. Outros filhos, como os da varredura e `date`, ainda recebem fd 8. (`bin/reinicio_sob_admissao.sh:25,42,60,62,63,66`) |
| **P4 — recusa/argv** | **Cura o casamento por mtime e o argv vazio no protocolo agendado.** Há nome por tentativa, conferência do id interno e preservação do argv. A primeira passagem aceita somente o arquivo identificado, porque o rc da esteira pode estar traduzido/mascarado; as chamadas diretas seguintes exigem rc75 e arquivo. Portanto, não implementa literalmente “rc75 + arquivo” na primeira passagem. Deploys sem id continuam compartilhando `manual`, inclusive o mesmo `.novo`. (`bin/deploy.sh:48,52,54,285`; `bin/deploy_agendado.sh:74,83,94,99,101,105,107,110,111`) |
| **P5 — desconhecido/registro** | **Cura fonte ausente e registro da falha do restart, mas não a classe “erro de leitura não vira zero”.** JSON inválido do placar ainda é ignorado; restart com fim anterior ao início é aceito. O fallback fixo de 660s também pode ser menor que o prazo efetivo do escritor. (`bin/rc137_no_restart.sh:20,44,51,53,56,60,63`; `bin/reinicio_sob_admissao.sh:25,61,64,67`) |
| **P6 — “não rodou” nos leitores** | **Cura a interpretação das linhas de admissão 70/75 nos dois leitores citados.** Contudo, um comando admitido também pode executar e retornar 70/75: o runner grava o mesmo formato, e os filtros passam a apagá-lo das corridas/durações. A proveniência não está representada. (`bin/cron_run.sh:16,18,20,25`; `app/core/management/commands/pipeline_placar.py:72`; `bin/crons_duracao.py:85`) |
| **P7 — selo sem mordida** | **Melhorou substancialmente, mas não fecha a classe.** Há fixtures para intervalo entre execs, fonte ausente, tempo decorrido e segunda varredura. Ainda faltam integração do deploy real, escrita inicial impossível e prazo resistente a TERM; detalhes no item 4. (`bin/tests/test_deploy_nao_mata_elo.sh:217,248,295,324,338,355`) |

O censo pinado não elimina indireções: declara `vigia_logins.sh=0`, embora o RAW execute `--auto` e esse ramo chame `bin/deploy.sh --sem-migrate` por caminho relativo, não seguido pelo reconhecedor. Ocorrência desse ramo em produção: **[nao verificado]**. (`bin/tests/test_deploy_nao_mata_elo.sh:312`; `app/config/crons.py:284`; `bin/vigia_logins.sh:71`; `bin/admissao_cron.sh:87`)

No contador real, executando apenas seu Python em memória, três entradas produziram `0`: placar truncado contendo `"exit":137`; restart `200 100` com fim de cron em epoch150; e início `100 -` com fim de cron em epoch900. São consequências, respectivamente, do descarte de JSON inválido, ausência de validação da ordem temporal e janela fixa. (`bin/rc137_no_restart.sh:20,44,53,63,70`)

2. **Quem escreve e quem lê**

| Arquivo/valor | Escritor ou produtor | Leitor/consumidor |
|---|---|---|
| `logs/crons_em_curso/<nome>_<emp>.<pid>` | `adm_marcar`, chamado pelo runner e pela descoberta da corrente; publica por temporário + `mv`. Limpeza por `adm_desmarcar` e varredura de órfãs. (`bin/admissao_cron.sh:143,153,160,134`; `bin/cron_run.sh:16`; `bin/encadeado.sh:48`) | `adm_marca_viva` lê PID/starttime/exe; `adm_segura` lê `casca` ou consulta árvore; restart consome `adm_seguram`. (`bin/admissao_cron.sh:105,119,139`; `bin/reinicio_sob_admissao.sh:38,42`) |
| `.admissao` / fd8 / flock | Cron abre e trava durante classificação/publicação; restart abre e trava durante conferência/restart/subida. (`bin/admissao_cron.sh:148,149,156`; `bin/reinicio_sob_admissao.sh:28,41,44,63,66`) | Os mesmos dois participantes disputam o mesmo arquivo. O comando do cron começa depois do fechamento do fd. (`bin/admissao_cron.sh:35,156`; `bin/cron_run.sh:20`) |
| `restarts.log` | Restart escreve `ini -` antes do ato e `ini fim` nos caminhos de sucesso/falha. (`bin/reinicio_sob_admissao.sh:59,61,62,64,67,69`) | Contador e selo; o placar consome a saída do contador. (`bin/rc137_no_restart.sh:40`; `bin/tests/test_deploy_nao_mata_elo.sh:343`; `bin/placar_code.sh:459`) |
| `recusa_cron.<id>.pendente` | `deploy.sh` escreve id e argv separados por NUL quando recebe rc75; publicação por `.novo` + `mv`. (`bin/deploy.sh:283,285,286`) | Agendado confere nome/id, lê argv, remove arquivo consumido e repete. Deploy manual bem-sucedido remove a recusa do seu id. (`bin/deploy_agendado.sh:83,99,101,102,104,113`; `bin/deploy.sh:361`) |
| rc70/75 da admissão | Literais de `adm_marcar`. (`bin/admissao_cron.sh:147,148,149,151,154`) | Runner impede execução, grava placar e alarma; descoberta traduz falha para alarme rc2. (`bin/cron_run.sh:16,18,22,25,44`; `bin/encadeado.sh:48,52,59,61`) |
| rc75 do restart/deploy | Restart recusa por espera/trava; deploy propaga e grava a recusa. (`bin/reinicio_sob_admissao.sh:41,48`; `bin/deploy.sh:283,288`) | Agendado trata arquivo e rc das novas tentativas. (`bin/deploy_agendado.sh:99,107,110,111,113`) |
| `EXITS_NAO_RODOU` | Declaração Python `(70,75)`. (`app/config/crons.py:1430`) | Pipeline importa; medidor carrega a configuração. O shell mantém literais próprios, comparados pelo selo. (`app/core/management/commands/pipeline_placar.py:9,72`; `bin/crons_duracao.py:54,85`; `bin/tests/test_deploy_nao_mata_elo.sh:372,376,377`) |
| `placar.jsonl` | Runner grava `ini/fim/dur/exit`, incluindo retorno real do comando. (`bin/cron_run.sh:20,25`) | Pipeline, medidor, vigia, contador e selo. (`app/core/management/commands/pipeline_placar.py:65`; `bin/crons_duracao.py:73`; `app/core/services/crons_quebrados.py:30`; `bin/rc137_no_restart.sh:55`; `bin/tests/test_deploy_nao_mata_elo.sh:264`) |
| `crons_duracao.json` | Medidor calcula p95 ×1,5, piso30, preserva valores antigos sem nova amostra e grava JSON. (`bin/crons_duracao.py:57,90,94,107,111`) | `config/crons.py` lê `dur_s` para `DUR_MAX_S` e horários derivados. (`app/config/crons.py:146,154,159,160,241,248`) |
| `rodou/erros/alarmes/dur_max` | Agregação de `medidos`; `dur_max` é máximo, distinto do envelope p95. (`app/core/management/commands/pipeline_placar.py:74,76,78,81,82`) | `placar` decide situação/dependência e o command apresenta. (`app/core/management/commands/pipeline_placar.py:88,94,96,102,109,125,129`) |

**Leitor não migrado confirmado:** `contrato_deploy.sh` ainda exige linha começando por `docker restart`; agora ela começa por `timeout`. Isso acusa as três cascas como ausentes e alimenta o placar de contrato. (`bin/contrato_deploy.sh:23,24,25`; `bin/reinicio_sob_admissao.sh:63`; `bin/placar_code.sh:130,131`)

Os dois leitores produtivos identificados de duração/“rodou” foram migrados. O vigia mantém 70/75 como quebra deliberadamente; o placar já trata saída desconhecida do contador como contribuição não zero. (`bin/crons_duracao.py:85`; `app/core/management/commands/pipeline_placar.py:72`; `app/core/services/crons_quebrados.py:46`; `bin/placar_code.sh:459,461`)

Duas ressalvas sobre P6:

- A espera da admissão termina **antes** de `T0`; portanto, os 900s de espera inflarem `dur` não são demonstrados pelo runner atual. As fixtures introduzem essas durações artificialmente. (`bin/cron_run.sh:16,17,24,25`; `bin/tests/test_deploy_nao_mata_elo.sh:380,381,382`)
- Se um envelope antigo estivesse contaminado e não surgisse amostra elegível, o valor antigo permaneceria. Contaminação efetiva em produção: **[nao verificado]**. (`bin/crons_duracao.py:94,107`)

3. **Relógio, laços, fd e ordem**

- **Relógio:** a política de espera lê `date` dentro de `_decorrido`, sem relógio monotônico ou instante recebido. Isso permite prolongamento por retrocesso. Pipeline e medidor também leem relógio, mas em seus pontos de entrada; no pipeline, hora e minuto vêm de duas leituras distintas. (`bin/reinicio_sob_admissao.sh:35,36`; `bin/crons_duracao.py:66,108`; `app/core/management/commands/pipeline_placar.py:122,124`)
- **Teto:** além do passo sem limitação e das varreduras sem prazo, o agendado espera `/tmp/regua.lock` sem `-w`. O limite de tentativas não fornece prazo absoluto para o fluxo inteiro. (`bin/reinicio_sob_admissao.sh:38,42,51`; `bin/deploy_agendado.sh:99,105`)
- **fd:** o comando do cron e os Docker do restart/subida não herdam fd8. A afirmação do comentário “filho nenhum” é mais ampla que a implementação: auxiliares da varredura e `date` ainda o herdam. (`bin/admissao_cron.sh:156`; `bin/reinicio_sob_admissao.sh:42,57,60,62,63,66`)
- **Janela varredura→flock:** está coberta entre os participantes migrados. Cron publica sob flock; deploy adquire o mesmo flock e varre novamente antes do restart. Não há publicação participante entre essa segunda varredura e o ato. Escritor produtivo externo a esse protocolo: **[nao verificado]**. (`bin/admissao_cron.sh:149,153,156`; `bin/reinicio_sob_admissao.sh:41,42,43,63`)

4. **Mordida do selo**

**Sim: por inspeção, o caso 8 distingue os dois mutantes perguntados; execução integral desses mutantes permanece [nao verificado].**

- Remover `[ "$rc" = 75 ] || break` faz a fixture `rc1` alcançar a mensagem falsa “rc75 sem a recusa”. O teste proíbe essa mensagem. **A mordida é no diagnóstico:** sem arquivo, o `while` já impediria terceira chamada. (`bin/deploy_agendado.sh:99,110,111`; `bin/tests/test_deploy_nao_mata_elo.sh:179,210`)
- Trocar a comparação de id dentro de `_recusou` por `true` aceita `id=outro` no nosso nome de arquivo e produz chamadas extras; o teste exige uma chamada. Trocar somente a chamada `_recusou` posterior ao rc75 por `true` conserva a proteção do `while` e não é distinguido pelos cenários atuais. (`bin/deploy_agendado.sh:87,99,111`; `bin/tests/test_deploy_nao_mata_elo.sh:180,213,214`)

| Caso novo/ampliado | Mordida e limite |
|---|---|
| **3/11** | Ataca classificação pela fonte e intervalo entre execs; testa lista Python e opções com valor. Não cobre os formatos/indireções de P1. (`bin/tests/test_deploy_nao_mata_elo.sh:103,295,300,304,306,308`) |
| **8** | Ataca argv, argv vazio, identidade, recusa persistente e rc incompatível. O trecho real executado começa **dentro** do ramo de recusa; não verifica sua seleção pelo rc recebido. (`bin/tests/test_deploy_nao_mata_elo.sh:191,200,203,207,210,214,217,220`) |
| **9** | Ataca fontes ausentes, restart ilegível e início sem fim; não testa placar truncado, fim invertido ou fallback divergente do escritor. (`bin/tests/test_deploy_nao_mata_elo.sh:242,245,248,251,253`; `bin/rc137_no_restart.sh:44,53,63`) |
| **10** | Adapta o reconhecimento da linha com `timeout`; mede somente comandos/encadeados com exit0/2, não todo RAW agora classificado pela fonte. Medida produtiva atual: **[nao verificado]**. (`bin/tests/test_deploy_nao_mata_elo.sh:258,262,267,275`) |
| **12** | Ataca soma de sleeps e passo zero. Não testa passo acima do teto, varredura lenta ou relógio retrocedendo. (`bin/tests/test_deploy_nao_mata_elo.sh:60,324,328,333`) |
| **13** | Ataca fd no Docker, restart lento e registro na falha. O fake vira `sleep`, que não exercita resistência a TERM; não há fixture de registro inicial não gravável. (`bin/tests/test_deploy_nao_mata_elo.sh:48,51,338,342,348`) |
| **14** | Ataca retirada da segunda varredura, mas presume por sleeps que o deploy chegou ao flock; falta confirmação dessa fase. (`bin/tests/test_deploy_nao_mata_elo.sh:355,357,360,363,367`) |
| **15 e testes Django** | Atacam inclusão de 70/75 nas métricas e preservação do alarme. Não distinguem comando realmente executado que retorna esses códigos, nem leitor que copie literalmente a tuple atual. (`bin/tests/test_deploy_nao_mata_elo.sh:375,380,392`; `app/chamados/tests/test_contract_crons.py:207,221,223,240`) |

**Mutante plausível sobrevivente:** manter a chamada em `deploy.sh`, mas trocar `_rr=$?` por `_rr=0`. O deploy ignoraria a recusa; o selo continuaria exercitando o restarter isolado e o corpo extraído da recusa. Sobrevivência integral executada: **[nao verificado]**. (`bin/deploy.sh:282,283,290`; `bin/tests/test_deploy_nao_mata_elo.sh:217,220`)

Outro sobrevivente previsto é permitir prosseguir quando falha a escrita inicial de `restarts.log`: os casos atuais verificam registros com diretório gravável, sem atacar esse pré-requisito. (`bin/reinicio_sob_admissao.sh:61`; `bin/tests/test_deploy_nao_mata_elo.sh:338,342,348`)

5. **Classes recorrentes da casa**

| Classe | Conferência |
|---|---|
| **Segundo juiz registrado** | Não identifiquei duplicação de juiz de negócio nos trechos novos: eles classificam admissão e filtram métricas. A constatação não certifica ausência global de duplicações. (`bin/admissao_cron.sh:55,83`; `app/core/management/commands/pipeline_placar.py:72`; `app/core/juizes.py:3,7`) |
| **Segundo escritor fora da porta** | Publicação produtiva de marca passa pela biblioteca; recusa e restart têm escritores identificados. A injeção direta da marca no caso14 pertence ao teste. (`bin/cron_run.sh:16`; `bin/encadeado.sh:48`; `bin/admissao_cron.sh:153`; `bin/deploy.sh:285`; `bin/reinicio_sob_admissao.sh:61`; `bin/tests/test_deploy_nao_mata_elo.sh:363`) |
| **Relógio fora da borda** | Há leitura direta dentro da política de espera; o teto continua dependente do relógio civil. (`bin/reinicio_sob_admissao.sh:35,36,46`) |
| **Derivação paralela da fonte única** | **Procede:** 660 é derivação fixa de prazos que o escritor pode alterar. Os códigos shell/Python também são declarações separadas, embora a igualdade seja cobrada pelo selo. (`bin/rc137_no_restart.sh:13,20`; `bin/reinicio_sob_admissao.sh:25,66`; `app/config/crons.py:1430`; `bin/tests/test_deploy_nao_mata_elo.sh:376,377`) |
| **Selo sem mordida** | Persistem lacunas de integração, proveniência e falha de registro descritas acima. (`bin/tests/test_deploy_nao_mata_elo.sh:217,338,380`) |
| **Ordem de trava** | A publicação e a segunda varredura cobrem a janela solicitada; o prazo de espera continua incompleto. (`bin/admissao_cron.sh:149,153`; `bin/reinicio_sob_admissao.sh:41,42,51`) |
| **Pouso antes do pré-requisito** | O código coloca registro antes do restart e migration antes da chamada ao restarter. Suite inteira verde, leitura final e pouso efetivo: **[nao verificado]**. (`bin/reinicio_sob_admissao.sh:61,63`; `bin/deploy.sh:124,282`; `CLAUDE.md:369,372,373`) |

6. **D1..D4**

- **D1 continua válido para o cenário descartado:** recusa superada pode terminar com sucesso; uma fatia posterior bem-sucedida não apaga falha anterior. Ressalva: `PIOR` guarda o **último não zero**, não máximo numérico ou severidade. (`bin/deploy_agendado.sh:106,114,123`)
- **D2 continua condicional à conduta declarada:** código não impede agendamento antes do pouso nem aplica janela aos retries. Cumprimento histórico do aval e ausência de novos agendamentos: **[nao verificado]**. (`bin/deploy_agendado.sh:50,55,99,105`)
- **D3 continua válido quanto à proteção do elo:** alarme falho não cria flag. Porém a repetição depende de novo disparo com rc ruim; não há retry incondicional do alarme perdido. (`bin/cron_run.sh:44,45`)
- **D4 precisa ser dividido.** Marca retida pelo vigia é conservadora, pois o trap limpa somente na saída. **Envelope morto com filho vivo não é conservador:** `/proc/<pid>` ausente torna a marca órfã, a varredura a remove e não procura o filho reparentado. Isso deixa um contraexemplo à proteção; ocorrência produtiva: **[nao verificado]**. (`bin/cron_run.sh:19,45,50`; `bin/admissao_cron.sh:108,124,131,134`)

## 02/11 23:38 -- HISTORICO (mais de 24 h) · sessao 74d8250b82c6 · - · - · origem=Ronald (tmux)

P5/P6 têm cura dos casos citados, mas restam contraexemplos de classe.

- **P5 fontes ausentes:** `FileNotFoundError` vira desconhecido, e o shell devolve `?`, rc2; `placar_code.sh` já consome desconhecido como contribuição não zero ao total. (`bin/rc137_no_restart.sh:51`, `bin/rc137_no_restart.sh:56`, `bin/rc137_no_restart.sh:75`, `bin/placar_code.sh:459`)
- **P5 fonte ilegível ainda pode virar zero:** JSON inválido no placar é descartado. Executei só o Python real em memória, substituindo `open` por `StringIO`: restarts `100 200` com placar `{"exit":137,quebrou` imprime `0`. A validação de restart também aceita fim anterior ao início. (`bin/rc137_no_restart.sh:44`, `bin/rc137_no_restart.sh:60`)
- **Fonte única duplicada:** `SEM_FIM_S=660` copia a conta `120+3×180`, mas o escritor aceita `ADM_RESTART_TIMEOUT_S` arbitrário; restart com prazo maior, sem registro final, pode ter rc137 fora da janela considerada. Em memória, restart `100 -` com rc137 em epoch900 imprime zero. (`bin/rc137_no_restart.sh:20`, `bin/rc137_no_restart.sh:53`, `bin/reinicio_sob_admissao.sh:25`, `bin/reinicio_sob_admissao.sh:66`)
- **Registro antes do ato:** o escritor grava `ini -` e aborta se isso falhar; tanto falha do restart quanto falha de subida chamam `_fim`. Essa parte cura o caso citado. (`bin/reinicio_sob_admissao.sh:61`, `bin/reinicio_sob_admissao.sh:64`, `bin/reinicio_sob_admissao.sh:67`)

**Escritores/leitores:**

| Objeto | Escritor | Leitores |
|---|---|---|
| `restarts.log` | início/fim em `bin/reinicio_sob_admissao.sh:59` | contador em `bin/rc137_no_restart.sh:40`; selo em `bin/tests/test_deploy_nao_mata_elo.sh:343` |
| `placar.jsonl` | runner grava RC e duração em `bin/cron_run.sh:25` | duração em `bin/crons_duracao.py:73`; pipeline em `app/core/management/commands/pipeline_placar.py:65`; vigia em `app/core/services/crons_quebrados.py:30`; contador em `bin/rc137_no_restart.sh:55`; selo em `bin/tests/test_deploy_nao_mata_elo.sh:264` |
| `EXITS_NAO_RODOU` | tuple declarada em `app/config/crons.py:1430` | import do pipeline em `app/core/management/commands/pipeline_placar.py:9`; carga do medidor em `bin/crons_duracao.py:54` |
| RC70/75 de admissão | literais shell em `bin/admissao_cron.sh:147` | runner em `bin/cron_run.sh:16`; descoberta em `bin/encadeado.sh:48`, traduzida para2 em `bin/encadeado.sh:59` |
| envelope `crons_duracao.json` | medidor em `bin/crons_duracao.py:111`, acionado por `bin/crons.sh:13` | `_dur_medida` e `DUR_MAX_S` em `app/config/crons.py:146`, `app/config/crons.py:159` |
| agregado `rodou/erros/dur_max` | `medidos` em `app/core/management/commands/pipeline_placar.py:74` | `placar` em `app/core/management/commands/pipeline_placar.py:88` e apresentação em `app/core/management/commands/pipeline_placar.py:125` |

Não encontrei leitor produtivo de duração/“rodou” que preserve a interpretação anterior: ambos foram migrados; o vigia deliberadamente mantém70/75 como quebra, e o contador seleciona apenas137. (`bin/crons_duracao.py:85`, `app/core/management/commands/pipeline_placar.py:72`, `app/core/services/crons_quebrados.py:46`, `bin/rc137_no_restart.sh:64`)

**P6 não fecha a proveniência:** comando admitido também pode executar e devolver70/75; o runner sobrescreve RC com o retorno real e grava o mesmo formato. Os filtros então dizem que essa corrida não ocorreu. A regra “70/75 = não executou” não é garantida pelo escritor. (`bin/cron_run.sh:18`, `bin/cron_run.sh:20`, `bin/cron_run.sh:25`, `app/core/management/commands/pipeline_placar.py:72`, `bin/crons_duracao.py:85`)

Os valores shell e Python são duas declarações mantidas iguais pelo selo via `grep`, não uma única fonte lida pelo shell. (`bin/admissao_cron.sh:147`, `app/config/crons.py:1430`, `bin/tests/test_deploy_nao_mata_elo.sh:372`)

A duração antiga permanece quando não há amostra elegível; portanto eventual envelope já contaminado não seria necessariamente corrigido só por instalar o filtro. Existência de contaminação em produção: **[nao verificado]**. (`bin/crons_duracao.py:94`, `bin/crons_duracao.py:107`)

A espera da admissão ocorre antes de `T0`: a afirmação de que os900s entram na duração não é comprovada pelo runner atual; são valores fabricados nas fixtures. (`bin/cron_run.sh:16`, `bin/cron_run.sh:17`, `bin/cron_run.sh:25`, `bin/tests/test_deploy_nao_mata_elo.sh:380`)

**D3:** válido quanto à proteção do elo: alarme está fora da admissão e flag só nasce com sucesso do `docker exec`. O retry é condicional ao próximo RC ruim; não há repetição incondicional de um alarme perdido. (`bin/cron_run.sh:44`, `bin/cron_run.sh:45`)

**D4:** não manter inteiro. Marca retida pelo vigia é conservadora porque o trap EXIT só limpa após o vigia terminar. Porém envelope morto com filho vivo não é conservador: `/proc/<pid>` ausente torna a marca órfã e ela é apagada; não existe busca pelo filho reparentado, pois a árvore só é examinada após validar a marca viva. Ocorrência produtiva: **[nao verificado]**. (`bin/cron_run.sh:19`, `bin/cron_run.sh:45`, `bin/cron_run.sh:50`, `bin/admissao_cron.sh:108`, `bin/admissao_cron.sh:124`, `bin/admissao_cron.sh:134`)

O teste antigo `test_medicao_de_cron_nao_expira.sh` não quebra pela nova constante: importa o medidor real e substitui apenas caminhos do placar/saída; `CRONS_PY` segue apontando à configuração real. A fixture sem `exit` passa como `None`. (`bin/tests/test_medicao_de_cron_nao_expira.sh:16`, `bin/tests/test_medicao_de_cron_nao_expira.sh:44`, `bin/tests/test_medicao_de_cron_nao_expira.sh:47`, `bin/crons_duracao.py:31`, `bin/crons_duracao.py:85`)

## 02/11 23:38 -- HISTORICO (mais de 24 h) · sessao 98ad748d0fa4 · - · - · origem=Ronald (tmux)

Achados por inspeção estática; execução da suíte e dos mutantes [nao verificado], pois o selo cria e remove arquivos em `bin/tests/test_deploy_nao_mata_elo.sh:40`.

- **Caso 8 mata a remoção de `[ "$rc" = 75 ] || break`.** Com o modo `rc1`, a segunda chamada retorna 1 sem arquivo (`bin/tests/test_deploy_nao_mata_elo.sh:179`). Sem a guarda de `bin/deploy_agendado.sh:110`, chega à mensagem falsa “rc 75 sem a recusa” de `bin/deploy_agendado.sh:111`; o teste a proíbe explicitamente em `bin/tests/test_deploy_nao_mata_elo.sh:210`. A mordida é no diagnóstico; o teste de arquivo já impediria uma terceira chamada (`bin/deploy_agendado.sh:99`).

- **Caso 8 mata substituir por `true` a comparação de ID dentro de `_recusou`.** A fixture põe `id=outro` no arquivo com nosso nome (`bin/tests/test_deploy_nao_mata_elo.sh:180`); sem a comparação em `bin/deploy_agendado.sh:87`, entra nas novas tentativas em `bin/deploy_agendado.sh:99`, contrariando a exigência de exatamente uma chamada em `bin/tests/test_deploy_nao_mata_elo.sh:214`. Se o mutante substituir somente a chamada `_recusou` de `bin/deploy_agendado.sh:111` por `true`, a proteção do `while` ainda existe; esse mutante sobreviveria aos cenários atuais por inspeção (`bin/deploy_agendado.sh:99`; `bin/tests/test_deploy_nao_mata_elo.sh:203`).

- **Mutante relevante que sobrevive:** manter a chamada de `bin/deploy.sh:282`, mas trocar `_rr=$?` por `_rr=0`. O deploy ignoraria a recusa e continuaria pela prova, enquanto o selo exercita o restarter isoladamente e extrai somente o corpo do ramo de recusa do deploy real (`bin/tests/test_deploy_nao_mata_elo.sh:217`, `bin/tests/test_deploy_nao_mata_elo.sh:220`). Não há execução integrada do deploy real nesse caso; sobrevivência executada [nao verificado].

- **Outro mutante que sobrevive:** trocar o tratamento de falha da escrita inicial por `|| true` em `bin/reinicio_sob_admissao.sh:61`. Os testes verificam registro em execução normal, timeout e falha do Docker, mas não simulam `restarts.log` não gravável (`bin/tests/test_deploy_nao_mata_elo.sh:338`, `bin/tests/test_deploy_nao_mata_elo.sh:342`, `bin/tests/test_deploy_nao_mata_elo.sh:348`). Sobrevivência executada [nao verificado].

- **Caso 11:** distingue fonte de árvore e ataca o intervalo entre dois execs; o intervalo usa `sleep 1.8`, sem barreira confirmando a fase exata (`bin/tests/test_deploy_nao_mata_elo.sh:295`, `bin/tests/test_deploy_nao_mata_elo.sh:299`). A fixture de lista Python e opções com valor verifica os exemplos citados (`bin/tests/test_deploy_nao_mata_elo.sh:304`, `bin/tests/test_deploy_nao_mata_elo.sh:306`, `bin/tests/test_deploy_nao_mata_elo.sh:308`).

- **Caso 12:** ataca a contagem de sleeps com sleep multiplicado por três e verifica PASSO exatamente `0`; não cobre varredura lenta, espera da trava consumindo orçamento, relógio retrocedendo ou PASSO enorme (`bin/tests/test_deploy_nao_mata_elo.sh:324`, `bin/tests/test_deploy_nao_mata_elo.sh:328`, `bin/tests/test_deploy_nao_mata_elo.sh:333`).

- **Caso 13:** verifica fd 8 no Docker de restart/prova e timeout de um processo que responde a TERM; o fake usa `exec sleep`, portanto não cobre filho que ignore TERM (`bin/tests/test_deploy_nao_mata_elo.sh:48`, `bin/tests/test_deploy_nao_mata_elo.sh:51`, `bin/tests/test_deploy_nao_mata_elo.sh:339`, `bin/tests/test_deploy_nao_mata_elo.sh:342`). O código usa `timeout` sem `--kill-after` e aceita timeout arbitrário do ambiente (`bin/reinicio_sob_admissao.sh:25`, `bin/reinicio_sob_admissao.sh:63`).

- **Caso 14:** ataca a retirada da segunda varredura: injeta marca viva enquanto o deploy deveria aguardar a trava e exige restart posterior ao fim; a sincronização é por sleeps, sem confirmação de que o deploy chegou ao flock (`bin/tests/test_deploy_nao_mata_elo.sh:355`, `bin/tests/test_deploy_nao_mata_elo.sh:360`, `bin/tests/test_deploy_nao_mata_elo.sh:363`, `bin/tests/test_deploy_nao_mata_elo.sh:367`).

- **Caso 15:** morde deixar 70/75 entrarem na duração, mas não uma derivação paralela que copie literalmente `(70,75)` no leitor: a fixture só usa os valores atuais (`bin/tests/test_deploy_nao_mata_elo.sh:375`, `bin/tests/test_deploy_nao_mata_elo.sh:380`, `bin/tests/test_deploy_nao_mata_elo.sh:392`). Sobrevivência executada [nao verificado].

- **Leitor não migrado confirmado:** `bin/contrato_deploy.sh:23` só aceita linha começando em `docker restart`, enquanto agora começa em `timeout` (`bin/reinicio_sob_admissao.sh:63`); acusa as três cascas ausentes em `bin/contrato_deploy.sh:24`. É consumido pelo placar em `bin/placar_code.sh:130`; bloqueio da régua por esse contrato [nao verificado]. O selo novo, por sua vez, entra no glob de selos da régua (`bin/regua.sh:138`).

- **D1 continua válido quanto ao cenário citado:** recusa superada pode resultar em sucesso da fatia, e sucesso de fatia posterior não limpa `PIOR`, pois a atribuição exige rc não zero (`bin/deploy_agendado.sh:114`, `bin/deploy_agendado.sh:123`). “PIOR” significa último rc não zero, não máximo numérico (`bin/deploy_agendado.sh:114`).

- **D2 continua sendo condicional de conduta:** o código permite agendar sem essa janela e repetir sem nova verificação horária (`bin/deploy_agendado.sh:50`, `bin/deploy_agendado.sh:55`, `bin/deploy_agendado.sh:99`). Cumprimento histórico de “não criar agendamento antes do pouso” [nao verificado].

## 02/11 23:37 -- HISTORICO (mais de 24 h) · sessao 912c78ee6f28 · - · - · origem=Ronald (tmux)

- **P1 cura a sentinela, mas não a classe completa.** A marca nasce `casca=1` pelo script literal e cobre os quatro passos da sentinela (`bin/admissao_cron.sh:150`; `mensageria/bin/sentinela_diario.sh:14`). O reconhecimento continua lexical, por linha, e segue somente caminhos absolutos, um nível (`bin/admissao_cron.sh:58`; `bin/admissao_cron.sh:87`). Contraexemplos: `docker exec \` + quebra de linha + `saas_core ...`, `docker compose -f compose.yml exec saas_core ...`, chamadas indiretas relativas e container por variável; fora do exec vivo, a árvore não segura (`bin/admissao_cron.sh:64`; `bin/admissao_cron.sh:66`; `bin/admissao_cron.sh:124`). Confirmei somente a função de reconhecimento, sem gravações: as duas primeiras formas retornam `casca=0`; ocorrência delas em produção [nao verificado].

- **O censo pinado não prova ausência de indireção.** Pina `vigia_logins.sh=0` (`bin/tests/test_deploy_nao_mata_elo.sh:312`), embora o RAW execute `vigia_logins.sh --auto` (`app/config/crons.py:284`) e seu ramo de recuperação chame `bin/deploy.sh --sem-migrate` por caminho relativo (`bin/vigia_logins.sh:71`). Esse deploy faz provas com `docker exec` depois de liberar a admissão (`bin/deploy.sh:290`; `bin/deploy.sh:298`; `bin/deploy.sh:314`). O ramo ter ocorrido em produção [nao verificado].

- **P2 não estabelece teto absoluto.** Substituiu soma de sleeps por tempo de calendário (`bin/reinicio_sob_admissao.sh:35`; `bin/reinicio_sob_admissao.sh:36`), mas permite `ADM_PASSO_S=3600` e dorme o passo completo mesmo com teto de 1 s (`bin/reinicio_sob_admissao.sh:19`; `bin/reinicio_sob_admissao.sh:21`; `bin/reinicio_sob_admissao.sh:51`). As varreduras também não têm timeout, e o sucesso da segunda varredura sai antes de conferir o tempo (`bin/reinicio_sob_admissao.sh:38`; `bin/reinicio_sob_admissao.sh:42`; `bin/reinicio_sob_admissao.sh:43`). Retrocesso do relógio pode prolongar a espera, pois usa `date +%s`, não relógio monotônico (`bin/reinicio_sob_admissao.sh:36`).

- **P3 fecha o fd para docker, mas não cura toda espera ilimitada.** Restart e provas fecham fd 8 (`bin/reinicio_sob_admissao.sh:63`; `bin/reinicio_sob_admissao.sh:66`). Entretanto `ADM_RESTART_TIMEOUT_S` aceita `0`, sem validação, e o `timeout` não tem escalada de TERM para KILL (`bin/reinicio_sob_admissao.sh:25`; `bin/reinicio_sob_admissao.sh:63`); comportamento concreto de processo ignorando TERM [nao verificado]. A promessa “filho nenhum” excede a implementação: a segunda varredura e os comandos `date` executam com fd 8 aberto (`bin/reinicio_sob_admissao.sh:42`; `bin/reinicio_sob_admissao.sh:60`; `bin/reinicio_sob_admissao.sh:62`).

- **A ordem de admissão cobre a janela citada em P7.** O cron adquire flock antes da publicação atômica da marca e fecha fd antes de executar o comando (`bin/admissao_cron.sh:148`; `bin/admissao_cron.sh:153`; `bin/admissao_cron.sh:156`; `bin/cron_run.sh:20`). O deploy adquire o mesmo flock e repete a varredura antes de prosseguir (`bin/reinicio_sob_admissao.sh:41`; `bin/reinicio_sob_admissao.sh:42`). Entre esses participantes, a marca não nasce depois da segunda varredura; escritores externos não participantes [nao verificado].

- **Mordida dos casos 11–14:** caso 11 cobre híbrido literal entre execs e algumas opções, mas não os formatos acima (`bin/tests/test_deploy_nao_mata_elo.sh:295`; `bin/tests/test_deploy_nao_mata_elo.sh:304`). Caso 12 usa `PASSO=1` e testa `PASSO=0`, não passo acima do teto (`bin/tests/test_deploy_nao_mata_elo.sh:60`; `bin/tests/test_deploy_nao_mata_elo.sh:328`; `bin/tests/test_deploy_nao_mata_elo.sh:333`). Caso 13 simula restart preso com `/usr/bin/sleep`, filho obediente, não processo resistente a TERM (`bin/tests/test_deploy_nao_mata_elo.sh:50`; `bin/tests/test_deploy_nao_mata_elo.sh:51`). Caso 14 deve matar a remoção integral da segunda varredura, mas sincroniza por sleeps, não confirmação de estado (`bin/tests/test_deploy_nao_mata_elo.sh:355`; `bin/tests/test_deploy_nao_mata_elo.sh:360`; `bin/tests/test_deploy_nao_mata_elo.sh:367`); execução do mutante [nao verificado].

- **Mutante plausível sobrevivente:** mudar apenas o default para `RESTART_TETO="${ADM_RESTART_TIMEOUT_S:-0}"`. O teste preso fornece explicitamente `2`, e os demais restarts falsos terminam imediatamente; nenhuma asserção cobra o default positivo (`bin/reinicio_sob_admissao.sh:25`; `bin/tests/test_deploy_nao_mata_elo.sh:51`; `bin/tests/test_deploy_nao_mata_elo.sh:342`). Sobrevivência executada [nao verificado].

- **D3/D4 permanecem conservadores no código inspecionado.** Falha do alarme não cria flag, permitindo novo disparo; o sucesso cria a flag (`bin/cron_run.sh:45`). A marca fica até o envelope sair, inclusive durante o vigia, por trap EXIT (`bin/cron_run.sh:19`; `bin/cron_run.sh:45`; `bin/cron_run.sh:50`), e um envelope vivo marcado `casca=1` segura (`bin/admissao_cron.sh:122`). Isso pode retardar deploy; ocorrência concreta em produção [nao verificado].

_1510 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
