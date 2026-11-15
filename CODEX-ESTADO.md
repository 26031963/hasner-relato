# CODEX-ESTADO

> Ultima resposta processada: **14/11/2026 21:47** · publicado em **14/11/2026 21:47** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 14/11 21:47 · sessao b777389f245f · - · - · origem=Ronald (tmux)

Todas as referencias abaixo sao da copia, sem executar testes nem escrever arquivos. [nao verificado]

| transicao | juiz / nascente | escritor / persistencia | leitor / prova |
|---|---|---|---|
| token vazio/antigo -> token registrado | Duas funcoes `api_registrar_fcm`: formato minimo e `:`; app/api/views.py:2461-2476; app/api/views_core.py:994-1009. Rotas app/api/urls.py:99; app/api/urls_core.py:18 | `_gravar_colab(...,'aparelho',{'fcm_token':token})`; app/api/views.py:2476; app/api/views_core.py:1009; `setattr/save` em app/colaboradores/services/colaborador.py:55-61. Nao limpa carimbos antigos de remocao nessas operacoes | Canal: app/core/canal.py:22. Envio: app/chamados/fcm_utils.py:104-106. Prova especifica de registro: nenhum identificado |
| token presente -> vazio por token compartilhado | Ambas APIs filtram mesmo token excluindo dono atual; app/api/views.py:2473; app/api/views_core.py:1006 | `limpar_fcm` -> update token, `fcm_removido_em`, `fcm_removido_motivo`; evento `fcm_token_removido`; app/colaboradores/fcm.py:55-67 | app/holerite/tests/test_selo_sem_notificacao.py::test_MORDE_aparelho_compartilhado_nao_rouba_token_em_silencio:182; o teste chama limpeza diretamente, nao as duas APIs |
| token presente -> vazio por FCM invalido | `_token_invalido`: NotFound ou InvalidArgument contendo registration/token; app/chamados/fcm_utils.py:20-27; `push_fcm`:89-94 | `_limpar_token`:41 -> `limpar_fcm`; app/colaboradores/fcm.py:61-67 | app/chamados/tests/test_push_fcm.py::test_10_push_fcm_token_invalido_limpa:206; app/holerite/tests/test_selo_sem_notificacao.py::test_o_FCM_que_recusa_o_token_passa_pelo_escritor_unico:173 |
| token presente -> vazio no logout | `api_logout`, `if colab.fcm_token`; app/api/views.py:2483-2494 | `_gravar_colab` direto, apenas token e atualizado_em; app/api/views.py:2494. Fora de `limpar_fcm`; nao grava motivo/carimbo especializado nesse sitio | app/core/canal.py:22; prova especializada nenhum |
| token presente -> vazio no reset onboarding | `resetar_onboarding`; app/core/credenciais.py:150-166; nascente tela app/colaboradores/views.py:1506 | `_gravar_colab(...,'credenciais',...)`:165-166. Trilha `reset_onboarding_adm` se usuario existir:174-181; nao `fcm_token_removido` | app/core/tests/test_bug116_credencial_deixa_trilha.py::test_MORDE_desparear_aparelho_deixa_linha_sem_o_token:108 |
| subscription inexistente/inativa -> ativa | `salvar_subscription`: POST, colaborador, endpoint/chaves completos; app/ponto/views.py:1975-1990 | `update_or_create(endpoint=..., defaults={'ativa':True,...})`; app/ponto/views.py:1991-1998; modelo app/ponto/models.py:542-552 | Cinco emissores leem ativa; app/chamados/push_utils.py:24; app/ponto/webpush.py:37; commands/processar_alertas_turno.py:275; processar_alertas_avancados.py:206/247. Prova de filtro: app/chamados/tests/test_push_subscription_ativa.py::test_04_nenhum_acesso_ignora_ativa:89 |
| subscription ativa -> inativa | WebPushException com 410/404; app/ponto/webpush.py:57-59 | `sub.ativa=False; save(['ativa'])`; app/ponto/webpush.py:65-66 | app/core/canal.py:30; app/holerite/tests/test_selo_sem_notificacao.py::test_MORDE_assinatura_DESLIGADA_nao_conta_como_canal:363 |
| subscription ativa -> apagada | Quatro juizes locais de 410/404: app/chamados/push_utils.py:50; app/ponto/management/commands/processar_alertas_turno.py:298; processar_alertas_avancados.py:225/260 | `sub.delete` respectivamente :51, :300, :226/:261 | app/chamados/tests/test_push_fcm.py::test_08_webpush_410_deleta_subscription:159-172 exige apagar |
| notificacao_negada_em None -> timestamp | `notificacao_negada`: POST, colaborador, campo ainda None; app/ponto/views.py:1941/1956-1962 | `atualizar_em_lote(...'onboarding', notificacao_negada_em=now)` e evento `notificacao_negada`; app/ponto/views.py:1964-1971 | Campo nunca descarimbado declarado app/ponto/views.py:1949-1951. Estado derivado prefere canal vivo antes da negativa: app/holerite/matriz.py:483-491. Prova app/holerite/tests/test_selo_sem_notificacao.py::test_a_porta_do_beacon_carimba_uma_vez_e_deixa_trilha:416 |
| inexistente -> EnvioHolerite pendente | `_talvez_job`, quantidade > CORTE_JOB; app/holerite/views.py:635-638. Nascentes validar_e_enviar :615 e reenviar_filtro :740 | create alvo congelado/total/criador/opcoes; app/holerite/views.py:638-642; default pendente app/holerite/models.py:252. Dispara command por Popen e grava pid/iniciado_em :643-645/:664-678 | Polling `_tela_envio`; app/holerite/views.py:681/724-729. app/holerite/tests/test_selo_job_envio.py::test_acima_do_corte_o_request_NAO_empurra_nada:80 |
| pendente -> enviando | command `_rodar`, estado em EM_CURSO; app/holerite/management/commands/enviar_holerites_em_lote.py:43-50 | queryset.update estado/pid/iniciado_em :51-53 | app/holerite/views.py:698-702. app/holerite/tests/test_selo_job_envio.py::test_o_job_envia_carimba_progresso_e_fecha_o_placar:143 |
| enviando -> concluido | `servicos.enviar` retorna sem exception; app/holerite/management/commands/enviar_holerites_em_lote.py:66-81 | estado, placar, progresso=total, concluido_em, pid=None; :79-81 | app/holerite/views.py:702-719. Prova anterior :143-155 |
| enviando -> erro | exception de `servicos.enviar`; command :71-78 | estado erro, repr(exc), concluido_em, pid=None; :74-76 | app/holerite/views.py:717-719. Prova especifica exception: nenhum identificado |
| pendente/enviando -> erro por processo morto | `_tela_envio` chama `job_morreu`; app/holerite/views.py:688; predicado estado/pid/carencia 15s/os.kill; app/holerite/models.py:283-293 | queryset.update estado erro/pid=None/erro/concluido_em; app/holerite/views.py:690-694 | app/holerite/tests/test_selo_job_envio.py::test_MORDE_job_morto_vira_erro_e_nao_barra_eterna:221 |
| holerite sem aviso/falha -> avisado | `servicos.enviar`, retorno _push ok; app/holerite/servicos.py:109-114. Nascentes views.validar_e_enviar :618 via servicos :155; views.reenviar_filtro :745; command enviar_holerites_em_lote :66 | `avisado_em=now`, limpa falha; app/holerite/servicos.py:113-114; evento holerite_envio :134-140 | Juiz leitura app/holerite/matriz.py:480-495. Prova app/holerite/tests/test_selo_vocabulario_envio.py::test_MORDE_o_envio_que_da_certo_apaga_a_falha:287 |
| holerite -> falha carimbada | _push sem ok e motivo diferente de sem_token; app/holerite/servicos.py:109/116/124 | falha_em/falha_motivo; :129-131; nao zera aviso anterior | app/holerite/matriz.py:480/493. app/holerite/tests/test_selo_vocabulario_envio.py::test_MORDE_push_que_falha_CARIMBA_a_falha:301 |
| holerite com falha -> falha limpa por sem_token | retorno motivo sem_token; app/holerite/servicos.py:116 | update falha_em=None/falha_motivo=''; :121. Nao escreve estado enum | app/holerite/matriz.py:483-493. Prova especifica dessa limpeza: nenhum identificado |
| holerite sem aviso/falha/avisado -> aviso/lembrete carimbado | command lembrar_holerites `--apply`; elegibilidade :77-94 e token_voltou :46-56 | no ok: lembretes_n+1, ultimo_lembrete_em, limpa falha; se sem aviso, avisado_em=agora; app/holerite/management/commands/lembrar_holerites.py:95-108 | proximo_lembrete :123-144. app/holerite/tests/test_selo_vocabulario_envio.py::test_MORDE_o_cron_REALMENTE_reenvia_quem_falhou:274. **Nao cron ativo**, exclusao expressa app/config/crons.py:1397 |
| holerite/lote -> aviso legado carimbado | `avisar_lote`, POST, nao avisado ou forcar=1, resultado FCM ok; app/holerite/views.py:1794/1803-1815 | holerite avisado_em :1821-1823; lote avisado_em e avisados_n mesmo com zero sucesso :1826-1828. Nao limpa falha de holerite | app/holerite/matriz.py:480. Prova especifica nenhum identificado |
| inexistente -> cobertura.push_status pendente | `designar_cobertura`; app/ponto/services/cobertura.py:22; validacoes :52-98 | create ordem com push_status pendente :101-113 | proprio emissor :156-187. app/ponto/tests/test_cobertura_caracterizacao.py::test_caminho_feliz_201:109 |
| pendente -> enviado / falhou | juiz local push_ok e FCM ok; app/ponto/services/cobertura.py:157-183 | push_enviado_em=now mesmo falhou; push_status e save :185-187 | resposta JSON :192-193; teste anterior so aceita enviado/falhou e carimbo :116-118 |
| inexistente -> cobertura.push_status '' | declaracao passada com substituto, marcos e ordem ainda inexistente; app/ponto/services/cobertura.py:275-291 | create push_status='' :291-294; sem envio deliberado :203-206 | app/ponto/tests/test_declarar_cobertura_passada.py::test_MORDE_com_nome_a_ordem_nasce_concluida_no_turno_e_sem_push:54 |

Censos e achados para consolidacao:

- **FCM: 26 nascentes diretas de producao** chamam `push_fcm_colaborador`; wrapper unico chama `push_fcm` em app/chamados/fcm_utils.py:106. Lista completa enviada em mensagem anterior. Envio externo e retorno message_id em app/chamados/fcm_utils.py:85-87; isso nao constitui ACK de consumo no aparelho.
- **Web Push: cinco sitios de chamada do driver**: app/chamados/push_utils.py:40; app/ponto/webpush.py:50; app/ponto/management/commands/processar_alertas_turno.py:288; processar_alertas_avancados.py:215/250. Os wrappers tradicionais tem **23 chamadas push_colaborador + 35 push_dp + 10 push_supervisao**; mais uma chamada push_web_colaborador app/holerite/servicos.py:42. Lista completa enviada.
- **L1 token removido**: cinco nascentes julgadoras (duas APIs compartilham, FCM invalido, logout, reset), referencias na tabela.
- **L1 registro token**: duas funcoes/predicados clonados app/api/views.py:2461-2476 e app/api/views_core.py:994-1009.
- **L1 subscription expirada**: cinco predicados 410/404, com destinos diferentes (um desativa, quatro apagam), referencias na tabela.
- **L1 EnvioHolerite -> erro**: exception no command app/holerite/management/commands/enviar_holerites_em_lote.py:71 e processo morto no polling app/holerite/views.py:688 / models.py:283-293.
- **L1 aviso holerite**: tres operacoes escrevem aviso por sucesso: app/holerite/servicos.py:113, app/holerite/management/commands/lembrar_holerites.py:99-104, app/holerite/views.py:1821-1823; elegibilidades diferentes.
- **L2 limpeza token: dois sitios fora da funcao especializada declarada escritora unica** (app/colaboradores/fcm.py:1): logout app/api/views.py:2494 e reset app/core/credenciais.py:165-166. Ambos passam pelo escritor generico do Colaborador, app/colaboradores/services/colaborador.py:55-61. Nao confundir fora de limpar_fcm com fora de toda porta.
- **Selo fraco identificado**: app/holerite/tests/test_selo_sem_notificacao.py::test_MORDE_nenhum_sitio_apaga_o_token_por_fora:240 procura so regex `update(...fcm_token='')` na mesma linha (:264), nao morde os dois dicionarios passados para `gravar`.
- **L2 Web Push**: app/ponto/webpush.py:1 declara emissor unico; quatro chamadas de driver/limpezas existem fora dele, referencias acima. `push_utils` nao passa pelo escritor que desativa.
- **L2 holerite**: `servicos.enviar` declara carimbar aviso (app/holerite/servicos.py:78), mas command e avisar_lote tambem escrevem campos de aviso (tabela). Nao ha porta unica de EnvioHolerite declarada no modelo lido: sua escrita dispersa nao deve virar violacao de porta inventada.
- **L3 EnvioHolerite**: concluido e erro sao dois terminais declarados app/holerite/models.py:241; command recusa fora de EM_CURSO app/holerite/management/commands/enviar_holerites_em_lote.py:47-50.
- **L3 cobertura push**: enviado, falhou e '' sao alcancaveis pelos escritores da tabela; nao encontrei operacao de producao que os retire. Terminal nao declarado app/ponto/models.py:1119-1126. As funcoes nao reapresentam a mesma ordem ao envio.
- **L3 campo de negativa**: notificacao_negada_em carimbado nao sai por operacao lida; terminal do campo declarado app/ponto/views.py:1949-1951. O estado derivado de canal pode sair pela assinatura/token; app/core/canal.py:22-30; app/holerite/matriz.py:483-491.
- **Falso positivo persistido**: cobertura define push_ok=True depois de `push_colaborador`, ignorando seu dict de retorno (app/ponto/services/cobertura.py:160-166); o wrapper pode devolver ok=False sem excecao por falta de usuario/subscription (app/chamados/push_utils.py:24-26/116-118). Assim `push_status='enviado'` nao prova que sequer algum driver confirmou sucesso; escritor app/ponto/services/cobertura.py:186. Teste caminho feliz nao morde essa distincao (app/ponto/tests/test_cobertura_caracterizacao.py:116-118).
- **Falso positivo nao persistido como estado local de push**: recobrar_questionario ignora dict de retorno FCM e marca `enviou=True` apos a chamada (app/chamados/services/disputa_emissao.py:1728-1732); apenas citar sitio da outra entidade.
- **Concorrencia EnvioHolerite**: command aceita tambem estado enviando e faz update por pk sem comparar estado no update, app/holerite/management/commands/enviar_holerites_em_lote.py:47-53. Corrida efetiva em producao [nao verificado]. Reentrada `enviando -> enviando` nao e mudanca de valor do estado.
- Command `zerar_dados_operacionais` apaga subscriptions app/ponto/management/commands/zerar_dados_operacionais.py:88, mas exige banco de ensaio :25-27; nao contar como nascente alcancavel em producao.
- Sem banco/log, ocorrencias reais, volume e entrega no aparelho [nao verificado]. Testes citados foram lidos, nao executados. [nao verificado]

## 14/11 21:46 · sessao b14fc0b2ba21 · - · - · origem=Ronald (tmux)

Censo por AST, sem importar o modulo: **89 entradas de relogio** (76 `C`, 13 `RAW`), **15 `ENC`**, **104 declaracoes**; 8 entradas `por_empresa=True` e universo de 3 empresas resultam em 105 linhas de relogio, antes de tentar derivar os horarios (`app/config/crons.py:17`, `:281`, `:755`, `:1523`, `:1801`, `:1888-1909`). A instalacao real e a execucao em producao sao [nao verificado].

### Outbox MensagemSaida

Modelo com **4 estados**: `pendente`, `enviada`, `falha`, `descartada`; default `pendente`; chave unica `dedup_key`; contador de tentativas, erro e datas (`mensageria/nucleo/models.py:79-95`).

| # | estado de partida -> estado de chegada | juiz (arquivo:linha) | operacao (funcao) | escritor (arquivo:linha) | o que persiste (campos/modelos/trilha) | leitores (arquivo:linha) | prova (teste ou selo que morde, arquivo::nome, ou "nenhum") | [nao verificado]
|---|---|---|---|---|---|---|---|
| M1 | inexistente -> pendente | Cliente ativo e nao pausado; core devolve algum chamado com `verbo=cobrar`; dedup da chave (`mensageria/nucleo/management/commands/cobrar.py:46-61`). Na escalada WhatsApp exige telefone e `_deve_escalar`: canal habilitado mais push descartado ou 2 enviados nos ultimos 7 dias (`:18-30`, `:73-75`). Sentinela exige cliente ativo, alerta elegivel, fora da sombra e destino cadastrado (`mensageria/nucleo/management/commands/entregar.py:35-55`) | `cobrar.Command.handle`; `entregar(cliente)` | **3 sitios** `get_or_create`: `cobrar.py:60`, `:75`; `entregar.py:54`; default do status em `models.py:91` | `MensagemSaida`: cliente, identificador destino, canal, corpo, dedup_key, status default e criada_em (`models.py:84-95`). Sentinela tambem grava `RegistroAcao`, sitio apenas (`entregar.py:64-74`) | Sender seleciona pendentes (`enviar_outbox.py:36-40`); admin (`mensageria/nucleo/admin.py:10-14`) | `mensageria/nucleo/tests/test_cobrar.py::test_01_so_verbo_cobrar_vira_mensagem` (:34), `::test_03_dedup_por_colab_dia` (:46); `test_cobrar_escalada.py::test_02_push_descartado_escala_na_hora` (:58), `::test_03_silencio_apos_dois_pushes_escala` (:66); `test_entregar.py::test_09_fora_da_sombra_materializa_uma_linha_por_alerta_por_dia` (:83) |
| M2 | pendente -> enviada | Cliente ativo/nao pausado e saldo do teto; push so quando resposta dict tem `ok`; WhatsApp quando driver retorna sem excecao (`enviar_outbox.py:28-40`, `:45-63`) | `enviar_outbox.Command.handle -> _enviar` | `mensageria/nucleo/management/commands/enviar_outbox.py:74-77` | status enviada, enviada_em=now, erro vazio; tentativas nao incrementa no sucesso (`:74-77`) | Teto do sender conta enviadas (`:29-32`); `_deve_escalar` conta enviadas (`cobrar.py:25-30`); admin (`admin.py:12-13`) | `test_enviar_outbox.py::test_01_envia_e_respeita_teto` (:24-31), `::test_04_enviada_nao_reenvia` (:58-62); `test_outbox_whatsapp.py::test_01_roteia_para_o_driver_com_o_destino_da_linha` (:34-43) |
| M3 | pendente -> pendente, com tentativa registrada | Excecao transitoria e tentativas ainda abaixo de 3 (`enviar_outbox.py:15`, `:64-71`) | `_enviar` | `enviar_outbox.py:65-71` | tentativas+1, erro truncado a 1000; status permanece pendente; logger.error (`:65-72`) | Proxima selecao de pendentes (`:36-40`); admin (`admin.py:12-13`) | `test_enviar_outbox.py::test_ok_false_nao_vira_enviada` (:83-91), `::test_erro_transitorio_continua_pendente` (:130-133); `test_outbox_whatsapp.py::test_03_erro_transitorio_tenta_de_novo` (:56-63) |
| M4 | pendente -> falha | Excecao que nao e `RecusaPermanente`, com tentativas >=3 (`enviar_outbox.py:64-70`) | `_enviar` | `enviar_outbox.py:65-71` | status falha, tentativas+1, erro, log (`:65-72`) | Admin (`admin.py:12-13`); sender deixa fora por selecionar somente pendente (`enviar_outbox.py:37`) | `test_enviar_outbox.py::test_03_falha_conta_tentativa_e_sela_na_terceira` (:47-55) |
| M5 | pendente -> descartada | `RecusaPermanente`; push classifica `sem_token`, `token_invalido` ou flag token_invalido (`enviar_outbox.py:19`, `:49-54`, `:67-68`); WhatsApp decide destino invalido e codigos permanentes do provedor (`mensageria/nucleo/whatsapp.py:33-34`, `:65-68`) | `_enviar`, chamando core ou driver WhatsApp | `enviar_outbox.py:65-71` | status descartada, tentativas+1 mesmo na primeira recusa, erro, log (`:65-72`) | `_deve_escalar` le descartada (`cobrar.py:25-29`); admin (`admin.py:12-13`); sender deixa fora (`enviar_outbox.py:37`) | `test_enviar_outbox.py::test_sem_token_vira_descartada_na_primeira` (:118-122), `::test_descartada_nao_e_reprocessada` (:124-128); `test_outbox_whatsapp.py::test_02_recusa_permanente_descarta_sem_queimar_tentativas` (:46-53) |

**Nascentes de producao do nascimento M1:** comando `cobrar` (`cobrar.py:39-45`); comando `entregar` (`entregar.py:79-91`); cron RAW 07:24 `sentinela_diario.sh` (`app/config/crons.py:688`) chama loop cujo terceiro passo e `entregar` (`mensageria/bin/sentinela_diario.sh:14-16`). Agendamento de `cobrar` e de `enviar_outbox`: [nao verificado]; nao aparecem literalmente nas declaracoes lidas de `config/crons.py`.

**Caminho push do sender:** `core_client.push` faz POST e devolve JSON, levantando em HTTP falho (`mensageria/nucleo/core_client.py:83-93`); a API tenta FCM, depois webpush quando FCM nao confirmou, e devolve `ok` agregado; em falha preserva motivo FCM para classificacao permanente do sender (`app/api/views_mensageria.py:478-520`). O driver WhatsApp exige ID de mensagem na resposta para retornar sucesso (`mensageria/nucleo/whatsapp.py:70-74`). O sender nao persiste esse ID em `MensagemSaida` (`enviar_outbox.py:74-77`; campos do modelo em `models.py:84-95`).

**L1 desta fatia:** M1 tem tres ramos de nascimento/juizes; M2 tem selecao do sender e confirmacao de dois drivers; M5 tem classificacao permanente no sender e no driver WhatsApp, nos sitios acima. M3/M4 compartilham a decisao excecao/teto dentro de `_enviar` (`enviar_outbox.py:64-71`).

**L2 desta fatia:** nao encontrei escrita explicita de status fora do sender, alem do nascimento pelos 3 `get_or_create` e default do modelo, nos sitios acima. Ha registro de `MensagemSaida` em `ModelAdmin` sem `readonly_fields` e sem override de gravacao (`mensageria/nucleo/admin.py:10-14`); **ESPECULACAO**: CRUD herdado pode fornecer uma porta generica para status, pois o codigo do framework nao foi lido. Portanto, nao afirmar escritor unico absoluto.

**L3 desta fatia:** `enviada`, `falha`, `descartada` nao saem por nenhuma operacao explicita lida; sender so seleciona pendente (`enviar_outbox.py:37`), levando a esses estados em `:68`, `:70`, `:74`. Sao terminais operacionais do sender, sem declaracao `TERMINAIS` no modelo (`models.py:82-95`); terminalidade absoluta diante do admin: [nao verificado]. `descartada` pode originar **outra mensagem** WhatsApp, sem mudar a mensagem anterior (`cobrar.py:28-29`, `:74-85`).

### Ciclo cron: marcas reais, sem inventar coluna status

Nao ha status persistido do cron em `config/crons.py`; o runner persiste marca de processo, linha de corrida JSONL, carimbo de fim e flag de quebra (`bin/cron_run.sh:9-13`, `:22-35`). `AGUARDA`, `OK`, `ERRO`, `NAO_RODOU`, `ALARME`, `DEPENDENCIA_QUEBRADA` sao classificacoes calculadas na leitura do placar, sem save (`app/core/management/commands/pipeline_placar.py:110-137`).

| # | estado de partida -> estado de chegada | juiz (arquivo:linha) | operacao (funcao) | escritor (arquivo:linha) | o que persiste (campos/modelos/trilha) | leitores (arquivo:linha) | prova (teste ou selo que morde, arquivo::nome, ou "nenhum") | [nao verificado]
|---|---|---|---|---|---|---|---|
| C1 | marca em_curso ausente -> presente | Invocacao do runner (`bin/cron_run.sh:5-10`) | script `cron_run.sh` | `bin/cron_run.sh:9-10` | `logs/crons_em_curso/<nome>_<emp>.<pid>` contendo timestamp (`:9-10`) | Deploy consulta marca e PID (`bin/deploy.sh:108-113`) | Estrutural somente: `bin/contrato_deploy.sh:81-82`; nenhum teste comportamental lido do writer |
| C2 | marca em_curso presente -> ausente | Trap EXIT (`bin/cron_run.sh:10`) | trap do runner | `bin/cron_run.sh:10` | Remove marca (`:10`) | Deploy (`bin/deploy.sh:108-113`) | nenhum |
| C3 | marca em_curso presente -> ausente | PID da marca nao existe em `/proc` (`bin/deploy.sh:108-109`) | `em_curso` | `bin/deploy.sh:109` | Remove marca considerada lixo (`:104`, `:109`) | `recusa_restart_com_cron` (`:110-113`) | nenhum comportamental lido para a remocao |
| C4 | corrida sem registro de fim -> registro JSONL | Retorno do comando `bash -c` (`bin/cron_run.sh:11-13`) | `cron_run.sh` | `bin/cron_run.sh:13` | Append nome, emp, ini, fim, dur, exit em `app/logs/placar.jsonl` (`:6-13`) | `pipeline_placar.medidos` (`app/core/management/commands/pipeline_placar.py:61-82`); `crons_quebrados` (`app/core/services/crons_quebrados.py:24-47`); medidor (`bin/crons_duracao.py:26`, `:59`) | `app/core/tests/test_crons_quebrados.py::test_MORDE_a_ultima_corrida_morta_conta_e_a_recuperada_nao` (:41-49), testa leitor |
| C5 | carimbo fim ausente -> presente; ou presente -> sobrescrito | RC igual 0 ou 2 (`bin/cron_run.sh:22`) | `cron_run.sh` | `bin/cron_run.sh:23-25` | timestamp em `logs/fim/<nome>.<emp>.<data>` (`:23-25`) | Encadeado verifica ja rodou (`bin/encadeado.sh:81-82`) e todos os gatilhos (`:84-90`) | `bin/tests/test_furo_encadeado_ao_cartorio.sh:24-27` e `:45-76`, com limitacoes abaixo |
| C6 | flag quebrado ausente -> presente | Nome != cron_quebrado, RC fora 0/2, flag ausente e comando avisar termina sucesso (`bin/cron_run.sh:29-33`) | `cron_run.sh -> cron_quebrado -> avisar_quebra` | `bin/cron_run.sh:33` | Touch `logs/crons_quebrados/<nome>_<emp>.flag`; chamada de Pauta apenas sitio (`app/core/management/commands/cron_quebrado.py:37`, `app/core/services/crons_quebrados.py:57-73`) | Runner consulta flag (`bin/cron_run.sh:33-35`) | `app/core/tests/test_crons_quebrados.py::test_MORDE_pauta_na_quebra_uma_so_e_fecha_na_volta` (:54-62), testa servico/command, nao touch |
| C7 | flag quebrado presente -> ausente | RC 0/2, flag presente, command recuperacao termina sucesso (`bin/cron_run.sh:32-35`) | `cron_run.sh -> cron_quebrado -> avisar_recuperacao` | `bin/cron_run.sh:35` | Remove flag; recuperacao da Pauta so sitio (`app/core/management/commands/cron_quebrado.py:34-35`, `app/core/services/crons_quebrados.py:76-82`) | Runner (`bin/cron_run.sh:33-35`) | Mesmo teste (:54-62), sem teste shell da flag lido |

**Todas as nascentes do runner:** render de entradas RAW e C pelo `_run` (`app/config/crons.py:1506-1509`, `:1883-1897`); entradas encadeadas pelo `exec` de `bin/encadeado.sh:101-104`. O render chama `--corrente` na linha da raiz quando ha elos (`app/config/crons.py:1907-1908`); o modo corrente consulta a declaracao e percorre elos (`bin/encadeado.sh:40-68`). A entrada `consumir_eventos_celula` roda declaradamente a cada minuto (`app/config/crons.py:393-394`); `reconciliar_chamados` esta como vigia `--alarme`, a cada 15 minutos (`:395`).

**L1 cron: 1 transicao** com dois juizes de remocao de marca: C2/C3, EXIT e PID inexistente (`bin/cron_run.sh:10`; `bin/deploy.sh:109`). Se a tabela consolidada mantiver C2 e C3 como duas linhas, reunir como mesma aresta com duas operacoes para contar L1 uma vez.

**L2 cron: 1 escritor fora do runner**: `em_curso` remove a marca (`bin/deploy.sh:109`), cujo escritor/deletor original e o envelope (`bin/cron_run.sh:9-10`).

**L3 cron:** registro JSONL e carimbo fim nao apresentam remocao de producao encontrada nos sitios pesquisados; sao trilhas/marcos historicos, nao coluna de estado da entidade (`bin/cron_run.sh:13`, `:25`). Nao converter automaticamente em “estado sem saida”. A flag quebrado e a marca em_curso tem saidas explicitas (`:10`, `:35`; `bin/deploy.sh:109`).

### Limitacoes que importam para consolidacao

- **15 selos cron estao marcados expectedFailure ate 20/11/2026**, inclusive envelope, expansao, duplicata, horarios e placar (`app/core/tests/marca_cron15.py:24`, `:29-47`, `:57-62`). Os nomes dos testes existem e suas assercoes mordem, mas nao sao prova de verde atual executado.
- A razao declarada e a sombra que nao cabe no corredor (`app/core/tests/marca_cron15.py:3-15`). Nao medi nem derivei o horario real.
- O selo shell de encadeamento tem parte estrutural e parte executavel, mas foi somente lido (`bin/tests/test_furo_encadeado_ao_cartorio.sh:24-27`, `:45-76`); nao o executar no trabalho somente leitura.
- Contagens sao da copia lida; crontab instalado, banco, flags e logs atuais sao [nao verificado].
- Agendamento efetivo do sender `enviar_outbox` e do `cobrar` permanece [nao verificado]; nascimento por `entregar` tem cron indireto confirmado nos sitios acima.
- O admin de `MensagemSaida` e uma lacuna de exclusividade de escritor, nao um `.save()` local demonstrado (`mensageria/nucleo/admin.py:10-14`).

## 14/11 21:46 · sessao 8c339da2e6f9 · MAPAS-DOS-RIOS · leitura · origem=Code

# MAPA DOS RIOS — FERIADO E CALENDARIO

## 1. RESUMO

1. `Feriado` tem **6 campos cadastrais**, **3 abrangencias** (`nacional`, `estadual`, `municipal`) e **nenhum campo de status**. (`app/core/models.py:5-28`)
2. Foram identificados **4 eventos de transicao**: nascimento, edicao, carimbo historico de fonte e exclusao; **5 sitios de persistencia**. (`app/core/views_config.py:70`, `:83`, `:144`, `:201`; `app/core/migrations/0013_remove_feriado_recorrente_feriado_fonte.py:35`)
3. Nascimento tem **2 portas**: cadastro manual e importacao; existem **4 rotas mutantes** e **1 entrada historica por RunPython**. (`app/core/urls.py:42-45`; `app/core/migrations/0013_remove_feriado_recorrente_feriado_fonte.py:58`)
4. Importacao admite **9 combinacoes mes/dia**; unicidade cadastral usa **4 campos**: data, abrangencia, UF e municipio. (`app/core/views_config.py:113-123`; `app/core/models.py:28`)
5. Calendario acrescenta **0 transicoes persistidas proprias**: oferta e competencia sao resultados em memoria. (`app/colaboradores/services/calendario.py:11`, `:769`, `:794-810`)
6. Oferta tem **3 pernas** e aceita **4 status**; permissao de quem observa fica separada. (`app/colaboradores/services/calendario.py:94-104`; `app/templates/colaboradores/partials/_calendario_grade.html:131`)
7. Existem **4 chamadas diretas** ao produtor do calendario; a porta plural tem **1 consumidor direto**. (`app/colaboradores/views.py:1949`; `app/colaboradores/services/drawer.py:121`; `app/relatorios/furos.py:160`; `app/colaboradores/services/calendario.py:132`; `app/chamados/services/vigia_fio.py:271`)
8. CRUD solicita trilha; importacao e backfill nao a solicitam nesses corpos; falhas de auditoria sao capturadas. (`app/core/views_config.py:75`, `:87`, `:127-152`, `:194`; `app/core/migrations/0013_remove_feriado_recorrente_feriado_fonte.py:20-35`; `app/core/auditoria.py:17-29`)

## 2. TABELA DE TRANSICOES

“Cadastrado”, “alterado” e “removido” abaixo descrevem existencia e valores do registro; nao sao enums adicionados ao modelo. As abrangencias usam as mesmas operacoes, sem portas especificas nacional→estadual→municipal. (`app/core/models.py:5-31`; `app/core/views_config.py:55-70`)

| # | estado de partida -> estado de chegada | juiz (arquivo:linha) | operacao (funcao) | escritor (arquivo:linha) | o que persiste (campos/modelos/trilha) | leitores (arquivo:linha) | prova (teste ou selo que morde, arquivo::nome, ou "nenhum") | [nao verificado]
|---|---|---|---|---|---|---|---|
| T1 | Registro inexistente → `Feriado` cadastrado. (`app/core/views_config.py:83`, `:144`) | Manual: `_requer_admin`, POST e `fonte` preenchida. (`app/core/views_config.py:12-13`, `:42-50`) Importacao: permissao/POST, data parseavel, allowlist e deduplicacao. (`app/core/views_config.py:136-147`, `:158`) Unicidade no modelo. (`app/core/models.py:28`) | **Nascente manual:** rota novo → `form_feriado`. (`app/core/urls.py:42`) **Nascente importacao:** rota importar → `importar_feriados_nacionais` → `_importar_do_payload`. (`app/core/urls.py:45`; `app/core/views_config.py:156`, `:167`) | Manual: `objects.create`. (`app/core/views_config.py:83-86`) Importado: `objects.get_or_create`. (`app/core/views_config.py:144-147`) | Manual grava os 6 campos e solicita trilha `criar`, com valores posteriores. (`app/core/views_config.py:83-94`) Importacao fixa nacional/UF vazia/municipio vazio e usa nome/fonte da allowlist como defaults; existente e preservado, sem nova criacao. Nao chama auditoria nesse corpo. (`app/core/views_config.py:127-152`) | Lista cadastral. (`app/core/views_config.py:24`) Alcance territorial. (`app/core/feriados.py:57-83`) Calendario. (`app/colaboradores/services/calendario.py:317-318`) | `app/core/tests/test_smoke_clique_feriado_form.py::test_o_clique_no_Salvar_do_novo_grava_o_feriado_digitado` — `app/core/tests/test_smoke_clique_feriado_form.py:119-127`. Importacao: `app/core/tests/test_contract_feriado_fonte.py::test_MORDE_payload_com_carnaval_cria_zero_carnaval` — `app/core/tests/test_contract_feriado_fonte.py:55-74`. |
| T2 | Mesmo registro, valores anteriores → valores submetidos dos 6 campos. (`app/core/views_config.py:61-70`) | `_requer_admin`, existencia por `fid`, POST e fonte preenchida. (`app/core/views_config.py:42-50`) | **Nascente:** rota editar → `form_feriado`. (`app/core/urls.py:43`; `app/core/views_config.py:41`) | Atribuicoes e `feriado.save()`. (`app/core/views_config.py:64-70`) | Atualiza nome/data/abrangencia/UF/municipio/fonte; solicita trilha `editar` com antes/depois. (`app/core/views_config.py:61-80`) | Lista e alcance. (`app/core/views_config.py:24`; `app/core/feriados.py:57-83`) Signal captura a data antiga e entrega datas antiga/nova ao servico de futuras. (`app/core/signals.py:14-36`) | `app/core/tests/test_smoke_clique_feriado_form.py::test_o_clique_no_Salvar_do_editar_muda_o_feriado_e_nao_cria_outro` — `app/core/tests/test_smoke_clique_feriado_form.py:135-143`. |
| T3 | Registro legado com `fonte=''` → fonte carimbada pela migracao. (`app/core/migrations/0013_remove_feriado_recorrente_feriado_fonte.py:29-35`) | Pula fonte preenchida; distingue nacional das demais abrangencias; nacional consulta mapa de datas ou recebe texto de ausencia de base. (`app/core/migrations/0013_remove_feriado_recorrente_feriado_fonte.py:29-34`) | **Nascente historica:** `Migration.operations` → `RunPython(carimbar_fonte, desfazer)`. (`app/core/migrations/0013_remove_feriado_recorrente_feriado_fonte.py:58`) | Modelo historico via `apps.get_model`; atribuicao e `save(update_fields=['fonte'])`. (`app/core/migrations/0013_remove_feriado_recorrente_feriado_fonte.py:27-35`) | Somente `fonte`: referencia cadastrada ou declaracao textual de pendencia. Sem chamada de auditoria nesse corpo. Reverso `desfazer` e `pass`. (`app/core/migrations/0013_remove_feriado_recorrente_feriado_fonte.py:17`, `:20-39`) | Lista exibe fonte. (`app/templates/core/config/feriados.html:48`) O predicado de alcance nao consulta fonte. (`app/core/feriados.py:57-67`) | nenhum |
| T4 | `Feriado` cadastrado → registro removido fisicamente. (`app/core/views_config.py:188-201`) | `_requer_admin`, POST e existencia do registro. (`app/core/views_config.py:186-188`) | **Nascente:** rota excluir → `excluir_feriado`. (`app/core/urls.py:44`; `app/core/views_config.py:185`) | `feriado.delete()`. (`app/core/views_config.py:201`) | Solicita trilha `excluir` antes do delete; motivo opcional; retrato anterior inclui nome/data/abrangencia/UF/municipio, sem fonte. (`app/core/views_config.py:193-200`) | `post_delete` entrega a data ao servico de futuras. (`app/core/signals.py:39-43`) Consultas seguintes leem o cadastro restante. (`app/core/feriados.py:27-29`) | `app/core/tests/test_smoke_clique_excluir_feriado.py::test_o_clique_em_Excluir_confirmado_na_caixa_apaga_o_feriado_com_trilha` — `app/core/tests/test_smoke_clique_excluir_feriado.py:110-126`. |

A trilha solicitada pelo CRUD passa por `registrar_log` e chega a `LogAuditoria`; o adaptador captura excecoes, portanto a chamada nao garante que a trilha persistiu. (`app/core/auditoria.py:8-29`; `app/core/observ.py:133-139`)

### Percurso do calendario: leitura, oferta, acao e competencia

- **Feriado nacional/local/praca:** `alcanca` decide por abrangencia, UF e municipio normalizado; sem municipio, somente nacional alcanca. `feriados_do_colab` compoe esse alcance com o municipio do posto do vinculo que responde pela data. Isso seleciona dados para leitura, sem alterar `Feriado`. (`app/core/feriados.py:32-67`, `:101-105`, `:124-166`)
- **Oferta de “Resolver dia”:** exige ultimo recurso, data passada e status em `falta|irregular|aberto|sem_certidao`. Ultimo recurso depende do inicio da competencia atual e da ausencia de pergunta viva respondida. O resultado e atribuido ao dicionario da tela. (`app/colaboradores/services/calendario.py:94-104`, `:463-477`, `:738-739`, `:769`)
- **Oferta versus permissao:** o template exige `pode_veredito` e `dia.resolver_dia_oferecido`; o predicado da oferta declara que permissao pertence ao observador. (`app/templates/colaboradores/partials/_calendario_grade.html:131-132`; `app/colaboradores/services/calendario.py:100-104`)
- **Resolver efetivamente:** o POST entra em `veredito_celula_view`, verifica metodo/permissao, datas e veredito, e despacha ao servico; esse corpo nao consulta novamente `resolver_dia_oferecido`. O cruzamento com a entidade que recebe a decisao fica no sitio `app/ponto/services/veredito_celula.py:211`. (`app/ponto/views.py:3352-3381`)
- **Competencia do calendario:** `janela_fechamento` usa corte da empresa, fallback 21 e limite 2–28. O calendario consulta a janela uma vez, usa-a em `modo=competencia` e entrega o rotulo pronto; as setas preservam esse modo. Nao grava competencia. (`app/ponto/janelas.py:10-21`, `:36-57`; `app/colaboradores/services/calendario.py:175-180`, `:794-810`; `app/templates/colaboradores/calendario_partial.html:37-41`)

**Nascentes de leitura localizadas:** perfil, drawer, relatorio e porta plural chamam `contexto_calendario`; o vigia consome a plural pelos seus dois censos. (`app/colaboradores/views.py:1949`; `app/colaboradores/services/drawer.py:121`; `app/relatorios/furos.py:160`; `app/colaboradores/services/calendario.py:132`; `app/chamados/services/vigia_fio.py:271`, `:429`, `:641`)

**Commands e crons que chegam a essa leitura:** `vigiar_fio_morto`, `backfill_fio_morto_e4b` e `materializar_passivo_validado` chamam os censos; o primeiro participa de encadeamento de cron. Esses sitios nao acrescentam escritor de Feriado ou Calendario. (`app/chamados/management/commands/vigiar_fio_morto.py:180-181`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:78`; `app/chamados/management/commands/materializar_passivo_validado.py:206`; `app/config/crons.py:1736-1738`)

**Demais commands com feriado no nome, discriminados por destino:** `reavaliar_ausencias_feriado` e vigia sem escrita, agendado as 07:02 com `--alarme`; `marcar_feriado_do_posto` escreve cadastro de Posto; `sincronizar_feriado_admin` chega ao escritor do vinculo. Os dois ultimos constam como atos humanos/sob ordem. Sao cruzamentos, sem mapa dessas outras entidades aqui. (`app/ponto/management/commands/reavaliar_ausencias_feriado.py:35-46`; `app/config/crons.py:608`, `:1346-1351`, `:1421`; `app/colaboradores/management/commands/marcar_feriado_do_posto.py:131-137`; `app/colaboradores/management/commands/sincronizar_feriado_admin.py:53-56`)

**Efeitos dos eventos de Feriado sobre outras entidades:** os sitios de saida sao `app/ponto/services/cadastro_futuro.py:49-79` e `app/chamados/signals.py:326-337`. O primeiro recebe datas antiga/nova no save e a data removida no delete; o segundo recebe apenas a data atual no save e declara nao tratar delete. (`app/core/signals.py:35-43`; `app/chamados/signals.py:322-337`)

**Provas do calendario, sem converter projecoes em transicoes:**

- Oferta: `app/chamados/tests/test_o261_vigia_fio.py::test_MORDE_cada_perna_do_predicado`, em `app/chamados/tests/test_o261_vigia_fio.py:930-934`; leitura pelo template: `app/chamados/tests/test_o261_vigia_fio.py::test_o_template_LE_o_juiz_e_nao_tem_segunda_copia`, em `app/chamados/tests/test_o261_vigia_fio.py:943-952`.
- Paridade da porta plural: `app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py::test_RED_porta_do_vigia_le_o_mesmo`, em `app/colaboradores/tests/test_e4cal_l1_r8_resolver_pela_lavra.py:79-87`.
- Competencia: `app/colaboradores/tests/test_ui_cal_competencia.py::test_RED_as_setas_nao_perdem_o_modo`, em `app/colaboradores/tests/test_ui_cal_competencia.py:135-144`; `app/colaboradores/tests/test_ui_cal_competencia.py::test_MORDE_o_template_NAO_deriva_a_janela`, em `app/colaboradores/tests/test_ui_cal_competencia.py:147-167`.

## 3. TRES LISTAS

### L1 TRANSICAO COM MAIS DE UM JUIZ — 4

Contagem literal de transicoes condicionadas por mais de uma funcao/predicado, incluindo permissao, metodo e existencia; essa contagem nao significa quatro conflitos entre regras concorrentes. Os gates correspondentes estao nos sitios abaixo. (`app/core/views_config.py:42-50`, `:136-147`, `:158`, `:186-188`; `app/core/migrations/0013_remove_feriado_recorrente_feriado_fonte.py:29-34`)

1. **T1 — nascimento:** manual combina `_requer_admin`, POST e fonte; importacao combina permissao/POST, data parseavel, allowlist e `get_or_create`. Sao duas admissoes distintas para o mesmo nascimento cadastral. (`app/core/views_config.py:12-13`, `:42-50`, `:136-147`, `:158`)
2. **T2 — edicao:** autorizacao, registro identificado, POST e fonte preenchida. (`app/core/views_config.py:42-50`)
3. **T3 — carimbo historico:** fonte ainda vazia, abrangencia nacional ou local e consulta ao mapa de datas no ramo nacional. (`app/core/migrations/0013_remove_feriado_recorrente_feriado_fonte.py:29-34`)
4. **T4 — exclusao:** autorizacao, POST e registro existente. (`app/core/views_config.py:186-188`)

A composicao da oferta do calendario nao aumenta essa contagem: ela produz um booleano em memoria, sem transicao persistida propria. (`app/colaboradores/services/calendario.py:103-104`, `:769`)

### L2 ESCRITOR FORA DA PORTA — 0 desvios demonstrados

- As cinco escritas localizadas estao dentro das operacoes identificadas na matriz: `form_feriado`, `_importar_do_payload`, `carimbar_fonte` e `excluir_feriado`. O modelo nao declara um escritor unico de estado que essas operacoes estejam contornando. (`app/core/models.py:5-31`; `app/core/views_config.py:41-94`, `:127-152`, `:185-201`; `app/core/migrations/0013_remove_feriado_recorrente_feriado_fonte.py:20-35`)
- Existe **1 escritor historico fora do CRUD runtime**, o backfill de fonte; ele foi incluido como porta propria T3, sem transforma-lo em violacao de uma porta unica inexistente. (`app/core/migrations/0013_remove_feriado_recorrente_feriado_fonte.py:20-35`, `:58`)
- O selo de consulta ao modelo **nao prova escritor unico**: isenta todo `views_config.py` e exclui migrations da varredura. (`app/core/tests/test_contract_feriado.py:49-64`)

### L3 ESTADO ALCANCAVEL SEM SAIDA — 0 demonstrados

- Registro nascido pelo cadastro ou importacao pode ser editado ou excluido; fonte carimbada tambem pode ser substituida pela edicao. (`app/core/views_config.py:83-86`, `:144-147`, `:64-70`, `:201`)
- Exclusao elimina o registro; nao grava um estado terminal. O modelo nao declara terminal, e nascimento continua disponivel para a chave cadastral. (`app/core/models.py:5-31`; `app/core/views_config.py:83`, `:201`)
- O reverso vazio da migracao nao torna a fonte terminal: a porta de edicao continua escrevendo esse campo. (`app/core/migrations/0013_remove_feriado_recorrente_feriado_fonte.py:38-39`; `app/core/views_config.py:69-70`)
- Oferta e modo do calendario sao recalculados por leitura; nao constituem estados persistidos sem saida. (`app/colaboradores/services/calendario.py:168-212`, `:769`, `:804`)

## 4. NAO LIDO OU INCERTO

- Correspondencia independente desta copia com o hash informado: **[nao verificado]**; a arvore disponibilizada nao permitiu confirmar o commit pelo Git. [nao verificado]
- Quantidades, ocorrencias, execucoes de cron e trilhas efetivamente existentes em producao para Feriado/Calendario: **[nao verificado]**. O dado raspado fornecido nao apresenta essa populacao.
- Testes citados foram lidos; resultado de execucao nesta analise: **[nao verificado]**. Nao foi localizada prova especifica do carimbo historico T3: **[nao verificado]**. [nao verificado]
- Quando a consulta de janela/perguntas do ultimo recurso falha, o calendario captura a excecao depois de inicializar valores de fallback; frequencia e efeito ocorrido em producao: **[nao verificado]**. (`app/colaboradores/services/calendario.py:459-479`)
- A fonte e exigida na porta manual, mas o campo aceita vazio no modelo e o alcance territorial nao a consulta. Qualidade das fontes existentes e consequencias concretas em producao: **[nao verificado]**. (`app/core/views_config.py:50-54`; `app/core/models.py:19-22`; `app/core/feriados.py:57-67`)
- Escritas externas a esta copia, comandos ad hoc e SQL executado fora dos sitios lidos: **[nao verificado]**.

## 14/11 21:46 · sessao 9737e6e4000b · - · - · origem=Ronald (tmux)

Censo adicional: **59 chamadas sintaticas `.eh_dia_trabalho(...)` em 39 arquivos de producao**, excluindo `tests/`, `test_*.py` e migrations; os 59 sitios estao agrupados abaixo. A contagem e de chamadas, portanto `relatorios/furos.py:67` e `ferias/management/commands/alertar_retornos.py:34` contam duas cada. Limite: chamadas dinamicas/aliases sem esse nome [nao verificado].

A autoridade `EscalaColaborador.eh_dia_trabalho` retorna `cel.trabalha` quando existe celula; senao chama a versao calculada. Nao consulta `ata.tipo_dia`, diferentemente de `dia_sem_trabalho`. `escala/models.py:1087-1096`.

| Consumo da resposta | Sitios lidos |
|---|---|
| **Somente True aceita trabalho** | `colaboradores/views_fase.py:101`; `colaboradores/views.py:2887`; `ponto/motor_calculo_v2.py:3449` (previstos da semana); `api/views.py:1580-1583` (marcos somente True e dentro vinculo); `ponto/services/cartorio.py:1769`; `escala/services/impacto_troca.py:39`; `escala/services/fase_na_tela.py:186`, `escala/services/fase_na_tela.py:196`; retorno amanha em `ferias/management/commands/alertar_retornos.py:34`. |
| **`is not True` recusa False e None** | `escala/utils.py:2216-2217` (`intervalo_declarado_do_dia` retorna zero); `escala/utils.py:2266-2267` (`minutos_previstos_do_dia` retorna zero); `core/services/painel_op.py:151-152`; `ponto/services/cartorio.py:1904-1905`; `ponto/management/commands/emitir_furo_retroativo.py:139-141` (conta limbo e ignora). |
| **False identifica folga/nao trabalho** | `ponto/motor_calculo_v2.py:2448`; `ponto/precedencia.py:538-543` (veta falta); `escala/utils.py:1605-1606` (ignora na grade por turno); `escala/regua_defesa.py:199` (fase invertida); `ponto/management/commands/vigia_plantio_overnight.py:46-49`; `escala/management/commands/celulas_contra_o_template.py:36-37`; hoje em `ferias/management/commands/alertar_retornos.py:34`. |
| **`is not False`: None segue admitido** | `chamados/services/disputa_emissao.py:820`; `ponto/calculador/alimentacao.py:445-447` (excecao tambem retorna True); `ponto/management/commands/diff_calculador.py:404-406` (mesmo fallback); `escala/services/escala_auto.py:144-145` (qualquer resposta naoFalse na amostra permite proposta de folga). |
| **`is not False` como exclusao: somente False permanece** | `colaboradores/views.py:2883-2884` filtra quem esta de folga hoje, antes de buscar retorno True em `colaboradores/views.py:2887`. |
| **Truthy seleciona trabalho; False e None ficam fora** | `colaboradores/views.py:289-294` e `colaboradores/views.py:2227-2239` (marcos e repasse da resposta); `escala/alimentacao.py:396-397` (datas previstas); `core/views.py:404-405` (ausentes hoje); `ponto/services/fechamento.py:731-732` (datas previstas, tambem exclui suprimidas); `ponto/services/espelho.py:224` (dias da semana); `ponto/management/commands/censo_fase_12x36.py:179`; `relatorios/management/commands/diff_extrato_parcial.py:53-54`; `ponto/management/commands/processar_alertas_avancados.py:49-50` (`not` => ignora False/None). |
| **Truthy para marcos, None separado e resposta repassada** | `api/views.py:260-261` e `api/views.py:269-270`; `api/views_core.py:248-249`, `api/views_core.py:257-258`; `api/views.py:920-924`, `api/views.py:936-937`. |
| **False/None/True explicitamente distintos** | `escala/utils.py:760-764` (False => folga, None => sem_ancora); `escala/views.py:40-54` (marcos truthy; False => folga, None => escala_suspeita, outro => previsto); `core/services/painel_op.py:34-35` (mesmo rotulo ternario); `escala/views_plano_folgas.py:76-82` (naoNone conta calculavel, somente False projeta folga); `escala/servico_jornada.py:548-555` (False => improcedente, True => falta_real, None/excecao => ambiguo); `chamados/detector_anomalias.py:166-172` (None => ok por ancora indefinida, False => grave); `escala/services/previsto_cego.py:103-107` (None => causa fase, True => causa marcos); `escala/services/propositor.py:225-231` (None/excecao => sem proposta; truthy adiciona previsto). |
| **None identifica desconhecimento/limbo** | `escala/utils.py:1760`; `escala/regua_defesa.py:145`, `escala/regua_defesa.py:176` (OR defeito_folga_nao_definida); `escala/services/furos_diarios.py:316-318` (`any None` => limbo); `ponto/management/commands/detectar_ausencias.py:188` (OR defeito); `relatorios/furos.py:67` (duas chamadas, hoje e amanha; qualquer naoNone exclui do levantamento de ancora indefinida). |
| **Consumo misto limbo/trabalho** | `ponto/management/commands/detectar_ausencias.py:144-160` usa naoNone para cadastro calculavel e None OR defeito para limbo; depois `ponto/management/commands/detectar_ausencias.py:306-307` ignora resposta falsy. `ponto/services/faltas_de_hoje.py:39-41` admite somente True OR defeito de folga, este ultimo denominado `limbo`. |
| **Repasse para outro leitor** | `ponto/services/diagnostico_escala.py:152`, `ponto/services/diagnostico_escala.py:173-174` leva resposta em `previsto`; `escala/services/fase_na_tela.py:68-70` tambem leva a `troca_de_ritmo`. O consumidor exige False para sequencia trabalhada em folga, True para ritmo anterior/furo, exclui None do universo trabalhado: `ponto/services/diagnostico_escala.py:64-75`; discordancia e contagens tambem distinguem True/False: `ponto/services/diagnostico_escala.py:110-112`, `ponto/services/diagnostico_escala.py:229-232`. |
| **Chamadas internas ao template, sem novo juiz de ata** | `escala/models.py:1319-1321` aceita resultado naoNone; se None, usa foto/paridade ou None em `escala/models.py:1363-1364`. `escala/models.py:1366-1371` repassa base, convertendo somente True em False quando feriado_vira_folga. |

**Mais 8 chamadas `.eh_dia_trabalho_calculado(...)`, separadas das 59**

- Adaptador interno: `escala/models.py:1096`.
- Precedencia: prefere `_cel_trabalha` naoNone convertido em bool; senao pergunta calculado, excecao => None. `ponto/precedencia.py:357-368`.
- Dois escritores/geradores usam a resposta diretamente como `CelulaDia.trabalha` e insumo de DNA: `ponto/portas/celula.py:289-294`, `ponto/portas/celula.py:911-917`. Apenas sitios citados; nao mapeei essas transicoes.
- Comparadores: resposta atual vs projetada e truthy/falsy para efeito em `colaboradores/management/commands/corrigir_escala_retroativa.py:120-133`; resposta para DNA candidato em `ponto/management/commands/censo_col900.py:115`; igualdade de trabalha+DNA em `ponto/management/commands/tripwire_celulas.py:27-33`; desigualdade de trabalha+DNA em `ponto/services/vigia_cadastro_futuro.py:76-80`.

**Outros sitios pedidos**

- `chamados/services/feriado_reavaliar.py:35-39`: exige `era_dia_previsto is not None`, faixa feriado/folga e `premissa_morta`; nao usa o juiz completo `dia_sem_trabalho`.
- `escala/utils.py:1645` nao decide trabalho/folga: testa nenhuma batida interna AND intervalo_livre; a finalidade local e derivacao de intervalo com copia do consumo. `escala/utils.py:1640-1650`.
- Criador da grade antiga: False vira folga e sai, None permanece como trabalho com `sem_ancora`; `escala/utils.py:760-764`. Criador por turno ignora somente False e depois carimba None; `escala/utils.py:1605-1606`, `escala/utils.py:1760-1765`.

Nao afirmo que os 59 sitios estejam todos ativos em uma configuracao concreta de producao: sua presenca e consumo no codigo foram lidos; execucao efetiva/alcancabilidade por URL, cron ou configuracao [nao verificado].

## 14/11 21:45 · sessao c0e564baf62e · - · - · origem=Ronald (tmux)

Catalogo concluido, so leitura; testes nao executados. [nao verificado] [nao verificado]

- **11 nascentes diretas de criacao em producao**: `app/api/views.py:654`; `app/api/views_core.py:645`; `app/ponto/services/triagem_batida.py:702`; `app/chamados/services/acoes_chamado.py:44`; `app/chamados/services/materializacao.py:933` e `:1013`; `app/chamados/services/regularizacao_ext.py:78`; `app/ponto/views.py:1818`; `app/ponto/services/justificativa.py:188` e `:361`; `app/ponto/services/veredito_celula.py:323`.
- **8 chamadas externas de `anotar`**: aprovacao offline `app/ponto/views.py:475`; conflitos `app/ponto/views.py:1838`, `app/chamados/services/acoes_chamado.py:54`, `app/ponto/services/justificativa.py:202` e `:377`; procedencia `app/chamados/services/materializacao.py:954`; desfazer retratacao `app/ponto/services/retratar_humano.py:207`; flip `app/ponto/services/flip_batida.py:30`.
- **7 nascentes de foto**: `app/api/views.py:772` e `:847`; `app/api/views_core.py:766` e `:831`; `app/ponto/services/triagem_batida.py:752`; `app/ponto/views.py:2943`; `app/chamados/services/acoes_chamado.py:67`. Escrita de `Batida.foto` em `app/ponto/registro_batida.py:307`; criacao/atualizacao de FotoBatida em `:306`/`:311`.
- **3 nascentes de flip**: `app/ponto/views.py:3306`; `app/ponto/services/veredito_celula.py:317`; `app/ponto/management/commands/flip_automatico.py:68`; cron diario `app/config/crons.py:707`.
- **1 aprovacao de existente alem desfazer**: offline muda `aprovada`, autor, instante e origem para `app`, em `app/ponto/views.py:475`. Desfazer escreve `aprovada=True,retratada_em=None` em `app/ponto/services/retratar_humano.py:207`. Geofence nao localizado como anotador de existente. [nao verificado]
- **2 escritores fora da porta**: remover faz `alvo.delete()` em `app/chamados/services/regularizacao_ext.py:137`; corrigir atribui tipo/instante e salva em `:154-156`. `alvo` e Batida pela FK `app/chamados/models.py:4101-4103`. Porta declarada `app/core/portas.py:103`.
- **Furo do selo AST**: essas duas escritas ficam sem entidade atribuida; balde documentado em `app/core/censo_escritas.py:43-47`, ignorado por `app/ponto/tests/test_chokepoint_batida_gate.py:30-31`. Nao concluir “zero fora da porta” pelo selo.
- **5 chamadas de criacao em comando de ensaio**: `app/ponto/management/commands/testar_pwa.py:184`, `:198`, `:219`, `:228`, `:229`; guarda em `:40`. `semear` tambem exige ensaio antes do bulk em `app/ponto/registro_batida.py:333-335`; fabrica smoke chama em `app/ponto/management/commands/gerar_batidas_smoke_motor.py:107`.
- **Origem nao e estado fechado pelos choices**: 6 choices declaradas em `app/ponto/models.py:13-20`, mas materializacao grava `disputa_s84_retro` e `disputa_ancora` em `app/chamados/services/materializacao.py:934` e `:1014`.
- **AEJ nao e nascente ativa pela interface**: retorna 410 em `app/ponto/views.py:57-70`.

Upstream de criacao para citar sem mapear outras entidades:

- Materializacao por disputa: `app/chamados/services/fio.py:670`, `app/chamados/services/acoes_disputa.py:139`, `app/chamados/services/validacao.py:492`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:147` → laco `app/chamados/services/materializacao.py:1382` → ramos `:1228`/`:1230` → criacoes `:933`/`:1013`.
- Unidade de materializacao chamada diretamente tambem em `app/chamados/services/validacao.py:486` e `app/chamados/services/passivo_validado.py:522`; ultimo pelo comando `app/chamados/management/commands/materializar_passivo_validado.py:113` e `app/chamados/services/passivo_validado.py:434`. Ambos comandos declarados sob demanda em `app/config/crons.py:786-787` e `:848-853`.
- Regularizacao, incluindo as escritas fora da porta: `app/chamados/views_regularizacao.py:217`, `app/api/views.py:2988`, `app/api/views_core.py:1309` → `app/chamados/services/regularizacao_ext.py:357`; retomada em `:225`, disparada por signal `app/chamados/signals.py:139`, on_commit `:146`.
- Veredito que pode criar/flipar: unica nascente externa localizada `app/ponto/views.py:3380`; a outra chamada fixa `faltou`, sem criacao, em `app/ponto/services/declarar_dia.py:215`.
- `materializar_pendentes_aprovadas` nao cria Batida: contrato e corpo em `app/chamados/services/materializacao.py:1414-1416` e `:1424-1437`; nao contar seu signal como nascente de criacao.

Provas precisas:

- `app/ponto/tests/test_porta_batida.py::test_MORDE_registrar_2x_a_mesma_batida_e_uma_so` (`:49`).
- `app/ponto/tests/test_porta_batida.py::test_MORDE_anotar_2x_mesmo_estado_1_trilha` (`:56`).
- `app/ponto/tests/test_porta_batida.py::test_MORDE_apagar_2x_1_trilha` (`:77`).
- `app/ponto/tests/test_offline_pendentes_trilha.py::test_rejeitar_deixa_trilha` (`:31`).
- `app/ponto/tests/test_flip_batida.py::test_post_flip_inverte_com_trilha` (`:20`) e `::test_retratada_nao_flipa` (`:29`).
- `app/chamados/tests/test_reg_externa_corrigir_remover.py::test_corrigir_tipo_E_para_S` (`:32`), `::test_remover` (`:46`), `::test_MORDE_competencia_trancada_NAO_segura_o_corrigir` (`:55`).
- `app/chamados/tests/test_chokepoint_reg_externa.py::test_reg_externa_nao_gera_par_oposto_em_60s` (`:23`).
- `app/ponto/tests/test_registrar_batida.py::test_oposto_60s_automatico_recusa` (`:32`), `::test_oposto_60s_humano_cria_e_flaga` (`:45`), `::test_mesmo_tipo_60s_dedup` (`:67`).
- Prova comportamental especifica de aprovar offline: nenhuma localizada. [nao verificado]

## 14/11 21:45 · sessao 5f8c8ea3ee26 · - · - · origem=Ronald (tmux)

**EventoCelula: censo e transicoes.** Campos operacionais: `processado_em`, `tentativas`, `erro`; tres estados derivados dos predicados: pendente vivo (`NULL`, `<5`), morto (`NULL`, `>=5`), processado (timestamp). Modelo: `app/escala/models.py:1857`; predicados: `app/chamados/reconciliador.py:1250`, `:1256`; escrita de sucesso: `:1402`.

| Transicao | Juiz | Operacao/escritor | Persistencia | Leitores/prova |
|---|---|---|---|---|
| inexistente → pendente vivo | Criterio distribuido nas nascentes listadas abaixo; escritor nao guarda/deduplica (`app/ponto/portas/celula.py:945`) | `emitir_evento_celula`, INSERT unico (`app/ponto/portas/celula.py:954`) | IDs da celula e colaborador, data; `criado_em`, `processado_em=NULL`, `tentativas=0`, `erro=''` (`app/escala/models.py:1857`) | `eventos_pendentes` (`app/chamados/reconciliador.py:1247`); prova `app/ponto/tests/test_o_outbox_celula.py::test_lavrar_veredito_com_save_gera_1_evento` (`:56`, assert `:61`) |
| pendente vivo → pendente vivo com tentativa incrementada | `falhas` nao vazio (`app/chamados/reconciliador.py:1395`), capturado por excecao (`:1389`) ou acumulado pelas operacoes (`:1369`, `:1373`, `:1376`, `:1384`) | `consumir_eventos_celula`, UPDATE (`:1396`) | `tentativas=F+1`, `erro` concatenado e truncado em 500; processado continua NULL (`:1396`) | Prova `app/ponto/tests/test_o_outbox_celula.py::test_MORDE_falha_de_UM_chamado_deixa_os_eventos_do_colab_pendentes` (`:209`, asserts `:238`) |
| pendente vivo na tentativa 4 → morto na tentativa 5 | Mesmo ramo de falha (`app/chamados/reconciliador.py:1395`); corte 5 (`:1210`, `:1256`) | Mesmo UPDATE (`:1396`) | tentativa 5, erro, NULL; deixa de ser selecionado pelo consumidor (`:1250`) | Prova parcial: `app/ponto/tests/test_o_outbox_celula.py::test_MORDE_evento_morto_conta_a_parte_e_nao_cega_vigia_nem_semeadura` (`:460`); teste monta morto por update (`:467`), nao executa cinco falhas |
| pendente vivo → processado | Ausencia de falhas e de adiamento (`app/chamados/reconciliador.py:1395`, `:1397`, `:1401`) | `consumir_eventos_celula`, UPDATE (`:1402`) | `processado_em=agora`; nao limpa erro/tentativas anteriores (`:1402`) | Prova `app/ponto/tests/test_o_outbox_celula.py::test_MORDE_consumir_2x_e_1_retratacao_0_duplicata` (`:163`); ordem fabrica antes da marca: `app/chamados/tests/test_e4_c20.py::test_consumidor_chama_fabrica_antes_de_marcar_evento` (`:30`) |
| processado → linha removida | `processado_em < agora-7 dias` (`app/chamados/reconciliador.py:1409`); constante 7 (`:1211`) | Cleanup ao final de `consumir_eventos_celula`, DELETE (`:1409`) | Remove linha (`:1409`) | Teste especifico nao encontrado [nao verificado] |

**Sem transicao persistida:** adiamento por `_orf['adiadas']>0` deixa pendente sem consumir tentativa (`app/chamados/reconciliador.py:1379`, `:1397`). Prova `app/chamados/tests/test_orfas_pelo_evento.py::test_MORDE_lampada_de_hoje_encerra_amanha_sem_batida_nova` (`:235`, asserts `:247`, `:248`, `:255`).

**Nascentes diretas: 9 sitios em 8 funcoes**, todos chamam o unico INSERT `app/ponto/portas/celula.py:954`:

- `reverter_regeneracao_dia`: `app/ponto/portas/celula.py:838`.
- `salvar_e_emitir`: `app/ponto/portas/celula.py:965`; juiz intersecao dos campos lavrados com `{veredito,ata}` (`:942`, `:961`, `:964`).
- `ChamadoColaborador.abrir`: `app/chamados/models.py:1027`; nascimento efetivo, modulo de turno e FK celula (`:1018`, `:1025`).
- `ChamadoColaborador.criar`: `app/chamados/models.py:1146`; modulo de turno e FK (`:1144`).
- `PerguntaDisputa.acordar_celulas`: `app/chamados/models.py:3304`; resolve FK/senao par, pula ausente e deduplica celula no lote (`:3295`).
- `_cartorio_post_save_chamado`: `app/chamados/signals.py:209`; mudanca de estado, nao criado, estado vivo, modulo de turno e FK (`:193`, `:200`, `:203`); INSERT best-effort em savepoint (`:207`).
- `semear_eventos_celula`: dois sitios `app/chamados/reconciliador.py:1298`, `:1311`; chamado vivo e pergunta orfa respectivamente, sem pendente vivo existente (`:1290`, `:1296`, `:1303`, `:1309`).
- `_emitir_eventos_dos_turnos_fechados`: `app/ponto/nucleo.py:93`; acionado por `recompute_turnos` quando aberto deixa de existir (`:64`, `:66`), somente celulas existentes (`:92`).

**Nascentes indiretas importantes — citar so sitio:**

- `salvar_e_emitir`: `lavrar_veredito` em `app/ponto/portas/celula.py:1066`, `:1098`; desfazer julgamento futuro `:1167`; `carimbar` `:1201`; `carimbar_precedencia` `:1216`. Ramo de somente status em `:1083` nao emite, pela intersecao em `:961`.
- `acordar_celulas`: `app/chamados/signals.py:259`; `app/chamados/models.py:1615`, `:3063`, `:4010`; `app/chamados/services/fio.py:934`, `:1046`; `app/chamados/services/disputa_emissao.py:1633`.
- Command normal consome em `app/chamados/management/commands/consumir_eventos_celula.py:31`; `--semear` somente semeia e retorna (`:28`, `:29`, `:30`). Exclusao de execucao no command (`:19`), nao dentro de `consumir_eventos_celula`; advisory lock em `app/chamados/reconciliador.py:1236`.
- Cron a cada minuto: `app/config/crons.py:394`.

**Quem consome e quem marca:** `consumir_eventos_celula` seleciona pendentes vivos ordenados por criacao/PK (`app/chamados/reconciliador.py:1344`), chama sete operacoes em sitios `:1365`, `:1366`, `:1369`, `:1373`, `:1376`, `:1384`, `:1388`, e depois marca (`:1393`). Se colaborador nao existe, pula operacoes e pode marcar processado mesmo assim (`:1357`, `:1358`, `:1402`). Lote default 500 (`:1316`), prazo 45 s (`:1212`), eventos ja lidos nao sao repetidos na mesma execucao (`:1344`, `:1347`).

**L1:** nascimento tem criterios distintos nas oito funcoes listadas; sucesso tem composicao `falhas/adiar`, nao um predicado puro central (`app/chamados/reconciliador.py:1395`, `:1397`).

**L2:** nenhum achado em EventoCelula fora de `emitir_evento_celula` e `consumir_eventos_celula`: INSERT `app/ponto/portas/celula.py:954`; UPDATE tentativa `app/chamados/reconciliador.py:1396`; UPDATE consumo `:1402`; DELETE `:1409`. Nao identifiquei selo especifico que proiba outro escritor do proprio EventoCelula [nao verificado].

**L3:** morto e alcancavel pelo incremento (`app/chamados/reconciliador.py:1396`) e nao tem saida de producao encontrada: excluido dos pendentes (`:1250`), retencao so processados (`:1409`). Terminal **declarado** pelo comentario `:1208`. Semear cria outro evento, nao ressuscita o morto; prova `app/ponto/tests/test_o_outbox_celula.py:469`, `:472`.

Nao executei testes [nao verificado]. Nao ha dado de banco/log fornecido para quantidades operacionais desta fila [nao verificado]. [nao verificado]

## 14/11 21:45 · sessao 7b1d97444c79 · MAPAS-DOS-RIOS · leitura · origem=Code

# MAPA DOS RIOS — DESLIGAMENTO

Caminhos relativos a `app/`. Estado observado: `situacao`, `data_demissao` e `motivo_desligamento`; “agendado” nao e valor novo de `situacao` (`colaboradores/models.py:295`, `colaboradores/models.py:323`, `colaboradores/models.py:350`, `colaboradores/management/commands/efetivar_desligamentos_agendados.py:8`).

## 1. RESUMO

1. Existem **4 valores declarados** de `situacao`: ativo, afastado, ferias e desligado (`colaboradores/models.py:295`).
2. O ciclo possui **3 atos**: agendar, efetivar e reativar; ha **1 saida adicional** de desligado pelo escritor de afastamento (`colaboradores/services/desligamento.py:107`, `colaboradores/services/desligamento.py:128`, `colaboradores/services/desligamento.py:235`, `ponto/views.py:2467`).
3. Efetivacao tem **2 nascentes diretas**: tela e command; o command possui **1 cron declarado**, com `--apply`, as 06:06 (`colaboradores/views.py:1437`, `colaboradores/management/commands/efetivar_desligamentos_agendados.py:57`, `config/crons.py:440`).
4. Reativacao tem **1 nascente direta**, na tela; exige desligado e grava ativo (`colaboradores/views.py:1417`, `colaboradores/views.py:1420`, `colaboradores/services/desligamento.py:242`).
5. Quem desliga pela tela: usuario autenticado, POST e permissao `editar_cadastro`; superuser passa. Disputa nao bloqueia essa acao (`colaboradores/views.py:1422`, `colaboradores/views.py:1425`, `colaboradores/views.py:1428`, `colaboradores/mixins.py:300`, `colaboradores/mixins.py:322`).
6. Cascata e condicional: resposta sem veredito preserva disputa; pergunta viva pode impedir fechamento do chamado (`colaboradores/services/desligamento.py:164`, `chamados/models.py:1664`).
7. Readmissao implementada como reativacao limpa demissao/motivo e libera acesso; seu corpo nao reabre vinculo, chamado, pergunta, disputa ou celula (`colaboradores/services/desligamento.py:235`).
8. L2 encontra **2 escritores explicitos adicionais** do campo e **3 saves integrais** fora dos executores do ciclo (`ponto/views.py:2467`, `ponto/management/commands/reverter_situacao_afastado.py:73`, `colaboradores/views.py:329`, `colaboradores/views.py:1345`, `colaboradores/views.py:1391`).
9. **0 estados sem saida neste recorte**: desligado tem reativacao; os outros tres valores podem passar pelo juiz de desligamento (`colaboradores/views.py:1417`, `colaboradores/services/desligamento.py:35`).
10. Quantidades de transicoes efetivamente ocorridas em producao e resultado atual dos testes: **[nao verificado]**. [nao verificado]

## 2. TABELA DE TRANSICOES

A primeira linha registra mudanca da intencao persistida, mantendo `situacao`; nao acrescenta estado ao enum (`colaboradores/services/desligamento.py:107`, `colaboradores/management/commands/efetivar_desligamentos_agendados.py:8`).

| # | estado de partida -> estado de chegada | juiz (arquivo:linha) | operacao (funcao) | escritor (arquivo:linha) | o que persiste (campos/modelos/trilha) | leitores (arquivo:linha) | prova (teste ou selo que morde, arquivo::nome, ou "nenhum") | [nao verificado]
|---|---|---|---|---|---|---|---|
| D1 | `situacao` em ativo/afastado/ferias, com intencao anterior qualquer → mesma `situacao`, data de demissao futura e motivo informado (`colaboradores/services/desligamento.py:38`, `colaboradores/services/desligamento.py:78`, `colaboradores/services/desligamento.py:110`) | Autorizacao da tela: `colaboradores/views.py:1428`; `julgar_desligamento`: ja desligado, data valida, teto de 90 dias, motivo preenchido e classificacao futura (`colaboradores/services/desligamento.py:35`, `colaboradores/services/desligamento.py:61`, `colaboradores/services/desligamento.py:68`, `colaboradores/services/desligamento.py:78`) | `executar_desligamento`, ramo `agendado`. Nascente: `desligar_colaborador` (`colaboradores/views.py:1434`, `colaboradores/views.py:1437`) | Dominio: `colaboradores/services/desligamento.py:110`; `gravar` faz `setattr` e save parcial (`colaboradores/services/colaborador.py:55`) | Data/motivo; `HistoricoVinculo` com observacao AGENDADO; evento `editar`, com `agendado=True`; retorna antes da cascata. Repetir pode substituir data/motivo e registrar outro evento (`colaboradores/services/desligamento.py:107`, `colaboradores/services/desligamento.py:113`, `colaboradores/services/desligamento.py:119`, `colaboradores/services/desligamento.py:126`) | Efetivador consulta data preenchida/vencida e exclui desligado (`colaboradores/management/commands/efetivar_desligamentos_agendados.py:38`); disponibilidade consulta data de demissao (`ponto/utils.py:170`) | `colaboradores/tests/test_selo_bug96_demissao_no_futuro.py::test_MORDE_agendado_NAO_desliga_hoje` (:83); `::test_MORDE_agendado_deixa_trilha_dizendo_que_NADA_foi_encerrado` (:151) |
| D2 | ativo/afastado/ferias → desligado; inclui intencao agendada cuja data venceu (`colaboradores/services/desligamento.py:38`, `colaboradores/services/desligamento.py:129`, `colaboradores/management/commands/efetivar_desligamentos_agendados.py:38`) | Tela: autorizacao e `julgar_desligamento` (`colaboradores/views.py:1428`, `colaboradores/views.py:1434`). Command: selecao por data/estado, `--apply`, mesmo juiz (`colaboradores/management/commands/efetivar_desligamentos_agendados.py:38`, :46, :51). Executor escolhe ramo pelo veredito (`colaboradores/services/desligamento.py:107`) | `executar_desligamento`. **Todas as nascentes diretas:** tela (`colaboradores/views.py:1437`) e command (`colaboradores/management/commands/efetivar_desligamentos_agendados.py:57`). Cron declarado chama esse command com `--apply` (`config/crons.py:440`) | Atribuicoes e `salvar_inteiro` (`colaboradores/services/desligamento.py:129`, :133); save completo (`colaboradores/services/colaborador.py:73`) | Situacao/data/motivo; acesso bloqueado; historico; cascata detalhada abaixo; novo chamado `desligamento_revisao` com contexto. Auditoria final e chamada depois do atomic (`colaboradores/services/desligamento.py:128`, :136, :139, :148, :164, :182, :195, :209, :229) | `esta_desligado` (`colaboradores/services/desligamento.py:23`); tela (`colaboradores/services/situacao_exibida.py:56`); veto de novo vinculo (`colaboradores/services/vinculo.py:91`); batida (`api/views_core.py:389`); folha (`folha/export.py:1205`) | `colaboradores/tests/test_selo_bug96_demissao_no_futuro.py::test_MORDE_hoje_continua_desligando_na_hora` (:104); `::test_MORDE_o_cron_consuma_quando_a_data_chega` (:121); `colaboradores/tests/test_desligar_guards.py::test_a_redesligar_nao_duplica` (:36); `chamados/tests/test_o261e_f1_porta_encerramento.py::test_f2_RED_desligamento_encerra_pela_ordem_da_lei_e_nao_cai` (:279) |
| D3 | desligado → ativo, demissao nula e motivo vazio (`colaboradores/services/desligamento.py:242`) | Autorizacao `editar_cadastro` e guarda `situacao == 'desligado'` ficam na view (`colaboradores/views.py:1413`, :1417). O executor inicia a escrita sem repetir essa guarda (`colaboradores/services/desligamento.py:241`) | `executar_reativacao`. Nascente direta: `reativar_colaborador` (`colaboradores/views.py:1420`) | Atribuicoes e `salvar_inteiro` (`colaboradores/services/desligamento.py:242`, :246); save completo (`colaboradores/services/colaborador.py:76`) | Situacao ativo, data nula, motivo vazio; acesso liberado; `HistoricoVinculo` com motivo `reativacao` e autor. Nao ha chamada de reabertura das dependencias no corpo da operacao (`colaboradores/services/desligamento.py:235`, :242, :248, :251) | Juiz do desligamento passa a nao reconhecer desligado (`colaboradores/services/desligamento.py:32`); tela (`colaboradores/services/situacao_exibida.py:56`); juiz de novo vinculo deixa de aplicar esse veto (`colaboradores/services/vinculo.py:91`) | `colaboradores/tests/test_reativar_colaborador.py::test_reativar_repoe_ativo_e_desbloqueia` (:41); `::test_reativar_so_staff` (:51), cujo corpo prova recusa daquele usuario; regra atual de autorizacao em `colaboradores/views.py:1413` |
| D4 | desligado → afastado por POST de INSS; data/motivo de demissao permanecem (`ponto/views.py:2438`, :2465; `colaboradores/services/colaborador.py:61`) | Permissao `editar_ausencias` (`ponto/views.py:2402`); validacao de `criar_ausencia` (`ponto/views.py:2447`, `ponto/services/ausencia.py:243`, :438); ramo `tipo == 'afastamento_inss'` (`ponto/views.py:2465`). O filtro ativo/afastado alimenta a lista, mas o POST resolve PK sem esse filtro (`ponto/views.py:2404`, :2438) | `lancar_ausencia`, efeito sobre o estado do colaborador. Nascente: POST dessa view (`ponto/views.py:2407`, :2467) | Chamada `gravar(..., 'situacao', {'situacao': 'afastado'})` (`ponto/views.py:2467`); save parcial (`colaboradores/services/colaborador.py:55`) | Somente `situacao` nessa escrita; o fato de ausencia e criado antes. Nao chama `executar_reativacao` (`ponto/views.py:2447`, :2467; `colaboradores/services/colaborador.py:61`) | `esta_desligado` deixa de reconhecer desligado (`colaboradores/services/desligamento.py:32`); efetivador ainda pode selecionar pela demissao preservada (`colaboradores/management/commands/efetivar_desligamentos_agendados.py:38`); reversor consulta afastados (`ponto/services/afastado_avisa.py:126`) | nenhum para o caminho especifico desligado → afastado **[nao verificado]** |

### Alcance da cascata de D2 — somente sitios cruzados

- **Vinculos:** chama `fechar_vinculos_ativos`; fecha apenas os ativos cujo inicio nao ultrapassa a demissao. Os posteriores sao recusados/logados; a lista de recusados e descartada pelo chamador (`colaboradores/services/desligamento.py:148`, `colaboradores/services/vinculo.py:622`, :641, :665, :862).
- **Celulas:** o fechamento de vigencia chama suplemento somente de futuras geradas ja existentes, limitado pela janela. Sem sucessor e satisfeitas as guardas, limpa geradora, grava `trabalha=False`, DNA sem vinculo, DNA anterior e carimbos; desfaz julgamento futuro (`colaboradores/services/vinculo.py:657`, `ponto/services/cadastro_futuro.py:26`, :31, :44, `ponto/portas/celula.py:251`, :259, :267).
- **Disputas/perguntas:** primeiro consulta `respostas_sem_veredito`; se houver, pula a disputa. Nas demais chama a porta com motivo desligamento; essa porta repete o juiz e encerra perguntas vivas por `fechar_por_admin`, com decisao SILENCIO e trilha propria (`colaboradores/services/desligamento.py:164`, `chamados/models.py:1942`, :1974, :1980, :1983).
- **Chamados:** fechamento da disputa dispara tentativa de reconciliar o pai; depois o executor percorre `NAO_FECHADOS` e pede `encerrar`. A guarda de pergunta viva pode recusar; nesse caso a nota nao afirma fechamento (`chamados/signals.py:19`, :43, `colaboradores/services/desligamento.py:182`, :187, `chamados/models.py:1664`).
- **Auditoria posterior:** depois da tentativa de fechar os chamados anteriores, cria outro chamado para registro oficial, com modulo `desligamento_revisao` e contexto de data, motivo, autor e instante (`colaboradores/services/desligamento.py:195`, :208, :209).
- **Autor no fechamento do chamado:** o executor passa `_ator`, mas o ramo `ato` de `transicionar_com_pauta` chama `ato()` sem encaminhar esse argumento (`colaboradores/services/desligamento.py:187`, `chamados/services/fecho_runtime.py:157`, :165).

O command `fechar_vinculo_de_desligado --apply` repara vinculos de quem ja esta desligado; nao e outra nascente da transicao D2. Seu sitio chama `encerrar_vinculo`, e o catalogo o identifica como comando manual (`colaboradores/management/commands/fechar_vinculo_de_desligado.py:34`, :55, `config/crons.py:1425`).

## 3. TRES LISTAS

### L1 TRANSICAO COM MAIS DE UM JUIZ — 4

Contagem inclui autorizacao e predicados de elegibilidade; repetir chamada ao mesmo juiz nao significa existir outra implementacao dele (`colaboradores/mixins.py:322`, `colaboradores/services/desligamento.py:35`).

1. **D1 — agendar:** autorizacao (`colaboradores/views.py:1428`); `julgar_desligamento` (`colaboradores/services/desligamento.py:35`); predicado `esta_desligado` (`colaboradores/services/desligamento.py:23`); classificacao futura e ramo executor (`colaboradores/services/desligamento.py:78`, :107).
2. **D2 — efetivar:** mesmos juizes da tela; no command, selecao data/estado e `--apply` antecedem o mesmo `julgar_desligamento` (`colaboradores/views.py:1428`, :1434, `colaboradores/management/commands/efetivar_desligamentos_agendados.py:38`, :46, :51). Os juizes das dependencias condicionam a cascata, sem vetar por si o estado desligado (`colaboradores/services/desligamento.py:129`, :165, :187).
3. **D3 — reativar:** permissao e predicado de desligado na view; ambos ficam fora do executor (`colaboradores/views.py:1413`, :1417, `colaboradores/services/desligamento.py:241`).
4. **D4 — saida por INSS:** permissao, validacao do fato e predicado do tipo; nenhum filtro de situacao acompanha a busca do PK no POST (`ponto/views.py:2402`, :2438, :2447, :2465).

### L2 ESCRITOR FORA DA PORTA — 5 sitios fora dos executores do ciclo

Criterio: persistencia de `situacao` fora de `executar_desligamento` e `executar_reativacao`, distinguindo **2 mudancas explicitas** de **3 saves integrais**. Todos os cinco usam a porta tecnica de Colaborador; ela declara que o chamador decide os valores e que `salvar_inteiro` salva o objeto completo (`colaboradores/services/colaborador.py:8`, :12, :16, :73`).

1. **Mudanca explicita para afastado:** `lancar_ausencia` (`ponto/views.py:2467`).
2. **Mudanca explicita para ativo:** `reverter_situacao_afastado`, pela porta `gravar`; cron declarado as 06:16 com `--apply` (`ponto/management/commands/reverter_situacao_afastado.py:73`, `config/crons.py:448`).
3. **Save integral em vincular posto:** inclui o estado carregado no objeto (`colaboradores/views.py:329`, `colaboradores/services/colaborador.py:76`).
4. **Save integral em editar cadastro:** inclui o estado carregado no objeto (`colaboradores/views.py:1345`, `colaboradores/services/colaborador.py:76`).
5. **Save integral em transferir posto:** inclui o estado carregado no objeto (`colaboradores/views.py:1391`, `colaboradores/services/colaborador.py:76`).

Nos tres saves integrais, sobrescrita de uma transicao concorrente por instancia antiga e **ESPECULACAO**; o mecanismo permite save completo, mas nao foi lida prova desse efeito nesses sitios (`colaboradores/services/colaborador.py:73`).

O selo existente protege a **porta tecnica**, com allowlist vazia e mutante de `update(situacao=...)`; ele nao prova exclusividade dos executores de dominio (`colaboradores/tests/test_chokepoint_cadastro_gate.py:23`, :33, :52, :74). Resultado atual desse selo: **[nao verificado]**.

Django admin nao entra na lista: `ColaboradorAdmin` herda `SoLeitura`, que nega adicionar, alterar e apagar, e torna os campos somente leitura (`colaboradores/admin.py:48`, `core/admin.py:59`, :68).

### L3 ESTADO ALCANCAVEL SEM SAIDA — 0 neste recorte

- **Desligado:** alcancado por D2 e possui saida declarada pela reativacao D3; portanto nao e terminal sem saida (`colaboradores/services/desligamento.py:129`, `colaboradores/views.py:1417`, `colaboradores/services/desligamento.py:242`).
- **Intencao agendada:** possui saida pelo efetivador quando vencida; enquanto nao desligado, novo pedido tambem pode substituir data/motivo (`colaboradores/management/commands/efetivar_desligamentos_agendados.py:38`, :57, `colaboradores/services/desligamento.py:110`).
- **Ativo/afastado/ferias:** o juiz de desligamento nao veta esses valores; o veto de estado e somente ja-desligado (`colaboradores/services/desligamento.py:38`, :70).
- A palavra “terminal” no comentario da cascata refere-se ao vinculo, nao declara desligado como estado irreversivel do colaborador (`colaboradores/services/desligamento.py:179`, :235).

## 4. O QUE FICOU INCERTO OU NAO VERIFICADO

- Identidade Git da copia com o hash informado: **[nao verificado]**.
- Banco, logs, instalacao dos crons, execucoes reais e contagem de eventos de desligamento/readmissao em producao: **[nao verificado]**. O horario citado e declaracao do codigo (`config/crons.py:440`).
- Testes e selos foram lidos; resultado atual da suite: **[nao verificado]**. [nao verificado]
- Ocorrencia real de D4 e prova comportamental especifica desligado → afastado: **[nao verificado]**. Os sitios da busca irrestrita e da escrita foram lidos (`ponto/views.py:2438`, :2467).
- Cancelamento explicito do agendamento nao foi localizado: **[nao verificado]**. A reativacao da tela exige ja estar desligado (`colaboradores/views.py:1417`).
- Atomicidade/idempotencia sob duas requisicoes concorrentes de desligamento: **[nao verificado]**. O teste encontrado cobre dois POSTs sequenciais; o juiz da view roda antes do atomic do executor (`colaboradores/tests/test_desligar_guards.py:40`, :45, `colaboradores/views.py:1434`, `colaboradores/services/desligamento.py:128`).
- **Auditoria final do cron:** `_ReqSistema.user=None`, mas `registrar_log` acessa `request.user.is_authenticated`; a excecao e capturada antes de emitir o evento. Essa consequencia decorre da leitura do caminho; ocorrencia em producao e teste especifico ficam **[nao verificado]** (`colaboradores/management/commands/efetivar_desligamentos_agendados.py:66`, `colaboradores/services/desligamento.py:229`, `core/auditoria.py:24`, :25).
- A rotina de reativacao nao registra nova `data_admissao` nem chama recomposicao das dependencias; prova comportamental abrangendo todas elas: **[nao verificado]** (`colaboradores/services/desligamento.py:235`, `colaboradores/tests/test_reativar_colaborador.py:41`).

## 14/11 21:45 · sessao b767f6b94ad8 · - · - · origem=Ronald (tmux)

Consolidado, so leitura. Todos os caminhos abaixo sao da copia; testes foram lidos, nao executados [nao verificado]. [nao verificado]

### Estados e unidade

- **4 rotulos**, derivados: `pendente`, `respondida`, `validada`, `resolvida` (`app/chamados/models.py:2672`). Nao existe coluna `estado`: a property le validacao, C6, resposta e materializacao (`app/chamados/models.py:3090`).
- `viva/morta` e resultado de juiz, nao quinto/sexto estado. Uma pergunta morta por leitura pode continuar rotulada pendente/respondida sem carimbo (`app/chamados/models.py:3100`).
- **8 atos de trilha**, diferentes de estados: resposta, validacao, devolucao, recusa, reabertura, silencio, retratacao, absorcao (`app/chamados/models.py:3913`).
- `DecisaoPergunta.registrar` e escritor unico da sequencia; linhas existentes nao se editam/apagam (`app/chamados/models.py:3959`, `app/chamados/models.py:3999`).

### Portas e transicoes

| transicao/ato | juiz | escritor e persistencia | nascentes diretas de producao | prova lida |
|---|---|---|---|---|
| inexistente → pendente, pergunta datada | Dia do turno `dia_do_turno_da_linha`, dedup `universo_do_fato`/`canonica_do_grupo`, resolvedora e colaborador validos (`app/chamados/models.py:2895`, `app/chamados/models.py:2931`, `app/chamados/models.py:2944`, `app/chamados/models.py:2961`) | `abrir` → `_sob_contrato` → `_nascer`: identidade, dia, celula/destino, texto/tipo/opcoes; insert, eventuais assinaturas recebidas e log `pergunta_emitida` (`app/chamados/models.py:2832`, `app/chamados/models.py:3070`, `app/chamados/models.py:3081`) | **4**: `app/chamados/services/disputa_emissao.py:444`, `app/chamados/services/disputa_emissao.py:837`, `app/chamados/services/declaracao_texto.py:376`, `app/chamados/services/declaracao_texto.py:666` | `app/chamados/tests/test_porta_pergunta_identidade.py:167::test_MORDE_abrir_2x_o_mesmo_fato_e_uma_pergunta_e_uma_trilha` |
| inexistente → pendente, conversa | Dia civil da chamada; sem dedup na porta; guardas de destino/contrato continuam (`app/chamados/models.py:2850`, `app/chamados/models.py:2857`, `app/chamados/models.py:2895`) | Mesma insercao `_nascer`, `conversa=True` (`app/chamados/models.py:2857`, `app/chamados/models.py:3070`) | **3**: `app/chamados/services/acoes_disputa.py:263`, `app/chamados/services/disputa_emissao.py:439`, `app/chamados/services/disputa_emissao.py:1371` | `app/chamados/tests/test_e1_entradas.py:178::test_MORDE_conversa_e_fato_de_nascimento` |
| existente → existente reapontada/rechaveada | Juizes do fato, rechaveamento e `_costura_nao_mata_o_fio` (`app/chamados/models.py:2996`, `app/chamados/models.py:3008`, `app/chamados/models.py:3028`, `app/chamados/models.py:3036`) | `_nascer` atualiza celula/destino/lavra; recusa costura deixa `costura_recusada`; devolve `criada=False` (`app/chamados/models.py:3020`, `app/chamados/models.py:3051`, `app/chamados/models.py:3060`) | Mesmas **4** chamadas de `abrir`, acima (`app/chamados/models.py:2845`) | Sitio principal de prova: `app/chamados/tests/test_porta_pergunta_identidade.py:167`; contratos adicionais referenciados no proprio corpo (`app/chamados/models.py:2995`, `app/chamados/models.py:3001`) |
| pendente → respondida; respondida → respondida; validada/resolvida → respondida quando assinatura/carimbo sao desfeitos | Validacao por tipo e texto nao vazio (`app/chamados/services/disputa_emissao.py:1432`); carimbo sem lastro e desfeito por `reabrir_por_resposta` (`app/chamados/models.py:3369`) | `responder_pergunta_disputa_core`: texto, instante, limpa validacao anterior, eventualmente carimbo; Decisao `resposta`, logs e nota de fio (`app/chamados/services/disputa_emissao.py:1467`, `app/chamados/services/disputa_emissao.py:1480`, `app/chamados/services/disputa_emissao.py:1508`, `app/chamados/services/disputa_emissao.py:1511`, `app/chamados/services/disputa_emissao.py:1520`, `app/chamados/services/disputa_emissao.py:1555`) | **5**: `app/chamados/views.py:1283`, `app/api/views.py:2620`, `app/chamados/services/acoes_disputa.py:371`, `app/chamados/services/declaracao_texto.py:419`, `app/chamados/services/declaracao_texto.py:693` | `app/chamados/tests/test_porta_resposta_pergunta.py:184::test_MORDE_resposta_vazia_nao_escreve_nada`; `:225::test_MORDE_resposta_NOVA_invalida_a_validacao_anterior`; `:196::test_CARACTERIZACAO_reenviar_a_MESMA_resposta_move_respondida_em` |
| pendente → respondida automaticamente | Recusa se ja tem texto (`app/chamados/services/disputa_emissao.py:1647`) | `responder_automaticamente`: texto, instante e Decisao `resposta`, motivo automatico (`app/chamados/services/disputa_emissao.py:1652`) | **1**: `app/api/foto_recebida.py:48` | `app/chamados/tests/test_porta_chamado_e3.py:46` e `:53`; `app/chamados/tests/test_e4_expand_orm.py:176` |
| texto sem carimbo → respondida | So sem respondida_em e com texto; instante fornecido pela trilha (`app/chamados/services/disputa_emissao.py:1670`) | `carimbar_resposta_pela_trilha`: instante, Decisao `resposta`, evento antes/depois (`app/chamados/services/disputa_emissao.py:1674`) | **1**: command `app/chamados/management/commands/curar_perguntas_em_limbo.py:67` | `app/chamados/tests/test_porta_resposta_pergunta.py:241::test_MORDE_carimbar_pela_trilha_2x_nao_mexe_no_instante`; `:257::test_MORDE_carimbar_sem_texto_nao_carimba` |
| respondida → validada | Existe, respondida, nao validada, plausibilidade; folga tem ramo proprio (`app/chamados/services/validacao.py:408`, `:416`, `:419`, `:427`, `:435`) | `_carimbar_validacao`: validada_em/por e Decisao `validacao` no mesmo atomic (`app/chamados/services/validacao.py:150`, `:157`, `:160`, `:162`); depois materializacao (`:484`, `:492`) | `validar_pergunta`: **5** calls: `app/chamados/services/acoes_disputa.py:19`, `:379`; `app/chamados/services/declaracao_texto.py:696`; `app/api/foto_recebida.py:51`; command `app/chamados/management/commands/validar_classe_a.py:100`. Helper de assinatura tem **3** calls: `app/chamados/services/validacao.py:311`, `:343`, `:458` | `app/chamados/tests/test_e1_entradas.py:94::test_MORDE_validacao_e_linha_com_o_relogio_do_carimbo`; `app/chamados/tests/test_o261_recusa_fala.py:135::test_MORDE_validacao_que_grava_continua_validada` |
| validada → respondida, devolucao ao admin | Hora declarada, nao materializada, motivo de recusa nao vazio (`app/chamados/services/validacao.py:213`, `:215`) | `devolver_validacao_recusada`: limpa assinatura, preserva resposta, Decisao `devolucao`; evento `validacao_devolvida` quando foto mudou (`app/chamados/services/validacao.py:222`, `:225`, `:228`, `:244`, `:246`) | **1** call producao: `app/chamados/services/validacao.py:487` | `app/chamados/tests/test_o261_recusa_fala.py:87::test_RED_recusa_por_paridade_devolve_ao_admin_com_o_motivo`; `:148::test_idempotente_validar_2x_a_recusada_mesmo_estado_uma_trilha` |
| respondida → pendente, recusa humana | Motivo obrigatorio, nao validada, tem resposta (`app/chamados/services/validacao.py:588`, `:600`, `:603`) | `recusar_pergunta` grava motivo e chama `reabrir_para_correcao(... ato='recusa')`; Decisao com foto/autor/motivo; nota, evento e push (`app/chamados/services/validacao.py:611`, `:617`, `:620`, `:625`, `:629`, `:640`) | **1** call de recusar: `app/chamados/services/acoes_disputa.py:80` | `app/chamados/tests/test_porta_validacao_pergunta.py:224::test_MORDE_recusar_SEM_request_carimba_reabre_e_deixa_trilha`; `:245`, `:258`, `:274` cobrem veto/idempotencia/ja validada |
| respondida/validada/resolvida → pendente, correcao/reabertura | `perguntas_corrigiveis` → `pergunta_sem_lastro_ou_viva`; absorcao rejeitada usa grupo proprio (`app/chamados/juizes.py:1671`, `:1492`; `app/chamados/services/materializacao.py:609`, `:662`, `:669`) | `reabrir_para_correcao`: limpa texto, resposta e assinatura; opcionalmente carimbo; Decisao reabertura/recusa com foto antes de update, acorda celula (`app/chamados/services/disputa_emissao.py:1593`, `:1615`, `:1630`, `:1632`, `:1633`) | **6** calls, incluindo alias: `app/chamados/models.py:2293`; `app/chamados/views.py:1436`; `app/chamados/services/acoes_disputa.py:103`; `app/chamados/services/validacao.py:620`; `app/chamados/services/materializacao.py:688`; command `app/chamados/management/commands/curar_perguntas_em_limbo.py:69` | `app/chamados/tests/test_reabertura_fantasma.py:57::test_reabertura_limpa_o_texto_junto_com_o_carimbo`; `:68::test_nenhum_sitio_reabre_por_fora`; `app/chamados/tests/test_e1_entradas.py:103` |
| pendente/respondida/validada → resolvida por carimbo nao identidade, quando juiz confirma morte | `carimbar` aplica carimbo em memoria e consulta C6; viva recusa/desfaz (`app/chamados/models.py:3174`, `:3185`, `:3195`, `:3199`) | `_escrever_carimbo` atribui materializada_em/por/via; `carimbar` salva, eventualmente completa resposta/validacao e lavra (`app/chamados/models.py:3153`, `:3186`, `:3205`, `:3208`) | **8** calls abaixo | `app/chamados/tests/test_o261e_f2_porta_carimbo.py:665::test_MORDE_encerrar_fantasma_nao_carimba_pergunta_viva`; `:676`, `:687`, `:713` |
| pergunta → carimbada por identidade; duplicata/retratada sao terminais declaradas | Identidade e julgada pelo chamador; `carimbar_identidade` confere conjunto permitido e recusa retorno de fora do fato (`app/chamados/models.py:3224`, `:3234`, `:3236`) | Mesmo `_escrever_carimbo`, opcional recusa_motivo, save (`app/chamados/models.py:3238`, `:3243`, `:3246`) | **5** calls abaixo; **5 vias** (`app/chamados/models.py:2566`) | `app/chamados/tests/test_o261e_f2_porta_carimbo.py:489::test_os_chamadores_de_cada_porta_interna_sao_exatamente_os_declarados`; `:498::test_cada_juiz_de_identidade_grava_a_SUA_via` |
| carimbada → sem carimbo por fantasma/competencia/resposta/sem lastro | Via especifica nos atalhos; C6 em desfazer sem lastro (`app/chamados/models.py:3352`, `:3369`, `:3376`; `app/chamados/services/relavra_via.py:242`) | `descarimbar`: tres campos limpos juntos; fora do fato recusa (`app/chamados/models.py:3259`); lote mesma exclusao (`:3320`) | `reabrir_fantasma` **1** `app/chamados/management/commands/reabrir_orfas_enterradas.py:157`; `reabrir_por_competencia` **1** `app/chamados/services/competencia_trancada.py:279`; `reabrir_por_resposta` **1** `app/chamados/services/disputa_emissao.py:1485`; desfazer sem lastro **1** chamada persistente de descarimbar `app/chamados/services/relavra_via.py:246` | `app/chamados/tests/test_porta_pergunta_identidade.py:223::test_MORDE_reabrir_por_competencia_2x_e_o_mesmo_estado`; `:242::test_MORDE_reabrir_fantasma_2x_e_o_mesmo_estado`; `app/chamados/tests/test_porta_veredito_pergunta.py:408::test_MORDE_carimbo_desfeito_devolve_a_pergunta_para_a_mesa` |
| resolvida muda → pendente, ressuscitacao | Universo proprio, sem texto, fora do fato excluida; marco nao aceso e C6 julga copia como ficaria (`app/chamados/models.py:1530`, `:1550`, `:1553`, `:1556`) | `ressuscitar_perguntas` → descarimbar_em_lote, limpa validacao/resposta, acorda celula e log com IDs/pulados (`app/chamados/models.py:1613`, `:1615`, `:1636`) | **3**: `app/chamados/models.py:1498`; `app/chamados/views_arquivar.py:86`; `app/ponto/services/cartorio.py:1673` | `app/chamados/tests/test_porta_pergunta_identidade.py:301::test_MORDE_ressuscitar_2x_e_o_mesmo_estado_e_UMA_trilha`; `:331::test_MORDE_pergunta_COM_resposta_do_colab_nao_ressuscita` |
| via derivada antiga → via de hoje, carimbo mantido | `via_de_hoje`: declarou hora ou `via_pelo_fato`, somente duas vias derivadas (`app/chamados/services/relavra_via.py:77`, `:95`, `:118`) | `relavrar`: via e lavra no mesmo save, evento antes/depois (`app/chamados/services/relavra_via.py:125`, `:127`, `:132`) | Command **1** `app/chamados/management/commands/relavrar_via_i6.py:102`; wrapper `relavrar_dia` tambem chama `relavrar` (`app/chamados/services/relavra_via.py:157`), mas nao achei chamador do wrapper [nao verificado] | `app/chamados/tests/test_porta_veredito_pergunta.py:343::test_MORDE_relavrar_refaz_o_veredito_no_MESMO_save`; `:395::test_MORDE_relavrar_nao_toca_via_que_e_DECISAO` |
| carimbada sem historia → via fato_materializado | Command exige marco aceso e via sem historia (`app/chamados/management/commands/encerrar_perguntas_com_fato.py:31`, `:40`, `:43`) | `encerrar_por_fato_no_espelho` troca somente via (`app/chamados/models.py:3329`, `:3331`) | **1**, command `app/chamados/management/commands/encerrar_perguntas_com_fato.py:68`; evento `pergunta_encerrada_por_fato` em `:70` | Selo autoriza explicitamente esta porta de via (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:54`, `:238`) |

### Todos os callers de carimbo

**Nao identidade: 8 calls**, cada um atravessa `carimbar` e seu C6 (`app/chamados/models.py:3195`).

1. `encerrar_fantasma`: `app/chamados/models.py:3343`; nascente unica `app/chamados/reconciliador.py:1148`, precedida por `veredito_orfa` (`:1136`), com evento (`:1150`).
2. Absorcao por ausencia: `app/chamados/services/materializacao.py:567`, via declaracao/ausencia_dia (`:555`), Decisao `absorcao` e ligacoes (`:569`).
3. Materializar unidade: `app/chamados/services/materializacao.py:1283`, resposta='validar', lavra; recusa levanta e reverte batida (`:1285`).
4. Pendentes aprovadas S144: `app/chamados/services/materializacao.py:1437`, antes juiz do lastro (`:1433`).
5. Resolver destino: `app/chamados/services/disputa_emissao.py:1986`, resposta='validar', lavra.
6. Fecho sem credor: `app/chamados/services/disputa_emissao.py:2060`, resposta='responder', evento `fecho_sem_credor` (`:2063`).
7. Sem turno aberto: `app/chamados/reconciliador.py:406`, guarda C6 antes (`:399`).
8. Celula absolveu: `app/chamados/reconciliador.py:596`.

**Identidade: 5 calls**, correspondencia literal juiz/via protegida por selo (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:399`).

1. Admin: `app/chamados/models.py:3120`, wrapper `fechar_por_admin`.
2. Duplicata: `app/chamados/services/fio.py:1360`.
3. Paridade: `app/chamados/services/materializacao.py:930`.
4. Fato ocupado/colisao: `app/chamados/services/materializacao.py:1105`.
5. Motivo retratado: `app/chamados/models.py:2117`, com Decisao `retratacao` (`:2112`).

**Wrapper fechar_por_admin: 4 calls**: `app/chamados/models.py:1977`; `app/chamados/services/validacao.py:312`, `:352`; `app/ponto/services/veredito_celula.py:190`.

**Descarimbar_em_lote: 2 calls**: `app/chamados/models.py:1613`; `app/chamados/services/disputa_emissao.py:1630`.

### Upstream corrigido, com contagens exatas

- `gerar_perguntas_disputa`: **5 calls**: `app/chamados/services/disputa_emissao.py:294`, `:670`, `:682`; `app/chamados/services/fabrica_da_celula.py:174`; command `app/chamados/management/commands/smoke_chamados.py:103`.
- `criar_ou_atualizar_disputa`: **5 calls**: `app/api/views_core.py:758`; `app/api/views.py:764`; `app/ponto/services/triagem_batida.py:735`; `app/chamados/services/fabrica_da_celula.py:172`; command `app/ponto/management/commands/marcar_foto_ausente_retro.py:96`.
- `criar_disputa_a_partir_de_chamados_orfaos`: **6 calls**: `app/chamados/services/acoes_disputa.py:222`; `app/chamados/services/cobrar_dia.py:49`; `app/chamados/services/pergunta_no_app.py:246`; commands `app/ponto/management/commands/processar_alertas_turno.py:385`, `app/ponto/management/commands/detectar_ausencias.py:803`, `app/ponto/management/commands/processar_alertas_avancados.py:159`.
- Command de passivo `perguntar_chamados_sem_pergunta` → `pergunta_no_app.perguntar` em `app/chamados/management/commands/perguntar_chamados_sem_pergunta.py:29`, que chama agrupador em `app/chamados/services/pergunta_no_app.py:246`.
- `perguntar_pela_celula`: **3 calls**: `app/ponto/services/cartorio.py:1045`; `app/chamados/services/fabrica_da_celula.py:301`; command `app/chamados/management/commands/backfill_fio_morto_e4b.py:143`.
- `perguntar_por_eventos`: **1** `app/chamados/reconciliador.py:1388`, dentro do consumidor.
- `desfazer_carimbo_sem_lastro`: **2** `app/chamados/services/fabrica_da_celula.py:169`, `:209`.
- `resolver_perguntas_por_chamado_resolvido`: **5 calls**: `app/chamados/signals.py:117`; `app/chamados/reconciliador.py:537`, `:1649`; `app/chamados/services/becos.py:54`; command `app/ponto/management/commands/reconciliar_fantasmas.py:281`.
- `materializar_perguntas_validadas_da_disputa`: **4 calls**: `app/chamados/services/fio.py:670`; `app/chamados/services/acoes_disputa.py:139`; `app/chamados/services/validacao.py:492`; command `app/chamados/management/commands/backfill_fio_morto_e4b.py:147`.
- `materializar_pendentes_aprovadas`: **1**, receiver S144 `app/chamados/signals.py:165`.
- S143 e `post_save` de chamado encerrado, condicionado a escrita de status (`app/chamados/signals.py:91`, `:97`, `:105`); S144 e `post_save` de disputa fechada (`app/chamados/signals.py:149`, `:155`). Sitios apenas, sem mapear essas outras entidades.
- Nao achei funcao de producao chamada `validar_respostas` [nao verificado]; o lote lido usa `validar_pergunta` (`app/chamados/services/validacao.py:773`, `app/chamados/management/commands/validar_classe_a.py:100`).

### Crons relevantes

- Produtores: alertas turno */5 (`app/config/crons.py:362`), avancados */30 (`:363`), detectar ausencias */5 (`:364`), consumidor de celula por minuto (`:394`), cartorio --apply 06:27 (`:616`).
- `reconciliar_fantasmas` e `reconciliar_chamados` agendados como **--alarme**, nao executar (`app/config/crons.py:392`, `:395`).
- `disparar_perguntas_competencia` encadeado com **--alarme** (`app/config/crons.py:1539`), declarado vigia (`:2062`).
- `reconciliar_perguntas_orfas` encadeado com **--alarme** (`app/config/crons.py:1544`), carimbo saiu para evento (`:2094`).
- `encerrar_orfas_pela_lampada` e chamado no consumidor (`app/chamados/reconciliador.py:1376`).

### L2/L3: cuidados para consolidacao

- Nao chamar `relavra_via.relavrar` ou `encerrar_por_fato_no_espelho` de escritor clandestino automaticamente: o selo declara **4 funcoes permitidas para via**, incluindo ambas (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:51`). O selo de carimbo combate atribuicao, setattr, update, bulk etc. (`:99`, `:260`, `:270`, `:296`).
- `carimbar` tambem assina resposta/validacao fora do helper humano, mas declara esse ato expressamente (`app/chamados/models.py:3162`, `:3186`); nao e descoberta clandestina por si.
- **2 vias fora do fato terminais declaradas**: duplicata e motivo_retratado (`app/chamados/models.py:2449`, `:2575`, `:2580`, `:3228`). As portas de volta recusam (`app/chamados/models.py:3262`, `:3320`), carimbo comum tambem (`:3176`), identidade nao retorna ao fato (`:3236`).
- A terminacao e de identidade, nao do rotulo `resolvida`, que tem varias operacoes de volta acima (`app/chamados/models.py:3345`, `:3373`, `:1559`).
- Nota importante: `carimbar_identidade` permite trocar uma via fora do fato por outra via fora do fato (`app/chamados/models.py:3236`); logo “nenhuma operacao sai” precisa usar unidade estado/identidade, nao afirmar imutabilidade absoluta de `via_resolucao`.

## 14/11 21:44 · sessao cfbe76f129cd · - · - · origem=Ronald (tmux)

Inventario consolidado, somente leitura; caminhos relativos a `app/`.

- **`criar_ausencia`: 9 sitios de chamada**, incluindo wrappers internos: `api/views.py:1805`; `ponto/views.py:2447`, `:2568`; `chamados/services/materializacao.py:491`, `:527`; `ponto/services/veredito_celula.py:337`; `ponto/services/checar_ausencia.py:46`; `ponto/services/ausencia.py:523`, `:1275`.
- **`garantir_ausencia`: 2 sitios**: `ponto/services/regularizacao.py:118`; `ponto/management/commands/marcar_abandono_provisorio.py:76`. Ambos pedem `falta` sem status explicito (`regularizacao.py:119`, `:122`; comando `:79`, `:83`).
- **`anexar_documento`: 3 sitios**: painel DP `ponto/views.py:2158`; endpoint proprio `api/views.py:3110`; reenvio do nascimento API, somente quando falta anexo na existente, `api/views.py:1836`, `:1838`.
- **`decidir`: 1 caller direto**, wrapper `_a4_decidir`, `ponto/views.py:88`; **7 callers do wrapper**: aprovar lote `:2301`, rejeitar lote `:2337`, cancelar lote `:2377`, aprovar individual/irmaos `:2675`, editar periodo de aprovada `:2708`, rejeitar individual/irmaos `:2736`, cancelar individual `:2772`. A edicao do periodo conserva o estado `aprovada` (`ponto/views.py:2705`; `ponto/services/ausencia.py:883`).
- **`editar_ausencia_registro`: 2 sitios**: `ponto/views.py:2834`; comando `rederivar_minutos_h06_comp11.py:169`. O comando passa `tipo=a.tipo`, portanto nao entra no rejulgamento condicionado a troca de tipo (`ponto/services/ausencia.py:591`).
- **`rejulgar_status_pelo_tipo`: 2 sitios**: edicao `ponto/services/ausencia.py:592`; comando `ponto/management/commands/rejulgar_status_por_tipo.py:35`. O comando so processa `aguardando_documento`; o inverso fica apenas contado (`:25`, `:36`).
- **`reverter_rejeicao`: 1 sitio**: `ponto/views.py:2616`; **`_reabrir`: 2 sitios**, nascimento sobre rejeitada `ponto/services/ausencia.py:350` e reversao explicita `:1172`.
- **`mudar_estado`: 5 sitios internos**: rejulgamento `ponto/services/ausencia.py:609`; anexo `:940`; decisao `:978`; vencimento do prazo `:1071`; reabertura `:1211`.
- **`escalonar_aguardando_documento`: 1 sitio**, comando `ponto/management/commands/escalonar_documentos_ausencia.py:26`; **1 cron declarado**, diario `07:51 --apply`, `config/crons.py:715`. Instalacao/executacao real: [nao verificado].
- **`espelhar_ferias`: 1 caller direto**, `ferias/models.py:394`, disparado pelo `save` do agendamento `:449`; **`rejeitar_ausencias_de_ferias`: 1 caller**, `ferias/services.py:683`; **`apagar_ausencias_de_ferias`: 2 callers**, `ferias/models.py:453`, `ferias/services.py:725`.

Nascentes externas dos wrappers de nascimento:

- Regularizacao: `ponto/views.py:3255`, `api/views.py:2854`, `api/views_core.py:1175` → `ponto/services/regularizacao.py:118`.
- Copiloto: `core/views_ia.py:985` → `ponto/services/checar_ausencia.py:46`, com `status='aprovada'` (`:47`); dispatch da intencao em `core/views_ia.py:154`.
- Veredito: `ponto/views.py:3380` e `ponto/services/declarar_dia.py:215` → `ponto/services/veredito_celula.py:337`, aprovado explicitamente (`:338`). Declaracao de falta vem do dispatch em `declarar_dia.py:167`, alcançado dinamicamente por `core/views_ia.py:775`; resolucao da funcao em `core/intake.py:481`, tabela em `:92`.
- Materializacao de escolha: `chamados/services/materializacao.py:1084` → `:491`; fachada externa `materializar_perguntas_validadas_da_disputa` chama a unidade em `:1382`, que despacha escolha em `:1232`. Callers da fachada: `chamados/services/acoes_disputa.py:139`, `fio.py:670`, `validacao.py:492`, `chamados/management/commands/backfill_fio_morto_e4b.py:147`.
- Materializacao de escape: `chamados/services/disputa_emissao.py:1541` → `materializacao.py:527`; tabela fixa `FALTA_DIA → falta/aprovada`, `ATESTADO → atestado/estado_inicial` em `materializacao.py:603`. Nascentes HTTP da resposta: `chamados/views.py:1283`, `api/views.py:2620`. Outros callers do core passam horas: `acoes_disputa.py:371`, `declaracao_texto.py:419`, `:693`.
- Ferias: cancelamento humano chama o servico por `ferias/views.py:396`, `:444`, `:605`, via wrapper `:561`; rejeicao em massa de ausencia em `ponto/services/ausencia.py:1355`, seguida do `save` do agendamento em `ferias/services.py:689`, que dispara remocao pelo espelho (`ponto/services/ausencia.py:1262`).
- Cron de ferias: `config/crons.py:442`, diario `06:15 --apply` → `ferias/management/commands/sincronizar_status_ferias.py:49` → `ferias/services.py:956` → gancho `ferias/models.py:449` → espelho. Pode criar ausencias faltantes por `ponto/services/ausencia.py:1275`.

Comandos sem cron rotineiro declarado:

- `marcar_abandono_provisorio`, classificado como acao sob ordem em `config/crons.py:1422`; o dry-run desfaz a transacao em `ponto/management/commands/marcar_abandono_provisorio.py:93`, `:98`.
- `rejulgar_status_por_tipo`, explicitamente nao cron em `config/crons.py:1203`.
- `rederivar_minutos_h06_comp11`, ato unico em `config/crons.py:1385`.
- `backfill_fio_morto_e4b`, ato unico em `config/crons.py:786`.

Migracao historica que muda estado:

- **1 arquivo / 2 escritores**, `ponto/migrations/0057_a4_backfill_pendente.py`: forward escolhe `pendente → aguardando_decisao` ou `aguardando_documento` pelo anexo/lista historica (`:30`, `:35`, `:37`, `:40`), escreve por `queryset.update` em `:43`, deixa `LogAuditoria` em `:44`.
- Reverse leva ambas abertas a `pendente`, limpa prazo e nao escreve trilha (`:58`, `:63`, `:65`). `RunPython` registra os dois sentidos em `:78`.
- `0055_a4_estados_com_porta.py:21` e `0056_a4_default_estado.py:16` alteram esquema/default. `0058_a4_relogio_recomeca.py:42`, `:63` e `0059_a4_d0_no_primeiro_dia_do_cron.py:36` alteram prazo, sem mudar status.

Achados relevantes:

- **`rejeitada → aprovada`: 0 nascente efetiva encontrada**; existe na matriz (`ponto/services/ausencia.py:839`), mas aprovacao humana exige `ABERTAS` (`ponto/views.py:2293`, `:2638`, `:2650`), edicao de periodo exige `aprovada` (`:2705`), e reabertura deriva somente estados abertos de `estado_inicial` (`ponto/services/ausencia.py:1189`, `:855`).
- A submissao web veta qualquer sobreposicao antes da porta (`ponto/views.py:2556`), enquanto a porta admite abono sobre falta segundo `linha_que_veta` (`ponto/services/ausencia.py:381`).
- O juiz inicial roda em `ponto/services/ausencia.py:392`, mas `status` explicito vence em `:403`; nascentes que forcam aprovada: web admin `ponto/views.py:2452`, copiloto `checar_ausencia.py:47`, veredito `veredito_celula.py:338`, espelho `ausencia.py:1277`, escape `FALTA_DIA` (`materializacao.py:604`).
- Estado legado `pendente` pode ser escrito pelo reverse da migracao (`0057_a4_backfill_pendente.py:65`); nao tem entrada na matriz atual (`ponto/services/ausencia.py:831`). Possui saida historica no forward da mesma migracao (`:43`).

## 14/11 21:44 · sessao 3224b63d0efd · - · - · origem=Ronald (tmux)

Auditoria independente concluida, apenas leitura.

- **L3 recomendado: 0 estados terminais de COMPETENCIA demonstrados.** Aprovacao sai por `reabrir`/`reabrir_lote`; tranca sai por `reabrir_periodo`; exportacao admite nova versao. `invalidada` e terminal do registro historico de TXT, nao da competencia. `app/ponto/services/fechamento.py:1344`, `app/ponto/services/fechamento.py:1432`, `app/folha/models.py:174`.
- Os **4 marcos sao booleanos cumulativos**, nao estados mutuamente exclusivos: `aprovada`, `exportada`, `trancada`, `paga`; `marco` devolve o verdadeiro mais adiantado e `aberta` quando nenhum existe. Nao desenhar uma cadeia obrigatoria: a lista `violacoes` somente conta ausencias do marco anterior. `app/ponto/services/fechamento.py:1461`, `app/ponto/services/fechamento.py:1532`, `app/ponto/services/fechamento.py:1542`.
- `exportado` existe nos choices de `FechamentoMensal`, mas nao encontrei nascente de producao que atribua esse valor; nao incluir como estado terminal alcancavel. A aprovacao consulta tanto `aprovado` quanto `exportado`. `app/ponto/models.py:399`, `app/ponto/services/fechamento.py:1533`. Ausencia global de escritor: **[nao verificado]**.

**Tranca e reabertura da competencia**

| transicao | nascentes / juizes | escritor / persistencia | leitores / prova |
|---|---|---|---|
| nao trancada → trancada, primeira vez | `aprovar_fechamento`: somente empresa informada e nenhum pulado; chamada `app/ponto/views.py:1369`; `trancar_periodo` decide `criado or reaberto_em != None` em `app/ponto/services/fechamento.py:1411` | `get_or_create`, chave empresa/mes/ano, `fechado_por`, `ativo=True`; `app/ponto/services/fechamento.py:1408` | `periodo_trancado`: `reaberto_em IS NULL`, `app/ponto/services/fechamento.py:1470`; `competencia_trancada`, `app/ponto/services/fechamento.py:1480`; `app/folha/tests/test_porta_folha.py::test_trancar_periodo_2x_e_uma_linha`, linha 74 |
| reaberta → trancada | mesma nascente `app/ponto/views.py:1370`; ramo `app/ponto/services/fechamento.py:1412` | `ativo=True`, novo fechado_por/em, limpa reaberto_por/em/motivo, save; `app/ponto/services/fechamento.py:1413` | mesmos leitores; `app/ponto/tests/test_marco_da_competencia.py::test_MORDE_fechada_e_trancada_e_nao_aprovada`, linha 39 cobre ciclo de abertura/tranca/reabertura |
| trancada → reaberta, aprovados → abertos | view exige POST, permissao `reabrir_periodo`, motivo nao vazio, periodo ainda trancado; `app/ponto/views.py:1525`, `app/ponto/views.py:1527`, `app/ponto/views.py:1539`, `app/ponto/views.py:1543` | `ativo=False`, reaberto_por/em/motivo, save; depois update dos aprovados para aberto e limpa aprovado_por/em; `app/ponto/services/fechamento.py:1437`, `app/ponto/services/fechamento.py:1449`; LogAuditoria fica na view `app/ponto/views.py:1548` | mesmos leitores; `app/folha/tests/test_porta_folha.py::test_reabrir_periodo_devolve_os_aprovados_e_a_2a_nao_grava`, linha 81 |

Nenhuma outra chamada de producao a essas duas portas apareceu na busca, incluindo commands e crons; censo exaustivo de chamadas dinamicas: **[nao verificado]**.

**L2: distinguir campo de estado, existencia da linha e escrita dos valores**

- Portas declaradas por modulo: `FechamentoMensal`/`PeriodoFechado` em `ponto/services/fechamento.py`; `ExportacaoDominio` em `folha/models.py`. `app/core/portas.py:500`, `app/core/portas.py:505`.
- O selo percorre apenas `app`, com allowlist vazia. Portanto nao protege `bin`. `app/folha/tests/test_chokepoint_folha_gate.py:26`, `app/folha/tests/test_chokepoint_folha_gate.py:30`, `app/folha/tests/test_chokepoint_folha_gate.py:58`.
- Ha **2 saves fora da porta**, mas seus campos excluem `status`, aprovado_por/em, previsto_em e derivado_em: `VALOR` vem de `CAMPOS` numericos. Assim nao sao achados estritos de escrita do campo status. `bin/restore_relavratura_10_2026.py:34`, `bin/restore_relavratura_10_2026.py:80`, `bin/restore_o263_comp10.py:27`, `bin/restore_o263_comp10.py:49`, `app/ponto/management/commands/aplicar_09_corte_b.py:67`.
- Ha **2 deletes de FechamentoMensal fora da porta**, executados quando `DRY=False`; removem linhas inteiras sem filtrar estado e podem retirar o ultimo testemunho de aprovacao da empresa. Devem aparecer como desvio da existencia da projecao, sem chama-los de update do campo status. `bin/restore_relavratura_10_2026.py:31`, `bin/restore_relavratura_10_2026.py:68`, `bin/restore_relavratura_10_2026.py:83`, `bin/restore_o263_comp10.py:23`, `bin/restore_o263_comp10.py:39`, `bin/restore_o263_comp10.py:52`; leitor do marco `app/ponto/services/fechamento.py:1533`.
- `restaurar_fechamento` declara que nao toca status/aprovado_por, mas aceita qualquer campo existente e exclui somente mes/ano/colaborador_id/id: tecnicamente pode receber status/carimbos. Nao encontrei chamada passando esses campos; o command `desvio_o68b` aceita snapshot JSON externo e encaminha suas chaves. Registrar como **capacidade sem ocorrencia comprovada**, nao transicao medida. `app/ponto/services/fechamento.py:1370`, `app/ponto/services/fechamento.py:1374`, `app/ponto/services/fechamento.py:1380`, `app/ponto/management/commands/desvio_o68b.py:148`, `app/ponto/management/commands/desvio_o68b.py:196`.

**L1 / juizes relevantes**

- A aprovacao e decidida por `classificar_export` e pelo modo selecionado que dispensa `conta_vazia`, antes dos escritores sem guardas de aprovacao. A view usa filtro `status='aberto'`; a porta `aprovar_lote` depende desse filtro do chamador para idempotencia. `app/ponto/views.py:1299`, `app/ponto/views.py:1307`, `app/ponto/views.py:1311`, `app/ponto/views.py:1365`, `app/ponto/services/fechamento.py:1328`.
- Trancar depende do resultado anterior da aprovacao e do predicado `empresa_id and not _pulados`, mais o predicado de criacao/retranca da propria porta. `app/ponto/views.py:1369`, `app/ponto/services/fechamento.py:1411`.
- Reabrir periodo tem juizes de permissao, motivo e existencia da tranca na view; a porta escreve sem repetir esses vetos. `app/ponto/views.py:1527`, `app/ponto/views.py:1539`, `app/ponto/views.py:1543`, `app/ponto/services/fechamento.py:1437`.
- Ha duas funcoes lendo identicamente a verdade da tranca: `periodo_trancado` e `competencia_trancada`, ambas usando `reaberto_em IS NULL`; `ativo` e documento. Isso e duplicacao literal do predicado de leitura, nao necessariamente duas decisoes independentes de transicao. `app/ponto/services/fechamento.py:1464`, `app/ponto/services/fechamento.py:1473`.

**Limites importantes para L3**

- A unica nascente de producao encontrada para `invalidar` passa sempre `substituida_por=novo`, depois de registrar o novo TXT. Revogar sem substituta e capacidade da porta e teste, sem nascente produtiva encontrada. `app/folha/management/commands/regerar_txt_dominio.py:120`, `app/folha/management/commands/regerar_txt_dominio.py:122`, `app/folha/models.py:143`, `app/folha/tests/test_k6_competencia_entregue.py:72`.
- A marca `paga` tambem nao e terminal comprovado: depende de Holerite ligado a lote publicado, e existe sitio de outra entidade que substitui documentos antigos por avulsos sem lote; citar somente o sitio. `app/ponto/services/fechamento.py:1539`, `app/holerite/views.py:1191`, `app/holerite/views.py:1193`, `app/holerite/views.py:1255`.
- Origem de `paga`, somente sitio cruzado: `app/holerite/management/commands/processar_lote_holerite.py:126`.

O censo AST foi executado sem Django e sem bytecode: retornou 14 sitios atribuiveis dessas tres entidades dentro de `app`, nenhum fora dos modulos declarados; nao e censo completo, pois nao reconheceu `cls.objects.create` nem varios querysets recebidos como parametros. O codigo declara essas limitacoes de atribuicao em `app/core/censo_escritas.py:8`, `app/core/censo_escritas.py:43`, `app/core/censo_escritas.py:53`; resultado da execucao sem artefato arquivo:linha: **[nao verificado]**.

## 14/11 21:44 · sessao 6f1fc616c034 · - · - · origem=Ronald (tmux)

Consolidacao CelulaDia; caminhos abaixo relativos a `app/`.

**Campos e contagens.** Existem 2 origens (`gerada`,`editada`), `trabalha` permite True/False/None, 10 vereditos de cartorio e 3 humanos; `dna` e snapshot JSON e `dna_anterior` guarda uma versao; unicidade e `(colaborador,data)` (`escala/models.py:1686`, `:1690`, `:1697`, `:1726`, `:1751`, `:1759`, `:1833`). DNA corrente tem versao 2 e 8 chaves; pode nomear vinculo nulo (`ponto/management/commands/gerar_celulas.py:16`, `:17`, `:80`).

Censo AST executado somente em memoria, com Python3 `-B`: **10 sitios atribuiveis** a CelulaDia, todos dentro da porta: `ponto/portas/celula.py:265`, `:279`, `:293`, `:310`, `:638`, `:647`, `:836`, `:914`, `:926`, `:1237`; soma-se o escritor generico lido `salvar_e_emitir`, `:963`, que o AST nao atribui a entidade. **0 escritores atribuiveis fora da porta**; o censo tem limites expressos para receptores desconhecidos (`core/censo_escritas.py:43`, `:54`); a arvore inteira produziu 132 escritas nao atribuiveis, portanto zero atribuivel nao prova ausencia absoluta [nao verificado]. Selo que exige allowlist vazia: `ponto/tests/test_chokepoint_celula_gate.py:22`, `::ChokepointCelulaGate.test_MORDE_nenhuma_escrita_de_celula_fora_da_porta`, `:50`.

**Tabela sugerida** (cada linha corresponde a mutacao; recusa/no-op nao vira transicao). “R” abaixo: leitores `EscalaColaborador.eh_dia_trabalho` (`escala/models.py:1094`), `marcos_do_dia` (`:1124`), juiz de vinculo pela geradora (`escala/alimentacao.py:269`), grade (`escala/services/leitor_celula.py:350`), reescrita (`escala/services/regeneracao.py:72`), status (`ponto/status_dia.py:201`).

| # | estado de partida -> estado de chegada | juiz (arquivo:linha) | operacao (funcao) | escritor (arquivo:linha) | o que persiste (campos/modelos/trilha) | leitores (arquivo:linha) | prova (teste ou selo que morde, arquivo::nome, ou "nenhum") | [nao verificado]
|---|---|---|---|---|---|---|---|
| C1 | inexistente -> origem=gerada, trabalha=True/False/None, DNA | cobertura e candidatura (`ponto/portas/celula.py:883`, `:906`); calculo (`:911`) | gerar_celulas_periodo; nascentes G abaixo | `ponto/portas/celula.py:914` | CelulaDia, geradora, DNA, gerada_em; nao emite EventoCelula neste create (`:914`) | R | `ponto/tests/test_porta_celula.py::test_MORDE_gerar_periodo_2x_e_a_mesma_celula`, `:36` |
| C2 | gerada passada, dna=None -> gerada com DNA preenchido | origem, passado e DNA nulo (`ponto/portas/celula.py:919`, `:921`, `:924`) | gerar_celulas_periodo; G | `ponto/portas/celula.py:926` | so dna; preserva trabalha e origem (`:922`, `:925`) | R | selo estrutural `ponto/tests/test_chokepoint_celula_gate.py::test_MORDE_nenhuma_escrita_de_celula_fora_da_porta`, `:50`; comportamental especifico nao localizado [nao verificado] |
| C3 | inexistente -> gerada por regeneracao | vigencia declarada, cobertura, lavra (`ponto/portas/celula.py:153`, `:229`, `:284`) + calculo (`:289`) | regenerar_celulas_vinculo / wrapper regenerar_celulas_dia; Rg abaixo | `ponto/portas/celula.py:293` | CelulaDia; trilha agregada regenerar_celulas_vinculo (`:369`); rabo comum (`:392`) | R | `ponto/tests/test_porta_celula_dia.py::test_pedir_UM_dia_toca_UMA_celula`, `:80` |
| C4 | gerada com desenho anterior -> gerada com trabalha/geradora/DNA recalculados | somente gerada e divergente (`ponto/portas/celula.py:297`); guardas C3; futuros filtram dono (`ponto/services/cadastro_futuro.py:37`) | regenerar_celulas_vinculo / regenerar_celulas_dia; Rg | `ponto/portas/celula.py:310` | trabalha/geradora/dna, dna_anterior, regenerada_em, regeneracoes+1 (`:305`); trilha agregada (`:373`); futura desfaz julgamento (`:314`) | R | `ponto/tests/test_porta_celula.py::test_MORDE_regenerar_2x_nao_sobe_regeneracoes_e_deixa_uma_trilha`, `:46`; `escala/tests/test_cobranca_orfa_regeneracao.py::test_reescrita_deixa_marca_na_celula_e_guarda_o_dna_anterior`, `:146` |
| C5 | gerada passada fora da vigencia, trabalha!=False -> gerada trabalha=False, DNA sem marcos | passado, mesma geradora, sem outro vinculo, sem lavra (`ponto/portas/celula.py:272`) | regenerar_celulas_vinculo; Rg | `ponto/portas/celula.py:279` | **somente trabalha e dna; nao atualiza dna_anterior/regenerada_em/regeneracoes neste ramo** (`:277`); trilha agregada (`:373`) | R | nenhum especifico localizado [nao verificado] |
| C6 | gerada futura fora da vigencia -> gerada trabalha=False, geradora=None, DNA sem vinculo | futuro/mesma geradora/sem outro vinculo/sem lavra (`ponto/portas/celula.py:251`) | regenerar_celulas_vinculo; Rg | `ponto/portas/celula.py:265` | trabalha=False, geradora=None, dna=montar_dna(None), dna_anterior, relogio/contador; reset julgamento (`:258`, `:267`) | R | testes familia `ponto/tests/test_cadastro_futuro_c5d_banco.py::test_MORDE_trilha_que_nao_grava_DESFAZ_a_limpeza`, `:38`; prova especifica do DNA sem vinculo [nao verificado] |
| C7 | gerada com dna_anterior -> gerada com DNA anterior restaurado | origem gerada, anterior presente e lavra (`ponto/portas/celula.py:818`, `:820`) | reverter_regeneracao_dia; nascente aplicar_col900 (`ponto/management/commands/aplicar_col900.py:186`) | `ponto/portas/celula.py:836` | restaura DNA/geradora/trabalha, limpa dna_anterior, contador+1, trilha antes/depois, EventoCelula (`:827`, `:838`, `:846`) | R | `ponto/tests/test_porta_celula_dia.py::test_reverter_devolve_o_dia_ao_dna_anterior`, `:181`; `::test_MORDE_reverter_2x_NAO_faz_ping_pong`, `:195`; `::test_MORDE_reverter_respeita_a_MESMA_guarda_de_lavra_da_ida`, `:227` |
| C8 | inexistente ou existente -> origem=editada com desenho humano | autor/motivo/bool, lavra, vinculo e idempotencia (`ponto/portas/celula.py:597`, `:606`, `:622`, `:632`) | editar_dia; unica chamada em producao e interna a trocar_plantao (`:713`); nascente externa **nao localizada** [nao verificado] | create `ponto/portas/celula.py:638`; save `:647` | trabalha/dna/geradora/origem/editada_em/editada_por; trilha antes/depois+motivo obrigatoria, rollback se falhar (`:649`, `:657`); nao finge regeneracao (`:576`) | R | `ponto/tests/test_o10b_editar_dia_troca.py::test_sem_autor_ou_sem_motivo_nao_escreve`, `:465`; `::test_editar_2x_e_um_estado_e_uma_trilha`, `:473`; `::test_CUSTO_celula_editada_fica_imune_a_regeneracao`, `:490` |
| C9 | par de celulas -> cedente editada/folga + substituto editada/trabalho | autor/motivo, dois lados, documentos, pessoas distintas, edicoes aceitas (`ponto/portas/celula.py:684`, `:691`, `:700`, `:702`, `:714`) | trocar_plantao; **0 chamadas externas localizadas em producao** [nao verificado] | mesmo editar_dia (`ponto/portas/celula.py:713`) | duas edicoes e trilha trocar_plantao no atomic (`:690`, `:724`); falha desfaz par (`:715`) | R | `ponto/tests/test_o10b_editar_dia_troca.py::test_A02_falha_entre_os_lados_nao_deixa_meio_par`, `:325`; `::test_B06_segunda_confirmacao_do_mesmo_par_e_idempotente`, `:238` |
| C10 | sem veredito ou cartorio anterior -> veredito cartorio e status da lavra | derivar_veredito (`escala/models.py:1624`); veredito_que_a_lavra_escreve (`ponto/portas/celula.py:970`); status_alvo (`:1009`) | lavrar_veredito; nascentes Ct abaixo | `ponto/portas/celula.py:1092` -> salvar_e_emitir `:963` | veredito/via/em/protestos/ata/status; LogAuditoria veredito_celula (`:1100`), EventoCelula (`:965`) | status `ponto/status_dia.py:201`; R; outbox e outro sitio `chamados/reconciliador.py:703` | `ponto/tests/test_porta_celula.py::test_MORDE_lavrar_2x_e_um_evento_e_o_relogio_nao_anda`, `:80`; `ponto/tests/test_o_outbox_celula.py::test_save_e_evento_entram_juntos_ou_nenhum`, `:142` |
| C11 | qualquer veredito -> trabalhou/faltou/justificado via admin | dominio humano e despachante (`ponto/services/veredito_celula.py:221`); porta (`ponto/portas/celula.py:1071`); status humano (`ponto/status_dia.py:65`) | veredito_celula -> cel.lavrar_veredito (`ponto/services/veredito_celula.py:352`); nascentes H abaixo | `ponto/portas/celula.py:1093` -> `:963` | via admin/em/veredito/status; preserva protestos anteriores (`:1074`), trilha da porta (`:1100`) e despachante (`ponto/services/veredito_celula.py:356`) | R; cartorio preserva decisao (`ponto/portas/celula.py:1061`) | `ponto/tests/test_o_outbox_celula.py::test_ata_da_celula_de_admin_acorda_o_consumidor`, `:132`; demais provas humanas `ponto/tests/test_veredito_celula.py` [nomes especificos nao lidos] |
| C12 | veredito existente, status ausente/antigo -> mesmo veredito com status lavrado | celula em dia e sem status (`ponto/services/cartorio.py:927`, `:930`); regra (`ponto/portas/celula.py:1075`) | _completar_status -> lavrar_veredito; command lavrar_status_competencia (`ponto/management/commands/lavrar_status_competencia.py:56`) | `_aplicar_status` `ponto/portas/celula.py:1028` -> `salvar_e_emitir :1083` | so status_dia/status_rotulo; nao move veredito_em nem emite outbox (`:1080`); trilha (`:1085`) | `ponto/status_dia.py:201` | selo AST referido no modelo `escala/models.py:1811`; especifico nao lido [nao verificado] |
| C13 | futura com julgamento cartorio -> campos do julgamento=None | futuro, algo julgado, sem decisao admin (`ponto/portas/celula.py:1146`, `:1149`, `:1155`) | desfazer_julgamento_da_futura; nascentes internas da regeneracao `:267`, `:314` e reversao `:842` | setattr `ponto/portas/celula.py:1166` -> salvar_e_emitir `:1167` | 13 campos limpos definidos `:1112`; EventoCelula; trilha obrigatoria julgamento_da_futura_desfeito (`:1168`); falha reverte (`:1172`) | R | `ponto/tests/test_cadastro_futuro_c5d_banco.py::test_MORDE_trilha_que_nao_grava_DESFAZ_a_limpeza`, `:38`; `::test_conservado_sem_trilha_recusa`, `:56` |
| C14 | carimbo ausente/antigo -> carimbo dos insumos atuais | diferenca de impressao/ausencia/tem_* ou lavra (`ponto/portas/celula.py:1190`) | carimbar; cartorio dois ramos (`ponto/services/cartorio.py:1314`, `:1873`) | `ponto/portas/celula.py:1194` -> `:1201` | julgada_em/impressao/ausencia/tem_entrada/tem_saida/insumos_em junto com lavra; carimbo e propria trilha (`:1185`) | cartorio compara impressao (`ponto/services/cartorio.py:1260`); R | `ponto/tests/test_porta_celula.py::test_MORDE_carimbar_2x_e_um_carimbo`, `:96` |
| C15 | ata sem/com precedencia antiga -> ata com precedencia atual | fatos_do_dia (`ponto/services/cartorio.py:1998`); diferenca (`ponto/portas/celula.py:1210`) | aplicar_precedencia_na_celula -> carimbar_precedencia (`ponto/services/cartorio.py:2000`); depois julgar_celula (`:2048`) | `ponto/portas/celula.py:1214` -> `:1216` | somente chave ata.precedencia; preserva outras chaves; EventoCelula (`:1212`) | R | `ponto/tests/test_porta_celula.py::test_MORDE_precedencia_2x_e_um_save`, `:111`; `::test_MORDE_a_lavra_do_cartorio_nao_apaga_a_precedencia`, `:122` |
| C16 | insumos nao carimbados/antigos -> insumos carimbados | igualdade do lote (`ponto/portas/celula.py:1229`) | carimbar_insumos_em_lote; command backfill_b5_insumos (`ponto/management/commands/backfill_b5_insumos.py:81`, `:84`) | `ponto/portas/celula.py:1237` | ausencia/tem_entrada/tem_saida/insumos_em; sem outbox neste bulk_update (`:1237`) | R | `ponto/tests/test_porta_celula.py::test_MORDE_insumos_em_lote_2x_e_um_carimbo`, `:159` |

**Nascentes G (2 chamadas externas).** `gerar_celulas` (`ponto/management/commands/gerar_celulas.py:349`), acionado 05:50 (`config/crons.py:426`); `curar_residuo_bug115` (`ponto/management/commands/curar_residuo_bug115.py:138`). Cron tambem regenera **HOJE existente** por `_regenerar_hoje_existentes`, chamada `:347` e regeneracao `:323`; futuras existentes apenas puladas (`ponto/portas/celula.py:930`).

**Nascentes Rg (14 chamadas a regenerar_celulas_vinculo, 13 externas + wrapper interno).** `colaboradores/services/vinculo.py:328` (alias `_regen`), `:904`, `:956`; `colaboradores/management/commands/corrigir_escala_retroativa.py:231`; `ponto/management/commands/gerar_celulas.py:323`; `ponto/services/cadastro_futuro.py:44`; `ponto/portas/regen_exportada.py:179`; `escala/folgas.py:293`; `escala/signals.py:73`, `:125`; `escala/services/escala_auto_executor.py:107`, `:147`; `escala/services/cadastro_tipo.py:353`; wrapper `ponto/portas/celula.py:773`. Regenerar dia tem 2 nascentes: `ponto/management/commands/aplicar_col900.py:234`, `diff_col900.py:72`; reverter tem 1: `aplicar_col900.py:186`.

**Eventos que chegam por futuras.** `regenerar_futuras_do_cadastro` e seletor, escritor continua Rg (`ponto/services/cadastro_futuro.py:9`, `:44`). Chamadores: `escala/signals.py:81`, `:132`; `colaboradores/services/vinculo.py:338`, `:662`, `:750`, `:910`, `:961`, `:1035`; `colaboradores/management/commands/corrigir_escala_retroativa.py:243`; `escala/services/cadastro_tipo.py:376`; `escala/folgas.py:298`, `:640`; `ponto/services/cadastro_futuro.py:77`. Este ultimo recebe feriado por `core/signals.py:35`, `:43`. Foto de folgas chega por `escala/folgas.py:215`, `:337`, `:380`, `:418`, `:523` e `escala/views.py:416`, passando por `relavrar_celulas_da_foto :247` -> Rg `:293`; cron da foto `config/crons.py:425`.

**Nascentes Ct da lavra cartorio.** Porta e chamada em `ponto/services/cartorio.py:934` (status), `:1292` (ramo sem batidas), `:1853` (normal). Entradas de producao `julgar_colab`: `ponto/management/commands/processar_cartorio.py:65`, `celula_veredito_velho.py:144`, `lavrar_status_competencia.py:56`; `escala/regua_defesa.py:692`; `ponto/services/cartorio.py:2040` (unitario). Entradas unitarias `julgar_celula`: `chamados/signals.py:218`, `ponto/signals.py:93`, `colaboradores/services/pos_vinculo.py:73`, `ponto/portas/celula.py:472`, `chamados/services/cobrar_dia.py:41`, `ponto/services/furos_sem_cobranca.py:128`, `ponto/services/vigia_de_hora.py:131`, `chamados/reconciliador.py:703`, `ponto/management/commands/rejulgar_ausencia_em_folga.py:78`, `curar_residuo_bug115.py:146`. Crons: cartorio 06:27 (`config/crons.py:616`), vigia de hora */10 (`:367`). Juiz unitario recusa futura/inexistente e hoje antes fim de turno, salvo teto (`ponto/services/cartorio.py:2025`, `:2028`, `:2033`).

**Nascentes H da decisao humana.** View alias `_vc` (`ponto/views.py:3354`, chamada `:3380`), e falta declarada (`ponto/services/declarar_dia.py:215`); intake aponta declaracao por caminho pontilhado (`core/intake.py:78`, `:92`), nao editar_dia. `veredito_celula` chama lavra humana em `ponto/services/veredito_celula.py:352`.

**L1 candidatos, 7 familias de transicao.**
1. C1: candidatura/cobertura/calculo (`ponto/portas/celula.py:883`, `:906`, `:911`; predicados `escala/alimentacao.py:189`, `:205`; calculo `escala/models.py:1252`).
2. C3/C4: vigencia/lavra/cobertura/calculo/origem+diferenca (`ponto/portas/celula.py:153`, `:218`, `:229`, `:289`, `:297`), mais seletor de dono futuro (`ponto/services/cadastro_futuro.py:37`).
3. C5/C6: cobertura e outro vinculo (`ponto/portas/celula.py:229`, `:254`, `:276`; `_outro_vinculo_cobre` `ponto/management/commands/gerar_celulas.py:283`).
4. C7: origem/anterior/lavra (`ponto/portas/celula.py:818`, `:820`).
5. C8/C9: autor/motivo/lavra/dono/idempotencia e par (`ponto/portas/celula.py:597`, `:606`, `:622`, `:632`, `:691`, `:700`, `:702`).
6. C10/C11/C12: classificar/derivar ou explicito/precedencia admin/status (`ponto/services/cartorio.py:1269`, `:1289`; `escala/models.py:1624`; `ponto/portas/celula.py:970`, `:1009`, `:1061`, `:1071`).
7. C13: futuro/ha julgamento/admin (`ponto/portas/celula.py:1146`, `:1149`, `:1155`).
Se a unidade L1 for linha de tabela, desdobrar e contar pelas C correspondentes; nao chamar composicao de guardas de “duplicacao” automaticamente.

**L2: 0 sitios atribuiveis fora do modulo.** Todos os 10 acima e escritor generico `salvar_e_emitir :963` pertencem as operacoes declaradas (`ponto/portas/celula.py:74`); `setattr` do reset tambem dentro (`:1166`). Nao achei SQL literal `escala_celuladia` fora; busca negativa nao prova SQL dinamico [nao verificado]. C5 e **escrita dentro da porta sem carimbo de regeneracao**, nao escritor fora (`:277` vs `:305`).

**L3: nenhum terminal da CelulaDia comprovado.** Gerada pode regenerar/reverter (`ponto/portas/celula.py:297`, `:818`); admin pode receber outra decisao admin, porta nao o veta (`:1071`, `:1093`). `editada` nao volta a `gerada` nos escritores lidos (`:644`; gerador pula `:919`; regen exige gerada `:297`; reversao exige gerada `:818`), mas pode receber nova edicao (`:641`), e alcance por nascente externa nao foi localizado [nao verificado]. Nao listar editada como terminal geral nem como efetivamente atingida por UI.

**Incertezas essenciais.** Ausencia de nascente externa editar_dia/trocar_plantao confirmada por busca global `.py`/`.sh` incluindo bin e intake; dispatch com nome montado em runtime e sessao shell permanecem [nao verificado]. Nenhum teste com banco executado; provas acima sao testes lidos [nao verificado quanto execucao]. Comentario do modelo/command diz que regeneracao so toca data>=hoje (`escala/models.py:1641`, `ponto/management/commands/gerar_celulas.py:2`), mas corpo alcanca passado explicitamente (`ponto/portas/celula.py:272`, `:297`); usar corpo no mapa. Trilha regeneracao/reversao e best-effort (`ponto/portas/celula.py:388`, `:852`), enquanto humana/reset exigem trilha ou rollback (`:657`, `:1172`).

## 14/11 21:44 · sessao 40aafdc9e7e8 · - · - · origem=Ronald (tmux)

Censo consolidado, somente leitura:

- **Carimbo da Pergunta: 0 gravacoes fora das portas declaradas.** Dois sitios escrevem `materializada_em/_por`: `_escrever_carimbo` atribui em memoria e `descarimbar_em_lote` faz `update`; o selo declara ambos por funcao e morde fugas. (`app/chamados/models.py:3153`, `app/chamados/models.py:3320`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:44`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:226`)
- **Via: 4 sitios, todos declarados**: `_escrever_carimbo`, `descarimbar_em_lote`, `encerrar_por_fato_no_espelho`, `relavra_via.relavrar`. A relavra e porta propria; nao e fuga. (`app/chamados/models.py:3157`, `app/chamados/models.py:3320`, `app/chamados/models.py:3331`, `app/chamados/services/relavra_via.py:125`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:51`)
- O detector AST do selo foi executado em memoria, sem Django nem arquivos: achou 6 ocorrencias dos campos de carimbo, sendo 4 da Pergunta e 2 de outra entidade; achou 4 ocorrencias da via; nenhuma fora das listas declaradas. A classificacao da outra entidade esta explicita no selo. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:47`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:205`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:226`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:238`)

Para L2, distinguir persistencia de atribuicao preparatoria:

- **Persistencia fora das portas: 0 achados** para resposta, validacao, carimbo, via, recusa, ancora e cache nos sitios lidos; as oito portas sancionadas estao enumeradas. (`app/core/portas.py:393`)
- **1 bloco de 3 atribuicoes fora de `lavrar`**: o command `backfill_veredito_lavrado` prepara `veredito_lavrado/julgado_em/janela_expira_em`; com `--apply`, delega a persistencia a `gravar_lavras_em_lote`. O selo explicita que o command deixou de gravar a entidade e cobra zero sitios de persistencia nele. (`app/chamados/management/commands/backfill_veredito_lavrado.py:138`, `app/chamados/management/commands/backfill_veredito_lavrado.py:143`, `app/chamados/veredito_lavrado.py:250`, `app/chamados/tests/test_porta_veredito_pergunta.py:194`)
- `resposta_colab/respondida_em`: resposta humana, resposta automatica, reparo pela trilha, reabertura e simetria do carimbo sao atos distintos, com os escritores presentes nas portas. (`app/chamados/services/disputa_emissao.py:1467`, `app/chamados/services/disputa_emissao.py:1593`, `app/chamados/services/disputa_emissao.py:1632`, `app/chamados/services/disputa_emissao.py:1652`, `app/chamados/services/disputa_emissao.py:1674`, `app/chamados/models.py:3188`)
- `validada_em/por`: validacao, devolucao, resposta que desfaz validacao, reabertura, ressuscitacao e simetria do carimbo escrevem pelos donos declarados. (`app/chamados/services/validacao.py:157`, `app/chamados/services/validacao.py:222`, `app/chamados/services/disputa_emissao.py:1480`, `app/chamados/services/disputa_emissao.py:1593`, `app/chamados/models.py:1613`, `app/chamados/models.py:3191`)
- `recusa_motivo`: **5 atribuicoes em 4 funcoes/donos**: identidade, pendencia do materializador, recusa do materializador, escape na resposta e recusa administrativa. (`app/chamados/models.py:3244`, `app/chamados/services/materializacao.py:856`, `app/chamados/services/materializacao.py:1108`, `app/chamados/services/disputa_emissao.py:1497`, `app/chamados/services/validacao.py:611`)
- `data_evento`: **1 ponto de atribuicao**, `gravar_dia_do_fato`, que deliberadamente deixa o save para o ato chamador; o selo exige zero atribuicoes fora dele. (`app/chamados/services/fio.py:791`, `app/chamados/services/fio.py:801`, `app/chamados/tests/test_e1_v15d_um_ponto_de_escrita.py:115`)
- Mutacoes em `_vivas_entre`, `_universo_da_ressuscitacao` e `_desfazer_em_memoria` sao simulacao/restauracao em memoria; nao contam como gravacao fora da porta. (`app/chamados/models.py:355`, `app/chamados/models.py:367`, `app/chamados/models.py:1541`, `app/chamados/models.py:1554`, `app/chamados/models.py:3215`)

Para L3:

- **Por rotulo de estado: 0 estados sem saida.** Existem quatro rotulos; pendente recebe resposta, respondida recebe validacao, validada pode receber resposta que desfaz validacao, resolvida pode ser descarimbada/ressuscitada. (`app/chamados/models.py:2672`, `app/chamados/services/disputa_emissao.py:1467`, `app/chamados/services/validacao.py:157`, `app/chamados/services/disputa_emissao.py:1480`, `app/chamados/models.py:3259`, `app/chamados/models.py:1613`)
- `duplicata/motivo_retratado` formam um **subuniverso declarado terminal quanto ao retorno ao fato**, nao novos rotulos de estado. A identidade ainda permite trocar uma dessas vias por outra; portanto nao declarar “duas vias sem nenhuma saida”. (`app/chamados/models.py:2566`, `app/chamados/models.py:3236`, `app/chamados/models.py:3249`, `app/chamados/models.py:3262`, `app/chamados/models.py:3320`)
- Nascentes desse subuniverso: retratacao de duplicata e remocao de motivo; provas especificas mordem a exclusao da fila e a preservacao da retratacao. (`app/chamados/services/fio.py:1360`, `app/chamados/models.py:2117`, `app/chamados/tests/test_leitores_ouvem_a_lei.py:28`, `app/chamados/tests/test_e1_entradas.py:142`)

Limites e cuidados:

- O censo generico retornou 20 sitios atribuiveis, mas **nao representa total real**: construcao por `cls` e `qs.update(**campos)` exigem leitura complementar; nao usar “20 escritores totais”. (`app/core/censo_escritas.py:43`, `app/chamados/models.py:3070`, `app/chamados/services/disputa_emissao.py:1632`)
- O selo forte exclui migrations e declara limitacoes para `__dict__`, nomes externos e `SET_NULL`; zero achados nao prova codigo dinamico arbitrario. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:111`, `app/chamados/tests/test_o261e_f2_porta_carimbo.py:208`)
- A migration de dados da Pergunta encontrada preenche identidade (`colaborador_id/instancia_marco`), sem R/V/carimbo; nao constitui transicao dos quatro rotulos. (`app/chamados/migrations/0051_decisao_pergunta.py:22`)
- Headers antigos ainda anunciam escrita direta do backfill; o corpo atual delega persistencia e o teste atualizado prende essa mudanca. (`app/chamados/veredito_lavrado.py:58`, `app/chamados/management/commands/backfill_veredito_lavrado.py:143`, `app/chamados/tests/test_porta_veredito_pergunta.py:206`)
- Cron confirmado para cache: `lavrar_perguntas_stale --apply` a cada cinco minutos; `reabrir_orfas_enterradas` esta classificado como comando de uma vez. (`app/config/crons.py:390`, `app/config/crons.py:824`)

## 14/11 21:44 · sessao eda415e096ba · - · - · origem=Ronald (tmux)

Auditoria concluida por leitura; testes nao executados [nao verificado quanto ao resultado atual]. [nao verificado]

**L2: 4 sitios persistem `status` fora de `mudar_estado`, com naturezas diferentes:**

| sitio | natureza |
|---|---|
| `app/ponto/services/ausencia.py:403`, `:446`, `:479` | `criar_ausencia`: nascimento com estado calculado ou parametro explicito; nao altera registro preexistente. |
| `app/ponto/services/ausencia.py:572` | `editar_ausencia_registro`: `save()` completo regrava estado existente; atribuicoes anteriores mexem em tipo/range/minutos, nao em status (`:561-571`); eventual troca de status vem depois pela porta (`:591-609`). |
| `app/ponto/services/ausencia.py:1355` | `rejeitar_ausencias_de_ferias`: altera estado existente diretamente por `queryset.update(status='rejeitada', atualizado_em=...)`; sem `save`/signals conforme declaracao `:1348-1349`. |
| `app/ponto/services/ausencia.py:1585` | `rederivar_fim`: `save()` completo regrava estado existente, sem atribuir novo estado; guarda contra ferias em `:1584`. |

`mudar_estado` declara escritor unico de funcao em `app/ponto/services/ausencia.py:873`, atribui em `:889`, persiste em `:900`. O gate estrutural aceita o arquivo inteiro: `app/core/portas.py:486`; `app/ponto/tests/test_chokepoint_ausencia_gate.py:35-36`. O Django admin de Ausencia usa `SoLeitura`: `app/ponto/admin.py:38`.

**L3: 0 estados sem saida na maquina:** 4 estados declarados em `app/ponto/models.py:701-704`; as 9 setas da tabela sao `2+3+1+3`, em `app/ponto/services/ausencia.py:831-839`. Nao ha terminal declarado nessa tabela (`:831-840`). A seta `rejeitada -> aprovada` consta da matriz (`:839`), mas nao localizei nascente de producao que a exercesse; os aprovadores humanos selecionam ABERTAS (`app/ponto/views.py:2293`, `:2650`) [nao verificado para caminhos dinamicos nao identificados].

**Provas para preencher a matriz (arquivo::nome, linha da definicao):**

| transicao/ato | prova lida |
|---|---|
| nascimento aguardando_documento | `app/ponto/tests/test_a4_estados_com_porta.py::test_MORDE_tipo_que_exige_doc_SEM_anexo_nasce_aguardando_documento`:92; assert em `:96`. |
| nascimento aguardando_decisao com anexo | mesmo arquivo `::test_MORDE_com_anexo_nasce_aguardando_decisao`:98; assert `:103`. |
| nascimento aguardando_decisao sem documento exigivel | mesmo arquivo `::test_tipo_que_NAO_exige_doc_nasce_aguardando_decisao`:105; assert `:110`. |
| nascimento aprovada | `app/ponto/tests/test_criar_ausencia.py::test_cria_range_e_aprova`:60; asserts `:62-64`. |
| aguardando_documento -> aguardando_decisao por anexo | `app/ponto/tests/test_a4_estados_com_porta.py::test_MORDE_anexar_documento_passa_a_bola_para_o_DP`:137; assert `:142`. |
| mesma seta por vencimento | mesmo arquivo `::test_MORDE_D7_sem_documento_vai_para_o_DP_e_NAO_vira_falta`:144; asserts `:154-155`; Pauta em `::test_MORDE_D7_abre_Pauta_para_o_DP`:157, assert `:165`. |
| mesma seta por edicao do tipo | `app/ponto/tests/test_ausencia_tipo_rejulga.py::test_MORDE_4280_atestado_vira_saida_antecipada_e_sai_de_aguardando_documento`:33; asserts `:39-40`. |
| aguardando_decisao -> aguardando_documento | mesmo arquivo `::test_MORDE_tipo_que_exige_pede_o_documento_com_prazo`:44; asserts `:49-50`. |
| aguardando_decisao -> aprovada | `app/ponto/tests/test_a4_estados_com_porta.py::test_so_o_DP_aprova`:168; assert `:173`. |
| aguardando_documento -> rejeitada | mesmo arquivo `::test_MORDE_reabrir_SEM_anexo_volta_a_dever_documento_com_prazo_novo`:342; chama rejeicao `:345` e verifica reabertura `:349`. |
| aguardando_decisao -> rejeitada | mesmo arquivo `::test_MORDE_lancar_sobre_rejeitada_reabre_e_nao_duplica`:324; assert `:330`. |
| aprovada -> rejeitada | `app/ponto/tests/test_editar_cancelar_ausencia.py::test_cancelar_aprovada_vira_rejeitada_com_trilha`:21; asserts `:26-29`. |
| rejeitada -> aguardando_decisao por relancamento | `app/ponto/tests/test_a4_estados_com_porta.py::test_MORDE_lancar_sobre_rejeitada_reabre_e_nao_duplica`:324; asserts `:335-340`. |
| rejeitada -> aguardando_documento por relancamento | mesmo arquivo `::test_MORDE_reabrir_SEM_anexo_volta_a_dever_documento_com_prazo_novo`:342; asserts `:348-351`. |
| duas reaberturas humanas, decididas pelo anexo | `app/ponto/tests/test_reverter_rejeicao.py::test_MORDE_o_ANEXO_decide_o_destino`:51; asserts `:59-66`. |
| motivo obrigatorio na reversao | mesmo arquivo `::test_MORDE_motivo_e_OBRIGATORIO`:68; asserts `:72-75`. |
| trilha da reversao | mesmo arquivo `::test_MORDE_a_trilha_diz_QUEM_QUANDO_e_o_MOTIVO`:86; asserts `:91-98`. |
| rejeitada -> aprovada direta | nenhum teste especifico localizado [nao verificado]. | [nao verificado]
| anexo/reenvio sem duplicar | `app/api/tests/test_bo_atestado_409_mudo.py::test_reenvio_identico_200_ja_recebido_e_UMA_linha`:47; asserts `:51-54`; `::test_reenvio_com_anexo_entra_na_existente`:56; asserts documento/contagem `:63-64`, sem assert de status. |
| upload inicial | `app/ponto/tests/test_upload_ausencia.py::test_documento_anexado_e_gravado`:34; asserts `:42-43`. |
| aprovada -> aprovada, periodo editado | `app/escala/tests/test_cobertura_parcial_periodo.py::test_RED_a_edicao_da_aprovada_passa_pela_decidir_com_trilha_antes_depois`:159; asserts estado, carimbo e trilha `:166-171`. |
| periodo pela view | mesmo arquivo `::test_admin_declara_o_periodo_e_a_trilha_guarda_antes_e_depois`:250; asserts `:253-255`. |
| periodo invalido recusado | mesmo arquivo `::test_porta_recusa_fim_igual_inicio_inteira_e_um_so_lado`:184; asserts `:188-193`. |
| bulk rejeicao de ferias | `app/ponto/tests/test_hj091_ferias_derivadas.py::test_MORDE_as_formas_de_escrita`:388 injeta `update(status='rejeitada')` em `:391`, exige deteccao `:397`; `::test_a_arvore_so_escreve_ferias_pelos_escritores_declarados`:399, assert `:402`. Whitelist explicita dessa funcao em `app/ponto/tests/selo_hj091.py:26`. |
| cancelamento origem leva ausencia | `app/ponto/tests/test_hj091_ferias_derivadas.py::test_RED_cancelar_agendamento_leva_a_linha_dele_fora_da_janela`:200; assert `:207-208` aceita rejeicao OU exclusao, nao prova isoladamente o update intermediario. |

**Selos de criacao/escrita:**

- `app/ponto/tests/test_contract_um_produtor_de_ausencia.py::test_MORDE_ninguem_cria_Ausencia_fora_do_produtor`:88; detecta aliases por import (`:49-55`) e cria/bulk_create/construtor (`:61-68`); contraprovas `::test_MORDE_o_varredor_pega_as_tres_formas`:113 e `::test_MORDE_o_varredor_segue_o_apelido`:129.
- `app/ponto/tests/test_chokepoint_ausencia_gate.py::test_MORDE_nenhuma_escrita_da_familia_fora_da_porta`:53; allowlist vazia `:25`; contraprova carimbo na view `::test_MORDE_o_gate_ve_o_carimbo_do_decisor_na_view`:70.
- O selo antigo de status olha apenas 3 modulos e variaveis chamadas `ausencia`/`aus`/`a`: `app/ponto/tests/test_a4_estados_com_porta.py:190-203`; nao prova sozinho unicidade global.
- Limites declarados do selo de ferias: variavel, SQL cru, objeto recebido como parametro, dict subscrito: `app/ponto/tests/selo_hj091.py:12-15`.

## 14/11 21:44 · sessao 5c1cbbd6e2b6 · - · - · origem=Ronald (tmux)

Censo final: **39 sitios fora dos testes: 19 `abrir`, 19 `criar` normais e 1 `criar` no smoke**. O smoke recusa banco sem `test` no nome e encerra com rollback; seus nascimentos nao persistem pelo `handle` normal (`app/chamados/management/commands/smoke_chamados.py:33`, `:40`, `:44`).

**Nascentes `abrir` — 19, todas pedem nascimento default; nenhuma passa `status_local` exato/default:**

| Sitio | Funcao | Lookup alem do colaborador |
|---|---|---|
| `app/ponto/management/commands/detectar_par_relampago.py:368` | `handle` | modulo `par_relampago` + contexto.data_turno; constante `:17` |
| `app/ponto/management/commands/reconciliar_grade.py:507` | `_emitir_furo` | `batida_ausente` + contexto.data_turno; constante `:54` |
| `app/ponto/management/commands/detectar_ausencias.py:191` | `handle` | `sem_previsao_escala` + status em VIVOS |
| Mesmo arquivo `:272` | `handle` | `ancora_limbo` |
| Mesmo arquivo `:511` | `_chamado_dia_sem_grade` | `sem_previsao_escala` + subcategoria `dia_sem_grade` + VIVOS |
| Mesmo arquivo `:694` | `_abrir_chamado` | `batida_ausente` + contexto.data_turno; constante `:29` |
| `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:151` | `handle` | `turno_aberto_24h` + contexto.data_turno |
| `app/ponto/management/commands/processar_alertas_turno.py:335` | `_abrir_chamado_critico` | `turno_aberto` + contexto.data_turno |
| `app/ponto/services/cluster_espurio.py:196` | `reconciliar_colab` | `cluster_batida_espuria` + contexto.data_turno; constante `:11` |
| `app/ponto/services/ausencia.py:722` | `abrir_chamado_ausencia_colab` | `ausencia_colab` + contexto.ausencia_id |
| `app/ponto/views.py:3052` | `ping_geo` | `ping_geo_fora` + criado_em__date |
| `app/escala/regua_defesa.py:51` | `vetar_cobranca_se_degenerada` | `regua_degenerada` + contexto.tipo_escala_id |
| `app/api/views.py:1966` | `api_justificativas` | `justificativa_colab` + categoria `justificativa` + VIVOS |
| `app/chamados/reconciliador.py:911` | `reemitir_chamado_do_dia` | `batida_ausente` + contexto.data_turno, chave definida `:836` |
| `app/chamados/services/disputa_emissao.py:568` | `_abrir_ou_agregar_disputa` | `disputa_supervisao` + categoria homonima + VIVOS |
| Mesmo arquivo `:1104` | `criar_disputa_a_partir_de_chamados_orfaos` | `disputa_supervisao_manual` + categoria `disputa_supervisao` + subcategoria `orfao_14h` + VIVOS |
| Mesmo arquivo `:1339` | `criar_disputa_ancora_de_chamado` | Idem, subcategoria `ancora_ausente` |
| `app/chamados/services/adesao.py:75` | `sincronizar_adesao` | `sem_previsao_escala` + subcategoria `nunca_bateu` |
| `app/chamados/views.py:973` | `abrir_chamado_rapido` | `mensagem_dp` + VIVOS |

**Nascentes `criar` normais — 19; todas omitem `status`:**

| Sitio | Funcao | Modulo / estado pedido pelo default |
|---|---|---|
| `app/colaboradores/views.py:1981` | `solicitar_beneficio` | beneficio / aberto |
| Mesmo arquivo `:2367` | `agenda_enviar_aceite` | agenda_dia / aberto |
| `app/ponto/utils.py:40` | `bloquear_se_sem_posto` | colaborador_sem_posto / aberto |
| Mesmo arquivo `:103` | `bloquear_se_em_ferias` | colaborador_em_ferias / aberto |
| `app/ponto/views.py:294` | `bater_ponto` | entrada_bloqueada_turno_aberto / aberto |
| `app/api/views.py:717` | `api_bater_ponto` | batida_fora_escala_leve / registrado |
| `app/api/views_core.py:711` | `api_bater_ponto` | batida_fora_escala_leve / registrado |
| `app/api/views.py:2170` | `api_abrir_chamado` | app_colaborador / aberto |
| `app/colaboradores/services/vinculo.py:410` | `_executar_aceite` | escala_colaborador / aberto |
| `app/colaboradores/services/desligamento.py:195` | `executar_desligamento` | desligamento_revisao / registrado |
| `app/ponto/services/justificativa.py:291` | `rejeitar_justificativa_dp` | justificativa / aberto |
| `app/ponto/services/geofence.py:361` | `verificar_geofence` | gps_ausente / registrado; gps_fora_geofence / aberto; escolha `:344` |
| `app/ponto/services/triagem_batida.py:107` | `triagem_bater_ponto` | acesso_bloqueado / aberto |
| Mesmo arquivo `:336` | `ramo_solicitar_anuencia` | batida_fora_horario / aberto |
| Mesmo arquivo `:459` | `_bloqueio_horario_clt` | batida_fora_horario / aberto |
| `app/ponto/management/commands/processar_alertas_avancados.py:121` | `handle` | turno_aberto_24h / aberto |
| `app/chamados/services/regularizacao_ext.py:247` | `criar_regularizacao_externa` | regularizacao_externa / aberto |
| `app/chamados/services/abertura.py:47` | `abrir_chamado_local` | modulo variavel / aberto ou registrado |
| Mesmo arquivo `:172` | `abrir_chamado_para_colaborador` | modulo variavel limitado ao catalogo / aberto |

Os estados acima decorrem de `app/chamados/models.py:1094`, `:1107`, `:1110` e da lista de auditoria em `app/chamados/catalogo/natureza.py:18`, consultada em `:36`.

`abertura.py:47` pode receber auditoria: a tela aceita qualquer chave de MOTIVOS (`app/chamados/views.py:170`, `:174`), e o service so troca o modulo quando a categoria fornece modulo diferente de `chamados` (`app/chamados/services/abertura.py:30`, `:33`). Ja `abertura.py:172` restringe a `tipos_para_colaborador` (`:148`), cujo catalogo oferece ausencia_colab, disputa_supervisao_manual, regularizacao_externa, agenda_dia e mensagem_dp, todos acionaveis (`app/chamados/catalogo/modulos.py:120`, `:146`, `:151`, `:156`, `:192`, `:306`).

**Pre-dedup fora de `criar` — 9 sitios:**

- Colaborador + modulo + data de criacao: `app/ponto/utils.py:31` e `:94`.
- Colaborador + modulo + VIVOS: `app/ponto/views.py:284`; `app/ponto/services/triagem_batida.py:102`, `:325`.
- Colaborador + modulo + janela de 14h, sem status: `app/ponto/services/triagem_batida.py:452`.
- Colaborador + subcategoria GPS + VIVOS: `app/ponto/services/geofence.py:353`.
- Primeiro turno_aberto + VIVOS + data_turno; depois turno_aberto_24h + NAO_FECHADOS sem dia: `app/ponto/management/commands/processar_alertas_avancados.py:100`, `:105`.
- Colaborador + modulo + VIVOS + assunto + descricao + prazo + autor no contexto, sob trava: `app/chamados/services/abertura.py:164`, `:165`.

**Wrappers e cron relevantes:**

- `emitir_furo_retroativo` chama `_abrir_chamado` em `app/ponto/management/commands/emitir_furo_retroativo.py:154`, chegando ao sitio `detectar_ausencias.py:694`.
- Cluster nasce pelo evento em `app/ponto/services/cartorio.py:1042` e pelo comando manual apply em `app/ponto/management/commands/detectar_cluster_espurio.py:69`, `:81`. Seu cron passa `--alarme`, portanto nao cria nesse agendamento (`app/config/crons.py:598`).
- Grade chama `_emitir_furo` em `app/ponto/management/commands/reconciliar_grade.py:254`; seu cron nao passa apply (`app/config/crons.py:462`).
- Cron ativo direto: processar_alertas_turno, processar_alertas_avancados, detectar_ausencias e detectar_par_relampago (`app/config/crons.py:362`, `:363`, `:364`, `:605`).
- Adesao: `app/escala/management/commands/apurar_furos_diarios.py:17`, encadeado a processar_cartorio em `app/config/crons.py:1527`.
- Desligamento: `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:57`, agendado com apply em `app/config/crons.py:440`.
- Ausencia: helper chamado em `app/api/views.py:1842` e `app/ponto/views.py:2579`.
- Abertura humana: `app/chamados/views.py:136` e `:197`.
- Regua degenerada: `app/ponto/management/commands/detectar_ausencias.py:601` e `reconciliar_grade.py:495`.
- Regularizacao: `app/colaboradores/views.py:2987`; wrapper `app/chamados/services/regularizacao_ext.py:319`.
- O kernel duplicado tem rota propria (`app/api/urls_core.py:5`, `:14`; `app/config/urls_core.py:6`).

Scan AST recursivo de todos os `.py` fora de testes/migrations encontrou os mesmos 39 sitios, todos em `app`; nenhum constructor/ORM externo de nascimento por alias resolvido. Esse resultado de busca nao constitui prova contra construcao dinamica [nao verificado]. [nao verificado]

## 14/11 21:44 · sessao bf117342febf · - · - · origem=Ronald (tmux)

Mapa parcial consolidado, somente leitura:

- `FechamentoMensal` declara **3 status**: `aberto`, `aprovado`, `exportado`; default `aberto`. `exportado` nao ganhou escritor literal de producao nas buscas realizadas; alcance em runtime [nao verificado]. (`app/ponto/models.py:399-410`)
- A entidade agregada usa **4 marcos independentes** e apresenta o mais adiantado: `aprovada`, `exportada`, `trancada`, `paga`; sem marco, `aberta`. Portanto, reabrir aprovados ou a tranca nao necessariamente faz o agregado aparecer `aberta`. (`app/ponto/services/fechamento.py:1461`, `app/ponto/services/fechamento.py:1532-1544`)
- Corte default 21 a 20, configuravel por empresa, limitado a 2..28. (`app/ponto/janelas.py:10-21`, `app/ponto/janelas.py:36-57`)

| Transicao | Juizes/nascentes | Escritor | Persistencia/prova |
|---|---|---|---|
| inexistente → aberto | Universo `universo_da_competencia`; filtro nomeado; `somente_leitura=False`; guarda de exportadas para unica chave `(9,2026)`. (`app/ponto/services/fechamento.py:554-564`, `app/ponto/services/fechamento.py:36-43`) | `recalcular_fechamento_mes`, `get_or_create`. (`app/ponto/services/fechamento.py:898-901`) | Dados calculados e `derivado_em`; `save` no aberto. (`app/ponto/services/fechamento.py:1137-1144`) |
| aberto → aprovado | Lote: `classificar_export` + `trava_do_grupo` + filtro `aberto`; individual: `fechamento_lido`, `classificar_export`, exclusao `IGNORA_NO_SELECIONADO`; TXT: nascente `folha/views.py:252`. (`app/ponto/views.py:1299-1315`, `app/ponto/views.py:1365`, `app/ponto/views.py:1483-1518`, `app/folha/views.py:252`) | `aprovar_lote`, `aprovar`. (`app/ponto/services/fechamento.py:1323-1340`) | `status`, `aprovado_por`, `aprovado_em`, observacao opcional; prova `app/folha/tests/test_porta_folha.py::PortaFolhaIdempotenteTest.test_aprovar_lote_2x_grava_uma_vez` (:33), `::test_aprovar_e_reabrir_um_colaborador_2x` (:58). |
| aprovado → aberto | Reavaliacao: `classificar_export`, `conta_vazia`, filtro `aprovado`; individual: status exatamente aprovado; reabertura da competencia: periodo trancado + motivo. (`app/ponto/views.py:1412-1426`, `app/ponto/views.py:1450-1455`, `app/ponto/views.py:1538-1546`) | `reabrir_lote`, `reabrir`, `reabrir_periodo`. (`app/ponto/services/fechamento.py:1344-1354`, `app/ponto/services/fechamento.py:1449-1452`) | Limpa carimbo aprovacao; logs das views. (`app/ponto/views.py:1427-1430`, `app/ponto/views.py:1456-1457`, `app/ponto/views.py:1548-1550`) Provas `app/folha/tests/test_porta_folha.py::test_reabrir_lote_2x` (:49), `::test_reabrir_periodo_devolve_os_aprovados_e_a_2a_nao_grava` (:81). |
| sem tranca / reaberta → trancada | Unica nascente direta encontrada: aprovacao de lote com empresa e sem pulados. (`app/ponto/views.py:1369-1370`) | `trancar_periodo`: cria linha ou limpa reabertura existente. (`app/ponto/services/fechamento.py:1408-1419`) | `PeriodoFechado`: empresa/competencia/autor, `ativo=True`; retranca limpa `reaberto_*`. (`app/ponto/services/fechamento.py:1408-1419`) Prova `app/folha/tests/test_porta_folha.py::test_trancar_periodo_2x_e_uma_linha` (:74). |
| trancada → reaberta | RBAC, POST, motivo nao vazio, `periodo_trancado`. (`app/ponto/views.py:1525-1546`) | `reabrir_periodo`. (`app/ponto/services/fechamento.py:1432-1452`) | `ativo=False`, autor/data/motivo; aprovados → aberto; log antes/depois. (`app/ponto/services/fechamento.py:1437-1452`, `app/ponto/views.py:1548-1550`) |
| projecao derivada → vencida | **7 nascentes diretas**: cadastro de tipo, municipio do posto, lote de justificativa, editar ausencia, decidir ausencia, aprovar justificativa, materializar saida. (`app/escala/services/cadastro_tipo.py:363`, `app/colaboradores/services/vinculo.py:959`, `app/ponto/views.py:1808`, `app/ponto/services/ausencia.py:587`, `app/ponto/services/ausencia.py:1013`, `app/ponto/services/justificativa.py:215`, `app/ponto/services/justificativa.py:390`) | `invalidar_fechamento`, filtra derivado nao nulo e atualiza. (`app/ponto/services/fechamento.py:1649-1656`) | `derivado_em=None`, `atualizado_em`; log python, sem LogAuditoria nesta funcao. (`app/ponto/services/fechamento.py:1656-1664`) Prova `app/ponto/tests/test_o48p6_projecao_carimbada.py::test_MORDE_invalidar_mata_o_carimbo_e_MOVE_o_contador` (:113). |
| projecao derivada → vencida por restauracao | **6 chamadas** em 5 commands: aplicar_09_corte_b :220 e :266; aplicar_09_he_intra :149; diff_reclassificar_partido :224; desvio_o68b :197. (`app/ponto/management/commands/aplicar_09_corte_b.py:220`, `app/ponto/management/commands/aplicar_09_corte_b.py:266`, `app/ponto/management/commands/aplicar_09_he_intra.py:149`, `app/ponto/management/commands/diff_reclassificar_partido.py:224`, `app/ponto/management/commands/desvio_o68b.py:197`) | `restaurar_fechamento`. (`app/ponto/services/fechamento.py:1373-1396`) | `derivado_em=None` por default; campos pedidos; log python; prova `app/ponto/tests/test_o48p6_projecao_carimbada.py::test_MORDE_restaurar_fechamento_mata_o_carimbo` (:165). **Correcao da contagem: sao 5 chamadas, em 4 commands**, conforme os cinco sitios citados. |
| previsto materializado → invalidado | Signal `post_save` de `EscalaColaborador`. (`app/escala/signals.py:137-146`) | `invalidar_previsto`. (`app/ponto/services/fechamento.py:1607-1613`) | Zera somente `previsto_em`; toca atualizado; nao zera `derivado_em`. Prova `app/folha/tests/test_porta_folha.py::test_invalidar_previsto_2x` (:92). |
| projecao vencida → derivada | Mesmas nascentes de recalculo; ramo aberto ou nao aberto preserva status. (`app/ponto/services/fechamento.py:1137-1189`) | `recalcular_fechamento_mes`. (`app/ponto/services/fechamento.py:1143-1144`, `app/ponto/services/fechamento.py:1160-1189`) | `derivado_em=now`, campos calculados; status/aprovacao preservados no update. Provas `app/ponto/tests/test_o48p6_projecao_carimbada.py::test_RED_o_carimbo_nasce_no_ramo_ABERTO` (:92), `::test_RED_o_carimbo_nasce_tambem_no_ramo_APROVADO` (:100). |

Nascentes diretas de `recalcular_fechamento_mes` que podem persistir:

- View: `app/ponto/views.py:1255`.
- Porta: `app/ponto/portas/regen_exportada.py:184`.
- Evento: `app/ponto/services/fechamento.py:1810`, agendado `on_commit` (:1824).
- Commands: `app/ponto/management/commands/aplicar_09_corte_b.py:212`; `app/ponto/management/commands/celula_veredito_velho.py:149`; `app/ponto/management/commands/diff_reclassificar_partido.py:206` (persiste apenas com `aplicar`, :213-214); `app/ponto/management/commands/recalcular_fechamento.py:118` (apenas `--apply`, :110-114).
- Scripts: `bin/aplicar_o263_comp10.py:51`; `bin/relavrar_o118_col221.py:53`; `bin/relavrar_o119_col221.py:52`.
- `bin/relavrar_o120_col221_09.py:74-75` fornece `permitir_exportada=True`, ausente na assinatura atual (`app/ponto/services/fechamento.py:448-451`); impossibilidade de entrar na funcao e inferencia direta do Python, sem execucao.
- Ensaio/rollback: `app/folha/management/commands/simular_folha.py:123` exige banco de ensaio (:110-111); `app/ponto/management/commands/plano_b_no_dinheiro.py:110-112`; `app/ponto/management/commands/folga_que_sumiu.py:70-71`; `app/ponto/management/commands/desvio_o68b.py:94-95`; `app/ponto/management/commands/diff_reclassificar_partido.py:161-162`; `bin/r5_idempotencia_frota.py:117` declara sombra (:123).
- Nenhum cron direto de recalculo e declarado; a configuracao explica expressamente a ausencia. (`app/config/crons.py:1148-1155`)

**5 sitios diretos** alimentam `recalcular_por_evento`: `app/ponto/registro_batida.py:178`, `app/ponto/portas/he.py:202`, `app/ponto/portas/celula.py:496`, `app/chamados/services/validacao.py:384`, `app/chamados/services/validacao.py:469`. Cadeias anteriores destas outras entidades ficam citadas somente pelo sitio, conforme escopo.

Alertas para consolidacao:

- O selo de porta considera **modulo**, nao funcao: `FechamentoMensal` e `PeriodoFechado` aceitam qualquer escritor dentro de `ponto/services/fechamento.py`. (`app/folha/tests/test_chokepoint_folha_gate.py:40`, `app/folha/tests/test_chokepoint_folha_gate.py:72-73`)
- `restaurar_fechamento` diz nao tocar status no docstring, mas aceita todo campo real e so retira identificadores; capacidade de escrever status existe. Nos chamadores lidos, nao encontrei pedido de status. (`app/ponto/services/fechamento.py:1370-1380`)
- `FechamentoMensal.save` veta somente aprovado → aprovado; nao concentra a lei das demais transicoes. (`app/ponto/models.py:496-503`)
- A segunda chamada de `reabrir_periodo` continua salvando os metadados do periodo; o teste de idempotencia mede apenas quantidade de fechamentos reabertos. (`app/ponto/services/fechamento.py:1437-1441`, `app/folha/tests/test_porta_folha.py:85-89`)
- Vencido e marcador de projecao, nao quarto status de `FechamentoMensal`. O proprio leitor define vencido como `derivado_em NULL`. (`app/ponto/services/fechamento.py:1679-1681`)
- Comentarios sobre cron da madrugada nao provam cron vigente: configuracao diz expressamente que recalculo nao tem cron. (`app/ponto/registro_batida.py:172`, `app/ponto/services/fechamento.py:1786`, `app/config/crons.py:1148-1155`)
- Testes foram lidos; nao executados. [nao verificado] [nao verificado]

## 14/11 21:43 · sessao e88a44e4d34e · - · - · origem=Ronald (tmux)

Leitura da retratacao e universo consolidada, sem executar testes. [nao verificado]

- **Retratar:** `retratada_em NULL -> timestamp`, junto de `aprovada=False`; juiz final apenas idempotencia em `app/ponto/registro_batida.py:271`; escrita por `anotar` em `:274`, `setattr/save` em `:253-254`; trilha em `:263`; relavratura por evento em `:281`.
- **Desfazer:** `retratada_em preenchido -> NULL`, junto de `aprovada=True`, por `app/ponto/services/retratar_humano.py:207`; usa o mesmo escritor `app/ponto/registro_batida.py:254`. Exige permissao (`retratar_humano.py:194`), retratacao existente (`:197`), ultima trilha humana (`:199-202`, regra em `:48-53`) e prazo de **7 dias** (`:32`, `:203-205`).
- **Universo:** entrar/sair da apuracao nao ganha outro campo: `batidas_apuraveis` filtra exclusivamente `retratada_em__isnull=True`, com recortes opcionais por instante/colaborador; nao filtra `aprovada` nem `origem`. Portanto `offline_pendente` nao retratada conta. `app/ponto/turnos.py:2856`, `:2859`, `:2876-2883`.
- **Retratada nao e terminal generico**, pois existe desfazer; retratacoes de cron/geofence/passivo e humanas vencidas nao passam por essa operacao. A funcao encaminha vencida ao “Resolver dia”, mas a saida dessa mesma Batida por esse caminho nao foi demonstrada nesta leitura. `app/ponto/services/retratar_humano.py:199-205`. Nao inventar estados separados pelo nome do autor ou origem da trilha.

**Nascentes completas da retratacao identificadas:**

| Nascente | Juizes antes da porta | Sitios |
|---|---|---|
| Humana | Permissao/ator `_pode`; motivo existente e ativo; idempotencia | `app/ponto/views.py:3320` → `app/ponto/services/retratar_humano.py:163-179`; URL `app/ponto/urls.py:61` |
| Recusa geofence | Chamado nao encerrado, motivo preenchido, batida pertencente ao chamado/colaborador | `app/chamados/views.py:544` **e** `app/chamados/services/fio.py:545` → `app/chamados/services/acoes_chamado.py:260-277` |
| Par relampago isolado | `par_completo`; procedencia/teto; simulacao com melhoria estrita e geometria perfeita | `app/ponto/management/commands/detectar_par_relampago.py:353-363`; `_alvo_retratavel` `:199-214`; `motivo_para_nao_retratar` `:185-196`; `escolher_retratavel` `:235-248`; `_retratar` chama porta em `:224` |
| Par relampago embutido | `par_completo`; guardas de procedencia/teto; exclusao do par deixa geometria perfeita | `app/ponto/management/commands/detectar_par_relampago.py:300-313`, `:254-272`, chamadas `_retratar` em `:399-400`, porta em `:224` |
| Command passivo | `classificar` + `a_retratar`: origem especifica, pergunta NULL, nao retratada, dia de FOLGA e competencia no escopo; `--apply` e arquivo de reversao | `app/ponto/services/passivo_s84.py:53-54`, `:89-96`, `:115-116`; `app/ponto/management/commands/passivo_disputa_retro.py:124`, `:132-152`, porta `:164` |

Sao **4 sitios persistentes de chamada direta** da porta (`acoes_chamado.py:277`, `retratar_humano.py:179`, `detectar_par_relampago.py:224`, `passivo_disputa_retro.py:164`). Ha **1 sitio adicional de ensaio**, `app/ponto/management/commands/diff_passivo_s84.py:82`, executado dentro de `atomic` e desfeito por `_Rollback`: `app/ponto/services/diff_frota.py:72`, `:86-90`.

Cron do par: **06:26 diario**, `--dias 2 --apply --retratar`, em `app/config/crons.py:605`. Passivo e diff estao declarados fora da rotina: `app/config/crons.py:854-857`, `:978-984`.

Desfazer tem **1 nascente**: `app/ponto/views.py:3332` → servico `retratar_humano.py:190`; URL `app/ponto/urls.py:62`.

**L1, contribuicao deste recorte:** retratacao tem multiplos juizes por percurso: humano (`retratar_humano.py:131`, `:163-175`), geofence (`acoes_chamado.py:260-273`), automatico (`detectar_par_relampago.py:185-196`, `:235-248`, `:254-272`, `:300`), passivo (`passivo_s84.py:89-96`, `:115-116`), alem da idempotencia comum (`registro_batida.py:271`). Desfazer tambem combina juizes de permissao, procedencia e prazo (`retratar_humano.py:194-205`).

**Leitores concretos:**

- Apuracao/cartorio: `app/ponto/services/cartorio.py:1172`; tela/contador: `app/core/views.py:114`; painel: `app/core/services/painel_op.py:83`; previa: `app/folha/previa.py:58`; exportacao: `app/folha/export.py:499`.
- Exibicao das retratadas e possibilidade de desfazer: `app/colaboradores/services/calendario.py:239-255`.
- Qualquer `save` rejulga celulas pelos signals: `app/ponto/signals.py:131-135`; recomputa nucleo em `:34-36`. Desfazer chama somente `anotar`, sem chamada propria a `_fechamento_por_evento`: `app/ponto/services/retratar_humano.py:207-209`.

**Provas que mordem, lidas:**

- `app/ponto/tests/test_porta_retratar_batida.py::test_MORDE_com_motivo_sai_da_apuracao_e_continua_no_banco`, `:94-110`.
- `app/ponto/tests/test_porta_retratar_batida.py::test_MORDE_idempotente_duas_chamadas_uma_trilha`, `:112-130`.
- `app/ponto/tests/test_porta_retratar_batida.py::test_MORDE_desfazer_vale_7_dias_e_depois_nao`, `:132-155`.
- `app/ponto/tests/test_porta_retratar_batida.py::test_MORDE_nao_desfaz_o_que_esta_porta_nao_retratou`, `:157-169`.
- `app/ponto/tests/test_porta_retratar_batida.py::test_MORDE_sem_a_acao_a_PORTA_recusa_mesmo_sem_passar_pela_tela`, `:251-260`.
- `app/chamados/tests/test_geofence_validar_e_recusar.py::test_MORDE_recusar_retrata_a_batida_com_motivo_e_trilha`, `:87-96`.
- `app/ponto/tests/test_retratacao_relavra_o_dia.py::test_retratar_chama_a_relavratura_por_evento`, `:33-38`.
- `app/ponto/tests/test_retratacao_relavra_o_dia.py::test_MORDE_retratar_de_novo_e_no_op`, `:40-45`.
- `app/ponto/tests/test_par_relampago_v3.py::test_eco_retratavel`, `:48-58`; `::test_par_real_de_marco_nao_retrata`, `:84-92`.
- `app/ponto/tests/test_par_relampago_pelo_turno.py::test_MORDE_o_dia_nao_encerrado_nao_tem_candidato_e_o_encerrado_tem`, `:58-64`.
- `app/ponto/tests/test_o247_retratada_fora_do_pareamento.py::test_RED_sitio1_orfa_RETRATADA_nao_vira_turno_aberto`, `:84-94`.
- `app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py::test_zero_universo_cru_alimenta_parear_turnos`, `:206-213`; `::test_a_catraca_dos_copiadores_a_mao_nao_cresce`, `:215-219`. **Limite declarado:** gap interprocedural e apuracao fora do pareador (`:26-35`); `TETO_COPIADORES=8` (`:191`) e teto, nao contagem atual executada.

**Incertezas:** suite nao executada [nao verificado]; ocorrencias/volumes reais de producao [nao verificado]; reversao concreta da mesma Batida via Resolver dia apos vencimento [nao verificado]. [nao verificado]

## 14/11 21:43 · sessao 051c43480789 · - · - · origem=Ronald (tmux)

**Base lida; nenhum arquivo criado/editado/apagado.** Linhas abaixo relativas à raiz da copia. Testes lidos, nao executados. [nao verificado quanto à execução] [nao verificado]

**Estados e trilha**

- PerguntaDisputa possui **4 rótulos derivados** (`pendente`, `respondida`, `validada`, `resolvida`), nao campo `status`; `estado` le carimbos e `pergunta_viva`: `app/chamados/models.py:2672`, `app/chamados/models.py:3090`. Vivo/morto pelo C6 é outro eixo: `app/chamados/juizes.py:1399`.
- DecisaoPergunta possui **8 atos**: resposta, validacao, devolucao, recusa, reabertura, silencio, retratacao, absorcao; escritor `registrar` usa bulk_create e acorda celulas; save/delete existentes recusam: `app/chamados/models.py:3913`, `app/chamados/models.py:3959`, `app/chamados/models.py:3999`.
- Leitor do histórico é `respostas_anteriores`, mostrando resposta superada e fotos de recusa/reabertura; template usa esse leitor: `app/chamados/models.py:3556`, `app/templates/chamados/partials/modal_fio.html:205`.

**Linhas prontas para a tabela de acompanhantes**

| transicao/ato | juiz e escritor | nascentes de produção | persistência e prova |
|---|---|---|---|
| pendente/respondida/validada → respondida, pela resposta nova | Validação por tipo/opções em `responder_pergunta_disputa_core`: `app/chamados/services/disputa_emissao.py:1432`; atribui texto/instante `:1467`, desfaz validação `:1480`, salva `:1508` | **5 sítios**: `app/api/views.py:2620`; `app/chamados/views.py:1283`; `app/chamados/services/acoes_disputa.py:371`; `app/chamados/services/declaracao_texto.py:419`, `:693` | Decisao RESPOSTA no mesmo atomic `disputa_emissao.py:1511`; evento validação desfeita `:1520`; followup `:1555`. Prova `app/chamados/tests/test_porta_resposta_pergunta.py::test_MORDE_resposta_NOVA_invalida_a_validacao_anterior` (`:225`); reenviar mesma resposta move instante (`::test_CARACTERIZACAO_reenviar_a_MESMA_resposta_move_respondida_em`, `:196`) |
| sem texto → respondida automática | Guarda não sobrescrever texto `app/chamados/services/disputa_emissao.py:1647`; escritor `:1652`–`:1657` | **1**: `app/api/foto_recebida.py:48` | texto/instante + Decisao RESPOSTA; selo enumera ato e um sítio: `app/chamados/tests/test_porta_resposta_pergunta.py:69` |
| texto sem instante → respondida pela trilha | Guarda texto e instante ausente `app/chamados/services/disputa_emissao.py:1670`; escritor `:1674`–`:1681` | **1**: command `app/chamados/management/commands/curar_perguntas_em_limbo.py:67` | respondida_em + Decisao RESPOSTA + evento; prova `app/chamados/tests/test_porta_resposta_pergunta.py::test_MORDE_carimbar_pela_trilha_2x_nao_mexe_no_instante` (`:241`) |
| respondida → validada/materializada ou devolvida ao admin | `validar_pergunta` guarda resposta `app/chamados/services/validacao.py:416`, já validada `:419`, implausibilidade `:427`, folga `:435`; `_carimbar_validacao` escreve `:157`–`:164`; `_materializar_uma` `:486`; devolução `:487` | **5 chamadas diretas**: `app/api/foto_recebida.py:51`; `app/chamados/services/acoes_disputa.py:19`, `:379`; `app/chamados/services/declaracao_texto.py:696`; command `app/chamados/management/commands/validar_classe_a.py:100`. Casca acao_validar também chamada por `app/chamados/views.py:501`; `app/chamados/services/fio.py:609`; lote `app/chamados/services/validacao.py:773`; `app/chamados/views_cobrar.py:208` | validada_em/por + Decisao VALIDACAO; recalculo `validacao.py:469`; materialização `:492`; provas `app/chamados/tests/test_e1_entradas.py::test_MORDE_validacao_e_linha_com_o_relogio_do_carimbo` (`:94`), carimbo C6 abaixo |
| validada cuja materialização recusou → respondida aguardando admin | `devolver_validacao_recusada` exige HORA, sem materializada e com motivo `app/chamados/services/validacao.py:213`; zera validação `:222`–`:225` | **1** produção: `app/chamados/services/validacao.py:487` | Decisao DEVOLUCAO `:228`; evento `:246`; pode reabrir dona via sistema `:240`–`:242`; `sem_reabrir` na validação desliga isso `:489` |
| respondida → pendente pela recusa humana | motivo obrigatório `app/chamados/services/validacao.py:588`; validada recusa `:600`; já pendente recusa `:603`; motivo escrito `:611`–`:617`; delega reabrir `:620` | **1 chamada direta**: `app/chamados/services/acoes_disputa.py:80`; casca acessada por `app/chamados/views.py:518`, `app/chamados/services/fio.py:638` | foto/autor/motivo em Decisao RECUSA `validacao.py:620`; followup `:625`; evento antes/depois `:629`; prova `app/chamados/tests/test_porta_validacao_pergunta.py::test_MORDE_recusar_SEM_request_carimba_reabre_e_deixa_trilha` (`:224`) |
| questionário corrigível → pendente; absorção desfeita → pendente | seleção vem do chamador; escritor `reabrir_para_correcao` `app/chamados/services/disputa_emissao.py:1562`; foto/Decisao `:1615`; update `:1632` ou descarimbo em lote `:1630` | **6 sítios**: `app/chamados/views.py:1436`; `app/chamados/services/acoes_disputa.py:103`; `app/chamados/models.py:2293`; recusa `app/chamados/services/validacao.py:620`; ausência rejeitada `app/chamados/services/materializacao.py:688`; command `app/chamados/management/commands/curar_perguntas_em_limbo.py:69` | zera texto/resposta/validação `disputa_emissao.py:1593`; preserva foto; acorda celulas `:1633`; com `desfazer_absorcao` também carimbo. Prova `app/chamados/tests/test_reabertura_fantasma.py::test_reabertura_limpa_o_texto_junto_com_o_carimbo` (`:57`) |
| pergunta sem carimbo → carimbada por fato (C6 deve considerá-la morta após aplicação) | `carimbar` proíbe identidade/tranca `app/chamados/models.py:3174`, recusa fora_do_fato `:3176`; aplica em memória `:3185`, julga C6 `:3195`, se viva desfaz `:3199`, salva `:3208` | **8 chamadas diretas** a carimbar: modelo `app/chamados/models.py:3343`; materialização `app/chamados/services/materializacao.py:567`, `:1283`, `:1437`; S128 `app/chamados/services/disputa_emissao.py:1986`, `:2060`; reconciliador `app/chamados/reconciliador.py:406`, `:596` | 3 campos materializada_em/por/via; simetria preenche resposta/validação somente com texto (`models.py:3186`); opcional lavra `:3205`. Prova `app/chamados/tests/test_o261e_f2_porta_carimbo.py::test_MORDE_encerrar_fantasma_nao_carimba_pergunta_viva` (`:665`), `::test_carimbo_recusado_devolve_a_memoria_como_estava` (`:713`) |
| pergunta → carimbada por identidade | `carimbar_identidade` exige uma das **5 vias de identidade** `app/chamados/models.py:2566`, `:3234`; quem julga é chamador `:3225`; guarda fora_do_fato `:3236`; grava `:3246` | **5 chamadas diretas**: admin `app/chamados/models.py:3120`; remover motivo `:2117`; duplicata `app/chamados/services/fio.py:1360`; paridade `app/chamados/services/materializacao.py:930`; colisão `:1105` | carimbo + recusa_motivo opcional; provas de chamadores/vias exatas `app/chamados/tests/test_o261e_f2_porta_carimbo.py:385`, `:399`, `::test_cada_juiz_de_identidade_grava_a_SUA_via` (`:498`) |
| carimbada → sem carimbo | `descarimbar` guarda fora_do_fato `app/chamados/models.py:3262`; escreve três juntos `:3264` e salva `:3268`; lote exclui fora_do_fato `:3320`–`:3322` | individuais: `app/chamados/services/relavra_via.py:246`; modelo simulação não persistente `app/chamados/models.py:1553`; wrappers fantasma `:3354`, resposta `:3371`, competência `:3378`. Lote: ressuscitar `:1613`; correção `app/chamados/services/disputa_emissao.py:1630` | wrapper fantasma vem do command `app/chamados/management/commands/reabrir_orfas_enterradas.py:157`; competência `app/chamados/services/competencia_trancada.py:279`; resposta `disputa_emissao.py:1485`; prova `app/chamados/tests/test_o261e_f2_porta_carimbo.py::test_descarimbar_leva_os_tres_campos_juntos` (`:754`) |
| carimbada → mesma materialização, nova via fato_materializado | chamador julga fato; porta idempotente/fora_do_fato `app/chamados/models.py:3324`; escrita via única `:3331`–`:3332` | **1** command `app/chamados/management/commands/encerrar_perguntas_com_fato.py:68` | evento antes/depois no command `:70`–`:76`; selo via reconhece porta `app/chamados/tests/test_o261e_f2_porta_carimbo.py:55` |
| via derivada antiga → via derivada de hoje e nova lavra | `via_de_hoje` e recorte `app/chamados/services/relavra_via.py:118`–`:122`; escreve via `:125`, lavra `:126`, save `:127` | command `app/chamados/management/commands/relavrar_via_i6.py:102`; wrapper `relavrar_dia` `app/chamados/services/relavra_via.py:157` sem chamador encontrado, fato também declarado `:20` | evento antes/depois `:132`; prova `app/chamados/tests/test_porta_veredito_pergunta.py::test_MORDE_relavrar_refaz_o_veredito_no_MESMO_save` (`:343`) |
| lavra vazia/antiga → sim/nao/indeterminado | `_pergunta_materializada_de_fato`, exceção → indeterminado `app/chamados/veredito_lavrado.py:262`–`:269`; escritor três campos `:270`–`:275` | **7 chamadas diretas/aliases de lavrar**: `app/chamados/models.py:312`, `:3060`, `:3206`; `app/chamados/services/fio.py:927`, `:1043`; `app/chamados/services/relavra_via.py:126`; command `app/chamados/management/commands/lavrar_perguntas_stale.py:89`; backfill calcula fora e grava lote `app/chamados/management/commands/backfill_veredito_lavrado.py:105`, `:138`–`:146` | três campos lavra; leitores `app/chamados/services/raiox.py:136`, `:184`; prova `app/chamados/tests/test_porta_veredito_pergunta.py::test_MORDE_a_lavra_grava_os_TRES_campos_no_banco` (`:259`) |

**Nascentes acima dos carimbos, para nao perder comando/cron**

- S128 `resolver_perguntas_por_chamado_resolvido`: chamadas `app/chamados/signals.py:117`; `app/chamados/services/becos.py:54`; command `app/ponto/management/commands/reconciliar_fantasmas.py:281`; `app/chamados/reconciliador.py:537`, `:1649`. Decide C6 `disputa_emissao.py:1966`, tranca `:1970`, texto com testemunha `:1979`; segundo ramo sem credor decide C6 `:2048`. `reconciliar_fantasmas --alarme` é cron `app/config/crons.py:392` (alarme; confirmar parâmetro antes de dizer que escreve no cron).
- S144 `materializar_pendentes_aprovadas` acionada no fechamento da disputa por `app/chamados/signals.py:164`; juiz lastro `app/chamados/services/materializacao.py:1433` e C6 na porta `:1437`.
- Sem turno: `lavrar_sem_turno_aberto` chamada em `app/chamados/reconciliador.py:1620`.
- Absolvição célula: chamadas `app/chamados/reconciliador.py:1366`, `:1433`; carimbo `:596`.
- Fantasma por lâmpada: consumidor `app/chamados/reconciliador.py:1376` → `encerrar_orfas_pela_lampada` julga `veredito_orfa` `:1136` → porta `:1148`.
- Desfazer carimbo sem lastro: `app/chamados/services/fabrica_da_celula.py:169`, `:209`; C6 na porta `app/chamados/services/relavra_via.py:242`; possível reabrir DS `:261`.
- Lavra stale tem cron **a cada 5 min com --apply**: `app/config/crons.py:390`.

**L1 acompanhantes**

1. Carimbo fato: juiz do chamador + C6 na porta; exemplos efetivos S128 C6 `disputa_emissao.py:1966` e `models.py:3195`; ausência `declarou_hora`/via `materializacao.py:555` e C6 `models.py:3195`; S144 lastro `materializacao.py:1433` e C6 `models.py:3195`; fantasma `veredito_orfa` `reconciliador.py:1136` e C6 `models.py:3195`.
2. Validar/materializar: guards `validacao.py:416`, `:419`, `:427`; juiz materialização e C6 `materializacao.py:1283`; eventual devolução `validacao.py:213`.
3. Descarimbo sem lastro: recorte `relavra_via.py:238`, C6 `:242`, fora_do_fato `models.py:3262`.
4. Identidade: cada dono julga, porta valida via `models.py:3234` e terminalidade `:3236`; **nao afirmar dois juízes equivalentes do fato**, pois C6 deliberadamente nao é consultado `:3225`.

**L2: usar definição estrita do pedido**

- Carimbo materializada_em/por/via: **0 fora das portas declaradas lidas**; escritor atribuição `_escrever_carimbo` `models.py:3153`, lote `:3320`, portas próprias da via `models.py:3331`, `relavra_via.py:125`. Selo enumera todos `test_o261e_f2_porta_carimbo.py:44`–`:58`; não executado.
- Cache veredito: **1 função / 3 campos atribuídos fora de lavrar**: backfill `backfill_veredito_lavrado.py:138`–`:140`, persistindo pela porta lote `:143`/`:146` → `veredito_lavrado.py:250`. Exceção/divida explicitamente declarada `core/portas.py:335`–`:343`; prova `test_porta_veredito_pergunta.py::test_o_backfill_e_divida_DECLARADA_e_nao_porta` (`:194`).
- Se contar campo validada_em fora de `_carimbar_validacao`, são **4 funções que o persistem**: `devolver_validacao_recusada` (`validacao.py:222`/`:225`); `responder_pergunta_disputa_core` (`disputa_emissao.py:1480`/`:1508`); `reabrir_para_correcao` (`:1593`/`:1632`, ou lote `:1630`); `PerguntaDisputa.carimbar` (`models.py:3191`/`:3208`). Estes são atos inversos/simetria explicitamente declarados, não uma fuga nova inferida; a porta do core é por módulo `core/portas.py:363`–`:390`.
- recusa_motivo possui donos diferentes declarados `core/portas.py:385`–`:390`; sítios atuais lidos: escape `disputa_emissao.py:1497`/`:1508`; recusa admin `validacao.py:611`/`:617`; pendência materializador `materializacao.py:856`/`:857`; recusa materializador `:1108`/`:1109`; identidade central opcional `models.py:3244`/`:3246`, chamada por duplicata `fio.py:1360`, colisão `materializacao.py:1105`. Não forçar “um escritor declarado” inexistente.

**L3**

- Nenhum dos **4 rótulos derivados** é demonstrado terminal: carimbo pode cair e resposta nova desfaz validação `models.py:3259`, `disputa_emissao.py:1480`.
- `fora_do_fato` é **terminal declarado quanto ao retorno à identidade do fato**, conjunto duplicata/motivo_retratado `models.py:2567`, `:3250`. Nascente duplicata `fio.py:1360`, retirada motivo `models.py:2117`. Carimbo, descarimbo e lote barram retorno `models.py:3176`, `:3236`, `:3262`, `:3320`.
- **Cuidado na contagem L3 pedida:** há operações que mudam campos mesmo fora_do_fato: `reabrir_para_correcao` padrão limpa texto/carimbos resposta/validação e não filtra fora_do_fato (`disputa_emissao.py:1593`, `:1632`), mas conserva materializada/via; `carimbar_identidade` pode trocar entre duas vias fora_do_fato (`models.py:3236`). Portanto “nenhuma operação de produção sai” só é provado se o estado analisado for o conjunto fora_do_fato, não cada via literal nem todos os campos.

**Incertezas reais**

- Comando `python` indisponível; não executei testes nem censo AST próprio; as contagens acima são sítios lidos/busca, não resultado de suite. [nao verificado quanto ao estado dos testes] [nao verificado]
- Cabeçalhos de core/portas e de serviços possuem números históricos diferentes do código atual; use os sítios atuais listados, não “9 escritores” ou “5 sítios” históricos (`core/portas.py:358`, `core/portas.py:386`, teste atual resposta `test_porta_resposta_pergunta.py:69`).
- Não li integralmente todos os chamadores transientes de materialização/reconciliação; “todas nascentes” acima é exaustivo para chamadas diretas das portas pesquisadas, não árvore inteira transitiva. [nao verificado para exaustividade transitiva]

## 14/11 21:43 · sessao d374deaaf163 · - · - · origem=Ronald (tmux)

Leitura do recorte app/PWA/API concluida; referencias abaixo sao da copia, sem executar testes. [nao verificado: execucao dos testes] [nao verificado]

**Transicoes da fila do app**

| Transicao observavel | Juiz/operacao | Escritor/persistencia | Nascentes e leitores | Prova |
|---|---|---|---|---|
| Sem item local → item em `batidas_pendentes` | `registrarBatidaOffline`; obtem gap e monta payload (`app/static/pwa.js:107`) | IndexedDB `.add`: tipo, timestamp corrigido, timestamp bruto, gap, coordenadas, accuracy, captured_at, foto (`app/static/pwa.js:112`) | **3 chamadas alcancaveis pelas telas atuais**: v2 erro de rede `app/templates/ponto/bater_ponto_v2.html:333`; v2 offline `:346`; triagem offline `app/templates/ponto/triagem/apto.html:282`. Leitores: sync `.getAll` (`app/static/pwa.js:130`), indicador `.count` (`:188`). Telas atuais selecionadas em `app/ponto/views.py:228` e `:359`. | Forma/GPS: `app/ponto/tests/test_l030_forma_js.py::test_MORDE_fonte_dos_dois_templates` (`:72`); renderizacao: `app/ponto/tests/test_l030_templates_renderizados.py::test_MORDE_as_DUAS_cascas_sem_interromper_POST_ou_fila` (`:14`). Nao prova execucao real do IndexedDB. |
| Item pendente → item removido, apos resposta HTTP bem-sucedida | `sincronizarBatidas`, `resp.ok`, depois parse JSON (`app/static/pwa.js:158`) | `.delete(b.id)` (`app/static/pwa.js:162`); nenhum campo local `sincronizada` e gravado nesse ramo (`:158`). | POST `/api/ponto/bater/` (`app/static/pwa.js:153`); gatilhos mensagem SW (`:16`), evento online (`:320`), inicializacao online (`:328`). | Nenhum teste localizado da semantica de remocao IndexedDB. [nao verificado] |
| Item pendente → item removido, apos HTTP 400 | `sincronizarBatidas`, `resp.status === 400` (`app/static/pwa.js:166`) | `.delete(b.id)` (`app/static/pwa.js:172`); somente `console.warn` com erro (`:169`). | Mesmos 3 gatilhos (`app/static/pwa.js:16`, `:320`, `:328`). | Nenhum teste localizado da semantica de remocao IndexedDB. [nao verificado] |

Rede/5xx/excecao preservam item; outros HTTP tambem nao entram nos dois ramos de remocao. Isso e ausencia de transicao, nao estado novo (`app/static/pwa.js:158`, `:166`, `:177`).

O SW recebe `sync-batidas`, delega para paginas abertas ou notifica para abrir o app; nao faz POST nem le IndexedDB nesse handler (`app/static/sw.js:112`). Registro de `sync-batidas` por `.sync.register` nao localizado. [nao verificado]

**Nascentes app para nascimento da Batida no servidor: 3 sitios**

1. API UI: `app/api/views.py:654` → `registrar_batida`, origem app, canal, sequencia, GPS e timestamp bruto (`:654`).
2. API core: `app/api/views_core.py:645` → mesmo escritor/campos (`:645`).
3. Web: `app/ponto/services/triagem_batida.py:702` → mesmo escritor, origem app, canal, sequencia, GPS, justificativa e aprovacao decididas pelo ramo (`:702`). Entrada via `bater_ponto` (`app/ponto/views.py:349`).

As duas APIs sao roteadas separadamente: UI `app/config/urls.py:37` → `app/api/urls.py:67`; core `app/config/urls_core.py:6` → `app/api/urls_core.py:14`.

**L1: nascimento app tem varios juizes anteriores ao escritor**

- Situacao fora `ativo/afastado` retorna 403 nas duas APIs (`app/api/views.py:403`; `app/api/views_core.py:389`).
- Tipo fora E/S retorna 400 (`app/api/views.py:426`; `app/api/views_core.py:412`).
- Dedup API ±5 minutos por tipo, **sem filtro de retratacao**, retorna existente (`app/api/views.py:527`; `app/api/views_core.py:513`). Dedup web dos 5 minutos anteriores tambem sem filtro de retratacao (`app/ponto/services/triagem_batida.py:662`).
- Depois, a porta decide novamente: dedup mesmo tipo ativo ±60s (`app/ponto/registro_batida.py:106`), chave exata inclusive retratada (`:120`), competencia (`:130`), oposto (`:138`).
- Tipo de nascimento e corrigido ao vivo por `proximo_tipo_de` (`app/api/views.py:570`; `app/api/views_core.py:556`) ou retroativamente por marcos (`app/api/views.py:583`).
- Frontend v2 exige foto antes do POST/enfileiramento: `garantirRequisitos` retorna false sem foto (`app/templates/ponto/bater_ponto_v2.html:285`), chamada antes do ramo online/offline (`:312`).
- Web decide nascimento com `aprovada=False` por fora de horario ou sinal de acesso (`app/ponto/services/triagem_batida.py:686`); APIs deixam o default `True` (`app/ponto/models.py:63`, `app/api/views.py:654`, `app/api/views_core.py:645`).

Provas de nascimento/recusa/idempotencia:

- `app/api/tests/test_l030_batida_pela_porta.py::test_MORDE_sem_coordenadas_ativo_e_afastado_gravam_nos_dois_kernels` (`:59`).
- `app/api/tests/test_l030_batida_pela_porta.py::test_MORDE_desligado_ferias_e_outro_seguem_barrados_antes_da_porta_nos_dois_kernels` (`:65`).
- `app/ponto/tests/test_batida_offline.py::test_payload_valido_cria_batida` (`:68`).
- `app/ponto/tests/test_batida_offline.py::test_idempotencia_mesmo_timestamp` (`:78`).
- `app/api/tests/test_api_bater_ponto.py::test_double_submit_mesmo_tipo_nao_duplica` (`:613`).
- `app/api/tests/test_api_bater_ponto.py::test_double_tap_tipo_oposto_em_segundos_nao_cria` (`:634`).

**Aprovacao/exclusao da antiga origem offline**

- `offline_pendente + aprovada=False` → `app + aprovada=True`, por POST `acao=aprovar`, com aprovador/data, via `anotar` (`app/ponto/views.py:459`, `:474`); escritor `app/ponto/registro_batida.py:252`.
- Mesma selecao → Batida apagada, por POST `acao=rejeitar`, via `apagar` (`app/ponto/views.py:480`); escritor `.delete` e trilha posterior (`app/ponto/registro_batida.py:285`).
- Prova de exclusao com trilha: `app/ponto/tests/test_offline_pendentes_trilha.py::test_rejeitar_deixa_trilha` (`:31`).
- Antigo endpoint `batida_offline` so retorna 410 (`app/ponto/views.py:422`); nenhuma nascente atual de criacao com origem offline_pendente localizada fora de testes/migrations. [nao verificado: inexistencia absoluta]
- Portanto nao afirmar que `offline_pendente` e alcancado hoje em producao apenas porque o ramo de decisao permanece; existe o choice (`app/ponto/models.py:17`) e a tela leitora (`app/ponto/views.py:444`).

**Foto**

- 3 anexos inline: APIs (`app/api/views.py:772`; `app/api/views_core.py:766`) e web (`app/ponto/services/triagem_batida.py:752`).
- 2 anexos posteriores pelo upload API (`app/api/views.py:847`; `app/api/views_core.py:831`).
- Todos chegam a `anexar_foto`: `Batida.foto` via `anotar`; APIs inline tambem criam `FotoBatida`; upload posterior faz `update_or_create` (`app/ponto/registro_batida.py:296`). Outras nascentes de foto ficam para o censo geral.

**Incertezas e precisao de escopo**

- Codigo nativo nao esta no repo, conforme a propria documentacao do parser; sua outbox nao foi lida (`app/api/hora_do_aparelho.py:3`). Estados/retries nativos: [nao verificado].
- Existem **5 chamadas lexicais** ao enfileiramento em templates: as 3 atuais citadas e 2 no template antigo `app/templates/ponto/bater_ponto.html:543`, `:597`; nao localizei renderizacao atual desse template, enquanto a view escolhe triagem ou v2 (`app/ponto/views.py:228`, `:359`). As 2 antigas nao devem inflar nascentes comprovadas.
- Fila guarda timestamp corrigido/gap/accuracy/captured_at (`app/static/pwa.js:108`), mas o formulario de sync envia o timestamp bruto, coordenadas e foto, sem esses quatro campos (`:143`). Backend deriva hora efetiva pelo timestamp bruto (`app/api/views.py:506`): aceita atraso entre 0 e 48h, senao usa agora (`app/api/hora_do_aparelho.py:135`). Campos `timestamp_gap_ms`/`sincronizado_em` existem (`app/ponto/models.py:75`), sem preenchimento nessas nascentes (`app/api/views.py:654`, `app/api/views_core.py:645`, `app/ponto/services/triagem_batida.py:702`).
- Nenhum dado de frota foi recebido para BATIDA; comentarios historicos do codigo nao foram tratados como medicao atual. [nao verificado]

## 14/11 21:43 · sessao e1cae7a46b02 · - · - · origem=Ronald (tmux)

Auditoria de provas e portas concluida; nenhum teste executado e nenhuma escrita feita [nao verificado por trilha externa]. [nao verificado]

- **Porta declarada:** `ferias/services.py` para os 3 modelos (`app/core/portas.py:488-490`). O selo exige zero escritores fora dela em `app/ponto/tests/test_chokepoint_ausencia_gate.py::test_MORDE_nenhuma_escrita_da_familia_fora_da_porta` (`:53-57`).
- **L2:** nenhum escritor externo encontrado pelo censo AST existente; seu alcance e limitado por inferencia de tipos e exclui migrations/tests (`app/core/censo_escritas.py:6-56`). O resultado de 24 sitios atribuiveis inclui uma construcao de preview sem persistencia (`app/ferias/services.py:330`) e nao deve virar contagem de transicoes.
- **L3:** `cancelado` e terminal declarado do **campo status** (`app/ferias/models.py:302-308`), protegido em `app/ferias/services.py:637` e `:954`; o registro ainda pode ser excluido junto do PA (`app/ferias/services.py:872-877`). `concluido` possui saida por cancelamento excepcional e por correcao da janela (`app/ferias/services.py:678-689`, `:707-721`).
- **Solicitacoes avaliadas:** a nascente HTTP so aceita `pendente` (`app/ferias/views.py:781-783`), mas os escritores nao repetem essa guarda (`app/ferias/services.py:756`, `:797`). Classificar aprovada/rejeitada como terminais depende de contar nascentes ou toda operacao de producao.
- **Confirmacao:** a view restringe envio a `nao_enviado/recusado` (`app/ferias/views.py:925`) e resposta a `aguardando` (`:973-974`); os escritores sob trava so vetam cancelamento (`app/ferias/services.py:887`, `:905`). Nao afirmar que `confirmado` e terminal de todas as operacoes.

Provas comportamentais prontas para a matriz:

| Transicao | Prova lida |
|---|---|
| Nascimento nos 3 regimes de data | `app/ferias/test_hj090_volta7.py::test_agendamento_usa_juiz_nos_tres_regimes` (`:115-124`) |
| Intake nasce em_gozo | `app/ferias/test_hj090_volta7.py::test_intake_nao_preserva_default_quando_gozo_ja_comecou` (`:128-132`) |
| Aprovar e materializar | `app/ferias/tests.py::test_aprovar_solicitacao_materializa_agendamento` (`:165-182`) |
| Aprovar sem materializar | `app/ferias/tests.py::test_aprovar_sem_periodo_disponivel_aprova_mas_avisa` (`:184-207`) |
| Rejeitar solicitacao | `app/ferias/tests.py::test_rejeitar_com_motivo_ok` (`:218-226`) |
| agendado → em_gozo | `app/ferias/test_sync_status.py::test_01_agendado_na_janela_vira_em_gozo` (`:49-51`) |
| agendado → concluido | `app/ferias/test_sync_status.py::test_02_agendado_ja_terminado_vira_concluido` (`:53-55`) |
| em_gozo → concluido | `app/ferias/test_sync_status.py::test_03_em_gozo_ja_terminado_vira_concluido` (`:57-59`) |
| Cancelar DP uma vez | `app/ferias/test_cancelar_2x.py::test_duas_fotos_carregadas_antes_devolvem_uma_vez` (`:104-110`) |
| Cancelar admin uma vez | `app/ferias/test_cancelar_2x.py::test_admin_2x_devolve_uma_vez_e_carimba_uma_vez` (`:120-126`) |
| concluido → cancelado | `app/ferias/test_cancelar_2x.py::test_porta_da_concluida_cancela_concluido_uma_vez` (`:144-152`) |
| Correcao concluida exige trilha | `app/ferias/test_corrigir_concluida.py::test_com_justificativa_desfaz_e_grava_trilha` (`:76-87`) |
| Cron nao ressuscita cancelado | `app/ferias/test_cancelar_2x.py::test_cron_com_foto_velha_nao_ressuscita_e_2o_cancelamento_nao_devolve` (`:201-208`) |
| Remarcar usa saldo travado | `app/ferias/test_cancelar_2x.py::test_remarcar_le_o_saldo_da_linha_travada` (`:244-250`) |
| Enviar confirmacao | `app/ferias/test_confirmacao_agendamento.py::test_enviar_seta_aguardando` (`:56-60`) |
| Confirmar | `app/ferias/test_confirmacao_agendamento.py::test_colab_confirma_carimba` (`:68-76`) |
| Recusar | `app/ferias/test_confirmacao_agendamento.py::test_colab_recusa` (`:78-85`) |

O teste chamado `test_select_for_update_aprovacao_unica` executa duas chamadas sequenciais; nao prova concorrencia real (`app/ferias/tests.py:228-242`).

## 14/11 21:43 · sessao d92ef46e9a22 · - · - · origem=Ronald (tmux)

Base de EC: `EscalaColaborador` usa `ativa`, `data_inicio`, `data_fim`, `tipo_escala`, `posto` (`app/escala/models.py:871-877`); unicidade por colaborador+inicio (`app/escala/models.py:1004`). Porta declarada por **arquivo** (`app/core/portas.py:470`, `app/colaboradores/services/vinculo.py:442-448`), nao por uma funcao unica. O censo puro identificado abaixo encontrou 17 escritas atribuiveis; a leitura mostrou mais 2 nao atribuidas pelo censo: `fechar_vigencia:641`, `encerrar_vinculo:845`. Assim, **19 sitios lidos de escrita EC**, todos no arquivo da porta; a limitacao do censo consta em `app/core/censo_escritas.py:15-18,43-47`.

Para economizar na consolidacao, `V = app/colaboradores/services/vinculo.py`.

| Transicao | Juiz e operacao | Escrita e persistencia | Nascentes de producao | Prova que li |
|---|---|---|---|---|
| inexistente -> `ativa=True` | `julgar_vinculo` verifica piso, posto, desligado, idempotencia, ancora obrigatoria/futura (`V:52-147`); `executar_vinculo` | create tipo/posto/inicio/ativa/ancora/origem/feriado (`V:287-297`); historico (`V:354-358`), LogAuditoria criar com foto (`V:381-383`) | UI `app/colaboradores/views.py:423`; regua `app/escala/regua_defesa.py:321`; chat `app/colaboradores/services/troca_pelo_chat.py:75`; command `app/ponto/management/commands/censo_fase_12x36.py:423` | `app/colaboradores/tests/test_trilha_do_vinculo.py::test_vinculo_novo_grava_CRIAR_e_o_anterior_fechado_grava_EDITAR:121` |
| existente ativa/inativa -> `ativa=True`, fim nulo, tipo/posto/ancora novos | mesmo juiz (`V:101-147`); executor ainda decide invalidar ancora automatica se geometria muda (`V:268-273`) | `V:274-283`; Log editar so diferencas (`V:370-379`); HistoricoVinculo (`V:349-358`) | mesmas 4 nascentes acima | `app/colaboradores/tests/test_trilha_do_vinculo.py::test_MORDE_trocar_tipo_de_vinculo_existente_grava_EDITAR_com_antes_e_depois:84`; `app/colaboradores/tests/test_vincular_escala.py::test_admin_ti_reativando_escala_nao_apaga_ancora:70` |
| vigente anterior ativa OU inativa sobreposta -> `ativa=False`, fim D-1 | selecao em `executar_vinculo` (`V:214-247`); filtro `data_inicio<=data_fim` em `fechar_vigencia` (`V:621-641`) | queryset.update (`V:641`); trilha por pk se autor (`V:642-651`); futuras pelo cadastro (`V:659-664`) | `V:214`, `V:241`; `V:862` desligamento; `V:1087` ajuste; `V:1108` desfazer; command `app/escala/management/commands/corrigir_vinculo_vigencia.py:107` | `app/escala/tests/test_vigencia_um_escritor.py::test_MORDE_o_PAR_fecha_o_anterior_e_NAO_toca_o_posterior:72`; `::test_MORDE_trilha_que_falha_DESFAZ_a_escrita:119` |
| ativa com inicio >=D -> `ativa=False`, fim preservado (absorvida) | `absorver_vigencias_posteriores` filtra ativa/inicio>=D e exclui pk preservada (`V:788-795`) | update ativa (`V:799`); Log por pk condicional ao autor (`V:808-816`); Historico sempre (`V:817-820`) | executor `V:253`; command `app/escala/management/commands/corrigir_vinculo_vigencia.py:120` | `app/colaboradores/tests/test_vinculo_linha_do_tempo.py::test_MORDE_o_posterior_e_ABSORVIDO_e_sobra_UM_vigente:82`; `::test_MORDE_a_absorcao_deixa_trilha_nos_DOIS_lugares:95` |
| ativa -> inativa/fim determinado por ato humano ou backfill desligado | `validar_vigencia` (`V:835`); `encerrar_vinculo` | save (`V:843-845`); Log antes/depois no atomic quando mudou (`V:847-851`) | `app/escala/views.py:269`; `app/colaboradores/management/commands/fechar_vinculo_de_desligado.py:56` | `app/colaboradores/tests/test_trilha_do_vinculo.py::test_encerrar_vinculo_com_autor_grava_editar_com_antes_e_depois:157`; `app/escala/tests/test_vigencia_um_escritor.py::test_a_porta_humana_RECUSA_com_mensagem:102` |
| fim<inicio -> ativa/fim reparado | `reabrir_vigencia_impossivel` exige estado atual impossivel, valida novo intervalo e motivo com autor (`V:711-728`) | update (`V:730`); Log + Historico condicionais ao autor (`V:731-743`); futuras (`V:750-752`) | command `app/escala/management/commands/corrigir_vinculo_vigencia.py:86` | `app/escala/tests/test_porta_repara_vigencia_impossivel.py::test_MORDE_vinculo_SADIO_e_RECUSADO:69`; `::test_MORDE_IDEMPOTENTE_chamar_2x_nao_grava_2a_trilha:88` |
| inexistente -> trecho retroativo **inativo** | `validar_vigencia` (`V:969`); comando verifica ancora futura (`app/colaboradores/management/commands/corrigir_escala_retroativa.py:185-189`) | create (`V:970-974`); Log no chamador (`app/colaboradores/management/commands/corrigir_escala_retroativa.py:198-203`) | command `app/colaboradores/management/commands/corrigir_escala_retroativa.py:196` com novo_trecho | selo AST `app/escala/tests/test_vigencia_um_escritor.py::test_UM_ESCRITOR_nenhum_outro_sitio_grava_data_fim_de_vinculo:171` |
| existente -> tipo/ancora/fim corrigidos no lugar | `validar_vigencia` (`V:979`); ancora futura no comando acima | save (`V:980-983`); Log no chamador (`app/colaboradores/management/commands/corrigir_escala_retroativa.py:213-219`) | command `app/colaboradores/management/commands/corrigir_escala_retroativa.py:212`; tambem `app/escala/services/propositor.py:765-771` chama `_rodar` desse command | mesmo selo AST `app/escala/tests/test_vigencia_um_escritor.py:171` |
| ancora existente -> nula | geometria de template mudou + origem automatica (`V:268-273`), OU invalidador ativa/ciclo permitido/ancora anterior ao inicio (`V:990-995`) | primeiro salvo `V:283`; segundo queryset.update `V:995`, sem signal | executor `V:309` -> `app/escala/utils.py:2103-2110` -> invalidador; primeiro direto executor | `app/colaboradores/tests/test_vinculo_ancora_geometria.py::test_geometria_muda_invalida_e_cobra_declaracao:56` |
| conjunto atual -> snapshot anterior; vinculos extras -> inexistentes | valida todos intervalos (`V:1007-1016`); recusa se remover geradora de celula editada (`V:1025-1028`) | delete extras (`V:1029`); update tipo/inicio/fim/ativa restaurados (`V:1031-1032`); futuras (`V:1034-1035`) | recusa aceite `app/chamados/services/acoes_chamado.py:149` | `app/colaboradores/tests/test_escala_aceite.py::test_recusar_escala_reverte_ao_anterior:126` |
| vinculo antigo -> fechado + novo ativo de template derivado | executor de escala auto exige aval com `!`, classe automatica e inicio futuro (`app/escala/services/escala_auto_executor.py:96-100`) | antigo por fechar_vigencia (`V:1087-1088`); create novo (`V:1089-1092`); Historico no executor (`app/escala/services/escala_auto_executor.py:110-113`) | executor `app/escala/services/escala_auto_executor.py:103`; command `app/escala/management/commands/ajustar_escalas_auto.py:95` | `app/escala/tests/test_escala_auto_executor.py::test_MORDE_template_derivado_trilha_e_passado_intocado:66` |
| ajuste ainda nao vigente -> novo excluido + antigo reaberto | executor valida marca sistema, prazo, localizacao antiga/nova (`app/escala/services/escala_auto_executor.py:128-143`); porta recusa geradora humana (`V:1103-1104`) | delete novo (`V:1105`), update antigo ativa=True/fim=None (`V:1106`) | executor `app/escala/services/escala_auto_executor.py:145`; command `app/escala/management/commands/ajustar_escalas_auto.py:39` | `app/escala/tests/test_escala_auto_executor.py::test_MORDE_desfazer_volta_o_vinculo_antigo:86` |
| ajuste ja vigente -> novo fechado + outro vinculo ativo do tipo antigo | mesmos juizes acima; ramo volta!=inicio (`V:1101`, `V:1108`) | fechar_vigencia (`V:1108-1109`), create (`V:1110-1113`); historico executor (`app/escala/services/escala_auto_executor.py:149-151`) | mesmos executor/command acima | mesmo teste de desfazer acima; cobertura especifica deste ramo [nao verificado] |
| calendario atual -> outro calendario/None | comparacao antes/depois (`V:459-463`); `trocar_calendario_de_folga` | save (`V:462`), futuras (`V:465`), Log (`V:467-471`) | `app/colaboradores/views.py:239`; lote `app/escala/views.py:409` | `app/colaboradores/tests/test_trilha_do_vinculo.py::test_MORDE_trocar_calendario_grava_editar_com_os_dois_calendarios_e_e_idempotente:141` |
| calendario nulo -> calendario padrao template | `_copiar_folga_padrao` exige padrao nao nulo e vinculo sem calendario (`V:157-159`) | save (`V:161-162`), foto de folgas atual/proximo (`V:163-166`) | executor `V:284`, `V:298` | `app/colaboradores/tests/test_vinculo_folga_padrao.py` [nome de teste nao verificado] |
| trabalha_em_feriado atual -> desejado | igualdade encerra sem efeito (`V:890-891`); `marcar_trabalha_em_feriado` | save (`V:893-894`); regeneracao passada (`V:904-908`) e futura (`V:910-913`) | command `app/colaboradores/management/commands/sincronizar_feriado_admin.py:56`; nao cronificado, catalogado sob ordem (`app/config/crons.py:1421`) | selo geral `app/escala/tests/test_chokepoint_escala_gate.py:55`; prova especifica [nao verificado] |

Leitores comuns dos estados EC: candidatura (`app/escala/alimentacao.py:148-153`); vigencia declarada (`:189-196`); cobre data (`:205-216`); vinculo do dia privilegia geradora da celula dentro do intervalo, depois ativa/inicio (`:269-286`). Signals leem anterior e novo (`app/escala/signals.py:25-28,56-58,113-115`), regeneram vigencia (`:73-83`) e ancora (`:125-134`), invalidam previsto (`:145-146`). Ficha exibe vinculos (`app/colaboradores/services/ficha.py:222-238`). A relacao completa de leitores externos nao e o objeto desta exploracao [nao verificado].

Nascentes indiretas a nao esquecer:
- Regua recebe UI lista (`app/escala/views.py:471`) e legado chamado (`app/chamados/views.py:451` -> `app/escala/regua_defesa.py:329`).
- Chat recebe `app/core/views_ia.py:1008`; tambem escritor dinamico declarado `app/core/intake.py:121`.
- Desligamento so como sitio externo: `app/colaboradores/services/desligamento.py:148-149` -> `V:862`. Nasce na UI `app/colaboradores/views.py:1437` e command `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:57`, cron diario **06:06 --apply** (`app/config/crons.py:440-441`).
- `censo_fase_12x36` tem ramo aplicador `:423`, mas o cron usa **--lavrar**, nao apply (`app/config/crons.py:638-639`).
- Reconciliar vinculo e tripwire entram com **--alarme**, nao representam transicao EC (`app/config/crons.py:435,489`).
- `ajustar_escalas_auto` esta declarado nao cronificado (`app/config/crons.py:1194-1196`).

L1: principais transicoes com mais de um predicado/funcao:
1. nascimento/atualizacao: `julgar_vinculo` (`V:52-147`) chama `recusa_ancora_futura` (`V:133`, `app/escala/models.py:938-972`), e executor decide invalidacao automatica (`V:268-273`).
2. fechamento tem juiz de intervalo Python `validar_vigencia` (`V:565`), filtro equivalente separado no escritor em lote (`V:622,641`) e CHECK de banco declarado (`app/escala/models.py:1005-1018`); teste afirma equivalencia banco/juiz `app/escala/tests/test_vigencia_constraint_e_o_juiz.py::test_MORDE_banco_e_juiz_concordam:79`.
3. reparo de impossivel: estado atual/novo/motivo (`V:711-728`).
4. snapshot: intervalo e celula humana (`V:1015,1027-1028`).
5. ajuste automatico: executor aval/classe/futuro (`app/escala/services/escala_auto_executor.py:96-100`) + fechar_vigencia (`V:622,641`).
6. desfazer: marca/prazo/ajuste existente (`app/escala/services/escala_auto_executor.py:128-142`) + guarda geradora humana/ramo de volta (`V:1101-1104`).

L2: zero fora do **arquivo** declarado nao equivale a zero fora da **funcao**. `fechar_vigencia` declara unico escritor do par `(ativa=False,data_fim)` (`V:580-581`), mas `encerrar_vinculo` o escreve (`V:843-845`), com excecao explicita save/signals (`V:829-833`); snapshot pode restaura-lo (`V:1031-1032`). Reativacoes independentes existem no executor (`V:276-283`), reparador (`V:730`) e desfazer (`V:1106`). Todos no arquivo declarado por `app/core/portas.py:470`. Selo `test_vigencia_um_escritor` verifica chamada da lei, **nao escritor unico**, e exclui fim=None deliberadamente (`app/escala/tests/test_vigencia_um_escritor.py:198-199,206-239`).

L3 EC: **0 estados persistentes comprovados sem saida**. Inativa com fim, absorvida sem fim e ativa podem ser reescritas/reativadas quando mesma data_inicio e escolhida (`V:101-124,259-283`); snapshot tambem restaura flag (`V:1031-1032`). Excluido e ausencia de registro, nao estado do modelo (`V:1029,1105`).

Incertezas: testes apenas lidos, nao executados; nao houve acesso a banco/log; numero de afetados em producao [nao verificado]. Nao encontrei outras nascentes EC em seed/smoke de producao na busca textual e censo; garantia contra SQL dinamico/aliases que escapem do censo [nao verificado]. [nao verificado]

_5330 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
