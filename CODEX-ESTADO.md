# CODEX-ESTADO

> Ultima resposta processada: **17/11/2026 14:52** · publicado em **17/11/2026 14:52** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 17/11 14:52 · sessao e11c1d0b2faa · CORTE-FOLHA-ESPELHO · leitura · origem=Code

**Parada no item (i): não encontrei o predicado composto do corte na cópia examinada. Construí-lo seria juiz novo**, mesmo aproveitando autoridades existentes. O registro mantém separados C6, C19 e K2. (`app/core/juizes.py:291`; `app/core/juizes.py:304`; `app/core/juizes.py:330`)

A evidência que impede anunciar uma função existente como resposta completa:

- **K2 — `classificar_export`:** o sítio atual da retenção consulta `motivos_retencao_espelho` e exclui por `furo_espelho`; essa casca delega ao retentor da célula. Não compõe pergunta viva com ação humana do dia. (`app/folha/export.py:968`; `app/folha/export.py:1301`; `app/folha/export.py:1307`)
- **Retentor da célula:** mantém retenção por célula ausente, veredito acusador e falta de veredito; aplica a isenção por `nao_retem_pela_pausa`. Esse laço não consulta C6 nem a tarefa. (`app/folha/export.py:1231`; `app/folha/export.py:1270`; `app/folha/export.py:1293`; `app/folha/export.py:1295`)
- **C19 — `tarefa_do_dia`:** publica ações, mas não exige pergunta viva para toda ação administrativa: há ramo que oferece validação justamente sem pergunta viva. Portanto, ação publicada não demonstra sozinha a composição exigida. (`app/chamados/services/tarefa_do_dia.py:357`; `app/chamados/services/tarefa_do_dia.py:363`; `app/chamados/services/tarefa_do_dia.py:733`)
- **`fora_da_folha`:** primeiro seleciona os já excluídos por `furo_espelho`; depois carrega a tarefa para os dias retidos. Essa composição explica a exclusão existente, mas não decide o novo acréscimo de retenção. (`app/folha/fora_da_folha.py:156`; `app/folha/fora_da_folha.py:165`; `app/folha/fora_da_folha.py:200`; `app/folha/fora_da_folha.py:217`)

**Não há, portanto, função existente demonstrada onde encaixar o ramo completo sob a condição “sem juiz novo”.** A inexistência absoluta fora dos caminhos examinados permanece [nao verificado].

Encerro aqui conforme a condição expressa do item (i); mapa fechado, matriz final, REDs e ataques cruzados não foram concluídos. [nao verificado]

perguntas de lei: 0

## 17/11 14:52 · sessao 4c77d83733de · - · - · origem=Ronald (tmux)

Complemento F9 — sem executar comandos de negócio.

A contagem anterior precisa deste ajuste: **56 referências sintáticas RVC em 48 linhas distintas**, pelo critério AST informado. Isso não constitui lista fechada global: aliases como `_q.date()` derivam de C e escapam à atribuição nominal (`app/chamados/management/commands/auditar_invariantes_chamados.py:470`, `app/chamados/management/commands/auditar_invariantes_chamados.py:478`). Chaves de snapshots também ficaram fora do critério (`app/chamados/management/commands/reabrir_orfas_enterradas.py:160`).

As 12 pontes CLI podem ser publicadas com o limite explícito “despacho de framework [nao verificado]”: [nao verificado]

```text
manage app/manage.py:18 -> auditor app/chamados/management/commands/auditar_invariantes_chamados.py:553 [nao verificado: dispatch]
manage app/manage.py:18 -> backfill app/chamados/management/commands/backfill_decisoes_pergunta.py:452 [nao verificado: dispatch]
manage app/manage.py:18 -> reabrir app/chamados/management/commands/reabrir_orfas_enterradas.py:53 [nao verificado: dispatch]
manage app/manage.py:18 -> limbo app/chamados/management/commands/curar_perguntas_em_limbo.py:43 [nao verificado: dispatch]
manage app/manage.py:18 -> auditor_disputas app/chamados/management/commands/auditar_disputas.py:62 [nao verificado: dispatch]
manage app/manage.py:18 -> encerrar app/chamados/management/commands/encerrar_perguntas_com_fato.py:27 [nao verificado: dispatch]
manage app/manage.py:18 -> dedup app/chamados/management/commands/dedup_perguntas_fato.py:48 [nao verificado: dispatch]
manage app/manage.py:18 -> migrar app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:195 [nao verificado: dispatch]
manage app/manage.py:18 -> validar app/chamados/management/commands/validar_classe_a.py:38 [nao verificado: dispatch]
manage app/manage.py:18 -> relavrar app/chamados/management/commands/relavrar_via_i6.py:82 [nao verificado: dispatch]
manage app/manage.py:18 -> fantasmas app/ponto/management/commands/reconciliar_fantasmas.py:152 [nao verificado: dispatch]
manage app/manage.py:18 -> grade app/ponto/management/commands/reconciliar_grade.py:74 [nao verificado: dispatch]
```

Essas são arestas de alcance por despacho, não chamadas literais presentes em `manage.py`; a chamada literal local é para `execute_from_command_line` (`app/manage.py:11`, `app/manage.py:18`).

Pontes adicionais de cron, verificadas:

```text
install bin/crons.sh:24 -> crons.handle app/core/management/commands/crons.py:36 [nao verificado: dispatch]
check bin/crons.sh:27 -> crons.handle app/core/management/commands/crons.py:36 [nao verificado: dispatch]
crons.handle app/core/management/commands/crons.py:50 -> render app/config/crons.py:1966
crons.handle app/core/management/commands/crons.py:57 -> render_mantendo_sombra_do_host app/core/management/commands/crons.py:71
render_mantendo_sombra_do_host app/core/management/commands/crons.py:94 -> render app/config/crons.py:1966
render_mantendo_sombra_do_host app/core/management/commands/crons.py:113 -> render app/config/crons.py:1966
render app/config/crons.py:1967 -> linhas app/config/crons.py:1886
linhas app/config/crons.py:1903 -> _run app/config/crons.py:1511
_run app/config/crons.py:1514 -> runner bin/cron_run.sh:11
runner bin/cron_run.sh:11 -> manage app/manage.py:18 [nao verificado: container/dispatch]
```

O comando transportado pelo runner é construído de `MANAGE`, nome do comando, schema e argumentos (`app/config/crons.py:21`, `app/config/crons.py:1899`); os três nomes relevantes vêm das entradas `reconciliar_fantasmas`, `reconciliar_grade` e auditor (`app/config/crons.py:392`, `app/config/crons.py:462`, `app/config/crons.py:542`). O install real chega a `crontab -` (`bin/crons.sh:40`); sua execução nesta instalação é [nao verificado].

Para ligar as funções aos sítios terminais, além das 21 arestas anteriores:

| Função que contém a leitura | Sítios terminais |
|---|---|
| `_contraprova_i21a`, auditor `:166` | auditor `:177` |
| `_contraprova_f1t`, auditor `:184` | auditor `:205` |
| `universo_i6`, auditor `:455` | auditor `:470`; uso do alias em `:478` |
| `_i6_pares`, auditor `:499` | auditor `:538` |
| `handle`, auditor `:553` | auditor `:588,:592,:905,:955,:1075,:1076,:1105,:1106,:1156,:1576,:1577,:1604,:1605,:1657,:1768,:1769,:1780` |
| `linhas_a_criar`, backfill `:44` | backfill `:56,:60,:61` |
| `admin_decidiu_sem_ato`, backfill `:94` | backfill `:101,:102` |
| `aplicar`, backfill `:163` | backfill `:325,:326` |
| `handle`, backfill `:452` | backfill `:504` |
| `handle`, reabrir `:53` | reabrir `:67,:68,:88,:91,:129,:130,:142,:143` |
| `nota_da_resposta`, limbo `:26` | limbo `:32` |
| `handle`, limbo `:43` | limbo `:46,:69` |
| `handle`, auditor_disputas `:62` | auditor_disputas `:71,:150` |
| `handle`, encerrar `:27` | encerrar `:32` |
| `handle`, dedup `:48` | dedup `:75` |
| `classificar`, migrar `:129` | migrar `:147` |
| `handle`, validar `:38` | validar `:80` |
| `handle`, fantasmas `:152` | fantasmas `:213` |
| `_relavrar_orfa`, grade `:430` | grade `:461` |

Todos os nomes da tabela são os mesmos caminhos expandidos na resposta anterior; o endpoint é a linha de leitura e não uma nova chamada de função.

As buscas estáticas de imports dos 12 módulos e `call_command` com nome literal encontraram somente os dois importadores de negócio já mapeados: dedup→backfill (`app/chamados/management/commands/dedup_perguntas_fato.py:104`, chamada `:105`) e relavrar→auditor (`app/chamados/management/commands/relavrar_via_i6.py:46`, chamada `:49`), além do worklist já excluído por não alcançar o sítio C (`app/ponto/worklist.py:37`, `app/ponto/worklist.py:55`). Ausência de import/call dinâmico é [nao verificado].

A continuação do escritor de decisões é:

```text
aplicar app/chamados/management/commands/backfill_decisoes_pergunta.py:324 -> registrar app/chamados/models.py:4110
registrar app/chamados/models.py:4121 -> acordar_celulas app/chamados/models.py:3341
acordar_celulas app/chamados/models.py:3358 -> emitir_evento_celula [nao verificado: linha da definição não aberta]
```

Essa continuação é emissão síncrona de outbox, não `on_commit` nessa ponte (`app/chamados/models.py:3342`, `app/chamados/models.py:3358`). Callbacks posteriores da porta de célula não foram fechados [nao verificado].

Correção da referência à validação: `validar_classe_a.py:100` chama `app/chamados/services/validacao.py:574`, conferido nesta cópia. Descartar a referência anterior `:403`, que vinha marcada como não verificada.

## 17/11 14:51 · sessao 8f6299d69f7a · - · - · origem=Ronald (tmux)

Complemento F3: a fábrica tem **7 chamadas diretas** de produção encontradas por busca dos nomes e imports; os sete caminhos chegam ao construtor em `app/chamados/services/tarefa_do_dia.py:730` e, com IDs não vazios, a `marcos_do_lote` em `:434`.

```text
beco_em_lote app/escala/services/cadastro_realidade.py:466 -> carga_da_tarefa app/chamados/services/tarefa_do_dia.py:728
comparar app/chamados/management/commands/diff_e3_sombra.py:134 -> carga_da_tarefa app/chamados/services/tarefa_do_dia.py:728
censo app/chamados/services/vigia_fio.py:357 -> carga_da_tarefa app/chamados/services/tarefa_do_dia.py:728
censo_alcance app/chamados/services/vigia_fio.py:592 -> carga_da_tarefa app/chamados/services/tarefa_do_dia.py:728
medir app/chamados/services/furo_sem_canal.py:94 -> carga_da_tarefa app/chamados/services/tarefa_do_dia.py:728
_carga app/folha/fora_da_folha.py:67 -> carga_da_tarefa app/chamados/services/tarefa_do_dia.py:728
tarefa_do_dia app/chamados/services/tarefa_do_dia.py:740 -> carga_da_tarefa app/chamados/services/tarefa_do_dia.py:728
carga_da_tarefa app/chamados/services/tarefa_do_dia.py:730 -> _carga_da_tarefa_cls app/chamados/services/tarefa_do_dia.py:368
carga_da_tarefa app/chamados/services/tarefa_do_dia.py:730 -> CargaDaTarefa.__init__ app/chamados/services/tarefa_do_dia.py:392
ato_do_dia app/chamados/management/commands/backfill_fio_morto_e4b.py:87 -> tarefa_do_dia app/chamados/services/tarefa_do_dia.py:733
comparar app/chamados/management/commands/diff_e3_sombra.py:145 -> tarefa_do_dia app/chamados/services/tarefa_do_dia.py:733
fora_da_folha app/folha/fora_da_folha.py:205 -> _carga app/folha/fora_da_folha.py:61
o_que_segura app/folha/fora_da_folha.py:276 -> _carga app/folha/fora_da_folha.py:61
dias_sem_fio app/ponto/services/pautas_dos_discordantes.py:85 -> _carga app/folha/fora_da_folha.py:61
montar_linhas app/folha/retrato_folha.py:1214 -> fora_da_folha app/folha/fora_da_folha.py:177
handle app/folha/management/commands/lavrar_retrato_folha.py:65 -> montar_linhas app/folha/retrato_folha.py:1174
encadeado app/config/crons.py:1655 -> handle app/folha/management/commands/lavrar_retrato_folha.py:43
handle app/ponto/management/commands/pautas_discordantes.py:72 -> dias_sem_fio app/ponto/services/pautas_dos_discordantes.py:46
dias_folga_nao_definida_sem_porta app/escala/services/cadastro_realidade.py:411 -> beco_em_lote app/escala/services/cadastro_realidade.py:414
_folga_nao_definida_sem_porta app/escala/services/cadastro_realidade.py:559 -> beco_em_lote app/escala/services/cadastro_realidade.py:414
lista app/escala/services/cadastro_realidade.py:850 -> _folga_nao_definida_sem_porta app/escala/services/cadastro_realidade.py:530
handle app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:105 -> dias_folga_nao_definida_sem_porta app/escala/services/cadastro_realidade.py:405
handle app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:54 -> _folga_nao_definida_sem_porta app/escala/services/cadastro_realidade.py:530
handle app/chamados/management/commands/backfill_fio_morto_e4b.py:255 -> ato_do_dia app/chamados/management/commands/backfill_fio_morto_e4b.py:82
handle app/chamados/management/commands/backfill_fio_morto_e4b.py:309 -> ato_do_dia app/chamados/management/commands/backfill_fio_morto_e4b.py:82
```

São **25 arestas adicionais**, sem repetir `CargaDaTarefa.__init__:434 -> marcos_do_lote:151`, que já estava no mapa principal. A segunda chamada de `comparar` usa carga pronta (`diff_e3_sombra.py:145`), portanto não abre segundo construtor; `tarefa_do_dia` só constrói quando `carga is None` (`tarefa_do_dia.py:739-740`).

Entradas ainda anteriores ao ramo `lista`, para eventual expansão:

```text
cadastro_realidade app/escala/views.py:487 -> lista app/escala/services/cadastro_realidade.py:773
handle app/escala/management/commands/fechar_avisos_regua_na_lista.py:28 -> lista app/escala/services/cadastro_realidade.py:773
handle app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:36 -> lista app/escala/services/cadastro_realidade.py:773
ListaDoDia.dados app/chamados/services/vigia_fio.py:194 -> lista app/escala/services/cadastro_realidade.py:773
```

O nome exato `cadastro_realidade` da view e `ListaDoDia.dados` do método precisam reconferência [nao verificado]; as linhas de chamada e o destino foram lidos.

Mais dois conectores completos:

```text
handle app/chamados/management/commands/lavrar_furo_sem_canal.py:31 -> medir app/chamados/services/furo_sem_canal.py:44
cron app/config/crons.py:531 -> chamada medir app/chamados/management/commands/lavrar_furo_sem_canal.py:31
```

O cron do retrato passa por um escritor de `RetratoFolha`, com `montar_linhas` usando a fonte da tarefa (`app/folha/management/commands/lavrar_retrato_folha.py:65-68`; `app/folha/retrato_folha.py:1214`); assim, “nenhuma escrita nova na F3” não demonstra ausência de efeito em outro escritor. O caminho omitido alcança campos do retrato, embora alteração concreta de valores em produção permaneça [nao verificado].

Nenhum signal ou callback **direto** de `carga_da_tarefa`/`_carga_da_tarefa_cls` apareceu na busca dos nomes/imports em produção; ausência de fecho transitivo por todos os importadores de `lista` permanece [nao verificado].

## 17/11 14:50 · sessao 597490bfcbf2 · - · - · origem=Ronald (tmux)

Complemento F4; mesmas abreviações D/M do primeiro envio.

Os 20 alvos do mapa anterior são exatamente:

```text
responder_pergunta_disputa_core D:1439
reabrir_para_correcao D:1576
responder_automaticamente D:1654
carimbar_resposta_pela_trilha D:1683
recobrar_questionario D:1712
reabrir_credor_de_pergunta_viva D:1827
_resolver_perguntas_por_chamado_resolvido D:1906
_tipo_pelo_marco M:512
_ts_tipo_do_plantio M:599
estado_aplicacao_pergunta M:852
_declarar_ausencia_de_pergunta M:944
_materializar_ausencia_de_resposta M:978
perguntas_absorvidas_por M:1085
devolver_absorvidas_da_ausencia_rejeitada M:1121
_materializar_resposta_hora M:1266
_materializar_resposta_data_hora M:1541
_materializar_resposta_escolha M:1626
_materializar_uma M:1809
materializar_perguntas_validadas_da_disputa M:2032
materializar_pendentes_aprovadas M:2098
```

Esse é um núcleo de leitura/mutação acompanhado; não equivale a todos os sítios de campo da fatia. Os sítios novos do par (`par_do_intervalo`, `_estado_pelo_derivador`, `_gravar_irma_do_par`) também precisam entrar na revisão da cura porque chamam/consomem esse núcleo em M:214, M:222, M:895, M:1695, M:1707.

Mais 26 arcos contáveis, para somar aos 80 anteriormente apresentados:

```text
include_chamados app/config/urls.py:28 -> urlpatterns app/chamados/urls.py:8
include_api app/config/urls.py:37 -> urlpatterns app/api/urls.py:16
include_api_core app/config/urls_core.py:6 -> urlpatterns app/api/urls_core.py:5
rota_local app/chamados/urls.py:14 -> detalhe_chamado_local app/chamados/views.py:359
rota_meu_atendimento app/chamados/urls.py:16 -> meu_atendimento app/chamados/views.py:559
rota_responder_disputa app/chamados/urls.py:17 -> responder_pergunta_disputa app/chamados/views.py:1228
rota_solicitar_correcao app/chamados/urls.py:19 -> solicitar_correcao_disputa app/chamados/views.py:1317
rota_reabrir_supervisao app/chamados/urls.py:20 -> reabrir_disputa_supervisao app/chamados/views.py:1372
rota_fio app/chamados/urls.py:21 -> fio_colaborador app/chamados/views.py:659
rota_modal app/chamados/urls.py:39 -> modal_fio_colaborador app/chamados/views.py:869
rota_responder_colaborador app/chamados/urls.py:42 -> responder_chamado_colaborador app/chamados/views.py:1044
rota_foto app/api/urls.py:59 -> api_upload_foto app/api/views.py:839
rota_responder_api app/api/urls.py:93 -> api_disputa_responder app/api/views.py:2584
rota_correcao_api app/api/urls.py:94 -> api_solicitar_correcao_disputa app/api/views.py:2670
rota_foto_core app/api/urls_core.py:8 -> api_upload_foto app/api/views_core.py:823
handle_ensaio app/chamados/management/commands/materializar_passivo_validado.py:104 -> reparar app/chamados/services/passivo_validado.py:289
handle_apply_dry app/chamados/management/commands/materializar_passivo_validado.py:113 -> reparar app/chamados/services/passivo_validado.py:289
reparar app/chamados/services/passivo_validado.py:380 -> _trabalhar_o_par app/chamados/services/passivo_validado.py:431
_trabalhar_o_par app/chamados/services/passivo_validado.py:443 -> _plantar_o_dia app/chamados/services/passivo_validado.py:489
post_save_Chamado app/chamados/signals.py:91 -> resolve_perguntas_ao_resolver app/chamados/signals.py:92
post_save_Disputa app/chamados/signals.py:149 -> materializa_perguntas_ao_fechar_disputa app/chamados/signals.py:150
post_save_Ausencia app/ponto/signals.py:275 -> _consumidor_post_save_ausencia app/ponto/signals.py:276
post_delete_Ausencia app/ponto/signals.py:282 -> _consumidor_post_delete_ausencia app/ponto/signals.py:283
_consumidor_post_save_ausencia app/ponto/signals.py:279 -> _consumidor_ausencia app/ponto/signals.py:250
_consumidor_post_delete_ausencia app/ponto/signals.py:284 -> _consumidor_ausencia app/ponto/signals.py:250
on_commit app/ponto/signals.py:272 -> _depois app/ponto/signals.py:254
```

**Correção necessária antes de publicar:** os destinos `urlpatterns app/api/urls.py:16`, `urlpatterns app/api/urls_core.py:5` e `responder_chamado_colaborador ...:1044` não foram confirmados pelo trecho aberto; marcar esses três destinos [nao verificado], ou conferir por `rg -n '^urlpatterns|^def responder_chamado_colaborador'`. As demais linhas do quadro foram abertas diretamente.

Os campos que o brief diz conservar estão presentes nestes sítios, excluídos dos 20 alvos por esse recorte:

- C/Disputa: `_derivar_chamado_destino` lê C em D:228; `motivo_fora_da_fabrica`, D:776; `criar_disputa_a_partir_de_chamados_orfaos`, D:1084/D:1104/D:1111; `criar_disputa_ancora_de_chamado`, D:1340/D:1347; `_fechar_disputa_se_quitada`, M:1989.
- A18/A06: `_carimbar_recusa` lê carimbo/via/recusa em M:1652-1656 e escreve recusa em M:1665-1666; `_balde_da_recusa` lê via em `app/chamados/services/passivo_validado.py:140-142`.
- Foto de reversão: `CAMPOS_REVERSAO`, `app/chamados/services/passivo_validado.py:64-66`; a devolução conserva foto explícita em M:1150-1154.
- `relavra_via` conserva via/carimbo: `relavrar` lê/escreve via em `app/chamados/services/relavra_via.py:118-127`; `relavrar_dia` filtra via em :154-155; `q_carimbo_desfazivel` filtra C em :181; `desfazer_carimbo_sem_lastro` fotografa C/via em :244-245 e descarimba pela porta em :246. Seu efeito indireto continua: chama C6 em :242, portanto a migração do leitor C6 pode mudar a decisão desse escritor.
- `fabrica_da_celula` conserva via/C: `motivos_a_perguntar` usa via e o Q do descarimbo em `app/chamados/services/fabrica_da_celula.py:149-153`; `perguntar` filtra C em :171 e chama escritor do descarimbo em :169; `perguntar_pela_celula` chama esse mesmo escritor em :209. Não há troca direta de leitor R/V nesses dois sítios no escopo descrito pelo brief; o comportamento transitivo depende do C6 que eles chamam (`relavra_via.py:242`).

Mais seis arcos opcionais, caso o quadro também enumere as exceções conservadas:

```text
julgar_colab app/ponto/services/cartorio.py:1045 -> perguntar_pela_celula app/chamados/services/fabrica_da_celula.py:178
perguntar_pela_celula app/chamados/services/fabrica_da_celula.py:207 -> motivos_a_perguntar app/chamados/services/fabrica_da_celula.py:92
perguntar_pela_celula app/chamados/services/fabrica_da_celula.py:219 -> perguntar app/chamados/services/fabrica_da_celula.py:160
perguntar app/chamados/services/fabrica_da_celula.py:169 -> desfazer_carimbo_sem_lastro app/chamados/services/relavra_via.py:187
perguntar_pela_celula app/chamados/services/fabrica_da_celula.py:209 -> desfazer_carimbo_sem_lastro app/chamados/services/relavra_via.py:187
handle app/chamados/management/commands/relavrar_via_i6.py:102 -> relavrar app/chamados/services/relavra_via.py:106
```

Alcance financeiro adicional verificável: o caminho `materializar_passivo_validado -> reparar -> _plantar_o_dia -> _materializar_uma` existe nas referências acima; a própria configuração classifica o comando como ato sobre competência exportada e dinheiro (`app/config/crons.py:853-858`). A unidade é o dia, com rollback geométrico, em `app/chamados/services/passivo_validado.py:489-513`. Portanto “F4 só muda leitura” não comprova “F4 não altera seleção/resultado de escrita financeira”; isso exige responder esse caminho no ataque. Resultado financeiro real da migração [nao verificado].

O comando não deve ser usado como verificação somente leitura: mesmo fora de `--apply`, `handle` cria pasta em `app/chamados/management/commands/materializar_passivo_validado.py:85` e escreve JSON em :118-119.

## 17/11 14:49 · sessao 8f6299d69f7a · - · - · origem=Ronald (tmux)

F3 — referências abaixo são da cópia, que já contém `_validar_o_par` e `devolver_paridade_bug_par_103` em `app/chamados/services/validacao.py:308,526`; os hashes e as sondas dos anexos não foram abertos [nao verificado].

Convenção do mapa: aresta = chamada estática em um sítio de chamada; caminhos compostos seguem pela concatenação das arestas. A lista abaixo cobre as entradas diretas nos leitores/guardas da F3 e os conectores encontrados. Fechamento transitivo de toda a fábrica dinâmica de `CargaDaTarefa`, de todos os importadores de `carga_da_tarefa` e de toda a infraestrutura de sinais [nao verificado]; portanto não certificar “lista fechada” global.

Mapa principal, 39 arestas:

```text
_validar_pelo_dono app/api/foto_recebida.py:51 -> validar_pergunta app/chamados/services/validacao.py:574
api_mensageria_validar app/api/views_mensageria.py:545 -> classe_do_lote app/chamados/services/validacao.py:1088
handle app/chamados/management/commands/auditar_invariantes_chamados.py:1163 -> marcos_do_lote app/chamados/veredito_lavrado.py:151
_central app/chamados/management/commands/backfill_fio_morto_e4b.py:72 -> montar_contexto_painel app/chamados/services/painel.py:198
handle app/chamados/management/commands/validar_classe_a.py:100 -> validar_pergunta app/chamados/services/validacao.py:574
acao_validar_pergunta app/chamados/services/acoes_disputa.py:19 -> validar_pergunta app/chamados/services/validacao.py:574
acao_recusar_pergunta app/chamados/services/acoes_disputa.py:82 -> recusar_pergunta app/chamados/services/validacao.py:770
acao_corrigir_resposta app/chamados/services/acoes_disputa.py:381 -> validar_pergunta app/chamados/services/validacao.py:574
aplicar_declaracao app/chamados/services/declaracao_texto.py:696 -> validar_pergunta app/chamados/services/validacao.py:574
montar_chamados_fio app/chamados/services/fio.py:282 -> marcos_do_lote app/chamados/veredito_lavrado.py:151
processar_post_fio app/chamados/services/fio.py:655 -> acao_validar_pergunta app/chamados/services/acoes_disputa.py:13
processar_post_fio app/chamados/services/fio.py:684 -> acao_recusar_pergunta app/chamados/services/acoes_disputa.py:67
processar_post_fio app/chamados/services/fio.py:685 -> acao_corrigir_resposta app/chamados/services/acoes_disputa.py:304
processar_post_fio app/chamados/services/fio.py:690 -> acao_reabrir_disputa app/chamados/services/acoes_disputa.py:92
etag_do_placar app/chamados/services/painel.py:91 -> marcador_do_placar app/chamados/services/painel.py:50
montar_contexto_painel app/chamados/services/painel.py:298 -> placar_aceite app/chamados/services/verbos.py:277
pks_stale_por_varredura app/chamados/services/raiox.py:38 -> marcos_do_lote app/chamados/veredito_lavrado.py:151
CargaDaTarefa.__init__ app/chamados/services/tarefa_do_dia.py:434 -> marcos_do_lote app/chamados/veredito_lavrado.py:151
validar_pergunta app/chamados/services/validacao.py:623 -> _e_aceite_de_folga app/chamados/services/validacao.py:378
validar_lote_classe_a app/chamados/services/validacao.py:978 -> classe_do_lote app/chamados/services/validacao.py:1088
validar_lote_classe_a app/chamados/services/validacao.py:989 -> acao_validar_pergunta app/chamados/services/acoes_disputa.py:13
fila_validar_classificada app/chamados/services/validacao.py:1165 -> classe_do_lote app/chamados/services/validacao.py:1088
dono_fora_do_fio app/chamados/services/vigia_fio.py:259 -> _e_aceite_de_folga app/chamados/services/validacao.py:378
detalhe_chamado_local app/chamados/views.py:475 -> acao_regularizar_disputa app/chamados/services/acoes_disputa.py:120
detalhe_chamado_local app/chamados/views.py:536 -> acao_corrigir_resposta app/chamados/services/acoes_disputa.py:304
painel_gestao app/chamados/views.py:727 -> montar_contexto_painel app/chamados/services/painel.py:198
placar_json app/chamados/views.py:820 -> etag_do_placar app/chamados/services/painel.py:79
placar_json app/chamados/views.py:852 -> montar_contexto_painel app/chamados/services/painel.py:198
raiox_lente app/chamados/views.py:1486 -> placar_equipe app/chamados/services/verbos.py:242
raiox_lente app/chamados/views.py:1488 -> placar_aceite app/chamados/services/verbos.py:277
validar_inline app/chamados/views_cobrar.py:208 -> acao_validar_pergunta app/chamados/services/acoes_disputa.py:13
validar_lote app/chamados/views_cobrar.py:261 -> validar_lote_classe_a app/chamados/services/validacao.py:938
proposta_lote app/chamados/views_cobrar.py:288 -> fila_validar_classificada app/chamados/services/validacao.py:1155
lote_contagem app/chamados/views_cobrar.py:349 -> fila_validar_classificada app/chamados/services/validacao.py:1155
lote_preview app/chamados/views_cobrar.py:361 -> fila_validar_classificada app/chamados/services/validacao.py:1155
validar_lote_tela app/chamados/views_cobrar.py:381 -> validar_lote_classe_a app/chamados/services/validacao.py:938
handle app/core/management/commands/lavrar_badge_navbar.py:39 -> montar_contexto_painel app/chamados/services/painel.py:198
_aplicar_lote app/core/views_ia.py:169 -> validar_lote_classe_a app/chamados/services/validacao.py:938
retrato_raiox app/folha/views_retrato.py:515 -> placar_aceite app/chamados/services/verbos.py:277
```

Conectores anteriores às entradas, mais 28 arestas verificadas:

```text
resolver_disputas_de_foto app/api/foto_recebida.py:74 -> _validar_pelo_dono app/api/foto_recebida.py:28
api_upload_foto app/api/views.py:861 -> resolver_disputas_de_foto app/api/foto_recebida.py:57
api_upload_foto app/api/views_core.py:845 -> resolver_disputas_de_foto app/api/foto_recebida.py:57
detalhe_chamado_local app/chamados/views.py:480 -> aplicar_declaracao app/chamados/services/declaracao_texto.py:641
detalhe_chamado_local app/chamados/views.py:501 -> acao_validar_pergunta app/chamados/services/acoes_disputa.py:13
detalhe_chamado_local app/chamados/views.py:518 -> acao_recusar_pergunta app/chamados/services/acoes_disputa.py:67
meu_atendimento app/chamados/views.py:569 -> montar_chamados_fio app/chamados/services/fio.py:190
fio_colaborador app/chamados/views.py:674 -> processar_post_fio app/chamados/services/fio.py:531
fio_colaborador app/chamados/views.py:677 -> montar_chamados_fio app/chamados/services/fio.py:190
modal_fio_colaborador app/chamados/views.py:954 -> processar_post_fio app/chamados/services/fio.py:531
_resp_modal_full app/chamados/views.py:889 -> montar_chamados_fio app/chamados/services/fio.py:190
modal_fio_colaborador app/chamados/views.py:956 -> _resp_modal_full app/chamados/views.py:882
contexto_parcial_fio app/chamados/services/fio.py:492 -> montar_chamados_fio app/chamados/services/fio.py:190
_resp_parcial app/chamados/services/fio.py:543 -> contexto_parcial_fio app/chamados/services/fio.py:483
_fio app/chamados/views.py:1400 -> contexto_parcial_fio app/chamados/services/fio.py:483
handle app/chamados/management/commands/lavrar_perguntas_stale.py:60 -> pks_stale_por_varredura app/chamados/services/raiox.py:10
censo app/chamados/services/vigia_fio.py:387 -> dono_fora_do_fio app/chamados/services/vigia_fio.py:240
censo_alcance app/chamados/services/vigia_fio.py:615 -> dono_fora_do_fio app/chamados/services/vigia_fio.py:240
handle app/chamados/management/commands/vigiar_fio_morto.py:180 -> censo app/chamados/services/vigia_fio.py:266
handle app/chamados/management/commands/vigiar_fio_morto.py:181 -> censo_alcance app/chamados/services/vigia_fio.py:487
_juizes app/chamados/management/commands/materializar_passivo_validado.py:206 -> censo app/chamados/services/vigia_fio.py:266
_alcance app/chamados/management/commands/backfill_fio_morto_e4b.py:78 -> censo_alcance app/chamados/services/vigia_fio.py:487
_aplicar_proposta app/core/views_ia.py:157 -> _aplicar_lote app/core/views_ia.py:160
ia_perguntar app/core/views_ia.py:874 -> _aplicar_proposta app/core/views_ia.py:142
cron app/config/crons.py:376 -> handle app/core/management/commands/lavrar_badge_navbar.py:23
cron app/config/crons.py:390 -> handle app/chamados/management/commands/lavrar_perguntas_stale.py:50
cron app/config/crons.py:542 -> handle app/chamados/management/commands/auditar_invariantes_chamados.py:552
encadeado app/config/crons.py:1742 -> handle app/chamados/management/commands/vigiar_fio_morto.py:119
```

**Atenção:** as duas últimas linhas de destino `handle:552`/`handle:119` devem ser reconferidas pelo pai antes de publicar [nao verificado]; foram inferidas do contexto, não abertas com linha exata. Para não contaminar o censo, use `config/crons.py:542 -> chamada marcos app/chamados/management/commands/auditar_invariantes_chamados.py:1163` e `config/crons.py:1742 -> chamadas censo app/chamados/management/commands/vigiar_fio_morto.py:180`.

A fábrica dinâmica tem mais entradas que o brief não enumera: `_carga_da_tarefa_cls` constrói a classe em `app/chamados/services/tarefa_do_dia.py:368,371`; `carga_da_tarefa` instancia-a em `:730`; seus chamadores incluem `app/escala/services/cadastro_realidade.py:466`, `app/chamados/management/commands/diff_e3_sombra.py:134`, `app/chamados/services/vigia_fio.py:357,592`, `app/chamados/services/furo_sem_canal.py:94`, `app/folha/fora_da_folha.py:67`, e `tarefa_do_dia` em `app/chamados/services/tarefa_do_dia.py:740`. Esse ramo impede afirmar lista fechada global só com o mapa de 67 arestas.

Ataques cruzados: 9.

1. **Unidade incompatível no marco — derivação paralela/selo sem mordida.** O brief pede `Max(pk)`/`Count` para `marcos_do_lote`, mas o contrato atual é `{colaborador_id: datetime}` (`app/chamados/veredito_lavrado.py:152`), `_acumula` escolhe máximo temporal (`:169-175`), e dois consumidores fazem `marco > julgado_em`: `esta_stale` (`:230-233`) e a varredura paralela (`app/chamados/services/raiox.py:45-46`). PK/contagem gera incompatibilidade de tipo; transformar de novo em máximo `ato_em` deixa backfill antigo invisível. O mapa omitido mais perigoso é o cron `app/config/crons.py:390 -> lavrar_perguntas_stale.py:60 -> raiox.py:38`, cujo consumidor continua comparando datetime (`raiox.py:46`). Precisa prova sobre ambos os consumidores, não só “marcador mudou”.

2. **Recusa passa a aceitar ato com projeção zerada e explode — segundo juiz.** Se a guarda de `recusar_pergunta` passar a ler ato (`app/chamados/services/validacao.py:813`) aceitando RESPOSTA com `respondida_em=None`, a mesma função executará `perg.respondida_em.isoformat()` (`:824`). A auditoria/followup também usa o texto cru (`:823,843`). Caso necessário: RESPOSTA no ato, R nulo, recusar pela tela e pelo fio (`app/chamados/views.py:518`; `app/chamados/services/fio.py:684`).

3. **Guarda pelo ato, escritor pelo texto velho — segundo escritor semântico fora da porta/segundo juiz.** O brief deixa `_carimbar_validacao` intacto; ele registra VALIDACAO com `ato_texto=perg.resposta_colab` (`app/chamados/services/validacao.py:175`), chamado depois da guarda em `:655`. Se F3 valida RESPOSTA cujo texto existe só no ato, a nova VALIDACAO documenta texto vazio/antigo. O caminho omitido `aplicar_declaracao -> validar_pergunta` está em `app/chamados/services/declaracao_texto.py:696`, além do upload em `app/api/foto_recebida.py:51`. A premissa de que nenhum escritor muda não resolve a incoerência do escritor já existente.

4. **BUG-PAR conserva outro juiz de V.** A guarda principal é `validacao.py:597`; a irmã é julgada separadamente por `q.validada_em is None` em `app/chamados/services/validacao.py:553`, seguida de nova VALIDACAO em `:554`. Migrar só a guarda principal pode duplicar a validação da irmã cujo ato já existe ou conservar uma validação devolvida. O caminho `validar_pergunta -> _validar_o_par` exige reconferência no código já com BUG-PAR, não apenas rebase textual (`:526-555`).

5. **Lote/chat permanecem com segundo juiz e confirmação crua.** `validar_lote_classe_a` rejeita por `perg.validada_em` antes da porta (`app/chamados/services/validacao.py:975`) e contabiliza sucesso pelo mesmo campo depois (`:996`); chama a porta em `:989`. Mudar a porta sem esses dois leitores deixa lote/chat discordarem de validar individual. Entradas omitidas pelo brief: `app/chamados/views_cobrar.py:261,381` e `app/core/views_ia.py:169`.

6. **Texto do ato na classificação, texto da projeção no delta — derivação paralela/segundo juiz.** A classificação lê texto em `app/chamados/services/validacao.py:1094-1106`, chama `delta_da_resposta` em `:1107` e `_slot_esperado` em `:1115`; o delta ainda lê `resposta_colab` em `app/chamados/juizes.py:1926`. A migração local pode classificar A/B usando textos diferentes. A unidade do delta é minuto (`juizes.py:1936-1948`); o instante de conflito precisa ser o slot do turno, não `data_evento + hora`: o caso noturno está explicitado em `validacao.py:1112-1114`. Teste deve divergir texto do ato/projeção e usar saída 07h do dia seguinte.

7. **D04 de equipe não autoriza mudar a unidade do aceite — derivação paralela.** `placar_equipe` conta ocorrências por autor (`app/chamados/services/verbos.py:267-271`), mas aceite conta colaboradores distintos com `vistos` (`:337-345`), não linhas RESPOSTA. A borda é `resposta > última cobrança` estrita (`:342`) e janela `>= d7` (`:335`). Contar linhas DP duplica pessoas. Há também first-seen sem ordenação: ao ver resposta de ontem, `:340` pula resposta de hoje do mesmo colaborador, de modo que o teste precisa verificar separadamente os conjuntos “hoje” e “7d”. Leitor omitido do brief: retrato de folha (`app/folha/views_retrato.py:515`).

8. **Relógios internos e dias civis — relógio fora da borda.** `placar_equipe` lê agora em `app/chamados/services/verbos.py:249`, aceite lê outro agora e outro dia em `:286-287`, painel lê outro agora em `app/chamados/services/painel.py:238-240`; o marcador tem quantum por `time.time()` (`painel.py:75-76`). “Hoje” é dia civil local e 7d são 168h deslizantes (`verbos.py:250-251`; `painel.py:236-240`), não dia do turno. Migrar para `ato_em__date`/UTC ou ler `now()` por item pode desalinhar os números na meia-noite. Precisa instante compartilhado na borda e RED da fronteira local; o brief não o inclui.

9. **Corte temporal e momento de inserção são coisas diferentes — relógio fora da borda/selo sem mordida.** `marcos_do_lote(ate=...)` corta inclusivamente em `__lte` (`app/chamados/veredito_lavrado.py:163-167`); a tarefa anuncia horizonte de atos e projeção (`app/chamados/services/tarefa_do_dia.py:381-385`) e passa `ate=agora` (`:434`). `DecisaoPergunta.ato_em` é instante do ato (`app/chamados/models.py:4040`), ordenado com pk (`:4056`), e a conservação fabrica atos de projeção anteriores (`:4081-4085`). Usar PK global sem corte temporal faz leitura histórica enxergar ato posterior; cortar por `ato_em` e chamar isso “estado que existia” não demonstra momento de inserção. O caso exigido precisa preservar o horizonte da tarefa e morder o consumidor, além do marcador.

## 17/11 14:48 · sessao 4c77d83733de · - · - · origem=Ronald (tmux)

F9 — leitura concluída sem alterações.

A cópia difere da base descrita: I21 já chama `q_respondida_com_texto`, em vez do filtro R cru do brief (`app/chamados/management/commands/auditar_invariantes_chamados.py:1574`). O contrato F1/F2 referido nos anexos não foi localizado: implementação de `atos_pergunta.leitura` e `_q_validada_pelo_ato` [nao verificado].

Para não inventar fechamento: o levantamento abaixo fecha as arestas estáticas dos comandos e helpers encontrados; o despacho interno de `tenant_command`, fornecido por dependência externa, é [nao verificado]. A entrada CLI local é `execute_from_command_line(sys.argv)` (`app/manage.py:18`); o render de cron usa `tenant_command` (`app/config/crons.py:21`, `app/config/crons.py:1899`).

Convenção: `C/` = `app/chamados/management/commands/`; `P/` = `app/ponto/management/commands/`. Cada linha abaixo é uma aresta; caminhos compostos seguem essas arestas.

caminhos: 21

1. cron `app/config/crons.py:542` -> `C/auditar_invariantes_chamados.py:553`
2. cron `app/config/crons.py:392` -> `P/reconciliar_fantasmas.py:152`
3. cron `app/config/crons.py:462` -> `P/reconciliar_grade.py:74`
4. auditor.handle `C/auditar_invariantes_chamados.py:737` -> universo_i6 `C/auditar_invariantes_chamados.py:455`
5. auditor.handle `C/auditar_invariantes_chamados.py:1587` -> _contraprova_i21a `C/auditar_invariantes_chamados.py:166`
6. auditor.handle `C/auditar_invariantes_chamados.py:1723` -> _contraprova_f1t `C/auditar_invariantes_chamados.py:184`
7. universo_i6 `C/auditar_invariantes_chamados.py:461` -> _i6_pares `C/auditar_invariantes_chamados.py:499`
8. perguntas_i6 `C/auditar_invariantes_chamados.py:496` -> universo_i6 `C/auditar_invariantes_chamados.py:455`
9. relavrar.handle `C/relavrar_via_i6.py:84` -> planejar `C/relavrar_via_i6.py:44`
10. planejar `C/relavrar_via_i6.py:49` -> universo_i6 `C/auditar_invariantes_chamados.py:455`
11. backfill.handle `C/backfill_decisoes_pergunta.py:455` -> linhas_a_criar `C/backfill_decisoes_pergunta.py:44`
12. linhas_a_criar `C/backfill_decisoes_pergunta.py:87` -> _escoar `C/backfill_decisoes_pergunta.py:65`
13. linhas_a_criar `C/backfill_decisoes_pergunta.py:90` -> _escoar `C/backfill_decisoes_pergunta.py:65`
14. backfill.handle `C/backfill_decisoes_pergunta.py:487` -> admin_decidiu_sem_ato `C/backfill_decisoes_pergunta.py:94`
15. backfill.handle `C/backfill_decisoes_pergunta.py:488` -> admin_decidiu_sem_ato `C/backfill_decisoes_pergunta.py:94`
16. backfill.handle, DRY com rollback `C/backfill_decisoes_pergunta.py:583` -> aplicar `C/backfill_decisoes_pergunta.py:163`
17. backfill.handle, APPLY `C/backfill_decisoes_pergunta.py:595` -> aplicar `C/backfill_decisoes_pergunta.py:163`
18. dedup.handle `C/dedup_perguntas_fato.py:105` -> aplicar `C/backfill_decisoes_pergunta.py:163`
19. limbo.handle `C/curar_perguntas_em_limbo.py:56` -> nota_da_resposta `C/curar_perguntas_em_limbo.py:26`
20. migrar.handle `C/migrar_data_evento_dia_do_turno.py:212` -> classificar `C/migrar_data_evento_dia_do_turno.py:129`
21. grade.handle `P/reconciliar_grade.py:228` -> _relavrar_orfa `P/reconciliar_grade.py:430`

As entradas CLI dos 12 comandos terminam em `handle`: auditor `C/auditar_invariantes_chamados.py:553`; backfill `C/backfill_decisoes_pergunta.py:452`; reabrir `C/reabrir_orfas_enterradas.py:53`; limbo `C/curar_perguntas_em_limbo.py:43`; auditor de disputas `C/auditar_disputas.py:62`; encerrar `C/encerrar_perguntas_com_fato.py:27`; dedup `C/dedup_perguntas_fato.py:48`; migrar `C/migrar_data_evento_dia_do_turno.py:195`; validar `C/validar_classe_a.py:38`; relavrar `C/relavrar_via_i6.py:82`; fantasmas `P/reconciliar_fantasmas.py:152`; grade `P/reconciliar_grade.py:74`. A chamada framework→handle é [nao verificado].

Inventário terminal conferido por AST, com critério explícito: atributo RVC, keyword ORM RVC ou argumento RVC de `only/values/values_list/defer/order_by`. São **56 referências**, sem contar chaves textuais de dicionários de trilha. Portanto, não reproduzir os “62 sítios” do anexo como contagem desta cópia.

| Arquivo | Referências terminais, arquivo:linha |
|---|---|
| `C/auditar_invariantes_chamados.py` | R `:177`; V `:205`; C `:470,:538,:588`; R `:592`; C `:905,:955`; V+C `:1075`; autor V `:1076`; R `:1105,:1106`; C `:1156`; V `:1576`; C `:1577`; V+C `:1604`; R `:1605,:1657`; V `:1768`; C `:1769`; V `:1780` |
| `C/backfill_decisoes_pergunta.py` | R+V `:56`; R+V+autor V `:60`; texto R `:61`; C `:101`; V `:102`; C `:325`; autor C+texto R `:326`; texto R `:504` |
| `C/reabrir_orfas_enterradas.py` | C `:67`; V `:68`; C `:88,:91,:129`; autor C `:130`; C `:142`; autor C `:143` |
| `C/curar_perguntas_em_limbo.py` | texto R `:32`; instante R+texto R `:46`; instante R `:69` |
| `C/auditar_disputas.py` | C `:71,:150` |
| `C/encerrar_perguntas_com_fato.py` | C+V `:32` |
| `C/dedup_perguntas_fato.py` | texto R `:75` |
| `C/migrar_data_evento_dia_do_turno.py` | C `:147` |
| `C/validar_classe_a.py` | texto R `:80` |
| `P/reconciliar_fantasmas.py` | C `:213` |
| `P/reconciliar_grade.py` | instante R `:461` |

lista fechada: 56 sitios **para esse critério AST**, não para toda string/chave de trilha nem para dispatch dinâmico [nao verificado].

Outras continuações que o censo precisa preservar:

- `_escoar` chama a conservação real, e não uma réplica (`C/backfill_decisoes_pergunta.py:67` -> `app/chamados/models.py:4081`).
- `aplicar` registra SILENCIO com instante, autor e texto da projeção (`C/backfill_decisoes_pergunta.py:324` -> `app/chamados/models.py:4110`).
- Registrar decisões acorda células; não é escrita isolada sem consequência (`app/chamados/models.py:4120`, `app/chamados/models.py:4121`).
- Limbo chama as duas portas de reparo (`C/curar_perguntas_em_limbo.py:67` -> `app/chamados/services/disputa_emissao.py:1683`; `C/curar_perguntas_em_limbo.py:69` -> `app/chamados/services/disputa_emissao.py:1576`).
- Validar classe A termina na validação de negócio (`C/validar_classe_a.py:100` -> `app/chamados/services/validacao.py:403` [linha da definição não reconferida: nao verificado]).
- Reabrir órfãs chama descarimbo e pode reabrir disputa e chamado pai (`C/reabrir_orfas_enterradas.py:157`, `C/reabrir_orfas_enterradas.py:165`, `C/reabrir_orfas_enterradas.py:172`).
- Encerrar termina na porta de fato e pode fechar disputa (`C/encerrar_perguntas_com_fato.py:68`, `C/encerrar_perguntas_com_fato.py:88`).
- Worklist importa helpers do comando fantasmas (`app/ponto/worklist.py:37`), usa o parser (`app/ponto/worklist.py:55`) e é chamada por view (`app/colaboradores/views.py:3065`) e comando (`app/ponto/management/commands/worklist_dp.py:13`). Esses helpers não contêm o sítio C `P/reconciliar_fantasmas.py:213`; não são uma entrada adicional nesse sítio.

Ataques adversariais:

1. **Retirar o prefiltro não equivale a trocar sua autoridade.** O brief permite remover o prefiltro de resposta de `_relavrar_orfa`; porém `fio_vivo` pergunta apenas se há qualquer pergunta viva, sem exigir resposta (`P/reconciliar_grade.py:460`, `app/chamados/juizes.py:1510`, `app/chamados/juizes.py:1519`). Pergunta pendente viva passaria a prender a relavra como respondida (`P/reconciliar_grade.py:462`). Classe: **derivação paralela / 2º juiz**. O ataque exige preservar “respondida E viva”, não apenas “viva” (`P/reconciliar_grade.py:458`).

2. **A cura de limbo proposta muda um contrato fechado e pode produzir RED sem mordida.** O comando compara o universo aos 21 IDs de `MEDIDAS`, recusa qualquer diferença e só carimba dois IDs fixos (`C/curar_perguntas_em_limbo.py:21`, `C/curar_perguntas_em_limbo.py:23`, `C/curar_perguntas_em_limbo.py:48`). Uma fixture genérica com ato e projeção zerada será recusada antes de alcançar as portas (`C/curar_perguntas_em_limbo.py:53`, `C/curar_perguntas_em_limbo.py:67`). Classe: **selo sem mordida**. A declaração de cron confirma cura única de acervo, não reparador geral (`app/config/crons.py:1476`).

3. **Texto do ato na procura da nota não basta para reparar a projeção.** `nota_da_resposta` procura texto da projeção (`C/curar_perguntas_em_limbo.py:32`); a porta de carimbo também exige e registra texto da projeção (`app/chamados/services/disputa_emissao.py:1692`, `app/chamados/services/disputa_emissao.py:1700`). Selecionar “ato RESPOSTA com projeção zerada” deixa a porta em no-op. Classe: **2º escritor fora da porta**, caso o construtor tente preencher texto no comando; ou **selo sem mordida**, caso só mude a seleção.

4. **I18 pode misturar validação do ato com autor da projeção.** A proposta troca V por SQL-V, mas o filtro humano continua em `validada_por__username` (`C/auditar_invariantes_chamados.py:1075`, `C/auditar_invariantes_chamados.py:1076`). Validação corrente no ato por sistema, com autor humano velho na projeção — ou o inverso — muda a classificação incorretamente. A conservação associa autor ao instante da validação (`app/chamados/models.py:4102`, `app/chamados/models.py:4105`). Classe: **derivação paralela / 2º juiz**.

5. **O sítio C da migração não é só rótulo.** `est` calculado pelo carimbo escolhe `_fonte_materializada` versus `_fonte_viva`, e condiciona qual universo pode migrar (`C/migrar_data_evento_dia_do_turno.py:147`, `C/migrar_data_evento_dia_do_turno.py:151`, `C/migrar_data_evento_dia_do_turno.py:158`). O primeiro ramo exige batida plantada viva; o segundo infere fonte de turno aberto ou só de saída (`C/migrar_data_evento_dia_do_turno.py:68`, `C/migrar_data_evento_dia_do_turno.py:98`, `C/migrar_data_evento_dia_do_turno.py:108`). Trocar o campo como simples SHOW quebra seleção. Classe: **2º juiz / derivação paralela**.

6. **Dia do turno, dia civil e instante têm unidades distintas.** Migração usa dia local da saída para identificar o erro e pede ao juiz o dia do turno da fonte (`C/migrar_data_evento_dia_do_turno.py:158`, `C/migrar_data_evento_dia_do_turno.py:160`). Backfill separa conversa porque sua `data_evento` é dia civil, sem competência de fato (`C/backfill_decisoes_pergunta.py:73`, `C/backfill_decisoes_pergunta.py:465`). Derivar competência de `ato_em.date()` ou tratar conversa como fato altera o universo protegido. Classe: **relógio fora da borda / derivação paralela**.

7. **Os cortes temporais existentes não têm a mesma semântica.** Órfãs usa janela aware fixa −03, com ambas as pontas inclusivas (`C/reabrir_orfas_enterradas.py:36`, `C/reabrir_orfas_enterradas.py:88`). I6 usa `.date()` diretamente no carimbo e inclui o dia do corte no piso (`C/auditar_invariantes_chamados.py:478`). Trocar C por instante de ato, ou normalizar datas sem decisão explícita, pode mudar lado do corte nas horas próximas à meia-noite. O timezone efetivo do carimbo retornado pelo banco é [nao verificado]. Classe: **relógio fora da borda**.

8. **O DRY também chega ao escritor e aos seus callbacks.** O brief cita o comando e o apply, mas não explicita que o DRY executa a porta real num atomic depois revertido (`C/backfill_decisoes_pergunta.py:582`, `C/backfill_decisoes_pergunta.py:583`, `C/backfill_decisoes_pergunta.py:584`). Essa porta registra decisão (`C/backfill_decisoes_pergunta.py:324`), que acorda células (`app/chamados/models.py:4121`). Um selo que exercite só seleção ou só APPLY não protege essa via. Classe: **selo sem mordida**. Efeitos externos que escapem ao rollback são [nao verificado].

9. **Conservação por correspondência não equivale a “existe ato”.** O escritor conserva por `(pergunta, ato, instante)` (`app/chamados/models.py:4094`, `app/chamados/models.py:4098`, `app/chamados/models.py:4102`); `linhas_a_criar` delega a ele (`C/backfill_decisoes_pergunta.py:67`). Uma seleção SQL baseada só na existência de RESPOSTA/VALIDACAO perde a re-resposta posterior ou ressuscita estado já devolvido. Classe: **2º juiz / derivação paralela**. Paridade real das faces SQL F1/F2 com essa conservação é [nao verificado].

ataques cruzados: 9

As respostas desses nove ataques em linha de brief/corte não estão comprovadas pela cópia: [nao verificado]. Não autorizar declaração `nao verificado 0` com este resultado.

## 17/11 14:48 · sessao 597490bfcbf2 · - · - · origem=Ronald (tmux)

Convenção: D=`app/chamados/services/disputa_emissao.py`; M=`app/chamados/services/materializacao.py`. São 48 arestas diretas verificadas para 20 funções do núcleo; o fechamento transitivo completo permanece [nao verificado]. A cópia já contém outros caminhos do par de intervalo, inclusive recursão `_materializar_uma -> _gravar_irma_do_par -> _materializar_uma` (M:1371, M:1707).

Mapa direto:

```text
_depois app/ponto/signals.py:262 -> devolver_absorvidas_da_ausencia_rejeitada M:1121
_validar_pelo_dono app/api/foto_recebida.py:48 -> responder_automaticamente D:1654
api_disputa_responder app/api/views.py:2627 -> responder_pergunta_disputa_core D:1439
reabrir_por_colab app/chamados/models.py:2318 -> reabrir_para_correcao D:1576
_slot_esperado app/chamados/juizes.py:376 -> _ts_tipo_do_plantio M:599
responder_pergunta_disputa app/chamados/views.py:1283 -> responder_pergunta_disputa_core D:1439
reabrir_disputa_supervisao app/chamados/views.py:1425 -> recobrar_questionario D:1712
reabrir_disputa_supervisao app/chamados/views.py:1436 -> reabrir_para_correcao D:1576
materializa_perguntas_ao_fechar_disputa app/chamados/signals.py:165 -> materializar_pendentes_aprovadas M:2098
_plantar_o_dia app/chamados/services/passivo_validado.py:537 -> _materializar_uma M:1809
par_do_intervalo M:214 -> _ts_tipo_do_plantio M:599
par_do_intervalo M:222 -> _ts_tipo_do_plantio M:599
respostas_intervalo_recusadas_paridade M:339 -> estado_aplicacao_pergunta M:852
_ts_tipo_do_plantio M:656 -> _tipo_pelo_marco M:512
espera_declarado_x_gravado M:757 -> estado_aplicacao_pergunta M:852
_estado_pelo_derivador M:895 -> _ts_tipo_do_plantio M:599
devolver_absorvidas_da_ausencia_rejeitada M:1147 -> perguntas_absorvidas_por M:1085
devolver_absorvidas_da_ausencia_rejeitada M:1164 -> reabrir_para_correcao D:1576
_materializar_resposta_hora M:1328 -> _ts_tipo_do_plantio M:599
_materializar_resposta_escolha M:1641 -> _declarar_ausencia_de_pergunta M:944
_gravar_irma_do_par M:1707 -> _materializar_uma M:1809
_materializar_uma M:1889 -> _materializar_resposta_hora M:1266
_materializar_uma M:1891 -> _materializar_resposta_data_hora M:1541
_materializar_uma M:1893 -> _materializar_resposta_escolha M:1626
materializar_perguntas_validadas_da_disputa M:2084 -> _materializar_uma M:1809
_validar_o_par app/chamados/services/validacao.py:555 -> _materializar_uma M:1809
validar_pergunta app/chamados/services/validacao.py:687 -> _materializar_uma M:1809
validar_pergunta app/chamados/services/validacao.py:698 -> materializar_perguntas_validadas_da_disputa M:2032
recusar_pergunta app/chamados/services/validacao.py:836 -> reabrir_para_correcao D:1576
declarar_hora_colab app/chamados/services/declaracao_texto.py:419 -> responder_pergunta_disputa_core D:1439
aplicar_declaracao app/chamados/services/declaracao_texto.py:693 -> responder_pergunta_disputa_core D:1439
responder_pergunta_disputa_core D:1555 -> _materializar_ausencia_de_resposta M:978
resolver_perguntas_por_chamado_resolvido D:1897 -> _resolver_perguntas_por_chamado_resolvido D:1906
resolver_perguntas_por_chamado_resolvido D:1903 -> _resolver_perguntas_por_chamado_resolvido D:1906
_resolver_perguntas_por_chamado_resolvido D:1969 -> reabrir_credor_de_pergunta_viva D:1827
explicar_pergunta app/chamados/services/explicador.py:124 -> estado_aplicacao_pergunta M:852
acao_reabrir_disputa app/chamados/services/acoes_disputa.py:105 -> reabrir_para_correcao D:1576
acao_regularizar_disputa app/chamados/services/acoes_disputa.py:141 -> materializar_perguntas_validadas_da_disputa M:2032
acao_corrigir_resposta app/chamados/services/acoes_disputa.py:373 -> responder_pergunta_disputa_core D:1439
montar_chamados_fio app/chamados/services/fio.py:376 -> estado_aplicacao_pergunta M:852
processar_post_fio app/chamados/services/fio.py:716 -> materializar_perguntas_validadas_da_disputa M:2032
aplicacao_da_resposta app/chamados/templatetags/chamados_tags.py:136 -> estado_aplicacao_pergunta M:852
handle app/chamados/management/commands/curar_perguntas_em_limbo.py:67 -> carimbar_resposta_pela_trilha D:1683
handle app/chamados/management/commands/curar_perguntas_em_limbo.py:69 -> reabrir_para_correcao D:1576
ato_do_dia app/chamados/management/commands/backfill_fio_morto_e4b.py:90 -> reabrir_credor_de_pergunta_viva D:1827
_executar_dia app/chamados/management/commands/backfill_fio_morto_e4b.py:135 -> reabrir_credor_de_pergunta_viva D:1827
_executar_dia app/chamados/management/commands/backfill_fio_morto_e4b.py:147 -> materializar_perguntas_validadas_da_disputa M:2032
_exportadas_classe_e app/chamados/management/commands/backfill_fio_morto_e4b.py:175 -> reabrir_credor_de_pergunta_viva D:1827
```

caminhos: 48  
lista fechada: 20 sitios do núcleo acima; fechamento de todo alcance [nao verificado].

Pontes verificadas para compor com o mapa:

```text
resolver_disputas_de_foto app/api/foto_recebida.py:74 -> _validar_pelo_dono app/api/foto_recebida.py:28
api_upload_foto app/api/views_core.py:845 -> resolver_disputas_de_foto app/api/foto_recebida.py:57
api_upload_foto app/api/views.py:861 -> resolver_disputas_de_foto app/api/foto_recebida.py:57
_validar_pelo_dono app/api/foto_recebida.py:51 -> validar_pergunta app/chamados/services/validacao.py:574
api_solicitar_correcao_disputa app/api/views.py:2686 -> reabrir_por_colab app/chamados/models.py:2263
solicitar_correcao_disputa app/chamados/views.py:1334 -> reabrir_por_colab app/chamados/models.py:2263
responder_chamado_colaborador app/chamados/views.py:1090 -> declarar_hora_colab app/chamados/services/declaracao_texto.py:306
detalhe_chamado_local app/chamados/views.py:475 -> acao_regularizar_disputa app/chamados/services/acoes_disputa.py:120
detalhe_chamado_local app/chamados/views.py:480 -> aplicar_declaracao app/chamados/services/declaracao_texto.py:641
detalhe_chamado_local app/chamados/views.py:501 -> acao_validar_pergunta app/chamados/services/acoes_disputa.py:13
detalhe_chamado_local app/chamados/views.py:518 -> acao_recusar_pergunta app/chamados/services/acoes_disputa.py:67
detalhe_chamado_local app/chamados/views.py:536 -> acao_corrigir_resposta app/chamados/services/acoes_disputa.py:304
meu_atendimento app/chamados/views.py:569 -> montar_chamados_fio app/chamados/services/fio.py:190
fio_colaborador app/chamados/views.py:674 -> processar_post_fio app/chamados/services/fio.py:531
fio_colaborador app/chamados/views.py:677 -> montar_chamados_fio app/chamados/services/fio.py:190
_resp_modal_full app/chamados/views.py:889 -> montar_chamados_fio app/chamados/services/fio.py:190
modal_fio_colaborador app/chamados/views.py:954 -> processar_post_fio app/chamados/services/fio.py:531
contexto_parcial_fio app/chamados/services/fio.py:492 -> montar_chamados_fio app/chamados/services/fio.py:190
acao_validar_pergunta app/chamados/services/acoes_disputa.py:19 -> validar_pergunta app/chamados/services/validacao.py:574
acao_recusar_pergunta app/chamados/services/acoes_disputa.py:82 -> recusar_pergunta app/chamados/services/validacao.py:770
acao_corrigir_resposta app/chamados/services/acoes_disputa.py:381 -> validar_pergunta app/chamados/services/validacao.py:574
_validar_o_par app/chamados/services/validacao.py:545 -> par_do_intervalo M:175
_gravar_irma_do_par M:1695 -> par_do_intervalo M:175
_materializar_resposta_hora M:1371 -> _gravar_irma_do_par M:1687
validar_pergunta app/chamados/services/validacao.py:683 -> _validar_o_par app/chamados/services/validacao.py:526
_pergunta_materializada_de_fato app/chamados/juizes.py:756 -> _slot_esperado app/chamados/juizes.py:345
classificar app/chamados/services/vigia_carimbo.py:110 -> _slot_esperado app/chamados/juizes.py:345
classe_do_lote app/chamados/services/validacao.py:1115 -> _slot_esperado app/chamados/juizes.py:345
resolve_perguntas_ao_resolver app/chamados/signals.py:117 -> resolver_perguntas_por_chamado_resolvido D:1887
handle app/ponto/management/commands/reconciliar_fantasmas.py:281 -> resolver_perguntas_por_chamado_resolvido D:1887
handle app/chamados/management/commands/validar_classe_a.py:100 -> validar_pergunta app/chamados/services/validacao.py:574
handle app/chamados/management/commands/bug_par_103.py:79 -> respostas_intervalo_recusadas_paridade M:295
```

Essas são outras 32 arestas; subtotal apresentado 80. Há dispatch de `post_save` para `resolve_perguntas_ao_resolver` e `materializa_perguntas_ao_fechar_disputa` (`app/chamados/signals.py:91`, `:149`); `Ausencia.post_save/post_delete` chega a `_consumidor_ausencia` (`app/ponto/signals.py:275`, `:279`, `:282`, `:284`), que agenda `_depois` por `on_commit` (`app/ponto/signals.py:254`, `:272`). A configuração chama `reconciliar_fantasmas --alarme` a cada quinze minutos (`app/config/crons.py:392`); a configuração de `backfill_fio_morto_e4b`, `materializar_passivo_validado`, `validar_classe_a` e `curar_perguntas_em_limbo` é de operação não rotineira (`app/config/crons.py:791`, `:853`, `:1363`, `:1476`). O wrapper completo cron/tenant desses caminhos permanece [nao verificado].

Omissões concretas do censo do brief F4: o par agora chama a derivação em M:214/M:222; existe leitor adicional em `explicador.py:124` e filtro em `chamados_tags.py:136`; as declarações chegam ao escritor por `declaracao_texto.py:419/:693`; a API chega por `api/views.py:2627`; a reabertura de supervisão chama diretamente D:1576 em `views.py:1436`. Referências correspondem ao mapa acima.

Ataques:

1. **Par fora dos três chamadores — derivação paralela/segundo juiz.** O RED-4 do brief enumera três entradas, mas o par deriva as duas metades em M:214/M:222 e decide validade lendo `q.validada_em` em M:199/M:201. A porta do admin chama esse par em `validacao.py:545`; a porta do plantio o chama em M:1695 e planta a irmã por M:1707. Migrar só os três leitores antigos pode fazer tela/porta/irmã discordarem sobre resposta e validade. A cura efetiva ainda é [nao verificado].

2. **`data_hora` conserva outro parser — derivação paralela.** `_slot_esperado` deriva `data_hora` diretamente da projeção em `app/chamados/juizes.py:363-370`; o materializador deriva o mesmo timestamp em M:1558-1565. Se F4 usar texto do ato apenas no materializador, o juiz continuará esperando o texto da projeção; M:1899-1906 reprova e desfaz o plantio. Esse caminho não está no RED de paridade dos “três chamadores” do brief.

3. **Relógio da trilha ≠ relógio de processamento — relógio fora da borda.** O upload fixa um `agora` de processamento em `app/api/foto_recebida.py:42` e entrega a resposta automática em :48; D:1675/D:1678 usam esse instante. A cura do limbo usa o instante histórico passado em D:1696/D:1699. Unificar essas guardas por “último texto do ato” sem preservar a fonte temporal pode transformar uma resposta histórica anterior à reabertura em resposta corrente. A ordem real de atos em produção é [nao verificado].

4. **Dia de turno ≠ dia civil — relógio fora da borda.** A derivação de hora faz `make_aware` com `data_evento` em M:685 e ajusta overnight em M:692; o envelope aceita as duas bordas (`<=`) em `app/chamados/juizes.py:328-333`. O passivo consulta competência por `data_evento`, usando `localdate(ts)` apenas como fallback (`app/chamados/services/passivo_validado.py:98-113`). Trocar o texto por ato não autoriza derivar competência a partir de `ato_em` nem do dia civil da batida. Contraexemplo obrigatório: turno do último dia da competência com saída no dia seguinte, exatamente na borda de 90 minutos. Frequência em produção [nao verificado].

5. **ABSORÇÃO não é apenas estado corrente — segundo juiz/segundo escritor.** A escrita registra vínculo explícito `absorcao_escape`/`absorcao_ausencia` em M:1045-1048; o leitor atual identifica uma geração por `(disputa, materializada_em)` em M:1109-1115 e exclui batidas vivas em M:1116-1118. A rejeição chega por callback pós-commit (`app/ponto/signals.py:262`, `:272`) e efetivamente apaga resposta/validade/carimbo por M:1164 -> D:1648. Um fold “existe ABSORCAO” sem geração e sem desfazimento pode devolver perguntas de absorção antiga. O brief mantém esse corte aberto; resolução implementada [nao verificado].

6. **Backfill não fecha escritor vivo — segundo escritor fora da porta.** `PerguntaDisputa.carimbar` sintetiza `respondida_em`/`validada_em` em `app/chamados/models.py:3240-3247` e salva em :3262 sem registrar RESPOSTA/VALIDACAO. Além de M:1043 e D:2082, há o caminho central M:1944-1945 e o resolver D:2008-2009. O último não está na lista de dois furos do §7 do brief F4. `linhas_da_projecao_sem_linha` apenas recompõe por `(ato, instante)` quando chamada (`app/chamados/models.py:4094-4105`); executar backfill uma vez não impede novas lacunas. Números de frota permanecem apenas dado fornecido [nao verificado].

7. **Unidade de plantio é o dia/par, não pergunta isolada — segundo juiz/selo sem mordida.** A porta do passivo faz savepoint do dia, mede geometria antes/depois e recusa batidas fora da janela (`app/chamados/services/passivo_validado.py:505-513`, `:526-550`); `_materializar_uma` também planta uma irmã por M:1371/M:1707. REDs que só verificam uma pergunta e contagens antes/depois podem passar mesmo deixando a outra metade com texto, validade ou timestamp de autoridade diferente. O caminho `passivo_validado._plantar_o_dia -> _materializar_uma` está citado, mas a exigência de paridade de atos das duas metades não está respondida no brief [nao verificado].

8. **Selo limitado aos cinco arquivos não morde leitores de retorno — selo sem mordida.** A troca de texto/instante alcança `explicador.py:124`, `chamados_tags.py:136`, `juizes._slot_esperado` em :376 e seus leitores `vigia_carimbo.py:110`/`validacao.py:1115`. Um AST que permita funções inteiras como exceção ou só varra os cinco arquivos do brief não detecta a derivação `data_hora` cru em `juizes.py:364`, nem validade cru da irmã em M:199/M:201. Implementação e mutantes do novo selo [nao verificado].

ataques cruzados: 8

## 17/11 14:47 · sessao 8434c99b0027 · COMPLEMENTO-1-1410 · verificador · origem=Code

**AVAL-DEVOLVIDO — somente item 1:** procedem as ressalvas sobre o critério de passagem e a exceção; a objeção de guarda inexistente não procede nesta cópia. (`app/chamados/reconciliador.py:576`; `comp1_aval.txt:2`, anexo transcrito.)

As referências a `comp1_aval.txt` contam as linhas do texto fornecido; correspondência com arquivo em disco: **[nao verificado]**.

1. **As premissas conferem?**

   **(a) PROCEDE: a guarda existe e é nomeável.** Chama-se `_retratacao_toca_entregue`; compõe `veto_de_competencia` sobre as perguntas alcançadas pela retratação. `retratar_com_motivo` consulta-a exatamente em `:576`, registra a recusa e retorna antes da transição em `:580`. Um AST reconhece a chamada e esse ramo. A leitura anterior que negava essa consulta não se sustenta aqui. (`app/chamados/reconciliador.py:515`, `app/chamados/reconciliador.py:528`, `app/chamados/reconciliador.py:538`, `app/chamados/reconciliador.py:576`, `app/chamados/reconciliador.py:579`.)

   **(b) PROCEDE a possibilidade de enumerar; NÃO PROCEDE equiparar os 15 sítios a 15 escritas.** Há operações concretas reconhecíveis: transição por `save`, carimbo por `save`, costura e lavra por `bulk_update`. Porém, `lavrar(salvar=False)` atribui campos em memória e deixa a persistência para outro corpo. Portanto, listar nomes `save/update/bulk_update` ou os 15 nomes de fronteira não identifica, sozinho, todas as escritas protegidas e sua primeira ocorrência na unidade. (`app/chamados/models.py:296`, `app/chamados/models.py:3273`, `app/chamados/services/fio.py:974`, `app/chamados/services/fio.py:975`, `app/chamados/veredito_lavrado.py:250`, `app/chamados/veredito_lavrado.py:270`, `app/chamados/veredito_lavrado.py:274`.)

   **(c) NÃO PROCEDE dizer que callback, reexport e apelido tornam este censo impossível.** As ligações mostradas são explícitas: `ato=ch.reconciliar`, execução de `ato()`, reexport por `ImportFrom` e `retratar as _retr`. São rastreáveis, embora uma visita apenas a chamadas diretas perca a referência passada como callback. (`app/chamados/reconciliador.py:580`, `app/chamados/services/fecho_runtime.py:165`, `app/chamados/models.py:4273`, `app/ponto/management/commands/supra_juiz.py:258`, `app/ponto/management/commands/supra_juiz.py:286`.)

   **PROCEDE a ressalva sobre a garantia geral:** reconhecer essas arestas não prova passagem obrigatória pela guarda em todos os caminhos. O AST existente já declara limites para receptor sem tipo e campos recebidos dinamicamente; a garantia irrestrita pedida pelo novo selo permanece **[nao verificado]**. (`app/chamados/tests/test_selo_status_so_pelas_portas.py:17`.)

2. **Colide com aval vigente?**

   **PROCEDE a tensão literal:** “allowlist vazia” e “exceção NOMEADA com motivo” coexistem no mesmo item. Se a exceção permite verde sem guarda, há dispensa; se permanece vermelha, não dispensa o sítio. O texto não determina esse efeito. (`comp1_aval.txt:2`, anexo transcrito.) A regra local exige cumprimento literal das condições do aval. (`CLAUDE.md:37`.) [nao verificado]

   **NÃO PROCEDE colisão automática com os tripwires existentes.** Eles prendem portas de status/disputa, atos de lavra e a guarda C6 `_vivas_entre`; essas perguntas diferem da passagem pela guarda de competência. O teste B2 atual contém mutante `sem_cerca` por mock e casos comportamentais, sem demonstrar o censo AST do 16º chamador solicitado. (`app/chamados/tests/test_selo_status_so_pelas_portas.py:37`, `app/chamados/tests/test_porta_veredito_pergunta.py:94`, `app/chamados/tests/test_o261e_f1_selo_encerramento.py:10`, `app/chamados/tests/test_e4c_b2_cerca_retratar.py:64`, `app/chamados/tests/test_e4c_b2_cerca_retratar.py:441`.)

3. **Alguma condição não fecha no literal?**

   **PROCEDE quanto a “passa pela guarda”.** A implementação faz consulta **e usa o resultado para impedir o ato**; conservar a chamada e retirar o retorno deixa o nome presente, mas desfaz a proteção. Uma ocorrência anterior no texto não equivale a esse comportamento. (`app/chamados/reconciliador.py:576`, `app/chamados/reconciliador.py:577`, `app/chamados/reconciliador.py:579`.)

   **PROCEDE quanto ao alcance de “chamador”, inclusive o 16º.** Há chamador de chamador legítimo: `retratar → _retratar → retratar_com_motivo`. Os dois primeiros não contêm a consulta localmente. Exigir guarda no mesmo corpo os reprovaria; aceitar qualquer ancestral guardado não demonstra que toda entrada esteja protegida. O item não fixa como essas relações entram na aceitação do selo. (`app/chamados/reconciliador.py:543`, `app/chamados/reconciliador.py:576`, `app/chamados/reconciliador.py:603`; `comp1_aval.txt:2`, anexo transcrito.)

   **NÃO PROCEDE interpretar proteção como zero escrita absoluta:** retenção e recusa podem gravar sua trilha; o teste existente a declara expressamente permitida. (`app/chamados/reconciliador.py:566`, `app/chamados/reconciliador.py:578`, `app/chamados/tests/test_e4c_b2_cerca_retratar.py:31`.)

4. **Toca exportada, dinheiro ou `!` fora do declarado?**

   **NÃO PROCEDE essa extrapolação pelo item descrito:** ele exige um teste e exclui porta do export e produção. A guarda consulta o estado de competência entregue, já declarado na B2. Isso não autoriza alterar a porta nem aplicar dados. (`comp1_aval.txt:2`, `comp1_aval.txt:5`, anexo transcrito; `app/chamados/services/fio.py:1357`, `app/chamados/services/fio.py:1364`, `app/chamados/tests/test_e4c_b2_cerca_retratar.py:4`.) Implementação, execução e efeito financeiro do novo selo: **[nao verificado]**.

## 17/11 14:46 · sessao cec369ddab7a · - · - · origem=Ronald (tmux)

- **1(b), nomes das portas: NÃO PROCEDE negar que sejam reconhecíveis por AST.** Há `save` de transição, carimbo e disputa em `app/chamados/models.py:296`, `app/chamados/models.py:3273`, `app/chamados/models.py:2027`; costura em `app/chamados/services/fio.py:975`; lote em `app/chamados/veredito_lavrado.py:250`.

- **1(b), equivalência entre 15 frentes e conjunto fechado de escritas: PROCEDE a ressalva.** `lavrar` pode alterar somente memória; quem persiste é outro corpo. Isso aparece em `app/chamados/veredito_lavrado.py:274`, `app/chamados/models.py:3271`, `app/chamados/models.py:3273` e `app/chamados/services/fio.py:974`. Nome de verbo sozinho não identifica campo, modelo e unidade; o selo existente declara limite para receptores/tipos e campos genéricos em `app/chamados/tests/test_selo_status_so_pelas_portas.py:17`.

- **1(c), callback/reexport/alias tornarem este censo impossível: NÃO PROCEDE.** O callback literal `ato=ch.reconciliar` está em `app/chamados/signals.py:50`, executado em `app/chamados/services/fecho_runtime.py:160` e `app/chamados/services/fecho_runtime.py:165`. O reexport é explícito em `app/chamados/models.py:4273`; o alias `retratar as _retr`, em `app/ponto/management/commands/supra_juiz.py:258`, chamado em `app/ponto/management/commands/supra_juiz.py:286`. Essas ligações são rastreáveis; visitar apenas `ast.Call` perde a referência callback em `app/chamados/signals.py:50`.

- **1(c)/3, AST sintático local provar passagem obrigatória em qualquer caminho: PROCEDE a limitação.** Ordem textual não basta para resolver identidade do receptor, propagação de callbacks e persistência posterior; a cópia já documenta limites de resolução estática em `app/chamados/tests/test_selo_status_so_pelas_portas.py:17`. Há escrita de estado antes da lavra subsequente em `app/chamados/models.py:296` e `app/chamados/models.py:312`.

- **4, selo AST implicar escrita em exportada ou dinheiro: NÃO PROCEDE pelo mecanismo consultado.** A cadeia existente consulta competência em `app/chamados/services/fio.py:1364`, `app/chamados/services/vigia_fio.py:800`, `app/chamados/services/vigia_fio.py:805` e lê exportações em `app/folha/export.py:588`, retornando o resultado em `app/folha/export.py:594`. A proteção de ENTREGUE está expressamente declarada no teste em `app/chamados/tests/test_e4c_b2_cerca_retratar.py:4`; a escrita de trilha permitida consta em `app/chamados/tests/test_e4c_b2_cerca_retratar.py:31`. Efeito financeiro concreto, implementação e execução do novo selo: **[nao verificado]**.

## 17/11 14:45 · sessao 48261a22038a · - · - · origem=Ronald (tmux)

**2 — Colisão com aval vigente**

- **PROCEDE quanto à tensão “allowlist vazia” × “exceção NOMEADA que perdoa sítio”**: ambas as cláusulas estão no item 1 fornecido; se a exceção permitir passar sem guarda, a allowlist deixou de estar vazia. O anexo não existe como arquivo local encontrado: **[nao verificado]** para arquivo:linha do próprio aval. A lei local exige escopo literal e exceção como porta separada com trilha, sem destravar a trava (`CLAUDE.md:32`, `CLAUDE.md:37`).
- **NÃO PROCEDE colisão automática por sobreposição com selos existentes.** Já há selo das escritas de status/disputa por portas nominais com motivo (`app/chamados/tests/test_selo_status_so_pelas_portas.py:1`, `app/chamados/tests/test_selo_status_so_pelas_portas.py:37`); selo de `reapontar_resolvedora` por ato exato (`app/chamados/tests/test_porta_reapontamento_pergunta.py:88`, `app/chamados/tests/test_porta_reapontamento_pergunta.py:118`); selo de `lavrar`/`gravar_lavras_em_lote` (`app/chamados/tests/test_porta_veredito_pergunta.py:94`, `app/chamados/tests/test_porta_veredito_pergunta.py:123`). Esses selos prendem escritores/portas, não consulta K6.
- **NÃO PROCEDE tratar o tripwire C6 existente como o selo K6 pedido.** Ele verifica `_vivas_entre` e `raise` antes de atribuir `status_local`; sua própria descrição distingue essa pergunta dos censos de escrita (`app/chamados/tests/test_o261e_f1_selo_encerramento.py:5`, `app/chamados/tests/test_o261e_f1_selo_encerramento.py:10`, `app/chamados/tests/test_o261e_f1_selo_encerramento.py:24`).
- **NÃO PROCEDE afirmar que o selo B2 atual já garante o 16º chamador por AST.** O existente aplica `mock` a `_retratacao_toca_entregue` e compara resultados de casos em transações (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:64`, `app/chamados/tests/test_e4c_b2_cerca_retratar.py:441`, `app/chamados/tests/test_e4c_b2_cerca_retratar.py:461`).

**3 — Condições literais**

- **NÃO PROCEDE dizer que o momento da barreira está inteiramente indefinido:** a matriz fornecida exige a carga inteira do ato antes da primeira escrita funcional; arquivo:linha local desse anexo: **[nao verificado]**. A implementação concreta consulta em `:576`, recusa por retorno em `:579` e só depois inicia a transição em `:580` (`app/chamados/reconciliador.py:576`, `app/chamados/reconciliador.py:579`, `app/chamados/reconciliador.py:580`).
- **PROCEDE a insuficiência de uma mera ocorrência lexical da guarda.** O mecanismo é consulta **mais uso do resultado para impedir o ato**; retirar o `return` ou ignorar `_vetadas` preserva o nome da chamada, mas rompe a proteção (`app/chamados/reconciliador.py:576`, `app/chamados/reconciliador.py:577`, `app/chamados/reconciliador.py:579`).
- **PROCEDE a indefinição do alcance de “chamador” no teste solicitado:** há callback executado em dois ramos (`app/chamados/services/fecho_runtime.py:160`, `app/chamados/services/fecho_runtime.py:165`) e reexport explícito (`app/chamados/models.py:4273`, `app/chamados/models.py:4274`); chamar uma função que chama outra não equivale à busca de `ast.Call` diretamente ao nome protegido.
- **NÃO PROCEDE exigir literalmente zero INSERTs antes da escrita protegida:** a própria retenção prévia registra trilha e retorna; a recusa K6 também registra trilha (`app/chamados/reconciliador.py:564`, `app/chamados/reconciliador.py:566`, `app/chamados/reconciliador.py:578`). O teste documenta a trilha como única escrita permitida na recusa (`app/chamados/tests/test_e4c_b2_cerca_retratar.py:31`).
- **PROCEDE distinguir atribuição em memória de persistência:** `lavrar(salvar=False)` atribui os três campos e deixa o save para o chamador; o lote persiste por `bulk_update` (`app/chamados/veredito_lavrado.py:254`, `app/chamados/veredito_lavrado.py:270`, `app/chamados/veredito_lavrado.py:274`, `app/chamados/veredito_lavrado.py:250`).

A fonte declara limites reais de seu AST atual para parâmetros genéricos, receptor sem tipo e argumentos dinâmicos; portanto, ela própria não sustenta universalidade sem recorte (`app/chamados/tests/test_selo_status_so_pelas_portas.py:17`).

## 17/11 14:45 · sessao 5401f539c5d7 · CONFRONTO-PILOTO · sonda · origem=Code

Reproduzo a saída raspada fornecida; nova execução e conteúdo de `sonda_34.py`: [nao verificado].

SAIDA_BEGIN
```text
sonda_leitura: sonda_34.py [6638f43f42690528] no cpuset 4-7, lendo sombra como leitor (LEITURA)

CHAVES_DIA ['ausencia', 'ausencia_icone', 'batidas', 'celulas_isencao', 'cor_dia', 'data', 'dia_pago', 'dia_pago_lavrada', 'falta_frase', 'falta_marcos', 'feriado', 'folga', 'he_fora_da_janela', 'inconsistente', 'isencao', 'minutos_realizados', 'orfas_grade', 'pago_h', 'palavra_dia', 'previsto', 'previsto_canon', 'realizado_aberto', 'realizado_grade', 'realizado_h', 'realizado_janela_descontada', 'realizado_longo', 'realizado_sem_turno', 'rubricas', 'turnos', 'veredito_dia']
RUBRICAS_EX {'lavrada': True, 'extras_50': 0.0, 'extras_100': 0.0, 'atraso': 0.0, 'antecipada': 0.0, 'noturnas': 0.8470222222222222, 'intra': 0.0, 'trabalhadas': 10.862295833333333, 'minutos_extra': 0.0}
RESUMO_CHAVES ['acrescimo_noturno', 'datas_em_aberto', 'datas_falta', 'datas_furo_apurado', 'datas_saida_orfa', 'datas_turno_aberto', 'dias_abono', 'dias_em_aberto', 'dias_falta', 'dias_furo_apurado', 'dias_saida_orfa', 'dias_turno_aberto', 'dias_inconsistentes', 'horas_abono', 'horas_falta', 'horas_previstas', 'noturnas_relogio', 'saldo', 'saldo_banco_horas', 'total_atraso', 'total_extras', 'total_extras_100', 'total_extras_50', 'total_intra_indenizada', 'total_noturnas', 'total_saida_antecipada', 'total_trabalhadas', 'trab_folga', 'turnos', 'turnos_abertos']
ITEM3 dias=351 (13 x 27); classes: {'difere_4_marcacoes': 96, 'sem_marcacao_nos_sem_numero': 149, 'par_unico': 50, 'igual': 48, 'dia_impar_nos_sem_numero': 2, 'dia_impar': 6}
EX dia_impar: C009 2026-10-28 col902 | marcas=07:58 12:06 18:07 q=3 | ele=248 min | nos(pago_h)=600 min | motivo/turnos=None
EX dia_impar_nos_sem_numero: C004 2026-11-08 col281 | marcas=06:58 q=1 | ele=0 min | nos(pago_h)=None min | motivo/turnos=None
EX difere_4_marcacoes: C002 2026-10-21 col882 | marcas=10:50 15:08 16:07 22:50 q=4 | ele=661 min | nos(pago_h)=652 min | motivo/turnos=None
EX igual: C004 2026-10-24 col281 | marcas=19:01 07:00 q=2 | ele=719 min | nos(pago_h)=718 min | motivo/turnos=None
EX par_unico: C003 2026-10-22 col719 | marcas=07:48 19:48 q=2 | ele=720 min | nos(pago_h)=671 min | motivo/turnos=None
EX sem_marcacao_nos_sem_numero: C002 2026-10-22 col882 | marcas=- q=0 | ele=0 min | nos(pago_h)=None min | motivo/turnos=None
ITEM4 cod | col | escala_dele | nossa_escala | Trab_impresso_min | ele_4m | ele_4m+par | ele_4m+par+impar | casa_com | Not_impresso_min | ele_N4 | ele_N4+Npar | casa_com
P C002 | col882 | 12x36 | 12x36 · 11:00-23:00 | 9117 | 9243 | 9243 | 9243 | nenhum | 708 | 700 | 700 | nenhum
P C003 | col719 | 12x36 | 12x36 · 07:00-19:00 | 8678 | 0 | 9368 | 9368 | nenhum | 0 | 0 | 0 | N4,N4+Npar
P C004 | col281 | 12x36 | 12x36 · 19:00-07:00 | 8637 | 0 | 8678 | 8678 | nenhum | 5040 | 0 | 5040 | N4+Npar
P C006 | col351 | 12x36 | 12x36 · 19:00-07:00 | 9335 | 0 | 9373 | 9373 | nenhum | 5460 | 0 | 5460 | N4+Npar
P C007 | col307 | 6x1 | [nome] CRIS · 6x1 · Seg/Ter/Qua/Qui/Sex/Sab 07:00-16:00 | 9340 | 8175 | 9390 | 9390 | nenhum | 0 | 0 | 0 | N4,N4+Npar
P C009 | col902 | 5x2 | 5x2 · Seg/Ter/Qua/Qui/Sex 08:00-18:00 | 9778 | 9249 | 9249 | 9497 | nenhum | 0 | 0 | 0 | N4,N4+Npar
P C011 | col327 | personalizado | CTB [nome] · PERSO · flexivel (so horas) | 5292 | 5321 | 5321 | 5321 | nenhum | 791 | 691 | 691 | nenhum
P C013 | col117 | 12x36 | 12x36 · 10:30-22:30 | 7919 | 7921 | 7921 | 7921 | nenhum | 411 | 360 | 360 | nenhum
P C014 | col125 | 12x36 | 12x36 · 19:00-07:00 | 9322 | 0 | 9360 | 9360 | nenhum | 6240 | 0 | 5460 | nenhum
P C015 | col89 | 6x1 | ESCALA ARTES · 6x1 · 10:00-18:20 | 9892 | 8797 | 8949 | 9011 | nenhum | 0 | 0 | 0 | N4,N4+Npar
P C016 | col133 | 5x2 | v1 · 5x2 · Seg/Ter/Qua/Qui/Sex 08:00-17:00 | 8628 | 8706 | 8706 | 8706 | nenhum | 0 | 0 | 0 | N4,N4+Npar
P C017 | col32 | 12x36 | 12x36 · 07:00-19:00 | 7815 | 5264 | 5984 | 6704 | nenhum | 0 | 0 | 0 | N4,N4+Npar
P C020 | col712 | 5x2 | 5x2 · Seg/Ter/Qua/Qui/Sex 07:00-17:00 | 8878 | 8591 | 9021 | 9021 | nenhum | 0 | 0 | 0 | N4,N4+Npar
FIM_SONDA_34
rc_sombra=0
```
SAIDA_END

1. Sim, são horas decimais: ×60 converte para minutos; a origem divide minutos por 60 e o formatador arredonda minutos ([dia_pago.py:178](/home/ronald/codex_ws/fbc5c76ced33bf25943f3a5bc78ebff257d68c56/app/ponto/services/dia_pago.py:178), [cartao_pela_celula.py:174](/home/ronald/codex_ws/fbc5c76ced33bf25943f3a5bc78ebff257d68c56/app/relatorios/cartao_pela_celula.py:174), [cartao_pela_celula.py:176](/home/ronald/codex_ws/fbc5c76ced33bf25943f3a5bc78ebff257d68c56/app/relatorios/cartao_pela_celula.py:176), [pdf_espelho.py:667](/home/ronald/codex_ws/fbc5c76ced33bf25943f3a5bc78ebff257d68c56/app/relatorios/pdf_espelho.py:667)).
2. “Trabalhado” imprime `pago_h`; badges usam `total_trabalhadas`/`total_noturnas` ([pdf_espelho.py:865](/home/ronald/codex_ws/fbc5c76ced33bf25943f3a5bc78ebff257d68c56/app/relatorios/pdf_espelho.py:865), [pdf_espelho.py:902](/home/ronald/codex_ws/fbc5c76ced33bf25943f3a5bc78ebff257d68c56/app/relatorios/pdf_espelho.py:902), [pdf_espelho.py:981](/home/ronald/codex_ws/fbc5c76ced33bf25943f3a5bc78ebff257d68c56/app/relatorios/pdf_espelho.py:981), [pdf_espelho.py:766](/home/ronald/codex_ws/fbc5c76ced33bf25943f3a5bc78ebff257d68c56/app/relatorios/pdf_espelho.py:766), [pdf_espelho.py:782](/home/ronald/codex_ws/fbc5c76ced33bf25943f3a5bc78ebff257d68c56/app/relatorios/pdf_espelho.py:782)).
3. Item 3, auditor/pago em minutos: `difere_4_marcacoes` (96), C002 21/10: 661/652; `par_unico` (50), C003 22/10: 720/671; `igual` (48), C004 24/10: 719/718 — tolerância da classe [nao verificado].
4. Demais classes: `dia_impar` (6), C009 28/10: 248/600; `dia_impar_nos_sem_numero` (2), C004 08/11: 0/None; `sem_marcacao_nos_sem_numero` (149), C002 22/10: 0/None; exemplos com `motivo/turnos=None` [nao verificado].
5. Item 4, trabalhadas: nenhuma das três somas casa nos 13 casos, abrangendo 12x36, 6x1, 5x2 e personalizado; resultado da saída fornecida, sem reprodução independente [nao verificado].
6. Noturno 12x36: `N4+Npar` casa em C004 (5040) e C006 (5460); nenhuma casa em C002 (708 versus 700), C013 (411 versus 360) e C014 (6240 versus 5460); personalizado C011: nenhuma (791 versus 691) [nao verificado].
7. Noturno zero: `N4` e `N4+Npar` coincidem em 12x36 C003/C017, 6x1 C007/C015 e 5x2 C009/C016/C020 [nao verificado]; com zero, o badge é omitido ([pdf_espelho.py:782](/home/ronald/codex_ws/fbc5c76ced33bf25943f3a5bc78ebff257d68c56/app/relatorios/pdf_espelho.py:782)).
8. Sem nova execução: `sonda_34.py` não foi localizado na cópia [nao verificado]; o dado fornecido termina em `rc_sombra=0`, cuja execução independente permanece [nao verificado].


## 17/11 14:25 · sessao 75c84ea53fc0 · COMPLEMENTO-1-1410 · verificador · origem=Code

**AVAL-DEVOLVIDO — somente item 1: PROCEDE o achado de premissa divergente da cópia.** A retratação existente está em `_retratar`, com transição e resolvedor; o ponto indicado como consulta de competência encerra uma docstring (`app/chamados/reconciliador.py:504`, `app/chamados/reconciliador.py:524`, `app/chamados/reconciliador.py:537`, `app/chamados/reconciliador.py:576`).

As referências aos anexos abaixo contam as linhas do texto transcrito.

1. **As premissas conferem?**

   **(a) PROCEDE a divergência.** Nesta cópia, a unidade consulta `retencao_da_retratacao`, que trata irmã pendente e justificativa alheia; não é a guarda de competência descrita. `retratar` delega diretamente a `_retratar`. A sequência `retratar_com_motivo → consulta em :576` não corresponde ao código inspecionado (`app/chamados/reconciliador.py:438`, `app/chamados/reconciliador.py:519`, `app/chamados/reconciliador.py:545`, `app/chamados/reconciliador.py:576`).

   Existem consultas nomeáveis de competência, mas locais: `_dia_fora_do_alcance_c` consulta entregue, enquanto o ramo HIST conserva o caminho anterior quando está fora desse alcance. Isso não constitui a guarda geral pressuposta (`app/chamados/reconciliador.py:743`, `app/chamados/reconciliador.py:745`, `app/chamados/reconciliador.py:1593`).

   **(b) PROCEDE a insuficiência da identificação das escritas.** AST consegue enumerar funções, métodos e chamadas explícitas: há `bulk_update` em `gravar_lavras_em_lote`, `save` em `lavrar` e persistência da costura. Entretanto, reconhecer apenas esses verbos não identifica automaticamente os modelos, campos e limites do ato protegido; `carimbar`, por exemplo, constrói `campos` por ramos antes do `save` (`app/chamados/veredito_lavrado.py:240`, `app/chamados/veredito_lavrado.py:250`, `app/chamados/veredito_lavrado.py:275`, `app/chamados/services/fio.py:974`, `app/chamados/models.py:3238`, `app/chamados/models.py:3262`).

   **(c) PROCEDE o limite da promessa por AST.** O callback aparece como referência `ato=ch.reconciliar`, sendo executado como `ato()`; existe também a aresta implícita `save → post_save → resolvedor`. Um censo de nomes/chamadas não demonstra passagem obrigatória pela guarda antes de toda escrita (`app/chamados/reconciliador.py:524`, `app/chamados/services/fecho_runtime.py:160`, `app/chamados/services/fecho_runtime.py:165`, `app/chamados/models.py:296`, `app/chamados/signals.py:91`, `app/chamados/signals.py:117`).

   **NAO PROCEDE que alias e reexport, isoladamente, impeçam análise estática.** São explícitos nestes exemplos: `retratar as _retr` e reexport do resolvedor. Identificar essas referências continua sendo diferente de provar o fluxo da guarda (`app/ponto/management/commands/supra_juiz.py:258`, `app/ponto/management/commands/supra_juiz.py:286`, `app/chamados/models.py:4259`, `app/chamados/models.py:4260`).

2. **Colide com o vigente?**

   **PROCEDE a incompatibilidade com uma expectativa existente**, caso a cerca passe a impedir a reconciliação com pergunta entregue: há teste exigindo expressamente que essa pergunta **não segure** `reconciliar()`, e o código atual exclui entregue da lista de vivas (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:113`, `app/chamados/tests/test_o261e_f1_porta_encerramento.py:117`, `app/chamados/models.py:364`).

   **NAO PROCEDE que os selos existentes já satisfaçam o pedido ou que a sobreposição seja, sozinha, colisão.** Eles verificam portas de escrita e ordem de `_vivas_entre`; a pergunta sobre competência é diferente (`app/chamados/tests/test_selo_status_so_pelas_portas.py:1`, `app/chamados/tests/test_porta_veredito_pergunta.py:94`, `app/chamados/tests/test_o261e_f1_selo_encerramento.py:5`, `app/chamados/tests/test_o261e_f1_selo_encerramento.py:24`).

   **PROCEDE a ambiguidade entre allowlist vazia e exceção nomeada:** se a exceção dispensa a guarda e deixa verde, funciona como permissão de violação; se apenas registra o sítio mantendo vermelho, não contradiz a lista vazia. O item não especifica esse efeito (`comp1_aval.txt:2`). A proibição localizada de allowlist em L-106 trata de commits de documentação, não estabelece proibição universal de declarações nominais (`app/docs/LEIS.md:132`).

3. **Alguma condição não fecha no literal?**

   **PROCEDE.** A matriz já exige consulta obrigatória **antes da primeira escrita funcional do ato**; portanto, mera presença da guarda em algum ancestral não basta. Falta fechar como o selo reconhece essa obrigação através das unidades encadeadas e callbacks (`matriz.r:17`, `app/chamados/services/becos.py:53`, `app/chamados/services/fecho_runtime.py:165`).

   Também não fica fechado se “16º chamador” inclui entrada indireta: wrappers podem apenas delegar, sem chamada local à guarda. Exigir guarda no mesmo corpo e aceitar proteção transitiva são critérios diferentes (`comp1_aval.txt:2`, `app/chamados/reconciliador.py:545`, `app/chamados/reconciliador.py:547`).

   Há ainda distinção entre atribuição em memória e persistência: `lavrar(..., salvar=False)` altera campos, mas quem salva é o chamador. Isso interfere no marco da “primeira escrita” (`app/chamados/veredito_lavrado.py:270`, `app/chamados/veredito_lavrado.py:274`, `app/chamados/services/fio.py:973`, `app/chamados/services/fio.py:974`).

4. **Toca exportada, dinheiro ou `!` fora do declarado?**

   **NAO PROCEDE afirmar esse alcance adicional pelo texto:** o complemento exclui porta do export e produção. Não autoriza apply financeiro nem mudança do gravado exportado (`comp1_aval.txt:5`, `CLAUDE.md:265`, `CLAUDE.md:406`, `CLAUDE.md:411`). Efeito material da implementação futura, selo vermelho com 16º chamador e mordida do mutante: **[nao verificado]**.

## 17/11 14:24 · sessao ae657b1ef1bd · CENSO-FECHA-DE-VERDADE · verificador · origem=Code

**AVAL-DEVOLVIDO, por item: PROCEDE em 1, 3, 4, 5, RED, CONTRATO e CONDUTA**, pelas lacunas abaixo. Item 2, RESERVA e PROIBIDO não apresentam colisão necessária com a regra vigente (`bin/disjuntor.py:1522`, `1698`, `2306`; `CLAUDE.md:394`).

“PROCEDE” abaixo significa achado contra o fechamento do aval. As reprovações B2/F2, os briefs efetivamente aprovados, os construtores atualmente abertos e o custo de 10–20 minutos permanecem **[nao verificado]**.

**1 — caminhos**

- **Q1 — Premissas:** conferem os endereços do juiz e da porta. Hoje o juiz exige leitores/escritores com referência, decisões e perguntas cortadas; não interpreta caminhos, ponto único ou lista fechada (`bin/disjuntor.py:1522`, `1565`, `1698`).
- **Q2 — Colisão:** **NAO PROCEDE** como incompatibilidade necessária: é aperto do juiz existente. Entretanto, um brief conforme hoje pode faltar aos novos requisitos: a própria fixture `CONFORME` não contém caminhos nem ataques (`bin/tests/test_censo_antes_da_vaga.sh:54`, `86`).
- **Q3 — Literal:** **PROCEDE.** Contar N linhas não comprova “TODO caminho”. Também ficam sem definição explícita N=0, duplicatas e correspondência entre destinos e “lista fechada: N sitios” **[nao verificado]**. A referência atual apenas casa aparência `arquivo.ext:numero`; não verifica existência, conteúdo ou relação chamador→sítio (`bin/disjuntor.py:1501`, `1566`).
- **Q4 — Exportada/dinheiro/!:** **NAO PROCEDE** como toque direto: este juiz lê o brief e retorna motivo; não escreve dado de negócio (`bin/disjuntor.py:1525`, `1577`).

**2 — nenhuma ocorrência de “nao verificado”**

- **Q1 — Premissas:** hoje vale o **primeiro** título que case `^(#{1,6})\s*CENSO\b`, com caixa literal. O bloco termina no próximo título de nível igual ou superior; subtítulos permanecem dentro (`bin/disjuntor.py:1496`, `1530`, `1538`).
- **Q2 — Colisão:** **NAO PROCEDE** como contradição necessária. O aperto passa a recusar conteúdo antes aceito; hoje não existe teste dessa expressão no juiz (`bin/disjuntor.py:1543`, `1577`).
- **Q3 — Literal:** **NAO PROCEDE** a objeção de capturar prosa fora do bloco, preservado o recorte existente. Dentro dele, inclusive subtítulos, a proibição é literal. “Citando a linha” ainda não existe: o parser conserva texto, mas descarta os números físicos ao montar `bloco` (`bin/disjuntor.py:1537`, `1544`).
- **Q4 — Exportada/dinheiro/!:** **NAO PROCEDE** como toque direto; é validação textual do brief (`bin/disjuntor.py:1525`, `1577`).

**3 — ataques cruzados**

- **Q1 — Premissas:** o juiz atual lê somente o brief; não busca ataques nem interpreta respostas a eles (`bin/disjuntor.py:1524`, `1547`, `1577`).
- **Q2 — Colisão:** **NAO PROCEDE** contra a revisão vigente: ela já exige classes anteriores de reprovação e escritores/leitores, inclusive fora do diff (`CLAUDE.md:373`, `379`).
- **Q3 — Literal:** **PROCEDE.** `ataques cruzados: 0 (busca: grep...)` declara um comando; não prova execução, resultado, universo pesquisado ou ausência de ataques. Igualmente, escrever “respondido por” não comprova que a resposta enfrenta o ataque. Essa comprovação não é feita pelo juiz atual (`bin/disjuntor.py:1525`, `1577`); buscas e respostas concretas: **[nao verificado]**.
- **Q4 — Exportada/dinheiro/!:** **NAO PROCEDE** como toque direto. Registrar uma resposta no brief não amplia autorização sobre negócio ou competência exportada (`CLAUDE.md:265`, `268`).

**4 — complemento é abertura e exige brief próprio**

- **Q1 — Premissas:** **PROCEDE a lacuna.** A porta recebe obra, texto, via e raiz; não recebe marcador de complemento, volta ou construtor já aberto. Um brief citado de outra pasta sob `logs/` pode satisfazer a abertura sem existir brief na pasta da obra (`bin/disjuntor.py:1698`, `1607`, `1619`, `1715`).
- **Q2 — Colisão:** **PROCEDE o risco concreto de revalidar material antigo.** O coletor reúne **todos** os `brief*.md` próprios e citados, e qualquer recusado barra a chamada. Assim, brief novo conforme não salva brief antigo da mãe também coletado. Aprovação anterior não é autorização duradoura nessa decisão (`bin/disjuntor.py:1604`, `1619`, `1703`, `1717`). Obras reais atingidas: **[nao verificado]**. Exigir brief próprio não elimina, por si, a competência técnica da sessão para encaixar complemento (`CLAUDE.md:276`, `278`).
- **Q3 — Literal:** **PROCEDE.** O reconhecimento foi remetido ao contrato, mas o contrato fornecido não distingue complemento de volta **[nao verificado em arquivo local]**. Continuar um processo aberto não chama novamente este portão; uma nova invocação de construtor passa por ele, sem sinal de continuidade (`bin/disjuntor.py:1851`, `1855`; `bin/codex_pergunta.sh:247`). A ressalva sobre obras abertas não fica automaticamente assegurada para suas próximas chamadas.
- **Q4 — Exportada/dinheiro/!:** **NAO PROCEDE** como autorização adicional. Complemento continua submetido ao escopo literal e aos casos que exigem `!` (`CLAUDE.md:265`, `268`).

**5 — fila 1, LATERAL-ESTRUTURA, tela e instrumento**

- **Q1 — Premissas:** fila 1, na conduta, é contrato 20/placar L-099 (`CLAUDE.md:291`). No código, `_fila1_viva` lê os IDs andáveis do bloco OBRAS; o hook exclui fechados/parados pela coluna de estado, e `_pertence_fila1` resolve identidade e ambiguidades (`bin/disjuntor.py:1630`, `1634`, `1688`; `bin/hook_stop_fila1.py:180`, `250`). **Não há classificação desses baldes na decisão atual**: fora da fila 1, a porta retorna imediatamente (`bin/disjuntor.py:1707`).
- **Q2 — Colisão:** **PROCEDE a interseção não resolvida.** Tela e fila 1 não são categorias necessariamente exclusivas: há registro histórico explícito de “TELA” que é “FILA 1 AGORA”; isso não prova obra aberta hoje (`app/docs/BACKLOG.md:1248`).
- **Q3 — Literal/costura:** **PROCEDE.** O CLI do wrapper também decide “fora da fila 1” e retorna **antes** de `_censo_nega`. Ampliar somente a porta deixa Codex lateral escapando por esse retorno (`bin/disjuntor.py:2306`, `2311`, `2315`). Além disso, “tela como está” preserva a cobrança atual quando ela pertence à fila 1; R6, lido como isenção universal de tela, diz outra coisa (`bin/disjuntor.py:1707`).
- **Q4 — Exportada/dinheiro/!:** **NAO PROCEDE** como toque direto; ampliar universo do portão não amplia aval de negócio (`CLAUDE.md:265`, `268`).

**RED — R1 a R6**

- **Q1 — Premissas:** existem testes de juiz, CLI, pretool e costura. A fixture conforme atual passa sem os novos campos; o caso de tela usa especificamente `CONGELADA, FILA 2` (`bin/tests/test_censo_antes_da_vaga.sh:44`, `54`, `86`, `252`).
- **Q2 — Colisão:** **NAO PROCEDE** contra exigir casos e mutantes: o selo vigente já distingue testes do juiz e testes da costura (`bin/tests/test_censo_antes_da_vaga.sh:13`, `18`).
- **Q3 — Literal:** **PROCEDE em R4/R6.** R4 depende da distinção complemento/volta ainda ausente; R6 não resolve tela∩fila1. R5 também precisa continuar satisfazendo os campos antigos: o aval diz “também”, e o juiz atual os exige (`bin/disjuntor.py:1698`, `1707`, `1565`). Nenhum resultado dos novos R1–R6 está provado **[nao verificado]**.
- **Q4 — Exportada/dinheiro/!:** **NAO PROCEDE** pelo desenho dos testes: o selo existente usa árvore de mentira, não BACKLOG de produção (`bin/tests/test_censo_antes_da_vaga.sh:20`, `36`).

**CONTRATO**

- **Q1 — Premissas:** os dois chamadores conferem. Agent construtor chama `_censo_nega` no pretool; há também chamada na recuperação de exceção (`bin/disjuntor.py:1851`, `1855`, `1763`). Codex passa pelo wrapper→CLI→porta, antes da vaga; o pretool deliberadamente não julga essa via (`bin/codex_pergunta.sh:247`; `bin/disjuntor.py:1832`, `2315`).
- **Q2 — Colisão:** **NAO PROCEDE** contra contrato prévio único e pouso próprio: ambos já são exigidos (`CLAUDE.md:327`, `394`; `app/docs/PISTA.md:170`).
- **Q3 — Fechamento:** **PROCEDE parcialmente.** Brief ilegível e exceção recusam no ramo alcançado (`bin/disjuntor.py:1527`, `1733`). PAREI provado exige wrapper vivo da mesma raiz, obra exata e papel `censo|leitura`; não basta publicar a linha (`bin/hook_stop_fila1.py:734`, `763`). Porém faltam o reconhecimento de complemento e a medida prometida: hoje ESTADO publica negativas **agregadas**, não por motivo (`bin/disjuntor.py:2104`, `2145`, `2164`). O contrato normativo também pede contador com esperado; o esperado da nova medida não foi explicitado **[nao verificado]** (`app/docs/PISTA.md:173`).
- **Q4 — Exportada/dinheiro/!:** **NAO PROCEDE** como toque direto. Desligamento já tem chave própria atribuída a Ronald; não é allowlist por obra (`bin/disjuntor.py:124`, `209`, `2291`).

**CONDUTA**

- **Q1 — Premissas:** conferir manualmente e registrar cabe à sessão; leitura/censo pertence ao despacho Codex (`CLAUDE.md:285`, `358`). A existência de publicação `CENSO-MANUAL` para qualquer obra concreta é **[nao verificado]**.
- **Q2 — Colisão:** **PROCEDE.** “Qualquer construtor ou complemento” inclui tela/instrumento, enquanto o item 5 os preserva; essa diferença está no literal fornecido **[nao verificado em arquivo local]**. O código atual não oferece classificação que resolva a diferença (`bin/disjuntor.py:1698`, `1707`).
- **Q3 — Limite:** **PROCEDE quanto à evidência.** A linha manual de contagens não prova mapas ou respostas. O Codex de leitura recebe cópia por `git archive`, e `logs/` é ignorado; os dados vivos chegam por anexos/sonda raspados, não pela cópia (`bin/codex_pergunta.sh:315`, `331`, `334`, `425`; `.gitignore:53`). Portanto não pode conferir buscas nos logs que não recebeu **[nao verificado]**.
- **Q4 — Exportada/dinheiro/!:** **NAO PROCEDE** como autorização de escrita; conferir e publicar censo não remove os limites do aval (`CLAUDE.md:265`, `268`).

**RESERVA**

- **Q1 — Premissas:** **NAO PROCEDE** como desvio de papel: ataque adversarial, censo e mapa de escritores/leitores cabem ao Codex. A lista vigente de classes já está descrita (`CLAUDE.md:358`, `367`, `373`).
- **Q2 — Colisão:** **NAO PROCEDE** como abertura necessária de construtor: leitura pode ocorrer sem vaga; a própria preparação registrada separa leitura/desenho de construção (`app/docs/BACKLOG.md:794`).
- **Q3 — Limite:** ataques efetivos, respostas e mapas completos de F4/F7/F3/F9 são **[nao verificado]**. A cópia registra preparação de F2/F4/F7, mas não substitui seus anexos (`app/docs/BACKLOG.md:822`; `bin/codex_pergunta.sh:331`).
- **Q4 — Exportada/dinheiro/!:** **NAO PROCEDE** para a leitura reservada. Eventual escrita resultante continua sob os limites de negócio (`CLAUDE.md:265`, `268`).

**PROIBIDO**

- **Q1 — Premissas:** **NAO PROCEDE** afirmar necessidade de novo juiz ou allowlist. Já existem juiz, porta e coletor compartilhados; a aprovação percorre todos os briefs coletados (`bin/disjuntor.py:1522`, `1698`, `1717`).
- **Q2 — Colisão:** **NAO PROCEDE**: a proibição de allowlist é compatível com o corte vigente. A ressalva temporal não autoriza inventar lista de obras dispensadas (`app/docs/CORTES.md:300`; `bin/disjuntor.py:1703`).
- **Q3 — Limite:** a existência de retorno “FORA” no CLI é uma decisão de alcance, não um segundo parser do brief; permanece, porém, a costura que impede ampliar somente `_censo_nega` (`bin/disjuntor.py:2311`, `2315`).
- **Q4 — Exportada/dinheiro/!:** **NAO PROCEDE** como permissão para contornar `!`; esses limites permanecem expressos (`CLAUDE.md:265`, `268`).

**QUEM ESCREVE / QUEM LÊ**

| Arquivo/campos | Quem escreve | Quem lê |
|---|---|---|
| `logs/<OBRA>/brief*.md`: leitores, escritores, decisões, perguntas | Autor concreto de cada brief: **[nao verificado]**. O portão não escreve o brief; lê e solicita correção (`bin/disjuntor.py:1525`, `1728`). | `_briefs_do_censo` coleta; `censo_do_brief` julga (`bin/disjuntor.py:1607`, `1718`). |
| Novos campos: caminhos, ponto único/lista fechada, ataques/respostas | Produtor concreto e autoridade para atestar completude: **[nao verificado]**. | Leitura futura pretendida no mesmo juiz; interpretação desses campos ainda ausente (`bin/disjuntor.py:1498`, `1547`). |
| BACKLOG: ID, descrição, estado e marcador | Sessão registra OBRAS; gerador preserva bloco/marcador escritos à mão. `backlog_estado.py` atualiza estado (`CLAUDE.md:436`; `bin/gerar_backlog.py:103`, `113`; `bin/backlog_estado.py:58`). | `_fila1_viva`→`_itens_andaveis`→`_pertence_fila1`; não lê balde semântico (`bin/disjuntor.py:1634`, `1688`; `bin/hook_stop_fila1.py:250`). |
| `chamadas.log`: hora, obra, papel, hash12, pid, `ini`, pergunta raspada | Wrapper (`bin/codex_pergunta.sh:266`, `308`). | `_linhas_chamadas` extrai hora/obra/papel/chave; `_censo_24h` casa abertura por obra+pid+hash (`bin/codex_limite_estado.py:63`; `bin/disjuntor.py:2126`, `2134`). |
| `chamadas.log`: `fim`, thread_id, rc, duração | Trap `_saida` do wrapper (`bin/codex_pergunta.sh:269`, `272`). | Parser, últimas chamadas e procedência do ESTADO (`bin/codex_limite_estado.py:66`, `82`, `92`; `bin/gerar_codex_estado.py:176`). |
| `logs/sessao/disjuntor.log`: aprovação/negativa/motivo | `trilha`, chamada pela porta e pelo CLI (`bin/disjuntor.py:193`, `1724`, `2317`). | `_censo_24h`; esta é a fonte das negativas. Recusa anterior à vaga não alcança o `ini` de `chamadas.log` (`bin/disjuntor.py:2090`, `2102`; `bin/codex_pergunta.sh:247`, `308`). |
| RELATO: `PAREI: censo-<obra> \| espera Codex` | Mecanismo de negativa via `esteira_vigia.no_topo` (`bin/disjuntor.py:302`, `324`). | Hook de parada verifica literal e produtor vivo; não relê os novos campos do brief (`bin/hook_stop_fila1.py:773`, `783`). |

O host do wrapper consulta a raiz viva; o Codex leitor trabalha na cópia e recebe anexos. São acessos distintos (`bin/codex_pergunta.sh:247`, `315`, `331`, `425`). Nenhuma identificação de construtor hoje aberto ou brief de produção foi inferida dessas referências **[nao verificado]**.

## 17/11 14:24 · sessao bd950d29c346 · - · - · origem=Ronald (tmux)

- **PROCEDE — existem selos sobre os mesmos escritores, mas com outra pergunta.** O selo de status fiscaliza quem escreve `status_local`/`fechada_em`, inclusive `save`, `update`, `bulk_update`, aliases e SQL (`app/chamados/tests/test_selo_status_so_pelas_portas.py:1`, `:10`, `:36`, `:340`). O selo do veredito fixa `lavrar` e `gravar_lavras_em_lote` e suas escritas nominais (`app/chamados/tests/test_porta_veredito_pergunta.py:94`, `:123`). Nenhum desses trechos prova passagem pela guarda de competência.

- **PROCEDE — existe tripwire de ordem, mas é outro predicado.** O tripwire existente exige `_vivas_entre` e `raise` antes da primeira atribuição a `status_local`, apenas em `_gravar_estado`; possui mutantes de chamada removida, `raise` removido e escrita antecipada (`app/chamados/tests/test_o261e_f1_selo_encerramento.py:10`, `:24`, `:64`, `:78`). O teste funcional existente exige que pergunta de competência ENTREGUE **não impeça** `reconciliar()` (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:113`). Portanto, não equivale à cerca pedida.

- **NAO PROCEDE — sobreposição de arquivos, sozinha, não demonstra duplicação de selo.** A advertência contra terceiro varredor trata expressamente da mesma pergunta “escrita de status fora da porta”; o próprio arquivo acrescenta uma pergunta diferente sobre ordem da guarda (`app/chamados/tests/test_o261e_f1_selo_encerramento.py:5`). A nova pergunta de competência não está demonstrada por esses selos.

- **PROCEDE — distinguir declaração de porta e exceção que perdoa violação.** O gate da família exige `ALLOWLIST == ()` e zero escrita fora de porta (`app/chamados/tests/test_chokepoint_chamado_gate.py:371`). Entretanto, portas nominais com motivos continuam declaradas no selo de status (`app/chamados/tests/test_selo_status_so_pelas_portas.py:36`). Uma lista dos 15 pontos, por si, não contradiz allowlist vazia; uma exceção que faça violação passar precisa ser confrontada com o literal do anexo [nao verificado quanto ao mecanismo ainda inexistente].

- **NAO PROCEDE — proibição universal de listas nominais não está comprovada.** A frase `proibido allowlist` em `CLAUDE.md:334` e `app/docs/LEIS.md:132` refere-se especificamente a commits só de documentação. Há inclusive exceção declarada e medida em selo de resolução (`app/chamados/tests/test_selo_resolucao_um_escritor.py:77`).

- **Limite já declarado na casa:** o selo de status admite que AST não resolve receptor/tipo genérico nem campos externos em todos os casos (`app/chamados/tests/test_selo_status_so_pelas_portas.py:17`). Logo, usá-lo como prova de completude irrestrita de chamadores seria indevido.

- A busca local não localizou o aval `RESPOSTAS-1711-1410`/`COMPLEMENTO-1-1410`; vigência adicional além do anexo fornecido: **[nao verificado]**.

## 17/11 14:23 · sessao 19d8c964bebe · - · - · origem=Ronald (tmux)

- **PROCEDE — a premissa de identidade da cópia falha.** O corpo atual é `_retratar`, com retenção em `app/chamados/reconciliador.py:519`, callback de reconciliação em `:524` e resolvedor em `:537`; `retratar` delega a ele em `:547`. A guarda `retratar_com_motivo → _retratacao_toca_entregue` descrita no anexo não corresponde a esse corpo. O reexport real está em `app/chamados/models.py:4260`; o resolvedor é definido em `app/chamados/services/disputa_emissao.py:1887`.

- **PROCEDE — AST sintático não equivale à propriedade semântica prometida.** O callback é passado como valor (`ato=ch.reconciliar`), em `app/chamados/reconciliador.py:524`, e executado como `ato()`, em `app/chamados/services/fecho_runtime.py:160` e `:165`. Há ainda ligação implícita `save → post_save → resolvedor`: `app/chamados/models.py:296`; `app/chamados/signals.py:91`; `:117`. Um censo de chamadas por nome não demonstra que a guarda domina todas as escritas desses caminhos.

- **NÃO PROCEDE — alias/reexport, isoladamente, tornariam o censo impossível.** São resolvíveis estaticamente nestes exemplos: `retratar as _retr`, em `app/ponto/management/commands/supra_juiz.py:258`, utilizado em `:286`; reexport do resolvedor em `app/chamados/models.py:4259`, `:4260`. Essa resolução identifica referências; não certifica a execução obrigatória de guarda antes da escrita, conforme a execução indireta em `app/chamados/services/fecho_runtime.py:165`.

- **PROCEDE — “qualquer ancestral” não determina passagem obrigatória anterior.** O callback pode executar por dois ramos, em `app/chamados/services/fecho_runtime.py:160` e `:165`; a reconciliação pode retornar sem transição, em `app/chamados/models.py:653`, ou escrever em `:655`. A existência de um ancestral com consulta não estabelece, sozinha, o caminho efetivamente executado nem a propagação da recusa.

- **Exportada/dinheiro/!** Existe consulta local de entregue na reabertura do credor, em `app/chamados/services/disputa_emissao.py:1857`, com veto por `motivo` antes de `credor.reabrir()`, em `:1867` e `:1873`; ela não é uma guarda universal dos quinze sítios. K6 é `competencia_entregue`, em `app/folha/export.py:556`; a composição por dia é `competencia_do_dia_entregue`, em `app/chamados/services/vigia_fio.py:785`. O limite vigente exige `!` para tocar gravado de exportada, em `CLAUDE.md:265`, e distingue apply financeiro de competência aberta, em `CLAUDE.md:406`. Impacto material futuro desse selo em exportada/dinheiro: **[nao verificado]**.

## 17/11 14:23 · sessao bbc39757284b · - · - · origem=Ronald (tmux)

- **1(a) — NÃO PROCEDE a premissa nesta cópia.** A unidade existente é `_retratar`, definida em `app/chamados/reconciliador.py:504`: consulta `retencao_da_retratacao` em `:519`, transiciona em `:524`, pode salvar justificativa em `:536` e chama o resolvedor em `:537`. `retratar` apenas delega (`app/chamados/reconciliador.py:545`, `:547`). A linha `:576` encerra a docstring de E2; não consulta competência (`app/chamados/reconciliador.py:550`, `:576`). A busca global de Python em `app` não encontrou os símbolos `retratar_com_motivo`, `_retratacao_toca_entregue` e `perguntas_alcancadas_pela_retratacao`; a matriz não corresponde integralmente à cópia inspecionada (`app/chamados/reconciliador.py:504`, `:545`, `:550`).

- **Existem predicados nomeáveis de competência, mas não equivalem à guarda geral descrita.** `_dia_fora_do_alcance_c` consulta `competencia_do_dia_entregue` e a tranca (`app/chamados/reconciliador.py:743`, `:745`, `:748`); no HIST, estar fora do alcance conserva o percurso anterior (`app/chamados/reconciliador.py:1593`, `:1594`, `:1608`). O helper `reabrir_credor_de_pergunta_viva` verifica competência da carga, porém somente considera dias diferentes de `None` (`app/chamados/services/disputa_emissao.py:1827`, `:1857`, `:1858`, `:1859`).

- **1(b) — PROCEDE listar os símbolos; NÃO PROCEDE confundir isso com identificar todas as escritas protegidas.** Há escritores explícitos para estado (`app/chamados/models.py:201`, `:296`), carimbo (`app/chamados/models.py:3207`, `:3213`, `:3262`), costura (`app/chamados/services/fio.py:972`, `:974`) e lote (`app/chamados/veredito_lavrado.py:240`, `:250`). Entretanto, `campos` é construído por ramos e passado ao `save` (`app/chamados/models.py:3238`, `:3243`, `:3247`, `:3261`, `:3262`); reconhecer somente `.save/.update/.bulk_update` não identifica modelo, campos nem unidade funcional desses atos (`app/chamados/models.py:296`, `:3262`).

- **3 — PROCEDE o achado de ambiguidade entre atribuição em memória e primeira persistência.** `lavrar(p, salvar=False)` atribui os três campos sem chamar `save`; a persistência fica no chamador (`app/chamados/veredito_lavrado.py:254`, `:270`, `:274`, `:275`; `app/chamados/services/fio.py:973`, `:974`; `app/chamados/models.py:3260`, `:3262`). Uma regra de presença/ordem de chamadas precisa distinguir esses comportamentos para sustentar o literal “antes da primeira escrita” [nao verificado].

- **A lista de 15 tem um alvo ausente e 14 nomes existentes.** As posições atuais são: `fechar_beco` (`app/chamados/services/becos.py:36`), resolvedor (`app/chamados/services/disputa_emissao.py:1887`), `reconciliar` (`app/chamados/models.py:642`), `transicionar` (`app/chamados/models.py:128`), `encerrar` (`app/chamados/models.py:1669`), `encerrar_por_competencia_trancada` (`app/chamados/models.py:1686`), `reabrir` (`app/chamados/models.py:1409`), `DisputaSupervisao.fechar` (`app/chamados/models.py:1936`), E2 (`app/chamados/reconciliador.py:550`), `reatar_pergunta_orfa` (`app/chamados/reconciliador.py:1534`), `encerrar_orfas_pela_lampada` (`app/chamados/reconciliador.py:1113`), `reapontar_resolvedora` (`app/chamados/services/fio.py:851`), `lavrar` (`app/chamados/veredito_lavrado.py:254`) e `gravar_lavras_em_lote` (`app/chamados/veredito_lavrado.py:240`).

## 17/11 14:23 · sessao 10ac03e7bce0 · CONFRONTO-PILOTO · sonda · origem=Code

Transcrição do dado fornecido, sem reexecução: `sonda_fp.py` não foi localizado na cópia e não há acesso à sombra. [nao verificado]

```text
SAIDA_BEGIN
sonda_leitura: sonda_fp.py [9b60a7ed9c0c0dce] no cpuset 4-7, lendo sombra como leitor (LEITURA)

emp2 colabs=741 com_batida_na_janela=401 codigos=11
emp3 colabs=163 com_batida_na_janela=101 codigos=5
emp4 colabs=35 com_batida_na_janela=20 codigos=4
M C001 | emp2 | nao mapeado | - | 1 pre-candidatos, 0 casam dia a dia
M C002 | emp2 | MAPEADO | col882 | 1 pre-candidatos, 1 casam dia a dia
M C003 | emp2 | MAPEADO | col719 | 1 pre-candidatos, 1 casam dia a dia
M C004 | emp2 | MAPEADO | col281 | 1 pre-candidatos, 1 casam dia a dia
M C005 | emp2 | nao mapeado | - | 0 pre-candidatos, 0 casam dia a dia
M C006 | emp2 | MAPEADO | col351 | 1 pre-candidatos, 1 casam dia a dia
M C007 | emp2 | MAPEADO | col307 | 1 pre-candidatos, 1 casam dia a dia
M C008 | emp2 | nao mapeado | - | 1 pre-candidatos, 0 casam dia a dia
M C009 | emp2 | MAPEADO | col902 | 1 pre-candidatos, 1 casam dia a dia
M C010 | emp2 | nao mapeado | - | 0 pre-candidatos, 0 casam dia a dia
M C011 | emp2 | MAPEADO | col327 | 1 pre-candidatos, 1 casam dia a dia
M C012 | emp3 | nao mapeado | - | 1 pre-candidatos, 0 casam dia a dia
M C013 | emp3 | MAPEADO | col117 | 1 pre-candidatos, 1 casam dia a dia
M C014 | emp3 | MAPEADO | col125 | 1 pre-candidatos, 1 casam dia a dia
M C015 | emp3 | MAPEADO | col89 | 1 pre-candidatos, 1 casam dia a dia
M C016 | emp3 | MAPEADO | col133 | 1 pre-candidatos, 1 casam dia a dia
M C017 | emp4 | MAPEADO | col32 | 1 pre-candidatos, 1 casam dia a dia
M C018 | emp4 | nao mapeado | - | 0 pre-candidatos, 0 casam dia a dia
M C019 | emp4 | nao mapeado | - | 0 pre-candidatos, 0 casam dia a dia
M C020 | emp4 | MAPEADO | col712 | 1 pre-candidatos, 1 casam dia a dia
ESTADO: mapearam 13 de 20
FIM_SONDA_FP
rc_sombra=0
SAIDA_END
```

O dado fornecido reporta **13/20 casamentos únicos**, nenhum ambíguo, `rc_sombra=0` e nenhuma ocorrência de `ERRO_COLETA`; sem validação independente. [nao verificado]  
C001, C008 e C012: cada um tem **0 candidatos finais**, após 1 pré-candidato; falharam na comparação diária, mas a divergência concreta não foi fornecida. [nao verificado]  
C005, C010, C018 e C019: cada um tem **0 pré-candidatos e 0 finais**; exclusão já no pré-filtro, cuja composição e causa concreta não estão demonstradas. [nao verificado]  
Dia: `str(data)` de um `date` coincide com `AAAA-MM-DD`; o PDF usa `dia['data']`, exibido como `%d/%m` + dia da semana. A implementação da comparação na sonda está [nao verificado]. [pdf_espelho.py:868](app/relatorios/pdf_espelho.py:868)  
Hora: o PDF usa `timezone.localtime(b.timestamp).strftime('%H:%M')`, somente para `orfa=False`; espúrias aparecem separadas como `*HH:MM`. [pdf_espelho.py:876](app/relatorios/pdf_espelho.py:876), [pdf_espelho.py:881](app/relatorios/pdf_espelho.py:881), [pdf_espelho.py:885](app/relatorios/pdf_espelho.py:885)  
Atribuir os sete insucessos a espúrias do auditor, multiconjunto de HH:MM ou erro de coleta, e confirmar que a sonda reproduz esses critérios: [nao verificado].

## 17/11 14:22 · sessao 64f2c72c2154 · - · - · origem=Ronald (tmux)

Achados para consolidar:

- **Item 5 — PROCEDE:** a porta atual decide somente pertinência da obra à fila 1 viva, por ID e estado das células do bloco OBRAS; não lê classe tela, instrumento ou balde: `bin/disjuntor.py:1707`, `bin/disjuntor.py:1688`, `bin/hook_stop_fila1.py:231`, `bin/hook_stop_fila1.py:250`. O literal novo não resolve a obra simultaneamente fila 1 e tela [nao verificado — anexo aval.txt]. Essa interseção não é impossível na documentação: há item histórico escrito “a outra metade e a TELA, e ela e a FILA 1 AGORA”, já fechado: `app/docs/BACKLOG.md:1248`. Não afirmar construtor desse item hoje aberto [nao verificado].

- **Item 4 / CONTRATO — PROCEDE:** o aval remete a identificação de complemento ao contrato, mas suas cinco cláusulas não dão um campo, evento, ID ou critério para distinguir complemento de volta [nao verificado — anexo aval.txt]. A API atual recebe obra/texto/via, sem parâmetro de complemento ou volta: `bin/disjuntor.py:1698`; o coletor reúne todos os briefs próprios e citados: `bin/disjuntor.py:1607`. A regra vigente permite à sessão decidir tecnicamente quando obra entra como complemento: `CLAUDE.md:276`, `CLAUDE.md:278`; exigir brief próprio não revoga necessariamente esse poder.

- **CONDUTA — PROCEDE:** “qualquer construtor ou complemento”, exigindo itens 1–4, abrange também tela e instrumento, enquanto o alcance do item 5 diz deixá-los como estão [nao verificado — anexo aval.txt]. O código existente não dá um significado mais estreito a “qualquer”: `bin/disjuntor.py:1698`, `bin/disjuntor.py:1737`.

- **PROIBIDO — NAO PROCEDE, quanto a alegar conflito obrigatório:** endurecer o juiz existente e manter a porta existente é compatível com as funções únicas atuais: `bin/disjuntor.py:1522`, `bin/disjuntor.py:1698`; o contrato prévio único e pouso separado de instrumento já são exigidos: `CLAUDE.md:394`, `CLAUDE.md:327`, `app/docs/PISTA.md:170`. Não há autorização para uma allowlist implícita de obras já aprovadas [nao verificado — anexo aval.txt]. `CENSO APROVADO` atual prova brief aprovado, não vaga aberta: `bin/disjuntor.py:1703`.

- **Exportada/dinheiro/! — NAO PROCEDE como efeito direto declarado:** os pontos de mudança indicados são o parser do brief e a porta de abertura, que leem arquivos e decidem recusa: `bin/disjuntor.py:1525`, `bin/disjuntor.py:1713`, `bin/disjuntor.py:1728`. Isso não amplia autorização de negócio: `CLAUDE.md:265`, `CLAUDE.md:268`; a competência exportada segue exigindo `!`: `CLAUDE.md:411`. Implementação futura além desses pontos [nao verificado].

- **RESERVA — NAO PROCEDE quanto a colisão necessária:** leitura adversarial e mapa de escritores/leitores já pertencem ao despacho Codex: `CLAUDE.md:358`, `CLAUDE.md:359`; a lista vigente de classes inclui segundo juiz, segundo escritor, relógio, derivação paralela, selo sem mordida, ordem de trava e pouso sem pré-requisito: `CLAUDE.md:373`. Estado atual dos ataques F4/F7/F3/F9 e sua resposta no brief [nao verificado]. [nao verificado]

Regra vigente e documentação:

- Fila 1 é contrato 20 e placar L-099 pela conduta: `CLAUDE.md:291`; o código usa os itens andáveis do bloco OBRAS: `bin/disjuntor.py:1634`. UI fila 2 tem recorte próprio: `CLAUDE.md:312`; instrumento em `bin/` tem pouso próprio: `CLAUDE.md:327`.
- O registro existente do CENSO-ANTES-DA-VAGA descreve brief `logs/<obra>/brief*.md`, perguntas sem corte, recusa e PAREI: `app/docs/BACKLOG.md:802`. Defeitos já registrados do instrumento ficam na linha seguinte: `app/docs/BACKLOG.md:803`.
- CORTES.md declara ser gerado do JSON: `app/docs/CORTES.md:3`; corte ESPINHA-ANTES-DA-UI mantém prioridade estrutural e proíbe allowlist: `app/docs/CORTES.md:300`.
- Contrato precisa declarar quem chama, espera, falha/limite, medida e desligamento; procedente muda o contrato antes do código: `app/docs/PISTA.md:170`, `app/docs/PISTA.md:174`.

**BACKLOG: quem escreve / quem lê**

- **Escrevem:** sessão registra prompt em OBRAS no mesmo turno, autorizada pela rotina: `CLAUDE.md:285`, `CLAUDE.md:436`. O bloco OBRAS e o marcador ORDEM-VIVA são escritos à mão: `bin/gerar_backlog.py:103`, `bin/gerar_backlog.py:113`; `backlog_estado.py` recebe ID/texto e grava coluna de estado `[3]`: `bin/backlog_estado.py:29`, `bin/backlog_estado.py:58`. O gerador escreve o arquivo preservando esse bloco: `bin/gerar_backlog.py:119`, `bin/gerar_backlog.py:138`.
- **Leem para o portão:** `disjuntor._fila1_viva`, que usa o juiz do hook: `bin/disjuntor.py:1630`, `bin/disjuntor.py:1634`; `_itens_andaveis` lê ID, fechamento e parada: `bin/hook_stop_fila1.py:244`, `bin/hook_stop_fila1.py:250`.
- **Outros leitores comprovados:** fabricante: `bin/fabricante_alvo.py:238`; contador de interrompidas: `bin/interrompidas_estado.py:89`; publicação do relato: `bin/relato.sh:130`.

## 17/11 14:22 · sessao 35317b459d4b · - · - · origem=Ronald (tmux)

Achados do wrapper/contrato, somente por leitura:

- **Item 5 — PROCEDE:** ampliar apenas `_censo_nega` não cobre Codex lateral: `main_censo` calcula pertença à fila 1 e retorna 0 antes da porta quando está fora; só depois chama `_censo_nega` (`bin/disjuntor.py:2306`, `bin/disjuntor.py:2311`, `bin/disjuntor.py:2315`). O wrapper efetivamente chama esse CLI antes de prender a vaga, passando raiz do repositório vivo, obra, pid, hash, pergunta e anexos (`bin/codex_pergunta.sh:241`, `bin/codex_pergunta.sh:247`, `bin/codex_pergunta.sh:252`).

- **Item 4 — PROCEDE:** hoje o coletor é união de todos os briefs da pasta da obra e dos citados; não exige que exista brief próprio quando um citado já serve (`bin/disjuntor.py:1604`, `bin/disjuntor.py:1607`, `bin/disjuntor.py:1619`). O wrapper fornece somente obra/papel/WT/pergunta/anexos, sem campo de complemento ou volta de construção (`bin/codex_pergunta.sh:57`, `bin/codex_pergunta.sh:81`, `bin/codex_pergunta.sh:247`). Não há nessa chamada informação que distinga complemento de nova chamada para obra aberta [nao verificado quanto a outros chamadores].

- **CONTRATO — PROCEDE na medida:** o ESTADO atual conta negativas totais, não por motivo: incrementa `negados` ao ler `NEGOU`, retornando um único número (`bin/disjuntor.py:2102`, `bin/disjuntor.py:2104`, `bin/disjuntor.py:2145`). Seu texto publica “aberturas negadas por censo nas 24 h”, também sem motivos (`bin/disjuntor.py:2164`). As negativas vêm de `logs/sessao/disjuntor.log`, não de `chamadas.log` (`bin/disjuntor.py:2090`). O censo antecede a gravação do `ini`; chamada recusada ali não alcança esse registro (`bin/codex_pergunta.sh:247`, `bin/codex_pergunta.sh:308`).

- **CONTRATO — NAO PROCEDE quanto a falta da porta no wrapper:** ela existe e qualquer rc diferente de zero recusa; raiz usada para BACKLOG/brief/RELATO é a raiz do repositório vivo, derivada do git-common-dir (`bin/codex_pergunta.sh:72`, `bin/codex_pergunta.sh:247`). Brief ilegível retorna motivo de recusa (`bin/disjuntor.py:1524`, `bin/disjuntor.py:1528`); falha ao ler pergunta/anexo vira negativa no CLI (`bin/disjuntor.py:2295`, `bin/disjuntor.py:2308`).

- **CONTRATO — NAO PROCEDE quanto a “PAREI provado como hoje”:** a prova existente exige wrapper vivo desta raiz, mesmo uid, obra exata e papel `censo|leitura`; quando acaba, deixa de provar espera (`bin/hook_stop_fila1.py:734`, `bin/hook_stop_fila1.py:744`, `bin/hook_stop_fila1.py:763`, `bin/hook_stop_fila1.py:770`). O hook não lê o conteúdo do brief para essa prova; verifica processo e ambiente (`bin/hook_stop_fila1.py:747`, `bin/hook_stop_fila1.py:759`).

- **CONDUTA/RESERVA — PROCEDE quanto ao acesso do leitor:** os quatro papéis de leitura do Codex recebem cópia feita por `git archive` e executam em `$WS`, sob read-only (`bin/codex_pergunta.sh:312`, `bin/codex_pergunta.sh:315`, `bin/codex_pergunta.sh:425`). `logs/` é ignorado (`.gitignore:53`). Portanto esse leitor não recebe os logs vivos por esse mecanismo; recebe os anexos concatenados e raspados no prompt (`bin/codex_pergunta.sh:331`, `bin/codex_pergunta.sh:334`, `bin/codex_pergunta.sh:360`). Exigir que o Codex confirme a busca nos logs sem fornecer seu resultado deixa essa confirmação [nao verificado]. O portão em host tem acesso à raiz viva por argumento; não confundir com o leitor Codex (`bin/codex_pergunta.sh:247`).

- **Item 1/RED — prova de compatibilidade antiga:** a fixture atualmente chamada `CONFORME` contém apenas CENSO, leitores, escritores, decisões e perguntas de lei 0; não contém caminhos/ataques (`bin/tests/test_censo_antes_da_vaga.sh:54`). A forma atual aprova quando esses requisitos fecham (`bin/disjuntor.py:1565`, `bin/disjuntor.py:1577`). Logo o aperto deliberadamente torna insuficiente um brief conforme ao contrato antigo; ocorrência de brief de produção nessas condições [nao verificado].

Quem escreve/lê:

| Arquivo/campo | Escritor | Leitor |
|---|---|---|
| `logs/<OBRA>/brief*.md` e novos campos caminhos/ponto/ataques | Autor concreto não demonstrado na cópia [nao verificado]; o aval anexo atribui conferência à sessão [nao verificado como arquivo:linha] | Coletor `_briefs_do_censo`, depois `censo_do_brief` (`bin/disjuntor.py:1607`, `bin/disjuntor.py:1718`) |
| `BACKLOG` | Registrar BACKLOG é ação pré-aprovada da sessão/Code (`CLAUDE.md:282`, `CLAUDE.md:285`); prompt que pede obra deve virar item no bloco OBRAS (`CLAUDE.md:436`) | `main_censo` consulta fila 1 antes da porta (`bin/disjuntor.py:2306`); `_censo_nega` recebe as mesmas células (`bin/disjuntor.py:2315`) |
| `chamadas.log`: hora, obra, papel, hash12, pid, `ini`, pergunta raspada | Wrapper, antes da chamada (`bin/codex_pergunta.sh:266`, `bin/codex_pergunta.sh:308`) | Parser `_linhas_chamadas`, que extrai t/obra/papel/chave, ignorando texto da pergunta (`bin/codex_limite_estado.py:49`, `bin/codex_limite_estado.py:63`) |
| `chamadas.log`: `fim`, thread_id, rc, duração | Trap `_saida` do wrapper (`bin/codex_pergunta.sh:269`, `bin/codex_pergunta.sh:272`) | Mesmo parser; últimas chamadas/rc e procedência por thread (`bin/codex_limite_estado.py:66`, `bin/codex_limite_estado.py:82`, `bin/codex_limite_estado.py:92`); `gerar_codex_estado` usa procedência (`bin/gerar_codex_estado.py:176`) |
| Casamento `ini` com censo aprovado | Wrapper escreve ini; trilha de aprovação vem da porta (`bin/codex_pergunta.sh:308`, `bin/disjuntor.py:1722`) | `_censo_24h`, mesma obra+pid+hash em janela de 600 s (`bin/disjuntor.py:2052`, `bin/disjuntor.py:2134`) |

A mudança de instrumento em `bin/` não pode ser entregue pelo papel construtor deste wrapper: ele proíbe `bin/` e tudo fora de app (`bin/codex_pergunta.sh:128`, `bin/codex_pergunta.sh:134`); isso é compatível com instrumento/pouso próprio (`CLAUDE.md:327`). Nenhum apply de dinheiro ou exportada decorre desses caminhos do portão; operações de negócio e estado de produção citados no anexo continuam [nao verificado].

## 17/11 14:22 · sessao 12b949535d78 · E4C-F2 · verificador · origem=Code

1. **O INSERT antigo grava `False` nas duas colunas.** Os defaults estão no banco nas migrations [0054:19](./app/chamados/migrations/0054_decisao_da_ressuscitacao.py#L19) e [0055:26](./app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py#L26).

   **Nenhuma das duas constraints citadas rejeita esse INSERT, para qualquer `ato`.** Ambas têm a forma `ato=... OR campo=False`; com `False`, a segunda condição sempre satisfaz o check ([0054:24](./app/chamados/migrations/0054_decisao_da_ressuscitacao.py#L24); [0055:34](./app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py#L34)).

2. **O backfill continua correto para as linhas alcançadas pelo UPDATE.** Ele atribui explicitamente `True` às RESPOSTAS com motivo; `db_default=False` não impede essa atualização ([0055:12](./app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py#L12)).

   A ordem apresentada é coerente: adiciona a coluna, atualiza, verifica constraints diferidas antes do próximo `ALTER TABLE` e adiciona o check. O código já emprega esse mecanismo na 0051 ([0055:22–35](./app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py#L22); [0051:101–104](./app/chamados/migrations/0051_decisao_pergunta.py#L101)). Execução efetiva dessa sequência: **[nao verificado]**.

   **A ressalva é temporal:** o backfill roda uma vez; INSERTs antigos posteriores recebem `False`, mesmo quando têm `ato='resposta'` e motivo preenchido — combinação que o próprio backfill classificaria como `True` ([0055:12](./app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py#L12), [0055:26–28](./app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py#L26)).

3. **Os dois campos e respectivos checks estão consistentes por inspeção.** Modelo e migrations declaram igualmente `default=False, db_default=False`, com as mesmas condições e nomes de constraints ([models.py:4178](./app/chamados/models.py#L4178), [4185](./app/chamados/models.py#L4185), [4203–4207](./app/chamados/models.py#L4203); [0054:19–25](./app/chamados/migrations/0054_decisao_da_ressuscitacao.py#L19); [0055:26–35](./app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py#L26)).

   `makemigrations` executado e sem diferenças, inclusive fora desses campos: **[nao verificado]**.

4. **O teste morde o default do banco, por construção.** O SQL omite as duas colunas, usa `RETURNING` e exige `(False, False)`; não passa pelo default Python. O segundo teste exige os dois defaults no catálogo ([teste:25–32](./app/chamados/tests/test_f2_db_default_das_colunas_novas.py#L25); [teste:34–39](./app/chamados/tests/test_f2_db_default_das_colunas_novas.py#L34)). Num banco reconstruído sem os `db_default` das migrations, o primeiro INSERT deve falhar por `NOT NULL`; remover apenas os defaults do modelo, mantendo o banco já migrado, não testa essa regressão ([0054:19](./app/chamados/migrations/0054_decisao_da_ressuscitacao.py#L19); [0055:26](./app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py#L26); [teste:25](./app/chamados/tests/test_f2_db_default_das_colunas_novas.py#L25)).

   **Não deixa a linha órfã para o teardown:** `_Desfaz` sai do `atomic`, desfazendo o INSERT antes da asserção; somente essa exceção é capturada ([teste:21–32](./app/chamados/tests/test_f2_db_default_das_colunas_novas.py#L21)). Isso pressupõe a FK diferida declarada no comentário; configuração real da FK e execução de `check_constraints`: **[nao verificado]** ([teste:24](./app/chamados/tests/test_f2_db_default_das_colunas_novas.py#L24)). A reversão de eventual avanço da sequência de IDs também não é verificada pelo teste ([teste:25](./app/chamados/tests/test_f2_db_default_das_colunas_novas.py#L25)).

5. **Escritores e leitores identificados:**

   - `da_ressuscitacao`: a ressuscitação escreve `True`; o predicado exige REABERTURA e marca verdadeira; a carga da tarefa usa esse predicado para excluir a linha. `False` conserva o tratamento comum ([models.py:1695](./app/chamados/models.py#L1695), [4214](./app/chamados/models.py#L4214); [tarefa_do_dia.py:426](./app/chamados/services/tarefa_do_dia.py#L426)).
   - `resposta_mantem_validacao`: resposta humana escreve `False`; automática, carimbo pela trilha e foto da projeção escrevem `True`; o backfill escreve `True` no histórico selecionado ([disputa_emissao.py:1534](./app/chamados/services/disputa_emissao.py#L1534), [1688](./app/chamados/services/disputa_emissao.py#L1688), [1710](./app/chamados/services/disputa_emissao.py#L1710); [models.py:4253](./app/chamados/models.py#L4253); [0055:12](./app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py#L12)).
   - A porta genérica instancia os dicionários e insere em lote; os leitores Python e SQL interpretam RESPOSTA com `False` como ato que desfaz validação ([models.py:4328](./app/chamados/models.py#L4328), [4281](./app/chamados/models.py#L4281), [3547](./app/chamados/models.py#L3547), [4311–4312](./app/chamados/models.py#L4311)).

   **Há, portanto, um contraexemplo semântico para a janela:** validação em T0; RESPOSTA automática ou pela trilha em T1>T0, inserida pelo código antigo após o backfill, omitindo a coluna. Ela recebe `False`, e a leitura nova desfaz a validação. Porém esses caminhos preservam `validada_em` e exigem `True` na cópia atual ([disputa_emissao.py:1685–1688](./app/chamados/services/disputa_emissao.py#L1685), [1707–1710](./app/chamados/services/disputa_emissao.py#L1707); [models.py:4281](./app/chamados/models.py#L4281), [4311–4312](./app/chamados/models.py#L4311)). Ocorrência desse caso em produção e corpos exatos desses escritores antigos: **[nao verificado]**.

**REPROVADO** para o pouso irrestrito descrito: o delta resolve a aceitação dos INSERTs, mas o backfill único não preserva a semântica das RESPOSTAS antigas posteriores que deveriam manter validação ([0055:12,28](./app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py#L12); [models.py:4281](./app/chamados/models.py#L4281)).

_6594 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
