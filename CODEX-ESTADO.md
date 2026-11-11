# CODEX-ESTADO

> Ultima resposta processada: **11/11/2026 12:10** · publicado em **11/11/2026 12:10** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 11/11 12:10 · sessao 9288c5b22025 · - · - · origem=Ronald (tmux)

Backend / sinais / testes, somente leitura: [nao verificado]

- **Servidor aceita batida sem selfie.** A porta web grava antes de olhar foto e só anexa quando há arquivo: `app/ponto/services/triagem_batida.py:700`, `app/ponto/services/triagem_batida.py:748`. As duas APIs leem foto opcional, transformam ausência em motivo de revisão e seguem para `registrar_batida`: `app/api/views.py:436`, `app/api/views.py:449`, `app/api/views.py:666`; `app/api/views_core.py:423`, `app/api/views_core.py:436`, `app/api/views_core.py:658`. O modelo permite foto vazia/nula: `app/ponto/models.py:78`.

- **Chokepoint não exige câmera/foto.** Seus argumentos obrigatórios são colaborador, timestamp, tipo e origem; os demais vêm em `**campos`; cria `Batida` sem exigir foto: `app/ponto/registro_batida.py:94`, `app/ponto/registro_batida.py:152`, `app/ponto/registro_batida.py:157`. Ele possui dedup e recusa por colisão tipo-oposto, questões distintas de selfie: `app/ponto/registro_batida.py:113`, `app/ponto/registro_batida.py:145`. A lei está em `CLAUDE.md:143`.

- **O juiz existente é de cobrança, não de admissibilidade.** `selfie_ausente` decide se foto ausente já é cobrável, com PWA no ato e APK após 90 minutos: `app/ponto/registro_batida.py:64`, `app/ponto/registro_batida.py:71`, `app/ponto/registro_batida.py:80`. Registro formal da pergunta “a foto da batida está ausente?” em `app/core/juizes.py:1192`. As duas APIs e o retro o consultam: `app/api/views.py:461`, `app/api/views_core.py:448`, `app/ponto/management/commands/marcar_foto_ausente_retro.py:74`. A porta web não faz essa consulta; grava e anexa condicionalmente: `app/ponto/services/triagem_batida.py:700`, `app/ponto/services/triagem_batida.py:748`.

- **Sinais não são iguais online web e offline sincronizado.** O catch online de apto apenas escreve aviso e mostra botões, sem acrescentar flag ao formulário; submit só inclui foto se existir: `app/templates/ponto/triagem/apto.html:156`, `app/templates/ponto/triagem/apto.html:228`. O serviço web determina `aprovada`/`justificativa` por fora de horário e `sinal_acesso`, sem considerar câmera ou foto ausente: `app/ponto/services/triagem_batida.py:685`, `app/ponto/services/triagem_batida.py:693`, `app/ponto/services/triagem_batida.py:710`. A API acrescenta `foto_ausente` e contexto `tipo_batida`; `liveness_status=indisponivel` acrescenta `liveness_indisponivel` e `motivo_cliente` vindo de `liveness_detail`: `app/api/views.py:464`, `app/api/views.py:501`; casca core: `app/api/views_core.py:451`, `app/api/views_core.py:488`.

- **Modelo dos sinais:** `Batida` possui `canal`, `foto`, `justificativa` e `aprovada`, sem campo específico de falha da câmera ou `liveness_status`: `app/ponto/models.py:8`, `app/ponto/models.py:55`, `app/ponto/models.py:56`, `app/ponto/models.py:63`, `app/ponto/models.py:78`. `foto_ausente`/`liveness_indisponivel` são motivos de `DisputaSupervisao`, armazenados em JSON de `motivos`/`contexto`: `app/chamados/models.py:1727`, `app/chamados/models.py:1730`, `app/chamados/models.py:1741`, `app/chamados/models.py:1745`. Assim, foto ausente não registra a causa “câmera indisponível”; o retro só conhece origem/canal/presença/tempo: `app/ponto/management/commands/marcar_foto_ausente_retro.py:66`.

- **Escritores/consumidores posteriores:** APIs associam `ultima_batida` e chamam a fábrica de disputa: `app/api/views.py:767`, `app/api/views_core.py:762`. A fábrica agrega/grava motivos e contexto, ou cria a disputa: `app/chamados/services/disputa_emissao.py:535`, `app/chamados/services/disputa_emissao.py:562`, `app/chamados/services/disputa_emissao.py:587`; gera perguntas: `app/chamados/services/disputa_emissao.py:669`. O leitor obtém pergunta pelo motivo e texto pelo contexto: `app/chamados/services/disputa_emissao.py:309`, `app/chamados/services/disputa_emissao.py:344`; definições de perguntas de foto e liveness em `app/chamados/catalogo/perguntas.py:94`, `app/chamados/catalogo/perguntas.py:112`. Retro lê tanto `FotoBatida` quanto `Batida.foto` e escreve cobrança `foto_ausente`: `app/ponto/management/commands/marcar_foto_ausente_retro.py:70`, `app/ponto/management/commands/marcar_foto_ausente_retro.py:95`.

- **Fotos recebidas depois:** porta `anexar_foto` escreve `Batida.foto` e, conforme modo, `FotoBatida`: `app/ponto/registro_batida.py:296`. Upload API exige foto para anexar a batida já existente, não para registrar a batida: `app/api/views_core.py:832`, `app/api/views.py:847`. Upload chama resolução das disputas de foto: `app/api/views_core.py:853`, `app/api/views.py:868`. Esse leitor filtra motivo, confere `ultima_batida.id`, apara motivo ou fecha disputa: `app/api/foto_recebida.py:57`, `app/api/foto_recebida.py:61`, `app/api/foto_recebida.py:67`, `app/api/foto_recebida.py:70`. Anexo web também só exige arquivo para anexar: `app/ponto/views.py:2895`.

- **Telemetria separada:** `api_diag_cam` recebe erro/modo/user-agent e escreve logger, sem associar sinal à batida: `app/api/views.py:2647`, `app/api/views.py:2655`, `app/api/views.py:2666`.

- **Rotas:** fila atual usa `/api/ponto/bater/`, com duas cascas de URLs: `app/api/urls.py:67`, `app/api/urls_core.py:14`. A rota antiga `/ponto/batida-offline/` existe, mas sempre responde 410: `app/ponto/urls.py:13`, `app/ponto/views.py:417`.

- **Testes relevantes, presença inspecionada, execução [nao verificado]:** `test_pwa_sem_foto_cria_batida_e_disputa` cobra HTTP 200, uma Batida e motivo `foto_ausente`; morderia bloqueio de servidor, não bloqueio do botão: `app/api/tests/test_api_bater_pwa.py:64`. A bateria offline também envia payload sem foto e cobra gravação: `app/ponto/tests/test_batida_offline.py:49`, `app/ponto/tests/test_batida_offline.py:68`. Z9 envia “camera bloqueada” e exige 200 + `liveness_indisponivel`: `app/api/tests/test_api_bater_ponto.py:185`. Testes do juiz cobrem PWA imediato, APK antes/depois do prazo, foto presente e origem não aparelho: `app/ponto/tests/test_selfie_ausente_juiz.py:27`. Teste de consumidores exige chamada ao juiz nos dois kernels: `app/ponto/tests/test_selfie_ausente_juiz.py:130`. Retro tem casos para marcação, foto recebida e prazo ainda aberto: `app/ponto/tests/test_bug126_selfie_invisivel.py:65`, `app/ponto/tests/test_bug126_selfie_invisivel.py:74`, `app/ponto/tests/test_bug126_selfie_invisivel.py:84`. O selo L030 cobre guardas GPS retornando 403, não foto/câmera nem JS: `app/api/tests/test_l030_selo_porta.py:3`, `app/api/tests/test_l030_selo_porta.py:18`, `app/api/tests/test_l030_selo_porta.py:83`.

## 11/11 12:10 · sessao b98828b87a03 · - · - · origem=Ronald (tmux)

Censo dos outros fronts, somente leitura.

| Sítio | Caminho | Resposta sem câmera/foto | Como |
|---|---|---|---|
| `app/templates/ponto/bater_ponto.html:246` | normal online/offline, antes de escolher E/S | **Bloqueia implicitamente** enquanto não liberar captura | `bloco-botoes` nasce `display:none`; `abrirCameraPre()` tenta câmera e, no catch, apenas oferece câmera do sistema, sem liberar botões (`:343-378`). Negação de permissão entra nesse mesmo catch (`:362`). |
| `app/templates/ponto/bater_ponto.html:384` | fallback do sistema | **Bloqueia** se cancelar/nenhum arquivo | `if (!file) return` (`:385-386`); libera botões somente após `FileReader.onload`, quando `_fotoBlob=file` (`:392-401`). |
| `app/templates/ponto/bater_ponto.html:407` | captura prévia | **Libera** se `toBlob` der null, lançar ou ficar sem callback | Tenta converter dataURL e libera botões mesmo sem blob (`:423-456`); salvaguarda de 1.500 ms também libera (`:458-467`). Porém `drawImage` e `toDataURL` ocorrem antes do `try` (`:414-417`), sem tratamento nessa função. |
| `app/templates/ponto/bater_ponto.html:470` | refazer | **Volta a bloquear** | Zera `_fotoBlob`, esconde botões e reabre câmera (`:471-477`); se nova câmera falhar, cai no fallback bloqueante descrito acima (`:362-378`). |
| `app/templates/ponto/bater_ponto.html:492` | envio online | **Libera sem foto**, se handler alcançado | Nenhum guard de `_fotoBlob`; anexa somente se existir (`:519-531`). Falha HTTP/rede cai na fila com `_fotoBlob`, inclusive null (`:540-543`). |
| `app/templates/ponto/bater_ponto.html:558` | offline, depois de escolher E/S | **Não bloqueia deliberadamente no catch** | Abre segunda câmera; mostra botão antes da solicitação (`:564-572`); catch apenas muda status para “Camera indisponivel” (`:579-581`). |
| `app/templates/ponto/bater_ponto.html:584` | confirmar offline sem stream | **Não retorna por falta de stream** | Só desenha se vídeo visível; faz `canvas.toBlob` incondicionalmente e enfileira o blob recebido, mesmo null (`:587-597`). Não possui fallback/timeout/try em volta de `toBlob` (`:594-610`); garantir comportamento em falha do canvas exige runtime **[nao verificado]**. Canvas nunca capturado pode produzir imagem vazia: inferência do desenho condicional e `toBlob` incondicional (`:587-597`), conteúdo real **[nao verificado]**. |
| `app/templates/ponto/bater_ponto.html:174` | confirmação de turno aberto | **Libera sem selfie nesse formulário** | POST com tipo/confirmar/latitude/longitude e botão submit, sem campo foto ou guarda de câmera (`:180-196`). |
| `app/templates/ponto/bater_ponto_v2.html:235` | v2 câmera, ambos caminhos | **Oferece fallback; não resolve admissibilidade** | Catch diferencia `NotAllowedError`, informa câmera negada/indisponível e abre input do sistema (`:241-247`); seleção vazia retorna (`:250-252`). |
| `app/templates/ponto/bater_ponto_v2.html:256` | v2 captura vazia/erro/timeout | **Continua até a guarda bloqueante** | Sem vídeo pronto resolve blob existente ou null (`:259`); `toBlob` null tenta dataURL (`:265`); catch e timeout de 1.500 ms tentam dataURL (`:266-267`). |
| `app/templates/ponto/bater_ponto_v2.html:285` | **v2 online e offline** | **Bloqueia explicitamente sem foto** | `garantirRequisitos` consulta permissão de câmera, mostra toast e retorna false quando `_fotoBlob` ausente (`:286-290`). `v2Confirmar` reabilita botão e retorna (`:307-312`) **antes** do teste `navigator.onLine` (`:315`); portanto não alcança POST online (`:322`) nem filas de fallback/offline (`:333`, `:346`). |
| `app/templates/core/onboarding.html:152` | onboarding, sem registro de batida próprio | **Etapa normal exige foto, mas permite configurar depois** | Catch troca para câmera do sistema (`:156-161`); seleção vazia retorna (`:168-169`); `salvarFoto` retorna sem blob (`:212-213`); captura `toBlob` não trata null/timeout (`:184-200`). Botão “Configurar depois” chama concluir e recarrega (`:120-125`). Backend computa faltas selfie/GPS/câmera (`app/core/views.py:479-486`) mas grava onboarding completo e responde ok mesmo com faltas (`:488-489`). |

Não há timeout explícito de **getUserMedia** nessas funções; os 1.500 ms são de conversão de imagem, e os demais timeouts próximos são de relógio/GPS (`app/templates/ponto/bater_ponto.html:343`, `:458`, `:480`; `app/templates/ponto/bater_ponto_v2.html:235`, `:267`, `:271`; `app/templates/core/onboarding.html:152`, `:263`). Comportamento do navegador quando a promessa nunca resolve **[nao verificado]**.

Scripts e includes:

- `bater_ponto.html` e `bater_ponto_v2.html` estendem `base.html` (`app/templates/ponto/bater_ponto.html:1`; `app/templates/ponto/bater_ponto_v2.html:1`). `apto.html` estende `base_triagem.html`, que estende `base.html` (`app/templates/ponto/triagem/apto.html:1`; `app/templates/ponto/triagem/base_triagem.html:1`).
- As três carregam `/static/pwa.js` defer (`app/templates/base.html:20`), `/static/js/hasner-ui.js?v=hx3` (`:83`), `hasner-table.js` (`:84`), `hasner-filter.js?v=ta5` (`:85`), `fita-mes.js?v=1` (`:86`), `geo-colab-map.js` (`:88`) e `popover-acao.js` (`:440`). Externo comum: htmx 1.9.10 do unpkg (`:19`).
- Só v2 acrescenta `/static/leaflet/leaflet.js` sob condição colaborador/sem sucesso (`app/templates/ponto/bater_ponto_v2.html:135-136`).
- Includes comuns da base: `core/_nudge_app_android.html`, condicional (`app/templates/base.html:92`); `includes/icons.html` (`:93`); `core/_icone_pauta.html` (`:290`); `core/_dialogo.html` (`:388`); `chamados/partials/_js_cobranca.html` (`:393`); `core/_drawer_generico.html` (`:399`); `core/_copiloto.html` (`:432`), este também inclui `_icone_pauta` (`app/templates/core/_copiloto.html:34`).
- A base de triagem acrescenta JS inline de relógio/GPS e bloco `extra_js`; nenhum `script src` adicional (`app/templates/ponto/triagem/base_triagem.html:25-76`).
- Onboarding é HTML independente com script inline, sem herança/includes/scripts externos (`app/templates/core/onboarding.html:1`, `:115`, `:330-332`).

Não identifiquei função/constante/endpoint comum consultado pelos três fronts para “selfie é impeditiva?”; a decisão hoje está nos ramos locais contraditórios acima (`app/templates/ponto/bater_ponto.html:362`, `:423`, `:492`; `app/templates/ponto/bater_ponto_v2.html:285`; `app/templates/ponto/triagem/apto.html:161`). Como **inferência de localização**, `app/static/pwa.js`, já carregado na base (`app/templates/base.html:20`) e chamado pelas telas para registrar (`app/templates/ponto/bater_ponto.html:543`; `app/templates/ponto/bater_ponto_v2.html:333`), é o menor módulo existente diretamente relacionado à batida que pode hospedar a resposta comum.

Sinais de câmera no onboarding são do **Colaborador**, não da Batida: front envia `{camera:true}` após salvar foto (`app/templates/core/onboarding.html:226-230`); backend grava `camera_concedida_em` (`app/core/views.py:1085-1090`); campo/modelo e leitura das etapas pendentes (`app/colaboradores/models.py:376`, `:405-413`). No catch de `bater_ponto` há telemetria `/api/diag/cam/` com erro/modo/user_agent (`app/templates/ponto/bater_ponto.html:367-377`); não vi sinal específico por batida sendo enviado nesse trecho (`:375`, `:530-543`).

Guarda existente relevante: `check_a_lei_batida.py` procura regex de guard de foto e frases em apto/bater (`app/scripts/check_a_lei_batida.py:13-23`); **não enumera v2**, não procura o `display:none` do catch e só encontra guard `_fotoBlob`/return na mesma linha (`:14-15`, `:20-21`). Seus checks de view apontam `app/apps/ponto/views.py` (`:24-29`) e arquivos ausentes são silenciosamente pulados (`:34-35`). Assim, esta guarda não morde os bloqueios observados do catch de `bater_ponto` ou da v2 (`app/templates/ponto/bater_ponto.html:362-378`; `app/templates/ponto/bater_ponto_v2.html:285-290`).

## 11/11 12:09 · sessao 56dd18a89d3f · - · - · origem=Ronald (tmux)

Fila e sincronização:

- A fila é IndexedDB `juliani-offline`, versão 2, store `batidas_pendentes`; nenhuma fila de batida em `localStorage` foi encontrada no escopo pesquisado. A implementação localizada está em `app/static/pwa.js:48-60`; outros enfileiradores [nao verificado].
- `registrarBatidaOffline()` aceita foto ausente: grava `foto: fotoBlob || null`, sem validação impeditiva da selfie. Guarda tipo, timestamps/gap, coordenadas/GPS e foto; não guarda flag de erro de câmera. `app/static/pwa.js:107-122`.
- A sincronização envia `foto` somente quando existe; usa POST multipart `/api/ponto/bater/`; acrescenta sempre `liveness_status=indisponivel` e `liveness_detail=PWA web sem liveness`. Isso informa ausência de liveness do PWA, sem distinguir câmera negada/indisponível/foto não capturada. `app/static/pwa.js:143-156`.
- Sucesso remove a entrada; HTTP 400 também remove definitivamente; erro de rede/5xx preserva para retentativa. `app/static/pwa.js:158-179`.
- `window.JulianiPWA` expõe o enfileirador; sincroniza no evento `online` e ao iniciar estando online. `app/static/pwa.js:305-328`.
- O SW ignora POST/PUT/PATCH/DELETE; não enfileira nem decide selfie. No evento `sync-batidas`, avisa janelas abertas para executarem a sincronização; sem janela, notifica para abrir o app. `app/static/sw.js:35-36`, `app/static/sw.js:112-129`.
- O fallback do SW trata resposta GET de rede/cache, sem requisito de câmera ou foto. `app/static/sw.js:29-30`, `app/static/sw.js:45-85`.

Falha específica offline:

- `apto.html` associa o botão a `confirmarFotoOffline`; no catch esconde o botão e põe `_streamOffline=null`, impedindo o clique normal de chegar à fila. `app/templates/ponto/triagem/apto.html:242-256`.
- Dentro de `confirmarFotoOffline`, `_streamOffline=null` não provoca retorno: só condiciona `getTracks().stop()`. A captura depende de `video.style.display`; `canvas.toBlob` continua e repassa o resultado à fila sem validar foto. Se o botão for chamado por outro meio, isso não comprova selfie válida: pode enviar conteúdo do canvas; resultado concreto do navegador [nao verificado]. `app/templates/ponto/triagem/apto.html:261-282`.
- V2 bloqueia antes da escolha online/offline: `garantirRequisitos()` retorna `false` sem `_fotoBlob`; o chamador volta antes tanto do fetch quanto da fila. `app/templates/ponto/bater_ponto_v2.html:285-290`, `app/templates/ponto/bater_ponto_v2.html:307-346`.

Selos/testes, apenas inspecionados; resultados atuais [nao verificado]: [nao verificado]

| Teste/selo | O que prova / limite | [nao verificado]
|---|---|
| `app/api/tests/test_api_bater_pwa.py:64-76` | Sem foto espera HTTP 200, Batida criada e disputa `foto_ausente`; morderia rejeição backend nesse caso. |
| `app/api/tests/test_api_bater_pwa.py:95-105` | Com foto espera 200 e ausência do motivo `foto_ausente`. |
| `app/ponto/tests/test_batida_offline.py:49-76` | Payload sem foto para `/api/ponto/bater/` espera 200 e Batida persistida. |
| `app/ponto/tests/test_batida_offline.py:119-128` | Espera disputa na batida sem foto/liveness. |
| `app/ponto/tests/test_selfie_ausente_juiz.py:27-47` | Cobra ausência por canal/prazo; não exercita captura ou clique. |
| `app/ponto/tests/test_selfie_ausente_juiz.py:130-140` | Confere consulta ao juiz nas duas cópias API. |
| `app/ponto/tests/test_l030_forma_js.py:31-55` | Selo de ausência de barreira GPS: filtra condições com `gpsLat/gpsLng/lat/lng`; no apto mede prefixo somente depois de `toBlob`. Não cobre catch que esconde o botão nem a barreira `_fotoBlob` de v2. |
| `app/ponto/tests/test_l030_templates_renderizados.py:14-37` | Renderiza apto/v2 nas duas cascas e aplica aquele mesmo oráculo GPS. |
| `bin/node_check.sh:11-20`, `bin/node_check.sh:33-37` | Apenas sintaxe dos JS estáticos próprios; inline de templates fica fora. Chamado pela régua em `bin/regua.sh:146-149`. |
| `app/core/tests/test_smoke_chromium.py:151-162`, `app/core/tests/test_smoke_chromium.py:172-189` | Abre `/ponto/espelho/` nas duas cascas e mede overlay/console; não captura câmera nem registra batida. |
| `app/core/tests/test_sw_offline_fallback.py:60-65`, `app/core/tests/test_sw_offline_fallback.py:79-101` | Mede Response/rede/cache do SW; não admite/rejeita selfie. |
| `app/core/tests/test_selo_modal_nao_nasce_vazio.py:135-143` | Guarda saída livre quando modal falha; não cobre câmera. |
| Smoke de clique específico sem câmera nas duas cascas | [nao verificado] |

O comando legado `testar_pwa` contém ensaio sem foto em `/ponto/batida-offline/`, mas não executa captura/click e sua expectativa “>48h rejeitada” está explícita; não tratar como prova do enfileirador atual, que envia para `/api/ponto/bater/`. `app/ponto/management/commands/testar_pwa.py:235-271`, `app/static/pwa.js:153`.

## 11/11 12:08 · sessao 555fd569cc38 · - · - · origem=Ronald (tmux)

Complemento do censo:

- **Resgatar** é controle manual que pode mudar estado: botão em `app/templates/chamados/partials/_lista_chamados.html:85`; quando o encerramento veio da competência trancada, chama `ch.reabrir(humano=True)` em `app/chamados/views_arquivar.py:64`. Não é seletor de destinos.
- **Abrir registro e validar** também muda estado: `app/templates/chamados/detalhe_local.html:233` → `ir_para_ausencia()` → `chamado.analisar()` em `app/chamados/views.py:1513`.
- **Atribuir/Assumir** não muda `status_local` nesse caminho: `app/chamados/views.py:1029` chama `assumir_responsavel`, que grava responsável e instante em `app/chamados/services/vinculos.py:131`.
- **Reabertura administrativa** oferece motivos manuais, por outro nome de select: `app/templates/chamados/partials/modal_fio.html:390` e `:393`; o receptor reabre chamado encerrado em `app/chamados/services/acoes_disputa.py:269`. As opções representam motivos, não destinos de estado.
- **Geofence** tem controles manuais nos dois locais: `app/templates/chamados/detalhe_local.html:285` e `app/templates/chamados/partials/modal_fio.html:506`. Autorizar, autorizar com advertência e recusar convergem para `resolvido` em `app/chamados/services/acoes_chamado.py:222` e `:277`.

Esses controles entram no censo amplo pedido, mas seus rótulos manuais não provam repetição do defeito “dois estados distintos com o mesmo texto”; são ações sobre o fato, com destinos fixados nos receptores acima.

`app/api/views.py:1143` é mapa manual de **exibição** (`status_display`), entregue na resposta GET em `app/api/views.py:1189`. O POST dessa API recebe `chamado_id` e `conteudo` e anexa ocorrência, sem receber estado (`app/api/views.py:982`).

Não achei registro/formulário de `ChamadoColaborador` nos arquivos `*admin*`/`*forms*`; `app/chamados/admin.py` está vazio. Existência de controle no Django admin publicado: `[nao verificado]`.

## 11/11 12:08 · sessao 1e67159c4f30 · - · - · origem=Ronald (tmux)

- **Bloqueia — selo permite reproduzir o defeito.** Em memória, substituí somente as tags de `resolvido` e `fechado` de `app/templates/chamados/detalhe.html:133` e `:134` por `<option value='resolvido'>Encerrado</option>` e `<option value='fechado'>Encerrado</option>`. O scanner real retornou cinco arquivos e `ruins=[]`: `_OPTION` reconhece apenas aspas duplas, e basta restar alguma ocorrência de `opcao_mover` no seletor. Âncoras: `app/chamados/tests/test_bo_rotulo_encerrado.py:19`, `:20`, `:58`, `:65`, `:69`. Executei o scanner em memória; execução da suíte Django completa: **[nao verificado]**. Os testes restantes consultam o mapa ou renderizam duas tags isoladas; o teste dos templates apenas chama `get_template`, sem verificar suas opções renderizadas (`:38`, `:43`, `:49`, `:73`).

- **Bloqueia — censo incompleto e includes não expandidos.** O universo limita-se a `templates/chamados` e a `name="novo_status"` com essa grafia; outro nome, aspas simples no `name` e controles fora desse diretório escapam (`app/chamados/tests/test_bo_rotulo_encerrado.py:17`, `:19`, `:26`). Um seletor contendo uma tag válida e um `{% include %}` com duas opções literais passa: o pai satisfaz a busca textual e o partial sem `<select>` não entra no censo (`:32`, `:65`, `:69`). A ordem dos atributos, isoladamente, **não** é fuga: `<option title="x" value="resolvido">` foi detectado pelo regex de `:20`.

- **Fila — selo não protege conjunto, ordem ou `value`.** Remover, repetir, reordenar tags ou trocar seu argumento por outro estado válido não encontra comparação com o conjunto anterior; o teste valida apenas que as chaves do mapa pertencem ao modelo e que não há opções literais reconhecidas (`app/chamados/tests/test_bo_rotulo_encerrado.py:43`, `:62`). A tag devolve exatamente o argumento como `value` (`app/chamados/templatetags/chamados_tags.py:77`).

- **Classes da pergunta 6:** selo sem mordida está presente nos casos acima. Não encontrei no trecho alterado outro juiz de transição: o registrado continua sendo `decidir` (`app/core/juizes.py:274`; `app/chamados/catalogo/motor.py:125`). O novo mapa responde à apresentação das opções (`app/chamados/services/verbos.py:132`, `:140`); o rótulo da pessoa permanece uma pergunta separada (`:162`). Duplicação de juiz registrado, derivação paralela ou campo com dois significados introduzidos pelo diff: **não demonstrados nesses caminhos**, com essas mesmas âncoras.

## 11/11 12:07 · sessao 80139f191a2c · - · - · origem=Ronald (tmux)

Achados e contratos rastreados, sem escrita de arquivos.

- **FILA — seletor sem efeito no detalhe GLPI, preexistente ao diff.** O formulário de `app/templates/chamados/detalhe.html:122` envia `novo_status` (`:130`–`:135`) para `chamados:followup`; a rota aponta a `adicionar_followup` (`app/chamados/urls.py:12`). Essa view lê apenas `conteudo` (`app/chamados/views.py:292`), chama `glpi.adicionar_followup` (`:304`) e retorna (`:310`), sem ler/aplicar `novo_status`. Portanto, nesse caminho, nenhum dos quatro valores muda `status_local`; efeito observado em produção `[nao verificado]`.

- **FILA — “Reabrir” não corresponde a toda reabertura tecnicamente permitida, preexistente ao diff.** Os detalhes enviam `aberto` sob “Reabrir” (`app/templates/chamados/detalhe_local.html:398`; `app/chamados/services/verbos.py:136`). O motor aceita `em_analise → aberto`, mas a matriz de `resolvido` só admite `fechado` e `em_analise` (`app/chamados/catalogo/motor.py:99`–`:106`), rejeitando `resolvido → aberto` para perfis diferentes de `admin_ti` (`:153`–`:155`). Superusuário vira `admin_ti` (`app/colaboradores/mixins.py:163`–`:164`). A regra não foi introduzida pelo diff fornecido `[nao verificado: classificação histórica na árvore sem git; comparação com anexo confirma value/label anterior iguais]`.

**Quem escreve/lê:**

| Item | Escritor | Leitores / efeito |
|---|---|---|
| `value` | `opcao_mover` escreve exatamente `status_local`, escapado por `format_html` (`app/chamados/templatetags/chamados_tags.py:74`–`:78`); argumentos literais nas cinco telas. | As views recebem códigos, nunca texto visível: local via `acao_resposta_livre`, fio/modal via `processar_post_fio`; GLPI ignora, conforme achado. |
| `novo_status`, detalhe local/painel turno | Formulários em `app/templates/chamados/detalhe_local.html:385` e `app/templates/chamados/partials/painel_chamado_turno.html:68`. | `detalhe_chamado_local` delega (`app/chamados/views.py:547`); serviço lê POST (`app/chamados/services/acoes_chamado.py:287`), exige gestor e pertencimento a `STATUS_CHOICES` (`:313`), chama `transicionar` (`:315`) e captura `ValueError` (`:320`). |
| `novo_status`, fio/modal | Controles em `app/templates/chamados/fio_colaborador.html:265` e `app/templates/chamados/partials/modal_fio.html:665`. | Views delegam ao mesmo serviço (`app/chamados/views.py:669`–`:674`, `:951`–`:954`); leitura em `app/chamados/services/fio.py:502`; ramo exige `chamado_id` **e conteúdo** (`:709`), RBAC (`:714`), código em `STATUS_CHOICES` (`:719`), `transicionar` (`:721`) e captura `ValueError` (`:722`). |
| Estados recebidos | `STATUS_CHOICES`: sete códigos (`app/chamados/models.py:28`–`:36`). | Pertencimento à lista é só filtro inicial; `transicionar` consulta perfil e juiz (`:130`–`:134`). Motor rejeita desconhecidos, terminais absolutos, destinos cancelado/superado, estado igual e transições fora da matriz (`app/chamados/catalogo/motor.py:139`–`:156`). Guardas de conteúdo permanecem (`app/chamados/models.py:136`, `:148`, `:181`). |
| Estado persistido | `transicionar → _aplicar` (`app/chamados/models.py:199`), `_aplicar → _gravar_estado` (`:447`–`:450`). | Atribuição a `self.status_local` em `:228`; persistência em `:296`. Nenhum rótulo participa da decisão. |
| `MOVER_PARA` | Definição única em `app/chamados/services/verbos.py:132`–`:137`. | Leitor produtivo único: `rotulo_de_movimento` (`:146`); testes também leem (`app/chamados/tests/test_bo_rotulo_encerrado.py:39`, `:45`–`:47`). |
| `ESTADO_TITLE` | Definição em `app/chamados/services/verbos.py:100`–`:108`. | `rotulo_de_movimento` (`:146`), `rotulo_de_linha` (`:155`), `rotulo_para_pessoa` (`:166`); testes (`app/chamados/tests/test_bo_rotulo_encerrado.py:47`). |
| `opcao_mover` | Tag registrada em `app/chamados/templatetags/chamados_tags.py:67`–`:78`. | Detalhe `:132`–`:135`, detalhe_local `:395`–`:398`, fio `:268`–`:270`, modal `:668`–`:670`, painel turno `:76`–`:78`, todos sob `app/templates/chamados/`. |

**KeyError:** nenhum chamador produtivo encontrado passa variável dinâmica. O único chamador Python produtivo é `app/chamados/templatetags/chamados_tags.py:76`; os 18 argumentos da tag são literais pertencentes aos mapas, nas linhas citadas acima. O acesso indexado pode lançar para outro código (`app/chamados/services/verbos.py:146`); ocorrência em produção `[nao verificado]`.

**Juiz duplicado / derivação paralela / semântica dupla:** o helper novo só escolhe apresentação (`app/chamados/services/verbos.py:140`–`:146`); não responde à pergunta de transição registrada em `app/core/juizes.py:108`, cuja autoridade continua `motor.decidir` (`:274`), chamada pelo modelo (`app/chamados/models.py:123`–`:125`). Texto, title e código permanecem separados na tag (`app/chamados/templatetags/chamados_tags.py:76`–`:78`); receptores comparam códigos (`app/chamados/services/acoes_chamado.py:313`, `app/chamados/services/fio.py:719`). Não encontrei leitor que compare o texto da opção. O uso de “arquivar” para `fechado` pode ser confundido com o véu de silêncio, cujo contrato preserva estado (`app/chamados/models.py:87`–`:92`), mas a expressão já estava no modal no diff anexo `[nao verificado: origem histórica fora do anexo]`; não é nova sobrecarga de campo pelo diff.

## 11/11 12:07 · sessao 555fd569cc38 · - · - · origem=Ronald (tmux)

Censo global não encontrou seletor de destino restante com opções manuais ou `espera`: os cinco usam `opcao_mover` em `app/templates/chamados/detalhe.html:130`, `detalhe_local.html:393`, `fio_colaborador.html:265`, `partials/modal_fio.html:665` e `partials/painel_chamado_turno.html:74`.

As leituras restantes de `espera` nesses arquivos são pílulas de exibição, fora dos seletores: `app/templates/chamados/detalhe.html:36`, `detalhe_local.html:68`, `fio_colaborador.html:188`, `partials/modal_fio.html:102`, `partials/painel_chamado_turno.html:18`.

Existe seletor manual fora de chamados, mas filtra relatório: `app/templates/relatorios/historico_chamados.html:54`; seu receptor usa `qs.filter(status_local=f_status)`, sem transição (`app/relatorios/views.py:919`).

Há outros controles manuais que alteram estado como consequência de ações de domínio:

- Aprovar/rejeitar benefício: botões em `app/templates/colaboradores/painel_solicitacao_beneficio.html:23` e JS em `app/templates/colaboradores/fila_solicitacoes_beneficio.html:138`; receptor escolhe `resolvido`/`fechado` em `app/colaboradores/views.py:2068`.
- Aceitar/recusar escala: controles em `app/templates/chamados/detalhe_local.html:163` e `:169`; ambos concluem em `resolvido` (`app/chamados/services/acoes_chamado.py:149`).
- Aceitar/recusar agenda: controles em `app/templates/chamados/detalhe_local.html:200` e `:209`; ambos concluem em `resolvido` (`app/chamados/services/acoes_chamado.py:108`).
- Aprovar exceção: `app/templates/chamados/detalhe_local.html:256`, `app/templates/chamados/partials/modal_fio.html:598`; conclusão `resolvido` em `app/chamados/services/acoes_chamado.py:70`.
- Autorizar/recusar geofence: recusa em `app/templates/chamados/partials/modal_fio.html:528`; os serviços concluem em `resolvido` (`app/chamados/services/acoes_chamado.py:222`, `:277`).

Esses controles usam ações e textos próprios, sem oferecer dois destinos com o mesmo rótulo; não constituem repetição atual do defeito do seletor (`app/colaboradores/views.py:2068`; `app/chamados/services/acoes_chamado.py:108`, `:149`).

## 11/11 11:57 · sessao 0adb21eb0cd2 · BO-ROTULO-ENCERRADO · censo · origem=Code

A duplicação está no código: `resolvido` e `fechado` recebem “Encerrado” em `ESPERA_COLAB`. **O modal distingue as opções por literais no template; não consulta uma fonte compartilhada para esses textos.** (`app/chamados/services/verbos.py:118`, `app/chamados/services/verbos.py:119`, `app/templates/chamados/partials/modal_fio.html:669`, `app/templates/chamados/partials/modal_fio.html:670`)

**1. Onde o detalhe monta as opções**

A página local é renderizada por `detalhe_chamado_local`, com contexto de `montar_contexto_detalhe`; a outra página é renderizada por `detalhe_chamado`. Os formulários e as opções estão nos próprios templates. (`app/chamados/views.py:549`, `app/chamados/views.py:550`, `app/chamados/services/acoes_chamado.py:399`, `app/chamados/views.py:262`, `app/templates/chamados/detalhe_local.html:385`, `app/templates/chamados/detalhe.html:122`)

| Opção/value | Detalhe local | Outro detalhe | Fonte do texto |
|---|---|---|---|
| vazio | `app/templates/chamados/detalhe_local.html:394` | `app/templates/chamados/detalhe.html:131` | Literal “— manter atual —” |
| `em_analise` | `app/templates/chamados/detalhe_local.html:395` | `app/templates/chamados/detalhe.html:132` | `"em_analise"\|espera\|first` |
| `resolvido` | `app/templates/chamados/detalhe_local.html:396` | `app/templates/chamados/detalhe.html:133` | `"resolvido"\|espera\|first` |
| `fechado` | `app/templates/chamados/detalhe_local.html:397` | `app/templates/chamados/detalhe.html:134` | `"fechado"\|espera\|first` |
| `aberto` | `app/templates/chamados/detalhe_local.html:398` | `app/templates/chamados/detalhe.html:135` | Literal “Reabrir” |

A cadeia que produz o texto é `espera` → `rotulo_para_pessoa` → `ESPERA_COLAB`. A função devolve `(texto, title)`; `first` escolhe o texto de espera, que coincide para os dois estados. (`app/chamados/templatetags/chamados_tags.py:63`, `app/chamados/templatetags/chamados_tags.py:64`, `app/chamados/services/verbos.py:138`, `app/chamados/services/verbos.py:141`, `app/chamados/services/verbos.py:118`, `app/chamados/services/verbos.py:119`)

Há um terceiro seletor com a mesma cadeia no fio do colaborador: placeholder literal em `app/templates/chamados/fio_colaborador.html:267`; `em_analise`, `resolvido` e `fechado` em `app/templates/chamados/fio_colaborador.html:268`, `app/templates/chamados/fio_colaborador.html:269`, `app/templates/chamados/fio_colaborador.html:270`.

Participação de form Python ou JS na produção desses textos: [não verificado]. A produção demonstrada acima ocorre nas expressões das próprias `<option>`.

**2. Fonte do modal do painel**

A view identifica e renderiza o modal em `app/chamados/views.py:869`, `app/chamados/views.py:870`, `app/chamados/views.py:927`.

| Value | Texto visível | `title` | Fonte |
|---|---|---|---|
| vazio | — mover para — | — | Literal, `app/templates/chamados/partials/modal_fio.html:667` |
| `em_analise` | Assumir a apuração | Em analise | Literal, `app/templates/chamados/partials/modal_fio.html:668` |
| `resolvido` | Encerrar | Resolvido | Literal, `app/templates/chamados/partials/modal_fio.html:669` |
| `fechado` | Encerrar e arquivar | Fechado | Literal, `app/templates/chamados/partials/modal_fio.html:670` |

Portanto, **a fonte atual das opções do modal é o próprio template**. O painel de turno repete os três textos e títulos à mão. (`app/templates/chamados/partials/modal_fio.html:668`, `app/templates/chamados/partials/modal_fio.html:669`, `app/templates/chamados/partials/modal_fio.html:670`, `app/templates/chamados/partials/painel_chamado_turno.html:76`, `app/templates/chamados/partials/painel_chamado_turno.html:77`, `app/templates/chamados/partials/painel_chamado_turno.html:78`)

Existem fontes que distinguem os **nomes dos estados**, mas aquelas opções não as consultam: choices do modelo, “Resolvido”/“Fechado”, e `ESTADO_TITLE`, também “Resolvido”/“Fechado”. (`app/chamados/models.py:31`, `app/chamados/models.py:32`, `app/chamados/services/verbos.py:103`, `app/chamados/services/verbos.py:104`)

**3. Censo dos leitores que apresentam estado ao humano**

| Leitor/saída | Fonte |
|---|---|
| Badge do detalhe | `espera`: texto e tooltip em `app/templates/chamados/detalhe.html:36`, `app/templates/chamados/detalhe.html:38` |
| Badge do detalhe local | `espera`: `app/templates/chamados/detalhe_local.html:68`, `app/templates/chamados/detalhe_local.html:70` |
| Badge do fio do colaborador | `espera`: `app/templates/chamados/fio_colaborador.html:188`, `app/templates/chamados/fio_colaborador.html:190` |
| Badge do painel de turno | `espera`: `app/templates/chamados/partials/painel_chamado_turno.html:18`, `app/templates/chamados/partials/painel_chamado_turno.html:20` |
| Badge do modal | `espera`: `app/templates/chamados/partials/modal_fio.html:102`, `app/templates/chamados/partials/modal_fio.html:104` |
| Seletores dos dois detalhes e do fio | `ESPERA_COLAB`, pela cadeia do item 1 |
| Seletor do modal | Literais: `app/templates/chamados/partials/modal_fio.html:668`, `app/templates/chamados/partials/modal_fio.html:669`, `app/templates/chamados/partials/modal_fio.html:670` |
| Seletor do painel de turno | Literais: `app/templates/chamados/partials/painel_chamado_turno.html:76`, `app/templates/chamados/partials/painel_chamado_turno.html:77`, `app/templates/chamados/partials/painel_chamado_turno.html:78` |
| Lista detalhada do painel | `rotulo_de_linha`; produção em `app/chamados/services/painel.py:384`, `app/chamados/services/painel.py:388`, `app/chamados/services/painel.py:394`; consumo em `app/templates/chamados/partials/_lista_chamados.html:80`, `app/templates/chamados/partials/_lista_chamados.html:81`, `app/templates/chamados/partials/_lista_chamados.html:87` |
| Lista resumida do painel | Mesma função; produção em `app/chamados/services/painel.py:485`, `app/chamados/services/painel.py:487`; consumo em `app/templates/chamados/partials/_lista_resumo.html:68`, `app/templates/chamados/partials/_lista_resumo.html:69`, `app/templates/chamados/partials/_lista_resumo.html:72` |
| Card de atendimento | Choices, via `get_status_local_display`: `app/templates/chamados/partials/_card_atendimento.html:23` |
| Histórico de chamados | Choices, via `get_status_local_display`: `app/templates/relatorios/historico_chamados.html:121` |
| Painel da justificativa | Choices, via `get_status_local_display`: `app/templates/ponto/partials/painel_justificativa.html:79` |
| Pauta de resposta tardia | Choices, via `get_status_local_display().lower()`: `app/chamados/services/resposta_tardia.py:63`, `app/chamados/services/resposta_tardia.py:67` |
| API de atendimento | Dicionário próprio em `app/api/views.py:1143`, `app/api/views.py:1144`, `app/api/views.py:1145`; fallback para código cru em `app/api/views.py:1146` |
| Filtro do relatório histórico | Literais “Aberto”, “Em análise”, “Resolvido”, “Fechado”: `app/templates/relatorios/historico_chamados.html:56`, `app/templates/relatorios/historico_chamados.html:57`, `app/templates/relatorios/historico_chamados.html:58`, `app/templates/relatorios/historico_chamados.html:59` |
| Manual servido ao humano | Literais em `app/templates/core/manual.html:207`, `app/templates/core/manual.html:215`, `app/templates/core/manual.html:216`, `app/templates/core/manual.html:217`, `app/templates/core/manual.html:219`, `app/templates/core/manual.html:222`, `app/templates/core/manual.html:225`, `app/templates/core/manual.html:249`, `app/templates/core/manual.html:250`, `app/templates/core/manual.html:251`, `app/templates/core/manual.html:252`, `app/templates/core/manual.html:254`, `app/templates/core/manual.html:278`, `app/templates/core/manual.html:282`, `app/templates/core/manual.html:283`, `app/templates/core/manual.html:284`, `app/templates/core/manual.html:285`, `app/templates/core/manual.html:286`, `app/templates/core/manual.html:294` |

Nos badges, `esp.0` vem de `ESPERA_COLAB` e `esp.1` de `ESTADO_TITLE`. Na lista administrativa, o texto principal é o verbo ou “Registro”; o estado distinguido fica no tooltip. (`app/chamados/services/verbos.py:131`, `app/chamados/services/verbos.py:133`, `app/chamados/services/verbos.py:135`, `app/chamados/services/verbos.py:141`, `app/chamados/services/verbos.py:142`)

Também há exposições do **código cru**, sem tradução:

| Saída | Referências |
|---|---|
| PDF dos chamados | Lê `ch.status_local` em `app/relatorios/pdf_chamados.py:116`; imprime em `app/relatorios/pdf_chamados.py:122`, `app/relatorios/pdf_chamados.py:125` |
| Tooltip do chamado no calendário | Produtores em `app/colaboradores/services/calendario.py:441`, `app/colaboradores/services/calendario.py:444`; consumidor em `app/templates/colaboradores/partials/_calendario_grade.html:108` |
| API mensageria | Campos crus em `app/api/views_mensageria.py:417`, `app/api/views_mensageria.py:1082` |
| Estado no histórico/API | `app/chamados/services/historico.py:156` → `app/api/views_mensageria.py:2166` |
| Texto das transições no histórico | Descrição com códigos gravada em `app/chamados/models.py:292`, `app/chamados/models.py:293`; lida em `app/chamados/services/historico.py:92`; textualizada em `app/chamados/services/historico.py:208`; entregue em `app/api/views_mensageria.py:2168` |
| Saídas operacionais | `app/chamados/management/commands/silenciar_chamados_isentos.py:41`, `app/ponto/management/commands/reconciliar_vinculo.py:57`, `app/ponto/management/commands/reavaliar_ausencias_feriado.py:42`, `app/ponto/management/commands/reavaliar_ausencias_lancadas.py:43`, `app/ponto/management/commands/reconciliar_geofence.py:36` |
| Erros do smoke | `app/chamados/management/commands/smoke_chamados.py:137`, `app/chamados/management/commands/smoke_chamados.py:150` |

O domínio GLPI tem seu próprio `STATUS_MAP` numérico; `_formatar_ticket` produz `status_label`, exibido na tabela de tickets. (`app/chamados/glpi_client.py:126`, `app/chamados/glpi_client.py:137`, `app/chamados/glpi_client.py:142`, `app/templates/chamados/partials/tabela_chamados.html:40`)

Outros textos manuais associados ao estado:

- “Reabrir”: `app/templates/chamados/detalhe.html:135`, `app/templates/chamados/detalhe_local.html:398`.
- “Este chamado está fechado”: `app/templates/chamados/detalhe.html:148`, `app/templates/chamados/detalhe_local.html:412`.
- Datas “Resolvido em”/“Resolvido”: `app/templates/chamados/detalhe.html:56`, `app/templates/chamados/detalhe_local.html:88`, `app/templates/relatorios/historico_chamados.html:127`.
- Títulos literais “Em analise”: `app/templates/chamados/detalhe.html:101`, `app/templates/chamados/fio_colaborador.html:225`, `app/templates/chamados/partials/_relatorio_geo.html:22`.
- Agrupamentos “Histórico/Cancelados/Encerrados” e “registrados”, sem correspondência individual de estado: `app/templates/chamados/painel_gestao.html:153`, `app/templates/chamados/painel_gestao.html:191`.

Renderização dos consumidores externos da API, inclusive aplicativo nativo: [não verificado].

**4. Campo, escritores, leitores e comparações**

O campo é `ChamadoColaborador.status_local`, ligado aos sete `STATUS_CHOICES`. Os códigos e conjuntos usados pelas decisões ficam no catálogo. (`app/chamados/models.py:28`, `app/chamados/models.py:52`, `app/chamados/catalogo/motor.py:19`, `app/chamados/catalogo/motor.py:24`, `app/chamados/catalogo/motor.py:29`, `app/chamados/catalogo/motor.py:30`, `app/chamados/catalogo/motor.py:31`, `app/chamados/catalogo/motor.py:32`)

| Escritor/porta | Local |
|---|---|
| Escrita de transição: `_gravar_estado` | Atribuição em `app/chamados/models.py:228`; trilha em `app/chamados/models.py:292`; persistência em `app/chamados/models.py:296` |
| `_aplicar` | Chama o escritor em `app/chamados/models.py:450` |
| `reconciliar` | `app/chamados/models.py:654` |
| `cancelar` | `app/chamados/models.py:788` |
| `analisar` | `app/chamados/models.py:806` |
| `reabrir` | `app/chamados/models.py:1442` |
| `encerrar` | `app/chamados/models.py:1619` |
| `encerrar_por_competencia_trancada` | `app/chamados/models.py:1639` |
| Nascimento por `abrir` | Estado preparado em `app/chamados/models.py:938`; `get_or_create` em `app/chamados/models.py:971` |
| Nascimento por `criar` | Estado resolvido em `app/chamados/models.py:1060`; `create` em `app/chamados/models.py:1095` |
| Simulação em memória | Atribuição temporária em `app/chamados/models.py:356`; restauração no `finally` em `app/chamados/models.py:371` |
| Backfills históricos | `app/chamados/migrations/0038_a3_registrado_backfill.py:10`, `app/chamados/migrations/0038_a3_registrado_backfill.py:15`, `app/chamados/migrations/0039_a3_via_backfill.py:24`, `app/chamados/migrations/0039_a3_via_backfill.py:33` |

Chamadores humanos de `transicionar`: `app/chamados/services/acoes_chamado.py:70`, `app/chamados/services/acoes_chamado.py:108`, `app/chamados/services/acoes_chamado.py:149`, `app/chamados/services/acoes_chamado.py:222`, `app/chamados/services/acoes_chamado.py:233`, `app/chamados/services/acoes_chamado.py:277`, `app/chamados/services/acoes_chamado.py:315`, `app/chamados/services/fio.py:721`, `app/colaboradores/views.py:2068`. Chamadas do smoke: `app/chamados/management/commands/smoke_chamados.py:134`, `app/chamados/management/commands/smoke_chamados.py:147`, `app/chamados/management/commands/smoke_chamados.py:163`.

Inventário dos demais arquivos com leituras diretas do campo em Python de produção, incluindo filtros ORM, atributos e serialização; as linhas identificam os sítios:

```text
app/api/views_core.py:309
app/api/views.py:320,1044,1142,1146,1152,1972,1982,3127
app/api/views_mensageria.py:417,1082

app/chamados/models.py:109,112,124,165,207,219,355,417,427,673,712,713,714,736,760,768,783,803,822,830,921,927,1219,1220,1221,1337,1342,1348,1358,1373,1374,1375,1398,1404,1440,1614,1630,1632,2683
app/chamados/juizes.py:780,858,2017,2162,2201
app/chamados/reconciliador.py:246,839,936,938,997,1204,1454,1587
app/chamados/rejuncao.py:61,79
app/chamados/signals.py:33,70,86,87,96,184,192,201,285,293,354,358
app/chamados/views.py:976,1477
app/chamados/views_cobrar.py:76
app/chamados/views_arquivar.py:121

app/chamados/services/abertura.py:166
app/chamados/services/acoes_chamado.py:25,90,132,176,232,252
app/chamados/services/acoes_disputa.py:269
app/chamados/services/adesao.py:89,102,143
app/chamados/services/becos.py:22
app/chamados/services/celula_carimbada.py:42,89,146
app/chamados/services/competencia_trancada.py:88,155,183,192,193,270
app/chamados/services/disputa_emissao.py:200,572,755,963,1109,1318,1344,1884,1910,1913
app/chamados/services/feriado_reavaliar.py:17
app/chamados/services/fila.py:126
app/chamados/services/fio.py:389,403,432,770
app/chamados/services/furo_sem_canal.py:83
app/chamados/services/historico.py:156,180
app/chamados/services/juizes_do_dia.py:92
app/chamados/services/limbo_folgas.py:100,220,278,302,314
app/chamados/services/materializacao.py:778,784
app/chamados/services/na_fila.py:17
app/chamados/services/painel.py:155,189,213,214,215,217,218,224,245,303,305,307,316,379,384,465
app/chamados/services/pergunta_no_app.py:151,184
app/chamados/services/resposta_tardia.py:35,57
app/chamados/services/silencio_ausencia.py:60,125
app/chamados/services/silencio_isentos.py:29
app/chamados/services/sla.py:37
app/chamados/services/verbos.py:230
app/chamados/services/vigia_fio.py:335,338,607,608,618
app/chamados/services/vigia_loop.py:34,117,120

app/colaboradores/services/calendario.py:437,441,442,444
app/colaboradores/services/desligamento.py:152,155
app/colaboradores/services/drawer.py:89
app/colaboradores/services/ficha.py:163
app/colaboradores/services/pos_vinculo.py:36
app/colaboradores/services/vinculo.py:385
app/colaboradores/management/commands/corrigir_escala_retroativa.py:141

app/escala/utils.py:1059
app/escala/services/cadastro_realidade.py:494
app/escala/services/regeneracao.py:170
app/escala/management/commands/fechar_avisos_regua_na_lista.py:34

app/ponto/signals.py:303
app/ponto/views.py:284,395,1616
app/ponto/worklist.py:40
app/ponto/services/ausencia.py:709
app/ponto/services/cartorio.py:473,1370,1629,1648
app/ponto/services/cluster_espurio.py:67,255,285,289,294
app/ponto/services/cobertura.py:114,226
app/ponto/services/entrada_sincronizada.py:49
app/ponto/services/esmeril_espelho.py:182
app/ponto/services/faltas_de_hoje.py:64
app/ponto/services/furos_sem_cobranca.py:69,70,129
app/ponto/services/furos_vetados.py:95
app/ponto/services/geofence.py:82,355
app/ponto/services/justificativa.py:43,61,77,118,121
app/ponto/services/regularizacao.py:133
app/ponto/services/triagem_batida.py:105,290,328
app/ponto/services/veredito_celula.py:149,155
app/ponto/services/vigia_de_hora.py:111

app/relatorios/views.py:745,920
app/relatorios/pdf_chamados.py:116
```

Leitores em comandos e scripts operacionais:

```text
app/chamados/management/commands/arquivar_prescritos.py:49
app/chamados/management/commands/auditar_chamados_orfaos.py:36
app/chamados/management/commands/auditar_invariantes_chamados.py:573,589,602,616,637,653,683,807,978,1015,1041,1068,1070,1072,1339,1576,1577,1607,1628
app/chamados/management/commands/carimbar_por_batida_coincidente.py:61,68
app/chamados/management/commands/disparar_perguntas_competencia.py:99
app/chamados/management/commands/geofence_registrados_para_analise.py:22,36
app/chamados/management/commands/lavrar_perguntas_stale.py:57
app/chamados/management/commands/reabrir_orfas_enterradas.py:98,134,170,171,173,174
app/chamados/management/commands/reconciliar_chamados.py:59
app/chamados/management/commands/retratar_chamados_lei_cala.py:34
app/chamados/management/commands/silenciar_chamados_isentos.py:41
app/chamados/management/commands/smoke_chamados.py:136,137,149,150

app/ponto/management/commands/detectar_ausencias.py:152,193,258,300,348,373,404,514,672,739,743,798
app/ponto/management/commands/detectar_intervalo_ausente.py:125
app/ponto/management/commands/emitir_furo_retroativo.py:95
app/ponto/management/commands/emitir_turno_aberto_retroativo.py:119,123
app/ponto/management/commands/escalonar_chamados_supervisao.py:45
app/ponto/management/commands/fechar_chamados_ausencia.py:28
app/ponto/management/commands/fechar_chamados_meio_periodo.py:56
app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:42
app/ponto/management/commands/gerar_celulas.py:245
app/ponto/management/commands/processar_alertas_avancados.py:103,108,155
app/ponto/management/commands/processar_alertas_turno.py:165,179,381
app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:31
app/ponto/management/commands/reavaliar_ausencias_feriado.py:42
app/ponto/management/commands/reavaliar_ausencias_lancadas.py:30,43
app/ponto/management/commands/reconciliar_fantasmas.py:159
app/ponto/management/commands/reconciliar_geofence.py:29,36
app/ponto/management/commands/reconciliar_grade.py:96,207,529
app/ponto/management/commands/reconciliar_vinculo.py:33,57
app/ponto/management/commands/supra_juiz.py:189,212

bin/censo_folha.py:30
bin/r5_idempotencia_frota.py:61
bin/snapshot_o118_col221.py:22
```

Leitores nos templates, incluindo condicionais de cor e existência de ação:

```text
app/templates/chamados/detalhe.html:36,37,39,41,117
app/templates/chamados/detalhe_local.html:68,69,71,73,153,176,244,279,343,375
app/templates/chamados/fio_colaborador.html:167,188,189,191,193,254
app/templates/chamados/partials/_card_atendimento.html:13,19,20,21,23,203
app/templates/chamados/partials/_lista_chamados.html:33,37,38,40,41,79
app/templates/chamados/partials/modal_fio.html:76,102,103,105,107,500,544,559,588,636,651
app/templates/chamados/partials/painel_chamado_turno.html:18,19,21,23,65
app/templates/ponto/partials/painel_justificativa.html:79
app/templates/relatorios/historico_chamados.html:121
```

Leitores históricos adicionais nas migrations: filtros em `app/chamados/migrations/0023_rebaixar_urgencia_resolvidos.py:5`, `app/chamados/migrations/0023_rebaixar_urgencia_resolvidos.py:14`, `app/chamados/migrations/0024_rebaixar_urgencia_resolvidos_regressao.py:6`, `app/chamados/migrations/0024_rebaixar_urgencia_resolvidos_regressao.py:1

[truncado em 20000 de 24377 caracteres]

## 11/11 11:54 · sessao e0331546fc34 · UI-PAINEL-MOSTRA-O-APP · construtor · origem=Code

A cura permanece pendente: a captura de `setUpClass` pelo `freeze_time` herdado é hipótese **[nao verificado]**. Preservei a fixture e o assert, conforme exigido ([investigação](/home/ronald/wt-codex-ui-painel-mostra-o-app-100626/app/docs/UI-PAINEL-MOSTRA-O-APP-v2.txt:27)).

Os 7 testes em memória passaram; os quatro mutantes isolados falharam por asserção. Comandos e resultados estão no [recibo](/home/ronald/wt-codex-ui-painel-mostra-o-app-100626/app/docs/UI-PAINEL-MOSTRA-O-APP-v2.txt:56). [nao verificado]

Falta o caminho de um Python com Django/Freezegun para comprovar a causa ([pendência](/home/ronald/wt-codex-ui-painel-mostra-o-app-100626/app/docs/UI-PAINEL-MOSTRA-O-APP-v2.txt:120)). Não fiz commit pela proibição explícita nas REGRAS ([registro](/home/ronald/wt-codex-ui-painel-mostra-o-app-100626/app/docs/UI-PAINEL-MOSTRA-O-APP-v2.txt:3)). [nao verificado]

## 11/11 11:53 · sessao 558816c733bd · - · - · origem=Ronald (tmux)

O campo é `ChamadoColaborador.status_local`, com sete escolhas em `app/chamados/models.py:28` e declaração em `app/chamados/models.py:52`. O catálogo compara códigos (`aberto`, `resolvido`, `fechado` etc.), definidos em `app/chamados/catalogo/motor.py:19`, com conjuntos em `:24`, `:29`, `:30`, `:31`, `:32` e decisões em `:125`.

Escritores persistidos e temporários:

- Transição: `_gravar_estado`, atribuição em `app/chamados/models.py:228`, trilha em `:292`, persistência em `:296`.
- Gates que chegam ao escritor: `_aplicar` `app/chamados/models.py:450`; `reconciliar` `:654`; `cancelar` `:788`; `analisar` `:806`; `reabrir` `:1442`; `encerrar` `:1619`; `encerrar_por_competencia_trancada` `:1639`.
- Nascimento por `abrir`: prepara estado em `app/chamados/models.py:938`, instancia provisória em `:961`, `get_or_create` em `:971`.
- Nascimento por `criar`: resolve estado em `app/chamados/models.py:1060`, instancia provisória em `:1070`, persiste em `:1095`.
- Posestado só em memória: `app/chamados/models.py:356`, restaurado no `finally` em `:371`. O selo declara explicitamente essa exceção em `app/chamados/tests/test_contract_status_nucleo.py:10`.
- Chamadores humanos de `transicionar`: `app/chamados/services/acoes_chamado.py:70,108,149,222,233,277,315`; `app/chamados/services/fio.py:721`; `app/colaboradores/views.py:2068`. Smoke operacional: `app/chamados/management/commands/smoke_chamados.py:134,147,163`.
- Escritores históricos de dados, separados de runtime: `app/chamados/migrations/0038_a3_registrado_backfill.py:10,15`; `app/chamados/migrations/0039_a3_via_backfill.py:24,33`.

Não encontrei comparação de **decisão de estado** pelos textos humanos na superfície pesquisada; decisões demonstradas usam códigos: `app/chamados/catalogo/motor.py:136,140,148,150,153`; validação de `dict(STATUS_CHOICES)` por chave em `app/chamados/services/fio.py:719` e `app/chamados/services/acoes_chamado.py:313`. O JS compara `.value` e `dataset.status` em `app/templates/chamados/painel_gestao.html:376,382`, produzido em `app/templates/chamados/partials/_lista_chamados.html:33`. Consumidores externos da API: [não verificado].

Há busca/ordenação genérica pelo texto, sem decidir estado: `app/static/js/hasner-table.js:191,194,209,235,236`; a coluna local fornece código/verbo em `app/templates/chamados/partials/_lista_chamados.html:79`. Isso deve ser distinguido de comparar `"Encerrado"` para autorizar uma transição. [referências acima]

Inventário compacto de leituras explícitas de `status_local` em Python de produção, incluindo atributos, `getattr`, filtros ORM e chaves de dicionários/trilha; testes, migrations e documentação foram separados. Nas linhas do próprio escritor, também aparecem leituras do estado anterior: [nao verificado]

```text
app/ponto/worklist.py:40
app/ponto/views.py:284,395,1616
app/ponto/signals.py:303
app/relatorios/views.py:745,920
app/relatorios/pdf_chamados.py:116
app/escala/utils.py:1059
app/api/views_core.py:309
app/api/views.py:320,1044,1142,1146,1152,1972,1982,3127
app/api/views_mensageria.py:417,1082
app/chamados/reconciliador.py:246,839,936,938,997,1204,1454,1587
app/chamados/views_cobrar.py:76
app/chamados/rejuncao.py:61,79
app/chamados/models.py:109,112,124,165,207,219,229,294,355,417,427,673,712,713,714,736,760,768,783,803,822,830,921,927,938,1070,1095,1219,1220,1221,1337,1342,1348,1358,1373,1374,1375,1398,1404,1440,1614,1630,1632,2683
app/chamados/juizes.py:780,858,2017,2162,2201
app/chamados/views.py:976,1477
app/chamados/views_arquivar.py:121
app/chamados/signals.py:33,70,86,87,96,184,192,201,285,293,354,358
app/colaboradores/services/calendario.py:437,441,442,444
app/colaboradores/services/pos_vinculo.py:36
app/colaboradores/services/drawer.py:89
app/colaboradores/services/vinculo.py:385
app/colaboradores/services/desligamento.py:152,155
app/colaboradores/services/ficha.py:163
app/colaboradores/management/commands/corrigir_escala_retroativa.py:141
app/ponto/services/faltas_de_hoje.py:64
app/ponto/services/cobertura.py:114,226
app/ponto/services/cartorio.py:473,1370,1629,1648
app/ponto/services/justificativa.py:43,61,77,118,121
app/ponto/services/entrada_sincronizada.py:49
app/ponto/services/furos_sem_cobranca.py:69,70,129
app/ponto/services/cluster_espurio.py:67,255,285,289,294
app/ponto/services/furos_vetados.py:95
app/ponto/services/vigia_de_hora.py:111
app/ponto/services/geofence.py:82,355
app/ponto/services/ausencia.py:709
app/ponto/services/veredito_celula.py:149,155
app/ponto/services/esmeril_espelho.py:182
app/ponto/services/triagem_batida.py:105,290,328
app/ponto/services/regularizacao.py:133
app/ponto/management/commands/supra_juiz.py:189,212
app/ponto/management/commands/escalonar_chamados_supervisao.py:45
app/ponto/management/commands/reavaliar_ausencias_lancadas.py:30,43
app/ponto/management/commands/processar_alertas_avancados.py:103,108,155
app/ponto/management/commands/reconciliar_grade.py:96,207,529
app/ponto/management/commands/reconciliar_fantasmas.py:159
app/ponto/management/commands/emitir_furo_retroativo.py:95
app/ponto/management/commands/reconciliar_vinculo.py:33,57
app/ponto/management/commands/detectar_ausencias.py:152,193,258,300,348,373,404,514,672,739,743,798
app/ponto/management/commands/emitir_turno_aberto_retroativo.py:119,123
app/ponto/management/commands/fechar_chamados_meio_periodo.py:56
app/ponto/management/commands/reconciliar_geofence.py:29,36
app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:42
app/ponto/management/commands/gerar_celulas.py:245
app/ponto/management/commands/reavaliar_ausencias_feriado.py:42
app/ponto/management/commands/processar_alertas_turno.py:165,179,381
app/ponto/management/commands/detectar_intervalo_ausente.py:125
app/ponto/management/commands/fechar_chamados_ausencia.py:28
app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:31
app/escala/services/regeneracao.py:170
app/escala/services/cadastro_realidade.py:494
app/escala/management/commands/fechar_avisos_regua_na_lista.py:34
app/chamados/services/vigia_loop.py:34,117,120
app/chamados/services/silencio_ausencia.py:60,125
app/chamados/services/na_fila.py:17
app/chamados/services/furo_sem_canal.py:83
app/chamados/services/vigia_fio.py:335,338,607,608,618
app/chamados/services/celula_carimbada.py:42,89,146
app/chamados/services/abertura.py:166
app/chamados/services/materializacao.py:778,784
app/chamados/services/silencio_isentos.py:29
app/chamados/services/resposta_tardia.py:35,57
app/chamados/services/pergunta_no_app.py:151,184
app/chamados/services/limbo_folgas.py:100,220,278,302,314
app/chamados/services/feriado_reavaliar.py:17
app/chamados/services/acoes_chamado.py:25,90,132,176,232,252
app/chamados/services/verbos.py:230
app/chamados/services/disputa_emissao.py:200,572,755,963,1109,1318,1344,1884,1910,1913
app/chamados/services/competencia_trancada.py:88,155,183,192,193,270
app/chamados/services/becos.py:22
app/chamados/services/painel.py:155,189,213,214,215,217,218,224,245,303,305,307,316,379,384,465
app/chamados/services/juizes_do_dia.py:92
app/chamados/services/acoes_disputa.py:269
app/chamados/services/fio.py:389,403,432,770
app/chamados/services/historico.py:156,180
app/chamados/services/adesao.py:89,102,143
app/chamados/services/fila.py:126
app/chamados/services/sla.py:37
app/chamados/management/commands/carimbar_por_batida_coincidente.py:61,68
app/chamados/management/commands/silenciar_chamados_isentos.py:41
app/chamados/management/commands/disparar_perguntas_competencia.py:99
app/chamados/management/commands/auditar_chamados_orfaos.py:36
app/chamados/management/commands/auditar_invariantes_chamados.py:573,589,602,616,637,653,683,807,978,1015,1041,1068,1070,1072,1339,1576,1577,1607,1628
app/chamados/management/commands/arquivar_prescritos.py:49
app/chamados/management/commands/reabrir_orfas_enterradas.py:98,134,170,171,173,174
app/chamados/management/commands/geofence_registrados_para_analise.py:22,36
app/chamados/management/commands/retratar_chamados_lei_cala.py:34
app/chamados/management/commands/reconciliar_chamados.py:59
app/chamados/management/commands/lavrar_perguntas_stale.py:57
app/chamados/management/commands/smoke_chamados.py:136,137,149,150
bin/snapshot_o118_col221.py:22
bin/r5_idempotencia_frota.py:61
bin/censo_folha.py:30
```

Leitores de template do campo bruto e/ou display, incluindo condicionais de cor e existência de ação:

```text
app/templates/chamados/fio_colaborador.html:167,188,189,191,193,254
app/templates/chamados/partials/painel_chamado_turno.html:18,19,21,23,65
app/templates/chamados/partials/modal_fio.html:76,102,103,105,107,500,544,559,588,636,651
app/templates/chamados/partials/_card_atendimento.html:13,19,20,21,23,203
app/templates/chamados/partials/_lista_chamados.html:33,37,38,40,41,79
app/templates/chamados/detalhe.html:36,37,39,41,117
app/templates/chamados/detalhe_local.html:68,69,71,73,153,176,244,279,343,375
app/templates/relatorios/historico_chamados.html:121
app/templates/ponto/partials/painel_justificativa.html:79
```

Leitores indiretos que recebem o código por argumento e geram rótulo: `app/chamados/services/verbos.py:125,138`; filtro intermediário em `app/chamados/templatetags/chamados_tags.py:54,64`. Os demais consumidores de rótulos estão na tarefa do outro agente. [referências citadas]

Selos do estado, úteis para demonstrar o limite da cura: escritor único em `app/chamados/tests/test_contract_status_nucleo.py:18` e asserção em `:33`; conjuntos canônicos em `app/chamados/tests/test_contract_status_literal.py:1`; scanner AST de escritores distingue atribuição de filtros em `bin/esmeril_chamado_censo.py:99,108,113`.

## 11/11 11:53 · sessao bc1033217c1a · - · - · origem=Ronald (tmux)

Censo dos rótulos de `ChamadoColaborador.status_local`, com fontes e consumidores:

| Fonte | Consumidores |
|---|---|
| Choices do modelo: `app/chamados/models.py:28-36`; campo associado em `:52` | `app/templates/chamados/partials/_card_atendimento.html:23`; `app/templates/relatorios/historico_chamados.html:121`; `app/templates/ponto/partials/painel_justificativa.html:79`; pauta de resposta tardia em `app/chamados/services/resposta_tardia.py:63-67` |
| `ESPERA_COLAB`: `app/chamados/services/verbos.py:114-122`; função `rotulo_para_pessoa` em `:138-142`; filtro `espera` em `app/chamados/templatetags/chamados_tags.py:54-64` | Badges: `detalhe.html:36-46`, `detalhe_local.html:68-78`, `fio_colaborador.html:188-198`, `partials/painel_chamado_turno.html:18-28`, `partials/modal_fio.html:102-112`, todos sob `app/templates/chamados/`. Seletores: `detalhe.html:132-134`, `detalhe_local.html:395-397`, `fio_colaborador.html:268-270`. Os dois estados terminais recebem “Encerrado” em `verbos.py:118-119`. |
| `ESTADO_TITLE`: `app/chamados/services/verbos.py:100-108`; distingue “Resolvido” e “Fechado” em `:103-104` | Segundo elemento do filtro `espera`, exibido nos `title` dos badges acima; também `rotulo_de_linha` em `:125-135`. |
| `rotulo_de_linha`: `app/chamados/services/verbos.py:125-135` | Produção do painel: `app/chamados/services/painel.py:384-394` e `:485-487`; consumo em `app/templates/chamados/partials/_lista_chamados.html:80-87` e `_lista_resumo.html:68-72`. Texto principal é verbo/“Registro”; estado fica no `title`. |
| Dicionário próprio da API | `app/api/views.py:1143-1146`: Aberto, Em atendimento, Resolvido, Fechado; fallback devolve código cru. |
| Literais do modal | `app/templates/chamados/partials/modal_fio.html:668-670`: “Assumir a apuração”, “Encerrar”, “Encerrar e arquivar”; títulos “Em analise”, “Resolvido”, “Fechado”. Não consulta mapa nesses elementos. |
| Literais do painel de turno | `app/templates/chamados/partials/painel_chamado_turno.html:76-78`: mesmas três opções e títulos do modal. |
| Literais do filtro de relatório | `app/templates/relatorios/historico_chamados.html:56-59`: Aberto, Em análise, Resolvido, Fechado. |
| Literais de documentação humana | `app/templates/core/manual.html:207`, `:215-217`, `:219`, `:222`, `:225`, `:249-254`, `:278`, `:282-286`, `:294`. |

Outros textos manuais próximos do estado: “Reabrir” nos seletores `app/templates/chamados/detalhe.html:135` e `detalhe_local.html:398`; “Este chamado está fechado” em `detalhe.html:148` e `detalhe_local.html:412`; “Resolvido em” em `detalhe.html:56`, `detalhe_local.html:88` e relatório `app/templates/relatorios/historico_chamados.html:127`. Os títulos “Em analise” escritos independentemente do estado estão em `app/templates/chamados/detalhe.html:101`, `fio_colaborador.html:225` e `partials/_relatorio_geo.html:22`.

Exposição do código cru:

| Sítio | Fonte |
|---|---|
| PDF: `app/relatorios/pdf_chamados.py:116`, renderizado em `:122-125` | `ch.status_local`, sem choices/mapa |
| Calendário: `app/templates/colaboradores/partials/_calendario_grade.html:108` | `dia.ticket_falta.status`, produzido a partir de `_c.status_local` em `app/colaboradores/services/calendario.py:441,444` |
| API mensageria: `app/api/views_mensageria.py:417` e `:1082` | `status_local` direto, campos `status_local` e `situacao` |
| Histórico do chamado: `app/chamados/services/historico.py:156`; API em `app/api/views_mensageria.py:2166` | `estado` recebe `status_local` cru |
| Eventos históricos | `app/chamados/models.py:292-294` grava descrição `status anterior -> novo via ...` com códigos; `services/historico.py:86-95` lê descrição; `:204-209` monta texto; API entrega em `views_mensageria.py:2168` |
| Comandos humanos | Código cru em `app/chamados/management/commands/silenciar_chamados_isentos.py:39-41`; `app/ponto/management/commands/reconciliar_vinculo.py:55-57`, `reavaliar_ausencias_feriado.py:42`, `reavaliar_ausencias_lancadas.py:43`, `reconciliar_geofence.py:36`; erros em `app/chamados/management/commands/smoke_chamados.py:137,150` |

GLPI tem outro domínio de estado: `app/chamados/glpi_client.py:126-133` define `STATUS_MAP` numérico; `:135-143` monta `status_label`; `:198,202` formatam listagem/detalhe; `app/templates/chamados/partials/tabela_chamados.html:40` exibe, com render da view em `app/chamados/views.py:340`.

A aba agrupada escreve “Histórico/Cancelados/Encerrados” em `app/templates/chamados/painel_gestao.html:153`; não é correspondência individual de estado. “registrados” do placar está em `:191`.

O JS do painel compara códigos: `app/templates/chamados/painel_gestao.html:382` usa `r.dataset.status === status`; o atributo vem de `app/templates/chamados/partials/_lista_chamados.html:33`. Renderização do consumidor externo da API/mobile: [não verificado].

## 11/11 11:52 · sessao aad120ef0dda · - · - · origem=Ronald (tmux)

1. **Detalhe: duas páginas produzem as opções.**

| Sítio | Texto produzido |
|---|---|
| `app/templates/chamados/detalhe_local.html:394` | Literal “— manter atual —”. |
| `app/templates/chamados/detalhe_local.html:395` | `"em_analise"|espera|first`. |
| `app/templates/chamados/detalhe_local.html:396` | `"resolvido"|espera|first`. |
| `app/templates/chamados/detalhe_local.html:397` | `"fechado"|espera|first`. |
| `app/templates/chamados/detalhe_local.html:398` | Literal “Reabrir”, value `aberto`. |
| `app/templates/chamados/detalhe.html:131` | Literal “— manter atual —”. |
| `app/templates/chamados/detalhe.html:132` | `"em_analise"|espera|first`. |
| `app/templates/chamados/detalhe.html:133` | `"resolvido"|espera|first`. |
| `app/templates/chamados/detalhe.html:134` | `"fechado"|espera|first`. |
| `app/templates/chamados/detalhe.html:135` | Literal “Reabrir”, value `aberto`. |

O filtro delega para `rotulo_para_pessoa`, que consulta `ESPERA_COLAB`; nesse mapa, `resolvido` e `fechado` têm ambos “Encerrado”. `app/chamados/templatetags/chamados_tags.py:53`, `app/chamados/templatetags/chamados_tags.py:63`, `app/chamados/services/verbos.py:138`, `app/chamados/services/verbos.py:118`, `app/chamados/services/verbos.py:119`.

A view local renderiza `detalhe_local.html` com `montar_contexto_detalhe`; o contexto entrega o chamado e permissões, sem lista de opções. `app/chamados/views.py:549`, `app/chamados/views.py:550`, `app/chamados/services/acoes_chamado.py:399`. A outra view renderiza `detalhe.html` diretamente. `app/chamados/views.py:262`. Os formulários são HTML nesses próprios templates. `app/templates/chamados/detalhe_local.html:385`, `app/templates/chamados/detalhe.html:122`. Gerador JS dessas opções: [não verificado].

2. **O modal distingue por literais no template; não consulta mapa compartilhado para suas opções.**

| Value | Texto | Title | Referência |
|---|---|---|---|
| vazio | — mover para — | — | `app/templates/chamados/partials/modal_fio.html:667` |
| `em_analise` | Assumir a apuração | Em analise | `app/templates/chamados/partials/modal_fio.html:668` |
| `resolvido` | Encerrar | Resolvido | `app/templates/chamados/partials/modal_fio.html:669` |
| `fechado` | Encerrar e arquivar | Fechado | `app/templates/chamados/partials/modal_fio.html:670` |

A view identifica esse componente como modal do painel e o renderiza. `app/chamados/views.py:869`, `app/chamados/views.py:870`, `app/chamados/views.py:927`. O painel lateral de turnos repete os mesmos três rótulos e titles, também à mão. `app/templates/chamados/partials/painel_chamado_turno.html:76`, `app/templates/chamados/partials/painel_chamado_turno.html:77`, `app/templates/chamados/partials/painel_chamado_turno.html:78`; renderizador em `app/chamados/views.py:347`, `app/chamados/views.py:353`.

Os choices do modelo distinguem “Resolvido” e “Fechado”, mas as opções dos quatro templates acima não os leem. `app/chamados/models.py:31`, `app/chamados/models.py:32`, `app/templates/chamados/detalhe_local.html:396`, `app/templates/chamados/detalhe.html:133`, `app/templates/chamados/partials/modal_fio.html:669`, `app/templates/chamados/partials/painel_chamado_turno.html:77`.

5. **Testes e leis relevantes.** [nao verificado]

- `test_o137_esmeril_chamado.py` varre texto de espera escrito à mão; exclui deliberadamente “Encerrado” do padrão. `app/chamados/tests/test_o137_esmeril_chamado.py:34`, `app/chamados/tests/test_o137_esmeril_chamado.py:99`.
- O mesmo teste exige que `espera(st)` devolva exatamente `rotulo_para_pessoa(st)` para todo estado conhecido e entradas de fallback. `app/chamados/tests/test_o137_esmeril_chamado.py:115`.
- Fixa literalmente `Registrado|Registrado (auditoria, sem acao devida)`, `Encerrado|Superado — o fato vive em outro chamado` e `Com o DP|Aberto (aguardando primeira analise)`. `app/chamados/tests/test_o137_esmeril_chamado.py:122`.
- Há guarda contra estado de banco visível nos templates de chamados: enumera “Resolvido”/“Fechado” e exige lista vazia de ofensores. Alterar opções para esses textos crus encontra essa guarda. `app/chamados/tests/test_ui_fatia2_taxonomia.py:25`, `app/chamados/tests/test_ui_fatia2_taxonomia.py:60`.
- Há teste exigindo o estado “Resolvido” no `title` da fila. `app/chamados/tests/test_ui_fatia2_taxonomia.py:131`.
- Teste existente que exija especificamente unicidade das opções `resolvido`/`fechado`: [não verificado]. [nao verificado]
- L2 determina a mesma função para tela e ação; o selo documental citado é `holerite/tests/test_selo_badge_viaja.py::test_o_numero_aparece_na_TELA_nao_so_no_contexto`. `app/docs/LEIS-UI.md:23`, `app/docs/LEIS-UI.md:29`.
- L7 determina texto vindo do mesmo mapa e proíbe hardcode por tela; declara **“sem selo ainda”**. `app/docs/LEIS-UI.md:77`, `app/docs/LEIS-UI.md:85`.

6. **Intersecção direta entre produtores desses textos e os cinco caminhos informados: nenhuma.** Os produtores identificados são os quatro templates, `chamados_tags.py` e `services/verbos.py` acima. `app/templates/chamados/detalhe_local.html:395`, `app/templates/chamados/detalhe.html:132`, `app/templates/chamados/partials/modal_fio.html:668`, `app/templates/chamados/partials/painel_chamado_turno.html:76`, `app/chamados/templatetags/chamados_tags.py:63`, `app/chamados/services/verbos.py:114`.

Há dependência de `app/chamados/juizes.py` na montagem de contexto, mas para `folha_julga_pontualidade`, sem produção desses rótulos. `app/chamados/services/acoes_chamado.py:331`, `app/chamados/views.py:926`. O caminho literal `app/ponto/cartorio.py` não foi encontrado [não verificado]; o arquivo existente é `app/ponto/services/cartorio.py` [não verificado].

## 11/11 11:50 · sessao 39b2c713671a · MAPA-20 · verificador · origem=Code

REPROVADO para pouso [nao verificado]

Referências `base:` apontam para a cópia disponível; `diff:` usa as linhas de destino do v3, sem aplicá-lo. Identidade da cópia com o HASH informado: [nao verificado]. Execução integral dos selos e publicação real: [nao verificado].

1. **Fechamento dos achados numerados da volta 2.**

   | Achado da v2 | Resultado no v3 |
   |---|---|
   | **1 — triagem 1–5** | **Nome fecha**: universo literal correto. **Tabela permanece parcial**: fecha separador estreito, cercas, cabeçalho vazio e pontuação; ainda aceita tabela escondida em comentário HTML. **Retido fecha os defeitos apontados**, agora incluindo conferência da remoção. **Foto fecha no publicador atual**. **Selo permanece parcial**, pelos mutantes do item 4 abaixo. (`diff:bin/mapa20_valida.py:29–39,82–103,120–122`; `diff:bin/relato.sh:94–114`; `diff:bin/tests/test_mapa20_valida.sh:151–164`.) | [nao verificado]
   | **2 — saídas e falhas** | Fecha a falha de commit seguida de sucesso aparente: agora há `|| exit 1`. São **14 comandos `exit`**, discriminados no item 3. `cd` e os `git add` continuam sem guarda de falha. (`diff:bin/relato.sh:146–164`.) | [nao verificado]
   | **3 — parser e conformidade** | **Parcial**: exige as 12 colunas, mas ainda aceita conteúdo invisível e fonte que não fundamenta N. As recusas a `_E3_`, ausência de pipes externos e barras invertidas permanecem, agora como restrições explícitas do formato. (`diff:bin/mapa20_valida.py:10–21,82–103,110–135`.) |
   | **4 — selo e mutantes** | Fecha o mutante específico sem reconhecimento de tils; acrescenta fiscalização textual da foto. Permanecem outros mutantes de validador, foto e instalação do retido. Recorte literalmente vazio continua impedindo verde. (`diff:bin/tests/test_mapa20_valida.sh:76–79,105–109,160–168`.) | [nao verificado]
   | **5 — selos vizinhos** | Nenhuma quebra demonstrada nos predicados que leem `relato.sh`; execução integral [nao verificado]. (`base:bin/tests/test_codex_estado.sh:104–111`; `base:bin/tests/test_relato_publica_claude_md.sh:15–20`; demais referências no item 5.) |
   | **6 — escritores e leitores** | A foto agora tem variável própria. O escritor efetivo da fonte e seus gatilhos continuam [nao verificado]; o comentário afirma atualização por sessão/evento, sem implementá-la nesse bloco. (`diff:bin/relato.sh:89–98`; `diff:bin/mapa20_valida.py:10`.) |
   | **7 — classes históricas** | Persistem **selo sem mordida** e interpretação de conteúdo de outras colunas como encerramento da obra. Pré-requisitos efetivos de pouso continuam [nao verificado]. (`diff:bin/mapa20_valida.py:141–142`; `diff:bin/tests/test_mapa20_valida.sh:151–164`; `base:CLAUDE.md:377–381,394`.) | [nao verificado]

2. **Complemento X-de-N: cumprimento e contraexemplos.**

   Nos exemplos, **G** é o mapa completo de 12 colunas, com 18 obras, SEM DONO e seção inicial `PRONTO AUSENTE`, conforme a fixture do selo. Os contraexemplos do parser foram conferidos em memória, sem arquivos. (`diff:bin/tests/test_mapa20_valida.sh:20–39`; `diff:bin/mapa20_valida.py:80–149`.)

   | Regra | Conferência |
   |---|---|
   | **(i) Sem X de N nem PRONTO AUSENTE → rc 1** | **Cumpre para as 18 obras**: exige o literal, N≥1 e X≤N; a regex também impede X negativo. **Não cumpre literalmente para toda linha**: SEM DONO recebe exceção e passa com `n/a (SEM DONO)`. (`diff:bin/mapa20_valida.py:43,123–140`.) |
   | **(ii) N sem fonte citada → rc 1** | **Cumpre apenas a aparência de citação**. Ausência do padrão reprova; existência, conteúdo, autoridade e número de itens da fonte não são conferidos. (`diff:bin/mapa20_valida.py:44,132–135`.) |
   | **(iii) Encerrada com X<N → rc 1** | **Cumpre para palavra reconhecida nas colunas após `obra`**, inclusive maiúsculas e flexões; também reprova encerramento com PRONTO AUSENTE. A varredura não distingue estado da obra de nomes, arquivos ou negações. (`diff:bin/mapa20_valida.py:48,129–142`.) |
   | **(iv) Lista PRONTO AUSENTE no topo** | Exige seção antes da tabela e igualdade com as obras marcadas. **Parcial**: lê somente a primeira seção; uma segunda seção contraditória é ignorada. (`diff:bin/mapa20_valida.py:55–77,104–106,146–148`.) |

   **Aceitos indevidamente:**

   - Em G, E3 com `999 de 999`, fonte `app/docs/BACKLOG.md:1` e falta `nada` passa. A linha citada é o título do BACKLOG, não uma lista numerada de PRONTO. Também passa `a.md:0`. O código não confere N nem prova escrita de X. (`base:app/docs/BACKLOG.md:1`; `diff:bin/mapa20_valida.py:44,132–137`.)
   - `<!--` numa linha, seguido de G e depois `-->`, passa: a tabela fica escondida no comentário. G inteiramente indentado com quatro espaços também passa como conteúdo de código Markdown. O parser veta cercas, mas aplica `strip()` às linhas e não reconhece comentários. (`diff:bin/mapa20_valida.py:82–89`.)
   - Duas seções iniciais: primeira com `- nenhuma`, segunda com `- O168`, tabela sem ausentes. Passa, embora o documento apresente listas contraditórias. (`diff:bin/mapa20_valida.py:58–77,146–148`.)

   **Recusados indevidamente como encerramento da obra:**

   - E3 com `1 de 2` e `cruza com = BO-ROTULO-ENCERRADO` reprova. O nome dessa obra fica corretamente isento na própria coluna `obra`, mas dispara a regex quando citado em outra coluna. (`diff:bin/mapa20_valida.py:33,48,141–142`.)
   - E3 com `1 de 2` e texto `nao encerrada`, ou caminho `logs/encerrada.md:10`, reprova sem afirmar encerramento de E3. (`diff:bin/mapa20_valida.py:48,141–142`.)

   A exigência de falta **item a item** não é comprovada: para X<N, só há veto ao literal `nada`, além da checagem geral de célula preenchida. A gravação de `pousou fase <F>, X de N` no RELATO também não é implementada pelo novo bloco. (`diff:bin/mapa20_valida.py:120–122,136–137`; `diff:bin/relato.sh:89–114`.) [nao verificado]

3. **Todos os `exit` de `relato.sh`, com seus valores.**

   | Local | Valor e condição |
   |---|---|
   | `base:bin/relato.sh:27` | `1`: falha da trava. |
   | `base:bin/relato.sh:28` | `1`: destino sem `.git`. |
   | `base:bin/relato.sh:32` | `1`: falha da lista de nomes. |
   | `base:bin/relato.sh:35` | `1`: falha do BACKLOG-TOPO. |
   | `base:bin/relato.sh:38` | `1`: falha do ESTADO. |
   | `base:bin/relato.sh:42` | `1`: falha da SESSAO. |
   | `base:bin/relato.sh:86` | `1`: falha da raspagem do RELATO retido. |
   | `diff:bin/relato.sh:110` | `1`: MAPA não raspado continua existindo após tentativa de remoção. |
   | `diff:bin/relato.sh:128` | `1`: falha de `gerar_avais.py`. |
   | `diff:bin/relato.sh:143` | `1`: falha da raspagem geral. |
   | `diff:bin/relato.sh:157` | `$MAPA_RC`: `0` ou `1`, índice sem mudança. |
   | `diff:bin/relato.sh:162` | `1`: falha do commit. | [nao verificado]
   | `diff:bin/relato.sh:163` | `1`: falha da expressão push/echo. | [nao verificado]
   | `diff:bin/relato.sh:164` | `$MAPA_RC`: `0` ou `1`, saída final. |

   **Validador vermelho executado → rc 0:** não há esse caminho no fluxo mostrado. `MAPA_RC` começa em zero, recebe um na retenção e não é zerado depois. (`diff:bin/relato.sh:94–100,157–164`.)

   **MAPA público pode sumir sem o Python reprovar:** sim, **sem executar o validador**: cópia da fonte falha, Python é pulado, raspagem do retido falha e o público é removido. Entretanto, o ciclo já está com rc 1. A remoção é incluída no commit; não há saída normal zero nesse caminho. (`diff:bin/relato.sh:97–113,151–164`.) [nao verificado]

   **Foto:** no publicador atual, sim: copia o vivo para `$MAPA_FOTO`, valida essa foto e entrega o mesmo caminho ao raspador. O Python isoladamente aceita qualquer caminho recebido na CLI; ele não impõe essa propriedade. (`diff:bin/relato.sh:94–98,143`; `diff:bin/mapa20_valida.py:152–167`.) [nao verificado]

   **Rc 0 não comprova toda a publicação:** se o `git add` do MAPA falhar e o índice estiver sem mudanças, a saída de “nada mudou” ainda pode retornar zero com mapa verde. Esse erro não é guardado. (`base:bin/relato.sh:21`; `diff:bin/relato.sh:151–157`.) [nao verificado]

4. **O selo ainda deixa mutantes plausíveis passarem.**

   A sobrevivência abaixo decorre dos cenários e predicados do selo; execução integral dos mutantes: [nao verificado].

   - **Validador:** retirar `re.I` de `_ENCERRADA`. Os casos de encerramento usam minúsculas e preservam seus resultados; `ENCERRADA` com `1 de 2` passa no mutante. (`diff:bin/mapa20_valida.py:48`; `diff:bin/tests/test_mapa20_valida.sh:41,53–56`.)
   - **Foto:** inserir `MAPA_FOTO="$RAIZ/logs/MAPA-20/mapa.md"` imediatamente antes de formar `MAPA_PAR`. Valida a foto e publica o vivo. Os sete cenários não alteram a fonte entre esses pontos; os greps exigem presença textual das referências, sem impedir reatribuição. (`diff:bin/relato.sh:97–98`; `diff:bin/tests/test_mapa20_valida.sh:151–164`.)
   - **Higiene do retido:** retirar o `&& mv`, ajustando a continuação. O raspador falso apenas copia os mesmos bytes de `MAPA VELHO`; o selo não distingue instalar a versão raspada de conservar a original. (`diff:bin/relato.sh:105–106`; `diff:bin/tests/test_mapa20_valida.sh:115–123,127,144,153–154`.)
   - **Falha de remoção:** retirar a guarda da linha 110 também preserva os cenários, pois nenhum injeta falha de `rm`. (`diff:bin/relato.sh:110`; `diff:bin/tests/test_mapa20_valida.sh:151–157`.)

   **Recorte literalmente vazio com selo verde:** não. Os três recortes têm guardas que incrementam `FALHAS` quando falta a expressão exigida; verde exige zero. Isso comprova presença mínima, não integridade dos recortes. (`diff:bin/tests/test_mapa20_valida.sh:105–109,168–169`.) [nao verificado]

5. **Selos vizinhos que leem `relato.sh`.**

   Nenhuma quebra estática demonstrada nos cinco arquivos; execução integral [nao verificado].

   | Selo | Conferência |
   |---|---|
   | `test_codex_estado.sh` | Recorta a partir de `CODEX_OK=""`, depois do novo bloco; seu veto a `exit` não captura a nova guarda. (`base:bin/tests/test_codex_estado.sh:104–111`; `diff:bin/relato.sh:110,122–124`.) |
   | `test_relato_publica_claude_md.sh` | A regex continua selecionando a raspagem geral; CLAUDE permanece nos três pontos. O novo `git add` indentado não altera a contagem por início de linha. (`base:bin/tests/test_relato_publica_claude_md.sh:15–20`; `diff:bin/relato.sh:105–106,129–153,162`.) |
   | `test_codex_estado_evento.sh` | Permanecem as chamadas exigidas, sem gerador CODEX inline. (`base:bin/tests/test_codex_estado_evento.sh:27–30`; `base:bin/relato.sh:32`; `diff:bin/relato.sh:123`.) |
   | `test_relato_afirma_com_prova.sh` | A chamada exigida permanece. (`base:bin/tests/test_relato_afirma_com_prova.sh:43–45`; `base:bin/relato.sh:55–56`.) |
   | `test_publicacao_sem_nome.sh` | O universo vem de `git ls-files`; MAPA rastreado entra automaticamente. Resultado de privacidade real [nao verificado]. (`base:bin/tests/test_publicacao_sem_nome.sh:58–73`.) |

6. **Quem escreve e quem lê.**

   | Campo/arquivo | Escritor | Leitor |
   |---|---|---|
   | `MAPA_RC` | Shell: `0` inicialmente; `1` por falha da cópia ou validação. | Saídas sem mudança e final. (`diff:bin/relato.sh:94–100,157,164`.) |
   | `MAPA_PAR` | Shell: vazio ou par foto→público após validação verde. | Argumento opcional da raspagem geral. (`diff:bin/relato.sh:94–98,143`.) | [nao verificado]
   | `MAPA_FOTO` | Shell define caminho; `cp` escreve os bytes; trap limpa o temporário. | Validador e raspador geral. (`base:bin/relato.sh:25`; `diff:bin/relato.sh:94–98,143`.) |
   | `logs/MAPA-20/mapa.md` | Comentários atribuem à sessão/`montar_mapa.py`; escritor efetivo e gatilhos [nao verificado]. | `cp` do publicador. (`diff:bin/mapa20_valida.py:10`; `diff:bin/relato.sh:89–97`.) |
   | Público `MAPA-20.md` | Raspador geral; no retido, raspador→arquivo ao lado→`mv`; falha pode removê-lo. | Raspagem do retido, Git e selo de privacidade. (`base:bin/raspar_publico.py:23–40`; `diff:bin/relato.sh:104–110,143,151–163`; `base:bin/tests/test_publicacao_sem_nome.sh:66–73`.) |
   | `MAPA_PATH` | Shell: vazio ou nome após `git add` bem-sucedido. | Mensagem e pathspec do commit, incluindo retirada. (`diff:bin/relato.sh:150–162`.) | [nao verificado]

7. **Classes históricas, uma a uma.**

   | Classe | Resultado |
   |---|---|
   | **2º juiz** | Não há novo juiz de nomes: máscara e conferência reutilizam `nome_no_texto`. A fonte e a prova de X/N não são verificadas semanticamente. (`base:bin/raspar_publico.py:15–16,31,36`; `diff:bin/mapa20_valida.py:132–135`.) |
   | **2º escritor fora da porta** | As novas escritas públicas estão no publicador, depois da trava existente. Escritor efetivo da fonte [nao verificado]. (`base:bin/relato.sh:27`; `diff:bin/relato.sh:97–114,143`.) | [nao verificado]
   | **Relógio fora da borda** | O diff não introduz relógio decisório; `date` permanece na mensagem de commit. O timer já existia. Atualização efetiva da fonte por evento [nao verificado]. (`diff:bin/relato.sh:89–90,161`; `base:bin/hasner-relato.timer:5–7`.) | [nao verificado]
   | **Derivação paralela de fonte única** | O publicador atual valida e publica a mesma foto, pelo raspador existente. Reuso dos censos e medições reais [nao verificado]. (`diff:bin/relato.sh:94–98,143`; `base:bin/raspar_publico.py:19–40`.) | [nao verificado]
   | **Selo sem mordida** | **Presente**, pelos mutantes específicos do item 4. (`diff:bin/tests/test_mapa20_valida.sh:53–56,115–123,151–164`.) |
   | **Ordem de trava** | Nenhuma aquisição nova. `codex_estado.sh --gerar` retorna antes da trava do ramo evento. (`base:bin/relato.sh:27`; `diff:bin/relato.sh:123`; `base:bin/codex_estado.sh:54–56,65–66`.) |
   | **Pouso antes do pré-requisito** | Contrato efetivo, mapa completo no ar, suíte e opus final [nao verificado]. Comentário apontando contrato não comprova seu conteúdo; a base exige contrato e suíte antes do opus. (`diff:bin/relato.sh:93`; `base:CLAUDE.md:377–381,394`.) | [nao verificado]
   | **Campo com dois significados** | **Presente na decisão de encerramento**: nome de dependência ou caminho é interpretado como estado da obra. `MAPA_RC` representa falha da etapa, incluindo cópia; `MAPA_PATH` inclui retirada e não prova publicação de mapa novo. (`diff:bin/mapa20_valida.py:141–142`; `diff:bin/relato.sh:94–100,150–164`.) |

## 11/11 11:48 · sessao 9f78342994b3 · - · - · origem=Ronald (tmux)

- **14 `exit` após o diff:** base `bin/relato.sh:27,28,32,35,38,42,86` → `1`; diff `bin/relato.sh:110,128,143,162,163` → `1`; diff `bin/relato.sh:157,164` → `$MAPA_RC`. Inicialização `0` em `diff:bin/relato.sh:94`, falha fixa `1` em `:100`; não há reset posterior, portanto validador vermelho executado não termina `0`.

- **Mutante FOTO sobrevivente, por inspeção:** inserir `MAPA_FOTO="$RAIZ/logs/MAPA-20/mapa.md"` imediatamente antes de `MAPA_PAR=...` em `diff:bin/relato.sh:98`. A validação ainda usa a foto, publicação usa o vivo. Os sete cenários não alteram a fonte entre esses pontos, e os greps apenas exigem presença textual das duas referências a `$MAPA_FOTO`. (`diff:bin/tests/test_mapa20_valida.sh:151–164`.)

- **Mutante de higiene sobrevivente, por inspeção:** remover o `&& mv` de `diff:bin/relato.sh:106`, ajustando a continuação da linha anterior. O stub só copia bytes idênticos e o teste espera exatamente `MAPA VELHO`; não distingue instalação da cópia raspada de preservação da cópia original. Em uso real, perde a troca do arquivo antigo pelo higienizado. (`diff:bin/tests/test_mapa20_valida.sh:115–123,127,144,153–154`.) [nao verificado]

- **Outro sobrevivente:** retirar a guarda de remoção fracassada em `diff:bin/relato.sh:110`; nenhum cenário injeta falha em `rm`. (`diff:bin/tests/test_mapa20_valida.sh:151–157`.)

- **Recorte literalmente vazio não dá selo verde:** os três `awk` estão em `diff:bin/tests/test_mapa20_valida.sh:105–107`; a guarda `:108–109` incrementa `FALHAS` se faltar a palavra exigida em qualquer recorte; a saída verde exige zero em `:168`. [nao verificado]

- Contagem em memória confirmou **169 linhas** no teste; cabeçalho do hunk correto. Execução integral dos testes e mutantes: **[nao verificado]**. [nao verificado]

## 11/11 11:47 · sessao 7f8232774c9a · FIO-E0-V4-E3 · sonda · origem=Code

```text
sonda_leitura: sonda_cel_apos_desl.py [fb90b86fb968e4b4] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

celulas_apos_desligamento_total n 4811
trabalha_true_apos_desligamento n 2546
colabs_distintos n 108
trabalha_true_passado_ate_hoje n 2110
trabalha_true_futuro n 436
por_origem 'gerada' n 2546
por_empresa 2 n 2092
por_empresa 3 n 454
por_mes_civil 2026-07 n 41
por_mes_civil 2026-08 n 230
por_mes_civil 2026-09 n 486
por_mes_civil 2026-10 n 1026
por_mes_civil 2026-11 n 642
por_mes_civil 2026-12 n 121
top_col col305 n 92
top_col col946 n 75
top_col col316 n 68
top_col col746 n 64
top_col col870 n 62
top_col col393 n 61
top_col col963 n 60
top_col col518 n 57
top_col col951 n 57
top_col col417 n 51
top_col col931 n 50
top_col col506 n 47
top_col col880 n 47
top_col col666 n 43
top_col col929 n 41
top_col col939 n 41
top_col col143 n 32
top_col col437 n 32
top_col col894 n 32
top_col col840 n 29
top_col col338 n 28
top_col col370 n 28
top_col col862 n 28
top_col col270 n 27
top_col col782 n 27
top_col col846 n 26
top_col col847 n 26
top_col col126 n 25
top_col col367 n 25
top_col col408 n 25
top_col col914 n 25
top_col col138 n 24
top_col col325 n 24
top_col col400 n 24
top_col col422 n 24
top_col col744 n 24
top_col col927 n 24
top_col col432 n 23
top_col col698 n 23
top_col col997 n 23
conferencia col318 n 10
conferencia col997 n 18
agora 2026-11-11 11:46:55
```

## 11/11 11:46 · sessao 44743de30430 · - · - · origem=Ronald (tmux)

Confirmado em Python em memória, sem criar arquivos:

- **Tabela invisível ainda aceita:** `<!--\n` + G + `-->\n` retorna lista de problemas vazia. G inteiramente indentada com quatro espaços também passa, embora seja bloco de código Markdown. O parser só veta cercas e aplica `strip()` às linhas. (`diff:bin/mapa20_valida.py:82–89`.)
- **Fonte inválida aceita:** `a.md:0` passa. A regex exige apenas aparência de citação; não valida linha positiva, existência nem pertencimento a aval/TICKETS/BACKLOG. (`diff:bin/mapa20_valida.py:44,134–135`.)
- **Fonte sem extensão recusada:** `BACKLOG:10` reprova pela regex. A validade concreta dessa referência como fonte numerada permanece [nao verificado]. (`diff:bin/mapa20_valida.py:44,134–135`.)
- **Falso vermelho de encerramento:** com `1 de 2`, células `nao encerrada` e `logs/encerrada.md:10` reprovam. A regex examina todas as células exceto o nome, sem distinguir status, negação ou caminho. O próprio nome `BO-ROTULO-ENCERRADO` fica corretamente excluído dessa busca. (`diff:bin/mapa20_valida.py:48,141–142`.)
- **Lista duplicada/contraditória aceita:** primeira seção `## PRONTO AUSENTE` com `- nenhuma`, seguida de segunda seção com `- O168`, e tabela sem ausentes: passa. Só a primeira seção é lida. (`diff:bin/mapa20_valida.py:58–77,104,146–148`.)
- **Mutante sobrevivente:** remover `re.I` da regex de encerramento preserva todos os casos do selo, escritos em minúsculas, mas aceita `ENCERRADA` com `1 de 2`. Sobrevivência por inspeção dos casos; suíte integral [nao verificado]. (`diff:bin/mapa20_valida.py:48`; `diff:bin/tests/test_mapa20_valida.sh:41,53–56`.) [nao verificado]

Fechamentos específicos da v2: universo literal corrigido; largura do separador, cabeçalho vazio e células só com pontuação barrados; cercas de crases/tils e casos 4/3 barrados. Permanecem, por decisão do formato fixo, as recusas a `_E3_`, tabela sem pipes externos e qualquer barra invertida. (`diff:bin/mapa20_valida.py:29–39,82–103,110–122`.)

## 11/11 11:44 · sessao 40e2b3668b76 · - · - · origem=Ronald (tmux)

Nenhuma quebra estática dos cinco selos vizinhos foi encontrada; execução integral deles fica [nao verificado], pois criam arquivos temporários.

- `test_codex_estado.sh`: recorta de `CODEX_OK=""` ao passo 3; o novo 2f termina antes disso. Portanto, o novo `exit 1` da guarda de remoção não entra no bloco proibido. (`base:bin/tests/test_codex_estado.sh:104–111`; `diff:bin/relato.sh:110,114,122–124`.)
- `test_relato_publica_claude_md.sh`: a regex da raspagem continua selecionando a chamada geral com CLAUDE; a nova re-raspagem de MAPA tem argumento depois de `$DEST` antes da continuação, não satisfazendo o prefixo dessa regex. O novo `git add` está indentado e não aumenta o número de linhas iniciadas por `git add -A `. CLAUDE permanece no pathspec. (`base:bin/tests/test_relato_publica_claude_md.sh:15–20`; `diff:bin/relato.sh:105–106,129–143,147,153,162`.)
- `test_codex_estado_evento.sh`: as chamadas de `codex_estado.sh` e `relato_nomes.sh` permanecem; o diff não introduz `gerar_codex_estado.py` inline. (`base:bin/tests/test_codex_estado_evento.sh:27–30`; `base:bin/relato.sh:32`; `diff:bin/relato.sh:123`.)
- `test_relato_afirma_com_prova.sh`: a chamada exigida não foi alterada. (`base:bin/tests/test_relato_afirma_com_prova.sh:43–45`; `base:bin/relato.sh:55–56`.)
- `test_publicacao_sem_nome.sh`: o universo continua vindo de `git ls-files` no público; MAPA rastreado é incluído automaticamente. Privacidade no público real: [nao verificado]. (`base:bin/tests/test_publicacao_sem_nome.sh:58–73`.)

Classes:

- **Segundo juiz:** a raspagem e sua conferência continuam usando `nome_no_texto`, sem novo predicado paralelo de nomes. (`base:bin/raspar_publico.py:15–16,31,36`; `diff:bin/relato.sh:105,129–143`.)
- **Segundo escritor fora da porta:** as duas escritas públicas novas estão dentro do publicador, depois da trava existente; escritor efetivo da fonte e outros escritores no ambiente: [nao verificado]. (`base:bin/relato.sh:27`; `diff:bin/relato.sh:97–114,129–143`.) [nao verificado]
- **Relógio fora da borda:** o diff não adiciona relógio decisório; `date` continua na mensagem de commit. O timer de cinco minutos já existe. (`diff:bin/relato.sh:161`; `base:bin/hasner-relato.timer:5–7`.) [nao verificado]
- **Ordem de trava:** nenhuma aquisição nova no diff. A rede `codex_estado.sh --gerar` retorna antes de adquirir a trava do ramo evento, evitando autoespera nesse caminho. (`base:bin/relato.sh:27`; `diff:bin/relato.sh:123`; `base:bin/codex_estado.sh:54–56,65–66`.)
- **Pouso antes do pré-requisito:** contrato efetivo, suíte completa e opus final: [nao verificado]. A base exige contrato antes do instrumento e suíte antes do opus; Codex não aprova sozinho o pouso. (`base:CLAUDE.md:377–381,394`; `diff:bin/relato.sh:93`.) [nao verificado]

Numeração confirmada do `relato.sh` após v3: o bloco novo ocupa **89–114**; `gerar_avais` fica em **128**; raspagem geral **129–143**; `git add` MAPA **153**; saídas normais **157/164**; commit com guarda **161–162**; push com guarda **163**. (`diff:bin/relato.sh:89–164`.) [nao verificado]

## 11/11 11:37 · sessao 70d2beb073f4 · FABLE-GABARITO · leitura · origem=Code

| caso | classe | arquivo do veredito opus | veredito em 1 linha | PROVA da resposta | PACOTE EXATO recebido | defeito real |
|---|---|---|---|---|---|---|
| TOTAL: 0 casos elegíveis | a: **0/6**; b: **0/3**; c: **0 comprovados** | — | Mínimos não atingidos | Nenhum candidato abaixo reúne pacote integral identificável e prova exigida; pacote obrigatório em `aval_gabarito_fable_1111.txt:1`. | Completude dos pacotes [nao verificado]; lacunas discriminadas em FORA. | — |

FORA

- **E3 08:3x horizonte** — REPROVADO; cura identificada: `2e9d534d8`, “E3 fix9 (copia): sem_adesao so do HORIZONTE_SEM_ADESAO 2026-11 em diante (opus final B1)”; prompt e diff exatos recebidos [nao verificado]. O `.raw` tem registros truncados e não permite recuperar o pacote. (`logs/FIO-E0-V4-E3/opus_final_e3_b1.txt:1-7`; `logs/FIO-E0-V4-E3/opus_final_e3.raw:1-12`.)
- **BO-0611-05 opus_final2 0.0** — o veredito é **APROVA**, com o problema dos mapas já registrado; cura posterior `fdf9fc05b`, “BO-0611-05 volta 8: modal_fio e _relatorio_geo perguntam ao juiz tem_gps (0.0 nao desenha mapa)”; prompt/diff exatos [nao verificado]. (`logs/BO-0611-05/opus_final2.txt:2-3`.)
- **BO-0611-05 opus_final1** — REPROVADO sobre `4ffb05150`; pacote do opus [nao verificado]: os nomes visíveis de censo não identificam sua pergunta final nem seu diff. (`logs/BO-0611-05/opus_final1.txt:1`.)
- **BO-0611-05 opus_final3** — APROVADO sobre `fdf9fc05b`, pouso `fb9269ea9`; pacote exato [nao verificado] e janela de 48 h ainda incompleta no gitlog fornecido. (`logs/BO-0611-05/opus_final3.txt:1-4`; `gitlog_14d.txt`, commit `fb9269ea9`.)
- **E4a-1** — REPROVADO sobre diff `f2962ab8d..629f2d244`; cura `5578212ef`, “FIO-E0-V4-E4 E4a-1: curas P1-P5 da 1a reprovacao do opus final (verificador)”; arquivo do prompt e diff salvo exatos [nao verificado]. (`logs/FIO-E0-V4-E4-ESCRITORES/opus_final_e4a1_1.txt:1-3`.)
- **E4A-2 final1** — REPROVADO sobre `11f82cfec`; pacote final integral [nao verificado]. (`logs/FIO-E0-V4-E4A-2/opus_final1.txt:1`.)
- **TRAVA-SELO v11** — APROVADO sobre `04ec2b8b0`, pouso `9ca563bb3`; `diff_v1.txt` e `preverif_v1.q` não demonstram o pacote final v11; tampouco há 48 h posteriores no gitlog. (`logs/TRAVA-SELO/opus_final_v11.r:1`; `gitlog_14d.txt`, commit `9ca563bb3`; pacote v11 [nao verificado].)
- **VERIFICADOR-EM-FABLE v3** — REPROVA sobre `238cd5cdf`; cura `5ab7b0ff3`, “VERIFICADOR-EM-FABLE (1) volta 4: a negativa sem obra tem prefixo proprio B3-SEM-OBRA”; `c1.diff` e `preverif1.q` não identificam o pacote final v3. (`logs/VERIFICADOR-EM-FABLE/opus_final_v3.txt:1-2`; pacote v3 [nao verificado].)
- **L-123-MEDIDA-EM-PY** — REPROVADO sobre `7a2e3bc5`; aparecem `diff.patch` e `v1.q`, mas sua vinculação ao pedido final e a relação completa de anexos [nao verificado]. (`logs/akmed/opus_final.txt:1`.)
- **AUDITORIA v5** — bloqueio sobre `500bbdde3`; pergunta/diff finais e commit da cura seguinte [nao verificado]. (`logs/auditoria_0911/opus_final_v5.txt:1-2`.)
- **C20-FIO-MUDO final v5** — REPROVA sobre `5f3ea8c2`; as três entradas reproduzem o mesmo laudo, sem demonstrar pacote final integral ou commit da cura seguinte. (`logs/c20_fiomudo/codex/opus_final_v5_achados.txt:1`; `logs/c20_fiomudo/opus_final_1d78ef7f/opus_final_v5_achados.txt:1`; `logs/c20_fiomudo/opus_final_3ee00412/opus_final_v5_achados.txt:1`; pacote/cura [nao verificado].)
- **C20-O169** — REPROVA sobre `6afa8aa1/e76e0419`; cura `e13209863`, “O169 volta opus: L-125 CLUSTER-NAO-FECHA-POR-IDADE entra no LEIS.md + ADR (bloqueante do opus final)”; pacote final exato [nao verificado]. (`logs/c20_o169/opus_final_1.txt:1-2`.)
- **COBPARC v4** — REPROVADO; o laudo distingue `113fd774` de `129bfeaa`, e suas duas cópias não identificam o pacote recebido integralmente. (`logs/cobparc/v4/opus_final.md:1-6`; `logs/cobparc/v4/pm/opus_final_achados.md:1-6`; pacote [nao verificado].)
- **COBPARC v7** — REPROVADO sobre `8f21ec29`; cura `d03410912`, “[COBERTURA-PARCIAL-UM-JUIZ] v8 achados 1 e 2 (opus final v7): o leitor da ata transporta justificado; o vinculo volta a contar”; prompt/diff finais exatos [nao verificado]. (`logs/cobparc/v7/opus_final.txt:1-4`.)
- **COBPARC v8** — REPROVADO sobre `f05713f5`, com amend `72eb11be` relatado; pacote exato e assunto do commit de cura no gitlog [nao verificado]. (`logs/cobparc/v8/opus_final.txt:1-3`.)
- **COBPARC-FILA-RELOGIO v1** — REPROVADO sobre `69a6a682`; pergunta/diff finais e commit da volta seguinte que cite o achado [nao verificado]. (`logs/cobparc_relogio/opus_final.txt:1-3`.)
- **COBPARC-FILA-RELOGIO v2** — REPROVADO sobre `12200105`; pacote exato e commit de cura seguinte [nao verificado]. (`logs/cobparc_relogio/opus_final.txt:10-11`.)
- **CODEX-CONSTRUTOR inicial** — REPROVA sobre `d63b837f`; a cura aparece em `bbdbceb91`, “CODEX-CONSTRUTOR-NEGOCIO volta 4: o inventario do construtor varre o FS da copia e .git em qualquer nivel e PROIBIDO”; porém, o prompt corta o nome do anexo em `logs/codexneg/mutantes_selo`. Completar esse nome pelo veredito reconstruiria o pacote. (`logs/codexneg/opus_final_r.md:1`; `logs/codexneg/opus_final_prompt.txt:3`; identidade integral do anexo [nao verificado].)
- **CODEX-CONSTRUTOR v4** — REPROVA sobre `9995e2a6`; cura `cf6360832`, “CODEX-CONSTRUTOR-NEGOCIO volta 5 (opus final v4 1b): todo python3 do wrapper sob -I”; prompt, diff e delta estão nomeados, mas a relação de triagens está truncada após `logs/codex`. (`logs/codexneg/opus_final_v4_r.md:1-3`; `logs/codexneg/opus_final_v4_prompt.txt:3`; pacote integral [nao verificado].)
- **CODEX-CONSTRUTOR v5** — APROVA sobre `37880aa1`, pouso `a1d70f619`; o catálogo mostra somente a cabeça de 12 das 20 linhas do prompt, com referências truncadas: completude dos anexos [nao verificado]. (`logs/codexneg/opus_final_v5_r.md:1`; `logs/codexneg/opus_final_v5_prompt.txt:6-8`.)
- **CONTENCAO v2/p1** — REPROVADO sobre `3a743cdd`; pacote final integral e commit da cura seguinte [nao verificado]. (`logs/contencao/v2/p1/opus_final.md:1-3`.)
- **CRONS-INSTALL-SOB-MARCA** — APROVADO sobre `073d840f`, pouso `3ce6f52b5`; pergunta/diff exatos do opus [nao verificado]. (`logs/cronsmarca/opus_final_1.txt:1`; `gitlog_14d.txt`, commit `3ce6f52b5`.)
- **CXR-TELA final1** — REPROVA sobre `71c91c97`; cura `bab61829f`, “CXR-TELA-LE-O-MOTOR volta 4 (opus final 1 REPROVA 71c91c97): selo C3 no caminho C11 e relogio na borda”; pacote final exato [nao verificado]. (`logs/cxr_tela/opus_final_71c91c97.md:1-4`.)
- **CXR-TELA final2** — APROVA sobre `6a5e5ad8`, pouso `94d41ea56`; pergunta/diff finais exatos [nao verificado]. (`logs/cxr_tela/opus_final_6a5e5ad8.md:1`; `gitlog_14d.txt`, commit `94d41ea56`.)
- **E4-CALENDARIO-L1** — REPROVADO sobre `15d4f10b`; há três perguntas nomeadas, sem identificação verificável de qual foi recebida e de todos os anexos/diffs. (`logs/e4cal/postmortem_opus/opus_final.txt:2`; pacote [nao verificado].)
- **E-SEM-VIVA-B final sobre 97411e5c** — REPROVA; cura `3be9c5423`, “E-SEM-VIVA-B volta 4: a C5 do desligado le a LAVRA do juiz _K2 (RetratoFolha) e nao recompoe rescisao + retentor”; pacote final exato [nao verificado]. (`logs/esemvivab/opus_final_97411e5c.md:1-3`.)
- **E-SEM-VIVA-B final sobre 1936c9d2** — APROVA, pouso `40a005457`; pergunta/diff finais exatos [nao verificado]. (`logs/esemvivab/opus_final_1936c9d2.md:1-2`; `gitlog_14d.txt`, commit `40a005457`.)
- **F3-BECO-1 final1** — REPROVADO sobre `e2e3d901`; cura `f10f8328a`, “F3-BECO-1 retrabalho: B1+F1+F5+F6 do laudo opus_final_1 (C11 em lote, dono unico do dia)”; pacote final exato [nao verificado]. (`logs/f3_beco1/opus_final_1.txt:1-2`.)
- **F3-BECO-1 final2** — APROVA o código sobre `59bbe2e3`, pouso `9c0fd9d3e`; pacote final exato [nao verificado]. (`logs/f3_beco1/opus_final_2.txt:1-3`; `gitlog_14d.txt`, commit `9c0fd9d3e`.)
- **E1 final sobre afb67623** — REPROVA; cura `6e53526de`, “FIO-E0-V4-E1 volta 12 (opus final 1 REPROVA afb67623, reprova 1 de 3, PISTA-LIVRE): S143 so trava quando o save ESCREVE o status”; pacote exato [nao verificado]. (`logs/fio_e0_v4/opus_final_e1_afb67623.md:1`.)
- **E1 final2 sobre 6695e972** — REPROVADO; pergunta/diff finais exatos [nao verificado]. (`logs/fio_e0_v4/opus_final2_e1_6695e972.md:1-2`.)
- **E1 final3 sobre 76644dc8** — REPROVADO; cura `d73d84a92`, “FIO-E0-V4-E1 volta 15d (opus final 3 reprovou 76644dc8): o reuso de abrir re-chaveia pela porta unica”; pacote exato [nao verificado]. (`logs/fio_e0_v4/opus_final3_e1_76644dc8.md:1-3`.)
- **E1 final4 sobre b0c4297c** — REPROVADO; pergunta/diff finais exatos [nao verificado]. (`logs/fio_e0_v4/opus_final4_e1_b0c4297c.md:1-2`.)
- **DEPLOY-PORTAO final5** — APROVA o intervalo `a5df8ed5..c24a1914`; pacote final exato [nao verificado], e as voltas posteriores `e04870002`/`20df97548` impedem presumir aprovação limpa desse intervalo. (`logs/fio_e0_v4/opus_final_deployportao5.md:1`; `gitlog_14d.txt`, commits `e04870002`, `20df97548`.)
- **E2 final sobre 3b853907** — REPROVADO; pergunta/diff finais exatos [nao verificado]. (`logs/fio_e0_v4/opus_final_e2.txt:1-6`.)
- **E2 final sobre d912345b** — REPROVA; `e2_opus_final_c2833d85_d912345b.diff` está nomeado, mas pergunta final e conjunto integral dos anexos [nao verificado]. (`logs/fio_e0_v4/opus_final_e2_d912345b.md:1-3`; `logs/fio_e0_v4/e2_opus_final_c2833d85_d912345b.diff:1`.)
- **GRADE-v5** — REPROVADO sobre `391b74dc`; pergunta/diff finais exatos [nao verificado]. (`logs/grade_folga/v5/pm/opus_final.txt:1`.)
- **GRADE-FOLGA-JUIZ sobre abd37234** — REPROVADO; `pv/` e `v10/` contêm cópias do mesmo laudo, não dois casos; pacote final exato [nao verificado]. (`logs/grade_juiz/pv/opus_final.txt:1`; `logs/grade_juiz/v10/opus_final.txt:1`.)
- **GRADE-FOLGA-JUIZ va** — REPROVADO sobre `d9cb0e42`; cura `60258b809`, “GRADE-FOLGA-JUIZ vb: cura dos 3 achados do opus final (prova e documento; codigo do juiz intocado)”; o brief identifica o ensaio por diretório, sem enumerar cada arquivo recebido. (`logs/grade_juiz/va/opus_final_achados.md:1-4`; `logs/grade_juiz/va/opus_final_brief.md:7`; pacote integral [nao verificado].)
- **GRADE-FOLGA-JUIZ vb** — brief identifica `60258b80`, diff e delta, mas arquivo do veredito final vb e commit do pouso no gitlog fornecido [nao verificado]. (`logs/grade_juiz/vb/opus_final_brief.md:1-3`.)
- **H12H06-B sobre 4e9380e5** — REPROVA; cura `90747d3a7`, “H12H06-B volta 5: editar_ausencia_registro vence a projecao do range antigo e do novo; o GRAVADO afirma a regra no DSR”; pacote final exato [nao verificado]. (`logs/h12h06b/opus_final_4e9380e5.md:1-3`.)
- **H12H06-B-PAUTA-08 v7** — APROVADO sobre `dbabd2ad`, pouso `967d48211`; pergunta/diff finais exatos [nao verificado]. (`logs/h12h06b/pauta08/v7/opus_final_dbabd2ad.md:1`; `gitlog_14d.txt`, commit `967d48211`.)
- **JUIZ-1 v5** — APROVADO sobre `d38e6b91`, pouso `630131b1f`; pacote final exato [nao verificado]. (`logs/jv1/opus_final_v5.txt:1`; `gitlog_14d.txt`, commit `630131b1f`.)
- **JUIZ-1 resolução de merge** — APROVADO sobre `ed9cd506 + c4525ff4`; pergunta/diff finais exatos e correspondência com o commit de pouso [nao verificado]. (`logs/jv1/opus_final_merge.txt:1`.)
- **LEN-VIVOS** — APROVADO sobre `f71d570e`, pouso `f9b89feaa`; pacote final exato [nao verificado]. (`logs/lenvivos/v2/opus_final.txt:1-2`; `gitlog_14d.txt`, commit `f9b89feaa`.)
- **MOTOR-LE-O-TURNO-INSTRUMENTO final2** — APROVA sobre `bc9db924`, pouso `935c8d961`; pacote final exato [nao verificado]. (`logs/motor_turno/piloto/opus_final2.md:1`; `gitlog_14d.txt`, commit `935c8d961`.)
- **O261-E-F2 final1** — REPROVADO sobre `7882a6e6`; cura `e9690d0c1`, “[O261-E-F2] volta 4: quem chama a porta do carimbo (selo AST), testemunha viva da classe (f) e DOSSIES:46”; pacote final exato [nao verificado]. (`logs/o261e/f2/opus_final_v1.md:1-2`.)
- **O261-E-F2 final2** — APROVADO sobre `5c2bb8a4`; pacote final exato e commit do pouso correspondente no gitlog [nao verificado]. (`logs/o261e/f2/opus_final_v2.md:1-2`.)
- **PADRAO-DE-CIMA final** — REPROVADO sobre `b3c0f8ec`; pergunta/diff finais exatos e commit da cura seguinte [nao verificado]. (`logs/padrao_de_cima/v6/r9/opus_final.txt:1-2`.)
- **PAR-NAO-ADJACENTE final1** — REPROVA sobre `795cafdf`; pacote final exato e commit da volta seguinte que cite o achado [nao verificado]. (`logs/par_nao_adjacente/opus_final_1.txt:1-2`.)
- **PAR-NAO-ADJACENTE final2** — APROVA o diff `d361dfee`; pergunta/diff salvos exatos e commit do pouso no gitlog [nao verificado]. (`logs/par_nao_adjacente/opus_final_2.txt:1`.)
- **BUG-PLANTIO-NOTURNO-MAIS-1 final** — BLOQUEIA; o laudo nomeia `final.diff` e `f1.r`, mas não a pergunta final salva nem o pacote integral; commit avaliado [nao verificado]. (`logs/plantio_mais1/opus_final.txt:1-4`, `:11`.)
- **SELOS-CANON-I1 final1** — REPROVA sobre `f0103befd`; cura `f56dc9628`, “SELOS-CANON-I1 volta 5 (construtor): pertenca da fila 1 pelo TOKEN da celula nas duas vias”; pacote final exato [nao verificado]. (`logs/selos_canon/i1_opus_final.md:1-2`.)
- **SELOS-CANON-I1 final2** — REPROVA sobre `788ae3e28`; cura `d1502c4ae`, “SELOS-CANON-I1 volta 6 (construtor opus): o espelho da pertenca da fila 1”; pacote final exato [nao verificado]. (`logs/selos_canon/i1_opus_final_2.md:1-2`.)
- **SELOS-CANON-I1 final3** — APROVADO, pouso `3803e6623`; pergunta/diff finais exatos [nao verificado]. (`logs/selos_canon/i1_opus_final_3.md:1`; `gitlog_14d.txt`, commit `3803e6623`.)
- **SNC2 final1** — APROVA sobre `e3ef579a`, pouso `ad28270d8`; pergunta/diff finais exatos [nao verificado]. (`logs/snc2/opus_final_1.txt:1-2`; `gitlog_14d.txt`, commit `ad28270d8`.)
- **SOMBRA (c)(1)+motor feriado** — APROVADO sobre `f40d356e+47691f0b`, pouso `0d234d122`; pacote final exato [nao verificado]. (`logs/sombra_c1/opus_final_1.txt:1`; `gitlog_14d.txt`, commit `0d234d122`.)
- **V2-BUGS-DO-MOTOR i2 final1** — REPROVADO sobre `2cb3a055`; pergunta/diff finais exatos [nao verificado]. (`logs/v2bugs/i2/opus_final.txt:1-5`.)
- **V2-BUGS-DO-MOTOR i2 final v6** — APROVADO sobre `a23831c3`, pouso `ffbda473e`; pergunta/diff finais exatos [nao verificado]. (`logs/v2bugs/i2/opus_final_v6.txt:1-2`; `gitlog_14d.txt`, commit `ffbda473e`.)
- **V3-P5 final** — REPROVADO sobre `c71b1038`; pergunta/diff finais exatos [nao verificado]. (`logs/v3p5/pv/opus_final.txt:1-8`.)

## 11/11 11:35 · sessao b1f0518d314b · - · - · origem=Ronald (tmux)

| candidato | motivo de exclusão |
|---|---|
| CODEXNEG inicial | O prompt e o diff estão nomeados, mas a referência ao anexo de mutantes termina em `logs/codexneg/mutantes_selo`; a identidade completa desse anexo recebido está [nao verificado]. `logs/codexneg/opus_final_prompt.txt:3`. |
| CODEXNEG v4 | O prompt identifica diff e delta, mas a relação de triagens termina truncada em `logs/codex`; pacote integral [nao verificado]. `logs/codexneg/opus_final_v4_prompt.txt:3`. |
| CODEXNEG v5 | O catálogo mostra apenas 12 das 20 linhas do prompt, com referências truncadas; não permite certificar todos os anexos recebidos. `logs/codexneg/opus_final_v5_prompt.txt:6`, `:8`; completude [nao verificado]. |
| GRADE va | Os nomes relativos `v1.r`, `v2.r`, `v1_triagem.md` são utilizáveis literalmente; porém, o ensaio é identificado por diretório, sem enumeração dos arquivos recebidos. `logs/grade_juiz/va/opus_final_brief.md:5`, `:7`; apenas `fase_C.txt` aparece em `:12`; pacote integral [nao verificado]. |
| GRADE vb | Há prompt e diffs nomeados, mas nenhum arquivo de veredito final vb é apresentado no catálogo. `logs/grade_juiz/vb/opus_final_brief.md:1`, `:3`; arquivo do veredito [nao verificado]. |

Zero desses cinco casos têm pacote e veredito integralmente certificáveis pelos recortes fornecidos; isso não afirma inexistência física dos arquivos. [nao verificado]

## 11/11 11:35 · sessao be6027cf7941 · FIO-E0-V4-E4B · construtor · origem=Code

Restaurei a releitura por disputa no C7, mantendo o memo das cargas e o N1 exigindo **2 leituras**. [disputa_emissao.py:2076](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/services/disputa_emissao.py:2076)

A perda ocorreu na **c4v5**; a c4v6 acrescentou a semente do E2. O memo podia divergir após reabertura do período. A cadeia completa, escritores e leitores estão no [recibo:8](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4-v14.txt:8).

Validação: RED na base, três mutantes falhando pela asserção comportamental, 94 testes de memória/protocolo e 40 selos passando. [recibo:105](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4-v14.txt:105) [nao verificado]

O comando ORM completo está no [recibo:149](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4-v14.txt:149). ORM, tetos C8 e suite integral: **[nao verificado]**. [nao verificado]

Sem commit, respeitando a proibição inicial de commit e escrita em `.git*`. [recibo:158](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4-v14.txt:158) [nao verificado]

_3820 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
