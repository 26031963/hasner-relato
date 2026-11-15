# MAPA DOS RIOS — ChamadoColaborador NASCE

Caminhos relativos a `app/`, exceto os iniciados por `bin/`. HASH informado: `a0bf6e78e9a2e7d67f9efee9b6a14141e33e3a5f`; correspondencia com um commit Git local: [nao verificado].

## 1. RESUMO

1. **2 portas de nascimento:** `abrir`, com dedup, e `criar`, sem dedup interno. (`chamados/models.py:880`, `chamados/models.py:1101`)
2. **38 sitios operacionais diretos:** 19 chamam `abrir`, 19 chamam `criar`; inventario individual abaixo, com arquivo:linha.
3. **1 sitio adicional fora de `tests/`:** o smoke; seu `handle` exige banco com `test` no nome e desfaz a transacao. (`chamados/management/commands/smoke_chamados.py:33`, `chamados/management/commands/smoke_chamados.py:44`, `chamados/management/commands/smoke_chamados.py:124`)
4. **2 chegadas de nascimento nos fluxos operacionais:** `aberto` e `registrado`; a natureza escolhe EMITIR ou REGISTRAR. (`chamados/models.py:1094`, `chamados/catalogo/motor.py:130`)
5. `abrir` serializa a busca e o nascimento sob trava; a identidade vem do lookup do chamador, sem unicidade declarada no modelo. (`chamados/models.py:107`, `chamados/models.py:949`, `chamados/models.py:953`)
6. **9 nascentes de `criar` fazem pre-dedup externo**, com criterios diferentes, discriminados no inventario abaixo. (`ponto/utils.py:31`, `ponto/utils.py:94`, `ponto/views.py:284`, `ponto/services/triagem_batida.py:102`, `ponto/services/triagem_batida.py:325`, `ponto/services/triagem_batida.py:452`, `ponto/services/geofence.py:353`, `ponto/management/commands/processar_alertas_avancados.py:100`, `chamados/services/abertura.py:165`)
7. **1 funcao escreve a trilha do nascimento**, com antes `None` e depois status/modulo/via; o registro e best-effort. (`chamados/models.py:1256`, `core/observ.py:105`)
8. **L2: 0 escritores de runtime encontrados fora das portas; 4 sitios historicos nas migrations**, excluidas pelo selo de runtime. (`chamados/tests/test_selo_status_so_pelas_portas.py:15`, `chamados/migrations/0038_a3_registrado_backfill.py:10`, `chamados/migrations/0038_a3_registrado_backfill.py:15`, `chamados/migrations/0039_a3_via_backfill.py:24`, `chamados/migrations/0039_a3_via_backfill.py:33`)

## 2. TABELA DE TRANSICOES

A tabela conta o nascimento efetivo da entidade. Pegar existente e recusar nascimento nao criam uma transicao; `abrir` pode encaminhar um existente encerrado ao sitio de renascimento. (`chamados/models.py:953`, `chamados/models.py:969`, `chamados/models.py:1012`, `chamados/models.py:1017`)

| # | estado de partida -> estado de chegada | juiz (arquivo:linha) | operacao (funcao) | escritor (arquivo:linha) | o que persiste (campos/modelos/trilha) | leitores (arquivo:linha) | prova (teste ou selo que morde, arquivo::nome, ou "nenhum") |
|---|---|---|---|---|---|---|---|
| N1 | inexistente -> aberto | Natureza acionavel: `chamados/catalogo/natureza.py:36`; escolha EMITIR: `chamados/models.py:1094`; motor: `chamados/catalogo/motor.py:130`; absolvimento pela celula: `chamados/models.py:1150`; veto da regua: `chamados/models.py:1173`. Dedup de `abrir`: `chamados/models.py:953`, `chamados/models.py:974`. | `abrir` nas 19 nascentes A; `criar` nas nascentes B, exceto B2/B8/B9; B15/B17 dependem do modulo. Chamadas e argumentos no inventario abaixo. | `abrir`: `chamados/models.py:1018`; `criar`: `chamados/models.py:1142`; trilha: `chamados/models.py:1265`; auditoria: `core/observ.py:134`. | `ChamadoColaborador`, `status_local=aberto`, campos recebidos e celula resolvida pela porta; datas automaticas: `chamados/models.py:67`; resolucao da celula: `chamados/models.py:1000`, `chamados/models.py:1113`. `LogAuditoria.nascimento_chamado`, antes `None`, depois status/modulo/via: `chamados/models.py:1265`. Para modulo de turno com celula, `EventoCelula`: `chamados/models.py:1024`, `chamados/models.py:1144`. | Painel: `chamados/services/painel.py:213`; receiver do cartorio: `chamados/signals.py:189`; receiver de isencao: `chamados/signals.py:284`; auditoria generica: `core/views.py:705`; contador por cron: `chamados/services/vigia_loop.py:86`. | `chamados/tests/test_a3_registrado.py::test_cobranca_continua_aberto` — `chamados/tests/test_a3_registrado.py:34`; `chamados/tests/test_chamado_nasce_com_trilha.py::test_abrir_deixa_uma_linha_so_no_nascimento_real` — `chamados/tests/test_chamado_nasce_com_trilha.py:38`. |
| N2 | inexistente -> registrado | Natureza de auditoria: `chamados/catalogo/natureza.py:18`; escolha REGISTRAR: `chamados/models.py:1094`; motor: `chamados/catalogo/motor.py:133`; mesmas guardas de celula/regua: `chamados/models.py:1118`, `chamados/models.py:1122`. | `criar`: B2, B8, B9, B15 quando `gps_ausente`, B17 quando recebe modulo de auditoria. `abrir` tambem comporta REGISTRAR, mas nenhuma das 19 nascentes A pede modulo de auditoria; catalogo A abaixo e `chamados/models.py:982`. | `criar`: `chamados/models.py:1142`; capacidade de `abrir`: `chamados/models.py:985`, `chamados/models.py:1018`; trilha: `chamados/models.py:1265`. | Mesmo modelo e trilha, com `status_local=registrado`: `chamados/models.py:1110`, `chamados/models.py:1142`, `chamados/models.py:1268`. `registrado` pertence a VIVOS, mas nao a COBRANCA: `chamados/catalogo/motor.py:24`, `chamados/catalogo/motor.py:29`. | Contador proprio de registrados: `chamados/services/fila.py:126`; natureza/dono: `chamados/catalogo/natureza.py:44`; receivers: `chamados/signals.py:189`, `chamados/signals.py:284`; auditoria: `core/views.py:705`. | `chamados/tests/test_a3_registrado.py::test_criar_auditoria_nasce_registrado` — `chamados/tests/test_a3_registrado.py:15`; `chamados/tests/test_a3_registrado.py::test_abrir_auditoria_nasce_registrado_e_dedupa_por_vivos` — `chamados/tests/test_a3_registrado.py:21`. |

### 2a. Nascentes diretas de `abrir` — 19

Em todas, o lookup inclui o colaborador. A porta consulta exatamente esse lookup; os filtros de status sao retirados apenas na segunda busca, destinada ao filho agrupado sob pai vivo. (`chamados/models.py:953`, `chamados/models.py:974`)

| ID | nascente / funcao | lookup alem do colaborador |
|---|---|---|
| A1 | `ponto/management/commands/detectar_par_relampago.py:368` — `handle` | `par_relampago` + `contexto_json__data_turno`; constante em `ponto/management/commands/detectar_par_relampago.py:17`. |
| A2 | `ponto/management/commands/reconciliar_grade.py:507` — `_emitir_furo` | `batida_ausente` + `contexto_json__data_turno`; constante em `ponto/management/commands/reconciliar_grade.py:54`. |
| A3 | `ponto/management/commands/detectar_ausencias.py:191` — `handle` | `sem_previsao_escala` + `status_local__in=VIVOS`. |
| A4 | `ponto/management/commands/detectar_ausencias.py:272` — `handle` | `ancora_limbo`. |
| A5 | `ponto/management/commands/detectar_ausencias.py:511` — `_chamado_dia_sem_grade` | `sem_previsao_escala` + subcategoria `dia_sem_grade` + VIVOS. |
| A6 | `ponto/management/commands/detectar_ausencias.py:694` — `_abrir_chamado` | `batida_ausente` + `contexto_json__data_turno`; constante em `ponto/management/commands/detectar_ausencias.py:29`. |
| A7 | `ponto/management/commands/emitir_turno_aberto_retroativo.py:151` — `handle` | `turno_aberto_24h` + `contexto_json__data_turno`. |
| A8 | `ponto/management/commands/processar_alertas_turno.py:335` — `_abrir_chamado_critico` | `turno_aberto` + `contexto_json__data_turno`. |
| A9 | `ponto/services/cluster_espurio.py:196` — `reconciliar_colab` | `cluster_batida_espuria` + `contexto_json__data_turno`; constante em `ponto/services/cluster_espurio.py:11`. |
| A10 | `ponto/services/ausencia.py:722` — `abrir_chamado_ausencia_colab` | `ausencia_colab` + `contexto_json__ausencia_id`. |
| A11 | `ponto/views.py:3052` — `ping_geo` | `ping_geo_fora` + `criado_em__date`. |
| A12 | `escala/regua_defesa.py:51` — `vetar_cobranca_se_degenerada` | `regua_degenerada` + `contexto_json__tipo_escala_id`. |
| A13 | `api/views.py:1966` — `api_justificativas` | `justificativa_colab` + categoria `justificativa` + VIVOS. |
| A14 | `chamados/reconciliador.py:911` — `reemitir_chamado_do_dia` | `batida_ausente` + `contexto_json__data_turno`; chave em `chamados/reconciliador.py:836`. |
| A15 | `chamados/services/disputa_emissao.py:568` — `_abrir_ou_agregar_disputa` | `disputa_supervisao` + categoria homonima + VIVOS. |
| A16 | `chamados/services/disputa_emissao.py:1104` — `criar_disputa_a_partir_de_chamados_orfaos` | `disputa_supervisao_manual` + categoria `disputa_supervisao` + subcategoria `orfao_14h` + VIVOS. |
| A17 | `chamados/services/disputa_emissao.py:1339` — `criar_disputa_ancora_de_chamado` | Mesmo modulo/categoria de A16, subcategoria `ancora_ausente` + VIVOS. |
| A18 | `chamados/services/adesao.py:75` — `sincronizar_adesao` | `sem_previsao_escala` + subcategoria `nunca_bateu`. |
| A19 | `chamados/views.py:973` — `abrir_chamado_rapido` | `mensagem_dp` + VIVOS. |

### 2b. Nascentes diretas de `criar` — 19 operacionais + 1 smoke

`criar` nao pesquisa chamado existente. O estado default e substituido pelo destino do motor; um `status` explicito diferente de `aberto` permanece como recebido. (`chamados/models.py:1101`, `chamados/models.py:1107`, `chamados/models.py:1110`, `chamados/models.py:1142`)

| ID | nascente / funcao | modulo, chegada e pre-dedup externo |
|---|---|---|
| B1 | `colaboradores/services/vinculo.py:410` — `_executar_aceite` | `escala_colaborador` -> aberto; encerra anteriores e cria novo: `colaboradores/services/vinculo.py:401`. |
| B2 | `colaboradores/services/desligamento.py:195` — `executar_desligamento` | `desligamento_revisao` -> registrado; modulo em `colaboradores/services/desligamento.py:208`. |
| B3 | `colaboradores/views.py:1981` — `solicitar_beneficio` | `beneficio` -> aberto. |
| B4 | `colaboradores/views.py:2367` — `agenda_enviar_aceite` | `agenda_dia` -> aberto. |
| B5 | `ponto/management/commands/processar_alertas_avancados.py:121` — `handle` | `turno_aberto_24h` -> aberto. Busca primeiro `turno_aberto` vivo da data; depois `turno_aberto_24h` em NAO_FECHADOS, sem data nessa segunda busca: `ponto/management/commands/processar_alertas_avancados.py:100`, `ponto/management/commands/processar_alertas_avancados.py:105`. |
| B6 | `ponto/utils.py:40` — `bloquear_se_sem_posto` | `colaborador_sem_posto` -> aberto; pre-dedup por colaborador/modulo/data de criacao: `ponto/utils.py:31`. |
| B7 | `ponto/utils.py:103` — `bloquear_se_em_ferias` | `colaborador_em_ferias` -> aberto; pre-dedup por colaborador/modulo/data de criacao: `ponto/utils.py:94`. |
| B8 | `api/views.py:717` — `api_bater_ponto` | `batida_fora_escala_leve` -> registrado; ramo de desvio leve: `api/views.py:705`, modulo em `api/views.py:725`. |
| B9 | `api/views_core.py:711` — `api_bater_ponto` | `batida_fora_escala_leve` -> registrado; ramo de desvio leve: `api/views_core.py:696`, modulo em `api/views_core.py:719`. |
| B10 | `api/views.py:2170` — `api_abrir_chamado` | `app_colaborador` -> aberto; modulo em `api/views.py:2179`. |
| B11 | `ponto/services/justificativa.py:291` — `rejeitar_justificativa_dp` | `justificativa` -> aberto; origem precisa estar pendente: `ponto/services/justificativa.py:277`. |
| B12 | `ponto/services/triagem_batida.py:107` — `triagem_bater_ponto` | `acesso_bloqueado` -> aberto; pre-dedup por colaborador/modulo/VIVOS: `ponto/services/triagem_batida.py:102`. |
| B13 | `ponto/services/triagem_batida.py:336` — `ramo_solicitar_anuencia` | `batida_fora_horario` -> aberto; pre-dedup por colaborador/modulo/VIVOS: `ponto/services/triagem_batida.py:325`. |
| B14 | `ponto/services/triagem_batida.py:459` — `_bloqueio_horario_clt` | `batida_fora_horario` -> aberto; pre-dedup por colaborador/modulo/janela de 14h, sem filtro de estado: `ponto/services/triagem_batida.py:452`. |
| B15 | `ponto/services/geofence.py:361` — `verificar_geofence` | `gps_ausente` -> registrado; `gps_fora_geofence` -> aberto; escolha em `ponto/services/geofence.py:344`. Pre-dedup por colaborador/subcategoria/VIVOS: `ponto/services/geofence.py:353`. |
| B16 | `chamados/services/regularizacao_ext.py:247` — `criar_regularizacao_externa` | `regularizacao_externa` -> aberto. |
| B17 | `chamados/services/abertura.py:47` — `abrir_chamado_local` | Modulo variavel -> aberto ou registrado; a tela aceita chaves de MOTIVOS: `chamados/views.py:170`, `chamados/views.py:174`; a categoria pode substituir o modulo: `chamados/services/abertura.py:30`. |
| B18 | `chamados/services/abertura.py:172` — `abrir_chamado_para_colaborador` | Modulo limitado aos tipos do catalogo -> aberto: `chamados/services/abertura.py:148`, `chamados/catalogo/modulos.py:306`. Pre-dedup sob trava por colaborador/modulo/VIVOS/assunto/descricao/prazo/autor: `chamados/services/abertura.py:164`, `chamados/services/abertura.py:165`. |
| B19 | `ponto/views.py:294` — `bater_ponto` | `entrada_bloqueada_turno_aberto` -> aberto; pre-dedup por colaborador/modulo/VIVOS: `ponto/views.py:284`. |
| B20 | `chamados/management/commands/smoke_chamados.py:124` — `_novo` | Passa `status` explicitamente. O laco cria origens aberto/em_analise/resolvido/registrado; fechado e criado separadamente: `chamados/management/commands/smoke_chamados.py:131`, `chamados/management/commands/smoke_chamados.py:147`, `chamados/catalogo/motor.py:99`. O `handle` restringe o banco e faz rollback: `chamados/management/commands/smoke_chamados.py:33`, `chamados/management/commands/smoke_chamados.py:44`. |

As chegadas dos sitios B decorrem da natureza do modulo e da conversao do default na porta. (`chamados/catalogo/natureza.py:18`, `chamados/catalogo/natureza.py:36`, `chamados/models.py:1110`)

### 2c. Entradas indiretas, commands e crons

Estes sitios alimentam as nascentes A/B; nao representam novos escritores de nascimento. As cadeias abaixo terminam nos escritores de `abrir`/`criar`. (`chamados/models.py:1018`, `chamados/models.py:1142`)

- **Formularios:** `chamados/views.py:197` -> B17; `chamados/views.py:136` -> B18; ausencia em `ponto/views.py:2579` e `api/views.py:1842` -> A10; regularizacao em `colaboradores/views.py:2987` e `chamados/services/regularizacao_ext.py:319` -> B16.
- **Batida:** `ponto/views.py:353` -> ramo que chama bloqueio horario em `ponto/services/triagem_batida.py:657` e geofence em `ponto/services/triagem_batida.py:753`; APIs chamam geofence em `api/views.py:781` e `api/views_core.py:775`.
- **Sem posto/ferias:** `ponto/services/triagem_batida.py:84` -> B6; `chamados/detector_anomalias.py:116` -> B7; detector chamado em `api/views.py:434` e `api/views_core.py:420`.
- **Regua degenerada:** `ponto/management/commands/detectar_ausencias.py:601` e `ponto/management/commands/reconciliar_grade.py:495` -> A12.
- **Cartorio/eventos:** cluster em `ponto/services/cartorio.py:1042` -> A9; emissor em `ponto/services/cartorio.py:1791` e `ponto/services/cartorio.py:1829` -> A6; fabrica em `ponto/services/cartorio.py:1045`, com chamada de emissao em `chamados/services/fabrica_da_celula.py:172`.
- **Reemissao:** `chamados/reconciliador.py:1622` -> A14, pelo reconciliador em `chamados/reconciliador.py:1434`, alimentado por eventos em `ponto/signals.py:215` e `chamados/signals.py:370`.
- **Cobrar dia:** `chamados/views_cobrar.py:325` -> `chamados/services/cobrar_dia.py:41`; repasse de orfaos em `chamados/services/cobrar_dia.py:49`.

| command ou script | caminho para nascimento / modo de escrita |
|---|---|
| `emitir_furo_retroativo` | Chama o emissor A6 em `ponto/management/commands/emitir_furo_retroativo.py:154`. |
| `processar_cartorio` | Chama julgamento com emissor em `ponto/management/commands/processar_cartorio.py:65`; chega a A6 pelos sitios do cartorio citados acima. |
| `vigia_de_hora` | `ponto/management/commands/vigia_de_hora.py:28` -> julgamento em `ponto/services/vigia_de_hora.py:131`. |
| `detectar_cluster_espurio --apply` | Guarda de escrita em `ponto/management/commands/detectar_cluster_espurio.py:69`; chama A9 em `ponto/management/commands/detectar_cluster_espurio.py:81`. |
| `apurar_furos_diarios` | Chama adesao/A18 em `escala/management/commands/apurar_furos_diarios.py:17`. |
| `efetivar_desligamentos_agendados --apply` | Chama desligamento/B2 em `colaboradores/management/commands/efetivar_desligamentos_agendados.py:57`. |
| `cobrar_furos_sem_cobranca --apply` | Guarda em `ponto/management/commands/cobrar_furos_sem_cobranca.py:27`; chama servico em `ponto/management/commands/cobrar_furos_sem_cobranca.py:30`, que julga em `ponto/services/furos_sem_cobranca.py:128`. |
| `celula_veredito_velho --aplicar` | Julgamento em `ponto/management/commands/celula_veredito_velho.py:144`; sem aplicar, rollback em `ponto/management/commands/celula_veredito_velho.py:156`. |
| `curar_residuo_bug115 --apply` | Chama A5 em `ponto/management/commands/curar_residuo_bug115.py:106`; outro ramo julga em `ponto/management/commands/curar_residuo_bug115.py:146`; guarda em `ponto/management/commands/curar_residuo_bug115.py:91`. |
| `rejulgar_ausencia_em_folga --apply` | Guardas em `ponto/management/commands/rejulgar_ausencia_em_folga.py:70` e `ponto/management/commands/rejulgar_ausencia_em_folga.py:73`; julgamento em `ponto/management/commands/rejulgar_ausencia_em_folga.py:78`. |
| `marcar_foto_ausente_retro --apply` | Guarda em `ponto/management/commands/marcar_foto_ausente_retro.py:89`; chama emissao em `ponto/management/commands/marcar_foto_ausente_retro.py:96`, chegando a A15 por `chamados/services/disputa_emissao.py:662`. |
| `reconciliar_grade --apply` | Guarda em `ponto/management/commands/reconciliar_grade.py:250`; chama A2 em `ponto/management/commands/reconciliar_grade.py:254` e A15 pelo sitio `ponto/management/commands/reconciliar_grade.py:424`. |
| Commands diretos que tambem repassam orfaos | `detectar_ausencias`: `ponto/management/commands/detectar_ausencias.py:803`; `processar_alertas_turno`: `ponto/management/commands/processar_alertas_turno.py:385`; `processar_alertas_avancados`: `ponto/management/commands/processar_alertas_avancados.py:159` -> A16. |
| Smoke, entrada indireta | Chama A15 em `chamados/management/commands/smoke_chamados.py:100`, alem de B20; sujeito ao mesmo rollback em `chamados/management/commands/smoke_chamados.py:44`. |
| `bin/r5_idempotencia_frota.py` | Chama `processar_cartorio(apply=True)` em `bin/r5_idempotencia_frota.py:109`, nas rodadas de `bin/r5_idempotencia_frota.py:125` e `bin/r5_idempotencia_frota.py:127`; destino real da execucao: [nao verificado]. |

**Agendamento presente no codigo:** alertas de turno a cada 5 minutos, avancados a cada 30, ausencias a cada 5, vigia de hora a cada 10; desligamentos com `--apply`; par relampago com `--apply --retratar`; cartorio com `--apply`; foto retroativa com `--apply`; apuracao de furos encadeada ao cartorio. (`config/crons.py:362`, `config/crons.py:363`, `config/crons.py:364`, `config/crons.py:367`, `config/crons.py:440`, `config/crons.py:605`, `config/crons.py:616`, `config/crons.py:686`, `config/crons.py:1527`)

**Agendamentos que nao emitem nesse modo:** grade sem `--apply`; cluster com `--alarme`. Retroativo de turno e declarado como ato, sem horario. (`config/crons.py:462`, `config/crons.py:598`, `config/crons.py:952`, `config/crons.py:961`)

### 2d. Dedup, recusas e trilha

- **Encontrou pelo lookup:** devolve o existente com `False`; se esta em ENCERRADOS, chama o sitio de renascimento antes de devolver. Isso nao e um novo nascimento. (`chamados/models.py:953`, `chamados/models.py:968`, `chamados/models.py:971`)
- **Filho agrupado sob pai vivo:** a busca sem filtros de status pode satisfazer o dedup; devolve o filho com `False`. (`chamados/models.py:974`, `chamados/models.py:978`, `chamados/models.py:979`)
- **Celula absolve:** nao cria chamado; `abrir` devolve `(None, False)`, `criar` devolve `None`. A trilha da recusa deduplica por celula/modulo/veredito, sem criterio temporal nessa consulta. (`chamados/models.py:1009`, `chamados/models.py:1118`, `chamados/models.py:1246`)
- **Regua veta:** nao cria chamado; registra `nascimento_recusado` na celula ou no colaborador. A dedup da trilha considera modulo/dia/veto/via e verifica se houve nascimento posterior. (`chamados/models.py:1014`, `chamados/models.py:1122`, `chamados/models.py:1215`, `chamados/models.py:1219`, `chamados/models.py:1227`)
- **Nasceu:** a trilha guarda status anterior `None`, status inicial, modulo e via `abrir`/`criar`; nao recebe motivo nem identifica a funcao nascente nesses campos. Ator/IP podem vir implicitamente da request ou do processo. (`chamados/models.py:1265`, `chamados/models.py:1268`, `core/observ.py:108`, `core/observ.py:117`)
- **Atomicidade:** chamado e evento da celula ficam no bloco transacional da porta; a auditoria possui savepoint proprio e falha best-effort. (`chamados/models.py:949`, `chamados/models.py:1027`, `chamados/models.py:1141`, `chamados/models.py:1146`, `core/observ.py:105`, `core/observ.py:133`)
- **Leitura da historia:** o historico sintetiza nascimento por `criado_em`, com autor “o sistema”; sua consulta de auditoria aceita somente `estado_chamado`. O vigia `trilha_x_banco` tambem filtra essa acao, enquanto `escritas_por_cron` conta as acoes pelo ator. (`chamados/services/historico.py:44`, `chamados/services/historico.py:75`, `chamados/services/historico.py:86`, `chamados/services/vigia_loop.py:114`, `chamados/services/vigia_loop.py:86`)

Provas adicionais lidas:

- Concorrencia/dedup: `chamados/tests/test_c2_corrida_do_abrir.py::test_MORDE_duas_na_janela_abrem_UM_chamado` — `chamados/tests/test_c2_corrida_do_abrir.py:127`.
- Trilha por `criar`: `chamados/tests/test_chamado_nasce_com_trilha.py::test_MORDE_criar_deixa_uma_linha_no_nascimento` — `chamados/tests/test_chamado_nasce_com_trilha.py:30`.
- Recusa real do emissor de turno: `ponto/tests/test_o_outbox_celula.py::test_RED_processar_alertas_turno_nao_abre_turno_aberto_sob_celula_absolvida` — `ponto/tests/test_o_outbox_celula.py:355`.
- Veto tambem em `criar`: `chamados/tests/test_o261_veto_na_porta.py::test_MORDE_criar_tambem_veta` — `chamados/tests/test_o261_veto_na_porta.py:448`.
- Chamado/evento juntos: `chamados/tests/test_c20_juiz3_outbox.py::test_RED_chamado_e_evento_entram_juntos_ou_nenhum` — `chamados/tests/test_c20_juiz3_outbox.py:118`.

## 3. TRES LISTAS

### L1 TRANSICAO COM MAIS DE UM JUIZ — 2

1. **N1, inexistente -> aberto:** natureza em `chamados/catalogo/natureza.py:36`; motor em `chamados/catalogo/motor.py:130`; absolvimento em `chamados/models.py:1150`, delegado a `chamados/reconciliador.py:1019`; veto em `chamados/models.py:1173`, delegado a `ponto/services/furos_vetados.py:27`; validacao de urgencia quando fornecida em `chamados/models.py:942` e `chamados/models.py:1131`. Em `abrir`, somam-se lookup e agrupamento: `chamados/models.py:953`, `chamados/models.py:978`. Em nove nascentes de `criar`, ha ainda os pre-dedups externos identificados em B5/B6/B7/B12/B13/B14/B15/B18/B19.
2. **N2, inexistente -> registrado:** mesma cadeia natureza/motor e mesmas guardas das portas; a escolha REGISTRAR esta em `chamados/models.py:1098`, e a chegada em `chamados/catalogo/motor.py:133`. Geofence possui decisao anterior de modulo e pre-dedup em `ponto/services/geofence.py:344` e `ponto/services/geofence.py:353`; a abertura local pode substituir o modulo em `chamados/services/abertura.py:30`.

Sao predicados complementares observados; a contagem nao afirma que sejam copias da mesma regra. (`chamados/catalogo/motor.py:5`, `chamados/models.py:1152`, `chamados/models.py:1175`)

### L2 ESCRITOR FORA DA PORTA — 4 sitios historicos; 0 encontrados no runtime

1. `registrar`: `aberto -> registrado`, por `queryset.update`. (`chamados/migrations/0038_a3_registrado_backfill.py:10`)
2. `desfazer`: `registrado -> aberto`, por `queryset.update`. (`chamados/migrations/0038_a3_registrado_backfill.py:15`)
3. `backfill`: `cancelado -> superado`, com via e ponteiro `superado_por`, quando encontra um irmao unico. (`chamados/migrations/0039_a3_via_backfill.py:23`, `chamados/migrations/0039_a3_via_backfill.py:24`)
4. `desfazer`: `superado -> cancelado`, limpando o ponteiro. (`chamados/migrations/0039_a3_via_backfill.py:33`)

Esses quatro sitios alteram existentes; nao criam chamados. Ha tambem escrita historica de `via_encerramento` sem alterar status. (`chamados/migrations/0038_a3_registrado_backfill.py:10`, `chamados/migrations/0038_a3_registrado_backfill.py:15`, `chamados/migrations/0039_a3_via_backfill.py:24`, `chamados/migrations/0039_a3_via_backfill.py:25`, `chamados/migrations/0039_a3_via_backfill.py:27`, `chamados/migrations/0039_a3_via_backfill.py:28`, `chamados/migrations/0039_a3_via_backfill.py:34`)

No runtime examinado, a persistencia do estado fica em `_gravar_estado`, `abrir` e `criar`; as duas atribuicoes de `_vivas_entre` sao temporarias e restauradas. (`chamados/models.py:228`, `chamados/models.py:296`, `chamados/models.py:1018`, `chamados/models.py:1142`, `chamados/models.py:356`, `chamados/models.py:371`)

A prova estrutural e `chamados/tests/test_selo_status_so_pelas_portas.py::test_nenhuma_escrita_fora_das_portas`, em `chamados/tests/test_selo_status_so_pelas_portas.py:340`; o selo exclui migrations e declara limites para tipos/argumentos dinamicos. (`chamados/tests/test_selo_status_so_pelas_portas.py:15`, `chamados/tests/test_selo_status_so_pelas_portas.py:17`)

### L3 ESTADO ALCANCAVEL SEM SAIDA — 0 incluindo migrations; 2 terminais no runtime corrente

- **Os dois destinos de nascimento possuem saida:** `aberto` e `registrado` podem ir a `em_analise` pela porta `analisar`. `em_analise` possui saidas no motor; `resolvido` e `fechado` possuem REABRIR. (`chamados/models.py:847`, `chamados/catalogo/motor.py:101`, `chamados/catalogo/motor.py:112`, `chamados/catalogo/motor.py:113`)
- **`cancelado`: terminal absoluto declarado no runtime.** Alcancado por `cancelar`, chamado pelos commands nos sitios `ponto/management/commands/fechar_chamados_vinculo_na_lista.py:58` e `escala/management/commands/fechar_avisos_regua_na_lista.py:48`; escritor em `chamados/models.py:835`; recusa de saida humana em `chamados/catalogo/motor.py:142`. Incluindo migrations, existe saida condicional para `superado`: `chamados/migrations/0039_a3_via_backfill.py:24`.
- **`superado`: terminal absoluto declarado no runtime, de acervo.** A chegada identificada e a migration; a antiga porta foi removida. (`chamados/catalogo/motor.py:106`, `chamados/models.py:841`, `chamados/migrations/0039_a3_via_backfill.py:24`) Incluindo o reverso da migration, existe saida para `cancelado`. (`chamados/migrations/0039_a3_via_backfill.py:33`)

A contagem literal de codigo fora de testes inclui esses reversos; por isso nao seria correto declarar ausencia absoluta de saida para os dois terminais sem delimitar o runtime. (`chamados/migrations/0038_a3_registrado_backfill.py:20`, `chamados/migrations/0039_a3_via_backfill.py:39`)

## 4. NAO LIDO OU INCERTO

- Banco, logs, volume de nascimentos, duplicatas atuais, perdas de trilha e frequencia real por nascente: **[nao verificado]**. Os numeros do anexo nao foram usados como medicao desta entidade.
- Implantacao e execucao efetiva dos crons: **[nao verificado]**; os sitios citados demonstram configuracao, nao execucao. (`config/crons.py:362`, `config/crons.py:440`, `config/crons.py:1527`)
- Aplicacao das migrations no ambiente real: **[nao verificado]**; foram lidos seus caminhos de ida e reverso. (`chamados/migrations/0038_a3_registrado_backfill.py:20`, `chamados/migrations/0039_a3_via_backfill.py:39`)
- Resultado atual das provas comportamentais: **[nao verificado]**. Os testes foram lidos; nao foi executada a suite Django.
- Completude contra chamadas construidas dinamicamente, SQL externo, shell externo ou receptores genericos: **[nao verificado]**. O proprio selo declara limitacoes de resolucao estatica. (`chamados/tests/test_selo_status_so_pelas_portas.py:17`)
- `criar(status=...)` e `abrir(defaults=...)` permitem sobrepor o estado escolhido pelo motor; os sitios operacionais inventariados nao exercitam nascimento terminal. O smoke exercita estados adicionais sob restricao de banco e rollback. (`chamados/models.py:987`, `chamados/models.py:1110`, `chamados/management/commands/smoke_chamados.py:33`, `chamados/management/commands/smoke_chamados.py:44`, `chamados/management/commands/smoke_chamados.py:127`)