# CODEX-ESTADO

> Ultima resposta processada: **15/11/2026 10:35** · publicado em **15/11/2026 10:36** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 15/11 10:35 · sessao 3a515ace382d · - · - · origem=Ronald (tmux)

Use `T = app/templates/ponto/bater_ponto_v2.html`.

- **NÃO PROCEDENTE — aviso persistente após selfie no fluxo normal.** O handler só aceita arquivo existente, grava `_fotoBlob` e repinta; o ramo com blob apaga o aviso, insere a imagem e troca o chip conjuntamente (`T:269-271`, `T:255-261`).
- **NÃO PROCEDENTE — cancelar a segunda seleção perde a primeira selfie.** `if(!f)return` preserva `_fotoBlob`, URL, imagem e chip anteriores; cancelamento não significa “apagar selfie” nesse contrato (`T:270-271`).
- **NÃO PROCEDENTE — segunda selfie conserva imagem anterior.** Primeiro revoga a URL anterior, cria URL do novo blob e remove o filho antigo por `innerHTML=''` antes de anexar o novo (`T:254-261`).
- **NÃO PROCEDENTE — prévia com `_fotoBlob` nulo por algum escritor normal.** A imagem só é criada sob `if(_fotoBlob)`; os escritores são inicialização nula, arquivo recebido e captura com preservação do blob anterior; nenhum escritor posterior limpa o blob (`T:146`, `T:255-260`, `T:271`, `T:305`).
- **Limite PROCEDENTE — blob existente não garante imagem decodificável.** O handler não verifica conteúdo nem decodificação, e a pintura marca “selfie pronta” antes de `load`, sem `error` handler; um arquivo inválido pode dar imagem quebrada com chip pronto. Existência desse caso em produção: **[não verificado]** (`T:90`, `T:270-271`, `T:256-261`).
- **NÃO PROCEDENTE — fluxo ao vivo passa a depender da prévia.** Sucesso de `getUserMedia` continua exibindo vídeo, escondendo placeholder e marcando câmera ao vivo; `capturarFrame` continua lendo vídeo quando há largura e usando `_fotoBlob` quando não há quadro (`T:238-240`, `T:277-285`). Uma câmera que pare depois de ter funcionado não tem observador de tracks/error para corrigir o chip; esse limite já pertence ao fluxo ao vivo, não à pintura nova (`T:235-245`, `T:151`).
- **NÃO PROCEDENTE — bfcache conserva indefinidamente selfie antiga por decisão desta tela.** `pageshow.persisted` pede reload; retorno visível após mais de cinco minutos também pede reload; nova execução inicializa `_fotoBlob=null` e markup inicial. Comportamento efetivo no iPhone/PWA e restauração de campos pelo navegador: **[não verificado]** (`T:319-324`, `T:146`, `T:87-90`).
- **Observação PROCEDENTE — prévia posterior não identifica a foto já salva offline.** Os dois ramos offline escondem apenas o botão, mantendo input/quadro interativos; selecionar outra selfie muda `_fotoBlob` e prévia, enquanto a fila conserva a foto passada no registro anterior. Pode haver f2 visível junto do aviso “Batida registrada localmente” referente a f1; isso não demonstra mudança do envio pelo diff (`T:269-271`, `T:352-357`, `T:365-370`; `app/static/pwa.js:107-118`; aviso em `T:122-126`).
- **NÃO PROCEDENTE — `v2Sucesso` deixa chip divergente visível.** Ele para tracks e cobre a página com overlay antes do redirecionamento; não limpa estado interno nem URL. O vídeo/chip subjacentes podem ficar congelados, mas o overlay os cobre (`T:150-157`, chamada `T:346`).

Item 3:

- **PROCEDENTE apenas na ordem literal — URL revogada ainda referenciada pelo `<img>`.** Revogação em `T:254` precede remoção da imagem em `T:260`. Isso ocorre na mesma execução síncrona; imagem quebrada persistente no caminho normal não está demonstrada. Uma exceção entre essas linhas poderia conservar a referência antiga; ocorrência concreta: **[não verificado]** (`T:254-260`).
- **NÃO PROCEDENTE — acúmulo de `<img>` a cada selfie.** `ph.innerHTML=''` remove a imagem anterior; a URL anterior também é revogada (`T:254`, `T:260`).
- **PROCEDENTE — falta cleanup explícito da última URL.** Há revogação somente na próxima pintura; `v2Sucesso`, `pageshow` e `visibilitychange` não revogam nem limpam a última imagem. É retenção durante a vida do documento, não prova de vazamento ilimitado (`T:254`, `T:150-157`, `T:319-324`).
- **NÃO PROCEDENTE — declaração tardia de `var` provoca corrida com `abrirCamera`.** A inicialização em `T:251` acontece antes do registro do listener em `T:273`; declarar a função antes não a executa (`T:235`, `T:251`, `T:273`).
- **NÃO PROCEDENTE — `innerHTML` novo recebe valor não confiável.** `_camNegada` apenas seleciona entre duas strings constantes; nome, conteúdo e nome de arquivo não são concatenados no HTML; a imagem é criada por DOM e recebe URL gerada (`T:242`, `T:256-266`).

## 15/11 10:35 · sessao c0a8833e7ae8 · - · - · origem=Ronald (tmux)

Item 4 — **MORDE parcialmente; há lacunas procedentes, sem asserção globalmente vazia.** A conclusão abaixo é leitura estática da sonda; execução e resultado verde/RED **[não verificado]**. O teste criaria e removeria diretório temporário, e o helper escreveria HTML/symlink/cópia; não foi executado para preservar a regra de somente leitura (`app/ponto/tests/test_camera_alternativa_previa.py:163`, `app/ponto/tests/test_camera_alternativa_previa.py:167`, `app/core/tests/test_smoke_chromium.py:94`, `app/core/tests/test_smoke_chromium.py:100`).

| Asserção | O que alcança; mutante plausível |
|---|---|
| `assertIsNotNone(chromium())` | Cobra executável presente, sem skip; não prova que JS executou (`app/ponto/tests/test_camera_alternativa_previa.py:158`, `app/core/tests/test_smoke_chromium.py:76`). |
| `assertIn('id="v2-cam-fallback"', html)` | Cobra presença literal no HTML renderizado; sozinha aceitaria elemento inutilizável, mas a sonda cobra acesso DOM e change (`app/ponto/tests/test_camera_alternativa_previa.py:162`, `app/ponto/tests/test_camera_alternativa_previa.py:80`, `app/ponto/tests/test_camera_alternativa_previa.py:91`). |
| `assertIsNotNone(veredito)` | Cobra marcador emitido pela sonda; ausência de JS/veredito falha, sem verde por silêncio (`app/ponto/tests/test_camera_alternativa_previa.py:66`, `app/ponto/tests/test_camera_alternativa_previa.py:168`, `app/core/tests/test_smoke_chromium.py:123`). |
| `assertNotIn('erro', r)` | Cobra elemento presente, aviso inicial e ausência das exceções capturadas; aviso inicial é somente `textContent`, não visibilidade (`app/ponto/tests/test_camera_alternativa_previa.py:82`, `app/ponto/tests/test_camera_alternativa_previa.py:84`, `app/ponto/tests/test_camera_alternativa_previa.py:125`, `app/ponto/tests/test_camera_alternativa_previa.py:176`). |
| `aviso=False` | Remove palavras do aviso do texto de todo quadro; trocar somente texto poderia passar esta parcela, mas falharia as parcelas de imagem (`app/ponto/tests/test_camera_alternativa_previa.py:77`, `app/ponto/tests/test_camera_alternativa_previa.py:99`, `app/ponto/tests/test_camera_alternativa_previa.py:131`). |
| `previa=True` | Exige primeiro `img` do pai de `ph` com URL `blob:`; não exige imagem descendente de `#v2-cam-ph` nem id `v2-cam-previa` (`app/ponto/tests/test_camera_alternativa_previa.py:83`, `app/ponto/tests/test_camera_alternativa_previa.py:94`, `app/ponto/tests/test_camera_alternativa_previa.py:100`). |
| `decodificada=True` | Exige imagem completa com largura natural 4; mata simples texto e imagem quebrada, mas não compara bytes, pixels nem altura (`app/ponto/tests/test_camera_alternativa_previa.py:89`, `app/ponto/tests/test_camera_alternativa_previa.py:95`, `app/ponto/tests/test_camera_alternativa_previa.py:101`). |
| `area=True` | Cobra área geométrica positiva; não verifica `visibility`, opacidade ou oclusão da imagem (`app/ponto/tests/test_camera_alternativa_previa.py:102`, `app/ponto/tests/test_camera_alternativa_previa.py:103`). |
| `toque=True` | Faz hit test real no centro de `ph` e cobra uma chamada ao `inp.click` substituído; mata perda do caminho de clique nesse ponto, mas não exige propriedade `ph.onclick`, nem abertura real da câmera do sistema (`app/ponto/tests/test_camera_alternativa_previa.py:87`, `app/ponto/tests/test_camera_alternativa_previa.py:104`, `app/ponto/tests/test_camera_alternativa_previa.py:108`). |
| `troca=True` | Exige URL blob diferente na segunda imagem; mata prévia estagnada, mas não prova local DOM ou conteúdo por si só (`app/ponto/tests/test_camera_alternativa_previa.py:109`, `app/ponto/tests/test_camera_alternativa_previa.py:113`). |
| `revoga=True` | Mata omissão da revogação anterior; não examina ordem relativa à retirada da imagem, nem se a URL nova também foi revogada indevidamente (`app/ponto/tests/test_camera_alternativa_previa.py:85`, `app/ponto/tests/test_camera_alternativa_previa.py:86`, `app/ponto/tests/test_camera_alternativa_previa.py:114`). |
| `decodificada2=True` | Cobra largura natural 8 e imagem completa; não compara bytes/altura e não mede área nem clique depois da segunda selfie (`app/ponto/tests/test_camera_alternativa_previa.py:110`, `app/ponto/tests/test_camera_alternativa_previa.py:115`). |
| `aviso2=False` | Cobra ausência textual depois da segunda selfie; novamente não inspeciona chip ou visibilidade (`app/ponto/tests/test_camera_alternativa_previa.py:116`). |
| `envio=True` | Cobra exatamente um POST anotado, foto presente e igualdade de tamanho/tipo com a segunda selfie; mata ausência de foto/POST e primeira foto com tamanho diferente, mas não compara conteúdo (`app/ponto/tests/test_camera_alternativa_previa.py:60`, `app/ponto/tests/test_camera_alternativa_previa.py:62`, `app/ponto/tests/test_camera_alternativa_previa.py:118`, `app/ponto/tests/test_camera_alternativa_previa.py:120`). |
| `assertEqual(r, ESPERADO)` | Cobra todos os dez resultados e conjunto exato de chaves; não há passe apenas por ausência do aviso (`app/ponto/tests/test_camera_alternativa_previa.py:131`, `app/ponto/tests/test_camera_alternativa_previa.py:177`). |

**PROCEDENTE — alcance mais amplo anunciado que o efetivamente medido:** uma prévia irmã de `ph`, dentro do quadro e posicionada fora do centro clicado, com `ph` ainda abrindo input, poderia passar; a busca é pelo pai e o clique por um ponto de `ph`, sem verificar descendência ou `onclick` da imagem (`app/ponto/tests/test_camera_alternativa_previa.py:83`, `app/ponto/tests/test_camera_alternativa_previa.py:94`, `app/ponto/tests/test_camera_alternativa_previa.py:106`). O chip também não é consultado pela sonda (`app/ponto/tests/test_camera_alternativa_previa.py:80`, `app/ponto/tests/test_camera_alternativa_previa.py:120`).

**PROCEDENTE — identidade da segunda foto insuficiente:** um mutante que envia outro blob de mesmo tamanho e MIME passa `envio`; não há comparação dos bytes da segunda foto (`app/ponto/tests/test_camera_alternativa_previa.py:120`). O mutante simples “guardar primeira foto e mostrar segunda” é detectado se os PNG gerados tiverem tamanhos diferentes; os tamanhos efetivos nesta execução **[não verificado]** (`app/ponto/tests/test_camera_alternativa_previa.py:70`, `app/ponto/tests/test_camera_alternativa_previa.py:95`, `app/ponto/tests/test_camera_alternativa_previa.py:110`).

**NÃO PROCEDENTE — sonda tentando ler variável privada inexistente:** `_fotoBlob` está no IIFE, mas a sonda chama a função global `window.v2Confirmar`, que usa o fecho, passa por `garantirRequisitos`, cai no fallback de `capturarFrame` quando vídeo não tem quadro e coloca `_fotoBlob` em `FormData`; o fetch interceptado observa esse valor por um leitor válido (`app/templates/ponto/bater_ponto_v2.html:138`, `app/templates/ponto/bater_ponto_v2.html:146`, `app/templates/ponto/bater_ponto_v2.html:278`, `app/templates/ponto/bater_ponto_v2.html:305`, `app/templates/ponto/bater_ponto_v2.html:326`, `app/templates/ponto/bater_ponto_v2.html:339`, `app/ponto/tests/test_camera_alternativa_previa.py:62`).

**NÃO PROCEDENTE — apenas uma casca no código do teste:** percorre `base.html` e `base_app.html`, remapeia o nome `base.html` no loader locmem para o conteúdo da casca escolhida, e renderiza o template que estende esse nome; ambas têm bloco `content`, que inclui o script da página (`app/ponto/tests/test_camera_alternativa_previa.py:38`, `app/ponto/tests/test_camera_alternativa_previa.py:141`, `app/ponto/tests/test_camera_alternativa_previa.py:148`, `app/ponto/tests/test_camera_alternativa_previa.py:172`, `app/templates/ponto/bater_ponto_v2.html:1`, `app/templates/ponto/bater_ponto_v2.html:3`, `app/templates/base.html:336`, `app/templates/base_app.html:94`). A seleção real de casca por sessão/rota não é exercitada: usa renderização direta e `AnonymousUser` (`app/ponto/tests/test_camera_alternativa_previa.py:148`, `app/ponto/tests/test_camera_alternativa_previa.py:150`).

**PROCEDENTE — não há prova de mutação RED neste teste:** o arquivo tem cenário verde esperado, mas não reinjeta cura defeituosa nem exige falha de mutante; o nome `test_MORDE` não acrescenta essa execução (`app/ponto/tests/test_camera_alternativa_previa.py:171`, `app/ponto/tests/test_camera_alternativa_previa.py:177`). Diferentemente, o helper smoke possui outro teste que injeta defeito explicitamente, mas esse teste não é chamado pelo teste da câmera (`app/core/tests/test_smoke_chromium.py:191`, `app/core/tests/test_smoke_chromium.py:212`, `app/ponto/tests/test_camera_alternativa_previa.py:35`).

**PROCEDENTE — caminhos ausentes da cobertura:** a câmera sempre rejeita, rede sempre é marcada online e o POST recebe HTTP 599; logo esse caso não cobre câmera funcionando, offline/fila, sucesso, cancelamento, pageshow/bfcache ou reload (`app/ponto/tests/test_camera_alternativa_previa.py:48`, `app/ponto/tests/test_camera_alternativa_previa.py:57`, `app/ponto/tests/test_camera_alternativa_previa.py:63`, `app/ponto/tests/test_camera_alternativa_previa.py:88`, `app/ponto/tests/test_camera_alternativa_previa.py:118`). Os erros de console retornados pelo helper são descartados (`app/ponto/tests/test_camera_alternativa_previa.py:165`, `app/core/tests/test_smoke_chromium.py:124`).

## 15/11 10:35 · sessao af0477d00d94 · - · - · origem=Ronald (tmux)

**Item 5 — classes da casa**

| Classe | Veredito sobre o diff | Evidência |
|---|---|---|
| Segundo juiz registrado | **NÃO PROCEDENTE.** O juiz registrado `selfie_ausente` decide ausência de foto da batida persistida e prazo do canal; o pintor apenas apresenta a selfie local já guardada. | `app/core/juizes.py:1202-1205`; `app/templates/ponto/bater_ponto_v2.html:255-267`. |
| Segundo escritor fora da porta | **NÃO PROCEDENTE para envio/persistência.** O handler continua atribuindo o arquivo a `_fotoBlob`; o novo pintor lê esse estado, cria a URL e escreve quadro/chip. As portas existentes continuam lendo `_fotoBlob` para FormData e fila. | `app/templates/ponto/bater_ponto_v2.html:254-271`, `:305`, `:339`, `:352`, `:365`; `app/static/pwa.js:107-118`. |
| Relógio fora da borda | **NÃO PROCEDENTE no diff.** O novo pintor não lê relógio; leituras existentes permanecem em relógio/GPS/retomada/feedback, fora do trecho alterado fornecido. | `app/templates/ponto/bater_ponto_v2.html:252-271`, `:163-170`, `:177-178`, `:311-323`, `:353-368`; comparação com `diff_v1.txt` anexado. |
| Derivação paralela de fonte única | **NÃO PROCEDENTE como nova fonte de verdade.** A decisão “prévia ou aviso” deriva diretamente de `_fotoBlob`; `_previaUrl` guarda o recurso visual, `_camNegada` seleciona um de dois avisos literais. | `app/templates/ponto/bater_ponto_v2.html:251-267`. |
| Selo/RED sem mordida | A análise detalhada cabe ao agente do teste; **não há prova de execução RED/GREEN no dado fornecido [não verificado]**. O teste contém verificações de prévia, troca, revogação e envio, portanto a acusação genérica de “só selo textual” não corresponde à implementação. | `app/ponto/tests/test_camera_alternativa_previa.py:99-125`, `:131-134`, `:170-177`. |
| Campo com dois significados | **NÃO PROCEDENTE como incompatibilidade de contrato.** `#v2-cam-ph` passa de placeholder/aviso para contêiner de prévia; o leitor de captura usa `#v2-cam`, não o conteúdo do quadro. `_fotoBlob` continua sendo o material enviado; File e Blob já são os valores originados pelo fallback e captura. | `app/templates/ponto/bater_ponto_v2.html:89`, `:255-271`, `:275-286`, `:305`, `:339`. |

**Item 6 — dependências externas**

- **NÃO PROCEDENTE: leitores externos dos ids/markup trocado.** A busca textual em `app` por `v2-cam` localizou os ids no próprio template e no teste novo; não localizou leitores executáveis nas outras telas/JS citados. A ausência é delimitada à cópia e à busca literal; dependências por código gerado ou serviços externos **[não verificado]**. Definição/escrita dos ids: `app/templates/ponto/bater_ponto_v2.html:87-90`, `:236-277`; consumo do teste: `app/ponto/tests/test_camera_alternativa_previa.py:80-81`, `:162`.
- `static/pwa.js` recebe a foto como argumento, grava-a em IndexedDB e depois a põe no FormData; não precisa inspecionar a prévia nem o placeholder. **NÃO PROCEDENTE.** `app/static/pwa.js:107-118`, `:143-156`.
- `sw.js` seleciona tratamento por método/URL e comunica sincronização por mensagem; a troca interna do quadro não participa desses contratos. **NÃO PROCEDENTE.** `app/static/sw.js:33-85`, `:112-125`; `app/static/pwa.js:16`.
- `triagem/apto.html` possui seu próprio estado e ids (`camera`, `preview`, `foto-input`, `camera-off`), sem consumir `v2-cam-*`. **NÃO PROCEDENTE.** `app/templates/ponto/triagem/apto.html:138-150`, `:167-186`, `:229-232`, `:242-282`.
- Ambos os dashboards apenas apontam para a rota `ponto:bater_ponto_v2`; não precisam do markup do quadro. **NÃO PROCEDENTE.** `app/templates/core/dashboard_colaborador.html:39-46`; `app/templates/core/dashboard_colaborador_v2.html:31-38`.

**Limite da comparação:** esta cópia não tem repositório Git utilizável pelo comando executado; comparação com os commits depende do diff anexado **[não verificado independentemente]**. [nao verificado]

## 15/11 10:34 · sessao ef7e8a8767ea · FATO-DE-AUSENCIA-ACORDA-A-PROJECAO · verificador · origem=Code

1. **Sim, a consulta é válida por análise estática para o Django 5.2.14 declarado.** `values_list` seleciona inclusive a PK da própria `Ausencia`, compatível com `of=('self',)`; o filtro usa a FK diretamente, e `order_by('pk')` substitui o `Meta.ordering` que levaria a `colaborador__nome`. A forma resultante termina em `ORDER BY id ASC FOR UPDATE OF ponto_ausencia`. Compilação e execução reais no Django/Postgres: **[nao verificado]**. (`app/requirements.txt:13`; `app/ponto/services/ausencia.py:1416`; `app/ponto/services/ausencia.py:1419`; `app/ponto/services/ausencia.py:1421`; `app/ponto/models.py:829`.)

   Os bloqueios de linha abrangem as Ausencias **daquele agendamento**, tipo `ferias`, com status diferente de `rejeitada`, em PK crescente. Não abrangem Colaborador, Agendamento ou Periodo por essa consulta. O `UPDATE pk__in` posterior encontra essas linhas já travadas; sua própria ordem de execução não introduz outra aquisição entre elas. (`app/ponto/services/ausencia.py:1420`; `app/ponto/services/ausencia.py:1421`; `app/ponto/services/ausencia.py:1425`.)

   Comparação com os demais usos efetivos de `of=('self',)`:

   - `trava_na_ordem`: também combina PK crescente com `values_list('pk')`, sobre `PerguntaDisputa`. (`app/chamados/models.py:2770`.)
   - `trocar_plantao`: carrega instâncias e faz `select_related('colaborador')`, mas restringe o bloqueio às Ausencias; ordena por PK. (`app/ponto/portas/celula.py:695`.)
   - H06: busca uma única Ausencia por PK, remove a ordenação padrão e restringe o bloqueio a `self`. (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:138`.)

2. **Não identifiquei ordem cruzada entre essas Ausencias nos fluxos normais examinados.** A rejeição agora segue PK crescente; o backfill e o registro normalmente consumido pela reversão seguem essa mesma ordem. (`app/ponto/services/ausencia.py:1420`; `app/ponto/services/ausencia.py:1608`; `app/ponto/services/ausencia.py:1619`; `app/ponto/services/ausencia.py:1650`.)

   - **Backfill:** trava Agendamentos e respectivos Periodos pelo join, ordenando por PK de Agendamento; depois trava Ausencias de férias sem origem por PK crescente, antes dos updates individuais. (`app/ponto/services/ausencia.py:1601`; `app/ponto/services/ausencia.py:1607`; `app/ponto/services/ausencia.py:1622`.)
   - **Reversão:** cada `UPDATE` por PK adquire o bloqueio correspondente. Consome a ordem do JSON, sem ordenar novamente; o arquivo produzido pelo apply herda a ordem crescente das Ausencias. (`app/ponto/services/ausencia.py:1537`; `app/ponto/services/ausencia.py:1479`; `app/ponto/services/ausencia.py:1619`; `app/ponto/services/ausencia.py:1650`.)
   - **Cancelamento admin:** adquire Agendamento → Periodo → Ausencias, chamando esta função depois das duas primeiras travas. (`app/ferias/services.py:636`; `app/ferias/services.py:639`; `app/ferias/services.py:679`; `app/ferias/services.py:683`.)
   - **Troca de plantão:** pode tecnicamente travar férias recebidas como cartas, pois não filtra tipo; adquire as Ausencias por PK crescente, depois as células por colaborador/data. (`app/ponto/portas/celula.py:695`; `app/ponto/portas/celula.py:711`.)
   - **H06:** trava uma linha por transação e confere sua identidade, incluindo tipo; divergência impede a edição. As portas genéricas de edição e mudança de estado recusam férias. (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:137`; `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:142`; `app/ponto/services/ausencia.py:579`; `app/ponto/services/ausencia.py:920`.)

   Há também updates históricos por PK nas migrations, fora deste delta, sem exclusão explícita de férias. (`app/ponto/migrations/0057_a4_backfill_pendente.py:43`; `app/ponto/migrations/0058_a4_relogio_recomeca.py:42`; `app/ponto/migrations/0059_a4_d0_no_primeiro_dia_do_cron.py:36`.)

3. **Os dois asserts mordem a regressão pretendida e nenhum exige um resultado falso.** A execução integral da suíte ORM/Postgres permanece **[nao verificado]**. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:141`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:271`.)

   **Editar:** o resultado legítimo atual é `data_fim=None`: um dia deriva `None`, a edição grava `dias_corridos=1`, e o modelo só recompõe fim vazio quando há mais de um dia. A releitura rejeita o antigo `21/10`. No ensaio dos corpos reais em memória, o código atual passou; retirar o `save()` deixou `21/10` e derrubou o assert. (`app/ponto/services/ausencia.py:164`; `app/ponto/services/ausencia.py:606`; `app/ponto/services/ausencia.py:615`; `app/ponto/models.py:912`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:140`.)

   **Desfazer:** a fixture nasce aprovada; retratar grava `False`; desfazer grava expressamente `True` e limpa `retratada_em`. No ensaio em memória, remover apenas a restauração de `aprovada` preservou `False` e derrubou o assert novo, mesmo limpando a retratação. (`app/ponto/models.py:63`; `app/ponto/registro_batida.py:276`; `app/ponto/services/retratar_humano.py:207`; `app/ponto/registro_batida.py:256`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:271`.)

4. **Nenhum outro defeito funcional identificado no delta fornecido.** A ressalva é o alcance do comentário “nunca”: a ordenação sustenta a aquisição dessas Ausencias, mas não constitui garantia global contra deadlocks envolvendo outras tabelas ou travas previamente adquiridas pelo chamador. (`app/ponto/services/ausencia.py:1418`; `app/ponto/services/ausencia.py:1420`; `app/ferias/services.py:679`.)

   Completude da comparação direta entre `1d806e163` e o hash informado, além do diff anexado: **[nao verificado]**.

## 15/11 10:33 · sessao b0adef3e92f4 · - · - · origem=Ronald (tmux)

Os dois asserts são corretos e mordem a regressão pretendida; a execução da suíte ORM/Postgres permanece **[nao verificado]**. [nao verificado]

- **Editar:** o estado legítimo atual é `data_fim=None`: `derivar_data_fim` retorna `None` para um dia; a edição atribui `dias_corridos=1`, atribui o fim e salva; `Ausencia.save` só recompõe fim vazio quando dias > 1. O assert aceita esse estado e rejeita o antigo `2026-10-21`. (`app/ponto/services/ausencia.py:154`, `app/ponto/services/ausencia.py:606`, `app/ponto/services/ausencia.py:615`, `app/ponto/models.py:903`, `app/ponto/models.py:912`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:141`.)

- **Desfazer:** a fixture nasce aprovada pelo default; retratar grava `aprovada=False`; desfazer manda gravar expressamente `aprovada=True` e `retratada_em=None`; `anotar` persiste ambos com `save(update_fields=...)`. Portanto o novo assert não exige um estado falso para essa fixture. Um teste anterior já cobra a mesma aprovação após desfazer. (`app/ponto/models.py:63`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:256`, `app/ponto/registro_batida.py:276`, `app/ponto/services/retratar_humano.py:207`, `app/ponto/registro_batida.py:254`, `app/ponto/registro_batida.py:256`, `app/ponto/tests/test_porta_retratar_batida.py:138`, `app/ponto/tests/test_porta_retratar_batida.py:142`.)

Executei os corpos reais extraídos por AST, com persistência e dependências simuladas em memória: editar atual → `None`, assert passa; mutante sem `ausencia.save()` → releitura `21/10`, assert falha. Desfazer atual → `True/None`, assert passa; mutante removendo apenas a chave `aprovada` → `False/None`, assert falha. Essa checagem comprova a mordida isolada dos asserts; integração com ORM, signals e banco **[nao verificado]**. (`app/ponto/services/ausencia.py:615`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:140`, `app/ponto/services/retratar_humano.py:207`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:269`.)

## 15/11 10:33 · sessao aa7283c56f82 · - · - · origem=Ronald (tmux)

- **Backfill:** dentro de `atomic`, trava agendamentos e períodos pelo join, ordenando por PK de agendamento; depois trava todas as Ausencias de férias sem origem por PK crescente e atualiza uma a uma nessa ordem (`app/ponto/services/ausencia.py:1599`, `:1601`, `:1607`, `:1622`, `:1624`).
- **Reversão:** faz `UPDATE` por PK, sem `select_for_update`, na ordem dos pares do registro; o registro produzido pelo apply herda a ordem crescente das Ausencias (`app/ponto/services/ausencia.py:1479`, `:1608`, `:1619`, `:1646`, `:1650`, `:1537`).
- **Cancelamento administrativo:** trava Agendamento → Periodo antes de chamar a rejeição; a rejeição passa a travar Ausencias por PK crescente (`app/ferias/services.py:636`, `:639`, `:679`, `:683`; `app/ponto/services/ausencia.py:1419`).
- **Troca de plantão:** pode receber Ausencias de qualquer tipo, pois a consulta não filtra `tipo`; trava as duas cartas por PK crescente, somente `self`, e depois as células por colaborador/data. Não cria inversão entre PKs de Ausencia frente à rejeição (`app/ponto/portas/celula.py:695`, `:696`, `:708`, `:711`).
- **H06:** trava somente uma Ausencia por transação; sua lista é de atestados/faltas e a identidade inclui tipo, portanto uma linha que virou férias é pulada antes da edição (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:50`, `:67`, `:137`, `:139`, `:142`, `:146`).
- **Portas genéricas:** edição, mudança de estado, anexo, decisão e reversão da rejeição recusam férias antes da gravação (`app/ponto/services/ausencia.py:579`, `:920`, `:966`, `:1017`, `:1209`; guarda em `:92`).
- **Migrações históricas:** também contêm updates por PK, sem filtro explícito excluindo férias, seguindo a iteração do queryset; não são consumidores novos do delta (`app/ponto/migrations/0057_a4_backfill_pendente.py:35`, `:43`; `0058_a4_relogio_recomeca.py:37`, `:42`, `:59`, `:63`; `0059_a4_d0_no_primeiro_dia_do_cron.py:32`, `:36`).

**Conclusão restrita ao delta:** não identifiquei ordem cruzada entre as Ausencias da rejeição e os registros normalmente produzidos pelo backfill/reversão; todos seguem PK crescente (`app/ponto/services/ausencia.py:1420`, `:1608`, `:1619`, `:1650`). A frase “nunca se travam em ordem cruzada” não prova ausência global de deadlocks: backfill adquire múltiplos agendamentos e seus períodos por join, enquanto cancelamento adquire um agendamento e depois seu período; eventual ciclo com período compartilhado seria preexistente e anterior à entrada em `rejeitar_ausencias_de_ferias` (`app/ponto/services/ausencia.py:1601`; `app/ferias/services.py:636`, `:639`, `:683`); ocorrência/SQL executado [nao verificado].

## 15/11 10:32 · sessao edbbd489bd52 · - · - · origem=Ronald (tmux)

Nenhum outro defeito funcional identificado no delta fornecido: a alteração de produção restringe e ordena a seleção das Ausencias; as outras duas alterações acrescentam verificações de persistência. (`app/ponto/services/ausencia.py:1416`, `app/ponto/services/ausencia.py:1419`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:140`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:271`)

Ressalva: a frase “duas chamadas nunca se travam em ordem cruzada” não demonstra ausência global de deadlock envolvendo outras tabelas ou uma transação externa; o trecho ordena apenas a aquisição dessas Ausencias. (`app/ponto/services/ausencia.py:1418`, `app/ponto/services/ausencia.py:1420`)

A completude do delta entre os hashes, além do anexo fornecido, permanece [nao verificado].

## 15/11 10:28 · sessao 80cd60d8871b · FATO-DE-AUSENCIA-ACORDA-A-PROJECAO · verificador · origem=Code

**O rollback protege o cancelamento inteiro no chamador produtivo; os testes ainda deixam passar regressões de concorrência e atomicidade do lote.** Evidências: `app/ferias/services.py:674`, `app/ferias/services.py:683`, `app/ponto/services/ausencia.py:1416`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:198`. Execução dos testes, mutantes e concorrência: [não verificado].

1. **Chamadores, transações e travas**

   O único chamador direto produtivo encontrado é `executar_cancelamento_admin`: abre `atomic`, trava **agendamento → período**, devolve saldo e chama a rejeição. A função acrescenta um `atomic` interno e o `select_for_update` das Ausências. Portanto, nesse caminho, a ordem é **agendamento → período → Ausências** (`app/ferias/services.py:674`, `app/ferias/services.py:679`, `app/ferias/services.py:683`, `app/ferias/services.py:636`, `app/ferias/services.py:639`, `app/ponto/services/ausencia.py:1416`).

   As entradas indiretas são cancelamento individual, correção de concluída e cancelamento em massa, pelo adaptador da view; há também a simulação de `listar_ferias_dp`, sob outro `atomic` (`app/ferias/views.py:396`, `app/ferias/views.py:444`, `app/ferias/views.py:605`, `app/ferias/views.py:557`, `app/ferias/management/commands/listar_ferias_dp.py:98`). Os demais chamadores diretos são os testes, nas linhas 180, 191 e 200 (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:180`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:191`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:200`).

   Quando `evento` falha, seu próprio savepoint desfaz a tentativa de trilha; a exceção é engolida e ele retorna `None`. A rejeição transforma esse retorno em `RuntimeError` **dentro** do seu `atomic`: desfaz status, `atualizado_em` e quaisquer trilhas anteriores daquela chamada; não alcança `_acordar_projecao` nem retorna `n` (`app/core/observ.py:87`, `app/core/observ.py:105`, `app/core/observ.py:133`, `app/core/observ.py:139`, `app/ponto/services/ausencia.py:1423`, `app/ponto/services/ausencia.py:1428`, `app/ponto/services/ausencia.py:1433`, `app/ponto/services/ausencia.py:1435`).

   No chamador produtivo, a exceção também sai do `atomic` externo: desfaz a devolução de saldo já salva e impede a gravação de `status='cancelado'`, observação e retorno normal (`app/ferias/services.py:680`, `app/ferias/services.py:682`, `app/ferias/services.py:683`, `app/ferias/services.py:687`, `app/ferias/services.py:690`). As três views capturam somente `AgendamentoNaoCancelavel`; o `RuntimeError` escapa desses tratamentos. A resposta HTTP efetiva é [não verificado] (`app/ferias/views.py:397`, `app/ferias/views.py:445`, `app/ferias/views.py:606`). A simulação captura a exceção, imprime `ERRO:RuntimeError:...` e recarrega o agendamento (`app/ferias/management/commands/listar_ferias_dp.py:112`).

   **Há risco de inversão entre linhas de Ausência**, embora o caminho principal respeite a hierarquia agendamento/período/Ausência. O novo SELECT não fixa `order_by`, herdando `-data, colaborador__nome`; a reversão do backfill atualiza as mesmas Ausências uma por vez, na ordem do arquivo, sem tomar antes as travas de agendamento/período (`app/ponto/services/ausencia.py:1417`, `app/ponto/models.py:829`, `app/ponto/services/ausencia.py:1647`, `app/ponto/services/ausencia.py:1535`). O apply produz o registro a partir de linhas ordenadas por PK (`app/ponto/services/ausencia.py:1605`, `app/ponto/services/ausencia.py:1617`).

   Interleaving plausível: A1 tem PK menor/data anterior; A2, PK maior/data posterior. Cancelamento trava A2 e espera A1; reversão trava A1 e espera A2. As travas anteriores do chamador não serializam a reversão, que não as solicita. Esse deadlock é uma possibilidade deduzida dessas ordens; ocorrência real [não verificado] (`app/ponto/models.py:829`, `app/ponto/services/ausencia.py:1648`).

   Também há possível ampliação da trava ao colaborador: a ordenação usa `colaborador__nome` e o SELECT não restringe `of=('self',)`. É inferência do ORM; SQL efetivo e bloqueios observados [não verificado] (`app/ponto/models.py:829`, `app/ponto/services/ausencia.py:1417`).

2. **Os quatro testes novos/alterados mordem, com estas lacunas** [nao verificado]

   Os mutantes abaixo são deduções estáticas; sobrevivência executada [não verificado].

   | Teste | O que verifica / mutante que passa | [nao verificado]
   |---|---|
   | Encolher o fim | Confere a fixture inicial e os dois carimbos, mas não relê o fato depois da edição. **Retirar `ausencia.save()`**, mantendo a alteração em memória e `_acordar_projecao`, passa sem persistir a edição (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:137`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:139`, `app/ponto/services/ausencia.py:604`, `app/ponto/services/ausencia.py:615`, `app/ponto/services/ausencia.py:625`). |
   | Rejeitar com trilha por dia | A nova comparação dos PKs mata “cinco trilhas, mas uma Ausência duplicada e outra omitida”. **Retirar `select_for_update`** continua passando: as duas chamadas são sequenciais (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:182`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:186`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:191`, `app/ponto/services/ausencia.py:1417`). |
   | Falha de trilha desfaz rejeição | O mock falha sempre, portanto já na primeira trilha. **Trocar o atomic do conjunto por um atomic por Ausência** passa: a primeira desfaz e não sobra rejeitada. Falhar só na segunda/quinta deixaria anteriores confirmadas, caso não exercitado (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:169`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:198`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:201`, `app/ponto/services/ausencia.py:1416`). |
   | Desfazer retratação | A releitura nova mata um falso desfazer que apenas chama recálculo. **Retirar `'aprovada': True`** do `anotar` passa, conservando `aprovada=False`: o teste só exige chamada e `retratada_em=None` (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:265`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:267`, `app/ponto/services/retratar_humano.py:207`, `app/ponto/registro_batida.py:276`). |

   **Sim, o patch alcança o nome usado.** O import de `evento as _ev_rej` é local à função e acontece enquanto o `mock.patch('core.observ.evento')` está ativo; não existe alias congelado no módulo nesse caminho (`app/ponto/services/ausencia.py:1414`, `app/ponto/services/ausencia.py:1428`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:198`).

3. **A fixture de edição chega ao helper, pela leitura estática**

   A criação com três dias deriva `data_fim=21/10`; o teste confirma isso antes de carimbar. A edição com um dia deriva fim nulo, salva e chama o helper com os fatos **anterior e posterior** (`app/ponto/models.py:912`, `app/ponto/models.py:917`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:134`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:137`, `app/ponto/services/ausencia.py:164`, `app/ponto/services/ausencia.py:615`, `app/ponto/services/ausencia.py:625`).

   Não há recusa por tranca nessa porta; a sobreposição exclui o próprio PK. A fixture não acrescenta outra Ausência, e essa função não exige documento nesse percurso (`app/ponto/services/ausencia.py:591`, `app/ponto/services/ausencia.py:593`, `app/ponto/turnos.py:2830`, `app/ponto/turnos.py:2851`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:45`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:134`, `app/ponto/services/ausencia.py:570`).

   `atestado` consta no seed ativo; a validação consulta o catálogo vigente. Não há indicação nessa fixture de catálogo que o exclua, mas o estado efetivo do catálogo/cache durante uma execução é [não verificado] (`app/ponto/migrations/0052_seed_tipoausencia.py:17`, `app/ponto/migrations/0052_seed_tipoausencia.py:36`, `app/ponto/catalogo/ausencias.py:278`, `app/ponto/catalogo/ausencias.py:284`, `app/ponto/services/ausencia.py:580`).

4. **Outro defeito exposto pelo delta: falha sem tratamento no lote**

   A nova exceção interrompe o cancelamento em massa antes da mensagem de resultado. Cada item usa sua própria unidade atômica; o loop só trata `AgendamentoNaoCancelavel`. Assim, sem transação externa adicional, itens anteriores permanecem confirmados, o item falho desfaz e os seguintes não são processados. Uma transação externa de request efetivamente configurada é [não verificado] (`app/ferias/views.py:600`, `app/ferias/views.py:605`, `app/ferias/views.py:606`, `app/ferias/views.py:611`, `app/ferias/services.py:674`).

   Os testes do delta chamam diretamente a rejeição; não verificam esse tratamento nas views nem o rollback conjunto de saldo/agendamento no chamador produtivo (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:180`, `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:200`).

## 15/11 10:27 · sessao 14cd1bafc151 · FIO-MORTO-CLASSE-D · leitura · origem=Code

**1. O colaborador estar na lista não implica que o dia esteja nela.** `_colabs` contém os colaboradores das linhas; `_dias` contém somente assinaturas com `data`, indexadas por `(colab_id, str(data)[:10])`. Dono ausente ou divergente **não exclui a chave**: ela permanece com valor `None`. [vigia_fio.py:216](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/vigia_fio.py:216), [cadastro_realidade.py:1014](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/escala/services/cadastro_realidade.py:1014), [cadastro_realidade.py:1038](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/escala/services/cadastro_realidade.py:1038).

Os ramos de exclusão na composição são:

| Camada | Predicado exato / efeito | Função real |
|---|---|---|
| Recorte da lista | `vis = {e.pk for e in empresas_visiveis()}`, intersectado com `self.empresas`, quando fornecido | `ListaDoAdmin._carregar`; visíveis são empresas ativas, excluído CNPJ de diagnóstico. [vigia_fio.py:211](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/vigia_fio.py:211), [queries.py:78](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/colaboradores/queries.py:78) |
| Lista sem conteúdo | `not por_colab and not cad` → `return None` | `cadastro_realidade.lista`. [cadastro_realidade.py:867](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/escala/services/cadastro_realidade.py:867) |
| Colaborador excluído | `v is None or (empresas is not None and v['empresa_id'] not in empresas)` | `lista`, alimentada por `_vinculos`. Esse ramo elimina o colaborador inteiro, portanto não explica sozinho a classe d. [cadastro_realidade.py:743](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/escala/services/cadastro_realidade.py:743), [cadastro_realidade.py:878](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/escala/services/cadastro_realidade.py:878) |
| Assinatura A desconhecida | `cod not in ASSINATURAS` → `continue`; linha só é publicada se `linha['assinaturas']` | `lista`. [cadastro_realidade.py:917](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/escala/services/cadastro_realidade.py:917) |
| Assinatura sem dia | `not a.get('data')`: não alimenta `_dias`; assinatura de outra data alimenta outra chave | `dias_na_lista`. Inclui assinaturas agregadas, mesmo quando o detalhe textual menciona dias. [cadastro_realidade.py:1025](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/escala/services/cadastro_realidade.py:1025) |
| C9/C10/C12 não publicaram aquele dia | Ausência de item datado correspondente nas lavras lidas; C12 admite item sem `data` | `_teto_do_dia`, `_fase_deslocada`, `_cxr_do_motor`, respectivamente. [cadastro_realidade.py:344](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/escala/services/cadastro_realidade.py:344), [cadastro_realidade.py:386](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/escala/services/cadastro_realidade.py:386), [cadastro_realidade.py:360](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/escala/services/cadastro_realidade.py:360) |
| Falha parcial de produtor | Os `except Exception` preservam a lista, mas deixam de acrescentar as assinaturas daquele produtor | `lista`: C2–C12 possuem guardas próprias; C1 é carregada antes delas. [cadastro_realidade.py:781](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/escala/services/cadastro_realidade.py:781), [cadastro_realidade.py:785](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/escala/services/cadastro_realidade.py:785) |
| Falha integral da lista | Exceção em `_carregar` deixa `_dias is None`; `dia_na_lista` retorna `None`, não `(False, None)` | `ListaDoAdmin._carregar` / `dia_na_lista`. Portanto essa falha não satisfaz o predicado d. [vigia_fio.py:219](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/vigia_fio.py:219), [vigia_fio.py:230](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/vigia_fio.py:230), [vigia_fio.py:696](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/chamados/services/vigia_fio.py:696) |

Na **C11**, todos os vetos, na ordem real, são:

| Ramo | Predicado exato | Quem responde |
|---|---|---|
| Fora das janelas | Empresa fora de `Empresa.objects.filter(ativa=True)`/recorte; competência ausente de `competencias_a_lavrar`; ou fora de `[ini, min(fim, ontem)]`; janela vazia não entra | `_folga_nao_definida_sem_porta`, `competencias_a_lavrar`, `janela_fechamento`. [cadastro_realidade.py:542](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/escala/services/cadastro_realidade.py:542), [he_pendente_lavrado.py:209](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/ponto/services/he_pendente_lavrado.py:209) |
| Fora do queryset | Não satisfaz a conjunção `colaborador__empresa=emp`, `colaborador__situacao='ativo'`, `veredito='indefinida'`, `protestos__contains=['PREVISTO_COM_DIA_INDEFINIDO']`, `data__gte=ini`, `data__lte=fim`; `not cels` encerra o lote | Query de `beco_em_lote`. [cadastro_realidade.py:449](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/escala/services/cadastro_realidade.py:449) |
| Sem pergunta viva | `not vivas` | `alimentar_portas(..., agora=agora)['vivas']`, lido por `beco_em_lote`. [cadastro_realidade.py:458](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/escala/services/cadastro_realidade.py:458), [cadastro_realidade.py:468](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/escala/services/cadastro_realidade.py:468) |
| Já tem porta | `porta_da_tarefa(tarefas[jan].tarefa(col[cid], d)) is not None` | `carga_da_tarefa` / `porta_da_tarefa`; `not dias` encerra o lote após esses dois vetos. [cadastro_realidade.py:464](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/escala/services/cadastro_realidade.py:464) |
| Sem âncora terminal | `not sorted({a for _p, a in ancs if a and st.get(a) in TERMINAIS})` | Âncora por `chamado_que_lista(p)`; status por query de `ChamadoColaborador`; terminais por `catalogo.motor.TERMINAIS`. Composição em `beco_em_lote`. [cadastro_realidade.py:471](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/escala/services/cadastro_realidade.py:471), [cadastro_realidade.py:496](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/escala/services/cadastro_realidade.py:496) |
| Folga não definida falsa | `not cad[(cid, ini)](d)['folga_nao_definida']` | `supra_juiz.cadastro_do_dia` → `vinculo_do_dia` → `_insumos_do_vinculo` → `EscalaColaborador.defeito_folga_nao_definida`. Sem vínculo, o insumo é `False`; com vínculo, é falso se não for 6x1, houver `folga_dia_semana` ou existir `FolgaDia` no mês. [cadastro_realidade.py:492](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/escala/services/cadastro_realidade.py:492), [cadastro_realidade.py:499](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/escala/services/cadastro_realidade.py:499), [supra_juiz.py:173](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/ponto/supra_juiz.py:173), [supra_juiz.py:206](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/ponto/supra_juiz.py:206), [models.py:1241](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/escala/models.py:1241) |

**2. O juiz L-099 distribui responsabilidade; não detecta divergência.** Sua assinatura real é esta. [e6_oraculo.py:53](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/ponto/management/commands/e6_oraculo.py:53):

```python
def dono_da_divergencia(
    horas_do_dia,
    em_cadastro_x_realidade,
    folga_nao_definida=False,
    sem_vinculo_no_dia=False,
    sem_vinculo_em_dia_de_trabalho=False,
    batida_fora_de_turno=False,
    fase_deslocada=False,
):
```

Ela não recebe `(colab, dia)` e sempre retorna `CADASTRO`, `BATIDA` ou `ESTRUTURA`. Não há retorno especial “sem divergência”: `dono_da_divergencia([], False)` retorna `ESTRUTURA`. [e6_oraculo.py:98](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/ponto/management/commands/e6_oraculo.py:98).

A montagem que permite perguntar por colaborador/dia é esta, seguida de `.dono(data)`. [e6_oraculo.py:128](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/ponto/management/commands/e6_oraculo.py:128), [e6_oraculo.py:182](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/ponto/management/commands/e6_oraculo.py:182):

```python
InsumosDoDono(
    colaborador, ini, fim, hoje, espelho=None, *,
    agora, dias_de_fase=None, cadastro=None,
    sem_motor=False, cxr=None,
).dono(data)
```

`None` com `sem_motor=True` significa **sem resposta por falta de cobertura da lavra**, não `sem_divergencia`. O command E6 verifica a divergência antes de chamar o dono. [e6_oraculo.py:142](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/ponto/management/commands/e6_oraculo.py:142), [e6_oraculo.py:189](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/ponto/management/commands/e6_oraculo.py:189), [e6_oraculo.py:334](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/ponto/management/commands/e6_oraculo.py:334).

**3. Script para o envelope.** O motivo observa o primeiro veto **efetivamente executado** pela C11 durante a montagem original; não força a avaliação dos ramos posteriores. Para `sem_divergencia`, aplica as comparações do E6; dias que o E6 não julga saem como erros nomeados. [cadastro_realidade.py:469](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/escala/services/cadastro_realidade.py:469), [e6_oraculo.py:334](/home/ronald/codex_ws/77dfa94fa85e3403e984a17bdaf625c52e26e3a1/app/ponto/management/commands/e6_oraculo.py:334).

A soma e a distribuição na sombra continuam **[nao verificado]**.

```python
# FIO-MORTO-CLASSE-D; copia 77dfa94fa85e3403e984a17bdaf625c52e26e3a1.
from django.utils import timezone as tz
agora = tz.now()  # unico relogio fisico
hoje = tz.localdate(agora)

import datetime as dt
import logging
import re
import sys
from collections import Counter
from unittest.mock import patch
from django.db import connection
from django_tenants.utils import schema_context
from colaboradores.models import Colaborador
from chamados.services.vigia_fio import ListaDoAdmin, censo, censo_alcance
from chamados.juizes import tranca_em_lote
from escala.services import cadastro_realidade as cr
from ponto.janelas import janela_fechamento
from ponto.management.commands.e6_oraculo import (
    InsumosDoDono, minutos_do_espelho, diferenca_e_a_janela_declarada,
)
from ponto.services.espelho import espelho_do_colab

class SQLNaoSelect(Exception): pass
class ListaSemResposta(Exception): pass
class ClasseDInconsistente(Exception): pass
class RastroC11Incompleto(Exception): pass
class E6SemLinha(Exception): pass
class E6IsencaoOuAusencia(Exception): pass
class E6SemLavratura(Exception): pass
class E6JanelaDeclaradaNaoJulgada(Exception): pass
class DonoSemResposta(Exception): pass
class SomaDiferenteDe212(Exception): pass
class OutrosDiferentesDe36(Exception): pass

def so_select(execute, sql, params, many, context):
    # Bloqueia DML/DDL e consultas com trava; nao imprime SQL/parametros.
    if (many or not re.match(r"\s*SELECT\b", sql, re.I)
            or re.search(r"\bFOR\s+(?:NO\s+KEY\s+UPDATE|KEY\s+SHARE|UPDATE|SHARE)\b",
                         sql, re.I)):
        raise SQLNaoSelect()
    return execute(sql, params, many, context)

# Observa a execucao REAL, sem substituir predicados ou montar outra lista.
# cadastro_realidade.py:450-455,469-470,496-500,525-556,768-957.
rastro = {"dados": None, "candidatos": set(), "janelas": None,
          "vetos": {}, "falhas": Counter()}
cod_lista = cr.lista.__code__
cod_beco = cr.beco_em_lote.__code__
cod_janelas = cr._folga_nao_definida_sem_porta.__code__
codigos = {cod_lista, cod_beco, cod_janelas}

def observar(frame, event, arg):
    code = frame.f_code
    if code not in codigos:
        return None
    loc = frame.f_locals
    n = frame.f_lineno
    if code is cod_lista:
        if event == "exception":
            rastro["falhas"][(n, type(arg[1]).__name__)] += 1
        elif event == "return" and isinstance(arg, dict):
            rastro["dados"] = arg
    elif code is cod_janelas:
        if event == "return" and "janelas" in loc:
            rastro["janelas"] = [
                (emp.pk, ini, fim) for emp, ini, fim in loc["janelas"]]
    elif event == "line":
        if n == 455:
            rastro["candidatos"].update(
                (cid, d.isoformat()) for cid, d, jan in loc["cels"].values())
        elif n == 470:
            k = (loc["cid"], loc["d"].isoformat())
            rastro["vetos"][k] = (
                "outro:C11_sem_pergunta_viva" if not loc["vivas"]
                else "outro:C11_porta_da_tarefa")
        elif n == 498:
            rastro["vetos"][
                (loc["cid"], loc["d"].isoformat())] = "sem_ancora_terminal"
        elif n == 500:
            rastro["vetos"][
                (loc["cid"], loc["d"].isoformat())] = "folga_nao_definida_False"
    return observar

# Trava o instrumento nas linhas da copia lida.
import linecache
fonte = cr.beco_em_lote.__code__.co_filename
for n, trecho in ((455, "if not cels:"), (470, "continue"),
                  (498, "continue"), (500, "continue")):
    assert linecache.getline(fonte, n).strip() == trecho

def motivo_real(cid, data_iso, emp):
    if rastro["falhas"]:
        return "erro:" + sorted(rastro["falhas"])[0][1]
    if (lista.dia_na_lista(cid, data_iso) != (False, None)
            or not lista.colab_na_lista(cid)):
        raise ClasseDInconsistente()
    k = (cid, data_iso)
    if k in rastro["vetos"]:
        return rastro["vetos"][k]
    if k in rastro["candidatos"] or rastro["janelas"] is None:
        raise RastroC11Incompleto()
    assinaturas = [
        a for posto, linhas in (rastro["dados"].get("postos") or ())
        for v in linhas if v.get("colab_id") == cid
        for a in (v.get("assinaturas") or ())
    ]
    origem = ("somente_outros_dias" if any(a.get("data") for a in assinaturas)
              else "assinaturas_sem_data")
    dia = dt.date.fromisoformat(data_iso)
    na_janela = any(e == emp and ini <= dia <= fim
                    for e, ini, fim in rastro["janelas"])
    return "outro:C11_%s+%s" % (
        "fora_queryset" if na_janela else "fora_janelas", origem)

cols, cargas = {}, {}
def colaborador(cid):
    if cid not in cols:
        try:
            cols[cid] = Colaborador.objects.select_related("empresa").get(pk=cid)
        except Exception as exc:
            cols[cid] = exc
    c = cols[cid]
    if isinstance(c, Exception):
        raise c
    return c

def dono_real(c, dia, comp):
    # Mesmo espelho, montagem e gates do command E6:
    # e6_oraculo.py:304,316-328,334-378.
    mes, ano = map(int, comp.split("/"))
    ini, fim = janela_fechamento(mes, ano, c.empresa)
    chave = (c.pk, ini, fim)
    if chave not in cargas:
        try:
            esp = espelho_do_colab(c, hoje, ini, fim, agora=agora)
            ins = InsumosDoDono(c, ini, fim, hoje, espelho=esp, agora=agora)
            cargas[chave] = (
                {str(x.get("data"))[:10]: x for x in (esp.get("dias") or [])},
                ins,
            )
        except Exception as exc:
            cargas[chave] = exc
    carga = cargas[chave]
    if isinstance(carga, Exception):
        raise carga
    dias_esp, ins = carga
    k = dia.isoformat()
    de = dias_esp.get(k)
    if de is None:
        raise E6SemLinha()
    if de.get("isencao") or de.get("ausencia") is not None:
        raise E6IsencaoOuAusencia()
    m_esp, fonte_esp = minutos_do_espelho(de)
    if m_esp is None:
        raise E6SemLavratura()
    m_e6 = ins.oraculo.minutos.get(k, 0.0)
    if m_esp == 0 and m_e6 == 0:
        return "sem_divergencia"
    if diferenca_e_a_janela_declarada(m_e6 - m_esp, de):
        raise E6JanelaDeclaradaNaoJulgada()
    if abs(m_e6 - m_esp) <= 10:
        return "sem_divergencia"
    dono = ins.dono(dia)
    if dono not in {"CADASTRO", "BATIDA", "ESTRUTURA"}:
        raise DonoSemResposta()
    return dono

tot, outros, erros = Counter(), Counter(), Counter()
nivel_log = logging.root.manager.disable
trace_anterior = sys.gettrace()
try:
    # Evita nomes/CPF ou escrita nos handlers de log.
    # Excecoes dos produtores da lista sao observadas e contadas.
    logging.disable(logging.CRITICAL)
    with schema_context("juliani"), \
            patch.object(tz, "now", return_value=agora), \
            connection.execute_wrapper(so_select):
        sys.settrace(observar)
        try:
            # Composicao da sonda_mapa_v2.py:31-36 (anexo).
            lista = ListaDoAdmin(hoje, None, agora=agora)
            with tranca_em_lote():
                vant = censo(None, hoje=hoje, limite=10000000, lista=lista)
                r = censo_alcance(
                    None, hoje=hoje, limite=10000000, lista=lista, agora=agora)
            rows = r.get("alcance_dias", [])
        finally:
            sys.settrace(trace_anterior)
        if lista._dias is None or rastro["dados"] is None:
            raise ListaSemResposta()
        for x in rows:
            cel, cid, bruto, comp, rota, causa, det, papel = x[:8]
            if rota != "sem_porta" or causa not in {"d", "c", "a", "e"}:
                continue
            pk = getattr(cel, "pk", cel)
            data_iso = str(bruto)[:10]
            emp, c, erro_col = 0, None, None
            try:
                c = colaborador(cid)
                emp = c.empresa_id
            except Exception as exc:
                erro_col = "erro:" + type(exc).__name__
            if causa == "d":
                try:
                    motivo = erro_col or motivo_real(cid, data_iso, emp)
                except Exception as exc:
                    motivo = "erro:" + type(exc).__name__
                try:
                    dono = erro_col or dono_real(
                        c, dt.date.fromisoformat(data_iso), comp)
                except Exception as exc:
                    dono = "erro:" + type(exc).__name__
                tot[(motivo, dono, comp)] += 1
                for campo, valor in (("motivo", motivo), ("dono", dono)):
                    if valor.startswith("erro:"):
                        erros[(campo, valor)] += 1
                print("D\tcel%s\tcol%s\t%s\t%s\temp%s\tmotivo=%s\tdono=%s" %
                      (pk, cid, data_iso, comp, emp, motivo, dono))
            else:
                detalhe = erro_col or det or "-"
                outros[(causa, detalhe, comp)] += 1
                if erro_col:
                    erros[("O", erro_col)] += 1
                print("O\tcel%s\tcol%s\t%s\t%s\temp%s\t%s\t%s" %
                      (pk, cid, data_iso, comp, emp, causa, detalhe))
        for (motivo, dono, comp), n in sorted(tot.items()):
            print("S motivo=%s dono=%s comp=%s n=%d" % (motivo, dono, comp, n))
        soma = sum(tot.values())
        print("S soma=%d" % soma)
        for (causa, detalhe, comp), n in sorted(outros.items()):
            print("S O causa=%s detalhe=%s comp=%s n=%d" %
                  (causa, detalhe, comp, n))
        print("S O soma=%d" % sum(outros.values()))
        for (campo, tipo), n in sorted(erros.items()):
            print("S campo=%s %s n=%d" % (campo, tipo, n))
        for (linha, tipo), n in sorted(rastro["falhas"].items()):
            print("S produtor=lista linha=%d erro:%s n=%d" % (linha, tipo, n))
        if soma != 212:
            raise SomaDiferenteDe212()
        if sum(outros.values()) != 36:
            raise OutrosDiferentesDe36()
finally:
    sys.settrace(trace_anterior)
    logging.disable(nivel_log)
print('FIM ok=1')
```

**4. Atribuição de obra:

[truncado em 20000 de 24855 caracteres]

## 15/11 10:27 · sessao 5c81ef3d2810 · - · - · origem=Ronald (tmux)

- **Editar alcança a invalidação, por leitura estática.** A fixture cria somente um atestado; `save` deriva 21/10 (`app/ponto/models.py:912-918`; teste:134-137). `atestado` existe no seed ativo (`app/ponto/migrations/0052_seed_tipoausencia.py:17,33-36`) e no fallback (`app/ponto/catalogo/ausencias.py:95,278`). Editar não consulta tranca (`app/ponto/services/ausencia.py:591`), exclui o próprio PK da sobreposição (`:593`; `app/ponto/turnos.py:2827-2835,2851-2853`) e não exige documento nesse caminho (`app/ponto/services/ausencia.py:570-633`). `dias_corridos=1` produz fim nulo (`:154-166`), salva (`:615`) e invalida pelos fatos antigo e novo (`:625-626`). Resultado executado: [não verificado].

- **Patch correto.** `mock.patch('core.observ.evento')` está ativo quando a função faz seu import local `_ev_rej`; alcança o nome efetivamente chamado (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:198-200`; `app/ponto/services/ausencia.py:1414,1428`).

- **Editar, mutante sobrevivente:** retirar `ausencia.save()` mantendo alteração em memória e `_acordar_projecao`. O teste verifica somente os carimbos depois da chamada, sem reler/assertar o fato editado (`app/ponto/services/ausencia.py:604-626`; teste:139-141). Sobrevivência deduzida, não executada [não verificado].

- **Rejeitar/trilha, teste fortalecido:** multiconjunto de PKs agora mata cinco trilhas duplicando um ID e omitindo outro, que contagem/status permitiam (`teste:182-187`). **Mutante sobrevivente:** remover `select_for_update`; esse caso só chama sequencialmente (`app/ponto/services/ausencia.py:1417`; teste:175-193). Sobrevivência deduzida [não verificado].

- **Rollback, mutante sobrevivente:** refatorar para um `atomic` por ausência, cada iteração atualizando e trilhando aquela linha. O mock sempre devolve `None`: falha a primeira e nada fica rejeitado, passando; falha apenas na segunda/quinta deixaria anteriores confirmadas. Não se injeta sequência de sucessos e falha nem se confere trilha, carimbo ou timestamp (`teste:169,195-202`; atomic atual: `app/ponto/services/ausencia.py:1416-1434`). Sobrevivência deduzida [não verificado].

- **Desfazer, teste fortalecido:** releitura de `retratada_em` mata falso desfazer que apenas chama recálculo (`teste:263-268`). **Mutante sobrevivente:** retirar `'aprovada': True` do `anotar` em desfazer: o teste segue verificando apenas `retratada_em` e chamada (`app/ponto/services/retratar_humano.py:207-213`; teste:265-268), embora retratar tenha gravado `aprovada=False` (`app/ponto/registro_batida.py:276`). Sobrevivência deduzida [não verificado]. Observação: o universo apurável filtra só `retratada_em`, portanto não afirmar exclusão da apuração por esse mutante (`app/ponto/turnos.py:2876-2883`).

## 15/11 10:26 · sessao f08063655a32 · - · - · origem=Ronald (tmux)

Chamadores e falha:

- Único chamador direto produtivo: `executar_cancelamento_admin`, em `app/ferias/services.py:683`, dentro do `atomic` de `app/ferias/services.py:674`; antes trava agendamento (`app/ferias/services.py:636`) e período (`app/ferias/services.py:639`). A função acrescenta `atomic` interno (`app/ponto/services/ausencia.py:1416`) e trava Ausencias (`app/ponto/services/ausencia.py:1417`).
- Chamadas diretas em testes: `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:180`, `:191`, `:200`. O arquivo `selo_hj091.py:26` apenas enumera o nome, não chama.
- Entradas produtivas indiretas: individual (`app/ferias/views.py:396`), correção concluída (`app/ferias/views.py:444`) e lote (`app/ferias/views.py:605`), pelo adaptador `app/ferias/views.py:557–561`. Também existe a simulação do command `listar_ferias_dp`, que chama o service em `app/ferias/management/commands/listar_ferias_dp.py:100`, sob outro `atomic` em `:98`.
- `evento` usa seu próprio savepoint (`app/core/observ.py:133`), engole falhas (`:105`, `:87–91`) e retorna `None` (`:139`). A nova checagem lança `RuntimeError` dentro do atomic interno (`app/ponto/services/ausencia.py:1428–1434`), desfazendo UPDATE e trilhas já gravadas nessa chamada (`:1423–1434`). A exceção sai sem alcançar invalidação (`:1435`).
- No chamador produtivo, também desfaz a devolução de saldo já salva (`app/ferias/services.py:680–683`); não chega à gravação de status/obs (`:684–689`) nem ao refresh/retorno (`:690–691`). As três views capturam apenas `AgendamentoNaoCancelavel` (`app/ferias/views.py:397`, `:445`, `:606`), portanto `RuntimeError` escapa; resposta HTTP concreta [não verificado]. No lote, itens anteriores já concluíram seus próprios atomics; o item falho aborta a continuação do loop (`app/ferias/views.py:600–611`; `app/ferias/services.py:674–691`). O command simulado captura a exceção e produz `ERRO:RuntimeError:...`, depois recarrega o agendamento (`app/ferias/management/commands/listar_ferias_dp.py:112–115`).

Travas:

- No fluxo principal, a nova trava respeita a ordem declarada agendamento → período → ausencia (`app/ferias/services.py:629–639`; `app/ponto/services/ausencia.py:1417–1419`). Editar ausência genericamente não é contraparte sobre férias: a guarda recusa esse tipo (`app/ponto/services/ausencia.py:579`, `:92–98`).
- Há uma inversão plausível **entre linhas de Ausencia** contra a reversão do backfill. O novo SELECT herda ordenação por `-data, colaborador__nome` (`app/ponto/models.py:829`), pois não sobrescreve `order_by` (`app/ponto/services/ausencia.py:1417–1419`). A reversão abre atomic e atualiza uma linha por vez na ordem do arquivo (`app/ponto/services/ausencia.py:1647–1649`, UPDATE em `:1535–1536`), sem travar agendamento/período. O apply monta o arquivo a partir de linhas por PK (`:1605–1606`), preserva essa ordem na classificação (`:1477`, `:1511`) e grava `vincular` (`:1617`). Com A1 de data anterior e A2 posterior: cancelamento pode travar A2 e esperar A1; reversão pode atualizar/travar A1 e esperar A2. A existência do escritor e as ordens estão no código; deadlock efetivamente ocorrido [não verificado].
- `colaborador__nome` também implica JOIN no SELECT e, sem `of=('self',)`, pode ampliar o conjunto bloqueado ao colaborador: inferência do ORM a partir de `app/ponto/models.py:829` e `app/ponto/services/ausencia.py:1417–1419`; SQL efetivo [não verificado].

## 15/11 10:23 · sessao faed775e58a7 · FATO-DE-AUSENCIA-ACORDA-A-PROJECAO · verificador · origem=Code

Há uma falha de competência no novo caminho de `desfazer`: uma saída após a meia-noite pode recalcular a competência seguinte e deixar velha a competência do turno. A rejeição em massa também continua podendo gravar o ato sem a trilha prometida. (`app/ponto/services/retratar_humano.py:212`; `app/ponto/services/fechamento.py:1567`, `1809`, `627`; `app/ponto/services/ausencia.py:1418`; `app/core/observ.py:139`)

Execução dos testes, identidade criptográfica da cópia e mudança efetiva em produção: **[nao verificado]**. O material fornecido contém código e anexos, sem linhas de produção que permitam medir pessoas, competências ou valores afetados: **[nao verificado]**. [nao verificado]

**1. Escritores, leitores e significado dos campos**

O censo abaixo identifica os caminhos encontrados na cópia; completude contra escrita dinâmica, SQL montado em execução ou processos externos: **[nao verificado]**. O próprio censo existente declara limitações de inferência. (`app/core/censo_escritas.py:8`, `42`, `47`)

| Campo | Quem escreve | Quem lê e significado |
|---|---|---|
| `Ausencia.status` | Criação; `mudar_estado`; rejeição bulk de férias. (`app/ponto/services/ausencia.py:440`, `926`, `938`, `1418`) | Cobertura para folha exige `aprovada`; cobrança admite qualquer não rejeitada. Estado aberto sai de `ABERTAS`. (`app/ponto/turnos.py:2511`; `app/ponto/services/ausencia.py:866`) |
| `Ausencia.data` | Criação e edição. (`app/ponto/services/ausencia.py:438`, `603`) | Data inicial da cobertura; também serve como dia inicial para crédito parcial, contagens e apresentação. (`app/ponto/turnos.py:2797`; `app/ponto/services/credito_parcial.py:27`; `app/ferias/models.py:149`) |
| `Ausencia.data_fim` | `derivar_data_fim`; criação/edição; derivação no modelo, persistida por transição ou rederivação. (`app/ponto/services/ausencia.py:152`, `439`, `604`, `936`, `1659`; `app/ponto/models.py:917`) | Fim **inclusivo**; vazio abre somente tipos `TIPOS_EM_ABERTO`; nos demais vale um dia. (`app/ponto/turnos.py:2786`) |
| `Ausencia.tipo` | Criação e edição. (`app/ponto/services/ausencia.py:438`, `602`) | Decide catálogo/efeito, abertura do intervalo e classificação parcial; o fechamento consulta esses julgamentos. (`app/ponto/models.py:850`; `app/ponto/turnos.py:2793`; `app/ponto/services/fechamento.py:681`, `694`) |
| `Ausencia.minutos` | Entrada da criação; edição; modelo deriva previsto da falta aprovada quando zero; transição/rederivação persistem. (`app/ponto/services/ausencia.py:439`, `612`, `936`, `1659`; `app/ponto/models.py:887`) | Minutos, convertidos em horas por `/60`; falta de intervalo é total rateado pelos dias; crédito parcial fica no dia inicial. (`app/ponto/models.py:875`; `app/folha/export.py:118`, `121`; `app/ponto/services/credito_parcial.py:27`) |
| `Ausencia.dias_corridos` | Entrada; edição; modelo deriva `(fim−início)+1`. (`app/ponto/services/ausencia.py:438`, `605`; `app/ponto/models.py:903`, `909`) | Duração civil inclusiva para derivar fim e apresentar/diagnosticar; não encontrei leitura direta desse campo no cálculo mensal. (`app/ponto/models.py:912`; `app/ponto/services/dia_decidido.py:621`; `app/ponto/services/vigia_ausencia.py:135`; `app/ponto/services/fechamento.py:638`, `683`, `874`) |
| `Ausencia.aprovada_em` | Criação aprovada; modelo completa em **aprovada ou rejeitada**; `decidir` aceita instante fornecido pelo chamador; rederivação pode completá-lo. (`app/ponto/services/ausencia.py:445`, `1026`, `1659`; `app/ponto/models.py:882`) | Instante de decisão para histórico e frescor, com fallback para criação; apresentação converte para hora local. (`app/ponto/services/dia_decidido.py:508`; `app/ponto/services/cartorio.py:385`; `app/templates/ponto/partials/painel_ausencia.html:275`) |
| `FechamentoMensal.derivado_em` | Recálculo aberto por `save`; demais estados por `update`; restauração põe NULL por padrão; invalidação põe NULL. (`app/ponto/services/fechamento.py:1143`, `1189`, `1391`, `1656`) | Carimbo de derivação; NULL reúne “nunca derivado” e “invalidado”. Contador/listagem padrão olham competência corrente. (`app/ponto/services/fechamento.py:1679`, `1692`, `1702`; `app/ponto/management/commands/projecoes_vencidas.py:49`) |
| `Justificativa.status` | Nascimento na API, tela, cron e regularização; decisão/materialização no serviço; lotes na tela; rejeição no reconciliador; regularização pode voltar a pendente. (`app/api/views.py:1999`; `app/ponto/views.py:1638`, `1799`, `1878`; `app/ponto/management/commands/processar_alertas_turno.py:394`; `app/ponto/services/justificativa.py:205`, `283`, `383`; `app/chamados/reconciliador.py:532`; `app/ponto/services/regularizacao.py:106`) | Estados próprios `pendente/aprovada/rejeitada`; cálculo lê somente aprovada pelo **dia do turno**. (`app/ponto/models.py:130`, `144`, `159`; `app/ponto/services/fechamento.py:660`) |
| `Batida.retratada_em` | `retratar_batida` grava `now()` por `anotar`; desfazer grava NULL pela mesma porta. (`app/ponto/registro_batida.py:254`, `276`; `app/ponto/services/retratar_humano.py:207`) | Presença exclui da apuração; instante também governa prazo humano de sete dias e frescor. (`app/ponto/turnos.py:2876`; `app/ponto/services/retratar_humano.py:53`, `109`; `app/chamados/veredito_lavrado.py:180`) |
| `LogAuditoria.acao/valor_antes/valor_depois` | Persistência runtime por `evento`; `registrar_log` e `evento_kw` adaptam argumentos. O novo bulk usa essa porta. (`app/core/observ.py:133`, `146`; `app/core/auditoria.py:22`; `app/ponto/services/ausencia.py:1419`) | Ação textual e JSON sem esquema comum; cada domínio interpreta seu payload. Desfazer lê última ação; relatórios humanos leem motivo; flips aceitam formatos históricos diferentes. (`app/core/models.py:296`, `300`; `app/ponto/services/retratar_humano.py:35`, `51`, `95`, `247`; `app/ponto/services/flip_batida.py:46`, `73`) |

Os principais leitores adicionais encontrados, agrupados para tornar o alcance conferível, são:

| Família | Sítios leitores |
|---|---|
| Ausência: geometria, precedência, dinheiro e lavra | `app/ponto/turnos.py:2531`, `2554`, `2600`, `2677`, `2715`, `2753`, `2786`, `2827`, `3076`, `3138`; `app/ponto/precedencia.py:273`, `337`; `app/ponto/services/fechamento.py:638`, `683`, `694`, `874`; `app/folha/export.py:70`, `87`, `117`, `140`, `160`; `app/ponto/services/efeito_ausencia.py:47`; `app/ponto/services/credito_parcial.py:26`; `app/ponto/services/cartorio.py:383`, `1356`; `app/ponto/signals.py:123`. |
| Ausência: chamados e cobrança | `app/chamados/services/materializacao.py:79`, `626`, `662`; `app/chamados/services/silencio_ausencia.py:66`, `78`, `94`; `app/chamados/juizes.py:479`, `1801`, `2380`; `app/chamados/services/historico.py:129`; `app/chamados/services/foto_e4b.py:251`, `285`; `app/chamados/services/adesao.py:69`; `app/chamados/services/vigia_fio.py:308`. |
| Ausência: grade, espelho e calendário | `app/escala/utils.py:1174`; `app/escala/servico_jornada.py:347`; `app/escala/services/cadastro_realidade.py:603`; `app/escala/services/furos_diarios.py:222`; `app/ponto/services/espelho.py:582`; `app/colaboradores/services/calendario.py:484`, `532`; `app/api/views.py:1566`. |
| Ausência: férias, relatórios e inteligência | `app/ferias/models.py:147`; `app/ferias/services.py:147`; `app/relatorios/views.py:197`, `277`, `304`; `app/inteligencia/views.py:71`, `232`, `516`; `app/inteligencia/resumo.py:39`; `app/inteligencia/management/commands/calcular_scores.py:12`; `app/inteligencia/management/commands/gerar_alertas.py:55`. |
| Ausência: serviços operacionais e vigias | `app/ponto/services/dia_decidido.py:448`, `481`, `508`, `550`, `614`; `app/ponto/services/vigia_ausencia.py:59`, `104`, `135`, `215`; `app/ponto/services/vigia_ausencia_veredito.py:46`; `app/ponto/services/alertas_ausencia.py:48`; `app/ponto/services/deslogados.py:163`; `app/ponto/services/faltas_de_hoje.py:59`; `app/ponto/services/afastado_avisa.py:39`, `127`; `app/ponto/services/aviso_da_batida.py:88`; `app/colaboradores/services/situacao_exibida.py:63`. |
| Ausência: telas/API | `app/ponto/views.py:1005`, `2079`, `2190`, `2230`, `2293`, `2370`, `2615`, `2705`, `2801`, `2853`, `2916`; `app/ponto/views_tipos_ausencia.py:24`; `app/api/views.py:1056`, `1853`, `1865`, `3076`; `app/api/views_mensageria.py:744`, `763`, `1034`, `1073`, `1246`; `app/colaboradores/services/detalhe.py:104`; `app/colaboradores/views.py:2927`; `app/ponto/admin.py:44`. |
| Ausência: templates | `app/templates/ponto/ausencias.html:150`; `app/templates/ponto/partials/painel_ausencia.html:51`, `123`, `152`, `275`, `310`; `app/templates/ponto/espelho.html:191`; `app/templates/ponto/partials/painel_fechamento.html:55`; `app/templates/ponto/historico_atestados.html:39`; `app/templates/chamados/fio_colaborador.html:62`; `app/templates/colaboradores/detalhe.html:74`; `app/templates/relatorios/afastados.html:40`. |
| Ausência: comandos de diagnóstico/cura | `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:99`, `142`, `167`; `app/ponto/management/commands/curar_ausencia_sem_fim.py:29`, `38`, `49`; `app/ponto/management/commands/ausencia_bloqueante_sem_fim.py:27`; `app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:36`; `app/ponto/management/commands/fechar_chamados_ausencia.py:34`; `app/ponto/management/commands/reconciliar_grade.py:401`; `app/ponto/management/commands/processar_alertas_turno.py:97`; `app/ponto/management/commands/processar_alertas_avancados.py:115`; `app/ponto/management/commands/detectar_ausencias.py:819`; `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:33`; `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:44`; `app/ponto/management/commands/diff_calculador.py:408`; `app/chamados/management/commands/auditar_invariantes_chamados.py:162`, `642`, `1469`. |
| Justificativa: cálculo e operação | `app/ponto/services/efeito_ausencia.py:40`; `app/escala/utils.py:1197`; `app/relatorios/management/commands/diff_extrato_parcial.py:42`; `app/ponto/views.py:1001`, `1707`, `1739`; `app/api/views.py:2095`; `app/core/views.py:205`, `244`, `1191`; `app/chamados/views.py:596`; `app/ponto/utils_turno_orfao.py:53`; `app/templates/ponto/partials/painel_justificativa.html:47`. |
| Batida retratada: apuração e projeções | `app/ponto/turnos.py:1602`, `1864`, `2221`, `2353`, `2469`, `2876`, `2915`; `app/ponto/motor_calculo_v2.py:671`; `app/ponto/services/cartorio.py:381`, `1204`; `app/chamados/reconciliador.py:217`; `app/chamados/juizes.py:758`, `817`; `app/chamados/services/materializacao.py:640`; `app/colaboradores/services/calendario.py:240`, `621`; `app/core/services/painel_op.py:93`; `app/escala/services/propositor.py:478`; `app/escala/services/meio_periodo.py:160`; `app/ferias/situacao.py:279`. |
| Batida retratada: demais consumidores | `app/ponto/registro_batida.py:109`, `141`; `app/api/views.py:676`; `app/api/views_core.py:667`; `app/colaboradores/services/ficha.py:145`; `app/colaboradores/services/pos_vinculo.py:39`; `app/colaboradores/services/senha_provisoria.py:25`; `app/chamados/services/fila_por_causa.py:191`; `app/chamados/services/desvio_escala.py:60`; `app/chamados/services/limbo_folgas.py:38`, `157`; `app/ponto/services/passivo_s84.py:54`; `app/ponto/services/adesao.py:7`, `35`, `58`; `app/ponto/services/piso_intervalo.py:41`; `app/ponto/services/flip_batida.py:15`; `app/ponto/services/flip_auto.py:229`, `287`; `app/ponto/services/lastro.py:262`; `app/ponto/services/faltas_de_hoje.py:55`; `app/ponto/services/deslogados.py:161`. |
| Auditoria: leitores genéricos e outros domínios | `app/core/views.py:705`; `app/chamados/services/historico.py:86`; `app/escala/services/regeneracao.py:58`; `app/chamados/models.py:404`; `app/chamados/reconciliador.py:480`; `app/chamados/juizes.py:1300`; `app/chamados/services/vigia_loop.py:32`, `116`. |

**Os significados divergem nos seguintes pontos:**

- **Fim vazio:** o help do modelo diz indeterminado; o juiz restringe essa interpretação aos tipos em aberto. O histórico de dias decididos usa sempre `data_fim or data`; o copiloto chama qualquer fim vazio de “sem data de volta”. São leituras incompatíveis para uma mesma linha aberta. (`app/ponto/models.py:751`; `app/ponto/turnos.py:2791`; `app/ponto/services/dia_decidido.py:552`, `558`; `app/api/views_mensageria.py:1077`)
- **Status:** há leitores que ainda procuram `Ausencia.status == 'pendente'`, embora os estados abertos sejam `aguardando_documento/aguardando_decisao`. (`app/api/views.py:1061`; `app/templates/ponto/espelho.html:191`; `app/ponto/models.py:701`; `app/ponto/services/ausencia.py:866`)
- **Duração aberta:** o histórico identifica aberto por `dias_corridos` vazio/zero; a criação permite afastamento aberto com o default de um dia. (`app/ponto/services/dia_decidido.py:621`; `app/ponto/services/ausencia.py:216`, `438`)
- **Contagem de faltas:** férias conta registros aprovados cujo início cai no período aquisitivo; folha expande e rateia os dias do intervalo. Uma falta de três dias é um registro para aquele contador. (`app/ferias/models.py:147`; `app/folha/export.py:114`, `118`, `140`)
- **Carimbo decisório:** `save` completa `aprovada_em` também para rejeição; o bulk de férias altera apenas estado e atualização, conservando eventual carimbo/decisor da aprovação anterior. (`app/ponto/models.py:882`; `app/ponto/services/ausencia.py:1418`)
- **Dia civil versus turno:** o evento usa data local do timestamp; a apuração atribui o turno à entrada. Essa divergência alcança o novo desfazer. (`app/ponto/services/fechamento.py:1567`, `1809`, `627`; `app/ponto/services/retratar_humano.py:212`)
- **Auditoria:** antes/depois não possuem esquema universal; os leitores de flip já precisam aceitar string, dict e NULL. O novo payload de rejeição é dict de estado. (`app/core/models.py:300`; `app/ponto/services/flip_batida.py:46`; `app/ponto/services/ausencia.py:1423`)

O fuso configurado é `America/Sao_Paulo`, com `USE_TZ=True`; os timestamps usados pela competência são convertidos para data local. Não identifiquei uma divergência nova de unidade minutos/horas ou de fim inclusivo nesses caminhos. (`app/config/settings/base.py:93`; `app/ponto/services/fechamento.py:1567`; `app/ponto/models.py:875`; `app/ponto/turnos.py:2799`)

**2. Escritores de Ausencia fora dos sítios alterados**

Não identifiquei produtor runtime explícito fora da porta que, em seu caminho normal, escape das invalidações agora acrescentadas. A porta declarada é `services/ausencia.py`, o admin é somente leitura e os chamadores de férias passam pelos helpers. Isso não certifica escrita dinâmica externa: **[nao verificado]**. (`app/core/portas.py:486`; `app/ponto/admin.py:37`; `app/ferias/models.py:444`, `451`; `app/ferias/services.py:683`, `725`)

Há estas distinções necessárias:

- O **escritor físico do modelo** continua fora do diff: `Ausencia.save` deriva minutos, duração, fim e carimbo sem invalidar autonomamente. Sua cobertura depende de quem o chama. (`app/ponto/models.py:878`; `app/ponto/services/ausencia.py:938`, `1659`)
- A migração histórica escreve status diretamente e sua reversão também; nesses sítios não há invalidação. Isso é código de migração, sem evidência de execução nesta produção. (`app/ponto/migrations/0057_a4_backfill_pendente.py:43`, `63`)
- A exclusão de ensaio não invalida e exige banco de ensaio. (`app/ponto/services/ausencia.py:1643`)
- A FK de agendamento tem `CASCADE`; a exclusão individual do modelo chama o helper antes de apagar. Um bypass por exclusão bulk do pai não recebe esse override, mas não identifiquei chamador runtime desse bypass. (`app/ponto/models.py:796`; `app/ferias/models.py:451`)
- Testes escrevem diretamente, inclusive por bulk; isso não demonstra escritor de produção. (`app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:79`, `81`; `app/ponto/tests/test_hj091_ferias_derivadas.py:945`)

O backfill de origem altera somente `agendamento_ferias_id`; não altera os insumos financeiros enumerados. (`app/ponto/services/ausencia.py:1514`, `1525`)

**3. Juiz duplicado, derivação paralela, escritor e relógio**

A nova função delega os dias a `dias_da_ausencia`, depois delega a invalidação; esta delega as competências a `competencias_do_fato`, que adapta `competencias_do_intervalo`. Não introduz outra fórmula de cobertura ou de competência. (`app/ponto/services/ausencia.py:142`, `149`; `app/ponto/services/fechamento.py:1649`, `1594`)

Ela calcula, porém, **um envelope entre o menor início e o maior fim**. Mover uma ausência entre intervalos distantes invalida também competências do buraco entre eles. É invalidação conservadora além da união exata dos fatos; não é nova apuração financeira. (`app/ponto/services/ausencia.py:145`; `app/ponto/services/fechamento.py:1656`)

A rejeição bulk permanece um segundo mecanismo de transição em relação à máquina `mudar_estado`, mas já existia no diff-base anexado; a alteração acrescenta fotografia, trilha e invalidação. O log continua passando pelo escritor existente. (`app/ponto/services/ausencia.py:909`, `1412`, `1418`, `1419`; `app/core/observ.py:133`)

**Há relógio novo fora da borda:** `_acordar_projecao` chama `localdate()` internamente, sem parâmetro `hoje/agora`; passa a executar isso inclusive em criação, reversão, espelho e rederivação. Uma decisão também pode chamar o helper duas vezes e obter tetos distintos numa virada de dia. (`app/ponto/services/ausencia.py:137`, `551`, `942`, `1021`, `1055`, `1306`, `1664`)

A premissa “competência futura não tem projeção gravada” não é imposta pelo recálculo, que recebe mês/ano explícitos. Se houver projeção futura de ausência aberta, o teto em hoje a deixa fresca; existência desse caso em produção: **[nao verificado]**. (`app/ponto/services/ausencia.py:130`, `142`; `app/ponto/services/fechamento.py:448`, `898`; `app/ponto/management/commands/recalcular_fechamento.py:78`)

**4. Transações, travas e efeitos antes do commit** [nao verificado]

| Caminho | Ordem e consequência |
|---|---|
| Cancelamento admin de férias | `atomic` → trava agendamento → trava período → rejeita/invalida → salva cancelamento e espelha/apaga/invalida. As novas escritas no fechamento ficam dentro da transação do cancelamento. (`app/ferias/services.py:636`, `639`, `674`, `683`, `689`; `app/ferias/models.py:448`) |
| Cancelamento DP | Salva período e agendamento dentro de `atomic`; o `save` chega ao espelho e à nova invalidação. (`app/ferias/services.py:651`; `app/ferias/models.py:448`; `app/ponto/services/ausencia.py:1306`) |
| Remarcação | Trava agendamento/período; apaga dias antigos e invalida; salva nova janela, criando espelho e invalidando; tudo no mesmo `atomic`. (`app/ferias/services.py:706`, `707`, `725`, `729`; `app/ponto/services/ausencia.py:551`, `1395`) |
| Curador de minutos | A edição e sua nova preparação de invalidação rodam sob trava da ausência e `atomic` do comando. (`app/ponto/management/commands/rederivar_minutos_h06_comp11.py:137`, `139`, `169`) |
| Regularização | Grava justificativa e invalida no mesmo `atomic`; busca e julgamento da órfã acontecem antes dele, sem trava nesse fluxo. (`app/ponto/services/regularizacao.py:52`, `61`, `89`, `107`, `113`) |
| Desfazer retratação | Salva/anota primeiro; o recálculo é registrado em `on_commit`. Dentro de transação espera o commit; em autocommit pode executar durante a chamada. (`app/ponto/services/retratar_humano.py:207`, `213`; `app/ponto/registro_batida.py:256`; `app/ponto/services/fechamento.py:1824`) |

`invalidar_fechamento` tem savepoints e captura exceções: sua falha permite conservar o ato, podendo deixar projeção fresca ou invalidação parcial entre competências. **A nova preparação não possui essa proteção:** importação, relógio e expansão dos dias rodam antes de entrar no helper protegido; uma exceção ali pode desfazer o ato no chamador transacional, ou escapar depois de um `save` já confirmado em autocommit. Ocorrência real: **[nao verificado]**. (`app/ponto/services/fechamento.py:1648`, `1651`, `1661`; `app/ponto/services/ausencia.py:133`, `142

[truncado em 20000 de 27838 caracteres]

## 15/11 10:23 · sessao 0aa7421a8dab · SITUACAO-DESLIGADO-SEM-PORTA · verificador · origem=Code

1. **Selfie: o onboarding continua sendo concluído pelo model.** `salvar_campos(..., ['foto'])` chama `Colaborador.save(update_fields=...)`; quando não há etapas pendentes, o override acrescenta `onboarding_pendente` e `onboarding_completo`. As etapas verificadas são os timestamps de câmera e GPS, e a selfie não os atribui nesse request (`app/core/views.py:1063`; `app/colaboradores/services/colaborador.py:108`; `app/colaboradores/models.py:406`; `app/colaboradores/models.py:420`).

   O receiver de `post_save` de **Colaborador** encontrado é `_isentos_post_save_colaborador`: ele ignora `update_fields`, verifica `raw`/`isento_ponto` e agenda o silenciamento em `on_commit`. Portanto, nesse receiver, trocar save completo por parcial não altera a condição de execução (`app/chamados/signals.py:270`).

   **Há diferenças de persistência além de `situacao`:** `atualizado_em`, declarado `auto_now=True`, fica fora do save parcial. A normalização de CPF e a geração de matrícula ausente continuam acontecendo em memória dentro de `save`, mas esses campos também ficam fora de `update_fields`. A própria view não atribui outro campo de cadastro antes de salvar a foto; os dois campos de onboarding são a ampliação explícita feita pelo model (`app/colaboradores/models.py:381`; `app/colaboradores/models.py:417`; `app/colaboradores/models.py:427`; `app/core/views.py:1048`; `app/colaboradores/services/colaborador.py:108`).

   A resposta de sucesso continua sendo `{'ok': True}`, consumida pela tela para registrar a permissão de câmera em outra requisição. Impacto observado dessas diferenças em usuários de produção: `[nao verificado]` (`app/core/views.py:1066`; `app/templates/core/onboarding.html:223`).

2. **O teste rejeita o save completo descrito e não passa por ausência de chamadas.** O patch é aplicado na classe `Colaborador`, envolve o POST, registra `kwargs.get('update_fields')` antes de executar o método original e rejeita qualquer chamada com `None` (`app/colaboradores/tests/test_lote3_desligado_sem_porta.py:237`; `app/colaboradores/tests/test_lote3_desligado_sem_porta.py:249`; `app/colaboradores/tests/test_lote3_desligado_sem_porta.py:254`).

   Se `FieldFile.save(save=True)` chama `instance.save()` sem argumentos, esse patch captura a chamada e registra `None`, fazendo o teste falhar. O despacho interno de `FieldFile` não está disponível no fonte local; sua confirmação e a execução efetiva contra **77dfa94fa** ficam `[nao verificado]`. A interceptação e a assertiva estão em `app/colaboradores/tests/test_lote3_desligado_sem_porta.py:239` e `app/colaboradores/tests/test_lote3_desligado_sem_porta.py:254`.

   Os escapes sugeridos não produzem aprovação vazia:

   - Redirect/HTML faz `r.json()` falhar; JSON sem `ok=True` reprova; nenhuma chamada capturada reprova (`app/colaboradores/tests/test_lote3_desligado_sem_porta.py:251`; `app/colaboradores/tests/test_lote3_desligado_sem_porta.py:252`).
   - Não há middleware de onboarding nas listas declaradas; o gate de plataforma libera UA ausente, compatível com esse `Client()` (`app/config/settings/base.py:53`; `app/config/settings/saas.py:67`; `app/core/middleware.py:272`; `app/colaboradores/tests/test_lote3_desligado_sem_porta.py:234`).
   - O teste usa `MEDIA_ROOT` temporário; falha de storage/save vira `ok=False` na view e reprova a assertiva (`app/colaboradores/tests/test_lote3_desligado_sem_porta.py:248`; `app/core/views.py:1067`).

   **Limite da cobertura:** ele permite `update_fields=[]` ou outros campos que não incluam `situacao`; não exige `'foto'`, não identifica o colaborador interceptado e não relê a foto no banco. Também não exercita a conclusão das etapas: a fixture marca `onboarding_completo=True`, mas não fornece timestamps de câmera/GPS (`app/colaboradores/tests/test_lote3_desligado_sem_porta.py:52`; `app/colaboradores/tests/test_lote3_desligado_sem_porta.py:239`; `app/colaboradores/tests/test_lote3_desligado_sem_porta.py:253`).

3. **Não encontrei chamador de produção com `situacao` em `tocar`.** Os usos encontrados de `tocar` levam `atualizado_em` (`app/core/views.py:66`; `app/core/views.py:148`; `app/api/views.py:2483`; `app/api/views.py:2501`; `app/api/views.py:2752`; `app/api/views.py:2763`; `app/api/views_core.py:1013`; `app/api/views_core.py:1070`; `app/api/views_core.py:1081`).

   Os dois chamadores de `gravar` que passam `situacao` em `campos` usam operação `'situacao'`: lançamento de INSS e reversor de afastamento. Outra operação seria recusada por `_conferir` antes da guarda, pois só `'situacao'` autoriza esse campo (`app/ponto/views.py:2472`; `app/ponto/management/commands/reverter_situacao_afastado.py:73`; `app/colaboradores/services/colaborador.py:38`; `app/colaboradores/services/colaborador.py:93`).

4. **Não identifiquei inversão prévia Disputa/Chamado/Pergunta → Colaborador nos chamadores atuais abaixo.** Todos os `salvar_inteiro` entram no levantamento porque escrevem `situacao` independentemente da operação (`app/colaboradores/services/colaborador.py:115`).

   | Chamador que escreve `situacao` | Transação e trava anterior à porta |
   |---|---|
   | `vincular_posto` → `salvar_inteiro('posto')` | Sem `atomic` local; nenhuma trava prévia no caminho (`app/colaboradores/views.py:310`; `app/colaboradores/views.py:329`). |
   | `editar_colaborador` → `salvar_inteiro('cadastro')` | `atomic` começa imediatamente antes da porta; nenhuma trava prévia nesse bloco (`app/colaboradores/views.py:1343`; `app/colaboradores/views.py:1345`). |
   | `transferir_posto` → `salvar_inteiro('posto')` | Dentro de `atomic`; antes da porta há apenas atribuições (`app/colaboradores/views.py:1387`; `app/colaboradores/views.py:1391`). |
   | `executar_desligamento` → `salvar_inteiro('situacao')` | Dentro de `atomic`; primeira gravação do bloco. A cascata de disputas/chamados vem depois (`app/colaboradores/services/desligamento.py:128`; `app/colaboradores/services/desligamento.py:133`; `app/colaboradores/services/desligamento.py:164`; `app/colaboradores/services/desligamento.py:186`). |
   | `lancar_ausencia` → `gravar(situacao='afastado')` | Sem `atomic` externo local; o bloco próprio de `criar_ausencia` já terminou quando a view grava situação (`app/ponto/views.py:2452`; `app/ponto/views.py:2472`; `app/ponto/services/ausencia.py:478`; `app/ponto/services/ausencia.py:512`). |
   | `reverter_situacao_afastado` → `gravar(situacao=SITUACAO_VOLTA)` | Sem `atomic` local nem trava anterior no comando (`app/ponto/management/commands/reverter_situacao_afastado.py:54`; `app/ponto/management/commands/reverter_situacao_afastado.py:73`). |

   O `gate_disputa` apenas consulta e delega; não mantém transação/trava. No desligamento ele é isento e delega diretamente (`app/colaboradores/mixins.py:300`; `app/colaboradores/mixins.py:307`; `app/colaboradores/mixins.py:313`; `app/colaboradores/views.py:1425`).

   Não encontrei `salvar_campos`/`atualizar_em_lote` de produção levando `situacao`; os usos encontrados levam foto, onboarding, cadastro ou aparelho (`app/core/views.py:1065`; `app/core/views.py:1094`; `app/core/views.py:1140`; `app/colaboradores/views.py:1218`; `app/colaboradores/management/commands/importar_telefones_nexti.py:113`; `app/ponto/views.py:1964`; `app/colaboradores/fcm.py:61`; `app/colaboradores/management/commands/backfill_plataforma_fossil.py:73`; `app/colaboradores/management/commands/normalizar_identificadores.py:109`).

   O ramo **agendado** usa operação `'situacao'`, mas grava somente data e motivo, sem escrever situação (`app/colaboradores/services/desligamento.py:110`).

   Assim, adquirir `trava_na_ordem(colab.pk)` antes da guarda respeitaria a ordem nesses caminhos, **desde que o mesmo `atomic` abranja guarda e gravação**. A ordem implementada é Colaborador → Disputa → Chamado → Pergunta. A guarda atual consulta sem essa trava e salva depois; a janela permanece no código (`app/chamados/models.py:2744`; `app/chamados/models.py:2753`; `app/chamados/models.py:2766`; `app/chamados/models.py:2770`; `app/colaboradores/services/colaborador.py:71`; `app/colaboradores/services/colaborador.py:98`). Ausência de deadlock em execução concorrente: `[nao verificado]`.

   **`executar_desligamento` não trava explicitamente a linha antes de gravar desligado:** abre `atomic`, atribui os campos e chama `salvar_inteiro`; para novo valor `'desligado'`, a guarda retorna antes de consultar o banco (`app/colaboradores/services/desligamento.py:128`; `app/colaboradores/services/desligamento.py:133`; `app/colaboradores/services/colaborador.py:57`). Os chamadores UI e cron também não fazem trava prévia (`app/colaboradores/views.py:1431`; `app/colaboradores/views.py:1437`; `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:38`; `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:57`). Efeitos físicos da trava implícita do `UPDATE`, sem banco: `[nao verificado]`.

## 15/11 10:23 · sessao 9f18f348c188 · E4C-F1 · construtor · origem=Code

Entrega **parcial**: 14 unidades migradas, E parcial e `_retratada_com_trilha` pausada por duas perguntas de lei. O selo zero permanece RED. HEAD conferido: `714a92a612907cd94acdf77395b2e8f39fd4c537`. [Entrega completa](/home/ronald/wt-codex-e4c-f1-095617/app/chamados/tests/e4c_f1_entrega.txt:1).

As linhas antigas são os acessos indicados no preparo; as novas são as definições atuais. [Mapa reconferido](/home/ronald/wt-codex-e4c-f1-095617/app/chamados/tests/e4c_f1_entrega.txt:6).

| Unidade | Antiga → nova | Destino aplicado | [nao verificado]
|---|---|---|
| `_retratada_com_trilha` | 288 → 358 | Pausada: L2 |
| `_slot_esperado` | 348,360 → 417 | D01; alimenta o derivador F4 |
| `_pergunta_materializada_de_fato` | 679,691,707 → 737 | E/DP/A06; escolha pausada |
| `_chave_memo_pv` | 852–854 → 900 | D06: ato e efeitos |
| `_resposta_concorda_com_a_luz` | 1356 → 1399 | D01 |
| `projecao_temporal_da_pergunta` | 1385–1386 → 1447 | DP até o instante + E |
| `pergunta_sem_lastro_ou_viva` | 1496 → 1558 | P/E |
| `respondida_sem_texto` | 1528 → 1591 | D01/conservação |
| `q_respondida_sem_texto` | 1548 → 1659 | Subquery/conservação |
| `q_respondida_com_texto` | 1556 → 1664 | Subquery/conservação |
| `perguntas_corrigiveis` | 1715 → 1823 | D01/P/E |
| `perguntas_respondidas_a_validar` | 1724 → 1830 | D01/P |
| `declarou_hora` | 1750 → 1845 | D01 |
| `delta_da_resposta` | 1897 → 1997 | D01 |
| `classificar_resposta_validacao` | 2001,2006 → 2103 | D01 |
| `motivo_implausivel_validacao` | 2023 → 2125 | D01 |

A leitura mecânica foi criada em [atos_pergunta.py:19](/home/ronald/wt-codex-e4c-f1-095617/app/chamados/services/atos_pergunta.py:19). A06, conservação E4b, assinatura A05 e sondador da guarda de escritor foram preservados; A03/A17/A18/A20 permanecem fora da migração desta fatia. [Exceções](/home/ronald/wt-codex-e4c-f1-095617/app/chamados/tests/e4c_f1_entrega.txt:24).

Os seis testes F1 cobrem fonte no ato, projeção contraditória, reabertura, invalidação do memo, passivo e A06. Entraram também os nove casos `DecisoesTest`, o selo AST com apelidos e seu mutante. [Provas e comandos individuais](/home/ronald/wt-codex-e4c-f1-095617/app/chamados/tests/e4c_f1_entrega.txt:49). [nao verificado]

Resultados locais:

- RED contra a base: **4 falhas de asserção**, com passivo/A06 preservados. [Registro](/home/ronald/wt-codex-e4c-f1-095617/app/chamados/tests/e4c_f1_red.txt:1).
- Fonte aplicada: **27 passam; 1 falha**, no selo zero pelos dois resíduos publicados. [Registro](/home/ronald/wt-codex-e4c-f1-095617/app/chamados/tests/e4c_f1_memoria.txt:1). [nao verificado]
- **31 mutantes mordidos**, sem contar erros de import como mordida. [Resultados](/home/ronald/wt-codex-e4c-f1-095617/app/chamados/tests/e4c_f1_mutantes.txt:1), [comando de cada mutante](/home/ronald/wt-codex-e4c-f1-095617/app/chamados/tests/e4c_f1_entrega.txt:69).
- `py_compile` executado sem gerar bytecode. Ruff indisponível, rc 127. [Compilação](/home/ronald/wt-codex-e4c-f1-095617/app/chamados/tests/e4c_f1_compile.txt:1), [ruff](/home/ronald/wt-codex-e4c-f1-095617/app/chamados/tests/e4c_f1_ruff.txt:1).

O [censo por unidade](/home/ronald/wt-codex-e4c-f1-095617/app/chamados/tests/e4c_f1_entrega.txt:7) relaciona escritores e leitores; o [esmeril semântico](/home/ronald/wt-codex-e4c-f1-095617/app/chamados/tests/e4c_f1_entrega.txt:38) distingue instante do ato, hora trabalhada, dia do turno e dia civil. Comparações PostgreSQL, contagens reais DP/Batida/Ausencia/FechamentoMensal e reconstrução histórica integral dos efeitos: **[nao verificado]**; os testes ORM ficaram preparados para a sessão. [Contrato ORM](/home/ronald/wt-codex-e4c-f1-095617/app/chamados/tests/test_e4c_f1_orm.py:44). [nao verificado]

As duas perguntas de lei são:

1. Qual fato substitui C na materialização de **escolha**, preservando a distinção entre validação e efeito? [Ramo preservado](/home/ronald/wt-codex-e4c-f1-095617/app/chamados/juizes.py:786).
2. Qual fato conserva o **fechamento obsoleto auditado**, distinguindo-o da remoção do motivo? [Unidade preservada](/home/ronald/wt-codex-e4c-f1-095617/app/chamados/juizes.py:358).

**PERGUNTA DE POUSO ANTECIPADA:** a mão do Ronald precisa definir esses dois fatos antes do selo zero; depois, Code/suite e opus final certificam. Necessidade adicional em kernel de batida, dinheiro ou tela: **[nao verificado]**. [Pendências de pouso](/home/ronald/wt-codex-e4c-f1-095617/app/chamados/tests/e4c_f1_entrega.txt:110). [nao verificado]

LEI-AKITA: origem=chamados/juizes.py (16 unidades), testemunha=DecisaoPergunta (sequencia ato_em,pk) + juiz registrado do destino, RED=test_e4c_f1_juizes_le_o_ato.F1MemoriaTest+DecisoesTest+SeloF1Test (selo zero RED), quem-mais-le=censo em chamados/tests/e4c_f1_entrega.txt:7, juizes novos=0, padrao=PARALLEL-CHANGE

## 15/11 10:21 · sessao 71fb7e8253e2 · - · - · origem=Ronald (tmux)

Q4 — censo independente, somente leitura:

| Escrita de `situacao` | `atomic` no chamador | Trava prévia de Disputa/Chamado/Pergunta |
|---|---|---|
| `vincular_posto → salvar_inteiro(..., 'posto')` | Sem `atomic` local | Nenhuma no caminho até a porta — `app/colaboradores/views.py:310`, `app/colaboradores/views.py:319`, `app/colaboradores/views.py:329`. |
| `editar_colaborador → salvar_inteiro(..., 'cadastro')` | Sim; porta é a primeira operação no bloco | Nenhuma — `app/colaboradores/views.py:1343`, `app/colaboradores/views.py:1345`. |
| `transferir_posto → salvar_inteiro(..., 'posto')` | Sim; antes da porta só atribui posto/empresa | Nenhuma — `app/colaboradores/views.py:1387`, `app/colaboradores/views.py:1391`. O decorator consulta disputa e delega, sem transação/trava — `app/colaboradores/mixins.py:300`, `app/colaboradores/mixins.py:307`, `app/colaboradores/mixins.py:313`. |
| `executar_desligamento → salvar_inteiro(..., 'situacao')` | Sim; primeira escrita no bloco | Nenhuma antes da gravação — `app/colaboradores/services/desligamento.py:128`, `app/colaboradores/services/desligamento.py:133`. Cascata de disputas/chamados vem depois — `app/colaboradores/services/desligamento.py:164`, `app/colaboradores/services/desligamento.py:186`. |
| `lancar_ausencia → gravar(..., {'situacao': 'afastado'})` | Sem `atomic` externo local | `criar_ausencia` roda antes, mas seu bloco próprio já terminou antes de a view escrever situação — `app/ponto/views.py:2400`, `app/ponto/views.py:2452`, `app/ponto/views.py:2472`, `app/ponto/services/ausencia.py:478`, `app/ponto/services/ausencia.py:479`. |
| `reverter_situacao_afastado → gravar(..., {'situacao': SITUACAO_VOLTA})` | Sem `atomic` local | Nenhuma — `app/ponto/management/commands/reverter_situacao_afastado.py:54`, `app/ponto/management/commands/reverter_situacao_afastado.py:62`, `app/ponto/management/commands/reverter_situacao_afastado.py:73`. |

Não encontrei chamador de produção de `salvar_campos` ou `atualizar_em_lote` levando `situacao`; o inventário positivo acima cobre os escritores localizados, incluindo todo `salvar_inteiro`, que sempre guarda e grava situação independentemente da operação — `app/colaboradores/services/colaborador.py:115`, `app/colaboradores/services/colaborador.py:118`, `app/colaboradores/services/colaborador.py:119`. Não surgiram chamadores dessas portas em `scripts/` ou `bin/` [nao verificado por execução].

O caminho agendado do executor usa operação `'situacao'`, mas grava somente data e motivo; portanto não entra na guarda atual — `app/colaboradores/services/desligamento.py:108`, `app/colaboradores/services/desligamento.py:110`, `app/colaboradores/services/colaborador.py:94`.

**Conclusão estática:** não localizei inversão prévia dessas quatro entidades nos chamadores existentes. Adquirir `trava_na_ordem(colab.pk)` antes da guarda, com a mesma transação abrangendo a gravação, respeita a ordem nos caminhos examinados; a ordem efetiva é Colaborador, Disputa, Chamado, Pergunta — `app/chamados/models.py:2744`, `app/chamados/models.py:2753`, `app/chamados/models.py:2766`, `app/chamados/models.py:2770`. Ausência de deadlock em execução concorrente [nao verificado].

**Executor:** não faz `select_for_update`/`trava_na_ordem` do colaborador antes de gravar desligado. Entra no `atomic`, atribui os campos e chama `salvar_inteiro`; além disso, novo valor desligado faz a guarda retornar sem sequer consultar a linha — `app/colaboradores/services/desligamento.py:128`, `app/colaboradores/services/desligamento.py:133`, `app/colaboradores/services/colaborador.py:57`, `app/colaboradores/services/colaborador.py:118`. A gravação antecede as travas da cascata; efeitos físicos das travas implícitas do `UPDATE`, sem banco [nao verificado].

Os dois chamadores localizados do executor também não pretravam: view carrega por `get_object_or_404` e chama executor; cron filtra e chama executor no laço — `app/colaboradores/views.py:1431`, `app/colaboradores/views.py:1437`, `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:38`, `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:57`. O decorator isento da view delega diretamente — `app/colaboradores/views.py:1425`, `app/colaboradores/mixins.py:301`.

## 15/11 10:21 · sessao 7811028f26d1 · - · - · origem=Ronald (tmux)

Censo estático da cópia, sem executar testes nem acessar banco/log: [nao verificado]

- Nenhum chamador de produção encontrado passa `situacao` em `tocar`; os usos encontrados passam somente `atualizado_em`: `app/api/views.py:2483`, `app/api/views.py:2501`, `app/api/views.py:2752`, `app/api/views.py:2763`, `app/api/views_core.py:1013`, `app/api/views_core.py:1070`, `app/api/views_core.py:1081`, `app/core/views.py:66`, `app/core/views.py:148`.
- Os únicos `gravar` de produção com `situacao` em `campos` usam operação `'situacao'`: lançamento INSS, `app/ponto/views.py:2472`; reversor, `app/ponto/management/commands/reverter_situacao_afastado.py:73`. Outra operação seria recusada antes da guarda: somente `'situacao'` autoriza esse campo em `app/colaboradores/services/colaborador.py:29`, `app/colaboradores/services/colaborador.py:38`, e `_conferir` precede a guarda em `app/colaboradores/services/colaborador.py:93`.
- `salvar_inteiro` grava `situacao` qualquer que seja a operação; portanto os usos `'posto'` e `'cadastro'` entram no censo de corrida: `app/colaboradores/services/colaborador.py:112`.

| Chamador que escreve `situacao` | Atomic e trava anterior de Disputa/Chamado/Pergunta |
|---|---|
| `vincular_posto` → `salvar_inteiro(..., 'posto')` | Sem atomic local e sem trava prévia no caminho até a chamada: `app/colaboradores/views.py:310`, `app/colaboradores/views.py:319`, `app/colaboradores/views.py:329`. |
| `editar_colaborador` → `salvar_inteiro(..., 'cadastro')` | Atomic começa imediatamente antes; nenhuma trava prévia nesse bloco: `app/colaboradores/views.py:1343`, `app/colaboradores/views.py:1345`. A view começa com leitura simples: `app/colaboradores/views.py:1243`. |
| `transferir_posto` → `salvar_inteiro(..., 'posto')` | Atomic precede apenas atribuições e a chamada: `app/colaboradores/views.py:1387`, `app/colaboradores/views.py:1391`. O decorator não adquire trava nem atomic: `app/colaboradores/mixins.py:300`, `app/colaboradores/mixins.py:307`, `app/colaboradores/mixins.py:313`. |
| `executar_desligamento` efetivo → `salvar_inteiro(..., 'situacao')` | Atomic começa em `app/colaboradores/services/desligamento.py:128`; primeira gravação do bloco em `app/colaboradores/services/desligamento.py:133`. Cascata sobre disputas/chamados vem depois: `app/colaboradores/services/desligamento.py:164`, `app/colaboradores/services/desligamento.py:186`. |
| `lancar_ausencia` → `gravar(..., {'situacao': 'afastado'})` | Sem atomic externo local na view: `app/ponto/views.py:2399`, `app/ponto/views.py:2407`. `criar_ausencia` retorna antes da chamada: `app/ponto/views.py:2452`, `app/ponto/views.py:2472`; seu atomic de criação envolve `a.save()` e já terminou no retorno: `app/ponto/services/ausencia.py:478`, `app/ponto/services/ausencia.py:512`. |
| Reversor → `gravar(..., {'situacao': SITUACAO_VOLTA})` | Sem atomic/trava anterior no comando: `app/ponto/management/commands/reverter_situacao_afastado.py:54`, `app/ponto/management/commands/reverter_situacao_afastado.py:61`, `app/ponto/management/commands/reverter_situacao_afastado.py:73`. `quem_ja_voltou` usa consulta simples: `app/ponto/services/afastado_avisa.py:126`. |

Não encontrei chamador de produção de `salvar_campos`/`atualizar_em_lote` que inclua `situacao`; os usos são foto/onboarding/cadastro/aparelho: `app/core/views.py:1065`, `app/core/views.py:1094`, `app/core/views.py:1140`, `app/colaboradores/views.py:1218`, `app/colaboradores/management/commands/importar_telefones_nexti.py:113`, `app/ponto/views.py:1964`, `app/colaboradores/fcm.py:61`, `app/colaboradores/management/commands/backfill_plataforma_fossil.py:73`, `app/colaboradores/management/commands/normalizar_identificadores.py:109`.

Logo, **não identifiquei inversão D/Ch/P → Colaborador nesses chamadores atuais** se a porta passasse a chamar `trava_na_ordem(colab.pk)` antes da guarda, mantendo guarda e save no mesmo atomic; a aquisição central efetivamente começa por Colaborador e segue Disputa → Chamado → Pergunta: `app/chamados/models.py:2744`, `app/chamados/models.py:2753`, `app/chamados/models.py:2766`, `app/chamados/models.py:2770`. A definição SaaS não habilita `ATOMIC_REQUESTS`: `app/config/settings/saas.py:83`; configuração efetivamente carregada em produção: `[nao verificado]`.

`executar_desligamento` **não chama trava explícita da linha de Colaborador antes de gravar desligado**: abre atomic, atribui e salva diretamente, `app/colaboradores/services/desligamento.py:128`, `app/colaboradores/services/desligamento.py:133`. Para novo valor `'desligado'`, a guarda retorna antes de consultar a linha: `app/colaboradores/services/colaborador.py:57`, `app/colaboradores/services/colaborador.py:58`. O save emite a escrita antes da cascata; a trava implícita do `UPDATE` não equivale à leitura prévia por `select_for_update` que o usuário propõe: `app/colaboradores/services/colaborador.py:119`, `app/chamados/models.py:2744`.

Os chamadores indiretos do executor também não adquirem trava antes: UI faz leitura simples e chama executor, com `gate_disputa(isento=True)` (`app/colaboradores/views.py:1425`, `app/colaboradores/views.py:1431`, `app/colaboradores/views.py:1437`; bypass em `app/colaboradores/mixins.py:301`); cron usa queryset comum e chama executor (`app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:38`, `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:57`).

Detalhe adicional: `Colaborador.save` pode chamar `trava_na_ordem` para gerar matrícula ausente, mas é trava de outro Colaborador da faixa, sem D/Ch/P pedidos, e acontece dentro do save, depois da guarda da porta: `app/colaboradores/models.py:427`, `app/colaboradores/models.py:436`, `app/colaboradores/services/colaborador.py:118`.

## 15/11 10:21 · sessao 8386020a0f5c · - · - · origem=Ronald (tmux)

1. A selfie continua chamando `Colaborador.save`, agora via `salvar_campos(..., ['foto'])`; portanto a conclusão automática continua: se `pk` existe e não há etapas pendentes, o model acrescenta `onboarding_pendente` e `onboarding_completo` ao `update_fields`. As etapas são `camera_concedida_em` e `gps_concedido_em`, não a existência da foto (`app/core/views.py:1063-1065`; `app/colaboradores/services/colaborador.py:102-108`; `app/colaboradores/models.py:406-426`).

2. O único receiver de `post_save` de **Colaborador** encontrado na cópia é `_isentos_post_save_colaborador`: ele ignora `update_fields`, retorna para `raw` ou não isento e agenda `silenciar_do_colab` em `on_commit`. Não possui dependência de save completo (`app/chamados/signals.py:270-281`; registro em `app/chamados/apps.py:8-10`).

3. Há diferença além de impedir regravação de `situacao`: `atualizado_em` é `auto_now=True` e não entra mais no save da selfie. Além disso, `save` continua normalizando `cpf` e gerando `matricula_hasner` quando faltante, mas esses campos não pertencem a `['foto']`, nem são acrescentados pelo override. Logo deixam de persistir como efeito colateral desse pedido; a atribuição de onboarding foi explicitamente preservada (`app/colaboradores/models.py:381`; `app/colaboradores/models.py:415-439`; `app/colaboradores/services/colaborador.py:108`). Efeito real dessas diferenças sobre usuários de produção: `[nao verificado]`.

4. Antes da gravação, a própria view apenas recupera `request.user.colaborador`, decodifica o POST e atribui a foto; não atribui contato, permissões ou outro campo de cadastro. As permissões são outra view, com seu próprio save parcial (`app/core/views.py:1048-1065`; `app/core/views.py:1073-1095`). Não há middleware de onboarding nas listas declaradas; o gate de plataforma libera UA ausente, compatível com o `Client()` sem UA do teste (`app/config/settings/base.py:53-66`; `app/config/settings/saas.py:67-74`; `app/core/middleware.py:272-274`; `app/colaboradores/tests/test_lote3_desligado_sem_porta.py:234`).

5. O teste não passa vazio: exige resposta JSON com `ok=True`, lista `chamadas` não vazia, e examina **todas** as chamadas capturadas, rejeitando `update_fields=None` e presença de `situacao`. Redirect/HTML faz `r.json()` falhar; JSON de erro falha na primeira assert; ausência de save falha na segunda (`app/colaboradores/tests/test_lote3_desligado_sem_porta.py:250-255`). Erros da view são devolvidos com `ok=False`, inclusive erros de storage/save (`app/core/views.py:1067-1068`).

6. O teste isola upload usando diretório temporário e `override_settings(MEDIA_ROOT=media)`; o backend declarado em SaaS é `FileSystemStorage`. Falha ao criar diretório/storage não é convertida em verde (`app/colaboradores/tests/test_lote3_desligado_sem_porta.py:248-251`; `app/config/settings/saas.py:109-112`). A execução real sob esses settings: `[nao verificado]`.

7. Pelo caminho esperado de `FieldFile.save(save=True)`, o save completo seria `instance.save()` e o patch de classe interceptaria essa chamada: `original = Colaborador.save` é guardado antes, e `mock.patch.object(Colaborador, 'save', espia)` envolve o POST. Assim a versão anexa `save=True` produziria `None` em `kwargs.get('update_fields')`, rejeitado pela assert (`app/colaboradores/tests/test_lote3_desligado_sem_porta.py:237-254`; caminho antigo fornecido no anexo). **Limite:** a implementação do Django não está presente na cópia acessível; confirmação desse dispatch interno e execução contra 77dfa94fa: `[nao verificado]`.

8. O teste é adequado para detectar save completo, mas não demonstra literalmente “só a foto”: permite qualquer conjunto de campos que não contenha `situacao`, não confere PK das chamadas e não relê `foto` do banco. Também não exercita a conclusão das etapas, pois a fixture só estabelece `onboarding_completo=True`, deixando GPS/câmera sem timestamps (`app/colaboradores/tests/test_lote3_desligado_sem_porta.py:52-58`; `app/colaboradores/tests/test_lote3_desligado_sem_porta.py:239-255`; `app/colaboradores/models.py:375-376`).

## 15/11 10:19 · sessao aa799e8e7d78 · - · - · origem=Ronald (tmux)

Censo estático, somente leitura, sem banco e sem executar testes. A evidência abaixo identifica os acessos encontrados; exaustividade contra SQL dinâmico ou execução externa é [nao verificado]. [nao verificado]

**Escritores de Ausencia**

| Campo | Escritores de produção |
|---|---|
| `tipo`, `data` | Nascimento em `app/ponto/services/ausencia.py:437`/`:483`/`:516`; edição em `app/ponto/services/ausencia.py:602`/`:603`/`:613`. |
| `data_fim` | Nascimento e edição derivados por `derivar_data_fim`, `app/ponto/services/ausencia.py:152`, `:289`, `:439`, `:604`; o modelo completa fim ausente em `app/ponto/models.py:912`/`:917`; `mudar_estado` persiste também essa derivação, `app/ponto/services/ausencia.py:936`; rederivação pelo save completo, `app/ponto/services/ausencia.py:1659`. |
| `dias_corridos` | Entrada criação/edição em `app/ponto/services/ausencia.py:438`/`:605`; modelo deriva do intervalo inclusivo em `app/ponto/models.py:903`/`:909`/`:911`; transição persiste em `app/ponto/services/ausencia.py:936`; rederivação em `:1659`. |
| `minutos` | Entrada criação/edição em `app/ponto/services/ausencia.py:439`/`:612`; modelo soma previsto para falta aprovada sem minutos, `app/ponto/models.py:887`/`:895`/`:897`; transição persiste em `app/ponto/services/ausencia.py:936`; rederivação em `:1659`. |
| `status` | Criação, `app/ponto/services/ausencia.py:440`; transição, `:926`/`:938`; rejeição bulk de férias, `:1420`. |
| `aprovada_em` | Criação aprovada, `app/ponto/services/ausencia.py:443`/`:445`; modelo auto-carimba tanto aprovada como rejeitada, `app/ponto/models.py:882`; transição inclui persistência, `app/ponto/services/ausencia.py:936`; decisão aceita instante da borda, `:1026`/`:1027`/`:1037`; rederivação pode completar carimbo, `:1659`. |
| Delete | Espelho inativo e encurtado em `app/ponto/services/ausencia.py:1303` e `:1333`; apagar férias em `:1393`; ensaio em `:1647`; CASCADE da FK de agendamento declarado em `app/ponto/models.py:796`/`:797`. |

A busca explícita e o censo AST existente não encontraram escritor runtime de `Ausencia` fora da porta; o admin é somente leitura, `app/ponto/admin.py:37`/`:38`. O censo reconhece escrituras nomeadas e também declara limitações de inferência, `app/core/censo_escritas.py:8`, `:42`, `:47`. Não use isso como prova absoluta contra escrita dinâmica.

Exceções históricas: migration `0057_a4_backfill_pendente.py` escreve status diretamente em `app/ponto/migrations/0057_a4_backfill_pendente.py:43` e desfaz em `:63`; não invalida fechamento nesses sítios. O delete de ensaio também não invalida, mas exige banco de ensaio, `app/ponto/services/ausencia.py:1643`/`:1646`/`:1647`. O backfill de origem modifica apenas FK, `app/ponto/services/ausencia.py:1524`/`:1525`; não modifica os sete campos pedidos.

**Leitores diretos e leitores centrais**

| Arquivo e pontos | Campos e uso |
|---|---|
| `app/ponto/models.py:720`, `:729`, `:741`, `:852`, `:872`, `:876`, `:882`, `:887`, `:892`, `:903` | Estado aberto/aprovável, tipo/efeito, representação, minutos→horas, carimbo e derivação de intervalo/minutos. |
| `app/ponto/turnos.py:2516`, `:2531`, `:2554`, `:2600`, `:2677`, `:2715`, `:2753`, `:2772`, `:2786`, `:2827`, `:3076`, `:3138`, `:3158` | Autoridades de efeito, interseção, cobertura, parcial, afastamento, sobreposição e carregamento em lote: `status`, `tipo`, `data`, `data_fim`, `minutos`. |
| `app/ponto/precedencia.py:273`, `:337` | `status`, `tipo`, `data`, `data_fim`; filtro e expansão própria de candidatos à precedência. |
| `app/ponto/services/fechamento.py:638`, `:681`, `:694`, `:873` | Ausências aprovadas, cobertura, classificação parcial e rateio de falta; consome `status`, `tipo`, `data`, `data_fim`, `minutos` pelas autoridades. Não encontrei leitura direta de `dias_corridos` ou `aprovada_em` no cálculo mensal. |
| `app/folha/export.py:70`, `:87`, `:117`, `:118`, `:121`, `:140`, `:160` | `tipo`, `status`, datas e minutos; expansão inclusiva e rateio do total pelos dias inteiros do registro. |
| `app/ponto/services/efeito_ausencia.py:47`, `:48`, `:49`, `:50`, `:53`, `:55` | Efeito aprovado, tipo e parcial/minutos; crédito no dia inicial. |
| `app/ponto/services/credito_parcial.py:26`, `:27`, `:28` | Mesmo universo aprovado, tipo/minutos/data para crédito parcial por dia. |
| `app/ponto/services/dia_decidido.py:448`, `:481`, `:493`, `:508`, `:520`, `:550`, `:558`, `:587`, `:614`, `:619` | Estado/tipo/minutos, carimbo decisório, decisões por dia e histórico; lê também `dias_corridos` em `:621` e `:622`. |
| `app/ponto/services/cartorio.py:383`, `:385`, `:1356` | Interseção de datas e carimbo `aprovada_em` para fato novo; cobertura para lavra. |
| `app/ponto/signals.py:123`, `:124` | Tipo/datas para dias a relavrar. |
| `app/ponto/services/vigia_ausencia.py:59`, `:92`, `:104`, `:108`, `:115`, `:120`, `:135`, `:136`, `:215`, `:218` | Estado, tipo, datas e dias_corridos nos diagnósticos. |
| `app/ponto/services/vigia_ausencia_veredito.py:46`, `:53`, `:54` | Aprovação, cobertura, tipo e parcial. |
| `app/ponto/services/alertas_ausencia.py:48`, `:50` | Cobertura aprovada e tipos médicos. |
| `app/chamados/services/materializacao.py:79`, `:83`, `:626`, `:627`, `:630`, `:662`, `:669` | Cobertura, tipo, data e rejeição para absorver/devolver perguntas. |
| `app/chamados/services/silencio_ausencia.py:66`, `:78`, `:94` | Cobertura, estado aberto/aprovado, tipo e data do chamado. |
| `app/chamados/juizes.py:479`, `:1801`, `:2380` | Cobertura/status aprovado; data original por PK. |
| `app/chamados/services/historico.py:129`, `:132` | Tipo/data para evento histórico. |
| `app/chamados/services/foto_e4b.py:251`, `:285`, `:287`, `:288` | Fotografa campos concretos e lê tipo/datas para alcance; aberto fica alcance desconhecido. |
| `app/ferias/models.py:147` | Tipo literal falta, estado aprovado e data inicial para contar faltas do PA. |
| `app/ferias/services.py:147`, `:152`, `:155`, `:156` | Tipo férias, status e data para confrontar espelho com agendamento. |
| `app/relatorios/views.py:197`, `:205`, `:277`, `:292`, `:304` | Estado/tipo/datas para dias médicos e corte civil do relatório. |
| `app/inteligencia/views.py:71`, `:84`, `:232`, `:239`, `:516` | Data inicial e agregados de tipo; `sem_ausencia` usa relação por data inicial. |
| `app/inteligencia/resumo.py:39`, `:42`, `:43`, `:44` | Ordenação por data e contagem de registros aprovados por efeito/tipo. |
| `app/inteligencia/management/commands/calcular_scores.py:12` | Data inicial, status não rejeitado e tipo; contagem de registros. |
| `app/inteligencia/management/commands/gerar_alertas.py:55` | Mesmo padrão de contagem por início no mês civil. |
| `app/colaboradores/services/detalhe.py:104` | Data inicial no mês civil e ordenação. |
| `app/colaboradores/views.py:2927`, `:2928` | Tipo/data/data_fim para afastamento atual. |
| `app/api/views.py:1056`, `:1061`, `:1853`, `:1865`, `:1866`, `:1867`, `:1868`, `:1870`, `:3076`, `:3086` | Estado, tipo, data e minutos: botão de anexo, listagem e guarda de upload. |
| `app/api/views_mensageria.py:744`, `:745`, `:746`, `:748`, `:749`, `:763`, `:1034`, `:1035`, `:1073`, `:1075` | Estado/tipo/datas em contagens e itens do copiloto. |
| `app/ponto/views.py:1005`, `:2079`, `:2190`, `:2230`, `:2293`, `:2334`, `:2370`, `:2615`, `:2638`, `:2648`, `:2705`, `:2722`, `:2724`, `:2766`, `:2801`, `:2811`, `:2853`, `:2877`, `:2916` | Listagens, agrupamento, guardas dos atos, formulário/antes/depois e histórico médico; vários dos sete campos. |
| `app/ponto/views_tipos_ausencia.py:24` | Contagem por tipo gravado. |
| `app/ponto/admin.py:44`, `:45`, `:48` | Exibição/filtro de tipo, data, minutos e estado. |

**Demais consumidores pela autoridade de cobertura**, portanto leitores indiretos de status/tipo/datas, e do parcial/minutos quando indicado:

- Grade: `app/escala/utils.py:1174`, `:1175`; falta do dia em `app/escala/servico_jornada.py:347`, `:348`; cadastro/realidade em `app/escala/services/cadastro_realidade.py:603`; furos em `app/escala/services/furos_diarios.py:222`.
- Espelhos/calendário: `app/ponto/services/espelho.py:582`, `:588`; `app/colaboradores/services/calendario.py:484`, `:532`; API em `app/api/views.py:1566`.
- Cobrança e elegibilidade: `app/chamados/services/adesao.py:69`; `app/chamados/services/vigia_fio.py:308`; `app/ponto/services/deslogados.py:163`; `app/ponto/services/faltas_de_hoje.py:59`; `app/colaboradores/services/situacao_exibida.py:63`; `app/api/views_mensageria.py:1246`.
- Afastamento/aviso: `app/ponto/services/afastado_avisa.py:39`, `:127`, `:129`; `app/ponto/services/aviso_da_batida.py:88`.
- Comandos: `app/ponto/management/commands/reconciliar_grade.py:401`; `processar_alertas_turno.py:97`; `processar_alertas_avancados.py:115`; `supra_juiz.py:216`; `vigia_b5_ausencia.py:68`; `emitir_furo_retroativo.py:99`; `emitir_turno_aberto_retroativo.py:108`; `backfill_b5_insumos.py:72`; `detectar_ausencias.py:819`; `rejulgar_ausencia_em_folga.py:33`; `ausencias_sem_efeito_na_celula.py:44`; `diff_calculador.py:408`.
- Auditoria de chamados: `app/chamados/management/commands/auditar_invariantes_chamados.py:162`, `:642`, `:659`, `:1469`.
- Curadores que leem campos diretamente: `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:99`, `:121`, `:142`, `:147`, `:167`, `:169`; `curar_ausencia_sem_fim.py:30`, `:38`, `:49`, `:56`; `ausencia_bloqueante_sem_fim.py:27`, `:39`; `reabrir_chamados_ausencia_aberta.py:36`, `:43`; `fechar_chamados_ausencia.py:34`.

**Templates leitores diretos**

`app/templates/ponto/ausencias.html:150`, `:159`, `:163`, `:177`; `app/templates/ponto/partials/painel_ausencia.html:51`, `:123`, `:124`, `:125`, `:152`, `:155`, `:157`, `:159`, `:163`, `:275`, `:293`, `:310`, `:313`; `app/templates/ponto/espelho.html:191`; `app/templates/ponto/partials/painel_fechamento.html:55`, `:56`; `app/templates/ponto/historico_atestados.html:39`; `app/templates/chamados/fio_colaborador.html:62`; `app/templates/colaboradores/detalhe.html:74`; `app/templates/relatorios/afastados.html:40`. O instante de aprovação do drawer é convertido via `localtime`, `app/templates/ponto/partials/painel_ausencia.html:275`.

**Divergências comprovadas, herdadas salvo nota**

1. `data_fim=NULL` não significa universalmente aberto: help do campo diz aberto, `app/ponto/models.py:751`/`:753`; juiz só abre tipos `TIPOS_EM_ABERTO`, `app/ponto/turnos.py:2791`/`:2793`/`:2796`.
2. API ainda testa status inexistente `pendente`, `app/api/views.py:1061`; espelho idem, `app/templates/ponto/espelho.html:191`; estados reais em `app/ponto/models.py:701`–`:704`.
3. `decididos_do_periodo` reduz toda linha sem fim a um dia, inclusive tipo aberto, `app/ponto/services/dia_decidido.py:552`/`:558`/`:560`; cobertura abre até a borda fornecida, `app/ponto/turnos.py:2793`.
4. Histórico identifica aberto pelo número de dias None/0, `app/ponto/services/dia_decidido.py:622`; criar afastamento permite/defaulta `dias_corridos=1`, `app/ponto/services/ausencia.py:216`/`:438`. A listagem do copiloto chama qualquer fim nulo de “sem data de volta”, `app/api/views_mensageria.py:1077`.
5. Faltas no PA são quantidade de registros iniciados dentro dele, `app/ferias/models.py:147`–`:150`; folha expande e rateia o intervalo inclusivo, `app/folha/export.py:114`/`:118`/`:121`/`:140`. Range de três dias é um registro para o PA; início anterior ao PA fica fora desse contador.
6. `aprovada_em` é carimbo de decisão, inclusive rejeição no save, `app/ponto/models.py:882`; rejeição bulk de férias não move esse carimbo nem decisor, `app/ponto/services/ausencia.py:1420`. Novo diff acrescenta trilha, mas mantém esse contraste de semântica.
7. Modelo deriva minutos de falta **antes** de completar fim ausente, `app/ponto/models.py:887`/`:892`/`:897` versus `:912`/`:917`. Uma falta aprovada com `dias_corridos>1`, sem fim e minutos zero, rederivada nesse estado, soma só o primeiro dia antes de ganhar o range; defeito herdado do save.
8. A redução da invalidação para tipo de dia único sem fim está coerente com os insumos financeiros encontrados: juiz devolve um dia, `app/ponto/turnos.py:2796`, e rateio usa a contagem desse juiz, `app/folha/export.py:118`/`:121`. Não achei dependência financeira de competências entre esse dia e hoje; prova dinâmica [nao verificado].

## 15/11 10:19 · sessao 8b0fcd9b754a · - · - · origem=Ronald (tmux)

**Achado principal: o novo evento de desfazer herda uma competência civil que pode ser diferente da competência do turno.** `desfazer` passa `batida.timestamp`; a porta converte o instante em data local e recalcula apenas uma competência. O fechamento, porém, atribui o turno ao dia da entrada e busca a saída no dia seguinte. Assim, uma S em 21/10 de turno iniciado em 20/10 pode acordar novembro e conservar outubro com a foto anterior; ocorrência real no dado fornecido: [nao verificado]. (`app/ponto/services/retratar_humano.py:212`; `app/ponto/services/fechamento.py:1567`; `app/ponto/services/fechamento.py:1595`; `app/ponto/services/fechamento.py:1809`; `app/ponto/services/fechamento.py:624`)

**FechamentoMensal.derivado_em**

- Escritores: derivação de registro aberto, `timezone.now()` e `save`; derivação de registro aprovado, `update`; restauração de snapshot, NULL por padrão mas aceita carimbo explícito; invalidação, NULL e atualização de `atualizado_em`. (`app/ponto/services/fechamento.py:1143`; `app/ponto/services/fechamento.py:1160`; `app/ponto/services/fechamento.py:1189`; `app/ponto/services/fechamento.py:1391`; `app/ponto/services/fechamento.py:1653`)
- Leitores: universo de projeções vencidas, contador e comando de exposição; NULL significa nunca derivado ou invalidado, sem distinguir os dois motivos. O contador padrão olha somente a competência corrente por empresa. (`app/ponto/models.py:454`; `app/ponto/services/fechamento.py:1692`; `app/ponto/services/fechamento.py:1702`; `app/ponto/management/commands/projecoes_vencidas.py:49`)
- O hash numérico exclui `derivado_em` e `atualizado_em`; a mera invalidação não altera esse hash. (`app/ponto/portas/regen_exportada.py:35`; `app/ponto/portas/regen_exportada.py:56`; `app/ponto/portas/regen_exportada.py:71`)
- O novo desfazer pode, pelo evento, recalcular números de competência exportada e lavrar DiaPago. Não há emissão de TXT na porta do evento; o download do TXT existente entrega a cópia gravada. Mudança efetiva de hash/TXT/dinheiro em produção: [nao verificado]. (`app/ponto/services/fechamento.py:1790`; `app/ponto/services/fechamento.py:1809`; `app/ponto/services/fechamento.py:1160`; `app/ponto/services/fechamento.py:1191`; `app/folha/views.py:549`)

**Justificativa.status**

- Semântica declarada: `pendente`, `aprovada`, `rejeitada`; `data_turno` é DateField do turno. A unicidade condicional usa `pendente`. (`app/ponto/models.py:130`; `app/ponto/models.py:144`; `app/ponto/models.py:159`; `app/ponto/models.py:189`)
- Escritores de nascimento pendente: API, formulário web, cron de alerta e regularização; os caminhos que reaproveitam pendente salvam tipo/descrição sem mudar status. (`app/api/views.py:1989`; `app/api/views.py:1999`; `app/ponto/views.py:1624`; `app/ponto/views.py:1638`; `app/ponto/management/commands/processar_alertas_turno.py:394`; `app/ponto/services/regularizacao.py:91`)
- Escritores de transição: aprovação DP, rejeição DP, materialização de saída, aprovação em lote, rejeição em lote, reconciliador pendente→rejeitada e regularização existente sem saída→pendente. Aprovação/materialização/lote aprovado já invalidam o fechamento; a regularização ganhou essa invalidação no diff. (`app/ponto/services/justificativa.py:205`; `app/ponto/services/justificativa.py:283`; `app/ponto/services/justificativa.py:383`; `app/ponto/views.py:1799`; `app/ponto/views.py:1878`; `app/chamados/reconciliador.py:532`; `app/ponto/services/regularizacao.py:105`; `app/ponto/services/justificativa.py:214`; `app/ponto/views.py:1807`)
- Leitores de cálculo: fechamento, efeito de ausência/espelho, grade e comando de comparação; todos filtram `aprovada` e usam `data_turno` inclusiva entre as pontas. (`app/ponto/services/fechamento.py:660`; `app/ponto/services/efeito_ausencia.py:40`; `app/escala/utils.py:1197`; `app/relatorios/management/commands/diff_extrato_parcial.py:42`)
- Leitores operacionais/apresentação: fila/histórico web, painel de fechamento, API, dashboard, atendimento e bloqueio de turno; pendente com saída informada significa regularização aguardando DP. (`app/ponto/views.py:1707`; `app/ponto/views.py:1739`; `app/ponto/views.py:1001`; `app/api/views.py:2095`; `app/core/views.py:205`; `app/core/views.py:244`; `app/core/views.py:1191`; `app/chamados/views.py:596`; `app/ponto/utils_turno_orfao.py:53`; `app/templates/ponto/partials/painel_justificativa.html:47`; `app/templates/chamados/meu_atendimento.html:89`)
- Todo save de Justificativa já acorda o cartório por signal, mas esse signal não invalida FechamentoMensal: são projeções distintas. (`app/ponto/signals.py:161`; `app/ponto/signals.py:165`)
- A regularização grava e invalida dentro do mesmo atomic; `invalidar_fechamento` usa savepoints próprios, engole falhas e não impede commit do ato se a invalidação falhar. (`app/ponto/services/regularizacao.py:89`; `app/ponto/services/regularizacao.py:107`; `app/ponto/services/regularizacao.py:113`; `app/ponto/services/fechamento.py:1648`; `app/ponto/services/fechamento.py:1661`)

**Batida.retratada_em**

- É instante nullable: preenchido exclui a batida da geometria/apuração; NULL a inclui independentemente de aprovação DP. Retratar grava `timezone.now()` por `anotar`; desfazer grava NULL pela mesma porta. (`app/ponto/models.py:65`; `app/ponto/turnos.py:2859`; `app/ponto/turnos.py:2876`; `app/ponto/registro_batida.py:276`; `app/ponto/services/retratar_humano.py:207`)
- A porta física aceita `retratada_em` entre campos anotáveis; aplica save e trilha. O diff não introduz escrita direta fora dela. (`app/ponto/registro_batida.py:205`; `app/ponto/registro_batida.py:237`; `app/ponto/registro_batida.py:254`)
- Leitores principais de apuração/geometria: `turnos`, motor, cartório, pareamento/reconciliador, materialização/prova de chamado, adesão, calendário, dashboards, escala e férias. (`app/ponto/turnos.py:1602`; `app/ponto/turnos.py:1614`; `app/ponto/turnos.py:1864`; `app/ponto/turnos.py:1902`; `app/ponto/turnos.py:2221`; `app/ponto/turnos.py:2353`; `app/ponto/turnos.py:2384`; `app/ponto/turnos.py:2469`; `app/ponto/turnos.py:2876`; `app/ponto/turnos.py:2915`; `app/ponto/motor_calculo_v2.py:671`; `app/ponto/services/cartorio.py:1204`; `app/chamados/reconciliador.py:217`; `app/chamados/juizes.py:758`; `app/chamados/juizes.py:817`; `app/chamados/services/materializacao.py:640`; `app/chamados/services/adesao.py:61`; `app/colaboradores/services/calendario.py:240`; `app/colaboradores/services/calendario.py:621`; `app/core/services/painel_op.py:93`; `app/escala/services/propositor.py:478`; `app/escala/services/meio_periodo.py:160`; `app/ferias/situacao.py:279`)
- Outros leitores runtime explícitos: deduplicação/registro, API, última batida/ficha/pós-vínculo/senha provisória, fila por causa, desvio de escala, limbo de folgas, passivo, adesão, piso de intervalo, flip, lastro, faltas de hoje e deslogados. (`app/ponto/registro_batida.py:109`; `app/ponto/registro_batida.py:141`; `app/api/views.py:676`; `app/api/views_core.py:667`; `app/colaboradores/services/ficha.py:145`; `app/colaboradores/services/pos_vinculo.py:39`; `app/colaboradores/services/senha_provisoria.py:25`; `app/chamados/services/fila_por_causa.py:191`; `app/chamados/services/desvio_escala.py:60`; `app/chamados/services/limbo_folgas.py:38`; `app/chamados/services/limbo_folgas.py:157`; `app/ponto/services/passivo_s84.py:54`; `app/ponto/services/adesao.py:7`; `app/ponto/services/adesao.py:35`; `app/ponto/services/adesao.py:58`; `app/ponto/services/piso_intervalo.py:41`; `app/ponto/services/flip_batida.py:15`; `app/ponto/services/flip_auto.py:229`; `app/ponto/services/flip_auto.py:287`; `app/ponto/services/lastro.py:262`; `app/ponto/services/faltas_de_hoje.py:55`; `app/ponto/services/deslogados.py:161`; `app/ponto/services/deslogados.py:174`)
- Leitores do instante, em vez de mera presença: prazo humano de sete dias e frescor de fatos/vereditos. Não há divergência de unidade: ambos usam datetime, mas respondem perguntas diferentes. (`app/ponto/services/retratar_humano.py:48`; `app/ponto/services/retratar_humano.py:53`; `app/ponto/services/retratar_humano.py:75`; `app/ponto/services/retratar_humano.py:109`; `app/ponto/services/cartorio.py:381`; `app/chamados/veredito_lavrado.py:180`)
- Comandos também leem esse campo, incluindo correção retroativa, passivo, censos e migração de datas. Estes são código operacional da árvore, não evidência de execução em produção. (`app/colaboradores/management/commands/corrigir_escala_retroativa.py:95`; `app/ponto/management/commands/passivo_disputa_retro.py:161`; `app/ponto/management/commands/diff_passivo_s84.py:80`; `app/ponto/management/commands/vigia_plantio_overnight.py:73`; `app/ponto/management/commands/diff_calculador.py:271`; `app/ponto/management/commands/diff_janela_he.py:94`; `app/ponto/management/commands/e6_piso_autopsia.py:72`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:138`; `app/chamados/management/commands/carimbar_por_batida_coincidente.py:73`)
- O evento novo agenda cálculo por `transaction.on_commit`; em autocommit o callback executa na chamada, e dentro de atomic aguarda o commit. A própria porta protege import/chamada e o agendador protege falhas de cálculo. (`app/ponto/registro_batida.py:177`; `app/ponto/services/fechamento.py:1807`; `app/ponto/services/fechamento.py:1818`; `app/ponto/services/fechamento.py:1823`)

**LogAuditoria ação/antes/depois**

- A persistência tem um escritor: `core.observ.evento`, com savepoint próprio; `registrar_log` e `evento_kw` apenas adaptam argumentos. O novo rejeitar reutiliza esse escritor, sem criar segunda persistência de log. (`app/core/observ.py:94`; `app/core/observ.py:124`; `app/core/observ.py:133`; `app/core/auditoria.py:22`; `app/core/observ.py:146`; `app/ponto/services/ausencia.py:1417`)
- `acao` é texto de até 64 caracteres, com choices não validados por create; antes/depois são JSON sem schema declarado. O ator implícito vem da requisição contextual ou do processo fora dela. (`app/core/models.py:296`; `app/core/models.py:300`; `app/core/observ.py:108`; `app/core/observ.py:116`)
- Leitores relacionados ao diff: desfazer exige que a última ação da batida seja `retratar_batida_admin`; tooltip/relatório de motivos leem `valor_depois`; flips leem antes/depois. (`app/ponto/services/retratar_humano.py:35`; `app/ponto/services/retratar_humano.py:51`; `app/ponto/services/retratar_humano.py:72`; `app/ponto/services/retratar_humano.py:93`; `app/ponto/services/retratar_humano.py:95`; `app/ponto/services/retratar_humano.py:228`; `app/ponto/services/retratar_humano.py:243`; `app/ponto/services/flip_batida.py:66`; `app/ponto/services/flip_batida.py:73`)
- Divergência histórica de formato já tratada: flip aceita payload string `E`/`S`, dict ou NULL. O novo rejeitar escreve dict de status; não achei leitor de payload de status de Ausencia além do teste novo. O curador apenas detecta qualquer trilha posterior. (`app/ponto/services/flip_batida.py:46`; `app/ponto/services/flip_batida.py:69`; `app/ponto/tests/test_lote7_fato_de_ausencia_acorda_projecao.py:166`; `app/ponto/management/commands/rederivar_minutos_h06_comp11.py:157`)
- Leitores genéricos e de outros domínios: UI de auditoria, histórico de chamados, regeneração de escala, retenção/reconciliação, juízes e vigia de loops. Eles usam ações/payloads próprios, não o novo dict de rejeição de férias. (`app/core/views.py:705`; `app/chamados/services/historico.py:86`; `app/escala/services/regeneracao.py:58`; `app/escala/services/regeneracao.py:61`; `app/chamados/models.py:404`; `app/chamados/reconciliador.py:480`; `app/chamados/juizes.py:1300`; `app/chamados/services/vigia_loop.py:32`; `app/chamados/services/vigia_loop.py:116`)

Esta é análise estática; execução dos testes, estado real de banco, logs e alteração efetiva de dinheiro/hash/TXT permanecem [nao verificado]. [nao verificado]

## 15/11 10:18 · sessao ebfe9f78f481 · - · - · origem=Ronald (tmux)

A função pura real é:

```python
def dono_da_divergencia(
    horas_do_dia,
    em_cadastro_x_realidade,
    folga_nao_definida=False,
    sem_vinculo_no_dia=False,
    sem_vinculo_em_dia_de_trabalho=False,
    batida_fora_de_turno=False,
    fase_deslocada=False,
):
```

Ela recebe insumos, **não recebe colaborador/data e não verifica se existe divergência**. Retorna sempre `CADASTRO`, `BATIDA` ou `ESTRUTURA`; por exemplo, horas vazias e demais insumos falsos retornam `ESTRUTURA`. `app/ponto/management/commands/e6_oraculo.py:53`, `:98`, `:110`.

A montagem por colaborador e competência é:

```python
InsumosDoDono(
    colaborador, ini, fim, hoje, espelho=None, *,
    agora, dias_de_fase=None, cadastro=None,
    sem_motor=False, cxr=None,
)
```

Seu método `.dono(data)` responde por aquele colaborador/data chamando o juiz puro com batidas, realizado, fase e cadastro reais. Também não verifica existência de divergência. `app/ponto/management/commands/e6_oraculo.py:128`, `:182`.

`sem_motor=True` com CXR sem cobertura pode devolver **`None = sem resposta`**, nunca `sem_divergencia`. A lavra distingue conjunto vazio conhecido de ausência de cobertura. `app/ponto/management/commands/e6_oraculo.py:142`, `:189`; `app/ponto/services/cxr_na_lista.py:165`, `:182`.

No comando E6, a existência de divergência é verificada **antes** de chamar `.dono(data)`: falta de linha, isenção/ausência e falta de lavratura ficam fora da comparação; zero/zero, diferença explicada pela janela declarada e diferença de até dez minutos não chegam ao dono. Só diferenças restantes recebem dono. `app/ponto/management/commands/e6_oraculo.py:334`, `:339`, `:342`, `:351`, `:354`, `:365`, `:378`.

Logo, não há resposta especial do juiz “quando não há divergência”: ele não faz essa pergunta. Inventar `None -> sem_divergencia` produziria classificação falsa. `app/ponto/management/commands/e6_oraculo.py:98`, `:110`, `:193`.

Para P3, consultar o dono real pode usar `InsumosDoDono(c, ini, fim, hoje, agora=agora).dono(data)`; quando precisa da lista do motor, a própria montagem chama `espelho_do_colab(..., agora=self.agora)`. É o mesmo caminho da medição E6 e das pautas. `app/ponto/management/commands/e6_oraculo.py:162`, `:195`; `app/ponto/management/commands/pautas_discordantes.py:82`, `:109`.

O comando de medição está explicitamente autorizado a chamar o motor; a proibição documentada recai sobre leitores de tela. `app/ponto/management/commands/pautas_discordantes.py:9`.

Não há função pronta observada que produza `sem_divergencia` nesse caminho. Caso o script precise imprimir essa categoria, deve aplicar os gates reais do comando E6 e deixar os dias sem julgamento identificados como erros, sem chamar ausência de julgamento de concordância. `app/ponto/management/commands/e6_oraculo.py:334`, `:346`, `:365`.

_5587 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
