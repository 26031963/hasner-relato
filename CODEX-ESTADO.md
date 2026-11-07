# CODEX-ESTADO

> Ultima resposta processada: **06/11/2026 22:28** · publicado em **06/11/2026 22:28** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 06/11 22:28 · sessao 1509e73a8ffd · - · - · origem=Ronald (tmux)

Complemento dos leitores genéricos aberto no código:

| Sítio/função | Leitura e tratamento de férias |
|---|---|
| `api.views.api_ausencias` GET | Lista as últimas 30 Ausencias do colaborador **sem excluir tipo/status**; serializa tipo, data, status, descrição, minutos e criação. Inclui `ferias`. `app/api/views.py:1861`, `:1870`, `:1873`, `:1875`, `:1877`, `:1878` |
| `api.views.api_chamados_v2` | Busca Ausencia pelo ID do contexto e colaborador, sem filtro de tipo; lê documento/status para oferecer anexo. O ramo exige status literal `pendente`. `app/api/views.py:1059`, `:1066`, `:1071` |
| `api.views.api_espelho_v2` | Carrega cobertura `para='cobranca'`, sem exclusão local de férias; lê o status para acrescentar `ausencia_pendente`. `app/api/views.py:1574`, `:1575`, `:1584`, `:1586` |
| `api.views.api_ausencia_documento` | Busca qualquer tipo pelo ID e colaborador; só aceita estados de `ABERTAS`. Assim a Ausencia de férias aprovada pode ser lida, mas o anexo é recusado pela condição de estado. `app/api/views.py:3092`, `:3101`, `:3102`, `:3103` |
| `api_mensageria_ausencias` | Agrega tipo/status por `ausencias_do_dp`; **exclui férias** por delegação. `app/api/views_mensageria.py:736`, `:741`, `:742`, `:743`; exclusão efetiva em `app/ponto/turnos.py:3067`, `:3068` |
| `api_mensageria_pessoa` | Lista ausências pela mesma porta que exclui férias; lê datas/tipo/status. O bloco de férias usa os serviços de Agendamento separadamente. `app/api/views_mensageria.py:1030`, `:1031`, `:1032`, `:1044`, `:1045` |
| `folha.export.ausencias_na_janela` | Leitor genérico de Ausencias aprovadas que intersectam a janela; aceita qualquer tipo recebido, inclusive férias. `app/folha/export.py:71`, `:87`, `:88`, `:90` |
| `folha.export.datas_no_periodo`, `_datas_ausencia_tipo_janela`, `_datas_ausencia_tipo` | Leem tipo/data/data_fim e expandem pela autoridade `dias_da_ausencia`; não excluem férias localmente. `app/folha/export.py:63`, `:69`, `:70`, `:146`, `:160`, `:161`, `:165` |
| `folha.export.minutos_de_falta_por_dia`, `datas_falta_janela` | Filtram pelos tipos de `descontam_vigentes`, não por férias literal. `app/folha/export.py:138`, `:140`, `:179`, `:181` |
| `folha.export.tipos_abonados_8932`, `datas_abonadas` | **Excluem férias expressamente** de 8932, independentemente do efeito cadastrado. `app/folha/export.py:193`, `:198`, `:201`, `:204`, `:205`; `app/ponto/catalogo/ausencias.py:290` |
| `relatorios.views.absenteismo` | Inclui qualquer tipo aprovado, portanto férias; lê tipo/datas, agrupa ocorrências e dias pelo juiz. Entrega tela e CSV. `app/relatorios/views.py:197`, `:204`, `:205`, `:209`, `:214`, `:215`, `:218`, `:234` |
| `relatorios.views.atestados_acumulados` | Filtra pelos tipos do catálogo vivo `TIPOS_MEDICOS`; não há exclusão literal de férias. Na semente, férias têm `medico=False`; classificação de produção **[não verificado]**. `app/relatorios/views.py:277`, `:282`, `:283`; `app/ponto/catalogo/ausencias.py:101`, `:385`, `:387` |
| `relatorios.pdf_espelho._coletar_dados_espelho` | Lê efeitos da ausência por delegação para obter justificadas, suprimidas, minutos parciais e dias inteiros, abrangendo férias conforme a cobertura. `app/relatorios/pdf_espelho.py:332`, `:333`, `:334`, `:335`, `:336`, `:406` |
| `inteligencia.views.index` | Conta ausências por tipo sem excluir férias nem status; “sem ausência” também consulta todas as Ausencias. `app/inteligencia/views.py:71`, `:84`, `:127` |
| `inteligencia.views.api_kpi` | “Sem ausência” considera qualquer tipo; “Top faltosos” conta qualquer ausência no período, incluindo férias. `app/inteligencia/views.py:231`, `:232`, `:238`, `:239`, `:242`, `:243`, `:244` |
| `inteligencia.views.exportar_pdf` | O ranking “Top Faltosos” também conta qualquer Ausencia do período, sem excluir férias/status. `app/inteligencia/views.py:516`, `:538`, `:539`, `:611`, `:613`, `:614` |
| `inteligencia.resumo.resumo_colaborador` | Lista todas as ausências recentes; totais de faltas/justificadas usam as aprovadas e o efeito vivo do catálogo. `app/inteligencia/resumo.py:39`, `:42`, `:43`, `:44`, `:45` |
| `_score_assiduidade` e `gerar_alertas.Command.handle` | Excluem rejeitadas e tipos de `abonam_vigentes`; férias ficam fora **se** continuarem abonadas no catálogo vivo. A semente diz ABONA, mas cadastro de produção **[não verificado]**. `app/inteligencia/management/commands/calcular_scores.py:12`, `:14`; `app/inteligencia/management/commands/gerar_alertas.py:55`, `:58`; `app/ponto/catalogo/ausencias.py:101`, `:249`, `:250`, `:278` |

Templates abertos que apresentam essas leituras:

- Absenteísmo apresenta tipo, ocorrências e dias, incluindo o grupo férias recebido da view. `app/templates/relatorios/absenteismo.html:36`, `:41`, `:42`, `:43`.
- Inteligência apresenta a pizza de tipos; “Top faltosos” e “Sem ausência” abrem os endpoints acima. `app/templates/inteligencia/index.html:91`, `:99`, `:141`, `:381`, `:382`.
- Fio do colaborador apresenta justificadas e as cinco Ausencias recentes, sem excluir férias. `app/templates/chamados/fio_colaborador.html:59`, `:60`, `:62`.
- Perfil do colaborador apresenta `ausencias_360` com data/tipo; a origem desse contexto não foi aberta nesta complementação **[não verificado]**. `app/templates/colaboradores/detalhe.html:63`, `:72`, `:74`, `:75`.
- Espelho apresenta o tipo e status da ausência recebida no dia, sem exclusão de férias no template. `app/templates/ponto/espelho.html:191`.
- Lista de Ausencias percorre os registros recebidos e apresenta seu tipo. `app/templates/ponto/ausencias.html:142`, `:157`.

`pautas.semanal.escrever_semanal` recebe contadores prontos em `linhas`, sem consultar Ausencia nesse caminho; a decisão sobre quais contadores estão acesos pertence ao chamador. `app/pautas/semanal.py:202`, `:206`, `:207`, `:214`. Os rótulos de problemas de ausência são apresentados nesse módulo, mas não constituem leitura da tabela Ausencia. `app/pautas/semanal.py:56`, `:58`, `:62`. Outros caminhos indiretos de pautas: **[não verificado]**.

## 06/11 22:25 · sessao cb8a8a969eb0 · - · - · origem=Ronald (tmux)

**Achado de classe: a proteção nova não fecha todos os escritores de `Ausencia(tipo='ferias')`.** As portas genéricas ainda podem escrever esse tipo sem consultar o agendamento: lançamento admin (`app/ponto/views.py:2378`, `:2415`), PWA (`app/ponto/views.py:2476`, `:2509`, `:2536`), API app (`app/api/views.py:1720`, `:1771`, `:1820`), Resolver dia (`app/ponto/views.py:3340`, `app/ponto/services/veredito_celula.py:311`, `:315`) e edição (`app/ponto/views.py:2761`, `:2794`, `:2796`; `app/ponto/services/ausencia.py:474`, `:498`, `:509`).

O `AusenciaForm` contém a vedação de férias (`app/ponto/forms.py:31`, `:65`), mas esses cinco caminhos não o aplicam: recebem o tipo diretamente e chamam o serviço nas linhas acima. A porta `criar_ausencia` valida catálogo e colisão, constrói a linha e salva; não exige agendamento ativo (`app/ponto/services/ausencia.py:193`, `:377`, `:385`, `:418`). Logo, após cancelar, um POST genérico pode recolocar ausência de férias na mesma janela livre, inclusive aprovada via admin/Resolver dia (`app/ponto/views.py:2420`; `app/ponto/services/veredito_celula.py:316`). Isso não ressuscita `AgendamentoFerias.status` nem devolve saldo novamente; é uma divergência entre decisão e espelho (`app/ferias/services.py:379`, `:382`; `app/ponto/services/ausencia.py:341`).

**Contraexemplo de foto velha:** edição carrega uma Ausência de férias (`app/ponto/views.py:2761`); cancelamento remove essa linha (`app/ponto/services/ausencia.py:1188`); edição passa a foto para `editar_ausencia_registro`, que faz `ausencia.save()` completo (`app/ponto/views.py:2794`; `app/ponto/services/ausencia.py:509`). O modelo não verifica o agendamento nem usa `force_update` (`app/ponto/models.py:868`, `:908`). Reinserção pelo fallback UPDATE→INSERT do Django: **inferência, execução e fonte ORM na cópia [não verificado]**.

Censo dos escritores da ausência de férias:

- Espelho: `AgendamentoFerias.save` → `_sync_ausencia` → `espelhar_ferias`; cria pela porta ou apaga quando cancelado (`app/ferias/models.py:439`, `:443`, `:384`; `app/ponto/services/ausencia.py:1180`, `:1188`, `:1202`).
- Cancelamento admin rejeita antes de o espelho apagar (`app/ferias/services.py:407`, `:413`; `app/ponto/services/ausencia.py:1281`).
- Troca de janela apaga o trecho antigo antes do novo espelho (`app/ferias/services.py:441`, `:446`).
- Exclusão do agendamento apaga a janela (`app/ferias/models.py:446`).
- Escritores genéricos: criação e edição já enumeradas; decisões de estado/carimbos (`app/ponto/services/ausencia.py:808`, `:824`, `:835`, `:897`, `:927`), anexação/reabertura (`:846`, `:1071`, `:1106`, `:1130`, `:1141`), rederivação de fim (`:1293`, `:1296`).
- Endpoints genéricos de decisão aceitam qualquer tipo: aprovar/rejeitar/cancelar em lote (`app/ponto/views.py:2282`, `:2290`, `:2316`, `:2319`, `:2345`, `:2351`); reverter rejeição (`:2583`), aprovar individual (`:2606`), editar período aprovado (`:2670`), rejeitar (`:2687`), cancelar (`:2728`).
- Comandos: `curar_ausencia_sem_fim` seleciona qualquer tipo não aberto com duração incoerente e chama rederivação (`app/ponto/management/commands/curar_ausencia_sem_fim.py:29`, `:46`); `rederivar_minutos_h06_comp11` chama edição, mas relê e trava a ausência antes (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:138`, `:169`).
- Django admin de Ausência é somente leitura (`app/ponto/admin.py:37`).

Leitores agrupados:

- Autoridades de vigência/cobertura, incluindo férias: `app/ponto/turnos.py:2536`, `:2733`, `:3071`; para folha só aprovada, para cobrança qualquer não rejeitada (`:2509`).
- Precedência lê todas as ausências não rejeitadas (`app/ponto/precedencia.py:273`); fechamento usa cobertura de folha (`app/ponto/services/fechamento.py:680`, `:683`); grade usa a mesma autoridade (`app/escala/utils.py:1195`).
- Cartões/histórico: `app/ponto/services/dia_decidido.py:529`, `:550`, `:566`, `:587`; API lista ausências de todos os tipos (`app/api/views.py:1861`); vigia conta férias explicitamente (`app/ponto/services/vigia_ausencia.py:215`).
- Eventos de inclusão/exclusão da ausência julgam os dias (`app/ponto/signals.py:143`, `:150`).

**Quem lê status alinhado:** badges e botões (`app/templates/ferias/_tabela_ferias.html:42`; `app/templates/ferias/_secao_colab.html:120`, `:124`, `:140`, `:175`; `app/templates/ferias/saldo.html:63`, `:70`), KPI/lista de próximas, aviso pendente e concluídas (`app/ferias/views.py:26`, `:32`, `:75`, `:82`), API férias (`app/api/views.py:2262`), disponibilidade “voltando” (`app/colaboradores/views.py:2843`), aviso de retorno (`app/ferias/management/commands/alertar_retornos.py:20`), fracionamento/situação (`app/ferias/situacao.py:117`, `:164`), permissões do objeto (`app/ferias/models.py:324`, `:328`, `:332`, `:339`, `:346`).

**Gate atual e folha não precisam de `status='em_gozo'`:** gate usa `em_gozo_hoje` (`app/ponto/utils.py:85`, `:89`), aviso idem (`app/ponto/services/aviso_da_batida.py:73`), chip/lente idem (`app/ferias/views.py:25`, `:74`), folha usa `ferias_na_janela` (`app/folha/export.py:484`, `:491`). A autoridade usa datas e exclui apenas cancelado (`app/ferias/services.py:51`, `:63`); está registrada em `app/core/juizes.py:250`.

As portas de cancelamento, remarcação e confirmação adquirem agendamento→período (`app/ferias/services.py:364`, `:367`), e só depois escrevem ausência (`:407`, `:441`, `:446`). Cron trava apenas agendamento (`:594`) antes do espelho; não observei inversão demonstrada entre esses caminhos. Ciclo global de deadlock **[não verificado]**.

## 06/11 22:25 · sessao be4f014968c2 · - · - · origem=Ronald (tmux)

Censo de produção, somente leitura:

| Campo | Escritores persistentes |
|---|---|
| `status` | Histórico: `app/ferias/services.py:282`, save `:298`; criação comum e aprovação usam default `agendado`: `:321`, `:489`, `:499`, `app/ferias/models.py:309`; canceladores: `services.py:382`, `:412`; remarcação/correção: `:438`, `:446`; cron: `:594`, `:597`. |
| Datas | Criação histórica `services.py:280`; comum `:322`; aprovação `:491`; alteração existente exclusivamente `_trocar_janela`, `:433`, `:446`. |
| `confirmacao` | Defaults `models.py:314`; envio `services.py:545`, `:547`; resposta `:562`, `:565`, `:567`. |
| `confirmado_em` | Default nulo `models.py:318`; envio limpa `services.py:546`; confirmação carimba `:566`; recusa não limpa `:562-563`. |

Os saves parciais encontrados são: `['status']`, `['status','obs']`, datas/dias/aviso/status e campos de confirmação; somente estes últimos deixam de espelhar, coerentemente com a mudança (`app/ferias/services.py:383`, `:413`, `:446`, `:547`, `:563`, `:567`, `:597`; `app/ferias/models.py:437-444`). Não apareceu dependência de espelhamento com `dias`, `obs`, `aviso_emitido` ou `agendado_por` isolados no censo de produção; completude contra reflexão/código externo: [não verificado]. O comentário “depende SO” omite `agendado_por`, lido para autoria das ausências novas (`app/ponto/services/ausencia.py:1205`), mas os escritores abertos só o atribuem na criação (`app/ferias/services.py:283`, `:323`, `:495`).

Leitores abertos:

- Autoridades: `ferias_na_janela` lê datas e exclui cancelado, `em_gozo_hoje` delega; `divergencia_status` e `comecam_entre` leem status/datas (`app/ferias/services.py:51`, `:63`, `:77-82`, `:101-108`).
- Modelo: permissões/properties de edição/cancelamento `app/ferias/models.py:324-346`; frações `:353-355`; sobreposição `:394-428`; período excluível `:243`.
- Views: filtros/KPIs `app/ferias/views.py:26-32`, `:72-90`; guards e auditorias `:366-382`, `:414-426`, `:465`, `:483-517`, `:544`, `:668`, `:836`; confirmação `:860`, `:908-909`; notificações `:878`, `:885`, `:925`.
- Situação: frações, vivos, avisos, batidas e colisões (`app/ferias/situacao.py:117-122`, `:164-168`, `:246-262`, `:270-280`, `:343-352`).
- Templates: `app/templates/ferias/saldo.html:59-70`; `_tabela_ferias.html:39-42`; `_secao_colab.html:117-176`; `cancelar_admin_massa.html:42-45`; `_meu_atendimento_ferias.html:6`.
- API: `app/api/views.py:2252-2268`; mensageria `app/api/views_mensageria.py:840-843`, `:900-901`, `:939-944`, `:1045-1046`, `:1093-1095`.
- Disponibilidade/retornos: `app/colaboradores/views.py:2843-2853`, `:2895-2896`; `app/ferias/management/commands/alertar_retornos.py:20-24`.
- Gate e aviso de batida consultam a autoridade pelas datas, sem depender de `status='em_gozo'` (`app/ponto/utils.py:85-89`; `app/ponto/services/aviso_da_batida.py:73-74`). Substituições leem a janela (`app/ponto/utils.py:197-200`); folha também (`app/folha/export.py:491-493`).
- Inteligência continua procurando `status='aprovado'`, ausente nas choices atuais (`app/inteligencia/views.py:94`, `:278`, `:286`; `app/ferias/models.py:293-309`).
- Pendências de confirmação: `app/chamados/views.py:627-629`; badges e botão `app/templates/ferias/_secao_colab.html:122-124`. Leitor de produção de `confirmado_em` além de sua escrita: [não verificado].

Duas ressalvas adicionais:

1. Cancelamento preserva `confirmacao='aguardando'`, e a query das pendências não exclui cancelados; o cartão pode continuar aparecendo, embora a resposta seja recusada pelo escritor (`app/ferias/services.py:382-383`, `:412-413`; `app/chamados/views.py:627-629`; `app/ferias/views.py:918-920`).
2. Duas respostas com fotos `aguardando`: confirmar e depois recusar ainda são aceitas; a segunda sobrescreve confirmação sem limpar o carimbo, porque a guarda travada só recusa cancelado (`app/ferias/services.py:560-567`). Isso não recria o espelho, porque os saves não tocam seus campos (`app/ferias/models.py:437-444`).

Ressalva do selo estrutural: o censo AST executado com `python3 -B` classificou os saves dos canceladores como `entidade=None`; o gate ignora essa classe antes de verificar a porta. Portanto ele não protege esses dois pontos contra movimento futuro para fora da porta (`app/ferias/services.py:383`, `:413`; `app/core/censo_escritas.py:489-491`; `app/ponto/tests/test_chokepoint_ausencia_gate.py:33-35`).

## 06/11 22:24 · sessao 1509e73a8ffd · - · - · origem=Ronald (tmux)

Auditoria estática: **não procede que cada teste morda o pai f9d6f3e2**. Há 23 testes; somente cinco distinguem a volta 3 do pai pelo valor. Resultado de execução contra o pai: **[não verificado]**. A alegação geral está em `app/ferias/test_cancelar_2x.py:20`.

| Testes | Contra o pai v2 fornecido | [nao verificado]
|---|---|
| Cinco de cancelamento DP, iniciados nas linhas 83, 90, 97, 104 e 112 | Já cobertos pela cura v2; não distinguem v3. `app/ferias/test_cancelar_2x.py:83`, `:90`, `:97`, `:104`, `:112` |
| Cinco de admin, iniciados em 120, 128, 137, 144 e 154 | Já passam conceitualmente no pai v2, que já contém `permitir_concluido`. Os dois últimos eliminam o TypeError desprotegido contra a base, mas não produzem RED contra v2. `app/ferias/test_cancelar_2x.py:120`, `:128`, `:137`, `:144`, `:154`; assinatura atual em `app/ferias/services.py:388` |
| Cron remarcado durante a corrida | **Morde pelo valor**: espera `agendado`; pai grava `concluido` calculado da foto. `app/ferias/test_cancelar_2x.py:187`, `:190`, `:194` |
| Cron com cancelamento e cron normal | Não distinguem v3: terminalidade já corrigida em v2; controle normal também passa. `app/ferias/test_cancelar_2x.py:201`, `:210` |
| Três de troca de janela | Não distinguem v3: trava, saldo vivo e recusa de cancelado já pertencem a v2. `app/ferias/test_cancelar_2x.py:227`, `:230`, `:233` |
| POST cancelar duas vezes | Não distingue v3. `app/ferias/test_cancelar_2x.py:247` |
| Premissa “ativo espelha/cancelar apaga” e confirmação normal | Controles que passam no pai. `app/ferias/test_cancelar_2x.py:272`, `:301` |
| Confirmar, recusar e enviar cancelado | **Três mordidas pelo valor**: a asserção de Ausencia/status/confirmacao vem antes do acesso à nova classe da exceção. `app/ferias/test_cancelar_2x.py:277`, `:281`, `:282`, `:284`, `:287`, `:290` |
| Save parcial da foto cancelada | **Morde pelo valor**: espera zero Ausencias e status cancelado. `app/ferias/test_cancelar_2x.py:293`, `:299` |

O item procedente 3 da triagem foi corrigido quanto ao TypeError antes da primeira asserção: a porta HTTP não passa o argumento novo pelo teste; o teste direto captura a primeira exceção e confere o estado antes de `assertIsNone`. Isso não prova RED contra v2, onde a assinatura já existe. `app/ferias/test_cancelar_2x.py:144`, `:149`, `:150`, `:154`, `:156`, `:157`, `:158`.

Nas views, **escritor que recusou por cancelamento não gera push nem sucesso**: envio retorna nos dois ramos de erro antes de PWA/FCM; resposta do colaborador retorna antes do push DP e das respostas positivas. `app/ferias/views.py:869`, `:874`, `:875`, `:883`, `:891`, `:918`, `:920`, `:921`, `:929`, `:932`. `resposta_acao(ok=False)` gera tipo `erro`, inclusive no toast HTMX. `app/core/respostas.py:50`, `:63`.

Há um contraexemplo preexistente para **confirmação com dois significados**: duas requests carregam `aguardando`; a primeira confirma, a segunda recusa. Sob trava, o escritor confere apenas `cancelado`, não `confirmacao`; o segundo ato deixa `confirmacao='recusado'` com `confirmado_em` da primeira confirmação. A view ainda envia “Férias recusadas” e sucesso porque o escritor aceitou. `app/ferias/views.py:908`, `:909`; `app/ferias/services.py:560`, `:562`, `:563`, `:565`, `:566`, `:567`; `app/ferias/views.py:924`, `:929`. Outro envio com foto elegível pode redefinir uma confirmação concorrente para `aguardando` e zerar seu carimbo, pois a elegibilidade está na foto da view e o escritor recusa apenas cancelado. `app/ferias/views.py:860`; `app/ferias/services.py:544`, `:545`, `:546`. Isso não recria Ausencia após a cura atual, porque esses saves não acionam o espelho. `app/ferias/models.py:437`, `:443`.

Limites dos selos:

- O teste chamado “sem escrita” confere somente `dias_concedidos`, `status` e `obs`; não verifica ausência de UPDATE. `app/ferias/test_cancelar_2x.py:59`, `:62`, `:97`, `:101`.
- O auxiliar cron descarta stdout; portanto esses testes não conferem `placar` ou classificação `recusado`. `app/ferias/test_cancelar_2x.py:185`; `app/ferias/management/commands/sincronizar_status_ferias.py:50`, `:54`.
- As corridas são injeções sequenciais após o juiz da foto; concorrência real e comportamento das travas no banco permanecem **[não verificado]**. `app/ferias/test_cancelar_2x.py:178`, `:182`, `:185`.
- Os novos testes de confirmação chamam serviços; os testes HTTP antigos cobrem fluxos normais, sem simular a recusa concorrente nem afirmar ausência de push. `app/ferias/test_cancelar_2x.py:277`, `:305`; `app/ferias/test_confirmacao_agendamento.py:56`, `:68`, `:78`.

## 06/11 22:12 · sessao e277584255b7 · HOLERITE-ZIP-MATRICULA · censo · origem=Code

**A v1 permanece acessível pelo roteamento e possui formulário de autoenvio. Não há base para declará-la morta apenas pela ausência de links externos.** Uso humano atual: **[nao verificado]**. (`app/config/urls.py:33`; `app/holerite/urls.py:9`; `app/templates/holerite/upload_lote.html:12`)

**(1) Referências à v1 e classificação**

| Arquivo:linha | Classificação e papel |
|---|---|
| `app/config/urls.py:33` | **PRODUÇÃO — roteamento:** monta `holerite.urls` sob `/holerite/`. |
| `app/holerite/urls.py:9` | **PRODUÇÃO — despacho:** registra `/holerite/upload-lote/`, nome `holerite:upload_lote`, callback `views.upload_lote`. |
| `app/holerite/views.py:1217` | Definição do sítio; exige login e ação `editar_folha` em `:1216` e `:1219`. |
| `app/holerite/views.py:1229` | **PRODUÇÃO — autorreferência:** `reverse('holerite:upload_lote')` quando a competência é inválida. |
| `app/holerite/views.py:1235` | **PRODUÇÃO — autorreferência:** mesmo reverse quando falta ZIP. |
| `app/holerite/views.py:1276` | **PRODUÇÃO — autorreferência:** mesmo reverse ao terminar o processamento. |
| `app/holerite/views.py:1279` | **PRODUÇÃO — render:** carrega `holerite/upload_lote.html`. |
| `app/templates/holerite/upload_lote.html:12` | **PRODUÇÃO — chamador HTML:** formulário POST sem `action`, portanto envia à URL atual; campo ZIP em `:39`, botão em `:42`. |
| `app/core/juizes.py:1095` | **Registro em código:** contém a string `def upload_lote(`; não chama a view nem sua URL. |
| `app/docs/BACKLOG.md:2064` | **DOC:** PORT-69. |
| `app/docs/CENSO_PORTAS.md:93` | **DOC:** linha do censo histórico. |
| `app/docs/CENSO_PORTAS.md:233` | **DOC:** nome e URL literal da v1. |
| `app/docs/TICKETS.md:699` | **DOC:** registra as duas telas e rotas. |

Não encontrei outra referência específica à v1 em `{% url %}`, menus, JS, API ou testes por nome/URL literal. Referências construídas dinamicamente e consumidores externos: **[nao verificado]**. Os formulários encontrados para a outra porta usam explicitamente `holerite:upload_lote_v2`. (`app/templates/holerite/_lote_drawer.html:17`; `app/templates/holerite/_drawer_modo_lote.html:13`)

**(2) Fecho exclusivo da remoção**

| Elemento | Evidência e consequência |
|---|---|
| Função `upload_lote` | Corpo completo em `app/holerite/views.py:1217`; processamento ZIP começa em `:1240`. Não delega a helper próprio. |
| Entrada de URL | `app/holerite/urls.py:9`: teria de sair junto; mantê-la apontaria para a função removida. |
| Template `holerite/upload_lote.html` | Único carregador identificado: `app/holerite/views.py:1279`. Seu formulário fica em `app/templates/holerite/upload_lote.html:12`. |
| Import `zipfile` em `views.py` | Declaração em `app/holerite/views.py:3`; único uso identificado nesse módulo em `:1240`. |
| Imports locais dentro da função | Saem com o corpo: `app/holerite/views.py:1223`, `:1227`, `:1228`, `:1233`, `:1234`, `:1252`, `:1268`, `:1269`. |

Não há classe Form nem outro helper exclusivo identificado: a função lê diretamente `POST`/`FILES` e executa o processamento no próprio corpo. (`app/holerite/views.py:1224`; `app/holerite/views.py:1230`; `app/holerite/views.py:1240`)

Os imports locais **não tornam exclusivos seus destinos**: `mes_ou`/`ano_ou` também são usados no upload avulso; `resposta_acao` também; `reverse` já é importado no topo; `ContentFile` é usado pelo motor. (`app/holerite/views.py:1154`; `app/holerite/views.py:1204`; `app/holerite/views.py:11`; `app/holerite/motor.py:403`)

Também permanecem `os`, os templates compartilhados e o pacote `holerite`, com outros consumidores. (`app/holerite/views.py:661`; `app/holerite/views.py:1698`; `app/templates/holerite/upload.html:1`; `app/templates/holerite/upload.html:4`; `app/holerite/urls.py:37`)

Os `__init__.py` de `holerite`, `management`, `management/commands` e `tests` foram inspecionados como vazios, sem linha citável; reexportação exclusiva: **[nao verificado por arquivo:linha]**. A entrada do registro também precisa ser reconciliada, pois um teste exige sua impressão no fonte. (`app/core/juizes.py:1095`; `app/core/tests/test_contract_portas.py:19`)

**(3) Testes que alcançam o sítio** [nao verificado]

| arquivo::classe::método | Alcance |
|---|---|
| `app/core/tests/test_rota_pede_sessao.py::RotaPedeSessaoTest::test_MORDE_nenhuma_rota_responde_sem_sessao` — `:255` | **Executa GET anônimo** pelo resolvedor: varredura em `:235`, chamada do callback em `:223`. Inclui holerite por `app/config/urls_ci.py:14`; na v1 alcança a guarda de login em `app/holerite/views.py:1216`. |
| `app/core/tests/test_contract_portas.py::FamiliaPortasTest::test_MORDE_cada_impressao_existe` — `:19` | Exige estaticamente a impressão registrada em `app/core/juizes.py:1095`; não executa upload. |
| `app/core/tests/test_contract_portas.py::CensoTest::test_MORDE_o_censo_acha_as_portas_do_admin` — `:32` | Inclui a rota no inventário; não faz POST nela. |
| `app/core/tests/test_contract_portas.py::CensoTest::test_a_metade_estatica_e_a_do_censo` — `:45` | Compara inventários de portas; não executa upload. |
| `app/core/tests/test_todo_template_compila.py::TodoTemplateCompilaTest::test_todo_html_da_arvore_compila` — `:44` | Compila o template por `get_template` em `:50`; não renderiza o fluxo. |
| `app/core/tests/test_contract_import_local_resolve.py::ImportLocalResolveTest::test_MORDE_todo_nome_importado_existe_no_modulo` — `:59` | Verifica os imports locais pelo fonte; inclui `holerite` em `:22`; não executa a função. |

Há ainda varredura estática do template nos seguintes métodos de `app/core/tests/test_contract_template_bem_formado.py::TemplateBemFormadoTest`: `test_MORDE_nenhum_atributo_lixo` (`:162`), `test_MORDE_nenhuma_marca_de_fonte_python` (`:169`), `test_MORDE_nenhum_comentario_vaza` (`:174`), `test_MORDE_nenhum_valor_cru_vira_markup` (`:180`) e `test_o_escape_da_casa_tem_UM_dono` (`:186`).

Não encontrei teste funcional específico de GET autenticado ou POST ZIP da v1. Cobertura funcional completa e resultado da execução dos testes: **[nao verificado]**. [nao verificado]

**(4) Outra porta de lote e proteção da ciência**

A porta canônica identificada é **`upload_lote_v2`, `/holerite/lote/`**. Há caminho de produção pelo quadrinho da matriz, passando por `drawer_modo` e pelo formulário do lote. (`app/holerite/urls.py:37`; `app/templates/holerite/matriz_dp.html:67`; `app/templates/holerite/_quadrinho_celula.html:17`; `app/holerite/views.py:357`; `app/templates/holerite/_drawer_modo_lote.html:13`)

Ela salva o arquivo e o `LoteHolerite`, dispara o command, apresenta prévia e dispara publicação na confirmação. O command chama `motor.processar`. (`app/holerite/views.py:1773`; `app/holerite/views.py:1781`; `app/holerite/views.py:1750`; `app/holerite/views.py:1759`; `app/holerite/management/commands/processar_lote_holerite.py:89`)

O casamento tem estas regras:

- **Domínio padrão:** nome completo normalizado — sem acentos, em maiúsculas e com espaços colapsados — restringido à empresa do CNPJ quando informado. (`app/holerite/motor.py:161`; `app/holerite/motor.py:168`; `app/holerite/motor.py:203`)
- **CPF presente na página:** prevalece se identificar exatamente um colaborador; esse retorno antecede o filtro por empresa. (`app/holerite/motor.py:199`)
- **Ressalva do configurável:** aceita `campo_match`, com fallback `codigo_dominio`, e constrói o filtro por esse campo. Portanto, não cabe afirmar que todas as modalidades impedem casamento por código/matrícula. (`app/holerite/views.py:1736`; `app/holerite/views.py:1741`; `app/holerite/motor.py:298`; `app/holerite/motor.py:218`)
- **V1 ZIP:** usa o nome-base do PDF para buscar `matricula_nexti`. (`app/holerite/views.py:1244`; `app/holerite/views.py:1246`)

A proteção ausente na v1 tem duas camadas: o motor marca existentes como `ja_publicado` e os pula; na gravação, consulta `assinatura__isnull=False` entre candidatos `ok` e lança `ValueError` se encontrar ciência. A v1 faz `.delete()` dos holerites da competência sem essa guarda; a assinatura possui `CASCADE`. (`app/holerite/motor.py:323`; `app/holerite/motor.py:344`; `app/holerite/motor.py:415`; `app/holerite/views.py:1255`; `app/holerite/models.py:193`)

Há teste de preservação da ciência em `app/holerite/tests/test_selo_comunicado.py::SeloRepublicacaoIncrementalTest::test_MORDE_republicar_nao_apaga_ciencia_e_cria_so_o_que_falta` (`:240`, verificação em `:264`). Cobertura que force especificamente o `ValueError`: **[nao verificado]**.

**(5) Cron, command, signal e JS**

Não encontrei chamada específica à URL v1 em cron, command, signal, API ou JS da árvore. Chamadas externas ou montagem dinâmica não identificada: **[nao verificado]**.

O command de lote encontrado pertence ao pipeline v2 e chama diretamente o motor; o cadastro de crons o descreve como job disparado pela view. (`app/holerite/management/commands/processar_lote_holerite.py:89`; `app/config/crons.py:1359`)

O service worker **pode repassar um GET já iniciado** para essa URL pelo transporte genérico; não origina uma chamada específica à v1 e ignora POST. (`app/static/sw.js:36`; `app/static/sw.js:45`; `app/static/sw.js:82`)

## 06/11 22:11 · sessao a2d45967af28 · - · - · origem=Ronald (tmux)

Fecho exclusivo encontrado:

- `app/holerite/views.py:1217`: a própria `upload_lote`; processamento integral fica no corpo até `:1282`, sem helper próprio.
- `app/holerite/urls.py:9`: rota exclusiva `upload-lote/`, nome `holerite:upload_lote`.
- `app/templates/holerite/upload_lote.html:1`: template exclusivo, renderizado por `app/holerite/views.py:1279`.
- `app/holerite/views.py:3`: import `zipfile`, cujo único uso neste módulo é `:1240`.
- Imports locais removíveis junto ao corpo: sanitizadores (`:1223`), respostas (`:1227`, `:1233`, `:1268`), `reverse` (`:1228`, `:1234`, `:1269`) e `ContentFile` (`:1252`). Isso não torna seus módulos exclusivos: sanitizadores também são usados em `:1154`; respostas em `:1204`; `reverse` já está no topo em `:11`; `ContentFile` é usado em `app/holerite/motor.py:403`.

O formulário é HTML direto, sem classe Form: `app/templates/holerite/upload_lote.html:12`; a view lê `POST`/`FILES` diretamente em `app/holerite/views.py:1224` e `:1230`. O form sem `action` submete à própria URL, portanto é chamador de produção quando a página é aberta (`app/templates/holerite/upload_lote.html:12`).

Não entram no fecho: `os`, usado também pelos jobs (`app/holerite/views.py:661`, `:1698`); `base.html` e `core/_voltar.html`, compartilhados por `app/templates/holerite/upload.html:1` e `:4`; pacote `holerite`, mantido pelas demais rotas (`app/holerite/urls.py:7`, `:37`). Os `__init__.py` inspecionados estão vazios, sem linha citável; existência de reexportação exclusiva: [nao verificado].

Testes que alcançam o sítio: [nao verificado]

| arquivo::classe::método | Alcance |
|---|---|
| `app/core/tests/test_rota_pede_sessao.py::RotaPedeSessaoTest::test_MORDE_nenhuma_rota_responde_sem_sessao` (`:255`) | Executa GET anônimo pelo resolvedor; varredura `:235`, chamada real do callback `:223`. Inclui holerite via `app/config/urls_ci.py:14`. Para v1, alcança o decorador `app/holerite/views.py:1216`, sem processamento ZIP. |
| `app/core/tests/test_contract_portas.py::FamiliaPortasTest::test_MORDE_cada_impressao_existe` (`:19`) | Contrato estático: exige a impressão registrada em `app/core/juizes.py:1095`. Remover a view exige reconciliar essa entrada. |
| `app/core/tests/test_contract_portas.py::CensoTest::test_MORDE_o_censo_acha_as_portas_do_admin` (`:32`) | Censo indireto, sem POST v1 (`:34`). |
| `app/core/tests/test_contract_portas.py::CensoTest::test_a_metade_estatica_e_a_do_censo` (`:45`) | Compara enumerações estáticas/dinâmicas de portas (`:49–51`); não executa o upload. |
| `app/core/tests/test_todo_template_compila.py::TodoTemplateCompilaTest::test_todo_html_da_arvore_compila` (`:44`) | Compila o template, enumerado em `:34–39`, por `get_template` em `:50`; não renderiza. |
| `app/core/tests/test_contract_import_local_resolve.py::ImportLocalResolveTest::test_MORDE_todo_nome_importado_existe_no_modulo` (`:59`) | Verifica imports locais da view por AST; inclui holerite em `:22`, percorre fontes em `:62–72`; não executa a função (`:10–12`). |
| `app/core/tests/test_contract_template_bem_formado.py::TemplateBemFormadoTest::test_MORDE_nenhum_atributo_lixo` (`:162`) | Varredura estática do template por `varrer`, `:76–100`. |
| Mesmo arquivo/classe, `test_MORDE_nenhuma_marca_de_fonte_python` (`:169`) | Idem. |
| Mesmo arquivo/classe, `test_MORDE_nenhum_comentario_vaza` (`:174`) | Idem. |
| Mesmo arquivo/classe, `test_MORDE_nenhum_valor_cru_vira_markup` (`:180`) | Varredura de HTML/JS em `:130–146`. |
| Mesmo arquivo/classe, `test_o_escape_da_casa_tem_UM_dono` (`:186`) | Varredura dos HTML em `:199–205`. |

Não encontrei teste funcional direto de GET autenticado/POST ZIP da v1 por nome de função, rota, URL literal ou campo `arquivo_zip`; cobertura funcional completa e resultado de execução: [nao verificado]. Os testes de sanitização dos outros apps usam suas próprias rotas, por exemplo `app/folha/tests/test_post_sanitizado_folha.py:17`, `app/core/tests/test_get_sanitizado_config.py:17` e `app/colaboradores/tests/test_get_sanitizado.py:34`.

## 06/11 22:11 · sessao 2afdac624006 · - · - · origem=Ronald (tmux)

A outra porta é **`upload_lote_v2`, `/holerite/lote/`**, registrada em `app/holerite/urls.py:37` sob o prefixo de `app/config/urls.py:33`. Há chamadores de produção no botão “+ Lote” (`app/templates/holerite/lista_dp.html:12`) e nos forms do drawer (`app/templates/holerite/_lote_drawer.html:17`, `app/templates/holerite/_drawer_modo_lote.html:13`).

O pipeline salva `LoteHolerite` e o arquivo original, dispara `processar_lote_holerite`, mostra prévia e dispara o mesmo command com `--publicar` na confirmação (`app/holerite/views.py:1750`, `app/holerite/views.py:1773`, `app/holerite/views.py:1781`, `app/holerite/views.py:1703`). O command chama `motor.processar`, controlando `dry_run` conforme a operação (`app/holerite/management/commands/processar_lote_holerite.py:89`).

O casamento atual do formato padrão **Domínio é por nome completo**, normalizado e restringido pela empresa do CNPJ quando informado (`app/holerite/motor.py:161`, `app/holerite/motor.py:168`, `app/holerite/motor.py:203`). Quando a página traz CPF e existe exatamente um colaborador com ele, o CPF precede o nome; esse retorno ocorre antes do filtro de empresa (`app/holerite/motor.py:199`). A carta/comunicado extrai CPF; o parser de recibos extrai nome e código (`app/holerite/motor.py:72`, `app/holerite/motor.py:117`).

**Ressalva necessária:** o formato configurável ainda aceita `campo_match`, com fallback `codigo_dominio` na view; o motor usa esse campo para construir o filtro. Portanto, não se pode afirmar que todas as modalidades dessa porta impedem matrícula (`app/holerite/views.py:1736`, `app/holerite/views.py:1741`, `app/holerite/motor.py:298`, `app/holerite/motor.py:218`).

A proteção da ciência tem duas camadas: holerites existentes da competência viram `ja_publicado` e são pulados; a gravação ainda verifica `assinatura__isnull=False` entre candidatos `ok` e lança `ValueError` se encontrar ciência (`app/holerite/motor.py:323`, `app/holerite/motor.py:344`, `app/holerite/motor.py:415`). A v1 apaga os holerites da competência sem essa consulta, e a assinatura tem FK `CASCADE`, podendo desaparecer junto (`app/holerite/views.py:1255`, `app/holerite/models.py:193`).

Testes relevantes **da porta canônica, não da v1**: [nao verificado]

- `app/holerite/tests/test_publicar_lote.py::SeloDeVIEWPublicarLoteTest::test_publicar_dispara_o_JOB_e_nao_parseia_no_request` — definição `:52`, chamada HTTP `:58`.
- `app/holerite/tests/test_publicar_lote.py::SeloDeVIEWPublicarLoteTest::test_publicar_com_previa_expirada_falha_alto` — definição `:69`, chamada HTTP `:71`.
- `app/holerite/tests/test_selo_job_lote.py::SeloJobLoteTest::test_MORDE_a_view_NAO_chama_o_motor` — definição `:72`, chamada HTTP `:81`.
- `app/holerite/tests/test_selo_job_lote.py::SeloJobLoteTest::test_o_job_escreve_progresso_e_deixa_a_previa` — `:100`, command `:102`.
- `app/holerite/tests/test_selo_job_lote.py::SeloJobLoteTest::test_MORDE_publicar_duas_vezes_nao_duplica` — `:125`.
- `app/holerite/tests/test_selo_c2_match_nome.py::SeloC2MatchNomeTest::test_MORDE_casa_por_nome_mesmo_SEM_codigo_no_cadastro` — `:61`.
- `app/holerite/tests/test_selo_c2_match_nome.py::SeloC2MatchNomeTest::test_MORDE_mesmo_nome_em_EMPRESAS_diferentes_o_CNPJ_desempata` — `:75`.
- `app/holerite/tests/test_selo_c2_match_nome.py::SeloC2MatchNomeTest::test_o_parser_dominio_declara_nome_completo` — `:111`.
- `app/holerite/tests/test_selo_comunicado.py::SeloRepublicacaoIncrementalTest::test_MORDE_republicar_nao_apaga_ciencia_e_cria_so_o_que_falta` — `:240`, preservação da assinatura verificada em `:264`.
- `app/holerite/tests/test_selo_comunicado.py::SeloRepublicacaoIncrementalTest::test_a_previa_avisa_quantos_ja_estao_publicados` — `:272`.
- `app/holerite/tests/test_selo_comunicado.py::SeloRepublicacaoIncrementalTest::test_MORDE_o_motor_nao_deleta_holerite_nenhum` — `:284`.
- `app/holerite/tests/test_selo_comunicado.py::SeloCpfDoComunicadoTest::test_MORDE_cpf_resgata_a_carta_com_nome_grafado_diferente` — `:356`.
- `app/holerite/tests/test_selo_comunicado.py::SeloCpfDoComunicadoTest::test_MORDE_cpf_desempata_homonimo` — `:366`.
- `app/holerite/tests/test_selo_comunicado.py::SeloCpfDoComunicadoTest::test_o_recibo_sem_cpf_segue_pelo_nome` — `:382`.
- `app/holerite/tests/test_selo_comunicado.py::SeloCpfDoComunicadoTest::test_cpf_que_nao_existe_cai_no_nome` — `:390`.

Cobertura dinâmica que force especificamente o `ValueError` da guarda de `app/holerite/motor.py:415`: **[nao verificado]**.

## 06/11 22:10 · sessao 3e5571285a6d · - · - · origem=Ronald (tmux)

Referências estáticas v1:

| Arquivo:linha | Classificação e papel |
|---|---|
| `app/config/urls.py:33` | PRODUÇÃO: monta `holerite.urls` sob `/holerite/`. |
| `app/holerite/urls.py:9` | PRODUÇÃO: registra `/holerite/upload-lote/`, nome `holerite:upload_lote`, callback `views.upload_lote`. |
| `app/holerite/views.py:1217` | Definição da view. |
| `app/holerite/views.py:1229` | PRODUÇÃO: autorreferência por `reverse`, competência inválida. |
| `app/holerite/views.py:1235` | PRODUÇÃO: autorreferência por `reverse`, ZIP ausente. |
| `app/holerite/views.py:1276` | PRODUÇÃO: autorreferência por `reverse`, fim do processamento. |
| `app/holerite/views.py:1279` | PRODUÇÃO: renderiza `holerite/upload_lote.html`. |
| `app/templates/holerite/upload_lote.html:12` | PRODUÇÃO: formulário `POST` sem `action`, envia à própria URL; ZIP em `:39`, submit em `:42`. |
| `app/core/juizes.py:1095` | Código de registro/censo: string `def upload_lote(`; não chama view nem URL. |
| `app/docs/BACKLOG.md:2064` | DOC: PORT-69. |
| `app/docs/CENSO_PORTAS.md:93` | DOC: linha do censo. |
| `app/docs/CENSO_PORTAS.md:233` | DOC: nome e URL literal. |
| `app/docs/TICKETS.md:699` | DOC: registra as duas telas/rotas. |

Os botões `+ Lote` encontrados chamam **v2**: `app/templates/holerite/lista_dp.html:12` e `app/templates/holerite/historico_lotes.html:7`.

Testes indiretos, sem executar a view: [nao verificado]

- `app/core/tests/test_contract_portas.py::FamiliaPortasTest::test_MORDE_cada_impressao_existe`, definição `:19`, lê fonte e exige impressão de `core/juizes.py:1095` em `:20–22`.
- `app/core/tests/test_contract_portas.py::CensoTest::test_MORDE_o_censo_acha_as_portas_do_admin`, `:32–43`, percorre censo.
- `app/core/tests/test_contract_portas.py::CensoTest::test_a_metade_estatica_e_a_do_censo`, `:45–51`, compara inventários.
- O inventário percorre resolver e lê fonte em `app/core/portas_da_tela.py:112–124`; não dispara request.

Nenhuma ocorrência específica de v1 encontrada na busca total `rg -uuu`, excluindo `.git`, em JS/static, API, commands, signals ou crons; completude dinâmica e chamadas externas **[nao verificado]**. O service worker repassa GET genericamente (`app/static/sw.js:33–36`, `:45–49`, `:82–85`) e ignora POST (`:36`), sem origem autônoma específica para essa URL. O command do lote chama `motor.processar` diretamente (`app/holerite/management/commands/processar_lote_holerite.py:89`); seu registro como job acionado pela view consta em `app/config/crons.py:1359`.

A rota está montada no código (`app/config/urls.py:33`), mas configuração implantada e uso humano atual **[nao verificado]**: SaaS permite substituir URLconf por ambiente (`app/config/settings/saas.py:77`).

## 06/11 22:06 · sessao 27e3556afc54 · FERIAS-CANCELAR-2X · verificador · origem=Code

**Pré-verificação: a volta 2 ainda não fecha pela classe.** Restam um cron capaz de gravar destino calculado de datas antigas, confirmação capaz de recriar o espelho de férias canceladas e um teste que falha no pai por parâmetro inexistente, antes de conferir valores. (`app/ferias/management/commands/sincronizar_status_ferias.py:38`; `app/ferias/services.py:578`; `app/ferias/services.py:538`; `app/ferias/models.py:437`; `app/ponto/services/ausencia.py:1202`; `app/ferias/test_cancelar_2x.py:146`.)

Este é um parecer estático. Execução dos testes, confirmação dos hashes históricos, concorrência reproduzida, banco, logs e impacto efetivo em produção: **[nao verificado]**. [nao verificado]

À pergunta **(1), quem escreve**, o censo produtivo encontrado é:

| Campo | Escritores |
|---|---|
| `AgendamentoFerias.status` | Criação normal e aprovação usam o padrão `agendado` do modelo: `app/ferias/models.py:309`, `app/ferias/services.py:321`, `app/ferias/services.py:484`. Histórico deriva pelas datas: `app/ferias/services.py:282`. Cancelamentos DP/admin: `app/ferias/services.py:377`, `app/ferias/services.py:407`. Troca de janela: `app/ferias/services.py:433`. Cron: `app/ferias/services.py:578`. |
| Datas e dias do agendamento | Criação normal: `app/ferias/services.py:322`; histórico: `app/ferias/services.py:280`; aprovação: `app/ferias/services.py:486`; alteração de existente centralizada em `_trocar_janela`: `app/ferias/services.py:428`, `app/ferias/services.py:441`. |
| `PeriodoAquisitivo.dias_concedidos` | Inicialização em zero: `app/ferias/views.py:185`, pela porta `app/ferias/services.py:259`. Histórico com saturação: `app/ferias/services.py:299`. Criação: `app/ferias/services.py:324`. Cancelamentos: `app/ferias/services.py:374`, `app/ferias/services.py:399`. Remarcação/correção: `app/ferias/services.py:427`. Aprovação: `app/ferias/services.py:495`. |
| `AgendamentoFerias.obs` | Texto recebido da tela: `app/ferias/views.py:287` → `app/ferias/services.py:323`. Histórico: `app/ferias/services.py:284`. Proveniência da solicitação: `app/ferias/services.py:491`. Lote: `app/ferias/views.py:637`. IA: `app/core/views_ia.py:688`. Cancelamento admin concatena carimbo: `app/ferias/services.py:406`, `app/ferias/services.py:408`. |
| Efeitos indiretos sobre o espelho | Confirmação não escreve status/datas, mas todo `save()` espelha a foto recebida: `app/ferias/services.py:538`, `app/ferias/services.py:547`, `app/ferias/services.py:551`, `app/ferias/models.py:437`. Exclusão individual limpa pela janela: `app/ferias/models.py:439`. Exclusão do ciclo remove seus agendamentos: `app/ferias/services.py:530`. |

Os **leitores de status/datas**, incluindo fora do diff, encontrados foram:

| Grupo | Referências |
|---|---|
| Consultas canônicas | Janela: `app/ferias/services.py:51`; férias hoje: `app/ferias/services.py:63`; divergências: `app/ferias/services.py:77`, `app/ferias/services.py:79`, `app/ferias/services.py:81`; começam na janela: `app/ferias/services.py:102`. |
| Modelo e guardas | Permissões/início: `app/ferias/models.py:324`, `app/ferias/models.py:328`, `app/ferias/models.py:332`, `app/ferias/models.py:339`, `app/ferias/models.py:346`; fracionamento: `app/ferias/models.py:353`, `app/ferias/models.py:362`; sobreposição: `app/ferias/models.py:394`, `app/ferias/models.py:400`, `app/ferias/models.py:420`, `app/ferias/models.py:428`; exclusão de ciclo: `app/ferias/models.py:243`. |
| Escritores que também leem | Duplicata: `app/ferias/services.py:316`; decisão sob trava: `app/ferias/services.py:359`; dias devolvidos: `app/ferias/services.py:374`, `app/ferias/services.py:399`; janela anterior: `app/ferias/services.py:426`; cron: `app/ferias/services.py:570`, `app/ferias/services.py:577`. |
| Espelho de ausência | Atividade e janela vêm do objeto recebido: `app/ponto/services/ausencia.py:1179`, `app/ponto/services/ausencia.py:1180`, `app/ponto/services/ausencia.py:1184`, `app/ponto/services/ausencia.py:1194`; pauta também lê janela: `app/ponto/services/ausencia.py:1251`. |
| Situação/avisos | `app/ferias/situacao.py:117`, `app/ferias/situacao.py:164`, `app/ferias/situacao.py:246`, `app/ferias/situacao.py:257`, `app/ferias/situacao.py:270`, `app/ferias/situacao.py:279`, `app/ferias/situacao.py:343`. |
| Views de férias | KPI/listagem: `app/ferias/views.py:26`, `app/ferias/views.py:27`, `app/ferias/views.py:32`, `app/ferias/views.py:72`, `app/ferias/views.py:76`, `app/ferias/views.py:78`, `app/ferias/views.py:80`, `app/ferias/views.py:82`, `app/ferias/views.py:88`, `app/ferias/views.py:90`; validação: `app/ferias/views.py:235`; guardas/auditoria: `app/ferias/views.py:364`, `app/ferias/views.py:366`, `app/ferias/views.py:382`, `app/ferias/views.py:406`, `app/ferias/views.py:414`, `app/ferias/views.py:426`, `app/ferias/views.py:465`, `app/ferias/views.py:483`, `app/ferias/views.py:515`; massa: `app/ferias/views.py:544`, `app/ferias/views.py:559`, `app/ferias/views.py:561`, `app/ferias/views.py:565`; edição: `app/ferias/views.py:668`; exclusão: `app/ferias/views.py:836`; confirmação: `app/ferias/views.py:860`, `app/ferias/views.py:871`, `app/ferias/views.py:878`, `app/ferias/views.py:913`. |
| Commands | Sincronização: `app/ferias/management/commands/sincronizar_status_ferias.py:33`, `app/ferias/management/commands/sincronizar_status_ferias.py:38`, `app/ferias/management/commands/sincronizar_status_ferias.py:50`; retornos: `app/ferias/management/commands/alertar_retornos.py:20`. |
| Outros consumidores | Disponibilidade: `app/colaboradores/views.py:2843`, `app/colaboradores/views.py:2853`, `app/colaboradores/views.py:2896`; API: `app/api/views.py:2252`, `app/api/views.py:2259`, `app/api/views.py:2262`; mensageria: `app/api/views_mensageria.py:840`, `app/api/views_mensageria.py:900`, `app/api/views_mensageria.py:901`, `app/api/views_mensageria.py:931`, `app/api/views_mensageria.py:941`, `app/api/views_mensageria.py:943`, `app/api/views_mensageria.py:1088`, `app/api/views_mensageria.py:1093`, `app/api/views_mensageria.py:1095`; atendimento: `app/chamados/views.py:627`; folha: `app/folha/export.py:491`, `app/folha/export.py:493`; inteligência: `app/inteligencia/views.py:94`, `app/inteligencia/views.py:278`, `app/inteligencia/views.py:286`. |
| Templates | `app/templates/ferias/_tabela_ferias.html:39`, `app/templates/ferias/_tabela_ferias.html:40`, `app/templates/ferias/_tabela_ferias.html:42`; `app/templates/ferias/cancelar_admin_massa.html:42`, `app/templates/ferias/cancelar_admin_massa.html:43`, `app/templates/ferias/cancelar_admin_massa.html:45`; `app/templates/ferias/saldo.html:59`, `app/templates/ferias/saldo.html:60`, `app/templates/ferias/saldo.html:63`, `app/templates/ferias/saldo.html:70`; `app/templates/ferias/_secao_colab.html:117`, `app/templates/ferias/_secao_colab.html:118`, `app/templates/ferias/_secao_colab.html:120`, `app/templates/ferias/_secao_colab.html:124`, `app/templates/ferias/_secao_colab.html:133`, `app/templates/ferias/_secao_colab.html:140`, `app/templates/ferias/_secao_colab.html:141`, `app/templates/ferias/_secao_colab.html:143`, `app/templates/ferias/_secao_colab.html:150`, `app/templates/ferias/_secao_colab.html:175`, `app/templates/ferias/_secao_colab.html:176`; `app/templates/ferias/_meu_atendimento_ferias.html:6`. |

Os **leitores de concedidos/saldo** encontrados foram:

| Grupo | Referências |
|---|---|
| Autoridade e permissões | Fórmula: `app/ferias/models.py:217`; status do ciclo: `app/ferias/models.py:95`; saldo para remarcar: `app/ferias/models.py:222`; permissões: `app/ferias/models.py:231`, `app/ferias/models.py:239`. |
| Serviços | Prévia/abono: `app/ferias/services.py:200`, `app/ferias/services.py:202`, `app/ferias/services.py:205`; aprovação: `app/ferias/services.py:480`; contas dos escritores nas mesmas linhas inventariadas acima. |
| Views | KPI/listagem: `app/ferias/views.py:31`, `app/ferias/views.py:60`; validação: `app/ferias/views.py:233`; auditoria: `app/ferias/views.py:368`, `app/ferias/views.py:383`, `app/ferias/views.py:416`, `app/ferias/views.py:427`, `app/ferias/views.py:486`, `app/ferias/views.py:518`; remarcação: `app/ferias/views.py:478`, `app/ferias/views.py:680`; direito: `app/ferias/views.py:812`; venda: `app/ferias/views.py:942`. |
| Situação | `app/ferias/situacao.py:69`, `app/ferias/situacao.py:178`, `app/ferias/situacao.py:182`, `app/ferias/situacao.py:192`, `app/ferias/situacao.py:206`, `app/ferias/situacao.py:296`. |
| IA/API | `app/core/views_ia.py:262`, `app/core/views_ia.py:269`, `app/core/views_ia.py:492`, `app/core/views_ia.py:503`, `app/core/views_ia.py:532`; `app/api/views_mensageria.py:833`, `app/api/views_mensageria.py:838`, `app/api/views_mensageria.py:952`, `app/api/views_mensageria.py:1101`. |
| Relatórios/commands | PDF: `app/relatorios/services.py:290`, `app/relatorios/services.py:316`; CSV: `app/relatorios/views.py:370`; alertas: `app/ferias/management/commands/alertar_ferias_vencendo.py:36`, `app/ferias/management/commands/alertar_ferias_vencendo.py:41`, `app/ferias/management/commands/alertar_ferias_vencendo.py:77`, `app/ferias/management/commands/alertar_ferias_vencendo.py:80`, `app/ferias/management/commands/alertar_ferias_vencendo.py:83`; pautas: `app/ferias/management/commands/pautas_de_ferias.py:97`. |
| Templates | `app/templates/ferias/saldo.html:37`, `app/templates/ferias/saldo.html:41`, `app/templates/ferias/saldo.html:46`; `app/templates/ferias/_secao_colab.html:49`, `app/templates/ferias/_secao_colab.html:50`, `app/templates/ferias/_secao_colab.html:67`, `app/templates/ferias/_secao_colab.html:68`, `app/templates/ferias/_secao_colab.html:77`; `app/templates/ferias/agendar.html:7`, `app/templates/ferias/agendar.html:22`; `app/templates/ferias/agendar_lote.html:40`; `app/templates/ferias/_tabela_ferias.html:27`; `app/templates/relatorios/ferias_vencendo.html:48`. |

Para `AgendamentoFerias.obs`, o leitor produtivo explícito encontrado é a concatenação do cancelamento admin; não encontrei interpretação desse texto como autoridade de status ou saldo. (`app/ferias/services.py:406`.) Completude diante de acessos dinâmicos: **[nao verificado]**.

À pergunta **(2), os quatro procedentes**, a situação é:

| Procedente da v1 | Resultado da leitura |
|---|---|
| Cron ressuscitava `cancelado` | Fechado nesse caminho: relê sob trava, preserva `cancelado` e devolve o status vivo. Porém continua aceitando destino derivado de datas antigas; detalhado na resposta (5). (`app/ferias/services.py:576`, `app/ferias/services.py:577`, `app/ferias/services.py:580`.) |
| Troca de janela ressuscitava cancelado e invertia a trava | Fechado para cancelar/remarcar/corrigir: todos passam por Agendamento → PA antes de Ausência, e a troca recusa cancelado. A declaração de ordem única **do módulo inteiro** continua falsa; ver resposta (4). (`app/ferias/services.py:359`, `app/ferias/services.py:362`, `app/ferias/services.py:425`, `app/ferias/services.py:436`.) |
| Admin comum/massa aceitavam concluído | Fechado nos chamadores encontrados: padrão recusa concluído; somente a porta de correção passa `True`. (`app/ferias/services.py:397`, `app/ferias/views.py:418`, `app/ferias/views.py:575`.) |
| RED por símbolo/qualquer exceção | Os testes de recusa agora verificam estado antes de consultar a classe. Entretanto o teste da porta concluída introduz outro erro de preparação histórica: parâmetro novo na primeira chamada, fora de `_tentar`. (`app/ferias/test_cancelar_2x.py:73`, `app/ferias/test_cancelar_2x.py:78`, `app/ferias/test_cancelar_2x.py:87`, `app/ferias/test_cancelar_2x.py:146`.) |

**A terminalidade ainda não cobre todos os efeitos.** Contraexemplo: carregar foto ativa para confirmação → cancelar e remover ausências → salvar confirmação pela foto antiga. O SQL altera apenas confirmação, mas `save()` chama o espelho usando status/datas antigos e pode recriar férias aprovadas. A coluna permanece cancelada e o saldo permanece devolvido; o espelho revive. O mesmo mecanismo pode recriar a janela antiga depois de remarcação. (`app/ferias/services.py:538`, `app/ferias/services.py:547`, `app/ferias/services.py:551`; `app/ferias/models.py:437`; `app/ponto/services/ausencia.py:1180`, `app/ponto/services/ausencia.py:1188`, `app/ponto/services/ausencia.py:1204`.) Reprodução: **[nao verificado]**.

O caminho HTTP existe: envio julga uma foto antes de salvar; resposta do colaborador busca por `confirmacao='aguardando'`, sem excluir cancelados. (`app/ferias/views.py:856`, `app/ferias/views.py:860`, `app/ferias/views.py:867`, `app/ferias/views.py:901`, `app/ferias/views.py:909`, `app/ferias/views.py:921`.)

Não identifiquei outro escritor produtivo encontrado que sobrescreva **a coluna** `cancelado` após essa correção; o cron e a troca passaram a proteger essa coluna. (`app/ferias/services.py:425`, `app/ferias/services.py:577`.) Ausência universal de caminhos dinâmicos: **[nao verificado]**.

O agregado sem trava permanece: criação/histórico/aprovação continuam somando sobre PA recebido ou consultado sem a trava comum. Isso conserva a classe já separada na triagem, sem constituir prova de nova dupla devolução do mesmo agendamento. (`app/ferias/services.py:299`, `app/ferias/services.py:324`, `app/ferias/services.py:474`, `app/ferias/services.py:495`.)

Quanto à **mordida de cada teste**, esta é a previsão estática pelos anexos; resultados executados contra pai/base: **[nao verificado]**. [nao verificado]

| Teste em `app/ferias/test_cancelar_2x.py` | Pai v1 — previsto | Base — previsto |
|---|---|---|
| `:83` mesmo objeto | Verde | RED: −30 | [nao verificado]
| `:90` objeto relido | Verde | RED: −30 | [nao verificado]
| `:97` segunda sem escrita | Verde | RED: −30 | [nao verificado]
| `:104` duas fotos | Verde | RED: −30 | [nao verificado]
| `:112` concluído DP | Verde | RED: 0/cancelado | [nao verificado]
| `:120` admin duas vezes | Verde | RED: −30 | [nao verificado]
| `:128` admin comum concluído | RED: 0 em vez de 30 | Mesmo RED |
| `:137` massa concluído | RED: 0 em vez de 30 | Mesmo RED |
| `:144` porta concluída | **Erro por `TypeError`** | **Mesmo erro** |
| `:154` cron foto velha | RED: `em_gozo` | Mesmo RED |
| `:164` cron normal | Verde | Verde | [nao verificado]
| `:182` remarcar cancelado | RED: −20 | Mesmo RED |
| `:185` corrigir cancelado | RED: −20 | Mesmo RED |
| `:188` saldo da foto | RED: 979 em vez de 10 | Mesmo RED |
| `:202` POST duas vezes | Verde | RED: −30 | [nao verificado]

Portanto, a afirmação universal de RED pelo valor não está sustentada: a primeira chamada do teste da concluída usa `permitir_concluido=True` antes dos asserts, parâmetro ausente nas assinaturas históricas fornecidas. (`app/ferias/test_cancelar_2x.py:20`, `app/ferias/test_cancelar_2x.py:146`, `app/ferias/test_cancelar_2x.py:147`; assinatura efetiva dos commits históricos: **[nao verificado]**.)

“Sem escrita” também mede somente igualdade de concedidos/status/obs; não mede ausência de SQL, ausências ou outros efeitos. As duas fotos são chamadas sequenciais, sem disputa real de conexões. (`app/ferias/test_cancelar_2x.py:59`, `app/ferias/test_cancelar_2x.py:62`, `app/ferias/test_cancelar_2x.py:101`, `app/ferias/test_cancelar_2x.py:106`.)

À pergunta **(3), tratamento das exceções**, todos os chamadores produtivos nominais encontrados tratam as filhas:

| Porta | Chamada / tratamento |
|---|---|
| DP | `app/ferias/views.py:330` / `app/ferias/views.py:331` |
| Admin comum | `app/ferias/views.py:370` / `app/ferias/views.py:371` |
| Correção concluída | `app/ferias/views.py:418` / `app/ferias/views.py:419` |
| Massa | `app/ferias/views.py:575` / `app/ferias/views.py:576` |
| Correção de datas | `app/ferias/views.py:503` / `app/ferias/views.py:504` |
| Remarcação | `app/ferias/views.py:688` / `app/ferias/views.py:689` |

O wrapper propaga; a massa soma recusas em pulados e incrementa feitos somente após sucesso. Seu `messages.success` apresenta os números reais, inclusive zero feitos, conforme o descarte anterior. (`app/ferias/views.py:527`, `app/ferias/views.py:577`, `app/ferias/views.py:579`, `app/ferias/views.py:581`.)

Não encontrei genérico engolindo essas recusas: o genérico da correção cobre só avisos, antes do escritor; o da aprovação envolve outra criação. A API encontrada é leitora; o admin de férias registra `Afastamento`, e o admin de Ausência é somente leitura. (`app/ferias/views.py:494`, `app/ferias/views.py:499`, `app/ferias/views.py:501`; `app/ferias/services.py:483`, `app/ferias/services.py:498`; `app/api/views.py:2252`; `app/ferias/admin.py:4`; `app/ponto/admin.py:38`.) Chamadas dinâmicas adicionais: **[nao verificado]**.

À pergunta **(4), atomic e ordem**, os usos encontrados de `select_for_update` estão dentro de `atomic`: os três chamadores do helper, o cron e a avaliação da solicitação. (`app/ferias/services.py:372`, `app/ferias/services.py:393`, `app/ferias/services.py:424`, `app/ferias/services.py:575`; `app/ferias/views.py:739`, `app/ferias/views.py:740`.)

**A ordem global não está uniformizada:**

| Caminho | Ordem observada |
|---|---|
| DP/admin/troca de janela | Trava Agendamento → PA → toca Ausência. (`app/ferias/services.py:359`, `app/ferias/services.py:362`, `app/ferias/services.py:378`, `app/ferias/services.py:402`, `app/ferias/services.py:436`.) |
| Criação normal | Insere Agendamento → espelha Ausência → atualiza PA sem reler/travar. (`app/ferias/services.py:321`, `app/ferias/models.py:437`, `app/ferias/services.py:326`.) |
| Histórico | Salva Agendamento/espelho → atualiza PA recebido. (`app/ferias/services.py:298`, `app/ferias/services.py:302`.) |
| Aprovação | Trava Solicitação → cria Agendamento/espelho → atualiza PA sem trava comum. (`app/ferias/views.py:740`, `app/ferias/services.py:494`, `app/ferias/services.py:497`.) |
| Cron | Trava Agendamento → salva/espelha; não trava PA. (`app/ferias/services.py:576`, `app/ferias/services.py:579`.) |
| Confirmação | Salva Agendamento/espelho sem transação ou trava próprias. (`app/ferias/services.py:534`, `app/ferias/services.py:542`.) |
| Demais portas | Sincronização, art.130, criação/realinhamento, edição e venda de PA não usam a trava comum; portas gerais de Ausência também não adquirem Agendamento/PA. (`app/ferias/services.py:215`, `app/ferias/services.py:243`, `app/ferias/services.py:254`, `app/ferias/services.py:514`, `app/ferias/services.py:557`; `app/ponto/services/ausencia.py:465`, `app/ponto/services/ausencia.py:808`, `app/ponto/services/ausencia.py:897`.) |

Os signals de Ausência julgam dias e agendam consumidores após commit; os trechos abertos não escrevem Agendamento/PA. (`app/ponto/signals.py:143`, `app/ponto/signals.py:150`, `app/ponto/signals.py:250`, `app/ponto/signals.py:272`.)

A diferença de ordem impede certificar o contrato global, mas **não basta para afirmar um deadlock concreto** entre janelas disjuntas. Ciclo demonstrado/reproduzido: **[nao verificado]**. O lote não mantém `atomic` externo sobre todos os itens; cada cancelamento abre sua unidade. (`app/ferias/views.py:570`, `app/ferias/views.py:575`, `app/ferias/services.py:393`.)

À pergunta **(5), cron**, a recusa de cancelamento é contada corretamente no caminho que chega à trava: o serviço devolve `vivo.status`, e o command incrementa `recusado` quando ele diverge do destino solicitado. (`app/ferias/services.py:580`; `app/ferias/management/commands/sincronizar_status_ferias.py:49`, `app/ferias/management/commands/sincronizar_status_ferias.py:50`, `app/ferias/management/commands/sincronizar_status_ferias.py:52`.)

O caminho rápido **não grava status velho**: retorna antes de qualquer escrita. Pode devolver foto obsoleta, contrariando a promessa de devolver o cancelado do banco. O command já pula igualdades antes da chamada; esses itens não entram no contador de recusas concorrentes. (`app/ferias/services.py:569`, `app/ferias/services.py:570`, `app/ferias/services.py:571`; `app/ferias/management/commands/sincronizar_status_ferias.py:39`.)

O contraexemplo restante é: cron fotografa janela passada e calcula `concluido` → admin move a janela 

[truncado em 20000 de 23372 caracteres]

## 06/11 22:04 · sessao e2c6fb6b0825 · - · - · origem=Ronald (tmux)

Achado principal: a proteção não cobre os efeitos de toda escrita de `AgendamentoFerias`. `pedir_confirmacao` e `registrar_resposta_do_colab` salvam uma foto sem reler/travar status ou datas; qualquer `save()` dispara o espelhamento, que decide se férias estão ativas pela foto recebida. Portanto, foto `agendado` → cancelamento concluído → confirmação antiga recria `Ausencia` de férias aprovada, embora o agendamento continue `cancelado` e os dias continuem devolvidos. O mesmo mecanismo pode recriar a janela antiga após remarcação. Referências: `app/ferias/services.py:534`, `app/ferias/services.py:542`, `app/ferias/models.py:434`, `app/ponto/services/ausencia.py:1179`, `app/ponto/services/ausencia.py:1198`. Execução desse cenário: [nao verificado].

As portas HTTP tornam o cenário alcançável: enviar confirmação carrega e julga a foto antes de chamar o escritor; responder carrega por confirmação `aguardando` sem filtrar status e depois salva. Referências: `app/ferias/views.py:856`, `app/ferias/views.py:860`, `app/ferias/views.py:867`, `app/ferias/views.py:901`, `app/ferias/views.py:909`, `app/ferias/views.py:921`.

Inventário dos escritores e ordem:

| Caminho | Escrita e ordem observadas |
|---|---|
| Cancelamento DP | `atomic` → trava Agendamento → trava PA → atualiza PA → salva Agendamento → espelha/remove Ausências. `app/ferias/services.py:372`, `app/ferias/services.py:359`, `app/ferias/services.py:362`, `app/ferias/services.py:374`, `app/ferias/services.py:378`, `app/ferias/models.py:437`. |
| Cancelamento admin | Mesma trava Agendamento → PA; depois PA → rejeição Ausências → Agendamento → remoção Ausências. `app/ferias/services.py:393`, `app/ferias/services.py:398`, `app/ferias/services.py:401`, `app/ferias/services.py:402`, `app/ferias/services.py:408`. |
| Remarcação/correção | `atomic` → Agendamento → PA → exclusão Ausências → Agendamento/sync → PA. `app/ferias/services.py:424`, `app/ferias/services.py:425`, `app/ferias/services.py:436`, `app/ferias/services.py:441`, `app/ferias/services.py:443`. |
| Cron | `atomic` → trava Agendamento → salva status/sync Ausências; não trava PA. `app/ferias/services.py:575`, `app/ferias/services.py:576`, `app/ferias/services.py:579`. |
| Criação normal | `atomic` → cria Agendamento/sync Ausências → incrementa PA recebido, sem reler/travar PA. `app/ferias/services.py:315`, `app/ferias/services.py:321`, `app/ferias/services.py:324`, `app/ferias/services.py:326`. |
| Gozo histórico | `atomic` → cria Agendamento/sync Ausências → atualiza PA recebido, com saturação. `app/ferias/services.py:277`, `app/ferias/services.py:298`, `app/ferias/services.py:299`, `app/ferias/services.py:302`. |
| Aprovação | A view trava Solicitação dentro de `atomic`; serviço consulta PA sem trava, cria Agendamento/sync Ausências e depois incrementa PA. `app/ferias/views.py:739`, `app/ferias/views.py:740`, `app/ferias/views.py:753`, `app/ferias/services.py:474`, `app/ferias/services.py:494`, `app/ferias/services.py:495`. |
| Confirmação | Salva Agendamento e dispara sync, sem `atomic`/trava próprios. `app/ferias/services.py:534`, `app/ferias/services.py:542`, `app/ferias/models.py:437`. |
| Exclusão de PA | `atomic` → exclui agendamentos → exclui PA; pressuposto de ausência de ativos fica no chamador. `app/ferias/services.py:526`, `app/ferias/services.py:529`, `app/ferias/services.py:530`, `app/ferias/services.py:531`. |
| Demais escritas de PA | Sincronização/art.130, criação/realinhamento, edição de datas/direito e venda não usam a trava comum. `app/ferias/services.py:215`, `app/ferias/services.py:243`, `app/ferias/services.py:254`, `app/ferias/services.py:514`, `app/ferias/services.py:557`. |
| Ausências | Espelhamento cria/remove; helpers excluem/rejeitam por janela. Portas gerais também editam dados/estado sem adquirir Agendamento/PA. `app/ponto/services/ausencia.py:1160`, `app/ponto/services/ausencia.py:1262`, `app/ponto/services/ausencia.py:1273`, `app/ponto/services/ausencia.py:465`, `app/ponto/services/ausencia.py:808`, `app/ponto/services/ausencia.py:897`. |

A ordem global declarada no helper não está satisfeita por criação, histórico e aprovação: esses caminhos escrevem Ausências antes de PA. Isso impede certificar “todo escritor na mesma ordem”; um deadlock concreto entre esses caminhos não foi demonstrado. Referências: `app/ferias/services.py:356`, `app/ferias/services.py:298`, `app/ferias/services.py:302`, `app/ferias/services.py:321`, `app/ferias/services.py:326`, `app/ferias/services.py:494`, `app/ferias/services.py:497`; ciclo reproduzido: [nao verificado].

O lote de cancelamento **não** mantém uma transação externa sobre todo o laço; cada chamada tem seu `atomic`. Portanto, não procede afirmar pelo código desse chamador o ciclo “lote retém PA após A1 e tenta A2 enquanto outro cancelamento retém A2”. Referências: `app/ferias/views.py:565`, `app/ferias/views.py:570`, `app/ferias/views.py:575`, `app/ferias/services.py:393`.

Todos os chamadores encontrados do helper executam sob `atomic`, assim como a seleção travada do cron e a aprovação da solicitação. Referências: `app/ferias/services.py:372`, `app/ferias/services.py:373`, `app/ferias/services.py:393`, `app/ferias/services.py:398`, `app/ferias/services.py:424`, `app/ferias/services.py:425`, `app/ferias/services.py:575`, `app/ferias/services.py:576`, `app/ferias/views.py:739`, `app/ferias/views.py:740`.

O saldo sem trava permanece como problema preexistente separado da dupla devolução. Contraexemplo pela UI: direito 30, concedidos 10, uma foto de PA com saldo 20; cancela AG10 → concedidos 0; criação AG20 usando a foto passa na validação antiga e grava concedidos 30, embora o correto seja 20; cancelar AG20 deixa concedidos 10 sem férias ativas. Referências: `app/ferias/views.py:273`, `app/ferias/views.py:289`, `app/ferias/views.py:299`, `app/ferias/services.py:374`, `app/ferias/services.py:324`, `app/ferias/services.py:326`. Reprodução: [nao verificado]. É compatível com a fila `FERIAS-SALDO-SEM-TRAVA` indicada na triagem fornecida; não constitui prova de novo cancelamento duplo. [nao verificado]

Os sinais de Ausência acionam julgamento do dia e consumidores após commit; não encontrei neles escrita de Agendamento/PA. Referências: `app/ponto/signals.py:143`, `app/ponto/signals.py:150`, `app/ponto/signals.py:250`, `app/ponto/signals.py:272`. O Django admin de férias registra somente `Afastamento`; `AusenciaAdmin` herda bloqueio de criação/edição/exclusão. Referências: `app/ferias/admin.py:2`, `app/ferias/admin.py:4`, `app/ponto/admin.py:37`, `app/core/admin.py:59`, `app/core/admin.py:62`, `app/core/admin.py:65`.

## 06/11 22:03 · sessao 1d1d2816dc19 · - · - · origem=Ronald (tmux)

Censo estático de leitores produtivos; execução e efeito em produção [nao verificado].

**AgendamentoFerias.status e datas**

- Autoridades: “férias hoje” registrada em `app/core/juizes.py:250` → `app/ferias/services.py:57`; saldo em `app/core/juizes.py:252`; fracionamento em `app/core/juizes.py:255`. O status por data fica em `app/ferias/models.py:273`, cancelado explicitamente fora em `app/ferias/models.py:278`.
- Consultas canônicas: janela lê datas/status em `app/ferias/services.py:51`; férias hoje em `app/ferias/services.py:63`; divergência lê status/datas em `app/ferias/services.py:77`, `:79`, `:81`; começam na janela em `app/ferias/services.py:102`.
- Modelo: `ja_iniciou`/permissões em `app/ferias/models.py:324`, `:328`, `:332`, `:339`, `:346`; fracionamento em `:353`, `:362`; veto de sobreposição lê status/data própria e data anterior em `:394`, `:400`, `:420`, `:428`; espelhamento via `:385`, acionado incondicionalmente por save em `:437`; delete lê datas em `:441`.
- Situação/avisos: frações em `app/ferias/situacao.py:117`; vivos em `:164`; aviso/vespera em `:246`, `:257`; férias com batida em `:270`, `:279`; colisões em `:343`.
- Views férias: KPI/filtros em `app/ferias/views.py:26`, `:27`, `:32`, `:72`, `:76`, `:78`, `:80`, `:82`, `:88`, `:90`; pré-validação em `:235`; guardas/auditoria admin em `:364`, `:366`, `:382`, `:406`, `:414`, `:426`, `:465`, `:483`, `:515`; massa em `:544`, `:559`, `:561`, `:565`; edição comum em `:668`; exclusão ciclo em `:836`; envio confirmação em `:860`, `:871`, `:878`; resposta colaborador em `:913`.
- Cron sincronização: consulta `app/ferias/management/commands/sincronizar_status_ferias.py:33`, cálculo pela foto `:38`, igualdade `:39`, log `:43`, recusa/contador `:50`.
- Retornos: `app/ferias/management/commands/alertar_retornos.py:20`; disponibilidade `app/colaboradores/views.py:2843`, `:2853`, e férias hoje por autoridade em `:2896`.
- API colaborador lê todas as linhas, sem filtrar cancelados: `app/api/views.py:2252`, datas `:2259`, `:2260`, status `:2262`, `:2268`.
- Mensageria: situação em `app/api/views_mensageria.py:840`; juiz em `:900`, `:901`; serialização datas/status em `:931`, `:941`, `:943`, `:1088`, `:1093`, `:1095`.
- Atendimento lê confirmação, ordena pelas datas e não exclui cancelado: `app/chamados/views.py:627`; render em `app/templates/chamados/meu_atendimento.html:5` → `app/templates/ferias/_meu_atendimento_ferias.html:6`.
- Inteligência filtra status **`aprovado`**, ausente dos choices `app/ferias/models.py:293`: `app/inteligencia/views.py:94`, `:278`, `:286`. Para agendamentos nos quatro valores declarados, esse painel não os encontra; dado legado com status extra [nao verificado]. Problema preexistente, fora do diff.
- Templates: `app/templates/ferias/_tabela_ferias.html:39`, `:40`, `:42`; `app/templates/ferias/cancelar_admin_massa.html:42`, `:43`, `:45`; `app/templates/ferias/saldo.html:59`, `:60`, `:63`, `:70`; `app/templates/ferias/_secao_colab.html:117`, `:118`, `:120`, `:124`, `:133`, `:140`, `:141`, `:143`, `:150`, `:175`, `:176`.

**dias_concedidos/saldo**

- Fonte: `app/ferias/models.py:217`; saldo de remarcação `:222`; status do ciclo lê agregado em `:95`; permissão agendar/vender em `:231`, `:239`.
- Escritores também leem agregado para contas: histórico `app/ferias/services.py:299`; criação `:324`; cancelamentos `:374`, `:399`; remarcação `:427`; aprovação `:495`. Pré-validação/preview com abono lê saldo em `:200`, `:202`, `:205`; aprovação em `:480`.
- Views: KPI/listagem `app/ferias/views.py:31`, `:60`; validação `:233`; auditoria `:368`, `:383`, `:416`, `:427`, `:486`, `:518`; remarcação `:478`, `:676`; edição do direito `:812`; venda `:942`.
- Situação: leitura canônica `app/ferias/situacao.py:69`, inconsistência `:178`, `:182`, vencidos/vencendo `:192`, `:206`, total `:296`.
- IA: seleção ciclo `app/core/views_ia.py:262`, `:269`, `:492`; default dias `:503`; “vender resto” `:532`.
- APIs: `app/api/views_mensageria.py:833`, `:838`, `:952`, `:1101`.
- Relatório PDF lê dias_concedidos no filtro `app/relatorios/services.py:290` e saldo `:316`; CSV `app/relatorios/views.py:370`.
- Commands alertas `app/ferias/management/commands/alertar_ferias_vencendo.py:36`, `:41`, corpo com saldo `:77`, `:80`, `:83`; pautas `app/ferias/management/commands/pautas_de_ferias.py:97`.
- Templates saldo: `app/templates/ferias/saldo.html:37`, `:41`, `:46`; seção colaborador `app/templates/ferias/_secao_colab.html:49`, `:50`, `:67`, `:68`, `:77`; agendar `app/templates/ferias/agendar.html:7`, `:22`; lote `app/templates/ferias/agendar_lote.html:40`; tabela `app/templates/ferias/_tabela_ferias.html:27`; relatório `app/templates/relatorios/ferias_vencendo.html:48`.

**obs / dois significados**

- Texto livre recebido por `app/ferias/views.py:287`, passado na criação em `:299` → `app/ferias/services.py:323`.
- Proveniência também entra no mesmo campo: histórico `app/ferias/services.py:284`; aprovação com solicitação e texto livre `:491`; lote `app/ferias/views.py:637`; IA `app/core/views_ia.py:688`.
- Leitor produtivo explícito encontrado do `AgendamentoFerias.obs`: concatenação do carimbo admin em `app/ferias/services.py:406`; escritor salva em `:408`. Não achei interpretação de `obs` como autoridade de status/saldo. Ausência universal de leitores por acesso dinâmico [nao verificado].
- `dias_concedidos` significa reserva e gozo juntos: criação já soma em `app/ferias/services.py:324`, histórico soma com saturação em `:299`; UI explicita “já agendados/gozados” em `app/templates/ferias/_secao_colab.html:49`. Rótulo visual “Gozo” é usado na mesma linha. Saturação é o descartado da triagem; não apresentar como descoberta nova.
- `status` mistura estágio derivado e decisão humana, mas essa separação está explicitada em `app/ferias/models.py:278`, e o leitor financeiro usa datas com exclusão explícita de cancelado em `app/ferias/services.py:51`.

**Contraexemplos e impacto**

1. **Cron ainda pode escrever status incompatível com datas vivas.** O command calcula `novo` usando datas da foto (`app/ferias/management/commands/sincronizar_status_ferias.py:38`). A porta relê a linha, mas só compara o status e escreve o `novo` recebido (`app/ferias/services.py:576`, `:577`, `:578`). Interleaving: foto antiga pede concluido; admin remarca para futuro e grava agendado (`app/ferias/services.py:428`, `:433`, `:441`); cron grava concluido nas datas futuras. DP passa a recusar (`app/ferias/services.py:373`), badges leem concluido, enquanto férias hoje/folha usam datas. Demonstração estática; reprodução concorrente [nao verificado].
2. Caminho rápido `app/ferias/services.py:570` não grava nada; pode devolver objeto com status velho, pois retorna sem query em `:571`. No command a igualdade já desvia em `app/ferias/management/commands/sincronizar_status_ferias.py:39`, portanto esse caso não entra em contagem de recusa.
3. Foto velha de confirmação também passa por `save(update_fields=...)` (`app/ferias/services.py:538`, `:547`, `:551`), e o modelo chama sync sempre (`app/ferias/models.py:437`). Encaminhei ao explorer de escritores a hipótese de recriar Ausencias de cancelado; conclusão depende da leitura de `espelhar_ferias`.
4. **Folha:** não lê agregado diretamente no cruzamento; chama janela canônica em `app/folha/export.py:491`, usa datas em `:493`; retém férias com batida em `:783`, `:875`. Assim cancelamento efetivo remove a janela desse bloqueio por `app/ferias/services.py:51`. Remover/recriar Ausencia pode atingir apuração por caminhos diferentes; análise do outro explorer.
5. Holerite: não encontrei referência produtiva a férias/dias_saldo no módulo `app/holerite`; dependência indireta de valores importados [nao verificado].
6. Relógio segue no miolo: carimbo admin `app/ferias/services.py:405`; remarcação aviso `:429` e status sem `hoje` em `:433` → `app/ferias/models.py:280`; permissões `app/ferias/models.py:324`; status ciclo sem parâmetro via `app/ferias/models.py:212` → `:94`. O command usa borda única `timezone.localdate` em `app/ferias/management/commands/sincronizar_status_ferias.py:27`. Diferença de fuso/data na execução [nao verificado].
7. Django admin férias só registra Afastamento em `app/ferias/admin.py:4`; não há ModelAdmin de AgendamentoFerias/PeriodoAquisitivo nesse arquivo. Registro dinâmico externo não encontrado [nao verificado].

## 06/11 22:03 · sessao 0411dcb88457 · - · - · origem=Ronald (tmux)

Chamadores das exceções estão tratados nas portas encontradas: cancelar DP em `app/ferias/views.py:330`/`:331`; admin comum `:370`/`:371`; concluída `:418`/`:419`; massa `:575`/`:576`; corrigir datas `:503`/`:504`; remarcar `:688`/`:689`. O wrapper apenas propaga (`app/ferias/views.py:527`). A massa conta recusas como pulados e só incrementa feitos após sucesso (`app/ferias/views.py:576`, `:579`).

O `except Exception` de edição administrativa cobre somente a consulta de avisos, anterior ao escritor (`app/ferias/views.py:494`, `:499`, `:501`). O genérico de aprovação cobre criação própria, sem chamar os escritores das novas exceções (`app/ferias/services.py:483`, `:498`). Django admin registra apenas `Afastamento` neste módulo (`app/ferias/admin.py:4`); a API encontrada apenas lê agendamentos (`app/api/views.py:2251`, `:2257`). Não encontrei chamadores nominais adicionais desses serviços na cópia; completude diante de chamadas dinâmicas: [nao verificado].

O cron conta corretamente a recusa por cancelamento no caminho que efetivamente trava: o serviço devolve `vivo.status`, e o command incrementa `recusado` quando difere do solicitado (`app/ferias/services.py:576`, `:580`; `app/ferias/management/commands/sincronizar_status_ferias.py:49`, `:50`, `:52`). O caminho rápido não grava status velho, porque retorna imediatamente; pode devolver uma fotografia obsoleta e contrariar a promessa de devolver o cancelado do banco (`app/ferias/services.py:569`, `:570`). O command já pula fotografias com status igual ao calculado, portanto também não conta cancelamentos concorrentes nesses itens (`app/ferias/management/commands/sincronizar_status_ferias.py:38`, `:39`).

Há outra corrida no cron: `novo` deriva das datas da fotografia, mas a gravação posterior trava apenas para conferir status, sem recalcular pelas datas atuais (`app/ferias/management/commands/sincronizar_status_ferias.py:38`; `app/ferias/services.py:576`, `:577`, `:578`). Contraexemplo estático: cron calcula `em_gozo`; edição administrativa move a janela ao futuro e grava `agendado`; cron obtém a trava depois e grava o antigo `em_gozo` (`app/ferias/services.py:425`, `:428`, `:433`, `:441`, `:578`). Reprodução executada: [nao verificado].

Há 15 testes no arquivo, definidos nas linhas abaixo; previsão contra pai/base vem dos patches fornecidos, com execução e commits históricos [nao verificado]. [nao verificado]

| Teste, em `app/ferias/test_cancelar_2x.py` | Contra pai v1 | Contra base |
|---|---|---|
| `:83`, mesmo objeto | Verde | RED: −30 em vez de 0 | [nao verificado]
| `:90`, objeto relido | Verde | RED: −30 | [nao verificado]
| `:97`, segunda sem escrita | Verde | RED: −30 | [nao verificado]
| `:104`, duas fotografias | Verde | RED: −30 | [nao verificado]
| `:112`, concluído DP | Verde | RED: 0/cancelado em vez de 30/concluído | [nao verificado]
| `:120`, admin duas vezes | Verde | RED: −30 | [nao verificado]
| `:128`, admin comum concluído | RED: 0 em vez de 30 | Mesmo RED |
| `:137`, massa concluído | RED: 0 em vez de 30 | Mesmo RED |
| `:144`, porta concluída | **ERROR por TypeError** | **Mesmo ERROR** |
| `:154`, cron fotografia velha | RED: em_gozo em vez de cancelado | Mesmo RED |
| `:164`, cron normal | Verde | Verde | [nao verificado]
| `:182`, remarcar cancelado | RED: −20 em vez de 0 | Mesmo RED |
| `:185`, corrigir cancelado | RED: −20 em vez de 0 | Mesmo RED |
| `:188`, saldo da fotografia | RED: 979 em vez de 10 | Mesmo RED |
| `:202`, POST duas vezes | Verde | RED: −30 | [nao verificado]

A porta concluída usa o parâmetro novo na primeira chamada, fora de `_tentar`; pai/base fornecidos não possuem esse parâmetro. Assim ela falha antes dos asserts de estado, contradizendo a afirmação universal de mordida por valor (`app/ferias/test_cancelar_2x.py:20`, `:21`, `:146`, `:147`). As demais recusas verificam estado antes de procurar a classe, evitando os antigos REDs por símbolo ausente (`app/ferias/test_cancelar_2x.py:68`, `:73`, `:77`, `:78`, `:87`, `:88`). “Sem escrita” comprova igualdade dos campos consultados, sem instrumentar a inexistência de SQL de escrita (`app/ferias/test_cancelar_2x.py:59`, `:62`, `:99`, `:101`).

Outro contraexemplo fora do golden: uma fotografia ativa usada para confirmar pode recriar o espelho depois do cancelamento. A confirmação salva somente seus campos, mas todo `save` executa sincronização, que decide atividade e datas pela fotografia recebida (`app/ferias/services.py:534`, `:538`, `:542`, `:547`, `:551`; `app/ferias/models.py:434`, `:437`; `app/ponto/services/ausencia.py:1179`, `:1180`, `:1184`, `:1202`). Sequência: carregar para confirmar → cancelar e apagar ausências → salvar confirmação com status ativo antigo → recriar ausências aprovadas; a coluna `status` permanece cancelada, mas o espelho revive (`app/ferias/views.py:856`, `:867`, `:901`, `:921`; `app/ponto/services/ausencia.py:1188`, `:1190`, `:1204`). Reprodução executada: [nao verificado].

## 06/11 22:01 · sessao 4fed66c7fd38 · V2-BUGS-I2 · sonda · origem=Code

**Sim: duplica a interseção entre 22h e a saída.** O `s1` é estendido até `fim`; depois, o laço soma também o `s2`, iniciado às 22h. Para saída às 23h, duplica 22–23; não necessariamente 22–24 inteiro. [`app/ponto/motor_calculo_v2.py:110`](/home/ronald/codex_ws/e962c45be5a097949880ae66b3a3d1a54fe2b858/app/ponto/motor_calculo_v2.py:110), [`app/ponto/motor_calculo_v2.py:112`](/home/ronald/codex_ws/e962c45be5a097949880ae66b3a3d1a54fe2b858/app/ponto/motor_calculo_v2.py:112)

Na `he_noturna`, essa duplicação depende de **`inicio_extra`** também cair antes das 05h: a função primeiro recua pelo total de HE e só então mede o noturno. Portanto, um período candidato pode ter excesso de noturno e diferença **zero na HE**. [`app/ponto/motor_calculo_v2.py:440`](/home/ronald/codex_ws/e962c45be5a097949880ae66b3a3d1a54fe2b858/app/ponto/motor_calculo_v2.py:440)

Contraexemplo próprio, deduzido do código: **01/10/2026, 04:00–23:00**, sem intervalos, prorrogação ligada, `extra_50_min=1140`, `extra_100_min=0`. O recuo encontra 04h; `s1` conta **1.140 min**, `s2` acrescenta **60 min**: `he_noturna` devolve **(1200, 0)**. Limitando somente `s1` às 22h, devolve **(1140, 0)**: excesso de **60 min**. Dedução, sem execução. [`app/ponto/motor_calculo_v2.py:392`](/home/ronald/codex_ws/e962c45be5a097949880ae66b3a3d1a54fe2b858/app/ponto/motor_calculo_v2.py:392), [`app/ponto/motor_calculo_v2.py:110`](/home/ronald/codex_ws/e962c45be5a097949880ae66b3a3d1a54fe2b858/app/ponto/motor_calculo_v2.py:110), [`app/ponto/motor_calculo_v2.py:448`](/home/ronald/codex_ws/e962c45be5a097949880ae66b3a3d1a54fe2b858/app/ponto/motor_calculo_v2.py:448)

O script abaixo intercepta a **chamada real** do fechamento a `apurar_por_fatia`, preserva os motores de todas as fatias e interrompe após medir `resultado.periodos`. Essa porta compõe os mesmos objetos; `_ap.motor`, isoladamente, representa apenas a última fatia. A lavratura fica posteriormente à interceptação. [`app/ponto/services/fechamento.py:754`](/home/ronald/codex_ws/e962c45be5a097949880ae66b3a3d1a54fe2b858/app/ponto/services/fechamento.py:754), [`app/ponto/services/fechamento.py:294`](/home/ronald/codex_ws/e962c45be5a097949880ae66b3a3d1a54fe2b858/app/ponto/services/fechamento.py:294), [`app/ponto/services/fechamento.py:317`](/home/ronald/codex_ws/e962c45be5a097949880ae66b3a3d1a54fe2b858/app/ponto/services/fechamento.py:317), [`app/ponto/services/fechamento.py:1190`](/home/ronald/codex_ws/e962c45be5a097949880ae66b3a3d1a54fe2b858/app/ponto/services/fechamento.py:1190)

**É medida, não cura:** mantém períodos, HE, intervalos e decisões originais; cria em memória uma variante da função existente alterando exclusivamente `s1_fim_efetivo = fim` para `min(fim, s2_ini)`. Chama a `he_noturna` real duas vezes por candidato e também compara o noturno de relógio nas pontas efetivas. Script completo proposto, sem execução; totais da frota **[não verificado]**.

```python
# v2_bugs_i2_sonda.py
# Proposta nao executada [nao verificado].
# Medida, nao cura. Nenhum arquivo e escrito por este Python.

import ast
import inspect
import io
import json
import logging
import math
import textwrap
from contextlib import ExitStack, redirect_stdout, redirect_stderr
from datetime import date, time
from unittest.mock import patch


class Falha(Exception):
    pass


class EncerrarComRollback(Exception):
    def __init__(self, dados):
        super().__init__("rollback_sonda")
        self.dados = dados


class Silencio(io.TextIOBase):
    def write(self, texto):
        return len(texto)

    def flush(self):
        pass


JANELAS = (
    (10, date(2026, 9, 21), date(2026, 10, 20)),
    (11, date(2026, 10, 21), date(2026, 11, 20)),
)

CAMPOS = (
    "periodos_condicao",
    "periodos_excesso_noturno",
    "periodos_excesso_he",
    "min_noturno_relogio_a_mais",
    "min_he50_noturna_a_mais",
    "min_he100_noturna_a_mais",
    "min_he_noturna_a_mais",
)
EPS = 1e-7


def vazio():
    return {campo: 0 for campo in CAMPOS}


def somar(destino, origem):
    for campo in CAMPOS:
        destino[campo] += origem[campo]


def pacote():
    return {
        "todos": vazio(),
        "fora_feriado_dobra": vazio(),
    }


def somar_pacote(destino, origem):
    for grupo in destino:
        somar(destino[grupo], origem[grupo])


def diferenca(antes, depois):
    antes, depois = float(antes), float(depois)
    if not (math.isfinite(antes) and math.isfinite(depois)):
        raise Falha("medida_nao_finita")
    valor = antes - depois
    if valor < -EPS:
        raise Falha("diferenca_negativa")
    return max(0.0, valor)


def variante_com_s1_limitado(modulo):
    # Fonte real: motor_calculo_v2.py:82-118; unica alteracao em :111.
    arvore = ast.parse(textwrap.dedent(
        inspect.getsource(modulo.minutos_noturnos)
    ))
    alvos = [
        node
        for node in ast.walk(arvore)
        if isinstance(node, ast.Assign)
        and len(node.targets) == 1
        and isinstance(node.targets[0], ast.Name)
        and node.targets[0].id == "s1_fim_efetivo"
        and isinstance(node.value, ast.Name)
        and node.value.id == "fim"
    ]
    if len(alvos) != 1:
        raise Falha("fonte_incompativel")

    alvo = alvos[0]
    alvo.value = ast.copy_location(
        ast.Call(
            func=ast.Name(id="min", ctx=ast.Load()),
            args=[
                alvo.value,
                ast.Name(id="s2_ini", ctx=ast.Load()),
            ],
            keywords=[],
        ),
        alvo.value,
    )
    ast.fix_missing_locations(arvore)
    ambiente = dict(vars(modulo))
    exec(compile(arvore, "<sonda_s1_limitado>", "exec"), ambiente)
    return ambiente["minutos_noturnos"]


def medir_resultado(ap, donos, ini, fim, mod, limitado):
    resultado = ap.resultado
    totais = pacote()
    dias = {}

    for periodo in resultado.periodos:
        motor = donos[id(periodo)]  # Ausencia do dono e erro, nunca fallback.
        if periodo.saida is None:
            continue

        entrada = mod.localizar(periodo.entrada)
        saida = mod.localizar(periodo.saida)
        prorrogacao = motor.prorrogacao_noturna_pos5h

        candidato = (
            bool(prorrogacao)
            and entrada.date() == saida.date()
            and entrada.time() < time(5)
            and saida.time() > time(22)
        )
        if not candidato:
            continue

        jornada = resultado.dia_da_jornada[id(periodo)]
        if not ini <= jornada <= fim:
            raise Falha("jornada_fora_da_janela")

        # Regra real, no motor desta fatia, com o dia julgado da jornada.
        dobra = motor._feriado_dobra_no_dia(jornada)

        argumentos_he = {
            "entrada": periodo.entrada,
            "saida": periodo.saida,
            "extra_50_min": periodo.minutos_extra_50,
            "extra_100_min": periodo.minutos_extra_100,
            "prorrogacao": prorrogacao,
            "intervalos": periodo.intervalos_do_motor,
        }
        efetivo_entrada = periodo.efetivo_entrada
        efetivo_saida = periodo.efetivo_saida
        if efetivo_entrada is None or efetivo_saida is None:
            raise Falha("segmento_efetivo_ausente")

        # Primeira chamada: he_noturna REAL, com os dados originais.
        he_antes = mod.he_noturna(**argumentos_he)
        noturno_antes = motor.calcular_noturnas(
            efetivo_entrada,
            efetivo_saida,
            periodo.intervalos_do_motor,
        )[0]

        if not math.isclose(
            noturno_antes,
            periodo.minutos_noturnos_relogio,
            rel_tol=1e-9,
            abs_tol=EPS,
        ):
            raise Falha("releitura_noturno_divergente")

        # Somente durante a medida; nao recalcula a apuracao.
        # A prorrogacao permanece a do periodo, inclusive nos intervalos.
        with patch.object(mod, "minutos_noturnos", limitado):
            # Segunda chamada: a MESMA he_noturna REAL.
            he_depois = mod.he_noturna(**argumentos_he)
            noturno_depois = motor.calcular_noturnas(
                efetivo_entrada,
                efetivo_saida,
                periodo.intervalos_do_motor,
            )[0]

        d50 = diferenca(he_antes[0], he_depois[0])
        d100 = diferenca(he_antes[1], he_depois[1])
        dnot = diferenca(noturno_antes, noturno_depois)
        dhe = d50 + d100

        medida = {
            "periodos_condicao": 1,
            "periodos_excesso_noturno": int(dnot > EPS),
            "periodos_excesso_he": int(dhe > EPS),
            "min_noturno_relogio_a_mais": dnot,
            "min_he50_noturna_a_mais": d50,
            "min_he100_noturna_a_mais": d100,
            "min_he_noturna_a_mais": dhe,
        }

        chave = (entrada.date().isoformat(), jornada.isoformat())
        por_dia = dias.setdefault(chave, pacote())
        for destino in (totais, por_dia):
            somar(destino["todos"], medida)
            if not dobra:
                somar(destino["fora_feriado_dobra"], medida)

    return {
        "periodos_produzidos": len(resultado.periodos),
        "totais": totais,
        "dias": dias,
    }


def medir_colaborador(cid, mes, ini, fim, fechamento, mod, limitado):
    from django.db import transaction

    original_apurar = fechamento.apurar_por_fatia
    donos = {}

    def observar_porta(colab, inicio, final, batidas,
                       escalas, celulas, montar_motor):
        if colab.pk != cid:
            raise Falha("colaborador_inesperado")
        if (inicio, final) != (ini, fim):
            raise Falha("corte_da_empresa_divergente")

        with ExitStack() as pilha:
            def observar_montagem(escala, fatia_ini, fatia_fim):
                motor = montar_motor(escala, fatia_ini, fatia_fim)
                calcular_real = motor.calcular_mes

                def observar_mes(*args, **kwargs):
                    parcial = calcular_real(*args, **kwargs)
                    for periodo in parcial.periodos:
                        anterior = donos.get(id(periodo))
                        if anterior is not None and anterior is not motor:
                            raise Falha("periodo_com_dois_motores")
                        donos[id(periodo)] = motor
                    return parcial

                pilha.enter_context(
                    patch.object(motor, "calcular_mes", observar_mes)
                )
                return motor

            ap = original_apurar(
                colab, inicio, final, batidas,
                escalas, celulas, observar_montagem,
            )

        dados = medir_resultado(
            ap, donos, ini, fim, mod, limitado,
        )
        # Sai antes do downstream/lavratura; rollback do atomic externo.
        raise EncerrarComRollback(dados)

    try:
        with transaction.atomic():
            with patch.object(
                fechamento, "apurar_por_fatia", observar_porta
            ):
                fechamento.recalcular_fechamento_mes(
                    mes=mes,
                    ano=2026,
                    colaborador_ids=[cid],
                    somente_leitura=True,
                    lavrar_dias_pagos=False,
                )
            raise Falha("porta_nao_alcancada")
    except EncerrarComRollback as encerramento:
        return encerramento.dados


def executar():
    from django.db import connection
    import ponto.motor_calculo_v2 as mod
    import ponto.services.fechamento as fechamento

    with connection.cursor() as cursor:
        cursor.execute("SELECT current_database(), current_schema()")
        if tuple(cursor.fetchone()) != ("sombra", "juliani"):
            raise Falha("ambiente_incorreto")

    limitado = variante_com_s1_limitado(mod)

    # Primeiro materializa os dois universos pela autoridade do fechamento.
    universos = [
        (
            mes, ini, fim,
            list(
                fechamento.universo_da_competencia(mes, 2026)
                .order_by("pk")
                .values_list("pk", flat=True)
            ),
        )
        for mes, ini, fim in JANELAS
    ]

    saidas = []
    soma_global = pacote()
    tentativas_global = 0
    validos_global = 0
    erros_global = 0
    ids_tentados = set()

    for mes, ini, fim, ids in universos:
        competencia = "2026-%02d" % mes
        soma_competencia = pacote()
        validos = 0
        erros = 0

        for cid in ids:
            identificador = "col%03d" % int(cid)
            ids_tentados.add(cid)
            try:
                dados = medir_colaborador(
                    cid, mes, ini, fim, fechamento, mod, limitado,
                )
            except Exception as erro:
                erros += 1
                # Mensagens e tracebacks podem conter dados pessoais.
                motivo = (
                    str(erro) if isinstance(erro, Falha)
                    else "erro_apuracao_ou_medida"
                )
                saidas.append({
                    "tipo": "colaborador",
                    "competencia": competencia,
                    "col": identificador,
                    "status": "nao_sei",
                    "contador_nao_sei": 1,
                    "motivo": motivo,
                    "soma": None,
                })
                continue

            validos += 1
            somar_pacote(soma_competencia, dados["totais"])
            saidas.append({
                "tipo": "colaborador",
                "competencia": competencia,
                "col": identificador,
                "status": "valido",
                "periodos_produzidos": dados["periodos_produzidos"],
                "soma": dados["totais"],
            })
            for (civil, jornada), valores in sorted(dados["dias"].items()):
                saidas.append({
                    "tipo": "dia",
                    "competencia": competencia,
                    "col": identificador,
                    "dia_civil": civil,
                    "dia_jornada": jornada,
                    "soma": valores,
                })

        if validos + erros != len(ids):
            raise Falha("universo_inconsistente")

        saidas.append({
            "tipo": "universo",
            "competencia": competencia,
            "inicio": ini.isoformat(),
            "fim": fim.isoformat(),
            "colabs_tentados": len(ids),
            "colabs_validos": validos,
            "colabs_com_erro": erros,
            "contador_nao_sei": erros,
            "status": "completo" if erros == 0 else "nao_sei",
            "soma_conhecida": soma_competencia,
            "soma": soma_competencia if erros == 0 else None,
        })

        tentativas_global += len(ids)
        validos_global += validos
        erros_global += erros
        somar_pacote(soma_global, soma_competencia)

    saidas.append({
        "tipo": "soma_ambas_competencias",
        "unidade_universo": "colaborador_competencia",
        "tentados": tentativas_global,
        "validos": validos_global,
        "com_erro": erros_global,
        "colabs_distintos_tentados": len(ids_tentados),
        "contador_nao_sei": erros_global,
        "status": "completo" if erros_global == 0 else "nao_sei",
        "soma_conhecida": soma_global,
        "soma": soma_global if erros_global == 0 else None,
        "natureza": "medida_nao_cura",
    })
    return saidas


def arredondar(valor):
    if isinstance(valor, dict):
        return {k: arredondar(v) for k, v in valor.items()}
    if isinstance(valor, list):
        return [arredondar(v) for v in valor]
    if isinstance(valor, float):
        return round(valor, 9)
    return valor


nivel_anterior = logging.root.manager.disable
try:
    logging.disable(logging.CRITICAL)
    with redirect_stdout(Silencio()), redirect_stderr(Silencio()):
        try:
            registros = executar()
        except Exception:
            registros = [{
                "tipo": "fatal",
                "status": "nao_sei",
                "motivo": "preparacao_ou_universo",
                "colabs_tentados": None,
                "colabs_validos": None,
                "colabs_com_erro": None,
                "soma": None,
            }]
finally:
    logging.disable(nivel_anterior)

for registro in registros:
    print(json.dumps(
        arredondar(registro),
        ensure_ascii=True,
        sort_keys=True,
        allow_nan=False,
    ))
```

No desenho, `todos.periodos_condicao` responde (1), e `fora_feriado_dobra.periodos_condicao` responde (2); as respectivas somas mostram excesso de relógio e de HE separadamente. Os candidatos com diferença zero continuam contados. Erro invalida o colaborador inteiro; a soma integral fica `null`, acompanhada da soma conhecida. Comportamento proposto **[não verificado]**.

A identificação de feriado-dobra chama `_feriado_dobra_no_dia` com a jornada julgada e o motor da fatia. Usar apenas `minutos_extra_100_feriado` seria insuficiente: comercial/partido podem concentrar a dobra diária no último período. `app/ponto/motor_calculo_v2.py:934`, `app/ponto/motor_calculo_v2.py:3092`, `app/ponto/motor_calculo_v2.py:3342`.

Os campos de aplicação acessados diretamente e as dependências delegadas aos juízes têm esta proveniência:

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `Colaborador.pk` | Porta de criação: `app/colaboradores/services/colaborador.py:91`; geração automática do identificador **[não verificado]** | Universo e apuração: `app/ponto/services/fechamento.py:439`, `app/ponto/services/fechamento.py:290` |
| `situacao`, `data_demissao` — universo | `app/colaboradores/services/desligamento.py:104`; persistência: `app/colaboradores/services/colaborador.py:73` | `app/ponto/services/fechamento.py:439` |
| `empresa_id` — universo/janela | Edição: `app/colaboradores/views.py:1313`; transferência: `app/colaboradores/views.py:1386` | `app/ponto/services/fechamento.py:437`, `app/ponto/services/fechamento.py:621` |
| `Empresa.dia_inicio_competencia` — janela | `app/colaboradores/views.py:523`; persistência: `app/colaboradores/services/estrutura.py:17` | `app/ponto/janelas.py:50`; fechamento: `app/ponto/services/fechamento.py:621` |
| `_Apuracao.resultado`, `resultado.periodos` | Retorno: `app/ponto/services/fechamento.py:318`; composição: `app/ponto/services/fechamento.py:294`; motor: `app/ponto/motor_calculo_v2.py:2494` | `app/ponto/services/fechamento.py:756`, `app/ponto/services/fechamento.py:1202` |
| `periodo.entrada`, `saida` | Construtores: `app/ponto/motor_calculo_v2.py:2751`, `app/ponto/motor_calculo_v2.py:2878`, `app/ponto/motor_calculo_v2.py:3208` | HE: `app/ponto/motor_calculo_v2.py:1015` |
| `efetivo_entrada`, `efetivo_saida`, `intervalos_do_motor` | Carimbo: `app/ponto/motor_calculo_v2.py:1840` | Alimentação do calculador: `app/ponto/calculador/alimentacao.py:5`; intervalos da HE: `app/ponto/motor_calculo_v2.py:1018` |
| `minutos_extra_50`, `minutos_extra_100` | 12x36: `app/ponto/motor_calculo_v2.py:2851`; partido: `app/ponto/motor_calculo_v2.py:3147`; comercial: `app/ponto/motor_calculo_v2.py:3404`; dobra: `app/ponto/motor_calculo_v2.py:3094`, `app/ponto/motor_calculo_v2.py:3345` | `app/ponto/motor_calculo_v2.py:1016` |
| `minutos_noturnos_relogio` | Escritor único: `app/ponto/motor_calculo_v2.py:1004` | Soma do resultado: `app/ponto/motor_calculo_v2.py:269`; localização exata do leitor agregado **[não verificado]** |
| `resultado.dia_da_jornada` | Motor: `app/ponto/motor_calculo_v2.py:2460`, `app/ponto/motor_calculo_v2.py:1075`; composição: `app/ponto/services/fechamento.py:311` | `app/ponto/calculador/periodo.py:596`, `app/ponto/calculador/periodo.py:611` |
| `motor.prorrogacao_noturna_pos5h` | Fábrica CCT: `app/core/regua_cct.py:454`, `app/core/regua_cct.py:466`; armazenamento: `app/ponto/motor_calculo_v2.py:762` | Noturno: `app/ponto/motor_calculo_v2.py:984`; HE: `app/ponto/motor_calculo_v2.py:1017` |
| `feriado_em_dobra`, `feriados`, `_feriados_injetados` — juiz delegado | `app/ponto/motor_calculo_v2.py:787`, `app/ponto/motor_calculo_v2.py:790`, `app/p

[truncado em 20000 de 20761 caracteres]

## 06/11 21:57 · sessao 0b0cf1ff4167 · - · - · origem=Ronald (tmux)

A porta fiel é chamar `recalcular_fechamento_mes(..., colaborador_ids=[pk], somente_leitura=True, lavrar_dias_pagos=False)` e interceptar sua chamada real a `apurar_por_fatia`, que entrega `resultado` em `app/ponto/services/fechamento.py:754-756`; assinatura em `app/ponto/services/fechamento.py:448-451`.

Desenho recomendado: patch temporário de `fechamento.apurar_por_fatia`; wrapper chama a função original com seus argumentos originais, captura o `_Apuracao.resultado` e levanta sentinela imediatamente. Envolver a chamada ao recalculador em `transaction.atomic`, capturando a sentinela fora do `with` é proposta de script, ainda não executada. [não verificado]

Para preservar o motor de cada período, envolver também o `montar_motor` recebido: ele cria a instância efetivamente usada pela fatia em `app/ponto/services/fechamento.py:279`; envolver seu `calcular_mes` e registrar `{id(p): motor}` dos períodos do resultado retornado. A chamada efetiva é `app/ponto/services/fechamento.py:286-291`. A composição preserva os objetos dos períodos em `app/ponto/services/fechamento.py:294-297`, explicitado em `app/ponto/services/fechamento.py:306-307`.

Leituras e escritores:

| Campo | Escritor | Leitor real |
|---|---|---|
| `resultado.periodos` | motor encaminha período em `app/ponto/motor_calculo_v2.py:2494`; composição em `app/ponto/services/fechamento.py:294-297` | fechamento em `app/ponto/services/fechamento.py:1202` |
| `entrada`, `saida` | construtores dos três motores em `app/ponto/motor_calculo_v2.py:2751`, `:2878`, `:3208` | `_aplicar_he_noturna` em `app/ponto/motor_calculo_v2.py:1015` |
| `minutos_extra_50`, `minutos_extra_100` | 12x36: `app/ponto/motor_calculo_v2.py:2811`, `:2851-2854`; partido: `:3094`, `:3147-3150`, `:3179`; comercial: `:3345`, `:3404-3407` | `_aplicar_he_noturna` em `app/ponto/motor_calculo_v2.py:1016` |
| `intervalos_do_motor` | `_carimbar_segmento` em `app/ponto/motor_calculo_v2.py:1842` | `_aplicar_he_noturna` em `app/ponto/motor_calculo_v2.py:1018` |
| `prorrogacao_noturna_pos5h` | inicialização em `app/ponto/motor_calculo_v2.py:758-762` | `_aplicar_he_noturna` em `app/ponto/motor_calculo_v2.py:1017` |
| universo de colaboradores | query canônica em `app/ponto/services/fechamento.py:439-445` | recalculador em `app/ponto/services/fechamento.py:554` |

As pontas usadas por `_aplicar_he_noturna` são `periodo.entrada/saida`, conforme `app/ponto/motor_calculo_v2.py:1015`; os campos `efetivo_entrada/saida` são escritos separadamente em `app/ponto/motor_calculo_v2.py:1840-1841`.

A classificação “fora de feriado-dobra” pode chamar o motor real da fatia: `motor._feriado_dobra_no_dia(dia)`, cuja resposta é `bool(self.feriado_em_dobra) and self.eh_feriado(dia)` em `app/ponto/motor_calculo_v2.py:934`. O contrato informa que o dia recebido é o início do plantão em `app/ponto/motor_calculo_v2.py:928-930`.

A primeira lavratura fica depois da porta interceptada e tem guarda `(not somente_leitura) or lavrar_dias_pagos`, em `app/ponto/services/fechamento.py:1190`; chamadas de `lavrar` em `app/ponto/services/fechamento.py:1200` e `app/ponto/services/fechamento.py:1276`.

## 06/11 21:57 · sessao 1157629651c1 · - · - · origem=Ronald (tmux)

Para classificar “fora de dia de feriado-dobra”, use o **motor que efetivamente calculou aquela fatia** e o **dia da jornada do período**:

```python
dia = resultado.dia_da_jornada[id(periodo)]
feriado_dobra = motor_da_fatia._feriado_dobra_no_dia(dia)
```

A regra chamada é `bool(self.feriado_em_dobra) and self.eh_feriado(dia)`; o próprio método estabelece que `dia` pertence ao início do plantão, incluindo suas caudas. [app/ponto/motor_calculo_v2.py:914-934]

Não use `_ap.motor` indiscriminadamente: esse atributo representa somente a **última fatia que rodou**, enquanto `resultado` reúne todas as fatias. [app/ponto/services/fechamento.py:171-174; app/ponto/services/fechamento.py:317-320]

Não use apenas `periodo.minutos_extra_100_feriado > 0`: no comercial/partido, o valor de **todo o dia** fica em `periodos_dia[-1]`, e os outros períodos daquele mesmo dia podem carregar zero. [app/ponto/motor_calculo_v2.py:3081-3095; app/ponto/motor_calculo_v2.py:3334-3346]

Proveniência dos campos:

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `resultado.dia_da_jornada[id(p)]` | Motor atribui a jornada em `data_jornada`, grava o mapa do período e o expõe no resultado; a composição mescla mapas. [app/ponto/motor_calculo_v2.py:2403-2408; app/ponto/motor_calculo_v2.py:2460; app/ponto/motor_calculo_v2.py:1073-1075; app/ponto/services/fechamento.py:311-316] | Releitura agrupa períodos pelo mapa. [app/ponto/calculador/periodo.py:596-615] |
| `motor.feriado_em_dobra` | `get_motor` resolve pelo ciclo quando não recebeu override, passa ao construtor e este armazena; fábrica CCT pode habilitar override para `12x36`. [app/ponto/motor_calculo_v2.py:3679-3690; app/ponto/motor_calculo_v2.py:787; app/core/regua_cct.py:473-479] | `_feriado_dobra_no_dia`. [app/ponto/motor_calculo_v2.py:934] |
| `motor.feriados` / `_feriados_injetados` | Fechamento obtém feriados do colaborador e os injeta na fábrica do motor; construtor guarda conjunto e sua identidade. [app/ponto/services/fechamento.py:647-648; app/ponto/services/fechamento.py:735-741; app/ponto/motor_calculo_v2.py:790-795] | `eh_feriado` consulta conjunto; quando conjunto injetado responde, ausência retorna `False`, sem fallback. [app/ponto/motor_calculo_v2.py:956-967] |
| `minutos_extra_100_feriado` | Motor escreve dobra de todo dia no último período; ramo 12x36 também escreve diretamente no período. [app/ponto/motor_calculo_v2.py:3092-3095; app/ponto/motor_calculo_v2.py:3342-3346; app/ponto/motor_calculo_v2.py:2809-2813] | Fechamento lê a propriedade agregada `resultado.horas_extra_100_feriado`. [app/ponto/services/fechamento.py:923] |
| `prorrogacao_noturna_pos5h` | Fábrica CCT define segundo piso/regra cadastrada; construtor armazena. [app/core/regua_cct.py:449-466; app/ponto/motor_calculo_v2.py:758-762] | `_aplicar_he_noturna` passa o atributo à função real. [app/ponto/motor_calculo_v2.py:1014-1018] |
| `intervalos_do_motor` | `_carimbar_segmento` copia os intervalos efetivamente considerados. [app/ponto/motor_calculo_v2.py:1833-1842] | `_aplicar_he_noturna` entrega-os à função real. [app/ponto/motor_calculo_v2.py:1014-1018] |

Uma captura por fatia pode envolver a instância retornada pelo `_montar_motor` real e interceptar seu `calcular_mes`: a porta compartilhada chama `montar_motor(...)` e, imediatamente depois, `motor.calcular_mes(...)`, antes de compor os mesmos objetos em `resultado.periodos`. [app/ponto/services/fechamento.py:279-297]

## 06/11 21:57 · sessao 78efd8081cd5 · - · - · origem=Ronald (tmux)

O universo correto é `universo_da_competencia(mes, ano)`, sem filtrar `situacao='ativo'`: inclui todo não desligado e desligado cuja demissão alcance o início da janela da própria empresa; sem `empresa_id`, une as empresas. Use `.order_by('pk')` para evitar a ordenação por nome da função. (`app/ponto/services/fechamento.py:382`, `:431-445`)

Campos envolvidos:

| Campo | Escritor | Leitor |
|---|---|---|
| `Colaborador.situacao`, `data_demissao` | `app/colaboradores/services/desligamento.py:104-108`; persistência em `app/colaboradores/services/colaborador.py:73-76` | `app/ponto/services/fechamento.py:432-442` |
| `Colaborador.empresa_id` | edição `app/colaboradores/views.py:1313`, persistência `:1341-1342`; transferência `:1385-1388` | `app/ponto/services/fechamento.py:437-438` |
| `Empresa.dia_inicio_competencia` | UI `app/colaboradores/views.py:496-499`, `:520-523`, `:531-538`; persistência `app/colaboradores/services/estrutura.py:12-20` | `app/ponto/janelas.py:36-57`; universo em `app/ponto/services/fechamento.py:436-438` |

As janelas solicitadas só coincidem com a janela real quando o corte efetivo é 21; valide `janela_fechamento` por colaborador e conte divergência como “não sei” em vez de apurar janela diferente silenciosamente. (`app/ponto/janelas.py:10-21`, `:36-57`)

`--rodar` exige o arquivo em `logs/sombra`, monta essa pasta como `/sombra` e chama `tenant_command shell --schema=juliani`. (`bin/sombra.sh:189-190`, `:483-491`)

O wrapper usa settings de sombra e proxy morto, com banco e host literais `sombra` / `sombra_db`. (`bin/sombra.sh:184-188`; `app/config/settings/sombra.py:29-50`)

Há um detalhe material: **antes do Python da sonda**, `--rodar` chama `lavra_de_prod`; ela lê o snapshot em produção e grava snapshots na sombra. Essas ações ficam fora da transação do script. Logo, o script Python pode ser apenas leitura/rollback, mas não se deve descrever o wrapper inteiro como “sem rede” ou “sem escrita”. (`bin/sombra.sh:138-166`, `:489-491`)

## 06/11 21:52 · sessao 6e064515fdcd · V2-BUGS-I2 · verificador · origem=Code

**Reprovaria a volta 1:** a correção deixa o partido com dois pares incompleto, mantém a promoção de FT partido sem HE100 noturna e transforma o resultado errado em expectativa obrigatória no F11d. (`app/ponto/motor_calculo_v2.py:3105`, `app/ponto/motor_calculo_v2.py:2897`, `app/ponto/tests/test_o280_bateria_da_lei.py:1705`)

1. **Escritores, leitores e significados**

   | Campo/função | Quem escreve | Quem lê |
   |---|---|---|
   | `minutos_extra_100_noturna` | `_aplicar_he_noturna`; resets mensais e bloqueio zeram. (`app/ponto/motor_calculo_v2.py:1020`, `:2938`, `:3329`, `:1475`) | `ResultadoMes`, lavratura normal, promoção no DiaPago e promoção no FM. (`app/ponto/motor_calculo_v2.py:344`; `app/ponto/services/dia_pago.py:184`, `:252`; `app/ponto/services/fechamento.py:963`) |
   | `minutos_extra_100_feriado` | Dobras por período e por dia; também a releitura do calculador. (`app/ponto/motor_calculo_v2.py:2812`, `:3095`, `:3263`, `:3346`; `app/ponto/calculador/periodo.py:844`) | `ResultadoMes`, DiaPago e contas que excluem a dobra do excedente semanal. (`app/ponto/motor_calculo_v2.py:332`, `:2637`, `:3474`; `app/ponto/services/dia_pago.py:183`) |
   | FM.`horas_extras_100_noturna` | `recalcular_fechamento_mes`, com persistência na mesma porta; `restaurar_fechamento` também aceita restaurar o campo. (`app/ponto/services/fechamento.py:924`, `:961`, `:1168`, `:1379`, `:1392`) | Reconciliação da `porta_export`, por nome de campo. (`app/folha/porta_export.py:451`, `:452`) |
   | DiaPago.`horas_extras_100_noturna` | `lavrar`, somando períodos normais e FT promovida; versão oráculo pode substituir pela rubrica calculada, dentro da mesma função. (`app/ponto/services/dia_pago.py:184`, `:252`, `:329`, `:345`, `:360`) | Soma da competência, reconciliação e identificação de dia com dinheiro lavrado. (`app/ponto/services/dia_pago.py:562`, `:863`; `app/folha/porta_export.py:451`) |
   | `he_noturna` | Função decisória, retorna dois valores; não persiste campos. (`app/ponto/motor_calculo_v2.py:409`, `:451`) | Casca do motor e calculador por período. (`app/ponto/motor_calculo_v2.py:1014`; `app/ponto/calculador/regras.py:832`) |

   **Unidade:** o split devolve minutos de relógio; `ResultadoMes`, FM e DiaPago convertem para horas dividindo por 60. A redução fica em `minutos_noturnos_legais = relogio × fator`; o fator vale 1 quando afastada a hora reduzida. Portanto HE100 noturna e adicional noturno legal **não são numericamente a mesma medida**. (`app/ponto/motor_calculo_v2.py:441`, `:448`, `:344`, `:986`, `:992`; `app/ponto/services/dia_pago.py:252`; `app/ponto/services/fechamento.py:963`)

   **Não há significado uniforme de dia nem de trecho:** o DiaPago recebe o mapa da jornada, mas a promoção identifica feriado pela data civil da entrada; o split lê pontas cruas, enquanto duração e noturnas usam pontas efetivas. (`app/ponto/services/fechamento.py:947`, `:949`, `:1216`; `app/ponto/services/dia_pago.py:128`; `app/ponto/motor_calculo_v2.py:1015`, `:3216`, `:3252`)

2. **Duplicação e FM versus soma dos dias**

   Não encontrei duplicação pelo caminho normal `periodos + ft_feriado`: o motor coloca cada período em um dos dois conjuntos; `ResultadoMes` soma somente os regulares; a promoção atribui essa base mais FT feriada, sem acumular novamente o FM anterior. O DiaPago recebe as mesmas partições. (`app/ponto/motor_calculo_v2.py:2492`, `:2494`, `:344`; `app/ponto/services/fechamento.py:961`, `:979`, `:1202`)

   **Existe duplicação dentro da própria função noturna:** com prorrogação habilitada, o segmento 00–05 é estendido até a saída e depois o segmento 22–24 é somado novamente. Exemplo reproduzido com as funções extraídas em memória: **03–23, HE100=1.200 minutos → HE100 noturna=1.260**, contando 22–23 duas vezes. A dívida já está documentada no código; a chamada nova do comercial passa a expô-la no mensal. (`app/ponto/motor_calculo_v2.py:110`, `:112`, `:116`, `:153`, `:3361`)

   **FM diferente da soma dos dias continua possível por caminhos preexistentes:** o FM é persistido antes da lavratura, cuja exceção é capturada; uma falha Python nessa etapa pode deixar DP antigo ou ausente. A restauração também permite alterar somente o FM. Ocorrência real: **[não verificado]**. (`app/ponto/services/fechamento.py:1143`, `:1159`, `:1198`, `:1237`, `:1392`)

3. **Efeito da chamada antes do `continue`**

   A hipótese de mudar `minutos_noturnos` ou `extra_50` **não se sustenta**: a casca só escreve os dois subconjuntos noturnos; nos ramos da dobra, HE50 e HE50 noturna já foram zeradas. Assim, nesse caminho, somente HE100 noturna muda. (`app/ponto/motor_calculo_v2.py:1019`, `:1022`, `:2934`, `:2937`, `:3325`, `:3328`)

   O contraexemplo concreto é outro: **cru 13–23, efetivo 13–22, HE100=540, noturnas=0**. A chamada escreve **60 de HE100 noturna**, porque lê a saída crua. Na reprodução em memória, esse foi o único campo alterado. O recorte da saída ao marco existe, mas o carimbo guarda os efetivos em campos separados. (`app/ponto/motor_calculo_v2.py:1015`, `:1742`, `:1750`, `:1840`, `:1841`)

4. **Dois pares: início errado para a dobra do dia**

   **O split não alcança todo o trecho extra do dia:** os 420 minutos da dobra ficam em `ultimo`; o início calculado é limitado à entrada desse período. Para **21–00 / 03–07**, ele alcança somente 03–07: **240 minutos**, deixando os 120 noturnos do primeiro par de fora. (`app/ponto/motor_calculo_v2.py:3087`, `:3094`, `:406`; `app/ponto/tests/test_o280_bateria_da_lei.py:1697`, `:1705`)

   Pelo diff fornecido, isso é **lacuna preexistente parcialmente corrigida**, não regressão numérica: o pai pulava o split e deixava zero; agora entrega 240, ainda abaixo dos 360 exigidos pelo próprio F11e. Execução no pai: **[não verificado]**. (`app/ponto/motor_calculo_v2.py:3102`, `:3105`; `app/ponto/tests/test_o280_bateria_da_lei.py:1717`)

5. **Mordida dos REDs**

   - F11/F11b/F11c cobram valores numéricos — 120 minutos, 60 minutos e 1 hora — por `assertEqual`; não aceitam qualquer exceção. Pelo diff, a falha esperada no pai é valor zero, sem depender de símbolo introduzido pela correção. Execução efetiva no pai: **[não verificado]**. (`app/ponto/tests/test_o280_bateria_da_lei.py:1266`, `:1594`, `:1632`, `:1674`)
   - **F11c mata o mutante “somente FM”**, pois verifica separadamente `dp_he100_not=1.0`. Entretanto cobre somente COM/comercial, deixando FT partido sem essa prova. (`app/ponto/tests/test_o280_bateria_da_lei.py:1659`, `:1674`, `:1679`)
   - **F11d morde pelo critério errado:** exige 240, o defeito conhecido. Uma correção completa quebraria esse teste. (`app/ponto/tests/test_o280_bateria_da_lei.py:1704`, `:1705`)
   - **F11e aceita falha anterior ao valor legal:** `expectedFailure` envolve também a montagem do caso e a chamada ao motor, sem restringir tipo ou mensagem. Seu resultado esperado não prova que a falha ocorreu na comparação 240 versus 360. (`app/ponto/tests/test_o280_bateria_da_lei.py:1688`, `:1689`, `:1708`, `:1716`)

6. **Classes estruturais e outro buraco da correção**

   Não identifiquei novo juiz da janela nem escritor fora da porta no diff: as chamadas reutilizam `he_noturna`, FM permanece no serviço registrado e DiaPago permanece em `lavrar`. Fechamento e lavratura apenas somam minutos; o oráculo chama a mesma função decisória. (`app/ponto/motor_calculo_v2.py:1014`; `app/core/portas.py:500`; `app/ponto/services/dia_pago.py:360`; `app/ponto/calculador/regras.py:832`)

   **A alegação “o motor JÁ escreveu” é falsa para FT partido:** seu `calcular_periodo` retorna sem HE/split, e o mensal recalcula apenas períodos regulares. Logo, um FT partido **21–00**, promovido com escala certa, pode receber **180 minutos de HE100 e zero HE100 noturna**, apesar dos 120 minutos noturnos. As novas linhas apenas leem esse zero. (`app/ponto/motor_calculo_v2.py:225`, `:2893`, `:2896`, `:2897`, `:2928`, `:3083`; `app/ponto/services/fechamento.py:954`, `:963`; `app/ponto/services/dia_pago.py:252`)

   As classes problemáticas são **selo que protege o erro**, **pontas cruas versus efetivas** e **dia civil versus jornada**. Na promoção, o segundo par pós-meia-noite pode receber classificação diferente do primeiro mesmo pertencendo à mesma jornada feriada. Ocorrência em produção: **[não verificado]**. (`app/ponto/tests/test_o280_bateria_da_lei.py:1705`; `app/ponto/motor_calculo_v2.py:1015`; `app/ponto/services/fechamento.py:947`, `:949`; `app/ponto/services/dia_pago.py:245`)

7. **TXT/Domínio**

   **O subconjunto alterado não muda diretamente os eventos do TXT:** `eventos_do_fechamento` emite HE50, HE100 normal/feriado e adicional noturno, sem emitir `horas_extras_100_noturna`; os totais usados nesses eventos não são alterados pela nova casca. Igualdade byte a byte de arquivos gerados: **[não verificado]**. (`app/folha/export.py:420`, `:439`, `:447`; `app/ponto/motor_calculo_v2.py:1019`)

   A `porta_export` lê o campo indiretamente pela soma do DiaPago e compara ao FM, mas **essa divergência fica fora de `falhas`**. Portanto a reconciliação observa o problema sem, por si só, impedir o TXT. (`app/ponto/services/dia_pago.py:562`; `app/folha/porta_export.py:451`, `:455`, `:687`, `:697`)

## 06/11 21:51 · sessao eb474d436086 · - · - · origem=Ronald (tmux)

Achados para consolidar:

- **M3 continua incompleto para turno partido:** `calcular_periodo` desse motor retorna com trabalhados/noturnos, sem calcular HE nem split (`app/ponto/motor_calculo_v2.py:2877`, `:2893`, `:2896`, `:2897`). O mensal recalcula somente `resultado.periodos`, deixando FT fora (`:2927`, `:3083`). Portanto FT partido promovido pode levar 180 min de HE100 e 120 min noturnos, mas **zero HE100 noturna**, pois o default é zero (`:225`) e as novas linhas apenas o somam (`app/ponto/services/fechamento.py:963`; `app/ponto/services/dia_pago.py:252`). Caso real em produção: [não verificado].

- **Item 3: nenhum outro campo muda pela nova chamada.** A casca só atribui HE100 noturna e HE50 noturna (`motor_calculo_v2.py:1019`, `:1021`); nos ramos da dobra, HE50 e HE50 noturna já foram zerados (`:2934`, `:2937`, `:3325`, `:3328`). Logo não há contraexemplo de alteração de `minutos_noturnos` ou `extra_50` por essa chamada (`:1014`–`:1022`).

- **Mas o subconjunto pode incluir minuto bloqueado:** a casca lê `entrada/saida` crus (`motor_calculo_v2.py:1015`), enquanto o recorte só grava `efetivo_entrada/saida` (`:1840`, `:1841`), usados no cálculo de duração/noturnas (`:3216`, `:3218`, `:3252`). Contraexemplo reproduzido com AST em memória: cru 13–23, efetivo 13–22, HE100=540, noturno=0 ⇒ nova chamada altera somente HE100 noturna, **0→60**. O corte da saída ao marco é permitido em `:1733`–`:1750`. O calculador usa os efetivos (`app/ponto/calculador/alimentacao.py:83`, `:106`), portanto também pode divergir do motor nesse cenário (`app/ponto/calculador/regras.py:832`).

- **Item 4: split não alcança o primeiro par.** A dobra inteira vai em `ultimo` (`motor_calculo_v2.py:3087`, `:3094`); `_inicio_do_trecho_extra` limita o início à entrada desse período (`:406`). AST exata confirmou 03–07 com extra100=420 ⇒ HE100 noturna=240. F11d exige 240 (`app/ponto/tests/test_o280_bateria_da_lei.py:1705`); F11e exige 360, mas está `expectedFailure` (`:1708`, `:1717`). Pelo diff fornecido, o pai pulava a chamada e deixava zero: **lacuna preexistente parcialmente corrigida**, não regressão numérica nova; execução do pai [não verificado].

- **Outra duplicidade possível dentro do próprio split:** `minutos_noturnos` estende o segmento 00–05 até `fim` (`motor_calculo_v2.py:110`, `:111`) e soma também 22–24 (`:112`–`:116`). AST exata: 03–23, HE100=1200, prorrogação=True ⇒ **HE100 noturna=1260**, contando 22–23 duas vezes. A dívida consta no próprio comentário (`:151`–`:160`); a chamada nova do comercial passa a expô-la no mensal (`:3361`). Ocorrência em produção [não verificado].

- **Não há soma aditiva por chamadas repetidas da casca:** ela sobrescreve o campo (`motor_calculo_v2.py:1020`). A classificação normal coloca o período em FT **ou** em períodos regulares (`:2492`, `:2493`, `:2494`); as properties de HE somam apenas os regulares (`:344`). Assim, não identifiquei duplicação M1/M2+M3 por esse caminho (`app/ponto/services/fechamento.py:933`, `:962`, `:963`).

## 06/11 21:50 · sessao f5b13d011799 · - · - · origem=Ronald (tmux)

- **Não há dupla soma por `periodos + ft_feriado` no caminho normal.** O motor põe cada período em um lado do `if/else` (`app/ponto/motor_calculo_v2.py:2468`, `:2492`, `:2494`); `ResultadoMes.horas_extra_100_noturna` soma somente `periodos` (`:344`). A promoção substitui o FM por essa base mais `_ft_feriado`, sem `+=` (`app/ponto/services/fechamento.py:961`). A partição remove feriados de `_ft_certa` e exclui ambos de `_ft_sem_escala` (`:952`, `:953`, `:979`, `:980`). O DiaPago recebe essas mesmas listas (`:1202`) e soma o subconjunto uma vez em cada ramo (`app/ponto/services/dia_pago.py:184`, `:252`).

- **Há divergência preexistente de significado do dia na promoção.** O motor decide folga pelo `data_jornada` e registra esse dia no mapa (`app/ponto/motor_calculo_v2.py:2448`, `:2460`); a promoção decide feriado pela **data civil da entrada** (`app/ponto/services/fechamento.py:947`, `:949`). A lavratura, por sua vez, usa o mapa (`app/ponto/services/dia_pago.py:245`, `:128`, `:132`; mapa passado em `app/ponto/services/fechamento.py:1216`). Contraexemplo deduzido: dois pares de uma jornada feriada, `21–00 / 03–07`; o segundo pode continuar em folga trabalhada por começar civilmente no dia seguinte, em vez de ser promovido com o primeiro. Ocorrência em produção [não verificado].

- **FM ≠ soma DP continua possível por caminho preexistente.** O FM é persistido antes da lavratura (`app/ponto/services/fechamento.py:1143`, `:1159`); falha em `lavrar` é capturada e apenas registrada (`:1237`, `:1240`), permitindo FM novo com DP antigo/ausente. A porta de restauração também aceita somente campos de FM (`:1357`, `:1379`, `:1391`). Ocorrência real [não verificado]. Diferença de armazenamento float/decimal não prova falha de reconciliação: a soma DP arredonda ao final (`app/ponto/services/dia_pago.py:562`, `:563`).

- **Unidades e escritores do mensal/dia:** DP declara rubricas em horas e usa `FloatField` (`app/ponto/models.py:325`, `:332`); FM usa decimal de duas casas (`:419`). Ambos leem minutos do período e dividem por 60, sem recalcular janela (`app/ponto/services/dia_pago.py:184`, `:252`; `app/ponto/services/fechamento.py:961`). Persistência DP fica na mesma `lavrar`, por `bulk_create` (`app/ponto/services/dia_pago.py:345`, `:359`, `:360`); FM escreve na porta de fechamento (`app/ponto/services/fechamento.py:1143`, `:1168`), também restaurável pela porta (`:1391`).

- **TXT não lê diretamente os splits alterados.** `eventos_do_fechamento` emite HE50, HE100 normal/feriado, folga e adicional noturno, sem evento para `_100_noturna` (`app/folha/export.py:439`, `:447`); comentários identificam os splits como subconjuntos (`:424`, `:433`). Porém mudança colateral em HE50 ou `minutos_noturnos_legais` pode alterar o TXT via `horas_extras_50` e `horas_noturnas` (`:441`, `:447`).

- **`porta_export` lê o campo, mas a reconciliação não bloqueia por si.** A soma inclui o campo via `CAMPOS_DIA` (`app/ponto/services/dia_pago.py:36`, `:562`), e compara cada campo ao FM com tolerância de 0,015h (`app/folha/porta_export.py:239`, `:451`, `:455`). As divergências são publicadas em `fechamento_x_soma_dias_pagos`, fora de `_falhas_proprias` (`:687`, `:697`). Portanto é testemunha, sem mordida impeditiva para essa classe (`:687`, `:697`).

- **O oráculo possui outra derivação, mas usa a mesma função decisória:** `regras.do_dia` chama `he_noturna` por período e soma (`app/ponto/calculador/regras.py:825`, `:832`, `:835`, `:839`); a mesma `lavrar` grava a versão separada `oraculo` (`app/ponto/services/fechamento.py:1276`, `:1287`, `:1293`). O fechamento e a lavratura do motor não fazem essa conta; leem o campo do período (`app/ponto/services/fechamento.py:963`; `app/ponto/services/dia_pago.py:252`).

## 06/11 21:50 · sessao abee3abff962 · - · - · origem=Ronald (tmux)

- **F11d cobra o defeito, não a lei:** exige `he100_not=240`; F11e afirma `360`, mas fica sob `expectedFailure`. Uma implementação correta dos dois pares reprovaria F11d e produziria sucesso inesperado em F11e. (`app/ponto/tests/test_o280_bateria_da_lei.py:1704`, `:1708`, `:1717`)
- **F11/F11b/F11c/F11d cobram números**, por `assertEqual`, sem aceitar exceção genérica. F11 e F11b exigem respectivamente 120 e 60 minutos; F11c exige 1 hora no FM **e** no DiaPago, portanto mata o mutante “somar somente no FM”. (`app/ponto/tests/test_o280_bateria_da_lei.py:1263`, `:1594`, `:1632`, `:1672`, `:1679`)
- **F11e tem mordida fraca:** `expectedFailure` cobre também o fixture e a execução do motor, antes da comparação; não restringe tipo ou mensagem da falha. Assim, uma falha anterior à divergência legal também pode satisfazer o selo. (`app/ponto/tests/test_o280_bateria_da_lei.py:1688`, `:1689`, `:1708`, `:1716`)
- **RED no pai:** execução efetiva não verificada; a cópia não possui repositório Git acessível. A análise estática mostra campos existentes e expectativa numérica: `leitura` usa `getattr(..., 0)`, e os modelos já declaram os campos noturnos. (`app/ponto/tests/test_o280_bateria_da_lei.py:167`, `app/ponto/models.py:332`, `:419`; execução no pai [não verificado])
- **Não identifiquei novo juiz da janela:** motor chama `he_noturna`; os acréscimos FM/DP apenas somam o campo já preenchido. O calculador existente chama essa mesma função por período. (`app/ponto/motor_calculo_v2.py:1014`, `app/ponto/services/fechamento.py:961`, `app/ponto/services/dia_pago.py:252`, `app/ponto/calculador/regras.py:824`)
- **Não identifiquei novo escritor fora da porta no diff:** FM continua na função autorizada pelo registro e persiste por `save`/`update` no mesmo serviço; os campos novos do DiaPago continuam em `lavrar`. (`app/core/portas.py:500`, `app/ponto/services/fechamento.py:1143`, `:1159`, `:1168`, `app/ponto/services/dia_pago.py:252`)
- **Não há relógio novo no diff fornecido:** a leitura do instante da passada já fica no serviço de fechamento, e as somas acrescentadas não consultam o relógio. (`app/ponto/services/fechamento.py:616`, `:961`; `app/ponto/services/dia_pago.py:252`)

_2615 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
