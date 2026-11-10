# CODEX-ESTADO

> Ultima resposta processada: **10/11/2026 20:15** · publicado em **10/11/2026 20:15** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 10/11 20:15 · sessao 939d1201216d · - · - · origem=Ronald (tmux)

2. **Desligamento**

- Porta HTTP: `POST colaboradores/<cid>/desligar/`, exige login e ação `editar_cadastro`, passa por `julgar_desligamento` e `executar_desligamento` — `app/colaboradores/urls.py:52`, `app/colaboradores/views.py:1419`, `app/colaboradores/views.py:1425`, `app/colaboradores/views.py:1430`.
- Porta agendada: comando seleciona `data_demissao <= hoje` e situação diferente de desligado; com `--apply`, chama o mesmo juiz/executor — `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:38`, `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:46`, `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:51`, `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:57`.
- Há declaração do cron às 06:06, com `--apply`; instalação/execução real não verificada — `app/config/crons.py:440`.
- Futuro dentro do teto de 90 dias somente agenda: grava `data_demissao` e motivo, histórico e auditoria; retorna antes de modificar usuário, situação ou vínculos — `app/colaboradores/services/desligamento.py:39`, `app/colaboradores/services/desligamento.py:57`, `app/colaboradores/services/desligamento.py:82`, `app/colaboradores/services/desligamento.py:85`, `app/colaboradores/services/desligamento.py:101`.
- Efetivação grava `situacao='desligado'`, data e motivo, depois chama `credenciais.definir_acesso(..., False)` — `app/colaboradores/services/desligamento.py:103`, `app/colaboradores/services/desligamento.py:104`, `app/colaboradores/services/desligamento.py:111`.
- `definir_acesso` mantém o registro e vínculo de usuário: altera apenas `usuario.is_active`, salva esse campo e registra auditoria; usuário ausente é ignorado — `app/core/credenciais.py:107`, `app/core/credenciais.py:108`, `app/core/credenciais.py:113`, `app/core/credenciais.py:114`, `app/core/credenciais.py:115`.
- Exclusão/revogação explícita de tokens, sessões, dispositivos ou FCM no desligamento: **não achei**; executor usa somente gravação da situação e `definir_acesso`, cuja implementação salva exclusivamente `is_active` — `app/colaboradores/services/desligamento.py:104`, `app/colaboradores/services/desligamento.py:108`, `app/colaboradores/services/desligamento.py:111`, `app/core/credenciais.py:107`.
- Limpeza de FCM/dispositivos existe em outra operação, `resetar_onboarding`, que o desligamento não chama — `app/core/credenciais.py:150`, `app/core/credenciais.py:165`, `app/core/credenciais.py:172`; `app/colaboradores/services/desligamento.py:103`.
- Demais efeitos: fecha vínculos ativos; fecha disputas abertas, preservando respostas sem veredito; tenta encerrar chamados; cria chamado DP de auditoria e faz `push_dp` — `app/colaboradores/services/desligamento.py:122`, `app/colaboradores/services/desligamento.py:139`, `app/colaboradores/services/desligamento.py:140`, `app/colaboradores/services/desligamento.py:154`, `app/colaboradores/services/desligamento.py:163`, `app/colaboradores/services/desligamento.py:187`.
- Django admin não é outra porta atual: `ColaboradorAdmin` herda `SoLeitura`, que nega inclusão, alteração e exclusão — `app/colaboradores/admin.py:47`, `app/core/admin.py:59`, `app/core/admin.py:62`, `app/core/admin.py:65`.
- Literalmente, existe também porta que limpa `data_demissao`: reativação HTTP chama executor que grava ativo/data nula e reativa usuário — `app/colaboradores/urls.py:53`, `app/colaboradores/views.py:1405`, `app/colaboradores/views.py:1417`, `app/colaboradores/services/desligamento.py:210`, `app/colaboradores/services/desligamento.py:211`, `app/colaboradores/services/desligamento.py:216`.
- Outra porta de produção que atribua desligado/data de demissão: **não achei**. [nao verificado]

3. **Férias**

- `ferias` é opção de `Colaborador.situacao`, mas escritor de `situacao='ferias'`: **não achei**; comentário do caminho PWA também documenta zero escritores e remoção do ramo antigo — `app/colaboradores/models.py:295`, `app/colaboradores/models.py:298`, `app/ponto/services/triagem_batida.py:68`.
- Início pelas datas deriva `AgendamentoFerias.status`; o cron chama `alinhar_status_por_data`, que trava/recarrega o agendamento e salva apenas `status` — `app/ferias/management/commands/sincronizar_status_ferias.py:37`, `app/ferias/management/commands/sincronizar_status_ferias.py:48`, `app/ferias/services.py:598`, `app/ferias/services.py:600`, `app/ferias/services.py:604`.
- Há declaração de cron diário às 06:15 com `--apply`; execução real não verificada — `app/config/crons.py:442`.
- `AgendamentoFerias.save()` espelha férias em `Ausencia`, inclusive quando muda só status — `app/ferias/models.py:444`, `app/ferias/models.py:446`, `app/ferias/models.py:450`, `app/ferias/models.py:386`.
- O espelhamento considera agendado/em_gozo/concluído e cria ausências aprovadas por dia; não altera usuário, tokens, sessões ou FCM — `app/ponto/services/ausencia.py:1178`, `app/ponto/services/ausencia.py:1180`, `app/ponto/services/ausencia.py:1198`, `app/ponto/services/ausencia.py:1202`.
- O juiz de férias atuais usa cobertura de datas, excluindo cancelados; não depende somente de status `em_gozo` — `app/ferias/services.py:47`, `app/ferias/services.py:51`, `app/ferias/services.py:57`, `app/ferias/services.py:63`.
- Bloqueio automático de credencial no início das férias: **não achei**; `alinhar_status_por_data` somente grava status e o `save` chama espelhamento — `app/ferias/services.py:598`, `app/ferias/services.py:604`, `app/ferias/models.py:446`.

Achados adicionais úteis à pergunta 4:

- `detectar_anomalias` chama `bloquear_se_em_ferias`, acrescentando motivo `em_ferias`; apesar do nome/documentação antiga, conferir consumidor para concluir sobre recusa — `app/chamados/detector_anomalias.py:113`, `app/chamados/detector_anomalias.py:115`, `app/chamados/detector_anomalias.py:118`.
- Esse helper consulta férias de hoje, cria chamado `colaborador_em_ferias` idempotente por colaborador/dia e envia push DP — `app/ponto/utils.py:85`, `app/ponto/utils.py:89`, `app/ponto/utils.py:94`, `app/ponto/utils.py:103`, `app/ponto/utils.py:118`, `app/ponto/utils.py:127`.
- `aviso_do_dia` devolve frase explícita “batida registrada”; recebe motivos do detector ou consulta férias pelas datas — `app/ponto/services/aviso_da_batida.py:35`, `app/ponto/services/aviso_da_batida.py:55`, `app/ponto/services/aviso_da_batida.py:69`, `app/ponto/services/aviso_da_batida.py:73`.
- Pauta DP do espelhamento ocorre por recusa de materialização de férias, durante save do agendamento, e não é por si evento pós-batida — `app/ponto/services/ausencia.py:1209`, `app/ponto/services/ausencia.py:1215`, `app/ponto/services/ausencia.py:1220`, `app/ponto/services/ausencia.py:1245`.
- Leitura administrativa `situacao_de_ferias` detecta batidas dentro do período e acrescenta aviso `ferias_com_batida`; não escreve — `app/ferias/situacao.py:269`, `app/ferias/situacao.py:277`, `app/ferias/situacao.py:283`, `app/ferias/situacao.py:285`.

## 10/11 20:15 · sessao e014fb4f63fb · - · - · origem=Ronald (tmux)

Portas encontradas, com foco nas três condições:

| Porta | `situacao` | `data_demissao` | `usuario.is_active` |
|---|---|---|---|
| PWA `/login/` | Exige exatamente `ativo`; portanto `ferias`, `afastado`, `desligado` impedem novo login. `app/core/urls.py:20`; `app/core/views.py:50`; `app/api/login_helpers.py:141` | Não achei leitura no helper. `app/api/login_helpers.py:107` | Delega a `django.contrib.auth.authenticate`; não achei implementação da biblioteca nesta árvore. `app/api/login_helpers.py:13`; `app/api/login_helpers.py:122` |
| API `/api/auth/login/` | Mesmo helper, exige `ativo`. `app/api/urls.py:60`; `app/api/views.py:209`; `app/api/login_helpers.py:141` | Não achei leitura. `app/api/login_helpers.py:107` | Mesma delegação a `authenticate`; checagem interna da biblioteca [nao verificado]. `app/api/login_helpers.py:122` |
| Renovação `/api/auth/refresh/` | Exige `ativo`, quando há Colaborador. `app/api/urls.py:63`; `app/api/views.py:115` | Não achei leitura nessa renovação. `app/api/views.py:76` | Confere explicitamente e devolve 401 se falso. `app/api/views.py:96` |
| JWT access já emitido | Não achei checagem de situação na classe local; chama `super().get_user()` e confere versão da senha. `app/api/credencial.py:186` | Não achei checagem. `app/api/credencial.py:186` | Delega a `JWTAuthentication`; checagem interna da biblioteca [nao verificado]. `app/api/credencial.py:177`; `app/api/credencial.py:188` |
| Sessão Django já existente | Não achei gate local por situação; `/ponto/bater/` usa `login_required`. `app/ponto/views.py:241` | Não achei checagem nessa porta. `app/ponto/views.py:241` | Delega à autenticação/sessão Django e, na API, `SessionAuthentication`; biblioteca [nao verificado]. `app/config/settings/base.py:39`; `app/config/settings/base.py:56`; `app/config/settings/base.py:59` |
| Bridge `/api/webview/token/` → `/webview/entrar/` | Emissão exige autenticação existente; consumo não revalida situação. `app/api/views.py:2995`; `app/api/views.py:3019`; `app/core/views.py:1271` | Não achei leitura nas duas funções. `app/api/views.py:2997`; `app/core/views.py:1256` | Não achei checagem explícita no consumo: após validar token, chama `login(request,t.usuario)`. `app/core/views.py:1271`; `app/core/views.py:1276` |
| `/api/auth/trocar-senha/`, que também emite novos JWTs | Não achei checagem; exige autenticação existente e senha atual. `app/api/urls.py:61`; `app/api/views.py:2863`; `app/api/views.py:2880`; `app/api/views.py:2928` | Não achei leitura nessa função. `app/api/views.py:2865` | Sem checagem explícita adicional, usa autenticação da API. `app/api/views.py:2864` |
| Sessão pelo `/admin/`, se o usuário puder autenticar pelo admin | Não achei implementação local da autenticação do admin. Montagem em `app/config/urls.py:24`; existência dessa porta documentada em `app/core/signals.py:4` | Não achei. `app/config/urls.py:24` | Implementação da biblioteca [nao verificado]. `app/config/urls.py:24` |

A configuração da API aceita JWT **ou sessão Django**; o access dura 12h e refresh 30 dias, com rotação. `app/config/settings/base.py:32`; `app/config/settings/base.py:47`.

A PWA v2 posta efetivamente em `/api/ponto/bater/`, com CSRF da sessão. Também permanece o POST direto em `/ponto/bater/`, que chama `ramo_registrar_es` e daí `registrar_batida`. `app/templates/ponto/bater_ponto_v2.html:322`; `app/ponto/views.py:346`; `app/ponto/services/triagem_batida.py:700`.

Na entrada da API de batida, exige `IsAuthenticated` e vínculo com Colaborador; situação cadastral não veta registro nessa view. A triagem PWA também não tem mais ramo de férias, e a porta final `registrar_batida` não verifica os três campos. `app/api/views.py:398`; `app/api/views.py:406`; `app/ponto/services/triagem_batida.py:68`; `app/ponto/registro_batida.py:94`.

Há um segundo URLconf do kernel com as mesmas portas login/refresh/batida e mesmas verificações locais: `app/config/urls_core.py:6`; `app/api/urls_core.py:9`; `app/api/urls_core.py:12`; `app/api/urls_core.py:14`; `app/api/views_core.py:153`; `app/api/views_core.py:173`; `app/api/views_core.py:196`; `app/api/views_core.py:386`.

O bridge valida uso único e TTL de 120s, mas não reconsulta os três campos no consumo. `app/core/models.py:327`; `app/core/views.py:1270`; `app/core/views.py:1271`; `app/core/views.py:1276`.

As versões declaradas são Django 5.2.14, DRF 3.17.1 e simplejwt 5.5.1; não achei o código dessas bibliotecas nem `AUTHENTICATION_BACKENDS` custom nesta árvore, então não há arquivo:linha local para comprovar o comportamento interno de `is_active`. `app/requirements.txt:13`; `app/requirements.txt:14`; `app/requirements.txt:15`; [nao verificado].

Não achei porta servidor de biometria/código de login. [nao verificado]

## 10/11 20:14 · sessao f0ed2325bcc3 · BO-0611-05 · sonda · origem=Code

Competência informada: **2026-10-21 a 2026-11-20**. [não verificado]

| Campo da sonda | Empresa 1 | Empresa 2 | Empresa 3 | Empresa 4 | Empresa 29 | Referência |
|---|---:|---:|---:|---:|---:|---|
| colabs | 7 | 732 | 163 | 35 | 2 | [não verificado] |
| desligados | 4 | 302 | 58 | 11 | 2 | [não verificado] |
| desligados_com_demissao_na_competencia | 0 | 11 | 7 | 0 | 0 | [não verificado] |
| desligados_sem_data_demissao | 0 | 0 | 0 | 1 | 0 | [não verificado] |
| desligados_com_usuario_ativo | 0 | 131 | 9 | 3 | 0 | [não verificado] |
| situacao_ferias | 0 | 0 | 0 | 0 | 0 | [não verificado] |
| agendamentos_ferias_tocando_competencia | 1 | 9 | 6 | 0 | 0 | [não verificado] |

| Empresa | janela_he, conforme saída | Referência |
|---|---|---|
| 1 | `(True, '2026-09-21', False)` | [não verificado] |
| 2 | `(True, '2026-08-21', True)` | [não verificado] |
| 3 | `(True, '2026-08-21', True)` | [não verificado] |
| 4 | `(True, '2026-08-21', True)` | [não verificado] |
| 29 | `(False, 'None', False)` | [não verificado] |

Contagens apresentadas sem divisão por empresa: [não verificado]

| Campo da sonda | Contagem | Referência |
|---|---:|---|
| batidas_desde_2108_com_data_local_apos_demissao | 20 | [não verificado] |
| batidas_dentro_de_ferias_agendadas_desde_2108 | 17 | [não verificado] |

A saída fornecida não contém linhas explícitas de erro nem indicação de contagens que a sonda não conseguiu realizar. [não verificado]

## 10/11 20:14 · sessao 8ce5f87320ca · - · - · origem=Ronald (tmux)

5. **Sim, o TXT exclui esses casos por regras explícitas.** O veredito vem de `classificar_export`; a montagem só aceita colaboradores com `status='entra'`. (`app/folha/export.py:757`, `app/folha/export.py:706`, `app/folha/export.py:718`)

- **Demissão:** `sai_pela_rescisao` testa `data_demissao is not None` e `data_demissao <= fim_janela`; o resultado vira `fora / rescisao_modulo_proprio`, independentemente de `situacao` ou existência de batida. Demissão posterior ao fim da janela não satisfaz esse teste. (`app/folha/export.py:54`, `app/folha/export.py:60`, `app/folha/export.py:858`)
- **Férias com batida:** cruza as datas de `AgendamentoFerias` com as batidas apuráveis pelo dia local; ignora agendamento cancelado, mas não exige `status='em_gozo'` nem `Colaborador.situacao='ferias'`. Encontrando dias, classifica `fora / ferias_com_batida` e registra a lista em `dias_ferias_com_batida`. (`app/ferias/services.py:47`, `app/ferias/services.py:51`, `app/folha/export.py:491`, `app/folha/export.py:499`, `app/folha/export.py:868`)
- **Aplicação efetiva:** a porta HTTP deriva os fechamentos em leitura, chama `classificar_export` e passa os mesmos objetos e itens para montar o TXT. (`app/folha/views.py:207`, `app/folha/views.py:212`, `app/folha/views.py:268`)
- **Retenção do espelho:** o campo materializado chama-se `motivos_espelho`; o recalculador o alimenta com `motivos_retencao_espelho`. Esse caminho corta a janela na demissão; assim, sua varredura não alcança dias posteriores à demissão. No próprio dia da demissão, ausência de célula/veredito não gera `sem_celula`. (`app/ponto/models.py:451`, `app/ponto/services/fechamento.py:1066`, `app/folha/export.py:1200`, `app/folha/export.py:1229`, `app/folha/export.py:1255`)
- **Universo do fechamento:** inclui qualquer situação diferente de `desligado`; para `desligado`, exige `data_demissao >= início da janela`. Assim, férias entram no universo; desligado com demissão anterior à janela ou nula fica fora desse universo. (`app/ponto/services/fechamento.py:431`, `app/ponto/services/fechamento.py:439`)
- **Visibilidade administrativa:** o painel lê o mesmo classificador e exibe contadores “Bateu em ferias” e “† Rescisao”; esse trecho somente monta a resposta HTML. (`app/folha/views.py:589`, `app/folha/views.py:593`, `app/folha/views.py:638`, `app/folha/views.py:641`)
- **Distinção:** as `travas_aprovacao` são `conta_vazia`, `turno_aberto` e `sem_ancora_12x36`; a exclusão por rescisão/férias ocorre na classificação de entrada do TXT. (`app/folha/export.py:822`, `app/folha/export.py:825`, `app/folha/export.py:827`, `app/folha/export.py:858`, `app/folha/export.py:870`)

## 10/11 20:11 · sessao b76f94c08b6a · - · - · origem=Ronald (tmux)

Lista nominal Python foi entregue nas três mensagens: **353 funções, 146 arquivos de `app/`**, pelo método sintático definido; consumidores indiretos de projeções e execução dinâmica não estão demonstrados por esse número [nao verificado].

Os **13 templates leitores diretos**, contando o JS embutido como parte do template:

| Arquivo:linha | Forma |
|---|---|
| `app/templates/colaboradores/detalhe.html:178` | Iteração `batidas_geo`; `b.timestamp`, `tipo`, `origem` em :180–184. |
| `app/templates/ponto/batidas_offline_pendentes.html:53` | Iteração de Batida; comparação `tipo == 'E'` :60; `timestamp_dispositivo` truthiness :71; `timestamp_gap_ms is not None` :78, comparação ±300000 :79; filtro juiz `tem_gps` :96. |
| `app/templates/ponto/materializar_saida_retroativa.html:25` | Truthiness das relações `batida_entrada` e `batida_gerada`; timestamp :27; pk :32; negação da relação :37. |
| `app/templates/ponto/regularizar_retorno.html:18` | `batida_entrada.timestamp` em template. |
| `app/templates/ponto/espelho.html:76` | Iteração `ultimas_batidas`; timestamp :77–78; listas `dia.batidas` :244 e comparação de tipo :246. |
| `app/templates/core/dashboard_supervisor.html:40` | Iteração de entradas `turnos_abertos`; `b.timestamp` :44. |
| `app/templates/core/_d2_feed.html:3` | Iteração; foto por truthiness :6; comparação `tipo == 'E'` :14; filtro `tem_gps` :16. |
| `app/templates/core/_d2_alertas.html:23` | Iteração `turnos_abertos`; `t.timestamp` :26. |
| `app/templates/chamados/fio_colaborador.html:116` | Timestamp/coordenadas de `ultima_geo`; iteração de históricos em :125–126. |
| `app/templates/colaboradores/partials/_calendario_grade.html:121` | Iteração `dia.batidas`, timestamp/tipo/pk, links para portas e procedência. |
| `app/templates/ponto/partials/geo_detalhe_turno.html:4` | `batida.colaborador` e `batida.timestamp`; amostras em :29 são PingGeo, não novas Batida. |
| `app/templates/ponto/partials/painel_justificativa.html:129` | Truthiness de relações `batida_entrada`/`batida_gerada` e timestamps :129–130. |
| `app/templates/core/dashboard2.html:236` | JS lê `d.batida`; truthiness conjunta de coordenadas :242/:287; histórico `h.timestamp` :260; timestamp da batida :277. |

**Um leitor adicional em static:** `app/static/js/geo-colab-map.js:29`, função `abrirGeoColab`: `parseFloat` dos dados de latitude/longitude e recusa por `isNaN(...) || isNaN(...)` em :39. A fila PWA continua no censo separado do estado IndexedDB: `app/static/pwa.js:113`, :132, :162, :172, :190.

**Escritores confirmados:** quatro funções primitivas, cinco operações, todas em `app/ponto/registro_batida.py`:

- `registrar_batida`: `create` em :152 e :157.
- `anotar`: `save` em :254.
- `apagar`: `delete` em :291.
- `semear`: `bulk_create` em :335.

`registrar_batida`, `anotar` e `apagar` já estão nas 353 funções leitoras; `semear` foi excluída dessa lista por ser somente escritora. `anexar_foto` escreve Batida indiretamente por `anotar` (:307), não constitui outra operação primitiva de escrita.

**Admin adicional confirmado:** `app/ponto/admin.py:11–15` registra `BatidaAdmin`, com leitura reflexiva de colaborador/timestamp/tipo/origem/aprovação e filtros. Não entrou nas 353 porque a leitura ocorre pelo framework. `AlertaGeofenceAdmin` também expõe a relação `batida` em `readonly_fields`, em `app/ponto/admin.py:30–34`; convém declarar esse leitor reflexivo separado, sem apresentá-lo como descoberta pelo AST.

Os programas fora de `app/` identificados pelo pai ficam separados do subtotal 353: os dois bins leitores e os programas de máscara/selo. Confirmei que a máscara é **escritora efetiva**, não mero registro: alias `R` em `bin/sombra_mascara_pessoal.py:12`, mapa `R.NULO` em :17 e `M.objects.update(...)` em :20; o selo lê por SQL `is not null` em `bin/sombra_selo_pessoal.py:55–58`.

## 10/11 20:08 · sessao 129941409e00 · - · - · origem=Ronald (tmux)

União nominal consolidada: **76 unidades identificadas = 47 anteriores + 29 adicionais**, deduplicadas por função Python, classe de admin, template, arquivo JS ou script. O número cobre os sítios listados; **fechamento de todos os consumidores genéricos de Chamado/Pergunta/Followup: [nao verificado]**.

As 29 adicionais são:

| Grupo | Unidades adicionais e forma |
|---|---|
| AlertaGeofence — 11 | `app/ponto/models.py:258` (`ainda_vale`: `is None`, conserva alerta); `app/ponto/admin.py:31` (filtro por tipo, campos readonly); `app/core/cell.py:15` (queryset ativo → conjunto); `app/core/views.py:199` (`dashboard`, conjunto por batida); `app/core/views.py:407` (`dashboard_supervisor`, comparação `tipo='ponto_fora'`); `app/core/views.py:783` (`painel_tv`, conjunto por colaborador); `app/core/views.py:1167` (`dashboard_dados`, conjunto por batida); `app/ponto/management/commands/reconciliar_alertas_geofence.py:33` (queryset, `is None` em `:42`, escrita `obsoleto_em` em `:52`); `app/ponto/management/commands/testar_pwa.py:291` (ensaio, `.exists()` em `:294`); `app/ponto/management/commands/zerar_dados_operacionais.py:47` (ensaio, `count`, exclusão total em `:85`); `app/templates/core/dashboard_supervisor.html:90` (truthiness da latitude do alerta). |
| Sombra — 3 | `bin/sombra_mascara_pessoal.py:12` (alias `R`, `R.NULO` → `objects.update` em `:17`/`:20`; JSON → `bulk_update` em `:44`/`:62`); `bin/sombra_regras_pessoais.py:36` (`json_`, recursão e regex de coordenada numérica → `None` em `:43`); `bin/sombra_selo_pessoal.py:55` (SQL `IS NOT NULL`; JSONPath por `R.CHAVES_GEO` em `:90`). As regras abrangem Batida/Alerta em `bin/sombra_regras_pessoais.py:75` e `:77`, e contexto de Chamado em `:145`. |
| Template — 1 | `app/templates/chamados/detalhe_local.html:274`: comparação explícita `modulo_origem == "gps_ausente"` e inclusão do relatório. |
| Leitores do módulo/rótulo — 9 | `app/chamados/catalogo/natureza.py:38` (`eh_acionavel`, `not in`); `:44` (`dono_do_chamado`, registro versus admin); `app/chamados/services/desenlace.py:38` (`rotulo`, membership/lookup); `:68` (`desenlace`, comparação de pilha e lookup); `app/chamados/services/verbos.py:158` (`_descritiva`, lookup por subcategoria); `:186` (`coletar_lente`, módulo e lookups em `:206`); `app/api/views.py:60` (`_motivo_label`, `.get`); `app/api/views_mensageria.py:1161` (`api_mensageria_decidir`, lookup por subcategoria); `app/chamados/management/commands/auditar_invariantes_chamados.py:1068` (queryset `modulo_origem__in=MODULOS_AUDITORIA`). |
| Cadeia efetiva de escrita de Chamado — 5 | `app/chamados/models.py:1051` (`_evento_nascimento`, natureza → EMITIR/REGISTRAR); `:1060`/`:1064`/`:1095` (`criar`, motor, estado inicial e INSERT); `app/chamados/services/nota.py:110` (`anexar_ocorrencia`, delega fusão); `:123` (`_fundir_contexto`, alias `_ctx`); `app/chamados/services/contexto.py:144` (`fundir_contexto`, união de listas, ausência por `k not in ctx` em `:155`, `save` em `:163`). |

A normalização de texto zero pode mudar também o responsável pelo chamado: `gps_ausente` pertence a `MODULOS_AUDITORIA`, enquanto `gps_fora_geofence` pertence aos módulos de validação administrativa; o nascimento segue essa natureza (`app/chamados/catalogo/natureza.py:18`; `app/chamados/catalogo/natureza.py:29`; `app/chamados/models.py:1051`).

Declarações auditadas, **sem somar como leitores de instância**: módulo `app/chamados/catalogo/modulos.py:162`; pergunta `app/chamados/catalogo/perguntas.py:88`; motivo `app/chamados/catalogo/motivos.py:42`; rótulo/comentário `app/chamados/services/verbos.py:77` e `:150`; rótulo `app/chamados/services/desenlace.py:21`; pergunta legada `app/chamados/models.py:3598`; mapas API `app/api/views.py:47` e `app/api/views_core.py:47`; opção do formulário `app/templates/chamados/partials/modal_fio.html:402`. `describe._catalogo` apenas inspeciona essas definições e fica fora do total (`app/core/management/commands/describe.py:114`).

`Advertencia.detalhe_json` recebe a cópia das coordenadas (`app/chamados/services/acoes_chamado.py:197`); outro leitor nominal dessas chaves não foi localizado. **Leitura dinâmica/reflexiva desse JSON: [nao verificado]**.

## 10/11 20:05 · sessao 2f185fdf9cd5 · BO-DISPUTA-SEM-PERGUNTA · construtor · origem=Code

Delta implementado: o replay consome os seis argumentos fixados no ensaio e aceita somente `criada=False` com o mesmo PK. Resultado divergente desfaz seu próprio savepoint e registra `replay-divergente` pelo observador existente. (`chamados/services/disputa_emissao.py:484`, `:688`, `:694`, `:698`, `:702`)

Dona fechada não recebe save; a defesa para dona aberta usa exclusivamente `agregar_estrangeiras`, sem contexto. `_replay` saiu dos dois helpers. O ramo de agregação permaneceu com seu fluxo anterior. (`chamados/services/disputa_emissao.py:315`, `:413`, `:592`, `:615`, `:701`)

Arquivos tocados nesta volta:

- [disputa_emissao.py](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/services/disputa_emissao.py:484): coletor e replay fixado.
- [test_bo_disputa_volta5_replay_red_memoria.py](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/test_bo_disputa_volta5_replay_red_memoria.py:25): caracterização invertida e renomeada.
- [test_bo_disputa_volta5_memoria.py](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/test_bo_disputa_volta5_memoria.py:156): argumentos exatos, divergências, defesa da aberta e mutantes.
- [test_bo_disputa_volta5.py](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/test_bo_disputa_volta5.py:135): cenário ORM solicitado.
- [test_bo_disputa_volta4_memoria.py](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/test_bo_disputa_volta4_memoria.py:44): expectativas anteriores de replay ajustadas.
- [test_bo_disputa_sem_pergunta_memoria.py](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:45): callbacks aceitam o coletor; mutante acompanha três atomics.
- [test_bo_disputa_sem_pergunta.py](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/test_bo_disputa_sem_pergunta.py:58): callback de falha encaminha o coletor.
- [test_medida_turno_do_par.py](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/core/tests/test_medida_turno_do_par.py:267): registro das linhas deslocadas.

F1–F3 e destino do ensaio:

| Item | Conferência |
|---|---|
| F1 | Confirmado antes da alteração: o RED terminou em `2 != 1`. A precedência permanece data do motivo antes de `ultima_batida`; o replay agora não chama resolvedor nem gerador. (`disputa_emissao.py:355`, `:361`, `:370`, `:693`; `test_bo_disputa_volta5_replay_red_memoria.py:25`) |
| F2 | Não encontrei contraexemplo nesse fluxo: qualquer aberta conduz à agregação e retorna antes do nascimento, inclusive duas abertas incoerentes. A eleição da pergunta usa o colaborador da disputa; a recuperação também exige esse colaborador. (`disputa_emissao.py:612`, `:615`, `:633`, `:687`; `chamados/models.py:2846`, `:2907`) |
| F3 | Confere quanto às escritas: reuso conserva a dona e altera a pergunta; inserção usa `disputa=disputa`. Precisão: a porta também **lê** a disputa antes disso para derivar episódio/identidade. (`chamados/models.py:2866`, `:2875`, `:2965`, `:2982`, `:3008`, `:3014`) |
| R2: pai desfeito | Não pode ser destino devolvido pelo derivador: nasce com categoria `disputa_supervisao`, excluída literalmente da consulta. Portanto não foi necessário substituir esse destino por `None`. (`disputa_emissao.py:201`, `:639`) |

A porta ainda deriva episódio BATIDA do contexto da dona. Se isso produzir outra identidade, a guarda rejeita o resultado; intervalo é MARCO e não recebe esse episódio. (`chamados/models.py:2866`, `:3460`; `disputa_emissao.py:694`)

T1 passou com chamadas `[(2, DIA), (dona.pk, DIA)]`, zero reatar, DS/chamados `[1, 0]`, uma pergunta total, reaponte preservado e zero save na dona. (`test_bo_disputa_volta5_replay_red_memoria.py:59`) [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_bo_disputa_volta5_replay_red_memoria.py
```

```text
Ran 1 test
OK
```

Os seis módulos em memória passaram, incluindo os testes anteriores ajustados. (`test_bo_disputa_sem_pergunta_memoria.py:291`, `:378`; `test_bo_disputa_volta4_memoria.py:44`, `:85`; `test_bo_disputa_volta5_memoria.py:382`) [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m unittest \
  chamados.tests.test_bo_disputa_sem_pergunta_memoria \
  chamados.tests.test_bo_disputa_volta2_memoria \
  chamados.tests.test_bo_disputa_volta3_memoria \
  chamados.tests.test_bo_disputa_volta4_memoria \
  chamados.tests.test_bo_disputa_volta5_memoria \
  chamados.tests.test_bo_disputa_volta5_replay_red_memoria
```

```text
Ran 47 tests
OK
```

T2: todos os oito mutantes tiveram controle verde com 15 testes e morreram por assertion, sem erro de montagem. Os dois novos reproduzem resolução no replay e aceitação de inserção. (`test_bo_disputa_volta5_memoria.py:26`, `:54`, `:75`, `:394`) [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_bo_disputa_volta5_memoria.py --mutante reentrada
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_bo_disputa_volta5_memoria.py --mutante salva_fechada
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_bo_disputa_volta5_memoria.py --mutante sem_guarda_push
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_bo_disputa_volta5_memoria.py --mutante nasce_sem_pergunta
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_bo_disputa_volta5_memoria.py --mutante eleicao_antes_trava
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_bo_disputa_volta5_memoria.py --mutante dedup_cala_replay
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_bo_disputa_volta5_memoria.py --mutante replay_re_resolve
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_bo_disputa_volta5_memoria.py --mutante replay_insere
```

Saída final de cada comando, emitida pelo harness: (`test_bo_disputa_volta5_memoria.py:401`)

```text
MUTANTE MORTO: reentrada assertions RED: 1
MUTANTE MORTO: salva_fechada assertions RED: 5
MUTANTE MORTO: sem_guarda_push assertions RED: 3
MUTANTE MORTO: nasce_sem_pergunta assertions RED: 4
MUTANTE MORTO: eleicao_antes_trava assertions RED: 1
MUTANTE MORTO: dedup_cala_replay assertions RED: 6
MUTANTE MORTO: replay_re_resolve assertions RED: 2
MUTANTE MORTO: replay_insere assertions RED: 1
```

T3 foi escrito com relógio fixo, porta real, dona fechada com data D+1, `qtd`/`ultima_em`, batida D e dois atos idênticos. Exige igualdade da linha inteira da dona, das linhas DS/chamados/perguntas, pertencimento da pergunta, zero push e zero reatar. **Execução ORM: [nao verificado].** (`test_bo_disputa_volta5.py:135`, `:142`, `:160`, `:173`) [nao verificado]

T4: `[814, 815] → [809, 810]`; `[1298] → [1293]`. A catraca em memória passou com 35 testes. (`core/tests/test_medida_turno_do_par.py:267`)

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B - <<'PY'
import pathlib, types, unittest
p = pathlib.Path('core/tests/test_medida_turno_do_par.py')
suite = types.ModuleType('suite_turno') [nao verificado]
suite.__file__ = str(p.resolve()) [nao verificado]
source = p.read_text().replace(
    'from django.test import SimpleTestCase',
    'from unittest import TestCase as SimpleTestCase')
exec(compile(source, str(p), 'exec'), suite.__dict__) [nao verificado]
unittest.main(module=suite, verbosity=1) [nao verificado]
PY
```

```text
Ran 35 tests
OK
```

Quem escreve e quem lê os campos relevantes ao delta:

| Campo | Escritor → leitor |
|---|---|
| Argumentos da pergunta | Catálogo/resolvedor/gerador produzem texto, tipo, opções, destino e dia; coletor conserva exatamente os seis argumentos e PK; replay os consome. (`disputa_emissao.py:426`, `:444`, `:458`, `:484`, `:488`, `:688`) |
| `P.pk`, `P.disputa_id`, `criada` | Porta insere ou devolve existente; núcleo identifica dona; replay exige mesmo PK e falso; wrapper distingue perguntas criadas de retornadas. (`chamados/models.py:3008`, `:3014`; `disputa_emissao.py:569`, `:694`, `:778`) |
| `P.data_evento`, célula, destino | Porta consulta identidade, rechaveia/costura e relavra; replay conserva o dia pedido no ensaio. (`chamados/models.py:2907`, `:2940`, `:2965`, `:2982`, `:3003`; `disputa_emissao.py:693`) |
| `DS.motivos` | Núcleo cria/agrega; modelo remove/reabre acrescentando; núcleo consulta perguntas locais para admissão. (`disputa_emissao.py:561`, `:583`, `:654`; `chamados/models.py:2031`, `:2063`, `:2156`) |
| `DS.contexto`, `qtd`, `ultima_em` | Merge escreve metadados e persistência salva contexto; resolvedor lê data/batida; porta BATIDA lê episódio. Replay não faz esse merge. (`disputa_emissao.py:526`, `:534`, `:535`, `:586`, `:355`, `:370`, `:688`; `chamados/models.py:3463`) |
| `DS.fechada_em` | Modelo fecha/reabre; núcleo, helper e wrapper leem abertura; receivers leem valor, sem exigir transição. (`chamados/models.py:1932`, `:2149`; `disputa_emissao.py:575`, `:595`, `:773`; `chamados/signals.py:28`, `:129`, `:153`) |
| Coletor/causas | Gerador escreve `reusos`; replay lê e registra divergência; helper escreve `agregada`; wrapper lê; observador recebe causas. (`disputa_emissao.py:488`, `:558`, `:597`, `:602`, `:688`, `:698`, `:770`) |

Leitor a leitor de `motivos`, para a política de motivos locais já presente na cópia:

| Leitor | Efeito/pressuposto |
|---|---|
| `api/foto_recebida.py:63`, `:70` | Seleção acompanha dona aberta, mas também exige `ultima_batida` correspondente; transferir só motivo não garante passar essa guarda. (`:67`) |
| `colaboradores/signals.py:74` | Remoção acompanha a dona aberta selecionada. (`:77`) |
| `escala/signals.py:158` | Remoção de âncora acompanha a dona aberta selecionada. (`:161`) |
| `chamados/models.py:1997` | Referência documental; execução exige abertura e motivo presente. (`:2031`, `:2036`) |
| `auditar_disputas.py:88` | Exige contexto dos motivos mantidos. |
| `auditar_disputas.py:142` | Exibe lista mantida. |
| `auditar_disputas.py:169` | Compara com perguntas locais, com exceções contextuais. (`:182`, `:188`) |
| `auditar_invariantes_chamados.py:50` | Resolve dias dos motivos mantidos; seleciona abertas sem nenhuma linha de pergunta. (`:62`) |
| `chamados/relatorios_disputa.py:52` | Rótulos da lista mantida. |
| `chamados/services/acoes_disputa.py:149` | Mensagem da lista mantida. |
| `chamados/services/fio.py:680` | Mensagem da lista mantida. |
| `pauta_aparelhos_compartilhados.py:49` | Cobertura também exige contexto de dispositivo; motivo sozinho não transfere essa evidência. (`:52`, `:55`) |
| `alertar_disputas_pendentes.py:82` | Log da lista mantida. |
| `templates/chamados/detalhe_local.html:301` | Exibição da lista mantida. |
| `templates/chamados/partials/modal_fio.html:326` | Aviso depende de âncora na lista. |
| `colaboradores/services/drawer.py:75` | Comentário; execução usa perguntas vivas. (`:78`) |

Neste delta, a dona fechada conserva motivos/contexto; portanto esses leitores não mudam por escrita nesses campos. Efeitos reais da alteração da pergunta: **[nao verificado]**. (`disputa_emissao.py:595`, `:701`; `test_bo_disputa_volta5_replay_red_memoria.py:65`)

A resposta da leitura na **cópia** é **D+1**: `_contexto_do_ato` começa pelo contexto guardado; receber somente `ultima_batida` preserva `motivo.data`. A agregação faz esse merge antes do gerador; o resolvedor escolhe a data explícita e não entra no fallback da batida. Não curei esse comportamento. (`disputa_emissao.py:526`, `:527`, `:538`, `:617`, `:620`, `:355`, `:361`, `:370`)

Atribuir esse resultado especificamente à base histórica `6c0517eb9`: **[nao verificado]**. Não consultei Git.

Para a sessão executar, além dos seis módulos em memória acima, ficam estes módulos; a integração ORM permanece **[nao verificado]**:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  chamados.tests.test_bo_disputa_sem_pergunta \
  chamados.tests.test_bo_disputa_volta5 \
  chamados.tests.test_porta_disputa_emissao \
  chamados.tests.test_porta_chamado_e3 \
  chamados.tests.test_sla_pelo_juiz \
  chamados.tests.test_selo_derivacao_pela_data_da_pergunta \
  chamados.tests.test_disputa_s127 \
  chamados.tests.test_c7_red_minimos \
  chamados.tests.test_invariantes_disputa \
  chamados.tests.test_hx_disputa_muda_sem_ata \
  chamados.tests.test_hx_disputa_muda \
  chamados.tests.test_e1_v15f \
  chamados.tests.test_e1_v7_catracas \
  core.tests.test_medida_turno_do_par
```

Não rodei Django/PostgreSQL, receivers reais, concorrência nem ruff; certificação pela suite e revisão final: **[nao verificado]**. [nao verificado]

## 10/11 20:03 · sessao 129941409e00 · - · - · origem=Ronald (tmux)

Consolidado A enviado com os sítios e o inventário contado. Pontos que precisam aparecer no resultado:

- **A6 é parcial:** a divergência texto/número existe (`app/ponto/services/geofence.py:145`; `app/ponto/registro_batida.py:15`), mas o teste atual **já caracteriza** `'0'`/`'0.0'` como truthy (`app/ponto/tests/test_bo061105_tem_gps_juiz_unico.py:79`).
- **A8 não comprova falha:** os dois leitores atuais delegam ao juiz (`app/templates/chamados/partials/_relatorio_geo.html:31`; `app/templates/chamados/partials/modal_fio.html:481`); a fixture sempre fornece as chaves (`app/ponto/tests/test_bo0611_05_templates_geo.py:132`). Consequência de chave ausente em execução: **[nao verificado]**.
- **A9 é parcial:** o módulo `templates_geo` individual não inclui zero no laço (`app/ponto/tests/test_bo0611_05_templates_geo.py:178`), mas o teste do juiz já exercita zeros nos contextos (`app/ponto/tests/test_bo061105_tem_gps_juiz_unico.py:218`). O detector fixa zero como evidência (`app/ponto/tests/test_bo0611_05_vinculo_geo_parcial.py:135`). RED histórico contra a base: **[nao verificado]**.
- `anotar` **não escreve coordenadas**: sua allowlist não as contém e campos externos são recusados (`app/ponto/registro_batida.py:203`; `app/ponto/registro_batida.py:241`). `semear` pode recebê-las genericamente, mas exige banco de ensaio (`app/ponto/registro_batida.py:326`; `app/ponto/registro_batida.py:335`).
- O leitor reflexivo do admin deve constar: `BatidaAdmin` usa `SoLeitura`, que expande todos os campos do modelo (`app/ponto/admin.py:12`; `app/core/admin.py:74`). O realtime não transporta geo: o payload explícito tem somente tipo, identificadores e timestamp (`app/ponto/realtime.py:32`).
- **Contagem consolidada:** 35 unidades diretas em produção + 1 escritor potencial de ensaio + 11 unidades de ingresso/transporte = **47 unidades identificadas**, antes dos delegadores command/signal. A lista nominal e as linhas foram enviadas nas mensagens anteriores; não apresentar esse número como censo de todos os consumidores genéricos de `AlertaGeofence`/`ChamadoColaborador`, cujo alcance depende de incluir a expansão de A7. **[nao verificado: fechamento desse universo genérico]**
- Casa verificável: **COMPLEMENTO de O254 / contrato batida–juiz**, `app/docs/BACKLOG.md:1458`, e BO pai `app/docs/BACKLOG.md:1873`. Os identificadores filhos pedidos não aparecem literalmente no BACKLOG desta cópia. **[nao verificado: casas registradas apenas em 6c0517eb9]**
- Lei/juiz/padrão: L-002/L-007/L-030 (`app/docs/LEIS.md:60`; `app/docs/LEIS.md:65`; `app/docs/LEIS.md:78`); juiz vivo `app/core/juizes.py:1201`; corte de migração e zero inválido `app/docs/CORTES.md:442`; padrão **Single Writer Principle**, `app/docs/GLOSSARIO.md:12`.
- **perguntas de lei: 0**, para migração e coerência de tipo já cobertas pelo corte. Se a cura incluir remover a guarda de posto, há **1** pergunta adicional: “Batida sem GPS e sem posto deve gerar `AlertaGeofence.sem_gps` e chamado `gps_ausente`?” Hoje o escritor exige posto (`app/ponto/services/geofence.py:270`), e o corte proíbe alterar o veredito nessa migração (`app/docs/CORTES.md:442`).

## 10/11 20:03 · sessao b76f94c08b6a · - · - · origem=Ronald (tmux)

Bloco B consolidado, somente leitura.

**Conferência B1–B7**

| Achado | Veredito e forma |
|---|---|
| B1 | **confere**: `app/templates/ponto/bater_ponto_v2.html:287` faz `if(!_fotoBlob)` e retorna `false` em :290; `v2Confirmar` encerra antes do POST em :312. A API já trata selfie ausente como sinal: `app/api/views_core.py:418`, :430–434; outra casca `app/api/views.py:431`, :443. |
| B2 | **parcial**: API usa queryset de mesmo tipo no intervalo inclusivo **±5 min**, `.first()` e truthiness `if dupla`, retornando HTTP 200 sem chamar a porta (`app/api/views_core.py:508`, :510, :514, :517; réplica `app/api/views.py:521`, :523, :527, :530). Web usa **[agora−5 min, agora]**, não ±5 min (`app/ponto/services/triagem_batida.py:662`, :663, :669–670). Não é demonstrado que cada tentativa seja um fato distinto: a idempotência é exigida em `CLAUDE.md:175`; a chave exata existe em `app/ponto/models.py:106` e `app/ponto/registro_batida.py:120`. |
| B3 | **parcial**: não impede “chegar a registrar_batida”: chega e a **própria porta** recusa com `(None, conflito)` (`app/ponto/registro_batida.py:145`, :150–151). Janela 300 s com escala/marcos, senão 60 s (`app/ponto/registro_batida.py:21`, :30–32); ainda há exceção quando `_casa_marco_proprio` responde True (:35–49, :145). As APIs consultam a mesma janela `_janela_oposto as _jop`, queryset excluindo tipo e retratadas, e respondem 200 `ignorada_intervalo` (`app/api/views_core.py:658–678`; réplica `app/api/views.py:666–686`). A web consulta oposta em **±60 s fixos**, sem `retratada_em` no filtro (`app/ponto/services/triagem_batida.py:714–725`), portanto pode devolver id nulo quando a porta recusou no envelope de 300 s. |
| B4 | **confere**: `tipo == 'E'`, truthiness de `context.get('turno_aberto')` e ausência de `request.POST.get('confirmar')` gravam sessão, marcam `aguardando_confirmacao=True` e fazem `return render` antes do ramo registrador (`app/ponto/views.py:316–330`; ramo em :346–350). A presença do turno é alimentada perguntando a `turno_aberto_de as _tad`, não somente pela última marcação (`app/ponto/services/triagem_batida.py:198–204`). |
| B5 | **confere**, com complemento: login lê `getattr(colab, 'situacao', 'ativo') != 'ativo'` em `app/api/login_helpers.py:141` e retorna erro :142. Refresh também tem cópia em **ambos** os kernels: `app/api/views_core.py:173–177` e `app/api/views.py:116–120`. Logo inclui férias e qualquer outro valor diferente de ativo; a mensagem do refresh chama tudo de “desligado”. São guardas de autenticação anteriores ao endpoint de bater, não 403 internos ao kernel da batida. |
| B6 | **confere**: `resp.ok` em `app/static/pwa.js:158` aceita o 200; lê JSON em :159 e não consulta `data.status`, apagando IndexedDB em :162. Também apaga 400 em :166–174. Online v2 faz `if(r.ok) v2Sucesso(...)` sem ler payload (`app/templates/ponto/bater_ponto_v2.html:327`). |
| B7 | **confere**: exigência do selo modal depende da presença textual de `hxConfirmar(` (`app/core/tests/test_selo_modal_nao_nasce_vazio.py:141–143`). Selo API só visita `If` anteriores à primeira chamada nominal `registrar_batida`, cuja condição mencione latitude/longitude/situacao e contenha `Response(status=403)` (`app/api/tests/test_l030_selo_porta.py:39–64`). Não detecta selfie JS, sessão/render, `ja_registrada` 200, recusa interna da porta, login nem refresh. Os próprios testes ainda exigem a colisão: `app/ponto/tests/test_f9d_t1_caracterizacao_ramo_quente.py:105`, :115–120. |

**(a) Lei, juiz e porta**

- L-030: `app/docs/LEIS.md:78`, `CLAUDE.md:140–141`; corte da obra em `app/docs/CORTES.md:440` manda censo de **toda** barreira e proíbe barrar por outro motivo.
- **Sem juiz registrado para “esta tentativa pode ser materializada?”**, “qual identidade de reenvio?” ou “qual situação pode autenticar?”. O registro da família batida contém período/par, geofence, selfie e GPS (`app/core/juizes.py:1186–1201`), não essas perguntas. Hoje decidem os `If` e filtros acima.
- Juízes auxiliares existentes: selfie ausente em `app/core/juizes.py:1195` → `app/ponto/registro_batida.py:80`; geometria/tipo em `app/core/juizes.py:208–209`. Selfie ausente é **cobrança**, explicitamente no corte `app/docs/CORTES.md:94`, não autorização para impedir registrar.
- Porta canônica: `app/core/portas.py:103` → `app/ponto/registro_batida.py:94`. Web chama em `app/ponto/services/triagem_batida.py:700–713`; APIs em `app/api/views_core.py:639–654` e `app/api/views.py:647–662`.
- `JUIZES.md` é lookup e manda prevalecer o vivo (`app/docs/JUIZES.md:10–15`); sua tabela de geometria está em :20–25.

**(b) Estado e decisor**

- Campo novo: **não**; escolhas novas de campo: **não**, para uma cura que apenas encaminhe o fato à porta existente. Selfie já é opcional (`app/ponto/models.py:78`); foto/GPS ausentes já seguem API (`app/api/views_core.py:417–434`, :639–654).
- Linhas que hoje não nascem **por esses caminhos**: **sim**. Exemplos são a tentativa interceptada pelo retorno de :517, pelo render de `app/ponto/views.py:330` ou pela recusa de `app/ponto/registro_batida.py:151`. Não se deve dizer que a combinação é impossível **globalmente**: autoridade humana já pode criar a colisão (`app/ponto/registro_batida.py:150–156`).
- Muda quem decide admissibilidade: **sim**, ao retirar o veto dos clientes/cascas/guardas para encaminhar à porta; não requer inventar juiz. A definição exata da cura do login geral permanece [nao verificado], porque o corte da batida não especifica permissões para todas as outras operações autenticadas (`app/docs/CORTES.md:440`).
- Existência e quantidade real de batidas hoje perdidas: **depende de dado: contagem de tentativas distintas retidas em cada barreira; colisões por 60/300 s; fila descartada por status; recusas de login/refresh por situação**. O dado fornecido não contém essas contagens [nao verificado].

**(c) Censo fechado de estados locais; limite do censo global**

Método: grep em todos os `.py`, `.html` e `.js` da aplicação para nomes dos retornos, sessão/confirmação e store IndexedDB; AST de produção preservando `ImportFrom.asname`, incluindo `management/`; verificadas buscas específicas em `config/crons.py` e `*/signals*.py`. Não há comparação literal de `ja_registrada`/`ignorada_intervalo` em leitor runtime achado: os usos literais runtime são os seis escritores abaixo; os consumidores de sucesso leem `Response.ok`. Testes são separados, não contados como produção (`app/api/tests/test_l030_selo_porta.py:37` mostra por que a forma precisa ser distinguida do significado).

**28 sítios sintáticos distintos nos três estados locais abaixo**, contados uma vez por `arquivo:linha`; `pop` conta como leitura/escrita no mesmo sítio:

1. **Retornos — 8 sítios**:
   - Escritores `ja_registrada`: `app/api/views_core.py:518`, `app/api/views.py:531`, `app/ponto/services/triagem_batida.py:670` — literal em dict de Response/JsonResponse.
   - Escritores `ignorada_intervalo`: `app/api/views_core.py:669`, `app/api/views.py:677`, `app/ponto/services/triagem_batida.py:721` — literal em dict.
   - Leitores genéricos de sucesso: `app/static/pwa.js:158` e `app/templates/ponto/bater_ponto_v2.html:327` — truthiness de `.ok`, sem comparação de status do payload.

2. **Sessão/confirmação — 15 sítios**:
   - Escritores: `app/ponto/views.py:319` (`session['confirmacao_pendente'] = dict`), :327 (`confirmacao_foto_b64 = ...`), :328 (`confirmacao_foto_nome = ...`), :329 (`context['aguardando_confirmacao']=True`).
   - Leitor contextual: `app/ponto/views.py:233` — truthiness de `.get`.
   - Leitores/deletores: `app/ponto/services/triagem_batida.py:505` (`confirmar and session.get`), :506 (`pop`), :508 e :510 (`cp.get`), :512 (`confirmar and session.get`), :513 e :514 (`pop`, último com default).
   - Templates: `app/templates/ponto/bater_ponto_v2.html:64` (`elif ... or aguardando_confirmacao`); `app/templates/ponto/bater_ponto.html:161` (`and not aguardando_confirmacao`) e :174 (`if aguardando_confirmacao`).
   - Formulário confirmado nasce em `app/templates/ponto/triagem/turno_confirmar.html:11` e `app/templates/ponto/bater_ponto.html:183` e é lido em `app/ponto/views.py:273`, :316; estes são **quatro sítios adicionais se incluir também o campo POST `confirmar`**, total local então **32**. O total 28 acima deliberadamente cobre estados de retorno, sessão/contexto e fila; não incorpora esse campo de entrada.

3. **Fila — 5 sítios**:
   - `app/static/pwa.js:113` — `.add` da linha.
   - :132 — `.getAll` para envio.
   - :162 e :172 — `.delete` por sucesso/400.
   - :190 — `.count` para indicador.
   - Definição do store em :54 é DDL local, separada das cinco operações sobre linhas.

**Guardas/decisores adicionais que a cura toca**, separados da contagem de estados acima:
- selfie: `app/templates/ponto/bater_ponto_v2.html:287` (nega truthiness do blob) e :312 (nega retorno da função);
- situação/autenticação: `app/api/login_helpers.py:141`, `app/api/views_core.py:173`, `app/api/views.py:116` — comparação `!= 'ativo'`;
- duplicidade: querysets `.first()` em `app/api/views_core.py:510`, `app/api/views.py:523`, `app/ponto/services/triagem_batida.py:663`, seguidos de truthiness;
- colisão da porta: `app/ponto/registro_batida.py:107` (janela), :113–115 (`mesma is not None`), :120–122 (mesma chave), :139–145 (`exclude(tipo=tipo)`, `.exists()`), :150 (`not autoridade_humana`);
- confirmação: `app/ponto/views.py:316` — conjunção tipo/truthiness/ausência de confirmar.

**Todos os escritores Batida sob o instrumento existente**: cinco operações do AST `core/censo_escritas`:
`app/ponto/registro_batida.py:152` create; :157 create; :254 save; :291 delete; :335 bulk_create. Fontes do instrumento: `app/core/censo_escritas.py:6–46`, :451, :469; porta declarada `app/core/portas.py:103`. O próprio censo tem inferência por nome e operações não atribuíveis, portanto não é prova absoluta de fluxo dinâmico (`app/core/censo_escritas.py:38–46`).

**Não fechar como “28/32 leitores+escritores de Batida”**: esse é apenas o inventário dos estados locais. Novas linhas também alimentam apuração, tela, comandos, signals e dinheiro. Meu AST amplo achou **218 unidades candidatas em 128 arquivos**, incluindo aliases, mas contém metadados/constantes e não acompanha listas recebidas por parâmetro; portanto **total global de leitores de novas Batida: [nao verificado]**. Exemplos comprovados adicionais:
- `app/ponto/signals.py:35`, :40, :49, :132, :139 — receivers de criação/remoção; `instance` e `created/raw`.
- `app/ponto/signals.py:19`, :108 — timestamp/colaborador por parâmetro.
- `app/ponto/turnos.py:2876` — queryset do universo apurável.
- `app/ponto/services/fechamento.py:633`, :811 — `_bap`, alias de `batidas_apuraveis`.
- `app/folha/export.py:499` — `batidas_apuraveis`.
- `app/ponto/management/commands/marcar_foto_ausente_retro.py:66`, :74 — queryset e juiz da selfie.
- `app/ponto/registro_batida.py:177–178` — chama recálculo.
- Uso de universo por parâmetro é limite que a própria documentação admite (`CLAUDE.md:160–163`), portanto não declarar o AST candidato como censo completo.

**(d) Casa**

**COMPLEMENTO de BO-0611-05**, `app/docs/BACKLOG.md:1873`, pelo corte `app/docs/CORTES.md:440`; não “BO simples”: já há guardas distribuídas por JS/template, três caminhos de gravação e autenticação. Não encontrei os IDs `TEM-GPS-LEITORES` ou `RESTO-DA-CLASSE` no BACKLOG desta cópia [nao verificado: resultado negativo de grep, sem linha citável]. O usuário forneceu que foram mapeados antes em outra árvore; não transformar isso em linha inexistente nesta.

A descrição do pai em `BACKLOG.md:1873` ainda diz API 403 sem coordenada e template interrompendo por GPS: **não confere no vivo desta árvore**. APIs chegam à porta com latitude/longitude (`app/api/views_core.py:639–654`; `app/api/views.py:647–662`); o requisito GPS do v2 termina em `return true` mesmo sem obter coordenadas (`app/templates/ponto/bater_ponto_v2.html:292–297`). A selfie continua barrando, conforme B1.

Não encontrei motivo para classificar como RAIZ-DA-DISPUTA/E/FIO/MOTOR: estas obras têm objetos diferentes explicitados em `app/docs/BACKLOG.md:1839`, :1862–1864; a casa direta já é BO-0611-05.

**(e) Padrão**

`Single Writer Principle`, termo literal em `app/docs/GLOSSARIO.md:12`, com `Architecture Conformance Test` em :15 para selar a **classe** das barreiras. A cura tem de preservar idempotência e a porta, não apagar indiscriminadamente qualquer retorno de reenvio (`CLAUDE.md:175`; `app/ponto/registro_batida.py:120–122`).

**(f) Perguntas de lei candidatas**

`perguntas de lei: 2`

1. **Qual identidade distingue um reenvio do mesmo fato de uma nova marcação do mesmo tipo em instante distinto dentro dos cinco minutos?** A lei exige zero barreira e idempotência (`app/docs/LEIS.md:78`; `CLAUDE.md:175`), mas o envelope temporal vem de decisão do código/comentário (`app/api/views_core.py:491–513`), enquanto a unicidade armazenada é por instante exato (`app/ponto/models.py:106`). Não perguntar novamente se selfie, GPS ou tipo oposto podem barrar: L-030 já responde.
2. **Para quem tem situação diferente de ativo, a L-030 exige permitir login/refresh geral, ou garantir somente acesso à porta de registrar uma batida, conservando as outras restrições da conta?** O corte existente disciplina gravação da batida (`app/docs/CORTES.md:440`) e o corte de credencial disciplina validade/revogação (`app/docs/CORTES.md:184`); nenhum dos dois explicita essa extensão de permissões. Hoje os três `!= 'ativo'` decidem (`app/api/login_helpers.py:141`; `app/api/views_core.py:173`; `app/api/views.py:116`).

## 10/11 20:01 · sessao a12c0edbce44 · - · - · origem=Ronald (tmux)

## BO-0611-05-DINHEIRO-DESLIGADO-FERIAS

**C1: parcial.** Os dois caminhos chegam a `registrar_batida`: `app/api/views_core.py:640` e `app/ponto/services/triagem_batida.py:701`. O primeiro já não bloqueia pela situação (`app/api/views_core.py:393`); o segundo transforma o sinal em justificativa/aprovação para revisão, continuando a gravar (`app/ponto/services/triagem_batida.py:693`). A porta cria `Batida` e dispara fechamento (`app/ponto/registro_batida.py:152`, `app/ponto/registro_batida.py:157`, `app/ponto/registro_batida.py:160`).

A função de evento calcula a competência do fato e chama o escritor sem recusa genérica por exportação (`app/ponto/services/fechamento.py:1809`, `app/ponto/services/fechamento.py:1810`), por `on_commit` (`app/ponto/services/fechamento.py:1824`). **Mas não se pode afirmar que toda batida produz dinheiro novo:** o universo exclui desligado com demissão anterior à janela ou sem data (`app/ponto/services/fechamento.py:432`, `app/ponto/services/fechamento.py:439`); a cadeia conserva guarda temporária de **09/2026** (`app/ponto/services/fechamento.py:36`), aplicada mediante exclusão dos colaboradores de empresas exportadas (`app/ponto/services/fechamento.py:560`, `app/ponto/services/fechamento.py:564`).

O teste fornecido no código cobre ambos os kernels e situações, verifica uma Batida e o retorno de seu id; não afirma nenhum valor de fechamento (`app/api/tests/test_l030_batida_pela_porta.py:39`, `app/api/tests/test_l030_batida_pela_porta.py:44`, `app/api/tests/test_l030_batida_pela_porta.py:50`). Situação textual `ferias` no teste não comprova férias efetivas em `Ausencia`/agendamento (`app/api/tests/test_l030_batida_pela_porta.py:28`, `app/api/tests/test_l030_batida_pela_porta.py:52`).

**(a)** Leis: L-030 manda conservar a batida (`app/docs/LEIS.md:78`); L-113 revoga a recusa de recálculo em exportada, preserva TXT anterior/trilha/DIFF de frota e altera expressamente L-092/L-052 (`app/docs/LEIS.md:153`). Logo a falta de recusa genérica não é defeito por si; a função trata exportada como carimbo `warning`, não como veto (`app/ponto/services/fechamento.py:539`, `app/ponto/services/fechamento.py:541`).

Juiz registrado do valor do par: `MotorBase.calcular_periodo` (`app/core/juizes.py:1188`); efeitos e cobertura da ausência: `efeito_vigente` e `cobertura_ausencia_periodo` (`app/core/juizes.py:252`, `app/core/juizes.py:253`); férias hoje: `em_gozo_hoje` (`app/core/juizes.py:262`); entregue/trancado: `competencia_entregue` e `periodo_trancado` (`app/core/juizes.py:327`, `app/core/juizes.py:328`). A tabela anexa local chama cálculo de `motor_calculo_v2` (`app/docs/JUIZES.md:29`).

**Sem juiz registrado específico para “uma batida de desligado/em férias deve produzir qual dinheiro?”** O caminho atual seleciona o universo, lê a cobertura para folha e monta o motor com a régua existente (`app/ponto/services/fechamento.py:439`, `app/ponto/services/fechamento.py:683`, `app/ponto/services/fechamento.py:735`). Não substituir essa cadeia por uma política presumida. [nao verificado]

Portas: batida `registrar_batida`; evento `_fechamento_por_evento` → `recalcular_por_evento`; escritor financeiro `recalcular_fechamento_mes` (`app/ponto/registro_batida.py:94`, `app/ponto/registro_batida.py:175`, `app/ponto/services/fechamento.py:1798`, `app/ponto/services/fechamento.py:448`). FM nasce por `get_or_create`, salva aberto e atualiza os demais status; `DiaPago` é reconstruído por exclusão e `bulk_create` (`app/ponto/services/fechamento.py:898`, `app/ponto/services/fechamento.py:1137`, `app/ponto/services/fechamento.py:1160`, `app/ponto/services/dia_pago.py:359`).

**(b)** Cura não especificada pelo C1; não cabe inventá-la. Para o **censo solicitado**, não há alteração proposta de estado ou decisor. O estado Batida e os efeitos de fechamento já existem nesta árvore (`app/ponto/registro_batida.py:157`, `app/ponto/services/fechamento.py:1810`). Uma cura futura que passe a recusar, excluir ou mudar o valor dessas batidas pode alterar decisor/combinação de estados, mas seu escopo permanece [nao verificado].

**(c)** Censo auditável de **chamadas diretas**, não um total de todos os leitores/escritores das tabelas: AST em memória de `app/**/*.py`, excluindo `tests/` e `migrations/`, resolvendo `ImportFrom`, `as` e import de módulo; grep complementar. Resultado: **5 chamadas de evento, 21 do escritor mensal, 3 do universo e 5 da competência do fato**. A cifra completa de leitores/escritores financeiros, incluindo templates/JS, permanece **[nao verificado]**; não somar essas chamadas e rotular o resultado de censo de estado.

As cinco chamadas de evento:

- `app/ponto/registro_batida.py:178`: evento da batida.
- `app/ponto/portas/celula.py:458`: evento do ato de célula.
- `app/ponto/portas/he.py:202`: decisão de HE.
- `app/chamados/services/validacao.py:361`: folga aceita.
- `app/chamados/services/validacao.py:446`: resposta validada.

As 21 chamadas do escritor, com sua forma:

- Escrita padrão: `app/folha/management/commands/simular_folha.py:123`; `app/ponto/management/commands/aplicar_09_corte_b.py:212`; `app/ponto/management/commands/celula_veredito_velho.py:149`; `app/ponto/management/commands/desvio_o68b.py:94`; `app/ponto/management/commands/diff_reclassificar_partido.py:161` e `:206`; `app/ponto/management/commands/folga_que_sumiu.py:70`; `app/ponto/management/commands/plano_b_no_dinheiro.py:110` (**alias de módulo `_fech`**); `app/ponto/management/commands/recalcular_fechamento.py:118`; `app/ponto/portas/regen_exportada.py:179`; `app/ponto/services/fechamento.py:1810`; `app/ponto/views.py:1252`.
- Leitura `somente_leitura=True`: `app/folha/management/commands/simular_folha.py:130`; `app/folha/views.py:208` (**alias `_recalc`**); `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:135`; `app/ponto/management/commands/apurar_furo_da_exportada.py:111`; `app/ponto/services/diff_frota.py:84` e `:87`; `app/ponto/services/fechamento.py:1759`.
- Leitura de FM **com escrita de DiaPago**, `somente_leitura=True, lavrar_dias_pagos=True`: `app/ponto/management/commands/diff_calculador.py:140` (**alias `_recalc`**); `app/ponto/management/commands/lavrar_dias_pagos.py:106`.

Leitores do universo: escritor mensal e duas telas (`app/ponto/services/fechamento.py:554`, `app/ponto/views.py:1078`, `app/ponto/views.py:1114`). A forma decisiva é queryset `~Q(situacao='desligado') | (Q(situacao='desligado') & data_demissao__gte=inicio_da_janela)` (`app/ponto/services/fechamento.py:432`, `app/ponto/services/fechamento.py:439`).

`post_save` de Batida também recomputa turnos e publica tempo real (`app/ponto/signals.py:34`, `app/ponto/signals.py:39`); julgamento da célula passa por `cartorio.julgar_celula` (`app/ponto/signals.py:85`, `app/ponto/signals.py:93`). Não há chamada direta adicional de `recalcular_por_evento` nesses signals pelo AST/grep. [nao verificado]

**Não usar como fato a promessa “cron da madrugada rega”** dos comentários de `recalcular_por_evento`: o cadastro declara expressamente que `recalcular_fechamento` **não tem cron**, por desenho (`app/config/crons.py:1130`). O comentário do command que promete recusa de exportada também está desatualizado em relação ao escritor (`app/ponto/management/commands/recalcular_fechamento.py:83`, `app/ponto/services/fechamento.py:539`).

**(d)** Encaixe sugerido: **COMPLEMENTO de O-PROJECOES**, cuja descrição cobre fonte, versão e reconstrução de espelho/DiaPago/fechamento (`app/docs/BACKLOG.md:737`). Não há prova de que este C1 trate de pareamento/dia para encaixá-lo em MOTOR-LE-O-TURNO (`app/docs/BACKLOG.md:1839`). A questão distinta de custo inline já possui casa própria (`app/docs/BACKLOG.md:734`).

**(e)** Padrão: **CQRS com projeções reconstruíveis** (`app/docs/GLOSSARIO.md:24`), mantendo **Single Writer Principle** (`app/docs/GLOSSARIO.md:12`).

**(f)** `perguntas de lei: 0`

O C1 não formula cura nem cenário financeiro sem resposta; a ausência de medição não é lacuna de lei. L-113 já responde a exportada; L-030 responde a conservação da batida (`app/docs/LEIS.md:153`, `app/docs/LEIS.md:78`).

**Depende de dado:** contagem de Batidas efetivamente novas por kernel/situação factual/competência; quantas caem fora do universo, na guarda de 09 exportada, ou produzem alterações em FM/DiaPago; DIFF por campo e competência contra gravado. Nenhuma dessas contagens consta do dado fornecido. [nao verificado]

## 10/11 19:59 · sessao 10354500077f · - · - · origem=Ronald (tmux)

Leitura concluída, sem editar arquivos.

- **F2 confere:** trava e seleção de aberta antecedem os ramos; havendo qualquer aberta, inclusive duas incoerentes, o código entra em agregação e retorna antes do nascimento. Assim, no ramo de nascimento, a única aberta criada pelo próprio ensaio não pode ser dona estrangeira. (`chamados/services/disputa_emissao.py:613`, `:615`, `:618`, `:636`, `:656`)
- **F3 confere quanto à FK da dona:** o reuso escreve célula, destino e relavratura da pergunta existente; retorna a mesma dona. A inserção usa `disputa=disputa`. Ressalva literal: `disputa` também participa das **leituras** de identidade/episódio anteriores; não entra exclusivamente na inserção como insumo de leitura. (`chamados/models.py:2866`, `:2875`, `:2965`, `:2982`, `:3003`, `:3008`, `:3014`)
- **R2, pai desfeito:** `_derivar_chamado_destino` exclui `categoria='disputa_supervisao'`; o pai criado pelo ensaio tem precisamente essa categoria. Portanto, esse pai não pode ser destino devolvido pelo derivador; não é necessário substituir esse destino por `None` nesse cenário. (`chamados/services/disputa_emissao.py:198`, `:201`, `:640`, `:642`)

O resolvedor ainda pode derivar episódio BATIDA do contexto da dona dentro da porta. A guarda de mesmo PK e `criada=False` deve desfazer qualquer resultado divergente. Intervalo pertence à família MARCO e não recebe esse episódio. (`chamados/models.py:2866`, `:3452`, `:3460`; `chamados/services/declaracao_texto.py:42`)

Na **cópia atual**, agregação simples escolhe **D+1**: o merge começa pelo contexto guardado, recebe somente `ultima_batida` e preserva `motivo.data`; o resolvedor consome essa data antes do fallback pela batida. O comportamento antecede o delta solicitado. A base histórica `6c0517eb9` não foi consultada: **[nao verificado]**. (`chamados/services/disputa_emissao.py:530`, `:531`, `:541`, `:619`, `:623`, `:328`, `:354`, `:358`, `:361`, `:370`)

| Campo | Escritores e leitores relevantes |
|---|---|
| `DS.motivos` | Núcleo cria/agrega; modelo poda e reabre acrescentando motivo. Núcleo admite pela existência de perguntas locais. (`chamados/services/disputa_emissao.py:564`, `:576`, `:595`, `:656`; `chamados/models.py:1961`, `:2031`, `:2063`, `:2156`) |
| `DS.contexto`, `qtd`, `ultima_em` | Merge escreve metadados; persistência grava contexto. Resolvedor lê data/batida; derivador lê data; porta BATIDA lê episódio. Busca de acessos específicos encontrou leitura de `qtd` no próprio merge e teste API; nenhum consumidor específico de `ultima_em` nessa busca. (`chamados/services/disputa_emissao.py:191`, `:355`, `:370`, `:530`, `:538`, `:539`, `:588`; `chamados/models.py:3463`; `api/tests/test_api_bater_pwa.py:91`) |
| `DS.fechada_em` | Modelo fecha/reabre; núcleo e wrapper distinguem aberta/fechada; os três receivers verificam valor, sem exigir transição. (`chamados/models.py:1932`, `:2149`; `chamados/services/disputa_emissao.py:578`, `:598`, `:616`, `:778`; `chamados/signals.py:28`, `:129`, `:153`) |
| Pergunta: PK/FK, dia, célula, destino | Porta elege pergunta e distingue reuso/inserção; reuso conserva dona. Escritas de dia também passam pelas portas de fio. (`chamados/models.py:2907`, `:2921`, `:2940`, `:2965`, `:2982`, `:3008`, `:3014`; `chamados/services/fio.py:790`, `:804`, `:979`) |
| `criada` e pergunta devolvida | Gerador separa pergunta retornada de realmente criada; wrapper só notifica motivos de perguntas realmente criadas e locais. (`chamados/services/disputa_emissao.py:484`, `:499`, `:510`, `:511`, `:514`, `:783`) |

Impacto leitor a leitor da política de motivos locais:

| Leitor | Consequência |
|---|---|
| `api/foto_recebida.py:63`, `:70` | Seleção acompanha a dona aberta, mas ainda exige `contexto.ultima_batida.id` correspondente; transferência só de motivo não garante passar essa guarda. (`:67`, `:68`, `:73`) |
| `colaboradores/signals.py:74` | Remoção de `posto_desativado` acompanha a dona aberta selecionada. (`:71`, `:77`) |
| `escala/signals.py:158` | Remoção de `ancora_ausente` acompanha a dona aberta selecionada. (`:155`, `:161`) |
| `chamados/models.py:1997` | É documentação; guardas executáveis verificam fechamento e motivo presente. (`:2031`, `:2033`, `:2036`) |
| `auditar_disputas.py:88` | Auditoria passa a exigir contexto dos motivos mantidos. (`:89`, `:91`) |
| `auditar_disputas.py:142` | Exibe a lista mantida. |
| `auditar_disputas.py:169` | Compara lista com perguntas locais; a interpretação já corresponde ao pertencimento local, com exceção contextual para emissão impossível. (`:182`, `:188`, `:191`) |
| `auditar_invariantes_chamados.py:50` | Resolve dias dos motivos mantidos; I7 continua selecionando abertas sem nenhuma linha de pergunta. (`:51`, `:62`, `:63`) |
| `chamados/relatorios_disputa.py:52` | Exibe rótulos dos motivos mantidos. |
| `chamados/services/acoes_disputa.py:149` | Mensagem usa os motivos mantidos. |
| `chamados/services/fio.py:680` | Mensagem usa os motivos mantidos. |
| `colaboradores/management/commands/pauta_aparelhos_compartilhados.py:49` | Cobertura pode mudar; também exige contexto com motivo/prefixo, que não acompanha a transferência somente de motivos. (`:52`, `:53`, `:55`) |
| `ponto/management/commands/alertar_disputas_pendentes.py:82` | Log usa lista mantida. |
| `templates/chamados/detalhe_local.html:301` | Exibe lista mantida. |
| `templates/chamados/partials/modal_fio.html:326` | Aviso depende da presença local de âncora. |
| `colaboradores/services/drawer.py:75` | Comentário descreve agregado que não poda; execução usa perguntas vivas, sem leitura direta da lista. (`:77`, `:78`) |

Para o **delta de replay com dona fechada**, esses leitores não devem mudar por escrita em motivos/contexto da dona, pois a decisão é persistir nada nela. A alteração da pergunta pode ter efeitos próprios pelos leitores da pergunta; execução desses efeitos permanece **[nao verificado]**. (`chamados/services/disputa_emissao.py:578`; `chamados/models.py:3006`, `:3007`)

## 10/11 19:56 · sessao 51de6528d488 · MAPA-FILA-BO · censo · origem=Code

O vínculo desta cópia ao hash informado não pôde ser conferido por Git: a árvore disponível não tem repositório Git reconhecido. [não verificado]

Em **(b)**, “não cria estado” significa que a cura usa campos, valores e combinações já admitidos pelas portas existentes; a ocorrência efetiva dessas combinações em produção continua [não verificado]. Distingo mudança do decisor de apresentação da mudança do juiz de negócio, conforme a separação entre autoridade e escritor registrada em `app/core/juizes.py:3`.

## DISPUTA-ZUMBI-1110

**(a) Lei, juiz e porta.** Aplicam-se **L-002**, testemunha lê a mesma autoridade, e **L-007**, escritor único com trilha (`app/docs/LEIS.md:60`, `app/docs/LEIS.md:65`). O corte C7 declara **`DisputaSupervisao.pode_fechar`**, registrado no código e no lookup (`app/docs/CORTES.md:272`, `app/core/juizes.py:279`, `app/docs/JUIZES.md:57`).

C7 exige **nenhuma resposta sem veredito e nenhum fio vivo**; a porta canônica é **`DisputaSupervisao.fechar`**, que grava conjuntamente fechamento, autor, resolução e motivo (`app/chamados/models.py:1853`, `app/chamados/models.py:1932`). Os carimbos das perguntas passam por `carimbar` e `carimbar_identidade` (`app/chamados/models.py:3103`, `app/chamados/models.py:3168`).

**Conferência do relato:**

- **Confere:** `lavrar_sem_turno_aberto` já consulta C7 e chama `fechar` (`app/chamados/reconciliador.py:411`, `app/chamados/reconciliador.py:412`).
- **Confere:** o explicador calcula “pode fechar” somente por `not _fio_vivo(ps)`, omitindo a guarda adicional de respostas sem veredito de C7 (`app/chamados/services/explicador.py:136`, `app/chamados/services/explicador.py:137`, `app/chamados/models.py:1853`).
- **“Fechamento só por beco” não confere globalmente:** existem caudas de fechamento na reconciliação e na materialização (`app/chamados/reconciliador.py:1170`, `app/chamados/services/materializacao.py:1316`).
- **“7 carimbadores sem C7” não está demonstrado como sete caminhos sem fechamento:** `encerrar_fantasma` alcança C7 pelo reconciliador; materialização tem caudas compartilhadas; `remover_motivo` chama diretamente `fechar` (`app/chamados/reconciliador.py:1147`, `app/chamados/reconciliador.py:1170`, `app/chamados/services/materializacao.py:1378`, `app/chamados/services/passivo_validado.py:405`, `app/chamados/models.py:2072`).

**(b) Cria estado: não. Muda quem decide: sim, no explicador.** Os campos e motivos de fechamento já existem, e a porta já produz a combinação fechada/resolução/motivo (`app/chamados/models.py:1757`, `app/chamados/models.py:1773`, `app/chamados/models.py:1780`, `app/chamados/models.py:1932`). Porém o bool apresentado deixaria de ser decidido pela negação local de `fio_vivo` e passaria a consumir C7; o juiz legal de fechamento permaneceria o existente (`app/chamados/services/explicador.py:137`, `app/chamados/models.py:1853`).

**(c) Censo do bool cujo decisor muda: 7 ocorrências = 1 escritor + 6 leitores.**

| Papel e forma | Sítio |
|---|---|
| Escritor: atribuição `pode = not _fio_vivo(ps)` | `app/chamados/services/explicador.py:137` |
| Leitor: condição de ternário | `app/chamados/services/explicador.py:138` |
| Leitor: truthiness no `if` | `app/chamados/services/explicador.py:139` |
| Leitor: exportação na chave `pode_fechar` | `app/chamados/services/explicador.py:163` |
| Leitor: truthiness de `dict.get` | `app/chamados/services/desenlace.py:63` |
| Leitor: truthiness em template, cor | `app/templates/chamados/partials/modal_fio.html:434` |
| Leitor: truthiness em template, mensagem | `app/templates/chamados/partials/modal_fio.html:445` |

O transporte preserva o dict: cache, atribuição a `ch.explicacao` e passagem ao alias `desenlace as _desen`; não o contei novamente como decisão (`app/chamados/services/fio.py:312`, `app/chamados/services/fio.py:314`, `app/chamados/services/fio.py:386`, `app/chamados/services/fio.py:403`).

Método: grep global de `pode_fechar`, `por_que_nao_fecha` e transportes de `explicacao`, seguido dos imports, aliases e usos locais de `pode`; as sete ocorrências estão enumeradas acima. Não apareceram consumidores adicionais em commands, crons, signals ou JS de `static/` para esse bool calculado. [não verificado por referência negativa arquivo:linha]

**Censo complementar da costura**, separado dos sete acima: AST de chamadas de método encontrou **13 chamadas de carimbo em 12 funções e 12 chamadas diretas a C7**; a unidade é `ast.Call`, independentemente do nome do receptor:

| Forma | Sítios |
|---|---|
| `.carimbar(...)`: 8 | `app/chamados/models.py:3287`; `app/chamados/reconciliador.py:405`; `app/chamados/reconciliador.py:595`; `app/chamados/services/disputa_emissao.py:1863`; `app/chamados/services/disputa_emissao.py:1937`; `app/chamados/services/materializacao.py:561`; `app/chamados/services/materializacao.py:1277`; `app/chamados/services/materializacao.py:1431` |
| `.carimbar_identidade(...)`: 5 | `app/chamados/models.py:2061`; `app/chamados/models.py:3064`; `app/chamados/services/fio.py:1359`; `app/chamados/services/materializacao.py:924`; `app/chamados/services/materializacao.py:1099` |
| `fechada_em is None and …pode_fechar()`: 6 | `app/chamados/reconciliador.py:411`; `app/chamados/reconciliador.py:611`; `app/chamados/services/becos.py:56`; `app/chamados/services/disputa_emissao.py:1963`; `app/chamados/services/materializacao.py:585`; `app/chamados/services/materializacao.py:1316` |
| `elif …pode_fechar()` | `app/chamados/reconciliador.py:1170` |
| Ternário sobre C7 | `app/chamados/services/becos.py:33` |
| `if not …pode_fechar()`: 2 | `app/chamados/services/competencia_trancada.py:153`; `app/chamados/services/competencia_trancada.py:202` |
| Atribuição seguida de truthiness | `app/chamados/services/disputa_emissao.py:1278`, `app/chamados/services/disputa_emissao.py:1282` |
| `if disp and disp.pode_fechar()` | `app/ponto/services/entrada_sincronizada.py:58` |

O signal genérico de salvamento da pergunta agenda a porta de becos após o commit; esse classificador exige pai resolvido e ausência de perguntas visíveis antes de consultar C7 (`app/chamados/signals.py:227`, `app/chamados/signals.py:238`, `app/chamados/services/becos.py:29`, `app/chamados/services/becos.py:31`, `app/chamados/services/becos.py:33`). O cron correspondente usa `--alarme`; isso não equivale a um fechamento automático geral (`app/config/crons.py:663`).

**(d) Casa:** **COMPLEMENTO de C7-DISPUTA-FECHA-POR-UM-JUIZ**, com interseção na série E3/E4; a casa já manda migrar decisões espalhadas para C6/C7 (`app/docs/BACKLOG.md:1468`, `app/docs/BACKLOG.md:1862`, `app/docs/BACKLOG.md:1863`). Os múltiplos sítios enumerados impedem classificá-lo como BO de sítio único.

**(e) Padrão:** **Single Writer Principle / juiz único**, termo existente, realizado ao retirar a decisão paralela do explicador e consumir C7 (`app/docs/GLOSSARIO.md:12`, `app/chamados/services/explicador.py:137`, `app/core/juizes.py:279`).

**Depende de dado:** contagem dos dez casos por instante do carimbo antes/depois de 22/10, via, estado do pai, respostas sem veredito e resultado atual de C7; também contagem de tentativas de fechamento recusadas. A atribuição histórica ao commit indicado no anexo não foi conferida. [não verificado] [nao verificado]

perguntas de lei: 0

O critério de fechamento e a guarda de resposta sem veredito já estão escritos; a separação histórica dos dez casos é pergunta de dado (`app/docs/CORTES.md:272`, `app/chamados/models.py:1853`).

## PERGUNTA-DUPLICATA-1110

**(a) Lei, juiz e porta.** Aplicam-se **L-001/L-002/L-003/L-007**: origem, autoridade comum, medição pela função real e escritor com trilha; a idempotência integra o contrato estrutural (`app/docs/LEIS.md:59`, `app/docs/LEIS.md:60`, `app/docs/LEIS.md:61`, `app/docs/LEIS.md:65`, `CLAUDE.md:175`).

**Sem juiz registrado para eleger a pergunta canônica.** C8 pergunta pela duplicidade de **chamado** e aponta para `ChamadoColaborador.abrir`; não registra a eleição de `PerguntaDisputa` (`app/core/juizes.py:114`, `app/core/juizes.py:280`). O lookup de fio aponta para `_disputa_do_chamado`, outra pergunta (`app/docs/JUIZES.md:40`).

Quem decide hoje:

- **Identidade:** MARCO por colaborador/dia/motivo/instância; BATIDA por episódio/motivo (`app/chamados/services/fio.py:1372`).
- **Canônica:** menor pk com resposta textual não vazia; na ausência dela, menor pk (`app/chamados/services/fio.py:1225`, `app/chamados/services/fio.py:1232`).
- **Retratabilidade:** `plano_de_retratacao` preserva mérito, episódios distintos e acervo sem episódio (`app/chamados/services/fio.py:1259`, `app/chamados/services/fio.py:1287`, `app/chamados/services/fio.py:1290`, `app/chamados/services/fio.py:1294`).
- **Porta/escritor:** `retratar_para_canonica` → `retratar_duplicata` → `carimbar_identidade`; a gravação física ocorre no modelo (`app/chamados/services/fio.py:1319`, `app/chamados/services/fio.py:1345`, `app/chamados/services/fio.py:1359`, `app/chamados/models.py:3190`).

**Conferência do relato.** Os **486 grupos/848 excedentes** são dados fornecidos sem chave de agrupamento nem pks; sua equivalência a duplicatas segundo o código não está demonstrada. [não verificado] O agrupador exclui conversa e vias fora do fato, distingue famílias e aplica horizonte; o plano também conserva perdedoras não retratáveis (`app/chamados/services/fio.py:1431`, `app/chamados/services/fio.py:1945`, `app/chamados/services/fio.py:1949`, `app/chamados/services/fio.py:1290`).

“Disputa aberta” não basta para demonstrar inflação: o app primeiro consulta `q_vivas`, confirma C6 e só depois classifica pendência por resposta ausente e disputa aberta (`app/chamados/services/pergunta_no_app.py:50`, `app/chamados/services/pergunta_no_app.py:58`, `app/chamados/services/pergunta_no_app.py:71`).

Há ainda uma limitação na proposta de “medir idade”: a pergunta dispõe de data do fato e carimbos posteriores, mas não de carimbo próprio de nascimento; essa ausência está registrada em **O326** (`app/chamados/models.py:2423`, `app/chamados/models.py:2448`, `app/chamados/models.py:2449`, `app/docs/BACKLOG.md:1633`). Idade exata de criação não pode ser inferida desses outros carimbos. [não verificado]

**(b) Cria estado: não. Muda quem decide: não**, no escopo de medir pelo universo existente e retratar apenas os excedentes aceitos pela porta atual. `duplicata` já é valor existente, e a porta já grava via/carimbo/autor/motivo; o nascimento também consulta a eleição existente pelo alias `_canonica` (`app/chamados/models.py:2495`, `app/chamados/models.py:3182`, `app/chamados/models.py:3186`, `app/chamados/models.py:3190`, `app/chamados/models.py:2905`, `app/chamados/models.py:2921`).

Adicionar nascimento próprio ou fato novo de identidade ampliaria esse escopo: são obras já separadas, **O326** e **FATO-A06** (`app/docs/BACKLOG.md:1633`, `app/docs/BACKLOG.md:1867`).

**(c) A condição de censo global dos campos não é acionada por não/não.** Como inventário complementar, contei por AST o perímetro de cinco símbolos — `canonica_do_grupo`, `plano_de_retratacao`, `grupos_de_colisao`, `retratar_para_canonica`, `retratar_duplicata` — resolvendo imports com `as`, inclusive `_canonica`. São **23 chamadas: 19 leituras de decisão e 4 delegações de escrita**, enumeradas abaixo:

| Arquivo | Sítios e forma |
|---|---|
| `app/chamados/management/commands/backfill_decisoes_pergunta.py` | `:276` eleição; `:290` plano/truthiness das categorias em `:295`; `:331` escrita; `:393` eleição após `len>=2` em `:392`; `:396` leitura `['retrata']`; `:485` grupos; `:506` eleição/comparação de pk em `:507`; `:510` plano/`len` em `:512`. |
| `app/chamados/management/commands/dedup_perguntas_fato.py` | `:58` grupos; `:76` eleição; `:81` plano, leitura `['retrata']` em `:88` e truthiness em `:89`. |
| `app/chamados/models.py` | `:2921` eleição pelo alias `_canonica`, condicionada à lista; `is not None` em `:2922`. |
| `app/chamados/services/declaracao_texto.py` | `:253` eleição/comparação de disputa e `_reusavel` em `:254`; `:275` eleição após truthiness da lista em `:271`. |
| `app/chamados/services/fio.py` | `:921` escrita condicionada à lista em `:920`; `:956` eleição/comparação de pk em `:957`; `:967` plano/`any` em `:968`; `:1340` plano; `:1341` escrita em compreensão com truthiness; `:1832` eleição condicionada às sementes em `:1833`; `:2155` eleição/comparação de pk em `:2165`; `:2158` plano/truthiness em `:2166`; `:2180` escrita/leitura `['retratadas']`. |

A unidade é chamada dos cinco símbolos, excluindo testes/migrations; o save físico subsequente não foi somado novamente (`app/chamados/services/fio.py:1359`, `app/chamados/models.py:3190`). Nenhuma referência adicional desses cinco símbolos apareceu em templates, signals ou JS; isso não constitui censo global dos campos. [não verificado por referência negativa arquivo:linha] A menção ao backfill em crons é declaração de papel de ato único (`app/config/crons.py:780`, `app/config/crons.py:783`).

**(d) Casa:** **COMPLEMENTO de FIO-E0-V4 E1**, articulado à E4; o fato próprio de duplicata/identidade fica depois da E4d (`app/docs/BACKLOG.md:1848`, `app/docs/BACKLOG.md:1863`, `app/docs/BACKLOG.md:1867`). Há um resíduo específico sobre episódio retratado, mas não se pode atribuir os 486 grupos a ele sem dados (`app/docs/BACKLOG.md:786`). [não verificado quanto ao alcance]

**(e) Padrão:** **idempotência pela chave do fato**, termo existente; a cura deve consumir a identidade compartilhada pelo nascimento e pelo dedup (`app/docs/GLOSSARIO.md:29`, `app/chamados/models.py:2905`, `app/chamados/services/fio.py:1372`).

**Depende de dado:** reclassificação dos 486 grupos pela chave real; quantidades retratáveis, recusadas por mérito, episódios unidos e acervo sem episódio; distribuição por horizonte/competência; quantos excedentes passam por C6 e entram efetivamente nos contadores. Os 848 não demonstram, isoladamente, inflação. [não verificado]

perguntas de lei: 1

**Quando uma linha BATIDA do acervo sem episódio e uma linha com episódio podem representar o mesmo fato, permitindo que uma retrate a outra?** O código declara essa lei aberta e hoje conserva ambas (`app/chamados/services/fio.py:1274`, `app/chamados/services/fio.py:1275`, `app/chamados/services/fio.py:1294`). Não foi localizada resposta posterior nas leis/cortes pesquisados. [não verificado quanto à ausência global] Depende de dado: quantos grupos fornecidos pertencem a essa classe. [não verificado]

## BO-ROTULO-ENCERRADO

**(a) Lei, juiz e porta.** Aplicam-se **L-002**, testemunha lê a autoridade; **LEIS-UI L2**, tela e ação consultam a mesma função; **L7** trata especificamente de ajuda proveniente do mapa do domínio, não estabelece literalmente “um rótulo por estado” (`app/docs/LEIS.md:60`, `app/docs/LEIS-UI.md:23`, `app/docs/LEIS-UI.md:77`).

**Sem juiz registrado para o texto das ações do seletor.** O registro chamado não contém essa pergunta; hoje os textos são decididos por dois blocos literais de template (`app/core/juizes.py:272`, `app/core/juizes.py:293`, `app/templates/chamados/partials/modal_fio.html:667`, `app/templates/chamados/partials/painel_chamado_turno.html:76`).

Para espera do colaborador, decide `rotulo_para_pessoa`, através do filtro `espera`; para transição de negócio, o juiz registrado é `motor.decidir`, acessado por `transicionar`/`pode_transicionar`, com escritor `_gravar_estado` (`app/chamados/services/verbos.py:138`, `app/chamados/templatetags/chamados_tags.py:63`, `app/core/juizes.py:274`, `app/docs/JUIZES.md:37`, `app/chamados/models.py:119`, `app/chamados/models.py:128`, `app/chamados/models.py:201`).

**Conferência do relato:**

- **Confere:** os dois detalhes usam `espera|first` e mostram “Encerrado” para `resolvido` e `fechado`, conforme o mapa (`app/templates/chamados/detalhe.html:133`, `app/templates/chamados/detalhe.html:134`, `app/templates/chamados/detalhe_local.html:396`, `app/templates/chamados/detalhe_local.html:397`, `app/chamados/services/verbos.py:118`, `app/chamados/services/verbos.py:119`).
- O defeito também alcança **um terceiro seletor**, no fio do colaborador (`app/templates/chamados/fio_colaborador.html:269`, `app/templates/chamados/fio_colaborador.html:270`).
- **Confere:** o modal distingue “Encerrar” de “Encerrar e arquivar” (`app/templates/chamados/partials/modal_fio.html:668`, `app/templates/chamados/partials/modal_fio.html:669`).
- **Não existe a fonte comum pressuposta pela cura descrita:** o modal contém literais, repetidos no painel do turno. Seu comentário distingue explicitamente verbo da ação e valor enviado à view (`app/templates/chamados/partials/modal_fio.html:660`, `app/templates/chamados/partials/modal_fio.html:667`, `app/templates/chamados/partials/painel_chamado_turno.html:76`).

**(b) Cria estado: não. Muda quem decide: sim, na apresentação.** Os valores enviados já existem; a cura centralizaria quem fornece o texto, preservando os `value` e a decisão de negócio (`app/chamados/models.py:31`, `app/chamados/models.py:32`, `app/templates/chamados/detalhe_local.html:396`, `app/templates/chamados/detalhe_local.html:397`). O servidor recebe `novo_status` e chama a mesma porta, que consulta o motor (`app/chamados/services/acoes_chamado.py:287`, `app/chamados/services/acoes_chamado.py:315`, `app/chamados/models.py:123`).

**(c) Censo do texto de espera/seletor: 13 sítios = 3 definidores/renderizadores + 10 leitores.** A unidade é função ou bloco de template; opções irmãs contam como um bloco.

| Papel | Sítio | Forma |
|---|---|---|
| Definidor | `app/chamados/services/verbos.py:114` | Dict `ESPERA_COLAB`, comparação por chave de estado |
| Definidor/renderizador | `app/templates/chamados/partials/modal_fio.html:667` | Texto literal das opções |
| Definidor/renderizador | `app/templates/chamados/partials/painel_chamado_turno.html:76` | Texto literal das opções |
| Leitor | `app/chamados/services/verbos.py:138` | Truthiness/normalização em `:140`, `.get` em `:141` |
| Leitor | `app/chamados/templatetags/chamados_tags.py:53` | Filtro registrado, import e delegação em `:63`/`:64` |
| Leitor | `app/templates/chamados/detalhe.html:36` | `espera`, leitura da tupla no badge |
| Leitor | `app/templates/chamados/detalhe_local.html:68` | `espera`, leitura da tupla no badge |
| Leitor | `app/templates/chamados/fio_colaborador.html:188` | `espera`, leitura da tupla no badge |
| Leitor | `app/templates/chamados/partials/painel_chamado_turno.html:18` | `espera`, leitura da tupla no badge |
| Leitor | `app/templates/chamados/partials/modal_fio.html:101` | `espera`, leitura da tupla no badge |
| Leitor | `app/templates/chamados/detalhe.html:132` | Constantes → `espera|first`, opções |
| Leitor | `app/templates/chamados/detalhe_local.html:395` | Constantes → `espera|first`, opções |
| Leitor | `app/templates/chamados/fio_colaborador.html:268` | Constantes → `espera|first`, opções |

Método: grep de símbolos/filtro/opções em Python, HTML e JS, mais AST dos Python com imports apelidados. O carregamento do mapa e a chamada da função estão em `app/chamados/services/verbos.py:141` e `app/chamados/templatetags/chamados_tags.py:64`. Não apareceram outros aliases, leitores em commands/crons/signals ou leituras textuais desses seletores no JS. [não verificado por referência negativa arquivo:linha]

`rotulo_de_linha` responde outra pergunta — palavra da fila administrativa — e não foi contado como fornecedor do texto desses seletores (`app/chamados/services/verbos.py:96`, `app/chamados/services/verbos.py:125`).

**(d) Casa:** **COMPLEMENTO de O137 — ESMERIL-NO-RASTRO**, classe “testemunha recalcula/rótulo não migrou”; a ligação está explicitada no filtro e no teste dessa família (`app/docs/BACKLOG.md:1363`, `app/chamados/templatetags/chamados_tags.py:55`, `app/chamados/tests/test_o137_esmeril_chamado.py:11`). Não é BO de sítio único: são três seletores e dois blocos literais de referência, enumerados em (c).

**(e) Padrão:** **Single Source of Truth / fonte única**, termo existente, aplicado ao vocabulário das ações; VERBO e ESPERA cont

[truncado em 20000 de 22188 caracteres]

## 10/11 19:54 · sessao 2f5571a09655 · - · - · origem=Ronald (tmux)

Confirmo **estado novo: não / muda decisor local: sim**: o explicador decide hoje por `not _fio_vivo(ps)`; a cura faria esse bool consumir C7, que também exige ausência de respostas sem veredito (`app/chamados/services/explicador.py:136`, `app/chamados/services/explicador.py:137`, `app/chamados/models.py:1853`).

Para contar **todas as ocorrências semânticas do bool**, use **7 = 1 escritor + 6 leitores**:

| Papel/forma | Sítio |
|---|---|
| Escritor: atribuição de `pode` | `app/chamados/services/explicador.py:137` |
| Leitor interno: condição do ternário | `app/chamados/services/explicador.py:138` |
| Leitor interno: truthiness no `if` | `app/chamados/services/explicador.py:139` |
| Leitor interno: valor exportado na chave `pode_fechar` | `app/chamados/services/explicador.py:163` |
| Leitor externo: `dict.get`, truthiness | `app/chamados/services/desenlace.py:63` |
| Leitor externo: truthiness em template | `app/templates/chamados/partials/modal_fio.html:434` |
| Leitor externo: truthiness em template | `app/templates/chamados/partials/modal_fio.html:445` |

A contagem **4** seria somente **1 produtor + 3 consumidores externos**, omitindo as leituras locais; prefiro **7** para atender literalmente ao censo (`app/chamados/services/explicador.py:137`, `app/chamados/services/explicador.py:138`, `app/chamados/services/explicador.py:139`, `app/chamados/services/explicador.py:163`).

O transporte não cria decisão: o retorno entra no cache e depois em `ch.explicacao`; o alias `desenlace as _desen` recebe o mesmo dict (`app/chamados/services/fio.py:312`, `app/chamados/services/fio.py:314`, `app/chamados/services/fio.py:386`, `app/chamados/services/fio.py:403`, `app/chamados/services/fio.py:404`). Os caminhos completo e parcial passam listas com explicação ao modal (`app/chamados/views.py:889`, `app/chamados/views.py:927`, `app/chamados/services/fio.py:446`, `app/chamados/services/fio.py:495`).

Grep global de `pode_fechar`, `por_que_nao_fecha`, `.explicacao` e `explicacao=`, excluindo testes/migrations, seguido dos aliases e transportes acima, não revelou outros consumidores do bool; a busca específica em `app/static/**/*.js` retornou zero ocorrências [não verificado por arquivo:linha negativo]. Os 25 sítios de chamadas de carimbo/C7 constituem outro recorte e não devem ser somados ao total **7** de (c) (`app/chamados/services/explicador.py:137`, `app/chamados/models.py:1853`).

## 10/11 19:54 · sessao 235eacb48a70 · - · - · origem=Ronald (tmux)

Confirmado: **não cria estado / sim, troca quem fornece o rótulo**, preservando o juiz da transição (`app/chamados/models.py:123`; `app/chamados/services/acoes_chamado.py:315`).

Os **13 sítios** abrangem o texto visível de espera e dos seletores. Os dois blocos literais contam uma vez cada, como definidor/renderizador (`app/templates/chamados/partials/modal_fio.html:667`; `app/templates/chamados/partials/painel_chamado_turno.html:76`). `rotulo_de_linha` responde outra pergunta — palavra da fila do administrador — e não fornece texto aos seletores (`app/chamados/services/verbos.py:125`).

Grep de JS estático e embutido não encontrou leitura textual desses seletores. Os candidatos examinados leem outros controles: `select[name=escape]` e `data-doc` (`app/templates/chamados/meu_atendimento.html:373`), ou `para-colab-tipo` e `data-pedeDia` (`app/templates/chamados/form_abertura.html:237`). O servidor recebe o `value` pelo nome `novo_status` (`app/chamados/services/acoes_chamado.py:287`). A negativa global, sem artefato gravado da busca, deve carregar **[não verificado]** no relatório.

## 10/11 19:53 · sessao 67ea159aafa1 · - · - · origem=Ronald (tmux)

Correção de **(f)**: para o item amplo, use:

perguntas de lei: 1

**“Quando uma linha BATIDA do acervo sem episódio e uma linha com episódio podem representar o mesmo fato, permitindo que uma retrate a outra?”** O código declara expressamente essa lei aberta em `app/chamados/services/fio.py:1274` e `:1275`; hoje preserva ambas, pelo ramo `sem_episodio_intocada` (`:1294`).

Busquei `sem_episodio_intocada`, `canonica_sem_episodio`, “sem episódio”, “acervo” combinado com “episódio/retrata/identidade” em LEIS, CORTES, CLAUDE e docs. Não encontrei corte que responda essa pergunta; a ausência global de resposta escrita é **[não verificado]**, mas a declaração explícita de pendência está no código (`app/chamados/services/fio.py:1275`).

**Alcance:** depende de dado: quantos dos 486 grupos/848 excedentes contêm essa combinação. O anexo não fornece chave nem pks **[não verificado]**. Essa pergunta não impede medir ou retratar os grupos que o plano já aceita em `app/chamados/services/fio.py:1297`; impede chamar toda perdedora de excedente curável sem classificar as recusas (`:1280`, `:1294`).

O N=0 anterior era válido somente para o subconjunto já aceito pela porta, não para todo o item genérico.

## 10/11 19:53 · sessao 235eacb48a70 · - · - · origem=Ronald (tmux)

## BO-ROTULO-ENCERRADO

**(a) Lei, juiz e porta.** A regra aplicável é “testemunha lê, não recalcula” (`CLAUDE.md:20`), acompanhada de tela e ação consultarem a mesma função (`app/docs/LEIS-UI.md:23`) e textos explicativos saírem do mapa do domínio (`app/docs/LEIS-UI.md:77`). Não encontrei uma lei literal “rótulo único por estado”; a fonte atual distingue duas perguntas: ação do administrador e espera do colaborador (`app/chamados/services/verbos.py:96`).

Para **rótulo de ação**, **sem juiz registrado**: o registro da família chamado enumera transição, premissa, permissão, dia, perguntas e demais autoridades, sem essa pergunta (`app/core/juizes.py:272` até `:293`). Hoje decidem dois blocos literais: `app/templates/chamados/partials/modal_fio.html:667` e `app/templates/chamados/partials/painel_chamado_turno.html:76`. Para o rótulo de espera, decide `rotulo_para_pessoa`, através do filtro `espera` (`app/chamados/templatetags/chamados_tags.py:53`, `app/chamados/services/verbos.py:138`).

A transição permanece com o juiz registrado `motor.decidir` (`app/core/juizes.py:274`, `app/docs/JUIZES.md:37`), acessado por `ChamadoColaborador.transicionar`/`pode_transicionar` (`app/chamados/models.py:119`, `:128`); seu escritor canônico é `_gravar_estado` (`app/chamados/models.py:201`).

**Conferência do relato.** Confere: os dois detalhes mostram “Encerrado” nas opções `resolvido` e `fechado`, porque usam `espera|first` (`app/templates/chamados/detalhe.html:133`, `:134`; `app/templates/chamados/detalhe_local.html:396`, `:397`) e o mapa traduz ambos assim (`app/chamados/services/verbos.py:118`, `:119`). O mesmo defeito aparece também em um terceiro seletor, não mencionado no relato (`app/templates/chamados/fio_colaborador.html:269`, `:270`). Confere que o modal distingue “Encerrar”/“Encerrar e arquivar” (`app/templates/chamados/partials/modal_fio.html:668`, `:669`).

Não confere a sugestão de uma **fonte comum já existente no modal**: os rótulos são literais no template; outro template repete os mesmos literais (`app/templates/chamados/partials/modal_fio.html:667`, `app/templates/chamados/partials/painel_chamado_turno.html:76`). O próprio comentário esclarece que ali se mostra **ação**, mantendo `value` como contrato com a view (`app/templates/chamados/partials/modal_fio.html:660`). O chamado de exemplo não pode ser conferido: depende de dado — contagem de ocorrências e renderização do pk citado em cada rota. [não verificado]

**(b)** Cria estado: **não**, para a cura limitada a rótulos: `resolvido` e `fechado` já são valores do campo existente (`app/chamados/models.py:31`, `:32`, `:52`), e os seletores já enviam esses mesmos valores (`app/templates/chamados/detalhe_local.html:396`, `:397`). Muda decisor de negócio: **não**, nesse escopo; a ação recebe `novo_status` e chama a mesma porta (`app/chamados/services/acoes_chamado.py:287`, `:313`, `:315`), cuja validação delega ao juiz atual (`app/chamados/models.py:123`). Centralizar o vocabulário muda sua fonte de apresentação; não autoriza mudar o julgamento da transição (`app/chamados/catalogo/motor.py:139`).

**(c) Censo relevante ao rótulo; censo global do estado dispensado por (b).** Unidade: função ou bloco de template; opções irmãs contam como um bloco. **13 sítios = 3 definidores + 10 leitores**:

| Tipo | Sítio | Forma |
|---|---|---|
| Definidor | `app/chamados/services/verbos.py:114` | dicionário `ESPERA_COLAB`; valores `resolvido`/`fechado` iguais |
| Definidor/renderizador | `app/templates/chamados/partials/modal_fio.html:667` | opções com texto literal e `value` de estado |
| Definidor/renderizador | `app/templates/chamados/partials/painel_chamado_turno.html:76` | opções com texto literal e `value` de estado |
| Leitor | `app/chamados/services/verbos.py:138` | normalização por truthiness em `:140`; `.get(st, fallback)` em `:141` |
| Leitor | `app/chamados/templatetags/chamados_tags.py:53` | filtro registrado; import local e delegação em `:63`/`:64` |
| Leitor | `app/templates/chamados/detalhe.html:36` | filtro `espera`, leitura de tupla para badge |
| Leitor | `app/templates/chamados/detalhe_local.html:68` | filtro `espera`, leitura de tupla para badge |
| Leitor | `app/templates/chamados/fio_colaborador.html:188` | filtro `espera`, leitura de tupla para badge |
| Leitor | `app/templates/chamados/partials/painel_chamado_turno.html:18` | filtro `espera`, leitura de tupla para badge |
| Leitor | `app/templates/chamados/partials/modal_fio.html:101` | filtro `espera`, leitura de tupla para badge |
| Leitor | `app/templates/chamados/detalhe.html:132` | constantes de estado → `espera|first` |
| Leitor | `app/templates/chamados/detalhe_local.html:395` | constantes de estado → `espera|first` |
| Leitor | `app/templates/chamados/fio_colaborador.html:268` | constantes de estado → `espera|first` |

Método executado: grep global em `.py`, `.html`, `.js` por `rotulo_para_pessoa`, `ESPERA_COLAB`, `|espera` e opções; AST dos Python de produção, com resolução de imports `as`. Encontrados um carregamento Python do mapa (`app/chamados/services/verbos.py:141`) e uma chamada da função (`app/chamados/templatetags/chamados_tags.py:64`). Nenhum outro alias/chamador foi encontrado; commands, `config/crons.py`, signals e JS de static não produziram ocorrências desses símbolos. Como prova negativa de execução sem artefato gravado, o resultado das buscas fica [não verificado] para quem só disponha deste relatório.

**(d)** **COMPLEMENTO de O137 — ESMERIL-NO-RASTRO**, especificamente a classe testemunha/rótulo que não migrou, já descrita nessa casa (`app/docs/BACKLOG.md:1363`). A ligação é explícita no teste da família chamado: a classe B centraliza a pílula de espera em `rotulo_para_pessoa` (`app/chamados/tests/test_o137_esmeril_chamado.py:11`), e o próprio filtro identifica O137 (`app/chamados/templatetags/chamados_tags.py:55`). Não há prova de BO de sítio único: são três seletores afetados e dois modelos literais de ação (`app/templates/chamados/detalhe.html:132`, `app/templates/chamados/detalhe_local.html:395`, `app/templates/chamados/fio_colaborador.html:268`, `app/templates/chamados/partials/modal_fio.html:667`, `app/templates/chamados/partials/painel_chamado_turno.html:76`). Esse encaixe não afirma que a condição atual de execução da O137 esteja satisfeita; a casa exige família fechada (`app/docs/BACKLOG.md:1363`).

**(e)** **Single Source of Truth / Aggregate Root — fonte única**, termo existente (`app/docs/GLOSSARIO.md:11`), aplicado ao vocabulário da ação, mantendo separado o vocabulário de espera já declarado (`app/chamados/services/verbos.py:96`).

**(f)** perguntas de lei: 0

No escopo limitado de apresentar os verbos já existentes e preservar `value`, a distinção ação/espera já está documentada (`app/templates/chamados/partials/modal_fio.html:660`, `app/chamados/services/verbos.py:96`); a obrigação de fonte única já existe (`CLAUDE.md:20`). Se se pretender registrar uma nova autoridade de domínio, aplica-se a regra de declaração prévia, assunto que este censo não autoriza construir (`CLAUDE.md:311`).

**Linha para tabela consolidada:**  
`BO-ROTULO-ENCERRADO | CLAUDE:20; LEIS-UI L2/L7; sem juiz registrado do rótulo; motor.decidir/transicionar/_gravar_estado | não/não | 13 (perímetro do rótulo; estado global n/a) | COMPLEMENTO de O137 (BACKLOG:1363) | Single Source of Truth (GLOSSARIO:11) | 0`

## 10/11 19:53 · sessao 67ea159aafa1 · - · - · origem=Ronald (tmux)

## PERGUNTA-DUPLICATA-1110

**(a)** Aplicam-se L-001 (origem), L-002 (mesma autoridade), L-003 (medir função real), L-007 (escritor e trilha), em `app/docs/LEIS.md:59`, `:60`, `:61`, `:65`, e idempotência estrutural de `CLAUDE.md:175`.

**Sem juiz registrado para “qual pergunta é duplicata / qual canônica fica”**: C8 pergunta pelo **chamado**, em `app/core/juizes.py:114`, e aponta para `ChamadoColaborador.abrir`, em `app/core/juizes.py:280`; a família chamado registrada termina em `app/core/juizes.py:294`, sem a eleição de pergunta. O lookup FIO aponta para `_disputa_do_chamado`, outra pergunta (`app/docs/JUIZES.md:40`).

Quem decide hoje:

- Identidade MARCO: colaborador/dia/motivo/instância; BATIDA: episódio/motivo, em `app/chamados/services/fio.py:1372`.
- Canônica: primeira por pk com resposta não vazia (`(resposta_colab or '').strip()`); senão menor pk, em `app/chamados/services/fio.py:1225`.
- Retratação permitida: `plano_de_retratacao`, em `app/chamados/services/fio.py:1259`, preserva mérito com resposta, episódios distintos e acervo sem episódio (`:1287`, `:1290`, `:1292`, `:1294`).
- Porta: `retratar_para_canonica` (`app/chamados/services/fio.py:1319`) → `retratar_duplicata` (`:1345`) → `carimbar_identidade` (`app/chamados/models.py:3168`, save `:3190`). Nada se apaga; a referência à canônica entra em `recusa_motivo` (`app/chamados/services/fio.py:1359`).

**(b)** Para a cura mínima compatível com o relato — medir corretamente e retratar excedentes pela porta existente — **cria estado: não; muda decisor: não**. O valor `duplicata` já existe (`app/chamados/models.py:2495`), a porta já grava via/carimbo/autor/motivo (`:3182`, `:3186`, `:3190`), e a eleição já é compartilhada inclusive pelo nascimento, via import apelidado `_canonica` (`:2905`, `:2921`). A presença efetiva desse estado no banco de produção permanece **[não verificado]**.

Construir um **novo fato próprio de identidade** seria outra obra: `FIO-E0-V4-FATO-A06`, posterior à E4d, registrada em `app/docs/BACKLOG.md:1867`; não se deve presumir essa ampliação de escopo.

**(c)** Como (b) é não/não, o censo global dos campos não é exigido pela condição do pedido. Para não apresentar “zero leitores”, contei o **perímetro das cinco funções de dedup acima** por AST em todos os `.py` da cópia, excluindo testes/migrations; resolvi `ImportFrom.asname`, incluindo `_canonica`. Resultado: **23 chamadas: 19 leituras de decisão e 4 delegações de escrita**, não 23 leitores/escritores globais dos campos. [nao verificado]

| Arquivo | Sítios e forma |
|---|---|
| `app/chamados/management/commands/backfill_decisoes_pergunta.py` | `:276` eleição; `:290` plano, categorias por truthiness em `:295`; `:331` delegação de escrita; `:393` eleição condicionada a `len>=2` em `:392`; `:396` leitura `['retrata']`; `:485` grupos; `:506` eleição/comparação com menor pk em `:507`; `:510` plano/contagem por `len` em `:512`. |
| `app/chamados/management/commands/dedup_perguntas_fato.py` | `:58` grupos com recorte; `:76` eleição; `:81` plano, leitura `['retrata']` em `:88` e truthiness em `:89`. |
| `app/chamados/models.py` | `:2921` eleição por alias `_canonica`, truthiness da lista e depois `is not None` em `:2922`. |
| `app/chamados/services/declaracao_texto.py` | `:253` eleição, comparação disputa e `_reusavel` em `:254`; `:275` eleição depois de truthiness da lista em `:271`. |
| `app/chamados/services/fio.py` | `:921` delegação de escrita sob truthiness em `:920`; `:956` eleição/comparação pk em `:957`; `:967` plano/`any` categorias em `:968`; `:1340` plano; `:1341` delegação de escrita em compreensão com truthiness do retorno; `:1832` eleição condicionada a sementes em `:1833`; `:2155` eleição/comparação pk em `:2165`; `:2158` plano/truthiness das recusas em `:2166`; `:2180` delegação de escrita/leitura `['retratadas']`. |

O escritor físico subsequente é `app/chamados/models.py:3190`, chamado de `app/chamados/services/fio.py:1359`; fica **fora das 23 chamadas**, cujo perímetro são os cinco símbolos. O grep desses símbolos em HTML/JS/signals não encontrou ocorrências; isso não prova ausência de leitores dos campos por outras formas **[não verificado]**. `app/config/crons.py:783` menciona o backfill somente na declaração de papel de ato único (`:780`), não como chamada executável.

Leituras relevantes para medir eventual inflação: C6 compara a via com `VIAS_QUE_VENCEM_A_RESPOSTA` e exige carimbo não nulo (`app/chamados/juizes.py:1336`); o queryset usa `via_resolucao__in` + `materializada_em__isnull=False` (`app/chamados/models.py:3352`) e os exclui em `:3359`; o app filtra `q_vivas` (`app/chamados/services/pergunta_no_app.py:50`), confirma C6 em `:58`, e separa pendentes por `respondida_em is None` e `fechada_em is None` em `:71`.

**(d)** **COMPLEMENTO de FIO-E0-V4 E1**, identidade e backfill do acervo, casa `app/docs/BACKLOG.md:1848`; articulado à migração dos escritores E4 (`:1863`). Há ainda resíduo específico de duplicatas sobre episódio retratado em `:786`; não há dado fornecido que permita dizer que os 486 grupos incluem esse resíduo **[não verificado]**. Não é BO simples: as 23 chamadas acima atravessam modelo, serviços e dois commands.

**(e)** Padrão direto: **idempotência pela chave do fato**, termo em `app/docs/GLOSSARIO.md:29`; fonte única/Aggregate Root consta em `:11`, e escritor único em `:12`. A identidade já é compartilhada pelo nascimento e pelo dedup (`app/chamados/models.py:2905`, `app/chamados/services/fio.py:1372`).

**Conferência do relato:** “486 grupos / 848 excedentes em disputa aberta” é dado fornecido, sem chave de agrupamento nem pks; sua equivalência ao universo canônico é **[não verificado]**. “Disputa aberta” não define duplicidade: `do_fato` usa colaborador/família fora das vias terminais (`app/chamados/models.py:2717`, `:2724`), enquanto conversa nasce sempre, sem dedup (`:2794`). O agrupador real exclui conversa e vias fora do fato (`app/chamados/services/fio.py:1431`), aplica chaves por família (`:1945`) e exige grupo com membro no horizonte (`:1949`); nem toda perdedora é retratável (`:1290`).

**Depende de dado:** contagem dos 486 grupos reclassificados pelo universo real; excedentes retratáveis versus mérito/episódios unidos/acervo sem episódio; distribuição por idade, horizonte e competência; quantos excedentes passam por C6 e efetivamente entram nos contadores/app. Os 848, isoladamente, não demonstram inflação **[não verificado]**.

perguntas de lei: 0

Para o escopo mínimo, a eleição e as recusas já têm regra explícita (`app/chamados/services/fio.py:1225`, `:1259`); registrar a autoridade existente é pendência de registro, não uma nova decisão de mérito. A união de acervo sem episódio com episódio está expressamente preservada e anotada como lei aberta (`:1274`), mas não foi demonstrado que o dado deste item exige resolvê-la **[não verificado]**.

**Linha de tabela sugerida:** `PERGUNTA-DUPLICATA-1110 | L-001/002/003/007; sem juiz registrado de identidade de pergunta; fio.retratar_para_canonica | não/não (escopo mínimo) | 23 chamadas do perímetro (19 leitura/4 delegação), não censo global | COMPLEMENTO FIO-E0-V4 E1/E4 | idempotência pela chave do fato | 0`.

## 10/11 19:53 · sessao 2f5571a09655 · - · - · origem=Ronald (tmux)

Para **DISPUTA-ZUMBI-1110**, a cura mínima é completar a costura dos carimbos com o juiz e a porta existentes; isso não exige estado novo nem juiz novo (`app/chamados/models.py:1848`, `app/chamados/models.py:1932`).

**(a) Lei/juiz/porta.** L-002 obriga testemunhas a lerem a mesma autoridade; L-007 exige escritor canônico; CLAUDE 4b registra um juiz por pergunta e um escritor por entidade (`app/docs/LEIS.md:60`, `app/docs/LEIS.md:65`, `CLAUDE.md:175`). O corte C7 define `DisputaSupervisao.pode_fechar` como juiz único; registro vivo `_C7`, confirmado pelo lookup (`app/docs/CORTES.md:272`, `app/core/juizes.py:279`, `app/docs/JUIZES.md:57`). O juiz responde **nenhuma resposta sem veredito E nenhum fio vivo** (`app/chamados/models.py:1853`). A porta `DisputaSupervisao.fechar` grava fechamento, autor, resolução e motivo conjuntamente (`app/chamados/models.py:1932`, `app/chamados/models.py:1936`). Os carimbos existentes saem de `PerguntaDisputa.carimbar`/`carimbar_identidade` (`app/chamados/models.py:3103`, `app/chamados/models.py:3168`).

**(b) Estado novo: não; muda quem decide: não**, limitado a completar chamadas ao C7/`fechar`, preservando esses contratos. Os campos e valores de fechamento já existem e a porta já os escreve; C7 já é registrado (`app/chamados/models.py:1757`, `app/chamados/models.py:1780`, `app/chamados/models.py:1932`, `app/core/juizes.py:279`). Confirmar quais combinações existem em produção depende de dado: contagem de disputas por `fechada_em × motivo_fechamento × via_resolucao das perguntas` [não verificado].

**(c) Censo global de estado não acionado por (b).** Para demonstrar a costura, contei nós `ast.Call` cujo método é `carimbar`, `carimbar_identidade` ou `pode_fechar`, em `app/**/*.py`, excluindo diretórios `tests` e `migrations`; esse recorte conta **13 chamadas de carimbo em 12 funções + 12 chamadas ao C7**, sem depender do nome da variável receptora (`app/chamados/models.py:3287`, `app/chamados/services/disputa_emissao.py:1863`, `app/chamados/services/disputa_emissao.py:1937`, referências completas abaixo).

O rol de carimbadores é:

| Forma | Sítios |
|---|---|
| `.carimbar(...)`, 8 chamadas | `app/chamados/models.py:3287`; `app/chamados/reconciliador.py:405`; `app/chamados/reconciliador.py:595`; `app/chamados/services/disputa_emissao.py:1863`; `app/chamados/services/disputa_emissao.py:1937`; `app/chamados/services/materializacao.py:561`; `app/chamados/services/materializacao.py:1277`; `app/chamados/services/materializacao.py:1431` |
| `.carimbar_identidade(...)`, 5 chamadas | `app/chamados/models.py:2061`; `app/chamados/models.py:3064`; `app/chamados/services/fio.py:1359`; `app/chamados/services/materializacao.py:924`; `app/chamados/services/materializacao.py:1099` |

O rol de leitores diretos de C7 é:

| Forma | Sítios |
|---|---|
| `fechada_em is None and ...pode_fechar()` | `app/chamados/reconciliador.py:411`; `app/chamados/reconciliador.py:611`; `app/chamados/services/becos.py:56`; `app/chamados/services/disputa_emissao.py:1963`; `app/chamados/services/materializacao.py:585`; `app/chamados/services/materializacao.py:1316` |
| `elif ...pode_fechar()` | `app/chamados/reconciliador.py:1170` |
| ternário por bool | `app/chamados/services/becos.py:33` |
| `if not ...pode_fechar()` | `app/chamados/services/competencia_trancada.py:153`; `app/chamados/services/competencia_trancada.py:202` |
| atribuição e teste posterior | `app/chamados/services/disputa_emissao.py:1278`; `app/chamados/services/disputa_emissao.py:1282` |
| `if disp and disp.pode_fechar()` | `app/ponto/services/entrada_sincronizada.py:58` |

O explicador tem **1 produtor de bool e 3 leituras diretas**: importa `fio_vivo as _fio_vivo` e calcula apenas `not _fio_vivo(ps)`; os dois templates usam truthiness e `desenlace` usa truthiness de `dict.get` (`app/chamados/services/explicador.py:136`, `app/chamados/services/explicador.py:137`, `app/templates/chamados/partials/modal_fio.html:434`, `app/templates/chamados/partials/modal_fio.html:445`, `app/chamados/services/desenlace.py:63`). O transporte/cache passa por `fio.py`, sem alterar esse bool (`app/chamados/services/fio.py:312`, `app/chamados/services/fio.py:314`). **Total deste recorte: 29 sítios = 13 + 12 + 1 + 3**; não apresentar como censo global de `fechada_em` ou `materializada_em` (referências dos quatro grupos acima).

**Conferência do relato.**

- `lavrar_sem_turno_aberto` já consulta C7 e chama `fechar`: confirmado (`app/chamados/reconciliador.py:411`, `app/chamados/reconciliador.py:412`).
- “Explicador sem a trava do C7”: confirmado especificamente quanto a `respostas_sem_veredito`; ele só nega `fio_vivo`, enquanto C7 exige ambos (`app/chamados/services/explicador.py:137`, `app/chamados/models.py:1853`).
- “Fechamento só por beco”: **não confere como afirmação global**; há fechamento após carimbo fora desse recorte, por exemplo reconciliação de órfãs e materialização (`app/chamados/reconciliador.py:1170`, `app/chamados/services/materializacao.py:1316`).
- O evento genérico após `PerguntaDisputa.save` chama a porta de becos; ela exige pai terminal e ausência de perguntas visíveis antes de consultar C7 (`app/chamados/signals.py:227`, `app/chamados/signals.py:238`, `app/chamados/services/becos.py:29`, `app/chamados/services/becos.py:31`, `app/chamados/services/becos.py:33`). O cron está agendado com `--alarme`; `--apply` continua opcional no comando (`app/config/crons.py:663`, `app/chamados/management/commands/fechar_becos_disputa.py:23`, `app/chamados/management/commands/fechar_becos_disputa.py:30`).
- “7 carimbadores sem C7”: **não estabelecido como sete caminhos sem fechamento**. Grep direto de cada corpo não basta: `encerrar_fantasma` tem cauda C7 no reconciliador; `_materializar_uma` tem caudas compartilhadas; `remover_motivo` chama diretamente a porta de fechamento (`app/chamados/reconciliador.py:1147`, `app/chamados/reconciliador.py:1170`, `app/chamados/services/materializacao.py:1378`, `app/chamados/services/passivo_validado.py:405`, `app/chamados/models.py:2072`).
- Os dez casos, data do carimbo e eventual recusa dependem de dado: contagem por `via_resolucao × materializada_em antes/depois de 22/10 × estado do pai × respostas_sem_veredito × C7 atual`, e trilha de tentativas/recusas [não verificado]. A autoria histórica do commit citado no anexo não foi verificada [não verificado]. [nao verificado]

**(d) Casa.** **COMPLEMENTO de C7-DISPUTA-FECHA-POR-UM-JUIZ**, cuja linha manda migrar regras espalhadas e usar C6/C7; interseção com série E4 para os escritores e E3 para apresentação (`app/docs/BACKLOG.md:1468`, `app/docs/BACKLOG.md:1863`, `app/docs/BACKLOG.md:1862`). Não cabe provar “um sítio só”: o recorte acima tem múltiplas portas e leitores (referências das tabelas).

**(e) Padrão.** **Single Writer Principle / juiz único**, termo literal do glossário, realizado ao fazer os chamadores e o explicador consumirem C7/porta existentes (`app/docs/GLOSSARIO.md:12`, `app/core/juizes.py:279`). Aggregate Root também existe no glossário, mas o sítio que a tabela associa a esse termo é `CelulaDia`; o defeito comprovado aqui é de consumo incompleto do juiz de fechamento (`app/docs/GLOSSARIO.md:11`, `app/chamados/services/explicador.py:137`, `app/chamados/models.py:1853`).

perguntas de lei: 0

O critério de fechamento e sua guarda de resposta sem veredito já estão escritos; falta de contagem histórica é pergunta de dado (`app/docs/CORTES.md:272`, `app/chamados/models.py:1853`).

## 10/11 19:50 · sessao 32814039580f · MAPA-FILA-BO · censo · origem=Code

São **quatro IDs únicos**; o noturno aparece duas vezes, inclusive na árvore (`app/docs/BACKLOG.md:1814`, `app/docs/BACKLOG.md:1874`). A identidade Git do HEAD informado é **[nao verificado]**: esta cópia não contém `.git`. Não consultei banco nem logs **[nao verificado por arquivo:linha]**.

Nos inventários abaixo, cada linha indica o arquivo; os números após cada função são linhas desse mesmo arquivo. A contagem usa funções ou templates distintos, deduplicando múltiplas leituras na mesma função. Testes não entram como consumidores de produção; migrations executáveis entram separadamente. O levantamento foi por AST, grep e leitura dos sítios **[nao verificado por arquivo:linha quanto à execução do levantamento]**. [nao verificado]

## BUG-PLANTIO-NOTURNO-MAIS-1

**(a) Lei, juiz e porta.** Aplicam-se a fonte única e o escritor único — L-002 e L-007 (`app/docs/LEIS.md:60`, `app/docs/LEIS.md:65`) —, a célula como fonte do previsto (`CLAUDE.md:145`) e a escrita de batida pela porta L-030 (`app/docs/LEIS.md:78`).

O juiz registrado do dia da batida é `turnos_do_colab` (`app/core/juizes.py:208`); a consulta também aponta a geometria e os marcos encadeados (`app/docs/JUIZES.md:20`, `app/docs/JUIZES.md:24`). `DiaDoTurno` apenas nomeia a resposta dessa autoridade: é `NewType`, não campo persistido nem juiz novo (`app/core/dia_do_turno.py:3`, `app/core/dia_do_turno.py:13`). Os adaptadores existentes são `dia_do_turno_da_batida` e `dia_do_turno_da_celula` (`app/ponto/turnos.py:2927`, `app/ponto/turnos.py:2939`).

As portas são:

- Nascimento: `PerguntaDisputa.abrir` (`app/chamados/models.py:2776`).
- Re-chaveamento do passivo: `rechavear_data_evento` (`app/chamados/services/fio.py:979`).
- Atribuição única de `data_evento`: `gravar_dia_do_fato` (`app/chamados/services/fio.py:800`).
- Plantio de batida: chamada de `registrar_batida` pelo materializador (`app/chamados/services/materializacao.py:926`).

**Conferência do relato.** A afirmação de que o emissor ainda grava necessariamente o dia da cobrança **não descreve o código atual**: o nascimento exige dia explícito, e o ajuste usa esse dia ou o seguinte; sem marcos, devolve o timestamp recebido (`app/chamados/models.py:2783`, `app/chamados/models.py:2787`, `app/chamados/juizes.py:324`, `app/chamados/juizes.py:329`). A conversa tem porta separada e conserva dia civil; portanto também seria incorreto afirmar que **todo** `data_evento` significa dia do turno (`app/chamados/models.py:2796`, `app/chamados/models.py:2801`).

Já existem construtor, porta de re-chaveamento e command de migração nesta cópia; “ainda sem construtor” não confere para este ID (`app/chamados/models.py:2776`, `app/chamados/services/fio.py:979`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:179`). Aplicação desse passivo em produção e incidência atual de D+2 são **[nao verificado]**.

**(b) Cria estado: sim, no sentido amplo de novo valor/composição por linha migrável.** Não acrescenta coluna: regrava `data_evento`, troca `celula_id`, relavra e persiste os três campos da lavra (`app/chamados/services/fio.py:1037`, `app/chamados/services/fio.py:1038`, `app/chamados/services/fio.py:1042`, `app/chamados/services/fio.py:1043`). A célula de destino precisa existir; essa porta não cria uma célula nova (`app/chamados/services/fio.py:1034`, `app/chamados/services/fio.py:1035`). A existência ou ausência dessas combinações no banco real é **[nao verificado]**.

**Muda decisor: não.** O novo dia continua vindo do juiz do turno; o escritor apenas recebe e atribui a resposta (`app/chamados/services/fio.py:794`, `app/chamados/services/fio.py:984`, `app/ponto/turnos.py:2935`).

**(c) Censo do estado tocado.** O recorte é `PerguntaDisputa.{data_evento, celula, veredito_lavrado, julgado_em, janela_expira_em}`, os campos efetivamente salvos pelo re-chaveamento (`app/chamados/services/fio.py:1043`).

Foram inventariadas **155 funções distintas** na união dos três levantamentos: 117 de dia, 52 de FK e 21 de lavra, com sobreposição removida. Acrescentam-se duas funções executáveis da migration, dois consumidores de tupla/evento e três templates: **162 sítios de aplicação**. Há ainda três mecanismos declarados do banco: **165 sítios na contagem ampliada**, mais quatro encaminhadores discriminados abaixo. A contagem é de acessos, escritas e portas estáticas; **não é contagem de todos os chamadores transitivos dessas funções** **[nao verificado quanto a acessos dinâmicos não identificados]**.

**Dia — 117 funções.** “Porta” abaixo significa fornecimento do valor para uma escrita canônica; não significa atribuição direta adicional.

| Arquivo | Funções, linhas e forma |
|---|---|
| `app/api/views.py:1183` | `api_chamados_v2` :1183–1184: truthiness, ISO e rótulo; `api_disputa_responder` :2655: mensagem. |
| `app/api/views_mensageria.py:568` | `_item_validar`: truthiness e ISO para JSON. |
| `app/chamados/canal.py:65` | `perguntas_mudas_com_resposta`: filtro `isnull=False` :65, ordenação :67, chave :72, payload :79. |
| `app/chamados/juizes.py:80` | `_dia_da_disputa` :80–81: truthiness/lista; `dias_do_fato` :214: truthiness; `_plantio_na_hora_do_slot` :271: fallback `or`; `_dia_de_turno_da_pergunta` :336: `getattr`, conversa→`None`; `_celulas_da_pergunta` :637: `getattr`; `_chave_memo_pv` :831: chave de memo; `_marco_esperado_da_grade` :1725: leitura; `_dias_das_perguntas_vivas` :2264–2268: filtro `isnull=False` e lista. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:63` | `_candidatos_i8` :63/:73/:74: truthiness e filtros de data; `_contraprova_i8bx` :148: intervalo `<=`; `_cobre_ausencia_i21` :160/:162: truthiness e argumento do juiz; `_piso_f3_antes_do_gerador` :282: `<`; `_contraprova_f3g` :298: filtro `lte` e `.exists()`; `contraprova_piso` :325/:326/:348/:361/:373: chaves, truthiness, filtro de vínculo e `.exists()`, intervalo; `handle` :892/:914/:926/:930/:957/:1665/:1708/:1778/:1779/:1804: filtros `isnull`, intervalos, `.exists()`, truthiness, igualdade ORM e argumento de juiz. |
| `app/chamados/management/commands/backfill_decisoes_pergunta.py:60` | `linhas_a_criar` :60: `.only`; `_escoar` :69: chave; `admin_decidiu_sem_ato` :104: filtro `isnull`; `competencia_de` :115/:118/:120: `is None`, chave e argumento; `aplicar` :282–283: `is None`/`is not None`; `_grupos_fora` :371: `values_list`. |
| `app/chamados/management/commands/backfill_veredito_lavrado.py:111` | `handle`: argumento de `expira_em`. |
| `app/chamados/management/commands/dedup_perguntas_fato.py:85` | `handle` :85–86: `is not None`/`is None`. |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py:74` | `handle`: mensagem. |
| `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:97` | `_fonte_viva` :97/:100/:102/:109/:113/:118: leitura e comparações; `classificar` :144/:148/:158/:163: janela, payload, desigualdade e igualdade; `handle` :205/:209/:211: filtros `isnull`, `gte`, `lte`; `_veto_do_grupo` :309: `values_list`; `_escrever_reversao` :354: JSON; `_migrar` :392: `values_list`; `_descarimbados` :442/:450: `Q` por igualdade e chave; `_reverter` :481/:488: JSON e argumento de veto. |
| `app/chamados/management/commands/reabrir_orfas_enterradas.py:68` | `handle` :68/:86/:91/:102/:117/:144/:145: filtro `isnull=False`, janela, mensagens e competência. |
| `app/chamados/models.py:364` | `_vivas_entre` :364: argumento e resultado `is not False`; `_plano_pergunta_viva` :390–391: argumento/truthiness; `q_dos_fatos` :2753: `Q` igualdade; `abrir` :2789 e `abrir_conversa` :2801: portas de escrita; `_nascer` :2940/:2956/:2962: desigualdade, mensagem e captura; `linhas_do_evento` :3228: `values_list`. |
| `app/chamados/reconciliador.py:370` | `lavrar_sem_turno_aberto` :370: leitura; `_dia_fora_do_alcance_c` :736: `getattr`/`is None`; `reemitir_chamado_do_dia` :954: porta de escrita; `_orfas_abertas` :1050: filtro `isnull`; `veredito_orfa` :1082/:1088/:1090: `is None`, `>=`, argumento de juiz; `encerrar_orfas_pela_lampada` :1152: mensagem; `semear_eventos_celula` :1303/:1305/:1307: conjuntos e igualdade ORM; `reatar_pergunta_orfa` :1557/:1567/:1642/:1655: argumento, truthiness/ISO e portas de escrita. |
| `app/chamados/services/aviso_pendencia.py:16` | `aviso_texto_livre`: truthiness/`strftime`. |
| `app/chamados/services/cobranca.py:83` | `checar_cobranca`: truthiness e conjunto ordenado. |
| `app/chamados/services/competencia_trancada.py:107` | `medir` :107 e `reabrir` :252: filtros `gte/lte`; `reabrir` :256 também interpola mensagem. |
| `app/chamados/services/declaracao_texto.py:370` | `declarar_hora_colab` :370/:377: portas de costura/nascimento; `insumos_em_regularizacao` :590: chave; `aplicar_declaracao` :667: porta de nascimento. |
| `app/chamados/services/disputa_emissao.py:399` | `gerar_perguntas_disputa` :399/:418/:451: igualdade ORM, `exclude` e nascimento; `_criar_pergunta_se_couber` :844: nascimento; `criar_disputa_a_partir_de_chamados_orfaos` :1074: fallback `or`; `_ja_perguntou` :1415: igualdade ORM; `responder_pergunta_disputa_core` :1498/:1531: mensagem e argumento; `_resolver_perguntas_por_chamado_resolvido` :1863/:1935: argumento da autoridade de via. |
| `app/chamados/services/explicador.py:70` | `explicar_pergunta` :70: truthiness/`strftime`; `por_que_nao_fecha` :122/:126: ordenação/truthiness. |
| `app/chamados/services/fabrica_da_celula.py:148` | `motivos_a_perguntar` :148 e `perguntar` :170: igualdade ORM. |
| `app/chamados/services/fio.py:87` | `data_do_fato_de` :87–88: leitura, truthiness/desigualdade; `_costura_nao_mata_o_fio` :776: competência; `gravar_dia_do_fato` :800: atribuição; `reapontar_resolvedora` :867/:934: truthiness, comparação e porta de escrita; `_colisao_ao_datar` :953: assert igualdade; `rechavear_data_evento` :1013/:1037/:1043/:1047/:1048: captura, atribuição pela porta, persistência e trilha; `rechavear_no_reuso` :1068/:1072: veto/mensagem; `reuso_sem_rechave` :1109: igualdade; `violacao_de_saida` :1133/:1136: igualdade/mensagem; `retratar_duplicata` :1362: mensagem; `q_do_horizonte` :1412: `Q(gte)`; `q_do_horizonte_ou_sem_dia` :1419: `Q(isnull)`; `_vizinhos_do_fato` :1447: argumento condicional; `chave_do_dia` :1460: tupla; `_dia_do_fato` :1495/:1497: `is not None` e construtor do tipo; `dia_do_turno_da_linha` :1630: `getattr`/fallback; `universo_do_fato` :1800: igualdade de tupla; `fecho_cruza_dias` :1854: `values_list`; `grupos_de_colisao` :1945/:1946/:1948/:1955/:1958/:1970/:1985: campos dinâmicos, `exclude`, values/order, `OuterRef`, filtro e tupla; `fora_do_indice` :2005: `isnull`; `batida_sem_origem_no_horizonte` :2024: `values_list`. |
| `app/chamados/services/historico.py:108` | `_eventos_dos_chamados`: truthiness/`strftime`. |
| `app/chamados/services/limbo_folgas.py:203` | `filhos_no_defeito`: igualdade ORM. |
| `app/chamados/services/materializacao.py:279` | `_tipo_pelo_marco` :279/:282: truthiness/argumento da grade; `_ts_tipo_do_plantio` :344–345: truthiness/leitura; `_declarar_ausencia_de_pergunta` :473–474: truthiness/leitura; `_materializar_ausencia_de_resposta` :517/:533/:573: leitura, igualdade ORM e payload; `perguntas_absorvidas_por` :624: igualdade ORM; `_materializar_uma` :1265: argumento da autoridade de via. |
| `app/chamados/services/passivo_validado.py:113` | `_dia_do_plantio` :113: fallback `or`; `_plantar_o_dia` :519–520: truthiness/conjunto. |
| `app/chamados/services/pergunta_no_app.py:115` | `retencao`: igualdade e `any`. |
| `app/chamados/services/relavra_via.py:103` | `via_de_hoje` :103: argumento da autoridade; `relavrar_dia` :154: igualdade ORM. |
| `app/chamados/services/tarefa_do_dia.py:233` | `_tarefa_pura` :233: igualdade; `CargaDaTarefa.__init__` :326/:329: filtros `gte/lte` e chave. |
| `app/chamados/services/validacao.py:269` | `_aceitar_a_folga` :269: leitura; `validar_pergunta` :446/:497/:510: recálculo, argumento e mensagem; `conferir_data_do_fato` :671: trilha JSON. |
| `app/chamados/services/vigia_fio.py:816` | `carimbos_sem_lastro_por_dono`: argumento da competência/classificação. |
| `app/chamados/veredito_lavrado.py:273` | `lavrar`: argumento da expiração. |
| `app/chamados/views_cobrar.py:176` | `validar_inline` :176/:185: ordenação ORM e truthiness do payload do canal. |
| `app/colaboradores/services/calendario.py:473` | `contexto_calendario` :473/:477: filtro `range` e chave; modelo por alias `_PD`. |
| `app/folha/previa.py:65` | `montar_previa` :65/:67: filtro `range` e chave. |
| `app/ponto/management/commands/mapa_divergencia.py:272` | `handle` :272–273: filtros `gte/lte`, `.only` e conjunto. |
| `app/ponto/management/commands/vigia_plantio_overnight.py:75` | `batidas_fora_do_turno_da_pergunta` :75/:86/:90: filtro relacionado `pergunta_origem__data_evento__isnull=False`, janela e julgamento. |
| `app/ponto/services/dia_do_colab.py:65` | `dia_do_colab`: igualdade ORM. |
| `app/ponto/services/veredito_celula.py:180` | `fechar_o_fio_do_dia`: igualdade ORM. |

**FK — 52 funções**, incluindo carregamento ORM explícito. Homônimos de `ChamadoColaborador.celula` não foram contados como FK da pergunta; a relação examinada está declarada em `app/chamados/models.py:2414`.

| Arquivo | Funções, linhas e forma |
|---|---|
| `app/chamados/models.py:311` | `_gravar_estado` :311 e `_ancoradas` :346: SELECT relacionado; `_universo_da_ressuscitacao` :1501: leitura/atribuição numa cópia em memória; `do_fato` :2725/:2727: SELECT; `_nascer` :2962/:2965/:2966/:3014: tupla, desigualdade, UPDATE/INSERT; `linhas_do_evento` :3228: `values_list`; `q_vivas` :3355–3358: `Q(isnull)`, `veredito__in` e negações. |
| `app/chamados/juizes.py:82` | `_dia_da_disputa` :82–83: truthiness/FK.data; `dias_do_fato` :212/:215: prefetch/truthiness; `_celulas_da_pergunta` :636–638: `getattr`, `is None`, identidade; `_chave_memo_pv` :818/:832: `getattr`/tupla; `fato_do_marco_no_espelho` :906–907: `getattr`, `is None`/ata; `_resposta_concorda_com_a_luz` :1293: FK→DNA; `_pergunta_viva_calc` :1338–1339: `getattr`, `is not None`, veredito/admin; `alimentar_portas` :1502/:1505: filtro ID `__in`, SELECT e chave; `_dias_das_perguntas_vivas` :2266: SELECT. |
| `app/chamados/reconciliador.py:631` | `qs_perguntas_absolviveis` :631/:637/:639/:641: `isnull=False`, `exclude` vazio/`__in`; `perguntas_absolvidas_pela_celula` :651 e `_orfas_abertas` :1051: SELECT; `encerrar_orfas_pela_lampada` :1155: auditoria; `semear_eventos_celula` :1306: truthiness do ID, FK ou fallback. |
| `app/chamados/relatorios_disputa.py:43` | `coletar`: prefetch `perguntas__celula`. |
| `app/colaboradores/services/calendario.py:474` | `contexto_calendario`: SELECT pelo alias `_PD`. |
| `app/chamados/services/vigia_carimbo.py:100` | `classificar` :100/:103: filtro ID `__in` e SELECT; `censo` :131: ID na amostra. |
| `app/chamados/services/furo_sem_canal.py:81` | `medir`: filtro ID `__in` e `values_list`. |
| `app/chamados/services/vigia_fio.py:399` | `censo` :399/:400/:466/:486: filtro ID `__in`, SELECT e chaves; `censo_alcance` :614–616: filtro, SELECT e chave; `viva_em_disputa_fechada` :749 e `carimbos_sem_lastro_por_dono` :807: SELECT. |
| `app/chamados/services/pergunta_no_app.py:50` | `perguntas_do_app`: SELECT. |
| `app/chamados/services/limbo_folgas.py:204` | `filhos_no_defeito` :204 e `reatar_perguntas_ao_limbo` :239: SELECT. |
| `app/chamados/services/declaracao_texto.py:588` | `insumos_em_regularizacao`: SELECT. |
| `app/chamados/services/disputa_emissao.py:1422` | `_ja_perguntou` :1422 e `_resolver_perguntas_por_chamado_resolvido` :1827: SELECT. |
| `app/chamados/services/competencia_trancada.py:108` | `medir`: SELECT. |
| `app/chamados/services/fio.py:226` | `montar_chamados_fio` :226 e `processar_post_fio` :597: SELECT; `reapontar_resolvedora` :934: igualdade/`is not None`; `rechavear_data_evento` :1013/:1035/:1038/:1043/:1048: leitura, truthiness/`is None`, atribuição, save e auditoria. |
| `app/chamados/services/relavra_via.py:237` | `desfazer_carimbo_sem_lastro`: SELECT. |
| `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:355` | `_escrever_reversao` :355: serialização; `_descarimbados` :448: SELECT; `_reverter` :496: desigualdade com ID salvo. |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py:34` | `handle`: SELECT. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:537` | `_i6_pares` :537: SELECT; `handle` :1161/:1603/:1604/:1605/:1784: SELECT, filtro `isnull=False`, `exclude` por veredito e leitura direta. |
| `app/chamados/management/commands/reabrir_orfas_enterradas.py:69` | `handle`: SELECT. |
| `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:122` | `_reverter`: SELECT. |
| `app/chamados/management/commands/lavrar_perguntas_stale.py:88` | `handle`: SELECT. |
| `app/chamados/management/commands/backfill_veredito_lavrado.py:101` | `handle`: SELECT. |

**Lavra — 21 funções**, união dos três campos; quem lê e escreve aparece uma vez.

| Sítio | Forma |
|---|---|
| `app/chamados/veredito_lavrado.py:270` — `lavrar` | Atribui veredito :270, julgamento :272 e expiração :273; salva :275. |
| `app/chamados/veredito_lavrado.py:250` — `gravar_lavras_em_lote` | `bulk_update` por `CAMPOS_DA_LAVRA` :237. |
| `app/chamados/veredito_lavrado.py:287` — `desfazer_lavra_em_memoria` | Restaura os três campos apenas em memória :287–289. |
| `app/chamados/management/commands/backfill_veredito_lavrado.py:120` — `handle` | Comparações `==`/`!=`, truthiness :120–135; atribuições :138–140; persistência pela porta :143/:146. |
| `app/chamados/models.py:308` — `_gravar_estado` | Import `lavrar as _lavrar_s2`; chamada :312. |
| `app/chamados/models.py:3003` — `_nascer` | Import `lavrar as _lv_s2`; lavra em memória e save :3004–3006. |
| `app/chamados/models.py:3146` — `carimbar` | Módulo `veredito_lavrado as _vl`; foto, lavra, save e restauração :3147–3154. |
| `app/chamados/services/fio.py:926` — `reapontar_resolvedora` | Alias `_lavrar`, `salvar=False`; persistência conjunta :927–928. |
| `app/chamados/services/fio.py:1042` — `rechavear_data_evento` | Alias `_lavrar`, `salvar=False`; persistência conjunta :1043. |
| `app/chamados/services/relavra_via.py:124` — `relavrar` | Foto, lavra/save :126–127, comparação :130 e auditoria :137. |
| `app/chamados/management/commands/lavrar_perguntas_stale.py:60` — `handle` | Seleciona stale; exige `--apply` :82; chama `lavrar` :89. |
| `app/chamados/veredito_lavrado.py:224` — `esta_stale` | Truthiness negativa, `is None`, `> agora`; relógio :228; marco `> julgado_em` :232. |
| `app/chamados/veredito_lavrado.py:114` — `relogio_virou` | `is None`; expiração `<= hoje` e julgamento local `< expiração` :116. |
| `app/chamados/veredito_lavrado.py:281` — `foto_da_lavra` | Tupla dos três atributos. |
| `app/chamados/services/raiox.py:34` — `pks_stale_por_varredura` | `values_list`; truthiness, `is None`, `> agora`, relógio e comparação de marco :41–46. |
| `app/chamados/services/raiox.py:135` — `creditos_perguntas` | `exclude(materializada_em__isnull=False, veredito_lavrado=VL_SIM)`. |
| `app/chamados/services/raiox.py:184` — `pergunta_sem_acao_possivel` | Truthiness; `== VL_SIM` :185; vazio chama juiz :187. |
| `app/chamados/services/tarefa_do_dia.py:351` — `CargaDaTarefa.__init__` | Mapa do veredito; `in ('sim','nao') and not esta_stale(...)` :352. |
| `app/chamados/services/tarefa_do_dia.py:141` — `_pela_decisao` | Leitura do mapa; `is None` :142; `== 'nao'` :144. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:1167` — `handle` | Pula stale; truthiness e desigualdade contra juiz real :1173. |
| `app/chamados/services/passivo_validado.py:83` — `_reversao_de` | `getattr(p,c,None)`; nomes vêm de `CAMPOS_REVERSAO` :64–66. |

**Complementos da contagem:**

| Sítio | Forma |
|---|---|
| `app/chamados/migrations/0053_pergunta_unica_por_familia.py:33` — `_no_fato` | `Q(data_evento__gte=HORIZONTE_DO_FATO)`. |
| `app/chamados/migrations/0053_pergunta_unica_por_familia.py:40` — `_guarda` | Agrupamento por dia, `Count`, filtro `n__gt=1` e contagem; modelo histórico por al

[truncado em 20000 de 39636 caracteres]

## 10/11 19:49 · sessao 88e167697efd · BO-DISPUTA-SEM-PERGUNTA · construtor · origem=Code

**Parei na condição explícita: o replay não reproduziu o reaponte. A cura ficou incompleta e o diff é parcial.** A reprodução preservada falha por assertion, sem erro de montagem. ([RED:62](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/test_bo_disputa_volta5_replay_red_memoria.py:62))

O caso encontrado é diferente da cadeia já triada: **no replay nasce uma pergunta**, porque ele resolve outro dia. O teste usa núcleo, resolvedor e gerador reais por AST; identidade/ORM/transação são doubles. Porta ORM real: **[nao verificado]**. ([reprodução:23](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/test_bo_disputa_volta5_replay_red_memoria.py:23)) [nao verificado]

| Passo | Resultado observado na sonda |
|---|---|
| Ensaio recebe `ultima_batida` de 08/07 | Reusa a pergunta de 08/07 da dona fechada. |
| Savepoint desfeito | Desfaz também seu reaponte. |
| Replay aplica o ato sobre o contexto antigo da dona | A data explícita de 09/07 prevalece sobre `ultima_batida`. |
| Porta recebe 09/07 | Nasce outra pergunta; o gerador chama `reatar_pergunta_orfa` uma vez. |
| Estado final | Uma DS, nenhum pai novo, **duas perguntas**; a original permanece sem o reaponte. |

As assertions positivas registram essa sequência antes do RED final. (`chamados/tests/test_bo_disputa_volta5_replay_red_memoria.py:57`)

A precedência está no resolvedor, que lê a data por motivo antes da batida; o replay conserva essa data antiga ao fazer o merge. Intervalo pertence à família MARCO, portanto não recebe o episódio da família BATIDA. (`chamados/services/disputa_emissao.py:354`, `chamados/services/disputa_emissao.py:370`, `chamados/services/disputa_emissao.py:694`, `chamados/catalogo/motivos.py:48`, `chamados/models.py:3452`)

Comando do RED preservado — resultado observado: `FAIL`, **`2 != 1`**:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_bo_disputa_volta5_replay_red_memoria.py
```

**O que ficou construído, sem entrega como cura concluída:** núcleo sem recursão, ensaio/savepoint/replay, causas internas, observação pelo `core.observ`, guarda do push e retirada de `Command._disputa` com lápide. (`chamados/services/disputa_emissao.py:546`, `chamados/services/disputa_emissao.py:413`, `core/observ.py:73`, `chamados/services/disputa_emissao.py:740`, `ponto/management/commands/reconciliar_grade.py:412`)

As sondas de (a)–(h) estão nestes sítios; a reprodução acima demonstra que elas ainda não bastavam para cobrir D3:

| Caso | Sítio |
|---|---|
| (a) Zero pergunta possível | `chamados/tests/test_bo_disputa_volta5_memoria.py:90` |
| (b) Estrangeira em dona aberta | `chamados/tests/test_bo_disputa_volta5_memoria.py:103` |
| (c) Fechada sem save/reatar; receivers ORM | `chamados/tests/test_bo_disputa_volta5_memoria.py:118`; `chamados/tests/test_bo_disputa_volta5.py:83` |
| (d) Ciclo e limite de replay | `chamados/tests/test_bo_disputa_volta4_memoria.py:44` |
| (e) Lote misto | `chamados/tests/test_bo_disputa_volta5_memoria.py:140` |
| (f) Idempotência | `chamados/tests/test_bo_disputa_volta5_memoria.py:155` |
| (g) Eleição após trava, dentro do atomic | `chamados/tests/test_bo_disputa_volta5_memoria.py:227` |
| (h) Reentrada e demais mutantes | `chamados/tests/test_bo_disputa_volta5_memoria.py:28` |

Os comandos dos mutantes entregues são os seguintes; **não reexecutei o conjunto final após a parada**, portanto seu resultado final fica **[nao verificado]**:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_bo_disputa_volta5_memoria.py --mutante reentrada
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_bo_disputa_volta5_memoria.py --mutante salva_fechada
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_bo_disputa_volta5_memoria.py --mutante sem_guarda_push
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_bo_disputa_volta5_memoria.py --mutante nasce_sem_pergunta
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_bo_disputa_volta5_memoria.py --mutante eleicao_antes_trava
PYTHONDONTWRITEBYTECODE=1 python3 chamados/tests/test_bo_disputa_volta5_memoria.py --mutante dedup_cala_replay
```

No censo, os escritores e leitores centrais ficaram assim:

| Campo/retorno | Escrita e leitura |
|---|---|
| `DS.motivos` | Núcleo cria e agrega; gerador consulta perguntas locais. Remoções continuam no modelo. (`disputa_emissao.py:576`, `:657`, `:564`; `chamados/models.py:1961`, `:2031`) |
| `DS.contexto`, `qtd`, `ultima_em` | Merge no emissor; resolvedor lê data e batida. (`disputa_emissao.py:525`, `:576`, `:354`, `:370`) |
| Pai, classificação, contexto e SLA | Nascimento pelo `ChamadoColaborador.abrir`; DS guarda o pai. (`disputa_emissao.py:640`, `:657`) |
| `DS.fechada_em` | Modelo fecha; núcleo/wrapper leem para impedir escrita na dona fechada e push. Receivers leem o valor. (`chamados/models.py:1932`; `disputa_emissao.py:576`, `:740`; `chamados/signals.py:28`, `:129`, `:153`) |
| Pergunta: dona, dia, célula e destino | Porta insere ou reaponta a existente preservando sua dona; núcleo lê FK/motivo e criação efetiva. (`chamados/models.py:2907`, `:3006`, `:3014`; `disputa_emissao.py:413`, `:569`) |
| Retorno nascimento/agregação | Núcleo mantém a tripla; wrapper consome também o coletor interno. (`disputa_emissao.py:546`, `:740`) |

Para cada leitor de `motivos`, o efeito previsto pelo diff parcial é:

| Leitor | Mudança de resultado |
|---|---|
| `api/foto_recebida.py:61` | Seleção acompanha a dona aberta com pergunta de foto. |
| `colaboradores/signals.py:71` | Revalidação de posto acompanha essa dona aberta. |
| `escala/signals.py:155` | Calibração de âncora acompanha essa dona aberta. |
| `chamados/models.py:2031` | Remoção conserva guardas de abertura e presença do motivo. |
| `auditar_disputas.py:88`, `:142`, `:169` | Audita/exibe motivos locais; comparação já usa perguntas locais. |
| `auditar_invariantes_chamados.py:50` | Resolve dias dos motivos mantidos; seleção de zero perguntas continua em `:62`. |
| `chamados/relatorios_disputa.py:52` | Rótulos passam a mostrar motivos locais. |
| `chamados/services/acoes_disputa.py:149` | Mesmo efeito na mensagem. |
| `chamados/services/fio.py:680` | Mesmo efeito no fio. |
| `pauta_aparelhos_compartilhados.py:49` | Cobertura pode passar à dona ou omitir motivo recusado sem pergunta. |
| `alertar_disputas_pendentes.py:82` | Log mostra os motivos mantidos. |
| `templates/chamados/detalhe_local.html:301` | Exibe os motivos mantidos. |
| `templates/chamados/partials/modal_fio.html:325` | Aviso acompanha a dona aberta com âncora. |
| `colaboradores/services/drawer.py:75` | Sem mudança direta: usa perguntas vivas em `:78`. |

Efeitos dessas mudanças nas cenas de produção: **[nao verificado]**.

Arquivos tocados no diff parcial:

- Produção: `chamados/services/disputa_emissao.py:257`, `core/observ.py:73`, `ponto/management/commands/reconciliar_grade.py:412`.
- Testes BO: `test_bo_disputa_sem_pergunta.py:32`, `test_bo_disputa_sem_pergunta_memoria.py:1`, `test_bo_disputa_volta2_memoria.py:1`, `test_bo_disputa_volta3_memoria.py:1`, `test_bo_disputa_volta4_memoria.py:1`, `test_bo_disputa_volta5_memoria.py:1`, `test_bo_disputa_volta5.py:1`, `test_bo_disputa_volta5_replay_red_memoria.py:1` — todos em `chamados/tests/`. [nao verificado]
- Selos: `chamados/tests/test_porta_chamado_e3.py:91`, `chamados/tests/test_sla_pelo_juiz.py:114`, `chamados/tests/test_porta_disputa_emissao.py:83`, `chamados/tests/test_selo_derivacao_pela_data_da_pergunta.py:133`, `core/tests/test_medida_turno_do_par.py:267`.

Suite Django, PostgreSQL, receivers reais e ruff: **[nao verificado]**. Para a certificação autorizada continuam pendentes os módulos BO acima, `test_porta_disputa_emissao`, `test_disputa_s127`, `test_c7_red_minimos`, `test_invariantes_disputa`, `test_hx_disputa_muda_sem_ata` e o módulo de ordem de trava indicado no pedido. **[nao verificado]** [nao verificado]

Órfãos, `_criar_pergunta_se_couber`, declaração, fábrica, smoke e auditor continuam reservados à 5b; a parada desta volta está no replay do núcleo. (`chamados/services/disputa_emissao.py:922`, `:1126`, `:711`, `:688`)

_3488 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
