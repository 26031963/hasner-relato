# CODEX-ESTADO

> Ultima resposta processada: **11/11/2026 12:14** · publicado em **11/11/2026 12:15** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 11/11 12:14 · sessao a92c83d22358 · - · - · origem=Ronald (tmux)

Achados procedentes no código; render testado em memória com `markdown_it` CommonMark + extensão table, renderer de produção [nao verificado].

| Variante sobre mapa válido | Validador | Render local / decisão |
|---|---|---|
| `&#124;` ou `&vert;` em célula | Passa | Mostra pipe literal na mesma célula; não cria coluna. `_celulas` divide só `|` literal: `bin/mapa20_valida.py:92`. |
| Segunda “tabela” usando apenas entidades como delimitadores | Passa | Renderiza parágrafo com pipes, sem segunda tabela; não é evasão estrutural observada. `bin/mapa20_valida.py:137`. |
| Segunda `<table><tr><td>E3</td><td>encerrada</td>…</tr></table>` após branca | **Passa** | Renderiza segunda tabela HTML. Guarda HTML só rejeita comentário, `bin/mapa20_valida.py:126`; coleta só linhas começadas por pipe, `:130`; julga encerrada só nas células coletadas, `:193`. **Alta/bloqueante.** |
| `enc&#101;rrada` em célula com 1 de 2 | **Passa** | Mostra `encerrada`; regex avalia bytes sem decodificar entidades: `bin/mapa20_valida.py:56`, `:67`, `:193`. **Alta/bloqueante.** |
| `<em>encerrada</em>` | Recusa | Palavra literal ainda casa: `bin/mapa20_valida.py:56`, `:193`. |
| `enc<em>errada</em>` | **Passa** | Leitor vê palavra encerrada com trecho em itálico; regex não remove tags: `bin/mapa20_valida.py:56`, `:67`. **Alta/bloqueante.** |
| `encerrada<br>` | Recusa | Palavra literal presente: `bin/mapa20_valida.py:56`, `:193`. |
| `encer\u200brada` | Passa | Mantém caractere invisível na saída; aparência efetiva depende de fonte/layout [nao verificado]. Regex exige palavra contígua: `bin/mapa20_valida.py:56`. |
| Branca com espaços/tab após tabela | Passa | `.strip()` considera branca e renderer encerra tabela: `bin/mapa20_valida.py:135`. |
| CRLF | Passa | `splitlines()` e leitura textual normalizam fronteiras: `bin/mapa20_valida.py:123`, `:209`. |
| `a\|b` dentro da tabela | Recusa | Proibição de qualquer barra invertida: `bin/mapa20_valida.py:145`. |
| Segunda tabela em lista, com pipes | Recusa | Pipe fora do bloco: `bin/mapa20_valida.py:137`; linhas canônicas indentadas caem antes em `:128`. |
| Segunda tabela com todas as linhas prefixadas por `>` | Recusa | Pipes fora: `bin/mapa20_valida.py:137`. |
| `> quote\n` imediatamente antes do cabeçalho canônico | **Passa** | **ZERO tabelas** no render local: todas as linhas canônicas viram continuação lazy do parágrafo do blockquote. Só há guarda de fronteira depois, `bin/mapa20_valida.py:135`; nenhuma antes, `:130`. **Média, forma fixa não assegurada.** |
| `- item\n` ou `1. item\n` imediatamente antes do cabeçalho | **Passa** | **ZERO tabelas**, continuação lazy da lista; mesmo defeito de fronteira anterior, `bin/mapa20_valida.py:130`, `:135`. **Média.** |
| Lista/blockquote seguidos de branca e tabela canônica em coluna zero | Passa | Uma tabela normal, sem evasão observada: `bin/mapa20_valida.py:130`. |
| Heading setext `titulo\n---\n` antes da tabela | Passa | Heading normal e uma tabela; não é evasão observada: `bin/mapa20_valida.py:130`. |
| `===` ou `---` imediatamente depois da tabela | Recusa | Qualquer linha seguinte não branca, embora `---` renderize como `<hr>` fora da tabela: `bin/mapa20_valida.py:135`. |
| Heading setext `encerrada\n---` separado por branca | Passa | Palavra fora da tabela fica sem julgamento; `bin/mapa20_valida.py:159`, `:193` só inspecionam bloco. |
| `<table>\n` antes da tabela e `\n\n</table>` depois | **Passa** | Zero tokens de tabela Markdown; pipes são HTML bruto. Também ocorre com `<pre>`, `<div>` e `<em>` em linha própria. Comentários são o único HTML vedado, `bin/mapa20_valida.py:126`; branca antes do fechamento satisfaz `:135`. **Média/alta conforme contrato de visibilidade.** |

Adicional: substituir todos os LF por VT, FF, NEL, U+2028 ou U+2029 mantém aprovação porque `splitlines()` os separa (`bin/mapa20_valida.py:123`), mas renderer local mostra zero tabelas. Interpretação do renderer de produção [nao verificado].

## 11/11 12:14 · sessao caef848ee75c · - · - · origem=Ronald (tmux)

Achados semânticos, todos reproduzidos em memória sem criar arquivos:

- **Procedente, alta:** a cura não fecha a classe de “encerrada” renderizada. Com `1 de 2`, `encerrad&#97;`, `encerr<em>ada</em>`, `encerrad\u200ba` e `encerrad\u200da` retornam zero problemas. `_ENCERRADA` procura caracteres contíguos; `_sem_nomes_e_caminhos` apenas remove nomes/caminhos, sem interpretar HTML ou caracteres invisíveis. Referências: `bin/mapa20_valida.py:56`, `bin/mapa20_valida.py:59`, `bin/mapa20_valida.py:67`, `bin/mapa20_valida.py:193`. A aparência exata dos caracteres invisíveis no publicador fica [nao verificado].

- **Procedente, média:** a cura introduz recusas de negação legítima com ênfase por `_`: `nao _encerrada_`, `_nao encerrada_` e `__nao encerrada__` retornam `"encerrada" com 1 de 2`. A nova fronteira encontra a palavra, mas `_NEGADA` exige `\b` antes de `nao` e somente espaços depois; os sublinhados impedem a isenção. Referências: `bin/mapa20_valida.py:52`, `bin/mapa20_valida.py:56`, `bin/mapa20_valida.py:68`, `bin/mapa20_valida.py:194`.

- **Procedente, média, preexistente:** `nao **encerrada**`, `nao <em>encerrada</em>` e `n&#227;o encerrada` também recusam. `nao encerrada`, `não encerrada` e `**nao encerrada**` passam. Referências: `bin/mapa20_valida.py:52`, `bin/mapa20_valida.py:68`.

- A remoção de nomes/caminhos continua preservando os casos intencionais `BO-ROTULO-ENCERRADO` e `logs/encerrada.md:10`. Referências: `bin/mapa20_valida.py:51`, `bin/mapa20_valida.py:61`, `bin/tests/test_mapa20_valida.sh:93`, `bin/tests/test_mapa20_valida.sh:95`.

- A remoção ocorre sem fronteiras, podendo fabricar contexto: `nao E3 encerrada` e `nao logs/estado.md encerrada` tornam-se `nao   encerrada` e passam; `encerradaE3` torna-se `encerrada ` e recusa. Se essas entradas representam uma afirmação ou texto legítimo na produção é [nao verificado]. Referências: `bin/mapa20_valida.py:61`, `bin/mapa20_valida.py:62`, `bin/mapa20_valida.py:63`, `bin/mapa20_valida.py:68`.

**Os quatro testes novos mordem pelo motivo certo; achado de teste inadequado não procedente.** Recriei o validador anterior exclusivamente em memória, removendo os dois novos bloqueios e restaurando `\bencerrad[ao]s?\b`: os quatro retornam zero problemas; no atual: [nao verificado]

| Caso | Resultado atual | Decisão |
|---|---|---|
| Linha sem borda colada | Recusa por linha colada | `bin/tests/test_mapa20_valida.sh:62`; `bin/mapa20_valida.py:135` |
| Segunda tabela sem bordas | Recusa por pipe fora | `bin/tests/test_mapa20_valida.sh:63`; `bin/mapa20_valida.py:137` |
| `_encerrada_` | Recusa por encerramento com `1 de 2` | `bin/tests/test_mapa20_valida.sh:65`; `bin/mapa20_valida.py:193` |
| `__encerrada__` | Recusa por encerramento com `1 de 2` | `bin/tests/test_mapa20_valida.sh:66`; `bin/mapa20_valida.py:193` |

A primeira entrada também cairia no bloqueio de pipe externo se somente o bloqueio de linha colada fosse removido; ela contém pipes internos. Referências: `bin/tests/test_mapa20_valida.sh:62`, `bin/mapa20_valida.py:137`.

## 11/11 12:13 · sessao 88846b740303 · - · - · origem=Ronald (tmux)

Compatibilidade da cura: **achado de regressão no formato fabricado pelo selo não procedente**. `mapa()` monta tabela com pipes nas bordas, prosa sem pipes fora dela e linha vazia depois dela; portanto não viola novas regras. `bin/tests/test_mapa20_valida.sh:25`, `:31`, `:32`, `:33`; decisões em `bin/mapa20_valida.py:135` e `:137`. O fixture adicional do publicador termina a tabela em EOF, também permitido, pois a condição exige existir linha seguinte. `bin/tests/test_mapa20_valida.sh:128`; `bin/mapa20_valida.py:135`.

No publicador, **regressão do fluxo não procedente**: o script copia `logs/MAPA-20/mapa.md` para snapshot e julga essa cópia; rc zero seleciona o par para publicação; falha fixa `MAPA_RC=1`, segue com o resto e devolve esse rc, inclusive quando nada mudou. `bin/relato.sh:94`, `:97`, `:98`, `:100`, `:143`, `:158`, `:165`. O raspador só transforma o snapshot depois do selo e escreve o destino público. `bin/relato.sh:143`; `bin/raspar_publico.py:28`, `:31`, `:40`. Resultado efetivo após raspagem: [nao verificado].

Quem escreve:

- Escritor produtivo é **declarado** como sessão, usando `logs/MAPA-20/montar_mapa.py`; implementação não está comprovada na cópia. `bin/mapa20_valida.py:10`; [nao verificado]. `logs/` está ignorado. `.gitignore:53`.
- Gerador sintético `mapa()`: `bin/tests/test_mapa20_valida.sh:32`; grava texto de teste em `:37`.
- Fixture sintético do publicador: `bin/tests/test_mapa20_valida.sh:128`; cópia para o caminho imitado em `:170`; fixture vermelho em `:171`.
- Wrapper sintético que simula regravação concorrente: `bin/tests/test_mapa20_valida.sh:147`.
- Publicador escreve snapshot por `cp`: `bin/relato.sh:97`; escreve `MAPA-20.md` através de `bin/raspar_publico.py:40`, selecionado em `bin/relato.sh:98` e `:143`.

Quem lê rc/saída:

- **Produção:** `bin/relato.sh:97` lê rc do validador pelo `if`; não faz parsing de sua saída, que segue stdout. `bin/relato.sh:97`, `:101`.
- **Teste do validador:** `bin/tests/test_mapa20_valida.sh:38` captura saída e retorna somente rc; `:109` compara rc esperado; `:110` verifica arquivo ausente; `:115` devolve falha.
- **Wrapper sintético:** `bin/tests/test_mapa20_valida.sh:146` lê rc e `:148` o repassa.
- **Teste do publicador:** `bin/tests/test_mapa20_valida.sh:175` captura saída do fluxo; `:176` guarda rc; `:180` lê esse rc; `:184` compara; `:185` exibe saída se falhar.
- **Chamador declarado do publicador:** unit executa `relato.sh`. `bin/hasner-relato.service:15`. Essa unit não declara `OnFailure`; tratamento/alerta externo e instalação viva: [nao verificado].

Severidade: nenhuma regressão demonstrada no formato sintético ou no fluxo; compatibilidade com o escritor produtivo real permanece [nao verificado]. `bin/tests/test_mapa20_valida.sh:33`; `bin/relato.sh:97`; `bin/mapa20_valida.py:10`.

## 11/11 12:12 · sessao c4149a800b40 · TRIAGEM-OFFLINE-SEM-CAMERA · censo · origem=Code

**Pela lei do sistema, NÃO: falta de selfie não deve impedir a batida. A cópia contém respostas contraditórias:** `apto` online libera; `apto` offline esconde a confirmação; v2 recusa explicitamente sem foto. (`CLAUDE.md:143-144`; `app/templates/ponto/triagem/apto.html:156-163`; `app/templates/ponto/triagem/apto.html:253-256`; `app/templates/ponto/bater_ponto_v2.html:285-290`.)

Inspeção somente leitura; não executei testes, banco ou logs. Identidade Git independente do hash informado e ocorrência concreta em produção: **[nao verificado]**. O anexo fornecido relata batida perdida; quantidade, usuários atingidos e frequência: **[nao verificado]**. [nao verificado]

**1. Censo dos sítios**

As rotas atuais selecionam triagem ou v2; `bater_ponto.html` permanece no censo como código existente, mas não é o template selecionado nessa view. (`app/ponto/urls.py:11-12`; `app/ponto/views.py:225-235`; `app/ponto/views.py:356-358`.)

| Sítio | Caminho | Resposta | Como |
|---|---|---|---|
| `apto`: abertura de câmera | Online | **Libera na falha**, inclusive permissão negada | O `catch` informa revisão e mostra `bloco-botoes`. Não diferencia causas. (`app/templates/ponto/triagem/apto.html:149-163`.) |
| `apto`: foto não capturada | Antes da escolha online/offline | **Bloqueia pela apresentação inicial** | Botões começam em `hidden`; ficam acessíveis pela confirmação ou pelo `catch`. (`app/templates/ponto/triagem/apto.html:93`; `app/templates/ponto/triagem/apto.html:156-185`.) |
| `apto`: conversão da foto | Online | **Libera com blob nulo**, se o callback ocorrer | Atribui o resultado sem validá-lo e mostra os botões. Não há proteção local para falha de desenho/conversão nem timeout do callback. (`app/templates/ponto/triagem/apto.html:167-186`.) |
| `apto`: refazer | Ambos, antes do envio | **Volta a esconder** | Zera foto, esconde os botões e tenta câmera novamente; uma rejeição volta ao `catch` permissivo. (`app/templates/ponto/triagem/apto.html:189-195`.) |
| `apto`: envio | Online | **Libera sem foto** | Anexa somente se `_fotoBlob` existir e submete o formulário. (`app/templates/ponto/triagem/apto.html:228-235`.) |
| `apto`: segunda câmera | Offline | **Bloqueia na rejeição** | Depois de desabilitar o botão principal, abre câmera offline; o `catch` esconde `btn-capturar-off`, pede supervisor e zera stream. (`app/templates/ponto/triagem/apto.html:210-212`; `app/templates/ponto/triagem/apto.html:236-256`.) |
| `apto`: confirmar sem stream | Offline | **Não tem veto por stream nulo** | Continua para `canvas.toBlob` e fila; o impedimento do clique está no ramo anterior. (`app/templates/ponto/triagem/apto.html:261-282`.) |
| `bater_ponto`: câmera inicial | Antes de online/offline | **Bloqueia implicitamente se continuar sem foto** | Botões nascem escondidos; o `catch` oferece câmera do sistema e telemetria, sem liberá-los. (`app/templates/ponto/bater_ponto.html:246`; `app/templates/ponto/bater_ponto.html:343-378`.) |
| `bater_ponto`: câmera do sistema | Ambos | **Bloqueia se cancelar** | Sem arquivo retorna; com arquivo libera somente no `FileReader.onload`. (`app/templates/ponto/bater_ponto.html:384-403`.) |
| `bater_ponto`: conversão | Ambos | **Libera em null, exceção ou timeout de `toBlob`** | Tenta dataURL e libera mesmo sem blob; salvaguarda de 1.500 ms. Entretanto, `drawImage`/`toDataURL` anteriores estão fora desse `try`. (`app/templates/ponto/bater_ponto.html:414-467`.) |
| `bater_ponto`: refazer | Ambos | **Volta a esconder** | Zera foto e reabre a câmera inicial. (`app/templates/ponto/bater_ponto.html:470-477`.) |
| `bater_ponto`: envio alcançado | Online/rede falha | **Libera sem foto** | Foto é condicional; falha de HTTP/rede enfileira `_fotoBlob`, inclusive nulo. (`app/templates/ponto/bater_ponto.html:519-543`.) |
| `bater_ponto`: segunda câmera | Offline | **Mantém confirmação disponível** | Mostra botão antes de pedir câmera; o `catch` apenas altera o texto. (`app/templates/ponto/bater_ponto.html:564-581`.) |
| `bater_ponto`: confirmação offline | Offline | **Sem veto explícito por foto/stream ausente** | Faz `toBlob` incondicional e passa o resultado à fila. Não protege o próprio `toBlob` com timeout ou `try`. (`app/templates/ponto/bater_ponto.html:584-610`.) |
| `bater_ponto`: confirmação de turno | Online | **Formulário sem requisito de selfie** | Envia confirmação/GPS sem campo de foto nessa etapa. (`app/templates/ponto/bater_ponto.html:174-196`.) |
| v2: câmera/fallback | V2 online/offline | **Fallback não libera sozinho** | Distingue `NotAllowedError`; oferece input do sistema. Seleção vazia retorna. (`app/templates/ponto/bater_ponto_v2.html:235-252`.) |
| v2: captura vazia/conversão | V2 online/offline | **Segue até a guarda de foto** | Sem vídeo pronto retorna foto anterior ou null; conversão tenta dataURL em null, exceção e timeout de 1.500 ms. (`app/templates/ponto/bater_ponto_v2.html:256-267`.) |
| v2: admissibilidade | **V2 online e offline** | **Bloqueia explicitamente** | Sem `_fotoBlob`, consulta permissão, mostra aviso e retorna false. O chamador retorna **antes** de escolher rede, POST ou fila. (`app/templates/ponto/bater_ponto_v2.html:285-290`; `app/templates/ponto/bater_ponto_v2.html:307-346`.) |
| Onboarding: etapa selfie | Configuração | **Etapa normal exige foto** | Falha oferece câmera do sistema; seleção vazia e `salvarFoto` sem blob retornam. Captura não trata null/timeout. (`app/templates/core/onboarding.html:152-200`; `app/templates/core/onboarding.html:212-213`.) |
| Onboarding: saída alternativa | Configuração/acesso | **Permite concluir sem selfie** | “Configurar depois” chama concluir; backend lista faltas, mas grava completo e responde `ok=True`. A view de bater ponto redireciona quem ainda não concluiu. (`app/templates/core/onboarding.html:120-125`; `app/core/views.py:479-489`; `app/ponto/views.py:243-244`.) |
| Enfileirador PWA | Offline/falha de envio | **Aceita sem foto** | Grava `fotoBlob || null`. (`app/static/pwa.js:107-122`.) |
| Sincronizador PWA | Retorno da rede | **Envia sem foto** | Só acrescenta arquivo quando presente. (`app/static/pwa.js:143-156`.) |
| Service worker | Offline/sync | **Não julga selfie** | Ignora não-GET; no sync delega às janelas ou notifica para abrir o app. (`app/static/sw.js:35-36`; `app/static/sw.js:112-129`.) |
| View web | Online triagem | **Aceita ausência de foto** | Registra primeiro; anexa somente quando existe foto. (`app/ponto/services/triagem_batida.py:700-713`; `app/ponto/services/triagem_batida.py:748-751`.) |
| API, duas cópias | V2/sync/app | **Aceita e sinaliza ausência** | Acrescenta `foto_ausente`; registro e anexação são separados. (`app/api/views.py:449-465`; `app/api/views_core.py:436-452`; `app/api/views_core.py:658-672`; `app/api/views_core.py:776-782`.) |
| Endpoints de anexação | Depois da batida | **Exigem foto para anexar** | Operam sobre batida existente; não são portas de criação da batida. (`app/api/views.py:847-859`; `app/api/views_core.py:832-844`; `app/ponto/views.py:2895-2905`.) |
| Endpoint offline antigo | Legado | **410, independentemente da selfie** | Foi aposentado em favor da API atual. (`app/ponto/views.py:419-428`.) |

Não há timeout próprio de **`getUserMedia`** nos ramos examinados; os 1.500 ms protegem conversão de imagem. Quando a promessa de câmera permanece pendente, não existe liberação alternativa temporizada nesses trechos; comportamento concreto no dispositivo: **[nao verificado]**. (`app/templates/ponto/triagem/apto.html:149-164`; `app/templates/ponto/triagem/apto.html:242-257`; `app/templates/ponto/bater_ponto.html:343-378`; `app/templates/ponto/bater_ponto_v2.html:235-267`; `app/templates/core/onboarding.html:152-162`.)

**2. `confirmarFotoOffline`, fila e servidor**

Com `_streamOffline === null`, `confirmarFotoOffline()` **não retorna por esse motivo**. O stream só condiciona parar as tracks. O desenho depende da visibilidade do vídeo; depois chama `canvas.toBlob` e enfileira seu resultado sem exigir foto. Também não reaproveita `_fotoBlob` da captura anterior. (`app/templates/ponto/triagem/apto.html:261-282`.)

Portanto, chamar essa função por outro meio pode alcançar a fila mesmo sem stream. Isso **não comprova selfie capturada**: o canvas pode não ter recebido frame nessa chamada; conteúdo efetivamente produzido pelo navegador: **[nao verificado]**. (`app/templates/ponto/triagem/apto.html:266-282`.)

A fila localizada é IndexedDB `juliani-offline`, store `batidas_pendentes`; aceita `foto:null`. Não foi localizada fila de batida em `localStorage` ou em `app/static/js`; implementação adicional: **[nao verificado]**. (`app/static/pwa.js:48-60`; `app/static/pwa.js:107-122`.)

O sync envia para `/api/ponto/bater/`, omitindo foto ausente. Sucesso remove a entrada; HTTP 400 também remove; rede/5xx preservam para retentativa. O SW não faz essa gravação diretamente. (`app/static/pwa.js:143-179`; `app/static/sw.js:112-129`.)

O chokepoint `registrar_batida(colaborador, timestamp, tipo, origem, ...)` **não exige foto nem permissão de câmera**; o modelo declara selfie opcional. Entretanto, isso não significa aceitação incondicional por outros motivos: há deduplicação e recusa por colisão de tipo oposto, além da guarda de competência para fato que não seja de chão. (`app/ponto/registro_batida.py:94-161`; `app/ponto/models.py:78`.)

**3. Fonte única e carregamento comum**

Não identifiquei autoridade consultada pelos fronts para **“selfie é impeditiva?”**. As decisões estão locais e contraditórias. Existe `selfie_ausente()`, mas sua pergunta é **cobrança da ausência**, por canal/prazo, e seus consumidores são backend; não é admissibilidade da captura. (`app/templates/ponto/triagem/apto.html:161-163`; `app/templates/ponto/triagem/apto.html:253-256`; `app/templates/ponto/bater_ponto_v2.html:285-290`; `app/ponto/registro_batida.py:64-91`; `app/core/juizes.py:1192-1195`.)

| Tela | Herança e scripts próprios |
|---|---|
| `apto` | Estende `base_triagem`, que estende `base.html`; acrescenta JS inline por `extra_js`. (`app/templates/ponto/triagem/apto.html:1`; `app/templates/ponto/triagem/base_triagem.html:1`; `app/templates/ponto/triagem/base_triagem.html:25-76`.) |
| `bater_ponto` | Estende `base.html`; lógica própria inline. (`app/templates/ponto/bater_ponto.html:1`; `app/templates/ponto/bater_ponto.html:285`.) |
| v2 | Estende `base.html`; acrescenta `/static/leaflet/leaflet.js` e lógica inline. (`app/templates/ponto/bater_ponto_v2.html:1`; `app/templates/ponto/bater_ponto_v2.html:135-137`.) |
| Onboarding | Documento independente, JS inline. (`app/templates/core/onboarding.html:1`; `app/templates/core/onboarding.html:115-330`.) |

Scripts herdados pelas três telas via `base.html`: `pwa.js`; `js/hasner-ui.js?v=hx3`; `hasner-table.js`; `hasner-filter.js?v=ta5`; `fita-mes.js?v=1`; `geo-colab-map.js`; `popover-acao.js`. Também carrega htmx externo. (`app/templates/base.html:19-20`; `app/templates/base.html:83-88`; `app/templates/base.html:440`.)

Includes da base: `_nudge_app_android` condicional, `includes/icons`, `_icone_pauta`, `_dialogo`, `_js_cobranca`, `_drawer_generico`, `_copiloto`; este também inclui `_icone_pauta`. (`app/templates/base.html:92-93`; `app/templates/base.html:290`; `app/templates/base.html:388-399`; `app/templates/base.html:432`; `app/templates/core/_copiloto.html:34`.)

Na outra casca, `base_app.html`, estão `pwa.js`, `hasner-ui`, `hasner-table`, `geo-colab-map`, `popover-acao` e htmx; includes `icons`, `_dialogo`, `_drawer_generico`. O teste de templates compõe apto/v2 com ambas as cascas. (`app/templates/base_app.html:19-20`; `app/templates/base_app.html:77-85`; `app/templates/base_app.html:145-154`; `app/templates/base_app.html:194`; `app/ponto/tests/test_l030_templates_renderizados.py:14-37`.)

**Inferência de localização:** o menor módulo existente ligado à batida e já carregado nas três telas é **`app/static/pwa.js`**, também presente nas duas cascas. É candidato para hospedar a resposta compartilhada sem acrescentar uma regra de domínio. (`app/templates/base.html:20`; `app/templates/base_app.html:20`; `app/static/pwa.js:305-318`.)

**4. Quem escreve e quem lê os sinais**

| Dado/sinal | Escritor | Leitor/efeito |
|---|---|---|
| Falha de câmera no `apto` | Escreve apenas texto/DOM; não envia causa específica. (`app/templates/ponto/triagem/apto.html:156-163`; `app/templates/ponto/triagem/apto.html:228-235`.) | Não há leitor de causa por batida nesse fluxo; a foto simplesmente pode faltar. (`app/ponto/services/triagem_batida.py:748-751`.) |
| Diagnóstico de câmera do `bater_ponto` | Envia erro, modo e user agent para `/api/diag/cam/`. (`app/templates/ponto/bater_ponto.html:367-375`.) | Endpoint escreve logger e responde `logged`; não associa sinal ao modelo da batida. Logs concretos: **[nao verificado]**. (`app/api/views.py:2647-2669`.) |
| Foto ausente na fila | `registrarBatidaOffline` grava `foto:null`; não grava motivo da ausência. (`app/static/pwa.js:113-119`.) | Sync lê `b.foto` e omite arquivo ausente. (`app/static/pwa.js:138-152`.) |
| `liveness_status/detail` | Sync sempre envia `indisponivel` / `PWA web sem liveness`. (`app/static/pwa.js:148-151`.) | APIs convertem em motivo/contexto `liveness_indisponivel`; não distingue câmera negada de outras causas. (`app/api/views.py:491-507`; `app/api/views_core.py:478-494`.) |
| `canal` | Derivado por `canal_da_batida(device_id)`; gravado nas APIs e no web. (`app/ponto/registro_batida.py:53-61`; `app/api/views_core.py:658-662`; `app/api/views.py:666-670`; `app/ponto/services/triagem_batida.py:700-705`.) | `selfie_ausente`/retro usam canal para prazo; modelo guarda o campo. (`app/ponto/registro_batida.py:75-91`; `app/ponto/models.py:55`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:73-77`.) |
| `foto_ausente` no ato | As duas APIs acrescentam motivo/contexto pela resposta de `selfie_ausente`. (`app/api/views.py:449-465`; `app/api/views_core.py:436-452`.) | API encaminha à emissão de disputa; persistência em `DisputaSupervisao.motivos/contexto`, geração de perguntas pelo serviço. (`app/api/views_core.py:763-774`; `app/chamados/models.py:1741-1747`; `app/chamados/services/disputa_emissao.py:634-685`.) |
| Foto efetivamente anexada | `anexar_foto`: escreve `Batida.foto` e, conforme modalidade, `FotoBatida`. (`app/ponto/registro_batida.py:296-311`.) | Retro lê **as duas fontes**; feed exibe foto ou ícone substituto. (`app/ponto/management/commands/marcar_foto_ausente_retro.py:66-77`; `app/templates/core/_d2_feed.html:6-7`.) |
| `foto_ausente` retroativo | Comando consulta juiz e em `--apply` emite disputa. (`app/ponto/management/commands/marcar_foto_ausente_retro.py:73-104`.) | Catálogo pergunta confirmação de presença; upload posterior lê motivo/última batida e apara ou fecha disputa. (`app/chamados/catalogo/perguntas.py:93-98`; `app/api/foto_recebida.py:57-80`.) |
| Permissão de câmera do onboarding | Front envia `{camera:true}`; backend grava `camera_concedida_em`. (`app/templates/core/onboarding.html:226-230`; `app/core/views.py:1085-1090`.) | `etapas_pendentes` e concluir onboarding leem esse campo de **Colaborador**, não de Batida. (`app/colaboradores/models.py:376`; `app/colaboradores/models.py:405-413`; `app/core/views.py:481-486`.) |

**Offline e online da triagem não gravam o mesmo sinal no ato.** O offline, ao sincronizar pela API, produz `foto_ausente` e liveness indisponível; o web registra sem foto, mas sua aprovação/justificativa considera horário/situação, sem consultar o juiz de selfie. A varredura posterior pode cobrar a ausência, sem recuperar a causa “câmera indisponível”. (`app/static/pwa.js:148-153`; `app/api/views_core.py:436-452`; `app/ponto/services/triagem_batida.py:685-713`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:73-104`.)

Não há campo específico `camera_indisponivel`/`sem_selfie` em `Batida`; os sinais citados vivem na ausência das fotos e nos motivos/contexto da disputa. (`app/ponto/models.py:8-112`; `app/chamados/models.py:1727-1747`.)

**5. Testes e selos existentes** [nao verificado]

Resultados atuais de execução: **[nao verificado]**. A tabela descreve o que o código dos testes verifica. [nao verificado]

| Teste/selo | Cobertura e capacidade de detectar regressão | [nao verificado]
|---|---|
| `test_api_bater_pwa` | Sem foto exige 200, Batida e `foto_ausente`; com foto exige ausência desse motivo. **Detectaria rejeição backend**, não bloqueio de clique. (`app/api/tests/test_api_bater_pwa.py:64-76`; `app/api/tests/test_api_bater_pwa.py:95-105`.) |
| `test_batida_offline` | Payload sem foto espera registro pela API e disputa; não exercita câmera/IndexedDB/clique. (`app/ponto/tests/test_batida_offline.py:49-76`; `app/ponto/tests/test_batida_offline.py:119-128`.) |
| `test_selfie_ausente_juiz` | Verifica canal/prazo e consulta ao juiz nas duas APIs. (`app/ponto/tests/test_selfie_ausente_juiz.py:27-47`; `app/ponto/tests/test_selfie_ausente_juiz.py:130-140`.) |
| `test_l030_forma_js` | Verifica barreiras de **GPS**; filtra condições por variáveis GPS. No apto, mede o trecho depois de `toBlob`. **Não detecta** o botão escondido no catch nem a guarda de foto da v2. (`app/ponto/tests/test_l030_forma_js.py:31-68`.) |
| `test_l030_templates_renderizados` | Aplica o mesmo oráculo a apto/v2 nas duas cascas; mantém a limitação anterior. (`app/ponto/tests/test_l030_templates_renderizados.py:14-37`.) |
| `check_a_lei_batida.py` | Procura assinaturas estreitas em apto/bater; foto+return na mesma linha. **Não enumera v2 nem procura botão escondido.** Checks de view apontam `app/apps/ponto/views.py`; arquivo ausente é pulado. (`app/scripts/check_a_lei_batida.py:13-35`.) |
| `node_check.sh` | Só sintaxe de JS estático próprio; `<script>` inline fica fora. **Não detecta essa regressão de comportamento.** (`bin/node_check.sh:11-20`; `bin/node_check.sh:33-37`.) |
| Smoke Chromium geral | Abre espelho nas duas cascas, verifica overlay/console; não registra batida nem exercita câmera. (`app/core/tests/test_smoke_chromium.py:151-189`.) |
| Teste do SW | Verifica fallback de rede/cache/Response, não selfie. (`app/core/tests/test_sw_offline_fallback.py:60-101`.) |
| Smoke de clique sem câmera nas duas cascas | Existência/cobertura específica: **[nao verificado]**. |

**6. Arquivos candidatos para conferir interseção**

Lista inferida dos pontos de decisão e cobertura; não representa alteração realizada. (`app/templates/ponto/triagem/apto.html:253-256`; `app/templates/ponto/bater_ponto_v2.html:285-290`; `app/scripts/check_a_lei_batida.py:13-30`.)

| Escopo | Arquivos candidatos e fundamento |
|---|---|
| Resposta compartilhada e migração dos fronts | `app/static/pwa.js`; `app/templates/ponto/triagem/apto.html`; `app/templates/ponto/bater_ponto_v2.html`; `app/templates/ponto/bater_ponto.html`. Já concentram fila e decisões locais. (`app/static/pwa.js:107`; `app/templates/ponto/triagem/apto.html:156-282`; `app/templates/ponto/bater_ponto_v2.html:285-346`; `app/templates/ponto/bater_ponto.html:343-610`.) |
| Proteção contra retorno do bloqueio | `app/scripts/check_a_lei_batida.py`; família `test_l030_forma_js`/`test_l030_templates_renderizados`; smoke de clique a acrescentar em arquivo ainda **[nao verificado]**. (`app/scripts/check_a_lei_batida.py:13-35`; `app/ponto/tests/test_l030_forma_js.py:31-68`; `app/ponto/tests/test_l030_templates_renderizados.py:14-37`.) |
| Se o escopo incluir uniformizar o sinal no ato | `app/ponto/services/triagem_batida.py`, `app/api/views.py`, `app/api/views_core.py`, com referência à cobrança existente em `app/ponto/registro_batida.py`. Essa diferença é separada do botão offline. (`app/ponto/services/triagem_batida.py:685-751`; `app/api/views.py:449-465`; `app/api/views_core.py:436-452`; `app/ponto/registro_batida.py:64-91`.) |
| Se houver mudança de carregamento | `app/templates/base.html` e `app/templates/base_app.html`; `pwa.js` já está carregado em ambas. (`app/templates/base.html:20`; `app/templates/base_app.html:20`.) |
| Onboarding, se incluído na cl

[truncado em 20000 de 20621 caracteres]

## 11/11 12:11 · sessao 4cfb93cca4fc · BO-ROTULO-ENCERRADO · verificador · origem=Code

**Bloqueia: o selo permite reintroduzir os dois estados com o mesmo texto visível.** A correção atual centraliza os rótulos, mas os testes deixam escapar opções com aspas simples e opções vindas de partials. ([test_bo_rotulo_encerrado.py:20](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/tests/test_bo_rotulo_encerrado.py:20), [test_bo_rotulo_encerrado.py:65](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/tests/test_bo_rotulo_encerrado.py:65))

1. **Censo dos controles.** Não encontrei seletor de destino remanescente com opções de estado manuais ou lendo `espera`. Os cinco encontrados usam a tag:

   | Seletor | Valores, na ordem atual |
   |---|---|
   | [detalhe.html:130](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/templates/chamados/detalhe.html:130) | `""`, `em_analise`, `resolvido`, `fechado`, `aberto` |
   | [detalhe_local.html:393](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/templates/chamados/detalhe_local.html:393) | `""`, `em_analise`, `resolvido`, `fechado`, `aberto` |
   | [fio_colaborador.html:265](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/templates/chamados/fio_colaborador.html:265) | `""`, `em_analise`, `resolvido`, `fechado` |
   | [modal_fio.html:665](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/templates/chamados/partials/modal_fio.html:665) | `""`, `em_analise`, `resolvido`, `fechado` |
   | [painel_chamado_turno.html:74](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/templates/chamados/partials/painel_chamado_turno.html:74) | `""`, `em_analise`, `resolvido`, `fechado` |

   Há controles manuais de **ações** que alcançam mudança de estado, sem oferecer opções de destino pelo mapa:

   | Controle manual | Onde / escritor |
   |---|---|
   | Aprovar/rejeitar benefício | [painel_solicitacao_beneficio.html:23](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/templates/colaboradores/painel_solicitacao_beneficio.html:23), também construído por JS em [fila_solicitacoes_beneficio.html:138](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/templates/colaboradores/fila_solicitacoes_beneficio.html:138); escolhe `resolvido`/`fechado` em [views.py:2068](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/colaboradores/views.py:2068). |
   | Aceitar/recusar escala e agenda | [detalhe_local.html:163](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/templates/chamados/detalhe_local.html:163), [detalhe_local.html:200](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/templates/chamados/detalhe_local.html:200); concluem em `resolvido` em [acoes_chamado.py:149](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/services/acoes_chamado.py:149) e [acoes_chamado.py:108](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/services/acoes_chamado.py:108). |
   | Aprovar exceção | [detalhe_local.html:256](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/templates/chamados/detalhe_local.html:256), [modal_fio.html:598](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/templates/chamados/partials/modal_fio.html:598); `resolvido` em [acoes_chamado.py:70](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/services/acoes_chamado.py:70). |
   | Autorizar geofence, com/sem advertência, ou recusar | [detalhe_local.html:285](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/templates/chamados/detalhe_local.html:285), [modal_fio.html:506](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/templates/chamados/partials/modal_fio.html:506); `resolvido` em [acoes_chamado.py:222](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/services/acoes_chamado.py:222) e [acoes_chamado.py:277](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/services/acoes_chamado.py:277). |
   | Abrir registro de ausência | [detalhe_local.html:233](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/templates/chamados/detalhe_local.html:233); chama `analisar()` em [views.py:1513](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/views.py:1513). |
   | Reabertura administrativa; resgatar | [modal_fio.html:390](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/templates/chamados/partials/modal_fio.html:390) oferece **motivos** e reabre em [acoes_disputa.py:269](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/services/acoes_disputa.py:269); [botão resgatar:85](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/templates/chamados/partials/_lista_chamados.html:85) pode reabrir pelo caso da tranca em [views_arquivar.py:64](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/views_arquivar.py:64). |

   O seletor manual fora de chamados, no histórico, é filtro: seu receptor aplica `qs.filter`, sem transição. ([historico_chamados.html:54](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/templates/relatorios/historico_chamados.html:54), [relatorios/views.py:920](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/relatorios/views.py:920))

2. **Quem escreve e quem lê; dois achados de fila.**

   - **`value`:** a tag escreve exatamente o código recebido, separado do texto e do `title`. Os argumentos vêm das chamadas literais da tabela acima. ([chamados_tags.py:76](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/templatetags/chamados_tags.py:76))
   - **Local e painel de turno:** chegam a `detalhe_chamado_local`, que delega à resposta livre; o serviço exige gestor e código pertencente a `STATUS_CHOICES`, depois chama `transicionar`. ([views.py:547](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/views.py:547), [acoes_chamado.py:287](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/services/acoes_chamado.py:287), [acoes_chamado.py:313](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/services/acoes_chamado.py:313))
   - **Fio e modal:** delegam ao mesmo `processar_post_fio`; ele lê o código, exige chamado/conteúdo e permissão contextual, verifica `STATUS_CHOICES` e chama `transicionar`. ([views.py:669](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/views.py:669), [views.py:953](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/views.py:953), [fio.py:502](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/services/fio.py:502), [fio.py:709](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/services/fio.py:709))
   - **Fila — detalhe GLPI sem efeito de status:** envia `novo_status` para `followup`, mas `adicionar_followup` só lê conteúdo e envia a mensagem ao GLPI; nenhum dos quatro destinos é aplicado nesse caminho. ([detalhe.html:122](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/templates/chamados/detalhe.html:122), [urls.py:12](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/urls.py:12), [views.py:292](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/views.py:292), [views.py:304](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/views.py:304))
   - **Fila — “Reabrir” pode ser recusado:** envia `aberto`; os receptores locais passam esse código ao modelo, mas `resolvido → aberto` é recusado para quem não é `admin_ti`. A matriz permite `em_analise → aberto`. ([verbos.py:136](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/services/verbos.py:136), [motor.py:99](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/catalogo/motor.py:99), [motor.py:153](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/catalogo/motor.py:153))

   `MOVER_PARA` tem definição única e é lido por `rotulo_de_movimento`; `ESTADO_TITLE` também alimenta `rotulo_de_linha` e `rotulo_para_pessoa`. A tag só monta HTML; seus leitores são os cinco templates e os testes. Não encontrei comparação do texto visível nos receptores: eles comparam códigos. ([verbos.py:100](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/services/verbos.py:100), [verbos.py:132](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/services/verbos.py:132), [verbos.py:146](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/services/verbos.py:146), [verbos.py:155](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/services/verbos.py:155), [verbos.py:166](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/services/verbos.py:166))

   A decisão e gravação continuam por `transicionar → decidir → _aplicar → _gravar_estado`, incluindo as guardas adicionais do modelo. ([models.py:123](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/models.py:123), [models.py:128](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/models.py:128), [models.py:199](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/models.py:199), [models.py:450](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/models.py:450))

3. **O diff fornecido preserva conjunto, ordem e `value`.** Os dois detalhes conservam quatro destinos; os demais, três, nas ordens da tabela. A tag escreve o próprio argumento como `value`. ([detalhe.html:132](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/templates/chamados/detalhe.html:132), [detalhe_local.html:395](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/templates/chamados/detalhe_local.html:395), [modal_fio.html:668](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/templates/chamados/partials/modal_fio.html:668), [chamados_tags.py:77](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/templatetags/chamados_tags.py:77))

   **Fila — essa preservação não está protegida pelo selo:** adicionar `opcao_mover "aberto"` ao modal, remover/reordenar tags ou trocar argumentos por outros válidos passa pelas verificações existentes. Elas não comparam as opções de cada seletor com seu conjunto anterior. ([test_bo_rotulo_encerrado.py:43](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/tests/test_bo_rotulo_encerrado.py:43), [test_bo_rotulo_encerrado.py:62](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/tests/test_bo_rotulo_encerrado.py:62))

4. **KeyError: nenhum caminho dinâmico encontrado na cópia.** O acesso indexado pode levantar, mas o único chamador produtivo Python é a tag; suas 18 chamadas nos cinco templates são literais cobertos pelo mapa. Ocorrência em produção: `[nao verificado]`. ([verbos.py:146](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/services/verbos.py:146), [chamados_tags.py:76](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/templatetags/chamados_tags.py:76), chamadas na tabela do item 1)

5. **Bloqueia — selo insuficiente para impedir a reincidência.** Executei o varredor extraído do teste, em memória, substituindo apenas as duas tags do detalhe por: [nao verificado]

   ```html
   <option value='resolvido'>Encerrado</option>
   <option value='fechado'>Encerrado</option>
   ```

   O resultado continuou **cinco arquivos, `ruins=[]`**: `_OPTION` só reconhece aspas duplas e basta restar outra `opcao_mover` no seletor. Os testes de mapa/tag isolada não observam essa alteração; o teste dos templates apenas compila. Execução integral da suíte após mutação: `[nao verificado]`. ([detalhe.html:133](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/templates/chamados/detalhe.html:133), [test_bo_rotulo_encerrado.py:20](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/tests/test_bo_rotulo_encerrado.py:20), [teste:49](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/tests/test_bo_rotulo_encerrado.py:49), [teste:69](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/tests/test_bo_rotulo_encerrado.py:69), [teste:73](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/tests/test_bo_rotulo_encerrado.py:73))

   Também escapam `name` com aspas simples, outro nome de controle e arquivos fora de `templates/chamados`. Um include com opções manuais passa mantendo uma tag no pai: o varredor não expande includes e o partial sem `<select>` fica invisível. **Mudar apenas a ordem dos atributos não escapa**: `title="..." value="resolvido"` foi detectado. ([teste:17](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/tests/test_bo_rotulo_encerrado.py:17), [teste:19](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/tests/test_bo_rotulo_encerrado.py:19), [teste:20](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/tests/test_bo_rotulo_encerrado.py:20), [teste:32](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/tests/test_bo_rotulo_encerrado.py:32))

6. **Classes anteriores:** está presente **selo sem mordida suficiente**, conforme item 5. Não encontrei segundo juiz de transição nem derivação paralela introduzidos: a autoridade registrada segue `motor.decidir`; o novo helper responde à apresentação das opções. Também não há sobrecarga nova demonstrada de código/texto/title, que permanecem separados; a desconexão “Reabrir”/`aberto` fica no achado de fila do item 2. ([core/juizes.py:274](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/core/juizes.py:274), [verbos.py:140](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/services/verbos.py:140), [chamados_tags.py:77](/home/ronald/codex_ws/2da7f77ae241c78797abd3c854749546151168d5/app/chamados/templatetags/chamados_tags.py:77))

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

_3825 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
