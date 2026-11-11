# CODEX-ESTADO

> Ultima resposta processada: **11/11/2026 17:28** · publicado em **11/11/2026 17:28** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 11/11 17:28 · sessao c3afbe28a431 · - · - · origem=Ronald (tmux)

- **O atraso agora existe no mundo simulado:** `tx_lento` adia a conclusão em 15.000 ms; `open_bloqueado_libera` emite `onblocked` e só abre 15.000 ms depois. O escritor executado é o real; o wrapper apenas conta chamadas e devolve sua promessa. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:161`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:213`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:309`)

- **Abortar/rejeitar no teto e esconder o aviso são regressões diretamente cobertas:** Mg/Mh/Mi/Mi2 restauram rejeição/aborto; Mo/Mo2 retiram emissão/passagem do aviso; Mp impede sua remoção. O controle precisa estar verde no mesmo lote e a morte precisa casar a mensagem esperada. Execução verde nesta cópia: **[nao verificado]**. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:639`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:1003`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:1028`)

- **Mutante natural vivo: remover `req.onerror` de `abrirDB`.** O falso `open` nunca lança nem dispara `onerror`; os mundos de abertura cobrem pendência, bloqueio e liberação, mas não erro definitivo de abertura. Assim, a remoção desse handler não altera seus observáveis; com esse evento, o código mutado ficaria pendente. Ocorrência em produção: **[nao verificado]**. (`app/static/pwa.js:82`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:209`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:676`)

- **Mutante vivo de conexão fechada:** trocar o sucesso de abertura por `e.target.result.close(); fim(true,e.target.result)` passa pelo falso, porque `close()` é vazio e `transaction()` não verifica fechamento. Portanto, o instrumento não protege a cura do fechamento tardio da v5. Efeito desse mutante no navegador real: **[nao verificado]**. (`app/static/pwa.js:81`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:226`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:236`)

- **A prova temporal ainda é parcial:** os julgadores de sucesso lento não comparam término da promessa/sucesso visual com `fila_ms`; a propriedade compara estados finais e omite tempos. Entretanto, resolver incondicionalmente no `req.onsuccess` seria detectado pelos mundos de aborto/erro/pendência e pelo sucesso lento sem aviso/reclique protegido; não é correto dizer que esse mutante simples passaria. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:489`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:516`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:705`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:828`)

- **“Persistidas” não é uma leitura de durabilidade nativa:** o falso já põe a linha no mapa durante `add`; `persistidas()` conta esse mapa. Os cenários de aborto/erro removem a linha, e `marca('fila')` só acontece na conclusão, mas o mundo pendente conserva uma linha provisória no mapa e seu julgador não exige `persistidas == 0`. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:166`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:173`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:195`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:267`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:516`)

- **Quota é simulação específica:** `tx_erro` faz rollback e dispara somente `tx.onerror` com `QuotaExceededError`; não exercita erro real de pedido, erro de abertura ou comportamento posterior do navegador. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:154`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:166`)

- **A alegação anterior “ArmazemEPostTest toca banco” não está comprovada por esta leitura:** herda de `SimpleTestCase`, renderiza com `SimpleNamespace`/`AnonymousUser` e usa IDB em memória; efeitos de importação/renderização e execução completa permanecem **[nao verificado]**. O ensaio certamente escreve HTML/JS/perfil temporários e depois apaga, por isso não foi executado sob a regra somente leitura. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:385`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:398`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:414`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:426`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:579`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:781`)

## 11/11 17:27 · sessao 2f63bfab402a · - · - · origem=Ronald (tmux)

- **Abertura lenta ou conexão depois de 2 s:** `abrirDB` apenas chama aviso; continua aguardando e aceita `onsuccess` tardio. Não fecha a conexão. (`app/static/pwa.js:71`, `app/static/pwa.js:81`)
- **`onblocked`:** avisa imediatamente, continua aguardando; não rejeita. (`app/static/pwa.js:83`)
- **Transação >2 s:** avisa e espera; `oncomplete` resolve, `onabort`/`onerror` rejeitam. Nenhum timer dessa transação chama `abort`. (`app/static/pwa.js:95`, `app/static/pwa.js:99`, `app/static/pwa.js:100`, `app/static/pwa.js:101`)
- **Quota:** se `add` lança, cai no catch; se chega como erro da transação, cai em `tx.onerror`. Ambos rejeitam, removem aviso e entregam erro ao template. Quota real nesta cópia: **[nao verificado]**. (`app/static/pwa.js:98`, `app/static/pwa.js:100`, `app/static/pwa.js:102`, `app/static/pwa.js:283`)
- **Sem evento definitivo:** promessa permanece pendente indefinidamente; aviso fica e a guarda do clique impede outro escritor. Isso está explicitamente assumido no contrato novo; não é sucesso nem rejeição. (`app/static/pwa.js:262`, `app/templates/ponto/triagem/apto.html:269`, `app/templates/ponto/bater_ponto_v2.html:294`)
- **Quem escreve:** aviso vem de `pwa.js`; sucesso/erro vêm dos templates após o `await`. No erro, apto destrava a guarda e v2 retira `disabled`; no sucesso escondem controles. Portanto o botão **não volta clicável em todos os desfechos**. (`app/static/pwa.js:250`, `app/templates/ponto/triagem/apto.html:294`, `app/templates/ponto/triagem/apto.html:308`, `app/templates/ponto/bater_ponto_v2.html:339`, `app/templates/ponto/bater_ponto_v2.html:342`)

**Ressalva importante:** o comentário equipara `error` a rollback definitivo, mas o código rejeita no `tx.onerror`, sem esperar `tx.onabort`. Portanto rollback confirmado não decorre dessa implementação. Um evento `error` seguido de `complete` deixaria a promessa rejeitada por causa da guarda `feito`; ocorrência disso no IndexedDB real com os chamadores atuais: **[nao verificado]**. Não encontrei evidência suficiente para transformar essa possibilidade em bloqueante de produto. (`app/static/pwa.js:51`, `app/static/pwa.js:94`, `app/static/pwa.js:100`, `app/static/pwa.js:101`)

**Tetos restantes:** config rejeita após 2 s sem abortar a operação; gap completo devolve fallback após 4 s; `/hora/` aborta fetch após 2 s; POST aborta fetch após 30 s e provoca fallback para fila. Nenhum desses timers aborta a escrita IndexedDB da batida. (`app/static/pwa.js:116`, `app/static/pwa.js:125`, `app/static/pwa.js:142`, `app/static/pwa.js:152`, `app/static/pwa.js:271`, `app/static/pwa.js:299`)

**Falso erro residual no caminho online:** abortar a espera do POST não confirma ausência de gravação no servidor. Se o servidor já gravou e a tentativa subsequente de fila falha, v2 mostra erro apesar da eventual gravação no servidor; ocorrência real **[nao verificado]**. Esse caminho conserva a pendência de identidade do clique. (`app/static/pwa.js:299`, `app/templates/ponto/bater_ponto_v2.html:323`, `app/templates/ponto/bater_ponto_v2.html:330`)

**Conclusão da subauditoria:** o bloqueante específico da v5 — timer que abortava escrita lenta — está removido no código. Não encontrei nova regressão demonstrada nesse delta; espera infinita com aviso permanece deliberadamente presente. (`app/static/pwa.js:95`, `app/static/pwa.js:99`, `app/static/pwa.js:262`)

## 11/11 17:27 · sessao fe2a4a6bd194 · - · - · origem=Ronald (tmux)

Chamadores e contratos:

- `apto.html` aguarda `registrarBatidaOffline` e ignora seu retorno; só depois escreve “salva no celular”; a rejeição mostra erro e libera `_confirmandoOffline`. Não exige booleano nem prazo de rejeição. (`app/templates/ponto/triagem/apto.html:289`, `app/templates/ponto/triagem/apto.html:294`, `app/templates/ponto/triagem/apto.html:308`)
- V2 chama o escritor na recuperação do POST e no ramo offline; também ignora retorno e escreve sucesso após o `await`; os catches liberam botão e `_enviando`. Sucesso esconde o botão; promessa pendente mantém a guarda. (`app/templates/ponto/bater_ponto_v2.html:294`, `app/templates/ponto/bater_ponto_v2.html:323`, `app/templates/ponto/bater_ponto_v2.html:326`, `app/templates/ponto/bater_ponto_v2.html:329`, `app/templates/ponto/bater_ponto_v2.html:336`, `app/templates/ponto/bater_ponto_v2.html:339`, `app/templates/ponto/bater_ponto_v2.html:342`)
- As duas cascas carregam `pwa.js`. Os acessos de produção encontrados ao escritor estão nessas duas telas. (`app/templates/base.html:20`, `app/templates/base_app.html:20`, `app/templates/ponto/triagem/apto.html:289`, `app/templates/ponto/bater_ponto_v2.html:323`, `app/templates/ponto/bater_ponto_v2.html:336`)
- SW não abre IndexedDB: o evento `sync` manda `SYNC_BATIDAS` às páginas; o listener de `pwa.js` chama `sincronizarBatidas`. O SW não espera confirmação da sincronização. (`app/static/sw.js:112`, `app/static/sw.js:116`, `app/static/sw.js:119`, `app/static/pwa.js:16`)
- Sync e indicador usam `abrirDB()` sem callback de aviso. Antes recebiam a rejeição pelo teto; agora podem aguardar indefinidamente e silenciosamente. Isso não segura o retorno do escritor, que dispara o indicador sem `await`. (`app/static/pwa.js:71`, `app/static/pwa.js:286`, `app/static/pwa.js:310`, `app/static/pwa.js:368`)
- Sync/indicador são disparados na carga e em eventos online/offline; sync também dispara o indicador ao terminar. As chamadas não consomem booleano ou mensagem. (`app/static/pwa.js:363`, `app/static/pwa.js:504`, `app/static/pwa.js:505`, `app/static/pwa.js:511`, `app/static/pwa.js:512`, `app/static/pwa.js:513`)

Escritores e leitores:

| Valor | Quem escreve | Quem lê |
|---|---|---|
| Fila `batidas_pendentes`; `id` | `abrirDB` cria store com `id` automático; escritor faz `add`. (`app/static/pwa.js:76`, `app/static/pwa.js:77`, `app/static/pwa.js:276`) | Sync faz `getAll`, exclui por `id`; indicador conta. (`app/static/pwa.js:313`, `app/static/pwa.js:343`, `app/static/pwa.js:353`, `app/static/pwa.js:371`) |
| `config.chave/valor` | `salvarConfig` faz `put`. (`app/static/pwa.js:114`) | `lerConfig` faz `get` e devolve `valor`. (`app/static/pwa.js:123`, `app/static/pwa.js:127`) |
| `ntp_gap` | `calcularESalvarGap`, diferença entre hora do servidor e aparelho com meia latência. (`app/static/pwa.js:155`, `app/static/pwa.js:156`, `app/static/pwa.js:157`, `app/static/pwa.js:158`) | Recuperação de erro e `obterGap`. (`app/static/pwa.js:162`, `app/static/pwa.js:170`) |
| `ntp_calculado_em` | `Date.now()` depois de salvar gap. (`app/static/pwa.js:159`) | `obterGap` aplica validade de 30 minutos. (`app/static/pwa.js:167`, `app/static/pwa.js:168`) |
| Instante, `timestamp`, `timestamp_dispositivo`, `timestamp_gap_ms` | Escritor lê `Date.now()` uma vez; calcula timestamp corrigido e guarda ambos os instantes e gap. (`app/static/pwa.js:270`, `app/static/pwa.js:272`, `app/static/pwa.js:277`, `app/static/pwa.js:278`, `app/static/pwa.js:279`) | Sync ordena pelo corrigido, mas envia somente o cru do aparelho. (`app/static/pwa.js:318`, `app/static/pwa.js:328`) |
| `tipo`, latitude, longitude, foto | Parâmetros das telas → `add`. (`app/templates/ponto/triagem/apto.html:289`, `app/templates/ponto/bater_ponto_v2.html:336`, `app/static/pwa.js:277`, `app/static/pwa.js:279`, `app/static/pwa.js:281`) | Sync coloca no `FormData`. (`app/static/pwa.js:325`, `app/static/pwa.js:326`, `app/static/pwa.js:327`, `app/static/pwa.js:333`) |
| `gps_accuracy`, `gps_captured_at` | Escritor guarda parâmetros. (`app/static/pwa.js:280`) | Não seguem no FormData desse sync; nenhum leitor de produção encontrado nesse caminho. (`app/static/pwa.js:324`, `app/static/pwa.js:333`) |

A rota do sync é `/api/ponto/bater/`, ligada a `views.api_bater_ponto`; a API lê o timestamp cru, aplica `hora_do_aparelho`, e entrega o efetivo e o cru ao chokepoint. O gap e o timestamp corrigido locais não decidem a hora no servidor nesse caminho. (`app/static/pwa.js:334`, `app/api/urls.py:67`, `app/api/views.py:519`, `app/api/views.py:520`, `app/api/views.py:666`, `app/api/views.py:678`)

`hora_do_aparelho` preserva a hora quando o atraso está entre zero e 48 horas; futuro/antigo demais passam pelo ramo de substituição por `agora`. (`app/api/hora_do_aparelho.py:62`, `app/api/hora_do_aparelho.py:141`, `app/api/hora_do_aparelho.py:142`, `app/api/hora_do_aparelho.py:146`, `app/api/hora_do_aparelho.py:148`)

`JulianiPWA.agora` ainda aguarda `obterGap` diretamente; a exportação existe, mas nenhum chamador de produção foi encontrado pela busca na cópia. (`app/static/pwa.js:487`, `app/static/pwa.js:488`, `app/static/pwa.js:501`)

Lei:

- A lei declara batida de chão sem bloqueio em runtime e escrita única em `registro_batida.py`. O delta não altera esse arquivo nem os POSTs das telas. (`CLAUDE.md:143`, `CLAUDE.md:144`; delta fornecido pelo usuário)
- A admissibilidade da câmera continua centralizada em `abrirCameraSelfie`/`capturarSelfie`; falta de foto devolve aviso e valor nulo. As telas passam foto nula ao escritor. Não há segundo juiz da falta de selfie introduzido pelo delta. (`app/static/pwa.js:198`, `app/static/pwa.js:211`, `app/static/pwa.js:222`, `app/static/pwa.js:232`, `app/templates/ponto/triagem/apto.html:275`, `app/templates/ponto/triagem/apto.html:277`, `app/templates/ponto/bater_ponto_v2.html:274`, `app/templates/ponto/bater_ponto_v2.html:275`)
- No servidor, `selfie_ausente` é o juiz da cobrança da ausência; a API acrescenta motivo de anomalia, sem retorno de recusa nesse ramo. (`app/ponto/registro_batida.py:80`, `app/ponto/registro_batida.py:91`, `app/api/views.py:459`, `app/api/views.py:461`, `app/api/views.py:464`)

Pendências v5 intactas:

- `DataTransfer`/`File`/atribuição de arquivos continuam fora do `try` que protege a captura na triagem online; uma exceção impede chegar ao submit, com o botão já desabilitado. (`app/templates/ponto/triagem/apto.html:212`, `app/templates/ponto/triagem/apto.html:230`, `app/templates/ponto/triagem/apto.html:234`, `app/templates/ponto/triagem/apto.html:236`, `app/templates/ponto/triagem/apto.html:241`)
- HTTP 400 do sync ainda exclui a fila com apenas `console.warn`; a pendência está registrada como aberta. (`app/static/pwa.js:347`, `app/static/pwa.js:350`, `app/static/pwa.js:353`, `app/docs/BACKLOG.md:1944`)
- V2 sem `postarBatidaNoTeto` continua usando `fetch`/`json` sem teto. (`app/templates/ponto/bater_ponto_v2.html:312`, `app/templates/ponto/bater_ponto_v2.html:313`, `app/templates/ponto/bater_ponto_v2.html:314`)
- Identidade do clique permanece declarada como obra separada, sem identificador persistido no registro da fila. (`app/static/pwa.js:265`, `app/static/pwa.js:266`, `app/static/pwa.js:276`, `app/static/pwa.js:282`)
- TICKETS ainda lista o template apagado como “MATAR”; scanner de URLs não inclui apto/v2; guard de lei não inclui v2 e ignora caminhos inexistentes. (`app/docs/TICKETS.md:693`, `app/tools/check_frontend_urls.py:25`, `app/scripts/check_a_lei_batida.py:14`, `app/scripts/check_a_lei_batida.py:21`, `app/scripts/check_a_lei_batida.py:31`)

## 11/11 17:05 · sessao 19ef5920d01b · PROPORCAO-POR-RISCO · verificador · origem=Code

REPROVADO

1. **Falta fechar a lacuna de padrões válidos mais gerais.** O harness continua usando somente `app/docs/*.md` como padrão válido com curinga. Não há caso para, por exemplo, `app/x/pre*pos.py`; uma implementação restrita ao padrão de docs sobreviveria — inferência estática. O selo implementa casamento geral por `fnmatchcase`. (`bin/tests/test_selo_proporcao.sh:27`, `bin/tests/test_selo_proporcao.sh:87`, `bin/selo_proporcao.py:102`, `bin/selo_proporcao.py:103`)

As demais lacunas têm os seguintes resultados, por análise estática:

| Lacuna da v10 | Caso atual e mordida |
|---|---|
| EXCECAO lida indevidamente do HEAD | **Coberta.** `excecao_tirada_no_head` remove PWA da exceção no HEAD. Pela BASE, PWA é A e retorna 1; lendo a exceção do HEAD, PWA cai na LISTA e tudo fica B, retornando 0. O CORTES alterado também é B pelo padrão de docs. (`bin/tests/test_selo_proporcao.sh:27`, `bin/tests/test_selo_proporcao.sh:101`, `bin/selo_proporcao.py:113`, `bin/selo_proporcao.py:115`, `bin/selo_proporcao.py:171`) |
| EXCECAO lida indevidamente do disco | **Coberta.** `excecao_do_disco` remove SW somente do arquivo não commitado. Pela BASE retorna 1; lendo a exceção do disco, SW cai em `static` e retorna 0. (`bin/tests/test_selo_proporcao.sh:175`, `bin/tests/test_selo_proporcao.sh:177`, `bin/tests/test_selo_proporcao.sh:179`, `bin/selo_proporcao.py:123`) |
| `?` na pasta | **Coberta.** `interrog_na_pasta` fornece blocos válidos, exceto pelo `?`; exige rc 2 e motivo `curinga so`. Desligando essa rejeição, o diff de template seria B/rc 0. (`bin/tests/test_selo_proporcao.sh:55`, `bin/tests/test_selo_proporcao.sh:58`, `bin/tests/test_selo_proporcao.sh:59`, `bin/tests/test_selo_proporcao.sh:147`, `bin/selo_proporcao.py:90`) |
| Rename chamado PWA, mas feito sobre SW | **Corrigida.** Há agora renames separados para SW e PWA. Sem a exceção, origem e destino seriam B; com ela, a origem mantém A/rc 1. (`bin/tests/test_selo_proporcao.sh:98`, `bin/tests/test_selo_proporcao.sh:99`, `bin/selo_proporcao.py:113`, `bin/selo_proporcao.py:151`) |
| Declaração A conferida somente por rc | **Coberta.** Os greps adicionais exigem classe real A no caso excepcional e classe real B no template; trocar apenas a classe impressa passa a falhar. (`bin/tests/test_selo_proporcao.sh:103`, `bin/tests/test_selo_proporcao.sh:104`, `bin/tests/test_selo_proporcao.sh:127`, `bin/tests/test_selo_proporcao.sh:128`, `bin/selo_proporcao.py:168`, `bin/selo_proporcao.py:169`) |

**Sobre passar por outro motivo:** os greps `CLASSE A (declarada A)` e `CLASSE B (declarada A)` distinguem os dois resumos; o texto comum sobre arquivos “fora” não satisfaz ambos. (`bin/tests/test_selo_proporcao.sh:104`, `bin/tests/test_selo_proporcao.sh:128`, `bin/selo_proporcao.py:169`, `bin/selo_proporcao.py:170`)

Há uma imprecisão remanescente: **`pwa_declarada_A` altera `sw.js`**, portanto comprova a classe real do SW, apesar do nome PWA. Isso exercita a mesma guarda de exceção, mas não demonstra especificamente PWA declarado A. (`bin/tests/test_selo_proporcao.sh:103`, `bin/selo_proporcao.py:113`)

Não identifiquei falha antecipada de setup nos demais casos citados: os caminhos existem na base sintética; o preparo exige sucesso dos comandos e commit diferente da BASE, registrando falha antes de retornar se isso não ocorrer. O grep do HEAD confirma a remoção; o do disco acusa se SW permaneceu. (`bin/tests/test_selo_proporcao.sh:28`, `bin/tests/test_selo_proporcao.sh:38`, `bin/tests/test_selo_proporcao.sh:47`, `bin/tests/test_selo_proporcao.sh:101`, `bin/tests/test_selo_proporcao.sh:178`)

**Isolamento:** as escritas explícitas ficam em `$T`, incluindo repositórios, backups e stubs. O `main` usado pelos casos é sintético; o CORTES vivo não alimenta esses casos. Entretanto, existe dependência **opcional** do HEAD/HEAD~1 da árvore do script: um caso consulta essas revisões e é omitido quando indisponíveis. Os executáveis testados também vêm dessa árvore. (`bin/tests/test_selo_proporcao.sh:11`, `bin/tests/test_selo_proporcao.sh:14`, `bin/tests/test_selo_proporcao.sh:16`, `bin/tests/test_selo_proporcao.sh:35`, `bin/tests/test_selo_proporcao.sh:202`, `bin/tests/test_selo_proporcao.sh:204`, `bin/tests/test_selo_proporcao.sh:239`)

| Blocos | QUEM ESCREVE → QUEM LÊ |
|---|---|
| Reais | Estão materializados em `app/docs/CORTES.md:444` e `app/docs/CORTES.md:445`; autoria histórica da inserção: **[nao verificado]**. O gerador reescreve o arquivo inteiro a partir do JSON, sem preservar o Markdown anterior. (`bin/gerar_cortes.py:23`, `bin/gerar_cortes.py:32`, `bin/gerar_cortes.py:107`, `bin/gerar_cortes.py:116`) |
| Sintéticos | `lista()` escreve os dois blocos; `base()` fornece a LISTA e usa a EXCECAO padrão. Os casos adulteram essas cópias temporárias. (`bin/tests/test_selo_proporcao.sh:18`, `bin/tests/test_selo_proporcao.sh:21`, `bin/tests/test_selo_proporcao.sh:27`, `bin/tests/test_selo_proporcao.sh:101`, `bin/tests/test_selo_proporcao.sh:177`) |
| Leitura do selo | `regras()` obtém **ambos da BASE** por `git show`; `bloco()` extrai os conjuntos; `main()` entrega-os a `motivo_b()`, que consulta EXCECAO antes da LISTA. (`bin/selo_proporcao.py:81`, `bin/selo_proporcao.py:86`, `bin/selo_proporcao.py:87`, `bin/selo_proporcao.py:113`, `bin/selo_proporcao.py:115`, `bin/selo_proporcao.py:150`, `bin/selo_proporcao.py:164`) |

Execução integral do harness e resultados de mutação nesta revisão: **[nao verificado]**.

## 11/11 17:05 · sessao 184a8eb0f9a1 · FIO-E0-V4-E4B · verificador · origem=Code

**REPROVADO.** A inversão **DP↔âncora da c8 foi curada pela classe dos adquirentes atuais da advisory E4B**. Porém, encontrei outro contraexemplo estrutural de espera circular, **Colaborador↔Célula**, anterior ao agrupador; e permanece o bloqueante de encerramento pela suíte inteira pendente. (`app/chamados/services/fecho_runtime.py:124`; `app/chamados/services/fecho_runtime.py:126`; `app/chamados/models.py:290`; `app/ponto/portas/celula.py:276`; `CLAUDE.md:375`)

Os GREENs e as mortes de mutantes fornecidos são evidência dos recortes executados; não os descartei. Não achei comprovante da suíte inteira. (`c9_orm.log:2`, anexo; `c9_orm.log:6`, anexo; `c9_orm.log:9`, anexo; `app/docs/FIO-E0-V4-E4B-c9.txt:103`)

**1. Achados da c8 e ordem das travas**

| Achado anterior | Classificação na c9 |
|---|---|
| **B1 — DP↔âncoras, incluindo emissão durante a transição** | **Curado pela CLASSE dos adquirentes atuais dessas âncoras.** O único laço produtivo encontrado que chama `_travar_ancora` agora toma DP primeiro. A emissão anterior da fábrica e o agrupador usam a mesma linha departamental, cuja unicidade está no modelo. Isso não certifica a ordem global das demais tabelas. (`app/chamados/services/fecho_runtime.py:124`; `app/chamados/services/fecho_runtime.py:126`; `app/pautas/services.py:350`; `app/pautas/services.py:363`; `app/pautas/models.py:167`) |
| Dia civil de conversa virando competência | **Curado pela CLASSE desse leitor**, preservado: juiz do dia, descarte de `None`, janela da empresa. (`app/chamados/services/fecho_runtime.py:114`; `app/chamados/services/fecho_runtime.py:118`; `app/chamados/juizes.py:336`) |
| Duplicação concorrente marcador+âncora | **Curado pela CLASSE dos participantes atuais do agrupador.** DP/advisory antecedem existência e criação. Não achei unicidade marcador+âncora na porta genérica/modelo. (`app/chamados/services/fecho_runtime.py:124`; `app/chamados/services/fecho_runtime.py:128`; `app/pautas/models.py:123`) |
| Motivo limbo truncado | **Curado pela CLASSE dos formatos produzidos pelo S143**, preservado: reserva motivos e abrevia IDs com contagens. (`app/chamados/services/fecho_runtime.py:64`; `app/chamados/services/fecho_runtime.py:76`; `app/chamados/tests/test_e4b_c6_orm.py:228`) |
| Coleta sobrevivendo ao rollback S143 | **Curado pela CLASSE das exceções propagadas**, preservado: merge depois da saída normal do savepoint; restauração no `finally`. (`app/chamados/services/fecho_runtime.py:36`; `app/chamados/services/fecho_runtime.py:40`; `app/chamados/signals.py:115`) |
| Reentrega não acrescenta IDs | **Não curado — FILA.** Pauta viva provoca `continue`; permanece a nota individual. (`app/chamados/services/fecho_runtime.py:128`; `app/chamados/services/fecho_runtime.py:130`; `app/chamados/services/disputa_emissao.py:1855`) |
| Fechamentos fora das duas bordas | **Não curado — FILA de alcance.** `fechar()` continua usando transição direta. (`app/chamados/models.py:454`; `app/chamados/services/acoes_chamado.py:316`; `app/chamados/services/fio.py:722`) |
| Relógio fora da borda | **Não curado — FILA preexistente.** S143 passa `hoje=None`; o juiz pode consultar `date.today()`. (`app/chamados/services/disputa_emissao.py:1945`; `app/chamados/juizes.py:577`) |
| Prova concorrente e falha real de `escrever` | **Não curado — FILA de prova.** Melhorou o reconhecimento da trava DP, mas o ensaio continua direto no agrupador. Não achei teste de falha real da escrita da Pauta; a falha injetada continua no resolver. (`app/chamados/tests/test_e4b_c6_orm.py:291`; `app/chamados/tests/test_e4b_c6_orm.py:298`; `app/chamados/tests/test_e4b_c6_orm.py:305`) |
| Leitores crus de `data_evento` | **Não curado — FILA preexistente.** (`app/chamados/services/disputa_emissao.py:1836`; `app/chamados/services/disputa_emissao.py:1984`; `app/chamados/services/disputa_emissao.py:2056`; `app/chamados/models.py:364`) |
| Lint | **Curado**, com GREEN fornecido nesta volta. (`app/chamados/tests/test_e4b_c6_memoria.py:7`; `app/chamados/tests/test_e4b_c6_orm.py:274`; `c9_orm.log:2`, anexo) |
| Suíte inteira | **Não curado — BLOQUEANTE de encerramento.** Não achei comprovante integral. (`CLAUDE.md:375`; `app/docs/FIO-E0-V4-E4B-c9.txt:103`) |

Nas ordens abaixo, **C/D/Ch/Q** significam Colaborador/Disputa/Chamado/Pergunta; **Cel**, Célula; **FM**, FechamentoMensal; **A**, advisory da âncora E4B. A porta comum implementa C→D→Ch→Q, cada conjunto por PK crescente. (`app/chamados/models.py:2666`; `app/chamados/models.py:2675`; `app/chamados/models.py:2688`; `app/chamados/models.py:2692`)

A ordem interna comum de `escrever` é **INSERT Pauta→trilha→obter/criar contador→UPDATE contador**, dentro do `atomic` da porta. Nos caminhos sem transação externa, a retenção termina no retorno dessa porta. (`app/pautas/services.py:152`; `app/pautas/services.py:180`; `app/pautas/services.py:186`; `app/pautas/services.py:188`; `app/pautas/services.py:363`; `app/pautas/services.py:365`)

| Caminho | Ordem encontrada |
|---|---|
| Duas bordas humanas | Ambas entram em `transicionar_com_pauta`: atomic externo→transição completa→agrupador. (`app/chamados/services/acoes_chamado.py:316`; `app/chamados/services/fio.py:722`; `app/chamados/services/fecho_runtime.py:152`; `app/chamados/services/fecho_runtime.py:156`; `app/chamados/services/fecho_runtime.py:159`) |
| Transição anterior ao agrupador | C→D→Ch→UPDATE Ch→receivers S143/cartório→escritas condicionais de Q/Cel/Pauta→relavra posterior de Q. As cascatas podem reentrar nas portas já travadas. (`app/chamados/models.py:290`; `app/chamados/models.py:296`; `app/chamados/models.py:309`; `app/chamados/services/disputa_emissao.py:1878`; `app/chamados/signals.py:217`) |
| Agrupador E4B | Atomic→**DP, se lote válido não vazio→todas A ordenadas→exists→INSERT Pauta→UPDATE DP**. (`app/chamados/services/fecho_runtime.py:122`; `app/chamados/services/fecho_runtime.py:124`; `app/chamados/services/fecho_runtime.py:125`; `app/chamados/services/fecho_runtime.py:128`; `app/chamados/services/fecho_runtime.py:132`) |
| Cadastro sem turno, inclusive durante a transição | C herdado ou adquirido→Pauta→DP; depois, no runtime, DP reentrante→A. (`app/chamados/services/fabrica_da_celula.py:204`; `app/chamados/services/fabrica_da_celula.py:365`; `app/chamados/services/fabrica_da_celula.py:369`; `app/chamados/services/fecho_runtime.py:124`) |
| Limbo exportado, inclusive durante a transição | Travas herdadas do julgamento→Pauta→DP; esse emissor não toma A. (`app/ponto/services/cartorio.py:1020`; `app/chamados/services/fabrica_da_celula.py:215`; `app/chamados/services/fabrica_da_celula.py:279`) |
| Pago/devido exportada | C→**FM `select_for_update`**→Pauta cabeça→DP→filhas→DP. (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:193`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:196`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:207`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:211`) |
| Passivo validado | Atomic do par→plantio/materialização→regeneração→escrita FM→Pauta cabeça/filhas ou sem movimento→DP. O plantio também alcança TM e C/D/Ch/Q; a regeneração pode emitir DP antes do recálculo FM. (`app/chamados/services/passivo_validado.py:371`; `app/chamados/services/passivo_validado.py:428`; `app/chamados/services/passivo_validado.py:438`; `app/chamados/services/passivo_validado.py:443`; `app/chamados/services/passivo_validado.py:450`; `app/ponto/nucleo.py:81`; `app/chamados/services/materializacao.py:1277`; `app/ponto/portas/regen_exportada.py:179`) |
| Emissões do passivo | Cabeça, filhas e saída sem movimento usam a mesma porta. (`app/chamados/services/passivo_validado.py:229`; `app/chamados/services/passivo_validado.py:233`; `app/chamados/services/passivo_validado.py:266`) |
| HE por período | Atomic externo→DecisaoHE `select_for_update`→decisão/reentrada→Pauta→DP. (`app/ponto/services/autorizacao_he_periodo.py:321`; `app/ponto/services/autorizacao_he_periodo.py:343`; `app/ponto/portas/he.py:555`; `app/ponto/portas/he.py:174`; `app/ponto/services/autorizacao_he_periodo.py:300`; `app/ponto/services/autorizacao_he_periodo.py:244`) |
| Férias novas/históricas | Agendamento→sincronização Ausência→eventual Pauta→DP→UPDATE PeriodoAquisitivo. (`app/ferias/services.py:298`; `app/ferias/services.py:302`; `app/ferias/services.py:321`; `app/ferias/services.py:326`; `app/ponto/services/ausencia.py:1216`; `app/ponto/services/ausencia.py:1245`) |
| Alteração de férias | Agendamento `select_for_update`→PeriodoAquisitivo `select_for_update`→alterações/sincronização de Ausência→eventual Pauta→DP. (`app/ferias/services.py:364`; `app/ferias/services.py:367`; `app/ferias/services.py:434`; `app/ferias/services.py:450`; `app/ponto/services/ausencia.py:1245`) |
| Cobertura passada | Atomic→eventual OrdemSubstituicao→com substituto, UPDATE de Pautas e contador supervisão→reconciliação dos Ch; sem substituto, reconciliação→nova Pauta supervisão→contador. Pode herdar DP das cascatas de fechamento. (`app/ponto/services/cobertura.py:283`; `app/ponto/services/cobertura.py:305`; `app/ponto/services/cobertura.py:317`; `app/ponto/services/cobertura.py:333`; `app/chamados/services/fabrica_da_celula.py:279`) |
| Ler/fazer/cancelar Pauta | UPDATE Pauta existente→trilha→contador do destinatário; nenhuma A nessa porta. (`app/pautas/services.py:215`; `app/pautas/services.py:218`; `app/pautas/services.py:247`; `app/pautas/services.py:251`; `app/pautas/services.py:272`; `app/pautas/services.py:276`; `app/pautas/services.py:294`; `app/pautas/services.py:297`) |
| Lavra avulsa | Obter/criar contador→UPDATE contador; não achei aquisição de A, Ch, Q ou FM nesse comando. (`app/pautas/management/commands/lavrar_pautas.py:51`; `app/pautas/services.py:363`; `app/pautas/services.py:365`) |

O restante do censo produtivo chega à ordem comum da porta nos pontos abaixo. Não achei advisory E4B própria nesses emissores; **ordem integral de transações eventualmente herdadas por chamadas dinâmicas: [nao verificado]**. (`app/chamados/services/fecho_runtime.py:83`; `app/chamados/services/fecho_runtime.py:126`)

| Chamadores | Pontos de chegada |
|---|---|
| Interface, resposta e suporte | `app/pautas/views.py:76`; `app/pautas/views.py:112`; `app/pautas/services.py:194`; `app/pautas/management/commands/suporte_hasner.py:133`. |
| Semanal | Escrita em `app/pautas/semanal.py:276`; fechamento de antigas em `app/pautas/semanal.py:229`. Os ramos são separados pelo retorno de `app/pautas/semanal.py:237`. |
| Pré-fechamento, férias e aparelhos | `app/folha/management/commands/pre_fechamento.py:110`; `app/ferias/management/commands/pautas_de_ferias.py:167`; `app/colaboradores/management/commands/pauta_aparelhos_compartilhados.py:107`. |
| Ausência vencida | Mudança de estado anterior à emissão; a Pauta chega pela porta. (`app/ponto/services/ausencia.py:1002`; `app/ponto/services/ausencia.py:1005`; `app/ponto/services/ausencia.py:1055`) |
| Discordantes, deslogados, esmeril | Novas Pautas e fechamento das antigas: `app/ponto/services/pautas_dos_discordantes.py:177`; `app/ponto/services/pautas_dos_discordantes.py:183`; `app/ponto/services/pautas_dos_deslogados.py:77`; `app/ponto/services/pautas_dos_deslogados.py:80`; `app/ponto/services/pautas_do_esmeril.py:76`; `app/ponto/services/pautas_do_esmeril.py:79`. Não achei atomic externo envolvendo os dois laços nesses emissores. |
| Resposta tardia | Nota anterior→Pauta→DP; reabertura bem-sucedida retorna antes desse ramo. (`app/chamados/services/resposta_tardia.py:48`; `app/chamados/services/resposta_tardia.py:53`; `app/chamados/services/resposta_tardia.py:63`) |
| Competência trancada | Atomics individuais de Ch/Disputa→emissão posterior da Pauta→DP. (`app/chamados/services/competencia_trancada.py:182`; `app/chamados/services/competencia_trancada.py:197`; `app/chamados/services/competencia_trancada.py:208`; `app/chamados/services/competencia_trancada.py:231`) |
| Reabrir órfãs enterradas | Atomics individuais C/D/Ch/Q→emissão posterior das Pautas. (`app/chamados/management/commands/reabrir_orfas_enterradas.py:153`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:178`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:208`) |
| Comandos de competência | Pauta cabeça/filhas→DP: `app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:134`; `app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:139`; `app/pautas/management/commands/pauta_dia_em_aberto.py:127`; `app/pautas/management/commands/pauta_dia_em_aberto.py:139`; `app/pautas/management/commands/pauta_dia_em_aberto.py:142`. |
| Outros destinatários | Pauta→contador correspondente: `app/ponto/services/afastado_avisa.py:74`; `app/core/services/crons_quebrados.py:71`; `app/core/management/commands/alarme_esteira.py:54`; `app/core/management/commands/alarme_arvore.py:50`. |

**B2 — BLOQUEANTE de ordem global: resta um ciclo C↔Cel permitido pelo código.** A entrada externa encontrada é o wizard de edição de template, que propaga a regeneração sem adquirir C antes de atualizar Cel. (`app/escala/views_wizard.py:108`; `app/escala/services/cadastro_tipo.py:306`; `app/escala/services/cadastro_tipo.py:320`; `app/escala/services/cadastro_tipo.py:348`; `app/ponto/portas/celula.py:276`)

| Passo | T1 — fechamento humano | T2 — edição de template |
|---|---|---|
| 1 | Adquire C/D/Ch; pausa antes do save de Ch, mantendo o atomic. (`app/chamados/models.py:280`; `app/chamados/models.py:290`; `app/chamados/models.py:296`) | |
| 2 | | Atualiza a mesma Cel, mantendo sua trava no atomic da regeneração. (`app/ponto/portas/celula.py:218`; `app/ponto/portas/celula.py:276`) |
| 3 | | Encontra a cobrança ainda viva; a marca de órfã chama `renovar_contexto`, que pede C e espera T1, mantendo Cel. (`app/ponto/management/commands/gerar_celulas.py:243`; `app/ponto/management/commands/gerar_celulas.py:255`; `app/chamados/services/contexto.py:103`; `app/chamados/models.py:486`) |
| 4 | Salva Ch; o receiver chama o cartório, que tenta atualizar a mesma Cel e espera T2, mantendo C. (`app/chamados/models.py:296`; `app/chamados/signals.py:217`; `app/ponto/services/cartorio.py:1845`; `app/ponto/portas/celula.py:1072`; `app/ponto/portas/celula.py:905`) | Continua esperando C. (`app/chamados/models.py:2666`) |

O estado testemunha exige vínculo ativo, célula gerada em competência aberta com DNA alterado, cobrança viva de furo com marcos e criação anterior à regeneração, além de dia passado presente na grade e impressão a relavrar. Esses guards são compatíveis no código; não achei trava comum anterior que impeça a intercalação. **Reprodução executada e ocorrência em produção: [nao verificado].** (`app/escala/services/cadastro_tipo.py:334`; `app/ponto/portas/celula.py:250`; `app/ponto/portas/celula.py:263`; `app/ponto/portas/celula.py:274`; `app/escala/services/regeneracao.py:148`; `app/escala/services/regeneracao.py:154`; `app/ponto/services/cartorio.py:1381`; `app/ponto/services/cartorio.py:1388`; `app/ponto/services/cartorio.py:1997`)

A pré-trava DP da c9 vem **depois** dessa transição e não alcança esse ciclo. É um achado herdado no caminho completo, não uma regressão atribuída ao novo SELECT DP. (`app/chamados/services/fecho_runtime.py:124`; `app/chamados/services/fecho_runtime.py:156`; `app/chamados/services/fecho_runtime.py:159`)

As outras comparações da matriz continuam **FILA de investigação**: DP↔PeriodoAquisitivo, C↔TM/Q e FM↔DP têm ordens condicionais opostas, mas não demonstrei os ciclos completos com guards/FKs e travas anteriores satisfeitos. Em particular, C adquirido antes de FM no pago/devido pode impedir uma intercalação pretendida com o mesmo colaborador. (`app/ferias/services.py:302`; `app/ferias/services.py:367`; `app/ponto/services/ausencia.py:1245`; `app/ponto/services/justificativa.py:141`; `app/ponto/nucleo.py:81`; `app/chamados/services/materializacao.py:1277`; `app/ponto/portas/regen_exportada.py:179`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:193`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:196`; [nao verificado])

**2. Quem escreve e quem lê**

| Campo/valor | Escritor/produtor | Leitor/consumidor e significado |
|---|---|---|
| Contador: `departamento='dp'`, `user=None` | Nova porta obtém/cria; `lavrar` usa o mesmo alvo. (`app/pautas/services.py:350`; `app/pautas/services.py:363`) | Trava, unicidade, leitura e reconciliação. **É departamental, não por empresa/competência.** (`app/pautas/services.py:351`; `app/pautas/models.py:167`; `app/pautas/services.py:375`; `app/pautas/management/commands/lavrar_pautas.py:29`) |
| PK/retorno do contador | `.get()` executa e devolve a linha. (`app/pautas/services.py:351`) | Agrupador ignora o retorno e depende do efeito da trava; `lavrar` atualiza pelo PK. (`app/chamados/services/fecho_runtime.py:124`; `app/pautas/services.py:365`) |
| `nao_lidas` | Default zero; recontagem em `lavrar`. (`app/pautas/models.py:161`; `app/pautas/services.py:367`) | Serviço→navbar, aba e mensageria; comando compara antes/depois. (`app/pautas/services.py:377`; `app/pautas/context_processors.py:29`; `app/pautas/views.py:34`; `app/api/views_mensageria.py:1344`; `app/pautas/management/commands/lavrar_pautas.py:46`) |
| `atrasadas`, `lavrado_em` | Default/`auto_now` na criação; atualização canônica na lavra. (`app/pautas/models.py:162`; `app/pautas/models.py:163`; `app/pautas/services.py:366`; `app/pautas/services.py:368`) | Comando lê atrasadas; foto E4B inclui todos os campos do contador. Não achei outro leitor produtivo específico de `lavrado_em` desse modelo. (`app/pautas/management/commands/lavrar_pautas.py:46`; `app/chamados/services/foto_e4b.py:309`) |
| `por_competencia` e vazio/não vazio | Agrupador preenche após juiz temporal e janela. (`app/chamados/services/fecho_runtime.py:114`; `app/chamados/services/fecho_runtime.py:118`; `app/chamados/services/fecho_runtime.py:120`) | Guard novo decide tocar DP; laços ordenam travas e emissão. (`app/chamados/services/fecho_runtime.py:123`; `app/chamados/services/fecho_runtime.py:125`; `app/chamados/services/fecho_runtime.py:127`) |
| `data_evento`, `conversa` | Abertura factual grava turno/False; conversa grava dia civil/True; porta do fio atribui. (`app/chamados/models.py:2789`; `app/chamados/models.py:2801`; `app/chamados/services/fio.py:801`) | Agrupador usa discriminador pelo juiz; leitores crus remanescentes não o fazem. **Dois significados preexistentes permanecem.** (`app/chamados/juizes.py:336`; `app/chamados/services/fecho_runtime.py:114`; `app/chamados/services/disputa_emissao.py:1836`; `app/chamados/models.py:364`) |
| Empresa/corte | Cadastro do colaborador e da empresa. (`app/colaboradores/models.py:321`; `app/colaboradores/models.py:46`; `app/colaboradores/services/estrutura.py:19`) | Empresa da pergunta e corte efetivo alimentam janela. (`app/chamados/services/fecho_runtime.py:117`; `app/ponto/janelas.py:50`; `app/ponto/janelas.py:63`) |
| Competência, `ancora_tipo`, `ancora_id` | Fim da janela produz `<empresa>:AAAA-MM`; porta persiste. (`app/chamados/services/fecho_runtime.py:118`; `app/chamados/services/fecho_runtime.py:119`; `app/pautas/services.py:184`) | Agrupamento, advisory, dedup, chip e payload. Não achei novo leitor interpretando essa âncora como mês civil. (`app/chamados/services/fecho_runtime.py:125`; `app/chamados/services/fecho_runtime.py:128`; `app/pautas/services.py:336`; `app/pautas/views.py:40`) |
| Marcador/chave advisory | Constante e SHA256 de marcador+tipo+âncora. (`app/chamados/services/fecho_runtime.py:12`; `app/chamados/services/fecho_runtime.py:86`) | PostgreSQL recebe chave; dedup lê prefixo textual. (`app/chamados/services/fecho_runtime.py:89`; `app/chamados/services/fecho_runtime.py:129`) |
| Motivo/IDs limbo/texto | S143 produz motivo e nota; montador reserva texto e abrevia listas. (`app/chamados/services/disputa_emissao.py:1840`; `app/chamados/services/disputa_emissao.py:1855`; `app/chamados/services/fecho_runtime.py:66`; 

[truncado em 20000 de 30199 caracteres]

## 11/11 17:00 · sessao 2866ebe68a87 · - · - · origem=Ronald (tmux)

A entrada produtiva pelo wizard permite o contraexemplo C↔Cel sem trava C antecedente encontrada: wizard→`salvar_tipo`→propagação do template→regeneração. (`app/escala/views_wizard.py:108`; `app/escala/services/cadastro_tipo.py:306`; `app/escala/services/cadastro_tipo.py:320`; `app/escala/services/cadastro_tipo.py:348`; `app/ponto/management/commands/gerar_celulas.py:13`)

| Passo | T1 — fechamento runtime | T2 — edição do template |
|---|---|---|
| 1 | Adquire C e Ch; mantém o atomic. (`app/chamados/models.py:290`) | |
| 2 | | Adquire a mesma Cel por UPDATE, no atomic da regeneração. (`app/ponto/portas/celula.py:218`; `app/ponto/portas/celula.py:276`) |
| 3 | | Consulta cobrança ainda viva no estado commitado anterior de T1; tenta marcar a órfã e espera C, mantendo Cel. (`app/ponto/management/commands/gerar_celulas.py:243`; `app/ponto/management/commands/gerar_celulas.py:255`; `app/chamados/services/contexto.py:103`; `app/chamados/models.py:486`) |
| 4 | Salva Ch; o receiver chama o cartório, que tenta UPDATE da mesma Cel e espera T2, mantendo C. (`app/chamados/models.py:296`; `app/chamados/signals.py:217`; `app/ponto/services/cartorio.py:1845`; `app/ponto/portas/celula.py:1072`; `app/ponto/portas/celula.py:905`) | Espera C. (`app/chamados/models.py:2666`) |

Os guards podem coexistir: vínculo ativo com vigência; célula gerada em competência aberta, DNA alterado; cobrança viva de furo com `marcos_faltantes`, criada antes da nova `regenerada_em`; cartório com dia passado presente na grade e impressão desatualizada. Não achei guarda ou trava comum anterior que impeça essa configuração. (`app/escala/services/cadastro_tipo.py:334`; `app/ponto/portas/celula.py:146`; `app/ponto/portas/celula.py:250`; `app/ponto/portas/celula.py:263`; `app/ponto/portas/celula.py:274`; `app/escala/services/regeneracao.py:148`; `app/escala/services/regeneracao.py:154`; `app/ponto/services/cartorio.py:1381`; `app/ponto/services/cartorio.py:1388`)

Isso constitui um **contraexemplo estrutural permitido pelo código**, além da simples enumeração de arestas opostas. Reprodução executada e ocorrência em produção: **[nao verificado]**. A pré-trava DP da c9 acontece depois da transição e não alcança esse ciclo anterior. (`app/chamados/services/fecho_runtime.py:124`; `app/chamados/services/fecho_runtime.py:156`; `app/chamados/services/fecho_runtime.py:159`)

## 11/11 16:58 · sessao 2866ebe68a87 · - · - · origem=Ronald (tmux)

Cura do ciclo DP↔âncora: o único adquirente produtivo da advisory E4B encontrado é `_travar_ancora`, chamado depois da pré-trava DP; o contador é único por departamento com `user=NULL`, sem empresa/competência. Isso fecha o ciclo antigo entre os dois ramos E4B, inclusive quando a transição já adquiriu DP. Não demonstra ordem única para o restante do sistema. (`app/chamados/services/fecho_runtime.py:89`; `app/chamados/services/fecho_runtime.py:124`; `app/chamados/services/fecho_runtime.py:126`; `app/pautas/models.py:167`)

Convenções úteis: C=Colaborador; D=Disputa; Ch=Chamado; Q=Pergunta; Cel=CelulaDia; FM=FechamentoMensal. A porta `trava_na_ordem` adquire C→D→Ch→Q, cada conjunto ordenado por PK. (`app/chamados/models.py:2666`; `app/chamados/models.py:2675`; `app/chamados/models.py:2688`; `app/chamados/models.py:2692`)

A ordem interna comum de `escrever` é INSERT Pauta→trilha→`lavrar`: get_or_create contador→UPDATE contador. O `atomic` começa na porta; portanto chamador sem `atomic` externo libera suas travas no retorno. (`app/pautas/services.py:152`; `app/pautas/services.py:180`; `app/pautas/services.py:186`; `app/pautas/services.py:188`; `app/pautas/services.py:363`; `app/pautas/services.py:365`)

| Caminho especial | Ordem demonstrável |
|---|---|
| Cadastro sem turno | C→Pauta→DP; durante runtime C/D/Ch anteriores ficam retidos. (`app/chamados/services/fabrica_da_celula.py:364`; `app/chamados/services/fabrica_da_celula.py:365`; `app/chamados/services/fabrica_da_celula.py:369`) |
| Limbo exportado | Pauta→DP; o emissor não adquire sua própria trava C ou advisory. Pode herdar as travas do cartório/transição. (`app/chamados/services/fabrica_da_celula.py:268`; `app/chamados/services/fabrica_da_celula.py:279`; `app/ponto/services/cartorio.py:1020`) |
| Pago/devido exportada | C→FM FOR UPDATE→Pauta cabeça→DP→Pautas filhas→DP. (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:193`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:196`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:207`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:211`) |
| Passivo validado | Atomic do par→plantio/materialização→regeneração→FM→Pauta cabeça/filhas ou Pauta sem movimento→DP. O plantio pode adquirir TM por DELETE e só depois C/D/Ch/Q por portas disparadas/carimbo. (`app/chamados/services/passivo_validado.py:371`; `app/chamados/services/passivo_validado.py:428`; `app/chamados/services/passivo_validado.py:438`; `app/chamados/services/passivo_validado.py:443`; `app/chamados/services/passivo_validado.py:450`; `app/ponto/nucleo.py:81`; `app/chamados/services/materializacao.py:1277`) |
| Retificação de competência pelo passivo | As emissões são `escrever` da cabeça, filhas e sem movimento, não apenas uma chamada. (`app/chamados/services/passivo_validado.py:229`; `app/chamados/services/passivo_validado.py:233`; `app/chamados/services/passivo_validado.py:266`) |
| HE de período | Atomic externo→DecisaoHE FOR UPDATE→decidir HE/retrava decisão→Pauta→DP; não achei advisory E4B. (`app/ponto/services/autorizacao_he_periodo.py:321`; `app/ponto/services/autorizacao_he_periodo.py:343`; `app/ponto/portas/he.py:555`; `app/ponto/portas/he.py:562`; `app/ponto/portas/he.py:174`; `app/ponto/services/autorizacao_he_periodo.py:300`; `app/ponto/services/autorizacao_he_periodo.py:244`) |
| Férias novas/históricas | Agendamento.save→sincronização Ausencia→eventual Pauta→DP→UPDATE PeriodoAquisitivo. (`app/ferias/services.py:298`; `app/ferias/services.py:302`; `app/ferias/services.py:321`; `app/ferias/services.py:326`; `app/ponto/services/ausencia.py:1216`; `app/ponto/services/ausencia.py:1245`) |
| Alteração de férias | Agendamento FOR UPDATE→PeriodoAquisitivo FOR UPDATE→Ausencia/apagamento→Agendamento.save/sincronização→eventual Pauta→DP. (`app/ferias/services.py:364`; `app/ferias/services.py:367`; `app/ferias/services.py:434`; `app/ferias/services.py:445`; `app/ferias/services.py:450`; `app/ponto/services/ausencia.py:1245`) |
| Ler/fazer/cancelar Pauta | UPDATE Pauta→trilha→contador destinatário. (`app/pautas/services.py:215`; `app/pautas/services.py:218`; `app/pautas/services.py:247`; `app/pautas/services.py:251`; `app/pautas/services.py:272`; `app/pautas/services.py:276`; `app/pautas/services.py:294`; `app/pautas/services.py:297`) |
| Lavra avulsa | get_or_create contador→UPDATE contador; não achei âncora/C/D/Ch/Q/FM adquiridos pelo comando. (`app/pautas/management/commands/lavrar_pautas.py:51`; `app/pautas/services.py:363`; `app/pautas/services.py:365`) |

Os emissores abaixo chegam à ordem comum da porta; nesses trechos não achei aquisição própria de advisory E4B/FOR UPDATE. A ordem integral de todas as possíveis transações herdadas, inclusive chamadas dinâmicas, é **[nao verificado]**.

- UI e resposta: `app/pautas/views.py:76`; `app/pautas/views.py:112`; `app/pautas/services.py:194`.
- Suporte: resposta pela porta em `app/pautas/management/commands/suporte_hasner.py:133`, opcionalmente seguida de marcar feita em `:136`; fechamento em `:147`. Não achei atomic envolvendo resposta+fecho nesses métodos. (`app/pautas/management/commands/suporte_hasner.py:127`; `app/pautas/management/commands/suporte_hasner.py:143`)
- Semanal: `app/pautas/semanal.py:276`; consumidores DP em `app/ponto/services/vigia_ausencia.py:239` e supervisão em `app/escala/services/fase_12x36.py:277`.
- DP: `app/folha/management/commands/pre_fechamento.py:110`; `app/ferias/management/commands/pautas_de_ferias.py:167`; `app/colaboradores/management/commands/pauta_aparelhos_compartilhados.py:107`; `app/ponto/services/ausencia.py:1055`; `app/ponto/services/ausencia.py:1245`; `app/ponto/services/pautas_dos_discordantes.py:177`; `app/ponto/services/pautas_dos_deslogados.py:77`.
- Chamados: `app/chamados/services/resposta_tardia.py:63`; `app/chamados/services/competencia_trancada.py:231`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:208`.
- Comandos DP: `app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:134`; `:139`; `app/pautas/management/commands/pauta_dia_em_aberto.py:127`; `:139`; `:142`.
- Outros departamentos: `app/ponto/services/afastado_avisa.py:74`; `app/ponto/services/pautas_do_esmeril.py:76`; `app/ponto/services/cobertura.py:333`; `app/core/services/crons_quebrados.py:71`; `app/core/management/commands/alarme_esteira.py:54`; `app/core/management/commands/alarme_arvore.py:50`.

Não achei ciclo DP→UPDATE de Pauta existente no semanal: o ramo que fecha antigas exige zero acesos e retorna antes do ramo de escrita. Também não achei atomic externo nos dois consumidores semanais. (`app/pautas/semanal.py:220`; `app/pautas/semanal.py:235`; `app/pautas/semanal.py:276`; `app/ponto/services/vigia_ausencia.py:227`; `app/escala/services/fase_12x36.py:254`)

Discordantes, deslogados e esmeril escrevem novas antes de fechar antigas, mas não achei atomic envolvendo os dois laços nem nos commands encontrados; cada porta faz seu próprio atomic. Logo não demonstrei retenção DP→UPDATE da pauta antiga nesses caminhos. (`app/ponto/services/pautas_dos_discordantes.py:175`; `:183`; `app/ponto/services/pautas_dos_deslogados.py:76`; `:80`; `app/ponto/services/pautas_do_esmeril.py:75`; `:79`; `app/ponto/management/commands/pautas_discordantes.py:118`; `app/ponto/management/commands/lavrar_deslogados.py:82`; `app/ponto/management/commands/pautas_do_esmeril.py:17`)

As inversões globais remanescentes têm arestas reais no código, mas não contei arestas opostas como deadlock já demonstrado:

- **Cel→C:** regeneração faz UPDATE Cel e chama `_marcar_cobrancas_orfas` ainda no atomic; a marca chama `renovar_contexto`, que trava C/D/Ch. `editar_dia` também faz FOR UPDATE Cel antes da mesma marca. Frente à transição C→Cel existe inversão estrutural; estado que satisfaz os guards simultaneamente e reprodução concorrente: **[nao verificado]**. (`app/ponto/portas/celula.py:276`; `:289`; `:593`; `:626`; `app/ponto/management/commands/gerar_celulas.py:255`; `app/chamados/services/contexto.py:103`)
- **TM→C/Q:** materialização registra Batida, cujo receiver recomputa TM; o carimbo da pergunta aparece depois. A aprovação de justificativa começa por C/D/Ch antes da Batida/recompute. O ciclo concreto e os guards conjuntos: **[nao verificado]**. (`app/chamados/services/materializacao.py:1007`; `app/ponto/nucleo.py:81`; `app/chamados/services/materializacao.py:1277`; `app/ponto/services/justificativa.py:141`)
- **DP→FM potencial:** dentro da regeneração de exportada, o julgamento pode emitir Pauta DP antes do recálculo FM; o outro lado pago/devido toma FM antes de DP. Entretanto a regeneração também pode adquirir C por marca ou emissão anterior, bloqueando a intercalação com pago/devido, que toma C primeiro. Ciclo concreto sem serialização antecedente: **[nao verificado]**. (`app/ponto/portas/celula.py:434`; `app/ponto/services/cartorio.py:1020`; `app/chamados/services/fabrica_da_celula.py:279`; `app/ponto/portas/regen_exportada.py:179`; `app/ponto/services/fechamento.py:1160`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:193`; `:196`; `:207`)
- **DP↔PeriodoAquisitivo potencial:** novas/históricas podem chegar a DP antes do UPDATE do período; alteração trava período antes da sincronização/Pauta. Guards, compatibilidade das travas implícitas de FK e reprodução concorrente: **[nao verificado]**. (`app/ferias/services.py:298`; `:302`; `:367`; `:450`; `app/ponto/services/ausencia.py:1245`)

Não achei prova executada de concorrência a duas conexões nem evidência de ausência global de ciclos; o próprio recibo mantém isso pendente. (`app/docs/FIO-E0-V4-E4B-c9.txt:103`)

## 11/11 16:57 · sessao c0d6a01bcd4e · - · - · origem=Ronald (tmux)

Prova c9 e limites:

- **Mede SQL real executado no ORM, em uma conexão**, porque `execute_wrapper` registra e chama `execute`; captura SQL/parâmetros/`in_atomic_block`. O caso chama diretamente `_escrever_pautas`; não observa a transição/cartório anteriores. (`app/chamados/tests/test_e4b_c6_orm.py:274`; `app/chamados/tests/test_e4b_c6_orm.py:290`; `app/chamados/tests/test_e4b_c6_orm.py:291`)
- **O gate exige DP antes da PRIMEIRA advisory**, `SELECT … FROM pautas_contadorpauta`, `departamento=%s` com parâmetro `dp`, `user_id IS NULL`, `FOR UPDATE` e atomic verdadeiro; depois verifica as chaves exatas ordenadas e todas antes de `exists`/INSERT/UPDATE. Pré-travar por SELECT deixa de passar despercebido como na c8. (`app/chamados/tests/e4b_c6_verificar.py:108`; `app/chamados/tests/e4b_c6_verificar.py:117`; `app/chamados/tests/e4b_c6_verificar.py:121`; `app/chamados/tests/e4b_c6_verificar.py:143`; `app/chamados/tests/e4b_c6_verificar.py:152`)
- **DP depois da primeira âncora morre**, pois já viola comparação com a primeira advisory. Outro departamento/usuário também morre. Travar somente um caminho não observado pelo caso permanece fora da conclusão; não achei ensaio de duas transações nem do ato completo com emissão prévia da fábrica. (`app/chamados/tests/e4b_c6_verificar.py:113`; `app/chamados/tests/e4b_c6_verificar.py:117`; `app/chamados/tests/e4b_c6_verificar.py:121`; `app/chamados/tests/test_e4b_c6_orm.py:291`)
- **“Contador de outra empresa/competência” não existe neste modelo:** identidade é departamento ou usuário; unicidade parcial por departamento/user nulo. A âncora da Pauta tem empresa/competência; o contador DP é global. (`app/pautas/models.py:34`; `app/pautas/models.py:157`; `app/pautas/models.py:167`)
- **Não travar lote vazio é o comportamento correto**, não mutante defeituoso. Os testes exigem ausência de qualquer SQL no contador para recusas vazias, conversa e pergunta sem dia; o mutante faz a chamada incondicional. (`app/chamados/services/fecho_runtime.py:123`; `app/chamados/tests/test_e4b_c6_memoria.py:233`; `app/chamados/tests/test_e4b_c6_memoria.py:241`; `app/chamados/tests/test_e4b_c6_orm.py:249`; `app/chamados/tests/test_e4b_c6_orm.py:255`; `app/chamados/tests/e4b_c6_verificar.py:93`)
- **Não certifica aquisição efetiva/retencão até commit.** O gate não inspeciona resultado/PK do SELECT nem identidade/continuidade da transação; só forma SQL, parâmetros e booleano de atomic. Um SELECT com filtro adicional que exclua a linha ainda satisfaz seu reconhecimento; COMMIT intermediário não recebe classificação. O código produtivo atual usa `.get()` no alvo correto, e a unicidade fecha esse risco na implementação atual. (`app/chamados/tests/e4b_c6_verificar.py:104`; `app/chamados/tests/e4b_c6_verificar.py:108`; `app/chamados/tests/e4b_c6_verificar.py:121`; `app/chamados/tests/e4b_c6_verificar.py:129`; `app/pautas/services.py:351`; `app/pautas/models.py:167`)

Fixtures/fakes:

- **Fixture explícita limita a alegação de ordem global:** é `TestCase`, prepara Pauta DP pela porta antes do wrapper e portanto já atualizou/travou o contador no atomic externo do teste. A medição posterior vê a reaquisição, não a primeira aquisição de toda a transação. (`app/chamados/tests/test_e4b_c6_orm.py:30`; `app/chamados/tests/test_e4b_c6_orm.py:264`; `app/chamados/tests/test_e4b_c6_orm.py:290`; `app/pautas/services.py:188`; `app/pautas/services.py:365`)
- **Criação concorrente do contador não está ensaiada:** a própria fixture exige contador já existente; o fake sempre devolve `(contador, False)`. (`app/chamados/tests/test_e4b_c6_orm.py:263`; `app/chamados/tests/test_e4b_c6_orm.py:267`; `app/chamados/tests/test_e4b_c6_memoria.py:71`)
- **Memória não comprova rollback nem trava real:** atomic só incrementa profundidade; cursor só registra; escrita só acrescenta lista ou emite SQL sintético. (`app/chamados/tests/test_e4b_c6_memoria.py:36`; `app/chamados/tests/test_e4b_c6_memoria.py:45`; `app/chamados/tests/test_e4b_c6_memoria.py:76`; `app/chamados/tests/test_e4b_c6_memoria.py:245`)
- **Não achei fixture invisível explicando os funcionais:** juiz real verifica viva/mortas e entrega exportada/aberta; depois há `refresh_from_db` e asserts persistidos. A falha de rollback é injetada no resolver, não em `escrever`. (`app/chamados/tests/test_e4b_c6_orm.py:80`; `app/chamados/tests/test_e4b_c6_orm.py:84`; `app/chamados/tests/test_e4b_c6_orm.py:113`; `app/chamados/tests/test_e4b_c6_orm.py:298`; `app/chamados/tests/test_e4b_c6_orm.py:305`)
- **Roteiro preserva a mordida:** GREEN antes dos ensaios, failure único no caso/alvo, sem ERROR nem precondição; os resultados fabricados são testes do parser. (`app/chamados/tests/e4b_c6_verificar.py:308`; `app/chamados/tests/e4b_c6_verificar.py:320`; `app/chamados/tests/test_e4b_c6_protocolo.py:135`; `app/chamados/tests/test_e4b_c6_protocolo.py:141`)

Mapa produtivo do delta/contador:

| Campo/valor | Escritor/produtor | Leitor/consumidor |
|---|---|---|
| `departamento`, `user=None` | Nova `travar_contador` cria/seleciona; `lavrar` escolhe mesmo alvo. (`app/pautas/services.py:350`; `app/pautas/services.py:363`) | Constraints, `nao_lidas`, reconciliação e foto E4B. (`app/pautas/models.py:167`; `app/pautas/services.py:375`; `app/pautas/management/commands/lavrar_pautas.py:29`; `app/chamados/services/foto_e4b.py:306`) |
| Retorno de `travar_contador` | `.get()` executa e retorna modelo. (`app/pautas/services.py:351`) | Chamador ignora retorno; depende do efeito da trava. (`app/chamados/services/fecho_runtime.py:124`) |
| `nao_lidas` | Defaults zero; recontagem canônica `lavrar`. (`app/pautas/models.py:161`; `app/pautas/services.py:367`) | `nao_lidas`→navbar/view; comando compara antes/depois; foto lê campos. (`app/pautas/services.py:377`; `app/pautas/context_processors.py:29`; `app/pautas/views.py:34`; `app/pautas/management/commands/lavrar_pautas.py:46`; `app/chamados/services/foto_e4b.py:309`) |
| `atrasadas`, `lavrado_em` | Defaults/auto_now no create; `lavrar` atualiza data e atrasadas. (`app/pautas/models.py:162`; `app/pautas/models.py:163`; `app/pautas/services.py:366`; `app/pautas/services.py:368`) | Comando lê atrasadas; foto inclui contador; não achei outro leitor produtivo específico de `lavrado_em` de ContadorPauta. (`app/pautas/management/commands/lavrar_pautas.py:46`; `app/chamados/services/foto_e4b.py:309`) |
| Guard `por_competencia` | Agrupamento deriva juiz de dia + janela da empresa. (`app/chamados/services/fecho_runtime.py:114`; `app/chamados/services/fecho_runtime.py:118`; `app/chamados/services/fecho_runtime.py:120`) | Novo guard evita contador no vazio; sorted ordena advisory e emissão. (`app/chamados/services/fecho_runtime.py:123`; `app/chamados/services/fecho_runtime.py:125`; `app/chamados/services/fecho_runtime.py:127`) |
| Contador em geral | Cinco chamadas internas de `lavrar`, mais command direto. (`app/pautas/services.py:188`; `app/pautas/services.py:218`; `app/pautas/services.py:251`; `app/pautas/services.py:276`; `app/pautas/services.py:297`; `app/pautas/management/commands/lavrar_pautas.py:51`) | Nova trava tem único chamador produtivo encontrado. (`app/chamados/services/fecho_runtime.py:124`) |

Não achei segundo escritor produtivo fora da porta nem novo juiz no delta; `Pauta` e `ContadorPauta` permanecem donos registrados em `pautas/services.py`. O selo tem allowlist vazia e verifica quatro campos; execução do selo nesta volta [nao verificado]. (`app/core/portas.py:507`; `app/pautas/tests/test_contract_um_escritor.py:19`; `app/pautas/tests/test_contract_um_escritor.py:24`; `app/pautas/tests/test_contract_um_escritor.py:37`)

Observação semântica para FILA: se existir Pauta DP viva mas faltar contador, a nova trava cria zero com `lavrado_em` atual; dedup pode sair sem chamar `lavrar`. O badge já retornava zero na ausência da linha, mas agora existe carimbo de “lavra” sem recontagem. Não achei FK exigindo contador para Pauta nem teste dessa criação/dedup; ocorrência concreta [nao verificado]. (`app/pautas/services.py:350`; `app/pautas/models.py:161`; `app/pautas/models.py:163`; `app/chamados/services/fecho_runtime.py:128`; `app/chamados/services/fecho_runtime.py:130`; `app/pautas/services.py:377`)

## 11/11 16:56 · sessao 217b4307724d · - · - · origem=Ronald (tmux)

Cura do ciclo da c8: **pela CLASSE dos atuais adquirentes da âncora E4B**. `_travar_ancora` é o único ponto de aquisição dessa advisory, chamado apenas pelo laço que agora sucede a trava DP; o contador tem unicidade por departamento com `user IS NULL`, e a nova porta usa os mesmos filtros de `lavrar`. A transição permanece anterior ao agrupador, mas sua aquisição antecipada de DP deixa de inverter DP↔âncora. (`app/chamados/services/fecho_runtime.py:83`; `app/chamados/services/fecho_runtime.py:124`; `app/chamados/services/fecho_runtime.py:126`; `app/chamados/services/fecho_runtime.py:156`; `app/pautas/services.py:350`; `app/pautas/services.py:363`; `app/pautas/models.py:167`)

Ordens do runtime:

- Duas bordas humanas → `transicionar_com_pauta`; ato externo abre `atomic`, transiciona, restaura contexto e entrega lote. (`app/chamados/services/acoes_chamado.py:315`; `app/chamados/services/fio.py:721`; `app/chamados/services/fecho_runtime.py:152`; `app/chamados/services/fecho_runtime.py:156`; `app/chamados/services/fecho_runtime.py:159`)
- Escritor da transição → **Colaborador → Disputas abertas → Chamados → UPDATE Chamado → receivers S143/cartório → eventuais Perguntas/Pautas → relavra S2 → DP → âncoras ordenadas → exists → INSERT Pauta/trilha → UPDATE DP**. A ordem dos grupos C/D/Ch/P está implementada na porta única; S143 usa-a novamente, de forma reentrante. (`app/chamados/models.py:280`; `app/chamados/models.py:290`; `app/chamados/models.py:296`; `app/chamados/models.py:309`; `app/chamados/models.py:2666`; `app/chamados/models.py:2675`; `app/chamados/models.py:2688`; `app/chamados/models.py:2692`; `app/chamados/services/disputa_emissao.py:1878`; `app/chamados/services/fecho_runtime.py:124`; `app/pautas/services.py:180`; `app/pautas/services.py:188`)
- Ramo cartório → fábrica cadastro: **C/D/Ch já presos → julgamento → C reentrante → INSERT Pauta → DP → retorno → eventuais escritas restantes da transição → DP reentrante → A**. (`app/chamados/signals.py:217`; `app/ponto/services/cartorio.py:1020`; `app/chamados/services/fabrica_da_celula.py:204`; `app/chamados/services/fabrica_da_celula.py:365`; `app/chamados/services/fabrica_da_celula.py:369`; `app/chamados/services/fecho_runtime.py:159`)
- Ramo cartório → fábrica limbo: **C/D/Ch herdados → julgamento → INSERT Pauta → DP → retorno → DP reentrante → A**. Esse emissor não adquire a advisory E4B. (`app/chamados/signals.py:217`; `app/ponto/services/cartorio.py:1020`; `app/chamados/services/fabrica_da_celula.py:215`; `app/chamados/services/fabrica_da_celula.py:279`; `app/chamados/services/fecho_runtime.py:124`)
- Agrupador avulso: **atomic → DP (se lote válido não vazio) → todas A ordenadas → exists → escrever → DP**. (`app/chamados/services/fecho_runtime.py:122`; `app/chamados/services/fecho_runtime.py:124`; `app/chamados/services/fecho_runtime.py:125`; `app/chamados/services/fecho_runtime.py:128`; `app/chamados/services/fecho_runtime.py:132`)

Não achei nova inversão DP↔A entre esses caminhos atuais; ciclos globais com outras tabelas dependem da auditoria dos demais chamadores. Ocorrência em produção e reprodução de espera circular: **[nao verificado]**. (`app/chamados/services/fecho_runtime.py:124`; `app/chamados/services/fecho_runtime.py:126`)

Classificação dos demais achados c8:

| Achado | Situação e evidência |
|---|---|
| Dia civil virando competência | **Curado pela CLASSE desse leitor**: usa `_dia_de_turno_da_pergunta`, exclui `None`, consulta janela da empresa. (`app/chamados/services/fecho_runtime.py:114`; `app/chamados/services/fecho_runtime.py:118`; `app/chamados/juizes.py:332`) |
| Duplicação marcador+âncora | **Curado pela CLASSE dos participantes atuais do agrupador**; genericamente não achei constraint marcador+âncora no modelo. (`app/chamados/services/fecho_runtime.py:124`; `app/chamados/services/fecho_runtime.py:128`; `app/pautas/models.py:123`) |
| Motivo limbo truncado | **Curado pela CLASSE dos formatos produzidos pelo S143**, com reserva textual e IDs abreviados sem cortar identificador. ORM compara texto entregue/persistido. (`app/chamados/services/fecho_runtime.py:64`; `app/chamados/services/fecho_runtime.py:76`; `app/chamados/tests/test_e4b_c6_orm.py:228`; `app/chamados/tests/test_e4b_c6_orm.py:230`) |
| Coleta após rollback S143 | **Curado pela CLASSE de exceções propagadas**: merge só após saída normal do savepoint e contexto restaurado em `finally`. (`app/chamados/services/fecho_runtime.py:36`; `app/chamados/services/fecho_runtime.py:40`; `app/chamados/signals.py:115`) |
| Reentrega não acrescenta IDs | **Não curado — FILA**: pauta viva provoca `continue`; nota individual S143 segue sendo escrita. (`app/chamados/services/fecho_runtime.py:128`; `app/chamados/services/fecho_runtime.py:130`; `app/chamados/services/disputa_emissao.py:1855`) |
| Fechamento fora das duas bordas | **Não curado — FILA de alcance**: `fechar` usa transição direta; só duas bordas instalam contexto. (`app/chamados/models.py:452`; `app/chamados/models.py:454`; `app/chamados/services/acoes_chamado.py:315`; `app/chamados/services/fio.py:721`) |
| Relógio fora da borda | **Não curado — FILA preexistente**: S143 fornece `hoje=None`; juiz usa `date.today()`. (`app/chamados/services/disputa_emissao.py:1945`; `app/chamados/juizes.py:577`) |
| Prova concorrente/falha real escrever | **Não curado — FILA de prova**: teste de travas chama agrupador diretamente; falha injetada é no resolver, não em `pautas.services.escrever`. Não achei ensaio da falha real dessa escrita. (`app/chamados/tests/test_e4b_c6_orm.py:291`; `app/chamados/tests/test_e4b_c6_orm.py:298`; `app/chamados/tests/test_e4b_c6_orm.py:305`) |
| Leitores crus `data_evento` | **Não curado — FILA preexistente**. (`app/chamados/services/disputa_emissao.py:1836`; `app/chamados/services/disputa_emissao.py:1984`; `app/chamados/services/disputa_emissao.py:2056`; `app/chamados/models.py:364`) |
| Lint | Cura anterior preservada; GREEN fornecido pelo anexo, certificação independente **[nao verificado]**. (`app/chamados/tests/test_e4b_c6_memoria.py:7`; `app/chamados/tests/test_e4b_c6_orm.py:274`) |
| Suíte inteira | **Não curado — BLOQUEANTE de encerramento**, conforme exigência e dado fornecido ainda parcial. (`CLAUDE.md:375`; `app/docs/FIO-E0-V4-E4B-c9.txt:103`) |

Classes antigas: não achei segundo juiz ou escrita produtiva E4B fora da porta no delta; composição usa juiz temporal, janela já existente e `escrever`. O retorno antecipado continua ausente: depois da recusa seguem S128, HX e C7. (`app/chamados/services/fecho_runtime.py:107`; `app/chamados/services/fecho_runtime.py:110`; `app/chamados/services/fecho_runtime.py:132`; `app/chamados/services/disputa_emissao.py:1950`; `app/chamados/services/disputa_emissao.py:1953`; `app/chamados/services/disputa_emissao.py:2007`; `app/chamados/services/disputa_emissao.py:2089`)

Campo com dois significados permanece preexistente: `abrir` escreve dia do turno com `conversa=False`, `abrir_conversa` escreve dia civil com `conversa=True`; leitores crus acima continuam sem o discriminador. (`app/chamados/models.py:2789`; `app/chamados/models.py:2801`; `app/chamados/juizes.py:336`; `app/chamados/services/disputa_emissao.py:1836`)

Externos do aval: não achei instalação de coletor nesses chamadores; chamam resolver e `registrar_recusa` sem contexto não entrega Pauta E4B. Quantidade de Pautas E4B fora do runtime em produção: **[nao verificado]**. (`app/chamados/services/becos.py:54`; `app/chamados/reconciliador.py:536`; `app/chamados/reconciliador.py:1648`; `app/ponto/management/commands/reconciliar_fantasmas.py:281`; `app/chamados/services/fecho_runtime.py:18`)

Fixture e conclusão: cenários funcionais estabelecem viva/mortas e exportada/aberta pelos juízes reais e conferem persistência, portanto não achei fixture invisível que explique esses GREENs; prova da ordem permanece limitada porque semeia Pauta/contador antes de instalar wrapper e chama diretamente o agrupador, sem transição/cartório. (`app/chamados/tests/test_e4b_c6_orm.py:80`; `app/chamados/tests/test_e4b_c6_orm.py:84`; `app/chamados/tests/test_e4b_c6_orm.py:113`; `app/chamados/tests/test_e4b_c6_orm.py:264`; `app/chamados/tests/test_e4b_c6_orm.py:290`; `app/chamados/tests/test_e4b_c6_orm.py:291`)

## 11/11 16:42 · sessao bba41c8761ef · LASTRO-FOLGA-FORA-DO-CATALOGO · verificador · origem=Code

**APROVADO na pré-verificação estática da c2c.** As duas correções correspondem aos caminhos afetados; execução da suíte e aprovação de pouso permanecem **[nao verificado]** (`app/ponto/tests/test_lastro_folga_save_guarda.py:180`, `app/ponto/catalogo/ausencias.py:261`, `CLAUDE.md:377`).

**(1)** `aguardando_decisao` é choice vigente, também default do campo, e aparece nos três sítios corrigidos, inclusive `_nova` (`app/ponto/models.py:702`, `app/ponto/models.py:799`, `app/ponto/tests/test_lastro_folga_save_guarda.py:52`, `app/ponto/tests/test_lastro_folga_save_guarda.py:141`, `app/ponto/tests/test_lastro_folga_save_guarda.py:180`).

**Nenhum desses testes passava antes por uma recusa de status no `full_clean`.** Na criação, essa recusa gerava `AusenciaInvalida.codigo == ''`, enquanto o teste exige `tipo_invalido`: portanto falhava antecipadamente, sem alcançar a recusa simulada do escritor. Os demais caminhos testados usam `save`, edição ou `rederivar_fim`, sem chamar `full_clean` (`app/ponto/services/ausencia.py:63`, `app/ponto/services/ausencia.py:395`, `app/ponto/services/ausencia.py:397`, `app/ponto/tests/test_lastro_folga_save_guarda.py:182`, `app/ponto/models.py:896`, `app/ponto/services/ausencia.py:522`, `app/ponto/services/ausencia.py:1314`).

A mordida estática de cada teste é: [nao verificado]

| Teste, abreviado | Braço exercitado e limite | [nao verificado]
|---|---|
| Nova fora do vivo | Exige recusa no `save` e nenhuma linha; aceita qualquer `ValidationError` (`app/ponto/tests/test_lastro_folga_save_guarda.py:73`). |
| `objects.create` fora do vivo | Exige recusa na criação direta e nenhuma linha; usa status válido `aprovada` (`app/ponto/tests/test_lastro_folga_save_guarda.py:79`, `app/ponto/models.py:703`). |
| Vivo → morto | Exige recusa e preservação do tipo gravado (`app/ponto/tests/test_lastro_folga_save_guarda.py:86`). |
| Legado → outro morto | Exige recusa e preservação do legado (`app/ponto/tests/test_lastro_folga_save_guarda.py:94`). |
| Desativado, sem troca | Verifica saída do catálogo e regravação com tipo/status preservados (`app/ponto/tests/test_lastro_folga_save_guarda.py:102`). |
| Legado sem troca / rederivar | Chama ambos os caminhos e exige ausência de recusa; verifica tipo persistido (`app/ponto/tests/test_lastro_folga_save_guarda.py:115`). |
| Legado → vivo | Exige persistência do tipo vivo (`app/ponto/tests/test_lastro_folga_save_guarda.py:127`). |
| Vivo fora das choices | Verifica ambas as pré-condições e exige gravação (`app/ponto/tests/test_lastro_folga_save_guarda.py:134`). |
| Cache entre schemas | Detecta resposta de A servida em B; usa mocks, sem troca real de schema (`app/ponto/tests/test_lastro_folga_save_guarda.py:157`). |
| Tradução na criação | Agora pode alcançar a guarda simulada; exige classe/código traduzidos e nenhuma linha (`app/ponto/tests/test_lastro_folga_save_guarda.py:169`, `app/ponto/tests/test_lastro_folga_save_guarda.py:175`). |
| Tradução na edição | Exige classe/código traduzidos; não verifica novamente o estado persistido (`app/ponto/tests/test_lastro_folga_save_guarda.py:189`). |
| Parcial sem `tipo` | Prova ausência de veto indevido e escrita correta. **Não prova literalmente “não pergunta”**, pois não verifica chamadas ao catálogo (`app/ponto/tests/test_lastro_folga_save_guarda.py:203`). |
| Parcial com `tipo` morto | Exige recusa e tipo anterior preservado; aceita qualquer `ValidationError` (`app/ponto/tests/test_lastro_folga_save_guarda.py:215`). |

A segunda classe herda os oito testes da primeira; execução e morte efetiva de mutantes: **[nao verificado]** (`app/ponto/tests/test_lastro_folga_save_guarda.py:145`).

**(2) Sim, existem outros casos, indiretamente pela view de edição:**

- `test_aus_salvar_erro.py` monta `status='pendente'`, envia à edição e exige sucesso/descrição persistida (`app/ponto/tests/test_aus_salvar_erro.py:30`, `app/ponto/tests/test_aus_salvar_erro.py:37`, `app/ponto/tests/test_aus_salvar_erro.py:74`).
- `test_editar_minutos.py` monta o mesmo status inválido; os testes de parcial e ausência de minutos exigem alteração persistida (`app/ponto/tests/test_editar_minutos.py:33`, `app/ponto/tests/test_editar_minutos.py:41`, `app/ponto/tests/test_editar_minutos.py:55`, `app/ponto/tests/test_editar_minutos.py:75`).

Ambos chegam a `editar_ausencia_registro` pela view. Não são falsos verdes por recusa de `full_clean`: a edição faz `save` sem essa validação, conservando a fixture inválida (`app/ponto/views.py:2794`, `app/ponto/services/ausencia.py:522`, `app/ponto/models.py:898`, `app/ponto/models.py:938`). Ausência absoluta de outros casos indiretos: **[nao verificado]**.

**(3)** A anotação é **compatível, embora pouco precisa no catálogo**, pois `object` admite o dicionário realmente armazenado (`app/ponto/catalogo/ausencias.py:261`, `app/ponto/catalogo/ausencias.py:289`).

| Papel | Chave e valor |
|---|---|
| Origem da chave | `_schema()` retorna `connection.schema_name`, ou `None` sem atributo (`app/ponto/catalogo/ausencias.py:265`). |
| Escritor | Grava na chave atual `(agora, v)`; `agora` vem de `monotonic`, e `v` do banco ou da semente (`app/ponto/catalogo/ausencias.py:283`, `app/ponto/catalogo/ausencias.py:284`, `app/ponto/catalogo/ausencias.py:288`, `app/ponto/catalogo/ausencias.py:289`). |
| Origem do catálogo | Dicionário código → tupla dos sete atributos, ou `CATALOGO` (`app/ponto/catalogo/ausencias.py:249`, `app/ponto/catalogo/ausencias.py:250`, `app/ponto/catalogo/ausencias.py:87`). |
| Leitor direto | Busca pela mesma chave; lê posição 0 como tempo e devolve posição 1 como catálogo (`app/ponto/catalogo/ausencias.py:285`, `app/ponto/catalogo/ausencias.py:286`, `app/ponto/catalogo/ausencias.py:287`). |
| Consumidores | Iteram códigos, fazem `.get` e indexam atributos do catálogo (`app/ponto/catalogo/ausencias.py:294`, `app/ponto/catalogo/ausencias.py:303`, `app/ponto/catalogo/ausencias.py:314`). |
| Invalidação | Limpa o dicionário inteiro (`app/ponto/catalogo/ausencias.py:272`). |

Resultado efetivo de `test_contract_mypy` nesta árvore: **[nao verificado]** (`app/core/tests/test_contract_mypy.py:19`).

**(4)** Não identifico bloqueante novo nas classes indicadas:

- **Selo/RED:** há mordida lógica, com as limitações explicitadas na tabela; RED/GREEN executados: **[nao verificado]** (`app/ponto/tests/test_lastro_folga_save_guarda.py:73`, `app/ponto/tests/test_lastro_folga_save_guarda.py:175`).
- **Status com dois significados:** os estados atuais separam documento e decisão; os dois arquivos do item (2) ainda conservam fixtures com o estado extinto (`app/ponto/models.py:690`, `app/ponto/models.py:701`, `app/ponto/models.py:702`, `app/ponto/tests/test_aus_salvar_erro.py:32`, `app/ponto/tests/test_editar_minutos.py:35`).
- **Segundo escritor:** a porta declarada continua sendo o serviço; o contorno por `update(tipo=...)` é fixture explícita de legado (`app/core/portas.py:486`, `app/ponto/tests/test_lastro_folga_save_guarda.py:63`, `app/ponto/tests/test_lastro_folga_save_guarda.py:67`).
- **Pré-requisito/pouso:** o BACKLOG exige medição; o RELATO registra censo, sonda e conferência. Artefatos originais, suíte integral verde e opus final: **[nao verificado]** (`app/docs/BACKLOG.md:1807`, `app/docs/RELATO.md:7368`, `CLAUDE.md:377`).

Permanece a contradição documental já apontada: o teste afirma que `folga` nunca esteve viva, mas o RELATO registra que estava. A pré-condição local sustenta o cenário de teste, não a afirmação histórica (`app/ponto/tests/test_lastro_folga_save_guarda.py:3`, `app/ponto/tests/test_lastro_folga_save_guarda.py:48`, `app/docs/RELATO.md:7369`).

## 11/11 16:41 · sessao 0c448c27339b · - · - · origem=Ronald (tmux)

**Há outros testes com status inválido que chegam à edição, indiretamente pela view:** [nao verificado]

- `app/ponto/tests/test_aus_salvar_erro.py:30-32` monta `Ausencia(status='pendente')`; `_post` chama a rota de edição em `:37-38`, e `test_04_sucesso_segue_salvando` exige sucesso e descrição persistida em `:74-79`. A view chama `editar_ausencia_registro` em `app/ponto/views.py:2794-2802`.
- `app/ponto/tests/test_editar_minutos.py:33-35` também monta `Ausencia(status='pendente')`; `_post` chama a mesma rota em `:37-42`. Os testes que exigem gravação são `test_02_tipo_parcial_aceita_minutos` em `:55-63` e `test_03_sem_minutos_zera_como_antes` em `:75-78`. O teste de recusa de tipo não parcial chama a porta em `:65-73`, cuja recusa antecede o save em `app/ponto/services/ausencia.py:492-496`.

`pendente` está fora dos quatro valores vigentes de `Ausencia.STATUS_CHOICES`, declarados em `app/ponto/models.py:701-704`.

Esses casos **não são falso verde causado por `full_clean`**: a edição chama `ausencia.save()` em `app/ponto/services/ausencia.py:521-527`; esse save aplica a guarda de tipo e as derivações, sem `full_clean` nem validação das choices de status, em `app/ponto/models.py:896-938`.

Não encontrei outro caso **direto**, fora do arquivo em revisão, que passe status inválido para `criar_ausencia`/`editar_ausencia_registro` ou monte `Ausencia` inválida e chame `full_clean`; exaustividade semântica além da busca estática: **[nao verificado]**. Execução dos testes: **[nao verificado]**. [nao verificado]

## 11/11 16:41 · sessao 1409a1f6a144 · - · - · origem=Ronald (tmux)

Item (4), sem bloqueante novo demonstrado no delta informado:

- **Status com dois significados:** `pendente` foi explicitamente extinto por misturar dívida documental do colaborador com decisão do DP; os dois estados atuais são separados. `aguardando_decisao` é choice vigente e default do campo (`app/ponto/models.py:690`, `app/ponto/models.py:701`, `app/ponto/models.py:702`, `app/ponto/models.py:799`).
- **Segundo escritor:** o censo AST executado, sem Django e com `python3 -B`, atribuiu todos os sítios produtivos encontrados de `Ausencia` ao serviço declarado como porta; criação em `app/ponto/services/ausencia.py:393`, edição em `app/ponto/services/ausencia.py:510`, persistência em `app/ponto/services/ausencia.py:522`, declaração em `app/core/portas.py:486`. O `update(tipo=...)` fora do save é fixture explícita de legado (`app/ponto/tests/test_lastro_folga_save_guarda.py:63`, `app/ponto/tests/test_lastro_folga_save_guarda.py:67`). Cobertura universal além das atribuições AST: **[nao verificado]**, pois o instrumento admite operações não atribuíveis (`app/core/censo_escritas.py:43`).
- **Pouso antes do pré-requisito:** o BACKLOG exige medir escritor/frota; o RELATO registra censo, sonda e conferência. A linha ainda aberta não demonstra descumprimento, mas os artefatos originais e o escritor histórico exato permanecem **[nao verificado]** (`app/docs/BACKLOG.md:1807`, `app/docs/RELATO.md:7368`).
- **Limitação documental herdada:** o teste ainda afirma que `folga` nunca esteve no catálogo vivo, enquanto o RELATO registra que estava; a pré-condição local verifica apenas o catálogo da fixture (`app/ponto/tests/test_lastro_folga_save_guarda.py:3`, `app/ponto/tests/test_lastro_folga_save_guarda.py:29`, `app/ponto/tests/test_lastro_folga_save_guarda.py:48`, `app/docs/RELATO.md:7369`).
- **Certificação final:** RED/GREEN, mutantes, suite integral desta árvore e opus final: **[nao verificado]**. A regra exige suite integral verde antes do opus final e impede o pré-verificador de aprovar sozinho o pouso (`CLAUDE.md:377`, `CLAUDE.md:378`, `CLAUDE.md:381`). [nao verificado]

## 11/11 16:41 · sessao f4b0c04b7cbd · - · - · origem=Ronald (tmux)

`aguardando_decisao` é valor vigente de `STATUS_CHOICES`, usado nos três sítios alterados: default de `_nova`, criação direta de código vivo e teste da porta de criação (`app/ponto/models.py:702`; `app/ponto/tests/test_lastro_folga_save_guarda.py:52`; `app/ponto/tests/test_lastro_folga_save_guarda.py:141`; `app/ponto/tests/test_lastro_folga_save_guarda.py:180`).

Nenhum teste dessas classes passava antes porque `full_clean` recusava o status inválido: o único caminho testado que chama `full_clean` é `criar_ausencia`; sua tradução desse erro usa `codigo=''`, enquanto o teste exige `tipo_invalido`, logo o status anterior causava falha antecipada, não falso positivo (`app/ponto/services/ausencia.py:395`; `app/ponto/services/ausencia.py:397`; `app/ponto/services/ausencia.py:63`; `app/ponto/tests/test_lastro_folga_save_guarda.py:182`). Os demais caminhos chegam ao `save` sem `full_clean`; trocar o status remove dado inválido da fixture, sem trocar a causa esperada de recusa (`app/ponto/models.py:896`; `app/ponto/models.py:938`; `app/ponto/services/ausencia.py:522`; `app/ponto/services/ausencia.py:1314`).

| Teste — identificação abreviada | Mordida estática e limite | [nao verificado]
|---|---|
| Nova fora do vivo | Chama `save`, exige `ValidationError` e ausência de linha; guarda recusa código novo fora do catálogo. Aceita qualquer `ValidationError` (`app/ponto/tests/test_lastro_folga_save_guarda.py:73`; `app/ponto/models.py:890`). |
| `objects.create` fora do vivo | Percorre criação direta, exige erro e nenhuma linha; status explícito `aprovada` também é válido (`app/ponto/tests/test_lastro_folga_save_guarda.py:79`; `app/ponto/models.py:703`). |
| Vivo → morto | Salva código vivo, tenta troca e confere persistência do código anterior (`app/ponto/tests/test_lastro_folga_save_guarda.py:86`). |
| Legado → outro morto | Fixture contorna `save` com `update`, troca para outro inválido e confere preservação do legado (`app/ponto/tests/test_lastro_folga_save_guarda.py:63`; `app/ponto/tests/test_lastro_folga_save_guarda.py:94`). |
| Tipo desativado, sem troca | Desativa cadastro, verifica saída do catálogo, regrava e confere tipo e status persistidos (`app/ponto/tests/test_lastro_folga_save_guarda.py:102`). |
| Legado sem troca / `rederivar_fim` | Regrava legado e chama efetivamente `rederivar_fim`, que faz novo `save`; confere tipo persistido (`app/ponto/tests/test_lastro_folga_save_guarda.py:115`; `app/ponto/services/ausencia.py:1314`). |
| Legado → vivo | Exige gravação e confere tipo vivo persistido (`app/ponto/tests/test_lastro_folga_save_guarda.py:127`). |
| Vivo fora das choices | Verifica ambas as pré-condições e exige gravação pelo `objects.create` (`app/ponto/tests/test_lastro_folga_save_guarda.py:134`). |
| Cache entre schemas | Atribui schemas e catálogos diferentes por mocks e exige respostas distintas; não demonstra troca real de schema nem retorno A→B→A (`app/ponto/tests/test_lastro_folga_save_guarda.py:157`). |
| Tradução na criação | Guarda mockada levanta código exato; teste exige `AusenciaInvalida.codigo='tipo_invalido'` e nenhuma linha. Após correção do status, a passagem por `full_clean` deixa de impedir alcançar o mock (`app/ponto/tests/test_lastro_folga_save_guarda.py:169`; `app/ponto/tests/test_lastro_folga_save_guarda.py:175`; `app/ponto/services/ausencia.py:395`; `app/ponto/services/ausencia.py:426`). |
| Tradução na edição | Guarda mockada levanta código exato; teste exige tradução. Não confere novamente o estado persistido (`app/ponto/tests/test_lastro_folga_save_guarda.py:189`; `app/ponto/services/ausencia.py:523`). |
| Parcial sem `tipo` | Detecta veto indevido, verifica descrição persistida e tipo preservado. O nome promete “não pergunta”, mas não há spy/assertiva de ausência de consulta: prova o comportamento da escrita, não literalmente zero chamadas ao catálogo (`app/ponto/tests/test_lastro_folga_save_guarda.py:203`). |
| Parcial com `tipo` morto | Exige recusa e preservação do tipo; aceita qualquer `ValidationError`, sem exigir código específico (`app/ponto/tests/test_lastro_folga_save_guarda.py:215`). |

`VoltaC2PreverifCodex` herda também os testes da primeira classe (`app/ponto/tests/test_lastro_folga_save_guarda.py:145`). Execução, RED/GREEN e mutantes nesta avaliação: **[nao verificado]**.

## 11/11 16:41 · sessao 671b82e37094 · PROPORCAO-POR-RISCO · verificador · origem=Code

APROVADO

Bloqueadores: nenhum demonstrado para os caminhos fixados no contexto da revisão; a EXCECAO precede a LISTA e as demais regras B. (`app/docs/CORTES.md:444`, `app/docs/CORTES.md:445`, `bin/selo_proporcao.py:113`, `bin/selo_proporcao.py:115`)

1. **As duas metades estão implementadas.** `app/docs/*.md` entrou na LISTA; `app/static/pwa.js` e `app/static/sw.js` entraram nominalmente na EXCECAO. (`app/docs/CORTES.md:444`, `app/docs/CORTES.md:445`)

   O rastro do SW é: as duas cascas carregam `pwa.js`; ele registra `/sw.js`; a URL chama `service_worker`, que lê o arquivo e responde JavaScript, sem renderizar template. (`app/templates/base.html:20`, `app/templates/base_app.html:20`, `app/static/pwa.js:14`, `app/config/urls.py:39`, `app/core/urls.py:24`, `app/core/views.py:570`, `app/core/views.py:573`)

   O arquivo aberto é **`/app/staticfiles/sw.js`, se existir; senão `/app/static/sw.js`**. A fonte `app/static/sw.js` está na EXCECAO pelo caminho exato; qual ramo e conteúdo estão efetivamente em produção é `[nao verificado]`. (`app/core/views.py:529`, `app/core/views.py:530`, `app/docs/CORTES.md:445`)

   Há uma inconsistência textual no dado: o aval anexado cita `static/js/pwa.js`, enquanto o contexto da revisão fixa `app/static/pwa.js`. O primeiro, traduzido para `app/static/js/pwa.js`, recebe B inclusive no harness; portanto o APROVADO considera o caminho explicitamente fixado no contexto, sem equiparar os dois nomes. (`aval_proporcao_docs_e_pwa_1111.txt:1`, anexo; `app/docs/CORTES.md:445`, `bin/tests/test_selo_proporcao.sh:99`)

2. **O curinga fica no último segmento e não atravessa pasta.** A validação proíbe `*?[` na pasta e `?[` no nome; o casamento exige pasta idêntica e usa `fnmatchcase`. (`bin/selo_proporcao.py:89`, `bin/selo_proporcao.py:90`, `bin/selo_proporcao.py:101`, `bin/selo_proporcao.py:103`)

   | Caminho, com a LISTA atual | Resultado |
   |---|---|
   | `app/docs/NOTA.md`, `app/docs/nota com espaco.md` | B pela LISTA. (`app/docs/CORTES.md:444`, `bin/selo_proporcao.py:103`) |
   | `app/docs/sub/NOTA.md`, `app/docs/../NOTA.md` | A: pasta diferente, sem normalização de `..`. (`bin/selo_proporcao.py:99`, `bin/selo_proporcao.py:103`, `bin/selo_proporcao.py:127`) |
   | `app/Docs/NOTA.md`, `app/docs/NOTA.MD` | A: comparação sensível à caixa. (`bin/selo_proporcao.py:103`, `bin/selo_proporcao.py:127`) |
   | `app/docs/NOTA.md/` | A: último segmento vazio e pasta diferente. (`bin/selo_proporcao.py:99`, `bin/selo_proporcao.py:103`, `bin/selo_proporcao.py:127`) |
   | `.md` fora de `app/docs` | Não entra por esse padrão; pode receber B por outra regra, como `static` ou `templates`. (`bin/selo_proporcao.py:103`, `bin/selo_proporcao.py:121`, `bin/selo_proporcao.py:123`) |

   Espaços não são separadores: o diff usa NUL. Apagados são julgados, e renomeações viram apagado mais novo por `--no-renames`; assim, apagar ou renomear um caminho da EXCECAO mantém esse lado A. (`bin/selo_proporcao.py:151`, `bin/selo_proporcao.py:152`, `bin/selo_proporcao.py:113`)

3. **A precedência está correta.** Um caminho exatamente presente na EXCECAO retorna A antes de consultar LISTA, `templates`, `static` ou testes; não há passagem posterior para B nessa chamada. Outro caminho com basename `pwa.js` ou `sw.js` pode receber B, pois a exceção é nominal, não por basename. (`bin/selo_proporcao.py:113`, `bin/selo_proporcao.py:115`, `bin/selo_proporcao.py:121`, `bin/selo_proporcao.py:123`, `bin/selo_proporcao.py:125`)

4. **Os dois blocos vêm da BASE.** Ausência ou duplicação de marcador, ordem invertida, item vazio/mal delimitado e curinga proibido levantam `NaoJulgado`; o Python e o wrapper entregam rc 2. Uma BASE anterior sem a EXCECAO também recebe rc 2: o HEAD não se autoriza sozinho. (`bin/selo_proporcao.py:74`, `bin/selo_proporcao.py:77`, `bin/selo_proporcao.py:79`, `bin/selo_proporcao.py:86`, `bin/selo_proporcao.py:90`, `bin/selo_proporcao.py:93`, `bin/selo_proporcao.py:150`, `bin/selo_proporcao.py:161`, `bin/selo_proporcao.sh:11`)

   **Não é um parser Markdown:** aceita um bloco completo dentro de crases/fence; linhas vazias ao redor dos itens são removidas por `strip()`; itens repetidos são deduplicados pelo conjunto. Uma linha vazia não equivale a item vazio entre `;`, que é rejeitado. Esses formatos aceitos não estão explicitamente proibidos pelo contrato fornecido. (`bin/selo_proporcao.py:74`, `bin/selo_proporcao.py:77`, `bin/selo_proporcao.py:79`, `bin/selo_proporcao.py:81`; `v10_contrato.txt`, restrição adicional: `[nao verificado]`)

5. **O harness morde os comportamentos principais, mas nem todo caso depende do braço novo.**

   - Docs positivos dependem do curinga; o PWA simultaneamente listado prova EXCECAO antes da LISTA; SW, apagado e rename exercitam a exceção antes de `static`. (`bin/tests/test_selo_proporcao.sh:27`, `bin/tests/test_selo_proporcao.sh:87`, `bin/tests/test_selo_proporcao.sh:88`, `bin/tests/test_selo_proporcao.sh:94`, `bin/tests/test_selo_proporcao.sh:95`, `bin/tests/test_selo_proporcao.sh:97`, `bin/tests/test_selo_proporcao.sh:98`)
   - `pwa_outra_pasta` passa sem EXCECAO. `pwa_declarada_A` também: verifica somente rc 0, que ocorre com declaração A tanto para classe real A quanto B. `pwa_renomeado` renomeia, de fato, **sw.js**. (`bin/tests/test_selo_proporcao.sh:49`, `bin/tests/test_selo_proporcao.sh:98`, `bin/tests/test_selo_proporcao.sh:99`, `bin/tests/test_selo_proporcao.sh:100`, `bin/selo_proporcao.py:171`, `bin/selo_proporcao.py:174`)
   - Os novos casos inválidos conferem rc **e motivo**; faltam, porém, `?` na pasta, padrões válidos mais gerais e adulteração da EXCECAO apenas no HEAD/disco. Um mutante que lesse somente a EXCECAO do HEAD poderia sobreviver — inferência da montagem dos casos. (`bin/tests/test_selo_proporcao.sh:51`, `bin/tests/test_selo_proporcao.sh:58`, `bin/tests/test_selo_proporcao.sh:59`, `bin/tests/test_selo_proporcao.sh:109`, `bin/tests/test_selo_proporcao.sh:134`, `bin/tests/test_selo_proporcao.sh:141`, `bin/tests/test_selo_proporcao.sh:162`)

   O harness integral não foi executado porque cria e remove arquivos; os resultados declarados de 85 casos, 32 RED e 53/53 mutantes permanecem `[nao verificado]`. (`bin/tests/test_selo_proporcao.sh:14`, `bin/tests/test_selo_proporcao.sh:22`, `bin/tests/test_selo_proporcao.sh:32`)

6. **QUEM ESCREVE / QUEM LÊ:**

   | Valor | Escritor → leitor |
   |---|---|
   | Blocos reais | Conteúdo em `CORTES.md:444–445` → `regras`, via `git show BASE:CORTES`. (`bin/selo_proporcao.py:86`, `bin/selo_proporcao.py:87`) |
   | Arquivo CORTES.md | Gerador lê JSON, monta Markdown e reescreve o arquivo inteiro → selo lê a versão BASE. (`bin/gerar_cortes.py:23`, `bin/gerar_cortes.py:32`, `bin/gerar_cortes.py:107`, `bin/gerar_cortes.py:116`, `bin/selo_proporcao.py:86`) |
   | Blocos sintéticos | `lista()` escreve; `base()` fornece os itens → selo invocado por `caso`/`caso_lista`. (`bin/tests/test_selo_proporcao.sh:21`, `bin/tests/test_selo_proporcao.sh:27`, `bin/tests/test_selo_proporcao.sh:48`, `bin/tests/test_selo_proporcao.sh:58`) |
   | `LISTA`, `EXCECAO` | Definidas no módulo → `regras` passa ambas a `bloco`. (`bin/selo_proporcao.py:47`, `bin/selo_proporcao.py:48`, `bin/selo_proporcao.py:87`) |
   | `CURINGA` | Definida no módulo → validações da LISTA e EXCECAO. (`bin/selo_proporcao.py:49`, `bin/selo_proporcao.py:90`, `bin/selo_proporcao.py:93`) |
   | `bloco` | Produz conjuntos de itens → `regras` valida e devolve o par. (`bin/selo_proporcao.py:81`, `bin/selo_proporcao.py:87`, `bin/selo_proporcao.py:95`) |
   | `regras` | Produz `(lista, excecao)` → `main` passa ambos a `motivo_b`. (`bin/selo_proporcao.py:150`, `bin/selo_proporcao.py:164`) |
   | `na_lista` | Produz booleano → `motivo_b` decide B pela LISTA. (`bin/selo_proporcao.py:104`, `bin/selo_proporcao.py:107`, `bin/selo_proporcao.py:115`) |
   | `motivo_b` | Produz motivo B ou `None` → `main` acumula caminhos A, calcula classe e rc. (`bin/selo_proporcao.py:164`, `bin/selo_proporcao.py:166`, `bin/selo_proporcao.py:168`, `bin/selo_proporcao.py:171`) |

   O gerador reescreve o Markdown sem preservar apêndices manuais; portanto a fragilidade declarada de manter esses blocos num arquivo gerado continua existindo. (`bin/gerar_cortes.py:97`, `bin/gerar_cortes.py:116`)

## 11/11 16:39 · sessao 8d5ba24aaeb4 · - · - · origem=Ronald (tmux)

Eu não usaria a divergência `static/js/pwa.js` como bloqueador único: o próprio usuário define explicitamente `app/static/pwa.js` no contexto da revisão, e esse caminho está na EXCECAO (`app/docs/CORTES.md:445`). O outro caminho é deliberadamente B no harness (`bin/tests/test_selo_proporcao.sh:99`). Vale registrar a inconsistência literal do anexo, sem supor que o usuário quis ampliar a exceção.

Também não considero automaticamente tortos blocos com linhas vazias ou itens repetidos: o parser aplica `strip()` e retorna conjunto (`bin/selo_proporcao.py:77–81`); o contrato fornecido não proíbe esses formatos. Marcadores em crase/fence são reconhecidos porque a leitura é textual (`bin/selo_proporcao.py:74–76`), mas não há requisito explícito de interpretar Markdown.

Minha recomendação, com esses elementos: **APROVADO**, sem bloqueador funcional demonstrado; registrar as lacunas do harness e a discrepância textual. Uma reprovação baseada somente nesses formatos acrescentaria requisitos que o contrato não declarou.

## 11/11 16:39 · sessao 07260de7f099 · - · - · origem=Ronald (tmux)

A cobertura nova é útil, mas não demonstra que cada caso depende do braço novo:

- `doc_md` e `doc_md_cortes` deixam de passar se o matcher curinga for desligado; os negativos cobrem subpasta, extensão, caixa e pasta diferente. A fixture contém apenas `app/docs/*.md`, portanto não exercita padrões com prefixo ou vários `*` (`bin/tests/test_selo_proporcao.sh:27`, `:87–93`; implementação `bin/selo_proporcao.py:98–107`).
- `pwa_excecao_e_lista` prova EXCECAO antes da LISTA porque a fixture também lista o PWA; `sw_excecao`, mistura com B, apagado e rename exercitam exceção antes de static (`bin/tests/test_selo_proporcao.sh:27`, `:94–98`; `bin/selo_proporcao.py:113–124`).
- `pwa_outra_pasta` é controle nominal: passa mesmo removendo a exceção. `pwa_declarada_A` também passa sem exceção, pois só verifica rc 0 e uma declaração A recebe rc 0 tanto para classificação real A quanto B (`bin/tests/test_selo_proporcao.sh:49`, `:99–100`; `bin/selo_proporcao.py:168–174`).
- O caso chamado `pwa_renomeado` realmente renomeia `sw.js`, não `pwa.js` (`bin/tests/test_selo_proporcao.sh:98`).
- Os novos casos de bloco usam LISTA válida e verificam rc 2 **e motivo**, evitando que ausência de outro bloco esconda a guarda pretendida (`bin/tests/test_selo_proporcao.sh:51–59`, `:134–144`).
- Falta `?` na pasta: há `*` na pasta, `[` na pasta, e `?` no nome. Remover apenas a proibição de `?` na pasta não é coberto por esses casos (`bin/tests/test_selo_proporcao.sh:141–144`; guarda `bin/selo_proporcao.py:90`).
- Não há caso independente de EXCECAO alterada só no HEAD/disco: os testes correspondentes adulteram apenas LISTA; os testes de bloco alteram BASE e o HEAD herda esse conteúdo. Ler somente a EXCECAO do HEAD pode sobreviver por essa ausência — inferência estática (`bin/tests/test_selo_proporcao.sh:51–58`, `:109–110`, `:162–166`).
- A exceção da fixture só contém caminhos static, embora a função aceite uma exceção alternativa; não há exercício nominal sob templates/tests (`bin/tests/test_selo_proporcao.sh:18–28`; precedência implementada em `bin/selo_proporcao.py:113–125`).

Não executei o harness porque ele cria e remove arquivos (`bin/tests/test_selo_proporcao.sh:14–32`). A execução alegada de 53/53 mutantes e 32 RED permanece `[nao verificado]`; os arquivos de resultados não constam da cópia disponível `[nao verificado]`.

## 11/11 16:38 · sessao be19b9bab978 · - · - · origem=Ronald (tmux)

Na cópia, o SW tem fonte nominal correta na EXCECAO; o caminho efetivamente aberto depende do runtime. (`app/docs/CORTES.md:445`; `app/core/views.py:528-530`.)

- As duas cascas carregam `pwa.js`, que registra `/sw.js` com escopo `/`. (`app/templates/base.html:20`; `app/templates/base_app.html:20`; `app/static/pwa.js:14`.)
- A URL raiz inclui `core.urls`; `/sw.js` chama `views.service_worker`. (`app/config/urls.py:39`; `app/core/urls.py:24`.)
- A view não renderiza template: abre `sw.js`, substitui `__BUILD_ID__` e responde JavaScript. (`app/core/views.py:570-573`.)
- `_raiz_do_static()` prefere `/app/staticfiles/sw.js` quando existe; caso contrário usa `/app/static/sw.js`. A fonte versionada é `app/static/sw.js`, incluída nominalmente em `CORTES.md`. Conteúdo e ramo efetivamente servido em produção: `[nao verificado]`. (`app/core/views.py:528-530`; `app/docs/CORTES.md:445`.)
- A configuração registra `BASE_DIR/static` como fonte de static e `BASE_DIR/staticfiles` como destino coletado; `staticfiles/` é ignorado pelo Git. (`app/config/settings/base.py:98-99`; `.gitignore:13`.)
- Mesmo um caminho de diff literal `app/staticfiles/sw.js` seria A pelo restante das regras: não tem componente `static`, não está na LISTA e cai no retorno final. Portanto, a diferença entre caminho fonte e coletado não demonstra B indevido. (`bin/selo_proporcao.py:98-107`; `bin/selo_proporcao.py:110-127`.)
- O código confirma escrita offline no PWA: `registrarBatidaOffline` abre transação de escrita e adiciona batida; sincronização remove itens enviados. O SW delega sincronização à página por mensagem, ou pede ao usuário para abrir o app. (`app/static/pwa.js:107-113`; `app/static/pwa.js:161-172`; `app/static/sw.js:112-127`.)

Escritor estrutural de `CORTES.md`: `bin/gerar_cortes.py` lê `app/docs/CORTES.json`, transforma cada `frase` em linha Markdown e reescreve o destino inteiro; a execução normal chama esse escritor. (`bin/gerar_cortes.py:23-24`; `bin/gerar_cortes.py:31-32`; `bin/gerar_cortes.py:106-109`; `bin/gerar_cortes.py:114-116`; `bin/gerar_cortes.py:142-152`.)

Os blocos atuais estão em `CORTES.md:444-445`, mas não há correspondentes PROPORCAO/LISTA-FECHADA/EXCECAO em `CORTES.json` na cópia `[nao verificado: ausência sem linha positiva única]`; o gerador não preserva apêndices manuais, então regeneração apagaria esses blocos. Esse é o limite (c) já declarado no anexo, não um achado fora do contrato. (`bin/gerar_cortes.py:97-116`; `app/docs/CORTES.md:3`; anexo `v10_contrato.txt`, item 4(c), sem arquivo local: `[nao verificado]`.)

## 11/11 16:05 · sessao 91788de74858 · PROPORCAO-POR-RISCO · verificador · origem=Code

APROVADO

Bloqueadores: nenhum novo demonstrado na cópia. O bloqueante 1 da v8 está curado pela guarda em `bin/selo_proporcao.py:80-81`.

1. **Cura e precedência da LISTA**

   `templatetags` agora é procurado em **todos os componentes de diretório**, antes dos braços `templates`, `static` e `tests`. Portanto, os dois arquivos do contraexemplo da v8 recebem `None`, tornam a obra A e produzem rc 1 quando declarada B (`bin/selo_proporcao.py:74-87`, `bin/selo_proporcao.py:123-135`).

   **Sim, `app/chamados/templatetags/chamados_tags.py` continua B pela LISTA**: consta em `app/docs/CORTES.md:444`, e a consulta retorna `'lista'` antes da guarda nova (`bin/selo_proporcao.py:76-80`). A execução usa a lista da **BASE**; sua presença naquela revisão histórica é [nao verificado], pois a cópia disponibilizada não permite resolver as revisões Git.

2. **Outras descobertas por pasta**

   Não encontrei outro contraexemplo concreto de descoberta automática na cópia [nao verificado quanto à ausência global e à execução em produção].

   - **Admin/apps:** a configuração padrão aponta para `saas`, que declara aplicativos de topo; não declara aplicativos sob `templates/static/tests` (`app/config/wsgi.py:14`, `app/config/settings/saas.py:22-53`). Caminhos como `app/core/admin.py` e `app/core/apps.py` recebem A pelas regras finais do classificador (`bin/selo_proporcao.py:82-88`).
   - **Signals/inicialização:** os imports encontrados são explícitos, por exemplo `core.signals`, `core.cell` e `chamados.signals`; esses caminhos também recebem A (`app/core/apps.py:8-10`, `app/chamados/apps.py:8-10`, `bin/selo_proporcao.py:82-88`).
   - **Checks, locale e fixtures:** internacionalização está ligada (`app/config/settings/base.py:92-95`). Não encontrei configuração ou caminho concreto que demonstre outra entrada automática classificada B; ausência global e comportamento interno desses carregadores nesta instalação: [nao verificado].
   - **Templatetags em outro nível:** a guarda usa pertinência em `dirs`, sem restringir profundidade; qualquer componente `templatetags` é barrado, salvo LISTA (`bin/selo_proporcao.py:74-81`).
   - **Limite anterior permanece:** o comando importa `core.tests.test_contract_tblib_no_paralelo`, que continua B pelo braço de teste. É o limite já declarado, com import explícito, não uma nova descoberta por pasta (`app/chamados/management/commands/tblib_ausente_na_imagem.py:17-18`, `bin/selo_proporcao.py:29-30`, `bin/selo_proporcao.py:86-87`).

3. **Os casos novos mordem o braço**

   Sim. Nenhum está na lista sintética, fora de `app/`, ou sob `migrations/management`; ambos dependem exclusivamente da condição nova (`bin/tests/test_selo_proporcao.sh:24`, `bin/tests/test_selo_proporcao.sh:70-71`, `bin/selo_proporcao.py:76-85`).

   | Caso | v9 | Removendo somente o braço novo |
   |---|---|---|
   | `tags_static_py` | Arquivos A; rc 1 | Arquivos B por `static`; rc 0 |
   | `tags_templates_py` | Arquivo A; rc 1 | Arquivo B por `templates`; rc 0 |

   Esses resultados seguem de `bin/selo_proporcao.py:80-85` e `bin/selo_proporcao.py:127-135`. Logo, **os testes passam na v9 e falham no mutante**, porque esperam rc 1 (`bin/tests/test_selo_proporcao.sh:44-45`, `bin/tests/test_selo_proporcao.sh:70-71`).

   Ressalva: `tags_templates_py` não cria `templates/__init__.py`; comprova o predicado de caminho, mas sua importação recursiva efetiva pelo Django é [nao verificado] (`bin/tests/test_selo_proporcao.sh:71`).

4. **Quem escreve e quem lê**

   | Valor | Quem produz/escreve | Quem lê |
   |---|---|---|
   | Caminho `p` | `git diff`, decodificação e laço: `bin/selo_proporcao.py:112-125` | `motivo_b`: `bin/selo_proporcao.py:73-74` |
   | `partes`, `dirs`, `nome` | Divisão/fatiamento: `bin/selo_proporcao.py:74-75` | Guarda nova e braços seguintes: `bin/selo_proporcao.py:80-87` |
   | LISTA real | Conteúdo atual: `app/docs/CORTES.md:444`; autor da inserção [nao verificado] | `git show BASE`, parser e consulta: `bin/selo_proporcao.py:62-76`, `bin/selo_proporcao.py:111` |
   | Arquivo `CORTES.md` | Gerador lê JSON e sobrescreve Markdown: `bin/gerar_cortes.py:23-24`, `bin/gerar_cortes.py:32`, `bin/gerar_cortes.py:116` | Selo lê versão BASE: `bin/selo_proporcao.py:62` |
   | LISTA sintética | Função `lista`, argumento e commit: `bin/tests/test_selo_proporcao.sh:19-28` | Mesmo parser: `bin/selo_proporcao.py:62-70` |
   | Arquivos/conteúdos novos | Scripts dos casos, executados e commitados pelo preparo: `bin/tests/test_selo_proporcao.sh:36-38`, `bin/tests/test_selo_proporcao.sh:70-71` | Selo lê **nomes**; conteúdo não entra no predicado: `bin/selo_proporcao.py:112`, `bin/selo_proporcao.py:73-88` |
   | Motivo/`None`, `fora`, classe e rc | Classificador e agregação: `bin/selo_proporcao.py:76-88`, `bin/selo_proporcao.py:123-135` | Protocolo e wrapper: `bin/selo_proporcao.py:145`, `bin/selo_proporcao.sh:7-11` |
   | Esperado `B 1`, rc observado e falhas | Casos, captura e contador: `bin/tests/test_selo_proporcao.sh:33`, `bin/tests/test_selo_proporcao.sh:44`, `bin/tests/test_selo_proporcao.sh:70-71` | Comparação e resultado final: `bin/tests/test_selo_proporcao.sh:45`, `bin/tests/test_selo_proporcao.sh:203-204` |

   O harness completo não foi executado: cria arquivos e repositório temporário, contrariando a restrição de somente leitura (`bin/tests/test_selo_proporcao.sh:13-28`). Execução integral e identidade Git do HEAD informado: [nao verificado].

## 11/11 16:04 · sessao bb6c2e8c42d3 · - · - · origem=Ronald (tmux)

Os dois casos isolam o braço novo; nenhum recebe rc 1 por outra guarda: pertencem a `app/`, não contêm `migrations`/`management` e não estão na lista sintética (`bin/tests/test_selo_proporcao.sh:24`, `:70-71`; `bin/selo_proporcao.py:76-85`).

Verificação somente em memória da função `motivo_b`, removendo exclusivamente `or 'templatetags' in dirs`:

| Arquivo sintético | v9 | Sem braço |
|---|---|---|
| `app/x/templatetags/static/__init__.py` | `None` | `static` |
| `app/x/templatetags/static/carga.py` | `None` | `static` |
| `app/x/templatetags/templates/carga.py` | `None` | `templates` |

Os resultados decorrem de `bin/selo_proporcao.py:80-85`. Com v9, ambos os **testes** passam porque o selo retorna 1, esperado pelo harness; sem o braço, os **arquivos** passam como B/rc 0 e os testes falham (`bin/tests/test_selo_proporcao.sh:44-45`, `:70-71`; `bin/selo_proporcao.py:127-135`).

`tags_templates_py` não cria `templates/__init__.py`, ao contrário do caso static; portanto é caso do predicado de caminho, sem demonstração suficiente de importação recursiva efetiva pelo Django (`bin/tests/test_selo_proporcao.sh:70-71`). Importação efetiva desse exemplo: [nao verificado].

A lista mantém precedência: `app/chamados/templatetags/chamados_tags.py` aparece em `app/docs/CORTES.md:444`; `motivo_b` devolve `'lista'` antes da guarda de templatetags (`bin/selo_proporcao.py:76-80`). A lista usada numa execução real é a revisão BASE, não necessariamente o arquivo atual (`bin/selo_proporcao.py:62`, `:111`).

Cadeia de escritores e leitores:

| Valor | Quem escreve/produz | Quem lê |
|---|---|---|
| Caminhos alterados `p` | Git diff, `bin/selo_proporcao.py:112`; decodificação `:117` | Laço `:124-125`; `motivo_b` `:74` |
| `partes`, `dirs`, `nome` | Split/fatiamento, `bin/selo_proporcao.py:74-75` | Guarda nova `:80`; regras `:82-87` |
| Lista real | Conteúdo em `app/docs/CORTES.md:444`; autor da inserção [nao verificado] | `git show BASE`, `bin/selo_proporcao.py:62`; parser `:63-70`; consulta `:76` |
| Arquivo `CORTES.md` | Gerador sobrescreve em `bin/gerar_cortes.py:116`, fonte JSON `:23`, lida em `:32` | Selo lê versão BASE em `bin/selo_proporcao.py:62` |
| Lista sintética | `bin/tests/test_selo_proporcao.sh:19`, argumento `:24`, commit `:28` | Mesmo parser do selo, `bin/selo_proporcao.py:62-70` |
| Arquivos e bytes dos novos casos | Scripts `bin/tests/test_selo_proporcao.sh:70-71`, executados por `:36`, versionados `:37-38` | Selo lê somente nomes, `bin/selo_proporcao.py:112`; conteúdo não entra no predicado `:73-88` |
| `m`/`None` | `bin/selo_proporcao.py:77-88` | Impressão/acumulação `:125-128` |
| `fora`, `real`, rc | `bin/selo_proporcao.py:123-135` | Protocolo `:145`; wrapper `bin/selo_proporcao.sh:7-11`; harness `bin/tests/test_selo_proporcao.sh:44-45` |
| Esperado `B 1` e nomes dos casos | `bin/tests/test_selo_proporcao.sh:70-71` | Argumentos do `caso` `:41-45` |
| Contador de falhas | `bin/tests/test_selo_proporcao.sh:33` | Resultado final `:203-204` |

O harness completo não foi executado: cria diretório temporário, repositório e arquivos (`bin/tests/test_selo_proporcao.sh:13-28`), incompatíveis com a exigência de somente leitura.

## 11/11 16:04 · sessao bf5e6ffaae43 · - · - · origem=Ronald (tmux)

Não encontrei outro bloqueante concreto de descoberta automática no material examinado [nao verificado].

- A configuração padrão aponta para `config.settings.saas`; ela herda `base` e monta os aplicativos de nomes de topo, sem aplicativos declarados sob `templates/static/tests`: `app/config/wsgi.py:14`, `app/manage.py:9`, `app/config/settings/saas.py:10`, `app/config/settings/saas.py:22`, `app/config/settings/saas.py:34`, `app/config/settings/saas.py:53`.
- Os imports de inicialização encontrados são explícitos: `signals`, `cell`, `painel_cell`, `calculadores`; seus caminhos não recebem B pelas regras automáticas: `app/core/apps.py:8`, `app/chamados/apps.py:8`, `app/colaboradores/apps.py:7`, `app/ponto/apps.py:7`, `app/escala/apps.py:8`, `app/inteligencia/apps.py:7`, `bin/selo_proporcao.py:82`.
- Templates e estáticos têm descoberta configurada, já contemplada pela letra do contrato: `app/config/settings/base.py:70`, `app/config/settings/base.py:97`; internacionalização está ligada: `app/config/settings/base.py:92`.
- O import de código B de teste por comando permanece o limite declarado de tblib: escritor da chamada em `app/chamados/management/commands/tblib_ausente_na_imagem.py:17`, uso do valor em `app/chamados/management/commands/tblib_ausente_na_imagem.py:18`.
- `crons` importa `core.tests.marca_cron15`, cujo nome não satisfaz `test_*.py`: `app/core/management/commands/crons.py:92`, `bin/selo_proporcao.py:86`.
- Não apareceram `.py` sob `app/**/templates/` ou `app/**/static/`, nem caminhos `fixtures/` ou `locale/` no inventário pesquisado; ausência global e configuração efetivamente usada em produção: [nao verificado].
- Não há código vendorizado do Django para confirmar nesta cópia a semântica interna de autodiscover de admin, apps, checks, locale ou fixtures [nao verificado].

_3993 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
