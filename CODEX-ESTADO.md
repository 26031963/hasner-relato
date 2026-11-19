# CODEX-ESTADO

> Ultima resposta processada: **19/11/2026 15:08** · publicado em **19/11/2026 15:09** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 19/11 15:08 · sessao b4d0bb13a00e · REPLAY-PORTA-EXPORT-SOL62 · verificador · origem=Code

**Reprovado na cópia.** O POST dispara o comando em segundo plano e o carimbo ganhou uma chave por competência. A obra ainda permite duas medições consecutivas do mesmo pedido, permite que uma tentativa sobrescreva o estado de outra e pode publicar `SELO VERDE` com um carimbo vermelho da própria rodada. A virada do dia também pode dar data de hoje a uma medição iniciada ontem. `app/folha/porta_export.py:774`, `app/folha/porta_export.py:990`, `app/folha/porta_export.py:1138`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:128`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:150`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:236`.

Os seis arquivos conferem com os hashes de conteúdo do diff. Revisei a cópia em leitura e executei apenas duas verificações em memória, do scanner AST e da agregação com resultados simulados. Hash do commit completo, suíte com banco, dados e execução em produção [nao verificado]. [nao verificado]

### 1. Quem escreve e quem lê

| Campo ou linha | Escritor e leitor encontrados na cópia |
|---|---|
| `porta_export_leitores:AAAA-MM` | `lavrar` escreve. O comando chama `lavrar` e `estado_da_porta` lê. Ambos usam `chave_do_carimbo`. O campo existente comporta 60 caracteres. Em testes, `_carimbo` escreve diretamente, `lavrar_verde_para_teste` delega a `lavrar` e o teste R4 consulta a linha diretamente. `app/folha/porta_export.py:775`, `app/folha/porta_export.py:875`, `app/folha/porta_export.py:917`, `app/folha/porta_export.py:1078`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:135`, `app/inteligencia/models.py:87`, `app/folha/tests/test_porta_do_export.py:40`, `app/folha/tests/test_porta_sem_beco.py:177`. |
| `porta_export_pedido:AAAA-MM` | `pedir` cria, `assumir` cria ou atualiza, `_linha_do_pedido` lê. Ambos os snapshots usam `escopo='empresa'`, `escopo_id=empresa.pk` e `data_ref` na identidade. Os escritores também preenchem `data_ini` com o dia civil. A porta consulta `data_ref` e não usa `data_ini`. `app/folha/porta_export.py:776`, `app/folha/porta_export.py:923`, `app/folha/porta_export.py:977`, `app/folha/porta_export.py:1009`, `app/folha/porta_export.py:1029`, `app/folha/porta_export.py:1078`, `app/inteligencia/models.py:103`. |
| `detalhe['estado']` do pedido | `pedir` grava `PEDIDO`, `assumir` grava `EM_CURSO` e `_mudar_estado` grava as demais transições chamadas por `pedir`, `encerrar` e `vigiar`. `disparar` pede `FALHOU` via `encerrar` quando recebe `OSError`. Leem o estado `pedir`, `vigiar`, `estado_da_porta` e o filtro da própria atualização condicional. `vigiar` lê `pedido_em` e `_campos_do_texto` lê `motivo`. `app/folha/porta_export.py:988`, `app/folha/porta_export.py:990`, `app/folha/porta_export.py:1005`, `app/folha/porta_export.py:1017`, `app/folha/porta_export.py:1032`, `app/folha/porta_export.py:1040`, `app/folha/porta_export.py:1050`, `app/folha/porta_export.py:1057`, `app/folha/porta_export.py:1080`, `app/folha/porta_export.py:1097`, `app/folha/porta_export.py:1141`. |
| `valor` | No carimbo, `lavrar` grava o total de falhas ou `-1` quando a medição não produz esse total. `estado_da_porta` decide com ele e `conferir` o exibe na divergência. No pedido, `pedir` e `assumir` gravam `NULL`. A decisão da porta usa o estado do pedido. `app/folha/porta_export.py:777`, `app/folha/porta_export.py:862`, `app/folha/porta_export.py:1011`, `app/folha/porta_export.py:1031`, `app/folha/porta_export.py:1080`. |
| `calculado_em` | O modelo atualiza o instante nos salvamentos via `auto_now`. `_mudar_estado` o grava explicitamente no `QuerySet.update`. A porta decide a validade pelo dia em `data_ref` e o prazo pelo `pedido_em`. `app/inteligencia/models.py:96`, `app/folha/porta_export.py:991`, `app/folha/porta_export.py:1057`, `app/folha/porta_export.py:1078`. |

Não encontrei leitor ativo da chave antiga nem segundo escritor do carimbo fora das fixtures. A chave antiga aparece como prefixo e em documentação histórica. A API genérica `serie` aceita chave explícita, sem chamada para essas chaves na cópia. O escritor genérico exige registro, e os registros encontrados são de outras métricas. `app/folha/porta_export.py:18`, `app/folha/porta_export.py:920`, `app/docs/RELATO-ARQUIVO.md:42296`, `app/docs/PENDENTES_RONALD.json:2342`, `app/inteligencia/metricas.py:39`, `app/inteligencia/metricas.py:62`, `app/inteligencia/calculadores.py:9`, `app/inteligencia/calculadores.py:302`.

### 2. Quando `conferir` libera

- **Para os valores produzidos por `medir`, o retorno vazio exige um snapshot com zero, filtrado por empresa, competência e dia civil da consulta.** Ausência, `NULL`, negativo e positivo barram. A competência vem da chave, e a leitura deixou de confrontar `detalhe.mes/ano`. Um `NaN` que chegasse ao leitor escaparia dos dois testes de sinal e cairia em `PRONTA`. Linha desse tipo na cópia de dados ou em produção [nao verificado]. `app/folha/porta_export.py:762`, `app/folha/porta_export.py:813`, `app/folha/porta_export.py:1078`, `app/folha/porta_export.py:1081`.
- **Na virada do dia, um carimbo com a data certa pode representar uma medição de ontem.** `medir` fixa o instante na entrada. `lavrar` consulta o dia de novo no fim. O pedido pode ficar em D, o carimbo em D+1 e a porta liberar em D+1. A leitura também consulta o relógio separadamente para pedido e carimbo. `app/folha/porta_export.py:318`, `app/folha/porta_export.py:338`, `app/folha/porta_export.py:774`, `app/folha/porta_export.py:978`, `app/folha/porta_export.py:1028`, `app/folha/porta_export.py:1077`.
- **Um verde anterior do mesmo dia pode liberar com uma nova medição em curso.** Basta o cron assumir depois da leitura do pedido e antes da decisão sobre o carimbo. A consulta à trava só acontece se a cópia lida do pedido disser `PEDIDO` ou `EM_CURSO`. Um verde anterior também prevalece sobre `FALHOU` já persistido. `app/folha/porta_export.py:1051`, `app/folha/porta_export.py:1077`, `app/folha/porta_export.py:1081`.

### 3. Request e concorrência

- **O request não chama `medir` nem `lavrar` para a porta.** Ele consulta, registra o pedido e usa `Popen` para o mesmo `tenant_command`, com schema, empresa, mês e ano. O recálculo que a view faz depois da liberação pertence à montagem do TXT. O diff altera a chamada da porta, e a montagem segue pelo mesmo `montar_linhas_export`. `app/folha/views.py:130`, `app/folha/views.py:209`, `app/folha/views.py:267`, `app/folha/porta_export.py:1108`, `app/folha/porta_export.py:1129`, `app/folha/porta_export.py:1138`.
- **Duas chamadas completas de `medir` para o mesmo alvo ficam mutuamente excluídas** se os processos usam o mesmo banco, o mesmo schema e preservam suas sessões PostgreSQL. O comando que perde a trava pula o alvo. Isso vale entre `saas_ui` e `saas_core` sob essas condições. Banco e sessões efetivos dos containers em produção [nao verificado]. `app/folha/porta_export.py:934`, `app/folha/porta_export.py:952`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:122`, `app/config/settings/saas.py:83`.
- **A trava não deduplica execuções consecutivas.** O cron pode concluir enquanto o filho do clique ainda sobe. Quando o filho consegue a trava, `assumir` sobrescreve `CONCLUIDO` e mede de novo. Dois cliques também podem repetir se o segundo guardou uma leitura de `SEM_MEDICAO` e só chama `pedir` após a conclusão do primeiro. Em paralelo, alvos de empresas diferentes podem repetir a mesma varredura `cartao_x_txt_divergentes`, que percorre todas as ativas da competência sob travas de empresa diferentes. `app/folha/porta_export.py:1017`, `app/folha/porta_export.py:1029`, `app/folha/porta_export.py:1108`, `app/folha/porta_export.py:667`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:122`, `app/relatorios/management/commands/cartao_x_txt_divergentes.py:51`.

### 4. Estados, vigia e chave da trava

- **Processo morto é tratado na próxima leitura.** `EM_CURSO` sem trava vira `FALHOU`. `PEDIDO` sem trava vira `FALHOU` após 300 s. Se a sessão continua segurando a trava e o job fica pendurado, a porta do dia segue em `EM_CONFERENCIA` sem prazo. Linhas antigas podem ficar com estado ativo persistido porque a vigia só consulta o dia corrente. `app/folha/porta_export.py:900`, `app/folha/porta_export.py:978`, `app/folha/porta_export.py:1051`, `app/folha/porta_export.py:1060`, `app/folha/porta_export.py:1077`.
- **Uma tentativa pode encerrar outra.** Exemplo concreto: o clique cria `PEDIDO`, o cron assume a linha, o `Popen` falha e o clique grava `FALHOU` sobre o `EM_CURSO` do cron. O cron pode lavrar e depois falhar ao gravar `CONCLUIDO`. O filtro testa só `pk` e estado, e o comando ignora o retorno. Pelo mesmo motivo, um vigia atrasado pode marcar como morta uma tentativa nova que voltou ao mesmo estado. Quando a atualização perde, o vigia devolve o objeto antigo sem releitura. `app/folha/porta_export.py:990`, `app/folha/porta_export.py:1037`, `app/folha/porta_export.py:1051`, `app/folha/porta_export.py:1141`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:135`.
- **O motivo de `FALHOU` pode sumir antes da resposta.** Sem carimbo, o POST vê `FALHA`, repede e limpa `detalhe` antes de montar a mensagem. Com carimbo verde ou divergente, o carimbo prevalece sobre o pedido falho. `app/folha/porta_export.py:1017`, `app/folha/porta_export.py:1081`, `app/folha/porta_export.py:1108`.
- **`pg_locks` está certo na faixa nominal.** Filtra o banco corrente, `advisory`, `granted`, `classid` com máscara sem sinal, `objid=k2` e `objsubid=2`. A codificação perde o século: 11/2000 e 11/2100 colidem para a mesma empresa, e ambos os anos passam no POST. Schemas distintos também colidem por CRC32. Verifiquei em memória `t_972a1e33e0984073` e `t_53b380036f90f404`, ambos com `334301602`. Empresas distintas têm `k2` distinto na faixa nominal, mas `pk=214749` em 11/2026 já excede `int4`. O código também não converte `k2` para sem sinal se uma entrada fora da faixa nominal o tornar negativo. `app/folha/porta_export.py:928`, `app/folha/porta_export.py:937`, `app/folha/porta_export.py:952`, `app/folha/porta_export.py:969`, `app/core/sanitiza.py:30`, `app/config/settings/base.py:104`.

### 5. Juízes, relógio e selo de tela

- **Não encontrei duplicação de juiz registrado nem inversão de ordem de trava no diff.** O rollout delega ao `_K8` e a medição consulta `classificar_export`, `_K1/_K2`. O comando toma a trava antes de `assumir`, e a transação local de `pedir` termina antes do disparo. A corrida identificada é entre a consulta de vida e a atualização do pedido. A derivação paralela de competência está no default preexistente da tela, que usa mês civil e já consta como pendência no registro. `app/core/juizes.py:329`, `app/core/juizes.py:345`, `app/core/juizes.py:824`, `app/ponto/services/gestao_he.py:427`, `app/folha/porta_export.py:469`, `app/folha/porta_export.py:1008`, `app/folha/porta_export.py:1051`, `app/folha/views.py:90`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:122`.
- **Há releitura de relógio fora da borda.** Além da virada entre `medir` e `lavrar`, o rollout consulta o dia por empresa. `data_ref` é dia civil e `calculado_em` é instante. O campo genérico `valor` já tem mais de uma unidade: percentual no armazém genérico e contagem de falhas ou sentinela no carimbo. O total do carimbo combina falhas próprias com falhas de toda a competência. `app/inteligencia/models.py:90`, `app/inteligencia/metricas.py:55`, `app/folha/porta_export.py:762`, `app/folha/porta_export.py:774`, `app/folha/porta_export.py:777`, `app/ponto/services/gestao_he.py:427`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:48`.
- **O selo AST morde literais e não cobre o fluxo de dados até o template.** Em memória, o scanner varreu 494 arquivos, encontrou zero ocorrências no recorte, detectou um literal proibido e deixou passar a mesma mensagem montada por concatenação. Ele exclui templates, JS e commands. Há caminho concreto para valores dinâmicos: a razão social cadastrada e o texto de exceções entram na recusa, que chega ao template via `messages` ou ao cliente via `HX-Trigger`. A instrução fixa antiga saiu. Ocorrência desses valores em produção [nao verificado]. `app/folha/tests/test_porta_sem_beco.py:354`, `app/folha/tests/test_porta_sem_beco.py:370`, `app/folha/tests/test_porta_sem_beco.py:384`, `app/folha/tests/test_porta_sem_beco.py:413`, `app/colaboradores/views.py:509`, `app/colaboradores/views.py:540`, `app/folha/porta_export.py:908`, `app/folha/porta_export.py:1097`, `app/folha/porta_export.py:1141`, `app/folha/views.py:135`, `app/core/respostas.py:63`, `app/templates/folha/exportar.html:10`.

### 6. Alvos, soma e `SystemExit`

- **Os alvos válidos delimitam a lavra.** `--rollout` pega as ativas em rollout na competência corrente de cada uma. `--empresa N` com ID positivo pega apenas a ativa indicada. **`--empresa 0` é aceito e tratado como opção ausente**, deixando a execução explícita por mês e ano carimbar todas as ativas. O subcontador de cartão percorre todas as ativas mesmo numa lavra restrita. O cron diário consta como fora do pipeline na cópia. Cron instalado ou executado em produção [nao verificado]. `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:35`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:44`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:52`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:135`, `app/relatorios/management/commands/cartao_x_txt_divergentes.py:51`, `app/config/crons.py:769`, `app/config/crons.py:887`.
- **A soma por competência está certa se as releituras do mesmo mês e ano concordam. O veredito pode inverter se discordam.** O último valor substitui o anterior. Em memória, com duas empresas em 10/2026 retornando `1` e `0`, uma terceira em 11/2026 retornando `0` e todas as falhas próprias em zero, o comando simulou lavras `1, 0, 0` e publicou `SELO VERDE`. A identidade por empresa passa nos três casos. Os testes de soma usam competência fixa e valores repetidos iguais, e o teste de rollout mede um único alvo com `medir` simulado. Com alvos compartilhados e total zero, o comando imprime `SEM VEREDITO` e retorna normalmente. `app/folha/porta_export.py:318`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:149`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:188`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:226`, `app/relatorios/tests/test_selo_leitores_no_mesmo_numero.py:136`, `app/relatorios/tests/test_selo_leitores_no_mesmo_numero.py:166`, `app/folha/tests/test_porta_sem_beco.py:226`.
- **O efeito de `SystemExit` depende do ponto da saída.** Exceção comum de `medir` grava `FALHOU` antes de sair. `SystemExit` dentro de `medir`, ou exceção na lavra, deixa `EM_CURSO` até a vigia após a liberação da trava. Saída antes de assumir deixa `PEDIDO` até o prazo, como no POST direto para empresa inativa. Se `cartao_x_txt=None`, o comando grava carimbo `-1` e `CONCLUIDO` antes de sair vermelho. Nesse caso o motivo se perde: `medir` devolve `erro_cartao_x_txt`, `lavrar` omite o campo e a mensagem tenta lê-lo. As validações posteriores também podem sair em vermelho depois de o carimbo já ter sido gravado. `app/folha/views.py:120`, `app/folha/porta_export.py:737`, `app/folha/porta_export.py:777`, `app/folha/porta_export.py:799`, `app/folha/porta_export.py:956`, `app/folha/porta_export.py:1053`, `app/folha/porta_export.py:1097`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:61`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:129`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:137`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:189`.

## 19/11 15:07 · sessao d5fdbc36d494 · - · - · origem=Ronald (tmux)

- **Carimbo.** `lavrar` é o escritor de produção. Grava chave por competência, empresa, dia local, valor e detalhe. O comando chama `lavrar` após `medir`. `estado_da_porta` consulta pela mesma função de chave, empresa e dia, e lê `valor`. `conferir` lê detalhes e valor da divergência. O único escritor direto adicional é uma fixture. A chave antiga aparece como prefixo e em documentos históricos, sem consulta executável encontrada. `app/folha/porta_export.py:18,768–799,812–863,917–920,1077–1090`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:128–136`; `app/folha/tests/test_porta_do_export.py:33–42`; `app/docs/RELATO-ARQUIVO.md:42296`; `app/docs/PENDENTES_RONALD.json:2342`.

- **Pedido.** `pedir` cria `PEDIDO`, `assumir` grava `EM_CURSO`, e `_mudar_estado` atualiza estado/timestamps por PK e estado anterior. O reset, o encerramento pelo comando, o vigia e a falha de `Popen` passam por essa transição. `_linha_do_pedido` consulta chave, empresa e dia. `pedir`, `vigiar` e `estado_da_porta` leem o estado. O prazo usa `pedido_em`, a mensagem usa o motivo, e `valor` do pedido nasce e permanece `NULL`. `calculado_em` usa `auto_now` nas gravações por modelo e atualização explícita no CAS, sem leitor decisório nessas famílias. `app/folha/porta_export.py:975–1063,1077–1099,1140–1144`; `app/inteligencia/models.py:87–105`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:128–136`.

- **Liberação.** O retorno vazio só passa por `PRONTA`. No fluxo normal, ela exige snapshot com a chave, empresa e dia consultados, de valor zero. A guarda já não cruza a competência do JSON. Uma linha anômala com `NaN` cairia no `else`, mas o caminho normal produz inteiros ou `-1`, e a existência de linha anômala é [não verificado]. Uma medição iniciada antes da meia-noite pode publicar carimbo do dia seguinte, pois `medir` fixa seu instante no início e `lavrar` lê a data novamente. `FALHOU` também fica oculto por um verde anterior. `app/folha/porta_export.py:318,337–338,664–691,762–777,812–814,1077–1090`.

- **Corridas.** O lock exclui `medir` simultâneo do mesmo alvo na mesma base/schema, mas jobs de empresas distintas repetem simultaneamente a testemunha global da mesma competência. O vigia pode marcar um sucessor `FALHOU` entre leitura da trava e CAS. Um `Popen` que falha pode fazer o mesmo a um cron já em curso na linha compartilhada. Um filho atrasado pode medir novamente depois que o cron libera a trava. `app/folha/porta_export.py:664–670,940–972,981–994,1023–1062,1140–1144`; `app/relatorios/management/commands/cartao_x_txt_divergentes.py:20–24,51–75`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:121–136`.

- **Escopo e veredito.** `--empresa 0` ignora o filtro e alcança todas as ativas. `DA_COMPETENCIA` registra a última leitura por mês/ano. Uma leitura positiva seguida por zero na mesma competência pode publicar selo verde com carimbo vermelho já lavrado para a primeira empresa. `cartao_x_txt=None` carimba `-1` e encerra como `CONCLUIDO` antes de sair vermelho. O detalhe do carimbo omite `erro_cartao_x_txt`, embora a mensagem o tente ler. `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:32–55,128–151,188–193,226–237`; `app/folha/porta_export.py:671–675,737,779–799,1094–1099`.

- **Selo.** O scanner AST lido em memória encontrou zero ocorrências em 494 arquivos do seu recorte. Morde literal proibido e deixa passar concatenação com `+` e dados interpolados. Razão social cadastrada e motivos de exceção podem carregar o texto até a resposta. A cópia cadastra o comando em `FORA_DE_PIPELINE`, sem cron diário ali. `app/folha/tests/test_porta_sem_beco.py:354–418`; `app/colaboradores/views.py:504–509,535–552`; `app/folha/porta_export.py:910–913,1094–1099`; `app/folha/views.py:130–135`; `app/core/respostas.py:63–71`; `app/templates/folha/exportar.html:10`; `app/config/crons.py:769,887–897`. Execução da suíte, do subprocesso real e comportamento em produção [não verificado].

## 19/11 15:07 · sessao aae1bfb4e6c5 · - · - · origem=Ronald (tmux)

**Q5.** O rollout delega ao juiz registrado `janela_atual` e a decisão da porta fica concentrada em `estado_da_porta`. A seleção inicial da tela ainda deriva a competência pelo mês civil de `timezone.now()`, pendência já registrada na cópia. `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:44–49`, `app/ponto/services/gestao_he.py:424–429`, `app/core/juizes.py:345,824–825`, `app/folha/porta_export.py:812–815`, `app/folha/views.py:88–97`.

- **Virada do dia.** `assumir` grava o dia inicial, `medir` fixa seu instante e `lavrar` consulta a data novamente quando termina. Medição iniciada em D, encerrada após meia-noite, pode liberar D+1 com âncora de D e pedido na partição de D. O leitor também consulta separadamente o dia do pedido e do carimbo. `app/folha/porta_export.py:21–23,313–338,774–777,975–978,1023–1034,1077–1090`.
- **Selo AST.** Em execução isolada somente em memória, o scanner varreu 494 arquivos da cópia, obteve zero ocorrências, pegou o literal e deixou passar a concatenação `'Ro' + 'de: ' + 'docker' + ' exec saas_core'`. Ele examina constantes contíguas de `.py` nos apps enumerados. A razão social entra por POST e pode carregar esses termos ao texto da porta, que chega ao template por Django messages. Motivos brutos de exceção também são interpolados. `app/folha/tests/test_porta_sem_beco.py:354–418`, `app/colaboradores/views.py:504–512,535–552`, `app/folha/porta_export.py:815–816,911–913,1094–1099,1140–1144`, `app/folha/views.py:130–135`, `app/core/respostas.py:63–71`, `app/templates/folha/exportar.html:10`.
- **Semântica.** O modelo documenta `valor` como percentual. O carimbo guarda contagem e sentinela `-1`, comportamento preexistente, e o pedido guarda `NULL`. `calculado_em` do pedido passa a indicar transição de estado. `app/inteligencia/models.py:81–96`, `app/folha/porta_export.py:775–779,987–991,1008–1011,1029–1033`.
- **Travas.** O command pega a advisory antes da linha do pedido. O POST encerra seu `atomic` local antes de disparar. O vigia faz leitura de `pg_locks` e CAS em operações separadas, com janela de corrida. `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:121–136`, `app/folha/porta_export.py:1008–1013,1051–1055,1108–1113`.

**Q6.** O command carimba apenas alvos com argumentos válidos, porém `--empresa 0` é aceito e cai no caminho de todas as ativas. A medição global de cartão × TXT varre todas as ativas da competência, inclusive fora do rollout, e pode repetir essa submedição em outro processo trancado por empresa diferente. `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:32–55,121–136`, `app/folha/porta_export.py:664–670`, `app/relatorios/management/commands/cartao_x_txt_divergentes.py:51–75`.

- **Veredito pode dar verde falso.** As competências distintas somam, mas dentro da mesma competência a última medição substitui as anteriores. Em sonda simulada do `handle`, empresas A/B em 11/2026 retornaram 1/0 e C em 12/2026 retornou 0. As chamadas simuladas de lavra foram 1/0/0 e a saída final foi `SELO VERDE`. Em execução real, o contador varre dados vivos a cada empresa. O teste de agregação fixa uma competência e o teste de rollout mocka `medir` para uma empresa. `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:121–151,224–237`, `app/folha/porta_export.py:313–318,664–670`, `app/relatorios/tests/test_selo_leitores_no_mesmo_numero.py:128–139,163–225`, `app/folha/tests/test_porta_sem_beco.py:217–232`.
- **`SystemExit` e motivo.** Exceção comum em `medir` grava `FALHOU`; `SystemExit` interno ou falha na lavra deixam `EM_CURSO` até a vigia. Empresa desativada entre clique e execução deixa `PEDIDO` até o prazo. `cartao_x_txt=None` grava `-1` e `CONCLUIDO` antes de encerrar vermelho. O erro de cartão calculado fica fora da lavra e a interface pode exibir motivo genérico. Validações posteriores também podem imprimir vermelho depois de carimbo persistido. `app/folha/views.py:109–120`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:52–65,129–139,165–193`, `app/folha/porta_export.py:671–675,737,775–799,954–959,1043–1062,1094–1099`.
- A cópia lista o selo em `FORA_DE_PIPELINE`. Instalação e execução em produção [não verificado]. `app/config/crons.py:769,887–897`.

## 19/11 15:07 · sessao 0cdddb10b871 · - · - · origem=Ronald (tmux)

**Q3.** O POST confere a porta e dispara o mesmo command via `Popen`, com schema, empresa e competência. `medir` e `lavrar` são chamados pelo command, sob a trava, antes de liberar a sessão. Sessões distintas que usam o mesmo banco e a mesma chave válida não medem simultaneamente enquanto a sessão proprietária mantém a trava. Configuração efetiva das duas cascas em produção [não verificado]. `app/folha/views.py:130–150`, `app/folha/porta_export.py:940–960,1108–1141`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:121–136`, `app/config/settings/saas.py:76–93`.

- Há duplicação sequencial. O cron pode consumir e concluir o `PEDIDO` enquanto o filho do clique inicia, e o filho, ao entrar depois, sobrescreve `CONCLUIDO` e mede novamente. Dois cliques com leitura inicial defasada também podem repetir, pois `pedir` aceita `CONCLUIDO` sem reconferir o carimbo. `app/folha/porta_export.py:997–1034,1108–1113`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:121–136`.
- Dois alvos de empresas diferentes podem executar simultaneamente a testemunha global `cartao_x_txt_divergentes`, que varre todas as empresas ativas da competência. As chamadas completas de `medir` continuam separadas pelo alvo. `app/folha/porta_export.py:665–670,928–937`, `app/relatorios/management/commands/cartao_x_txt_divergentes.py:20–24,51–70`.

**Q4.** O vigia usa banco atual, `granted`, `classid` sem sinal, `objid` e `objsubid=2`, coerentes com os dois `int4` na faixa nominal. Há colisão de 2000 com 2100 para o mesmo mês e empresa, ambas datas aceitas pelo POST. Há colisão de CRC32 entre os schemas `t_972a1e33e0984073` e `t_53b380036f90f404` pelo algoritmo local, verificada em memória. Empresa 214749 em 11/2026 produz `2147492611`, fora de `int4`. Instâncias reais [não verificado]. `app/folha/porta_export.py:928–937,963–972`, `app/core/sanitiza.py:8,30–35`, `app/config/settings/base.py:104`.

- Sem trava viva, a próxima leitura transforma `EM_CURSO` em `FALHOU` ou expira `PEDIDO` após 300 s. Com sessão viva e medição travada, inexiste prazo para sair da conferência. Um `PEDIDO` do dia seguinte também pode enxergar a mesma trava de ontem, porque a data está na linha e ausente da chave advisory. `app/folha/porta_export.py:900,937,975–978,1043–1063,1077–1082`.
- O CAS aceita apenas `pk` e estado. Um vigia atrasado pode reprovar um `PEDIDO` novo que reutilizou o mesmo `pk`. Se o CAS falha, o retorno continua usando a instância anterior e pode exibir conferência temporariamente. `app/folha/porta_export.py:981–994,1017–1019,1043–1063`.
- Se o cron assume enquanto o clique ainda tenta iniciar, a falha de `Popen` pode gravar `FALHOU` por cima do cron vivo. O cron pode lavrar, mas sua transição para `CONCLUIDO` exige estado ativo e seu retorno é ignorado. `app/folha/porta_export.py:1023–1040,1140–1145`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:128–136`.
- `FALHOU` perde a precedência para carimbo existente, inclusive verde. Sem carimbo, o POST pode consumir a própria falha detectada e apagar o motivo ao repedir antes de renderizar. `app/folha/porta_export.py:1017–1019,1080–1099,1108–1113`.

**Relógio e cron.** `medir` congela um instante de início, `assumir` data o pedido e `lavrar` relê o dia ao terminar. Uma rodada cruzando a meia-noite pode dar carimbo de hoje a uma medida de ontem. A leitura do pedido e do carimbo também recebe o dia de duas chamadas separadas. O diário consta fora do pipeline na cópia. `app/folha/porta_export.py:318,338,774–777,975–978,1028–1033,1077–1079`, `app/config/crons.py:769,887–897`.

**Q6 complementar.** `--empresa 0` é ignorado e leva às empresas ativas porque o recorte testa a veracidade do inteiro. Empresa inativa pode gerar `PEDIDO` pelo POST, mas o command sai antes de assumir e o vigia só o expira depois. `app/folha/views.py:109–120`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:32–65`, `app/folha/porta_export.py:1056–1063`. A soma deduplica cada competência pelo último valor. Se o contador global de A vale 1 e o de B, da mesma competência, vale 0, o selo pode publicar VERDE com carimbo vermelho para A. A identidade verificada por empresa não valida igualdade entre leituras globais. `app/folha/porta_export.py:318,665–673,762–764`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:135,148–151,188–193,226–237`.

Os seis blobs locais correspondem aos identificadores pós-diff de `diff_A.patch`. HEAD e execução dos testes [não verificado]. [nao verificado]

## 19/11 15:06 · sessao aea9697bb03f · DST-L2-JZ1-C7 · verificador · origem=Code

1. **As premissas conferem na cópia.** `retratar_com_motivo` entra no `atomic`, toma `trava_na_ordem` quando há `colaborador_id` e relê só `justificativa_relacionada` com `ch.refresh_from_db(fields=['justificativa_relacionada'])`. A retenção e o ato recebem a instância com o ponteiro relido. A correspondência da cópia com o hash informado está [nao verificado]. [app/chamados/reconciliador.py:537] [app/chamados/reconciliador.py:544] [app/chamados/reconciliador.py:557]

   O teste `test_iii_b_fora_do_ato_PONTEIRO_gravado_D3_rejeita` carrega `ch`, grava o ponteiro para J3 sem atualizar a instância e chama a unidade com `aprovando=False`. Ele exige `(True, None)`, chamado `resolvido` e J3 `rejeitada`. O ramo de retenção por justificativa alheia só roda em `_APROVANDO`. No ato, o D3 inclui o ID do ponteiro na consulta de pendentes e grava a rejeição. A execução do teste está [nao verificado]. [app/chamados/tests/test_dst_l2_jz1_c7_retencao_sob_trava.py:177] [app/chamados/tests/test_dst_l2_jz1_c7_retencao_sob_trava.py:192] [app/chamados/reconciliador.py:453] [app/chamados/reconciliador.py:583] [app/ponto/services/justificativa.py:280] [app/ponto/services/justificativa.py:351] [nao verificado]

2. **O item 5 resolve a colisão apontada na volta 1.** O aval de construção diz em B1 “reler do BANCO o ponteiro do chamado” e em B2(iii) “fora de _APROVANDO nada muda (a rejeicao pelo D3 segue sendo a regra)”. O aval 1510 diz “B1 PREVALECE” e determina que a frase passa a ler-se “a REGRA nao muda”. Isso autoriza a mudança de resultado que motivou o `BLOQUEIA` da volta 1 e que o próprio teste iii-b declara. A localização dos trechos dos anexos em arquivo está [nao verificado]. [app/chamados/tests/test_dst_l2_jz1_c7_retencao_sob_trava.py:17] [nao verificado]

3. **A contagem dos pares fecha. A contagem dos desfechos fica indefinida.** O primeiro universo é enumerável pelo `status_local` em `VIVOS` (`aberto`, `em_analise`, `registrado`) e pela FK gravada apontando para uma justificativa com `status='pendente'`. Cada par pode ser listado pelos dois PKs. O total e a lista da frota estão [nao verificado]. [app/chamados/catalogo/motor.py:24] [app/chamados/models.py:75] [app/ponto/services/justificativa.py:283]

   A unidade real do desfecho completo é uma só, `retratar_com_motivo`. Ela executa retenção, transição, D3 e cerca. `retencao_da_retratacao` decide a retenção, `pendentes_do_vinculo` define as candidatas do D3 e `reconciliaria()` consulta a decisão de fechamento. Cada função cobre uma parte do desfecho. [app/chamados/reconciliador.py:439] [app/chamados/reconciliador.py:528] [app/chamados/reconciliador.py:557] [app/chamados/reconciliador.py:576] [app/ponto/services/justificativa.py:262] [app/chamados/models.py:670]

   O item 5 deixa em aberto qual ponteiro a instância velha teria, se a sonda roda dentro ou fora de `_APROVANDO` e se “desfecho” significa o retorno da unidade, o estado do chamado, o da justificativa ou a combinação deles. No iii-b, por exemplo, a rejeição muda enquanto o chamado resolve. Rodar o código atual com duas instâncias diferentes também não reproduz a leitura antiga, porque ambas recebem o ponteiro do banco. Sem definir essa comparação, o par gravado identifica um candidato à medição, sem dizer se ele mudaria de desfecho. [app/chamados/tests/test_dst_l2_jz1_c7_retencao_sob_trava.py:18] [app/chamados/tests/test_dst_l2_jz1_c7_retencao_sob_trava.py:177] [app/chamados/reconciliador.py:453] [app/chamados/reconciliador.py:557]

4. **O fluxo inspecionado não abre exceção à cerca nem contém escrita direta de dinheiro.** A cerca veta a escrita de pergunta de competência entregue e desfaz o ato no savepoint. O D3 grava `status` e `descricao` da justificativa e pode acionar o cartório pelo `post_save`. A exigência de `!` para alterar o `FechamentoMensal` exportado está na L-092. Efeitos indiretos sobre competência exportada ou valores na frota estão [nao verificado]. [app/chamados/reconciliador.py:562] [app/chamados/services/fio.py:1424] [app/chamados/services/fio.py:1448] [app/ponto/services/justificativa.py:351] [app/ponto/signals.py:161] [CLAUDE.md:411]

**PROCEDE (o item 5 não define a referência de comparação nem o desfecho a contar na sonda).** [app/chamados/reconciliador.py:453] [app/chamados/reconciliador.py:557] [app/chamados/tests/test_dst_l2_jz1_c7_retencao_sob_trava.py:192]

## 19/11 14:56 · sessao c9af4452ff4a · E4C-F5-CONSTROI · construtor · origem=Code

Cura entregue: instantes do ato e texto da projeção nos dois juízes ([juizes.py:1389](/home/ronald/wt-codex-e4c-f5-constroi-144636/app/chamados/juizes.py:1389), [juizes.py:1559](/home/ronald/wt-codex-e4c-f5-constroi-144636/app/chamados/juizes.py:1559)).

- [Diff completo](/home/ronald/wt-codex-e4c-f5-constroi-144636/app/docs/E4C-F5-VOLTA2.diff:1).
- [REDs e comandos individuais](/home/ronald/wt-codex-e4c-f5-constroi-144636/app/docs/E4C-F5-VOLTA2.md:29).
- [Mutantes ancorados e qual RED derruba cada um](/home/ronald/wt-codex-e4c-f5-constroi-144636/app/docs/E4C-F5-VOLTA2.md:68).

LEI-AKITA: padrao=Parallel Change / Expand-Contract; [registro completo](/home/ronald/wt-codex-e4c-f5-constroi-144636/app/docs/E4C-F5-VOLTA2.md:22).

Banco e suíte não executados. RED/GREEN, custo real e morte dos mutantes: [não verificado]. [nao verificado]

## 19/11 14:54 · sessao 6499ab8fd055 · REPLAY-F5-SOL · verificador · origem=Code

1. **NÃO BLOQUEIA.** O bloqueio específico do `strip()` bilateral está fechado. Os dois logs fornecidos mostram no topo o mesmo diff, aplicando `strip()` aos dois lados da comparação. Na base `71905e5e6`, `M_strip_velho` passa os 3 testes (`rc=0`). Em `6978b3234`, `M_strip_novo` roda 4 testes e tem uma única falha (`rc=1`). A falha é na asserção da primeira diferença de espaço, que recebe `(0, 1)` quando exige `(1, 1)`. A fixture chegou à chamada do contador. Os hashes dos conteúdos locais conferem com o diff fornecido. Os HEADs, as execuções e o verde da árvore nova sem mutante ficam **[nao verificado]** de forma independente. [vigia_texto_resposta.py:16](/home/ronald/codex_ws/6978b323441b9f7d1231eb19b7fcf4b415434386/app/chamados/services/vigia_texto_resposta.py:16), [test_e4c_f5_contador_texto.py:43](/home/ronald/codex_ws/6978b323441b9f7d1231eb19b7fcf4b415434386/app/chamados/tests/test_e4c_f5_contador_texto.py:43)

2. **BLOQUEIA a prova de comparação literal.** O caso novo chama diretamente o mesmo `contar()` que o vigia usa. Grava `ato` na decisão e exige uma divergência ao trocar somente a projeção para `ato ` e depois para ` ato`. Pela leitura dos casos, `casefold()` bilateral sobreviveria e esconderia `ato` × `ATO`. É plausível hoje porque o contador compara dois textos armazenados e a porta humana preserva a caixa. Pelo mesmo desenho da fixture, `strip()` aplicado só ao lado da leitura também sobreviveria, pois o texto do ato já vem sem espaços nas pontas. A execução desses dois mutantes e a ocorrência dessas divergências em produção ficam **[nao verificado]**. `None` × `''` já é tratado como equivalente nos caminhos atuais, com campos não anuláveis e conversão para `''` no contador e na leitura. [test_e4c_f5_contador_texto.py:21](/home/ronald/codex_ws/6978b323441b9f7d1231eb19b7fcf4b415434386/app/chamados/tests/test_e4c_f5_contador_texto.py:21), [test_e4c_f5_contador_texto.py:40](/home/ronald/codex_ws/6978b323441b9f7d1231eb19b7fcf4b415434386/app/chamados/tests/test_e4c_f5_contador_texto.py:40), [vigiar_fio_morto.py:194](/home/ronald/codex_ws/6978b323441b9f7d1231eb19b7fcf4b415434386/app/chamados/management/commands/vigiar_fio_morto.py:194), [disputa_emissao.py:1446](/home/ronald/codex_ws/6978b323441b9f7d1231eb19b7fcf4b415434386/app/chamados/services/disputa_emissao.py:1446), [models.py:2634](/home/ronald/codex_ws/6978b323441b9f7d1231eb19b7fcf4b415434386/app/chamados/models.py:2634), [models.py:4170](/home/ronald/codex_ws/6978b323441b9f7d1231eb19b7fcf4b415434386/app/chamados/models.py:4170), [models.py:4310](/home/ronald/codex_ws/6978b323441b9f7d1231eb19b7fcf4b415434386/app/chamados/models.py:4310)

3. **NÃO BLOQUEIA.** Os conjuntos vazios respeitam o tipo devolvido por `creditos_perguntas` e eliminam o `TypeError` específico nos testes de pertinência `ch.id in _resp` e `ch.id in _viva`. Isso permite chegar a `desenlace` no caminho padrão do fio, com os mocks do teste e `com_explicacao=False`. A asserção continua exigindo o mesmo total de consultas com uma e cinco perguntas e um mínimo de chamadas a `leitura` em cada cenário. O total absoluto pode subir porque o caminho agora alcança `_orf(ch)`, que pode consultar o banco. O teste descarta o retorno e, portanto, uma nova exceção capturada no bloco ainda poderia passar despercebida. O verde do teste de custo e os totais antes e depois ficam **[nao verificado]**, pois as execuções fornecidas cobrem o módulo do contador. [raiox.py:149](/home/ronald/codex_ws/6978b323441b9f7d1231eb19b7fcf4b415434386/app/chamados/services/raiox.py:149), [fio.py:195](/home/ronald/codex_ws/6978b323441b9f7d1231eb19b7fcf4b415434386/app/chamados/services/fio.py:195), [fio.py:454](/home/ronald/codex_ws/6978b323441b9f7d1231eb19b7fcf4b415434386/app/chamados/services/fio.py:454), [disputa_emissao.py:776](/home/ronald/codex_ws/6978b323441b9f7d1231eb19b7fcf4b415434386/app/chamados/services/disputa_emissao.py:776), [test_e4c_f5_volta1_leem_o_ato.py:88](/home/ronald/codex_ws/6978b323441b9f7d1231eb19b7fcf4b415434386/app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:88), [test_e4c_f5_volta1_leem_o_ato.py:139](/home/ronald/codex_ws/6978b323441b9f7d1231eb19b7fcf4b415434386/app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:139)

4. **BLOQUEIA pela lacuna de RED do item 2.** O RED tem mordida para o `strip()` bilateral e deixa passar, por inspeção, normalizações que quebram a comparação literal. Os dois hunks se limitam ao caso de teste do contador e ao tipo de retorno do mock de créditos. Neles não aparece segundo juiz, segundo escritor fora da porta, relógio fora da borda, derivação paralela, alteração de ordem de trava ou campo com dois significados. [test_e4c_f5_contador_texto.py:40](/home/ronald/codex_ws/6978b323441b9f7d1231eb19b7fcf4b415434386/app/chamados/tests/test_e4c_f5_contador_texto.py:40), [test_e4c_f5_volta1_leem_o_ato.py:146](/home/ronald/codex_ws/6978b323441b9f7d1231eb19b7fcf4b415434386/app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:146)

## 19/11 14:49 · sessao a66bbb7c88ef · CADEIA-20-PASSO-1 · leitura · origem=Code

1. **Quem cria as colunas e acomoda a batida**

O caminho começa em `TipoEscala.save()`: sem par completo de intervalo, inclusive em `horarios_por_dia`, ele define `intervalo_modo='duracao'`. `_intervalo_livre()` verifica esse modo — não verifica diretamente a ausência de horários. ([models.py:790](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/models.py:790), [models.py:243](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/models.py:243))

Dentro de `montar_grade_prevista_periodo_por_turno`, a sequência é:

- `_intervalo_livre()` ativa `_tem_dur`; o builder insere **dois slots** `('I', '·S')` e `('I', '·E')` na régua. ([utils.py:1392](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:1392), [utils.py:1411](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:1411))
- Sem `_hii/_hfi`, recolhe as batidas `is_intervalo=True`, ainda não consumidas, entre os instantes inicial e final do turno; com seus horários, fabrica marcos internos alternando `S/E`. **Esse ramo não exige um par completo de batidas.** ([utils.py:1662](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:1662), [utils.py:1720](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:1720))
- Chama `_match_marcos(..., turno=None, derivar=False)`, que usa `_alinhar`; depois carimba as células internas com `papel='I'`, `intervalo=True` e ocupa os slots disponíveis. Isso explica uma única `S 01:30` ocupar o primeiro `I`. ([utils.py:1724](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:1724), [utils.py:646](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:646), [utils.py:1749](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:1749))

Existe ainda um segundo caminho: uma flag **não consumida** pode ocupar um slot `I` vazio na conservação final; essa passagem procura a linha pela **data civil da batida**. ([utils.py:1843](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:1843))

**Commit de introdução: [não verificado].** A cópia não contém histórico Git utilizável; `git log` e `git blame` retornaram “not a git repository”. O comentário “28/06” documenta uma atribuição da regra, mas não comprova seu commit. ([models.py:776](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/models.py:776))

2. **Diurno 08–17 com `S 12:00 is_intervalo=True`**

**Vai para a coluna `I`, pelo mesmo caminho**, sob as mesmas condições de cadastro e dia de trabalho. `12:00` está entre as pontas, entra em `_fdt`, gera o marco interno e recebe `papel='I'`; não há teste de “diurno versus noturno” nessa decisão. ([utils.py:1662](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:1662), [utils.py:1720](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:1720), [utils.py:1752](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:1752))

A meia-noite muda os **instantes absolutos**: `encadear_marcos` acrescenta um dia quando o próximo horário não supera o anterior. Assim, `01:30 de d+1` fica dentro de `[22:00 de d, 06:00 de d+1]`; o predicado de inclusão continua igual. ([utils.py:344](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:344), [utils.py:1666](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:1666))

3. **Quando uma flag vira órfã**

Há três caminhos relevantes:

- O matcher deixa a batida sem marco e a marca como consumida; a conservação final então a ignora, mesmo havendo `I` vazio. ([utils.py:678](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:678), [utils.py:1845](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:1845))
- Uma célula interna casada excede a quantidade de slots `I`. ([utils.py:1753](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:1753))
- Uma flag chega não consumida à conservação final, mas sua linha civil não tem slot `I` livre. ([utils.py:1850](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:1850))

**Caso mínimo mantendo as duas pontas, sem depender de empates: três batidas** — `E d 22:00`, `S d+1 06:00`, `S d+1 06:30 is_intervalo=True`. A última sobrevive ao dedupe de dez minutos, fica fora do turno previsto — portanto não fabrica marco interno —, mas dentro do envelope do matcher, que vai até `07:30` com `tol_min=90`. Como as pontas já estão ocupadas exatamente, `06:30` sobra e fica consumida. ([utils.py:1355](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:1355), [utils.py:1666](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:1666), [utils.py:638](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:638), [utils.py:678](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:678))

**Se a sobra precisa estar dentro de 22–06**, um caso por saturação é: `E22`, `S00:30(flag)`, `E01:00(flag)`, `S01:30(flag)`, `S06`. As três internas geram três marcos; somente duas cabem nos slots `I`, então `01:30` vira órfã. São cinco batidas para esse mecanismo, preservando as pontas. ([utils.py:1411](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:1411), [utils.py:1721](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:1721), [utils.py:1758](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:1758))

Nos dois exemplos, a sobra produzida dentro do processamento do turno é guardada no **dia da entrada `d`**, conservando o instante em `d+1`. ([utils.py:1727](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:1727), [utils.py:1784](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:1784))

4. **Quem consome essa decisão**

- **Cartório:** chama a grade e transforma o dia em ata. A ressalva decisiva é que, quando existem marcos reconhecidos no DNA, `lampadas_do_dia` percorre **as chaves do DNA**; o `I` sintético não vira automaticamente lâmpada. A ata copia a lista de órfãs resultante. ([cartorio.py:1287](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/ponto/services/cartorio.py:1287), [cartorio.py:660](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/ponto/services/cartorio.py:660), [cartorio.py:675](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/ponto/services/cartorio.py:675), [cartorio.py:764](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/ponto/services/cartorio.py:764))
- **Espelho:** normalmente lê a célula lavrada, reconstruindo as células pelas lâmpadas da ata. Também possui caminho pelo builder; o adaptador apresenta suas células e órfãs. ([espelho.py:533](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/ponto/services/espelho.py:533), [leitor_celula.py:114](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/services/leitor_celula.py:114), [espelho.py:503](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/ponto/services/espelho.py:503), [utils.py:1131](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/escala/utils.py:1131))
- **Motor:** recebe, quando há células alimentadas, o mapa de papéis derivado da ata e o passa ao pareador. O papel `Xi` exige duas luzes de pausa; não decorre simplesmente de uma célula bruta com `papel='I'`. ([motor_calculo_v2.py:494](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/ponto/motor_calculo_v2.py:494), [turnos.py:1567](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/ponto/turnos.py:1567), [turnos.py:1827](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/ponto/turnos.py:1827))
- **`detectar_ausencias`:** consulta a entrada pela autoridade de turnos, o veto pela célula e, para fechar chamado existente, as lâmpadas dos marcos cobrados. São leituras indiretas; esses caminhos não consultam o `papel='I'` bruto da grade. ([detectar_ausencias.py:336](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/ponto/management/commands/detectar_ausencias.py:336), [detectar_ausencias.py:631](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/ponto/management/commands/detectar_ausencias.py:631), [detectar_ausencias.py:404](/home/ronald/codex_ws/e632496263f023f8b63c0247ac3ebbeaa0245bd1/app/ponto/management/commands/detectar_ausencias.py:404))

## 19/11 14:49 · sessao f9c895e56fad · - · - · origem=Ronald (tmux)

ok

## 19/11 14:48 · sessao 989197c67577 · - · - · origem=Ronald (tmux)

Para Q3, **“5 batidas é o mínimo” só vale para excesso de slots internos, mantendo as duas pontas e evitando colisões de instantes**. Há casos menores. (app/escala/utils.py:1664; app/escala/utils.py:1735; app/escala/utils.py:1753)

- **Excesso interno, caso claro:** `E22(d), S01(flag,d+1), E02(flag,d+1), S03(flag,d+1), S06(d+1)`. Os três flags fabricam três marcos internos; os dois primeiros ocupam `I`, `03:00` excede os dois slots e vira órfã do dia `d`, com instante em `d+1`. Pontas reais impedem a reconciliação posterior. (app/escala/utils.py:1720; app/escala/utils.py:1752; app/escala/utils.py:1758; app/escala/utils.py:1784; app/escala/utils.py:2039)

- **Menor, preservando pontas, mas fora do intervalo estrito do turno:** `E22(d), S06:00(d+1), S06:30(flag,d+1)`. O dedupe mantém a última S porque são 30 minutos de diferença; `06:30` não fabrica marco porque está fora de `[22,06]`, mas entra no envelope até `07:30`; o matcher a declara órfã e consumida. A conservação final pula batidas consumidas, mesmo com slots `I` livres. Portanto ela fica órfã em `d`. (app/escala/utils.py:1355; app/escala/utils.py:1666; app/escala/utils.py:641; app/escala/utils.py:678; app/escala/utils.py:680; app/escala/utils.py:1845; app/escala/utils.py:1784)

- **Colisão interna, mantendo pontas:** `E22, S01(flag), E01(sem flag), S06`, com as duas batidas de `01:00` no mesmo instante e nessa ordem na entrada. Tipos diferentes sobrevivem ao dedupe; existe só um marco sintético `01:00`. No empate, o DP atribui o marco à segunda batida e deixa o flag órfão. São quatro batidas. (app/escala/utils.py:1344; app/escala/utils.py:1355; app/escala/utils.py:229; app/escala/utils.py:237; app/escala/utils.py:1664; app/escala/utils.py:1721; app/escala/utils.py:678)

- **Sem exigir pontas reais**, apenas `S01(flag), E01(sem flag)` no mesmo instante e nessa ordem já deixa o flag órfão no matcher; o cluster remove sua atribuição distante à ponta, e `consumed` impede resgate posterior. Não recomendo chamar esse caso de mínimo universal sem delimitar as premissas. (app/escala/utils.py:646; app/escala/utils.py:659; app/escala/utils.py:663; app/escala/utils.py:665; app/escala/utils.py:680; app/escala/utils.py:1845)

Simulei `_alinhar` e `_match_marcos` extraídos por AST em memória, com stubs de construção das células e da guarda usando os valores atuais `90`/`15`; confirmei os resultados locais acima. Execução integral do builder/Django: **[não verificado]**. (app/escala/regua_defesa.py:395; app/escala/orfa_classe.py:3)

## 19/11 14:46 · sessao ae4ce0df198f · - · - · origem=Ronald (tmux)

Para a pergunta 4, os leitores não consomem todos diretamente o dicionário `papel='I'`:

- **Cartório:** monta a grade em `app/ponto/services/cartorio.py:1287`, conta células preenchidas mais órfãs em `app/ponto/services/cartorio.py:1537` e gera a ata em `app/ponto/services/cartorio.py:1549`. A ata copia `orfas` em `app/ponto/services/cartorio.py:764`.
- **Ressalva decisiva:** quando há DNA com marcos reconhecidos, as lâmpadas vêm somente das chaves declaradas do DNA (`app/ponto/services/cartorio.py:639`, `app/ponto/services/cartorio.py:660`, `app/ponto/services/cartorio.py:675`). Assim, uma coluna I sintética não se torna automaticamente lâmpada de intervalo; somente o ramo alternativo percorre todas as células (`app/ponto/services/cartorio.py:735`).
- **Espelho:** normalmente lê a célula lavrada, por `grade_da_celula` (`app/ponto/services/espelho.py:533`); o leitor reconstrói as células a partir das lâmpadas (`app/escala/services/leitor_celula.py:114`, `app/escala/services/leitor_celula.py:129`). O builder é caminho de degradação (`app/ponto/services/espelho.py:503`, `app/ponto/services/espelho.py:574`) e chega à montagem da grade em `app/escala/utils.py:2384`. A apresentação recebe realizado e órfãs em `app/ponto/services/espelho.py:703`, preservados pelo adaptador em `app/escala/utils.py:1128`, `app/escala/utils.py:1131`.
- **Motor:** recebe o papel derivado da ata quando há células alimentadas (`app/ponto/motor_calculo_v2.py:494`, `app/ponto/motor_calculo_v2.py:500`) e passa esse mapa ao pareador (`app/ponto/motor_calculo_v2.py:503`). Não lê diretamente o `papel='I'` da grade; o mapa distingue `X` e `Xi`, e `Xi` exige pelo menos duas luzes de pausa (`app/ponto/turnos.py:1567`, `app/ponto/turnos.py:1827`).
- **detectar_ausencias:** consulta a entrada pela autoridade de turnos (`app/ponto/management/commands/detectar_ausencias.py:336`), cujo pareador recebe o papel da ata (`app/ponto/turnos.py:1997`). Antes da emissão, consulta o veto da célula (`app/ponto/management/commands/detectar_ausencias.py:631`), que lê `trabalha` e `veredito` (`app/ponto/marcos.py:155`). Para fechar chamado, consulta as lâmpadas dos marcos cobrados (`app/ponto/management/commands/detectar_ausencias.py:404`, `app/ponto/services/lastro.py:193`, `app/ponto/services/lastro.py:175`).

Não há, no dado fornecido, comprovação de impacto financeiro nem de comportamento adicional em produção. [não verificado]

## 19/11 14:46 · sessao 6f8405a34ae5 · - · - · origem=Ronald (tmux)

O commit inicial permanece **[não verificado]**: `git log`, `git blame` e `git status` falham com `fatal: not a git repository`; esta cópia não contém `.git`. Não há como fornecer o `log -S`/blame solicitado apenas com a árvore disponível. **[não verificado]** [nao verificado]

A cadeia atual que explica o comportamento é:

- `TipoEscala.save()` deriva `intervalo_modo='duracao'` na ausência de par completo no escalar e em `horarios_por_dia`: `app/escala/models.py:790–794`. `_intervalo_livre()` retorna exatamente esse modo: `app/escala/models.py:243–250`.
- O builder marca `_tem_dur=True` ao encontrar `_intervalo_livre()` e cria dois slots `('I', '·S')`, `('I', '·E')`: `app/escala/utils.py:1392–1411`.
- Sem `_hii` e `_hfi`, recolhe batidas não consumidas com `is_intervalo=True` dentro dos instantes absolutos do turno: `app/escala/utils.py:1662–1666`; fabrica marcos internos alternando `S/E`: `app/escala/utils.py:1720–1722`.
- Após o matcher, transforma as células internas fora da régua em `papel='I'`, `intervalo=True` e ocupa os slots; excedentes tornam-se órfãs: `app/escala/utils.py:1749–1759`.
- Conservação final também tenta inserir flag não consumida em slot I vazio, mas usa a **data civil da batida**; sem slot livre, torna-a órfã: `app/escala/utils.py:1847–1861`.

Há um teste que documenta explicitamente o contrato de absorção: “sem hii/hfi” + flag deve aparecer na grade e não virar órfã, com `E19/S02(flag)/E03(flag)/S07`: `app/escala/tests/test_split_adeilton.py:81–106`.

Comentários atribuem a regra de derivar duração a “28/06”, mas isso não comprova introdução em commit: `app/escala/models.py:776–780`. O documento cita `5f915ff4`, de 15/10, para um refinamento posterior do ramo **sem flag** — exclusão de sobras próximas das pontas — portanto não responde à origem dos slots e da absorção por flag: `app/docs/TICKETS.md:136`.

## 19/11 14:43 · sessao 85d51754a8da · REPLAY-F5-SOL · verificador · origem=Code

Referências aos anexos usam a numeração do texto fornecido, começando na primeira linha de cada anexo.

1. **NÃO BLOQUEIA — fechado o bloqueio específico do `strip()`.** Os dois logs mostram a mesma mutação aplicada: comparação após `strip()` nos dois lados. O velho passa três testes, `rc=0`; o novo falha entre quatro testes, `rc=1`. (`runs_v4.txt:30`, `runs_v4.txt:62`, `runs_v4.txt:76`, `runs_v4.txt:87`) [nao verificado]

   A falha nova é a assertiva de negócio: recebeu `(0, 1)` quando esperava `(1, 1)`, depois de trocar a projeção para `'ato '`. A fixture chegou à comparação; não foi erro de montagem. O segundo cenário, `' ato'`, existe, mas esse log não chega até ele porque a primeira assertiva interrompe o teste. (`runs_v4.txt:39`, `runs_v4.txt:43`, `app/chamados/tests/test_e4c_f5_contador_texto.py:45`, `app/chamados/tests/test_e4c_f5_contador_texto.py:47`)

2. **BLOQUEIA — permanece uma lacuna na prova de comparação literal.** O caso novo exige que espaço inicial e final sejam significativos para o contador vivo, cuja comparação continua direta. Ele não prova sensibilidade a todas as outras diferenças textuais. (`app/chamados/tests/test_e4c_f5_contador_texto.py:40`, `app/chamados/services/vigia_texto_resposta.py:16`)

   **Mutante plausível:** aplicar `casefold()` aos dois operandos. Por leitura, sobreviveria aos casos desse módulo: preserva as diferenças de espaço e mantém `'ato'` diferente de `'projecao'`; nenhum cenário confronta `'ato'` com `'ATO'`. É plausível como normalização indevida, pois os campos aceitam texto sem restrição de caixa. Sobrevivência executada desse mutante: **[não verificado]**. (`app/chamados/tests/test_e4c_f5_contador_texto.py:23`, `app/chamados/tests/test_e4c_f5_contador_texto.py:31`, `app/chamados/tests/test_e4c_f5_contador_texto.py:45`, `app/chamados/models.py:2634`, `app/chamados/models.py:4170`)

   Já “ignorar `None` versus `''`” não oferece contraexemplo plausível nesse caminho atual: o contador já normaliza a projeção com `or ''`, e `leitura()` produz texto vazio para ausência, normalizando também o texto dos atos. (`app/chamados/services/vigia_texto_resposta.py:16`, `app/chamados/models.py:4263`, `app/chamados/models.py:4310`, `app/chamados/models.py:4316`, `app/chamados/models.py:4325`)

3. **NÃO BLOQUEIA — a troca corrige a causa tipada identificável.** O consumidor faz `ch.id in _resp` e `ch.id in _viva`; inteiros provocam `TypeError`, conjuntos permitem essas operações e correspondem ao contrato real de retorno. (`app/chamados/services/fio.py:441`, `app/chamados/services/fio.py:454`, `app/chamados/services/raiox.py:149`, `app/chamados/services/raiox.py:158`, `app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:146`)

   Isso permite avançar pelo trecho antes interrompido, mas **não certifica o fio inteiro**: permanecem quatro patches, `com_explicacao` fica no padrão falso e o teste não exige desenlace válido nem ausência de exceção capturada. Os logs desta volta executam somente o módulo do contador; execução corrigida do teste de custo: **[não verificado]**. (`app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:143`, `app/chamados/services/fio.py:195`, `app/chamados/services/fio.py:340`, `app/chamados/services/fio.py:459`, `runs_v4.txt:7`, `runs_v4.txt:16`)

   A métrica permanece “mesmo número de consultas com uma e cinco perguntas”, acompanhada da quantidade mínima de chamadas a `leitura()`. O número absoluto pode mudar: agora pode alcançar `_orf(ch)`, cujo caminho contém consulta `.exists()`, antes interrompido pelo pertencimento em inteiro. Totais efetivos antes/depois: **[não verificado]**. (`app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:88`, `app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:147`, `app/chamados/services/fio.py:456`, `app/chamados/services/disputa_emissao.py:776`, `app/chamados/services/disputa_emissao.py:810`)

4. **BLOQUEIA — apenas pela cobertura residual de RED descrita no item 2.** O novo RED tem mordida demonstrada contra `strip()`, mas deixa descoberta a normalização por caixa; não cabe concluir fidelidade literal geral. (`runs_v4.txt:39`, `app/chamados/tests/test_e4c_f5_contador_texto.py:40`)

   No delta fornecido, não aparece segundo juiz, segundo escritor fora da porta, relógio fora da borda, derivação paralela, nova ordem de trava ou campo com dois significados: as alterações se limitam ao novo teste e ao retorno da fixture de créditos. Essa conclusão fica restrita ao delta. (`diff_v4.patch:1`, `diff_v4.patch:22`) [nao verificado]

## 19/11 14:35 · sessao 2d4833eae486 · E4C-F5 · verificador · origem=Code

**AVAL-DEVOLVIDO nos itens 1, 3a, 3b e 3c**, pelos motivos abaixo. “PROCEDE” significa procedência da objeção ao item. As evidências centrais são o corte posterior sobre texto, a prova de contagem já existente e a separação entre produto e instrumento. (`app/docs/RELATO.md:32`; `app/chamados/tests/test_e4c_f5_contador_texto.py:26-38`; `CLAUDE.md:327-328`.)

Os anexos raspados são a referência fornecida para esta revisão; sua correspondência com arquivos integrais locais, a identidade Git da cópia e os resultados dos logs citados ficam **[não verificado]**. As conclusões sobre código são por inspeção; execução de REDs e mutantes **[não verificado]**.

**1 — PROCEDE: “0815 INTEIRO como está” desconsidera uma decisão posterior registrada.**

1. **Premissas:** conferem as três combinações: app passa a pergunta projetada ao juiz, que lê R/T do argumento; a property passa a leitura do ato; o fio combina instante do ato com texto projetado. (`app/chamados/services/pergunta_no_app.py:83-86`; `app/chamados/juizes.py:1552`; `app/chamados/models.py:3791`; `app/chamados/services/fio.py:189-192`.)
2. **Colisão:** a cópia registra **RESPOSTAS-1911-1000, item 1(B)**: “texto segue da projecao; F5 migra so R/V”. Isso altera a migração de texto prevista no 0815/brief fornecidos; reafirmar aquele desenho integralmente deixa duas instruções incompatíveis. (`app/docs/RELATO.md:20,32`; `app/docs/PROMPTS.md:2615`.)
3. **Literal:** concluir as três voltas em um pouso atende à exigência do 0815 fornecido; o defeito é preservar “como está” um desenho cuja regra de texto foi posteriormente modificada. A própria ordem sofreu transferência documentada da tag para a volta 2. (`app/docs/RELATO.md:32,36-37`.)
4. **Competência/dinheiro/`!`:** a volta 3 alcança escrita e recálculo financeiro por portas existentes; isso já está declarado no 0815 fornecido. Não há prova de movimento proibido efetivo **[não verificado]**. A guarda encontrada não demonstra proteção universal: K6 seleciona metades de intervalo e pula os demais tipos. (`app/chamados/services/validacao.py:743,753-755`; `app/chamados/services/materializacao.py:804-820`; `CLAUDE.md:265-268`.)

**2 — NÃO PROCEDE: a certificação precisa acompanhar a árvore final.**

1. **Premissas:** `Ran 17040 OK`, o conteúdo do merge e a árvore certificada por `suite_m` são **[não verificado]**.
2. **Colisão:** tratar a suíte anterior como sinal é compatível com “A arvore que a suite certificou e a que POUSA”; a certificação não acompanha alterações posteriores de Python. (`CLAUDE.md:320-324`.) [nao verificado]
3. **Literal:** pedir suíte e opus sobre as três voltas não reduz o PRONTO fornecido. A execução e a ordem efetivamente observadas permanecem **[não verificado]**. [nao verificado]
4. **Competência/dinheiro/`!`:** a exigência trata de certificação; não autoriza qualquer exceção às fronteiras de produção. (`CLAUDE.md:265-268,320-328`.)

**3a — PROCEDE: autoridade e prazo não fecham com as instruções preservadas.**

1. **Premissas:** as três fontes conferem, inclusive a mistura explícita do fio. (`app/chamados/services/pergunta_no_app.py:86`; `app/chamados/juizes.py:1552`; `app/chamados/models.py:3791`; `app/chamados/services/fio.py:192`.)
2. **Colisão:** “mesma autoridade” não determina qual leitura prevalece diante do corte **RESPOSTAS-1911-1000 item 1(B)**. Interpretá-la como R/T integralmente do ato contraria “F5 migra so R/V” e o teste que exige texto projetado no fio. (`app/docs/RELATO.md:32`; `app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:58-65`.)
3. **Literal:** mudar somente os dois juízes da volta 2 não unifica os três sítios: o fio chama `respondida_sem_texto` com uma leitura híbrida; a property já entrega R/T da leitura do ato. Portanto, “ao fim da volta 2”, junto da manutenção integral daquela divisão de voltas, exige alterações adicionais fora dos dois juízes. O encaixe dessas alterações não está definido no literal fornecido **[não verificado]**. (`app/chamados/services/fio.py:187-192`; `app/chamados/models.py:3791`; `app/chamados/juizes.py:1389,1552`.)
4. **Competência/dinheiro/`!`:** o juiz temporal alimenta vitalidade, e o juiz de resposta também recebe objetos já projetados. Seus efeitos não se limitam aos três exemplos. Movimento real de entregue/exportada **[não verificado]**; não há autorização para tocar seu gravado. (`app/chamados/juizes.py:1404,1423`; `CLAUDE.md:265-268`.)

**3b — PROCEDE: a carga confere; igualdade de contagem não evidencia o RED pedido.**

1. **Premissas:** `contar()` seleciona todas as perguntas por padrão, aplica prefetch e percorre o queryset sem iterador/lote. O vigia chama essa função dentro de `tranca_em_lote`. **51.563 é [não verificado]**, embora conste do comentário. (`app/chamados/services/vigia_texto_resposta.py:3,8-18`; `app/chamados/management/commands/vigiar_fio_morto.py:186-195`.)
2. **Colisão:** preservar os resultados enquanto se altera a carga é compatível com o contrato existente de comparação entre ato e texto corrente. (`app/chamados/services/vigia_texto_resposta.py:1-4,16-18`.)
3. **Literal:** “RED de contagem igual antes e depois”, isoladamente, não demonstra a falha de carregamento: a implementação atual já satisfaz testes de contagens, recortes e múltiplos atos. O teste de custo existente mede consultas, não carregamento integral em memória. (`app/chamados/tests/test_e4c_f5_contador_texto.py:26-38,67-80`.)
4. **Competência/dinheiro/`!`:** o corpo dessa função consulta, compara e conta; não contém escrita desses domínios nem chamada ao motor. (`app/chamados/services/vigia_texto_resposta.py:8-18`.)

**3c — PROCEDE: a alternativa “leitor no ESTADO” colide com o pouso único.** [nao verificado]

1. **Premissas:** o universo é gravado em chave própria, fora de `censo/esperado`; o ESTADO compara esses dois mapas e não considera essa chave. O teste lê o universo. Assim, zero nesse universo não gera, por esse campo, alerta próprio de cegueira. (`app/chamados/management/commands/vigiar_fio_morto.py:275-287`; `bin/fio_estado.py:121-126,191-197`; `app/chamados/tests/test_e4c_f5_contador_texto.py:65`.)
2. **Colisão:** a frase vigente é **“INSTRUMENTO (…) nao pousa com PRODUTO: pouso proprio, depois”**. O próprio leitor do ESTADO declara esse enquadramento. Prender sua alteração à mesma obra e ao único pouso da F5 conflita com essa regra. (`CLAUDE.md:327-328`; `bin/fio_estado.py:4-7`.)
3. **Literal:** a alternativa de retirada não apresenta essa colisão de pouso. Contudo, a L-111 citada trata literalmente de função sem chamador de produção; sua aplicação a uma chave de dados não é literal. (`app/docs/LEIS.md:137`; `app/chamados/management/commands/vigiar_fio_morto.py:287`.)
4. **Competência/dinheiro/`!`:** retirar a chave não equivale a apagar um arquivo usado por produção. Não há exigência automática de `!` por essa retirada; apagar o arquivo inteiro seria outra ação. (`CLAUDE.md:265-268`; `app/chamados/management/commands/vigiar_fio_morto.py:287`.)

**3d — NÃO PROCEDE: o reforço pedido corresponde à fragilidade encontrada.**

1. **Premissas:** o selo **já usa AST**, mas reconhece atributos e chamadas pelo identificador literal `p`. Nesta cópia, seu trecho relevante está em **153–178**, e não integralmente em 146–176. (`app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:146-178`.)
2. **Colisão:** eliminar a dependência do nome da variável preserva a finalidade do selo. Seu limite vigente é R/V; T está expressamente excluído pela lei B. (`app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:154-167`.)
3. **Literal:** a condição de apelido é pertinente: as comparações com `'p'` deixam esse reconhecimento escapar. A sobrevivência de mutante executado é **[não verificado]**. Além disso, `respondida_sem_texto(p)` não tem argumentos posteriores para a inspeção feita pelo selo. (`app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:165-178`.)
4. **Competência/dinheiro/`!`:** o sítio declarado é código de teste, sem escrita de produção nesse trecho. (`app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:153-178`.)

**3e — NÃO PROCEDE: a triagem não determina mudança de regra.**

1. **Premissas:** a comparação do contador é literal; há teste para espaços. Os dois `.lower()` citados atuam em leituras para classificação. A afirmação global de que nenhum escritor indireto transforma caixa permanece **[não verificado]**. (`app/chamados/services/vigia_texto_resposta.py:16`; `app/chamados/tests/test_e4c_f5_contador_texto.py:40-48`; `app/chamados/juizes.py:2041-2047`; `app/chamados/services/validacao.py:1181-1193`.)
2. **Colisão:** triagem motivada não contraria o contrato literal existente. (`app/chamados/services/vigia_texto_resposta.py:16`.)
3. **Literal:** o teste desse módulo não contém o confronto `'Ato'`/`'ato'`; resultado do mutante correspondente **[não verificado]**. (`app/chamados/tests/test_e4c_f5_contador_texto.py:20-80`.)
4. **Competência/dinheiro/`!`:** a exigência fornecida é documental; qualquer alteração futura de regra permanece **[não verificado]** e sujeita ao escopo literal. (`CLAUDE.md:265-268`.)

**4 — NÃO PROCEDE: a exigência de prova é compatível com o portão.**

1. **Premissas:** fechamento do PRONTO, medidas e smoke da árvore inteira são **[não verificado]**.
2. **Colisão:** a linha exigida reproduz o PRONTO do 0815 fornecido; não demonstra redução dele. A regra local também exige passagem de etapa com a porta anterior provada no RELATO. (`CLAUDE.md:282-285`.)
3. **Literal:** “item sem prova = não pede suíte” é uma condição verificável; não equivale a afirmar que a condição já esteja cumprida **[não verificado]**. [nao verificado]
4. **Competência/dinheiro/`!`:** publicar o registro não autoriza movimentos adicionais; permanecem as restrições de competência, dados e motor. (`CLAUDE.md:265-268,285-287`.)

**5 — NÃO PROCEDE como objeção demonstrada; a remissão ao item d tem alcance limitado.**

1. **Premissas:** vagas atuais, estado de FERIADO/C7/B2b e texto integral do aval 1335 são **[não verificado]**.
2. **Colisão:** prioridade de fila 1 encontra respaldo local. O PRE-APROVADO **d** exige agente esperando job próprio, job destacado e necessidade da vaga por obra de fila 1. (`CLAUDE.md:288-294`.)
3. **Literal:** esse d não constitui autorização geral para cancelar qualquer suíte ou opus ativo. A ocorrência concreta de uma ação incompatível é **[não verificado]**. (`CLAUDE.md:289-294`.) [nao verificado]
4. **Competência/dinheiro/`!`:** prioridade e liberação de vaga não suspendem as ações reservadas ao `!`. (`CLAUDE.md:265-268,294-295`.)

**6 — NÃO PROCEDE: preservar trabalho na cópia não é pousar parcialmente.**

1. **Premissas:** os commits efetivamente existentes e a linha de encerramento do dia são **[não verificado]**. [nao verificado]
2. **Colisão:** o requisito fornecido de commit por volta e ausência de pouso parcial mantém a separação entre construir na cópia e integrar/publicar. (`CLAUDE.md:309-313,325-328`.) [nao verificado]
3. **Literal:** o encerramento do dia não prova PRONTO nem substitui certificação da árvore final; a suíte certifica a árvore que pousa. (`CLAUDE.md:320-324`.) [nao verificado]
4. **Competência/dinheiro/`!`:** commit na cópia não autoriza alterar gravado exportado, dados de escala/vínculo, motor ou apagar arquivo de produção. (`CLAUDE.md:265-268`.) [nao verificado]

## 19/11 14:34 · sessao aae86a382a75 · - · - · origem=Ronald (tmux)

**Competência/dinheiro/`!`:**

- O item 1 inclui portas que podem mover dinheiro por efeito: `validar_pergunta` carimba e chama recalculo por evento (`app/chamados/services/validacao.py:743`, `:753`); o evento chama `recalcular_fechamento_mes` (`app/ponto/services/fechamento.py:1809`) e agenda por `on_commit` (`:1824`). Isso já está declarado nas medidas e restrições do 0815 fornecido; movimento real de produção **[não verificado]**.
- A presença de K6 não prova cerca universal de entregue/exportada: a composição usa `_k6_que_barra` (`app/chamados/services/validacao.py:225`), que consulta só metades de intervalo (`app/chamados/services/materializacao.py:804`, `:808`) e pula as outras (`:819`). Com `somente_entregue=True`, o restante retorna sem barreira (`:841`). Casos concretos que mudariam **[não verificado]**.
- A correção escreve resposta antes de chamar validação (`app/chamados/services/acoes_disputa.py:375`, `:383`); a recusa grava motivo e reabre a resposta (`app/chamados/services/validacao.py:922`, `:925`); a devolução limpa V/VP e registra ato (`:273`, `:276`, `:279`). Não presumir que a guarda da validação cobre cada chamada direta.
- `!` exige tocar gravado exportado, dado de escala/vínculo, apagar/voltar arquivo usado em produção, regra fora do pedido, atalho ou `motor_calculo_v2.py` (`CLAUDE.md:265`). O aval 1435 não determina esses atos fora do escopo; execução futura **[não verificado]**.
- Retirar contador/campo não equivale a apagar arquivo de produção: a exigência literal é apagar/voltar **arquivo** (`CLAUDE.md:266`).

**3c: colisão documental confirmada.**

O leitor do ESTADO é instrumento declarado no próprio arquivo: `bin/fio_estado.py:4` a `:7` diz que o produto pousou antes e que esse leitor pousa “em commit proprio, depois do produto”. A regra vigente exige instrumento separado de produto (`CLAUDE.md:327`, `app/docs/LEIS.md:131`). Portanto, a alternativa “ganha leitor no ESTADO”, presa à F5 inteira em um pouso, tem colisão literal; não é automaticamente item de `!` (`CLAUDE.md:265`).

**Aval adicional citado pelo laudo:**

Há registro local de RESPOSTAS-1911-1000 item 1(B): texto segue da projeção, F5 migra somente R/V e nasce contador de divergência (`app/docs/RELATO.md:32`, `app/docs/PROMPTS.md:2615`). O RELATO registra explicitamente instante do ato e texto da projeção (`app/docs/RELATO.md:20`). “Uma fonte” não pode ser interpretada automaticamente como migrar T para ato; o literal integral desse aval **[não verificado]**.

**5: vagas.**

PRE-APROVADO(d) exige job destacado **e** obra de fila 1 precisando da vaga (`CLAUDE.md:292`); ordem geral é fila 1, negócio, instrumento (`:291`). O 1335 citado e o estado atual de B2b/FERIADO/C7 **[não verificado]**.

## 19/11 14:34 · sessao 9e8384c60c96 · - · - · origem=Ronald (tmux)

Itens auditados, sem execução de testes nem acesso a banco/log: [nao verificado]

**1 — PROCEDE quanto à compatibilidade literal, com premissa técnica confirmada.**

- As três combinações existem: app passa a pergunta crua ao juiz (`app/chamados/services/pergunta_no_app.py:83-86`), cujo corpo lê R/T do argumento (`app/chamados/juizes.py:1552`); property passa `DecisaoPergunta.leitura(self)` (`app/chamados/models.py:3791`); fio combina R da leitura com T da projeção (`app/chamados/services/fio.py:189-192`).
- Há aval posterior ao 0815 registrado na cópia: **RESPOSTAS-1911-1000 item 1(B)** determina “texto segue da projecao; F5 migra so R/V” (`app/docs/RELATO.md:32`); a linha explicativa registra devolução do texto à projeção e manutenção do instante no ato (`app/docs/RELATO.md:20`). Assim, reafirmar 0815 “INTEIRO como está”, sem tratar esse corte posterior, deixa conflito documental relevante (`app/docs/RELATO.md:20`, `app/docs/RELATO.md:32`).
- A cópia documenta tag retirada da volta 1 e transferida à volta 2, embora a divisão original a tenha colocado na volta 1 (`app/docs/RELATO.md:36-37`). A tag ainda lê texto cru em `app/chamados/templatetags/chamados_tags.py:166`.
- Hash real, conteúdo dos commits e pouso: [não verificado]. A árvore não contém `.git` na raiz; não há confirmação Git independente [não verificado]. [nao verificado]
- Alcance financeiro/exportado deste item depende da continuação ainda não realizada [não verificado].

**3a — PROCEDE por conflito de autoridade não resolvido no literal.**

- A premissa das três combinações confere nos mesmos endereços acima (`app/chamados/services/pergunta_no_app.py:86`, `app/chamados/models.py:3791`, `app/chamados/services/fio.py:192`).
- “Mesma autoridade” não define literalmente qual par deve prevalecer diante do corte B: texto da projeção é exigência registrada do **RESPOSTAS-1911-1000 item 1(B)** (`app/docs/RELATO.md:20`, `app/docs/RELATO.md:32`), enquanto a testemunha `leitura` devolve R/T da sequência com passivo (`app/chamados/models.py:4295-4326`).
- Migrar o fio para R/T do ato contraria também a expectativa atual explícita do teste “Lei B”, que exige continuar esperando quando ato tem texto e projeção tem texto vazio (`app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:58-65`).
- A condição “fim da volta 2” não exige necessariamente antecipar portas da volta 3: os três sítios citados são partição, property e espera (`app/chamados/services/pergunta_no_app.py:80-90`, `app/chamados/models.py:3785-3791`, `app/chamados/services/fio.py:178-192`). Contudo, a ordem exata dos reparos ainda não foi implementada [não verificado].
- L-130 exige um juiz para todas as finalidades, mas não resolve sozinho a contradição específica entre ato e texto corrente (`app/docs/LEIS.md:151`).

**3d — NÃO PROCEDE como objeção à exigência do item; a premissa da fragilidade confere.**

- O selo já usa AST; seu intervalo real nesta cópia é **153–178**, enquanto 146–150 pertence ao teste de custo do fio (`app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:146-160`).
- Ele restringe reconhecimento ao identificador literal `p`, tanto em atributos quanto no primeiro argumento de chamadas (`app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:165-167`). Logo, um apelido escapa a essas condições por inspeção estática dessas linhas (`app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:165-178`).
- `respondida_sem_texto(p)` não contém argumento posterior a inspecionar e o selo não percorre o corpo do predicado chamado (`app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:168-178`); o predicado efetivamente lê R/T crus do argumento (`app/chamados/juizes.py:1531-1534`).
- O caso existente com R projetada ausente mantém T projetado preenchido; não é o caso R=None/T='' mencionado no laudo (`app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:42-49`).
- O item pede reforço do selo e mutante; resultado de mutante executado [não verificado]. Não há colisão documental demonstrada por esse reforço [não verificado].
- Atenção ao limite já estabelecido: selo atual varre apenas R/V e exclui T expressamente por lei B (`app/chamados/tests/test_e4c_f5_volta1_leem_o_ato.py:156`).

**3e — NÃO PROCEDE como objeção à triagem pedida; a comparação literal confere.**

- O contador compara texto por `!=`, sem normalização (`app/chamados/services/vigia_texto_resposta.py:16`).
- Há teste para diferenças apenas de espaços, mas nenhum caso `'Ato'`/`'ato'` nesse módulo de testes (`app/chamados/tests/test_e4c_f5_contador_texto.py:20-80`).
- Os dois `.lower()` citados pelo laudo são leituras para classificação, usando variável local `resp`, sem gravar os campos nesses trechos (`app/chamados/juizes.py:2041-2047`, `app/chamados/services/validacao.py:1181-1193`).
- Os escritores citados zeram ou copiam texto: `app/chamados/models.py:1575`, `app/chamados/models.py:1702-1703`, `app/chamados/services/disputa_emissao.py:1481`, `app/chamados/services/disputa_emissao.py:1617`, `app/chamados/services/disputa_emissao.py:1682-1686`, `app/chamados/services/validacao.py:440`. Exclusividade global de todos os escritores e ausência absoluta de transformação por caminho indireto [não verificado].
- “Triagem com motivo” não obriga mudar comparação nem criar regra de caixa; implementação futura e impacto fora do declarado [não verificado].

## 19/11 14:33 · sessao cb05fce58f6e · - · - · origem=Ronald (tmux)

**3b — PROCEDE, por condição de RED que não fecha literalmente.**

1. A carga integral confere: `contar()` seleciona todas as perguntas por padrão, aplica `prefetch_related('decisoes')` e percorre o queryset diretamente, sem iterador/lote (`app/chamados/services/vigia_texto_resposta.py:8-18`). A chamada fica dentro de `tranca_em_lote` (`app/chamados/management/commands/vigiar_fio_morto.py:186-195`), e há elo configurado na corrente cron (`app/config/crons.py:1737`). As **51.563 linhas são [não verificado]**: aparecem somente como declaração no comentário (`app/chamados/services/vigia_texto_resposta.py:3`).
2. Mudar a forma da carga preservando a comparação não colide, por si, com a lei registrada: ela estabelece texto da projeção, ato como testemunha (`app/chamados/services/vigia_texto_resposta.py:1-4`; `app/docs/RELATO.md:20`).
3. “RED de contagem igual antes e depois” mistura conservação com falha anterior: o código atual já retorna `(divergentes, universo)` e os testes existentes já exigem esses totais para conjuntos com/sem divergência, recorte e múltiplos atos (`app/chamados/services/vigia_texto_resposta.py:16-18`; `app/chamados/tests/test_e4c_f5_contador_texto.py:26-38`). A igualdade numérica, isolada, não demonstra o defeito de carga nem uma falha anterior; o teste de custo existente cobra quantidade de consultas, não pico de memória/execução em lotes (`app/chamados/tests/test_e4c_f5_contador_texto.py:67-80`). Não há RED novo executado nesta revisão [não verificado].
4. O corpo de `contar` só consulta e conta; não contém escrita em domínio/dinheiro/escala/vínculo nem acesso ao motor (`app/chamados/services/vigia_texto_resposta.py:8-18`). Ausência de efeitos globais de toda execução em produção [não verificado].

**3c — PROCEDE, por colisão da alternativa “leitor no ESTADO” com o pouso único.** [nao verificado]

1. A chave é escrita fora de `censo/esperado` (`app/chamados/management/commands/vigiar_fio_morto.py:269-287`). Busca textual integral encontrou somente esse escritor e o assert de teste (`app/chamados/tests/test_e4c_f5_contador_texto.py:65`); o leitor ESTADO compara apenas as chaves de `esperado` contra `censo` e não lê o universo textual (`bin/fio_estado.py:121-126`, `:143-162`). Portanto, zero divergências com universo textual zero não produz alerta por esse universo; isso é independente das outras condições de validade do carimbo (`bin/fio_estado.py:74-95`, `:125-141`, `:191-197`). **A referência `bin/fio_estado.py:66-67` do laudo não comprova o alegado: contém `TETO_H`, não o contador.**
2. A frase vigente é: **“INSTRUMENTO (`bin/`, selo de host, portao do push) nao pousa com PRODUTO: pouso proprio, depois”** (`CLAUDE.md:327-328`; também `app/docs/LEIS.md:131`). O próprio leitor declara que pousa como instrumento em commit próprio depois do produto (`bin/fio_estado.py:4-7`). Assim, exigir esse leitor dentro da MESMA obra, antes do opus final e sob UM pouso, não fecha sem exceção declarada; o 1435 diz “Nenhuma lei nova” e preserva o 0815 integralmente (anexo raspado; inexistência de arquivo local do aval [não verificado]).
3. A alternativa “sai no mesmo commit” tem fundamento apenas analógico na L-111: o literal da lei trata **função** sem chamador de produção, não qualquer chave informativa (`app/docs/LEIS.md:137`; `CLAUDE.md:191`). A chave aqui é um dado gravado e o total continua sendo impresso na saída (`app/chamados/management/commands/vigiar_fio_morto.py:228-229`, `:287`). Não afirmar que L-111 manda literalmente apagar este campo.
4. Apagar essa chave não equivale, no código apresentado, a apagar arquivo usado por produção; modificar o leitor host é instrumento (`app/chamados/management/commands/vigiar_fio_morto.py:287`; `bin/fio_estado.py:6-7`). O `!` exige apagar/voltar ao HEAD **arquivo** de produção, mexer no gravado exportado, dado de escala/vínculo ou motor (`CLAUDE.md:265-268`). Efeito futuro não declarado [não verificado].

**Evidências extras importantes para sua consolidação:**

- O aval **RESPOSTAS-1911-1000** não veio como anexo literal, mas a cópia registra seu conteúdo: **“F5 migra so R/V”** (`app/docs/PROMPTS.md:2615`), **“TEXTO da projecao … instante segue do ato”** (`app/docs/RELATO.md:20`). A unificação do texto no ato foi adiada para a E4D, quando morrer a projeção (`app/docs/BACKLOG.md:2021`). Isso importa especialmente para 3a: a escolha de autoridade não é livre nem resolvida somente por exigir “mesma autoridade”.
- O contador de divergência já tem leitor genérico pelo `esperado` (`app/chamados/management/commands/vigiar_fio_morto.py:108`, `:277`; `bin/fio_estado.py:125-126`, `:191-194`); a lacuna é especificamente seu **universo**, não ausência completa de leitor do contador.
- A linha de fila local exige contador com leitor ESTADO e `>0 = PERGUNTA no topo` e o atribui ao RESPOSTAS-1911-1000 item 1 (`app/docs/BACKLOG.md:2026`); não demonstra que universo zero já seja tratado.

## 19/11 14:08 · sessao 01b1b0d2b971 · CADEIA-20-PASSO-1 · construtor · origem=Code

Implementei as alterações de P1 a P5 na cópia. A execução dos testes e a morte dos mutantes permanecem **[não verificado]**; não rodei a suíte nem acessei banco. A análise sintática dos sete arquivos alterados passou com `ast.parse`. [escala/tests/test_cadeia20_mutantes.py:1] [nao verificado]

**Inventário**

- P1 e P5: a medida conta células desalinhadas por quantidade ou ordem de horas e entradas sem razão e sem falha nomeada. O booleano também exige zero células desalinhadas. [cartorio.py:1001](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cartorio.py:1001)
- P2: removi `data_turno` e `fuso` da assinatura. Antes e depois, os chamadores encontrados são [cartorio.py:991](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/ponto/services/cartorio.py:991), [test_cadeia20_coletor.py:15](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_cadeia20_coletor.py:15) e [test_razao_da_sobra.py:29](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_razao_da_sobra.py:29); a assinatura está em [orfa_classe.py:30](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/orfa_classe.py:30).
- P3: o teste captura a primeira lavra de cada braço, com objetos recarregados e rollback; compara `SELECT` por causa das gravações dependentes da diferença de estado. [test_cadeia20_lavra.py:207](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_cadeia20_lavra.py:207) [nao verificado]
- P4: acrescentei os casos de sobra noturna e saída cruzada, com a resposta esperada nos docstrings. Sua passagem e a necessidade eventual de cura adicional estão **[não verificado]**. [test_cadeia20_costura.py:110](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_cadeia20_costura.py:110)
- REDs de P1 e P5: [test_cadeia20_lavra.py:149](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_cadeia20_lavra.py:149). Mutantes novos m15 a m19: [test_cadeia20_mutantes.py:61](/home/ronald/wt-codex-cadeia-20-passo-1-112113/app/escala/tests/test_cadeia20_mutantes.py:61).

Para a sessão executar os REDs e o teto, com a configuração Django e banco de teste próprios da sessão **[não verificado]**: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test escala.tests.test_cadeia20_lavra.LavraDasSobrasTest.test_RED_medida_compensacao_e_ordem_por_celula
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test escala.tests.test_cadeia20_lavra.LavraDasSobrasTest.test_RED_medida_sem_razao_sem_falha
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test escala.tests.test_cadeia20_lavra.TetoConsultaLavraTest.test_mesmo_acrescimo_em_n_e_2n_dias
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test escala.tests.test_cadeia20_costura.CosturaTest.test_RED_sobra_depois_da_meia_noite_no_dia_do_turno
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test escala.tests.test_cadeia20_costura.CosturaTest.test_RED_saida_cruzada_remove_mesma_ocorrencia
```

Comandos individuais dos mutantes novos **[não verificado]**:

```bash
PYTHONDONTWRITEBYTECODE=1 CADEIA20_MUTANTE=m15_instante_saida_cruzada_nao_removido python3 manage.py test escala.tests.test_cadeia20_mutantes.MutantesTest.test_mutantes_mordem_no_assert_pretendido
PYTHONDONTWRITEBYTECODE=1 CADEIA20_MUTANTE=m16_ignora_desalinhamento python3 manage.py test escala.tests.test_cadeia20_mutantes.MutantesTest.test_mutantes_mordem_no_assert_pretendido
PYTHONDONTWRITEBYTECODE=1 CADEIA20_MUTANTE=m17_ignora_falta_de_falha python3 manage.py test escala.tests.test_cadeia20_mutantes.MutantesTest.test_mutantes_mordem_no_assert_pretendido
PYTHONDONTWRITEBYTECODE=1 CADEIA20_MUTANTE=m18_ignora_hora python3 manage.py test escala.tests.test_cadeia20_mutantes.MutantesTest.test_mutantes_mordem_no_assert_pretendido
PYTHONDONTWRITEBYTECODE=1 CADEIA20_MUTANTE=m19_ignora_desalinhamento_no_booleano python3 manage.py test escala.tests.test_cadeia20_mutantes.MutantesTest.test_mutantes_mordem_no_assert_pretendido
```

## 19/11 14:08 · sessao 35d98b4515e0 · E4C-F5 · censo · origem=Code

O censo abaixo classifica os **cinco achados numerados**. O item “Casefold” não entra na contagem porque não está numerado; ele foi explicitamente tratado em `verif4/r.txt`, item 2 [linha do anexo não verificada].

Os anexos foram fornecidos no texto da pergunta, sem numeração de linhas, e não foram localizados como arquivos na cópia. Por isso, identifico cada trecho pelo caminho e pelo item do anexo; a **linha física de cada citação dos anexos está [não verificada]**. A cópia examinada também não contém o contador nem o teste novo mencionados no opus, de modo que sua correspondência com a versão final permanece [não verificada]. [nao verificado]

Para distinguir **(a)** de **(d)**, uso **(a)** quando uma resposta viu parte do tema, mas não identificou o achado completo; uso **(d)** quando a pergunta cobria a lacuna específica e nenhuma resposta ou triagem fornecida a identificou.

1. **BLOQUEIA — classe (a), falha do Codex.** A pergunta de verificação incluía pré-requisito e pouso; a resposta deixou esses pontos sem verificação, embora a revisão anterior tivesse registrado as exigências das três voltas e das provas. O dado fornecido não mostra uma triagem que tenha descartado esse achado específico [não verificado]. [nao verificado]

   - Pergunta literal, `logs/ESPINHA-BRIEFS/F5/v1/verif2/q.txt`, item 6: “pouso antes do pre-requisito”; “Alguma presente no diff ou nos escritores/leitores dos campos que ele toca, mesmo fora do diff?”
   - Resposta literal, `logs/ESPINHA-BRIEFS/F5/v1/verif2/r.txt`, item 6: “Pré-requisitos e pouso real também são **[não verificado]**.”
   - Evidência de que o Codex já havia visto as condições, `logs/ESPINHA-BRIEFS/F5/c3/etapaA2/r.txt`, §7: “três voltas verdes na ordem 1/2/3”, “medidas (a), (b) e (c) no relato” e “smoke do app — badge e pendentes — nas duas cascas”.

   A ausência das medidas e do smoke no RELATO é uma conclusão do opus fornecido; sua conferência independente no RELATO está [não verificado].

2. **Três combinações de fonte — classe (a), falha do Codex.** O verificador viu a diferença entre a API e o leitor alterado, mas não identificou na resposta a combinação de três fontes descrita no achado. A medição do impacto em produção está [não verificado].

   - Pergunta literal, `logs/ESPINHA-BRIEFS/F5/v1/verif3/q.txt`, item 2: “QUEM ESCREVE e QUEM LE cada campo/valor que o diff le ou grava”; “todos supoem o MESMO significado?”
   - Resposta literal, `logs/ESPINHA-BRIEFS/F5/v1/verif3/r.txt`, item 1: “a API ainda expõe `respondida_em` da projeção no JSON; o botão de correção, porém, consulta o leitor alterado.”
   - Limite literal da resposta, no item 2 do mesmo arquivo: “A enumeração exaustiva de **todos** os escritores e leitores fora desse escopo, e a uniformidade de seus fusos e significados, permanecem **[não verificado]**.”

   A triagem de `verif1` tratou de outra divisão de fontes, na tag, e a considerou procedente: “tag combina fontes”; “a tag SAI da volta 1 e entra na volta 2” (`logs/ESPINHA-BRIEFS/F5/v1/verif1/triagem.txt`, item 1). Ela não é um descarte do achado das três combinações [não verificado].

3. **Carga integral do contador em memória — classe (e), não perguntado.** As perguntas de custo citadas exigiam verificar consultas e N+1; não pediam o volume mantido em memória nem uso de `iterator`. Tempo e memória em produção estão [não verificado].

   - Pergunta literal mais próxima, `logs/ESPINHA-BRIEFS/F5/v1/verif3/q.txt`, item 5: “Custo: cada chamador real dos leitores tocados traz `decisoes` prefetchado? Consulta por linha (N+1) em algum caminho de tela/lote?”
   - Resposta literal, `logs/ESPINHA-BRIEFS/F5/v1/verif3/r.txt`, item 5: “Cobrança, fio, histórico e contador carregam `decisoes` em lote” e “custo total e outros N+1 das telas ou da frota são **[não verificado]**.”
   - Quanto à ausência de pergunta sobre memória em todas as perguntas fornecidas: [não verificado] por linha física dos anexos.

4. **Universo sem leitor e zero sobre universo vazio — classe (e), não perguntado.** Foi pedida a publicação do universo e quem lia o contador de divergência; os trechos citados não pediam quem lia a chave de universo nem o comportamento do ESTADO para universo vazio.

   - Pergunta literal, `logs/ESPINHA-BRIEFS/F5/v1/q3.txt`, item 1: “`reg['divergencia_texto_resposta_universo'] >= 1`.”
   - Pergunta literal, `logs/ESPINHA-BRIEFS/F5/v1/verif3/q.txt`, item 3: “Quem le o contador (ESTADO) e o que acontece com valor > 0?”
   - Resposta literal, `logs/ESPINHA-BRIEFS/F5/v1/verif3/r.txt`, item 2: “o vigia grava o resultado em `censo` e zero em `esperado`; o leitor do ESTADO compara as duas entradas genericamente.”
   - O achado específico sobre não haver leitor do universo e sobre universo vazio não aparece nesses trechos de resposta; sua ausência nas demais respostas fornecidas está [não verificado] por linha física dos anexos.

5. **Selo limitado à variável `p` e caso ausente — classe (d), modo comum.** A pergunta pedia mutantes plausíveis de cada sítio e a resposta apontou outro mutante, do contador. O dado de perguntas, respostas e triagens fornecido não identifica a limitação do selo a `p` nem o caso específico descrito pelo opus [não verificado] por linha física dos anexos.

   - Pergunta literal, `logs/ESPINHA-BRIEFS/F5/v1/verif3/q.txt`, item 4: “Ha mutante plausivel de cada sitio tocado que nenhum teste morde?”
   - Resposta literal, `logs/ESPINHA-BRIEFS/F5/v1/verif3/r.txt`, item 4: “**Mutante plausível sem mordida demonstrada:** comparar os textos após `strip()` no contador”.
   - Pergunta anterior sobre a forma do selo, `logs/ESPINHA-BRIEFS/F5/v1/verif1/q.txt`, item 5: “Apelido (`getattr(p, campo)` com campo variavel, `p.__dict__`, `values()`) escapa?”
   - Resposta literal correspondente, `logs/ESPINHA-BRIEFS/F5/v1/verif1/r.txt`, item 5: “Ele detecta atributos diretos do argumento e certas strings literais em chamadas; portanto `getattr(p, campo)` com nome variável, `p.__dict__` e `values()` com campo variável escapam dessa inspeção.”

| Achado | Classe |
|---|---|
| 1. Escopo do aval e pouso | (a) Codex | [nao verificado]
| 2. Três combinações de fonte | (a) Codex |
| 3. Memória do contador | (e) Não perguntado |
| 4. Leitura do universo vazio | (e) Não perguntado |
| 5. Selo e caso ausente | (d) Modo comum |

**Contagem:** (a) 2; (b) 0; (c) 0; (d) 1; (e) 2. Essa contagem é sobre os cinco achados numerados do anexo `opus_final.r`; numeração física do anexo [não verificada].

## 19/11 14:07 · sessao f279c43dcb2d · CADEIA-20-PASSO-1 · sonda · origem=Code

# app/ponto/management/commands/processar_cartorio.py:33
# app/ponto/services/cartorio.py:959
# app/ponto/services/cartorio.py:2002
# app/ponto/services/cartorio.py::medir_razoes_sobras [nao verificado]
# app/ponto/services/cxr_na_lista.py:152
# app/ponto/services/espelho.py:51
# app/ponto/services/espelho.py:1210
# app/chamados/reconciliador.py:465
# app/core/observ.py:94
# app/core/models.py:285
# app/chamados/models.py:33
# app/escala/models.py:1635

import contextlib
import copy
import hashlib
import inspect
import io
import json
from datetime import date, datetime
from zoneinfo import ZoneInfo

from django.db import transaction
from django.utils import timezone
from freezegun import freeze_time

from colaboradores.models import Colaborador
from core.models import LogAuditoria
from escala.models import CelulaDia
from ponto.management.commands.detectar_ausencias import Command as Emissor
from ponto.services import cartorio
from ponto.services.cxr_na_lista import ultimo_lavrado
from ponto.services.espelho import espelho_do_colab
import chamados.models as modelos_chamados


INI = date(2026, 10, 21)
FIM = date(2026, 11, 20)
HOJE = FIM
FUSO = ZoneInfo("America/Sao_Paulo")
AGORA = datetime(2026, 11, 20, 23, 59, tzinfo=FUSO)
CAMPOS = (
    "minutos_trabalhados",
    "minutos_atraso",
    "minutos_saida_antecipada",
    "minutos_intrajornada_indenizada",
    "minutos_extra",
    "minutos_noturnos_legais",
)
ACOES_RETENCAO = (
    "retratacao_retida_irma_orfa",
    "retratacao_retida_competencia_entregue",
    "retratacao_retida_justificativa_alheia",
)


class DesfazerSonda(Exception):
    pass


def json_canonico(valor):
    return json.dumps(
        valor, sort_keys=True, ensure_ascii=True, separators=(",", ":"), default=str
    )


def fotografia(queryset):
    return {
        linha["pk"]: json_canonico(linha)
        for linha in queryset.order_by().values("pk", *[
            campo.attname for campo in queryset.model._meta.concrete_fields
            if not campo.primary_key
        ])
    }


def variacao(antes, depois):
    return {
        "antes": len(antes),
        "depois": len(depois),
        "criados": len(depois.keys() - antes.keys()),
        "alterados": sum(
            antes[pk] != depois[pk] for pk in antes.keys() & depois.keys()
        ),
        "excluidos": len(antes.keys() - depois.keys()),
    }


def somar_medida(total, medida):
    for chave, valor in medida.items():
        if chave == "contador_igual_universo":
            total[chave] = total.get(chave, True) and valor
        elif isinstance(valor, dict):
            destino = total.setdefault(chave, {})
            for subchave, numero in valor.items():
                destino[subchave] = destino.get(subchave, 0) + numero
        else:
            total[chave] = total.get(chave, 0) + valor


def main():
    linhas = []
    erros = []
    desfeito = False
    concluiu_transacao = False
    primeira_celula = None
    estado_original = None

    modelo_chamado = getattr(modelos_chamados, "Chamado", None)
    if modelo_chamado is None:
        modelo_chamado = modelos_chamados.ChamadoColaborador

    with freeze_time(AGORA), timezone.override(FUSO):
        try:
            with transaction.atomic():
                universo = CelulaDia.objects.filter(
                    colaborador__empresa_id__in=(2, 3, 4),
                    data__gte=INI,
                    data__lte=FIM,
                )
                ids = list(
                    universo.order_by()
                    .values_list("colaborador_id", flat=True)
                    .distinct()
                    .order_by("colaborador_id")
                )
                quantidade_celulas = universo.count()
                primeira_celula = universo.order_by("pk").values_list(
                    "pk", flat=True
                ).first()
                if primeira_celula is not None:
                    estado_original = fotografia(
                        CelulaDia.objects.filter(pk=primeira_celula)
                    )

                chamados_antes = fotografia(modelo_chamado.objects.all())
                retencoes = LogAuditoria.objects.filter(
                    acao__in=ACOES_RETENCAO, modelo="ChamadoColaborador"
                )
                retencoes_antes = fotografia(retencoes)

                lavra = ultimo_lavrado() if ids else None
                aceita_lavra = (
                    "lavra" in inspect.signature(cartorio.julgar_colab).parameters
                )
                medir = getattr(cartorio, "medir_razoes_sobras", None)
                medida_total = {}
                totais = {campo: 0 for campo in CAMPOS}
                totais.update(periodos=0, celulas=0, orfas=0)
                atas_primeira = {}
                ids_concluidos = []

                def relavrar(colab, celulas):
                    kwargs = dict(
                        hoje=HOJE,
                        agora=AGORA,
                        apply_=True,
                        emissor=Emissor(),
                        out=cartorio.contadores(),
                        prefix="",
                        write=lambda mensagem: None,
                        forcar=True,
                    )
                    if aceita_lavra:
                        kwargs["lavra"] = lavra
                    cartorio.julgar_colab(colab, celulas, **kwargs)

                for cid in ids:
                    try:
                        with transaction.atomic(), contextlib.redirect_stdout(io.StringIO()):
                            colab = Colaborador.objects.get(pk=cid)
                            celulas_passadas = list(
                                CelulaDia.objects.filter(
                                    colaborador=colab,
                                    data__gte=INI,
                                    data__lte=FIM,
                                    data__lt=HOJE,
                                ).order_by("data", "pk")
                            )
                            if celulas_passadas:
                                relavrar(colab, celulas_passadas)

                            celulas = list(
                                CelulaDia.objects.filter(
                                    colaborador=colab, data__gte=INI, data__lte=FIM
                                ).order_by("data", "pk")
                            )
                            espelho = espelho_do_colab(
                                colab, HOJE, INI, FIM, agora=AGORA
                            )
                            resultado = espelho["resultado"]
                            periodos = list(resultado.periodos or []) + list(
                                resultado.periodos_ft or []
                            )
                            periodos = [
                                p for p in periodos
                                if INI <= timezone.localtime(p.entrada).date() <= FIM
                            ]
                            dinheiro = {
                                campo: sum(getattr(p, campo) or 0 for p in periodos)
                                for campo in CAMPOS
                            }
                            dinheiro["periodos"] = len(periodos)
                            orfas = []
                            atas = {}
                            for celula in celulas:
                                ata = celula.ata or {}
                                lista = ata.get("orfas") or []
                                if not isinstance(lista, list):
                                    raise TypeError("orfas")
                                orfas.append([celula.pk, lista, len(lista)])
                                atas[celula.pk] = copy.deepcopy(celula.ata)

                            medida = medir(celulas) if medir is not None else None

                        dinheiro["orfas_sha1"] = hashlib.sha1(
                            json_canonico(orfas).encode("utf-8")
                        ).hexdigest()
                        linhas.append("DF {} {}".format(cid, json_canonico(dinheiro)))
                        for campo in CAMPOS:
                            totais[campo] += dinheiro[campo]
                        totais["periodos"] += dinheiro["periodos"]
                        totais["celulas"] += len(orfas)
                        totais["orfas"] += sum(item[2] for item in orfas)
                        atas_primeira.update(atas)
                        ids_concluidos.append(cid)
                        if medida is not None:
                            somar_medida(medida_total, medida)
                    except Exception as exc:
                        erros.append((cid, type(exc).__name__))

                mudaram = 0
                for cid in ids_concluidos:
                    try:
                        with transaction.atomic(), contextlib.redirect_stdout(io.StringIO()):
                            colab = Colaborador.objects.get(pk=cid)
                            celulas_passadas = list(
                                CelulaDia.objects.filter(
                                    colaborador=colab,
                                    data__gte=INI,
                                    data__lte=FIM,
                                    data__lt=HOJE,
                                ).order_by("data", "pk")
                            )
                            if celulas_passadas:
                                relavrar(colab, celulas_passadas)
                            atuais = list(
                                CelulaDia.objects.filter(
                                    colaborador=colab, data__gte=INI, data__lte=FIM
                                ).values_list("pk", "ata")
                            )
                            mudancas_colab = sum(
                                pk not in atas_primeira or atas_primeira[pk] != ata
                                for pk, ata in atuais
                            )
                            pks_atuais = {pk for pk, _ in atuais}
                            pks_primeira = {
                                pk for pk, _ in CelulaDia.objects.filter(
                                    colaborador=colab, data__gte=INI, data__lte=FIM
                                ).values_list("pk", "ata")
                                if pk in atas_primeira
                            }
                            mudancas_colab += len(pks_primeira - pks_atuais)
                        mudaram += mudancas_colab
                    except Exception as exc:
                        erros.append((cid, type(exc).__name__))

                chamados_depois = fotografia(modelo_chamado.objects.all())
                retencoes_depois = fotografia(retencoes)

                linhas.append("TOTAL " + json_canonico(totais))
                if medir is None:
                    linhas.append("MEDIDA ausente")
                else:
                    if not ids_concluidos:
                        medida_total = medir([])
                    linhas.append("MEDIDA " + json_canonico(medida_total))
                linhas.append(
                    "IDEMP celulas_que_mudaram_na_2a={}".format(mudaram)
                )
                linhas.append(
                    "RETENCAO antes={} depois={}".format(
                        json_canonico({
                            "chamados": len(chamados_antes),
                            "retencoes": len(retencoes_antes),
                        }),
                        json_canonico({
                            "chamados": variacao(chamados_antes, chamados_depois),
                            "retencoes": variacao(retencoes_antes, retencoes_depois),
                        }),
                    )
                )
                linhas.append(
                    "UNIVERSO colabs={} celulas={}".format(
                        len(ids), quantidade_celulas
                    )
                )
                concluiu_transacao = True
                raise DesfazerSonda()
        except DesfazerSonda:
            pass
        except Exception as exc:
            erros.append((0, type(exc).__name__))

        if primeira_celula is not None and estado_original is not None:
            try:
                desfeito = (
                    fotografia(CelulaDia.objects.filter(pk=primeira_celula))
                    == estado_original
                )
            except Exception as exc:
                erros.append((0, type(exc).__name__))

    for linha in linhas:
        print(linha)
    for cid, classe in erros:
        print("ERRO {} {}".format(cid, classe))
    print("ERROS={}".format(len(erros)))
    print("DESFEITO={}".format(desfeito))
    if erros or not concluiu_transacao or not desfeito:
        print("NAO_CONCLUIDO")


main()

_7387 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
