# CODEX-ESTADO

> Ultima resposta processada: **31/10/2026 17:44** · publicado em **31/10/2026 17:48** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 31/10 17:44 · sessao 7e00fa505aa6 · TRAVA-COBERTURA · leitura · origem=Code

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 31/10 17:42 · sessao bc4f2203bbdb · - · - · origem=Ronald (tmux)

Censo de reentrância: nos caminhos versionados examinados, nenhum ancestral identificado segura `/tmp/juliani_db_test.lock` antes do disparo; os ancestrais prendem arquivos distintos, e a trava global protege apenas o comando filho. Ancestrais externos e processos de produção são **[não verificado]**. (`bin/trava_teste.sh:35`; `bin/trava_teste.sh:42`)

| Chamador | Cadeia e lock ancestral identificado |
|---|---|
| `suite` | `suite → exec trava → docker`; não adquire flock antes do `exec`. (`bin/suite.sh:96`) |
| `regua` | `regua → trava → docker`; o próprio script segura `/tmp/regua.lock`, distinto do global. (`bin/regua.sh:16`; `bin/regua.sh:97`; `bin/regua.sh:165`) |
| `pre-push` | `git push → hook wrapper → bin/pre-push → _teste_irmao → trava`; prende `/tmp/regua.lock`, ou aceita `REGUA_TRAVA_HERDADA=1`. (`bin/hooks.sh:56`; `bin/pre-push.sh:80`; `bin/pre-push.sh:86`; `bin/pre-push.sh:142`) |
| `vigia_arvore` | Prende `/tmp/vigia_arvore.lock`; chama sequencialmente `suite → trava`, depois outra trava na confirmação viva. (`bin/vigia_arvore.sh:24`; `bin/vigia_arvore.sh:83`; `bin/vigia_arvore.sh:106`; `bin/vigia_arvore.sh:131`) |
| `regua_relogio` | Chama diretamente `trava → docker`; seu corpo anterior prepara diretório e env-file, sem flock próprio. (`bin/regua_relogio.sh:31`; `bin/regua_relogio.sh:46`; `bin/regua_relogio.sh:51`) |
| `molde_fatia` | `esteira → rodar → teste → trava`; ancestral segura `/tmp/esteira___FATIA__.lock`, distinto. (`bin/molde_fatia/esteira.sh:7`; `bin/molde_fatia/esteira.sh:15`; `bin/molde_fatia/rodar.sh:40`) |
| `molde_fatia MAPA` | Mesmo ancestral; MAPA ainda executa docker diretamente e termina antes da função `teste`. (`bin/molde_fatia/rodar.sh:28`; `bin/molde_fatia/rodar.sh:33`) |
| `molde_autocura` | `esteira → rodar → teste → docker`; ancestral segura `/tmp/esteira___FATIA__.lock`; a cadeia só começa após `rodar` retornar. (`bin/molde_autocura/esteira.sh:6`; `bin/molde_autocura/esteira.sh:14`; `bin/molde_autocura/esteira.sh:23`; `bin/molde_autocura/rodar.sh:20`) |
| `isolamento` | `cadeia → regua → trava → docker`, retorna; então `cadeia → isolar_ou_parar → _iso_roda`. A cadeia ainda segura `/tmp/regua.lock`, mas a trava do teste anterior já retornou. (`bin/molde_fatia/cadeia.sh:14`; `bin/molde_fatia/cadeia.sh:51`; `bin/molde_fatia/cadeia.sh:55`; `bin/isolamento.sh:45`) |
| `placar_code` | `_cred` abre subshell, gera env-file e dispara docker diretamente. Não há aquisição global nesse sítio. (`bin/placar_code.sh:121`; `bin/placar_code.sh:122`) |
| `esteira_vigia` | `vigia_esteira → Python → passada → _isolar/_rodar_testes`, sob `/tmp/vigia_esteira.lock`, distinto. (`bin/vigia_esteira.sh:9`; `bin/vigia_esteira.sh:11`; `app/core/esteira_vigia.py:1473`; `app/core/esteira_vigia.py:740`) |
| Python pelo integrador | `integrador → integrador_lote.rodar → _suite → regua → trava`; integrador segura `/tmp/integrador.lock`, os locks da árvore/raia e `/tmp/regua.lock`; chamadas Python adicionais acontecem fora do comando protegido da régua. (`bin/integrador.sh:9`; `bin/integrador.sh:32`; `bin/integrador.sh:51`; `bin/integrador.sh:52`; `app/core/integrador_lote.py:639`; `app/core/integrador_lote.py:375`) |

Há **sete sítios** chamadores de `_rodar_testes`: quatro internos (`_isolar`, `auto_revert`, `curar_por_relogio`, `autocura`) e três no integrador (`_suite`, `_isolamento`, `_conferir_quarentena`). (`app/core/esteira_vigia.py:740`; `app/core/esteira_vigia.py:1269`; `app/core/esteira_vigia.py:1319`; `app/core/esteira_vigia.py:1406`; `app/core/integrador_lote.py:373`; `app/core/integrador_lote.py:403`; `app/core/integrador_lote.py:769`)

Perda de rc e contraexemplos:

- `suite` usa `exec`, preservando o resultado da trava; `_teste_irmao` guarda `_rc`, distingue 75 e devolve `_rc`. (`bin/suite.sh:96`; `bin/pre-push.sh:141`; `bin/pre-push.sh:149`; `bin/pre-push.sh:157`)
- `regua` recebe diretamente o status, mas o descarta e decide por texto `Ran`/`OK`; um 75 vira “a suite nem rodou” e rc final 1. (`bin/regua.sh:165`; `bin/regua.sh:170`; `bin/regua.sh:178`; `bin/regua.sh:205`)
- `regua_relogio` continua após o pipeline e reinicia `RC=0`; ausência de teste pode terminar OK se a lista conhecida estiver vazia. (`bin/regua_relogio.sh:51`; `bin/regua_relogio.sh:58`; `bin/regua_relogio.sh:67`; `bin/regua_relogio.sh:77`)
- `regua_calendario` ignora o rc dos três docker e compara apenas nomes vermelhos; três falhas de infraestrutura podem terminar OK com lista vazia. (`bin/regua_calendario.sh:43`; `bin/regua_calendario.sh:48`; `bin/regua_calendario.sh:55`; `bin/regua_calendario.sh:59`)
- `vigia_arvore` troca ausência de `Ran` por rc 1; nas confirmações decide pelo log, sem preservar 75. (`bin/vigia_arvore.sh:88`; `bin/vigia_arvore.sh:90`; `bin/vigia_arvore.sh:106`; `bin/vigia_arvore.sh:136`)
- Moldes descartam os rc de RED/GREEN e decidem pelo texto; o molde de autocura termina com `echo`, podendo retornar 0 após falha do teste. (`bin/molde_fatia/rodar.sh:46`; `bin/molde_fatia/rodar.sh:48`; `bin/molde_autocura/rodar.sh:24`; `bin/molde_autocura/rodar.sh:27`)
- `_iso_roda` devolve o resultado do grep; falha de infraestrutura pode ser atribuída à árvore por `isolar_ou_parar`. (`bin/isolamento.sh:31`; `bin/isolamento.sh:34`; `bin/isolamento.sh:45`; `bin/isolamento.sh:49`)
- `placar_code` não guarda falha de `teste_envfile` nem do docker/pipeline; saída vazia vira `_cred=0`. (`bin/placar_code.sh:121`; `bin/placar_code.sh:127`; `bin/placar_code.sh:128`)
- `_rodar_testes` ignora `returncode`, devolvendo apenas presença de `OK`; após migração, 75 também se tornaria `False`, indistinto de vermelho. `_isolar` grava “arvore” e `_isolamento` devolve verdadeiro nessa situação. (`app/core/esteira_vigia.py:760`; `app/core/esteira_vigia.py:762`; `app/core/esteira_vigia.py:741`; `app/core/integrador_lote.py:403`)
- `_conferir_quarentena` ignora até esse booleano: log sem `FAIL/ERROR` libera todos os testes da lista, mesmo que nada tenha rodado. (`app/core/integrador_lote.py:769`; `app/core/integrador_lote.py:771`; `app/core/integrador_lote.py:772`)
- `_suite` ignora rc da régua e escolhe o arquivo `/tmp/regua_*` mais recente; falha antes de produzir log novo pode ler uma execução anterior. (`app/core/integrador_lote.py:375`; `app/core/integrador_lote.py:376`; `app/core/integrador_lote.py:379`)
- Existe cópia adicional de pre-push em `app/core/hooks`, fora do universo `bin/**/*.sh + Python` do contrato: docker direto em `_teste_irmao`, com duas chamadas de testes. Sua ativação é **[não verificado]**; o wrapper oficial gerado aponta a `bin/pre-push.sh`. (`app/core/hooks/pre-push.sh:54`; `app/core/hooks/pre-push.sh:82`; `app/core/hooks/pre-push.sh:87`; `bin/hooks.sh:56`)

Acoplamentos de testes ao texto/argv: [nao verificado]

- `test_arvore_vermelha` exige literais `desfazer_copia`, `exec bash "$script"`, `git archive HEAD app`, `SNAP=/tmp/vigia_arvore_commit_`, `sozinhos` e `alarme_arvore`; extrai o **primeiro** `grep -oP` de isolamento e executa só esse grep em log sintético. Não executa os scripts nem cria cadeia aninhada de trava. (`app/core/tests/test_arvore_vermelha.py:39`; `app/core/tests/test_arvore_vermelha.py:53`; `app/core/tests/test_arvore_vermelha.py:58`; `app/core/tests/test_arvore_vermelha.py:61`; `app/core/tests/test_arvore_vermelha.py:67`; `app/core/tests/test_arvore_vermelha.py:72`)
- `test_suite_por_lote` analisa o texto do molde com regex de linhas contendo `LABELS` e `regua.sh`/`$LABELS`; não executa a suite do integrador. (`app/core/tests/test_suite_por_lote.py:81`; `app/core/tests/test_suite_por_lote.py:86`; `app/core/integrador_lote.py:203`)
- `test_prepush_veredito_e_o_rc` recorta a função por `^_teste_irmao()`/`^}`, exige o literal `return "$_rc"` e proíbe `| tail -12; }`. (`bin/tests/test_prepush_veredito_e_o_rc.sh:61`; `bin/tests/test_prepush_veredito_e_o_rc.sh:64`; `bin/tests/test_prepush_veredito_e_o_rc.sh:68`)
- `test_prepush_testa_o_commit` exige o argv textual `-v "$ARVORE_PUSH/app":/app`, proíbe a montagem da árvore viva e cobra nomes dos auxiliares. (`bin/tests/test_prepush_testa_o_commit.sh:63`; `bin/tests/test_prepush_testa_o_commit.sh:65`; `bin/tests/test_prepush_testa_o_commit.sh:69`; `bin/tests/test_prepush_testa_o_commit.sh:71`)
- `test_recursos_sourced` junta continuações e exige `TESTE_DOCKER` ou `--cpuset-cpus` em cada linha contendo `docker run` e `manage.py test`. (`bin/tests/test_recursos_sourced.sh:38`; `bin/tests/test_recursos_sourced.sh:42`)
- `test_segredo_fora_do_argv` busca `-e DB_PASSWORD=` só em `bin`, exige os literais `teste_envfile()` e `umask 177`, além do modo 600 do arquivo existente. (`bin/tests/test_segredo_fora_do_argv.sh:24`; `bin/tests/test_segredo_fora_do_argv.sh:39`; `bin/tests/test_segredo_fora_do_argv.sh:42`; `bin/tests/test_segredo_fora_do_argv.sh:48`)
- `test_fonte_unica_antes_do_uso` depende de regex de `source ... recursos.sh` e posição da chamada das funções; aceita mesma linha, como no placar; varre só scripts diretos de `bin` e `molde_fatia`. (`bin/tests/test_fonte_unica_antes_do_uso.sh:38`; `bin/tests/test_fonte_unica_antes_do_uso.sh:41`; `bin/tests/test_fonte_unica_antes_do_uso.sh:47`; `bin/tests/test_fonte_unica_antes_do_uso.sh:54`)

`gate_cobranca` não dispara teste: cria `SONDA` única no host, mas copia para nome fixo `/tmp/gate_sonda.py` **dentro de `saas_core`**, e usa `manage.py shell`/`tenant_command`; esse temporário não fica no `logs/` do env-file. Falha de coleta vira zero pelo `_int`, portanto dado ausente pode parecer medição zero. (`bin/gate_cobranca.sh:18`; `bin/gate_cobranca.sh:36`; `bin/gate_cobranca.sh:37`; `bin/gate_cobranca.sh:40`; `bin/gate_cobranca.sh:54`)

## 31/10 17:42 · sessao 36dbb69fb8ed · E4-7-SAIDA-III · verificador · origem=Code

**A volta 2 ainda não fecha a verificação:** A1 corrige o noturno do teste, mas admite outro caso de data divergente; A2 reduz consultas, porém mantém N+1 nos encerrados; A3 e A4 têm a cura pedida no código. (`app/core/services/painel_op.py:97`, `app/core/services/painel_op.py:202`, `app/core/services/painel_op.py:214`; `app/templates/core/_painel_card.html:69`)

RED/GREEN da suíte Django, número efetivamente medido pela sonda, ocorrência dos contraexemplos em produção e ordem histórica de pouso: **[não verificado]**. [nao verificado]

1) **Quem escreve e quem lê.**

Os campos abaixo são escritos no contexto em memória do painel; `dia` lê o estado persistido pelo D4. O escritor persistente de `status_dia/status_rotulo` continua sendo a porta de lavratura. (`app/core/services/painel_op.py:225`; `app/ponto/status_dia.py:171`; `app/ponto/portas/celula.py:701`, `app/ponto/portas/celula.py:709`)

| Campo/valor | Quem escreve ou fornece | Quem lê |
|---|---|---|
| `status=normal/geofence/turno_aberto` | O juiz fornece `_tm`; o painel escolhe os rótulos usando alerta geo e fim previsto +30 minutos. **Os quatro rótulos não vêm prontos do juiz.** (`app/core/services/painel_op.py:168`, `app/core/services/painel_op.py:172`) | Filtro, contadores, agregado geo, ordenação, avatar, badge e filtro JS via atributo. (`app/core/services/painel_op.py:202`, `app/core/services/painel_op.py:244`, `app/core/services/painel_op.py:288`, `app/core/services/painel_op.py:296`; `app/templates/core/_painel_card.html:6`, `app/templates/core/_painel_card.html:54`, `app/templates/core/_painel_card.html:66`) |
| `status=encerrado` | Painel: existe E recente e não existe `_tm` vivo. Não exige encontrar um turno efetivamente fechado. (`app/core/services/painel_op.py:193`) | Além dos leitores anteriores, abre o caminho de data/D4. (`app/core/services/painel_op.py:214`; `app/templates/core/_painel_card.html:67`) |
| `status=None` | Painel: sem turno vivo e sem E selecionada. (`app/core/services/painel_op.py:196`) | Avatar/badge usam `previsao`; Designar exige também `previsto_agora`. (`app/templates/core/_painel_card.html:54`, `app/templates/core/_painel_card.html:66`, `app/templates/core/_painel_card.html:69`) |
| `previsao=folga/escala_suspeita/previsto/None` | `_previsao_de_hoje`: pergunta `EC.eh_dia_trabalho`; ausência de EC/tipo produz `None`. (`app/core/services/painel_op.py:27`; `app/escala/models.py:1048`) | Avatar e badge; testes dos dois primeiros casos. (`app/templates/core/_painel_card.html:54`, `app/templates/core/_painel_card.html:66`; `app/core/tests/test_e4_7iii_painel_e_presenca.py:94`, `app/core/tests/test_e4_7iii_painel_e_presenca.py:103`) |
| `dia=None` ou `{status,rotulo,fonte}` | Inicialização local; D4 somente após encontrar `_d_turno` e obter encerramento verdadeiro. (`app/core/services/painel_op.py:212`) | Tooltip e texto `enc. · status` do card. (`app/templates/core/_painel_card.html:66`) |
| `data_turno` | `_tm.data_turno`; sem vivo, `_d_turno`; sem mapeamento, **hoje**. (`app/core/services/painel_op.py:230`) | Testes de ancoragem e novo A1. Leitor produtivo adicional encontrado: **[não verificado]**. (`app/core/tests/test_painel_op_turno_aberto_ancora.py:50`; `app/core/tests/test_painel_op_turno_pelo_juiz.py:80`; `app/core/tests/test_e4_7iii_painel_e_presenca.py:263`) |
| `filtro_status` | GET da página/lote; serviço transforma palavra fora de `statuses` em `todos`. (`app/core/views.py:609`, `app/core/views.py:627`; `app/core/services/painel_op.py:64`) | Recorte das linhas e exclusão de postos vazios; a view ainda publica no contexto o GET bruto. (`app/core/services/painel_op.py:202`, `app/core/services/painel_op.py:284`; `app/core/views.py:669`) |
| `statuses` | Lista fixa dos quatro valores, rótulos e cores. (`app/core/services/painel_op.py:56`) | Validação do filtro, retorno e contexto da view; consumidor atual no HTML: **[não verificado]**. (`app/core/services/painel_op.py:64`, `app/core/services/painel_op.py:314`; `app/core/views.py:639`, `app/core/views.py:672`) |
| `_turno_vivo` | `turnos_abertos_de`, descartando respostas sem turno/entrada. (`app/core/services/painel_op.py:97`) | `_em_turno_ids`, presença/cobertura, duração, status e data da linha. (`app/core/services/painel_op.py:101`, `app/core/services/painel_op.py:168`, `app/core/services/painel_op.py:176`, `app/core/services/painel_op.py:230`, `app/core/services/painel_op.py:286`) |
| `contadores` | Quatro somas sobre **linhas já filtradas**; linhas com `status=None` não entram nessas somas. (`app/core/services/painel_op.py:202`, `app/core/services/painel_op.py:244`) | View e stats da página/fragmento. (`app/core/views.py:637`, `app/core/views.py:670`; `app/templates/core/_painel_stats.html:32`; `app/templates/core/_painel_fragment.html:2`) |
| `previsto_agora` | Composição local de `eh_dia_trabalho` e `marcos_turno`, para ontem/hoje, usando EC ativa. (`app/core/services/painel_op.py:109`, `app/core/services/painel_op.py:144`) | Cobertura do posto e condição de Designar. (`app/core/services/painel_op.py:290`; `app/templates/core/_painel_card.html:69`) |

A entrada selecionada é a última E apurável por timestamp, com limite inferior de 24 horas e **sem limite superior**; ela também fornece horário, ID, coordenadas e entrada usada na classificação geo. (`app/core/services/painel_op.py:78`, `app/core/services/painel_op.py:83`, `app/core/services/painel_op.py:223`, `app/core/services/painel_op.py:228`; `app/ponto/turnos.py:2844`)

| Atributo/ação do card | Escritor | Leitor |
|---|---|---|
| `data-search` | Nome do posto/empresa. (`app/templates/core/_painel_card.html:3`) | Busca por `dataset.search`. (`app/templates/core/painel_operacional.html:198`) |
| `data-cobertura` | Cobertura calculada do posto. (`app/templates/core/_painel_card.html:4`) | Filtro Descobertos. (`app/templates/core/painel_operacional.html:176`) |
| `data-tem-geofence` | `p.geofence > 0` → `1/0`. (`app/templates/core/_painel_card.html:5`) | Filtro Geofence. (`app/templates/core/painel_operacional.html:177`) |
| `data-tem-aberto` | Concatena um `1` por colaborador aberto. (`app/templates/core/_painel_card.html:6`) | Filtro Aberto exige exatamente `'1'`. (`app/templates/core/painel_operacional.html:178`) |
| `data-tem-ausente` | Removido; o card atual publica apenas os atributos anteriores. (`app/templates/core/_painel_card.html:3`) | O filtro atual conserva Descobertos/Geofence/Aberto. (`app/templates/core/painel_operacional.html:173`) |
| Designar | `previsto_agora and not status`; publica titular ID/nome, posto ID/nome e empresa ID. (`app/templates/core/_painel_card.html:69`) | `abrirSubstModal` copia os cinco atributos para `estado`. (`app/templates/core/painel_operacional.html:280`) |

O modal envia titular/posto/janela ao GET de disponíveis e titular/substituto/posto/escala/janela ao POST de criação; o POST delega à porta `designar_cobertura`. `empresa_id` não integra esse POST, e o botão não transporta `data_turno`. (`app/templates/core/painel_operacional.html:343`, `app/templates/core/painel_operacional.html:410`; `app/ponto/views.py:3027`, `app/ponto/views.py:3065`; `app/templates/core/_painel_card.html:69`)

As portas chamadas pelo diff têm estes produtores e leitores externos:

| Porta | Quem produz a resposta | Outros leitores diretos encontrados |
|---|---|---|
| `turnos_abertos_de` | Carregador plural chama o mesmo `_turno_aberto_puro` do singular. (`app/ponto/turnos.py:2266`, `app/ponto/turnos.py:2333`) | Serviço situacional. (`app/colaboradores/services/situacional.py:169`) |
| `dia_das_batidas` | Dias dos turnos recebidos; batida não reclamada recebe dia civil. (`app/ponto/turnos.py:2883`) | API e espelho. (`app/api/views.py:1446`, `app/api/views.py:2090`; `app/ponto/services/espelho.py:462`) |
| `dias_encerrados` | Carrega células/vínculos e chama `_dia_encerrado_motivo_puro` por dia. (`app/ponto/turnos.py:2129`) | Calendário, cartório, D4, emissão retroativa e suprajuiz. (`app/colaboradores/services/calendario.py:697`; `app/ponto/services/cartorio.py:910`; `app/ponto/status_dia.py:176`; `app/ponto/management/commands/emitir_furo_retroativo.py:62`; `app/ponto/management/commands/supra_juiz.py:108`) |
| `status_do_dia_lavrado` | Lê célula e pergunta encerramento; devolve lavra ou resposta temporal. (`app/ponto/status_dia.py:157`) | Chamador produtivo direto encontrado nesta cópia: painel. (`app/core/services/painel_op.py:220`) |
| `turnos_do_colab` | Carrega universo vivo, células, vínculos/ata; pareia e recorta por `data_turno`. (`app/ponto/turnos.py:1848`, `app/ponto/turnos.py:1973`) | Calendário, espelho, drawer, API, PDF, núcleo, escala e caminhos de chamados/apuração listados abaixo. (`app/colaboradores/services/calendario.py:301`; `app/ponto/services/espelho.py:625`; `app/colaboradores/services/drawer.py:45`; `app/api/views.py:1449`; `app/relatorios/pdf_espelho.py:213`; `app/ponto/nucleo.py:47`) |

O restante do censo direto de `turnos_do_colab` inclui:

- Chamados: detector, juízes, modelo, emissão e materialização. (`app/chamados/detector_anomalias.py:155`; `app/chamados/juizes.py:369`, `app/chamados/juizes.py:1727`; `app/chamados/models.py:1260`; `app/chamados/services/disputa_emissao.py:644`; `app/chamados/services/materializacao.py:109`, `app/chamados/services/materializacao.py:204`)
- Escala: defesa, escala automática, impacto de troca, jornada e grade. (`app/escala/regua_defesa.py:358`; `app/escala/services/escala_auto.py:87`; `app/escala/services/impacto_troca.py:18`; `app/escala/servico_jornada.py:250`, `app/escala/servico_jornada.py:392`; `app/escala/utils.py:1524`, `app/escala/utils.py:1838`)
- Ponto: bordas, esmeril, passivo, regularização, teto, triagem, vigia, leitura de turno e views. (`app/ponto/services/bordas_realizado.py:66`; `app/ponto/services/esmeril_espelho.py:98`; `app/ponto/services/passivo_s84.py:82`; `app/ponto/services/regularizacao.py:61`; `app/ponto/services/teto_na_lista.py:68`; `app/ponto/services/triagem_batida.py:552`; `app/ponto/services/vigia_pareador_celula.py:97`; `app/ponto/turno_leitura.py:89`, `app/ponto/turno_leitura.py:95`; `app/ponto/views.py:377`, `app/ponto/views.py:1586`, `app/ponto/views.py:3167`)
- Comandos: censos, ausências, pares relâmpago, DNA, oráculo, turno retroativo, abandono e vigias. (`app/ponto/management/commands/censo_fase_12x36.py:180`; `app/ponto/management/commands/detectar_ausencias.py:336`; `app/ponto/management/commands/detectar_par_relampago.py:101`; `app/ponto/management/commands/dna_x_batida_real.py:69`; `app/ponto/management/commands/e6_oraculo.py:147`; `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:88`; `app/ponto/management/commands/marcar_abandono_provisorio.py:46`; `app/ponto/management/commands/verificar_nucleo.py:29`; `app/ponto/management/commands/vigia_plantio_overnight.py:38`; `app/relatorios/management/commands/vinculo_do_dia_divergentes.py:124`)
- No próprio módulo: realizado, encerramento, par completo, mapa de dias e batidas do dia-turno. (`app/ponto/turnos.py:778`, `app/ponto/turnos.py:2079`, `app/ponto/turnos.py:2132`, `app/ponto/turnos.py:2152`, `app/ponto/turnos.py:2882`, `app/ponto/turnos.py:2917`)

`ALLOWLIST` e `LOOKUP_ALLOWLIST` são escritas como conjuntos no módulo do contrato. A primeira alimenta os testes de novos `.date()` e entradas podres; a segunda alimenta os testes equivalentes de lookups ORM. Retirar o painel da primeira é coerente com a mudança sintática; mantê-lo na segunda corresponde aos lookups de geo/atividade ainda existentes. Isso **não verifica a semântica da data retornada indiretamente** por `dia_das_batidas`. (`app/ponto/tests/test_contract_no_batida_date.py:10`, `app/ponto/tests/test_contract_no_batida_date.py:98`, `app/ponto/tests/test_contract_no_batida_date.py:233`, `app/ponto/tests/test_contract_no_batida_date.py:247`, `app/ponto/tests/test_contract_no_batida_date.py:266`; `app/core/services/painel_op.py:104`, `app/core/services/painel_op.py:217`, `app/core/services/painel_op.py:274`)

Fora da tela, o registro vivo mantém pendentes os contadores, badge/atributo de aberto e filtro JS. O host lê o mesmo medidor por `bin/leitor_dia_estado.py`; o gerador de estado consome esse resultado. (`app/core/juizes.py:715`, `app/core/juizes.py:727`; `bin/leitor_dia_estado.py:25`, `bin/leitor_dia_estado.py:35`; `bin/gerar_estado.py:284`)

2) **A1..A4: cura e contraexemplos.**

| Item | Resultado da inspeção |
|---|---|
| **A1** | Cura a fixture 19–07, pausa 00–01, consulta 08h: agora busca o dia do turno e o teste exige esse dia. **Não fecha toda a classe**, conforme contraexemplo abaixo. (`app/core/services/painel_op.py:215`; `app/core/tests/test_e4_7iii_painel_e_presenca.py:241`, `app/core/tests/test_e4_7iii_painel_e_presenca.py:263`) |
| **A2** | Cura o custo do D4 para linhas excluídas pelo filtro e troca o turno vivo por lote. Para linhas encerradas preservadas, continuam chamadas individuais e recargas internas. Número medido exigido na triagem: **[não verificado]**. (`app/core/services/painel_op.py:97`, `app/core/services/painel_op.py:202`, `app/core/services/painel_op.py:214`; `app/ponto/status_dia.py:171`) |
| **A3** | Cura a cobertura parcial: o botão deixou de exigir posto descoberto. O teste tem dois previstos, um presente e exige botão apenas no outro. Saída antecipada continua barrada por `status=encerrado`, como já reconhecido na fila fornecida. (`app/templates/core/_painel_card.html:69`; `app/core/tests/test_e4_7iii_painel_e_presenca.py:335`, `app/core/tests/test_e4_7iii_painel_e_presenca.py:361`; `app/core/services/painel_op.py:193`) |
| **A4** | Cura `ausente/folga/lixo` na porta compartilhada por página e lote. O teste preserva também o filtro válido `encerrado`. Há quebra herdada do refresh para filtros válidos, detalhada no item 5. (`app/core/services/painel_op.py:64`; `app/core/views.py:607`, `app/core/views.py:632`; `app/core/tests/test_e4_7iii_painel_e_presenca.py:371`) |

**Contraexemplo construído de A1, somente com batidas passadas:** colaborador ativo, vínculo diurno 07–19 com intervalo 12–13; agora **17/09/2026 às 20h local**; E viva em **14/09 às 07h** e E viva em **16/09 às 20h com `is_intervalo=True`**, nenhuma outra batida. A flag existe no modelo; o pareador a absorve no turno corrente, e a guarda temporal desse trecho exige `not _iv`. Assim, o turno de 14/09 contém as duas batidas. (`app/ponto/models.py:26`; `app/ponto/turnos.py:1324`, `app/ponto/turnos.py:1347`, `app/ponto/turnos.py:1369`, `app/ponto/turnos.py:1495`)

Nesse caso:

- O plural lê 15–17/09, não recebe a E inicial e ignora a E intervalada isolada. O painel encontra a E de 16/09 nas 24 horas e a chama `encerrado`. (`app/ponto/turnos.py:2323`, `app/ponto/turnos.py:1369`; `app/core/services/painel_op.py:78`, `app/core/services/painel_op.py:83`, `app/core/services/painel_op.py:193`)
- `turnos_do_colab(15,17)` lê a E antiga pelo pad, mas elimina o turno de 14/09 pelo recorte final. `dia_das_batidas` então usa o dia civil **16/09**, que vira `data_turno` da linha. (`app/ponto/turnos.py:1857`, `app/ponto/turnos.py:1973`, `app/ponto/turnos.py:2889`; `app/core/services/painel_op.py:217`, `app/core/services/painel_op.py:230`)
- O espelho mensal atribui essa mesma E ao turno de **14/09**; o calendário mensal também a recebe nesse turno. Portanto, há divergência concreta de pertencimento/data entre as janelas. Ocorrência em produção: **[não verificado]**. (`app/ponto/services/espelho.py:462`; `app/ponto/turnos.py:2884`; `app/colaboradores/services/calendario.py:301`)

Para A2, o próprio cenário do teste — passar de três para seis colaboradores E07/S19, consulta 20h, filtro `todos` — percorre o bloco D4 mais vezes. `turnos_do_colab` consulta batidas/células/vínculos; `dias_encerrados` recarrega células/vínculos; existindo célula, D4 consulta-a e recalcula turnos/encerramento. O custo residual cresce por linha; seu coeficiente efetivo permanece **[não verificado]**. (`app/core/tests/test_e4_7iii_painel_e_presenca.py:269`, `app/core/tests/test_e4_7iii_painel_e_presenca.py:275`; `app/ponto/turnos.py:1858`, `app/ponto/turnos.py:2129`; `app/ponto/status_dia.py:171`)

Os novos testes plantam `status_dia` diretamente e não comparam painel, espelho e calendário para o mesmo caso. Os outros dois leitores ainda possuem classificadores próprios; igualdade entre suas palavras e o D4 não foi demonstrada por esses testes. (`app/core/tests/test_e4_7iii_painel_e_presenca.py:76`, `app/core/tests/test_e4_7iii_painel_e_presenca.py:260`; `app/ponto/services/espelho.py:152`; `app/colaboradores/services/calendario.py:93`)

3) **Universos e janelas não são integralmente iguais.**

| Caminho | Universo/janela efetivos |
|---|---|
| Entrada/candidatos do painel | Vivas desde `agora−24h`, sem teto superior; colaboradores ativos do recorte empresarial. (`app/core/services/painel_op.py:70`, `app/core/services/painel_op.py:78`, `app/core/services/painel_op.py:83`, `app/core/services/painel_op.py:94`) |
| Turno vivo singular/plural | Vivas por data civil de hoje−2 até hoje; mesmo corpo puro. O painel restringe antes os candidatos a 24h. (`app/ponto/turnos.py:2190`, `app/ponto/turnos.py:2323`, `app/ponto/turnos.py:2333`; `app/core/services/painel_op.py:94`) |
| Turnos para D4 do painel | Dias de turno `[hoje−2,hoje]`; consulta de batidas recebe pad `[hoje−4,hoje+1]`, mas o resultado volta a ser recortado por `data_turno`. (`app/core/services/painel_op.py:215`; `app/ponto/turnos.py:1845`, `app/ponto/turnos.py:1973`) |
| Mapa e portão do painel | Recebem corretamente a **mesma lista** e a mesma janela anterior. (`app/core/services/painel_op.py:216`) |
| D4 interno | Recarrega turnos e encerramento para `[data,data]`, não reutiliza a lista do painel. (`app/ponto/status_dia.py:175`) |
| Espelho/calendário | Usam o período apresentado, mensal ou personalizado. O calendário passa seus próprios turnos à juíza da mesma janela. (`app/ponto/services/espelho.py:462`, `app/ponto/services/espelho.py:625`; `app/colaboradores/services/calendario.py:285`, `app/colaboradores/services/calendario.py:301`, `app/colaboradores/services/calendario.py:697`) |

A identidade do filtro `retratada_em IS NULL` foi preservada nesses carregadores; isso não torna suas janelas equivalentes. O próprio contrato de alimentação avisa que a resposta do pareamento depende da janela. (`app/ponto/turnos.py:1858`, `app/ponto/turnos.py:2111`, `app/ponto/turnos.py:2324`, `app/ponto/turnos.py:2890`)

**Não: dois dias não cobrem universalmente todo turno cuja E recente vira “encerrado”.** O caso intervalado acima refuta a afirmação, porque a autoridade de apresentação dá dia civil à batida cujo turno foi excluído da lista recebida. (`app/ponto/turnos.py:1976`, `app/ponto/turnos.py:2884`, `app/ponto/turnos.py:2893`)

Há ainda um recorte herdado que perde presença normal: **24x48 07–07, E ontem às 07h, sem S, consulta hoje às 08h**. O juiz pode mantê-la viva até 11h, mas a E já tem 25 horas e fica fora dos candidatos do painel. A troca para plural não corrige isso. (`app/ponto/selecao_periodo.py:164`; `app/ponto/turnos.py:1987`, `app/ponto/turnos.py:2005`; `app/core/services/painel_op.py:78`, `app/core/services/painel_op.py:94`)

Também não há garantia para E futura: uma E depois de amanhã passa pelo limite inferior, fica fora da janela do plural/mapa e pode resultar em `encerrado`, `dia=None`, `data_turno=hoje`. Existência desse dado em produção: **[não verificado]**. (`app/core/services/painel_op.py:83`, `app/core/services/painel_op.py:193`, `app/core/services/painel_op.py:230`; `app/ponto/turnos.py:2844`)

4) **Classes de reprovação e mordida dos testes.** [nao verificado]

- **Segundo juiz:** a troca singular→plural não cria um: ambos chegam ao mesmo corpo. O dia do turno e o D4 já têm autoridades registradas. Entretanto, `dia_das_batidas` responde também à apresentação de batidas não reclamadas; usar sua resposta como identidade de turno é justamente a fragilidade de A1. (`app/ponto/turnos.py:2266`, `app/ponto/turnos.py:2333`, `app/ponto/t

[truncado em 20000 de 28318 caracteres]

## 31/10 17:42 · sessao 834d7c0b4f9f · - · - · origem=Ronald (tmux)

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 31/10 17:41 · sessao a39845de0124 · - · - · origem=Ronald (tmux)

1. **Leitores/escritores identificados**

- A própria trava abre/trunca `/tmp/juliani_db_test.lock`, obtém o flock, escreve `<ESTEIRA_QUEM ou basename>:<pid> desde <data>` em `.quem` e escreve `livre desde <data>` no `EXIT` (`bin/trava_teste.sh:21`, `bin/trava_teste.sh:23`, `bin/trava_teste.sh:35-41`).
- `--quem` só testa se `.quem` tem conteúdo e o imprime inteiro; sem conteúdo imprime `livre`. Não consulta flock, PID nem Docker (`bin/trava_teste.sh:28`).
- A mensagem de timeout também lê `.quem` inteiro, usando `?` quando o `cat` falha (`bin/trava_teste.sh:37`).
- **O handoff não lê `--quem`.** Consulta diretamente os locks de teste e sombra com `flock -n "$_arq" true`; imprime “trava nunca criada”, “LIVRE” ou “TOMADA”, sem nome do detentor ou contêiner (`bin/handoff_sessao.sh:223-235`).
- Os hooks PreCompact chamam o handoff, e SessionStart `compact`/`clear` imprimem o Markdown gerado; portanto consomem indiretamente a classificação do flock, sem ler `.quem` (`.claude/settings.json:20`, `.claude/settings.json:29`, `.claude/settings.json:40`, `.claude/settings.json:53`).
- As sessões recebem orientação documental para consultar `--quem`; a documentação atual explica que `livre` responde pelo chamador e não garante ausência de suíte (`CLAUDE.md:203-205`). [nao verificado]
- O selo da trava lê o **código** para substituir a linha literal `TRAVA=/tmp/juliani_db_test.lock`, exercitando uma cópia com lock isolado; não lê o lock global (`bin/tests/test_trava_teste.sh:101-114`).
- Outros leitores/escritores executáveis em Python/hooks de `.quem` ou consumidores programáticos de `--quem`: **[não verificado]**; o censo textual não encontrou além dos sítios acima.

2. **Falhas concretas relevantes**

- O contrato atribui ao handoff a leitura de `--quem`, mas ele usa flock diretamente; corrigir somente `--quem` deixa o handoff chamando um recurso “LIVRE” mesmo com contêiner órfão. O caminho que publica LIVRE termina no sucesso do flock, sem Docker (`bin/handoff_sessao.sh:230-233`).
- O selo atual mede por **arquivo**: percorre somente `bin/*.sh`, ignora qualquer arquivo cujo caminho case `*test_*`, aceita a mera presença de `trava_teste.sh` em qualquer ponto e concede isenção por presença de `REGUA_DB`, salvo dois padrões literais de suíte completa (`bin/tests/test_trava_teste.sh:40-63`).
- Contraexemplo existente: o molde é julgado separadamente apenas pela presença textual de `trava_teste.sh`; um comando protegido pode aprovar o arquivo inteiro e esconder outro comando sem trava (`bin/tests/test_trava_teste.sh:70-71`).
- Contraexemplo sintético demonstrável pela lógica: um arquivo com `python manage.py test core`, `REGUA_DB` somente em outra linha e sem trava é dispensado; um arquivo com dois testes, dos quais só um usa a trava, também passa (`bin/tests/test_trava_teste.sh:54-62`).
- `--quem` pode publicar informação lateral antiga: não verifica que o PID escrito continua vivo nem que o flock esteja preso; sem `.quem`, responde livre independentemente do flock (`bin/trava_teste.sh:28`, `bin/trava_teste.sh:40-41`).
- Não há atualmente injeção de label, inspeção de órfãos nem parada de contêiner no corpo da trava; ela simplesmente executa `"$@"` após gravar `.quem` (`bin/trava_teste.sh:35-42`).

3. **Testes/selos presos ao texto ou forma** [nao verificado]

- O selo da trava depende da linha **exata** `TRAVA=/tmp/juliani_db_test.lock`; qualquer mudança de forma que impeça o `sed` aciona aborto antes dos testes de concorrência (`bin/tests/test_trava_teste.sh:103-114`).
- O mesmo selo exige `trava_teste.sh` textual no molde, usa os padrões literais `test api chamados` e `test ${ONLY:-$LABELS}`, e só prova serialização/timeout com comandos `bash`, `sleep` e `true`, sem Docker (`bin/tests/test_trava_teste.sh:59-71`, `bin/tests/test_trava_teste.sh:119-137`).
- O selo do handoff exige literalmente `flock -n`, `juliani_db_test.lock` e `sombra.lock`, proibindo o nome de `pgrep`; migrar o handoff integralmente para `--quem` exige adaptar esse selo (`bin/tests/test_handoff_sessao.sh:115-118`).
- O selo de `suite.sh` exige as substrings `trava_teste.sh`, `manage.py test`, uma regex específica de `LABELS=$(grep -oP ...regua.sh`, e ausência de `DB_PASSWORD` (`bin/tests/test_suite_sh.sh:50-58`).
- O selo de segredo exige literalmente `teste_envfile()` e `umask 177`; uma implementação equivalente com outra forma reprova (`bin/tests/test_segredo_fora_do_argv.sh:39-43`).
- Django exige em `isolamento.sh` as strings `desfazer_copia` e `exec bash "$script"` (`app/core/tests/test_arvore_vermelha.py:51-54`).
- O teste Django extrai a **primeira ocorrência** de `grep -oP '...'` do código de isolamento e executa a regex extraída contra fixture; adicionar antes outro `grep -oP` pode quebrar esse teste sem alterar a extração real dos IDs (`app/core/tests/test_arvore_vermelha.py:56-63`).
- Django exige em `vigia_arvore.sh` as strings `git archive HEAD app`, `SNAP=/tmp/vigia_arvore_commit_`, `sozinhos` e `alarme_arvore` (`app/core/tests/test_arvore_vermelha.py:65-73`).
- O selo de recursos junta continuações e identifica comandos pelas strings `docker run` e `manage.py test`, exigindo `TESTE_DOCKER` ou `--cpuset-cpus`; esconder o comando atrás de função muda o alcance desse censo (`bin/tests/test_recursos_sourced.sh:38-43`).
- O selo de cpuset procura contêineres cujo `.Path` + `.Args` contenham literalmente `manage.py test`; uma forma como `python manage.py` com subcomando montado por shell pode escapar da detecção (`bin/tests/test_cpuset_de_teste.sh:41-50`).
- O selo de ordem da fonte identifica `source` e chamadas das funções por regex de linha; aceita fonte e uso na mesma linha, como no placar (`bin/tests/test_fonte_unica_antes_do_uso.sh:38-48`).
- O teste Django do molde exige que `integrador_lote.roda_suite_completa()` retorne lista vazia ao ler `bin/molde_fatia/rodar.sh`; alterações de invocação precisam preservar a semântica reconhecida por esse juiz (`app/core/tests/test_suite_por_lote.py:81-87`).

## 31/10 17:39 · sessao 092d76fd4836 · - · - · origem=Ronald (tmux)

A1 cura o noturno comum do RED: a entrada de retorno 01h agora é datada pelo turno, e o mesmo conjunto de turnos alimenta `dia_das_batidas` e `dias_encerrados`. Entretanto, isso não garante equivalência para qualquer cadastro/batida: o painel usa `[hoje-2, hoje]`, o D4 recarrega `[data,data]`, e o espelho/calendário usam seu período apresentado. O próprio carregador declara que o pareamento depende da janela. (`app/core/services/painel_op.py:214-220`; `app/ponto/status_dia.py:175-176`; `app/ponto/services/espelho.py:462`; `app/ponto/services/espelho.py:625`; `app/colaboradores/services/calendario.py:301`; `app/colaboradores/services/calendario.py:697`; `app/ponto/turnos.py:2111-2114`.)

Contraexemplo passado construído: agora **17/09/2026 20h local**, vínculo ativo diurno **07–19**, intervalo **12–13**; batidas vivas **14/09 07h E** e **16/09 20h E com `is_intervalo=True`**, sem outras batidas. A flag é permitida pelo modelo e consumida antes das regras de borda; a guarda temporal exige `not _iv`, portanto não barra essa batida já marcada. O pareador produz turno aberto de **14/09**, contendo ambas as batidas. Esse resultado puro foi reproduzido em memória com `python3 -B`, sem arquivo ou banco; ocorrência em produção **[não verificado]**. (`app/ponto/models.py:26`; `app/ponto/turnos.py:1324`; `app/ponto/turnos.py:1347-1374`; `app/ponto/turnos.py:1495-1496`; `app/ponto/turnos.py:316-321`.)

Nesse cenário, o plural lê batidas somente de **15 a 17/09**: não recebe a entrada inicial; a entrada intervalada isolada é ignorada, por não haver turno corrente. O painel, porém, encontra a E de 16/09 nas últimas 24h e declara **encerrado**. Sua consulta de turnos lê a entrada antiga pelo pad, mas elimina o turno de 14/09 porque `data_turno` fica fora de `[15,17]`; `dia_das_batidas` atribui então a batida ao dia civil **16/09**. O espelho mensal atribui essa mesma batida a **14/09**, e o calendário mensal a recebe no turno de 14/09. Logo, **a janela de dois dias não cobre todo turno que uma entrada recente pode integrar**. (`app/ponto/turnos.py:2323-2326`; `app/ponto/turnos.py:1369-1374`; `app/core/services/painel_op.py:78`; `app/core/services/painel_op.py:83-85`; `app/core/services/painel_op.py:193-195`; `app/ponto/turnos.py:1857-1861`; `app/ponto/turnos.py:1973-1976`; `app/ponto/turnos.py:2884-2893`; `app/core/services/painel_op.py:217`; `app/core/services/painel_op.py:230`; `app/ponto/services/espelho.py:462`; `app/colaboradores/services/calendario.py:301-302`.)

Outro contraexemplo, **herdado**, do recorte dos candidatos: escala **24x48 07–07**, entrada ontem às 07h sem saída, consulta hoje às 08h, nenhuma outra batida. O juiz direto mantém aberta viva até **11h**, mas o corte de 24h do painel elimina colaborador e entrada, produzindo ausência de presença na linha e na cobertura. A troca singular→plural não cura esse recorte. (`app/ponto/turnos.py:1987-1994`; `app/ponto/turnos.py:2005-2011`; `app/core/services/painel_op.py:78`; `app/core/services/painel_op.py:83-99`; `app/core/services/painel_op.py:167-200`.)

O painel também não limita as entradas por `agora`: passa somente `ini`, sem `fim`; candidatos seguem o mesmo corte sem teto superior. Uma E futura depois de amanhã pode ser selecionada como entrada “encerrada”, ficar sem mapeamento na janela e receber `data_turno=hoje`. Existência desse dado em produção **[não verificado]**. (`app/core/services/painel_op.py:83-85`; `app/core/services/painel_op.py:94-96`; `app/ponto/turnos.py:2844-2852`; `app/core/services/painel_op.py:193-220`; `app/core/services/painel_op.py:230`.)

Censo das portas, por chamadas diretas de produção encontradas:

- **`status_do_dia_lavrado`**: painel, exclusivamente nesse censo. (`app/core/services/painel_op.py:220`.)
- **`turnos_abertos_de` de `ponto`**: painel e situacional; o homônimo de chamados é outra função. (`app/core/services/painel_op.py:98`; `app/colaboradores/services/situacional.py:169`; `app/chamados/reconciliador.py:82`.)
- **`dias_encerrados`**: calendário, painel, cartório, D4, emissão retroativa e comando suprajuiz. (`app/colaboradores/services/calendario.py:697`; `app/core/services/painel_op.py:218`; `app/ponto/services/cartorio.py:910`; `app/ponto/status_dia.py:176`; `app/ponto/management/commands/emitir_furo_retroativo.py:62`; `app/ponto/management/commands/supra_juiz.py:108`.)
- **`dia_das_batidas`**: API, painel e espelho. O PDF deixou de perguntar diretamente e desenha a resposta da tela. (`app/api/views.py:1446`; `app/api/views.py:2090`; `app/core/services/painel_op.py:217`; `app/ponto/services/espelho.py:462`; `app/relatorios/pdf_espelho.py:529-533`.)
- **`turnos_do_colab`, leitores principais**: API; juízes/modelo/detector/materialização de chamados; calendário e drawer; painel; escala automática, impacto de troca, serviço de jornada e grade; núcleo, espelho, regularização, triagem, verificadores e views; PDF e comandos. (`app/api/views.py:1449`; `app/chamados/juizes.py:369`; `app/chamados/juizes.py:1727`; `app/chamados/models.py:1260`; `app/chamados/detector_anomalias.py:155`; `app/chamados/services/materializacao.py:109`; `app/chamados/services/materializacao.py:204`; `app/chamados/services/disputa_emissao.py:644`; `app/colaboradores/services/calendario.py:301`; `app/colaboradores/services/drawer.py:45`; `app/core/services/painel_op.py:216`; `app/escala/services/escala_auto.py:87`; `app/escala/services/impacto_troca.py:18`; `app/escala/servico_jornada.py:250`; `app/escala/servico_jornada.py:392`; `app/escala/utils.py:1524`; `app/escala/utils.py:1838`; `app/ponto/nucleo.py:47`; `app/ponto/services/espelho.py:625`; `app/ponto/services/regularizacao.py:61`; `app/ponto/services/triagem_batida.py:552`; `app/ponto/views.py:377`; `app/ponto/views.py:1586`; `app/ponto/views.py:3167`; `app/relatorios/pdf_espelho.py:213`; `app/relatorios/pdf_espelho.py:447`.)
- **`bin/`**: referências de cadastro/censo, sem chamada direta dessas cinco portas encontrada no censo AST; os sítios textuais relevantes são estes. (`bin/esmeril_turno_censo.py:63`; `bin/esmeril_vinculo_censo.py:45`.)

O caminho plural usa o mesmo corpo puro do singular e o mesmo recorte civil de três dias; não nasce um juiz novo nessa troca. O painel restringe previamente seus candidatos às últimas 24h, enquanto o serviço situacional passa sua lista de colaboradores. (`app/ponto/turnos.py:2176-2195`; `app/ponto/turnos.py:2266-2276`; `app/ponto/turnos.py:2330-2334`; `app/core/services/painel_op.py:94-98`; `app/colaboradores/services/situacional.py:169`.)

## 31/10 17:38 · sessao 2e4fc01188d4 · - · - · origem=Ronald (tmux)

Auditoria estática; testes não executados, resultados RED/GREEN e contagens reais [não verificado]. Os testes do medidor criam diretórios e arquivos temporários (`app/core/tests/test_e4_7iii_painel_e_presenca.py:182-195`); o selo shell de juiz também pode gravar a base e cria temporário (`bin/tests/test_juiz_novo_tem_corte.sh:31-34,47-60`).

**A2 não elimina N+1:** filtro vem antes do D4 (`app/core/services/painel_op.py:202-220`) e turno vivo usa plural (`:93-100`), mas, para cada encerrado preservado, há `turnos_do_colab`, `dia_das_batidas`, `dias_encerrados` e potencial D4 (`:214-220`). `turnos_do_colab` consulta batidas/células/escalas (`app/ponto/turnos.py:1858-1886`); `dias_encerrados` recarrega células/escalas mesmo recebendo turnos (`:2129-2132`); D4 consulta célula e recarrega turnos/dias numa janela singleton (`app/ponto/status_dia.py:171-176`). A quantidade medida não está no dado entregue [não verificado].

**Mordida individual dos 16 testes**, por inspeção das asserções — execução [não verificado]: [nao verificado]

| Teste (sufixo) | O que derruba; limite | [nao verificado]
|---|---|
| `dia_de_trabalho_antes...` | Retorno de `ausente`, perda de `previsto`, posto descoberto antes de começar: asserções diretas (`app/core/tests/test_e4_7iii_painel_e_presenca.py:88-98`). |
| `folga_de_hoje...` | Retorno de `status='folga'` ou perda de previsão folga (`:100-104`). |
| `contadores_e_filtros...` | Reintrodução das chaves removidas no retorno do serviço; não renderiza filtro/card (`:106-114`). |
| `encerrado_de_turno_antes...` | Exposição do D4 antes do fim previsto; isoladamente passa se todo D4 for removido, por exigir `None` (`:120-131`). |
| `retratada_unica...` | Volta da consulta crua da atividade: espiona o argumento `teve_atividade`; par vivo impede apagar todas (`:133-157`). |
| `dia_encerrado_mostra...` | Remover D4 ou devolver status/fonte diferente; contrapartida positiva ao teste anterior (`:163-172`). | [nao verificado]
| `painel_e_PRESENCA...` | Remover categorias/exclusões ou permitir novas derivações na cópia mínima; não valida semântica de presença (`:198-209`). |
| `presenca_que_volta...ausente` | Muta literal no código copiado e exige retorno à catraca; verifica detector, não execução do painel (`:211-215`). |
| `presenca_que_volta...folga` | Idem, literal folga (`:217-221`). |
| `previsao_que_diz...` | Idem, previsto→ok (`:223-227`). |
| `noturno_com_pausa...` | Retorno à data civil da volta da pausa e perda do D4 do turno: exige data D, `ok`, `lavrado` (`:241-266`). |
| `filtro_que_exclui...` | Reintrodução de custo por encerrado antes do filtro ou de juiz singular por candidato: compara 3/6 encerrados, filtro normal (`:299-305`). |
| `turno_vivo_em_lote...` | Custo adicional por linha fora das quatro funções mockadas; deixa explicitamente todo D4 sem fiscalização de crescimento (`:292-297,307-318`). |
| **`MEDIDA_residuo...`** | **Não é tripwire da cura: só exige `n6>=n3`. Remover todo D4 permite `n6==n3` e passa; crescimento arbitrariamente maior também passa** (`:320-329`). |
| `dois_previstos_um_presente...` | Reintroduzir condição cobertura descoberto tira botão do ausente parcial; par presente impede botão indiscriminado (`:335-362`). |
| `status_ausente_ou_folga...` | Remover normalização faz conjuntos diferentes; caso encerrado preserva filtro válido. Não testa GET/contexto/HTMX (`:371-384`). |

**ALLOWLIST / LOOKUP_ALLOWLIST.** Escritor das duas é o próprio módulo de teste, via literais (`app/ponto/tests/test_contract_no_batida_date.py:10-72,98-154`). Leitores: detector novos `.date()` (`:233-236`), novos lookups (`:238-245`), entradas podres de cada lista (`:247-278`). Remover painel da primeira é coerente com a troca de `data_turno` para juiz (`app/core/services/painel_op.py:217,230`); manter na segunda corresponde aos lookups efetivos de geofence e atividade (`:104,274`), com categoria residente painel hoje (`app/ponto/tests/test_contract_no_batida_date.py:126-129`). O detector `.date()` segue referências sintáticas a timestamp, wrappers e variáveis locais (`:179-206`); não é prova semântica de todo caminho indireto.

**Autoridades e duplicação.** Registro vivo manda dia da batida para `turnos_do_colab`, turno aberto para `turno_aberto_de`, dia encerrado para `dia_encerrado`, status para D4 (`app/core/juizes.py:173,177,180,204`). Plural declara carregar dados para o mesmo `_turno_aberto_puro` e o chama, sem novo predicado (`app/ponto/turnos.py:2266-2275,2330-2334`). `dia_das_batidas` adapta turnos recebidos e também atribui dia civil à batida não reclamada — vale examinar esse segundo ramo no universo do painel (`:2855-2878`). O histórico de sua declaração indevida como segundo juiz é documentado no selo, sem provar erro novo desta chamada (`bin/tests/test_juiz_novo_tem_corte.sh:6-11`).

**Medidor não garante fonte única.** Categoria só testa palavras permitidas, não de qual autoridade derivaram (`app/core/medida_leitor_do_dia.py:366-375`). A exclusão de painel permite presença com seus literais e a de previsão permite três palavras (`:269-307`); uma segunda derivação com as mesmas palavras passa por esse predicado. O instrumento declara cegueira para indireção, booleanos, templates/JS e decisões que reutilizam quantidade de literais (`:334-343`). Previsão ternária continua em dois lugares (`app/escala/views.py:54`; `app/core/services/painel_op.py:35`), ambas perguntando `EC.eh_dia_trabalho` (`:40`; `:34`), sem novo juiz de aritmética demonstrado por esse ternário.

**Escritor/relógio/trava.** Mudanças produtivas do painel constroem dicionários e retornam contexto; não introduzem gravação de estado ou bloqueio transacional (`app/core/services/painel_op.py:225-241,294-315`). Relógio é capturado uma vez na borda do serviço (`:53-54`) e passado ao plural/encerramento/D4 (`:98,219-220`); os defaults internos `timezone.now()` não são usados por essas chamadas explícitas (`app/ponto/turnos.py:2124-2125,2295-2296`; `app/ponto/status_dia.py:169-170`). A view de minha escala conserva `datetime.date.today()` (`app/escala/views.py:26`), e não há caso UTC/local cruzando dia nos freezes novos (`app/core/tests/test_e4_7iii_painel_e_presenca.py:85,117,160,238,269,332,365`).

Pré-requisitos estão presentes na cópia: D4 (`app/ponto/status_dia.py:157`), plural (`app/ponto/turnos.py:2266`), categorias previsão/presença (`app/core/medida_leitor_do_dia.py:266-274`) e respectivas exclusões (`:297-307`). Ordem histórica de pouso, RED antes de cura e passagem das travas [não verificado]; a cópia não possui metadados Git utilizáveis [não verificado].

**Leitores adicionais das guardas:** suíte consome o medidor em `app/core/tests/test_medida_leitor_do_dia.py:29`, verifica novos/sumidos/sítios/exclusões em `:39-55,73-83`; host carrega o mesmo módulo e publica sua lista e alarmes em `bin/leitor_dia_estado.py:25-35,46-69`; selo host exige medidor único e testa catraca/plantas (`bin/tests/test_leitor_dia_estado.sh:25-43,45-71`). Core integra labels da suíte (`bin/regua.sh:23`).

## 31/10 17:37 · sessao fe7ba70ebfc3 · - · - · origem=Ronald (tmux)

A3 e A4 têm cura no caminho pedido: Designar usa `previsto_agora and not status`, sem depender da cobertura; filtro desconhecido vira `todos` antes de recortar linhas. A execução dos testes está [não verificado]. (`app/templates/core/_painel_card.html:69`; `app/core/services/painel_op.py:64`; `app/core/services/painel_op.py:202`)

Mapa de escritores/leitores:

| Campo | Escritor | Leitores |
|---|---|---|
| `status` | Ramos `normal/geofence/turno_aberto/encerrado/None`, `painel_op.py:172–200`; publicação `:232` | Filtro `:202`, contadores `:245–248`, agregado geofence `:288`, ordenação `:296`; card `:6`, `:36`, `:54`, `:66–69` |
| `previsao` | `_previsao_de_hoje`, `painel_op.py:27–35`; publicação `:239` | Avatar e badge, `_painel_card.html:54`, `:66–67`; novos testes `test_e4_7iii_painel_e_presenca.py:95`, `:104` | [nao verificado]
| `dia` | `None`, depois D4 sob portão, `painel_op.py:212–220`; publicação `:240` | Tooltip, texto do badge, `_painel_card.html:66–67`; testes `test_e4_7iii_painel_e_presenca.py:131`, `:170–172`, `:264–266` | [nao verificado]
| `data_turno` | Turno vivo, depois mapa do juiz, fallback hoje, `painel_op.py:230` | Testes `test_painel_op_turno_aberto_ancora.py:50`, `test_painel_op_turno_pelo_juiz.py:80`, novo `test_e4_7iii_painel_e_presenca.py:263`; consumidor de produção fora do serviço não identificado [não verificado] | [nao verificado]
| `filtro_status` | GET página `core/views.py:627`; GET lote `:609`; normalização serviço `painel_op.py:64–65` | Serviço `:202`, `:284`; contexto da página mantém valor bruto `core/views.py:669`; lotes preservam GET em `_qs_sem_offset_pn`, `:580–585` |
| `statuses` | `painel_op.py:56–61` | Validação `:64`, retorno `:314`; view `core/views.py:639`, contexto `:672`; uso no template atual não identificado [não verificado] |
| `contadores` | `painel_op.py:244–249` | `core/views.py:637`, `:670`; `_painel_stats.html:32–35`, incluído pela página `painel_operacional.html:122` e fragmento `_painel_fragment.html:2–3` |

Os caminhos de produção que chamam o serviço são a página e o lote; o lote reconstrói o painel inteiro e só fatia os postos depois. (`app/core/views.py:607`; `app/core/views.py:632`; `app/core/views.py:610–615`)

Os `data-*` do card:

- `data-search` recebe posto/empresa; busca lê `dataset.search`. (`app/templates/core/_painel_card.html:3`; `app/templates/core/painel_operacional.html:198`)
- `data-cobertura`, `data-tem-geofence`, `data-tem-aberto` recebem os agregados/status; filtro JS lê os três. (`app/templates/core/_painel_card.html:4–6`; `app/templates/core/painel_operacional.html:176–182`)
- Designar publica titular ID/nome, posto ID/nome e empresa ID; JS copia para `estado`. (`app/templates/core/_painel_card.html:69`; `app/templates/core/painel_operacional.html:280–285`)
- GET de disponíveis envia titular, posto e janela; endpoint lê esses parâmetros. (`app/templates/core/painel_operacional.html:343`; `app/ponto/views.py:3065–3068`)
- POST envia titular, substituto, posto, escala e janela; view delega à porta `designar_cobertura`. Empresa ID não integra esse POST. (`app/templates/core/painel_operacional.html:410–421`; `app/ponto/views.py:3027–3038`)
- O botão não transporta `data_turno`; a janela nasce do relógio do navegador. (`app/templates/core/_painel_card.html:69`; `app/templates/core/painel_operacional.html:293–301`)

Resíduos concretos fora do diff:

1. **Filtro URL válido desaparece no refresh:** página aceita `?status=encerrado`, mas `hx-get` periódico inclui apenas `fragment` e empresa; após atualização, o serviço recebe `todos`. (`app/core/views.py:627`; `app/templates/core/painel_operacional.html:147`; `app/core/views.py:627`)
2. **Dois abertos no mesmo posto somem do filtro Aberto:** template produz `"11"` e JS exige exatamente `"1"`. (`app/templates/core/_painel_card.html:6`; `app/templates/core/painel_operacional.html:178`)
3. **Lotes chegam fora do filtro/busca ativo:** reaplicação acontece somente quando `afterSwap.target.id === 'pop-body'`; sentinela dos lotes usa `hx-target="this"`. (`app/templates/core/painel_operacional.html:161–166`; `app/templates/core/_painel_lote.html:7–8`)
4. **Janela de Designar mistura data UTC e hora local:** `toISOString().slice(0,10)` fornece a data e `toTimeString()` a hora. Exemplo deduzido: abrir às 22h de 31/10 em UTC−3 gera início padrão 23h com data UTC 01/11, deslocando um dia; ocorrência real [não verificado]. (`app/templates/core/painel_operacional.html:294–301`)
5. **Contexto não reflete a normalização A4:** o serviço usa `todos`, enquanto `filtro_status` no contexto e `qs_lotes` continuam com o parâmetro bruto; não há leitor atual identificado que torne isso quebra visual [não verificado]. (`app/core/services/painel_op.py:64–65`; `app/core/views.py:669`; `app/core/views.py:580–585`)
6. **Selo antigo de crescimento não ganhou encerrados:** fixture de 45 e mais 100 cria colaboradores/escalas, sem batidas; portanto não exercita D4 por linha. (`app/core/tests/test_selo_lotes_painel.py:41–53`; `app/core/tests/test_selo_lotes_painel.py:95–106`)

Instrumentos fora do diff:

- Registro vivo mantém pendências dos quatro contadores, badge de aberto, `data-tem-aberto` e filtro JS; não foram declaradas curadas. (`app/core/juizes.py:715–716`; `:727–732`)
- `bin/leitor_dia_estado.py` carrega o mesmo medidor e chama `censo`; `bin/gerar_estado.py` usa seu resultado. (`bin/leitor_dia_estado.py:25–35`; `bin/gerar_estado.py:284–291`)
- `bin/sitios_por_pergunta.py` lê `core/juizes.py` por AST para produzir o inventário. (`bin/sitios_por_pergunta.py:22`; `:32–43`)
- O esmeril de batida não inclui `painel_op.py` na superfície de apuração, portanto não constitui proteção dessa leitura no painel. (`bin/esmeril_batida_censo.py:79–87`)
- O tripwire de contagem de pendência inclui o construtor do painel entre os leitores obrigados a usar `qs_na_fila`. (`app/core/tests/test_tripwire_contagem_de_pendencia.py:44–58`)

ALLOWLIST/LOOKUP_ALLOWLIST:

- A primeira controla `.date()` sobre timestamp via visitante AST; novos ofensores e entradas podres têm testes distintos. A retirada de `painel_op.py` é coerente com a troca por `dia_das_batidas`. (`app/ponto/tests/test_contract_no_batida_date.py:43–45`; `:187–217`; `:233–236`; `:247–264`)
- A segunda controla lookups ORM `timestamp__date`; painel permanece residente para “hoje”, compatível com os lookups geofence/atividade ainda existentes. (`app/ponto/tests/test_contract_no_batida_date.py:126–129`; `:157–175`; `app/core/services/painel_op.py:104`; `:274`)
- Novos lookups e entradas podres têm asserções próprias; não foi ampliada a exceção do painel no diff fornecido. (`app/ponto/tests/test_contract_no_batida_date.py:238–245`; `:266–278`)

## 31/10 17:09 · sessao 45436fe12f28 · E4-7-SAIDA-III · verificador · origem=Code

Encontrei três problemas relevantes: **D4 consultado pela data errada no noturno**, **N+1 novo nas linhas encerradas** e **perda do botão “Designar” em cobertura parcial**. As causas estão em `app/core/services/painel_op.py:194`, `app/core/services/painel_op.py:195` e `app/templates/core/_painel_card.html:69`.

Não há casos individuais de produção no material fornecido; incidência e impacto em produção são **[nao verificado]**. Os contraexemplos abaixo são deduções do código, sem execução contra banco **[nao verificado quanto à reprodução]**.

**1. Quem escreve e quem lê**

| Campo/valor | Quem escreve ou fornece | Quem lê |
|---|---|---|
| `status`: `normal`, `geofence`, `turno_aberto`, `encerrado`, `None` | O próprio painel escolhe as palavras: presença pelo juiz, alerta geo e comparação com fim previsto; `encerrado` significa entrada encontrada sem turno vivo. `app/core/services/painel_op.py:85`, `app/core/services/painel_op.py:158`, `app/core/services/painel_op.py:168`, `app/core/services/painel_op.py:181`. | Filtro, contadores, agregação e ordenação: `app/core/services/painel_op.py:198`, `app/core/services/painel_op.py:224`, `app/core/services/painel_op.py:273`, `app/core/services/painel_op.py:281`. Card, avatar, badge, indicador aberto e Designar: `app/templates/core/_painel_card.html:6`, `app/templates/core/_painel_card.html:36`, `app/templates/core/_painel_card.html:54`, `app/templates/core/_painel_card.html:67`, `app/templates/core/_painel_card.html:69`. |
| `previsao`: `folga`, `escala_suspeita`, `previsto`, `None` | `_previsao_de_hoje` traduz `EC.eh_dia_trabalho`; este lê `CelulaDia.trabalha`, com fallback aritmético. `app/core/services/painel_op.py:27`, `app/core/services/painel_op.py:218`, `app/escala/models.py:1066`. | Avatar e fallback do badge; testes novos. `app/templates/core/_painel_card.html:54`, `app/templates/core/_painel_card.html:67`, `app/core/tests/test_e4_7iii_painel_e_presenca.py:95`. |
| `dia` | Painel inicializa `None`, pergunta encerramento e chama D4. D4 lê a célula; o escritor persistente é a porta de lavra. `app/core/services/painel_op.py:192`, `app/ponto/status_dia.py:171`, `app/ponto/portas/celula.py:705`, `app/ponto/portas/celula.py:709`. | Card lê `rotulo` e `status`; teste também verifica `fonte`. `app/templates/core/_painel_card.html:66`, `app/templates/core/_painel_card.html:67`, `app/core/tests/test_e4_7iii_painel_e_presenca.py:171`. |
| `contadores` | Quatro somas sobre as linhas **já filtradas**. `app/core/services/painel_op.py:198`, `app/core/services/painel_op.py:223`. | View repassa; stats lê quatro chaves; teste verifica a remoção das antigas. `app/core/views.py:637`, `app/core/views.py:670`, `app/templates/core/_painel_stats.html:32`, `app/core/tests/test_e4_7iii_painel_e_presenca.py:109`. |
| `statuses` | Lista com quatro opções. `app/core/services/painel_op.py:230`. | View lê e repassa ao contexto. `app/core/views.py:639`, `app/core/views.py:672`. Consumidor final adicional **[nao verificado]**. |
| `ausentes` do posto | Removido; o dicionário atual não publica essa chave. `app/core/services/painel_op.py:279`. | Card atual usa `em_turno` e `geofence`; teste exige ausência da chave. `app/templates/core/_painel_card.html:12`, `app/core/tests/test_e4_7iii_painel_e_presenca.py:112`. |
| `bateu_hoje_ids` | `batidas_apuraveis(None)` filtradas pela data; a autoridade exclui retratadas. `app/core/services/painel_op.py:258`, `app/ponto/turnos.py:2845`. | `_teve_ativ`, depois `classificar_cobertura`. `app/core/services/painel_op.py:276`. |
| `PALAVRAS_DA_PRESENCA` | Constante com as quatro palavras. `app/core/medida_leitor_do_dia.py:272`. | `PALAVRAS_DA_CATEGORIA`, predicado do censo e teste novo. `app/core/medida_leitor_do_dia.py:274`, `app/core/medida_leitor_do_dia.py:367`, `app/core/tests/test_e4_7iii_painel_e_presenca.py:207`. |
| `NAO_E_LEITOR` | Registro de exclusões; inclui as duas funções do painel. `app/core/medida_leitor_do_dia.py:275`, `app/core/medida_leitor_do_dia.py:302`. | Censo aplica categoria/motivo e procura exclusões mortas; selo verifica categorias e sobreposição. `app/core/medida_leitor_do_dia.py:366`, `app/core/medida_leitor_do_dia.py:403`, `app/core/tests/test_medida_leitor_do_dia.py:73`. |
| `TETO_NAO_E_LEITOR` | Constante `10`. `app/core/medida_leitor_do_dia.py:309`. | Selo exige igualdade com o tamanho da lista. `app/core/tests/test_medida_leitor_do_dia.py:75`. |
| `DECLARADOS` | Cinco funções: duas do calendário, API, espelho administrativo e furos. `app/core/medida_leitor_do_dia.py:316`. | Censo calcula novos/sumidos/sítios alterados; selo cobra os resultados. `app/core/medida_leitor_do_dia.py:395`, `app/core/tests/test_medida_leitor_do_dia.py:39`, `app/core/tests/test_medida_leitor_do_dia.py:50`. |

Página, fragmento e lote passam pelo mesmo construtor; grade e lote incluem o mesmo card. `app/core/views.py:607`, `app/core/views.py:632`, `app/core/views.py:679`, `app/templates/core/_painel_grid.html:2`, `app/templates/core/_painel_lote.html:7`.

Fora do diff, testes continuam lendo presença, entrada e âncora: `app/core/tests/test_painel_op_turno_pelo_juiz.py:75`, `app/core/tests/test_painel_op_turno_aberto_ancora.py:49`, `app/core/tests/test_e4_7_painel_entrada_retratada.py:80`.

O publicador do ESTADO carrega **o mesmo medidor**, publica contagem, lista, alarmes e limitações; não calcula outro censo. A S3 também importa seu detector de dinheiro. `bin/leitor_dia_estado.py:32`, `bin/leitor_dia_estado.py:46`, `bin/gerar_estado.py:290`, `app/ponto/tests/test_s3_placar_exercicio.py:38`.

`core/juizes.py` não consome o payload: mantém pendências de contadores, badges e filtros por impressões do código. A obra alterou descrições, sem retirar essas pendências. `app/core/juizes.py:715`, `app/core/juizes.py:727`, `app/core/juizes.py:731`.

**2. Leitores que ainda esperam o contrato antigo**

Nos consumidores atuais rastreados, não encontrei expectativa residual de `status='ausente'`, `status='folga'` ou `p.ausentes`: o badge consulta `previsao`, os stats usam quatro contadores e o JS filtra descoberto/geofence/aberto. `app/templates/core/_painel_card.html:67`, `app/templates/core/_painel_stats.html:32`, `app/templates/core/painel_operacional.html:173`.

**Permanece uma incompatibilidade nas URLs:** ambas as views aceitam `?status=ausente` ou `?status=folga` e passam o valor sem normalização. O serviço elimina todas as linhas por igualdade e depois omite os postos vazios. Favoritos ou integrações reais usando essas URLs são **[nao verificado]**. `app/core/views.py:607`, `app/core/views.py:626`, `app/core/services/painel_op.py:198`, `app/core/services/painel_op.py:269`.

Não identifiquei API consumidora desse construtor. A API do espelho continua com classificação própria, declarada separadamente no medidor; consumidor externo do payload do painel é **[nao verificado]**. `app/core/medida_leitor_do_dia.py:323`.

**3. Classes de reprovação**

- **Segundo julgamento da data do turno:** a nova leitura do D4 usa a data civil da última entrada, enquanto o juiz registrado para pertencimento da batida é `turnos_do_colab`. No turno fechado, o painel também publica essa data civil em `data_turno`. `app/core/services/painel_op.py:194`, `app/core/services/painel_op.py:209`, `app/core/juizes.py:177`.

- **Escolha de vínculo fora da autoridade:** o painel seleciona por `ativa=True`, sem consultar vínculo do dia ou cobertura da data. Isso alimenta previsão, cobertura e fim previsto, embora essas autoridades estejam registradas. É caminho herdado, agora também alimentando o campo novo. `app/core/services/painel_op.py:97`, `app/core/services/painel_op.py:132`, `app/core/services/painel_op.py:218`, `app/core/juizes.py:1255`.

- **Previsão duplicada:** helper e `minha_escala` repetem a tradução True/False/None. Ambos perguntam trabalho à EC; portanto não há segunda aritmética de escala nesse ternário, mas existem duas traduções e escolhas independentes do vínculo. `app/core/services/painel_op.py:34`, `app/escala/views.py:25`, `app/escala/views.py:40`, `app/escala/views.py:54`.

- **Segundo escritor persistente:** não identifiquei novo escritor no caminho alterado. O painel monta dicionários e D4 consulta dados. O `.update(status_dia=...)` novo pertence à fixture, não ao serviço. `app/core/services/painel_op.py:192`, `app/ponto/status_dia.py:171`, `app/core/tests/test_e4_7iii_painel_e_presenca.py:76`.

- **Relógio:** a cadeia nova recebe o mesmo `agora` capturado pelo construtor; não encontrei leitura independente nova nesses chamados. Permanecem relógios do navegador e `date.today()` em `minha_escala`; discrepância por fuso do processo é **[nao verificado]**. `app/core/services/painel_op.py:53`, `app/core/services/painel_op.py:195`, `app/templates/core/painel_operacional.html:155`, `app/templates/core/painel_operacional.html:294`, `app/escala/views.py:26`.

- **N+1 novo, confirmado pela estrutura:** cada linha encerrada chama `dias_encerrados` sem alimentação; quando passa, D4 consulta célula, recarrega turnos e chama novamente `dias_encerrados`. Isso acontece antes do filtro de status. Quantidade e latência reais são **[nao verificado]**. `app/core/services/painel_op.py:193`, `app/core/services/painel_op.py:198`, `app/ponto/turnos.py:2129`, `app/ponto/status_dia.py:171`.

- **Selo de custo não cobre esse ramo:** as fixtures de crescimento criam vínculos sem batidas, portanto não exercitam linhas encerradas. Já existia outro N+1, chamando presença por colaborador, apesar da porta plural disponível. `app/core/tests/test_selo_lotes_painel.py:41`, `app/core/tests/test_selo_lotes_painel.py:95`, `app/core/services/painel_op.py:85`, `app/ponto/turnos.py:2266`.

Os REDs novos cobram as trocas literais `encerrado→ausente`, `encerrado→folga` e `previsto→ok`. Contudo, o medidor não verifica a origem dos campos: só examina certas produções de literais reconhecidos. `app/core/tests/test_e4_7iii_painel_e_presenca.py:211`, `app/core/medida_leitor_do_dia.py:194`, `app/core/medida_leitor_do_dia.py:200`.

Executei o censo somente em memória: **5 = status 5 + valor 0**, sem alarmes. Também mantiveram esse resultado duas alterações exclusivamente em memória: publicar `status` por `{'resultado': 'falta'}['resultado']` e substituir D4 por `{'status': 'falta', 'fonte': 'paralelo'}`. A limitação decorre do tratamento de dicionários/indireção; isso demonstra limite do medidor, não aprovação desses mutantes pela suíte completa. `app/core/medida_leitor_do_dia.py:200`, `app/core/medida_leitor_do_dia.py:335`, `app/core/medida_leitor_do_dia.py:368`, `app/core/medida_leitor_do_dia.py:389`.

**4. Contraexemplos concretos**

Todos abaixo são cenários sintéticos inferidos do código; reprodução executada e ocorrência em produção são **[nao verificado]**.

| Cadastro + batidas + hora | Diferença resultante |
|---|---|
| Ativo, 12x36 **19–07**, intervalo **00–01**, âncora 17/09; E17/09 19h, S18/09 00h, E18/09 01h, S18/09 07h; consulta em **18/09 08h**; D4 de 17/09 lavrado `ok`. | Painel pega a E de retorno, consulta encerramento de **18/09** e omite o D4 de **17/09**. A janela consultada termina em 19/09 07h; calendário agrupa o turno em 17/09. `app/core/services/painel_op.py:72`, `app/core/services/painel_op.py:194`, `app/escala/servico_jornada.py:61`, `app/ponto/turnos.py:2102`, `app/colaboradores/services/calendario.py:301`. |
| Ativo, EC 12x36 **07–19**, início **18/09**, âncora 17/09; sem célula nem batidas; consulta **17/09 09h**. | Painel anuncia `previsto` e conta previsão agora, embora o vínculo ainda não cubra a data. Calendário aplica a guarda de vigência e fica sem previsão. `app/core/services/painel_op.py:34`, `app/core/services/painel_op.py:139`, `app/escala/models.py:1260`, `app/colaboradores/services/calendario.py:766`. |
| Ativo, **07–19**, única E17/09 07h, sem S; consulta **17/09 23h01**. | Presença expira após fim+4h: painel diz `encerrado`. Calendário ainda lê `t.aberto` e diz `aberto`. Assim, `encerrado` não prova saída registrada. `app/ponto/turnos.py:1987`, `app/ponto/turnos.py:2008`, `app/core/services/painel_op.py:181`, `app/colaboradores/services/calendario.py:811`, `app/colaboradores/services/calendario.py:144`. |

A diferença de âncora noturna não está coberta pelo teste existente: ele termina sem a S final; os testes novos de D4 são diurnos. `app/core/tests/test_painel_op_turno_aberto_ancora.py:41`, `app/core/tests/test_e4_7iii_painel_e_presenca.py:120`, `app/core/tests/test_e4_7iii_painel_e_presenca.py:163`.

**5. O que quebra ou permanece quebrado fora do diff**

- **Designar em cobertura parcial — regressão:** dois previstos 07–19, às09 um presente e outro sem batida. O juiz retorna `parcial`, o painel converte para `coberto` e o botão exige `descoberto`; desaparece a ação para o titular sem presença. `app/ponto/selecao_periodo.py:246`, `app/core/services/painel_op.py:261`, `app/templates/core/_painel_card.html:69`.

- **Designar após saída antecipada:** E07/S08, consulta às09, jornada até19. O titular continua previsto e o posto pode estar descoberto, mas `status='encerrado'` impede o botão por `not c.status`. `app/core/services/painel_op.py:181`, `app/core/services/painel_op.py:217`, `app/templates/core/_painel_card.html:69`.

- **Filtro “Aberto” perde postos com dois abertos:** o template gera `data-tem-aberto="11"`; JS aceita somente `'1'`. `app/templates/core/_painel_card.html:6`, `app/templates/core/painel_operacional.html:178`.

- **Lotes novos escapam do filtro/busca:** a reaplicação só ocorre quando o alvo do swap é `pop-body`; sentinelas de lote substituem a si mesmas. `app/templates/core/painel_operacional.html:161`, `app/templates/core/_painel_lote.html:8`.

- **KPI “sem escala” mistura universos:** denominador conta todos os postos ativos; numeradores vêm do painel filtrado; o residual inclui encerrados e postos fora da empresa selecionada. `app/core/views.py:648`, `app/core/views.py:651`, `app/templates/core/_painel_stats.html:24`.

- **O dashboard ainda deriva ausentes separadamente:** `ausentes_hoje` não é consumidor da chave removida; continua sendo outra classificação por trabalho e ausência de entrada. A TV também mantém atividade por batidas cruas e previsão por hora do template. `app/core/views.py:388`, `app/core/views.py:404`, `app/core/views.py:792`, `app/core/views.py:848`.

- **Selo host inconsistente por construção:** a cópia mínima não inclui `ponto/status_dia.py`, agora obrigatório para carregar o vocabulário. Ela produz indisponibilidade, e depois a expressão aritmética recebe `NB=NAO`. É dedução estática; execução do script **[nao verificado]**. `bin/tests/test_leitor_dia_estado.sh:46`, `app/core/medida_leitor_do_dia.py:162`, `bin/leitor_dia_estado.py:35`, `bin/tests/test_leitor_dia_estado.sh:59`.

A suíte não foi executada: suas fixtures criam registros e arquivos temporários, incompatíveis com a restrição desta verificação. Resultado da suíte e REDs executados são **[nao verificado]**. `app/core/tests/test_e4_7iii_painel_e_presenca.py:45`, `app/core/tests/test_e4_7iii_painel_e_presenca.py:183`.

## 31/10 17:07 · sessao 9ff881fc009d · - · - · origem=Ronald (tmux)

Achados por leitura estática; execução, incidência e impacto em produção: **[nao verificado]**.

1. **P1 — D4 consultado no dia errado após intervalo noturno.** `entradas_hoje` conserva a última E cronológica, inclusive retorno de intervalo; a nova consulta usa `localdate(entrada.timestamp)` como dia. Não pergunta ao juiz o `data_turno` do turno fechado. O próprio campo `data_turno` repete esse fallback quando não existe turno vivo. (`app/core/services/painel_op.py:72-75,193-196,209`; ordenação da fonte: `app/ponto/turnos.py:2852`.)

   Contraexemplo: ativo, 12x36 19–07, intervalo 00–01, âncora 17/09; E17/09 19h, S18/09 00h, E18/09 01h, S18/09 07h; agora 18/09 08h; célula17 lavrada `ok`. O painel escolhe E18/09 01h e consulta encerramento de **18/09**, cuja janela noturna termina em **19/09 07h**; `dia=None`. O calendário agrupa esse turno em **17/09**. Portanto o painel omite o D4 do turno encerrado e apresenta entrada/data ancoradas no retorno do intervalo. É inferência estática, não reprodução executada. (`app/core/services/painel_op.py:155,181-196,207-209`; `app/escala/servico_jornada.py:61-93`; `app/ponto/turnos.py:2102-2105`; `app/colaboradores/services/calendario.py:297-302`.)

   A geometria dessa fixture já aparece no teste de âncora noturna, mas ele termina sem S final; os novos testes D4 usam somente 07–19. (`app/core/tests/test_painel_op_turno_aberto_ancora.py:3-6,33-42`; `app/core/tests/test_e4_7iii_painel_e_presenca.py:120-131,163-172`.)

2. **P1/P2 — N+1 novo por linha encerrada, com recarga duplicada.** Cada linha `encerrado` chama `dias_encerrados` sem células/turnos alimentados. Esse carregador consulta células, vínculos e turnos; se verdadeiro, o painel chama D4, que consulta a célula, recarrega turnos e chama novamente `dias_encerrados`. O custo cresce com o número de linhas encerradas, inclusive antes do filtro de status. Quantidade e latência reais: [nao verificado]. (`app/core/services/painel_op.py:193-199`; `app/ponto/turnos.py:2129-2133`; `app/ponto/status_dia.py:171-176`.)

   Já havia N+1 de presença: `turno_aberto_de` dentro de laço, embora exista `turnos_abertos_de` com carregamento plural. (`app/core/services/painel_op.py:85-86`; `app/ponto/turnos.py:2266-2274`.)

3. **P2 — previsão/cobertura continuam escolhendo vínculo por `ativa=True`, fora da autoridade registrada.** O painel monta um dict de vínculos ativos e usa diretamente esse vínculo em `_previsao_de_hoje`, `_previsto_agora` e no fim da janela. A autoridade cadastrada para vínculo do dia é `vinculo_do_dia`, que respeita geradora e vigência; para cobertura da data, `vinculo_cobre_a_data`. (`app/core/services/painel_op.py:97-102,132-147,152,176-178,218`; `app/core/juizes.py:1255-1256`; `app/escala/alimentacao.py:173-184,237-254`.)

   Contraexemplo: hoje 17/09 09h; ativo com EC 12x36 07–19, `ativa=True`, `data_inicio=18/09`, âncora17; sem célula nem batidas. Painel produz `previsao='previsto'` e `previsto_agora=True`; calendário guarda a vigência, obtém `eh_trabalho=None` e cai em `sem_dados`. Inferência estática: a aritmética da EC não testa `data_inicio`. (`app/core/services/painel_op.py:27-35,132-147`; `app/escala/models.py:1260-1263`; `app/colaboradores/services/calendario.py:766-778,118-143`.)

4. **Discrepância legada concreta: turno expirado sem S.** Ativo, 07–19, única E17/09 07h, agora17/09 23h01. O juiz de presença expira a aberta após fim+4h; painel passa a `encerrado`, embora não haja S. O calendário ainda usa `any(t.aberto)` e pinta `aberto`. O novo D4 pode aparecer nessa linha após o encerramento pelo relógio. Inferência estática; ocorrência real [nao verificado]. (`app/ponto/turnos.py:1987-2011,2097-2105`; `app/core/services/painel_op.py:181-196`; `app/colaboradores/services/calendario.py:144-145,811`.)

5. **Derivação paralela de previsão existe como tradução duplicada, não como nova aritmética de escala.** `_previsao_de_hoje` e `minha_escala` repetem o mesmo ternário True/False/None → previsto/folga/escala_suspeita; ambas delegam trabalho à EC. A duplicação é da tradução e da escolha/alimentação do vínculo. (`app/core/services/painel_op.py:27-35`; `app/escala/views.py:25,40,54`; `app/escala/models.py:1048-1073`.)

6. **Relógio do painel:** a nova cadeia usa um `agora` capturado no construtor e passado ao juiz de presença, ao encerramento e ao D4; não encontrei nova leitura independente nesses caminhos efetivamente chamados. `minha_escala` continua usando `datetime.date.today()`, diferente da data local derivada do instante no painel; discrepância depende do fuso do processo [nao verificado]. (`app/core/services/painel_op.py:53-54,86,195-196`; `app/ponto/status_dia.py:169-170`; `app/ponto/turnos.py:2124-2125,2261-2263`; `app/escala/views.py:26`.)

7. **Segundo escritor:** o construtor publicado não grava D4, célula ou batida; o novo caminho chama leitores. Os testes plantam `status_dia` diretamente por `.update()`, mas isso está no código de teste. Não atribuir ao serviço um segundo escritor por causa da fixture. (`app/core/services/painel_op.py:190-196`; `app/ponto/status_dia.py:171-185`; `app/core/tests/test_e4_7iii_painel_e_presenca.py:75-82`.)

8. **Selos que deixam passar a âncora errada:** o contrato de batida por data permite todo o arquivo `painel_op.py`, com comentário sobre transferência do legado; a nova derivação `_d_enc` fica dentro dessa permissão por arquivo. Os novos testes não incluem turno fechado com retorno de intervalo após meia-noite nem teto de consultas. (`app/ponto/tests/test_contract_no_batida_date.py:32-43`; `app/core/services/painel_op.py:194`; `app/core/tests/test_e4_7iii_painel_e_presenca.py:85-172`.)

## 31/10 17:06 · sessao c60283801c54 · - · - · origem=Ronald (tmux)

Mapa do medidor e dos selos:

- `PALAVRAS_DA_PRESENCA` é definido no módulo e lido por `PALAVRAS_DA_CATEGORIA`, pelo predicado de `censo` e pelo teste novo; contém normal/geofence/turno_aberto/encerrado. O detector, porém, só entrega palavras existentes no vocabulário geral; normal/geofence ficam fora dele por desenho. (`app/core/medida_leitor_do_dia.py:169`, `:194`, `:270-274`, `:367-368`; `app/core/tests/test_e4_7iii_painel_e_presenca.py:207`)
- `NAO_E_LEITOR` é definido em `app/core/medida_leitor_do_dia.py:275`; as duas novas entradas estão em `:302-307`. `censo` consulta categoria/motivo e calcula exclusões mortas (`:366-370`, `:403`); o selo valida categoria/motivo/tamanho e ausência de sobreposição com DECLARADOS (`app/core/tests/test_medida_leitor_do_dia.py:73-83`).
- `TETO_NAO_E_LEITOR=10` é definido em `app/core/medida_leitor_do_dia.py:309` e comparado ao tamanho exato da lista em `app/core/tests/test_medida_leitor_do_dia.py:75`.
- `DECLARADOS` mantém cinco funções: duas do calendário, API do espelho, espelho admin e relatório de furos. É definido em `app/core/medida_leitor_do_dia.py:316-332`; `censo` lê motivos, conta declarados e calcula novos/sumidos/sítios alterados (`:377`, `:395-401`). O selo cobra esses resultados (`app/core/tests/test_medida_leitor_do_dia.py:39-55`, `:69-71`).
- `bin/leitor_dia_estado.py` carrega o mesmo módulo e chama `censo`, sem detector paralelo (`:25`, `:32-35`). Publica contagem/lista/alarmes/pontos cegos (`:46-69`); `bin/gerar_estado.py` importa `ler`/`linha` (`:290-291`). A S3 importa o detector de dinheiro do mesmo módulo (`app/ponto/tests/test_s3_placar_exercicio.py:38`).
- `core/juizes.py` mantém painel, badges e filtro como pendências: contadores em `:715-716`, ausentes_hoje em `:717-718`, badge aberto e atributo de filtro em `:727-730`, JS em `:731-732`. A obra atualiza os motivos, não declara esses sítios curados. O juiz de turno é registrado em `:180` e `:683`; status do dia em `:173`; também constam de `app/docs/JUIZES.md:20` e `:74-75`.

Validação executada somente em memória, com `python3 -B`: o censo desta cópia retornou 5 = status 5 + valor 0, sem novos, sumidos, sítios alterados, exclusões mortas ou ilegíveis. Os campos provêm de `app/core/medida_leitor_do_dia.py:389-408`; o resultado de execução não foi gravado em arquivo [nao verificado].

Mordida real e limite:

- Os testes novos realmente plantam `encerrado → ausente`, `encerrado → folga` e `previsto → ok`, exigindo aparecimento em novos e incremento de contagem. (`app/core/tests/test_e4_7iii_painel_e_presenca.py:211-227`)
- O predicado **não verifica todos os valores de status nem a origem do dia**: considera apenas literais previamente reconhecidos pelo detector; valores dentro de dict não entram, assim como palavras fora do vocabulário. (`app/core/medida_leitor_do_dia.py:194`, `:200-212`, `:368`)
- Contra-mutações executadas somente em memória mantiveram 5 e todos os alarmes vazios:
  1. `painel_op.py:174`: `status = 'normal'` substituído por `status = {'resultado': 'falta'}['resultado']`.
  2. `painel_op.py:196`: chamada ao D4 substituída por `dia = {'status': 'falta', 'fonte': 'paralelo'}`.
  3. Mesmo sítio substituído por `dia = 'normal' if colab.id else 'geofence'`.
  
  Isso prova limite do medidor, não aprovação pela suíte completa; os resultados em memória não possuem arquivo de registro [nao verificado]. A causa está em `app/core/medida_leitor_do_dia.py:194-212`, e a limitação de indireção/templates/vocabulário é publicada em `:334-343`.

Quebra concreta fora do diff:

- O selo do host constrói cópia mínima com leitor, medidor e calendário, mas não copia `app/ponto/status_dia.py`. (`bin/tests/test_leitor_dia_estado.sh:46-50`)
- O medidor exige esse arquivo para obter o vocabulário antes do censo. (`app/core/medida_leitor_do_dia.py:124`, `:162-163`, `:352`)
- Portanto a cópia mínima retorna `medido=False`, e `campo` produz `NAO|...`; depois o teste avalia `$((NB + 1))` com NB=NAO sob `set -u`, falhando antes de demonstrar a planta +1. (`bin/leitor_dia_estado.py:35-37`; `bin/tests/test_leitor_dia_estado.sh:13`, `:33-36`, `:50`, `:59`)
- Essa falha é dedução estática; o script não foi executado porque cria e apaga arquivos temporários. (`bin/tests/test_leitor_dia_estado.sh:46-56`)

## 31/10 17:06 · sessao 41d28737325d · - · - · origem=Ronald (tmux)

Achados prioritários:

1. **Regressão no “Designar” em cobertura parcial.** Com dois colaboradores previstos 07–19, às 09h um tem E07 e o outro não bateu: o ausente recebe `status=None`, `previsto_agora=True`; cobertura retorna `parcial`, mas painel converte para `coberto`, e o botão novo exige `descoberto`. Portanto desaparece o caminho de designação para o segundo titular. Fontes: `app/core/services/painel_op.py:184`, `:217`, `:261`, `:275`; `app/ponto/selecao_periodo.py:244`; `app/templates/core/_painel_card.html:69`. A condição anterior, `status == 'ausente'`, está no diff fornecido [nao verificado quanto a execução].

2. **Encerramento antecipado também impede designação.** Titular 07–19 com E07/S08, às09: recebe `encerrado`; continua previsto, posto pode ficar descoberto, mas `not c.status` é falso. Fontes: `app/core/services/painel_op.py:181`, `:193`, `:217`; `app/templates/core/_painel_card.html:69`.

3. **URLs antigas continuam aceitas e agora esvaziam o painel.** Ambas as views passam `?status=ausente`/`folga` sem validação ao serviço; igualdade com o status novo não encontra ninguém, depois os postos vazios são omitidos. Fontes: `app/core/views.py:607`, `:626`, `:632`; `app/core/services/painel_op.py:198`, `:269`. Existência de favoritos/integração usando essas URLs: [nao verificado].

4. **Selo de N+1 não exercita o ramo novo.** A fixture de 45 colaboradores e os 100 acrescentados possuem vínculos, sem batidas; a comparação de contagem nunca testa colaboradores `encerrado`, cujo ramo chama os dois serviços individualmente. Fontes: `app/core/tests/test_selo_lotes_painel.py:41`, `:95`, `:111`; `app/core/services/painel_op.py:193`.

Mapa dos leitores:

| Campo | Escritor | Leitores encontrados |
|---|---|---|
| `status` | Serviço escolhe as quatro palavras ou `None`: `app/core/services/painel_op.py:168`, `:181`, `:188`, `:211` | Filtro/contadores/geofence/ordenação no próprio serviço: `:198`, `:224`, `:273`, `:281`; avatar, badge, aberto e botão: `app/templates/core/_painel_card.html:6`, `:36`, `:54`, `:66`, `:69` |
| `previsao` | `_previsao_de_hoje`, chamada ao criar linha: `app/core/services/painel_op.py:27`, `:218` | Avatar e fallback textual do badge: `app/templates/core/_painel_card.html:54`, `:66`, `:67` |
| `dia` | Condição encerrado + juiz + D4: `app/core/services/painel_op.py:192` | Título `rotulo` e texto `status`: `app/templates/core/_painel_card.html:66`, `:67` |
| `contadores` | Quatro somas: `app/core/services/painel_op.py:223` | View repassa: `app/core/views.py:637`, `:670`; template lê quatro: `app/templates/core/_painel_stats.html:32` |
| `statuses` | Lista de quatro: `app/core/services/painel_op.py:230` | View lê/repassa: `app/core/views.py:639`, `:672`. Leitor final atual não encontrado [nao verificado] |
| `ausentes` do posto | Removido no diff; dicionário atual não possui: `app/core/services/painel_op.py:279` | Card atual não pede a chave; cabeçalho usa em_turno/geofence: `app/templates/core/_painel_card.html:12`, `:34`, `:35` |
| `bateu_hoje_ids` | Apuráveis por data: `app/core/services/painel_op.py:258` | `_teve_ativ`, depois `classificar_cobertura`: `:276`, `:277` |

As duas entradas de produção encontradas são página/fragmento e lote; ambas usam o mesmo serviço. Página repassa linhas, postos, contadores e statuses; lote utiliza postos. Fontes: `app/core/views.py:607`, `:610`, `:632`, `:635`, `:637`, `:639`, `:679`. Os dois templates de grade/lote incluem o mesmo card: `app/templates/core/_painel_grid.html:2`, `app/templates/core/_painel_lote.html:7`. Rotas HTML: `app/core/urls.py:29`, `:30`; consumidor API do serviço não demonstrado [nao verificado].

**Não encontrei leitor atual do painel ainda esperando `ausente`/`folga` em `status` ou `ausentes`** nos consumidores acima: badge passa a consultar `previsao`; filtros JS só conhecem descoberto/geofence/turno_aberto; stats leem as quatro chaves. Fontes: `app/templates/core/_painel_card.html:67`; `app/templates/core/painel_operacional.html:173`; `app/templates/core/_painel_stats.html:32`.

Testes externos ao diff consomem `status=normal`, `encerrado`, `turno_aberto`, `entrada`, `data_turno`, `tempo_turno`, cobertura e chamados; não esperam explicitamente as palavras removidas. Fontes: `app/core/tests/test_painel_op_turno_pelo_juiz.py:75`, `:78`, `:84`; `app/core/tests/test_painel_op_turno_aberto_ancora.py:49`; `app/core/tests/test_e4_7_painel_entrada_retratada.py:80`, `:82`; `app/core/tests/test_painel_previsto_celula.py:55`; `app/core/tests/test_registrado_nao_e_pendencia.py:67`.

Fora do diff, permanecem:

- **Filtro “Aberto” perde posto com dois turnos abertos:** card concatena `11`, JS aceita apenas string `'1'`. Fontes: `app/templates/core/_painel_card.html:6`; `app/templates/core/painel_operacional.html:178`. Registro anterior do mesmo bug: `app/docs/CENSO_NARNIA.md:225`.
- **Filtros/busca não são reaplicados aos lotes:** handler `afterSwap` só trata alvo `pop-body`; sentinelas substituem `this` por novos cards. Fontes: `app/templates/core/painel_operacional.html:161`; `app/templates/core/_painel_lote.html:8`.
- **KPI “sem escala” inclui encerrados e ignora empresa no denominador:** total conta todos os postos ativos, enquanto numeradores vêm do serviço filtrado; residual é rotulado sem escala. Fontes: `app/core/views.py:648`, `:651`; `app/templates/core/_painel_stats.html:24`.
- **TV continua outra derivação:** usa batidas cruas para atividade e previsão por hora do template, sem fase/DNA; não consome o serviço operacional. Fontes: `app/core/views.py:792`, `:848`. Os status TV são `fora`, `normal`, `geofence`, `aberto`, consumidos por JS próprio. Fontes: `app/core/views.py:807`, `:816`, `:828`; `app/templates/core/painel_tv.html:161`, `:177`.
- **Relógio do cliente permanece em duas funções:** exibição e período default de designação. Fontes: `app/templates/core/painel_operacional.html:155`, `:294`.
- **Registro vivo ainda declara pendências de derivação no serviço/card/JS:** contadores, KPIs, badges e filtros continuam em `PENDENTES['tela']`. Fontes: `app/core/juizes.py:715`, `:719`, `:727`, `:729`, `:731`.

Não executei testes: os testes relevantes criam registros e os testes do medidor também criam arquivos temporários, incompatíveis com somente leitura. Fontes: `app/core/tests/test_selo_lotes_painel.py:32`, `app/core/tests/test_medida_leitor_do_dia.py:98`. Resultado de execução: [nao verificado].

## 31/10 16:54 · sessao bab72768ff86 · DISJUNTOR-B1-ESPERA-OCIOSA · leitura · origem=Code

**O v1 tem dois problemas centrais: E2 continuaria sem detectar um Bash único já esperando, e seu critério confundiria trabalho legítimo sem escrita com ócio.** A medição só ocorre no PreToolUse do próprio agente; o Stop apenas consulta disparos gravados. O medidor não consulta o processo em execução (`bin/disjuntor.py:413`, `bin/disjuntor.py:478`, `bin/disjuntor.py:1435`, `bin/hook_stop_fila1.py:1053`).

Os contraexemplos abaixo são cenários construídos para avaliar o contrato; sua ocorrência em produção é **[nao verificado]**. Resultado de execução dos testes: **[nao verificado]**; o script cria, altera e apaga arquivos, contrariando a restrição desta análise (`bin/tests/test_disjuntor.sh:39`, `bin/tests/test_disjuntor.sh:50`).

**Respostas diretas**

- **O payload real de PreToolUse de subagente traz `agent_id`? [nao verificado].** O código o lê em `bin/disjuntor.py:1429` e também o usa na identificação da negativa em `bin/disjuntor.py:216`. O teste fabrica esse campo; isso comprova a expectativa do código, não o envio pelo harness (`bin/tests/test_disjuntor.sh:242`, `bin/tests/test_disjuntor.sh:85`).
- **Uma suite em primeiro plano, sem `sleep`, seria considerada ociosa pelo E2? Sim, quando medida**, se o agente não for isento, tiver idade suficiente, cópia citada e nenhum arquivo recente. O medidor não distingue suite, RED, ruff, git ou espera (`bin/disjuntor.py:418`, `bin/disjuntor.py:425`, `bin/disjuntor.py:450`, `bin/disjuntor.py:454`).
- **Isso não significa disparar durante o Bash no minuto 10.** Sem outra chamada do agente, não há nova medição; os 60 segundos são intervalo mínimo entre medições solicitadas, não um agendamento (`bin/disjuntor.py:478`, `bin/disjuntor.py:480`, `bin/disjuntor.py:1436`). O Stop não supre essa ausência (`bin/hook_stop_fila1.py:1053`).

**Confronto por cláusula**

| Cláusula do contrato | Suporte atual e o que falta | Contraexemplo ou consequência concreta |
|---|---|---|
| Cabeçalho: aval, instrumento e pouso posterior | Aval específico e ordem efetiva de pouso: **[nao verificado]**. O código continua declarando B1 com 15 minutos e consumo de tokens (`bin/disjuntor.py:7`, `bin/disjuntor.py:68`, `bin/disjuntor.py:452`). | O cabeçalho não estabelece uma decisão classificatória; a divergência verificável está nos critérios seguintes (`bin/disjuntor.py:450`). |
| **E1 — sleep, laços, follow e timeout** | Não há ramo E1 no PreToolUse atual. Bash comum de agente passa apenas pela medição genérica B1, sem classificação dessas esperas (`bin/disjuntor.py:1426`, `bin/disjuntor.py:1435`, `bin/disjuntor.py:1440`). Falta reconhecimento, negativa específica, instrução comando+log e emissão `B1-ESPERA`. | **FN atual:** agente jovem executando `until grep -q rc= x.log; do sleep 60; done` passa pelo B1 por idade insuficiente (`bin/disjuntor.py:450`). **FP do texto proposto:** `while false; do sleep 60; done` contém laço com sleep, mas executa zero iterações; a precisão da futura regex é **[nao verificado]**. |
| **E1 — leitura de log “EM CURSO”** | O código atual não examina caminho, conteúdo `^rc=` ou mtime do log para decidir sobre Bash (`bin/disjuntor.py:1430`, `bin/disjuntor.py:1435`). | **FP do contrato:** `tail -n 40 /tmp/wt-A/red.out` logo após RED concluído, sem `rc=` e com mtime recente. O runner RED grava saída, mas não acrescenta esse marcador (`bin/molde_autocura/rodar.sh:18`, `bin/molde_autocura/rodar.sh:24`). A própria suite também termina diretamente no processo de teste (`bin/suite.sh:96`). |
| **E1 — só subagente; principal fora** | Há leitura de `agent_id`; Bash comum sem ele retorna antes de I/O. A negativa e a exceção para Handback já têm molde B1 (`bin/disjuntor.py:1429`, `bin/disjuntor.py:1431`, `bin/disjuntor.py:1435`, `bin/disjuntor.py:1395`). | **FN condicionado:** se o harness omitir `agent_id`, `sleep 600` de subagente será tratado como Bash sem agente. O envio real desse campo é **[nao verificado]** (`bin/disjuntor.py:1431`). |
| **E2 — 15→10 minutos; retirar tokens** | As duas mudanças faltam: constante ainda 15 e `tokens <= 0` ainda impede disparo (`bin/disjuntor.py:68`, `bin/disjuntor.py:452`). | **FP do contrato:** agente `general-purpose`, vivo há 12 minutos, cópia `/tmp/wt-A` citada, arquivos antigos, pede `git -C /tmp/wt-A diff` ou inicia a suite. E2 nega pelo estado, sem avaliar o trabalho solicitado; o molde atual aplica a negativa a qualquer ferramenta exceto Handback (`bin/disjuntor.py:1435`). |
| **E2 — “vivo” e “arquivo novo”** | “Vivo” deriva do menor timestamp legível do transcript; “novo” significa arquivo regular com mtime recente, inclusive arquivo antigo modificado. Não mede criação nem execução produtiva (`bin/disjuntor.py:405`, `bin/disjuntor.py:441`, `bin/disjuntor.py:450`). | **FN:** agente esperando, mas um arquivo da cópia recebe atualização periódica: a medição encontra mtime recente e libera. Não exige que a atualização venha do agente ou represente avanço (`bin/disjuntor.py:405`, `bin/disjuntor.py:458`). |
| **E2 — PreToolUse nega; Stop pede TaskStop** | Existe, mas Stop só considera disparos já gravados e IDs reconhecidos como agentes async sem entrega; não mede novamente nem encerra o agente (`bin/disjuntor.py:489`, `bin/disjuntor.py:376`, `bin/hook_stop_fila1.py:1053`). O recado ocorre no caminho final do Stop, depois de possíveis saídas antecipadas (`bin/hook_stop_fila1.py:1207`, `bin/hook_stop_fila1.py:1297`, `bin/hook_stop_fila1.py:1300`). | **FN:** agente entra em Bash antes do limiar e permanece esperando sem nova ferramenta; não nasce disparo para o Stop consultar (`bin/disjuntor.py:1435`, `bin/hook_stop_fila1.py:1054`). |
| **QUEM CHAMA — mesmos hooks** | A configuração aponta para os dois comandos descritos (`.claude/settings.json:9`, `.claude/settings.json:68`). Porém são caminhos absolutos em `/home/ronald/saas-hasner`, externos à cópia auditada; identidade com os arquivos realmente executados é **[nao verificado]**. | Manter os mesmos chamadores, sem acrescentar medição ao Stop, preserva o FN do Bash único (`bin/hook_stop_fila1.py:1054`). |
| **QUEM ESPERA — síncrono, memória 1/min** | Decisão e cache existem; não há medição periódica independente. Há também espera limitada por contenção da trava do estado (`bin/disjuntor.py:251`, `bin/disjuntor.py:256`, `bin/disjuntor.py:478`). | **FN concreto sem sleep aparente:** `bash bin/suite.sh --only "ponto core"` pode ficar em `flock` por até 3600 segundos. A espera está dentro do script chamado (`bin/suite.sh:96`, `bin/trava_teste.sh:22`, `bin/trava_teste.sh:36`). |
| **FALHA — ilegível não dispara; painel `nao_sei`** | Meta/transcript/cópia ilegíveis podem produzir motivo “nao sei” sem disparo. Não existe contador B1 `nao_sei` no estado nem publicação desse motivo no painel (`bin/disjuntor.py:417`, `bin/disjuntor.py:424`, `bin/disjuntor.py:457`, `bin/disjuntor.py:481`, `bin/disjuntor.py:1615`). Parser específico E1 ainda falta (`bin/disjuntor.py:1426`). | **FN assumido pela política:** espera com meta ilegível passa (`bin/disjuntor.py:417`). **Ressalva:** disparo já gravado retorna antes de reler meta/transcript/cópia; portanto “ilegível = não dispara” não libera um B1 anteriormente disparado (`bin/disjuntor.py:476`). |
| **LIMITE — script próprio invisível ao E1; E2 pega em 10 minutos** | A segunda promessa não decorre dos chamadores atuais: B1 só mede no PreToolUse do agente (`bin/disjuntor.py:1436`). | **FN:** `bash x.sh`, cujo conteúdo seja `sleep 3600`, iniciado antes do limiar e sem chamada posterior. Mesmo retirar tokens não cria a medição ausente (`bin/disjuntor.py:478`, `bin/hook_stop_fila1.py:1054`). |
| **DIFF/apply suspende E2; E1 não** | PreToolUse suspende B1 quando `sombra.lock` está tomado ou ilegível; mera existência do arquivo não basta (`bin/disjuntor.py:307`, `bin/disjuntor.py:318`, `bin/disjuntor.py:1435`). Falta E1 independente dessa guarda. O Stop não verifica DIFF antes do recado B1 (`bin/hook_stop_fila1.py:1053`). | **FP/interrupção indevida possível:** B1 dispara; depois começa DIFF com lock tomado; Stop ainda pede TaskStop, pois consulta o disparo persistido sem essa guarda (`bin/disjuntor.py:476`, `bin/hook_stop_fila1.py:1054`). |
| **MEDIDA — disparos, negações, `nao_sei`, enumeração** | Leitor já agrega disparos pelo identificador, mas enumeração não inclui `B1-ESPERA`; negações são agregadas, sem separação por regra; falta `nao_sei` B1. Janela é móvel de 24 horas, não dia civil (`bin/disjuntor.py:1548`, `bin/disjuntor.py:1570`, `bin/disjuntor.py:1579`, `bin/disjuntor.py:1679`). | Se surgisse uma linha `DISJUNTOR B1-ESPERA ...`, o leitor a contaria, mas a enumeração atual não a exibiria. O painel também não conta uma espera que nunca foi medida (`bin/disjuntor.py:1579`, `bin/disjuntor.py:1679`, `bin/disjuntor.py:1436`). |
| **QUEM DESLIGA — chave global do Ronald** | A chave é respeitada por PreToolUse e Stop; a proteção da própria chave ocorre antes do desligamento (`bin/disjuntor.py:144`, `bin/disjuntor.py:1486`, `bin/hook_stop_fila1.py:1039`). | **FP existente sobre git legítimo:** `git log --all --oneline --grep=DISJUNTOR_DESLIGADO` é negado por simplesmente mencionar a chave, inclusive com os disjuntores desligados (`bin/disjuntor.py:160`, `bin/disjuntor.py:1486`). |

**Os quatro trabalhos legítimos pedidos**

Considere agente `general-purpose` com cópia citada e sem mtime recente. Esses comandos não precisam produzir arquivo novo na cópia; E2 não contém um discriminador que os separe de espera (`bin/disjuntor.py:418`, `bin/disjuntor.py:454`, `bin/disjuntor.py:1435`).

| Trabalho | Comando/forma concreta e evidência |
|---|---|
| Suite `--only` em primeiro plano | `bash bin/suite.sh --dir /tmp/wt-A --only "ponto core" --espera 1`. Opções suportadas; execução por `exec` e Docker sem `-d`, com bytecode desativado (`bin/suite.sh:46`, `bin/suite.sh:96`, `bin/suite.sh:99`). |
| Docker do RED | A chamada `teste $T/orig/app test_juliani_esacred $T/red.out $IDS` executa Docker em primeiro plano e grava log fora de `orig/app` (`bin/molde_autocura/rodar.sh:20`, `bin/molde_autocura/rodar.sh:24`). |
| Ruff | `docker run --rm -v /tmp/wt-A/app:/app:ro -w /app saas-hasner-core:latest ruff check . --no-cache`, seguindo a forma existente com montagem somente leitura e cache desativado (`bin/molde_autocura/rodar.sh:13`). |
| Git | `git -C /tmp/wt-A diff`. B1 não examina o comando antes de negar. O exemplo usa `general-purpose`: `verificador` e os demais tipos de `SO_LEITURA` são isentos no medidor atual (`bin/disjuntor.py:91`, `bin/disjuntor.py:418`, `bin/disjuntor.py:1435`). |

**Os cinco MORDE**

| Item | Situação atual e lacuna |
|---|---|
| **1 — until/sleep negado; mutante sem E1 vermelho** | Falta ramo e caso E1. Os casos B1 atuais são `b1_nega`, `b1_nao_nega`, `b1_recado`; mutantes diretos cobram mtime e Handback (`bin/tests/test_disjuntor.sh:1468`, `bin/tests/test_disjuntor.sh:1577`). Um agente jovem com o laço passa hoje (`bin/disjuntor.py:450`). |
| **2 — sleep 5 e principal sleep 600 passam** | Principal sem `agent_id` já passa. Entretanto agente com B1 disparado tem até `sleep 5`, ruff ou git negados. O contrato precisa delimitar que o controle positivo de E1 usa E2 não disparado (`bin/disjuntor.py:1431`, `bin/disjuntor.py:1435`). |
| **3 — sem tokens por 10 minutos dispara; mutante vermelho** | Hoje ausência de tokens impede disparo. A fixture injeta 510 tokens no último turno; falta caso específico e mutante da condição (`bin/disjuntor.py:452`, `bin/tests/test_disjuntor.sh:237`, `bin/tests/test_disjuntor.sh:1577`). O teste ainda exige que agente com 10 minutos passe (`bin/tests/test_disjuntor.sh:258`). |
| **4 — arquivo recente não dispara** | Já existe controle com arquivo de três minutos (`bin/tests/test_disjuntor.sh:254`). Mas, após `disparado=true`, arquivo novo não rearma: o retorno antecede a medição. É contraexemplo ao item se escrito sem restringir o estado inicial (`bin/disjuntor.py:476`). |
| **5 — log com rc final não negado** | Falta caso específico E1 (`bin/tests/test_disjuntor.sh:1468`). O texto precisa resolver precedência com `tail -f` e distinguir rc da execução atual: log ativo contendo `rc=0` de execução anterior escaparia da heurística proposta; ocorrência em produção **[nao verificado]**. |

**Quem escreve e quem lê o estado**

Os caminhos são `logs/sessao/disjuntor_estado.json` e `logs/sessao/disjuntor.log` (`bin/disjuntor.py:61`). No JSON, a gravação física passa por `_com_estado`, com flock, temporário e substituição; `ler_estado` faz a leitura direta (`bin/disjuntor.py:237`, `bin/disjuntor.py:260`, `bin/disjuntor.py:271`, `bin/disjuntor.py:291`).

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `b1[agent_id]` | `b1_do_agente` cria/atualiza a entrada, via `_com_estado` (`bin/disjuntor.py:474`, `bin/disjuntor.py:481`, `bin/disjuntor.py:486`). | O próprio `b1_do_agente`; `b1_disparados` percorre as entradas (`bin/disjuntor.py:475`, `bin/disjuntor.py:490`). |
| `.checado` | `b1_do_agente`, com `agora` após medir (`bin/disjuntor.py:481`). | Mesmo método, para intervalo de 60 segundos (`bin/disjuntor.py:478`). |
| `.disparado` | `b1_do_agente`, com `bool(med['disparo'])` (`bin/disjuntor.py:481`). | Mesmo método e `b1_disparados`; este último alimenta o Stop (`bin/disjuntor.py:476`, `bin/disjuntor.py:490`, `bin/hook_stop_fila1.py:1054`). |
| `.motivo` | `b1_do_agente`, a partir do medidor (`bin/disjuntor.py:481`). | Retorno do disparo/cache e composição da negativa PreToolUse (`bin/disjuntor.py:477`, `bin/disjuntor.py:479`, `bin/disjuntor.py:1438`). |
| `tokens`, `copias`, contador `nao_sei` | Tokens/cópias são resultados transitórios do medidor; não são campos persistidos em B1. “Nao sei” pode ficar apenas no texto de `.motivo`; não há contador correspondente nessa gravação (`bin/disjuntor.py:417`, `bin/disjuntor.py:460`, `bin/disjuntor.py:481`). | O painel não lê esses motivos B1 para contá-los (`bin/disjuntor.py:1615`). |

No log, **o escritor físico é `trilha`**, que acrescenta carimbo, evento e detalhe achatado/truncado em 300 caracteres (`bin/disjuntor.py:128`).

| Registro/campo do log | Quem produz | Quem lê |
|---|---|---|
| Carimbo e evento | `trilha` (`bin/disjuntor.py:133`). | `ler`, que separa tokens e interpreta o instante (`bin/disjuntor.py:1559`, `bin/disjuntor.py:1563`). |
| `DISJUNTOR B1 <id>: medida \| ação` | `b1_do_agente` → `dispara` → `trilha` (`bin/disjuntor.py:483`, `bin/disjuntor.py:139`). | `ler` conta pelo quarto token, `B1`; não usa o ID para a contagem (`bin/disjuntor.py:1579`). |
| `DISJUNTOR B1-ESPERA ...` | Falta produtor no ramo atual de PreToolUse (`bin/disjuntor.py:1426`). | O leitor genérico comporta o identificador; falta inclusão na enumeração de `linha` (`bin/disjuntor.py:1579`, `bin/disjuntor.py:1679`). |
| `NEGOU` e motivo | `main_pretool`, antes da resposta deny (`bin/disjuntor.py:1489`). | `ler` incrementa total agregado de negativas (`bin/disjuntor.py:1570`). |
| `FALHA` | Falhas de persistência, PreToolUse e registro no RELATO; Stop também chama `DJ.trilha` (`bin/disjuntor.py:281`, `bin/disjuntor.py:232`, `bin/disjuntor.py:1506`, `bin/hook_stop_fila1.py:1061`). | `ler` conta especificamente `relato_negacao` como negativa sem RELATO; isso não constitui contador de medições B1 desconhecidas (`bin/disjuntor.py:1572`). |
| `ALVO`, hash e comando | `relato_negacao` (`bin/disjuntor.py:211`). | Não integra os eventos lidos pelo painel (`bin/disjuntor.py:1554`). |

O publicador do ESTADO chama `disjuntor.ler/linha`; **o Stop decide seu recado B1 pelo JSON, não pelo log** (`bin/gerar_estado.py:280`, `bin/hook_stop_fila1.py:1054`).

## 31/10 16:52 · sessao 6de02a40a9c1 · - · - · origem=Ronald (tmux)

- **`agent_id` real no payload de subagente: [nao verificado].** O código espera esse campo em `pretool`, lendo `payload.get('agent_id')`; sem ele, um Bash comum sai antes de qualquer I/O (`bin/disjuntor.py:1429–1432`). A evidência disponível é sintética: `_pl` fabrica JSON com `agent_id` (`bin/tests/test_disjuntor.sh:242`), e `_pre` entrega o JSON ao hook (`bin/tests/test_disjuntor.sh:85`). Não há prova local de uma captura real do harness [nao verificado]. O campo também identifica o agente na trilha do RELATO (`bin/disjuntor.py:216`).

- **E2 como escrito confundiria suite produtiva com ociosidade quando medido.** `medir_b1` conhece tipo do agente, texto do transcript, timestamps, tokens e mtime de arquivos; não examina comando em execução nem distingue trabalho CPU/DB de espera (`bin/disjuntor.py:413–461`). O tipo `general-purpose` não é isento; isentos são `verificador`, `Explore`, `Plan`, `claude-code-guide`, `statusline-setup` (`bin/disjuntor.py:90–91`, `bin/disjuntor.py:418–419`). Remover o requisito de tokens e usar janela de 10 min classificaria como disparada uma suite em primeiro plano de agente com cópia citada e sem escrita nessa cópia em 10 min; a duração real de qualquer suite do dado é [nao verificado].

- **Mas não dispararia automaticamente no minuto 10 de UM Bash já em execução.** `b1_do_agente` só é chamado pelo PreToolUse daquele agente (`bin/disjuntor.py:1435–1436`). Os 60 s são cache de medição sob demanda, sem agendador (`bin/disjuntor.py:478–481`). O Stop não mede: lê `b1_disparados` e cruza com agentes de fundo ainda sem entrega (`bin/hook_stop_fila1.py:1053–1054`; `bin/disjuntor.py:489–490`). Portanto, Bash único iniciado antes do limiar, sem chamadas posteriores, pode continuar além de 10 min sem novo disparo; remover tokens não fecha esse buraco (`bin/disjuntor.py:1435–1436`, `bin/hook_stop_fila1.py:1054`).

- **Contra-exemplo concreto, sem executar:** estado de transcript com `agentType=general-purpose`, primeiro timestamp há 20 min, cópia absoluta `/tmp/wt-a` citada, arquivos sem mtime recente, último turno assistente há 11 min, e ferramenta Bash `bash bin/suite.sh --dir /tmp/wt-a --only "ponto core"` ainda rodando. Hoje, em medição aos 20 min, falta token nos últimos 15 min pode livrar; com E2 e medição, a ausência de arquivos dispara, mesmo se a suite estiver calculando (`bin/disjuntor.py:425–453`, `bin/disjuntor.py:454–461`). A sintaxe `--dir`/`--only` é suportada (`bin/suite.sh:46–47`); a execução final é foreground `docker run ... python manage.py test` com `PYTHONDONTWRITEBYTECODE=1` (`bin/suite.sh:96–103`). Se nenhuma ferramenta nova for chamada, não há medição aos 20 min (`bin/disjuntor.py:1435–1436`).

- **Outro FN concreto de espera sem `sleep`:** a mesma chamada `bash bin/suite.sh --only "ponto core"` pode aguardar a trava. Suite sempre chama `trava_teste.sh` inclusive com `--only` (`bin/suite.sh:91–96`); a trava faz `flock -w "$ESPERA"` com default 3600 s (`bin/trava_teste.sh:22`, `bin/trava_teste.sh:35–38`). Isso escapa de regex que olha apenas o Bash externo e dos padrões E1 descritos; E2 não o captura no meio de um único Bash pelo motivo acima (`bin/disjuntor.py:1435–1436`, `bin/hook_stop_fila1.py:1054`).

- **“Vivo” hoje não consulta processo:** deriva da menor data de qualquer linha JSON com timestamp; JSON quebrado e timestamp inválido são pulados (`bin/disjuntor.py:430–451`). **“Arquivo novo” inclui arquivo antigo modificado:** basta qualquer arquivo regular com `mtime >= janela`, fora dos diretórios ignorados (`bin/disjuntor.py:402–406`). **Disparo é persistente:** `disparado=true` retorna sem reavaliar, mesmo que apareça arquivo posterior (`bin/disjuntor.py:476–477`).

## 31/10 16:52 · sessao 7ba21874f2bc · - · - · origem=Ronald (tmux)

Auditoria estática; não executei testes, pois o script cria árvore temporária, escreve fixtures e apaga arquivos no EXIT (`bin/tests/test_disjuntor.sh:39-55`). Resultado real da suíte: [nao verificado].

- Os testes atuais certificam **15 minutos e tokens positivos**: fixture sempre injeta 510 tokens (`bin/tests/test_disjuntor.sh:227-239`), a negativa exige texto “15 min” (`bin/tests/test_disjuntor.sh:245-250`), e agente com exatamente 10 minutos deve passar (`bin/tests/test_disjuntor.sh:258-259`). Reduzir para 10 minutos exige atualizar essa expectativa.
- Já existem controles para arquivo recente, verificador, DIFF, sessão principal e ilegibilidade (`bin/tests/test_disjuntor.sh:252-270`), além de pedido de TaskStop e retirada do pedido após killed (`bin/tests/test_disjuntor.sh:272-280`).
- Os cinco MORDE novos não estão implementados como casos E1/E2: os casos B1 continuam `b1_nega`, `b1_nao_nega`, `b1_recado` (`bin/tests/test_disjuntor.sh:1468`). Os mutantes diretos de B1 removem mtime ou exceção de Handback (`bin/tests/test_disjuntor.sh:1577-1578`), e os outros testam cópia/meta ilegíveis (`bin/tests/test_disjuntor.sh:1731-1735`).

Contraexemplos concretos, condicionados ao estado sintético E2: `general-purpose`, vivo há 12 minutos, `/tmp/wt-A` citada, nenhum arquivo recente. Ocorrência desse estado em produção: [nao verificado].

1. `bash bin/suite.sh --dir /tmp/wt-A --only "ponto core" --espera 1`: opções legítimas documentadas (`bin/suite.sh:24-27`), execução foreground por `exec`/docker sem `-d`, com `PYTHONDONTWRITEBYTECODE=1` (`bin/suite.sh:96-103`). E2 não distingue esse trabalho de espera, pois seu molde mede apenas idade/tokens/mtime (`bin/disjuntor.py:428-461`).
2. RED foreground: o molde executa docker contra a árvore anterior e grava `red.out` fora de `orig/app` (`bin/molde_autocura/rodar.sh:18-24`). É trabalho útil sem necessidade de escrever na árvore examinada.
3. Ruff: `docker run ... -v /tmp/wt-A/app:/app:ro ... sh -c "ruff check . --output-format concise --no-cache"` segue execução existente, read-only e sem cache (`bin/molde_autocura/rodar.sh:13`).
4. `git -C /tmp/wt-A diff`: a negativa de B1 não examina o comando, logo também alcança leitura legítima de git (`bin/disjuntor.py:1428-1439`). Para verificador há isenção explícita, portanto usar `general-purpose` neste contraexemplo (`bin/disjuntor.py:418-419`).

E1 também confunde “sem `^rc=`” com “em curso”: `tail -n 40 /tmp/wt-A/red.out`, logo após RED concluído, satisfaz a heurística proposta, embora o runner RED não grave `rc=` (`bin/molde_autocura/rodar.sh:18-24`). `suite.sh` termina diretamente no comando de teste, também sem acrescentar marcador (`bin/suite.sh:96-103`). O protocolo documental verifica veredito, `Ran` e rc **do processo**, sem impor `rc=` no arquivo (`CLAUDE.md:191-195`). Um log ainda ativo contendo `rc=0` de execução anterior constitui o falso negativo inverso; estado hipotético, [nao verificado].

O `agent_id` dos testes é **fabricado pelo teste**, não prova de payload real do harness: `_pl` imprime o JSON com esse campo (`bin/tests/test_disjuntor.sh:242`); o hook de teste apenas encaminha esse JSON ao stdin (`bin/tests/test_disjuntor.sh:85-86`).

## 31/10 16:52 · sessao 2b38ef829f7c · - · - · origem=Ronald (tmux)

Mapa de estado e integração, apenas por inspeção de código:

| Campo/arquivo | Quem escreve | Quem lê | Semântica e lacuna |
|---|---|---|---|
| `disjuntor_estado.json` | `_com_estado`: carrega JSON, executa transição, grava temporário e `os.replace`, sob `flock` (`bin/disjuntor.py:237`, `:247`, `:260`, `:268`, `:271`). | `_com_estado` e `ler_estado` (`bin/disjuntor.py:260`, `:291`). | JSON ilegível levanta; ausência vira `{}`; falha de gravação preserva a decisão já calculada e gera `FALHA` (`bin/disjuntor.py:262`, `:265`, `:281`, `:296`). |
| `b1` | `b1_do_agente`, via `est.setdefault('b1', {})` (`bin/disjuntor.py:473`). | Próprio `b1_do_agente` e `b1_disparados` (`bin/disjuntor.py:475`, `:489`). | Chaveado somente por `agent_id`, não por sessão (`bin/disjuntor.py:475`, `:481`). |
| `b1[agent_id].checado` | `b1_do_agente` grava `agora` ao medir (`bin/disjuntor.py:481`). | Mesmo método compara idade com `B1_REAVALIA_S=60` (`bin/disjuntor.py:69`, `:478`). | É intervalo mínimo entre chamadas; não é timer/autonomia de reavaliação (`bin/disjuntor.py:478`, `:1436`). |
| `b1[agent_id].disparado` | `bool(med['disparo'])` (`bin/disjuntor.py:481`). | `b1_do_agente` e `b1_disparados` (`bin/disjuntor.py:476`, `:490`). | Disparo fica latched: retorna antes de medir novamente; arquivo novo posterior não o desarma (`bin/disjuntor.py:476`, `:480`). |
| `b1[agent_id].motivo` | Copiado de `medir_b1` (`bin/disjuntor.py:481`). | Retorno de disparo/cache e mensagem de negativa (`bin/disjuntor.py:477`, `:479`, `:1438`). | `nao sei` existe como texto do motivo; não há contador B1 `nao_sei` no estado gravado (`bin/disjuntor.py:417`, `:424`, `:457`, `:481`). |
| `disjuntor.log`: carimbo, evento, detalhe | Escritor físico `trilha`: append com carimbo local, detalhe achatado e truncado em 300 caracteres (`bin/disjuntor.py:128`). | `ler` divide tokens, interpreta carimbo/evento (`bin/disjuntor.py:1554`, `:1559`, `:1563`). | Trilha ilegível torna disparos/negações/liberações `None`; arquivo ausente conta zero (`bin/disjuntor.py:1584`, `:1590`). |
| Evento `DISJUNTOR B1 <id>: medida \| ação` | `b1_do_agente` chama `dispara`, que chama `trilha` (`bin/disjuntor.py:483`, `:139`). | `ler` agrupa pelo quarto token, portanto por `B1`; `linha` renderiza enumeração fixa (`bin/disjuntor.py:1578`, `:1677`). | Sem ramo/escritor `B1-ESPERA`; enumeração atual não o inclui (`bin/disjuntor.py:483`, `:1679`). |
| Evento `NEGOU` | `main_pretool` antes de emitir resposta deny (`bin/disjuntor.py:1489`). | `ler` incrementa total agregado `negou` (`bin/disjuntor.py:1570`). | Não agrupa negações por regra/agente (`bin/disjuntor.py:1570`). |
| Eventos `FALHA`, `ALVO` | Falhas de estado/PreToolUse/RELATO e `relato_negacao`; Stop também chama `DJ.trilha('FALHA', ...)` (`bin/disjuntor.py:212`, `:232`, `:281`, `:1506`; `bin/hook_stop_fila1.py:1061`). | Painel lê `FALHA` para contar apenas `relato_negacao`; ignora `ALVO` (`bin/disjuntor.py:1554`, `:1572`). | Falha de medição B1 representada somente por motivo `nao sei` não vira essa contagem (`bin/disjuntor.py:417`, `:481`, `:1572`). |

`tokens` e `copias` são retornados por `medir_b1` quando dispara, mas não são persistidos como campos B1; persistem apenas `checado`, `disparado`, `motivo` (`bin/disjuntor.py:460`, `:481`).

Integração:

- `.claude/settings.json` liga `Stop` a `/home/ronald/saas-hasner/bin/hook_stop_fila1.py` e `PreToolUse` a `/home/ronald/saas-hasner/bin/disjuntor.py pretool`, com matcher vazio (`.claude/settings.json:3`, `:9`, `:62`, `:68`). Esses caminhos são absolutos e externos à cópia auditada; equivalência com arquivos atualmente executados em produção é **[nao verificado]**.
- PreToolUse lê `payload.agent_id` em `bin/disjuntor.py:1429`; ausência permite saída imediata para Bash comum (`bin/disjuntor.py:1431`). A cópia prova que o código espera esse campo; não prova que o harness realmente o envia **[nao verificado]**.
- B1 mede exclusivamente quando PreToolUse de agente chama `b1_do_agente`; `SubagentHandback` é excluído e DIFF suspende esse ramo (`bin/disjuntor.py:1435`).
- Stop **não mede B1**: lê `b1_disparados`, cruza com `agentes_sem_entrega` e apenas compõe texto `TaskStop` (`bin/hook_stop_fila1.py:1053`). `agentes_sem_entrega` reconhece só agentes lançados async e notificações de fim no transcript principal (`bin/disjuntor.py:366`, `:376`).
- Stop lê apenas `transcript_path` do payload; não distingue por `agent_id` (`bin/hook_stop_fila1.py:1184`). O recado B1 só é acrescentado no caminho final que imprime `decision:block`; saídas antecipadas podem evitá-lo (`bin/hook_stop_fila1.py:1207`, `:1297`, `:1300`).
- Consequência para E2: remover tokens e mudar 15→10 **não detecta um Bash já rodando sem nova chamada**. A memória de 60 s não agenda medidas; Stop não as produz (`bin/disjuntor.py:478`, `:1436`; `bin/hook_stop_fila1.py:1054`).
- DIFF usa tentativa de `flock` compartilhado em `logs/sombra.lock`; lock ocupado ou ilegível suspende, arquivo inexistente não suspende (`bin/disjuntor.py:307`). Porém Stop não consulta essa guarda antes de pedir TaskStop de B1 já disparado: estado latched anterior + lock adquirido depois ainda gera pedido (`bin/hook_stop_fila1.py:1039`, `:1054`).
- Chave: `desligado` testa `lexists` (`bin/disjuntor.py:144`); PreToolUse chama guarda da chave antes de respeitar desligamento (`bin/disjuntor.py:1486`); Stop respeita desligamento (`bin/hook_stop_fila1.py:1039`). Comandos Bash que apenas mencionam `DISJUNTOR_DESLIG` também são negados, inclusive leitura/git legítimos (`bin/disjuntor.py:160`).
- Painel: `gerar_estado.py` chama `disjuntor.ler/linha` (`bin/gerar_estado.py:279`); `ler` conta janela móvel de 86.400 s, não dia civil (`bin/disjuntor.py:1548`); `linha` enumera B1/B2/B3/B4/B4-CODEX/ANEXO-ENXUTO (`bin/disjuntor.py:1679`).
- Hook Stop consulta `stat` desses arquivos para impedir que atualizações do próprio painel contem como prova de espera; não lê o conteúdo nessa função (`bin/hook_stop_fila1.py:585`, `:590`, `:598`).

## 31/10 16:40 · sessao 0449d96ad80d · PADRAO-DE-CIMA · leitura · origem=Code

1. **Parecer geral: há infraestrutura reaproveitável, mas o PADRAO-DE-CIMA ainda precisa de uma decisão própria.** O B3 atual agrupa por **obra**, conta reprovações finais/triagens procedentes e dispara em limiares diferentes dos desenhos por entidade; sua transição não implementa `visto_em`, janela de desenhos ou episódios nomeados. (`bin/disjuntor.py:878`, `bin/disjuntor.py:891`, `bin/disjuntor.py:896`, `bin/disjuntor.py:912`)

   Duas premissas do anexo estão desatualizadas: **o parser já extrai `CODEX_OBRA`**, e **exceções do envelope já negam no ramo B3**. (`bin/disjuntor.py:1337`, `bin/disjuntor.py:1496`)

   Os anexos fornecidos não contêm linhas reais do RELATO nem conteúdo de `disjuntor_estado.json`; todos os cenários abaixo são sintéticos, e sua ocorrência em produção é **[nao verificado]**.

2. **Linha “quem chama”: pontos de entrada existem; falta conectar a nova decisão.**

   O PreToolUse chama `disjuntor.py pretool`; o Stop consulta `julga_b3`; o wrapper consulta diretamente `--b3-parada`, depois de esperar pelo slot e obter a trava da resposta. Esses são os pontos existentes para integrar o instrumento. (`.claude/settings.json:68`, `bin/hook_stop_fila1.py:1048`, `bin/codex_pergunta.sh:156`, `bin/codex_pergunta.sh:162`, `bin/codex_pergunta.sh:165`)

   **Contraexemplo:** entidade `Batida` disparada para `OBRA-A`; chamada feita por `export CODEX_OBRA=OBRA-A; CODEX_PAPEL=verificador bin/codex_pergunta.sh …`. O parser da invocação não recupera a obra exportada no trecho anterior; implementar PADRAO somente no PreToolUse deixaria essa passagem descoberta. O wrapper recebe a obra efetiva e hoje protege apenas pelo B3. Cenário **[nao verificado]**; comportamento fundamentado em `bin/disjuntor.py:1292`, `bin/disjuntor.py:1335`, `bin/disjuntor.py:1443`, `bin/codex_pergunta.sh:44`, `bin/codex_pergunta.sh:165`.

3. **Linha “quem espera” + regra BLOQUEIO: implementação parcial; o alcance atual é menor.**

   `_obra_de()` já identifica obra em prompt, e `_nega()` já emite a negativa de ferramenta. Entretanto, o ramo B3 de Agent exige **verificador e leitura final**: não alcança construtor nem verificador de desenho. (`bin/disjuntor.py:504`, `bin/disjuntor.py:1395`, `bin/disjuntor.py:1454`)

   **Contraexemplo:** estado pretendido `Batida.disparado=true`, obras alcançadas `{OBRA-A}`. `Agent(prompt="Construa a obra OBRA-A")` e verificador com “leitura de desenho da obra OBRA-A” passam pelo filtro B3 atual; uma nova decisão precisa alcançá-los, supondo os demais disjuntores livres. Cenário **[nao verificado]**; filtros em `bin/disjuntor.py:114`, `bin/disjuntor.py:499`, `bin/disjuntor.py:1454`.

   **Limite oposto:** bloquear toda obra do programa alcançaria `OBRA-C`, que nunca produziu evento na entidade, contrariando o escopo declarado. Esse estado e essa associação são **[nao verificado]**; o código atual não fornece agrupamento por entidade, apenas `b3[obra]`. (`bin/disjuntor.py:891`, `bin/disjuntor.py:897`)

4. **Linha FALHA/LIMITE + regra NAO_SEI: não se pode copiar o comportamento do B3 sem restringi-lo.**

   Estado ilegível já levanta erro; `julga_b3()` transforma a exceção em motivo de desconhecimento, e `estado_b3()` devolve `NAO_SEI` para **qualquer obra consultada** quando existe esse motivo. Isso é mais amplo que “somente as obras das entidades disparadas no último estado legível”. (`bin/disjuntor.py:265`, `bin/disjuntor.py:291`, `bin/disjuntor.py:977`, `bin/disjuntor.py:986`)

   **Contraexemplo:** último estado legível alcançava apenas `{OBRA-A}`; o JSON vira `{lixo`; consulta de `OBRA-B` também é recusada pelo B3 atual. Para o novo ramo, seria bloqueio indevido. Cenário **[nao verificado]**; decisão em `bin/disjuntor.py:982`, `bin/disjuntor.py:992`, `bin/disjuntor.py:1046`.

   Falta uma fonte recuperável do **último escopo legível**: os leitores atuais retornam o estado presente ou levantam erro; não recuperam uma fotografia anterior. Guardar essa fotografia apenas dentro do JSON que pode ficar ilegível não resolve a recuperação. (`bin/disjuntor.py:259`, `bin/disjuntor.py:291`)

   RELATO ilegível também exige adaptação: `_topo_consistente()` retorna `None`, mas a decisão B3 apenas impede liberação nesse caso; o motivo explícito de `NAO_SEI` nessa transição vem da triagem ilegível. (`bin/disjuntor.py:681`, `bin/disjuntor.py:884`, `bin/disjuntor.py:925`, `bin/disjuntor.py:964`)

   A garantia de **consumir antes de liberar** já tem molde utilizável: gravação sob trava, confirmação de persistência e cancelamento da liberação quando a escrita falha. (`bin/disjuntor.py:237`, `bin/disjuntor.py:274`, `bin/disjuntor.py:958`)

5. **Linha MEDIDA: painel e ESTADO existem; as medidas novas não.**

   Há escritor de disparos, agregador de trilha e formatador do painel; o gerador de ESTADO importa esse mesmo agregador/formatador. (`bin/disjuntor.py:139`, `bin/disjuntor.py:1546`, `bin/disjuntor.py:1677`, `bin/gerar_estado.py:280`)

   Faltam desenhos por entidade, liberações do novo instrumento e eventos `entidade=None`: o retorno atual expõe medidas B3, e a apresentação enumera IDs fixos. (`bin/disjuntor.py:1615`, `bin/disjuntor.py:1620`, `bin/disjuntor.py:1679`)

   **Contraexemplo:** acrescentar somente `DISJUNTOR PADRAO-DE-CIMA` à trilha permitiria ao agregador contar o ID, mas ele continuaria ausente da enumeração apresentada; dois eventos sem entidade também não apareceriam como tal. Cenário **[nao verificado]**; agregação/apresentação em `bin/disjuntor.py:1578`, `bin/disjuntor.py:1679`.

6. **Linha “quem desliga” + regra DESLIGAMENTO: a chave existe, mas há uma falha na porta do wrapper.**

   `desligado()` consulta `DISJUNTOR_DESLIGADO` por `lexists`; PreToolUse e Stop respeitam essa chave. A guarda também impede ferramentas do Code que nomeiem a chave ou removam o hook por determinadas edições. (`bin/disjuntor.py:144`, `bin/disjuntor.py:152`, `bin/disjuntor.py:1486`, `bin/hook_stop_fila1.py:1039`)

   Porém o wrapper chama `--b3-parada` incondicionalmente para verificador; o caminho CLI → `b3_parada()` → `estado_b3()` não consulta a chave. (`bin/codex_pergunta.sh:164`, `bin/disjuntor.py:1709`, `bin/disjuntor.py:1042`, `bin/disjuntor.py:989`)

   **Contraexemplo:** chave presente, `b3["OBRA-A"].disparado=true`, fontes legíveis. O hook fica desligado, mas o wrapper ainda recusa a chamada. Reutilizar essa porta para PADRAO exige corrigir também o desligamento nela. Cenário **[nao verificado]**; `bin/disjuntor.py:1487`, `bin/disjuntor.py:1043`, `bin/codex_pergunta.sh:165`.

7. **Regra EVENTO: falta o reconhecedor de eventos do RELATO vivo.**

   O B3 atual obtém reprovações de transcripts e triagens; seu reconhecedor de linhas do RELATO serve à **liberação**, aceitando `DESENHO` e `ERRO-BOBO`, não às três formas de evento contratadas. (`bin/disjuntor.py:736`, `bin/disjuntor.py:883`, `bin/disjuntor.py:626`)

   `_topo_consistente()` lê os bytes completos, mas devolve somente as duas primeiras linhas úteis. Reutilizar diretamente sua saída para contar desenhos perderia eventos no restante do RELATO. (`bin/disjuntor.py:662`, `bin/disjuntor.py:679`, `bin/disjuntor.py:686`, `bin/hook_stop_fila1.py:439`)

   **Contraexemplo sintético:** estas duas linhas, no corpo depois do topo, deveriam produzir dois eventos `Batida`, mas não seriam vistas por esse recorte **[nao verificado]**:

   ```text
   DESENHO OBRA-A: pergunta=x app/ponto/registro_batida.py:152
   PERGUNTA DE DESENHO OBRA-B: pergunta=y app/ponto/registro_batida.py:157
   ```

   Os dois sítios são escritas explícitas de `Batida`. (`app/ponto/registro_batida.py:152`, `app/ponto/registro_batida.py:157`)

   Para evitar falsos eventos, o reconhecedor novo precisa distinguir as formas válidas de `~~DESENHO…~~`, `DESENHO <obra>: …` e prosa como `resultado = DESENHO`. Não há esse reconhecedor na decisão atual. (`bin/disjuntor.py:878`, `bin/disjuntor.py:930`)

8. **Regra TEMPO: hash reaproveitável como mecanismo; identidade e janela ainda precisam ser implementadas.**

   `_sha_b3()` calcula SHA da linha com normalização própria do topo; não registra primeira observação. A janela de 24 horas existente no painel usa timestamps da **trilha**, não `visto_em` de desenhos. (`bin/disjuntor.py:571`, `bin/disjuntor.py:1548`, `bin/disjuntor.py:1563`)

   **Contraexemplo de retorno:** evento com `visto_em=T−90000` some e volta em `T`; recriar seu registro com `visto_em=T` faria um evento antigo voltar à janela. É necessário preservar a identidade mesmo fora da janela. Estado sintético **[nao verificado]**; esse histórico não pertence ao estado B3 hoje manipulado. (`bin/disjuntor.py:891`, `bin/disjuntor.py:936`)

   **Contraexemplo de normalização:** acrescentar `RESPONDIDA` à mesma linha muda o hash de `_sha_b3()`. Usá-lo diretamente pode transformar uma atualização do mesmo desenho em segundo evento. (`bin/disjuntor.py:573`) A normalização contratual que impediria essa duplicação não foi especificada **[nao verificado]**.

9. **Regra ENTIDADE: o censo fornece insumos, mas não o resolvedor completo.**

   `varrer_arvore()` expõe `arquivo`, `linha`, `entidade`, `op` e `expr`, com caminhos relativos a `app/`. Não expõe função/intervalo nem flag estruturada `DIRETA`; esses vínculos precisam de adaptação. (`app/core/censo_escritas.py:361`, `app/core/censo_escritas.py:469`, `app/core/censo_escritas.py:489`)

   Não basta aceitar `entidade != None`: o censo também atribui entidades por inferência de campo, relacionamento e nome da variável. (`app/core/censo_escritas.py:387`, `app/core/censo_escritas.py:413`, `app/core/censo_escritas.py:420`, `app/core/censo_escritas.py:424`)

   **Contraexemplo:** `app/ponto/registro_batida.py:254` contém `batida.save(update_fields=list(campos))`; a atribuição por nome não pode ser promovida automaticamente a DIRETA. Agrupá-la com um evento direto de `Batida` pode provocar disparo indevido e impedir o fallback ao programa. Cenário **[nao verificado]**; escrita e inferência em `app/ponto/registro_batida.py:254`, `app/core/censo_escritas.py:424`.

   `modelos()` atende literalmente à busca por nomes de classes, mas inclui qualquer classe de topo com bases, exceto nomes terminados em `QuerySet`/`Manager`. Por exemplo, inclui a exceção `ResolvedoraCruzada(ValueError)`: duas citações poderiam agrupar uma “entidade” que não é modelo Django. (`app/core/censo_escritas.py:89`, `app/core/censo_escritas.py:108`, `app/chamados/models.py:2179`, `app/core/tests/test_censo_escritas.py:264`)

   O fallback BACKLOG → PROGRAMA ainda carece de resolvedor: as relações documentais aparecem como “1o do programa”, “complemento” e “linha da E4”. Uma busca simples por token não cobre essas formas. (`app/docs/BACKLOG.md:771`, `app/docs/BACKLOG.md:773`, `app/docs/BACKLOG.md:774`)

10. **Regra GATILHO: exige contador por entidade e nomeação por associação evento–entidade.**

    O contador atual mede reprovações e voltas Codex por obra; não implementa `>=2 eventos na janela` e `>=1 não nomeado`. (`bin/disjuntor.py:909`, `bin/disjuntor.py:912`)

    **Contraexemplo de redisparo:** `e1` e `e2` são `Batida`, continuam na janela e foram nomeados; chega `e3`, também `Batida`, não nomeado. Um simples marcador permanente “entidade liberada” deixaria passar o terceiro evento; o contrato exige novo disparo. Estado sintético **[nao verificado]**; o mecanismo atual de exclusão é por IDs liberados/rearmados do B3. (`bin/disjuntor.py:898`, `bin/disjuntor.py:905`)

    **Contraexemplo com múltiplas entidades:** `e1` pertence a `{Batida, CelulaDia}`. Liberar `Batida` e escrever um único booleano global `e1.nomeado=true` também nomearia indevidamente sua participação em `CelulaDia`. O registro precisa distinguir essas associações. Proposta e cenário **[nao verificado]**.

    A permanência de um disparo depois de todos os eventos saírem da janela não está definida no anexo; copiar o travamento persistente do B3 escolheria esse comportamento implicitamente. **[nao verificado]**; persistência atual em `bin/disjuntor.py:912`, `bin/disjuntor.py:922`, `bin/disjuntor.py:938`.

11. **Regra LIBERAÇÃO: atomicidade aproveitável; sintaxe e episódio são novos.**

    `linhas_libera_b3()` não reconhece `PADRAO-DE-CIMA <entidade>: classe=… padrao=… armadilha=… obra=…`; aceita somente o desenho B3 ou erro bobo. (`bin/disjuntor.py:626`, `bin/disjuntor.py:639`, `bin/disjuntor.py:650`)

    Há um extrator de vocabulário em `bin/tests/test_lei_akita.sh`, dentro de heredoc Python. Ele consulta GLOSSARIO/PLANO por `git show`, compara termos completos e não oferece uma regra independente para “nome clássico declarado”. (`bin/tests/test_lei_akita.sh:65`, `bin/tests/test_lei_akita.sh:75`, `bin/tests/test_lei_akita.sh:89`, `bin/tests/test_lei_akita.sh:138`) A forma dessa declaração continua **[nao verificado]**.

    **Contraexemplo de liberação insuficiente:** entidade disparada com eventos de `{OBRA-A, OBRA-B}`; aplicar o consumo atual apenas a `b3[OBRA-A]` não nomeia o episódio da entidade nem libera coerentemente as duas obras. O consumo atual pertence a cada obra. Cenário **[nao verificado]**; `bin/disjuntor.py:909`, `bin/disjuntor.py:932`, `bin/disjuntor.py:939`.

    **Contraexemplo de persistência:** se a gravação do consumo falhar, responder “liberado” permitiria reutilizar a mesma linha na chamada seguinte. O B3 já oferece o tratamento correto dessa falha, restaurando a negativa após escrita malsucedida. (`bin/disjuntor.py:958`, `bin/disjuntor.py:960`)

12. **Campos de estado: hoje não há escritores/leitores dos campos novos; há estes donos concretos da infraestrutura existente.**

    | Campo/artefato existente | Quem escreve | Quem lê |
    |---|---|---|
    | `disjuntor_estado.json` físico | `_com_estado`, temporário e substituição: `bin/disjuntor.py:237`, `bin/disjuntor.py:271` | `_com_estado` e `ler_estado`: `bin/disjuntor.py:260`, `bin/disjuntor.py:291` |
    | `b3[obra].reprovados`, `codex`, `disparado` | `decide_b3`: `bin/disjuntor.py:900`, `bin/disjuntor.py:907`, `bin/disjuntor.py:913` | `decide_b3`: `bin/disjuntor.py:909`; resultado por `estado_b3`: `bin/disjuntor.py:989` |
    | `disparado_em`, `topo_no_disparo` | `_fotografa`: `bin/disjuntor.py:727`, `bin/disjuntor.py:730` | `decide_b3`: `bin/disjuntor.py:927`, `bin/disjuntor.py:934` |
    | `consumidas`, `liberado` | `decide_b3`: `bin/disjuntor.py:936`, `bin/disjuntor.py:939` | `decide_b3`: `bin/disjuntor.py:898`, `bin/disjuntor.py:932` |
    | `<resposta>.id`: obra/papel/hash/pid/fim/sha | Wrapper: `bin/codex_pergunta.sh:198` | `_le_id` e triagem: `bin/disjuntor.py:753`, `bin/disjuntor.py:795` |
    | `triagem.log`; `b3_triagem_linhas` | `_triagem_sob_trava`: `bin/disjuntor.py:812`, `bin/disjuntor.py:815`; contador também atualizado em `bin/disjuntor.py:853` | `_triagens`: `bin/disjuntor.py:823` |
    | `disjuntor.log` | `trilha`/`dispara`: `bin/disjuntor.py:128`, `bin/disjuntor.py:139` | `ler` → `linha` → ESTADO: `bin/disjuntor.py:1554`, `bin/disjuntor.py:1677`, `bin/gerar_estado.py:280` |

    Para os campos novos, o mapa necessário seria o seguinte; **nomes de estruturas e donos futuros são proposta, sem implementação [nao verificado]**:

    | Novo estado necessário | Escritor proposto | Leitores propostos |
    |---|---|---|
    | SHA, `visto_em`, obra e entidades de cada evento | Varredura na decisão única | Janela, agrupamento e painel |
    | Nomeação por `(SHA, entidade)`/episódio | Transição de liberação | Gatilho e redisparo |
    | Entidade disparada e obras alcançadas | Transição de disparo/liberação | PreToolUse, wrapper e Stop |
    | SHA de nomeação consumido | Liberação, antes de autorizar | Próximas liberações |
    | Último escopo legível recuperável | Persistência da decisão válida | Tratamento restrito de `NAO_SEI` |
    | Contagens de desenhos, disparos, liberações e `None` | Decisão/registro de transições | Painel e ESTADO |

13. **Parser de CODEX_OBRA: mora hoje em `disjuntor.py`; é diferente do parser de prompts.**

    **Comando Bash:** `_trechos()` separa invocações; `_atribuicoes()` usa `shlex.split()`, remove aspas e conserva a última atribuição; `_codex_cmds()` retorna `obra`. Não aplica a regex de nomes de `_obra_de()`. (`bin/disjuntor.py:1251`, `bin/disjuntor.py:1309`, `bin/disjuntor.py:1325`, `bin/disjuntor.py:1337`)

    **Wrapper:** recebe o valor efetivo de `CODEX_OBRA` e exige apenas que não esteja vazio; assim aceita `obra_teste` e `O9 (C)`, embora o primeiro não seja reconhecido por `_obra_de()`. (`bin/codex_pergunta.sh:44`, `bin/codex_pergunta.sh:45`, `bin/disjuntor.py:97`, `bin/disjuntor.py:521`)

    **Prompt de Agent:** aceita âncoras como `obra X`, `Obra: X`, Markdown e `leitura … da X`; escolhe a primeira ocorrência válida. IDs aceitam formas como `PADRAO-DE-CIMA`, `OBRA-x` e `O261`; IDs sem hífen podem carregar cauda: `O9 (c)` vira `O9 (C)`. Um ID com hífen descarta essa cauda: `OBRA-X (C)` vira `OBRA-X`. (`bin/disjuntor.py:97`, `bin/disjuntor.py:104`, `bin/disjuntor.py:504`, `bin/disjuntor.py:518`, `bin/disjuntor.py:526`)

    Há colisão por ausência de borda direita: `obra OBRA-X_Y` é lida como `OBRA-X`. Também há risco quando um prompt menciona outra obra antes da obra executada. (`bin/disjuntor.py:103`, `bin/disjuntor.py:104`, `bin/disjuntor.py:514`, `bin/tests/test_disjuntor.sh:693`)

    **Limites concretos do parser Bash:**

    | Forma | Resultado atual |
    |---|---|
    | `CODEX_OBRA="O9 (C)" CODEX_PAPEL=verificador …` | Extrai `O9 (C)`. (`bin/disjuntor.py:1314`, `bin/disjuntor.py:1337`) |
    | `env CODEX_OBRA=OBRA-A CODEX_PAPEL=verificador …` | Não reconhece essa disposição: atribuições são previstas antes dos prefixos. (`bin/disjuntor.py:1243`) |
    | `export CODEX_OBRA=OBRA-A; CODEX_PAPEL=verificador …` | Não transporta a obra entre trechos. (`bin/disjuntor.py:1292`, `bin/disjuntor.py:1335`) |
    | `CODEX_OBRA="$OBRA" CODEX_PAPEL=verificador …` | Extrai o literal `$OBRA`, sem expansão. (`bin/disjuntor.py:1314`, `bin/disjuntor.py:1321`) |

    Mudar a identidade de obra afeta contagem de transcripts, seleção de Agent, classificação de exceções e reconhecimento de liberação. As chaves persistidas e a triagem usam a obra como texto; uma nova normalização pode separar histórico antigo e chamadas novas. (`bin/disjuntor.py:557`, `bin/disjuntor.py:637`, `bin/disjuntor.py:850`, `bin/disjuntor.py:897`, `bin/disjuntor.py:1416`, `bin/disjuntor.py:1455`)

14. **Regra MORDE: os casos precisam testar a decisão nova, com agrupamento explícito.**

    As escritas abaixo permitem montar casos com entidades distintas sem banco: `registro_batida.py:152` e `:157` escrevem `Batida`; `portas/celula.py:247` escreve `CelulaDia`. (`app/ponto/registro_batida.py:152`, `app/ponto/registro_batida.py:157`, `app/ponto/portas/celula.py:247`)

    Com SHAs distintos, `visto_em=T−60/T−30` e ambos não nomeados, os resultados contratuais esperados seriam estes; execução do instrumento/mutante **[nao verificado]**:

    | Eventos | Programa | Esperado |
    |---|---|---|
    | `Batida`, `Batida` | Qualquer | Dispara |
    | `Batida`, `CelulaDia` | Diferentes | Não dispara |
    | `Batida`, `CelulaDia` | Mesmo programa | Não dispara |
    | `None`, `None` | Não resolvido | Mede dois; não agrupa |
    | Dois `Batida` nomeados + terceiro novo | Qualquer | Redispara |

    O mutante “conta global” deve falhar nos dois casos de entidades distintas; o terceiro caso também detecta uma implementação que soma programa apesar de entidade resolvida. Resultado efetivamente VERMELHO **[nao verificado]**.

    Os selos existentes criam arquivos e mutantes; sua execução não cabe na restrição de somente leitura desta auditoria. (`bin/tests/test_disjuntor.sh:39`, `bin/tests/test_disjuntor.sh:1478`, `bin/tests/test_disjuntor.sh:1487`)

## 31/10 16:38 · sessao eb6a9f7f9f33 · F1C4-3-EMISSORES · verificador · origem=Code

A delegação do ramo do supra está implementada, e os dois ramos do cartório entregam atas montadas no julgamento corrente. A exceção `_protesta` continua real: ela impede que a primeira cláusula do juiz seja alcançada pelo cartório quando `trabalha=False`. (`app/ponto/supra_juiz.py:65-71`; `app/ponto/services/cartorio.py:1012-1029`, `:1184-1214`)

Há também um defeito adicional no delta: a nova deduplicação de Pauta permite duplicatas sob concorrência, por intercalação entre a consulta e a criação. Ocorrência efetiva em produção: **[não verificado]**. (`app/chamados/services/fabrica_da_celula.py:180-185`; `app/pautas/services.py:152-185`; `app/pautas/models.py:113-133`)

**1. Escritores, leitores e autoridades**

O predicado efetivo do juiz é:

```python
cel.trabalha is False or bool(
    ata.get('tipo_dia') and ata.get('tipo_dia') != 'trabalho'
)
```

Portanto, a segunda cláusula exige um valor verdadeiro: `None` e `''` não bastam; `trabalha=None` pode ser calado por uma ata preenchida com tipo diferente de trabalho. (`app/chamados/juizes.py:407-408`)

| Campo/valor | Quem escreve | Quem lê e como decide |
|---|---|---|
| `CelulaDia.trabalha` | Geração/regeneração pela porta, a partir do calendário calculado; há também limpeza para `False` e reversão pelo DNA anterior. (`app/ponto/portas/celula.py:231-260`, `:532`, `:590-615`) | O juiz lê `is False`; fábrica, materialização, cluster e intervalo passaram a consultá-lo. (`app/chamados/juizes.py:408`; `app/chamados/services/fabrica_da_celula.py:80`; `app/chamados/services/materializacao.py:160`; `app/ponto/services/cluster_espurio.py:28`; `app/ponto/management/commands/detectar_intervalo_ausente.py:51`) |
| `ata.tipo_dia` | `ata_do_dia` copia o tipo da grade; a persistência passa por `lavrar_veredito`. (`app/ponto/services/cartorio.py:568`; `app/escala/models.py:1787-1796`; `app/ponto/portas/celula.py:726-768`) | O juiz aplica a segunda cláusula. `grade_da_celula` mantém projeção própria de tipo; declaração mantém veto próprio por tipo. (`app/chamados/juizes.py:408`; `app/escala/services/leitor_celula.py:345-346`; `app/chamados/services/declaracao_texto.py:95`) |
| `ata.minutos_previstos` | Cartório grava `int(dia.get('minutos_previstos') or 0)` pela mesma porta. (`app/ponto/services/cartorio.py:569`; `app/ponto/portas/celula.py:726-768`) | O leitor usa o número da ata; se ausente, consulta `minutos_previstos_do_dia` somente quando `cel.trabalha` é verdadeiro, senão usa zero. O supra lê a projeção em `prev`. Esses passos não consultam `dia_sem_trabalho`. (`app/escala/services/leitor_celula.py:347-354`; `app/ponto/supra_juiz.py:223`) |
| `ata.lampadas` | `lampadas_do_dia` monta; `ata_do_dia` incorpora; dispensa por ausência/justificativa e teto podem alterar as luzes antes da lavra. (`app/ponto/services/cartorio.py:541-569`, `:611-624`, `:707-717`) | Materialização lê slots/luzes; cluster conta apagadas; intervalo indexa lâmpadas; leitor projeta células; `lastro` procura os marcos cobrados. São perguntas sobre os marcos, adicionais ao gate de dia sem trabalho. (`app/chamados/services/materializacao.py:158-164`; `app/ponto/services/cluster_espurio.py:29-31`; `app/ponto/management/commands/detectar_intervalo_ausente.py:64`; `app/escala/services/leitor_celula.py:92-121`; `app/ponto/services/lastro.py:82-95`) |
| `INSUMO_DIA_SEM_TRABALHO` | Constante do supra; escrita pelo adaptador e por `grade_da_celula`. Cartório usa o adaptador nos dois ramos. (`app/ponto/supra_juiz.py:57-62`; `app/escala/services/leitor_celula.py:370`; `app/ponto/services/cartorio.py:1029`, `:1213`) | `_dia_sem_trabalho_pelo_juiz` exige a chave e entrega `trabalha` e `ata` ao juiz, sem predicado substituto. (`app/ponto/supra_juiz.py:65-71`) |
| `chamados_vivos` | Commands passam `len(vivos)`, selecionados por `VIVOS`; cartório normal passa `len(_protestam)`; `nunca_bateu` passa zero. (`app/chamados/catalogo/motor.py:24`; `app/ponto/management/commands/supra_juiz.py:194-208`; `app/ponto/management/commands/emitir_furo_retroativo.py:92-101`; `app/ponto/services/cartorio.py:1029`, `:1206-1214`) | O supra exige contagem verdadeira nesse ramo. A contagem dos commands não consulta o juiz de dia; a do cartório passa pelo predicado próprio `_protesta`. (`app/ponto/supra_juiz.py:470-472`; `app/ponto/services/cartorio.py:1206-1208`) |
| `cobertura_status` | Vem de `ausencia_cobre(..., para='cobranca')`, por `getattr(cob, 'status', None)`, nos commands e no cartório. (`app/ponto/management/commands/supra_juiz.py:199-209`; `app/ponto/management/commands/emitir_furo_retroativo.py:97-102`; `app/ponto/services/cartorio.py:982`, `:1030`, `:1108`, `:1216`) | O ramo exige **exatamente `None`**, não apenas “não aprovada”. É guarda de cobertura, não outro juiz de dia sem trabalho. (`app/ponto/supra_juiz.py:471`) |
| Novos `Pauta.texto`/`ancora_id` | Fábrica constrói `colaborador_id:AAAA-MM-DD`, marcador e tags; chama `escrever`, que normaliza, acrescenta contexto e persiste. Não há criação direta de Pauta pela fábrica. (`app/chamados/services/fabrica_da_celula.py:175-190`; `app/pautas/services.py:163-185`) | A fábrica relê âncora/texto para deduplicar; API expõe ambos; painel mostra texto e tenta navegar pela âncora; chip consulta a âncora. Não são decisões de cobrança. (`app/chamados/services/fabrica_da_celula.py:180-183`; `app/pautas/views.py:40-41`; `app/templates/core/_copiloto.html:405`, `:564-573`; `app/pautas/services.py:334-338`) |

A fábrica consulta o juiz na entrada, mas depois passa por `motivos_pela_ata → _ata_do_dia → _marcos_faltantes`, onde sobrevive o veto próprio `tipo_dia != 'trabalho'`. Portanto, delegar o primeiro gate não eliminou todos os predicados do caminho. (`app/chamados/services/fabrica_da_celula.py:49`, `:80`, `:111`; `app/chamados/services/declaracao_texto.py:79-96`)

Os leitores anteriores `_celula_furada`, `ChamadoColaborador` e `reconciliar_grade` também consultam o juiz; possuem seus próprios universos e guardas ao redor da consulta. (`app/chamados/juizes.py:411-443`; `app/chamados/models.py:1227-1232`; `app/ponto/management/commands/reconciliar_grade.py:274-282`)

Nos testes, há escrita direta de `trabalha/ata` para construir as situações adversariais; isso não representa um novo escritor de produção. (`app/chamados/tests/test_f1c4_3_emissores.py:65-66`; `app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:43`, `:100-101`)

**2. Produtores do dict, ausência de insumo e idade da ata**

| Caminho | Produção do dict e ata entregue |
|---|---|
| Cron/command `supra_juiz` | Usa `grade_da_celula`: insumo com `cel.trabalha` e **ata já gravada**. (`app/ponto/management/commands/supra_juiz.py:116`, `:206-209`; `app/escala/services/leitor_celula.py:332`, `:370`) |
| `emitir_furo_retroativo` | Mesmo leitor e **ata já gravada**. (`app/ponto/management/commands/emitir_furo_retroativo.py:59`, `:99-102`; `app/escala/services/leitor_celula.py:332`, `:370`) |
| Cartório, `nunca_bateu` | Monta `_ata_nb` neste julgamento, entrega-a ao helper e depois à lavra. (`app/ponto/services/cartorio.py:1012`, `:1029`, `:1044-1045`) |
| Cartório, normal | Monta `_ata_agora.ata`, publica em `_tl.atas` e entrega esse objeto ao helper antes da lavra final. (`app/ponto/services/cartorio.py:1184-1188`, `:1213`, `:1563-1565`) |
| Evento `julgar_celula` | Carrega a célula e chama `julgar_colab`; desemboca nos mesmos dois ramos, não cria outro adaptador de dict. (`app/ponto/services/cartorio.py:1708-1723`, `:777`) |

O leitor produz `fora_vinculo` e `sem_celula` **sem insumo**, mas ambos são barrados por `SEM_JURISDICAO` antes do acesso obrigatório. Com célula no ramo normal do leitor, a chave é acrescentada. (`app/escala/services/leitor_celula.py:340-370`; `app/ponto/supra_juiz.py:48`, `:470-472`)

Nos caminhos de produção identificados, não encontrei um dict sem insumo que alcance esse acesso: são os dois commands e as duas chamadas do cartório. A catraca declara esses mesmos arquivos. Ausência efetiva de `KeyError` em produção: **[não verificado]**. (`app/ponto/management/commands/supra_juiz.py:206`; `app/ponto/management/commands/emitir_furo_retroativo.py:99`; `app/ponto/services/cartorio.py:1028`, `:1212`; `app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:161-165`)

Nos testes existem estes modos de produção do dict: [nao verificado]

- Pelo leitor real, após alteração da célula: recebe ata gravada na fixture e insumo do leitor. (`app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:39-45`)
- Pelo cartório real, com espião no adaptador: recebe ata corrente. (`app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:89-104`)
- Pelo adaptador em fixtures de folga/indefinido. (`app/ponto/tests/test_f1c4_3_supra_indefinido_nao_retrata.py:17-24`; `app/ponto/tests/test_supra_juiz.py:231-235`; `app/ponto/tests/test_supra_juiz_cobranca.py:102-105`)
- Por dict manual com insumo explícito, inclusive tipo externo diferente do tipo na ata. (`app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:147-155`)
- Por dict manual sem insumo: deliberadamente para exigir `KeyError`, testar `SEM_JURISDICAO` ou exercitar caminhos barrados por previsto/chamados/cobertura. (`app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:135-145`; `app/ponto/tests/test_supra_juiz_cobranca.py:107-119`; `app/ponto/tests/test_supra_juiz_indefinido.py:21-48`)

**`tipo_dia='trabalho'` não protege mais um dict sem insumo**: previsto zero, chamados vivos e cobertura `None` fazem esse dict chegar ao acesso obrigatório. A proteção por tipo agora cobre somente `SEM_JURISDICAO`. (`app/ponto/supra_juiz.py:470-472`)

**3. Sítios que conservam outra decisão**

| Sítio | Predicado e universo | Equivalência ao juiz |
|---|---|---|
| Cartório `_protesta` | `trabalha is not False and marcos_acesos(...) is not True`, sobre chamados vivos; também participa de `cobrado`, adiamento e reconciliação. (`app/ponto/services/cartorio.py:1206-1214`, `:1250`, `:1281-1287`) | **Não**: ignora `ata.tipo_dia` e considera satisfação dos marcos. (`app/ponto/services/cartorio.py:1207`; `app/chamados/juizes.py:408`) |
| Cartório, emissão total | Exige `ec.eh_dia_trabalho(data) is True`, depois dos gates de furo, situação, piso e afastamento. (`app/ponto/services/cartorio.py:1492-1507`) | **Não**: com célula, essa autoridade lê `trabalha`, sem a segunda cláusula da ata. (`app/escala/models.py:1066-1073`) |
| Cartório `_marcos_do_furo_parcial` | Recusa vínculo ausente ou `eh_dia_trabalho is not True`. (`app/ponto/services/cartorio.py:1604-1609`) | **Não**, pela mesma distinção entre previsão e ata. (`app/escala/models.py:1071-1073`) |
| Declaração `_marcos_faltantes` | Só aceita tipo projetado `trabalho`; lê dia e véspera, desta aproveitando somente saída. (`app/chamados/services/declaracao_texto.py:79-96`, `:116-123`) | **Não**: não aplica a cláusula soberana quando a ata diz trabalho; universo inclui véspera. (`app/chamados/services/declaracao_texto.py:88-96`; `app/chamados/juizes.py:408`) |
| Disputa, identificação do dia | Só aceita `tipo_dia == 'trabalho'` na grade da data da entrada aberta. (`app/chamados/services/disputa_emissao.py:56-70`) | **Não**: decide pelo tipo projetado, sem consultar o juiz. (`app/chamados/services/disputa_emissao.py:65-68`) |
| Disputa `_tem_base` | Aceita batida primeiro; senão previsão `is not False`. (`app/chamados/services/disputa_emissao.py:749-753`) | **Não**: aceita `None` e não consulta o tipo da ata nesse gate. (`app/chamados/services/disputa_emissao.py:750-753`) |
| Lote de perguntas | Pré-seleciona somente `trabalha=True` na janela. (`app/chamados/management/commands/disparar_perguntas_competencia.py:74-76`) | **Não**: `None/trabalho` fica fora antes dos demais filtros. (`app/chamados/management/commands/disparar_perguntas_competencia.py:76`; `app/chamados/juizes.py:408`) |
| Fila por causa | Dias previstos são células `trabalha=True` dos colaboradores da fila. (`app/chamados/services/fila_por_causa.py:191-200`) | **Não**: considera somente o campo, sem ata. (`app/chamados/services/fila_por_causa.py:192-194`) |
| Vigia de hora | Candidatas `trabalha=True`, empresa, hoje/ontem e janela do turno. (`app/ponto/services/vigia_de_hora.py:15-26`) | **Não**: `None` não chega ao julgamento por essa seleção. (`app/ponto/services/vigia_de_hora.py:21-23`) |
| Vigia de entrada | Somente `trabalha=True` e ata com lâmpadas, na janela ou data consultada. (`app/ponto/services/vigia_entrada.py:73-85`, `:120-125`) | **Não**: mesmo recorte por `True`; é instrumento de leitura. (`app/ponto/services/vigia_entrada.py:79`, `:120`) |
| `dia_do_colab` | `not cel.trabalha` produz “Dia sem trabalho previsto”. (`app/ponto/services/dia_do_colab.py:121-123`) | **Não**: inclui `None` nessa descrição. (`app/ponto/services/dia_do_colab.py:121`; `app/chamados/juizes.py:408`) |
| `passivo_s84` | Célula truthy vira TRABALHO; `False/None` viram FOLGA, sobre batidas atribuídas ao dia-turno. (`app/ponto/services/passivo_s84.py:88-96`) | **Não**: colapsa o indeterminado em folga; a classificação participa da seleção posterior. (`app/ponto/services/passivo_s84.py:96`, `:108-112`) |
| Veto em `ponto.marcos` | `trabalha is False` dá FOLGA; depois considera vereditos vetados. (`app/ponto/marcos.py:155-163`) | Só coincide com a primeira cláusula; responde ao veto, com outro conjunto de condições. (`app/ponto/marcos.py:159-162`) |
| `flip_auto` | Célula `False` recusa dia diurno; no noturno exige véspera `True` para recuar. (`app/ponto/services/flip_auto.py:201-215`) | **Não**: decisão geométrica entre dia/véspera, sem tipo da ata. (`app/ponto/services/flip_auto.py:208-215`) |
| Worklist | Tipo projetado diferente de trabalho retorna gap indeterminado antes da leitura das lâmpadas. (`app/ponto/worklist.py:18-26`) | **Não**: não aplica a primeira cláusula soberana. (`app/ponto/worklist.py:19-22`) |
| Bordas do realizado | Só tipo projetado `trabalho`, entre vínculos ativos e dentro da janela. (`app/ponto/services/bordas_realizado.py:62-75`) | **Não**: universo de auditoria do realizado. (`app/ponto/services/bordas_realizado.py:76-87`) |
| Outros ramos do supra | Conservam gates por tipo; `dia_de_cobranca` admite ausência parcial. (`app/ponto/supra_juiz.py:281`, `:304-308`, `:402-420`, `:443`, `:476-491`) | **Não**: não foram todos migrados; ausência parcial pode ser cobrável ali enquanto o juiz cala ata `ausencia`. (`app/ponto/supra_juiz.py:299-305`; `app/chamados/juizes.py:408`) |

Há ainda recortes próprios nos instrumentos: auditor de chamados (`not trabalha`/`is not True`), esmeril (`is False`/truthy), tripwire (`True`, ou `False/None` mais ata folga), `dna_x_batida_real` e `plano_b_no_dinheiro` (`True`). Seus universos são invariantes e diagnósticos, não a mesma pergunta de cobrança do juiz. (`app/chamados/management/commands/auditar_invariantes_chamados.py:376`, `:1443`, `:1542`; `app/ponto/services/esmeril_espelho.py:71`, `:121-124`; `app/ponto/management/commands/tripwire_celulas.py:98`, `:257`, `:338-344`; `app/ponto/management/commands/dna_x_batida_real.py:58-65`; `app/ponto/management/commands/plano_b_no_dinheiro.py:142-144`)

Os leitores de **previsão** também precisam permanecer distinguidos: alerta avançado usa resultado falsey; retroativo exige `True`; detector de anomalias trata `None` como indeterminação e `False` como folga; faltas de hoje aceita `True` ou defeito cadastral; plantio overnight verifica `False`. Eles consultam a autoridade prevista, que, havendo célula, retorna `cel.trabalha`. (`app/ponto/management/commands/processar_alertas_avancados.py:34-50`; `app/ponto/management/commands/emitir_furo_retroativo.py:117-129`; `app/chamados/detector_anomalias.py:164-170`; `app/ponto/services/faltas_de_hoje.py:32-42`; `app/ponto/management/commands/vigia_plantio_overnight.py:38-50`; `app/escala/models.py:1071-1073`)

`detectar_ausencias` conserva tratamento específico do limbo previsto; propositor exige tipo trabalho em dicts; simulação e `veredito_celula` julgam grade remontada. Não são consultas equivalentes ao juiz da cobrança lavrada. (`app/ponto/management/commands/detectar_ausencias.py:136-159`; `app/ponto/propositor.py:34-37`, `:201-211`; `app/ponto/services/simular_dia.py:5-12`; `app/ponto/services/veredito_celula.py:216-222`)

A documentação reconhece parte dessa dívida em `F1C4-3-NONE-LEITORES` e `F1C4-3-EMISSORES-FILA`; não sustenta que todos esses leitores tenham migrado. (`app/docs/BACKLOG.md:805`, `:823`)

**4. Os dois testes novos** [nao verificado]

| Teste | Mutação mínima no cartório que deve deixá-lo vermelho, por análise estática | [nao verificado]
|---|---|
| `test_MORDE_ramo_nunca_bateu_entrega_a_ata_deste_julgamento` | Trocar `ata=_ata_nb` por `ata=cel.ata` em `app/ponto/services/cartorio.py:1029`: entrega a sentinela e tipo `folga`, contrariando as asserções. (`app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:100-111`, `:116-119`) |
| `test_MORDE_ramo_normal_entrega_a_ata_deste_julgamento` | Trocar `ata=_ata_agora.ata` por `ata=cel.ata` em `app/ponto/services/cartorio.py:1213`: falha pelas mesmas verificações e pela identidade com a ata em curso. (`app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:100-111`, `:121-132`) |

**Nenhum passa se o espião nunca for chamado**: `assertTrue(vistos)` antecede o laço. **Nenhum passa recebendo a ata anterior intacta**: ambos rejeitam a sentinela e exigem tipo `trabalho`. (`app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:100-114`)

Não passam por fixture vazia quanto ao **transporte observado**: exigem chamada, conteúdo e substituição da ata gravada; o normal também exige identidade com `_tl.atas`. (`app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:107-114`, `:132`)

Entretanto, não demonstram a execução do ramo de cobrança nem a chamada efetiva ao juiz: `nunca_bateu` passa zero vivos; a fixture normal começa com célula de trabalho e jornada prevista. (`app/ponto/services/cartorio.py:1029`; `app/ponto/supra_juiz.py:471-472`; `app/ponto/tests/test_bo_inss_nunca_bateu_le_a_precedencia.py:72-90`)

O teste `nunca_bateu` é mais fraco quanto à integridade da ata: trocar seu argumento por `{'tipo_dia': 'trabalho'}` satisfaz as verificações de conteúdo, mesmo omitindo minutos e lâmpadas. O normal exige identidade com a ata em curso, mas nenhum dos dois compara integralmente o insumo com a ata persistida. (`app/ponto/tests/test_f1c4_3_supra_pergunta_ao_juiz.py:106-114`, `:132`)

Execução dos testes Django e dos mutantes completos: **[não verificado]**. [nao verificado]

**5. Correspondência entre documentos e código**

- **Correto:** migração do ramo no mesmo pouso, guarda `SEM_JURISDICAO` antes do juiz e transporte pelo insumo. (`app/docs/CORTES.md:415`; `app/docs/JUIZES.md:58-59`; `app/ponto/supra_juiz.py:470-472`)
- **Correto:** ata deste julgamento nos dois ramos do cartório. (`app/docs/CORTES.md:415`; `app/ponto/services/cartorio.py:1012-1029`, `:1184-1213`)
- **Correto com qualificação:** `_protesta` conserva somente a primeira cláusula **sobre a natureza do dia**; a função inteira também pergunta por marcos acesos. “Zera chamados vivos” vale para `trabalha=False`, não incondicionalmente. (`app/docs/BACKLOG.md:807`; `app/docs/JUIZES.md:60-61`; `app/core/juizes.py:144-146`; `app/ponto/services/cartorio.py:1206-1214`)
- **Imprecisão contradita pelos casos vazios:** a fórmula documental `ata.tipo_dia != trabalho` omite a guarda de preenchimento. Para `True/None` com tipo `None/''`, o juiz não cala. (`app/docs/CORTES.md:415`; `app/docs/JUIZES.md:56`; `app/core/juizes.py:138`; `app/chamados/juizes.py:408`)
- **Imprecisão se lida literalmente:** “`trabalha=None` COBRA” não vale quando a segunda cláusula é verdadeira. `None` com ata `folga`, `ausencia`, `falta` ou `indefinido` é dia sem trabalho para o juiz. (`app/docs/JUIZES.md:57`; `app/core/juizes.py:139`; `app/chamados/juizes.py:408`)
- **Ressalva temporal ausente em `_C18`:** “célula JÁ LAVRADA” não descreve esse leitor do cartório, que consulta a ata corrente antes da persistência final; `CORTES` descreve essa exceção corretamente. (`app/core/juiz

[truncado em 20000 de 24186 caracteres]

## 31/10 16:38 · sessao 1d6ef70ea0d8 · - · - · origem=Ronald (tmux)

1. **Situação do instrumento.** O código atual é B3 por **obra**, alimentado por veredictos de transcripts e triagens Codex; o disparo usa `n>=1` reprovação final ou `nc>=3` voltas procedentes, sem entidades ou janela de desenhos (`bin/disjuntor.py:736`, `bin/disjuntor.py:862`, `bin/disjuntor.py:896`, `bin/disjuntor.py:912`). Não existem nessa transição os campos `visto_em`, entidade, eventos nomeados ou consumo de `PADRAO-DE-CIMA`; o estado manipulado é `b3`, `b3_sem_obra`, reprovações, Codex e liberações por obra (`bin/disjuntor.py:891`, `bin/disjuntor.py:936`). Resultado executado do instrumento novo: [não verificado].

2. **Quem chama / quem espera.** Há pontos de integração: PreToolUse chama `main_pretool` pela configuração; Stop chama `julga_b3` para acrescentar recados; wrapper pergunta diretamente `--b3-parada` depois de obter slot e trava da resposta (`.claude/settings.json:68`, `bin/hook_stop_fila1.py:1048`, `bin/codex_pergunta.sh:162`, `bin/codex_pergunta.sh:165`). Hoje o Agent alcançado por B3 é somente `subagent_type=verificador` cujo prompt passa `_eh_final`; construtor e verificador de desenho não são alcançados por esse ramo (`bin/disjuntor.py:1451`, `bin/disjuntor.py:1454`, `bin/disjuntor.py:499`). Portanto reutilizar o ramo sem ampliá-lo deixa passar os Agents que o contrato novo pretende bloquear (`bin/disjuntor.py:1454`).

3. **Evento / tempo.** `_topo_consistente` já lê o RELATO inteiro duas vezes e verifica bytes/stat, mas devolve somente as duas primeiras linhas úteis recortadas por `_bloco_do_topo`; serve à liberação atual, não à varredura de todos os eventos vivos (`bin/disjuntor.py:662`, `bin/disjuntor.py:679`, `bin/disjuntor.py:686`, `bin/hook_stop_fila1.py:415`, `bin/hook_stop_fila1.py:439`). `_sha_b3` fornece SHA-256 truncado a 16 hex após `strip().lstrip('*_`> ')`; é normalização específica do topo, não definição pronta para identidade imutável de evento (`bin/disjuntor.py:571`). A janela de 24h atual é apenas painel/trilha, baseada no timestamp da trilha, não `visto_em` de desenho (`bin/disjuntor.py:1548`, `bin/disjuntor.py:1563`). Contraexemplo hipotético: dois eventos válidos na terceira e quarta linhas do RELATO seriam invisíveis se o instrumento usar diretamente a saída de `_topo_consistente` (`bin/hook_stop_fila1.py:439`). Contraexemplo hipotético de identidade: `DESENHO OBRA-X: ...` e `> DESENHO OBRA-X: ...` têm o mesmo hash nessa função; se `>` deveria ser semanticamente diferente, a normalização precisa ser especificada (`bin/disjuntor.py:573`).

4. **Disparo / liberação.** Existem a trava única, gravação por temporário+`os.replace`, anúncio após gravação e reversão da liberação se não gravou; são infraestrutura reutilizável (`bin/disjuntor.py:237`, `bin/disjuntor.py:271`, `bin/disjuntor.py:274`, `bin/disjuntor.py:948`, `bin/disjuntor.py:958`). O reconhecedor existente só aceita `DESENHO <obra>: lei=... pergunta=... <arquivo>:<linha>` ou `ERRO-BOBO`; não valida classe/padrão/armadilha/obra nem consulta GLOSSÁRIO/PLANO (`bin/disjuntor.py:626`, `bin/disjuntor.py:639`, `bin/disjuntor.py:650`). O consumo atual é por **obra**: `b3[obra].consumidas`; não é consumo global do SHA de um episódio por entidade (`bin/disjuntor.py:909`, `bin/disjuntor.py:932`, `bin/disjuntor.py:939`). Contraexemplo hipotético: se uma mesma linha de liberação nomear várias obras, o consumidor atual pode consumi-la uma vez em cada `b3[obra]`, porque o histórico consumido pertence a cada obra (`bin/disjuntor.py:637`, `bin/disjuntor.py:939`).

5. **Falha/NAO_SEI.** O contrato está desatualizado ao afirmar que a exceção do envelope hoje libera todo ramo: o B3 já fecha a exceção com negativa por `_ramo_b3`; outros ramos ainda falham abertos (`bin/disjuntor.py:1496`, `bin/disjuntor.py:1498`, `bin/disjuntor.py:1506`). O comportamento B3 atual excede o escopo proposto: `julga_b3` retorna `{}`+motivo em qualquer exceção e `estado_b3` devolve `NAO_SEI` para **qualquer obra consultada** se há motivo; não restringe a obras do último estado legível (`bin/disjuntor.py:977`, `bin/disjuntor.py:982`, `bin/disjuntor.py:989`, `bin/disjuntor.py:992`). `ler_estado` não mantém cópia anterior recuperável e `_com_estado` preserva o JSON ilegível, levantando erro (`bin/disjuntor.py:259`, `bin/disjuntor.py:265`, `bin/disjuntor.py:291`). Contraexemplo hipotético: último estado legível disparou apenas `OBRA-X`; JSON vira `{lixo`; consulta verificador de `OBRA-Y` também recebe NAO_SEI/recusa pelo B3 atual (`bin/disjuntor.py:982`, `bin/disjuntor.py:993`, `bin/disjuntor.py:1046`). Para implementar o limite novo falta uma fonte durável recuperável de `obras_em_entidades_disparadas`; seu escritor/leitor ainda não existe nas funções de estado examinadas (`bin/disjuntor.py:237`, `bin/disjuntor.py:291`).

6. **Medida.** Há `dispara` para trilha padronizada, `ler` para agregações 24h e `linha` para painel; ESTADO importa ambos diretamente (`bin/disjuntor.py:139`, `bin/disjuntor.py:1546`, `bin/disjuntor.py:1677`, `bin/gerar_estado.py:280`). O painel enumera IDs fixos B1/B2/B3/B4/B4-CODEX/ANEXO-ENXUTO; adicionar só um novo evento à trilha não o faz aparecer nessa enumeração, e não há desenhos por entidade ou eventos None (`bin/disjuntor.py:1679`, `bin/disjuntor.py:1615`). Escritores atuais de métricas: `trilha`/`dispara` e anúncio da decisão; leitores: `ler`/`linha` e gerador de ESTADO (`bin/disjuntor.py:128`, `bin/disjuntor.py:139`, `bin/disjuntor.py:948`, `bin/disjuntor.py:1554`, `bin/gerar_estado.py:281`).

7. **Desliga — falha concreta na porta atual.** `desligado` já usa `lexists` para a chave global; PreToolUse e Stop respeitam-na (`bin/disjuntor.py:144`, `bin/disjuntor.py:1487`, `bin/hook_stop_fila1.py:1039`). Porém o wrapper sempre consulta B3 para verificador, e o caminho CLI → `b3_parada` → `estado_b3` → `decide_b3` não consulta a chave (`bin/codex_pergunta.sh:164`, `bin/disjuntor.py:1709`, `bin/disjuntor.py:1042`, `bin/disjuntor.py:989`, `bin/disjuntor.py:877`). Contraexemplo hipotético: chave existe, `b3["OBRA-X"]={"reprovados":["a1"],"disparado":true}`, fontes legíveis; o hook fica desligado, mas wrapper recusa OBRA-X com rc3 (`bin/disjuntor.py:1487`, `bin/disjuntor.py:1043`, `bin/codex_pergunta.sh:165`). A proteção da chave é textual e muito ampla: até Bash meramente contendo `DISJUNTOR_DESLIG` é negado; não autentica autoria Ronald por credencial (`bin/disjuntor.py:160`). Não prometer “só Ronald” como propriedade de segurança forte (`bin/disjuntor.py:152`).

8. **Mapa de estado existente e proposta explicitamente ausente.**
   - Estado físico: leitura/escrita exclusivamente pela infraestrutura `_com_estado` e leitura independente `ler_estado`; trava `.lock`, temporário e substituição atômica (`bin/disjuntor.py:247`, `bin/disjuntor.py:249`, `bin/disjuntor.py:260`, `bin/disjuntor.py:271`, `bin/disjuntor.py:291`).
   - `b3_limiar`: `_migra_limiar` lê e escreve (`bin/disjuntor.py:699`, `bin/disjuntor.py:710`).
   - `b3_sem_obra`: `decide_b3` acumula; `_b3_sem_obra` lê para painel (`bin/disjuntor.py:892`, `bin/disjuntor.py:895`, `bin/disjuntor.py:1624`).
   - `b3[obra].reprovados`: `decide_b3` acumula, mede e zera ao liberar; rearme também zera (`bin/disjuntor.py:900`, `bin/disjuntor.py:910`, `bin/disjuntor.py:938`, `bin/disjuntor.py:1020`).
   - `codex`: decisão recalcula da triagem, mede e zera; `_triagens` lê os IDs existentes para impedir redução da fonte (`bin/disjuntor.py:832`, `bin/disjuntor.py:907`, `bin/disjuntor.py:911`, `bin/disjuntor.py:938`).
   - `disparado`: decisão e rearme escrevem; decisão lê (`bin/disjuntor.py:913`, `bin/disjuntor.py:922`, `bin/disjuntor.py:938`, `bin/disjuntor.py:1020`).
   - `disparado_em`, `topo_no_disparo`: `_fotografa` escreve; decisão lê (`bin/disjuntor.py:727`, `bin/disjuntor.py:730`, `bin/disjuntor.py:927`, `bin/disjuntor.py:934`).
   - `consumidas`: decisão lê e acrescenta SHA por obra (`bin/disjuntor.py:932`, `bin/disjuntor.py:939`).
   - `liberado`, `rearmado`: decisão/liberação, migração e rearme escrevem; acumulação e recálculo leem para excluir IDs antigos (`bin/disjuntor.py:708`, `bin/disjuntor.py:898`, `bin/disjuntor.py:905`, `bin/disjuntor.py:936`, `bin/disjuntor.py:1018`).
   - `b3_triagem_linhas`: escritor de triagem incrementa, `_triagens` lê e atualiza (`bin/disjuntor.py:815`, `bin/disjuntor.py:829`, `bin/disjuntor.py:853`).
   - Campos **novos propostos**, ainda sem escritor/leitor: registro por SHA com `visto_em/obra/entidades/nomeado_em_episodio`; episódios por entidade com obras alcançadas; consumo global de SHA de nomeação; contadores por entidade/None; cópia recuperável de último escopo legível. Nenhuma dessas estruturas pertence ao schema B3 manipulado na decisão atual; funções futuras/nomes exatos: [não verificado] (`bin/disjuntor.py:881`, `bin/disjuntor.py:891`, `bin/disjuntor.py:936`).

9. **MORDE.** A suíte atual declara casos B3/Codex e mutantes de reconhecimento, atomicidade, NAO_SEI e porta, mas sua lista de execução não inclui casos PADRAO-DE-CIMA por entidade (`bin/tests/test_disjuntor.sh:1468`, `bin/tests/test_disjuntor.sh:1626`, `bin/tests/test_disjuntor.sh:1757`). Os testes criam diretórios/arquivos e mutantes: não executá-los sob a exigência de somente leitura (`bin/tests/test_disjuntor.sh:39`, `bin/tests/test_disjuntor.sh:1478`, `bin/tests/test_disjuntor.sh:1487`). Resultado de teste e mutante global do instrumento novo: [não verificado]. Os casos necessários do contrato devem fixar estados explícitos: entidades E1/E2, mesmo programa P1, SHA distintos, dois `visto_em` dentro de 86400s, pelo menos um `nomeado=false`; com E1/E2 não dispara, com E1/E1 dispara e mutação que remove chave entidade deve tornar primeiro caso vermelho. Implementação desses testes hoje: [não verificado].

10. **Observação adicional para consolidação.** O parser já extrai `CODEX_OBRA` da atribuição imediata em `_codex_cmds`, contrariando o anexo; o docstring da função ficou desatualizado ao listar retorno sem `obra` (`bin/disjuntor.py:1325`, `bin/disjuntor.py:1326`, `bin/disjuntor.py:1337`). Não há AGENTS.md na árvore auditada pelo comando de localização; aplicabilidade de instruções externas: [não verificado].

## 31/10 16:38 · sessao 657183ad6699 · - · - · origem=Ronald (tmux)

1. **Entidade: material reutilizável existe, resolvedor do contrato ainda precisa ser construído.** `varrer_arvore()` entrega `{arquivo, linha, entidade, op, expr}`; caminhos são relativos a `app/`. Não entrega nome da função, intervalo da função, `end_lineno` ou atributo `DIRETA`; `_anotar()` guarda apenas linha, entidade, operação e expressão. A resolução de “arquivo:linha → entidade direta naquela linha/função” exige adaptador e, para referência dentro de função fora da linha de escrita, outro mapeamento AST. (`app/core/censo_escritas.py:361`, `app/core/censo_escritas.py:469`, `app/core/censo_escritas.py:489`)

2. **Separar atribuição direta de inferência é obrigatório.** Gerente explícito `Modelo.objects...` gera entidade diretamente; amarração a modelo e `self` também podem gerar expressão sem marcador. Inferências aparecem em `expr` como `[campo]`, `[relacionado]`, `[update_fields]`, `[nome]`; não existe flag estruturada de proveniência. Tratar todo `entidade != None` como DIRETA viola esse corte do contrato. (`app/core/censo_escritas.py:377`, `app/core/censo_escritas.py:406`, `app/core/censo_escritas.py:413`, `app/core/censo_escritas.py:420`, `app/core/censo_escritas.py:424`)

3. **Contraexemplo sintético de agrupamento direto correto.** Com `visto_em=agora−60/−30`, ambos ainda não nomeados:
   ```
   DESENHO OBRA-A: pergunta=x app/ponto/registro_batida.py:152
   DESENHO OBRA-B: pergunta=y app/ponto/registro_batida.py:157
   ```
   As duas escritas são `Batida.objects.create(...)`; a entidade correta é Batida para ambas e o contrato exige disparo. Trocar a segunda referência por `app/ponto/portas/celula.py:247` dá CelulaDia e exige ausência de disparo; um programa comum hipotético não muda isso. As escritas e a atribuição direta estão em `app/ponto/registro_batida.py:152`, `app/ponto/registro_batida.py:157`, `app/ponto/portas/celula.py:247`, `app/core/censo_escritas.py:377`. Estado e linhas de RELATO acima são sintéticos, **[não verificado]** em produção.

4. **Contraexemplo sintético para DIRETA.** `DESENHO OBRA-A: pergunta=x app/ponto/registro_batida.py:254` referencia `batida.save(update_fields=list(campos))`; o censo a atribui por nome da variável (`[nome]`), pois `update_fields` não é literal. Usar essa entidade como DIRETA pode agrupar indevidamente com outro evento Batida e impedir o fallback para programa; o filtro correto precisa descartar essa proveniência. (`app/ponto/registro_batida.py:254`, `app/core/censo_escritas.py:433`, `app/core/censo_escritas.py:424`) Ocorrência em RELATO/produção **[não verificado]**.

5. **`modelos()` satisfaz literalmente o vocabulário de classes do contrato, com uma limitação já demonstrada pelos testes.** Aceita classe de topo com alguma base em `*/models.py` ou `*/models/*.py`, excluindo nomes terminados em `QuerySet`/`Manager`; não verifica registro Django. Assim inclui `ResolvedoraCruzada(ValueError)`. Duas linhas sintéticas que a citem podem disparar uma entidade que é exceção, não modelo Django; isso decorre do próprio critério literal `modelos()` escolhido no contrato. (`app/core/censo_escritas.py:89`, `app/core/censo_escritas.py:95`, `app/core/censo_escritas.py:108`, `app/chamados/models.py:2179`, `app/core/tests/test_censo_escritas.py:264`, `app/core/tests/test_censo_escritas.py:289`)

6. **Fallback obra → PROGRAMA carece de resolvedor e gramática única.** O BACKLOG contém relações em prosa diferentes: “complemento”, “linha da E4 do PROGRAMA…” e itens com nome do programa. “CALENDARIO-HOJE-MINIMO” diz apenas “1o do programa”; extrair associação só por busca de token `PROGRAMA-...` perde essa ligação. A obra PADRAO-DE-CIMA está registrada como construção futura sobre B3. (`app/docs/BACKLOG.md:771`, `app/docs/BACKLOG.md:772`, `app/docs/BACKLOG.md:773`, `app/docs/BACKLOG.md:774`, `app/docs/BACKLOG.md:787`) Associação em estado de produção **[não verificado]**.

7. **Há validador de vocabulário aproveitável, mas dentro de selo shell, sem API runtime do instrumento.** `vocabulario()` está no heredoc Python de `bin/tests/test_lei_akita.sh`; lê as duas primeiras colunas do GLOSSARIO, negritos do NORTE e nomes em itálico dos ADRs do PLANO. Normaliza caixa, espaços e Markdown, compara termos inteiros, separa alternativas e alguns nomes em parênteses. Sua leitura usa `git show` do commit julgado, não o arquivo vivo; reutilizá-lo sem decidir essa diferença pode rejeitar padrão já editado no arquivo e ainda não commitado. (`bin/tests/test_lei_akita.sh:65`, `bin/tests/test_lei_akita.sh:74`, `bin/tests/test_lei_akita.sh:75`, `bin/tests/test_lei_akita.sh:80`, `bin/tests/test_lei_akita.sh:89`, `bin/tests/test_lei_akita.sh:102`, `bin/tests/test_lei_akita.sh:112`, `bin/tests/test_lei_akita.sh:138`)

8. **“Nome clássico declarado” requer definição adicional no instrumento.** O validador existente só aceita `VOCAB` ou exceções `n/a`; não tem regra independente para declaração de nome clássico. O texto documental atual também restringe padrão novo a corte do Ronald; o contrato anexo introduz uma alternativa cuja forma de declaração não foi fornecida **[não verificado]**. (`bin/tests/test_lei_akita.sh:120`, `bin/tests/test_lei_akita.sh:124`, `bin/tests/test_lei_akita.sh:138`, `app/docs/PLANO-PADROES.md:46`, `app/docs/PLANO-PADROES.md:53`)

9. **MORDE do censo não prova MORDE do novo instrumento.** Os testes existentes verificam distinção entre escrita direta, inferência e entidade desconhecida; são úteis como suporte do resolvedor. O teste “duas entidades distintas no mesmo programa não dispara” ainda pertence ao novo instrumento, cujo registro está como obra futura. (`app/core/tests/test_censo_escritas.py:25`, `app/core/tests/test_censo_escritas.py:80`, `app/core/tests/test_censo_escritas.py:92`, `app/core/tests/test_censo_escritas.py:110`, `app/docs/BACKLOG.md:787`) Nenhum teste foi executado porque os selos existentes podem criar arquivos; fiz somente leitura e uma consulta Python `-B` em memória **[não verificado]** quanto a execução dos selos.

_1164 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
