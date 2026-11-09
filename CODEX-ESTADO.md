# CODEX-ESTADO

> Ultima resposta processada: **09/11/2026 00:45** · publicado em **09/11/2026 00:47** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 09/11 00:45 · sessao b8280ae407bb · FIO-E0-V4-E1-FILA · sonda · origem=Code

Transcrição literal do dado fornecido; execução da sonda sem verificação independente. [nao verificado]

```text
ERRO p= 48633 _Lavra 
ERRO p= 48634 _Lavra 
ERRO p= 48637 _Lavra 
ERRO p= 48638 _Lavra 
ERRO p= 48641 _Lavra 
REATARc universo colabs= 248 perguntas= 3359 erros= 52
REATARc HIST reapontaria a TERMINAL= 0 a vivo= 11
REATARc c1 lavraria= 0 julgar_celula escreveria= 0
REATARc c2 abrir chamado= 33 criaria novo= 33 veto na porta= 0
REATARc c2 abrir achou TERMINAL e tentou RENASCER= 0 status= []
REATARc RENASCER escreveria (guardas passaram)= 0 vetado pelas guardas= 0
REATARc pares (pergunta,chamado) que renasceriam (ate 20)= []
FIM sonda_v22c_acervo
```

1. **Sim, existe dependência no código que pode mudar o caminho do c2.** `julgar_celula` chama `julgar_colab`; o corpo lavra e persiste a célula antes de a fábrica chamar a emissão de perguntas, que pode alcançar `reatar_pergunta_orfa` e o c2 ainda dentro do julgamento. (app/ponto/services/cartorio.py:1989, app/ponto/services/cartorio.py:1802, app/ponto/services/cartorio.py:1822, app/ponto/services/cartorio.py:997; app/chamados/services/fabrica_da_celula.py:186, app/chamados/services/fabrica_da_celula.py:151; app/chamados/services/disputa_emissao.py:464; app/chamados/reconciliador.py:1617)

   Nesse caminho, o c2 consulta `protestos` e pode retornar antes de `abrir`; também consulta `ata` e `julgada_em` para o veto `parcial_so_lista`. Portanto, desfazer as escritas pode mudar essas decisões. (app/chamados/reconciliador.py:867, app/chamados/reconciliador.py:869, app/chamados/reconciliador.py:878, app/chamados/reconciliador.py:882) Que isso tenha mudado algum caso concreto da sonda: [nao verificado].

2. **Não há nova guarda de veto depois da escrita que efetiva a reabertura.** `_renascer_por_premissa_viva` verifica situação, cascata, tranca e régua antes de chamar `reabrir`; esta consulta `recusa_da_reabertura` antes de `_gravar_estado`. Depois da gravação, ressuscita perguntas e retorna `True`; o renascimento anexa a ocorrência e retorna `True`. (app/chamados/models.py:1005, app/chamados/models.py:1019, app/chamados/models.py:1390, app/chamados/models.py:1432, app/chamados/models.py:1441, app/chamados/models.py:1022)

   **Há uma ressalva para “primeira escrita”: recusas também podem escrever trilha ou redirecionar perguntas e retornar `False`.** Assim, interceptar qualquer escrita, sem distinguir sua finalidade, não prova renascimento; o veto já foi decidido antes desses efeitos. (app/chamados/models.py:1017, app/chamados/models.py:1396, app/chamados/models.py:1402, app/chamados/models.py:1414, app/chamados/models.py:1426) Como a sonda distingue essas escritas: [nao verificado].

3. **Não há escrita persistente do c1 antes da chamada a `get_user_sistema`.** Antes dela estão as guardas; `p.carimbar` e o eventual fechamento da disputa vêm depois. (app/chamados/reconciliador.py:362, app/chamados/reconciliador.py:371, app/chamados/reconciliador.py:398, app/chamados/reconciliador.py:400, app/chamados/reconciliador.py:405, app/chamados/reconciliador.py:412) A própria `get_user_sistema` usa `get_or_create`, podendo escrever durante a chamada. (app/core/utils.py:18)

## 09/11 00:40 · sessao d3b51dece676 · FIO-E0-V4-E1-FILA · sonda · origem=Code

Transcrição literal do trecho fornecido; execução e autenticidade [nao verificado]:

```text
sonda_leitura: sonda_v22b_acervo.py [a670b468a300454b] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

2026-11-09 00:37:11,564 WARNING chamados.services.disputa_emissao S160 _derivar_chamado_destino: 2 candidatos pro motivo 'orfao_14h' (disputa=4929, colab=42) — pergunta criada SEM destino.
2026-11-09 00:37:11,594 WARNING chamados.services.disputa_emissao S160 _derivar_chamado_destino: 2 candidatos pro motivo 'orfao_14h' (disputa=4929, colab=42) — pergunta criada SEM destino.
2026-11-09 00:37:18,056 WARNING chamados.services.disputa_emissao S160 _derivar_chamado_destino: 2 candidatos pro motivo 'saida_sem_entrada' (disputa=7952, colab=364) — pergunta criada SEM destino.
2026-11-09 00:37:18,063 WARNING chamados.services.disputa_emissao S160 _derivar_chamado_destino: 2 candidatos pro motivo 'orfao_14h' (disputa=7952, colab=364) — pergunta criada SEM destino.
2026-11-09 00:37:20,948 WARNING chamados.services.disputa_emissao S160 _derivar_chamado_destino: 2 candidatos pro motivo 'intervalo_saida' (disputa=5362, colab=503) — pergunta criada SEM destino.
2026-11-09 00:37:26,476 WARNING chamados.services.disputa_emissao S160 _derivar_chamado_destino: 2 candidatos pro motivo 'intervalo_saida' (disputa=7721, colab=584) — pergunta criada SEM destino.
2026-11-09 00:37:26,485 WARNING chamados.services.disputa_emissao S160 _derivar_chamado_destino: 2 candidatos pro motivo 'intervalo_volta' (disputa=7721, colab=584) — pergunta criada SEM destino.
2026-11-09 00:37:26,493 WARNING chamados.services.disputa_emissao S160 _derivar_chamado_destino: 2 candidatos pro motivo 'orfao_14h' (disputa=7721, colab=584) — pergunta criada SEM destino.
2026-11-09 00:37:39,118 WARNING chamados.services.disputa_emissao S160 _derivar_chamado_destino: 2 candidatos pro motivo 'saida_sem_entrada' (disputa=1651, colab=841) — pergunta criada SEM destino.
2026-11-09 00:37:39,129 WARNING chamados.services.disputa_emissao S160 _derivar_chamado_destino: 2 candidatos pro motivo 'intervalo_saida' (disputa=1651, colab=841) — pergunta criada SEM destino.
2026-11-09 00:37:39,140 WARNING chamados.services.disputa_emissao S160 _derivar_chamado_destino: 2 candidatos pro motivo 'intervalo_volta' (disputa=1651, colab=841) — pergunta criada SEM destino.
2026-11-09 00:37:39,154 WARNING chamados.services.disputa_emissao S160 _derivar_chamado_destino: 2 candidatos pro motivo 'orfao_14h' (disputa=1651, colab=841) — pergunta criada SEM destino.
2026-11-09 00:37:39,167 WARNING chamados.services.disputa_emissao S160 _derivar_chamado_destino: 2 candidatos pro motivo 'saida_sem_entrada' (disputa=4984, colab=844) — pergunta criada SEM destino.
2026-11-09 00:37:39,178 WARNING chamados.services.disputa_emissao S160 _derivar_chamado_destino: 2 candidatos pro motivo 'intervalo_saida' (disputa=4984, colab=844) — pergunta criada SEM destino.
2026-11-09 00:37:39,187 WARNING chamados.services.disputa_emissao S160 _derivar_chamado_destino: 2 candidatos pro motivo 'intervalo_volta' (disputa=4984, colab=844) — pergunta criada SEM destino.
2026-11-09 00:37:39,197 WARNING chamados.services.disputa_emissao S160 _derivar_chamado_destino: 2 candidatos pro motivo 'orfao_14h' (disputa=4984, colab=844) — pergunta criada SEM destino.
2026-11-09 00:37:39,410 WARNING chamados.services.disputa_emissao S160 _derivar_chamado_destino: 2 candidatos pro motivo 'orfao_14h' (disputa=1397, colab=865) — pergunta criada SEM destino.
2026-11-09 00:37:45,002 WARNING chamados.services.disputa_emissao S160 _derivar_chamado_destino: 2 candidatos pro motivo 'saida_sem_entrada' (disputa=4542, colab=917) — pergunta criada SEM destino.
2026-11-09 00:37:45,017 WARNING chamados.services.disputa_emissao S160 _derivar_chamado_destino: 2 candidatos pro motivo 'orfao_14h' (disputa=4542, colab=917) — pergunta criada SEM destino.
REATAR universo colabs= 248 perguntas= 3359 erros= 0
REATAR reapontaria a Chamado TERMINAL (HIST)= 0 pks(ate 20)= []
REATAR reapontaria a Chamado vivo (derivado)= 11
REATAR reapontaria a Chamado de OUTRO colab= 0
REATAR chegaria ao lavrar (c1)= 3344 ao reemitir (c2, renascer possivel)= 3344 pks(ate 20)= [51431, 29870, 35204, 51830, 52437, 52500, 54837, 54898, 36476, 36477, 36484, 36485, 36486, 36487, 36488, 36489, 36490, 36491, 36492, 36493]
FIM sonda_v22b_acervo
```

**Cobertura integral das interceptações: [nao verificado].** O fonte `sonda_v22b_acervo.py` não acompanha o dado e não foi localizado na cópia; a saída não informa quais atributos foram substituídos nem seus retornos. Portanto, não consigo identificar uma escrita como comprovadamente **não interceptada**. [nao verificado]

A conferência do código mostra os seguintes pontos relevantes:

- `perguntas_orfas_reatavies` apenas monta o filtro; `reconciliar_perguntas_orfas_colab` percorre esse conjunto, chama `reatar_pergunta_orfa` e registra exceções. (`app/chamados/reconciliador.py:1495`, `app/chamados/reconciliador.py:1517`, `app/chamados/reconciliador.py:1522`, `app/chamados/reconciliador.py:1525`)
- **Costura:** `reapontar_resolvedora` salva a pergunta e pode retratar outras perguntas por colisão; essa retratação chega ao `save` de `carimbar_identidade`. Substituir a função inteira impede a execução desses caminhos internos. (`app/chamados/services/fio.py:913`, `app/chamados/services/fio.py:916`, `app/chamados/services/fio.py:1324`, `app/chamados/services/fio.py:1342`, `app/chamados/models.py:3180`)
- **c1:** pode obter/criar o usuário sistema, carimbar a pergunta e fechar a disputa. Substituir `lavrar_sem_turno_aberto` inteira impede esses caminhos internos. (`app/chamados/reconciliador.py:400`, `app/chamados/reconciliador.py:405`, `app/chamados/reconciliador.py:412`; escritas em `app/core/utils.py:18`, `app/chamados/models.py:3142`, `app/chamados/models.py:1934`)
- **c2:** pode criar ou renascer chamado e depois costurar a pergunta. Há também `cartorio.julgar_celula` durante a avaliação de `_ctx_c2`, **antes de entrar em `ChamadoColaborador.abrir`**. Interceptar somente `abrir` não bloqueia essa chamada; substituir `reemitir_chamado_do_dia` inteira bloqueia o caminho. (`app/chamados/reconciliador.py:702`, `app/chamados/reconciliador.py:910`, `app/chamados/reconciliador.py:920`, `app/chamados/reconciliador.py:954`, `app/chamados/models.py:922`, `app/chamados/models.py:971`)
- **Atenção ao HIST:** em `resolver_perguntas_por_chamado_resolvido(hist[0], get_user_sistema())`, o argumento `get_user_sistema()` é executado mesmo que o resolvedor tenha sido substituído. Ele contém `User.objects.get_or_create`, uma escrita potencial se o usuário não existir. Esse caminho depende de o reapontamento retornar `_ok=True`; sua interceptação pela sonda permanece [nao verificado]. (`app/chamados/reconciliador.py:1638`, `app/chamados/reconciliador.py:1639`, `app/chamados/reconciliador.py:1644`, `app/core/utils.py:18`)
- Os `save`s reais de pergunta também alcançam receivers: fechamento de beco após commit e emissão condicional de evento da célula. A emissão chega a `EventoCelula.objects.create`. (`app/chamados/signals.py:224`, `app/chamados/signals.py:225`, `app/chamados/signals.py:244`, `app/chamados/models.py:3238`, `app/ponto/portas/celula.py:891`)

**Import local: sim, pega o atributo substituído**, desde que a substituição esteja em `chamados.services.fio.reapontar_resolvedora` quando o import executar. O vínculo local é obtido dentro de cada chamada de `reatar_pergunta_orfa`; c2 também faz seu próprio import local. A substituição efetivamente feita pela sonda permanece [nao verificado]. (`app/chamados/reconciliador.py:1546`, `app/chamados/reconciliador.py:791`)

## 09/11 00:39 · sessao 572f905bcef1 · BUG-DEPLOY-PORTAO · verificador · origem=Code

1. **Achado funcional: pendências simultâneas deixam as duas portas bloqueadas.** Com `PEND=1` e `PEND_MSG=1`, o deploy normal recusa pelo mensageria e recomenda `--restart`; este recusa pelo core e recomenda deploy normal. A orientação nova não resolve essa combinação. O caso 17 cobre apenas sua recusa no modo normal; o 22 cobre somente mensageria pendente. Ocorrência em produção: [nao verificado]. (`bin/deploy.sh:200`, `bin/deploy.sh:202`, `bin/deploy.sh:219`, `bin/deploy.sh:225`; `bin/tests/test_deploy_portoes_antes_do_migrate.sh:198`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:218`)

2. **Pergunta 1 — não identifiquei caminho para apply do core com `RESTART=1`.** `APLICAR_MIGRATE` e `RESTART` nascem zerados, neutralizando valores herdados. O único escritor posterior de `APLICAR_MIGRATE=1` está no `else` depois da recusa de restart; `falhou` encerra o script. Os ramos anteriores também encerram ou, no conferir, não armam o apply. (`bin/deploy.sh:61`, `bin/deploy.sh:70`, `bin/deploy.sh:92`, `bin/deploy.sh:206`, `bin/deploy.sh:208`, `bin/deploy.sh:211`, `bin/deploy.sh:215`, `bin/deploy.sh:219`, `bin/deploy.sh:230`, `bin/deploy.sh:312`)

   As flags não são combinadas: apenas `$1` seleciona o modo. `--restart --sem-migrate` interpreta `--sem-migrate` como motivo; na ordem inversa, seleciona somente `--sem-migrate`. Isso também vale para `--sem-sombra`, `--conferir` e `--reload-*`. `DEPLOY_SEM_SOMBRA` herdado dispensa a sombra, sem alterar a autorização de migrate. (`bin/deploy.sh:56`, `bin/deploy.sh:63`, `bin/deploy.sh:64`, `bin/deploy.sh:65`, `bin/deploy.sh:72`, `bin/deploy.sh:74`, `bin/deploy.sh:82`, `bin/deploy.sh:85`, `bin/deploy.sh:86`, `bin/deploy.sh:152`)

3. **Pergunta 2 — somente mensageria pendente conserva o caminho ao restart, com os demais portões verdes.** `RESTART=1` escapa da recusa por `PEND_MSG`; `PEND=0` não arma o apply do core. Depois dos portões, chega à checagem final de cron e ao `docker restart saas_core saas_ui mensageria`. O compose declara o migrate do mensageria no boot; sua execução real em produção é [nao verificado]. (`bin/deploy.sh:200`, `bin/deploy.sh:204`, `bin/deploy.sh:302`, `bin/deploy.sh:312`, `bin/deploy.sh:335`, `bin/deploy.sh:338`, `bin/deploy.sh:339`; `docker-compose.mensageria.yml:20`)

4. **Pergunta 3 — censo dos leitores e escritores:**

   | Nome | Escritores | Leitores/chamadas |
   |---|---|---|
   | `PEND` | `bin/deploy.sh:185`, `187` | `bin/deploy.sh:189`, `204`, `209`, `212`, `216`, `224` |
   | `APLICAR_MIGRATE` | `bin/deploy.sh:61`, `230` | `bin/deploy.sh:312` |
   | `RESTART` | `bin/deploy.sh:70`, `73` | `bin/deploy.sh:115`, `200`, `219`, `335`, `441` |
   | `recusa_restart_com_cron` | definição em `bin/deploy.sh:110` | chamadas em `bin/deploy.sh:117`, `338` |

   O leitor textual externo é `bin/contrato_deploy.sh`: reconhece o ramo `RESTART`, exige a chamada antes do restart e exige a função. Não exige migrate no restart nem checagem na 1c. O selo injeta `APLICAR_MIGRATE` pelo ambiente; o outro `PEND`, em `sondas.sh`, é um diretório independente. (`bin/contrato_deploy.sh:41`, `bin/contrato_deploy.sh:47`, `bin/contrato_deploy.sh:50`; `bin/tests/test_deploy_portoes_antes_do_migrate.sh:109`; `bin/sondas.sh:18`, `bin/sondas.sh:21`)

   Não identifiquei leitor decisório que dependa do significado antigo: `PEND` continua booleano do check, `APLICAR_MIGRATE` autoriza o apply e `RESTART` seleciona a publicação por restart. A incompatibilidade está nas recomendações recíprocas do achado 1 e na descrição obsoleta do caso 14, abaixo. (`bin/deploy.sh:184`, `bin/deploy.sh:200`, `bin/deploy.sh:225`, `bin/deploy.sh:230`, `bin/deploy.sh:312`, `bin/deploy.sh:335`; `bin/tests/test_deploy_portoes_antes_do_migrate.sh:187`)

5. **Pergunta 4 — remover a chamada da 1c não cria caminho sem recusa antes do restart.** O único `docker restart` continua imediatamente depois de `recusa_restart_com_cron`; há também a chamada inicial. Permanece a corrida entre a amostragem final e o restart: o envelope pode criar uma marca nesse intervalo, sem trava compartilhada. É o residual explicitamente preservado, não uma proteção de exclusão. (`bin/deploy.sh:117`, `bin/deploy.sh:309`, `bin/deploy.sh:338`, `bin/deploy.sh:339`; `bin/cron_run.sh:9`, `bin/cron_run.sh:10`)

6. **Pergunta 5 — os quatro casos mordem as regressões nomeadas, por inspeção das asserções, com limites:**

   - **11:** exige erro, zero apply e zero restart. Se o migrate voltar e gerar o cron falso, a recusa final não mascara o apply: o contador o reprova. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:86`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:111`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:174`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:175`)
   - **13:** cobra a mesma recusa sem cron injetado, cobrindo o restart com core pendente e portões configurados verdes. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:182`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:183`)
   - **22:** exige sucesso, zero apply do core e um restart quando somente mensageria pende. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:218`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:219`)
   - **23:** exige esses mesmos resultados sem pendências injetadas. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:222`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:223`)

   **Limites:** 11/13 não fixam a seção nem a mensagem da recusa; movê-la para depois dos portões, ainda antes do apply, preservaria suas asserções. 22/23 contam qualquer comando `restart`, sem conferir seus containers; o docker falso não executa boot. Portanto, não provam a aplicação real da migration do mensageria. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:94`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:120`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:175`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:183`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:219`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:223`)

7. **Pergunta 6 — há duas afirmações documentais falsas.** “Antes de qualquer portão” contradiz a ordem: árvore, cron, auth e sombra vêm antes da recusa nova. Já “antes do banco andar”, no sentido de impedir o apply do core, corresponde ao fluxo. (`bin/deploy.sh:101`, `bin/deploy.sh:117`, `bin/deploy.sh:145`, `bin/deploy.sh:162`, `bin/deploy.sh:221`, `bin/deploy.sh:224`, `bin/deploy.sh:313`)

   O caso 14 ficou obsoleto: ainda diz que a **1c** recusa o cron nascido na casca, mas agora sua pendência de core aborta na seção 1, antes de executar a casca que criaria a marca. A 1c já não contém essa recusa. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:39`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:74`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:186`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:187`; `bin/deploy.sh:219`, `bin/deploy.sh:312`)

O selo comportamental não foi executado nesta revisão: ele cria e remove arquivos temporários. Resultado dinâmico dos casos: [nao verificado]. (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:60`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:64`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:131`)

## 09/11 00:39 · sessao d2e43dd5157c · - · - · origem=Ronald (tmux)

Auditoria das fronteiras, sem assumir os patches da sonda:

- `reatar_pergunta_orfa` chama c1 em `app/chamados/reconciliador.py:1615`; c1 obtém usuário em `:400`, carimba em `:405` e fecha disputa em `:412`. Escritas internas: `app/core/utils.py:18` (`User.objects.get_or_create`), `app/chamados/models.py:3142` (`carimbar.save`), `app/chamados/models.py:1934` (`DisputaSupervisao.fechar.save`) e auditoria em `app/chamados/models.py:1935` → `app/core/observ.py:134`.
- c2 é chamado em `app/chamados/reconciliador.py:1617`; chama `ChamadoColaborador.abrir` em `:910` e `reapontar_resolvedora` em `:954`. O argumento `defaults` avalia `_ctx_c2` em `:920`, que pode executar `cartorio.julgar_celula` em `:702` **antes da chamada a `abrir`**. Interceptar c2 inteiro cobre esse caminho; interceptar apenas `abrir` não cobre a avaliação de `_ctx_c2`. (`app/chamados/reconciliador.py:702`, `:910`, `:920`)
- `abrir` pode criar chamado em `app/chamados/models.py:971`; no existente encerrado chama renascimento em `:922`, cuja execução pode reabrir em `:1019` e anexar nota em `:1022`. A nota grava em `app/chamados/services/nota.py:103` ou `:107`.
- HIST: após reapontamento aprovado, a expressão `resolver_perguntas_por_chamado_resolvido(hist[0], get_user_sistema())` avalia `get_user_sistema()` antes de chamar o resolvedor. Assim, **mesmo com o resolvedor substituído**, o `get_or_create` do usuário continua alcançável, salvo interceptação adicional ou retorno `_ok=False` no reapontamento. (`app/chamados/reconciliador.py:1638`, `:1639`, `:1644`; `app/core/utils.py:18`)
- Resolver histórico, quando executado: carimba em `app/chamados/services/disputa_emissao.py:1764` e `:1838`, pode reabrir pai em `:1789`, inserir nota em `:1791`, auditar em `:1841` e fechar disputa em `:1865`. Gravações correspondentes: `app/chamados/models.py:3142`, `app/chamados/services/nota.py:103`, `app/core/observ.py:134`, `app/chamados/models.py:1934`.
- `_derivar_chamado_destino` consulta candidatos e pode emitir WARNING; não contém gravação ORM no corpo lido. (`app/chamados/services/disputa_emissao.py:198`, `:226`, `:240`, `:254`)
- `_dia_fora_do_alcance_c` escreve somente memo na instância no corpo lido, em `app/chamados/reconciliador.py:748`.

Sem o código da sonda, quais desses pontos foram efetivamente interceptados permanece **[nao verificado]**.

## 09/11 00:38 · sessao 5900d8a4c6d7 · - · - · origem=Ronald (tmux)

- **Bloqueio circular quando ambos pendem:** modo normal recusa `PEND_MSG>0` e recomenda `--restart`; este recusa `PEND>0` e recomenda modo normal. Nenhum dos dois avança com `PEND=1/PEND_MSG=1`; `--conferir` apenas consulta. Referências: `bin/deploy.sh:200`, `bin/deploy.sh:202`, `bin/deploy.sh:219`, `bin/deploy.sh:225`, `bin/deploy.sh:234`. Ocorrência em produção: **[nao verificado]**. O caso 17 cobre apenas a recusa normal com ambos pendentes; o 22 cobre somente mensageria pendente: `bin/tests/test_deploy_portoes_antes_do_migrate.sh:198`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:218`.

- **Censo `PEND`:** escritores `bin/deploy.sh:185`, `bin/deploy.sh:187`; leitores `bin/deploy.sh:189`, `bin/deploy.sh:204`, `bin/deploy.sh:209`, `bin/deploy.sh:212`, `bin/deploy.sh:216`, `bin/deploy.sh:224`. O homônimo em `bin/sondas.sh:18` é caminho de diretório, lido em `bin/sondas.sh:19`, `bin/sondas.sh:21`, `bin/sondas.sh:22`.

- **Censo `APLICAR_MIGRATE`:** escritores `bin/deploy.sh:61`, `bin/deploy.sh:230`; único leitor decisório `bin/deploy.sh:312`. O selo injeta o valor de ambiente em `bin/tests/test_deploy_portoes_antes_do_migrate.sh:109` e cobra sua neutralização em `bin/tests/test_deploy_portoes_antes_do_migrate.sh:170`.

- **Censo `RESTART`:** escritores `bin/deploy.sh:70`, `bin/deploy.sh:73`; leitores `bin/deploy.sh:115`, `bin/deploy.sh:200`, `bin/deploy.sh:219`, `bin/deploy.sh:335`, `bin/deploy.sh:441`. O último escolhe apenas a mensagem final: `bin/deploy.sh:442`, `bin/deploy.sh:444`.

- **Censo `recusa_restart_com_cron`:** definição `bin/deploy.sh:110`; chamadas `bin/deploy.sh:117`, `bin/deploy.sh:338`. O contrato externo reconhece textualmente o ramo `RESTART` e exige chamada antes do restart e definição da função: `bin/contrato_deploy.sh:41`, `bin/contrato_deploy.sh:47`, `bin/contrato_deploy.sh:50`. Esses leitores não exigem migrate no restart nem rechecagem na 1c: `bin/contrato_deploy.sh:41`, `bin/contrato_deploy.sh:45`, `bin/contrato_deploy.sh:47`.

- **Sem leitor decisório com significado antigo identificado:** `PEND` permanece resultado booleano do check; `APLICAR_MIGRATE` continua autorização de apply; `RESTART` continua seleção da publicação por restart. A incompatibilidade encontrada está nas recomendações operacionais recíprocas, quando ambos pendem: `bin/deploy.sh:184`, `bin/deploy.sh:200`, `bin/deploy.sh:225`, `bin/deploy.sh:230`, `bin/deploy.sh:312`, `bin/deploy.sh:335`.

## 09/11 00:38 · sessao 3198ce51949f · - · - · origem=Ronald (tmux)

- **Comentário falso:** `bin/deploy.sh:221` promete recusa “antes de qualquer portao”, porém árvore, cron, auth e sombra já passaram em `bin/deploy.sh:101`, `bin/deploy.sh:117`, `bin/deploy.sh:145` e `bin/deploy.sh:162`. A recusa nova ocorre em `bin/deploy.sh:219`. A promessa de impedir o apply do core é sustentada pela atribuição exclusiva em `bin/deploy.sh:230` e pelo apply em `bin/deploy.sh:312`.

- **Caso 14 ficou com explicação falsa:** o cabeçalho e a mensagem atribuem a recusa à 1c (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:39`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:187`). Mas ele fornece core pendente em `bin/tests/test_deploy_portoes_antes_do_migrate.sh:186`, portanto aborta na seção 1 (`bin/deploy.sh:219`), antes da prova que criaria o cron (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:74`). A 1c também já não tem recusa por cron (`bin/deploy.sh:312`).

- **Casos 11 e 13 mordem o retorno do migrate no restart:** ambos exigem `RC!=0`, zero apply e zero restart (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:175`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:183`). O contador exclui `--check` e `--plan` (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:111`). No 11, a marca só surgiria no apply falso (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:86`); mesmo que a recusa final por cron evitasse restart, o apply contado reprovaria o caso (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:175`). O 13 também cobre o caminho sem cron injetado (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:182`).

- **Casos 22 e 23 mordem o fechamento indevido do restart:** o 22 injeta somente pendência de mensageria e exige sucesso, zero apply do core e uma chamada restart (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:218`); o 23 exige o mesmo sem pendência injetada (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:222`). Eles cobrem uma recusa ampliada indevidamente para mensageria ou para todo restart (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:219`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:223`).

- **Limites dos quatro casos:** 11/13 não verificam a seção ou mensagem da recusa nem ausência de chamadas aos portões; verificam apenas os três resultados (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:175`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:183`). Mover a recusa para depois dos portões, mantendo-a antes do apply, preservaria essas asserções (`bin/deploy.sh:302`, `bin/deploy.sh:312`). 22/23 contam qualquer linha `restart`, sem conferir os containers (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:120`); o docker falso devolve sucesso sem executar boot (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:94`). Assim, não provam aplicação real da migration de mensageria [nao verificado].

- **Demais comentários novos:** “nunca num --restart”, entendido como apply do core, combina com o fluxo (`bin/deploy.sh:219`, `bin/deploy.sh:230`, `bin/deploy.sh:312`). O boot declarado de mensageria executa migrate (`docker-compose.mensageria.yml:20`). A janela entre a amostragem final de cron e o restart continua explícita no código (`bin/deploy.sh:338`, `bin/deploy.sh:339`); o envelope cria marca sem trava compartilhada e executa o comando uma vez (`bin/cron_run.sh:9`, `bin/cron_run.sh:11`). Testes não executados nesta revisão [nao verificado].

## 09/11 00:38 · sessao 7a58b9f3bf40 · - · - · origem=Ronald (tmux)

1. **Não há caminho ordinário com `RESTART=1` e apply do core.** `APLICAR_MIGRATE` e `RESTART` são zerados, anulando valores herdados; somente o modo `--restart` arma `RESTART`. O único escritor posterior de `APLICAR_MIGRATE=1` está no `else` depois da recusa por `RESTART`; `falhou` encerra o shell. (`bin/deploy.sh:61`, `bin/deploy.sh:70`, `bin/deploy.sh:72`, `bin/deploy.sh:92`, `bin/deploy.sh:219`, `bin/deploy.sh:230`, `bin/deploy.sh:312`)

2. **As opções não são combinadas pelo parser.** Apenas `$1` define o modo; `$2` é motivo em `--restart` e `--sem-sombra`. Portanto, `--restart --sem-migrate` interpreta `--sem-migrate` como motivo, mantendo a recusa do core; na ordem inversa o modo será `--sem-migrate`, sem restart. `DEPLOY_SEM_SOMBRA` altera somente a dispensa da sombra, sem armar migration. (`bin/deploy.sh:56`, `bin/deploy.sh:63`, `bin/deploy.sh:72`, `bin/deploy.sh:74`, `bin/deploy.sh:82`, `bin/deploy.sh:85`, `bin/deploy.sh:86`, `bin/deploy.sh:152`, `bin/deploy.sh:219`)

3. **Mensageria pendente com core sem pendência conserva o restart, condicionado aos outros portões.** `RESTART=1` dispensa a recusa de mensageria; `PEND=0` não arma o apply; o fluxo chega à recusa final por cron e ao comando de restart. O compose declara migration da mensageria no boot. (`bin/deploy.sh:200`, `bin/deploy.sh:204`, `bin/deploy.sh:230`, `bin/deploy.sh:335`, `bin/deploy.sh:338`, `bin/deploy.sh:339`, `docker-compose.mensageria.yml:20`)

4. **Remover a checagem da 1c não deixa caminho sem chamada de recusa antes do restart.** Há recusa inicial e outra imediatamente anterior ao único `docker restart`; `falhou` encerra a execução. Permanece a janela entre a amostragem final e o comando: o envelope cria sua marca independentemente, sem exclusão comum. (`bin/deploy.sh:92`, `bin/deploy.sh:115`, `bin/deploy.sh:117`, `bin/deploy.sh:338`, `bin/deploy.sh:339`, `bin/cron_run.sh:9`, `bin/cron_run.sh:10`, `bin/cron_run.sh:11`)

5. **Comentário falso de ordem:** “Recusa AQUI, antes de qualquer portao” não corresponde ao fluxo. A árvore, o cron e os portões de auth/sombra vêm antes da seção 1; `--sem-sombra` pode inclusive gravar seu log antes dessa recusa. (`bin/deploy.sh:221`, `bin/deploy.sh:101`, `bin/deploy.sh:117`, `bin/deploy.sh:145`, `bin/deploy.sh:160`, `bin/deploy.sh:162`, `bin/deploy.sh:224`)

6. **“Única porta” da migration da mensageria precisa ser lido no escopo do deploy.** O compose executa `migrate` em qualquer boot que use esse comando; o código prova que `--restart` é a porta deste script para provocá-lo. Exclusividade operacional fora dele: [nao verificado]. (`bin/deploy.sh:223`, `bin/deploy.sh:335`, `bin/deploy.sh:339`, `docker-compose.mensageria.yml:20`)

Nenhum teste foi executado; comportamento real dos containers, banco e marcas de produção: [nao verificado]. [nao verificado]

## 09/11 00:35 · sessao 50955c5d0d17 · FIO-E0-V4-E1-FILA · sonda · origem=Code

1. Transcrição dos números fornecidos; verificação independente em produção **[nao verificado]**:

```text
S143 P com destino= 38403
S143 destino de outro colab (vs disputa.colaborador) total= 0 alcancavel_pelo_S143= 0
S143 destino de outro colab (vs P.colaborador) total= 0 alcancavel_pelo_S143= 0
REATAR orfas_abertas= 5525
REATAR orfa com chamado_destino terminal= 1060
REATAR orfa com Chamado terminal do colab no dia (alcancavel pelo HIST/renascer)= 1851
```

2. **A equivalência dos filtros da sonda com o código é [nao verificado]**: foi fornecida a saída, sem o corpo de `sonda_v22_acervo.py`; sua implementação não foi localizada na cópia **[nao verificado]**.

Os filtros efetivos do código são:

- **S143, seleção de perguntas:** destino igual ao chamado, `materializada_em IS NULL` e disputa aberta. Não exige igualdade entre os colaboradores do destino, da disputa e da Pergunta; também não exige `validada_em IS NULL` nem data presente. Depois, pula pergunta em voo, viva, de dia trancado ou respondida sem materialização de fato (`app/chamados/services/disputa_emissao.py:1724`, `1738`, `1744`, `1748`, `1757`). O receiver exige chamado em `ENCERRADOS` e gravação do status (`app/chamados/signals.py:96`, `104`).

- **REATAR, seleção do chamador:** destino **nulo**, colaborador da disputa, disputa aberta, materialização nula e motivo entre os quatro do grupo DIA. Não exige validação nula nem data presente (`app/chamados/reconciliador.py:1496`; `app/chamados/models.py:3690`). Portanto, não se pode equiparar automaticamente esse universo às 5525 “órfãs” da saída: o mesmo dado inclui 1060 “órfãs” com destino terminal **[nao verificado]**.

- **HIST:** só é tentado quando a derivação não encontra destino. Usa `p.data_evento`, com fallback para a data do contexto; busca mesmo colaborador, `ENCERRADOS`, igualdade de `contexto_json.data_turno` e categoria diferente de `disputa_supervisao`. Ainda filtra motivo, desvia a viva para c2 dentro do alcance novo e exige exatamente um candidato histórico (`app/chamados/reconciliador.py:1552`, `1563`, `1582`, `1589`, `1600`, `1603`, `1619`). **HIST não chama o renascimento:** reaponta e chama o resolvedor S128 (`app/chamados/reconciliador.py:1638`, `1644`).

- **c2/reemitir:** chega após HIST vazio e recusa de c1. Aplica gates de motivo, data, colaborador ativo, horizonte, isenção, ausência, protesto, parcial só lista, célula, régua e motor; para intervalos, exige também viva em competência aberta. Abre pela chave colaborador + `batida_ausente` + `data_turno`; só costura se o resultado estiver vivo (`app/chamados/reconciliador.py:1615`, `808`, `820`, `835`, `850`, `855`, `869`, `875`, `886`, `900`, `906`, `908`, `910`, `936`).

A condição isolada **“há terminal do colaborador no dia” é mais larga que a elegibilidade efetiva de HIST/c2**, pelos filtros acima. Se a sonda aplica esses filtros adicionais ou restringe indevidamente validação/data/motivos, é **[nao verificado]**. Também há diferença de vocabulário: `TERMINAIS` inclui cancelado/superado; HIST e o dedup que tenta renascer usam `ENCERRADOS`, apenas resolvido/fechado (`app/chamados/catalogo/motor.py:30`; `app/chamados/reconciliador.py:1583`; `app/chamados/models.py:921`).

3. **Existe caminho de Pergunta até tentativa de renascimento fora de HIST/c2; afirmar que a sonda não o conta é [nao verificado].**

Um caminho concreto é:

- Materialização da Pergunta registra Batida (`app/chamados/services/materializacao.py:926`; `app/ponto/registro_batida.py:157`).
- O signal da Batida chama o cartório (`app/ponto/signals.py:131`, `93`).
- Persistindo furo parcial, com aplicação, sem chamado vivo e sem bloqueio de “parcial só lista”, o cartório chama o emissor (`app/ponto/services/cartorio.py:1767`, `1777`).
- O emissor chama `abrir` por colaborador, módulo e dia, sem filtro de Pergunta nessa chave. Encontrando chamado encerrado, o dedup tenta `_renascer_por_premissa_viva`, sujeito às guardas e à aceitação de `reabrir` (`app/ponto/management/commands/detectar_ausencias.py:693`; `app/chamados/models.py:906`, `921`, `1005`, `1019`).

Esse caminho existe no código; sua ocorrência, quantidade e exclusão dos **1851** informados são **[nao verificado]**.

## 09/11 00:35 · sessao 7bc8cd84a86b · - · - · origem=Ronald (tmux)

Há dois pontos produtivos que chamam `_renascer_por_premissa_viva`: o dedup de `ChamadoColaborador.abrir`, ao encontrar `ENCERRADOS` (`app/chamados/models.py:906`, `app/chamados/models.py:921`), e a resposta tardia no fio, se a célula acusa furo (`app/chamados/services/resposta_tardia.py:35`, `app/chamados/services/resposta_tardia.py:45`).

Caminho concreto a partir de Pergunta, adicional ao HIST/c2:

1. Materialização da resposta hora usa a Pergunta e registra Batida (`app/chamados/services/materializacao.py:794`, `app/chamados/services/materializacao.py:926`).
2. A porta cria Batida (`app/ponto/registro_batida.py:157`); o `post_save` julga os dias (`app/ponto/signals.py:131`, `app/ponto/signals.py:93`).
3. O cartório entrega o emissor a `julgar_colab` (`app/ponto/services/cartorio.py:1964`, `app/ponto/services/cartorio.py:1989`).
4. Persistindo furo parcial, sem chamado vivo, com `apply_` e sem bloqueio `parcial_so_lista`, o cartório chama `_abrir_chamado` (`app/ponto/services/cartorio.py:1767`, `app/ponto/services/cartorio.py:1777`).
5. O emissor chama `ChamadoColaborador.abrir` por colaborador, módulo e `data_turno`, sem filtro de Pergunta nem status nessa chave (`app/ponto/management/commands/detectar_ausencias.py:693`); o dedup pode encontrar um encerrado e tentar renascê-lo (`app/chamados/models.py:906`, `app/chamados/models.py:921`).

A tentativa só efetivamente renasce passando pelas guardas: colaborador ativo, sem encerramento por cascata, competência não trancada, ausência de veto de régua e aceitação por `reabrir` (`app/chamados/models.py:1005`, `app/chamados/models.py:1019`).

A resposta tardia também tenta renascer sem selecionar Pergunta ou órfã; a view busca diretamente o Chamado e chama o serviço (`app/chamados/views.py:1046`, `app/chamados/views.py:1053`). Esse caminho é resposta no fio, não resposta direta de `PerguntaDisputa` (`app/chamados/views.py:1041`).

**Não é possível afirmar que o item (2) da sonda deixa esses caminhos fora da contagem sem a fonte do script** [nao verificado]. O próprio dado fornecido admite “órfã com chamado_destino terminal”; portanto, “órfã” não permite inferir `chamado_destino IS NULL` nesse script [nao verificado].

`_ressuscitar_por_resposta` não é prova de renascimento do status: o corpo apenas levanta o silêncio do destino ou pai com `arquivo.levantar` (`app/chamados/services/materializacao.py:711`, `app/chamados/services/materializacao.py:718`).

## 09/11 00:19 · sessao b599f9f2e362 · FIO-E0-V4-E1-FILA · verificador · origem=Code

A cura produtiva está coerente com a normalização da trava; a cura dos testes ficou **parcial na mensagem e na cobertura da conferência**. A mensagem atual imprime as três parcelas corretamente, mas os testes só exigem os IDs da disputa. (`app/chamados/services/fio.py:2092`; `app/chamados/services/fio.py:2098`; `app/chamados/tests/test_e1_fila.py:507`; `app/chamados/tests/test_e1_fila.py:519`)

Execução da suíte, conteúdo de `e1_fila_red5.log`, estado do banco e ocorrência em produção: **[nao verificado]**. [nao verificado]

**1. Triagem: o que procede e o que ficou parcial**

| Item da triagem | Verificação estática |
|---|---|
| `None` comparado, mas não travável | **Curado como alinhamento de conjuntos:** `_pks` ignora `None`; `_fora[2]` agora também. Porém, `PerguntaDisputa.disputa` é FK sem `null=True`: o teste fabrica a segunda leitura, sem gravar NULL. Não demonstra uma ocorrência produtiva. (`app/chamados/models.py:2661`; `app/chamados/services/fio.py:2093`; `app/chamados/models.py:2424`; `app/chamados/tests/test_e1_fila.py:509`) |
| Resultado preenchido sem exigir migração | **Curado:** exige `([p.pk], [], [])` e relê o destino gravado. O MORDE sem laço exige lista vazia e destino antigo. (`app/chamados/tests/test_e1_fila.py:547`; `app/chamados/tests/test_e1_fila.py:555`) |
| Diferença trocada por XOR | **Curado para o encolhimento exercitado:** a segunda leitura vazia deve passar. XOR produziria parcelas antigas e recusaria. (`app/chamados/tests/test_e1_fila.py:521`; `app/chamados/services/fio.py:2092`) |
| Cardinalidade no lugar do conjunto de disputas | **Curado:** substitui a disputa mantendo a pergunta e exige recusa. (`app/chamados/tests/test_e1_fila.py:509`; `app/chamados/tests/test_e1_fila.py:513`) |
| Mensagem sem IDs | **Parcial:** exige somente `disputas [1000000000]`; apagar os IDs de colaboradores ou perguntas da mensagem conserva os asserts existentes. (`app/chamados/tests/test_e1_fila.py:494`; `app/chamados/tests/test_e1_fila.py:507`; `app/chamados/tests/test_e1_fila.py:519`) |
| Duas leituras só conferidas sem exceção | **Curado:** a asserção fica depois do `except`, nos dois ramos. (`app/chamados/tests/test_e1_fila.py:482`) |
| Remoção de `key=str` | **Coerente:** a disputa referencia um `BigAutoField`; `None` sai antes da ordenação. (`app/chamados/migrations/0013_disputasupervisao.py:20`; `app/chamados/services/fio.py:2093`; `app/chamados/services/fio.py:2098`) |

Os dois itens de FILA pertencem à investigação desta obra: são campos usados para escolher as travas, e o escopo inclui seus escritores e leitores. Isso não os transforma em defeitos concorrentes demonstrados; falta um escritor produtivo que altere esses FKs depois do nascimento. (`CLAUDE.md:376`; `app/chamados/services/fio.py:2085`; `app/chamados/models.py:3420`; alcance produtivo **[nao verificado]**)

**2. Quem escreve, quem lê e o que efetivamente se compara**

O conjunto nasce das perguntas apontadas para velho/novo, acrescido de `universo_da_linha` de cada uma. A mesma função é chamada antes e depois da trava. (`app/chamados/services/fio.py:2032`; `app/chamados/services/fio.py:2084`; `app/chamados/services/fio.py:2088`)

| Campo/valor | Escritor | Leitores relevantes ao ato |
|---|---|---|
| `Pergunta.colaborador_id` | No nascimento, `_chave_de_identidade` deriva da disputa ou recusa divergência. Há também o preenchimento histórico da migration 0051. (`app/chamados/models.py:3430`; `app/chamados/migrations/0051_decisao_pergunta.py:25`) | Composição de `_cs`, conferência, trava de Colaborador e juiz da costura. (`app/chamados/services/fio.py:2085`; `app/chamados/services/fio.py:2092`; `app/chamados/models.py:2674`; `app/chamados/juizes.py:2003`) |
| `Pergunta.pk` | Identidade automática `BigAutoField`; a porta insere com `force_insert=True`. (`app/chamados/migrations/0014_perguntadisputa.py:19`; `app/chamados/models.py:3019`) | Lista para travar Perguntas, conferência e composição dos grupos do laço. (`app/chamados/services/fio.py:2087`; `app/chamados/services/fio.py:2092`; `app/chamados/services/fio.py:2143`) |
| `Pergunta.disputa_id` | A disputa entra no construtor usado pelo nascimento; depois ocorre a inserção. (`app/chamados/models.py:3016`; `app/chamados/models.py:3019`) | Lista para travar Disputas, conferência e portas internas de retratação. (`app/chamados/services/fio.py:2086`; `app/chamados/services/fio.py:2093`; `app/chamados/services/fio.py:1330`) |
| Donos de velho/novo | O nascimento do Chamado recebe `colaborador`; `criar` o persiste. (`app/chamados/models.py:1054`; `app/chamados/models.py:1095`) | Entram em `_cs` pelas instâncias recebidas; a conferência não relê esses donos. (`app/chamados/services/fio.py:2085`; `app/chamados/services/fio.py:2092`) |
| Mensagem de `UniversoMudouSobATrava` | Montada com velho/novo e as três parcelas ordenadas. (`app/chamados/services/fio.py:2095`) | `reabrir` captura, registra `str(_e)[:300]` em `depois['erro']` e retorna `False`; o helper de teste captura a string inteira. (`app/chamados/models.py:1417`; `app/chamados/models.py:1424`; `app/chamados/tests/test_e1_fila.py:489`) |

O evento é persistido pela porta `core.observ.evento`, que escreve `descricao` e `valor_depois` em `LogAuditoria`; portanto o registro produtivo pode conter somente os primeiros 300 caracteres do erro. (`app/core/observ.py:94`; `app/core/observ.py:134`; `app/chamados/models.py:1424`)

Se `P0` é a primeira leitura e `P1` a segunda, a relação é esta:

| Classe | IDs pedidos à trava | Parcela que provoca recusa |
|---|---|---|
| Colaborador | `C0 = {dono velho, dono novo} ∪ colaboradores(P0)` | `colaboradores(P1) − C0` |
| Pergunta | `pk(P0)` | `pk(P1) − pk(P0)` |
| Disputa | `disputas(P0) − {None}` após normalização | `(disputas(P1) − disputas(P0)) − {None}` |
| Chamado | `{velho.pk, novo.pk}` | Não há parcela de conferência para Chamado. |

Essas expressões vêm da chamada e da formação de `_fora`; a normalização está em `_pks`. (`app/chamados/services/fio.py:2085`; `app/chamados/services/fio.py:2086`; `app/chamados/services/fio.py:2092`; `app/chamados/models.py:2661`)

Logo:

- **A conferência exige cobertura, não igualdade:** encolher é permitido; linhas da primeira leitura podem continuar travadas mesmo ausentes na segunda. (`app/chamados/services/fio.py:2092`; `app/chamados/tests/test_e1_fila.py:521`)
- **Para `disputa_id` inteiro, não há filtro divergente entre os lados:** `_pks` não descarta zero ou negativos; descarta `None`, assim como a conferência. (`app/chamados/models.py:2661`; `app/chamados/services/fio.py:2093`)
- **Não há valor `None` comparado como nova disputa faltante na v5.** O inverso possível é uma disputa antiga continuar travada após desaparecer da segunda leitura, comportamento deliberadamente aceito. (`app/chamados/services/fio.py:2093`; `app/chamados/tests/test_e1_fila.py:529`)
- **IDs calculados não são comprovantes de linhas adquiridas:** as queries travam linhas existentes, e a migração não usa o retorno da trava para conferir aquisições. Integridade efetiva do banco: **[nao verificado]**. (`app/chamados/models.py:2674`; `app/chamados/models.py:2683`; `app/chamados/models.py:2702`; `app/chamados/services/fio.py:2086`)

A ordem implementada é Colaborador → Disputa → Chamado → Pergunta, por PK em cada classe. (`app/chamados/models.py:2674`; `app/chamados/models.py:2683`; `app/chamados/models.py:2696`; `app/chamados/models.py:2700`)

**3. Mordida dos testes e mutantes residuais** [nao verificado]

Os testes novos distinguem estaticamente retirar `-{None}`, usar XOR, comparar cardinalidade das disputas e suprimir o laço. O instrumento de mutação recompila a função real e exige uma única ocorrência da âncora. (`app/chamados/tests/test_e1_fila.py:48`; `app/chamados/tests/test_e1_fila.py:513`; `app/chamados/tests/test_e1_fila.py:521`; `app/chamados/tests/test_e1_fila.py:526`; `app/chamados/tests/test_e1_fila.py:555`)

Persistem estes mutantes plausíveis **pelos oráculos inspecionados**; sobrevivência executada na suíte inteira: **[nao verificado]**. [nao verificado]

| Mutante | Por que os asserts atuais não o distinguem |
|---|---|
| Na mensagem, trocar `sorted(_fora[0])` ou `sorted(_fora[1])` por `[]` | Colaborador/Pergunta exigem apenas mensagem verdadeira; só Disputa tem conteúdo exigido. (`app/chamados/services/fio.py:2098`; `app/chamados/tests/test_e1_fila.py:507`; `app/chamados/tests/test_e1_fila.py:519`) |
| Imprimir todas as disputas de `_sob`, em vez de somente as externas | No caso que verifica texto, a única disputa foi substituída; o caso que acrescenta disputa só exige recusa. (`app/chamados/tests/test_e1_fila.py:502`; `app/chamados/tests/test_e1_fila.py:509`; `app/chamados/tests/test_e1_fila.py:519`) |
| Trocar a condição `_fora[1]` por crescimento da cardinalidade dos PKs | Há acréscimo de PK e esvaziamento, mas não substituição de PK mantendo cardinalidade. (`app/chamados/services/fio.py:2094`; `app/chamados/tests/test_e1_fila.py:498`; `app/chamados/tests/test_e1_fila.py:523`) |
| Comparar apenas cardinalidades dos colaboradores de `P1` e `P0` | O caso exclusivo de colaborador acrescenta um valor; não substitui o conjunto mantendo tamanho. (`app/chamados/tests/test_e1_fila.py:500`; `app/chamados/tests/test_e1_fila.py:509`) |
| Executar o laço imediatamente antes do `if _fora` | Os casos de corrida usam costura cruzada, já recusada pela porta; depois a exceção continua ocorrendo. O cenário efetivamente migrável só é usado sem corrida. (`app/chamados/tests/test_e1_fila.py:147`; `app/chamados/tests/test_e1_fila.py:547`; `app/chamados/tests/test_e1_fila.py:563`; `app/chamados/services/fio.py:863`) |

O teste “nada migra” comprova recusa e destino final nesse cenário; **não comprova que uma migração possível foi impedida ou desfeita**. A segunda leitura é simulada, enquanto o laço volta a consultar o banco real. (`app/chamados/tests/test_e1_fila.py:474`; `app/chamados/tests/test_e1_fila.py:563`; `app/chamados/services/fio.py:2118`; `app/chamados/services/fio.py:2122`)

**4. Escritores produtivos dos dois itens de FILA**

**Não identifiquei escritor produtivo que troque esses FKs após o nascimento; não há prova universal de ausência — alcance concorrente [nao verificado].**

- **Dono do Chamado:** sua fotografia realmente não é relida para `_cs`. A atribuição aparente em `pergunta_no_app` apenas compartilha a instância do mesmo colaborador dentro de grupos formados pelo próprio ID. A porta dinâmica de texto aceita somente título, assunto e descrição. (`app/chamados/services/fio.py:2085`; `app/chamados/services/pergunta_no_app.py:133`; `app/chamados/services/pergunta_no_app.py:139`; `app/chamados/services/texto.py:18`; `app/chamados/services/texto.py:25`)
- **Colaborador/disputa da Pergunta:** a possibilidade programática existe: depois de `_state.adding`, `save` não repete a guarda da identidade. Isso prova permissividade da API, não um escritor que efetivamente altere os campos. (`app/chamados/models.py:3420`; `app/chamados/models.py:3423`; `app/chamados/models.py:3430`)
- **Escritor histórico encontrado:** a migration 0051 preenche `colaborador_id` por `Subquery`, via `RunPython`; não é um caminho da execução normal da migração de resolvedoras. (`app/chamados/migrations/0051_decisao_pergunta.py:25`; `app/chamados/migrations/0051_decisao_pergunta.py:101`)
- **Porta ampla encontrada:** `descarimbar_em_lote(**outros)` admite outros campos, mas os chamadores rastreados passam resposta/validação. O outro lote também usa dicionário fixo desses campos. (`app/chamados/models.py:3252`; `app/chamados/models.py:3266`; `app/chamados/models.py:1565`; `app/chamados/services/disputa_emissao.py:1517`; `app/chamados/services/disputa_emissao.py:1554`; `app/chamados/services/disputa_emissao.py:1556`)
- **Escritas deste ato:** reapontamento grava destino/lavra, eventualmente dia; retratação grava carimbo e campos associados. Não alteram os três FKs investigados. (`app/chamados/services/fio.py:866`; `app/chamados/services/fio.py:879`; `app/chamados/services/fio.py:922`; `app/chamados/models.py:3128`; `app/chamados/models.py:3152`)

Portanto, os dois FILA sustentam **riscos latentes da interface**, mas não sustentam, com a evidência disponível, uma corrida produtiva bloqueante. (`app/chamados/models.py:3420`; `app/chamados/services/fio.py:2085`; corrida produtiva **[nao verificado]**)

**5. Classes que já reprovaram obra**

| Classe | Resultado nesta verificação |
|---|---|
| Segundo juiz duplicando registrado | **Não nas linhas tocadas:** a guarda decide cobertura de travas; vitalidade continua delegada a `pergunta_viva`, registrada como autoridade. (`app/chamados/services/fio.py:2026`; `app/chamados/services/fio.py:2113`; `app/core/juizes.py:266`) |
| Segundo escritor fora da porta | **Não nas linhas tocadas:** o diff modifica conjuntos/mensagem; o laço continua usando reapontamento e retratação pelas portas existentes. (`app/chamados/services/fio.py:2092`; `app/chamados/services/fio.py:2165`; `app/chamados/services/fio.py:2171`) |
| Relógio fora da borda | **Não no delta v5:** a conferência não lê relógio. No caminho histórico de expiração, `_hoje` usa `timezone.now` ou o instante recebido. (`app/chamados/services/fio.py:2092`; `app/chamados/veredito_lavrado.py:95`) |
| Derivação paralela de fonte única | **Não na construção do universo:** as duas leituras usam `_conjunto_da_migracao`; esta e o laço usam `universo_da_linha`. (`app/chamados/services/fio.py:2032`; `app/chamados/services/fio.py:2084`; `app/chamados/services/fio.py:2088`; `app/chamados/services/fio.py:2142`) |
| Selo/RED sem mordida | **Sim, residual de cobertura:** IDs de duas classes, substituição de PK e precedência da conferência ainda têm os escapes descritos acima. (`app/chamados/tests/test_e1_fila.py:507`; `app/chamados/tests/test_e1_fila.py:519`; `app/chamados/tests/test_e1_fila.py:563`) |
| Ordem de trava | **Não há inversão demonstrada neste ato:** uma chamada adquire as quatro classes na ordem; crescimento provoca exceção antes do laço. (`app/chamados/services/fio.py:2086`; `app/chamados/services/fio.py:2094`; `app/chamados/models.py:2674`; `app/chamados/models.py:2700`) |
| Pouso antes do pré-requisito | **[nao verificado]**. A regra exige suíte verde antes do opus e opus antes do pouso; os anexos não comprovam essa sequência. (`CLAUDE.md:374`; `CLAUDE.md:378`) | [nao verificado]
| Campo com dois significados | **Não introduzido pelo delta:** `disputa_id` continua sendo FK de Disputa; `None` é ignorado na normalização. A igualdade colaborador da Pergunta/disputa é verificada apenas no nascimento; divergência produtiva posterior **[nao verificado]**. (`app/chamados/models.py:2424`; `app/chamados/models.py:2661`; `app/chamados/models.py:3420`; `app/chamados/models.py:3433`) |

## 09/11 00:17 · sessao aad97d481b14 · - · - · origem=Ronald (tmux)

Não localizei escritor de produção que **mude**, após o nascimento, `ChamadoColaborador.colaborador_id`, `PerguntaDisputa.colaborador_id` ou `PerguntaDisputa.disputa_id`. O alcance concorrente dos dois itens de FILA permanece **[nao verificado]**, sem escritor concreto demonstrado.

Evidências e exclusões:

- **Nascimento da Pergunta:** `disputa` entra no construtor e a inserção ocorre com `force_insert=True` (`app/chamados/models.py:3016`, `app/chamados/models.py:3019`); `colaborador_id` é derivado da disputa ou validado contra ela (`app/chamados/models.py:3430`, `app/chamados/models.py:3432`, `app/chamados/models.py:3433`).
- **Mutabilidade latente existe:** `save()` só chama a guarda da identidade quando `_state.adding` (`app/chamados/models.py:3420`); depois repassa ao `super().save()` (`app/chamados/models.py:3423`). Isso permite escrita programática futura, mas não demonstra um chamador produtivo que a faça.
- **Escritor histórico:** a migração preenche `colaborador_id` por `Subquery` (`app/chamados/migrations/0051_decisao_pergunta.py:25`), registrada em `RunPython` (`app/chamados/migrations/0051_decisao_pergunta.py:101`). Não é escritor da execução normal de `migrar_resolvedoras_vivas`.
- **A atribuição aparente `colaborador_id` no fluxo de abertura é proxy:** o objeto é `_Proxy`, não modelo persistido (`app/chamados/models.py:2888`, `app/chamados/models.py:2890`, `app/chamados/models.py:2894`).
- **A atribuição aparente de dono do Chamado não troca o ID:** os chamados são agrupados pelo próprio `colaborador_id`, e a mesma instância é compartilhada no grupo, sem save (`app/chamados/services/pergunta_no_app.py:133`, `app/chamados/services/pergunta_no_app.py:139`, `app/chamados/services/pergunta_no_app.py:141`).
- **`descarimbar_em_lote(**outros)` seria uma porta ampla:** aceita outros campos e escreve por `update` (`app/chamados/models.py:3252`, `app/chamados/models.py:3266`), mas os chamadores encontrados passam apenas resposta/validação (`app/chamados/models.py:1565`; `app/chamados/services/disputa_emissao.py:1517`, `app/chamados/services/disputa_emissao.py:1554`).
- **Outros lotes:** `reabrir_para_correcao` escreve o dicionário fixo de resposta/validação (`app/chamados/services/disputa_emissao.py:1517`, `app/chamados/services/disputa_emissao.py:1556`); `bulk_update` escreve somente a lavra (`app/chamados/veredito_lavrado.py:217`, `app/chamados/veredito_lavrado.py:230`).
- **Migração:** o reapontamento salva destino/lavra, eventualmente o dia (`app/chamados/services/fio.py:866`, `app/chamados/services/fio.py:879`, `app/chamados/services/fio.py:922`, `app/chamados/services/fio.py:924`); a retratação salva carimbo, resposta/validação e lavra (`app/chamados/models.py:3128`, `app/chamados/models.py:3133`, `app/chamados/models.py:3137`, `app/chamados/models.py:3151`, `app/chamados/models.py:3152`).
- **`setattr` do texto do Chamado:** restrito a `titulo`, `assunto`, `descricao`, com recusa dos demais campos (`app/chamados/services/texto.py:18`, `app/chamados/services/texto.py:25`, `app/chamados/services/texto.py:27`, `app/chamados/services/texto.py:33`).
- **`setattr` genérico de reexibição:** só altera memória, sem salvar (`app/core/reexibir.py:21`, `app/core/reexibir.py:45`, `app/core/reexibir.py:50`); seus chamadores são de colaboradores (`app/colaboradores/views.py:1094`, `app/colaboradores/views.py:1259`).
- **Admin:** `app/chamados/admin.py` está vazio; a ausência também é documentada (`app/core/contratos_estruturais.py:331`). Não encontrei registro desses modelos nos outros admins nem ModelForm/ModelSerializer que os edite **[nao verificado: ausência universal]**. O `ready()` do app só importa signals (`app/chamados/apps.py:8`, `app/chamados/apps.py:10`).

Rodei o censo AST puro em memória, sem Django nem gravação de arquivo. Ele confirma os sítios acima, mas não fornece prova formal de ausência: o próprio censo admite operações não atribuíveis e inferências (`app/core/censo_escritas.py:38`, `app/core/censo_escritas.py:43`, `app/core/censo_escritas.py:49`).

## 09/11 00:17 · sessao 579d17b8fa80 · - · - · origem=Ronald (tmux)

Análise estática; execução e sobrevivência dinâmica dos mutantes: [nao verificado].

- **Curas efetivas:** a segunda leitura ocorre também no ramo com recusa; o helper exige exatamente duas chamadas após capturar a exceção (`app/chamados/tests/test_e1_fila.py:474`, `app/chamados/tests/test_e1_fila.py:489`, `app/chamados/tests/test_e1_fila.py:491`).
- **None:** retirar `- {None}` de `fio.py:2093` volta a recusar a troca Disputa→None e é detectado pelo teste novo (`app/chamados/tests/test_e1_fila.py:526`).
- **XOR:** substituir a diferença por diferença simétrica recusa o conjunto vazio e é detectado (`app/chamados/tests/test_e1_fila.py:521`; cálculo em `app/chamados/services/fio.py:2092`).
- **Cardinalidade da Disputa:** comparar somente tamanhos perde a substituição mantendo uma linha; o novo caso altera a disputa da pergunta existente e exige recusa (`app/chamados/tests/test_e1_fila.py:509`, `app/chamados/tests/test_e1_fila.py:517`).
- **Migração efetiva:** agora exige simultaneamente `migradas=[p.pk]`, demais saídas vazias e FK gravado no destino; remover o laço deixa o FK antigo e a lista vazia (`app/chamados/tests/test_e1_fila.py:539`, `app/chamados/tests/test_e1_fila.py:551`, `app/chamados/tests/test_e1_fila.py:555`).
- **MORDE:** recompila a função real com uma substituição cujo trecho deve aparecer exatamente uma vez; não usa uma função fictícia (`app/chamados/tests/test_e1_fila.py:48`; extração em `app/chamados/tests/test_e1_v15e_contrato_de_saida.py:331`).

Lacunas e mutantes sobreviventes por inspeção:

1. **Mensagem parcialmente coberta.** A única exigência sobre seu conteúdo é a substring `disputas [1000000000]`; os casos exclusivos de Colaborador e Pergunta só exigem mensagem verdadeira. Trocar `sorted(_fora[0])` ou `sorted(_fora[1])` por `[]`, trocar os IDs velho/novo do prefixo, ou ordenar a disputa em ordem inversa conserva os asserts atuais (`app/chamados/tests/test_e1_fila.py:494`, `app/chamados/tests/test_e1_fila.py:507`, `app/chamados/tests/test_e1_fila.py:519`; mensagem em `app/chamados/services/fio.py:2096`).

2. **Mensagem com conjunto maior também escapa.** Substituir o último argumento por `sorted({x.disputa_id for x in _sob} - {None})` imprime todas as disputas presentes, inclusive as já travadas. No único teste que verifica texto, a única disputa foi substituída, portanto o conjunto maior coincide com o esperado; o caso de acréscimo não examina texto (`app/chamados/tests/test_e1_fila.py:502`, `app/chamados/tests/test_e1_fila.py:509`, `app/chamados/tests/test_e1_fila.py:519`; argumento em `app/chamados/services/fio.py:2098`).

3. **Cardinalidade de Pergunta continua sem substituição equivalente.** Trocar a condição `_fora[1]` por `len({x.pk for x in _sob}) > len({x.pk for x in _ps})` conserva os casos presentes: eles acrescentam PK ou esvaziam o conjunto; a troca conservando cardinalidade cobre somente Disputa (`app/chamados/tests/test_e1_fila.py:498`, `app/chamados/tests/test_e1_fila.py:509`, `app/chamados/tests/test_e1_fila.py:523`; guarda em `app/chamados/services/fio.py:2094`).

4. **Colaborador requer cuidado.** O mutante `len(C_sob)>len(_cs)` é detectado: `_cs` já contém os dois donos, e acrescentar o colaborador sintético faz 2 contra 2. Já comparar cardinalidades dos colaboradores das duas leituras de Pergunta, `len(C_sob)>len(C_ps)`, conserva os casos atuais porque não existe substituição exclusiva de colaborador mantendo cardinalidade (`app/chamados/tests/test_e1_fila.py:147`, `app/chamados/tests/test_e1_fila.py:500`; formação de `_cs` em `app/chamados/services/fio.py:2085`).

5. **Precedência da conferência não é exigida.** Mover `_migrar_sob_trava` para imediatamente antes do `if _fora` conserva as verificações atuais: as corridas usam a pergunta de outro colaborador, recusada pela porta antes de gravar, e depois a exceção continua ocorrendo. O teste “nada migra” verifica apenas o destino final desse cenário já recusável; o cenário que efetivamente migra é exercitado sem corrida (`app/chamados/tests/test_e1_fila.py:147`, `app/chamados/tests/test_e1_fila.py:547`, `app/chamados/tests/test_e1_fila.py:563`; recusa anterior à escrita em `app/chamados/services/fio.py:863`; ordem atual em `app/chamados/services/fio.py:2094`).

6. **Corrida simulada, sem escritor concorrente.** O helper modifica somente o retorno da segunda chamada; o laço consulta novamente o banco real. Portanto esses testes demonstram a condição de recusa, mas não demonstram concorrência real, aquisição das travas ou que o mesmo universo continue estável depois da conferência (`app/chamados/tests/test_e1_fila.py:474`, `app/chamados/tests/test_e1_fila.py:486`; releituras em `app/chamados/services/fio.py:2118`, `app/chamados/services/fio.py:2122`).

## 09/11 00:16 · sessao 13dc7743181f · - · - · origem=Ronald (tmux)

Para a pergunta 5, convém separar “violação demonstrada” de “ausência universal”: o escopo inclui escritores e leitores dos campos, mesmo fora do diff, conforme CLAUDE.md:376-377.

| Classe | Resultado estático no caminho auditado |
|---|---|
| 2º juiz duplicando registrado | **Não demonstrado.** A nova guarda verifica cobertura de travas, explicitamente operacional em `app/chamados/services/fio.py:2026-2029`; vitalidade vem de `pergunta_viva` (`:2113-2124`), registrado em `app/core/juizes.py:266`; identidade/canônica continuam em `universo_da_linha`/`canonica_do_grupo` (`fio.py:2142-2149`). |
| 2º escritor fora da porta | **Não introduzido pelas linhas produtivas do v5.** A mudança está em diferença de conjuntos/mensagem (`fio.py:2092-2098`); escrita continua via `reapontar_resolvedora` e `retratar_para_canonica` (`:2165-2171`). A primeira grava destino e lavra em `:922-925`; a segunda delega à retratação em `:1327-1332`, que usa `carimbar_identidade` em `:1350-1353`. Ausência em toda a árvore depende do censo do agente escritores [nao verificado]. |
| Relógio fora da borda | **Não nas linhas v5** (`fio.py:2092-2098`). No escopo histórico FILA, `_hoje` usa `timezone.localdate(agora or timezone.now())` em `app/chamados/veredito_lavrado.py:95-102`; `expira_em` usa `_hoje(agora)` em `:137`; testes distinguem relógio da borda/date.today e duas leituras em `app/chamados/tests/test_e1_fila.py:349-385`. Execução [nao verificado]. |
| Derivação paralela de fonte única | **Não demonstrada na conferência.** Leituras pré/sob trava usam a mesma `_conjunto_da_migracao`, que chama `universo_da_linha` (`fio.py:2032-2039`, `:2084-2088`); o laço chama a mesma função (`:2142`). `universo_da_linha` e `universo_do_grupo` usam `fecho_do_fato` (`:1896-1911`). |
| Selo/RED sem mordida | **Há limite residual demonstrável.** `test_RED_a_porta_recusa_e_nada_migra` conserva `_cenario` cruzado e só observa destino velho (`test_e1_fila.py:563-571`); `_cenario` põe Pergunta do primeiro colaborador em chamados do segundo (`:147-155`), e `reapontar_resolvedora` já recusaria a costura cruzada (`fio.py:862-865`). Portanto esse teste não demonstra rollback de uma migração que ocorreria. O novo sucesso real exige destino novo e saída exata (`test_e1_fila.py:539-553`), curando o antigo “algum resultado”, mas não transformando aquele teste de rollback em cenário migrável. | [nao verificado]
| Ordem de trava | **Não há inversão nova demonstrada.** Uma chamada passa todos C/D/Ch/P (`fio.py:2084-2087`); implementação adquire C, D, Ch, P com ordenação por PK (`app/chamados/models.py:2673-2701`); conferência recusa antes do laço (`fio.py:2092-2099`). Travas internas ficam cobertas se os conjuntos conferidos correspondem às linhas que efetivamente serão usadas; mutabilidade/callers exigem resultado do agente escritores [nao verificado]. |
| Pouso antes do pré-requisito | **[nao verificado].** Regra exige suite verde antes do opus e proíbe Codex aprovar pouso sozinho (`CLAUDE.md:374-378`). Cabeçalho declara aval e marco histórico (`test_e1_fila.py:1-10`), mas isso não prova execução nem sequência de pouso [nao verificado]. | [nao verificado]
| Campo com dois significados | **Não introduzido na mudança v5.** `disputa_id` permanece identificador de Disputa; `None` representa ausência de linha travável, normalizada na guarda (`fio.py:2090-2093`) e em `_pks` (`models.py:2660-2661`). O risco de divergência entre dono direto e dono da Disputa é pré-existente: igualdade só é aplicada no nascimento (`models.py:3420-3435`); alcance por escritor produtivo depende do agente escritores [nao verificado]. | [nao verificado]

Triagem: a retirada de `None` da parcela de Disputa está alinhada à trava, pois `_pks` ignora `None` (`models.py:2660-2661`) e a conferência agora também (`fio.py:2093`). A dispensa de `key=str` é consistente com os IDs inteiros presentes no modelo/caminho, mas valores reais de produção não constam da sonda [nao verificado]. A triagem não traz item denominado `DESCARTE`; traz dois itens `FILA` [nao verificado: texto do anexo sem arquivo:linha].

Observação para consolidar: não classificar genericamente como “fora desta obra” apenas por não estar no diff, pois `CLAUDE.md:376-377` inclui explicitamente escritores e leitores. A relevância concreta dos dois FILA depende de escritor produtivo alcançável, que esta subtarefa não censou [nao verificado].

## 08/11 23:57 · sessao 667030d3457f · FIO-E0-V4-E1-FILA · verificador · origem=Code

**A volta 4 corrige a omissão de Disputa na conferência, mas b2 ficou parcialmente curado: o teste novo exige resultado preenchido, sem exigir migração efetiva. A equivalência literal entre IDs comparados e IDs traváveis também falha para `None`.** (`app/chamados/services/fio.py:2091`; `app/chamados/tests/test_e1_fila.py:511`; `app/chamados/models.py:2660`)

Execução da suíte, concorrência real e incidência em produção: **[nao verificado]**. [nao verificado]

1. **Triagem: descartes, filas e curas**

| Item | Verificação |
|---|---|
| **a — FILA dos chamadores que ignoram `False`** | **Procedente como defeito pendente.** Adesão incrementa sempre; reconciliador devolve `None` após ignorar a recusa e seu chamador conta `CREATE`; ação administrativa ignora o retorno e anuncia sucesso; view atribui qualquer recusa a irmão vivo. (`app/chamados/services/adesao.py:92`; `app/ponto/management/commands/reconciliar_grade.py:529`; `app/ponto/management/commands/reconciliar_grade.py:254`; `app/chamados/services/acoes_disputa.py:267`; `app/chamados/services/acoes_disputa.py:296`; `app/chamados/views_arquivar.py:70`) A justificativa histórica “anteriores à obra”: **[nao verificado]**. |
| **b1 — `or` → `and`** | **Curado no alvo.** Há crescimento isolado de Pergunta, Colaborador e Disputa, além do mutante explícito. (`app/chamados/tests/test_e1_fila.py:489`; `app/chamados/tests/test_e1_fila.py:504`) |
| **b2 — supressão do laço** | **Cura parcial.** O par novo distingue a supressão literal, mas aceita só `recusadas`. Seu cenário põe uma Pergunta do primeiro colaborador em chamados do segundo; o reapontamento recusa esse cruzamento antes da escrita. Logo “o laço produziu resultado” foi coberto; “migração efetiva sob trava” não foi exigida pelo novo RED. (`app/chamados/tests/test_e1_fila.py:147`; `app/chamados/tests/test_e1_fila.py:511`; `app/chamados/tests/test_e1_fila.py:517`; `app/chamados/services/fio.py:863`) |
| **b3 — captura ampliada para `RuntimeError`** | **Curado.** O teste exige propagação da exceção genérica; a captura produtiva continua específica. (`app/chamados/tests/test_e1_fila.py:566`; `app/chamados/models.py:1417`) |
| **b4 — modelo/objeto do evento** | **Curado.** Agora exige exatamente `('ChamadoColaborador', velho.pk)`. (`app/chamados/tests/test_e1_fila.py:564`) |
| **b5 — DESCARTE do retorno de `aplicar_declaracao`** | **Procedente para a promessa de trava.** O RED exige canônica, destino e ordem; a proposta é selecionada antes da chamada de trava. Isso não certifica sucesso funcional completo da aplicação. (`app/chamados/tests/test_e1_fila.py:442`; `app/chamados/services/declaracao_texto.py:643`) |
| **b6 — trava para só um motivo** | **DESCARTE procedente na implementação atual.** O helper recebe motivo/dia e consulta o universo correspondente, sem restringir motivo; ambas as portas o chamam após selecionar o fato. (`app/chamados/services/declaracao_texto.py:282`; `app/chamados/services/declaracao_texto.py:346`; `app/chamados/services/declaracao_texto.py:650`) |
| **c — mudança de `disputa_id`** | **Curado para entrada de ID novo não nulo.** A diferença de disputas participa da recusa e ganhou caso isolado; resta a assimetria de `None` descrita abaixo. (`app/chamados/services/fio.py:2091`; `app/chamados/tests/test_e1_fila.py:497`) |
| **d — relógio e derivação da janela** | **FILA tecnicamente procedente.** O juiz ainda usa `date.today()`, enquanto `expira_em` reproduz a condição da janela em implementação própria. (`app/chamados/juizes.py:575`; `app/chamados/juizes.py:601`; `app/chamados/veredito_lavrado.py:120`; `app/chamados/veredito_lavrado.py:136`) O registro prévio na fila citada: **[nao verificado]**. |

2. **Quem escreve, quem lê e o universo conferido**

| Campo/valor | Escritor comprovado | Leitor relevante |
|---|---|---|
| Pergunta `pk` | `BigAutoField`; nascimento insere com `save(force_insert=True)`. (`app/chamados/migrations/0014_perguntadisputa.py:19`; `app/chamados/models.py:3019`) | Lista pedida à trava e diferença de Perguntas. (`app/chamados/services/fio.py:2087`; `app/chamados/services/fio.py:2091`) |
| Pergunta `colaborador_id` | `_chave_de_identidade` deriva da Disputa e rejeita valor divergente; migration preenche o acervo. (`app/chamados/models.py:3430`; `app/chamados/migrations/0051_decisao_pergunta.py:25`) | `_cs`, conferência e consulta da identidade do fato. (`app/chamados/services/fio.py:2085`; `app/chamados/services/fio.py:2091`; `app/chamados/models.py:2762`) |
| Pergunta `disputa_id` | Nascimento recebe `disputa` e insere a Pergunta. (`app/chamados/models.py:3016`) | Pedido à trava, diferença nova e trava interna da retratação. (`app/chamados/services/fio.py:2086`; `app/chamados/services/fio.py:2092`; `app/chamados/services/fio.py:1329`) |
| Donos de `velho`/`novo` | A migração recebe as instâncias e lê seus `colaborador_id`; não os escreve. (`app/chamados/services/fio.py:2042`; `app/chamados/services/fio.py:2085`) | Entram em `_cs`, mesmo sem Pergunta daquele dono. (`app/chamados/services/fio.py:2085`) |
| `_conjunto_da_migracao` | É lista calculada: Perguntas destinadas a velho/novo, somadas ao universo de cada uma; pode conter repetição. (`app/chamados/services/fio.py:2037`) | Duas leituras: cálculo inicial e conferência após a trava. (`app/chamados/services/fio.py:2084`; `app/chamados/services/fio.py:2088`) |

**Quem altera a pertinência ao conjunto:** nascimento e reapontamento/reuso escrevem `chamado_destino`; rechaveamento altera dia/célula; carimbo altera a via que a consulta do fato exclui. A expansão delega a `universo_da_linha`/`fecho_do_fato`. (`app/chamados/models.py:3016`; `app/chamados/models.py:2971`; `app/chamados/services/fio.py:922`; `app/chamados/services/fio.py:1028`; `app/chamados/models.py:3182`; `app/chamados/models.py:2732`; `app/chamados/services/fio.py:1906`)

**O pedido desta chamada é exatamente:**

| Classe | IDs solicitados |
|---|---|
| Colaborador | Donos de velho/novo ∪ colaboradores das `_ps`. (`app/chamados/services/fio.py:2085`) |
| Disputa | `disputa_id` das `_ps`. (`app/chamados/services/fio.py:2086`) |
| Chamado | Somente `velho.pk` e `novo.pk`. (`app/chamados/services/fio.py:2086`) |
| Pergunta | `pk` das `_ps`. (`app/chamados/services/fio.py:2087`) |

`trava_na_ordem` elimina repetidos e `None`, consulta linhas existentes e executa **C → D → Ch → P**, cada classe por PK crescente. Nesta chamada, os pais das Disputas não são acrescentados automaticamente: falta a flag `abertas_do_colaborador`. (`app/chamados/models.py:2660`; `app/chamados/models.py:2674`; `app/chamados/models.py:2683`; `app/chamados/models.py:2687`; `app/chamados/models.py:2696`; `app/chamados/models.py:2700`; `app/chamados/services/fio.py:2086`)

**Para IDs existentes e não nulos, a conferência cobre corretamente inclusão por classe.** Encolhimento e troca de associações entre IDs já travados podem passar: não exigem outra linha. Exigir igualdade dos conjuntos recusaria desnecessariamente esses casos. (`app/chamados/services/fio.py:2091`)

**Não há uma quarta coleção dinâmica de PKs de Chamado omitida:** os dois PKs vêm dos argumentos e permanecem fixos. Entretanto, seus donos são lidos das instâncias recebidas e não são relidos sob trava; uma troca persistida de dono não seria conferida aqui. Escritor produtivo dessa troca e sua alcançabilidade: **[nao verificado]**. (`app/chamados/services/fio.py:2085`; `app/chamados/services/fio.py:2088`)

A imutabilidade de `Pergunta.colaborador_id`/`disputa_id` após nascimento também não é garantida pelo `save`: a validação da identidade só roda quando `adding`; depois ele delega ao ORM. Escrita indireta que explore isso: **[nao verificado]**. (`app/chamados/models.py:3420`)

3. **Mordida e mutantes sobreviventes**

Os casos isolados mordem a ausência de cada parcela da guarda, mas simulam mudança **acrescentando** um `SimpleNamespace`; nos casos de colaborador/disputa, acrescentam outro objeto com PK já existente, em vez de substituir a versão anterior. Não demonstram corrida entre transações. (`app/chamados/tests/test_e1_fila.py:473`; `app/chamados/tests/test_e1_fila.py:495`)

O helper `_recusa_com` retorna imediatamente na exceção: sua asserção de duas leituras só executa quando não houve recusa. O teste separado de recusa original, porém, exige as duas leituras. (`app/chamados/tests/test_e1_fila.py:484`; `app/chamados/tests/test_e1_fila.py:529`)

Os seguintes mutantes não são distinguidos pelos formatos dos novos casos; sobrevivência na **suíte inteira**: **[nao verificado]**. [nao verificado]

| Mutante | Falha que escapa |
|---|---|
| Diferença de disputas `A - B` → `A ^ B` | Recusa também encolhimento; os casos acrescentam uma linha e não exercitam remoção. (`app/chamados/services/fio.py:2092`; `app/chamados/tests/test_e1_fila.py:476`) |
| Parcela de disputas → `{len(_sob)} - {len(_ps)}` | Passa no crescimento artificial, mas perde troca real de disputa mantendo o número de Perguntas. (`app/chamados/services/fio.py:2092`; `app/chamados/tests/test_e1_fila.py:473`) |
| Remover `key=str` | A mensagem pode explodir com diferença contendo simultaneamente `None` e inteiro; nenhum caso novo produz essa diferença mista. (`app/chamados/services/fio.py:2097`; `app/chamados/tests/test_e1_fila.py:469`; `app/chamados/tests/test_e1_fila.py:492`) |
| Imprimir a parcela errada na mensagem | Os testes conferem ocorrência da exceção, sem exigir os IDs de disputas no texto. (`app/chamados/services/fio.py:2097`; `app/chamados/tests/test_e1_fila.py:502`; `app/chamados/tests/test_e1_fila.py:527`) |

A fragilidade de b2 é dos testes novos: já existe teste antigo que exige `migradas`, relê a Pergunta e verifica destino persistido. (`app/chamados/tests/test_e1_v15g_portas.py:364`)

4. **`disputa_id=None`**

**`None` nunca é travado como Disputa.** `_pks` o remove; com `disputas=[None]`, sem universo adicional, nem há consulta de Disputa. A diferença nova, entretanto, conserva `None` como elemento comparável. (`app/chamados/models.py:2660`; `app/chamados/models.py:2682`; `app/chamados/services/fio.py:2092`)

| Disputas antes → depois | Resultado da guarda |
|---|---|
| `{7}` → `{None}` | Recusa por `{None}`, embora nenhuma Disputa nova precise de trava. (`app/chamados/services/fio.py:2092`; `app/chamados/models.py:2661`) |
| `{7, None}` → `{None}` | Aceita: `None` já estava no conjunto inicial. (`app/chamados/services/fio.py:2092`) |
| `{None}` → `{8}` | Recusa corretamente por `{8}`. (`app/chamados/services/fio.py:2092`) |
| `{7, 8}` → `{8}` | Aceita corretamente o encolhimento. (`app/chamados/services/fio.py:2092`) |

Portanto, **comparado e travável não são literalmente equivalentes**. Contudo, `Pergunta.disputa` é FK obrigatória tanto no modelo quanto na migration de criação. `None` persistido no esquema real de produção: **[nao verificado]**; nos testes, ele é explicitamente sintético. (`app/chamados/models.py:2424`; `app/chamados/migrations/0014_perguntadisputa.py:27`; `app/chamados/tests/test_e1_fila.py:469`)

5. **Classes de reprovação**

| Classe | Resposta com escopo |
|---|---|
| **Segundo juiz duplicando registrado** | **Não nas linhas novas:** conferem cobertura de IDs; o laço continua consultando `pergunta_viva`, autoridade C6 registrada. (`app/chamados/services/fio.py:2091`; `app/chamados/services/fio.py:2123`; `app/core/juizes.py:266`) |
| **Segundo escritor fora da porta** | **Não acrescentado pela V4:** a escrita permanece depois da conferência, pelo laço existente. Ausência global de qualquer escritor indireto: **[nao verificado]**. (`app/chamados/services/fio.py:2098`) |
| **Relógio fora da borda** | **Sim no contexto**, no `date.today()` do juiz; a lavra usa `_hoje` com `timezone.now`. (`app/chamados/juizes.py:575`; `app/chamados/veredito_lavrado.py:102`) |
| **Derivação paralela de fonte única** | **Sim no contexto:** `expira_em` reproduz a condição da janela do juiz. (`app/chamados/veredito_lavrado.py:120`; `app/chamados/veredito_lavrado.py:141`; `app/chamados/juizes.py:601`) |
| **Selo/RED sem mordida** | **Parcial em b2:** morde supressão literal, mas não exige migração efetiva. (`app/chamados/tests/test_e1_fila.py:511`; `app/chamados/tests/test_e1_fila.py:517`) |
| **Ordem de trava** | **Não há inversão acrescentada pela V4:** mantém pedido único no topo e recusa antes do laço quando falta ID. Ordem global sob concorrência real: **[nao verificado]**. (`app/chamados/services/fio.py:2086`; `app/chamados/services/fio.py:2093`) |
| **Pouso antes do pré-requisito** | **Não no miolo novo:** conferência precede laço; aprovação do destino precede retratação. Pouso/deploy e cumprimento dos pré-requisitos externos: **[nao verificado]**. (`app/chamados/services/fio.py:2098`; `app/chamados/services/fio.py:2164`; `app/chamados/services/fio.py:2170`) |
| **Campo com dois significados** | **Sim, declarado no contexto:** `data_evento` representa dia do turno para fato e dia civil para conversa; há discriminador e tratamento explícitos. A V4 não altera esse campo. (`app/chamados/models.py:2820`; `app/chamados/juizes.py:2006`; `app/chamados/services/fio.py:2091`) |

## 08/11 23:57 · sessao e1c9c1085810 · BUG-DEPLOY-PORTAO-ANTES-DO-MIGRATE · verificador · origem=Code

**REPROVA para o contrato absoluto informado.** A volta 4 restaura a recusa exigida para cron vivo, mas conserva um portão depois do migrate; o caso 11 agora exige justamente banco alterado e restart recusado (`bin/deploy.sh:301,326`; `bin/tests/test_deploy_portoes_antes_do_migrate.sh:163–165`; `CLAUDE.md:90`). Autorização para excepcionar esse caminho do contrato: [nao verificado].

A análise abaixo é estática. Execução dos 13 casos, RED dos mutantes e ocorrência desses caminhos em produção: [nao verificado].

**1. Triagem da volta 3**

| Item | Veredito nesta obra |
|---|---|
| **PROCEDENTE 1** | Curou a violação da lei do restart: a recusa voltou a ser incondicional. Não curou simultaneamente o contrato de portões antes do migrate (`bin/deploy.sh:294–301,323–327`; `CLAUDE.md:90`). |
| **PROCEDENTE 2** | “UM leitor” agora corresponde ao único teste do valor de `APLICAR_MIGRATE`. “UM escritor” exige distinguir a inicialização da armação; literalmente há duas atribuições (`bin/deploy.sh:59–61,223,299`). |
| **PROCEDENTE 3** | Curado estaticamente: `PUB_OK` compara apply com a primeira publicação e é exigido nos casos 3 e 13 (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:110–114,132,172`). RED efetivo: [nao verificado]. |
| **PROCEDENTE 4** | **Mal curado:** o caso 12 cria cron antes de iniciar o deploy; a guarda inicial já recusa. Remover somente a rechecagem imediatamente anterior ao apply continua passando esse cenário (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:97–98,167–169`; `bin/deploy.sh:115–118,300`). |
| **PROCEDENTE 5** | A dependência de PID fixo foi retirada: as marcas usam `$$` do selo, exportado para o falso migrate (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:75,96–97`). |
| **NÃO PROCEDE 6** | Defensável **na localização declarada**: a configuração agenda deploy e envelope sob `/home/ronald/saas-hasner`. Não é equivalência universal: o envelope escreve em endereço absoluto e o deploy lê relativamente à própria localização (`app/config/crons.py:19,326,1475,1486`; `bin/cron_run.sh:9`; `bin/deploy.sh:53,108`). Caminhos efetivos em produção: [nao verificado]. |
| **FILA 7** | **Pertence à promessa desta obra:** a 1c afirma que mensageria já respondeu. Hoje sua guarda precede o apply, mas o falso deste selo sempre responde sucesso para ela; o vizinho testa recusa por mensageria, sem exigir banco core intocado (`bin/deploy.sh:194–203,292,301`; `bin/tests/test_deploy_portoes_antes_do_migrate.sh:76`; `bin/tests/test_deploy_reload_gracioso.sh:150–156`). |
| **FILA 8** | **Pertence à promessa:** faltam cenários verdes de auth com pendência em agendado/`--sem-migrate`. O caso 4 para antes, por auth; o caso 10 não tem pendência. Remover as recusas desses modos sobreviveria aos 13 por inspeção (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:135–137,159–161`; `bin/deploy.sh:211–218`). |
| **FILA 9** | Lacuna de cobertura, sem defeito atual demonstrado: `--reload-copia` passa pela mesma chamada de auth, mas não aparece nos 13 cenários (`bin/deploy.sh:82,126,145`; `bin/tests/test_deploy_portoes_antes_do_migrate.sh:123–173`). |
| **DESCARTE 10** | **Improcedente como descarte.** Consultar novamente pode obter outra resposta e encerrar o deploy após o apply. Além disso, a consulta também apaga marcas consideradas mortas (`bin/deploy.sh:92,108–113,301,326`). |
| **FILA 11** | O contador realmente não prova os alvos, mas o vizinho exige a linha exata do restart das três cascas. Não é bloqueio adicional demonstrado aqui (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:109,172`; `bin/tests/test_deploy_reload_gracioso.sh:134–139`). |
| **DESCARTE 12** | **Justificativa exagerada:** o vizinho verifica troca de workers e sobrevivência do exec, mas simula reciclagem verde, rotas 200 e sucesso dos demais exec. Não demonstra mordida de falhas de readiness/rotas/reciclagem (`bin/tests/test_deploy_reload_gracioso.sh:40–41,57–58,107–112`). Provas de resultado da publicação continuam distintas de pré-requisitos verificáveis antes dela (`bin/deploy.sh:344–357,365–403`). |
| **DESCARTE 13** | **Procede para reabertura sob o contrato literal.** Reciclagem vermelha encerra o deploy depois de migrate/publicação; verifica inclusive configuração e crontab, condições que podem já estar erradas antes do apply (`bin/deploy.sh:301,327,344,410`; `bin/selo_reciclagem.sh:45–48,53–78`). |
| **PERGUNTA 14** | Continua material: as consultas não estabelecem exclusão com o nascimento de cron. Restaurar a última consulta preserva a recusa observada, mas não fecha a corrida (`bin/deploy.sh:108–117,300,326–327`; `bin/cron_run.sh:9–11`). |

**2. Escritores e leitores**

| Campo/protocolo | Quem escreve | Quem lê |
|---|---|---|
| `APLICAR_MIGRATE` | Deploy inicializa `0` e arma `1` quando há pendência core em modo elegível (`bin/deploy.sh:61,204–223`). O selo injeta `1` herdado no caso 10, sobrescrito pela inicialização (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:98,159`). | Único leitor operacional: decisão de executar apply (`bin/deploy.sh:299`). |
| `RESTART` | Deploy inicializa `0`; somente `MODO=--restart` escreve `1`, exigindo motivo (`bin/deploy.sh:70–75`). | Guarda inicial; exceção da pendência de mensageria; rechecagem pré-apply; escolha restart/HUP; mensagem final (`bin/deploy.sh:115,200,300,323,429`). Fora dele, o contrato lê o **texto** do ramo (`bin/contrato_deploy.sh:41–47`). |
| Marcas `logs/crons_em_curso` | Envelope cria diretório/nome `N_E.$$`, grava timestamp e remove no `EXIT` (`bin/cron_run.sh:9–10`). **Deploy também modifica o conjunto**, apagando marcas sem PID existente (`bin/deploy.sh:108–109`). | Deploy enumera marcas, extrai PID do sufixo, consulta `/proc`, devolve nomes vivos e decide a recusa (`bin/deploy.sh:108–113`). A mesma função é chamada em três pontos (`bin/deploy.sh:117,300,326`). |

O deploy **não lê o timestamp gravado dentro da marca**: seu protocolo efetivo usa existência do arquivo, nome e PID do sufixo (`bin/cron_run.sh:10`; `bin/deploy.sh:108–109`). O contrato externo apenas procura os textos dos caminhos, sem consumir marcas (`bin/contrato_deploy.sh:81–82`).

Os escritores de marcas dos testes são o falso migrate e a preparação do cron anterior, além do selo vizinho com PID de seu exec falso (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:75,96–97`; `bin/tests/test_deploy_reload_gracioso.sh:90–91`). Os elos da corrente também entram pelo envelope, depois de esperar carimbos de fim (`bin/encadeado.sh:89,102–104`).

Portanto, o comentário sobre `APLICAR_MIGRATE` bate quanto ao **leitor único** e ao **único ponto que arma a decisão**; não descreve literalmente uma única atribuição. A consulta das marcas tampouco é somente leitura (`bin/deploy.sh:59–61,109,223,299`).

**3. Mordida dos casos 1–13**

A tabela indica mutantes detectáveis pelas asserções, por inspeção; execução desses mutantes: [nao verificado].

| Caso | Mutante que detecta |
|---|---|
| **1** | Auth removida, ignorada ou posterior ao apply: exige recusa sem migrate/HUP (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:123–125`). |
| **2** | Sombra removida, ignorada ou posterior ao apply (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:127–129`). |
| **3** | Apply ausente/duplicado; auth, sombra `--conferir` ou casca ausentes/depois dele; publicação antes do apply; HUP ausente/duplicado (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:103–117,131–133`). |
| **4** | Agendado deixa de **chamar** auth. Não isola respeito ao retorno: a pendência também poderia recusar (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:135–137`). |
| **5** | `--conferir` aplica/publica, consulta auth/sombra ou deixa de terminar com sucesso (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:139–141`). |
| **6** | Prova de casca removida, ignorada ou posterior ao apply (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:143–145`). |
| **7** | `--sem-sombra` também dispensa auth ou migra antes dela (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:147–149`). |
| **8** | Apply incondicional sem pendência, ou ausência de HUP nesse caminho (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:151–153`). |
| **9** | Agendado chama auth, mas ignora a recusa; sem pendência para mascarar o erro (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:155–157`). |
| **10** | Retirada da inicialização permite `APLICAR_MIGRATE=1` herdado aplicar no `--sem-migrate` (`bin/deploy.sh:61`; `bin/tests/test_deploy_portoes_antes_do_migrate.sh:98,159–161`). |
| **11** | Retirada da recusa final ou retorno à condição da volta 3, permitindo restart sobre cron nascido no apply (`bin/deploy.sh:326`; `bin/tests/test_deploy_portoes_antes_do_migrate.sh:75,163–165`). |
| **12** | Retirada das barreiras anteriores ao apply para cron **já vivo no início**. Não distingue qual delas respondeu (`bin/deploy.sh:117,300`; `bin/tests/test_deploy_portoes_antes_do_migrate.sh:97,167–169`). |
| **13** | Apply ausente/duplicado; restart ausente/duplicado; restart antes do apply (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:110–114,171–173`). |

**Sobreviventes relevantes:**

- **Na linha efetivamente alterada `bin/deploy.sh:326`:** substituir a chamada por `[ "$APLICAR_MIGRATE" != 1 ] || recusa_restart_com_cron`. Preserva a recusa com apply, satisfazendo 11/13; o 12 para na guarda inicial. Deixa desprotegido cron tardio em restart **sem pendência core**, cenário inexistente nos 13 (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:163,167,171`; `bin/deploy.sh:117,326–327`).
- **Na mesma linha alterada:** condicionar a recusa a `PEND_MSG=0`. Sobrevive porque o falso sempre devolve sucesso para mensageria; falha justamente quando há pendência dela e restart autorizado (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:76`; `bin/deploy.sh:194–203,326`).
- **Na proteção que o novo caso 12 promete selar:** remover somente `bin/deploy.sh:300`. A marca anterior já é barrada em `117`; a marca do caso 11 só nasce dentro do migrate. Falta nascimento entre essas guardas (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:75,97,167–169`; `bin/deploy.sh:117,300–301`).

Há também erro no comentário novo: anuncia comparação apply/publicação nos casos **3,8,11**, mas ela é exigida nos **3,13**. Pelas próprias definições, 8 não tem apply e 11 não tem publicação (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:36,110–114,132,152,164,172`).

**4. Comentário da seção 1c e caminhos adicionais**

A descrição acerta que auth, sombra aplicável, guardas de pendência e prova de casca ficam antes do apply; a prova de casca usa container sem rede (`bin/deploy.sh:145,151–162,194–218,286,301`; `bin/prova_de_casca.sh:30–34`). Acerta também que o envelope executa o comando uma vez, sem retry nesse fluxo, e que a corrente espera arquivos de conclusão (`bin/cron_run.sh:11,22–25,38`; `bin/encadeado.sh:89–96`).

**A janela residual não se limita à duração do migrate.** Este caminho é permitido pela sequência do código:

1. A consulta pré-apply termina sem marca viva (`bin/deploy.sh:300`; `bin/deploy.sh:108–111`).
2. O migrate termina com sucesso (`bin/deploy.sh:301`).
3. Um envelope cria sua marca **depois desse término** (`bin/cron_run.sh:9–11`).
4. A seção 2 consulta novamente, encontra o cron e recusa (`bin/deploy.sh:324–327`).

Logo, **sim**, há banco andado e recusa na seção 2 fora do residual literalmente descrito como cron que nasce “durante o migrate”. Também cabe nascimento depois da observação prévia e antes de começar o apply (`bin/deploy.sh:295–301,326`; `bin/cron_run.sh:9`).

Existe ainda a corrida inversa: cron nasce depois da última observação e antes do restart; a consulta não impede que esse cron seja atingido pelo restart (`bin/deploy.sh:326–327`; `bin/cron_run.sh:9–11`). Ocorrência real: [nao verificado].

O comentário promete pergunta no topo do RELATO, mas o topo disponível trata de outras obras; registro da pergunta prometida: [nao verificado] (`bin/deploy.sh:298`; `app/docs/RELATO.md:2–12`).

**5. Classes de reprovação**

| Classe | Sim/não e prova nesta versão |
|---|---|
| **Segundo juiz duplicando o existente** | **Não identificado na cura:** as três consultas chamam a mesma função; auth também permanece delegada ao mesmo script (`bin/deploy.sh:110–117,145,300,326`). |
| **Segundo escritor fora da porta** | **Sim para mutação das marcas:** além do envelope, o deploy apaga marcas. É limpeza já existente, não novo aplicador de migration/publicador (`bin/cron_run.sh:9–10`; `bin/deploy.sh:109,301,327,344`). |
| **Relógio fora da borda** | **Não introduzido pelas alterações mostradas:** a decisão temporal permanece em `janela_auth.sh`. Há limite preexistente: dia/hora são lidos separadamente e não são revalidados na publicação (`bin/janela_auth.sh:49–56`; `bin/deploy.sh:145,301,327,344`). Ocorrência de atravessar a janela: [nao verificado]. |
| **Derivação paralela de fonte única** | **Não para a regra de recusa:** há função única reutilizada. O endereço das marcas tem duas bases de construção, equivalentes apenas na instalação declarada (`bin/deploy.sh:53,108–117,300,326`; `bin/cron_run.sh:9`; `app/config/crons.py:19`). |
| **Selo/RED sem mordida** | **Sim, parcialmente:** caso 12 não protege isoladamente a rechecagem pré-apply; o comentário de cobertura também aponta casos errados (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:36,97,167–169`; `bin/deploy.sh:117,300`). RED executado: [nao verificado]. |
| **Ordem de trava** | **Sim quanto à falta de exclusão comum**, não há deadlock demonstrado: envelope cria marca diretamente e deploy amostra sem impedir novos nascimentos (`bin/cron_run.sh:9–11`; `bin/deploy.sh:108–113,300,326–327`). |
| **Pouso antes do pré-requisito** | **Sim para apply antes da decisão final de recusa**; não para a ordem atual apply→HUP/restart (`bin/deploy.sh:301,326–327,344`). O selo aceita expressamente o primeiro caminho (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:164`). |
| **Campo com dois significados** | **Não demonstrado em `APLICAR_MIGRATE`/`RESTART`:** o primeiro é decisão de aplicar, não comprovante de aplicação; o segundo é modo manual, não comprovante de restart concluído. Seus leitores atuais respeitam isso (`bin/deploy.sh:61,70–75,115,200,223,299–300,323,429`). As marcas compartilham o protocolo de nome/PID; o timestamp não participa da decisão (`bin/cron_run.sh:9–10`; `bin/deploy.sh:108–113`). |

O reload agendado **lê atualmente o mesmo portão de auth**, sem isenção por modo; a configuração chama esse mesmo deploy com `--reload-agendado` (`bin/deploy.sh:65,126–145`; `app/config/crons.py:326`). Instalação efetiva desse cron e resultado em produção: [nao verificado].

## 08/11 23:56 · sessao 08a5d54a032d · - · - · origem=Ronald (tmux)

**Conferência e trava.** Para IDs não nulos existentes, a condição é de inclusão correta: todo Colaborador/Pergunta/Disputa da segunda leitura precisa pertencer à respectiva classe solicitada na primeira. Trocas de associação entre linhas já cobertas e encolhimentos podem passar: não exigem adquirir outra linha. `_cs` inclui também os donos de `velho` e `novo`, portanto aceitar Pergunta cujo colaborador passou a ser um desses donos é correto: ele já foi solicitado à trava. (`app/chamados/services/fio.py:2084–2098`)

| Valor | Escritor comprovado | Leitor relevante |
|---|---|---|
| Pergunta `pk` | Definição `BigAutoField(primary_key=True)` na criação do modelo; inserção por `p.save(force_insert=True)`. (`app/chamados/migrations/0014_perguntadisputa.py:19`; `app/chamados/models.py:3016–3019`) | Pedido da trava e conferência. (`app/chamados/services/fio.py:2086–2092`) |
| Pergunta `disputa_id` | Nascimento por `p = cls(disputa=disputa, ...)`, seguido de inserção. (`app/chamados/models.py:3016–3019`) | Pedido da trava e diferença nova; retratação solicita novamente as disputas da canônica/perdedoras. (`app/chamados/services/fio.py:2086–2092`; `app/chamados/services/fio.py:1328–1330`) |
| Pergunta `colaborador_id` | `_chave_de_identidade` deriva do dono da disputa e recusa valor explícito divergente; migration preenche o acervo. (`app/chamados/models.py:3430–3435`; `app/chamados/migrations/0051_decisao_pergunta.py:25`) | Construção de `_cs`, conferência, identidade/dedup do fato. (`app/chamados/services/fio.py:2085–2091`; `app/chamados/models.py:2761–2763`) |
| `_conjunto_da_migracao` | Não é coluna: a função monta lista das Perguntas cujo destino é velho/novo e soma o universo de cada semente. (`app/chamados/services/fio.py:2032–2039`) | Leitura antes e depois da trava. (`app/chamados/services/fio.py:2084–2088`) |
| Destino que determina sementes | Nascimento e reapontamento/reuso. (`app/chamados/models.py:3016`; `app/chamados/services/fio.py:922–925`; `app/chamados/models.py:2971–3009`) | Filtro das sementes. (`app/chamados/services/fio.py:2037`) |
| Identidade que determina expansão | Dia/célula podem mudar no rechaveamento; via de resolução muda na porta do carimbo. (`app/chamados/services/fio.py:1028–1034`; `app/chamados/models.py:3182–3190`) | `universo_da_linha` delega ao fecho; o passo do fato exclui vias fora do fato. (`app/chamados/services/fio.py:1906–1911`; `app/chamados/models.py:2730–2736`) |

**O que efetivamente é solicitado à trava:** C = `_cs`; D = disputas das `_ps`; Ch = `{velho.pk, novo.pk}`; P = pks das `_ps`. A implementação elimina `None` e repetidos, executa cada classe por pk crescente e na ordem C→D→Ch→P. Os Chamados pais das disputas **não** entram automaticamente nesta chamada: isso depende de `abertas_do_colaborador`, ausente aqui. (`app/chamados/services/fio.py:2085–2087`; `app/chamados/models.py:2660–2702`)

**Chamado não conferido:** seu conjunto de pks é fixo pelos argumentos `velho`/`novo`; não depende de associação mutável das Perguntas. Logo não há quarta classe de pks dinâmica omitida nesta chamada. Contudo, os donos de `velho`/`novo` vêm das instâncias recebidas e não são relidos sob trava; `_sob` apenas relê Perguntas. Uma eventual troca persistida de dono dos endpoints não seria detectada por esta conferência. Escritor concreto dessa troca após nascimento não foi localizado; ocorrência e alcançabilidade: **[nao verificado]**. (`app/chamados/services/fio.py:2085–2092`)

O censo AST em memória de Python de produção não encontrou atribuição explícita de `Pergunta.disputa/disputa_id` após nascimento, nem atualização literal dessas chaves. Não transforma imutabilidade em garantia: o `save` genérico só chama `_chave_de_identidade` em inserções e depois delega ao ORM; persistência por escritor indireto/dinâmico continua **[nao verificado]**. (`app/chamados/models.py:3420–3423`)

**None quebra a equivalência literal entre conjunto comparado e conjunto travável.** `_pks` elimina `None`; `disputas=[None]` não executa consulta de Disputa. Entretanto, `_fora[2]` compara conjuntos crus. Portanto, se `None` aparece apenas na segunda leitura, ocorre recusa embora nenhuma nova linha de Disputa corresponda a esse valor. Se `None` já estava na primeira leitura, não há recusa por ele; qualquer ID real novo continua sendo recusado. (`app/chamados/models.py:2660–2661`; `app/chamados/models.py:2679–2684`; `app/chamados/services/fio.py:2091–2093`)

Exemplos deduzidos da expressão:

- Antes `{7}`, depois `{None}` → recusa por `{None}`, embora `None` não seja travado. (`app/chamados/services/fio.py:2092–2093`; `app/chamados/models.py:2661`)
- Antes `{None, 7}`, depois `{None, 8}` → recusa por `{8}`. (`app/chamados/services/fio.py:2092–2093`)
- Antes `{7, 8}`, depois `{8}` → aceita, corretamente para inclusão de linhas necessárias. (`app/chamados/services/fio.py:2092–2093`)

A FK `Pergunta.disputa` é obrigatória tanto no modelo atual quanto na migration de criação: não declara `null=True`. Assim, a diferença de semântica existe na função, mas a presença de `None` em Pergunta persistida pelo esquema esperado não foi demonstrada; estado real do schema/dados de produção: **[nao verificado]**. (`app/chamados/models.py:2424–2428`; `app/chamados/migrations/0014_perguntadisputa.py:27`)

## 08/11 23:55 · sessao 460631968e5b · - · - · origem=Ronald (tmux)

Leitura estática; nenhuma execução de teste. Resultado de produção e RED/GREEN efetivo: [nao verificado]. [nao verificado]

1. **Contrato absoluto ainda não está cumprido.** O apply ocorre em `bin/deploy.sh:301`, mas o portão de cron ainda pode recusar em `bin/deploy.sh:326`; o caso 11 passa a exigir exatamente `1 migrate / 0 restart / rc!=0`, aceitando esse residual no selo (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:163-165`). A lei local exige recusa do restart com cron vivo (`CLAUDE.md:90`); restaurá-la corrige a troca de lei da volta 3, mas não resolve simultaneamente a promessa “todo portão antes do migrate” (`bin/deploy.sh:294-298`).

2. **PROCEDENTE 4 mal curado: caso 12 não protege a recusa imediatamente anterior ao apply.** A marca é criada antes da execução do deploy (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:97`), portanto a primeira consulta já recusa em `bin/deploy.sh:115-117`. Apagar somente `bin/deploy.sh:300` continua atendendo ao caso 12 (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:167-169`); o caso 11 só cria a marca dentro do apply (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:75`). Falta cenário com marca surgindo entre a 0e e a 1c (`bin/deploy.sh:117`, `bin/deploy.sh:300`).

3. **DESCARTE 10 procede para esta obra.** “Só ler” não torna a segunda consulta inofensiva: `recusa_restart_com_cron` chama `falhou`, que termina com exit 1 (`bin/deploy.sh:92`, `bin/deploy.sh:110-113`), e a consulta após o apply está explicitamente presente (`bin/deploy.sh:301`, `bin/deploy.sh:326`).

4. **O comentário 1c estreita demais o residual.** A janela real vai da observação de marcas feita na chamada de `bin/deploy.sh:300` até a observação feita na chamada de `bin/deploy.sh:326`, ambas por `bin/deploy.sh:111`. Pode surgir marca **depois que migrate terminou**, antes da consulta da seção 2; a criação de marca não toma trava compartilhada (`bin/cron_run.sh:9-11`). Logo há caminho estático banco andado + seção 2 recusando fora da expressão literal “cron que NASCE durante o migrate” (`bin/deploy.sh:295-297`). Ocorrência em produção: [nao verificado].

5. **FILA 7 e 8 são dependências da promessa, mesmo preexistentes.** A 1c afirma que mensageria e pendências já responderam (`bin/deploy.sh:292`), e hoje de fato suas guardas estão antes do apply (`bin/deploy.sh:194-218`, `bin/deploy.sh:301`). Entretanto o falso nunca faz a mensageria recusar (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:76`); os cenários agendados com pendência têm auth vermelho e param antes dessa guarda (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:135-137`), e o `--sem-migrate` do caso 10 não tem pendência (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:159-161`). Remover as recusas de `bin/deploy.sh:211-218` sobrevive aos 13 por inspeção; execução do mutante: [nao verificado].

6. **FILA 9 é lacuna de modo, sem bug atual demonstrado.** A cópia altera os indicadores em `bin/deploy.sh:82`, mas passa pela mesma chamada de auth, sem ramo de isenção por modo (`bin/deploy.sh:126-145`). Os 13 cenários não chamam `--reload-copia` (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:123-173`).

7. **FILA 11: o limite do contador é real, mas há cobertura vizinha dos alvos.** Os 13 contam linhas iniciadas por restart (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:109`), enquanto o vizinho exige a linha exata `restart saas_core saas_ui mensageria` (`bin/tests/test_deploy_reload_gracioso.sh:135-139`). A lista atual é essa (`bin/deploy.sh:327`); resultado de execução do vizinho: [nao verificado].

8. **DESCARTE 12 tem justificativa exagerada.** O vizinho verifica troca de workers e sobrevivência do exec nos casos verdes (`bin/tests/test_deploy_reload_gracioso.sh:107-112`), mas transforma `selo_reciclagem` em `exit 0` (`bin/tests/test_deploy_reload_gracioso.sh:40-41`), responde sempre `STATUS 200` às provas de rota (`bin/tests/test_deploy_reload_gracioso.sh:57`) e devolve sucesso aos outros exec, incluindo readiness (`bin/tests/test_deploy_reload_gracioso.sh:58`). Assim não demonstra mordida das recusas de readiness/rotas/reciclagem (`bin/deploy.sh:366-367`, `bin/deploy.sh:403`, `bin/deploy.sh:410`).

9. **DESCARTE 13 exige ressalva para o contrato literal.** Reciclagem não é apenas contador: vermelho chama `falhou` após a publicação (`bin/deploy.sh:410`), e mede configurações do disco, cmdline/compose e crontab instalado (`bin/selo_reciclagem.sh:45-48`, `bin/selo_reciclagem.sh:53-78`). Essas verificações podem recusar por condição já existente antes do apply (`bin/deploy.sh:301`, `bin/deploy.sh:410`). A afirmação “todo portão respondeu” precisa de uma exclusão explícita desse selo ou de sua execução anterior; a exclusão não está na frase da 1c (`bin/deploy.sh:291-298`).

10. **Registro de desenho prometido não localizado.** O comentário manda ao topo do RELATO (`bin/deploy.sh:298`), mas o topo da cópia trata de outras obras (`app/docs/RELATO.md:2-12`); busca em todo o arquivo por `deploy.?portao|portao.?antes` retornou zero ocorrências. Existência da pergunta prometida: [nao verificado].

Classes arquiteturais, dentro deste delta:

- **Segundo juiz: não demonstrado.** As três consultas usam a mesma função (`bin/deploy.sh:110`, `bin/deploy.sh:117`, `bin/deploy.sh:300`, `bin/deploy.sh:326`); auth continua em um script (`bin/deploy.sh:145`).
- **Segundo escritor de publicação: não introduzido pelo delta.** Restart permanece no ramo explícito e HUP no outro ramo (`bin/deploy.sh:323-344`); a recusa restaurada não acrescenta publicação (`bin/deploy.sh:326`).
- **Relógio fora da borda: nenhum novo `date` no delta fornecido [nao verificado quanto ao histórico].** O juiz lê dia e hora separadamente (`bin/janela_auth.sh:49-56`) e o deploy o consulta antes de checks, collectstatic, prova e migrate (`bin/deploy.sh:145`, `bin/deploy.sh:184`, `bin/deploy.sh:266`, `bin/deploy.sh:286`, `bin/deploy.sh:301`); atravessar a janela temporal antes da publicação é possível por escalonamento, ocorrência [nao verificado].
- **Derivação paralela: não na recusa de cron.** Repetição chama a função única (`bin/deploy.sh:110-117`, `bin/deploy.sh:300`, `bin/deploy.sh:326`).
- **Selo sem mordida: sim, parcialmente.** Caso 12 não distingue a retirada da linha 300; configuração de marca anterior e primeira recusa provam a máscara (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:97`, `bin/deploy.sh:115-117`). RED efetivamente rodado: [nao verificado].
- **Ordem/exclusão de trava: sim, falta exclusão comum; deadlock não demonstrado.** O envelope cria marca e roda comando diretamente (`bin/cron_run.sh:9-11`); o deploy somente amostra a marca (`bin/deploy.sh:108-113`, `bin/deploy.sh:300-326`).
- **Pouso antes do pré-requisito: não para apply antes de publicação nos caminhos atuais** (`bin/deploy.sh:301`, `bin/deploy.sh:327`, `bin/deploy.sh:344`); **sim para apply antes da decisão final de recusa**, no residual aceito (`bin/deploy.sh:301`, `bin/deploy.sh:326`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:164`).

## 08/11 23:54 · sessao d3cf7e7093bb · - · - · origem=Ronald (tmux)

- **b1 e c curados no alvo dos testes**: casos isolados de pk, colaborador e disputa exigem recusa, e o mutante `or → and` é construído explicitamente; isso distingue a ausência de cada parcela da guarda. (`app/chamados/tests/test_e1_fila.py:489`, `app/chamados/tests/test_e1_fila.py:504`)

- **b2 parcialmente curado**: retirar literalmente `_migrar_sob_trava` é distinguido pelo novo par RED/MORDE, mas o RED aceita apenas `recusadas` preenchida. Seu cenário usa Pergunta do colaborador base e chamados de outro colaborador, que o juiz recusa por cruzamento; portanto esse RED não exige migração efetiva nem escrita no banco. (`app/chamados/tests/test_e1_fila.py:147`, `app/chamados/tests/test_e1_fila.py:511`, `app/chamados/tests/test_e1_fila.py:517`, `app/chamados/juizes.py:1999`) Teste antigo exige migração efetiva de conversa, inclusive refresh e destino persistido. (`app/chamados/tests/test_e1_v15g_portas.py:364`)

- **b3 e b4 curados diretamente**: RuntimeError genérica deve subir; modelo e pk do evento agora são conferidos. A captura produtiva permanece específica de `UniversoMudouSobATrava`. (`app/chamados/tests/test_e1_fila.py:566`, `app/chamados/tests/test_e1_fila.py:564`, `app/chamados/models.py:1417`)

- **b5 DESCARTE defensável para a promessa de trava**: RED exige canônica e destino travados, além da ordem. A porta seleciona proposta antes de chamar `_trava_do_ato`; resposta e validação posteriores determinam seu retorno, uma exigência diferente. (`app/chamados/tests/test_e1_fila.py:442`, `app/chamados/services/declaracao_texto.py:643`, `app/chamados/services/declaracao_texto.py:681`)

- **b6 DESCARTE por construção atual**: `_trava_do_ato` recebe motivo/dia, usa `universo_do_fato` e não restringe motivo. Ambas as portas chamam o helper incondicionalmente após selecionar o fato. Os testes próprios exercitam somente `saida_sem_entrada`, portanto uma futura guarda que limitasse a trava a esse motivo escaparia desses casos. (`app/chamados/services/declaracao_texto.py:282`, `app/chamados/services/declaracao_texto.py:339`, `app/chamados/services/declaracao_texto.py:646`, `app/chamados/tests/test_e1_fila.py:180`, `app/chamados/tests/test_e1_fila.py:438`)

- **Mutantes plausíveis ainda não distinguidos pelos novos casos**:
  - Trocar a diferença de disputas por diferença simétrica: recusaria também encolhimento, embora nenhuma disputa usada estivesse fora da trava. A sonda só acrescenta uma linha; falta caso de remoção/encolhimento. (`app/chamados/services/fio.py:2092`, `app/chamados/tests/test_e1_fila.py:473`)
  - Remover `key=str` da ordenação da mensagem: falharia quando a diferença contivesse simultaneamente `None` e um inteiro; cada caso injeta somente uma linha adicional e não forma essa diferença mista. (`app/chamados/services/fio.py:2097`, `app/chamados/tests/test_e1_fila.py:469`, `app/chamados/tests/test_e1_fila.py:492`)
  - Substituir o efeito real do laço por preenchimento artificial de `recusadas` satisfaria o novo RED que promete “o laço roda”; o teste só consulta o retorno. Outros testes antigos podem barrar isso. (`app/chamados/tests/test_e1_fila.py:511`, `app/chamados/tests/test_e1_v15g_portas.py:364`)

- **Limites da prova**: a corrida é simulada por patch de `_conjunto_da_migracao` com `SimpleNamespace`, inclusive duplicando um pk com outro colaborador/disputa; prova a guarda por conjuntos, sem demonstrar concorrência real. O helper retorna imediatamente na exceção, antes da asserção das duas leituras; essa asserção só acontece quando não houve recusa. (`app/chamados/tests/test_e1_fila.py:466`, `app/chamados/tests/test_e1_fila.py:479`) Execução dos testes e sobrevivência na suíte inteira: [nao verificado].

## 08/11 23:54 · sessao 3cf5076efdda · - · - · origem=Ronald (tmux)

Triagem **a: FILA defensável apenas como delimitação do diff; defeitos reais permanecem**. O contador de adesão ignora o retorno e incrementa sempre (`app/chamados/services/adesao.py:89-92`); o reconciliador ignora a recusa (`app/ponto/management/commands/reconciliar_grade.py:529-531`) e o chamador conta `CREATE` (`app/ponto/management/commands/reconciliar_grade.py:254-259`); a ação administrativa reabre Disputa e cria conversa antes de chamar `reabrir`, ignora retorno e anuncia sucesso (`app/chamados/services/acoes_disputa.py:256-267`, `:294-296`); a view atribui todo `False` a irmão vivo (`app/chamados/views_arquivar.py:70-72`). Outros motivos de `False` existem atualmente (`app/chamados/models.py:1391-1407`, `:1437-1438`), mas anterioridade à obra não se prova na cópia sem histórico: **[nao verificado]**.

Triagem **d: FILA técnica real, não cura concluída**. O juiz ainda lê `date.today()` (`app/chamados/juizes.py:569-575`) e tem leitores (`:674`, `:764`, `:1217`). `expira_em` diz reproduzir a regra (`app/chamados/veredito_lavrado.py:120-122`) e conserva implementação própria (`:136-143`), comparável ao juiz (`app/chamados/juizes.py:595-601`). A cura f5 faz `expira_em` usar a borda (`app/chamados/veredito_lavrado.py:95-102`, `:137`), com instante passado pela lavra (`:251-253`). Registro prévio da FILA/cronologia: **[nao verificado]**.

Para a pergunta 5, separar **diff v4** de **contexto da obra**:

| Classe | Resultado e prova |
|---|---|
| Segundo juiz que duplica juiz registrado | **Não identificado nas linhas v4**: elas conferem conjuntos antes de executar o laço (`app/chamados/services/fio.py:2091-2098`); o laço consulta `pergunta_viva` e `canonica_do_grupo` (`:2112`, `:2123`, `:2145`). A autoridade C6 registrada é `pergunta_viva` (`app/core/juizes.py:266`). Ausência absoluta na árvore: **[nao verificado]**. |
| Segundo escritor fora da porta | **Não introduzido pelo diff v4**: ele não acrescenta gravações e mantém `_migrar_sob_trava` depois da conferência (`app/chamados/services/fio.py:2091-2098`). Censo global completo: **[nao verificado]**. |
| Relógio fora da borda | **Sim no contexto**, em `app/chamados/juizes.py:575`; f5 cura somente o leitor da lavra (`app/chamados/veredito_lavrado.py:95-102`, `:137`). |
| Derivação paralela da fonte única | **Sim no contexto**, regra da janela repetida por `expira_em` (`app/chamados/veredito_lavrado.py:120-143`) frente ao juiz (`app/chamados/juizes.py:569-601`). |
| Ordem de trava | **Porta central executa C→D→Ch→P**, respectivamente `app/chamados/models.py:2674`, `:2683`, `:2696`, `:2700`. V4 preserva chamada única de topo (`app/chamados/services/fio.py:2086-2087`). Exatidão do universo depende dos achados do explorador locks. |
| Pouso antes do pré-requisito | **Sim no contexto da ação administrativa**: disputa e conversa mudadas antes de consultar o resultado da reabertura (`app/chamados/services/acoes_disputa.py:256-267`). **Não nas linhas v4**: conferência precede laço (`app/chamados/services/fio.py:2093-2098`); destino aprovado precede retratação (`:2164-2170`). |
| Campo com dois significados | Não encontrei exemplo adicional comprovado nesta exploração; **[nao verificado]**. |

Para **b5**, o descarte é procedente no contrato específico de ordem: o RED exige canônica e destino presentes na sequência e ausência de inversões (`app/chamados/tests/test_e1_fila.py:442-447`), enquanto a porta toma a trava depois de encontrar proposta (`app/chamados/services/declaracao_texto.py:643-650`). O teste não prova sucesso funcional da aplicação; isso fica **[nao verificado]**.

## 08/11 23:54 · sessao 7d1c7808290c · - · - · origem=Ronald (tmux)

- `APLICAR_MIGRATE`: escritores de valor são inicialização `0` e armação `1` (`bin/deploy.sh:61`, `bin/deploy.sh:223`); único leitor operacional é a decisão do apply (`bin/deploy.sh:299`). O comentário “UM escritor” é correto somente contando a armação e excluindo a inicialização explicitamente mencionada na linha seguinte; literalmente existem duas atribuições (`bin/deploy.sh:59`, `bin/deploy.sh:60`). O selo também injeta `APLICAR_MIGRATE=1` no ambiente do subprocesso, sobreposto pela inicialização (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:98`, `bin/deploy.sh:61`).

- `RESTART`: escritores `0` e `1`, este último exclusivamente para `MODO=--restart` (`bin/deploy.sh:70`, `bin/deploy.sh:72`, `bin/deploy.sh:73`). Leitores: guarda inicial de cron (`bin/deploy.sh:115`), exceção da pendência da mensageria (`bin/deploy.sh:200`), guarda imediatamente anterior ao apply (`bin/deploy.sh:300`), escolha restart/HUP (`bin/deploy.sh:323`) e mensagem final (`bin/deploy.sh:429`). Fora dele, o contrato lê o texto do ramo, não o valor em execução (`bin/contrato_deploy.sh:41`).

- Marcas: o envelope cria diretório/nome `N_E.$$`, escreve timestamp e registra remoção no `EXIT` (`bin/cron_run.sh:9`, `bin/cron_run.sh:10`). O deploy enumera arquivos, lê PID do sufixo, consulta `/proc`, devolve o nome dos vivos **e apaga marcas mortas**; portanto também é escritor de limpeza (`bin/deploy.sh:108`, `bin/deploy.sh:109`). A recusa consome esses nomes (`bin/deploy.sh:111`, `bin/deploy.sh:112`), sendo chamada em três pontos (`bin/deploy.sh:117`, `bin/deploy.sh:300`, `bin/deploy.sh:326`). O conteúdo ISO não é lido pelo deploy; o protocolo efetivo depende do nome e da existência do PID (`bin/cron_run.sh:10`, `bin/deploy.sh:109`).

- Escritores dos selos: marca inicial e marca durante o migrate no selo desta obra (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:97`, `bin/tests/test_deploy_portoes_antes_do_migrate.sh:75`), ambas usando PID vivo exportado pelo selo (`bin/tests/test_deploy_portoes_antes_do_migrate.sh:96`); selo vizinho escreve marca com PID de seu processo falso (`bin/tests/test_deploy_reload_gracioso.sh:90`, `bin/tests/test_deploy_reload_gracioso.sh:91`). O contrato apenas procura os textos dos caminhos nos scripts (`bin/contrato_deploy.sh:81`, `bin/contrato_deploy.sh:82`).

- O endereço do escritor é absoluto (`bin/cron_run.sh:9`); o leitor deriva a raiz da localização do script (`bin/deploy.sh:53`, `bin/deploy.sh:108`). A configuração agenda ambos sob `/home/ronald/saas-hasner`, tornando-os equivalentes **nessa localização declarada** (`app/config/crons.py:19`, `app/config/crons.py:326`, `app/config/crons.py:1475`, `app/config/crons.py:1486`). Equivalência universal ou resolução real do caminho em produção: [nao verificado].

- Os elos também passam pelo mesmo escritor de marcas: `encadeado.sh` executa `cron_run.sh` depois de esperar arquivos de fim (`bin/encadeado.sh:89`, `bin/encadeado.sh:102`, `bin/encadeado.sh:104`). O envelope executa o comando uma vez, carimba fim apenas para rc `0`/`2`, e termina com o rc; não há retry nesse fluxo (`bin/cron_run.sh:11`, `bin/cron_run.sh:22`, `bin/cron_run.sh:25`, `bin/cron_run.sh:38`).

- O comentário limita demais o residual ao cron que “NASCE durante o migrate” e à “duração do migrate” (`bin/deploy.sh:295`, `bin/deploy.sh:296`). Sem exclusão entre envelope e deploy, a marca pode nascer **depois da varredura prévia terminar, antes do apply começar**, ou **depois do apply retornar, antes da varredura final**; nesses caminhos o banco pode andar e a seção 2 recusar (`bin/deploy.sh:300`, `bin/deploy.sh:301`, `bin/deploy.sh:324`, `bin/deploy.sh:326`, `bin/cron_run.sh:9`). Uma marca também pode nascer depois da varredura final e antes do restart, permitindo matar seu comando (`bin/deploy.sh:326`, `bin/deploy.sh:327`, `bin/cron_run.sh:9`, `bin/cron_run.sh:11`). Ocorrência real desses interleavings: [nao verificado].

_2953 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
